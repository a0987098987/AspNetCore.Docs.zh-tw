---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 第 5 部分： 使用 Knockout.js 建立動態 UI |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 1f645b853fba999ade11c420eceba1b310f80ca3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368424"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="0bb4d-102">第 5 部分： 使用 Knockout.js 建立動態 UI</span><span class="sxs-lookup"><span data-stu-id="0bb4d-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>
====================
<span data-ttu-id="0bb4d-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0bb4d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0bb4d-104">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="0bb4d-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="0bb4d-105">使用 Knockout.js 建立動態 UI</span><span class="sxs-lookup"><span data-stu-id="0bb4d-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="0bb4d-106">在本節中，我們將使用 Knockout.js 將功能新增至 [系統管理] 檢視。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="0bb4d-107">[Knockout.js](http://knockoutjs.com/)是 Javascript 程式庫，可讓您輕鬆地將 HTML 控制項繫結至資料。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="0bb4d-108">Knockout.js 使用 Model View ViewModel (MVVM) 模式。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="0bb4d-109">*模型*商務網域 （在我們的案例、 products 和 orders） 中資料的伺服器端表示法。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="0bb4d-110">*檢視*是展示層 」 (HTML)。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="0bb4d-111">*檢視模型*是可儲存模型資料的 Javascript 物件。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="0bb4d-112">檢視模型是程式碼的抽象概念的 UI。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="0bb4d-113">它並不知道 HTML 的表示法。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="0bb4d-114">相反地，它代表檢視，例如"的項目清單 」 的抽象功能。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="0bb4d-115">檢視是資料繫結至檢視模型。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="0bb4d-116">檢視模型的更新會自動反映在檢視中。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="0bb4d-117">檢視模型也會從檢視中，例如按鈕點選，取得事件，並執行模型，例如建立訂單上的作業。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="0bb4d-118">首先，我們會定義檢視模型。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-118">First we'll define the view-model.</span></span> <span data-ttu-id="0bb4d-119">在那之後，我們會將 HTML 標記繫結至檢視模型。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="0bb4d-120">Admin.cshtml 中新增下列 Razor 區段：</span><span class="sxs-lookup"><span data-stu-id="0bb4d-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="0bb4d-121">您可以在檔案中的任何位置新增這一節。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="0bb4d-122">呈現檢視時，區段會出現在底部的 [HTML] 頁面中，以滑鼠右鍵在關閉前&lt;/b&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="0bb4d-123">所有的指令碼的這個頁面將會移內以註解的指令碼標記：</span><span class="sxs-lookup"><span data-stu-id="0bb4d-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="0bb4d-124">首先，定義的檢視模型類別：</span><span class="sxs-lookup"><span data-stu-id="0bb4d-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="0bb4d-125">**ko.observableArray**是一種特殊的 Knockout，呼叫中的物件*observable*。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="0bb4d-126">從[Knockout.js 文件](http://knockoutjs.com/documentation/observables.html)： 可預見值會是 「 JavaScript 物件，可通知有關變更的訂閱者 」。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="0bb4d-127">當可預見值的內容變更時，檢視會自動更新來比對。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="0bb4d-128">若要填入`products`陣列中，對 web API 中的 AJAX 要求。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="0bb4d-129">您應該記得我們儲存的檢視封包中的 API 的基底 URI (請參閱[第 4 部分](using-web-api-with-entity-framework-part-4.md)的教學課程)。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="0bb4d-130">接下來，將功能加入檢視模型來建立、 更新及刪除產品。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="0bb4d-131">這些函式會提交至 web API 的 AJAX 呼叫，並使用結果更新的檢視模型。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="0bb4d-132">現在最重要的部分： 當 DOM 是 fulled 的載入、 呼叫**ko.applyBindings**函式並傳入的新執行個體`ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="0bb4d-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="0bb4d-133">**Ko.applyBindings**方法會啟用 Knockout，並將檢視模型至檢視。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="0bb4d-134">既然我們已經檢視模型，我們可以建立繫結。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="0bb4d-135">在 Knockout.js，您可以新增`data-bind`至 HTML 元素屬性。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="0bb4d-136">例如，若要將 HTML 清單繫結至陣列，使用`foreach`繫結：</span><span class="sxs-lookup"><span data-stu-id="0bb4d-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="0bb4d-137">`foreach`繫結會逐一查看陣列，並建立子陣列中的每個物件的項目。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="0bb4d-138">陣列物件的屬性可以參考的子項目上的繫結。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="0bb4d-139">將 [產品更新] 清單中的下列繫結：</span><span class="sxs-lookup"><span data-stu-id="0bb4d-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="0bb4d-140">`<li>`項目範圍內，就會發生**foreach**繫結。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="0bb4d-141">表示 Knockout 會轉譯一次到每個產品中的項目`products`陣列。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="0bb4d-142">所有內的繫結`<li>`項目參考該產品的執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="0bb4d-143">例如，`$data.Name`是指`Name`產品上的屬性。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="0bb4d-144">若要設定的文字輸入的值時，使用`value`繫結。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="0bb4d-145">按鈕會繫結至函式上模型檢視中，使用`click`繫結。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="0bb4d-146">Product 執行個體是做為參數傳遞至每個函式。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="0bb4d-147">如需詳細資訊， [Knockout.js 文件](http://knockoutjs.com/documentation/observables.html)有良好的各種繫結的描述。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="0bb4d-148">接下來，新增的繫結**提交**新增產品表單上的事件：</span><span class="sxs-lookup"><span data-stu-id="0bb4d-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="0bb4d-149">這個繫結呼叫`create`函式在檢視模型來建立新的產品。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="0bb4d-150">以下是 [系統管理] 檢視的完整程式碼：</span><span class="sxs-lookup"><span data-stu-id="0bb4d-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="0bb4d-151">執行應用程式、 系統管理員帳戶，登入，並按一下 「 系統管理員 」 連結。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="0bb4d-152">您應該查看的產品清單，而且能夠建立、 更新或刪除產品。</span><span class="sxs-lookup"><span data-stu-id="0bb4d-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0bb4d-153">[上一頁](using-web-api-with-entity-framework-part-4.md)
> [下一頁](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="0bb4d-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
