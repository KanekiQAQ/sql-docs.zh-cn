---
title: 将域或复合域附加到引用数据 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
f1_keywords:
- sql13.dqs.dm.refdata.f1
- sql13.dqs.dm.refcatalog.f1
ms.assetid: 36af981c-d0d0-4dc6-afe5-bbb3c97845dc
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 8171d552d850ec929f7aba5b55b6f0e2c6ae7265
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67935638"
---
# <a name="attach-domain-or-composite-domain-to-reference-data"></a>将域或复合域附加到引用数据

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  本主题介绍如何将数据质量知识库中的域/复合域附加到 Microsoft Azure 市场中的引用数据服务，以便针对高质量引用数据生成知识。 每个引用数据服务包含一个架构（数据列）。 在将域或复合域附加到引用数据服务后，必须将此附加域或所附加的复合域内的各个域映射到引用数据服务架构中的相应列。 通过将复合域附加到引用数据服务，您可以只将一个域附加到引用数据服务，然后将复合域内的各域映射到引用数据服务架构中的相应列。  

> [!IMPORTANT]
> 本文提及以前可从 Azure DataMarket 获取的第三方参考数据服务。 DataMarket 和数据服务（包括 Melissa 地址数据）在 2016 年 12 月 31 日之后已不再使用。 因此，无法继续使用 DataMarket 中的指定服务运行本文中的示例。 但仍可使用第三方参考数据提供商提供的在线参考数据服务。

> [!WARNING]  
>  当将域映射到引用数据服务架构中的列时，附加到引用数据服务的复合域会出现在域下拉列表中。 不要将复合域映射到引用数据服务中的列；只需将复合域内的各个域映射到引用数据服务架构中的相应列。 否则，将导致错误。  
  
 如果您应该选择使用某一引用数据服务，则引用数据服务架构可能具有一个必须映射到适当域的必填列。 引用数据架构中的必填列使用“(M)”与列名称区分开来。 例如，“AddressLine”是“Melissa Data - Address Data”中的必填架构列，而“CompanyName”是“Digital Trowel Inc. - Us companies and professional data for SQL users”中的必填架构列     。  
  
 在本主题中，我们将创建 4 个域：Address Line、City、State 和 Zip。在复合域 Address Verification 之下，将该复合域附加到 Melissa Data - Address Check 引用数据服务，然后将复合域内的各个域映射到引用数据服务架构中的相应列       。  
  
## <a name="before-you-begin"></a>开始之前  
  
###  <a name="Prerequisites"></a> 先决条件  
 您必须配置了 [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) 后才能使用引用数据服务。 请参阅[将 DQS 配置为使用引用数据](../data-quality-services/configure-dqs-to-use-reference-data.md)。  
  
###  <a name="Security"></a> 安全性  
  
#### <a name="permissions"></a>权限  
 您必须具有针对 DQS_MAIN 数据库的 dqs_kb_editor 角色才能将域映射到引用数据。  
  
##  <a name="Map"></a> 将域从 Melissa Data 映射到引用数据  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)][运行 Data Quality Client 应用程序](../data-quality-services/run-the-data-quality-client-application.md)。  
  
2.  在 [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] 主屏幕中，在 **“知识库管理”** 下，单击 **“新建知识库”** 。  
  
3.  在 **“新建知识库”** 屏幕上，为新的知识库键入名称，单击 **“域管理”** 活动，然后单击 **“创建”** 。  
  
4.  在 **“域管理”** 屏幕中，单击 **“创建域”** 图标以创建一个域。 创建以下 4 个域：Address Line、City、State 和 Zip     。  
  
5.  单击 **“创建复合域”** 图标以便创建一个复合域。 在 **“创建复合域”** 对话框中，在 **“复合域名称”** 框中键入 **Address Verification** ，并且在该复合域中包括在步骤 3 中创建的所有域。 单击 **“确定”** 。  
  
6.  在左侧的 **“域”** 窗格中，通过单击 **Address Verification**选择该复合域，然后单击右侧的 **“引用数据”** 选项卡。  
  
7.  单击 **“浏览”** 图标。  
  
8.  在 **“联机引用数据提供程序目录”** 对话框中：  
  
    1.  在“DataMarket Data Quality Services”下，选中“Melissa Data - Address”复选框   。  
  
    2.  将 Melissa Data - Address Check 引用数据服务的列映射到适当的域（Address Line、City、State 和 Zip）。 您通过在 **“RDS 架构”** 列中选择某一引用数据服务列，然后在 **“域”** 列中选择适当的域，映射这些列。 若要在表中添加更多的行，请单击 **“添加架构项”** 图标。  
  
    3.  单击 **“确定”** 保存更改并关闭 **“联机引用数据提供程序目录”** 对话框。  
  
         ![“联机引用数据提供程序目录”对话框](../data-quality-services/media/dqs-onlinereferencedataproviderscatalog.gif "Online Reference Data Providers Catalog dialog box")  
  
        > [!NOTE]  
        >  -   在 **“联机引用数据提供程序目录”** 对话框中， **DataMarket Data Quality Services** 节点将显示已在 Microsoft Azure 市场中订阅的所有引用数据服务提供程序。 如果您已在 DQS 中配置了直接联机第三方引用数据服务提供程序，则它们将显示在称作 **“第三方直接联机提供程序”** 的另一个节点下（现在未提供，因为没有在 DQS 中配置直接联机第三方引用数据服务提供程序）。  
  
9. 您将返回到 **“引用数据”** 选项卡。在“提供程序设置”区域中，根据需要更改下面框中的值：   
  
    -   **自动更正阈值**：将自动通过其置信度高于此域值的引用数据服务进行更正。 以相应百分比值的小数表示形式输入一个值。 例如，对于 90% 输入 0.9。  
  
    -   **建议的候选项**：要通过引用数据服务显示的建议候选项数量。  
  
    -   **最低置信度**：将忽略来自其置信度低于该值的引用数据服务的建议。 以相应百分比值的小数表示形式输入一个值。 例如，对于 60% 输入 0.6。  
  
10. 单击 **“完成”** 将发布知识库。 在知识库成功发布后，将会出现一条确认消息。  
  
 您可以将此知识库用于数据质量项目中的清理活动，以便基于 Melissa Data 通过 Microsoft Azure 市场提供的知识标准化和清理您的源数据中的美国地址。  
  
##  <a name="FollowUp"></a> 跟进：在将域映射到引用数据后  
 创建一个数据质量项目，然后通过将其与本主题中创建的知识库进行比较，对包含美国地址的源数据运行清理活动。 请参阅[使用引用数据（外部）知识清理数据](../data-quality-services/cleanse-data-using-reference-data-external-knowledge.md)。  
  
## <a name="see-also"></a>请参阅  
 [DQS 中的 Reference Data Services](../data-quality-services/reference-data-services-in-dqs.md)   
 [数据清理](../data-quality-services/data-cleansing.md)  
  
  
