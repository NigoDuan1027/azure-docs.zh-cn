---
title: 使用 Azure 媒体服务的云上传和存储 | Microsoft Docs
description: 本文介绍云上传和存储的概念。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/12/2019
ms.author: juliako
ms.openlocfilehash: d49d056ab84b60389df8bcaf1c75d6224633863d
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2019
ms.locfileid: "56337850"
---
# <a name="cloud-upload-and-storage"></a>云上传和存储

若要开始管理、加密、编码、分析和流式处理 Azure 中的媒体内容，需要创建媒体服务帐户。 创建媒体服务帐户时，需要提供 Azure 存储帐户资源的名称。 指定存储帐户会附加到媒体服务帐户。 

媒体服务帐户和所有关联的存储帐户必须位于同一 Azure 订阅中。 强烈建议在媒体服务帐户所在的位置使用存储帐户，避免额外的延迟和数据出口成本

必须具有一个主存储帐户，并且可以拥有任意数量的与媒体服务帐户关联的辅助存储帐户。 媒体服务支持常规用途 v2 (GPv2) 或常规用途 v1 (GPv1) 帐户。 

>[!NOTE]
> 不允许将仅限 Blob 的帐户作为主帐户。 

建议使用 GPv2，以便在热存储层和冷存储层之间进行选择。 若要了解存储帐户的详细信息，请参阅 [Azure 存储帐户概述](../../storage/common/storage-account-overview.md)。 

## <a name="assets-in-a-storage-account"></a>存储帐户中的资产

在媒体服务 v3 中，存储 API 用于上传文件。

> [!Note]
> 在不使用媒体服务 API 的情况下，不应该试更改媒体服务 SDK 生成的 BLOB 容器内容。

若要了解如何结合使用存储 API 与媒体服务来上传输入文件，请查看[从本地文件创建作业输入](job-input-from-local-file-how-to.md)。 
 
## <a name="next-steps"></a>后续步骤

若要了解如何将存储帐户附加到媒体服务帐户，请参阅[创建帐户](create-account-cli-quickstart.md)。
