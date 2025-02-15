---
title: 第 6 课：将数据库从一个源计算机的本地到 Windows Azure 中的目标计算机 |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: database-engine
ms.topic: conceptual
ms.assetid: d9134ade-7b03-4c5c-8ed3-3bc369a61691
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 1a5787a3f5aecd746ac9aafd5850e6109ebcd999
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "66090702"
---
# <a name="lesson-6-migrate-a-database-from-a-source-machine-on-premises-to-a-destination-machine-in-windows-azure"></a>第 6 课：将数据库从本地源计算机迁移至 Windows Azure 中的目标计算机
  本课假定您已有另一个 SQL Server，它可能位于另一个本地计算机中或位于 Windows Azure 的虚拟机中。 有关如何在 Windows Azure 中创建 SQL Server 虚拟机的信息，请参阅[预配 Windows Azure 上的 SQL Server 虚拟机](http://www.windowsazure.com/manage/windows/common-tasks/install-sql-server/)。 在 Windows Azure 中设置 SQL Server 虚拟机后，确保可在另一计算机中通过 SQL Server Management Studio 连接到此虚拟机中的 SQL Server 实例。  
  
 本课还假定您已完成以下步骤：  
  
-   具有 Windows Azure 存储帐户。  
  
-   已在 Windows Azure 存储帐户下创建了容器。  
  
-   已在容器上创建了具有读取、写入和列表权限的策略。 还生成了 SAS 密钥。  
  
-   已在源计算机上创建了 SQL Server 凭据。  
  
-   已在 Windows Azure 中创建了目标 SQL Server 虚拟机。 建议通过选择包含 SQL Server 2014 的平台映像创建该虚拟机。  
  
 若要将数据库从本地 SQL Server 迁移到 Windows Azure 中的另一个虚拟机，可执行以下步骤：  
  
1.  在源计算机（本教程中为本地计算机）中，在 SQL Server Management Studio 中打开查询窗口。 通过执行以下语句，分离数据库以将其移至另一个计算机：  
  
    ```  
    -- Detach the database in the source machine   
    USE master  
    EXEC sp_detach_db 'TestDB1', 'true';  
    ```  
  
2.  如果需要将数据库传输到目标计算机，则首先必须准备该数据库。 若要准备目标计算机，首先需要在目标计算机中创建一个 SQL Server 凭据。 如果这是加密数据库，则还需要将源计算机中的证书导入到目标计算机。  
  
    1.  若要在目标计算机中创建 SQL Server 凭据，请执行以下步骤：  
  
        1.  在源计算机中通过 SQL Server Management Studio 连接到目标计算机。  或者，直接在目标计算机中启动 SQL Server Management Studio。  
  
        2.  在标准工具栏上单击**新查询**。  
  
        3.  将以下示例复制并粘贴到查询窗口中，并根据需要进行修改。 以下语句创建 SQL Server 凭据以存储你的存储容器的共享访问证书。  
  
            ```sql  
  
            USE master   
            GO   
            CREATE CREDENTIAL [http://teststorageaccnt.blob.core.windows.net/testcontainer]   
            WITH IDENTITY='SHARED ACCESS SIGNATURE',   
            SECRET = 'your SAS key'   
            GO  
  
            ```  
  
        4.  若要查看所有可用的凭据，可在查询窗口中运行以下语句：  
  
            ```sql  
            SELECT * from sys.credentials   
            ```  
  
        5.  连接到目标服务器后，打开查询窗口并运行：  
  
            ```sql  
  
            -- Create a master key and a server certificate   
            USE master   
            GO   
            CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'MySQLKey01'; -- You may use a different password.   
            GO   
            CREATE CERTIFICATE MySQLCert   
            FROM FILE = 'C:\certs\MySQLCert.CER'   
            WITH PRIVATE KEY   
            (   
            FILE = 'C:\certs\MySQLPrivateKeyFile.PVK',   
            DECRYPTION BY PASSWORD = 'MySQLKey01'   
            );   
            GO  
  
            ```  
  
             此步骤结束时，目标计算机已导入从源计算机备份的加密证书。 接下来，可将数据文件附加到目标计算机中。  
  
    2.  然后，使用 FOR ATTACH 选项通过指向 Windows Azure 存储中现有文件的数据和日志文件创建数据库。 在查询窗口中，运行以下语句：  
  
        ```sql  
  
        --Create a database on the destination server   
        CREATE DATABASE TestDB1onDest   
        ON   
        (NAME = TestDB1_data,   
            FILENAME = 'https://teststorageaccnt.blob.core.windows.net/testcontainer/TestDB1Data.mdf' )   
        LOG ON   
         (NAME = TestDB1_log,   
            FILENAME = 'https://teststorageaccnt.blob.core.windows.net/testcontainer/TestDB1Log.ldf')   
        FOR ATTACH   
        GO  
  
        ```  
  
    3.  在对象资源管理器中，单击“数据库”，右键单击“刷新”。 应可看到列出新创建的数据库 TestDB1onDest。  
  
    4.  接下来，在查询窗口中运行以下语句：  
  
        ```sql  
  
        USE TestDB1onDest   
        SELECT * FROM Table1;   
        GO  
  
        ```  
  
         随后应列出在第 4 课中输入的所有数据。  
  
 注意，加密的数据库传输到另一个计算实例，但未移动数据。  
  
 若要使用 SQL Server Management Studio 用户界面通过指向 Windows Azure 存储中现有文件的数据和日志文件创建数据库，请执行以下步骤：  
  
1.  在“对象资源管理器”  中，连接到一个 SQL Server 数据库引擎实例，然后展开该实例。  
  
2.  右键单击“数据库”  ，然后单击“新建数据库”  。 然后，右键单击“TestDB1”。 单击“任务”，然后单击“分离”。 在“分离”对话框窗口中，选中“删除连接”。 单击“确定”  。  
  
3.  连接到目标计算机，该计算机具有 SQL Server 2014 CTP2 或更高版本。 若要准备目标计算机，需要在目标计算机中创建一个 SQL Server 凭据，使其指向将 TestDB1 放入的同一容器。 如果要在同一个计算机上重新附加，则不需要创建另一个凭据。  
  
4.  在中**对象资源管理器**，右键单击**数据库**然后单击**附加**。  
  
5.  在中**附加数据库**对话框框，以指定要附加的数据库中，单击**添加**。 在中**定位数据库文件**对话框窗口：  
  
     对于数据库数据文件位置，键入： `https://teststorageaccnt.blob.core.windows.net/testcontainer/`。  
  
     对于文件名称，键入： `TestDB1Data.mdf`。  
  
6.  单击“确定”  。  
  
     ![SQL 14 CTP2](../tutorials/media/ss-was-tutlesson-6-7.gif "SQL 14 CTP2")  
  
 **下一课：**  
  
 [第 7 课：数据文件移到 Windows Azure 存储](../relational-databases/lesson-6-generate-activity-and-backup-log-using-file-snapshot-backup.md)  
  
  
