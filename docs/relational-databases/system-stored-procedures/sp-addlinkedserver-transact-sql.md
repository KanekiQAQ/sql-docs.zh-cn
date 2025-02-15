---
title: sp_addlinkedserver (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 09/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_addlinkedserver_TSQL
- sp_addlinkedserver
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addlinkedserver
ms.assetid: fed3adb0-4c15-4a1a-8acd-1b184aff558f
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 8df629aa707e5c0f63ef5bdcb9c77d7f8f2e2aca
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68072684"
---
# <a name="spaddlinkedserver-transact-sql"></a>sp_addlinkedserver (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  创建链接服务器。 链接服务器让用户可以对 OLE DB 数据源进行分布式异类查询。 使用创建链接的服务器后**sp_addlinkedserver**、 分布式查询可以对该服务器运行。 如果链接服务器定义为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例，则可执行远程存储过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addlinkedserver [ @server= ] 'server' [ , [ @srvproduct= ] 'product_name' ]   
     [ , [ @provider= ] 'provider_name' ]  
     [ , [ @datasrc= ] 'data_source' ]   
     [ , [ @location= ] 'location' ]   
     [ , [ @provstr= ] 'provider_string' ]   
     [ , [ @catalog= ] 'catalog' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @server = ] 'server'` 是要创建的链接服务器的名称。 *server* 的数据类型为 **sysname**，无默认值。  
  
`[ @srvproduct = ] 'product_name'` 是要添加为链接服务器的 OLE DB 数据源的产品名称。 *product_name*是**nvarchar (** 128 **)** ，默认值为 NULL。 如果**SQL Server**， *provider_name*， *data_source*，*位置*， *provider_string*，和*目录*无需指定。  
  
`[ @provider = ] 'provider_name'` 是对应于此数据源的 OLE DB 访问接口的唯一编程标识符 (PROGID)。 *provider_name*必须是唯一的当前计算机上安装的指定 OLE DB 访问接口。 *provider_name*是**nvarchar (** 128 **)** ，默认值为 NULL; 但是，如果*provider_name*是省略，则使用 SQLNCLI。 （使用 SQLNCLI 并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将重定向到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口的最新版本。）OLE DB 访问接口应以指定的 progid 在注册表中注册。  
  
`[ @datasrc = ] 'data_source'` 根据 OLE DB 访问接口的说明，请为数据源的名称。 *data_source*是**nvarchar (** 4000 **)** 。 *data_source*作为以初始化 OLE DB 访问接口的 DBPROP_INIT_DATASOURCE 属性传递。  
  
`[ @location = ] 'location'` 根据 OLE DB 访问接口的说明，请为数据库的位置。 *位置*是**nvarchar (** 4000 **)** ，默认值为 NULL。 *位置*作为 DBPROP_INIT_LOCATION 属性传递以初始化 OLE DB 访问接口。  
  
`[ @provstr = ] 'provider_string'` 是标识唯一的数据源的 OLE DB 提供程序特定的连接字符串。 *provider_string*是**nvarchar (** 4000 **)** ，默认值为 NULL。 *provstr*传递给 IDataInitialize，或者设置为 DBPROP_INIT_PROVIDERSTRING 属性以初始化 OLE DB 访问接口。  
  
 当针对创建链接的服务器[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序实例可以通过为服务器使用 SERVER 关键字指定 =*servername*\\*instancename*指定的特定实例[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 *servername*所在的计算机的名称[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]正在运行，并*instancename*的特定实例的名称[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]用户将连接到。  
  
> [!NOTE]
>  若要访问镜像数据库，则连接字符串必须包含数据库名称。 该名称是数据访问接口启用故障转移尝试所必需的。 可以在指定的数据库 **@provstr** 或 **@catalog** 参数。 此外，连接字符串还可以提供故障转移伙伴名称。  
  
`[ @catalog = ] 'catalog'` 是与 OLE DB 访问接口建立连接时要使用的目录。 *目录*是**sysname**，默认值为 NULL。 *目录*作为 DBPROP_INIT_CATALOG 属性传入以初始化 OLE DB 访问接口进行传递。 在针对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例定义链接服务器时，目录指向链接服务器映射到的默认数据库。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无。  
  
## <a name="remarks"></a>备注  
 下表显示为能通过 OLE DB 访问数据源而建立链接服务器的方法。 对于特定的数据源，可以使用多种方法为其设置链接服务器；该表中可能有多行适用于一种数据源类型。 此表还显示**sp_addlinkedserver**设置链接服务器使用的参数值。  
  
|远程 OLE DB 数据源|OLE DB 访问接口|product_name|provider_name|data_source|location|provider_string|catalog|  
|-------------------------------|---------------------|-------------------|--------------------|------------------|--------------|----------------------|-------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] <sup>1</sup> （默认值）||||||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口||**SQLNCLI**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的网络名称（用于默认实例）|||数据库名称（可选）|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口||**SQLNCLI**|*servername*\\*instancename* （用于特定实例）|||数据库名称（可选）|  
|Oracle，版本 8 及更高版本|Oracle Provider for OLE DB|Any|**OraOLEDB.Oracle**|用于 Oracle 数据库的别名||||  
|Access/Jet|Microsoft OLE DB Provider for Jet|Any|**Microsoft.Jet.OLEDB.4.0**|Jet 数据库文件的完整路径||||  
|ODBC 数据源|Microsoft OLE DB Provider for ODBC|Any|**MSDASQL**|ODBC 数据源的系统 DSN||||  
|ODBC 数据源|[!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB Provider for ODBC|Any|**MSDASQL**|||ODBC 连接字符串||  
|文件系统|[!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB Provider for Indexing Service|Any|**MSIDXS**|索引服务目录名称||||  
|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel 电子表格|[!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB Provider for Jet|Any|**Microsoft.Jet.OLEDB.4.0**|Excel 文件的完整路径||Excel 5.0||  
|IBM DB2 数据库|[!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB Provider for DB2|Any|**DB2OLEDB**|||请参阅[!INCLUDE[msCoName](../../includes/msconame-md.md)]OLE DB Provider for DB2 文档。|DB2 数据库的目录名称|  
  
 <sup>1</sup>设置链接服务器的这种方式强制链接服务器的远程实例的网络名称相同的名称[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 使用*data_source*以指定的服务器。  
  
 <sup>2</sup> "任何"指产品名称可以是任何内容。  
  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序是与一起使用的提供程序[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]如果未不指定任何提供程序名称，或如果[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]被指定为产品名称。 即使指定了较早版本的访问接口名称 SQLOLEDB，在保存到目录时该名称也将改为 SQLNCLI。  
  
 *Data_source*，*位置*， *provider_string*，以及*目录*参数标识链接的数据库服务器到点。 如果其中任一参数为 NULL，则不设置相应的 OLE DB 初始化属性。  
  
 在群集环境中，当指定指向 OLE DB 数据源的文件名时，应使用通用命名规则 (UNC) 名称或共享驱动器指定位置。  
  
 **sp_addlinkedserver**不能在用户定义的事务内执行。  
  
> [!IMPORTANT]
>  通过创建链接的服务器时**sp_addlinkedserver**，为所有本地登录名添加默认自映射。 对于非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]提供程序，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]已经过身份验证登录名可能会无法访问的提供程序下[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]服务帐户。 管理员应考虑使用 `sp_droplinkedsrvlogin <linkedserver_name>, NULL` 删除全局映射。  
  
## <a name="permissions"></a>权限  
 `sp_addlinkedserver`语句需要`ALTER ANY LINKED SERVER`权限。 (SSMS**新建链接服务器**要求的成员身份的方式实现对话框`sysadmin`固定的服务器角色。)  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-the-microsoft-sql-server-native-client-ole-db-provider"></a>A. 使用 Microsoft SQL Server Native Client OLE DB 访问接口  
 下面的示例将创建一个名为 `SEATTLESales` 的链接服务器。 产品名称为 `SQL Server`，未使用访问接口名称。  
  
```  
USE master;  
GO  
EXEC sp_addlinkedserver   
   N'SEATTLESales',  
   N'SQL Server';  
GO  
```  
  
 下面的示例创建一个链接的服务器`S1_instance1`的实例上[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]通过使用[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序。  
  
```  
EXEC sp_addlinkedserver     
   @server=N'S1_instance1',   
   @srvproduct=N'',  
   @provider=N'SQLNCLI',   
   @datasrc=N'S1\instance1';  
```  
  
### <a name="b-using-the-microsoft-ole-db-provider-for-microsoft-access"></a>B. 使用 Microsoft OLE DB Provider for Microsoft Access  
 Microsoft.Jet.OLEDB.4.0 访问接口连接到使用 2002-2003 格式的 Microsoft Access 数据库。 下面的示例将创建一个名为 `SEATTLE Mktg` 的链接服务器。  
  
> [!NOTE]  
>  此示例假定这两个[!INCLUDE[msCoName](../../includes/msconame-md.md)]访问示例和示例**Northwind**安装数据库并且**Northwind**数据库位于 C:\Msoffice\Access\Samples 中。  
  
```  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Mktg',   
   @provider = N'Microsoft.Jet.OLEDB.4.0',   
   @srvproduct = N'OLE DB Provider for Jet',  
   @datasrc = N'C:\MSOffice\Access\Samples\Northwind.mdb';  
GO  
```  
  
 Microsoft.ACE.OLEDB.12.0 访问接口连接到使用 2007 格式的 Microsoft Access 数据库。 下面的示例将创建一个名为 `SEATTLE Mktg` 的链接服务器。  
  
> [!NOTE]  
>  此示例假定这两个[!INCLUDE[msCoName](../../includes/msconame-md.md)]访问示例和示例**Northwind**安装数据库并且**Northwind**数据库位于 C:\Msoffice\Access\Samples 中。  
  
```  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Mktg',   
   @provider = N'Microsoft.ACE.OLEDB.12.0',   
   @srvproduct = N'OLE DB Provider for ACE',  
   @datasrc = N'C:\MSOffice\Access\Samples\Northwind.accdb';  
GO  
```  
  
### <a name="c-using-the-microsoft-ole-db-provider-for-odbc-with-the-datasource-parameter"></a>C. 将 Microsoft OLE DB Provider for ODBC 与 data_source 参数一起使用  
 下面的示例创建名为的链接的服务器`SEATTLE Payroll`，它使用[!INCLUDE[msCoName](../../includes/msconame-md.md)]OLE DB Provider for ODBC (`MSDASQL`) 和*data_source*参数。  
  
> [!NOTE]  
>  在使用该链接服务器之前，必须在该服务器中将指定的 ODBC 数据源名称定义为系统 DSN。  
  
```  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Payroll',   
   @srvproduct = N'',  
   @provider = N'MSDASQL',   
   @datasrc = N'LocalServer';  
GO  
```  
  
### <a name="d-using-the-microsoft-ole-db-provider-for-excel-spreadsheet"></a>D. 将 Microsoft OLE DB Provider 用于 Excel 电子表格  
 若要创建链接的服务器定义使用[!INCLUDE[msCoName](../../includes/msconame-md.md)]OLE DB Provider for Jet 访问 1997年-2003年格式的 Excel 电子表格首先创建一个命名的范围在 Excel 中通过指定要选择的 Excel 工作表的行和列。 这样，可以在分布式查询中将此范围的名称引用为表名称。  
  
```  
EXEC sp_addlinkedserver 'ExcelSource',  
   'Jet 4.0',  
   'Microsoft.Jet.OLEDB.4.0',  
   'c:\MyData\DistExcl.xls',  
   NULL,  
   'Excel 5.0';  
GO  
```  
  
 若要访问 Excel 电子表格中的数据，请将单元范围与名称相关联。 以下查询通过使用先前设置的链接服务器，将指定的命名范围 `SalesData` 作为表来访问。  
  
```  
SELECT *  
   FROM ExcelSource...SalesData;  
GO  
```  
  
 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在可以访问远程共享的域帐户下运行，则可以使用 UNC 路径来代替映射驱动器。  
  
```  
EXEC sp_addlinkedserver 'ExcelShare',  
   'Jet 4.0',  
   'Microsoft.Jet.OLEDB.4.0',  
   '\\MyServer\MyShare\Spreadsheets\DistExcl.xls',  
   NULL,  
   'Excel 5.0';  
```  
  
 若要连接到 Excel 2007 格式的 Excel 电子表格，请使用 ACE 访问接口。  
  
```  
EXEC sp_addlinkedserver @server = N'ExcelDataSource',   
@srvproduct=N'ExcelData', @provider=N'Microsoft.ACE.OLEDB.12.0',   
@datasrc=N'C:\DataFolder\People.xlsx',  
@provstr=N'EXCEL 12.0' ;  
  
```  
  
### <a name="e-using-the-microsoft-ole-db-provider-for-jet-to-access-a-text-file"></a>E. 使用 Microsoft OLE DB Provider for Jet 访问文本文件  
 以下示例创建直接访问文本文件的链接服务器，而没有将这些文件链接为 Access .mdb 文件中的表。 访问接口为 `Microsoft.Jet.OLEDB.4.0`，访问接口字符串为 `Text`。  
  
 数据源是包含文本文件的目录的完整路径。 schema.ini 文件（描述文本文件的结构）必须与此文本文件存在于相同的目录中。 有关如何创建 Schema.ini 文件的详细信息，请参阅 Jet 数据库引擎文档。  
  
```  
--Create a linked server.  
EXEC sp_addlinkedserver txtsrv, N'Jet 4.0',   
   N'Microsoft.Jet.OLEDB.4.0',  
   N'c:\data\distqry',  
   NULL,  
   N'Text';  
GO  
  
--Set up login mappings.  
EXEC sp_addlinkedsrvlogin txtsrv, FALSE, Admin, NULL;  
GO  
  
--List the tables in the linked server.  
EXEC sp_tables_ex txtsrv;  
GO  
  
--Query one of the tables: file1#txt  
--using a four-part name.   
SELECT *   
FROM txtsrv...[file1#txt];  
```  
  
### <a name="f-using-the-microsoft-ole-db-provider-for-db2"></a>F. 使用 Microsoft OLE DB Provider for DB2  
 以下示例创建名为 `DB2` 的链接服务器，该服务器使用 `Microsoft OLE DB Provider for DB2`。  
  
```  
EXEC sp_addlinkedserver  
   @server=N'DB2',  
   @srvproduct=N'Microsoft OLE DB Provider for DB2',  
   @catalog=N'DB2',  
   @provider=N'DB2OLEDB',  
   @provstr=N'Initial Catalog=PUBS;  
       Data Source=DB2;  
       HostCCSID=1252;  
       Network Address=XYZ;  
       Network Port=50000;  
       Package Collection=admin;  
       Default Schema=admin;';  
```  
  
### <a name="g-add-a-includesssdsfullincludessssdsfull-mdmd-as-a-linked-server-for-use-with-distributed-queries-on-cloud-and-on-premise-databases"></a>G. 添加一个 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 作为链接服务器以用于云数据库和本地数据库上的分布式查询  
 您可以添加一个 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 作为链接服务器，然后将它用于跨本地数据库和云数据库的分布式查询。 这是用于跨本地公司网络和 Windows Azure 云的数据库混合解决方案的组件。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]现成产品包含分布式的查询功能，以便您可以编写查询来合并来自本地数据源的数据和远程数据源 (包括数据从非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]数据源) 定义为链接服务器。 每个 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]（虚拟 master 除外）可以添加为单个链接服务器，然后在您的数据库应用程序中像任何其他数据库一样直接使用它。  
  
 使用 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 的好处包括便于管理、高可用性和可伸缩性、使用熟悉的开发模型以及采用关系数据模型。 您的数据库应用程序要求决定了它将在云中如何使用 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。 您可以立即将所有数据移到 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]，或逐渐迁移一部分数据而将其余数据保留在本地。 对于这类混合数据库应用程序，[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 现在可以作为链接服务器添加且数据库应用程序可以发布分布式查询命令来合并 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 和本地数据源上的数据。  
  
 下面是简单的示例说明了如何连接到[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]使用分布式的查询：  
  
```  
------ Configure the linked server  
-- Add one Windows Azure SQL DB as Linked Server  
EXEC sp_addlinkedserver  
@server='myLinkedServer', -- here you can specify the name of the linked server  
@srvproduct='',       
@provider='sqlncli', -- using SQL Server Native Client  
@datasrc='myServer.database.windows.net',   -- add here your server name  
@location='',  
@provstr='',  
@catalog='myDatabase'  -- add here your database name as initial catalog (you cannot connect to the master database)  
-- Add credentials and options to this linked server  
EXEC sp_addlinkedsrvlogin  
@rmtsrvname = 'myLinkedServer',  
@useself = 'false',  
@rmtuser = 'myLogin',             -- add here your login on Azure DB  
@rmtpassword = 'myPassword' -- add here your password on Azure DB  
EXEC sp_serveroption 'myLinkedServer', 'rpc out', true;  
------ Now you can use the linked server to execute 4-part queries  
-- You can create a new table in the Azure DB  
exec ('CREATE TABLE t1tutut2(col1 int not null CONSTRAINT PK_col1 PRIMARY KEY CLUSTERED (col1) )') at myLinkedServer  
-- Insert data from your local SQL Server  
exec ('INSERT INTO t1tutut2 VALUES(1),(2),(3)') at myLinkedServer  
  
-- Query the data using 4-part names  
select * from myLinkedServer.myDatabase.dbo.myTable  
```  
  
## <a name="see-also"></a>请参阅  
 [分布式查询存储的过程&#40;Transact SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_dropserver &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md)   
 [sp_serveroption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)   
 [sp_setnetname &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-setnetname-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [系统表 (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
