---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 第 5 部分： 使用解 Knockout.js 建立動態 UI |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: b63446d076fbb1143641dead788042967b996bf8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873806"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="f85b4-102">第 5 部分： 使用解 Knockout.js 建立動態 UI</span><span class="sxs-lookup"><span data-stu-id="f85b4-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>
====================
<span data-ttu-id="f85b4-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f85b4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f85b4-104">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="f85b4-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="f85b4-105">建立動態 UI 與解 Knockout.js</span><span class="sxs-lookup"><span data-stu-id="f85b4-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="f85b4-106">在本節中，我們將使用解 Knockout.js，將功能加入至 [系統管理] 檢視。</span><span class="sxs-lookup"><span data-stu-id="f85b4-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="f85b4-107">[解 Knockout.js](http://knockoutjs.com/)是 Javascript 程式庫，以簡化 HTML 控制項繫結至資料。</span><span class="sxs-lookup"><span data-stu-id="f85b4-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="f85b4-108">解 Knockout.js 使用模型-檢視-ViewModel (MVVM) 模式。</span><span class="sxs-lookup"><span data-stu-id="f85b4-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="f85b4-109">*模型*商務網域中 （在我們的案例、 products 和 orders） 中的資料的伺服器端表示法。</span><span class="sxs-lookup"><span data-stu-id="f85b4-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="f85b4-110">*檢視*是展示層 」 (HTML)。</span><span class="sxs-lookup"><span data-stu-id="f85b4-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="f85b4-111">*檢視模型*是 Javascript 物件，包含模型資料。</span><span class="sxs-lookup"><span data-stu-id="f85b4-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="f85b4-112">檢視模型是抽象的 ui 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f85b4-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="f85b4-113">它不了解 HTML 表示法。</span><span class="sxs-lookup"><span data-stu-id="f85b4-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="f85b4-114">相反地，它代表抽象檢視，例如 「 清單的項目 」 的功能。</span><span class="sxs-lookup"><span data-stu-id="f85b4-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="f85b4-115">檢視是資料繫結至檢視模型。</span><span class="sxs-lookup"><span data-stu-id="f85b4-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="f85b4-116">檢視模型的更新會自動反映在檢視中。</span><span class="sxs-lookup"><span data-stu-id="f85b4-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="f85b4-117">檢視模型也會從檢視中，例如按一下按鈕，取得事件，並執行模型，例如建立訂單上的作業。</span><span class="sxs-lookup"><span data-stu-id="f85b4-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="f85b4-118">首先，我們會定義檢視模型。</span><span class="sxs-lookup"><span data-stu-id="f85b4-118">First we'll define the view-model.</span></span> <span data-ttu-id="f85b4-119">在這之後，我們會檢視模型繫結的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="f85b4-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="f85b4-120">將下列 Razor 區段加入至 Admin.cshtml:</span><span class="sxs-lookup"><span data-stu-id="f85b4-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="f85b4-121">您可以在檔案中任何位置加入這一節。</span><span class="sxs-lookup"><span data-stu-id="f85b4-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="f85b4-122">呈現檢視時，區段會出現在頁面底部的 HTML，以滑鼠右鍵在關閉前&lt;/b&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="f85b4-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="f85b4-123">所有的指令碼的這個頁面將會移內以註解的指令碼標記：</span><span class="sxs-lookup"><span data-stu-id="f85b4-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="f85b4-124">首先，定義的檢視模型類別：</span><span class="sxs-lookup"><span data-stu-id="f85b4-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="f85b4-125">**ko.observableArray**是一種特殊的 Knockout，呼叫中的物件*observable*。</span><span class="sxs-lookup"><span data-stu-id="f85b4-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="f85b4-126">從[解 Knockout.js 文件](http://knockoutjs.com/documentation/observables.html): observable 為 「 JavaScript 物件通知有關變更的訂閱者 」。</span><span class="sxs-lookup"><span data-stu-id="f85b4-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="f85b4-127">當可觀察的內容變更時，檢視會自動更新來比對。</span><span class="sxs-lookup"><span data-stu-id="f85b4-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="f85b4-128">若要填入`products`陣列，請對 web API 的 AJAX 要求。</span><span class="sxs-lookup"><span data-stu-id="f85b4-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="f85b4-129">還記得我們針對檢視包中的 API，儲存的基底 URI (請參閱[第 4 部分](using-web-api-with-entity-framework-part-4.md)的教學課程)。</span><span class="sxs-lookup"><span data-stu-id="f85b4-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="f85b4-130">接下來，將功能加入檢視模型來建立、 更新及刪除產品。</span><span class="sxs-lookup"><span data-stu-id="f85b4-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="f85b4-131">這些函式會提交至 web API 的 AJAX 呼叫，並使用結果更新檢視模型。</span><span class="sxs-lookup"><span data-stu-id="f85b4-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="f85b4-132">現在最重要的部分： 當 DOM 是 fulled 載入、 呼叫**ko.applyBindings**函式，並傳入的新執行個體`ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="f85b4-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="f85b4-133">**Ko.applyBindings**方法啟動 Knockout 和繫結在一起檢視模型檢視。</span><span class="sxs-lookup"><span data-stu-id="f85b4-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="f85b4-134">現在，我們已經檢視模型，我們可以建立繫結。</span><span class="sxs-lookup"><span data-stu-id="f85b4-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="f85b4-135">在解 Knockout.js，您可以加入`data-bind`至 HTML 元素屬性。</span><span class="sxs-lookup"><span data-stu-id="f85b4-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="f85b4-136">例如，若要將 HTML 清單繫結至陣列，使用`foreach`繫結：</span><span class="sxs-lookup"><span data-stu-id="f85b4-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="f85b4-137">`foreach`繫結逐一查看陣列和陣列中建立的每個物件的項目子系。</span><span class="sxs-lookup"><span data-stu-id="f85b4-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="f85b4-138">陣列物件的屬性可以參考繫結的子項目上。</span><span class="sxs-lookup"><span data-stu-id="f85b4-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="f85b4-139">將下列繫結加入至 「 產品更新 」 清單：</span><span class="sxs-lookup"><span data-stu-id="f85b4-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="f85b4-140">`<li>`項目範圍內發生**foreach**繫結。</span><span class="sxs-lookup"><span data-stu-id="f85b4-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="f85b4-141">表示 Knockout 會轉譯一次中每項產品的項目`products`陣列。</span><span class="sxs-lookup"><span data-stu-id="f85b4-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="f85b4-142">所有的繫結內`<li>`元素參考該產品的執行個體。</span><span class="sxs-lookup"><span data-stu-id="f85b4-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="f85b4-143">例如，`$data.Name`指`Name`產品上的屬性。</span><span class="sxs-lookup"><span data-stu-id="f85b4-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="f85b4-144">若要設定的文字輸入值，請使用`value`繫結。</span><span class="sxs-lookup"><span data-stu-id="f85b4-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="f85b4-145">按鈕會繫結至函式上模型檢視中，使用`click`繫結。</span><span class="sxs-lookup"><span data-stu-id="f85b4-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="f85b4-146">產品執行個體做為參數傳遞至每個函式。</span><span class="sxs-lookup"><span data-stu-id="f85b4-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="f85b4-147">如需詳細資訊，[解 Knockout.js 文件](http://knockoutjs.com/documentation/observables.html)有良好的各種繫結的描述。</span><span class="sxs-lookup"><span data-stu-id="f85b4-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="f85b4-148">接下來，加入的繫結**提交**新增產品表單上的事件：</span><span class="sxs-lookup"><span data-stu-id="f85b4-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="f85b4-149">這個繫結呼叫`create`函式上檢視模型建立新的產品。</span><span class="sxs-lookup"><span data-stu-id="f85b4-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="f85b4-150">以下是管理檢視的完整程式碼：</span><span class="sxs-lookup"><span data-stu-id="f85b4-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="f85b4-151">執行應用程式的系統管理員帳戶，登入，按"Admin"連結。</span><span class="sxs-lookup"><span data-stu-id="f85b4-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="f85b4-152">您應該查看的產品清單，並能夠建立、 更新或刪除產品。</span><span class="sxs-lookup"><span data-stu-id="f85b4-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f85b4-153">[上一頁](using-web-api-with-entity-framework-part-4.md)
> [下一頁](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="f85b4-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
