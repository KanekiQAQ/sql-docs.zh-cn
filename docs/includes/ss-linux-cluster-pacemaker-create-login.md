---
ms.openlocfilehash: d2519b1cc56081f8a35308ac41e11f46a7f97211
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68215609"
---
1. **在所有 SQL Server 上，为 Pacemaker 创建 Server 登录名**。 以下 Transact-SQL 创建登录名：

   ```Transact-SQL
   USE [master]
   GO
   CREATE LOGIN [pacemakerLogin] with PASSWORD= N'ComplexP@$$w0rd!'
    
   ALTER SERVER ROLE [sysadmin] ADD MEMBER [pacemakerLogin]
   ```

  在创建可用性组时，pacemaker 用户需要 ALTER、 控件和 VIEW DEFINITION 权限的可用性组中，创建它之后，但之前的任何节点添加到其中。

1. **在所有 SQL Server 上，保存 SQL Server 登录名的凭据**。

   ```bash
   echo 'pacemakerLogin' >> ~/pacemaker-passwd
   echo 'ComplexP@$$w0rd!' >> ~/pacemaker-passwd
   sudo mv ~/pacemaker-passwd /var/opt/mssql/secrets/passwd
   sudo chown root:root /var/opt/mssql/secrets/passwd
   sudo chmod 400 /var/opt/mssql/secrets/passwd # Only readable by root
   ```
