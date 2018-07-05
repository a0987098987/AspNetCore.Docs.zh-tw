---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 Code-Only 更新-12 個 8 |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 16a9e48034536964d526717ecde2a9b813818963
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816080"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a><span data-ttu-id="8f512-103">使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 Code-Only 更新-8 12</span><span class="sxs-lookup"><span data-stu-id="8f512-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a Code-Only Update - 8 of 12</span></span>
====================
<span data-ttu-id="8f512-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8f512-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="8f512-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="8f512-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="8f512-106">這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。</span><span class="sxs-lookup"><span data-stu-id="8f512-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="8f512-107">如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="8f512-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="8f512-108">在數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。</span><span class="sxs-lookup"><span data-stu-id="8f512-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="8f512-109">顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Azure App Service Web Apps 的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="8f512-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="8f512-110">總覽</span><span class="sxs-lookup"><span data-stu-id="8f512-110">Overview</span></span>

<span data-ttu-id="8f512-111">初始部署之後，您的工作，維護及開發您的網站會繼續，而且之前長時間中，您會想要將更新部署。</span><span class="sxs-lookup"><span data-stu-id="8f512-111">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="8f512-112">本教學課程會帶領您完成將更新部署到您的應用程式程式碼的程序。</span><span class="sxs-lookup"><span data-stu-id="8f512-112">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="8f512-113">此更新未牽涉到資料庫變更;您會看到有什麼不一樣將在下一個教學課程中的資料庫變更部署。</span><span class="sxs-lookup"><span data-stu-id="8f512-113">This update does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="8f512-114">提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。</span><span class="sxs-lookup"><span data-stu-id="8f512-114">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="making-a-code-change"></a><span data-ttu-id="8f512-115">進行程式碼變更</span><span class="sxs-lookup"><span data-stu-id="8f512-115">Making a Code Change</span></span>

<span data-ttu-id="8f512-116">做為更新的簡單範例，用您的應用程式，您會加入至**講師**頁面上選取的講師所教授的課程清單。</span><span class="sxs-lookup"><span data-stu-id="8f512-116">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="8f512-117">如果您執行**講師**頁面上，您會發現有**選取**連結在方格中，但它們不進行以外進行的任何資料列背景開啟灰色。</span><span class="sxs-lookup"><span data-stu-id="8f512-117">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

<span data-ttu-id="8f512-118">[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8f512-118">[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)</span></span>

<span data-ttu-id="8f512-119">現在您將新增時執行的程式碼**選取**連結按一下，並顯示一份所選講師所教授的課程。</span><span class="sxs-lookup"><span data-stu-id="8f512-119">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

<span data-ttu-id="8f512-120">在  *Instructors.aspx*，加入下列標記的後面**ErrorMessageLabel** `Label`控制項：</span><span class="sxs-lookup"><span data-stu-id="8f512-120">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

<span data-ttu-id="8f512-121">執行網頁，然後選取講師。</span><span class="sxs-lookup"><span data-stu-id="8f512-121">Run the page and select an instructor.</span></span> <span data-ttu-id="8f512-122">您會看到一份該講師所教授的課程。</span><span class="sxs-lookup"><span data-stu-id="8f512-122">You see a list of courses taught by that instructor.</span></span>

<span data-ttu-id="8f512-123">[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="8f512-123">[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-code-update-to-the-test-environment"></a><span data-ttu-id="8f512-124">程式碼將更新部署至測試環境</span><span class="sxs-lookup"><span data-stu-id="8f512-124">Deploying the Code Update to the Test Environment</span></span>

<span data-ttu-id="8f512-125">部署到測試環境是只需執行一種單鍵發行一次。</span><span class="sxs-lookup"><span data-stu-id="8f512-125">Deploying to the test environment is a simple matter of running one-click publish again.</span></span> <span data-ttu-id="8f512-126">若要讓這個程序更快速，您可以使用**Web 單鍵發佈**工具列。</span><span class="sxs-lookup"><span data-stu-id="8f512-126">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

<span data-ttu-id="8f512-127">在 **檢視**功能表上，選擇**工具列**，然後選取**Web 單鍵發佈**。</span><span class="sxs-lookup"><span data-stu-id="8f512-127">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

<span data-ttu-id="8f512-129">在 **方案總管 中**，選取 ContosoUniversity 專案。</span><span class="sxs-lookup"><span data-stu-id="8f512-129">In **Solution Explorer**, select the ContosoUniversity project.</span></span>

<span data-ttu-id="8f512-130">**Web 單鍵發佈**工具列上，選擇**測試**發行設定檔，然後按一下 **發佈 Web** （指向左、 右的箭號圖示）。</span><span class="sxs-lookup"><span data-stu-id="8f512-130">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

<span data-ttu-id="8f512-132">Visual Studio 會部署更新的應用程式和瀏覽器會自動開啟至首頁。</span><span class="sxs-lookup"><span data-stu-id="8f512-132">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span> <span data-ttu-id="8f512-133">執行 Instructors 頁面，然後選取講師，以確認更新已成功部署。</span><span class="sxs-lookup"><span data-stu-id="8f512-133">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="8f512-134">[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="8f512-134">[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)</span></span>

<span data-ttu-id="8f512-135">您通常也進行迴歸測試 （也就是測試以確定新的變更沒有破壞任何現有功能的站台的其餘部分）。</span><span class="sxs-lookup"><span data-stu-id="8f512-135">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="8f512-136">但本教學課程中，您會略過該步驟並繼續將更新部署到生產環境。</span><span class="sxs-lookup"><span data-stu-id="8f512-136">But for this tutorial you'll skip that step and proceed to deploy the update to production.</span></span>

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a><span data-ttu-id="8f512-137">防止重新部署到生產環境的初始資料庫狀態</span><span class="sxs-lookup"><span data-stu-id="8f512-137">Preventing Redeployment of the Initial Database State to Production</span></span>

<span data-ttu-id="8f512-138">在實際的應用程式中，您初始部署之後，您的生產網站與使用者互動，資料庫會以填入即時資料。</span><span class="sxs-lookup"><span data-stu-id="8f512-138">In a real application, users interact with your production site after your initial deployment, and the databases are populated with live data.</span></span> <span data-ttu-id="8f512-139">因此，您不打算重新部署其初始狀態，會清除所有的即時資料中的成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="8f512-139">Therefore, you don't want to redeploy the membership database in its initial state, which would wipe out all of the live data.</span></span> <span data-ttu-id="8f512-140">因為 SQL Server Compact 資料庫中的檔案*應用程式\_資料*資料夾中，您必須防止變更部署設定，因此，中的檔案*應用程式\_資料*資料夾無法部署。</span><span class="sxs-lookup"><span data-stu-id="8f512-140">Since SQL Server Compact databases are files in the *App\_Data* folder, you have to prevent this by changing deployment settings so that files in the *App\_Data* folder aren't deployed.</span></span>

<span data-ttu-id="8f512-141">開啟**專案屬性**ContosoUniversity 專案，然後選取視窗**封裝/發行 Web**  索引標籤。請確定**組態**下拉式方塊已**作用 （發行）** 或**版本**選取，請選取**檔案排除應用程式\_Data 資料夾**。</span><span class="sxs-lookup"><span data-stu-id="8f512-141">Open the **Project Properties** window for the ContosoUniversity project, and select the **Package/Publish Web** tab. Make sure that the **Configuration** drop-down box has either **Active (Release)** or **Release** selected, select **Exclude files from the App\_Data folder**.</span></span>

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

<span data-ttu-id="8f512-143">如果您決定要在未來部署偵錯組建，最好在其中進行相同的偵錯組建組態變更： 變更**組態**要**偵錯**，然後選取**排除從應用程式的檔案\_Data 資料夾**。</span><span class="sxs-lookup"><span data-stu-id="8f512-143">In case you decide to deploy a debug build in the future, it's a good idea to make the same change for the Debug build configuration: change **Configuration** to **Debug** and then select **Exclude files from the App\_Data folder**.</span></span>

<span data-ttu-id="8f512-144">儲存並關閉**封裝/發行 Web**  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="8f512-144">Save and close the **Package/Publish Web** tab.</span></span>

> [!NOTE] 
> 
> [!IMPORTANT]
> <span data-ttu-id="8f512-145">請確定您沒有**移除目的地上的其他檔案**選取您的發行設定檔中。</span><span class="sxs-lookup"><span data-stu-id="8f512-145">Make sure that you don't have **Remove additional files at destination** selected in your publish profiles.</span></span> <span data-ttu-id="8f512-146">如果您選取該選項時，部署程序會刪除您在應用程式的資料庫\_中的已部署的站台，而且資料將會刪除應用程式\_資料資料夾本身。</span><span class="sxs-lookup"><span data-stu-id="8f512-146">If you select that option, the deployment process will delete the databases that you have in App\_Data in the deployed site, and it will delete the App\_Data folder itself.</span></span>


## <a name="preventing-user-access-to-the-production-site-during-update"></a><span data-ttu-id="8f512-147">防止使用者存取到生產網站，在更新期間</span><span class="sxs-lookup"><span data-stu-id="8f512-147">Preventing User Access to the Production Site During Update</span></span>

<span data-ttu-id="8f512-148">您要部署的變更現在是簡單的變更至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="8f512-148">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="8f512-149">但有時候您部署較大的變更，和在此情況下站台可以行為怪異如果使用者在完成部署之前要求的頁面。</span><span class="sxs-lookup"><span data-stu-id="8f512-149">But sometimes you deploy larger changes, and in that case the site can behave strangely if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="8f512-150">若要避免這個問題，您可以使用*應用程式\_offline.htm*檔案。</span><span class="sxs-lookup"><span data-stu-id="8f512-150">To prevent this, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="8f512-151">當您將名為的檔案*應用程式\_offline.htm*在您的應用程式根資料夾中，IIS 會自動顯示該檔案，而不是執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8f512-151">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="8f512-152">因此若要防止存取在部署期間，您將放*應用程式\_offline.htm*根資料夾中，在執行部署程序，然後再移除*應用程式\_offline.htm*。</span><span class="sxs-lookup"><span data-stu-id="8f512-152">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm*.</span></span>

<span data-ttu-id="8f512-153">在 **方案總管**，以滑鼠右鍵按一下方案 （不是其中一個專案），然後選取**新的方案資料夾**。</span><span class="sxs-lookup"><span data-stu-id="8f512-153">In **Solution Explorer**, right-click the solution (not one of the projects) and select **New Solution Folder**.</span></span>

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

<span data-ttu-id="8f512-155">將資料夾命名*SolutionFiles*。</span><span class="sxs-lookup"><span data-stu-id="8f512-155">Name the folder *SolutionFiles*.</span></span>

<span data-ttu-id="8f512-156">新的資料夾中建立名為 HTML 網頁*應用程式\_offline.htm*。</span><span class="sxs-lookup"><span data-stu-id="8f512-156">In the new folder create an HTML page named *app\_offline.htm*.</span></span> <span data-ttu-id="8f512-157">以下列標記取代現有的內容：</span><span class="sxs-lookup"><span data-stu-id="8f512-157">Replace the existing contents with the following markup:</span></span>

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

<span data-ttu-id="8f512-158">您可以複製*應用程式\_offline.htm*檔案，以使用 FTP 連線的站台或**檔案管理員**裝載提供者的控制項台中公用程式。</span><span class="sxs-lookup"><span data-stu-id="8f512-158">You can copy the *app\_offline.htm* file to the site by using an FTP connection or the **File Manager** utility in the hosting provider's control panel.</span></span> <span data-ttu-id="8f512-159">本教學課程中，您將使用**檔案管理員**。</span><span class="sxs-lookup"><span data-stu-id="8f512-159">For this tutorial, you'll use the **File Manager**.</span></span>

<span data-ttu-id="8f512-160">開啟控制台，然後選取**檔案管理員**如同在[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="8f512-160">Open the control panel and select **File Manager** as you did in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="8f512-161">選取  **contosouniversity.com** ，然後**wwwroot**以取得您的應用程式根資料夾，然後按一下**上載**。</span><span class="sxs-lookup"><span data-stu-id="8f512-161">Select **contosouniversity.com** and then **wwwroot** to get to your application's root folder, and then click **Upload**.</span></span>

<span data-ttu-id="8f512-162">[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="8f512-162">[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)</span></span>

<span data-ttu-id="8f512-163">在 **上傳檔案**對話方塊中，選取*應用程式\_offline.htm*檔案，然後按一下**上載**。</span><span class="sxs-lookup"><span data-stu-id="8f512-163">In the **Upload File** dialog box, select the *app\_offline.htm* file and then click **Upload**.</span></span>

<span data-ttu-id="8f512-164">[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="8f512-164">[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)</span></span>

<span data-ttu-id="8f512-165">瀏覽至您的網站 URL。</span><span class="sxs-lookup"><span data-stu-id="8f512-165">Browse to your site's URL.</span></span> <span data-ttu-id="8f512-166">您會看到*應用程式\_offline.htm*頁面現在會顯示而不是您的首頁。</span><span class="sxs-lookup"><span data-stu-id="8f512-166">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

<span data-ttu-id="8f512-167">[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="8f512-167">[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)</span></span>

<span data-ttu-id="8f512-168">您現在已準備好部署至生產環境。</span><span class="sxs-lookup"><span data-stu-id="8f512-168">You are now ready to deploy to production.</span></span>

## <a name="deploying-the-code-update-to-the-production-environment"></a><span data-ttu-id="8f512-169">程式碼將更新部署至生產環境</span><span class="sxs-lookup"><span data-stu-id="8f512-169">Deploying the Code Update to the Production Environment</span></span>

<span data-ttu-id="8f512-170">在  **Web 單鍵發佈**工具列上，選擇**生產**發行設定檔，然後按一下 **發佈 Web**。</span><span class="sxs-lookup"><span data-stu-id="8f512-170">In the **Web One Click Publish** toolbar, choose the **Production** publish profile and then click **Publish Web**.</span></span>

<span data-ttu-id="8f512-171">Visual Studio 會部署更新的應用程式和瀏覽器開啟至網站的首頁。</span><span class="sxs-lookup"><span data-stu-id="8f512-171">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="8f512-172">*應用程式\_offline.htm*檔案會顯示。</span><span class="sxs-lookup"><span data-stu-id="8f512-172">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="8f512-173">您可以測試以確認成功部署之前，必須先移除*應用程式\_offline.htm*檔案。</span><span class="sxs-lookup"><span data-stu-id="8f512-173">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>

<span data-ttu-id="8f512-174">返回**檔案管理員**控制項台中應用程式。</span><span class="sxs-lookup"><span data-stu-id="8f512-174">Return to the **File Manager** application in the control panel.</span></span> <span data-ttu-id="8f512-175">選取  **contosouniversity.com**並**wwwroot**，選取**應用程式\_offline.htm**，然後按一下**刪除**。</span><span class="sxs-lookup"><span data-stu-id="8f512-175">Select **contosouniversity.com** and **wwwroot**, select **app\_offline.htm**, and then click **Delete**.</span></span>

<span data-ttu-id="8f512-176">[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="8f512-176">[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)</span></span>

<span data-ttu-id="8f512-177">在瀏覽器中，開啟 Instructors 頁面在公用網站中，然後選取講師，以確認更新已成功部署。</span><span class="sxs-lookup"><span data-stu-id="8f512-177">In the browser, open the Instructors page in the public site, and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="8f512-178">[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="8f512-178">[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)</span></span>

<span data-ttu-id="8f512-179">您現在已部署應用程式更新未未牽涉到資料庫變更。</span><span class="sxs-lookup"><span data-stu-id="8f512-179">You've now deployed an application update that did not involve a database change.</span></span> <span data-ttu-id="8f512-180">下一個教學課程會示範如何將資料庫變更部署。</span><span class="sxs-lookup"><span data-stu-id="8f512-180">The next tutorial shows you how to deploy a database change.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8f512-181">[上一頁](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="8f512-181">[Previous](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
[Next](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)</span></span>
