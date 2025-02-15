---
title: 注释、 形状、 Excel Services 不支持其他对象 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: df13c3fa92d5439e559e286424438569b3ead86b
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68164394"
---
# <a name="comments-shapes-other-objects-not-supported-by-excel-services"></a>注释形状，不支持 Excel Services 的其他对象
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  在你将切片器从 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 字段列表添加到 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿时会发生此错误。  
  
## <a name="details"></a>详细信息  
  
|||  
|-|-|  
|适用于|[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint|  
|产品版本|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]、 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|  
|原因|Excel Web Access 无法呈现用于控制从 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 字段列表添加到某一工作簿的切片器的位置和格式的形状对象。|  
|消息正文|Excel Services 不支持以下功能并且可能无法显示或者只能部分显示以下功能：<br /><br /> 注释、形状或其他对象<br /><br /> 某些功能（例如外部数据查询）显示只能在 Microsoft Excel 中刷新的缓存数据。|  
  
## <a name="explanation"></a>解释  
 当你在浏览器中打开 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿并且对于“不支持的功能: 此工作簿可能无法按预期显示”  消息单击“详细信息”  按钮时，Excel Web Access 将显示此错误。  
  
 出现此错误的原因是因为 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿包含其布局由 Excel 中隐藏的形状对象控制的切片器。 该形状对象在水平和垂直位置上控制切片器的格式和位置。  
  
 Excel Services 无法呈现形状对象，但因为该对象是隐藏的，所以尽管无法呈现，但这并不是问题。  
  
## <a name="user-action"></a>用户操作  
 可以忽略此错误。 单击 **“确定”** 以便关闭该错误消息并且可以放心地继续使用工作簿和切片器。  
  
  
