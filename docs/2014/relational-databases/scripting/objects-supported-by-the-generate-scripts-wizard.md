---
title: 生成脚本向导支持的对象 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: database-engine
ms.topic: conceptual
ms.assetid: 071eb2cb-f073-41ca-9f4d-11d3b8803495
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 58e3aa77c7c21b89917c23c80f42330442863a18
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "66063929"
---
# <a name="objects-supported-by-the-generate-scripts-wizard"></a>生成脚本向导支持的对象
  “生成和发布脚本向导”支持 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]所支持的对象的一个子集。  
  
## <a name="supported-objects"></a>支持的对象  
 下表列出“生成和发布脚本向导”支持的可发布对象。  
  
||||||  
|-|-|-|-|-|  
|应用程序角色|数据库角色|架构|用户定义聚合|视图<sup>1</sup>|  
|Assembly|DEFAULT 约束|存储过程<sup>1</sup>|用户定义数据类型|XML 架构集合|  
|CHECK 约束|全文目录|同义词|用户定义函数||  
|CLR （公共语言运行时） 存储过程<sup>1</sup>|索引|表|用户定义表||  
|CLR 用户定义函数|规则|用户<sup>2</sup>|用户定义类型||  
  
 <sup>1</sup>发布但不加密。  
  
 <sup>2</sup>作为角色发布数据库中存在的任何非系统用户。  
  
  
