<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="49cab-101">本教程向您介绍如何构建通用 Windows 平台 (UWP) 应用程序, 该应用程序使用 Microsoft Graph API 检索用户的日历信息。</span><span class="sxs-lookup"><span data-stu-id="49cab-101">This tutorial teaches you how to build a Universal Windows Platform (UWP) app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="49cab-102">如果您只想下载已完成的教程, 可以下载或克隆[GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-uwp)。</span><span class="sxs-lookup"><span data-stu-id="49cab-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-uwp).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49cab-103">先决条件</span><span class="sxs-lookup"><span data-stu-id="49cab-103">Prerequisites</span></span>

<span data-ttu-id="49cab-104">在开始本教程之前, 应在运行 Windows 10 的计算机上安装[Visual Studio](https://visualstudio.microsoft.com/vs/) , 并[启用开发人员模式](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)。</span><span class="sxs-lookup"><span data-stu-id="49cab-104">Before you start this tutorial, you should have [Visual Studio](https://visualstudio.microsoft.com/vs/) installed on a computer running Windows 10 with [Developer mode turned on](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).</span></span> <span data-ttu-id="49cab-105">如果没有 Visual Studio, 请访问 "下载选项" 的上一个链接。</span><span class="sxs-lookup"><span data-stu-id="49cab-105">If you do not have Visual Studio, visit the previous link for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="49cab-106">本教程是使用 Visual Studio 2017 版本15.8.1 编写的。</span><span class="sxs-lookup"><span data-stu-id="49cab-106">This tutorial was written with Visual Studio 2017 version 15.8.1.</span></span> <span data-ttu-id="49cab-107">本指南中的步骤可能适用于其他版本, 但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="49cab-107">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="49cab-108">反馈</span><span class="sxs-lookup"><span data-stu-id="49cab-108">Feedback</span></span>

<span data-ttu-id="49cab-109">请在[GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-uwp)中提供有关本教程的任何反馈。</span><span class="sxs-lookup"><span data-stu-id="49cab-109">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-uwp).</span></span>