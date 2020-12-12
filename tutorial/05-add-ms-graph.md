<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将 Microsoft Graph 合并到应用程序中。 对于此应用程序，你将使用 [适用于 .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) 的 Microsoft Graph 客户端库调用 Microsoft Graph。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

1. 为日历视图添加新页面。 在解决方案资源管理器中右键 **单击 GraphTu一l** 项目，**然后选择>新项目...** 选择 **"空白页**"， `CalendarPage.xaml` 在"名称"字段中输入，然后选择"**添加"。**

1. 打开 `CalendarPage.xaml` 并添加现有元素中的以下 `<Grid>` 行。

    ```xaml
    <TextBlock x:Name="Events" TextWrapping="Wrap"/>
    ```

1. 在 `CalendarPage.xaml.cs` 文件顶部 `using` 打开并添加以下语句。

    ```csharp
    using Microsoft.Graph;
    using Microsoft.Toolkit.Graph.Providers;
    using Microsoft.Toolkit.Uwp.UI.Controls;
    using Newtonsoft.Json;
    ```

1. 将以下函数添加到 `CalendarPage` 类。

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

    考虑执行中的 `OnNavigatedTo` 代码。

    - 将调用的 URL 为 `/me/calendarview` 。
        - 和 `startDateTime` `endDateTime` 参数定义日历视图的开始和结束。
        - `Prefer: outlook.timezone`标头使事件和事件在用户的 `start` `end` 时区中返回。
        - `Select`该函数将每个事件返回的字段限制为仅应用将实际使用的字段。
        - 该 `OrderBy` 函数按开始日期和时间对结果进行排序。
        - `Top`函数最多请求 50 个事件。

1. 修改 `NavView_ItemInvoked` 文件中的方法 `MainPage.xaml.cs` 以将现有 `switch` 语句替换为以下内容。

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

现在，你可以运行应用、登录并单击左侧菜单中的日历导航项。 您应该在用户的日历上看到事件的 JSON 转储。

## <a name="display-the-results"></a>显示结果

1. 将其中的全部 `CalendarPage.xaml` 内容替换为以下内容。

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

1. 打开 `CalendarPage.xaml.cs` 此行 `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` 并将其替换为以下内容。

    ```csharp
    EventList.ItemsSource = events.CurrentPage.ToList();
    ```

    如果现在运行应用并选择日历，则应该获取数据网格中的事件列表。 但是 **，"开始**"和"结束"值以非用户友好方式显示。 可以使用值转换器控制这些值 [的显示方式](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)。

1. 在解决方案资源管理器中右键单击 **GraphTu一l** 项目，然后选择>**类...** 命名类 `GraphDateTimeTimeZoneConverter.cs` ，然后选择"**添加"。** 将文件的全部内容替换为以下内容。

    :::code language="csharp" source="../demo/GraphTutorial/GraphDateTimeTimeZoneConverter.cs" id="ConverterSnippet":::

    此代码采用 Microsoft Graph 返回 [的 dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) 结构，并解析为 `DateTimeOffset` 对象。 然后，它将值转换为用户的时区并返回格式化的值。

1. 在 `CalendarPage.xaml` 元素之前打开 **并添加** `<Grid>` 以下内容。

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="ResourcesSnippet":::

1. 将最后两 `DataGridTextColumn` 个元素替换为以下内容。

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="BindingSnippet" highlight="4,9":::

1. 运行应用，登录，然后单击 **"日历"** 导航项。 应该会看到设置"开始"和" **结束"值** 格式 **的事件列表** 。

    ![事件表的屏幕截图](./images/add-msgraph-01.png)
