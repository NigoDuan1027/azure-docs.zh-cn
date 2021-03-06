---
title: 改进分类器 - 自定义影像服务
titlesuffix: Azure Cognitive Services
description: 了解如何提高分类器的质量。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.openlocfilehash: 54faf8b37afa953aac7d411df2f539188dc5b451
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/15/2019
ms.locfileid: "56310203"
---
# <a name="how-to-improve-your-classifier"></a>如何改进分类器

本指南介绍如何提高自定义视觉服务分类器的质量。 分类器的质量取决于向其提供的已标记数据的数量、质量和种类，以及整个数据集的平衡程度。 一个好的分类器具有平衡的训练数据集，该数据集是将提交到分类器的代表数据集。 此类分类器的生成过程是迭代性的；通常需要进行几轮训练才能达到预期效果。

下面帮助你生成更准确分类器的通用模式：

1. 第一轮定型
1. 添加更多的图像并平衡数据；重新训练
1. 添加具有不同背景、照明、对象大小、相机角度和样式的图像；重新训练
1. 使用新图像来测试预测
1. 根据预测结果修改现有的训练数据

## <a name="overfitting"></a>过度拟合

有时，分类器将会根据图像中共有的任意特征来学习预测。 例如，创建苹果与柑橘的分类器时，如果使用了手中的苹果和白盘中的柑橘的图像，则分类器可能会过分注重于手与白盘，而不是苹果与柑橘。

![意外分类的图像](./media/getting-started-improving-your-classifier/unexpected.png)

若要解决此问题，请使用通过更多不同图像训练的以下指导：提供不同角度、背景、对象大小、组和其他差异的图像。

## <a name="data-quantity"></a>数据数量

训练图像的数目是最重要的因素。 我们建议一开始为每个标签至少使用 50 个图像。 使用更少的图像时，过度拟合的风险会提高，同时，性能数字可能会指出质量良好，但模型可能很难处理实际数据。 

## <a name="data-balance"></a>数据平衡

考虑训练数据的相对数量也很重要。 例如，对两个不同的标签分别使用 50 和 500 个图像会使训练数据集不平衡。 这使得模型对其中一个标签的预测准确度高于另一个标签。 如果在图像最少的标签与图像最多的标签之间至少保持 1:2 的比例，则可能会看到更好的结果。 例如，如果图像最多的标签有 500 个图像，则图像最少的标签应该至少有 250 个图像用于训练。

## <a name="data-variety"></a>数据多样性

请务必使用在正常使用期间提交到分类器的代表性图像。 否则，分类器可能会根据图像中共有的任意特征来学习预测。 例如，创建苹果与柑橘的分类器时，如果使用了手中的苹果和白盘中的柑橘的图像，则分类器可能会过分注重于手与白盘，而不是苹果与柑橘。

![意外分类的图像](./media/getting-started-improving-your-classifier/unexpected.png)

若要更正此问题，请包含多样化的图像，以确保分类器能够适当通用化。 以下是可使定型集更加多样化的一些方法：

* __背景：__ 在不同背景的前面提供对象的图像。 自然场景中的照片比中性背景前的照片更好，因为前者为分类器提供了更多信息。

    ![背景图像示例](./media/getting-started-improving-your-classifier/background.png)

* __照明：__ 提供不同照明下的图像（即在闪光、高曝光等设置下拍摄），尤其是在用于预测的图像具有不同照明的情况下。 使用不同饱和度、色调和亮度的图像也很有帮助。

    ![照明图像示例](./media/getting-started-improving-your-classifier/lighting.png)

* __对象大小：__ 提供具有不同大小和数量的对象的图像（例如，几串香蕉的照片和一根香蕉的特写照片）。 不同的大小有助于分类器涵盖所有方面。

    ![大小图像示例](./media/getting-started-improving-your-classifier/size.png)

* __照相机角度：__ 提供以不同照相机角度拍摄的图像。 或者，如果必须使用固定相机（例如监控摄像头）拍摄所有照片，请确保为每个定期出现的对象分配不同的标签，以避免过度拟合 &mdash; 将不相关的对象（例如灯柱）解释为关键特征。

    ![角度图像示例](./media/getting-started-improving-your-classifier/angle.png)

* __样式：__ 提供类相同、样式不同的图像（例如，不同品种的同一种水果）。 但是，如果对象的样式有很大的差异（例如，“米老鼠”与现实的老鼠），我们建议将它们标记为单独的类，以更好地表示其独特特征。

    ![样式图像示例](./media/getting-started-improving-your-classifier/style.png)

## <a name="use-prediction-images-for-further-training"></a>使用预测图像进一步训练

通过将图像提交到预测终结点来使用或测试图像分类器时，自定义视觉服务会存储这些图像。 然后，你可以使用这些图像来改进模型。

1. 若要查看提交到分类器的图像，请打开[自定义视觉网页](https://customvision.ai)，转到相应项目，然后选择“预测”选项卡。默认视图显示当前迭代的图像。 可使用“迭代”下拉菜单查看先前迭代期间提交的图像。

    ![预测选项卡的屏幕截图，视图中包含图像](./media/getting-started-improving-your-classifier/predictions.png)

2. 将鼠标悬停在图像上可查看分类器预测的标记。 图像将会排序，能够为分类器带来最大改善的图像列在顶部。 若要使用不同的排序方法，请在“排序”部分做出选择。 

    若要将某个图像添加到现有训练数据，请选择该图像，设置正确的标记，然后单击“保存并关闭”。 该图像将从“预测”中删除，并添加到训练图像集。 可选择“定型图像”选项卡查看该图像。

    ![“标记”页面的图像](./media/getting-started-improving-your-classifier/tag.png)

3. 然后使用“训练”按钮重新训练分类器。

## <a name="visually-inspect-predictions"></a>直观检查预测

若要检查图像预测，请转到“训练图像”选项卡，在“迭代”下拉菜单中选择前一个训练迭代，然后检查“标记”部分下的一个或多个标记。 现在，视图应会围绕其模型未能正确预测给定标记的每个图像显示一个红框。

![迭代历史记录的图像](./media/getting-started-improving-your-classifier/iteration.png)

有时，视觉检查可以识别模式，然后，你可通过添加更多训练数据或修改现有训练数据来纠正这些模式。 例如，苹果与酸橙的分类器可能会错误地将所有青苹果标记为酸橙。 可以通过添加并提供包含青苹果的已标记图像的训练数据来纠正此问题。

## <a name="next-steps"></a>后续步骤

本指南介绍了使自定义图像分类模型变得更准确的几种方法。 接下来，请了解如何通过将图像提交到预测 API 来以编程方式测试这些图像。

> [!div class="nextstepaction"]
[使用预测 API](use-prediction-api.md)
