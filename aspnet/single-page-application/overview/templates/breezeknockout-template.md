---
uid: single-page-application/overview/templates/breezeknockout-template
title: 幫助您輕鬆/Knockout 範本 |Microsoft 文件
author: madskristensen
description: 幫助您輕鬆/Knockout 單一網頁應用程式範本
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 07ec099a0381458fe42c1972a2554f76fd34638c
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "26506697"
---
<a name="breezeknockout-template"></a><span data-ttu-id="85167-103">幫助您輕鬆/Knockout 範本</span><span class="sxs-lookup"><span data-stu-id="85167-103">Breeze/Knockout template</span></span>
====================
<span data-ttu-id="85167-104">由[Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="85167-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="85167-105">幫助您輕鬆/Knockout MVC 範本是由行政區鈴鐺寫入</span><span class="sxs-lookup"><span data-stu-id="85167-105">The Breeze/Knockout MVC Template was written by Ward Bell</span></span>
> 
> [<span data-ttu-id="85167-106">下載幫助您輕鬆/Knockout MVC 範本</span><span class="sxs-lookup"><span data-stu-id="85167-106">Download the Breeze/Knockout MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282649)


<span data-ttu-id="85167-107">聽說的 「 單一網頁應用程式 」 (SPA) 和不想知道它是什麼。</span><span class="sxs-lookup"><span data-stu-id="85167-107">You've heard of "single page application" (SPA) and wondered what it is.</span></span> <span data-ttu-id="85167-108">雖然您無法讀取其相關資訊，您會自行而遇到它。</span><span class="sxs-lookup"><span data-stu-id="85167-108">While you could read about it, you'd rather experience it for yourself.</span></span> <span data-ttu-id="85167-109">但是，誰有時間，若要下載範例？</span><span class="sxs-lookup"><span data-stu-id="85167-109">But who has time to download a sample?</span></span> <span data-ttu-id="85167-110">如果您已有 Visual Studio，您將最多有範例 SPA 和執行在少於 60 秒為單位搭配 ASP.NET MVC 4 「 幫助您輕鬆/Knockout 單一頁面應用程式 」 範本。</span><span class="sxs-lookup"><span data-stu-id="85167-110">If you've got Visual Studio, you'll have an example SPA up and running in less than 60 seconds with the ASP.NET MVC 4 "Breeze/Knockout Single Page Application" template.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a><span data-ttu-id="85167-111">什麼是幫助您輕鬆/Knockout SPA 範本？</span><span class="sxs-lookup"><span data-stu-id="85167-111">What is the Breeze/Knockout SPA Template?</span></span>

<span data-ttu-id="85167-112">大部分的專案範本產生的應用程式的基本架構。</span><span class="sxs-lookup"><span data-stu-id="85167-112">Most project templates generate an application skeleton.</span></span> <span data-ttu-id="85167-113">這些添 flesh 打加入您的程式碼，並最終傳遞應用程式運作。</span><span class="sxs-lookup"><span data-stu-id="85167-113">You put flesh on those bones by adding your code and eventually deliver a working application.</span></span> <span data-ttu-id="85167-114">幫助您輕鬆/Knockout SPA 範本會不同。</span><span class="sxs-lookup"><span data-stu-id="85167-114">The Breeze/Knockout SPA template is different.</span></span> <span data-ttu-id="85167-115">它會產生要研究的範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="85167-115">It generates a sample application for you to study.</span></span> <span data-ttu-id="85167-116">它會示範 SPA 應用程式設計和許多建置 SPA 的技術。</span><span class="sxs-lookup"><span data-stu-id="85167-116">It demonstrates a SPA application design and many of the techniques for building a SPA.</span></span>

<span data-ttu-id="85167-117">幫助您輕鬆/Knockout 範本上是一種變化[KnockoutJS SPA 範本](../introduction/knockoutjs-template.md)包含在 ASP.NET 和 Web 工具 2012.2 更新。</span><span class="sxs-lookup"><span data-stu-id="85167-117">The Breeze/Knockout template is a variation on the [KnockoutJS SPA template](../introduction/knockoutjs-template.md) included in the ASP.NET and Web Tools 2012.2 Update.</span></span> <span data-ttu-id="85167-118">幫助您輕鬆 SPA 範本會產生具有相同的使用者經驗的應用程式，但有不同的實作，幫助您輕鬆使用資料管理。</span><span class="sxs-lookup"><span data-stu-id="85167-118">The Breeze SPA template generates an application with the same user experience, but it has a different implementation, using Breeze for data management.</span></span>

<span data-ttu-id="85167-119">KnockoutJS SPA 範本可讓服務要求未經處理的 jQuery AJAX，即適合簡單的應用程式。</span><span class="sxs-lookup"><span data-stu-id="85167-119">The KnockoutJS SPA template makes service requests with raw jQuery AJAX, which is adequate for a simple application.</span></span> <span data-ttu-id="85167-120">但更複雜的應用程式有較嚴苛的資料管理需求。</span><span class="sxs-lookup"><span data-stu-id="85167-120">But more sophisticated apps have more demanding data management requirements.</span></span> <span data-ttu-id="85167-121">例如，大部分的應用程式：</span><span class="sxs-lookup"><span data-stu-id="85167-121">For example, most applications:</span></span>

- <span data-ttu-id="85167-122">查詢並重新擴充的使用者工作階段期間查詢伺服器。</span><span class="sxs-lookup"><span data-stu-id="85167-122">Query and re-query the server during an extended user session.</span></span>
- <span data-ttu-id="85167-123">加入查詢的篩選、 排序和分頁。</span><span class="sxs-lookup"><span data-stu-id="85167-123">Add query filters, sorting, and paging.</span></span>
- <span data-ttu-id="85167-124">跨多個螢幕共用相同的資料。</span><span class="sxs-lookup"><span data-stu-id="85167-124">Share the same data across multiple screens.</span></span>
- <span data-ttu-id="85167-125">累積變更許多物件，然後將它們儲存為單一交易。</span><span class="sxs-lookup"><span data-stu-id="85167-125">Accumulate changes to many objects, then save them as a single transaction.</span></span>
- <span data-ttu-id="85167-126">驗證用戶端上的變更，因此使用者可以更正錯誤，再認可變更到資料庫。</span><span class="sxs-lookup"><span data-stu-id="85167-126">Validate changes on the client, so the user can correct mistakes before committing changes to the database.</span></span>

<span data-ttu-id="85167-127">BreezeJS 程式庫會為您釋出您開發最重要的應用程式邏輯和使用者體驗，來處理這些乏味的雜務。</span><span class="sxs-lookup"><span data-stu-id="85167-127">The BreezeJS library handles these chores for you, freeing you to develop the application logic and user experience that matter most.</span></span>

<span data-ttu-id="85167-128">[**幫助您輕鬆**](http://www.breezejs.com/?utm_source=ms-spa)是開放原始碼程式庫建置豐富的資料以 JavaScript 和 HTML 的應用程式在過去已傳遞做為獨立的桌面應用程式類型的應用程式。</span><span class="sxs-lookup"><span data-stu-id="85167-128">[**Breeze**](http://www.breezejs.com/?utm_source=ms-spa) is an open source library for building rich data applications in JavaScript and HTML, the kinds of apps that have historically been delivered as stand-alone desktop applications.</span></span>

<span data-ttu-id="85167-129">幫助您輕鬆/Knockout 範本可協助您採取更穩固的資料管理基礎結構非常重要的首要步驟。</span><span class="sxs-lookup"><span data-stu-id="85167-129">The Breeze/Knockout template helps you take that first crucial step toward a more robust data management infrastructure.</span></span> <span data-ttu-id="85167-130">它會產生等同於面向外部 KnockoutJS SPA 範本範例待辦事項應用程式。</span><span class="sxs-lookup"><span data-stu-id="85167-130">It produces a sample Todo application that is outwardly identical to the KnockoutJS SPA template.</span></span> <span data-ttu-id="85167-131">在內部，它會以幫助您輕鬆取代 AJAX 資料層，以便比較兩個方法來並行。</span><span class="sxs-lookup"><span data-stu-id="85167-131">On the inside, it replaces the AJAX data layer with Breeze, so you can compare the two approaches side-by-side.</span></span> <span data-ttu-id="85167-132">當然，幾乎沒有碰觸到可能會幫助您輕鬆的應用程式。</span><span class="sxs-lookup"><span data-stu-id="85167-132">Of course, it barely touches the potential of a Breeze application.</span></span> <span data-ttu-id="85167-133">不過您看到輕而易舉的運作方式以及如何稍有需要，才能將該轉換。</span><span class="sxs-lookup"><span data-stu-id="85167-133">But you'll see how Breeze works and how little is required to make that transition.</span></span>

<span data-ttu-id="85167-134">我們現在就開始吧。</span><span class="sxs-lookup"><span data-stu-id="85167-134">Let's get started.</span></span>

## <a name="create-a-breezeknockout-template-project"></a><span data-ttu-id="85167-135">建立幫助您輕鬆/Knockout 範本專案</span><span class="sxs-lookup"><span data-stu-id="85167-135">Create a Breeze/Knockout Template Project</span></span>

<span data-ttu-id="85167-136">下載並安裝的範本，按一下下載按鈕上方。</span><span class="sxs-lookup"><span data-stu-id="85167-136">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="85167-137">範本會封裝成 Visual Studio 擴充功能 (VSIX) 檔案。</span><span class="sxs-lookup"><span data-stu-id="85167-137">The template is packaged as a Visual Studio Extension (VSIX) file.</span></span> <span data-ttu-id="85167-138">您可能需要重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="85167-138">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="85167-139">在**範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。</span><span class="sxs-lookup"><span data-stu-id="85167-139">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="85167-140">在下**Visual C#**，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="85167-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="85167-141">在專案範本清單中選取**ASP.NET MVC 4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="85167-141">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="85167-142">為專案名稱，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="85167-142">Name the project and click **OK**.</span></span>

<span data-ttu-id="85167-143">在**新專案**精靈中，選取**幫助您輕鬆 Knockout SPA**。</span><span class="sxs-lookup"><span data-stu-id="85167-143">In the **New Project** wizard, select **Breeze Knockout SPA**.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

<span data-ttu-id="85167-144">按 Ctrl + F5 以建置並執行應用程式，但不偵錯，或按 f5 開始執行並偵錯。</span><span class="sxs-lookup"><span data-stu-id="85167-144">Press Ctrl-F5 to build and run the application without debugging, or press F5 to run with debugging.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

<span data-ttu-id="85167-145">當第一次執行應用程式時，它會顯示登入畫面。</span><span class="sxs-lookup"><span data-stu-id="85167-145">When the application first runs, it displays a login screen.</span></span> <span data-ttu-id="85167-146">按一下 「 註冊 」 連結，新的頁面 glides 到檢視，您可以在其中輸入使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="85167-146">Click the "Sign up" link and a new page glides into view, where you can enter a username and password.</span></span> <span data-ttu-id="85167-147">（登入和註冊網頁會建置使用 ASP.NET MVC）。當您提交註冊表單時，伺服器會產生 TodoList 與您帳戶的兩個項目。</span><span class="sxs-lookup"><span data-stu-id="85167-147">(The login and registration pages are built using ASP.NET MVC.) When you submit the registration form, the server generates a TodoList with two items for your account.</span></span> <span data-ttu-id="85167-148">然後它呈現給您一個黃色的提示。</span><span class="sxs-lookup"><span data-stu-id="85167-148">Then it presents them to you on a yellow note.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

<span data-ttu-id="85167-149">現在您已在土地的 SPA。</span><span class="sxs-lookup"><span data-stu-id="85167-149">Now you are in the land of SPA.</span></span> <span data-ttu-id="85167-150">所有項目您會看到及體驗時操作 Todos 轉譯，而且藉助 Knockout 和輕而易舉的用戶端上管理。</span><span class="sxs-lookup"><span data-stu-id="85167-150">Everything you see and experience while manipulating Todos is rendered and managed on the client with the help of Knockout and Breeze.</span></span> <span data-ttu-id="85167-151">探索應用程式的使用者身分...</span><span class="sxs-lookup"><span data-stu-id="85167-151">Explore the app as a user …</span></span> <span data-ttu-id="85167-152">但與開發人員的紅眼。</span><span class="sxs-lookup"><span data-stu-id="85167-152">but with a developer's eye.</span></span> <span data-ttu-id="85167-153">使用瀏覽器中的開發人員工具，來擷取網路流量。</span><span class="sxs-lookup"><span data-stu-id="85167-153">Use the developer tools in your browser to capture the network traffic.</span></span> <span data-ttu-id="85167-154">(在 Internet Explorer： 按 F12，選取**網路**索引標籤，然後按一下**開始擷取**。)現在請嘗試下列各項：</span><span class="sxs-lookup"><span data-stu-id="85167-154">(In Internet Explorer: Press F12, select the **Network** tab, and click **Start capturing**.) Now try the following:</span></span>

- <span data-ttu-id="85167-155">加入新的 Todo 項目。</span><span class="sxs-lookup"><span data-stu-id="85167-155">Add a new Todo item.</span></span>
- <span data-ttu-id="85167-156">按一下標籤，然後編輯 Todo 項目標題</span><span class="sxs-lookup"><span data-stu-id="85167-156">Click the label and edit the Todo item title</span></span>
- <span data-ttu-id="85167-157">請檢查的核取方塊，來標記項目完成。</span><span class="sxs-lookup"><span data-stu-id="85167-157">Check a checkbox to mark the item done.</span></span> <span data-ttu-id="85167-158">請注意，文字方塊將會停用，因此不再是可編輯的標題。</span><span class="sxs-lookup"><span data-stu-id="85167-158">Notice that the textbox is disabled, so the title is no longer editable.</span></span>
- <span data-ttu-id="85167-159">按一下右邊的標籤 'x'。</span><span class="sxs-lookup"><span data-stu-id="85167-159">Click the ‘x' to the right of the label.</span></span> <span data-ttu-id="85167-160">項目就會消失，並且從資料庫中刪除。</span><span class="sxs-lookup"><span data-stu-id="85167-160">The item disappears and is deleted from the database.</span></span>
- <span data-ttu-id="85167-161">挑選另一個項目，並清除其標題。</span><span class="sxs-lookup"><span data-stu-id="85167-161">Pick another item and clear its title.</span></span> <span data-ttu-id="85167-162">您會取得的標題是必要的驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="85167-162">You'll get a validation error that the title is required.</span></span> <span data-ttu-id="85167-163">短暫的暫停之後, 就會還原先前的標題。</span><span class="sxs-lookup"><span data-stu-id="85167-163">After a brief pause, the previous title is restored.</span></span>
- <span data-ttu-id="85167-164">輸入將為非常長的標題。</span><span class="sxs-lookup"><span data-stu-id="85167-164">Type in a ridiculously long title.</span></span> <span data-ttu-id="85167-165">您會取得不同的驗證錯誤的標題是太長。</span><span class="sxs-lookup"><span data-stu-id="85167-165">You'll get a different validation error that the title is too long.</span></span>
- <span data-ttu-id="85167-166">按一下 加入 Todo 清單 」 按鈕。</span><span class="sxs-lookup"><span data-stu-id="85167-166">Click the "Add Todo List" button.</span></span> <span data-ttu-id="85167-167">上述清單的左邊會出現新的清單。</span><span class="sxs-lookup"><span data-stu-id="85167-167">A new list appears to the left of the previous list.</span></span>
- <span data-ttu-id="85167-168">播放以 TodoList 標題，觸發其必要和長度驗證。</span><span class="sxs-lookup"><span data-stu-id="85167-168">Play with the TodoList title, triggering its required and length validations.</span></span>
- <span data-ttu-id="85167-169">按一下 [標題] 文字方塊中，若要清除的錯誤訊息中。</span><span class="sxs-lookup"><span data-stu-id="85167-169">Click in the title textbox to clear the error message.</span></span>
- <span data-ttu-id="85167-170">按一下右上角刪除 TodoList 和其 todos 圓形中的"x"。</span><span class="sxs-lookup"><span data-stu-id="85167-170">Click the "x" in the circle in the upper right corner to delete the TodoList and its todos.</span></span>

<span data-ttu-id="85167-171">驗證邏輯是由輕而易舉的執行的用戶端。</span><span class="sxs-lookup"><span data-stu-id="85167-171">The validation logic is performed client-side by Breeze.</span></span> <span data-ttu-id="85167-172">伺服器模型類別上的驗證屬性傳播至用戶端，而且用戶端連絡伺服器之前，自動執行。</span><span class="sxs-lookup"><span data-stu-id="85167-172">Validation attributes on the server model classes are propagated to the client and executed automatically before the client contacts the server.</span></span>

<span data-ttu-id="85167-173">檢閱網路流量。</span><span class="sxs-lookup"><span data-stu-id="85167-173">Review the network traffic.</span></span> <span data-ttu-id="85167-174">請注意，沒有呼叫伺服器時沒有幫助您輕鬆偵測到錯誤。</span><span class="sxs-lookup"><span data-stu-id="85167-174">Notice that there were no calls to the server when Breeze detected an error.</span></span> <span data-ttu-id="85167-175">每個有效的變更會導致 POST 要求以"/ api/Todo/SaveChanges"。</span><span class="sxs-lookup"><span data-stu-id="85167-175">Each valid change resulted in a POST request to "/api/Todo/SaveChanges".</span></span> <span data-ttu-id="85167-176">幫助您輕鬆包裹在一起所做的變更並將它們傳送一起做為單一要求至 Web API 控制器`SaveChanges`方法。</span><span class="sxs-lookup"><span data-stu-id="85167-176">Breeze bundles the changes and sends them together as a single request to the Web API controller's `SaveChanges` method.</span></span> <span data-ttu-id="85167-177">這不同於 KockoutJS SPA 範本，可讓 PUT、 POST 和個別刪除要求每個項目。</span><span class="sxs-lookup"><span data-stu-id="85167-177">That's different from KockoutJS SPA template, which makes PUT, POST, and DELETE requests for each item individually.</span></span>

## <a name="peek-inside"></a><span data-ttu-id="85167-178">查看內部</span><span class="sxs-lookup"><span data-stu-id="85167-178">Peek inside</span></span>

<span data-ttu-id="85167-179">此應用程式具有用戶端和伺服器端。</span><span class="sxs-lookup"><span data-stu-id="85167-179">This application has a client side and a server side.</span></span> <span data-ttu-id="85167-180">用戶端堆疊包含極小的 HTML 和 （在 「 應用程式 」 資料夾中） 的應用程式 JavaScript 模組的組合以及協力廠商的 JavaScript 程式庫 （在 「 指令碼 」 資料夾中）。</span><span class="sxs-lookup"><span data-stu-id="85167-180">The client-side stack consists of a little HTML and a combination of application JavaScript modules (in the "app" folder) plus third-party JavaScript libraries (in the "Scripts" folder).</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

<span data-ttu-id="85167-181">如果您已經調查 KnockoutJS SPA 範本，這應該看起來很類似。</span><span class="sxs-lookup"><span data-stu-id="85167-181">If you've investigated the KnockoutJS SPA template, this should look very familiar.</span></span> <span data-ttu-id="85167-182">焦點放在藍色方塊。</span><span class="sxs-lookup"><span data-stu-id="85167-182">Focus on the blue boxes.</span></span> <span data-ttu-id="85167-183">UI 架構是模型-檢視-ViewModel (MVVM)，在其中檢視的 HTML widget 會完全支援簡報程式碼分開中檢視模型。</span><span class="sxs-lookup"><span data-stu-id="85167-183">The UI architecture is Model-View-ViewModel (MVVM), in which the HTML widgets of the view are cleanly separated from the supporting presentation code in the view-model.</span></span> <span data-ttu-id="85167-184">資料繫結系統 (在此情況下 Knockout) 協調的檢視和檢視模型，讓每個可以執行其工作的其他使用者不知情的情況下。</span><span class="sxs-lookup"><span data-stu-id="85167-184">A data binding system (Knockout in this case) coordinates the view and view-model so that each can do its job without intimate knowledge of the other.</span></span>

<span data-ttu-id="85167-185">模型封裝 Todo 資料。</span><span class="sxs-lookup"><span data-stu-id="85167-185">The model encapsulates the Todo data.</span></span> <span data-ttu-id="85167-186">模型中的實體可以進行建構幫助您輕鬆使用 Knockout 可觀察的屬性，讓它們可以直接繫結至檢視中的 widget。</span><span class="sxs-lookup"><span data-stu-id="85167-186">Entities in the model are constructed by Breeze with Knockout observable properties, so they can be bound directly to widgets in the view.</span></span> <span data-ttu-id="85167-187">檢視模型會要求來取得和儲存模型實體的資料內容。</span><span class="sxs-lookup"><span data-stu-id="85167-187">The view-model asks the data context to acquire and save the model entities.</span></span> <span data-ttu-id="85167-188">資料內容會委派來幫助您輕鬆工作的絕大部分。</span><span class="sxs-lookup"><span data-stu-id="85167-188">The data context delegates most of the work to Breeze.</span></span>

<span data-ttu-id="85167-189">伺服器端堆疊包含某些開發人員程式碼和三個原則.NET 程式庫： Web API、 Entity Framework 和 Breeze.NET:</span><span class="sxs-lookup"><span data-stu-id="85167-189">The server-side stack consists of some developer code and three principle .NET libraries: Web API, Entity Framework, and Breeze.NET:</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

<span data-ttu-id="85167-190">基本架構時 KockoutJS SPA 範本相同。</span><span class="sxs-lookup"><span data-stu-id="85167-190">The basic architecture is the same as the KockoutJS SPA template.</span></span> <span data-ttu-id="85167-191">不過，實作會更簡單： DTOs 已刪除，而且大部分的 Entity Framework 的詳細資料已委派給 Breeze.NET。</span><span class="sxs-lookup"><span data-stu-id="85167-191">However, the implementation is much simpler: The DTOs were deleted, and most of the Entity Framework details have been delegated to Breeze.NET.</span></span>

## <a name="next-steps"></a><span data-ttu-id="85167-192">後續步驟</span><span class="sxs-lookup"><span data-stu-id="85167-192">Next Steps</span></span>

<span data-ttu-id="85167-193">我們建議您瀏覽程式碼中，由[廣泛的討論](http://www.breezejs.com/spa-template?utm_source=ms-spa)用戶端和伺服器堆疊，幫助您輕鬆網站上的。</span><span class="sxs-lookup"><span data-stu-id="85167-193">We suggest that you explore the code, guided by the [extensive discussion](http://www.breezejs.com/spa-template?utm_source=ms-spa) of both the client and the server stacks on the Breeze website.</span></span>

<span data-ttu-id="85167-194">您可能會嘗試播放使用輕而易舉的用戶端查詢;加入一些篩選和排序。</span><span class="sxs-lookup"><span data-stu-id="85167-194">You might try playing with Breeze client-side query; add some filters and sorts.</span></span> <span data-ttu-id="85167-195">您可以加入更多的模型屬性，以取得更佳的感覺端對端 SPA 程式開發的多個實體。</span><span class="sxs-lookup"><span data-stu-id="85167-195">You might add more model properties and more entities to get a better feel for end-to-end SPA development.</span></span> <span data-ttu-id="85167-196">當您確信設計的時您可以卸除 Todo 功能，並取代它們。</span><span class="sxs-lookup"><span data-stu-id="85167-196">When you are confident of the design, you can tear out the Todo features and replace them with your own.</span></span>

<span data-ttu-id="85167-197">很快就可以輕鬆進行下一個大步驟： 加入用戶端螢幕，並在它們之間巡覽。</span><span class="sxs-lookup"><span data-stu-id="85167-197">Soon you'll be ready for the next big step: Adding client-side screens and navigating among them.</span></span> <span data-ttu-id="85167-198">您將留下此 SPA 範本，並開啟選項，以更完整的 SPA 堆疊，例如[John Papa 熱毛巾](https://github.com/johnpapa/HotTowel#readme "熱毛巾")，這樣會將 Durandal 十分簡單和 Knockout 混合加入。</span><span class="sxs-lookup"><span data-stu-id="85167-198">You'll leave this SPA template behind and turn to a more complete SPA stack, such as [John Papa's Hot Towel](https://github.com/johnpapa/HotTowel#readme "Hot Towel"), which adds Durandal to the Breeze and Knockout mix.</span></span>
