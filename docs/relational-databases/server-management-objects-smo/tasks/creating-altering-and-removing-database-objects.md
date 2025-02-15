---
title: 使用数据库对象 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- database objects [SMO]
- objects [SMO]
ms.assetid: 702fd63d-8734-4a02-872e-aecfb037c787
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 91d8e3c5381ec1df38ea172a3bd68e23db8cfc72
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68092777"
---
# <a name="creating-altering-and-removing-database-objects"></a>创建、更改和删除数据库对象
[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

  创建 SMO 对象的各个阶段如下：  
  
1.  创建对象的实例。  
  
2.  设置对象属性。  
  
3.  创建子对象的实例。  
  
4.  设置子对象属性。  
  
5.  创建对象。  

[!INCLUDE[freshInclude](../../../includes/paragraph-content/fresh-note-steps-feedback.md)]

 当在 SMO 应用程序中创建 SMO 对象的实例时，它们不存在的实例上[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]直到**创建**颁发方法。 但是，不需要发出**创建**方法为每个单独的对象。 如果对象具有一组子对象，仅此父对象必须运行**创建**方法。 例如，表定义要求它至少包含一个列才能存在。 并且，列不能独立于表存在。 表与其列之间存在相互依赖的关系。  
  
 使用 <xref:Microsoft.SqlServer.Management.Dmf.Policy.Alter%2A> 方法可以更改对象。 对对象的若干更改（例如，向对象的集合之一添加子对象或更改属性值）将组成一批共同运行。 **Alter**方法可减少网络流量并提高整体性能。  
  
 **Drop**语句用于删除对象并将所有所需最初创建该对象及其相互依赖子对象。  
  
## <a name="see-also"></a>请参阅  
 [SMO 对象模型](../../../relational-databases/server-management-objects-smo/smo-object-model.md)  
  
  
