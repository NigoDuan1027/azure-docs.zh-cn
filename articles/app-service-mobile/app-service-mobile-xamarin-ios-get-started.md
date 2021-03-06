---
title: 适用于 Xamarin iOS 应用的 Azure 应用服务移动应用入门 | Microsoft Docs
description: 按照本教程进行操作，开始使用移动应用进行 Xamarin.iOS 开发。
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: d08ebc7c1e52022ec918020387264c464d1cd967
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2019
ms.locfileid: "56883413"
---
# <a name="create-a-xamarinios-app"></a>创建 Xamarin iOS 应用
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>概述
本教程说明如何使用 Azure 移动应用后端向 Xamarin.iOS 移动应用添加基于云的后端服务。  将创建一个新的移动应用后端以及一个简单的 *待办事项列表* Xamarin.iOS 应用，此应用将应用数据存储在 Azure 中。

只有在完成本教程后，才可以学习有关使用 Azure 应用服务中的移动应用功能的所有其他 Xamarin.iOS 教程。

## <a name="prerequisites"></a>先决条件
若要完成本教程，需要具备以下先决条件：

* 有效的 Azure 帐户。 如果没有帐户，可以注册 Azure 试用版并获取多达 10 个免费的移动应用，即使在试用期结束之后仍可继续使用这些应用。 有关详细信息，请参阅 [Azure 免费试用](https://azure.microsoft.com/pricing/free-trial/)。
* Visual Studio with Xamarin。 有关说明，请参阅 [设置和安装 Visual Studio 和 Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) 。
* 安装了 Xcode v7.0 版或更高版本以及 Xamarin Studio Community 的 Mac。 请参阅[设置和安装 Visual Studio 和 Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) 以及 [Mac 用户的设置、安装和验证](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN)。

## <a name="create-an-azure-mobile-app-backend"></a>创建 Azure 移动应用后端
按照这些步骤创建移动应用后端。

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>配置服务器项目
现已预配可供移动客户端应用程序使用的 Azure 移动应用后端。 接下来，为简单的“待办事项列表”后端下载服务器项目并将其发布到 Azure。

按照下列步骤将服务器项目配置为使用 Node.js 或 .NET 后端。

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>下载并运行 Xamarin.iOS 应用
1. 在浏览器窗口中，打开 [Azure 门户] 。
2. 在移动应用的“设置”边栏选项卡上，单击“开始使用” > “Xamarin.iOS”。 在步骤 3 下，单击“创建新应用”  （如果尚未选择它）。  接下来，单击“下载”  按钮。

      将下载连接到移动后端的客户端应用程序。 将压缩的项目文件保存到本地计算机，并记下保存位置。
3. 解压缩下载的项目，并在 Xamarin Studio（或 Visual Studio）中打开它。

    ![][9]

    ![][8]
4. 按 F5 键生成项目，并在 iPhone 模拟器中启动应用。
5. 在应用中键入有意义的文本（例如 *Learn Xamarin*），并单击“+”按钮。

    ![][10]

    来自请求的数据插入到 TodoItem 表。 移动应用后端返回存储在表中的项，数据显示在列表中。

> [!NOTE]
> 可以在 QSTodoService.cs C# 文件中查看用于访问移动应用后端以查询和插入数据的代码。
>
>

## <a name="next-steps"></a>后续步骤
* [向应用添加脱机同步](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [向应用添加身份验证](app-service-mobile-xamarin-ios-get-started-users.md)
* [向 Xamarin.Android 应用添加推送通知](app-service-mobile-xamarin-ios-get-started-push.md)
* [如何使用 Azure 移动应用的托管客户端](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure 门户]: https://portal.azure.com/
