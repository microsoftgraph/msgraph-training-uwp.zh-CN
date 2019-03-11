<!-- markdownlint-disable MD002 MD041 -->

在本练习中, 你将扩展上一练习中的应用程序, 以支持 Azure AD 的身份验证。 若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph, 这是必需的。 在此步骤中, 将[AadLogin](https://docs.microsoft.com/dotnet/api/microsoft.toolkit.uwp.ui.controls.graph.aadlogin?view=win-comm-toolkit-dotnet-stable)控件从[Windows 社区工具包](https://github.com/Microsoft/WindowsCommunityToolkit)集成到应用程序中。

右键单击 "解决方案资源管理器" 中的 "**教程**" 项目, 然后选择 "**添加 > 新建项目 ...**"。选择 "**资源文件 (resources.resw)**", 命名该`OAuth.resw`文件, 然后选择 "**添加**"。 在 Visual Studio 中打开新文件时, 请按如下所示创建两个资源。

- **名称:**`AppId`, **Value:** 在应用程序注册门户中生成的应用程序 ID
- **名称:**`Scopes`, **Value:**`User.Read Calendars.Read`

![Visual Studio 编辑器中的 resources.resw 文件的屏幕截图](./images/edit-resources-01.png)

> [!IMPORTANT]
> 如果您使用的是源代码管理 (如 git), 现在可以从源代码管理中排除`OAuth.resw`该文件, 以避免无意中泄漏您的应用程序 ID。

## <a name="configure-the-aadlogin-control"></a>配置 AadLogin 控件

首先添加代码以读取资源文件中的值。 打开`MainPage.xaml.cs`并将以下`using`语句添加到文件顶部。

```cs
using Microsoft.Toolkit.Services.MicrosoftGraph;
```

将 `RootFrame.Navigate(typeof(HomePage));` 行替换成以下代码。

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

此代码从`OAuth.resw`加载设置, 并使用这些值初始化的`MicrosoftGraphService`全局实例。

现在, 为`SignInCompleted` `AadLogin`控件上的事件添加事件处理程序。 打开`MainPage.xaml`文件, 并将现有`<graphControls:AadLogin>`元素替换为以下项。

```xml
<graphControls:AadLogin x:Name="Login"
    HorizontalAlignment="Left"
    View="SmallProfilePhotoLeft"
    AllowSignInAsDifferentUser="False"
    SignInCompleted="Login_SignInCompleted"
    SignOutCompleted="Login_SignOutCompleted"
    />
```

然后, 将以下函数添加到`MainPage`中`MainPage.xaml.cs`的类。

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

最后, 在 "解决方案资源管理**** 器" 中, 展开`HomePage.xaml.cs`"主页" 和 "打开"。 在`this.InitializeComponent();`行后面添加以下代码。

```cs
if ((App.Current as App).IsAuthenticated)
{
    HomePageMessage.Text = "Welcome! Please use the menu to the left to select a view.";
}
```

重新启动应用程序, 并单击应用程序顶部的 "**登录**" 控件。 登录后, UI 应更改, 以指示您已成功登录。

![登录后的应用程序的屏幕截图](./images/add-aad-auth-01.png)

> [!NOTE]
> 该`AadLogin`控件实现为您存储和刷新访问令牌的逻辑。 令牌存储在安全存储中并根据需要进行刷新。