---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 第 8 節： 購物車與 Ajax 更新 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 8 節涵蓋了購物車與 Ajax 更新。
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 881d47b5b4df5a4d310a1b3a7eec6ee97b0d42ea
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823835"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="36740-104">第 8 節： 購物車與 Ajax 更新</span><span class="sxs-lookup"><span data-stu-id="36740-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="36740-105">藉由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="36740-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="36740-106">MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="36740-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="36740-107">MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。</span><span class="sxs-lookup"><span data-stu-id="36740-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="36740-108">本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="36740-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="36740-109">第 8 節涵蓋了購物車與 Ajax 更新。</span><span class="sxs-lookup"><span data-stu-id="36740-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="36740-110">我們要允許使用者將專輯購物車中，而不註冊，但他們必須註冊為完成簽出的來賓。</span><span class="sxs-lookup"><span data-stu-id="36740-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="36740-111">將購物和簽出程序會分成兩個控制站： 允許以匿名方式將項目加入購物車，ShoppingCart 控制器和簽出控制器用來處理結帳程序。</span><span class="sxs-lookup"><span data-stu-id="36740-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="36740-112">我們將開始在此區段中，「 購物車 」，並建置下一節中的簽出程序。</span><span class="sxs-lookup"><span data-stu-id="36740-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="36740-113">加入購物車、 訂單及 OrderDetail 的模型類別</span><span class="sxs-lookup"><span data-stu-id="36740-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="36740-114">我們的購物車及結帳程序將使用的一些新的類別。</span><span class="sxs-lookup"><span data-stu-id="36740-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="36740-115">以滑鼠右鍵按一下 [模型] 資料夾，並加入購物車類別 (Cart.cs) 為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="36740-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="36740-116">這個類別是非常類似於我們到目前為止，使用 [RecordId] 屬性的 [金鑰] 屬性除外的其他項目。</span><span class="sxs-lookup"><span data-stu-id="36740-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="36740-117">我們的購物車項目會具有名為允許匿名購物，CartID 的字串識別項，但資料表包含名為記錄識別碼的整數主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="36740-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="36740-118">依照慣例，Entity Framework Code First 所預期，名為購物車的資料表的主索引鍵會 CartId 或識別碼，但我們可以輕鬆地覆寫，透過註解] 或 [程式碼如果我們想。</span><span class="sxs-lookup"><span data-stu-id="36740-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="36740-119">這是我們要如何使用簡單的慣例在 Entity Framework Code First 時他們符合我們的範例，但我們正在不受限於它們時沒有。</span><span class="sxs-lookup"><span data-stu-id="36740-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="36740-120">接下來，新增下列程式碼的 Order 類別 (Order.cs)。</span><span class="sxs-lookup"><span data-stu-id="36740-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="36740-121">此類別會追蹤訂單的摘要和傳遞資訊。</span><span class="sxs-lookup"><span data-stu-id="36740-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="36740-122">**它不會在尚未編譯**，因為它有相依於類別，我們尚未建立 OrderDetails 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="36740-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="36740-123">讓我們修正問題，現在將類別命名為 OrderDetail.cs，新增下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="36740-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="36740-124">我們要將一個最後的更新對我們 MusicStoreEntities 類別，以包含它們會公開這些新的模型類別，也包括 DbSet DbSets&lt;演出者&gt;。</span><span class="sxs-lookup"><span data-stu-id="36740-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="36740-125">更新的 MusicStoreEntities 類別會顯示為下面。</span><span class="sxs-lookup"><span data-stu-id="36740-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="36740-126">管理 「 購物車 」 的商務邏輯</span><span class="sxs-lookup"><span data-stu-id="36740-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="36740-127">接下來，我們將建立 ShoppingCart 類別 [Models] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="36740-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="36740-128">ShoppingCart 模型處理購物車資料表的資料存取。</span><span class="sxs-lookup"><span data-stu-id="36740-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="36740-129">此外，它會處理的商務邏輯來新增和移除購物車中的項目。</span><span class="sxs-lookup"><span data-stu-id="36740-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="36740-130">因為我們不想要要求使用者登入帳戶，只是為了將項目加入購物車，我們會將使用者指派暫時的唯一識別碼 （使用 GUID 或全域唯一識別碼） 時存取購物車。</span><span class="sxs-lookup"><span data-stu-id="36740-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="36740-131">我們將會儲存這個使用 ASP.NET 工作階段類別的識別碼。</span><span class="sxs-lookup"><span data-stu-id="36740-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="36740-132">*注意： ASP.NET 工作階段是方便的地方來儲存使用者專屬資訊到期後他們離開網站。雖然不當使用工作階段狀態可以有較大的網站上的效能影響，我們淺使用適用於示範用途。*</span><span class="sxs-lookup"><span data-stu-id="36740-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="36740-133">ShoppingCart 類別會公開下列方法：</span><span class="sxs-lookup"><span data-stu-id="36740-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="36740-134">**AddToCart**會專輯做為參數，並將它新增至使用者的購物車。</span><span class="sxs-lookup"><span data-stu-id="36740-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="36740-135">購物車資料表會追蹤每一個 album 的數量，因為它會包含邏輯，以視需要建立新的資料列，或只是遞增的數量，如果使用者已排序過的相簿的一個複本。</span><span class="sxs-lookup"><span data-stu-id="36740-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="36740-136">**RemoveFromCart**會專輯識別碼，並移除使用者的購物車。</span><span class="sxs-lookup"><span data-stu-id="36740-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="36740-137">如果使用者只會有一份專輯購物車中，會移除資料列。</span><span class="sxs-lookup"><span data-stu-id="36740-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="36740-138">**EmptyCart**移除使用者的購物車中的所有項目。</span><span class="sxs-lookup"><span data-stu-id="36740-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="36740-139">**GetCartItems**擷取一份 CartItems 顯示或處理。</span><span class="sxs-lookup"><span data-stu-id="36740-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="36740-140">**GetCount**擷取的使用者具有客戶購物車中的專輯總數。</span><span class="sxs-lookup"><span data-stu-id="36740-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="36740-141">**GetTotal**計算總成本的購物車中的所有項目。</span><span class="sxs-lookup"><span data-stu-id="36740-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="36740-142">**CreateOrder**簽出階段期間會將訂單購物車。</span><span class="sxs-lookup"><span data-stu-id="36740-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="36740-143">**GetCart**是可讓我們來取得購物車物件的控制站的靜態方法。</span><span class="sxs-lookup"><span data-stu-id="36740-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="36740-144">它會使用**GetCartId**方法以處理 CartId 讀取使用者的工作階段。</span><span class="sxs-lookup"><span data-stu-id="36740-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="36740-145">GetCartId 方法需要 HttpContextBase，以便它可以讀取使用者的 CartId 從使用者的工作階段。</span><span class="sxs-lookup"><span data-stu-id="36740-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="36740-146">以下是完整**ShoppingCart 類別**:</span><span class="sxs-lookup"><span data-stu-id="36740-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="36740-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="36740-147">ViewModels</span></span>

<span data-ttu-id="36740-148">購物車控制器需要一些複雜資訊傳達給它的檢視不會完全對應到我們的模型物件。</span><span class="sxs-lookup"><span data-stu-id="36740-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="36740-149">我們不想要修改以符合我們的檢視; 我們模型模型類別應該代表我們的網域，使用者介面。</span><span class="sxs-lookup"><span data-stu-id="36740-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="36740-150">一個解決方案是將資訊傳遞至我們的檢視使用 ViewBag 類別，我們使用存放區管理員下拉式清單中的資訊，但透過 ViewBag 傳遞大量資訊取得難以管理。</span><span class="sxs-lookup"><span data-stu-id="36740-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="36740-151">此解決方案是使用*ViewModel*模式。</span><span class="sxs-lookup"><span data-stu-id="36740-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="36740-152">使用此模式時我們會建立強型別的類別，適用於我們的特定檢視案例，以及其公開屬性，動態值/內容所需的我們的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="36740-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="36740-153">然後填入並將這些檢視最佳化的類別傳遞至我們的檢視範本，若要使用我們的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="36740-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="36740-154">這可讓型別安全、 編譯時間檢查，以及檢視範本中的編輯器 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="36740-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="36740-155">我們將建立兩個檢視的模型，以供在我們的購物車控制器： ShoppingCartViewModel 會保留使用者的購物車，內容和 ShoppingCartRemoveViewModel 會用來顯示確認資訊，當使用者移除的項目從購物車。</span><span class="sxs-lookup"><span data-stu-id="36740-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="36740-156">我們為了簡化組織的專案根目錄中，讓我們建立一個新的 Viewmodel 資料夾。</span><span class="sxs-lookup"><span data-stu-id="36740-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="36740-157">以滑鼠右鍵按一下專案，選取 新增 / 新的資料夾。</span><span class="sxs-lookup"><span data-stu-id="36740-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="36740-158">將資料夾命名為 Viewmodel。</span><span class="sxs-lookup"><span data-stu-id="36740-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="36740-159">接下來，新增 ShoppingCartViewModel 類別 Viewmodel 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="36740-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="36740-160">它有兩個屬性： 購物車項目和保存的所有項目總價購物車中的十進位值的清單。</span><span class="sxs-lookup"><span data-stu-id="36740-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="36740-161">現在加入 ShoppingCartRemoveViewModel Viewmodel 資料夾中，使用下列四個屬性。</span><span class="sxs-lookup"><span data-stu-id="36740-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="36740-162">購物車控制器</span><span class="sxs-lookup"><span data-stu-id="36740-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="36740-163">「 購物車 」 控制器有三個主要用途： 項目加入購物車移除購物車中的項目，檢視購物車中的項目。</span><span class="sxs-lookup"><span data-stu-id="36740-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="36740-164">它會利用三個類別我們剛建立： ShoppingCartViewModel、 ShoppingCartRemoveViewModel 和 ShoppingCart。</span><span class="sxs-lookup"><span data-stu-id="36740-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="36740-165">例如，StoreController 和 StoreManagerController，我們將新增欄位以保留 MusicStoreEntities 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="36740-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="36740-166">使用空白控制器範本的專案中加入新的 「 購物車 」 控制站。</span><span class="sxs-lookup"><span data-stu-id="36740-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="36740-167">以下是完成 ShoppingCart 控制器。</span><span class="sxs-lookup"><span data-stu-id="36740-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="36740-168">索引 和 新增控制器動作看起來應該很熟悉。</span><span class="sxs-lookup"><span data-stu-id="36740-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="36740-169">移除和 CartSummary 控制器的動作會處理兩個特殊案例，我們將在下一節中討論。</span><span class="sxs-lookup"><span data-stu-id="36740-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="36740-170">與 jQuery Ajax 更新</span><span class="sxs-lookup"><span data-stu-id="36740-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="36740-171">接下來，我們將建立的購物車索引頁面 ShoppingCartViewModel 強型別，並使用使用之前的相同方法的清單檢視範本。</span><span class="sxs-lookup"><span data-stu-id="36740-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="36740-172">不過，而不是使用 Html.ActionLink 移除購物車中的項目，我們將使用 jQuery 來 「 連接 」 在此檢視中的所有連結具有 HTML 類別 RemoveLink 的 click 事件。</span><span class="sxs-lookup"><span data-stu-id="36740-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="36740-173">而不是張貼的表單，此 click 事件處理常式就只會使 AJAX 回呼至我們 RemoveFromCart 控制器動作。</span><span class="sxs-lookup"><span data-stu-id="36740-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="36740-174">RemoveFromCart 傳回 JSON 序列化的結果，我們 jQuery 回呼然後剖析，並執行四個頁面使用 jQuery 的快速更新：</span><span class="sxs-lookup"><span data-stu-id="36740-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="36740-175">從清單中移除已刪除的相簿</span><span class="sxs-lookup"><span data-stu-id="36740-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="36740-176">更新標頭中的購物車計數</span><span class="sxs-lookup"><span data-stu-id="36740-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="36740-177">向使用者顯示更新訊息</span><span class="sxs-lookup"><span data-stu-id="36740-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="36740-178">更新的購物車 」 總價格</span><span class="sxs-lookup"><span data-stu-id="36740-178">Updates the cart total price</span></span>

<span data-ttu-id="36740-179">因為移除案例都會由 Ajax 回呼，[索引] 檢視中，我們不需要額外的檢視 RemoveFromCart 動作。</span><span class="sxs-lookup"><span data-stu-id="36740-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="36740-180">以下是 /ShoppingCart/Index 檢視的完整程式碼：</span><span class="sxs-lookup"><span data-stu-id="36740-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="36740-181">若要測試時，我們需要能夠加入我們的購物車中的項目。</span><span class="sxs-lookup"><span data-stu-id="36740-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="36740-182">我們會更新我們**存放區詳細資料**檢視中包含 [新增至購物車] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="36740-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="36740-183">雖然我們注意到這個問題，我們可以包含一些我們已新增的專輯額外資訊自我們上次更新此檢視： 內容類型、 演出者、 價格和專輯封面。</span><span class="sxs-lookup"><span data-stu-id="36740-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="36740-184">更新的存放區詳細資料檢視程式碼會出現，如下所示。</span><span class="sxs-lookup"><span data-stu-id="36740-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="36740-185">現在我們可以按一下 透過市集，並測試新增和移除專輯，與我們的購物車。</span><span class="sxs-lookup"><span data-stu-id="36740-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="36740-186">執行應用程式，並瀏覽至存放區索引。</span><span class="sxs-lookup"><span data-stu-id="36740-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="36740-187">接下來，按一下以檢視相簿一份內容類型。</span><span class="sxs-lookup"><span data-stu-id="36740-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="36740-188">現在按一下專輯標題會顯示我們已更新的專輯詳細資料檢視，包括 [新增至購物車] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="36740-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="36740-189">按一下 [新增至購物車] 按鈕會顯示我們的購物車索引檢視與購物車摘要清單。</span><span class="sxs-lookup"><span data-stu-id="36740-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="36740-190">之後載入 「 購物車 」，您可以按一下從購物車連結移除，以查看您的購物車的 Ajax 更新。</span><span class="sxs-lookup"><span data-stu-id="36740-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="36740-191">我們建置出運作中的 「 購物車 」 可讓未註冊的使用者將項目新增至購物車。</span><span class="sxs-lookup"><span data-stu-id="36740-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="36740-192">在下列區段中，我們將允許其註冊，並完成結帳程序。</span><span class="sxs-lookup"><span data-stu-id="36740-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="36740-193">[上一頁](mvc-music-store-part-7.md)
> [下一頁](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="36740-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
