---
title: 访问扩展事件日志中的诊断信息 | Microsoft Docs
ms.date: 03/14/2017
ms.reviewer: ''
ms.prod: sql
ms.technology: native-client
ms.topic: reference
ms.assetid: aaa180c2-5e1a-4534-a125-507c647186ab
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a60a54d930f87ebe054d3d414b31049fd26a4a2a
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68103427"
---
# <a name="accessing-diagnostic-information-in-the-extended-events-log"></a>访问扩展事件日志中的诊断信息
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  从开始[!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 和数据访问跟踪 ([数据访问跟踪](https://go.microsoft.com/fwlink/?LinkId=125805)) 已更新，以使其更轻松地从连接环中获得有关连接失败的诊断信息从扩展的事件日志缓冲区和应用程序性能信息。  
  
 有关读取扩展事件日志的信息，请参阅[查看事件会话数据](https://msdn.microsoft.com/library/ac742a01-2a95-42c7-b65e-ad565020dc49)。  
  
> [!NOTE]  
>  该功能只用于故障排除和诊断目的，可能不适合审核或安全目的。  
  
## <a name="remarks"></a>备注  
 对于连接操作，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 将发送客户端连接 ID。 如果连接失败，可以访问连接环形缓冲区（[在 SQL Server 2008 中使用连接环形缓冲区解决连接问题](https://go.microsoft.com/fwlink/?LinkId=207752)），查找 ClientConnectionID 字段并获取有关连接失败的诊断信息  。 仅当发生错误时，才在环形缓冲区中记录客户端连接 ID。 （如果在发送预登录数据包前连接失败，将不会生成客户端连接 ID。）客户端连接 ID 是 16 字节的 GUID。 如果将 client_connection_id 操作添加到扩展事件会话中的事件，则还可以在扩展事件输出目标中找到客户端连接 ID  。 如果需要进一步的诊断帮助，则可以启用数据访问跟踪并再次运行连接命令，然后观察数据访问跟踪中的 ClientConnectionID 字段以查看是否有失败的操作  。  
  
 如果您使用的中的 ODBC[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]本机客户端且连接成功，你可以获取客户端使用的连接 ID **SQL_COPT_SS_CLIENT_CONNECTION_ID**属性与[SQLGetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md).  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 还发送特定于线程的活动 ID。 如果开始会话时启用了 TRACK_CAUSAILITY 选项，则会在扩展的事件会话中捕获活动 ID。 对于与活动连接有关的性能问题，可以从客户端的数据访问跟踪中获取活动 ID（ActivityID 字段），然后在扩展事件输出中找到该活动 ID  。 扩展事件中的活动 ID 是 16 字节 GUID（与客户端连接 ID 的 GUID 不同），且后面追加四个字节的序列号。 该序列号表示某个请求在线程内的顺序，并指示线程的批处理和 RPC 语句的相对顺序。 当启用数据访问跟踪且数据访问跟踪配置字中的第 18 位设置为“打开”时，可以针对 SQL 批处理语句和 RPC 请求发送 ActivityID（可选）  。  
  
 以下是一个使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 启动扩展事件会话的示例，该会话将存储在环形缓冲区中，并将记录从客户端针对 RPC 和批处理操作发送的活动 ID。  
  
```  
create event session MySession on server   
add event connectivity_ring_buffer_recorded,   
add event sql_statement_starting (action (client_connection_id)),   
add event sql_statement_completed (action (client_connection_id)),   
add event rpc_starting (action (client_connection_id)),   
add event rpc_completed (action (client_connection_id))  
add target ring_buffer with (track_causality=on)  
  
```  
  
## <a name="control-file"></a>控制文件  
 在 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 中，SQL Server Native Client 控制文件 (ctrl.guid.snac11) 的内容为：  
  
```  
{8B98D3F2-3CC6-0B9C-6651-9649CCE5C752}  0x00000000  0   MSDADIAG.ETW  
{2DA81B52-908E-7DB6-EF81-76856BB47C4F}  0xFFFFFFFF  0   SQLNCLI11.1  
```  
  
## <a name="mof-file"></a>MOF 文件  
 在 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 中，SQL Server Native Client mof 文件的内容为：  
  
```  
#pragma classflags("forceupdate")  
#pragma namespace ("\\\\.\\Root\\WMI")  
  
/////////////////////////////////////////////////////////////////////////////  
//  
//  MSDADIAG.ETW  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW"),  
 Guid("{8B98D3F2-3CC6-0B9C-6651-9649CCE5C752}"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW : EventTrace  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW"),  
 Guid("{8B98D3F3-3CC6-0B9C-6651-9649CCE5C752}"),  
 DisplayName("msdadiag"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace : Bid2Etw_MSDADIAG_ETW  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW formatted output (A)"),  
 EventType(17),  
 EventTypeName("TextA"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace_TextA : Bid2Etw_MSDADIAG_ETW_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringA"),  
     extension("RString"),  
     read  
    ]  
    object msgStr;  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW formatted output (W)"),  
 EventType(18),  
 EventTypeName("TextW"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace_TextW : Bid2Etw_MSDADIAG_ETW_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringW"),  
     extension("RWString"),  
     read  
    ]  
    object msgStr;  
};  
  
/////////////////////////////////////////////////////////////////////////////  
//  
//  SQLNCLI11.1  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1"),  
 Guid("{2DA81B52-908E-7DB6-EF81-76856BB47C4F}"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1 : EventTrace  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1"),  
 Guid("{2DA81B53-908E-7DB6-EF81-76856BB47C4F}"),  
 DisplayName("SQLNCLI11.1"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1_Trace : Bid2Etw_SQLNCLI11_1  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1 formatted output (A)"),  
 EventType(17),  
 EventTypeName("TextA"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1_Trace_TextA : Bid2Etw_SQLNCLI11_1_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringA"),  
     extension("RString"),  
     read  
    ]  
    object msgStr;  
};  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1 formatted output (W)"),  
 EventType(18),  
 EventTypeName("TextW"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1_Trace_TextW : Bid2Etw_SQLNCLI11_1_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringW"),  
     extension("RWString"),  
     read  
    ]  
    object msgStr;  
};  
```  
  
## <a name="see-also"></a>请参阅  
 [处理错误和消息](../../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  
  
