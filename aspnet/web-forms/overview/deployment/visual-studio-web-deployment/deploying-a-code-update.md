---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 使用 Visual Studio 的 ASP.NET Web 部署： 部署程式碼更新 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: a3d181f3ec8db74781c550720ba4331bdf6478b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823604"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="7be7c-103">使用 Visual Studio 的 ASP.NET Web 部署： 部署程式碼更新</span><span class="sxs-lookup"><span data-stu-id="7be7c-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="7be7c-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7be7c-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="7be7c-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="7be7c-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="7be7c-106">本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="7be7c-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="7be7c-107">這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="7be7c-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="7be7c-108">總覽</span><span class="sxs-lookup"><span data-stu-id="7be7c-108">Overview</span></span>

<span data-ttu-id="7be7c-109">初始部署之後，您的工作，維護及開發您的網站會繼續，而且之前長時間中，您會想要將更新部署。</span><span class="sxs-lookup"><span data-stu-id="7be7c-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="7be7c-110">本教學課程會帶領您完成將更新部署到您的應用程式程式碼的程序。</span><span class="sxs-lookup"><span data-stu-id="7be7c-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="7be7c-111">會實作，以及在本教學課程中部署的更新並不包含資料庫變更;您會看到有什麼不一樣將在下一個教學課程中的資料庫變更部署。</span><span class="sxs-lookup"><span data-stu-id="7be7c-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="7be7c-112">提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。</span><span class="sxs-lookup"><span data-stu-id="7be7c-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="7be7c-113">變更的程式碼</span><span class="sxs-lookup"><span data-stu-id="7be7c-113">Make a code change</span></span>

<span data-ttu-id="7be7c-114">做為更新的簡單範例，用您的應用程式，您會加入至**講師**頁面上選取的講師所教授的課程清單。</span><span class="sxs-lookup"><span data-stu-id="7be7c-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="7be7c-115">如果您執行**講師**頁面上，您會發現有**選取**連結在方格中，但它們不進行以外進行的任何資料列背景開啟灰色。</span><span class="sxs-lookup"><span data-stu-id="7be7c-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![選取的講師頁面](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="7be7c-117">現在您將新增時執行的程式碼**選取**連結按一下，並顯示一份所選講師所教授的課程。</span><span class="sxs-lookup"><span data-stu-id="7be7c-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="7be7c-118">在  *Instructors.aspx*，加入下列標記的後面**ErrorMessageLabel** `Label`控制項：</span><span class="sxs-lookup"><span data-stu-id="7be7c-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="7be7c-119">執行網頁，然後選取講師。</span><span class="sxs-lookup"><span data-stu-id="7be7c-119">Run the page and select an instructor.</span></span> <span data-ttu-id="7be7c-120">您會看到一份該講師所教授的課程。</span><span class="sxs-lookup"><span data-stu-id="7be7c-120">You see a list of courses taught by that instructor.</span></span>

    ![教授課程講師頁面](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="7be7c-122">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="7be7c-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="7be7c-123">將程式碼更新部署到測試環境</span><span class="sxs-lookup"><span data-stu-id="7be7c-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="7be7c-124">您可以使用您的發行設定檔部署至測試、 預備及生產環境之前，您需要變更資料庫的發行選項。</span><span class="sxs-lookup"><span data-stu-id="7be7c-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="7be7c-125">您不再需要執行的 grant 和資料部署指令碼的成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="7be7c-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="7be7c-126">開啟**發佈 Web**精靈，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發佈**。</span><span class="sxs-lookup"><span data-stu-id="7be7c-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="7be7c-127">按一下 **測試**中程式碼剖析**設定檔**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="7be7c-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="7be7c-128">按一下 [**設定**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="7be7c-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="7be7c-129">底下**DefaultConnection**中**資料庫**區段中，清除**更新資料庫**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="7be7c-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="7be7c-130">按一下 **設定檔**索引標籤，然後再按一下**預備**中程式碼剖析**的設定檔**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="7be7c-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="7be7c-131">當詢問您是否要儲存所做的變更**測試**設定檔，請按一下 **是**。</span><span class="sxs-lookup"><span data-stu-id="7be7c-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="7be7c-132">暫存設定檔中進行相同變更。</span><span class="sxs-lookup"><span data-stu-id="7be7c-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="7be7c-133">重複的程序中的實際執行設定檔進行相同變更。</span><span class="sxs-lookup"><span data-stu-id="7be7c-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="7be7c-134">關閉**發佈 Web**精靈。</span><span class="sxs-lookup"><span data-stu-id="7be7c-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="7be7c-135">現在只需執行一種單鍵發行一次是部署到測試環境。</span><span class="sxs-lookup"><span data-stu-id="7be7c-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="7be7c-136">若要讓這個程序更快速，您可以使用**Web 單鍵發佈**工具列。</span><span class="sxs-lookup"><span data-stu-id="7be7c-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="7be7c-137">在 **檢視**功能表上，選擇**工具列**，然後選取**Web 單鍵發佈**。</span><span class="sxs-lookup"><span data-stu-id="7be7c-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="7be7c-139">在 **方案總管 中**，選取 ContosoUniversity 專案。</span><span class="sxs-lookup"><span data-stu-id="7be7c-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="7be7c-140">**Web 單鍵發佈**工具列上，選擇**測試**發行設定檔，然後按一下 **發佈 Web** （指向左、 右的箭號圖示）。</span><span class="sxs-lookup"><span data-stu-id="7be7c-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="7be7c-142">Visual Studio 會部署更新的應用程式和瀏覽器會自動開啟至首頁。</span><span class="sxs-lookup"><span data-stu-id="7be7c-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="7be7c-143">執行 Instructors 頁面，然後選取講師，以確認更新已成功部署。</span><span class="sxs-lookup"><span data-stu-id="7be7c-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="7be7c-144">您通常也進行迴歸測試 （也就是測試以確定新的變更沒有破壞任何現有功能的站台的其餘部分）。</span><span class="sxs-lookup"><span data-stu-id="7be7c-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="7be7c-145">但本教學課程中，您會略過該步驟並繼續將更新部署到預備和生產環境。</span><span class="sxs-lookup"><span data-stu-id="7be7c-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="7be7c-146">當您重新部署時，Web Deploy 會自動決定哪些檔案已變更，只複製變更檔案伺服器。</span><span class="sxs-lookup"><span data-stu-id="7be7c-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="7be7c-147">根據預設，Web Deploy 使用上次變更日期檔案以判斷哪些已變更。</span><span class="sxs-lookup"><span data-stu-id="7be7c-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="7be7c-148">某些原始檔控制系統時您不會變更檔案內容，變更檔案，甚至是日期。</span><span class="sxs-lookup"><span data-stu-id="7be7c-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="7be7c-149">在此情況下，您可能要設定 Web Deploy，用以判斷哪些檔案已變更的檔案總和檢查碼。</span><span class="sxs-lookup"><span data-stu-id="7be7c-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="7be7c-150">如需詳細資訊，請參閱 <<c0> [ 為什麼執行我的所有檔案重新部署未變更它們雖然？](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) ASP.NET 部署常見問題集。</span><span class="sxs-lookup"><span data-stu-id="7be7c-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="7be7c-151">將應用程式離線部署期間</span><span class="sxs-lookup"><span data-stu-id="7be7c-151">Take the application offline during deployment</span></span>

<span data-ttu-id="7be7c-152">您要部署的變更現在是簡單的變更至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="7be7c-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="7be7c-153">但有時候部署較大的變更，或部署程式碼和資料庫的變更，以及站台可能不正確地運作，如果使用者在完成部署之前要求的頁面。</span><span class="sxs-lookup"><span data-stu-id="7be7c-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="7be7c-154">若要防止使用者存取站台，在進行部署時，您可以使用*應用程式\_offline.htm*檔案。</span><span class="sxs-lookup"><span data-stu-id="7be7c-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="7be7c-155">當您將名為的檔案*應用程式\_offline.htm*在您的應用程式根資料夾中，IIS 會自動顯示該檔案，而不是執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7be7c-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="7be7c-156">因此若要防止存取在部署期間，您將放*應用程式\_offline.htm*根資料夾中，在執行部署程序，然後再移除*應用程式\_offline.htm*後成功部署。</span><span class="sxs-lookup"><span data-stu-id="7be7c-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="7be7c-157">您可以設定自動將預設的 Web Deploy*應用程式\_offline.htm*啟動部署時，在伺服器上檔案，並完成時移除它。</span><span class="sxs-lookup"><span data-stu-id="7be7c-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="7be7c-158">若要執行，所以您必須執行是將下列 XML 項目新增至您的發行設定檔 (.pubxml) 檔案：</span><span class="sxs-lookup"><span data-stu-id="7be7c-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="7be7c-159">在本教學課程中，您會看到如何建立及使用自訂*應用程式\_offline.htm*檔案。</span><span class="sxs-lookup"><span data-stu-id="7be7c-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="7be7c-160">使用*應用程式\_offline.htm*預備網站中不必要的因為您不需要存取預備網站的使用者。</span><span class="sxs-lookup"><span data-stu-id="7be7c-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="7be7c-161">但是，最好使用暫存的測試所有您打算在生產環境中部署的方式。</span><span class="sxs-lookup"><span data-stu-id="7be7c-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="7be7c-162">建立應用程式\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="7be7c-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="7be7c-163">在 **方案總管 中**，以滑鼠右鍵按一下方案，然後按一下**新增**，然後按一下 **新項目**。</span><span class="sxs-lookup"><span data-stu-id="7be7c-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="7be7c-164">建立**HTML 網頁**名為*應用程式\_offline.htm* (在刪除最後一個"l" *.html* Visual Studio 會建立預設的延伸模組)。</span><span class="sxs-lookup"><span data-stu-id="7be7c-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="7be7c-165">樣板的標記取代為下列標記：</span><span class="sxs-lookup"><span data-stu-id="7be7c-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="7be7c-166">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="7be7c-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="7be7c-167">複製應用程式\_offline.htm 網站的根資料夾</span><span class="sxs-lookup"><span data-stu-id="7be7c-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="7be7c-168">您可以使用任何 FTP 工具將檔案複製到網站。</span><span class="sxs-lookup"><span data-stu-id="7be7c-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="7be7c-169">[FileZilla](http://filezilla-project.org/)是熱門的 FTP 工具和螢幕擷取畫面所示。</span><span class="sxs-lookup"><span data-stu-id="7be7c-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="7be7c-170">若要使用 FTP 工具，您需要三件事： FTP URL、 使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="7be7c-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="7be7c-171">URL 會顯示在 Azure 管理入口網站的網站的儀表板頁面上，而且在中，則可以找到的使用者名稱和密碼的 FTP *.publishsettings*您稍早下載的檔案。</span><span class="sxs-lookup"><span data-stu-id="7be7c-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="7be7c-172">下列步驟示範如何取得這些值。</span><span class="sxs-lookup"><span data-stu-id="7be7c-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="7be7c-173">在 Azure 管理入口網站中，按一下**網站**索引標籤，然後按一下 預備網站。</span><span class="sxs-lookup"><span data-stu-id="7be7c-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="7be7c-174">在 **儀表板**頁面上，捲動以尋找中的 FTP 主機名稱**快速概覽**一節。</span><span class="sxs-lookup"><span data-stu-id="7be7c-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![FTP 主機名稱](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="7be7c-176">開啟預備 *.publishsettings*在記事本或其他文字編輯器中的檔案。</span><span class="sxs-lookup"><span data-stu-id="7be7c-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="7be7c-177">尋找`publishProfile`FTP 設定檔的項目。</span><span class="sxs-lookup"><span data-stu-id="7be7c-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="7be7c-178">複製`userName`和`userPWD`值。</span><span class="sxs-lookup"><span data-stu-id="7be7c-178">Copy the `userName` and `userPWD` values.</span></span>

    ![FTP 認證](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="7be7c-180">開啟您的 FTP 工具並登入 FTP URL。</span><span class="sxs-lookup"><span data-stu-id="7be7c-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="7be7c-181">複製*應用程式\_offline.htm*從解決方案資料夾 */site/wwwroot*預備網站中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="7be7c-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![複製 app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="7be7c-183">瀏覽至您的預備網站 URL。</span><span class="sxs-lookup"><span data-stu-id="7be7c-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="7be7c-184">您會看到*應用程式\_offline.htm*頁面現在會顯示而不是您的首頁。</span><span class="sxs-lookup"><span data-stu-id="7be7c-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![在瀏覽器視窗的 app_offline.htm](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="7be7c-186">您現在已準備好部署至預備環境。</span><span class="sxs-lookup"><span data-stu-id="7be7c-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="7be7c-187">將程式碼更新部署至預備和生產環境</span><span class="sxs-lookup"><span data-stu-id="7be7c-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="7be7c-188">在  **Web 單鍵發佈**工具列上，選擇**預備**發行設定檔，然後按一下 **發佈 Web**。</span><span class="sxs-lookup"><span data-stu-id="7be7c-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="7be7c-189">Visual Studio 會部署更新的應用程式和瀏覽器開啟至網站的首頁。</span><span class="sxs-lookup"><span data-stu-id="7be7c-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="7be7c-190">*應用程式\_offline.htm*檔案會顯示。</span><span class="sxs-lookup"><span data-stu-id="7be7c-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="7be7c-191">您可以測試以確認成功部署之前，必須先移除*應用程式\_offline.htm*檔案。</span><span class="sxs-lookup"><span data-stu-id="7be7c-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="7be7c-192">返回至您的 FTP 工具，並刪除**應用程式\_offline.htm**從預備網站。</span><span class="sxs-lookup"><span data-stu-id="7be7c-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="7be7c-193">在瀏覽器中，開啟 Instructors 頁面在預備網站中，然後選取講師，以確認更新已成功部署。</span><span class="sxs-lookup"><span data-stu-id="7be7c-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="7be7c-194">如您所做預備環境，請遵循相同的程序，用於生產環境。</span><span class="sxs-lookup"><span data-stu-id="7be7c-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="7be7c-195">正在檢閱變更，並部署特定的檔案</span><span class="sxs-lookup"><span data-stu-id="7be7c-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="7be7c-196">Visual Studio 2012 也讓您能夠部署個別的檔案。</span><span class="sxs-lookup"><span data-stu-id="7be7c-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="7be7c-197">選取的檔案中，您可以檢視本機版本與已部署的版本之間的差異、 將檔案部署到目的地環境中，或從目的地環境的檔案複製到本機的專案。</span><span class="sxs-lookup"><span data-stu-id="7be7c-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="7be7c-198">在本教學課程的這一節中，您會看到如何使用這些功能。</span><span class="sxs-lookup"><span data-stu-id="7be7c-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="7be7c-199">若要部署變更</span><span class="sxs-lookup"><span data-stu-id="7be7c-199">Make a change to deploy</span></span>

1. <span data-ttu-id="7be7c-200">開啟*Content/Site.css*，並尋找的區塊`body`標記。</span><span class="sxs-lookup"><span data-stu-id="7be7c-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="7be7c-201">值變更`background-color`從`#fff`至`darkblue`。</span><span class="sxs-lookup"><span data-stu-id="7be7c-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="7be7c-202">在 [發佈預覽] 視窗檢視變更</span><span class="sxs-lookup"><span data-stu-id="7be7c-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="7be7c-203">當您使用**發佈 Web**精靈來發行專案中，您可以看到變更由按兩下該檔案中的發行的移**預覽**視窗。</span><span class="sxs-lookup"><span data-stu-id="7be7c-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="7be7c-204">以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下 **發佈**。</span><span class="sxs-lookup"><span data-stu-id="7be7c-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="7be7c-205">從**設定檔**下拉式清單中，選取**測試**發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="7be7c-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="7be7c-206">按一下  **Preview**，然後按一下**開始預覽**。</span><span class="sxs-lookup"><span data-stu-id="7be7c-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="7be7c-207">在  **Preview**窗格中，按兩下**Site.css**。</span><span class="sxs-lookup"><span data-stu-id="7be7c-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![按兩下 Site.css](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="7be7c-209">**預覽變更**對話方塊會顯示預覽將會部署變更。</span><span class="sxs-lookup"><span data-stu-id="7be7c-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Site.css 預覽變更](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="7be7c-211">如果您按兩下*Web.config*檔案，**預覽變更**對話方塊會顯示您的組建的效果組態轉換及發行設定檔轉換。</span><span class="sxs-lookup"><span data-stu-id="7be7c-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="7be7c-212">此時您還沒做任何項目會導致*Web.config*檔案伺服器上用來變更，因此您會不看到任何變更。</span><span class="sxs-lookup"><span data-stu-id="7be7c-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="7be7c-213">不過，**預覽變更**視窗未正確地顯示兩項變更。</span><span class="sxs-lookup"><span data-stu-id="7be7c-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="7be7c-214">看起來將會移除兩個 XML 項目。</span><span class="sxs-lookup"><span data-stu-id="7be7c-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="7be7c-215">當您選取這些項目會新增在發行程序**Execute Code First Migrations 在應用程式啟動**Code First 的內容類別。</span><span class="sxs-lookup"><span data-stu-id="7be7c-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="7be7c-216">比較完成發行程序會新增這些項目，讓它看起來像是他們正在移除雖然它們不會移除之前。</span><span class="sxs-lookup"><span data-stu-id="7be7c-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="7be7c-217">在未來版本中，將會更正這個錯誤。</span><span class="sxs-lookup"><span data-stu-id="7be7c-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="7be7c-218">按一下 [ **關閉**]。</span><span class="sxs-lookup"><span data-stu-id="7be7c-218">Click **Close**.</span></span>
6. <span data-ttu-id="7be7c-219">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="7be7c-219">Click **Publish**.</span></span>
7. <span data-ttu-id="7be7c-220">當瀏覽器中開啟測試網站的首頁上時，按下 CTRL + F5，導致硬碟的重新整理以查看 CSS 變更的影響。</span><span class="sxs-lookup"><span data-stu-id="7be7c-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![CSS 變更的影響](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="7be7c-222">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="7be7c-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="7be7c-223">將特定的檔案，從 方案總管發佈</span><span class="sxs-lookup"><span data-stu-id="7be7c-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="7be7c-224">假設您不喜歡的藍色背景並想要還原為原始的色彩。</span><span class="sxs-lookup"><span data-stu-id="7be7c-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="7be7c-225">在本節中您會還原原始設定，藉由發行特定的檔案，直接從**方案總管 中**。</span><span class="sxs-lookup"><span data-stu-id="7be7c-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="7be7c-226">開啟*Content/Site.css* ，並還原`background-color`設為  `#fff`。</span><span class="sxs-lookup"><span data-stu-id="7be7c-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="7be7c-227">在 **方案總管**，以滑鼠右鍵按一下*Content/Site.css*檔案。</span><span class="sxs-lookup"><span data-stu-id="7be7c-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="7be7c-228">操作功能表會顯示三個發行選項。</span><span class="sxs-lookup"><span data-stu-id="7be7c-228">The context menu shows three publish options.</span></span>

    ![發行選項，從 方案總管](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="7be7c-230">按一下 **預覽變更為 Site.css**。</span><span class="sxs-lookup"><span data-stu-id="7be7c-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="7be7c-231">視窗會開啟顯示目的地環境中的本機檔案和其版本之間的差異。</span><span class="sxs-lookup"><span data-stu-id="7be7c-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="7be7c-233">在**方案總管 中**，以滑鼠右鍵按一下**Site.css**再按一下**發行 Site.css**。</span><span class="sxs-lookup"><span data-stu-id="7be7c-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="7be7c-234">**Web 發行活動**視窗會顯示已發行的檔案。</span><span class="sxs-lookup"><span data-stu-id="7be7c-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Web 發行活動時段](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="7be7c-236">開啟瀏覽器並前往`http://localhost/contosouniversity`URL] 和 [重新整理以查看變更的 CSS 效果然後按 CTRL + F5 鍵會造成一種硬式。</span><span class="sxs-lookup"><span data-stu-id="7be7c-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![使用一般的 CSS 的首頁](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="7be7c-238">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="7be7c-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="7be7c-239">總結</span><span class="sxs-lookup"><span data-stu-id="7be7c-239">Summary</span></span>

<span data-ttu-id="7be7c-240">您現在已了解幾種方式部署應用程式更新未牽涉到資料庫變更，以及您已了解如何預覽來確認更新項目是您所預期的變更。</span><span class="sxs-lookup"><span data-stu-id="7be7c-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="7be7c-241">Instructors 頁面現在具有**教授課程**一節。</span><span class="sxs-lookup"><span data-stu-id="7be7c-241">The Instructors page now has a **Courses Taught** section.</span></span>

![教授課程講師頁面](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="7be7c-243">下一個教學課程會示範如何將資料庫變更部署： 對資料庫和 Instructors 頁面，您將新增出生日期欄位。</span><span class="sxs-lookup"><span data-stu-id="7be7c-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7be7c-244">[上一頁](deploying-to-production.md)
> [下一頁](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="7be7c-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
