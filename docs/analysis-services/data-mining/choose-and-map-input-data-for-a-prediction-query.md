---
title: 选择和映射输入的数据为预测查询 |Microsoft Docs
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0781c35dfe7bcc1ea99be3d68fcbb839d5f9374b
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62724854"
---
# <a name="choose-and-map-input-data-for-a-prediction-query"></a>为预测查询选择和映射输入数据
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  在您根据挖掘模型创建预测时，通常通过向模型馈送新数据来创建预测。 （时序模型是个例外情况，它只能基于历史数据进行预测。）若要向模型提供新数据，您必须确保数据可作为数据源视图的一部分提供。 如果您事先知道哪些数据将用于预测，则可以在用于创建模型的数据源视图中包括这些数据。 否则，您可能需要创建一个新的数据源视图。 有关详细信息，请参阅 [多维模型中的数据源视图](../../analysis-services/multidimensional-models/data-source-views-in-multidimensional-models.md)。  
  
 有时候，您所需的数据可能包含在一对多联接中的多个表内。 用于关联模型或顺序分析和聚类分析模型的数据便是这种情况，它们将使用链接到包含产品或事务详细信息的嵌套表的事例表。 如果您的模型使用事例嵌套表结构，则您用于预测的数据也必须具有事例嵌套表结构。  
  
> [!WARNING]  
>  不能添加位于其他数据源视图中的新列或映射列。 选择的数据源视图必须包含预测查询所需的所有列。  
  
 在您确定了包含将用于预测的数据的表后，必须将外部数据中的列映射到挖掘模型中的列。 例如，如果您的模型基于人口统计信息和调查响应预测客户购买行为，则您的输入数据应包含与模型中的数据通常对应的信息。 您无需对每个单列都具有匹配的数据，但可以匹配的列越多，预测效果就越好。 如果您尝试映射具有不同数据类型的列，则系统可能会显示错误消息。 在此情况下，您可以在数据源视图中定义命名计算，以便将新的列数据强制转换或转换为模型所需的数据类型。 有关详细信息，请参阅[在数据源视图中定义命名计算 (Analysis Services)](../../analysis-services/multidimensional-models/define-named-calculations-in-a-data-source-view-analysis-services.md)。  
  
 在您选择要用于预测的数据时，所选数据源中的某些列可能会基于名称相似性和匹配的数据类型，自动映射到挖掘模型列。 可以使用 **“挖掘模型预测”** 中的 **“修改映射”** 对话框更改映射的列、删除不合适的映射或为现有列创建新映射。 “挖掘模型预测”  设计图面还支持连接的拖放编辑。  
  
-   若要创建新连接，只需在“挖掘模型”  表中选择一列，然后将该列拖到“选择输入表”  表中的对应列上。  
  
-   若要删除某个连接，请选中该连接线，然后按 Delete 键。  
  
 下面的过程说明如何使用 **“指定嵌套联接”** 对话框修改在事例表和嵌套表之间创建的、用作预测查询输入的联接。  
  
### <a name="select-an-input-table"></a>选择输入表  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 的数据挖掘设计器中，单击“挖掘准确性图表”  选项卡的“选择输入表”  表上的“选择事例表”  。  
  
     此时将打开 **“选择表”** 对话框，在该对话框中，您可以选择包含查询所基于的数据的表。  
  
2.  在 **“选择表”** 对话框中，从 **“数据源”** 列表中选择数据源。  
  
3.  在“表/视图名称”  下，选择包含希望用于测试模型的数据的表。  
  
4.  单击“确定”  。  
  
     挖掘结构中的列将自动映射到输入表中相同名称的列。  
  
### <a name="change-the-way-that-input-data-is-mapped-to-the-model"></a>更改输入数据映射到模型的方式  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]的数据挖掘设计器中，选择 **“挖掘模型预测”** 选项卡。  
  
2.  在 **“挖掘模型”** 菜单中，选择 **“修改连接”** 。  
  
     此时，将打开 **“修改映射”** 对话框。 在此对话框中， **“挖掘模型列”** 列中列出了所选挖掘结构中的列。 “表列”  列中列出了在“选择输入表”  对话框中选择的外部数据源中的列。 外部数据源中的列将映射到挖掘模型中的列。  
  
3.  在 **“表列”** 下，选择与要映射到的挖掘模型列相对应的行。  
  
4.  从外部数据源可用列的列表中选择一个新列。 选择列表中的空白项以删除列映射。  
  
5.  单击“确定”  。  
  
     设计器中将显示新的列映射。  
  
### <a name="remove-a-relationship-between-input-tables"></a>删除各输入表之间的关系  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 的数据挖掘设计器中“挖掘模型预测”  选项卡的“选择输入表”  表上，单击“修改联接”  。  
  
     此时，将打开 **“指定嵌套联接”** 对话框。  
  
2.  选择关系。  
  
3.  单击 **“删除关系”** 。  
  
4.  单击“确定”  。  
  
     这样便可删除事例表和嵌套表之间的关系。  
  
### <a name="create-a-new-relationship-between-input-tables"></a>创建各输入表之间新的关系  
  
1.  在数据挖掘设计器中“挖掘模型预测”  选项卡的“选择输入表”  表上，单击“修改联接”  。  
  
     此时，将打开 **“指定嵌套联接”** 对话框。  
  
2.  单击 **“添加关系”** 。  
  
     将打开 **“创建关系”** 对话框。  
  
3.  在 **“源列”** 中选择嵌套表的键。  
  
4.  在 **“目标列”** 中选择事例表的键。  
  
5.  在 **“创建关系”** 对话框中，单击 **“确定”** 。  
  
6.  在 **“指定嵌套联接”** 对话框中，单击 **“确定”** 。  
  
     这样便在事例表和嵌套表之间创建了新的关系。  
  
### <a name="add-a-nested-table-to-the-input-tables-of-a-prediction-query"></a>将嵌套表添加到预测查询的输入表  
  
1.  在数据挖掘设计器的 **“挖掘模型预测”** 选项卡中，单击 **“选择事例表”** 打开 **“选择表”** 对话框。  
  
    > [!NOTE]  
    >  如果尚未指定事例表，则不能将嵌套表添加到输入中。 使用嵌套表要求您正用于预测的挖掘模型也使用嵌套表。  
  
2.  在 **“选择表”** 对话框中，从 **“数据源”** 列表中选择一个数据源，然后在数据源视图中选择包含事例数据的表。 [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  单击 **“选择嵌套表”** 打开 **“选择表”** 对话框。  
  
4.  在 **“选择表”** 对话框中，从 **“数据源”** 列表中选择一个数据源，然后在数据源视图中选择包含嵌套数据的表。 [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     如果已经存在关系，则挖掘模型中的列将自动映射到输入表中具有相同名称的列。 通过单击 **“修改联接”** 以打开 **“创建关系”** 对话框，可以修改嵌套表与事例表之间的关系。  
  
## <a name="see-also"></a>请参阅  
 [预测查询（数据挖掘）](../../analysis-services/data-mining/prediction-queries-data-mining.md)  
  
  
