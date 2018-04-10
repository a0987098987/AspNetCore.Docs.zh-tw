---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: 加入的檢視 (VB) |Microsoft 文件
author: Rick-Anderson
description: 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: c9675eb7776116ecbe910d5515abfe9b4391df22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-view-vb"></a><span data-ttu-id="11ff7-103">加入的檢視 (VB)</span><span class="sxs-lookup"><span data-stu-id="11ff7-103">Adding a View (VB)</span></span>
====================
<span data-ttu-id="11ff7-104">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="11ff7-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="11ff7-105">本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="11ff7-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="11ff7-106">開始之前，請確定您已安裝下面所列的必要條件。</span><span class="sxs-lookup"><span data-stu-id="11ff7-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="11ff7-107">您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="11ff7-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="11ff7-108">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="11ff7-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="11ff7-109">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="11ff7-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="11ff7-110">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="11ff7-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="11ff7-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="11ff7-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="11ff7-112">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="11ff7-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="11ff7-113">使用本主題隨附在 Visual Web Developer 專案中的使用 VB.NET 原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="11ff7-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="11ff7-114">[下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="11ff7-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="11ff7-115">如果您偏好 C#，切換至[C# 版本](../cs/adding-a-view.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="11ff7-115">If you prefer C#, switch to the [C# version](../cs/adding-a-view.md) of this tutorial.</span></span>


<span data-ttu-id="11ff7-116">這一節我們將修改`HelloWorldController`類別來使用檢視範本檔案完全封裝的程序產生 HTML 用戶端的回應。</span><span class="sxs-lookup"><span data-stu-id="11ff7-116">In this section we're going to modify the `HelloWorldController` class to use a view template file to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="11ff7-117">讓我們開始使用檢視範本與`Index`方法中的`HelloWorldController`類別。</span><span class="sxs-lookup"><span data-stu-id="11ff7-117">Let's start by using a view template with the `Index` method in the `HelloWorldController` class.</span></span> <span data-ttu-id="11ff7-118">目前`Index`方法會傳回硬式編碼控制器類別內的訊息字串。</span><span class="sxs-lookup"><span data-stu-id="11ff7-118">Currently the `Index` method returns a string with a message that is hard-coded within the controller class.</span></span> <span data-ttu-id="11ff7-119">變更`Index`方法以傳回`View`物件，如下所示：</span><span class="sxs-lookup"><span data-stu-id="11ff7-119">Change the `Index` method to return a `View` object, as shown in the following:</span></span>

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

<span data-ttu-id="11ff7-120">我們現在將檢視範本加入至受測專案，我們可以叫用`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="11ff7-120">Let's now add a view template to our project that we can invoke with the `Index` method.</span></span> <span data-ttu-id="11ff7-121">若要這樣做，以滑鼠右鍵按一下`Index`方法，然後按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="11ff7-121">To do this, right-click inside the `Index` method and click **Add View**.</span></span>

<span data-ttu-id="11ff7-122">[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="11ff7-122">[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)</span></span>

<span data-ttu-id="11ff7-123">**加入檢視** 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="11ff7-123">The **Add View** dialog box appears.</span></span> <span data-ttu-id="11ff7-124">保留預設的項目，再按**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="11ff7-124">Leave the default entries and click the **Add** button.</span></span>

<span data-ttu-id="11ff7-125">[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="11ff7-125">[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)</span></span>

<span data-ttu-id="11ff7-126">*MvcMovie\Views\HelloWorld*資料夾和*MvcMovie\Views\HelloWorld\Index.vbhtml*建立檔案。</span><span class="sxs-lookup"><span data-stu-id="11ff7-126">The *MvcMovie\Views\HelloWorld* folder and the *MvcMovie\Views\HelloWorld\Index.vbhtml* file are created.</span></span> <span data-ttu-id="11ff7-127">您可以看到它們在**方案總管 中**:</span><span class="sxs-lookup"><span data-stu-id="11ff7-127">You can see them in **Solution Explorer**:</span></span>

<span data-ttu-id="11ff7-128">[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="11ff7-128">[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)</span></span>

<span data-ttu-id="11ff7-129">將一些 HTML 下的`<h2>`標記。</span><span class="sxs-lookup"><span data-stu-id="11ff7-129">Add some HTML under the `<h2>` tag.</span></span> <span data-ttu-id="11ff7-130">修改*MvcMovie\Views\HelloWorld\Index.vbhtml*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="11ff7-130">The modified *MvcMovie\Views\HelloWorld\Index.vbhtml* file is shown below.</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

<span data-ttu-id="11ff7-131">執行應用程式，並瀏覽至&quot;hello world&quot;控制站 (`http://localhost:xxxx/HelloWorld`)。</span><span class="sxs-lookup"><span data-stu-id="11ff7-131">Run the application and browse to the &quot;hello world&quot; controller (`http://localhost:xxxx/HelloWorld`).</span></span> <span data-ttu-id="11ff7-132">`Index`在控制器方法不執行許多工作; 它只需執行陳述式`return View()`，這表示我們想要使用的檢視範本檔案來呈現給用戶端的回應。</span><span class="sxs-lookup"><span data-stu-id="11ff7-132">The `Index` method in your controller didn't do much work; it simply ran the statement `return View()`, which indicated that we wanted to use a view template file to render a response to the client.</span></span> <span data-ttu-id="11ff7-133">因為我們並未明確指定要使用的檢視範本檔案的名稱，預設使用 ASP.NET MVC *Index.vbhtml*內的檢視檔案*\Views\HelloWorld*資料夾。</span><span class="sxs-lookup"><span data-stu-id="11ff7-133">Because we did not explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the *Index.vbhtml* view file within the *\Views\HelloWorld* folder.</span></span> <span data-ttu-id="11ff7-134">下圖顯示字串硬式編碼在檢視中。</span><span class="sxs-lookup"><span data-stu-id="11ff7-134">The image below shows the string hard-coded in the view.</span></span>

<span data-ttu-id="11ff7-135">[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="11ff7-135">[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)</span></span>

<span data-ttu-id="11ff7-136">看起來很好。</span><span class="sxs-lookup"><span data-stu-id="11ff7-136">Looks pretty good.</span></span> <span data-ttu-id="11ff7-137">不過，請注意，在瀏覽器標題列顯示&quot;索引&quot;大的標題，在頁面上顯示 &quot;我的 MVC 應用程式。&quot;讓我們將這些變更。</span><span class="sxs-lookup"><span data-stu-id="11ff7-137">However, notice that the browser's title bar says &quot;Index&quot; and the big title on the page says &quot;My MVC Application.&quot; Let's change those.</span></span>

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="11ff7-138">變更檢視和版面配置頁</span><span class="sxs-lookup"><span data-stu-id="11ff7-138">Changing views and layout pages</span></span>

<span data-ttu-id="11ff7-139">首先，我們變更文字&quot;我的 MVC 應用程式。&quot;該文字共用，並顯示每一頁上。</span><span class="sxs-lookup"><span data-stu-id="11ff7-139">First, let's change the text &quot;My MVC Application.&quot; That text is shared and appears on every page.</span></span> <span data-ttu-id="11ff7-140">它實際上不會顯示僅在一個地方我們的受測專案，即使它是我們的應用程式中的每一頁上。</span><span class="sxs-lookup"><span data-stu-id="11ff7-140">It actually appears in only one place in our project, even though it's on every page in our application.</span></span> <span data-ttu-id="11ff7-141">移至*/檢視表/共用*資料夾中的**方案總管 中**並開啟 *\_Layout.vbhtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="11ff7-141">Go to the */Views/Shared* folder in **Solution Explorer** and open the *\_Layout.vbhtml* file.</span></span> <span data-ttu-id="11ff7-142">這個檔案稱為版面配置頁面，它是共用&quot;殼層&quot;所有其他頁面使用。</span><span class="sxs-lookup"><span data-stu-id="11ff7-142">This file is called a layout page and it's the shared &quot;shell&quot; that all other pages use.</span></span>

<span data-ttu-id="11ff7-143">請注意`@RenderBody()`檔案底部附近的程式碼行。</span><span class="sxs-lookup"><span data-stu-id="11ff7-143">Note the `@RenderBody()` line of code near the bottom of the file.</span></span> <span data-ttu-id="11ff7-144">`RenderBody` 這是的預留位置，其中您所建立的所有頁面會都出現&quot;包裝&quot;版面配置頁面中。</span><span class="sxs-lookup"><span data-stu-id="11ff7-144">`RenderBody` is a placeholder where all the pages you create show up, &quot;wrapped&quot; in the layout page.</span></span> <span data-ttu-id="11ff7-145">變更`<h1>`標題從**&quot;**我的 MVC 應用程式&quot;至&quot;MVC 電影應用程式&quot;。</span><span class="sxs-lookup"><span data-stu-id="11ff7-145">Change the `<h1>` heading from **&quot;** My MVC Application&quot; to &quot;MVC Movie App&quot;.</span></span>

[!code-html[Main](adding-a-view/samples/sample3.html)]

<span data-ttu-id="11ff7-146">執行應用程式，並注意它現在說&quot;MVC 電影應用程式&quot;。</span><span class="sxs-lookup"><span data-stu-id="11ff7-146">Run the application and note it now says &quot;MVC Movie App&quot;.</span></span> <span data-ttu-id="11ff7-147">按一下**有關**連結，且頁面上顯示&quot;MVC 電影應用程式&quot;、 太。</span><span class="sxs-lookup"><span data-stu-id="11ff7-147">Click the **About** link, and that page shows &quot;MVC Movie App&quot;, too.</span></span>

<span data-ttu-id="11ff7-148">完整 *\_Layout.vbhtml*檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="11ff7-148">The complete *\_Layout.vbhtml* file is shown below:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

<span data-ttu-id="11ff7-149">現在，我們來變更索引頁面 （檢視） 的標題。</span><span class="sxs-lookup"><span data-stu-id="11ff7-149">Now, let's change the title of the Index page (view).</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

<span data-ttu-id="11ff7-150">開啟*MvcMovie\Views\HelloWorld\Index.vbhtml*。</span><span class="sxs-lookup"><span data-stu-id="11ff7-150">Open *MvcMovie\Views\HelloWorld\Index.vbhtml*.</span></span> <span data-ttu-id="11ff7-151">若要變更的兩個地方： 第一次，文字會出現在標題的瀏覽器，然後在次要標頭 (`<h2>`項目)。</span><span class="sxs-lookup"><span data-stu-id="11ff7-151">There are two places to make a change: first, the text that appears in the title of the browser, and then in the secondary header (the `<h2>` element).</span></span> <span data-ttu-id="11ff7-152">我們會讓它們稍有不同，才可看到的許多程式碼變更的應用程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="11ff7-152">We'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>

<span data-ttu-id="11ff7-153">執行應用程式，並瀏覽至`http://localhost:xx/HelloWorld`。</span><span class="sxs-lookup"><span data-stu-id="11ff7-153">Run the application and browse to`http://localhost:xx/HelloWorld`.</span></span> <span data-ttu-id="11ff7-154">請注意，瀏覽器標題、主要標題和次要標題已變更</span><span class="sxs-lookup"><span data-stu-id="11ff7-154">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="11ff7-155">所以可以輕鬆地變更大些微變更，您的應用程式中檢視。</span><span class="sxs-lookup"><span data-stu-id="11ff7-155">It's easy to make big changes in your application with small changes to a view.</span></span> <span data-ttu-id="11ff7-156">(如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。</span><span class="sxs-lookup"><span data-stu-id="11ff7-156">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="11ff7-157">請在瀏覽器中按 Ctrl + F5 以強制載入來自伺服器的回應)。</span><span class="sxs-lookup"><span data-stu-id="11ff7-157">Press Ctrl+F5 in your browser to force the response from the server to be loaded.)</span></span>

<span data-ttu-id="11ff7-158">[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="11ff7-158">[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)</span></span>

<span data-ttu-id="11ff7-159">我們一點&quot;資料&quot;(在此情況下&quot;Hello World ！&quot;訊息) 是硬式編碼，不過。</span><span class="sxs-lookup"><span data-stu-id="11ff7-159">Our little bit of &quot;data&quot; (in this case the &quot;Hello World!&quot; message) is hard-coded, though.</span></span> <span data-ttu-id="11ff7-160">我們的 MVC 應用程式具有 V (views)，而且我們有 C （控制站），但沒有 M （模型） 尚未。</span><span class="sxs-lookup"><span data-stu-id="11ff7-160">Our MVC application has V (views) and we've got C (controllers), but no M (model) yet.</span></span> <span data-ttu-id="11ff7-161">稍後，我們將逐步解說如何建立資料庫，並從其擷取模型資料。</span><span class="sxs-lookup"><span data-stu-id="11ff7-161">Shortly, we'll walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="11ff7-162">將資料從控制器傳遞至檢視</span><span class="sxs-lookup"><span data-stu-id="11ff7-162">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="11ff7-163">我們移至資料庫，並討論模型之前，不過，我們要先討論有關將資訊從控制器傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="11ff7-163">Before we go to a database and talk about models, though, let's first talk about passing information from the Controller to a View.</span></span> <span data-ttu-id="11ff7-164">我們想要傳遞呈現 HTML 回應至用戶端所需要的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="11ff7-164">We want to pass what a view template requires in order to render an HTML response to a client.</span></span> <span data-ttu-id="11ff7-165">通常建立和由控制器類別傳遞給檢視範本時，這些物件，而且它們應該包含檢視範本需要的資料，並不多。</span><span class="sxs-lookup"><span data-stu-id="11ff7-165">These objects are typically created and passed by a controller class to a view template, and they should contain only the data that the view template requires — and no more.</span></span>

<span data-ttu-id="11ff7-166">先前使用`HelloWorldController`類別`Welcome`動作方法耗用`name`和`numTimes`參數和參數值至瀏覽器然後輸出。</span><span class="sxs-lookup"><span data-stu-id="11ff7-166">Previously with the `HelloWorldController` class, the `Welcome` action method took a `name` and a `numTimes` parameter and then output the parameter values to the browser.</span></span> <span data-ttu-id="11ff7-167">而非保有繼續直接轉譯這個回應的控制站，讓我們改為說該資料包中檢視。</span><span class="sxs-lookup"><span data-stu-id="11ff7-167">Rather than have the controller continue to render this response directly, let's instead we'll put that data in a bag for the View.</span></span> <span data-ttu-id="11ff7-168">控制器和檢視可以使用`ViewBag`物件來保存該資料。</span><span class="sxs-lookup"><span data-stu-id="11ff7-168">Controllers and Views can use a `ViewBag` object to hold that data.</span></span> <span data-ttu-id="11ff7-169">以便自動傳遞到檢視表範本，並且用來呈現 HTML 回應使用的屬性包內容做為資料。</span><span class="sxs-lookup"><span data-stu-id="11ff7-169">That will be passed over to a view template automatically, and used to render the HTML response using the contents of the bag as data.</span></span> <span data-ttu-id="11ff7-170">控制器關於一件事，而另一個檢視範本，這樣一來，讓我們能夠維護全新&quot;的重要性分離&quot;應用程式中。</span><span class="sxs-lookup"><span data-stu-id="11ff7-170">That way the controller is concerned with one thing and the view template with another — enabling us to maintain clean &quot;separation of concerns&quot; within the application.</span></span>

<span data-ttu-id="11ff7-171">或者，我們無法定義自訂類別，然後自己上建立該物件的執行個體、 填入資料再將它傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="11ff7-171">Alternatively, we could define a custom class, then create an instance of that object on our own, fill it with data and pass it to the View.</span></span> <span data-ttu-id="11ff7-172">通常稱為 ViewModel，因為它是自訂的檢視模型。</span><span class="sxs-lookup"><span data-stu-id="11ff7-172">That is often called a ViewModel, because it's a custom Model for the View.</span></span> <span data-ttu-id="11ff7-173">對於小量的資料，不過，ViewBag 非常適合。</span><span class="sxs-lookup"><span data-stu-id="11ff7-173">For small amounts of data, however, the ViewBag works great.</span></span>

<span data-ttu-id="11ff7-174">返回*HelloWorldController.vb*檔案變更`Welcome`內將訊息和 NumTimes 放入 ViewBag 控制器方法。</span><span class="sxs-lookup"><span data-stu-id="11ff7-174">Return to the *HelloWorldController.vb* file change the `Welcome` method inside the controller to put the Message and NumTimes into the ViewBag.</span></span> <span data-ttu-id="11ff7-175">ViewBag 是動態的物件。</span><span class="sxs-lookup"><span data-stu-id="11ff7-175">The ViewBag is a dynamic object.</span></span> <span data-ttu-id="11ff7-176">這表示您可以將任何您要它。</span><span class="sxs-lookup"><span data-stu-id="11ff7-176">That means you can put whatever you want in to it.</span></span> <span data-ttu-id="11ff7-177">ViewBag 並沒有已定義的內容，直到您將放在它之內的項目。</span><span class="sxs-lookup"><span data-stu-id="11ff7-177">The ViewBag has no defined properties until you put something inside it.</span></span>

<span data-ttu-id="11ff7-178">完整`HelloWorldController.vb`與新的類別，在相同的檔案。</span><span class="sxs-lookup"><span data-stu-id="11ff7-178">The complete `HelloWorldController.vb` with the new class in the same file.</span></span>

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

<span data-ttu-id="11ff7-179">現在我們 ViewBag 包含會自動傳遞到檢視的資料。</span><span class="sxs-lookup"><span data-stu-id="11ff7-179">Now our ViewBag contains data that will be passed over to the View automatically.</span></span> <span data-ttu-id="11ff7-180">同樣地，或者我們可以有自己的物件，像這樣如果傳入我們喜歡：</span><span class="sxs-lookup"><span data-stu-id="11ff7-180">Again, alternatively we could have passed in our own object like this if we liked:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

<span data-ttu-id="11ff7-181">現在，我們必須`WelcomeView`範本 ！</span><span class="sxs-lookup"><span data-stu-id="11ff7-181">Now we need a `WelcomeView` template!</span></span> <span data-ttu-id="11ff7-182">執行應用程式，因此新的程式碼會編譯。</span><span class="sxs-lookup"><span data-stu-id="11ff7-182">Run the application so the new code is compiled.</span></span> <span data-ttu-id="11ff7-183">關閉瀏覽器，以滑鼠右鍵按一下`Welcome`方法，然後再按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="11ff7-183">Close the browser, right-click inside the `Welcome` method, and then click **Add View**.</span></span>

<span data-ttu-id="11ff7-184">以下是您**加入檢視**對話方塊看起來。</span><span class="sxs-lookup"><span data-stu-id="11ff7-184">Here's what your **Add View** dialog box looks like.</span></span>

<span data-ttu-id="11ff7-185">[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="11ff7-185">[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)</span></span>

<span data-ttu-id="11ff7-186">下列程式碼下新增`<h2>`中新項目<em> 褖畫惎。</em>vbhtml 檔案。</span><span class="sxs-lookup"><span data-stu-id="11ff7-186">Add the following code under the `<h2>` element in the new <em>Welcome.</em>vbhtml file.</span></span> <span data-ttu-id="11ff7-187">我們將進行迴圈，並假設&quot;Hello&quot;依需求多次使用者可指定我們應該 ！</span><span class="sxs-lookup"><span data-stu-id="11ff7-187">We'll make a loop and say &quot;Hello&quot; as many times as the user says we should!</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

<span data-ttu-id="11ff7-188">執行應用程式，並瀏覽至 `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`</span><span class="sxs-lookup"><span data-stu-id="11ff7-188">Run the application and browse to `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`</span></span>

<span data-ttu-id="11ff7-189">現在從 URL 取得及資料自動傳送到控制器。</span><span class="sxs-lookup"><span data-stu-id="11ff7-189">Now data is taken from the URL and passed to the controller automatically.</span></span> <span data-ttu-id="11ff7-190">控制器的封裝將資料`Model`物件，以及檢視該物件傳遞。</span><span class="sxs-lookup"><span data-stu-id="11ff7-190">The controller packages up the data into a `Model` object and passes that object to the view.</span></span> <span data-ttu-id="11ff7-191">檢視比對使用者顯示為 HTML 的資料。</span><span class="sxs-lookup"><span data-stu-id="11ff7-191">The view than displays the data as HTML to the user.</span></span>

<span data-ttu-id="11ff7-192">[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="11ff7-192">[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)</span></span>

<span data-ttu-id="11ff7-193">就是一種的&quot;M&quot;的模型，但資料庫類型。</span><span class="sxs-lookup"><span data-stu-id="11ff7-193">Well, that was a kind of an &quot;M&quot; for model, but not the database kind.</span></span> <span data-ttu-id="11ff7-194">讓我們運用所學的內容，建立電影的資料庫。</span><span class="sxs-lookup"><span data-stu-id="11ff7-194">Let's take what we've learned and create a database of movies.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="11ff7-195">[上一頁](adding-a-controller.md)
> [下一頁](adding-a-model.md)</span><span class="sxs-lookup"><span data-stu-id="11ff7-195">[Previous](adding-a-controller.md)
[Next](adding-a-model.md)</span></span>
