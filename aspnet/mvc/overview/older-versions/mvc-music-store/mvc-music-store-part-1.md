---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 第 1 部分： 概觀和檔案]-> [新增專案 |Microsoft 文件
author: jongalloway
description: 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 1 部分涵蓋概觀和檔案]-> [新的專案。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868967"
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="34478-104">第 1 部分： 概觀和檔案]-> [新增專案</span><span class="sxs-lookup"><span data-stu-id="34478-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="34478-105">由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="34478-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="34478-106">MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="34478-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="34478-107">MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。</span><span class="sxs-lookup"><span data-stu-id="34478-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="34478-108">此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="34478-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="34478-109">第 1 部分涵蓋的概觀和檔案-&gt;新的專案。</span><span class="sxs-lookup"><span data-stu-id="34478-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="34478-110">總覽</span><span class="sxs-lookup"><span data-stu-id="34478-110">Overview</span></span>

<span data-ttu-id="34478-111">MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Web Developer web 程式開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="34478-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="34478-112">我們將會啟動慢，因此初學者層級的 web 程式開發體驗也沒關係。</span><span class="sxs-lookup"><span data-stu-id="34478-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="34478-113">我們會建置應用程式是一個簡單的音樂存放區。</span><span class="sxs-lookup"><span data-stu-id="34478-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="34478-114">有三個應用程式的主要部分： 購物、 簽出，以及系統管理。</span><span class="sxs-lookup"><span data-stu-id="34478-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="34478-115">訪客可以瀏覽相簿的影片：</span><span class="sxs-lookup"><span data-stu-id="34478-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="34478-116">他們可以檢視單張專輯，並將它加入至購物車：</span><span class="sxs-lookup"><span data-stu-id="34478-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="34478-117">它們可以檢閱其購物車，並移除不再需要的任何項目：</span><span class="sxs-lookup"><span data-stu-id="34478-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="34478-118">繼續簽出將會提示他們登入或註冊的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="34478-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="34478-119">建立帳戶後，它們可以透過填寫 傳送和付款資訊完成順序。</span><span class="sxs-lookup"><span data-stu-id="34478-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="34478-120">為了簡單起見，我們執行令人讚嘆的升級： 的所有項目是如果使用者輸入促銷代碼 「 可用 」 的免費 ！</span><span class="sxs-lookup"><span data-stu-id="34478-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="34478-121">排序之後，他們會看到簡單確認畫面：</span><span class="sxs-lookup"><span data-stu-id="34478-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="34478-122">除了客戶 faceing 頁面，我們也會建立顯示一份相簿的系統管理員可以建立、 編輯、 系統管理員區段，並刪除專輯：</span><span class="sxs-lookup"><span data-stu-id="34478-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="34478-123">1.檔案-&gt;新專案</span><span class="sxs-lookup"><span data-stu-id="34478-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="34478-124">安裝軟體</span><span class="sxs-lookup"><span data-stu-id="34478-124">Installing the software</span></span>

<span data-ttu-id="34478-125">本教學課程一開始會先建立使用免費 Visual Web Developer 2010 Express （這是可用），新的 ASP.NET MVC 3 專案，然後我們會以累加方式將新增至建立的完整運作應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="34478-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="34478-126">過程中，我們將討論資料庫存取權、 表單張貼案例、 資料驗證使用 AJAX 頁面更新和驗證、 使用者登入，以及多個一致的頁面配置，請使用主版頁面。</span><span class="sxs-lookup"><span data-stu-id="34478-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="34478-127">您可以展開冒險逐步解說，或者，您可以下載完成的應用程式從[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)。</span><span class="sxs-lookup"><span data-stu-id="34478-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="34478-128">您可以使用 Visual Studio 2010 SP1 或 Visual Web Developer 2010 Express SP1 （Visual Studio 2010 的免費版本） 建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="34478-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="34478-129">我們將使用 SQL Server Compact （也免費） 來主控資料庫。</span><span class="sxs-lookup"><span data-stu-id="34478-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="34478-130">開始之前，請確定您已安裝下面所列的必要條件。</span><span class="sxs-lookup"><span data-stu-id="34478-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="34478-131">[Visual Studio Web Developer Express SP1 必要條件]</span><span class="sxs-lookup"><span data-stu-id="34478-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="34478-132">[ASP.NET MVC 3 Tools Update]</span><span class="sxs-lookup"><span data-stu-id="34478-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="34478-133">[SQL Server Compact 4.0]-包括執行階段和工具的支援</span><span class="sxs-lookup"><span data-stu-id="34478-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="34478-134">建立新的 ASP.NET MVC 3 專案</span><span class="sxs-lookup"><span data-stu-id="34478-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="34478-135">我們會先從在 Visual Web Developer 的 [檔案] 功能表中選取 「 新專案 」。</span><span class="sxs-lookup"><span data-stu-id="34478-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="34478-136">這會開啟 [新增專案] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="34478-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="34478-137">我們將選取的 Visual C#-&gt;網站範本左邊的群組，然後選擇 中心資料行中的 < ASP.NET MVC 3 Web 應用程式 」 範本。</span><span class="sxs-lookup"><span data-stu-id="34478-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="34478-138">MvcMusicStore 您專案的名稱，然後按 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="34478-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="34478-139">這會顯示次要 對話方塊，讓我們進行一些 MVC 特定設定，為受測專案。</span><span class="sxs-lookup"><span data-stu-id="34478-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="34478-140">選取下列選項：</span><span class="sxs-lookup"><span data-stu-id="34478-140">Select the following:</span></span>

<span data-ttu-id="34478-141">專案範本： 選取 空白</span><span class="sxs-lookup"><span data-stu-id="34478-141">Project Template - select Empty</span></span>

<span data-ttu-id="34478-142">檢視引擎-選取 Razor</span><span class="sxs-lookup"><span data-stu-id="34478-142">View Engine - select Razor</span></span>

<span data-ttu-id="34478-143">使用 HTML5 語意標記-檢查</span><span class="sxs-lookup"><span data-stu-id="34478-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="34478-144">請確認您的設定，如下所示，然後按 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="34478-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="34478-145">這會建立專案。</span><span class="sxs-lookup"><span data-stu-id="34478-145">This will create our project.</span></span> <span data-ttu-id="34478-146">讓我們看看我們的應用程式，在右邊的 [方案總管] 中已加入的資料夾。</span><span class="sxs-lookup"><span data-stu-id="34478-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="34478-147">空白的 MVC 3 範本未完全空白 – 它會加入基本資料夾結構：</span><span class="sxs-lookup"><span data-stu-id="34478-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="34478-148">ASP.NET MVC 會使用一些基本的命名慣例的資料夾名稱：</span><span class="sxs-lookup"><span data-stu-id="34478-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="34478-149">**Folder**</span><span class="sxs-lookup"><span data-stu-id="34478-149">**Folder**</span></span> | <span data-ttu-id="34478-150">**目的**</span><span class="sxs-lookup"><span data-stu-id="34478-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="34478-151">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="34478-151">**/Controllers**</span></span> | <span data-ttu-id="34478-152">控制站回應從瀏覽器中輸入時，決定要用來執行工作，並將回應傳回至使用者。</span><span class="sxs-lookup"><span data-stu-id="34478-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="34478-153">**/Views**</span><span class="sxs-lookup"><span data-stu-id="34478-153">**/Views**</span></span> | <span data-ttu-id="34478-154">檢視保存我們 UI 的範本</span><span class="sxs-lookup"><span data-stu-id="34478-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="34478-155">**/Models**</span><span class="sxs-lookup"><span data-stu-id="34478-155">**/Models**</span></span> | <span data-ttu-id="34478-156">模型保存和操作資料</span><span class="sxs-lookup"><span data-stu-id="34478-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="34478-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="34478-157">**/Content**</span></span> | <span data-ttu-id="34478-158">此資料夾包含我們影像、 CSS 和任何其他靜態內容</span><span class="sxs-lookup"><span data-stu-id="34478-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="34478-159">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="34478-159">**/Scripts**</span></span> | <span data-ttu-id="34478-160">此資料夾包含我們的 JavaScript 檔案</span><span class="sxs-lookup"><span data-stu-id="34478-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="34478-161">因為預設的 ASP.NET MVC framework 會使用 「 透過組態的慣例 」 的方法，而讓某些資料夾的命名慣例為基礎的預設假設甚至空的 ASP.NET MVC 應用程式中包含這些資料夾。</span><span class="sxs-lookup"><span data-stu-id="34478-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="34478-162">比方說，控制站預設會尋找檢視 [檢視] 資料夾中的您不必明確指定這項程式碼中。</span><span class="sxs-lookup"><span data-stu-id="34478-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="34478-163">會繼續使用預設慣例減少程式碼，您需要撰寫，也可讓它更容易了解您的專案的其他開發人員。</span><span class="sxs-lookup"><span data-stu-id="34478-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="34478-164">我們將說明當我們建立我們的應用程式的多個這些慣例。</span><span class="sxs-lookup"><span data-stu-id="34478-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="34478-165">下一步</span><span class="sxs-lookup"><span data-stu-id="34478-165">Next</span></span>](mvc-music-store-part-2.md)
