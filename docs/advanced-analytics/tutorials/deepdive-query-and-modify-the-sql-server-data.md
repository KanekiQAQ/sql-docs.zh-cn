---
title: 使用 RevoScaleR 查询和修改 SQL Server 数据
description: 本教程演示如何使用 R 语言在 SQL Server 上查询和修改数据。
ms.prod: sql
ms.technology: machine-learning
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.openlocfilehash: 3fb28d287c7b92e9b399aa7bc0bb606c618eed47
ms.sourcegitcommit: 9062c5e97c4e4af0bbe5be6637cc3872cd1b2320
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2019
ms.locfileid: "68469801"
---
# <a name="query-and-modify-the-sql-server-data-sql-server-and-revoscaler-tutorial"></a>查询和修改 SQL Server 数据 (SQL Server 和 RevoScaleR 教程)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

本课程是有关如何在 SQL Server 中使用[RevoScaleR 函数](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler)的[RevoScaleR 教程](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)的一部分。

在上一课中, 您将数据加载[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]到中。 在此步骤中, 可以使用**RevoScaleR**浏览和修改数据:

> [!div class="checklist"]
> * 返回有关变量的基本信息
> * 从原始数据创建分类数据

分类数据或*系数变量*对于探索数据可视化效果非常有用。 可以将它们用作直方图的输入, 以了解可变数据的外观。

## <a name="query-for-columns-and-types"></a>查询列和类型

使用 R IDE 或 Rgui.exe 运行 R 脚本。 

首先，获取列及其数据类型的列表。 您可以使用函数[rxGetVarInfo](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxgetvarinfoxdf)并指定要分析的数据源。 你还可以使用[rxGetVarNames](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxgetvarnames), 具体取决于你的**RevoScaleR**版本。 
  
```R
rxGetVarInfo(data = sqlFraudDS)
```

**结果**

```R
Var 1: custID, Type: integer
Var 2: gender, Type: integer
Var 3: state, Type: integer
Var 4: cardholder, Type: integer
Var 5: balance, Type: integer
Var 6: numTrans, Type: integer
Var 7: numIntlTrans, Type: integer
Var 8: creditLine, Type: integer
Var 9: fraudRisk, Type: integer
```

## <a name="create-categorical-data"></a>创建分类数据

所有变量都作为整数存储, 但某些变量表示分类数据 (在 R 中称为*因子变量*)。例如, 列*状态*包含用作50状态以及哥伦比亚特区的标识符的数字。 为了更加轻松地理解数据，可将数字替换为州名的缩写列表。

在此步骤中, 你将创建一个包含缩写的字符串向量, 然后将这些分类值映射到原始整数标识符。 然后, 在*colInfo*参数中使用新变量, 以指定将此列作为因子处理。 分析数据或移动数据时, 将使用缩写并将该列作为一个因素进行处理。

将列作为用作因子前将其映射到缩写也可真正提高性能。 有关详细信息, 请参阅[R 和数据优化](../r/r-and-data-optimization-r-services.md)。

1. 首先创建 R 变量*stateAbb*, 并定义要添加到其中的字符串向量, 如下所示。
  
    ```R
    stateAbb <- c("AK", "AL", "AR", "AZ", "CA", "CO", "CT", "DC",
        "DE", "FL", "GA", "HI","IA", "ID", "IL", "IN", "KS", "KY", "LA",
        "MA", "MD", "ME", "MI", "MN", "MO", "MS", "MT", "NB", "NC", "ND",
        "NH", "NJ", "NM", "NV", "NY", "OH", "OK", "OR", "PA", "RI","SC",
        "SD", "TN", "TX", "UT", "VA", "VT", "WA", "WI", "WV", "WY")
    ```

2. 接下来，创建名为 ccColInfo 的列信息对象，该对象指定现有整数值到分类级别（州名的缩写）的映射。
  
    此语句还为 gender 和 cardholder 创建因子变量。
  
    ```R
    ccColInfo <- list(
    gender = list(
              type = "factor",
              levels = c("1", "2"),
              newLevels = c("Male", "Female")
              ),
    cardholder = list(
                  type = "factor",
                  levels = c("1", "2"),
                  newLevels = c("Principal", "Secondary")
                   ),
    state = list(
             type = "factor",
             levels = as.character(1:51),
             newLevels = stateAbb
             ),
    balance = list(type = "numeric")
    )
    ```
  
3. 若要创建[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用更新数据的数据源, 请像以前一样调用**RxSqlServerData**函数, 但添加*colInfo*参数。
  
    ```R
    sqlFraudDS <- RxSqlServerData(connectionString = sqlConnString,
    table = sqlFraudTable, colInfo = ccColInfo,
    rowsPerRead = sqlRowsPerRead)
    ```
  
    - 对于 table 参数，可传入变量 sqlFraudTable，其中包含之前创建的数据源。
    - 对于 colInfo 参数，可传入 ccColInfo 变量，其中包含列数据类型和因子级别。

4.  现在可使用函数 rxGetVarInfo 查看新数据源中的变量。
  
    ```R
    rxGetVarInfo(data = sqlFraudDS)
    ```

    **结果**
    
    ```R
    Var 1: custID, Type: integer
    Var 2: gender  2 factor levels: Male Female
    Var 3: state   51 factor levels: AK AL AR AZ CA ... VT WA WI WV WY
    Var 4: cardholder  2 factor levels: Principal Secondary
    Var 5: balance, Type: integer
    Var 6: numTrans, Type: integer
    Var 7: numIntlTrans, Type: integer
    Var 8: creditLine, Type: integer
    Var 9: fraudRisk, Type: integer
    ```

现在，指定的三个变量（*gender*、 *state*和 *cardholder*）将被视为因子。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [定义并使用计算上下文](../../advanced-analytics/tutorials/deepdive-define-and-use-compute-contexts.md)