---
title: 如何使用 sqlrutils 创建存储过程
description: 使用 SQL Server 中的 sqlrutils R 包将 R 语言代码捆绑到一个可作为参数传递给存储过程的函数。
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.openlocfilehash: a224bed65cd7d3fd1b6dda4ed10d56f79ecc12ee
ms.sourcegitcommit: 9062c5e97c4e4af0bbe5be6637cc3872cd1b2320
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2019
ms.locfileid: "68470151"
---
# <a name="create-a-stored-pprocedure-using-sqlrutils"></a>使用 sqlrutils 创建存储的 pProcedure
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

本文介绍转换 R 代码以作为 T-sql 存储过程运行的步骤。 为了获得最佳的可能结果，可能需要对代码进行某种程度的修改，以确保所有输入可参数化。

## <a name="bkmk_rewrite"></a>步骤 1. 重写 R 脚本

为了获得最佳结果, 应重写 R 代码, 将其封装为单一函数。

函数使用的所有变量都应在函数内定义, 或应定义为输入参数。 请参阅本文中的[示例代码](#samples)。

此外, 因为 R 函数的输入参数将成为 SQL 存储过程的输入参数, 所以必须确保输入和输出符合以下类型要求:

### <a name="inputs"></a>输入

输入参数中最多可有一个数据帧。

数据帧内的对象以及函数的其他输入参数必须是以下 R 数据类型之一：
- POSIXct
- NUMERIC
- character
- integer
- 逻辑
- raw

如果输入类型不是上述类型之一，则需进行序列化并作为 *raw*传入函数。 在这种情况下，该函数还必须包括要反序列化输入的代码。

### <a name="outputs"></a>outputs

该函数可输出以下项之一：

- 包含受支持数据类型的数据帧。 数据帧中所有对象必须使用受支持数据类型之一。
- 包含最多一个数据帧的命名列表。 列表的所有成员应该使用受支持数据类型之一。
- 如果函数没有返回任何结果，则为 NULL

## <a name="step-2-generate-required-objects"></a>步骤 2. 生成必需的对象

在将 R 代码清理并可作为单个函数进行调用后, 你将使用**sqlrutils**包中的函数来准备可以传递到实际生成存储过程的构造函数的输入和输出。

**sqlrutils**提供定义输入数据架构和类型的函数, 并定义输出数据架构和类型。 它还包括可将 R 对象转换为所需输出类型的函数。 你可以根据代码使用的数据类型, 执行多个函数调用来创建所需的对象。

### <a name="inputs"></a>输入

如果函数使用输入, 则为每个输入调用以下函数:

- `setInputData`如果输入是数据帧
- `setInputParameter`对于所有其他输入类型

执行每个函数调用时, 将创建一个 R 对象, 稍后会将该对象作为参数传递`StoredProcedure`给, 以创建完整的存储过程。

### <a name="outputs"></a>outputs

**sqlrutils**提供了多个用于将 R 对象 (如列表) 转换为数据的函数。 SQL Server 所需的帧。
如果函数直接输出数据帧，并未先将其包装到列表中，则可跳过此步骤。
如果函数返回 NULL, 还可以跳过此步骤的转换。

转换列表或从列表中获取特定项时, 请从以下函数中进行选择:

- `setOutputData`如果要从列表中获取的变量是数据帧
- `setOutputParameter`对于列表中的所有其他成员

执行每个函数调用时, 将创建一个 R 对象, 稍后会将该对象作为参数传递`StoredProcedure`给, 以创建完整的存储过程。

## <a name="step-3-generate-the-stored-procedure"></a>步骤 3. 生成存储过程

当所有输入和输出参数准备就绪时, 请调用`StoredProcedure`构造函数。

**Usage**

`StoredProcedure (func, spName, ..., filePath = NULL ,dbName = NULL, connectionString = NULL, batchSeparator = "GO")`

为了说明这一点, 假设您要使用以下参数创建一个名为**sp_rsample**的存储过程:

- 使用现有函数**foosql**。 此函数基于 R 函数**foo**中的现有代码, 但你重写该函数符合[此部分](#bkmk_rewrite)中所述的要求, 并将更新的函数命名为**foosql**。
- 使用数据帧**queryinput**作为输入
- 使用 R 变量名称 ( **sqloutput** ) 生成输出数据帧
- 你需要将 t-sql 代码创建为`C:\Temp`文件夹中的文件, 以便以后可以使用 SQL Server Management Studio 运行该代码

```R
StoredProcedure (foosql, sp_rsample, queryinput, sqloutput, filePath = "C:\\Temp")
```

> [!NOTE]
> 由于要将文件写入文件系统, 因此可以省略定义数据库连接的参数。

函数的输出是一个 T-sql 存储过程, 可在 SQL Server 2016 (需要 R 服务) 或 SQL Server 2017 (需要机器学习服务 R) 的实例上执行。 

有关其他示例, 请通过从 R 环境调用`help(StoredProcedure)`来查看包帮助。

## <a name="step-4-register-and-run-the-stored-procedure"></a>步骤 4. 注册并运行存储过程

可以通过两种方式运行存储过程:

- 使用 T-sql, 从支持连接到 SQL Server 2016 或 SQL Server 2017 实例的任何客户端
- 从 R 环境

这两种方法都要求在要使用存储过程的数据库中注册存储过程。

### <a name="register-the-stored-procedure"></a>注册存储过程

您可以使用 R 来注册存储过程, 也可以在 T-sql 中运行 CREATE PROCEDURE 语句。

- 使用 T-sql。  如果你更喜欢 t-sql, 请打开 SQL Server Management Studio (或任何其他可运行 sql DDL 命令的客户端), 并使用`StoredProcedure`函数准备的代码执行 CREATE PROCEDURE 语句。
- 使用 R。尽管仍处于 R 环境中, 但可以使用**sqlrutils**中的`registerStoredProcedure`函数向数据库注册存储过程。

  例如, 你可以通过进行以下 R 调用, 在*sqlConnStr*中定义的实例和数据库中注册存储过程**sp_rsample** :

  ```R
  registerStoredProcedure(sp_rsample, sqlConnStr)
  ```


> [!IMPORTANT]
> 无论你使用的是 R 还是 SQL, 你都必须使用有权创建新数据库对象的帐户运行该语句。

### <a name="run-using-sql"></a>使用 SQL 运行

创建存储过程后, 使用支持 T-sql 的任何客户端打开到 SQL 数据库的连接, 并传递存储过程所需的任何参数的值。

### <a name="run-using-r"></a>使用 R 运行

如果要从 R 代码执行存储过程, 而不是 SQL Server, 则需要进行一些额外的准备工作。 例如, 如果存储过程需要输入值, 则必须先设置这些输入参数, 然后才能执行函数, 然后将这些对象传递到 R 代码中的存储过程。

调用已准备的 SQL 存储过程的整个过程如下所示:

1. 调用 `getInputParameters` 以获取输入参数对象的列表。
2. 定义 `$query` 或为每个输入参数设置 `$value` 。
3. 使用 `executeStoredProcedure` 从 R 开发环境执行该存储过程，传递设置的输入参数对象列表。

## <a name = "samples"></a>实例

此示例显示了从 SQL Server 数据库获取数据、对数据执行一些转换并将其保存到其他数据库的 R 脚本之前和之后的版本。

此简单示例仅用于演示如何重新排列 R 代码, 以便更轻松地转换为存储过程。

### <a name="before-code-preparation"></a>在代码准备之前


```R
sqlConnFrom <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=AirlineSrc;Trusted_Connection=Yes;"
  
sqlConnTo <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=AirlineTest;Trusted_Connection=Yes;"
  
sqlQueryAirline <- "SELECT TOP 10000 ArrDelay, CRSDepTime, DayOfWeek FROM [AirlineDemoSmall]"
  
dsSqlFrom <- RxSqlServerData(sqlQuery = sqlQueryAirline, connectionString = sqlConnFrom)
  
dsSqlTo <- RxSqlServerData(table = "cleanData", connectionString = sqlConnTo)
  
xFunc <- function(data) {
    data$CRSDepHour <- as.integer(trunc(data$CRSDepTime))
    return(data)
    }
  
xVars <- c("CRSDepTime")
  
sqlCompute <- RxInSqlServer(numTasks = 4, connectionString = sqlConnTo)
  
rxOpen(dsSqlFrom)
rxOpen(dsSqlTo)
  
if (rxSqlServerTableExists("cleanData", connectionString = sqlConnTo))   {
    rxSqlServerDropTable("cleanData")}
  
rxDataStep(inData = dsSqlFrom, 
     outFile = dsSqlTo,
     transformFunc = xFunc,
     transformVars = xVars,
     overwrite = TRUE)
```

> [!NOTE]
> 
> 当使用 ODBC 连接而不是调用*RxSqlServerData*函数时, 必须先使用*rxOpen*打开连接, 然后才能对数据库执行操作。


### <a name="after-code-preparation"></a>代码准备后

在更新后的版本中, 第一行定义函数名称。 原始 R 解决方案中的所有其他代码将成为该函数的一部分。

```R
myetl1function <- function() { 
   sqlConnFrom <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=Airline01;Trusted_Connection=Yes;"
   sqlConnTo <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer02;Database=Airline02;Trusted_Connection=Yes;"
    
   sqlQueryAirline <- "SELECT TOP 10000 ArrDelay, CRSDepTime, DayOfWeek FROM [AirlineDemoSmall]"

   dsSqlFrom <- RxSqlServerData(sqlQuery = sqlQueryAirline, connectionString = sqlConnFrom)
  
   dsSqlTo <- RxSqlServerData(table = "cleanData", connectionString = sqlConnTo)
  
   xFunc <- function(data) {
     data$CRSDepHour <- as.integer(trunc(data$CRSDepTime))
     return(data)}
  
   xVars <- c("CRSDepTime")
  
   sqlCompute <- RxInSqlServer(numTasks = 4, connectionString = sqlConnTo)
  
   if (rxSqlServerTableExists("cleanData", connectionString = sqlConnTo)) {rxSqlServerDropTable("cleanData")}
  
   rxDataStep(inData = dsSqlFrom, 
        outFile = dsSqlTo,
        transformFunc = xFunc,
        transformVars = xVars,
        overwrite = TRUE)
   return(NULL)
}
```

> [!NOTE]
> 
> 尽管不需要显式打开 ODBC 连接作为代码的一部分，但 ODBC 连接仍需使用 **sqlrutils**。

## <a name="see-also"></a>请参阅

[sqlrutils (SQL)](ref-r-sqlrutils.md)


