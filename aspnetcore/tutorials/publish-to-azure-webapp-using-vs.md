---
title: "使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/16/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: fdd439a5e4fd41e72fd7a3c602183d4a4e9e0aa5
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2017
---
<span data-ttu-id="46a60-103">/zh-TW</span><span class="sxs-lookup"><span data-stu-id="46a60-103">/en-us</span></span>

# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="46a60-104">使用 Visual Studio 將 ASP.NET Core Web 應用程式發行到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="46a60-104">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="46a60-105">由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Cesar Blum Silveira](https://github.com/cesarbs) 及 [Rachel Appel](https://twitter.com/rachelappel) 共同編纂</span><span class="sxs-lookup"><span data-stu-id="46a60-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="46a60-106">如果您是使用 Mac，請參閱[從 Visual Studio for Mac 發佈至 Azure](https://blog.xamarin.com/publish-azure-visual-studio-mac/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="46a60-106">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="46a60-107">設定</span><span class="sxs-lookup"><span data-stu-id="46a60-107">Set up</span></span>

* <span data-ttu-id="46a60-108">如果沒有帳戶，請開啟[免費的 Azure 帳戶](https://aka.ms/K5y5yh)。</span><span class="sxs-lookup"><span data-stu-id="46a60-108">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="46a60-109">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="46a60-109">Create a web app</span></span>

<span data-ttu-id="46a60-110">在 Visual Studio 起始畫面中，選取 [檔案] > [新增] > [專案...]</span><span class="sxs-lookup"><span data-stu-id="46a60-110">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![檔案功能表](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="46a60-112">完成 [新增專案] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="46a60-112">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="46a60-113">在左窗格中，選取 [.NET Core]。</span><span class="sxs-lookup"><span data-stu-id="46a60-113">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="46a60-114">在中央窗格中，選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="46a60-114">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="46a60-115">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="46a60-115">Select **OK**.</span></span>

![[新增專案] 對話](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="46a60-117">在 [新增 ASP.NET Core Web 應用程式] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="46a60-117">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="46a60-118">選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="46a60-118">Select **Web Application**.</span></span>
* <span data-ttu-id="46a60-119">選取 [變更驗證]。</span><span class="sxs-lookup"><span data-stu-id="46a60-119">Select **Change Authentication**.</span></span>

![[新增專案] 對話](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="46a60-121">[變更驗證] 對話方塊會隨即出現。</span><span class="sxs-lookup"><span data-stu-id="46a60-121">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="46a60-122">選取 [個別使用者帳戶]。</span><span class="sxs-lookup"><span data-stu-id="46a60-122">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="46a60-123">選取 [確定] 返回 [新增 ASP.NET Core Web 應用程式]，然後再次選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="46a60-123">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![[新增 ASP.NET Core Web 驗證] 對話方塊](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="46a60-125">Visual Studio 會建立解決方案。</span><span class="sxs-lookup"><span data-stu-id="46a60-125">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="46a60-126">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="46a60-126">Run the app</span></span>

* <span data-ttu-id="46a60-127">按 CTRL+F5 執行專案。</span><span class="sxs-lookup"><span data-stu-id="46a60-127">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="46a60-128">測試 [關於] 和 [連絡人] 連結。</span><span class="sxs-lookup"><span data-stu-id="46a60-128">Test the **About** and **Contact** links.</span></span>

![在 Microsoft Edge 中於 localhost 開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="46a60-130">註冊使用者</span><span class="sxs-lookup"><span data-stu-id="46a60-130">Register a user</span></span>

* <span data-ttu-id="46a60-131">選取 [註冊] 並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="46a60-131">Select **Register** and register a new user.</span></span> <span data-ttu-id="46a60-132">您可以使用虛構的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="46a60-132">You can use a fictitious email address.</span></span> <span data-ttu-id="46a60-133">提交時，頁面會顯示下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="46a60-133">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="46a60-134">「內部伺服器錯誤: 處理要求時資料庫作業失敗。SQL 例外狀況：無法開啟資料庫。為應用程式資料庫內容套用現有的移轉可能會解決此問題。」</span><span class="sxs-lookup"><span data-stu-id="46a60-134">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="46a60-135">選取 [套用移轉]，並在頁面更新後重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="46a60-135">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![內部伺服器錯誤：處理要求時資料庫作業失敗。](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="46a60-139">應用程式會顯示用來註冊新使用者的電子郵件和 [登出] 連結。</span><span class="sxs-lookup"><span data-stu-id="46a60-139">The app displays the email used to register the new user and a **Log out** link.</span></span>

![在 Microsoft Edge 中開啟的 Web 應用程式。](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="46a60-142">將應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="46a60-142">Deploy the app to Azure</span></span>

<span data-ttu-id="46a60-143">在方案總管中，以滑鼠右鍵按一下專案，然後選取 [發行]。</span><span class="sxs-lookup"><span data-stu-id="46a60-143">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="46a60-145">在 [發行] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="46a60-145">In the **Publish** dialog:</span></span>

* <span data-ttu-id="46a60-146">選取 [Microsoft Azure App Service]。</span><span class="sxs-lookup"><span data-stu-id="46a60-146">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="46a60-147">選取齒輪圖示，然後選取 [建立設定檔]。</span><span class="sxs-lookup"><span data-stu-id="46a60-147">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="46a60-148">選取 [建立設定檔]。</span><span class="sxs-lookup"><span data-stu-id="46a60-148">Select **Create Profile**.</span></span>

![[發行] 對話方塊](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="46a60-150">建立 Azure 資源</span><span class="sxs-lookup"><span data-stu-id="46a60-150">Create Azure resources</span></span>

<span data-ttu-id="46a60-151">出現 [建立應用程式服務] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="46a60-151">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="46a60-152">輸入訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="46a60-152">Enter your subscription.</span></span>
* <span data-ttu-id="46a60-153">會填入 [應用程式名稱]、[資源群組] 和 [App Service 方案] 輸入欄位。</span><span class="sxs-lookup"><span data-stu-id="46a60-153">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="46a60-154">您可以保留這些名稱，或變更它們。</span><span class="sxs-lookup"><span data-stu-id="46a60-154">You can keep these names or change them.</span></span>

![[應用程式服務] 對話方塊](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="46a60-156">選取 [服務] 索引標籤來建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="46a60-156">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="46a60-157">選取綠色的 **+** 圖示來建立新的 SQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="46a60-157">Select the green **+** icon to create a new SQL Database</span></span>

![新增 SQL 資料庫](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="46a60-159">在 [設定 SQL 資料庫] 對話方塊上選取 [新增...] 來建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="46a60-159">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![新增 SQL 資料庫與伺服器](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="46a60-161">[設定 SQL Server] 對話方塊會隨即出現。</span><span class="sxs-lookup"><span data-stu-id="46a60-161">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="46a60-162">輸入系統管理員使用者名稱與密碼，然後選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="46a60-162">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="46a60-163">您可以保留預設的**伺服器名稱**。</span><span class="sxs-lookup"><span data-stu-id="46a60-163">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="46a60-164">"admin" 不允許作為系統管理員使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="46a60-164">"admin" is not allowed as the administrator user name.</span></span>

![[設定 SQL Server] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="46a60-166">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="46a60-166">Select **OK**.</span></span>

<span data-ttu-id="46a60-167">Visual Studio 會回到 [建立 App Service] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="46a60-167">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="46a60-168">在 [建立 App Service] 對話方塊上選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="46a60-168">Select **Create** on the **Create App Service** dialog.</span></span>

![[設定 SQL Database] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="46a60-170">Visual Studio 會在 Azure 上建立 Web 應用程式和 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="46a60-170">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="46a60-171">此步驟可能需要幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="46a60-171">This step can take a few minutes.</span></span> <span data-ttu-id="46a60-172">如需所建立資源的資訊，請參閱[其他資源](#additonal-resources)。</span><span class="sxs-lookup"><span data-stu-id="46a60-172">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="46a60-173">部署完成時，請選取 [設定]：</span><span class="sxs-lookup"><span data-stu-id="46a60-173">When deployment completes, select **Settings**:</span></span>

![[設定 SQL Server] 對話方塊](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="46a60-175">在 [發行] 對話方塊的 [設定] 頁面上：</span><span class="sxs-lookup"><span data-stu-id="46a60-175">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="46a60-176">展開 [資料庫] 並選取 [在執行階段使用此連接字串]。</span><span class="sxs-lookup"><span data-stu-id="46a60-176">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="46a60-177">展開 [Entity Framework 移轉] 並選取 [在發行時套用此移轉]。</span><span class="sxs-lookup"><span data-stu-id="46a60-177">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="46a60-178">選取 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="46a60-178">Select **Save**.</span></span> <span data-ttu-id="46a60-179">Visual Studio 會回到 [發行] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="46a60-179">Visual Studio returns to the **Publish** dialog.</span></span> 

![[發行] 對話方塊：設定面板](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="46a60-181">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="46a60-181">Click **Publish**.</span></span> <span data-ttu-id="46a60-182">Visual Studio 會將您的應用程式發行至 Azure。</span><span class="sxs-lookup"><span data-stu-id="46a60-182">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="46a60-183">部署完成時，會在瀏覽器中開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="46a60-183">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="46a60-184">在 Azure 中測試應用程式</span><span class="sxs-lookup"><span data-stu-id="46a60-184">Test your app in Azure</span></span>

* <span data-ttu-id="46a60-185">測試 [關於] 和 [連絡人] 連結</span><span class="sxs-lookup"><span data-stu-id="46a60-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="46a60-186">註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="46a60-186">Register a new user</span></span>

![在 Microsoft Edge 中於 Azure App Service 上開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="46a60-188">更新應用程式</span><span class="sxs-lookup"><span data-stu-id="46a60-188">Update the app</span></span>

* <span data-ttu-id="46a60-189">編輯 *Pages/About.cshtml* Razor 頁面，並變更其內容。</span><span class="sxs-lookup"><span data-stu-id="46a60-189">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="46a60-190">例如，您可以修改段落，使其說出 "Hello ASP.NET Core!"：[!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="46a60-190">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="46a60-191">以滑鼠右鍵按一下專案，然後再次選取 [發行...]。</span><span class="sxs-lookup"><span data-stu-id="46a60-191">Right-click on the project and select **Publish...** again.</span></span>

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="46a60-193">發行應用程式後，請驗證您進行的變更在 Azure 上有效。</span><span class="sxs-lookup"><span data-stu-id="46a60-193">After the app is published, verify the changes you made are available on Azure.</span></span>

![驗證工作是否完成](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="46a60-195">清除</span><span class="sxs-lookup"><span data-stu-id="46a60-195">Clean up</span></span>

<span data-ttu-id="46a60-196">當您完成測試應用程式時，請移至 [Azure 入口網站](https://portal.azure.com/)並刪除應用程式。</span><span class="sxs-lookup"><span data-stu-id="46a60-196">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="46a60-197">選取 [資源群組]，然後選取您建立的資源群組。</span><span class="sxs-lookup"><span data-stu-id="46a60-197">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure 入口網站：資訊看板功能表中的 [資源群組]](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="46a60-199">在 [資源群組] 頁面中，選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="46a60-199">In the **Resource groups** page, select **Delete**.</span></span>

![Azure 入口網站：[資源群組] 頁面](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="46a60-201">輸入資源群組的名稱，並選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="46a60-201">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="46a60-202">您在本教學課程中建立的應用程式和其他所有資源都會立即從 Azure 中刪除。</span><span class="sxs-lookup"><span data-stu-id="46a60-202">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="46a60-203">後續步驟</span><span class="sxs-lookup"><span data-stu-id="46a60-203">Next steps</span></span>

* [<span data-ttu-id="46a60-204">使用 Visual Studio 與 Git 持續部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="46a60-204">Continuous Deployment to Azure with Visual Studio and Git</span></span>](../publishing/azure-continuous-deployment.md)


## <a name="additonal-resources"></a><span data-ttu-id="46a60-205">其他資源</span><span class="sxs-lookup"><span data-stu-id="46a60-205">Additonal resources</span></span>

* [<span data-ttu-id="46a60-206">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="46a60-206">Azure App Service</span></span>](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="46a60-207">Azure 資源群組</span><span class="sxs-lookup"><span data-stu-id="46a60-207">Azure resource groups</span></span>](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="46a60-208">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="46a60-208">Azure SQL Database</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/)