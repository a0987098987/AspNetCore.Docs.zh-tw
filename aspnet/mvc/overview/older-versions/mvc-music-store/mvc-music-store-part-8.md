---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 第 8 部分： 購物車 Ajax 更新 |Microsoft 文件
author: jongalloway
description: 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 8 部涵蓋使用 Ajax 更新購物車。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 195c01ff0d71b2bfd0c00e71244d47a166330921
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871281"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="88831-104">第 8 部： 與 Ajax 更新購物車</span><span class="sxs-lookup"><span data-stu-id="88831-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="88831-105">由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="88831-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="88831-106">MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="88831-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="88831-107">MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。</span><span class="sxs-lookup"><span data-stu-id="88831-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="88831-108">此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="88831-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="88831-109">第 8 部涵蓋使用 Ajax 更新購物車。</span><span class="sxs-lookup"><span data-stu-id="88831-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="88831-110">我們會讓使用者能夠將購物車中的專輯未註冊，但他們需要註冊為來賓完成簽出。</span><span class="sxs-lookup"><span data-stu-id="88831-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="88831-111">在購物與簽出程序會分成兩個控制站： ShoppingCart 控制器以允許以匿名方式將項目新增至購物車和簽出控制器用來處理在結帳程序。</span><span class="sxs-lookup"><span data-stu-id="88831-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="88831-112">我們將這一節，購物車的開頭，然後建置在結帳程序下, 一節。</span><span class="sxs-lookup"><span data-stu-id="88831-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="88831-113">加入購物車、 順序和 OrderDetail 的模型類別</span><span class="sxs-lookup"><span data-stu-id="88831-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="88831-114">我們的購物車及結帳程序將使用的一些新的類別。</span><span class="sxs-lookup"><span data-stu-id="88831-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="88831-115">以滑鼠右鍵按一下 [模型] 資料夾，並加入下列程式碼的購物車類別 (Cart.cs)。</span><span class="sxs-lookup"><span data-stu-id="88831-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="88831-116">這個類別是我們目前為止，使用 [RecordId] 屬性 [Key] 屬性外的其他項目非常類似。</span><span class="sxs-lookup"><span data-stu-id="88831-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="88831-117">我們的購物車項目將具有名為 允許匿名購物 CartID 字串識別項，但資料表包含名為 RecordId 的主索引鍵的整數。</span><span class="sxs-lookup"><span data-stu-id="88831-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="88831-118">依照慣例，名為購物車的資料表的主索引鍵將會為 CartId 或識別碼，但我們可以輕鬆地覆寫的註解或程式碼透過如果我們想要必須要有 Entity Framework 程式碼優先。</span><span class="sxs-lookup"><span data-stu-id="88831-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="88831-119">這是我們要如何使用簡單的慣例在 Entity Framework 程式碼優先當它們符合我們的範例，但我們正在不受限於它們時不會。</span><span class="sxs-lookup"><span data-stu-id="88831-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="88831-120">接下來，加入下列程式碼的 Order 類別 (Order.cs)。</span><span class="sxs-lookup"><span data-stu-id="88831-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="88831-121">此類別會追蹤訂單的摘要和傳遞資訊。</span><span class="sxs-lookup"><span data-stu-id="88831-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="88831-122">**它不會在尚未編譯**，因為它相依於尚未建立我們類別 OrderDetails 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="88831-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="88831-123">讓我們來修正問題，現在將類別命名為 OrderDetail.cs，加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="88831-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="88831-124">我們要將一個上次更新對我們 MusicStoreEntities 類別，以包含會將公開這些新的模型類別，也包括 DbSet DbSets&lt;演出者&gt;。</span><span class="sxs-lookup"><span data-stu-id="88831-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="88831-125">更新的 MusicStoreEntities 類別會顯示為下面。</span><span class="sxs-lookup"><span data-stu-id="88831-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="88831-126">管理購物車商務邏輯</span><span class="sxs-lookup"><span data-stu-id="88831-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="88831-127">接下來，我們將建立 ShoppingCart 類別 Models 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="88831-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="88831-128">ShoppingCart 模型處理購物車資料表資料存取。</span><span class="sxs-lookup"><span data-stu-id="88831-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="88831-129">此外，它會處理商務邏輯來新增和移除購物車的項目。</span><span class="sxs-lookup"><span data-stu-id="88831-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="88831-130">因為我們不想要求使用者登入帳戶，只是為了將項目新增至購物車，我們將會指派使用者的暫存的唯一識別碼 （使用 GUID 或全域唯一識別碼） 當他們存取購物車。</span><span class="sxs-lookup"><span data-stu-id="88831-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="88831-131">我們會將儲存這個識別碼使用 ASP.NET 工作階段類別。</span><span class="sxs-lookup"><span data-stu-id="88831-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="88831-132">*注意： ASP.NET 工作階段是方便的位置來儲存使用者特定的資訊將到期後他們離開網站。不當使用的工作階段狀態可能會影響效能較大的站台上，而淺使用將適用於示範用途。*</span><span class="sxs-lookup"><span data-stu-id="88831-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="88831-133">ShoppingCart 類別會公開下列方法：</span><span class="sxs-lookup"><span data-stu-id="88831-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="88831-134">**AddToCart**採用相簿做為參數，並將它加入至使用者的購物車。</span><span class="sxs-lookup"><span data-stu-id="88831-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="88831-135">購物車資料表會追蹤每個相簿的數量，因為它包含邏輯，以視需要建立新的資料列，或如果使用者已排序過的相簿的一個複本，只要遞增數量。</span><span class="sxs-lookup"><span data-stu-id="88831-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="88831-136">**RemoveFromCart**採用專輯識別碼，並將它從使用者的購物車移除。</span><span class="sxs-lookup"><span data-stu-id="88831-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="88831-137">如果使用者只會有一份專輯購物車中，會移除資料列。</span><span class="sxs-lookup"><span data-stu-id="88831-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="88831-138">**EmptyCart**移除使用者的購物車中的所有項目。</span><span class="sxs-lookup"><span data-stu-id="88831-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="88831-139">**GetCartItems**擷取 CartItems 清單來顯示或處理。</span><span class="sxs-lookup"><span data-stu-id="88831-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="88831-140">**GetCount**擷取的使用者具有及其購物車中的專輯總數。</span><span class="sxs-lookup"><span data-stu-id="88831-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="88831-141">**GetTotal**計算總成本的購物車中的所有項目。</span><span class="sxs-lookup"><span data-stu-id="88831-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="88831-142">**CreateOrder**簽出階段期間會將訂單購物車。</span><span class="sxs-lookup"><span data-stu-id="88831-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="88831-143">**GetCart**是靜態方法可讓我們來取得購物車物件的控制站。</span><span class="sxs-lookup"><span data-stu-id="88831-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="88831-144">它會使用**GetCartId**方法以處理 CartId 讀取使用者的工作階段。</span><span class="sxs-lookup"><span data-stu-id="88831-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="88831-145">GetCartId 方法需要 HttpContextBase，使它能夠讀取使用者的 CartId 從使用者的工作階段。</span><span class="sxs-lookup"><span data-stu-id="88831-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="88831-146">以下是完整**ShoppingCart 類別**:</span><span class="sxs-lookup"><span data-stu-id="88831-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="88831-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="88831-147">ViewModels</span></span>

<span data-ttu-id="88831-148">我們的購物車控制器需要一些複雜資訊傳達給它的檢視不會清楚對應到我們的模型物件。</span><span class="sxs-lookup"><span data-stu-id="88831-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="88831-149">我們不想修改以符合我們的檢視; 我們模型模型類別應該代表我們的網域使用者介面。</span><span class="sxs-lookup"><span data-stu-id="88831-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="88831-150">一個解決方式是將資訊傳遞至我們使用 ViewBag 類別，如我們所做的存放區管理員下拉式清單中的資訊，但傳遞的大量資訊透過 ViewBag 取得難以管理的檢視。</span><span class="sxs-lookup"><span data-stu-id="88831-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="88831-151">對此的解決方案是使用*ViewModel*模式。</span><span class="sxs-lookup"><span data-stu-id="88831-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="88831-152">使用此模式時我們建立強型別的類別，最佳化的特定檢視案例中，以及其公開之動態值/內容我們檢視範本所需的屬性。</span><span class="sxs-lookup"><span data-stu-id="88831-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="88831-153">我們控制器類別可以填入並將這些檢視最佳化類別傳遞給我們要使用的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="88831-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="88831-154">這可讓類型安全、 編譯時期檢查，以及編輯器 IntelliSense 中檢視範本。</span><span class="sxs-lookup"><span data-stu-id="88831-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="88831-155">我們將建立兩個檢視的模型，以供我們的購物車控制器中： ShoppingCartViewModel 將保存內容的使用者的購物車，並在使用者移除的項目時顯示確認資訊用於 ShoppingCartRemoveViewModel從購物車。</span><span class="sxs-lookup"><span data-stu-id="88831-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="88831-156">為了讓事情變組織受測專案的根目錄中，我們來建立新的 ViewModels 資料夾。</span><span class="sxs-lookup"><span data-stu-id="88831-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="88831-157">以滑鼠右鍵按一下專案，選取 新增 / 新資料夾。</span><span class="sxs-lookup"><span data-stu-id="88831-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="88831-158">將資料夾命名為 ViewModels。</span><span class="sxs-lookup"><span data-stu-id="88831-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="88831-159">接下來，加入 ShoppingCartViewModel 類別 ViewModels 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="88831-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="88831-160">它有兩個屬性： 購物車的項目，且在購物車中保留的所有項目總價的十進位值的清單。</span><span class="sxs-lookup"><span data-stu-id="88831-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="88831-161">現在加入 ShoppingCartRemoveViewModel ViewModels 資料夾，具有下列四個屬性。</span><span class="sxs-lookup"><span data-stu-id="88831-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="88831-162">購物車控制器</span><span class="sxs-lookup"><span data-stu-id="88831-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="88831-163">購物車控制器有三個主要用途： 將項目加入至購物車、 從購物車，移除項目和檢視購物車中的項目。</span><span class="sxs-lookup"><span data-stu-id="88831-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="88831-164">如此可讓我們使用三個類別的剛建立： ShoppingCartViewModel、 ShoppingCartRemoveViewModel 和 ShoppingCart。</span><span class="sxs-lookup"><span data-stu-id="88831-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="88831-165">如 StoreController 及 StoreManagerController 中，我們會將新增欄位以保留 MusicStoreEntities 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="88831-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="88831-166">使用空白的控制器範本專案中加入新的購物車控制器。</span><span class="sxs-lookup"><span data-stu-id="88831-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="88831-167">以下是完成 ShoppingCart 控制器。</span><span class="sxs-lookup"><span data-stu-id="88831-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="88831-168">索引和加入控制器動作應該不會感到陌生。</span><span class="sxs-lookup"><span data-stu-id="88831-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="88831-169">移除和 CartSummary 控制器的動作處理兩個特殊情況下，我們將在下一節中討論。</span><span class="sxs-lookup"><span data-stu-id="88831-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="88831-170">使用 jQuery Ajax 更新</span><span class="sxs-lookup"><span data-stu-id="88831-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="88831-171">接下來，我們將建立的購物車索引頁面 ShoppingCartViewModel 強型別，並使用清單檢視的範本使用之前的相同方法。</span><span class="sxs-lookup"><span data-stu-id="88831-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="88831-172">不過，而不是使用 Html.ActionLink 移除購物車的項目，我們會使用 jQuery 「 連接 」 的這個檢視中的所有連結具有 HTML 類別 RemoveLink click 事件。</span><span class="sxs-lookup"><span data-stu-id="88831-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="88831-173">而不是張貼的表單，此 click 事件處理常式將只會對 AJAX 回呼我們 RemoveFromCart 控制器動作。</span><span class="sxs-lookup"><span data-stu-id="88831-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="88831-174">RemoveFromCart 傳回 JSON 序列化的結果，我們 jQuery 回呼然後剖析，並執行四個快速更新使用 jQuery 頁面：</span><span class="sxs-lookup"><span data-stu-id="88831-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="88831-175">從清單中移除已刪除的相簿</span><span class="sxs-lookup"><span data-stu-id="88831-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="88831-176">更新購物車中的計數標頭</span><span class="sxs-lookup"><span data-stu-id="88831-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="88831-177">向使用者顯示更新訊息</span><span class="sxs-lookup"><span data-stu-id="88831-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="88831-178">更新購物車總價格</span><span class="sxs-lookup"><span data-stu-id="88831-178">Updates the cart total price</span></span>

<span data-ttu-id="88831-179">移除案例都會由索引檢視表中的 Ajax 回撥，因為我們不需要額外的檢視 RemoveFromCart 動作。</span><span class="sxs-lookup"><span data-stu-id="88831-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="88831-180">以下是 /ShoppingCart/Index 檢視的完整程式碼：</span><span class="sxs-lookup"><span data-stu-id="88831-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="88831-181">若要測試時，我們需要能夠將項目加入至我們的購物車。</span><span class="sxs-lookup"><span data-stu-id="88831-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="88831-182">我們會將更新我們**存放區詳細資料**檢視，以加入 [新增至購物車] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="88831-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="88831-183">當我們在到達時，我們可以包含一些我們已加入的專輯額外資訊的自我們上次更新此檢視： 內容類型、 演出者、 價格和專輯封面。</span><span class="sxs-lookup"><span data-stu-id="88831-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="88831-184">更新的存放區詳細資料檢視程式碼會出現如下所示。</span><span class="sxs-lookup"><span data-stu-id="88831-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="88831-185">現在我們可以按一下存放區，而測試加入和移除與我們的購物車的專輯。</span><span class="sxs-lookup"><span data-stu-id="88831-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="88831-186">執行應用程式，並瀏覽至存放區索引。</span><span class="sxs-lookup"><span data-stu-id="88831-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="88831-187">接下來，按一下以檢視專輯的清單類型。</span><span class="sxs-lookup"><span data-stu-id="88831-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="88831-188">在專輯標題上按一下 立即顯示我們已更新的專輯詳細資料檢視，包括 「 新增至購物車 」 按鈕。</span><span class="sxs-lookup"><span data-stu-id="88831-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="88831-189">按一下 新增至購物車 」 按鈕顯示我們的購物車索引檢視與購物車摘要清單。</span><span class="sxs-lookup"><span data-stu-id="88831-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="88831-190">之後載入您的購物車，您可以按一下移除購物車連結以查看您的購物車的 Ajax 更新。</span><span class="sxs-lookup"><span data-stu-id="88831-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="88831-191">我們已建置出購物車，可讓使用者取消註冊項目加入購物車可運作。</span><span class="sxs-lookup"><span data-stu-id="88831-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="88831-192">在下列區段中，我們將允許註冊，並且完成簽出程序。</span><span class="sxs-lookup"><span data-stu-id="88831-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="88831-193">[上一頁](mvc-music-store-part-7.md)
> [下一頁](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="88831-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
