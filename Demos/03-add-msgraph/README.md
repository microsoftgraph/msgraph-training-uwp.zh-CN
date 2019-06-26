# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="db73d-101">如何运行已完成的项目</span><span class="sxs-lookup"><span data-stu-id="db73d-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db73d-102">先决条件</span><span class="sxs-lookup"><span data-stu-id="db73d-102">Prerequisites</span></span>

<span data-ttu-id="db73d-103">若要在此文件夹中运行已完成的项目, 您需要以下各项:</span><span class="sxs-lookup"><span data-stu-id="db73d-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="db73d-104">在开发计算机上安装的[Visual Studio](https://visualstudio.microsoft.com/vs/) 。</span><span class="sxs-lookup"><span data-stu-id="db73d-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) installed on your development machine.</span></span> <span data-ttu-id="db73d-105">如果没有 Visual Studio, 请访问 "下载选项" 的上一个链接。</span><span class="sxs-lookup"><span data-stu-id="db73d-105">If you do not have Visual Studio, visit the previous link for download options.</span></span> <span data-ttu-id="db73d-106">(**注意:** 本教程是使用 Visual Studio 2017 版本15.81 编写的。</span><span class="sxs-lookup"><span data-stu-id="db73d-106">(**Note:** This tutorial was written with Visual Studio 2017 version 15.81.</span></span> <span data-ttu-id="db73d-107">本指南中的步骤可能适用于其他版本, 但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="db73d-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="db73d-108">使用 Outlook.com 上的邮箱的个人 Microsoft 帐户, 或者是 Microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="db73d-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="db73d-109">如果你没有 Microsoft 帐户, 可以使用以下几种方法获取免费帐户:</span><span class="sxs-lookup"><span data-stu-id="db73d-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="db73d-110">你可以[注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="db73d-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="db73d-111">你可以[注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program)以获取免费的 office 365 订阅。</span><span class="sxs-lookup"><span data-stu-id="db73d-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-native-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="db73d-112">在 Azure Active Directory 管理中心注册本机应用程序</span><span class="sxs-lookup"><span data-stu-id="db73d-112">Register a native application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="db73d-113">打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。然后，使用**个人帐户**（亦称为“Microsoft 帐户”）或**工作或学校帐户**登录。</span><span class="sxs-lookup"><span data-stu-id="db73d-113">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="db73d-114">在左侧导航栏中选择 " **Azure Active Directory** ", 然后选择 "**管理**" 下的 "**应用程序注册**"。</span><span class="sxs-lookup"><span data-stu-id="db73d-114">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="db73d-115">应用注册的屏幕截图</span><span class="sxs-lookup"><span data-stu-id="db73d-115">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="db73d-116">选择“新注册”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="db73d-116">Select **New registration**.</span></span> <span data-ttu-id="db73d-117">在“注册应用”\*\*\*\* 页上，按如下方式设置值。</span><span class="sxs-lookup"><span data-stu-id="db73d-117">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="db73d-118">将“名称”\*\*\*\* 设置为“`UWP Graph Tutorial`”。</span><span class="sxs-lookup"><span data-stu-id="db73d-118">Set **Name** to `UWP Graph Tutorial`.</span></span>
    - <span data-ttu-id="db73d-119">将“受支持的帐户类型”\*\*\*\* 设置为“任何组织目录中的帐户和个人 Microsoft 帐户”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="db73d-119">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="db73d-120">在 "**重定向 URI**" 下, 将下拉列表更改为 "**公用客户端 (移动 & 桌面)**", 并将值设置为`urn:ietf:wg:oauth:2.0:oob`。</span><span class="sxs-lookup"><span data-stu-id="db73d-120">Under **Redirect URI**, change the dropdown to **Public client (mobile & desktop)**, and set the value to `urn:ietf:wg:oauth:2.0:oob`.</span></span>

    !["注册应用程序" 页的屏幕截图](/tutorial/images/aad-register-app.png)

1. <span data-ttu-id="db73d-122">选择“注册”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="db73d-122">Choose **Register**.</span></span> <span data-ttu-id="db73d-123">在**UWP Graph 教程**页上, 复制**应用程序 (客户端) ID**的值并保存它, 下一步将需要它。</span><span class="sxs-lookup"><span data-stu-id="db73d-123">On the **UWP Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新应用注册的应用程序 ID 的屏幕截图](/tutorial/images/aad-application-id.png)

## <a name="configure-the-sample"></a><span data-ttu-id="db73d-125">配置示例</span><span class="sxs-lookup"><span data-stu-id="db73d-125">Configure the sample</span></span>

1. <span data-ttu-id="db73d-126">将`OAuth.resw.example`文件重命名`OAuth.resw`为。</span><span class="sxs-lookup"><span data-stu-id="db73d-126">Rename the `OAuth.resw.example` file to `OAuth.resw`.</span></span>
1. <span data-ttu-id="db73d-127">在`graph-tutorial.sln` Visual Studio 中打开。</span><span class="sxs-lookup"><span data-stu-id="db73d-127">Open `graph-tutorial.sln` in Visual Studio.</span></span>
1. <span data-ttu-id="db73d-128">在 visual `OAuth.resw` studio 中编辑文件。将`YOUR_APP_ID_HERE`替换为你从应用注册门户获取的**应用程序 Id** 。</span><span class="sxs-lookup"><span data-stu-id="db73d-128">Edit the `OAuth.resw` file in visual studio.Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="db73d-129">在 "解决方案资源管理器" 中, 右键单击**graph 教程**解决方案并选择 "**还原 NuGet 包**"。</span><span class="sxs-lookup"><span data-stu-id="db73d-129">In Solution Explorer, right-click the **graph-tutorial** solution and choose **Restore NuGet Packages**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="db73d-130">运行示例</span><span class="sxs-lookup"><span data-stu-id="db73d-130">Run the sample</span></span>

<span data-ttu-id="db73d-131">在 Visual Studio 中, 按**F5**或选择 "**调试" > "开始调试**"。</span><span class="sxs-lookup"><span data-stu-id="db73d-131">In Visual Studio, press **F5** or choose **Debug > Start Debugging**.</span></span>