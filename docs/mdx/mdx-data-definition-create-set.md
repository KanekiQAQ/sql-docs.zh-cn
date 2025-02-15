---
title: CREATE SET 语句 (MDX) |Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 91b040a1c625387ed25855d3c3eb26955088f5c8
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68038332"
---
# <a name="mdx-data-definition---create-set"></a>MDX 数据定义 - CREATE SET


  为当前多维数据集创建具有会话作用域的命名集。  
  
## <a name="syntax"></a>语法  
  
```  
  
CREATE [SESSION] [ STATIC | DYNAMIC ] [HIDDEN] SET   
   CURRENTCUBE | Cube_Name  
      .Set_Name AS 'Set_Expression'  
      [,Property_Name = Property_Value, ...n]  
```  
  
## <a name="arguments"></a>参数  
 *Cube_Name*  
 提供的多维数据集名称的有效字符串表达式。  
  
 *Set_Name*  
 为要创建的命名集提供名称的有效字符串表达式。  
  
 *Set_Expression*  
 返回集的有效多维表达式 (MDX)。  
  
 *Property_Name*  
 提供集属性名称的有效字符串。  
  
 *Property_Value*  
 定义集属性值的有效标量表达式。  
  
## <a name="remarks"></a>备注  
 命名集是为重用而创建的维度成员集（或者是用于定义集的表达式）。 例如，使用命名集可以定义由销售额排在前十名的商店组成的维度成员集。 此组可以静态，或通过所示的函数定义[TopCount](../mdx/topcount-mdx.md)。 此命名集然后使用需要的前 10 个存储组的位置。  
  
 CREATE SET 语句创建命名的集，仍可在整个会话，并因此，可在多个查询在会话中。 有关详细信息，请参阅[创建会话作用域计算成员&#40;MDX&#41;](../analysis-services/multidimensional-models/mdx/mdx-calculated-members-session-scoped-calculated-members.md)。  
  
 也可以定义用于单个查询的命名集。 若要定义这种集，请在 SELECT 语句中使用 WITH 子句。 有关 WITH 子句的详细信息，请参阅[命名集创建查询作用域&#40;MDX&#41;](../analysis-services/multidimensional-models/mdx/mdx-named-sets-creating-query-scoped-named-sets.md)。  
  
 *Set_Expression*子句可以包含支持 MDX 语法的任何函数。 如果使用 CREATE SET 语句创建集时没有指定 SESSION 子句，则该集的作用域为会话。 使用 WITH 子句创建具有查询作用域的集。  
  
 指定当前连接的多维数据集以外的多维数据集将导致错误。 因此，应使用 CURRENTCUBE 来代替多维数据集名称，以表示当前的多维数据集。  
  
## <a name="scope"></a>范围  
 用户定义的集的作用域可以是下表所列的任意一个作用域。  
  
 查询作用域  
 集的可见性和生存期被限制在查询中。 集是在单个查询中定义的。 查询作用域将覆盖会话作用域。 有关详细信息，请参阅[命名集创建查询作用域&#40;MDX&#41;](../analysis-services/multidimensional-models/mdx/mdx-named-sets-creating-query-scoped-named-sets.md)。  
  
 会话作用域  
 集的可见性和生存期被限制在创建该集的会话中。 （生存期小于会话持续时间如果在发出 DROP SET 语句，对集。）CREATE SET 语句创建会话作用域的集。 使用 WITH 子句创建具有查询作用域的集。  
  
### <a name="example"></a>示例  
 以下示例创建名为 Core Products 的集。 SELECT 查询并显示调用新创建的集。 必须先执行 CREATE SET 语句才可以执行 SELECT 查询 - 这两个语句不能在同一批中执行。  
  
```  
CREATE SET [Adventure Works].[Core Products] AS '{[Product].[Category].[Bikes]}'  
  
SELECT [Core Products] ON 0  
  FROM [Adventure Works]  
```  
  
## <a name="set-evaluation"></a>集的求取  
 集的求取可以定义不同的发生方式；可以定义为仅在集创建时发生一次，也可以定义为在每次使用集时发生。  
  
 STATIC  
 指示只在执行 CREATE SET 语句时，求取一次该集。  
  
 DYNAMIC  
 指示每次在查询中使用该集时，都进行求取。  
  
## <a name="set-visibility"></a>集的可见性  
 对于查询多维数据集的其他用户来说，集可以是可见的，也可以是不可见的。  
  
 HIDDEN  
 指定该集对查询多维数据集的用户不可见。  
  
## <a name="standard-properties"></a>标准属性  
 每个集都有一组默认属性。 当客户端应用程序连接到[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]的默认属性是受支持，或可用于获得支持，根据管理员的选择。  
  
|属性标识符|含义|  
|-------------------------|-------------|  
|CAPTION|客户端应用程序用作集标题的字符串。|  
|DISPLAY_FOLDER|标识客户端应用程序用来显示集的显示文件夹路径的字符串。 文件夹级别的分隔符由客户端应用程序定义。 有关工具和客户端提供的[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]，反斜杠 (\\) 作为级别分隔符。 若要为已定义的集提供多个显示文件夹，则使用分号 (;) 来分隔文件夹。|  
  
## <a name="see-also"></a>请参阅  
 [DROP SET 语句&#40;MDX&#41;](../mdx/mdx-data-definition-drop-set.md)   
 [MDX 数据定义语句&#40;MDX&#41;](../mdx/mdx-data-definition-statements-mdx.md)  
  
  
