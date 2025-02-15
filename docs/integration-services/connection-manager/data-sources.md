---
title: 数据源 | Microsoft Docs
ms.custom: ''
ms.date: 08/27/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- data sources [Integration Services], about data sources
ms.assetid: 7ac81612-9822-470f-8d0f-a1dc96142fe3
author: janinezhang
ms.author: janinez
ms.openlocfilehash: 89b6660f5a989cec23b5a92c09dabb36f891e1c2
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67968115"
---
# <a name="data-sources"></a>“数据源”

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 包括一个可在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包中使用的设计时对象：数据源。  
  
 数据源对象是对连接的引用，它至少包括一个连接字符串和一个数据源标识符。 数据源对象还可以包括其他元数据，如说明、名称、用户名和密码。  
  
> **注意**：只能将数据源添加到配置为使用包部署模型的项目。 如果将项目配置为使用项目部署模型，您使用在项目级别创建的连接管理器来共享连接，而不使用数据源。  
>   
>  有关部署模型的详细信息，请参阅 [Deployment of Projects and Packages](../packages/deploy-integration-services-ssis-projects-and-packages.md)。 有关将项目转换为项目部署模型的详细信息，请参阅 [Deploy Projects to Integration Services Server](https://msdn.microsoft.com/library/hh231102.aspx)。  
  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包中使用数据源的好处如下：  
  
-   数据源具有项目作用域，这意味着在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目中创建的数据源对该项目的所有包都可用。 数据源可以在一次定义后，由多个包中的连接管理器引用。  
  
-   数据源提供数据源对象与其包引用之间的同步。 如果数据源和引用它的包在同一个项目中，在数据源更改时就会自动更新数据源引用的连接字符串属性。  
  
## <a name="reference-data-sources"></a>引用数据源  
 要将数据源对象添加到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目，请在“解决方案资源管理器”  中右键单击“数据源”  文件夹，然后单击“新建数据源”  。 该项将添加到 **“数据源”** 文件夹。 如果要使用在其他项目中创建的数据源对象，则必须首先将其添加到该项目。  
  
 将引用数据源对象的连接管理器添加到包之后，便可以在该包中使用数据源对象。 可以在生成包控制流和数据流之前将其添加到包中，也可以作为构造控制流和数据流的一个步骤来添加。  
  
 数据源对象表示对数据源的简单连接，通过它可以访问它所引用的数据存储区中的对象。 例如，连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]AdventureWorks 示例数据库的数据源对象包括来自该数据库的所有 60 个表。  
  
 在数据源和引用它的连接管理器之间没有依赖关系。 即使数据源不再是项目的一部分，包仍然有效，因为有关该数据源的信息（例如其连接类型和连接字符串）已包括在包定义中。  
  
  
