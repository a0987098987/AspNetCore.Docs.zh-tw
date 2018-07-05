---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: 新增控制器 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 77389bfa4774857eb2a607b0a70e982826312e03
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802288"
---
<a name="adding-a-controller"></a><span data-ttu-id="0e87c-102">新增控制器</span><span class="sxs-lookup"><span data-stu-id="0e87c-102">Adding a Controller</span></span>
====================
<span data-ttu-id="0e87c-103">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0e87c-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="0e87c-104">代表 MVC*模型-檢視-控制器*。</span><span class="sxs-lookup"><span data-stu-id="0e87c-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="0e87c-105">MVC 是用於開發也架構，可測試且易於維護的應用程式的模式。</span><span class="sxs-lookup"><span data-stu-id="0e87c-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="0e87c-106">MVC 架構的應用程式包含：</span><span class="sxs-lookup"><span data-stu-id="0e87c-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="0e87c-107">**M**模型： 代表應用程式的資料，並使用驗證邏輯來強制執行商務規則，該資料的類別。</span><span class="sxs-lookup"><span data-stu-id="0e87c-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="0e87c-108">**V** iews： 您的應用程式用來動態產生 HTML 回應的範本檔案。</span><span class="sxs-lookup"><span data-stu-id="0e87c-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="0e87c-109">**C** ontrollers： 類別，可處理連入的瀏覽器要求，擷取模型資料，然後指定 將回應傳回給瀏覽器的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="0e87c-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="0e87c-110">我們就可以涵蓋在本教學課程系列中的所有這些概念，並示範如何使用它們來建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="0e87c-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

> [!NOTE]
> <span data-ttu-id="0e87c-111">預設的 MVC 上一個步驟中選取範本。</span><span class="sxs-lookup"><span data-stu-id="0e87c-111">In the previous step the Default MVC template was selected.</span></span> <span data-ttu-id="0e87c-112">這會建立主資料夾]、 帳戶和預設的 [管理控制器。</span><span class="sxs-lookup"><span data-stu-id="0e87c-112">This creates Home, Account and Manage Controllers by default.</span></span>

<span data-ttu-id="0e87c-113">讓我們開始建立控制器類別。</span><span class="sxs-lookup"><span data-stu-id="0e87c-113">Let's begin by creating a controller class.</span></span> <span data-ttu-id="0e87c-114">在 [**方案總管] 中**，以滑鼠右鍵按一下*控制站*資料夾，然後按一下**新增**，然後**控制器**。</span><span class="sxs-lookup"><span data-stu-id="0e87c-114">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>


![](adding-a-controller/_static/image1.png)

<span data-ttu-id="0e87c-115">在 [**新增 Scaffold** ] 對話方塊中，按一下**MVC 5 控制器-空白**，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="0e87c-115">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  
 

<span data-ttu-id="0e87c-116">將新的控制器命名為"HelloWorldController 」，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="0e87c-116">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![新增控制器](adding-a-controller/_static/image3.png)

<span data-ttu-id="0e87c-118">請注意，在**方案總管] 中**新的檔案已經建立的具名*HelloWorldController.cs*和 [新資料夾*Views\HelloWorld*。</span><span class="sxs-lookup"><span data-stu-id="0e87c-118">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="0e87c-119">控制器是在 IDE 中開啟。</span><span class="sxs-lookup"><span data-stu-id="0e87c-119">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="0e87c-120">取代下列程式碼檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="0e87c-120">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="0e87c-121">控制器方法會傳回 HTML 的字串做為範例。</span><span class="sxs-lookup"><span data-stu-id="0e87c-121">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="0e87c-122">名為控制器`HelloWorldController`第一種方法稱為`Index`。</span><span class="sxs-lookup"><span data-stu-id="0e87c-122">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="0e87c-123">讓我們叫用它從瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="0e87c-123">Let's invoke it from a browser.</span></span> <span data-ttu-id="0e87c-124">執行應用程式 （按 F5 或 ctrl+f5）。</span><span class="sxs-lookup"><span data-stu-id="0e87c-124">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="0e87c-125">在瀏覽器中，附加&quot;HelloWorld&quot; [網址] 列中的路徑。</span><span class="sxs-lookup"><span data-stu-id="0e87c-125">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="0e87c-126">(例如，在圖中，其下的`http://localhost:1234/HelloWorld.`) 的瀏覽器頁面看起來像下列螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="0e87c-126">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="0e87c-127">在上述的方法中，程式碼會直接傳回字串。</span><span class="sxs-lookup"><span data-stu-id="0e87c-127">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="0e87c-128">您告訴系統只會傳回一些 HTML，且啟動成功 ！</span><span class="sxs-lookup"><span data-stu-id="0e87c-128">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="0e87c-129">根據傳入 URL 不同控制器類別 （和不同的動作方法），會叫用 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="0e87c-129">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="0e87c-130">使用 ASP.NET MVC 的預設值的 URL 路由邏輯會使用像這樣的格式，以判斷哪些程式碼叫用：</span><span class="sxs-lookup"><span data-stu-id="0e87c-130">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="0e87c-131">您設定的格式中的路由*應用程式\_Start/RouteConfig.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="0e87c-131">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="0e87c-132">當您執行應用程式，並不提供任何 URL 區段時，則會預設為"Home"控制器和"Index"動作方法的區段中指定預設值的上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="0e87c-132">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="0e87c-133">URL 的第一個部分會判斷要執行的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="0e87c-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="0e87c-134">因此 */HelloWorld*對應至`HelloWorldController`類別。</span><span class="sxs-lookup"><span data-stu-id="0e87c-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="0e87c-135">URL 的第二部分會判斷要執行的類別上的動作方法。</span><span class="sxs-lookup"><span data-stu-id="0e87c-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="0e87c-136">因此 */HelloWorld/Index*會導致`Index`方法`HelloWorldController`類別來執行。</span><span class="sxs-lookup"><span data-stu-id="0e87c-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="0e87c-137">請注意，我們只需要瀏覽至 */HelloWorld*而`Index`方法已由預設值。</span><span class="sxs-lookup"><span data-stu-id="0e87c-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="0e87c-138">這是因為方法命名為`Index`是會在控制器呼叫，如果沒有明確指定的預設方法。</span><span class="sxs-lookup"><span data-stu-id="0e87c-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="0e87c-139">URL 區段的第三個部分 (`Parameters`) 是路由資料。</span><span class="sxs-lookup"><span data-stu-id="0e87c-139">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="0e87c-140">我們將在本教學課程中，於稍後看到路由資料。</span><span class="sxs-lookup"><span data-stu-id="0e87c-140">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="0e87c-141">瀏覽至 `http://localhost:xxxx/HelloWorld/Welcome`。</span><span class="sxs-lookup"><span data-stu-id="0e87c-141">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="0e87c-142">`Welcome`方法會執行，且會傳回字串&quot;這是 歡迎使用動作方法...&quot;.</span><span class="sxs-lookup"><span data-stu-id="0e87c-142">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="0e87c-143">預設的 MVC 對應是`/[Controller]/[ActionName]/[Parameters]`。</span><span class="sxs-lookup"><span data-stu-id="0e87c-143">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="0e87c-144">在此 URL 中，控制器是 `HelloWorld`，而 `Welcome` 是動作方法。</span><span class="sxs-lookup"><span data-stu-id="0e87c-144">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="0e87c-145">您尚未使用 URL 的 `[Parameters]` 部分。</span><span class="sxs-lookup"><span data-stu-id="0e87c-145">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="0e87c-146">讓我們在範例稍微修改，讓您可以將從 URL 的某些參數資訊傳遞到控制器 (例如 */HelloWorld/歡迎？ 名稱 = Scott&amp;numtimes = 4*)。</span><span class="sxs-lookup"><span data-stu-id="0e87c-146">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="0e87c-147">變更您`Welcome`方法以包含兩個參數，如下所示。</span><span class="sxs-lookup"><span data-stu-id="0e87c-147">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="0e87c-148">請注意，程式碼會使用 C# 選擇性參數功能，在表示`numTimes`參數應該預設為 1，如果沒有傳遞任何值，該參數。</span><span class="sxs-lookup"><span data-stu-id="0e87c-148">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="0e87c-149">安全性注意事項： 上述程式碼會使用[HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx)來保護應用程式免於遭受惡意輸入 (也就是 JavaScript)。</span><span class="sxs-lookup"><span data-stu-id="0e87c-149">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="0e87c-150">如需詳細資訊，請參閱[如何： 保護對指令碼會利用在 Web 應用程式中藉由套用 HTML 編碼字串](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="0e87c-150">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>


 <span data-ttu-id="0e87c-151">執行應用程式，並瀏覽至範例 URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`)。</span><span class="sxs-lookup"><span data-stu-id="0e87c-151">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="0e87c-152">您可以嘗試不同的值，如`name`和`numtimes`在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="0e87c-152">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="0e87c-153">[ASP.NET MVC 模型繫結系統](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)會自動將對應方法中的參數從查詢字串，在網址列中的具名的參數。</span><span class="sxs-lookup"><span data-stu-id="0e87c-153">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="0e87c-154">在範例中，上述的 URL 區段 ( `Parameters`) 未使用，`name`並`numTimes`參數會當做傳遞[查詢字串](http://en.wikipedia.org/wiki/Query_string)。</span><span class="sxs-lookup"><span data-stu-id="0e87c-154">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="0e87c-155">?</span><span class="sxs-lookup"><span data-stu-id="0e87c-155">The ?</span></span> <span data-ttu-id="0e87c-156">（問號） 上述 URL 中是分隔符號，並查詢字串遵循。</span><span class="sxs-lookup"><span data-stu-id="0e87c-156">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="0e87c-157">&amp; 字元可分隔查詢字串。</span><span class="sxs-lookup"><span data-stu-id="0e87c-157">The &amp; character separates query strings.</span></span>

<span data-ttu-id="0e87c-158">取代下列程式碼中的 歡迎使用方法：</span><span class="sxs-lookup"><span data-stu-id="0e87c-158">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="0e87c-159">執行應用程式，並輸入下列 URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="0e87c-159">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="0e87c-160">這次的第三個 URL 區段符合路由參數`ID.``Welcome`動作方法中包含的參數 (`ID`)，比對中的 URL 規格`RegisterRoutes`方法。</span><span class="sxs-lookup"><span data-stu-id="0e87c-160">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="0e87c-161">在 ASP.NET MVC 應用程式，通常會更傳入作為路由資料 （如同上述的識別碼） 比將它們傳遞作為查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="0e87c-161">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="0e87c-162">您也可以加入的路由將兩者`name`和`numtimes`參數做為 URL 中的路由資料中。</span><span class="sxs-lookup"><span data-stu-id="0e87c-162">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="0e87c-163">在 *應用程式\_Start\RouteConfig.cs*檔案中，新增"Hello"路由：</span><span class="sxs-lookup"><span data-stu-id="0e87c-163">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="0e87c-164">執行應用程式，並瀏覽至`/localhost:XXX/HelloWorld/Welcome/Scott/3`。</span><span class="sxs-lookup"><span data-stu-id="0e87c-164">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="0e87c-165">對於許多的 MVC 應用程式，預設路由可正常運作。</span><span class="sxs-lookup"><span data-stu-id="0e87c-165">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="0e87c-166">稍後在本教學課程中將使用模型繫結器的資料，您將學習，您不需要修改預設路由。</span><span class="sxs-lookup"><span data-stu-id="0e87c-166">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="0e87c-167">在這些範例中已執行控制器&quot;VC&quot; MVC 部分 — 也就是檢視和控制器工作。</span><span class="sxs-lookup"><span data-stu-id="0e87c-167">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="0e87c-168">控制器會直接傳回 HTML。</span><span class="sxs-lookup"><span data-stu-id="0e87c-168">The controller is returning HTML directly.</span></span> <span data-ttu-id="0e87c-169">通常您不希望控制器直接傳回 HTML，因為，變得很麻煩的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0e87c-169">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="0e87c-170">而是我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="0e87c-170">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="0e87c-171">讓我們看如何能夠在下一步。</span><span class="sxs-lookup"><span data-stu-id="0e87c-171">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0e87c-172">[上一頁](getting-started.md)
> [下一頁](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="0e87c-172">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
