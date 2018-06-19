---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: 建立自訂 HTML Helper (VB) |Microsoft 文件
author: microsoft
description: 本教學課程的目標是為了示範如何建立您可以使用 MVC 檢視中的自訂 HTML Helper。 利用 HTML Helper...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 6980026e2653eacb71697f9b34def9bc38638726
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871502"
---
<a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="e16dc-104">建立自訂 HTML Helper (VB)</span><span class="sxs-lookup"><span data-stu-id="e16dc-104">Creating Custom HTML Helpers (VB)</span></span>
====================
<span data-ttu-id="e16dc-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e16dc-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="e16dc-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="e16dc-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="e16dc-107">本教學課程的目標是為了示範如何建立您可以使用 MVC 檢視中的自訂 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="e16dc-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="e16dc-108">利用 HTML Helper，您可以減少的冗長的輸入，您必須執行建立標準的 HTML 網頁的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="e16dc-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="e16dc-109">本教學課程的目標是為了示範如何建立您可以使用 MVC 檢視中的自訂 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="e16dc-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="e16dc-110">利用 HTML Helper，您可以減少的冗長的輸入，您必須執行建立標準的 HTML 網頁的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="e16dc-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="e16dc-111">在本教學課程的第一個部分中，我會說明一些現有的 HTML Helper 內含在 ASP.NET MVC framework。</span><span class="sxs-lookup"><span data-stu-id="e16dc-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="e16dc-112">我接下來，描述建立自訂的 HTML Helper 的兩個方法： 我說明如何建立自訂的 HTML Helper，藉由建立的共用的方法並建立擴充方法。</span><span class="sxs-lookup"><span data-stu-id="e16dc-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="e16dc-113">了解 HTML Helper</span><span class="sxs-lookup"><span data-stu-id="e16dc-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="e16dc-114">HTML Helper 是只會傳回字串的方法。</span><span class="sxs-lookup"><span data-stu-id="e16dc-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="e16dc-115">字串可以代表任何一種您想要的內容。</span><span class="sxs-lookup"><span data-stu-id="e16dc-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="e16dc-116">例如，您可以使用 HTML Helper 來呈現標準的 HTML 標記，例如 HTML`<input>`和`<img>`標記。</span><span class="sxs-lookup"><span data-stu-id="e16dc-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="e16dc-117">您也可以使用 HTML Helper 來呈現更複雜的內容，例如索引標籤區域或資料庫資料的 HTML 表格。</span><span class="sxs-lookup"><span data-stu-id="e16dc-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="e16dc-118">ASP.NET MVC 架構包括下列的標準 （這不是完整的清單） 的 HTML Helper 集：</span><span class="sxs-lookup"><span data-stu-id="e16dc-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="e16dc-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="e16dc-119">Html.ActionLink()</span></span>
- <span data-ttu-id="e16dc-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="e16dc-120">Html.BeginForm()</span></span>
- <span data-ttu-id="e16dc-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="e16dc-121">Html.CheckBox()</span></span>
- <span data-ttu-id="e16dc-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="e16dc-122">Html.DropDownList()</span></span>
- <span data-ttu-id="e16dc-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="e16dc-123">Html.EndForm()</span></span>
- <span data-ttu-id="e16dc-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="e16dc-124">Html.Hidden()</span></span>
- <span data-ttu-id="e16dc-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="e16dc-125">Html.ListBox()</span></span>
- <span data-ttu-id="e16dc-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="e16dc-126">Html.Password()</span></span>
- <span data-ttu-id="e16dc-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="e16dc-127">Html.RadioButton()</span></span>
- <span data-ttu-id="e16dc-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="e16dc-128">Html.TextArea()</span></span>
- <span data-ttu-id="e16dc-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="e16dc-129">Html.TextBox()</span></span>

<span data-ttu-id="e16dc-130">例如，請考慮列出 1 中的表單。</span><span class="sxs-lookup"><span data-stu-id="e16dc-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="e16dc-131">此表單會呈現兩個標準的 HTML Helper （請參閱圖 1） 的協助。</span><span class="sxs-lookup"><span data-stu-id="e16dc-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="e16dc-132">這個表單用`Html.BeginForm()`和`Html.TextBox()`Helper 方法。</span><span class="sxs-lookup"><span data-stu-id="e16dc-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>


<span data-ttu-id="e16dc-133">[![使用 HTML Helper 呈現頁面](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e16dc-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="e16dc-134">**圖 01**： 以 HTML Helper 呈現的頁面 ([按一下以檢視完整大小的影像](creating-custom-html-helpers-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e16dc-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>


<span data-ttu-id="e16dc-135">**列出 1 – `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="e16dc-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="e16dc-136">`Html.BeginForm()` Helper 方法用來建立的開頭和結尾 HTML`<form>`標記。</span><span class="sxs-lookup"><span data-stu-id="e16dc-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="e16dc-137">請注意，`Html.BeginForm()`方法呼叫內的 using 陳述式。</span><span class="sxs-lookup"><span data-stu-id="e16dc-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="e16dc-138">使用陳述式時，可確保`<form>`標記取得關閉結尾所使用的區塊。</span><span class="sxs-lookup"><span data-stu-id="e16dc-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="e16dc-139">如果想要的話，而不是建立在 using 區塊中，您可以呼叫 Html.EndForm() Helper 方法，以關閉`<form>`標記。</span><span class="sxs-lookup"><span data-stu-id="e16dc-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="e16dc-140">使用何種方法來建立開啟和關閉`<form>`看起來您最直覺的標記。</span><span class="sxs-lookup"><span data-stu-id="e16dc-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="e16dc-141">`Html.TextBox()` Helper 方法會在程式碼範例 1 中用來呈現 HTML`<input>`標記。</span><span class="sxs-lookup"><span data-stu-id="e16dc-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="e16dc-142">如果您選取檢視來源瀏覽器中您會看到列出 2 中的 HTML 原始檔。</span><span class="sxs-lookup"><span data-stu-id="e16dc-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="e16dc-143">請注意，來源就會包含標準的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="e16dc-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e16dc-144">請注意， `Html.TextBox()`HTML Helper 呈現與`<%= %>`標記，而不是`<% %>`標記。</span><span class="sxs-lookup"><span data-stu-id="e16dc-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="e16dc-145">如果您不想加入等號，然後執行任何動作取得瀏覽器中呈現。</span><span class="sxs-lookup"><span data-stu-id="e16dc-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="e16dc-146">ASP.NET MVC framework 會包含較少的協助程式。</span><span class="sxs-lookup"><span data-stu-id="e16dc-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="e16dc-147">最可能的原因，您必須擴充 MVC 架構，以自訂 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="e16dc-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="e16dc-148">在本教學課程的其餘部分，您會學習建立自訂的 HTML Helper 的兩個方法。</span><span class="sxs-lookup"><span data-stu-id="e16dc-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="e16dc-149">**列出 2 – `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="e16dc-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="e16dc-150">建立使用共用方法的 HTML Helper</span><span class="sxs-lookup"><span data-stu-id="e16dc-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="e16dc-151">若要建立新的 HTML Helper 的最簡單方式是建立共用的方法會傳回字串。</span><span class="sxs-lookup"><span data-stu-id="e16dc-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="e16dc-152">例如，假設，您決定要建立新的 HTML Helper 呈現 HTML`<label>`標記。</span><span class="sxs-lookup"><span data-stu-id="e16dc-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="e16dc-153">您可以列出 2 中使用類別來呈現`<label>`。</span><span class="sxs-lookup"><span data-stu-id="e16dc-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="e16dc-154">**列出 2 – `Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="e16dc-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="e16dc-155">沒有任何特殊有關列表 2 中的類別。</span><span class="sxs-lookup"><span data-stu-id="e16dc-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="e16dc-156">`Label()`方法只會傳回字串。</span><span class="sxs-lookup"><span data-stu-id="e16dc-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="e16dc-157">使用修改的索引檢視表中列出的 3`LabelHelper`來呈現 HTML`<label>`標記。</span><span class="sxs-lookup"><span data-stu-id="e16dc-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="e16dc-158">請注意，檢視包含`<%@ imports %>`匯入 Application1.Helpers 命名空間的指示詞。</span><span class="sxs-lookup"><span data-stu-id="e16dc-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="e16dc-159">**列出 2 – `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="e16dc-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="e16dc-160">建立具有擴充方法的 HTML Helper</span><span class="sxs-lookup"><span data-stu-id="e16dc-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="e16dc-161">如果您想要建立只使用的 HTML Helper，例如標準包含在 ASP.NET MVC 架構，則您需要建立擴充方法的 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="e16dc-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="e16dc-162">擴充方法可讓您將新方法加入至現有的類別。</span><span class="sxs-lookup"><span data-stu-id="e16dc-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="e16dc-163">建立時的 HTML Helper 方法，您將新方法新增至`HtmlHelper`檢視的 Html 屬性所代表的類別。</span><span class="sxs-lookup"><span data-stu-id="e16dc-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="e16dc-164">Visual Basic 模組中列出的 3 新增名為擴充方法`Label()`至`HtmlHelper`類別。</span><span class="sxs-lookup"><span data-stu-id="e16dc-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="e16dc-165">有幾個您應該注意到此模組的相關的東西。</span><span class="sxs-lookup"><span data-stu-id="e16dc-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="e16dc-166">首先，請注意此模組以裝飾`<Extension()>`屬性。</span><span class="sxs-lookup"><span data-stu-id="e16dc-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="e16dc-167">若要使用這個屬性，您必須匯入`System.Runtime.CompilerServices`命名空間</span><span class="sxs-lookup"><span data-stu-id="e16dc-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="e16dc-168">第二，請注意，第一個參數`Label()`方法代表`HtmlHelper`類別。</span><span class="sxs-lookup"><span data-stu-id="e16dc-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="e16dc-169">擴充方法的第一個參數會指出延伸模組方法所擴充的類別。</span><span class="sxs-lookup"><span data-stu-id="e16dc-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="e16dc-170">**列出 3 – `Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="e16dc-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="e16dc-171">您建立擴充方法，並建置您的應用程式成功後，擴充方法會出現在 Visual Studio Intellisense，如同所有其他類別的方法 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="e16dc-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="e16dc-172">唯一的差別是該方法會出現以特殊符號旁邊 （向下箭號圖示） 的擴充功能。</span><span class="sxs-lookup"><span data-stu-id="e16dc-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="e16dc-173">[![使用 Html.Label() 擴充方法](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="e16dc-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="e16dc-174">**圖 02**: Html.Label() 擴充方法 ([按一下以檢視完整大小的影像](creating-custom-html-helpers-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="e16dc-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>


<span data-ttu-id="e16dc-175">修改的索引檢視表中列出的 4 會使用 Html.Label() 擴充方法的所有呈現其&lt;標籤&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="e16dc-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="e16dc-176">**列出 4 – `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="e16dc-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="e16dc-177">總結</span><span class="sxs-lookup"><span data-stu-id="e16dc-177">Summary</span></span>

<span data-ttu-id="e16dc-178">在本教學課程中，您將學會建立自訂的 HTML Helper 的兩個方法。</span><span class="sxs-lookup"><span data-stu-id="e16dc-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="e16dc-179">首先，您學到如何建立自訂`Label()`HTML Helper 所建立的共用的方法傳回的字串。</span><span class="sxs-lookup"><span data-stu-id="e16dc-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="e16dc-180">接下來，您學到如何建立自訂`Label()`上建立擴充方法的 HTML Helper 方法`HtmlHelper`類別。</span><span class="sxs-lookup"><span data-stu-id="e16dc-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="e16dc-181">在本教學課程中，我會著重於建立非常簡單的 HTML Helper 方法。</span><span class="sxs-lookup"><span data-stu-id="e16dc-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="e16dc-182">請注意，HTML Helper 就是您想要一樣複雜。</span><span class="sxs-lookup"><span data-stu-id="e16dc-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="e16dc-183">您可以建置豐富的內容，例如樹狀檢視、 功能表、 或資料表的資料庫資料的呈現的 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="e16dc-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e16dc-184">[上一頁](asp-net-mvc-views-overview-vb.md)
> [下一頁](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e16dc-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
