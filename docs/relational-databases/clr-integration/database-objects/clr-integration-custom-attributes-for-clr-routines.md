---
title: CLR 例程的自定义属性 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- routines [CLR integration]
- SqlFacet attribute
- SqlTrigger attribute
- SqlProcedure attribute
- custom attributes [CLR integration]
- SqlUserDefinedAggregate attribute
- attributes [CLR integration]
- SqlMethod attribute
- SqlFunction attribute
- common language runtime [SQL Server], attributes
- SqlUserDefinedTypeAttribute attribute
ms.assetid: 95069d22-b05d-4670-b053-15ee2a664e33
author: rothja
ms.author: jroth
ms.openlocfilehash: 1c1105579dc5da82a66fb101e559e1fb96feac1f
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68138633"
---
# <a name="clr-integration-custom-attributes-for-clr-routines"></a>CLR 例程的 CLR 集成自定义属性
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  列出的特性可以应用于公共语言运行时 (CLR) 例程、 用户定义类型和在中注册的用户定义聚合[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。 如果未应用此属性，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将采用默认值。 列出的属性中定义**Microsoft.SqlServer.Server**命名空间。  
  
## <a name="the-sqluserdefinedaggregate-attribute"></a>SqlUserDefinedAggregate 属性  
 **SqlUserDefinedAggregate**属性指示方法应注册为用户定义的聚合。 必须使用此属性注释每个用户定义聚合。  
  
 有关详细信息，请参阅[SqlUserDefinedAggregateAttribute](https://go.microsoft.com/fwlink/?LinkId=124626)。  
  
## <a name="the-sqlfunction-attribute"></a>SqlFunction 属性  
 **SqlFunction**属性指示方法应注册为具有相应函数属性集的函数。  
  
 有关详细信息，请参阅[SqlFunctionAttribute](https://go.microsoft.com/fwlink/?LinkId=128019)。  
  
## <a name="the-sqlfacet-attribute"></a>SqlFacet 属性  
 **SqlFacet**属性用于返回有关用户定义类型 (UDT) 表达式的返回类型的信息。  
  
 有关详细信息，请参阅[SqlFacetAttribute](https://go.microsoft.com/fwlink/?LinkId=128020)。  
  
## <a name="the-sqlprocedure-attribute"></a>SqlProcedure 属性  
 **SqlProcedure**属性指示方法应注册为存储过程。 此属性仅由 Visual Studio 用于将指定的方法自动注册为存储过程；[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 不使用此属性。  
  
 有关详细信息，请参阅[SqlProcedureAttribute](https://go.microsoft.com/fwlink/?LinkId=128021)。  
  
## <a name="the-sqltrigger-attribute"></a>SqlTrigger 属性  
 **SqlTrigger**属性指示方法应注册为触发器。  
  
 有关详细信息，请参阅[SqlTriggerContext](https://go.microsoft.com/fwlink/?LinkId=128022)并[SqlTriggerAttribute](https://go.microsoft.com/fwlink/?LinkId=203898)。  
  
## <a name="the-sqluserdefinedtypeattribute"></a>SqlUserDefinedTypeAttribute  
 可将 SqlUserDefinedTypeAttribute 应用于程序集中的类定义。 此属性会使 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 创建绑定到具有此自定义属性的类定义的用户定义类型。  
  
 有关详细信息，请参阅[SqlUserDefinedTypeAttribute](https://go.microsoft.com/fwlink/?LinkId=128024)。  
  
## <a name="the-sqlmethod-attribute"></a>SqlMethod 属性  
 **SqlMethod**特性用于指示一种方法的确定性和数据访问属性或 udt 属性。  
  
 有关详细信息，请参阅[SqlMethodAttribute](https://go.microsoft.com/fwlink/?LinkId=128025)。  
  
## <a name="see-also"></a>请参阅  
 [CLR 用户定义聚合](../../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-aggregates.md)   
 [CLR 用户定义函数](../../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions.md)   
 [CLR 用户定义类型](../../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)   
 [CLR 存储过程](https://msdn.microsoft.com/library/bbdd51b2-a9b4-4916-ba6f-7957ac6c3f33)   
 [CLR 触发器](https://msdn.microsoft.com/library/302a4e4a-3172-42b6-9cc0-4a971ab49c1c)  
  
  
