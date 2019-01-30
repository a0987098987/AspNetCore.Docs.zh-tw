---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: 使用 Entity Framework 6 的 Web API 2 |Microsoft Docs
author: MikeWasson
description: 本教學課程將教導您使用 ASP.NET Web API 建立 web 應用程式的基本概念後端。 本教學課程會使用 Entity Framework 6 的資料配置...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 266c808e3525787181038d2de473194989039e02
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236519"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="9d97f-104">使用 Web API 2 和 Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="9d97f-104">Using Web API 2 with Entity Framework 6</span></span>
====================

[<span data-ttu-id="9d97f-105">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="9d97f-105">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="9d97f-106">本教學課程將教導您使用 ASP.NET Web API 建立 web 應用程式的基本概念後端。</span><span class="sxs-lookup"><span data-stu-id="9d97f-106">This tutorial teaches you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="9d97f-107">本教學課程會使用資料層和 Knockout.js Entity Framework 6，用戶端 JavaScript 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d97f-107">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="9d97f-108">本教學課程也會示範如何將應用程式部署至 Azure App Service Web Apps。</span><span class="sxs-lookup"><span data-stu-id="9d97f-108">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9d97f-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="9d97f-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="9d97f-110">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="9d97f-110">Web API 2.1</span></span>
> - <span data-ttu-id="9d97f-111">Visual Studio 2017 (下載 Visual Studio 2017[此處](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="9d97f-111">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="9d97f-112">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="9d97f-112">Entity Framework 6</span></span>
> - <span data-ttu-id="9d97f-113">.NET 4.7</span><span class="sxs-lookup"><span data-stu-id="9d97f-113">.NET 4.7</span></span>
> - <span data-ttu-id="9d97f-114">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="9d97f-114">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="9d97f-115">本教學課程會使用 ASP.NET Web API 2 與 Entity Framework 6 建立 web 應用程式管理後端資料庫。</span><span class="sxs-lookup"><span data-stu-id="9d97f-115">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="9d97f-116">以下是您將建立的應用程式的螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="9d97f-116">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="9d97f-117">應用程式會使用單一頁面應用程式 (SPA) 設計。</span><span class="sxs-lookup"><span data-stu-id="9d97f-117">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="9d97f-118">「 單一頁面應用程式 」 是載入單一 HTML 頁面，並以動態方式，而不是載入新的頁面更新頁面的 web 應用程式的一般詞彙。</span><span class="sxs-lookup"><span data-stu-id="9d97f-118">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="9d97f-119">在初始網頁載入後，應用程式會與透過 AJAX 要求的伺服器。</span><span class="sxs-lookup"><span data-stu-id="9d97f-119">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="9d97f-120">AJAX 要求傳回的 JSON 資料，應用程式用來更新 UI。</span><span class="sxs-lookup"><span data-stu-id="9d97f-120">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="9d97f-121">AJAX 不是新的但是今天很輕鬆地建置及維護的大型複雜的 SPA 應用程式的 JavaScript 架構。</span><span class="sxs-lookup"><span data-stu-id="9d97f-121">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="9d97f-122">本教學課程會使用[Knockout.js](http://knockoutjs.com/)，但是您可以使用任何的 JavaScript 用戶端架構。</span><span class="sxs-lookup"><span data-stu-id="9d97f-122">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="9d97f-123">以下是此應用程式主要建置組塊：</span><span class="sxs-lookup"><span data-stu-id="9d97f-123">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="9d97f-124">ASP.NET MVC 建立 HTML 網頁。</span><span class="sxs-lookup"><span data-stu-id="9d97f-124">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="9d97f-125">ASP.NET Web API 處理 AJAX 要求，並傳回 JSON 資料。</span><span class="sxs-lookup"><span data-stu-id="9d97f-125">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="9d97f-126">Knockout.js 資料繫結的 HTML 項目至 JSON 資料。</span><span class="sxs-lookup"><span data-stu-id="9d97f-126">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="9d97f-127">Entity Framework 與資料庫交談。</span><span class="sxs-lookup"><span data-stu-id="9d97f-127">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="9d97f-128">請參閱在 Azure 上執行此應用程式</span><span class="sxs-lookup"><span data-stu-id="9d97f-128">See this app running on Azure</span></span>

<span data-ttu-id="9d97f-129">若要查看為即時 web 應用程式正在執行的完成的網站嗎？</span><span class="sxs-lookup"><span data-stu-id="9d97f-129">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="9d97f-130">您可以部署到您的 Azure 帳戶的完整版本的應用程式藉由選取下面的按鈕。</span><span class="sxs-lookup"><span data-stu-id="9d97f-130">You can deploy a complete version of the app to your Azure account by selecting the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="9d97f-131">您需要有 Azure 帳戶才能將此解決方案部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="9d97f-131">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="9d97f-132">如果您還沒有帳戶，您會有下列選項：</span><span class="sxs-lookup"><span data-stu-id="9d97f-132">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="9d97f-133">[免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度來試用 Azure 付費服務，您可以使用和甚至用後最多，您可保留帳戶，並使用免費的 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="9d97f-133">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="9d97f-134">[啟用 MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶信用額度每月都會提供您 Azure 付費服務，您可以使用。</span><span class="sxs-lookup"><span data-stu-id="9d97f-134">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="9d97f-135">建立專案</span><span class="sxs-lookup"><span data-stu-id="9d97f-135">Create the project</span></span>

<span data-ttu-id="9d97f-136">開啟 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="9d97f-136">Open Visual Studio.</span></span> <span data-ttu-id="9d97f-137">從**檔案**功能表上，選取**新增**，然後選取**專案**。</span><span class="sxs-lookup"><span data-stu-id="9d97f-137">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="9d97f-138">(或選取**新的專案**[開始] 頁面上。)</span><span class="sxs-lookup"><span data-stu-id="9d97f-138">(Or select **New Project** on the Start page.)</span></span>

<span data-ttu-id="9d97f-139">在**新的專案**對話方塊中，選取**Web**左窗格中並**ASP.NET Web 應用程式 (.NET Framework)** 在中間窗格中。</span><span class="sxs-lookup"><span data-stu-id="9d97f-139">In the **New Project** dialog, select **Web** in the left pane and **ASP.NET Web Application (.NET Framework)** in the middle pane.</span></span> <span data-ttu-id="9d97f-140">將專案命名為**BookService** ，然後選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="9d97f-140">Name the project **BookService** and select **OK**.</span></span>

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

<span data-ttu-id="9d97f-141">在 **新的 ASP.NET 專案**對話方塊中，選取**Web API**範本。</span><span class="sxs-lookup"><span data-stu-id="9d97f-141">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image12.png)](part-1/_static/image12.png)


<span data-ttu-id="9d97f-142">選取 [確定] 建立專案。</span><span class="sxs-lookup"><span data-stu-id="9d97f-142">Select **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="9d97f-143">設定 Azure 設定 （選擇性）</span><span class="sxs-lookup"><span data-stu-id="9d97f-143">Configure Azure settings (optional)</span></span>

<span data-ttu-id="9d97f-144">建立專案之後，您可以選擇在任何時間部署至 Azure App Service Web Apps。</span><span class="sxs-lookup"><span data-stu-id="9d97f-144">After you create the project, you can choose to deploy to Azure App Service Web Apps at any time.</span></span> 

1. <span data-ttu-id="9d97f-145">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**發佈**。</span><span class="sxs-lookup"><span data-stu-id="9d97f-145">In Solution Explorer, right-click on your project and select **Publish**.</span></span>

2. <span data-ttu-id="9d97f-146">在出現的視窗中，選取**啟動**。</span><span class="sxs-lookup"><span data-stu-id="9d97f-146">In the window that appears, select **Start**.</span></span> <span data-ttu-id="9d97f-147">**挑選發行目標** 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="9d97f-147">The **Pick a publish target** dialog box appears.</span></span>

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. <span data-ttu-id="9d97f-148">選取 [建立設定檔]。</span><span class="sxs-lookup"><span data-stu-id="9d97f-148">Select **Create Profile**.</span></span> <span data-ttu-id="9d97f-149">[建立 App Service] 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="9d97f-149">The **Create App Service** dialog box appears.</span></span>

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   <span data-ttu-id="9d97f-150">接受預設值，或輸入不同的值做為應用程式名稱，而裝載方案、 Azure 訂用帳戶和地理區域的資源群組。</span><span class="sxs-lookup"><span data-stu-id="9d97f-150">Accept the defaults, or enter different values for the application name, resource group, hosting plan, Azure subscription, and geographical region.</span></span> 

4. <span data-ttu-id="9d97f-151">選取 **建立 SQL database**。</span><span class="sxs-lookup"><span data-stu-id="9d97f-151">Select **Create a SQL database**.</span></span> <span data-ttu-id="9d97f-152">**設定 SQL Server**  對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="9d97f-152">The **Configure SQL Server** dialog box appears.</span></span> 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   <span data-ttu-id="9d97f-153">接受預設值，或輸入不同的值。</span><span class="sxs-lookup"><span data-stu-id="9d97f-153">Accept the defaults or enter different values.</span></span> <span data-ttu-id="9d97f-154">請輸入**系統管理員使用者名稱**並**系統管理員密碼**新資料庫。</span><span class="sxs-lookup"><span data-stu-id="9d97f-154">Enter an **Adminstrator Username** and **Administrator Password** for your new database.</span></span> <span data-ttu-id="9d97f-155">選取 **確定**完成時。</span><span class="sxs-lookup"><span data-stu-id="9d97f-155">Select **OK** when you're done.</span></span> <span data-ttu-id="9d97f-156">**建立 App Service**頁面隨即再度出現。</span><span class="sxs-lookup"><span data-stu-id="9d97f-156">The **Create App Service** page reappears.</span></span>

5. <span data-ttu-id="9d97f-157">選取 **建立**來建立您的設定檔。</span><span class="sxs-lookup"><span data-stu-id="9d97f-157">Select **Create** to create your profile.</span></span> <span data-ttu-id="9d97f-158">表示部署正在進行中右下角會出現一則訊息。</span><span class="sxs-lookup"><span data-stu-id="9d97f-158">A message appears in the lower-right corner indicating that deployment is in progress.</span></span> <span data-ttu-id="9d97f-159">在一段時間之後,**發佈**視窗隨即再度出現。</span><span class="sxs-lookup"><span data-stu-id="9d97f-159">After a short while, the **Publish** window reappears.</span></span>

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    <span data-ttu-id="9d97f-160">現在可使用您建立用來部署應用程式的設定檔。</span><span class="sxs-lookup"><span data-stu-id="9d97f-160">The profile you created to deploy the app is now available.</span></span> 


> [!div class="step-by-step"]
> [<span data-ttu-id="9d97f-161">下一步</span><span class="sxs-lookup"><span data-stu-id="9d97f-161">Next</span></span>](part-2.md)
