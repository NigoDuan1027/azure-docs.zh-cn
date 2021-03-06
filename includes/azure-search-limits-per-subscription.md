---
title: include 文件
description: include 文件
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
ms.date: 04/04/2018
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: 8a4f794c8ef24a90498954629c131904621c5b43
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2018
ms.locfileid: "38755549"
---
可以在一个订阅中创建多个服务，每个服务在特定层进行预配，仅受每个层所允许的服务数所限制。 例如，在同一订阅中，最多可以在基本层创建 12 个服务，在 S1 层也创建 12 个服务。 有关层的详细信息，请参阅[为 Azure 搜索选择 SKU 或层](../articles/search/search-sku-tier.md)。

最大服务数限制可以根据请求提高。 如果需要在同一订阅中使用更多服务，请与 Azure 支持部门联系。

| 资源            | 免费&nbsp;<sup>1</sup> | 基本 | S1  | S2 | S3 | S3&nbsp;HD |
| ------------------- | ---- | ----- | --- | -- | -- | ----- |
| 最大服务数    |1     | 12    | 12  | 6  | 6  | 6     |
| 最大规模（以 SU 为单位）&nbsp;<sup>2</sup> |不适用 |3 SU |36 个 SU |36 个 SU |36 个 SU |36 个 SU |

<sup>1</sup> 免费层基于共享资源，而不基于专用资源。 共享资源不支持纵向扩展。

<sup>2</sup> 搜索单位 (SU) 即计费单位，以*副本*或*分区*形式分配。 进行存储、索引和查询操作同时需要这两个资源。 若要了解有关 SU 计算的详细信息，请参阅[缩放查询和索引工作负荷的资源级别](../articles/search/search-capacity-planning.md)。 