---
title: 温度预生成实体
titleSuffix: Azure
description: 本文包含了语言理解 (LUIS) 中的温度预构建实体信息。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 3629dc96fbb86236888014d9de9a7b7b3d86c298
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2019
ms.locfileid: "55867386"
---
# <a name="temperature-prebuilt-entity-for-a-luis-app"></a>LUIS 应用的温度预生成实体
“温度”提取了各种温度类型。 此实体已定型，因此不需要将包含温度的陈述示例添加到应用程序中。 [许多语言区域](luis-reference-prebuilt-entities.md)都支持存在温度实体。 

## <a name="types-of-temperature"></a>温度类型
温度托管在 [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819) GitHub 存储库中

## <a name="resolution-for-prebuilt-temperature-entity"></a>预构建温度实体的解析
以下示例显示了 **builtin.temperature** 实体的解析。

```json
{
  "query": "set the temperature to 30 degrees",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.85310787
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.85310787
    }
  ],
  "entities": [
    {
      "entity": "30 degrees",
      "type": "builtin.temperature",
      "startIndex": 23,
      "endIndex": 32,
      "resolution": {
        "unit": "Degree",
        "value": "30"
      }
    }
  ]
}
```

## <a name="next-steps"></a>后续步骤

了解[百分比](luis-reference-prebuilt-percentage.md)、[数字](luis-reference-prebuilt-number.md)和[存在时间](luis-reference-prebuilt-age.md)实体。 
