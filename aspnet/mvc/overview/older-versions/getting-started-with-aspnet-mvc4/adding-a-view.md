---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: 新增檢視 |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本就可以使用這裡使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 更容易遵循，並示範...
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7c7179419a126eaa998252e75ac38b3b1a2b23d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830977"
---
<a name="adding-a-view"></a><span data-ttu-id="88112-104">新增檢視</span><span class="sxs-lookup"><span data-stu-id="88112-104">Adding a View</span></span>
====================
<span data-ttu-id="88112-105">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="88112-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="88112-106">本教學課程中的更新的版本可[此處](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="88112-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="88112-107">它更安全、 更容易遵循，並示範更多的功能。</span><span class="sxs-lookup"><span data-stu-id="88112-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="88112-108">在本節中您要修改`HelloWorldController`類別，以使用檢視範本檔案，完全封裝產生 HTML 回應給用戶端的程序。</span><span class="sxs-lookup"><span data-stu-id="88112-108">In this section you're going to modify the `HelloWorldController` class to use view template files to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="88112-109">您將建立檢視範本檔案中使用[Razor 檢視引擎](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)使用 ASP.NET MVC 3 引進。</span><span class="sxs-lookup"><span data-stu-id="88112-109">You'll create a view template file using the [Razor view engine](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduced with ASP.NET MVC 3.</span></span> <span data-ttu-id="88112-110">以 razor 為基礎的檢視範本具有 *.cshtml*副檔名，且提供優雅的方式來建立 HTML 輸出使用 C#。</span><span class="sxs-lookup"><span data-stu-id="88112-110">Razor-based view templates have a *.cshtml* file extension, and provide an elegant way to create HTML output using C#.</span></span> <span data-ttu-id="88112-111">Razor 字元和時撰寫檢視範本時，所需按鍵的數目降至最低，並可讓程式碼撰寫工作流程迅速、 流暢。</span><span class="sxs-lookup"><span data-stu-id="88112-111">Razor minimizes the number of characters and keystrokes required when writing a view template, and enables a fast, fluid coding workflow.</span></span>

<span data-ttu-id="88112-112">目前，`Index` 方法會傳回字串，內含在控制器類別中硬式編碼的訊息。</span><span class="sxs-lookup"><span data-stu-id="88112-112">Currently the `Index` method returns a string with a message that is hard-coded in the controller class.</span></span> <span data-ttu-id="88112-113">變更`Index`方法來傳回`View`物件，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="88112-113">Change the `Index` method to return a `View` object, as shown in the following code:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

<span data-ttu-id="88112-114">`Index`上述方法來產生 HTML 回應給瀏覽器中使用檢視範本。</span><span class="sxs-lookup"><span data-stu-id="88112-114">The `Index` method above uses a view template to generate an HTML response to the browser.</span></span> <span data-ttu-id="88112-115">控制器方法 (也稱為[動作方法](http://rachelappel.com/asp.net-mvc-actionresults-explained))，例如`Index`上述，方法通常會傳回[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (或衍生自類別[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx))，不是基本類型與字串相同。</span><span class="sxs-lookup"><span data-stu-id="88112-115">Controller methods (also known as [action methods](http://rachelappel.com/asp.net-mvc-actionresults-explained)), such as the `Index` method above, generally return an [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (or a class derived from [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), not primitive types like string.</span></span>

<span data-ttu-id="88112-116">在專案中，新增您可以使用檢視範本`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="88112-116">In the project, add a view template that you can use with the `Index` method.</span></span> <span data-ttu-id="88112-117">若要這樣做，以滑鼠右鍵按一下`Index`方法，然後按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="88112-117">To do this, right-click inside the `Index` method and click **Add View**.</span></span>

![](adding-a-view/_static/image1.png)

<span data-ttu-id="88112-118">**加入檢視** 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="88112-118">The **Add View** dialog box appears.</span></span> <span data-ttu-id="88112-119">保留預設值的方式，然後按一下 **新增**按鈕：</span><span class="sxs-lookup"><span data-stu-id="88112-119">Leave the defaults the way they are and click the **Add** button:</span></span>

![](adding-a-view/_static/image2.png)

<span data-ttu-id="88112-120">*MvcMovie\Views\HelloWorld*資料夾並*MvcMovie\Views\HelloWorld\Index.cshtml*建立檔案。</span><span class="sxs-lookup"><span data-stu-id="88112-120">The *MvcMovie\Views\HelloWorld* folder and the *MvcMovie\Views\HelloWorld\Index.cshtml* file are created.</span></span> <span data-ttu-id="88112-121">您可以看到它們**方案總管 中**:</span><span class="sxs-lookup"><span data-stu-id="88112-121">You can see them in **Solution Explorer**:</span></span>

![](adding-a-view/_static/image3.png)

<span data-ttu-id="88112-122">下列所示*Index.cshtml*所建立的檔案：</span><span class="sxs-lookup"><span data-stu-id="88112-122">The following shows the *Index.cshtml* file that was created:</span></span>

![HelloWorldIndex](adding-a-view/_static/image4.png)

<span data-ttu-id="88112-124">新增下列 HTML 下的`<h2>`標記。</span><span class="sxs-lookup"><span data-stu-id="88112-124">Add the following HTML under the `<h2>` tag.</span></span>

[!code-html[Main](adding-a-view/samples/sample2.html)]

<span data-ttu-id="88112-125">完整*MvcMovie\Views\HelloWorld\Index.cshtml*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="88112-125">The complete *MvcMovie\Views\HelloWorld\Index.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

<span data-ttu-id="88112-126">如果您使用 Visual Studio 2012 中，在 [方案總管] 中，以滑鼠右鍵按一下*Index.cshtml*檔案，然後選取**Page Inspector 中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="88112-126">If you are using Visual Studio 2012, in solution explorer, right click the *Index.cshtml* file and select **View in Page Inspector**.</span></span>

![PI](adding-a-view/_static/image5.png)

<span data-ttu-id="88112-128">[Page Inspector 教學課程](../../views/using-page-inspector-in-aspnet-mvc.md)提供的這項新工具的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="88112-128">The [Page Inspector tutorial](../../views/using-page-inspector-in-aspnet-mvc.md) has more information about this new tool.</span></span>

<span data-ttu-id="88112-129">或者，執行應用程式，並瀏覽至`HelloWorld`控制站 (`http://localhost:xxxx/HelloWorld`)。</span><span class="sxs-lookup"><span data-stu-id="88112-129">Alternatively, run the application and browse to the `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`).</span></span> <span data-ttu-id="88112-130">`Index`在控制器方法並沒有進行太多工作; 它只會執行陳述式`return View()`，其所指定方法應使用檢視範本檔案來呈現至瀏覽器的回應。</span><span class="sxs-lookup"><span data-stu-id="88112-130">The `Index` method in your controller didn't do much work; it simply ran the statement `return View()`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="88112-131">因為您沒有明確指定要使用的檢視範本檔案的名稱，預設使用 ASP.NET MVC *Index.cshtml*檢視中的檔案*\Views\HelloWorld*資料夾。</span><span class="sxs-lookup"><span data-stu-id="88112-131">Because you didn't explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the *Index.cshtml* view file in the *\Views\HelloWorld* folder.</span></span> <span data-ttu-id="88112-132">下圖顯示的字串&quot;Hello from our View Template ！&quot;硬式編碼在檢視中。</span><span class="sxs-lookup"><span data-stu-id="88112-132">The image below shows the string &quot;Hello from our View Template!&quot; hard-coded in the view.</span></span>

![](adding-a-view/_static/image6.png)

<span data-ttu-id="88112-133">看起來很好。</span><span class="sxs-lookup"><span data-stu-id="88112-133">Looks pretty good.</span></span> <span data-ttu-id="88112-134">不過，請注意，瀏覽器的標題列會顯示&quot;我的 ASP.NET 的編製索引&quot;並在頁面頂端的大型連結會顯示&quot;您的標誌。&quot;下面&quot;您標誌。&quot;連結大約會註冊記錄檔中的連結，並連結至首頁，以下和連絡人 頁面。</span><span class="sxs-lookup"><span data-stu-id="88112-134">However, notice that the browser's title bar shows &quot;Index My ASP.NET A&quot; and the big link on the top of the page says &quot;your logo here.&quot; Below the &quot;your logo here.&quot; link are registration and log in links, and below that links to Home, About and Contact pages.</span></span> <span data-ttu-id="88112-135">讓我們變更其中某些問題。</span><span class="sxs-lookup"><span data-stu-id="88112-135">Let's change some of these.</span></span>

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="88112-136">變更檢視和版面配置頁</span><span class="sxs-lookup"><span data-stu-id="88112-136">Changing Views and Layout Pages</span></span>

<span data-ttu-id="88112-137">首先，您想要變更的地方&quot;您標誌。&quot;標題在頁面頂端。</span><span class="sxs-lookup"><span data-stu-id="88112-137">First, you want to change the &quot;your logo here.&quot; title at the top of the page.</span></span> <span data-ttu-id="88112-138">該文字會每個頁面。</span><span class="sxs-lookup"><span data-stu-id="88112-138">That text is common to every page.</span></span> <span data-ttu-id="88112-139">雖然看起來的應用程式中的每個頁面上，它實際上被實作在專案中，只有一個位置。</span><span class="sxs-lookup"><span data-stu-id="88112-139">It's actually implemented in only one place in the project, even though it appears on every page in the application.</span></span> <span data-ttu-id="88112-140">移至 */檢視/Shared*資料夾中的**方案總管**，然後開啟 *\_Layout.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="88112-140">Go to the */Views/Shared* folder in **Solution Explorer** and open the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="88112-141">這個檔案稱為*版面配置頁*很共用&quot;shell&quot;所有其他頁面使用。</span><span class="sxs-lookup"><span data-stu-id="88112-141">This file is called a *layout page* and it's the shared &quot;shell&quot; that all other pages use.</span></span>

![_LayoutCshtml](adding-a-view/_static/image7.png)

<span data-ttu-id="88112-143">版面配置範本可讓您在一個位置指定的 HTML 容器配置，您的網站，然後將它套用到您的網站中的多個頁面。</span><span class="sxs-lookup"><span data-stu-id="88112-143">Layout templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="88112-144">找到 `@RenderBody()` 這行。</span><span class="sxs-lookup"><span data-stu-id="88112-144">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="88112-145">`RenderBody` 是顯示您建立之所有檢視特定頁面的預留位置，「包裝」&quot;&quot;在版面配置頁中。</span><span class="sxs-lookup"><span data-stu-id="88112-145">`RenderBody` is a placeholder where all the view-specific pages you create show up, &quot;wrapped&quot; in the layout page.</span></span> <span data-ttu-id="88112-146">例如，如果您選取 [關於] 連結中， *Views\Home\About.cshtml*內呈現檢視`RenderBody`方法。</span><span class="sxs-lookup"><span data-stu-id="88112-146">For example, if you select the About link, the *Views\Home\About.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<span data-ttu-id="88112-147">變更網站標題中的 標題的版面配置範本，從&quot;您的標誌&quot;要&quot;MVC Movie&quot;。</span><span class="sxs-lookup"><span data-stu-id="88112-147">Change the site-title heading in the layout template from &quot;your logo here&quot; to &quot;MVC Movie&quot;.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

<span data-ttu-id="88112-148">取代為下列標記中的標題項目的內容：</span><span class="sxs-lookup"><span data-stu-id="88112-148">Replace the contents of the title element with the following markup:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

<span data-ttu-id="88112-149">執行應用程式和通知，現在指出&quot;MVC Movie &quot;。</span><span class="sxs-lookup"><span data-stu-id="88112-149">Run the application and notice that it now says &quot;MVC Movie &quot;.</span></span> <span data-ttu-id="88112-150">按一下 **關於**連結，您會看到該頁面會顯示如何&quot;MVC Movie&quot;也。</span><span class="sxs-lookup"><span data-stu-id="88112-150">Click the **About** link, and you see how that page shows &quot;MVC Movie&quot;, too.</span></span> <span data-ttu-id="88112-151">我們能夠在版面配置範本一次進行變更，並已在站台上的所有頁面都反映新的標題。</span><span class="sxs-lookup"><span data-stu-id="88112-151">We were able to make the change once in the layout template and have all pages on the site reflect the new title.</span></span>

![](adding-a-view/_static/image8.png)

<span data-ttu-id="88112-152">現在，讓我們變更索引 檢視的標題。</span><span class="sxs-lookup"><span data-stu-id="88112-152">Now, let's change the title of the Index view.</span></span>

<span data-ttu-id="88112-153">開啟*MvcMovie\Views\HelloWorld\Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="88112-153">Open *MvcMovie\Views\HelloWorld\Index.cshtml*.</span></span> <span data-ttu-id="88112-154">有兩個地方進行變更： 首先，文字，會出現在瀏覽器的標題，然後次要標頭 (`<h2>`項目)。</span><span class="sxs-lookup"><span data-stu-id="88112-154">There are two places to make a change: first, the text that appears in the title of the browser, and then in the secondary header (the `<h2>` element).</span></span> <span data-ttu-id="88112-155">您會使這些變更略有不同，因此可看出哪一段程式碼變更了應用程式的哪個部分。</span><span class="sxs-lookup"><span data-stu-id="88112-155">You'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

<span data-ttu-id="88112-156">若要表示顯示，上述程式碼的 HTML 標題`Title`的屬性`ViewBag`物件 (這是在*Index.cshtml*檢視範本)。</span><span class="sxs-lookup"><span data-stu-id="88112-156">To indicate the HTML title to display, the code above sets a `Title` property of the `ViewBag` object (which is in the *Index.cshtml* view template).</span></span> <span data-ttu-id="88112-157">如果您回頭看的版面配置範本的原始程式碼中，您會注意到的範本會使用此值`<title>`一部分的項目`<head>`我們先前修改 HTML 一節。</span><span class="sxs-lookup"><span data-stu-id="88112-157">If you look back at the source code of the layout template, you'll notice that the template uses this value in the `<title>` element as part of the `<head>` section of the HTML that we modified previously.</span></span> <span data-ttu-id="88112-158">使用此`ViewBag`方法時，您可以輕鬆地傳遞其他參數之間檢視範本和配置檔案。</span><span class="sxs-lookup"><span data-stu-id="88112-158">Using this `ViewBag` approach, you can easily pass other parameters between your view template and your layout file.</span></span>

<span data-ttu-id="88112-159">執行應用程式，並瀏覽至`http://localhost:xx/HelloWorld`。</span><span class="sxs-lookup"><span data-stu-id="88112-159">Run the application and browse to `http://localhost:xx/HelloWorld`.</span></span> <span data-ttu-id="88112-160">請注意，瀏覽器標題、主要標題和次要標題已變更</span><span class="sxs-lookup"><span data-stu-id="88112-160">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="88112-161">(如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。</span><span class="sxs-lookup"><span data-stu-id="88112-161">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="88112-162">請在瀏覽器中按 Ctrl + F5 以強制載入來自伺服器的回應)。瀏覽器標題會透過`ViewBag.Title`我們在中設定*Index.cshtml*檢視範本和其他&quot;-Movie App&quot;配置檔案中新增。</span><span class="sxs-lookup"><span data-stu-id="88112-162">Press Ctrl+F5 in your browser to force the response from the server to be loaded.) The browser title is created with the `ViewBag.Title` we set in the *Index.cshtml* view template and the additional &quot;- Movie App&quot; added in the layout file.</span></span>

<span data-ttu-id="88112-163">另外也請注意如何在內容*Index.cshtml*檢視範本與合併 *\_Layout.cshtml*檢視範本和單一 HTML 回應已傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="88112-163">Also notice how the content in the *Index.cshtml* view template was merged with the *\_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="88112-164">版面配置範本可讓您輕鬆進行會套用到應用程式之所有頁面的變更。</span><span class="sxs-lookup"><span data-stu-id="88112-164">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span>

![](adding-a-view/_static/image9.png)

<span data-ttu-id="88112-165">我們一點&quot;資料&quot;(在此情況下&quot;Hello from our View Template ！&quot;訊息) 是硬式編碼，不過。</span><span class="sxs-lookup"><span data-stu-id="88112-165">Our little bit of &quot;data&quot; (in this case the &quot;Hello from our View Template!&quot; message) is hard-coded, though.</span></span> <span data-ttu-id="88112-166">MVC 應用程式具有&quot;V&quot; （檢視），您可盡情&quot;C&quot; （控制器），但沒有&quot;M&quot; （模型） 尚未。</span><span class="sxs-lookup"><span data-stu-id="88112-166">The MVC application has a &quot;V&quot; (view) and you've got a &quot;C&quot; (controller), but no &quot;M&quot; (model) yet.</span></span> <span data-ttu-id="88112-167">不久之後，我們將逐步解說如何建立資料庫，並從其擷取模型資料。</span><span class="sxs-lookup"><span data-stu-id="88112-167">Shortly, we'll walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="88112-168">將資料從控制器傳遞至檢視</span><span class="sxs-lookup"><span data-stu-id="88112-168">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="88112-169">我們移至資料庫，並討論模型之前，不過，讓我們先討論將資訊從控制器傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="88112-169">Before we go to a database and talk about models, though, let's first talk about passing information from the controller to a view.</span></span> <span data-ttu-id="88112-170">控制器類別會叫用，以回應傳入的 URL 要求。</span><span class="sxs-lookup"><span data-stu-id="88112-170">Controller classes are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="88112-171">控制器類別是回應的您可在此撰寫處理傳入的瀏覽器的程式碼要求、 從資料庫擷取資料和最終決定哪一種將傳回給瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="88112-171">A controller class is where you write the code that handles the incoming browser requests, retrieves data from a database, and ultimately decides what type of response to send back to the browser.</span></span> <span data-ttu-id="88112-172">然後從控制器來產生並格式化瀏覽器的 HTML 回應使用檢視範本。</span><span class="sxs-lookup"><span data-stu-id="88112-172">View templates can then be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="88112-173">控制器是負責提供任何資料或物件所需為了讓檢視範本來呈現至瀏覽器的回應。</span><span class="sxs-lookup"><span data-stu-id="88112-173">Controllers are responsible for providing whatever data or objects are required in order for a view template to render a response to the browser.</span></span> <span data-ttu-id="88112-174">最佳做法是：**檢視範本應該永遠不會執行商務邏輯或直接與資料庫互動**。</span><span class="sxs-lookup"><span data-stu-id="88112-174">A best practice: **A view template should never perform business logic or interact with a database directly**.</span></span> <span data-ttu-id="88112-175">相反地，檢視範本應該只使用由控制器提供給它的資料。</span><span class="sxs-lookup"><span data-stu-id="88112-175">Instead, a view template should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="88112-176">維護這&quot;關注點分離&quot;精簡、 可測試且更易於維護，協助保持您的程式碼。</span><span class="sxs-lookup"><span data-stu-id="88112-176">Maintaining this &quot;separation of concerns&quot; helps keep your code clean, testable and more maintainable.</span></span>

<span data-ttu-id="88112-177">目前，`Welcome`中的動作方法`HelloWorldController`類別會採用`name`和`numTimes`參數，然後輸出到瀏覽器直接的值。</span><span class="sxs-lookup"><span data-stu-id="88112-177">Currently, the `Welcome` action method in the `HelloWorldController` class takes a `name` and a `numTimes` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="88112-178">與其讓控制器呈現這個回應，做為字串，讓我們變更控制器以改為使用檢視範本。</span><span class="sxs-lookup"><span data-stu-id="88112-178">Rather than have the controller render this response as a string, let's change the controller to use a view template instead.</span></span> <span data-ttu-id="88112-179">檢視範本會產生動態回應，這表示您需要將適當數量的資料從控制器傳遞至檢視，以便產生回應。</span><span class="sxs-lookup"><span data-stu-id="88112-179">The view template will generate a dynamic response, which means that you need to pass appropriate bits of data from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="88112-180">您可以透過讓控制器將檢視範本需要的動態資料 （參數） 放置`ViewBag`檢視範本之後可以存取的物件。</span><span class="sxs-lookup"><span data-stu-id="88112-180">You can do this by having the controller put the dynamic data (parameters) that the view template needs in a `ViewBag` object that the view template can then access.</span></span>

<span data-ttu-id="88112-181">返回*HelloWorldController.cs*檔案，並變更`Welcome`方法來加入`Message`並`NumTimes`值`ViewBag`物件。</span><span class="sxs-lookup"><span data-stu-id="88112-181">Return to the *HelloWorldController.cs* file and change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewBag` object.</span></span> <span data-ttu-id="88112-182">`ViewBag` 是動態物件，這表示您可以放置任何內容給它;`ViewBag`物件有定義的屬性，直到您將其內的項目。</span><span class="sxs-lookup"><span data-stu-id="88112-182">`ViewBag` is a dynamic object, which means you can put whatever you want in to it; the `ViewBag` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="88112-183">[ASP.NET MVC 模型繫結系統](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)會自動將對應的具名的參數 (`name`和`numTimes`) 從 [網址] 列到您的方法中的參數中的查詢字串。</span><span class="sxs-lookup"><span data-stu-id="88112-183">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters (`name` and `numTimes`) from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="88112-184">完整的 *HelloWorldController.cs* 檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="88112-184">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

<span data-ttu-id="88112-185">現在`ViewBag`物件包含將會自動傳遞至檢視的資料。</span><span class="sxs-lookup"><span data-stu-id="88112-185">Now the `ViewBag` object contains data that will be passed to the view automatically.</span></span>

<span data-ttu-id="88112-186">接下來，您需要一個歡迎視圖模板！</span><span class="sxs-lookup"><span data-stu-id="88112-186">Next, you need a Welcome view template!</span></span> <span data-ttu-id="88112-187">在 **建置**功能表上，選取**建置 MvcMovie**藉此確定編譯專案。</span><span class="sxs-lookup"><span data-stu-id="88112-187">In the **Build** menu, select **Build MvcMovie** to make sure the project is compiled.</span></span>

<span data-ttu-id="88112-188">然後以滑鼠右鍵按一下`Welcome`方法，然後按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="88112-188">Then right-click inside the `Welcome` method and click **Add View**.</span></span>

![](adding-a-view/_static/image10.png)

<span data-ttu-id="88112-189">以下是什麼**加入檢視**對話方塊看起來像：</span><span class="sxs-lookup"><span data-stu-id="88112-189">Here's what the **Add View** dialog box looks like:</span></span>

![](adding-a-view/_static/image11.png)

<span data-ttu-id="88112-190">按一下 **新增**，然後新增下列程式碼底下`<h2>`在新的項目*Welcome.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="88112-190">Click **Add**, and then add the following code under the `<h2>` element in the new *Welcome.cshtml* file.</span></span> <span data-ttu-id="88112-191">您會建立迴圈，指出&quot;Hello&quot;使用者講的是預期的次數。</span><span class="sxs-lookup"><span data-stu-id="88112-191">You'll create a loop that says &quot;Hello&quot; as many times as the user says it should.</span></span> <span data-ttu-id="88112-192">完整*Welcome.cshtml*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="88112-192">The complete *Welcome.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

<span data-ttu-id="88112-193">執行應用程式，並瀏覽至下列 URL:</span><span class="sxs-lookup"><span data-stu-id="88112-193">Run the application and browse to the following URL:</span></span>

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

<span data-ttu-id="88112-194">資料是從 URL 取得並傳遞控制站使用現在[模型繫結器](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)。</span><span class="sxs-lookup"><span data-stu-id="88112-194">Now data is taken from the URL and passed to the controller using the [model binder](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx).</span></span> <span data-ttu-id="88112-195">控制器將資料封裝成`ViewBag`物件並檢視該物件傳遞。</span><span class="sxs-lookup"><span data-stu-id="88112-195">The controller packages the data into a `ViewBag` object and passes that object to the view.</span></span> <span data-ttu-id="88112-196">檢視則會顯示為 HTML 資料給使用者。</span><span class="sxs-lookup"><span data-stu-id="88112-196">The view then displays the data as HTML to the user.</span></span>

![](adding-a-view/_static/image12.png)

<span data-ttu-id="88112-197">在上述範例中，我們使用`ViewBag`將資料從控制器傳遞至檢視的物件。</span><span class="sxs-lookup"><span data-stu-id="88112-197">In the sample above, we used a `ViewBag` object to pass data from the controller to a view.</span></span> <span data-ttu-id="88112-198">第二個教學課程中，我們將檢視模型將資料從控制器傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="88112-198">Latter in the tutorial, we will use a view model to pass data from a controller to a view.</span></span> <span data-ttu-id="88112-199">若要將資料傳遞的檢視模型方法通常會更優先檢視包方法。</span><span class="sxs-lookup"><span data-stu-id="88112-199">The view model approach to passing data is generally much preferred over the view bag approach.</span></span> <span data-ttu-id="88112-200">請參閱部落格文章[動態 V 強型別檢視](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="88112-200">See the blog entry [Dynamic V Strongly Typed Views](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) for more information.</span></span>

<span data-ttu-id="88112-201">其實是一種的&quot;M&quot;模型，但不是資料庫類型。</span><span class="sxs-lookup"><span data-stu-id="88112-201">Well, that was a kind of an &quot;M&quot; for model, but not the database kind.</span></span> <span data-ttu-id="88112-202">讓我們運用所學的內容，建立電影的資料庫。</span><span class="sxs-lookup"><span data-stu-id="88112-202">Let's take what we've learned and create a database of movies.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="88112-203">[上一頁](adding-a-controller.md)
> [下一頁](adding-a-model.md)</span><span class="sxs-lookup"><span data-stu-id="88112-203">[Previous](adding-a-controller.md)
[Next](adding-a-model.md)</span></span>
