---
title: bcp_collen | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_collen
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_collen function
ms.assetid: faaf1f7a-81f2-4852-a178-56602c33673a
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e3b1a42cf9ec888e3cf5bbb265eb0f42c5ecc0c8
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67895714"
---
# <a name="bcpcollen"></a>bcp_collen
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  为目标为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的当前大容量复制设置程序变量中的数据长度。  
  
## <a name="syntax"></a>语法  
  
```  
  
RETCODE bcp_collen (  
        HDBC hdbc,  
        DBINT cbData,  
        INT idxServerCol);  
```  
  
## <a name="arguments"></a>参数  
 *hdbc*  
 是大容量复制启用 ODBC 连接句柄。  
  
 *cbData*  
 程序变量中数据的长度，不包括任何长度指示器或终止符的长度。 设置*cbData*为 SQL_NULL_DATA 指示复制到服务器的所有行都包含列的 NULL 值。 设置为 SQL_VARLEN_DATA 则指示使用字符串终止符或其他方法确定复制的数据的长度。 如果同时提供长度指示器和终止符，系统将使用二者中复制数据量较少的一个。  
  
 *idxServerCol*  
 数据复制到的表中的列的序号位置。 第一列为 1。 报告列的序号位置[SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md)。  
  
## <a name="returns"></a>返回  
 SUCCEED 或 FAIL。  
  
## <a name="remarks"></a>备注  
 **Bcp_collen**函数，可向复制数据时更改特定列的程序变量中的数据长度[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]与[bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md)。  
  
 最初，确定数据长度时[bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md)调用。 如果调用之间更改的数据长度**bcp_sendrow**且没有长度前缀或终止符正在使用时，可以调用**bcp_collen**重置该长度。 下次调用**bcp_sendrow**使用通过调用设置的长度**bcp_collen**。  
  
 必须调用**bcp_collen**一次针对你想要修改其数据长度的表中每个列。  
  
## <a name="see-also"></a>请参阅  
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
