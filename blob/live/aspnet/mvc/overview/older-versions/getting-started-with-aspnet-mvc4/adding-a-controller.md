---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: "加入控制器 |Microsoft 文件"
author: Rick-Anderson
description: "注意： 本教學課程中的更新的版本這裡會提供使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 容易遵循，以及示範..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 69af91401e51470fbc0b67103345325201b06723
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a><span data-ttu-id="939e3-104">新增控制器</span><span class="sxs-lookup"><span data-stu-id="939e3-104">Adding a Controller</span></span>
====================
<span data-ttu-id="939e3-105">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="939e3-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="939e3-106">本教學課程的更新的版本時使用[這裡](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="939e3-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="939e3-107">它更安全、 容易遵循，及示範更多的功能。</span><span class="sxs-lookup"><span data-stu-id="939e3-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="939e3-108">代表 MVC*模型-檢視-控制器*。</span><span class="sxs-lookup"><span data-stu-id="939e3-108">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="939e3-109">MVC 是用於開發架構，可測試且容易維護的應用程式的模式。</span><span class="sxs-lookup"><span data-stu-id="939e3-109">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="939e3-110">MVC 架構的應用程式包含：</span><span class="sxs-lookup"><span data-stu-id="939e3-110">MVC-based applications contain:</span></span>

- <span data-ttu-id="939e3-111">**M** odels： 類別，表示應用程式的資料，以及可使用的驗證邏輯將強制執行商務規則，該資料。</span><span class="sxs-lookup"><span data-stu-id="939e3-111">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="939e3-112">**V** iews： 您的應用程式用來動態產生 HTML 回應的範本檔案。</span><span class="sxs-lookup"><span data-stu-id="939e3-112">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="939e3-113">**C** ontrollers： 處理內送的瀏覽器要求類別擷取模型資料，然後指定 將回應傳回至瀏覽器的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="939e3-113">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="939e3-114">我們將會涵蓋在本教學課程系列中的所有這些概念，並示範如何使用它們來建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="939e3-114">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="939e3-115">讓我們先建立控制器類別。</span><span class="sxs-lookup"><span data-stu-id="939e3-115">Let's begin by creating a controller class.</span></span> <span data-ttu-id="939e3-116">在**方案總管 中**，以滑鼠右鍵按一下*控制器*資料夾，然後選取**加入控制器**。</span><span class="sxs-lookup"><span data-stu-id="939e3-116">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span>

![](adding-a-controller/_static/image1.png)

<span data-ttu-id="939e3-117">命名新的控制站&quot;HelloWorldController&quot;。</span><span class="sxs-lookup"><span data-stu-id="939e3-117">Name your new controller &quot;HelloWorldController&quot;.</span></span> <span data-ttu-id="939e3-118">保留為預設範本**空白的 MVC 控制器**按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="939e3-118">Leave the default template as **Empty MVC controller** and click **Add**.</span></span>

![加入控制器](adding-a-controller/_static/image2.png)

<span data-ttu-id="939e3-120">請注意，在**方案總管 中**新檔案已經建立的具名*HelloWorldController.cs*。</span><span class="sxs-lookup"><span data-stu-id="939e3-120">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs*.</span></span> <span data-ttu-id="939e3-121">已在 IDE 中開啟的檔案。</span><span class="sxs-lookup"><span data-stu-id="939e3-121">The file is open in the IDE.</span></span>

![](adding-a-controller/_static/image3.png)

<span data-ttu-id="939e3-122">取代下列程式碼檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="939e3-122">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="939e3-123">控制器方法會傳回 HTML 的字串做為範例。</span><span class="sxs-lookup"><span data-stu-id="939e3-123">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="939e3-124">控制站命名為`HelloWorldController`上述第一種方法稱為`Index`。</span><span class="sxs-lookup"><span data-stu-id="939e3-124">The controller is named `HelloWorldController` and the first method above is named `Index`.</span></span> <span data-ttu-id="939e3-125">讓我們來叫用它從瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="939e3-125">Let's invoke it from a browser.</span></span> <span data-ttu-id="939e3-126">執行應用程式 （按 F5 或 Ctrl + F5）。</span><span class="sxs-lookup"><span data-stu-id="939e3-126">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="939e3-127">在瀏覽器中附加&quot;HelloWorld&quot;網址列中的路徑。</span><span class="sxs-lookup"><span data-stu-id="939e3-127">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="939e3-128">(例如，在圖例中，其下的`http://localhost:1234/HelloWorld.`) 網頁瀏覽器中的看起來像下列螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="939e3-128">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="939e3-129">上述的方法，在程式碼會直接傳回字串。</span><span class="sxs-lookup"><span data-stu-id="939e3-129">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="939e3-130">告訴系統只傳回一些 HTML，且啟動成功 ！</span><span class="sxs-lookup"><span data-stu-id="939e3-130">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="939e3-131">根據傳入的 URL 不同控制器類別 （和不同的動作方法內），會叫用 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="939e3-131">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="939e3-132">使用 ASP.NET MVC 的預設的 URL 路由邏輯來判斷哪些程式碼叫用使用的格式如下：</span><span class="sxs-lookup"><span data-stu-id="939e3-132">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="939e3-133">URL 的第一個部分會決定要執行的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="939e3-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="939e3-134">因此*/HelloWorld*對應至`HelloWorldController`類別。</span><span class="sxs-lookup"><span data-stu-id="939e3-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="939e3-135">URL 的第二個部分會決定要執行的類別上的動作方法。</span><span class="sxs-lookup"><span data-stu-id="939e3-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="939e3-136">因此*/HelloWorld/索引*導致`Index`方法`HelloWorldController`類別來執行。</span><span class="sxs-lookup"><span data-stu-id="939e3-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="939e3-137">請注意，我們只需要瀏覽至*/HelloWorld*和`Index`方法使用預設值。</span><span class="sxs-lookup"><span data-stu-id="939e3-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="939e3-138">這是因為方法名為`Index`是將會在控制站呼叫，如果沒有明確指定的預設方法。</span><span class="sxs-lookup"><span data-stu-id="939e3-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="939e3-139">瀏覽至 `http://localhost:xxxx/HelloWorld/Welcome`。</span><span class="sxs-lookup"><span data-stu-id="939e3-139">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="939e3-140">`Welcome`方法會執行，且會傳回字串&quot;這是該褖畫惎動作方法...&quot;.</span><span class="sxs-lookup"><span data-stu-id="939e3-140">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="939e3-141">預設的 MVC 對應是否`/[Controller]/[ActionName]/[Parameters]`。</span><span class="sxs-lookup"><span data-stu-id="939e3-141">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="939e3-142">在此 URL 中，控制器是 `HelloWorld`，而 `Welcome` 是動作方法。</span><span class="sxs-lookup"><span data-stu-id="939e3-142">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="939e3-143">您尚未使用 URL 的 `[Parameters]` 部分。</span><span class="sxs-lookup"><span data-stu-id="939e3-143">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="939e3-144">讓我們在範例稍微修改，讓您可以將從 URL 的某些參數資訊傳遞至控制器 (例如， */HelloWorld/歡迎畫面？ 名稱 = Scott&amp;numtimes = 4*)。</span><span class="sxs-lookup"><span data-stu-id="939e3-144">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="939e3-145">變更您`Welcome`方法，將包含兩個參數，如下所示。</span><span class="sxs-lookup"><span data-stu-id="939e3-145">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="939e3-146">請注意，程式碼會使用 C# 選擇性參數功能表示`numTimes`參數應該預設為 1，如果該參數不傳遞任何值。</span><span class="sxs-lookup"><span data-stu-id="939e3-146">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

<span data-ttu-id="939e3-147">執行您的應用程式，並瀏覽至範例 URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`。</span><span class="sxs-lookup"><span data-stu-id="939e3-147">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span></span> <span data-ttu-id="939e3-148">您可以嘗試不同的值`name`和`numtimes`在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="939e3-148">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="939e3-149">[ASP.NET MVC 模型繫結系統](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)會自動對應 網址列中的查詢字串中您的方法參數的具名的參數。</span><span class="sxs-lookup"><span data-stu-id="939e3-149">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="939e3-150">在這些範例中這兩個已執行控制器&quot;VC&quot; MVC 部分 — 也就是，則檢視和控制器的工作。</span><span class="sxs-lookup"><span data-stu-id="939e3-150">In both these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="939e3-151">控制器直接傳回 HTML。</span><span class="sxs-lookup"><span data-stu-id="939e3-151">The controller is returning HTML directly.</span></span> <span data-ttu-id="939e3-152">通常您不想直接傳回 HTML，因為變得很麻煩的程式碼的控制站。</span><span class="sxs-lookup"><span data-stu-id="939e3-152">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="939e3-153">改為我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="939e3-153">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="939e3-154">讓我們來看我們如何執行這在下一步。</span><span class="sxs-lookup"><span data-stu-id="939e3-154">Let's look next at how we can do this.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="939e3-155">[上一頁](intro-to-aspnet-mvc-4.md)
[下一頁](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="939e3-155">[Previous](intro-to-aspnet-mvc-4.md)
[Next](adding-a-view.md)</span></span>
