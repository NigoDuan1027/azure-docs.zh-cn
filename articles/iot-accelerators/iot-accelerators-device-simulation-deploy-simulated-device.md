---
title: IoT 部署自定义模拟设备 - Azure | Microsoft Docs
description: 本操作指南介绍如何将自定义模拟设备部署到设备模拟解决方案加速器。
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/14/2018
ms.topic: conceptual
ms.openlocfilehash: e51d0330c3ed804bff8cdb06265bb8c39141733f
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2018
ms.locfileid: "43346458"
---
# <a name="deploy-a-new-simulated-device"></a>部署新的模拟设备

远程监视和设备模拟解决方案加速器都允许你定义自己的模拟设备。 本文介绍如何将自定义冷却器设备类型和新灯泡设备类型部署到设备模拟解决方案加速器。

本文中的步骤假设你已完成[创建并测试新模拟设备](iot-accelerators-remote-monitoring-create-simulated-device.md)操作指南，并拥有定义自定义冷却器和新灯泡设备类型的文件。

本操作指南中的步骤介绍如何：

1. 使用 SSH 访问承载设备模拟解决方案加速器的虚拟机的文件系统。

1. 配置 Docker 以从 Docker 容器外的位置加载设备模型。

1. 使用自定义设备模型文件运行模拟。

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

完成本操作方法指南中的步骤需要有效的 Azure 订阅。

如果还没有 Azure 订阅，可以在开始前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>先决条件

按照本操作方法指南操作需要：

- [设备模拟解决方案加速器](https://www.azureiotsolutions.com/Accelerators#solutions/types/DS)的一个已部署实例。
- 一个用于运行 `ssh` 和 `scp` 命令的本地 bash shell。 在 Windows 上，安装 bash 的简单方法是安装 [git](https://git-scm.com/download/win)。
- 自定义的设备模型文件，如[创建并测试新模拟设备](iot-accelerators-remote-monitoring-create-simulated-device.md)中所述的文件。

[!INCLUDE [iot-solution-accelerators-access-vm](../../includes/iot-solution-accelerators-access-vm.md)]

## <a name="configure-docker"></a>配置 Docker

本部分将配置 Docker，以从虚拟机中的 /tmp/devicemodels 文件夹加载设备模型文件，而不是从 Docker 容器内部加载。 在本地计算机上的 bash shell 中运行本部分中的命令：

1. 使用 SSH 从本地计算机连接到 Azure 中的虚拟机。 以下命令假设虚拟机 vm-vikxv 的公共 IP 地址为 104.41.128.108，将此值替换为上一部分中虚拟机的公共 IP 地址：

   ```sh
    ssh azureuser@104.41.128.108
    ```

    按照提示使用在上一部分中设置的密码登录虚拟机。

1. 配置设备模拟服务，从容器外加载设备模型。 请先打开 Docker 配置文件：

    ```sh
    sudo nano /app/docker-compose.yml
    ```

    找到 devicesimulation 容器的设置，然后编辑卷设置，如以下代码片段所示：

    ```yml
    # To mount custom device models, upload the files to the VM, edit and uncomment the following line:
          - /tmp/devicemodels:/app/webservice/data/devicemodels:ro
    # To mount a custom service configuration, create a custom file, edit and uncomment the following line:
    #     - /app/custom-device-simulation-appsettings.ini:/app/webservice/appsettings.ini:ro
    ```

    保存更改。

1. 创建用于存储 JSON 和脚本文件的文件夹：

    ```sh
    sudo mkdir -p /tmp/devicemodels/scripts
    ```

    使 bash 窗口和 SSH 会话处于打开状态。

1. 将自定义设备模型文件复制到虚拟机中。 在创建自定义设备模型的计算机上的另一 bash shell 中运行此命令。 首先，导航到包含设备模型 JSON 文件的本地文件夹。 以下命令假设虚拟机的公共 IP 地址为 104.41.128.108，将此值替换为你的虚拟机的公共 IP 地址。 出现提示时输入虚拟机密码：

    ```sh
    scp *json azureuser@104.41.128.108:/tmp/devicemodels
    cd scripts
    scp *js azureuser@104.41.128.108:/tmp/devicemodels/scripts
    ```

1. 重新启动设备模拟 Docker 容器以使用新设备模型。 在打开到虚拟机的 SSH 会话的 bash shell 中运行以下命令：

    ```sh
    sudo /app/start.sh
    ```

    如果要查看正在运行的 Docker 容器的状态及其容器 ID，请使用以下命令：

    ```sh
    docker ps
    ```

    如果要查看设备模拟容器中的日志，请运行以下命令。 将容器 ID 替换为你的设备模拟容器的 ID：

    ```sh
    docker logs -f 5d3f3e78822e
    ```

## <a name="run-simulation"></a>运行模拟

现即可使用自定义设备模型运行模拟：

1. 从 [Microsoft Azure IoT 解决方案加速器](https://www.azureiotsolutions.com/Accelerators#dashboard)启动设备模拟 web UI。

1. 借助 Web UI 使用一个自定义设备模型配置和运行模拟。

1. 运行模拟时，单击 Azure 门户中的“查看 IoT 中心指标”以监视模拟。 或者，可使用以下 Azure CLI 命令监视设备到云的流量。 使用你的中心名称替换 IoT 中心名称：

    ```azurecli-interactive
    az iot hub monitor-events -n contoso-simulation9dc75
    ```

## <a name="clean-up-resources"></a>清理资源

如果打算深入进行探索，请保留已部署的设备模拟解决方案加速器。

如果不再需要该解决方案加速器，请在[预配的解决方案](https://www.azureiotsolutions.com/Accelerators#dashboard)页中选择它，然后单击“删除解决方案”将其删除。

## <a name="next-steps"></a>后续步骤

本指南介绍如何将自定义设备模型部署到设备模拟解决方案加速器。 建议你接下来了解有关[设备模型架构](iot-accelerators-device-simulation-device-schema.md)的详细信息。