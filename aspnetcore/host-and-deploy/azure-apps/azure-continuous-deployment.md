---
title: 搭配 ASP.NET Core 使用 Visual Studio 與 Git 持續部署至 Azure
author: rick-anderson
description: 了解如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過 Git 持續部署將它部署到 Azure App Service。
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: e12c2ee0b78db105b431770e8644e7d19d915765
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284420"
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="667a7-103">搭配 ASP.NET Core 使用 Visual Studio 與 Git 持續部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="667a7-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="667a7-104">作者：[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="667a7-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="667a7-105">本教學課程說明如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過持續部署將它從 Visual Studio 部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="667a7-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="667a7-106">另請參閱[使用 Azure Pipelines 建立您的第一個管線](/azure/devops/pipelines/get-started-yaml)，顯示如何使用 Azure DevOps Services，為 [Azure App Service](/azure/app-service/app-service-web-overview) 設定持續傳遞 (CD) 工作流程。</span><span class="sxs-lookup"><span data-stu-id="667a7-106">See also [Create your first pipeline with Azure Pipelines](/azure/devops/pipelines/get-started-yaml), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Azure DevOps Services.</span></span> <span data-ttu-id="667a7-107">Azure Pipelines (一種 Azure DevOps Services 服務) 可輕鬆設定強大的部署管線，為託管於 Azure App Service 的應用程式發佈更新。</span><span class="sxs-lookup"><span data-stu-id="667a7-107">Azure Pipelines (an Azure DevOps Services service) simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="667a7-108">您可以透過 Azure 入口網站設定這個管道，以建置，執行測試，部署到預備位置，然後再部署到實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="667a7-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="667a7-109">若要完成本教學課程，您需要 Microsoft Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="667a7-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="667a7-110">若要取得帳戶，請[啟動 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F)或[註冊免費試用](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="667a7-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="667a7-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="667a7-111">Prerequisites</span></span>

<span data-ttu-id="667a7-112">本教學課程假設您已安裝下列軟體：</span><span class="sxs-lookup"><span data-stu-id="667a7-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="667a7-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="667a7-113">Visual Studio</span></span>](https://www.visualstudio.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="667a7-114">[Git](https://git-scm.com/downloads) (適用於 Windows)</span><span class="sxs-lookup"><span data-stu-id="667a7-114">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="667a7-115">建立 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="667a7-115">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="667a7-116">啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="667a7-116">Start Visual Studio.</span></span>

1. <span data-ttu-id="667a7-117">從 [檔案] 功能表選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="667a7-117">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="667a7-118">選取 [ASP.NET Core Web 應用程式] 專案範本。</span><span class="sxs-lookup"><span data-stu-id="667a7-118">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="667a7-119">它會出現在 [已安裝] > [範本] > [Visual C#] > [.NET Core] 下。</span><span class="sxs-lookup"><span data-stu-id="667a7-119">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="667a7-120">將專案命名為 `SampleWebAppDemo`。</span><span class="sxs-lookup"><span data-stu-id="667a7-120">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="667a7-121">選取 [建立新的 Git 存放庫] 選項，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="667a7-121">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![[新增專案] 對話](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="667a7-123">在 [新增 ASP.NET Core 專案] 對話方塊中，選取 ASP.NET Core 的 [空白] 範本，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="667a7-123">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![[新增 ASP.NET Core 專案] 對話方塊](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="667a7-125">.NET Core 的最新版本為 2.0。</span><span class="sxs-lookup"><span data-stu-id="667a7-125">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="667a7-126">在本機執行 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="667a7-126">Running the web app locally</span></span>

1. <span data-ttu-id="667a7-127">一旦 Visual Studio 完成建立應用程式之後，您即可選取 [偵錯] > [開始偵錯] 以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="667a7-127">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="667a7-128">或者，也可以按 **F5**。</span><span class="sxs-lookup"><span data-stu-id="667a7-128">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="667a7-129">系統可能需要一點時間來初始化 Visual Studio 和新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="667a7-129">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="667a7-130">完成後，瀏覽器會顯示執行中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="667a7-130">Once it's complete, the browser shows the running app.</span></span>

   ![顯示執行中應用程式的瀏覽器視窗，該應用程式顯示 'Hello World!'](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="667a7-132">在檢閱執行中的 Web 應用程式之後，請關閉瀏覽器，然後選取 Visual Studio 工具列中的「停止偵錯」圖示以停止應用程式。</span><span class="sxs-lookup"><span data-stu-id="667a7-132">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="667a7-133">在 Azure 入口網站中建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="667a7-133">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="667a7-134">下列步驟會在 Azure 入口網站中建立 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="667a7-134">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="667a7-135">登入 [Azure 入口網站](https://portal.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="667a7-135">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="667a7-136">選取入口網站介面左上角的 [新增]。</span><span class="sxs-lookup"><span data-stu-id="667a7-136">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="667a7-137">選取 [Web + 行動] > [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="667a7-137">Select **Web + Mobile** > **Web App**.</span></span>

   ![Microsoft Azure 入口網站：[新增] 按鈕：[Marketplace] 下的 [Web + 行動]：[精選應用程式] 下的 [Web 應用程式] 按鈕](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="667a7-139">在 [Web 應用程式] 刀鋒視窗中，為 [App Service 名稱] 輸入唯一值。</span><span class="sxs-lookup"><span data-stu-id="667a7-139">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![[Web 應用程式] 刀鋒視窗](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="667a7-141">[App Service 名稱] 必須是唯一的名稱。</span><span class="sxs-lookup"><span data-stu-id="667a7-141">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="667a7-142">提供名稱時，入口網站會強制執行這項規則。</span><span class="sxs-lookup"><span data-stu-id="667a7-142">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="667a7-143">如果輸入不同的值，請將本教學課程中的每個 **SampleWebAppDemo** 取代為該值。</span><span class="sxs-lookup"><span data-stu-id="667a7-143">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="667a7-144">此外，請在 [Web 應用程式] 刀鋒視窗中，選取現有的 [App Service 方案/位置] 或另外新建一個。</span><span class="sxs-lookup"><span data-stu-id="667a7-144">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="667a7-145">如果要建立新的方案，請選取定價層、位置和其他選項。</span><span class="sxs-lookup"><span data-stu-id="667a7-145">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="667a7-146">如需 App Service 方案的詳細資訊，請參閱 [Azure App Service 方案深入概觀](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)。</span><span class="sxs-lookup"><span data-stu-id="667a7-146">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="667a7-147">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="667a7-147">Select **Create**.</span></span> <span data-ttu-id="667a7-148">Azure 將會開始佈建並啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="667a7-148">Azure will provision and start the web app.</span></span>

   ![Azure 入口網站：範例 Web 應用程式示範 01 [基本資訊] 刀鋒視窗](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="667a7-150">啟用新 Web 應用程式的 Git 發行功能</span><span class="sxs-lookup"><span data-stu-id="667a7-150">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="667a7-151">Git 是一種分散式版本控制系統，可用來部署 Azure App Service Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="667a7-151">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="667a7-152">Web 應用程式程式碼會儲存在本機 Git 存放庫中，而系統會透過將該程式碼推送至遠端存放庫來將它部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="667a7-152">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="667a7-153">登入 [Azure 入口網站](https://portal.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="667a7-153">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="667a7-154">選取 [應用程式服務]以檢視與 Azure 訂用帳戶相關聯的應用程式服務清單。</span><span class="sxs-lookup"><span data-stu-id="667a7-154">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="667a7-155">選取本教學課程上一節所建立的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="667a7-155">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="667a7-156">在 [開發] 刀鋒視窗中，選取 [部署選項] > [選擇來源] > [本機 Git 存放庫]。</span><span class="sxs-lookup"><span data-stu-id="667a7-156">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![[設定] 刀鋒視窗：[部署來源] 刀鋒視窗：[選擇來源] 刀鋒視窗](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="667a7-158">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="667a7-158">Select **OK**.</span></span>

1. <span data-ttu-id="667a7-159">如果尚未設定發行 Web 應用程式或其他 App Service 應用程式所需的部署認證，請立即設定：</span><span class="sxs-lookup"><span data-stu-id="667a7-159">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="667a7-160">選取 [設定] > [部署認證]。</span><span class="sxs-lookup"><span data-stu-id="667a7-160">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="667a7-161">[設定部署認證] 刀鋒視窗隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="667a7-161">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="667a7-162">建立使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="667a7-162">Create a user name and password.</span></span> <span data-ttu-id="667a7-163">儲存該密碼，以於日後設定 Git 時使用。</span><span class="sxs-lookup"><span data-stu-id="667a7-163">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="667a7-164">選取 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="667a7-164">Select **Save**.</span></span>

1. <span data-ttu-id="667a7-165">在 [Web 應用程式] 刀鋒視窗中，選取 [設定] > [屬性]。</span><span class="sxs-lookup"><span data-stu-id="667a7-165">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="667a7-166">[GIT URL] 下方會顯示作為部署目的地遠端 Git 存放庫的 URL。</span><span class="sxs-lookup"><span data-stu-id="667a7-166">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="667a7-167">複製 **GIT URL** 值，以便於教學課程稍後使用。</span><span class="sxs-lookup"><span data-stu-id="667a7-167">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure 入口網站：應用程式的 [屬性] 刀鋒視窗](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="667a7-169">將 Web 應用程式發行至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="667a7-169">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="667a7-170">在本節中，使用 Visual Studio 建立本機 Git 存放庫，並將 Web 應用程式從該存放庫推送到 Azure 以進行部署。</span><span class="sxs-lookup"><span data-stu-id="667a7-170">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="667a7-171">其中包括下列步驟：</span><span class="sxs-lookup"><span data-stu-id="667a7-171">The steps involved include the following:</span></span>

* <span data-ttu-id="667a7-172">使用 [GIT URL] 值來新增遠端存放庫設定，以便本機存放庫可以被部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="667a7-172">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="667a7-173">認可專案的變更。</span><span class="sxs-lookup"><span data-stu-id="667a7-173">Commit project changes.</span></span>
* <span data-ttu-id="667a7-174">將專案變更從本機存放庫推送到 Azure 上的遠端存放庫。</span><span class="sxs-lookup"><span data-stu-id="667a7-174">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="667a7-175">在方案總管中，以滑鼠右鍵按一下 [方案 'SampleWebAppDemo']，然後選取 [認可]。</span><span class="sxs-lookup"><span data-stu-id="667a7-175">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="667a7-176">[Team Explorer] 隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="667a7-176">The **Team Explorer** is displayed.</span></span>

   ![Team Explorer 的 [連線] 索引標籤](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="667a7-178">在 **Team Explorer** 中，選取 [首頁] (首頁圖示) > [設定] > [存放庫設定]。</span><span class="sxs-lookup"><span data-stu-id="667a7-178">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="667a7-179">在 [存放庫設定] 的 [遠端] 區段中，選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="667a7-179">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="667a7-180">[新增遠端] 對話方塊隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="667a7-180">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="667a7-181">將遠端的 [名稱] 設為 **Azure-SampleApp**。</span><span class="sxs-lookup"><span data-stu-id="667a7-181">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="667a7-182">將 [擷取] 的值設為本教學課程稍早從 Azure 所複製的 [Git URL]。</span><span class="sxs-lookup"><span data-stu-id="667a7-182">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="667a7-183">請注意，這應該是結尾為 **.git** 的 URL。</span><span class="sxs-lookup"><span data-stu-id="667a7-183">Note that this is the URL that ends with **.git**.</span></span>

   ![[編輯遠端] 對話方塊](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="667a7-185">或者，透過開啟 [命令視窗]，變更為專案目錄，然後輸入命令，來從 [命令視窗] 指定遠端存放庫。</span><span class="sxs-lookup"><span data-stu-id="667a7-185">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="667a7-186">範例：</span><span class="sxs-lookup"><span data-stu-id="667a7-186">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="667a7-187">選取 [首頁] (首頁圖示) > [設定] > [全域設定]。</span><span class="sxs-lookup"><span data-stu-id="667a7-187">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="667a7-188">確認名稱和電子郵件地址已設定完畢。</span><span class="sxs-lookup"><span data-stu-id="667a7-188">Confirm that the name and email address are set.</span></span> <span data-ttu-id="667a7-189">必要時，請選取 [更新]。</span><span class="sxs-lookup"><span data-stu-id="667a7-189">Select **Update** if required.</span></span>

1. <span data-ttu-id="667a7-190">選取 [首頁] > [變更]，即可返回 [變更] 檢視。</span><span class="sxs-lookup"><span data-stu-id="667a7-190">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="667a7-191">輸入認可訊息，例如 **Initial Push #1**，然後選取 [認可]。</span><span class="sxs-lookup"><span data-stu-id="667a7-191">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="667a7-192">此動作會在本機建立一項*認可*作業。</span><span class="sxs-lookup"><span data-stu-id="667a7-192">This action creates a *commit* locally.</span></span>

   ![Team Explorer 的 [連線] 索引標籤](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="667a7-194">或者，透過開啟 [命令視窗]，變更為專案目錄，然後輸入 git 命令，來從 [命令視窗] 認可變更。</span><span class="sxs-lookup"><span data-stu-id="667a7-194">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="667a7-195">範例：</span><span class="sxs-lookup"><span data-stu-id="667a7-195">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="667a7-196">選取 [首頁] > [同步] > [動作] > [開啟命令提示字元]。</span><span class="sxs-lookup"><span data-stu-id="667a7-196">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="667a7-197">命令提示字元會開啟並切換至專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="667a7-197">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="667a7-198">在命令視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="667a7-198">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="667a7-199">輸入稍早在 Azure 中建立的 Azure [部署認證] 密碼。</span><span class="sxs-lookup"><span data-stu-id="667a7-199">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="667a7-200">此命令會開始進行將本機專案檔推送到 Azure 的程序。</span><span class="sxs-lookup"><span data-stu-id="667a7-200">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="667a7-201">上述命令的輸出結尾會顯示部署成功的訊息。</span><span class="sxs-lookup"><span data-stu-id="667a7-201">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="667a7-202">如果需在專案上進行共同作業，請考慮在推送到 Azure 之前，先推送到 [GitHub](https://github.com) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="667a7-202">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="667a7-203">驗證作用中的部署</span><span class="sxs-lookup"><span data-stu-id="667a7-203">Verify the Active Deployment</span></span>

<span data-ttu-id="667a7-204">確認 Web 應用程式已成功從本機環境傳送至 Azure。</span><span class="sxs-lookup"><span data-stu-id="667a7-204">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="667a7-205">在 [Azure 入口網站](https://portal.azure.com)中，選取該 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="667a7-205">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="667a7-206">選取 [部署] > [部署選項]。</span><span class="sxs-lookup"><span data-stu-id="667a7-206">Select **Deployment** > **Deployment options**.</span></span>

![Azure 入口網站：[設定] 刀鋒視窗：顯示成功部署的 [部署] 刀鋒視窗](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="667a7-208">在 Azure 中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="667a7-208">Run the app in Azure</span></span>

<span data-ttu-id="667a7-209">在 Web 應用程式已部署至 Azure 之後，請執行該應用程式。</span><span class="sxs-lookup"><span data-stu-id="667a7-209">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="667a7-210">這可由下列兩種方法來完成：</span><span class="sxs-lookup"><span data-stu-id="667a7-210">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="667a7-211">在 Azure 入口網站中，找出該 Web 應用程式的 Web 應用程式刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="667a7-211">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="667a7-212">選取 [瀏覽]以在預設瀏覽器中檢視該應用程式。</span><span class="sxs-lookup"><span data-stu-id="667a7-212">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="667a7-213">開啟瀏覽器，並輸入 Web 應用程式的 URL。</span><span class="sxs-lookup"><span data-stu-id="667a7-213">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="667a7-214">範例：`http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="667a7-214">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="667a7-215">更新 Web 應用程式，並重新發行</span><span class="sxs-lookup"><span data-stu-id="667a7-215">Update the web app and republish</span></span>

<span data-ttu-id="667a7-216">在針對本機程式碼做出變更後，請重新發行：</span><span class="sxs-lookup"><span data-stu-id="667a7-216">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="667a7-217">在 Visual Studio 的方案總管中，開啟 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="667a7-217">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="667a7-218">在 `Configure` 方法中，修改 `Response.WriteAsync` 方法，使其如下所示：</span><span class="sxs-lookup"><span data-stu-id="667a7-218">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="667a7-219">儲存對 *Startup.cs* 所做的變更。</span><span class="sxs-lookup"><span data-stu-id="667a7-219">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="667a7-220">在方案總管中，以滑鼠右鍵按一下 [方案 'SampleWebAppDemo']，然後選取 [認可]。</span><span class="sxs-lookup"><span data-stu-id="667a7-220">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="667a7-221">[Team Explorer] 隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="667a7-221">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="667a7-222">輸入認可訊息，例如 `Update #2`。</span><span class="sxs-lookup"><span data-stu-id="667a7-222">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="667a7-223">按 [認可] 按鈕，認可專案中的變更。</span><span class="sxs-lookup"><span data-stu-id="667a7-223">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="667a7-224">選取 [首頁] > [同步] > [動作] > [推送]。</span><span class="sxs-lookup"><span data-stu-id="667a7-224">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="667a7-225">或者，透過開啟 [命令視窗]，變更為專案目錄，然後輸入 git 命令，來從 [命令視窗] 推送變更。</span><span class="sxs-lookup"><span data-stu-id="667a7-225">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="667a7-226">範例：</span><span class="sxs-lookup"><span data-stu-id="667a7-226">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="667a7-227">在 Azure 中檢視更新的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="667a7-227">View the updated web app in Azure</span></span>

<span data-ttu-id="667a7-228">從 Azure 入口網站的 Web 應用程式刀鋒視窗選取 [瀏覽]，或是開啟瀏覽器並輸入 Web 應用程式的 URL，來檢視更新之後的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="667a7-228">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="667a7-229">範例：`http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="667a7-229">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="667a7-230">其他資源</span><span class="sxs-lookup"><span data-stu-id="667a7-230">Additional resources</span></span>

* [<span data-ttu-id="667a7-231">使用 Azure Pipelines 建立您的第一個管線</span><span class="sxs-lookup"><span data-stu-id="667a7-231">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)
* [<span data-ttu-id="667a7-232">專案 Kudu</span><span class="sxs-lookup"><span data-stu-id="667a7-232">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
* <xref:host-and-deploy/visual-studio-publish-profiles>
