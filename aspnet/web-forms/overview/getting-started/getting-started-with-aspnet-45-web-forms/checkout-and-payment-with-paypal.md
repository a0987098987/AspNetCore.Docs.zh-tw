---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: 簽出與使用 PayPal 付款 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 3299da33a68f02ac1b3ffe7c037d06d8ece9455e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835627"
---
<a name="checkout-and-payment-with-paypal"></a><span data-ttu-id="db9f8-103">簽出與使用 PayPal 付款</span><span class="sxs-lookup"><span data-stu-id="db9f8-103">Checkout and Payment with PayPal</span></span>
====================
<span data-ttu-id="db9f8-104">藉由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="db9f8-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="db9f8-105">[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="db9f8-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="db9f8-106">本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="db9f8-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="db9f8-107">Visual Studio 2013[含有 C# 原始程式碼專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附了本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="db9f8-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="db9f8-108">本教學課程說明如何修改 Wingtip Toys 範例應用程式，以包含使用者的授權資料、 註冊與使用 PayPal 付款。</span><span class="sxs-lookup"><span data-stu-id="db9f8-108">This tutorial describes how to modify the Wingtip Toys sample application to include user authorization, registration, and payment using PayPal.</span></span> <span data-ttu-id="db9f8-109">只有已登入的使用者必須購買產品的授權。</span><span class="sxs-lookup"><span data-stu-id="db9f8-109">Only users who are logged in will have authorization to purchase products.</span></span> <span data-ttu-id="db9f8-110">ASP.NET 4.5 Web Form 專案範本的內建的使用者註冊功能已包含許多您的需要。</span><span class="sxs-lookup"><span data-stu-id="db9f8-110">The ASP.NET 4.5 Web Forms project template's built-in user registration functionality already includes much of what you need.</span></span> <span data-ttu-id="db9f8-111">您將新增 PayPal Express 簽出功能。</span><span class="sxs-lookup"><span data-stu-id="db9f8-111">You will add PayPal Express Checkout functionality.</span></span> <span data-ttu-id="db9f8-112">在本教學課程中，您會使用 PayPal 開發人員測試環境，因此會不傳輸任何實際的資金。</span><span class="sxs-lookup"><span data-stu-id="db9f8-112">In this tutorial you be using the PayPal developer testing environment, so no actual funds will be transferred.</span></span> <span data-ttu-id="db9f8-113">在本教學課程結束時，您將測試應用程式，選取要加入 「 購物車 」 中，按一下 [簽出] 按鈕，以及傳輸資料到 PayPal 測試網站的產品。</span><span class="sxs-lookup"><span data-stu-id="db9f8-113">At the end of the tutorial, you will test the application by selecting products to add to the shopping cart, clicking the checkout button, and transferring data to the PayPal testing web site.</span></span> <span data-ttu-id="db9f8-114">PayPal 測試網站上，您會確認您的傳送和付款資訊，然後返回本機 Wingtip Toys 範例應用程式，以確認及完成購買程序。</span><span class="sxs-lookup"><span data-stu-id="db9f8-114">On the PayPal testing web site, you will confirm your shipping and payment information and then return to the local Wingtip Toys sample application to confirm and complete the purchase.</span></span>

<span data-ttu-id="db9f8-115">有數個特製化的經驗豐富的第三方付款處理器中線上購物，該位址延展性和安全性。</span><span class="sxs-lookup"><span data-stu-id="db9f8-115">There are several experienced third-party payment processors that specialize in online shopping that address scalability and security.</span></span> <span data-ttu-id="db9f8-116">ASP.NET 開發人員應該考慮使用協力廠商付款解決方案之前實作購物和購買方案的優點。</span><span class="sxs-lookup"><span data-stu-id="db9f8-116">ASP.NET developers should consider the advantages of utilizing a third party payment solution before implementing a shopping and purchasing solution.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="db9f8-117">Wingtip Toys 範例應用程式被設計成以 ASP.NET web 開發人員顯示 ASP.NET 的特定概念和可用的功能。</span><span class="sxs-lookup"><span data-stu-id="db9f8-117">The Wingtip Toys sample application was designed to shown specific ASP.NET concepts and features available to ASP.NET web developers.</span></span> <span data-ttu-id="db9f8-118">此範例應用程式不被適合所有可能的情況下，對於延展性和安全性。</span><span class="sxs-lookup"><span data-stu-id="db9f8-118">This sample application was not optimized for all possible circumstances in regard to scalability and security.</span></span>


## <a name="what-youll-learn"></a><span data-ttu-id="db9f8-119">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="db9f8-119">What you'll learn:</span></span>

- <span data-ttu-id="db9f8-120">如何限制存取特定資料夾中的頁面。</span><span class="sxs-lookup"><span data-stu-id="db9f8-120">How to restrict access to specific pages in a folder.</span></span>
- <span data-ttu-id="db9f8-121">如何建立從匿名的購物車 」 的已知的購物車。</span><span class="sxs-lookup"><span data-stu-id="db9f8-121">How to create a known shopping cart from an anonymous shopping cart.</span></span>
- <span data-ttu-id="db9f8-122">如何為專案啟用 SSL。</span><span class="sxs-lookup"><span data-stu-id="db9f8-122">How to enable SSL for the project.</span></span>
- <span data-ttu-id="db9f8-123">如何將 OAuth 提供者新增至專案。</span><span class="sxs-lookup"><span data-stu-id="db9f8-123">How to add an OAuth provider to the project.</span></span>
- <span data-ttu-id="db9f8-124">如何使用 PayPal 購買產品使用 PayPal 測試環境。</span><span class="sxs-lookup"><span data-stu-id="db9f8-124">How to use PayPal to purchase products using the PayPal testing environment.</span></span>
- <span data-ttu-id="db9f8-125">如何顯示詳細資料，從在 PayPal **DetailsView**控制項。</span><span class="sxs-lookup"><span data-stu-id="db9f8-125">How to display details from PayPal in a **DetailsView** control.</span></span>
- <span data-ttu-id="db9f8-126">如何更新 Wingtip Toys 應用程式的資料庫，以取得從 PayPal 的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="db9f8-126">How to update the database of the Wingtip Toys application with details obtained from PayPal.</span></span>

## <a name="adding-order-tracking"></a><span data-ttu-id="db9f8-127">新增訂單追蹤</span><span class="sxs-lookup"><span data-stu-id="db9f8-127">Adding Order Tracking</span></span>

<span data-ttu-id="db9f8-128">在本教學課程中，您將建立兩個新的類別，來追蹤資料從使用者所建立的順序。</span><span class="sxs-lookup"><span data-stu-id="db9f8-128">In this tutorial, you'll create two new classes to track data from the order a user has created.</span></span> <span data-ttu-id="db9f8-129">類別會追蹤出貨資訊; 購買總計，以及付款確認的資料。</span><span class="sxs-lookup"><span data-stu-id="db9f8-129">The classes will track data regarding shipping information, purchase total, and payment confirmation.</span></span>

### <a name="add-the-order-and-orderdetail-model-classes"></a><span data-ttu-id="db9f8-130">新增的順序和 OrderDetail 模型類別</span><span class="sxs-lookup"><span data-stu-id="db9f8-130">Add the Order and OrderDetail Model Classes</span></span>

<span data-ttu-id="db9f8-131">稍早在本教學課程系列中，您已定義之分類的產品，結構描述，而且建立購物車項目`Category`， `Product`，並`CartItem`中的類別*模型*資料夾。</span><span class="sxs-lookup"><span data-stu-id="db9f8-131">Earlier in this tutorial series, you defined the schema for categories, products, and shopping cart items by creating the `Category`, `Product`, and `CartItem` classes in the *Models* folder.</span></span> <span data-ttu-id="db9f8-132">現在您將加入兩個新的類別，來定義產品訂單及訂單的詳細資料的結構描述。</span><span class="sxs-lookup"><span data-stu-id="db9f8-132">Now you will add two new classes to define the schema for the product order and the details of the order.</span></span>

1. <span data-ttu-id="db9f8-133">在 **模型**資料夾中，加入新的類別，名為*Order.cs*。</span><span class="sxs-lookup"><span data-stu-id="db9f8-133">In the **Models** folder, add a new class named *Order.cs*.</span></span>   
   <span data-ttu-id="db9f8-134">新的類別檔案會顯示在編輯器中。</span><span class="sxs-lookup"><span data-stu-id="db9f8-134">The new class file is displayed in the editor.</span></span>
2. <span data-ttu-id="db9f8-135">您可以將預設的程式碼取代為下列：</span><span class="sxs-lookup"><span data-stu-id="db9f8-135">Replace the default code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. <span data-ttu-id="db9f8-136">新增*OrderDetail.cs*類別，即可*模型*資料夾。</span><span class="sxs-lookup"><span data-stu-id="db9f8-136">Add an *OrderDetail.cs* class to the *Models* folder.</span></span>
4. <span data-ttu-id="db9f8-137">取代為下列程式碼中的預設程式碼：</span><span class="sxs-lookup"><span data-stu-id="db9f8-137">Replace the default code with the following code:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

<span data-ttu-id="db9f8-138">`Order`和`OrderDetail`類別包含的結構描述，以定義用於購買和出貨的訂單資訊。</span><span class="sxs-lookup"><span data-stu-id="db9f8-138">The `Order` and `OrderDetail` classes contain the schema to define the order information used for purchasing and shipping.</span></span>

<span data-ttu-id="db9f8-139">此外，您必須更新資料庫內容類別，可管理的實體類別，並提供對資料庫的資料存取。</span><span class="sxs-lookup"><span data-stu-id="db9f8-139">In addition, you will need to update the database context class that manages the entity classes and that provides data access to the database.</span></span> <span data-ttu-id="db9f8-140">若要這樣做，您將加入新建立的順序並`OrderDetail`模型類別來`ProductContext`類別。</span><span class="sxs-lookup"><span data-stu-id="db9f8-140">To do this, you will add the newly created Order and `OrderDetail` model classes to `ProductContext` class.</span></span>

1. <span data-ttu-id="db9f8-141">在 **方案總管**，尋找並開啟*ProductContext.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="db9f8-141">In **Solution Explorer**, find and open the *ProductContext.cs* file.</span></span>
2. <span data-ttu-id="db9f8-142">將反白顯示的程式碼加入*ProductContext.cs*檔案，如下所示：</span><span class="sxs-lookup"><span data-stu-id="db9f8-142">Add the highlighted code to the *ProductContext.cs* file as shown below:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

<span data-ttu-id="db9f8-143">如先前在本教學課程系列中的程式碼中所述*ProductContext.cs*檔都會將`System.Data.Entity`命名空間，所以您需要的 Entity framework 的所有核心功能的存取權。</span><span class="sxs-lookup"><span data-stu-id="db9f8-143">As mentioned previously in this tutorial series, the code in the *ProductContext.cs* file adds the `System.Data.Entity` namespace so that you have access to all the core functionality of the Entity Framework.</span></span> <span data-ttu-id="db9f8-144">這項功能包括查詢、 插入、 更新和刪除資料，藉由使用強型別物件的功能。</span><span class="sxs-lookup"><span data-stu-id="db9f8-144">This functionality includes the capability to query, insert, update, and delete data by working with strongly typed objects.</span></span> <span data-ttu-id="db9f8-145">在上述程式碼`ProductContext`類別加入至新加入的 Entity Framework 存取`Order`和`OrderDetail`類別。</span><span class="sxs-lookup"><span data-stu-id="db9f8-145">The above code in the `ProductContext` class adds Entity Framework access to the newly added `Order` and `OrderDetail` classes.</span></span>

## <a name="adding-checkout-access"></a><span data-ttu-id="db9f8-146">新增簽出的存取權</span><span class="sxs-lookup"><span data-stu-id="db9f8-146">Adding Checkout Access</span></span>

<span data-ttu-id="db9f8-147">Wingtip Toys 範例應用程式可讓匿名使用者檢閱，並將產品加入購物車。</span><span class="sxs-lookup"><span data-stu-id="db9f8-147">The Wingtip Toys sample application allows anonymous users to review and add products to a shopping cart.</span></span> <span data-ttu-id="db9f8-148">不過，當匿名使用者選擇購買它們新增至購物車的產品時，他們必須登入至站台。</span><span class="sxs-lookup"><span data-stu-id="db9f8-148">However, when anonymous users choose to purchase the products they added to the shopping cart, they must log on to the site.</span></span> <span data-ttu-id="db9f8-149">一旦登入，他們可以存取受限制的網頁之 Web 應用程式，處理簽出，並購買程序。</span><span class="sxs-lookup"><span data-stu-id="db9f8-149">Once they have logged on, they can access the restricted pages of the Web application that handle the checkout and purchase process.</span></span> <span data-ttu-id="db9f8-150">這些限制的頁面包含在*簽出*應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="db9f8-150">These restricted pages are contained in the *Checkout* folder of the application.</span></span>

### <a name="add-a-checkout-folder-and-pages"></a><span data-ttu-id="db9f8-151">新增簽出資料夾和網頁</span><span class="sxs-lookup"><span data-stu-id="db9f8-151">Add a Checkout Folder and Pages</span></span>

<span data-ttu-id="db9f8-152">您現在會建立*簽出*資料夾和它的客戶會看到在結帳程序中的頁面。</span><span class="sxs-lookup"><span data-stu-id="db9f8-152">You will now create the *Checkout* folder and the pages in it that the customer will see during the checkout process.</span></span> <span data-ttu-id="db9f8-153">本教學課程稍後，您將會更新這些頁面。</span><span class="sxs-lookup"><span data-stu-id="db9f8-153">You will update these pages later in this tutorial.</span></span>

1. <span data-ttu-id="db9f8-154">以滑鼠右鍵按一下專案名稱 (**Wingtip Toys**) 中**方案總管**，然後選取**加入新的資料夾**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-154">Right-click the project name (**Wingtip Toys**) in **Solution Explorer** and select **Add a New Folder**.</span></span> 

    ![簽出和付款使用 PayPal-新的資料夾](checkout-and-payment-with-paypal/_static/image1.png)
2. <span data-ttu-id="db9f8-156">將新資料夾命名*簽出*。</span><span class="sxs-lookup"><span data-stu-id="db9f8-156">Name the new folder *Checkout*.</span></span>
3. <span data-ttu-id="db9f8-157">以滑鼠右鍵按一下*簽出*資料夾，然後選取**新增**-&gt;**新項目**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-157">Right-click the *Checkout* folder and then select **Add**-&gt;**New Item**.</span></span> 

    ![簽出和付款使用 PayPal-新的項目](checkout-and-payment-with-paypal/_static/image2.png)
4. <span data-ttu-id="db9f8-159">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="db9f8-159">The **Add New Item** dialog box is displayed.</span></span>
5. <span data-ttu-id="db9f8-160">選取  **Visual C#**  - &gt; **Web**左側的 範本 群組。</span><span class="sxs-lookup"><span data-stu-id="db9f8-160">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="db9f8-161">然後，從中間窗格中，選取**使用主版頁面的 Web Form**並將它命名*CheckoutStart.aspx*。</span><span class="sxs-lookup"><span data-stu-id="db9f8-161">Then, from the middle pane, select **Web Form with Master Page**and name it *CheckoutStart.aspx*.</span></span> 

    ![簽出與使用 PayPal 付款-加入新的項目 對話方塊](checkout-and-payment-with-paypal/_static/image3.png)
6. <span data-ttu-id="db9f8-163">同樣地，選取*Site.Master*主版頁面檔案。</span><span class="sxs-lookup"><span data-stu-id="db9f8-163">As before, select the *Site.Master* file as the master page.</span></span>
7. <span data-ttu-id="db9f8-164">加入下列的其他頁面，以*簽出*資料夾使用上述相同步驟：</span><span class="sxs-lookup"><span data-stu-id="db9f8-164">Add the following additional pages to the *Checkout* folder using the same steps above:</span></span>   

    - <span data-ttu-id="db9f8-165">CheckoutReview.aspx</span><span class="sxs-lookup"><span data-stu-id="db9f8-165">CheckoutReview.aspx</span></span>
    - <span data-ttu-id="db9f8-166">CheckoutComplete.aspx</span><span class="sxs-lookup"><span data-stu-id="db9f8-166">CheckoutComplete.aspx</span></span>
    - <span data-ttu-id="db9f8-167">CheckoutCancel.aspx</span><span class="sxs-lookup"><span data-stu-id="db9f8-167">CheckoutCancel.aspx</span></span>
    - <span data-ttu-id="db9f8-168">CheckoutError.aspx</span><span class="sxs-lookup"><span data-stu-id="db9f8-168">CheckoutError.aspx</span></span>

### <a name="add-a-webconfig-file"></a><span data-ttu-id="db9f8-169">加入 Web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="db9f8-169">Add a Web.config File</span></span>

<span data-ttu-id="db9f8-170">藉由新增新*Web.config*的檔案*簽出*資料夾中，您將能夠存取限於包含在資料夾中的所有頁面。</span><span class="sxs-lookup"><span data-stu-id="db9f8-170">By adding a new *Web.config* file to the *Checkout* folder, you will be able to restrict access to all the pages contained in the folder.</span></span>

1. <span data-ttu-id="db9f8-171">以滑鼠右鍵按一下*簽出*資料夾，然後選取**新增** - &gt; **新項目**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-171">Right-click the *Checkout* folder and select **Add** -&gt; **New Item**.</span></span>  
   <span data-ttu-id="db9f8-172">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="db9f8-172">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="db9f8-173">選取  **Visual C#**  - &gt; **Web**左側的 範本 群組。</span><span class="sxs-lookup"><span data-stu-id="db9f8-173">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="db9f8-174">然後，從中間窗格中，選取**Web 組態檔**，接受預設名稱*Web.config*，然後選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-174">Then, from the middle pane, select **Web Configuration File**, accept the default name of *Web.config*, and then select **Add**.</span></span>
3. <span data-ttu-id="db9f8-175">取代現有的 XML 中的內容*Web.config*以下列檔案：</span><span class="sxs-lookup"><span data-stu-id="db9f8-175">Replace the existing XML content in the *Web.config* file with the following:</span></span>  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. <span data-ttu-id="db9f8-176">儲存*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="db9f8-176">Save the *Web.config* file.</span></span>

<span data-ttu-id="db9f8-177">*Web.config*檔案會指定所有未知的使用者，Web 應用程式必須拒絕存取的頁面中包含*簽出*資料夾。</span><span class="sxs-lookup"><span data-stu-id="db9f8-177">The *Web.config* file specifies that all unknown users of the Web application must be denied access to the pages contained in the *Checkout* folder.</span></span> <span data-ttu-id="db9f8-178">不過，如果使用者已註冊的帳戶，並登入，他們會是已知的使用者，而且必須在頁面的存取權*簽出*資料夾。</span><span class="sxs-lookup"><span data-stu-id="db9f8-178">However, if the user has registered an account and is logged on, they will be a known user and will have access to the pages in the *Checkout* folder.</span></span>

<span data-ttu-id="db9f8-179">請務必請注意，ASP.NET 組態如下所示的階層，其中每個*Web.config*檔案適用於組態設定中的資料夾和所有其下的子目錄。</span><span class="sxs-lookup"><span data-stu-id="db9f8-179">It's important to note that ASP.NET configuration follows a hierarchy, where each *Web.config* file applies configuration settings to the folder that it is in and to all of the child directories below it.</span></span>

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a><span data-ttu-id="db9f8-180">為專案啟用 SSL</span><span class="sxs-lookup"><span data-stu-id="db9f8-180">Enable SSL for the Project</span></span>

 <span data-ttu-id="db9f8-181">安全通訊端層 (SSL) 是定義為允許 Web 伺服器和 Web 用戶端通訊更安全地透過加密的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="db9f8-181">Secure Sockets Layer (SSL) is a protocol defined to allow Web servers and Web clients to communicate more securely through the use of encryption.</span></span> <span data-ttu-id="db9f8-182">不使用 SSL，用戶端和伺服器之間傳送的資料時，對封包探查實體存取網路的任何人開啟。</span><span class="sxs-lookup"><span data-stu-id="db9f8-182">When SSL is not used, data sent between the client and server is open to packet sniffing by anyone with physical access to the network.</span></span> <span data-ttu-id="db9f8-183">此外，數個常見的驗證結構描述是不安全的在一般的 HTTP。</span><span class="sxs-lookup"><span data-stu-id="db9f8-183">Additionally, several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="db9f8-184">特別是，基本驗證和表單驗證來傳送未加密的認證。</span><span class="sxs-lookup"><span data-stu-id="db9f8-184">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="db9f8-185">若要安全，這些驗證結構描述必須使用 SSL。</span><span class="sxs-lookup"><span data-stu-id="db9f8-185">To be secure, these authentication schemes must use SSL.</span></span> 

1. <span data-ttu-id="db9f8-186">在 [**方案總管] 中**，按一下**WingtipToys**專案，然後按**F4**顯示**屬性**視窗。</span><span class="sxs-lookup"><span data-stu-id="db9f8-186">In **Solution Explorer**, click the **WingtipToys** project, then press **F4** to display the **Properties** window.</span></span>
2. <span data-ttu-id="db9f8-187">變更**啟用 SSL**至`true`。</span><span class="sxs-lookup"><span data-stu-id="db9f8-187">Change **SSL Enabled** to `true`.</span></span>
3. <span data-ttu-id="db9f8-188">複製**SSL URL**讓您可以在稍後使用。</span><span class="sxs-lookup"><span data-stu-id="db9f8-188">Copy the **SSL URL** so you can use it later.</span></span>   
 <span data-ttu-id="db9f8-189">SSL URL 將是`https://localhost:44300/`除非您先前已建立 SSL Web Sites，（如下所示）。</span><span class="sxs-lookup"><span data-stu-id="db9f8-189">The SSL URL will be `https://localhost:44300/` unless you've previously created SSL Web Sites (as shown below).</span></span>   
    <span data-ttu-id="db9f8-190">![專案屬性](checkout-and-payment-with-paypal/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="db9f8-190">![Project Properties](checkout-and-payment-with-paypal/_static/image4.png)</span></span>
4. <span data-ttu-id="db9f8-191">在 **方案總管**，以滑鼠右鍵按一下**WingtipToys**專案，然後按一下**屬性**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-191">In **Solution Explorer**, right click the **WingtipToys** project and click **Properties**.</span></span>
5. <span data-ttu-id="db9f8-192">在左側索引標籤中，按一下**Web**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-192">In the left tab, click **Web**.</span></span>
6. <span data-ttu-id="db9f8-193">變更**專案 Url**使用**SSL URL**您稍早儲存。</span><span class="sxs-lookup"><span data-stu-id="db9f8-193">Change the **Project Url** to use the **SSL URL** that you saved earlier.</span></span>   
    <span data-ttu-id="db9f8-194">![專案 Web 屬性](checkout-and-payment-with-paypal/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="db9f8-194">![Project Web Properties](checkout-and-payment-with-paypal/_static/image5.png)</span></span>
7. <span data-ttu-id="db9f8-195">儲存頁面，按下**CTRL + S**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-195">Save the page by pressing **CTRL+S**.</span></span>
8. <span data-ttu-id="db9f8-196">按 **Ctrl+F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="db9f8-196">Press **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="db9f8-197">Visual Studio 會顯示可避開 SSL 警告的選項。</span><span class="sxs-lookup"><span data-stu-id="db9f8-197">Visual Studio will display an option to allow you to avoid SSL warnings.</span></span>
9. <span data-ttu-id="db9f8-198">按一下 **是**信任 IIS Express SSL 憑證，並繼續。</span><span class="sxs-lookup"><span data-stu-id="db9f8-198">Click **Yes** to trust the IIS Express SSL certificate and continue.</span></span>   
    <span data-ttu-id="db9f8-199">![IIS Express SSL 憑證的詳細資料](checkout-and-payment-with-paypal/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="db9f8-199">![IIS Express SSL certificate details](checkout-and-payment-with-paypal/_static/image6.png)</span></span>  
 <span data-ttu-id="db9f8-200">會顯示安全性警告。</span><span class="sxs-lookup"><span data-stu-id="db9f8-200">A Security Warning is displayed.</span></span>
10. <span data-ttu-id="db9f8-201">按一下 **是**將憑證安裝到您的 localhost。</span><span class="sxs-lookup"><span data-stu-id="db9f8-201">Click **Yes** to install the certificate to your localhost.</span></span>   
    <span data-ttu-id="db9f8-202">![安全性警告對話方塊](checkout-and-payment-with-paypal/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="db9f8-202">![Security Warning dialog box](checkout-and-payment-with-paypal/_static/image7.png)</span></span>  
 <span data-ttu-id="db9f8-203">瀏覽器視窗隨即出現。</span><span class="sxs-lookup"><span data-stu-id="db9f8-203">The browser window will be displayed.</span></span>

<span data-ttu-id="db9f8-204">您現在可以輕鬆地測試在本機使用 SSL 的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="db9f8-204">You can now easily test your Web application locally using SSL.</span></span>

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a><span data-ttu-id="db9f8-205">新增 OAuth 2.0 提供者</span><span class="sxs-lookup"><span data-stu-id="db9f8-205">Add an OAuth 2.0 Provider</span></span>

<span data-ttu-id="db9f8-206">ASP.NET Web Forms 提供成員資格和驗證的增強功能的選項。</span><span class="sxs-lookup"><span data-stu-id="db9f8-206">ASP.NET Web Forms provides enhanced options for membership and authentication.</span></span> <span data-ttu-id="db9f8-207">這些增強功能包括 OAuth。</span><span class="sxs-lookup"><span data-stu-id="db9f8-207">These enhancements include OAuth.</span></span> <span data-ttu-id="db9f8-208">OAuth 是開放式的通訊協定，可讓 web、 行動和桌面應用程式以簡單、 標準的方法中的安全授權。</span><span class="sxs-lookup"><span data-stu-id="db9f8-208">OAuth is an open protocol that allows secure authorization in a simple and standard method from web, mobile, and desktop applications.</span></span> <span data-ttu-id="db9f8-209">ASP.NET Web Forms 範本使用 OAuth 來公開 Facebook、 Twitter、 Google 和 Microsoft 作為驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="db9f8-209">The ASP.NET Web Forms template uses OAuth to expose Facebook, Twitter, Google and Microsoft as authentication providers.</span></span> <span data-ttu-id="db9f8-210">雖然本教學課程僅使用 Google 作為驗證提供者，您可以輕鬆地修改程式碼來使用任何提供者。</span><span class="sxs-lookup"><span data-stu-id="db9f8-210">Although this tutorial uses only Google as the authentication provider, you can easily modify the code to use any of the providers.</span></span> <span data-ttu-id="db9f8-211">實作其他提供者的步驟會在本教學課程中，您會看到的步驟非常類似。</span><span class="sxs-lookup"><span data-stu-id="db9f8-211">The steps to implement other providers are very similar to the steps you will see in this tutorial.</span></span>

<span data-ttu-id="db9f8-212">除了驗證之外，本教學課程也會使用角色來實作授權。</span><span class="sxs-lookup"><span data-stu-id="db9f8-212">In addition to authentication, the tutorial will also use roles to implement authorization.</span></span> <span data-ttu-id="db9f8-213">只有在您加入的使用者`canEdit`角色可以變更資料 （建立、 編輯或刪除連絡人）。</span><span class="sxs-lookup"><span data-stu-id="db9f8-213">Only those users you add to the `canEdit` role will be able to change data (create, edit, or delete contacts).</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="db9f8-214">Windows Live 應用程式只接受即時工作網站 URL，因此您無法使用本機的網站 URL，測試登入。</span><span class="sxs-lookup"><span data-stu-id="db9f8-214">Windows Live applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>


<span data-ttu-id="db9f8-215">下列步驟可讓您新增 Google 驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="db9f8-215">The following steps will allow you to add a Google authentication provider.</span></span>

1. <span data-ttu-id="db9f8-216">開啟*應用程式\_Start\Startup.Auth.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="db9f8-216">Open the *App\_Start\Startup.Auth.cs* file.</span></span>
2. <span data-ttu-id="db9f8-217">移除註解字元`app.UseGoogleAuthentication()`方法讓，則方法會顯示為如下所示：</span><span class="sxs-lookup"><span data-stu-id="db9f8-217">Remove the comment characters from the `app.UseGoogleAuthentication()` method so that the method appears as follows:</span></span> 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. <span data-ttu-id="db9f8-218">瀏覽至[Google 開發人員主控台](https://console.developers.google.com/)。</span><span class="sxs-lookup"><span data-stu-id="db9f8-218">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span> <span data-ttu-id="db9f8-219">您也必須使用您的 Google 開發人員電子郵件帳戶 (gmail.com) 登入。</span><span class="sxs-lookup"><span data-stu-id="db9f8-219">You will also need to sign-in with your Google developer email account (gmail.com).</span></span> <span data-ttu-id="db9f8-220">如果您沒有 Google 帳戶，請選取**建立帳戶**連結。</span><span class="sxs-lookup"><span data-stu-id="db9f8-220">If you do not have a Google account, select the **Create an account** link.</span></span>   
   <span data-ttu-id="db9f8-221">接下來，您會看到**Google 開發人員主控台**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-221">Next, you'll see the **Google Developers Console**.</span></span>   
    <span data-ttu-id="db9f8-222">![Google 開發人員主控台](checkout-and-payment-with-paypal/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="db9f8-222">![Google Developers Console](checkout-and-payment-with-paypal/_static/image8.png)</span></span>
4. <span data-ttu-id="db9f8-223">按一下 **建立專案**按鈕，然後輸入專案名稱和識別碼 （您可以使用預設值）。</span><span class="sxs-lookup"><span data-stu-id="db9f8-223">Click the **Create Project** button and enter a project name and ID (you can use the default values).</span></span> <span data-ttu-id="db9f8-224">然後，按一下**協議 核取方塊**並**建立** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="db9f8-224">Then, click the **agreement checkbox** and the **Create** button.</span></span>  

    ![Google-新增專案](checkout-and-payment-with-paypal/_static/image9.png)

   <span data-ttu-id="db9f8-226">在幾秒鐘的時間將會建立新的專案，您的瀏覽器會顯示新的 [專案] 頁面。</span><span class="sxs-lookup"><span data-stu-id="db9f8-226">In a few seconds the new project will be created and your browser will display the new projects page.</span></span>
5. <span data-ttu-id="db9f8-227">在左側索引標籤中，按一下**Api &amp; auth**，然後按一下**認證**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-227">In the left tab, click **APIs &amp; auth**, and then click **Credentials**.</span></span>
6. <span data-ttu-id="db9f8-228">按一下 **建立新的用戶端識別碼**下方**OAuth**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-228">Click the **Create New Client ID** under **OAuth**.</span></span>   
   <span data-ttu-id="db9f8-229">**建立用戶端識別碼**就會顯示對話方塊。</span><span class="sxs-lookup"><span data-stu-id="db9f8-229">The **Create Client ID** dialog will be displayed.</span></span>   
    <span data-ttu-id="db9f8-230">![Google-建立用戶端識別碼](checkout-and-payment-with-paypal/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="db9f8-230">![Google - Create Client ID](checkout-and-payment-with-paypal/_static/image10.png)</span></span>
7. <span data-ttu-id="db9f8-231">在 [**建立用戶端識別碼**] 對話方塊中，保留預設值**Web 應用程式**應用程式類型。</span><span class="sxs-lookup"><span data-stu-id="db9f8-231">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
8. <span data-ttu-id="db9f8-232">設定**授權 JavaScript Origins**至您稍早在本教學課程中使用的 SSL URL (`https://localhost:44300/`除非您已建立 SSL 的其他專案)。</span><span class="sxs-lookup"><span data-stu-id="db9f8-232">Set the **Authorized JavaScript Origins** to the SSL URL you used earlier in this tutorial (`https://localhost:44300/` unless you've created other SSL projects).</span></span>   
   <span data-ttu-id="db9f8-233">此 URL 是您的應用程式的原點。</span><span class="sxs-lookup"><span data-stu-id="db9f8-233">This URL is the origin for your application.</span></span> <span data-ttu-id="db9f8-234">此範例中，您將僅輸入 localhost 測試 URL。</span><span class="sxs-lookup"><span data-stu-id="db9f8-234">For this sample, you will only enter the localhost test URL.</span></span> <span data-ttu-id="db9f8-235">不過，您可以輸入帳戶的 localhost 和生產環境的多個 Url。</span><span class="sxs-lookup"><span data-stu-id="db9f8-235">However, you can enter multiple URLs to account for localhost and production.</span></span>
9. <span data-ttu-id="db9f8-236">設定**Authorized Redirect URI**如下：</span><span class="sxs-lookup"><span data-stu-id="db9f8-236">Set the **Authorized Redirect URI** to the following:</span></span> 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   <span data-ttu-id="db9f8-237">這個值是 URI ASP.NET OAuth 使用者與 google OAuth 伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="db9f8-237">This value is the URI that ASP.NET OAuth users to communicate with the google OAuth server.</span></span> <span data-ttu-id="db9f8-238">請記住您在上面使用的 SSL URL (`https://localhost:44300/`除非您已建立 SSL 的其他專案)。</span><span class="sxs-lookup"><span data-stu-id="db9f8-238">Remember the SSL URL you used above (    `https://localhost:44300/` unless you've created other SSL projects).</span></span>
10. <span data-ttu-id="db9f8-239">按一下 [**建立用戶端識別碼**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="db9f8-239">Click the **Create Client ID** button.</span></span>
11. <span data-ttu-id="db9f8-240">在 Google 開發人員主控台的左側功能表中，按一下 **同意畫面**功能表項目，然後設定您的電子郵件地址和產品名稱。</span><span class="sxs-lookup"><span data-stu-id="db9f8-240">On the left menu of the Google Developers Console, click the **Consent screen** menu item, then set your email address and product name.</span></span> <span data-ttu-id="db9f8-241">完成表單後，按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-241">When you have completed the form, click **Save**.</span></span>
12. <span data-ttu-id="db9f8-242">按一下  **Api**功能表項目，向下的捲動，然後按一下  **off**旁**Google + API**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-242">Click the **APIs** menu item, scroll down and click the **off** button next to **Google+ API**.</span></span>   
    <span data-ttu-id="db9f8-243">接受此選項可讓 Google + API。</span><span class="sxs-lookup"><span data-stu-id="db9f8-243">Accepting this option will enable the Google+ API.</span></span>
13. <span data-ttu-id="db9f8-244">您也必須更新**Microsoft.Owin** 3.0.0 版的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="db9f8-244">You must also update the **Microsoft.Owin** NuGet package to version 3.0.0.</span></span>   
    <span data-ttu-id="db9f8-245">從**工具**功能表上，選取**NuGet 套件管理員**，然後選取**管理方案的 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-245">From the **Tools** menu, select **NuGet Package Manager** and then select **Manage NuGet Packages for Solution**.</span></span>  
    <span data-ttu-id="db9f8-246">從**管理 NuGet 套件** 視窗中，尋找並更新**Microsoft.Owin** 3.0.0 版的封裝。</span><span class="sxs-lookup"><span data-stu-id="db9f8-246">From the **Manage NuGet Packages** window, find and update the **Microsoft.Owin** package to version 3.0.0.</span></span>
14. <span data-ttu-id="db9f8-247">在 Visual Studio 中，更新`UseGoogleAuthentication`方法*Startup.Auth.cs*藉由複製並貼上頁面**用戶端識別碼**並**用戶端祕密**至方法。</span><span class="sxs-lookup"><span data-stu-id="db9f8-247">In Visual Studio, update the `UseGoogleAuthentication` method of the *Startup.Auth.cs* page by copying and pasting the **Client ID** and **Client Secret** into the method.</span></span> <span data-ttu-id="db9f8-248">**用戶端識別碼**並**用戶端祕密**如下所示的值是範例，將無法運作。</span><span class="sxs-lookup"><span data-stu-id="db9f8-248">The **Client ID** and **Client Secret** values shown below are samples and will not work.</span></span> 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. <span data-ttu-id="db9f8-249">按下**CTRL + F5**以建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="db9f8-249">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="db9f8-250">按一下 **登入**連結。</span><span class="sxs-lookup"><span data-stu-id="db9f8-250">Click the **Log in** link.</span></span>
16. <span data-ttu-id="db9f8-251">底下**另一個服務用來登入**，按一下**Google**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-251">Under **Use another service to log in**, click **Google**.</span></span>  
    <span data-ttu-id="db9f8-252">![登入](checkout-and-payment-with-paypal/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="db9f8-252">![Log in](checkout-and-payment-with-paypal/_static/image11.png)</span></span>
17. <span data-ttu-id="db9f8-253">如果您需要輸入認證時，您將會重新導向至 google 網站，您會在此輸入您的認證。</span><span class="sxs-lookup"><span data-stu-id="db9f8-253">If you need to enter your credentials, you will be redirected to the google site where you will enter your credentials.</span></span>  
    ![Google-登入](checkout-and-payment-with-paypal/_static/image12.png)
18. <span data-ttu-id="db9f8-255">您輸入認證之後，系統會提示您授與您剛才建立的 web 應用程式的權限。</span><span class="sxs-lookup"><span data-stu-id="db9f8-255">After you enter your credentials, you will be prompted to give permissions to the web application you just created.</span></span>  
    ![專案預設服務帳戶](checkout-and-payment-with-paypal/_static/image13.png)
19. <span data-ttu-id="db9f8-257">按一下 **接受**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-257">Click **Accept**.</span></span> <span data-ttu-id="db9f8-258">您現在會重新導向回到**註冊**頁**WingtipToys**應用程式，您可以在此註冊您的 Google 帳戶。</span><span class="sxs-lookup"><span data-stu-id="db9f8-258">You will now be redirected back to the **Register** page of the **WingtipToys** application where you can register your Google account.</span></span>  
    <span data-ttu-id="db9f8-259">![使用您的 Google 帳戶註冊](checkout-and-payment-with-paypal/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="db9f8-259">![Register with your Google Account](checkout-and-payment-with-paypal/_static/image14.png)</span></span>
20. <span data-ttu-id="db9f8-260">您可以選擇變更用於 Gmail 帳戶、 本機電子郵件註冊名稱，但您通常想要保留預設電子郵件別名 （也就是一個您用來驗證）。</span><span class="sxs-lookup"><span data-stu-id="db9f8-260">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="db9f8-261">按一下 **登入**如上所示。</span><span class="sxs-lookup"><span data-stu-id="db9f8-261">Click **Log in** as shown above.</span></span>

### <a name="modifying-login-functionality"></a><span data-ttu-id="db9f8-262">修改登入功能</span><span class="sxs-lookup"><span data-stu-id="db9f8-262">Modifying Login Functionality</span></span>

<span data-ttu-id="db9f8-263">如先前所述在本教學課程系列中，大部分的使用者註冊功能已包含在 ASP.NET Web Forms 範本預設。</span><span class="sxs-lookup"><span data-stu-id="db9f8-263">As previously mentioned in this tutorial series, much of the user registration functionality has been included in the ASP.NET Web Forms template by default.</span></span> <span data-ttu-id="db9f8-264">現在您將修改預設值*Login.aspx*並*Register.aspx*頁面來呼叫`MigrateCart`方法。</span><span class="sxs-lookup"><span data-stu-id="db9f8-264">Now you will modify the default *Login.aspx* and *Register.aspx* pages to call the `MigrateCart` method.</span></span> <span data-ttu-id="db9f8-265">`MigrateCart`方法關聯匿名的購物車中的新登入的使用者。</span><span class="sxs-lookup"><span data-stu-id="db9f8-265">The `MigrateCart` method associates a newly logged in user with an anonymous shopping cart.</span></span> <span data-ttu-id="db9f8-266">藉由建立關聯的使用者和購物車，Wingtip Toys 範例應用程式將能夠維護購物車，每次造訪的使用者。</span><span class="sxs-lookup"><span data-stu-id="db9f8-266">By associating the user and shopping cart, the Wingtip Toys sample application will be able to maintain the shopping cart of the user between visits.</span></span>

1. <span data-ttu-id="db9f8-267">在 **方案總管**，尋找並開啟*帳戶*資料夾。</span><span class="sxs-lookup"><span data-stu-id="db9f8-267">In **Solution Explorer**, find and open the *Account* folder.</span></span>
2. <span data-ttu-id="db9f8-268">修改程式碼後置頁面，名為*Login.aspx.cs*加入以黃色反白顯示的程式碼，使它看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="db9f8-268">Modify the code-behind page named *Login.aspx.cs* to include the code highlighted in yellow, so that it appears as follows:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. <span data-ttu-id="db9f8-269">儲存*Login.aspx.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="db9f8-269">Save the *Login.aspx.cs* file.</span></span>

<span data-ttu-id="db9f8-270">現在，您可以忽略沒有任何定義的警告`MigrateCart`方法。</span><span class="sxs-lookup"><span data-stu-id="db9f8-270">For now, you can ignore the warning that there is no definition for the `MigrateCart` method.</span></span> <span data-ttu-id="db9f8-271">您將新增它稍後在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="db9f8-271">You will be adding it a bit later in this tutorial.</span></span>

<span data-ttu-id="db9f8-272">*Login.aspx.cs*支援登入方法的程式碼後置檔案。</span><span class="sxs-lookup"><span data-stu-id="db9f8-272">The *Login.aspx.cs* code-behind file supports a LogIn method.</span></span> <span data-ttu-id="db9f8-273">藉由檢查的 Login.aspx 頁面，您會看到此頁面包含的 「 登入 」 按鈕，當按一下 觸發程序`LogIn`上程式碼後置處理常式。</span><span class="sxs-lookup"><span data-stu-id="db9f8-273">By inspecting the Login.aspx page, you'll see that this page includes a "Log in" button that when click triggers the `LogIn` handler on the code-behind.</span></span>

<span data-ttu-id="db9f8-274">當`Login`方法*Login.aspx.cs*會呼叫名為購物籃中的新執行個體`usersShoppingCart`建立。</span><span class="sxs-lookup"><span data-stu-id="db9f8-274">When the `Login` method on the *Login.aspx.cs* is called, a new instance of the shopping cart named `usersShoppingCart` is created.</span></span> <span data-ttu-id="db9f8-275">擷取的購物車 (GUID) 識別碼並將其設為`cartId`變數。</span><span class="sxs-lookup"><span data-stu-id="db9f8-275">The ID of the shopping cart (a GUID) is retrieved and set to the `cartId` variable.</span></span> <span data-ttu-id="db9f8-276">然後，`MigrateCart`呼叫方法時，傳遞兩者`cartId`和這個方法登入的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="db9f8-276">Then, the `MigrateCart` method is called, passing both the `cartId` and the name of the logged-in user to this method.</span></span> <span data-ttu-id="db9f8-277">購物車移轉時，用來識別匿名購物車的 GUID 會取代使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="db9f8-277">When the shopping cart is migrated, the GUID used to identify the anonymous shopping cart is replaced with the user name.</span></span>

<span data-ttu-id="db9f8-278">除了修改*Login.aspx.cs*程式碼後置檔案，以移轉購物車，當使用者登入，您也必須修改*Register.aspx.cs 程式碼後置檔案*移轉購物車當使用者建立新的帳戶及登入。</span><span class="sxs-lookup"><span data-stu-id="db9f8-278">In addition to modifying the *Login.aspx.cs* code-behind file to migrate the shopping cart when the user logs in, you must also modify the *Register.aspx.cs code-behind file* to migrate the shopping cart when the user creates a new account and logs in.</span></span>

1. <span data-ttu-id="db9f8-279">在 *帳號*資料夾中，開啟的程式碼後置檔案命名為*Register.aspx.cs*。</span><span class="sxs-lookup"><span data-stu-id="db9f8-279">In the *Account* folder, open the code-behind file named *Register.aspx.cs*.</span></span>
2. <span data-ttu-id="db9f8-280">修改程式碼後置檔案包含下列程式碼以黃色，使其出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="db9f8-280">Modify the code-behind file by including the code in yellow, so that it appears as follows:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. <span data-ttu-id="db9f8-281">儲存*Register.aspx.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="db9f8-281">Save the *Register.aspx.cs* file.</span></span> <span data-ttu-id="db9f8-282">同樣地，關於忽略警告`MigrateCart`方法。</span><span class="sxs-lookup"><span data-stu-id="db9f8-282">Once again, ignore the warning about the `MigrateCart` method.</span></span>

<span data-ttu-id="db9f8-283">請注意程式碼中所使用`CreateUser_Click`事件處理常式是非常類似於您在中使用的程式碼`LogIn`方法。</span><span class="sxs-lookup"><span data-stu-id="db9f8-283">Notice that the code you used in the `CreateUser_Click` event handler is very similar to the code you used in the `LogIn` method.</span></span> <span data-ttu-id="db9f8-284">當使用者註冊或登入站台，而呼叫`MigrateCart`方法會進行。</span><span class="sxs-lookup"><span data-stu-id="db9f8-284">When the user registers or logs in to the site, a call to the `MigrateCart` method will be made.</span></span>

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="db9f8-285">移轉購物車</span><span class="sxs-lookup"><span data-stu-id="db9f8-285">Migrating the Shopping Cart</span></span>

<span data-ttu-id="db9f8-286">已更新的登入和註冊程序之後，您可以加入購物車 」 使用移轉的程式碼`MigrateCart`方法。</span><span class="sxs-lookup"><span data-stu-id="db9f8-286">Now that you have the log-in and registration process updated, you can add the code to migrate the shopping cart using the `MigrateCart` method.</span></span>

1. <span data-ttu-id="db9f8-287">在 **方案總管**，尋找*邏輯*資料夾，然後開啟*ShoppingCartActions.cs*類別檔案。</span><span class="sxs-lookup"><span data-stu-id="db9f8-287">In **Solution Explorer**, find the *Logic* folder and open the *ShoppingCartActions.cs* class file.</span></span>
2. <span data-ttu-id="db9f8-288">加入現有的程式碼中的黃色反白顯示的程式碼*ShoppingCartActions.cs*檔案，以便中的程式碼*ShoppingCartActions.cs*檔案看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="db9f8-288">Add the code highlighted in yellow to the existing code in the *ShoppingCartActions.cs* file, so that the code in the *ShoppingCartActions.cs* file appears as follows:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

<span data-ttu-id="db9f8-289">`MigrateCart`方法會使用現有 cartId 來尋找使用者的購物車。</span><span class="sxs-lookup"><span data-stu-id="db9f8-289">The `MigrateCart` method uses the existing cartId to find the shopping cart of the user.</span></span> <span data-ttu-id="db9f8-290">接下來，所有的購物車項目執行迴圈的程式碼，並取代`CartId`屬性 (依照`CartItem`結構描述) 的登入的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="db9f8-290">Next, the code loops through all the shopping cart items and replaces the `CartId` property (as specified by the `CartItem` schema) with the logged-in user name.</span></span>

### <a name="updating-the-database-connection"></a><span data-ttu-id="db9f8-291">正在更新資料庫連接</span><span class="sxs-lookup"><span data-stu-id="db9f8-291">Updating the Database Connection</span></span>

<span data-ttu-id="db9f8-292">如果您要遵循此教學課程使用**預先建置**Wingtip Toys 範例應用程式，您必須重新建立的預設成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="db9f8-292">If you are following this tutorial using the **prebuilt** Wingtip Toys sample application, you must recreate the default membership database.</span></span> <span data-ttu-id="db9f8-293">藉由修改預設的連接字串，成員資格資料庫將會建立下一次執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="db9f8-293">By modifying the default connection string, the membership database will be created the next time the application runs.</span></span>

1. <span data-ttu-id="db9f8-294">開啟*Web.config*專案根目錄的檔案。</span><span class="sxs-lookup"><span data-stu-id="db9f8-294">Open the *Web.config* file at the root of the project.</span></span>
2. <span data-ttu-id="db9f8-295">更新預設的連接字串，使它看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="db9f8-295">Update the default connection string so that it appears as follows:</span></span>   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a><span data-ttu-id="db9f8-296">整合 PayPal</span><span class="sxs-lookup"><span data-stu-id="db9f8-296">Integrating PayPal</span></span>

<span data-ttu-id="db9f8-297">PayPal 是網頁型計費平台可接受的線上商家的付款。</span><span class="sxs-lookup"><span data-stu-id="db9f8-297">PayPal is a web-based billing platform that accepts payments by online merchants.</span></span> <span data-ttu-id="db9f8-298">本教學課程接下來會說明如何將 PayPal 的 Express 簽出功能整合到您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="db9f8-298">This tutorial next explains how to integrate PayPal's Express Checkout functionality into your application.</span></span> <span data-ttu-id="db9f8-299">快速簽出可讓您的客戶使用 PayPal 支付它們新增至購物車的項目。</span><span class="sxs-lookup"><span data-stu-id="db9f8-299">Express Checkout allows your customers to use PayPal to pay for the items they have added to their shopping cart.</span></span>

### <a name="create-paylpal-test-accounts"></a><span data-ttu-id="db9f8-300">建立 PaylPal 測試帳戶</span><span class="sxs-lookup"><span data-stu-id="db9f8-300">Create PaylPal Test Accounts</span></span>

<span data-ttu-id="db9f8-301">若要使用 PayPal 測試環境，您必須建立並驗證開發人員測試帳戶。</span><span class="sxs-lookup"><span data-stu-id="db9f8-301">To use the PayPal testing environment, you must create and verify a developer test account.</span></span> <span data-ttu-id="db9f8-302">若要建立買方測試帳戶和賣方測試帳戶，您將使用開發人員測試帳戶。</span><span class="sxs-lookup"><span data-stu-id="db9f8-302">You will use the developer test account to create a buyer test account and a seller test account.</span></span> <span data-ttu-id="db9f8-303">開發人員測試帳戶認證也可讓 Wingtip Toys 範例應用程式存取 PayPal 測試環境。</span><span class="sxs-lookup"><span data-stu-id="db9f8-303">The developer test account credentials also will allow the Wingtip Toys sample application to access the PayPal testing environment.</span></span>

1. <span data-ttu-id="db9f8-304">在瀏覽器中，瀏覽至 PayPal 的開發人員測試站台：</span><span class="sxs-lookup"><span data-stu-id="db9f8-304">In a browser, navigate to the PayPal developer testing site:</span></span>   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. <span data-ttu-id="db9f8-305">如果您沒有 PayPal 開發人員帳戶，建立新的帳戶，即可**註冊**並遵循註冊步驟。</span><span class="sxs-lookup"><span data-stu-id="db9f8-305">If you don't have a PayPal developer account, create a new account by clicking **Sign Up**and following the sign up steps.</span></span> <span data-ttu-id="db9f8-306">如果您有現有的 PayPal 開發人員帳戶，登入，即可**登入**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-306">If you have an existing PayPal developer account, sign in by clicking **Log In**.</span></span> <span data-ttu-id="db9f8-307">您將需要 PayPal 開發人員帳戶來測試本教學課程稍後的 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="db9f8-307">You will need your PayPal developer account to test the Wingtip Toys sample application later in this tutorial.</span></span>
3. <span data-ttu-id="db9f8-308">如果您只是已註冊 PayPal 開發人員帳戶，您可能需要驗證您使用 PayPal 的 PayPal 開發人員帳戶。</span><span class="sxs-lookup"><span data-stu-id="db9f8-308">If you have just signed up for your PayPal developer account, you may need to verify your PayPal developer account with PayPal.</span></span> <span data-ttu-id="db9f8-309">您可以遵循 PayPal 傳送電子郵件帳戶的步驟，以確認您的帳戶。</span><span class="sxs-lookup"><span data-stu-id="db9f8-309">You can verify your account by following the steps that PayPal sent to your email account.</span></span> <span data-ttu-id="db9f8-310">一旦確認您 PayPal 的開發人員帳戶，登入 PayPal 的開發人員測試站台。</span><span class="sxs-lookup"><span data-stu-id="db9f8-310">Once you have verified your PayPal developer account, log back into the PayPal developer testing site.</span></span>
4. <span data-ttu-id="db9f8-311">之後您登入 PayPal 開發人員網站與您 PayPal 開發人員帳戶，您需要建立 PayPal 買方測試帳戶，如果您不知道已經有一個。</span><span class="sxs-lookup"><span data-stu-id="db9f8-311">After you are logged in to the PayPal developer site with your PayPal developer account you need to create a PayPal buyer test account if you don't already have one.</span></span> <span data-ttu-id="db9f8-312">若要在 PayPal 網站，按一下 建立購買者測試帳戶，**應用程式**索引標籤，然後按一下**沙箱帳戶**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-312">To create a buyer test account, on the PayPal site click the **Applications** tab and then click **Sandbox accounts**.</span></span>   
 <span data-ttu-id="db9f8-313">**沙箱測試帳戶**頁面會顯示。</span><span class="sxs-lookup"><span data-stu-id="db9f8-313">The **Sandbox test accounts** page is shown.</span></span>   

    > [!NOTE] 
    > 
    > <span data-ttu-id="db9f8-314">PayPal 開發人員網站已提供商家的測試帳戶。</span><span class="sxs-lookup"><span data-stu-id="db9f8-314">The PayPal Developer site already provides a merchant test account.</span></span>

    ![簽出與使用 PayPal-沙箱測試帳戶的付款](checkout-and-payment-with-paypal/_static/image15.png)
5. <span data-ttu-id="db9f8-316">在沙箱測試的 [帳戶] 頁面中，按一下**建立帳戶**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-316">On the Sandbox test accounts page, click **Create Account**.</span></span>
6. <span data-ttu-id="db9f8-317">在 **建立測試帳戶**頁面上選擇買方測試帳戶的電子郵件和您選擇的密碼。</span><span class="sxs-lookup"><span data-stu-id="db9f8-317">On the **Create test account** page choose a buyer test account email and password of your choice.</span></span>   

    > [!NOTE] 
    > 
    > <span data-ttu-id="db9f8-318">您必須購買者電子郵件地址和密碼才能測試 Wingtip Toys 範例應用程式，在本教學課程結尾處。</span><span class="sxs-lookup"><span data-stu-id="db9f8-318">You will need the buyer email addresses and password to test the Wingtip Toys sample application at the end of this tutorial.</span></span>

    ![簽出與使用 PayPal-沙箱測試帳戶的付款](checkout-and-payment-with-paypal/_static/image16.png)
7. <span data-ttu-id="db9f8-320">建立買方測試帳戶，方法是按一下**建立帳戶** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="db9f8-320">Create the buyer test account by clicking the **Create Account** button.</span></span>  
 <span data-ttu-id="db9f8-321">**沙箱測試帳戶**頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="db9f8-321">The **Sandbox Test accounts** page is displayed.</span></span> 

    ![簽出與使用 PayPal-PaylPal 帳戶的付款](checkout-and-payment-with-paypal/_static/image17.png)
8. <span data-ttu-id="db9f8-323">在 **沙箱測試帳戶**頁面上，按一下**促進**電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="db9f8-323">On the **Sandbox test accounts** page, click the **facilitator** email account.</span></span>  
    <span data-ttu-id="db9f8-324">**設定檔**並**通知**選項會出現。</span><span class="sxs-lookup"><span data-stu-id="db9f8-324">**Profile** and **Notification** options appear.</span></span>
9. <span data-ttu-id="db9f8-325">選取 **設定檔**選項，然後按一下**API 認證**若要檢視您的 API 認證商家的測試帳戶。</span><span class="sxs-lookup"><span data-stu-id="db9f8-325">Select the **Profile** option, then click **API credentials** to view your API credentials for the merchant test account.</span></span>
10. <span data-ttu-id="db9f8-326">將測試 API 認證複製到 [記事本]。</span><span class="sxs-lookup"><span data-stu-id="db9f8-326">Copy the TEST API credentials to notepad.</span></span>

<span data-ttu-id="db9f8-327">您必須顯示傳統測試 API 認證 （使用者名稱、 密碼和簽章） 進行 API 呼叫，Wingtip Toys 範例應用程式從測試環境的 PayPal。</span><span class="sxs-lookup"><span data-stu-id="db9f8-327">You will need your displayed Classic TEST API credentials (Username, Password, and Signature) to make API calls from the Wingtip Toys sample application to the PayPal testing environment.</span></span> <span data-ttu-id="db9f8-328">您將在下一個步驟中新增認證。</span><span class="sxs-lookup"><span data-stu-id="db9f8-328">You will add the credentials in the next step.</span></span>

### <a name="add-paypal-class-and-api-credentials"></a><span data-ttu-id="db9f8-329">新增 PayPal 類別和 API 認證</span><span class="sxs-lookup"><span data-stu-id="db9f8-329">Add PayPal Class and API Credentials</span></span>

<span data-ttu-id="db9f8-330">您會將大部分的 PayPal 程式碼，為單一類別。</span><span class="sxs-lookup"><span data-stu-id="db9f8-330">You will place the majority of the PayPal code into a single class.</span></span> <span data-ttu-id="db9f8-331">這個類別包含用來使用 PayPal 進行通訊的方法。</span><span class="sxs-lookup"><span data-stu-id="db9f8-331">This class contains the methods used to communicate with PayPal.</span></span> <span data-ttu-id="db9f8-332">此外，您會將您 PayPal 的認證新增至此類別。</span><span class="sxs-lookup"><span data-stu-id="db9f8-332">Also, you will add your PayPal credentials to this class.</span></span>

1. <span data-ttu-id="db9f8-333">在 Visual Studio 內 Wingtip Toys 範例應用程式，以滑鼠右鍵按一下**邏輯**資料夾，然後選取**新增** - &gt; **新項目**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-333">In the Wingtip Toys sample application within Visual Studio, right-click the **Logic** folder and then select **Add** -&gt; **New Item**.</span></span>   
   <span data-ttu-id="db9f8-334">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="db9f8-334">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="db9f8-335">底下**Visual C#** 從**已安裝**左邊的窗格，選取**程式碼**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-335">Under **Visual C#** from the **Installed** pane on the left, select **Code**.</span></span>
3. <span data-ttu-id="db9f8-336">從中間窗格中，選取**類別**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-336">From the middle pane, select **Class**.</span></span> <span data-ttu-id="db9f8-337">這個新類別命名**PayPalFunctions.cs**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-337">Name this new class **PayPalFunctions.cs**.</span></span>
4. <span data-ttu-id="db9f8-338">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="db9f8-338">Click **Add**.</span></span>  
   <span data-ttu-id="db9f8-339">新的類別檔案會顯示在編輯器中。</span><span class="sxs-lookup"><span data-stu-id="db9f8-339">The new class file is displayed in the editor.</span></span>
5. <span data-ttu-id="db9f8-340">取代為下列程式碼中的預設程式碼：</span><span class="sxs-lookup"><span data-stu-id="db9f8-340">Replace the default code with the following code:</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. <span data-ttu-id="db9f8-341">新增您稍早在本教學課程中，讓您可以函式呼叫 PayPal 測試環境顯示 Merchant API 認證 （使用者名稱、 密碼和簽章）。</span><span class="sxs-lookup"><span data-stu-id="db9f8-341">Add the Merchant API credentials (Username, Password, and Signature) that you displayed earlier in this tutorial so that you can make function calls to the PayPal testing environment.</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="db9f8-342">在此範例應用程式只會將認證加入至 C# 檔案 (.cs)。</span><span class="sxs-lookup"><span data-stu-id="db9f8-342">In this sample application you are simply adding credentials to a C# file (.cs).</span></span> <span data-ttu-id="db9f8-343">不過，在實作的解決方案中，您應該考慮加密您在組態檔中的認證。</span><span class="sxs-lookup"><span data-stu-id="db9f8-343">However, in a implemented solution, you should consider encrypting your credentials in a configuration file.</span></span>


<span data-ttu-id="db9f8-344">NVPAPICaller 類別包含大部分的 PayPal 功能。</span><span class="sxs-lookup"><span data-stu-id="db9f8-344">The NVPAPICaller class contains the majority of the PayPal functionality.</span></span> <span data-ttu-id="db9f8-345">在類別中的程式碼提供購買從 PayPal 測試環境的測試所需的方法。</span><span class="sxs-lookup"><span data-stu-id="db9f8-345">The code in the class provides the methods needed to make a test purchase from the PayPal testing environment.</span></span> <span data-ttu-id="db9f8-346">下列三個 PayPal 函式用來進行購買：</span><span class="sxs-lookup"><span data-stu-id="db9f8-346">The following three PayPal functions are used to make purchases:</span></span>

- <span data-ttu-id="db9f8-347">`SetExpressCheckout` 函式</span><span class="sxs-lookup"><span data-stu-id="db9f8-347">`SetExpressCheckout` function</span></span>
- <span data-ttu-id="db9f8-348">`GetExpressCheckoutDetails` 函式</span><span class="sxs-lookup"><span data-stu-id="db9f8-348">`GetExpressCheckoutDetails` function</span></span>
- <span data-ttu-id="db9f8-349">`DoExpressCheckoutPayment` 函式</span><span class="sxs-lookup"><span data-stu-id="db9f8-349">`DoExpressCheckoutPayment` function</span></span>

<span data-ttu-id="db9f8-350">`ShortcutExpressCheckout`方法會收集測試採購資訊與產品詳細資料，從購物車，然後呼叫`SetExpressCheckout`PayPal 函式。</span><span class="sxs-lookup"><span data-stu-id="db9f8-350">The `ShortcutExpressCheckout` method collects the test purchase information and product details from the shopping cart and calls the `SetExpressCheckout` PayPal function.</span></span> <span data-ttu-id="db9f8-351">`GetCheckoutDetails`方法確認採購詳細資料，並呼叫`GetExpressCheckoutDetails`PayPal 函式之前先測試購買。</span><span class="sxs-lookup"><span data-stu-id="db9f8-351">The `GetCheckoutDetails` method confirms purchase details and calls the `GetExpressCheckoutDetails` PayPal function before making the test purchase.</span></span> <span data-ttu-id="db9f8-352">`DoCheckoutPayment`方法呼叫完成從測試環境的測試購買`DoExpressCheckoutPayment`PayPal 函式。</span><span class="sxs-lookup"><span data-stu-id="db9f8-352">The `DoCheckoutPayment` method completes the test purchase from the testing environment by calling the `DoExpressCheckoutPayment` PayPal function.</span></span> <span data-ttu-id="db9f8-353">其餘的程式碼支援的 PayPal 方法和處理程序，例如字串的編碼方式、 解碼字串、 處理陣列，以及決定認證。</span><span class="sxs-lookup"><span data-stu-id="db9f8-353">The remaining code supports the PayPal methods and process, such as encoding strings, decoding strings, processing arrays, and determining credentials.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="db9f8-354">PayPal 可讓您包含選擇性的購買詳細資料，根據[PayPal 的 API 規格](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout)。</span><span class="sxs-lookup"><span data-stu-id="db9f8-354">PayPal allows you to include optional purchase details based on [PayPal's API specification](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout).</span></span> <span data-ttu-id="db9f8-355">您可以藉由擴充 Wingtip Toys 範例應用程式中的程式碼，包含當地語系化詳細資料、 產品描述、 稅務、 客戶服務編號，以及許多其他選擇性欄位。</span><span class="sxs-lookup"><span data-stu-id="db9f8-355">By extending the code in the Wingtip Toys sample application, you can include localization details, product descriptions, tax, a customer service number, as well as many other optional fields.</span></span>


<span data-ttu-id="db9f8-356">請注意，傳回與 [取消] Url 中指定**ShortcutExpressCheckout**方法使用的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="db9f8-356">Notice that the return and cancel URLs that are specified in the **ShortcutExpressCheckout** method use a port number.</span></span>

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

<span data-ttu-id="db9f8-357">Visual Web Developer 中執行時使用 SSL 的 web 專案，通常使用連接埠 44300 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="db9f8-357">When Visual Web Developer runs a web project using SSL, commonly the port 44300 is used for the web server.</span></span> <span data-ttu-id="db9f8-358">如上所示，連接埠號碼是 44300。</span><span class="sxs-lookup"><span data-stu-id="db9f8-358">As shown above, the port number is 44300.</span></span> <span data-ttu-id="db9f8-359">當您執行應用程式時，您可以看到不同的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="db9f8-359">When you run the application, you could see a different port number.</span></span> <span data-ttu-id="db9f8-360">連接埠號碼必須正確設定程式碼中，您才能成功執行 Wingtip Toys 範例應用程式在本教學課程結尾處。</span><span class="sxs-lookup"><span data-stu-id="db9f8-360">Your port number needs to be correctly set in the code so that you can successful run the Wingtip Toys sample application at the end of this tutorial.</span></span> <span data-ttu-id="db9f8-361">本教學課程的下一節會說明如何擷取本機主機通訊埠編號和 PayPal 類別的更新。</span><span class="sxs-lookup"><span data-stu-id="db9f8-361">The next section of this tutorial explains how to retrieve the local host port number and update the PayPal class.</span></span>

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a><span data-ttu-id="db9f8-362">更新 PayPal 類別中的 LocalHost 連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="db9f8-362">Update the LocalHost Port Number in the PayPal Class</span></span>

<span data-ttu-id="db9f8-363">Wingtip Toys 範例應用程式會購買產品，瀏覽到 PayPal 測試站台，並將傳回給 Wingtip Toys 範例應用程式的本機執行個體。</span><span class="sxs-lookup"><span data-stu-id="db9f8-363">The Wingtip Toys sample application purchases products by navigating to the PayPal testing site and returning to your local instance of the Wingtip Toys sample application.</span></span> <span data-ttu-id="db9f8-364">若要有 PayPal 傳回至正確的 URL，您必須指定連接埠號碼，在本機執行的應用程式中先前所述的 PayPal 程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="db9f8-364">In order to have PayPal return to the correct URL, you need to specify the port number of the locally running sample application in the PayPal code mentioned above.</span></span>

1. <span data-ttu-id="db9f8-365">以滑鼠右鍵按一下專案名稱 (**WingtipToys**) 中**方案總管**，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-365">Right-click the project name (**WingtipToys**) in **Solution Explorer** and select **Properties**.</span></span>
2. <span data-ttu-id="db9f8-366">在左欄中，選取**Web**  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="db9f8-366">In the left column, select the **Web** tab.</span></span>
3. <span data-ttu-id="db9f8-367">擷取連接埠號碼**專案 Url**  方塊中。</span><span class="sxs-lookup"><span data-stu-id="db9f8-367">Retrieve the port number from the **Project Url** box.</span></span>
4. <span data-ttu-id="db9f8-368">如有需要更新`returnURL`並`cancelURL`PayPal 類別中 (`NVPAPICaller`) 中*PayPalFunctions.cs*檔案，以使用您的 web 應用程式的連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="db9f8-368">If needed, update the `returnURL` and `cancelURL` in the PayPal class (`NVPAPICaller`) in the *PayPalFunctions.cs* file to use the port number of your web application:</span></span>   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

<span data-ttu-id="db9f8-369">現在您加入的程式碼會比對您的本機 Web 應用程式的預期連接埠。</span><span class="sxs-lookup"><span data-stu-id="db9f8-369">Now the code that you added will match the expected port for your local Web application.</span></span> <span data-ttu-id="db9f8-370">PayPal 能夠回到本機電腦上的正確的 URL。</span><span class="sxs-lookup"><span data-stu-id="db9f8-370">PayPal will be able to return to the correct URL on your local machine.</span></span>

### <a name="add-the-paypal-checkout-button"></a><span data-ttu-id="db9f8-371">新增 [PayPal 簽出] 按鈕</span><span class="sxs-lookup"><span data-stu-id="db9f8-371">Add the PayPal Checkout Button</span></span>

<span data-ttu-id="db9f8-372">現在，主要的 PayPal 函式已新增至範例應用程式，您可以開始加入標記和呼叫這些函式所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="db9f8-372">Now that the primary PayPal functions have been added to the sample application, you can begin adding the markup and code needed to call these functions.</span></span> <span data-ttu-id="db9f8-373">首先，您必須新增使用者會看到在購物車 頁面的 簽出 按鈕。</span><span class="sxs-lookup"><span data-stu-id="db9f8-373">First, you must add the checkout button that the user will see on the shopping cart page.</span></span>

1. <span data-ttu-id="db9f8-374">開啟*ShoppingCart.aspx*檔案。</span><span class="sxs-lookup"><span data-stu-id="db9f8-374">Open the *ShoppingCart.aspx* file.</span></span>
2. <span data-ttu-id="db9f8-375">捲動到檔案底部，然後尋找`<!--Checkout Placeholder -->`註解。</span><span class="sxs-lookup"><span data-stu-id="db9f8-375">Scroll to the bottom of the file and find the `<!--Checkout Placeholder -->` comment.</span></span>
3. <span data-ttu-id="db9f8-376">使用註解取代`ImageButton`控制，讓標記取代，如下所示：</span><span class="sxs-lookup"><span data-stu-id="db9f8-376">Replace the comment with an `ImageButton` control so that the mark up is replaced as follows:</span></span>  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. <span data-ttu-id="db9f8-377">在  *ShoppingCart.aspx.cs*檔案，之後`UpdateBtn_Click`檔案，結尾附近的事件處理常式加入`CheckOutBtn_Click`事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="db9f8-377">In the *ShoppingCart.aspx.cs* file, after the `UpdateBtn_Click` event handler near the end of the file, add the `CheckOutBtn_Click` event handler:</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. <span data-ttu-id="db9f8-378">此外，在*ShoppingCart.aspx.cs*檔案中，將參考加入`CheckoutBtn`，如此一來，新的影像按鈕參考，如下所示：</span><span class="sxs-lookup"><span data-stu-id="db9f8-378">Also in the *ShoppingCart.aspx.cs* file, add a reference to the `CheckoutBtn`, so that the new image button is referenced as follows:</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. <span data-ttu-id="db9f8-379">將變更儲存至兩者*ShoppingCart.aspx*檔案並*ShoppingCart.aspx.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="db9f8-379">Save your changes to both the *ShoppingCart.aspx* file and the *ShoppingCart.aspx.cs* file.</span></span>
7. <span data-ttu-id="db9f8-380">從功能表中，選取**偵錯**-&gt;**建置 WingtipToys**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-380">From the menu, select **Debug**-&gt;**Build WingtipToys**.</span></span>  
   <span data-ttu-id="db9f8-381">將會重建專案以加入新**ImageButton**控制項。</span><span class="sxs-lookup"><span data-stu-id="db9f8-381">The project will be rebuilt with the newly added **ImageButton** control.</span></span>

### <a name="send-purchase-details-to-paypal"></a><span data-ttu-id="db9f8-382">採購單詳細資料傳送至 PayPal</span><span class="sxs-lookup"><span data-stu-id="db9f8-382">Send Purchase Details to PayPal</span></span>

<span data-ttu-id="db9f8-383">當使用者按一下**簽出**購物車 頁面上的按鈕 (*ShoppingCart.aspx*)，它們會開始在購買程序。</span><span class="sxs-lookup"><span data-stu-id="db9f8-383">When the user clicks the **Checkout** button on the shopping cart page (*ShoppingCart.aspx*), they'll begin the purchase process.</span></span> <span data-ttu-id="db9f8-384">下列程式碼會呼叫第一個所需購買產品的 PayPal 函式。</span><span class="sxs-lookup"><span data-stu-id="db9f8-384">The following code calls the first PayPal function needed to purchase products.</span></span>

1. <span data-ttu-id="db9f8-385">從*簽出*資料夾中，開啟的程式碼後置檔案命名為*CheckoutStart.aspx.cs*。</span><span class="sxs-lookup"><span data-stu-id="db9f8-385">From the *Checkout* folder, open the code-behind file named *CheckoutStart.aspx.cs*.</span></span>   
   <span data-ttu-id="db9f8-386">請務必開啟程式碼後置檔案。</span><span class="sxs-lookup"><span data-stu-id="db9f8-386">Be sure to open the code-behind file.</span></span>
2. <span data-ttu-id="db9f8-387">將現有的程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="db9f8-387">Replace the existing code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

<span data-ttu-id="db9f8-388">當應用程式的使用者按一下**簽出**在購物車頁面上，瀏覽器 按鈕會瀏覽至*CheckoutStart.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="db9f8-388">When the user of the application clicks the **Checkout** button on the shopping cart page, the browser will navigate to the *CheckoutStart.aspx* page.</span></span> <span data-ttu-id="db9f8-389">當*CheckoutStart.aspx*頁面會隨即載入，`ShortcutExpressCheckout`呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="db9f8-389">When the *CheckoutStart.aspx* page loads, the `ShortcutExpressCheckout` method is called.</span></span> <span data-ttu-id="db9f8-390">此時，使用者會轉移至 PayPal 測試的網站。</span><span class="sxs-lookup"><span data-stu-id="db9f8-390">At this point, the user is transferred to the PayPal testing web site.</span></span> <span data-ttu-id="db9f8-391">PayPal 在網站上，使用者會輸入他們的 PayPal 認證、 檢閱購買詳細資料、 接受 PayPal 合約和 「 Wingtip Toys 範例應用程式會傳回其中`ShortcutExpressCheckout`方法完成。</span><span class="sxs-lookup"><span data-stu-id="db9f8-391">On the PayPal site, the user enters their PayPal credentials, reviews the purchase details, accepts the PayPal agreement and returns to the Wingtip Toys sample application where the `ShortcutExpressCheckout` method completes.</span></span> <span data-ttu-id="db9f8-392">當`ShortcutExpressCheckout`方法完成時，它會將使用者重新導向*CheckoutReview.aspx*頁面中指定`ShortcutExpressCheckout`方法。</span><span class="sxs-lookup"><span data-stu-id="db9f8-392">When the `ShortcutExpressCheckout` method is complete, it will redirect the user to the *CheckoutReview.aspx* page specified in the `ShortcutExpressCheckout` method.</span></span> <span data-ttu-id="db9f8-393">這可讓使用者檢閱 Wingtip Toys 範例應用程式中的從訂單詳細資料。</span><span class="sxs-lookup"><span data-stu-id="db9f8-393">This allows the user to review the order details from within the Wingtip Toys sample application.</span></span>

### <a name="review-order-details"></a><span data-ttu-id="db9f8-394">檢閱訂單詳細資料</span><span class="sxs-lookup"><span data-stu-id="db9f8-394">Review Order Details</span></span>

<span data-ttu-id="db9f8-395">傳回從 PayPal 之後, *CheckoutReview.aspx* Wingtip Toys 範例應用程式的頁面會顯示訂單詳細資料。</span><span class="sxs-lookup"><span data-stu-id="db9f8-395">After returning from PayPal, the *CheckoutReview.aspx* page of the Wingtip Toys sample application displays the order details.</span></span> <span data-ttu-id="db9f8-396">此頁面可讓使用者購買產品前請先檢閱訂單詳細資料。</span><span class="sxs-lookup"><span data-stu-id="db9f8-396">This page allows the user to review the order details before purchasing the products.</span></span> <span data-ttu-id="db9f8-397">*CheckoutReview.aspx*必須建立頁面，如下所示：</span><span class="sxs-lookup"><span data-stu-id="db9f8-397">The *CheckoutReview.aspx* page must be created as follows:</span></span>

1. <span data-ttu-id="db9f8-398">在 *簽出*資料夾中，開啟名為網頁*CheckoutReview.aspx*。</span><span class="sxs-lookup"><span data-stu-id="db9f8-398">In the *Checkout* folder, open the page named *CheckoutReview.aspx*.</span></span>
2. <span data-ttu-id="db9f8-399">以下列內容取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="db9f8-399">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. <span data-ttu-id="db9f8-400">開啟名為程式碼後置頁面*CheckoutReview.aspx.cs*並以下列內容取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="db9f8-400">Open the code-behind page named *CheckoutReview.aspx.cs* and replace the existing code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

<span data-ttu-id="db9f8-401">**DetailsView**控制項用來顯示已從 PayPal 傳回訂單詳細資料。</span><span class="sxs-lookup"><span data-stu-id="db9f8-401">The **DetailsView** control is used to display the order details that have been returned from PayPal.</span></span> <span data-ttu-id="db9f8-402">而且，上述程式碼會將訂單詳細資料儲存至 Wingtip Toys 資料庫做`OrderDetail`物件。</span><span class="sxs-lookup"><span data-stu-id="db9f8-402">Also, the above code saves the order details to the Wingtip Toys database as an `OrderDetail` object.</span></span> <span data-ttu-id="db9f8-403">當使用者按一下**Complete Order**  按鈕，就會重新導向至*CheckoutComplete.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="db9f8-403">When the user clicks on the **Complete Order** button, they are redirected to the *CheckoutComplete.aspx* page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="db9f8-404">**祕訣**</span><span class="sxs-lookup"><span data-stu-id="db9f8-404">**Tip**</span></span>
> 
> <span data-ttu-id="db9f8-405">在標記*CheckoutReview.aspx*頁面上，注意`<ItemStyle>`標記用來變更中項目的樣式**DetailsView**靠近頁面底部的控制項。</span><span class="sxs-lookup"><span data-stu-id="db9f8-405">In the markup of the *CheckoutReview.aspx* page, notice that the `<ItemStyle>` tag is used to change the style of the items within the **DetailsView** control near the bottom of the page.</span></span> <span data-ttu-id="db9f8-406">藉由檢視中的網頁**設計檢視**(藉由選取**設計**在 Visual Studio 左下角)，然後選取**DetailsView**控制項，然後選取**智慧標籤**(在頂端的箭號圖示右邊的控制項)，您將能夠看到**DetailsView 工作**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-406">By viewing the page in **Design View** (by selecting **Design** at the lower left corner of Visual Studio), then selecting the **DetailsView** control, and selecting the **Smart Tag** (the arrow icon at the top right of the control), you will be able to see the **DetailsView Tasks**.</span></span>
> 
> ![簽出與使用 PayPal 付款-編輯欄位](checkout-and-payment-with-paypal/_static/image18.png)
> 
> <span data-ttu-id="db9f8-408">藉由選取**編輯欄位**，則**欄位**對話方塊會隨即出現。</span><span class="sxs-lookup"><span data-stu-id="db9f8-408">By selecting **Edit Fields**, the **Fields** dialog box will appear.</span></span> <span data-ttu-id="db9f8-409">在此對話方塊中您可以輕鬆地控制的視覺屬性，例如**ItemStyle**的**DetailsView**控制項。</span><span class="sxs-lookup"><span data-stu-id="db9f8-409">In this dialog box you can easily control the visual properties, such as **ItemStyle**, of the **DetailsView** control.</span></span>
> 
> ![簽出與使用 PayPal-欄位 對話方塊的付款](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a><span data-ttu-id="db9f8-411">完成購買程序</span><span class="sxs-lookup"><span data-stu-id="db9f8-411">Complete Purchase</span></span>

<span data-ttu-id="db9f8-412">*CheckoutComplete.aspx*網頁可讓從 PayPal 的購買。</span><span class="sxs-lookup"><span data-stu-id="db9f8-412">*CheckoutComplete.aspx* page makes the purchase from PayPal.</span></span> <span data-ttu-id="db9f8-413">如先前所述，使用者必須按一下**Complete Order**按鈕時，應用程式會瀏覽至之前*CheckoutComplete.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="db9f8-413">As mentioned above, the user must click on the **Complete Order** button before the application will navigate to the *CheckoutComplete.aspx* page.</span></span>

1. <span data-ttu-id="db9f8-414">在 *簽出*資料夾中，開啟名為網頁*CheckoutComplete.aspx*。</span><span class="sxs-lookup"><span data-stu-id="db9f8-414">In the *Checkout* folder, open the page named *CheckoutComplete.aspx*.</span></span>
2. <span data-ttu-id="db9f8-415">以下列內容取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="db9f8-415">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. <span data-ttu-id="db9f8-416">開啟名為程式碼後置頁面*CheckoutComplete.aspx.cs*並以下列內容取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="db9f8-416">Open the code-behind page named *CheckoutComplete.aspx.cs* and replace the existing code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

<span data-ttu-id="db9f8-417">當*CheckoutComplete.aspx*載入頁面時，`DoCheckoutPayment`呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="db9f8-417">When the *CheckoutComplete.aspx* page is loaded, the `DoCheckoutPayment` method is called.</span></span> <span data-ttu-id="db9f8-418">如前所述，`DoCheckoutPayment`方法完成購買程序，從 PayPal 測試環境。</span><span class="sxs-lookup"><span data-stu-id="db9f8-418">As mentioned earlier, the `DoCheckoutPayment` method completes the purchase from the PayPal testing environment.</span></span> <span data-ttu-id="db9f8-419">PayPal 完成之後的訂單，採購*CheckoutComplete.aspx*頁面會顯示付款交易`ID`來購買。</span><span class="sxs-lookup"><span data-stu-id="db9f8-419">Once PayPal has completed the purchase of the order, the *CheckoutComplete.aspx* page displays a payment transaction `ID` to the purchaser.</span></span>

### <a name="handle-cancel-purchase"></a><span data-ttu-id="db9f8-420">處理取消購買</span><span class="sxs-lookup"><span data-stu-id="db9f8-420">Handle Cancel Purchase</span></span>

<span data-ttu-id="db9f8-421">如果使用者決定取消購買，它們會被導向至*CheckoutCancel.aspx*頁面，它們會在其中查看其訂單已取消。</span><span class="sxs-lookup"><span data-stu-id="db9f8-421">If the user decides to cancel the purchase, they will be directed to the *CheckoutCancel.aspx* page where they will see that their order has been cancelled.</span></span>

1. <span data-ttu-id="db9f8-422">開啟名為頁面*CheckoutCancel.aspx*中*簽出*資料夾。</span><span class="sxs-lookup"><span data-stu-id="db9f8-422">Open the page named *CheckoutCancel.aspx* in the *Checkout* folder.</span></span>
2. <span data-ttu-id="db9f8-423">以下列內容取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="db9f8-423">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a><span data-ttu-id="db9f8-424">處理購買錯誤</span><span class="sxs-lookup"><span data-stu-id="db9f8-424">Handle Purchase Errors</span></span>

<span data-ttu-id="db9f8-425">在購買程序期間發生的錯誤會由*CheckoutError.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="db9f8-425">Errors during the purchase process will be handled by the *CheckoutError.aspx* page.</span></span> <span data-ttu-id="db9f8-426">程式碼後置*CheckoutStart.aspx*頁面上， *CheckoutReview.aspx*頁面上，而*CheckoutComplete.aspx*頁面將每個重新導向至*CheckoutError.aspx*頁面上，如果發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="db9f8-426">The code-behind of the *CheckoutStart.aspx* page, the *CheckoutReview.aspx* page, and the *CheckoutComplete.aspx* page will each redirect to the *CheckoutError.aspx* page if an error occurs.</span></span>

1. <span data-ttu-id="db9f8-427">開啟名為頁面*CheckoutError.aspx*中*簽出*資料夾。</span><span class="sxs-lookup"><span data-stu-id="db9f8-427">Open the page named *CheckoutError.aspx* in the *Checkout* folder.</span></span>
2. <span data-ttu-id="db9f8-428">以下列內容取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="db9f8-428">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

<span data-ttu-id="db9f8-429">*CheckoutError.aspx*簽出程序期間發生錯誤時，將會顯示錯誤詳細資料的頁面。</span><span class="sxs-lookup"><span data-stu-id="db9f8-429">The *CheckoutError.aspx* page is displayed with the error details when an error occurs during the checkout process.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="db9f8-430">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="db9f8-430">Running the Application</span></span>

<span data-ttu-id="db9f8-431">執行應用程式，以了解如何購買產品。</span><span class="sxs-lookup"><span data-stu-id="db9f8-431">Run the application to see how to purchase products.</span></span> <span data-ttu-id="db9f8-432">請注意，您將執行中的 PayPal 測試環境。</span><span class="sxs-lookup"><span data-stu-id="db9f8-432">Note that you will be running in the PayPal testing environment.</span></span> <span data-ttu-id="db9f8-433">要交換沒有實際的金額。</span><span class="sxs-lookup"><span data-stu-id="db9f8-433">No actual money is being exchanged.</span></span>

1. <span data-ttu-id="db9f8-434">請確定所有檔案會儲存在 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="db9f8-434">Make sure all your files are saved in Visual Studio.</span></span>
2. <span data-ttu-id="db9f8-435">開啟網頁瀏覽器並瀏覽至[ https://developer.paypal.com ](https://developer.paypal.com/)。</span><span class="sxs-lookup"><span data-stu-id="db9f8-435">Open a Web browser and navigate to [https://developer.paypal.com](https://developer.paypal.com/).</span></span>
3. <span data-ttu-id="db9f8-436">使用您稍早在本教學課程中建立您 PayPal 開發人員帳戶的登入。</span><span class="sxs-lookup"><span data-stu-id="db9f8-436">Login with your PayPal developer account that you created earlier in this tutorial.</span></span>  
   <span data-ttu-id="db9f8-437">您必須在登入 PayPal 的開發人員沙箱[ https://developer.paypal.com ](https://developer.paypal.com/)測試快速簽出。</span><span class="sxs-lookup"><span data-stu-id="db9f8-437">For PayPal's developer sandbox, you need to be logged in at [https://developer.paypal.com](https://developer.paypal.com/) to test express checkout.</span></span> <span data-ttu-id="db9f8-438">這只適用於 PayPal 的沙箱測試、 未 PayPal 的即時環境。</span><span class="sxs-lookup"><span data-stu-id="db9f8-438">This only applies to PayPal's sandbox testing, not to PayPal's live environment.</span></span>
4. <span data-ttu-id="db9f8-439">在 Visual Studio 中按**F5**執行 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="db9f8-439">In Visual Studio, press **F5** to run the Wingtip Toys sample application.</span></span>  
   <span data-ttu-id="db9f8-440">重建資料庫之後，瀏覽器會開啟並顯示*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="db9f8-440">After the database rebuilds, the browser will open and show the *Default.aspx* page.</span></span>
5. <span data-ttu-id="db9f8-441">選取產品類別目錄，例如 Cars，然後按一下 加入購物車的三個不同的產品**加入購物車**旁邊每項產品。</span><span class="sxs-lookup"><span data-stu-id="db9f8-441">Add three different products to the shopping cart by selecting the product category, such as "Cars" and then clicking **Add to Cart** next to each product.</span></span>  
   <span data-ttu-id="db9f8-442">購物車就會顯示您已選取的產品。</span><span class="sxs-lookup"><span data-stu-id="db9f8-442">The shopping cart will display the product you have selected.</span></span>
6. <span data-ttu-id="db9f8-443">按一下 [ **PayPal**簽出] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="db9f8-443">Click the **PayPal** button to checkout.</span></span> 

    ![簽出與使用 PayPal 付款-購物車](checkout-and-payment-with-paypal/_static/image20.png)

   <span data-ttu-id="db9f8-445">正在簽出，將會需要您具備 Wingtip Toys 範例應用程式的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="db9f8-445">Checking out will require that you have a user account for the Wingtip Toys sample application.</span></span>
7. <span data-ttu-id="db9f8-446">按一下  **Google**上現有的 gmail.com 電子郵件帳戶登入頁面右側的連結。</span><span class="sxs-lookup"><span data-stu-id="db9f8-446">Click the **Google** link on the right of the page to log in with an existing gmail.com email account.</span></span>  
   <span data-ttu-id="db9f8-447">如果您沒有 gmail.com 帳戶，您可以建立另一個用於測試用途[www.gmail.com](https://www.gmail.com/)。</span><span class="sxs-lookup"><span data-stu-id="db9f8-447">If you do not have a gmail.com account, you can create one for testing purposes at [www.gmail.com](https://www.gmail.com/).</span></span> <span data-ttu-id="db9f8-448">您也可以使用標準的本機帳戶，依序按一下 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="db9f8-448">You can also use a standard local account by clicking "Register".</span></span> 

    ![簽出與使用 PayPal 付款-登入](checkout-and-payment-with-paypal/_static/image21.png)
8. <span data-ttu-id="db9f8-450">使用您的 gmail 帳戶和密碼登入。</span><span class="sxs-lookup"><span data-stu-id="db9f8-450">Sign in with your gmail account and password.</span></span> 

    ![簽出與使用 PayPal-Gmail 登入的付款](checkout-and-payment-with-paypal/_static/image22.png)
9. <span data-ttu-id="db9f8-452">按一下 [**登入**gmail 帳戶向您 Wingtip Toys 範例應用程式的使用者名稱] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="db9f8-452">Click the **Log in** button to register your gmail account with your Wingtip Toys sample application user name.</span></span> 

    ![簽出與使用 PayPal-註冊帳戶的付款](checkout-and-payment-with-paypal/_static/image23.png)
10. <span data-ttu-id="db9f8-454">PayPal 測試在網站上，新增您**買方**電子郵件地址和您稍早在本教學課程中建立的密碼，然後按一下**登入** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="db9f8-454">On the PayPal test site, add your **buyer** email address and password that you created earlier in this tutorial, then click the **Log In** button.</span></span> 

    ![簽出與使用 PayPal-PayPal 登入的付款](checkout-and-payment-with-paypal/_static/image24.png)
11. <span data-ttu-id="db9f8-456">同意 PayPal 原則，然後按一下**同意後繼續** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="db9f8-456">Agree to the PayPal policy and click the **Agree and Continue** button.</span></span>  
    <span data-ttu-id="db9f8-457">請注意，此頁面才會顯示第一次您使用此 PayPal 帳戶。</span><span class="sxs-lookup"><span data-stu-id="db9f8-457">Note that this page is only displayed the first time you use this PayPal account.</span></span> <span data-ttu-id="db9f8-458">再次請注意，這是測試帳戶，交換任何實際的成本。</span><span class="sxs-lookup"><span data-stu-id="db9f8-458">Again note that this is a test account, no real money is exchanged.</span></span> 

    ![簽出和使用 PayPal-原則 PayPal 付款](checkout-and-payment-with-paypal/_static/image25.png)
12. <span data-ttu-id="db9f8-460">檢閱訂單資訊在測試環境 檢閱 頁面，然後按一下 PayPal**繼續**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-460">Review the order information on the PayPal testing environment review page and click **Continue**.</span></span> 

    ![簽出與使用 PayPal-檢閱資訊的付款](checkout-and-payment-with-paypal/_static/image26.png)
13. <span data-ttu-id="db9f8-462">在  *CheckoutReview.aspx*頁面上，確認訂單金額，檢視產生的送貨地址。</span><span class="sxs-lookup"><span data-stu-id="db9f8-462">On the *CheckoutReview.aspx* page, verify the order amount and view the generated shipping address.</span></span> <span data-ttu-id="db9f8-463">然後，按一下**Complete Order**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="db9f8-463">Then, click the **Complete Order** button.</span></span> 

    ![簽出和使用 PayPal-順序檢閱付款](checkout-and-payment-with-paypal/_static/image27.png)
14. <span data-ttu-id="db9f8-465">**CheckoutComplete.aspx**頁面會顯示與付款交易識別碼。</span><span class="sxs-lookup"><span data-stu-id="db9f8-465">The **CheckoutComplete.aspx** page is displayed with a payment transaction ID.</span></span> 

    ![簽出與使用 PayPal-簽出完整的付款](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a><span data-ttu-id="db9f8-467">檢閱資料庫</span><span class="sxs-lookup"><span data-stu-id="db9f8-467">Reviewing the Database</span></span>

<span data-ttu-id="db9f8-468">藉由檢閱更新的資料，Wingtip Toys 範例應用程式資料庫中執行應用程式之後，您可以看到應用程式成功地錄製購買的產品。</span><span class="sxs-lookup"><span data-stu-id="db9f8-468">By reviewing the updated data in the Wingtip Toys sample application database after running the application, you can see that the application successfully recorded the purchase of the products.</span></span>

<span data-ttu-id="db9f8-469">您可以檢查所包含的資料*Wingtiptoys.mdf*使用的資料庫檔案**資料庫總管**視窗 (**伺服器總管**Visual Studio 中的視窗) 一樣稍早在本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="db9f8-469">You can inspect the data contained in the *Wingtiptoys.mdf* database file by using the **Database Explorer** window (**Server Explorer** window in Visual Studio) as you did earlier in this tutorial series.</span></span>

1. <span data-ttu-id="db9f8-470">如果它仍為開啟狀態，請關閉瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="db9f8-470">Close the browser window if it is still open.</span></span>
2. <span data-ttu-id="db9f8-471">在 Visual Studio 中，選取**顯示所有檔案**頂端的圖示**方案總管**可讓您展開**應用程式\_資料**資料夾。</span><span class="sxs-lookup"><span data-stu-id="db9f8-471">In Visual Studio, select the **Show All Files** icon at the top of **Solution Explorer** to allow you to expand the **App\_Data** folder.</span></span>
3. <span data-ttu-id="db9f8-472">依序展開**應用程式\_資料**資料夾。</span><span class="sxs-lookup"><span data-stu-id="db9f8-472">Expand the **App\_Data** folder.</span></span>  
 <span data-ttu-id="db9f8-473">您可能需要選取**顯示所有檔案**資料夾圖示。</span><span class="sxs-lookup"><span data-stu-id="db9f8-473">You may need to select the **Show All Files** icon for the folder.</span></span>
4. <span data-ttu-id="db9f8-474">以滑鼠右鍵按一下*Wingtiptoys.mdf*資料庫檔案，然後選取**開啟**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-474">Right-click the *Wingtiptoys.mdf* database file and select **Open**.</span></span>  
    <span data-ttu-id="db9f8-475">**伺服器總管**隨即出現。</span><span class="sxs-lookup"><span data-stu-id="db9f8-475">**Server Explorer** is displayed.</span></span>
5. <span data-ttu-id="db9f8-476">依序展開**資料表**資料夾。</span><span class="sxs-lookup"><span data-stu-id="db9f8-476">Expand the **Tables** folder.</span></span>
6. <span data-ttu-id="db9f8-477">以滑鼠右鍵按一下**訂單**資料表，然後選取**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-477">Right-click the **Orders**table and select **Show Table Data**.</span></span>  
 <span data-ttu-id="db9f8-478">**訂單**資料表會顯示。</span><span class="sxs-lookup"><span data-stu-id="db9f8-478">The **Orders** table is displayed.</span></span>
7. <span data-ttu-id="db9f8-479">檢閱**PaymentTransactionID**資料行，以確認成功的交易。</span><span class="sxs-lookup"><span data-stu-id="db9f8-479">Review the **PaymentTransactionID** column to confirm successful transactions.</span></span> 

    ![簽出與使用 PayPal-檢閱資料庫的付款](checkout-and-payment-with-paypal/_static/image29.png)
8. <span data-ttu-id="db9f8-481">關閉**訂單**資料表視窗中。</span><span class="sxs-lookup"><span data-stu-id="db9f8-481">Close the **Orders** table window.</span></span>
9. <span data-ttu-id="db9f8-482">在 [伺服器總管] 中，以滑鼠右鍵按一下**OrderDetails**資料表，然後選取**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-482">In the Server Explorer, right-click the **OrderDetails** table and select **Show Table Data**.</span></span>
10. <span data-ttu-id="db9f8-483">檢閱`OrderId`並`Username`中的值**OrderDetails**資料表。</span><span class="sxs-lookup"><span data-stu-id="db9f8-483">Review the `OrderId` and `Username` values in the **OrderDetails** table.</span></span> <span data-ttu-id="db9f8-484">請注意，這些值符合`OrderId`並`Username`值納入**訂單**資料表。</span><span class="sxs-lookup"><span data-stu-id="db9f8-484">Note that these values match the `OrderId` and `Username` values included in the **Orders** table.</span></span>
11. <span data-ttu-id="db9f8-485">關閉**OrderDetails**資料表視窗中。</span><span class="sxs-lookup"><span data-stu-id="db9f8-485">Close the **OrderDetails** table window.</span></span>
12. <span data-ttu-id="db9f8-486">以滑鼠右鍵按一下 Wingtip Toys 資料庫檔案 (*Wingtiptoys.mdf*)，然後選取**親密**。</span><span class="sxs-lookup"><span data-stu-id="db9f8-486">Right-click the Wingtip Toys database file (*Wingtiptoys.mdf*) and select **Close Connection**.</span></span>
13. <span data-ttu-id="db9f8-487">如果您看不**方案總管 中** 視窗中，按一下**方案總管 中**底部**伺服器總管**視窗以顯示**方案總管 中**一次。</span><span class="sxs-lookup"><span data-stu-id="db9f8-487">If you do not see the **Solution Explorer** window, click **Solution Explorer** at the bottom of the **Server Explorer** window to show the **Solution Explorer** again.</span></span>

## <a name="summary"></a><span data-ttu-id="db9f8-488">總結</span><span class="sxs-lookup"><span data-stu-id="db9f8-488">Summary</span></span>

<span data-ttu-id="db9f8-489">在本教學課程中新增訂單及訂單詳細資料的結構描述追蹤購買的產品。</span><span class="sxs-lookup"><span data-stu-id="db9f8-489">In this tutorial you added order and order detail schemas to track the purchase of products.</span></span> <span data-ttu-id="db9f8-490">您也會整合到 Wingtip Toys 範例應用程式的 PayPal 功能。</span><span class="sxs-lookup"><span data-stu-id="db9f8-490">You also integrated PayPal functionality into the Wingtip Toys sample application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="db9f8-491">其他資源</span><span class="sxs-lookup"><span data-stu-id="db9f8-491">Additional Resources</span></span>

<span data-ttu-id="db9f8-492">[PřEHLED Konfigurace Technologie asp.net](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="db9f8-492">[ASP.NET Configuration Overview](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)</span></span>  
[<span data-ttu-id="db9f8-493">使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET Web Forms 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="db9f8-493">Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[<span data-ttu-id="db9f8-494">Microsoft Azure-免費試用版</span><span class="sxs-lookup"><span data-stu-id="db9f8-494">Microsoft Azure - Free Trial</span></span>](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a><span data-ttu-id="db9f8-495">免責聲明</span><span class="sxs-lookup"><span data-stu-id="db9f8-495">Disclaimer</span></span>

<span data-ttu-id="db9f8-496">本教學課程包含範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="db9f8-496">This tutorial contains sample code.</span></span> <span data-ttu-id="db9f8-497">這類的範例程式碼係依 「 現況 」 不含任何種類的擔保。</span><span class="sxs-lookup"><span data-stu-id="db9f8-497">Such sample code is provided "as is" without warranty of any kind.</span></span> <span data-ttu-id="db9f8-498">因此，Microsoft 不保證精確度、 完整性或範例程式碼的品質。</span><span class="sxs-lookup"><span data-stu-id="db9f8-498">Accordingly, Microsoft does not guarantee the accuracy, integrity, or quality of the sample code.</span></span> <span data-ttu-id="db9f8-499">若要自行承擔使用的範例程式碼即表示您同意。</span><span class="sxs-lookup"><span data-stu-id="db9f8-499">You agree to use the sample code at your own risk.</span></span> <span data-ttu-id="db9f8-500">在任何情況下 Microsoft 一概不給您以任何方式任何範例程式碼，內容，包括但不是限於任何錯誤或省略任何範例程式碼、 內容或任何遺失或損毀所造成的結果的任何範例程式碼使用任何種類。</span><span class="sxs-lookup"><span data-stu-id="db9f8-500">Under no circumstances will Microsoft be liable to you in any way for any sample code, content, including but not limited to, any errors or omissions in any sample code, content, or any loss or damage of any kind incurred as a result of the use of any sample code.</span></span> <span data-ttu-id="db9f8-501">您茲此通知和茲此同意賠償、 儲存及保留 Microsoft 無害因而遭受任何和所有的遺失、 遺失、 傷害或任何種類包括，但不限於由 occasioned 或所引發的回傳時，資料的損毀的宣告傳輸、 使用或依賴包括但不是限於其中表示的檢視。</span><span class="sxs-lookup"><span data-stu-id="db9f8-501">You are hereby notified and do hereby agree to indemnify, save and hold Microsoft harmless from and against any and all loss, claims of loss, injury or damage of any kind including, without limitation, those occasioned by or arising from material that you post, transmit, use or rely on including, but not limited to, the views expressed therein.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="db9f8-502">[上一頁](shopping-cart.md)
> [下一頁](membership-and-administration.md)</span><span class="sxs-lookup"><span data-stu-id="db9f8-502">[Previous](shopping-cart.md)
[Next](membership-and-administration.md)</span></span>
