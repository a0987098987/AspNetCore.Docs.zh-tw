---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: "使用 Visual Studio 的 ASP.NET Web 部署： 部署到生產環境 |Microsoft 文件"
author: tdykstra
description: "此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: 2c49e7f6925b1ca172642747c5052ba97d70d036
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a><span data-ttu-id="3caeb-103">使用 Visual Studio 的 ASP.NET Web 部署： 部署到生產環境</span><span class="sxs-lookup"><span data-stu-id="3caeb-103">ASP.NET Web Deployment using Visual Studio: Deploying to Production</span></span>
====================
<span data-ttu-id="3caeb-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="3caeb-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="3caeb-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="3caeb-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="3caeb-106">此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="3caeb-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="3caeb-107">數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="3caeb-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="3caeb-108">總覽</span><span class="sxs-lookup"><span data-stu-id="3caeb-108">Overview</span></span>

<span data-ttu-id="3caeb-109">在本教學課程中，設定 Microsoft Azure 帳戶、 建立預備與生產環境和部署到預備環境的 ASP.NET web 應用程式和實際執行環境中的使用 Visual Studio 單鍵發行功能。</span><span class="sxs-lookup"><span data-stu-id="3caeb-109">In this tutorial, you set up a Microsoft Azure account, create staging and production environments, and deploy your ASP.NET web application to the staging and production environments by using the Visual Studio one-click publish feature.</span></span>

<span data-ttu-id="3caeb-110">如果您想要的話，您可以將它部署到協力廠商主機服務提供者。</span><span class="sxs-lookup"><span data-stu-id="3caeb-110">If you prefer, you can deploy to a third-party hosting provider.</span></span> <span data-ttu-id="3caeb-111">本教學課程中所述的程序的大部分都是相同的主控提供者或 azure，不同之處在於每個提供者有它自己的帳戶和網站管理的使用者介面。</span><span class="sxs-lookup"><span data-stu-id="3caeb-111">Most of the procedures described in this tutorial are the same for a hosting provider or for Azure, except that each provider has its own user interface for account and web site management.</span></span> <span data-ttu-id="3caeb-112">您可以找到主控提供者中的[的提供者組件庫](https://www.microsoft.com/web/hosting)Microsoft.com 網站上。</span><span class="sxs-lookup"><span data-stu-id="3caeb-112">You can find a hosting provider in the [gallery of providers](https://www.microsoft.com/web/hosting) on the Microsoft.com web site.</span></span>

<span data-ttu-id="3caeb-113">提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，務必檢查本教學課程系列的 [疑難排解] 頁面。</span><span class="sxs-lookup"><span data-stu-id="3caeb-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the Troubleshooting page in this tutorial series.</span></span>

## <a name="get-a-microsoft-azure-account"></a><span data-ttu-id="3caeb-114">取得 Microsoft Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="3caeb-114">Get a Microsoft Azure account</span></span>

<span data-ttu-id="3caeb-115">如果您還沒有 Azure 帳戶，您可以建立免費的試用帳戶只需要幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="3caeb-115">If you don't already have an Azure account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="3caeb-116">如需詳細資訊，請參閱[Azure 免費試用](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="3caeb-116">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

## <a name="create-a-staging-environment"></a><span data-ttu-id="3caeb-117">建立預備環境</span><span class="sxs-lookup"><span data-stu-id="3caeb-117">Create a staging environment</span></span>

> [!NOTE]
> <span data-ttu-id="3caeb-118">由於本教學課程所撰寫，Azure 應用程式服務會加入新功能，可自動化許多程序中建立暫存和實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="3caeb-118">Since this tutorial was written, Azure App Service added a new feature to automate many of the processes for creating staging and production environments.</span></span> <span data-ttu-id="3caeb-119">請參閱[設定預備環境中 Azure App Service web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/)。</span><span class="sxs-lookup"><span data-stu-id="3caeb-119">See [Set up staging environments for web apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).</span></span>


<span data-ttu-id="3caeb-120">中所述[部署至測試環境教學課程](deploying-to-iis.md)、 最可靠的測試環境是在具有方式就像是實際執行的網站主機服務提供者網站。</span><span class="sxs-lookup"><span data-stu-id="3caeb-120">As explained in the [Deploy to the Test Environment tutorial](deploying-to-iis.md), the most reliable test environment is a web site at the hosting provider that's just like the production web site.</span></span> <span data-ttu-id="3caeb-121">在許多主控提供者，您就必須權衡這個的優點和重大的額外成本，但在 Azure 中您可以建立其他可用的 web 應用程式為您執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3caeb-121">At many hosting providers you would have to weigh the benefits of this against significant additional cost, but in Azure you can create an additional free web app as your staging app.</span></span> <span data-ttu-id="3caeb-122">您也需要資料庫，並透過您的生產資料庫的費用的額外費用將會是 none 或最少。</span><span class="sxs-lookup"><span data-stu-id="3caeb-122">You also need a database, and the additional expense for that over the expense of your production database will be either none or minimal.</span></span> <span data-ttu-id="3caeb-123">在 Azure 中您必須支付您所使用的資料庫儲存體數量，而不是每個資料庫，而且您將使用在預備環境中的其他儲存體數量最小。</span><span class="sxs-lookup"><span data-stu-id="3caeb-123">In Azure you pay for the amount of database storage you use rather than for each database, and the amount of additional storage you'll use in staging will be minimal.</span></span>

<span data-ttu-id="3caeb-124">中所述[部署至測試環境教學課程](deploying-to-iis.md)、 預備和生產環境，您要將兩個資料庫部署至一個資料庫中。</span><span class="sxs-lookup"><span data-stu-id="3caeb-124">As explained in the [Deploy to the Test Environment tutorial](deploying-to-iis.md), in staging and production you're going to deploy your two databases into one database.</span></span> <span data-ttu-id="3caeb-125">如果您想要將它們分開，處理程序都是一樣，但是您會建立每個環境的其他資料庫，而當您建立的發行設定檔時，會選取每個資料庫的正確目的地字串。</span><span class="sxs-lookup"><span data-stu-id="3caeb-125">If you wanted to keep them separate, the process would be the same except that you'd create an additional database for each environment and you would select the correct destination string for each database when you create the publish profile.</span></span>

<span data-ttu-id="3caeb-126">本節的教學課程中，您將建立的 web 應用程式和資料庫来用於臨時環境中，而且您會部署到預備環境並建立及部署到生產環境之前，請測試那里。</span><span class="sxs-lookup"><span data-stu-id="3caeb-126">In this section of the tutorial you'll create a web app and database to use for the staging environment, and you'll deploy to staging and test there before creating and deploying to the production environment.</span></span>

> [!NOTE]
> <span data-ttu-id="3caeb-127">下列步驟顯示如何在 Azure App Service 中建立 web 應用程式，使用 Azure 管理入口網站。</span><span class="sxs-lookup"><span data-stu-id="3caeb-127">The following steps show how to create a web app in Azure App Service by using the Azure management portal.</span></span> <span data-ttu-id="3caeb-128">在 Azure sdk 最新版本中，您同樣可以這樣不需要離開 Visual Studio 中，使用 [伺服器總管]。</span><span class="sxs-lookup"><span data-stu-id="3caeb-128">In the latest version of the Azure SDK, you can also do this without leaving Visual Studio, by using Server Explorer.</span></span> <span data-ttu-id="3caeb-129">在 Visual Studio 2013 中，您也可以直接從 [發行] 對話方塊建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3caeb-129">In Visual Studio 2013, you can also create a web app directly from the Publish dialog.</span></span> <span data-ttu-id="3caeb-130">如需詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式。](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)</span><span class="sxs-lookup"><span data-stu-id="3caeb-130">For more information, see [Create an ASP.NET web app in Azure App Service.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)</span></span>


1. <span data-ttu-id="3caeb-131">在[Azure 管理入口網站](https://manage.windowsazure.com/)，按一下 **網站**，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-131">In the [Azure Management Portal](https://manage.windowsazure.com/), click **Websites**, and then click **New**.</span></span>
2. <span data-ttu-id="3caeb-132">按一下**網站**，然後按一下 **自訂建立**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-132">Click **Website**, and then click **Custom Create**.</span></span>

    <span data-ttu-id="3caeb-133">**新增網站-自訂建立**精靈 隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="3caeb-133">The **New Website - Custom Create** wizard opens.</span></span> <span data-ttu-id="3caeb-134">**自訂建立**精靈可讓您建立的網站和資料庫在相同的時間。</span><span class="sxs-lookup"><span data-stu-id="3caeb-134">The **Custom Create** wizard enables you to create a web site and a database at the same time.</span></span>
3. <span data-ttu-id="3caeb-135">在**建立網站**步驟在精靈中，輸入字串**URL**方塊，以使用唯一的 URL 做為您的應用程式的預備環境。</span><span class="sxs-lookup"><span data-stu-id="3caeb-135">In the **Create Website** step of the wizard, enter a string in the **URL** box to use as the unique URL for your application's staging environment.</span></span> <span data-ttu-id="3caeb-136">例如，輸入 ContosoUniversity staging123 （包括讓其成為唯一以防 ContosoUniversity 臨時區域不會採取在最後的隨機數字）。</span><span class="sxs-lookup"><span data-stu-id="3caeb-136">For example, enter ContosoUniversity-staging123 (including random numbers at the end to make it unique in case ContosoUniversity-staging is taken).</span></span>

    <span data-ttu-id="3caeb-137">完整的 URL 將會包含哪些您在此處輸入，加上尾碼，您所看到的文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="3caeb-137">The complete URL will consist of what you enter here plus the suffix that you see next to the text box.</span></span>
4. <span data-ttu-id="3caeb-138">在**區域**下拉式清單中，選擇最接近您的地區。</span><span class="sxs-lookup"><span data-stu-id="3caeb-138">In the **Region** drop-down list, choose the region that is closest to you.</span></span>

    <span data-ttu-id="3caeb-139">此設定指定您的 web 應用程式將執行中的資料中心。</span><span class="sxs-lookup"><span data-stu-id="3caeb-139">This setting specifies which data center your web app will run in.</span></span>
5. <span data-ttu-id="3caeb-140">在**資料庫**下拉式清單中，選擇**建立新的 SQL database**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-140">In the **Database** drop-down list, choose **Create a new SQL database**.</span></span>
6. <span data-ttu-id="3caeb-141">在**資料庫連接字串名稱**方塊中，保留預設值為*DefaultConnection*。</span><span class="sxs-lookup"><span data-stu-id="3caeb-141">In the **DB Connection String Name** box, leave the default value, *DefaultConnection*.</span></span>
7. <span data-ttu-id="3caeb-142">按一下底部的方塊右邊的箭號。</span><span class="sxs-lookup"><span data-stu-id="3caeb-142">Click the arrow that points to the right at the bottom of the box.</span></span>

    <span data-ttu-id="3caeb-143">下圖顯示**建立網站**對話方塊的範例值。</span><span class="sxs-lookup"><span data-stu-id="3caeb-143">The following illustration shows the **Create Website** dialog with sample values in it.</span></span> <span data-ttu-id="3caeb-144">URL 和您所輸入的區域將會不同。</span><span class="sxs-lookup"><span data-stu-id="3caeb-144">The URL and Region that you have entered will be different.</span></span>

    ![建立網站的步驟](deploying-to-production/_static/image1.png)

    <span data-ttu-id="3caeb-146">精靈會進入**指定資料庫設定**步驟。</span><span class="sxs-lookup"><span data-stu-id="3caeb-146">The wizard advances to the **Specify database settings** step.</span></span>
8. <span data-ttu-id="3caeb-147">在**名稱**方塊中，輸入*ContosoUniversity*加上隨機的數字，讓其成為唯一，例如*ContosoUniversity123*。</span><span class="sxs-lookup"><span data-stu-id="3caeb-147">In the **Name** box, enter *ContosoUniversity* plus a random number to make it unique, for example *ContosoUniversity123*.</span></span>
9. <span data-ttu-id="3caeb-148">在**伺服器**方塊中，選取**新 SQL Database 伺服器**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-148">In the **Server** box, select **New SQL Database Server**.</span></span>
10. <span data-ttu-id="3caeb-149">輸入系統管理員名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="3caeb-149">Enter an administrator name and password.</span></span>

    <span data-ttu-id="3caeb-150">您不輸入現有的名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="3caeb-150">You aren't entering an existing name and password here.</span></span> <span data-ttu-id="3caeb-151">您正在輸入的新名稱和您正在定義可存取資料庫時使用的密碼。</span><span class="sxs-lookup"><span data-stu-id="3caeb-151">You're entering a new name and password that you're defining now to use later when you access the database.</span></span>
11. <span data-ttu-id="3caeb-152">在**區域**方塊中，選擇您選擇 web 應用程式的相同地區。</span><span class="sxs-lookup"><span data-stu-id="3caeb-152">In the **Region** box, choose the same region that you chose for the web app.</span></span>

    <span data-ttu-id="3caeb-153">保留網頁伺服器和資料庫伺服器位於相同區域中提供最佳的效能並減少費用。</span><span class="sxs-lookup"><span data-stu-id="3caeb-153">Keeping the web server and the database server in the same region gives you the best performance and minimizes expenses.</span></span>
12. <span data-ttu-id="3caeb-154">按一下底部的方塊以表示您已完成的核取記號。</span><span class="sxs-lookup"><span data-stu-id="3caeb-154">Click the check mark at the bottom of the box to indicate that you're finished.</span></span>

    <span data-ttu-id="3caeb-155">下圖顯示**指定資料庫設定**對話方塊的範例值。</span><span class="sxs-lookup"><span data-stu-id="3caeb-155">The following illustration shows the **Specify database settings** dialog with sample values in it.</span></span> <span data-ttu-id="3caeb-156">您輸入的值可能會不同。</span><span class="sxs-lookup"><span data-stu-id="3caeb-156">The values you have entered may be different.</span></span>

    ![資料庫的設定步驟的 新增網站-建立資料庫精靈](deploying-to-production/_static/image2.png)

    <span data-ttu-id="3caeb-158">在管理入口網站返回網站] 頁面上，而**狀態**欄顯示 [正在建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3caeb-158">The Management Portal returns to the Websites page, and the **Status** column shows that the web app is being created.</span></span> <span data-ttu-id="3caeb-159">（通常小於一分鐘），一段時間之後**狀態**資料行會顯示已成功建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3caeb-159">After a while (typically less than a minute), the **Status** column shows that the web app was successfully created.</span></span> <span data-ttu-id="3caeb-160">在左側導覽列中的 web 應用程式，您會在您的帳戶數目旁會顯示**網站**圖示和資料庫數目旁會顯示**SQL 資料庫**圖示。</span><span class="sxs-lookup"><span data-stu-id="3caeb-160">In the navigation bar at the left, the number of web apps you have in your account appears next to the **Websites** icon, and the number of databases appears next to the **SQL Databases** icon.</span></span>

    ![管理入口網站，建立網站的網站頁面](deploying-to-production/_static/image3.png)

    <span data-ttu-id="3caeb-162">您的 web 應用程式名稱會從圖例中的範例應用程式不同。</span><span class="sxs-lookup"><span data-stu-id="3caeb-162">Your web app name will be different from the example app in the illustration.</span></span>

## <a name="deploy-the-application-to-staging"></a><span data-ttu-id="3caeb-163">部署到預備環境的應用程式</span><span class="sxs-lookup"><span data-stu-id="3caeb-163">Deploy the application to staging</span></span>

<span data-ttu-id="3caeb-164">既然您已經建立的 web 應用程式和資料庫預備環境，您可以部署它的專案。</span><span class="sxs-lookup"><span data-stu-id="3caeb-164">Now that you have created a web app and database for the staging environment, you can deploy the project to it.</span></span>

> [!NOTE]
> <span data-ttu-id="3caeb-165">以下指示說明如何建立發行設定檔下載*.publishsettings*不只適用於 Azure，也為協力廠商裝載提供者的檔案。</span><span class="sxs-lookup"><span data-stu-id="3caeb-165">These instructions show how to create a publish profile by downloading a *.publishsettings* file, which works not only for Azure but also for third-party hosting providers.</span></span> <span data-ttu-id="3caeb-166">最新版的 Azure SDK 也可讓您直接連接到 Azure，從 Visual Studio，然後選擇 從 web 應用程式必須在您的 Azure 帳戶中的清單。</span><span class="sxs-lookup"><span data-stu-id="3caeb-166">The latest Azure SDK also enables you to connect directly to Azure from Visual Studio, and choose from a list of web apps that you have in your Azure account.</span></span> <span data-ttu-id="3caeb-167">在 Visual Studio 2013 中，您可以登入 Azure 從**Web Publish**對話方塊或從**伺服器總管**視窗。</span><span class="sxs-lookup"><span data-stu-id="3caeb-167">In Visual Studio 2013, you can sign in to Azure from the **Web Publish** dialog or from the **Server Explorer** window.</span></span> <span data-ttu-id="3caeb-168">如需詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="3caeb-168">For more information, see [Create an ASP.NET web app in Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span>


### <a name="download-the-publishsettings-file"></a><span data-ttu-id="3caeb-169">下載.publishsettings 檔案</span><span class="sxs-lookup"><span data-stu-id="3caeb-169">Download the .publishsettings file</span></span>

1. <span data-ttu-id="3caeb-170">按一下您剛才建立的 web 應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="3caeb-170">Click the name of the web app that you just created.</span></span>

    ![按一下以移至儀表板的站台](deploying-to-production/_static/image4.png)
2. <span data-ttu-id="3caeb-172">在下**快速概覽**中**儀表板**索引標籤上，按一下 **下載發行設定檔**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-172">Under **Quick glance** in the **Dashboard** tab, click **Download publish profile**.</span></span>

    ![下載發行設定檔的連結](deploying-to-production/_static/image5.png)

    <span data-ttu-id="3caeb-174">此步驟中下載檔案，其中包含所有您需要以應用程式部署至您的 web 應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="3caeb-174">This step downloads a file that contains all of the settings that you need in order to deploy an application to your web app.</span></span> <span data-ttu-id="3caeb-175">您將此檔案匯入到 Visual Studio，因此您不需要手動輸入此資訊。</span><span class="sxs-lookup"><span data-stu-id="3caeb-175">You'll import this file into Visual Studio so you don't have to enter this information manually.</span></span>
3. <span data-ttu-id="3caeb-176">儲存*.publishsettings*檔案，您可以從 Visual Studio 存取的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="3caeb-176">Save the *.publishsettings* file in a folder that you can access from Visual Studio.</span></span>

    ![儲存.publishsettings 檔案](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > <span data-ttu-id="3caeb-178">安全性- *.publishsettings*檔案包含您的認證 （未編碼），可用來管理 Azure 訂用帳戶和服務。</span><span class="sxs-lookup"><span data-stu-id="3caeb-178">Security - The *.publishsettings* file contains your credentials (unencoded) that are used to administer your Azure subscriptions and services.</span></span> <span data-ttu-id="3caeb-179">這個檔案的安全性最佳作法是暫時儲存 （例如在 Libraries\Documents 資料夾），在來源目錄之外，並匯入完成後再將它刪除。</span><span class="sxs-lookup"><span data-stu-id="3caeb-179">The security best practice for this file is to store it temporarily outside your source directories (for example in the Libraries\Documents folder), and then delete it once the import has completed.</span></span> <span data-ttu-id="3caeb-180">取得存取權的惡意使用者*.publishsettings*檔案可以編輯、 建立和刪除您的 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="3caeb-180">A malicious user who gains access to the *.publishsettings* file can edit, create, and delete your Azure services.</span></span>

### <a name="create-a-publish-profile"></a><span data-ttu-id="3caeb-181">建立發行設定檔</span><span class="sxs-lookup"><span data-stu-id="3caeb-181">Create a publish profile</span></span>

1. <span data-ttu-id="3caeb-182">在 Visual Studio 中，以滑鼠右鍵按一下中的 ContosoUniversity 專案**方案總管 中**選取**發行**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="3caeb-182">In Visual Studio, right-click the ContosoUniversity project in **Solution Explorer** and select **Publish** from the context menu.</span></span>

    <span data-ttu-id="3caeb-183">**發行 Web**精靈 隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="3caeb-183">The **Publish Web** wizard opens.</span></span>
2. <span data-ttu-id="3caeb-184">按一下**設定檔** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="3caeb-184">Click the **Profile** tab.</span></span>
3. <span data-ttu-id="3caeb-185">按一下 [匯入] 。</span><span class="sxs-lookup"><span data-stu-id="3caeb-185">Click **Import**.</span></span>
4. <span data-ttu-id="3caeb-186">瀏覽至*.publishsettings*您稍早，下載檔案，然後按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-186">Navigate to the *.publishsettings* file you downloaded earlier, and then click **Open**.</span></span>

    ![匯入發行設定 對話方塊](deploying-to-production/_static/image7.png)
5. <span data-ttu-id="3caeb-188">在**連接**索引標籤上，按一下 **驗證連線**確定設定正確無誤。</span><span class="sxs-lookup"><span data-stu-id="3caeb-188">In the **Connection** tab, click **Validate Connection** to make sure that the settings are correct.</span></span>

    <span data-ttu-id="3caeb-189">當已驗證的連接時，旁邊顯示綠色的核取記號**驗證連線** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="3caeb-189">When the connection has been validated, a green check mark is shown next to the **Validate Connection** button.</span></span>

    <span data-ttu-id="3caeb-190">對於某些裝載的提供者，當您按一下**驗證連線**，您可能會看到**憑證錯誤** 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="3caeb-190">For some hosting providers, when you click **Validate Connection**, you might see a **Certificate Error** dialog box.</span></span> <span data-ttu-id="3caeb-191">如果您這樣做，請確認伺服器名稱是您所預期。</span><span class="sxs-lookup"><span data-stu-id="3caeb-191">If you do, verify that the server name is what you expect.</span></span> <span data-ttu-id="3caeb-192">如果伺服器名稱正確，請選取**儲存這個憑證以供未來 Visual Studio 工作階段**按一下**接受**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-192">If the server name is correct, select **Save this certificate for future sessions of Visual Studio** and click **Accept**.</span></span> <span data-ttu-id="3caeb-193">（此錯誤表示主機服務提供者已選擇來避免所購買的 SSL 憑證適用於您要部署的 URL 的費用。</span><span class="sxs-lookup"><span data-stu-id="3caeb-193">(This error means that the hosting provider has chosen to avoid the expense of purchasing an SSL certificate for the URL that you are deploying to.</span></span> <span data-ttu-id="3caeb-194">如果您想要使用的有效憑證來建立安全連接，請連絡您的主機服務提供者。）</span><span class="sxs-lookup"><span data-stu-id="3caeb-194">If you prefer to establish a secure connection by using a valid certificate, contact your hosting provider.)</span></span>
6. <span data-ttu-id="3caeb-195">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="3caeb-195">Click **Next**.</span></span>

    ![連接成功圖示，然後在 [連線] 索引標籤中的下一步按鈕](deploying-to-production/_static/image8.png)
7. <span data-ttu-id="3caeb-197">在**設定**索引標籤上，依序展開**檔案發行選項**，然後選取**檔案排除應用程式\_資料資料夾**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-197">In the **Settings** tab, expand **File Publish Options**, and then select **Exclude files from the App\_Data folder**.</span></span>

    <span data-ttu-id="3caeb-198">如需下的其他選項**檔案發行選項**，請參閱[部署至 IIS](deploying-to-iis.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="3caeb-198">For information about the other options under **File Publish Options**, see the [deploying to IIS](deploying-to-iis.md) tutorial.</span></span> <span data-ttu-id="3caeb-199">螢幕擷取畫面，會顯示此步驟和下列資料庫的設定步驟的結果已結尾的資料庫設定步驟。</span><span class="sxs-lookup"><span data-stu-id="3caeb-199">The screen shot that shows the result of this step and the following database configuration steps is at the end of the database configuration steps.</span></span>
8. <span data-ttu-id="3caeb-200">在下**DefaultConnection**中**資料庫**區段中，成員資格資料庫的資料庫部署設定。</span><span class="sxs-lookup"><span data-stu-id="3caeb-200">Under **DefaultConnection** in the **Databases** section, configure database deployment for the membership database.</span></span>
9. 1. <span data-ttu-id="3caeb-201">選取**更新資料庫**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-201">Select **Update database**.</span></span>

        <span data-ttu-id="3caeb-202">**遠端的連接字串**方塊的正下方**DefaultConnection**填寫.publishsettings 檔案的連接字串。連接字串包含 SQL Server 認證，會以純文字儲存*.pubxml*檔案。</span><span class="sxs-lookup"><span data-stu-id="3caeb-202">The **Remote connection string** box directly below **DefaultConnection** is filled in with the connection string from the .publishsettings file.The connection string includes SQL Server credentials, which are stored in plain text in the *.pubxml* file.</span></span> <span data-ttu-id="3caeb-203">如果您不想將它們儲存到永久那里，您可以部署資料庫之後從發行設定檔移除它們，並將其儲存在 Azure 中。</span><span class="sxs-lookup"><span data-stu-id="3caeb-203">If you prefer not to store them permanently there, you can remove them from the publish profile after the database is deployed and store them in Azure instead.</span></span> <span data-ttu-id="3caeb-204">如需詳細資訊，請參閱[如何保護您的 ASP.NET 資料庫連接字串安全從來源部署至 Azure 時](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx)Scott Hanselman 部落格上。</span><span class="sxs-lookup"><span data-stu-id="3caeb-204">For more information, see [How to keep your ASP.NET database connection strings secure when deploying to Azure from Source](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) on Scott Hanselman's blog.</span></span>
    2. <span data-ttu-id="3caeb-205">按一下**設定資料庫更新**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-205">Click **Configure database updates**.</span></span>
    3. <span data-ttu-id="3caeb-206">在**設定資料庫更新**對話方塊中，按一下 **加入 SQL 指令碼**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-206">In the **Configure Database Updates** dialog box, click **Add SQL Script**.</span></span>
    4. <span data-ttu-id="3caeb-207">在**加入 SQL 指令碼**方塊中，瀏覽至*aspnet-資料-prod.sql*指令碼，您稍早儲存的方案資料夾中，然後按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-207">In the **Add SQL Script** box, navigate to the *aspnet-data-prod.sql* script that you saved earlier in the solution folder, and then click **Open**.</span></span>
    5. <span data-ttu-id="3caeb-208">關閉**設定資料庫更新** 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="3caeb-208">Close the **Configure Database Updates** dialog box.</span></span>
10. <span data-ttu-id="3caeb-209">在下**SchoolContext**中**資料庫**區段中，選取**執行 Code First 移轉 （在應用程式啟動時執行）**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-209">Under **SchoolContext** in the **Databases** section, select **Execute Code First Migrations (runs on application start)**.</span></span>

    <span data-ttu-id="3caeb-210">Visual Studio 會顯示**執行 Code First 移轉**而不是**更新資料庫**如`DbContext`類別。</span><span class="sxs-lookup"><span data-stu-id="3caeb-210">Visual Studio displays **Execute Code First Migrations** instead of **Update Database** for `DbContext` classes.</span></span> <span data-ttu-id="3caeb-211">如果您想要使用而不是移轉的 dbDacFx 提供者部署的資料庫，您可以使用存取`DbContext`類別，請參閱[如何部署 Code First 移轉沒有資料庫？](https://msdn.microsoft.com/en-us/library/ee942158.aspx#deploy_code_first_without_migrations)中的 Visual Studio Web 部署常見問題集和 MSDN 上的 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="3caeb-211">If you want to use the dbDacFx provider instead of Migrations to deploy a database that you access by using a `DbContext` class, see [How do I deploy a Code First database without Migrations?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#deploy_code_first_without_migrations) in the Web Deployment FAQ for Visual Studio and ASP.NET on MSDN.</span></span>

    <span data-ttu-id="3caeb-212">**設定** 索引標籤現在看起來像下列的範例：</span><span class="sxs-lookup"><span data-stu-id="3caeb-212">The **Settings** tab now looks like the following example:</span></span>

    ![臨時區域的 [設定] 索引標籤](deploying-to-production/_static/image9.png)
11. <span data-ttu-id="3caeb-214">執行下列步驟來儲存設定檔，並將它重新命名*臨時*:</span><span class="sxs-lookup"><span data-stu-id="3caeb-214">Perform the following steps to save the profile and rename it to *Staging*:</span></span>

    1. <span data-ttu-id="3caeb-215">按一下**設定檔**索引標籤，然後再按一下**管理設定檔**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-215">Click the **Profile** tab, and then click **Manage Profiles**.</span></span>
    2. <span data-ttu-id="3caeb-216">匯入建立兩個新的設定檔，另一個用於 FTP，一個用於 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="3caeb-216">The import created two new profiles, one for FTP and one for Web Deploy.</span></span> <span data-ttu-id="3caeb-217">設定 Web Deploy 設定檔： 重新命名此設定檔來*臨時*。</span><span class="sxs-lookup"><span data-stu-id="3caeb-217">You configured the Web Deploy profile: rename this profile to *Staging*.</span></span>

        ![重新命名到預備環境的設定檔](deploying-to-production/_static/image10.png)
    3. <span data-ttu-id="3caeb-219">關閉**編輯 Web 發行設定檔** 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="3caeb-219">Close the **Edit Web Publish Profiles** dialog box.</span></span>
    4. <span data-ttu-id="3caeb-220">關閉**發行 Web**精靈。</span><span class="sxs-lookup"><span data-stu-id="3caeb-220">Close the **Publish Web** wizard.</span></span>

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a><span data-ttu-id="3caeb-221">設定發行設定檔環境指標轉換</span><span class="sxs-lookup"><span data-stu-id="3caeb-221">Configure a publish profile transform for the environment indicator</span></span>

> [!NOTE]
> <span data-ttu-id="3caeb-222">本節說明如何設定 Web.config 轉換環境指標。</span><span class="sxs-lookup"><span data-stu-id="3caeb-222">This section shows how to set up a Web.config transform for the environment indicator.</span></span> <span data-ttu-id="3caeb-223">因為指標位於`<appSettings>`項目，您有另一個替代選項指定當您要部署至 Azure App Service 的轉換。</span><span class="sxs-lookup"><span data-stu-id="3caeb-223">Because the indicator is in the `<appSettings>` element, you have another alternative for specifying the transformation when you're deploying to Azure App Service.</span></span> <span data-ttu-id="3caeb-224">如需詳細資訊，請參閱[在 Azure 中的指定 Web.config 設定](web-config-transformations.md#watransforms)。</span><span class="sxs-lookup"><span data-stu-id="3caeb-224">For more information, see [Specifying Web.config settings in Azure](web-config-transformations.md#watransforms).</span></span>


1. <span data-ttu-id="3caeb-225">在**方案總管 中**，依序展開**屬性**，然後展開**PublishProfiles**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-225">In **Solution Explorer**, expand **Properties**, and then expand **PublishProfiles**.</span></span>
2. <span data-ttu-id="3caeb-226">以滑鼠右鍵按一下*Staging.pubxml*，然後按一下 **新增 Config 轉換**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-226">Right-click *Staging.pubxml*, and then click **Add Config Transform**.</span></span>

    ![新增預備 Config 轉換](deploying-to-production/_static/image11.png)

    <span data-ttu-id="3caeb-228">Visual Studio 會建立*Web.Staging.config*轉換檔案並開啟它。</span><span class="sxs-lookup"><span data-stu-id="3caeb-228">Visual Studio creates the *Web.Staging.config* transform file and opens it.</span></span>
3. <span data-ttu-id="3caeb-229">在*Web.Staging.config*轉換檔案，請插入下列程式碼開頭之後立即`configuration`標記。</span><span class="sxs-lookup"><span data-stu-id="3caeb-229">In the *Web.Staging.config* transform file, insert the following code immediately after the opening `configuration` tag.</span></span>

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    <span data-ttu-id="3caeb-230">當您使用的暫存發行設定檔時，這項轉換就會設定"Prod"的環境標記。</span><span class="sxs-lookup"><span data-stu-id="3caeb-230">When you use the Staging publish profile, this transform sets the environment indicator to "Prod".</span></span> <span data-ttu-id="3caeb-231">在已部署的 web 應用程式時，就看任何尾碼，例如 「 （開發） 」 或 「 （測試） 」 之後的 「 Contoso 大學"H1 標題。</span><span class="sxs-lookup"><span data-stu-id="3caeb-231">In the deployed web app, you won't see any suffix such as "(Dev)" or "(Test)" after the "Contoso University" H1 heading.</span></span>
4. <span data-ttu-id="3caeb-232">以滑鼠右鍵按一下*Web.Staging.config*檔案，然後按一下 **預覽轉換**先確定您已編碼的轉換會產生預期的變更。</span><span class="sxs-lookup"><span data-stu-id="3caeb-232">Right-click the *Web.Staging.config* file and click **Preview Transform** to make sure that the transform you coded produces the expected changes.</span></span>

    <span data-ttu-id="3caeb-233">**Web.config 預覽** 視窗會顯示結果的同時套用*Web.Release.config*轉換和*Web.Staging.config*轉換。</span><span class="sxs-lookup"><span data-stu-id="3caeb-233">The **Web.config Preview** window shows the result of applying both the *Web.Release.config* transforms and the *Web.Staging.config* transforms.</span></span>

### <a name="prevent-public-use-of-the-test-app"></a><span data-ttu-id="3caeb-234">防止公用用途的測試應用程式</span><span class="sxs-lookup"><span data-stu-id="3caeb-234">Prevent public use of the test app</span></span>

<span data-ttu-id="3caeb-235">預備應用程式的一個重要考量是會即時在網際網路上，但您不想使用它公用。</span><span class="sxs-lookup"><span data-stu-id="3caeb-235">An important consideration for the staging app is that it will be live on the Internet, but you don't want the public to use it.</span></span> <span data-ttu-id="3caeb-236">人們會尋找並使用它的可能性降到最低，您可以使用一或多個下列方法：</span><span class="sxs-lookup"><span data-stu-id="3caeb-236">To minimize the likelihood that people will find and use it, you can use one or more of the following methods:</span></span>

- <span data-ttu-id="3caeb-237">設定存取權給暫存應用程式只允許您用來測試臨時的 IP 位址的防火牆規則。</span><span class="sxs-lookup"><span data-stu-id="3caeb-237">Set firewall rules that allow access to the staging app only from IP addresses that you use to test staging.</span></span>
- <span data-ttu-id="3caeb-238">使用模糊化會難以猜測的 URL。</span><span class="sxs-lookup"><span data-stu-id="3caeb-238">Use an obfuscated URL that would be impossible to guess.</span></span>
- <span data-ttu-id="3caeb-239">建立*robots.txt*檔案，以確保搜尋引擎在搜尋結果中，，不編目給它的測試應用程式和報表的連結。</span><span class="sxs-lookup"><span data-stu-id="3caeb-239">Create a *robots.txt* file to ensure that search engines will not crawl the test app and report links to it in search results.</span></span>

<span data-ttu-id="3caeb-240">這些方法的第一個最有效，但未涵蓋在本教學課程因為將會要求您將部署到 Azure 雲端服務，而不是 Azure 應用程式服務。</span><span class="sxs-lookup"><span data-stu-id="3caeb-240">The first of these methods is the most effective but is not covered in this tutorial because it would require that you deploy to an Azure Cloud Service instead of Azure App Service.</span></span> <span data-ttu-id="3caeb-241">在 Azure 中的 IP 限制和雲端服務的相關詳細資訊，請參閱[計算裝載選項所提供 Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me)和[封鎖特定 IP 位址存取 Web 角色](https://msdn.microsoft.com/en-us/library/windowsazure/jj154098.aspx)。</span><span class="sxs-lookup"><span data-stu-id="3caeb-241">For more information about Cloud Services and IP restrictions in Azure, see [Compute Hosting Options Provided by Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) and [Block Specific IP Addresses from Accessing a Web Role](https://msdn.microsoft.com/en-us/library/windowsazure/jj154098.aspx).</span></span> <span data-ttu-id="3caeb-242">如果您要部署到協力廠商主機服務提供者，請連絡提供者，以了解如何實作 IP 限制。</span><span class="sxs-lookup"><span data-stu-id="3caeb-242">If you are deploying to a third-party hosting provider, contact the provider to find out how to implement IP restrictions.</span></span>

<span data-ttu-id="3caeb-243">此教學課程中，您將建立*robots.txt*檔案。</span><span class="sxs-lookup"><span data-stu-id="3caeb-243">For this tutorial, you'll create a *robots.txt* file.</span></span>

1. <span data-ttu-id="3caeb-244">在**方案總管 中**，ContosoUniversity 專案上按一下滑鼠右鍵，然後按一下**加入新項目**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-244">In **Solution Explorer**, right-click the ContosoUniversity project and click **Add New Item**.</span></span>
2. <span data-ttu-id="3caeb-245">建立新**文字檔**名為*robots.txt*，並放入其中的下列文字：</span><span class="sxs-lookup"><span data-stu-id="3caeb-245">Create a new **Text File** named *robots.txt*, and put the following text in it:</span></span>

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    <span data-ttu-id="3caeb-246">`User-agent`一行告訴檔案中的規則套用至所有搜尋引擎網頁自動尋檢 (robots)，搜尋引擎和`Disallow`一行指定應該進行編目的網站上的任何頁面。</span><span class="sxs-lookup"><span data-stu-id="3caeb-246">The `User-agent` line tells search engines that the rules in the file apply to all search engine web crawlers (robots), and the `Disallow` line specifies that no pages on the site should be crawled.</span></span>

    <span data-ttu-id="3caeb-247">您希望搜尋引擎目錄生產應用程式，因此您需要從生產環境部署中排除此檔案。</span><span class="sxs-lookup"><span data-stu-id="3caeb-247">You do want search engines to catalog your production app, so you need to exclude this file from production deployment.</span></span> <span data-ttu-id="3caeb-248">若要執行，您將設定在生產環境中的設定發行設定檔時建立它。</span><span class="sxs-lookup"><span data-stu-id="3caeb-248">To do that, you'll configure a setting in the Production publish profile when you create it.</span></span>

### <a name="deploy-to-staging"></a><span data-ttu-id="3caeb-249">部署到預備環境</span><span class="sxs-lookup"><span data-stu-id="3caeb-249">Deploy to staging</span></span>

1. <span data-ttu-id="3caeb-250">開啟**發行 Web** Contoso 大學專案上按一下滑鼠右鍵，然後按一下精靈**發行**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-250">Open the **Publish Web** wizard by right-clicking the Contoso University project and clicking **Publish**.</span></span>
2. <span data-ttu-id="3caeb-251">請確定**臨時**已選取設定檔。</span><span class="sxs-lookup"><span data-stu-id="3caeb-251">Make sure that the **Staging** profile is selected.</span></span>
3. <span data-ttu-id="3caeb-252">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="3caeb-252">Click **Publish**.</span></span>

    <span data-ttu-id="3caeb-253">**輸出**視窗會顯示所執行的部署動作，並回報成功完成部署。</span><span class="sxs-lookup"><span data-stu-id="3caeb-253">The **Output** window shows what deployment actions were taken and reports successful completion of the deployment.</span></span> <span data-ttu-id="3caeb-254">預設的瀏覽器會自動開啟至已部署的 web 應用程式的 URL。</span><span class="sxs-lookup"><span data-stu-id="3caeb-254">The default browser automatically opens to the URL of the deployed web app.</span></span>

## <a name="test-in-the-staging-environment"></a><span data-ttu-id="3caeb-255">在預備環境中測試</span><span class="sxs-lookup"><span data-stu-id="3caeb-255">Test in the staging environment</span></span>

<span data-ttu-id="3caeb-256">請注意，環境標記不存在 (沒有任何 「 （測試） 」 或 「 （開發） 」 之後的 H1 標題，其中顯示*Web.config*環境指標的轉換是否成功。</span><span class="sxs-lookup"><span data-stu-id="3caeb-256">Notice that the environment indicator is absent (there is no "(Test)" or "(Dev)" after the H1 heading, which shows that the *Web.config* transformation for the environment indicator was successful.</span></span>

![首頁上的預備環境](deploying-to-production/_static/image12.png)

<span data-ttu-id="3caeb-258">執行**學生**頁面，確認已部署的資料庫已經沒有學生。</span><span class="sxs-lookup"><span data-stu-id="3caeb-258">Run the **Students** page to verify that the deployed database has no students.</span></span>

<span data-ttu-id="3caeb-259">執行**講師**頁面，即可確認 Code First 植入講師資料的資料庫：</span><span class="sxs-lookup"><span data-stu-id="3caeb-259">Run the **Instructors** page to verify that Code First seeded the database with instructor data:</span></span>

<span data-ttu-id="3caeb-260">選取**新增學生**從**學生**功能表上，新增一位學生，，然後再檢視中新的學生**學生**頁面，即可確認您可以成功地寫入至資料庫.</span><span class="sxs-lookup"><span data-stu-id="3caeb-260">Select **Add Students** from the **Students** menu, add a student, and then view the new student in the **Students** page to verify that you can successfully write to the database.</span></span>

<span data-ttu-id="3caeb-261">從**課程**頁面上，按一下**更新信用額度**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-261">From the **Courses** page, click **Update Credits**.</span></span> <span data-ttu-id="3caeb-262">**更新信用額度**頁面需要系統管理員權限，所以**登入**頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="3caeb-262">The **Update Credits** page requires administrator permissions, so the **Log In** page is displayed.</span></span> <span data-ttu-id="3caeb-263">輸入您建立更早 （"admin"、"prodpwd"） 的系統管理員帳戶認證。</span><span class="sxs-lookup"><span data-stu-id="3caeb-263">Enter the administrator account credentials that you created earlier ("admin" and "prodpwd").</span></span> <span data-ttu-id="3caeb-264">**更新信用額度**頁面隨即顯示，以確認您在上一個教學課程中建立的系統管理員帳戶已正確地部署到測試環境。</span><span class="sxs-lookup"><span data-stu-id="3caeb-264">The **Update Credits** page is displayed, which verifies that the administrator account that you created in the previous tutorial was correctly deployed to the test environment.</span></span>

<span data-ttu-id="3caeb-265">要求會造成錯誤 ELMAH 會追蹤，並接著要求 ELMAH 錯誤報告無效的 URL。</span><span class="sxs-lookup"><span data-stu-id="3caeb-265">Request an invalid URL to cause an error that ELMAH will track, and then request the ELMAH error report.</span></span> <span data-ttu-id="3caeb-266">如果您要部署到協力廠商主機服務提供者，您可能會發現基於相同理由，它是空的上一個教學課程中的報告為空白。</span><span class="sxs-lookup"><span data-stu-id="3caeb-266">If you are deploying to a third-party hosting provider, you will probably find that the report is empty for the same reason that it was empty in the previous tutorial.</span></span> <span data-ttu-id="3caeb-267">您必須使用裝載提供者的帳戶管理工具來設定啟用 ELMAH 寫入記錄檔資料夾的資料夾權限。</span><span class="sxs-lookup"><span data-stu-id="3caeb-267">You will have to use the hosting provider's account management tools to configure folder permissions to enable ELMAH to write to the log folder.</span></span>

<span data-ttu-id="3caeb-268">您所建立的應用程式現在正在執行，就如同您將使用生產環境 web 應用程式在雲端中。</span><span class="sxs-lookup"><span data-stu-id="3caeb-268">The application that you created is now running in the cloud in a web app that is just like what you will use for production.</span></span> <span data-ttu-id="3caeb-269">因為一切運作正常下, 一個步驟是部署到生產環境。</span><span class="sxs-lookup"><span data-stu-id="3caeb-269">Since everything is working correctly, the next step is to deploy to production.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="3caeb-270">部署到生產環境</span><span class="sxs-lookup"><span data-stu-id="3caeb-270">Deploy to production</span></span>

<span data-ttu-id="3caeb-271">建立生產環境 web 應用程式和部署到生產環境的程序是預備環境，與相同，不同之處在於您需要排除*robots.txt*從部署。</span><span class="sxs-lookup"><span data-stu-id="3caeb-271">The process for creating a production web app and deploying to production is the same as for staging, except that you need to exclude the *robots.txt* from deployment.</span></span> <span data-ttu-id="3caeb-272">若要這樣做，您將編輯發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="3caeb-272">To do that you'll edit the publish profile file.</span></span>

### <a name="create-the-production-environment-and-the-production-publish-profile"></a><span data-ttu-id="3caeb-273">建立生產環境和生產環境的發行設定檔</span><span class="sxs-lookup"><span data-stu-id="3caeb-273">Create the production environment and the production publish profile</span></span>

1. <span data-ttu-id="3caeb-274">在 Azure 中，您用於執行相同的程序建立生產環境 web 應用程式和資料庫。</span><span class="sxs-lookup"><span data-stu-id="3caeb-274">Create the production web app and database in Azure, following the same procedure that you used for staging.</span></span>

    <span data-ttu-id="3caeb-275">當您建立資料庫時，您可以選擇將您稍早建立的相同伺服器上，或建立新的伺服器。</span><span class="sxs-lookup"><span data-stu-id="3caeb-275">When you create the database, you can choose to put it on the same server you created earlier, or create a new server.</span></span>
2. <span data-ttu-id="3caeb-276">下載*.publishsettings*檔案。</span><span class="sxs-lookup"><span data-stu-id="3caeb-276">Download the *.publishsettings* file.</span></span>
3. <span data-ttu-id="3caeb-277">建立發行設定檔匯入實際執行*.publishsettings*檔案，您用於執行相同的程序。</span><span class="sxs-lookup"><span data-stu-id="3caeb-277">Create the publish profile by importing the production *.publishsettings* file, following the same procedure that you used for staging.</span></span>

    <span data-ttu-id="3caeb-278">別忘了設定資料的部署指令碼，在**DefaultConnection**中**資料庫**區段**設定** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="3caeb-278">Don't forget to configure the data deployment script under **DefaultConnection** in the **Databases** section of the **Settings** tab.</span></span>
4. <span data-ttu-id="3caeb-279">重新命名的發行設定檔*生產*。</span><span class="sxs-lookup"><span data-stu-id="3caeb-279">Rename the publish profile to *Production*.</span></span>
5. <span data-ttu-id="3caeb-280">設定發行設定檔轉換環境指示器，相同的程序用來將暫存資料庫...</span><span class="sxs-lookup"><span data-stu-id="3caeb-280">Configure a publish profile transform for the environment indicator, following the same procedure that you used for staging..</span></span>

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a><span data-ttu-id="3caeb-281">編輯排除 robots.txt.pubxml 檔案</span><span class="sxs-lookup"><span data-stu-id="3caeb-281">Edit the .pubxml file to exclude robots.txt</span></span>

<span data-ttu-id="3caeb-282">發行設定檔的檔案會命名為&lt;profilename&gt;*.pubxml*和位於*PublishProfiles*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3caeb-282">Publish profile files are named &lt;profilename&gt;*.pubxml* and are located in the *PublishProfiles* folder.</span></span> <span data-ttu-id="3caeb-283">*PublishProfiles*資料夾低於*屬性*資料夾中的 C# web 應用程式專案，在*我的專案*VB web 應用程式專案中或之下的資料夾*應用程式\_資料*資料夾中的 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="3caeb-283">The *PublishProfiles* folder is under the *Properties* folder in a C# web application project, under the *My Project* folder in a VB web application project, or under the *App\_Data* folder in a web app project.</span></span> <span data-ttu-id="3caeb-284">每個*.pubxml*檔包含套用至其中一個設定發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="3caeb-284">Each *.pubxml* file contains settings that apply to one publish profile.</span></span> <span data-ttu-id="3caeb-285">您在發行網站精靈中輸入的值會儲存在這些檔案，以及您可以編輯它們，以便建立或變更不會在 Visual Studio UI 中提供的設定。</span><span class="sxs-lookup"><span data-stu-id="3caeb-285">The values you enter in the Publish Web wizard are stored in these files, and you can edit them to create or change settings that aren't made available in the Visual Studio UI.</span></span>

<span data-ttu-id="3caeb-286">根據預設， *.pubxml*檔案包含在專案中，當您建立的發行設定檔，但您可以排除專案和 Visual Studio 仍會使用這些方法。</span><span class="sxs-lookup"><span data-stu-id="3caeb-286">By default, *.pubxml* files are included in the project when you create a publish profile, but you can exclude them from the project and Visual Studio will still use them.</span></span> <span data-ttu-id="3caeb-287">Visual Studio 尋找*PublishProfiles*資料夾*.pubxml*檔案，無論是否包含在專案中。</span><span class="sxs-lookup"><span data-stu-id="3caeb-287">Visual Studio looks in the *PublishProfiles* folder for *.pubxml* files, regardless of whether they are included in the project.</span></span>

<span data-ttu-id="3caeb-288">每個*.pubxml*檔案沒有*。 pubxml.user*檔案。</span><span class="sxs-lookup"><span data-stu-id="3caeb-288">For each *.pubxml* file there is a *.pubxml.user* file.</span></span> <span data-ttu-id="3caeb-289">*。 Pubxml.user*檔案包含加密的密碼，如果您選取**儲存密碼**選項，而且依預設就會從專案排除。</span><span class="sxs-lookup"><span data-stu-id="3caeb-289">The *.pubxml.user* file contains the encrypted password if you selected the **Save password** option, and by default it is excluded from the project.</span></span>

<span data-ttu-id="3caeb-290">A *.pubxml*檔案包含屬於特定發行設定檔的設定。</span><span class="sxs-lookup"><span data-stu-id="3caeb-290">A *.pubxml* file contains the settings that pertain to a specific publish profile.</span></span> <span data-ttu-id="3caeb-291">如果您想要設定適用於所有設定檔的設定，您可以建立*。 wpp.targets*檔案。</span><span class="sxs-lookup"><span data-stu-id="3caeb-291">If you want to configure settings that apply to all profiles, you can create a *.wpp.targets* file.</span></span> <span data-ttu-id="3caeb-292">在建置程序會將這些檔案匯入*.csproj*或*.vbproj*專案檔，因此大部分的設定，您可以設定專案檔中設定這些檔案中。</span><span class="sxs-lookup"><span data-stu-id="3caeb-292">The build process imports these files into the *.csproj* or *.vbproj* project file, so most settings that you can configure in the project file can be configured in these files.</span></span> <span data-ttu-id="3caeb-293">如需有關*.pubxml*檔案和*。 wpp.targets*檔，請參閱[如何： 編輯發行設定檔 (.pubxml) 檔中的部署設定而。 wpp.targets Visual Studio 中的檔案Web 專案](https://msdn.microsoft.com/en-us/library/ff398069.aspx)。</span><span class="sxs-lookup"><span data-stu-id="3caeb-293">For more information about *.pubxml* files and *.wpp.targets* files, see [How to: Edit Deployment Settings in Publish Profile (.pubxml) Files and the .wpp.targets File in Visual Studio Web Projects](https://msdn.microsoft.com/en-us/library/ff398069.aspx).</span></span>

1. <span data-ttu-id="3caeb-294">在**方案總管 中**，依序展開**屬性**展開**PublishProfiles**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-294">In **Solution Explorer**, expand **Properties** and expand **PublishProfiles**.</span></span>
2. <span data-ttu-id="3caeb-295">以滑鼠右鍵按一下*Production.pubxml*按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-295">Right-click *Production.pubxml* and click **Open**.</span></span>

    ![開啟.pubxml 檔案](deploying-to-production/_static/image13.png)
3. <span data-ttu-id="3caeb-297">以滑鼠右鍵按一下*Production.pubxml*按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="3caeb-297">Right-click *Production.pubxml* and click **Open**.</span></span>
4. <span data-ttu-id="3caeb-298">加入下列幾行的結束之前立即`PropertyGroup`項目：</span><span class="sxs-lookup"><span data-stu-id="3caeb-298">Add the following lines immediately before the closing `PropertyGroup` element:</span></span>

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    <span data-ttu-id="3caeb-299">.Pubxml 檔案現在看起來像下列的範例：</span><span class="sxs-lookup"><span data-stu-id="3caeb-299">The .pubxml file now looks like the following example:</span></span>

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    <span data-ttu-id="3caeb-300">如需如何排除檔案和資料夾的詳細資訊，請參閱[可以我排除特定檔案或資料夾從部署嗎？](https://msdn.microsoft.com/en-us/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment)中**適用於 Visual Studio 和 ASP.NET 的 Web 部署常見問題集**MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="3caeb-300">For more information about how to exclude files and folders, see [Can I exclude specific files or folders from deployment?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) in the **Web Deployment FAQ for Visual Studio and ASP.NET** on MSDN.</span></span>

### <a name="deploy-to-production"></a><span data-ttu-id="3caeb-301">部署到生產環境</span><span class="sxs-lookup"><span data-stu-id="3caeb-301">Deploy to production</span></span>

1. <span data-ttu-id="3caeb-302">開啟**發行 Web**精靈，確定**生產**發行設定檔已選取，然後按一下**啟動預覽**上**預覽** 索引標籤可讓您確認*robots.txt*不將檔案複製到生產環境應用程式。</span><span class="sxs-lookup"><span data-stu-id="3caeb-302">Open the **Publish Web** wizard make sure that the **Production** publish profile is selected, and then click **Start Preview** on the **Preview** tab to verify that the *robots.txt* file will not be copied to the production app.</span></span>

    ![若要發行至生產環境的檔案的預覽](deploying-to-production/_static/image14.png)

    <span data-ttu-id="3caeb-304">檢閱將複製的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="3caeb-304">Review the list of files that will be copied.</span></span> <span data-ttu-id="3caeb-305">您會看到所有*.cs*檔案，包括*.aspx.vb*， *。 aspx.designer.cs*， *Master.cs*，和*Master.designer.cs*檔案會省略。</span><span class="sxs-lookup"><span data-stu-id="3caeb-305">You'll see that all of the *.cs* files, including *.aspx.cs*, *.aspx.designer.cs*, *Master.cs*, and *Master.designer.cs* files are omitted.</span></span> <span data-ttu-id="3caeb-306">這段程式碼編譯成*ContosoUniversity.dll*和*ContosUniversity.pdb*了中的檔案*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3caeb-306">All of this code has been compiled into the *ContosoUniversity.dll* and *ContosUniversity.pdb* files that you'll find in the *bin* folder.</span></span> <span data-ttu-id="3caeb-307">因為只有*.dll*才能的執行應用程式，而且您稍早指定，應該部署到執行應用程式所需的檔案、 no *.cs*檔會複製到目的地環境。</span><span class="sxs-lookup"><span data-stu-id="3caeb-307">Because only the *.dll* is needed to run the application, and you specified earlier that only files needed to run the application should be deployed, no *.cs* files were copied to the destination environment.</span></span> <span data-ttu-id="3caeb-308">*Obj*資料夾和*ContosoUniversity.csproj*和*。 副檔名為.csproj.user*檔案已省略相同的原因。</span><span class="sxs-lookup"><span data-stu-id="3caeb-308">The *obj* folder and the *ContosoUniversity.csproj* and *.csproj.user* files are omitted for the same reason.</span></span>

    <span data-ttu-id="3caeb-309">按一下**發行**將部署到實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="3caeb-309">Click **Publish** to deploy to the production environment.</span></span>
2. <span data-ttu-id="3caeb-310">在生產環境中，您用於執行相同的程序中的測試。</span><span class="sxs-lookup"><span data-stu-id="3caeb-310">Test in production, following the same procedure you used for staging.</span></span>

    <span data-ttu-id="3caeb-311">所有項目相當於除了 URL 的臨時區域與缺乏*robots.txt*檔案。</span><span class="sxs-lookup"><span data-stu-id="3caeb-311">Everything is identical to staging except for the URL and the absence of the *robots.txt* file.</span></span>

## <a name="summary"></a><span data-ttu-id="3caeb-312">總結</span><span class="sxs-lookup"><span data-stu-id="3caeb-312">Summary</span></span>

<span data-ttu-id="3caeb-313">您現在已成功部署和測試您的 web 應用程式，它可公開在網際網路上。</span><span class="sxs-lookup"><span data-stu-id="3caeb-313">You have now successfully deployed and tested your web app and it is available publicly over the Internet.</span></span>

![首頁生產環境](deploying-to-production/_static/image15.png)

<span data-ttu-id="3caeb-315">在下一個教學課程中，您將更新應用程式程式碼，並將變更部署到測試、 預備及實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="3caeb-315">In the next tutorial, you'll update application code and deploy the change to the test, staging, and production environments.</span></span>

> [!NOTE]
> <span data-ttu-id="3caeb-316">在實際執行環境中使用您的應用程式時您應該實作復原計劃。</span><span class="sxs-lookup"><span data-stu-id="3caeb-316">While your application is in use in the production environment you should be implementing a recovery plan.</span></span> <span data-ttu-id="3caeb-317">也就是說，您應該會定期備份您的資料庫從生產環境應用程式至安全的儲存體位置，並應該保持數個層代的這類備份。</span><span class="sxs-lookup"><span data-stu-id="3caeb-317">That is, you should be periodically backing up your databases from the production app to a secure storage location, and you should be keeping several generations of such backups.</span></span> <span data-ttu-id="3caeb-318">當您更新資料庫時，請立即變更之前的備份複本。</span><span class="sxs-lookup"><span data-stu-id="3caeb-318">When you update the database, you should make a backup copy from immediately before the change.</span></span> <span data-ttu-id="3caeb-319">然後，如果您犯了錯誤，並不探索它，直到它部署到生產環境之後，您將仍然能夠將資料庫復原到它損毀前的狀態。</span><span class="sxs-lookup"><span data-stu-id="3caeb-319">Then, if you make a mistake and don't discover it until after you have deployed it to production, you will still be able to recover the database to the state it was in before it became corrupted.</span></span> <span data-ttu-id="3caeb-320">如需詳細資訊，請參閱[Azure SQL Database 備份和還原](https://msdn.microsoft.com/en-us/library/windowsazure/jj650016.aspx)。</span><span class="sxs-lookup"><span data-stu-id="3caeb-320">For more information, see [Azure SQL Database Backup and Restore](https://msdn.microsoft.com/en-us/library/windowsazure/jj650016.aspx).</span></span>


> [!NOTE]
> <span data-ttu-id="3caeb-321">在本教學課程 SQL Server 版本，您要部署為 Azure SQL Database。</span><span class="sxs-lookup"><span data-stu-id="3caeb-321">In this tutorial the SQL Server edition that you are deploying to is Azure SQL Database.</span></span> <span data-ttu-id="3caeb-322">部署程序類似於其他 SQL Server 版本時，真正的實際執行應用程式可能在某些情況下就 for Azure SQL Database 需要特殊的程式碼。</span><span class="sxs-lookup"><span data-stu-id="3caeb-322">While the deployment process is similar to other editions of SQL Server, a real production application might require special code for Azure SQL Database in some scenarios.</span></span> <span data-ttu-id="3caeb-323">如需詳細資訊，請參閱[使用 Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb)和[SQL Server 和 Azure SQL Database 之間選擇](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing)。</span><span class="sxs-lookup"><span data-stu-id="3caeb-323">For more information, see [Working with Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) and [Choosing between SQL Server and Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3caeb-324">[上一頁](setting-folder-permissions.md)
[下一頁](deploying-a-code-update.md)</span><span class="sxs-lookup"><span data-stu-id="3caeb-324">[Previous](setting-folder-permissions.md)
[Next](deploying-a-code-update.md)</span></span>
