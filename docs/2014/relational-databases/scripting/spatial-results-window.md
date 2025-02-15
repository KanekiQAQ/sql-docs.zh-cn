---
title: “空间结果”窗口 | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: database-engine
ms.topic: conceptual
ms.assetid: c2d5a477-6496-4d01-adee-7322ebdfadf3
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 606ac348ce4ee7bed65a7bcbe6d7ebbbd0a7f87d
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "66063801"
---
# <a name="spatial-results-window"></a>“空间结果”窗口
  **“空间结果”** 窗口提供了用于查看空间数据的可视化映射工具。 若要查看空间结果，查询结果中必须包括一个包含几何图形或地域数据的空间列。  
  
> [!NOTE]  
>  仅当结果返回到 **“结果”** 窗口中的某个网格时， **“空间结果”** 窗口才可用。 如果指定将结果作为文本返回，则此窗口不可用。  
  
## <a name="options"></a>选项  
 **选择空间列**  
 在查询结果中，从空间列指定要查看的空间列。 一次只能选择一个列。  
  
 **选择标签列**  
 在查询结果中，从返回的列指定要标记空间数据的非空间列。 一次只能选择一个列。  
  
 如果某个查询中仅返回了点实例，则此选项不可用。  
  
 **选择投影**  
 显示以下四个投影之一中的地理数据：Equirectangular、Mercator、Robinson 或 Bonne。  
  
 此选项不适用于几何图形数据。  
  
 **缩放**  
 按指数比例调整映射显示。  
  
 **显示网格线**  
 打开或关闭坐标网格线。  
  
 对于多边形形状，仅当形状大到能够容纳标签文本时，才显示标签。 若要显示小形状的标签，请调整缩放比例。  
  
> [!NOTE]  
>  不能标记点实例。  
  
## <a name="see-also"></a>请参阅  
 [在对象资源管理器中查看空间数据](view-spatial-data-in-object-explorer.md)   
 [空间数据 (SQL Server)](../spatial/spatial-data-sql-server.md)   
 [数据库引擎查询编辑器 (SQL Server Management Studio)](database-engine-query-editor-sql-server-management-studio.md)   
 [查询和文本编辑器 (SQL Server Management Studio)](query-and-text-editors-sql-server-management-studio.md)  
  
  
