---
title: 编辑 Oracle 数据库属性 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- oraProp
ms.assetid: 58dc99f1-ee6b-4508-bb66-2bc589611ff7
author: janinezhang
ms.author: janinez
ms.openlocfilehash: 6bc7a37a2989d50cda1722aef066107ed35f09f0
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68011514"
---
# <a name="edit-the-oracle-database-properties"></a>编辑 Oracle 数据库属性

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


  使用属性编辑器中的“Oracle”选项卡可对在新建实例向导的“创建 CDC 数据库”页中提供的说明进行更改，以及对 Oracle 日志挖掘数据库连接信息进行更改。  
  
 下表对 **“Oracle”** 选项卡中的信息进行了说明。  
  
 **名称**  
 在新建实例向导的“创建 CDC 数据库”页中输入的 CDC 实例的名称。 此字段是只读的，不能编辑此信息。  
  
 **Description**  
 您可以编辑新实例的说明或者添加说明（如果您在创建 CDC 实例时未添加说明）。  
  
 **Oracle 连接字符串**  
 正使用 Oracle 数据库的计算机的 Oracle 连接字符串。 此字段是只读的，不能编辑此信息。 其原因在于，对连接字符串的某些更改可能会将 Oracle CDC 实例完全指向其他 Oracle 数据库，从而破坏在 **cdc.xdbcdc_config** 表中存储的 CDC 实例状态。 如果需要较小的更改，您可以使用 SQL Server Management Studio 直接在配置表中更改连接字符串。  
  
 **Oracle 日志挖掘身份验证**  
 若要为包含日志挖掘器的 Oracle 数据库输入身份验证凭据，请在“身份验证”  下选择以下选项之一：  
  
-   **Windows 身份验证**：选择此选项可使用当前的 Windows 域凭据。 只有当 Oracle 数据库配置为使用 Windows 身份验证时，才可以使用此选项。  
  
-   **Oracle 身份验证**：如果选择此选项，则必须在连接到的 Oracle 数据库中为用户键入“用户名”和“密码”   。  
  
 您可以在查看器中查看 Oracle 数据库属性。 在使用查看器时，信息是只读的。 查看器还在表中包括捕获列的列表。 有关如何访问查看器的信息，请参阅 [How to Manage a CDC Instance](../../integration-services/change-data-capture/how-to-manage-a-cdc-instance.md)。  
  
## <a name="see-also"></a>另请参阅  
 [如何从 CDC 设计器控制台管理 CDC 服务](../../integration-services/change-data-capture/how-to-manage-a-cdc-service-from-the-cdc-designer-console.md)   
 [连接到 Oracle 源数据库](../../integration-services/change-data-capture/connect-to-an-oracle-source-database.md)   
 [连接到 Oracle](../../integration-services/change-data-capture/connect-to-oracle.md)  
  
  
