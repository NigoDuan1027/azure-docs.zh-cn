---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: cdebdf7258e99457191754cd73513fdb3744f8e9
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/15/2019
ms.locfileid: "56323423"
---
| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 每个[资源组](../articles/azure-resource-manager/resource-group-overview.md#resource-groups)的资源数（按资源类型） |800 |根据资源类型而异 |
| 部署历史记录中每个资源组的部署数 |800 |800 |
| 每个部署的资源数 |800 |800 |
| 管理锁数（按唯一的作用域） |20 |20 |
| 标记数（按资源或资源组） |15 |15 |
| 标记键长度 |512 |512 |
| 标记值长度 |256 |256 |


#### <a name="template-limits"></a>模板限制

| 值 | 默认限制 | 最大限制 |
| --- | --- | --- |
| parameters |256 |256 |
| 变量 |256 |256 |
| 资源（包括副本计数） |800 |800 |
| Outputs |64 |64 |
| 模板表达式 |24,576 个字符 |24,576 个字符 |
| 已导出模板中的资源 |200 |200 | 
| 模板大小 |1 MB |1 MB |
| 参数文件大小 |64 KB |64 KB |

通过使用嵌套模板，可超出某些模板限制。 有关详细信息，请参阅[部署 Azure 资源时使用链接的模板](../articles/azure-resource-manager/resource-group-linked-templates.md)。 若要减少参数、变量或输出的数量，可以将几个值合并为一个对象。 有关详细信息，请参阅[对象即参数](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md)。

如果达到每个资源组的部署数限制 800，则会从历史记录中删除不再需要的部署。 可以使用 Azure CLI 的 [az group deployment delete](/cli/azure/group/deployment) 或 PowerShell 中的 [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/remove-azurermresourcegroupdeployment) 删除历史记录中的条目。 从部署历史记录中删除条目不会影响部署资源。 