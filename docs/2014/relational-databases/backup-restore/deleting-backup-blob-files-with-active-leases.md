---
title: 删除具有活动租约的备份 Blob 文件 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 13a8f879-274f-4934-a722-b4677fc9a782
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 3066700945d2d6dad33f04c6bc905720daab61c3
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62876167"
---
# <a name="deleting-backup-blob-files-with-active-leases"></a>删除具有活动租约的备份 Blob 文件
  在备份到 Windows Azure 存储区或从中还原时，SQL Server 获得无限期租约以锁定对 blob 的独占访问。 当成功完成备份或还原过程时，释放租约。 如果备份或还原失败，备份过程将尝试清除所有无效 blob。 但是，如果由于持续很长时间的网络连接故障而导致备份失败，备份过程可能无法再次访问 blob 且 blob 可能保持孤立状态。 这意味着在释放租约前，不能写入或删除 blob。 本主题说明如何释放租约和删除 blob。  
  
 有关租约类型的详细信息，请阅读此 [文章](https://go.microsoft.com/fwlink/?LinkId=275664)。  
  
 如果备份操作失败，它可能生成无效的备份文件。  备份 blob 文件可能还有活动租约，以防止其被删除或覆盖。  为了删除或覆盖这类 blob，应首先中断租约。如果备份失败，我们建议您清除租约并删除 blob。 您还可以选择作为存储管理任务一部分的定期清除。  
  
 如果还原失败，将不阻止后续还原，因此活动租约不会导致问题。 仅当必须覆盖或删除 blob 时，才有必要中断租约。  
  
## <a name="managing-orphaned-blobs"></a>管理孤立的 Blob  
 以下步骤说明在备份或还原活动失败后如何进行清除。 可以使用 PowerShell 脚本来执行所有这些步骤。 在接下来的章节中提供了代码示例：  
  
1.  **标识具有租约的 blob:** 如果有运行备份过程的脚本或进程，可能可以捕获脚本或进程内的失败并使用它清除 blob。   您还可以使用 LeaseStats 和 LeastState 属性来标识具有租约的 blob。 一旦您标识了 blob，我们建议您查看列表，在删除 blob 前验证备份文件的有效性。  
  
2.  **中断租约：** 获得授权的请求可以中断租约而不提供租约 ID。 有关详细信息，请参阅 [此处](https://go.microsoft.com/fwlink/?LinkID=275664) 。  
  
    > [!TIP]  
    >  SQL Server 发出租约 ID 以在还原操作期间建立独占访问。 还原租约 ID 是 BAC2BAC2BAC2BAC2BAC2BAC2BAC2BAC2。  
  
3.  **删除 Blob:** 要删除具有活动租约的 Blob，必须先中断租约。  
  
###  <a name="Code_Example"></a> PowerShell 脚本示例  
 **\*\* 重要\* \* **如果正在运行 PowerShell 2.0，您可能必须加载 Microsoft WindowsAzure.Storage.dll 程序集的问题。 我们建议您升级到 Powershell 3.0 以解决该问题。 您也可以为 PowerShell 2.0 使用以下解决方法：  
  
-   使用以下语句创建或修改 powershell.exe.config 文件以在运行时加载 .NET 2.0 和 .NET 4.0 程序集：  
  
    ```  
    <?xml version="1.0"?>   
    <configuration>   
        <startup useLegacyV2RuntimeActivationPolicy="true">   
            <supportedRuntime version="v4.0.30319"/>   
            <supportedRuntime version="v2.0.50727"/>   
        </startup>   
    </configuration>  
  
    ```  
  
 下面的示例说明了如何标识具有活动租约的 blob，然后中断租约。 该示例还演示如何为释放租约 ID 进行筛选。  
  
 有关运行此脚本的提示  
  
> [!WARNING]  
>  如果在运行此脚本的同时执行备份到 Windows Azure Blob 存储服务，则备份可能失败，因为此脚本将中断备份操作此时要获取的租约。 我们建议在维护时间段或预计没有执行备份时运行此脚本。  
  
1.  当您运行此脚本时，将提示您为存储帐户、存储密钥、容器和 windows azure 存储程序集路径和名称参数提供值。 存储程序集的路径为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的安装目录。 存储程序集的文件名为 Microsoft.WindowsAzure.Storage.dll。 以下是提示和输入的值的示例：  
  
    ```  
    cmdlet  at command pipeline position 1  
    Supply values for the following parameters:  
    storageAccount: mycloudstorageaccount  
    storageKey: 0BopKY7eEha3gBnistYk+904nf  
    blobContainer: mycontainer   
    storageAssemblyPath: C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Binn\Microsoft.WindowsAzure.Storage.dll  
    ```  
  
2.  如果没有已锁定租约的 blob，您应该看到以下消息：  
  
     **没有具有已锁定租约状态的 blob**  
  
     如果有具有已锁定租约的 blob，您应该看到以下消息：  
  
     **正在中断租约**  
  
     **\<Blob 的 URL> 上的租约是还原租约：仅当某个 blob 具有仍处于活动状态的还原租约时，才会看到此消息。**  
  
     **\<Blob 的 URL> 上的租约不是还原租约，正在中断 \<Blob 的 URL> 上的租约。**  
  
```  
param(  
       [Parameter(Mandatory=$true)]  
       [string]$storageAccount,  
       [Parameter(Mandatory=$true)]  
       [string]$storageKey,  
       [Parameter(Mandatory=$true)]  
       [string]$blobContainer,  
       [Parameter(Mandatory=$true)]  
       [string]$storageAssemblyPath  
)  
  
# Well known Restore Lease ID  
$restoreLeaseId = "BAC2BAC2BAC2BAC2BAC2BAC2BAC2BAC2"  
  
# Load the storage assembly without locking the file for the duration of the PowerShell session  
$bytes = [System.IO.File]::ReadAllBytes($storageAssemblyPath)  
[System.Reflection.Assembly]::Load($bytes)  
  
$cred = New-Object 'Microsoft.WindowsAzure.Storage.Auth.StorageCredentials' $storageAccount, $storageKey  
  
$client = New-Object 'Microsoft.WindowsAzure.Storage.Blob.CloudBlobClient' "https://$storageAccount.blob.core.windows.net", $cred  
  
$container = $client.GetContainerReference($blobContainer)  
  
#list all the blobs  
$allBlobs = $container.ListBlobs()   
  
$lockedBlobs = @()  
# filter blobs that are have Lease Status as "locked"  
foreach($blob in $allBlobs)  
{  
    $blobProperties = $blob.Properties   
    if($blobProperties.LeaseStatus -eq "Locked")  
    {  
        $lockedBlobs += $blob  
  
    }  
}  
  
if ($lockedBlobs.Count -eq 0)  
    {   
        Write-Host " There are no blobs with locked lease status"  
    }  
if($lockedBlobs.Count -gt 0)  
{  
    write-host "Breaking leases"  
    foreach($blob in $lockedBlobs )   
    {  
        try  
        {  
            $blob.AcquireLease($null, $restoreLeaseId, $null, $null, $null)  
            Write-Host "The lease on $($blob.Uri) is a restore lease"  
        }  
        catch [Microsoft.WindowsAzure.Storage.StorageException]  
        {  
            if($_.Exception.RequestInformation.HttpStatusCode -eq 409)  
            {  
                Write-Host "The lease on $($blob.Uri) is not a restore lease"  
            }  
        }  
  
        Write-Host "Breaking lease on $($blob.Uri)"  
        $blob.BreakLease($(New-TimeSpan), $null, $null, $null) | Out-Null  
    }  
}  
  
```  
  
## <a name="see-also"></a>请参阅  
 [SQL Server 备份到 URL 最佳实践和故障排除](sql-server-backup-to-url-best-practices-and-troubleshooting.md)  
  
  
