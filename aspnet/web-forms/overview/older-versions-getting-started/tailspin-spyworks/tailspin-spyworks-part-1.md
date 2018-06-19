---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 第 1 部分： 檔案]-> [新增專案 |Microsoft 文件
author: JoeStagner
description: 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 1 部分涵蓋概觀和檔案/新增的專案。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: a1b9681516e626b6a0eec420b168a74e05d88afb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892461"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="3ab8e-104">第 1 部分： 檔案]-> [新增專案</span><span class="sxs-lookup"><span data-stu-id="3ab8e-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="3ab8e-105">由[Joe stagner 以](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="3ab8e-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="3ab8e-106">Tailspin Spyworks 示範建立功能強大、 可調整的應用程式的.NET 平台是如何 hierarchy 簡單。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="3ab8e-107">它會顯示如何使用 ASP.NET 4 中的強大的新功能，建置線上商店，包括購物、 簽出，以及管理關閉。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="3ab8e-108">此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="3ab8e-109">第 1 部分涵蓋概觀和檔案/新增的專案。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="3ab8e-110">概觀</span><span class="sxs-lookup"><span data-stu-id="3ab8e-110">Overview</span></span>

<span data-ttu-id="3ab8e-111">本教學課程是 ASP.NET WebForms 的簡介。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="3ab8e-112">我們將會啟動慢，因此初學者層級的 web 程式開發體驗也沒關係。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="3ab8e-113">我們會建置應用程式是一個簡單的線上存放區。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="3ab8e-114">訪客可以瀏覽依分類的產品：</span><span class="sxs-lookup"><span data-stu-id="3ab8e-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="3ab8e-115">他們可以檢視單一產品，並將它加入至購物車：</span><span class="sxs-lookup"><span data-stu-id="3ab8e-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="3ab8e-116">它們可以檢閱其購物車，並移除不再需要的任何項目：</span><span class="sxs-lookup"><span data-stu-id="3ab8e-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="3ab8e-117">繼續簽出將會提示使用者</span><span class="sxs-lookup"><span data-stu-id="3ab8e-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="3ab8e-118">排序之後，他們會看到簡單確認畫面：</span><span class="sxs-lookup"><span data-stu-id="3ab8e-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="3ab8e-119">我們一開始會藉由在 Visual Studio 2010 中，建立新的 ASP.NET WebForms 專案，我們將以累加方式新增至建立的完整運作應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="3ab8e-120">過程中，我們將討論資料庫存取權、 清單和格線檢視、 資料更新頁面、 資料驗證、 主版頁面使用一致的頁面配置、 AJAX、 驗證、 使用者成員資格，等等。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="3ab8e-121">您可以展開冒險逐步解說，或者，您可以下載從完成的應用程式 [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="3ab8e-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="3ab8e-122">您可以使用 Visual Studio 2010 或從可用 Visual Web Developer 2010 [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/)。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="3ab8e-123">若要建置應用程式，您可以使用 SQL Server 或免費 SQL Server Express 來裝載資料庫。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="3ab8e-124">檔案 / 新增專案</span><span class="sxs-lookup"><span data-stu-id="3ab8e-124">File / New Project</span></span>

<span data-ttu-id="3ab8e-125">我們會先從 Visual Studio 中的 [檔案] 功能表中選取新的專案。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="3ab8e-126">這會開啟 [新增專案] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="3ab8e-127">我們將選取的 Visual C# / 網站範本左邊的群組，然後選擇 中心資料行中的 「 ASP.NET Web 應用程式 」 範本。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="3ab8e-128">TailspinSpyworks 您專案的名稱，然後按 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="3ab8e-129">這會建立專案。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-129">This will create our project.</span></span> <span data-ttu-id="3ab8e-130">讓我們看看我們在右邊的 [方案總管] 中的應用程式中包含的資料夾。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="3ab8e-131">空白的方案不是完全空白 – 它會加入基本資料夾結構：</span><span class="sxs-lookup"><span data-stu-id="3ab8e-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="3ab8e-132">請注意在 ASP.NET 4 預設專案範本所實作的慣例。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="3ab8e-133">「 帳戶 」 資料夾 ASP 實作基本的使用者介面。網路的成員資格的子系統。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="3ab8e-134">「 指令碼 」 資料夾做為用戶端 JavaScript 檔案的儲存機制並核心 jQuery.js 檔案依預設會提供。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="3ab8e-135">「 樣式 」 資料夾用來組織在網站上的視覺效果 （CSS 樣式表）</span><span class="sxs-lookup"><span data-stu-id="3ab8e-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="3ab8e-136">當我們按下 F5 執行應用程式，並呈現 default.aspx 頁面上看到下列項目。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="3ab8e-137">我們第一個應用程式增強功能會以 CSS 類別和相關聯的映像檔，在我們想要我們 Tailspin Spyworks 應用程式 visual asthetics 取代預設 WebForms 範本中的 Style.css 檔案。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="3ab8e-138">這樣做之後我們 default.aspx 頁面上呈現像這樣。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="3ab8e-139">請注意上方的影像連結方的頁面已加入至主版頁面的功能表項目。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="3ab8e-140">只有 「 登入 」 及 「 帳戶 」 連結指向存在的頁面 （預設範本所產生） 和建置應用程式，我們會實作網頁的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="3ab8e-141">我們也要將主版頁面重新放置到樣式目錄。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="3ab8e-142">雖然這是喜好設定它可能方便的方式稍有如果我們決定要在未來執行我們的應用程式"skinable"。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="3ab8e-143">之後這樣我們就必須變更主版頁面中所有的.aspx 檔案參考產生預設的 ASP.NET WebForms 網頁。</span><span class="sxs-lookup"><span data-stu-id="3ab8e-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3ab8e-144">下一步</span><span class="sxs-lookup"><span data-stu-id="3ab8e-144">Next</span></span>](tailspin-spyworks-part-2.md)
