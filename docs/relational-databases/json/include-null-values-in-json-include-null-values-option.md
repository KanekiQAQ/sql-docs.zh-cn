---
title: 在 JSON 中添加 NULL 值 - INCLUDE_NULL_VALUES 选项 | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.reviewer: genemi
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- INCLUDE_NULL_VALUES (FOR JSON)
ms.assetid: 06873768-3778-4ed8-a1db-61758726bda0
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9b6abe1bbad1f1c6e5c1f2e200c58e4ae865c1e3
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67909306"
---
# <a name="include-null-values-in-json---includenullvalues-option"></a>在 JSON 中添加 NULL 值 - INCLUDE_NULL_VALUES 选项
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  若要在 **FOR JSON** 子句的 JSON 输出中添加 NULL 值，请指定 **INCLUDE_NULL_VALUES** 选项。  
  
 如果你没有指定 **INCLUDE_NULL_VALUES** 选项，则 JSON 输出不会包括查询结果中 NULL 值所对应的属性。  
  
## <a name="examples"></a>示例  
 以下示例介绍了在指定和未指定 **INCLUDE_NULL_VALUES** 选项的情况下 **FOR JSON** 子句的输出。  
  
|未指定 **INCLUDE_NULL_VALUES** 选项|已指定 **INCLUDE_NULL_VALUES** 选项|  
|--------------------------------------------------|-----------------------------------------------|  
|`{    "name": "John",    "surname": "Doe" }`|`{    "name": "John",    "surname": "Doe",    "age": null,    "phone": null }`|  
  
 下面又通过一个示例介绍了在指定 **INCLUDE_NULL_VALUES** 选项的情况下 **FOR JSON** 子句的输出。  
  
 **“数据集属性”**  
  
```sql  
SELECT name, surname  
FROM emp  
FOR JSON AUTO, INCLUDE_NULL_VALUES    
```  
  
 **结果**  
  
```json  
[{
    "name": "John",
    "surname": null
}, {
    "name": "Jane",
    "surname": "Doe"
}] 
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>详细了解 SQL Server 和 Azure SQL 数据库中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 视频

有关 SQL Server 和 Azure SQL 数据库中内置 JSON 支持的视频介绍，请观看以下视频：

-   [SQL Server 2016 和 JSON 支持](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [在 SQL Server 2016 和 Azure SQL 数据库中使用 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON 充当 NoSQL 和关系环境之间的桥梁](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)

## <a name="see-also"></a>另请参阅  
 [FOR 子句 (Transact-SQL)](../../t-sql/queries/select-for-clause-transact-sql.md)  
  
  
