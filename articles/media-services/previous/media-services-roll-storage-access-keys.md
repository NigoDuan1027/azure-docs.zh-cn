---
title: 轮转存储访问密钥后更新媒体服务 | Microsoft Docs
description: 此文将提供有关在轮转存储访问密钥后如何更新媒体服务的指导。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: a892ebb0-0ea0-4fc8-b715-60347cc5c95b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: milanga;cenkdin;juliako
ms.openlocfilehash: 419e7b1ae05f92b5b97f5317c0315a71958eff9e
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/11/2019
ms.locfileid: "56005159"
---
# <a name="update-media-services-after-rolling-storage-access-keys"></a>轮转存储访问密钥后更新媒体服务 

创建新的 Azure 媒体服务 (AMS) 帐户时，系统还会要求选择用于存储媒体内容的 Azure 存储帐户。 可将多个存储帐户添加到媒体服务帐户。 本文介绍如何轮换存储密钥。 此外，介绍如何将存储帐户添加到媒体帐户。 

若要执行本文所述的操作，应使用 [Azure 资源管理器 API](/rest/api/media/operations/azure-media-services-rest-api-reference) 和 [Powershell](https://docs.microsoft.com/powershell/module/azurerm.media)。  有关详细信息，请参阅[如何使用 PowerShell 和 Resource Manager 管理 Azure 资源](../../azure-resource-manager/powershell-azure-resource-manager.md)。

## <a name="overview"></a>概述

在创建新的存储帐户后，Azure 将生成两个 512 位存储访问密钥，用于对你存储帐户的访问进行身份验证。 要保持存储连接的安全性，我们建议定期重新生成并轮转存储访问密钥。 将提供两个访问密钥（主密钥和辅助密钥），以便在重新生成其中一个访问密钥时，始终能够使用另一个访问密钥连接到存储帐户。 此过程也称为“轮转访问密钥”。

媒体服务依赖于为它提供的存储密钥。 具体而言，用于流式传输或下载资产的定位符依赖于指定的存储访问密钥。 创建 AMS 帐户时，媒体服务默认依赖于主存储访问密钥，但用户可以更新 AMS 的存储密钥。 必须遵照本文中所述的步骤，确保媒体服务知晓要使用的密钥。  

>[!NOTE]
> 如果有多个存储帐户，请对每个存储帐户执行此过程。 存储密钥的轮换顺序不是固定的。 可以先轮换辅助密钥，再轮换主密钥，反之亦然。
>
> 在对生产帐户执行本文中所述的步骤之前，请确保对生产前帐户测试这些步骤。
>

## <a name="steps-to-rotate-storage-keys"></a>轮换存储密钥的步骤 
 
 1. 通过 PowerShell cmdlet 或 [Azure](https://portal.azure.com/) 门户更改存储帐户主密钥。
 2. 结合相应的参数调用 Sync-AzureRmMediaServiceStorageKeys cmdlet，强制媒体帐户选择存储帐户密钥
 
    以下示例演示如何将密钥同步到存储帐户。
  
         Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. 等待大约一小时。 验证流式处理方案是否正常工作。
 4. 通过 PowerShell cmdlet 或 Azure 门户更改存储帐户辅助密钥。
 5. 结合相应的参数调用 Sync-AzureRmMediaServiceStorageKeys powershell，强制媒体帐户选择新的存储帐户密钥。 
 6. 等待大约一小时。 验证流式处理方案是否正常工作。
 
### <a name="a-powershell-cmdlet-example"></a>PowerShell cmdlet 示例 

以下示例演示如何获取存储帐户并将它与 AMS 帐户同步。

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-to-add-storage-accounts-to-your-ams-account"></a>将存储帐户添加到 AMS 帐户的步骤

以下文章介绍了如何将存储帐户添加到 AMS 帐户：[将多个存储帐户附加到一个媒体服务帐户](meda-services-managing-multiple-storage-accounts.md)。

## <a name="media-services-learning-paths"></a>媒体服务学习路径
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>提供反馈
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>致谢
我们诚挚地向以下人员表达谢意，是他们协助完成了本文档的写作工作：Cenk Dingiloglu、Milan Gada、Seva Titov。
