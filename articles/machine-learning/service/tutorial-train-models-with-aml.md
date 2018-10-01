---
title: 教程：使用 Azure 机器学习定型图像分类模型
description: 了解如何使用 Python Jupyter Notebook 定型 scikit-learn 图像分类模型。 本教程是由两个部分构成的系列教程的第一部分。
author: hning86
ms.author: haining
ms.topic: conceptual
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: bed4abcce3019607715416b5194a2ddecc89b76a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966605"
---
# <a name="tutorial-1-train-an-image-classification-model-with-azure-machine-learning"></a>教程 #1：使用 Azure 机器学习定型图像分类模型

在本教程中，你将在本地和远程计算资源上定型机器学习模型。 将在 Python Jupyter Notebook中使用 Azure 机器学习服务的定型和部署工作流。  然后可以将 Notebook 用作模板，使用你自己的数据来定型机器学习。 本教程是由两个部分构成的系列教程的第一部分。  

本教程将 [MNIST](http://yann.lecun.com/exdb/mnist/) 数据集和 [scikit-learn](http://scikit-learn.org) 与 Azure 机器学习配合使用来定型简单的逻辑回归。  MNIST 是包含 70,000 张灰度图像的常用数据集。 每个图像是 28x28 像素的手写数字，代表一个从 0 到 9 的数字。 目标是创建多类分类器，以确定给定图像代表的数字。 

了解如何：

> [!div class="checklist"]
> * 设置开发环境
> * 访问和检查数据
> * 使用常用的 scikit-learn 机器学习库在本地定型简单的逻辑回归 
> * 在远程群集上定型多个模型
> * 查看定型结果，然后注册最佳模型

稍后，你将在[本教程的第二部分](tutorial-deploy-models-with-aml.md)学习如何选择模型并对其进行部署。 

如果还没有 Azure 订阅，可以在开始前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="get-the-notebook"></a>获取 Notebook

为方便起见，本教程以 Jupyter Notebook 的形式提供。 使用以下任一方法运行 `tutorials/01.train-models.ipynb` Notebook：

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]

## <a name="set-up-your-development-environment"></a>设置开发环境

开发工作的所有设置都可以在 Python Notebook 中完成。  设置包括：

* 导入 Python 程序包
* 连接一个工作区以启用本地计算机和远程资源之间的通信
* 创建一个试验来跟踪所有运行
* 创建要用于定型的远程计算目标

### <a name="import-packages"></a>导入包

导入在此会话中所需的 Python 包。 此外显示 Azure 机器学习 SDK 版本。

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt

import azureml
from azureml.core import Workspace, Run

# check core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="connect-to-workspace"></a>连接到工作区

从现有工作区创建工作区对象。 `Workspace.from_config()` 读取文件 config.json 并将详细信息加载到一个名为 `ws` 的对象。

```python
# load workspace configuration from the config.json file in the current folder.
ws = Workspace.from_config()
print(ws.name, ws.location, ws.resource_group, ws.location, sep = '\t')
```

### <a name="create-experiment"></a>创建试验

创建一个试验来跟踪工作区中的所有运行。  

```python
experiment_name = 'sklearn-mnist'

from azureml.core import Experiment
exp = Experiment(workspace=ws, name=experiment_name)
```

### <a name="create-remote-compute-target"></a>创建远程计算目标

Azure Batch AI 是一项托管的服务，可以让数据科学家在 Azure 虚拟机（包括带 GPU 支持的 VM）群集上定型机器学习模型。  在本教程中，创建 Azure Batch AI 群集作为定型环境。 如果工作区中尚且没有群集，此代码将创建一个群集。 

 创建群集需要大约 5 分钟。 如果群集已在工作区中，此代码将使用它，并跳过创建过程。


```python
from azureml.core.compute import ComputeTarget, BatchAiCompute
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
batchai_cluster_name = "traincluster"

try:
    # look for the existing cluster by name
    compute_target = ComputeTarget(workspace=ws, name=batchai_cluster_name)
    if compute_target is BatchAiCompute:
        print('found compute target {}, just use it.'.format(batchai_cluster_name))
    else:
        print('{} exists but it is not a Batch AI cluster. Please choose a different name.'.format(batchai_cluster_name))
except ComputeTargetException:
    print('creating a new compute target...')
    compute_config = BatchAiCompute.provisioning_configuration(vm_size="STANDARD_D2_V2", # small CPU-based VM
                                                                #vm_priority='lowpriority', # optional
                                                                autoscale_enabled=True,
                                                                cluster_min_nodes=0, 
                                                                cluster_max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, batchai_cluster_name, compute_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it uses the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
    # Use the 'status' property to get a detailed status for the current cluster. 
    print(compute_target.status.serialize())
```

现在已有所需的包和计算资源，可以在云中定型模型。 

## <a name="explore-data"></a>浏览数据

对模型进行定型之前，需要了解用于定型的数据。  此外需要将数据复制到云，以便云定型环境来访问该数据。  本部分介绍以下操作：

* 下载 MNIST 数据集
* 显示一些示例图像
* 将数据上传到云

### <a name="download-the-mnist-dataset"></a>下载 MNIST 数据集

下载 MNIST 数据集，并将文件保存到本地 `data` 目录。  下载用于定型和测试的图像和标签。  


```python
import os
import urllib.request

os.makedirs('./data', exist_ok = True)

urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename='./data/train-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename='./data/train-labels.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename='./data/test-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename='./data/test-labels.gz')
```

### <a name="display-some-sample-images"></a>显示一些示例图像

将压缩文件加载到 `numpy` 数组。 然后，使用 `matplotlib` 从数据集随意绘制 30 张图像，并在上方附加标签。



```python
# make sure utils.py is in the same directory as this code
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the model converge faster.
X_train = load_data('./data/train-images.gz', False) / 255.0
y_train = load_data('./data/train-labels.gz', True).reshape(-1)

X_test = load_data('./data/test-images.gz', False) / 255.0
y_test = load_data('./data/test-labels.gz', True).reshape(-1)

# now let's show some randomly chosen images from the traininng set.
count = 0
sample_size = 30
plt.figure(figsize = (16, 6))
for i in np.random.permutation(X_train.shape[0])[:sample_size]:
    count = count + 1
    plt.subplot(1, sample_size, count)
    plt.axhline('')
    plt.axvline('')
    plt.text(x=10, y=-10, s=y_train[i], fontsize=18)
    plt.imshow(X_train[i].reshape(28, 28), cmap=plt.cm.Greys)
plt.show()
```

随机图像示例显示：

![随机图像示例](./media/tutorial-train-models-with-aml/digits.png)

现在你已了解这些图像的外观和预期预测结果。

### <a name="upload-data-to-the-cloud"></a>将数据上传到云

现在，通过将数据从本地计算机上传到云，使数据可以远程访问，以便远程定型时可以访问这些数据。 数据存储是与你的工作区相关联的方便构造，用于上传/下载数据，并与远程计算目标交互。 

MNIST 文件被上传到数据存储根目录下一个名为 `mnist` 的目录。

```python
ds = ws.get_default_datastore()
print(ds.datastore_type, ds.account_name, ds.container_name)

ds.upload(src_dir='./data', target_path='mnist', overwrite=True, show_progress=True)
```
现在你已经拥有开始定型模型所需的一切条件。 

## <a name="train-a-model-locally"></a>在本地定型模型

从 scikit-learn 在本地定型简单的逻辑回归模型。

**本地定型可能需要一两分钟**，具体取决于计算机配置。

```python
%%time
from sklearn.linear_model import LogisticRegression

clf = LogisticRegression()
clf.fit(X_train, y_train)
```

接下来，使用测试集进行预测，并计算准确性。 

```python
y_hat = clf.predict(X_test)
print(np.average(y_hat == y_test))
```

本地模型准确性显示：

`0.9202`

只需几行代码，便可以获得 92%的准确性。

## <a name="train-on-a-remote-cluster"></a>在远程群集上定型

现在可以通过使用不同正则化率构建一个模型来扩展此简单模型。 这一次将在远程资源上定型模型。  

对于此任务，将作业提交到之前设置的远程定型群集。  若要提交作业：
* 创建目录
* 创建定型脚本
* 创建估算器
* 提交作业 

### <a name="create-a-directory"></a>创建目录

创建一个目录，将所需的代码从计算机发送到远程资源。

```python
import os
script_folder = './sklearn-mnist'
os.makedirs(script_folder, exist_ok=True)
```

### <a name="create-a-training-script"></a>创建定型脚本

若要将作业提交到群集，首先创建定型脚本。 运行以下代码，以在刚创建的目录中创建名为 `train.py` 的定型脚本。 此定型将正则化率添加到定型算法，以此会生成与本地版本略有不同的模型。

```python
%%writefile $script_folder/train.py

import argparse
import os
import numpy as np

from sklearn.linear_model import LogisticRegression
from sklearn.externals import joblib

from azureml.core import Run
from utils import load_data

# let user feed in 2 parameters, the location of the data files (from datastore), and the regularization rate of the logistic regression model
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = os.path.join(args.data_folder, 'mnist')
print('Data folder:', data_folder)

# load train and test set into numpy arrays
# note we scale the pixel intensity values to 0-1 (by dividing it with 255.0) so the model can converge faster.
X_train = load_data(os.path.join(data_folder, 'train-images.gz'), False) / 255.0
X_test = load_data(os.path.join(data_folder, 'test-images.gz'), False) / 255.0
y_train = load_data(os.path.join(data_folder, 'train-labels.gz'), True).reshape(-1)
y_test = load_data(os.path.join(data_folder, 'test-labels.gz'), True).reshape(-1)
print(X_train.shape, y_train.shape, X_test.shape, y_test.shape, sep = '\n')

# get hold of the current run
run = Run.get_submitted_run()

print('Train a logistic regression model with regularizaion rate of', args.reg)
clf = LogisticRegression(C=1.0/args.reg, random_state=42)
clf.fit(X_train, y_train)

print('Predict the test set')
y_hat = clf.predict(X_test)

# calculate accuracy on the prediction
acc = np.average(y_hat == y_test)
print('Accuracy is', acc)

run.log('regularization rate', np.float(args.reg))
run.log('accuracy', np.float(acc))

os.makedirs('outputs', exist_ok=True)
# note file saved in the outputs folder is automatically uploaded into experiment record
joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')
```

请注意该脚本获取数据和保存模型的方式：

+ 定型脚本读取参数以查找包含数据的目录。  稍后提交作业时，请参考数据存储获取此参数：`parser.add_argument('--data-folder', type = str, dest = 'data_folder', help = 'data directory mounting point')`

    
+ 定型脚本将模型保存到一个名为“输出”的目录。 <br/>
`joblib.dump(value = clf, filename = 'outputs/sklearn_mnist_model.pkl')`<br/>
此目录中编写的所有内容都会自动上传到你的工作区。 你将在本教程后面从此目录访问模型。

从定型脚本中引用文件 `utils.py` 来正确加载数据集。  将此脚本复制到脚本文件夹，以便可以与远程资源上的定型脚本一起访问。


```python
import shutil
shutil.copy('utils.py', script_folder)
```


### <a name="create-an-estimator"></a>创建估算器

估算器对象用于提交运行。  通过运行以下代码创建估算器以定义：

* 估算器对象的名称，`est`
* 包含脚本的目录。 此目录中的所有文件都上传到群集节点以便执行。 
* 计算目标。  在本例中，将使用你创建的 Batch AI 群集
* 定型脚本名称，train.py
* 定型脚本所需的参数 
* 定型所需的 Python 包

在本教程中，此目标是 Batch AI 群集。 此项目目录中的所有文件都上传到群集节点以便执行。 data_folder 设置为使用数据存储 (`ds.as_mount()`)。

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

est = Estimator(source_directory=script_folder,
                script_params=script_params,
                compute_target=compute_target,
                entry_script='train.py',
                conda_packages=['scikit-learn'])
```


### <a name="submit-the-job-to-the-cluster"></a>将作业提交到群集

通过提交估算器对象来运行此试验。

```python
run = exp.submit(config=est)
run
```

由于调用是异步形式，启动作业时它将返回“正在运行”状态。

## <a name="monitor-a-remote-run"></a>监视远程运行

总的来说，首次运行需要大约 10 分钟。 但对于后续运行，只要不更改脚本依赖项，将重复使用相同的映像，因此容器启动时间要快得多。

下面是等待时发生的情况：

- 映像创建：将创建与估算器指定的 Python 环境相匹配的 Docker 映像。 映像将上传到工作区。 创建和上传映像需要大约 5 分钟。 

  此阶段针对每个 Python 环境发生一次，因为容器已缓存用于后续运行。  映像创建期间，日志流式传输到运行历史记录。 可以使用这些日志监视映像创建进度。

- 缩放：如果远程群集需要比当前可用的节点更多的节点，则会自动添加其他节点。 缩放通常需要大约 5 分钟。

- 运行：在此阶段，将所需的脚本和文件发送到计算目标，装载/复制数据存储，然后运行 entry_script。 运行作业时，stdout 和 ./logs 目录将流式传输到运行历史记录。 可以使用这些日志监视运行进度。

- 后期处理：运行的 /outputs 目录将复制到工作区中的运行历史记录，以便可以访问这些结果。


可以通过多种方式查看正在运行的作业的进度。 本教程使用 Jupyter 小组件以及 `wait_for_completion` 方法。 

### <a name="jupyter-widget"></a>Jupyter 小组件

查看使用 Jupyter 小组件的运行进度。  和运行提交一样，该小组件采用异步方式，并每隔 10-15 秒提供实时更新，直到作业完成。


```python
from azureml.train.widgets import RunDetails
RunDetails(run).show()
```

以下仍是定型结束时显示的小组件的快照：

![Notebook 小组件](./media/tutorial-train-models-with-aml/widget.png)

### <a name="get-log-results-upon-completion"></a>完成时获取日志结果

模型定型和监视在后台发生。 在运行更多代码之前，请耐心等待，直到该模型完成定型。 使用 `wait_for_completion` 显示模型定型过程何时完成。 


```python
run.wait_for_completion(show_output=False) # specify True for a verbose log
```

### <a name="display-run-results"></a>显示运行结果

现在你拥有一个在远程群集上定型的模型。  检索模型的准确性：

```python
print(run.get_metrics())
```
输出显示远程模型的准确性要略高于本地模型，因为在定型期间增加了正则化率。  

`{'regularization rate': 0.8, 'accuracy': 0.9204}`

在部署教程中，将了解有关此模型的更多详细信息。

## <a name="register-model"></a>注册模型

定型脚本的最后一步在执行作业的群集 VM 内名为 `outputs` 的目录中编写文件 `outputs/sklearn_mnist_model.pkl`。 `outputs` 是一个专门目录，此目录中的所有内容都会自动都上传到工作区。  此内容在工作区下试验的运行记录中显示。 因此，模型文件现还在工作区中提供。

可以看到与运行关联的文件。

```python
print(run.get_file_names())
```

在工作区中注册模型，以便你（或其他协作者）可以在后面查询、检查和部署该模型。

```python
# register model 
model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
print(model.name, model.id, model.version, sep = '\t')
```

## <a name="clean-up-resources"></a>清理资源

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

此外可以只删除 Azure 托管的计算群集。 但是，由于自动缩放已启用，并且群集最小值为 0，在不使用时，此特定资源不会产生额外的计算费用。


```python
# optionally, delete the Azure Managed Compute cluster
compute_target.delete()
```

## <a name="next-steps"></a>后续步骤

在本 Azure 机器学习教程中，已使用 Python 执行以下操作：

> [!div class="checklist"]
> * 设置开发环境
> * 访问和检查数据
> * 使用常用的 scikit-learn 机器学习库在本地定型简单的逻辑回归
> * 在远程群集上定型多个模型
> * 查看定型详细信息，然后注册最佳模型

可以使用本系列教程的下一部分中的说明来部署此注册模型：

> [!div class="nextstepaction"]
> [教程 2 - 部署模型](tutorial-deploy-models-with-aml.md)