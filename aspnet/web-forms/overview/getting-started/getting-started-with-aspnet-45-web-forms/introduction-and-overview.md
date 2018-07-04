---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Getting Started with ASP.NET 4.5 Web Form 和 Visual Studio 2013 |Microsoft Docs
author: Erikre
description: 此逐步教學課程系列將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 55e7a5e8c0c7df9597f8597cba49d33da23e9159
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365402"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="778da-103">Getting Started with ASP.NET 4.5 Web Form 和 Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="778da-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="778da-104">藉由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="778da-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="778da-105">[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="778da-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="778da-106">此逐步教學課程系列將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="778da-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="778da-107">ASP.NET Web Form 測驗</span><span class="sxs-lookup"><span data-stu-id="778da-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="778da-108">測試您的知識並藉由採取 ASP.NET Web Form 測驗加強重要概念。</span><span class="sxs-lookup"><span data-stu-id="778da-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="778da-109">這項測驗被專從本教學課程系列中所包含的內容。</span><span class="sxs-lookup"><span data-stu-id="778da-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="778da-110">在測驗中的每個問題時提供說明以及其他指引的連結。</span><span class="sxs-lookup"><span data-stu-id="778da-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="778da-111">簡介</span><span class="sxs-lookup"><span data-stu-id="778da-111">Introduction</span></span>

<span data-ttu-id="778da-112">這一系列的教學課程會引導您完成建立 ASP.NET Web Forms 應用程式使用 Visual Studio Express 2013 for Web 和 ASP.NET 4.5 所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="778da-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="778da-113">應用程式，您將建立名為**Wingtip Toys**。</span><span class="sxs-lookup"><span data-stu-id="778da-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="778da-114">這是簡化的範例銷售項目線上存放區的前端 web 站台。</span><span class="sxs-lookup"><span data-stu-id="778da-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="778da-115">本教學課程系列會反白顯示 ASP.NET 4.5 中所提供的新功能。</span><span class="sxs-lookup"><span data-stu-id="778da-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="778da-116">註解是歡迎畫面中，我們會盡所有努力更新本教學課程系列，根據您的建議。</span><span class="sxs-lookup"><span data-stu-id="778da-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="778da-117">下載已完成專案</span><span class="sxs-lookup"><span data-stu-id="778da-117">Download completed project</span></span>

<span data-ttu-id="778da-118">您可以下載 C# 專案，其中包含已完成本教學課程。</span><span class="sxs-lookup"><span data-stu-id="778da-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="778da-119">[開始使用 ASP.NET 4.5 Web Form 與 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="778da-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="778da-120">檢閱的內容採取相關的 ASP.NET Web Form 測驗</span><span class="sxs-lookup"><span data-stu-id="778da-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="778da-121">完成本教學課程之後，測試您的知識，並強化採取的重要概念[ASP.NET Web Form 測驗](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001)。</span><span class="sxs-lookup"><span data-stu-id="778da-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="778da-122">這項測驗被專從本教學課程系列中所包含的內容。</span><span class="sxs-lookup"><span data-stu-id="778da-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="778da-123">在測驗中的每個問題時提供說明以及其他指引的連結。</span><span class="sxs-lookup"><span data-stu-id="778da-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="778da-124">ASP.NET Web Form 測驗</span><span class="sxs-lookup"><span data-stu-id="778da-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="778da-125">適用對象</span><span class="sxs-lookup"><span data-stu-id="778da-125">Audience</span></span>

<span data-ttu-id="778da-126">本教學課程系列的對象是有經驗的開發人員不熟悉 ASP.NET Web Form。</span><span class="sxs-lookup"><span data-stu-id="778da-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="778da-127">在本教學課程系列中，您想要的開發人員應該具備下列技術：</span><span class="sxs-lookup"><span data-stu-id="778da-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="778da-128">熟悉物件導向程式設計 (OOP) 語言</span><span class="sxs-lookup"><span data-stu-id="778da-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="778da-129">熟悉 Web 程式開發概念 (HTML、 CSS、 JavaScript)</span><span class="sxs-lookup"><span data-stu-id="778da-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="778da-130">熟悉關聯式資料庫概念</span><span class="sxs-lookup"><span data-stu-id="778da-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="778da-131">熟悉多層式架構概念</span><span class="sxs-lookup"><span data-stu-id="778da-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="778da-132">如果您有興趣檢閱上面所列的區域，請考慮檢閱下列內容：</span><span class="sxs-lookup"><span data-stu-id="778da-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="778da-133">Getting Started with Visual C#</span><span class="sxs-lookup"><span data-stu-id="778da-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="778da-134">[Web 開發](https://msdn.microsoft.com/beginner/bb308760.aspx)， [HTML、 CSS、 JavaScript、 SQL、 PHP、 JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="778da-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="778da-135">關聯式資料庫</span><span class="sxs-lookup"><span data-stu-id="778da-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="778da-136">多層式架構</span><span class="sxs-lookup"><span data-stu-id="778da-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="778da-137">應用程式功能</span><span class="sxs-lookup"><span data-stu-id="778da-137">Application Features</span></span>

<span data-ttu-id="778da-138">在這一系列提供的 ASP.NET Web Form 功能包括：</span><span class="sxs-lookup"><span data-stu-id="778da-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="778da-139">Web 應用程式專案 （不是網站專案）</span><span class="sxs-lookup"><span data-stu-id="778da-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="778da-140">Web Form</span><span class="sxs-lookup"><span data-stu-id="778da-140">Web Forms</span></span>
- <span data-ttu-id="778da-141">主版頁面設定</span><span class="sxs-lookup"><span data-stu-id="778da-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="778da-142">啟動程序</span><span class="sxs-lookup"><span data-stu-id="778da-142">Bootstrap</span></span>
- <span data-ttu-id="778da-143">Entity Framework Code First，LocalDB</span><span class="sxs-lookup"><span data-stu-id="778da-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="778da-144">要求驗證</span><span class="sxs-lookup"><span data-stu-id="778da-144">Request Validation</span></span>
- <span data-ttu-id="778da-145">強型別資料控制項中，模型繫結，資料註解，以及值提供者</span><span class="sxs-lookup"><span data-stu-id="778da-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="778da-146">SSL 和 OAuth</span><span class="sxs-lookup"><span data-stu-id="778da-146">SSL and OAuth</span></span>
- <span data-ttu-id="778da-147">ASP.NET 身分識別、 組態和授權</span><span class="sxs-lookup"><span data-stu-id="778da-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="778da-148">低調驗證</span><span class="sxs-lookup"><span data-stu-id="778da-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="778da-149">路由</span><span class="sxs-lookup"><span data-stu-id="778da-149">Routing</span></span>
- <span data-ttu-id="778da-150">ASP.NET 錯誤處理</span><span class="sxs-lookup"><span data-stu-id="778da-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="778da-151">應用程式案例和工作</span><span class="sxs-lookup"><span data-stu-id="778da-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="778da-152">本系列中所示範的工作包括：</span><span class="sxs-lookup"><span data-stu-id="778da-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="778da-153">建立、 檢閱和執行新的專案</span><span class="sxs-lookup"><span data-stu-id="778da-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="778da-154">建立資料庫結構</span><span class="sxs-lookup"><span data-stu-id="778da-154">Creating the database structure</span></span>
- <span data-ttu-id="778da-155">初始化並植入資料庫</span><span class="sxs-lookup"><span data-stu-id="778da-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="778da-156">自訂使用樣式、 圖形和主版頁面 UI</span><span class="sxs-lookup"><span data-stu-id="778da-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="778da-157">加入頁面和導覽</span><span class="sxs-lookup"><span data-stu-id="778da-157">Adding pages and navigation</span></span>
- <span data-ttu-id="778da-158">顯示功能表的詳細資訊和產品資料</span><span class="sxs-lookup"><span data-stu-id="778da-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="778da-159">建立購物車</span><span class="sxs-lookup"><span data-stu-id="778da-159">Creating a shopping cart</span></span>
- <span data-ttu-id="778da-160">新增 SSL 和 OAuth 支援</span><span class="sxs-lookup"><span data-stu-id="778da-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="778da-161">新增付款方法</span><span class="sxs-lookup"><span data-stu-id="778da-161">Adding a payment method</span></span>
- <span data-ttu-id="778da-162">包括系統管理員角色和使用者使用應用程式</span><span class="sxs-lookup"><span data-stu-id="778da-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="778da-163">限制存取特定頁面以及資料夾</span><span class="sxs-lookup"><span data-stu-id="778da-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="778da-164">將檔案上傳至 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="778da-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="778da-165">實作輸入的驗證</span><span class="sxs-lookup"><span data-stu-id="778da-165">Implementing input validation</span></span>
- <span data-ttu-id="778da-166">註冊 web 應用程式的路由</span><span class="sxs-lookup"><span data-stu-id="778da-166">Registering routes for the web application</span></span>
- <span data-ttu-id="778da-167">實作錯誤處理和錯誤記錄</span><span class="sxs-lookup"><span data-stu-id="778da-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="778da-168">總覽</span><span class="sxs-lookup"><span data-stu-id="778da-168">Overview</span></span>

<span data-ttu-id="778da-169">如果您不熟悉 ASP.NET Web Form，但沒有熟悉程式設計概念，您有正確的教學課程。</span><span class="sxs-lookup"><span data-stu-id="778da-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="778da-170">如果您已熟悉 ASP.NET Web Form，您可以直接從本教學課程系列 ASP.NET 4.5 中所提供的新功能。</span><span class="sxs-lookup"><span data-stu-id="778da-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="778da-171">如果您不熟悉程式設計概念和 ASP.NET Web Form，請參閱 Web Form 中提供的其他教學課程[開始使用](../../../index.md)ASP.NET 網站上的一節。</span><span class="sxs-lookup"><span data-stu-id="778da-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="778da-172">特定**最新**ASP.NET 4.5 功能提供在此 Web Form 中的教學課程系列包括下列：</span><span class="sxs-lookup"><span data-stu-id="778da-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="778da-173">建立簡單的 UI 專案該供應項目[支援多個 ASP.NET 架構](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add)（Web Form、 MVC 和 Web API）。</span><span class="sxs-lookup"><span data-stu-id="778da-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="778da-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)，版面配置和佈景主題的架構，可提供回應式設計和佈景主題的功能。</span><span class="sxs-lookup"><span data-stu-id="778da-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="778da-175">[ASP.NET Identity](../../../../identity/index.md)，新的 ASP.NET 成員資格系統，與 web 裝載軟體，而不是 IIS 的運作方式在所有 ASP.NET framework 和運作方式相同。</span><span class="sxs-lookup"><span data-stu-id="778da-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="778da-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)、 更新 Entity Framework 可讓您擷取和操作資料做為強型別的物件、 存取資料以非同步的方式，處理暫時性連接失敗，以及記錄 SQL 陳述式。</span><span class="sxs-lookup"><span data-stu-id="778da-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="778da-177">ASP.NET 4.5 功能的完整清單，請參閱 < [ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊](../../../../visual-studio/overview/2013/release-notes.md)。</span><span class="sxs-lookup"><span data-stu-id="778da-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="778da-178">Wingtip Toys 範例應用程式</span><span class="sxs-lookup"><span data-stu-id="778da-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="778da-179">下列螢幕擷取畫面會提供您將在本教學課程系列中建立 ASP.NET Web forms 應用程式的快速檢視。</span><span class="sxs-lookup"><span data-stu-id="778da-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="778da-180">當您從 Visual Studio Express 2013 for Web 中執行應用程式時，您會看到下列 web 首頁。</span><span class="sxs-lookup"><span data-stu-id="778da-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys-預設頁面](introduction-and-overview/_static/image1.png)

<span data-ttu-id="778da-182">您可以註冊為新的使用者，或現有的使用者身分登入。</span><span class="sxs-lookup"><span data-stu-id="778da-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="778da-183">瀏覽是由從資料庫擷取可用的產品，在頂端提供每個產品類別。</span><span class="sxs-lookup"><span data-stu-id="778da-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="778da-184">藉由選取 [產品] 連結，您將能夠看到所有可用產品清單。</span><span class="sxs-lookup"><span data-stu-id="778da-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys-產品](introduction-and-overview/_static/image2.png)

<span data-ttu-id="778da-186">您也可以選取任何列出的產品，以查看個別的產品詳細資料。</span><span class="sxs-lookup"><span data-stu-id="778da-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys-產品詳細資料](introduction-and-overview/_static/image3.png)

<span data-ttu-id="778da-188">身為使用者，您可以註冊及登入使用 Web 表單範本的預設功能。</span><span class="sxs-lookup"><span data-stu-id="778da-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="778da-189">本教學課程也說明如何使用現有的 Gmail 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="778da-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="778da-190">此外，您可以登入為系統管理員新增和移除資料庫中的產品。</span><span class="sxs-lookup"><span data-stu-id="778da-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys-登入](introduction-and-overview/_static/image4.png)

<span data-ttu-id="778da-192">一旦您已登入的使用者身分，您可以新增至購物車及結帳使用 PayPal 的產品。</span><span class="sxs-lookup"><span data-stu-id="778da-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="778da-193">請注意，此範例應用程式設計來透過 PayPal 的開發人員沙箱函式。</span><span class="sxs-lookup"><span data-stu-id="778da-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="778da-194">沒有實際的金錢交易就會進行。</span><span class="sxs-lookup"><span data-stu-id="778da-194">No actual money transaction will take place.</span></span>

![Wingtip Toys-購物車](introduction-and-overview/_static/image5.png)

<span data-ttu-id="778da-196">PayPal 會確認您的帳戶、 訂單和付款資訊。</span><span class="sxs-lookup"><span data-stu-id="778da-196">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys-PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="778da-198">傳回從 PayPal 之後, 您可以檢閱並完成您的訂單。</span><span class="sxs-lookup"><span data-stu-id="778da-198">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys-順序檢閱](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="778da-200">必要條件</span><span class="sxs-lookup"><span data-stu-id="778da-200">Prerequisites</span></span>

<span data-ttu-id="778da-201">在開始之前，請確定您已在電腦上安裝下列軟體：</span><span class="sxs-lookup"><span data-stu-id="778da-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="778da-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)或是[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。</span><span class="sxs-lookup"><span data-stu-id="778da-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="778da-203">.NET Framework 會自動安裝。</span><span class="sxs-lookup"><span data-stu-id="778da-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="778da-204">本教學課程系列會使用 Microsoft Visual Studio Express 2013 for Web。</span><span class="sxs-lookup"><span data-stu-id="778da-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="778da-205">您可以使用 Microsoft Visual Studio Express 2013 for Web 或 Microsoft Visual Studio 2013，才能完成本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="778da-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="778da-206">Microsoft Visual Studio 2013 和 Microsoft Visual Studio Express 2013 for Web 將通常稱為 Visual Studio 在此教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="778da-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="778da-207">如果您已經安裝 Visual Studio 版本，安裝程序會安裝 Visual Studio 2013 或 Microsoft Visual Studio Express 2013 for Web 的現有版本旁邊。</span><span class="sxs-lookup"><span data-stu-id="778da-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="778da-208">您在舊版中建立的站台可以在 Visual Studio 2013 中開啟，並繼續在舊版中開啟。</span><span class="sxs-lookup"><span data-stu-id="778da-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="778da-209">本逐步解說假設您已經選擇*Web 開發*設定集合，第一次您啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="778da-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="778da-210">如需詳細資訊，請參閱[如何： 選取 Web 程式開發環境設定](https://msdn.microsoft.com/library/ff521558.aspx)。</span><span class="sxs-lookup"><span data-stu-id="778da-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="778da-211">下載範例應用程式</span><span class="sxs-lookup"><span data-stu-id="778da-211">Download the Sample Application</span></span>

<span data-ttu-id="778da-212">安裝必要條件之後, 您已準備好開始本教學課程系列中建立新的 Web 專案所呈現項目。</span><span class="sxs-lookup"><span data-stu-id="778da-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="778da-213">如果您想要**選擇性地**執行本教學課程系列會建立範例應用程式，您可以下載從 MSDN 範例網站。</span><span class="sxs-lookup"><span data-stu-id="778da-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="778da-214">此下載包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="778da-214">This download contains the following:</span></span>

- <span data-ttu-id="778da-215">中的範例應用程式*WingtipToys*資料夾。</span><span class="sxs-lookup"><span data-stu-id="778da-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="778da-216">若要建立範例應用程式中的使用的資源*WingtipToys 資產*中的資料夾*WingtipToys*資料夾。</span><span class="sxs-lookup"><span data-stu-id="778da-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="778da-217">從 MSDN 範例網站下載檔案：</span><span class="sxs-lookup"><span data-stu-id="778da-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="778da-218">[開始使用 ASP.NET 4.5 Web Form 與 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="778da-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="778da-219">下載<em>.zip</em>檔案。</span><span class="sxs-lookup"><span data-stu-id="778da-219">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="778da-220">若要查看已完成本教學課程系列所建立的專案，尋找並選取<em>C#</em>中的資料夾<em>.zip</em>檔案。</span><span class="sxs-lookup"><span data-stu-id="778da-220">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="778da-221">儲存<em>C#</em> folderto 您使用以使用 Visual Studio 2013 專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="778da-221">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="778da-222">根據預設，Visual Studio 2013 的 [專案] 資料夾如下：</span><span class="sxs-lookup"><span data-stu-id="778da-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="778da-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="778da-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="778da-224">重新命名***C#*** 資料夾，以***WingtipToys***。</span><span class="sxs-lookup"><span data-stu-id="778da-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="778da-225">如果您已經有一個名為資料夾*WingtipToys*在專案資料夾中，暫時重新命名該現有的資料夾重新命名之前*C#* 資料夾*WingtipToys*。</span><span class="sxs-lookup"><span data-stu-id="778da-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="778da-226">若要執行已完成的專案，開啟*WingtipToys*資料夾，然後按兩下*WingtipToys.sln*檔案。</span><span class="sxs-lookup"><span data-stu-id="778da-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="778da-227">Visual Studio 2013 將開啟的專案。</span><span class="sxs-lookup"><span data-stu-id="778da-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="778da-228">接下來，以滑鼠右鍵按一下*Default.aspx*檔案在 [方案總管] 視窗中，並從滑鼠右鍵功能表中按一下 瀏覽器中檢視。</span><span class="sxs-lookup"><span data-stu-id="778da-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="778da-229">教學課程的支援和註解</span><span class="sxs-lookup"><span data-stu-id="778da-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="778da-230">使用隨附的問與答一節[Getting Started with ASP.NET 4.5 Web Form 與 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 範例的任何問題或意見。</span><span class="sxs-lookup"><span data-stu-id="778da-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="778da-231">在此教學課程系列的註解是 褖畫惎，且致力此教學課程系列更新時進行納入帳戶更正或建議的教學課程的註解中所提供的增強功能。</span><span class="sxs-lookup"><span data-stu-id="778da-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="778da-232">在開發期間，發生錯誤時，或如果網站不會正確執行，可能讓問題的來源複雜線索的錯誤訊息，或可能不會說明如何修正此問題。</span><span class="sxs-lookup"><span data-stu-id="778da-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="778da-233">為了協助您利用一些常見的問題案例，您也可以使用[ASP.NET 論壇](https://forums.asp.net/)或隨附的 [問與答] 區段[開始使用 ASP.NET 4.5 Web Form 與 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 範例。</span><span class="sxs-lookup"><span data-stu-id="778da-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="778da-234">如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必查看上述位置。</span><span class="sxs-lookup"><span data-stu-id="778da-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="778da-235">下一步</span><span class="sxs-lookup"><span data-stu-id="778da-235">Next</span></span>](create-the-project.md)
