---
title: Azure Blob 上载任务 | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpblobuptask.f1
- sql14.dts.designer.afpblobuptask.f1
ms.assetid: 6ea068b0-4cd8-45b5-b89d-09b8f25040c0
author: janinezhang
ms.author: janinez
ms.openlocfilehash: bdc915d459144cd2c745f0ca97df55380f819138
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67947395"
---
# <a name="azure-blob-upload-task"></a>Azure blob 上载任务

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


“Azure blob 上传任务”  可让 SSIS 包将文件上传到 Azure blob 存储区中。
    
若要添加“Azure blob 上传任务”  ，请将其拖放到“SSIS 设计器”中，双击或右键单击它，然后单击“编辑”  调出下面的“Azure blob 上传任务编辑器”  对话框。  
  
 “Azure blob 上载任务”  是[适用于 Azure 的 SQL Server Integration Services (SSIS) 功能包](../../integration-services/azure-feature-pack-for-integration-services-ssis.md)组件。
  
 下表介绍了此对话框中的各个字段。  

|**字段**|**Description**|  
|---|---|  
|AzureStorageConnection|选择一个现有的 Azure 存储连接管理器或创建一个新的连接管理器，用于引用指向在其中托管 blob 文件的 Azure 存储帐户。|  
|BlobContainer|指定包含上载文件作为 blob 的 blob 容器的名称。|  
|BlobDirectory|指定将上载的文件作为块 blob 存储的 blob 目录。 Blob 目录是虚拟的层次结构。 如果 blob 已存在，它会被替换。|  
|LocalDirectory|指定包含要上载的文件的本地目录。|  
|SearchRecursively|指定是否在子目录中以递归方式搜索。|  
|FileName|指定用于选择具有指定名称模式的文件的名称筛选器。 例如，`MySheet*.xls\*` 包括 `MySheet001.xls` 和 `MySheetABC.xlsx` 等文件。|  
|TimeRangeFrom/TimeRangeTo|指定时间范围筛选器。 将包括在 **TimeRangeFrom** 之后以及 **TimeRangeTo** 之前修改的文件。|  
