---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: ASP.NET MVC 檢視概觀 (C#) |Microsoft Docs
author: StephenWalther
description: 什麼是 ASP.NET MVC 檢視，它有何不同的 HTML 網頁？ 在本教學課程中，Stephen Walther 會為您介紹檢視，並向您示範如何 t...
ms.author: aspnetcontent
ms.date: 02/16/2008
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: d2fc96f7e991dd7c4e0b3e9ff5c589c1075010ac
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833655"
---
<a name="aspnet-mvc-views-overview-c"></a><span data-ttu-id="86d23-104">ASP.NET MVC 檢視概觀 (C#)</span><span class="sxs-lookup"><span data-stu-id="86d23-104">ASP.NET MVC Views Overview (C#)</span></span>
====================
<span data-ttu-id="86d23-105">藉由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="86d23-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="86d23-106">什麼是 ASP.NET MVC 檢視，它有何不同的 HTML 網頁？</span><span class="sxs-lookup"><span data-stu-id="86d23-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="86d23-107">在本教學課程中，Stephen Walther 會介紹檢視，並示範如何使用檢視資料和檢視表中的 HTML Helper 的利用。</span><span class="sxs-lookup"><span data-stu-id="86d23-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="86d23-108">本教學課程的目的是為您提供 ASP.NET MVC 檢視、 檢視資料和 HTML 協助程式簡介。</span><span class="sxs-lookup"><span data-stu-id="86d23-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="86d23-109">本教學課程結束時，您應該了解如何建立新的檢視，將資料從控制器傳遞至檢視，並使用 HTML 協助程式產生內容檢視中。</span><span class="sxs-lookup"><span data-stu-id="86d23-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="86d23-110">了解檢視</span><span class="sxs-lookup"><span data-stu-id="86d23-110">Understanding Views</span></span>

<span data-ttu-id="86d23-111">針對 ASP.NET 或動態伺服器網頁，ASP.NET MVC 不包含任何直接對應至頁面的項目。</span><span class="sxs-lookup"><span data-stu-id="86d23-111">For ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="86d23-112">ASP.NET MVC 應用程式中，頁面上沒有對應至在您輸入您的瀏覽器的網址列的 URL 路徑的磁碟。</span><span class="sxs-lookup"><span data-stu-id="86d23-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="86d23-113">ASP.NET MVC 應用程式中最有用的頁面是呼叫*檢視*。</span><span class="sxs-lookup"><span data-stu-id="86d23-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="86d23-114">ASP.NET MVC 應用程式中，連入的瀏覽器要求對應至控制器動作。</span><span class="sxs-lookup"><span data-stu-id="86d23-114">ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="86d23-115">控制器動作可能會傳回檢視。</span><span class="sxs-lookup"><span data-stu-id="86d23-115">A controller action might return a view.</span></span> <span data-ttu-id="86d23-116">不過，控制器動作可能會執行其他類型的動作，例如將您重新導向至另一個控制器動作。</span><span class="sxs-lookup"><span data-stu-id="86d23-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="86d23-117">列表 1 包含名為 HomeController 的簡單控制器。</span><span class="sxs-lookup"><span data-stu-id="86d23-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="86d23-118">HomeController 會公開名為 index （） 和 Details() 的兩個控制器動作。</span><span class="sxs-lookup"><span data-stu-id="86d23-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="86d23-119">**列表 1-HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="86d23-119">**Listing 1 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

<span data-ttu-id="86d23-120">您可以呼叫第一個動作，也就是 index （） 動作中，您的瀏覽器網址列中輸入下列 URL:</span><span class="sxs-lookup"><span data-stu-id="86d23-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="86d23-121">/ Home/Index</span><span class="sxs-lookup"><span data-stu-id="86d23-121">/Home/Index</span></span>

<span data-ttu-id="86d23-122">您可以呼叫第二個動作，也就是 Details() 動作中，您的瀏覽器中輸入此位址：</span><span class="sxs-lookup"><span data-stu-id="86d23-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="86d23-123">/ Home/詳細資料</span><span class="sxs-lookup"><span data-stu-id="86d23-123">/Home/Details</span></span>

<span data-ttu-id="86d23-124">Index （） 動作傳回的檢視。</span><span class="sxs-lookup"><span data-stu-id="86d23-124">The Index() action returns a view.</span></span> <span data-ttu-id="86d23-125">您所建立的大部分動作會傳回檢視。</span><span class="sxs-lookup"><span data-stu-id="86d23-125">Most actions that you create will return views.</span></span> <span data-ttu-id="86d23-126">不過，動作可以傳回其他類型的動作結果。</span><span class="sxs-lookup"><span data-stu-id="86d23-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="86d23-127">例如，Details() 動作會傳回傳入的要求重新導向至 index （） 動作 RedirectToActionResult。</span><span class="sxs-lookup"><span data-stu-id="86d23-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="86d23-128">Index （） 動作包含下列一行程式碼：</span><span class="sxs-lookup"><span data-stu-id="86d23-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="86d23-129">View();</span><span class="sxs-lookup"><span data-stu-id="86d23-129">View();</span></span>

<span data-ttu-id="86d23-130">這行程式碼會傳回必須位於 web 伺服器上的下列路徑的檢視：</span><span class="sxs-lookup"><span data-stu-id="86d23-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="86d23-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="86d23-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="86d23-132">檢視的路徑會從控制器的名稱和控制器動作的名稱來推斷。</span><span class="sxs-lookup"><span data-stu-id="86d23-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="86d23-133">如果您想，您可以明確有關檢視。</span><span class="sxs-lookup"><span data-stu-id="86d23-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="86d23-134">下列程式碼會傳回名為 Fred 的檢視：</span><span class="sxs-lookup"><span data-stu-id="86d23-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="86d23-135">檢視 (Fred);</span><span class="sxs-lookup"><span data-stu-id="86d23-135">View( Fred );</span></span>

<span data-ttu-id="86d23-136">這行程式碼執行時，檢視會傳回從下列路徑：</span><span class="sxs-lookup"><span data-stu-id="86d23-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="86d23-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="86d23-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="86d23-138">如果您打算建立 ASP.NET MVC 應用程式的單元測試它是個不錯的主意，明確了檢視表名稱。</span><span class="sxs-lookup"><span data-stu-id="86d23-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="86d23-139">如此一來，您可以建立單元測試以確認控制器動作未傳回預期的檢視。</span><span class="sxs-lookup"><span data-stu-id="86d23-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="86d23-140">將內容新增至檢視</span><span class="sxs-lookup"><span data-stu-id="86d23-140">Adding Content to a View</span></span>

<span data-ttu-id="86d23-141">檢視是一種標準 (X) 可以包含指令碼的 HTML 文件。</span><span class="sxs-lookup"><span data-stu-id="86d23-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="86d23-142">您可以使用指令碼新增至檢視動態內容。</span><span class="sxs-lookup"><span data-stu-id="86d23-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="86d23-143">例如，列表 2 中的檢視會顯示目前的日期和時間。</span><span class="sxs-lookup"><span data-stu-id="86d23-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="86d23-144">**列表 2-\Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="86d23-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

<span data-ttu-id="86d23-145">請注意，在 列表 2 中的 HTML 頁面的主體包含下列指令碼：</span><span class="sxs-lookup"><span data-stu-id="86d23-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="86d23-146">&lt;% Response.Write(DateTime.Now);%&gt;</span><span class="sxs-lookup"><span data-stu-id="86d23-146">&lt;% Response.Write(DateTime.Now);%&gt;</span></span>

<span data-ttu-id="86d23-147">使用指令碼分隔符號&lt;%和 %&gt;來標記的開頭和結尾的指令碼。</span><span class="sxs-lookup"><span data-stu-id="86d23-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="86d23-148">此指令碼是以 C# 撰寫的。</span><span class="sxs-lookup"><span data-stu-id="86d23-148">This script is written in C#.</span></span> <span data-ttu-id="86d23-149">它會藉由呼叫 response.write （） 方法，來將內容呈現至瀏覽器中顯示目前的日期和時間。</span><span class="sxs-lookup"><span data-stu-id="86d23-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="86d23-150">指令碼分隔符號&lt;%和 %&gt;可用來執行一或多個陳述式。</span><span class="sxs-lookup"><span data-stu-id="86d23-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="86d23-151">因為您經常呼叫 response.write （） 時，Microsoft 提供您與快速鍵呼叫 response.write （） 方法。</span><span class="sxs-lookup"><span data-stu-id="86d23-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="86d23-152">[列表 3] 檢視會使用分隔符號&lt;%= %&gt;呼叫 response.write （） 的捷徑。</span><span class="sxs-lookup"><span data-stu-id="86d23-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="86d23-153">**列表 3-Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="86d23-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

<span data-ttu-id="86d23-154">您可以使用任何.NET 語言來產生動態內容檢視中。</span><span class="sxs-lookup"><span data-stu-id="86d23-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="86d23-155">一般來說，您將會使用 Visual Basic.NET 或 C# 來撰寫您的控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="86d23-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="86d23-156">使用 HTML Helper 來產生檢視內容</span><span class="sxs-lookup"><span data-stu-id="86d23-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="86d23-157">若要讓您更輕鬆地將內容新增至檢視，您可以利用所謂*HTML 協助程式*。</span><span class="sxs-lookup"><span data-stu-id="86d23-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="86d23-158">HTML 協助程式，通常是產生字串的方法。</span><span class="sxs-lookup"><span data-stu-id="86d23-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="86d23-159">您可以使用 HTML 協助程式來產生標準的 HTML 項目，例如文字方塊、 連結、 下拉式清單和清單方塊。</span><span class="sxs-lookup"><span data-stu-id="86d23-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="86d23-160">例如，在 列表 4 善加利用三個 HTML 協助程式中，檢視 BeginForm()、 TextBox() 和 Password() 協助程式，來產生登入會形成 （請參閱 圖 1）。</span><span class="sxs-lookup"><span data-stu-id="86d23-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="86d23-161">**列表 4-\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="86d23-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


<span data-ttu-id="86d23-162">[![[新增專案] 對話方塊](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="86d23-162">[![The New Project dialog box](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="86d23-163">**圖 01**： 標準的登入表單 ([按一下以檢視完整大小的影像](asp-net-mvc-views-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="86d23-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-cs/_static/image2.png))</span></span>


<span data-ttu-id="86d23-164">所有 HTML 協助程式方法會呼叫檢視的 Html 屬性。</span><span class="sxs-lookup"><span data-stu-id="86d23-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="86d23-165">比方說，您會藉由呼叫 Html.TextBox() 方法呈現文字方塊。</span><span class="sxs-lookup"><span data-stu-id="86d23-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="86d23-166">請注意，您會使用指令碼分隔符號&lt;%= %&gt;時呼叫的 Html.TextBox() 」 和 「 Html.Password() 協助程式。</span><span class="sxs-lookup"><span data-stu-id="86d23-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="86d23-167">這些協助程式只會傳回字串。</span><span class="sxs-lookup"><span data-stu-id="86d23-167">These helpers simply return a string.</span></span> <span data-ttu-id="86d23-168">您需要呼叫 response.write （），以呈現至瀏覽器的字串。</span><span class="sxs-lookup"><span data-stu-id="86d23-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="86d23-169">使用 HTML 協助程式方法是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="86d23-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="86d23-170">可以讓您的生活更容易藉由縮減的 HTML 和您要撰寫的指令碼。</span><span class="sxs-lookup"><span data-stu-id="86d23-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="86d23-171">表 5 中的檢視轉譯完全相同的形式列表 4 中的檢視，而不需使用 HTML 協助程式。</span><span class="sxs-lookup"><span data-stu-id="86d23-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="86d23-172">**列表 5-\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="86d23-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

<span data-ttu-id="86d23-173">您也可以選擇建立您自己的 HTML 協助程式。</span><span class="sxs-lookup"><span data-stu-id="86d23-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="86d23-174">例如，您可以建立會自動顯示 HTML 表格中的一組資料庫記錄的 GridView() helper 方法。</span><span class="sxs-lookup"><span data-stu-id="86d23-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="86d23-175">我們在本教學課程中探索此主題**建立的自訂 HTML 協助程式**。</span><span class="sxs-lookup"><span data-stu-id="86d23-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="86d23-176">使用將資料傳遞至檢視的檢視資料</span><span class="sxs-lookup"><span data-stu-id="86d23-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="86d23-177">您可以使用 檢視資料來將資料從控制器傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="86d23-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="86d23-178">將您透過郵件傳送與封裝相似的檢視資料。</span><span class="sxs-lookup"><span data-stu-id="86d23-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="86d23-179">必須使用這個套件傳送所有資料從控制器傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="86d23-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="86d23-180">例如，列表 6 中的控制站新增一則訊息檢視資料。</span><span class="sxs-lookup"><span data-stu-id="86d23-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="86d23-181">**列表 6-ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="86d23-181">**Listing 6 - ProductController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

<span data-ttu-id="86d23-182">控制器 ViewData 屬性表示名稱 / 值組的集合。</span><span class="sxs-lookup"><span data-stu-id="86d23-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="86d23-183">在列表 6 中，index （） 方法會將項目加入至名為具有值為 Hello World 訊息的檢視資料收集 ！。</span><span class="sxs-lookup"><span data-stu-id="86d23-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="86d23-184">Index （） 方法所傳回的檢視時，檢視資料會自動傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="86d23-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="86d23-185">列表 7 中的檢視的檢視資料中擷取訊息，並呈現至瀏覽器的訊息。</span><span class="sxs-lookup"><span data-stu-id="86d23-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="86d23-186">**列表 7-\Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="86d23-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

<span data-ttu-id="86d23-187">請注意檢視在轉譯的訊息時，會採用 Html.Encode() HTML 協助程式方法的優點。</span><span class="sxs-lookup"><span data-stu-id="86d23-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="86d23-188">Html.Encode() HTML 協助程式的特殊字元編碼這類&lt;和&gt;成可放心地顯示在網頁中的字元。</span><span class="sxs-lookup"><span data-stu-id="86d23-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="86d23-189">每當您呈現使用者提交到網站的內容時，您應該編碼內容，以防止 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="86d23-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="86d23-190">(因為我們建立訊息自行 ProductController 中，我們不 t 是否真的要對訊息進行編碼。</span><span class="sxs-lookup"><span data-stu-id="86d23-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="86d23-191">不過，它是很好的習慣，以顯示內容從檢視中檢視資料擷取時，務必呼叫 Html.Encode() 方法）。</span><span class="sxs-lookup"><span data-stu-id="86d23-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="86d23-192">在列表 7 中，我們將從控制器的簡單字串訊息傳遞至檢視的檢視資料的優點。</span><span class="sxs-lookup"><span data-stu-id="86d23-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="86d23-193">您也可以使用檢視的資料來傳遞其他類型的資料，例如資料庫記錄，從控制器到檢視的集合。</span><span class="sxs-lookup"><span data-stu-id="86d23-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="86d23-194">比方說，如果您要在檢視中，顯示產品資料庫資料表的內容，則您會傳遞集合資料庫的記錄檢視中的資料。</span><span class="sxs-lookup"><span data-stu-id="86d23-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="86d23-195">您也可以選擇將強型別的檢視資料從控制器傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="86d23-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="86d23-196">我們在本教學課程中探索此主題**了解強型別檢視資料和檢視表**。</span><span class="sxs-lookup"><span data-stu-id="86d23-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="86d23-197">總結</span><span class="sxs-lookup"><span data-stu-id="86d23-197">Summary</span></span>

<span data-ttu-id="86d23-198">本教學課程簡短介紹 ASP.NET MVC 檢視、 檢視資料和 HTML 協助程式。</span><span class="sxs-lookup"><span data-stu-id="86d23-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="86d23-199">在第一個區段中，您已了解如何將新的檢視新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="86d23-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="86d23-200">您已了解您必須新增檢視正確的資料夾以從特定的控制器呼叫它。</span><span class="sxs-lookup"><span data-stu-id="86d23-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="86d23-201">接下來，我們討論過 HTML 協助程式的主題。</span><span class="sxs-lookup"><span data-stu-id="86d23-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="86d23-202">您已了解如何 HTML 協助程式可讓您輕鬆地產生標準的 HTML 內容。</span><span class="sxs-lookup"><span data-stu-id="86d23-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="86d23-203">最後，您已了解如何利用將資料從控制器傳遞至檢視的檢視資料。</span><span class="sxs-lookup"><span data-stu-id="86d23-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="86d23-204">下一步</span><span class="sxs-lookup"><span data-stu-id="86d23-204">Next</span></span>](creating-custom-html-helpers-cs.md)
