---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: 購物車 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 2823970144e6dce4a3a9f4d28dabfabe9fe053d6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833454"
---
<a name="shopping-cart"></a><span data-ttu-id="9358f-103">購物車</span><span class="sxs-lookup"><span data-stu-id="9358f-103">Shopping Cart</span></span>
====================
<span data-ttu-id="9358f-104">藉由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="9358f-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="9358f-105">[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="9358f-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="9358f-106">本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="9358f-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="9358f-107">Visual Studio 2013[含有 C# 原始程式碼專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附了本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="9358f-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="9358f-108">本教學課程說明將購物車新增 「 Wingtip Toys 範例 ASP.NET Web Forms 應用程式所需的商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="9358f-108">This tutorial describes the business logic required to add a shopping cart to the Wingtip Toys sample ASP.NET Web Forms application.</span></span> <span data-ttu-id="9358f-109">此教學課程的上一個教學課程 [顯示資料的項目和詳細資料]，並且是 Wingtip 玩具店教學課程系列的一部分。</span><span class="sxs-lookup"><span data-stu-id="9358f-109">This tutorial builds on the previous tutorial "Display Data Items and Details" and is part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="9358f-110">當您完成本教學課程中時，您的範例應用程式的使用者可以新增、 移除及修改客戶購物車中的產品。</span><span class="sxs-lookup"><span data-stu-id="9358f-110">When you've completed this tutorial, the users of your sample app will be able to add, remove, and modify the products in their shopping cart.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="9358f-111">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="9358f-111">What you'll learn:</span></span>

1. <span data-ttu-id="9358f-112">如何建立 web 應用程式的購物車。</span><span class="sxs-lookup"><span data-stu-id="9358f-112">How to create a shopping cart for the web application.</span></span>
2. <span data-ttu-id="9358f-113">如何讓使用者將項目加入至購物車。</span><span class="sxs-lookup"><span data-stu-id="9358f-113">How to enable users to add items to the shopping cart.</span></span>
3. <span data-ttu-id="9358f-114">如何新增[GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction)控制項來顯示購物車 」 詳細資料。</span><span class="sxs-lookup"><span data-stu-id="9358f-114">How to add a [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) control to display shopping cart details.</span></span>
4. <span data-ttu-id="9358f-115">如何計算並顯示訂單總計。</span><span class="sxs-lookup"><span data-stu-id="9358f-115">How to calculate and display the order total.</span></span>
5. <span data-ttu-id="9358f-116">如何移除和更新購物車中的項目。</span><span class="sxs-lookup"><span data-stu-id="9358f-116">How to remove and update items in the shopping cart.</span></span>
6. <span data-ttu-id="9358f-117">如何加入購物車 」 計數器。</span><span class="sxs-lookup"><span data-stu-id="9358f-117">How to include a shopping cart counter.</span></span>

## <a name="code-features-in-this-tutorial"></a><span data-ttu-id="9358f-118">在本教學課程的程式碼功能：</span><span class="sxs-lookup"><span data-stu-id="9358f-118">Code features in this tutorial:</span></span>

1. <span data-ttu-id="9358f-119">Entity Framework Code First</span><span class="sxs-lookup"><span data-stu-id="9358f-119">Entity Framework Code First</span></span>
2. <span data-ttu-id="9358f-120">資料註釋</span><span class="sxs-lookup"><span data-stu-id="9358f-120">Data Annotations</span></span>
3. <span data-ttu-id="9358f-121">強型別資料控制項</span><span class="sxs-lookup"><span data-stu-id="9358f-121">Strongly typed data controls</span></span>
4. <span data-ttu-id="9358f-122">模型繫結</span><span class="sxs-lookup"><span data-stu-id="9358f-122">Model binding</span></span>

## <a name="creating-a-shopping-cart"></a><span data-ttu-id="9358f-123">建立購物車</span><span class="sxs-lookup"><span data-stu-id="9358f-123">Creating a Shopping Cart</span></span>

<span data-ttu-id="9358f-124">稍早在本教學課程系列中，您可以加入頁面和程式碼，以檢視資料庫中的產品資料。</span><span class="sxs-lookup"><span data-stu-id="9358f-124">Earlier in this tutorial series, you added pages and code to view product data from a database.</span></span> <span data-ttu-id="9358f-125">在本教學課程中，您將建立購物車管理產品的使用者有興趣購買。</span><span class="sxs-lookup"><span data-stu-id="9358f-125">In this tutorial, you'll create a shopping cart to manage the products that users are interested in buying.</span></span> <span data-ttu-id="9358f-126">使用者將能夠瀏覽並放入購物車新增項目，即使未註冊或登入。</span><span class="sxs-lookup"><span data-stu-id="9358f-126">Users will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="9358f-127">若要管理購物車存取權，您會將使用者指派的唯一`ID`第一次使用全域唯一識別碼 (GUID)，當使用者存取購物車。</span><span class="sxs-lookup"><span data-stu-id="9358f-127">To manage shopping cart access, you will assign users a unique `ID` using a globally unique identifier (GUID) when the user accesses the shopping cart for the first time.</span></span> <span data-ttu-id="9358f-128">想要儲存這`ID`使用 ASP.NET 工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="9358f-128">You'll store this `ID` using the ASP.NET Session state.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9358f-129">ASP.NET 工作階段狀態是方便的地方來儲存使用者專屬資訊的使用者離開網站就會到期。</span><span class="sxs-lookup"><span data-stu-id="9358f-129">The ASP.NET Session state is a convenient place to store user-specific information which will expire after the user leaves the site.</span></span> <span data-ttu-id="9358f-130">雖然不當使用工作階段狀態可以有較大的網站上的效能影響，受到使用的工作階段狀態的運作也供示範之用。</span><span class="sxs-lookup"><span data-stu-id="9358f-130">While misuse of session state can have performance implications on larger sites, light use of session state works well for demonstration purposes.</span></span> <span data-ttu-id="9358f-131">Wingtip Toys 範例專案會示範如何使用工作階段狀態而不需要外部提供者，其中的工作階段狀態是儲存在同處理序裝載站台的 web 伺服器上。</span><span class="sxs-lookup"><span data-stu-id="9358f-131">The Wingtip Toys sample project shows how to use session state without an external provider, where session state is stored in-process on the web server hosting the site.</span></span> <span data-ttu-id="9358f-132">提供的應用程式的多個執行個體的大型網站或在不同的伺服器執行的應用程式的多個執行個體的站台，請考慮使用**Windows Azure 快取服務**。</span><span class="sxs-lookup"><span data-stu-id="9358f-132">For larger sites that provide multiple instances of an application or for sites that run multiple instances of an application on different servers, consider using **Windows Azure Cache Service**.</span></span> <span data-ttu-id="9358f-133">此快取服務提供分散式快取服務是在外部網站，可解決使用同處理序工作階段狀態的問題。</span><span class="sxs-lookup"><span data-stu-id="9358f-133">This Cache Service provides a distributed caching service that is external to the web site and solves the problem of using in-process session state.</span></span> <span data-ttu-id="9358f-134">如需詳細資訊，請參閱[如何使用 ASP.NET 工作階段狀態與 Windows Azure 網站](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider)。</span><span class="sxs-lookup"><span data-stu-id="9358f-134">For more information see, [How to Use ASP.NET Session State with Windows Azure Web Sites](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).</span></span>


### <a name="add-cartitem-as-a-model-class"></a><span data-ttu-id="9358f-135">新增 CartItem 做為模型類別</span><span class="sxs-lookup"><span data-stu-id="9358f-135">Add CartItem as a Model Class</span></span>

<span data-ttu-id="9358f-136">稍早在本教學課程系列中，您必須定義的分類和產品資料的結構描述建立`Category`並`Product`中的類別*模型*資料夾。</span><span class="sxs-lookup"><span data-stu-id="9358f-136">Earlier in this tutorial series, you defined the schema for the category and product data by creating the `Category` and `Product` classes in the *Models* folder.</span></span> <span data-ttu-id="9358f-137">現在，加入新的類別，來定義購物車的結構描述。</span><span class="sxs-lookup"><span data-stu-id="9358f-137">Now, add a new class to define the schema for the shopping cart.</span></span> <span data-ttu-id="9358f-138">稍後在本教學課程中，您會將類別來處理資料的存取權`CartItem`資料表。</span><span class="sxs-lookup"><span data-stu-id="9358f-138">Later in this tutorial, you will add a class to handle data access to the `CartItem` table.</span></span> <span data-ttu-id="9358f-139">這個類別會提供商務邏輯，以新增、 移除及更新購物車中的項目。</span><span class="sxs-lookup"><span data-stu-id="9358f-139">This class will provide the business logic to add, remove, and update items in the shopping cart.</span></span>

1. <span data-ttu-id="9358f-140">以滑鼠右鍵按一下*模型*資料夾，然後選取**新增** - &gt; **新項目**。</span><span class="sxs-lookup"><span data-stu-id="9358f-140">Right-click the *Models* folder and select **Add** -&gt; **New Item**.</span></span> 

    ![「 購物車 」-新的項目](shopping-cart/_static/image1.png)
2. <span data-ttu-id="9358f-142">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="9358f-142">The **Add New Item** dialog box is displayed.</span></span> <span data-ttu-id="9358f-143">選取 **程式碼**，然後選取**類別**。</span><span class="sxs-lookup"><span data-stu-id="9358f-143">Select **Code**, and then select **Class**.</span></span> 

    ![「 購物車 」-加入新項目 對話方塊](shopping-cart/_static/image2.png)
3. <span data-ttu-id="9358f-145">這個新類別命名*CartItem.cs*。</span><span class="sxs-lookup"><span data-stu-id="9358f-145">Name this new class *CartItem.cs*.</span></span>
4. <span data-ttu-id="9358f-146">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="9358f-146">Click **Add**.</span></span>  
   <span data-ttu-id="9358f-147">新的類別檔案會顯示在編輯器中。</span><span class="sxs-lookup"><span data-stu-id="9358f-147">The new class file is displayed in the editor.</span></span>
5. <span data-ttu-id="9358f-148">取代為下列程式碼中的預設程式碼：</span><span class="sxs-lookup"><span data-stu-id="9358f-148">Replace the default code with the following code:</span></span>   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

<span data-ttu-id="9358f-149">`CartItem`類別包含的結構描述會定義每個使用者新增至購物車的產品。</span><span class="sxs-lookup"><span data-stu-id="9358f-149">The `CartItem` class contains the schema that will define each product a user adds to the shopping cart.</span></span> <span data-ttu-id="9358f-150">這個類別是與其他您稍早在本教學課程系列中建立的結構描述類別類似。</span><span class="sxs-lookup"><span data-stu-id="9358f-150">This class is similar to the other schema classes you created earlier in this tutorial series.</span></span> <span data-ttu-id="9358f-151">依照慣例，Entity Framework Code First 所預期的主索引鍵`CartItem`資料表會`CartItemId`或`ID`。</span><span class="sxs-lookup"><span data-stu-id="9358f-151">By convention, Entity Framework Code First expects that the primary key for the `CartItem` table will be either `CartItemId` or `ID`.</span></span> <span data-ttu-id="9358f-152">不過，程式碼會覆寫預設行為使用資料註解`[Key]`屬性。</span><span class="sxs-lookup"><span data-stu-id="9358f-152">However, the code overrides the default behavior by using the data annotation `[Key]` attribute.</span></span> <span data-ttu-id="9358f-153">`Key` ItemId 屬性的屬性會指定`ItemID`屬性是主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9358f-153">The `Key` attribute of the ItemId property specifies that the `ItemID` property is the primary key.</span></span>

<span data-ttu-id="9358f-154">`CartId`屬性會指定`ID`来購買的項目相關聯的使用者。</span><span class="sxs-lookup"><span data-stu-id="9358f-154">The `CartId` property specifies the `ID` of the user that is associated with the item to purchase.</span></span> <span data-ttu-id="9358f-155">您將新增程式碼來建立此使用者`ID`當使用者存取購物車。</span><span class="sxs-lookup"><span data-stu-id="9358f-155">You'll add code to create this user `ID` when the user accesses the shopping cart.</span></span> <span data-ttu-id="9358f-156">這`ID`也會儲存為 ASP.NET 工作階段變數。</span><span class="sxs-lookup"><span data-stu-id="9358f-156">This `ID` will also be stored as an ASP.NET Session variable.</span></span>

### <a name="update-the-product-context"></a><span data-ttu-id="9358f-157">更新的產品內容</span><span class="sxs-lookup"><span data-stu-id="9358f-157">Update the Product Context</span></span>

<span data-ttu-id="9358f-158">除了新增`CartItem`類別，您必須更新資料庫內容類別，可管理的實體類別，並提供對資料庫的資料存取。</span><span class="sxs-lookup"><span data-stu-id="9358f-158">In addition to adding the `CartItem` class, you will need to update the database context class that manages the entity classes and that provides data access to the database.</span></span> <span data-ttu-id="9358f-159">若要這樣做，您將新建`CartItem`模型類別來`ProductContext`類別。</span><span class="sxs-lookup"><span data-stu-id="9358f-159">To do this, you will add the newly created `CartItem` model class to the `ProductContext` class.</span></span>

1. <span data-ttu-id="9358f-160">在 **方案總管**，尋找並開啟*ProductContext.cs*中的檔案*模型*資料夾。</span><span class="sxs-lookup"><span data-stu-id="9358f-160">In **Solution Explorer**, find and open the *ProductContext.cs* file in the *Models* folder.</span></span>
2. <span data-ttu-id="9358f-161">將反白顯示的程式碼加入*ProductContext.cs*檔案，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9358f-161">Add the highlighted code to the *ProductContext.cs* file as follows:</span></span>  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

<span data-ttu-id="9358f-162">如先前在本教學課程系列中的程式碼中所述*ProductContext.cs*檔都會將`System.Data.Entity`命名空間，所以您需要的 Entity framework 的所有核心功能的存取權。</span><span class="sxs-lookup"><span data-stu-id="9358f-162">As mentioned previously in this tutorial series, the code in the *ProductContext.cs* file adds the `System.Data.Entity` namespace so that you have access to all the core functionality of the Entity Framework.</span></span> <span data-ttu-id="9358f-163">這項功能包括查詢、 插入、 更新和刪除資料，藉由使用強型別物件的功能。</span><span class="sxs-lookup"><span data-stu-id="9358f-163">This functionality includes the capability to query, insert, update, and delete data by working with strongly typed objects.</span></span> <span data-ttu-id="9358f-164">`ProductContext`類別加入至新加入的存取`CartItem`模型類別。</span><span class="sxs-lookup"><span data-stu-id="9358f-164">The `ProductContext` class adds access to the newly added `CartItem` model class.</span></span>

### <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="9358f-165">管理購物車商務邏輯</span><span class="sxs-lookup"><span data-stu-id="9358f-165">Managing the Shopping Cart Business Logic</span></span>

<span data-ttu-id="9358f-166">接下來，您將建立`ShoppingCart`類別的新*邏輯*資料夾。</span><span class="sxs-lookup"><span data-stu-id="9358f-166">Next, you'll create the `ShoppingCart` class in a new *Logic* folder.</span></span> <span data-ttu-id="9358f-167">`ShoppingCart`類別處理的資料存取`CartItem`資料表。</span><span class="sxs-lookup"><span data-stu-id="9358f-167">The `ShoppingCart` class handles data access to the `CartItem` table.</span></span> <span data-ttu-id="9358f-168">類別也會包含商務邏輯，來新增、 移除及更新購物車中的項目。</span><span class="sxs-lookup"><span data-stu-id="9358f-168">The class will also include the business logic to add, remove, and update items in the shopping cart.</span></span>

<span data-ttu-id="9358f-169">您將加入購物車邏輯將會包含的功能來管理下列動作：</span><span class="sxs-lookup"><span data-stu-id="9358f-169">The shopping cart logic that you will add will contain the functionality to manage the following actions:</span></span>

1. <span data-ttu-id="9358f-170">將項目加入購物車</span><span class="sxs-lookup"><span data-stu-id="9358f-170">Adding items to the shopping cart</span></span>
2. <span data-ttu-id="9358f-171">移除購物車中的項目</span><span class="sxs-lookup"><span data-stu-id="9358f-171">Removing items from the shopping cart</span></span>
3. <span data-ttu-id="9358f-172">取得此購物車識別碼</span><span class="sxs-lookup"><span data-stu-id="9358f-172">Getting the shopping cart ID</span></span>
4. <span data-ttu-id="9358f-173">正在擷取項目從購物車 」</span><span class="sxs-lookup"><span data-stu-id="9358f-173">Retrieving items from the shopping cart</span></span>
5. <span data-ttu-id="9358f-174">加總所有的購物車項目數量</span><span class="sxs-lookup"><span data-stu-id="9358f-174">Totaling the amount of all the shopping cart items</span></span>
6. <span data-ttu-id="9358f-175">更新的購物車資料</span><span class="sxs-lookup"><span data-stu-id="9358f-175">Updating the shopping cart data</span></span>

<span data-ttu-id="9358f-176">購物車 頁面 (*ShoppingCart.aspx*) 和購物車類別會一起用來存取購物車資料。</span><span class="sxs-lookup"><span data-stu-id="9358f-176">A shopping cart page (*ShoppingCart.aspx*) and the shopping cart class will be used together to access shopping cart data.</span></span> <span data-ttu-id="9358f-177">購物車 頁面會顯示所有使用者新增至購物車的項目。</span><span class="sxs-lookup"><span data-stu-id="9358f-177">The shopping cart page will display all the items the user adds to the shopping cart.</span></span> <span data-ttu-id="9358f-178">Besides 購物車網頁和類別，您將建立的頁面 (*AddToCart.aspx*) 若要將產品加入購物車。</span><span class="sxs-lookup"><span data-stu-id="9358f-178">Besides the shopping cart page and class, you'll create a page (*AddToCart.aspx*) to add products to the shopping cart.</span></span> <span data-ttu-id="9358f-179">您也將加入程式碼*ProductList.aspx*頁面並*ProductDetails.aspx*會提供連結的頁面*AddToCart.aspx*頁面上，好讓使用者可以加入放入購物車的產品。</span><span class="sxs-lookup"><span data-stu-id="9358f-179">You will also add code to the *ProductList.aspx* page and the *ProductDetails.aspx* page that will provide a link to the *AddToCart.aspx* page, so that the user can add products to the shopping cart.</span></span>

<span data-ttu-id="9358f-180">下圖顯示當使用者加入至購物車的產品，就會發生的基本程序。</span><span class="sxs-lookup"><span data-stu-id="9358f-180">The following diagram shows the basic process that occurs when the user adds a product to the shopping cart.</span></span>

![「 購物車 」-新增至購物車](shopping-cart/_static/image3.png)

<span data-ttu-id="9358f-182">當使用者按一下**加入購物車**上的連結*ProductList.aspx*頁面或*ProductDetails.aspx*  頁面上，應用程式會瀏覽至*AddToCart.aspx*頁面，然後自動為*ShoppingCart.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="9358f-182">When the user clicks the **Add To Cart** link on either the *ProductList.aspx* page or the *ProductDetails.aspx* page, the application will navigate to the *AddToCart.aspx* page and then automatically to the *ShoppingCart.aspx* page.</span></span> <span data-ttu-id="9358f-183">*AddToCart.aspx*頁面將選取的產品加入購物車 ShoppingCart 類別中呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="9358f-183">The *AddToCart.aspx* page will add the select product to the shopping cart by calling a method in the ShoppingCart class.</span></span> <span data-ttu-id="9358f-184">*ShoppingCart.aspx*頁面會顯示已新增至購物車的產品。</span><span class="sxs-lookup"><span data-stu-id="9358f-184">The *ShoppingCart.aspx* page will display the products that have been added to the shopping cart.</span></span>

#### <a name="creating-the-shopping-cart-class"></a><span data-ttu-id="9358f-185">建立購物車類別</span><span class="sxs-lookup"><span data-stu-id="9358f-185">Creating the Shopping Cart Class</span></span>

<span data-ttu-id="9358f-186">`ShoppingCart`類別會新增到不同的資料夾中的應用程式，以便將會有明顯的區分模型 （[模型] 資料夾）、 （根資料夾） 的頁面和邏輯 （邏輯資料夾）。</span><span class="sxs-lookup"><span data-stu-id="9358f-186">The `ShoppingCart` class will be added to a separate folder in the application so that there will be a clear distinction between the model (Models folder), the pages (root folder) and the logic (Logic folder).</span></span>

1. <span data-ttu-id="9358f-187">在 [**方案總管] 中**，以滑鼠右鍵按一下**WingtipToys**專案，然後選取**新增**-&gt;**新資料夾**.</span><span class="sxs-lookup"><span data-stu-id="9358f-187">In **Solution Explorer**, right-click the **WingtipToys**project and select **Add**-&gt;**New Folder**.</span></span> <span data-ttu-id="9358f-188">將新資料夾命名*邏輯*。</span><span class="sxs-lookup"><span data-stu-id="9358f-188">Name the new folder *Logic*.</span></span>
2. <span data-ttu-id="9358f-189">以滑鼠右鍵按一下*邏輯*資料夾，然後選取**新增** - &gt; **新項目**。</span><span class="sxs-lookup"><span data-stu-id="9358f-189">Right-click the *Logic* folder and then select **Add** -&gt; **New Item**.</span></span>
3. <span data-ttu-id="9358f-190">加入新的類別檔案，名為*ShoppingCartActions.cs*。</span><span class="sxs-lookup"><span data-stu-id="9358f-190">Add a new class file named *ShoppingCartActions.cs*.</span></span>
4. <span data-ttu-id="9358f-191">取代為下列程式碼中的預設程式碼：</span><span class="sxs-lookup"><span data-stu-id="9358f-191">Replace the default code with the following code:</span></span>   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

<span data-ttu-id="9358f-192">`AddToCart`方法可讓要納入購物車的產品為基礎的個別產品`ID`。</span><span class="sxs-lookup"><span data-stu-id="9358f-192">The `AddToCart` method enables individual products to be included in the shopping cart based on the product `ID`.</span></span> <span data-ttu-id="9358f-193">產品加入購物車中，或如果購物車已經包含該產品的項目，就會遞增數量。</span><span class="sxs-lookup"><span data-stu-id="9358f-193">The product is added to the cart, or if the cart already contains an item for that product, the quantity is incremented.</span></span>

<span data-ttu-id="9358f-194">`GetCartId`方法會傳回購物車`ID`使用者。</span><span class="sxs-lookup"><span data-stu-id="9358f-194">The `GetCartId` method returns the cart `ID` for the user.</span></span> <span data-ttu-id="9358f-195">購物車`ID`用來追蹤使用者具有客戶購物車中的項目。</span><span class="sxs-lookup"><span data-stu-id="9358f-195">The cart `ID` is used to track the items that a user has in their shopping cart.</span></span> <span data-ttu-id="9358f-196">如果使用者沒有現有的購物車`ID`，新車`ID`為其建立。</span><span class="sxs-lookup"><span data-stu-id="9358f-196">If the user does not have an existing cart `ID`, a new cart `ID` is created for them.</span></span> <span data-ttu-id="9358f-197">如果使用者身分登入已註冊的使用者，購物車`ID`設為其使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="9358f-197">If the user is signed in as a registered user, the cart `ID` is set to their user name.</span></span> <span data-ttu-id="9358f-198">不過，如果使用者未登入時，購物車`ID`設為唯一的值 (GUID)。</span><span class="sxs-lookup"><span data-stu-id="9358f-198">However, if the user is not signed in, the cart `ID` is set to a unique value (a GUID).</span></span> <span data-ttu-id="9358f-199">GUID 可確保該只有一個購物車會建立每個使用者工作階段為基礎。</span><span class="sxs-lookup"><span data-stu-id="9358f-199">A GUID ensures that only one cart is created for each user, based on session.</span></span>

<span data-ttu-id="9358f-200">`GetCartItems`方法會傳回一份 「 購物車項目的使用者。</span><span class="sxs-lookup"><span data-stu-id="9358f-200">The `GetCartItems` method returns a list of shopping cart items for the user.</span></span> <span data-ttu-id="9358f-201">稍後在本教學課程中，您會看到模型繫結，用來顯示購物車的項目購物車 」 使用`GetCartItems`方法。</span><span class="sxs-lookup"><span data-stu-id="9358f-201">Later in this tutorial, you will see that model binding is used to display the cart items in the shopping cart using the `GetCartItems` method.</span></span>

### <a name="creating-the-add-to-cart-functionality"></a><span data-ttu-id="9358f-202">建立加入--車功能</span><span class="sxs-lookup"><span data-stu-id="9358f-202">Creating the Add-To-Cart Functionality</span></span>

<span data-ttu-id="9358f-203">如先前所述，您將建立名為處理網頁*AddToCart.aspx* ，用以將新產品加入購物車的使用者。</span><span class="sxs-lookup"><span data-stu-id="9358f-203">As mentioned earlier, you will create a processing page named *AddToCart.aspx* that will be used to add new products to the shopping cart of the user.</span></span> <span data-ttu-id="9358f-204">此頁面會呼叫`AddToCart`方法中的`ShoppingCart`您剛才建立的類別。</span><span class="sxs-lookup"><span data-stu-id="9358f-204">This page will call the `AddToCart` method in the `ShoppingCart` class that you just created.</span></span> <span data-ttu-id="9358f-205">*AddToCart.aspx*頁面就會預期收到的產品`ID`傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="9358f-205">The *AddToCart.aspx* page will expect that a product `ID` is passed to it.</span></span> <span data-ttu-id="9358f-206">此產品`ID`呼叫時，會使用`AddToCart`方法中的`ShoppingCart`類別。</span><span class="sxs-lookup"><span data-stu-id="9358f-206">This product `ID` will be used when calling the `AddToCart` method in the `ShoppingCart` class.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9358f-207">您將修改程式碼後置 (*AddToCart.aspx.cs*) 的這個頁面上，而不是頁面的 UI (*AddToCart.aspx*)。</span><span class="sxs-lookup"><span data-stu-id="9358f-207">You will be modifying the code-behind (*AddToCart.aspx.cs*) for this page, not the page UI (*AddToCart.aspx*).</span></span>


#### <a name="to-create-the-add-to-cart-functionality"></a><span data-ttu-id="9358f-208">若要建立購物車新增功能：</span><span class="sxs-lookup"><span data-stu-id="9358f-208">To create the Add-To-Cart functionality:</span></span>

1. <span data-ttu-id="9358f-209">在 **方案總管 中**，以滑鼠右鍵按一下**WingtipToys**專案，按一下 **新增** - &gt; **新項目**。</span><span class="sxs-lookup"><span data-stu-id="9358f-209">In **Solution Explorer**, right-click the **WingtipToys**project, click **Add** -&gt; **New Item**.</span></span>  
   <span data-ttu-id="9358f-210">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="9358f-210">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="9358f-211">將標準的新頁面 (Web Form) 新增至名為應用程式*AddToCart.aspx*。</span><span class="sxs-lookup"><span data-stu-id="9358f-211">Add a standard new page (Web Form) to the application named *AddToCart.aspx*.</span></span> 

    ![「 購物車 」-加入 Web Form](shopping-cart/_static/image4.png)
3. <span data-ttu-id="9358f-213">在 **方案總管**，以滑鼠右鍵按一下*AddToCart.aspx*頁面，然後按一下**檢視程式碼**。</span><span class="sxs-lookup"><span data-stu-id="9358f-213">In **Solution Explorer**, right-click the *AddToCart.aspx* page and then click **View Code**.</span></span> <span data-ttu-id="9358f-214">*AddToCart.aspx.cs*在編輯器中開啟程式碼後置檔案。</span><span class="sxs-lookup"><span data-stu-id="9358f-214">The *AddToCart.aspx.cs* code-behind file is opened in the editor.</span></span>
4. <span data-ttu-id="9358f-215">取代現有的程式碼中*AddToCart.aspx.cs*以下列程式碼後置：</span><span class="sxs-lookup"><span data-stu-id="9358f-215">Replace the existing code in the *AddToCart.aspx.cs* code-behind with the following:</span></span>   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

<span data-ttu-id="9358f-216">當*AddToCart.aspx*載入頁面時，產品`ID`會從查詢字串。</span><span class="sxs-lookup"><span data-stu-id="9358f-216">When the *AddToCart.aspx* page is loaded, the product `ID` is retrieved from the query string.</span></span> <span data-ttu-id="9358f-217">接下來，購物車 」 類別的執行個體，並用來呼叫`AddToCart`您稍早在本教學課程中新增的方法。</span><span class="sxs-lookup"><span data-stu-id="9358f-217">Next, an instance of the shopping cart class is created and used to call the `AddToCart` method that you added earlier in this tutorial.</span></span> <span data-ttu-id="9358f-218">`AddToCart`方法，包含在*ShoppingCartActions.cs*檔案中，包含邏輯，可將選取的產品加入購物車或遞增的所選產品的產品數量。</span><span class="sxs-lookup"><span data-stu-id="9358f-218">The `AddToCart` method, contained in the *ShoppingCartActions.cs* file, includes the logic to add the selected product to the shopping cart or increment the product quantity of the selected product.</span></span> <span data-ttu-id="9358f-219">如果尚未加入購物車產品，產品會新增至`CartItem`資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="9358f-219">If the product hasn't been added to the shopping cart, the product is added to the `CartItem` table of the database.</span></span> <span data-ttu-id="9358f-220">如果產品已經加入至購物車，使用者會新增額外的項目相同產品之產品數量會在遞增`CartItem`資料表。</span><span class="sxs-lookup"><span data-stu-id="9358f-220">If the product has already been added to the shopping cart and the user adds an additional item of the same product, the product quantity is incremented in the `CartItem` table.</span></span> <span data-ttu-id="9358f-221">最後，頁面重新導向回到*ShoppingCart.aspx*將在下一個步驟中，其中使用者會看到更新的購物車中的項目清單的頁面。</span><span class="sxs-lookup"><span data-stu-id="9358f-221">Finally, the page redirects back to the *ShoppingCart.aspx* page that you'll add in the next step, where the user sees an updated list of items in the cart.</span></span>

<span data-ttu-id="9358f-222">如先前所述，使用者`ID`用來識別特定使用者相關聯的產品。</span><span class="sxs-lookup"><span data-stu-id="9358f-222">As previously mentioned, a user `ID` is used to identify the products that are associated with a specific user.</span></span> <span data-ttu-id="9358f-223">這`ID`中的資料列加入`CartItem`資料表每次使用者加入至購物車的產品。</span><span class="sxs-lookup"><span data-stu-id="9358f-223">This `ID` is added to a row in the `CartItem` table each time the user adds a product to the shopping cart.</span></span>

### <a name="creating-the-shopping-cart-ui"></a><span data-ttu-id="9358f-224">建立購物車 UI</span><span class="sxs-lookup"><span data-stu-id="9358f-224">Creating the Shopping Cart UI</span></span>

<span data-ttu-id="9358f-225">*ShoppingCart.aspx*頁面會顯示使用者已新增至購物車的產品。</span><span class="sxs-lookup"><span data-stu-id="9358f-225">The *ShoppingCart.aspx* page will display the products that the user has added to their shopping cart.</span></span> <span data-ttu-id="9358f-226">它也會提供能夠新增、 移除及更新購物車中的項目。</span><span class="sxs-lookup"><span data-stu-id="9358f-226">It will also provide the ability to add, remove and update items in the shopping cart.</span></span>

1. <span data-ttu-id="9358f-227">在 **方案總管 中**，以滑鼠右鍵按一下**WingtipToys**，按一下 **新增** - &gt; **新項目**。</span><span class="sxs-lookup"><span data-stu-id="9358f-227">In **Solution Explorer**, right-click **WingtipToys**, click **Add** -&gt; **New Item**.</span></span>  
   <span data-ttu-id="9358f-228">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="9358f-228">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="9358f-229">加入新的頁面 (Web Form)，其中包含所選取的主版頁面**使用主版頁面的 Web Form**。</span><span class="sxs-lookup"><span data-stu-id="9358f-229">Add a new page (Web Form) that includes a master page by selecting **Web Form using Master Page**.</span></span> <span data-ttu-id="9358f-230">將新頁面命名*ShoppingCart.aspx*。</span><span class="sxs-lookup"><span data-stu-id="9358f-230">Name the new page *ShoppingCart.aspx*.</span></span>
3. <span data-ttu-id="9358f-231">選取  **Site.Master**附加至新建立的主版頁面 *.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="9358f-231">Select **Site.Master** to attach the master page to the newly created *.aspx* page.</span></span>
4. <span data-ttu-id="9358f-232">在  *ShoppingCart.aspx*頁面上，以下列標記取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="9358f-232">In the *ShoppingCart.aspx* page, replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

<span data-ttu-id="9358f-233">*ShoppingCart.aspx*頁面包含**GridView**控制項，名為`CartList`。</span><span class="sxs-lookup"><span data-stu-id="9358f-233">The *ShoppingCart.aspx* page includes a **GridView** control named `CartList`.</span></span> <span data-ttu-id="9358f-234">這個控制項將從資料庫繫結的購物車資料使用模型繫結**GridView**控制項。</span><span class="sxs-lookup"><span data-stu-id="9358f-234">This control uses model binding to bind the shopping cart data from the database to the **GridView** control.</span></span> <span data-ttu-id="9358f-235">當您設定`ItemType`的屬性**GridView**控制項，資料繫結運算式`Item`位於控制項和控制項的標記會變成強型別。</span><span class="sxs-lookup"><span data-stu-id="9358f-235">When you set the `ItemType` property of the **GridView** control, the data-binding expression `Item` is available in the markup of the control and the control becomes strongly typed.</span></span> <span data-ttu-id="9358f-236">如稍早在本教學課程系列中所述，您可以選取 詳細資料的`Item`物件使用 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="9358f-236">As mentioned earlier in this tutorial series, you can select details of the `Item` object using IntelliSense.</span></span> <span data-ttu-id="9358f-237">若要設定資料控制項來使用模型繫結選取資料，您將`SelectMethod`控制項的屬性。</span><span class="sxs-lookup"><span data-stu-id="9358f-237">To configure a data control to use model binding to select data, you set the `SelectMethod` property of the control.</span></span> <span data-ttu-id="9358f-238">在上述的標記，您可以設定`SelectMethod`使用 GetShoppingCartItems 方法，這個方法會傳回一份`CartItem`物件。</span><span class="sxs-lookup"><span data-stu-id="9358f-238">In the markup above, you set the `SelectMethod` to use the GetShoppingCartItems method which returns a list of `CartItem` objects.</span></span> <span data-ttu-id="9358f-239">**GridView**資料控制項在網頁生命週期中的適當時間呼叫的方法，並自動繫結傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="9358f-239">The **GridView** data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="9358f-240">`GetShoppingCartItems`必須仍會加入方法。</span><span class="sxs-lookup"><span data-stu-id="9358f-240">The `GetShoppingCartItems` method must still be added.</span></span>

#### <a name="retrieving-the-shopping-cart-items"></a><span data-ttu-id="9358f-241">正在擷取購物車項目</span><span class="sxs-lookup"><span data-stu-id="9358f-241">Retrieving the Shopping Cart Items</span></span>

<span data-ttu-id="9358f-242">接下來，您可以加入程式碼*ShoppingCart.aspx.cs*程式碼後置擷取並填入購物車 UI。</span><span class="sxs-lookup"><span data-stu-id="9358f-242">Next, you add code to the *ShoppingCart.aspx.cs* code-behind to retrieve and populate the Shopping Cart UI.</span></span>

1. <span data-ttu-id="9358f-243">在 **方案總管**，以滑鼠右鍵按一下*ShoppingCart.aspx*頁面，然後按一下**檢視程式碼**。</span><span class="sxs-lookup"><span data-stu-id="9358f-243">In **Solution Explorer**, right-click the *ShoppingCart.aspx* page and then click **View Code**.</span></span> <span data-ttu-id="9358f-244">*ShoppingCart.aspx.cs*在編輯器中開啟程式碼後置檔案。</span><span class="sxs-lookup"><span data-stu-id="9358f-244">The *ShoppingCart.aspx.cs* code-behind file is opened in the editor.</span></span>
2. <span data-ttu-id="9358f-245">將現有的程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9358f-245">Replace the existing code with the following:</span></span>  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

<span data-ttu-id="9358f-246">如前文所述，`GridView`資料控制呼叫`GetShoppingCartItems`方法在適當的時間，在頁面生命週期，並自動將傳回的資料繫結。</span><span class="sxs-lookup"><span data-stu-id="9358f-246">As mentioned above, the `GridView` data control calls the `GetShoppingCartItems` method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="9358f-247">`GetShoppingCartItems`方法建立的執行個體`ShoppingCartActions`物件。</span><span class="sxs-lookup"><span data-stu-id="9358f-247">The `GetShoppingCartItems` method creates an instance of the `ShoppingCartActions` object.</span></span> <span data-ttu-id="9358f-248">然後，程式碼會使用該執行個體傳回購物車中的項目，藉由呼叫`GetCartItems`方法。</span><span class="sxs-lookup"><span data-stu-id="9358f-248">Then, the code uses that instance to return the items in the cart by calling the `GetCartItems` method.</span></span>

### <a name="adding-products-to-the-shopping-cart"></a><span data-ttu-id="9358f-249">加入至購物車的產品</span><span class="sxs-lookup"><span data-stu-id="9358f-249">Adding Products to the Shopping Cart</span></span>

<span data-ttu-id="9358f-250">當任一*ProductList.aspx*或*ProductDetails.aspx*頁面顯示時，使用者將能夠將產品加入購物車的連結。</span><span class="sxs-lookup"><span data-stu-id="9358f-250">When either the *ProductList.aspx* or the *ProductDetails.aspx* page is displayed, the user will be able to add the product to the shopping cart using a link.</span></span> <span data-ttu-id="9358f-251">當他們按一下連結時，應用程式瀏覽至名為處理頁面*AddToCart.aspx*。</span><span class="sxs-lookup"><span data-stu-id="9358f-251">When they click the link, the application navigates to the processing page named *AddToCart.aspx*.</span></span> <span data-ttu-id="9358f-252">*AddToCart.aspx*頁面會呼叫`AddToCart`方法中的`ShoppingCart`您稍早在本教學課程中新增的類別。</span><span class="sxs-lookup"><span data-stu-id="9358f-252">The *AddToCart.aspx* page will call the `AddToCart` method in the `ShoppingCart` class that you added earlier in this tutorial.</span></span>

<span data-ttu-id="9358f-253">現在，您可以新增**加入購物車**連結，同時*ProductList.aspx*頁面和*ProductDetails.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="9358f-253">Now, you'll add an **Add to Cart** link to both the *ProductList.aspx* page and the *ProductDetails.aspx* page.</span></span> <span data-ttu-id="9358f-254">此連結會包含產品`ID`，從資料庫擷取。</span><span class="sxs-lookup"><span data-stu-id="9358f-254">This link will include the product `ID` that is retrieved from the database.</span></span>

1. <span data-ttu-id="9358f-255">在 **方案總管**，尋找並開啟名為頁面*ProductList.aspx*。</span><span class="sxs-lookup"><span data-stu-id="9358f-255">In **Solution Explorer**, find and open the page named *ProductList.aspx*.</span></span>
2. <span data-ttu-id="9358f-256">新增以黃色反白顯示的標記*ProductList.aspx*頁面上，使整個頁面出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9358f-256">Add the markup highlighted in yellow to the *ProductList.aspx* page so that the entire page appears as follows:</span></span>  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a><span data-ttu-id="9358f-257">測試購物車</span><span class="sxs-lookup"><span data-stu-id="9358f-257">Testing the Shopping Cart</span></span>

<span data-ttu-id="9358f-258">執行以查看您如何將產品加入購物車應用程式。</span><span class="sxs-lookup"><span data-stu-id="9358f-258">Run the application to see how you add products to the shopping cart.</span></span>

1. <span data-ttu-id="9358f-259">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9358f-259">Press **F5** to run the application.</span></span>  
 <span data-ttu-id="9358f-260">專案重新建立資料庫之後，瀏覽器會開啟並顯示*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="9358f-260">After the project recreates the database, the browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="9358f-261">選取 **汽車**類別瀏覽功能表。</span><span class="sxs-lookup"><span data-stu-id="9358f-261">Select **Cars** from the category navigation menu.</span></span>  
 <span data-ttu-id="9358f-262">*ProductList.aspx*頁面會顯示 [Cars] 分類中包含的產品。</span><span class="sxs-lookup"><span data-stu-id="9358f-262">The *ProductList.aspx* page is displayed showing only products included in the "Cars" category.</span></span> 

    ![「 購物車 」-汽車](shopping-cart/_static/image5.png)
3. <span data-ttu-id="9358f-264">按一下 **加入購物車**第一項產品旁邊的連結列 (轉換 car)。</span><span class="sxs-lookup"><span data-stu-id="9358f-264">Click the **Add to Cart** link next to the first product listed (the convertible car).</span></span>   
 <span data-ttu-id="9358f-265">*ShoppingCart.aspx*顯示頁面，顯示購物車 」 中的選取項目。</span><span class="sxs-lookup"><span data-stu-id="9358f-265">The *ShoppingCart.aspx* page is displayed, showing the selection in your shopping cart.</span></span> 

    ![「 購物車 」-購物車](shopping-cart/_static/image6.png)
4. <span data-ttu-id="9358f-267">選取以檢視其他產品**平面**類別瀏覽功能表。</span><span class="sxs-lookup"><span data-stu-id="9358f-267">View additional products by selecting **Planes** from the category navigation menu.</span></span>
5. <span data-ttu-id="9358f-268">按一下 **加入購物車**旁邊所列的第一個產品的連結。</span><span class="sxs-lookup"><span data-stu-id="9358f-268">Click the **Add to Cart** link next to the first product listed.</span></span>  
 <span data-ttu-id="9358f-269">*ShoppingCart.aspx*頁面會顯示額外的項目。</span><span class="sxs-lookup"><span data-stu-id="9358f-269">The *ShoppingCart.aspx* page is displayed with the additional item.</span></span>
6. <span data-ttu-id="9358f-270">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="9358f-270">Close the browser.</span></span>

### <a name="calculating-and-displaying-the-order-total"></a><span data-ttu-id="9358f-271">計算並顯示訂單總計</span><span class="sxs-lookup"><span data-stu-id="9358f-271">Calculating and Displaying the Order Total</span></span>

<span data-ttu-id="9358f-272">除了產品加入購物車中，您會新增`GetTotal`方法，以`ShoppingCart`類別，並在 [購物車] 頁面中顯示訂單總金額。</span><span class="sxs-lookup"><span data-stu-id="9358f-272">In addition to adding products to the shopping cart, you will add a `GetTotal` method to the `ShoppingCart` class and display the total order amount in the shopping cart page.</span></span>

1. <span data-ttu-id="9358f-273">在 [**方案總管] 中**，開啟*ShoppingCartActions.cs*中的檔案*邏輯*資料夾。</span><span class="sxs-lookup"><span data-stu-id="9358f-273">In **Solution Explorer**, open the *ShoppingCartActions.cs* file in the *Logic* folder.</span></span>
2. <span data-ttu-id="9358f-274">新增下列`GetTotal`方法以黃色反白顯示`ShoppingCart`類別，使類別看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="9358f-274">Add the following `GetTotal` method highlighted in yellow to the `ShoppingCart` class, so that the class appears as follows:</span></span>   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

<span data-ttu-id="9358f-275">首先，`GetTotal`方法取得使用者的購物車識別碼。</span><span class="sxs-lookup"><span data-stu-id="9358f-275">First, the `GetTotal` method gets the ID of the shopping cart for the user.</span></span> <span data-ttu-id="9358f-276">然後方法會取得購物車總計購物車中所列每個產品的產品數量乘以產品價格。</span><span class="sxs-lookup"><span data-stu-id="9358f-276">Then the method gets the cart total by multiplying the product price by the product quantity for each product listed in the cart.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9358f-277">上述程式碼會使用可為 null 的型別 」`int?`"。</span><span class="sxs-lookup"><span data-stu-id="9358f-277">The above code uses the nullable type "`int?`".</span></span> <span data-ttu-id="9358f-278">可為 null 的型別可以代表基礎類型，同時也為 null 值的所有值。</span><span class="sxs-lookup"><span data-stu-id="9358f-278">Nullable types can represent all the values of an underlying type, and also as a null value.</span></span> <span data-ttu-id="9358f-279">如需詳細資訊，請參閱[使用可為 Null 的型別](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx)。</span><span class="sxs-lookup"><span data-stu-id="9358f-279">For more information see, [Using Nullable Types](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).</span></span>


### <a name="modify-the-shopping-cart-display"></a><span data-ttu-id="9358f-280">修改購物車顯示</span><span class="sxs-lookup"><span data-stu-id="9358f-280">Modify the Shopping Cart Display</span></span>

<span data-ttu-id="9358f-281">接下來，您將修改的程式碼*ShoppingCart.aspx*頁面，即可呼叫`GetTotal`方法，以及總的顯示*ShoppingCart.aspx*頁面載入頁面時。</span><span class="sxs-lookup"><span data-stu-id="9358f-281">Next you'll modify the code for the *ShoppingCart.aspx* page to call the `GetTotal` method and display that total on the *ShoppingCart.aspx* page when the page loads.</span></span>

1. <span data-ttu-id="9358f-282">在 **方案總管**，以滑鼠右鍵按一下*ShoppingCart.aspx*頁面，然後選取**檢視程式碼**。</span><span class="sxs-lookup"><span data-stu-id="9358f-282">In **Solution Explorer**, right-click the *ShoppingCart.aspx* page and select **View Code**.</span></span>
2. <span data-ttu-id="9358f-283">在  *ShoppingCart.aspx.cs*檔案中，更新`Page_Load`處理常式，加入下列程式碼以黃色反白顯示：</span><span class="sxs-lookup"><span data-stu-id="9358f-283">In the *ShoppingCart.aspx.cs* file, update the `Page_Load` handler by adding the following code highlighted in yellow:</span></span>   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

<span data-ttu-id="9358f-284">當*ShoppingCart.aspx*頁面載入時，會載入購物車物件並呼叫，以擷取購物車總計`GetTotal`方法`ShoppingCart`類別。</span><span class="sxs-lookup"><span data-stu-id="9358f-284">When the *ShoppingCart.aspx* page loads, it loads the shopping cart object and then retrieves the shopping cart total by calling the `GetTotal` method of the `ShoppingCart` class.</span></span> <span data-ttu-id="9358f-285">購物車是空的如果該效果會顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="9358f-285">If the shopping cart is empty, a message to that effect is displayed.</span></span>

### <a name="testing-the-shopping-cart-total"></a><span data-ttu-id="9358f-286">測試的購物車總計</span><span class="sxs-lookup"><span data-stu-id="9358f-286">Testing the Shopping Cart Total</span></span>

<span data-ttu-id="9358f-287">執行應用程式現在以查看您如何可以不只將產品加入購物車，但您所見的購物車總計。</span><span class="sxs-lookup"><span data-stu-id="9358f-287">Run the application now to see how you can not only add a product to the shopping cart, but you can see the shopping cart total.</span></span>

1. <span data-ttu-id="9358f-288">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9358f-288">Press **F5** to run the application.</span></span>  
 <span data-ttu-id="9358f-289">瀏覽器會開啟並顯示*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="9358f-289">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="9358f-290">選取 **汽車**類別瀏覽功能表。</span><span class="sxs-lookup"><span data-stu-id="9358f-290">Select **Cars** from the category navigation menu.</span></span>
3. <span data-ttu-id="9358f-291">按一下 **加入購物車**第一項產品旁邊的連結。</span><span class="sxs-lookup"><span data-stu-id="9358f-291">Click the **Add To Cart** link next to the first product.</span></span>   
 <span data-ttu-id="9358f-292">*ShoppingCart.aspx*頁面會顯示訂單總計。</span><span class="sxs-lookup"><span data-stu-id="9358f-292">The *ShoppingCart.aspx* page is displayed with the order total.</span></span> 

    ![「 購物車 」-車總計](shopping-cart/_static/image7.png)
4. <span data-ttu-id="9358f-294">加入購物車中的某些其他產品 （例如平面）。</span><span class="sxs-lookup"><span data-stu-id="9358f-294">Add some other products (for example, a plane) to the cart.</span></span>
5. <span data-ttu-id="9358f-295">*ShoppingCart.aspx*頁面會顯示已更新的總計，您已新增的所有產品。</span><span class="sxs-lookup"><span data-stu-id="9358f-295">The *ShoppingCart.aspx* page is displayed with an updated total for all the products you've added.</span></span> 

    ![「 購物車 」-有很多產品](shopping-cart/_static/image8.png)
6. <span data-ttu-id="9358f-297">關閉瀏覽器視窗以停止執行中應用程式。</span><span class="sxs-lookup"><span data-stu-id="9358f-297">Stop the running app by closing the browser window.</span></span>

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a><span data-ttu-id="9358f-298">將更新和簽出 按鈕加入至購物車</span><span class="sxs-lookup"><span data-stu-id="9358f-298">Adding Update and Checkout Buttons to the Shopping Cart</span></span>

<span data-ttu-id="9358f-299">若要允許使用者修改購物車，您將新增**更新** 按鈕並**簽出**購物車 頁面的按鈕。</span><span class="sxs-lookup"><span data-stu-id="9358f-299">To allow the users to modify the shopping cart, you'll add an **Update** button and a **Checkout** button to the shopping cart page.</span></span> <span data-ttu-id="9358f-300">**簽出**按鈕不會等到稍後在本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="9358f-300">The **Checkout** button is not used until later in this tutorial series.</span></span>

1. <span data-ttu-id="9358f-301">在 [**方案總管] 中**，開啟*ShoppingCart.aspx* web 應用程式專案的根目錄中的頁面。</span><span class="sxs-lookup"><span data-stu-id="9358f-301">In **Solution Explorer**, open the *ShoppingCart.aspx* page in the root of the web application project.</span></span>
2. <span data-ttu-id="9358f-302">新增**更新** 按鈕並**簽出**按鈕來*ShoppingCart.aspx*頁面上，新增至現有的標記，黃色反白顯示的標記中所示下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9358f-302">To add the **Update** button and the **Checkout** button to the *ShoppingCart.aspx* page, add the markup highlighted in yellow to the existing markup, as shown in the following code:</span></span>   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

<span data-ttu-id="9358f-303">當使用者按一下**更新** 按鈕，`UpdateBtn_Click`會呼叫事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="9358f-303">When the user clicks the **Update** button, the `UpdateBtn_Click` event handler will be called.</span></span> <span data-ttu-id="9358f-304">這個事件處理常式會呼叫您將在下一個步驟新增的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9358f-304">This event handler will call the code that you'll add in the next step.</span></span>

<span data-ttu-id="9358f-305">接下來，您可以在此更新中所包含的程式碼*ShoppingCart.aspx.cs*檔案，以循環購物車項目並呼叫`RemoveItem`和`UpdateItem`方法。</span><span class="sxs-lookup"><span data-stu-id="9358f-305">Next, you can update the code contained in the *ShoppingCart.aspx.cs* file to loop through the cart items and call the `RemoveItem` and `UpdateItem` methods.</span></span>

1. <span data-ttu-id="9358f-306">在 [**方案總管] 中**，開啟*ShoppingCart.aspx.cs* web 應用程式專案的根目錄中的檔案。</span><span class="sxs-lookup"><span data-stu-id="9358f-306">In **Solution Explorer**, open the *ShoppingCart.aspx.cs* file in the root of the web application project.</span></span>
2. <span data-ttu-id="9358f-307">新增以黃色反白顯示的下列程式碼區段*ShoppingCart.aspx.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="9358f-307">Add the following code sections highlighted in yellow to the *ShoppingCart.aspx.cs* file:</span></span>   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

<span data-ttu-id="9358f-308">當使用者按一下**更新**按鈕*ShoppingCart.aspx*  頁面上，會呼叫 UpdateCartItems 方法。</span><span class="sxs-lookup"><span data-stu-id="9358f-308">When the user clicks the **Update** button on the *ShoppingCart.aspx* page, the UpdateCartItems method is called.</span></span> <span data-ttu-id="9358f-309">UpdateCartItems 方法會取得在購物車中的每個項目之更新的值。</span><span class="sxs-lookup"><span data-stu-id="9358f-309">The UpdateCartItems method gets the updated values for each item in the shopping cart.</span></span> <span data-ttu-id="9358f-310">接著，UpdateCartItems 方法會呼叫`UpdateShoppingCartDatabase`方法 （新增和下一個步驟中所述） 以新增或移除購物車中的項目。</span><span class="sxs-lookup"><span data-stu-id="9358f-310">Then, the UpdateCartItems method calls the `UpdateShoppingCartDatabase` method (added and explained in the next step) to either add or remove items from the shopping cart.</span></span> <span data-ttu-id="9358f-311">一旦資料庫更新以反映放入購物車中，更新**GridView**控制項，會藉由呼叫更新購物車 頁面上`DataBind`方法**GridView**。</span><span class="sxs-lookup"><span data-stu-id="9358f-311">Once the database has been updated to reflect the updates to the shopping cart, the **GridView** control is updated on the shopping cart page by calling the `DataBind` method for the **GridView**.</span></span> <span data-ttu-id="9358f-312">此外，在購物車 頁面上的訂單總金額會更新以反映更新的項目清單。</span><span class="sxs-lookup"><span data-stu-id="9358f-312">Also, the total order amount on the shopping cart page is updated to reflect the updated list of items.</span></span>

### <a name="updating-and-removing-shopping-cart-items"></a><span data-ttu-id="9358f-313">更新與移除購物車項目</span><span class="sxs-lookup"><span data-stu-id="9358f-313">Updating and Removing Shopping Cart Items</span></span>

<span data-ttu-id="9358f-314">在 [ *ShoppingCart.aspx* ] 頁面上，您可以看到更新的項目數量，以及移除項目已加入的控制項。</span><span class="sxs-lookup"><span data-stu-id="9358f-314">On the *ShoppingCart.aspx* page, you can see controls have been added for updating the quantity of an item and removing an item.</span></span> <span data-ttu-id="9358f-315">現在，加入將使用這些控制項的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9358f-315">Now, add the code that will make these controls work.</span></span>

1. <span data-ttu-id="9358f-316">在 [**方案總管] 中**，開啟*ShoppingCartActions.cs*中的檔案*邏輯*資料夾。</span><span class="sxs-lookup"><span data-stu-id="9358f-316">In **Solution Explorer**, open the *ShoppingCartActions.cs* file in the *Logic* folder.</span></span>
2. <span data-ttu-id="9358f-317">新增下列程式碼中以黃色反白顯示*ShoppingCartActions.cs*類別檔案：</span><span class="sxs-lookup"><span data-stu-id="9358f-317">Add the following code highlighted in yellow to the *ShoppingCartActions.cs* class file:</span></span>   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

<span data-ttu-id="9358f-318">`UpdateShoppingCartDatabase`方法，從呼叫`UpdateCartItems`方法*ShoppingCart.aspx.cs*頁面上，包含邏輯，可更新或移除購物車中的項目。</span><span class="sxs-lookup"><span data-stu-id="9358f-318">The `UpdateShoppingCartDatabase` method, called from the `UpdateCartItems` method on the *ShoppingCart.aspx.cs* page, contains the logic to either update or remove items from the shopping cart.</span></span> <span data-ttu-id="9358f-319">`UpdateShoppingCartDatabase`方法逐一查看購物車 」 清單中的所有資料列。</span><span class="sxs-lookup"><span data-stu-id="9358f-319">The `UpdateShoppingCartDatabase` method iterates through all the rows within the shopping cart list.</span></span> <span data-ttu-id="9358f-320">如果要移除已標示的購物車項目或數量小於一，`RemoveItem`呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="9358f-320">If a shopping cart item has been marked to be removed, or the quantity is less than one, the `RemoveItem` method is called.</span></span> <span data-ttu-id="9358f-321">否則，購物車項目會檢查更新時`UpdateItem`呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="9358f-321">Otherwise, the shopping cart item is checked for updates when the `UpdateItem` method is called.</span></span> <span data-ttu-id="9358f-322">已移除或更新購物車項目之後，資料庫會儲存變更。</span><span class="sxs-lookup"><span data-stu-id="9358f-322">After the shopping cart item has been removed or updated, the database changes are saved.</span></span>

<span data-ttu-id="9358f-323">`ShoppingCartUpdates`結構用來保存所有的購物車項目。</span><span class="sxs-lookup"><span data-stu-id="9358f-323">The `ShoppingCartUpdates` structure is used to hold all the shopping cart items.</span></span> <span data-ttu-id="9358f-324">`UpdateShoppingCartDatabase`方法會使用`ShoppingCartUpdates`結構，以判斷任何項目，是要更新或移除。</span><span class="sxs-lookup"><span data-stu-id="9358f-324">The `UpdateShoppingCartDatabase` method uses the `ShoppingCartUpdates` structure to determine if any of the items need to be updated or removed.</span></span>

<span data-ttu-id="9358f-325">在下一個教學課程中，您將使用`EmptyCart`方法，以清除 購物車後購買產品。</span><span class="sxs-lookup"><span data-stu-id="9358f-325">In the next tutorial, you will use the `EmptyCart` method to clear the shopping cart after purchasing products.</span></span> <span data-ttu-id="9358f-326">但現在，您將使用`GetCount`剛加入的方法*ShoppingCartActions.cs*檔案，以判斷購物車中有多少項目。</span><span class="sxs-lookup"><span data-stu-id="9358f-326">But for now, you will use the `GetCount` method that you just added to the *ShoppingCartActions.cs* file to determine how many items are in the shopping cart.</span></span>

### <a name="adding-a-shopping-cart-counter"></a><span data-ttu-id="9358f-327">加入購物車計數器</span><span class="sxs-lookup"><span data-stu-id="9358f-327">Adding a Shopping Cart Counter</span></span>

<span data-ttu-id="9358f-328">若要允許使用者檢視購物車中的項目總數，將會加入至計數器*Site.Master*頁面。</span><span class="sxs-lookup"><span data-stu-id="9358f-328">To allow the user to view the total number of items in the shopping cart, you will add a counter to the *Site.Master* page.</span></span> <span data-ttu-id="9358f-329">這個計數器也將當成放入購物車的連結。</span><span class="sxs-lookup"><span data-stu-id="9358f-329">This counter will also act as a link to the shopping cart.</span></span>

1. <span data-ttu-id="9358f-330">在 **方案總管**，開啟*Site.Master*頁面。</span><span class="sxs-lookup"><span data-stu-id="9358f-330">In **Solution Explorer**, open the *Site.Master* page.</span></span>
2. <span data-ttu-id="9358f-331">修改標記加入購物車計數器連結，如黃色的導覽 」 一節中所示，使其出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9358f-331">Modify the markup by adding the shopping cart counter link as shown in yellow to the navigation section so it appears as follows:</span></span>  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. <span data-ttu-id="9358f-332">接下來，更新的程式碼後置*Site.Master.cs*檔案加上黃色反白顯示，如下所示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="9358f-332">Next, update the code-behind of the *Site.Master.cs* file by adding the code highlighted in yellow as follows:</span></span>  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

<span data-ttu-id="9358f-333">頁面會轉譯為 HTML 之前,`Page_PreRender`就會引發事件。</span><span class="sxs-lookup"><span data-stu-id="9358f-333">Before the page is rendered as HTML, the `Page_PreRender` event is raised.</span></span> <span data-ttu-id="9358f-334">在 `Page_PreRender`處理常式，購物車的總數取決於呼叫`GetCount`方法。</span><span class="sxs-lookup"><span data-stu-id="9358f-334">In the `Page_PreRender` handler, the total count of the shopping cart is determined by calling the `GetCount` method.</span></span> <span data-ttu-id="9358f-335">傳回的值加入至`cartCount`標記中包含的範圍*Site.Master*頁面。</span><span class="sxs-lookup"><span data-stu-id="9358f-335">The returned value is added to the `cartCount` span included in the markup of the *Site.Master* page.</span></span> <span data-ttu-id="9358f-336">`<span>`標記可讓內部的項目正確呈現。</span><span class="sxs-lookup"><span data-stu-id="9358f-336">The `<span>` tags enables the inner elements to be properly rendered.</span></span> <span data-ttu-id="9358f-337">當站台的任何頁面顯示時，將會顯示購物車總計。</span><span class="sxs-lookup"><span data-stu-id="9358f-337">When any page of the site is displayed, the shopping cart total will be displayed.</span></span> <span data-ttu-id="9358f-338">使用者也可以按一下以顯示購物車的購物車總計。</span><span class="sxs-lookup"><span data-stu-id="9358f-338">The user can also click the shopping cart total to display the shopping cart.</span></span>

## <a name="testing-the-completed-shopping-cart"></a><span data-ttu-id="9358f-339">測試已完成的購物車</span><span class="sxs-lookup"><span data-stu-id="9358f-339">Testing the Completed Shopping Cart</span></span>

<span data-ttu-id="9358f-340">購物車 」 中，您可以執行的應用程式現在以查看您可以新增、 刪除和更新項目。</span><span class="sxs-lookup"><span data-stu-id="9358f-340">You can run the application now to see how you can add, delete, and update items in the shopping cart.</span></span> <span data-ttu-id="9358f-341">購物車總計將會反映在購物車中的所有項目的總成本。</span><span class="sxs-lookup"><span data-stu-id="9358f-341">The shopping cart total will reflect the total cost of all items in the shopping cart.</span></span>

1. <span data-ttu-id="9358f-342">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9358f-342">Press **F5** to run the application.</span></span>  
 <span data-ttu-id="9358f-343">瀏覽器隨即開啟並顯示*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="9358f-343">The browser opens and shows the *Default.aspx* page.</span></span>
2. <span data-ttu-id="9358f-344">選取 **汽車**類別瀏覽功能表。</span><span class="sxs-lookup"><span data-stu-id="9358f-344">Select **Cars** from the category navigation menu.</span></span>
3. <span data-ttu-id="9358f-345">按一下 **加入購物車**第一項產品旁邊的連結。</span><span class="sxs-lookup"><span data-stu-id="9358f-345">Click the **Add To Cart** link next to the first product.</span></span>   
 <span data-ttu-id="9358f-346">*ShoppingCart.aspx*頁面會顯示訂單總計。</span><span class="sxs-lookup"><span data-stu-id="9358f-346">The *ShoppingCart.aspx* page is displayed with the order total.</span></span>
4. <span data-ttu-id="9358f-347">選取 **平面**類別瀏覽功能表。</span><span class="sxs-lookup"><span data-stu-id="9358f-347">Select **Planes** from the category navigation menu.</span></span>
5. <span data-ttu-id="9358f-348">按一下 **加入購物車**第一項產品旁邊的連結。</span><span class="sxs-lookup"><span data-stu-id="9358f-348">Click the **Add To Cart** link next to the first product.</span></span>
6. <span data-ttu-id="9358f-349">設定為 3 的購物車中的第一個項目的數量，然後選取**移除的項目**第二個項目的核取方塊。<a id="a"></a></span><span class="sxs-lookup"><span data-stu-id="9358f-349">Set the quantity of the first item in the shopping cart to 3 and select the **Remove Item** check box of the second item.<a id="a"></a></span></span>
7. <span data-ttu-id="9358f-350">按一下 [**更新**按鈕以更新購物車] 頁面，並顯示新的訂單總數。</span><span class="sxs-lookup"><span data-stu-id="9358f-350">Click the **Update** button to update the shopping cart page and display the new order total.</span></span> 

    ![購物車-車更新](shopping-cart/_static/image9.png)

## <a name="summary"></a><span data-ttu-id="9358f-352">總結</span><span class="sxs-lookup"><span data-stu-id="9358f-352">Summary</span></span>

<span data-ttu-id="9358f-353">在本教學課程中，您已建立購物車 Wingtip Toys Web Form 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="9358f-353">In this tutorial, you have created a shopping cart for the Wingtip Toys Web Forms sample application.</span></span> <span data-ttu-id="9358f-354">在本教學課程中，您已使用 Entity Framework Code First 資料註解、 強型別的資料控制項，模型繫結。</span><span class="sxs-lookup"><span data-stu-id="9358f-354">During this tutorial you have used Entity Framework Code First, data annotations, strongly typed data controls, and model binding.</span></span>

<span data-ttu-id="9358f-355">購物車支援加入、 刪除和更新的使用者已選擇要購買的項目。</span><span class="sxs-lookup"><span data-stu-id="9358f-355">The shopping cart supports adding, deleting, and updating items that the user has selected for purchase.</span></span> <span data-ttu-id="9358f-356">除了實作的購物車 」 功能，您已了解如何顯示在 購物車項目的**GridView**控制，並計算訂單總計。</span><span class="sxs-lookup"><span data-stu-id="9358f-356">In addition to implementing the shopping cart functionality, you have learned how to display shopping cart items in a **GridView** control and calculate the order total.</span></span>

## <a name="addition-information"></a><span data-ttu-id="9358f-357">其他資訊</span><span class="sxs-lookup"><span data-stu-id="9358f-357">Addition Information</span></span>

[<span data-ttu-id="9358f-358">ASP.NET 工作階段狀態概觀</span><span class="sxs-lookup"><span data-stu-id="9358f-358">ASP.NET Session State Overview</span></span>](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> <span data-ttu-id="9358f-359">[上一頁](display_data_items_and_details.md)
> [下一頁](checkout-and-payment-with-paypal.md)</span><span class="sxs-lookup"><span data-stu-id="9358f-359">[Previous](display_data_items_and_details.md)
[Next](checkout-and-payment-with-paypal.md)</span></span>
