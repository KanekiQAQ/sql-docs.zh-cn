---
title: APP_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- APP_NAME_TSQL
- APP_NAME
dev_langs:
- TSQL
helpviewer_keywords:
- name checking for current session [SQL Server]
- sessions [SQL Server], application names
- applications [SQL Server], names
- current session application names
- APP_NAME function
ms.assetid: e491e192-9b30-4243-bc19-33c133fe08a8
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: e9ad22c9490c04906dafcccb74d9dd16f9496852
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68040368"
---
# <a name="appname-transact-sql"></a>APP_NAME (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

此函数返回当前会话的应用程序名称（如果应用程序设置了该名称值）。
  
> [!IMPORTANT]  
>  客户端提供了应用程序名称，且 `APP_NAME` 未通过任何方式验证应用程序名称值。 请不要使用 `APP_NAME` 作为安全检查的一部分。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```sql
  
APP_NAME  ( )  
```  
  
## <a name="return-types"></a>返回类型  
**nvarchar(128)**
  
## <a name="remarks"></a>Remarks  
使用 `APP_NAME` 可区分不同的应用程序，这可作为对这些应用程序执行不同操作的一种方式。 例如，`APP_NAME` 可以区分不同的应用程序，允许每个应用程序使用不同的日期格式。 它还可允许向特定应用程序返回信息性消息。
  
要在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中设置应用程序名称，请单击“连接到数据库引擎”对话框中的“选项”。 在“其他连接参数”选项卡上，提供 app 属性，格式为 `;app='application_name'`
  
## <a name="example"></a>示例  
此示例检查启动此进程的客户端应用程序是否为 `SQL Server Management Studio` 会话。 然后，它提供 US 或 ANSI 格式的日期值。
  
```sql
USE AdventureWorks2012;  
GO  
IF APP_NAME() = 'Microsoft SQL Server Management Studio - Query'  
PRINT 'This process was started by ' + APP_NAME() + '. The date is ' + CONVERT ( varchar(100) , GETDATE(), 101) + '.';  
ELSE   
PRINT 'This process was started by ' + APP_NAME() + '. The date is ' + CONVERT ( varchar(100) , GETDATE(), 102) + '.';  
GO  
```  
  
## <a name="see-also"></a>另请参阅
[系统函数 (Transact-SQL)](../../relational-databases/system-functions/system-functions-for-transact-sql.md)  
[函数](../../t-sql/functions/functions.md)
  
  
