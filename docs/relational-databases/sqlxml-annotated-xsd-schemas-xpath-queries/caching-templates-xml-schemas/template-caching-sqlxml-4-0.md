---
title: 模板缓存 (SQLXML 4.0) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- registry keys [SQLXML]
- cache [SQLXML]
- templates [SQLXML], caching
ms.assetid: 73e151c6-b24e-4422-a116-51e0846bc6f5
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dd36af88aa2cb01c0c2bce05c7c023122eea87cb
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68093261"
---
# <a name="template-caching-sqlxml-40"></a>模板缓存 (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  模板缓存极大地提高了性能。 如果设置模板缓存，该模板将在首次执行时保留在内存中。 这样可提高后续执行该模板的性能。  
  
 在注册表中添加以下项可以设置模板缓存大小：  
  
```  
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SQLXML4\TemplateCacheSize  
```  
  
> [!CAUTION]  
>  [!INCLUDE[ssNoteRegistry](../../../includes/ssnoteregistry-md.md)]  
  
 模板大小的设置应以可用内存和正在使用的模板数为基础。 默认值为**TemplateCacheSize**大小为 31。 如果模板访问看起来较慢，您可以增加缓存大小；如果内存较少，则可以降低缓存大小。  
  
 为了提高性能，建议您设置**TemplateCacheSize**大于您通常使用的模板数。 如果**TemlateCacheSize**小于您拥有的数量，性能将随着模板增加的数量。 **TemplateCacheSize**可以设置为最多为 128。  
  
 每次使用缓存的模板时，将检查模板文件的修改时间以确定是否需要刷新该模板。 其原因在于磁盘副本新于缓存副本。  
  
> [!NOTE]  
>  不能缓存模板参数和命令属性。  
  
## <a name="see-also"></a>请参阅  
 [架构缓存&#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/schema-caching-sqlxml-4-0.md)   
 [XSL 缓存&#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/xsl-caching-sqlxml-4-0.md)  
  
  
