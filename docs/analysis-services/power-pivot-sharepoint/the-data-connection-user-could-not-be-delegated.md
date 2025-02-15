---
title: 无法对用户数据的连接进行委托 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: cbf9b41b58e4c492c4b278aa4cad60fa26dbcb08
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68208011"
---
# <a name="the-data-connection-user-could-not-be-delegated"></a>不能委派用户数据的连接
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  对于包含 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据的 Excel 工作簿，如果 Excel Services 无法连接到 SharePoint 中的 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 服务器实例，则会返回此错误。  
  
## <a name="details"></a>详细信息  
  
|||  
|-|-|  
|适用于|[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint|  
|产品版本|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]、 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|  
|原因|在尝试使用 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据访问接口时出现连接错误。|  
|消息正文|数据连接使用 Windows 身份验证并且无法对用户凭据进行委托。 以下连接无法刷新：[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据|  
  
## <a name="explanation"></a>解释  
 有多个原因会导致出现此错误消息。 但所有这些原因都具有一个共性，就是 Excel Services 无法从 SharePoint 的声明令牌中获取有效的 Windows 用户标识。 在 Excel 工作簿包含 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据时，如果以下任何条件成立，则会出现此错误：  
  
-   Claims to Windows Token Service 未运行。 您可以通过查看 SharePoint 日志文件确认导致此错误的原因。 如果 SharePoint 日志包括消息“The pipe endpoint 'net.pipe://localhost/s4u/022694f3-9fbd-422b-b4b2-312e25dae2a2' could not be found on your local machine”，则 Claims to Windows Token Service 未运行。 若要启动该服务，请使用管理中心，然后在“服务”控制台应用程序中确认该服务正在运行。  
  
-   域控制器不可用于确认用户标识。 Claims to Windows Token Service 不使用缓存凭据。 它会验证每个连接的用户标识。 您可以通过查看 SharePoint 日志文件确认导致此错误的原因。 如果 SharePoint 日志包括消息“Failed to get WindowsIdentity from IClaimsIdentity”，则无法对该用户标识进行身份验证。  
  
-   计算机必须是同一域中的成员，或者必须处于具有双向信任关系的域中。  
  
-   您必须使用 Windows 域用户帐户。 这些帐户必须具有通用主体名称 (UPN)。  
  
-   Excel Services 服务帐户必须具有 Active Directory 权限以便查询对象。  
  
## <a name="user-action"></a>用户操作  
 使用以下说明可以查看 Claims to Windows Token Service 的状态。  
  
 对于所有其他情况，请与您的网络管理员联系。  
  
#### <a name="enable-claims-to-windows-token-service"></a>启用 Claims to Windows Token Service  
  
1.  在管理中心的“系统设置”中，单击 **“管理服务器上的服务”** 。  
  
2.  选择 **“Claims to Windows Token Service”** ，然后单击 **“开始”** 。  
  
3.  验证服务是否也在“服务”控制台上运行：  
  
    1.  在“管理工具”中，单击“服务”。  
  
    2.  如果 Claims to Windows Token Service 未运行，则启动它。  
  
## <a name="see-also"></a>请参阅  
 [配置 Power Pivot 服务帐户](../../analysis-services/power-pivot-sharepoint/configure-power-pivot-service-accounts.md)  
  
  
