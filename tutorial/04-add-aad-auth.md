<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将扩展上一练习中的应用程序，以支持使用 Azure AD 进行身份验证。 这是获取调用 Microsoft Graph 所需的 OAuth 访问令牌所必需的。 在此步骤中，你将从 Windows Graph 控件将 **LoginButton** [控件集成到](https://github.com/windows-toolkit/Graph-Controls) 应用程序中。

1. 在解决方案资源管理器中右键单击 **GraphTu一l** 项目， **然后选择>新项目...** Choose **Resources File (.resw) ，** name the file `OAuth.resw` and select **Add**. 当新文件在Visual Studio时，创建两个资源，如下所示。

    - **名称** `AppId` **：，值** ：在应用程序注册门户中生成的应用 ID
    - **名称** `Scopes` ：，**值：**`User.Read User.ReadBasic.All People.Read MailboxSettings.Read Calendars.ReadWrite`

    ![OAuth.resw 文件在Visual Studio屏幕截图](./images/edit-resources-01.png)

    > [!IMPORTANT]
    > 如果你使用的是源代码管理（如 git），那么现在应该将文件从源代码管理中排除，以避免意外泄露 `OAuth.resw` 应用 ID。

## <a name="configure-the-loginbutton-control"></a>配置 LoginButton 控件

1. 打开 `MainPage.xaml.cs` 以下语句 `using` 并将其添加到文件顶部。

    ```csharp
    using Microsoft.Toolkit.Graph.Providers;
    ```

1. 将现有构造函数替换为以下内容。

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="ConstructorSnippet":::

    此代码从 MSAL 提供程序加载设置，并初始化 `OAuth.resw` 具有这些值的 MSAL 提供程序。

1. 现在，为事件添加 `ProviderUpdated` 事件处理程序 `ProviderManager` 。 将以下函数添加到 `MainPage` 类。

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="ProviderUpdatedSnippet":::

    当提供程序更改或提供程序状态更改时，将触发此事件。

1. 在解决方案资源管理器中，展开 **HomePage.xaml** 并打开 `HomePage.xaml.cs` 。 将现有构造函数替换为以下内容。

    :::code language="csharp" source="../demo/GraphTutorial/HomePage.xaml.cs" id="ConstructorSnippet":::

1. 重新启动应用 **，然后单击应用** 顶部的"登录"控件。 登录后，UI 应更改以指示你已成功登录。

    ![登录后应用屏幕截图](./images/add-aad-auth-01.png)

    > [!NOTE]
    > `ButtonLogin`该控件实现存储和刷新访问令牌的逻辑。 令牌存储在安全存储中并根据需要刷新。
