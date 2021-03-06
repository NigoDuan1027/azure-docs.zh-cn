---
title: Azure SQL 数据仓库发行说明 | Microsoft Docs
description: Azure SQL 数据仓库发行说明。
services: sql-data-warehouse
author: mlee3gsd
ms.author: anumjs
ms.reviewer: jrasnick
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/09/2019
ms.openlocfilehash: 2ac60287c9d92ab6230e1dd6777504036e54492d
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2019
ms.locfileid: "56244668"
---
# <a name="azure-sql-data-warehouse-release-notes-and-documentation-updates"></a>Azure SQL 数据仓库发行说明和文档更新

Azure SQL 数据仓库 (SQL DW) 是基于云的企业数据仓库，可利用大规模并行处理 (MPP) 对数 PB 的数据快速运行复杂的查询。 将 SQL 数据仓库用作大数据解决方案的关键组件。 使用简单的 PolyBase T-SQL 查询将大数据导入 SQL 数据仓库，然后利用 MPP 运行高性能分析。 进行集成和分析时，数据仓库是企业获取见解能够依赖的唯一事实来源。

单击下面的链接可了解有关 Azure SQL 数据仓库的最新版本中可以预期的新功能和改进的更多信息。 你可以期望在确定的维护计划期间收到这些服务更新。

- [SQL 数据仓库版本 10.0.10106.0（一月）](./release-notes-10-0-10106-0.md)
- [2018 年 12 月](./release-notes-december-2018.md)
- [2018 年 10 月](./release-notes-october-2018.md)
- [2018 年 9 月](./release-notes-september-2018.md)
- [2018 年 8 月](./release-notes-august-2018.md)
- [2018 年 7 月](./release-notes-july-2018.md)
- [2018 年 6 月](./release-notes-june-2018.md)
- [2018 年 5 月](./release-notes-may-2018.md)

## <a name="checking-the-code-version-that-has-been-applied-to-your-data-warehouse"></a>检查已应用于数据仓库的代码版本

确认已将哪个版本应用于数据仓库。 通过 SSMS 连接到数据仓库，并运行以下语法以返回当前版本的 SQL 数据仓库。

```sql
SELECT @@VERSION AS 'SQL Data Warehouse';
```

示例输出：![SQL 数据仓库版本](./media/release-notes/dw-version.png)

请使用标识的版本确认已将哪个版本应用于 Azure SQL 数据仓库。 


## <a name="next-steps"></a>后续步骤
- [详细了解](https://docs.microsoft.com/azure/sql-data-warehouse/viewing-maintenance-schedule)如何查看维护计划。 
- [详细了解](https://docs.microsoft.com/azure/sql-data-warehouse/changing-maintenance-schedule)如何更改维护计划。
- [详细了解](https://docs.microsoft.com/azure/monitoring-and-diagnostics/alert-metric)如何使用 Azure Monitor 创建、查看和管理警报。
- [详细了解](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook)用于日志警报规则的 Webhook 操作。
- [详细了解](https://docs.microsoft.com/azure/service-health/service-health-overview) Azure 服务运行状况