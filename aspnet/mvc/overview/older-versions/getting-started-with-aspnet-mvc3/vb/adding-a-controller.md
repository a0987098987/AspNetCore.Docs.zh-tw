---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: "加入控制器 (VB) |Microsoft 文件"
author: Rick-Anderson
description: "本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 74113d76a74b1da27a7f9a33a13038a0c36ad036
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller-vb"></a><span data-ttu-id="79cd0-103">加入控制器 (VB)</span><span class="sxs-lookup"><span data-stu-id="79cd0-103">Adding a Controller (VB)</span></span>
====================
<span data-ttu-id="79cd0-104">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="79cd0-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="79cd0-105">本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="79cd0-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="79cd0-106">開始之前，請確定您已安裝下面所列的必要條件。</span><span class="sxs-lookup"><span data-stu-id="79cd0-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="79cd0-107">您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="79cd0-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="79cd0-108">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="79cd0-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="79cd0-109">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="79cd0-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="79cd0-110">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="79cd0-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="79cd0-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="79cd0-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="79cd0-112">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="79cd0-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="79cd0-113">使用本主題隨附在 Visual Web Developer 專案中的使用 VB.NET 原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="79cd0-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="79cd0-114">[下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="79cd0-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="79cd0-115">如果您偏好 C#，切換至[C# 版本](../cs/adding-a-controller.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="79cd0-115">If you prefer C#, switch to the [C# version](../cs/adding-a-controller.md) of this tutorial.</span></span>


<span data-ttu-id="79cd0-116">代表 MVC*模型-檢視-控制器*。</span><span class="sxs-lookup"><span data-stu-id="79cd0-116">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="79cd0-117">MVC 是一種模式來開發應用程式，使每個部分都有個別的責任：</span><span class="sxs-lookup"><span data-stu-id="79cd0-117">MVC is a pattern for developing applications such that each part has a separate responsibility:</span></span>

- <span data-ttu-id="79cd0-118">模型： 您的應用程式資料。</span><span class="sxs-lookup"><span data-stu-id="79cd0-118">Model: The data for your application.</span></span>
- <span data-ttu-id="79cd0-119">檢視： 您的應用程式將用來動態產生 HTML 回應範本檔案。</span><span class="sxs-lookup"><span data-stu-id="79cd0-119">Views: The template files your application will use to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="79cd0-120">控制站： 類別，處理內送的 URL 要求，應用程式，可擷取模型資料，然後指定 呈現給用戶端的回應的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="79cd0-120">Controllers: Classes that handle incoming URL requests to the application, retrieve model data, and then specify view templates that render a response to the client.</span></span>

<span data-ttu-id="79cd0-121">我們將會在本教學課程涵蓋所有這些概念，並示範如何使用它們來建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="79cd0-121">We'll be covering all these concepts in this tutorial and show you how to use them to build an application.</span></span>

<span data-ttu-id="79cd0-122">建立新的控制，以滑鼠右鍵按一下*控制器*資料夾中的**方案總管] 中**，然後選取 [**加入控制器**。</span><span class="sxs-lookup"><span data-stu-id="79cd0-122">Create a new controller by right-clicking the *Controllers* folder in **Solution Explorer** and then selecting **Add Controller**.</span></span>

<span data-ttu-id="79cd0-123">[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="79cd0-123">[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)</span></span>

<span data-ttu-id="79cd0-124">命名新的控制站&quot;HelloWorldController&quot;按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="79cd0-124">Name your new controller &quot;HelloWorldController&quot; and click **Add**.</span></span>

<span data-ttu-id="79cd0-125">[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="79cd0-125">[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)</span></span>

<span data-ttu-id="79cd0-126">請注意，在**方案總管 中**右邊確認已為您建立新的檔案命名為*HelloWorldController.cs* ，檔案已在 IDE 中開啟。</span><span class="sxs-lookup"><span data-stu-id="79cd0-126">Notice in **Solution Explorer** on the right that a new file has been created for you named *HelloWorldController.cs* and that the file is open in the IDE.</span></span>

<span data-ttu-id="79cd0-127">在新`public class HelloWorldController`封鎖，請建立兩個新方法看起來像下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="79cd0-127">Inside the new `public class HelloWorldController` block, create two new methods that look like the following code.</span></span> <span data-ttu-id="79cd0-128">我們會直接從控制器中傳回 HTML 的字串，做為範例。</span><span class="sxs-lookup"><span data-stu-id="79cd0-128">We'll return a string of HTML directly from the controller as an example.</span></span>

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

<span data-ttu-id="79cd0-129">將控制器命名為`HelloWorldController`，為您的新方法`Index`。</span><span class="sxs-lookup"><span data-stu-id="79cd0-129">Your controller is named `HelloWorldController` and your new method is named `Index`.</span></span> <span data-ttu-id="79cd0-130">執行應用程式 （按 F5 或 Ctrl + F5）。</span><span class="sxs-lookup"><span data-stu-id="79cd0-130">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="79cd0-131">一旦您的瀏覽器開始，附加&quot;HelloWorld&quot;網址列中的路徑。</span><span class="sxs-lookup"><span data-stu-id="79cd0-131">Once your browser has started up, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="79cd0-132">(我的電腦上有`http://localhost:43246/HelloWorld`) 您的瀏覽器看起來像下列螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="79cd0-132">(On my computer, it's `http://localhost:43246/HelloWorld`) Your browser will look like the screenshot below.</span></span> <span data-ttu-id="79cd0-133">上述的方法，在程式碼會直接傳回字串。</span><span class="sxs-lookup"><span data-stu-id="79cd0-133">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="79cd0-134">我們會告訴系統只傳回一些 HTML，且啟動成功 ！</span><span class="sxs-lookup"><span data-stu-id="79cd0-134">We told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="79cd0-135">根據傳入的 URL 不同控制器類別 （和不同的動作方法內），會叫用 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="79cd0-135">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="79cd0-136">使用 ASP.NET MVC 的預設對應邏輯來控制哪些程式碼會叫用使用的格式如下：</span><span class="sxs-lookup"><span data-stu-id="79cd0-136">The default mapping logic used by ASP.NET MVC uses a format like this to control what code is invoked:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="79cd0-137">URL 的第一個部分會決定要執行的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="79cd0-137">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="79cd0-138">因此*/HelloWorld*對應至`HelloWorldController`類別。</span><span class="sxs-lookup"><span data-stu-id="79cd0-138">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="79cd0-139">URL 的第二個部分會決定要執行的類別上的動作方法。</span><span class="sxs-lookup"><span data-stu-id="79cd0-139">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="79cd0-140">因此*/HelloWorld/索引*導致`Index`方法`HelloWorldController`類別來執行。</span><span class="sxs-lookup"><span data-stu-id="79cd0-140">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="79cd0-141">請注意，我們只需要瀏覽*/HelloWorld*上方和`Index`方法使用預設值。</span><span class="sxs-lookup"><span data-stu-id="79cd0-141">Notice that we only had to visit */HelloWorld* above and the `Index` method was used by default.</span></span> <span data-ttu-id="79cd0-142">這是因為方法名為`Index`是將會在控制站呼叫，如果沒有明確指定的預設方法。</span><span class="sxs-lookup"><span data-stu-id="79cd0-142">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="79cd0-143">瀏覽至 `http://localhost:xxxx/HelloWorld/Welcome`。</span><span class="sxs-lookup"><span data-stu-id="79cd0-143">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="79cd0-144">`Welcome`方法會執行，且會傳回字串&quot;這是該褖畫惎動作方法...&quot;.</span><span class="sxs-lookup"><span data-stu-id="79cd0-144">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="79cd0-145">預設的 MVC 對應是否`/[Controller]/[ActionName]/[Parameters]`。</span><span class="sxs-lookup"><span data-stu-id="79cd0-145">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="79cd0-146">此 url 中，控制器是`HelloWorld`和`Welcome`是方法。</span><span class="sxs-lookup"><span data-stu-id="79cd0-146">For this URL, the controller is `HelloWorld` and `Welcome` is the method.</span></span> <span data-ttu-id="79cd0-147">我們尚未使用`[Parameters]`尚未 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="79cd0-147">We haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="79cd0-148">讓我們在範例稍微修改，讓我們可以從 URL 傳遞中的某些參數資訊，控制站 (例如， */HelloWorld/歡迎畫面？ 名稱 = Scott&amp;numtimes = 4*)。</span><span class="sxs-lookup"><span data-stu-id="79cd0-148">Let's modify the example slightly so that we can pass some parameter information in from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="79cd0-149">變更您`Welcome`方法，將包含兩個參數，如下所示。</span><span class="sxs-lookup"><span data-stu-id="79cd0-149">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="79cd0-150">請注意，我們已經使用 VB 選擇性參數功能表示`numTimes`參數應該預設為 1，如果該參數不傳遞任何值。</span><span class="sxs-lookup"><span data-stu-id="79cd0-150">Note that we've used the VB optional parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

<span data-ttu-id="79cd0-151">執行您的應用程式，並瀏覽至`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **。**</span><span class="sxs-lookup"><span data-stu-id="79cd0-151">Run your application and browse to `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`**.**</span></span> <span data-ttu-id="79cd0-152">您可以嘗試不同的值`name`和`numtimes`。</span><span class="sxs-lookup"><span data-stu-id="79cd0-152">You can try different values for `name` and `numtimes`.</span></span> <span data-ttu-id="79cd0-153">系統會自動對應具名的參數從您在網址列中的查詢字串參數，在您的方法。</span><span class="sxs-lookup"><span data-stu-id="79cd0-153">The system automatically maps the named parameters from your query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="79cd0-154">在這些範例中這兩個控制器已執行 MVC 的 VC 部分 — 這是檢視和控制器的工作。</span><span class="sxs-lookup"><span data-stu-id="79cd0-154">In both these examples the controller has been doing the VC portion of MVC — that is the view and controller work.</span></span> <span data-ttu-id="79cd0-155">控制器直接傳回 HTML。</span><span class="sxs-lookup"><span data-stu-id="79cd0-155">The controller is returning HTML directly.</span></span> <span data-ttu-id="79cd0-156">通常，我們不想直接傳回 HTML，因為變得很麻煩的程式碼的控制站。</span><span class="sxs-lookup"><span data-stu-id="79cd0-156">Ordinarily we don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="79cd0-157">改為我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="79cd0-157">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="79cd0-158">讓我們看看如何我們可以執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="79cd0-158">Let's look at how we can do this.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="79cd0-159">[上一頁](intro-to-aspnet-mvc-3.md)
[下一頁](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="79cd0-159">[Previous](intro-to-aspnet-mvc-3.md)
[Next](adding-a-view.md)</span></span>
