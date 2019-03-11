# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="192f3-101">如何运行已完成的项目</span><span class="sxs-lookup"><span data-stu-id="192f3-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="192f3-102">先决条件</span><span class="sxs-lookup"><span data-stu-id="192f3-102">Prerequisites</span></span>

<span data-ttu-id="192f3-103">若要在此文件夹中运行已完成的项目, 您需要以下各项:</span><span class="sxs-lookup"><span data-stu-id="192f3-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="192f3-104">在开发计算机上安装的[Visual Studio](https://visualstudio.microsoft.com/vs/) 。</span><span class="sxs-lookup"><span data-stu-id="192f3-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) installed on your development machine.</span></span> <span data-ttu-id="192f3-105">如果没有 Visual Studio, 请访问 "下载选项" 的上一个链接。</span><span class="sxs-lookup"><span data-stu-id="192f3-105">If you do not have Visual Studio, visit the previous link for download options.</span></span> <span data-ttu-id="192f3-106">(**注意:** 本教程是使用 Visual Studio 2017 版本15.81 编写的。</span><span class="sxs-lookup"><span data-stu-id="192f3-106">(**Note:** This tutorial was written with Visual Studio 2017 version 15.81.</span></span> <span data-ttu-id="192f3-107">本指南中的步骤可能适用于其他版本, 但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="192f3-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="192f3-108">使用 Outlook.com 上的邮箱的个人 Microsoft 帐户, 或者是 microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="192f3-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="192f3-109">如果你没有 Microsoft 帐户, 可以使用以下几种方法获取免费帐户:</span><span class="sxs-lookup"><span data-stu-id="192f3-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="192f3-110">你可以[注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="192f3-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="192f3-111">你可以[注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program)以获取免费的 office 365 订阅。</span><span class="sxs-lookup"><span data-stu-id="192f3-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-native-application-with-the-application-registration-portal"></a><span data-ttu-id="192f3-112">在应用程序注册门户中注册本机应用程序</span><span class="sxs-lookup"><span data-stu-id="192f3-112">Register a native application with the Application Registration Portal</span></span>

1. <span data-ttu-id="192f3-113">打开浏览器并导航到[应用程序注册门户](https://apps.dev.microsoft.com), 并使用**个人帐户**(亦称: Microsoft 帐户) 或**工作或学校帐户**进行登录。</span><span class="sxs-lookup"><span data-stu-id="192f3-113">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="192f3-114">选择页面顶部的 "**添加应用**"。</span><span class="sxs-lookup"><span data-stu-id="192f3-114">Select **Add an app** at the top of the page.</span></span>

    > <span data-ttu-id="192f3-115">**注意:** 如果在页面上看到多个 "**添加应用程序**" 按钮, 请选择与 "**聚合应用程序**" 列表对应的项。</span><span class="sxs-lookup"><span data-stu-id="192f3-115">**Note:** If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="192f3-116">在 "**注册应用程序**" 页上, 将**应用程序名称**设置为**UWP Graph 教程**, 然后选择 "**创建**"。</span><span class="sxs-lookup"><span data-stu-id="192f3-116">On the **Register your application** page, set the **Application Name** to **UWP Graph Tutorial** and select **Create**.</span></span>

    ![在应用注册门户网站中创建新应用程序的屏幕截图](../../../Images/arp-create-app-01.png)

1. <span data-ttu-id="192f3-118">在 " **UWP Graph 教程注册**" 页上的 "**属性**" 部分下, 复制**应用程序 Id** , 因为稍后将需要它。</span><span class="sxs-lookup"><span data-stu-id="192f3-118">On the **UWP Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![新创建的应用程序 ID 的屏幕截图](../../../Images/arp-create-app-02.png)

1. <span data-ttu-id="192f3-120">向下滚动到 "**平台**" 部分。</span><span class="sxs-lookup"><span data-stu-id="192f3-120">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="192f3-121">选择 "**添加平台**"。</span><span class="sxs-lookup"><span data-stu-id="192f3-121">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="192f3-122">在 "**添加平台**" 对话框中, 选择 "**本机应用程序**"。</span><span class="sxs-lookup"><span data-stu-id="192f3-122">In the **Add Platform** dialog, select **Native Application**.</span></span>

        ![为应用程序创建平台的屏幕截图](../../../Images/arp-create-app-03.png)

1. <span data-ttu-id="192f3-124">滚动到页面底部, 然后选择 "**保存**"。</span><span class="sxs-lookup"><span data-stu-id="192f3-124">Scroll to the bottom of the page and select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="192f3-125">配置示例</span><span class="sxs-lookup"><span data-stu-id="192f3-125">Configure the sample</span></span>

1. <span data-ttu-id="192f3-126">将`OAuth.resw.example`文件重命名`OAuth.resw`为。</span><span class="sxs-lookup"><span data-stu-id="192f3-126">Rename the `OAuth.resw.example` file to `OAuth.resw`.</span></span>
1. <span data-ttu-id="192f3-127">在`graph-tutorial.sln` Visual Studio 中打开。</span><span class="sxs-lookup"><span data-stu-id="192f3-127">Open `graph-tutorial.sln` in Visual Studio.</span></span>
1. <span data-ttu-id="192f3-128">在 visual `OAuth.resw` studio 中编辑文件。将`YOUR_APP_ID_HERE`替换为你从应用注册门户获取的**应用程序 Id** 。</span><span class="sxs-lookup"><span data-stu-id="192f3-128">Edit the `OAuth.resw` file in visual studio.Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="192f3-129">在 "解决方案资源管理器" 中, 右键单击**graph 教程**解决方案并选择 "**还原 NuGet 包**"。</span><span class="sxs-lookup"><span data-stu-id="192f3-129">In Solution Explorer, right-click the **graph-tutorial** solution and choose **Restore NuGet Packages**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="192f3-130">运行示例</span><span class="sxs-lookup"><span data-stu-id="192f3-130">Run the sample</span></span>

<span data-ttu-id="192f3-131">在 Visual Studio 中, 按**F5**或选择 "**调试" > "开始调试**"。</span><span class="sxs-lookup"><span data-stu-id="192f3-131">In Visual Studio, press **F5** or choose **Debug > Start Debugging**.</span></span>