---
title: 快速入门 - 使用 Azure 空间定位点创建 HoloLens 应用 | Microsoft Docs
description: 此快速入门介绍如何使用空间定位点生成 HoloLens 应用。
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: a0e34ad8847ed3740af72b4c27dfbc0090cf3dfa
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/24/2019
ms.locfileid: "56752457"
---
# <a name="quickstart-create-a-hololens-app-with-azure-spatial-anchors-in-cwinrt-and-directx"></a>快速入门：在 C++/WinRT 和 DirectX 中使用 Azure 空间定位点创建 HoloLens 应用

此快速指南介绍如何在 C++/WinRT 和 DirectX 中使用 [Azure 空间定位点](../overview.md)创建 HoloLens 应用。 Azure 空间定位点是一种跨平台开发人员服务。通过该服务，可以使用能够随时间推移跨设备保存自己位置的对象创建混合的现实体验。 完成后，将获得一个 HoloLens 应用，该应用可以保存和重新调用空间定位点。

将了解如何执行以下操作：

> [!div class="checklist"]
> * 创建空间定位点帐户
> * 配置空间定位点帐户标识符和帐户密钥
> * 在 HoloLens 设备上部署和运行

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>先决条件

若要完成本快速入门，请确保具备以下项：

- 具有通用 Windows 平台开发工作负载和 Windows 10 SDK（10.0.17763.0 或更新版本）组件，且安装了 <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017+</a> 的 Windows 计算机。
- 适用于 Visual Studio 的 [C++/WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix) 应从 [Visual Studio Marketplace](https://marketplace.visualstudio.com/) 安装。
- 启用了[开发人员模式](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio)的 HoloLens 设备。 本文需要包含 [Windows 2018 年 10 月 10 日更新](https://blogs.windows.com/windowsexperience/2018/10/02/find-out-whats-new-in-windows-and-office-in-october/)（也称为 RS5）的 HoloLens 设备。 要在 HoloLens 上更新为最新版本，请打开“设置”应用，转到“更新和安全”，然后选择“检查更新”按钮。
- 应用必须在其 AppX 清单中设置 spatialPerception 功能。

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>打开示例项目

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

在 Visual Studio 中打开 `HoloLens\DirectX\SampleHoloLens.sln`。

## <a name="configure-account-identifier-and-key"></a>配置帐户标识符和密钥

接下来，使用帐户标识符和之前设置空间定位点资源时记录的帐户密钥来配置应用。

打开 `HoloLens\DirectX\SampleHoloLens\ViewController.cpp`。

找到 `SpatialAnchorsAccountKey` 字段并将 `Set me` 替换为帐户密钥。

找到 `SpatialAnchorsAccountId` 字段并将 `Set me` 替换为帐户标识符。

## <a name="deploy-the-app-to-your-hololens"></a>将应用部署到 HoloLens

将“解决方案配置”更改为“发布”，将“解决方案平台”更改为 x86，并从部署目标选项中选择“设备”。

![Visual Studio 配置](./media/get-started-hololens/visual-studio-configuration.png)

打开 HoloLens 设备，登录并使用 USB 电缆将其连接到电脑。

选择“调试” > “开始调试”以部署应用并开始调试。

按照应用中的说明，放置并重新调用定位点。

在 Visual Studio 中，通过选择“停止调试”或按“Shift+F5”停止应用。

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [教程：跨设备共享空间定位点](../tutorials/tutorial-share-anchors-across-devices.md)
