---
title: bcp_colptr |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_colptr
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_colptr function
ms.assetid: 02ece13e-1da3-4f9d-b860-3177e43d2471
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 09dc87f27f1e6cc9dc062addbe97945b47dc7802
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67895708"
---
# <a name="bcpcolptr"></a>bcp_colptr
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  将当前副本的程序变量数据地址设置到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中。  
  
## <a name="syntax"></a>语法  
  
```  
  
RETCODE bcp_colptr (  
        HDBC hdbc,  
        LPCBYTE pData,  
        INT idxServerCol);  
```  
  
## <a name="arguments"></a>参数  
 *hdbc*  
 是大容量复制启用 ODBC 连接句柄。  
  
 *pData*  
 指向要复制的数据的指针。 如果绑定的数据类型是大值类型 （如 SQLTEXT 或 SQLIMAGE）， *pData*可以为 NULL。 为空*pData*指示长数据值将发送到 SQL Server 中使用的区块[bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md)。  
  
 如果*pData*设置为 NULL，并且列与绑定字段对应的大值类型，不是**bcp_colptr**失败。  
  
 有关大值类型的详细信息，请参阅[bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) **。**  
  
 *idxServerCol*  
 数据复制的目标数据库表中的列的序号位置。 表中的第一列为列 1。 报告列的序号位置[SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md)。  
  
## <a name="returns"></a>返回  
 SUCCEED 或 FAIL。  
  
## <a name="remarks"></a>备注  
 **Bcp_colptr**功能，您可以更改特定列的源数据的地址，将数据复制到与 SQL Server 时[bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md)。  
  
 最初，通过调用设置的用户数据指针**bcp_bind**。 如果程序变量数据地址发生更改调用之间**bcp_sendrow**，可以调用**bcp_colptr**重置的数据的指针。 下次调用**bcp_sendrow**发送到调用的数据**bcp_colptr**。  
  
 必须单独**bcp_colptr**要修改其数据地址的表中的每列的调用。  
  
## <a name="see-also"></a>请参阅  
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
