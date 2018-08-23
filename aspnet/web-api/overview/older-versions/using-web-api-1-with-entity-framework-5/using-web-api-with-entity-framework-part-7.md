---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 第 7 節： 建立主要頁面 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: bb4704e7f4f13fab04acdbdd642174884517e18a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831805"
---
<a name="part-7-creating-the-main-page"></a><span data-ttu-id="56857-102">第 7 節： 建立主要頁面</span><span class="sxs-lookup"><span data-stu-id="56857-102">Part 7: Creating the Main Page</span></span>
====================
<span data-ttu-id="56857-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="56857-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="56857-104">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="56857-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="56857-105">建立主要頁面</span><span class="sxs-lookup"><span data-stu-id="56857-105">Creating the Main Page</span></span>

<span data-ttu-id="56857-106">在本節中，您將建立主應用程式頁面。</span><span class="sxs-lookup"><span data-stu-id="56857-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="56857-107">此頁面將會比 [管理員] 頁面中，更為複雜，因此我們將方法的幾個步驟。</span><span class="sxs-lookup"><span data-stu-id="56857-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="56857-108">過程中，您會看到一些更進階的 Knockout.js 技術。</span><span class="sxs-lookup"><span data-stu-id="56857-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="56857-109">以下是基本的版面配置的頁面：</span><span class="sxs-lookup"><span data-stu-id="56857-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="56857-110">「 產品 」 包含產品的陣列。</span><span class="sxs-lookup"><span data-stu-id="56857-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="56857-111">「 購物車 」 保留數量的產品的陣列。</span><span class="sxs-lookup"><span data-stu-id="56857-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="56857-112">按一下 「 加入購物車 」 更新購物車。</span><span class="sxs-lookup"><span data-stu-id="56857-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="56857-113">「 訂單 」 保留訂單識別碼的陣列。</span><span class="sxs-lookup"><span data-stu-id="56857-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="56857-114">[詳細資料] 保留訂單詳細資料，也就是陣列的項目 （產品以數量）</span><span class="sxs-lookup"><span data-stu-id="56857-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="56857-115">我們一開始先在 HTML 中，定義一些基本的版面配置，不含資料繫結或指令碼。</span><span class="sxs-lookup"><span data-stu-id="56857-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="56857-116">開啟檔案 Views/Home/Index.cshtml，並將所有的內容取代為下列：</span><span class="sxs-lookup"><span data-stu-id="56857-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="56857-117">接下來，新增指令碼區段，並建立空的檢視模型：</span><span class="sxs-lookup"><span data-stu-id="56857-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="56857-118">根據設計，稍早所述，我們的檢視模型可以需要可預見值的產品、 購物車、 訂單及詳細資料。</span><span class="sxs-lookup"><span data-stu-id="56857-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="56857-119">新增下列變數以`AppViewModel`物件：</span><span class="sxs-lookup"><span data-stu-id="56857-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="56857-120">使用者可以從產品清單的項目加入購物車，並移除購物車中的項目。</span><span class="sxs-lookup"><span data-stu-id="56857-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="56857-121">若要封裝這些函式，我們將建立代表產品的另一個檢視模型類別。</span><span class="sxs-lookup"><span data-stu-id="56857-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="56857-122">將下列程式碼新增至 `AppViewModel`：</span><span class="sxs-lookup"><span data-stu-id="56857-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="56857-123">`ProductViewModel`類別包含兩個函式用來移動進出購物車的產品：`addItemToCart`一個單位的產品加入購物車和`removeAllFromCart`移除所有數量的產品。</span><span class="sxs-lookup"><span data-stu-id="56857-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="56857-124">使用者可以選取現有的訂單，並取得訂單的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="56857-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="56857-125">我們將會封裝至其他檢視模型的這項功能：</span><span class="sxs-lookup"><span data-stu-id="56857-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="56857-126">`OrderDetailsViewModel`初始化的順序，它將 AJAX 要求傳送至伺服器擷取訂單詳細資料。</span><span class="sxs-lookup"><span data-stu-id="56857-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="56857-127">另外而且請注意`total`屬性上的`OrderDetailsViewModel`。</span><span class="sxs-lookup"><span data-stu-id="56857-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="56857-128">這個屬性是一種特殊的可預見值呼叫[計算可預見值](http://knockoutjs.com/documentation/computedObservables.html)。</span><span class="sxs-lookup"><span data-stu-id="56857-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="56857-129">如同名稱所暗示，計算的可預見值可讓您計算的值的資料繫結&#8212;在此情況下，總成本的順序。</span><span class="sxs-lookup"><span data-stu-id="56857-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="56857-130">接下來，新增這些函式來`AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="56857-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="56857-131">`resetCart` 移除購物車中的所有項目。</span><span class="sxs-lookup"><span data-stu-id="56857-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="56857-132">`getDetails` 取得訂單的詳細資料 (由新的 pusing`OrderDetailsViewModel`拖曳至`details`清單)。</span><span class="sxs-lookup"><span data-stu-id="56857-132">`getDetails` gets the details for an order (by pusing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="56857-133">`createOrder` 建立新的訂單，然後清空購物車。</span><span class="sxs-lookup"><span data-stu-id="56857-133">`createOrder` creates a new order and empties the cart.</span></span>


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="56857-134">最後，藉由提出 AJAX 要求產品和訂單的初始化檢視模型：</span><span class="sxs-lookup"><span data-stu-id="56857-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="56857-135">好，這是許多程式碼，但我們在建置向上逐步，希望設計很清楚。</span><span class="sxs-lookup"><span data-stu-id="56857-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="56857-136">現在我們可以將某些 Knockout.js 繫結的 html。</span><span class="sxs-lookup"><span data-stu-id="56857-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="56857-137">**產品**</span><span class="sxs-lookup"><span data-stu-id="56857-137">**Products**</span></span>

<span data-ttu-id="56857-138">以下是產品清單中的繫結：</span><span class="sxs-lookup"><span data-stu-id="56857-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="56857-139">這會逐一查看產品陣列，並顯示名稱和價格。</span><span class="sxs-lookup"><span data-stu-id="56857-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="56857-140">只有當使用者登入時，會顯示 [新增順序] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="56857-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="56857-141">[新增順序] 按鈕呼叫`addItemToCart`上`ProductViewModel`產品的執行個體。</span><span class="sxs-lookup"><span data-stu-id="56857-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="56857-142">這示範了不錯的功能的 Knockout.js： 當檢視模型包含其他檢視模型時，您可以將繫結套用至內部模型。</span><span class="sxs-lookup"><span data-stu-id="56857-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="56857-143">在此範例中，內的繫結`foreach`套用至每個`ProductViewModel`執行個體。</span><span class="sxs-lookup"><span data-stu-id="56857-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="56857-144">這個方法會比較簡潔，比將所有的功能放入單一檢視模型。</span><span class="sxs-lookup"><span data-stu-id="56857-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="56857-145">**購物車**</span><span class="sxs-lookup"><span data-stu-id="56857-145">**Cart**</span></span>

<span data-ttu-id="56857-146">以下是購物車的繫結：</span><span class="sxs-lookup"><span data-stu-id="56857-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="56857-147">這會逐一查看車陣列，並顯示名稱、 價格和數量。</span><span class="sxs-lookup"><span data-stu-id="56857-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="56857-148">請注意，「 移除 」 連結的 「 建立訂單 」 按鈕會繫結至檢視模型函式。</span><span class="sxs-lookup"><span data-stu-id="56857-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="56857-149">**訂單**</span><span class="sxs-lookup"><span data-stu-id="56857-149">**Orders**</span></span>

<span data-ttu-id="56857-150">以下是訂單清單的繫結：</span><span class="sxs-lookup"><span data-stu-id="56857-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="56857-151">這會逐一查看訂單，並顯示訂單識別碼。</span><span class="sxs-lookup"><span data-stu-id="56857-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="56857-152">連結上的 click 事件繫結至`getDetails`函式。</span><span class="sxs-lookup"><span data-stu-id="56857-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="56857-153">**訂單詳細資料**</span><span class="sxs-lookup"><span data-stu-id="56857-153">**Order Details**</span></span>

<span data-ttu-id="56857-154">以下是訂單明細的繫結：</span><span class="sxs-lookup"><span data-stu-id="56857-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="56857-155">這會逐一查看在順序中的項目，並顯示產品、 價格和 quanity。</span><span class="sxs-lookup"><span data-stu-id="56857-155">This iterates over the items in the order and displays the product, price, and quanity.</span></span> <span data-ttu-id="56857-156">周圍的 div 時，會詳細資料陣列包含一或多個項目，只顯示。</span><span class="sxs-lookup"><span data-stu-id="56857-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="56857-157">結論</span><span class="sxs-lookup"><span data-stu-id="56857-157">Conclusion</span></span>

<span data-ttu-id="56857-158">在本教學課程中，您可以建立使用 Entity Framework 與資料庫，並提供在資料層之上的公眾對應介面的 ASP.NET Web API 通訊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="56857-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="56857-159">我們可以利用 ASP.NET MVC 4 來轉譯 HTML 網頁和 Knockout.js 以及 jQuery 提供動態的互動，而不需要頁面重新載入。</span><span class="sxs-lookup"><span data-stu-id="56857-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="56857-160">其他資源：</span><span class="sxs-lookup"><span data-stu-id="56857-160">Additional resources:</span></span>

- [<span data-ttu-id="56857-161">ASP.NET 資料存取內容對應</span><span class="sxs-lookup"><span data-stu-id="56857-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="56857-162">Entity Framework 開發人員中心</span><span class="sxs-lookup"><span data-stu-id="56857-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="56857-163">上一步</span><span class="sxs-lookup"><span data-stu-id="56857-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
