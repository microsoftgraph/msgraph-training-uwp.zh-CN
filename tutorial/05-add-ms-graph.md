<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1ab2b-101">在此练习中，你将 Microsoft Graph 合并到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="1ab2b-102">对于此应用程序，你将使用 [适用于 .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) 的 Microsoft Graph 客户端库调用 Microsoft Graph。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-102">For this application, you will use the [Microsoft Graph Client Library for .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="1ab2b-103">从 Outlook 获取日历事件</span><span class="sxs-lookup"><span data-stu-id="1ab2b-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="1ab2b-104">为日历视图添加新页面。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-104">Add a new page for the calendar view.</span></span> <span data-ttu-id="1ab2b-105">在解决方案资源管理器中右键 **单击 GraphTu一l** 项目，**然后选择>新项目...** 选择 **"空白页**"， `CalendarPage.xaml` 在"名称"字段中输入，然后选择"**添加"。**</span><span class="sxs-lookup"><span data-stu-id="1ab2b-105">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `CalendarPage.xaml` in the **Name** field, and select **Add**.</span></span>

1. <span data-ttu-id="1ab2b-106">打开 `CalendarPage.xaml` 并添加现有元素中的以下 `<Grid>` 行。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-106">Open `CalendarPage.xaml` and add the following line inside the existing `<Grid>` element.</span></span>

    ```xaml
    <TextBlock x:Name="Events" TextWrapping="Wrap"/>
    ```

1. <span data-ttu-id="1ab2b-107">在 `CalendarPage.xaml.cs` 文件顶部 `using` 打开并添加以下语句。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-107">Open `CalendarPage.xaml.cs` and add the following `using` statements at the top of the file.</span></span>

    ```csharp
    using Microsoft.Graph;
    using Microsoft.Toolkit.Graph.Providers;
    using Microsoft.Toolkit.Uwp.UI.Controls;
    using Newtonsoft.Json;
    ```

1. <span data-ttu-id="1ab2b-108">将以下函数添加到 `CalendarPage` 类。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-108">Add the following functions to the `CalendarPage` class.</span></span>

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
            // Get the user's mailbox settings to determine
            // their time zone
            var user = await graphClient.Me.Request()
                .Select(u => new { u.MailboxSettings })
                .GetAsync();

            var startOfWeek = GetUtcStartOfWeekInTimeZone(DateTime.Today, user.MailboxSettings.TimeZone);
            var endOfWeek = startOfWeek.AddDays(7);

            var queryOptions = new List<QueryOption>
            {
                new QueryOption("startDateTime", startOfWeek.ToString("o")),
                new QueryOption("endDateTime", endOfWeek.ToString("o"))
            };

            // Get the events
            var events = await graphClient.Me.CalendarView.Request(queryOptions)
                .Header("Prefer", $"outlook.timezone=\"{user.MailboxSettings.TimeZone}\"")
                .Select(ev => new
                {
                    ev.Subject,
                    ev.Organizer,
                    ev.Start,
                    ev.End
                })
                .OrderBy("start/dateTime")
                .Top(50)
                .GetAsync();

            // TEMPORARY: Show the results as JSON
            Events.Text = JsonConvert.SerializeObject(events.CurrentPage);
        }
        catch (ServiceException ex)
        {
            ShowNotification($"Exception getting events: {ex.Message}");
        }

        base.OnNavigatedTo(e);
    }

    private static DateTime GetUtcStartOfWeekInTimeZone(DateTime today, string timeZoneId)
    {
        TimeZoneInfo userTimeZone = TimeZoneInfo.FindSystemTimeZoneById(timeZoneId);

        // Assumes Sunday as first day of week
        int diff = System.DayOfWeek.Sunday - today.DayOfWeek;

        // create date as unspecified kind
        var unspecifiedStart = DateTime.SpecifyKind(today.AddDays(diff), DateTimeKind.Unspecified);

        // convert to UTC
        return TimeZoneInfo.ConvertTimeToUtc(unspecifiedStart, userTimeZone);
    }
    ```

    <span data-ttu-id="1ab2b-109">考虑执行中的 `OnNavigatedTo` 代码。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-109">Consider with the code in `OnNavigatedTo` is doing.</span></span>

    - <span data-ttu-id="1ab2b-110">将调用的 URL 为 `/me/calendarview` 。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-110">The URL that will be called is `/me/calendarview`.</span></span>
        - <span data-ttu-id="1ab2b-111">和 `startDateTime` `endDateTime` 参数定义日历视图的开始和结束。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-111">The `startDateTime` and `endDateTime` parameters define the start and end of the calendar view.</span></span>
        - <span data-ttu-id="1ab2b-112">`Prefer: outlook.timezone`标头使事件和事件在用户的 `start` `end` 时区中返回。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-112">The `Prefer: outlook.timezone` header causes the `start` and `end` of the events to be returned in the user's time zone.</span></span>
        - <span data-ttu-id="1ab2b-113">`Select`该函数将每个事件返回的字段限制为仅应用将实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-113">The `Select` function limits the fields returned for each event to just those the app will actually use.</span></span>
        - <span data-ttu-id="1ab2b-114">该 `OrderBy` 函数按开始日期和时间对结果进行排序。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-114">The `OrderBy` function sorts the results by the start date and time.</span></span>
        - <span data-ttu-id="1ab2b-115">`Top`函数最多请求 50 个事件。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-115">The `Top` function requests at most 50 events.</span></span>

1. <span data-ttu-id="1ab2b-116">修改 `NavView_ItemInvoked` 文件中的方法 `MainPage.xaml.cs` 以将现有 `switch` 语句替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-116">Modify the `NavView_ItemInvoked` method in the `MainPage.xaml.cs` file to replace the existing `switch` statement with the following.</span></span>

    ```csharp
    switch (invokedItem.ToLower())
    {
        case "new event":
            throw new NotImplementedException();
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

<span data-ttu-id="1ab2b-117">现在，你可以运行应用、登录并单击左侧菜单中的日历导航项。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-117">You can now run the app, sign in, and click the **Calendar** navigation item in the left-hand menu.</span></span> <span data-ttu-id="1ab2b-118">您应该在用户的日历上看到事件的 JSON 转储。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-118">You should see a JSON dump of the events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="1ab2b-119">显示结果</span><span class="sxs-lookup"><span data-stu-id="1ab2b-119">Display the results</span></span>

1. <span data-ttu-id="1ab2b-120">将其中的全部 `CalendarPage.xaml` 内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-120">Replace the entire contents of `CalendarPage.xaml` with the following.</span></span>

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

1. <span data-ttu-id="1ab2b-121">打开 `CalendarPage.xaml.cs` 此行 `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` 并将其替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-121">Open `CalendarPage.xaml.cs` and replace the `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` line with the following.</span></span>

    ```csharp
    EventList.ItemsSource = events.CurrentPage.ToList();
    ```

    <span data-ttu-id="1ab2b-122">如果现在运行应用并选择日历，则应该获取数据网格中的事件列表。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-122">If you run the app now and select the calendar, you should get a list of events in a data grid.</span></span> <span data-ttu-id="1ab2b-123">但是 **，"开始**"和"结束"值以非用户友好方式显示。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-123">However, the **Start** and **End** values are displayed in a non-user-friendly manner.</span></span> <span data-ttu-id="1ab2b-124">可以使用值转换器控制这些值 [的显示方式](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-124">You can control how those values are displayed by using a [value converter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).</span></span>

1. <span data-ttu-id="1ab2b-125">在解决方案资源管理器中右键单击 **GraphTu一l** 项目，然后选择>**类...** 命名类 `GraphDateTimeTimeZoneConverter.cs` ，然后选择"**添加"。**</span><span class="sxs-lookup"><span data-stu-id="1ab2b-125">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > Class...**. Name the class `GraphDateTimeTimeZoneConverter.cs` and select **Add**.</span></span> <span data-ttu-id="1ab2b-126">将文件的全部内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-126">Replace the entire contents of the file with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/GraphDateTimeTimeZoneConverter.cs" id="ConverterSnippet":::

    <span data-ttu-id="1ab2b-127">此代码采用 Microsoft Graph 返回 [的 dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) 结构，并解析为 `DateTimeOffset` 对象。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-127">This code takes the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) structure returned by Microsoft Graph and parses it into a `DateTimeOffset` object.</span></span> <span data-ttu-id="1ab2b-128">然后，它将值转换为用户的时区并返回格式化的值。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-128">It then converts the value into the user's time zone and returns the formatted value.</span></span>

1. <span data-ttu-id="1ab2b-129">在 `CalendarPage.xaml` 元素之前打开 **并添加** `<Grid>` 以下内容。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-129">Open `CalendarPage.xaml` and add the following **before** the `<Grid>` element.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="ResourcesSnippet":::

1. <span data-ttu-id="1ab2b-130">将最后两 `DataGridTextColumn` 个元素替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-130">Replace the last two `DataGridTextColumn` elements with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="BindingSnippet" highlight="4,9":::

1. <span data-ttu-id="1ab2b-131">运行应用，登录，然后单击 **"日历"** 导航项。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-131">Run the app, sign in, and click the **Calendar** navigation item.</span></span> <span data-ttu-id="1ab2b-132">应该会看到设置"开始"和" **结束"值** 格式 **的事件列表** 。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-132">You should see the list of events with the **Start** and **End** values formatted.</span></span>

    ![事件表的屏幕截图](./images/add-msgraph-01.png)
