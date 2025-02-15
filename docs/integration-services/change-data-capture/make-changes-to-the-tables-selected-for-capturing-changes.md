---
title: 更改用于捕获更改的表 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- makChanToTab
ms.assetid: a309f82a-c6ef-464d-a6ef-df555bfb609a
author: janinezhang
ms.author: janinez
ms.openlocfilehash: d363df2409f51186a3b459e2e75398c6f5e1dd4d
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68101701"
---
# <a name="make-changes-to-the-tables-selected-for-capturing-changes"></a>对为捕获更改选择的表进行更改

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


  在此对话框中，您可以从要捕获变更的所选表中选择特定列。 您还可以编辑 **“安全角色”** 和 **“捕获实例”** 信息。  
  
 您可以在此对话框中对为捕获变更而选择的表进行以下更改。  
  
 **对在 CDC 实例中包含的列进行更改：**  
  
 执行下列一种或两种操作：  
  
-   选中要包括的任何其他列旁边的复选框。  
  
-   取消选中不想再包括的任何列旁边的复选框。  
  
 **更改特定列的数据类型**：  
  
 您可以为特定列选择不同的数据类型。 您只能将数据类型更改为与原始数据类型兼容的数据类型。  
  
 若要更改数据类型，请在 **“数据类型”** 列中单击，然后选择不同的数据类型。 只有与原始数据类型兼容的数据类型可用。  
  
> [!NOTE]  
>  如果没有其他数据类型可供选择，则下拉列表将不可用。  
  
 **更改安全角色**  
  
 键入一个新名称或在 **“安全角色”** 字段中编辑安全访问控制角色的名称。  
  
 **更改或添加捕获实例**  
  
 在 **“捕获实例”** 字段中，输入捕获实例的名称。  
  
## <a name="see-also"></a>另请参阅  
 [如何创建 SQL Server 更改数据库实例](../../integration-services/change-data-capture/how-to-create-the-sql-server-change-database-instance.md)   
 [选择 Oracle 表和列](../../integration-services/change-data-capture/select-oracle-tables-and-columns.md)  
  
  
