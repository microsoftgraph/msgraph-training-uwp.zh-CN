<!-- markdownlint-disable MD002 MD041 -->

在此部分中，你将创建新的 UWP 应用。

1. 打开Visual Studio，然后选择 **"新建项目"。** 选择 **使用 C# (Windows**) "空白应用"选项，然后选择"下 **一步"。**

    ![Visual Studio 2019 新建项目对话框](./images/vs-create-new-project.png)

1. 在 **"配置新项目"对话框中**， `GraphTutorial` 输入项目 **名称** 字段，然后选择"**创建"。**

    ![Visual Studio 2019 配置新项目对话框](./images/vs-configure-new-project.png)

    > [!IMPORTANT]
    > 确保为这些实验室说明中指定的 Visual Studio 项目输入完全相同的名称。 项目Visual Studio名称将成为代码中命名空间的一部分。 这些说明中的代码取决于与这些Visual Studio中指定的项目名称匹配的命名空间。 如果使用其他项目名称，则代码将不会编译，除非您调整所有命名空间以匹配Visual Studio项目名称时输入的项目名称。

1. 选择“**确定**”。 在 **"新建通用 Windows 平台项目**"对话框中，确保将最低版本设置为 `Windows 10, Version 1809 (10.0; Build 17763)` 或更高版本，然后选择"**确定"。**

## <a name="install-nuget-packages"></a>安装 NuGet 包

在继续之前，请安装一些稍后将使用的其他 NuGet 程序包。

- [Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid/) 以显示 Microsoft Graph 返回的信息。
- [Microsoft.Toolkit.Graph.Controls，](https://www.nuget.org/packages/Microsoft.Toolkit.Graph.Controls) 用于处理登录和访问令牌检索。

1. Select **Tools > NuGet 程序包管理器 > 程序包管理器 Console.** 在程序包管理器控制台中，输入以下命令。

    ```powershell
    Install-Package Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid -IncludePrerelease
    Install-Package Microsoft.Toolkit.Graph.Controls -IncludePrerelease
    ```

## <a name="design-the-app"></a>设计应用

在此部分中，你将为应用创建 UI。

1. 首先添加应用程序级变量以跟踪身份验证状态。 在解决方案资源管理器中，展开 **App.xaml****并打开** App.xaml.cs。 将以下属性添加到 `App` 类。

    ```csharp
    public bool IsAuthenticated { get; set; }
    ```

1. 定义主页的布局。 打开 `MainPage.xaml` 并替换其全部内容，并替换为以下内容。

    :::code language="xaml" source="../demo/GraphTutorial/MainPage.xaml" id="MainPageXamlSnippet":::

    这将定义一个基本[NavigationView，](/uwp/api/windows.ui.xaml.controls.navigationview)其中"主页"、"日历"和"新建"事件导航链接充当应用的主视图。   它还在视图的标题中添加一个 [LoginButton](https://github.com/windows-toolkit/Graph-Controls) 控件。 该控件将允许用户登录和注销。该控件尚未完全启用，你将在稍后的练习中对其进行配置。

1. 在解决方案资源管理器 **中右键单击图形教程** 项目，然后选择">**新项目..."。** 选择 **"空白页**"， `HomePage.xaml` 在"名称"字段中输入，然后选择"**添加"。** 将文件中 `<Grid>` 现有的元素替换为以下内容。

    :::code language="xaml" source="../demo/GraphTutorial/HomePage.xaml" id="HomePageGridSnippet" highlight="2-5":::

1. 在 **解决方案资源管理器中展开 MainPage.xaml** 并打开 `MainPage.xaml.cs` 。 将以下函数 `MainPage` 添加到类以管理身份验证状态。

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="SetAuthStateSnippet":::

1. 将以下代码添加到 `MainPage()` 行 **后的** `this.InitializeComponent();` 构造函数。

    ```csharp
    // Initialize auth state to false
    SetAuthState(false);

    // Configure MSAL provider
    // TEMPORARY
    MsalProvider.ClientId = "11111111-1111-1111-1111-111111111111";

    // Navigate to HomePage.xaml
    RootFrame.Navigate(typeof(HomePage));
    ```

    当应用首次启动时，它将初始化身份验证状态并 `false` 导航到主页。

1. 添加以下事件处理程序，以在用户从导航视图中选择项目时加载请求的页面。

    ```csharp
    private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
    {
        var invokedItem = args.InvokedItem as string;

        switch (invokedItem.ToLower())
        {
            case "new event":
                throw new NotImplementedException();
                break;
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

1. 保存所有更改，然后按 **F5** 或选择"调试>**开始调试Visual Studio。**

    > [!NOTE]
    > 请确保为计算机配置 (ARM、x64、x86) 。

    ![主页的屏幕截图](./images/create-app-01.png)
