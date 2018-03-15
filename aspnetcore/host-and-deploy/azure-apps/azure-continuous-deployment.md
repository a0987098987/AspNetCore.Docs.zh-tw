---
title: "使用 Visual Studio 在 Azure 和 ASP.NET Core 使用 Git 連續部署"
author: rick-anderson
description: "了解如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過 Git 持續部署將它部署到 Azure App Service。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 7302de1ace62dba53b317039aac7f4763314aa19
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="df430-103">使用 Visual Studio 在 Azure 和 ASP.NET Core 使用 Git 連續部署</span><span class="sxs-lookup"><span data-stu-id="df430-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="df430-104">作者：[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="df430-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="df430-105">本教學課程會示範如何建立 ASP.NET Core web 應用程式使用 Visual Studio，並將它從 Visual Studio 部署至 Azure App Service 使用連續部署。</span><span class="sxs-lookup"><span data-stu-id="df430-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="df430-106">另請參閱 [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic) (使用 VSTS 來建立 Azure Web 應用程式，並透過連續部署來發行)，其中說明如何使用 Visual Studio Team Services 來設定 [Azure App Service](/azure/app-service/app-service-web-overview) 的持續傳遞 (CD) 工作流程。</span><span class="sxs-lookup"><span data-stu-id="df430-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="df430-107">在 Team Services 中的 azure 持續傳遞可簡化強固的部署管線設定可發行的 Azure App Service 中裝載的應用程式的更新。</span><span class="sxs-lookup"><span data-stu-id="df430-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="df430-108">從 Azure 入口網站中建置、 執行測試、 將部署到預備位置，然後再部署到生產環境，可以設定管線。</span><span class="sxs-lookup"><span data-stu-id="df430-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="df430-109">若要完成本教學課程中，Microsoft Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="df430-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="df430-110">若要取得帳戶，[啟用 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F)或[申請免費的試用版](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="df430-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df430-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="df430-111">Prerequisites</span></span>

<span data-ttu-id="df430-112">本教學課程會假設已安裝下列軟體：</span><span class="sxs-lookup"><span data-stu-id="df430-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="df430-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="df430-113">Visual Studio</span></span>](https://www.visualstudio.com)
* <span data-ttu-id="df430-114">[.NET core SDK](https://www.microsoft.com/net/download/core) (執行階段和工具)</span><span class="sxs-lookup"><span data-stu-id="df430-114">[.NET Core SDK](https://www.microsoft.com/net/download/core) (runtime and tooling)</span></span>
* <span data-ttu-id="df430-115">[Git](https://git-scm.com/downloads) (適用於 Windows)</span><span class="sxs-lookup"><span data-stu-id="df430-115">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="df430-116">建立 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="df430-116">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="df430-117">啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="df430-117">Start Visual Studio.</span></span>

1. <span data-ttu-id="df430-118">從 [檔案] 功能表選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="df430-118">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="df430-119">選取 [ASP.NET Core Web 應用程式] 專案範本。</span><span class="sxs-lookup"><span data-stu-id="df430-119">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="df430-120">它會出現在 [已安裝] > [範本] > [Visual C#] > [.NET Core] 下。</span><span class="sxs-lookup"><span data-stu-id="df430-120">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="df430-121">將專案命名為 `SampleWebAppDemo`。</span><span class="sxs-lookup"><span data-stu-id="df430-121">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="df430-122">選取**建立新的 Git 儲存機制**選項，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="df430-122">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![[新增專案] 對話](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="df430-124">在 [新增 ASP.NET Core 專案] 對話方塊中，選取 ASP.NET Core 的 [空白] 範本，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="df430-124">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![[新增 ASP.NET 專案] 對話方塊](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="df430-126">.NET Core 的最新版本為 2.0。</span><span class="sxs-lookup"><span data-stu-id="df430-126">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="df430-127">在本機執行 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="df430-127">Running the web app locally</span></span>

1. <span data-ttu-id="df430-128">一旦 Visual Studio 完成建立應用程式之後，您即可選取 [偵錯] > [開始偵錯] 以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="df430-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="df430-129">或者，您可以按下**F5**。</span><span class="sxs-lookup"><span data-stu-id="df430-129">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="df430-130">系統可能需要一點時間來初始化 Visual Studio 和新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="df430-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="df430-131">一旦完成後，瀏覽器會顯示執行中應用程式。</span><span class="sxs-lookup"><span data-stu-id="df430-131">Once it's complete, the browser shows the running app.</span></span>

   ![顯示執行中應用程式的瀏覽器視窗，該應用程式顯示 'Hello World!'](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="df430-133">檢閱後執行的 Web 應用程式，請關閉瀏覽器，並選取 [停止偵錯] 圖示以停止應用程式的 Visual Studio 的工具列中。</span><span class="sxs-lookup"><span data-stu-id="df430-133">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="df430-134">在 Azure 入口網站中建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="df430-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="df430-135">下列步驟會在 Azure 入口網站中建立 web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="df430-135">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="df430-136">登入[Azure 入口網站](https://portal.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="df430-136">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="df430-137">選取**新增**在入口網站介面的左上角。</span><span class="sxs-lookup"><span data-stu-id="df430-137">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="df430-138">選取**Web + 行動** > **Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="df430-138">Select **Web + Mobile** > **Web App**.</span></span>

   ![Microsoft Azure 入口網站：新按鈕：Marketplace 下方的 [Web + 行動]：[精選 App] 下方的 [Web 應用程式] 按鈕](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="df430-140">在 [Web 應用程式] 刀鋒視窗中，為 [App Service 名稱] 輸入唯一值。</span><span class="sxs-lookup"><span data-stu-id="df430-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![[Web 應用程式] 刀鋒視窗](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="df430-142">**應用程式服務名稱**必須是唯一的名稱。</span><span class="sxs-lookup"><span data-stu-id="df430-142">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="df430-143">提供名稱時，入口網站會強制執行這項規則。</span><span class="sxs-lookup"><span data-stu-id="df430-143">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="df430-144">如果提供不同的值，以取代該值，每次發生**SampleWebAppDemo**在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="df430-144">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="df430-145">此外，請在 [Web 應用程式] 刀鋒視窗中，選取現有的 [App Service 方案/位置] 或另外新建一個。</span><span class="sxs-lookup"><span data-stu-id="df430-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="df430-146">如果建立新的計畫，請選取定價層、 位置和其他選項。</span><span class="sxs-lookup"><span data-stu-id="df430-146">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="df430-147">如需有關應用程式服務方案的詳細資訊，請參閱[Azure App Service 方案深入概觀](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)。</span><span class="sxs-lookup"><span data-stu-id="df430-147">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="df430-148">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="df430-148">Select **Create**.</span></span> <span data-ttu-id="df430-149">Azure 會佈建並啟動 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="df430-149">Azure will provision and start the web app.</span></span>

   ![Azure 入口網站：範例 Web 應用程式示範 01 [基本資訊] 刀鋒視窗](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="df430-151">啟用新 Web 應用程式的 Git 發行功能</span><span class="sxs-lookup"><span data-stu-id="df430-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="df430-152">Git 是分散式的版本控制系統可以用來部署 Azure App Service web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="df430-152">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="df430-153">Web 應用程式程式碼會儲存在本機 Git 儲存機制並推送至遠端儲存機制的程式碼部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="df430-153">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="df430-154">登入[Azure 入口網站](https://portal.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="df430-154">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="df430-155">選取**應用程式服務**，檢視 Azure 訂用帳戶相關聯的應用程式服務的清單。</span><span class="sxs-lookup"><span data-stu-id="df430-155">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="df430-156">選取在本教學課程的上一節中建立的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="df430-156">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="df430-157">在 [開發] 刀鋒視窗中，選取 [部署選項] > [選擇來源] > [本機 Git 存放庫]。</span><span class="sxs-lookup"><span data-stu-id="df430-157">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![[設定] 刀鋒視窗：[部署來源] 刀鋒視窗：[選擇來源] 刀鋒視窗](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="df430-159">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="df430-159">Select **OK**.</span></span>

1. <span data-ttu-id="df430-160">如果發佈的 web 應用程式或其他 App Service 應用程式的部署認證您尚未先前已設定，立即設定：</span><span class="sxs-lookup"><span data-stu-id="df430-160">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="df430-161">選取**設定** > **部署認證**。</span><span class="sxs-lookup"><span data-stu-id="df430-161">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="df430-162">**設定部署認證**刀鋒視窗會顯示。</span><span class="sxs-lookup"><span data-stu-id="df430-162">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="df430-163">建立使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="df430-163">Create a user name and password.</span></span> <span data-ttu-id="df430-164">設定 Git 時，儲存供稍後使用的密碼。</span><span class="sxs-lookup"><span data-stu-id="df430-164">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="df430-165">選取 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="df430-165">Select **Save**.</span></span>

1. <span data-ttu-id="df430-166">在**Web 應用程式**刀鋒視窗中，選取**設定** > **屬性**。</span><span class="sxs-lookup"><span data-stu-id="df430-166">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="df430-167">將部署至遠端 Git 儲存機制的 URL 會顯示在下方**GIT URL**。</span><span class="sxs-lookup"><span data-stu-id="df430-167">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="df430-168">複製 **GIT URL** 值，以便於教學課程稍後使用。</span><span class="sxs-lookup"><span data-stu-id="df430-168">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure 入口網站：應用程式的 [屬性] 刀鋒視窗](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="df430-170">發行至 Azure App Service web 應用程式</span><span class="sxs-lookup"><span data-stu-id="df430-170">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="df430-171">在本節中，建立使用 Visual Studio 和推播從該儲存機制至 Azure 部署 web 應用程式的本機 Git 儲存機制。</span><span class="sxs-lookup"><span data-stu-id="df430-171">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="df430-172">其中包括下列步驟：</span><span class="sxs-lookup"><span data-stu-id="df430-172">The steps involved include the following:</span></span>

* <span data-ttu-id="df430-173">新增使用 GIT URL 值，因此本機儲存機制可部署至 Azure 的遠端儲存機制設定。</span><span class="sxs-lookup"><span data-stu-id="df430-173">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="df430-174">認可變更專案。</span><span class="sxs-lookup"><span data-stu-id="df430-174">Commit project changes.</span></span>
* <span data-ttu-id="df430-175">在 Azure 上至遠端儲存機制將專案變更從本機儲存機制推送。</span><span class="sxs-lookup"><span data-stu-id="df430-175">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="df430-176">在方案總管中，以滑鼠右鍵按一下 [方案 'SampleWebAppDemo']，然後選取 [認可]。</span><span class="sxs-lookup"><span data-stu-id="df430-176">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="df430-177">**Team Explorer**隨即出現。</span><span class="sxs-lookup"><span data-stu-id="df430-177">The **Team Explorer** is displayed.</span></span>

   ![Team Explorer 的 [連線] 索引標籤](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="df430-179">在 **Team Explorer** 中，選取 [首頁] (首頁圖示) > [設定] > [存放庫設定]。</span><span class="sxs-lookup"><span data-stu-id="df430-179">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="df430-180">在**遙控器**區段**儲存機制設定**，選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="df430-180">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="df430-181">**加入遠端**對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="df430-181">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="df430-182">將遠端的 [名稱] 設為 **Azure-SampleApp**。</span><span class="sxs-lookup"><span data-stu-id="df430-182">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="df430-183">設定的值**提取**至**Git URL** ，從 Azure 複製稍早在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="df430-183">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="df430-184">請注意，這應該是結尾為 **.git** 的 URL。</span><span class="sxs-lookup"><span data-stu-id="df430-184">Note that this is the URL that ends with **.git**.</span></span>

   ![[編輯遠端] 對話方塊](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="df430-186">或者，指定遠端儲存機制從**命令視窗**開啟**命令視窗**，變更至專案目錄中，輸入命令。</span><span class="sxs-lookup"><span data-stu-id="df430-186">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="df430-187">範例：</span><span class="sxs-lookup"><span data-stu-id="df430-187">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="df430-188">選取 [首頁] (首頁圖示) > [設定] > [全域設定]。</span><span class="sxs-lookup"><span data-stu-id="df430-188">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="df430-189">確認已設定的名稱和電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="df430-189">Confirm that the name and email address are set.</span></span> <span data-ttu-id="df430-190">選取**更新**必要。</span><span class="sxs-lookup"><span data-stu-id="df430-190">Select **Update** if required.</span></span>

1. <span data-ttu-id="df430-191">選取 [首頁] > [變更]，即可返回 [變更] 檢視。</span><span class="sxs-lookup"><span data-stu-id="df430-191">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="df430-192">輸入 「 認可 」 訊息，例如**初始推送 #1**選取**認可**。</span><span class="sxs-lookup"><span data-stu-id="df430-192">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="df430-193">這個動作會建立*認可*在本機。</span><span class="sxs-lookup"><span data-stu-id="df430-193">This action creates a *commit* locally.</span></span>

   ![Team Explorer 的 [連線] 索引標籤](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="df430-195">或者，變更從認可**命令視窗**開啟**命令視窗**、 專案目錄中，變更，以及輸入 git 命令。</span><span class="sxs-lookup"><span data-stu-id="df430-195">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="df430-196">範例：</span><span class="sxs-lookup"><span data-stu-id="df430-196">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="df430-197">選取 [首頁] > [同步] > [動作] > [開啟命令提示字元]。</span><span class="sxs-lookup"><span data-stu-id="df430-197">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="df430-198">命令提示字元會開啟至專案目錄中。</span><span class="sxs-lookup"><span data-stu-id="df430-198">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="df430-199">在命令視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="df430-199">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="df430-200">輸入 Azure**部署認證**稍早在 Azure 中建立的密碼。</span><span class="sxs-lookup"><span data-stu-id="df430-200">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="df430-201">此命令會啟動本機專案檔案推送至 Azure 的程序。</span><span class="sxs-lookup"><span data-stu-id="df430-201">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="df430-202">上述命令的輸出，結束部署成功的訊息。</span><span class="sxs-lookup"><span data-stu-id="df430-202">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="df430-203">如果需要在專案上的共同作業，請考慮將推送到[GitHub](https://github.com)之前推入至 Azure。</span><span class="sxs-lookup"><span data-stu-id="df430-203">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="df430-204">驗證作用中的部署</span><span class="sxs-lookup"><span data-stu-id="df430-204">Verify the Active Deployment</span></span>

<span data-ttu-id="df430-205">請確認從本機環境到 Azure 的 web 應用程式傳送成功。</span><span class="sxs-lookup"><span data-stu-id="df430-205">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="df430-206">在[Azure 入口網站](https://portal.azure.com)、 選取 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="df430-206">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="df430-207">選取**部署** > **部署選項**。</span><span class="sxs-lookup"><span data-stu-id="df430-207">Select **Deployment** > **Deployment options**.</span></span>

![Azure 入口網站：[設定] 刀鋒視窗：顯示成功部署的 [部署] 刀鋒視窗](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="df430-209">在 Azure 中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="df430-209">Run the app in Azure</span></span>

<span data-ttu-id="df430-210">現在 web 應用程式部署至 Azure 時，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="df430-210">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="df430-211">即可達成這兩種方式：</span><span class="sxs-lookup"><span data-stu-id="df430-211">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="df430-212">在 Azure 入口網站中，找出 web 應用程式 [web 應用程式] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="df430-212">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="df430-213">選取**瀏覽**在預設瀏覽器中檢視應用程式。</span><span class="sxs-lookup"><span data-stu-id="df430-213">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="df430-214">開啟瀏覽器並輸入 web 應用程式的 URL。</span><span class="sxs-lookup"><span data-stu-id="df430-214">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="df430-215">範例：`http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="df430-215">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="df430-216">更新 web 應用程式，並重新發行</span><span class="sxs-lookup"><span data-stu-id="df430-216">Update the web app and republish</span></span>

<span data-ttu-id="df430-217">變更本機的程式碼後，重新發佈：</span><span class="sxs-lookup"><span data-stu-id="df430-217">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="df430-218">在 Visual Studio 的方案總管中，開啟 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="df430-218">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="df430-219">在 `Configure` 方法中，修改 `Response.WriteAsync` 方法，使其如下所示：</span><span class="sxs-lookup"><span data-stu-id="df430-219">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="df430-220">變更儲存到*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="df430-220">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="df430-221">在方案總管中，以滑鼠右鍵按一下 [方案 'SampleWebAppDemo']，然後選取 [認可]。</span><span class="sxs-lookup"><span data-stu-id="df430-221">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="df430-222">**Team Explorer**隨即出現。</span><span class="sxs-lookup"><span data-stu-id="df430-222">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="df430-223">輸入 「 認可 」 訊息，例如`Update #2`。</span><span class="sxs-lookup"><span data-stu-id="df430-223">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="df430-224">按 [認可] 按鈕，認可專案中的變更。</span><span class="sxs-lookup"><span data-stu-id="df430-224">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="df430-225">選取 [首頁] > [同步] > [動作] > [推送]。</span><span class="sxs-lookup"><span data-stu-id="df430-225">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="df430-226">或者，從將變更內容推送**命令視窗**開啟**命令視窗**、 專案目錄中，變更，以及輸入 git 命令。</span><span class="sxs-lookup"><span data-stu-id="df430-226">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="df430-227">範例：</span><span class="sxs-lookup"><span data-stu-id="df430-227">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="df430-228">在 Azure 中檢視更新的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="df430-228">View the updated web app in Azure</span></span>

<span data-ttu-id="df430-229">檢視更新的 web 應用程式選取**瀏覽**從 web 應用程式 刀鋒視窗在 Azure 入口網站或開啟瀏覽器，然後輸入 web 應用程式的 URL。</span><span class="sxs-lookup"><span data-stu-id="df430-229">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="df430-230">範例：`http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="df430-230">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="df430-231">其他資源</span><span class="sxs-lookup"><span data-stu-id="df430-231">Additional resources</span></span>

* [<span data-ttu-id="df430-232">使用 VSTS 來建立及發行至 Azure Web 應用程式使用的連續部署</span><span class="sxs-lookup"><span data-stu-id="df430-232">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="df430-233">專案 Kudu</span><span class="sxs-lookup"><span data-stu-id="df430-233">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
