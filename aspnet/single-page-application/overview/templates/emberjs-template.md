---
uid: single-page-application/overview/templates/emberjs-template
title: "EmberJS 範本 |Microsoft 文件"
author: xqiu
description: "EmberJS 範本"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="emberjs-template"></a><span data-ttu-id="61543-103">EmberJS 範本</span><span class="sxs-lookup"><span data-stu-id="61543-103">EmberJS template</span></span>
====================
<span data-ttu-id="61543-104">由[Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="61543-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="61543-105">Nathan Totten、 Thiago Santos 和 Xinyang Qiu 寫入 EmberJS MVC 範本。</span><span class="sxs-lookup"><span data-stu-id="61543-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="61543-106">下載 EmberJS MVC 範本</span><span class="sxs-lookup"><span data-stu-id="61543-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="61543-107">EmberJS SPA 範本被設計來協助您快速地開始建置使用 EmberJS 互動的用戶端 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="61543-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="61543-108">「 單一頁面應用程式 」 (SPA) 是載入單一 HTML 頁面，並以動態方式，而不必載入新的頁面更新頁面的 web 應用程式的一般詞彙。</span><span class="sxs-lookup"><span data-stu-id="61543-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="61543-109">初始頁面載入之後, SPA 交談 AJAX 要求透過與伺服器。</span><span class="sxs-lookup"><span data-stu-id="61543-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="61543-110">AJAX 沒有新增，但現在有更輕鬆地建立及維護的大型複雜的 SPA 應用程式的 JavaScript 架構。</span><span class="sxs-lookup"><span data-stu-id="61543-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="61543-111">此外，html5 和 CSS3 會讓您更輕鬆地建立豐富的 Ui。</span><span class="sxs-lookup"><span data-stu-id="61543-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="61543-112">EmberJS SPA 範本使用[Ember](http://emberjs.com/)來處理從 AJAX 要求的頁面更新 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="61543-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="61543-113">Ember.js 會同步處理的最新資料的網頁中使用資料繫結。</span><span class="sxs-lookup"><span data-stu-id="61543-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="61543-114">這樣一來，您不需要撰寫任何程式碼，逐步解說的 JSON 資料，並更新 DOM</span><span class="sxs-lookup"><span data-stu-id="61543-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="61543-115">相反地，您將宣告式屬性放在告訴 Ember.js 如何將資料呈現的 HTML。</span><span class="sxs-lookup"><span data-stu-id="61543-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="61543-116">在伺服器端，EmberJS 範本是幾乎等同[KnockoutJS SPA 範本](../introduction/knockoutjs-template.md)。</span><span class="sxs-lookup"><span data-stu-id="61543-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="61543-117">它會使用 ASP.NET MVC 提供 HTML 文件和 ASP.NET Web API，以處理來自用戶端的 AJAX 要求的服務。</span><span class="sxs-lookup"><span data-stu-id="61543-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="61543-118">如需這些層面的範本，請參閱[KnockoutJS 範本](../introduction/knockoutjs-template.md)文件。</span><span class="sxs-lookup"><span data-stu-id="61543-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="61543-119">本主題著重在 Knockout 範本和 EmberJS 範本之間的差異。</span><span class="sxs-lookup"><span data-stu-id="61543-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="61543-120">建立 EmberJS SPA 範本專案</span><span class="sxs-lookup"><span data-stu-id="61543-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="61543-121">下載並安裝的範本，按一下下載按鈕上方。</span><span class="sxs-lookup"><span data-stu-id="61543-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="61543-122">您可能需要重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="61543-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="61543-123">在**範本**窗格中，選取**已安裝的範本**展開**Visual C#**節點。</span><span class="sxs-lookup"><span data-stu-id="61543-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="61543-124">在下**Visual C#**，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="61543-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="61543-125">在專案範本清單中選取**ASP.NET MVC 4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="61543-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="61543-126">為專案名稱，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="61543-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="61543-127">在**新專案**精靈中，選取**Ember.js SPA 專案**。</span><span class="sxs-lookup"><span data-stu-id="61543-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="61543-128">EmberJS SPA 範本概觀</span><span class="sxs-lookup"><span data-stu-id="61543-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="61543-129">EmberJS 範本使用 jQuery Ember.js，以建立平滑的互動式 UI Handlebars.js 的組合。</span><span class="sxs-lookup"><span data-stu-id="61543-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="61543-130">Ember.js 是使用用戶端 MVC 模式的 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="61543-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="61543-131">A*範本*書面 Handlebars 樣板化的語言，描述應用程式使用者介面。</span><span class="sxs-lookup"><span data-stu-id="61543-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="61543-132">在發行模式中[Handlebars 編譯器](https://github.com/Myslik/csharp-ember-handlebars)用於配套並編譯 handlebars 範本。</span><span class="sxs-lookup"><span data-stu-id="61543-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="61543-133">A*模型*儲存它會從伺服器 （ToDo 清單和 ToDo 項目） 取得的應用程式資料。</span><span class="sxs-lookup"><span data-stu-id="61543-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="61543-134">A*控制器*儲存應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="61543-134">A *controller* stores application state.</span></span> <span data-ttu-id="61543-135">控制站通常會顯示模型資料與對應的範本。</span><span class="sxs-lookup"><span data-stu-id="61543-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="61543-136">A*檢視*會轉譯從應用程式的基本事件和傳遞給控制器。</span><span class="sxs-lookup"><span data-stu-id="61543-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="61543-137">A*路由器*管理應用程式狀態保持同步 Url 和範本。</span><span class="sxs-lookup"><span data-stu-id="61543-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="61543-138">此外，Ember 資料程式庫可以用來同步處理 （透過 rest 式 API 從伺服器取得） 的 JSON 物件和用戶端模型。</span><span class="sxs-lookup"><span data-stu-id="61543-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="61543-139">EmberJS SPA 範本會將指令碼組織成八個圖層：</span><span class="sxs-lookup"><span data-stu-id="61543-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="61543-140">webapi\_adapter.js、 webapi\_serializer.js： 擴充 Ember 資料程式庫，才能使用 ASP.NET Web API。</span><span class="sxs-lookup"><span data-stu-id="61543-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="61543-141">Scripts/helpers.js： 定義新 Ember Handlebars helper。</span><span class="sxs-lookup"><span data-stu-id="61543-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="61543-142">Scripts/app.js： 建立應用程式，並設定配接器和序列化程式。</span><span class="sxs-lookup"><span data-stu-id="61543-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="61543-143">指令碼/應用程式模型/\*.js： 定義模型。</span><span class="sxs-lookup"><span data-stu-id="61543-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="61543-144">指令碼/應用程式檢視/\*.js： 定義的檢視。</span><span class="sxs-lookup"><span data-stu-id="61543-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="61543-145">控制站的指令碼/應用程式/\*.js： 定義控制站。</span><span class="sxs-lookup"><span data-stu-id="61543-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="61543-146">指令碼/應用程式/路由、 Scripts/app/router.js： 定義路由。</span><span class="sxs-lookup"><span data-stu-id="61543-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="61543-147">範本 /\*.hbs： 定義 handlebars 範本。</span><span class="sxs-lookup"><span data-stu-id="61543-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="61543-148">讓我們看看一些詳細指令碼。</span><span class="sxs-lookup"><span data-stu-id="61543-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="61543-149">模型</span><span class="sxs-lookup"><span data-stu-id="61543-149">Models</span></span>

<span data-ttu-id="61543-150">模型會定義指令碼/應用程式/models 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="61543-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="61543-151">有兩個模型檔： todoItem.js 和 todoList.js。</span><span class="sxs-lookup"><span data-stu-id="61543-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="61543-152">**todo.model.js**定義的待辦事項清單中的用戶端 （瀏覽器） 模型。</span><span class="sxs-lookup"><span data-stu-id="61543-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="61543-153">有兩種模型類別： todoItem 和 todoList。</span><span class="sxs-lookup"><span data-stu-id="61543-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="61543-154">在 Ember，模型是 DS 的子類別。模型。</span><span class="sxs-lookup"><span data-stu-id="61543-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="61543-155">模型可以具有屬性與屬性：</span><span class="sxs-lookup"><span data-stu-id="61543-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="61543-156">模型可以定義其他模型的關聯性：</span><span class="sxs-lookup"><span data-stu-id="61543-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="61543-157">模型可以有計算繫結至其他屬性的屬性：</span><span class="sxs-lookup"><span data-stu-id="61543-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="61543-158">模型可以具有觀察者函式，觀察到的屬性變更時叫用：</span><span class="sxs-lookup"><span data-stu-id="61543-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="61543-159">檢視</span><span class="sxs-lookup"><span data-stu-id="61543-159">Views</span></span>

<span data-ttu-id="61543-160">檢視中定義的指令碼/應用程式檢視資料夾中。</span><span class="sxs-lookup"><span data-stu-id="61543-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="61543-161">檢視轉譯的應用程式 UI 中的事件。</span><span class="sxs-lookup"><span data-stu-id="61543-161">A view translates events from the application UI.</span></span> <span data-ttu-id="61543-162">事件處理常式可以回呼叫控制器函式，或只是直接呼叫資料內容。</span><span class="sxs-lookup"><span data-stu-id="61543-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="61543-163">例如，下列程式碼取自 views/TodoItemEditView.js。</span><span class="sxs-lookup"><span data-stu-id="61543-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="61543-164">它會定義事件處理輸入的文字欄位。</span><span class="sxs-lookup"><span data-stu-id="61543-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="61543-165">控制器</span><span class="sxs-lookup"><span data-stu-id="61543-165">Controller</span></span>

<span data-ttu-id="61543-166">控制器的控制站的指令碼/應用程式資料夾中定義。</span><span class="sxs-lookup"><span data-stu-id="61543-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="61543-167">若要表示的單一模型，擴充`Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="61543-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="61543-168">在控制站也可以藉由擴充代表的模型集合`Ember.ArrayController`。</span><span class="sxs-lookup"><span data-stu-id="61543-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="61543-169">比方說，TodoListController 表示陣列`todoList`物件。</span><span class="sxs-lookup"><span data-stu-id="61543-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="61543-170">控制器會依據 todoList 識別碼，以遞減順序排序：</span><span class="sxs-lookup"><span data-stu-id="61543-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="61543-171">控制器會定義名為函式`addTodoList`，建立新的 todoList，並將它加入至陣列。</span><span class="sxs-lookup"><span data-stu-id="61543-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="61543-172">若要查看呼叫此函式的方式，開啟名為 todoListTemplate.html，範本資料夾中的範本檔案。</span><span class="sxs-lookup"><span data-stu-id="61543-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="61543-173">下列範本程式碼將繫結至按鈕`addTodoList`函式：</span><span class="sxs-lookup"><span data-stu-id="61543-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="61543-174">控制器也包含`error`屬性，其中包含錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="61543-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="61543-175">以下是顯示錯誤訊息 （也在 todoListTemplate.html) 的範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="61543-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="61543-176">路由</span><span class="sxs-lookup"><span data-stu-id="61543-176">Routes</span></span>

<span data-ttu-id="61543-177">Router.js 定義路由和預設範本，若要顯示，設定應用程式狀態，並符合為路由的 Url:</span><span class="sxs-lookup"><span data-stu-id="61543-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="61543-178">TodoListRoute.js 覆寫 setupController 函式，如 TodoListRoute 載入資料：</span><span class="sxs-lookup"><span data-stu-id="61543-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="61543-179">Ember 會使用命名慣例，來比對 Url、 路由名稱、 控制器及範本。</span><span class="sxs-lookup"><span data-stu-id="61543-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="61543-180">如需詳細資訊，請參閱[http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS 說明文件。</span><span class="sxs-lookup"><span data-stu-id="61543-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="61543-181">範本</span><span class="sxs-lookup"><span data-stu-id="61543-181">Templates</span></span>

<span data-ttu-id="61543-182">[範本] 資料夾包含四個範本：</span><span class="sxs-lookup"><span data-stu-id="61543-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="61543-183">application.hbs： 呈現應用程式啟動時的預設範本。</span><span class="sxs-lookup"><span data-stu-id="61543-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="61543-184">about.hbs:"/ 關於 」 路由範本。</span><span class="sxs-lookup"><span data-stu-id="61543-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="61543-185">index.hbs： 根範本"/"的路由。</span><span class="sxs-lookup"><span data-stu-id="61543-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="61543-186">todoList.hbs： 的範本 」 / todo"路由。</span><span class="sxs-lookup"><span data-stu-id="61543-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="61543-187">\_navbar.hbs: 範本會定義在導覽功能表。</span><span class="sxs-lookup"><span data-stu-id="61543-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="61543-188">應用程式範本就像是主版頁面。</span><span class="sxs-lookup"><span data-stu-id="61543-188">The application template acts like a master page.</span></span> <span data-ttu-id="61543-189">它包含頁首、 頁尾和"{{插座}}"將根據路由範本中。</span><span class="sxs-lookup"><span data-stu-id="61543-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="61543-190">如需 Ember 中的應用程式範本的詳細資訊，請參閱[http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/)。</span><span class="sxs-lookup"><span data-stu-id="61543-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="61543-191">"/ TodoList 」 範本包含兩個迴圈運算式。</span><span class="sxs-lookup"><span data-stu-id="61543-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="61543-192">外部迴圈是`{{#each controller}}`，與內部迴圈是`{{#each todos}}`。</span><span class="sxs-lookup"><span data-stu-id="61543-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="61543-193">下列程式碼會示範內建`Ember.Checkbox`檢視，請自訂`App.TodoItemEditView`，以及與連結`deleteTodo`動作。</span><span class="sxs-lookup"><span data-stu-id="61543-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="61543-194">`HtmlHelperExtensions` Controllers/HtmlHelperExensions.cs，在定義的類別定義協助程式函式來快取，並插入範本時檔案**偵錯**設**true** Web.config 檔案中。</span><span class="sxs-lookup"><span data-stu-id="61543-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="61543-195">呼叫此函式會從定義 Views/Home/App.cshtml 的 ASP.NET MVC 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="61543-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="61543-196">函式呼叫不含引數，會呈現所有範本資料夾中的範本檔案。</span><span class="sxs-lookup"><span data-stu-id="61543-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="61543-197">您也可以指定子資料夾或特定範本檔案。</span><span class="sxs-lookup"><span data-stu-id="61543-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="61543-198">當**偵錯**是**false**在 Web.config 中，應用程式包含組合項目 」 ~/bundles/templates"。</span><span class="sxs-lookup"><span data-stu-id="61543-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="61543-199">這個套件組合的項目會加入 BundleConfig.cs，使用 Handlebars 編譯器程式庫中：</span><span class="sxs-lookup"><span data-stu-id="61543-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
