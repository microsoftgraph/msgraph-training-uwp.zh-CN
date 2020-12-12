<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="04a63-101">在此部分中，您将添加在用户日历上创建事件的能力。</span><span class="sxs-lookup"><span data-stu-id="04a63-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="04a63-102">为新事件视图添加新页面。</span><span class="sxs-lookup"><span data-stu-id="04a63-102">Add a new page for the new event view.</span></span> <span data-ttu-id="04a63-103">在解决方案资源管理器中右键 **单击 GraphTu一l** 项目，**然后选择>新项目...** 选择 **"空白页**"， `NewEventPage.xaml` 在"名称"字段中输入，然后选择"**添加"。**</span><span class="sxs-lookup"><span data-stu-id="04a63-103">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `NewEventPage.xaml` in the **Name** field, and select **Add**.</span></span>

1. <span data-ttu-id="04a63-104">打开 **NewEventPage.xaml** 并将其内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="04a63-104">Open **NewEventPage.xaml** and replace its contents with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/NewEventPage.xaml" id="NewEventPageXamlSnippet":::

1. <span data-ttu-id="04a63-105">打开 **NewEventPage.xaml.cs，** 将以下 `using` 语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="04a63-105">Open **NewEventPage.xaml.cs** and add the following `using` statements to the top of the file.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="UsingStatementsSnippet":::

1. <span data-ttu-id="04a63-106">将 **INotifyPropertyChange 接口** 添加到 **NewEventPage** 类。</span><span class="sxs-lookup"><span data-stu-id="04a63-106">Add the **INotifyPropertyChange** interface to the **NewEventPage** class.</span></span> <span data-ttu-id="04a63-107">将现有的类声明替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="04a63-107">Replace the existing class declaration with the following.</span></span>

    ```csharp
    public sealed partial class NewEventPage : Page, INotifyPropertyChanged
    {
        public NewEventPage()
        {
            this.InitializeComponent();
            DataContext = this;
        }
    }
    ```

1. <span data-ttu-id="04a63-108">将以下属性添加到 **NewEventPage** 类。</span><span class="sxs-lookup"><span data-stu-id="04a63-108">Add the following properties to the **NewEventPage** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="PropertiesSnippet":::

1. <span data-ttu-id="04a63-109">添加以下代码，在页面加载时从 Microsoft Graph 获取用户的时区。</span><span class="sxs-lookup"><span data-stu-id="04a63-109">Add the following code to get the user's time zone from Microsoft Graph when the page loads.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="LoadTimeZoneSnippet":::

1. <span data-ttu-id="04a63-110">添加以下代码以创建事件。</span><span class="sxs-lookup"><span data-stu-id="04a63-110">Add the following code to create the event.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="CreateEventSnippet":::

1. <span data-ttu-id="04a63-111">修改 `NavView_ItemInvoked` MainPage.xaml.cs 文件中的方法，以将现有 `switch` 语句替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="04a63-111">Modify the `NavView_ItemInvoked` method in the **MainPage.xaml.cs** file to replace the existing `switch` statement with the following.</span></span>

    ```csharp
    switch (invokedItem.ToLower())
    {
        case "new event":
            RootFrame.Navigate(typeof(NewEventPage));
            break;
        case "calendar":
            RootFrame.Navigate(typeof(CalendarPage));
            break;
        case "home":
        default:
            RootFrame.Navigate(typeof(HomePage));
            break;
    }
    ```

1. <span data-ttu-id="04a63-112">保存更改并运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="04a63-112">Save your changes and run the app.</span></span> <span data-ttu-id="04a63-113">登录，选择"**新建** 事件"菜单项，填写表单，然后选择"创建"将事件添加到用户日历。</span><span class="sxs-lookup"><span data-stu-id="04a63-113">Sign in, select the **New event** menu item, fill in the form, and select **Create** to add an event to the user's calendar.</span></span>

    ![新事件页面的屏幕截图](images/create-event-01.png)
