---
title: "在 ASP.NET Core 解 Knockout.js MVVM 架構"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: 87b4fdc86f6bb870ae0a8cc85688a549fd0740ac
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a><span data-ttu-id="58ea1-103">在 ASP.NET Core 解 Knockout.js MVVM 架構</span><span class="sxs-lookup"><span data-stu-id="58ea1-103">Knockout.js MVVM Framework in ASP.NET Core</span></span>

<span data-ttu-id="58ea1-104">由[Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="58ea1-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="58ea1-105">Knockout 是常用的 JavaScript 程式庫，可簡化建立複雜資料為基礎的使用者介面。</span><span class="sxs-lookup"><span data-stu-id="58ea1-105">Knockout is a popular JavaScript library that simplifies the creation of complex data-based user interfaces.</span></span> <span data-ttu-id="58ea1-106">它可以是單獨使用或與其他程式庫，例如 jQuery。</span><span class="sxs-lookup"><span data-stu-id="58ea1-106">It can be used alone or with other libraries, such as jQuery.</span></span> <span data-ttu-id="58ea1-107">其主要用途是定義為 JavaScript 物件，基礎資料模型繫結 UI 項目，如此當 ui 進行變更，則會更新模型，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="58ea1-107">Its primary purpose is to bind UI elements to an underlying data model defined as a JavaScript object, such that when changes are made to the UI, the model is updated, and vice versa.</span></span> <span data-ttu-id="58ea1-108">Knockout 有助於模型-檢視-ViewModel (MVVM) 模式的 web 應用程式的用戶端行為的使用。</span><span class="sxs-lookup"><span data-stu-id="58ea1-108">Knockout facilitates the use of a Model-View-ViewModel (MVVM) pattern in a web application's client-side behavior.</span></span> <span data-ttu-id="58ea1-109">其中一個必須了解 Knockout 的 MVVM 實作使用時的兩個主要概念是可預見值 」 和 「 繫結。</span><span class="sxs-lookup"><span data-stu-id="58ea1-109">The two main concepts one must learn when working with Knockout's MVVM implementation are Observables and Bindings.</span></span>

## <a name="getting-started"></a><span data-ttu-id="58ea1-110">使用者入門</span><span class="sxs-lookup"><span data-stu-id="58ea1-110">Getting started</span></span>

<span data-ttu-id="58ea1-111">Knockout 會部署為單一的 JavaScript 檔案，因此安裝和使用它是非常直接使用[bower](bower.md)。</span><span class="sxs-lookup"><span data-stu-id="58ea1-111">Knockout is deployed as a single JavaScript file, so installing and using it is very straightforward using [bower](bower.md).</span></span> <span data-ttu-id="58ea1-112">假設您已經有[bower](bower.md)和[gulp](using-gulp.md)設定，開啟*bower.json*中 ASP.NET Core 專案，並加入 knockout 相依性，如下所示：</span><span class="sxs-lookup"><span data-stu-id="58ea1-112">Assuming you already have [bower](bower.md) and [gulp](using-gulp.md) configured, open *bower.json* in your ASP.NET Core project and add the knockout dependency as shown here:</span></span>

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

<span data-ttu-id="58ea1-113">有了這個位置，然後您可以手動執行 bower 開啟工作執行器總管 （在其他視窗 ‣ 工作執行器總管的檢視 ‣） 然後下工作，以滑鼠右鍵按一下 bower 並選取 [執行]。</span><span class="sxs-lookup"><span data-stu-id="58ea1-113">With this in place, you can then manually run bower by opening the Task Runner Explorer (under View ‣ Other Windows ‣ Task Runner Explorer) and then under Tasks, right-click on bower and select Run.</span></span> <span data-ttu-id="58ea1-114">結果看起來應該類似這樣：</span><span class="sxs-lookup"><span data-stu-id="58ea1-114">The result should appear similar to this:</span></span>

![執行中工作執行器總管 knockout bower](knockout/_static/bower-knockout.png)

<span data-ttu-id="58ea1-116">現在，如果您在您的專案中尋找`wwwroot`資料夾中，您應該會看到 knockout 安裝的 lib 資料夾底下。</span><span class="sxs-lookup"><span data-stu-id="58ea1-116">Now if you look in your project's `wwwroot` folder, you should see knockout installed under the lib folder.</span></span>

![knockout lib 資料夾中安裝](knockout/_static/wwwroot-knockout.png)

<span data-ttu-id="58ea1-118">建議在生產環境中您參考 knockout 透過內容傳遞網路或 CDN，這會增加您的使用者將已經快取的檔案的副本，並因此將不需要完全下載它的可能性。</span><span class="sxs-lookup"><span data-stu-id="58ea1-118">It's recommended that in your production environment you reference knockout via a Content Delivery Network, or CDN, as this increases the likelihood that your users will already have a cached copy of the file and thus will not need to download it at all.</span></span> <span data-ttu-id="58ea1-119">Knockout 位於數個 Cdn，包括 Microsoft Ajax CDN，這裡：</span><span class="sxs-lookup"><span data-stu-id="58ea1-119">Knockout is available on several CDNs, including the Microsoft Ajax CDN, here:</span></span>

[<span data-ttu-id="58ea1-120">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="58ea1-120">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

<span data-ttu-id="58ea1-121">若要包含 Knockout 會使用此頁面上，只要加入`<script>`無論您將主控它 （應用程式，或透過 CDN） 從參考檔案的項目：</span><span class="sxs-lookup"><span data-stu-id="58ea1-121">To include Knockout on a page that will use it, simply add a `<script>` element referencing the file from wherever you will be hosting it (with your application, or via a CDN):</span></span>

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a><span data-ttu-id="58ea1-122">可預見值、 ViewModels 和簡單繫結</span><span class="sxs-lookup"><span data-stu-id="58ea1-122">Observables, ViewModels, and simple binding</span></span>

<span data-ttu-id="58ea1-123">您可能已經很熟悉使用 JavaScript 來管理在網頁上，透過直接存取 DOM 項目，或使用像 jQuery 程式庫。</span><span class="sxs-lookup"><span data-stu-id="58ea1-123">You may already be familiar with using JavaScript to manipulate elements on a web page, either via direct access to the DOM or using a library like jQuery.</span></span> <span data-ttu-id="58ea1-124">通常這種行為是藉由撰寫程式碼直接設定項目值以回應特定的使用者動作來達成。</span><span class="sxs-lookup"><span data-stu-id="58ea1-124">Typically this kind of behavior is achieved by writing code to directly set element values in response to certain user actions.</span></span> <span data-ttu-id="58ea1-125">使用 Knockout、 宣告式方法不會執行相反地，透過此頁面上的項目會繫結至屬性的物件上。</span><span class="sxs-lookup"><span data-stu-id="58ea1-125">With Knockout, a declarative approach is taken instead, through which elements on the page are bound to properties on an object.</span></span> <span data-ttu-id="58ea1-126">不需要撰寫程式碼來操作 DOM 項目，ViewModel 物件時，只需互動使用者動作和 Knockout 會負責確保頁面項目同步處理。</span><span class="sxs-lookup"><span data-stu-id="58ea1-126">Instead of writing code to manipulate DOM elements, user actions simply interact with the ViewModel object, and Knockout takes care of ensuring the page elements are synchronized.</span></span>

<span data-ttu-id="58ea1-127">簡單的範例，請考量以下頁面清單。</span><span class="sxs-lookup"><span data-stu-id="58ea1-127">As a simple example, consider the page list below.</span></span> <span data-ttu-id="58ea1-128">它包含`<span>`具有項目`data-bind`屬性，指出文字內容，應該繫結至 authorName。</span><span class="sxs-lookup"><span data-stu-id="58ea1-128">It includes a `<span>` element with a `data-bind` attribute indicating that the text content should be bound to authorName.</span></span> <span data-ttu-id="58ea1-129">接下來，在單一屬性，定義變數 viewModel 在 JavaScript 區塊`authorName`，設為某個值。</span><span class="sxs-lookup"><span data-stu-id="58ea1-129">Next, in a JavaScript block a variable viewModel is defined with a single property, `authorName`, set to some value.</span></span> <span data-ttu-id="58ea1-130">最後，呼叫`ko.applyBindings`進行時，傳入這個 viewModel 變數。</span><span class="sxs-lookup"><span data-stu-id="58ea1-130">Finally, a call to `ko.applyBindings` is made, passing in this viewModel variable.</span></span>

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

<span data-ttu-id="58ea1-131">在瀏覽器的內容中檢視時<span>viewModel 變數中的值會取代項目：</span><span class="sxs-lookup"><span data-stu-id="58ea1-131">When viewed in the browser, the content of the <span> element is replaced with the value in the viewModel variable:</span></span>

![knockout 簡單繫結](knockout/_static/simple-binding-screenshot.png)

<span data-ttu-id="58ea1-133">我們現在有簡單的單向繫結工作。</span><span class="sxs-lookup"><span data-stu-id="58ea1-133">We now have simple one-way binding working.</span></span> <span data-ttu-id="58ea1-134">請注意，不在程式碼中未我們撰寫 JavaScript 將值指派至範圍的內容。</span><span class="sxs-lookup"><span data-stu-id="58ea1-134">Notice that nowhere in the code did we write JavaScript to assign a value to the span's contents.</span></span> <span data-ttu-id="58ea1-135">如果我們想要管理 ViewModel，我們可以採用此步驟進一步加入 HTML 輸入的文字方塊中，和例如繫結至它的值，因此：</span><span class="sxs-lookup"><span data-stu-id="58ea1-135">If we want to manipulate the ViewModel, we can take this a step further and add an HTML input textbox, and bind to its value, like so:</span></span>

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

<span data-ttu-id="58ea1-136">我們重新載入該頁面，請參閱此值確實會繫結至輸入方塊：</span><span class="sxs-lookup"><span data-stu-id="58ea1-136">Reloading the page, we see that this value is indeed bound to the input box:</span></span>

![knockout 輸入繫結](knockout/_static/input-binding-screenshot.png)

<span data-ttu-id="58ea1-138">不過，若我們變更在文字方塊中的值，對應中的值`<span>`項目不會變更。</span><span class="sxs-lookup"><span data-stu-id="58ea1-138">However, if we change the value in the textbox, the corresponding value in the `<span>` element doesn't change.</span></span> <span data-ttu-id="58ea1-139">為什麼？</span><span class="sxs-lookup"><span data-stu-id="58ea1-139">Why not?</span></span>

<span data-ttu-id="58ea1-140">問題在於，執行任何動作都已收到通知`<span>`，它會更新。</span><span class="sxs-lookup"><span data-stu-id="58ea1-140">The issue is that nothing notified the `<span>` that it needed to be updated.</span></span> <span data-ttu-id="58ea1-141">只要更新 ViewModel 不足夠，本身，除非 ViewModel 屬性會包裝在一種特殊類型。</span><span class="sxs-lookup"><span data-stu-id="58ea1-141">Simply updating the ViewModel isn't by itself sufficient, unless the ViewModel's properties are wrapped in a special type.</span></span> <span data-ttu-id="58ea1-142">我們需要使用**可預見值**中針對任何屬性，必須先變更自動更新發生 ViewModel。</span><span class="sxs-lookup"><span data-stu-id="58ea1-142">We need to use **observables** in the ViewModel for any properties that need to have changes automatically updated as they occur.</span></span> <span data-ttu-id="58ea1-143">藉由變更使用 ViewModel`ko.observable("value")`而不是只是 「 值 」 ViewModel 會更新任何 HTML 項目發生變更時繫結至它的值。</span><span class="sxs-lookup"><span data-stu-id="58ea1-143">By changing the ViewModel to use `ko.observable("value")` instead of just "value", the ViewModel will update any HTML elements that are bound to its value whenever a change occurs.</span></span> <span data-ttu-id="58ea1-144">請注意輸入的方塊沒有更新它們的值，直到它們失去焦點，因此您看不到繫結項目，當您輸入的變更。</span><span class="sxs-lookup"><span data-stu-id="58ea1-144">Note that input boxes don't update their value until they lose focus, so you won't see changes to bound elements as you type.</span></span>

> [!NOTE]
> <span data-ttu-id="58ea1-145">新增支援即時更新之後每個按鍵動作就是新增的,`valueUpdate: "afterkeydown"`至`data-bind`屬性的內容。</span><span class="sxs-lookup"><span data-stu-id="58ea1-145">Adding support for live updating after each keypress is simply a matter of adding `valueUpdate: "afterkeydown"` to the `data-bind` attribute's contents.</span></span> <span data-ttu-id="58ea1-146">您也可以使用，取得此行為`data-bind="textInput: authorName"`取得即時更新的值。</span><span class="sxs-lookup"><span data-stu-id="58ea1-146">You can also get this behavior by using `data-bind="textInput: authorName"` to get instant updates of values.</span></span> 

<span data-ttu-id="58ea1-147">它使用 ko.observable 在更新之後，我們 viewModel:</span><span class="sxs-lookup"><span data-stu-id="58ea1-147">Our viewModel, after updating it to use ko.observable:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="58ea1-148">Knockout 支援許多不同類型的繫結。</span><span class="sxs-lookup"><span data-stu-id="58ea1-148">Knockout supports a number of different kinds of bindings.</span></span> <span data-ttu-id="58ea1-149">到目前為止我們已看到如何繫結至`text`和`value`。</span><span class="sxs-lookup"><span data-stu-id="58ea1-149">So far we've seen how to bind to `text` and to `value`.</span></span> <span data-ttu-id="58ea1-150">您也可以繫結至任何給定的屬性。</span><span class="sxs-lookup"><span data-stu-id="58ea1-150">You can also bind to any given attribute.</span></span> <span data-ttu-id="58ea1-151">例如，若要建立超連結與錨定標記，`src`屬性可以繫結到 viewModel。</span><span class="sxs-lookup"><span data-stu-id="58ea1-151">For instance, to create a hyperlink with an anchor tag, the `src` attribute can be bound to the viewModel.</span></span> <span data-ttu-id="58ea1-152">Knockout 也支援函式繫結。</span><span class="sxs-lookup"><span data-stu-id="58ea1-152">Knockout also supports binding to functions.</span></span> <span data-ttu-id="58ea1-153">為了示範這點，讓我們來更新以包含作者的 twitter 控點，viewModel 和作者的 twitter 網頁的連結，以顯示 twitter 控點。</span><span class="sxs-lookup"><span data-stu-id="58ea1-153">To demonstrate this, let's update the viewModel to include the author's twitter handle, and display the twitter handle as a link to the author's twitter page.</span></span> <span data-ttu-id="58ea1-154">我們將會執行這三個階段。</span><span class="sxs-lookup"><span data-stu-id="58ea1-154">We'll do this in three stages.</span></span>

<span data-ttu-id="58ea1-155">首先，新增 HTML，以便顯示超連結，我們將示範括號括住的作者名稱後面：</span><span class="sxs-lookup"><span data-stu-id="58ea1-155">First, add the HTML to display the hyperlink, which we'll show in parentheses after the author's name:</span></span>

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

<span data-ttu-id="58ea1-156">接下來，更新以包含 twitterUrl 和 twitterAlias 屬性 viewModel:</span><span class="sxs-lookup"><span data-stu-id="58ea1-156">Next, update the viewModel to include the twitterUrl and twitterAlias properties:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="58ea1-157">請注意，此時我們尚未您尚未更新到正確的 URL，這個 twitter 別名 twitterUrl – 只要指向 twitter.com。</span><span class="sxs-lookup"><span data-stu-id="58ea1-157">Notice that at this point we haven't yet updated the twitterUrl to go to the correct URL for this twitter alias – it's just pointing at twitter.com.</span></span> <span data-ttu-id="58ea1-158">同時也請注意，我們使用新的 Knockout 函式， `computed`，如 twitterUrl。</span><span class="sxs-lookup"><span data-stu-id="58ea1-158">Also notice that we're using a new Knockout function, `computed`, for twitterUrl.</span></span> <span data-ttu-id="58ea1-159">這是會通知任何 UI 項目，如果變更了它的可觀察函式。</span><span class="sxs-lookup"><span data-stu-id="58ea1-159">This is an observable function that will notify any UI elements if it changes.</span></span> <span data-ttu-id="58ea1-160">不過，讓它 viewModel 中有其他屬性的存取權，我們需要變更方式，我們會建立 viewModel，使每一個屬性是其本身的陳述式。</span><span class="sxs-lookup"><span data-stu-id="58ea1-160">However, for it to have access to other properties in the viewModel, we need to change how we are creating the viewModel, so that each property is its own statement.</span></span>

<span data-ttu-id="58ea1-161">修訂的 viewModel 宣告如下所示。</span><span class="sxs-lookup"><span data-stu-id="58ea1-161">The revised viewModel declaration is shown below.</span></span> <span data-ttu-id="58ea1-162">它現在會宣告為函式。</span><span class="sxs-lookup"><span data-stu-id="58ea1-162">It is now declared as a function.</span></span> <span data-ttu-id="58ea1-163">請注意，每一個屬性自己陳述式現在，請以分號結束。</span><span class="sxs-lookup"><span data-stu-id="58ea1-163">Notice that each property is its own statement now, ending with a semicolon.</span></span> <span data-ttu-id="58ea1-164">也請注意，若要存取 twitterAlias 屬性值，我們需要執行它，使其參考包含 （）。</span><span class="sxs-lookup"><span data-stu-id="58ea1-164">Also notice that to access the twitterAlias property value, we need to execute it, so its reference includes ().</span></span>

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="58ea1-165">結果如預期般運作的瀏覽器中：</span><span class="sxs-lookup"><span data-stu-id="58ea1-165">The result works as expected in the browser:</span></span>

![knockout 超連結](knockout/_static/hyperlink-screenshot.png)

<span data-ttu-id="58ea1-167">Knockout 也支援特定 UI 項目事件，例如 click 事件的繫結。</span><span class="sxs-lookup"><span data-stu-id="58ea1-167">Knockout also supports binding to certain UI element events, such as the click event.</span></span> <span data-ttu-id="58ea1-168">這可讓您輕鬆並以宣告方式繫結 UI 項目內的應用程式 viewModel 函式。</span><span class="sxs-lookup"><span data-stu-id="58ea1-168">This allows you to easily and declaratively bind UI elements to functions within the application's viewModel.</span></span> <span data-ttu-id="58ea1-169">簡單舉例如下，我們可以加入的按鈕，按一下時，會修改為全部大寫的作者 twitterAlias。</span><span class="sxs-lookup"><span data-stu-id="58ea1-169">As a simple example, we can add a button that, when clicked, modifies the author's twitterAlias to be all caps.</span></span>

<span data-ttu-id="58ea1-170">首先，我們加入按鈕、 繫結至按鈕的按一下事件，並參考函式名稱，我們將新增到 viewModel:</span><span class="sxs-lookup"><span data-stu-id="58ea1-170">First, we add the button, binding to the button's click event, and referencing the function name we're going to add to the viewModel:</span></span>

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

<span data-ttu-id="58ea1-171">然後將函數加入 viewModel 裝設修改 viewModel 的狀態。</span><span class="sxs-lookup"><span data-stu-id="58ea1-171">Then, add the function to the viewModel, and wire it up to modify the viewModel's state.</span></span> <span data-ttu-id="58ea1-172">請注意，若要設定新值給 twitterAlias 屬性，我們呼叫做為方法並傳入新值。</span><span class="sxs-lookup"><span data-stu-id="58ea1-172">Notice that to set a new value to the twitterAlias property, we call it as a method and pass in the new value.</span></span>

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="58ea1-173">執行程式碼，並按一下按鈕會如預期般修改顯示的連結：</span><span class="sxs-lookup"><span data-stu-id="58ea1-173">Running the code and clicking the button modifies the displayed link as expected:</span></span>

![超連結大寫](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a><span data-ttu-id="58ea1-175">控制流程</span><span class="sxs-lookup"><span data-stu-id="58ea1-175">Control flow</span></span>

<span data-ttu-id="58ea1-176">Knockout 包含可執行條件式和迴圈作業的繫結。</span><span class="sxs-lookup"><span data-stu-id="58ea1-176">Knockout includes bindings that can perform conditional and looping operations.</span></span> <span data-ttu-id="58ea1-177">迴圈作業是特別適用於繫結至 UI 清單、 功能表和方格或資料表的資料的清單。</span><span class="sxs-lookup"><span data-stu-id="58ea1-177">Looping operations are especially useful for binding lists of data to UI lists, menus, and grids or tables.</span></span> <span data-ttu-id="58ea1-178">Foreach 繫結會逐一查看陣列。</span><span class="sxs-lookup"><span data-stu-id="58ea1-178">The foreach binding will iterate over an array.</span></span> <span data-ttu-id="58ea1-179">可觀察陣列搭配使用時，它會在加入或移除從該陣列，而不需要重新建立 UI 樹狀目錄中的每個項目中的項目時自動更新 UI 項目。</span><span class="sxs-lookup"><span data-stu-id="58ea1-179">When used with an observable array, it will automatically update the UI elements when items are added or removed from the array, without re-creating every element in the UI tree.</span></span> <span data-ttu-id="58ea1-180">下列範例會使用新的 viewModel 包括遊戲結果的可觀察陣列。</span><span class="sxs-lookup"><span data-stu-id="58ea1-180">The following example uses a new viewModel which includes an observable array of game results.</span></span> <span data-ttu-id="58ea1-181">它使用兩個資料行與繫結至簡單的資料表`foreach`上的繫結`<tbody>`項目。</span><span class="sxs-lookup"><span data-stu-id="58ea1-181">It is bound to a simple table with two columns using a `foreach` binding on the `<tbody>` element.</span></span> <span data-ttu-id="58ea1-182">每個`<tr>`內的項目`<tbody>`gameResults 集合項目會繫結。</span><span class="sxs-lookup"><span data-stu-id="58ea1-182">Each `<tr>` element within `<tbody>` will be bound to an element of the gameResults collection.</span></span>

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

<span data-ttu-id="58ea1-183">請注意，這次我們使用 ViewModel 以大寫"V"因為我們希望能建構它使用"new"（在 applyBindings 呼叫）。</span><span class="sxs-lookup"><span data-stu-id="58ea1-183">Notice that this time we're using ViewModel with a capital “V" because we expect to construct it using “new" (in the applyBindings call).</span></span> <span data-ttu-id="58ea1-184">在執行時，頁面會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="58ea1-184">When executed, the page results in the following output:</span></span>

![knockout 資料錄檢視模型](knockout/_static/record-screenshot.png)

<span data-ttu-id="58ea1-186">為了示範可觀察的集合運作，讓我們加入更多的功能。</span><span class="sxs-lookup"><span data-stu-id="58ea1-186">To demonstrate that the observable collection is working, let's add a bit more functionality.</span></span> <span data-ttu-id="58ea1-187">我們可以包含的另一個遊戲 ViewModel 結果記錄的能力，並將按鈕和一些 UI，才能使用這個新的函式。</span><span class="sxs-lookup"><span data-stu-id="58ea1-187">We can include the ability to record the results of another game to the ViewModel, and then add a button and some UI to work with this new function.</span></span>  <span data-ttu-id="58ea1-188">首先，讓我們來建立 addResult 方法：</span><span class="sxs-lookup"><span data-stu-id="58ea1-188">First, let's create the addResult method:</span></span>

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

<span data-ttu-id="58ea1-189">將這個方法繫結至按鈕使用`click`繫結：</span><span class="sxs-lookup"><span data-stu-id="58ea1-189">Bind this method to a button using the `click` binding:</span></span>

```html
<button data-bind="click: addResult">Add New Result</button>
```

<span data-ttu-id="58ea1-190">在瀏覽器中開啟頁面，並按一下按鈕數次，導致每按一下新的資料表資料列：</span><span class="sxs-lookup"><span data-stu-id="58ea1-190">Open the page in the browser and click the button a couple of times, resulting in a new table row with each click:</span></span>

![將結果加入](knockout/_static/record-addresult-screenshot.png)

<span data-ttu-id="58ea1-192">有幾種方式以支援在 UI 中，通常為內嵌，或在另一個表單，加入新的記錄。</span><span class="sxs-lookup"><span data-stu-id="58ea1-192">There are a few ways to support adding new records in the UI, typically either inline or in a separate form.</span></span> <span data-ttu-id="58ea1-193">我們可以輕易修改要使用文字方塊和 dropdownlists 以便轉變為可編輯的資料表。</span><span class="sxs-lookup"><span data-stu-id="58ea1-193">We can easily modify the table to use textboxes and dropdownlists so that the whole thing is editable.</span></span> <span data-ttu-id="58ea1-194">只要變更`<tr>`項目所示：</span><span class="sxs-lookup"><span data-stu-id="58ea1-194">Just change the `<tr>` element as shown:</span></span>

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

<span data-ttu-id="58ea1-195">請注意，`$root`指的是根 ViewModel，這是可能的選項公開的位置。</span><span class="sxs-lookup"><span data-stu-id="58ea1-195">Note that `$root` refers to the root ViewModel, which is where the possible choices are exposed.</span></span> <span data-ttu-id="58ea1-196">`$data`是指任何目前的模型是在指定的內容位在此情況下，它會參考 resultChoices 陣列，其中每一個都是簡單字串的個別項目。</span><span class="sxs-lookup"><span data-stu-id="58ea1-196">`$data` refers to whatever the current model is within a given context - in this case it refers to an individual element of the resultChoices array, each of which is a simple string.</span></span>

<span data-ttu-id="58ea1-197">這項變更，與整個方格會變成可編輯：</span><span class="sxs-lookup"><span data-stu-id="58ea1-197">With this change, the entire grid becomes editable:</span></span>

![可編輯的格線](knockout/_static/editable-grid-screenshot.png)

<span data-ttu-id="58ea1-199">如果不使用 Knockout，我們無法達成這些全部都使用 jQuery，但它可能不會幾乎最大效率。</span><span class="sxs-lookup"><span data-stu-id="58ea1-199">If we weren't using Knockout, we could achieve all of this using jQuery, but most likely it would not be nearly as efficient.</span></span> <span data-ttu-id="58ea1-200">Knockout 的 UI 項目中，追蹤 ViewModel 中的項目相對應的繫結的資料，並只會更新需要新增、 移除或更新這些項目。</span><span class="sxs-lookup"><span data-stu-id="58ea1-200">Knockout tracks which bound data items in the ViewModel correspond to which UI elements, and only updates those elements that need to be added, removed, or updated.</span></span> <span data-ttu-id="58ea1-201">花費的心力，以達到這個目的使用 jQuery 或直接的 DOM 管理自己和即使如此如果我們要顯示資料表的資料為基礎 （例如 win 遺失記錄） 彙總的結果時，我們需要一次循環它和剖析HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="58ea1-201">It would take significant effort to achieve this ourselves using jQuery or direct DOM manipulation, and even then if we then wanted to display aggregate results (such as a win-loss record) based on the table's data, we would need to once more loop through it and parse the HTML elements.</span></span>  <span data-ttu-id="58ea1-202">使用 Knockout，顯示 win 遺失記錄是一般。</span><span class="sxs-lookup"><span data-stu-id="58ea1-202">With Knockout, displaying the win-loss record is trivial.</span></span> <span data-ttu-id="58ea1-203">我們可以計算 ViewModel 本身內，並且顯示搭配簡單的文字繫結和`<span>`。</span><span class="sxs-lookup"><span data-stu-id="58ea1-203">We can perform the calculations within the ViewModel itself, and then display it with a simple text binding and a `<span>`.</span></span>

<span data-ttu-id="58ea1-204">若要建置 win 遺失記錄字串，我們可以使用計算的可觀察。</span><span class="sxs-lookup"><span data-stu-id="58ea1-204">To build the win-loss record string, we can use a computed observable.</span></span> <span data-ttu-id="58ea1-205">請注意，參考到 ViewModel 內的可觀察屬性必須是函式呼叫，否則它們將不會擷取可觀察的值 (也就是`gameResults()`不`gameResults`中顯示的程式碼):</span><span class="sxs-lookup"><span data-stu-id="58ea1-205">Note that references to observable properties within the ViewModel must be function calls, otherwise they will not retrieve the value of the observable (i.e. `gameResults()` not `gameResults` in the code shown):</span></span>

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

<span data-ttu-id="58ea1-206">將此函式繫結至的範圍內`<h1>`在頁面頂端的項目：</span><span class="sxs-lookup"><span data-stu-id="58ea1-206">Bind this function to a span within the `<h1>` element at the top of the page:</span></span>

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

<span data-ttu-id="58ea1-207">結果：</span><span class="sxs-lookup"><span data-stu-id="58ea1-207">The result:</span></span>

![Win 遺失](knockout/_static/record-winloss-screenshot.png)

<span data-ttu-id="58ea1-209">將資料列加入或修改任何資料列的結果資料行中選取的元素會更新在視窗的頂端顯示的記錄。</span><span class="sxs-lookup"><span data-stu-id="58ea1-209">Adding rows or modifying the selected element in any row's Result column will update the record shown at the top of the window.</span></span>

<span data-ttu-id="58ea1-210">除了為值的繫結，您也可以使用幾乎任何合法的 JavaScript 運算式內的繫結。</span><span class="sxs-lookup"><span data-stu-id="58ea1-210">In addition to binding to values, you can also use almost any legal JavaScript expression within a binding.</span></span> <span data-ttu-id="58ea1-211">比方說，如果 UI 項目應該只會出現在某些情況下，例如當值超過特定閾值，您可以指定這以邏輯方式繫結運算式內：</span><span class="sxs-lookup"><span data-stu-id="58ea1-211">For example, if a UI element should only appear under certain conditions, such as when a value exceeds a certain threshold, you can specify this logically within the binding expression:</span></span>

```html
<div data-bind="visible: customerValue > 100"></div>
```

<span data-ttu-id="58ea1-212">這`<div>`才會顯示 customerValue 時超過 100。</span><span class="sxs-lookup"><span data-stu-id="58ea1-212">This `<div>` will only be visible when the customerValue is over 100.</span></span>

## <a name="templates"></a><span data-ttu-id="58ea1-213">範本</span><span class="sxs-lookup"><span data-stu-id="58ea1-213">Templates</span></span>

<span data-ttu-id="58ea1-214">Knockout 有支援範本，以便您可以輕鬆地從您的行為，將您的 UI，或以累加方式載入視的大型應用程式的 UI 項目。</span><span class="sxs-lookup"><span data-stu-id="58ea1-214">Knockout has support for templates, so that you can easily separate your UI from your behavior, or incrementally load UI elements into a large application on demand.</span></span> <span data-ttu-id="58ea1-215">我們可以更新自己的範本進行每個資料列，就是直接提取 HTML 縮小成範本，並在資料繫結呼叫中依名稱指定的範本上, 前一個範例`<tbody>`。</span><span class="sxs-lookup"><span data-stu-id="58ea1-215">We can update our previous example to make each row its own template by simply pulling the HTML out into a template and specifying the template by name in the data-bind call on `<tbody>`.</span></span>

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

<span data-ttu-id="58ea1-216">Knockout 也支援其他範本化引擎，例如 jQuery.tmpl 程式庫和 Underscore.js 的範本化引擎。</span><span class="sxs-lookup"><span data-stu-id="58ea1-216">Knockout also supports other templating engines, such as the jQuery.tmpl library and Underscore.js's templating engine.</span></span>

## <a name="components"></a><span data-ttu-id="58ea1-217">元件</span><span class="sxs-lookup"><span data-stu-id="58ea1-217">Components</span></span>

<span data-ttu-id="58ea1-218">元件可讓您組織及重複使用的 UI 程式碼通常以及 UI 程式碼所依賴的 ViewModel 資料。</span><span class="sxs-lookup"><span data-stu-id="58ea1-218">Components allow you to organize and reuse UI code, usually along with the ViewModel data on which the UI code depends.</span></span> <span data-ttu-id="58ea1-219">若要建立元件，您只需要指定其範本和其 viewModel，並為它命名。</span><span class="sxs-lookup"><span data-stu-id="58ea1-219">To create a component, you simply need to specify its template and its viewModel, and give it a name.</span></span> <span data-ttu-id="58ea1-220">藉由呼叫 `ko.components.register()` 即可達到此目的。</span><span class="sxs-lookup"><span data-stu-id="58ea1-220">This is done by calling `ko.components.register()`.</span></span> <span data-ttu-id="58ea1-221">除了定義的範本和 viewmodel 內嵌，可以載入從外部檔案，使用程式庫如*require.js*，並產生非常全新且更有效率的程式碼。</span><span class="sxs-lookup"><span data-stu-id="58ea1-221">In addition to defining the templates and viewmodel inline, they can be loaded from external files using a library like *require.js*, resulting in very clean and efficient code.</span></span>

## <a name="communicating-with-apis"></a><span data-ttu-id="58ea1-222">應用程式開發介面與通訊</span><span class="sxs-lookup"><span data-stu-id="58ea1-222">Communicating with APIs</span></span>

<span data-ttu-id="58ea1-223">Knockout 可以使用任何 JSON 格式的資料。</span><span class="sxs-lookup"><span data-stu-id="58ea1-223">Knockout can work with any data in JSON format.</span></span> <span data-ttu-id="58ea1-224">擷取並儲存使用 Knockout 資料的常見方式是使用 jQuery，支援`$.getJSON()`函式來擷取資料，而`$.post()`方法將資料從瀏覽器傳送至應用程式開發介面端點。</span><span class="sxs-lookup"><span data-stu-id="58ea1-224">A common way to retrieve and save data using Knockout is with jQuery, which supports the `$.getJSON()` function to retrieve data, and the `$.post()` method to send data from the browser to an API endpoint.</span></span> <span data-ttu-id="58ea1-225">當然，如果您想以不同的方式來傳送和接收 JSON 資料，Knockout 會與其也適用。</span><span class="sxs-lookup"><span data-stu-id="58ea1-225">Of course, if you prefer a different way to send and receive JSON data, Knockout will work with it as well.</span></span>

## <a name="summary"></a><span data-ttu-id="58ea1-226">總結</span><span class="sxs-lookup"><span data-stu-id="58ea1-226">Summary</span></span>

<span data-ttu-id="58ea1-227">Knockout 提供簡單且精緻的方法，來繫結至 ViewModel 中定義的用戶端應用程式的目前狀態的 UI 項目。</span><span class="sxs-lookup"><span data-stu-id="58ea1-227">Knockout provides a simple, elegant way to bind UI elements to the current state of the client application, defined in a ViewModel.</span></span> <span data-ttu-id="58ea1-228">Knockout 的繫結語法會使用資料繫結屬性中，套用至要處理的 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="58ea1-228">Knockout's binding syntax uses the data-bind attribute, applied to HTML elements that are to be processed.</span></span> <span data-ttu-id="58ea1-229">Knockout 可以有效率地呈現，並藉由追蹤 UI 項目更新大型資料集，只處理的變更影響的項目。</span><span class="sxs-lookup"><span data-stu-id="58ea1-229">Knockout is able to efficiently render and update large data sets by tracking UI elements and only processing changes to affected elements.</span></span> <span data-ttu-id="58ea1-230">UI 邏輯使用範本，可以視需要從外部檔案載入的元件可以分割大型應用程式。</span><span class="sxs-lookup"><span data-stu-id="58ea1-230">Large applications can break up UI logic using templates and components, which can be loaded on demand from external files.</span></span> <span data-ttu-id="58ea1-231">目前第 3 版，Knockout 是穩定的 JavaScript 程式庫，可以改善需要豐富型用戶端互動的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="58ea1-231">Currently version 3, Knockout is a stable JavaScript library that can improve web applications that require rich client interactivity.</span></span>
