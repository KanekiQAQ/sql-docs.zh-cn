---
title: 步骤 7：添加和配置 OLE DB 目标 | Microsoft Docs
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 442c841d-d528-4bf0-8724-7156f909ee50
author: janinezhang
ms.author: janinez
ms.openlocfilehash: 042d304dcdd0519d858fcf7369e9b200a57f1959
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67902592"
---
# <a name="lesson-1-7-add-and-configure-the-ole-db-destination"></a>第 1-7 课：添加和配置 OLE DB 目标

[!INCLUDE[ssis-appliesto](../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]



现在，包可以从平面文件源中提取数据，并将该数据转换为与目标兼容的格式。 下一个任务是将已转换的数据加载到目标。 要加载数据，请将 OLE DB 目标添加到数据流。 OLE DB 目标可以使用数据库表、视图或 SQL 命令将数据加载到各种 OLE DB 兼容的数据库中。  
  
在此任务中，将添加和配置 OLE DB 目标以使用以前创建的 OLE DB 连接管理器。  
  
## <a name="add-and-configure-the-sample-ole-db-destination"></a>添加和配置示例 OLE DB 目标  
  
1.  在“SSIS 工具箱”中，展开“其他目标”，并将“OLE DB 目标”拖到“数据流”选项卡的设计图面上。将 OLE DB 目标直接放置在“Lookup Date Key”转换的下面。  
  
2.  选择“Lookup Date Key”转换，并将蓝色箭头拖到新“OLE DB 目标”上，以便将两个组件连接在一起。  
  
3.  在“选择输入输出”对话框中，选择“输出”列表框中的“查找匹配输出”，然后选择“确定”。  
  
4.  在“数据流”设计图面上，在新“OLE DB 目标”组件中选择名称“OLE DB 目标”，然后将名称更改为 Sample OLE DB Destination。  
  
5.  双击 **Sample OLE DB Destination**。  
  
6.  在“OLE DB 目标编辑器”对话框中，确保已在“OLE DB 连接管理器”框中选中 localhost.AdventureWorksDW2012。  
  
7.  在“表或视图的名称”框中，输入或选择 [dbo].[FactCurrencyRate]。  
  
8.  选择“新建”按钮以创建新表。  将脚本中的表名从“示例 OLE DB 目标”更改为 NewFactCurrencyRate。  选择“确定”。  
  
9. 选择“确定”后，该对话框关闭，“表或视图的名称”自动更改为 NewFactCurrencyRate。  
  
10. 选择“映射”。  
  
11. 验证 **AverageRate**、 **CurrencyKey**、 **EndOfDayRate**以及 **DateKey** 输入列是否已正确映射到目标列。 如果映射了同名列，则说明映射正确。  
  
12. 选择“确定”。  
  
13. 右键单击“Sample OLE DB Destination”目标，再选择“属性”。  
  
14. 在“属性”窗口中，确认“LocaleID”属性已设置为“英语(美国)”，且“DefaultCodePage”属性已设置为“1252”。  
  
## <a name="go-to-next-task"></a>转到下一个任务
[步骤 8：为第 1 课包添加批注并设置格式](../integration-services/lesson-1-8-making-the-lesson-1-package-easier-to-understand.md)  
  
## <a name="see-also"></a>另请参阅  
[OLE DB 目标](../integration-services/data-flow/ole-db-destination.md)  
  
  
  
