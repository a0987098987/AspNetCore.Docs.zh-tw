---
title: 將檢視新增至 MVC 應用程式
author: Rick-Anderson
description: 將檢視新增至 MVC 應用程式
ms.author: riande
ms.date: 01/23/2019
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: afa7584529566ebe82a0eb3849de89bd0df064bd
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837750"
---
<a name="adding-a-view"></a><span data-ttu-id="ef23d-103">新增檢視</span><span class="sxs-lookup"><span data-stu-id="ef23d-103">Adding a View</span></span>
====================
<span data-ttu-id="ef23d-104">藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="ef23d-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="ef23d-105">在本節中您要修改`HelloWorldController`類別，以使用檢視範本檔案，完全封裝產生 HTML 回應給用戶端的程序。</span><span class="sxs-lookup"><span data-stu-id="ef23d-105">In this section you're going to modify the `HelloWorldController` class to use view template files to cleanly encapsulate the process of generating HTML responses to a client.</span></span> 

<span data-ttu-id="ef23d-106">您將建立檢視範本檔案中使用[Razor 檢視引擎](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md)。</span><span class="sxs-lookup"><span data-stu-id="ef23d-106">You'll create a view template file using the [Razor view engine](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md).</span></span> <span data-ttu-id="ef23d-107">以 razor 為基礎的檢視範本具有 *.cshtml*副檔名，且提供優雅的方式來建立 HTML 輸出使用 C#。</span><span class="sxs-lookup"><span data-stu-id="ef23d-107">Razor-based view templates have a *.cshtml* file extension, and provide an elegant way to create HTML output using C#.</span></span> <span data-ttu-id="ef23d-108">Razor 字元和時撰寫檢視範本時，所需按鍵的數目降至最低，並可讓程式碼撰寫工作流程迅速、 流暢。</span><span class="sxs-lookup"><span data-stu-id="ef23d-108">Razor minimizes the number of characters and keystrokes required when writing a view template, and enables a fast, fluid coding workflow.</span></span>

<span data-ttu-id="ef23d-109">目前，`Index` 方法會傳回字串，內含在控制器類別中硬式編碼的訊息。</span><span class="sxs-lookup"><span data-stu-id="ef23d-109">Currently the `Index` method returns a string with a message that is hard-coded in the controller class.</span></span> <span data-ttu-id="ef23d-110">變更`Index`方法來呼叫控制器[檢視](/dotnet/api/microsoft.aspnetcore.mvc.controller.view#Microsoft_AspNetCore_Mvc_Controller_View)方法，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="ef23d-110">Change the `Index` method to call the controllers [View](/dotnet/api/microsoft.aspnetcore.mvc.controller.view#Microsoft_AspNetCore_Mvc_Controller_View) method, as shown in the following code:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

<span data-ttu-id="ef23d-111">`Index`上述方法來產生 HTML 回應給瀏覽器中使用檢視範本。</span><span class="sxs-lookup"><span data-stu-id="ef23d-111">The `Index` method above uses a view template to generate an HTML response to the browser.</span></span> <span data-ttu-id="ef23d-112">控制器方法 (也稱為[動作方法](http://rachelappel.com/asp.net-mvc-actionresults-explained))，例如`Index`上述，方法通常會傳回[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (或衍生自類別[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx))，不是基本類型與字串相同。</span><span class="sxs-lookup"><span data-stu-id="ef23d-112">Controller methods (also known as [action methods](http://rachelappel.com/asp.net-mvc-actionresults-explained)), such as the `Index` method above, generally return an [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (or a class derived from [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), not primitive types like string.</span></span>

<span data-ttu-id="ef23d-113">以滑鼠右鍵按一下*Views\HelloWorld*資料夾，然後按一下**新增**，然後按一下**MVC 5 檢視頁面 (Razor) 的版面配置與**。</span><span class="sxs-lookup"><span data-stu-id="ef23d-113">Right click the *Views\HelloWorld* folder and click **Add**, then click **MVC 5 View Page with Layout (Razor)**.</span></span>
  
![](adding-a-view/_static/image1.png)   
  
<span data-ttu-id="ef23d-114">在**指定項目的名稱**對話方塊方塊中，輸入*索引*，然後按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="ef23d-114">In the **Specify Name for Item** dialog box, enter *Index*, and then click **OK**.</span></span>  
  
![](adding-a-view/_static/image2.png)  
  
<span data-ttu-id="ef23d-115">在 **選取版面配置頁** 對話方塊中，接受預設值 **\_Layout.cshtml** ，按一下  **確定**。</span><span class="sxs-lookup"><span data-stu-id="ef23d-115">In the **Select a Layout Page** dialog, accept the default **\_Layout.cshtml** and click **OK**.</span></span>  
  
![](adding-a-view/_static/image3.png)  
  
<span data-ttu-id="ef23d-116">在對話方塊中， *Views\Shared*的左窗格中選取資料夾。</span><span class="sxs-lookup"><span data-stu-id="ef23d-116">In the dialog above, the *Views\Shared* folder is selected in the left pane.</span></span> <span data-ttu-id="ef23d-117">如果您有自訂的版面配置檔案，另一個資料夾中，您可以選取它。</span><span class="sxs-lookup"><span data-stu-id="ef23d-117">If you had a custom layout file in another folder, you could select it.</span></span> <span data-ttu-id="ef23d-118">我們將稍後在本教學課程中討論的配置檔案</span><span class="sxs-lookup"><span data-stu-id="ef23d-118">We'll talk about the layout file later in the tutorial</span></span>

<span data-ttu-id="ef23d-119">*MvcMovie\Views\HelloWorld\Index.cshtml*建立檔案。</span><span class="sxs-lookup"><span data-stu-id="ef23d-119">The *MvcMovie\Views\HelloWorld\Index.cshtml* file is created.</span></span>

![](adding-a-view/_static/image4.png)

<span data-ttu-id="ef23d-120">新增下列醒目提示的標記。</span><span class="sxs-lookup"><span data-stu-id="ef23d-120">Add the following highlighted markup.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

<span data-ttu-id="ef23d-121">以滑鼠右鍵按一下*Index.cshtml*檔案，然後選取**瀏覽器中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="ef23d-121">Right click the *Index.cshtml* file and select **View in Browser**.</span></span>

![PI](adding-a-view/_static/image5.png)

<span data-ttu-id="ef23d-123">您也可以以滑鼠右鍵按一下*Index.cshtml*檔案，然後選取**Page Inspector 中的檢視。**</span><span class="sxs-lookup"><span data-stu-id="ef23d-123">You can also right click the *Index.cshtml* file and select **View in Page Inspector.**</span></span> <span data-ttu-id="ef23d-124">請參閱[Page Inspector 教學課程](../../views/using-page-inspector-in-aspnet-mvc.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="ef23d-124">See the [Page Inspector tutorial](../../views/using-page-inspector-in-aspnet-mvc.md) for more information.</span></span>

<span data-ttu-id="ef23d-125">或者，執行應用程式，並瀏覽至`HelloWorld`控制站 (`http://localhost:xxxx/HelloWorld`)。</span><span class="sxs-lookup"><span data-stu-id="ef23d-125">Alternatively, run the application and browse to the `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`).</span></span> <span data-ttu-id="ef23d-126">`Index`在控制器方法並沒有進行太多工作; 它只會執行陳述式`return View()`，其所指定方法應使用檢視範本檔案來呈現至瀏覽器的回應。</span><span class="sxs-lookup"><span data-stu-id="ef23d-126">The `Index` method in your controller didn't do much work; it simply ran the statement `return View()`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="ef23d-127">因為您沒有明確指定要使用的檢視範本檔案的名稱，預設使用 ASP.NET MVC *Index.cshtml* 檢視中的檔案 *\Views\HelloWorld* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ef23d-127">Because you didn't explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the *Index.cshtml* view file in the *\Views\HelloWorld* folder.</span></span> <span data-ttu-id="ef23d-128">下圖顯示的字串&quot;Hello from our View Template ！&quot;硬式編碼在檢視中。</span><span class="sxs-lookup"><span data-stu-id="ef23d-128">The image below shows the string &quot;Hello from our View Template!&quot; hard-coded in the view.</span></span>

![](adding-a-view/_static/image6.png)

<span data-ttu-id="ef23d-129">看起來很好。</span><span class="sxs-lookup"><span data-stu-id="ef23d-129">Looks pretty good.</span></span> <span data-ttu-id="ef23d-130">不過，請注意，瀏覽器的標題列會顯示 索引-My ASP.NET 應用程式，「 巨量的連結，在頁面頂端顯示 "Application name"。</span><span class="sxs-lookup"><span data-stu-id="ef23d-130">However, notice that the browser's title bar shows "Index - My ASP.NET Application," and the big link at the top of the page says "Application name."</span></span> <span data-ttu-id="ef23d-131">根據如何小您進行瀏覽器視窗，您可能需要按一下以查看右上角的三個橫條來**首頁**，**有關**，**連絡人**， **註冊**並**登入**連結。</span><span class="sxs-lookup"><span data-stu-id="ef23d-131">Depending on how small you make your browser window, you might need to click the three bars in the upper right to see the to the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="ef23d-132">變更檢視和版面配置頁</span><span class="sxs-lookup"><span data-stu-id="ef23d-132">Changing Views and Layout Pages</span></span>

<span data-ttu-id="ef23d-133">首先，您想要變更的地方&quot;應用程式名稱&quot;在頁面頂端的連結。</span><span class="sxs-lookup"><span data-stu-id="ef23d-133">First, you want to change the &quot;Application name&quot; link at the top of the page.</span></span> <span data-ttu-id="ef23d-134">該文字會每個頁面。</span><span class="sxs-lookup"><span data-stu-id="ef23d-134">That text is common to every page.</span></span> <span data-ttu-id="ef23d-135">雖然看起來的應用程式中的每個頁面上，它實際上被實作在專案中，只有一個位置。</span><span class="sxs-lookup"><span data-stu-id="ef23d-135">It's actually implemented in only one place in the project, even though it appears on every page in the application.</span></span> <span data-ttu-id="ef23d-136">移至 */檢視/Shared*資料夾中的**方案總管**，然後開啟 *\_Layout.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="ef23d-136">Go to the */Views/Shared* folder in **Solution Explorer** and open the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="ef23d-137">這個檔案稱為*版面配置頁*而且它位於所有其他頁面使用的共用資料夾。</span><span class="sxs-lookup"><span data-stu-id="ef23d-137">This file is called a *layout page* and it's in the shared folder that all other pages use.</span></span>

![_LayoutCshtml](adding-a-view/_static/image7.png)

<span data-ttu-id="ef23d-139">版面配置範本可讓您在一個位置指定的 HTML 容器配置，您的網站，然後將它套用到您的網站中的多個頁面。</span><span class="sxs-lookup"><span data-stu-id="ef23d-139">Layout templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="ef23d-140">找到 `@RenderBody()` 這行。</span><span class="sxs-lookup"><span data-stu-id="ef23d-140">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="ef23d-141">`RenderBody` 是顯示您建立之所有檢視特定頁面的預留位置，「包裝」&quot;&quot;在版面配置頁中。</span><span class="sxs-lookup"><span data-stu-id="ef23d-141">`RenderBody` is a placeholder where all the view-specific pages you create show up, &quot;wrapped&quot; in the layout page.</span></span> <span data-ttu-id="ef23d-142">比方說，如果您選取**關於**連結*Views\Home\About.cshtml*內呈現檢視`RenderBody`方法。</span><span class="sxs-lookup"><span data-stu-id="ef23d-142">For example, if you select the **About** link, the *Views\Home\About.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<span data-ttu-id="ef23d-143">變更標題元素的內容。</span><span class="sxs-lookup"><span data-stu-id="ef23d-143">Change the contents of the title element.</span></span> <span data-ttu-id="ef23d-144">變更[ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx)從 [配置] 範本中&quot;應用程式名稱&quot;來&quot;MVC Movie&quot;並將控制器從`Home`至`Movies`。</span><span class="sxs-lookup"><span data-stu-id="ef23d-144">Change the [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) in the layout template from &quot;Application name&quot; to &quot;MVC Movie&quot; and the controller from `Home` to `Movies`.</span></span> <span data-ttu-id="ef23d-145">完整的版面配置檔如下所示：</span><span class="sxs-lookup"><span data-stu-id="ef23d-145">The complete layout file is shown below:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

<span data-ttu-id="ef23d-146">執行應用程式和通知，現在指出&quot;MVC Movie &quot;。</span><span class="sxs-lookup"><span data-stu-id="ef23d-146">Run the application and notice that it now says &quot;MVC Movie &quot;.</span></span> <span data-ttu-id="ef23d-147">按一下 **關於**連結，您會看到該頁面會顯示如何&quot;MVC Movie&quot;也。</span><span class="sxs-lookup"><span data-stu-id="ef23d-147">Click the **About** link, and you see how that page shows &quot;MVC Movie&quot;, too.</span></span> <span data-ttu-id="ef23d-148">我們能夠在版面配置範本一次進行變更，並已在站台上的所有頁面都反映新的標題。</span><span class="sxs-lookup"><span data-stu-id="ef23d-148">We were able to make the change once in the layout template and have all pages on the site reflect the new title.</span></span>

![](adding-a-view/_static/image8.png)

<span data-ttu-id="ef23d-149">當我們第一次建立*Views\HelloWorld\Index.cshtml*檔案，它包含下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ef23d-149">When we first created the *Views\HelloWorld\Index.cshtml* file, it contained the following code:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

<span data-ttu-id="ef23d-150">上面的 Razor 程式碼會明確設定 [配置] 頁面。</span><span class="sxs-lookup"><span data-stu-id="ef23d-150">The Razor code above is explicitly setting the layout page.</span></span> <span data-ttu-id="ef23d-151">檢查*檢視\\_ViewStart.cshtml*檔案，其中包含完全相同的 Razor 標記。</span><span class="sxs-lookup"><span data-stu-id="ef23d-151">Examine the *Views\\_ViewStart.cshtml* file, it contains the exact same Razor markup.</span></span> <span data-ttu-id="ef23d-152">*[檢視\\_ViewStart.cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* 檔案會定義所有檢視都會都使用通用配置，因此您可以在其中註 out 或移除該程式碼從*Views\HelloWorld\Index.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="ef23d-152">The *[Views\\_ViewStart.cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* file defines the common layout that all views will use, therefore you can comment out or remove that code from the *Views\HelloWorld\Index.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

<span data-ttu-id="ef23d-153">您可以使用 `Layout` 屬性來設定不同的版面配置檢視，或將它設定為 `null`，因此不會使用任何版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="ef23d-153">You can use the `Layout` property to set a different layout view, or set it to `null` so no layout file will be used.</span></span>

<span data-ttu-id="ef23d-154">現在，讓我們變更索引 檢視的標題。</span><span class="sxs-lookup"><span data-stu-id="ef23d-154">Now, let's change the title of the Index view.</span></span>

<span data-ttu-id="ef23d-155">開啟*MvcMovie\Views\HelloWorld\Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="ef23d-155">Open *MvcMovie\Views\HelloWorld\Index.cshtml*.</span></span> <span data-ttu-id="ef23d-156">有兩個地方進行變更： 首先，文字，會出現在瀏覽器的標題，然後次要標頭 (`<h2>`項目)。</span><span class="sxs-lookup"><span data-stu-id="ef23d-156">There are two places to make a change: first, the text that appears in the title of the browser, and then in the secondary header (the `<h2>` element).</span></span> <span data-ttu-id="ef23d-157">您會使這些變更略有不同，因此可看出哪一段程式碼變更了應用程式的哪個部分。</span><span class="sxs-lookup"><span data-stu-id="ef23d-157">You'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

<span data-ttu-id="ef23d-158">若要表示顯示，上述程式碼的 HTML 標題`Title`的屬性`ViewBag`物件 (這是在*Index.cshtml*檢視範本)。</span><span class="sxs-lookup"><span data-stu-id="ef23d-158">To indicate the HTML title to display, the code above sets a `Title` property of the `ViewBag` object (which is in the *Index.cshtml* view template).</span></span> <span data-ttu-id="ef23d-159">請注意，版面配置範本 ( *Views\Shared\\_Layout.cshtml* ) 會使用此值`<title>`一部分的項目`<head>`我們先前修改 HTML 一節。</span><span class="sxs-lookup"><span data-stu-id="ef23d-159">Notice that the layout template ( *Views\Shared\\_Layout.cshtml* ) uses this value in the `<title>` element as part of the `<head>` section of the HTML that we modified previously.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

<span data-ttu-id="ef23d-160">使用此`ViewBag`方法時，您可以輕鬆地傳遞其他參數之間檢視範本和配置檔案。</span><span class="sxs-lookup"><span data-stu-id="ef23d-160">Using this `ViewBag` approach, you can easily pass other parameters between your view template and your layout file.</span></span>

<span data-ttu-id="ef23d-161">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="ef23d-161">Run the application.</span></span> <span data-ttu-id="ef23d-162">請注意，瀏覽器標題、主要標題和次要標題已變更</span><span class="sxs-lookup"><span data-stu-id="ef23d-162">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="ef23d-163">(如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。</span><span class="sxs-lookup"><span data-stu-id="ef23d-163">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="ef23d-164">請在瀏覽器中按 Ctrl + F5 以強制載入來自伺服器的回應)。瀏覽器標題會透過`ViewBag.Title`我們在中設定*Index.cshtml*檢視範本和其他&quot;-Movie App&quot;配置檔案中新增。</span><span class="sxs-lookup"><span data-stu-id="ef23d-164">Press Ctrl+F5 in your browser to force the response from the server to be loaded.) The browser title is created with the `ViewBag.Title` we set in the *Index.cshtml* view template and the additional &quot;- Movie App&quot; added in the layout file.</span></span>

<span data-ttu-id="ef23d-165">另外也請注意如何在內容*Index.cshtml*檢視範本與合併 *\_Layout.cshtml*檢視範本和單一 HTML 回應已傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="ef23d-165">Also notice how the content in the *Index.cshtml* view template was merged with the *\_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="ef23d-166">版面配置範本可讓您輕鬆進行會套用到應用程式之所有頁面的變更。</span><span class="sxs-lookup"><span data-stu-id="ef23d-166">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span>

![](adding-a-view/_static/image9.png)

<span data-ttu-id="ef23d-167">我們一點&quot;資料&quot;(在此情況下&quot;Hello from our View Template ！&quot;訊息) 是硬式編碼，不過。</span><span class="sxs-lookup"><span data-stu-id="ef23d-167">Our little bit of &quot;data&quot; (in this case the &quot;Hello from our View Template!&quot; message) is hard-coded, though.</span></span> <span data-ttu-id="ef23d-168">MVC 應用程式具有&quot;V&quot; （檢視），您可盡情&quot;C&quot; （控制器），但沒有&quot;M&quot; （模型） 尚未。</span><span class="sxs-lookup"><span data-stu-id="ef23d-168">The MVC application has a &quot;V&quot; (view) and you've got a &quot;C&quot; (controller), but no &quot;M&quot; (model) yet.</span></span> <span data-ttu-id="ef23d-169">不久之後，我們將逐步解說如何建立資料庫，並從其擷取模型資料。</span><span class="sxs-lookup"><span data-stu-id="ef23d-169">Shortly, we'll walk through how to create a database and retrieve model data from it.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="ef23d-170">將資料從控制器傳遞至檢視</span><span class="sxs-lookup"><span data-stu-id="ef23d-170">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="ef23d-171">我們移至資料庫，並討論模型之前，不過，讓我們先討論將資訊從控制器傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="ef23d-171">Before we go to a database and talk about models, though, let's first talk about passing information from the controller to a view.</span></span> <span data-ttu-id="ef23d-172">控制器類別會叫用，以回應傳入的 URL 要求。</span><span class="sxs-lookup"><span data-stu-id="ef23d-172">Controller classes are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="ef23d-173">控制器類別是回應的您可在此撰寫處理傳入的瀏覽器的程式碼要求、 從資料庫擷取資料和最終決定哪一種將傳回給瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="ef23d-173">A controller class is where you write the code that handles the incoming browser requests, retrieves data from a database, and ultimately decides what type of response to send back to the browser.</span></span> <span data-ttu-id="ef23d-174">然後從控制器來產生並格式化瀏覽器的 HTML 回應使用檢視範本。</span><span class="sxs-lookup"><span data-stu-id="ef23d-174">View templates can then be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="ef23d-175">控制器是負責提供任何資料或物件所需為了讓檢視範本來呈現至瀏覽器的回應。</span><span class="sxs-lookup"><span data-stu-id="ef23d-175">Controllers are responsible for providing whatever data or objects are required in order for a view template to render a response to the browser.</span></span> <span data-ttu-id="ef23d-176">最佳做法：**檢視範本應該永遠不會執行商務邏輯或直接與資料庫互動**。</span><span class="sxs-lookup"><span data-stu-id="ef23d-176">A best practice: **A view template should never perform business logic or interact with a database directly**.</span></span> <span data-ttu-id="ef23d-177">相反地，檢視範本應該只使用由控制器提供給它的資料。</span><span class="sxs-lookup"><span data-stu-id="ef23d-177">Instead, a view template should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="ef23d-178">維護這&quot;關注點分離&quot;精簡、 可測試且更易於維護，協助保持您的程式碼。</span><span class="sxs-lookup"><span data-stu-id="ef23d-178">Maintaining this &quot;separation of concerns&quot; helps keep your code clean, testable and more maintainable.</span></span>

<span data-ttu-id="ef23d-179">目前，`Welcome`中的動作方法`HelloWorldController`類別會採用`name`和`numTimes`參數，然後輸出到瀏覽器直接的值。</span><span class="sxs-lookup"><span data-stu-id="ef23d-179">Currently, the `Welcome` action method in the `HelloWorldController` class takes a `name` and a `numTimes` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="ef23d-180">與其讓控制器呈現這個回應，做為字串，讓我們變更控制器以改為使用檢視範本。</span><span class="sxs-lookup"><span data-stu-id="ef23d-180">Rather than have the controller render this response as a string, let's change the controller to use a view template instead.</span></span> <span data-ttu-id="ef23d-181">檢視範本會產生動態回應，這表示您需要將適當數量的資料從控制器傳遞至檢視，以便產生回應。</span><span class="sxs-lookup"><span data-stu-id="ef23d-181">The view template will generate a dynamic response, which means that you need to pass appropriate bits of data from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="ef23d-182">您可以透過讓控制器將檢視範本需要的動態資料 （參數） 放置`ViewBag`檢視範本之後可以存取的物件。</span><span class="sxs-lookup"><span data-stu-id="ef23d-182">You can do this by having the controller put the dynamic data (parameters) that the view template needs in a `ViewBag` object that the view template can then access.</span></span>

<span data-ttu-id="ef23d-183">返回*HelloWorldController.cs*檔案，並變更`Welcome`方法來加入`Message`並`NumTimes`值`ViewBag`物件。</span><span class="sxs-lookup"><span data-stu-id="ef23d-183">Return to the *HelloWorldController.cs* file and change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewBag` object.</span></span> <span data-ttu-id="ef23d-184">`ViewBag` 是動態物件，這表示您可以放置任何內容給它;`ViewBag`物件有定義的屬性，直到您將其內的項目。</span><span class="sxs-lookup"><span data-stu-id="ef23d-184">`ViewBag` is a dynamic object, which means you can put whatever you want in to it; the `ViewBag` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="ef23d-185">[ASP.NET MVC 模型繫結系統](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)會自動將對應的具名的參數 (`name`和`numTimes`) 從 [網址] 列到您的方法中的參數中的查詢字串。</span><span class="sxs-lookup"><span data-stu-id="ef23d-185">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters (`name` and `numTimes`) from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="ef23d-186">完整的 *HelloWorldController.cs* 檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="ef23d-186">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

<span data-ttu-id="ef23d-187">現在`ViewBag`物件包含將會自動傳遞至檢視的資料。</span><span class="sxs-lookup"><span data-stu-id="ef23d-187">Now the `ViewBag` object contains data that will be passed to the view automatically.</span></span> <span data-ttu-id="ef23d-188">接下來，您需要一個歡迎視圖模板！</span><span class="sxs-lookup"><span data-stu-id="ef23d-188">Next, you need a Welcome view template!</span></span> <span data-ttu-id="ef23d-189">在**建置**功能表上，選取**建置方案**（或 Ctrl + Shift + B） 若要確定專案會編譯。</span><span class="sxs-lookup"><span data-stu-id="ef23d-189">In the **Build** menu, select **Build Solution** (or Ctrl+Shift+B) to make sure the project is compiled.</span></span> <span data-ttu-id="ef23d-190">以滑鼠右鍵按一下*Views\HelloWorld*資料夾，然後按一下**新增**，然後按一下**MVC 5 檢視頁面 (Razor) 的版面配置與**。</span><span class="sxs-lookup"><span data-stu-id="ef23d-190">Right click the *Views\HelloWorld* folder and click **Add**, then click **MVC 5 View Page with Layout (Razor)**.</span></span>
  
![](adding-a-view/_static/image10.png)   
  
<span data-ttu-id="ef23d-191">在**指定項目的名稱**對話方塊方塊中，輸入*褖畫惎*，然後按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="ef23d-191">In the **Specify Name for Item** dialog box, enter *Welcome*, and then click **OK**.</span></span>   
  
<span data-ttu-id="ef23d-192">在 **選取版面配置頁** 對話方塊中，接受預設值 **\_Layout.cshtml** ，按一下  **確定**。</span><span class="sxs-lookup"><span data-stu-id="ef23d-192">In the **Select a Layout Page** dialog, accept the default **\_Layout.cshtml** and click **OK**.</span></span>  
  
![](adding-a-view/_static/image11.png)   

<span data-ttu-id="ef23d-193">*MvcMovie\Views\HelloWorld\Welcome.cshtml*建立檔案。</span><span class="sxs-lookup"><span data-stu-id="ef23d-193">The *MvcMovie\Views\HelloWorld\Welcome.cshtml* file is created.</span></span>

<span data-ttu-id="ef23d-194">取代中的標記*Welcome.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="ef23d-194">Replace the markup in the *Welcome.cshtml* file.</span></span> <span data-ttu-id="ef23d-195">您會建立迴圈，指出&quot;Hello&quot;使用者講的是預期的次數。</span><span class="sxs-lookup"><span data-stu-id="ef23d-195">You'll create a loop that says &quot;Hello&quot; as many times as the user says it should.</span></span> <span data-ttu-id="ef23d-196">完整*Welcome.cshtml*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="ef23d-196">The complete *Welcome.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

<span data-ttu-id="ef23d-197">執行應用程式，並瀏覽至下列 URL:</span><span class="sxs-lookup"><span data-stu-id="ef23d-197">Run the application and browse to the following URL:</span></span>

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

<span data-ttu-id="ef23d-198">資料是從 URL 取得並傳遞控制站使用現在[模型繫結器](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)。</span><span class="sxs-lookup"><span data-stu-id="ef23d-198">Now data is taken from the URL and passed to the controller using the [model binder](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx).</span></span> <span data-ttu-id="ef23d-199">控制器將資料封裝成`ViewBag`物件並檢視該物件傳遞。</span><span class="sxs-lookup"><span data-stu-id="ef23d-199">The controller packages the data into a `ViewBag` object and passes that object to the view.</span></span> <span data-ttu-id="ef23d-200">檢視則會顯示為 HTML 資料給使用者。</span><span class="sxs-lookup"><span data-stu-id="ef23d-200">The view then displays the data as HTML to the user.</span></span>

![](adding-a-view/_static/image12.png)

<span data-ttu-id="ef23d-201">在上述範例中，我們使用`ViewBag`將資料從控制器傳遞至檢視的物件。</span><span class="sxs-lookup"><span data-stu-id="ef23d-201">In the sample above, we used a `ViewBag` object to pass data from the controller to a view.</span></span> <span data-ttu-id="ef23d-202">稍後在教學課程中，我們將使用檢視模型將控制器中的資料傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="ef23d-202">Later in the tutorial, we will use a view model to pass data from a controller to a view.</span></span> <span data-ttu-id="ef23d-203">若要將資料傳遞的檢視模型方法通常會更優先檢視包方法。</span><span class="sxs-lookup"><span data-stu-id="ef23d-203">The view model approach to passing data is generally much preferred over the view bag approach.</span></span> <span data-ttu-id="ef23d-204">請參閱部落格文章[動態 V 強型別檢視](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="ef23d-204">See the blog entry [Dynamic V Strongly Typed Views](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) for more information.</span></span> 

<span data-ttu-id="ef23d-205">其實是一種的&quot;M&quot;模型，但不是資料庫類型。</span><span class="sxs-lookup"><span data-stu-id="ef23d-205">Well, that was a kind of an &quot;M&quot; for model, but not the database kind.</span></span> <span data-ttu-id="ef23d-206">讓我們運用所學的內容，建立電影的資料庫。</span><span class="sxs-lookup"><span data-stu-id="ef23d-206">Let's take what we've learned and create a database of movies.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ef23d-207">[上一頁](adding-a-controller.md)
> [下一頁](adding-a-model.md)</span><span class="sxs-lookup"><span data-stu-id="ef23d-207">[Previous](adding-a-controller.md)
[Next](adding-a-model.md)</span></span>
