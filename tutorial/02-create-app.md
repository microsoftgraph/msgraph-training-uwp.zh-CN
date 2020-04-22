<!-- markdownlint-disable MD002 MD041 -->

在本节中，您将创建一个新的 UWP 应用程序。

1. 打开 Visual Studio，然后选择 "**新建项目**"。 选择使用 c # 的**空白应用程序（通用窗口）** 选项，然后选择 "**下一步**"。

    ![Visual Studio 2019 "新建项目" 对话框](./images/vs-create-new-project.png)

1. 在 "**配置新项目**" 对话框中， `GraphTutorial`在 "**项目名称**" 字段中输入，然后选择 "**创建**"。

    ![Visual Studio 2019 配置新项目对话框](./images/vs-configure-new-project.png)

    > [!IMPORTANT]
    > 确保为在这些实验室说明中指定的 Visual Studio 项目输入完全相同的名称。 Visual Studio 项目名称将成为代码中的命名空间的一部分。 这些指令中的代码取决于与这些说明中指定的 Visual Studio 项目名称匹配的命名空间。 如果使用其他项目名称，则代码将不会编译，除非您调整所有命名空间以匹配您在创建项目时输入的 Visual Studio 项目名称。

1. 选择“确定”****。 在 "**新建通用 Windows 平台项目**" 对话框中，确保将**最低版本**设置为`Windows 10, Version 1809 (10.0; Build 17763)`或更高，然后选择 **"确定"**。

## <a name="install-nuget-packages"></a>安装 NuGet 包

在继续之前，请安装稍后将使用的一些其他 NuGet 包。

- 用于添加应用程序内通知和加载指示器的一些 UI 控件的[控件](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls/)。
- [若要](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid/)显示 microsoft Graph 返回的信息，则为。
- 用于处理登录和访问令牌检索的[Microsoft 工具包](https://www.nuget.org/packages/Microsoft.Toolkit.Graph.Controls)。

1. 选择 "**工具" > NuGet 包管理器 "> 程序包管理器控制台**"。 在 "程序包管理器控制台" 中，输入以下命令。

    ```powershell
    Install-Package Microsoft.Toolkit.Uwp.Ui.Controls -Version 6.0.0
    Install-Package Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid -Version 6.0.0
    Install-Package Microsoft.Toolkit.Graph.Controls -IncludePrerelease
    ```

## <a name="design-the-app"></a>设计应用程序

在本节中，您将为应用程序创建 UI。

1. 首先添加应用程序级变量以跟踪身份验证状态。 在 "解决方案资源管理器" 中，展开**app.xaml**并打开**App.xaml.cs**。 将以下属性添加到`App`类中。

    ```csharp
    public bool IsAuthenticated { get; set; }
    ```

1. 定义主页的布局。 打开`MainPage.xaml`并将其全部内容替换为以下内容。

    :::code language="xaml" source="../demo/GraphTutorial/MainPage.xaml" id="MainPageXamlSnippet":::

    这将定义基本[NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) ，并使用**Home**和**Calendar**导航链接充当应用程序的主视图。 此外，它还会在视图的标题中添加一个[LoginButton](https://github.com/windows-toolkit/Graph-Controls)控件。 该控件将允许用户登录和注销。该控件尚未完全启用，你将在后续练习中对其进行配置。

1. 右键单击 "解决方案资源管理器" 中的 "**教程**" 项目，然后选择 "**添加 > 新项 ...**"。选择 "**空白页**" `HomePage.xaml` ，在 "**名称**" 字段中输入，然后选择 "**添加**"。 将文件中`<Grid>`的现有元素替换为以下项。

    :::code language="xaml" source="../demo/GraphTutorial/HomePage.xaml" id="HomePageGridSnippet" highlight="2-5":::

1. 展开**MainPage.xaml** "解决方案资源管理器" 中的`MainPage.xaml.cs`MainPage，然后打开。 将以下函数添加到`MainPage`类以管理身份验证状态。

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="SetAuthStateSnippet":::

1. 将下面的代码添加到`MainPage()`构造函数中`this.InitializeComponent();`的行**后面**。

    ```csharp
    // Initialize auth state to false
    SetAuthState(false);

    // Configure MSAL provider
    // TEMPORARY
    MsalProvider.ClientId = "11111111-1111-1111-1111-111111111111";

    // Navigate to HomePage.xaml
    RootFrame.Navigate(typeof(HomePage));
    ```

    当应用程序第一次启动时，它会将身份`false`验证状态初始化为并导航到主页。

1. 添加以下事件处理程序，以便在用户从导航视图中选择项目时加载请求的页面。

    ```csharp
    private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
    {
        var invokedItem = args.InvokedItem as string;

        switch (invokedItem.ToLower())
        {
            case "calendar":
                throw new NotImplementedException();
                break;
            case "home":
            default:
                RootFrame.Navigate(typeof(HomePage));
                break;
        }
    }
    ```

1. 保存所有更改，然后按**F5**或选择调试 > 在 Visual Studio 中**启动调试**。

    > [!NOTE]
    > 确保为您的计算机选择适当的配置（ARM、x64、x86）。

    ![主页的屏幕截图](./images/create-app-01.png)
