---
title: 將控制器新增至 ASP.NET Core MVC 應用程式
author: rick-anderson
description: 了解如何將控制器新增至簡單的 ASP.NET Core MVC 應用程式。
ms.author: riande
ms.date: 02/28/2017
uid: tutorials/first-mvc-app/adding-controller
ms.openlocfilehash: ab97b875956ec262623ed9862ace6a930331d80d
ms.sourcegitcommit: 979dbfc5e9ce09b9470789989cddfcfb57079d94
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682316"
---
# <a name="add-a-controller-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="7c119-103">將控制器新增至 ASP.NET Core MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="7c119-103">Add a controller to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="7c119-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7c119-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7c119-105">Model-View-Controller (MVC) 架構模式可將一個應用程式劃分成三個主要元件：模型 (**M**)、檢視 (**V**) 和控制器 (**C**)。</span><span class="sxs-lookup"><span data-stu-id="7c119-105">The Model-View-Controller (MVC) architectural pattern separates an app into three main components: **M**odel, **V**iew, and **C**ontroller.</span></span> <span data-ttu-id="7c119-106">MVC 模式可協助您建立比傳統整合型應用程式更可測試且更易於更新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c119-106">The MVC pattern helps you create apps that are more testable and easier to update than traditional monolithic apps.</span></span> <span data-ttu-id="7c119-107">MVC 架構的應用程式包含：</span><span class="sxs-lookup"><span data-stu-id="7c119-107">MVC-based apps contain:</span></span>

* <span data-ttu-id="7c119-108">模型 (**M**)：代表應用程式資料的類別。</span><span class="sxs-lookup"><span data-stu-id="7c119-108">**M**odels: Classes that represent the data of the app.</span></span> <span data-ttu-id="7c119-109">模型類別使用驗證邏輯對該資料強制執行商務規則。</span><span class="sxs-lookup"><span data-stu-id="7c119-109">The model classes use validation logic to enforce business rules for that data.</span></span> <span data-ttu-id="7c119-110">通常，模型物件會在資料庫中擷取並儲存模型狀態。</span><span class="sxs-lookup"><span data-stu-id="7c119-110">Typically, model objects retrieve and store model state in a database.</span></span> <span data-ttu-id="7c119-111">在本教學課程中，`Movie` 模型會從資料庫擷取電影資料，將其提供給檢視或更新它。</span><span class="sxs-lookup"><span data-stu-id="7c119-111">In this tutorial, a `Movie` model retrieves movie data from a database, provides it to the view or updates it.</span></span> <span data-ttu-id="7c119-112">更新的資料會寫入資料庫。</span><span class="sxs-lookup"><span data-stu-id="7c119-112">Updated data is written to a database.</span></span>

* <span data-ttu-id="7c119-113">檢視 (**V**)：檢視是顯示應用程式使用者介面 (UI) 的元件。</span><span class="sxs-lookup"><span data-stu-id="7c119-113">**V**iews: Views are the components that display the app's user interface (UI).</span></span> <span data-ttu-id="7c119-114">一般而言，此 UI 會顯示模型資料。</span><span class="sxs-lookup"><span data-stu-id="7c119-114">Generally, this UI displays the model data.</span></span>

* <span data-ttu-id="7c119-115">控制器 (**C**)：處理瀏覽器要求的類別。</span><span class="sxs-lookup"><span data-stu-id="7c119-115">**C**ontrollers: Classes that handle browser requests.</span></span> <span data-ttu-id="7c119-116">它們會擷取模型資料，並呼叫傳回回應的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="7c119-116">They retrieve model data and call view templates that return a response.</span></span> <span data-ttu-id="7c119-117">在 MVC 應用程式中，檢視只能顯示資訊；控制器則會處理和回應使用者輸入和互動。</span><span class="sxs-lookup"><span data-stu-id="7c119-117">In an MVC app, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="7c119-118">例如，控制器會處理路由資料和查詢字串值，並將這些值傳遞至模型。</span><span class="sxs-lookup"><span data-stu-id="7c119-118">For example, the controller handles route data and query-string values, and passes these values to the model.</span></span> <span data-ttu-id="7c119-119">此模型可能會使用這些值來查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="7c119-119">The model might use these values to query the database.</span></span> <span data-ttu-id="7c119-120">例如，`https://localhost:5001/Home/Privacy` 具有 `Home` (控制器) 和 `Privacy` (在首頁控制器上呼叫的動作方法) 的路由資料。</span><span class="sxs-lookup"><span data-stu-id="7c119-120">For example, `https://localhost:5001/Home/Privacy` has route data of `Home` (the controller) and `Privacy` (the action method to call on the home controller).</span></span> <span data-ttu-id="7c119-121">`https://localhost:5001/Movies/Edit/5` 是要使用電影控制器編輯識別碼 = 5 之電影的要求。</span><span class="sxs-lookup"><span data-stu-id="7c119-121">`https://localhost:5001/Movies/Edit/5` is a request to edit the movie with ID=5 using the movie controller.</span></span> <span data-ttu-id="7c119-122">本教學課程稍後會說明路由資料。</span><span class="sxs-lookup"><span data-stu-id="7c119-122">Route data is explained later in the tutorial.</span></span>

<span data-ttu-id="7c119-123">MVC 模式可協助您建立應用程式，用來隔離應用程的不同層面 (輸入邏輯、商務邏輯和 UI 邏輯)，同時提供這些項目之間的鬆散結合。</span><span class="sxs-lookup"><span data-stu-id="7c119-123">The MVC pattern helps you create apps that separate the different aspects of the app (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="7c119-124">此模式指定每一種邏輯應該位於應用程式中的位置。</span><span class="sxs-lookup"><span data-stu-id="7c119-124">The pattern specifies where each kind of logic should be located in the app.</span></span> <span data-ttu-id="7c119-125">UI 邏輯位於檢視。</span><span class="sxs-lookup"><span data-stu-id="7c119-125">The UI logic belongs in the view.</span></span> <span data-ttu-id="7c119-126">輸入邏輯位於控制器。</span><span class="sxs-lookup"><span data-stu-id="7c119-126">Input logic belongs in the controller.</span></span> <span data-ttu-id="7c119-127">商務邏輯則位於模型。</span><span class="sxs-lookup"><span data-stu-id="7c119-127">Business logic belongs in the model.</span></span> <span data-ttu-id="7c119-128">這項隔離可協助您管理建置應用程式時的複雜度，因為它可讓您一次處理實作的其中一個層面，而不影響另一個層面的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7c119-128">This separation helps you manage complexity when you build an app, because it enables you to work on one aspect of the implementation at a time without impacting the code of another.</span></span> <span data-ttu-id="7c119-129">例如，您可以處理檢視程式碼，而不需要根據商務邏輯程式碼。</span><span class="sxs-lookup"><span data-stu-id="7c119-129">For example, you can work on the view code without depending on the business logic code.</span></span>

<span data-ttu-id="7c119-130">本教學課程系列中涵蓋了這些概念，並且顯示如何使用它們來建置電影應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c119-130">We cover these concepts in this tutorial series and show you how to use them to build a movie app.</span></span> <span data-ttu-id="7c119-131">MVC 專案包含｢控制器｣  和｢檢視｣  的資料夾。</span><span class="sxs-lookup"><span data-stu-id="7c119-131">The MVC project contains folders for the *Controllers* and *Views*.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="7c119-132">新增控制器</span><span class="sxs-lookup"><span data-stu-id="7c119-132">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7c119-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c119-133">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7c119-134">在 [方案總管]  中，以滑鼠右鍵按一下 [控制器] > [新增] > [控制器]  
  ![操作功能表](adding-controller/_static/add_controller.png)</span><span class="sxs-lookup"><span data-stu-id="7c119-134">In **Solution Explorer**, right-click **Controllers > Add > Controller**
![Contextual menu](adding-controller/_static/add_controller.png)</span></span>

* <span data-ttu-id="7c119-135">在 [新增 Scaffold]  對話方塊中，選取 [MVC 控制器 - 空白] </span><span class="sxs-lookup"><span data-stu-id="7c119-135">In the **Add Scaffold** dialog box, select **MVC Controller - Empty**</span></span>

  ![新增 MVC 控制器並將其命名](adding-controller/_static/ac.png)

* <span data-ttu-id="7c119-137">在 [Add Empty MVC Controller] \(新增空白 MVC 控制器\)  對話方塊中，輸入 **HelloWorldController**，然後選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="7c119-137">In the **Add Empty MVC Controller dialog**, enter **HelloWorldController** and select **ADD**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7c119-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c119-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7c119-139">選取**總管**圖示，然後 Control+按一下 (按一下滑鼠右鍵) [控制器] > [新增檔案]  ，將新檔案命名為 *HelloWorldController.cs*。</span><span class="sxs-lookup"><span data-stu-id="7c119-139">Select the **EXPLORER** icon and then control-click (right-click) **Controllers > New File** and name the new file *HelloWorldController.cs*.</span></span>

  ![操作功能表](~/tutorials/first-mvc-app-xplat/adding-controller/_static/new_file.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7c119-141">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7c119-141">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7c119-142">在方案總管  中，以滑鼠右鍵按一下 [控制器] > [新增] > [新增檔案]  。</span><span class="sxs-lookup"><span data-stu-id="7c119-142">In **Solution Explorer**, right-click **Controllers > Add > New File**.</span></span>
<span data-ttu-id="7c119-143">![操作功能表](~/tutorials/first-mvc-app-mac/adding-controller/_static/add_controller.png)</span><span class="sxs-lookup"><span data-stu-id="7c119-143">![Contextual menu](~/tutorials/first-mvc-app-mac/adding-controller/_static/add_controller.png)</span></span>

<span data-ttu-id="7c119-144">選取 [ASP.NET Core]  和 [MVC 控制器類別]  。</span><span class="sxs-lookup"><span data-stu-id="7c119-144">Select **ASP.NET Core** and **MVC Controller Class**.</span></span>

<span data-ttu-id="7c119-145">將控制器命名為 **HelloWorldController**。</span><span class="sxs-lookup"><span data-stu-id="7c119-145">Name the controller **HelloWorldController**.</span></span>

![新增 MVC 控制器並將其命名](~/tutorials/first-mvc-app-mac/adding-controller/_static/ac.png)

---

<span data-ttu-id="7c119-147">以下列內容取代 *Controllers/HelloWorldController.cs* 的內容：</span><span class="sxs-lookup"><span data-stu-id="7c119-147">Replace the contents of *Controllers/HelloWorldController.cs* with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

<span data-ttu-id="7c119-148">控制器中的每個 `public` 方法可呼叫作為 HTTP 端點。</span><span class="sxs-lookup"><span data-stu-id="7c119-148">Every `public` method in a controller is callable as an HTTP endpoint.</span></span> <span data-ttu-id="7c119-149">在上述範例中，這兩種方法都會傳回一個字串。</span><span class="sxs-lookup"><span data-stu-id="7c119-149">In the sample above, both methods return a string.</span></span> <span data-ttu-id="7c119-150">請注意每個方法之前的註解。</span><span class="sxs-lookup"><span data-stu-id="7c119-150">Note the comments preceding each method.</span></span>

<span data-ttu-id="7c119-151">HTTP 端點是 Web 應用程式中的可設定目標 URL，例如 `https://localhost:5001/HelloWorld`，其結合使用的通訊協定：`HTTPS`、網頁伺服器的網路位置 (包括 TCP 連接埠)：`localhost:5001`，以及目標 URI `HelloWorld`。</span><span class="sxs-lookup"><span data-stu-id="7c119-151">An HTTP endpoint is a targetable URL in the web application, such as `https://localhost:5001/HelloWorld`, and combines the protocol used: `HTTPS`, the network location of the web server (including the TCP port): `localhost:5001` and the target URI `HelloWorld`.</span></span>

<span data-ttu-id="7c119-152">第一個註解指出這是 [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) 方法，叫用方法是將 `/HelloWorld/` 附加至基底 URL。</span><span class="sxs-lookup"><span data-stu-id="7c119-152">The first comment states this is an [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) method that's invoked by appending `/HelloWorld/` to the base URL.</span></span> <span data-ttu-id="7c119-153">第二個註解會指定 [HTTP GET](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) 方法，叫用方法是將 `/HelloWorld/Welcome/` 附加至 URL。</span><span class="sxs-lookup"><span data-stu-id="7c119-153">The second comment specifies an [HTTP GET](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) method that's invoked by appending `/HelloWorld/Welcome/` to the URL.</span></span> <span data-ttu-id="7c119-154">稍後在本教學課程中，您將使用 Scaffolding 引擎產生 `HTTP POST` 方法來更新資料。</span><span class="sxs-lookup"><span data-stu-id="7c119-154">Later on in the tutorial the scaffolding engine is used to generate `HTTP POST` methods which update data.</span></span>

<span data-ttu-id="7c119-155">在非偵錯模式中執行應用程式，並將 "HelloWorld" 附加至網址列中的路徑。</span><span class="sxs-lookup"><span data-stu-id="7c119-155">Run the app in non-debug mode and append "HelloWorld" to the path in the address bar.</span></span> <span data-ttu-id="7c119-156">`Index` 方法會傳回一個字串。</span><span class="sxs-lookup"><span data-stu-id="7c119-156">The `Index` method returns a string.</span></span>

![顯示應用程式回應 "This is my default action" 的瀏覽器視窗](~/tutorials/first-mvc-app/adding-controller/_static/hell1.png)

<span data-ttu-id="7c119-158">MVC 會根據傳入 URL 叫用控制器類別 (和其中的動作方法)。</span><span class="sxs-lookup"><span data-stu-id="7c119-158">MVC invokes controller classes (and the action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="7c119-159">MVC 使用的預設 [URL 路由邏輯](xref:mvc/controllers/routing)使用像這樣的格式來判斷要叫用的程式碼：</span><span class="sxs-lookup"><span data-stu-id="7c119-159">The default [URL routing logic](xref:mvc/controllers/routing) used by MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="7c119-160">您可以在 *Startup.cs* 檔案的 `Configure` 方法中設定路由格式。</span><span class="sxs-lookup"><span data-stu-id="7c119-160">The routing format is set in the `Configure` method in *Startup.cs* file.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_1&highlight=5)]

<span data-ttu-id="7c119-161">當您瀏覽至應用程式而不提供任何 URL 區段時，則會預設為上方醒目提示之範本行中指定的 "Home" 控制器和 "Index" 方法。</span><span class="sxs-lookup"><span data-stu-id="7c119-161">When you browse to the app and don't supply any URL segments, it defaults to the "Home" controller and the "Index" method specified in the template line highlighted above.</span></span>

<span data-ttu-id="7c119-162">第一個 URL 區段決定要執行的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="7c119-162">The first URL segment determines the controller class to run.</span></span> <span data-ttu-id="7c119-163">因此，`localhost:xxxx/HelloWorld` 會對應到 **HelloWorld**Controller 類別。</span><span class="sxs-lookup"><span data-stu-id="7c119-163">So `localhost:xxxx/HelloWorld` maps to the **HelloWorld**Controller class.</span></span> <span data-ttu-id="7c119-164">URL 區段的第二部分則決定類別上的動作方法。</span><span class="sxs-lookup"><span data-stu-id="7c119-164">The second part of the URL segment determines the action method on the class.</span></span> <span data-ttu-id="7c119-165">因此，`localhost:xxxx/HelloWorld/Index` 會導致 `HelloWorldController` 類別的 `Index` 方法執行。</span><span class="sxs-lookup"><span data-stu-id="7c119-165">So `localhost:xxxx/HelloWorld/Index` would cause the `Index` method of the `HelloWorldController` class to run.</span></span> <span data-ttu-id="7c119-166">請注意，您只需要瀏覽至 `localhost:xxxx/HelloWorld`，根據預設就會呼叫 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="7c119-166">Notice that you only had to browse to `localhost:xxxx/HelloWorld` and the `Index` method was called by default.</span></span> <span data-ttu-id="7c119-167">這是因為，`Index` 是在未明確指定方法名稱時，會在控制器上呼叫的預設方法。</span><span class="sxs-lookup"><span data-stu-id="7c119-167">That's because `Index` is the default method that will be called on a controller if a method name isn't explicitly specified.</span></span> <span data-ttu-id="7c119-168">URL 區段的第三個部分 (`id`) 是路由資料。</span><span class="sxs-lookup"><span data-stu-id="7c119-168">The third part of the URL segment ( `id`) is for route data.</span></span> <span data-ttu-id="7c119-169">本教學課程稍後會說明路由資料。</span><span class="sxs-lookup"><span data-stu-id="7c119-169">Route data is explained later in the tutorial.</span></span>

<span data-ttu-id="7c119-170">瀏覽至 `https://localhost:xxxx/HelloWorld/Welcome`。</span><span class="sxs-lookup"><span data-stu-id="7c119-170">Browse to `https://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="7c119-171">`Welcome` 方法隨即執行，並傳回字串 `This is the Welcome action method...`。</span><span class="sxs-lookup"><span data-stu-id="7c119-171">The `Welcome` method runs and returns the string `This is the Welcome action method...`.</span></span> <span data-ttu-id="7c119-172">在此 URL 中，控制器是 `HelloWorld`，而 `Welcome` 是動作方法。</span><span class="sxs-lookup"><span data-stu-id="7c119-172">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="7c119-173">您尚未使用 URL 的 `[Parameters]` 部分。</span><span class="sxs-lookup"><span data-stu-id="7c119-173">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![顯示應用程式回應 "This is the Welcome action method" 的瀏覽器視窗](~/tutorials/first-mvc-app/adding-controller/_static/welcome.png)

<span data-ttu-id="7c119-175">修改程式碼 ，將 URL 中的某些參數資訊傳遞到控制器。</span><span class="sxs-lookup"><span data-stu-id="7c119-175">Modify the code to pass some parameter information from the URL to the controller.</span></span> <span data-ttu-id="7c119-176">例如，`/HelloWorld/Welcome?name=Rick&numtimes=4`。</span><span class="sxs-lookup"><span data-stu-id="7c119-176">For example, `/HelloWorld/Welcome?name=Rick&numtimes=4`.</span></span> <span data-ttu-id="7c119-177">變更 `Welcome` 方法以包含兩個參數，如下列程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="7c119-177">Change the `Welcome` method to include two parameters as shown in the following code.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

<span data-ttu-id="7c119-178">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="7c119-178">The preceding code:</span></span>

* <span data-ttu-id="7c119-179">使用 C# 選擇性參數功能來指出若未針對 `numTimes` 參數傳遞任何值時，該參數預設為 1。</span><span class="sxs-lookup"><span data-stu-id="7c119-179">Uses the C# optional-parameter feature to indicate that the `numTimes` parameter defaults to 1 if no value is passed for that parameter.</span></span> <!-- remove for simplified -->
* <span data-ttu-id="7c119-180">使用 `HtmlEncoder.Default.Encode` 來保護應用程式免於遭受惡意輸入 (也就是 JavaScript) 攻擊。</span><span class="sxs-lookup"><span data-stu-id="7c119-180">Uses `HtmlEncoder.Default.Encode` to protect the app from malicious input (namely JavaScript).</span></span>
* <span data-ttu-id="7c119-181">在 `$"Hello {name}, NumTimes is: {numTimes}"` 中使用[字串插值](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings)。</span><span class="sxs-lookup"><span data-stu-id="7c119-181">Uses [Interpolated Strings](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings) in `$"Hello {name}, NumTimes is: {numTimes}"`.</span></span> <!-- remove for simplified -->

<span data-ttu-id="7c119-182">執行應用程式，然後瀏覽至：</span><span class="sxs-lookup"><span data-stu-id="7c119-182">Run the app and browse to:</span></span>

   `https://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

<span data-ttu-id="7c119-183">(將 xxxx 取代為您的連接埠編號)。您可以在 URL 中針對 `name` 和 `numtimes` 嘗試不同的值。</span><span class="sxs-lookup"><span data-stu-id="7c119-183">(Replace xxxx with your port number.) You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="7c119-184">MVC [模型繫結](xref:mvc/models/model-binding)系統會自動將網址列上查詢字串中的具名參數對應至方法中的參數。</span><span class="sxs-lookup"><span data-stu-id="7c119-184">The MVC [model binding](xref:mvc/models/model-binding) system automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="7c119-185">如需詳細資訊，請參閱[模型繫結](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="7c119-185">See [Model Binding](xref:mvc/models/model-binding) for more information.</span></span>

![瀏覽器視窗，其中顯示應用程式回應 Hello Rick, NumTimes is:4](~/tutorials/first-mvc-app/adding-controller/_static/rick4.png)

<span data-ttu-id="7c119-187">在上方的影像中，不會使用 URL 區段 (`Parameters`) ，`name` 和 `numTimes` 則以[查詢字串](https://wikipedia.org/wiki/Query_string)的方式傳遞。</span><span class="sxs-lookup"><span data-stu-id="7c119-187">In the image above, the URL segment (`Parameters`) isn't used, the `name` and `numTimes` parameters are passed as [query strings](https://wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="7c119-188">上述 URL 中的 `?` (問號) 是分隔符號，隨後接著查詢字串。</span><span class="sxs-lookup"><span data-stu-id="7c119-188">The `?` (question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="7c119-189">`&` 字元可分隔查詢字串。</span><span class="sxs-lookup"><span data-stu-id="7c119-189">The `&` character separates query strings.</span></span>

<span data-ttu-id="7c119-190">以下列程式碼取代 `Welcome` 方法：</span><span class="sxs-lookup"><span data-stu-id="7c119-190">Replace the `Welcome` method with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

<span data-ttu-id="7c119-191">執行應用程式，並輸入下列 URL：`https://localhost:xxx/HelloWorld/Welcome/3?name=Rick`</span><span class="sxs-lookup"><span data-stu-id="7c119-191">Run the app and enter the following URL: `https://localhost:xxx/HelloWorld/Welcome/3?name=Rick`</span></span>

<span data-ttu-id="7c119-192">此時，第三個 URL 區段符合路由參數 `id`。</span><span class="sxs-lookup"><span data-stu-id="7c119-192">This time the third URL segment matched the route parameter `id`.</span></span> <span data-ttu-id="7c119-193">`Welcome` 方法包含符合 `MapControllerRoute` 方法中 URL 範本的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="7c119-193">The `Welcome` method contains a parameter `id` that matched the URL template in the `MapControllerRoute` method.</span></span> <span data-ttu-id="7c119-194">結尾的 `?` (在 `id?` 中) 表示 `id` 是選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="7c119-194">The trailing `?` (in `id?`) indicates the `id` parameter is optional.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_1&highlight=5)]

<span data-ttu-id="7c119-195">在這些範例中，控制器已執行 MVC 的 "VC" 部分，也就是 **V**iew 和 **C**ontroller 工作。</span><span class="sxs-lookup"><span data-stu-id="7c119-195">In these examples the controller has been doing the "VC" portion of MVC - that is, the **V**iew and the **C**ontroller work.</span></span> <span data-ttu-id="7c119-196">控制器會直接傳回 HTML。</span><span class="sxs-lookup"><span data-stu-id="7c119-196">The controller is returning HTML directly.</span></span> <span data-ttu-id="7c119-197">一般來說，您不希望控制器直接傳回 HTML，因為撰寫程式碼和維護會變得很麻煩。</span><span class="sxs-lookup"><span data-stu-id="7c119-197">Generally you don't want controllers returning HTML directly, since that becomes very cumbersome to code and maintain.</span></span> <span data-ttu-id="7c119-198">相反地，您通常使用個別的 Razor 檢視範本檔案來產生 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="7c119-198">Instead you typically use a separate Razor view template file to generate the HTML response.</span></span> <span data-ttu-id="7c119-199">您可在接下來的教學課程中這麼做。</span><span class="sxs-lookup"><span data-stu-id="7c119-199">You do that in the next tutorial.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7c119-200">[上一頁](start-mvc.md)
> [下一頁](adding-view.md)</span><span class="sxs-lookup"><span data-stu-id="7c119-200">[Previous](start-mvc.md)
[Next](adding-view.md)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7c119-201">Model-View-Controller (MVC) 架構模式可將一個應用程式劃分成三個主要元件：模型 (**M**)、檢視 (**V**) 和控制器 (**C**)。</span><span class="sxs-lookup"><span data-stu-id="7c119-201">The Model-View-Controller (MVC) architectural pattern separates an app into three main components: **M**odel, **V**iew, and **C**ontroller.</span></span> <span data-ttu-id="7c119-202">MVC 模式可協助您建立比傳統整合型應用程式更可測試且更易於更新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c119-202">The MVC pattern helps you create apps that are more testable and easier to update than traditional monolithic apps.</span></span> <span data-ttu-id="7c119-203">MVC 架構的應用程式包含：</span><span class="sxs-lookup"><span data-stu-id="7c119-203">MVC-based apps contain:</span></span>

* <span data-ttu-id="7c119-204">模型 (**M**)：代表應用程式資料的類別。</span><span class="sxs-lookup"><span data-stu-id="7c119-204">**M**odels: Classes that represent the data of the app.</span></span> <span data-ttu-id="7c119-205">模型類別使用驗證邏輯對該資料強制執行商務規則。</span><span class="sxs-lookup"><span data-stu-id="7c119-205">The model classes use validation logic to enforce business rules for that data.</span></span> <span data-ttu-id="7c119-206">通常，模型物件會在資料庫中擷取並儲存模型狀態。</span><span class="sxs-lookup"><span data-stu-id="7c119-206">Typically, model objects retrieve and store model state in a database.</span></span> <span data-ttu-id="7c119-207">在本教學課程中，`Movie` 模型會從資料庫擷取電影資料，將其提供給檢視或更新它。</span><span class="sxs-lookup"><span data-stu-id="7c119-207">In this tutorial, a `Movie` model retrieves movie data from a database, provides it to the view or updates it.</span></span> <span data-ttu-id="7c119-208">更新的資料會寫入資料庫。</span><span class="sxs-lookup"><span data-stu-id="7c119-208">Updated data is written to a database.</span></span>

* <span data-ttu-id="7c119-209">檢視 (**V**)：檢視是顯示應用程式使用者介面 (UI) 的元件。</span><span class="sxs-lookup"><span data-stu-id="7c119-209">**V**iews: Views are the components that display the app's user interface (UI).</span></span> <span data-ttu-id="7c119-210">一般而言，此 UI 會顯示模型資料。</span><span class="sxs-lookup"><span data-stu-id="7c119-210">Generally, this UI displays the model data.</span></span>

* <span data-ttu-id="7c119-211">控制器 (**C**)：處理瀏覽器要求的類別。</span><span class="sxs-lookup"><span data-stu-id="7c119-211">**C**ontrollers: Classes that handle browser requests.</span></span> <span data-ttu-id="7c119-212">它們會擷取模型資料，並呼叫傳回回應的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="7c119-212">They retrieve model data and call view templates that return a response.</span></span> <span data-ttu-id="7c119-213">在 MVC 應用程式中，檢視只能顯示資訊；控制器則會處理和回應使用者輸入和互動。</span><span class="sxs-lookup"><span data-stu-id="7c119-213">In an MVC app, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="7c119-214">例如，控制器會處理路由資料和查詢字串值，並將這些值傳遞至模型。</span><span class="sxs-lookup"><span data-stu-id="7c119-214">For example, the controller handles route data and query-string values, and passes these values to the model.</span></span> <span data-ttu-id="7c119-215">此模型可能會使用這些值來查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="7c119-215">The model might use these values to query the database.</span></span> <span data-ttu-id="7c119-216">例如，`https://localhost:5001/Home/About` 具有 `Home` (控制器) 和 `About` (在首頁控制器上呼叫的動作方法) 的路由資料。</span><span class="sxs-lookup"><span data-stu-id="7c119-216">For example, `https://localhost:5001/Home/About` has route data of `Home` (the controller) and `About` (the action method to call on the home controller).</span></span> <span data-ttu-id="7c119-217">`https://localhost:5001/Movies/Edit/5` 是要使用電影控制器編輯識別碼 = 5 之電影的要求。</span><span class="sxs-lookup"><span data-stu-id="7c119-217">`https://localhost:5001/Movies/Edit/5` is a request to edit the movie with ID=5 using the movie controller.</span></span> <span data-ttu-id="7c119-218">本教學課程稍後會說明路由資料。</span><span class="sxs-lookup"><span data-stu-id="7c119-218">Route data is explained later in the tutorial.</span></span>

<span data-ttu-id="7c119-219">MVC 模式可協助您建立應用程式，用來隔離應用程的不同層面 (輸入邏輯、商務邏輯和 UI 邏輯)，同時提供這些項目之間的鬆散結合。</span><span class="sxs-lookup"><span data-stu-id="7c119-219">The MVC pattern helps you create apps that separate the different aspects of the app (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="7c119-220">此模式指定每一種邏輯應該位於應用程式中的位置。</span><span class="sxs-lookup"><span data-stu-id="7c119-220">The pattern specifies where each kind of logic should be located in the app.</span></span> <span data-ttu-id="7c119-221">UI 邏輯位於檢視。</span><span class="sxs-lookup"><span data-stu-id="7c119-221">The UI logic belongs in the view.</span></span> <span data-ttu-id="7c119-222">輸入邏輯位於控制器。</span><span class="sxs-lookup"><span data-stu-id="7c119-222">Input logic belongs in the controller.</span></span> <span data-ttu-id="7c119-223">商務邏輯則位於模型。</span><span class="sxs-lookup"><span data-stu-id="7c119-223">Business logic belongs in the model.</span></span> <span data-ttu-id="7c119-224">這項隔離可協助您管理建置應用程式時的複雜度，因為它可讓您一次處理實作的其中一個層面，而不影響另一個層面的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7c119-224">This separation helps you manage complexity when you build an app, because it enables you to work on one aspect of the implementation at a time without impacting the code of another.</span></span> <span data-ttu-id="7c119-225">例如，您可以處理檢視程式碼，而不需要根據商務邏輯程式碼。</span><span class="sxs-lookup"><span data-stu-id="7c119-225">For example, you can work on the view code without depending on the business logic code.</span></span>

<span data-ttu-id="7c119-226">本教學課程系列中涵蓋了這些概念，並且顯示如何使用它們來建置電影應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c119-226">We cover these concepts in this tutorial series and show you how to use them to build a movie app.</span></span> <span data-ttu-id="7c119-227">MVC 專案包含｢控制器｣  和｢檢視｣  的資料夾。</span><span class="sxs-lookup"><span data-stu-id="7c119-227">The MVC project contains folders for the *Controllers* and *Views*.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="7c119-228">新增控制器</span><span class="sxs-lookup"><span data-stu-id="7c119-228">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7c119-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c119-229">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7c119-230">在 [方案總管]  中，以滑鼠右鍵按一下 [控制器] > [新增] > [控制器]  
  ![操作功能表](adding-controller/_static/add_controller.png)</span><span class="sxs-lookup"><span data-stu-id="7c119-230">In **Solution Explorer**, right-click **Controllers > Add > Controller**
![Contextual menu](adding-controller/_static/add_controller.png)</span></span>

* <span data-ttu-id="7c119-231">在 [新增 Scaffold]  對話方塊中，選取 [MVC 控制器 - 空白] </span><span class="sxs-lookup"><span data-stu-id="7c119-231">In the **Add Scaffold** dialog box, select **MVC Controller - Empty**</span></span>

  ![新增 MVC 控制器並將其命名](adding-controller/_static/ac.png)

* <span data-ttu-id="7c119-233">在 [Add Empty MVC Controller] \(新增空白 MVC 控制器\)  對話方塊中，輸入 **HelloWorldController**，然後選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="7c119-233">In the **Add Empty MVC Controller dialog**, enter **HelloWorldController** and select **ADD**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7c119-234">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c119-234">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7c119-235">選取**總管**圖示，然後 Control+按一下 (按一下滑鼠右鍵) [控制器] > [新增檔案]  ，將新檔案命名為 *HelloWorldController.cs*。</span><span class="sxs-lookup"><span data-stu-id="7c119-235">Select the **EXPLORER** icon and then control-click (right-click) **Controllers > New File** and name the new file *HelloWorldController.cs*.</span></span>

  ![操作功能表](~/tutorials/first-mvc-app-xplat/adding-controller/_static/new_file.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7c119-237">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7c119-237">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7c119-238">在方案總管  中，以滑鼠右鍵按一下 [控制器] > [新增] > [新增檔案]  。</span><span class="sxs-lookup"><span data-stu-id="7c119-238">In **Solution Explorer**, right-click **Controllers > Add > New File**.</span></span>
<span data-ttu-id="7c119-239">![操作功能表](~/tutorials/first-mvc-app-mac/adding-controller/_static/add_controller.png)</span><span class="sxs-lookup"><span data-stu-id="7c119-239">![Contextual menu](~/tutorials/first-mvc-app-mac/adding-controller/_static/add_controller.png)</span></span>

<span data-ttu-id="7c119-240">選取 [ASP.NET Core]  和 [MVC 控制器類別]  。</span><span class="sxs-lookup"><span data-stu-id="7c119-240">Select **ASP.NET Core** and **MVC Controller Class**.</span></span>

<span data-ttu-id="7c119-241">將控制器命名為 **HelloWorldController**。</span><span class="sxs-lookup"><span data-stu-id="7c119-241">Name the controller **HelloWorldController**.</span></span>

![新增 MVC 控制器並將其命名](~/tutorials/first-mvc-app-mac/adding-controller/_static/ac.png)

---

<span data-ttu-id="7c119-243">以下列內容取代 *Controllers/HelloWorldController.cs* 的內容：</span><span class="sxs-lookup"><span data-stu-id="7c119-243">Replace the contents of *Controllers/HelloWorldController.cs* with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

<span data-ttu-id="7c119-244">控制器中的每個 `public` 方法可呼叫作為 HTTP 端點。</span><span class="sxs-lookup"><span data-stu-id="7c119-244">Every `public` method in a controller is callable as an HTTP endpoint.</span></span> <span data-ttu-id="7c119-245">在上述範例中，這兩種方法都會傳回一個字串。</span><span class="sxs-lookup"><span data-stu-id="7c119-245">In the sample above, both methods return a string.</span></span> <span data-ttu-id="7c119-246">請注意每個方法之前的註解。</span><span class="sxs-lookup"><span data-stu-id="7c119-246">Note the comments preceding each method.</span></span>

<span data-ttu-id="7c119-247">HTTP 端點是 Web 應用程式中的可設定目標 URL，例如 `https://localhost:5001/HelloWorld`，其結合使用的通訊協定：`HTTPS`、網頁伺服器的網路位置 (包括 TCP 連接埠)：`localhost:5001`，以及目標 URI `HelloWorld`。</span><span class="sxs-lookup"><span data-stu-id="7c119-247">An HTTP endpoint is a targetable URL in the web application, such as `https://localhost:5001/HelloWorld`, and combines the protocol used: `HTTPS`, the network location of the web server (including the TCP port): `localhost:5001` and the target URI `HelloWorld`.</span></span>

<span data-ttu-id="7c119-248">第一個註解指出這是 [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) 方法，叫用方法是將 `/HelloWorld/` 附加至基底 URL。</span><span class="sxs-lookup"><span data-stu-id="7c119-248">The first comment states this is an [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) method that's invoked by appending `/HelloWorld/` to the base URL.</span></span> <span data-ttu-id="7c119-249">第二個註解會指定 [HTTP GET](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) 方法，叫用方法是將 `/HelloWorld/Welcome/` 附加至 URL。</span><span class="sxs-lookup"><span data-stu-id="7c119-249">The second comment specifies an [HTTP GET](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) method that's invoked by appending `/HelloWorld/Welcome/` to the URL.</span></span> <span data-ttu-id="7c119-250">稍後在本教學課程中，您將使用 Scaffolding 引擎產生 `HTTP POST` 方法來更新資料。</span><span class="sxs-lookup"><span data-stu-id="7c119-250">Later on in the tutorial the scaffolding engine is used to generate `HTTP POST` methods which update data.</span></span>

<span data-ttu-id="7c119-251">在非偵錯模式中執行應用程式，並將 "HelloWorld" 附加至網址列中的路徑。</span><span class="sxs-lookup"><span data-stu-id="7c119-251">Run the app in non-debug mode and append "HelloWorld" to the path in the address bar.</span></span> <span data-ttu-id="7c119-252">`Index` 方法會傳回一個字串。</span><span class="sxs-lookup"><span data-stu-id="7c119-252">The `Index` method returns a string.</span></span>

![顯示應用程式回應 "This is my default action" 的瀏覽器視窗](~/tutorials/first-mvc-app/adding-controller/_static/hell1.png)

<span data-ttu-id="7c119-254">MVC 會根據傳入 URL 叫用控制器類別 (和其中的動作方法)。</span><span class="sxs-lookup"><span data-stu-id="7c119-254">MVC invokes controller classes (and the action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="7c119-255">MVC 使用的預設 [URL 路由邏輯](xref:mvc/controllers/routing)使用像這樣的格式來判斷要叫用的程式碼：</span><span class="sxs-lookup"><span data-stu-id="7c119-255">The default [URL routing logic](xref:mvc/controllers/routing) used by MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="7c119-256">您可以在 *Startup.cs* 檔案的 `Configure` 方法中設定路由格式。</span><span class="sxs-lookup"><span data-stu-id="7c119-256">The routing format is set in the `Configure` method in *Startup.cs* file.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

<!-- 
Add link to explain lambda.
Remove link for simplified tutorial.
-->

<span data-ttu-id="7c119-257">當您瀏覽至應用程式而不提供任何 URL 區段時，則會預設為上方醒目提示之範本行中指定的 "Home" 控制器和 "Index" 方法。</span><span class="sxs-lookup"><span data-stu-id="7c119-257">When you browse to the app and don't supply any URL segments, it defaults to the "Home" controller and the "Index" method specified in the template line highlighted above.</span></span>

<span data-ttu-id="7c119-258">第一個 URL 區段決定要執行的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="7c119-258">The first URL segment determines the controller class to run.</span></span> <span data-ttu-id="7c119-259">因此，`localhost:xxxx/HelloWorld` 會對應至 `HelloWorldController` 類別。</span><span class="sxs-lookup"><span data-stu-id="7c119-259">So `localhost:xxxx/HelloWorld` maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="7c119-260">URL 區段的第二部分則決定類別上的動作方法。</span><span class="sxs-lookup"><span data-stu-id="7c119-260">The second part of the URL segment determines the action method on the class.</span></span> <span data-ttu-id="7c119-261">因此，`localhost:xxxx/HelloWorld/Index` 會導致 `HelloWorldController` 類別的 `Index` 方法執行。</span><span class="sxs-lookup"><span data-stu-id="7c119-261">So `localhost:xxxx/HelloWorld/Index` would cause the `Index` method of the `HelloWorldController` class to run.</span></span> <span data-ttu-id="7c119-262">請注意，您只需要瀏覽至 `localhost:xxxx/HelloWorld`，根據預設就會呼叫 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="7c119-262">Notice that you only had to browse to `localhost:xxxx/HelloWorld` and the `Index` method was called by default.</span></span> <span data-ttu-id="7c119-263">這是因為 `Index` 是未明確指定方法名稱時，在控制器上呼叫的預設方法。</span><span class="sxs-lookup"><span data-stu-id="7c119-263">This is because `Index` is the default method that will be called on a controller if a method name isn't explicitly specified.</span></span> <span data-ttu-id="7c119-264">URL 區段的第三個部分 (`id`) 是路由資料。</span><span class="sxs-lookup"><span data-stu-id="7c119-264">The third part of the URL segment ( `id`) is for route data.</span></span> <span data-ttu-id="7c119-265">本教學課程稍後會說明路由資料。</span><span class="sxs-lookup"><span data-stu-id="7c119-265">Route data is explained later in the tutorial.</span></span>

<span data-ttu-id="7c119-266">瀏覽至 `https://localhost:xxxx/HelloWorld/Welcome`。</span><span class="sxs-lookup"><span data-stu-id="7c119-266">Browse to `https://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="7c119-267">`Welcome` 方法隨即執行，並傳回字串 `This is the Welcome action method...`。</span><span class="sxs-lookup"><span data-stu-id="7c119-267">The `Welcome` method runs and returns the string `This is the Welcome action method...`.</span></span> <span data-ttu-id="7c119-268">在此 URL 中，控制器是 `HelloWorld`，而 `Welcome` 是動作方法。</span><span class="sxs-lookup"><span data-stu-id="7c119-268">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="7c119-269">您尚未使用 URL 的 `[Parameters]` 部分。</span><span class="sxs-lookup"><span data-stu-id="7c119-269">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![顯示應用程式回應 "This is the Welcome action method" 的瀏覽器視窗](~/tutorials/first-mvc-app/adding-controller/_static/welcome.png)

<span data-ttu-id="7c119-271">修改程式碼 ，將 URL 中的某些參數資訊傳遞到控制器。</span><span class="sxs-lookup"><span data-stu-id="7c119-271">Modify the code to pass some parameter information from the URL to the controller.</span></span> <span data-ttu-id="7c119-272">例如，`/HelloWorld/Welcome?name=Rick&numtimes=4`。</span><span class="sxs-lookup"><span data-stu-id="7c119-272">For example, `/HelloWorld/Welcome?name=Rick&numtimes=4`.</span></span> <span data-ttu-id="7c119-273">變更 `Welcome` 方法以包含兩個參數，如下列程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="7c119-273">Change the `Welcome` method to include two parameters as shown in the following code.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

<span data-ttu-id="7c119-274">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="7c119-274">The preceding code:</span></span>

* <span data-ttu-id="7c119-275">使用 C# 選擇性參數功能來指出若未針對 `numTimes` 參數傳遞任何值時，該參數預設為 1。</span><span class="sxs-lookup"><span data-stu-id="7c119-275">Uses the C# optional-parameter feature to indicate that the `numTimes` parameter defaults to 1 if no value is passed for that parameter.</span></span> <!-- remove for simplified -->
* <span data-ttu-id="7c119-276">使用 `HtmlEncoder.Default.Encode` 來保護應用程式免於遭受惡意輸入 (也就是 JavaScript) 攻擊。</span><span class="sxs-lookup"><span data-stu-id="7c119-276">Uses `HtmlEncoder.Default.Encode` to protect the app from malicious input (namely JavaScript).</span></span>
* <span data-ttu-id="7c119-277">在 `$"Hello {name}, NumTimes is: {numTimes}"` 中使用[字串插值](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings)。</span><span class="sxs-lookup"><span data-stu-id="7c119-277">Uses [Interpolated Strings](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings) in `$"Hello {name}, NumTimes is: {numTimes}"`.</span></span> <!-- remove for simplified -->

<span data-ttu-id="7c119-278">執行應用程式，然後瀏覽至：</span><span class="sxs-lookup"><span data-stu-id="7c119-278">Run the app and browse to:</span></span>

   `https://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

<span data-ttu-id="7c119-279">(將 xxxx 取代為您的連接埠編號)。您可以在 URL 中針對 `name` 和 `numtimes` 嘗試不同的值。</span><span class="sxs-lookup"><span data-stu-id="7c119-279">(Replace xxxx with your port number.) You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="7c119-280">MVC [模型繫結](xref:mvc/models/model-binding)系統會自動將網址列上查詢字串中的具名參數對應至方法中的參數。</span><span class="sxs-lookup"><span data-stu-id="7c119-280">The MVC [model binding](xref:mvc/models/model-binding) system automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="7c119-281">如需詳細資訊，請參閱[模型繫結](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="7c119-281">See [Model Binding](xref:mvc/models/model-binding) for more information.</span></span>

![瀏覽器視窗，其中顯示應用程式回應 Hello Rick, NumTimes is:4](~/tutorials/first-mvc-app/adding-controller/_static/rick4.png)

<span data-ttu-id="7c119-283">在上方的影像中，不會使用 URL 區段 (`Parameters`) ，`name` 和 `numTimes` 則以[查詢字串](https://wikipedia.org/wiki/Query_string)的方式傳遞。</span><span class="sxs-lookup"><span data-stu-id="7c119-283">In the image above, the URL segment (`Parameters`) isn't used, the `name` and `numTimes` parameters are passed as [query strings](https://wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="7c119-284">上述 URL 中的 `?` (問號) 是分隔符號，隨後接著查詢字串。</span><span class="sxs-lookup"><span data-stu-id="7c119-284">The `?` (question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="7c119-285">`&` 字元可分隔查詢字串。</span><span class="sxs-lookup"><span data-stu-id="7c119-285">The `&` character separates query strings.</span></span>

<span data-ttu-id="7c119-286">以下列程式碼取代 `Welcome` 方法：</span><span class="sxs-lookup"><span data-stu-id="7c119-286">Replace the `Welcome` method with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

<span data-ttu-id="7c119-287">執行應用程式，並輸入下列 URL：`https://localhost:xxx/HelloWorld/Welcome/3?name=Rick`</span><span class="sxs-lookup"><span data-stu-id="7c119-287">Run the app and enter the following URL: `https://localhost:xxx/HelloWorld/Welcome/3?name=Rick`</span></span>

<span data-ttu-id="7c119-288">此時，第三個 URL 區段符合路由參數 `id`。</span><span class="sxs-lookup"><span data-stu-id="7c119-288">This time the third URL segment matched the route parameter `id`.</span></span> <span data-ttu-id="7c119-289">`Welcome` 方法包含符合 `MapRoute` 方法中 URL 範本的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="7c119-289">The `Welcome` method contains a parameter `id` that matched the URL template in the `MapRoute` method.</span></span> <span data-ttu-id="7c119-290">結尾的 `?` (在 `id?` 中) 表示 `id` 是選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="7c119-290">The trailing `?` (in `id?`) indicates the `id` parameter is optional.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

<span data-ttu-id="7c119-291">在這些範例中，控制器已執行 MVC 的 "VC" 部分，也就是檢視和控制器工作。</span><span class="sxs-lookup"><span data-stu-id="7c119-291">In these examples the controller has been doing the "VC" portion of MVC - that is, the view and controller work.</span></span> <span data-ttu-id="7c119-292">控制器會直接傳回 HTML。</span><span class="sxs-lookup"><span data-stu-id="7c119-292">The controller is returning HTML directly.</span></span> <span data-ttu-id="7c119-293">一般來說，您不希望控制器直接傳回 HTML，因為撰寫程式碼和維護會變得很麻煩。</span><span class="sxs-lookup"><span data-stu-id="7c119-293">Generally you don't want controllers returning HTML directly, since that becomes very cumbersome to code and maintain.</span></span> <span data-ttu-id="7c119-294">相反地，您通常使用個別的 Razor 檢視範本檔案來協助產生 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="7c119-294">Instead you typically use a separate Razor view template file to help generate the HTML response.</span></span> <span data-ttu-id="7c119-295">您可在接下來的教學課程中這麼做。</span><span class="sxs-lookup"><span data-stu-id="7c119-295">You do that in the next tutorial.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7c119-296">[上一頁](start-mvc.md)
> [下一頁](adding-view.md)</span><span class="sxs-lookup"><span data-stu-id="7c119-296">[Previous](start-mvc.md)
[Next](adding-view.md)</span></span>

::: moniker-end
