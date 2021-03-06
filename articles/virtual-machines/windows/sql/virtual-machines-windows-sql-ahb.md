---
title: 如何在 Azure 中更改 SQL Server VM 的许可模型 | Microsoft Docs
description: 了解如何在 Azure 中切换 SQL 虚拟机的许可。
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/14/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 8526716b299d26d8d70c9c5e5cdace34e188d019
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2019
ms.locfileid: "56111061"
---
# <a name="how-to-change-the-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>如何在 Azure 中更改 SQL Server 虚拟机的许可模型
本文介绍如何在 Azure 中使用新的 SQL VM 资源提供程序 **Microsoft.SqlVirtualMachine** 来更改 SQL Server 虚拟机的许可模型。 对于托管 SQL Server 的虚拟机 (VM) 来说，有两个许可模型 -“按使用情况付费”和“自带许可证(BYOL)”。 现在可以通过 PowerShell 或 Azure CLI 对 SQL Server VM 使用哪个许可模型进行修改。 

“按使用情况付费 (PAYG)”模型意味着 Azure VM 的每秒运行成本包括 SQL Server 许可证的费用。

“自带许可 (BYOL)”模型也称 [Azure 混合权益](https://azure.microsoft.com/pricing/hybrid-benefit/)，允许在运行 SQL Server 的 VM 中使用你自己的 SQL Server 许可证。 有关价格的详细信息，请参阅 [SQL Server VM 定价指南](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance)。

在两个许可证模型之间进行切换不会导致停机，不会重启 VM，不会增加费用（事实上，激活 AHB 会降低费用），并且会立即生效。 

  >[!NOTE]
  > - 当前，仅当从即用即付 SQL Server VM 映像开始时，才能使用转换许可模型的功能。 如果从门户首先使用自带许可映像，则无法将该映像转换为即用即付。 
  > - CSP 客户可通过首先部署即用即付 VM，然后将它转换为自带许可，来利用 AHB 权益。 


## <a name="prerequisites"></a>先决条件
使用 SQL VM 资源提供程序需要安装 SQL IaaS 扩展。 因此，若要继续利用 SQL VM 提供程序，需要：
- 一个 [Azure 订阅](https://azure.microsoft.com/free/)。
- 一个装有 [SQL IaaS 扩展](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension)的 [SQL Server VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision)。 


## <a name="register-existing-sql-server-vm-with-sql-resource-provider"></a>将现有 SQL Server VM 注册到 SQL 资源提供程序
在许可模型之间进行切换是新的 SQL VM 资源提供程序 (Microsoft.SqlVirtualMachine) 提供的一项功能。 在 2018 年 12 月之后部署的 SQL Server VM 会自动注册到新的资源提供程序。 但是，在此日期之前部署的现有 VM 需要手动注册到资源提供程序，然后它们才能切换许可模型。 

  > [!NOTE] 
  > 如果删除 SQL VM 资源，则会回退到映像的硬编码许可证设置。 

### <a name="register-sql-resource-provider-with-your-subscription"></a>将 SQL 资源提供程序注册到订阅 

若要向 SQL 资源提供程序注册 SQL Server VM，必须将资源提供程序注册到订阅。 可以使用 PowerShell 或 Azure 门户实现此目的。 

#### <a name="using-powershell"></a>使用 PowerShell
下面的代码片段将 SQL 资源提供程序注册到 Azure 订阅。 

```powershell
# Register the new SQL resource provider for your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```

#### <a name="using-azure-portal"></a>使用 Azure 门户
以下步骤使用 Azure 门户将 SQL 资源提供程序注册到 Azure 订阅。 

1. 打开 Azure 门户，导航到“所有服务”。 
1. 导航到“订阅”，选择感兴趣的订阅。  
1. 在“订阅”边栏选项卡中，导航到“资源提供程序”。 
1. 在筛选器中键入 `sql`，以便显示与 SQL 相关的资源提供程序。 
1. 根据所需操作为 **Microsoft.SqlVirtualMachine** 提供程序选择“注册”、“重新注册”或“取消注册”。 

  ![修改提供程序](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

### <a name="register-sql-server-vm-with-sql-resource-provider"></a>将 SQL Server VM 注册到 SQL 资源提供程序
将 SQL 资源提供程序注册到订阅之后，可以使用 PowerShell 将 SQL Server VM 注册到 SQL 资源提供程序。 


```powershell
# Register your existing SQL Server VM with the new resource provider
# example: $vm=Get-AzureRmVm -ResourceGroupName AHBTest -Name AHBTest
$vm=Get-AzureRmVm -ResourceGroupName <ResourceGroupName> -Name <VMName>
New-AzureRmResource -ResourceName $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines -Properties @{virtualMachineResourceId=$vm.Id}
```


## <a name="use-powershell"></a>使用 PowerShell 
可以使用 PowerShell 更改许可模型。  请确保在切换许可模型之前，SQL Server VM 已注册到新的 SQL 资源提供程序。 

以下代码片段将“按使用情况付费”许可证模型切换为“BYOL”（或者使用 Azure 混合权益）： 
```PowerShell
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="AHUB"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
``` 

以下代码片段将“BYOL”模型切换为“按使用情况付费”：
```PowerShell
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="PAYG"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
```

  >[!NOTE]
  > 若要在许可证之间进行切换，必须使用新的 SQL VM 资源提供程序。 如果你在将 SQL Server VM 注册到新提供程序之前尝试运行这些命令，可能会遇到以下错误：`Get-AzResource : The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set. ` 如果看到此错误，请将 SQL Server VM 注册到新的资源提供程序。 

 

## <a name="use-azure-cli"></a>使用 Azure CLI
可以通过 Azure CLI 更改许可模型。  请确保在切换许可模型之前，SQL Server VM 已注册到新的 SQL 资源提供程序。 

以下代码片段将“按使用情况付费”许可证模型切换为“BYOL”（或者使用 Azure 混合权益）：
```azurecli
az resource update -g <resource_group_name> -n <sql_virtual_machine_name> --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=AHUB
# example: az resource update -g AHBTest -n AHBTest --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=AHUB
```

以下代码片段将“BYOL”模型切换为“按使用情况付费”： 
```azurecli
az resource update -g <resource_group_name> -n <sql_virtual_machine_name> --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=PAYG
# example: az resource update -g AHBTest -n AHBTest --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=PAYG
```

  >[!NOTE]
  >若要在许可证之间进行切换，必须使用新的 SQL VM 资源提供程序。 如果你在将 SQL Server VM 注册到新提供程序之前尝试运行这些命令，可能会遇到以下错误：`The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. ` 如果你看到此错误，请[将 SQL Server VM 注册到新的资源提供程序](#register-existing-sql-server-vm-with-sql-resource-provider)。 

## <a name="view-current-licensing"></a>查看当前许可 

可以使用以下代码片段查看 SQL Server VM 的当前许可模型。 

```PowerShell
# View current licensing model for your SQL Server VM
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType
```

## <a name="known-errors"></a>已知错误

### <a name="sql-iaas-extension-is-not-installed-on-virtual-machine"></a>虚拟机上未安装 SQL IaaS 扩展
必须安装 SQL IaaS 扩展才能将 SQL Server VM 注册到 SQL VM 资源提供程序。 如果尝试在安装 SQL IaaS 扩展之前注册 SQL Server VM，将遇到以下错误：

`Sql IaaS Extension is not installed on Virtual Machine: '{0}'. Please make sure it is installed and in running state and try again later.`

若要解决此问题，请在尝试注册 SQL Server VM 之前先安装 SQL IaaS 扩展。 

  > [!NOTE]
  > 安装 SQL IaaS 扩展会重启 SQL Server 服务，请只在维护时段进行安装。 有关详细信息，请参阅 [SQL IaaS 扩展安装](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension#installation)。 

### <a name="cannot-validate-argument-on-parameter-sku"></a>无法验证参数“Sku”中的自变量
尝试使用 4.0 以上版本的 Azure PowerShell 更改 SQL Server VM 许可模型时，可能会遇到以下错误：

`Set-AzResource : Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

若要解决此错误，请在切换许可模型时，取消注释上述 PowerShell 代码片段中的以下行： 
```PowerShell
# the following code snippet is necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new()
```

使用以下代码验证 Azure PowerShell 版本：

```PowerShell
Get-Module -ListAvailable -Name Azure -Refresh
```

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅以下文章： 

* [Windows VM 上的 SQL Server 概述](virtual-machines-windows-sql-server-iaas-overview.md)
* [Windows VM 上的 SQL Server 常见问题解答](virtual-machines-windows-sql-server-iaas-faq.md)
* [Windows VM 上的 SQL Server 定价指南](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Windows VM 上的 SQL Server 发行说明](virtual-machines-windows-sql-server-iaas-release-notes.md)


