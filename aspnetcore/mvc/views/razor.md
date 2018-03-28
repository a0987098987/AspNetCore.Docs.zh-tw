---
title: ASP.NET Core 的 Razor 語法參考
author: rick-anderson
description: 了解將伺服器架構程式碼內嵌到網頁中的 Razor 標記語法。
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/razor
ms.openlocfilehash: 98021cc76555f0c1402764c845471a4730b01b20
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="b726f-103">ASP.NET Core 的 Razor 語法</span><span class="sxs-lookup"><span data-stu-id="b726f-103">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="b726f-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Luke Latham](https://github.com/guardrex)、[Taylor Mullen](https://twitter.com/ntaylormullen) 與 [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="b726f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="b726f-105">Razor 是將伺服器架構程式碼內嵌到網頁中的標記語法。</span><span class="sxs-lookup"><span data-stu-id="b726f-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="b726f-106">Razor 語法是由 Razor 標記、C# 和 HTML 所組成。</span><span class="sxs-lookup"><span data-stu-id="b726f-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="b726f-107">含有 Razor 的檔案通常具有 *.cshtml* 副檔名。</span><span class="sxs-lookup"><span data-stu-id="b726f-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="b726f-108">轉譯 HTML</span><span class="sxs-lookup"><span data-stu-id="b726f-108">Rendering HTML</span></span>

<span data-ttu-id="b726f-109">Razor 語言預設為 HTML。</span><span class="sxs-lookup"><span data-stu-id="b726f-109">The default Razor language is HTML.</span></span> <span data-ttu-id="b726f-110">轉譯 Razor 標記中的 HTML 與轉譯 HTML 檔案中的 HTML 並無不同。</span><span class="sxs-lookup"><span data-stu-id="b726f-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="b726f-111">伺服器會依原狀轉譯 *.cshtml* Razor 檔案中的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="b726f-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="b726f-112">Razor 語法</span><span class="sxs-lookup"><span data-stu-id="b726f-112">Razor syntax</span></span>

<span data-ttu-id="b726f-113">Razor 支援 C#，並使用 `@` 符號從 HTML 轉換成 C#。</span><span class="sxs-lookup"><span data-stu-id="b726f-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="b726f-114">Razor 會評估 C# 運算式並轉譯成 HTML 輸出。</span><span class="sxs-lookup"><span data-stu-id="b726f-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="b726f-115">當 `@` 符號後面接著 [Razor 保留關鍵字](#razor-reserved-keywords)時，它會轉換成 Razor 特定標記；</span><span class="sxs-lookup"><span data-stu-id="b726f-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="b726f-116">否則會轉換成一般 C#。</span><span class="sxs-lookup"><span data-stu-id="b726f-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="b726f-117">若要將 Razor 標記中的 `@` 符號逸出，請使用第二個 `@` 符號：</span><span class="sxs-lookup"><span data-stu-id="b726f-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="b726f-118">此程式碼在 HTML 中是使用單一 `@` 符號轉譯：</span><span class="sxs-lookup"><span data-stu-id="b726f-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="b726f-119">HTML 屬性及含有電子郵件地址的內容不會將 `@` 符號視為轉換字元。</span><span class="sxs-lookup"><span data-stu-id="b726f-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="b726f-120">Razor 剖析不會處理下列範例中的電子郵件地址：</span><span class="sxs-lookup"><span data-stu-id="b726f-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="b726f-121">Razor 隱含運算式</span><span class="sxs-lookup"><span data-stu-id="b726f-121">Implicit Razor expressions</span></span>

<span data-ttu-id="b726f-122">Razor 隱含運算式會以 `@` 開頭，後面接著 C# 程式碼：</span><span class="sxs-lookup"><span data-stu-id="b726f-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="b726f-123">除了 C# `await` 關鍵字以外，隱含運算式不能包含空格。</span><span class="sxs-lookup"><span data-stu-id="b726f-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="b726f-124">如果 C# 陳述式具有明確的結束字元，則可以混合空格：</span><span class="sxs-lookup"><span data-stu-id="b726f-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="b726f-125">隱含運算式「不能」包含 C# 泛型，因為括弧 (`<>`) 內的字元會解譯為 HTML 標籤。</span><span class="sxs-lookup"><span data-stu-id="b726f-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="b726f-126">下列程式碼**無效**：</span><span class="sxs-lookup"><span data-stu-id="b726f-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="b726f-127">上述程式碼會產生類似下列其中一項的編譯器錯誤：</span><span class="sxs-lookup"><span data-stu-id="b726f-127">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="b726f-128">"Int" 項目未關閉。</span><span class="sxs-lookup"><span data-stu-id="b726f-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="b726f-129">所有項目都必須自行結束或具有相對應的結束標籤。</span><span class="sxs-lookup"><span data-stu-id="b726f-129">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="b726f-130">無法將方法群組 'GenericMethod' 轉換成非委派類型 'type'。</span><span class="sxs-lookup"><span data-stu-id="b726f-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="b726f-131">您是否想要叫用方法？</span><span class="sxs-lookup"><span data-stu-id="b726f-131">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="b726f-132">泛型方法呼叫必須包裝在 [Razor 明確運算式](#explicit-razor-expressions)或 [Razor 程式碼區塊](#razor-code-blocks)中。</span><span class="sxs-lookup"><span data-stu-id="b726f-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="b726f-133">Razor 明確運算式</span><span class="sxs-lookup"><span data-stu-id="b726f-133">Explicit Razor expressions</span></span>

<span data-ttu-id="b726f-134">Razor 明確運算式是由 `@` 符號與對稱的括弧所組成。</span><span class="sxs-lookup"><span data-stu-id="b726f-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="b726f-135">為了轉譯上週的時間，使用了下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="b726f-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="b726f-136">`@()` 括弧內的任何內容都會經過評估並轉譯成輸出。</span><span class="sxs-lookup"><span data-stu-id="b726f-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="b726f-137">上一節中所述的隱含運算式通常不能包含空格。</span><span class="sxs-lookup"><span data-stu-id="b726f-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="b726f-138">在下列程式碼中，不會從目前的時間減去一週：</span><span class="sxs-lookup"><span data-stu-id="b726f-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="b726f-139">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="b726f-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="b726f-140">明確運算式可用來串連文字與運算式結果：</span><span class="sxs-lookup"><span data-stu-id="b726f-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="b726f-141">若沒有明確運算式，`<p>Age@joe.Age</p>` 會視為電子郵件地址，並轉譯 `<p>Age@joe.Age</p>`。</span><span class="sxs-lookup"><span data-stu-id="b726f-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="b726f-142">當撰寫為明確運算式時，會轉譯 `<p>Age33</p>`。</span><span class="sxs-lookup"><span data-stu-id="b726f-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="b726f-143">您可以使用明確運算式，透過 *.cshtml* 檔案中的泛型方法轉譯輸出。</span><span class="sxs-lookup"><span data-stu-id="b726f-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="b726f-144">在隱含運算式中，括弧 (`<>`) 內的字元會解譯為 HTML 標籤。</span><span class="sxs-lookup"><span data-stu-id="b726f-144">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="b726f-145">下列標記**不是**有效的 Razor：</span><span class="sxs-lookup"><span data-stu-id="b726f-145">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="b726f-146">上述程式碼會產生類似下列其中一項的編譯器錯誤：</span><span class="sxs-lookup"><span data-stu-id="b726f-146">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="b726f-147">"Int" 項目未關閉。</span><span class="sxs-lookup"><span data-stu-id="b726f-147">The "int" element wasn't closed.</span></span> <span data-ttu-id="b726f-148">所有項目都必須自行結束或具有相對應的結束標籤。</span><span class="sxs-lookup"><span data-stu-id="b726f-148">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="b726f-149">無法將方法群組 'GenericMethod' 轉換成非委派類型 'type'。</span><span class="sxs-lookup"><span data-stu-id="b726f-149">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="b726f-150">您是否想要叫用方法？</span><span class="sxs-lookup"><span data-stu-id="b726f-150">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="b726f-151">下列標記顯示撰寫此程式碼的正確方式。</span><span class="sxs-lookup"><span data-stu-id="b726f-151">The following markup shows the correct way write this code.</span></span> <span data-ttu-id="b726f-152">程式碼會撰寫為明確運算式：</span><span class="sxs-lookup"><span data-stu-id="b726f-152">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="b726f-153">運算式編碼</span><span class="sxs-lookup"><span data-stu-id="b726f-153">Expression encoding</span></span>

<span data-ttu-id="b726f-154">評估為字串的 C# 運算式會以 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="b726f-154">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="b726f-155">評估為 `IHtmlContent` 的 C# 運算式會透過 `IHtmlContent.WriteTo` 直接轉譯。</span><span class="sxs-lookup"><span data-stu-id="b726f-155">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="b726f-156">未評估為 `IHtmlContent` 的 C# 運算式會由 `ToString` 轉換成字串，並經過編碼再轉譯。</span><span class="sxs-lookup"><span data-stu-id="b726f-156">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="b726f-157">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="b726f-157">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="b726f-158">此 HTML 在瀏覽器中會顯示為：</span><span class="sxs-lookup"><span data-stu-id="b726f-158">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="b726f-159">`HtmlHelper.Raw` 輸出不會經過編碼，而是轉譯為 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="b726f-159">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="b726f-160">對未清理的使用者輸入使用 `HtmlHelper.Raw` 會造成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="b726f-160">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="b726f-161">使用者輸入可能包含惡意的 JavaScript 或其他惡意探索。</span><span class="sxs-lookup"><span data-stu-id="b726f-161">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="b726f-162">清理使用者輸入並不容易。</span><span class="sxs-lookup"><span data-stu-id="b726f-162">Sanitizing user input is difficult.</span></span> <span data-ttu-id="b726f-163">請避免對使用者輸入使用 `HtmlHelper.Raw`。</span><span class="sxs-lookup"><span data-stu-id="b726f-163">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="b726f-164">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="b726f-164">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="b726f-165">Razor 程式碼區塊</span><span class="sxs-lookup"><span data-stu-id="b726f-165">Razor code blocks</span></span>

<span data-ttu-id="b726f-166">Razor 程式碼區塊會以 `@` 開頭，並以 `{}` 括住。</span><span class="sxs-lookup"><span data-stu-id="b726f-166">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="b726f-167">不同於運算式，程式碼區塊內的 C# 程式碼不會轉譯。</span><span class="sxs-lookup"><span data-stu-id="b726f-167">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="b726f-168">一個檢視中的程式碼區塊和運算式會共用相同的範圍並依序定義：</span><span class="sxs-lookup"><span data-stu-id="b726f-168">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="b726f-169">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="b726f-169">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="b726f-170">隱含轉換</span><span class="sxs-lookup"><span data-stu-id="b726f-170">Implicit transitions</span></span>

<span data-ttu-id="b726f-171">程式碼區塊中的預設語言是 C#，但 Razor Page 可以轉換回 HTML：</span><span class="sxs-lookup"><span data-stu-id="b726f-171">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="b726f-172">明確分隔的轉換</span><span class="sxs-lookup"><span data-stu-id="b726f-172">Explicit delimited transition</span></span>

<span data-ttu-id="b726f-173">若要定義程式碼區塊中應該轉譯 HTML 的子區段，請使用 Razor **\<text>** 標籤括住要轉譯的字元：</span><span class="sxs-lookup"><span data-stu-id="b726f-173">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="b726f-174">使用此方法可轉譯未以 HTML 標籤括住的 HTML。</span><span class="sxs-lookup"><span data-stu-id="b726f-174">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="b726f-175">若沒有 HTML 或 Razor 標籤，就會發生 Razor 執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="b726f-175">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="b726f-176">**\<text>** 標籤可用來控制轉譯內容時的空白字元：</span><span class="sxs-lookup"><span data-stu-id="b726f-176">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="b726f-177">**\<text>** 標籤之間的內容會轉譯。</span><span class="sxs-lookup"><span data-stu-id="b726f-177">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="b726f-178">**\<text>** 標籤前後不能有空白字元出現在 HTML 輸出中。</span><span class="sxs-lookup"><span data-stu-id="b726f-178">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="b726f-179">使用 @ 的明確行轉換：</span><span class="sxs-lookup"><span data-stu-id="b726f-179">Explicit Line Transition with @:</span></span>

<span data-ttu-id="b726f-180">若要將一整行的其餘部分轉譯為程式碼區塊內的 HTML，請使用 `@:` 語法：</span><span class="sxs-lookup"><span data-stu-id="b726f-180">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="b726f-181">若程式碼中沒有 `@:`，就會產生 Razor 執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="b726f-181">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="b726f-182">警告：Razor 檔案中的額外 `@` 字元可能會導致稍後在區塊的陳述式中發生編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="b726f-182">Warning: Extra `@` characters in a Razor file can cause cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="b726f-183">這些編譯器錯誤可能很難了解，因為實際錯誤發生在回報的錯誤之前。</span><span class="sxs-lookup"><span data-stu-id="b726f-183">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="b726f-184">將多個明確/隱含運算式合併成單一程式碼區塊之後，經常會出現此錯誤。</span><span class="sxs-lookup"><span data-stu-id="b726f-184">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="b726f-185">控制結構</span><span class="sxs-lookup"><span data-stu-id="b726f-185">Control Structures</span></span>

<span data-ttu-id="b726f-186">控制結構是程式碼區塊的延伸。</span><span class="sxs-lookup"><span data-stu-id="b726f-186">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="b726f-187">程式碼區塊的所有層面 (轉換成標記、內嵌 C#) 也適用於下列結構：</span><span class="sxs-lookup"><span data-stu-id="b726f-187">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="b726f-188">條件式 @if、else if、else 和 @switch</span><span class="sxs-lookup"><span data-stu-id="b726f-188">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="b726f-189">`@if` 控制何時執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="b726f-189">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="b726f-190">`else` 和 `else if` 不需要 `@` 符號：</span><span class="sxs-lookup"><span data-stu-id="b726f-190">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="b726f-191">下列標記示範如何使用 switch 陳述式：</span><span class="sxs-lookup"><span data-stu-id="b726f-191">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="b726f-192">迴圈 @for、@foreach、@while 和 @do while</span><span class="sxs-lookup"><span data-stu-id="b726f-192">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="b726f-193">樣板化 HTML 可以透過迴圈控制陳述式轉譯。</span><span class="sxs-lookup"><span data-stu-id="b726f-193">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="b726f-194">若要轉譯人員清單：</span><span class="sxs-lookup"><span data-stu-id="b726f-194">To render a list of people:</span></span>

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

<span data-ttu-id="b726f-195">支援的迴圈陳述式如下：</span><span class="sxs-lookup"><span data-stu-id="b726f-195">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="b726f-196">複合 @using</span><span class="sxs-lookup"><span data-stu-id="b726f-196">Compound @using</span></span>

<span data-ttu-id="b726f-197">在 C# 中，`using` 陳述式可用來確保物件經過處置。</span><span class="sxs-lookup"><span data-stu-id="b726f-197">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="b726f-198">在 Razor 中，使用相同的機制來建立 HTML 協助程式，以包含其他內容。</span><span class="sxs-lookup"><span data-stu-id="b726f-198">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="b726f-199">在下列程式碼中，HTML 協助程式使用 `@using` 陳述式來轉譯表單標籤：</span><span class="sxs-lookup"><span data-stu-id="b726f-199">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


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

<span data-ttu-id="b726f-200">範圍層級動作可以透過[標籤協助程式](xref:mvc/views/tag-helpers/intro)來執行。</span><span class="sxs-lookup"><span data-stu-id="b726f-200">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="b726f-201">@try、catch、finally</span><span class="sxs-lookup"><span data-stu-id="b726f-201">@try, catch, finally</span></span>

<span data-ttu-id="b726f-202">例外狀況處理會類似於 C#：</span><span class="sxs-lookup"><span data-stu-id="b726f-202">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="b726f-203">Razor 可以使用 lock 陳述式來保護關鍵區段：</span><span class="sxs-lookup"><span data-stu-id="b726f-203">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="b726f-204">註解</span><span class="sxs-lookup"><span data-stu-id="b726f-204">Comments</span></span>

<span data-ttu-id="b726f-205">Razor 支援 C# 和 HTML 註解：</span><span class="sxs-lookup"><span data-stu-id="b726f-205">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="b726f-206">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="b726f-206">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="b726f-207">伺服器會先移除 Razor 註解，再轉譯網頁。</span><span class="sxs-lookup"><span data-stu-id="b726f-207">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="b726f-208">Razor 使用 `@*  *@` 來分隔註解。</span><span class="sxs-lookup"><span data-stu-id="b726f-208">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="b726f-209">下列程式碼會標記為註解，以確保伺服器不會轉譯任何標記：</span><span class="sxs-lookup"><span data-stu-id="b726f-209">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="b726f-210">指示詞</span><span class="sxs-lookup"><span data-stu-id="b726f-210">Directives</span></span>

<span data-ttu-id="b726f-211">Razor 指示詞是以隱含運算式表示，這些運算式具有保留關鍵字，後面接著 `@` 符號。</span><span class="sxs-lookup"><span data-stu-id="b726f-211">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="b726f-212">指示詞通常會變更檢視的剖析方式或啟用不同的功能。</span><span class="sxs-lookup"><span data-stu-id="b726f-212">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="b726f-213">了解 Razor 如何針對檢視產生程式碼，可讓您更容易了解指示詞的運作方式。</span><span class="sxs-lookup"><span data-stu-id="b726f-213">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="b726f-214">程式碼會產生類似如下的類別：</span><span class="sxs-lookup"><span data-stu-id="b726f-214">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="b726f-215">本文稍後的[檢視針對檢視所產生的 Razor C# 類別](#viewing-the-razor-c-class-generated-for-a-view)一節將說明如何檢視此產生的類別。</span><span class="sxs-lookup"><span data-stu-id="b726f-215">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="b726f-216">`@using` 指示詞會將 C# `using` 指示詞新增至產生的檢視：</span><span class="sxs-lookup"><span data-stu-id="b726f-216">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="b726f-217">`@model` 指示詞會指定傳遞至檢視的模型類型：</span><span class="sxs-lookup"><span data-stu-id="b726f-217">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="b726f-218">在使用個別使用者帳戶建立的 ASP.NET Core MVC 應用程式中，*Views/Account/Login.cshtml* 檢視包含下列模型宣告：</span><span class="sxs-lookup"><span data-stu-id="b726f-218">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="b726f-219">產生的類別繼承自 `RazorPage<dynamic>`：</span><span class="sxs-lookup"><span data-stu-id="b726f-219">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="b726f-220">Razor 會公開 `Model` 屬性，以存取傳遞至檢視的模型：</span><span class="sxs-lookup"><span data-stu-id="b726f-220">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="b726f-221">`@model` 指示詞會指定此屬性的類型。</span><span class="sxs-lookup"><span data-stu-id="b726f-221">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="b726f-222">該指示詞會將 `RazorPage<T>` 中的 `T` 指定為檢視從中衍生的產生類別。</span><span class="sxs-lookup"><span data-stu-id="b726f-222">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="b726f-223">若未指定 `@model` 指示詞，`Model` 屬性的類型為 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="b726f-223">If the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="b726f-224">模型的值會從控制器傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="b726f-224">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="b726f-225">如需詳細資訊，請參閱＜強型別模型和 @model 關鍵字＞。</span><span class="sxs-lookup"><span data-stu-id="b726f-225">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="b726f-226">`@inherits` 指示詞可讓您完全控制檢視所繼承的類別：</span><span class="sxs-lookup"><span data-stu-id="b726f-226">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="b726f-227">下列程式碼是自訂 Razor Page 類型：</span><span class="sxs-lookup"><span data-stu-id="b726f-227">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="b726f-228">`CustomText` 會顯示在檢視中：</span><span class="sxs-lookup"><span data-stu-id="b726f-228">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="b726f-229">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="b726f-229">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="b726f-230">`@model` 和 `@inherits` 可用於相同的檢視。</span><span class="sxs-lookup"><span data-stu-id="b726f-230">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="b726f-231">`@inherits` 可能位於檢視匯入的 *_ViewImports.cshtml* 檔案中：</span><span class="sxs-lookup"><span data-stu-id="b726f-231">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="b726f-232">下列程式碼是強型別檢視的範例：</span><span class="sxs-lookup"><span data-stu-id="b726f-232">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="b726f-233">如果模型中傳遞了 "rick@contoso.com"，檢視會產生下列 HTML 標記：</span><span class="sxs-lookup"><span data-stu-id="b726f-233">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="b726f-234">`@inject` 指示詞可讓 Razor Page 從[服務容器](xref:fundamentals/dependency-injection)將服務插入至檢視。</span><span class="sxs-lookup"><span data-stu-id="b726f-234">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="b726f-235">如需詳細資訊，請參閱[在檢視中插入相依性](xref:mvc/views/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="b726f-235">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="b726f-236">`@functions` 指示詞可讓 Razor Page 將函式階層內容新增至檢視：</span><span class="sxs-lookup"><span data-stu-id="b726f-236">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="b726f-237">例如: </span><span class="sxs-lookup"><span data-stu-id="b726f-237">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="b726f-238">程式碼會產生下列 HTML 標記：</span><span class="sxs-lookup"><span data-stu-id="b726f-238">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="b726f-239">下列程式碼是產生的 Razor C# 類別：</span><span class="sxs-lookup"><span data-stu-id="b726f-239">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="b726f-240">`@section` 指示詞可搭配[配置](xref:mvc/views/layout)使用，讓檢視可以轉譯 HTML 頁面中不同部分的內容。</span><span class="sxs-lookup"><span data-stu-id="b726f-240">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="b726f-241">如需詳細資訊，請參閱[區段](xref:mvc/views/layout#layout-sections-label)。</span><span class="sxs-lookup"><span data-stu-id="b726f-241">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="b726f-242">標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="b726f-242">Tag Helpers</span></span>

<span data-ttu-id="b726f-243">[標籤協助程式](xref:mvc/views/tag-helpers/intro)有三個相關的指示詞。</span><span class="sxs-lookup"><span data-stu-id="b726f-243">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="b726f-244">指示詞</span><span class="sxs-lookup"><span data-stu-id="b726f-244">Directive</span></span> | <span data-ttu-id="b726f-245">功能</span><span class="sxs-lookup"><span data-stu-id="b726f-245">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="b726f-246">使標籤協助程式可供檢視。</span><span class="sxs-lookup"><span data-stu-id="b726f-246">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="b726f-247">移除先前從檢視新增的標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="b726f-247">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="b726f-248">指定標籤前置字元，以啟用標籤協助程式支援，並將標籤協助程式使用方式設為明確。</span><span class="sxs-lookup"><span data-stu-id="b726f-248">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="b726f-249">Razor 保留關鍵字</span><span class="sxs-lookup"><span data-stu-id="b726f-249">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="b726f-250">Razor 關鍵字</span><span class="sxs-lookup"><span data-stu-id="b726f-250">Razor keywords</span></span>

* <span data-ttu-id="b726f-251">page (需要 ASP.NET Core 2.0 和更新版本)</span><span class="sxs-lookup"><span data-stu-id="b726f-251">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="b726f-252">函式</span><span class="sxs-lookup"><span data-stu-id="b726f-252">functions</span></span>
* <span data-ttu-id="b726f-253">繼承</span><span class="sxs-lookup"><span data-stu-id="b726f-253">inherits</span></span>
* <span data-ttu-id="b726f-254">model</span><span class="sxs-lookup"><span data-stu-id="b726f-254">model</span></span>
* <span data-ttu-id="b726f-255">section</span><span class="sxs-lookup"><span data-stu-id="b726f-255">section</span></span>
* <span data-ttu-id="b726f-256">helper (ASP.NET Core 目前不支援)</span><span class="sxs-lookup"><span data-stu-id="b726f-256">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="b726f-257">Razor 關鍵字會使用 `@(Razor Keyword)` (例如 `@(functions)`) 逸出。</span><span class="sxs-lookup"><span data-stu-id="b726f-257">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="b726f-258">C# Razor 關鍵字</span><span class="sxs-lookup"><span data-stu-id="b726f-258">C# Razor keywords</span></span>

* <span data-ttu-id="b726f-259">大小寫</span><span class="sxs-lookup"><span data-stu-id="b726f-259">case</span></span>
* <span data-ttu-id="b726f-260">do</span><span class="sxs-lookup"><span data-stu-id="b726f-260">do</span></span>
* <span data-ttu-id="b726f-261">default</span><span class="sxs-lookup"><span data-stu-id="b726f-261">default</span></span>
* <span data-ttu-id="b726f-262">for</span><span class="sxs-lookup"><span data-stu-id="b726f-262">for</span></span>
* <span data-ttu-id="b726f-263">foreach</span><span class="sxs-lookup"><span data-stu-id="b726f-263">foreach</span></span>
* <span data-ttu-id="b726f-264">if</span><span class="sxs-lookup"><span data-stu-id="b726f-264">if</span></span>
* <span data-ttu-id="b726f-265">else</span><span class="sxs-lookup"><span data-stu-id="b726f-265">else</span></span>
* <span data-ttu-id="b726f-266">lock</span><span class="sxs-lookup"><span data-stu-id="b726f-266">lock</span></span>
* <span data-ttu-id="b726f-267">switch</span><span class="sxs-lookup"><span data-stu-id="b726f-267">switch</span></span>
* <span data-ttu-id="b726f-268">try</span><span class="sxs-lookup"><span data-stu-id="b726f-268">try</span></span>
* <span data-ttu-id="b726f-269">catch</span><span class="sxs-lookup"><span data-stu-id="b726f-269">catch</span></span>
* <span data-ttu-id="b726f-270">finally</span><span class="sxs-lookup"><span data-stu-id="b726f-270">finally</span></span>
* <span data-ttu-id="b726f-271">使用</span><span class="sxs-lookup"><span data-stu-id="b726f-271">using</span></span>
* <span data-ttu-id="b726f-272">while</span><span class="sxs-lookup"><span data-stu-id="b726f-272">while</span></span>

<span data-ttu-id="b726f-273">C# Razor 關鍵字必須使用 `@(@C# Razor Keyword)` (例如 `@(@case)`) 雙重逸出。</span><span class="sxs-lookup"><span data-stu-id="b726f-273">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="b726f-274">第一個 `@` 會將 Razor 剖析器逸出。</span><span class="sxs-lookup"><span data-stu-id="b726f-274">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="b726f-275">第二個 `@` 會將 C# 剖析器逸出。</span><span class="sxs-lookup"><span data-stu-id="b726f-275">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="b726f-276">Razor 未使用的保留關鍵字</span><span class="sxs-lookup"><span data-stu-id="b726f-276">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="b726f-277">namespace</span><span class="sxs-lookup"><span data-stu-id="b726f-277">namespace</span></span>
* <span data-ttu-id="b726f-278">Class - 類別</span><span class="sxs-lookup"><span data-stu-id="b726f-278">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="b726f-279">檢視針對檢視所產生的 Razor C# 類別</span><span class="sxs-lookup"><span data-stu-id="b726f-279">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="b726f-280">請將下列類別新增至 ASP.NET Core MVC 專案：</span><span class="sxs-lookup"><span data-stu-id="b726f-280">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="b726f-281">以 `CustomTemplateEngine` 類別覆寫 MVC 所新增的 `RazorTemplateEngine`：</span><span class="sxs-lookup"><span data-stu-id="b726f-281">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="b726f-282">在 `CustomTemplateEngine` 的 `return csharpDocument` 陳述式上設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="b726f-282">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="b726f-283">當程式在中斷點停止執行時，請檢視 `generatedCode` 的值。</span><span class="sxs-lookup"><span data-stu-id="b726f-283">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![generatedCode 的文字視覺化檢視](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="b726f-285">檢視查閱和區分大小寫</span><span class="sxs-lookup"><span data-stu-id="b726f-285">View lookups and case sensitivity</span></span>

<span data-ttu-id="b726f-286">Razor 檢視引擎會針對檢視執行區分大小寫的查閱。</span><span class="sxs-lookup"><span data-stu-id="b726f-286">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="b726f-287">不過，實際查閱則取決於基礎檔案系統：</span><span class="sxs-lookup"><span data-stu-id="b726f-287">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="b726f-288">檔案式來源：</span><span class="sxs-lookup"><span data-stu-id="b726f-288">File based source:</span></span> 
  * <span data-ttu-id="b726f-289">在具有不區分大小寫之檔案系統的作業系統上 (例如 Windows)，實體檔案提供者查閱不會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="b726f-289">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="b726f-290">例如，`return View("Test")` 針對 */Views/Home/Test.cshtml* 和 */Views/home/test.cshtml* (以及任何其他大小寫變體) 會有相符的結果。</span><span class="sxs-lookup"><span data-stu-id="b726f-290">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="b726f-291">在區分大小寫的檔案系統上 (例如 Linux、OSX 及使用 `EmbeddedFileProvider`)，查閱會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="b726f-291">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="b726f-292">例如，`return View("Test")` 會明確符合 */Views/Home/Test.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="b726f-292">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="b726f-293">先行編譯的檢視：在 ASP.NET Core 2.0 和更新版本中，在所有作業系統上查閱先行編譯的檢視不會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="b726f-293">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="b726f-294">此行為與 Windows 上之實體檔案提供者的行為相同。</span><span class="sxs-lookup"><span data-stu-id="b726f-294">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="b726f-295">如果兩個先行編譯的檢視只有大小寫不同，查閱的結果不會由此決定。</span><span class="sxs-lookup"><span data-stu-id="b726f-295">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="b726f-296">建議開發人員比對檔案和目錄的大小寫以及下列項目的大小寫：</span><span class="sxs-lookup"><span data-stu-id="b726f-296">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="b726f-297">區域、控制器和動作名稱。</span><span class="sxs-lookup"><span data-stu-id="b726f-297">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="b726f-298">Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="b726f-298">Razor Pages.</span></span>
    
<span data-ttu-id="b726f-299">比對大小寫可確保不論基礎檔案系統為何，部署作業都能夠找到其值。</span><span class="sxs-lookup"><span data-stu-id="b726f-299">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
