---
title: SqlErrorLogFile 类 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: wmi
ms.topic: reference
ms.assetid: 2b83ae4a-c0d4-414c-b6e5-a41ec7c13159
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 849a487cefc5ec0be58eabcc2e312b7dd047aa4b
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68052468"
---
# <a name="sqlerrorlogfile-class"></a>SqlErrorLogFile 类
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  提供用于查看与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日志文件有关的信息的属性。  
  
## <a name="syntax"></a>语法  
  
```  
  
class SQLErrorLogFile  
{  
   uint32ArchiveNumber;  
   stringInstanceName;  
   datetimeLastModified;  
   uint32LogFileSize;  
   stringName;  
  
};  
```  
  
## <a name="properties"></a>properties  
 SQLErrorLogFile 类定义以下属性。  
  
|||  
|-|-|  
|ArchiveNumber|数据类型： **uint32**<br /><br /> 访问类型：只读<br /><br /> <br /><br /> 日志文件的存档号。|  
|InstanceName|数据类型：**字符串**<br /><br /> 访问类型：只读<br /><br /> 限定符：键<br /><br /> <br /><br /> 日志文件所在的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|  
|LastModified|数据类型：**日期时间**<br /><br /> 访问类型：只读<br /><br /> <br /><br /> 上次修改日志文件的日期。|  
|LogFileSize|数据类型： **uint32**<br /><br /> 访问类型：只读<br /><br /> <br /><br /> 日志文件大小（字节）。|  
|名称|数据类型：**字符串**<br /><br /> 访问类型：只读<br /><br /> 限定符：键<br /><br /> <br /><br /> 日志文件名。|  
  
## <a name="remarks"></a>备注  
  
|||  
|-|-|  
|MOF|Sqlmgmprovider xpsp2up.mof|  
|DLL|Sqlmgmprovider.dll|  
|命名空间|\root\Microsoft\SqlServer\ComputerManagement10|  
  
## <a name="example"></a>示例  
 下面的示例检索与指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日志文件有关的信息。 若要运行该示例，请替换\< *Instance_Name*> 实例，例如，Instance1 的名称。  
  
```  
on error resume next  
set strComputer = "."  
set objWMIService = GetObject("winmgmts:\\.\root\Microsoft\SqlServer\ComputerManagement10")  
set LogFiles = objWmiService.ExecQuery("SELECT * FROM SqlErrorLogFile WHERE InstanceName = '<Instance_Name>'")  
  
For Each logFile in LogFiles  
  
WScript.Echo "Instance Name:  " & logFile.InstanceName & vbNewLine _  
    & "Log File Name:  " & logFile.Name & vbNewLine _  
    & "Archive Number: " & logFile.ArchiveNumber & vbNewLine _  
    & "Log File Size:  " & logFile.LogFileSize & " bytes" & vbNewLine _  
    & "Last Modified:  " & logFile.LastModified & vbNewLine _  
  
Next   
```  
  
## <a name="comments"></a>注释  
 当*InstanceName*未提供在 WQL 语句中，则查询将返回默认实例的信息。 例如，以下 WQL 语句将返回与来自默认实例 (MSSQLSERVER) 的所有日志文件有关的信息。  
  
```  
"SELECT * FROM SqlErrorLogFile"  
```  
  
## <a name="security"></a>安全性  
 若要连接到[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]日志文件通过 WMI，必须在本地和远程计算机上具有以下权限：  
  
-   读取访问权限**Root\Microsoft\SqlServer\ComputerManagement10** WMI 命名空间。 默认情况下，每个人都可以通过“启用帐户”权限获得读取权限。  
  
    > [!NOTE]  
    >  有关如何验证 WMI 权限的信息，请参阅本主题的安全性部分[查看脱机日志文件](../../relational-databases/logs/view-offline-log-files.md)。  
  
-   包含错误日志的文件夹的读取权限。 默认情况下，错误日志位于以下路径中 (其中\<*驱动器 >* 表示您安装的驱动器[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]并\< *InstanceName*> 是实例的名称[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]):  
  
     **\<驱动器 >: \Program Files\Microsoft SQL Server\MSSQL11** **。\<实例名 > \MSSQL\Log**  
  
 如果您在通过防火墙进行连接，则请确保在防火墙中针对远程目标计算机上的 WMI 设置例外。 有关详细信息，请参阅[连接到 WMI 远程启动 Windows Vista](https://go.microsoft.com/fwlink/?LinkId=178848)。  
  
## <a name="see-also"></a>请参阅  
 [SqlErrorLogEvent 类](../../relational-databases/wmi-provider-configuration-classes/sqlerrorlogevent-class.md)   
 [查看脱机日志文件](../../relational-databases/logs/view-offline-log-files.md)  
  
  
