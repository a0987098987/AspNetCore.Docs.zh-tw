---
uid: single-page-application/overview/templates/backbonejs-template
title: Backbone 範本 |Microsoft Docs
author: madskristensen
description: Backbone.js SPA 範本
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 641e149155fbee2655024bec3b76dce5243e7d59
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362501"
---
<a name="backbone-template"></a><span data-ttu-id="da081-103">Backbone 範本</span><span class="sxs-lookup"><span data-stu-id="da081-103">Backbone Template</span></span>
====================
<span data-ttu-id="da081-104">藉由[Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="da081-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="da081-105">Kazi Manzur Rashid 寫入骨幹 SPA 範本</span><span class="sxs-lookup"><span data-stu-id="da081-105">The Backbone SPA Template was written by Kazi Manzur Rashid</span></span>
> 
> [<span data-ttu-id="da081-106">下載 Backbone.js SPA 範本</span><span class="sxs-lookup"><span data-stu-id="da081-106">Download the Backbone.js SPA Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=293631)


<span data-ttu-id="da081-107">Backbone.js SPA 範本設計來協助您開始快速建置互動式用戶端 web 應用程式使用[Backbone.js。](http://backbonejs.org/)</span><span class="sxs-lookup"><span data-stu-id="da081-107">The Backbone.js SPA template is designed to get you started quickly building interactive client-side web apps using [Backbone.js.](http://backbonejs.org/)</span></span>

<span data-ttu-id="da081-108">此範本會針對開發 ASP.NET MVC 中的 Backbone.js 應用程式提供初始的基本架構。</span><span class="sxs-lookup"><span data-stu-id="da081-108">The template provides an initial skeleton for developing a Backbone.js application in ASP.NET MVC.</span></span> <span data-ttu-id="da081-109">根據預設，它會提供基本的使用者登入功能，包括使用者註冊、 登入的密碼重設，以及使用基本的電子郵件範本的使用者確認。</span><span class="sxs-lookup"><span data-stu-id="da081-109">Out of the box it provides basic user login functionality, including user sign-up, sign-in, password reset, and user confirmation with basic email templates.</span></span>

<span data-ttu-id="da081-110">需求：</span><span class="sxs-lookup"><span data-stu-id="da081-110">Requirements:</span></span>

- [<span data-ttu-id="da081-111">ASP.NET 和 Web 工具 2012.2 更新</span><span class="sxs-lookup"><span data-stu-id="da081-111">ASP.NET and Web Tools 2012.2 update</span></span>](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a><span data-ttu-id="da081-112">建立中樞範本專案</span><span class="sxs-lookup"><span data-stu-id="da081-112">Create a Backbone Template Project</span></span>

<span data-ttu-id="da081-113">下載並安裝的範本，按一下上方的 [下載] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="da081-113">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="da081-114">範本會封裝成 Visual Studio 擴充功能 (VSIX) 檔案。</span><span class="sxs-lookup"><span data-stu-id="da081-114">The template is packaged as a Visual Studio Extension (VSIX) file.</span></span> <span data-ttu-id="da081-115">您可能需要重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="da081-115">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="da081-116">在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。</span><span class="sxs-lookup"><span data-stu-id="da081-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="da081-117">底下**Visual C#**，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="da081-117">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="da081-118">在專案範本清單中，選取**ASP.NET MVC 4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="da081-118">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="da081-119">將專案命名，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="da081-119">Name the project and click **OK**.</span></span>

![](backbonejs-template/_static/image1.png)

<span data-ttu-id="da081-120">在 [**新的專案**] 精靈，選取 Backbone.js SPA 專案。</span><span class="sxs-lookup"><span data-stu-id="da081-120">In the **New Project** wizard, select Backbone.js SPA Project.</span></span>

![](backbonejs-template/_static/image2.png)

<span data-ttu-id="da081-121">按 Ctrl-F5 以建置並執行應用程式，但不偵錯，或按下 F5 以執行並偵錯。</span><span class="sxs-lookup"><span data-stu-id="da081-121">Press Ctrl-F5 to build and run the application without debugging, or press F5 to run with debugging.</span></span>

![](backbonejs-template/_static/image3.png)

<span data-ttu-id="da081-122">按一下 [我的帳戶] 會顯示登入頁面：</span><span class="sxs-lookup"><span data-stu-id="da081-122">Clicking "My Account" brings up the login page:</span></span>

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a><span data-ttu-id="da081-123">逐步解說： 用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="da081-123">Walkthrough: Client Code</span></span>

<span data-ttu-id="da081-124">讓我們開始在用戶端。</span><span class="sxs-lookup"><span data-stu-id="da081-124">Let's starts with the client side.</span></span> <span data-ttu-id="da081-125">用戶端應用程式指令碼位於 ~/Scripts/application 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="da081-125">The client application scripts are located in the ~/Scripts/application folder.</span></span> <span data-ttu-id="da081-126">應用程式以[TypeScript](http://www.typescriptlang.org/) （.ts 檔案） 這會編譯成 JavaScript （.js 檔案）。</span><span class="sxs-lookup"><span data-stu-id="da081-126">The application is written in [TypeScript](http://www.typescriptlang.org/) (.ts files) which are compiled into JavaScript (.js files).</span></span>

<span data-ttu-id="da081-127">**應用程式**</span><span class="sxs-lookup"><span data-stu-id="da081-127">**Application**</span></span>

<span data-ttu-id="da081-128">`Application` 定義於 application.ts。</span><span class="sxs-lookup"><span data-stu-id="da081-128">`Application` is defined in application.ts.</span></span> <span data-ttu-id="da081-129">此物件會初始化應用程式，並做為根命名空間。</span><span class="sxs-lookup"><span data-stu-id="da081-129">This object initializes the application and acts as the root namespace.</span></span> <span data-ttu-id="da081-130">它會維護組態和狀態會在應用程式之間共用的資訊，例如是否將使用者登入。</span><span class="sxs-lookup"><span data-stu-id="da081-130">It maintains configuration and state information that is shared across the application, such as whether the user is signed in.</span></span>

<span data-ttu-id="da081-131">`application.start`方法建立強制回應的檢視，並將附加事件處理常式的應用程式層級的事件，例如使用者登入。</span><span class="sxs-lookup"><span data-stu-id="da081-131">The `application.start` method creates the modal views and attaches event handlers for application-level events, such as user sign-in.</span></span> <span data-ttu-id="da081-132">接下來，它會建立預設路由器，並檢查是否已指定任何用戶端的 URL。</span><span class="sxs-lookup"><span data-stu-id="da081-132">Next, it creates the default router and checks whether any client-side URL is specified.</span></span> <span data-ttu-id="da081-133">如果不是，它會重新導向至預設的 url (# ！ /)。</span><span class="sxs-lookup"><span data-stu-id="da081-133">If not, it redirects to the default url (#!/).</span></span>

<span data-ttu-id="da081-134">**事件**</span><span class="sxs-lookup"><span data-stu-id="da081-134">**Events**</span></span>

<span data-ttu-id="da081-135">開發鬆散聯繫元件時，是很重要的事件。</span><span class="sxs-lookup"><span data-stu-id="da081-135">Events are always important when developing loosely coupled components.</span></span> <span data-ttu-id="da081-136">應用程式通常會執行多個作業以回應使用者動作。</span><span class="sxs-lookup"><span data-stu-id="da081-136">Applications often perform multiple operations in response to a user action.</span></span> <span data-ttu-id="da081-137">骨幹提供內建事件的元件，例如模型、 集合和檢視。</span><span class="sxs-lookup"><span data-stu-id="da081-137">Backbone provides built-in events with components such as Model, Collection, and View.</span></span> <span data-ttu-id="da081-138">而不是建立這些元件間的交互相依性，此範本會使用"pub/sub"模型： `events` events.ts 中, 定義的物件做為事件中樞以供發佈和訂閱的應用程式事件。</span><span class="sxs-lookup"><span data-stu-id="da081-138">Instead of creating inter-dependencies among these components, the template uses a "pub/sub" model: The `events` object, defined in events.ts, acts as an event hub for publishing and subscribing to application events.</span></span> <span data-ttu-id="da081-139">`events`物件是單一值。</span><span class="sxs-lookup"><span data-stu-id="da081-139">The `events` object is a singleton.</span></span> <span data-ttu-id="da081-140">下列程式碼示範如何訂閱事件，然後再觸發事件：</span><span class="sxs-lookup"><span data-stu-id="da081-140">The following code shows how to subscribe to an event and then trigger the event:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

<span data-ttu-id="da081-141">**路由器**</span><span class="sxs-lookup"><span data-stu-id="da081-141">**Router**</span></span>

<span data-ttu-id="da081-142">在 Backbone.js，路由器會提供方法讓路由用戶端頁面並將它們連接至動作和事件。</span><span class="sxs-lookup"><span data-stu-id="da081-142">In Backbone.js, a router provides methods for routing client-side pages and connecting them to actions and events.</span></span> <span data-ttu-id="da081-143">範本會定義單一的路由器，router.ts 中。</span><span class="sxs-lookup"><span data-stu-id="da081-143">The template defines a single router, in router.ts.</span></span> <span data-ttu-id="da081-144">路由器會建立 activable 檢視表，並切換檢視時，會維護狀態。</span><span class="sxs-lookup"><span data-stu-id="da081-144">The router creates the activable views and maintains the state when switching views.</span></span> <span data-ttu-id="da081-145">（下一節會描述 activable 檢視）。專案一開始，有兩個虛擬的檢視，家用以及有關。</span><span class="sxs-lookup"><span data-stu-id="da081-145">(Activable views are described in the next section.) Initially, the project has two dummy views, Home and About.</span></span> <span data-ttu-id="da081-146">它也有找不到檢視中，如果不知道路由就會顯示。</span><span class="sxs-lookup"><span data-stu-id="da081-146">It also has a NotFound view, which is displayed if the route is not known.</span></span>

<span data-ttu-id="da081-147">**檢視**</span><span class="sxs-lookup"><span data-stu-id="da081-147">**Views**</span></span>

<span data-ttu-id="da081-148">~/Scripts/application/檢視表中定義的檢視。</span><span class="sxs-lookup"><span data-stu-id="da081-148">The views are defined in ~/Scripts/application/views.</span></span> <span data-ttu-id="da081-149">有兩種類型的檢視、 activable 檢視和強制回應對話方塊檢視。</span><span class="sxs-lookup"><span data-stu-id="da081-149">There are two kinds of views, activable views and modal dialog views.</span></span> <span data-ttu-id="da081-150">路由器會叫用 activable 的檢視。</span><span class="sxs-lookup"><span data-stu-id="da081-150">Activable views are invoked by the router.</span></span> <span data-ttu-id="da081-151">當 activable 檢視顯示時，所有其他 activable 檢視就會變成非使用中。</span><span class="sxs-lookup"><span data-stu-id="da081-151">When an activable view is shown, all other activable views become inactive.</span></span> <span data-ttu-id="da081-152">若要建立 activable 的檢視，延伸檢視`Activable`物件：</span><span class="sxs-lookup"><span data-stu-id="da081-152">To create an activable view, extend the view with the `Activable` object:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

<span data-ttu-id="da081-153">使用擴充`Activable`將兩個新方法加入至檢視中，`activate`和`deactivate`。</span><span class="sxs-lookup"><span data-stu-id="da081-153">Extending with `Activable` adds two new methods to the view, `activate` and `deactivate`.</span></span> <span data-ttu-id="da081-154">路由器會呼叫這些方法來啟用和停用仍檢視。</span><span class="sxs-lookup"><span data-stu-id="da081-154">The router calls these methods to activate and deactive the view.</span></span>

<span data-ttu-id="da081-155">強制回應檢視會實作為[Twitter Bootstrap](http://twitter.github.com/bootstrap/)強制回應對話方塊。</span><span class="sxs-lookup"><span data-stu-id="da081-155">Modal views are implemented as [Twitter Bootstrap](http://twitter.github.com/bootstrap/) modal dialogs.</span></span> <span data-ttu-id="da081-156">`Membership`和`Profile`檢視是強制回應的檢視。</span><span class="sxs-lookup"><span data-stu-id="da081-156">The `Membership` and `Profile` views are modal views.</span></span> <span data-ttu-id="da081-157">模型檢視可以叫用任何應用程式事件。</span><span class="sxs-lookup"><span data-stu-id="da081-157">Model views can be invoked by any application events.</span></span> <span data-ttu-id="da081-158">例如，在`Navigation`檢視中，按一下 [我的帳戶] 連結會顯示其中一個`Membership`檢視或`Profile`檢視中的，根據使用者是否登入。</span><span class="sxs-lookup"><span data-stu-id="da081-158">For example, in the `Navigation` view, clicking the "My Account" link shows either the `Membership` view or the `Profile` view, depending on whether the user is logged in.</span></span> <span data-ttu-id="da081-159">`Navigation`將 click 事件處理常式，有任何子項目`data-command`屬性。</span><span class="sxs-lookup"><span data-stu-id="da081-159">The `Navigation` attaches click event handlers to any child elements that have the `data-command` attribute.</span></span> <span data-ttu-id="da081-160">以下是 HTML 標記：</span><span class="sxs-lookup"><span data-stu-id="da081-160">Here is the HTML markup:</span></span>

[!code-html[Main](backbonejs-template/samples/sample3.html)]

<span data-ttu-id="da081-161">以下是 navigation.ts 來連結事件中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="da081-161">Here is the code in navigation.ts to hook up the events:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

<span data-ttu-id="da081-162">**模型**</span><span class="sxs-lookup"><span data-stu-id="da081-162">**Models**</span></span>

<span data-ttu-id="da081-163">~/Scripts/application/模型中定義的模型。</span><span class="sxs-lookup"><span data-stu-id="da081-163">The models are defined in ~/Scripts/application/models.</span></span> <span data-ttu-id="da081-164">這些模型都具有三個基本項目： 預設屬性，驗證規則和伺服器端結束點。</span><span class="sxs-lookup"><span data-stu-id="da081-164">The models all have three basic things: default attributes, validation rules, and a server-side end point.</span></span> <span data-ttu-id="da081-165">以下是典型的範例：</span><span class="sxs-lookup"><span data-stu-id="da081-165">Here is a typical example:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

<span data-ttu-id="da081-166">**外掛程式**</span><span class="sxs-lookup"><span data-stu-id="da081-166">**Plug-ins**</span></span>

<span data-ttu-id="da081-167">~/Scripts/application/lib 資料夾包含一些好用的 jQuery 外掛程式。Form.ts 檔案會定義外掛程式，可使用的表單資料。</span><span class="sxs-lookup"><span data-stu-id="da081-167">The ~/Scripts/application/lib folder contains a few handy jQuery plug-ins. The form.ts file defines a plug-in for working with form data.</span></span> <span data-ttu-id="da081-168">通常您要序列化或還原序列化表單資料，並顯示任何模型驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="da081-168">Often you need to serialize or deserialize form data and show any model validation errors.</span></span> <span data-ttu-id="da081-169">具有方法，例如外掛程式 form.ts `serializeFields`， `deserializeFields`，和`showFieldErrors`。</span><span class="sxs-lookup"><span data-stu-id="da081-169">The form.ts plug-in has methods such as `serializeFields`, `deserializeFields`, and `showFieldErrors`.</span></span> <span data-ttu-id="da081-170">下列範例示範如何序列化至模型的表單。</span><span class="sxs-lookup"><span data-stu-id="da081-170">The following example shows how to serialize a form to a model.</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

<span data-ttu-id="da081-171">外掛程式 flashbar.ts 提供各種意見反應訊息給使用者。</span><span class="sxs-lookup"><span data-stu-id="da081-171">The flashbar.ts plug-in gives various kinds of feedback messages to the user.</span></span> <span data-ttu-id="da081-172">方法會`$.showSuccessbar`，`$.showErrorbar`和`$.showInfobar`。</span><span class="sxs-lookup"><span data-stu-id="da081-172">The methods are `$.showSuccessbar`, `$.showErrorbar` and `$.showInfobar`.</span></span> <span data-ttu-id="da081-173">在幕後，它會使用 Twitter Bootstrap 警示顯示良好動畫的訊息。</span><span class="sxs-lookup"><span data-stu-id="da081-173">Behind the scenes, it uses Twitter Bootstrap alerts to show nicely animated messages.</span></span>

<span data-ttu-id="da081-174">外掛程式 confirm.ts 取代瀏覽器的確認對話方塊中，雖然 API 是有點不同：</span><span class="sxs-lookup"><span data-stu-id="da081-174">The confirm.ts plug-in replaces the browser's confirm dialog, although the API is somewhat different:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a><span data-ttu-id="da081-175">逐步解說： 伺服端程式碼</span><span class="sxs-lookup"><span data-stu-id="da081-175">Walkthrough: Server Code</span></span>

<span data-ttu-id="da081-176">現在讓我們看看伺服器端。</span><span class="sxs-lookup"><span data-stu-id="da081-176">Now let's look at the server side.</span></span>

<span data-ttu-id="da081-177">**控制器**</span><span class="sxs-lookup"><span data-stu-id="da081-177">**Controllers**</span></span>

<span data-ttu-id="da081-178">在單一頁面應用程式中，伺服器會扮演小型角色在使用者介面。</span><span class="sxs-lookup"><span data-stu-id="da081-178">In a single page application, the server plays only a small role in the user interface.</span></span> <span data-ttu-id="da081-179">一般來說，伺服器會呈現初始網頁，然後傳送和接收 JSON 資料。</span><span class="sxs-lookup"><span data-stu-id="da081-179">Typically, the server renders the initial page and then sends and receives JSON data.</span></span>

<span data-ttu-id="da081-180">範本有兩個 MVC 控制器：`HomeController`呈現初始頁面，並`SupportsController`用來確認新的使用者帳戶和密碼重設。</span><span class="sxs-lookup"><span data-stu-id="da081-180">The template has two MVC controllers: `HomeController` renders the initial page, and `SupportsController` is used to confirm new user accounts and reset passwords.</span></span> <span data-ttu-id="da081-181">在範本中的所有控制站是 ASP.NET Web API 控制器，傳送與接收 JSON 資料。</span><span class="sxs-lookup"><span data-stu-id="da081-181">All other controllers in the template are ASP.NET Web API controllers, which send and receive JSON data.</span></span> <span data-ttu-id="da081-182">根據預設，新的控制器使用`WebSecurity`類別來執行與使用者相關的工作。</span><span class="sxs-lookup"><span data-stu-id="da081-182">By default, the controllers use the new `WebSecurity` class to perform user-related tasks.</span></span> <span data-ttu-id="da081-183">不過，他們也有選擇性建構函式，可讓您傳入委派來執行這些工作。</span><span class="sxs-lookup"><span data-stu-id="da081-183">However, they also have optional constructors that let you pass in delegates for these tasks.</span></span> <span data-ttu-id="da081-184">這可讓測試更為容易，並讓您取代`WebSecurity`與其他項目，使用 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="da081-184">This makes testing easier, and lets you replace `WebSecurity` with something else, by using an IoC Container.</span></span> <span data-ttu-id="da081-185">請看以下範例：</span><span class="sxs-lookup"><span data-stu-id="da081-185">Here is an example:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a><span data-ttu-id="da081-186">檢視</span><span class="sxs-lookup"><span data-stu-id="da081-186">Views</span></span>

<span data-ttu-id="da081-187">檢視設計為模組化： 頁面的每個區段都有自己專用的檢視。</span><span class="sxs-lookup"><span data-stu-id="da081-187">The views are designed to be modular: Each section of a page has its own dedicated view.</span></span> <span data-ttu-id="da081-188">在單一頁面應用程式中，常會包含沒有任何對應控制器的檢視。</span><span class="sxs-lookup"><span data-stu-id="da081-188">In a single page application, it is common to include views that do not have any corresponding controller.</span></span> <span data-ttu-id="da081-189">您可以藉由呼叫包含檢視`@Html.Partial('myView')`，但這會取得單調乏味。</span><span class="sxs-lookup"><span data-stu-id="da081-189">You can include a view by calling `@Html.Partial('myView')`, but this gets tedious.</span></span> <span data-ttu-id="da081-190">為了簡化起見，範本會定義協助程式方法， `IncludeClientViews`，呈現所有指定的資料夾中的檢視：</span><span class="sxs-lookup"><span data-stu-id="da081-190">To make this easier, the template defines a helper method, `IncludeClientViews`, that renders all of the views in a specified folder:</span></span>

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

<span data-ttu-id="da081-191">如果未指定資料夾名稱，預設的資料夾名稱會是"ClientViews 」。</span><span class="sxs-lookup"><span data-stu-id="da081-191">If the folder name is not specified, the default folder name is "ClientViews".</span></span> <span data-ttu-id="da081-192">如果您的用戶端檢視也會使用部分檢視，名稱以底線字元的部分檢視 (例如`_SignUp`)。</span><span class="sxs-lookup"><span data-stu-id="da081-192">If your client view also uses partial views, name the partial view with an underscore character (for example, `_SignUp`).</span></span> <span data-ttu-id="da081-193">`IncludeClientViews`方法不包括其名稱開頭為底線的任何檢視。</span><span class="sxs-lookup"><span data-stu-id="da081-193">The `IncludeClientViews` method excludes any views whose name starts with an underscore.</span></span> <span data-ttu-id="da081-194">若要併入用戶端檢視中的部分檢視，請呼叫`Html.ClientView('SignUp')`而不是`Html.Partial('_SignUp')`。</span><span class="sxs-lookup"><span data-stu-id="da081-194">To include a partial view in the client view, call `Html.ClientView('SignUp')` instead of `Html.Partial('_SignUp')`.</span></span>

<span data-ttu-id="da081-195">**傳送電子郵件**</span><span class="sxs-lookup"><span data-stu-id="da081-195">**Sending Email**</span></span>

<span data-ttu-id="da081-196">若要傳送電子郵件，此範本會使用[郵遞](http://aboutcode.net/postal)。</span><span class="sxs-lookup"><span data-stu-id="da081-196">To send email, the template uses [Postal](http://aboutcode.net/postal).</span></span> <span data-ttu-id="da081-197">不過，程式碼的其餘部分抽離郵遞`IMailer`介面，以便您可以使用另一個實作，輕鬆地取代它。</span><span class="sxs-lookup"><span data-stu-id="da081-197">However, Postal is abstracted from the rest of the code with the `IMailer` interface, so you can easily replace it with another implementation.</span></span> <span data-ttu-id="da081-198">電子郵件範本位於 [檢視/電子郵件] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="da081-198">The email templates are located in the Views/Emails folder.</span></span> <span data-ttu-id="da081-199">寄件者的電子郵件地址在 web.config 檔案中，指定在`sender.email`的索引鍵**appSettings**一節。</span><span class="sxs-lookup"><span data-stu-id="da081-199">The sender's email address is specified in the web.config file, in the `sender.email` key of the **appSettings** section.</span></span> <span data-ttu-id="da081-200">也，當`debug="true"`在 web.config 中，應用程式不需要使用者電子郵件確認，加速開發。</span><span class="sxs-lookup"><span data-stu-id="da081-200">Also, when `debug="true"` in web.config, the application does not require user email confirmation, to speed up development.</span></span>

## <a name="github"></a><span data-ttu-id="da081-201">GitHub</span><span class="sxs-lookup"><span data-stu-id="da081-201">GitHub</span></span>

<span data-ttu-id="da081-202">您也可以找到 Backbone.js SPA 範本上[GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa)。</span><span class="sxs-lookup"><span data-stu-id="da081-202">You can also find the Backbone.js SPA template on [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).</span></span>
