---
title: "使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 6f697ed4d8876a19cd058533e4f6a5d4f7cdc2fb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="24923-103">使用 Visual Studio 將 ASP.NET Core Web 應用程式發行到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="24923-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="24923-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Cesar Blum Silveira](https://github.com/cesarbs) 及 [Rachel Appel](https://twitter.com/rachelappel) 共同編纂</span><span class="sxs-lookup"><span data-stu-id="24923-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="24923-105">如果您是使用 Mac，請參閱[從 Visual Studio for Mac 發佈至 Azure](https://blog.xamarin.com/publish-azure-visual-studio-mac/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="24923-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="24923-106">設定</span><span class="sxs-lookup"><span data-stu-id="24923-106">Set up</span></span>

* <span data-ttu-id="24923-107">如果沒有帳戶，請開啟[免費的 Azure 帳戶](https://aka.ms/K5y5yh)。</span><span class="sxs-lookup"><span data-stu-id="24923-107">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="24923-108">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="24923-108">Create a web app</span></span>

<span data-ttu-id="24923-109">在 Visual Studio 起始畫面中，選取 [檔案] > [新增] > [專案...]</span><span class="sxs-lookup"><span data-stu-id="24923-109">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![檔案功能表](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="24923-111">完成 [新增專案] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="24923-111">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="24923-112">在左窗格中，選取 [.NET Core]。</span><span class="sxs-lookup"><span data-stu-id="24923-112">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="24923-113">在中央窗格中，選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="24923-113">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="24923-114">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="24923-114">Select **OK**.</span></span>

![[新增專案] 對話](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="24923-116">在 [新增 ASP.NET Core Web 應用程式] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="24923-116">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="24923-117">選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="24923-117">Select **Web Application**.</span></span>

* <span data-ttu-id="24923-118">選取 [變更驗證]。</span><span class="sxs-lookup"><span data-stu-id="24923-118">Select **Change Authentication**.</span></span>

![[新增專案] 對話](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="24923-120">[變更驗證] 對話方塊會隨即出現。</span><span class="sxs-lookup"><span data-stu-id="24923-120">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="24923-121">選取 [個別使用者帳戶]。</span><span class="sxs-lookup"><span data-stu-id="24923-121">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="24923-122">選取 [確定] 返回 [新增 ASP.NET Core Web 應用程式]，然後再次選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="24923-122">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![[新增 ASP.NET Core Web 驗證] 對話方塊](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="24923-124">Visual Studio 會建立解決方案。</span><span class="sxs-lookup"><span data-stu-id="24923-124">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="24923-125">在本機執行應用程式</span><span class="sxs-lookup"><span data-stu-id="24923-125">Run the app locally</span></span>

* <span data-ttu-id="24923-126">選擇 [偵錯]，然後 [啟動但不偵錯] 以在本機執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="24923-126">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="24923-127">按一下 [關於] 與 [連絡資訊] 連結，以驗證 Web 應用程式能夠運作。</span><span class="sxs-lookup"><span data-stu-id="24923-127">Click the **About** and **Contact** links to verify the web application works.</span></span>

![在 Microsoft Edge 中於 localhost 開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="24923-129">選取 [註冊] 並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="24923-129">Select **Register** and register a new user.</span></span> <span data-ttu-id="24923-130">您可以使用虛構的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="24923-130">You can use a fictitious email address.</span></span> <span data-ttu-id="24923-131">提交時，頁面會顯示下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="24923-131">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="24923-132">「內部伺服器錯誤: 處理要求時資料庫作業失敗。SQL 例外狀況：無法開啟資料庫。為應用程式資料庫內容套用現有的移轉可能會解決此問題。」</span><span class="sxs-lookup"><span data-stu-id="24923-132">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="24923-133">選取 [套用移轉]，並在頁面更新後重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="24923-133">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![內部伺服器錯誤：處理要求時資料庫作業失敗。](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="24923-137">應用程式會顯示用來註冊新使用者的電子郵件和 [登出] 連結。</span><span class="sxs-lookup"><span data-stu-id="24923-137">The app displays the email used to register the new user and a **Log out** link.</span></span>

![在 Microsoft Edge 中開啟的 Web 應用程式。](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="24923-140">將應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="24923-140">Deploy the app to Azure</span></span>

<span data-ttu-id="24923-141">關閉網頁，回到 Visual Studio 並在 [偵錯] 功能表選取 [停止偵錯]。</span><span class="sxs-lookup"><span data-stu-id="24923-141">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="24923-142">在方案總管中，以滑鼠右鍵按一下專案，然後選取 [發行]。</span><span class="sxs-lookup"><span data-stu-id="24923-142">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="24923-144">在 [發行] 對話方塊中，選取 [Microsoft Azure App Service]，然後按一下 [發行]。</span><span class="sxs-lookup"><span data-stu-id="24923-144">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![[發行] 對話方塊](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="24923-146">為應用程式提供唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="24923-146">Name the app a unique name.</span></span> 

* <span data-ttu-id="24923-147">選取訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="24923-147">Select a subscription.</span></span>

* <span data-ttu-id="24923-148">對資源群組選取 [新增...]，並輸入新資源群組的名稱。</span><span class="sxs-lookup"><span data-stu-id="24923-148">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="24923-149">對應用程式服務方案選取 [新增...]，並選取您附近的位置。</span><span class="sxs-lookup"><span data-stu-id="24923-149">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="24923-150">您可以保留預設產生的名稱。</span><span class="sxs-lookup"><span data-stu-id="24923-150">You can keep the name that is generated by default.</span></span>

![[應用程式服務] 對話方塊](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="24923-152">選取 [服務] 索引標籤來建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="24923-152">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="24923-153">選取綠色的 **+** 圖示來建立新的 SQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="24923-153">Select the green **+** icon to create a new SQL Database</span></span>

![新增 SQL 資料庫](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="24923-155">在 [設定 SQL 資料庫] 對話方塊上選取 [新增...] 來建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="24923-155">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![新增 SQL 資料庫與伺服器](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="24923-157">[設定 SQL Server] 對話方塊會隨即出現。</span><span class="sxs-lookup"><span data-stu-id="24923-157">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="24923-158">輸入系統管理員使用者名稱與密碼，然後選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="24923-158">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="24923-159">請勿忘記您在此步驟中建立的使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="24923-159">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="24923-160">您可以保留預設的**伺服器名稱**。</span><span class="sxs-lookup"><span data-stu-id="24923-160">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="24923-161">輸入資料庫與連接字串的名稱。</span><span class="sxs-lookup"><span data-stu-id="24923-161">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="24923-162">"admin" 不允許作為系統管理員使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="24923-162">"admin" is not allowed as the administrator user name.</span></span>

![[設定 SQL Server] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="24923-164">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="24923-164">Select **OK**.</span></span>

<span data-ttu-id="24923-165">Visual Studio 會回到 [建立 App Service] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="24923-165">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="24923-166">在 [建立 App Service] 對話方塊上選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="24923-166">Select **Create** on the **Create App Service** dialog.</span></span>

![[設定 SQL Database] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="24923-168">在 [發行] 對話方塊中按一下 [設定] 連結。</span><span class="sxs-lookup"><span data-stu-id="24923-168">Click the **Settings** link in the **Publish** dialog.</span></span>

![[發行] 對話方塊：連線面板](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="24923-170">在 [發行] 對話方塊的 [設定] 頁面上：</span><span class="sxs-lookup"><span data-stu-id="24923-170">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="24923-171">展開 [資料庫] 並選取 [在執行階段使用此連接字串]。</span><span class="sxs-lookup"><span data-stu-id="24923-171">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="24923-172">展開 [Entity Framework 移轉] 並選取 [在發行時套用此移轉]。</span><span class="sxs-lookup"><span data-stu-id="24923-172">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="24923-173">選取 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="24923-173">Select **Save**.</span></span> <span data-ttu-id="24923-174">Visual Studio 會回到 [發行] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="24923-174">Visual Studio returns to the **Publish** dialog.</span></span> 

![[發行] 對話方塊：設定面板](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="24923-176">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="24923-176">Click **Publish**.</span></span> <span data-ttu-id="24923-177">Visual Studio 會將您的應用程式發行至 Azure，並在您的瀏覽器中啟動雲端應用程式。</span><span class="sxs-lookup"><span data-stu-id="24923-177">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="24923-178">在 Azure 中測試應用程式</span><span class="sxs-lookup"><span data-stu-id="24923-178">Test your app in Azure</span></span>

* <span data-ttu-id="24923-179">測試 [關於] 和 [連絡人] 連結</span><span class="sxs-lookup"><span data-stu-id="24923-179">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="24923-180">註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="24923-180">Register a new user</span></span>

![在 Microsoft Edge 中於 Azure App Service 上開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="24923-182">更新應用程式</span><span class="sxs-lookup"><span data-stu-id="24923-182">Update the app</span></span>

* <span data-ttu-id="24923-183">編輯 *Pages/About.cshtml* Razor 頁面，並變更其內容。</span><span class="sxs-lookup"><span data-stu-id="24923-183">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="24923-184">例如，您可以修改段落使其說出 "Hello ASP.NET Core!"：</span><span class="sxs-lookup"><span data-stu-id="24923-184">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="24923-185">以滑鼠右鍵按一下專案，然後再次選取 [發行...]。</span><span class="sxs-lookup"><span data-stu-id="24923-185">Right-click on the project and select **Publish...** again.</span></span>

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="24923-187">發行應用程式後，請驗證您進行的變更在 Azure 上有效。</span><span class="sxs-lookup"><span data-stu-id="24923-187">After the app is published, verify the changes you made are available on Azure.</span></span>

![驗證工作是否完成](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="24923-189">清除</span><span class="sxs-lookup"><span data-stu-id="24923-189">Clean up</span></span>

<span data-ttu-id="24923-190">當您完成測試應用程式時，請移至 [Azure 入口網站](https://portal.azure.com/)並刪除應用程式。</span><span class="sxs-lookup"><span data-stu-id="24923-190">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="24923-191">選取 [資源群組]，然後選取您建立的資源群組。</span><span class="sxs-lookup"><span data-stu-id="24923-191">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure 入口網站：資訊看板功能表中的 [資源群組]](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="24923-193">在 [資源群組] 頁面中，選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="24923-193">In the **Resource groups** page, select **Delete**.</span></span>

![Azure 入口網站：[資源群組] 頁面](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="24923-195">輸入資源群組的名稱，並選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="24923-195">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="24923-196">您在本教學課程中建立的應用程式和其他所有資源都會立即從 Azure 中刪除。</span><span class="sxs-lookup"><span data-stu-id="24923-196">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="24923-197">後續步驟</span><span class="sxs-lookup"><span data-stu-id="24923-197">Next steps</span></span>

* [<span data-ttu-id="24923-198">使用 Visual Studio 與 Git 持續部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="24923-198">Continuous Deployment to Azure with Visual Studio and Git</span></span>](../publishing/azure-continuous-deployment.md)
