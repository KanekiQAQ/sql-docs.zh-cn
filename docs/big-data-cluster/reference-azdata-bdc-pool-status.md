---
title: azdata bdc 池状态引用
titleSuffix: SQL Server big data clusters
description: Azdata bdc 池状态命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 07/24/2019
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d0a5925af4f16f2147988b2318880d9acec664c3
ms.sourcegitcommit: 1f222ef903e6aa0bd1b14d3df031eb04ce775154
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2019
ms.locfileid: "68426127"
---
# <a name="azdata-bdc-pool-status"></a>azdata bdc 池状态

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx.md)]

以下文章提供了**azdata**工具中的**bdc 池状态**命令参考。 有关其他**azdata**命令的详细信息, 请参阅[azdata reference](reference-azdata.md)。

## <a name="commands"></a>命令
|     |     |
| --- | --- |
[azdata bdc 池状态显示](#azdata-bdc-pool-status-show) | 池状态。
## <a name="azdata-bdc-pool-status-show"></a>azdata bdc 池状态显示
池状态。
```bash
azdata bdc pool status show --kind -k 
                            [--name -n]
```
### <a name="examples"></a>示例
获取存储池的状态。
```bash
azdata bdc pool status show --kind storage --name default
```
获取数据池的状态。
```bash
azdata bdc pool status show --kind data --name default
```
获取计算池的状态。
```bash
azdata bdc pool status show --kind compute --name default
```
获取主池的状态。
```bash
azdata bdc pool status show --kind master --name default
```
获取 spark 池的状态。
```bash
azdata bdc pool status show --kind spark --name default
```
### <a name="required-parameters"></a>必需的参数
#### `--kind -k`
大数据群集池类型。
### <a name="optional-parameters"></a>可选参数
#### `--name -n`
大数据群集池名称。
`default`
### <a name="global-arguments"></a>全局参数
#### `--debug`
提高日志记录详细程度以显示所有调试日志。
#### `--help -h`
显示此帮助消息并退出。
#### `--output -o`
输出格式。  允许的值: json、jsonc、table、tsv。  默认值: json。
#### `--query -q`
JMESPath 查询字符串。 有关[http://jmespath.org/](http://jmespath.org/])详细信息和示例, 请参阅。
#### `--verbose`
提高日志记录详细程度。 使用--debug 获取完整的调试日志。

## <a name="next-steps"></a>后续步骤

有关如何安装**azdata**工具的详细信息, 请参阅[安装 azdata 以管理 SQL Server 2019 大数据群集](deploy-install-azdata.md)。
