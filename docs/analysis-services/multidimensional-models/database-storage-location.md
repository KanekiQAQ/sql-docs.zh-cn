---
title: 数据库存储位置 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0544e7e7d552098806d4c3d93751631d03520139
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62714794"
---
# <a name="database-storage-location"></a>数据库存储位置
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  通常会出现这样的情况， [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据库管理员 (dba) 希望某个数据库驻留在服务器数据文件夹之外。 这些情况通常是由于业务需要，如提高性能或扩展存储。 对于上述情况，通过 **DbStorageLocation** 数据库属性， [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dba 可以在本地磁盘或网络设备中指定数据库的位置。  
  
## <a name="dbstoragelocation-database-property"></a>DbStorageLocation 数据库属性  
 **DbStorageLocation** 数据库属性指定了 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 在其中创建和管理所有数据库数据和元数据文件的文件夹。 除数据库元数据文件之外（它存储在服务器数据文件夹中），所有元数据文件都存储在 **DbStorageLocation** 文件夹中。 在设置 **DbStorageLocation** 数据库属性的值时，需考虑两个重要的注意事项：  
  
-   必须将 **DbStorageLocation** 数据库属性设置为现有 UNC 文件夹路径或空字符串。 空字符串是服务器数据文件夹的默认值。 如果该文件夹不存在，则在执行 **Create**、 **Attach**、或 **Alter** 命令时会产生错误。  
  
-   不能将 **DbStorageLocation** 数据库属性设置为指向服务器数据文件夹或它的任何一个子文件夹。 如果该位置指向服务器数据文件夹或它的任何一个子文件夹，则在执行 **Create**、 **Attach**、或 **Alter** 命令时会产生错误。  
  
> [!IMPORTANT]  
>  建议您设置 UNC 路径以使用存储区域网络 (SAN)、基于 iSCSI 的网络或本地附加的磁盘。 网络共享的任何 UNC 路径或任何长滞后时间远程存储解决方案导致不支持的安装.  
  
### <a name="dbstoragelocation-compared-to-storagelocation"></a>相对于 StorageLocation 的 DbStorageLocation  
 **DbStorageLocation** 指定了所有数据库数据和元数据文件所在的文件夹，而 **StorageLocation** 指定了多维数据集的一个或多个分区所在的文件夹。 **StorageLocation** 可以独立于 **DbStorageLocation**进行设置。 这是 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dba 根据预期的结果做出的决定，很多时候一个属性或另一个属性的使用会重叠。  
  
## <a name="dbstoragelocation-usage"></a>DbStorageLocation 用法  
 **DbStorageLocation** 数据库属性用作   “分离/附加”数据库命令序列、   “备份/还原”数据库命令序列或“同步”  数据库命令中的“创建”  数据库命令的一部分。 更改 **DbStorageLocation** 数据库属性被认为是数据库对象的结构更改。 这意味着必须重新创建所有元数据并且重新处理数据。  
  
> [!IMPORTANT]  
>  不应使用 **Alter** 命令更改数据库存储位置。 相反，我们建议使用一系列 **Detach**/**Attach** 数据库命令（请参阅 [移动 Analysis Services 数据库](../../analysis-services/multidimensional-models/move-an-analysis-services-database.md)、 [附加和分离 Analysis Services 数据库](../../analysis-services/multidimensional-models/attach-and-detach-analysis-services-databases.md)）。  
  
## <a name="see-also"></a>请参阅  
 <xref:Microsoft.AnalysisServices.Database.DbStorageLocation%2A>   
 [附加和分离 Analysis Services 数据库](../../analysis-services/multidimensional-models/attach-and-detach-analysis-services-databases.md)   
 [移动 Analysis Services 数据库](../../analysis-services/multidimensional-models/move-an-analysis-services-database.md)   
 [DbStorageLocation 元素](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/dbstoragelocation-element)   
 [Create 元素 (XMLA)](https://docs.microsoft.com/bi-reference/xmla/xml-elements-commands/create-element-xmla)   
 [附加元素](https://docs.microsoft.com/bi-reference/xmla/xml-elements-commands/attach-element)   
 [Synchronize 元素 (XMLA)](https://docs.microsoft.com/bi-reference/xmla/xml-elements-commands/synchronize-element-xmla)  
  
  
