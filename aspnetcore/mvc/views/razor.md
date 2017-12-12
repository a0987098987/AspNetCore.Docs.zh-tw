---
title: "適用於 ASP.NET Core razor 語法參考"
author: rick-anderson
description: "深入了解伺服器基礎的程式碼內嵌到網頁的 Razor 標記語法。"
keywords: "ASP.NET Core，Razor，Razor 指示詞"
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: e3c3149254d602db1fcc6d42360690be026189a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="3b311-104">Razor 語法的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3b311-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="3b311-105">由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Luke Latham](https://github.com/guardrex)， [Taylor Mullen](https://twitter.com/ntaylormullen)，和[Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="3b311-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex),  [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="3b311-106">Razor 是將伺服器端程式碼內嵌到網頁標記語法。</span><span class="sxs-lookup"><span data-stu-id="3b311-106">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="3b311-107">Razor 語法包含 Razor 標記、 C# 和 HTML。</span><span class="sxs-lookup"><span data-stu-id="3b311-107">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="3b311-108">通常包含 Razor 檔案有*.cshtml*檔案副檔名。</span><span class="sxs-lookup"><span data-stu-id="3b311-108">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="3b311-109">將 HTML 呈現</span><span class="sxs-lookup"><span data-stu-id="3b311-109">Rendering HTML</span></span>

<span data-ttu-id="3b311-110">預設 Razor 語言為 HTML。</span><span class="sxs-lookup"><span data-stu-id="3b311-110">The default Razor language is HTML.</span></span> <span data-ttu-id="3b311-111">轉譯 HTML Razor 標記中的並無不同轉譯 HTML，從 HTML 檔案。</span><span class="sxs-lookup"><span data-stu-id="3b311-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span>  <span data-ttu-id="3b311-112">中的 HTML 標記*.cshtml* Razor 檔案會呈現未變更的伺服器。</span><span class="sxs-lookup"><span data-stu-id="3b311-112">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="3b311-113">Razor 語法</span><span class="sxs-lookup"><span data-stu-id="3b311-113">Razor syntax</span></span>

<span data-ttu-id="3b311-114">Razor 支援 C#，並使用`@`轉換來自 HTML，以 C# 中的符號。</span><span class="sxs-lookup"><span data-stu-id="3b311-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="3b311-115">Razor 評估 C# 運算式，並將它們呈現的 HTML 輸出中。</span><span class="sxs-lookup"><span data-stu-id="3b311-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="3b311-116">當`@`符號後面[Razor 保留關鍵字](#razor-reserved-keywords)，它就會轉換成特定的 Razor 標記。</span><span class="sxs-lookup"><span data-stu-id="3b311-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="3b311-117">否則，會轉換成一般的 C#。</span><span class="sxs-lookup"><span data-stu-id="3b311-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="3b311-118">要逸出`@`Razor 標記中的符號，請使用第二個`@`符號：</span><span class="sxs-lookup"><span data-stu-id="3b311-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="3b311-119">程式碼會以 HTML 轉譯單一`@`符號：</span><span class="sxs-lookup"><span data-stu-id="3b311-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="3b311-120">HTML 屬性和內容包含電子郵件地址不會將`@`符號為轉換字元。</span><span class="sxs-lookup"><span data-stu-id="3b311-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="3b311-121">電子郵件地址，在下列範例中被不受影響的 Razor 剖析：</span><span class="sxs-lookup"><span data-stu-id="3b311-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="3b311-122">Razor 的隱含運算式</span><span class="sxs-lookup"><span data-stu-id="3b311-122">Implicit Razor expressions</span></span>

<span data-ttu-id="3b311-123">Razor 的隱含運算式開頭`@`後面接著 C# 程式碼：</span><span class="sxs-lookup"><span data-stu-id="3b311-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="3b311-124">除了 C#`await`關鍵字，隱含運算式不能包含空格。</span><span class="sxs-lookup"><span data-stu-id="3b311-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="3b311-125">如果 C# 陳述式已清除結束，混合空格：</span><span class="sxs-lookup"><span data-stu-id="3b311-125">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="3b311-126">隱含運算式**無法**包含 C# 泛型，括號內的字元 (`<>`) 會解譯為 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="3b311-126">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="3b311-127">下列程式碼是**不**有效：</span><span class="sxs-lookup"><span data-stu-id="3b311-127">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="3b311-128">上述程式碼會產生編譯器錯誤類似下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="3b311-128">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="3b311-129">"Int"項目未結束。</span><span class="sxs-lookup"><span data-stu-id="3b311-129">The "int" element was not closed.</span></span>  <span data-ttu-id="3b311-130">所有元素都必須自行關閉或沒有對稱的結束標記。</span><span class="sxs-lookup"><span data-stu-id="3b311-130">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="3b311-131">無法將方法群組 'GenericMethod' 為非委派類型 'object' 的轉換。</span><span class="sxs-lookup"><span data-stu-id="3b311-131">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="3b311-132">您是否想要叫用的方法？ '</span><span class="sxs-lookup"><span data-stu-id="3b311-132">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="3b311-133">泛型方法的呼叫必須包裝在[明確 Razor 運算式](#explicit-razor-expressions)或[Razor 程式碼區塊](#razor-code-blocks)。</span><span class="sxs-lookup"><span data-stu-id="3b311-133">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span> <span data-ttu-id="3b311-134">這項限制不適用於*.vbhtml* Razor 檔案，因為 Visual Basic 語法會將泛型型別參數，而不是方括號括住。</span><span class="sxs-lookup"><span data-stu-id="3b311-134">This restriction doesn't apply to *.vbhtml* Razor files because Visual Basic syntax places parentheses around generic type parameters instead of brackets.</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="3b311-135">Razor 的明確運算式</span><span class="sxs-lookup"><span data-stu-id="3b311-135">Explicit Razor expressions</span></span>

<span data-ttu-id="3b311-136">Razor 的明確運算式組成`@`有對稱的括號的符號。</span><span class="sxs-lookup"><span data-stu-id="3b311-136">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="3b311-137">若要轉譯上週的時間，會使用下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="3b311-137">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="3b311-138">內的任何內容`@()`括號會評估並呈現至輸出。</span><span class="sxs-lookup"><span data-stu-id="3b311-138">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="3b311-139">上一節中所述的隱含運算式通常不能包含空格。</span><span class="sxs-lookup"><span data-stu-id="3b311-139">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="3b311-140">下列程式碼中，一週未減去目前的時間：</span><span class="sxs-lookup"><span data-stu-id="3b311-140">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="3b311-141">程式碼會轉譯下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="3b311-141">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="3b311-142">明確運算式可以用來串連文字與運算式結果：</span><span class="sxs-lookup"><span data-stu-id="3b311-142">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="3b311-143">沒有明確的運算式，`<p>Age@joe.Age</p>`會被視為電子郵件地址和`<p>Age@joe.Age</p>`轉譯。</span><span class="sxs-lookup"><span data-stu-id="3b311-143">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="3b311-144">做為明確的運算式，寫入時`<p>Age33</p>`轉譯。</span><span class="sxs-lookup"><span data-stu-id="3b311-144">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="3b311-145">明確運算式可以用來呈現在泛型方法的輸出*.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="3b311-145">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="3b311-146">在隱含運算式中，括號內的字元 (`<>`) 會解譯為 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="3b311-146">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="3b311-147">下列標記是**不**有效 Razor:</span><span class="sxs-lookup"><span data-stu-id="3b311-147">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="3b311-148">上述程式碼會產生編譯器錯誤類似下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="3b311-148">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="3b311-149">"Int"項目未結束。</span><span class="sxs-lookup"><span data-stu-id="3b311-149">The "int" element was not closed.</span></span>  <span data-ttu-id="3b311-150">所有元素都必須自行關閉或沒有對稱的結束標記。</span><span class="sxs-lookup"><span data-stu-id="3b311-150">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="3b311-151">無法將方法群組 'GenericMethod' 為非委派類型 'object' 的轉換。</span><span class="sxs-lookup"><span data-stu-id="3b311-151">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="3b311-152">您是否想要叫用的方法？ '</span><span class="sxs-lookup"><span data-stu-id="3b311-152">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="3b311-153">下列標記會顯示正確的方式寫入這段程式碼。</span><span class="sxs-lookup"><span data-stu-id="3b311-153">The following markup shows the correct way write this code.</span></span>  <span data-ttu-id="3b311-154">程式碼會寫入做為明確運算式：</span><span class="sxs-lookup"><span data-stu-id="3b311-154">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

<span data-ttu-id="3b311-155">注意： 這項限制不適用於*.vbhtml* Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="3b311-155">Note: this restriction doesn't apply to *.vbhtml* Razor files.</span></span>  <span data-ttu-id="3b311-156">與*.vbhtml* Razor 檔案，Visual Basic 語法會將泛型型別參數，而不是方括號括住。</span><span class="sxs-lookup"><span data-stu-id="3b311-156">With *.vbhtml* Razor files, Visual Basic syntax places parentheses around generic type parameters instead of brackets.</span></span>

## <a name="expression-encoding"></a><span data-ttu-id="3b311-157">運算式的編碼方式</span><span class="sxs-lookup"><span data-stu-id="3b311-157">Expression encoding</span></span>

<span data-ttu-id="3b311-158">C# 運算式，評估為字串會以 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="3b311-158">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="3b311-159">C# 運算式會評估得出`IHtmlContent`轉譯直接透過`IHtmlContent.WriteTo`。</span><span class="sxs-lookup"><span data-stu-id="3b311-159">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="3b311-160">C# 運算式未評估成`IHtmlContent`會轉換成字串`ToString`和編碼它們在呈現之前。</span><span class="sxs-lookup"><span data-stu-id="3b311-160">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="3b311-161">程式碼會轉譯下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="3b311-161">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="3b311-162">HTML 會顯示為瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="3b311-162">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="3b311-163">`HtmlHelper.Raw`輸出不編碼，但轉譯為 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="3b311-163">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="3b311-164">使用`HtmlHelper.Raw`unsanitized 使用者輸入會造成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="3b311-164">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="3b311-165">使用者輸入可能包含惡意的 JavaScript 或其他入侵。</span><span class="sxs-lookup"><span data-stu-id="3b311-165">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="3b311-166">免於使用者輸入並不容易。</span><span class="sxs-lookup"><span data-stu-id="3b311-166">Sanitizing user input is difficult.</span></span> <span data-ttu-id="3b311-167">請避免使用`HtmlHelper.Raw`與使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="3b311-167">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="3b311-168">程式碼會轉譯下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="3b311-168">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="3b311-169">Razor 程式碼區塊</span><span class="sxs-lookup"><span data-stu-id="3b311-169">Razor code blocks</span></span>

<span data-ttu-id="3b311-170">Razor 程式碼區塊的開頭`@`和由`{}`。</span><span class="sxs-lookup"><span data-stu-id="3b311-170">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="3b311-171">不同於運算式中，不會呈現 C# 程式碼區塊內的程式碼。</span><span class="sxs-lookup"><span data-stu-id="3b311-171">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="3b311-172">程式碼區塊和檢視中的運算式共用相同範圍和順序中所定義：</span><span class="sxs-lookup"><span data-stu-id="3b311-172">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

<span data-ttu-id="3b311-173">程式碼會轉譯下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="3b311-173">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="3b311-174">隱含轉換</span><span class="sxs-lookup"><span data-stu-id="3b311-174">Implicit transitions</span></span>

<span data-ttu-id="3b311-175">程式碼區塊中的預設語言是 C# 中，但 Razor 頁面可以轉換回 HTML:</span><span class="sxs-lookup"><span data-stu-id="3b311-175">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="3b311-176">明確分隔的轉換</span><span class="sxs-lookup"><span data-stu-id="3b311-176">Explicit delimited transition</span></span>

<span data-ttu-id="3b311-177">若要定義應該呈現的 HTML 程式碼區塊的子區段，使用 Razor 括住的字元轉譯**\<文字 >**標記：</span><span class="sxs-lookup"><span data-stu-id="3b311-177">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="3b311-178">使用此方式來呈現不以 HTML 標記括住的 HTML。</span><span class="sxs-lookup"><span data-stu-id="3b311-178">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="3b311-179">沒有 HTML 或 Razor 的標籤，就會發生 Razor 執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="3b311-179">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="3b311-180">**\<文字 >**標記可用於轉譯內容時控制空白字元：</span><span class="sxs-lookup"><span data-stu-id="3b311-180">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="3b311-181">之間的內容**\<文字 >**標記的呈現。</span><span class="sxs-lookup"><span data-stu-id="3b311-181">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="3b311-182">不能有空白之前或之後**\<文字 >** HTML 輸出中會顯示標籤。</span><span class="sxs-lookup"><span data-stu-id="3b311-182">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="3b311-183">使用明確列轉換 @:</span><span class="sxs-lookup"><span data-stu-id="3b311-183">Explicit Line Transition with @:</span></span>

<span data-ttu-id="3b311-184">若要轉譯為 HTML 的整行的其餘部分的程式碼區塊內，使用`@:`語法：</span><span class="sxs-lookup"><span data-stu-id="3b311-184">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="3b311-185">不含`@:`在程式碼會產生 Razor 的執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="3b311-185">Without the `@:` in the code,  a Razor runtime error is generated.</span></span>

<span data-ttu-id="3b311-186">警告： 額外`@`Razor 檔案中的字元可能會造成在陳述式會導致編譯器錯誤稍後區塊。</span><span class="sxs-lookup"><span data-stu-id="3b311-186">Warning: Extra `@` characters in a Razor file can cause  cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="3b311-187">這些編譯器錯誤很難了解，因為實際的錯誤發生之前報告的錯誤。</span><span class="sxs-lookup"><span data-stu-id="3b311-187">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span>  <span data-ttu-id="3b311-188">此錯誤之後結合成單一的程式碼區塊的多個隱含/明確運算式是常見的。</span><span class="sxs-lookup"><span data-stu-id="3b311-188">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="3b311-189">控制結構</span><span class="sxs-lookup"><span data-stu-id="3b311-189">Control Structures</span></span>

<span data-ttu-id="3b311-190">控制結構是一種擴充的程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="3b311-190">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="3b311-191">所有層面的程式碼區塊 （轉換到標記中，內嵌 C#） 也適都用於下列結構：</span><span class="sxs-lookup"><span data-stu-id="3b311-191">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="3b311-192">條件式@if，else 的 if、 和@switch</span><span class="sxs-lookup"><span data-stu-id="3b311-192">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="3b311-193">`@if`控制項時執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="3b311-193">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="3b311-194">`else`和`else if`不需要`@`符號：</span><span class="sxs-lookup"><span data-stu-id="3b311-194">`else` and `else if` don't require the `@` symbol:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

<span data-ttu-id="3b311-195">下列標記會顯示如何使用 switch 陳述式：</span><span class="sxs-lookup"><span data-stu-id="3b311-195">The following markup shows how to use a switch statement:</span></span>

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="3b311-196">迴圈@for， @foreach， @while，和@do時</span><span class="sxs-lookup"><span data-stu-id="3b311-196">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="3b311-197">樣板化 HTML 可以轉譯迴圈控制陳述式。</span><span class="sxs-lookup"><span data-stu-id="3b311-197">Templated HTML can be rendered with looping control statements.</span></span>  <span data-ttu-id="3b311-198">若要轉譯的人員清單：</span><span class="sxs-lookup"><span data-stu-id="3b311-198">To render a list of people:</span></span>

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

<span data-ttu-id="3b311-199">支援下列的迴圈陳述式：</span><span class="sxs-lookup"><span data-stu-id="3b311-199">The following looping statements are supported:</span></span>

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a><span data-ttu-id="3b311-200">複合@using</span><span class="sxs-lookup"><span data-stu-id="3b311-200">Compound @using</span></span>

<span data-ttu-id="3b311-201">在 C# 中，`using`陳述式用來確保處置物件。</span><span class="sxs-lookup"><span data-stu-id="3b311-201">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="3b311-202">在 Razor，相同的機制用來建立包含其他內容的 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="3b311-202">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="3b311-203">下列程式碼中，HTML Helper 呈現表單標記與`@using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="3b311-203">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

<span data-ttu-id="3b311-204">範圍層級的動作可以使用執行[標記協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="3b311-204">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="3b311-205">@trycatch、 finally</span><span class="sxs-lookup"><span data-stu-id="3b311-205">@try, catch, finally</span></span>

<span data-ttu-id="3b311-206">例外狀況處理會類似於 C#:</span><span class="sxs-lookup"><span data-stu-id="3b311-206">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="3b311-207">Razor 具有保護與 lock 陳述式的關鍵區段的功能：</span><span class="sxs-lookup"><span data-stu-id="3b311-207">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="3b311-208">註解</span><span class="sxs-lookup"><span data-stu-id="3b311-208">Comments</span></span>

<span data-ttu-id="3b311-209">Razor 支援 C# 和 HTML 註解：</span><span class="sxs-lookup"><span data-stu-id="3b311-209">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="3b311-210">程式碼會轉譯下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="3b311-210">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="3b311-211">呈現網頁之前，伺服器會移除 razor 註解。</span><span class="sxs-lookup"><span data-stu-id="3b311-211">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="3b311-212">使用 razor`@*  *@`來分隔註解。</span><span class="sxs-lookup"><span data-stu-id="3b311-212">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="3b311-213">下列程式碼會標記為註解，讓伺服器不會呈現任何標記：</span><span class="sxs-lookup"><span data-stu-id="3b311-213">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="3b311-214">指示詞</span><span class="sxs-lookup"><span data-stu-id="3b311-214">Directives</span></span>

<span data-ttu-id="3b311-215">Razor 指示詞都由下列保留的關鍵字的隱含運算式`@`符號。</span><span class="sxs-lookup"><span data-stu-id="3b311-215">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="3b311-216">指示詞通常會變更的方式，檢視會剖析或啟用不同的功能。</span><span class="sxs-lookup"><span data-stu-id="3b311-216">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="3b311-217">了解 Razor 如何產生程式碼 檢視，可以更輕鬆地了解指示詞的運作方式。</span><span class="sxs-lookup"><span data-stu-id="3b311-217">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="3b311-218">程式碼會產生的類別如下所示：</span><span class="sxs-lookup"><span data-stu-id="3b311-218">The code generates a class similar to the following:</span></span>

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

<span data-ttu-id="3b311-219">在本文中 > 一節稍後[檢視產生檢視的 Razor C# 類別](#viewing-the-razor-c-class-generated-for-a-view)說明如何檢視這個產生的類別。</span><span class="sxs-lookup"><span data-stu-id="3b311-219">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="3b311-220">`@using`指示詞加入 C#`using`指示詞加入產生的檢視：</span><span class="sxs-lookup"><span data-stu-id="3b311-220">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="3b311-221">`@model`指示詞會指定傳遞至檢視的模型型別：</span><span class="sxs-lookup"><span data-stu-id="3b311-221">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="3b311-222">在 ASP.NET Core MVC 應用程式中使用個別的使用者帳戶建立*Views/Account/Login.cshtml*檢視包含下列模型宣告：</span><span class="sxs-lookup"><span data-stu-id="3b311-222">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="3b311-223">產生的類別繼承自`RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="3b311-223">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="3b311-224">Razor 公開`Model`屬性，以存取模型傳遞至檢視：</span><span class="sxs-lookup"><span data-stu-id="3b311-224">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="3b311-225">`@model`指示詞會指定此屬性的型別。</span><span class="sxs-lookup"><span data-stu-id="3b311-225">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="3b311-226">指示詞會指定`T`中`RazorPage<T>`，所產生類別，衍生自的檢視。</span><span class="sxs-lookup"><span data-stu-id="3b311-226">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="3b311-227">如果`@model`指示詞並不指定，`Model`屬性屬於型別`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="3b311-227">If  the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="3b311-228">模型值會從控制器到檢視。</span><span class="sxs-lookup"><span data-stu-id="3b311-228">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="3b311-229">如需詳細資訊，請參閱 [強類型模型和@model關鍵字。</span><span class="sxs-lookup"><span data-stu-id="3b311-229">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="3b311-230">`@inherits`指示詞提供的檢視所繼承的類別的完整控制權：</span><span class="sxs-lookup"><span data-stu-id="3b311-230">The `@inherits` directive provides  full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="3b311-231">下列程式碼是自訂的 Razor 頁面類型：</span><span class="sxs-lookup"><span data-stu-id="3b311-231">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="3b311-232">`CustomText`會顯示在檢視中：</span><span class="sxs-lookup"><span data-stu-id="3b311-232">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="3b311-233">程式碼會轉譯下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="3b311-233">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="3b311-234">`@model`和`@inherits`可以使用相同的檢視中。</span><span class="sxs-lookup"><span data-stu-id="3b311-234">`@model` and `@inherits` can be used in the same view.</span></span>  <span data-ttu-id="3b311-235">`@inherits`可以是*_ViewImports.cshtml*檢視匯入的檔案：</span><span class="sxs-lookup"><span data-stu-id="3b311-235">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="3b311-236">下列程式碼是強型別檢視的範例：</span><span class="sxs-lookup"><span data-stu-id="3b311-236">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="3b311-237">如果 「rick@contoso.com」 會在模型中，檢視就會產生下列的 HTML 標記：</span><span class="sxs-lookup"><span data-stu-id="3b311-237">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="3b311-238">`@inject`指示詞可讓 Razor 頁面，將服務從[服務容器](xref:fundamentals/dependency-injection)插入檢視。</span><span class="sxs-lookup"><span data-stu-id="3b311-238">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="3b311-239">如需詳細資訊，請參閱[檢視的相依性插入](xref:mvc/views/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="3b311-239">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="3b311-240">`@functions`指示詞可讓 Razor 頁面將函式層級內容加入至檢視：</span><span class="sxs-lookup"><span data-stu-id="3b311-240">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="3b311-241">例如: </span><span class="sxs-lookup"><span data-stu-id="3b311-241">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="3b311-242">程式碼會產生下列的 HTML 標記：</span><span class="sxs-lookup"><span data-stu-id="3b311-242">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="3b311-243">下列程式碼會產生的 Razor C# 類別：</span><span class="sxs-lookup"><span data-stu-id="3b311-243">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="3b311-244">`@section`指示詞用於搭配[配置](xref:mvc/views/layout)啟用檢視轉譯的 HTML 網頁的不同部分中的內容。</span><span class="sxs-lookup"><span data-stu-id="3b311-244">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="3b311-245">如需詳細資訊，請參閱[區段](xref:mvc/views/layout#layout-sections-label)。</span><span class="sxs-lookup"><span data-stu-id="3b311-245">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="3b311-246">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="3b311-246">Tag Helpers</span></span>

<span data-ttu-id="3b311-247">有三個指示詞與[標記協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="3b311-247">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="3b311-248">指示詞</span><span class="sxs-lookup"><span data-stu-id="3b311-248">Directive</span></span> | <span data-ttu-id="3b311-249">函式</span><span class="sxs-lookup"><span data-stu-id="3b311-249">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="3b311-250">使標記協助程式可供檢視。</span><span class="sxs-lookup"><span data-stu-id="3b311-250">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="3b311-251">移除先前加入從檢視表的標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="3b311-251">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="3b311-252">指定啟用標記協助程式支援，且能夠明確標記協助程式使用方式的標記首碼。</span><span class="sxs-lookup"><span data-stu-id="3b311-252">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="3b311-253">Razor 保留關鍵字</span><span class="sxs-lookup"><span data-stu-id="3b311-253">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="3b311-254">Razor 關鍵字</span><span class="sxs-lookup"><span data-stu-id="3b311-254">Razor keywords</span></span>

* <span data-ttu-id="3b311-255">頁面 （需要 ASP.NET Core 2.0 和更新版本）</span><span class="sxs-lookup"><span data-stu-id="3b311-255">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="3b311-256">函式</span><span class="sxs-lookup"><span data-stu-id="3b311-256">functions</span></span>
* <span data-ttu-id="3b311-257">繼承</span><span class="sxs-lookup"><span data-stu-id="3b311-257">inherits</span></span>
* <span data-ttu-id="3b311-258">model</span><span class="sxs-lookup"><span data-stu-id="3b311-258">model</span></span>
* <span data-ttu-id="3b311-259">section</span><span class="sxs-lookup"><span data-stu-id="3b311-259">section</span></span>
* <span data-ttu-id="3b311-260">（目前不支援 ASP.NET Core） 的協助程式</span><span class="sxs-lookup"><span data-stu-id="3b311-260">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="3b311-261">Razor 關鍵字會以逸出`@(Razor Keyword)`(例如， `@(functions)`)。</span><span class="sxs-lookup"><span data-stu-id="3b311-261">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="3b311-262">C# Razor 關鍵字</span><span class="sxs-lookup"><span data-stu-id="3b311-262">C# Razor keywords</span></span>

* <span data-ttu-id="3b311-263">case</span><span class="sxs-lookup"><span data-stu-id="3b311-263">case</span></span>
* <span data-ttu-id="3b311-264">do</span><span class="sxs-lookup"><span data-stu-id="3b311-264">do</span></span>
* <span data-ttu-id="3b311-265">default</span><span class="sxs-lookup"><span data-stu-id="3b311-265">default</span></span>
* <span data-ttu-id="3b311-266">for</span><span class="sxs-lookup"><span data-stu-id="3b311-266">for</span></span>
* <span data-ttu-id="3b311-267">foreach</span><span class="sxs-lookup"><span data-stu-id="3b311-267">foreach</span></span>
* <span data-ttu-id="3b311-268">if</span><span class="sxs-lookup"><span data-stu-id="3b311-268">if</span></span>
* <span data-ttu-id="3b311-269">else</span><span class="sxs-lookup"><span data-stu-id="3b311-269">else</span></span>
* <span data-ttu-id="3b311-270">lock</span><span class="sxs-lookup"><span data-stu-id="3b311-270">lock</span></span>
* <span data-ttu-id="3b311-271">switch</span><span class="sxs-lookup"><span data-stu-id="3b311-271">switch</span></span>
* <span data-ttu-id="3b311-272">try</span><span class="sxs-lookup"><span data-stu-id="3b311-272">try</span></span>
* <span data-ttu-id="3b311-273">catch</span><span class="sxs-lookup"><span data-stu-id="3b311-273">catch</span></span>
* <span data-ttu-id="3b311-274">finally</span><span class="sxs-lookup"><span data-stu-id="3b311-274">finally</span></span>
* <span data-ttu-id="3b311-275">使用</span><span class="sxs-lookup"><span data-stu-id="3b311-275">using</span></span>
* <span data-ttu-id="3b311-276">while</span><span class="sxs-lookup"><span data-stu-id="3b311-276">while</span></span>

<span data-ttu-id="3b311-277">C# Razor 關鍵字必須是雙逸出與`@(@C# Razor Keyword)`(例如， `@(@case)`)。</span><span class="sxs-lookup"><span data-stu-id="3b311-277">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="3b311-278">第一個`@`逸出 Razor 剖析器。</span><span class="sxs-lookup"><span data-stu-id="3b311-278">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="3b311-279">第二個`@`逸出的 C# 剖析器。</span><span class="sxs-lookup"><span data-stu-id="3b311-279">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="3b311-280">不使用 Razor 的保留的關鍵字</span><span class="sxs-lookup"><span data-stu-id="3b311-280">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="3b311-281">namespace</span><span class="sxs-lookup"><span data-stu-id="3b311-281">namespace</span></span>
* <span data-ttu-id="3b311-282">Class - 類別</span><span class="sxs-lookup"><span data-stu-id="3b311-282">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="3b311-283">檢視檢視產生的 Razor C# 類別</span><span class="sxs-lookup"><span data-stu-id="3b311-283">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="3b311-284">加入 ASP.NET Core MVC 專案中的下列類別：</span><span class="sxs-lookup"><span data-stu-id="3b311-284">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="3b311-285">覆寫`RazorTemplateEngine`加入與 MVC`CustomTemplateEngine`類別：</span><span class="sxs-lookup"><span data-stu-id="3b311-285">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="3b311-286">在 設定中斷點`return csharpDocument`陳述式的`CustomTemplateEngine`。</span><span class="sxs-lookup"><span data-stu-id="3b311-286">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="3b311-287">當在中斷點停止執行程式時，檢視 值`generatedCode`。</span><span class="sxs-lookup"><span data-stu-id="3b311-287">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![GeneratedCode 文字視覺化檢視](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="3b311-289">檢視對應及區分大小寫</span><span class="sxs-lookup"><span data-stu-id="3b311-289">View lookups and case sensitivity</span></span>

<span data-ttu-id="3b311-290">Razor 檢視引擎會執行檢視的區分大小寫的查閱。</span><span class="sxs-lookup"><span data-stu-id="3b311-290">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="3b311-291">不過，實際查閱是由基礎檔案系統的方式決定：</span><span class="sxs-lookup"><span data-stu-id="3b311-291">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="3b311-292">根據的來源檔案：</span><span class="sxs-lookup"><span data-stu-id="3b311-292">File based source:</span></span> 
  * <span data-ttu-id="3b311-293">在作業系統上使用不區分大小寫的檔案系統 (例如 Windows)，實體檔案提供者對應是不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="3b311-293">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="3b311-294">例如，`return View("Test")`符合的項目會導致*/Views/Home/Test.cshtml*， */Views/home/test.cshtml*，和任何其他大小寫變數。</span><span class="sxs-lookup"><span data-stu-id="3b311-294">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="3b311-295">在區分大小寫的檔案系統上 (例如 Linux、 OSX，與`EmbeddedFileProvider`)，查詢不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="3b311-295">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="3b311-296">例如，`return View("Test")`特別相符項目*/Views/Home/Test.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="3b311-296">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="3b311-297">先行編譯的檢視： 使用 ASP.NET Core 2.0 和更新版本，查閱先行編譯的檢視是不區分大小寫在所有作業系統上。</span><span class="sxs-lookup"><span data-stu-id="3b311-297">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="3b311-298">行為是在 Windows 上的實體檔案提供者的行為相同。</span><span class="sxs-lookup"><span data-stu-id="3b311-298">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="3b311-299">如果兩個先行編譯的檢視只有大小寫不同，查詢的結果會是不具決定性。</span><span class="sxs-lookup"><span data-stu-id="3b311-299">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="3b311-300">開發人員都要比對的大小寫的檔案和目錄名稱的大小寫：</span><span class="sxs-lookup"><span data-stu-id="3b311-300">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="3b311-301">區域、 控制器和動作名稱。</span><span class="sxs-lookup"><span data-stu-id="3b311-301">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="3b311-302">Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="3b311-302">Razor Pages.</span></span>
    
<span data-ttu-id="3b311-303">相符的情況下，可確保部署尋找及其不論基礎檔案系統的檢視。</span><span class="sxs-lookup"><span data-stu-id="3b311-303">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
