---
title: CompoundCurve | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: ae357f9b-e3e2-4cdf-af02-012acda2e466
author: MladjoA
ms.author: mlandzic
manager: craigg
ms.openlocfilehash: e234b06917d77e68577e72fbdc7bca1ad033cef8
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "66014344"
---
# <a name="compoundcurve"></a>CompoundCurve
  `CompoundCurve` 是几何图形或地域类型的零个或多个连续 `CircularString` 或 `LineString` 实例的集合。  
  
> [!IMPORTANT]  
>  有关详细的说明和示例的新空间功能在此版本中，包括`CompoundCurve`子类型，请下载白皮书[SQL Server 2012 中的新空间功能](https://go.microsoft.com/fwlink/?LinkId=226407)。  
  
 可以实例化空的 `CompoundCurve` 实例，但要使 `CompoundCurve` 有效，它必须满足以下条件：  
  
1.  它必须至少包含一个 `CircularString` 或 `LineString` 实例。  
  
2.  `CircularString` 或 `LineString` 实例的序列必须是连续的。  
  
 如果`CompoundCurve`包含多个序列`CircularString`和`LineString`实例，结束端点除外的最后一个实例的每个实例必须是序列中的下一个实例的起始端点。 这意味着，如果在序列中前一个实例的结束点是 (4 3 7 2)，则该序列中的下一个实例的起始点必须是 (4 3 7 2)。 请注意，该点的 Z（标高）和 M（度量）值也必须相同。 如果这两个点之间存在差异，则将引发 `System.FormatException` 。 `CircularString` 中的点不一定必须有 Z 值或 M 值。 如果未向前一个实例的结束点提供 Z 值或 M 值，下一个实例的起始点就不能包含 Z 值或 M 值。 如果前一个序列的结束点是 (4 3)，则后一个序列的起始点必须是 (4 3)；它不能是 (4 3 7 2）。 `CompoundCurve` 实例中的所有点要么不得有 Z 值，要么其 Z 值必须相同。  
  
## <a name="compoundcurve-instances"></a>CompoundCurve 实例  
 下图显示了有效的 `CompoundCurve` 类型。  
  
 ![](../../database-engine/media/f278742e-b861-4555-8b51-3d972b7602bf.png "f278742e-b861-4555-8b51-3d972b7602bf")  
  
### <a name="accepted-instances"></a>接受的实例  
 如果 `CompoundCurve` 实例是空实例或者它满足以下条件，则接受该实例。  
  
1.  `CompoundCurve` 实例包含的所有实例都是接受的圆弧线段实例。 有关接受的圆弧线段实例的详细信息，请参阅 [LineString](linestring.md) 和 [CircularString](circularstring.md)。  
  
2.  `CompoundCurve` 实例中的所有圆弧线段都连接在一起。 每个后续圆弧线段上的第一个点与前一个圆弧线段上的最后一个点相同。  
  
    > [!NOTE]  
    >  Z 和 M 坐标也要相同。 因此，所有四个坐标 X、Y、Z 和 M 必须相同。  
  
3.  所有包含的实例都不是空实例。  
  
 下面的示例显示接受的 `CompoundCurve` 实例。  
  
```  
DECLARE @g1 geometry = 'COMPOUNDCURVE EMPTY';  
DECLARE @g2 geometry = 'COMPOUNDCURVE(CIRCULARSTRING(1 0, 0 1, -1 0), (-1 0, 2 0))';  
```  
  
 下面的示例显示未被接受的 `CompoundCurve` 实例。 这些实例引发 `System.FormatException`。  
  
```  
DECLARE @g1 geometry = 'COMPOUNDCURVE(CIRCULARSTRING EMPTY)';  
DECLARE @g2 geometry = 'COMPOUNDCURVE(CIRCULARSTRING(1 0, 0 1, -1 0), (1 0, 2 0))';  
```  
  
### <a name="valid-instances"></a>有效实例  
 如果满足以下条件，则 `CompoundCurve` 实例有效。  
  
1.  `CompoundCurve` 实例是接受的实例。  
  
2.  `CompoundCurve` 实例包含的所有圆弧线段实例都是有效实例。  
  
 下面的示例显示有效的 `CompoundCurve` 实例。  
  
```  
DECLARE @g1 geometry = 'COMPOUNDCURVE EMPTY';  
DECLARE @g2 geometry = 'COMPOUNDCURVE(CIRCULARSTRING(1 0, 0 1, -1 0), (-1 0, 2 0))';  
DECLARE @g3 geometry = 'COMPOUNDCURVE(CIRCULARSTRING(1 1, 1 1, 1 1), (1 1, 3 5, 5 4))';  
SELECT @g1.STIsValid(), @g2.STIsValid(), @g3.STIsValid();  
  
```  
  
 `@g3` 有效，因为 `CircularString` 实例有效。 有关详细信息的有效性`CircularString`实例，请参阅[CircularString](circularstring.md)。  
  
 下面的示例显示无效的 `CompoundCurve` 实例。  
  
```  
DECLARE @g1 geometry = 'COMPOUNDCURVE(CIRCULARSTRING(1 1, 1 1, 1 1), (1 1, 3 5, 5 4, 3 5))';  
DECLARE @g2 geometry = 'COMPOUNDCURVE((1 1, 1 1))';  
DECLARE @g3 geometry = 'COMPOUNDCURVE(CIRCULARSTRING(1 1, 2 3, 1 1))';  
SELECT @g1.STIsValid(), @g2.STIsValid(), @g3.STIsValid();  
```  
  
 `@g1` 无效，因为第二个实例是无效的 LineString 实例。 `@g2` 无效，因为 `LineString` 实例无效。 `@g3` 无效，因为 `CircularString` 实例无效。 有关详细信息有效`CircularString`并`LineString`实例，请参阅[CircularString](circularstring.md)并[LineString](linestring.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-instantiating-a-geometry-instance-with-an-empty-compooundcurve"></a>A. 使用空的 CompoundCurve 实例化一个几何图形实例  
 下面的示例演示如何创建空的 `CompoundCurve` 实例：  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('COMPOUNDCURVE EMPTY');  
```  
  
### <a name="b-declaring-and-instantiating-a-geometry-instance-using-a-compoundcurve-in-the-same-statement"></a>B. 在同一语句中使用 CompoundCurve 声明和实例化一个几何图形实例  
 下面的示例演示如何在同一语句中使用 `geometry` 声明和初始化 `CompoundCurve`实例：  
  
```sql  
DECLARE @g geometry = 'COMPOUNDCURVE ((2 2, 0 0),CIRCULARSTRING (0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0))';  
```  
  
### <a name="c-instantiating-a-geography-instance-with-a-compoundcurve"></a>C. 使用 CompoundCurve 实例化一个地域实例  
 下面的示例演示如何使用 `CompoundCurve` 声明和初始化 `geography` 实例：  
  
```sql  
DECLARE @g geography = 'COMPOUNDCURVE(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
```  
  
### <a name="d-storing-a-square-in-a-compoundcurve-instance"></a>D. 将一个正方形存储在 CompoundCurve 实例中  
 下面的示例通过两种不同方式使用 `CompoundCurve` 实例存储一个正方形。  
  
```sql  
DECLARE @g1 geometry, @g2 geometry;  
SET @g1 = geometry::Parse('COMPOUNDCURVE((1 1, 1 3), (1 3, 3 3),(3 3, 3 1), (3 1, 1 1))');  
SET @g2 = geometry::Parse('COMPOUNDCURVE((1 1, 1 3, 3 3, 3 1, 1 1))');  
SELECT @g1.STLength(), @g2.STLength();  
```  
  
 `@g1` 和 `@g2` 的长度相同。 请注意，在该示例中 `CompoundCurve` 实例可以存储一个或多个 `LineString` 实例。  
  
### <a name="e-instantiating-a-geometry-instance-using-a-compoundcurve-with-multiple-circularstrings"></a>E. 使用包含多个 CircularString 的 CompoundCurve 实例化一个几何图形实例  
 下面的示例演示如何使用两个不同的 `CircularString` 实例初始化 `CompoundCurve`。  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(0 2, 2 0, 4 2), CIRCULARSTRING(4 2, 2 4, 0 2))');  
SELECT @g.STLength();  
```  
  
 此示例将产生以下输出：12.566370...相当于 4 批处理。 该示例中的 `CompoundCurve` 实例存储一个半径为 2 的圆。 前面的两个代码示例不一定非要使用 `CompoundCurve`。 对于第一个示例， `LineString` 实例将更为简单，对于第二个示例， `CircularString` 实例将更为简单。 然而，下一个示例显示在何种情况下 `CompoundCurve` 提供更好的替代方法。  
  
### <a name="f-using-a-compoundcurve-to-store-a-semicircle"></a>F. 使用 CompoundCurve 存储一个半圆  
 下面的示例使用 `CompoundCurve` 实例存储一个半圆。  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(0 2, 2 0, 4 2), (4 2, 0 2))');  
SELECT @g.STLength();  
```  
  
### <a name="g-storing-multiple-circularstring-and-linestring-instances-in-a-compoundcurve"></a>G. 将多个 CircularString 和 LineString 实例存储在一个 CompoundCurve 中  
 下面的示例演示如何使用 `CircularString` 存储多个 `LineString` 和 `CompoundCurve`实例。  
  
```sql  
DECLARE @g geometry  
SET @g = geometry::Parse('COMPOUNDCURVE((3 5, 3 3), CIRCULARSTRING(3 3, 5 1, 7 3), (7 3, 7 5), CIRCULARSTRING(7 5, 5 7, 3 5))');  
SELECT @g.STLength();  
```  
  
### <a name="h-storing-instances-with-z-and-m-values"></a>H. 存储具有 Z 值和 M 值的实例  
 下面的示例演示如何使用 `CompoundCurve` 实例存储具有 Z 值和 M 值的 `CircularString` 和 `LineString` 实例的序列。  
  
```sql  
SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(7 5 4 2, 5 7 4 2, 3 5 4 2), (3 5 4 2, 8 7 4 2))');  
```  
  
### <a name="i-illustrating-why-circularstring-instances-must-be-explicitly-declared"></a>I. 说明为什么必须显式声明 CircularString 实例  
 下面的示例说明了为什么必须显式声明 `CircularString` 实例。 程序员试图将一个圆形存储在 `CompoundCurve` 实例中。  
  
```sql  
DECLARE @g1 geometry;    
DECLARE @g2 geometry;  
SET @g1 = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(0 2, 2 0, 4 2), (4 2, 2 4, 0 2))');  
SELECT 'Circle One', @g1.STLength() AS Perimeter;  -- gives an inaccurate amount  
SET @g2 = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(0 2, 2 0, 4 2), CIRCULARSTRING(4 2, 2 4, 0 2))');  
SELECT 'Circle Two', @g2.STLength() AS Perimeter;  -- now we get an accurate amount  
```  
  
 输出如下所示：  
  
```  
Circle One11.940039...  
Circle Two12.566370...  
```  
  
 在外围网络两个圆为大约 4???，这是周长的实际值。 但是，圆形 1 的周长很不准确。 圆形 1 的 `CompoundCurve` 实例存储一个圆弧线段 (ABC) 和两条直线段（CD、DA)。 `CompoundCurve` 实例必须存储两个圆弧线段（ABC、CDA）才可以定义一个圆形。 `LineString` 实例定义圆形 1 的 `CompoundCurve` 实例中的第二组点（4 2、2 4、0 2）。 必须在 `CircularString` 内部显式声明 `CompoundCurve`实例。  
  
## <a name="see-also"></a>请参阅  
 [STIsValid（geometry 数据类型）](/sql/t-sql/spatial-geometry/stisvalid-geometry-data-type)   
 [STLength（geometry 数据类型）](/sql/t-sql/spatial-geometry/stlength-geometry-data-type)   
 [STStartPoint（geometry 数据类型）](/sql/t-sql/spatial-geometry/ststartpoint-geometry-data-type)   
 [STEndpoint（geometry 数据类型）](/sql/t-sql/spatial-geometry/stendpoint-geometry-data-type)   
 [LineString](linestring.md)   
 [CircularString](circularstring.md)   
 [空间数据类型概述](spatial-data-types-overview.md)   
 [点](point.md)  
  
  
