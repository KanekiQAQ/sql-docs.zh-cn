---
title: 'Isscommandwithparameters:: Setparameterproperties (OLE DB) |Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- ISSCommandWithParameters::SetParameterProperties (OLE DB)
apitype: COM
helpviewer_keywords:
- SetParameterProperties method
ms.assetid: 4cd0281a-a2a0-43df-8e46-eb478b64cb4b
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0e4ec4a160920d8490cb7578de76656a1253adac
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68050958"
---
# <a name="isscommandwithparameterssetparameterproperties-ole-db"></a>ISSCommandWithParameters::SetParameterProperties (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  按照序号基于每个参数设置参数属性，或者通过指定 SSPARAMPROPS 结构数组来设置大容量参数属性。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT SetParameterProperties(  
      DB_UPARAMS cParams,   
      SSPARAMPROPS rgParamProperties[]);  
```  
  
## <a name="arguments"></a>参数  
 *cParams*[in]  
 rgParamProperties 数组中 SSPARAMPROPS 结构的数量  。 如果此数值为零，ISSCommandWithParameters::SetParameterProperties 将删除可能已经为该命令中的任何参数指定的所有属性  。  
  
 *rgParamProperties*[in]  
 要设置的 SSPARAMPROPS 结构数组。  
  
## <a name="return-code-values"></a>返回代码值  
 ISSCommandWithParameters::SetParameterProperties 方法返回的错误代码与核心 OLE DB ICommandProperties::SetProperties 方法返回的错误代码相同   。  
  
## <a name="remarks"></a>备注  
 允许按照序号基于每个参数使用此方法设置参数属性，或者在已经从属性数组生成 SSPARAMPROPS 之后使用单个 ISSCommandWithParameters::SetParameterProperties 调用设置参数属性  。  
  
 在调用 ISSCommandWithParameters::SetParameterProperties 方法之前，必须首先调用 SetParameterInfo 方法   。 调用 `SetParameterProperties(0, NULL)` 可清除所有指定的参数属性，而调用 `SetParameterInfo(0,NULL,NULL)` 则会清除所有参数信息（包括可能与某个参数相关的任何属性）。  
  
 调用**isscommandwithparameters:: Setparameterproperties**来指定参数的类型不是属性 DBTYPE_XML 或 DBTYPE_UDT 将返回 DB_E_ERRORSOCCURRED 或 DB_S_ERRORSOCCURRED，并*dwStatus*使用 dbpropstatus_notset 标记该参数的 SSPARAMPROPS 中包含的所有 Dbprop 字段。 应当遍历 SSPARAMPROPS 中包含的每个 DBPROPSET 的 DBPROP 数组，以检测 DB_E_ERRORSOCCURRED 或 DB_S_ERRORSOCCURRED 引用了哪些参数。  
  
 如果调用 ISSCommandWithParameters::SetParameterProperties 以指定尚未使用 SetParameterInfo 设置其参数信息的参数的属性，访问接口将返回 E_UNEXPECTED 并显示以下错误消息   ：  
  
 在没有先调用 SetParameterInfo 方法的情况下，不能为指定参数调用 SetParameterProperties 方法。 在设置参数属性之前，必须首先设置参数信息。  
  
 如果对 ISSCommandWithParameters::SetParameterProperties 的调用包含某些参数，其中已设置了参数信息，而另一些参数未设置参数信息，则返回的 SSPARAMPROPS 属性集的 DBPROPSET 中的 dwStatus 属性将带有 DBSTATUS_NOTSET  。  
  
 SSPARAMPROPS 结构的定义如下所示：  
  
 `struct SSPARAMPROPS {`  
  
 `DBORDINAL iOrdinal;`  
  
 `ULONG cPropertySets;`  
  
 `DBPROPSET *rgPropertySets;`  
  
 `};`  
  
 从数据库引擎中的改进[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]允许 isscommandwithparameters:: Setparameterproperties 若要获取的预期的结果更准确描述。 这些更准确的结果可能不同于 isscommandwithparameters:: Setparameterproperties 的早期版本中返回的值[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 有关详细信息，请参阅[元数据发现](../../relational-databases/native-client/features/metadata-discovery.md)。  
  
|成员|描述|  
|------------|-----------------|  
|iOrdinal |所传递参数的序号。|  
|*cPropertySets*|rgPropertySets 中 DBPROPSET 结构的数量  。|  
|*rgPropertySets*|指向内存中将返回 DBPROPSET 结构数组的位置的指针。|  
  
## <a name="see-also"></a>请参阅  
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
