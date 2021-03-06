---
title: 支持 Azure 资源托管标识的 Azure 服务
description: 支持 Azure 资源托管标识和 Azure AD 身份验证的服务列表
services: active-directory
author: priyamohanram
ms.author: priyamo
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: active-directory
ms.subservice: msi
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2aa25d2e0fe4700eab69d383d1ec8b57fca86bd4
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2019
ms.locfileid: "56201544"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>支持 Azure 资源托管标识的服务

Azure 资源的托管标识在 Azure Active Directory 中为 Azure 服务提供了一个自动托管标识。 使用托管标识，可对支持 Azure AD 身份验证的任何服务进行身份验证，而无需在代码中插入凭据。 我们正在跨 Azure 将 Azure 资源托管标识与 Azure AD 身份验证集成。 请经常再回来看看，确定这部分内容是否有更新。

> [!NOTE]
> Azure 资源托管标识是以前称为托管服务标识 (MSI) 的服务的新名称。

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>支持 Azure 资源托管标识的 Azure 服务

以下 Azure 服务支持 Azure 资源托管标识：

### <a name="azure-virtual-machines"></a>Azure 虚拟机

|托管标识类型 |  所有正式发布版<br>全球 Azure 区域 | Azure Government |Azure 德国|Azure 中国世纪互联|
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 预览 | 预览 | 预览 | 预览 |
| 用户分配 | 预览 | 预览 | 预览 | 预览 | 预览

请参阅以下列表来配置 Azure 虚拟机的托管标识（在可用的区域中）：

- [Azure 门户](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure 资源管理器模板](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-virtual-machine-scale-sets"></a>Azure 虚拟机规模集

|托管标识类型 |  所有正式发布版<br>全球 Azure 区域 | Azure Government |Azure 德国|Azure 中国世纪互联|
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 预览 | 预览 | 预览 |
| 用户分配 | 预览 | 预览 | 预览 | 预览

请参阅以下列表来配置 Azure 虚拟机规模集的托管标识（在可用的区域中）：

- [Azure 门户](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure 资源管理器模板](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-app-service"></a>Azure 应用服务

|托管标识类型 |  所有正式发布版<br>全球 Azure 区域 | Azure Government |Azure 德国|Azure 中国世纪互联|
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 可用 | 可用 | 可用 |
| 用户分配 | 预览 | 不可用 | 不可用 | 不可用

请参阅以下列表来配置 Azure 应用服务的托管标识（在可用的区域中）：

- [Azure 门户](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure 资源管理器模板](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-functions"></a>Azure Functions

托管标识类型 |  所有正式发布版<br>全球 Azure 区域 | Azure Government |Azure 德国|Azure 中国世纪互联|
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 可用 | 可用 | 可用 |
| 用户分配 | 预览 | 不可用 | 不可用 | 不可用

请参阅以下列表来配置 Azure Functions 的托管标识（在可用的区域中）：

- [Azure 门户](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure 资源管理器模板](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-logic-apps"></a>Azure 逻辑应用

托管标识类型 |  所有正式发布版<br>全球 Azure 区域 | Azure Government |Azure 德国|Azure 中国世纪互联|
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 可用 | 可用 | 可用 |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用

请参阅以下列表来配置 Azure 逻辑应用的托管标识（在可用的区域中）：

- [Azure 门户](/azure/logic-apps/create-managed-service-identity#azure-portal)
- [Azure 资源管理器模板](/azure/app-service/overview-managed-identity)

### <a name="azure-data-factory-v2"></a>Azure 数据工厂 V2

托管标识类型 |  所有正式发布版<br>全球 Azure 区域 | Azure Government |Azure 德国|Azure 中国世纪互联|
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 不可用 | 不可用 | 不可用 |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用

请参阅以下列表来配置 Azure 数据工厂 V2 的托管标识（在可用的区域中）：

- [Azure 门户](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)
- [PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)
- [REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)
- [SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk)

### <a name="azure-api-management"></a>Azure API 管理

托管标识类型 |  所有正式发布版<br>全球 Azure 区域 | Azure Government |Azure 德国|Azure 中国世纪互联|
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 可用 | 不可用 | 不可用 |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用

请参阅以下列表来配置 Azure API 管理的托管标识（在可用的区域中）：

- [Azure 资源管理器模板](/azure/api-management/api-management-howto-use-managed-service-identity)

### <a name="azure-container-instances"></a>Azure 容器实例

托管标识类型 |  所有正式发布版<br>全球 Azure 区域 | Azure Government |Azure 德国|Azure 中国世纪互联|
| --- | --- | --- | --- | --- |
| 系统分配 | Linux：预览<br>Windows:不可用 | 不可用 | 不可用 | 不可用 |
| 用户分配 | Linux：预览<br>Windows:不可用 | 不可用 | 不可用 | 不可用

请参阅以下列表来配置 Azure 容器实例的托管标识（在可用的区域中）：

- [Azure CLI](~/articles/container-instances/container-instances-managed-identity.md)
- [Azure 资源管理器模板](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)
- [YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file)


## <a name="azure-services-that-support-azure-ad-authentication"></a>支持 Azure AD 身份验证的 Azure 服务

以下服务支持 Azure AD 身份验证，已通过使用 Azure 资源托管标识的客户端服务进行测试。

| 服务 | 资源 ID | 状态 | 分配访问权限 |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure 资源管理器 | `https://management.azure.com/` | 可用 | [Azure 门户](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[Azure CLI](howto-assign-access-CLI.md) <br>[Azure 资源管理器模板](../../role-based-access-control/role-assignments-template.md) |
| Azure 密钥保管库 | `https://vault.azure.net` | 可用 |  
| Azure Data Lake | `https://datalake.azure.net/` | 可用 |
| Azure SQL | `https://database.windows.net/` | 可用 |
| Azure 事件中心 | `https://eventhubs.azure.net` | 预览 |
| Azure 服务总线 | `https://servicebus.azure.net` | 预览 |
| Azure 存储 | `https://storage.azure.com/` | 预览 |
