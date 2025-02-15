---
title: 全文搜索 DDL、函数、存储过程和视图 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
ms.assetid: 98c36715-4195-482e-a4a3-d93ff65b75f1
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3e6674c40fe0f0b01d9a2b59104b67e0eff7cd2f
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68082774"
---
# <a name="full-text-search-ddl-functions-stored-procedures-and-views"></a>全文搜索 DDL、函数、存储过程和视图
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  列出支持全文搜索（包括属性搜索功能）的 Transact-SQL 语句和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库对象。  
  
 此列表中不包括不推荐使用的对象。  
  
 有关支持语义搜索的数据库对象的列表，请参阅 [Semantic Search DDL, Functions, Stored Procedures, and Views](../../relational-databases/search/semantic-search-ddl-functions-stored-procedures-and-views.md)。  
  
##  <a name="ddl"></a> Transact-SQL 数据定义语言 (DDL) 语句  
  
-   [CREATE FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)  
  
-   [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md)  
  
-   [CREATE FULLTEXT STOPLIST (Transact-SQL)](../../t-sql/statements/create-fulltext-stoplist-transact-sql.md)  
  
-   [CREATE SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/create-search-property-list-transact-sql.md)  
  
-   [ALTER FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)  
  
-   [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
-   [ALTER FULLTEXT STOPLIST (Transact-SQL)](../../t-sql/statements/alter-fulltext-stoplist-transact-sql.md)  
  
-   [ALTER SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/alter-search-property-list-transact-sql.md)  
  
-   [DROP FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md)  
  
-   [DROP FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/drop-fulltext-index-transact-sql.md)  
  
-   [DROP FULLTEXT STOPLIST (Transact-SQL)](../../t-sql/statements/drop-fulltext-stoplist-transact-sql.md)  
  
-   [DROP SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/drop-search-property-list-transact-sql.md)  
  
##  <a name="func"></a> 系统谓词和功能  
  
-   [CONTAINS (Transact-SQL)](../../t-sql/queries/contains-transact-sql.md)  
  
-   [CONTAINSTABLE (Transact-SQL)](../../relational-databases/system-functions/containstable-transact-sql.md)  
  
-   [FREETEXT (Transact-SQL)](../../t-sql/queries/freetext-transact-sql.md)  
  
-   [FREETEXTTABLE (Transact-SQL)](../../relational-databases/system-functions/freetexttable-transact-sql.md)  
  
##  <a name="meta"></a> 系统元数据函数  
  
-   [COLUMNPROPERTY (Transact-SQL)](../../t-sql/functions/columnproperty-transact-sql.md)  
  
-   [FULLTEXTCATALOGPROPERTY (Transact-SQL)](../../t-sql/functions/fulltextcatalogproperty-transact-sql.md)  
  
-   [FULLTEXTSERVICEPROPERTY (Transact-SQL)](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)  
  
-   [INDEXPROPERTY (Transact-SQL)](../../t-sql/functions/indexproperty-transact-sql.md)  
  
-   [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md)  
  
-   [OBJECTPROPERTYEX (Transact-SQL)](../../t-sql/functions/objectpropertyex-transact-sql.md)  
  
-   [SERVERPROPERTY (Transact-SQL)](../../t-sql/functions/serverproperty-transact-sql.md)  
  
##  <a name="proc"></a> 系统存储过程  
  
-   [sp_fulltext_keymappings (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md)  
  
-   [sp_fulltext_load_thesaurus_file (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-load-thesaurus-file-transact-sql.md)  
  
-   [sp_fulltext_pendingchanges (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-pendingchanges-transact-sql.md)  
  
-   [sp_fulltext_service (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)  
  
-   [sp_help_fulltext_system_components (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-fulltext-system-components-transact-sql.md)  
  
##  <a name="cat"></a> 系统视图 - 目录视图  
  
-   [sys.fulltext_catalogs (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)  
  
-   [sys.fulltext_document_types (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md)  
  
-   [sys.fulltext_index_catalog_usages (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-index-catalog-usages-transact-sql.md)  
  
-   [sys.fulltext_index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)  
  
-   [sys.fulltext_index_fragments (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md)  
  
-   [sys.fulltext_indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)  
  
-   [sys.fulltext_languages (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)  
  
-   [sys.fulltext_stoplists (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)  
  
-   [sys.fulltext_stopwords (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)  
  
-   [sys.fulltext_system_stopwords (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-system-stopwords-transact-sql.md)  
  
-   [sys.registered_search_properties (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)  
  
-   [sys.registered_search_property_lists (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)  
  
##  <a name="dmv"></a> 系统视图 - 动态管理视图  
  
-   [sys.dm_fts_active_catalogs (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-active-catalogs-transact-sql.md)  
  
-   [sys.dm_fts_fdhosts (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-fdhosts-transact-sql.md)  
  
-   [sys.dm_fts_index_keywords (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)  
  
-   [sys.dm_fts_index_keywords_by_document (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
-   [sys.dm_fts_index_keywords_by_property (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)  
  
-   [sys.dm_fts_index_population (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)  
  
-   [sys.dm_fts_memory_buffers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md)  
  
-   [sys.dm_fts_memory_pools (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-pools-transact-sql.md)  
  
-   [sys.dm_fts_outstanding_batches (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-outstanding-batches-transact-sql.md)  
  
-   [sys.dm_fts_parser (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-parser-transact-sql.md)  
  
-   [sys.dm_fts_population_ranges (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-population-ranges-transact-sql.md)  
  
  
