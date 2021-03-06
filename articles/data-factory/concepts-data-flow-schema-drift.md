---
title: Azure 数据工厂映射数据流架构偏差
description: 使用架构偏差在 Azure 数据工厂中生成可复原的数据流
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: 5a3d817f96770370615ff4ae953cb8655bd245c1
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/16/2019
ms.locfileid: "56329041"
---
# <a name="mapping-data-flow-schema-drift"></a>映射数据流架构偏差

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

“架构偏差”的概念是指源经常更改元数据。 可以动态添加、删除或更改字段、列和类型等。 如果不处理架构偏差，当上游数据源发生更改时，数据流就很容易发生更改。 当传入的列和字段发生更改时，典型的 ETL 模式将会失败，因为它们往往绑定到这些源名称。

为了防范架构偏差，必须在数据流工具中提供相应的机制，使数据工程师能够：

* 定义具有可变字段名称、数据类型、值和大小的源
* 定义可以使用数据模式而不是硬编码字段和值的转换参数
* 定义可以识别与传入字段相匹配而不是使用命名字段的表达式

在 Azure 数据工厂数据流中，可通过以下工作流查看这些机制：

* 在源转换中选择“允许架构偏差”

<img src="media/data-flow/schemadrift001.png" width="400">

* 选择此选项后，每次执行数据流都会从源读取所有传入字段，并通过整个流将其传递到接收器。

* 请务必使用“自动映射”来映射接收器转换中的所有新字段，以选取所有新字段并将其载入目标：

<img src="media/data-flow/automap.png" width="400">

* 使用简单的“源 -> 接收器”（即复制）映射在该方案中引入新字段后，一切都会正常工作。

* 若要在处理架构偏差的该工作流中添加转换，可以使用模式匹配来按名称、类型和值对列进行匹配。

* 若要创建一个可识别“架构偏差”的转换，请在“派生列”或“聚合”转换中单击“添加列模式”。

<img src="media/data-flow/columnpattern.png" width="400">

> [!NOTE]
> 需要在数据流中做出体系结构方面的决策，使整个流接受架构偏差。 这样做可以防范源中发生架构更改。 但是，整个数据流中列和类型的前期绑定将会丢失。 Azure 数据工厂将架构偏差流视为后期绑定流，因此，当你生成转换时，整个流的架构视图中不会显示列名。

<img src="media/data-flow/taxidrift1.png" width="400">

在出租车演示示例数据流中，包含 TripFare 源的底部数据流中有一个示例架构偏差。 请注意，在“聚合”转换中，我们将对聚合字段使用“列模式”设计。 我们不会命名特定的列或按位置查找列，而是假设每次运行后数据可能会更改并且可能不按相同的顺序显示。

在此 Azure 数据工厂数据流架构偏差处理示例中，我们已生成一个聚合用于扫描“double”类型的列，并知道数据域包含每段行程的价格。 然后，可以针对源中的所有 double 字段执行聚合数学计算，不管该列在哪个位置载入，也不管该列的命名如何。

Azure 数据工厂数据流语法使用 $$ 来表示匹配模式中每个匹配的列。 也可以使用复杂字符串搜索和正则表达式函数来按列名进行匹配。 在这种情况下，我们将根据“double”类型的列的每个匹配项创建新的聚合字段名称，并将文本 ```_total``` 追加到每个匹配的名称： 

```concat($$, '_total')```

然后，我们将舍入和累加每个匹配列的值：

```round(sum ($$))```

可以使用 Azure 数据工厂数据流示例“出租车演示”对此进行测试。 使用数据流设计图面顶部的“调试”切换开关打开调试会话，以交互方式查看结果：

<img src="media/data-flow/taxidrift2.png" width="800">

