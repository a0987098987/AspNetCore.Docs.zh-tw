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
ms.openlocfilehash: a9efad38b1c75bd3a186b4ec85861357ecf744b9
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="de7e2-104">使用 Visual Studio 與 Git 持續部署到適用於 ASP.NET Core 的 Azure</span><span class="sxs-lookup"><span data-stu-id="de7e2-104">Continuous deployment to Azure for ASP.NET Core, with Visual Studio and Git</span></span>

<span data-ttu-id="de7e2-105">作者：[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="de7e2-105">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="de7e2-106">本教學課程說明如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過連續部署將它從 Visual Studio 部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="de7e2-106">This tutorial shows you how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span> 

<span data-ttu-id="de7e2-107">另請參閱 [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic) (使用 VSTS 來建立 Azure Web 應用程式，並透過連續部署來發行)，其中說明如何使用 Visual Studio Team Services 來設定 [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) 的持續傳遞 (CD) 工作流程。</span><span class="sxs-lookup"><span data-stu-id="de7e2-107">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) using Visual Studio Team Services.</span></span> <span data-ttu-id="de7e2-108">Team Services 中的 Azure 持續傳遞功能，可簡化將應用程式更新發行到 Azure App Service 的部署管道設定。</span><span class="sxs-lookup"><span data-stu-id="de7e2-108">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for your app to Azure App Service.</span></span> <span data-ttu-id="de7e2-109">您可以透過 Azure 入口網站設定這個管道，以建置、執行測試、部署到預備位置，然後再部署到生產環境。</span><span class="sxs-lookup"><span data-stu-id="de7e2-109">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot,  and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="de7e2-110">若要完成本教學課程，您需要 Microsoft Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="de7e2-110">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="de7e2-111">如果您沒有帳戶，可以[啟動 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)或[註冊免費試用帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="de7e2-111">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de7e2-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="de7e2-112">Prerequisites</span></span>

<span data-ttu-id="de7e2-113">本教學課程假設您已安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="de7e2-113">This tutorial assumes you have already installed the following:</span></span>

* [<span data-ttu-id="de7e2-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de7e2-114">Visual Studio</span></span>](https://www.visualstudio.com)

* <span data-ttu-id="de7e2-115">[ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (執行階段和工具)</span><span class="sxs-lookup"><span data-stu-id="de7e2-115">[ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (runtime and tooling)</span></span>

* <span data-ttu-id="de7e2-116">[Git](https://git-scm.com/downloads) (適用於 Windows)</span><span class="sxs-lookup"><span data-stu-id="de7e2-116">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="de7e2-117">建立 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="de7e2-117">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="de7e2-118">啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="de7e2-118">Start Visual Studio.</span></span>

2. <span data-ttu-id="de7e2-119">從 [檔案] 功能表選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-119">From the **File** menu, select **New** > **Project**.</span></span>

3. <span data-ttu-id="de7e2-120">選取 [ASP.NET Web 應用程式] 專案範本。</span><span class="sxs-lookup"><span data-stu-id="de7e2-120">Select the **ASP.NET Web Application** project template.</span></span> <span data-ttu-id="de7e2-121">它會出現在 [已安裝] > [範本] > [Visual C#] > [Web] 下方。</span><span class="sxs-lookup"><span data-stu-id="de7e2-121">It appears under **Installed** > **Templates** > **Visual C#** > **Web**.</span></span> <span data-ttu-id="de7e2-122">將專案命名為 `SampleWebAppDemo`。</span><span class="sxs-lookup"><span data-stu-id="de7e2-122">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="de7e2-123">選取 [建立新的 Git 存放庫] 選項，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-123">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![[新增專案] 對話](azure-continuous-deployment/_static/01-new-project.png)

4. <span data-ttu-id="de7e2-125">在 [新增 ASP.NET 專案] 對話方塊中，選取 ASP.NET Core 的 [空白] 範本，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-125">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![[新增 ASP.NET 專案] 對話方塊](azure-continuous-deployment/_static/02-web-site-template.png)


### <a name="running-the-web-app-locally"></a><span data-ttu-id="de7e2-127">在本機執行 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="de7e2-127">Running the web app locally</span></span>

1. <span data-ttu-id="de7e2-128">一旦 Visual Studio 完成建立應用程式之後，您即可選取 [偵錯] -> [開始偵錯] 以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="de7e2-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** -> **Start Debugging**.</span></span> <span data-ttu-id="de7e2-129">或者，也可以按 **F5**。</span><span class="sxs-lookup"><span data-stu-id="de7e2-129">As an alternative, you can press **F5**.</span></span>

   <span data-ttu-id="de7e2-130">系統可能需要一點時間來初始化 Visual Studio 和新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="de7e2-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="de7e2-131">完成後，瀏覽器即會顯示執行中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="de7e2-131">Once it is complete, the browser will show the running app.</span></span>

   ![顯示執行中應用程式的瀏覽器視窗，該應用程式顯示 'Hello World!'](azure-continuous-deployment/_static/04-browser-runapp.png)

2. <span data-ttu-id="de7e2-133">在您檢閱執行中的 Web 應用程式之後，請關閉瀏覽器，然後按一下 Visual Studio 工具列中的「停止偵錯」圖示以停止應用程式。</span><span class="sxs-lookup"><span data-stu-id="de7e2-133">After reviewing the running Web app, close the browser and click the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="de7e2-134">在 Azure 入口網站中建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="de7e2-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="de7e2-135">下列步驟將引導您在 Azure 入口網站中建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="de7e2-135">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="de7e2-136">登入 [Azure 入口網站](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="de7e2-136">Log in to the [Azure Portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="de7e2-137">點選入口網站左上角的 [新增]</span><span class="sxs-lookup"><span data-stu-id="de7e2-137">TAP **NEW** at the top left of the Portal</span></span>
3. <span data-ttu-id="de7e2-138">點選 [Web + 行動] > [Web 應用程式]</span><span class="sxs-lookup"><span data-stu-id="de7e2-138">TAP **Web + Mobile** > **Web App**</span></span>

    ![Microsoft Azure 入口網站：新按鈕：Marketplace 下方的 [Web + 行動]：[精選 App] 下方的 [Web 應用程式] 按鈕](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  <span data-ttu-id="de7e2-140">在 [Web 應用程式] 刀鋒視窗中，為 [App Service 名稱] 輸入唯一值。</span><span class="sxs-lookup"><span data-stu-id="de7e2-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

    ![[Web 應用程式] 刀鋒視窗](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    ><span data-ttu-id="de7e2-142">[App Service 名稱] 必須是唯一的名稱。</span><span class="sxs-lookup"><span data-stu-id="de7e2-142">The **App Service Name** name needs to be unique.</span></span> <span data-ttu-id="de7e2-143">當您嘗試輸入名稱時，入口網站會強制執行這項規則。</span><span class="sxs-lookup"><span data-stu-id="de7e2-143">The portal will enforce this rule when you attempt to enter the name.</span></span> <span data-ttu-id="de7e2-144">輸入不同的值之後，您必須將本教學課程中看到的每個 **SampleWebAppDemo** 取代為該值。</span><span class="sxs-lookup"><span data-stu-id="de7e2-144">After you enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span>

    &nbsp;
    
    <span data-ttu-id="de7e2-145">此外，請在 [Web 應用程式] 刀鋒視窗中，選取現有的 [App Service 方案/位置] 或另外新建一個。</span><span class="sxs-lookup"><span data-stu-id="de7e2-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="de7e2-146">如果您建立新的方案，請選取定價層、位置和其他選項。</span><span class="sxs-lookup"><span data-stu-id="de7e2-146">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="de7e2-147">如需 App Service 方案的詳細資訊，請參閱 [Azure App Service 方案深入概觀](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/)。</span><span class="sxs-lookup"><span data-stu-id="de7e2-147">For more information on App Service plans, [Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span></span>

5.  <span data-ttu-id="de7e2-148">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="de7e2-148">Click **Create**.</span></span> <span data-ttu-id="de7e2-149">Azure 即會佈建並啟動您的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="de7e2-149">Azure will provision and start your web app.</span></span>

    ![Azure 入口網站：範例 Web 應用程式示範 01 [基本資訊] 刀鋒視窗](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="de7e2-151">啟用新 Web 應用程式的 Git 發行功能</span><span class="sxs-lookup"><span data-stu-id="de7e2-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="de7e2-152">Git 是一種分散式版本控制系統，可用來部署您的 Azure App Service Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="de7e2-152">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="de7e2-153">您可將針對 Web 應用程式所撰寫的程式碼儲存在本機 Git 存放庫，並將這些程式碼推送至遠端存放庫以部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="de7e2-153">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="de7e2-154">登入 [Azure 入口網站](https://portal.azure.com) (如果您尚未登入)。</span><span class="sxs-lookup"><span data-stu-id="de7e2-154">Log into the [Azure Portal](https://portal.azure.com), if you're not already logged in.</span></span>

2. <span data-ttu-id="de7e2-155">在瀏覽窗格的底部，按一下 [瀏覽]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-155">Click **Browse**, located at the bottom of the navigation pane.</span></span>

3. <span data-ttu-id="de7e2-156">按一下 [Web 應用程式]，即可檢視與 Azure 訂用帳戶建立關聯的 Web 應用程式清單。</span><span class="sxs-lookup"><span data-stu-id="de7e2-156">Click **Web Apps** to view a list of the web apps associated with your Azure subscription.</span></span>

4. <span data-ttu-id="de7e2-157">選取您在本教學課程上一節所建立的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="de7e2-157">Select the web app you created in the previous section of this tutorial.</span></span>

5. <span data-ttu-id="de7e2-158">如果未顯示 [設定] 刀鋒視窗，請選取 [Web 應用程式] 刀鋒視窗中的 [設定]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-158">If the **Settings** blade is not shown, select **Settings** in the **Web App** blade.</span></span>

6. <span data-ttu-id="de7e2-159">在 [設定] 刀鋒視窗中，選取 [部署來源] > [選擇來源] > [本機 Git 存放庫]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-159">In the **Settings** blade, select **Deployment source** > **Choose Source** > **Local Git Repository**.</span></span>

   ![[設定] 刀鋒視窗：[部署來源] 刀鋒視窗：[選擇來源] 刀鋒視窗](azure-continuous-deployment/_static/08-azure-localrepository.png)

7. <span data-ttu-id="de7e2-161">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-161">Click **OK**.</span></span>

8. <span data-ttu-id="de7e2-162">如果您先前尚未設定用來發行 Web 應用程式或其他 App Service 應用程式的部署認證，請立即設定：</span><span class="sxs-lookup"><span data-stu-id="de7e2-162">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>

   * <span data-ttu-id="de7e2-163">按一下 [設定] > [部署認證]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-163">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="de7e2-164">[設定部署認證] 刀鋒視窗隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="de7e2-164">The **Set deployment credentials** blade will be displayed.</span></span>

   * <span data-ttu-id="de7e2-165">建立使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="de7e2-165">Create a user name and password.</span></span>  <span data-ttu-id="de7e2-166">稍後設定 Git 時需要此密碼。</span><span class="sxs-lookup"><span data-stu-id="de7e2-166">You'll need this password later when setting up Git.</span></span>

   * <span data-ttu-id="de7e2-167">按一下 [儲存] 。</span><span class="sxs-lookup"><span data-stu-id="de7e2-167">Click **Save**.</span></span>

9. <span data-ttu-id="de7e2-168">在 [Web 應用程式] 刀鋒視窗中，按一下 [設定] > [屬性]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-168">In the **Web App** blade, click **Settings** > **Properties**.</span></span> <span data-ttu-id="de7e2-169">[GIT URL] 下方顯示的遠端 Git 存放庫 URL，即為您的部署目的地。</span><span class="sxs-lookup"><span data-stu-id="de7e2-169">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>

10. <span data-ttu-id="de7e2-170">複製 **GIT URL** 值，以便於教學課程稍後使用。</span><span class="sxs-lookup"><span data-stu-id="de7e2-170">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure 入口網站：應用程式的 [屬性] 刀鋒視窗](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="de7e2-172">將 Web 應用程式發行至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="de7e2-172">Publish your web app to Azure App Service</span></span>

<span data-ttu-id="de7e2-173">在本節中，您會使用 Visual Studio 建立本機 Git 存放庫，並將 Web 應用程式從該存放庫推送到 Azure 以進行部署。</span><span class="sxs-lookup"><span data-stu-id="de7e2-173">In this section, you will create a local Git repository using Visual Studio and push from that repository to Azure to deploy your web app.</span></span> <span data-ttu-id="de7e2-174">其中包括下列步驟：</span><span class="sxs-lookup"><span data-stu-id="de7e2-174">The steps involved include the following:</span></span>

   * <span data-ttu-id="de7e2-175">使用 GIT URL 值來新增遠端存放庫設定，以將本機存放庫部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="de7e2-175">Add the remote repository setting using your GIT URL value, so you can deploy your local repository to Azure.</span></span>

   * <span data-ttu-id="de7e2-176">認可專案的變更。</span><span class="sxs-lookup"><span data-stu-id="de7e2-176">Commit your project changes.</span></span>

   * <span data-ttu-id="de7e2-177">將專案變更從本機存放庫推送到 Azure 上的遠端存放庫。</span><span class="sxs-lookup"><span data-stu-id="de7e2-177">Push your project changes from your local repository to your remote repository on Azure.</span></span>

&nbsp;
   
1.  <span data-ttu-id="de7e2-178">在方案總管中，以滑鼠右鍵按一下 [方案 'SampleWebAppDemo']，然後選取 [認可]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-178">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="de7e2-179">**Team Explorer** 隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="de7e2-179">The **Team Explorer** will be displayed.</span></span>

    ![Team Explorer 的 [連線] 索引標籤](azure-continuous-deployment/_static/10-team-explorer.png)

2.  <span data-ttu-id="de7e2-181">在 **Team Explorer** 中，選取 [首頁] \(首頁圖示) > [設定] > [存放庫設定]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-181">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

3.  <span data-ttu-id="de7e2-182">在 [存放庫設定] 的 [遠端] 區段中，選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-182">In the **Remotes** section of the **Repository Settings** select **Add**.</span></span> <span data-ttu-id="de7e2-183">隨即顯示 [新增遠端] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="de7e2-183">The **Add Remote** dialog box will be displayed.</span></span>

4.  <span data-ttu-id="de7e2-184">將遠端的 [名稱] 設為 **Azure-SampleApp**。</span><span class="sxs-lookup"><span data-stu-id="de7e2-184">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

5.  <span data-ttu-id="de7e2-185">將 [擷取] 的值設為您在本教學課程稍早時從 Azure 複製的 **Git URL**。</span><span class="sxs-lookup"><span data-stu-id="de7e2-185">Set the value for **Fetch** to the **Git URL** that you copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="de7e2-186">請注意，這應該是結尾為 **.git** 的 URL。</span><span class="sxs-lookup"><span data-stu-id="de7e2-186">Note that this is the URL that ends with **.git**.</span></span>

    ![[編輯遠端] 對話方塊](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    ><span data-ttu-id="de7e2-188">或者，您可以開啟**命令視窗**、變更為您的專案目錄，並輸入命令，以透過**命令視窗**指定遠端存放庫。</span><span class="sxs-lookup"><span data-stu-id="de7e2-188">As an alternative, you can specify the remote repository from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the command.</span></span> <span data-ttu-id="de7e2-189">例如：`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span><span class="sxs-lookup"><span data-stu-id="de7e2-189">For example:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span></span>

6.  <span data-ttu-id="de7e2-190">選取 [首頁] \(首頁圖示) > [設定] > [全域設定]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-190">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="de7e2-191">確認您已設定好名稱和電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="de7e2-191">Make sure you have your name and your email address set.</span></span> <span data-ttu-id="de7e2-192">您可能也需要選取 [更新]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-192">You may also need to select **Update**.</span></span>

7.  <span data-ttu-id="de7e2-193">選取 [首頁] > [變更]，即可返回 [變更] 檢視。</span><span class="sxs-lookup"><span data-stu-id="de7e2-193">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

8.  <span data-ttu-id="de7e2-194">輸入認可訊息，例如 **Initial Push #1**，然後按一下 [認可]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-194">Enter a commit message, such as **Initial Push #1** and click **Commit**.</span></span> <span data-ttu-id="de7e2-195">此動作會在本機建立一項「認可」作業。</span><span class="sxs-lookup"><span data-stu-id="de7e2-195">This action will create a *commit* locally.</span></span> <span data-ttu-id="de7e2-196">接下來，您必須與 Azure 進行同步。</span><span class="sxs-lookup"><span data-stu-id="de7e2-196">Next, you need to *sync* with Azure.</span></span>

    ![Team Explorer 的 [連線] 索引標籤](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    ><span data-ttu-id="de7e2-198">或者，您可以開啟**命令視窗**、變更為您的專案目錄，並輸入 git 命令，以透過**命令視窗**認可變更。</span><span class="sxs-lookup"><span data-stu-id="de7e2-198">As an alternative, you can commit your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the git commands.</span></span> <span data-ttu-id="de7e2-199">例如: </span><span class="sxs-lookup"><span data-stu-id="de7e2-199">For example:</span></span>
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  <span data-ttu-id="de7e2-200">選取 [首頁] > [同步] > [動作] > [開啟命令提示字元]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-200">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="de7e2-201">命令提示字元即會開啟您的專案目錄。</span><span class="sxs-lookup"><span data-stu-id="de7e2-201">The command prompt will open to your project directory.</span></span>

10.  <span data-ttu-id="de7e2-202">在命令視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="de7e2-202">Enter the following command in the command window:</span></span>

    `git push -u Azure-SampleApp master`

11.  <span data-ttu-id="de7e2-203">輸入您稍早在 Azure 中建立的 Azure **部署認證**密碼。</span><span class="sxs-lookup"><span data-stu-id="de7e2-203">Enter your Azure **deployment credentials** password that you created earlier in Azure.</span></span>

    >[!NOTE]
    ><span data-ttu-id="de7e2-204">輸入密碼時，系統不會將密碼顯示出來。</span><span class="sxs-lookup"><span data-stu-id="de7e2-204">Your password will not be visible as you enter it.</span></span>

    <span data-ttu-id="de7e2-205">此命令會開始進行將本機專案檔推送到 Azure 的程序。</span><span class="sxs-lookup"><span data-stu-id="de7e2-205">This command will start the process of pushing your local project files to Azure.</span></span> <span data-ttu-id="de7e2-206">上述命令的輸出結尾會顯示部署成功的訊息。</span><span class="sxs-lookup"><span data-stu-id="de7e2-206">The output from the above command ends with a message that deployment was successful.</span></span>
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > <span data-ttu-id="de7e2-207">如果您需要在專案上共同作業，應該考慮先將其推送到 [GitHub](https://github.com)，之後再推送至 Azure。</span><span class="sxs-lookup"><span data-stu-id="de7e2-207">If you need to collaborate on a project, you should consider pushing to [GitHub](https://github.com) in between pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="de7e2-208">驗證作用中的部署</span><span class="sxs-lookup"><span data-stu-id="de7e2-208">Verify the Active Deployment</span></span>

<span data-ttu-id="de7e2-209">您可以驗證是否已將 Web 應用程式從本機環境成功傳送至 Azure。</span><span class="sxs-lookup"><span data-stu-id="de7e2-209">You can verify that you successfully transferred the web app from your local environment to Azure.</span></span> <span data-ttu-id="de7e2-210">您可以查看提列的成功部署。</span><span class="sxs-lookup"><span data-stu-id="de7e2-210">You'll see the listed successful deployment.</span></span>

1. <span data-ttu-id="de7e2-211">在 [Azure 入口網站](https://portal.azure.com)中，選取您的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="de7e2-211">In the [Azure Portal](https://portal.azure.com), select your web app.</span></span> <span data-ttu-id="de7e2-212">然後，選取 [設定] > [持續部署]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-212">Then, select **Settings** > **Continuous deployment**.</span></span>

   ![Azure 入口網站：[設定] 刀鋒視窗：顯示成功部署的 [部署] 刀鋒視窗](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="de7e2-214">在 Azure 中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="de7e2-214">Run the app in Azure</span></span>

<span data-ttu-id="de7e2-215">現在，您已將 Web 應用程式部署至 Azure，即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="de7e2-215">Now that you have deployed your web app to Azure, you can run the app.</span></span>

<span data-ttu-id="de7e2-216">有兩種方法可以達到這個目的：</span><span class="sxs-lookup"><span data-stu-id="de7e2-216">This can be done in two ways:</span></span>

* <span data-ttu-id="de7e2-217">在 Azure 入口網站中，找出您 Web 應用程式的 Web 應用程式刀鋒視窗，並按一下 [瀏覽]，即可在預設瀏覽器中檢視您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="de7e2-217">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app in your default browser.</span></span>

* <span data-ttu-id="de7e2-218">開啟瀏覽器，並輸入您的 Web 應用程式 URL。</span><span class="sxs-lookup"><span data-stu-id="de7e2-218">Open a browser and enter the URL for your web app.</span></span> <span data-ttu-id="de7e2-219">例如: </span><span class="sxs-lookup"><span data-stu-id="de7e2-219">For example:</span></span>

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a><span data-ttu-id="de7e2-220">更新 Web 應用程式，並重新發行</span><span class="sxs-lookup"><span data-stu-id="de7e2-220">Update your web app and republish</span></span>

<span data-ttu-id="de7e2-221">您可以在變更本機程式碼之後，重新發行。</span><span class="sxs-lookup"><span data-stu-id="de7e2-221">After you make changes to your local code, you can republish.</span></span>

1.  <span data-ttu-id="de7e2-222">在 Visual Studio 的方案總管中，開啟 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="de7e2-222">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

2.  <span data-ttu-id="de7e2-223">在 `Configure` 方法中，修改 `Response.WriteAsync` 方法，使其如下所示：</span><span class="sxs-lookup"><span data-stu-id="de7e2-223">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  <span data-ttu-id="de7e2-224">將變更儲存到 *Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="de7e2-224">Save changes to *Startup.cs*.</span></span>

4.  <span data-ttu-id="de7e2-225">在方案總管中，以滑鼠右鍵按一下 [方案 'SampleWebAppDemo']，然後選取 [認可]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-225">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="de7e2-226">**Team Explorer** 隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="de7e2-226">The **Team Explorer** will be displayed.</span></span>

5.  <span data-ttu-id="de7e2-227">輸入認可訊息，例如：</span><span class="sxs-lookup"><span data-stu-id="de7e2-227">Enter a commit message, such as:</span></span>

    ```none
    Update #2
    ```

6.  <span data-ttu-id="de7e2-228">按 [認可] 按鈕，認可專案中的變更。</span><span class="sxs-lookup"><span data-stu-id="de7e2-228">Press the **Commit** button to commit the project changes.</span></span>

7.  <span data-ttu-id="de7e2-229">選取 [首頁] > [同步] > [動作] > [推送]。</span><span class="sxs-lookup"><span data-stu-id="de7e2-229">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

>[!NOTE]
><span data-ttu-id="de7e2-230">或者，您可以開啟**命令視窗**、變更為您的專案目錄，並輸入 git 命令，以透過**命令視窗**推送變更。</span><span class="sxs-lookup"><span data-stu-id="de7e2-230">As an alternative, you can push your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering a git command.</span></span> <span data-ttu-id="de7e2-231">例如: </span><span class="sxs-lookup"><span data-stu-id="de7e2-231">For example:</span></span>
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="de7e2-232">在 Azure 中檢視更新的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="de7e2-232">View the updated web app in Azure</span></span>

<span data-ttu-id="de7e2-233">您可以從 Azure 入口網站的 Web 應用程式刀鋒視窗選取 [瀏覽]，或開啟瀏覽器，然後輸入 Web 應用程式的 URL，以檢視更新的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="de7e2-233">View your updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for your web app.</span></span> <span data-ttu-id="de7e2-234">例如: </span><span class="sxs-lookup"><span data-stu-id="de7e2-234">For example:</span></span>

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a><span data-ttu-id="de7e2-235">其他資源</span><span class="sxs-lookup"><span data-stu-id="de7e2-235">Additional Resources</span></span>

* [<span data-ttu-id="de7e2-236">發行和部署</span><span class="sxs-lookup"><span data-stu-id="de7e2-236">Publishing and Deployment</span></span>](index.md)

* [<span data-ttu-id="de7e2-237">專案 Kudu</span><span class="sxs-lookup"><span data-stu-id="de7e2-237">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
