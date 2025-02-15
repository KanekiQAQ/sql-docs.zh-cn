---
title: 'Ibcpsession:: Bcpexec (OLE DB) |Microsoft Docs'
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- IBCPSession::BCPExec (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPExec method
ms.assetid: 0f4ebb63-cf03-4e53-846e-6c3021cde007
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e63acff9459065a2daaeadcd1831e9a367322037
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68091044"
---
# <a name="ibcpsessionbcpexec-ole-db"></a>IBCPSession::BCPExec (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  执行大容量复制操作。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT BCPExec(   
      DBROWCOUNT *pRowsCopied);  
```  
  
## <a name="remarks"></a>备注  
 BCPExec 方法将数据从用户文件复制到数据库表或执行相反的操作，具体取决于与 [IBCPSession::BCPInit](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpinit-ole-db.md) 方法一起使用的 eDirection 参数的值   。  
  
 在调用 BCPExec 之前，请用有效的用户文件名调用 BCPInit 方法   。 如果没有这样做，会导致错误。 唯一的例外就是如果查询要用于大容量复制操作。 这种情况下，在 BCPInit 方法中将表名指定为 NULL，然后使用 BCP_OPTION_HINTS 选项指定查询  。  
  
 BCPExec 方法是可能胜任任何时间长度的唯一大容量复制方法  。 因此，它是支持异步模式的唯一大容量复制方法。 若要使用异步模式，请在调用 BCPExec 方法之前，将特定于提供程序的会话属性 SSPROP_ASYNCH_BULKCOPY 设置为 VARIANT_TRUE  。 此属性位于 DBPROPSET_SQLSERVERSESSION 属性集中。 若要完成测试，请用相同参数调用 BCPExec 方法  。 如果大容量复制尚未完成，则 BCPExec 方法会返回 DB_S_ASYNCHRONOUS  。 它还会在 pRowsCopied 参数中返回已发送到服务器或从服务器接收的行数的状态计数  。 发送到服务器的行直到到达批的末尾时才会提交。  
  
## <a name="arguments"></a>参数  
 pRowsCopied  [out]  
 指向 DWORD 的指针。 BCPExec 方法用成功复制的行数填充 DWORD  。 如果将 pRowsCopied 参数设置为 NULL，则 BCPExec 方法会忽略该参数   。  
  
## <a name="return-code-values"></a>返回代码值  
 S_OK  
 方法成功。  
  
 E_FAIL  
 出现访问接口特定的错误；若要获取详细信息，请使用 [ISQLServerErrorInfo](https://msdn.microsoft.com/library/a8323b5c-686a-4235-a8d2-bda43617b3a1) 接口。  
  
 E_UNEXPECTED  
 意外调用了该方法。 例如，在调用该方法之前，未调用 BCPInit 方法  。 如果通过使用 BCP_OPTION_ABORT 选项中止了操作，并且随后调用了 BCPExec 方法，则也会发生此调用  。  
  
 E_OUTOFMEMORY  
 内存不足错误。  
  
 DB_S_ENDOFROWSET  
 大容量复制操作完成，所有数据传输已完成。  
  
 DB_S_ASYNCHRONOUS  
 当前这批行已复制。 请再次调用 BCPExec 方法以传输下一批  。  
  
 DB_S_ERRORSOCCURRED  
 在大容量复制操作期间出现错误，可能尚未复制某些行。 错误数仍然小于允许的最大错误数。  
  
## <a name="see-also"></a>请参阅  
 [IBCPSession &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-ole-db.md)   
 [执行大容量复制操作](../../relational-databases/native-client/features/performing-bulk-copy-operations.md)  
  
  
