<!-- markdownlint-disable MD002 MD041 -->

在本练习中, 将把 Microsoft Graph 合并到应用程序中。 对于此应用程序, 您将使用[Microsoft Graph 客户端库进行 .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet)以调用 microsoft graph。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

首先添加 "日历" 视图的新页面。 右键单击 "解决方案资源管理器" 中的 "**教程**" 项目, 然后选择 "**添加 > 新项 ...**"。选择 "**空白页**" `CalendarPage.xaml` , 在 "**名称**" 字段中输入, 然后选择 "**添加**"。

打开`CalendarPage.xaml`并将以下行添加到现有`<Grid>`元素中。

```xml
<TextBlock x:Name="Events" TextWrapping="Wrap"/>
```

打开`CalendarPage.xaml.cs`并将以下`using`语句添加到文件顶部。

```cs
using Microsoft.Toolkit.Services.MicrosoftGraph;
using Microsoft.Toolkit.Uwp.UI.Controls;
using Newtonsoft.Json;
```

然后, 将以下函数添加到`CalendarPage`类中。

```cs
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
    // Get the Graph client from the service
    var graphClient = MicrosoftGraphService.Instance.GraphProvider;

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

考虑中`OnNavigatedTo`的代码执行操作。

- 将调用的 URL 为`/v1.0/me/events`。
- `Select`函数将为每个事件返回的字段限制为仅供视图实际使用的字段。
- `OrderBy`函数按其创建日期和时间对结果进行排序, 最新项目最先开始。

在运行应用程序之前, 为了能够导航到此日历页, 请修改`NavView_ItemInvoked` `MainPage.xaml.cs`文件中的方法以替换`throw new NotImplementedException();`该行, 如下所示。

```cs
case "calendar":
    RootFrame.Navigate(typeof(CalendarPage));
    break;
```

您现在可以运行应用程序, 登录, 然后单击左侧菜单中的 "**日历**" 导航项。 您应该会看到用户日历上的事件的 JSON 转储。

## <a name="display-the-results"></a>显示结果

现在, 您可以将 JSON 转储替换为以用户友好的方式显示结果的内容。 将全部内容替换为`CalendarPage.xaml`以下内容。

```xml
<Page
    x:Class="graph_tutorial.CalendarPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:graph_tutorial"
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

这会将`TextBlock`替换替换`DataGrid`为。 现在打开`CalendarPage.xaml.cs`并将`Events.Text = JsonConvert.SerializeObject(events.CurrentPage);`行替换为以下代码行。

```cs
EventList.ItemsSource = events.CurrentPage.ToList();
```

如果现在运行应用程序并选择日历, 则应获取数据网格中的事件列表。 但是,**开始**和**结束**值以非用户友好方式显示。 您可以使用[值转换器](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)控制这些值的显示方式。

右键单击 "解决方案资源管理器" 中的 "**教程**" 项目, 然后选择 "**添加 > 类 ...**"。命名该类`GraphDateTimeTimeZoneConverter.cs`并选择 "**添加**"。 将文件的全部内容替换为以下内容。

```cs
using Microsoft.Graph;
using System;

namespace graph_tutorial
{
    class GraphDateTimeTimeZoneConverter : Windows.UI.Xaml.Data.IValueConverter
    {
        public object Convert(object value, Type targetType, object parameter, string language)
        {
            DateTimeTimeZone date = value as DateTimeTimeZone;

            if (date != null)
            {
                // Resolve the time zone
                var timezone = TimeZoneInfo.FindSystemTimeZoneById(date.TimeZone);
                // Parse method assumes local time, which may not be the case
                var parsedDateAsLocal = DateTimeOffset.Parse(date.DateTime);
                // Determine the offset from UTC time for the specific date
                // Making this call adjusts for DST as appropriate
                var tzOffset = timezone.GetUtcOffset(parsedDateAsLocal.DateTime);
                // Create a new DateTimeOffset with the specific offset from UTC
                var correctedDate = new DateTimeOffset(parsedDateAsLocal.DateTime, tzOffset);
                // Return the local date time string
                return correctedDate.LocalDateTime.ToString();
            }

            return string.Empty;
        }

        public object ConvertBack(object value, Type targetType, object parameter, string language)
        {
            throw new NotImplementedException();
        }
    }
}
```

此代码采用由 Microsoft Graph 返回的[datetimetimezone type](https://docs.microsoft.com/graph/api/resources/datetimetimezone?view=graph-rest-1.0)结构, 并将其分析`DateTimeOffset`为对象。 然后, 它将值转换为用户的时区并返回带格式的值。

打开`CalendarPage.xaml`并在`<Grid>`元素**前面**添加以下项。

```xml
<Page.Resources>
    <local:GraphDateTimeTimeZoneConverter x:Key="DateTimeTimeZoneValueConverter" />
</Page.Resources>
```

然后, 将`Binding="{Binding Start.DateTime}"`行替换为以下代码行。

```xml
Binding="{Binding Start, Converter={StaticResource DateTimeTimeZoneValueConverter}}"
```

将`Binding="{Binding End.DateTime}"`行替换为以下代码行。

```xml
Binding="{Binding End, Converter={StaticResource DateTimeTimeZoneValueConverter}}"
```

运行应用程序, 登录, 然后单击 "**日历**" 导航项。 您应该会看到 "**开始**" 和 "**结束**" 值格式化的事件的列表。

![事件表的屏幕截图](./images/add-msgraph-01.png)
