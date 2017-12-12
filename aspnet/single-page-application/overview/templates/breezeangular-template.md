---
uid: single-page-application/overview/templates/breezeangular-template
title: "幫助您輕鬆/Angular 範本 |Microsoft 文件"
author: madskristensen
description: "幫助您輕鬆/Angular 單一網頁應用程式範本"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: faf28a510a83b7fa07585904344176601c2e1f34
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="breezeangular-template"></a><span data-ttu-id="5a289-103">幫助您輕鬆/Angular 範本</span><span class="sxs-lookup"><span data-stu-id="5a289-103">Breeze/Angular template</span></span>
====================
<span data-ttu-id="5a289-104">由[Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="5a289-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="5a289-105">幫助您輕鬆/Angular MVC 範本是由行政區鈴鐺寫入</span><span class="sxs-lookup"><span data-stu-id="5a289-105">The Breeze/Angular MVC Template was written by Ward Bell</span></span>
> 
> [<span data-ttu-id="5a289-106">下載幫助您輕鬆/角度的 MVC 範本</span><span class="sxs-lookup"><span data-stu-id="5a289-106">Download the Breeze/Angular MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=286437)


<span data-ttu-id="5a289-107">[AngularJS](http://angularjs.org)建置單一頁面應用程式 (SPAs) 會將來自 Google 的開放原始碼程式庫。</span><span class="sxs-lookup"><span data-stu-id="5a289-107">[AngularJS](http://angularjs.org) is an open source library from Google for building Single Page Applications (SPAs).</span></span> <span data-ttu-id="5a289-108">它提供資料繫結、 相依性插入和螢幕管理。</span><span class="sxs-lookup"><span data-stu-id="5a289-108">It offers data binding, dependency injection, and screen management.</span></span> <span data-ttu-id="5a289-109">搭配[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa)，另一個開放原始碼程式庫的資料模型和資料管理，且您有很棒的 HTML/JavaScript 用戶端應用程式的重要元素。</span><span class="sxs-lookup"><span data-stu-id="5a289-109">Combine it with [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), another open source library for data modeling and data management, and you have the essential ingredients for a great HTML/JavaScript client app.</span></span>

<span data-ttu-id="5a289-110">幫助您輕鬆/Angular SPA 範本上是一種變化[KnockoutJS SPA 範本](../introduction/knockoutjs-template.md)包含在 ASP.NET 和 Web 工具 2012.2 更新。</span><span class="sxs-lookup"><span data-stu-id="5a289-110">The Breeze/Angular SPA template is a variation on the [KnockoutJS SPA template](../introduction/knockoutjs-template.md) included in the ASP.NET and Web Tools 2012.2 Update.</span></span> <span data-ttu-id="5a289-111">如果您已有 Visual Studio，您必須範例 SPA 啟動並執行在少於 60 秒。</span><span class="sxs-lookup"><span data-stu-id="5a289-111">If you've got Visual Studio, you'll have an example SPA up and running in less than 60 seconds.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

<span data-ttu-id="5a289-112">面向外部，應用程式看起來非常類似 KnockoutJS SPA 範本。</span><span class="sxs-lookup"><span data-stu-id="5a289-112">Outwardly, the application looks the very similar to the KnockoutJS SPA template.</span></span> <span data-ttu-id="5a289-113">但實際上相當不同。</span><span class="sxs-lookup"><span data-stu-id="5a289-113">But it's quite different under the hood.</span></span> <span data-ttu-id="5a289-114">KnockoutJS 範本，Knockout 用於資料繫結與原始 AJAX 資料存取。</span><span class="sxs-lookup"><span data-stu-id="5a289-114">The KnockoutJS template uses Knockout for data binding and raw AJAX for data access.</span></span> <span data-ttu-id="5a289-115">幫助您輕鬆/Angular 範本用於 Angular 資料繫結和幫助您輕鬆存取資料。</span><span class="sxs-lookup"><span data-stu-id="5a289-115">The Breeze/Angular template uses Angular for data binding and Breeze for data access.</span></span> <span data-ttu-id="5a289-116">這些庫啟用額外的功能，包括頁面導覽和歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="5a289-116">These libaries enable additional capabilities, including page navigation and history.</span></span>

<span data-ttu-id="5a289-117">以下是應用程式的相關頁面：</span><span class="sxs-lookup"><span data-stu-id="5a289-117">Here is the application's About page:</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

<span data-ttu-id="5a289-118">此頁面會顯示目前的使用者工作階段期間的執行記錄檔事件包括：</span><span class="sxs-lookup"><span data-stu-id="5a289-118">This page displays a running log of events during the current user session, including:</span></span>

- <span data-ttu-id="5a289-119">分頁。</span><span class="sxs-lookup"><span data-stu-id="5a289-119">Paging.</span></span> <span data-ttu-id="5a289-120">請注意在 #2 與快照 #7 Todo 控制站建立。</span><span class="sxs-lookup"><span data-stu-id="5a289-120">Note the Todo controller creation at #2 and #7.</span></span>
- <span data-ttu-id="5a289-121">遠端查詢 (#3) 和本機快取查詢 (#7)。</span><span class="sxs-lookup"><span data-stu-id="5a289-121">Remote queries (#3) and local cache queries (#7).</span></span>
- <span data-ttu-id="5a289-122">儲存新的 （#5、 #6），修改 (#4) 的實體。</span><span class="sxs-lookup"><span data-stu-id="5a289-122">Saving new (#5, #6) and modified (#4) entities.</span></span>
- <span data-ttu-id="5a289-123">驗證用戶端 (#9)，讓使用者可以在認可變更之前，更正錯誤，資料庫的變更。</span><span class="sxs-lookup"><span data-stu-id="5a289-123">Changes validated on the client (#9), so the user can correct mistakes before committing changes to the database.</span></span>

<span data-ttu-id="5a289-124">沒有進一步探索此範本中包括：</span><span class="sxs-lookup"><span data-stu-id="5a289-124">There's more to explore in this template, including:</span></span>

- <span data-ttu-id="5a289-125">動態載入的 HTML 檢視範本。</span><span class="sxs-lookup"><span data-stu-id="5a289-125">Dynamic loading of HTML view templates.</span></span>
- <span data-ttu-id="5a289-126">自訂資料繫結，透過 Angular 「 指示詞。 」</span><span class="sxs-lookup"><span data-stu-id="5a289-126">Custom data binding through Angular "directives."</span></span>
- <span data-ttu-id="5a289-127">在模組化和相依性插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="5a289-127">Modularity and dependency injection.</span></span>
- <span data-ttu-id="5a289-128">篩選、 排序、 分頁、 預測和包含相關實體的查詢。</span><span class="sxs-lookup"><span data-stu-id="5a289-128">Query filters, sorts, paging, projections, and inclusion of related entities.</span></span>
- <span data-ttu-id="5a289-129">跨多個螢幕共用資料。</span><span class="sxs-lookup"><span data-stu-id="5a289-129">Sharing data across multiple screens.</span></span>
- <span data-ttu-id="5a289-130">將多個變更儲存為單一交易。</span><span class="sxs-lookup"><span data-stu-id="5a289-130">Saving multiple changes as a single transaction.</span></span>
- <span data-ttu-id="5a289-131">驗證規則會自動從伺服器傳送的 JavaScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="5a289-131">Validation rules propagated automatically from the server to the JavaScript client.</span></span>

<span data-ttu-id="5a289-132">我們現在就開始吧。</span><span class="sxs-lookup"><span data-stu-id="5a289-132">Let's get started.</span></span>

## <a name="create-a-breezeangular-template-project"></a><span data-ttu-id="5a289-133">建立幫助您輕鬆/角度範本專案</span><span class="sxs-lookup"><span data-stu-id="5a289-133">Create a Breeze/Angular Template Project</span></span>

<span data-ttu-id="5a289-134">下載並安裝的範本，按一下下載按鈕上方。</span><span class="sxs-lookup"><span data-stu-id="5a289-134">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="5a289-135">範本會封裝成 Visual Studio 擴充功能 (VSIX) 檔案。</span><span class="sxs-lookup"><span data-stu-id="5a289-135">The template is packaged as a Visual Studio Extension (VSIX) file.</span></span> <span data-ttu-id="5a289-136">您可能需要重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="5a289-136">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="5a289-137">在**範本**窗格中，選取**已安裝的範本**展開**Visual C#**節點。</span><span class="sxs-lookup"><span data-stu-id="5a289-137">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="5a289-138">在下**Visual C#**，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="5a289-138">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="5a289-139">在專案範本清單中選取**ASP.NET MVC 4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="5a289-139">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="5a289-140">為專案名稱，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="5a289-140">Name the project and click **OK**.</span></span>

<span data-ttu-id="5a289-141">在**新專案**精靈中，選取**幫助您輕鬆角度 SPA**。</span><span class="sxs-lookup"><span data-stu-id="5a289-141">In the **New Project** wizard, select **Breeze Angular SPA**.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

<span data-ttu-id="5a289-142">按 Ctrl + F5 以建置並執行應用程式，但不偵錯，或按 f5 開始執行並偵錯。</span><span class="sxs-lookup"><span data-stu-id="5a289-142">Press Ctrl-F5 to build and run the application without debugging, or press F5 to run with debugging.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

<span data-ttu-id="5a289-143">當第一次執行應用程式時，它會顯示登入畫面。</span><span class="sxs-lookup"><span data-stu-id="5a289-143">When the application first runs, it displays a login screen.</span></span> <span data-ttu-id="5a289-144">按一下 「 註冊 」 連結，新的頁面 glides 到檢視，您可以在其中輸入使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="5a289-144">Click the "Sign up" link and a new page glides into view, where you can enter a username and password.</span></span> <span data-ttu-id="5a289-145">（登入和註冊網頁會建置使用 ASP.NET MVC）。當您提交註冊表單時，伺服器會產生 TodoList 與您帳戶的兩個項目。</span><span class="sxs-lookup"><span data-stu-id="5a289-145">(The login and registration pages are built using ASP.NET MVC.) When you submit the registration form, the server generates a TodoList with two items for your account.</span></span> <span data-ttu-id="5a289-146">然後它呈現給您一個黃色的提示。</span><span class="sxs-lookup"><span data-stu-id="5a289-146">Then it presents them to you on a yellow note.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

<span data-ttu-id="5a289-147">現在您已在土地的 SPA。</span><span class="sxs-lookup"><span data-stu-id="5a289-147">Now you are in the land of SPA.</span></span> <span data-ttu-id="5a289-148">所有項目您會看到及體驗時操作 Todos 轉譯，而且藉助 Knockout 和輕而易舉的用戶端上管理。</span><span class="sxs-lookup"><span data-stu-id="5a289-148">Everything you see and experience while manipulating Todos is rendered and managed on the client with the help of Knockout and Breeze.</span></span> <span data-ttu-id="5a289-149">探索應用程式的使用者身分...</span><span class="sxs-lookup"><span data-stu-id="5a289-149">Explore the app as a user …</span></span> <span data-ttu-id="5a289-150">但與開發人員的紅眼。</span><span class="sxs-lookup"><span data-stu-id="5a289-150">but with a developer's eye.</span></span> <span data-ttu-id="5a289-151">使用瀏覽器中的開發人員工具，來擷取網路流量。</span><span class="sxs-lookup"><span data-stu-id="5a289-151">Use the developer tools in your browser to capture the network traffic.</span></span> <span data-ttu-id="5a289-152">(在 Internet Explorer： 按 F12，選取**網路**索引標籤，然後按一下**開始擷取**。)現在請嘗試下列各項：</span><span class="sxs-lookup"><span data-stu-id="5a289-152">(In Internet Explorer: Press F12, select the **Network** tab, and click **Start capturing**.) Now try the following:</span></span>

- <span data-ttu-id="5a289-153">加入新的 Todo 項目。</span><span class="sxs-lookup"><span data-stu-id="5a289-153">Add a new Todo item.</span></span>
- <span data-ttu-id="5a289-154">按一下標籤，然後編輯 Todo 項目標題</span><span class="sxs-lookup"><span data-stu-id="5a289-154">Click the label and edit the Todo item title</span></span>
- <span data-ttu-id="5a289-155">請檢查的核取方塊，來標記項目完成。</span><span class="sxs-lookup"><span data-stu-id="5a289-155">Check a checkbox to mark the item done.</span></span> <span data-ttu-id="5a289-156">請注意，文字方塊將會停用，因此不再是可編輯的標題。</span><span class="sxs-lookup"><span data-stu-id="5a289-156">Notice that the textbox is disabled, so the title is no longer editable.</span></span>
- <span data-ttu-id="5a289-157">按一下右邊的標籤 'x'。</span><span class="sxs-lookup"><span data-stu-id="5a289-157">Click the ‘x' to the right of the label.</span></span> <span data-ttu-id="5a289-158">項目就會消失，並且從資料庫中刪除。</span><span class="sxs-lookup"><span data-stu-id="5a289-158">The item disappears and is deleted from the database.</span></span>
- <span data-ttu-id="5a289-159">挑選另一個項目，並清除其標題。</span><span class="sxs-lookup"><span data-stu-id="5a289-159">Pick another item and clear its title.</span></span> <span data-ttu-id="5a289-160">您會取得的標題是必要的驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="5a289-160">You'll get a validation error that the title is required.</span></span> <span data-ttu-id="5a289-161">短暫的暫停之後, 就會還原先前的標題。</span><span class="sxs-lookup"><span data-stu-id="5a289-161">After a brief pause, the previous title is restored.</span></span>
- <span data-ttu-id="5a289-162">輸入將為非常長的標題。</span><span class="sxs-lookup"><span data-stu-id="5a289-162">Type in a ridiculously long title.</span></span> <span data-ttu-id="5a289-163">您會取得不同的驗證錯誤的標題是太長。</span><span class="sxs-lookup"><span data-stu-id="5a289-163">You'll get a different validation error that the title is too long.</span></span>
- <span data-ttu-id="5a289-164">按一下 加入 Todo 清單 」 按鈕。</span><span class="sxs-lookup"><span data-stu-id="5a289-164">Click the "Add Todo List" button.</span></span> <span data-ttu-id="5a289-165">上述清單的左邊會出現新的清單。</span><span class="sxs-lookup"><span data-stu-id="5a289-165">A new list appears to the left of the previous list.</span></span>
- <span data-ttu-id="5a289-166">播放以 TodoList 標題，觸發其必要和長度驗證。</span><span class="sxs-lookup"><span data-stu-id="5a289-166">Play with the TodoList title, triggering its required and length validations.</span></span>
- <span data-ttu-id="5a289-167">按一下 [標題] 文字方塊中，若要清除的錯誤訊息中。</span><span class="sxs-lookup"><span data-stu-id="5a289-167">Click in the title textbox to clear the error message.</span></span>
- <span data-ttu-id="5a289-168">按一下右上角刪除 TodoList 和其 todos 圓形中的"x"。</span><span class="sxs-lookup"><span data-stu-id="5a289-168">Click the "x" in the circle in the upper right corner to delete the TodoList and its todos.</span></span>
- <span data-ttu-id="5a289-169">按一下右上方，以查看這些活動的記錄檔的 「 關於 」 連結。</span><span class="sxs-lookup"><span data-stu-id="5a289-169">Click the "About" link in the upper right to see a log of these activities.</span></span>

<span data-ttu-id="5a289-170">驗證邏輯是由輕而易舉的執行的用戶端。</span><span class="sxs-lookup"><span data-stu-id="5a289-170">The validation logic is performed client-side by Breeze.</span></span> <span data-ttu-id="5a289-171">伺服器模型類別上的驗證屬性傳播至用戶端，而且用戶端連絡伺服器之前，自動執行。</span><span class="sxs-lookup"><span data-stu-id="5a289-171">Validation attributes on the server model classes are propagated to the client and executed automatically before the client contacts the server.</span></span>

<span data-ttu-id="5a289-172">檢閱網路流量。</span><span class="sxs-lookup"><span data-stu-id="5a289-172">Review the network traffic.</span></span> <span data-ttu-id="5a289-173">請注意，沒有呼叫伺服器時沒有幫助您輕鬆偵測到錯誤。</span><span class="sxs-lookup"><span data-stu-id="5a289-173">Notice that there were no calls to the server when Breeze detected an error.</span></span> <span data-ttu-id="5a289-174">每個有效的變更會導致 POST 要求以"/ api/Todo/SaveChanges"。</span><span class="sxs-lookup"><span data-stu-id="5a289-174">Each valid change resulted in a POST request to "/api/Todo/SaveChanges".</span></span> <span data-ttu-id="5a289-175">幫助您輕鬆包裹在一起所做的變更並將它們傳送一起做為單一要求至 Web API 控制器`SaveChanges`方法。</span><span class="sxs-lookup"><span data-stu-id="5a289-175">Breeze bundles the changes and sends them together as a single request to the Web API controller's `SaveChanges` method.</span></span> <span data-ttu-id="5a289-176">這不同於 KockoutJS SPA 範本，可讓 PUT、 POST 和個別刪除要求每個項目。</span><span class="sxs-lookup"><span data-stu-id="5a289-176">That's different from KockoutJS SPA template, which makes PUT, POST, and DELETE requests for each item individually.</span></span>

<span data-ttu-id="5a289-177">另外而且請注意沒有網路流量時沒有您切換 TodoList 之間，以及有關頁面。</span><span class="sxs-lookup"><span data-stu-id="5a289-177">Also, notice there is no network traffic when you switch between the TodoList and About pages.</span></span> <span data-ttu-id="5a289-178">這是因為查詢已被限於本機幫助您輕鬆快取。</span><span class="sxs-lookup"><span data-stu-id="5a289-178">That's because the query has been constrained to the local Breeze cache.</span></span>

## <a name="peek-inside"></a><span data-ttu-id="5a289-179">查看內部</span><span class="sxs-lookup"><span data-stu-id="5a289-179">Peek inside</span></span>

<span data-ttu-id="5a289-180">此應用程式具有用戶端和伺服器端。</span><span class="sxs-lookup"><span data-stu-id="5a289-180">This application has a client side and a server side.</span></span> <span data-ttu-id="5a289-181">用戶端堆疊包含極小的 HTML 和 （在 「 應用程式 」 資料夾中） 的應用程式 JavaScript 模組的組合以及協力廠商的 JavaScript 程式庫 （在 「 指令碼 」 資料夾中）。</span><span class="sxs-lookup"><span data-stu-id="5a289-181">The client-side stack consists of a little HTML and a combination of application JavaScript modules (in the "app" folder) plus third-party JavaScript libraries (in the "Scripts" folder).</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

<span data-ttu-id="5a289-182">UI 架構分隔檢視的 HTML 的 widget 支援簡報中的程式碼控制站。</span><span class="sxs-lookup"><span data-stu-id="5a289-182">The UI architecture separates the HTML widgets of the views from the supporting presentation code in the controllers.</span></span> <span data-ttu-id="5a289-183">角度的資料繫結系統協調檢視和控制器，讓每個可以執行其他相當深不知情的情況下其工作。</span><span class="sxs-lookup"><span data-stu-id="5a289-183">The Angular data-binding system coordinates views and controllers so that each can do its job without intimate knowledge of the other.</span></span>

<span data-ttu-id="5a289-184">控制器會要求來取得和儲存模型實體的資料內容。</span><span class="sxs-lookup"><span data-stu-id="5a289-184">The controller asks the data context to acquire and save the model entities.</span></span> <span data-ttu-id="5a289-185">資料內容會委派十分簡單，建構自我追蹤模型物件從 JSON 查詢結果來工作的絕大部分。</span><span class="sxs-lookup"><span data-stu-id="5a289-185">The data context delegates most of the work to Breeze, which constructs self-tracking model objects from JSON query results.</span></span>

<span data-ttu-id="5a289-186">伺服器端堆疊包含某些開發人員程式碼和三個原則.NET 程式庫： Web API、 Entity Framework 和 Breeze.NET:</span><span class="sxs-lookup"><span data-stu-id="5a289-186">The server-side stack consists of some developer code and three principle .NET libraries: Web API, Entity Framework, and Breeze.NET:</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

<span data-ttu-id="5a289-187">基本架構時 KockoutJS SPA 範本相同。</span><span class="sxs-lookup"><span data-stu-id="5a289-187">The basic architecture is the same as the KockoutJS SPA template.</span></span> <span data-ttu-id="5a289-188">不過，實作會更簡單： DTOs 已刪除，而且大部分的 Entity Framework 的詳細資料已委派給 Breeze.NET。</span><span class="sxs-lookup"><span data-stu-id="5a289-188">However, the implementation is much simpler: The DTOs were deleted, and most of the Entity Framework details have been delegated to Breeze.NET.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a289-189">後續步驟</span><span class="sxs-lookup"><span data-stu-id="5a289-189">Next Steps</span></span>

<span data-ttu-id="5a289-190">我們建議您瀏覽程式碼中，由[廣泛的討論](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa)用戶端和伺服器堆疊，幫助您輕鬆網站上的。</span><span class="sxs-lookup"><span data-stu-id="5a289-190">We suggest that you explore the code, guided by the [extensive discussion](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) of both the client and the server stacks on the Breeze website.</span></span>

<span data-ttu-id="5a289-191">您可能會嘗試播放使用輕而易舉的用戶端查詢;加入一些篩選和排序。</span><span class="sxs-lookup"><span data-stu-id="5a289-191">You might try playing with Breeze client-side query; add some filters and sorts.</span></span> <span data-ttu-id="5a289-192">您可以加入更多的模型屬性，以取得更佳的感覺端對端 SPA 程式開發的多個實體。</span><span class="sxs-lookup"><span data-stu-id="5a289-192">You might add more model properties and more entities to get a better feel for end-to-end SPA development.</span></span> <span data-ttu-id="5a289-193">當您確信設計的時您可以卸除 Todo 功能，並取代它們。</span><span class="sxs-lookup"><span data-stu-id="5a289-193">When you are confident of the design, you can tear out the Todo features and replace them with your own.</span></span>

<span data-ttu-id="5a289-194">祝各位寫程式愉快！</span><span class="sxs-lookup"><span data-stu-id="5a289-194">Happy coding!</span></span>
