---
uid: single-page-application/overview/templates/emberjs-template
title: EmberJS 範本 |Microsoft Docs
author: xqiu
description: EmberJS 範本
ms.author: aspnetcontent
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 2488e9c10550bd9b11c675572c70618f6ca4ac05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823773"
---
<a name="emberjs-template"></a><span data-ttu-id="c7e60-103">EmberJS 範本</span><span class="sxs-lookup"><span data-stu-id="c7e60-103">EmberJS template</span></span>
====================
<span data-ttu-id="c7e60-104">藉由[Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="c7e60-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="c7e60-105">EmberJS MVC 範本是由 Nathan Totten、 Thiago Santos 和 Xinyang Qiu 撰寫。</span><span class="sxs-lookup"><span data-stu-id="c7e60-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="c7e60-106">下載 EmberJS MVC 範本</span><span class="sxs-lookup"><span data-stu-id="c7e60-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="c7e60-107">EmberJS SPA 範本可讓您開始快速建置使用 EmberJS 的互動式用戶端 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c7e60-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="c7e60-108">「 單一頁面應用程式 」 (SPA) 是載入單一 HTML 頁面，並以動態方式，而不是載入新的頁面更新頁面的 web 應用程式的一般詞彙。</span><span class="sxs-lookup"><span data-stu-id="c7e60-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="c7e60-109">在初始網頁載入後，SPA 會與透過 AJAX 要求的伺服器。</span><span class="sxs-lookup"><span data-stu-id="c7e60-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="c7e60-110">AJAX 是新奇，但今天很輕鬆地建置及維護的大型複雜的 SPA 應用程式的 JavaScript 架構。</span><span class="sxs-lookup"><span data-stu-id="c7e60-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="c7e60-111">此外，html5 和 css3 等讓您更輕鬆地建立豐富的 Ui。</span><span class="sxs-lookup"><span data-stu-id="c7e60-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="c7e60-112">EmberJS SPA 範本會使用[Ember](http://emberjs.com/)來處理從 AJAX 要求的頁面更新的 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="c7e60-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="c7e60-113">Ember.js 會使用資料繫結與最新的資料同步處理頁面。</span><span class="sxs-lookup"><span data-stu-id="c7e60-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="c7e60-114">如此一來，您不需要撰寫任何程式碼，逐步解說的 JSON 資料，並更新 DOM</span><span class="sxs-lookup"><span data-stu-id="c7e60-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="c7e60-115">相反地，您將宣告式屬性放在告訴 Ember.js 如何將資料呈現的 HTML。</span><span class="sxs-lookup"><span data-stu-id="c7e60-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="c7e60-116">伺服器端上 EmberJS 範本是幾乎等同[KnockoutJS SPA 範本](../introduction/knockoutjs-template.md)。</span><span class="sxs-lookup"><span data-stu-id="c7e60-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="c7e60-117">它會使用 ASP.NET MVC 提供 HTML 文件和 ASP.NET Web API，以處理來自用戶端的 AJAX 要求。</span><span class="sxs-lookup"><span data-stu-id="c7e60-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="c7e60-118">如需關於這些方面的範本，請參閱[KnockoutJS 範本](../introduction/knockoutjs-template.md)文件。</span><span class="sxs-lookup"><span data-stu-id="c7e60-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="c7e60-119">本主題著重於 Knockout 範本和 EmberJS 範本之間的差異。</span><span class="sxs-lookup"><span data-stu-id="c7e60-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="c7e60-120">建立 EmberJS SPA 範本專案</span><span class="sxs-lookup"><span data-stu-id="c7e60-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="c7e60-121">下載並安裝的範本，按一下上方的 [下載] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c7e60-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="c7e60-122">您可能需要重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="c7e60-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="c7e60-123">在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。</span><span class="sxs-lookup"><span data-stu-id="c7e60-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="c7e60-124">底下**Visual C#**，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="c7e60-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="c7e60-125">在專案範本清單中，選取**ASP.NET MVC 4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="c7e60-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="c7e60-126">將專案命名，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="c7e60-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="c7e60-127">在 **新的專案**精靈中，選取**Ember.js SPA 專案**。</span><span class="sxs-lookup"><span data-stu-id="c7e60-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="c7e60-128">EmberJS SPA 範本概觀</span><span class="sxs-lookup"><span data-stu-id="c7e60-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="c7e60-129">EmberJS 範本會使用 jQuery、 Ember.js、 Handlebars.js 建立平滑的互動式 UI 的組合。</span><span class="sxs-lookup"><span data-stu-id="c7e60-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="c7e60-130">Ember.js 是使用用戶端 MVC 模式的 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="c7e60-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="c7e60-131">A*範本*、 Handlebars 範本化語言中，以撰寫描述應用程式使用者介面。</span><span class="sxs-lookup"><span data-stu-id="c7e60-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="c7e60-132">在發行模式中， [Handlebars 編譯器](https://github.com/Myslik/csharp-ember-handlebars)用來包裝並編譯 handlebars 範本。</span><span class="sxs-lookup"><span data-stu-id="c7e60-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="c7e60-133">A*模型*儲存應用程式資料，它會取得從伺服器 （「 待辦事項清單 」 和 「 待辦事項項目 」）。</span><span class="sxs-lookup"><span data-stu-id="c7e60-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="c7e60-134">A*控制器*儲存應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="c7e60-134">A *controller* stores application state.</span></span> <span data-ttu-id="c7e60-135">控制器通常會顯示模型資料與對應的範本。</span><span class="sxs-lookup"><span data-stu-id="c7e60-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="c7e60-136">A*檢視*會轉譯從應用程式的基本事件，並將傳遞到控制器。</span><span class="sxs-lookup"><span data-stu-id="c7e60-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="c7e60-137">A*路由器*管理應用程式狀態，讓 Url 和範本保持同步。</span><span class="sxs-lookup"><span data-stu-id="c7e60-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="c7e60-138">颾魤 ㄛ Ember 資料程式庫可用來同步處理 （透過支援的 RESTful API 從伺服器取得） 的 JSON 物件和用戶端模型。</span><span class="sxs-lookup"><span data-stu-id="c7e60-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="c7e60-139">EmberJS SPA 範本會將指令碼組織成八個層級：</span><span class="sxs-lookup"><span data-stu-id="c7e60-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="c7e60-140">webapi\_adapter.js、 webapi\_serializer.js： 擴充 Ember 資料程式庫，才能使用 ASP.NET Web API。</span><span class="sxs-lookup"><span data-stu-id="c7e60-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="c7e60-141">Scripts/helpers.js： 定義新的 Ember Handlebars 協助程式。</span><span class="sxs-lookup"><span data-stu-id="c7e60-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="c7e60-142">Scripts/app.js： 建立應用程式，並設定在配接器與序列化程式。</span><span class="sxs-lookup"><span data-stu-id="c7e60-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="c7e60-143">指令碼/應用程式/模型/\*.js： 定義模型。</span><span class="sxs-lookup"><span data-stu-id="c7e60-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="c7e60-144">指令碼/應用程式/檢視/\*.js： 定義檢視。</span><span class="sxs-lookup"><span data-stu-id="c7e60-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="c7e60-145">控制站的指令碼/應用程式/\*.js： 定義控制站。</span><span class="sxs-lookup"><span data-stu-id="c7e60-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="c7e60-146">指令碼/應用程式/routes、 Scripts/app/router.js： 定義的路由。</span><span class="sxs-lookup"><span data-stu-id="c7e60-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="c7e60-147">範本 /\*.hbs： 定義 handlebars 範本。</span><span class="sxs-lookup"><span data-stu-id="c7e60-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="c7e60-148">讓我們看看一些更多詳細資料中的這些指令碼。</span><span class="sxs-lookup"><span data-stu-id="c7e60-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="c7e60-149">模型</span><span class="sxs-lookup"><span data-stu-id="c7e60-149">Models</span></span>

<span data-ttu-id="c7e60-150">模型被定義在指令碼/應用程式/models 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="c7e60-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="c7e60-151">有兩個模型檔案： todoItem.js 和 todoList.js。</span><span class="sxs-lookup"><span data-stu-id="c7e60-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="c7e60-152">**todo.model.js**定義待辦事項清單的用戶端 （瀏覽器） 模型。</span><span class="sxs-lookup"><span data-stu-id="c7e60-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="c7e60-153">有兩個模型類別： todoItem 和 todoList。</span><span class="sxs-lookup"><span data-stu-id="c7e60-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="c7e60-154">在 Ember，模型會是 DS 的子類別。模型。</span><span class="sxs-lookup"><span data-stu-id="c7e60-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="c7e60-155">模型可以具有與屬性的屬性：</span><span class="sxs-lookup"><span data-stu-id="c7e60-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="c7e60-156">模型可以定義其他模型的關聯性：</span><span class="sxs-lookup"><span data-stu-id="c7e60-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="c7e60-157">模型可以有計算繫結至其他屬性的屬性：</span><span class="sxs-lookup"><span data-stu-id="c7e60-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="c7e60-158">模型可以具有觀察者函式，所觀察的屬性變更時，會叫用：</span><span class="sxs-lookup"><span data-stu-id="c7e60-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="c7e60-159">檢視</span><span class="sxs-lookup"><span data-stu-id="c7e60-159">Views</span></span>

<span data-ttu-id="c7e60-160">在 [指令碼/app/views] 資料夾中定義的檢視。</span><span class="sxs-lookup"><span data-stu-id="c7e60-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="c7e60-161">檢視轉譯應用程式 UI 中的事件。</span><span class="sxs-lookup"><span data-stu-id="c7e60-161">A view translates events from the application UI.</span></span> <span data-ttu-id="c7e60-162">事件處理常式可以回撥至控制器函式，或只是直接呼叫資料內容。</span><span class="sxs-lookup"><span data-stu-id="c7e60-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="c7e60-163">例如，下列程式碼取自於 views/TodoItemEditView.js。</span><span class="sxs-lookup"><span data-stu-id="c7e60-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="c7e60-164">它會定義之事件處理常式的輸入的文字欄位。</span><span class="sxs-lookup"><span data-stu-id="c7e60-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="c7e60-165">控制器</span><span class="sxs-lookup"><span data-stu-id="c7e60-165">Controller</span></span>

<span data-ttu-id="c7e60-166">控制器會定義在指令碼/app/controllers 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="c7e60-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="c7e60-167">若要表示的單一模型，擴充`Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="c7e60-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="c7e60-168">在控制站也可以藉由擴充代表的模型集合`Ember.ArrayController`。</span><span class="sxs-lookup"><span data-stu-id="c7e60-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="c7e60-169">比方說，TodoListController 代表陣列的`todoList`物件。</span><span class="sxs-lookup"><span data-stu-id="c7e60-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="c7e60-170">控制器會依 todoList 識別碼，以遞減順序排序：</span><span class="sxs-lookup"><span data-stu-id="c7e60-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="c7e60-171">控制器會定義名為函式`addTodoList`，這會建立新的 todoList，並將它加入至陣列。</span><span class="sxs-lookup"><span data-stu-id="c7e60-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="c7e60-172">若要查看如何呼叫這個函式時，開啟名為 todoListTemplate.html，在 [範本] 資料夾中的範本檔案。</span><span class="sxs-lookup"><span data-stu-id="c7e60-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="c7e60-173">下列範本程式碼會繫結至按鈕`addTodoList`函式：</span><span class="sxs-lookup"><span data-stu-id="c7e60-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="c7e60-174">控制器也含有`error`屬性，其中包含錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="c7e60-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="c7e60-175">以下是顯示 （也是在 todoListTemplate.html) 的錯誤訊息的範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="c7e60-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="c7e60-176">路由</span><span class="sxs-lookup"><span data-stu-id="c7e60-176">Routes</span></span>

<span data-ttu-id="c7e60-177">Router.js 會定義路由和預設範本，若要顯示的方式，設定應用程式狀態，並比對路由的 Url:</span><span class="sxs-lookup"><span data-stu-id="c7e60-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="c7e60-178">TodoListRoute.js TodoListRoute 針對載入資料，藉由覆寫 setupController 函式：</span><span class="sxs-lookup"><span data-stu-id="c7e60-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="c7e60-179">Ember 會使用命名慣例，來比對 Url、 路由名稱、 控制器及範本。</span><span class="sxs-lookup"><span data-stu-id="c7e60-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="c7e60-180">如需詳細資訊，請參閱 < [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS 說明文件。</span><span class="sxs-lookup"><span data-stu-id="c7e60-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="c7e60-181">範本</span><span class="sxs-lookup"><span data-stu-id="c7e60-181">Templates</span></span>

<span data-ttu-id="c7e60-182">[範本] 資料夾包含四個範本：</span><span class="sxs-lookup"><span data-stu-id="c7e60-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="c7e60-183">application.hbs： 轉譯應用程式啟動時的預設範本。</span><span class="sxs-lookup"><span data-stu-id="c7e60-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="c7e60-184">about.hbs:"/ [關於]"路由範本。</span><span class="sxs-lookup"><span data-stu-id="c7e60-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="c7e60-185">index.hbs： 範本的根"/"的路由。</span><span class="sxs-lookup"><span data-stu-id="c7e60-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="c7e60-186">todoList.hbs: 範本"/ 待辦事項 」 路由。</span><span class="sxs-lookup"><span data-stu-id="c7e60-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="c7e60-187">\_navbar.hbs： 範本會定義導覽功能表。</span><span class="sxs-lookup"><span data-stu-id="c7e60-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="c7e60-188">應用程式範本的作用就像主版頁面。</span><span class="sxs-lookup"><span data-stu-id="c7e60-188">The application template acts like a master page.</span></span> <span data-ttu-id="c7e60-189">它包含為頁首、 頁尾及"{{輸出}}"將取決於路由範本中。</span><span class="sxs-lookup"><span data-stu-id="c7e60-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="c7e60-190">如需有關 Ember 中的應用程式範本的詳細資訊，請參閱[ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/)。</span><span class="sxs-lookup"><span data-stu-id="c7e60-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="c7e60-191">"/ TodoList 」 範本包含兩個迴圈的運算式。</span><span class="sxs-lookup"><span data-stu-id="c7e60-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="c7e60-192">外部迴圈`{{#each controller}}`，與內部迴圈是`{{#each todos}}`。</span><span class="sxs-lookup"><span data-stu-id="c7e60-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="c7e60-193">下列程式碼會顯示內建`Ember.Checkbox`檢視，請自訂`App.TodoItemEditView`，並使用連結`deleteTodo`動作。</span><span class="sxs-lookup"><span data-stu-id="c7e60-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="c7e60-194">`HtmlHelperExtensions` Controllers/HtmlHelperExensions.cs，所定義的類別定義協助程式函式來快取，並插入範本檔的時機**偵錯**設定為**true** Web.config 檔案中。</span><span class="sxs-lookup"><span data-stu-id="c7e60-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="c7e60-195">此函式呼叫從 Views/Home/App.cshtml 中定義的 ASP.NET MVC 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="c7e60-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="c7e60-196">呼叫不含引數，函式會呈現所有範本 資料夾中的範本檔案。</span><span class="sxs-lookup"><span data-stu-id="c7e60-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="c7e60-197">您也可以指定子資料夾或特定的範本檔案。</span><span class="sxs-lookup"><span data-stu-id="c7e60-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="c7e60-198">當**偵錯**是**false**在 Web.config 中，應用程式包含的套件組合的項目 」 ~/bundles/templates"。</span><span class="sxs-lookup"><span data-stu-id="c7e60-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="c7e60-199">這個套件組合項目會加入 BundleConfig.cs，使用 Handlebars 編譯器程式庫中：</span><span class="sxs-lookup"><span data-stu-id="c7e60-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
