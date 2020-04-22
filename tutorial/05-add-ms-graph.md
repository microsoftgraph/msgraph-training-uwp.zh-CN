<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f9726-101">在本练习中，将把 Microsoft Graph 合并到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="f9726-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="f9726-102">对于此应用程序，您将使用[Microsoft Graph 客户端库进行 .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet)以调用 microsoft graph。</span><span class="sxs-lookup"><span data-stu-id="f9726-102">For this application, you will use the [Microsoft Graph Client Library for .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="f9726-103">从 Outlook 获取日历事件</span><span class="sxs-lookup"><span data-stu-id="f9726-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="f9726-104">为 "日历" 视图添加新页面。</span><span class="sxs-lookup"><span data-stu-id="f9726-104">Add a new page for the calendar view.</span></span> <span data-ttu-id="f9726-105">在 "解决方案资源管理器" 中右键单击 " **GraphTutorial** " 项目，然后选择 "**添加 > 新项 ...**"。选择 "**空白页**" `CalendarPage.xaml` ，在 "**名称**" 字段中输入，然后选择 "**添加**"。</span><span class="sxs-lookup"><span data-stu-id="f9726-105">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `CalendarPage.xaml` in the **Name** field, and select **Add**.</span></span>

1. <span data-ttu-id="f9726-106">打开`CalendarPage.xaml`并将以下行添加到现有`<Grid>`元素中。</span><span class="sxs-lookup"><span data-stu-id="f9726-106">Open `CalendarPage.xaml` and add the following line inside the existing `<Grid>` element.</span></span>

    ```xaml
    <TextBlock x:Name="Events" TextWrapping="Wrap"/>
    ```

1. <span data-ttu-id="f9726-107">打开`CalendarPage.xaml.cs`并将以下`using`语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="f9726-107">Open `CalendarPage.xaml.cs` and add the following `using` statements at the top of the file.</span></span>

    ```csharp
    using Microsoft.Toolkit.Graph.Providers;
    using Microsoft.Toolkit.Uwp.UI.Controls;
    using Newtonsoft.Json;
    ```

1. <span data-ttu-id="f9726-108">将以下函数添加到`CalendarPage`类中。</span><span class="sxs-lookup"><span data-stu-id="f9726-108">Add the following functions to the `CalendarPage` class.</span></span>

    ```csharp
    private void ShowNotification(string message)
    {
        // Get the main page that contains the InAppNotification
        var mainPage = (Window.Current.Content as Frame).Content as MainPage;

        // Get the notification control
        var notification = mainPage.FindName("Notification") as InAppNotification;

        notification.Show(message);
    }

    protected override async void OnNavigatedTo(NavigationEventArgs e)
    {
        // Get the Graph client from the provider
        var graphClient = ProviderManager.Instance.GlobalProvider.Graph;

        try
        {
            // Get the events
            var events = await graphClient.Me.Events.Request()
                .Select("subject,organizer,start,end")
                .OrderBy("createdDateTime DESC")
                .GetAsync();

            // TEMPORARY: Show the results as JSON
            Events.Text = JsonConvert.SerializeObject(events.CurrentPage);
        }
        catch(Microsoft.Graph.ServiceException ex)
        {
            ShowNotification($"Exception getting events: {ex.Message}");
        }

        base.OnNavigatedTo(e);
    }
    ```

    <span data-ttu-id="f9726-109">考虑中`OnNavigatedTo`的代码执行操作。</span><span class="sxs-lookup"><span data-stu-id="f9726-109">Consider with the code in `OnNavigatedTo` is doing.</span></span>

    - <span data-ttu-id="f9726-110">将调用的 URL 为`/v1.0/me/events`。</span><span class="sxs-lookup"><span data-stu-id="f9726-110">The URL that will be called is `/v1.0/me/events`.</span></span>
    - <span data-ttu-id="f9726-111">`Select`函数将为每个事件返回的字段限制为仅供视图实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="f9726-111">The `Select` function limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="f9726-112">`OrderBy`函数按其创建日期和时间对结果进行排序，最新项目最先开始。</span><span class="sxs-lookup"><span data-stu-id="f9726-112">The `OrderBy` function sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="f9726-113">修改`NavView_ItemInvoked` `MainPage.xaml.cs`文件中的方法，以将现有`switch`语句替换为以下语句。</span><span class="sxs-lookup"><span data-stu-id="f9726-113">Modify the `NavView_ItemInvoked` method in the `MainPage.xaml.cs` file to replace the existing `switch` statement with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="SwitchStatementSnippet" highlight="4":::

<span data-ttu-id="f9726-114">您现在可以运行应用程序，登录，然后单击左侧菜单中的 "**日历**" 导航项。</span><span class="sxs-lookup"><span data-stu-id="f9726-114">You can now run the app, sign in, and click the **Calendar** navigation item in the left-hand menu.</span></span> <span data-ttu-id="f9726-115">您应该会看到用户日历上的事件的 JSON 转储。</span><span class="sxs-lookup"><span data-stu-id="f9726-115">You should see a JSON dump of the events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="f9726-116">显示结果</span><span class="sxs-lookup"><span data-stu-id="f9726-116">Display the results</span></span>

1. <span data-ttu-id="f9726-117">将全部内容替换为`CalendarPage.xaml`以下内容。</span><span class="sxs-lookup"><span data-stu-id="f9726-117">Replace the entire contents of `CalendarPage.xaml` with the following.</span></span>

    ```xaml
    <Page
        x:Class="GraphTutorial.CalendarPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:GraphTutorial"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:controls="using:Microsoft.Toolkit.Uwp.UI.Controls"
        mc:Ignorable="d"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <Grid>
            <controls:DataGrid x:Name="EventList" Grid.Row="1"
                    AutoGenerateColumns="False">
                <controls:DataGrid.Columns>
                    <controls:DataGridTextColumn
                            Header="Organizer"
                            Width="SizeToCells"
                            Binding="{Binding Organizer.EmailAddress.Name}"
                            FontSize="20" />
                    <controls:DataGridTextColumn
                            Header="Subject"
                            Width="SizeToCells"
                            Binding="{Binding Subject}"
                            FontSize="20" />
                    <controls:DataGridTextColumn
                            Header="Start"
                            Width="SizeToCells"
                            Binding="{Binding Start.DateTime}"
                            FontSize="20" />
                    <controls:DataGridTextColumn
                            Header="End"
                            Width="SizeToCells"
                            Binding="{Binding End.DateTime}"
                            FontSize="20" />
                </controls:DataGrid.Columns>
            </controls:DataGrid>
        </Grid>
    </Page>
    ```

1. <span data-ttu-id="f9726-118">打开`CalendarPage.xaml.cs`并将`Events.Text = JsonConvert.SerializeObject(events.CurrentPage);`行替换为以下代码行。</span><span class="sxs-lookup"><span data-stu-id="f9726-118">Open `CalendarPage.xaml.cs` and replace the `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` line with the following.</span></span>

    ```csharp
    EventList.ItemsSource = events.CurrentPage.ToList();
    ```

    <span data-ttu-id="f9726-119">如果现在运行应用程序并选择日历，则应获取数据网格中的事件列表。</span><span class="sxs-lookup"><span data-stu-id="f9726-119">If you run the app now and select the calendar, you should get a list of events in a data grid.</span></span> <span data-ttu-id="f9726-120">但是，**开始**和**结束**值以非用户友好方式显示。</span><span class="sxs-lookup"><span data-stu-id="f9726-120">However, the **Start** and **End** values are displayed in a non-user-friendly manner.</span></span> <span data-ttu-id="f9726-121">您可以使用[值转换器](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)控制这些值的显示方式。</span><span class="sxs-lookup"><span data-stu-id="f9726-121">You can control how those values are displayed by using a [value converter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).</span></span>

1. <span data-ttu-id="f9726-122">在 "解决方案资源管理器" 中右键单击 " **GraphTutorial** " 项目，然后选择 "**添加 > 类 ...**"。命名该类`GraphDateTimeTimeZoneConverter.cs`并选择 "**添加**"。</span><span class="sxs-lookup"><span data-stu-id="f9726-122">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > Class...**. Name the class `GraphDateTimeTimeZoneConverter.cs` and select **Add**.</span></span> <span data-ttu-id="f9726-123">将文件的全部内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="f9726-123">Replace the entire contents of the file with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/GraphDateTimeTimeZoneConverter.cs" id="ConverterSnippet":::

    <span data-ttu-id="f9726-124">此代码采用由 Microsoft Graph 返回的[datetimetimezone type](/graph/api/resources/datetimetimezone?view=graph-rest-1.0)结构，并将其分析`DateTimeOffset`为对象。</span><span class="sxs-lookup"><span data-stu-id="f9726-124">This code takes the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) structure returned by Microsoft Graph and parses it into a `DateTimeOffset` object.</span></span> <span data-ttu-id="f9726-125">然后，它将值转换为用户的时区并返回带格式的值。</span><span class="sxs-lookup"><span data-stu-id="f9726-125">It then converts the value into the user's time zone and returns the formatted value.</span></span>

1. <span data-ttu-id="f9726-126">打开`CalendarPage.xaml`并在`<Grid>`元素**前面**添加以下项。</span><span class="sxs-lookup"><span data-stu-id="f9726-126">Open `CalendarPage.xaml` and add the following **before** the `<Grid>` element.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="ResourcesSnippet":::

1. <span data-ttu-id="f9726-127">将最后两个`DataGridTextColumn`元素替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="f9726-127">Replace the last two `DataGridTextColumn` elements with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="BindingSnippet" highlight="4,9":::

1. <span data-ttu-id="f9726-128">运行应用程序，登录，然后单击 "**日历**" 导航项。</span><span class="sxs-lookup"><span data-stu-id="f9726-128">Run the app, sign in, and click the **Calendar** navigation item.</span></span> <span data-ttu-id="f9726-129">您应该会看到 "**开始**" 和 "**结束**" 值格式化的事件的列表。</span><span class="sxs-lookup"><span data-stu-id="f9726-129">You should see the list of events with the **Start** and **End** values formatted.</span></span>

    ![事件表的屏幕截图](./images/add-msgraph-01.png)
