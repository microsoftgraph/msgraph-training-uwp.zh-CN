<!-- markdownlint-disable MD002 MD041 -->

本教程向您介绍如何构建通用 Windows 平台（UWP）应用程序，该应用程序使用 Microsoft Graph API 检索用户的日历信息。

> [!TIP]
> 如果您只想下载已完成的教程，可以下载或克隆[GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-uwp)。

## <a name="prerequisites"></a>先决条件

在开始本教程之前，应在运行 Windows 10 的计算机上安装[Visual Studio](https://visualstudio.microsoft.com/vs/) ，并[启用开发人员模式](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)。 如果没有 Visual Studio，请访问 "下载选项" 的上一个链接。

您还应具有一个个人的 Microsoft 帐户，其中包含 Outlook.com 上的邮箱，或 Microsoft 工作或学校帐户。 如果你没有 Microsoft 帐户，可以使用以下几种方法获取免费帐户：

- 你可以[注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。
- 你可以[注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program)以获取免费的 office 365 订阅。

> [!NOTE]
> 本教程是使用 Visual Studio 2019 版本16.5.0 编写的。 本指南中的步骤可能适用于其他版本，但尚未经过测试。

## <a name="watch-the-tutorial"></a>观看教程

此模块已记录，在 Office 开发 YouTube 频道中可用。

<!-- markdownlint-disable MD033 MD034 -->
<br/>

> [!VIDEO https://www.youtube-nocookie.com/embed/oBYCBxkWMRA]
<!-- markdownlint-enable MD033 MD034 -->

## <a name="feedback"></a>反馈

请在[GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-uwp)中提供有关本教程的任何反馈。
