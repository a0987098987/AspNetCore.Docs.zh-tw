---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: 新增檢視 (C#) |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 4a4eacfdcb0b53da377e9b6812a7ce50aa94b551
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831702"
---
<a name="adding-a-view-c"></a><span data-ttu-id="64e53-103">新增檢視 (C#)</span><span class="sxs-lookup"><span data-stu-id="64e53-103">Adding a View (C#)</span></span>
====================
<span data-ttu-id="64e53-104">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="64e53-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="64e53-105">本教學課程中的更新的版本可[此處](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="64e53-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="64e53-106">它更安全、 更容易遵循，並示範更多的功能。</span><span class="sxs-lookup"><span data-stu-id="64e53-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="64e53-107">本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="64e53-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="64e53-108">在開始之前，請確定您已安裝符合下列先決條件。</span><span class="sxs-lookup"><span data-stu-id="64e53-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="64e53-109">您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="64e53-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="64e53-110">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="64e53-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="64e53-111">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="64e53-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="64e53-112">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="64e53-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="64e53-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="64e53-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="64e53-114">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="64e53-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="64e53-115">使用本主題隨附了 C# 原始程式碼的 Visual Web Developer 專案。</span><span class="sxs-lookup"><span data-stu-id="64e53-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="64e53-116">[下載 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="64e53-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="64e53-117">如果您偏好 Visual Basic，切換到[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="64e53-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="64e53-118">在本節中您要修改`HelloWorldController`類別，以使用檢視範本檔案，完全封裝產生 HTML 回應給用戶端的程序。</span><span class="sxs-lookup"><span data-stu-id="64e53-118">In this section you're going to modify the `HelloWorldController` class to use view template files to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="64e53-119">您將建立使用新的檢視範本檔案[Razor 檢視引擎](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)使用 ASP.NET MVC 3 引進。</span><span class="sxs-lookup"><span data-stu-id="64e53-119">You'll create a view template file using the new [Razor view engine](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduced with ASP.NET MVC 3.</span></span> <span data-ttu-id="64e53-120">以 razor 為基礎的檢視範本具有 *.cshtml*副檔名，且提供優雅的方式來建立 HTML 輸出使用 C#。</span><span class="sxs-lookup"><span data-stu-id="64e53-120">Razor-based view templates have a *.cshtml* file extension, and provide an elegant way to create HTML output using C#.</span></span> <span data-ttu-id="64e53-121">Razor 字元和時撰寫檢視範本時，所需按鍵的數目降至最低，並可讓程式碼撰寫工作流程迅速、 流暢。</span><span class="sxs-lookup"><span data-stu-id="64e53-121">Razor minimizes the number of characters and keystrokes required when writing a view template, and enables a fast, fluid coding workflow.</span></span>

<span data-ttu-id="64e53-122">開始使用檢視範本，內含`Index`方法中的`HelloWorldController`類別。</span><span class="sxs-lookup"><span data-stu-id="64e53-122">Start by using a view template with the `Index` method in the `HelloWorldController` class.</span></span> <span data-ttu-id="64e53-123">目前，`Index` 方法會傳回字串，內含在控制器類別中硬式編碼的訊息。</span><span class="sxs-lookup"><span data-stu-id="64e53-123">Currently the `Index` method returns a string with a message that is hard-coded in the controller class.</span></span> <span data-ttu-id="64e53-124">變更`Index`方法來傳回`View`物件，如下列所示：</span><span class="sxs-lookup"><span data-stu-id="64e53-124">Change the `Index` method to return a `View` object, as shown in the following:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

<span data-ttu-id="64e53-125">此程式碼會使用檢視範本來產生瀏覽器的 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="64e53-125">This code uses a view template to generate an HTML response to the browser.</span></span> <span data-ttu-id="64e53-126">在專案中，新增您可以使用檢視範本`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="64e53-126">In the project, add a view template that you can use with the `Index` method.</span></span> <span data-ttu-id="64e53-127">若要這樣做，以滑鼠右鍵按一下`Index`方法，然後按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="64e53-127">To do this, right-click inside the `Index` method and click **Add View**.</span></span>

![](adding-a-view/_static/image1.png)

<span data-ttu-id="64e53-128">**加入檢視** 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="64e53-128">The **Add View** dialog box appears.</span></span> <span data-ttu-id="64e53-129">保留預設值的方式，然後按一下 **新增**按鈕：</span><span class="sxs-lookup"><span data-stu-id="64e53-129">Leave the defaults the way they are and click the **Add** button:</span></span>

![](adding-a-view/_static/image2.png)

<span data-ttu-id="64e53-130">*MvcMovie\Views\HelloWorld*資料夾並*MvcMovie\Views\HelloWorld\Index.cshtml*建立檔案。</span><span class="sxs-lookup"><span data-stu-id="64e53-130">The *MvcMovie\Views\HelloWorld* folder and the *MvcMovie\Views\HelloWorld\Index.cshtml* file are created.</span></span> <span data-ttu-id="64e53-131">您可以看到它們**方案總管 中**:</span><span class="sxs-lookup"><span data-stu-id="64e53-131">You can see them in **Solution Explorer**:</span></span>

![](adding-a-view/_static/image3.png)

<span data-ttu-id="64e53-132">下列所示*Index.cshtml*所建立的檔案：</span><span class="sxs-lookup"><span data-stu-id="64e53-132">The following shows the *Index.cshtml* file that was created:</span></span>

<span data-ttu-id="64e53-133">[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="64e53-133">[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)</span></span>

<span data-ttu-id="64e53-134">將下的一些 HTML`<h2>`標記。</span><span class="sxs-lookup"><span data-stu-id="64e53-134">Add some HTML under the `<h2>` tag.</span></span> <span data-ttu-id="64e53-135">已修改*MvcMovie\Views\HelloWorld\Index.cshtml*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="64e53-135">The modified *MvcMovie\Views\HelloWorld\Index.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

<span data-ttu-id="64e53-136">執行應用程式，並瀏覽至`HelloWorld`控制站 (`http://localhost:xxxx/HelloWorld`)。</span><span class="sxs-lookup"><span data-stu-id="64e53-136">Run the application and browse to the `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`).</span></span> <span data-ttu-id="64e53-137">`Index`在控制器方法並沒有進行太多工作; 它只會執行陳述式`return View()`，其所指定方法應使用檢視範本檔案來呈現至瀏覽器的回應。</span><span class="sxs-lookup"><span data-stu-id="64e53-137">The `Index` method in your controller didn't do much work; it simply ran the statement `return View()`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="64e53-138">因為您沒有明確指定要使用的檢視範本檔案的名稱，預設使用 ASP.NET MVC *Index.cshtml*檢視中的檔案*\Views\HelloWorld*資料夾。</span><span class="sxs-lookup"><span data-stu-id="64e53-138">Because you didn't explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the *Index.cshtml* view file in the *\Views\HelloWorld* folder.</span></span> <span data-ttu-id="64e53-139">下圖顯示字串硬式編碼在檢視中。</span><span class="sxs-lookup"><span data-stu-id="64e53-139">The image below shows the string hard-coded in the view.</span></span>

![](adding-a-view/_static/image6.png)

<span data-ttu-id="64e53-140">看起來很好。</span><span class="sxs-lookup"><span data-stu-id="64e53-140">Looks pretty good.</span></span> <span data-ttu-id="64e53-141">不過，請注意，瀏覽器的標題列會顯示"Index"，並大的標題在頁面上顯示 「 我 MVC 應用程式。 」</span><span class="sxs-lookup"><span data-stu-id="64e53-141">However, notice that the browser's title bar says "Index" and the big title on the page says "My MVC Application."</span></span> <span data-ttu-id="64e53-142">讓我們將這些變更。</span><span class="sxs-lookup"><span data-stu-id="64e53-142">Let's change those.</span></span>

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="64e53-143">變更檢視和版面配置頁</span><span class="sxs-lookup"><span data-stu-id="64e53-143">Changing Views and Layout Pages</span></span>

<span data-ttu-id="64e53-144">首先，您會想要變更在頁面頂端的 [我的 MVC 應用程式] 標題。</span><span class="sxs-lookup"><span data-stu-id="64e53-144">First, you want to change the "My MVC Application" title at the top of the page.</span></span> <span data-ttu-id="64e53-145">該文字會每個頁面。</span><span class="sxs-lookup"><span data-stu-id="64e53-145">That text is common to every page.</span></span> <span data-ttu-id="64e53-146">它實際上被實作只要在一處，在專案中，即使它出現在應用程式中的每個頁面上。</span><span class="sxs-lookup"><span data-stu-id="64e53-146">It actually is implemented in only one place in the project, even though it appears on every page in the application.</span></span> <span data-ttu-id="64e53-147">移至 */檢視/Shared*資料夾中的**方案總管**，然後開啟 *\_Layout.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="64e53-147">Go to the */Views/Shared* folder in **Solution Explorer** and open the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="64e53-148">這個檔案稱為*版面配置頁*且共用 「 殼層 」 的所有其他頁面使用。</span><span class="sxs-lookup"><span data-stu-id="64e53-148">This file is called a *layout page* and it's the shared "shell" that all other pages use.</span></span>

<span data-ttu-id="64e53-149">[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="64e53-149">[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)</span></span>

<span data-ttu-id="64e53-150">版面配置範本可讓您在一個位置指定的 HTML 容器配置，您的網站，然後將它套用到您的網站中的多個頁面。</span><span class="sxs-lookup"><span data-stu-id="64e53-150">Layout templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="64e53-151">請注意`@RenderBody()`檔案底部附近的行。</span><span class="sxs-lookup"><span data-stu-id="64e53-151">Note the `@RenderBody()` line near the bottom of the file.</span></span> <span data-ttu-id="64e53-152">`RenderBody` 是的預留位置，其中您所建立的所有檢視特定頁面都出現，「 包裝的 」 版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="64e53-152">`RenderBody` is a placeholder where all the view-specific pages you create show up, "wrapped" in the layout page.</span></span> <span data-ttu-id="64e53-153">變更從 「 我 MVC 應用程式 」 到 「 MVC 電影應用程式 」 的版面配置範本中的標題。</span><span class="sxs-lookup"><span data-stu-id="64e53-153">Change the title heading in the layout template from "My MVC Application" to "MVC Movie App".</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

<span data-ttu-id="64e53-154">執行應用程式，並注意現在會顯示 「 MVC 電影應用程式 」。</span><span class="sxs-lookup"><span data-stu-id="64e53-154">Run the application and notice that it now says "MVC Movie App".</span></span> <span data-ttu-id="64e53-155">按一下 **關於**連結，您會看到如何該頁面會顯示 「 MVC 電影應用程式，「 太。</span><span class="sxs-lookup"><span data-stu-id="64e53-155">Click the **About** link, and you see how that page shows "MVC Movie App", too.</span></span> <span data-ttu-id="64e53-156">我們能夠在版面配置範本一次進行變更，並已在站台上的所有頁面都反映新的標題。</span><span class="sxs-lookup"><span data-stu-id="64e53-156">We were able to make the change once in the layout template and have all pages on the site reflect the new title.</span></span>

![](adding-a-view/_static/image9.png)

<span data-ttu-id="64e53-157">完整 *\_Layout.cshtml*檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="64e53-157">The complete *\_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

<span data-ttu-id="64e53-158">現在，讓我們變更 （檢視） 中的 [索引] 頁面的標題。</span><span class="sxs-lookup"><span data-stu-id="64e53-158">Now, let's change the title of the Index page (view).</span></span>

<span data-ttu-id="64e53-159">開啟*MvcMovie\Views\HelloWorld\Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="64e53-159">Open *MvcMovie\Views\HelloWorld\Index.cshtml*.</span></span> <span data-ttu-id="64e53-160">有兩個地方進行變更： 首先，文字，會出現在瀏覽器的標題，然後次要標頭 (`<h2>`項目)。</span><span class="sxs-lookup"><span data-stu-id="64e53-160">There are two places to make a change: first, the text that appears in the title of the browser, and then in the secondary header (the `<h2>` element).</span></span> <span data-ttu-id="64e53-161">您會使這些變更略有不同，因此可看出哪一段程式碼變更了應用程式的哪個部分。</span><span class="sxs-lookup"><span data-stu-id="64e53-161">You'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

<span data-ttu-id="64e53-162">若要表示顯示，上述程式碼的 HTML 標題`Title`的屬性`ViewBag`物件 (這是在*Index.cshtml*檢視範本)。</span><span class="sxs-lookup"><span data-stu-id="64e53-162">To indicate the HTML title to display, the code above sets a `Title` property of the `ViewBag` object (which is in the *Index.cshtml* view template).</span></span> <span data-ttu-id="64e53-163">如果您回頭看的版面配置範本的原始程式碼中，您會注意到的範本會使用此值`<title>`一部分的項目`<head>`HTML 一節。</span><span class="sxs-lookup"><span data-stu-id="64e53-163">If you look back at the source code of the layout template, you'll notice that the template uses this value in the `<title>` element as part of the `<head>` section of the HTML.</span></span> <span data-ttu-id="64e53-164">使用這個方法，可以檢視範本和版面配置檔之間輕鬆地傳遞其他參數。</span><span class="sxs-lookup"><span data-stu-id="64e53-164">Using this approach, you can easily pass other parameters between your view template and your layout file.</span></span>

<span data-ttu-id="64e53-165">執行應用程式，並瀏覽至`http://localhost:xx/HelloWorld`。</span><span class="sxs-lookup"><span data-stu-id="64e53-165">Run the application and browse to `http://localhost:xx/HelloWorld`.</span></span> <span data-ttu-id="64e53-166">請注意，瀏覽器標題、主要標題和次要標題已變更</span><span class="sxs-lookup"><span data-stu-id="64e53-166">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="64e53-167">(如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。</span><span class="sxs-lookup"><span data-stu-id="64e53-167">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="64e53-168">請在瀏覽器中按 Ctrl + F5 以強制載入來自伺服器的回應)。</span><span class="sxs-lookup"><span data-stu-id="64e53-168">Press Ctrl+F5 in your browser to force the response from the server to be loaded.)</span></span>

<span data-ttu-id="64e53-169">另外也請注意如何在內容*Index.cshtml*檢視範本與合併 *\_Layout.cshtml*檢視範本和單一 HTML 回應已傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="64e53-169">Also notice how the content in the *Index.cshtml* view template was merged with the *\_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="64e53-170">版面配置範本可讓您輕鬆進行會套用到應用程式之所有頁面的變更。</span><span class="sxs-lookup"><span data-stu-id="64e53-170">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span>

![](adding-a-view/_static/image10.png)

<span data-ttu-id="64e53-171">然而，我們的這一點點「資料」(在此案例中為 "Hello from our View Template!"</span><span class="sxs-lookup"><span data-stu-id="64e53-171">Our little bit of "data" (in this case the "Hello from our View Template!"</span></span> <span data-ttu-id="64e53-172">訊息) 是硬式編碼的資料。</span><span class="sxs-lookup"><span data-stu-id="64e53-172">message) is hard-coded, though.</span></span> <span data-ttu-id="64e53-173">MVC 應用程式具有 "V" (檢視)，並已取得 "C" (控制器)，但還沒有 "M" (模型)。</span><span class="sxs-lookup"><span data-stu-id="64e53-173">The MVC application has a "V" (view) and you've got a "C" (controller), but no "M" (model) yet.</span></span> <span data-ttu-id="64e53-174">不久之後，我們將逐步解說如何建立資料庫，並從其擷取模型資料。</span><span class="sxs-lookup"><span data-stu-id="64e53-174">Shortly, we'll walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="64e53-175">將資料從控制器傳遞至檢視</span><span class="sxs-lookup"><span data-stu-id="64e53-175">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="64e53-176">我們移至資料庫，並討論模型之前，不過，讓我們先討論將資訊從控制器傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="64e53-176">Before we go to a database and talk about models, though, let's first talk about passing information from the controller to a view.</span></span> <span data-ttu-id="64e53-177">控制器類別會叫用，以回應傳入的 URL 要求。</span><span class="sxs-lookup"><span data-stu-id="64e53-177">Controller classes are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="64e53-178">控制器類別是您撰寫處理傳入的參數，會從資料庫擷取資料和最終決定哪一種回應傳送回瀏覽器的程式碼的位置。</span><span class="sxs-lookup"><span data-stu-id="64e53-178">A controller class is where you write the code that handles the incoming parameters, retrieves data from a database, and ultimately decides what type of response to send back to the browser.</span></span> <span data-ttu-id="64e53-179">然後從控制器來產生並格式化瀏覽器的 HTML 回應使用檢視範本。</span><span class="sxs-lookup"><span data-stu-id="64e53-179">View templates can then be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="64e53-180">控制器是負責提供任何資料或物件所需為了讓檢視範本來呈現至瀏覽器的回應。</span><span class="sxs-lookup"><span data-stu-id="64e53-180">Controllers are responsible for providing whatever data or objects are required in order for a view template to render a response to the browser.</span></span> <span data-ttu-id="64e53-181">檢視範本應該永遠不會執行商務邏輯，或直接與資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="64e53-181">A view template should never perform business logic or interact with a database directly.</span></span> <span data-ttu-id="64e53-182">相反地，它應該只使用由控制器提供給它的資料。</span><span class="sxs-lookup"><span data-stu-id="64e53-182">Instead, it should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="64e53-183">維護此 「 關注點分離 」，可協助保護您的程式碼，簡潔且更容易維護。</span><span class="sxs-lookup"><span data-stu-id="64e53-183">Maintaining this "separation of concerns" helps keep your code clean and more maintainable.</span></span>

<span data-ttu-id="64e53-184">目前，`Welcome`中的動作方法`HelloWorldController`類別會採用`name`和`numTimes`參數，然後輸出到瀏覽器直接的值。</span><span class="sxs-lookup"><span data-stu-id="64e53-184">Currently, the `Welcome` action method in the `HelloWorldController` class takes a `name` and a `numTimes` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="64e53-185">與其讓控制器呈現這個回應，做為字串，讓我們變更控制器以改為使用檢視範本。</span><span class="sxs-lookup"><span data-stu-id="64e53-185">Rather than have the controller render this response as a string, let's change the controller to use a view template instead.</span></span> <span data-ttu-id="64e53-186">檢視範本會產生動態回應，這表示您需要將適當數量的資料從控制器傳遞至檢視，以便產生回應。</span><span class="sxs-lookup"><span data-stu-id="64e53-186">The view template will generate a dynamic response, which means that you need to pass appropriate bits of data from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="64e53-187">您可以透過讓控制器將檢視範本需要的動態資料放置`ViewBag`檢視範本之後可以存取的物件。</span><span class="sxs-lookup"><span data-stu-id="64e53-187">You can do this by having the controller put the dynamic data that the view template needs in a `ViewBag` object that the view template can then access.</span></span>

<span data-ttu-id="64e53-188">返回*HelloWorldController.cs*檔案，並變更`Welcome`方法來加入`Message`並`NumTimes`值`ViewBag`物件。</span><span class="sxs-lookup"><span data-stu-id="64e53-188">Return to the *HelloWorldController.cs* file and change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewBag` object.</span></span> <span data-ttu-id="64e53-189">`ViewBag` 是動態物件，這表示您可以放置任何內容給它;`ViewBag`物件有定義的屬性，直到您將其內的項目。</span><span class="sxs-lookup"><span data-stu-id="64e53-189">`ViewBag` is a dynamic object, which means you can put whatever you want in to it; the `ViewBag` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="64e53-190">完整的 *HelloWorldController.cs* 檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="64e53-190">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

<span data-ttu-id="64e53-191">現在`ViewBag`物件包含將會自動傳遞至檢視的資料。</span><span class="sxs-lookup"><span data-stu-id="64e53-191">Now the `ViewBag` object contains data that will be passed to the view automatically.</span></span>

<span data-ttu-id="64e53-192">接下來，您需要一個歡迎視圖模板！</span><span class="sxs-lookup"><span data-stu-id="64e53-192">Next, you need a Welcome view template!</span></span> <span data-ttu-id="64e53-193">在 **偵錯**功能表上，選取**建置 MvcMovie**藉此確定編譯專案。</span><span class="sxs-lookup"><span data-stu-id="64e53-193">In the **Debug** menu, select **Build MvcMovie** to make sure the project is compiled.</span></span>

<span data-ttu-id="64e53-194">[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="64e53-194">[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)</span></span>

<span data-ttu-id="64e53-195">然後以滑鼠右鍵按一下`Welcome`方法，然後按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="64e53-195">Then right-click inside the `Welcome` method and click **Add View**.</span></span> <span data-ttu-id="64e53-196">以下是什麼**加入檢視**對話方塊看起來像：</span><span class="sxs-lookup"><span data-stu-id="64e53-196">Here's what the **Add View** dialog box looks like:</span></span>

![](adding-a-view/_static/image13.png)

<span data-ttu-id="64e53-197">按一下 **新增**，然後新增下列程式碼底下`<h2>`在新的項目*Welcome.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="64e53-197">Click **Add**, and then add the following code under the `<h2>` element in the new *Welcome.cshtml* file.</span></span> <span data-ttu-id="64e53-198">您將建立多次使用者指出應該顯示"Hello"迴圈。</span><span class="sxs-lookup"><span data-stu-id="64e53-198">You'll create a loop that says "Hello" as many times as the user says it should.</span></span> <span data-ttu-id="64e53-199">完整*Welcome.cshtml*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="64e53-199">The complete *Welcome.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

<span data-ttu-id="64e53-200">執行應用程式，並瀏覽至下列 URL:</span><span class="sxs-lookup"><span data-stu-id="64e53-200">Run the application and browse to the following URL:</span></span>

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

<span data-ttu-id="64e53-201">現在資料已從 URL 取得，會自動傳遞到控制器。</span><span class="sxs-lookup"><span data-stu-id="64e53-201">Now data is taken from the URL and passed to the controller automatically.</span></span> <span data-ttu-id="64e53-202">控制器將資料封裝成`ViewBag`物件並檢視該物件傳遞。</span><span class="sxs-lookup"><span data-stu-id="64e53-202">The controller packages the data into a `ViewBag` object and passes that object to the view.</span></span> <span data-ttu-id="64e53-203">檢視則會顯示為 HTML 資料給使用者。</span><span class="sxs-lookup"><span data-stu-id="64e53-203">The view then displays the data as HTML to the user.</span></span>

![](adding-a-view/_static/image14.png)

<span data-ttu-id="64e53-204">這是一種代表模型的 "M"，但不是資料庫類型。</span><span class="sxs-lookup"><span data-stu-id="64e53-204">Well, that was a kind of an "M" for model, but not the database kind.</span></span> <span data-ttu-id="64e53-205">讓我們運用所學的內容，建立電影的資料庫。</span><span class="sxs-lookup"><span data-stu-id="64e53-205">Let's take what we've learned and create a database of movies.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="64e53-206">[上一頁](adding-a-controller.md)
> [下一頁](adding-a-model.md)</span><span class="sxs-lookup"><span data-stu-id="64e53-206">[Previous](adding-a-controller.md)
[Next](adding-a-model.md)</span></span>
