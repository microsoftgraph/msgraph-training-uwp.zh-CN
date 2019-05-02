<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d1c2e-101">在本练习中, 你将扩展上一练习中的应用程序, 以支持 Azure AD 的身份验证。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="d1c2e-102">若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph, 这是必需的。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="d1c2e-103">在此步骤中, 将[AadLogin](https://docs.microsoft.com/dotnet/api/microsoft.toolkit.uwp.ui.controls.graph.aadlogin?view=win-comm-toolkit-dotnet-stable)控件从[Windows 社区工具包](https://github.com/Microsoft/WindowsCommunityToolkit)集成到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-103">In this step you will integrate the [AadLogin](https://docs.microsoft.com/dotnet/api/microsoft.toolkit.uwp.ui.controls.graph.aadlogin?view=win-comm-toolkit-dotnet-stable) control from the [Windows Community Toolkit](https://github.com/Microsoft/WindowsCommunityToolkit) into the application.</span></span>

<span data-ttu-id="d1c2e-104">右键单击 "解决方案资源管理器" 中的 "**教程**" 项目, 然后选择 "**添加 > 新建项目 .。。**"。选择 "**资源文件 (resources.resw)**", 命名该`OAuth.resw`文件, 然后选择 "**添加**"。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-104">Right-click the **graph-tutorial** project in Solution Explorer and choose **Add > New Item...**. Choose **Resources File (.resw)**, name the file `OAuth.resw` and choose **Add**.</span></span> <span data-ttu-id="d1c2e-105">在 Visual Studio 中打开新文件时, 请按如下所示创建两个资源。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-105">When the new file opens in Visual Studio, create two resources as follows.</span></span>

- <span data-ttu-id="d1c2e-106">**名称:**`AppId`, **Value:** 在应用程序注册门户中生成的应用程序 ID</span><span class="sxs-lookup"><span data-stu-id="d1c2e-106">**Name:** `AppId`, **Value:** the app ID you generated in Application Registration Portal</span></span>
- <span data-ttu-id="d1c2e-107">**名称:**`Scopes`, **Value:**`User.Read Calendars.Read`</span><span class="sxs-lookup"><span data-stu-id="d1c2e-107">**Name:** `Scopes`, **Value:** `User.Read Calendars.Read`</span></span>

![Visual Studio 编辑器中的 resources.resw 文件的屏幕截图](./images/edit-resources-01.png)

> [!IMPORTANT]
> <span data-ttu-id="d1c2e-109">如果您使用的是源代码管理 (如 git), 现在可以从源代码管理中排除`OAuth.resw`该文件, 以避免无意中泄漏您的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-109">If you're using source control such as git, now would be a good time to exclude the `OAuth.resw` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="configure-the-aadlogin-control"></a><span data-ttu-id="d1c2e-110">配置 AadLogin 控件</span><span class="sxs-lookup"><span data-stu-id="d1c2e-110">Configure the AadLogin control</span></span>

<span data-ttu-id="d1c2e-111">首先添加代码以读取资源文件中的值。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-111">Start by adding code to read the values out of the resources file.</span></span> <span data-ttu-id="d1c2e-112">打开`MainPage.xaml.cs`并将以下`using`语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-112">Open `MainPage.xaml.cs` and add the following `using` statement to the top of the file.</span></span>

```cs
using Microsoft.Toolkit.Services.MicrosoftGraph;
```

<span data-ttu-id="d1c2e-113">将 `RootFrame.Navigate(typeof(HomePage));` 行替换成以下代码。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-113">Replace the `RootFrame.Navigate(typeof(HomePage));` line with the following code.</span></span>

```cs
// Load OAuth settings
var oauthSettings = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("OAuth");
var appId = oauthSettings.GetString("AppId");
var scopes = oauthSettings.GetString("Scopes");

if (string.IsNullOrEmpty(appId) || string.IsNullOrEmpty(scopes))
{
    Notification.Show("Could not load OAuth Settings from resource file.");
}
else
{
    // Initialize Graph
    MicrosoftGraphService.Instance.AuthenticationModel = MicrosoftGraphEnums.AuthenticationModel.V2;
    MicrosoftGraphService.Instance.Initialize(appId,
        MicrosoftGraphEnums.ServicesToInitialize.UserProfile,
        scopes.Split(' '));

    // Navigate to HomePage.xaml
    RootFrame.Navigate(typeof(HomePage));
}
```

<span data-ttu-id="d1c2e-114">此代码从`OAuth.resw`加载设置, 并使用这些值初始化的`MicrosoftGraphService`全局实例。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-114">This code loads the settings from `OAuth.resw` and initializes the global instance of the `MicrosoftGraphService` with those values.</span></span>

<span data-ttu-id="d1c2e-115">现在, 为`SignInCompleted` `AadLogin`控件上的事件添加事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-115">Now add an event handler for the `SignInCompleted` event on the `AadLogin` control.</span></span> <span data-ttu-id="d1c2e-116">打开`MainPage.xaml`文件, 并将现有`<graphControls:AadLogin>`元素替换为以下项。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-116">Open the `MainPage.xaml` file and replace the existing `<graphControls:AadLogin>` element with the following.</span></span>

```xml
<graphControls:AadLogin x:Name="Login"
    HorizontalAlignment="Left"
    View="SmallProfilePhotoLeft"
    AllowSignInAsDifferentUser="False"
    SignInCompleted="Login_SignInCompleted"
    SignOutCompleted="Login_SignOutCompleted"
    />
```

<span data-ttu-id="d1c2e-117">然后, 将以下函数添加到`MainPage`中`MainPage.xaml.cs`的类。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-117">Then add the following functions to the `MainPage` class in `MainPage.xaml.cs`.</span></span>

```cs
private void Login_SignInCompleted(object sender, Microsoft.Toolkit.Uwp.UI.Controls.Graph.SignInEventArgs e)
{
    // Set the auth state
    SetAuthState(true);
    // Reload the home page
    RootFrame.Navigate(typeof(HomePage));
}

private void Login_SignOutCompleted(object sender, EventArgs e)
{
    // Set the auth state
    SetAuthState(false);
    // Reload the home page
    RootFrame.Navigate(typeof(HomePage));
}
```

<span data-ttu-id="d1c2e-118">最后, 在 "解决方案资源管理\*\*\*\* 器" 中, 展开`HomePage.xaml.cs`"主页" 和 "打开"。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-118">Finally, in Solution Explorer, expand **HomePage.xaml** and open `HomePage.xaml.cs`.</span></span> <span data-ttu-id="d1c2e-119">在`this.InitializeComponent();`行后面添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-119">Add the following code after the `this.InitializeComponent();` line.</span></span>

```cs
if ((App.Current as App).IsAuthenticated)
{
    HomePageMessage.Text = "Welcome! Please use the menu to the left to select a view.";
}
```

<span data-ttu-id="d1c2e-120">重新启动应用程序, 并单击应用程序顶部的 "**登录**" 控件。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-120">Restart the app and click the **Sign In** control at the top of the app.</span></span> <span data-ttu-id="d1c2e-121">登录后, UI 应更改, 以指示您已成功登录。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-121">Once you've signed in, the UI should change to indicate that you've successfully signed-in.</span></span>

![登录后的应用程序的屏幕截图](./images/add-aad-auth-01.png)

> [!NOTE]
> <span data-ttu-id="d1c2e-123">该`AadLogin`控件实现为您存储和刷新访问令牌的逻辑。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-123">The `AadLogin` control implements the logic of storing and refreshing the access token for you.</span></span> <span data-ttu-id="d1c2e-124">令牌存储在安全存储中并根据需要进行刷新。</span><span class="sxs-lookup"><span data-stu-id="d1c2e-124">The tokens are stored in secure storage and refreshed as needed.</span></span>