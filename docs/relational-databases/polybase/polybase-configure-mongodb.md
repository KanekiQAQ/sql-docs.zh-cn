---
title: 配置 PolyBase 以访问 MongoDB 中的外部数据 | Microsoft Docs
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: Abiola
ms.author: aboke
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15 || =sqlallproducts-allversions'
ms.openlocfilehash: ffce4eeb7cffab8b2a8a25c6972fdc01aaf72345
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68062321"
---
# <a name="configure-polybase-to-access-external-data-in-mongodb"></a>配置 PolyBase 以访问 MongoDB 中的外部数据

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

本文介绍如何使用 SQL Server 实例上的 PolyBase 来查询 MongoDB 中的外部数据。

## <a name="prerequisites"></a>必备条件

如果尚未安装 PolyBase，请参阅 [PolyBase 安装](polybase-installation.md)。

在创建数据库范围凭据之前，必须先创建[主密钥](../../t-sql/statements/create-master-key-transact-sql.md)。 
    

## <a name="configure-a-mongodb-external-data-source"></a>配置 MongoDB 外部数据源

若要查询 MongoDB 数据源中的数据，必须创建外部表以引用外部数据。 本节提供用于创建这些外部表的示例代码。

此部分中使用了以下 Transact-SQL 命令：

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md) 
- [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)

1. 创建数据库范围凭据以访问 MongoDB 数据源。

    ```sql
    /*  specify credentials to external data source
    *  IDENTITY: user name for external source. 
    *  SECRET: password for external source.
    */
    CREATE DATABASE SCOPED CREDENTIAL credential_name WITH IDENTITY = 'username', Secret = 'password';
    ```
1. 使用 [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) 创建外部数据源。

    ```sql
    /*  LOCATION: Location string should be of format '<type>://<server>[:<port>]'.
    *  PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    *CONNECTION_OPTIONS: Specify driver location
    *  CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (LOCATION = 'mongodb://<server>[:<port>]',
    -- PUSHDOWN = ON | OFF,
    CREDENTIAL = credential_name);
    ```

1. **可选：** 在外部表上创建统计信息。

    为了获得最佳查询性能，我们建议在外部表列上创建统计信息，尤其是用于联接、筛选和聚合的统计信息。

    ```sql
    CREATE STATISTICS statistics_name ON customer (C_CUSTKEY) WITH FULLSCAN; 
    ```

>[!IMPORTANT] 
>创建外部数据源后，可以使用 [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) 命令在该数据源上创建可查询的表。 

## <a name="flattening"></a>平展
 为 MongoDB 文档集合中的嵌套和重复数据启用平展。 要求用户启用 `create an external table` 并通过可能包含嵌套和/或重复数据的 MongoDB 文档集合显式指定关系架构。 我们在将来的里程碑中将支持通过 mongo 文档集合的自动架构检测。
JSON 嵌套/重复数据类型将按如下所示平展

* 对象：大括号括起来的无序键/值集合（嵌套）

   - 我们将为每个对象键创建表列

     * 列名称：objectname_keyname

* 数组：有序值，以逗号分隔，用方括号括起来（重复）

   - 我们将为每个数组项添加新表行

   - 我们将按每个数组创建一列，用于存储数组项索引

     * 列名称：arrayname_index

     * 数据类型：bigint

此技术存在几个潜在问题，其中包括两个问题：

* 一个空的重复字段将有效地屏蔽包含在相同记录的平面字段中的数据

* 存在多个重复字段可能导致生成的行数呈爆炸式增长

例如，我们评估以非关系 JSON 格式存储的 MongoDB 示例数据集餐馆集合。 每家餐馆都有一个嵌套的地址字段和按不同日期分配的一组等级。 下图显示了包含嵌套地址和嵌套重复等级的典型餐馆。

![MongoDB 平展](../../relational-databases/polybase/media/mongo-flattening.png "MongoDB 餐馆平展")

对象地址将按如下所示平展：

* 嵌套字段 restaurant.address.building 将变为 restaurant.address_building
* 嵌套字段 restaurant.address.coord 将变为 restaurant.address_coord
* 嵌套字段 restaurant.address.street 将变为 restaurant.address_street
* 嵌套字段 restaurant.address.zipcode 将变为 restaurant.address_zipcode

数组等级将按如下所示平展：

| grades_date | grades_grade  | games_score | 
| ------------- | ------------------------- | -------------- |
|1393804800000 |仅当辅助副本配置为使用手动故障转移模式，并且至少一个辅助副本当前与主要副本同步时， |2|
|1378857600000|仅当辅助副本配置为使用手动故障转移模式，并且至少一个辅助副本当前与主要副本同步时， |6|
|135898560000 |仅当辅助副本配置为使用手动故障转移模式，并且至少一个辅助副本当前与主要副本同步时， |10|
|1322006400000|仅当辅助副本配置为使用手动故障转移模式，并且至少一个辅助副本当前与主要副本同步时， |9|
|1299715200000 |B |14|

## <a name="cosmos-db-connection"></a>Cosmos DB 连接

使用 Cosmos DB mongo api 和 Mongo DB PolyBase 连接器，可创建 Cosmos DB 实例的外部表  。 可按照以上列出的相同步骤完成此操作。 确保数据库范围凭据、服务器地址、端口和位置字符串反映 Cosmos DB 服务器的相应内容。 

## <a name="next-steps"></a>后续步骤

若要了解有关 PolyBase 的详细信息，请参阅 [SQL Server PolyBase 的概述](polybase-guide.md)。
