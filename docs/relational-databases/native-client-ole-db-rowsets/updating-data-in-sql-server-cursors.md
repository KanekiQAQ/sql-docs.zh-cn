---
title: 更新 SQL Server 游标中的数据 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- updating data [SQL Server]
- isolation levels [SQL Server]
- delayed update mode [OLE DB]
- immediate update mode [OLE DB]
- cursors [OLE DB]
- data updates [SQL Server], OLE DB
ms.assetid: 732dafee-f2d5-4aef-aad7-3a8bf3b1e876
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 883eda7f0e1ed233f4c221c2afdb2236d3be2ca1
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68103479"
---
# <a name="updating-data-in-sql-server-cursors"></a>更新 SQL Server 游标中的数据
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  正在提取和更新数据时[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]游标， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口使用者应用程序绑定通过相同的注意事项和约束应用于任何其他客户端应用程序。  
  
 只有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 游标中的行才参与并发数据访问控制。 当使用者请求可修改的行集时，并发控制是通过 DBPROP_LOCKMODE 控制的。 若要修改并发访问控制的级别，使用者应在打开该行集之前设置 DBPROP_LOCKMODE 属性。  
  
 如果客户端应用程序设计使事务长时间保持打开状态，事务隔离级别在定位行时可能造成严重滞后。 默认情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口使用 DBPROPVAL_TI_READCOMMITTED 指定的已提交读隔离级别。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口支持脏读的隔离时行集并发为只读。 因此，使用者可以在可修改行集中请求更高级别的隔离，但是不能成功请求任何更低级别。  
  
## <a name="immediate-and-delayed-update-modes"></a>立即更新模式和延迟更新模式  
 在立即更新模式中，对 IRowsetChange::SetData 的每次调用均导致与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之间发生一次往返  。 如果使用者对一个行进行了多次更改，通过一个 SetData 调用提交所有更改将更有效  。  
  
 在延迟更新模式中，针对 IRowsetUpdate::Update 的 cRows 和 rghRows 参数中指示的每个行执行一次与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之间的往返    。  
  
 无论哪种模式，往返表示当未针对行集打开事务对象时的不同事务。  
  
 使用时**irowsetupdate:: Update**，则[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 访问接口尝试处理指示的每一行。 对于任何行不会停止因无效数据、 长度或状态的值而引起的错误[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 访问接口处理。 可能修改参与更新的所有其他行，也可能不修改这些行。 使用者必须检查返回*prgRowStatus*数组以确定失败的任何特定行时[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 访问接口返回 DB_S_ERRORSOCCURRED。  
  
 使用者不应假定行以任意特定顺序处理。 如果使用者要求按顺序处理基于多个行的数据修改，使用者应在应用程序逻辑中建立该顺序，并打开一个事务以包含该过程。  
  
## <a name="see-also"></a>请参阅  
 [更新行集中的数据](../../relational-databases/native-client-ole-db-rowsets/updating-data-in-rowsets.md)  
  
  
