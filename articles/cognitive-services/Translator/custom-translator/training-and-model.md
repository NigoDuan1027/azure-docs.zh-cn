---
title: 什么是培训和模型？ - 自定义翻译
titleSuffix: Azure Cognitive Services
description: 模型是为特定语言对提供翻译的系统。 成功培训的结果即为一种模型。 在培训模型时，需要三种互斥的数据集：培训数据集、调整数据集和测试数据集。
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: v-rada
ms.openlocfilehash: e681d40a56bc481fce5a114513a579465ba07c85
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2018
ms.locfileid: "51626704"
---
# <a name="what-are-trainings-and-models"></a>什么是培训和模型？

模型是为特定语言对提供翻译的系统。
成功培训的结果即为一种模型。 在培训模型时，需要三种互斥的数据集：培训数据集、调整数据集和测试数据集。 还可以提供字典数据。

如果在对培训进行排队时仅提供培训数据，则自定义翻译将自动组合调整和测试数据集。 它将从培训数据中排除 5000 个句子，并各自使用 2500 个句子来组合调整和测试集。

## <a name="training-dataset-for-custom-translator"></a>自定义翻译的培训数据集

培训集中包含的文档由自定义翻译用作构建模型的基础。 在培训执行期间，这些文档中存在的句子是对齐的（或配对的）。 可以随意修改培训文档集。 可以在一个模型中包含你认为具有切线相关性的文档。 此外，在另一个模型中排除它们，以查看 [BLEU（双语评估研究）评分](what-is-bleu-score.md)中的影响。 只要保持调整集和测试集不变，就可以随意更改培训集的组成内容。 这种方法是修改翻译系统质量的一种有效方法。

可以在一个项目中运行多个培训，并跨所有培训运行比较 [BLEU 评分](what-is-bleu-score.md)。 当运行多个培训以进行比较时，请确保每次都指定相同的调整/测试数据。 另外，请务必在[“测试”](how-to-view-system-test-results.md)选项卡中手动检查结果。

## <a name="tuning-dataset-for-custom-translator"></a>自定义翻译的调整数据集

自定义翻译使用此集中包含的并行文档来调整翻译系统以获得最佳结果。

在培训期间使用调整集来将翻译系统的所有参数和权重调整到最佳值。 仔细选择调整集：调整集应代表你在将来打算翻译的文档的内容。 调整集对所产生的翻译的质量具有重大影响。 调整使翻译系统能够提供最接近在调整数据集中提供的样本的翻译。 调整集无需超过 2500 个句子。 为了获得最佳的翻译质量，建议通过选择最具代表性的句子选项来手动选择调整集。

创建调整集时，请选择将要翻译的有意义且有代表性长度的句子。 还应选择一些具有你要翻译的单词和短语的句子，这些单词和短语预期大致分布于你未来的翻译中。 在实践中，句子长度为 8 到 18 个单词将产生最佳结果，因为这些句子包含足够的上下文来显示变形并提供重要的短语长度，而不会过于复杂。

在调整集中使用的句子类型的一个不错描述是散文：实际流畅的句子。 不是表格单元格，不是诗歌，不是事物列表，不仅是标点符号，或句子中的数字 - 常规语言。

如果手动选择调整数据集，则它不应与培训和测试数据具有任何相同的句子。 调整集对翻译质量具有重大影响 - 请仔细选择句子。

如果不确定要为调整集选择什么，只需选择培训集并让自定义翻译为你选择调整集。 在让自定义翻译自动选择调整集时，它将使用双语培训文档中的随机句子子集，并从培训材料本身中排除这些句子。

## <a name="testing-dataset-for-custom-translator"></a>自定义翻译的测试数据集

测试集中包括的并行文档用于计算 BLEU（双语评估研究）评分。 此评分表示翻译系统的质量。 此评分实际告知翻译系统在此次训练后提供的翻译，与测试数据集中的参考句子之间的近似程度。

BLEU 评分是自动翻译与参考翻译之间的差值的度量。 其值范围为 0 到 100。 得分为 0 表示翻译中没有出现单个参考词。 得分为 100 表示自动翻译与参考完全匹配：相同的单词位于完全相同的位置。 你收到的分数是测试集中所有句子的 BLEU 分数平均值。

测试集应该包括并行文档，其中目标语言句子是对应的源语言句子在对中最理想的翻译。 你可能想要使用与编写调整集相同的标准。 但是，测试集对翻译系统的质量没有任何影响。 它专门用于生成 BLEU 分数，而不是其他任何内容。

测试集无需超过 2500 个句子。 在让系统自动选择测试集时，它将使用双语培训文档中的随机句子子集，并从培训材料本身中排除这些句子。

可以通过导航到模型中的测试选项卡，查看测试集的自定义翻译，并将它们与测试集中提供的翻译进行比较。