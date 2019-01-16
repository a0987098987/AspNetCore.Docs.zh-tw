---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Getting Started with ASP.NET 4.7 Web Form 和 Visual Studio 2017 |Microsoft Docs
author: Erikre
description: 此逐步教學課程系列將教導您建置使用 ASP.NET 4.7 和 Microsoft Visual Studio 的 ASP.NET Web Forms 應用程式的基本概念
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341546"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a><span data-ttu-id="8d8c5-103">Getting Started with ASP.NET 4.5 Web Form 和 Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8d8c5-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2017</span></span>
====================

<span data-ttu-id="8d8c5-104">[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="8d8c5-104">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="8d8c5-105">本系列教學課程會示範如何建置 ASP.NET 4.5 和 Microsoft Visual Studio 2017 的 ASP.NET Web Forms 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-105">This tutorial series shows you how to build an ASP.NET Web Forms application with ASP.NET 4.5 and Microsoft Visual Studio 2017.</span></span> 

## <a name="introduction"></a><span data-ttu-id="8d8c5-106">簡介</span><span class="sxs-lookup"><span data-stu-id="8d8c5-106">Introduction</span></span>

<span data-ttu-id="8d8c5-107">本教學課程系列會引導您完成建立 ASP.NET Web Forms 應用程式使用 Visual Studio 2017 和 ASP.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-107">This tutorial series guides you through creating an ASP.NET Web Forms application using Visual Studio 2017 and ASP.NET 4.5.</span></span> <span data-ttu-id="8d8c5-108">您將建立名為應用程式**Wingtip Toys** -簡化店面網站銷售線上項目。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-108">You'll create an application named **Wingtip Toys** - a simplified storefront web site selling items online.</span></span> <span data-ttu-id="8d8c5-109">在系列中，ASP.NET 4.5 的新功能會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-109">During the series, new ASP.NET 4.5 features are highlighted.</span></span>

### <a name="target-audience"></a><span data-ttu-id="8d8c5-110">目標對象</span><span class="sxs-lookup"><span data-stu-id="8d8c5-110">Target audience</span></span>

<span data-ttu-id="8d8c5-111">ASP.NET Web Form 的新的開發人員是本教學課程系列的目標對象。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-111">Developers new to ASP.NET Web Forms are the target audience for this tutorial series.</span></span>

<span data-ttu-id="8d8c5-112">您應該具備一些知識，在下列區域：</span><span class="sxs-lookup"><span data-stu-id="8d8c5-112">You should have some knowledge in the following areas:</span></span>

- <span data-ttu-id="8d8c5-113">物件導向程式設計 (OOP) 和語言</span><span class="sxs-lookup"><span data-stu-id="8d8c5-113">Object-oriented programming (OOP) and languages</span></span>
- <span data-ttu-id="8d8c5-114">Web 程式開發 (HTML、 CSS、 JavaScript)</span><span class="sxs-lookup"><span data-stu-id="8d8c5-114">Web development (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="8d8c5-115">關聯式資料庫</span><span class="sxs-lookup"><span data-stu-id="8d8c5-115">Relational databases</span></span>
- <span data-ttu-id="8d8c5-116">多層式架構</span><span class="sxs-lookup"><span data-stu-id="8d8c5-116">N-tier architecture</span></span>

<span data-ttu-id="8d8c5-117">若要檢閱這些區域，請考慮研究下列內容：</span><span class="sxs-lookup"><span data-stu-id="8d8c5-117">To review these areas, consider studying the following content:</span></span>

- [<span data-ttu-id="8d8c5-118">Getting Started with Visual C#</span><span class="sxs-lookup"><span data-stu-id="8d8c5-118">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="8d8c5-119">[Web 開發](https://msdn.microsoft.com/beginner/bb308760.aspx)， [HTML、 CSS、 JavaScript、 SQL、 PHP、 JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="8d8c5-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="8d8c5-120">關聯式資料庫</span><span class="sxs-lookup"><span data-stu-id="8d8c5-120">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="8d8c5-121">多層式架構</span><span class="sxs-lookup"><span data-stu-id="8d8c5-121">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="8d8c5-122">應用程式功能</span><span class="sxs-lookup"><span data-stu-id="8d8c5-122">Application features</span></span>

<span data-ttu-id="8d8c5-123">在這一系列提供的 ASP.NET Web Form 功能包括：</span><span class="sxs-lookup"><span data-stu-id="8d8c5-123">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="8d8c5-124">Web 應用程式專案 （不是網站專案）</span><span class="sxs-lookup"><span data-stu-id="8d8c5-124">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="8d8c5-125">Web Form</span><span class="sxs-lookup"><span data-stu-id="8d8c5-125">Web Forms</span></span>
- <span data-ttu-id="8d8c5-126">主版頁面設定</span><span class="sxs-lookup"><span data-stu-id="8d8c5-126">Master Pages, Configuration</span></span>
- <span data-ttu-id="8d8c5-127">啟動程序</span><span class="sxs-lookup"><span data-stu-id="8d8c5-127">Bootstrap</span></span>
- <span data-ttu-id="8d8c5-128">Entity Framework Code First，LocalDB</span><span class="sxs-lookup"><span data-stu-id="8d8c5-128">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="8d8c5-129">要求驗證</span><span class="sxs-lookup"><span data-stu-id="8d8c5-129">Request Validation</span></span>
- <span data-ttu-id="8d8c5-130">強型別資料控制項</span><span class="sxs-lookup"><span data-stu-id="8d8c5-130">Strongly-typed Data Controls</span></span>
- <span data-ttu-id="8d8c5-131">模型繫結</span><span class="sxs-lookup"><span data-stu-id="8d8c5-131">Model Binding</span></span>
- <span data-ttu-id="8d8c5-132">資料註釋</span><span class="sxs-lookup"><span data-stu-id="8d8c5-132">Data Annotations</span></span>
- <span data-ttu-id="8d8c5-133">值提供者</span><span class="sxs-lookup"><span data-stu-id="8d8c5-133">Value Providers</span></span>
- <span data-ttu-id="8d8c5-134">SSL 和 OAuth</span><span class="sxs-lookup"><span data-stu-id="8d8c5-134">SSL and OAuth</span></span>
- <span data-ttu-id="8d8c5-135">ASP.NET 身分識別、 組態和授權</span><span class="sxs-lookup"><span data-stu-id="8d8c5-135">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="8d8c5-136">低調驗證</span><span class="sxs-lookup"><span data-stu-id="8d8c5-136">Unobtrusive Validation</span></span>
- <span data-ttu-id="8d8c5-137">路由</span><span class="sxs-lookup"><span data-stu-id="8d8c5-137">Routing</span></span>
- <span data-ttu-id="8d8c5-138">ASP.NET 錯誤處理</span><span class="sxs-lookup"><span data-stu-id="8d8c5-138">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="8d8c5-139">應用程式案例和工作</span><span class="sxs-lookup"><span data-stu-id="8d8c5-139">Application scenarios and tasks</span></span>

<span data-ttu-id="8d8c5-140">教學課程系列的工作包括：</span><span class="sxs-lookup"><span data-stu-id="8d8c5-140">Tutorial series tasks include:</span></span>

- <span data-ttu-id="8d8c5-141">建立、 檢視和執行新的專案</span><span class="sxs-lookup"><span data-stu-id="8d8c5-141">Creating, reviewing, and running a new project</span></span>
- <span data-ttu-id="8d8c5-142">建立資料庫的結構</span><span class="sxs-lookup"><span data-stu-id="8d8c5-142">Creating a database structure</span></span>
- <span data-ttu-id="8d8c5-143">初始化並植入資料庫</span><span class="sxs-lookup"><span data-stu-id="8d8c5-143">Initializing and seeding a database</span></span>
- <span data-ttu-id="8d8c5-144">自訂樣式、 圖形和主版頁面的 UI</span><span class="sxs-lookup"><span data-stu-id="8d8c5-144">Customizing the UI with styles, graphics, and a master page</span></span>
- <span data-ttu-id="8d8c5-145">加入頁面和導覽</span><span class="sxs-lookup"><span data-stu-id="8d8c5-145">Adding pages and navigation</span></span>
- <span data-ttu-id="8d8c5-146">顯示功能表的詳細資訊和產品資料</span><span class="sxs-lookup"><span data-stu-id="8d8c5-146">Displaying menu details and product data</span></span>
- <span data-ttu-id="8d8c5-147">建立購物車</span><span class="sxs-lookup"><span data-stu-id="8d8c5-147">Creating a shopping cart</span></span>
- <span data-ttu-id="8d8c5-148">新增 SSL 和 OAuth 支援</span><span class="sxs-lookup"><span data-stu-id="8d8c5-148">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="8d8c5-149">新增付款方法</span><span class="sxs-lookup"><span data-stu-id="8d8c5-149">Adding a payment method</span></span>
- <span data-ttu-id="8d8c5-150">包括系統管理員角色和使用者使用應用程式</span><span class="sxs-lookup"><span data-stu-id="8d8c5-150">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="8d8c5-151">限制存取特定頁面以及資料夾</span><span class="sxs-lookup"><span data-stu-id="8d8c5-151">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="8d8c5-152">將檔案上傳至 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="8d8c5-152">Uploading a file to the web application</span></span>
- <span data-ttu-id="8d8c5-153">實作輸入的驗證</span><span class="sxs-lookup"><span data-stu-id="8d8c5-153">Implementing input validation</span></span>
- <span data-ttu-id="8d8c5-154">註冊 web 應用程式的路由</span><span class="sxs-lookup"><span data-stu-id="8d8c5-154">Registering routes for the web application</span></span>
- <span data-ttu-id="8d8c5-155">實作錯誤處理和錯誤記錄</span><span class="sxs-lookup"><span data-stu-id="8d8c5-155">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="8d8c5-156">總覽</span><span class="sxs-lookup"><span data-stu-id="8d8c5-156">Overview</span></span>

<span data-ttu-id="8d8c5-157">本教學課程系列是為人熟悉的程式設計概念，但新的 ASP.NET Web form。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-157">This tutorial series is intended for someone familiar with programming concepts, but new to ASP.NET Web Forms.</span></span> <span data-ttu-id="8d8c5-158">如果您已熟悉 ASP.NET Web Form，這一系列仍有助於您了解新的 ASP.NET 4.5 功能。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-158">If you're already familiar with ASP.NET Web Forms, this series can still help you learn about new ASP.NET 4.5 features.</span></span> <span data-ttu-id="8d8c5-159">如讀者熟悉程式設計概念和 ASP.NET Web Form，請參閱其他的 Web Form 教學課程中提供[開始使用](../../../index.md)ASP.NET 網站上的一節。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-159">For readers unfamiliar with programming concepts and ASP.NET Web Forms, see the additional Web Forms tutorials provided in the [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="8d8c5-160">在本教學課程系列中提供的 ASP.NET 4.5 包含下列功能：</span><span class="sxs-lookup"><span data-stu-id="8d8c5-160">The ASP.NET 4.5 provided in this tutorial series includes the following features:</span></span>

- <span data-ttu-id="8d8c5-161">提供用於建立專案的簡單 UI[許多 ASP.NET 架構支援](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add)（Web Form、 MVC 和 Web API）。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-161">A simple UI for creating projects that offers [support for many ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="8d8c5-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)、 配置、 佈景主題，以及回應式設計架構。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout, theming, and responsive design framework.</span></span>
- <span data-ttu-id="8d8c5-163">[ASP.NET Identity](../../../../identity/index.md)，新的 ASP.NET 成員資格系統，與 web 裝載軟體，而不是 IIS 的運作方式在所有 ASP.NET framework 和運作方式相同。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-163">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- [<span data-ttu-id="8d8c5-164">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8d8c5-164">Entity Framework 6</span></span>](https://msdn.microsoft.com/data/ef.aspx)

  <span data-ttu-id="8d8c5-165">Entity Framework，讓您更新：</span><span class="sxs-lookup"><span data-stu-id="8d8c5-165">An update to the Entity Framework enabling you to:</span></span>
  - <span data-ttu-id="8d8c5-166">擷取與操作資料當做強型別物件</span><span class="sxs-lookup"><span data-stu-id="8d8c5-166">Retrieve and manipulate data as strongly-typed objects</span></span>
  - <span data-ttu-id="8d8c5-167">以非同步方式存取資料</span><span class="sxs-lookup"><span data-stu-id="8d8c5-167">Access data asynchronously</span></span>
  - <span data-ttu-id="8d8c5-168">處理暫時性連接失敗</span><span class="sxs-lookup"><span data-stu-id="8d8c5-168">Handle transient connection faults</span></span>
  - <span data-ttu-id="8d8c5-169">記錄 SQL 陳述式</span><span class="sxs-lookup"><span data-stu-id="8d8c5-169">Log SQL statements</span></span>

<span data-ttu-id="8d8c5-170">如需完整的 ASP.NET 4.5 功能清單，請參閱 < [ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊](../../../../visual-studio/overview/2013/release-notes.md)。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-170">For a complete ASP.NET 4.5 feature list, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="8d8c5-171">Wingtip Toys 範例應用程式</span><span class="sxs-lookup"><span data-stu-id="8d8c5-171">The Wingtip Toys sample application</span></span>

<span data-ttu-id="8d8c5-172">下列螢幕擷取畫面是來自您在本教學課程系列中建立 ASP.NET Web Forms 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-172">The following screenshots are from the ASP.NET Web Forms application that you create in this tutorial series.</span></span> <span data-ttu-id="8d8c5-173">當您在 Visual Studio 中執行應用程式時，下列 web 首頁隨即出現。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-173">When you run the application in Visual Studio, the following web Home page appears.</span></span>

![Wingtip Toys-預設頁面](introduction-and-overview/_static/image1.png)

<span data-ttu-id="8d8c5-175">您可以註冊為新的使用者，或現有的使用者身分登入。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-175">You can register as a new user, or sign in as an existing user.</span></span> <span data-ttu-id="8d8c5-176">頂端導覽列有產品類別和其產品以從資料庫中的連結。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-176">The top navigation has links to product categories and their products from the database.</span></span>

<span data-ttu-id="8d8c5-177">如果您選取**產品**，會顯示所有可用的產品。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-177">If you select **Products**, all available products are displayed.</span></span> 

![Wingtip Toys-產品](introduction-and-overview/_static/image2.png)

<span data-ttu-id="8d8c5-179">如果您選取特定產品時，會顯示產品詳細資料。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-179">If you select a specific product, product details are displayed.</span></span>


![Wingtip Toys-產品詳細資料](introduction-and-overview/_static/image3.png)

<span data-ttu-id="8d8c5-181">身為使用者，您可以註冊及登入 Web 表單範本的預設功能。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-181">As a user, you can register and sign in with Web Forms template default functionality.</span></span> <span data-ttu-id="8d8c5-182">本教學課程也說明如何使用現有的 Gmail 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-182">This tutorial also explains how to sign in using an existing Gmail account.</span></span> <span data-ttu-id="8d8c5-183">此外，您可以新增和移除資料庫中的產品的系統管理員身分登入。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-183">Additionally, you can sign in as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys-登入](introduction-and-overview/_static/image4.png)

<span data-ttu-id="8d8c5-185">一旦您的使用者身分登入，您可以新增產品至購物車及使用 PayPal 簽出。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-185">Once you've signed in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="8d8c5-186">範例應用程式被設計用於 PayPal 的開發人員沙箱中。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-186">The sample application is designed to work in PayPal's developer sandbox.</span></span> <span data-ttu-id="8d8c5-187">沒有實際的金錢交易將會發生。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-187">No actual money transaction takes place.</span></span>

![Wingtip Toys-購物車](introduction-and-overview/_static/image5.png)

<span data-ttu-id="8d8c5-189">PayPal 確認您的帳戶、 訂單和付款資訊。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-189">PayPal confirms your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="8d8c5-191">傳回從 PayPal 之後, 您可以檢閱並完成您的訂單。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-191">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys-順序檢閱](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="8d8c5-193">必要條件</span><span class="sxs-lookup"><span data-stu-id="8d8c5-193">Prerequisites</span></span>

<span data-ttu-id="8d8c5-194">在開始之前，請確定您的電腦上安裝下列軟體：</span><span class="sxs-lookup"><span data-stu-id="8d8c5-194">Before you start, make sure the following software is installed on your computer:</span></span>

- <span data-ttu-id="8d8c5-195">[Microsoft Visual Studio 2017 或 Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-195">[Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span></span>

<span data-ttu-id="8d8c5-196">.NET Framework 會自動安裝。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-196">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="8d8c5-197">本教學課程系列會使用 Microsoft Visual Studio Community 2017。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-197">This tutorial series uses Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="8d8c5-198">您可以使用其中一個，或 Microsoft Visual Studio 2017 才能完成本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-198">You can use either that or Microsoft Visual Studio 2017 to complete this tutorial series.</span></span>

<span data-ttu-id="8d8c5-199">請注意下列有關 Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="8d8c5-199">Note the following about Visual Studio:</span></span>

* <span data-ttu-id="8d8c5-200">Microsoft Visual Studio 2017 和 Microsoft Visual Studio Community 2017 指*Visual Studio*在此教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-200">Microsoft Visual Studio 2017 and Microsoft Visual Studio Community 2017 are referred to as *Visual Studio* throughout this tutorial series.</span></span>

* <span data-ttu-id="8d8c5-201">Visual Studio 2017 安裝已安裝任何舊版旁邊。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-201">Visual Studio 2017 is installed next to any older versions already installed.</span></span> <span data-ttu-id="8d8c5-202">在舊版中建立的網站可以在 Visual Studio 2017 中開啟，並繼續在舊版中開啟。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-202">Sites created in earlier versions can be opened in Visual Studio 2017 and continue to open in previous versions.</span></span>

* <span data-ttu-id="8d8c5-203">第一次啟動 Visual Studio，則會假設您已選取*Web 開發*設定。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-203">The first time you started Visual Studio, it is assumed you selected the *Web Development* settings.</span></span> <span data-ttu-id="8d8c5-204">如需詳細資訊，請參閱[＜How to：選取 Web 開發環境設定](https://msdn.microsoft.com/library/ff521558.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-204">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

<span data-ttu-id="8d8c5-205">安裝必要條件之後, 您就可以開始建立本教學課程系列中的 Web 專案項目。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-205">After installing the prerequisites, you're ready to begin creating the Web project presented in this tutorial series.</span></span>

## <a name="download-the-sample-application"></a><span data-ttu-id="8d8c5-206">下載範例應用程式</span><span class="sxs-lookup"><span data-stu-id="8d8c5-206">Download the sample application</span></span>

 <span data-ttu-id="8d8c5-207">您可以從 MSDN 範例網站，隨時下載完整的範例 applicatiion 在：</span><span class="sxs-lookup"><span data-stu-id="8d8c5-207">You can download the completed sample applicatiion at anytime from the MSDN Samples site:</span></span>

<span data-ttu-id="8d8c5-208">[開始使用 ASP.NET 4.5 Web Form 與 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="8d8c5-208">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

 <span data-ttu-id="8d8c5-209">此下載有下列項目：</span><span class="sxs-lookup"><span data-stu-id="8d8c5-209">This download has the following items:</span></span>

- <span data-ttu-id="8d8c5-210">中的範例應用程式*WingtipToys*資料夾。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-210">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="8d8c5-211">若要建立範例應用程式中的使用的資源*WingtipToys 資產*中的資料夾*WingtipToys*資料夾。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-211">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

<span data-ttu-id="8d8c5-212">下載 *.zip*檔案。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-212">The download is a *.zip* file.</span></span> <span data-ttu-id="8d8c5-213">若要查看已完成本教學課程系列所建立的專案，尋找並選取*C#* .zip 檔案的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-213">To see the completed project that this tutorial series creates, find and select the *C#* folder in the .zip file.</span></span> <span data-ttu-id="8d8c5-214">儲存C#您用來處理與 Visual Studio 專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-214">Save the C# folder to the folder you use to work with Visual Studio projects.</span></span> <span data-ttu-id="8d8c5-215">根據預設，Visual Studio 2017 的 [專案] 資料夾是：</span><span class="sxs-lookup"><span data-stu-id="8d8c5-215">By default, the Visual Studio 2017 projects folder is:</span></span>

<span data-ttu-id="8d8c5-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span><span class="sxs-lookup"><span data-stu-id="8d8c5-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span></span>

<span data-ttu-id="8d8c5-217">重新命名***C#*** 資料夾，以***WingtipToys***。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-217">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="8d8c5-218">如果您已經有一個名為資料夾*WingtipToys*在專案資料夾中，暫時重新命名該現有的資料夾重新命名之前*C#* 資料夾*WingtipToys*。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-218">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>

<span data-ttu-id="8d8c5-219">若要執行已完成的專案，開啟*WingtipToys*資料夾，然後按兩下*WingtipToys.sln*檔案。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-219">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="8d8c5-220">Visual Studio 2017 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-220">Visual Studio 2017 opens the project.</span></span> <span data-ttu-id="8d8c5-221">接下來，以滑鼠右鍵按一下*Default.aspx*中的檔案**方案總管**，然後選取**瀏覽器中檢視**。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-221">Next, right-click the *Default.aspx* file in **Solution Explorer** and select **View In Browser**.</span></span>

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a><span data-ttu-id="8d8c5-222">ASP.NET Web Form 測驗來檢閱內容</span><span class="sxs-lookup"><span data-stu-id="8d8c5-222">Take a ASP.NET Web Forms quiz to review content</span></span>

<span data-ttu-id="8d8c5-223">完成之後的教學課程系列，測驗測試您的知識，並強化的重要概念。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-223">After completing the tutorial series, take a quiz to test your knowledge and reinforce key concepts.</span></span> <span data-ttu-id="8d8c5-224">每個問題提供的說明和其他指引的連結。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-224">Each question provides an explanation and links to additional guidance.</span></span>

 * [<span data-ttu-id="8d8c5-225">ASP.NET Web Form 測驗</span><span class="sxs-lookup"><span data-stu-id="8d8c5-225">ASP.NET Web Forms Quiz</span></span>](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a><span data-ttu-id="8d8c5-226">教學課程的支援和註解</span><span class="sxs-lookup"><span data-stu-id="8d8c5-226">Tutorial support and comments</span></span>

<span data-ttu-id="8d8c5-227">問題和註解，請使用包含在問與答一節[Getting Started with ASP.NET 4.5 Web Form 與 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 範例頁面。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-227">For questions and comments, use the Q and A section included on the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span>

<span data-ttu-id="8d8c5-228">在本教學課程系列的註解是歡迎畫面。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-228">Comments on this tutorial series are welcome.</span></span> <span data-ttu-id="8d8c5-229">本教學課程系列更新時，盡量考慮更正或建議的改進。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-229">When this tutorial series is updated, every effort is made to consider corrections or suggestions for improvements.</span></span>

<span data-ttu-id="8d8c5-230">如果發生錯誤，對應的錯誤訊息可能會造成混淆，沒有很好的說明，如何修正此問題。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-230">If an error occurs, the corresponding error messages could be confusing, with no good explanation on how to fix it.</span></span> <span data-ttu-id="8d8c5-231">如需協助，您可以檢查[ASP.NET 論壇](https://forums.asp.net/)。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-231">For help, you can check the [ASP.NET forums](https://forums.asp.net/).</span></span> <span data-ttu-id="8d8c5-232">另一個不錯的來源是中的問與答一節[Getting Started with ASP.NET 4.5 Web Form 與 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 範例頁面。</span><span class="sxs-lookup"><span data-stu-id="8d8c5-232">Another good source is the Q and A section in the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="8d8c5-233">下一步</span><span class="sxs-lookup"><span data-stu-id="8d8c5-233">Next</span></span>](create-the-project.md)
