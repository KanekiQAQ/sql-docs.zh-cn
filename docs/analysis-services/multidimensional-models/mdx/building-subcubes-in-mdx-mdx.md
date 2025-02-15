---
title: 构建 MDX (MDX) 中的子多维数据集 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7bf6396ebe7cfe18aa7d1005d39095a35713e10b
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68208801"
---
# <a name="building-subcubes-in-mdx-mdx"></a>在 MDX 中生成子多维数据集 (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  子多维数据集是多维数据集的子集，用于表示基础数据的筛选视图。 通过将多维数据集限制为一个子多维数据集，可以改善查询的性能。  
  
 若要定义子多维数据集，请按照本主题所述使用 [CREATE SUBCUBE](../../../mdx/mdx-data-definition-create-subcube.md) 语句。  
  
## <a name="create-subcube-syntax"></a>CREATE SUBCUBE 语法  
 使用下列语法创建子多维数据集：  
  
```  
CREATE SUBCUBE Subcube_Identifier AS Subcube_Expression  
```  
  
 CREATE SUBCUBE 语法非常简单。 *Subcube_Identifier* 参数标识子多维数据集所基于的多维数据集。 *Subcube_Expression* 参数选择将成为子多维数据集的多维数据集中部分。  
  
 创建子多维数据集后，该子多维数据集将成为所有 MDX 查询的上下文，直到会话结束或运行 [DROP SUBCUBE](../../../mdx/mdx-data-definition-drop-subcube.md) 语句。  
  
### <a name="what-a-subcube-contains"></a>子多维数据集包含的内容  
 尽管 CREATE SUBCUBE 语句使用起来非常简单，但是该语句本身并不明确显示构成子多维数据集的所有成员。 在定义子多维数据集时，需遵循下列规则：  
  
-   如果包含某个层次结构的“(全部)”  成员，则包含该层次结构的每个成员。  
  
-   如果包含任一成员，则包含该成员的祖先和后代。  
  
-   如果包含某个级别的每个成员，则包含该层次结构的所有成员。 如果其他层次结构的成员不与此级别的成员共存，它们将被排除（例如，不对称的层次结构，如不包含客户的城市）。  
  
-   子多维数据集将始终包含多维数据集的每个“(全部)”  成员。  
  
 另外，子多维数据集中的聚合数据将进行可视求和。 例如，一个子多维数据集包含 `USA`、 `WA`和 `OR`。 `USA` 的聚合值将是 `{WA,OR}` 的和，因为该子多维数据集只定义了 `WA` 和 `OR` 这两个州。 所有其他州将被忽略。  
  
 另外，显式引用子多维数据集外的单元将返回在整个多维数据集的上下文中计算的单元值。 例如，创建一个限制为本年度的子多维数据集。 然后使用 [ParallelPeriod](../../../mdx/parallelperiod-mdx.md) 函数将本年度与去年相比较。 即使上一年的值之外的子多维数据集，将返回值之间的差异。  
  
 最后，如果原始上下文没有被覆盖，在嵌套 select 语句中计算的集函数将在嵌套 select 语句的上下文中计算。 如果上下文被覆盖，集函数将在整个多维数据集的上下文中计算。  
  
## <a name="create-subcube-example"></a>CREATE SUBCUBE 示例  
 下面的示例创建的子多维数据集将 Budget 多维数据集仅限制到帐户 4200 和 4300：  
  
 `CREATE SUBCUBE Budget AS SELECT {[Account].[Account].&[4200], [Account].[Account].&[4300] } ON 0 FROM Budget`  
  
 为会话创建子多维数据集后，后续的查询将针对该子多维数据集，而不是整个多维数据集。 例如，运行下列查询。 此查询将只返回帐户 4200 和 4300 中的成员。  
  
 `SELECT [Account].[Account].Members ON 0, Measures.Members ON 1 FROM Budget`  
  
## <a name="see-also"></a>请参阅  
 [在查询中建立多维数据集上下文 (MDX)](../../../analysis-services/multidimensional-models/mdx/establishing-cube-context-in-a-query-mdx.md)   
 [MDX 查询基础知识 (Analysis Services)](../../../analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services.md)  
  
  
