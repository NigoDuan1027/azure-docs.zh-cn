---
title: 教程：Azure Active Directory 与 Work.com 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 与 Work.com 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1e5d097afa3af111a7de8341b90d8c6c05ac0347
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2019
ms.locfileid: "56205743"
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>教程：Azure Active Directory 与 Work.com 的集成

本教程介绍如何将 Work.com 与 Azure Active Directory (Azure AD) 集成。

将 Work.com 与 Azure AD 集成提供以下优势：

- 可在 Azure AD 中控制谁有权访问 Work.com
- 可让用户使用其 Azure AD 帐户自动登录到 Work.com（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Work.com 的集成，需要准备好以下各项：

- Azure AD 订阅
- 已启用 Work.com 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库添加 Work.com
1. 配置和测试 Azure AD 单一登录

## <a name="add-workcom-from-the-gallery"></a>从库添加 Work.com
若要配置 Work.com 与 Azure AD 的集成，需要从库中将 Work.com 添加到托管 SaaS 应用列表。

**若要从库添加 Work.com，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

1. 在搜索框中键入 **Work.com**，在结果面板中选择“Workrite”，并单击“添加”按钮添加该应用程序。

    ![从库添加](./media/work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，基于一个名为“Britta Simon”的测试用户使用 Work.com 配置和测试 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Work.com 用户。 换句话说，需要建立 Azure AD 用户与 Work.com 中相关用户之间的链接关系。

可通过将 Azure AD 中“用户名”的值指定为 Work.com 中“用户名”的值来建立此链接关系。

若要配置和测试 Work.com 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 Work.com 测试用户](#create-a-workcom-test-user)** - 在 Work.com 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
1. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将介绍如何在 Azure 门户中启用 Azure AD 单一登录并在 Work.com 应用程序中配置单一登录。

>[!NOTE]
>若要配置单一登录，需要设置一个自定义 Work.com 域名。 需要至少定义一个域名，测试该域名，并将其部署到整个组织。

**若要配置 Work.com 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中，在 **Work.com** 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![基于 SAML 的登录](./media/work-com-tutorial/tutorial_work-com_samlbase.png)

1. 在“Work.com 域和 URL”部分中，执行以下步骤：

    ![“Work.com 域和 URL”部分](./media/work-com-tutorial/tutorial_work-com_url.png)

    在“登录 URL”文本框中，使用以下模式键入 URL： `http://<companyname>.my.salesforce.com`

    > [!NOTE] 
    > 此值不是真实值。 使用实际登录 URL 更新此值。 请联系 [Work.com 客户端支持团队](https://help.salesforce.com/articleView?id=000159855&type=3)获取此值。 

1. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![“SAML 签名证书”部分](./media/work-com-tutorial/tutorial_work-com_certificate.png) 

1. 单击“保存”按钮。

    ![保存按钮](./media/work-com-tutorial/tutorial_general_400.png)

1. 在“Work.com 配置”部分，单击“配置 Work.com”打开“配置登录”窗口。 从“快速参考”部分中复制“注销 URL”、“SAML 实体 ID”和“SAML 单一登录服务 URL”。

    ![“Work.com 配置”部分](./media/work-com-tutorial/tutorial_work-com_configure.png) 
1. 以管理员身份登录到 Work.com 租户。

1. 转到“设置”。
   
    ![设置](./media/work-com-tutorial/ic794108.png "设置")

1. 在左侧导航窗格中的“管理”部分中，单击“域管理”以展开相关部分，并单击“我的域”，打开“我的域”页。 
   
    ![我的域](./media/work-com-tutorial/ic767825.png "我的域")

1. 要验证域是否已正确设置，请确保它在“步骤 4 部署到用户”中，并复查“我的域设置”。
   
    ![部署到用户的域](./media/work-com-tutorial/ic784377.png "部署到用户的域")

1. 登录到 Work.com 租户。

1. 转到“设置”。
    
    ![设置](./media/work-com-tutorial/ic794108.png "设置")

1. 展开“安全控件”菜单，并单击“单一登录设置”。
    
    ![单一登录设置](./media/work-com-tutorial/ic794113.png "Single Sign-On Settings")

1. 在“单一登录设置”对话框页上，执行以下步骤：
    
    ![已启用 SAML](./media/work-com-tutorial/ic781026.png "已启用 SAML")
    
    a. 选择“已启用 SAML”。
    
    b. 单击“新建” 。

1. 在“SAML 单一登录设置”部分中，执行以下步骤：
    
    ![SAML 单一登录设置](./media/work-com-tutorial/ic794114.png "SAML Single Sign-On Settings")
    
    a. 在“名称”文本框中，键入配置名称。  
       
    > [!NOTE]
    > 提供“名称”值时会自动填充“API 名称”文本框。
    
    b. 将从 Azure 门户复制的“SAML 实体 ID”的值粘贴到“颁发者”文本框中。
    
    c. 若要从 Azure 门户上传已下载的证书，请单击“浏览”。
    
    d. 在“实体 ID”文本框中，键入 `https://salesforce-work.com`。
    
    e. 对于“SAML 标识类型”，选择“断言包含用户对象的联合 ID”。
    
    f. 对于“SAML 标识位置”，选择“标识位于 Subject 语句的 NameIdentifier 元素中”。
    
    g. 在“标识提供者登录 URL”文本框中，粘贴从 Azure 门户复制的“SAML 单一登录服务 URL”值。

    h. 在“标识提供者注销 URL”文本框中，粘贴从 Azure 门户复制的“注销 URL”值。
    
    i. 对于“服务提供程序发起的请求绑定”，请选择“HTTP Post”。
    
    j. 单击“ **保存**”。

1. 在 Work.com 经典门户内，从左侧导航窗格中，单击“域管理”以展开相关部分，并单击“我的域”，打开“我的域”页。 
    
    ![我的域](./media/work-com-tutorial/ic794115.png "我的域")

1. 在“我的域”页上的“登录页品牌打造”部分中，单击“编辑”。
    
    ![登录页品牌打造](./media/work-com-tutorial/ic767826.png "登录页品牌打造")

1. 在“登录页品牌打造”页上的“身份验证服务”部分中，会显示 **SAML SSO 设置**的名称。 选择它，并单击“保存”。
    
    ![登录页品牌打造](./media/work-com-tutorial/ic784366.png "登录页品牌打造")

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/work-com-tutorial/create_aaduser_01.png) 

1. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![“用户和组”->“所有用户”](./media/work-com-tutorial/create_aaduser_02.png) 

1. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![添加](./media/work-com-tutorial/create_aaduser_03.png) 

1. 在“用户”对话框页上，执行以下步骤：
 
    ![“用户”对话框页](./media/work-com-tutorial/create_aaduser_04.png) 

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="create-a-workcom-test-user"></a>创建 Work.com 测试用户
要使 Azure Active Directory 用户能够登录，必须将这些用户预配到 Work.com 中。 对于 Work.com，需要手动执行预配。

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>若要配置用户设置，请执行以下步骤：
1. 以管理员身份登录到 Work.com 公司站点。

1. 转到“设置”。
   
    ![设置](./media/work-com-tutorial/IC794108.png "设置")
1. 转到“管理用户”\>“用户”。
   
    ![管理用户](./media/work-com-tutorial/IC784369.png "管理用户")

1. 单击“新建用户”。
   
    ![所有用户](./media/work-com-tutorial/IC794117.png "所有用户")

1. 在“编辑用户”部分，在要预配到相关文本框的有效 Azure AD 帐户的属性中执行以下步骤：
   
    ![用户编辑](./media/work-com-tutorial/ic794118.png "用户编辑")
   
    a. 在“名字”文本框中，键入用户的**名字** (**Britta**)。
    
    b. 在“姓氏”文本框中，键入用户的**姓氏** (**Simon**)。
    
    c. 在“别名”文本框中，键入用户的**别名** (**BrittaS**)。
    
    d. 在“电子邮件”文本框中，键入用户的**电子邮件地址**（例如 **Brittasimon@contoso.com**）。
    
    e. 在“用户名”文本框中，键入用户的用户名（例如 **Brittasimon@contoso.com**）。
    
    f. 在“昵称”文本框中，键入用户的**昵称** (**Simon**)。
    
    g. 选择“角色”、“用户许可证”和“配置文件”。
    
    h. 单击“ **保存**”。  
      
    > [!NOTE]
    > Azure AD 帐户持有者会收到一封电子邮件，其中包含用于在激活帐户前确认帐户的链接。
    > 
    > 

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Work.com 的权限，允许她使用 Azure 单一登录。

![分配用户][200] 

**若要将 Britta Simon 分配到 Work.com，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“Work.com”。

    ![应用列表中的 Work.com](./media/work-com-tutorial/tutorial_work-com_app.png) 

1. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

在访问面板中单击“Work.com”磁贴时，应会自动登录到 Work.com 应用程序。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/work-com-tutorial/tutorial_general_01.png
[2]: ./media/work-com-tutorial/tutorial_general_02.png
[3]: ./media/work-com-tutorial/tutorial_general_03.png
[4]: ./media/work-com-tutorial/tutorial_general_04.png

[100]: ./media/work-com-tutorial/tutorial_general_100.png

[200]: ./media/work-com-tutorial/tutorial_general_200.png
[201]: ./media/work-com-tutorial/tutorial_general_201.png
[202]: ./media/work-com-tutorial/tutorial_general_202.png
[203]: ./media/work-com-tutorial/tutorial_general_203.png

