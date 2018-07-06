---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 使用 Visual Studio 的 ASP.NET Web 部署： 部署至生產環境 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: 5bc39162bc53dc27230f8ccc55266212a51b6380
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369641"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a><span data-ttu-id="63d47-103">使用 Visual Studio 的 ASP.NET Web 部署： 部署至生產環境</span><span class="sxs-lookup"><span data-stu-id="63d47-103">ASP.NET Web Deployment using Visual Studio: Deploying to Production</span></span>
====================
<span data-ttu-id="63d47-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="63d47-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="63d47-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="63d47-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="63d47-106">本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="63d47-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="63d47-107">這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="63d47-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="63d47-108">總覽</span><span class="sxs-lookup"><span data-stu-id="63d47-108">Overview</span></span>

<span data-ttu-id="63d47-109">在本教學課程中，設定 Microsoft Azure 帳戶、 建立預備與生產環境和部署 ASP.NET web 應用程式至預備環境和生產環境中的使用 Visual Studio 單鍵發行功能。</span><span class="sxs-lookup"><span data-stu-id="63d47-109">In this tutorial, you set up a Microsoft Azure account, create staging and production environments, and deploy your ASP.NET web application to the staging and production environments by using the Visual Studio one-click publish feature.</span></span>

<span data-ttu-id="63d47-110">如果您想，您可以部署到協力廠商裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="63d47-110">If you prefer, you can deploy to a third-party hosting provider.</span></span> <span data-ttu-id="63d47-111">本教學課程中所述的程序的大部分都是相同的主控提供者或 azure 中，不同之處在於每個提供者都有它自己的帳戶和網站管理的使用者介面。</span><span class="sxs-lookup"><span data-stu-id="63d47-111">Most of the procedures described in this tutorial are the same for a hosting provider or for Azure, except that each provider has its own user interface for account and web site management.</span></span> <span data-ttu-id="63d47-112">您可以找到在主機服務提供者[提供者的資源庫](https://www.microsoft.com/web/hosting)Microsoft.com 網站上。</span><span class="sxs-lookup"><span data-stu-id="63d47-112">You can find a hosting provider in the [gallery of providers](https://www.microsoft.com/web/hosting) on the Microsoft.com web site.</span></span>

<span data-ttu-id="63d47-113">提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查在本教學課程系列中的 [疑難排解] 頁面。</span><span class="sxs-lookup"><span data-stu-id="63d47-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the Troubleshooting page in this tutorial series.</span></span>

## <a name="get-a-microsoft-azure-account"></a><span data-ttu-id="63d47-114">取得 Microsoft Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="63d47-114">Get a Microsoft Azure account</span></span>

<span data-ttu-id="63d47-115">如果您還沒有 Azure 帳戶，您可以建立免費的試用帳戶，只需要幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="63d47-115">If you don't already have an Azure account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="63d47-116">如需詳細資訊，請參閱 < [Azure 免費試用](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="63d47-116">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

## <a name="create-a-staging-environment"></a><span data-ttu-id="63d47-117">建立預備環境</span><span class="sxs-lookup"><span data-stu-id="63d47-117">Create a staging environment</span></span>

> [!NOTE]
> <span data-ttu-id="63d47-118">因為撰寫本教學課程中，Azure App Service 就會加入新的功能，可自動化許多建立預備與生產環境的程序。</span><span class="sxs-lookup"><span data-stu-id="63d47-118">Since this tutorial was written, Azure App Service added a new feature to automate many of the processes for creating staging and production environments.</span></span> <span data-ttu-id="63d47-119">請參閱[設定預備環境中 Azure App Service 的 web apps](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/)。</span><span class="sxs-lookup"><span data-stu-id="63d47-119">See [Set up staging environments for web apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).</span></span>


<span data-ttu-id="63d47-120">中所述[部署至測試環境教學課程](deploying-to-iis.md)、 最可靠的測試環境是在有就像實際執行的網站主機服務提供者的網站。</span><span class="sxs-lookup"><span data-stu-id="63d47-120">As explained in the [Deploy to the Test Environment tutorial](deploying-to-iis.md), the most reliable test environment is a web site at the hosting provider that's just like the production web site.</span></span> <span data-ttu-id="63d47-121">在許多主機服務提供者，您就必須權衡針對重大的額外成本，此優點，但在 Azure 中您可以建立其他的免費 web 應用程式和預備應用程式。</span><span class="sxs-lookup"><span data-stu-id="63d47-121">At many hosting providers you would have to weigh the benefits of this against significant additional cost, but in Azure you can create an additional free web app as your staging app.</span></span> <span data-ttu-id="63d47-122">您也需要資料庫，而且，透過您的實際執行資料庫的費用的額外費用會是 none 或最小。</span><span class="sxs-lookup"><span data-stu-id="63d47-122">You also need a database, and the additional expense for that over the expense of your production database will be either none or minimal.</span></span> <span data-ttu-id="63d47-123">在 Azure 中您需支付您所使用的資料庫儲存體數量，而不是針對每個資料庫，並在預備環境中，您將使用的額外儲存空間量也能降低。</span><span class="sxs-lookup"><span data-stu-id="63d47-123">In Azure you pay for the amount of database storage you use rather than for each database, and the amount of additional storage you'll use in staging will be minimal.</span></span>

<span data-ttu-id="63d47-124">中所述[部署至測試環境教學課程](deploying-to-iis.md)、 預備和生產環境，您要將您的兩個資料庫部署至一個資料庫中。</span><span class="sxs-lookup"><span data-stu-id="63d47-124">As explained in the [Deploy to the Test Environment tutorial](deploying-to-iis.md), in staging and production you're going to deploy your two databases into one database.</span></span> <span data-ttu-id="63d47-125">如果您想要將它們分開，程序會相同，但是您會建立適用於每個環境的其他資料庫，而當您建立發行設定檔時，會選取每個資料庫的正確的目的地字串。</span><span class="sxs-lookup"><span data-stu-id="63d47-125">If you wanted to keep them separate, the process would be the same except that you'd create an additional database for each environment and you would select the correct destination string for each database when you create the publish profile.</span></span>

<span data-ttu-id="63d47-126">您將在本教學課程的這一節中建立 web 應用程式和預備環境中，使用的資料庫，而且您將部署至預備環境，並那里測試，再建立及部署到生產環境。</span><span class="sxs-lookup"><span data-stu-id="63d47-126">In this section of the tutorial you'll create a web app and database to use for the staging environment, and you'll deploy to staging and test there before creating and deploying to the production environment.</span></span>

> [!NOTE]
> <span data-ttu-id="63d47-127">下列步驟示範如何使用 Azure 管理入口網站，在 Azure App Service 中建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="63d47-127">The following steps show how to create a web app in Azure App Service by using the Azure management portal.</span></span> <span data-ttu-id="63d47-128">在 Azure SDK 最新版本中，您也可以執行這不需要離開 Visual Studio 中，使用 [伺服器總管]。</span><span class="sxs-lookup"><span data-stu-id="63d47-128">In the latest version of the Azure SDK, you can also do this without leaving Visual Studio, by using Server Explorer.</span></span> <span data-ttu-id="63d47-129">在 Visual Studio 2013 中，您也可以直接從 [發佈] 對話方塊，以建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="63d47-129">In Visual Studio 2013, you can also create a web app directly from the Publish dialog.</span></span> <span data-ttu-id="63d47-130">如需詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式。](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)</span><span class="sxs-lookup"><span data-stu-id="63d47-130">For more information, see [Create an ASP.NET web app in Azure App Service.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)</span></span>


1. <span data-ttu-id="63d47-131">在  [Azure 管理入口網站](https://manage.windowsazure.com/)，按一下**網站**，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="63d47-131">In the [Azure Management Portal](https://manage.windowsazure.com/), click **Websites**, and then click **New**.</span></span>
2. <span data-ttu-id="63d47-132">按一下 **網站**，然後按一下**自訂建立**。</span><span class="sxs-lookup"><span data-stu-id="63d47-132">Click **Website**, and then click **Custom Create**.</span></span>

    <span data-ttu-id="63d47-133">**新的網站-自訂建立**精靈 隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="63d47-133">The **New Website - Custom Create** wizard opens.</span></span> <span data-ttu-id="63d47-134">**自訂建立**精靈可讓您在同一時間建立網站和資料庫。</span><span class="sxs-lookup"><span data-stu-id="63d47-134">The **Custom Create** wizard enables you to create a web site and a database at the same time.</span></span>
3. <span data-ttu-id="63d47-135">在 **建立網站**步驟的精靈中，輸入字串**URL**方塊，以使用唯一的 URL 做為您的應用程式的預備環境。</span><span class="sxs-lookup"><span data-stu-id="63d47-135">In the **Create Website** step of the wizard, enter a string in the **URL** box to use as the unique URL for your application's staging environment.</span></span> <span data-ttu-id="63d47-136">例如，輸入 ContosoUniversity staging123 （包括使其成為唯一萬一 ContosoUniversity 預備環境會在結尾的隨機數字）。</span><span class="sxs-lookup"><span data-stu-id="63d47-136">For example, enter ContosoUniversity-staging123 (including random numbers at the end to make it unique in case ContosoUniversity-staging is taken).</span></span>

    <span data-ttu-id="63d47-137">完整的 URL 將會包含您在此處輸入，加上尾碼，您會看到的文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="63d47-137">The complete URL will consist of what you enter here plus the suffix that you see next to the text box.</span></span>
4. <span data-ttu-id="63d47-138">在 **地區**下拉式清單中，選擇離您最近的區域。</span><span class="sxs-lookup"><span data-stu-id="63d47-138">In the **Region** drop-down list, choose the region that is closest to you.</span></span>

    <span data-ttu-id="63d47-139">此設定會指定 web 應用程式將執行中的資料中心。</span><span class="sxs-lookup"><span data-stu-id="63d47-139">This setting specifies which data center your web app will run in.</span></span>
5. <span data-ttu-id="63d47-140">在 **資料庫**下拉式清單中，選擇**建立新的 SQL database**。</span><span class="sxs-lookup"><span data-stu-id="63d47-140">In the **Database** drop-down list, choose **Create a new SQL database**.</span></span>
6. <span data-ttu-id="63d47-141">在  **DB 連接字串名稱**方塊中，保留預設值為*DefaultConnection*。</span><span class="sxs-lookup"><span data-stu-id="63d47-141">In the **DB Connection String Name** box, leave the default value, *DefaultConnection*.</span></span>
7. <span data-ttu-id="63d47-142">按一下底部的方塊右邊的箭號。</span><span class="sxs-lookup"><span data-stu-id="63d47-142">Click the arrow that points to the right at the bottom of the box.</span></span>

    <span data-ttu-id="63d47-143">如下圖所示**建立網站**範例值 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="63d47-143">The following illustration shows the **Create Website** dialog with sample values in it.</span></span> <span data-ttu-id="63d47-144">URL 和您所輸入的區域將會不同。</span><span class="sxs-lookup"><span data-stu-id="63d47-144">The URL and Region that you have entered will be different.</span></span>

    ![建立網站的步驟](deploying-to-production/_static/image1.png)

    <span data-ttu-id="63d47-146">精靈會進入**指定的資料庫設定**步驟。</span><span class="sxs-lookup"><span data-stu-id="63d47-146">The wizard advances to the **Specify database settings** step.</span></span>
8. <span data-ttu-id="63d47-147">在 **名稱**方塊中，輸入*ContosoUniversity*再加上隨機的數字，使其成為唯一，例如*ContosoUniversity123*。</span><span class="sxs-lookup"><span data-stu-id="63d47-147">In the **Name** box, enter *ContosoUniversity* plus a random number to make it unique, for example *ContosoUniversity123*.</span></span>
9. <span data-ttu-id="63d47-148">在 **伺服器**方塊中，選取**新的 SQL Database 伺服器**。</span><span class="sxs-lookup"><span data-stu-id="63d47-148">In the **Server** box, select **New SQL Database Server**.</span></span>
10. <span data-ttu-id="63d47-149">輸入系統管理員名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="63d47-149">Enter an administrator name and password.</span></span>

    <span data-ttu-id="63d47-150">您不輸入現有的名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="63d47-150">You aren't entering an existing name and password here.</span></span> <span data-ttu-id="63d47-151">您輸入新名稱和您定義現在以供未來存取資料庫時使用的密碼。</span><span class="sxs-lookup"><span data-stu-id="63d47-151">You're entering a new name and password that you're defining now to use later when you access the database.</span></span>
11. <span data-ttu-id="63d47-152">在 **地區**方塊中，選擇您為 web 應用程式的相同區域。</span><span class="sxs-lookup"><span data-stu-id="63d47-152">In the **Region** box, choose the same region that you chose for the web app.</span></span>

    <span data-ttu-id="63d47-153">將 web 伺服器和資料庫伺服器保留在相同的區域提供您最佳的效能，並將費用降至最低。</span><span class="sxs-lookup"><span data-stu-id="63d47-153">Keeping the web server and the database server in the same region gives you the best performance and minimizes expenses.</span></span>
12. <span data-ttu-id="63d47-154">按一下底部的方塊以表示您已完成的核取記號。</span><span class="sxs-lookup"><span data-stu-id="63d47-154">Click the check mark at the bottom of the box to indicate that you're finished.</span></span>

    <span data-ttu-id="63d47-155">如下圖所示**指定的資料庫設定**範例值 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="63d47-155">The following illustration shows the **Specify database settings** dialog with sample values in it.</span></span> <span data-ttu-id="63d47-156">您輸入的值可能會不同。</span><span class="sxs-lookup"><span data-stu-id="63d47-156">The values you have entered may be different.</span></span>

    ![資料庫的設定步驟的 新增網站-建立資料庫精靈](deploying-to-production/_static/image2.png)

    <span data-ttu-id="63d47-158">管理入口網站會回到 網站 頁面中，而**狀態**欄顯示 正在建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="63d47-158">The Management Portal returns to the Websites page, and the **Status** column shows that the web app is being created.</span></span> <span data-ttu-id="63d47-159">一段時間之後 （通常少於一分鐘），**狀態**資料行會顯示已成功建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="63d47-159">After a while (typically less than a minute), the **Status** column shows that the web app was successfully created.</span></span> <span data-ttu-id="63d47-160">在左側導覽列中的 web 應用程式，您會在您的帳戶數目旁會出現**網站**圖示，然後資料庫數目旁會出現**SQL 資料庫**圖示。</span><span class="sxs-lookup"><span data-stu-id="63d47-160">In the navigation bar at the left, the number of web apps you have in your account appears next to the **Websites** icon, and the number of databases appears next to the **SQL Databases** icon.</span></span>

    ![管理入口網站中，建立網站的 [網站] 頁面](deploying-to-production/_static/image3.png)

    <span data-ttu-id="63d47-162">您的 web 應用程式名稱會不同於在圖例中的範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="63d47-162">Your web app name will be different from the example app in the illustration.</span></span>

## <a name="deploy-the-application-to-staging"></a><span data-ttu-id="63d47-163">部署至預備環境的應用程式</span><span class="sxs-lookup"><span data-stu-id="63d47-163">Deploy the application to staging</span></span>

<span data-ttu-id="63d47-164">既然您已建立 web 應用程式和預備環境的資料庫，您可以部署到它的專案。</span><span class="sxs-lookup"><span data-stu-id="63d47-164">Now that you have created a web app and database for the staging environment, you can deploy the project to it.</span></span>

> [!NOTE]
> <span data-ttu-id="63d47-165">這些指示說明如何建立發行設定檔下載 *.publishsettings*檔案，這不只適用於 Azure，也適用於協力廠商主機服務提供者。</span><span class="sxs-lookup"><span data-stu-id="63d47-165">These instructions show how to create a publish profile by downloading a *.publishsettings* file, which works not only for Azure but also for third-party hosting providers.</span></span> <span data-ttu-id="63d47-166">最新版的 Azure SDK 也可讓您直接連接至 Azure，從 Visual Studio，從您在您的 Azure 帳戶的 web 應用程式清單中選擇。</span><span class="sxs-lookup"><span data-stu-id="63d47-166">The latest Azure SDK also enables you to connect directly to Azure from Visual Studio, and choose from a list of web apps that you have in your Azure account.</span></span> <span data-ttu-id="63d47-167">在 Visual Studio 2013 中，您可以登入 Azure **Web Publish**對話方塊或從**伺服器總管**視窗。</span><span class="sxs-lookup"><span data-stu-id="63d47-167">In Visual Studio 2013, you can sign in to Azure from the **Web Publish** dialog or from the **Server Explorer** window.</span></span> <span data-ttu-id="63d47-168">如需詳細資訊，請參閱 < [Azure App Service 中建立 ASP.NET web 應用程式](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="63d47-168">For more information, see [Create an ASP.NET web app in Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span>


### <a name="download-the-publishsettings-file"></a><span data-ttu-id="63d47-169">下載.publishsettings 檔案</span><span class="sxs-lookup"><span data-stu-id="63d47-169">Download the .publishsettings file</span></span>

1. <span data-ttu-id="63d47-170">按一下您剛才建立的 web 應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="63d47-170">Click the name of the web app that you just created.</span></span>

    ![按一下以移至儀表板網站](deploying-to-production/_static/image4.png)
2. <span data-ttu-id="63d47-172">底下**概覽**中**儀表板**索引標籤上，按一下 **下載發行設定檔**。</span><span class="sxs-lookup"><span data-stu-id="63d47-172">Under **Quick glance** in the **Dashboard** tab, click **Download publish profile**.</span></span>

    ![下載發行設定檔連結](deploying-to-production/_static/image5.png)

    <span data-ttu-id="63d47-174">此步驟中下載檔案，其中包含所有您需要以應用程式部署至您的 web 應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="63d47-174">This step downloads a file that contains all of the settings that you need in order to deploy an application to your web app.</span></span> <span data-ttu-id="63d47-175">您將此檔案匯入到 Visual Studio 讓您不必手動輸入此資訊。</span><span class="sxs-lookup"><span data-stu-id="63d47-175">You'll import this file into Visual Studio so you don't have to enter this information manually.</span></span>
3. <span data-ttu-id="63d47-176">儲存 *.publishsettings*檔案的資料夾中，您可以從 Visual Studio 存取。</span><span class="sxs-lookup"><span data-stu-id="63d47-176">Save the *.publishsettings* file in a folder that you can access from Visual Studio.</span></span>

    ![儲存.publishsettings 檔案](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > <span data-ttu-id="63d47-178">安全性- *.publishsettings*檔案包含您的認證 （未編碼），用來管理您的 Azure 訂用帳戶和服務。</span><span class="sxs-lookup"><span data-stu-id="63d47-178">Security - The *.publishsettings* file contains your credentials (unencoded) that are used to administer your Azure subscriptions and services.</span></span> <span data-ttu-id="63d47-179">此檔案的安全性最佳作法是暫時儲存 （例如在 Libraries\Documents 資料夾），在來源目錄之外，並匯入完成後再將它刪除。</span><span class="sxs-lookup"><span data-stu-id="63d47-179">The security best practice for this file is to store it temporarily outside your source directories (for example in the Libraries\Documents folder), and then delete it once the import has completed.</span></span> <span data-ttu-id="63d47-180">惡意使用者取得存取權 *.publishsettings*檔案可以編輯、 建立和刪除您的 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="63d47-180">A malicious user who gains access to the *.publishsettings* file can edit, create, and delete your Azure services.</span></span>

### <a name="create-a-publish-profile"></a><span data-ttu-id="63d47-181">建立發行設定檔</span><span class="sxs-lookup"><span data-stu-id="63d47-181">Create a publish profile</span></span>

1. <span data-ttu-id="63d47-182">在 Visual Studio 中，以滑鼠右鍵按一下 ContosoUniversity 中的專案**方案總管**，然後選取**發佈**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="63d47-182">In Visual Studio, right-click the ContosoUniversity project in **Solution Explorer** and select **Publish** from the context menu.</span></span>

    <span data-ttu-id="63d47-183">**發佈 Web**精靈 隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="63d47-183">The **Publish Web** wizard opens.</span></span>
2. <span data-ttu-id="63d47-184">按一下 [**設定檔**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="63d47-184">Click the **Profile** tab.</span></span>
3. <span data-ttu-id="63d47-185">按一下 [匯入] 。</span><span class="sxs-lookup"><span data-stu-id="63d47-185">Click **Import**.</span></span>
4. <span data-ttu-id="63d47-186">瀏覽至 *.publishsettings*您稍早下載的檔案，然後按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="63d47-186">Navigate to the *.publishsettings* file you downloaded earlier, and then click **Open**.</span></span>

    ![匯入發行設定 對話方塊](deploying-to-production/_static/image7.png)
5. <span data-ttu-id="63d47-188">在 **連接**索引標籤上，按一下**驗證連線**藉此確定設定正確無誤。</span><span class="sxs-lookup"><span data-stu-id="63d47-188">In the **Connection** tab, click **Validate Connection** to make sure that the settings are correct.</span></span>

    <span data-ttu-id="63d47-189">當已驗證的連接時旁, 顯示綠色的核取記號**驗證連線** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="63d47-189">When the connection has been validated, a green check mark is shown next to the **Validate Connection** button.</span></span>

    <span data-ttu-id="63d47-190">對於某些裝載的提供者，當您按一下 [**驗證連線**，您可能會看到**憑證錯誤**] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="63d47-190">For some hosting providers, when you click **Validate Connection**, you might see a **Certificate Error** dialog box.</span></span> <span data-ttu-id="63d47-191">如果您這樣做，請確認伺服器名稱是您的預期。</span><span class="sxs-lookup"><span data-stu-id="63d47-191">If you do, verify that the server name is what you expect.</span></span> <span data-ttu-id="63d47-192">如果伺服器名稱正確，請選取**儲存此憑證以供未來的工作階段的 Visual Studio**然後按一下**接受**。</span><span class="sxs-lookup"><span data-stu-id="63d47-192">If the server name is correct, select **Save this certificate for future sessions of Visual Studio** and click **Accept**.</span></span> <span data-ttu-id="63d47-193">（此錯誤表示主機服務提供者已選擇來避免針對您要部署的 URL 中購買 SSL 憑證的費用。</span><span class="sxs-lookup"><span data-stu-id="63d47-193">(This error means that the hosting provider has chosen to avoid the expense of purchasing an SSL certificate for the URL that you are deploying to.</span></span> <span data-ttu-id="63d47-194">如果您想要使用的有效憑證來建立安全連接，請連絡您的主機服務提供者。）</span><span class="sxs-lookup"><span data-stu-id="63d47-194">If you prefer to establish a secure connection by using a valid certificate, contact your hosting provider.)</span></span>
6. <span data-ttu-id="63d47-195">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="63d47-195">Click **Next**.</span></span>

    ![連接成功圖示，然後在 連線 索引標籤中的下一步 按鈕](deploying-to-production/_static/image8.png)
7. <span data-ttu-id="63d47-197">在**設定**索引標籤上，展開**檔案發行選項**，然後選取**排除應用程式中的檔案\_Data 資料夾**。</span><span class="sxs-lookup"><span data-stu-id="63d47-197">In the **Settings** tab, expand **File Publish Options**, and then select **Exclude files from the App\_Data folder**.</span></span>

    <span data-ttu-id="63d47-198">如需其他選項下,**檔案發佈選項**，請參閱[部署至 IIS](deploying-to-iis.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="63d47-198">For information about the other options under **File Publish Options**, see the [deploying to IIS](deploying-to-iis.md) tutorial.</span></span> <span data-ttu-id="63d47-199">螢幕擷取畫面的顯示此步驟以及下列資料庫的設定步驟的結果是資料庫的組態步驟的結尾。</span><span class="sxs-lookup"><span data-stu-id="63d47-199">The screen shot that shows the result of this step and the following database configuration steps is at the end of the database configuration steps.</span></span>
8. <span data-ttu-id="63d47-200">底下**DefaultConnection**中**資料庫**區段中，設定成員資格資料庫的資料庫部署。</span><span class="sxs-lookup"><span data-stu-id="63d47-200">Under **DefaultConnection** in the **Databases** section, configure database deployment for the membership database.</span></span>
9. 1. <span data-ttu-id="63d47-201">選取 **更新資料庫**。</span><span class="sxs-lookup"><span data-stu-id="63d47-201">Select **Update database**.</span></span>

        <span data-ttu-id="63d47-202">**遠端的連接字串**下方的方塊**DefaultConnection** .publishsettings 檔案的連接字串會填入。連接字串包含會儲存在以純文字的 SQL Server 認證 *.pubxml*檔案。</span><span class="sxs-lookup"><span data-stu-id="63d47-202">The **Remote connection string** box directly below **DefaultConnection** is filled in with the connection string from the .publishsettings file.The connection string includes SQL Server credentials, which are stored in plain text in the *.pubxml* file.</span></span> <span data-ttu-id="63d47-203">如果您不想將它們儲存到永久那里，您可以在部署資料庫之後，從發行設定檔移除它們，並將它們儲存在 Azure 中。</span><span class="sxs-lookup"><span data-stu-id="63d47-203">If you prefer not to store them permanently there, you can remove them from the publish profile after the database is deployed and store them in Azure instead.</span></span> <span data-ttu-id="63d47-204">如需詳細資訊，請參閱 <<c0> [ 如何保護您的 ASP.NET 資料庫連接字串從來源部署至 Azure 時](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx)Scott Hanselman 的部落格上。</span><span class="sxs-lookup"><span data-stu-id="63d47-204">For more information, see [How to keep your ASP.NET database connection strings secure when deploying to Azure from Source](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) on Scott Hanselman's blog.</span></span>
      2. <span data-ttu-id="63d47-205">按一下 **設定資料庫更新**。</span><span class="sxs-lookup"><span data-stu-id="63d47-205">Click **Configure database updates**.</span></span>
      3. <span data-ttu-id="63d47-206">在 [**設定資料庫更新**] 對話方塊中，按一下**新增 SQL 指令碼**。</span><span class="sxs-lookup"><span data-stu-id="63d47-206">In the **Configure Database Updates** dialog box, click **Add SQL Script**.</span></span>
      4. <span data-ttu-id="63d47-207">在 **新增 SQL 指令碼**方塊中，瀏覽至*aspnet-資料-prod.sql*指令碼，您在方案資料夾中，稍早儲存，然後按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="63d47-207">In the **Add SQL Script** box, navigate to the *aspnet-data-prod.sql* script that you saved earlier in the solution folder, and then click **Open**.</span></span>
      5. <span data-ttu-id="63d47-208">關閉**設定資料庫更新** 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="63d47-208">Close the **Configure Database Updates** dialog box.</span></span>
10. <span data-ttu-id="63d47-209">底下**SchoolContext**中**資料庫**區段中，選取**Execute Code First Migrations （在應用程式啟動時執行）**。</span><span class="sxs-lookup"><span data-stu-id="63d47-209">Under **SchoolContext** in the **Databases** section, select **Execute Code First Migrations (runs on application start)**.</span></span>

    <span data-ttu-id="63d47-210">Visual Studio 會顯示**Execute Code First Migrations**而不是**更新資料庫**如`DbContext`類別。</span><span class="sxs-lookup"><span data-stu-id="63d47-210">Visual Studio displays **Execute Code First Migrations** instead of **Update Database** for `DbContext` classes.</span></span> <span data-ttu-id="63d47-211">如果您想要的 dbDacFx 提供者而非移轉，若要部署的資料庫，您可以使用存取`DbContext`類別，請參閱[如何部署沒有移轉的 Code First 資料庫？](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations)中適用於 Visual Studio Web 部署常見問題集和 MSDN 上的 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="63d47-211">If you want to use the dbDacFx provider instead of Migrations to deploy a database that you access by using a `DbContext` class, see [How do I deploy a Code First database without Migrations?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) in the Web Deployment FAQ for Visual Studio and ASP.NET on MSDN.</span></span>

    <span data-ttu-id="63d47-212">**設定** 索引標籤現在看起來如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="63d47-212">The **Settings** tab now looks like the following example:</span></span>

    ![預備環境的 [設定] 索引標籤](deploying-to-production/_static/image9.png)
11. <span data-ttu-id="63d47-214">執行下列步驟，以儲存設定檔，並將它重新命名*臨時*:</span><span class="sxs-lookup"><span data-stu-id="63d47-214">Perform the following steps to save the profile and rename it to *Staging*:</span></span>

    1. <span data-ttu-id="63d47-215">按一下 **設定檔**索引標籤，然後再按一下**管理設定檔**。</span><span class="sxs-lookup"><span data-stu-id="63d47-215">Click the **Profile** tab, and then click **Manage Profiles**.</span></span>
    2. <span data-ttu-id="63d47-216">匯入建立兩個新的設定檔，一個用於 FTP，一個用於 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="63d47-216">The import created two new profiles, one for FTP and one for Web Deploy.</span></span> <span data-ttu-id="63d47-217">設定 Web Deploy 的設定檔： 重新命名此設定檔*臨時*。</span><span class="sxs-lookup"><span data-stu-id="63d47-217">You configured the Web Deploy profile: rename this profile to *Staging*.</span></span>

        ![重新命名設定檔至預備環境](deploying-to-production/_static/image10.png)
    3. <span data-ttu-id="63d47-219">關閉**編輯 Web 發行設定檔** 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="63d47-219">Close the **Edit Web Publish Profiles** dialog box.</span></span>
    4. <span data-ttu-id="63d47-220">關閉**發佈 Web**精靈。</span><span class="sxs-lookup"><span data-stu-id="63d47-220">Close the **Publish Web** wizard.</span></span>

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a><span data-ttu-id="63d47-221">設定環境指示器的發行設定檔轉換</span><span class="sxs-lookup"><span data-stu-id="63d47-221">Configure a publish profile transform for the environment indicator</span></span>

> [!NOTE]
> <span data-ttu-id="63d47-222">本節說明如何設定環境指標 Web.config 轉換。</span><span class="sxs-lookup"><span data-stu-id="63d47-222">This section shows how to set up a Web.config transform for the environment indicator.</span></span> <span data-ttu-id="63d47-223">因為指標位於`<appSettings>`項目，您有指定的轉換，當您部署至 Azure App Service 的另一個替代方式。</span><span class="sxs-lookup"><span data-stu-id="63d47-223">Because the indicator is in the `<appSettings>` element, you have another alternative for specifying the transformation when you're deploying to Azure App Service.</span></span> <span data-ttu-id="63d47-224">如需詳細資訊，請參閱 <<c0> [ 在 Azure 中的指定 Web.config 設定](web-config-transformations.md#watransforms)。</span><span class="sxs-lookup"><span data-stu-id="63d47-224">For more information, see [Specifying Web.config settings in Azure](web-config-transformations.md#watransforms).</span></span>


1. <span data-ttu-id="63d47-225">在 [**方案總管] 中**，展開**屬性**，然後展開**PublishProfiles**。</span><span class="sxs-lookup"><span data-stu-id="63d47-225">In **Solution Explorer**, expand **Properties**, and then expand **PublishProfiles**.</span></span>
2. <span data-ttu-id="63d47-226">以滑鼠右鍵按一下*Staging.pubxml*，然後按一下**新增組態轉換**。</span><span class="sxs-lookup"><span data-stu-id="63d47-226">Right-click *Staging.pubxml*, and then click **Add Config Transform**.</span></span>

    ![新增設定轉換的預備環境](deploying-to-production/_static/image11.png)

    <span data-ttu-id="63d47-228">Visual Studio 會建立*Web.Staging.config*轉換檔案並開啟它。</span><span class="sxs-lookup"><span data-stu-id="63d47-228">Visual Studio creates the *Web.Staging.config* transform file and opens it.</span></span>
3. <span data-ttu-id="63d47-229">在  *Web.Staging.config*轉換檔中，開啟之後，立即插入下列程式碼`configuration`標記。</span><span class="sxs-lookup"><span data-stu-id="63d47-229">In the *Web.Staging.config* transform file, insert the following code immediately after the opening `configuration` tag.</span></span>

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    <span data-ttu-id="63d47-230">當您使用預備發行設定檔時，這項轉換就會設定到 「 生產 」 環境指標。</span><span class="sxs-lookup"><span data-stu-id="63d47-230">When you use the Staging publish profile, this transform sets the environment indicator to "Prod".</span></span> <span data-ttu-id="63d47-231">在已部署的 web 應用程式中，您不會看到任何尾碼，例如"(Dev)"（測試） 」 之後的"Contoso University"H1 標題。</span><span class="sxs-lookup"><span data-stu-id="63d47-231">In the deployed web app, you won't see any suffix such as "(Dev)" or "(Test)" after the "Contoso University" H1 heading.</span></span>
4. <span data-ttu-id="63d47-232">以滑鼠右鍵按一下*Web.Staging.config*檔案，然後按一下**預覽轉換**藉此確定您編寫的轉換會產生預期的變更。</span><span class="sxs-lookup"><span data-stu-id="63d47-232">Right-click the *Web.Staging.config* file and click **Preview Transform** to make sure that the transform you coded produces the expected changes.</span></span>

    <span data-ttu-id="63d47-233">**Web.config 預覽** 視窗會顯示結果的同時套用*Web.Release.config*轉換並*Web.Staging.config*轉換。</span><span class="sxs-lookup"><span data-stu-id="63d47-233">The **Web.config Preview** window shows the result of applying both the *Web.Release.config* transforms and the *Web.Staging.config* transforms.</span></span>

### <a name="prevent-public-use-of-the-test-app"></a><span data-ttu-id="63d47-234">防止公用測試應用程式使用</span><span class="sxs-lookup"><span data-stu-id="63d47-234">Prevent public use of the test app</span></span>

<span data-ttu-id="63d47-235">預備應用程式的一個重要考量是，會即時在網際網路上，但您不想將它公開。</span><span class="sxs-lookup"><span data-stu-id="63d47-235">An important consideration for the staging app is that it will be live on the Internet, but you don't want the public to use it.</span></span> <span data-ttu-id="63d47-236">人會尋找並使用它的可能性降到最低，您可以使用一或多個下列方法：</span><span class="sxs-lookup"><span data-stu-id="63d47-236">To minimize the likelihood that people will find and use it, you can use one or more of the following methods:</span></span>

- <span data-ttu-id="63d47-237">設定防火牆規則，允許只能從您用來測試預備環境的 IP 位址存取預備應用程式。</span><span class="sxs-lookup"><span data-stu-id="63d47-237">Set firewall rules that allow access to the staging app only from IP addresses that you use to test staging.</span></span>
- <span data-ttu-id="63d47-238">使用無法猜測的隱藏的 URL。</span><span class="sxs-lookup"><span data-stu-id="63d47-238">Use an obfuscated URL that would be impossible to guess.</span></span>
- <span data-ttu-id="63d47-239">建立*robots.txt*檔案，以確保搜尋引擎在搜尋結果中，，不編目，測試應用程式和報表連結。</span><span class="sxs-lookup"><span data-stu-id="63d47-239">Create a *robots.txt* file to ensure that search engines will not crawl the test app and report links to it in search results.</span></span>

<span data-ttu-id="63d47-240">這些方法的第一個是最有效率的但因為它會要求您將部署到 Azure 雲端服務而不是 Azure App Service 未涵蓋在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="63d47-240">The first of these methods is the most effective but is not covered in this tutorial because it would require that you deploy to an Azure Cloud Service instead of Azure App Service.</span></span> <span data-ttu-id="63d47-241">如需有關雲端服務的詳細資訊和在 Azure 中的 IP 限制，請參閱[計算裝載選項所提供 Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me)並[封鎖特定 IP 位址存取 Web 角色](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx)。</span><span class="sxs-lookup"><span data-stu-id="63d47-241">For more information about Cloud Services and IP restrictions in Azure, see [Compute Hosting Options Provided by Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) and [Block Specific IP Addresses from Accessing a Web Role](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx).</span></span> <span data-ttu-id="63d47-242">如果您要部署到協力廠商主機服務提供者，請連絡提供者，以了解如何實作 IP 限制。</span><span class="sxs-lookup"><span data-stu-id="63d47-242">If you are deploying to a third-party hosting provider, contact the provider to find out how to implement IP restrictions.</span></span>

<span data-ttu-id="63d47-243">本教學課程中，您將建立*robots.txt*檔案。</span><span class="sxs-lookup"><span data-stu-id="63d47-243">For this tutorial, you'll create a *robots.txt* file.</span></span>

1. <span data-ttu-id="63d47-244">在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**加入新項目**。</span><span class="sxs-lookup"><span data-stu-id="63d47-244">In **Solution Explorer**, right-click the ContosoUniversity project and click **Add New Item**.</span></span>
2. <span data-ttu-id="63d47-245">建立新**文字檔**名為*robots.txt*，並將下列文字放在它：</span><span class="sxs-lookup"><span data-stu-id="63d47-245">Create a new **Text File** named *robots.txt*, and put the following text in it:</span></span>

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    <span data-ttu-id="63d47-246">`User-agent`一行告訴檔案中的規則適用於所有搜尋引擎 web 編目程式 （機器人），搜尋引擎和`Disallow`一行可讓您指定應該進行編目的網站上的任何頁面。</span><span class="sxs-lookup"><span data-stu-id="63d47-246">The `User-agent` line tells search engines that the rules in the file apply to all search engine web crawlers (robots), and the `Disallow` line specifies that no pages on the site should be crawled.</span></span>

    <span data-ttu-id="63d47-247">您希望搜尋引擎目錄生產應用程式，因此您需要從生產環境部署中排除此檔案。</span><span class="sxs-lookup"><span data-stu-id="63d47-247">You do want search engines to catalog your production app, so you need to exclude this file from production deployment.</span></span> <span data-ttu-id="63d47-248">若要執行，您會設定在生產環境中的設定發行設定檔，當您在建立時。</span><span class="sxs-lookup"><span data-stu-id="63d47-248">To do that, you'll configure a setting in the Production publish profile when you create it.</span></span>

### <a name="deploy-to-staging"></a><span data-ttu-id="63d47-249">部署至預備環境</span><span class="sxs-lookup"><span data-stu-id="63d47-249">Deploy to staging</span></span>

1. <span data-ttu-id="63d47-250">開啟**發佈 Web** Contoso 大學專案上按一下滑鼠右鍵，然後按一下精靈**發佈**。</span><span class="sxs-lookup"><span data-stu-id="63d47-250">Open the **Publish Web** wizard by right-clicking the Contoso University project and clicking **Publish**.</span></span>
2. <span data-ttu-id="63d47-251">請確定**臨時**已選取設定檔。</span><span class="sxs-lookup"><span data-stu-id="63d47-251">Make sure that the **Staging** profile is selected.</span></span>
3. <span data-ttu-id="63d47-252">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="63d47-252">Click **Publish**.</span></span>

    <span data-ttu-id="63d47-253">**輸出**視窗會顯示所執行的部署動作，並回報成功完成部署。</span><span class="sxs-lookup"><span data-stu-id="63d47-253">The **Output** window shows what deployment actions were taken and reports successful completion of the deployment.</span></span> <span data-ttu-id="63d47-254">預設瀏覽器會自動開啟已部署的 web 應用程式的 URL。</span><span class="sxs-lookup"><span data-stu-id="63d47-254">The default browser automatically opens to the URL of the deployed web app.</span></span>

## <a name="test-in-the-staging-environment"></a><span data-ttu-id="63d47-255">在預備環境中測試</span><span class="sxs-lookup"><span data-stu-id="63d47-255">Test in the staging environment</span></span>

<span data-ttu-id="63d47-256">請注意環境指標不存在 (沒有 「 （測試） 」 或"(Dev)"之後的 H1 標題，其中會顯示*Web.config*環境指標的轉換是否成功。</span><span class="sxs-lookup"><span data-stu-id="63d47-256">Notice that the environment indicator is absent (there is no "(Test)" or "(Dev)" after the H1 heading, which shows that the *Web.config* transformation for the environment indicator was successful.</span></span>

![首頁上的預備環境](deploying-to-production/_static/image12.png)

<span data-ttu-id="63d47-258">執行**學生**頁面，確認已部署的資料庫有任何學生。</span><span class="sxs-lookup"><span data-stu-id="63d47-258">Run the **Students** page to verify that the deployed database has no students.</span></span>

<span data-ttu-id="63d47-259">執行**講師**頁面，確認第一個程式碼植入資料庫與講師資料：</span><span class="sxs-lookup"><span data-stu-id="63d47-259">Run the **Instructors** page to verify that Code First seeded the database with instructor data:</span></span>

<span data-ttu-id="63d47-260">選取 **新增的學生**從**學生**功能表上，新增為學生，，然後檢視 在新的學生**學生**頁面，確認您可以成功地寫入至資料庫.</span><span class="sxs-lookup"><span data-stu-id="63d47-260">Select **Add Students** from the **Students** menu, add a student, and then view the new student in the **Students** page to verify that you can successfully write to the database.</span></span>

<span data-ttu-id="63d47-261">從**課程**頁面上，按一下**更新信用額度**。</span><span class="sxs-lookup"><span data-stu-id="63d47-261">From the **Courses** page, click **Update Credits**.</span></span> <span data-ttu-id="63d47-262">**更新信用額度**頁面會要求系統管理員權限，所以**登入**頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="63d47-262">The **Update Credits** page requires administrator permissions, so the **Log In** page is displayed.</span></span> <span data-ttu-id="63d47-263">輸入您建立舊版 （"admin"和"prodpwd 」） 的系統管理員帳戶認證。</span><span class="sxs-lookup"><span data-stu-id="63d47-263">Enter the administrator account credentials that you created earlier ("admin" and "prodpwd").</span></span> <span data-ttu-id="63d47-264">**更新信用額度**顯示網頁時，會驗證您在上一個教學課程中建立的系統管理員帳戶已正確部署到測試環境。</span><span class="sxs-lookup"><span data-stu-id="63d47-264">The **Update Credits** page is displayed, which verifies that the administrator account that you created in the previous tutorial was correctly deployed to the test environment.</span></span>

<span data-ttu-id="63d47-265">要求無效的 URL，以引發錯誤，ELMAH 會追蹤，並接著要求 ELMAH 錯誤報告。</span><span class="sxs-lookup"><span data-stu-id="63d47-265">Request an invalid URL to cause an error that ELMAH will track, and then request the ELMAH error report.</span></span> <span data-ttu-id="63d47-266">如果您要部署到協力廠商主機服務提供者，您可能會發現基於相同原因，並將該空白先前的教學課程中，報告為空白。</span><span class="sxs-lookup"><span data-stu-id="63d47-266">If you are deploying to a third-party hosting provider, you will probably find that the report is empty for the same reason that it was empty in the previous tutorial.</span></span> <span data-ttu-id="63d47-267">您必須使用裝載提供者的帳戶管理工具來設定資料夾權限，以啟用要寫入的記錄檔資料夾的 ELMAH。</span><span class="sxs-lookup"><span data-stu-id="63d47-267">You will have to use the hosting provider's account management tools to configure folder permissions to enable ELMAH to write to the log folder.</span></span>

<span data-ttu-id="63d47-268">您所建立的應用程式現在正在雲端中就如同您會將它用於生產環境的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="63d47-268">The application that you created is now running in the cloud in a web app that is just like what you will use for production.</span></span> <span data-ttu-id="63d47-269">由於一切運作正常下, 一個步驟是部署到生產環境。</span><span class="sxs-lookup"><span data-stu-id="63d47-269">Since everything is working correctly, the next step is to deploy to production.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="63d47-270">部署到生產環境</span><span class="sxs-lookup"><span data-stu-id="63d47-270">Deploy to production</span></span>

<span data-ttu-id="63d47-271">建立生產 web 應用程式和部署至生產環境的程序是預備環境，與相同，不同之處在於您需要排除*robots.txt*從部署。</span><span class="sxs-lookup"><span data-stu-id="63d47-271">The process for creating a production web app and deploying to production is the same as for staging, except that you need to exclude the *robots.txt* from deployment.</span></span> <span data-ttu-id="63d47-272">若要這樣做，您將編輯發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="63d47-272">To do that you'll edit the publish profile file.</span></span>

### <a name="create-the-production-environment-and-the-production-publish-profile"></a><span data-ttu-id="63d47-273">建立生產環境和生產環境的發行設定檔</span><span class="sxs-lookup"><span data-stu-id="63d47-273">Create the production environment and the production publish profile</span></span>

1. <span data-ttu-id="63d47-274">在 Azure 中，遵循相同的程序，您用於預備環境中建立的生產 web 應用程式和資料庫。</span><span class="sxs-lookup"><span data-stu-id="63d47-274">Create the production web app and database in Azure, following the same procedure that you used for staging.</span></span>

    <span data-ttu-id="63d47-275">當您建立資料庫時，您可以選擇將它放在您稍早建立的相同伺服器上，或建立新的伺服器。</span><span class="sxs-lookup"><span data-stu-id="63d47-275">When you create the database, you can choose to put it on the same server you created earlier, or create a new server.</span></span>
2. <span data-ttu-id="63d47-276">下載 *.publishsettings*檔案。</span><span class="sxs-lookup"><span data-stu-id="63d47-276">Download the *.publishsettings* file.</span></span>
3. <span data-ttu-id="63d47-277">建立發行設定檔匯入生產 *.publishsettings*遵循相同的程序，您用於暫存的檔案。</span><span class="sxs-lookup"><span data-stu-id="63d47-277">Create the publish profile by importing the production *.publishsettings* file, following the same procedure that you used for staging.</span></span>

    <span data-ttu-id="63d47-278">別忘了設定底下的資料部署指令碼**DefaultConnection**中**資料庫**一節**設定** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="63d47-278">Don't forget to configure the data deployment script under **DefaultConnection** in the **Databases** section of the **Settings** tab.</span></span>
4. <span data-ttu-id="63d47-279">重新命名發行設定檔*生產*。</span><span class="sxs-lookup"><span data-stu-id="63d47-279">Rename the publish profile to *Production*.</span></span>
5. <span data-ttu-id="63d47-280">設定發行設定檔轉換環境指示器，遵循相同的程序，您用於預備環境...</span><span class="sxs-lookup"><span data-stu-id="63d47-280">Configure a publish profile transform for the environment indicator, following the same procedure that you used for staging..</span></span>

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a><span data-ttu-id="63d47-281">編輯.pubxml 檔案，以排除 robots.txt</span><span class="sxs-lookup"><span data-stu-id="63d47-281">Edit the .pubxml file to exclude robots.txt</span></span>

<span data-ttu-id="63d47-282">發行設定檔的檔案會命名為&lt;profilename&gt;*.pubxml*且位於*PublishProfiles*資料夾。</span><span class="sxs-lookup"><span data-stu-id="63d47-282">Publish profile files are named &lt;profilename&gt;*.pubxml* and are located in the *PublishProfiles* folder.</span></span> <span data-ttu-id="63d47-283">*PublishProfiles*的資料夾是下*屬性*資料夾中的 C# web 應用程式專案，在*我的專案*VB web 應用程式專案中或之下的資料夾*應用程式\_資料*資料夾中的 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="63d47-283">The *PublishProfiles* folder is under the *Properties* folder in a C# web application project, under the *My Project* folder in a VB web application project, or under the *App\_Data* folder in a web app project.</span></span> <span data-ttu-id="63d47-284">每個 *.pubxml*檔案包含套用至其中一個設定發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="63d47-284">Each *.pubxml* file contains settings that apply to one publish profile.</span></span> <span data-ttu-id="63d47-285">您在 [發行 Web] 精靈中輸入的值會儲存在這些檔案，以及您可以編輯它們以建立或變更不可在 Visual Studio UI 中的設定。</span><span class="sxs-lookup"><span data-stu-id="63d47-285">The values you enter in the Publish Web wizard are stored in these files, and you can edit them to create or change settings that aren't made available in the Visual Studio UI.</span></span>

<span data-ttu-id="63d47-286">根據預設， *.pubxml*檔案會包含在專案上，當您建立發行設定檔，但您可以排除專案和 Visual Studio 仍然使用它們。</span><span class="sxs-lookup"><span data-stu-id="63d47-286">By default, *.pubxml* files are included in the project when you create a publish profile, but you can exclude them from the project and Visual Studio will still use them.</span></span> <span data-ttu-id="63d47-287">Visual Studio 尋找*PublishProfiles*資料夾 *.pubxml*檔案，不論它們是否包含在專案中。</span><span class="sxs-lookup"><span data-stu-id="63d47-287">Visual Studio looks in the *PublishProfiles* folder for *.pubxml* files, regardless of whether they are included in the project.</span></span>

<span data-ttu-id="63d47-288">每個 *.pubxml*檔案沒有 *.pubxml.user*檔案。</span><span class="sxs-lookup"><span data-stu-id="63d47-288">For each *.pubxml* file there is a *.pubxml.user* file.</span></span> <span data-ttu-id="63d47-289">*.Pubxml.user*檔案包含加密的密碼，如果您選取**儲存密碼**選項，並依預設就會從專案排除。</span><span class="sxs-lookup"><span data-stu-id="63d47-289">The *.pubxml.user* file contains the encrypted password if you selected the **Save password** option, and by default it is excluded from the project.</span></span>

<span data-ttu-id="63d47-290">A *.pubxml*檔案包含屬於特定發行設定檔的設定。</span><span class="sxs-lookup"><span data-stu-id="63d47-290">A *.pubxml* file contains the settings that pertain to a specific publish profile.</span></span> <span data-ttu-id="63d47-291">如果您想要設定適用於所有的設定檔的設定，您可以建立 *.wpp.targets*檔案。</span><span class="sxs-lookup"><span data-stu-id="63d47-291">If you want to configure settings that apply to all profiles, you can create a *.wpp.targets* file.</span></span> <span data-ttu-id="63d47-292">建置程序匯入這些檔案放進 *.csproj*或是 *.vbproj*專案檔，因此大部分的設定，您可以設定專案檔中設定這些檔案中。</span><span class="sxs-lookup"><span data-stu-id="63d47-292">The build process imports these files into the *.csproj* or *.vbproj* project file, so most settings that you can configure in the project file can be configured in these files.</span></span> <span data-ttu-id="63d47-293">如需詳細資訊 *.pubxml*檔案並 *.wpp.targets*檔案，請參閱[How to： 編輯發行設定檔 (.pubxml) 檔中的部署設定和.wpp.targets 檔案在 Visual Studio 中Web 專案](https://msdn.microsoft.com/library/ff398069.aspx)。</span><span class="sxs-lookup"><span data-stu-id="63d47-293">For more information about *.pubxml* files and *.wpp.targets* files, see [How to: Edit Deployment Settings in Publish Profile (.pubxml) Files and the .wpp.targets File in Visual Studio Web Projects](https://msdn.microsoft.com/library/ff398069.aspx).</span></span>

1. <span data-ttu-id="63d47-294">在 [**方案總管] 中**，展開**屬性**展開**PublishProfiles**。</span><span class="sxs-lookup"><span data-stu-id="63d47-294">In **Solution Explorer**, expand **Properties** and expand **PublishProfiles**.</span></span>
2. <span data-ttu-id="63d47-295">以滑鼠右鍵按一下*Production.pubxml*然後按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="63d47-295">Right-click *Production.pubxml* and click **Open**.</span></span>

    ![開啟.pubxml 檔案](deploying-to-production/_static/image13.png)
3. <span data-ttu-id="63d47-297">以滑鼠右鍵按一下*Production.pubxml*然後按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="63d47-297">Right-click *Production.pubxml* and click **Open**.</span></span>
4. <span data-ttu-id="63d47-298">新增下列幾行的結束之前立即`PropertyGroup`項目：</span><span class="sxs-lookup"><span data-stu-id="63d47-298">Add the following lines immediately before the closing `PropertyGroup` element:</span></span>

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    <span data-ttu-id="63d47-299">專屬的.pubxml 檔案現在看起來如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="63d47-299">The .pubxml file now looks like the following example:</span></span>

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    <span data-ttu-id="63d47-300">如需如何排除檔案和資料夾的詳細資訊，請參閱[我是否能排除特定檔案或資料夾從部署？](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment)中**適用於 Visual Studio 和 ASP.NET Web 部署常見問題集**MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="63d47-300">For more information about how to exclude files and folders, see [Can I exclude specific files or folders from deployment?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) in the **Web Deployment FAQ for Visual Studio and ASP.NET** on MSDN.</span></span>

### <a name="deploy-to-production"></a><span data-ttu-id="63d47-301">部署到生產環境</span><span class="sxs-lookup"><span data-stu-id="63d47-301">Deploy to production</span></span>

1. <span data-ttu-id="63d47-302">開啟**發佈 Web**精靈，確定**生產**發行設定檔已選取，然後按一下**開始預覽**上**預覽** 索引標籤，確認*robots.txt*不將檔案複製到實際執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="63d47-302">Open the **Publish Web** wizard make sure that the **Production** publish profile is selected, and then click **Start Preview** on the **Preview** tab to verify that the *robots.txt* file will not be copied to the production app.</span></span>

    ![檔案發行至生產環境的預覽](deploying-to-production/_static/image14.png)

    <span data-ttu-id="63d47-304">檢閱將複製之檔案的清單。</span><span class="sxs-lookup"><span data-stu-id="63d47-304">Review the list of files that will be copied.</span></span> <span data-ttu-id="63d47-305">您會看到的所有 *.cs*檔案，包括 *。 aspx.cs*， *。 aspx.designer.cs*， *Master.cs*，和*Master.designer.cs*檔案會省略。</span><span class="sxs-lookup"><span data-stu-id="63d47-305">You'll see that all of the *.cs* files, including *.aspx.cs*, *.aspx.designer.cs*, *Master.cs*, and *Master.designer.cs* files are omitted.</span></span> <span data-ttu-id="63d47-306">這段程式碼編譯成*ContosoUniversity.dll*並*ContosUniversity.pdb*中找到的檔案*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="63d47-306">All of this code has been compiled into the *ContosoUniversity.dll* and *ContosUniversity.pdb* files that you'll find in the *bin* folder.</span></span> <span data-ttu-id="63d47-307">因為只有 *.dll*才能的執行應用程式，以及您稍早指定程式可部署到執行應用程式所需的檔案、 no *.cs*檔案複製到目的地環境。</span><span class="sxs-lookup"><span data-stu-id="63d47-307">Because only the *.dll* is needed to run the application, and you specified earlier that only files needed to run the application should be deployed, no *.cs* files were copied to the destination environment.</span></span> <span data-ttu-id="63d47-308">*Obj*資料夾並*ContosoUniversity.csproj*並 *.csproj.user*檔案已省略基於相同原因。</span><span class="sxs-lookup"><span data-stu-id="63d47-308">The *obj* folder and the *ContosoUniversity.csproj* and *.csproj.user* files are omitted for the same reason.</span></span>

    <span data-ttu-id="63d47-309">按一下 **發佈**来部署到生產環境。</span><span class="sxs-lookup"><span data-stu-id="63d47-309">Click **Publish** to deploy to the production environment.</span></span>
2. <span data-ttu-id="63d47-310">在生產環境中，遵循相同的程序，您用於預備環境中測試。</span><span class="sxs-lookup"><span data-stu-id="63d47-310">Test in production, following the same procedure you used for staging.</span></span>

    <span data-ttu-id="63d47-311">所有項目等同於 URL 除外的預備環境和缺乏*robots.txt*檔案。</span><span class="sxs-lookup"><span data-stu-id="63d47-311">Everything is identical to staging except for the URL and the absence of the *robots.txt* file.</span></span>

## <a name="summary"></a><span data-ttu-id="63d47-312">總結</span><span class="sxs-lookup"><span data-stu-id="63d47-312">Summary</span></span>

<span data-ttu-id="63d47-313">您現在已成功部署和測試您的 web 應用程式，它可公開在網際網路上。</span><span class="sxs-lookup"><span data-stu-id="63d47-313">You have now successfully deployed and tested your web app and it is available publicly over the Internet.</span></span>

![首頁上生產環境](deploying-to-production/_static/image15.png)

<span data-ttu-id="63d47-315">在下一個教學課程中，您將更新應用程式程式碼，並將變更部署到測試、 預備及生產環境。</span><span class="sxs-lookup"><span data-stu-id="63d47-315">In the next tutorial, you'll update application code and deploy the change to the test, staging, and production environments.</span></span>

> [!NOTE]
> <span data-ttu-id="63d47-316">在生產環境中使用您的應用程式時您應該實作復原計劃。</span><span class="sxs-lookup"><span data-stu-id="63d47-316">While your application is in use in the production environment you should be implementing a recovery plan.</span></span> <span data-ttu-id="63d47-317">也就是您應該會定期備份您的資料庫從生產環境應用程式到安全的儲存體位置，以及您應該保留數個層代的這種備份。</span><span class="sxs-lookup"><span data-stu-id="63d47-317">That is, you should be periodically backing up your databases from the production app to a secure storage location, and you should be keeping several generations of such backups.</span></span> <span data-ttu-id="63d47-318">當您更新資料庫時，您要立即在變更之前的備份複本。</span><span class="sxs-lookup"><span data-stu-id="63d47-318">When you update the database, you should make a backup copy from immediately before the change.</span></span> <span data-ttu-id="63d47-319">然後，如果發生錯誤，而不加以探索直到您已將它部署到生產環境之後，您仍然能夠將資料庫復原到其損毀前的狀態。</span><span class="sxs-lookup"><span data-stu-id="63d47-319">Then, if you make a mistake and don't discover it until after you have deployed it to production, you will still be able to recover the database to the state it was in before it became corrupted.</span></span> <span data-ttu-id="63d47-320">如需詳細資訊，請參閱 < [Azure SQL Database 備份和還原](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx)。</span><span class="sxs-lookup"><span data-stu-id="63d47-320">For more information, see [Azure SQL Database Backup and Restore](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).</span></span>
> 
> 
> [!NOTE]
> <span data-ttu-id="63d47-321">在本教學課程 SQL Server 版本，您要部署會是 Azure SQL Database。</span><span class="sxs-lookup"><span data-stu-id="63d47-321">In this tutorial the SQL Server edition that you are deploying to is Azure SQL Database.</span></span> <span data-ttu-id="63d47-322">部署程序類似於其他 SQL Server 版本時，實際的生產應用程式可能在某些情況下就為 Azure SQL Database 需要特殊的程式碼。</span><span class="sxs-lookup"><span data-stu-id="63d47-322">While the deployment process is similar to other editions of SQL Server, a real production application might require special code for Azure SQL Database in some scenarios.</span></span> <span data-ttu-id="63d47-323">如需詳細資訊，請參閱 <<c0> [ 熟悉 Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb)並[SQL Server 和 Azure SQL Database 之間進行選擇](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing)。</span><span class="sxs-lookup"><span data-stu-id="63d47-323">For more information, see [Working with Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) and [Choosing between SQL Server and Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="63d47-324">[上一頁](setting-folder-permissions.md)
> [下一頁](deploying-a-code-update.md)</span><span class="sxs-lookup"><span data-stu-id="63d47-324">[Previous](setting-folder-permissions.md)
[Next](deploying-a-code-update.md)</span></span>
