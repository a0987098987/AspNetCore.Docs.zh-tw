---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: 新增檢視 (VB) |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: cbb1b9572c1b1eb671eb2756b5920dc823963e8c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826810"
---
<a name="adding-a-view-vb"></a><span data-ttu-id="d0d26-103">新增檢視 (VB)</span><span class="sxs-lookup"><span data-stu-id="d0d26-103">Adding a View (VB)</span></span>
====================
<span data-ttu-id="d0d26-104">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d0d26-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="d0d26-105">本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="d0d26-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="d0d26-106">在開始之前，請確定您已安裝符合下列先決條件。</span><span class="sxs-lookup"><span data-stu-id="d0d26-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="d0d26-107">您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="d0d26-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="d0d26-108">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="d0d26-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="d0d26-109">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="d0d26-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="d0d26-110">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="d0d26-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="d0d26-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="d0d26-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="d0d26-112">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="d0d26-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="d0d26-113">使用本主題隨附了 VB.NET 原始程式碼的 Visual Web Developer 專案。</span><span class="sxs-lookup"><span data-stu-id="d0d26-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="d0d26-114">[下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="d0d26-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="d0d26-115">如果您偏好 C#，切換至[C# 版本](../cs/adding-a-view.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="d0d26-115">If you prefer C#, switch to the [C# version](../cs/adding-a-view.md) of this tutorial.</span></span>


<span data-ttu-id="d0d26-116">這一節我們將修改`HelloWorldController`類別，以使用檢視範本檔案，完全封裝產生 HTML 回應給用戶端的程序。</span><span class="sxs-lookup"><span data-stu-id="d0d26-116">In this section we're going to modify the `HelloWorldController` class to use a view template file to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="d0d26-117">讓我們開始使用檢視範本，內含`Index`方法中的`HelloWorldController`類別。</span><span class="sxs-lookup"><span data-stu-id="d0d26-117">Let's start by using a view template with the `Index` method in the `HelloWorldController` class.</span></span> <span data-ttu-id="d0d26-118">目前`Index`方法會傳回硬式編碼在控制器類別內的訊息字串。</span><span class="sxs-lookup"><span data-stu-id="d0d26-118">Currently the `Index` method returns a string with a message that is hard-coded within the controller class.</span></span> <span data-ttu-id="d0d26-119">變更`Index`方法來傳回`View`物件，如下列所示：</span><span class="sxs-lookup"><span data-stu-id="d0d26-119">Change the `Index` method to return a `View` object, as shown in the following:</span></span>

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

<span data-ttu-id="d0d26-120">讓我們現在將檢視範本新增至我們的專案，我們可以叫用`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="d0d26-120">Let's now add a view template to our project that we can invoke with the `Index` method.</span></span> <span data-ttu-id="d0d26-121">若要這樣做，以滑鼠右鍵按一下`Index`方法，然後按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="d0d26-121">To do this, right-click inside the `Index` method and click **Add View**.</span></span>

<span data-ttu-id="d0d26-122">[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d0d26-122">[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)</span></span>

<span data-ttu-id="d0d26-123">**加入檢視** 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="d0d26-123">The **Add View** dialog box appears.</span></span> <span data-ttu-id="d0d26-124">保留預設的項目，然後按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d0d26-124">Leave the default entries and click the **Add** button.</span></span>

<span data-ttu-id="d0d26-125">[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d0d26-125">[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)</span></span>

<span data-ttu-id="d0d26-126">*MvcMovie\Views\HelloWorld*資料夾並*MvcMovie\Views\HelloWorld\Index.vbhtml*建立檔案。</span><span class="sxs-lookup"><span data-stu-id="d0d26-126">The *MvcMovie\Views\HelloWorld* folder and the *MvcMovie\Views\HelloWorld\Index.vbhtml* file are created.</span></span> <span data-ttu-id="d0d26-127">您可以看到它們**方案總管 中**:</span><span class="sxs-lookup"><span data-stu-id="d0d26-127">You can see them in **Solution Explorer**:</span></span>

<span data-ttu-id="d0d26-128">[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d0d26-128">[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)</span></span>

<span data-ttu-id="d0d26-129">將下的一些 HTML`<h2>`標記。</span><span class="sxs-lookup"><span data-stu-id="d0d26-129">Add some HTML under the `<h2>` tag.</span></span> <span data-ttu-id="d0d26-130">已修改*MvcMovie\Views\HelloWorld\Index.vbhtml*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="d0d26-130">The modified *MvcMovie\Views\HelloWorld\Index.vbhtml* file is shown below.</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

<span data-ttu-id="d0d26-131">執行應用程式，並瀏覽至&quot;hello world&quot;控制站 (`http://localhost:xxxx/HelloWorld`)。</span><span class="sxs-lookup"><span data-stu-id="d0d26-131">Run the application and browse to the &quot;hello world&quot; controller (`http://localhost:xxxx/HelloWorld`).</span></span> <span data-ttu-id="d0d26-132">`Index`在控制器方法並沒有進行太多工作; 它只會執行陳述式`return View()`，這表示我們想要使用檢視範本檔案來呈現給用戶端的回應。</span><span class="sxs-lookup"><span data-stu-id="d0d26-132">The `Index` method in your controller didn't do much work; it simply ran the statement `return View()`, which indicated that we wanted to use a view template file to render a response to the client.</span></span> <span data-ttu-id="d0d26-133">因為我們未明確指定要使用的檢視範本檔案的名稱，預設使用 ASP.NET MVC *Index.vbhtml*檢視檔案內*\Views\HelloWorld*資料夾。</span><span class="sxs-lookup"><span data-stu-id="d0d26-133">Because we did not explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the *Index.vbhtml* view file within the *\Views\HelloWorld* folder.</span></span> <span data-ttu-id="d0d26-134">下圖顯示字串硬式編碼在檢視中。</span><span class="sxs-lookup"><span data-stu-id="d0d26-134">The image below shows the string hard-coded in the view.</span></span>

<span data-ttu-id="d0d26-135">[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="d0d26-135">[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)</span></span>

<span data-ttu-id="d0d26-136">看起來很好。</span><span class="sxs-lookup"><span data-stu-id="d0d26-136">Looks pretty good.</span></span> <span data-ttu-id="d0d26-137">不過，請留意到瀏覽器的標題列顯示&quot;Index&quot; ，並大的標題在頁面上顯示&quot;我的 MVC 應用程式。&quot;讓我們將這些變更。</span><span class="sxs-lookup"><span data-stu-id="d0d26-137">However, notice that the browser's title bar says &quot;Index&quot; and the big title on the page says &quot;My MVC Application.&quot; Let's change those.</span></span>

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="d0d26-138">變更檢視和版面配置頁</span><span class="sxs-lookup"><span data-stu-id="d0d26-138">Changing views and layout pages</span></span>

<span data-ttu-id="d0d26-139">首先，讓我們變更文字&quot;我的 MVC 應用程式。&quot;該文字會共用，並出現在每一頁。</span><span class="sxs-lookup"><span data-stu-id="d0d26-139">First, let's change the text &quot;My MVC Application.&quot; That text is shared and appears on every page.</span></span> <span data-ttu-id="d0d26-140">它實際上不會顯示只有一個位置，在專案中，即使是在我們的應用程式中的每個頁面上。</span><span class="sxs-lookup"><span data-stu-id="d0d26-140">It actually appears in only one place in our project, even though it's on every page in our application.</span></span> <span data-ttu-id="d0d26-141">移至 */檢視/Shared*資料夾中的**方案總管**，然後開啟 *\_Layout.vbhtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="d0d26-141">Go to the */Views/Shared* folder in **Solution Explorer** and open the *\_Layout.vbhtml* file.</span></span> <span data-ttu-id="d0d26-142">這個檔案稱為版面配置頁面，而且共用&quot;shell&quot;所有其他頁面使用。</span><span class="sxs-lookup"><span data-stu-id="d0d26-142">This file is called a layout page and it's the shared &quot;shell&quot; that all other pages use.</span></span>

<span data-ttu-id="d0d26-143">請注意`@RenderBody()`檔案底部附近的程式碼行。</span><span class="sxs-lookup"><span data-stu-id="d0d26-143">Note the `@RenderBody()` line of code near the bottom of the file.</span></span> <span data-ttu-id="d0d26-144">`RenderBody` 是，您所建立的所有頁面的都顯示位置的預留位置&quot;包裝&quot;版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="d0d26-144">`RenderBody` is a placeholder where all the pages you create show up, &quot;wrapped&quot; in the layout page.</span></span> <span data-ttu-id="d0d26-145">變更`<h1>`標題從**&quot;** 我的 MVC 應用程式&quot;來&quot;MVC 電影應用程式&quot;。</span><span class="sxs-lookup"><span data-stu-id="d0d26-145">Change the `<h1>` heading from **&quot;** My MVC Application&quot; to &quot;MVC Movie App&quot;.</span></span>

[!code-html[Main](adding-a-view/samples/sample3.html)]

<span data-ttu-id="d0d26-146">執行應用程式，並記下它現在說&quot;MVC 電影應用程式&quot;。</span><span class="sxs-lookup"><span data-stu-id="d0d26-146">Run the application and note it now says &quot;MVC Movie App&quot;.</span></span> <span data-ttu-id="d0d26-147">按一下 **關於**連結，且頁面上顯示&quot;MVC 電影應用程式&quot;也。</span><span class="sxs-lookup"><span data-stu-id="d0d26-147">Click the **About** link, and that page shows &quot;MVC Movie App&quot;, too.</span></span>

<span data-ttu-id="d0d26-148">完整 *\_Layout.vbhtml*檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="d0d26-148">The complete *\_Layout.vbhtml* file is shown below:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

<span data-ttu-id="d0d26-149">現在，讓我們變更 （檢視） 中的 [索引] 頁面的標題。</span><span class="sxs-lookup"><span data-stu-id="d0d26-149">Now, let's change the title of the Index page (view).</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

<span data-ttu-id="d0d26-150">開啟*MvcMovie\Views\HelloWorld\Index.vbhtml*。</span><span class="sxs-lookup"><span data-stu-id="d0d26-150">Open *MvcMovie\Views\HelloWorld\Index.vbhtml*.</span></span> <span data-ttu-id="d0d26-151">有兩個地方進行變更： 首先，文字，會出現在瀏覽器的標題，然後次要標頭 (`<h2>`項目)。</span><span class="sxs-lookup"><span data-stu-id="d0d26-151">There are two places to make a change: first, the text that appears in the title of the browser, and then in the secondary header (the `<h2>` element).</span></span> <span data-ttu-id="d0d26-152">我們要讓它們稍有不同的位元的程式碼變更應用程式的一部分，您可以看到。</span><span class="sxs-lookup"><span data-stu-id="d0d26-152">We'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>

<span data-ttu-id="d0d26-153">執行應用程式，並瀏覽至`http://localhost:xx/HelloWorld`。</span><span class="sxs-lookup"><span data-stu-id="d0d26-153">Run the application and browse to`http://localhost:xx/HelloWorld`.</span></span> <span data-ttu-id="d0d26-154">請注意，瀏覽器標題、主要標題和次要標題已變更</span><span class="sxs-lookup"><span data-stu-id="d0d26-154">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="d0d26-155">它很容易就能只進行小變更的應用程式中的重大變更對檢視表。</span><span class="sxs-lookup"><span data-stu-id="d0d26-155">It's easy to make big changes in your application with small changes to a view.</span></span> <span data-ttu-id="d0d26-156">(如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。</span><span class="sxs-lookup"><span data-stu-id="d0d26-156">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="d0d26-157">請在瀏覽器中按 Ctrl + F5 以強制載入來自伺服器的回應)。</span><span class="sxs-lookup"><span data-stu-id="d0d26-157">Press Ctrl+F5 in your browser to force the response from the server to be loaded.)</span></span>

<span data-ttu-id="d0d26-158">[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="d0d26-158">[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)</span></span>

<span data-ttu-id="d0d26-159">我們一點&quot;資料&quot;(在此情況下&quot;Hello World ！&quot;訊息) 是硬式編碼，不過。</span><span class="sxs-lookup"><span data-stu-id="d0d26-159">Our little bit of &quot;data&quot; (in this case the &quot;Hello World!&quot; message) is hard-coded, though.</span></span> <span data-ttu-id="d0d26-160">MVC 應用程式可能會有 V （檢視），我們有 C （控制站），但沒有 M （模型） 尚未。</span><span class="sxs-lookup"><span data-stu-id="d0d26-160">Our MVC application has V (views) and we've got C (controllers), but no M (model) yet.</span></span> <span data-ttu-id="d0d26-161">不久之後，我們將逐步解說如何建立資料庫，並從其擷取模型資料。</span><span class="sxs-lookup"><span data-stu-id="d0d26-161">Shortly, we'll walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="d0d26-162">將資料從控制器傳遞至檢視</span><span class="sxs-lookup"><span data-stu-id="d0d26-162">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="d0d26-163">我們移至資料庫，並討論模型之前，不過，讓我們先討論將資訊從控制器傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="d0d26-163">Before we go to a database and talk about models, though, let's first talk about passing information from the Controller to a View.</span></span> <span data-ttu-id="d0d26-164">我們想要將檢視範本呈現 HTML 回應給用戶端所需要的傳遞。</span><span class="sxs-lookup"><span data-stu-id="d0d26-164">We want to pass what a view template requires in order to render an HTML response to a client.</span></span> <span data-ttu-id="d0d26-165">這些物件是通常建立控制器類別傳遞至檢視範本，而且它們應該包含在檢視範本需要的資料，並不多。</span><span class="sxs-lookup"><span data-stu-id="d0d26-165">These objects are typically created and passed by a controller class to a view template, and they should contain only the data that the view template requires — and no more.</span></span>

<span data-ttu-id="d0d26-166">與先前`HelloWorldController`類別，`Welcome`動作方法所花費`name`和`numTimes`參數和參數值至瀏覽器然後輸出。</span><span class="sxs-lookup"><span data-stu-id="d0d26-166">Previously with the `HelloWorldController` class, the `Welcome` action method took a `name` and a `numTimes` parameter and then output the parameter values to the browser.</span></span> <span data-ttu-id="d0d26-167">而非讓控制器繼續直接呈現這個回應，讓我們改為我們會將放該資料封包中檢視。</span><span class="sxs-lookup"><span data-stu-id="d0d26-167">Rather than have the controller continue to render this response directly, let's instead we'll put that data in a bag for the View.</span></span> <span data-ttu-id="d0d26-168">控制器和檢視，可以使用`ViewBag`物件來保存該資料。</span><span class="sxs-lookup"><span data-stu-id="d0d26-168">Controllers and Views can use a `ViewBag` object to hold that data.</span></span> <span data-ttu-id="d0d26-169">會自動傳遞到檢視範本，以及用來呈現 HTML 回應使用封包的內容做為資料。</span><span class="sxs-lookup"><span data-stu-id="d0d26-169">That will be passed over to a view template automatically, and used to render the HTML response using the contents of the bag as data.</span></span> <span data-ttu-id="d0d26-170">這樣一來，控制器會關心一件事並與另一個檢視範本，讓我們可以維護乾淨&quot;關注點分離&quot;應用程式中。</span><span class="sxs-lookup"><span data-stu-id="d0d26-170">That way the controller is concerned with one thing and the view template with another — enabling us to maintain clean &quot;separation of concerns&quot; within the application.</span></span>

<span data-ttu-id="d0d26-171">或者，我們可以定義的自訂類別，然後建立該物件的執行個體自己、 填入資料並將它傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="d0d26-171">Alternatively, we could define a custom class, then create an instance of that object on our own, fill it with data and pass it to the View.</span></span> <span data-ttu-id="d0d26-172">通常稱為 ViewModel，因為它是自訂模型檢視。</span><span class="sxs-lookup"><span data-stu-id="d0d26-172">That is often called a ViewModel, because it's a custom Model for the View.</span></span> <span data-ttu-id="d0d26-173">若是小量的資料，不過，ViewBag 運作良好。</span><span class="sxs-lookup"><span data-stu-id="d0d26-173">For small amounts of data, however, the ViewBag works great.</span></span>

<span data-ttu-id="d0d26-174">返回*HelloWorldController.vb*檔案變更`Welcome`NumTimes 與訊息放入 ViewBag 控制器內的方法。</span><span class="sxs-lookup"><span data-stu-id="d0d26-174">Return to the *HelloWorldController.vb* file change the `Welcome` method inside the controller to put the Message and NumTimes into the ViewBag.</span></span> <span data-ttu-id="d0d26-175">ViewBag 是動態物件。</span><span class="sxs-lookup"><span data-stu-id="d0d26-175">The ViewBag is a dynamic object.</span></span> <span data-ttu-id="d0d26-176">這表示您可以放置任何內容給它。</span><span class="sxs-lookup"><span data-stu-id="d0d26-176">That means you can put whatever you want in to it.</span></span> <span data-ttu-id="d0d26-177">ViewBag 有定義的屬性，直到您將其內的項目。</span><span class="sxs-lookup"><span data-stu-id="d0d26-177">The ViewBag has no defined properties until you put something inside it.</span></span>

<span data-ttu-id="d0d26-178">完整`HelloWorldController.vb`與新的類別，在相同的檔案。</span><span class="sxs-lookup"><span data-stu-id="d0d26-178">The complete `HelloWorldController.vb` with the new class in the same file.</span></span>

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

<span data-ttu-id="d0d26-179">現在我們 ViewBag 包含將自動傳遞到檢視的資料。</span><span class="sxs-lookup"><span data-stu-id="d0d26-179">Now our ViewBag contains data that will be passed over to the View automatically.</span></span> <span data-ttu-id="d0d26-180">同樣地，或者我們可以具有傳入我們自己的物件，就像這樣如果我們想：</span><span class="sxs-lookup"><span data-stu-id="d0d26-180">Again, alternatively we could have passed in our own object like this if we liked:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

<span data-ttu-id="d0d26-181">現在，我們必須`WelcomeView`範本 ！</span><span class="sxs-lookup"><span data-stu-id="d0d26-181">Now we need a `WelcomeView` template!</span></span> <span data-ttu-id="d0d26-182">執行應用程式，因此新程式碼會編譯。</span><span class="sxs-lookup"><span data-stu-id="d0d26-182">Run the application so the new code is compiled.</span></span> <span data-ttu-id="d0d26-183">關閉瀏覽器，以滑鼠右鍵按一下`Welcome`方法，然後再按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="d0d26-183">Close the browser, right-click inside the `Welcome` method, and then click **Add View**.</span></span>

<span data-ttu-id="d0d26-184">以下是您**加入檢視**對話方塊看起來像。</span><span class="sxs-lookup"><span data-stu-id="d0d26-184">Here's what your **Add View** dialog box looks like.</span></span>

<span data-ttu-id="d0d26-185">[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="d0d26-185">[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)</span></span>

<span data-ttu-id="d0d26-186">下列程式碼下新增`<h2>`中新項目<em>褖畫惎。</em>vbhtml 檔案。</span><span class="sxs-lookup"><span data-stu-id="d0d26-186">Add the following code under the `<h2>` element in the new <em>Welcome.</em>vbhtml file.</span></span> <span data-ttu-id="d0d26-187">我們將進行迴圈，並假設&quot;Hello&quot;使用者說我們應該無限次 ！</span><span class="sxs-lookup"><span data-stu-id="d0d26-187">We'll make a loop and say &quot;Hello&quot; as many times as the user says we should!</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

<span data-ttu-id="d0d26-188">執行應用程式，並瀏覽至 `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`</span><span class="sxs-lookup"><span data-stu-id="d0d26-188">Run the application and browse to `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`</span></span>

<span data-ttu-id="d0d26-189">現在資料已從 URL 取得，會自動傳遞到控制器。</span><span class="sxs-lookup"><span data-stu-id="d0d26-189">Now data is taken from the URL and passed to the controller automatically.</span></span> <span data-ttu-id="d0d26-190">控制器會封裝將資料分成`Model`物件並檢視該物件傳遞。</span><span class="sxs-lookup"><span data-stu-id="d0d26-190">The controller packages up the data into a `Model` object and passes that object to the view.</span></span> <span data-ttu-id="d0d26-191">檢視比對使用者顯示為 HTML 的資料。</span><span class="sxs-lookup"><span data-stu-id="d0d26-191">The view than displays the data as HTML to the user.</span></span>

<span data-ttu-id="d0d26-192">[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="d0d26-192">[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)</span></span>

<span data-ttu-id="d0d26-193">其實是一種的&quot;M&quot;模型，但不是資料庫類型。</span><span class="sxs-lookup"><span data-stu-id="d0d26-193">Well, that was a kind of an &quot;M&quot; for model, but not the database kind.</span></span> <span data-ttu-id="d0d26-194">讓我們運用所學的內容，建立電影的資料庫。</span><span class="sxs-lookup"><span data-stu-id="d0d26-194">Let's take what we've learned and create a database of movies.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d0d26-195">[上一頁](adding-a-controller.md)
> [下一頁](adding-a-model.md)</span><span class="sxs-lookup"><span data-stu-id="d0d26-195">[Previous](adding-a-controller.md)
[Next](adding-a-model.md)</span></span>
