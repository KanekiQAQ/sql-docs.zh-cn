---
title: 显示 R 函数的快速入门-SQL Server 机器学习
description: 本快速入门介绍如何为高级统计计算编写 R 函数。
ms.prod: sql
ms.technology: machine-learning
ms.date: 01/04/2019
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.openlocfilehash: f43709f563d1dc5838cdd6636bcac4dc5664a6da
ms.sourcegitcommit: 9062c5e97c4e4af0bbe5be6637cc3872cd1b2320
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2019
ms.locfileid: "68469273"
---
# <a name="quickstart-using-r-functions"></a>快速入门：使用 R 函数
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

如果你已完成前面的快速入门, 就会熟悉基本操作并为更复杂的内容 (如统计函数) 做好准备。 在 T-sql 中实现复杂的高级统计函数可在 R 中完成, 只需要一行代码。

在本快速入门中, 你将在 SQL Server 存储过程中嵌入 R 数学和实用函数。

## <a name="prerequisites"></a>先决条件

以前的快速入门,[验证 SQL Server 中是否存在 r](quickstart-r-verify.md), 提供了设置本快速入门所需的 R 环境的信息和链接。

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>创建一个存储过程来生成随机数字

为简单起见, 让我们使用 r `stats`包, 默认情况下, 当你在 SQL Server 中安装 R 功能支持时, 将默认安装和加载此包。 此包中包含数百个用于执行常用统计任务的函数，其中，`rnorm` 函数在给定了标准差和平均数的情况下使用正态分布生成指定数量的随机数字。

例如，以下 R 代码在给定了标准差 3 的情况下返回平均数为 50 的 100 个数字。

```R
as.data.frame(rnorm(100, mean = 50, sd = 3));
```

要从 T-SQL 调用此行 R 代码，请运行 sp_execute_external_script 并在 R 脚本参数中添加此 R 函数，如下所示：

```sql
EXEC sp_execute_external_script
      @language = N'R'
    , @script = N'
         OutputDataSet <- as.data.frame(rnorm(100, mean = 50, sd =3));'
    , @input_data_1 = N'   ;'
      WITH RESULT SETS (([Density] float NOT NULL));
```

如果你希望更轻松地生成不同的一组随机数字，那该怎么办？

与 SQL Server 结合使用时, 这很简单: 定义从用户获取参数的存储过程。 然后，将那些参数作为变量传递给 R 脚本。

```sql
CREATE PROCEDURE MyRNorm (@param1 int, @param2 int, @param3 int)
AS
    EXEC sp_execute_external_script
      @language = N'R'
    , @script = N'
         OutputDataSet <- as.data.frame(rnorm(mynumbers, mymean, mysd));'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
    WITH RESULT SETS (([Density] float NOT NULL));
```

+ 第一行定义了在执行该存储过程时必需的每个 SQL 输入参数。

+ 以 `@params` 开头的行定义了 R 代码使用的所有变量，以及对应的 SQL 数据类型。

+ 紧跟在后面的行将 SQL 参数名称映射到对应的 R 变量名称。

现在，你已将 R 函数包装在了一个存储过程中，你可以轻松调用该函数并传入不同的值，如下所示：

```sql
EXEC MyRNorm @param1 = 100,@param2 = 50, @param3 = 3
```

## <a name="use-r-utility-functions-for-troubleshooting"></a>使用 R 实用工具函数进行故障排除

默认情况下, R 的安装包括`utils`包, 其中提供了用于调查当前 R 环境的各种实用工具函数。 如果你发现你的 R 代码在 SQL Server 中执行时和在环境外执行时执行方式存在差异，则上述函数会比较有用。

例如，你可以使用 R `memory.limit()` 函数获取当前 R 环境的内存。 因为 `utils` 包默认情况下已安装但未加载，因此你必须首先使用 `library()` 函数加载该包。

```sql
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
        library(utils);
        mymemory <- memory.limit();
        OutputDataSet <- as.data.frame(mymemory);'
    , @input_data_1 = N' ;'
WITH RESULT SETS (([Col1] int not null));
```

许多用户喜欢使用 r 中的系统计时函数 (如`system.time`和`proc.time`) 来捕获 r 进程使用的时间, 并分析性能问题。

有关示例, 请参阅本教程:[创建数据功能](../tutorials/walkthrough-create-data-features.md)。 在本演练中, R 计时函数嵌入在解决方案中, 用于比较两种方法的性能, 以便从数据创建特征:R 函数与T-SQL 函数。

## <a name="next-steps"></a>后续步骤

接下来，你将在 SQL Server 中使用 R 创建一个预测模型。

> [!div class="nextstepaction"]
> [起步创建预测模型](quickstart-r-create-predictive-model.md)
