---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 第 6 節： ASP.NET 成員資格 |Microsoft Docs
author: JoeStagner
description: 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 6 部分加入 ASP.NET 成員資格。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6d01a745ca1428f065f564f676d483ee807eb52e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368469"
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="312c7-104">第 6 節： ASP.NET 成員資格</span><span class="sxs-lookup"><span data-stu-id="312c7-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="312c7-105">藉由[Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="312c7-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="312c7-106">Tailspin Spyworks 示範建立功能強大、 可擴充的應用程式，適用於.NET 平台是如何富含簡單。</span><span class="sxs-lookup"><span data-stu-id="312c7-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="312c7-107">它會展示如何在 ASP.NET 4 中使用最棒的新功能，建置線上商店，包括購物、 簽出，以及系統管理。</span><span class="sxs-lookup"><span data-stu-id="312c7-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="312c7-108">本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="312c7-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="312c7-109">第 6 部分加入 ASP.NET 成員資格。</span><span class="sxs-lookup"><span data-stu-id="312c7-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a>  <span data-ttu-id="312c7-110">使用 ASP.NET 成員資格</span><span class="sxs-lookup"><span data-stu-id="312c7-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="312c7-111">按一下 [安全性]</span><span class="sxs-lookup"><span data-stu-id="312c7-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="312c7-112">請確定我們使用表單驗證。</span><span class="sxs-lookup"><span data-stu-id="312c7-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="312c7-113">您可以使用 [建立使用者] 連結來建立幾個使用者。</span><span class="sxs-lookup"><span data-stu-id="312c7-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="312c7-114">完成時，請參閱 [方案總管] 視窗，並重新整理檢視。</span><span class="sxs-lookup"><span data-stu-id="312c7-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="312c7-115">請注意，ASPNETDB。已建立好的 MDF。</span><span class="sxs-lookup"><span data-stu-id="312c7-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="312c7-116">此檔案包含的資料表，以支援核心 ASP.NET 服務，例如成員資格。</span><span class="sxs-lookup"><span data-stu-id="312c7-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="312c7-117">現在我們可以開始實作結帳程序。</span><span class="sxs-lookup"><span data-stu-id="312c7-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="312c7-118">藉由建立 CheckOut.aspx 頁面開始。</span><span class="sxs-lookup"><span data-stu-id="312c7-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="312c7-119">CheckOut.aspx 頁面應僅供已登入，因此我們將會限制存取權登入使用者以及未登入的登入頁面的重新導向使用者的使用者。</span><span class="sxs-lookup"><span data-stu-id="312c7-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="312c7-120">若要這樣做我們會將下列加入我們的 web.config 檔案的組態區段。</span><span class="sxs-lookup"><span data-stu-id="312c7-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="312c7-121">ASP.NET Web Form 應用程式範本會自動加入我們的 web.config 檔案中的 [驗證] 區段，並建立預設登入頁面。</span><span class="sxs-lookup"><span data-stu-id="312c7-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="312c7-122">我們必須修改 Login.aspx 背後的程式碼移轉匿名的購物車，當使用者登入的檔案。</span><span class="sxs-lookup"><span data-stu-id="312c7-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="312c7-123">變更頁面\_載入事件，如下所示。</span><span class="sxs-lookup"><span data-stu-id="312c7-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="312c7-124">然後新增"LoggedIn"事件處理常式如下設定新登入的使用者工作階段名稱，並變更使用者的購物車中的暫時工作階段識別碼，我們 MyShoppingCart 類別中呼叫 MigrateCart; 方法。</span><span class="sxs-lookup"><span data-stu-id="312c7-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="312c7-125">（在.cs 檔案中實作）</span><span class="sxs-lookup"><span data-stu-id="312c7-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="312c7-126">實作 MigrateCart() 方法如下所示。</span><span class="sxs-lookup"><span data-stu-id="312c7-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="312c7-127">在 checkout.aspx 我們將使用 EntityDataSource 和 GridView 中我們查看頁面就像我們在我們購物車 頁面。</span><span class="sxs-lookup"><span data-stu-id="312c7-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="312c7-128">請注意，我們的 GridView 控制項指定名為 MyList"ondatabound"事件處理常式\_RowDataBound 現在讓我們來實作該事件處理常式，就像這樣。</span><span class="sxs-lookup"><span data-stu-id="312c7-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="312c7-129">加總的購物車每個資料列繫結，並更新 GridView 的底端列這個方法會保留。</span><span class="sxs-lookup"><span data-stu-id="312c7-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="312c7-130">在這個階段中，我們已實作放置順序 「 檢閱 」 的簡報。</span><span class="sxs-lookup"><span data-stu-id="312c7-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="312c7-131">讓我們加入我們的頁面中的幾行程式碼處理空白的購物車案例\_Load 事件：</span><span class="sxs-lookup"><span data-stu-id="312c7-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="312c7-132">當使用者按一下 [提交] 按鈕時我們會執行下列程式碼中的提交按鈕 Click 事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="312c7-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="312c7-133">「"訂單提交程序是在 SubmitOrder() 方法中，我們 MyShoppingCart 類別的實作。</span><span class="sxs-lookup"><span data-stu-id="312c7-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="312c7-134">SubmitOrder 將會：</span><span class="sxs-lookup"><span data-stu-id="312c7-134">SubmitOrder will:</span></span>

- <span data-ttu-id="312c7-135">購物車中採取所有明細項目，並使用它們來建立新的訂單記錄和相關聯的 OrderDetails 記錄。</span><span class="sxs-lookup"><span data-stu-id="312c7-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="312c7-136">計算出貨日期。</span><span class="sxs-lookup"><span data-stu-id="312c7-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="312c7-137">清除 購物車。</span><span class="sxs-lookup"><span data-stu-id="312c7-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="312c7-138">基於此範例應用程式的中，我們會計算出貨日期只要目前的日期加上兩天。</span><span class="sxs-lookup"><span data-stu-id="312c7-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="312c7-139">執行應用程式現在將允許我們購物程序從開始到完成的測試。</span><span class="sxs-lookup"><span data-stu-id="312c7-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="312c7-140">[上一頁](tailspin-spyworks-part-5.md)
> [下一頁](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="312c7-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
