---
title: 使用结果集 |Microsoft Docs
ms.custom: ''
ms.date: 07/31/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 4fc4b1c6-3075-4ad7-9244-865d9ede7ae6
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 9d33dbebaad162feb77a4cbea8de33993fc79f14
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67956956"
---
# <a name="working-with-result-sets"></a>使用结果集

[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中包含的数据时，一种数据处理方法是使用结果集。 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 支持通过 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象使用结果集。 通过使用 SQLServerResultSet 对象，可以检索 SQL 语句或存储过程返回的数据，并根据需要更新数据，然后将该数据保存回数据库中。  
  
此外，SQLServerResultSet 对象还提供了多种方法，可用于在其数据行中进行导航、获取或设置其中包含的数据，以及建立针对基础数据库中更改的各种敏感度级别。  
  
> [!NOTE]  
> 有关管理结果集的详细信息 (包括其更改敏感度), 请参阅[通过 JDBC 驱动程序管理结果集](../../../connect/jdbc/managing-result-sets-with-the-jdbc-driver.md)。  
  
此部分的主题说明了使用结果集来处理 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中所包含数据的各种方法。  
  
## <a name="in-this-section"></a>本节内容  
  
| 主题                                                                                           | 描述                                                                                                                                                                                             |
| ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [结果集数据检索示例](../../../connect/jdbc/code-samples/retrieving-result-set-data-sample.md) | 说明如何使用结果集从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中检索数据，并将其显示出来。                                                         |
| [结果集数据修改示例](../../../connect/jdbc/code-samples/modifying-result-set-data-sample.md)   | 说明如何使用结果集在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中插入、检索和修改数据。                                                      |
| [结果集数据缓存示例](../../../connect/jdbc/code-samples/caching-result-set-data-sample.md)       | 说明如何使用结果集从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中检索大量数据，以及如何控制这些数据在客户端的缓存方式。 |
  
## <a name="see-also"></a>另请参阅  

[示例 JDBC 驱动程序应用程序](../../../connect/jdbc/code-samples/sample-jdbc-driver-applications.md)  
  
