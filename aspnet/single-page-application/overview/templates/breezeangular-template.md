---
uid: single-page-application/overview/templates/breezeangular-template
title: Breeze/Angular 範本 |Microsoft Docs
author: madskristensen
description: Breeze/Angular 單一頁面應用程式範本
ms.author: aspnetcontent
ms.date: 03/08/2013
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: 3799c891625b28312b54ed33628748dcc1b74925
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811746"
---
<a name="breezeangular-template"></a><span data-ttu-id="77846-103">Breeze/Angular 範本</span><span class="sxs-lookup"><span data-stu-id="77846-103">Breeze/Angular template</span></span>
====================
<span data-ttu-id="77846-104">藉由[Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="77846-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="77846-105">Breeze/Angular MVC 範本所編寫的 Ward Bell</span><span class="sxs-lookup"><span data-stu-id="77846-105">The Breeze/Angular MVC Template was written by Ward Bell</span></span>
> 
> [<span data-ttu-id="77846-106">Breeze/Angular 的 MVC 範本下載</span><span class="sxs-lookup"><span data-stu-id="77846-106">Download the Breeze/Angular MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=286437)


<span data-ttu-id="77846-107">[AngularJS](http://angularjs.org)是來自 Google 的開放原始碼程式庫來建置單一頁面應用程式 (Spa)。</span><span class="sxs-lookup"><span data-stu-id="77846-107">[AngularJS](http://angularjs.org) is an open source library from Google for building Single Page Applications (SPAs).</span></span> <span data-ttu-id="77846-108">它提供資料繫結、 相依性插入和畫面管理。</span><span class="sxs-lookup"><span data-stu-id="77846-108">It offers data binding, dependency injection, and screen management.</span></span> <span data-ttu-id="77846-109">搭配使用[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa)，適用於資料模型化和資料管理，而您的另一個開放原始碼程式庫有很棒的 HTML/JavaScript 用戶端應用程式不可或缺的要素。</span><span class="sxs-lookup"><span data-stu-id="77846-109">Combine it with [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), another open source library for data modeling and data management, and you have the essential ingredients for a great HTML/JavaScript client app.</span></span>

<span data-ttu-id="77846-110">Breeze/Angular SPA 範本上是一種變化[KnockoutJS SPA 範本](../introduction/knockoutjs-template.md)包含在 ASP.NET 和 Web 工具 2012.2 Update。</span><span class="sxs-lookup"><span data-stu-id="77846-110">The Breeze/Angular SPA template is a variation on the [KnockoutJS SPA template](../introduction/knockoutjs-template.md) included in the ASP.NET and Web Tools 2012.2 Update.</span></span> <span data-ttu-id="77846-111">如果您有 Visual Studio，您必須範例 SPA 啟動並執行在少於 60 秒。</span><span class="sxs-lookup"><span data-stu-id="77846-111">If you've got Visual Studio, you'll have an example SPA up and running in less than 60 seconds.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

<span data-ttu-id="77846-112">面向外部，應用程式看起來非常類似 KnockoutJS SPA 範本。</span><span class="sxs-lookup"><span data-stu-id="77846-112">Outwardly, the application looks the very similar to the KnockoutJS SPA template.</span></span> <span data-ttu-id="77846-113">但實際上相當不同。</span><span class="sxs-lookup"><span data-stu-id="77846-113">But it's quite different under the hood.</span></span> <span data-ttu-id="77846-114">KnockoutJS 範本使用 Knockout 來資料繫結和資料存取的未經處理的 AJAX。</span><span class="sxs-lookup"><span data-stu-id="77846-114">The KnockoutJS template uses Knockout for data binding and raw AJAX for data access.</span></span> <span data-ttu-id="77846-115">Breeze/Angular 範本會使用 Angular 資料繫結和幫助您輕鬆進行資料存取。</span><span class="sxs-lookup"><span data-stu-id="77846-115">The Breeze/Angular template uses Angular for data binding and Breeze for data access.</span></span> <span data-ttu-id="77846-116">這些庫啟用額外的功能，包括頁面導覽和歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="77846-116">These libaries enable additional capabilities, including page navigation and history.</span></span>

<span data-ttu-id="77846-117">以下是應用程式的 About 頁面：</span><span class="sxs-lookup"><span data-stu-id="77846-117">Here is the application's About page:</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

<span data-ttu-id="77846-118">此頁面會顯示執行記錄檔的事件，在目前的使用者工作階段，包括：</span><span class="sxs-lookup"><span data-stu-id="77846-118">This page displays a running log of events during the current user session, including:</span></span>

- <span data-ttu-id="77846-119">分頁。</span><span class="sxs-lookup"><span data-stu-id="77846-119">Paging.</span></span> <span data-ttu-id="77846-120">請注意在 #2 與快照 #7 在 Todo 控制站建立。</span><span class="sxs-lookup"><span data-stu-id="77846-120">Note the Todo controller creation at #2 and #7.</span></span>
- <span data-ttu-id="77846-121">遠端查詢 (3) 和本機快取查詢 (#7)。</span><span class="sxs-lookup"><span data-stu-id="77846-121">Remote queries (#3) and local cache queries (#7).</span></span>
- <span data-ttu-id="77846-122">儲存新的 （#5，6），修改 (#4) 的實體。</span><span class="sxs-lookup"><span data-stu-id="77846-122">Saving new (#5, #6) and modified (#4) entities.</span></span>
- <span data-ttu-id="77846-123">驗證用戶端 (#9)，讓使用者可以更正錯誤，然後再認可變更到資料庫的變更。</span><span class="sxs-lookup"><span data-stu-id="77846-123">Changes validated on the client (#9), so the user can correct mistakes before committing changes to the database.</span></span>

<span data-ttu-id="77846-124">沒有進一步探索此範本中，包括：</span><span class="sxs-lookup"><span data-stu-id="77846-124">There's more to explore in this template, including:</span></span>

- <span data-ttu-id="77846-125">動態 HTML 檢視範本的方式載入。</span><span class="sxs-lookup"><span data-stu-id="77846-125">Dynamic loading of HTML view templates.</span></span>
- <span data-ttu-id="77846-126">自訂資料繫結，透過 Angular"指示詞。 」</span><span class="sxs-lookup"><span data-stu-id="77846-126">Custom data binding through Angular "directives."</span></span>
- <span data-ttu-id="77846-127">模組化和相依性插入。</span><span class="sxs-lookup"><span data-stu-id="77846-127">Modularity and dependency injection.</span></span>
- <span data-ttu-id="77846-128">查詢篩選、 排序、 分頁、 投影和包含相關實體。</span><span class="sxs-lookup"><span data-stu-id="77846-128">Query filters, sorts, paging, projections, and inclusion of related entities.</span></span>
- <span data-ttu-id="77846-129">多個畫面間的資料共用。</span><span class="sxs-lookup"><span data-stu-id="77846-129">Sharing data across multiple screens.</span></span>
- <span data-ttu-id="77846-130">當做單一交易中儲存多個變更。</span><span class="sxs-lookup"><span data-stu-id="77846-130">Saving multiple changes as a single transaction.</span></span>
- <span data-ttu-id="77846-131">驗證規則會自動從伺服器傳送至 JavaScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="77846-131">Validation rules propagated automatically from the server to the JavaScript client.</span></span>

<span data-ttu-id="77846-132">我們現在就開始吧。</span><span class="sxs-lookup"><span data-stu-id="77846-132">Let's get started.</span></span>

## <a name="create-a-breezeangular-template-project"></a><span data-ttu-id="77846-133">建立 Breeze/Angular 範本專案</span><span class="sxs-lookup"><span data-stu-id="77846-133">Create a Breeze/Angular Template Project</span></span>

<span data-ttu-id="77846-134">下載並安裝的範本，按一下上方的 [下載] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="77846-134">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="77846-135">範本會封裝成 Visual Studio 擴充功能 (VSIX) 檔案。</span><span class="sxs-lookup"><span data-stu-id="77846-135">The template is packaged as a Visual Studio Extension (VSIX) file.</span></span> <span data-ttu-id="77846-136">您可能需要重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="77846-136">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="77846-137">在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。</span><span class="sxs-lookup"><span data-stu-id="77846-137">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="77846-138">底下**Visual C#**，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="77846-138">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="77846-139">在專案範本清單中，選取**ASP.NET MVC 4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="77846-139">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="77846-140">將專案命名，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="77846-140">Name the project and click **OK**.</span></span>

<span data-ttu-id="77846-141">在 **新的專案**精靈中，選取**幫助您輕鬆 Angular SPA**。</span><span class="sxs-lookup"><span data-stu-id="77846-141">In the **New Project** wizard, select **Breeze Angular SPA**.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

<span data-ttu-id="77846-142">按 Ctrl-F5 以建置並執行應用程式，但不偵錯，或按下 F5 以執行並偵錯。</span><span class="sxs-lookup"><span data-stu-id="77846-142">Press Ctrl-F5 to build and run the application without debugging, or press F5 to run with debugging.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

<span data-ttu-id="77846-143">當第一次執行應用程式時，它會顯示登入畫面。</span><span class="sxs-lookup"><span data-stu-id="77846-143">When the application first runs, it displays a login screen.</span></span> <span data-ttu-id="77846-144">按一下 「 註冊 」 連結，新的頁面 glides 到檢視中，您可以在其中輸入使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="77846-144">Click the "Sign up" link and a new page glides into view, where you can enter a username and password.</span></span> <span data-ttu-id="77846-145">（登入] 和 [註冊頁面會建置使用 ASP.NET MVC）。當您提交註冊表單時，伺服器就會產生 TodoList 與您帳戶的兩個項目。</span><span class="sxs-lookup"><span data-stu-id="77846-145">(The login and registration pages are built using ASP.NET MVC.) When you submit the registration form, the server generates a TodoList with two items for your account.</span></span> <span data-ttu-id="77846-146">然後它將它們呈現給您在黃色便箋上。</span><span class="sxs-lookup"><span data-stu-id="77846-146">Then it presents them to you on a yellow note.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

<span data-ttu-id="77846-147">現在您已經在 land 的 SPA。</span><span class="sxs-lookup"><span data-stu-id="77846-147">Now you are in the land of SPA.</span></span> <span data-ttu-id="77846-148">所有項目您查看和體驗而操作 Todo 是呈現和管理的 Knockout 和 Breeze 協助用戶端上。</span><span class="sxs-lookup"><span data-stu-id="77846-148">Everything you see and experience while manipulating Todos is rendered and managed on the client with the help of Knockout and Breeze.</span></span> <span data-ttu-id="77846-149">探索應用程式的使用者身分...</span><span class="sxs-lookup"><span data-stu-id="77846-149">Explore the app as a user …</span></span> <span data-ttu-id="77846-150">但是開發人員的注意。</span><span class="sxs-lookup"><span data-stu-id="77846-150">but with a developer's eye.</span></span> <span data-ttu-id="77846-151">使用瀏覽器中的開發人員工具，來擷取網路流量。</span><span class="sxs-lookup"><span data-stu-id="77846-151">Use the developer tools in your browser to capture the network traffic.</span></span> <span data-ttu-id="77846-152">(在 Internet Explorer： 按 f12 鍵，選取**網路**索引標籤，然後按一下**開始擷取**。)現在請嘗試下列各項：</span><span class="sxs-lookup"><span data-stu-id="77846-152">(In Internet Explorer: Press F12, select the **Network** tab, and click **Start capturing**.) Now try the following:</span></span>

- <span data-ttu-id="77846-153">加入新的 Todo 項目。</span><span class="sxs-lookup"><span data-stu-id="77846-153">Add a new Todo item.</span></span>
- <span data-ttu-id="77846-154">按一下標籤，然後編輯待辦事項項目標題</span><span class="sxs-lookup"><span data-stu-id="77846-154">Click the label and edit the Todo item title</span></span>
- <span data-ttu-id="77846-155">核取方塊來標記項目完成。</span><span class="sxs-lookup"><span data-stu-id="77846-155">Check a checkbox to mark the item done.</span></span> <span data-ttu-id="77846-156">請注意，已停用的文字方塊中，因此不再是可編輯的標題。</span><span class="sxs-lookup"><span data-stu-id="77846-156">Notice that the textbox is disabled, so the title is no longer editable.</span></span>
- <span data-ttu-id="77846-157">按一下 'x' 標籤的右邊。</span><span class="sxs-lookup"><span data-stu-id="77846-157">Click the ‘x' to the right of the label.</span></span> <span data-ttu-id="77846-158">項目會消失，而從資料庫中刪除。</span><span class="sxs-lookup"><span data-stu-id="77846-158">The item disappears and is deleted from the database.</span></span>
- <span data-ttu-id="77846-159">挑選另一個項目，並清除它的標題。</span><span class="sxs-lookup"><span data-stu-id="77846-159">Pick another item and clear its title.</span></span> <span data-ttu-id="77846-160">您會收到標題是必要的驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="77846-160">You'll get a validation error that the title is required.</span></span> <span data-ttu-id="77846-161">在短暫的暫停之後, 就會還原先前的標題。</span><span class="sxs-lookup"><span data-stu-id="77846-161">After a brief pause, the previous title is restored.</span></span>
- <span data-ttu-id="77846-162">輸入在非常長的標題。</span><span class="sxs-lookup"><span data-stu-id="77846-162">Type in a ridiculously long title.</span></span> <span data-ttu-id="77846-163">您會收到不同的驗證錯誤，標題會太長。</span><span class="sxs-lookup"><span data-stu-id="77846-163">You'll get a different validation error that the title is too long.</span></span>
- <span data-ttu-id="77846-164">按一下 「 新增待辦事項清單 」 按鈕。</span><span class="sxs-lookup"><span data-stu-id="77846-164">Click the "Add Todo List" button.</span></span> <span data-ttu-id="77846-165">左側的前一份清單會出現新的清單。</span><span class="sxs-lookup"><span data-stu-id="77846-165">A new list appears to the left of the previous list.</span></span>
- <span data-ttu-id="77846-166">播放以 TodoList 標題，觸發其必要和長度驗證。</span><span class="sxs-lookup"><span data-stu-id="77846-166">Play with the TodoList title, triggering its required and length validations.</span></span>
- <span data-ttu-id="77846-167">按一下以清除錯誤訊息的 [標題] 文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="77846-167">Click in the title textbox to clear the error message.</span></span>
- <span data-ttu-id="77846-168">按一下 [x] 來刪除 TodoList 和其 todo 右上角圓圈。</span><span class="sxs-lookup"><span data-stu-id="77846-168">Click the "x" in the circle in the upper right corner to delete the TodoList and its todos.</span></span>
- <span data-ttu-id="77846-169">按一下右上方，若要查看這些活動的記錄檔上的 「 關於 」 連結。</span><span class="sxs-lookup"><span data-stu-id="77846-169">Click the "About" link in the upper right to see a log of these activities.</span></span>

<span data-ttu-id="77846-170">驗證邏輯是由幫助您輕鬆的執行的用戶端。</span><span class="sxs-lookup"><span data-stu-id="77846-170">The validation logic is performed client-side by Breeze.</span></span> <span data-ttu-id="77846-171">在伺服器的模型類別上的驗證屬性會傳播至用戶端，並自動執行前用戶端會聯繫伺服器。</span><span class="sxs-lookup"><span data-stu-id="77846-171">Validation attributes on the server model classes are propagated to the client and executed automatically before the client contacts the server.</span></span>

<span data-ttu-id="77846-172">檢閱網路流量。</span><span class="sxs-lookup"><span data-stu-id="77846-172">Review the network traffic.</span></span> <span data-ttu-id="77846-173">請注意，任何對伺服器的呼叫時沒有幫助您輕鬆偵測到錯誤。</span><span class="sxs-lookup"><span data-stu-id="77846-173">Notice that there were no calls to the server when Breeze detected an error.</span></span> <span data-ttu-id="77846-174">每個有效的變更會導致 POST 要求為"/ api/Todo/SaveChanges"。</span><span class="sxs-lookup"><span data-stu-id="77846-174">Each valid change resulted in a POST request to "/api/Todo/SaveChanges".</span></span> <span data-ttu-id="77846-175">幫助您輕鬆組合所做的變更並將其傳送一起做為單一要求 Web API 控制器的`SaveChanges`方法。</span><span class="sxs-lookup"><span data-stu-id="77846-175">Breeze bundles the changes and sends them together as a single request to the Web API controller's `SaveChanges` method.</span></span> <span data-ttu-id="77846-176">這是從 KockoutJS SPA 範本，可讓 PUT、 POST 和 DELETE 要求每個項目的個別不同。</span><span class="sxs-lookup"><span data-stu-id="77846-176">That's different from KockoutJS SPA template, which makes PUT, POST, and DELETE requests for each item individually.</span></span>

<span data-ttu-id="77846-177">此外，請注意沒有網路流量切換 TodoList 之間，以及有關頁面時。</span><span class="sxs-lookup"><span data-stu-id="77846-177">Also, notice there is no network traffic when you switch between the TodoList and About pages.</span></span> <span data-ttu-id="77846-178">這是因為查詢限定於本機的幫助您輕鬆快取。</span><span class="sxs-lookup"><span data-stu-id="77846-178">That's because the query has been constrained to the local Breeze cache.</span></span>

## <a name="peek-inside"></a><span data-ttu-id="77846-179">眼</span><span class="sxs-lookup"><span data-stu-id="77846-179">Peek inside</span></span>

<span data-ttu-id="77846-180">此應用程式具有用戶端和伺服器端。</span><span class="sxs-lookup"><span data-stu-id="77846-180">This application has a client side and a server side.</span></span> <span data-ttu-id="77846-181">用戶端堆疊一些 HTML 和 （在 「 應用程式 」 資料夾中） 的應用程式 JavaScript 模組的組合加上組成第三方 JavaScript 程式庫 （在 「 指令碼 」 資料夾中）。</span><span class="sxs-lookup"><span data-stu-id="77846-181">The client-side stack consists of a little HTML and a combination of application JavaScript modules (in the "app" folder) plus third-party JavaScript libraries (in the "Scripts" folder).</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

<span data-ttu-id="77846-182">UI 架構會將檢視的 HTML widget 分開的控制器中支援的展示程式碼。</span><span class="sxs-lookup"><span data-stu-id="77846-182">The UI architecture separates the HTML widgets of the views from the supporting presentation code in the controllers.</span></span> <span data-ttu-id="77846-183">Angular 的資料繫結系統會協調，讓每個可以執行其工作，而不需要深入了解其他檢視和控制器。</span><span class="sxs-lookup"><span data-stu-id="77846-183">The Angular data-binding system coordinates views and controllers so that each can do its job without intimate knowledge of the other.</span></span>

<span data-ttu-id="77846-184">控制器會要求要取得和儲存模型實體的資料內容。</span><span class="sxs-lookup"><span data-stu-id="77846-184">The controller asks the data context to acquire and save the model entities.</span></span> <span data-ttu-id="77846-185">資料內容會將大部分的工作變得輕而易舉，建構 JSON 查詢結果中的自我追蹤模型物件的委派。</span><span class="sxs-lookup"><span data-stu-id="77846-185">The data context delegates most of the work to Breeze, which constructs self-tracking model objects from JSON query results.</span></span>

<span data-ttu-id="77846-186">伺服器端堆疊包含一些開發人員程式碼和三個主要.NET 程式庫： Web API、 Entity Framework 和 Breeze.NET:</span><span class="sxs-lookup"><span data-stu-id="77846-186">The server-side stack consists of some developer code and three principle .NET libraries: Web API, Entity Framework, and Breeze.NET:</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

<span data-ttu-id="77846-187">基本架構是 KockoutJS SPA 範本相同。</span><span class="sxs-lookup"><span data-stu-id="77846-187">The basic architecture is the same as the KockoutJS SPA template.</span></span> <span data-ttu-id="77846-188">不過，實作是更簡單： 已刪除的 Dto，和大部分的 Entity Framework 細節已委派給 Breeze.NET。</span><span class="sxs-lookup"><span data-stu-id="77846-188">However, the implementation is much simpler: The DTOs were deleted, and most of the Entity Framework details have been delegated to Breeze.NET.</span></span>

## <a name="next-steps"></a><span data-ttu-id="77846-189">後續步驟</span><span class="sxs-lookup"><span data-stu-id="77846-189">Next Steps</span></span>

<span data-ttu-id="77846-190">我們建議您瀏覽程式碼，藉由指引[廣泛討論](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa)的用戶端和伺服器堆疊，幫助您輕鬆網站上的。</span><span class="sxs-lookup"><span data-stu-id="77846-190">We suggest that you explore the code, guided by the [extensive discussion](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) of both the client and the server stacks on the Breeze website.</span></span>

<span data-ttu-id="77846-191">您可以嘗試播放使用變得輕而易舉的用戶端查詢;加入一些篩選和排序。</span><span class="sxs-lookup"><span data-stu-id="77846-191">You might try playing with Breeze client-side query; add some filters and sorts.</span></span> <span data-ttu-id="77846-192">您可以加入更多的模型屬性並取得端對端 SPA 開發更加了解多個實體。</span><span class="sxs-lookup"><span data-stu-id="77846-192">You might add more model properties and more entities to get a better feel for end-to-end SPA development.</span></span> <span data-ttu-id="77846-193">自信的設計時，您可以卸除的 Todo 功能，並取代它們。</span><span class="sxs-lookup"><span data-stu-id="77846-193">When you are confident of the design, you can tear out the Todo features and replace them with your own.</span></span>

<span data-ttu-id="77846-194">祝各位寫程式愉快！</span><span class="sxs-lookup"><span data-stu-id="77846-194">Happy coding!</span></span>
