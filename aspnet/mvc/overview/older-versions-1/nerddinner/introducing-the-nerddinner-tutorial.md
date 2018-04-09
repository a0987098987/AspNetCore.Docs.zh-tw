---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: 簡介 NerdDinner 教學課程 |Microsoft 文件
author: shanselman
description: 若要深入了解新的架構，最好是建立它的項目。 本教學課程逐步解說如何建置使用 ASP.NE 很小，但完整的應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 3d925a7dc89fc0c742468653c5c138a0f1d71231
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="db45f-104">簡介 NerdDinner 教學課程</span><span class="sxs-lookup"><span data-stu-id="db45f-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="db45f-105">由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="db45f-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="db45f-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="db45f-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="db45f-107">若要深入了解新的架構，最好是建立它的項目。</span><span class="sxs-lookup"><span data-stu-id="db45f-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="db45f-108">本教學課程會逐步解說如何建置小，但是完成，應用程式使用 ASP.NET MVC 1，並介紹一些背後的核心概念。</span><span class="sxs-lookup"><span data-stu-id="db45f-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="db45f-109">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="db45f-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="db45f-110">NerdDinner 教學課程</span><span class="sxs-lookup"><span data-stu-id="db45f-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="db45f-111">若要深入了解新的架構，最好是建立它的項目。</span><span class="sxs-lookup"><span data-stu-id="db45f-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="db45f-112">本教學課程會逐步解說如何建置小，但是完成，請使用 ASP.NET MVC 應用程式，並介紹一些背後的核心概念。</span><span class="sxs-lookup"><span data-stu-id="db45f-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="db45f-113">我們即將建置的應用程式稱為 「 NerdDinner"。</span><span class="sxs-lookup"><span data-stu-id="db45f-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="db45f-114">NerdDinner 提供簡單的方法來尋找和組織線上 dinners 障礙人士：</span><span class="sxs-lookup"><span data-stu-id="db45f-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="db45f-115">NerdDinner 可讓您建立、 編輯和刪除 dinners 註冊的使用者。</span><span class="sxs-lookup"><span data-stu-id="db45f-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="db45f-116">它會強制一致的驗證和商務規則集執行整個應用程式：</span><span class="sxs-lookup"><span data-stu-id="db45f-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="db45f-117">訪客可以使用以 AJAX 為基礎的對應搜尋即將 dinners 持有附近它們：</span><span class="sxs-lookup"><span data-stu-id="db45f-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="db45f-118">按一下 dinner 會移到詳細資料頁面它們可以在哪裡取得詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="db45f-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="db45f-119">如果他們想要參加的 dinner 他們可以登入，或在網站上註冊：</span><span class="sxs-lookup"><span data-stu-id="db45f-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="db45f-120">然後，他們就可以按一下參加事件以 AJAX 為基礎的 RSVP 連結：</span><span class="sxs-lookup"><span data-stu-id="db45f-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="db45f-121">實作 NerdDinner</span><span class="sxs-lookup"><span data-stu-id="db45f-121">Implementing NerdDinner</span></span>

<span data-ttu-id="db45f-122">我們即將開始我們 NerdDinner 應用程式使用的檔案-&gt;Visual Studio 中建立新的 ASP.NET MVC 專案的新專案命令。</span><span class="sxs-lookup"><span data-stu-id="db45f-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="db45f-123">我們會再以累加方式加入功能與特性。</span><span class="sxs-lookup"><span data-stu-id="db45f-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="db45f-124">在過程中，我們將討論：</span><span class="sxs-lookup"><span data-stu-id="db45f-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="db45f-125">如何建立新的 ASP.NET MVC 專案</span><span class="sxs-lookup"><span data-stu-id="db45f-125">How to create a new ASP.NET MVC Project</span></span>](# "建立新的 ASP.NET MVC 專案")
2. [<span data-ttu-id="db45f-126">如何建立資料庫</span><span class="sxs-lookup"><span data-stu-id="db45f-126">How to create a database</span></span>](# "建立資料庫")
3. [<span data-ttu-id="db45f-127">如何建立使用商務規則驗證模型</span><span class="sxs-lookup"><span data-stu-id="db45f-127">How to build a model with business rule validations</span></span>](# "建立使用商務規則驗證模型")
4. [<span data-ttu-id="db45f-128">如何使用控制器和檢視來實作清單/詳細資料 UI</span><span class="sxs-lookup"><span data-stu-id="db45f-128">How to use controllers and views to implement a listing/details UI</span></span>](# "使用控制器和檢視來實作詳細資料清單/UI")
5. <span data-ttu-id="db45f-129">[如何提供 CRUD （建立、 讀取、 更新、 刪除） 資料組成項目支援](# "提供 CRUD （建立、 讀取、 更新、 刪除） 資料表單項目支援")</span><span class="sxs-lookup"><span data-stu-id="db45f-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="db45f-130">如何使用別的 ViewData 及實作 ViewModel 類別</span><span class="sxs-lookup"><span data-stu-id="db45f-130">How to use ViewData and implement ViewModel classes</span></span>](# "使用別的 ViewData 和實作 ViewModel 類別")
7. [<span data-ttu-id="db45f-131">如何重複使用 UI 使用主版頁面和 partials</span><span class="sxs-lookup"><span data-stu-id="db45f-131">How to re-use UI using master pages and partials</span></span>](# "重複使用 UI 使用主版頁面和 Partials")
8. [<span data-ttu-id="db45f-132">如何實作有效率的資料分頁</span><span class="sxs-lookup"><span data-stu-id="db45f-132">How to implement efficient data paging</span></span>](# "實作有效率的資料分頁")
9. [<span data-ttu-id="db45f-133">如何保護應用程式使用驗證和授權</span><span class="sxs-lookup"><span data-stu-id="db45f-133">How to secure applications using authentication and authorization</span></span>](# "安全的應用程式使用驗證和授權")
10. [<span data-ttu-id="db45f-134">如何動態更新使用 AJAX</span><span class="sxs-lookup"><span data-stu-id="db45f-134">How to use AJAX to deliver dynamic updates</span></span>](# "使用 AJAX 傳送動態更新")
11. [<span data-ttu-id="db45f-135">如何實作對應實例中使用 AJAX</span><span class="sxs-lookup"><span data-stu-id="db45f-135">How to use AJAX to implement mapping scenarios</span></span>](# "使用 AJAX 實作對應案例")
12. [<span data-ttu-id="db45f-136">如何啟用自動化的單元測試</span><span class="sxs-lookup"><span data-stu-id="db45f-136">How to enable automated unit testing</span></span>](# "啟用自動化單元測試")

<span data-ttu-id="db45f-137">您可以建立自己的 NerdDinner 從頭完成每個步驟我們在本章中的逐步解說。</span><span class="sxs-lookup"><span data-stu-id="db45f-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="db45f-138">或者，您可以下載完整的版的原始碼： [GitHub 上的 NerdDinner](https://github.com/AspNetMVPSamples/NerdDinner)。</span><span class="sxs-lookup"><span data-stu-id="db45f-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="db45f-139">您也可以選擇性地也可以[下載免費的 PDF 版本，本教學課程的](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)如果您想要讀取的離線教學課程。</span><span class="sxs-lookup"><span data-stu-id="db45f-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="db45f-140">您可以使用 Visual Studio 2008 或免費 Visual Web Developer 2008 Express 建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="db45f-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="db45f-141">您可以使用 SQL Server 或免費的 SQL Server Express 資料庫。</span><span class="sxs-lookup"><span data-stu-id="db45f-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="db45f-142">您可以安裝 ASP.NET MVC、 Visual Web Developer 2008 Express，以及 SQL Server Express （免費） 使用的 V2 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="db45f-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="db45f-143">現在讓我們開始吧...</span><span class="sxs-lookup"><span data-stu-id="db45f-143">Now let's get started....</span></span>

<span data-ttu-id="db45f-144">現在我們已經探討 NerdDinner 是什麼，讓我們來彙總我們套筒它們套並撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="db45f-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="db45f-145">我們一開始會使用檔案-&gt;建立 NerdDinner 應用程式的 Visual Studio 中的新專案。</span><span class="sxs-lookup"><span data-stu-id="db45f-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="db45f-146">下一步</span><span class="sxs-lookup"><span data-stu-id="db45f-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
