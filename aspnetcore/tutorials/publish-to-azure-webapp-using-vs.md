---
title: "使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: c4df5f4551760fbc4ed4b11362d249b24f186e00
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="5ad2f-103">使用 Visual Studio 將 ASP.NET Core Web 應用程式發行到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5ad2f-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="5ad2f-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Cesar Blum Silveira](https://github.com/cesarbs)</span><span class="sxs-lookup"><span data-stu-id="5ad2f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Cesar Blum Silveira](https://github.com/cesarbs)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="5ad2f-105">設定開發環境</span><span class="sxs-lookup"><span data-stu-id="5ad2f-105">Set up the development environment</span></span>

* <span data-ttu-id="5ad2f-106">安裝最新的 [Azure SDK for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-106">Install the latest [Azure SDK for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs).</span></span> <span data-ttu-id="5ad2f-107">如果還沒有 Visual Studio，SDK 會安裝它。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-107">The SDK installs Visual Studio if you don't already have it.</span></span>

> [!NOTE]
> <span data-ttu-id="5ad2f-108">如果您的電腦沒有許多相依性，SDK 安裝可能需要超過 30 分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-108">The SDK installation can take more than 30 minutes if your machine doesn't have many of the dependencies.</span></span>

* <span data-ttu-id="5ad2f-109">安裝 [.NET Core + Visual Studio 工具](http://go.microsoft.com/fwlink/?LinkID=798306)</span><span class="sxs-lookup"><span data-stu-id="5ad2f-109">Install [.NET Core + Visual Studio tooling](http://go.microsoft.com/fwlink/?LinkID=798306)</span></span>

* <span data-ttu-id="5ad2f-110">驗證您的 [Azure 帳戶](https://portal.azure.com/)。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-110">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="5ad2f-111">您可以[建立免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/)或[啟用 Visual Studio 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-111">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="5ad2f-112">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="5ad2f-112">Create a web app</span></span>

<span data-ttu-id="5ad2f-113">在 Visual Studio 起始頁中，點選 [新增專案...]。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-113">In the Visual Studio Start Page, tap **New Project...**.</span></span>

![起始頁](publish-to-azure-webapp-using-vs/_static/new_project.png)

<span data-ttu-id="5ad2f-115">或者，您可以使用功能表來建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-115">Alternatively, you can use the menus to create a new project.</span></span> <span data-ttu-id="5ad2f-116">點選 [檔案] > [新增] > [專案...]。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-116">Tap **File > New > Project...**.</span></span>

![檔案功能表](publish-to-azure-webapp-using-vs/_static/alt_new_project.png)

<span data-ttu-id="5ad2f-118">完成 [新增專案] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="5ad2f-118">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="5ad2f-119">在左窗格中，點選 [Web]</span><span class="sxs-lookup"><span data-stu-id="5ad2f-119">In the left pane, tap **Web**</span></span>

* <span data-ttu-id="5ad2f-120">在中央窗格中，點選 [ASP.NET Core Web 應用程式 (.NET Core)]</span><span class="sxs-lookup"><span data-stu-id="5ad2f-120">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>

* <span data-ttu-id="5ad2f-121">點選 [確定]</span><span class="sxs-lookup"><span data-stu-id="5ad2f-121">Tap **OK**</span></span>

![[新增專案] 對話](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="5ad2f-123">在 [新增 ASP.NET Core Web 應用程式 (.NET Core)] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="5ad2f-123">In the **New ASP.NET Core Web Application (.NET Core)** dialog:</span></span>

* <span data-ttu-id="5ad2f-124">點選 [Web 應用程式]</span><span class="sxs-lookup"><span data-stu-id="5ad2f-124">Tap **Web Application**</span></span>

* <span data-ttu-id="5ad2f-125">確認 [驗證] 已設定為 [個別使用者帳戶]</span><span class="sxs-lookup"><span data-stu-id="5ad2f-125">Verify **Authentication** is set to **Individual User Accounts**</span></span>

* <span data-ttu-id="5ad2f-126">確認**未**核取 [雲端中的主機]</span><span class="sxs-lookup"><span data-stu-id="5ad2f-126">Verify **Host in the cloud** is **not** checked</span></span>

* <span data-ttu-id="5ad2f-127">點選 [確定]</span><span class="sxs-lookup"><span data-stu-id="5ad2f-127">Tap **OK**</span></span>

![[新增 ASP.NET Core Web 應用程式 (.NET Core)] 對話方塊](publish-to-azure-webapp-using-vs/_static/noath.png)

## <a name="test-the-app-locally"></a><span data-ttu-id="5ad2f-129">在本機測試應用程式</span><span class="sxs-lookup"><span data-stu-id="5ad2f-129">Test the app locally</span></span>

* <span data-ttu-id="5ad2f-130">按 **Ctrl-F5** 在本機執行應用程式</span><span class="sxs-lookup"><span data-stu-id="5ad2f-130">Press **Ctrl-F5** to run the app locally</span></span>

* <span data-ttu-id="5ad2f-131">點選 [關於] 和 [連絡人] 連結。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-131">Tap the **About** and **Contact** links.</span></span> <span data-ttu-id="5ad2f-132">根據裝置大小，您可能需要點選瀏覽圖示來顯示連結</span><span class="sxs-lookup"><span data-stu-id="5ad2f-132">Depending on the size of your device, you might need to tap the navigation icon to show the links</span></span>

![在 Microsoft Edge 中於 localhost 開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="5ad2f-134">點選 [註冊]，並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-134">Tap **Register** and register a new user.</span></span> <span data-ttu-id="5ad2f-135">您可以使用虛構的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-135">You can use a fictitious email address.</span></span> <span data-ttu-id="5ad2f-136">在提交時，您會收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="5ad2f-136">When you submit, you'll get the following error:</span></span>

![內部伺服器錯誤：處理要求時資料庫作業失敗。](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="5ad2f-140">您可以使用兩種不同的方式解決這個問題：</span><span class="sxs-lookup"><span data-stu-id="5ad2f-140">You can fix the problem in two different ways:</span></span>

* <span data-ttu-id="5ad2f-141">點選 [套用移轉]，一旦頁面更新，請重新整理頁面；或</span><span class="sxs-lookup"><span data-stu-id="5ad2f-141">Tap **Apply Migrations** and, once the page updates, refresh the page; or</span></span>

* <span data-ttu-id="5ad2f-142">從專案目錄中的命令提示字元執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="5ad2f-142">Run the following from a command prompt in the project's directory:</span></span>

  <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

  ```
  dotnet ef database update
     ```

<span data-ttu-id="5ad2f-143">應用程式會顯示用來註冊新使用者的電子郵件和 [登出] 連結。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-143">The app displays the email used to register the new user and a **Log off** link.</span></span>

![在 Microsoft Edge 中開啟的 Web 應用程式。](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="5ad2f-146">將應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="5ad2f-146">Deploy the app to Azure</span></span>

<span data-ttu-id="5ad2f-147">確認部署的已發佈應用程式未在執行中。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-147">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="5ad2f-148">當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-148">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="5ad2f-149">因為無法複製鎖定的檔案，所以無法執行部署。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-149">Deployment can't occur because locked files can't be copied.</span></span>

<span data-ttu-id="5ad2f-150">在方案總管中，以滑鼠右鍵按一下專案，然後選取 [發行]。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-150">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="5ad2f-152">在 [發行] 對話方塊中，點選 [Microsoft Azure App Service]。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-152">In the **Publish** dialog, tap **Microsoft Azure App Service**.</span></span>

![[發行] 對話方塊](publish-to-azure-webapp-using-vs/_static/maas1.png)

<span data-ttu-id="5ad2f-154">點選 [新增...] 來建立新的資源群組。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-154">Tap **New...** to create a new resource group.</span></span> <span data-ttu-id="5ad2f-155">建立新的資源群組可讓您更輕鬆地刪除您在本教學課程中建立的所有 Azure 資源。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-155">Creating a new resource group will make it easier to delete all the Azure resources you create in this tutorial.</span></span>

![[應用程式服務] 對話方塊](publish-to-azure-webapp-using-vs/_static/newrg1.png)

<span data-ttu-id="5ad2f-157">建立新的資源群組和應用程式服務方案：</span><span class="sxs-lookup"><span data-stu-id="5ad2f-157">Create a new resource group and app service plan:</span></span>

* <span data-ttu-id="5ad2f-158">針對資源群組點選 [新增...]，然後輸入新資源群組的名稱。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-158">Tap **New...** for the resource group and enter a name for the new resource group</span></span>

* <span data-ttu-id="5ad2f-159">針對應用程式服務方案點選 [新增...]，然後選取您附近的位置。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-159">Tap **New...** for the  app service plan and select a location near you.</span></span> <span data-ttu-id="5ad2f-160">您可以保留預設產生的名稱</span><span class="sxs-lookup"><span data-stu-id="5ad2f-160">You can keep the default generated name</span></span>

* <span data-ttu-id="5ad2f-161">點選 [探索其他 Azure 服務] 以建立新的資料庫</span><span class="sxs-lookup"><span data-stu-id="5ad2f-161">Tap **Explore additional Azure services** to create a new database</span></span>

![[新增資源群組] 對話方塊：裝載面板](publish-to-azure-webapp-using-vs/_static/cas.png)

* <span data-ttu-id="5ad2f-163">點選綠色的  **+** 圖示以建立新的 SQL Database</span><span class="sxs-lookup"><span data-stu-id="5ad2f-163">Tap the green **+** icon to create a new SQL Database</span></span>

![[新增資源群組] 對話方塊：服務面板](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="5ad2f-165">在 [設定 SQL Database] 對話方塊上點選 [新增...] 來建立新的資料庫伺服器。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-165">Tap **New...** on the **Configure SQL Database** dialog to create a new database server.</span></span>

![[設定 SQL Database] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf.png)

* <span data-ttu-id="5ad2f-167">輸入系統管理員使用者名稱和密碼，然後點選 [確定]。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-167">Enter an administrator user name and password, and then tap **OK**.</span></span> <span data-ttu-id="5ad2f-168">請勿忘記您在此步驟中建立的使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-168">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="5ad2f-169">您可以保留預設的**伺服器名稱**</span><span class="sxs-lookup"><span data-stu-id="5ad2f-169">You can keep the default **Server Name**</span></span>

![[設定 SQL Server] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

> [!NOTE]
> <span data-ttu-id="5ad2f-171">"admin" 不允許作為系統管理員使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-171">"admin" is not allowed as the administrator user name.</span></span>

* <span data-ttu-id="5ad2f-172">在 [設定 SQL Database] 對話方塊中選點 [確定]</span><span class="sxs-lookup"><span data-stu-id="5ad2f-172">Tap **OK** on the  **Configure SQL Database** dialog</span></span>

![[設定 SQL Database] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="5ad2f-174">在 [建立應用程式服務] 對話方塊上點選 [建立]</span><span class="sxs-lookup"><span data-stu-id="5ad2f-174">Tap **Create** on the **Create App Service** dialog</span></span>

![[建立應用程式服務] 對話方塊](publish-to-azure-webapp-using-vs/_static/create_as.png)

* <span data-ttu-id="5ad2f-176">在 [發行] 對話方塊中點選 [下一步]</span><span class="sxs-lookup"><span data-stu-id="5ad2f-176">Tap **Next** in the **Publish** dialog</span></span>

![[發行] 對話方塊：連線面板](publish-to-azure-webapp-using-vs/_static/pubc.png)

* <span data-ttu-id="5ad2f-178">在 [發行] 對話方塊的 [設定] 階段：</span><span class="sxs-lookup"><span data-stu-id="5ad2f-178">On the **Settings** stage of the **Publish** dialog:</span></span>

  * <span data-ttu-id="5ad2f-179">展開 [資料庫] 並核取 [在執行階段使用此連線字串]</span><span class="sxs-lookup"><span data-stu-id="5ad2f-179">Expand **Databases** and check **Use this connection string at runtime**</span></span>

  * <span data-ttu-id="5ad2f-180">展開 [Entity Framework 移轉] 並核取 [在發行時套用此移轉]</span><span class="sxs-lookup"><span data-stu-id="5ad2f-180">Expand **Entity Framework Migrations** and check **Apply this migration on publish**</span></span>

* <span data-ttu-id="5ad2f-181">點選 [發行] 並等候 Visual Studio 完成發行應用程式</span><span class="sxs-lookup"><span data-stu-id="5ad2f-181">Tap **Publish** and wait until Visual Studio finishes publishing your app</span></span>

![[發行] 對話方塊：設定面板](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="5ad2f-183">Visual Studio 會將您的應用程式發行至 Azure，並在您的瀏覽器中啟動雲端應用程式。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-183">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="5ad2f-184">在 Azure 中測試應用程式</span><span class="sxs-lookup"><span data-stu-id="5ad2f-184">Test your app in Azure</span></span>

* <span data-ttu-id="5ad2f-185">測試 [關於] 和 [連絡人] 連結</span><span class="sxs-lookup"><span data-stu-id="5ad2f-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="5ad2f-186">註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="5ad2f-186">Register a new user</span></span>

![在 Microsoft Edge 中於 Azure App Service 上開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="update-the-app"></a><span data-ttu-id="5ad2f-188">更新應用程式</span><span class="sxs-lookup"><span data-stu-id="5ad2f-188">Update the app</span></span>

* <span data-ttu-id="5ad2f-189">編輯 `Views/Home/About.cshtml` Razor 檢視檔案，然後變更其內容。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-189">Edit the `Views/Home/About.cshtml` Razor view file and change its contents.</span></span> <span data-ttu-id="5ad2f-190">例如: </span><span class="sxs-lookup"><span data-stu-id="5ad2f-190">For example:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [7]}} -->

```html
@{
       ViewData["Title"] = "About";
   }
   <h2>@ViewData["Title"].</h2>
   <h3>@ViewData["Message"]</h3>

   <p>My updated about page.</p>
   ```

* <span data-ttu-id="5ad2f-191">以滑鼠右鍵按一下專案，然後再次點選 [發行...]</span><span class="sxs-lookup"><span data-stu-id="5ad2f-191">Right-click on the project and tap **Publish...** again</span></span>

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="5ad2f-193">發行應用程式之後，請確認您所做的變更可在 Azure 上使用</span><span class="sxs-lookup"><span data-stu-id="5ad2f-193">After the app is published, verify the changes you made are available on Azure</span></span>

### <a name="clean-up"></a><span data-ttu-id="5ad2f-194">清除</span><span class="sxs-lookup"><span data-stu-id="5ad2f-194">Clean up</span></span>

<span data-ttu-id="5ad2f-195">當您完成測試應用程式時，請移至 [Azure 入口網站](https://portal.azure.com/)並刪除應用程式。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-195">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="5ad2f-196">選取 [資源群組]，然後點選您建立的資源群組</span><span class="sxs-lookup"><span data-stu-id="5ad2f-196">Select **Resource groups**, then tap the resource group you created</span></span>

![Azure 入口網站：資訊看板功能表中的 [資源群組]](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="5ad2f-198">在 [資源群組] 刀鋒視窗中，點選 [刪除]</span><span class="sxs-lookup"><span data-stu-id="5ad2f-198">In the **Resource group** blade, tap **Delete**</span></span>

![Azure 入口網站：[資源群組] 刀鋒視窗](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="5ad2f-200">輸入資源群組的名稱，然後點選 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="5ad2f-200">Enter the name of the resource group and tap **Delete**.</span></span> <span data-ttu-id="5ad2f-201">您在本教學課程中建立的應用程式和其他所有資源都會立即從 Azure 中刪除</span><span class="sxs-lookup"><span data-stu-id="5ad2f-201">Your app and all other resources created in this tutorial are now deleted from Azure</span></span>

### <a name="next-steps"></a><span data-ttu-id="5ad2f-202">後續步驟</span><span class="sxs-lookup"><span data-stu-id="5ad2f-202">Next steps</span></span>

* [<span data-ttu-id="5ad2f-203">ASP.NET Core MVC 與 Visual Studio 使用者入門</span><span class="sxs-lookup"><span data-stu-id="5ad2f-203">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="5ad2f-204">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="5ad2f-204">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="5ad2f-205">基礎概念</span><span class="sxs-lookup"><span data-stu-id="5ad2f-205">Fundamentals</span></span>](../fundamentals/index.md)
