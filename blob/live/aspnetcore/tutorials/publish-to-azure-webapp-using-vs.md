---
title: "使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure"
author: rick-anderson
description: "了解如何使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure App Service。"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/16/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: e8de630c4fa8cff5395f7246b91384d4725e4ca6
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2018
---
<span data-ttu-id="f997a-104">/zh-TW</span><span class="sxs-lookup"><span data-stu-id="f997a-104">/en-us</span></span>

# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="f997a-105">使用 Visual Studio 將 ASP.NET Core Web 應用程式發行到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f997a-105">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="f997a-106">由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Cesar Blum Silveira](https://github.com/cesarbs) 及 [Rachel Appel](https://twitter.com/rachelappel) 共同編纂</span><span class="sxs-lookup"><span data-stu-id="f997a-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="f997a-107">如果您是使用 Mac，請參閱[從 Visual Studio for Mac 發佈至 Azure](https://blog.xamarin.com/publish-azure-visual-studio-mac/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="f997a-107">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="f997a-108">設定</span><span class="sxs-lookup"><span data-stu-id="f997a-108">Set up</span></span>

* <span data-ttu-id="f997a-109">如果沒有帳戶，請開啟[免費的 Azure 帳戶](https://aka.ms/K5y5yh)。</span><span class="sxs-lookup"><span data-stu-id="f997a-109">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="f997a-110">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="f997a-110">Create a web app</span></span>

<span data-ttu-id="f997a-111">在 Visual Studio 起始畫面中，選取 [檔案] > [新增] > [專案...]</span><span class="sxs-lookup"><span data-stu-id="f997a-111">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![檔案功能表](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="f997a-113">完成 [新增專案] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="f997a-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="f997a-114">在左窗格中，選取 [.NET Core]。</span><span class="sxs-lookup"><span data-stu-id="f997a-114">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="f997a-115">在中央窗格中，選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="f997a-115">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f997a-116">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="f997a-116">Select **OK**.</span></span>

![[新增專案] 對話](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="f997a-118">在 [新增 ASP.NET Core Web 應用程式] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="f997a-118">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="f997a-119">選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="f997a-119">Select **Web Application**.</span></span>
* <span data-ttu-id="f997a-120">選取 [變更驗證]。</span><span class="sxs-lookup"><span data-stu-id="f997a-120">Select **Change Authentication**.</span></span>

![[新增專案] 對話](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="f997a-122">[變更驗證] 對話方塊會隨即出現。</span><span class="sxs-lookup"><span data-stu-id="f997a-122">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="f997a-123">選取 [個別使用者帳戶]。</span><span class="sxs-lookup"><span data-stu-id="f997a-123">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="f997a-124">選取 [確定] 返回 [新增 ASP.NET Core Web 應用程式]，然後再次選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="f997a-124">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![[新增 ASP.NET Core Web 驗證] 對話方塊](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="f997a-126">Visual Studio 會建立解決方案。</span><span class="sxs-lookup"><span data-stu-id="f997a-126">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="f997a-127">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="f997a-127">Run the app</span></span>

* <span data-ttu-id="f997a-128">按 CTRL+F5 執行專案。</span><span class="sxs-lookup"><span data-stu-id="f997a-128">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="f997a-129">測試 [關於] 和 [連絡人] 連結。</span><span class="sxs-lookup"><span data-stu-id="f997a-129">Test the **About** and **Contact** links.</span></span>

![在 Microsoft Edge 中於 localhost 開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="f997a-131">註冊使用者</span><span class="sxs-lookup"><span data-stu-id="f997a-131">Register a user</span></span>

* <span data-ttu-id="f997a-132">選取 [註冊] 並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="f997a-132">Select **Register** and register a new user.</span></span> <span data-ttu-id="f997a-133">您可以使用虛構的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="f997a-133">You can use a fictitious email address.</span></span> <span data-ttu-id="f997a-134">提交時，頁面會顯示下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="f997a-134">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="f997a-135">「內部伺服器錯誤: 處理要求時資料庫作業失敗。SQL 例外狀況：無法開啟資料庫。為應用程式資料庫內容套用現有的移轉可能會解決此問題。」\*</span><span class="sxs-lookup"><span data-stu-id="f997a-135">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="f997a-136">選取 [套用移轉]，並在頁面更新後重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="f997a-136">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![內部伺服器錯誤：處理要求時資料庫作業失敗。](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="f997a-140">應用程式會顯示用來註冊新使用者的電子郵件和 [登出] 連結。</span><span class="sxs-lookup"><span data-stu-id="f997a-140">The app displays the email used to register the new user and a **Log out** link.</span></span>

![在 Microsoft Edge 中開啟的 Web 應用程式。](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="f997a-143">將應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="f997a-143">Deploy the app to Azure</span></span>

<span data-ttu-id="f997a-144">在方案總管中，以滑鼠右鍵按一下專案，然後選取 [發行]。</span><span class="sxs-lookup"><span data-stu-id="f997a-144">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="f997a-146">在 [發行] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="f997a-146">In the **Publish** dialog:</span></span>

* <span data-ttu-id="f997a-147">選取 [Microsoft Azure App Service]。</span><span class="sxs-lookup"><span data-stu-id="f997a-147">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="f997a-148">選取齒輪圖示，然後選取 [建立設定檔]。</span><span class="sxs-lookup"><span data-stu-id="f997a-148">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="f997a-149">選取 [建立設定檔]。</span><span class="sxs-lookup"><span data-stu-id="f997a-149">Select **Create Profile**.</span></span>

![[發行] 對話方塊](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="f997a-151">建立 Azure 資源</span><span class="sxs-lookup"><span data-stu-id="f997a-151">Create Azure resources</span></span>

<span data-ttu-id="f997a-152">出現 [建立應用程式服務] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="f997a-152">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="f997a-153">輸入訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="f997a-153">Enter your subscription.</span></span>
* <span data-ttu-id="f997a-154">會填入 [應用程式名稱]、[資源群組] 和 [App Service 方案] 輸入欄位。</span><span class="sxs-lookup"><span data-stu-id="f997a-154">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="f997a-155">您可以保留這些名稱，或變更它們。</span><span class="sxs-lookup"><span data-stu-id="f997a-155">You can keep these names or change them.</span></span>

![[應用程式服務] 對話方塊](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="f997a-157">選取 [服務] 索引標籤來建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f997a-157">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="f997a-158">選取綠色的 **+** 圖示來建立新的 SQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="f997a-158">Select the green **+** icon to create a new SQL Database</span></span>

![新增 SQL 資料庫](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="f997a-160">在 [設定 SQL 資料庫] 對話方塊上選取 [新增...] 來建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f997a-160">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![新增 SQL 資料庫與伺服器](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="f997a-162">[設定 SQL Server] 對話方塊會隨即出現。</span><span class="sxs-lookup"><span data-stu-id="f997a-162">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="f997a-163">輸入系統管理員使用者名稱與密碼，然後選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="f997a-163">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="f997a-164">您可以保留預設的**伺服器名稱**。</span><span class="sxs-lookup"><span data-stu-id="f997a-164">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="f997a-165">"admin" 不允許作為系統管理員使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="f997a-165">"admin" is not allowed as the administrator user name.</span></span>

![[設定 SQL Server] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="f997a-167">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="f997a-167">Select **OK**.</span></span>

<span data-ttu-id="f997a-168">Visual Studio 會回到 [建立 App Service] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="f997a-168">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="f997a-169">在 [建立 App Service] 對話方塊上選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f997a-169">Select **Create** on the **Create App Service** dialog.</span></span>

![[設定 SQL Database] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="f997a-171">Visual Studio 會在 Azure 上建立 Web 應用程式和 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="f997a-171">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="f997a-172">此步驟可能需要幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="f997a-172">This step can take a few minutes.</span></span> <span data-ttu-id="f997a-173">如需所建立資源的資訊，請參閱[其他資源](#additonal-resources)。</span><span class="sxs-lookup"><span data-stu-id="f997a-173">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="f997a-174">部署完成時，請選取 [設定]：</span><span class="sxs-lookup"><span data-stu-id="f997a-174">When deployment completes, select **Settings**:</span></span>

![[設定 SQL Server] 對話方塊](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="f997a-176">在 [發行] 對話方塊的 [設定] 頁面上：</span><span class="sxs-lookup"><span data-stu-id="f997a-176">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="f997a-177">展開 [資料庫] 並選取 [在執行階段使用此連接字串]。</span><span class="sxs-lookup"><span data-stu-id="f997a-177">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="f997a-178">展開 [Entity Framework 移轉] 並選取 [在發行時套用此移轉]。</span><span class="sxs-lookup"><span data-stu-id="f997a-178">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="f997a-179">選取 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="f997a-179">Select **Save**.</span></span> <span data-ttu-id="f997a-180">Visual Studio 會回到 [發行] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="f997a-180">Visual Studio returns to the **Publish** dialog.</span></span> 

![[發行] 對話方塊：設定面板](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="f997a-182">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="f997a-182">Click **Publish**.</span></span> <span data-ttu-id="f997a-183">Visual Studio 會將您的應用程式發行至 Azure。</span><span class="sxs-lookup"><span data-stu-id="f997a-183">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="f997a-184">部署完成時，會在瀏覽器中開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="f997a-184">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="f997a-185">在 Azure 中測試應用程式</span><span class="sxs-lookup"><span data-stu-id="f997a-185">Test your app in Azure</span></span>

* <span data-ttu-id="f997a-186">測試 [關於] 和 [連絡人] 連結</span><span class="sxs-lookup"><span data-stu-id="f997a-186">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="f997a-187">註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="f997a-187">Register a new user</span></span>

![在 Microsoft Edge 中於 Azure App Service 上開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="f997a-189">更新應用程式</span><span class="sxs-lookup"><span data-stu-id="f997a-189">Update the app</span></span>

* <span data-ttu-id="f997a-190">編輯 *Pages/About.cshtml* Razor 頁面，並變更其內容。</span><span class="sxs-lookup"><span data-stu-id="f997a-190">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="f997a-191">例如，您可以修改段落，使其說出 "Hello ASP.NET Core!"：[!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="f997a-191">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="f997a-192">以滑鼠右鍵按一下專案，然後再次選取 [發行...]。</span><span class="sxs-lookup"><span data-stu-id="f997a-192">Right-click on the project and select **Publish...** again.</span></span>

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="f997a-194">發行應用程式後，請驗證您進行的變更在 Azure 上有效。</span><span class="sxs-lookup"><span data-stu-id="f997a-194">After the app is published, verify the changes you made are available on Azure.</span></span>

![驗證工作是否完成](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="f997a-196">清除</span><span class="sxs-lookup"><span data-stu-id="f997a-196">Clean up</span></span>

<span data-ttu-id="f997a-197">當您完成測試應用程式時，請移至 [Azure 入口網站](https://portal.azure.com/)並刪除應用程式。</span><span class="sxs-lookup"><span data-stu-id="f997a-197">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="f997a-198">選取 [資源群組]，然後選取您建立的資源群組。</span><span class="sxs-lookup"><span data-stu-id="f997a-198">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure 入口網站：資訊看板功能表中的 [資源群組]](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="f997a-200">在 [資源群組] 頁面中，選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="f997a-200">In the **Resource groups** page, select **Delete**.</span></span>

![Azure 入口網站：[資源群組] 頁面](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="f997a-202">輸入資源群組的名稱，並選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="f997a-202">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="f997a-203">您在本教學課程中建立的應用程式和其他所有資源都會立即從 Azure 中刪除。</span><span class="sxs-lookup"><span data-stu-id="f997a-203">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="f997a-204">後續步驟</span><span class="sxs-lookup"><span data-stu-id="f997a-204">Next steps</span></span>

* [<span data-ttu-id="f997a-205">使用 Visual Studio 與 Git 持續部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="f997a-205">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="f997a-206">其他資源</span><span class="sxs-lookup"><span data-stu-id="f997a-206">Additonal resources</span></span>

* [<span data-ttu-id="f997a-207">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f997a-207">Azure App Service</span></span>](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="f997a-208">Azure 資源群組</span><span class="sxs-lookup"><span data-stu-id="f997a-208">Azure resource groups</span></span>](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="f997a-209">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="f997a-209">Azure SQL Database</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/)