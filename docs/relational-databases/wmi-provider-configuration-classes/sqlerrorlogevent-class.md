---
title: SqlErrorLogEvent 类 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: wmi
ms.topic: reference
helpviewer_keywords:
- SqlErrorLogEvent class
- SqlErrorLogFile class
ms.assetid: bde6c467-38d0-4766-a7af-d6c9d6302b07
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: e36d05c39fb3bc4fc19d2ea0c28def7cd626fbf1
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68052526"
---
# <a name="sqlerrorlogevent-class"></a>SqlErrorLogEvent 类
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  提供用于查看指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日志文件中的事件的属性。  
  
## <a name="syntax"></a>语法  
  
```  
  
class SQLErrorLogEvent   
{  
   stringFileName;  
   stringInstanceName;  
   datetimeLogDate;  
   stringMessage;  
   stringProcessInfo;  
};  
```  
  
## <a name="properties"></a>properties  
 SQLErrorLogEvent 类定义以下属性。  
  
|||  
|-|-|  
|FileName|数据类型：**字符串**<br /><br /> 访问类型：只读<br /><br /> <br /><br /> 错误日志文件的名称。|  
|InstanceName|数据类型：**字符串**<br /><br /> 访问类型：只读<br /><br /> 限定符：键<br /><br /> 日志文件所在的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|  
|LogDate|数据类型：**日期时间**<br /><br /> 访问类型：只读<br /><br /> 限定符：键<br /><br /> <br /><br /> 在日志文件中记录该事件的日期和时间。|  
|Message|数据类型：**字符串**<br /><br /> 访问类型：只读<br /><br /> <br /><br /> 事件消息。|  
|ProcessInfo|数据类型：**字符串**<br /><br /> 访问类型：只读<br /><br /> <br /><br /> 与事件的源服务器进程 ID (SPID) 有关的信息。|  
  
## <a name="remarks"></a>备注  
  
|||  
|-|-|  
|MOF|Sqlmgmproviderxpsp2up.mof|  
|DLL|Sqlmgmprovider.dll|  
|命名空间|\root\Microsoft\SqlServer\ComputerManagement10|  
  
## <a name="example"></a>示例  
 下面的示例显示如何检索指定的日志文件中所有记录的事件的值。 若要运行该示例，请替换\< *Instance_Name*> 的实例的名称与[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，例如 Instance1 和替换 File_Name 错误日志文件的名称，例如 ERRORLOG.1。  
  
```  
on error resume next  
strComputer = "."  
Set objWMIService = GetObject("winmgmts:" _  
    & "{impersonationLevel=impersonate}!\\" _  
    & strComputer & "\root\MICROSOFT\SqlServer\ComputerManagement10")  
set logEvents = objWmiService.ExecQuery("SELECT * FROM SqlErrorLogEvent WHERE InstanceName = '<Instance_Name>' AND FileName = 'File_Name'")  
  
For Each logEvent in logEvents  
WScript.Echo "Instance Name: " & logEvent.InstanceName & vbNewLine _  
    & "Log Date: " & logEvent.LogDate & vbNewLine _  
    & "Log File Name: " & logEvent.FileName & vbNewLine _  
    & "Process Info: " & logEvent.ProcessInfo & vbNewLine _  
    & "Message: " & logEvent.Message & vbNewLine _  
  
Next  
```  
  
## <a name="comments"></a>注释  
 当*InstanceName*或*FileName*未提供在 WQL 语句中，该查询将返回默认实例和当前信息[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]日志文件。 例如，以下 WQL 语句将从默认实例 (MSSQLSERVER) 上的当前日志文件 (ERRORLOG) 返回所有日志事件。  
  
```  
"SELECT * FROM SqlErrorLogEvent"  
```  
  
## <a name="security"></a>安全性  
 若要连接到[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]日志文件通过 WMI，必须在本地和远程计算机上具有以下权限：  
  
-   读取访问权限**Root\Microsoft\SqlServer\ComputerManagement10** WMI 命名空间。 默认情况下，每个人都可以通过“启用帐户”权限获得读取权限。  
  
-   包含错误日志的文件夹的读取权限。 默认情况下，错误日志位于以下路径中 (其中\<*驱动器 >* 表示您安装的驱动器[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]并\< *InstanceName*> 是实例的名称[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]):  
  
     **\<驱动器 >: \Program Files\Microsoft SQL Server\MSSQL13** **。\<实例名 > \MSSQL\Log**  
  
 如果您在通过防火墙进行连接，则请确保在防火墙中针对远程目标计算机上的 WMI 设置例外。 有关详细信息，请参阅[连接到 WMI 远程启动 Windows Vista](https://go.microsoft.com/fwlink/?LinkId=178848)。  
  
## <a name="see-also"></a>请参阅  
 [SqlErrorLogFile 类](../../relational-databases/wmi-provider-configuration-classes/sqlerrorlogfile-class.md)   
 [查看脱机日志文件](../../relational-databases/logs/view-offline-log-files.md)  
  
  
