<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="273f6-101">在此练习中，你将扩展上一练习中的应用程序，以支持使用 Azure AD 进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="273f6-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="273f6-102">这是获取调用 Microsoft Graph 所需的 OAuth 访问令牌所必需的。</span><span class="sxs-lookup"><span data-stu-id="273f6-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="273f6-103">在此步骤中，你将从 Windows Graph 控件将 **LoginButton** [控件集成到](https://github.com/windows-toolkit/Graph-Controls) 应用程序中。</span><span class="sxs-lookup"><span data-stu-id="273f6-103">In this step you will integrate the **LoginButton** control from the [Windows Graph Controls](https://github.com/windows-toolkit/Graph-Controls) into the application.</span></span>

1. <span data-ttu-id="273f6-104">在解决方案资源管理器中右键单击 **GraphTu一l** 项目， **然后选择>新项目...** Choose **Resources File (.resw) ，** name the file `OAuth.resw` and select **Add**.</span><span class="sxs-lookup"><span data-stu-id="273f6-104">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Resources File (.resw)**, name the file `OAuth.resw` and select **Add**.</span></span> <span data-ttu-id="273f6-105">当新文件在Visual Studio时，创建两个资源，如下所示。</span><span class="sxs-lookup"><span data-stu-id="273f6-105">When the new file opens in Visual Studio, create two resources as follows.</span></span>

    - <span data-ttu-id="273f6-106">**名称** `AppId` **：，值** ：在应用程序注册门户中生成的应用 ID</span><span class="sxs-lookup"><span data-stu-id="273f6-106">**Name:** `AppId`, **Value:** the app ID you generated in Application Registration Portal</span></span>
    - <span data-ttu-id="273f6-107">**名称** `Scopes` ：，**值：**`User.Read User.ReadBasic.All People.Read MailboxSettings.Read Calendars.ReadWrite`</span><span class="sxs-lookup"><span data-stu-id="273f6-107">**Name:** `Scopes`, **Value:** `User.Read User.ReadBasic.All People.Read MailboxSettings.Read Calendars.ReadWrite`</span></span>

    ![OAuth.resw 文件在Visual Studio屏幕截图](./images/edit-resources-01.png)

    > [!IMPORTANT]
    > <span data-ttu-id="273f6-109">如果你使用的是源代码管理（如 git），那么现在应该将文件从源代码管理中排除，以避免意外泄露 `OAuth.resw` 应用 ID。</span><span class="sxs-lookup"><span data-stu-id="273f6-109">If you're using source control such as git, now would be a good time to exclude the `OAuth.resw` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="configure-the-loginbutton-control"></a><span data-ttu-id="273f6-110">配置 LoginButton 控件</span><span class="sxs-lookup"><span data-stu-id="273f6-110">Configure the LoginButton control</span></span>

1. <span data-ttu-id="273f6-111">打开 `MainPage.xaml.cs` 以下语句 `using` 并将其添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="273f6-111">Open `MainPage.xaml.cs` and add the following `using` statement to the top of the file.</span></span>

    ```csharp
    using Microsoft.Toolkit.Graph.Providers;
    ```

1. <span data-ttu-id="273f6-112">将现有构造函数替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="273f6-112">Replace the existing constructor with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="ConstructorSnippet":::

    <span data-ttu-id="273f6-113">此代码从 MSAL 提供程序加载设置，并初始化 `OAuth.resw` 具有这些值的 MSAL 提供程序。</span><span class="sxs-lookup"><span data-stu-id="273f6-113">This code loads the settings from `OAuth.resw` and initializes the MSAL provider with those values.</span></span>

1. <span data-ttu-id="273f6-114">现在，为事件添加 `ProviderUpdated` 事件处理程序 `ProviderManager` 。</span><span class="sxs-lookup"><span data-stu-id="273f6-114">Now add an event handler for the `ProviderUpdated` event on the `ProviderManager`.</span></span> <span data-ttu-id="273f6-115">将以下函数添加到 `MainPage` 类。</span><span class="sxs-lookup"><span data-stu-id="273f6-115">Add the following function to the `MainPage` class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="ProviderUpdatedSnippet":::

    <span data-ttu-id="273f6-116">当提供程序更改或提供程序状态更改时，将触发此事件。</span><span class="sxs-lookup"><span data-stu-id="273f6-116">This event triggers when the provider changes, or when the provider state changes.</span></span>

1. <span data-ttu-id="273f6-117">在解决方案资源管理器中，展开 **HomePage.xaml** 并打开 `HomePage.xaml.cs` 。</span><span class="sxs-lookup"><span data-stu-id="273f6-117">In Solution Explorer, expand **HomePage.xaml** and open `HomePage.xaml.cs`.</span></span> <span data-ttu-id="273f6-118">将现有构造函数替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="273f6-118">Replace the existing constructor with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/HomePage.xaml.cs" id="ConstructorSnippet":::

1. <span data-ttu-id="273f6-119">重新启动应用 **，然后单击应用** 顶部的"登录"控件。</span><span class="sxs-lookup"><span data-stu-id="273f6-119">Restart the app and click the **Sign In** control at the top of the app.</span></span> <span data-ttu-id="273f6-120">登录后，UI 应更改以指示你已成功登录。</span><span class="sxs-lookup"><span data-stu-id="273f6-120">Once you've signed in, the UI should change to indicate that you've successfully signed-in.</span></span>

    ![登录后应用屏幕截图](./images/add-aad-auth-01.png)

    > [!NOTE]
    > <span data-ttu-id="273f6-122">`ButtonLogin`该控件实现存储和刷新访问令牌的逻辑。</span><span class="sxs-lookup"><span data-stu-id="273f6-122">The `ButtonLogin` control implements the logic of storing and refreshing the access token for you.</span></span> <span data-ttu-id="273f6-123">令牌存储在安全存储中并根据需要刷新。</span><span class="sxs-lookup"><span data-stu-id="273f6-123">The tokens are stored in secure storage and refreshed as needed.</span></span>
