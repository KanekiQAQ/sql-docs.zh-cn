---
title: srv_paramlen（扩展存储过程 API）| Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
apiname:
- srv_paramlen
apilocation:
- opends60.dll
apitype: DLLExport
dev_langs:
- C++
helpviewer_keywords:
- srv_paramlen
ms.assetid: d1fe92ff-cad6-4396-8216-125e5642e81e
author: rothja
ms.author: jroth
ms.openlocfilehash: 5c89a9ddc1020f29bbcd661ec4c9672ba37f7770
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68005704"
---
# <a name="srvparamlen-extended-stored-procedure-api"></a>srv_paramlen（扩展存储过程 API）
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]请改用 CLR 集成。  
  
 返回远程存储过程调用参数的数据长度。 此函数已被 srv_paraminfo 函数取代  。  
  
## <a name="syntax"></a>语法  
  
```  
  
int srv_paramlen (  
SRV_PROC *  
srvproc  
,  
int  
n   
);  
```  
  
## <a name="arguments"></a>参数  
 srvproc   
 指向作为特定客户端连接句柄（在这里为接收远程存储过程调用的句柄）的 SRV_PROC 结构的指针。 该结构包含扩展存储过程 API 库用于管理应用程序和客户端之间的通信和数据的信息。  
  
 *n*  
 指示参数的编号。 第一个参数是 1。  
  
## <a name="returns"></a>返回  
 参数数据的实际长度（字节）。 如果没有第 n 个参数或没有远程存储过程，则返回 -1  。 如果第 n 个参数为 NULL，则返回 0  。  
  
 如果参数为以下 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 系统数据类型之一，则此函数返回以下值。  
  
|新数据类型|输入数据长度|  
|--------------------|-----------------------|  
|**BITN**|**NULL：** 1<br /><br /> **ZERO：** 1<br /><br /> **>=255：** N/A<br /><br /> **<255：** N/A|  
|**BIGVARCHAR**|**NULL：** 0<br /><br /> **ZERO：** 1<br /><br /> **>=255：** 255<br /><br /> <255：实际长度  |  
|**BIGCHAR**|**NULL：** 0<br /><br /> **ZERO：** 255<br /><br /> **>=255：** 255<br /><br /> **<255：** 255|  
|**BIGBINARY**|**NULL：** 0<br /><br /> **ZERO：** 255<br /><br /> **>=255：** 255<br /><br /> **<255：** 255|  
|**BIGVARBINARY**|**NULL：** 0<br /><br /> **ZERO：** 1<br /><br /> **>=255：** 255<br /><br /> <255：实际长度  |  
|**NCHAR**|**NULL：** 0<br /><br /> **ZERO：** 255<br /><br /> **>=255：** 255<br /><br /> **<255：** 255|  
|**NVARCHAR**|**NULL：** 0<br /><br /> **ZERO：** 1<br /><br /> **>=255：** 255<br /><br /> <255：实际长度  |  
|**NTEXT**|**NULL：** -1<br /><br /> **ZERO：** -1<br /><br /> **>=255：** -1<br /><br /> **\<255：** -1|  
  
 \*   实际长度  = 多字节字符串 (cch) 的长度   
  
## <a name="remarks"></a>Remarks  
 每个远程存储过程参数都具有实际数据长度和最大数据长度。 对于不允许使用 Null 值的标准固定长度数据类型，实际长度和最大长度相同。 对于可变长度数据类型，长度可以变化。 例如，声明为 varchar(30) 的参数可以具有长度仅为 10 个字节的数据  。 该参数的实际长度为 10，最大长度为 30。 srv_paramlen 函数获取远程存储过程的实际数据长度（以字节为单位）  。 要获取参数的最大数据长度，请使用 srv_parammaxlen  。  
  
 使用参数调用远程存储过程时，可以按名称或位置（未命名）传递参数。 如果使用部分按名称传递，部分按位置传递的参数调用远程存储过程，则会发生错误。 仍然会调用 SRV_RPC 处理程序，但是它看起来没有参数并且 srv_rpcparams 返回 0  。  
  
> [!IMPORTANT]  
>  应全面检查扩展存储过程的源代码，并在生产服务器中安装编译的 DLL 之前，对这些 DLL 进行测试。 有关安全检查和测试的信息，请访问此 [Microsoft 网站](https://go.microsoft.com/fwlink/?LinkID=54761&amp;clcid=0x409 https://msdn.microsoft.com/security/)。  
  
## <a name="see-also"></a>另请参阅  
 [srv_paraminfo（扩展存储过程 API）](../../relational-databases/extended-stored-procedures-reference/srv-paraminfo-extended-stored-procedure-api.md)   
 [srv_rpcparams（扩展存储过程 API）](../../relational-databases/extended-stored-procedures-reference/srv-rpcparams-extended-stored-procedure-api.md)  
  
  
