---
title: SqlXmlAdapter 对象 （SQLXML 托管类） |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- void Update(DataSet ds) method
- SqlXmlAdapter object
- void Fill(DataSet ds) method
- SQLXML Managed Classes, SqlXmlAdapter object
- Managed Classes [SQLXML], SqlXmlAdapter object
ms.assetid: 0a16eddf-fc26-4d92-82d4-359b5fb905d5
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 761a3be11c75844d7e7e014339cfa03bc2196ad3
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68119632"
---
# <a name="sqlxml-managed-classes---sqlxmladapter-object"></a>SQLXML 托管类 - SqlXmlAdapter 对象
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  此对象提供用于促进与 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework 中的数据集进行交互的方法。 有关工作示例，请参阅[在.NET 环境中访问 SQLXML 功能](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md)。  
  
 SqlXmlAdapter 对象支持以下方法：  
  
 void Fill (DataSet ds)  
 用从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 检索的 XML 数据填充 .NET Framework 中的数据集。  
  
 void Update (DataSet ds)  
 用数据集中的数据更新 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中的记录。  
  
 SqlXmlAdapter 对象支持以下构造函数：  
  
```  
public SqlXmlAdapter(SqlXmlCommand  cmd)   
  
public SqlXmlAdapter(  
                     string commandText,   
                     SqlXmlCommandType cmdType,   
                     string connectionString  
                      )   
  
public SqlXmlAdapter(  
                     Stream commandStream,   
                     SqlXmlCommandType cmdType,   
                     string connectionString  
                     )   
```  
  
## <a name="see-also"></a>请参阅  
 [SqlXmlCommand 对象&#40;SQLXML 托管类&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-managed-classes-sqlxmlcommand-object.md)   
 [SqlXmlParameter 对象&#40;SQLXML 托管类&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-managed-classes-sqlxmlparameter-object.md)  
  
  
