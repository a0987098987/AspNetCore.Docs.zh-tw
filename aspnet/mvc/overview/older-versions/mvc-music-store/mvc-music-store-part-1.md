---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 第 1 部分： 概觀和 檔案-> 新增專案 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 1 部分涵蓋概觀和 檔案-> 新專案。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: c03b62db2227c167c68ca5cf8174e4322658d39d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377885"
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="9a654-104">第 1 部分： 概觀和 檔案-> 新增專案</span><span class="sxs-lookup"><span data-stu-id="9a654-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="9a654-105">藉由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="9a654-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="9a654-106">MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="9a654-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="9a654-107">MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。</span><span class="sxs-lookup"><span data-stu-id="9a654-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="9a654-108">本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="9a654-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="9a654-109">第 1 部分涵蓋的概觀和檔案-&gt;新專案。</span><span class="sxs-lookup"><span data-stu-id="9a654-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="9a654-110">總覽</span><span class="sxs-lookup"><span data-stu-id="9a654-110">Overview</span></span>

<span data-ttu-id="9a654-111">MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Web Developer web 程式開發教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="9a654-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="9a654-112">我們會慢慢開始，因此層級的 web 開發初學者體驗也沒關係。</span><span class="sxs-lookup"><span data-stu-id="9a654-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="9a654-113">我們將建置的應用程式是簡單的音樂播放存放區。</span><span class="sxs-lookup"><span data-stu-id="9a654-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="9a654-114">有三個應用程式的主要部分： 購物、 簽出，以及系統管理。</span><span class="sxs-lookup"><span data-stu-id="9a654-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="9a654-115">訪客可以瀏覽相簿的內容類型：</span><span class="sxs-lookup"><span data-stu-id="9a654-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="9a654-116">他們可以檢視單張專輯，並將它新增至購物車：</span><span class="sxs-lookup"><span data-stu-id="9a654-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="9a654-117">它們可以檢閱其購物車，並移除不再需要的任何項目：</span><span class="sxs-lookup"><span data-stu-id="9a654-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="9a654-118">繼續簽出將會提示他們登入或註冊的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="9a654-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="9a654-119">建立帳戶後，他們可以完成順序來填寫 出貨和付款資訊。</span><span class="sxs-lookup"><span data-stu-id="9a654-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="9a654-120">為了簡單起見，我們執行令人讚嘆的升級： 所有項目都可用，如果他們輸入促銷代碼 「 免費 」 ！</span><span class="sxs-lookup"><span data-stu-id="9a654-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="9a654-121">之後排序，就會看到簡單的確認畫面：</span><span class="sxs-lookup"><span data-stu-id="9a654-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="9a654-122">客戶 faceing 除了頁面之外，我們也將建置會顯示一份相簿的系統管理員可以建立、 編輯、 系統管理員一節，並刪除 album:</span><span class="sxs-lookup"><span data-stu-id="9a654-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="9a654-123">1.檔案-&gt;新專案</span><span class="sxs-lookup"><span data-stu-id="9a654-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="9a654-124">安裝軟體</span><span class="sxs-lookup"><span data-stu-id="9a654-124">Installing the software</span></span>

<span data-ttu-id="9a654-125">本教學課程一開始會先建立新的 ASP.NET MVC 3 專案，使用免費 Visual Web Developer 2010 Express （這是免費的），然後再將會以累加方式新增功能來建立完整的運作應用程式。</span><span class="sxs-lookup"><span data-stu-id="9a654-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="9a654-126">過程中，我們將討論資料庫存取、 表單張貼案例、 資料驗證，使用一致的頁面配置、 使用 AJAX 頁面更新和驗證、 使用者登入，以及多個主版頁面。</span><span class="sxs-lookup"><span data-stu-id="9a654-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="9a654-127">您可以照著逐步進行，或者您可以下載完成的應用程式，從[MVC Music 市集](https://github.com/evilDave/MVC-Music-Store)。</span><span class="sxs-lookup"><span data-stu-id="9a654-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="9a654-128">您可以使用 Visual Studio 2010 SP1 或 Visual Web Developer 2010 Express SP1 （Visual Studio 2010 的免費版本） 建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="9a654-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="9a654-129">我們將使用 SQL Server Compact （同樣免費） 來裝載資料庫。</span><span class="sxs-lookup"><span data-stu-id="9a654-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="9a654-130">在開始之前，請確定您已安裝符合下列先決條件。</span><span class="sxs-lookup"><span data-stu-id="9a654-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="9a654-131">[Visual Studio Web Developer Express SP1 必要條件]</span><span class="sxs-lookup"><span data-stu-id="9a654-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="9a654-132">[ASP.NET MVC 3 Tools Update]</span><span class="sxs-lookup"><span data-stu-id="9a654-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="9a654-133">[SQL Server Compact 4.0]-包括執行階段和工具支援</span><span class="sxs-lookup"><span data-stu-id="9a654-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="9a654-134">建立新的 ASP.NET MVC 3 專案</span><span class="sxs-lookup"><span data-stu-id="9a654-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="9a654-135">我們一開始是從在 Visual Web Developer 的 [檔案] 功能表中選取 [新增專案]。</span><span class="sxs-lookup"><span data-stu-id="9a654-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="9a654-136">這會顯示 [新增專案] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="9a654-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="9a654-137">我們將選取的 Visual C#-&gt; Web 範本左側的群組，然後選擇中間欄中的 「 ASP.NET MVC 3 Web 應用程式 」 範本。</span><span class="sxs-lookup"><span data-stu-id="9a654-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="9a654-138">您的專案 MvcMusicStore 命名，然後按 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="9a654-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="9a654-139">這會顯示次要 對話方塊，讓我們能夠得到一些 MVC 的特定設定我們的專案。</span><span class="sxs-lookup"><span data-stu-id="9a654-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="9a654-140">選取下列選項：</span><span class="sxs-lookup"><span data-stu-id="9a654-140">Select the following:</span></span>

<span data-ttu-id="9a654-141">專案範本-選取 空白</span><span class="sxs-lookup"><span data-stu-id="9a654-141">Project Template - select Empty</span></span>

<span data-ttu-id="9a654-142">檢視引擎-選取 Razor</span><span class="sxs-lookup"><span data-stu-id="9a654-142">View Engine - select Razor</span></span>

<span data-ttu-id="9a654-143">使用 HTML5 語意標記為已核取</span><span class="sxs-lookup"><span data-stu-id="9a654-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="9a654-144">請確認您的設定，如下所示，然後按 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="9a654-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="9a654-145">這會建立我們的專案。</span><span class="sxs-lookup"><span data-stu-id="9a654-145">This will create our project.</span></span> <span data-ttu-id="9a654-146">讓我們看看我們在右邊的 [方案總管] 中的應用程式已新增的資料夾。</span><span class="sxs-lookup"><span data-stu-id="9a654-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="9a654-147">空白的 MVC 3 範本不是完全空白-它會新增一個基本的資料夾結構：</span><span class="sxs-lookup"><span data-stu-id="9a654-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="9a654-148">ASP.NET MVC 會使用資料夾名稱的一些基本的命名慣例：</span><span class="sxs-lookup"><span data-stu-id="9a654-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="9a654-149">[資料夾]</span><span class="sxs-lookup"><span data-stu-id="9a654-149">**Folder**</span></span> | <span data-ttu-id="9a654-150">**目的**</span><span class="sxs-lookup"><span data-stu-id="9a654-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="9a654-151">**/ 控制站**</span><span class="sxs-lookup"><span data-stu-id="9a654-151">**/Controllers**</span></span> | <span data-ttu-id="9a654-152">控制站回應輸入從瀏覽器中，決定要用來執行工作，並回應傳回給使用者的項目。</span><span class="sxs-lookup"><span data-stu-id="9a654-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="9a654-153">**/ 檢視表**</span><span class="sxs-lookup"><span data-stu-id="9a654-153">**/Views**</span></span> | <span data-ttu-id="9a654-154">檢視保留我們的 UI 範本</span><span class="sxs-lookup"><span data-stu-id="9a654-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="9a654-155">**/ 模型**</span><span class="sxs-lookup"><span data-stu-id="9a654-155">**/Models**</span></span> | <span data-ttu-id="9a654-156">模型保存和管理資料</span><span class="sxs-lookup"><span data-stu-id="9a654-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="9a654-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="9a654-157">**/Content**</span></span> | <span data-ttu-id="9a654-158">此資料夾保留我們的映像、 CSS 和任何其他靜態內容</span><span class="sxs-lookup"><span data-stu-id="9a654-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="9a654-159">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="9a654-159">**/Scripts**</span></span> | <span data-ttu-id="9a654-160">此資料夾保留我們的 JavaScript 檔案</span><span class="sxs-lookup"><span data-stu-id="9a654-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="9a654-161">即使空的 ASP.NET MVC 應用程式中包含這些資料夾，因為預設 ASP.NET MVC 架構會使用 「 convention over configuration"的方法和某些資料夾的命名慣例為基礎的預設假設。</span><span class="sxs-lookup"><span data-stu-id="9a654-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="9a654-162">比方說，控制站預設會尋找 [Views] 資料夾中的檢視的而不需明確指定這個程式碼中。</span><span class="sxs-lookup"><span data-stu-id="9a654-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="9a654-163">繼續使用預設慣例可以減少程式碼，您需要撰寫，也可讓它更容易供其他開發人員了解您的專案。</span><span class="sxs-lookup"><span data-stu-id="9a654-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="9a654-164">我們將說明當我們建立我們的應用程式的多個這些慣例。</span><span class="sxs-lookup"><span data-stu-id="9a654-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9a654-165">下一步</span><span class="sxs-lookup"><span data-stu-id="9a654-165">Next</span></span>](mvc-music-store-part-2.md)
