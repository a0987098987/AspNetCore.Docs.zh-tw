---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: 新增控制器 (C#) |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express 服務 Pack 1，哪些 i 的 ASP.NET MVC Web 應用程式的基本概念...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b31cf3bdf18c144d2735973119367b01de0353fe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830563"
---
<a name="adding-a-controller-c"></a><span data-ttu-id="0fe5b-103">新增控制器 (C#)</span><span class="sxs-lookup"><span data-stu-id="0fe5b-103">Adding a Controller (C#)</span></span>
====================
<span data-ttu-id="0fe5b-104">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0fe5b-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="0fe5b-105">本教學課程中的更新的版本可[此處](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="0fe5b-106">它更安全、 更容易遵循，並示範更多的功能。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="0fe5b-107">本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="0fe5b-108">在開始之前，請確定您已安裝符合下列先決條件。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="0fe5b-109">您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="0fe5b-110">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="0fe5b-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="0fe5b-111">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="0fe5b-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="0fe5b-112">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="0fe5b-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="0fe5b-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="0fe5b-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="0fe5b-114">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="0fe5b-115">使用本主題隨附了 C# 原始程式碼的 Visual Web Developer 專案。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="0fe5b-116">[下載 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="0fe5b-117">如果您偏好 Visual Basic，切換到[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="0fe5b-118">代表 MVC*模型-檢視-控制器*。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-118">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="0fe5b-119">MVC 是開發應用程式的架構以及易於維護的模式。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-119">MVC is a pattern for developing applications that are well architected and easy to maintain.</span></span> <span data-ttu-id="0fe5b-120">MVC 架構的應用程式包含：</span><span class="sxs-lookup"><span data-stu-id="0fe5b-120">MVC-based applications contain:</span></span>

- <span data-ttu-id="0fe5b-121">控制站： 類別，可處理傳入的要求，應用程式、 擷取模型資料，然後指定 將回應傳回給用戶端的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-121">Controllers: Classes that handle incoming requests to the application, retrieve model data, and then specify view templates that return a response to the client.</span></span>
- <span data-ttu-id="0fe5b-122">代表資料的應用程式，並強制執行商務規則，該資料使用的驗證邏輯的模型： 類別。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-122">Models: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="0fe5b-123">您的應用程式用來動態產生 HTML 回應的檢視： 範本檔案。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-123">Views: Template files that your application uses to dynamically generate HTML responses.</span></span>

<span data-ttu-id="0fe5b-124">我們就可以涵蓋在本教學課程系列中的所有這些概念，並示範如何使用它們來建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-124">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="0fe5b-125">讓我們開始建立控制器類別。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-125">Let's begin by creating a controller class.</span></span> <span data-ttu-id="0fe5b-126">在 **方案總管**，以滑鼠右鍵按一下*控制站*資料夾，然後選取**新增控制器**。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-126">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span>

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

<span data-ttu-id="0fe5b-127">將新的控制器"HelloWorldController 」 的名稱。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-127">Name your new controller "HelloWorldController".</span></span> <span data-ttu-id="0fe5b-128">保留預設範本，因為**空白控制器**然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-128">Leave the default template as **Empty controller** and click **Add**.</span></span>

<span data-ttu-id="0fe5b-129">[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="0fe5b-129">[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)</span></span>

<span data-ttu-id="0fe5b-130">請注意，在**方案總管**新的檔案已經建立的具名*HelloWorldController.cs*。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-130">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs*.</span></span> <span data-ttu-id="0fe5b-131">在 IDE 中開啟檔案。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-131">The file is open in the IDE.</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="0fe5b-132">內`public class HelloWorldController`區塊中，建立兩個方法，看起來像下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-132">Inside the `public class HelloWorldController` block, create two methods that look like the following code.</span></span> <span data-ttu-id="0fe5b-133">控制器會傳回 HTML 的字串做為範例。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-133">The controller will return a string of HTML as an example.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="0fe5b-134">名為您的控制器`HelloWorldController`和上述的第一個方法命名為`Index`。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-134">Your controller is named `HelloWorldController` and the first method above is named `Index`.</span></span> <span data-ttu-id="0fe5b-135">讓我們叫用它從瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-135">Let's invoke it from a browser.</span></span> <span data-ttu-id="0fe5b-136">執行應用程式 （按 F5 或 ctrl+f5）。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-136">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="0fe5b-137">在瀏覽器中，附加"HelloWorld"的網址列中的路徑。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-137">In the browser, append "HelloWorld" to the path in the address bar.</span></span> <span data-ttu-id="0fe5b-138">(例如，在圖中，其下的`http://localhost:43246/HelloWorld.`) 的瀏覽器頁面看起來像下列螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-138">(For example, in the illustration below, it's `http://localhost:43246/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="0fe5b-139">在上述的方法中，程式碼會直接傳回字串。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-139">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="0fe5b-140">您告訴系統只會傳回一些 HTML，且啟動成功 ！</span><span class="sxs-lookup"><span data-stu-id="0fe5b-140">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="0fe5b-141">根據傳入 URL 不同控制器類別 （和不同的動作方法），會叫用 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-141">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="0fe5b-142">使用 ASP.NET MVC 的預設對應邏輯會使用像這樣的格式，以判斷哪些程式碼叫用：</span><span class="sxs-lookup"><span data-stu-id="0fe5b-142">The default mapping logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="0fe5b-143">URL 的第一個部分會判斷要執行的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-143">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="0fe5b-144">因此 */HelloWorld*對應至`HelloWorldController`類別。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-144">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="0fe5b-145">URL 的第二部分會判斷要執行的類別上的動作方法。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-145">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="0fe5b-146">因此 */HelloWorld/Index*會導致`Index`方法`HelloWorldController`類別來執行。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-146">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="0fe5b-147">請注意，我們只需要瀏覽至 */HelloWorld*而`Index`方法已由預設值。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-147">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="0fe5b-148">這是因為方法命名為`Index`是會在控制器呼叫，如果沒有明確指定的預設方法。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-148">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="0fe5b-149">瀏覽至 `http://localhost:xxxx/HelloWorld/Welcome`。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-149">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="0fe5b-150">`Welcome` 方法隨即執行，並傳回字串 "This is the Welcome action method..."。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-150">The `Welcome` method runs and returns the string "This is the Welcome action method...".</span></span> <span data-ttu-id="0fe5b-151">預設的 MVC 對應是`/[Controller]/[ActionName]/[Parameters]`。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-151">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="0fe5b-152">在此 URL 中，控制器是 `HelloWorld`，而 `Welcome` 是動作方法。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-152">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="0fe5b-153">您尚未使用 URL 的 `[Parameters]` 部分。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-153">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="0fe5b-154">讓我們在範例稍微修改，讓您可以將從 URL 的某些參數資訊傳遞到控制器 (例如 */HelloWorld/歡迎？ 名稱 = Scott&amp;numtimes = 4*)。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-154">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="0fe5b-155">變更您`Welcome`方法以包含兩個參數，如下所示。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-155">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="0fe5b-156">請注意，程式碼會使用 C# 選擇性參數功能，在表示`numTimes`參數應該預設為 1，如果沒有傳遞任何值，該參數。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-156">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

<span data-ttu-id="0fe5b-157">執行應用程式，並瀏覽至範例 URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-157">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span></span> <span data-ttu-id="0fe5b-158">您可以嘗試不同的值，如`name`和`numtimes`在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-158">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="0fe5b-159">系統會自動對應方法中的參數從查詢字串，在網址列中的具名的參數。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-159">The system automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="0fe5b-160">在這些範例中兩個控制器已執行 MVC 的"VC"部分，也就是檢視和控制器工作。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-160">In both these examples the controller has been doing the "VC" portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="0fe5b-161">控制器會直接傳回 HTML。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-161">The controller is returning HTML directly.</span></span> <span data-ttu-id="0fe5b-162">通常您不希望控制器直接傳回 HTML，因為，變得很麻煩的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-162">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="0fe5b-163">而是我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-163">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="0fe5b-164">讓我們看如何能夠在下一步。</span><span class="sxs-lookup"><span data-stu-id="0fe5b-164">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0fe5b-165">[上一頁](intro-to-aspnet-mvc-3.md)
> [下一頁](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="0fe5b-165">[Previous](intro-to-aspnet-mvc-3.md)
[Next](adding-a-view.md)</span></span>
