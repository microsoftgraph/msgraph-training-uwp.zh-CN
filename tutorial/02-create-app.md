<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5858b-101">在此部分中，你将创建新的 UWP 应用。</span><span class="sxs-lookup"><span data-stu-id="5858b-101">In this section you'll create a new UWP app.</span></span>

1. <span data-ttu-id="5858b-102">打开Visual Studio，然后选择 **"新建项目"。**</span><span class="sxs-lookup"><span data-stu-id="5858b-102">Open Visual Studio, and select **Create a new project**.</span></span> <span data-ttu-id="5858b-103">选择 **使用 C# (Windows**) "空白应用"选项，然后选择"下 **一步"。**</span><span class="sxs-lookup"><span data-stu-id="5858b-103">Choose the **Blank App (Universal Windows)** option that uses C#, then select **Next**.</span></span>

    ![Visual Studio 2019 新建项目对话框](./images/vs-create-new-project.png)

1. <span data-ttu-id="5858b-105">在 **"配置新项目"对话框中**， `GraphTutorial` 输入项目 **名称** 字段，然后选择"**创建"。**</span><span class="sxs-lookup"><span data-stu-id="5858b-105">In the **Configure your new project** dialog, enter `GraphTutorial` in the **Project name** field and select **Create**.</span></span>

    ![Visual Studio 2019 配置新项目对话框](./images/vs-configure-new-project.png)

    > [!IMPORTANT]
    > <span data-ttu-id="5858b-107">确保为这些实验室说明中指定的 Visual Studio 项目输入完全相同的名称。</span><span class="sxs-lookup"><span data-stu-id="5858b-107">Ensure that you enter the exact same name for the Visual Studio Project that is specified in these lab instructions.</span></span> <span data-ttu-id="5858b-108">项目Visual Studio名称将成为代码中命名空间的一部分。</span><span class="sxs-lookup"><span data-stu-id="5858b-108">The Visual Studio Project name becomes part of the namespace in the code.</span></span> <span data-ttu-id="5858b-109">这些说明中的代码取决于与这些Visual Studio中指定的项目名称匹配的命名空间。</span><span class="sxs-lookup"><span data-stu-id="5858b-109">The code inside these instructions depends on the namespace matching the Visual Studio Project name specified in these instructions.</span></span> <span data-ttu-id="5858b-110">如果使用其他项目名称，则代码将不会编译，除非您调整所有命名空间以匹配Visual Studio项目名称时输入的项目名称。</span><span class="sxs-lookup"><span data-stu-id="5858b-110">If you use a different project name the code will not compile unless you adjust all the namespaces to match the Visual Studio Project name you enter when you create the project.</span></span>

1. <span data-ttu-id="5858b-111">选择“**确定**”。</span><span class="sxs-lookup"><span data-stu-id="5858b-111">Select **OK**.</span></span> <span data-ttu-id="5858b-112">在 **"新建通用 Windows 平台项目**"对话框中，确保将最低版本设置为 `Windows 10, Version 1809 (10.0; Build 17763)` 或更高版本，然后选择"**确定"。**</span><span class="sxs-lookup"><span data-stu-id="5858b-112">In the **New Universal Windows Platform Project** dialog, ensure that the **Minimum version** is set to `Windows 10, Version 1809 (10.0; Build 17763)` or later and select **OK**.</span></span>

## <a name="install-nuget-packages"></a><span data-ttu-id="5858b-113">安装 NuGet 包</span><span class="sxs-lookup"><span data-stu-id="5858b-113">Install NuGet packages</span></span>

<span data-ttu-id="5858b-114">在继续之前，请安装一些稍后将使用的其他 NuGet 程序包。</span><span class="sxs-lookup"><span data-stu-id="5858b-114">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="5858b-115">[Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid/) 以显示 Microsoft Graph 返回的信息。</span><span class="sxs-lookup"><span data-stu-id="5858b-115">[Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid/) to display the information returned by Microsoft Graph.</span></span>
- <span data-ttu-id="5858b-116">[Microsoft.Toolkit.Graph.Controls，](https://www.nuget.org/packages/Microsoft.Toolkit.Graph.Controls) 用于处理登录和访问令牌检索。</span><span class="sxs-lookup"><span data-stu-id="5858b-116">[Microsoft.Toolkit.Graph.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Graph.Controls) to handle login and access token retrieval.</span></span>

1. <span data-ttu-id="5858b-117">Select **Tools > NuGet 程序包管理器 > 程序包管理器 Console.**</span><span class="sxs-lookup"><span data-stu-id="5858b-117">Select **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="5858b-118">在程序包管理器控制台中，输入以下命令。</span><span class="sxs-lookup"><span data-stu-id="5858b-118">In the Package Manager Console, enter the following commands.</span></span>

    ```powershell
    Install-Package Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid -IncludePrerelease
    Install-Package Microsoft.Toolkit.Graph.Controls -IncludePrerelease
    ```

## <a name="design-the-app"></a><span data-ttu-id="5858b-119">设计应用</span><span class="sxs-lookup"><span data-stu-id="5858b-119">Design the app</span></span>

<span data-ttu-id="5858b-120">在此部分中，你将为应用创建 UI。</span><span class="sxs-lookup"><span data-stu-id="5858b-120">In this section you'll create the UI for the app.</span></span>

1. <span data-ttu-id="5858b-121">首先添加应用程序级变量以跟踪身份验证状态。</span><span class="sxs-lookup"><span data-stu-id="5858b-121">Start by adding an application-level variable to track authentication state.</span></span> <span data-ttu-id="5858b-122">在解决方案资源管理器中，展开 **App.xaml\*\*\*\*并打开** App.xaml.cs。</span><span class="sxs-lookup"><span data-stu-id="5858b-122">In Solution Explorer, expand **App.xaml** and open **App.xaml.cs**.</span></span> <span data-ttu-id="5858b-123">将以下属性添加到 `App` 类。</span><span class="sxs-lookup"><span data-stu-id="5858b-123">Add the following property to the `App` class.</span></span>

    ```csharp
    public bool IsAuthenticated { get; set; }
    ```

1. <span data-ttu-id="5858b-124">定义主页的布局。</span><span class="sxs-lookup"><span data-stu-id="5858b-124">Define the layout for the main page.</span></span> <span data-ttu-id="5858b-125">打开 `MainPage.xaml` 并替换其全部内容，并替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="5858b-125">Open `MainPage.xaml` and replace its entire contents with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/MainPage.xaml" id="MainPageXamlSnippet":::

    <span data-ttu-id="5858b-126">这将定义一个基本[NavigationView，](/uwp/api/windows.ui.xaml.controls.navigationview)其中"主页"、"日历"和"新建"事件导航链接充当应用的主视图。  </span><span class="sxs-lookup"><span data-stu-id="5858b-126">This defines a basic [NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) with **Home**, **Calendar**, and **New event** navigation links to act as the main view of the app.</span></span> <span data-ttu-id="5858b-127">它还在视图的标题中添加一个 [LoginButton](https://github.com/windows-toolkit/Graph-Controls) 控件。</span><span class="sxs-lookup"><span data-stu-id="5858b-127">It also adds a [LoginButton](https://github.com/windows-toolkit/Graph-Controls) control in the header of the view.</span></span> <span data-ttu-id="5858b-128">该控件将允许用户登录和注销。该控件尚未完全启用，你将在稍后的练习中对其进行配置。</span><span class="sxs-lookup"><span data-stu-id="5858b-128">That control will allow the user to sign in and out. The control isn't fully enabled yet, you will configure it in a later exercise.</span></span>

1. <span data-ttu-id="5858b-129">在解决方案资源管理器 **中右键单击图形教程** 项目，然后选择">**新项目..."。** 选择 **"空白页**"， `HomePage.xaml` 在"名称"字段中输入，然后选择"**添加"。**</span><span class="sxs-lookup"><span data-stu-id="5858b-129">Right-click the **graph-tutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `HomePage.xaml` in the **Name** field, and select **Add**.</span></span> <span data-ttu-id="5858b-130">将文件中 `<Grid>` 现有的元素替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="5858b-130">Replace the existing `<Grid>` element in the file with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/HomePage.xaml" id="HomePageGridSnippet" highlight="2-5":::

1. <span data-ttu-id="5858b-131">在 **解决方案资源管理器中展开 MainPage.xaml** 并打开 `MainPage.xaml.cs` 。</span><span class="sxs-lookup"><span data-stu-id="5858b-131">Expand **MainPage.xaml** in Solution Explorer and open `MainPage.xaml.cs`.</span></span> <span data-ttu-id="5858b-132">将以下函数 `MainPage` 添加到类以管理身份验证状态。</span><span class="sxs-lookup"><span data-stu-id="5858b-132">Add the following function to the `MainPage` class to manage authentication state.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="SetAuthStateSnippet":::

1. <span data-ttu-id="5858b-133">将以下代码添加到 `MainPage()` 行 **后的** `this.InitializeComponent();` 构造函数。</span><span class="sxs-lookup"><span data-stu-id="5858b-133">Add the following code to the `MainPage()` constructor **after** the `this.InitializeComponent();` line.</span></span>

    ```csharp
    // Initialize auth state to false
    SetAuthState(false);

    // Configure MSAL provider
    // TEMPORARY
    MsalProvider.ClientId = "11111111-1111-1111-1111-111111111111";

    // Navigate to HomePage.xaml
    RootFrame.Navigate(typeof(HomePage));
    ```

    <span data-ttu-id="5858b-134">当应用首次启动时，它将初始化身份验证状态并 `false` 导航到主页。</span><span class="sxs-lookup"><span data-stu-id="5858b-134">When the app first starts, it will initialize the authentication state to `false` and navigate to the home page.</span></span>

1. <span data-ttu-id="5858b-135">添加以下事件处理程序，以在用户从导航视图中选择项目时加载请求的页面。</span><span class="sxs-lookup"><span data-stu-id="5858b-135">Add the following event handler to load the requested page when the user selects an item from the navigation view.</span></span>

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

1. <span data-ttu-id="5858b-136">保存所有更改，然后按 **F5** 或选择"调试>**开始调试Visual Studio。**</span><span class="sxs-lookup"><span data-stu-id="5858b-136">Save all of your changes, then press **F5** or select **Debug > Start Debugging** in Visual Studio.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5858b-137">请确保为计算机配置 (ARM、x64、x86) 。</span><span class="sxs-lookup"><span data-stu-id="5858b-137">Make sure you select the appropriate configuration for your machine (ARM, x64, x86).</span></span>

    ![主页的屏幕截图](./images/create-app-01.png)
