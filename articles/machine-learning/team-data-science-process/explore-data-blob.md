---
title: 使用 pandas 浏览 Azure blob 存储中的数据 - Team Data Science Process
description: 如何使用 pandas Python 包浏览存储在 Azure blob 容器中的数据。
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 1d76f9a76c85f4f3f052c3b409f5be28b51ebe91
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2019
ms.locfileid: "55459122"
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a>使用 pandas 浏览 Azure blob 存储中的数据

本文介绍如何使用 [pandas](http://pandas.pydata.org/) Python 包浏览存储在 Azure blob 容器中的数据。

此任务是[团队数据科学过程](overview.md)中的一个步骤。

## <a name="prerequisites"></a>先决条件
本文假设用户具备以下条件：

* 已创建 Azure 存储帐户。 如果需要说明，请参阅[创建 Azure 存储帐户](../../storage/common/storage-quickstart-create-account.md)
* 将数据存储在 Azure Blob 存储帐户中。 如果需要说明，请参阅[将数据移动到和移出 Azure 存储](../../storage/common/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>将数据加载到 pandas 数据帧
要浏览和操作数据集，首先必须从 blob 源将数据集下载到本地文件，然后将数据集加载到 pandas 数据帧。 下面是此过程的所需步骤：

1. 通过 blob 服务使用下方 Python 代码示例从 Azure blob 下载数据。 使用特定值替代下方代码中的变量： 
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))
2. 从下载的文件中将数据读入 pandas 数据帧。
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

现在可以准备浏览数据并在此数据集上生成功能了。

## <a name="blob-dataexploration"></a>使用 pandas 浏览数据的示例
下方举例说明了如何使用 pandas 浏览数据：

1. 检查**行数和列数** 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. 在下方数据集中**检查**前面或后面几**行**：
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. 使用如下示例代码检查每列导入的**数据类型**
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. 如下所示，检查数据中列的**基本统计信息**
   
        dataframe_blobdata.describe()
5. 如下所示，查看每列值的条目数
   
        dataframe_blobdata['<column_name>'].value_counts()
6. 使用下方示例代码计算每列中的**缺失值**与实际项目数
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. 如果数据中的特定列存在**缺失值**，可按如下方法进行替代：
   
     dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape
   
   另一种替代缺失值的方法是使用模式函数：
   
     dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        
8. 使用数量不定的量化创建**直方图**绘制出变量分布情况    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. 使用散点图或内置关联函数查看变量间的**关联**
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

