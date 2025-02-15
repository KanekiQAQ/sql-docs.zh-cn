---
title: 合并成员临时表 (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- database [Master Data Services], attributes staging table
- attributes staging table [Master Data Services]
ms.assetid: 070681ed-be99-49ae-93bd-6402f2134ace
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 35e68ce4e92a9de688ab686af7244dc6b597989c
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67911428"
---
# <a name="consolidated-member-staging-table-master-data-services"></a>合并成员临时表 (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  使用 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库中的合并成员临时表 (stg.name_Consolidated) 可以创建、更新、停用和删除合并成员。 您还可以使用它更新合并成员的属性值。  
  
##  <a name="TableColumns"></a> 表列  
 下表说明 Consolidated 临时表中每个字段的用途。  
  
|列名|描述|  
|-----------------|-----------------|  
|**ID**|自动分配的标识符。 不要在此字段中输入值。 如果尚未处理此批次，则此字段为空。|  
|**ImportType**<br /><br /> 必填|确定临时数据与 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库中已存在的数据匹配时执行何种操作。<br /><br /> **0**：创建新成员。 使用临时数据替换现有的 MDS 数据，但是仅限临时数据不为 NULL 时。 忽略 NULL 值。 若要将属性值更改为 NULL，请使用 **~NULL~** 。<br /><br /> **1**：仅创建新成员。 对现有 MDS 数据的任何更新都将失败。<br /><br /> **2**：创建新成员。 使用临时数据替换现有的 MDS 数据。 如果导入了 NULL 值，它们将覆盖现有的 MDS 值。<br /><br /> **3**：基于 Code 值停用成员。 维护所有属性、层次结构和集合成员身份以及事务，但是在 UI 中已无法使用。 如果成员用作另一个成员的基于域的属性值，则无法停用。<br /><br /> **4**：基于 Code 值永久删除成员。 永久删除所有属性、层次结构和集合成员身份以及事务。 如果成员用作另一个成员的基于域的属性值，则无法删除。|  
|**ImportStatus_ID**<br /><br /> 必填|导入过程的状态。 可能的值有：<br /><br /> **0**- 指定此值表示记录已经准备好临时存储。<br /><br /> **1**- 自动分配的值，表示记录的暂存过程已成功。<br /><br /> **2**- 自动分配的值，表示记录的临时过程已失败。|  
|**Batch_ID**<br /><br /> 仅对 Web 服务为必需的|自动分配的标识符，该标识符将记录分组以便临时存储。 将为此批次中的所有成员分配此标识符，此标识符显示在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 用户界面中的 **ID** 列。<br /><br /> 如果尚未处理此批次，则此字段为空。|  
|**BatchTag**<br /><br /> 必需，但是 Web 服务除外|批次的唯一名称，最多包含 50 个字符。|  
|**HierarchyName**<br /><br /> 必填|显式层次结构名称。 每个合并成员只能属于一个层次结构。|  
|**ErrorCode**|显示错误代码。 有关 **ImportStatus_ID** 为 **2**的所有记录，请参阅 [临时过程错误 (Master Data Services)](../master-data-services/staging-process-errors-master-data-services.md)。|  
|**Code**<br /><br /> 必需，自动为 **ImportType1** 或 **2** 生成代码的情况除外。有关详细信息，请参阅[自动创建代码 (Master Data Services)](../master-data-services/automatic-code-creation-master-data-services.md)|成员的唯一代码。|  
|**名称**<br /><br /> 可选|成员的名称。|  
|**NewCode**|仅当要更改成员代码时才使用。|  
|\<属性名称>|实体中的每个属性对应的列。 对于 **ImportType** 为 **0** 或 **2**时使用它。 对于自由格式属性，请为该属性指定新的文本或字符串值。 对于基于域的属性，请为将要成为属性的成员指定代码。 对于链接属性，URL 必须以“https://”开头  。<br /><br /> <br /><br /> 注意:不能暂存文件属性。|  
  
## <a name="see-also"></a>请参阅  
 [概述：导入表中数据 (Master Data Services)](../master-data-services/overview-importing-data-from-tables-master-data-services.md)   
 [查看暂存过程中出现的错误 (Master Data Services)](../master-data-services/view-errors-that-occur-during-staging-master-data-services.md)   
 [临时过程错误 (Master Data Services)](../master-data-services/staging-process-errors-master-data-services.md)  
  
  
