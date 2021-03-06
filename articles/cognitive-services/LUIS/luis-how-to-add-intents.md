---
title: 添加意向
titleSuffix: Language Understanding - Azure Cognitive Services
description: 向 LUIS 应用添加意向，识别具有相同意向的问题或命令组。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: e6ab51d568c950ca7fd503150557df288d84174f
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2019
ms.locfileid: "55867726"
---
# <a name="add-intents-to-determine-user-intention-of-utterances"></a>添加意向以确定用户的话语意向

向 LUIS 应用添加[意向](luis-concept-intent.md)，识别具有相同意向的问题或命令组。 

可以依次转到顶部导航栏的“生成”部分和左侧面板的“意向”来管理意向。 

## <a name="add-intent"></a>添加意向

1. 在“意向”页上，选择“创建新意向”。

1. 在“创建新意向”对话框中，输入意向名称 `GetEmployeeInformation`，然后单击“完成”。

    ![添加意向](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

## <a name="add-an-example-utterance"></a>添加示例话语

示例表述是用户问题或命令的文本示例。 若要传授语言理解 (LUIS)，需将示例话语添加到意向。

1. 在“GetEmployeeInformation”意向详细信息页上的意向名称下的文本框中，输入预期用户会输入的相关话语，例如 `Does John Smith work in Seattle?`，然后按 Enter。
 
    ![意向详细信息页的屏幕截图，其中突出显示了表述](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

    LUIS 会将所有话语转换为小写，并在连字符等标记的两侧添加空格。

## <a name="intent-prediction-discrepancy-errors"></a>意向预测偏差错误 

意向中的话语可能在所选意向和预测分数之间存在意向预测偏差。 LUIS 通过用红框围住示例话语行上的“已标记意向”来指示这种偏差。 

![意向详细信息页的屏幕截图，其中包含表述预测差异错误](./media/luis-how-to-add-intents/prediction-discrepancy-intent.png) 

在顶部导航栏中，选择“训练”。 预测偏差现已消失。

## <a name="add-a-custom-entity"></a>添加自定义实体

将话语添加到意向后，可以选择话语内部的文本来创建自定义实体。 通过自定义实体可以标记要提取的文本，以及正确的意向。 

1. 在话语中选择单词 `Seattle`。 系统会使用方括号括住文本，并显示一个下拉菜单。 

    ![在“意向详细信息”页上创建自定义实体的屏幕截图](./media/luis-how-to-add-intents/create-custom-entity.png) 

    此示例选择了要标记为实体的单个单词。 可将单个单词或短语标记为实体。

1. 在菜单的顶部文本框中输入 `Location`，然后选择“创建新实体”。 

    ![在“意向详细信息”页上创建自定义实体名称的屏幕截图](./media/luis-how-to-add-intents/create-custom-entity-name.png) 

1. 在用于创建实体的“要创建哪种类型的实体?”弹出窗口中，验证“实体名称”是否为“位置”，“实体类型”是否为“简单”。 选择“完成”。

## <a name="entity-prediction-discrepancy-errors"></a>实体预测偏差错误 

带红色下划线的实体表示存在[实体预测偏差](luis-how-to-add-example-utterances.md#entity-status-predictions)。 由于这是第一个出现的实体，LUIS 并未获得足够的示例，因此无法十分确信已使用正确的实体标记了此文本。 训练应用时，这种偏差将会消除。 

![“意向详细信息”页的屏幕截图，其中以蓝色突出显示了自定义实体名称](./media/luis-how-to-add-intents/create-custom-entity-name-blue-highlight.png) 

文本以蓝色突出显示，表示它是一个实体。  

## <a name="add-a-prebuilt-entity"></a>添加预构建实体

有关信息，请参阅[预生成实体](luis-how-to-add-entities.md#add-prebuilt-entity)。

## <a name="using-the-contextual-toolbar"></a>使用上下文工具栏

在列表中选择一个或多个示例话语后，可以选中某个话语左侧的框，使用话语列表上方的工具栏执行以下操作：

* 重新分配意向：将话语移到不同的意向
* 删除话语
* 实体筛选器：仅显示包含已筛选实体的话语
* 全部显示/仅显示错误：显示存在预测错误的话语，或显示所有话语
* 实体/标记视图：显示包含实体名称的实体视图，或显示话语的原始文本
* 放大镜：搜索包含特定文本的话语

## <a name="working-with-an-individual-utterance"></a>使用单个话语

可以通过话语右侧的省略号菜单针对单个话语执行以下操作：

* 编辑：更改话语的文本
* 删除：从意向中删除话语。 如果仍想保留该话语，则更好的方法是将其移到 **None** 意向。 
* 添加模式：使用模式可以提取常用话语以及标记可替换的文本和可忽略的文本，从而减少在意向中提供更多话语的需要。 

使用“已标记的意向”列可以更改话语的意向。

## <a name="train-your-app-after-changing-model-with-intents"></a>在对模型进行意向更改后对应用进行培训

添加、编辑或删除意向后，请[训练](luis-how-to-train.md)并[发布](luis-how-to-publish-app.md)应用，以便将更改应用到终结点查询。 

## <a name="next-steps"></a>后续步骤

详细了解如何添加包含实体的[示例话语](luis-how-to-add-example-utterances.md)。 
