---
title: Academic Graph 实体属性 - 学术知识 API
titlesuffix: Azure Cognitive Services
description: 了解可以与学术知识 API 中的 Academic Graph 结合使用的实体属性。
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: accc2803895f3892075cdd9877ca98344ab88bd1
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884811"
---
# <a name="entity-attributes"></a>实体属性

Academic Graph 包含 7 类实体。 所有实体都有实体 ID 和实体类型。

## <a name="common-entity-attributes"></a>常见实体属性
Name    |说明                |Type       | 操作
------- | ------------------------- | --------- | ----------------------------
ID      |实体 ID                  |Int64      |等于
Ty      |实体类型                |枚举   |等于

## <a name="entity-type-enum"></a>实体类型枚举
Name                                                            |值
----------------------------------------------------------------|-----
[论文](PaperEntityAttributes.md)                               |0
[作者](AuthorEntityAttributes.md)                             |1
[期刊](JournalEntityAttributes.md)                           |2
[会议录](JournalEntityAttributes.md)                 |3
[会议实例](ConferenceInstanceEntityAttributes.md)    |4
[隶属关系](AffiliationEntityAttributes.md)                   |5
[研究领域](FieldsOfStudyEntityAttributes.md)                      |6

