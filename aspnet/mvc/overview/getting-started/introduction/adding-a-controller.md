---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: "加入控制器 |Microsoft 文件"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: c8f317b2ac133f560461917af1588b7a1fa51c4f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-controller"></a><span data-ttu-id="0a4e1-102">新增控制器</span><span class="sxs-lookup"><span data-stu-id="0a4e1-102">Adding a Controller</span></span>
====================
<span data-ttu-id="0a4e1-103">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0a4e1-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="0a4e1-104">代表 MVC*模型-檢視-控制器*。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="0a4e1-105">MVC 是用於開發架構，可測試且容易維護的應用程式的模式。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="0a4e1-106">MVC 架構的應用程式包含：</span><span class="sxs-lookup"><span data-stu-id="0a4e1-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="0a4e1-107">**M** odels： 類別，表示應用程式的資料，以及可使用的驗證邏輯將強制執行商務規則，該資料。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="0a4e1-108">**V** iews： 您的應用程式用來動態產生 HTML 回應的範本檔案。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="0a4e1-109">**C** ontrollers： 處理內送的瀏覽器要求類別擷取模型資料，然後指定 將回應傳回至瀏覽器的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="0a4e1-110">我們將會涵蓋在本教學課程系列中的所有這些概念，並示範如何使用它們來建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

> [!NOTE]
> <span data-ttu-id="0a4e1-111">預設的 MVC 在先前步驟中選取範本。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-111">In the previous step the Default MVC template was selected.</span></span> <span data-ttu-id="0a4e1-112">這會建立主資料夾、 帳戶和預設的管理控制站。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-112">This creates Home, Account and Manage Controllers by default.</span></span>

<span data-ttu-id="0a4e1-113">讓我們先建立控制器類別。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-113">Let's begin by creating a controller class.</span></span> <span data-ttu-id="0a4e1-114">在**方案總管 中**，以滑鼠右鍵按一下*控制器*資料夾，然後按一下**新增**，然後**控制器**。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-114">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>


![](adding-a-controller/_static/image1.png)

<span data-ttu-id="0a4e1-115">在**加入 Scaffold**對話方塊中，按一下  **MVC 5 控制器空白**，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-115">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  
 

<span data-ttu-id="0a4e1-116">將新的控制站 」 HelloWorldController"，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-116">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![加入控制器](adding-a-controller/_static/image3.png)

<span data-ttu-id="0a4e1-118">請注意，在**方案總管 中**新檔案已經建立的具名*HelloWorldController.cs*和新的資料夾*Views\HelloWorld*。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-118">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="0a4e1-119">控制器是在 IDE 中開啟。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-119">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="0a4e1-120">取代下列程式碼檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-120">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="0a4e1-121">控制器方法會傳回 HTML 的字串做為範例。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-121">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="0a4e1-122">控制站命名為`HelloWorldController`第一種方法稱為`Index`。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-122">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="0a4e1-123">讓我們來叫用它從瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-123">Let's invoke it from a browser.</span></span> <span data-ttu-id="0a4e1-124">執行應用程式 （按 F5 或 Ctrl + F5）。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-124">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="0a4e1-125">在瀏覽器中附加&quot;HelloWorld&quot;網址列中的路徑。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-125">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="0a4e1-126">(例如，在圖例中，其下的`http://localhost:1234/HelloWorld.`) 網頁瀏覽器中的看起來像下列螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-126">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="0a4e1-127">上述的方法，在程式碼會直接傳回字串。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-127">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="0a4e1-128">告訴系統只傳回一些 HTML，且啟動成功 ！</span><span class="sxs-lookup"><span data-stu-id="0a4e1-128">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="0a4e1-129">根據傳入的 URL 不同控制器類別 （和不同的動作方法內），會叫用 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-129">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="0a4e1-130">使用 ASP.NET MVC 的預設的 URL 路由邏輯來判斷哪些程式碼叫用使用的格式如下：</span><span class="sxs-lookup"><span data-stu-id="0a4e1-130">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="0a4e1-131">您設定的格式中的路由*應用程式\_Start/RouteConfig.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-131">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="0a4e1-132">當您執行應用程式，並不提供任何 URL 區段時，其預設值為"Home"控制器和 「 索引 」 動作方法的區段中指定預設值的上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-132">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="0a4e1-133">URL 的第一個部分會決定要執行的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="0a4e1-134">因此*/HelloWorld*對應至`HelloWorldController`類別。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="0a4e1-135">URL 的第二個部分會決定要執行的類別上的動作方法。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="0a4e1-136">因此*/HelloWorld/索引*導致`Index`方法`HelloWorldController`類別來執行。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="0a4e1-137">請注意，我們只需要瀏覽至*/HelloWorld*和`Index`方法使用預設值。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="0a4e1-138">這是因為方法名為`Index`是將會在控制站呼叫，如果沒有明確指定的預設方法。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="0a4e1-139">URL 區段的第三個部分 (`Parameters`) 是路由資料。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-139">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="0a4e1-140">我們會在本教學課程稍後看到路由資料。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-140">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="0a4e1-141">瀏覽至 `http://localhost:xxxx/HelloWorld/Welcome`。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-141">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="0a4e1-142">`Welcome`方法會執行，且會傳回字串&quot;這是該  褖畫惎動作方法...&quot;.</span><span class="sxs-lookup"><span data-stu-id="0a4e1-142">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="0a4e1-143">預設的 MVC 對應是否`/[Controller]/[ActionName]/[Parameters]`。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-143">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="0a4e1-144">在此 URL 中，控制器是 `HelloWorld`，而 `Welcome` 是動作方法。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-144">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="0a4e1-145">您尚未使用 URL 的 `[Parameters]` 部分。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-145">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="0a4e1-146">讓我們在範例稍微修改，讓您可以將從 URL 的某些參數資訊傳遞至控制器 (例如， */HelloWorld/歡迎畫面？ 名稱 = Scott&amp;numtimes = 4*)。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-146">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="0a4e1-147">變更您`Welcome`方法，將包含兩個參數，如下所示。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-147">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="0a4e1-148">請注意，程式碼會使用 C# 選擇性參數功能表示`numTimes`參數應該預設為 1，如果該參數不傳遞任何值。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-148">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="0a4e1-149">安全性注意事項： 上方的程式碼會使用[HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx)為了防止惡意的輸入 (也就是 JavaScript) 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-149">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="0a4e1-150">如需詳細資訊，請參閱[How to： 保護對指令碼會利用 Web 應用程式中藉由套用 HTML 編碼字串](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-150">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>


 <span data-ttu-id="0a4e1-151">執行您的應用程式，並瀏覽至範例 URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`)。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-151">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="0a4e1-152">您可以嘗試不同的值`name`和`numtimes`在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-152">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="0a4e1-153">[ASP.NET MVC 模型繫結系統](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)會自動對應 網址列中的查詢字串中您的方法參數的具名的參數。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-153">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="0a4e1-154">在範例中，上述的 URL 區段 ( `Parameters`) 不使用`name`和`numTimes`做為參數傳遞[查詢字串](http://en.wikipedia.org/wiki/Query_string)。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-154">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="0a4e1-155">?</span><span class="sxs-lookup"><span data-stu-id="0a4e1-155">The ?</span></span> <span data-ttu-id="0a4e1-156">（問號） 上述 URL 中是分隔符號，並遵循查詢字串。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-156">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="0a4e1-157">&amp; 字元可分隔查詢字串。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-157">The &amp; character separates query strings.</span></span>

<span data-ttu-id="0a4e1-158">下列程式碼取代該  褖畫惎方法：</span><span class="sxs-lookup"><span data-stu-id="0a4e1-158">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="0a4e1-159">執行應用程式，並輸入下列 URL:`http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="0a4e1-159">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="0a4e1-160">此時第三個 URL 區段的比對路由參數`ID.``Welcome`動作的方法包含參數 (`ID`) 符合中的 URL 規格`RegisterRoutes`方法。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-160">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="0a4e1-161">在 ASP.NET MVC 應用程式更通常會傳入做為路由資料 （如同我們上述識別碼） 比查詢字串的形式傳遞這些參數。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-161">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="0a4e1-162">您也可以新增的路由將兩者`name`和`numtimes`參數做為 URL 中的路由資料中。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-162">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="0a4e1-163">在*應用程式\_Start\RouteConfig.cs*檔案中加入"Hello"路由：</span><span class="sxs-lookup"><span data-stu-id="0a4e1-163">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="0a4e1-164">執行應用程式，並瀏覽至`/localhost:XXX/HelloWorld/Welcome/Scott/3`。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-164">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="0a4e1-165">對於許多的 MVC 應用程式的預設路由會正常運作。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-165">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="0a4e1-166">您將學習稍後在本教學課程，將使用模型繫結器的資料，就不需要修改該預設路由。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-166">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="0a4e1-167">這些範例中已執行控制器&quot;VC&quot; MVC 部分 — 也就是，則檢視和控制器的工作。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-167">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="0a4e1-168">控制器直接傳回 HTML。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-168">The controller is returning HTML directly.</span></span> <span data-ttu-id="0a4e1-169">通常您不想直接傳回 HTML，因為變得很麻煩的程式碼的控制站。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-169">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="0a4e1-170">改為我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-170">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="0a4e1-171">讓我們來看我們如何執行這在下一步。</span><span class="sxs-lookup"><span data-stu-id="0a4e1-171">Let's look next at how we can do this.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0a4e1-172">[上一頁](getting-started.md)
[下一頁](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="0a4e1-172">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
