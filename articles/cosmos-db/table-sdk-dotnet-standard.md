---
title: Azure Cosmos DB 表 API .NET Standard SDK 和资源
description: 了解有关 Azure Cosmos DB 表 API 和 .NET Standard SDK 的所有信息，包括发布日期、停用日期和各版本之间所做的更改。
author: wmengmsft
ms.author: wmeng
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: reference
ms.date: 10/18/2018
ms.openlocfilehash: ce7cc489b107ce4bd95270b9a7f8cb560a2d2398
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2019
ms.locfileid: "55249640"
---
# <a name="azure-cosmos-db-table-net-standard-api-download-and-release-notes"></a>Azure Cosmos DB 表 .NET Standard API：下载和发行说明
> [!div class="op_single_selector"]

> * [.NET](table-sdk-dotnet.md)
> * [.NET 标准](table-sdk-dotnet-standard.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK 下载**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table)|
|**当前受支持的框架**|[Microsoft .NET Standard 2.0](https://www.nuget.org/packages/NETStandard.Library)|

## <a name="release-notes"></a>发行说明

### <a name="a-name0101-preview0101-preview"></a><a name="0.10.1-preview"/>0.10.1 预览版
* 针对 Azure 存储表终结点添加了对 SAS 令牌以及 TablePermissions、ServiceProperties 和 ServiceStats 的操作的支持。 
   > [!NOTE]
   > 尚不支持以前的 Azure 存储表 SDK 中的某些功能，例如客户端加密。

### <a name="a-name0100-preview0100-preview"></a><a name="0.10.0-preview"/>0.10.0 预览版
* 针对 Azure 存储表终结点添加了对核心 CRUD、批处理和查询操作的支持。 
   > [!NOTE]
   > 尚不支持以前的 Azure 存储表 SDK 中的某些功能，例如客户端加密。

### <a name="a-name091-preview091-preview"></a><a name="0.9.1-preview"/>0.9.1 预览版
* Azure Cosmos DB 表 .NET Standard SDK 是一个跨平台 .NET 库，可高效访问 Cosmos DB 上的表数据模型。 此初始版本支持完整的表和实体 CRUD + 查询功能集，其中 API 与[用于 .NET Framework 的 Cosmos DB 表 SDK](table-sdk-dotnet.md) 类似。 
   > [!NOTE]
   >  0.9.1 预览版尚不支持 Azure 存储表终结点。

## <a name="release-and-retirement-dates"></a>发布日期和停用日期
Microsoft 至少会在停用 SDK 前提前 12 个月发出通知，以便顺利转换为更高版本/受支持版本。

| 版本 | 发布日期 | 停用日期 |
| --- | --- | --- |
| [0.10.1 预览版](#0.10.1-preview) |2019 年 1 月 22 日 |--- |
| [0.10.0 预览版](#0.10.0-preview) |2018 年 12 月 18 日 |--- |
| [0.9.1 预览版](#0.9.1-preview) |2018 年 10 月 18 日 |--- |


## <a name="faq"></a>常见问题解答

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>另请参阅
若要了解有关 Azure Cosmos DB 表 API 的详细信息，请参阅 [Azure Cosmos DB 表 API 简介](table-introduction.md)。 
