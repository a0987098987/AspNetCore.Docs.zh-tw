---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: 使用 Entity Framework 6 的 Web API 2 |Microsoft Docs
author: MikeWasson
description: 本教學課程將教導您使用 ASP.NET Web API 建立 web 應用程式的基本概念後端。 本教學課程會使用 Entity Framework 6 的資料配置...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: d65c0ea35ec766ef9d9093c6502230f9de72a3f3
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795206"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="5922d-104">使用 Web API 2 和 Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="5922d-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="5922d-105">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5922d-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="5922d-106">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="5922d-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="5922d-107">本教學課程將教導您使用 ASP.NET Web API 建立 web 應用程式的基本概念後端。</span><span class="sxs-lookup"><span data-stu-id="5922d-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="5922d-108">本教學課程會使用資料層和 Knockout.js Entity Framework 6，用戶端 JavaScript 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5922d-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="5922d-109">本教學課程也會示範如何將應用程式部署至 Azure App Service Web Apps。</span><span class="sxs-lookup"><span data-stu-id="5922d-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5922d-110">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="5922d-110">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="5922d-111">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="5922d-111">Web API 2.1</span></span>
> - <span data-ttu-id="5922d-112">Visual Studio 2013 (下載 Visual Studio 2017[此處](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="5922d-112">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="5922d-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="5922d-113">Entity Framework 6</span></span>
> - <span data-ttu-id="5922d-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5922d-114">.NET 4.5</span></span>
> - <span data-ttu-id="5922d-115">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="5922d-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="5922d-116">本教學課程會使用 ASP.NET Web API 2 與 Entity Framework 6 建立 web 應用程式管理後端資料庫。</span><span class="sxs-lookup"><span data-stu-id="5922d-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="5922d-117">以下是您將建立的應用程式的螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="5922d-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="5922d-118">應用程式會使用單一頁面應用程式 (SPA) 設計。</span><span class="sxs-lookup"><span data-stu-id="5922d-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="5922d-119">「 單一頁面應用程式 」 是載入單一 HTML 頁面，並以動態方式，而不是載入新的頁面更新頁面的 web 應用程式的一般詞彙。</span><span class="sxs-lookup"><span data-stu-id="5922d-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="5922d-120">在初始網頁載入後，應用程式會與透過 AJAX 要求的伺服器。</span><span class="sxs-lookup"><span data-stu-id="5922d-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="5922d-121">AJAX 要求傳回的 JSON 資料，應用程式用來更新 UI。</span><span class="sxs-lookup"><span data-stu-id="5922d-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="5922d-122">AJAX 不是新的但是今天很輕鬆地建置及維護的大型複雜的 SPA 應用程式的 JavaScript 架構。</span><span class="sxs-lookup"><span data-stu-id="5922d-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="5922d-123">本教學課程會使用[Knockout.js](http://knockoutjs.com/)，但是您可以使用任何的 JavaScript 用戶端架構。</span><span class="sxs-lookup"><span data-stu-id="5922d-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="5922d-124">以下是此應用程式主要建置組塊：</span><span class="sxs-lookup"><span data-stu-id="5922d-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="5922d-125">ASP.NET MVC 建立 HTML 網頁。</span><span class="sxs-lookup"><span data-stu-id="5922d-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="5922d-126">ASP.NET Web API 處理 AJAX 要求，並傳回 JSON 資料。</span><span class="sxs-lookup"><span data-stu-id="5922d-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="5922d-127">Knockout.js 資料繫結的 HTML 項目至 JSON 資料。</span><span class="sxs-lookup"><span data-stu-id="5922d-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="5922d-128">Entity Framework 與資料庫交談。</span><span class="sxs-lookup"><span data-stu-id="5922d-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="5922d-129">請參閱在 Azure 上執行此應用程式</span><span class="sxs-lookup"><span data-stu-id="5922d-129">See this App Running on Azure</span></span>

<span data-ttu-id="5922d-130">若要查看為即時 web 應用程式正在執行的完成的網站嗎？</span><span class="sxs-lookup"><span data-stu-id="5922d-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="5922d-131">您可以部署您的 Azure 帳戶的應用程式的完整版本，只要按一下下面的按鈕。</span><span class="sxs-lookup"><span data-stu-id="5922d-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="5922d-132">您需要有 Azure 帳戶才能將此解決方案部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="5922d-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="5922d-133">如果您還沒有帳戶，您會有下列選項：</span><span class="sxs-lookup"><span data-stu-id="5922d-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="5922d-134">[免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度來試用 Azure 付費服務，您可以使用和甚至用後最多，您可保留帳戶，並使用免費的 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="5922d-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="5922d-135">[啟用 MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶信用額度每月都會提供您 Azure 付費服務，您可以使用。</span><span class="sxs-lookup"><span data-stu-id="5922d-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="5922d-136">建立專案</span><span class="sxs-lookup"><span data-stu-id="5922d-136">Create the Project</span></span>

<span data-ttu-id="5922d-137">開啟 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="5922d-137">Open Visual Studio.</span></span> <span data-ttu-id="5922d-138">從**檔案**功能表上，選取**新增**，然後選取**專案**。</span><span class="sxs-lookup"><span data-stu-id="5922d-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="5922d-139">(或者按一下**新的專案**[開始] 頁面上。)</span><span class="sxs-lookup"><span data-stu-id="5922d-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="5922d-140">在**新的專案** 對話方塊中，按一下**Web**的左窗格中並**ASP.NET Web 應用程式**在中間窗格中。</span><span class="sxs-lookup"><span data-stu-id="5922d-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="5922d-141">專案 BookService 命名，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="5922d-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="5922d-142">在 **新的 ASP.NET 專案**對話方塊中，選取**Web API**範本。</span><span class="sxs-lookup"><span data-stu-id="5922d-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="5922d-143">如果您想要裝載在 Azure App Service 中的專案，將保留**雲端中的主機**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="5922d-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="5922d-144">按一下 [確定] 建立專案。</span><span class="sxs-lookup"><span data-stu-id="5922d-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="5922d-145">設定 Azure 設定 （選擇性）</span><span class="sxs-lookup"><span data-stu-id="5922d-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="5922d-146">如果您離開**雲端中的主機**核取選項，Visual Studio 會提示您登入 Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="5922d-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="5922d-147">登入 Azure 之後，Visual Studio 會提示您設定 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5922d-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="5922d-148">輸入網站的名稱，選取您 Azure 訂用帳戶，然後選取地理區域。</span><span class="sxs-lookup"><span data-stu-id="5922d-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="5922d-149">底下**資料庫伺服器**，選取**建立新的伺服器**。</span><span class="sxs-lookup"><span data-stu-id="5922d-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="5922d-150">輸入系統管理員使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="5922d-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="5922d-151">下一步</span><span class="sxs-lookup"><span data-stu-id="5922d-151">Next</span></span>](part-2.md)
