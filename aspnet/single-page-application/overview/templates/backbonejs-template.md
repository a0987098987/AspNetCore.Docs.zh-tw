---
uid: single-page-application/overview/templates/backbonejs-template
title: "中樞範本 |Microsoft 文件"
author: madskristensen
description: "Backbone.js SPA 範本"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 3b8eabd3cefcb96dc40bbf6cc6e3ee81accb0d7c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="backbone-template"></a><span data-ttu-id="7294c-103">中樞範本</span><span class="sxs-lookup"><span data-stu-id="7294c-103">Backbone Template</span></span>
====================
<span data-ttu-id="7294c-104">由[Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="7294c-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="7294c-105">Kazi Manzur Rashid 寫入中樞 SPA 範本</span><span class="sxs-lookup"><span data-stu-id="7294c-105">The Backbone SPA Template was written by Kazi Manzur Rashid</span></span>
> 
> [<span data-ttu-id="7294c-106">下載 Backbone.js SPA 範本</span><span class="sxs-lookup"><span data-stu-id="7294c-106">Download the Backbone.js SPA Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=293631)


<span data-ttu-id="7294c-107">Backbone.js SPA 範本為了協助您快速地開始建置互動式用戶端 web 應用程式使用[Backbone.js。](http://backbonejs.org/)</span><span class="sxs-lookup"><span data-stu-id="7294c-107">The Backbone.js SPA template is designed to get you started quickly building interactive client-side web apps using [Backbone.js.](http://backbonejs.org/)</span></span>

<span data-ttu-id="7294c-108">此範本會針對開發 ASP.NET MVC 中的 Backbone.js 應用程式提供初始的基本架構。</span><span class="sxs-lookup"><span data-stu-id="7294c-108">The template provides an initial skeleton for developing a Backbone.js application in ASP.NET MVC.</span></span> <span data-ttu-id="7294c-109">根據預設，它會提供基本的使用者登入功能，包括註冊、 登入、 重設使用者密碼，以及具有基本的電子郵件範本的使用者確認。</span><span class="sxs-lookup"><span data-stu-id="7294c-109">Out of the box it provides basic user login functionality, including user sign-up, sign-in, password reset, and user confirmation with basic email templates.</span></span>

<span data-ttu-id="7294c-110">需求：</span><span class="sxs-lookup"><span data-stu-id="7294c-110">Requirements:</span></span>

- [<span data-ttu-id="7294c-111">ASP.NET 和 Web 工具 2012.2 更新</span><span class="sxs-lookup"><span data-stu-id="7294c-111">ASP.NET and Web Tools 2012.2 update</span></span>](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a><span data-ttu-id="7294c-112">建立中樞範本專案</span><span class="sxs-lookup"><span data-stu-id="7294c-112">Create a Backbone Template Project</span></span>

<span data-ttu-id="7294c-113">下載並安裝的範本，按一下下載按鈕上方。</span><span class="sxs-lookup"><span data-stu-id="7294c-113">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="7294c-114">範本會封裝成 Visual Studio 擴充功能 (VSIX) 檔案。</span><span class="sxs-lookup"><span data-stu-id="7294c-114">The template is packaged as a Visual Studio Extension (VSIX) file.</span></span> <span data-ttu-id="7294c-115">您可能需要重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="7294c-115">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="7294c-116">在**範本**窗格中，選取**已安裝的範本**展開**Visual C#**節點。</span><span class="sxs-lookup"><span data-stu-id="7294c-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="7294c-117">在下**Visual C#**，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="7294c-117">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="7294c-118">在專案範本清單中選取**ASP.NET MVC 4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="7294c-118">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="7294c-119">為專案名稱，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="7294c-119">Name the project and click **OK**.</span></span>

![](backbonejs-template/_static/image1.png)

<span data-ttu-id="7294c-120">在**新專案**精靈、 選取 Backbone.js SPA 專案。</span><span class="sxs-lookup"><span data-stu-id="7294c-120">In the **New Project** wizard, select Backbone.js SPA Project.</span></span>

![](backbonejs-template/_static/image2.png)

<span data-ttu-id="7294c-121">按 Ctrl + F5 以建置並執行應用程式，但不偵錯，或按 f5 開始執行並偵錯。</span><span class="sxs-lookup"><span data-stu-id="7294c-121">Press Ctrl-F5 to build and run the application without debugging, or press F5 to run with debugging.</span></span>

![](backbonejs-template/_static/image3.png)

<span data-ttu-id="7294c-122">按一下 「 我的帳戶 」 隨即開啟登入頁面：</span><span class="sxs-lookup"><span data-stu-id="7294c-122">Clicking "My Account" brings up the login page:</span></span>

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a><span data-ttu-id="7294c-123">逐步解說： 用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="7294c-123">Walkthrough: Client Code</span></span>

<span data-ttu-id="7294c-124">讓我們開始在用戶端。</span><span class="sxs-lookup"><span data-stu-id="7294c-124">Let's starts with the client side.</span></span> <span data-ttu-id="7294c-125">用戶端應用程式指令碼位於 ~/Scripts/application 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="7294c-125">The client application scripts are located in the ~/Scripts/application folder.</span></span> <span data-ttu-id="7294c-126">應用程式以[TypeScript](http://www.typescriptlang.org/) （.ts 檔案） 但會編譯成 JavaScript （.js 檔案）。</span><span class="sxs-lookup"><span data-stu-id="7294c-126">The application is written in [TypeScript](http://www.typescriptlang.org/) (.ts files) which are compiled into JavaScript (.js files).</span></span>

<span data-ttu-id="7294c-127">**應用程式**</span><span class="sxs-lookup"><span data-stu-id="7294c-127">**Application**</span></span>

<span data-ttu-id="7294c-128">`Application`application.ts 中定義。</span><span class="sxs-lookup"><span data-stu-id="7294c-128">`Application` is defined in application.ts.</span></span> <span data-ttu-id="7294c-129">此物件會初始化應用程式，並做為根命名空間。</span><span class="sxs-lookup"><span data-stu-id="7294c-129">This object initializes the application and acts as the root namespace.</span></span> <span data-ttu-id="7294c-130">例如使用者是否登入，它就會維護所有應用程式時，會共用的組態和狀態資訊。</span><span class="sxs-lookup"><span data-stu-id="7294c-130">It maintains configuration and state information that is shared across the application, such as whether the user is signed in.</span></span>

<span data-ttu-id="7294c-131">`application.start`方法建立強制回應檢視，並將附加事件處理常式應用程式層級事件，例如 使用者登入。</span><span class="sxs-lookup"><span data-stu-id="7294c-131">The `application.start` method creates the modal views and attaches event handlers for application-level events, such as user sign-in.</span></span> <span data-ttu-id="7294c-132">接下來，它會建立預設路由器，並檢查是否已指定任何用戶端 URL。</span><span class="sxs-lookup"><span data-stu-id="7294c-132">Next, it creates the default router and checks whether any client-side URL is specified.</span></span> <span data-ttu-id="7294c-133">如果不是，它會重新導向至預設的 url (# ！ /)。</span><span class="sxs-lookup"><span data-stu-id="7294c-133">If not, it redirects to the default url (#!/).</span></span>

<span data-ttu-id="7294c-134">**事件**</span><span class="sxs-lookup"><span data-stu-id="7294c-134">**Events**</span></span>

<span data-ttu-id="7294c-135">當開發鬆散結合的元件，會一律重要事件。</span><span class="sxs-lookup"><span data-stu-id="7294c-135">Events are always important when developing loosely coupled components.</span></span> <span data-ttu-id="7294c-136">應用程式通常會執行多個作業以回應使用者動作。</span><span class="sxs-lookup"><span data-stu-id="7294c-136">Applications often perform multiple operations in response to a user action.</span></span> <span data-ttu-id="7294c-137">Backbone 提供內建事件的元件，例如模型、 收集及檢視。</span><span class="sxs-lookup"><span data-stu-id="7294c-137">Backbone provides built-in events with components such as Model, Collection, and View.</span></span> <span data-ttu-id="7294c-138">而不是建立在這些元件間的相依性，範本會使用"pub/sub 」 模型： `events` events.ts 中, 定義的物件做為事件中心來發佈和訂閱應用程式事件。</span><span class="sxs-lookup"><span data-stu-id="7294c-138">Instead of creating inter-dependencies among these components, the template uses a "pub/sub" model: The `events` object, defined in events.ts, acts as an event hub for publishing and subscribing to application events.</span></span> <span data-ttu-id="7294c-139">`events`物件是單一值。</span><span class="sxs-lookup"><span data-stu-id="7294c-139">The `events` object is a singleton.</span></span> <span data-ttu-id="7294c-140">下列程式碼會示範如何訂閱事件，然後觸發事件：</span><span class="sxs-lookup"><span data-stu-id="7294c-140">The following code shows how to subscribe to an event and then trigger the event:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

<span data-ttu-id="7294c-141">**路由器**</span><span class="sxs-lookup"><span data-stu-id="7294c-141">**Router**</span></span>

<span data-ttu-id="7294c-142">在 Backbone.js，路由器會提供方法讓路由用戶端網頁並將它們連接至動作和事件。</span><span class="sxs-lookup"><span data-stu-id="7294c-142">In Backbone.js, a router provides methods for routing client-side pages and connecting them to actions and events.</span></span> <span data-ttu-id="7294c-143">範本會定義單一路由器，router.ts 中。</span><span class="sxs-lookup"><span data-stu-id="7294c-143">The template defines a single router, in router.ts.</span></span> <span data-ttu-id="7294c-144">路由器會建立 activable 檢視表，並切換檢視時，會維護狀態。</span><span class="sxs-lookup"><span data-stu-id="7294c-144">The router creates the activable views and maintains the state when switching views.</span></span> <span data-ttu-id="7294c-145">（下一節會描述 activable 檢視）。專案一開始，有兩個空的檢視，家用以及有關。</span><span class="sxs-lookup"><span data-stu-id="7294c-145">(Activable views are described in the next section.) Initially, the project has two dummy views, Home and About.</span></span> <span data-ttu-id="7294c-146">它也有找不到檢視中，如果不知道路由就會顯示。</span><span class="sxs-lookup"><span data-stu-id="7294c-146">It also has a NotFound view, which is displayed if the route is not known.</span></span>

<span data-ttu-id="7294c-147">**檢視**</span><span class="sxs-lookup"><span data-stu-id="7294c-147">**Views**</span></span>

<span data-ttu-id="7294c-148">~/Scripts/application/檢視表中定義的檢視。</span><span class="sxs-lookup"><span data-stu-id="7294c-148">The views are defined in ~/Scripts/application/views.</span></span> <span data-ttu-id="7294c-149">有兩種類型的檢視、 activable 檢視和強制回應對話方塊檢視。</span><span class="sxs-lookup"><span data-stu-id="7294c-149">There are two kinds of views, activable views and modal dialog views.</span></span> <span data-ttu-id="7294c-150">Activable 檢視叫用之路由器。</span><span class="sxs-lookup"><span data-stu-id="7294c-150">Activable views are invoked by the router.</span></span> <span data-ttu-id="7294c-151">當 activable 檢視顯示時，所有其他 activable 檢視成為非使用中。</span><span class="sxs-lookup"><span data-stu-id="7294c-151">When an activable view is shown, all other activable views become inactive.</span></span> <span data-ttu-id="7294c-152">若要建立 activable 的檢視，延伸檢視`Activable`物件：</span><span class="sxs-lookup"><span data-stu-id="7294c-152">To create an activable view, extend the view with the `Activable` object:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

<span data-ttu-id="7294c-153">使用擴充`Activable`將兩個新方法加入至檢視，`activate`和`deactivate`。</span><span class="sxs-lookup"><span data-stu-id="7294c-153">Extending with `Activable` adds two new methods to the view, `activate` and `deactivate`.</span></span> <span data-ttu-id="7294c-154">路由器會呼叫這些方法來啟用和停用仍檢視。</span><span class="sxs-lookup"><span data-stu-id="7294c-154">The router calls these methods to activate and deactive the view.</span></span>

<span data-ttu-id="7294c-155">強制回應檢視會實作為[Twitter Bootstrap](http://twitter.github.com/bootstrap/)強制回應對話方塊。</span><span class="sxs-lookup"><span data-stu-id="7294c-155">Modal views are implemented as [Twitter Bootstrap](http://twitter.github.com/bootstrap/) modal dialogs.</span></span> <span data-ttu-id="7294c-156">`Membership`和`Profile`檢視是強制回應的檢視。</span><span class="sxs-lookup"><span data-stu-id="7294c-156">The `Membership` and `Profile` views are modal views.</span></span> <span data-ttu-id="7294c-157">模型檢視可以叫用任何應用程式事件。</span><span class="sxs-lookup"><span data-stu-id="7294c-157">Model views can be invoked by any application events.</span></span> <span data-ttu-id="7294c-158">例如，在`Navigation` 檢視中，按一下 我的帳戶 連結顯示`Membership`檢視或`Profile` 檢視中的，根據使用者是否登入。</span><span class="sxs-lookup"><span data-stu-id="7294c-158">For example, in the `Navigation` view, clicking the "My Account" link shows either the `Membership` view or the `Profile` view, depending on whether the user is logged in.</span></span> <span data-ttu-id="7294c-159">`Navigation`附加 click 事件處理常式，有任何子項目的`data-command`屬性。</span><span class="sxs-lookup"><span data-stu-id="7294c-159">The `Navigation` attaches click event handlers to any child elements that have the `data-command` attribute.</span></span> <span data-ttu-id="7294c-160">以下是 HTML 標記：</span><span class="sxs-lookup"><span data-stu-id="7294c-160">Here is the HTML markup:</span></span>

[!code-html[Main](backbonejs-template/samples/sample3.html)]

<span data-ttu-id="7294c-161">下列程式碼中的事件連結至 navigation.ts:</span><span class="sxs-lookup"><span data-stu-id="7294c-161">Here is the code in navigation.ts to hook up the events:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

<span data-ttu-id="7294c-162">**模型**</span><span class="sxs-lookup"><span data-stu-id="7294c-162">**Models**</span></span>

<span data-ttu-id="7294c-163">模型是 ~/Scripts/application/模型中定義。</span><span class="sxs-lookup"><span data-stu-id="7294c-163">The models are defined in ~/Scripts/application/models.</span></span> <span data-ttu-id="7294c-164">這些模型都具有三個基本項目： 預設屬性，驗證規則和伺服器端結束點。</span><span class="sxs-lookup"><span data-stu-id="7294c-164">The models all have three basic things: default attributes, validation rules, and a server-side end point.</span></span> <span data-ttu-id="7294c-165">以下是典型的範例：</span><span class="sxs-lookup"><span data-stu-id="7294c-165">Here is a typical example:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

<span data-ttu-id="7294c-166">**外掛程式**</span><span class="sxs-lookup"><span data-stu-id="7294c-166">**Plug-ins**</span></span>

<span data-ttu-id="7294c-167">~/Scripts/application/lib 資料夾包含幾個便利的 jQuery 外掛程式。Form.ts 檔案會定義使用表單資料的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="7294c-167">The ~/Scripts/application/lib folder contains a few handy jQuery plug-ins. The form.ts file defines a plug-in for working with form data.</span></span> <span data-ttu-id="7294c-168">通常您需要序列化或還原序列化表單資料，並顯示任何模型驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="7294c-168">Often you need to serialize or deserialize form data and show any model validation errors.</span></span> <span data-ttu-id="7294c-169">外掛程式 form.ts 有方法例如`serializeFields`， `deserializeFields`，和`showFieldErrors`。</span><span class="sxs-lookup"><span data-stu-id="7294c-169">The form.ts plug-in has methods such as `serializeFields`, `deserializeFields`, and `showFieldErrors`.</span></span> <span data-ttu-id="7294c-170">下列範例會示範如何序列化至模型的表單。</span><span class="sxs-lookup"><span data-stu-id="7294c-170">The following example shows how to serialize a form to a model.</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

<span data-ttu-id="7294c-171">外掛程式 flashbar.ts 提供不同種類的回應訊息給使用者。</span><span class="sxs-lookup"><span data-stu-id="7294c-171">The flashbar.ts plug-in gives various kinds of feedback messages to the user.</span></span> <span data-ttu-id="7294c-172">方法都是`$.showSuccessbar`，`$.showErrorbar`和`$.showInfobar`。</span><span class="sxs-lookup"><span data-stu-id="7294c-172">The methods are `$.showSuccessbar`, `$.showErrorbar` and `$.showInfobar`.</span></span> <span data-ttu-id="7294c-173">在幕後，它會使用 Twitter Bootstrap 警示顯示動畫得很好的訊息。</span><span class="sxs-lookup"><span data-stu-id="7294c-173">Behind the scenes, it uses Twitter Bootstrap alerts to show nicely animated messages.</span></span>

<span data-ttu-id="7294c-174">外掛程式 confirm.ts 取代瀏覽器的確認對話方塊中，雖然應用程式開發介面會稍有不同：</span><span class="sxs-lookup"><span data-stu-id="7294c-174">The confirm.ts plug-in replaces the browser's confirm dialog, although the API is somewhat different:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a><span data-ttu-id="7294c-175">逐步解說： 伺服端程式碼</span><span class="sxs-lookup"><span data-stu-id="7294c-175">Walkthrough: Server Code</span></span>

<span data-ttu-id="7294c-176">現在讓我們看看伺服器端。</span><span class="sxs-lookup"><span data-stu-id="7294c-176">Now let's look at the server side.</span></span>

<span data-ttu-id="7294c-177">**控制器**</span><span class="sxs-lookup"><span data-stu-id="7294c-177">**Controllers**</span></span>

<span data-ttu-id="7294c-178">在單一頁面應用程式中，伺服器會扮演小型角色在使用者介面。</span><span class="sxs-lookup"><span data-stu-id="7294c-178">In a single page application, the server plays only a small role in the user interface.</span></span> <span data-ttu-id="7294c-179">一般而言，伺服器會呈現初始頁面，然後傳送和接收 JSON 資料。</span><span class="sxs-lookup"><span data-stu-id="7294c-179">Typically, the server renders the initial page and then sends and receives JSON data.</span></span>

<span data-ttu-id="7294c-180">該範本含有兩個 MVC 控制器：`HomeController`呈現的初始頁面，並`SupportsController`用來確認新的使用者帳戶和密碼重設。</span><span class="sxs-lookup"><span data-stu-id="7294c-180">The template has two MVC controllers: `HomeController` renders the initial page, and `SupportsController` is used to confirm new user accounts and reset passwords.</span></span> <span data-ttu-id="7294c-181">在範本中的所有控制站是 ASP.NET Web API 控制器，其中傳送和接收 JSON 資料。</span><span class="sxs-lookup"><span data-stu-id="7294c-181">All other controllers in the template are ASP.NET Web API controllers, which send and receive JSON data.</span></span> <span data-ttu-id="7294c-182">根據預設，新的控制站使用`WebSecurity`類別以執行與使用者相關的工作。</span><span class="sxs-lookup"><span data-stu-id="7294c-182">By default, the controllers use the new `WebSecurity` class to perform user-related tasks.</span></span> <span data-ttu-id="7294c-183">不過，他們也擁有可讓您委派中傳遞這些工作的選擇性建構函式。</span><span class="sxs-lookup"><span data-stu-id="7294c-183">However, they also have optional constructors that let you pass in delegates for these tasks.</span></span> <span data-ttu-id="7294c-184">這可讓測試更容易，並讓您取代`WebSecurity`與其他項目，使用 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="7294c-184">This makes testing easier, and lets you replace `WebSecurity` with something else, by using an IoC Container.</span></span> <span data-ttu-id="7294c-185">請看以下範例：</span><span class="sxs-lookup"><span data-stu-id="7294c-185">Here is an example:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a><span data-ttu-id="7294c-186">檢視</span><span class="sxs-lookup"><span data-stu-id="7294c-186">Views</span></span>

<span data-ttu-id="7294c-187">檢視設計成模組： 頁面的每個區段都有自己專用的檢視。</span><span class="sxs-lookup"><span data-stu-id="7294c-187">The views are designed to be modular: Each section of a page has its own dedicated view.</span></span> <span data-ttu-id="7294c-188">在單一頁面應用程式中，它會包含沒有相對應的控制項的檢視。</span><span class="sxs-lookup"><span data-stu-id="7294c-188">In a single page application, it is common to include views that do not have any corresponding controller.</span></span> <span data-ttu-id="7294c-189">您可以藉由呼叫包含檢視`@Html.Partial('myView')`，但這會取得繁瑣。</span><span class="sxs-lookup"><span data-stu-id="7294c-189">You can include a view by calling `@Html.Partial('myView')`, but this gets tedious.</span></span> <span data-ttu-id="7294c-190">若要進行簡化，範本會定義協助程式方法， `IncludeClientViews`，呈現所有指定的資料夾中的檢視：</span><span class="sxs-lookup"><span data-stu-id="7294c-190">To make this easier, the template defines a helper method, `IncludeClientViews`, that renders all of the views in a specified folder:</span></span>

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

<span data-ttu-id="7294c-191">如果未指定資料夾名稱，則預設資料夾名稱是"ClientViews"。</span><span class="sxs-lookup"><span data-stu-id="7294c-191">If the folder name is not specified, the default folder name is "ClientViews".</span></span> <span data-ttu-id="7294c-192">如果您的用戶端檢視也會使用部分檢視，檢視的名稱部分以底線字元 (例如， `_SignUp`)。</span><span class="sxs-lookup"><span data-stu-id="7294c-192">If your client view also uses partial views, name the partial view with an underscore character (for example, `_SignUp`).</span></span> <span data-ttu-id="7294c-193">`IncludeClientViews`方法排除任何檢視名稱開頭為底線。</span><span class="sxs-lookup"><span data-stu-id="7294c-193">The `IncludeClientViews` method excludes any views whose name starts with an underscore.</span></span> <span data-ttu-id="7294c-194">若要包含在用戶端檢視的部分檢視，呼叫`Html.ClientView('SignUp')`而不是`Html.Partial('_SignUp')`。</span><span class="sxs-lookup"><span data-stu-id="7294c-194">To include a partial view in the client view, call `Html.ClientView('SignUp')` instead of `Html.Partial('_SignUp')`.</span></span>

<span data-ttu-id="7294c-195">**傳送電子郵件**</span><span class="sxs-lookup"><span data-stu-id="7294c-195">**Sending Email**</span></span>

<span data-ttu-id="7294c-196">若要傳送電子郵件，範本會使用[郵遞](http://aboutcode.net/postal)。</span><span class="sxs-lookup"><span data-stu-id="7294c-196">To send email, the template uses [Postal](http://aboutcode.net/postal).</span></span> <span data-ttu-id="7294c-197">不過，從程式碼的其餘部分區隔郵遞`IMailer`介面，以便您可以使用另一個實作，輕鬆地取代它。</span><span class="sxs-lookup"><span data-stu-id="7294c-197">However, Postal is abstracted from the rest of the code with the `IMailer` interface, so you can easily replace it with another implementation.</span></span> <span data-ttu-id="7294c-198">電子郵件範本位於 [檢視/電子郵件] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="7294c-198">The email templates are located in the Views/Emails folder.</span></span> <span data-ttu-id="7294c-199">寄件者的電子郵件地址在 web.config 檔案中，指定在`sender.email`索引鍵**appSettings** > 一節。</span><span class="sxs-lookup"><span data-stu-id="7294c-199">The sender's email address is specified in the web.config file, in the `sender.email` key of the **appSettings** section.</span></span> <span data-ttu-id="7294c-200">也，當`debug="true"`在 web.config 中，應用程式不需要使用者電子郵件確認，加速開發。</span><span class="sxs-lookup"><span data-stu-id="7294c-200">Also, when `debug="true"` in web.config, the application does not require user email confirmation, to speed up development.</span></span>

## <a name="github"></a><span data-ttu-id="7294c-201">GitHub</span><span class="sxs-lookup"><span data-stu-id="7294c-201">GitHub</span></span>

<span data-ttu-id="7294c-202">您也可以在找到 Backbone.js SPA 範本[GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa)。</span><span class="sxs-lookup"><span data-stu-id="7294c-202">You can also find the Backbone.js SPA template on [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).</span></span>
