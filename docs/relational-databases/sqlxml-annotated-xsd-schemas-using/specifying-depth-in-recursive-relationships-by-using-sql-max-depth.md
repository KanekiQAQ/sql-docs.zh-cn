---
title: 指定递归关系中使用 sql:max-深度 |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- max-depth annotation
- XPath queries [SQLXML], recursive relationships
- depth in recursive relationships [SQLXML]
- annotated XSD schemas, recursive relationships
- relationships [SQLXML], recursive relationships
- self joins
- recursive relationships [SQLXML]
- recursion [SQLXML]
- sql:max-depth
- recursive joins [SQLXML]
ms.assetid: 0ffdd57d-dc30-44d9-a8a0-f21cadedb327
author: MightyPen
ms.author: genemi
ms.reviewer: ''
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3a7385f5152c911d3c1d0985ea9c3a105e738067
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68066952"
---
# <a name="specifying-depth-in-recursive-relationships-by-using-sqlmax-depth"></a>使用 sql:max-depth 指定递归关系中的深度
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  在关系数据库中，当表涉及与其自身的关系时，将它称为递归关系。 例如，在监督者和被监督者关系中，存储雇员记录的表涉及与其自身的关系。 在这种情况下，雇员表在关系的一端扮演监督者的角色，而在另一端则扮演被监督者的角色。  
  
 映射架构可以包含元素及其祖先属于相同类型的递归关系。  
  
## <a name="example-a"></a>示例 A  
 考虑以下表：  
  
```  
Emp (EmployeeID, FirstName, LastName, ReportsTo)  
```  
  
 在该表中，ReportsTo 列存储经理的雇员 ID。  
  
 假设您想生成一个雇员 XML 层次结构，其中经理雇员位于层次结构的顶部，而向该经理报告的雇员出现在对应的层次结构中，如以下 XML 片段示例所示。 此代码段显示的内容是*递归树*雇员 1。  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
  <Emp FirstName="Nancy" EmployeeID="1" LastName="Devolio">  
     <Emp FirstName="Andrew" EmployeeID="2" LastName="Fuller" />   
     <Emp FirstName="Janet" EmployeeID="3" LastName="Leverling">  
        <Emp FirstName="Margaret" EmployeeID="4" LastName="Peacock">  
          <Emp FirstName="Steven" EmployeeID="5" LastName="Devolio">  
...  
...  
</root>  
```  
  
 在该片段中，雇员 5 向雇员 4 报告，雇员 4 向雇员 3 报告，雇员 3 和 2 向雇员 1 报告。  
  
 若要产生该结果，可以使用以下 XSD 架构，并指定针对它的 XPath 查询。 此架构描述 **\<Emp >** 元素的类型为 EmployeeType 组成 **\<Emp >** 相同类型，为 EmployeeType 的子元素。 此即属于递归关系（元素和其祖先属于相同类型）。 此外，架构使用 **\<sql: relationship >** 来描述监督者和被监督者之间的父-子关系。 请注意，在这 **\<sql: relationship >** ，Emp 是父和子表。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"  
                                  parent="Emp"  
                                  parent-key="EmployeeID"  
                                  child="Emp"  
                                  child-key="ReportsTo" />  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp" type="EmployeeType"   
                          sql:relation="Emp"   
                          sql:key-fields="EmployeeID"   
                          sql:limit-field="ReportsTo" />  
  <xsd:complexType name="EmployeeType">  
    <xsd:sequence>  
      <xsd:element name="Emp" type="EmployeeType"   
                              sql:relation="Emp"   
                              sql:key-fields="EmployeeID"  
                              sql:relationship="SupervisorSupervisee"  
                              sql:max-depth="6" />  
    </xsd:sequence>   
    <xsd:attribute name="EmployeeID" type="xsd:ID" />  
    <xsd:attribute name="FirstName" type="xsd:string"/>  
    <xsd:attribute name="LastName" type="xsd:string"/>  
  </xsd:complexType>  
</xsd:schema>  
```  
  
 因为该关系是递归关系，所以需要以某种方式指定在架构中的递归深度。 否则，结果将是无穷递归的（雇员向雇员报告而该雇员又向雇员报告，以此类推）。 **Sql:max-深度**批注，可以指定递归深度。 在此特定示例中，若要为指定值**sql:max-深度**，您必须知道管理层次结构在公司中出现的深度。  
  
> [!NOTE]  
>  该架构指定**sql:-字段**批注，但未指定**sql:-值**批注。 这会使得所生成的层次结构的顶级节点仅为那些不向任何人报告的雇员。 （ReportsTo 为 NULL。）指定**sql:-字段**且不指定**sql:-值**（默认为 NULL） 批注来实现此目的。 如果希望包括每个可能的报告生成的 XML 树 （表中每个雇员的报告树），请删除**sql:-字段**从架构的批注。  
  
> [!NOTE]  
>  以下过程使用 tempdb 数据库。  
  
#### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>若要测试示例 XPath 查询根据架构  
  
1.  在虚拟根目录所指向的 tempdb 数据库中创建称为 Emp 的示例表。  
  
    ```  
    USE tempdb  
    CREATE TABLE Emp (  
           EmployeeID int primary key,   
           FirstName  varchar(20),   
           LastName   varchar(20),   
           ReportsTo int)  
    ```  
  
2.  添加以下示例数据：  
  
    ```  
    INSERT INTO Emp values (1, 'Nancy', 'Devolio',NULL)  
    INSERT INTO Emp values (2, 'Andrew', 'Fuller',1)  
    INSERT INTO Emp values (3, 'Janet', 'Leverling',1)  
    INSERT INTO Emp values (4, 'Margaret', 'Peacock',3)  
    INSERT INTO Emp values (5, 'Steven', 'Devolio',4)  
    INSERT INTO Emp values (6, 'Nancy', 'Buchanan',5)  
    INSERT INTO Emp values (7, 'Michael', 'Suyama',6)  
    ```  
  
3.  复制上面的架构代码，并将它粘贴到文本文件中。 将该文件另存为 maxDepth.xml。  
  
4.  复制以下模板，并将它粘贴到文本文件中。 该文件另存为 maxDepthT.xml，保存在与 maxDepth.xml 相同的目录中。 模板中的查询将返回 Emp 表中的所有雇员。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="maxDepth.xml">  
        /Emp  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     为映射架构 (maxDepth.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\MyDir\maxDepth.xml"  
    ```  
  
5.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。 有关详细信息，请参阅[使用 ADO 执行 SQLXML 4.0 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

 下面是结果：  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
  <Emp FirstName="Nancy" EmployeeID="1" LastName="Devolio">  
  <Emp FirstName="Andrew" EmployeeID="2" LastName="Fuller" />   
    <Emp FirstName="Janet" EmployeeID="3" LastName="Leverling">  
      <Emp FirstName="Margaret" EmployeeID="4" LastName="Peacock">  
        <Emp FirstName="Steven" EmployeeID="5" LastName="Devolio">  
          <Emp FirstName="Nancy" EmployeeID="6" LastName="Buchanan">  
            <Emp FirstName="Michael" EmployeeID="7" LastName="Suyama" />   
          </Emp>  
        </Emp>  
      </Emp>  
    </Emp>  
  </Emp>  
</root>  
```  
  
> [!NOTE]  
>  若要产生不同深度的结果中的层次结构，更改的值**sql:max-深度**架构中的批注和每次更改之后再次执行该模板。  
  
 在前面的架构，所有 **\<Emp >** 元素都有完全相同的特性集 (**EmployeeID**， **FirstName**，和**LastName**)。 下面的架构已经过稍许修改返回额外**ReportsTo**属性的所有 **\<Emp >** 向经理报告的元素。  
  
 例如，该 XML 片段显示雇员 1 的下级：  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
<Emp FirstName="Nancy" EmployeeID="1" LastName="Devolio">  
  <Emp FirstName="Andrew" EmployeeID="2"   
       ReportsTo="1" LastName="Fuller" />   
  <Emp FirstName="Janet" EmployeeID="3"   
       ReportsTo="1" LastName="Leverling">  
...  
...  
```  
  
 这是修改后的架构：  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:documentation>  
      Customer-Order-Order Details Schema  
      Copyright 2000 Microsoft. All rights reserved.  
    </xsd:documentation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"   
                  parent="Emp"  
                  parent-key="EmployeeID"  
                  child="Emp"  
                  child-key="ReportsTo" />  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp"   
                   type="EmpType"   
                   sql:relation="Emp"   
                   sql:key-fields="EmployeeID"   
                   sql:limit-field="ReportsTo" />  
  <xsd:complexType name="EmpType">  
    <xsd:sequence>  
       <xsd:element name="Emp"   
                    type="EmpType"   
                    sql:relation="Emp"   
                    sql:key-fields="EmployeeID"  
                    sql:relationship="SupervisorSupervisee"  
                    sql:max-depth="6"/>  
    </xsd:sequence>   
    <xsd:attribute name="EmployeeID" type="xsd:int" />  
    <xsd:attribute name="FirstName" type="xsd:string"/>  
    <xsd:attribute name="LastName" type="xsd:string"/>  
    <xsd:attribute name="ReportsTo" type="xsd:int" />  
  </xsd:complexType>  
</xsd:schema>  
```  
  
## <a name="sqlmax-depth-annotation"></a>sql:max-depth 批注  
 在由递归关系组成的架构中，必须在架构中显式指定递归的深度。 若要成功生成可返回所请求的结果的相应 FOR XML EXPLICIT 查询，则必须这样做。  
  
 使用**sql:max-深度**架构在架构中描述的递归关系中指定递归的深度中的批注。 值**sql:max-深度**批注是一个正整数 （1 到 50 个），该值指示递归数：如果值为 1 使递归停止于该元素为其**sql:max-深度**指定批注; 值为 2 使递归停止于的元素的下一级**sql:max-深度**指定;等等。  
  
> [!NOTE]  
>  在基础实现中，针对映射架构指定 XPath 查询转换为 SELECT...FOR XML EXPLICIT 查询。 该查询需要您指定一个有限的递归深度。 更高版本为指定的值**sql:max-深度**、 更大的 FOR XML EXPLICIT 查询的生成。 这可能会使检索时间变长。  
  
> [!NOTE]  
>  Updategram 和 XML 大容量加载将忽略最大深度批注。 这意味着，不管为最大深度指定了什么值，都将发生递归更新或插入。  
  
## <a name="specifying-sqlmax-depth-on-complex-elements"></a>在复杂元素上指定 sql:max-depth  
 **Sql:max-深度**可以在任何复杂内容元素上指定批注。  
  
### <a name="recursive-elements"></a>递归元素  
 如果**sql:max-深度**父元素和在递归关系中，子元素上指定**sql:max-深度**父级上指定的批注优先。 例如，在以下架构中， **sql:max-深度**父和子雇员元素上指定批注。 在这种情况下， **sql:max-深度 = 4**上指定 **\<Emp >** 父元素 （扮演监督程序的角色），将优先。 **Sql:max-深度**指定在子 **\<Emp >** 元素 （扮演被监督者角色） 将被忽略。  
  
#### <a name="example-b"></a>示例 B  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"  
                                  parent="Emp"  
                                  parent-key="EmployeeID"  
                                  child="Emp"  
                                  child-key="ReportsTo" />  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp" type="EmployeeType"   
                          sql:relation="Emp"   
                          sql:key-fields="EmployeeID"   
                          sql:limit-field="ReportsTo"   
                          sql:max-depth="3" />  
  <xsd:complexType name="EmployeeType">  
    <xsd:sequence>  
      <xsd:element name="Emp" type="EmployeeType"   
                              sql:relation="Emp"   
                              sql:key-fields="EmployeeID"  
                              sql:relationship="SupervisorSupervisee"  
                              sql:max-depth="2" />  
    </xsd:sequence>   
    <xsd:attribute name="EmployeeID" type="xsd:ID" />  
    <xsd:attribute name="FirstName" type="xsd:string"/>  
    <xsd:attribute name="LastName" type="xsd:string"/>  
  </xsd:complexType>  
</xsd:schema>  
```  
  
 若要测试该架构，请为本主题前面的示例 A 提供的步骤。  
  
### <a name="nonrecursive-elements"></a>非递归元素  
 如果**sql:max-深度**不会导致任何递归的架构中的元素上指定批注，它将被忽略。 在以下架构中，  **\<Emp >** 元素组成 **\<常量 >** 子元素，而又有 **\<Emp >** 子元素。  
  
 在此架构中， **sql:max-深度**上指定的批注 **\<常量 >** 元素被忽略，因为之间没有递归 **\<Emp>** 父并 **\<常量 >** 子元素。 但之间的递归 **\<Emp >** 祖先和 **\<Emp >** 子。 该架构指定**sql:max-深度**上的批注。 因此， **sql:max-深度**祖先指定的批注 ( **\<Emp >** 监督者角色中) 将优先。  
  
#### <a name="example-c"></a>示例 C  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"   
                  parent="Emp"   
                  child="Emp"   
                  parent-key="EmployeeID"   
                  child-key="ReportsTo"/>  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp"   
               sql:relation="Emp"   
               type="EmpType"  
               sql:limit-field="ReportsTo"  
               sql:max-depth="1" />  
    <xsd:complexType name="EmpType" >  
      <xsd:sequence>  
       <xsd:element name="Constant"   
                    sql:is-constant="1"   
                    sql:max-depth="20" >  
         <xsd:complexType >  
           <xsd:sequence>  
            <xsd:element name="Emp"   
                         sql:relation="Emp" type="EmpType"  
                         sql:relationship="SupervisorSupervisee"   
                         sql:max-depth="3" />  
         </xsd:sequence>  
         </xsd:complexType>  
         </xsd:element>  
      </xsd:sequence>  
      <xsd:attribute name="EmployeeID" type="xsd:int" />  
    </xsd:complexType>  
</xsd:schema>  
```  
  
 若要测试该架构，请执行为本主题前面的示例 A 提供的步骤。  
  
## <a name="complex-types-derived-by-restriction"></a>由限制派生的复杂类型  
 如果你具有的复杂类型派生 **\<限制 >** ，相应的基复杂类型的元素不能指定**sql:max-深度**批注。 在这些情况下， **sql:max-深度**批注可以添加到派生类型的元素。  
  
 另一方面，如果你具有的复杂类型派生 **\<扩展 >** ，可以指定相应的基本复杂类型的元素**sql:max-深度**批注。  
  
 例如，以下 XSD 架构生成一个错误，因为**sql:max-深度**基类型上指定批注。 由派生的类型上不支持此批注 **\<限制 >** 从另一种类型。 若要解决此问题，必须更改架构，并指定**sql:max-深度**派生类型中的元素上的批注。  
  
#### <a name="example-d"></a>示例 D  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:msdata="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:complexType name="CustomerBaseType">   
    <xsd:sequence>  
       <xsd:element name="CID" msdata:field="CustomerID" />  
       <xsd:element name="CompanyName"/>  
       <xsd:element name="Customers" msdata:max-depth="3">  
         <xsd:annotation>  
           <xsd:appinfo>  
             <msdata:relationship  
                     parent="Customers"  
                     parent-key="CustomerID"  
                     child-key="CustomerID"  
                     child="Customers" />  
           </xsd:appinfo>  
         </xsd:annotation>  
       </xsd:element>  
    </xsd:sequence>  
  </xsd:complexType>  
  <xsd:element name="Customers" type="CustomerType"/>  
  <xsd:complexType name="CustomerType">  
    <xsd:complexContent>  
       <xsd:restriction base="CustomerBaseType">  
          <xsd:sequence>  
            <xsd:element name="CID"   
                         type="xsd:string"/>  
            <xsd:element name="CompanyName"   
                         type="xsd:string"  
                         msdata:field="CName" />  
            <xsd:element name="Customers"   
                         type="CustomerType" />  
          </xsd:sequence>  
       </xsd:restriction>  
    </xsd:complexContent>  
  </xsd:complexType>  
</xsd:schema>   
```  
  
 在架构中， **sql:max-深度**上指定**CustomerBaseType**复杂类型。 该架构还指定 **\<客户 >** 类型的元素**CustomerType**，它派生自**CustomerBaseType**。 这样的架构上指定的 XPath 查询将生成错误，因为**sql:max-深度**不支持在 restriction 基类型中定义的元素上。  
  
## <a name="schemas-with-a-deep-hierarchy"></a>具有深层次结构的架构  
 您可能有一个包含深层次结构的架构，在该层次结构中，元素包含子元素，而该子元素又包含另一个子元素，以此类推。 如果**sql:max-深度**批注此类架构中指定生成包含超过 500 级 （顶级元素在第 1 级、 其的子级别为 2，等等） 的层次结构的 XML 文档，则返回错误。  
  
  
