---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: "ASP.NET MVC 檢視概觀 (VB) |Microsoft 文件"
author: StephenWalther
description: "什麼是 ASP.NET MVC 檢視，以及如何不同的 HTML 網頁？ 在本教學課程中，作者： Stephen Walther 為您介紹檢視，並示範如何 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: c85b969aa4457d0326b4a16da218db9e11d01e10
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-views-overview-vb"></a><span data-ttu-id="5e585-104">ASP.NET MVC 檢視概觀 (VB)</span><span class="sxs-lookup"><span data-stu-id="5e585-104">ASP.NET MVC Views Overview (VB)</span></span>
====================
<span data-ttu-id="5e585-105">由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="5e585-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="5e585-106">什麼是 ASP.NET MVC 檢視，以及如何不同的 HTML 網頁？</span><span class="sxs-lookup"><span data-stu-id="5e585-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="5e585-107">在本教學課程中，作者： Stephen Walther 為您介紹檢視，並示範如何使用的檢視資料和檢視表中的 HTML Helper 利用。</span><span class="sxs-lookup"><span data-stu-id="5e585-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="5e585-108">本教學課程的目的是為您提供簡介 ASP.NET MVC 檢視、 檢視資料和 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="5e585-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="5e585-109">本教學課程結束時，您應該了解如何建立新的檢視、 傳遞資料從控制器到檢視，以及用來產生內容檢視中的 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="5e585-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="5e585-110">了解檢視</span><span class="sxs-lookup"><span data-stu-id="5e585-110">Understanding Views</span></span>

<span data-ttu-id="5e585-111">不同於 ASP.NET 或動態伺服器網頁，ASP.NET MVC 不包含任何項目會直接對應至頁面。</span><span class="sxs-lookup"><span data-stu-id="5e585-111">Unlike ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="5e585-112">ASP.NET MVC 應用程式中，沒有任何頁面對應至您在您的瀏覽器的網址列輸入的 url 路徑的磁碟上。</span><span class="sxs-lookup"><span data-stu-id="5e585-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="5e585-113">在 ASP.NET MVC 應用程式頁面的最接近動作是呼叫*檢視*。</span><span class="sxs-lookup"><span data-stu-id="5e585-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="5e585-114">ASP.NET MVC 應用程式中，連入的瀏覽器要求會對應至控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="5e585-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="5e585-115">控制器動作，可能會傳回檢視表。</span><span class="sxs-lookup"><span data-stu-id="5e585-115">A controller action might return a view.</span></span> <span data-ttu-id="5e585-116">不過，控制器動作，可能會執行其他類型的動作，例如將您重新導向至另一個控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="5e585-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="5e585-117">清單 1 包含名為 HomeController 簡單控制器。</span><span class="sxs-lookup"><span data-stu-id="5e585-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="5e585-118">HomeController 公開 （expose) 名為 index （） 和 Details() 兩個控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="5e585-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="5e585-119">**列出 1-HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="5e585-119">**Listing 1 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

<span data-ttu-id="5e585-120">您可以呼叫的第一個動作，index （） 的動作，您的瀏覽器網址列中輸入下列 URL:</span><span class="sxs-lookup"><span data-stu-id="5e585-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="5e585-121">/ Home/Index</span><span class="sxs-lookup"><span data-stu-id="5e585-121">/Home/Index</span></span>

<span data-ttu-id="5e585-122">您可以在您的瀏覽器中輸入此位址，叫用第二個動作，Details() 動作：</span><span class="sxs-lookup"><span data-stu-id="5e585-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="5e585-123">/ 主/詳細資料</span><span class="sxs-lookup"><span data-stu-id="5e585-123">/Home/Details</span></span>

<span data-ttu-id="5e585-124">Index 動作傳回的檢視。</span><span class="sxs-lookup"><span data-stu-id="5e585-124">The Index() action returns a view.</span></span> <span data-ttu-id="5e585-125">您所建立的大部分動作將會傳回檢視表。</span><span class="sxs-lookup"><span data-stu-id="5e585-125">Most actions that you create will return views.</span></span> <span data-ttu-id="5e585-126">但是，動作可以傳回其他類型的動作結果。</span><span class="sxs-lookup"><span data-stu-id="5e585-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="5e585-127">例如，Details() 動作將會傳回傳入的要求重新導向至 index 動作 RedirectToActionResult。</span><span class="sxs-lookup"><span data-stu-id="5e585-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="5e585-128">Index 動作包含下列一行程式碼：</span><span class="sxs-lookup"><span data-stu-id="5e585-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="5e585-129">View()</span><span class="sxs-lookup"><span data-stu-id="5e585-129">View()</span></span>

<span data-ttu-id="5e585-130">這行程式碼會傳回必須位於下列路徑在您的網頁伺服器上的檢視：</span><span class="sxs-lookup"><span data-stu-id="5e585-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="5e585-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="5e585-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="5e585-132">檢視的路徑是從控制器名稱和控制器動作的名稱來推斷。</span><span class="sxs-lookup"><span data-stu-id="5e585-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="5e585-133">如果您想要的話，您可以明確了解檢視。</span><span class="sxs-lookup"><span data-stu-id="5e585-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="5e585-134">下列程式碼會傳回名為 Fred 的檢視：</span><span class="sxs-lookup"><span data-stu-id="5e585-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="5e585-135">檢視 (Fred)</span><span class="sxs-lookup"><span data-stu-id="5e585-135">View( Fred )</span></span>

<span data-ttu-id="5e585-136">執行這行程式碼時，檢視會傳回從下列路徑：</span><span class="sxs-lookup"><span data-stu-id="5e585-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="5e585-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="5e585-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5e585-138">如果您打算建立 ASP.NET MVC 應用程式的單元測試它是個不錯的主意明確了解檢視名稱。</span><span class="sxs-lookup"><span data-stu-id="5e585-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="5e585-139">這樣一來，您可以建立單元測試以確認控制器動作未傳回預期的檢視。</span><span class="sxs-lookup"><span data-stu-id="5e585-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="5e585-140">將內容加入至檢視</span><span class="sxs-lookup"><span data-stu-id="5e585-140">Adding Content to a View</span></span>

<span data-ttu-id="5e585-141">檢視是一種標準 (X) 可以包含指令碼的 HTML 文件。</span><span class="sxs-lookup"><span data-stu-id="5e585-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="5e585-142">您可以使用指令碼新增至檢視動態內容。</span><span class="sxs-lookup"><span data-stu-id="5e585-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="5e585-143">例如，列表 2 中的檢視會顯示目前的日期和時間。</span><span class="sxs-lookup"><span data-stu-id="5e585-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="5e585-144">**列出 2-\Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="5e585-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

<span data-ttu-id="5e585-145">請注意，清單 2 中的 HTML 網頁的主體會包含下列指令碼：</span><span class="sxs-lookup"><span data-stu-id="5e585-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="5e585-146">&lt;%Response.write(datetime.now)%&gt;</span><span class="sxs-lookup"><span data-stu-id="5e585-146">&lt;% Response.Write(DateTime.Now)%&gt;</span></span>

<span data-ttu-id="5e585-147">使用指令碼分隔符號&lt;%和 %&gt;標記開頭和結尾的指令碼。</span><span class="sxs-lookup"><span data-stu-id="5e585-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="5e585-148">此指令碼是以 Visual basic 撰寫的。</span><span class="sxs-lookup"><span data-stu-id="5e585-148">This script is written in Visual basic.</span></span> <span data-ttu-id="5e585-149">它會顯示目前的日期和時間，藉由呼叫 Response.write 方法轉譯至瀏覽器的內容。</span><span class="sxs-lookup"><span data-stu-id="5e585-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="5e585-150">指令碼分隔符號&lt;%和 %&gt;可用來執行一或多個陳述式。</span><span class="sxs-lookup"><span data-stu-id="5e585-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="5e585-151">因為您經常呼叫 Response.write，Microsoft 提供您與快速鍵呼叫 Response.write 方法。</span><span class="sxs-lookup"><span data-stu-id="5e585-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="5e585-152">範例 3 中的檢視使用分隔符號&lt;%= 和 %&gt;快捷 Response.write 函式呼叫。</span><span class="sxs-lookup"><span data-stu-id="5e585-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="5e585-153">**列出 3-Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="5e585-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

<span data-ttu-id="5e585-154">您可以使用任何.NET 語言來產生動態內容檢視中。</span><span class="sxs-lookup"><span data-stu-id="5e585-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="5e585-155">一般來說，就會使用 Visual Basic.NET 或 C# 來撰寫您的控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="5e585-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="5e585-156">使用 HTML Helper 來產生檢視內容</span><span class="sxs-lookup"><span data-stu-id="5e585-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="5e585-157">若要讓您更輕鬆地將內容加入檢視，您可以利用所謂*HTML Helper*。</span><span class="sxs-lookup"><span data-stu-id="5e585-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="5e585-158">HTML Helper，通常是產生的字串的方法。</span><span class="sxs-lookup"><span data-stu-id="5e585-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="5e585-159">您可以使用 HTML Helper 來產生標準 HTML 項目，例如文字方塊、 連結、 下拉式清單和清單方塊。</span><span class="sxs-lookup"><span data-stu-id="5e585-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="5e585-160">例如，在三個 HTML Helper-列出 4 利用檢視 BeginForm()、 TextBox() 和 Password() helper-產生登入表單 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="5e585-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="5e585-161">**列出 4-\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="5e585-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]


<span data-ttu-id="5e585-162">[![新增專案 對話方塊](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5e585-162">[![The New Project dialog box](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="5e585-163">**圖 01**： 標準登入表單 ([按一下以檢視完整大小的影像](asp-net-mvc-views-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="5e585-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="5e585-164">所有 HTML Helper 方法會呼叫檢視的 Html 屬性。</span><span class="sxs-lookup"><span data-stu-id="5e585-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="5e585-165">比方說，您會藉由呼叫 Html.TextBox() 方法呈現文字方塊。</span><span class="sxs-lookup"><span data-stu-id="5e585-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="5e585-166">請注意，您會使用指令碼分隔符號&lt;%= 和 %&gt;呼叫 Html.TextBox() 和 Html.Password() helper 時。</span><span class="sxs-lookup"><span data-stu-id="5e585-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="5e585-167">這些 helper 就只會傳回字串。</span><span class="sxs-lookup"><span data-stu-id="5e585-167">These helpers simply return a string.</span></span> <span data-ttu-id="5e585-168">您需要呼叫 Response.write 才能轉譯至瀏覽器的字串。</span><span class="sxs-lookup"><span data-stu-id="5e585-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="5e585-169">使用 HTML Helper 方法是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="5e585-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="5e585-170">它們會簡化您的生活減少的 HTML 和需要撰寫的指令碼。</span><span class="sxs-lookup"><span data-stu-id="5e585-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="5e585-171">列出 5 中的檢視而不需使用 HTML Helper 呈現完全相同的形式中列出的 4 的檢視。</span><span class="sxs-lookup"><span data-stu-id="5e585-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="5e585-172">**列出 5-\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="5e585-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

<span data-ttu-id="5e585-173">您也可以選擇建立您自己的 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="5e585-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="5e585-174">例如，您可以建立自動 HTML 表格中顯示的一組資料庫記錄的 GridView() helper 方法。</span><span class="sxs-lookup"><span data-stu-id="5e585-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="5e585-175">我們在本教學課程中瀏覽本主題**建立自訂 HTML Helper**。</span><span class="sxs-lookup"><span data-stu-id="5e585-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="5e585-176">將資料傳遞至檢視中使用檢視資料</span><span class="sxs-lookup"><span data-stu-id="5e585-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="5e585-177">您可以使用檢視資料將從控制器的資料傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="5e585-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="5e585-178">將檢視資料，例如您透過郵件傳送的封裝。</span><span class="sxs-lookup"><span data-stu-id="5e585-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="5e585-179">必須使用這個套件傳送傳遞從控制器到檢視的所有資料。</span><span class="sxs-lookup"><span data-stu-id="5e585-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="5e585-180">例如，程式碼範例 6 中的控制站新增一則訊息檢視資料。</span><span class="sxs-lookup"><span data-stu-id="5e585-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="5e585-181">**列出 6-ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="5e585-181">**Listing 6 - ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

<span data-ttu-id="5e585-182">控制器別的 ViewData 屬性代表名稱 / 值組的集合。</span><span class="sxs-lookup"><span data-stu-id="5e585-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="5e585-183">列出第 6 index （） 方法會將項目加入至檢視資料收集，名為訊息的值為 Hello World ！。</span><span class="sxs-lookup"><span data-stu-id="5e585-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="5e585-184">檢視 index （） 方法傳回時，檢視資料會自動傳遞到檢視。</span><span class="sxs-lookup"><span data-stu-id="5e585-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="5e585-185">列出 7 的檢視從檢視資料擷取訊息，並呈現至瀏覽器的訊息。</span><span class="sxs-lookup"><span data-stu-id="5e585-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="5e585-186">**列出 7-\Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="5e585-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

<span data-ttu-id="5e585-187">請注意，檢視利用 Html.Encode() HTML Helper 方法轉譯訊息時。</span><span class="sxs-lookup"><span data-stu-id="5e585-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="5e585-188">Html.Encode() HTML Helper 的特殊字元編碼例如&lt;和&gt;成安全的網頁中顯示的字元。</span><span class="sxs-lookup"><span data-stu-id="5e585-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="5e585-189">每當您轉譯使用者提交到網站的內容，您應該編碼內容，以防止 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="5e585-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="5e585-190">(因為我們訊息自行建立 ProductController 中，我們不需要對訊息進行編碼。</span><span class="sxs-lookup"><span data-stu-id="5e585-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="5e585-191">不過，它是個好習慣以顯示內容從檢視表中的檢視資料擷取時請務必呼叫 Html.Encode() 方法）。</span><span class="sxs-lookup"><span data-stu-id="5e585-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="5e585-192">在列出 7 中，我們利用檢視来傳遞的資料與簡單的字串訊息從控制器到檢視。</span><span class="sxs-lookup"><span data-stu-id="5e585-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="5e585-193">您也可以使用檢視資料，將其他類型的資料，例如資料庫記錄，從控制器到檢視的集合。</span><span class="sxs-lookup"><span data-stu-id="5e585-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="5e585-194">例如，如果您想要在檢視中，顯示產品資料庫資料表的內容則將集合資料庫的記錄檢視中資料。</span><span class="sxs-lookup"><span data-stu-id="5e585-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="5e585-195">您也可以選擇從控制器的強型別的檢視資料傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="5e585-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="5e585-196">我們在本教學課程中瀏覽本主題**了解強型別檢視資料和檢視表**。</span><span class="sxs-lookup"><span data-stu-id="5e585-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="5e585-197">總結</span><span class="sxs-lookup"><span data-stu-id="5e585-197">Summary</span></span>

<span data-ttu-id="5e585-198">本教學課程提供簡介 ASP.NET MVC 檢視、 檢視資料和 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="5e585-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="5e585-199">在第一個區段中，您將學會如何將新的檢視加入至您的專案。</span><span class="sxs-lookup"><span data-stu-id="5e585-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="5e585-200">您已了解，您必須加入檢視到正確的資料夾才能呼叫從特定的控制站。</span><span class="sxs-lookup"><span data-stu-id="5e585-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="5e585-201">接下來，我們將討論主題的 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="5e585-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="5e585-202">您已學習如何 HTML Helper 可讓您輕鬆地產生標準 HTML 內容。</span><span class="sxs-lookup"><span data-stu-id="5e585-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="5e585-203">最後，您會學到如何利用將從控制器的資料傳遞至檢視的檢視資料。</span><span class="sxs-lookup"><span data-stu-id="5e585-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5e585-204">[上一頁](passing-data-to-view-master-pages-cs.md)
[下一頁](creating-custom-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5e585-204">[Previous](passing-data-to-view-master-pages-cs.md)
[Next](creating-custom-html-helpers-vb.md)</span></span>
