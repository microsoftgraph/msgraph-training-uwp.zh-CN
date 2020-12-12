<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="90654-101">本教程指导你如何生成通用 Windows 平台 (UWP) 应用，该应用使用 Microsoft Graph API 检索用户的日历信息。</span><span class="sxs-lookup"><span data-stu-id="90654-101">This tutorial teaches you how to build a Universal Windows Platform (UWP) app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="90654-102">如果你更喜欢仅下载已完成的教程，可以下载或克隆 [GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-uwp)。</span><span class="sxs-lookup"><span data-stu-id="90654-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-uwp).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90654-103">先决条件</span><span class="sxs-lookup"><span data-stu-id="90654-103">Prerequisites</span></span>

<span data-ttu-id="90654-104">在开始本教程之前，你应该先在[Visual Studio](https://visualstudio.microsoft.com/vs/) Windows 10（已打开开发人员模式）的计算机上安装[。](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)</span><span class="sxs-lookup"><span data-stu-id="90654-104">Before you start this tutorial, you should have [Visual Studio](https://visualstudio.microsoft.com/vs/) installed on a computer running Windows 10 with [Developer mode turned on](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).</span></span> <span data-ttu-id="90654-105">如果您没有此Visual Studio，请访问上一链接，查看下载选项。</span><span class="sxs-lookup"><span data-stu-id="90654-105">If you do not have Visual Studio, visit the previous link for download options.</span></span>

<span data-ttu-id="90654-106">你还应该拥有具有邮箱的个人 Microsoft 帐户Outlook.com或 Microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="90654-106">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="90654-107">如果你没有 Microsoft 帐户，有几个选项可以获取免费帐户：</span><span class="sxs-lookup"><span data-stu-id="90654-107">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="90654-108">你可以 [注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="90654-108">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="90654-109">你可以 [注册 Office 365 开发人员计划](https://developer.microsoft.com/office/dev-program) ，获取免费的 Office 365 订阅。</span><span class="sxs-lookup"><span data-stu-id="90654-109">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="90654-110">本教程是使用 Visual Studio 2019 版本 16.8.3 编写的。</span><span class="sxs-lookup"><span data-stu-id="90654-110">This tutorial was written with Visual Studio 2019 version 16.8.3.</span></span> <span data-ttu-id="90654-111">本指南中的步骤可能与其他版本一起运行，但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="90654-111">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="90654-112">反馈</span><span class="sxs-lookup"><span data-stu-id="90654-112">Feedback</span></span>

<span data-ttu-id="90654-113">请在 GitHub 存储库中提供有关本教程 [的任何反馈](https://github.com/microsoftgraph/msgraph-training-uwp)。</span><span class="sxs-lookup"><span data-stu-id="90654-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-uwp).</span></span>
