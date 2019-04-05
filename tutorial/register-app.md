<!-- markdownlint-disable MD002 MD041 -->

在本练习中, 你将使用 azure Active Directory 管理中心创建新的 azure AD native 应用程序。

1. 打开浏览器并导航到[Azure Active Directory 管理中心](https://aad.portal.azure.com), 并使用**个人帐户**(亦称为 "Microsoft 帐户") 或**工作或学校帐户**进行登录。

1. 在左侧导航栏中选择 " **Azure Active Directory** ", 然后选择 "**管理**" 下的 "**应用注册 (预览)** "。

    ![应用注册的屏幕截图 ](./images/aad-portal-app-registrations.png)

1. 选择 "**新建注册**"。 在 "**注册应用程序**" 页上, 按如下所示设置值。

    - 将**名称**设置`UWP Graph Tutorial`为。
    - 将**支持的帐户类型**设置为**任何组织目录和个人 Microsoft 帐户中的帐户**。
    - 将**重定向 URI**留空。

    !["注册应用程序" 页的屏幕截图](./images/aad-register-an-app.png)

1. 选择 "**注册**"。 在**UWP Graph 教程**页上, 复制**应用程序 (客户端) ID**的值并保存它, 下一步将需要它。

    ![新应用注册的应用程序 ID 的屏幕截图](./images/aad-application-id.png)

1. 选择 "**添加重定向 URI** " 链接。 在 "**重定向 uri** " 页上, 找到 "**公共客户端 (移动、桌面)** " 部分的 "建议的重定向 uri" 部分。 选择 URI `urn:ietf:wg:oauth:2.0:oob` , 然后选择 "**保存**"。

    !["重定向 uri" 页的屏幕截图](./images/aad-redirect-uris.png)