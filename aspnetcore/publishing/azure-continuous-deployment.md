---
title: "使用 Visual Studio 與 Git 持續部署到 Azure"
author: rick-anderson
description: "了解如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過 Git 持續部署將它部署到 Azure App Service。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: f7ea2e76fdee19a3d964e42053f0060a0a505e5b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="b3192-104">使用 Visual Studio 與 Git 持續部署到適用於 ASP.NET Core 的 Azure</span><span class="sxs-lookup"><span data-stu-id="b3192-104">Continuous deployment to Azure for ASP.NET Core, with Visual Studio and Git</span></span>

<span data-ttu-id="b3192-105">作者：[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="b3192-105">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="b3192-106">本教學課程說明如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過連續部署將它從 Visual Studio 部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="b3192-106">This tutorial shows you how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span> 

<span data-ttu-id="b3192-107">另請參閱 [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic) (使用 VSTS 來建立 Azure Web 應用程式，並透過連續部署來發行)，其中說明如何使用 Visual Studio Team Services 來設定 [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) 的持續傳遞 (CD) 工作流程。</span><span class="sxs-lookup"><span data-stu-id="b3192-107">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) using Visual Studio Team Services.</span></span> <span data-ttu-id="b3192-108">Team Services 中的 Azure 持續傳遞功能，可簡化將應用程式更新發行到 Azure App Service 的部署管道設定。</span><span class="sxs-lookup"><span data-stu-id="b3192-108">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for your app to Azure App Service.</span></span> <span data-ttu-id="b3192-109">您可以透過 Azure 入口網站設定這個管道，以建置、執行測試、部署到預備位置，然後再部署到生產環境。</span><span class="sxs-lookup"><span data-stu-id="b3192-109">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot,  and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="b3192-110">若要完成本教學課程，您需要 Microsoft Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="b3192-110">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="b3192-111">如果您沒有帳戶，可以[啟動 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)或[註冊免費試用帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="b3192-111">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b3192-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="b3192-112">Prerequisites</span></span>

<span data-ttu-id="b3192-113">本教學課程假設您已安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="b3192-113">This tutorial assumes you have already installed the following:</span></span>

* [<span data-ttu-id="b3192-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b3192-114">Visual Studio</span></span>](https://www.visualstudio.com)

* <span data-ttu-id="b3192-115">[ASP.NET Core](https://www.microsoft.com/net/download/core) (執行階段和工具)</span><span class="sxs-lookup"><span data-stu-id="b3192-115">[ASP.NET Core](https://www.microsoft.com/net/download/core) (runtime and tooling)</span></span>

* <span data-ttu-id="b3192-116">[Git](https://git-scm.com/downloads) (適用於 Windows)</span><span class="sxs-lookup"><span data-stu-id="b3192-116">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="b3192-117">建立 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b3192-117">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="b3192-118">啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="b3192-118">Start Visual Studio.</span></span>

2. <span data-ttu-id="b3192-119">從 [檔案] 功能表選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="b3192-119">From the **File** menu, select **New** > **Project**.</span></span>

3. <span data-ttu-id="b3192-120">選取 [ASP.NET Core Web 應用程式] 專案範本。</span><span class="sxs-lookup"><span data-stu-id="b3192-120">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="b3192-121">它會出現在 [已安裝] > [範本] > [Visual C#] > [.NET Core] 下。</span><span class="sxs-lookup"><span data-stu-id="b3192-121">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="b3192-122">將專案命名為 `SampleWebAppDemo`。</span><span class="sxs-lookup"><span data-stu-id="b3192-122">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="b3192-123">選取 [建立新的 Git 存放庫] 選項，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="b3192-123">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![[新增專案] 對話](azure-continuous-deployment/_static/01-new-project.png)

4. <span data-ttu-id="b3192-125">在 [新增 ASP.NET Core 專案] 對話方塊中，選取 ASP.NET Core 的 [空白] 範本，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="b3192-125">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![[新增 ASP.NET 專案] 對話方塊](azure-continuous-deployment/_static/02-web-site-template.png)

>[!NOTE]
    ><span data-ttu-id="b3192-127">.NET Core 的最新版本為 2.0</span><span class="sxs-lookup"><span data-stu-id="b3192-127">Most recent release of .NET Core is 2.0</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="b3192-128">在本機執行 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b3192-128">Running the web app locally</span></span>

1. <span data-ttu-id="b3192-129">一旦 Visual Studio 完成建立應用程式之後，您即可選取 [偵錯] -> [開始偵錯] 以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3192-129">Once Visual Studio finishes creating the app, run the app by selecting **Debug** -> **Start Debugging**.</span></span> <span data-ttu-id="b3192-130">或者，也可以按 **F5**。</span><span class="sxs-lookup"><span data-stu-id="b3192-130">As an alternative, you can press **F5**.</span></span>

   <span data-ttu-id="b3192-131">系統可能需要一點時間來初始化 Visual Studio 和新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3192-131">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="b3192-132">完成後，瀏覽器即會顯示執行中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3192-132">Once it is complete, the browser will show the running app.</span></span>

   ![顯示執行中應用程式的瀏覽器視窗，該應用程式顯示 'Hello World!'](azure-continuous-deployment/_static/04-browser-runapp.png)

2. <span data-ttu-id="b3192-134">在您檢閱執行中的 Web 應用程式之後，請關閉瀏覽器，然後按一下 Visual Studio 工具列中的「停止偵錯」圖示以停止應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3192-134">After reviewing the running Web app, close the browser and click the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="b3192-135">在 Azure 入口網站中建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b3192-135">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="b3192-136">下列步驟將引導您在 Azure 入口網站中建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3192-136">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="b3192-137">登入 [Azure 入口網站](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="b3192-137">Log in to the [Azure Portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="b3192-138">點選入口網站左上角的 [新增]</span><span class="sxs-lookup"><span data-stu-id="b3192-138">TAP **NEW** at the top left of the Portal</span></span>
3. <span data-ttu-id="b3192-139">點選 [Web + 行動] > [Web 應用程式]</span><span class="sxs-lookup"><span data-stu-id="b3192-139">TAP **Web + Mobile** > **Web App**</span></span>

    ![Microsoft Azure 入口網站：新按鈕：Marketplace 下方的 [Web + 行動]：[精選 App] 下方的 [Web 應用程式] 按鈕](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  <span data-ttu-id="b3192-141">在 [Web 應用程式] 刀鋒視窗中，為 [App Service 名稱] 輸入唯一值。</span><span class="sxs-lookup"><span data-stu-id="b3192-141">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

    ![[Web 應用程式] 刀鋒視窗](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    ><span data-ttu-id="b3192-143">[App Service 名稱] 必須是唯一的名稱。</span><span class="sxs-lookup"><span data-stu-id="b3192-143">The **App Service Name** name needs to be unique.</span></span> <span data-ttu-id="b3192-144">當您嘗試輸入名稱時，入口網站會強制執行這項規則。</span><span class="sxs-lookup"><span data-stu-id="b3192-144">The portal will enforce this rule when you attempt to enter the name.</span></span> <span data-ttu-id="b3192-145">輸入不同的值之後，您必須將本教學課程中看到的每個 **SampleWebAppDemo** 取代為該值。</span><span class="sxs-lookup"><span data-stu-id="b3192-145">After you enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span>

    &nbsp;
    
    <span data-ttu-id="b3192-146">此外，請在 [Web 應用程式] 刀鋒視窗中，選取現有的 [App Service 方案/位置] 或另外新建一個。</span><span class="sxs-lookup"><span data-stu-id="b3192-146">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="b3192-147">如果您建立新的方案，請選取定價層、位置和其他選項。</span><span class="sxs-lookup"><span data-stu-id="b3192-147">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="b3192-148">如需 App Service 方案的詳細資訊，請參閱 [Azure App Service 方案深入概觀](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/)。</span><span class="sxs-lookup"><span data-stu-id="b3192-148">For more information on App Service plans, [Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span></span>

5.  <span data-ttu-id="b3192-149">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="b3192-149">Click **Create**.</span></span> <span data-ttu-id="b3192-150">Azure 即會佈建並啟動您的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3192-150">Azure will provision and start your web app.</span></span>

    ![Azure 入口網站：範例 Web 應用程式示範 01 [基本資訊] 刀鋒視窗](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="b3192-152">啟用新 Web 應用程式的 Git 發行功能</span><span class="sxs-lookup"><span data-stu-id="b3192-152">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="b3192-153">Git 是一種分散式版本控制系統，可用來部署您的 Azure App Service Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3192-153">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="b3192-154">您可將針對 Web 應用程式所撰寫的程式碼儲存在本機 Git 存放庫，並將這些程式碼推送至遠端存放庫以部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="b3192-154">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="b3192-155">登入 [Azure 入口網站](https://portal.azure.com) (如果您尚未登入)。</span><span class="sxs-lookup"><span data-stu-id="b3192-155">Log into the [Azure Portal](https://portal.azure.com), if you're not already logged in.</span></span>

2. <span data-ttu-id="b3192-156">按一下 [應用程式服務]，即可檢視與 Azure 訂用帳戶相關聯的應用程式服務清單。</span><span class="sxs-lookup"><span data-stu-id="b3192-156">Click **App Services** to view a list of the app services associated with your Azure subscription.</span></span>

3. <span data-ttu-id="b3192-157">選取您在本教學課程上一節所建立的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3192-157">Select the web app you created in the previous section of this tutorial.</span></span>

4. <span data-ttu-id="b3192-158">在 [開發] 刀鋒視窗中，選取 [部署選項] > [選擇來源] > [本機 Git 存放庫]。</span><span class="sxs-lookup"><span data-stu-id="b3192-158">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![[設定] 刀鋒視窗：[部署來源] 刀鋒視窗：[選擇來源] 刀鋒視窗](azure-continuous-deployment/_static/deployment-options.png)

5. <span data-ttu-id="b3192-160">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="b3192-160">Click **OK**.</span></span>

6. <span data-ttu-id="b3192-161">如果您先前尚未設定用來發行 Web 應用程式或其他 App Service 應用程式的部署認證，請立即設定：</span><span class="sxs-lookup"><span data-stu-id="b3192-161">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>

   * <span data-ttu-id="b3192-162">按一下 [設定] > [部署認證]。</span><span class="sxs-lookup"><span data-stu-id="b3192-162">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="b3192-163">[設定部署認證] 刀鋒視窗隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="b3192-163">The **Set deployment credentials** blade will be displayed.</span></span>

   * <span data-ttu-id="b3192-164">建立使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="b3192-164">Create a user name and password.</span></span>  <span data-ttu-id="b3192-165">稍後設定 Git 時需要此密碼。</span><span class="sxs-lookup"><span data-stu-id="b3192-165">You'll need this password later when setting up Git.</span></span>

   * <span data-ttu-id="b3192-166">按一下 [儲存] 。</span><span class="sxs-lookup"><span data-stu-id="b3192-166">Click **Save**.</span></span>

7. <span data-ttu-id="b3192-167">在 [Web 應用程式] 刀鋒視窗中，按一下 [設定] > [屬性]。</span><span class="sxs-lookup"><span data-stu-id="b3192-167">In the **Web App** blade, click **Settings** > **Properties**.</span></span> <span data-ttu-id="b3192-168">[GIT URL] 下方顯示的遠端 Git 存放庫 URL，即為您的部署目的地。</span><span class="sxs-lookup"><span data-stu-id="b3192-168">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>

8. <span data-ttu-id="b3192-169">複製 **GIT URL** 值，以便於教學課程稍後使用。</span><span class="sxs-lookup"><span data-stu-id="b3192-169">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure 入口網站：應用程式的 [屬性] 刀鋒視窗](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="b3192-171">將 Web 應用程式發行至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b3192-171">Publish your web app to Azure App Service</span></span>

<span data-ttu-id="b3192-172">在本節中，您會使用 Visual Studio 建立本機 Git 存放庫，並將 Web 應用程式從該存放庫推送到 Azure 以進行部署。</span><span class="sxs-lookup"><span data-stu-id="b3192-172">In this section, you will create a local Git repository using Visual Studio and push from that repository to Azure to deploy your web app.</span></span> <span data-ttu-id="b3192-173">其中包括下列步驟：</span><span class="sxs-lookup"><span data-stu-id="b3192-173">The steps involved include the following:</span></span>

   * <span data-ttu-id="b3192-174">使用 GIT URL 值來新增遠端存放庫設定，以將本機存放庫部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="b3192-174">Add the remote repository setting using your GIT URL value, so you can deploy your local repository to Azure.</span></span>

   * <span data-ttu-id="b3192-175">認可專案的變更。</span><span class="sxs-lookup"><span data-stu-id="b3192-175">Commit your project changes.</span></span>

   * <span data-ttu-id="b3192-176">將專案變更從本機存放庫推送到 Azure 上的遠端存放庫。</span><span class="sxs-lookup"><span data-stu-id="b3192-176">Push your project changes from your local repository to your remote repository on Azure.</span></span>

&nbsp;
   
1.  <span data-ttu-id="b3192-177">在方案總管中，以滑鼠右鍵按一下 [方案 'SampleWebAppDemo']，然後選取 [認可]。</span><span class="sxs-lookup"><span data-stu-id="b3192-177">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="b3192-178">**Team Explorer** 隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="b3192-178">The **Team Explorer** will be displayed.</span></span>

    ![Team Explorer 的 [連線] 索引標籤](azure-continuous-deployment/_static/10-team-explorer.png)

2.  <span data-ttu-id="b3192-180">在 **Team Explorer** 中，選取 [首頁] (首頁圖示) > [設定] > [存放庫設定]。</span><span class="sxs-lookup"><span data-stu-id="b3192-180">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

3.  <span data-ttu-id="b3192-181">在 [存放庫設定] 的 [遠端] 區段中，選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="b3192-181">In the **Remotes** section of the **Repository Settings** select **Add**.</span></span> <span data-ttu-id="b3192-182">隨即顯示 [新增遠端] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="b3192-182">The **Add Remote** dialog box will be displayed.</span></span>

4.  <span data-ttu-id="b3192-183">將遠端的 [名稱] 設為 **Azure-SampleApp**。</span><span class="sxs-lookup"><span data-stu-id="b3192-183">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

5.  <span data-ttu-id="b3192-184">將 [擷取] 的值設為您在本教學課程稍早時從 Azure 複製的 **Git URL**。</span><span class="sxs-lookup"><span data-stu-id="b3192-184">Set the value for **Fetch** to the **Git URL** that you copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="b3192-185">請注意，這應該是結尾為 **.git** 的 URL。</span><span class="sxs-lookup"><span data-stu-id="b3192-185">Note that this is the URL that ends with **.git**.</span></span>

    ![[編輯遠端] 對話方塊](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    ><span data-ttu-id="b3192-187">或者，您可以開啟**命令視窗**、變更為您的專案目錄，並輸入命令，以透過**命令視窗**指定遠端存放庫。</span><span class="sxs-lookup"><span data-stu-id="b3192-187">As an alternative, you can specify the remote repository from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the command.</span></span> <span data-ttu-id="b3192-188">例如：`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span><span class="sxs-lookup"><span data-stu-id="b3192-188">For example:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span></span>

6.  <span data-ttu-id="b3192-189">選取 [首頁] (首頁圖示) > [設定] > [全域設定]。</span><span class="sxs-lookup"><span data-stu-id="b3192-189">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="b3192-190">確認您已設定好名稱和電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="b3192-190">Make sure you have your name and your email address set.</span></span> <span data-ttu-id="b3192-191">您可能也需要選取 [更新]。</span><span class="sxs-lookup"><span data-stu-id="b3192-191">You may also need to select **Update**.</span></span>

7.  <span data-ttu-id="b3192-192">選取 [首頁] > [變更]，即可返回 [變更] 檢視。</span><span class="sxs-lookup"><span data-stu-id="b3192-192">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

8.  <span data-ttu-id="b3192-193">輸入認可訊息，例如 **Initial Push #1**，然後按一下 [認可]。</span><span class="sxs-lookup"><span data-stu-id="b3192-193">Enter a commit message, such as **Initial Push #1** and click **Commit**.</span></span> <span data-ttu-id="b3192-194">此動作會在本機建立一項「認可」作業。</span><span class="sxs-lookup"><span data-stu-id="b3192-194">This action will create a *commit* locally.</span></span> <span data-ttu-id="b3192-195">接下來，您必須與 Azure 進行同步。</span><span class="sxs-lookup"><span data-stu-id="b3192-195">Next, you need to *sync* with Azure.</span></span>

    ![Team Explorer 的 [連線] 索引標籤](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    ><span data-ttu-id="b3192-197">或者，您可以開啟**命令視窗**、變更為您的專案目錄，並輸入 git 命令，以透過**命令視窗**認可變更。</span><span class="sxs-lookup"><span data-stu-id="b3192-197">As an alternative, you can commit your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the git commands.</span></span> <span data-ttu-id="b3192-198">例如: </span><span class="sxs-lookup"><span data-stu-id="b3192-198">For example:</span></span>
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  <span data-ttu-id="b3192-199">選取 [首頁] > [同步] > [動作] > [開啟命令提示字元]。</span><span class="sxs-lookup"><span data-stu-id="b3192-199">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="b3192-200">命令提示字元即會開啟您的專案目錄。</span><span class="sxs-lookup"><span data-stu-id="b3192-200">The command prompt will open to your project directory.</span></span>

10.  <span data-ttu-id="b3192-201">在命令視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="b3192-201">Enter the following command in the command window:</span></span>

    `git push -u Azure-SampleApp master`

11.  <span data-ttu-id="b3192-202">輸入您稍早在 Azure 中建立的 Azure **部署認證**密碼。</span><span class="sxs-lookup"><span data-stu-id="b3192-202">Enter your Azure **deployment credentials** password that you created earlier in Azure.</span></span>

    >[!NOTE]
    ><span data-ttu-id="b3192-203">輸入密碼時，系統不會將密碼顯示出來。</span><span class="sxs-lookup"><span data-stu-id="b3192-203">Your password will not be visible as you enter it.</span></span>

    <span data-ttu-id="b3192-204">此命令會開始進行將本機專案檔推送到 Azure 的程序。</span><span class="sxs-lookup"><span data-stu-id="b3192-204">This command will start the process of pushing your local project files to Azure.</span></span> <span data-ttu-id="b3192-205">上述命令的輸出結尾會顯示部署成功的訊息。</span><span class="sxs-lookup"><span data-stu-id="b3192-205">The output from the above command ends with a message that deployment was successful.</span></span>
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > <span data-ttu-id="b3192-206">如果您需要在專案上共同作業，應該考慮先將其推送到 [GitHub](https://github.com)，之後再推送至 Azure。</span><span class="sxs-lookup"><span data-stu-id="b3192-206">If you need to collaborate on a project, you should consider pushing to [GitHub](https://github.com) in between pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="b3192-207">驗證作用中的部署</span><span class="sxs-lookup"><span data-stu-id="b3192-207">Verify the Active Deployment</span></span>

<span data-ttu-id="b3192-208">您可以驗證是否已將 Web 應用程式從本機環境成功傳送至 Azure。</span><span class="sxs-lookup"><span data-stu-id="b3192-208">You can verify that you successfully transferred the web app from your local environment to Azure.</span></span> <span data-ttu-id="b3192-209">您可以查看提列的成功部署。</span><span class="sxs-lookup"><span data-stu-id="b3192-209">You'll see the listed successful deployment.</span></span>

1. <span data-ttu-id="b3192-210">在 [Azure 入口網站](https://portal.azure.com)中，選取您的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3192-210">In the [Azure Portal](https://portal.azure.com), select your web app.</span></span> <span data-ttu-id="b3192-211">然後，選取 [部署] > [部署選項]。</span><span class="sxs-lookup"><span data-stu-id="b3192-211">Then, select **Deployment** > **Deployment options**.</span></span>

   ![Azure 入口網站：[設定] 刀鋒視窗：顯示成功部署的 [部署] 刀鋒視窗](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="b3192-213">在 Azure 中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b3192-213">Run the app in Azure</span></span>

<span data-ttu-id="b3192-214">現在，您已將 Web 應用程式部署至 Azure，即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3192-214">Now that you have deployed your web app to Azure, you can run the app.</span></span>

<span data-ttu-id="b3192-215">有兩種方法可以達到這個目的：</span><span class="sxs-lookup"><span data-stu-id="b3192-215">This can be done in two ways:</span></span>

* <span data-ttu-id="b3192-216">在 Azure 入口網站中，找出您 Web 應用程式的 Web 應用程式刀鋒視窗，並按一下 [瀏覽]，即可在預設瀏覽器中檢視您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3192-216">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app in your default browser.</span></span>

* <span data-ttu-id="b3192-217">開啟瀏覽器，並輸入您的 Web 應用程式 URL。</span><span class="sxs-lookup"><span data-stu-id="b3192-217">Open a browser and enter the URL for your web app.</span></span> <span data-ttu-id="b3192-218">例如: </span><span class="sxs-lookup"><span data-stu-id="b3192-218">For example:</span></span>

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a><span data-ttu-id="b3192-219">更新 Web 應用程式，並重新發行</span><span class="sxs-lookup"><span data-stu-id="b3192-219">Update your web app and republish</span></span>

<span data-ttu-id="b3192-220">您可以在變更本機程式碼之後，重新發行。</span><span class="sxs-lookup"><span data-stu-id="b3192-220">After you make changes to your local code, you can republish.</span></span>

1.  <span data-ttu-id="b3192-221">在 Visual Studio 的方案總管中，開啟 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b3192-221">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

2.  <span data-ttu-id="b3192-222">在 `Configure` 方法中，修改 `Response.WriteAsync` 方法，使其如下所示：</span><span class="sxs-lookup"><span data-stu-id="b3192-222">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  <span data-ttu-id="b3192-223">將變更儲存到 *Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="b3192-223">Save changes to *Startup.cs*.</span></span>

4.  <span data-ttu-id="b3192-224">在方案總管中，以滑鼠右鍵按一下 [方案 'SampleWebAppDemo']，然後選取 [認可]。</span><span class="sxs-lookup"><span data-stu-id="b3192-224">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="b3192-225">**Team Explorer** 隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="b3192-225">The **Team Explorer** will be displayed.</span></span>

5.  <span data-ttu-id="b3192-226">輸入認可訊息，例如：</span><span class="sxs-lookup"><span data-stu-id="b3192-226">Enter a commit message, such as:</span></span>

    ```none
    Update #2
    ```

6.  <span data-ttu-id="b3192-227">按 [認可] 按鈕，認可專案中的變更。</span><span class="sxs-lookup"><span data-stu-id="b3192-227">Press the **Commit** button to commit the project changes.</span></span>

7.  <span data-ttu-id="b3192-228">選取 [首頁] > [同步] > [動作] > [推送]。</span><span class="sxs-lookup"><span data-stu-id="b3192-228">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

>[!NOTE]
><span data-ttu-id="b3192-229">或者，您可以開啟**命令視窗**、變更為您的專案目錄，並輸入 git 命令，以透過**命令視窗**推送變更。</span><span class="sxs-lookup"><span data-stu-id="b3192-229">As an alternative, you can push your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering a git command.</span></span> <span data-ttu-id="b3192-230">例如: </span><span class="sxs-lookup"><span data-stu-id="b3192-230">For example:</span></span>
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="b3192-231">在 Azure 中檢視更新的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b3192-231">View the updated web app in Azure</span></span>

<span data-ttu-id="b3192-232">您可以從 Azure 入口網站的 Web 應用程式刀鋒視窗選取 [瀏覽]，或開啟瀏覽器，然後輸入 Web 應用程式的 URL，以檢視更新的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3192-232">View your updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for your web app.</span></span> <span data-ttu-id="b3192-233">例如: </span><span class="sxs-lookup"><span data-stu-id="b3192-233">For example:</span></span>

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a><span data-ttu-id="b3192-234">其他資源</span><span class="sxs-lookup"><span data-stu-id="b3192-234">Additional Resources</span></span>

* [<span data-ttu-id="b3192-235">發行和部署</span><span class="sxs-lookup"><span data-stu-id="b3192-235">Publishing and Deployment</span></span>](index.md)

* [<span data-ttu-id="b3192-236">專案 Kudu</span><span class="sxs-lookup"><span data-stu-id="b3192-236">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
