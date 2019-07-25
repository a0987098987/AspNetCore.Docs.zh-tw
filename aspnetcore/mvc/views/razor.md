---
title: ASP.NET Core 的 Razor 語法參考
author: rick-anderson
description: 了解將伺服器架構程式碼內嵌到網頁中的 Razor 標記語法。
ms.author: riande
ms.date: 06/12/2019
uid: mvc/views/razor
ms.openlocfilehash: 634623aa6df860a2ff728da4e65d277326db37e2
ms.sourcegitcommit: 051f068c78931432e030b60094c38376d64d013e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440361"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="53d98-103">ASP.NET Core 的 Razor 語法參考</span><span class="sxs-lookup"><span data-stu-id="53d98-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="53d98-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Luke Latham](https://github.com/guardrex)、[Taylor Mullen](https://twitter.com/ntaylormullen) 與 [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="53d98-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="53d98-105">Razor 是將伺服器架構程式碼內嵌到網頁中的標記語法。</span><span class="sxs-lookup"><span data-stu-id="53d98-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="53d98-106">Razor 語法是由 Razor 標記、C# 和 HTML 所組成。</span><span class="sxs-lookup"><span data-stu-id="53d98-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="53d98-107">含有 Razor 的檔案通常具有 *.cshtml* 副檔名。</span><span class="sxs-lookup"><span data-stu-id="53d98-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="53d98-108">轉譯 HTML</span><span class="sxs-lookup"><span data-stu-id="53d98-108">Rendering HTML</span></span>

<span data-ttu-id="53d98-109">Razor 語言預設為 HTML。</span><span class="sxs-lookup"><span data-stu-id="53d98-109">The default Razor language is HTML.</span></span> <span data-ttu-id="53d98-110">轉譯 Razor 標記中的 HTML 與轉譯 HTML 檔案中的 HTML 並無不同。</span><span class="sxs-lookup"><span data-stu-id="53d98-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="53d98-111">伺服器會依原狀轉譯 *.cshtml* Razor 檔案中的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="53d98-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="53d98-112">Razor 語法</span><span class="sxs-lookup"><span data-stu-id="53d98-112">Razor syntax</span></span>

<span data-ttu-id="53d98-113">Razor 支援 C#，並使用 `@` 符號從 HTML 轉換成 C#。</span><span class="sxs-lookup"><span data-stu-id="53d98-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="53d98-114">Razor 會評估 C# 運算式並轉譯成 HTML 輸出。</span><span class="sxs-lookup"><span data-stu-id="53d98-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="53d98-115">當 `@` 符號後面接著 [Razor 保留關鍵字](#razor-reserved-keywords)時，它會轉換成 Razor 特定標記；</span><span class="sxs-lookup"><span data-stu-id="53d98-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="53d98-116">否則會轉換成一般 C#。</span><span class="sxs-lookup"><span data-stu-id="53d98-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="53d98-117">若要將 Razor 標記中的 `@` 符號逸出，請使用第二個 `@` 符號：</span><span class="sxs-lookup"><span data-stu-id="53d98-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="53d98-118">此程式碼在 HTML 中是使用單一 `@` 符號轉譯：</span><span class="sxs-lookup"><span data-stu-id="53d98-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="53d98-119">HTML 屬性及含有電子郵件地址的內容不會將 `@` 符號視為轉換字元。</span><span class="sxs-lookup"><span data-stu-id="53d98-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="53d98-120">Razor 剖析不會處理下列範例中的電子郵件地址：</span><span class="sxs-lookup"><span data-stu-id="53d98-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="53d98-121">Razor 隱含運算式</span><span class="sxs-lookup"><span data-stu-id="53d98-121">Implicit Razor expressions</span></span>

<span data-ttu-id="53d98-122">Razor 隱含運算式會以 `@` 開頭，後面接著 C# 程式碼：</span><span class="sxs-lookup"><span data-stu-id="53d98-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="53d98-123">除了 C# `await` 關鍵字以外，隱含運算式不能包含空格。</span><span class="sxs-lookup"><span data-stu-id="53d98-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="53d98-124">如果 C# 陳述式具有明確的結束字元，則可以混合空格：</span><span class="sxs-lookup"><span data-stu-id="53d98-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="53d98-125">隱含運算式「不能」  包含 C# 泛型，因為括弧 (`<>`) 內的字元會解譯為 HTML 標籤。</span><span class="sxs-lookup"><span data-stu-id="53d98-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="53d98-126">下列程式碼**無效**：</span><span class="sxs-lookup"><span data-stu-id="53d98-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="53d98-127">上述程式碼會產生類似下列其中一項的編譯器錯誤：</span><span class="sxs-lookup"><span data-stu-id="53d98-127">The preceding code generates a compiler error similar to one of the following:</span></span>

* <span data-ttu-id="53d98-128">"Int" 項目未關閉。</span><span class="sxs-lookup"><span data-stu-id="53d98-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="53d98-129">所有項目都必須自行結束或具有相對應的結束標籤。</span><span class="sxs-lookup"><span data-stu-id="53d98-129">All elements must be either self-closing or have a matching end tag.</span></span>
* <span data-ttu-id="53d98-130">無法將方法群組 'GenericMethod' 轉換成非委派類型 'type'。</span><span class="sxs-lookup"><span data-stu-id="53d98-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="53d98-131">您是否想要叫用方法？</span><span class="sxs-lookup"><span data-stu-id="53d98-131">Did you intend to invoke the method?\`</span></span>

<span data-ttu-id="53d98-132">泛型方法呼叫必須包裝在 [Razor 明確運算式](#explicit-razor-expressions)或 [Razor 程式碼區塊](#razor-code-blocks)中。</span><span class="sxs-lookup"><span data-stu-id="53d98-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="53d98-133">Razor 明確運算式</span><span class="sxs-lookup"><span data-stu-id="53d98-133">Explicit Razor expressions</span></span>

<span data-ttu-id="53d98-134">Razor 明確運算式是由 `@` 符號與對稱的括弧所組成。</span><span class="sxs-lookup"><span data-stu-id="53d98-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="53d98-135">為了轉譯上週的時間，使用了下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="53d98-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="53d98-136">`@()` 括弧內的任何內容都會經過評估並轉譯成輸出。</span><span class="sxs-lookup"><span data-stu-id="53d98-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="53d98-137">上一節中所述的隱含運算式通常不能包含空格。</span><span class="sxs-lookup"><span data-stu-id="53d98-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="53d98-138">在下列程式碼中，不會從目前的時間減去一週：</span><span class="sxs-lookup"><span data-stu-id="53d98-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="53d98-139">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="53d98-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="53d98-140">明確運算式可用來串連文字與運算式結果：</span><span class="sxs-lookup"><span data-stu-id="53d98-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="53d98-141">若沒有明確運算式，`<p>Age@joe.Age</p>` 會視為電子郵件地址，並轉譯 `<p>Age@joe.Age</p>`。</span><span class="sxs-lookup"><span data-stu-id="53d98-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="53d98-142">當撰寫為明確運算式時，會轉譯 `<p>Age33</p>`。</span><span class="sxs-lookup"><span data-stu-id="53d98-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="53d98-143">您可以使用明確運算式，透過 *.cshtml* 檔案中的泛型方法轉譯輸出。</span><span class="sxs-lookup"><span data-stu-id="53d98-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="53d98-144">下列標記會顯示如何修正稍早顯示的錯誤，此錯誤是由 C# 泛型的括弧所造成。</span><span class="sxs-lookup"><span data-stu-id="53d98-144">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="53d98-145">程式碼會撰寫為明確運算式：</span><span class="sxs-lookup"><span data-stu-id="53d98-145">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="53d98-146">運算式編碼</span><span class="sxs-lookup"><span data-stu-id="53d98-146">Expression encoding</span></span>

<span data-ttu-id="53d98-147">評估為字串的 C# 運算式會以 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="53d98-147">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="53d98-148">評估為 `IHtmlContent` 的 C# 運算式會透過 `IHtmlContent.WriteTo` 直接轉譯。</span><span class="sxs-lookup"><span data-stu-id="53d98-148">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="53d98-149">未評估為 `IHtmlContent` 的 C# 運算式會由 `ToString` 轉換成字串，並經過編碼再轉譯。</span><span class="sxs-lookup"><span data-stu-id="53d98-149">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="53d98-150">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="53d98-150">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="53d98-151">此 HTML 在瀏覽器中會顯示為：</span><span class="sxs-lookup"><span data-stu-id="53d98-151">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="53d98-152">`HtmlHelper.Raw` 輸出不會經過編碼，而是轉譯為 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="53d98-152">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="53d98-153">對未清理的使用者輸入使用 `HtmlHelper.Raw` 會造成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="53d98-153">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="53d98-154">使用者輸入可能包含惡意的 JavaScript 或其他惡意探索。</span><span class="sxs-lookup"><span data-stu-id="53d98-154">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="53d98-155">清理使用者輸入並不容易。</span><span class="sxs-lookup"><span data-stu-id="53d98-155">Sanitizing user input is difficult.</span></span> <span data-ttu-id="53d98-156">請避免對使用者輸入使用 `HtmlHelper.Raw`。</span><span class="sxs-lookup"><span data-stu-id="53d98-156">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="53d98-157">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="53d98-157">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="53d98-158">Razor 程式碼區塊</span><span class="sxs-lookup"><span data-stu-id="53d98-158">Razor code blocks</span></span>

<span data-ttu-id="53d98-159">Razor 程式碼區塊會以 `@` 開頭，並以 `{}` 括住。</span><span class="sxs-lookup"><span data-stu-id="53d98-159">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="53d98-160">不同於運算式，程式碼區塊內的 C# 程式碼不會轉譯。</span><span class="sxs-lookup"><span data-stu-id="53d98-160">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="53d98-161">一個檢視中的程式碼區塊和運算式會共用相同的範圍並依序定義：</span><span class="sxs-lookup"><span data-stu-id="53d98-161">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="53d98-162">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="53d98-162">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="53d98-163">在程式碼區塊中，使用標記宣告[區域函式](/dotnet/csharp/programming-guide/classes-and-structs/local-functions)作為樣板化方法：</span><span class="sxs-lookup"><span data-stu-id="53d98-163">In code blocks, declare [local functions](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) with markup to serve as templating methods:</span></span>

```cshtml
@{
    void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }

    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}
```

<span data-ttu-id="53d98-164">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="53d98-164">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="implicit-transitions"></a><span data-ttu-id="53d98-165">隱含轉換</span><span class="sxs-lookup"><span data-stu-id="53d98-165">Implicit transitions</span></span>

<span data-ttu-id="53d98-166">程式碼區塊中的預設語言是 C#，但 Razor Page 可以轉換回 HTML：</span><span class="sxs-lookup"><span data-stu-id="53d98-166">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="53d98-167">明確分隔的轉換</span><span class="sxs-lookup"><span data-stu-id="53d98-167">Explicit delimited transition</span></span>

<span data-ttu-id="53d98-168">若要定義程式碼區塊中應該轉譯 HTML 的子區段，請使用 Razor **\<text>** 標籤括住要轉譯的字元：</span><span class="sxs-lookup"><span data-stu-id="53d98-168">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="53d98-169">使用此方法可轉譯未以 HTML 標籤括住的 HTML。</span><span class="sxs-lookup"><span data-stu-id="53d98-169">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="53d98-170">若沒有 HTML 或 Razor 標籤，就會發生 Razor 執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="53d98-170">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="53d98-171">**\<text>** 標籤可用來控制轉譯內容時的空白字元：</span><span class="sxs-lookup"><span data-stu-id="53d98-171">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="53d98-172">**\<text>** 標籤之間的內容會轉譯。</span><span class="sxs-lookup"><span data-stu-id="53d98-172">Only the content between the **\<text>** tag is rendered.</span></span>
* <span data-ttu-id="53d98-173">**\<text>** 標籤前後不能有空白字元出現在 HTML 輸出中。</span><span class="sxs-lookup"><span data-stu-id="53d98-173">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="53d98-174">使用 @ 的明確行轉換：</span><span class="sxs-lookup"><span data-stu-id="53d98-174">Explicit Line Transition with @:</span></span>

<span data-ttu-id="53d98-175">若要將一整行的其餘部分轉譯為程式碼區塊內的 HTML，請使用 `@:` 語法：</span><span class="sxs-lookup"><span data-stu-id="53d98-175">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="53d98-176">若程式碼中沒有 `@:`，就會產生 Razor 執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="53d98-176">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="53d98-177">警告：Razor 檔案中的額外 `@` 字元可能會導致稍後在區塊的陳述式中發生編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="53d98-177">Warning: Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="53d98-178">這些編譯器錯誤可能很難了解，因為實際錯誤發生在回報的錯誤之前。</span><span class="sxs-lookup"><span data-stu-id="53d98-178">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="53d98-179">將多個明確/隱含運算式合併成單一程式碼區塊之後，經常會出現此錯誤。</span><span class="sxs-lookup"><span data-stu-id="53d98-179">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="53d98-180">控制結構</span><span class="sxs-lookup"><span data-stu-id="53d98-180">Control structures</span></span>

<span data-ttu-id="53d98-181">控制結構是程式碼區塊的延伸。</span><span class="sxs-lookup"><span data-stu-id="53d98-181">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="53d98-182">程式碼區塊的所有層面 (轉換成標記、內嵌 C#) 也適用於下列結構：</span><span class="sxs-lookup"><span data-stu-id="53d98-182">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="53d98-183">條件式 @if、else if、else 和 @switch</span><span class="sxs-lookup"><span data-stu-id="53d98-183">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="53d98-184">`@if` 控制何時執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="53d98-184">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="53d98-185">`else` 和 `else if` 不需要 `@` 符號：</span><span class="sxs-lookup"><span data-stu-id="53d98-185">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="53d98-186">下列標記示範如何使用 switch 陳述式：</span><span class="sxs-lookup"><span data-stu-id="53d98-186">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="53d98-187">迴圈 @for、@foreach、@while 和 @do while</span><span class="sxs-lookup"><span data-stu-id="53d98-187">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="53d98-188">樣板化 HTML 可以透過迴圈控制陳述式轉譯。</span><span class="sxs-lookup"><span data-stu-id="53d98-188">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="53d98-189">若要轉譯人員清單：</span><span class="sxs-lookup"><span data-stu-id="53d98-189">To render a list of people:</span></span>

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

<span data-ttu-id="53d98-190">支援的迴圈陳述式如下：</span><span class="sxs-lookup"><span data-stu-id="53d98-190">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="53d98-191">複合 @using</span><span class="sxs-lookup"><span data-stu-id="53d98-191">Compound @using</span></span>

<span data-ttu-id="53d98-192">在 C# 中，`using` 陳述式可用來確保物件經過處置。</span><span class="sxs-lookup"><span data-stu-id="53d98-192">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="53d98-193">在 Razor 中，使用相同的機制來建立 HTML 協助程式，以包含其他內容。</span><span class="sxs-lookup"><span data-stu-id="53d98-193">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="53d98-194">在下列程式碼中，HTML 協助程式使用 `@using` 陳述式來轉譯表單標籤：</span><span class="sxs-lookup"><span data-stu-id="53d98-194">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>

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

<span data-ttu-id="53d98-195">範圍層級動作可以透過[標籤協助程式](xref:mvc/views/tag-helpers/intro)來執行。</span><span class="sxs-lookup"><span data-stu-id="53d98-195">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="53d98-196">@try、catch、finally</span><span class="sxs-lookup"><span data-stu-id="53d98-196">@try, catch, finally</span></span>

<span data-ttu-id="53d98-197">例外狀況處理會類似於 C#：</span><span class="sxs-lookup"><span data-stu-id="53d98-197">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="53d98-198">Razor 可以使用 lock 陳述式來保護關鍵區段：</span><span class="sxs-lookup"><span data-stu-id="53d98-198">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="53d98-199">註解</span><span class="sxs-lookup"><span data-stu-id="53d98-199">Comments</span></span>

<span data-ttu-id="53d98-200">Razor 支援 C# 和 HTML 註解：</span><span class="sxs-lookup"><span data-stu-id="53d98-200">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="53d98-201">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="53d98-201">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="53d98-202">伺服器會先移除 Razor 註解，再轉譯網頁。</span><span class="sxs-lookup"><span data-stu-id="53d98-202">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="53d98-203">Razor 使用 `@*  *@` 來分隔註解。</span><span class="sxs-lookup"><span data-stu-id="53d98-203">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="53d98-204">下列程式碼會標記為註解，以確保伺服器不會轉譯任何標記：</span><span class="sxs-lookup"><span data-stu-id="53d98-204">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="53d98-205">指示詞</span><span class="sxs-lookup"><span data-stu-id="53d98-205">Directives</span></span>

<span data-ttu-id="53d98-206">Razor 指示詞是以隱含運算式表示，這些運算式具有保留關鍵字，後面接著 `@` 符號。</span><span class="sxs-lookup"><span data-stu-id="53d98-206">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="53d98-207">指示詞通常會變更檢視的剖析方式或啟用不同的功能。</span><span class="sxs-lookup"><span data-stu-id="53d98-207">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="53d98-208">了解 Razor 如何針對檢視產生程式碼，可讓您更容易了解指示詞的運作方式。</span><span class="sxs-lookup"><span data-stu-id="53d98-208">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="53d98-209">程式碼會產生類似如下的類別：</span><span class="sxs-lookup"><span data-stu-id="53d98-209">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="53d98-210">本文稍後在[檢查針對檢視所產生的 Razor C# 類別](#inspect-the-razor-c-class-generated-for-a-view)一節中說明如何檢視這個產生的類別。</span><span class="sxs-lookup"><span data-stu-id="53d98-210">Later in this article, the section [Inspect the Razor C# class generated for a view](#inspect-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

<a name="using"></a>

### <a name="using"></a>@using

<span data-ttu-id="53d98-211">`@using` 指示詞會將 C# `using` 指示詞新增至產生的檢視：</span><span class="sxs-lookup"><span data-stu-id="53d98-211">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="53d98-212">`@model` 指示詞會指定傳遞至檢視的模型類型：</span><span class="sxs-lookup"><span data-stu-id="53d98-212">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="53d98-213">在使用個別使用者帳戶建立的 ASP.NET Core MVC 應用程式中，*Views/Account/Login.cshtml* 檢視包含下列模型宣告：</span><span class="sxs-lookup"><span data-stu-id="53d98-213">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="53d98-214">產生的類別繼承自 `RazorPage<dynamic>`：</span><span class="sxs-lookup"><span data-stu-id="53d98-214">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="53d98-215">Razor 會公開 `Model` 屬性，以存取傳遞至檢視的模型：</span><span class="sxs-lookup"><span data-stu-id="53d98-215">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="53d98-216">`@model` 指示詞會指定此屬性的類型。</span><span class="sxs-lookup"><span data-stu-id="53d98-216">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="53d98-217">該指示詞會將 `RazorPage<T>` 中的 `T` 指定為檢視從中衍生的產生類別。</span><span class="sxs-lookup"><span data-stu-id="53d98-217">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="53d98-218">若未指定 `@model` 指示詞，`Model` 屬性的類型為 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="53d98-218">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="53d98-219">模型的值會從控制器傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="53d98-219">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="53d98-220">如需詳細資訊，請參閱[強型別模型和 &commat;model 關鍵字](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword)。</span><span class="sxs-lookup"><span data-stu-id="53d98-220">For more information, see [Strongly typed models and the &commat;model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="53d98-221">`@inherits` 指示詞可讓您完全控制檢視所繼承的類別：</span><span class="sxs-lookup"><span data-stu-id="53d98-221">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="53d98-222">下列程式碼是自訂 Razor Page 類型：</span><span class="sxs-lookup"><span data-stu-id="53d98-222">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="53d98-223">`CustomText` 會顯示在檢視中：</span><span class="sxs-lookup"><span data-stu-id="53d98-223">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="53d98-224">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="53d98-224">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="53d98-225">`@model` 和 `@inherits` 可用於相同的檢視。</span><span class="sxs-lookup"><span data-stu-id="53d98-225">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="53d98-226">`@inherits` 可能位於檢視匯入的 *_ViewImports.cshtml* 檔案中：</span><span class="sxs-lookup"><span data-stu-id="53d98-226">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="53d98-227">下列程式碼是強型別檢視的範例：</span><span class="sxs-lookup"><span data-stu-id="53d98-227">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="53d98-228">如果模型中傳遞了 "rick@contoso.com"，檢視會產生下列 HTML 標記：</span><span class="sxs-lookup"><span data-stu-id="53d98-228">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

<span data-ttu-id="53d98-229">`@inject` 指示詞可讓 Razor Page 從[服務容器](xref:fundamentals/dependency-injection)將服務插入至檢視。</span><span class="sxs-lookup"><span data-stu-id="53d98-229">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="53d98-230">如需詳細資訊，請參閱[在檢視中插入相依性](xref:mvc/views/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="53d98-230">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="53d98-231">`@functions` 指示詞可讓 Razor 頁面將 C# 程式碼區塊新增至檢視：</span><span class="sxs-lookup"><span data-stu-id="53d98-231">The `@functions` directive enables a Razor Page to add a C# code block to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="53d98-232">例如：</span><span class="sxs-lookup"><span data-stu-id="53d98-232">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="53d98-233">程式碼會產生下列 HTML 標記：</span><span class="sxs-lookup"><span data-stu-id="53d98-233">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="53d98-234">下列程式碼是產生的 Razor C# 類別：</span><span class="sxs-lookup"><span data-stu-id="53d98-234">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="53d98-235">具有標記時，`@functions` 方法會作為樣板化方法：</span><span class="sxs-lookup"><span data-stu-id="53d98-235">`@functions` methods serve as templating methods when they have markup:</span></span>

```cshtml
@{
    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}

@functions {
    private void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }
}
```

<span data-ttu-id="53d98-236">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="53d98-236">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="attribute"></a>@attribute

<span data-ttu-id="53d98-237">`@attribute` 指示詞會將指定屬性新增至所產生頁面或檢視的類別。</span><span class="sxs-lookup"><span data-stu-id="53d98-237">The `@attribute` directive adds the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="53d98-238">下列範例會新增 `[Authorize]` 屬性：</span><span class="sxs-lookup"><span data-stu-id="53d98-238">The following example adds the `[Authorize]` attribute:</span></span>

```cshtml
@attribute [Authorize]
```

### <a name="namespace"></a>@namespace

<span data-ttu-id="53d98-239">`@namespace` 指示詞會設定所產生頁面或檢視類別的命名空間：</span><span class="sxs-lookup"><span data-stu-id="53d98-239">The `@namespace` directive sets the namespace of the class of the generated page or view:</span></span>

```cshtml
@namespace Your.Namespace.Here
```

<span data-ttu-id="53d98-240">如果頁面或檢視匯入具有 `@namespace` 指示詞的 API，則原始檔案的命名空間會設定為相對於該命名空間。</span><span class="sxs-lookup"><span data-stu-id="53d98-240">If a page or view imports API with an `@namespace` directive, the original file's namespace is set relative to that namespace.</span></span> 

<span data-ttu-id="53d98-241">如果 *MyApp/Pages/\_ViewImports.cshtml* 包含 `@namespace Hello.World`，則匯入 `Hello.World` 命名空間的網頁或檢視命名空間會如下表設定。</span><span class="sxs-lookup"><span data-stu-id="53d98-241">If *MyApp/Pages/\_ViewImports.cshtml* contains `@namespace Hello.World`, the namespace of pages or views that import the `Hello.World` namespace is set as shown in the following table.</span></span>

| <span data-ttu-id="53d98-242">頁面 (或檢視)</span><span class="sxs-lookup"><span data-stu-id="53d98-242">Page (or view)</span></span>                     | <span data-ttu-id="53d98-243">命名空間</span><span class="sxs-lookup"><span data-stu-id="53d98-243">Namespace</span></span>               |
| ---------------------------------- | ----------------------- |
| <span data-ttu-id="53d98-244">*MyApp/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="53d98-244">*MyApp/Pages/Index.cshtml*</span></span>         | `Hello.World`           |
| <span data-ttu-id="53d98-245">*MyApp/Pages/MorePages/Bar.cshtml*</span><span class="sxs-lookup"><span data-stu-id="53d98-245">*MyApp/Pages/MorePages/Bar.cshtml*</span></span> | `Hello.World.MorePages` |

<span data-ttu-id="53d98-246">如果多個匯入檔案都有 `@namespace` 指示詞，則會使用目錄鏈結中最接近網頁或檢視的檔案。</span><span class="sxs-lookup"><span data-stu-id="53d98-246">If multiple import files have the `@namespace` directive, the file closest to the page or view in the directory chain is used.</span></span>

### <a name="section"></a>@section

<span data-ttu-id="53d98-247">`@section` 指示詞可搭配[配置](xref:mvc/views/layout)使用，讓網頁或檢視可以轉譯 HTML 頁面中不同部分中的內容。</span><span class="sxs-lookup"><span data-stu-id="53d98-247">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable pages or views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="53d98-248">如需詳細資訊，請參閱[區段](xref:mvc/views/layout#layout-sections-label)。</span><span class="sxs-lookup"><span data-stu-id="53d98-248">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="templated-razor-delegates"></a><span data-ttu-id="53d98-249">樣板化 Razor 委派</span><span class="sxs-lookup"><span data-stu-id="53d98-249">Templated Razor delegates</span></span>

<span data-ttu-id="53d98-250">Razor 範本可讓您使用下列格式定義 UI 程式碼片段：</span><span class="sxs-lookup"><span data-stu-id="53d98-250">Razor templates allow you to define a UI snippet with the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="53d98-251">下列範例說明如何以 <xref:System.Func%602> 的形式指定樣板化 Razor 委派。</span><span class="sxs-lookup"><span data-stu-id="53d98-251">The following example illustrates how to specify a templated Razor delegate as a <xref:System.Func%602>.</span></span> <span data-ttu-id="53d98-252">該範例會指定 [dynamic 類型](/dotnet/csharp/programming-guide/types/using-type-dynamic)作為委派所封裝方法的參數。</span><span class="sxs-lookup"><span data-stu-id="53d98-252">The [dynamic type](/dotnet/csharp/programming-guide/types/using-type-dynamic) is specified for the parameter of the method that the delegate encapsulates.</span></span> <span data-ttu-id="53d98-253">並指定 [object 類型](/dotnet/csharp/language-reference/keywords/object)作為委派的傳回值。</span><span class="sxs-lookup"><span data-stu-id="53d98-253">An [object type](/dotnet/csharp/language-reference/keywords/object) is specified as the return value of the delegate.</span></span> <span data-ttu-id="53d98-254">此範本會搭配具有 `Name` 屬性之 `Pet` 的 <xref:System.Collections.Generic.List%601> 來使用。</span><span class="sxs-lookup"><span data-stu-id="53d98-254">The template is used with a <xref:System.Collections.Generic.List%601> of `Pet` that has a `Name` property.</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }
}
```

```cshtml
@{
    Func<dynamic, object> petTemplate = @<p>You have a pet named <strong>@item.Name</strong>.</p>;

    var pets = new List<Pet>
    {
        new Pet { Name = "Rin Tin Tin" },
        new Pet { Name = "Mr. Bigglesworth" },
        new Pet { Name = "K-9" }
    };
}
```

<span data-ttu-id="53d98-255">此範本使用 `foreach` 陳述式所提供的 `pets` 進行轉譯：</span><span class="sxs-lookup"><span data-stu-id="53d98-255">The template is rendered with `pets` supplied by a `foreach` statement:</span></span>

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

<span data-ttu-id="53d98-256">轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="53d98-256">Rendered output:</span></span>

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

<span data-ttu-id="53d98-257">您也可以提供內嵌 Razor 範本作為方法的引數。</span><span class="sxs-lookup"><span data-stu-id="53d98-257">You can also supply an inline Razor template as an argument to a method.</span></span> <span data-ttu-id="53d98-258">在下列範例中，`Repeat` 方法會接收 Razor 範本。</span><span class="sxs-lookup"><span data-stu-id="53d98-258">In the following example, the `Repeat` method receives a Razor template.</span></span> <span data-ttu-id="53d98-259">此方法使用範本來產生 HTML 內容，並重複出現清單所提供的項目：</span><span class="sxs-lookup"><span data-stu-id="53d98-259">The method uses the template to produce HTML content with repeats of items supplied from a list:</span></span>

```cshtml
@using Microsoft.AspNetCore.Html

@functions {
    public static IHtmlContent Repeat(IEnumerable<dynamic> items, int times,
        Func<dynamic, IHtmlContent> template)
    {
        var html = new HtmlContentBuilder();

        foreach (var item in items)
        {
            for (var i = 0; i < times; i++)
            {
                html.AppendHtml(template(item));
            }
        }

        return html;
    }
}
```

<span data-ttu-id="53d98-260">使用先前範例中的寵物清單，呼叫 `Repeat` 方法並指定：</span><span class="sxs-lookup"><span data-stu-id="53d98-260">Using the list of pets from the prior example, the `Repeat` method is called with:</span></span>

* <span data-ttu-id="53d98-261"><xref:System.Collections.Generic.List%601> 的 `Pet`。</span><span class="sxs-lookup"><span data-stu-id="53d98-261"><xref:System.Collections.Generic.List%601> of `Pet`.</span></span>
* <span data-ttu-id="53d98-262">每個寵物的重複次數。</span><span class="sxs-lookup"><span data-stu-id="53d98-262">Number of times to repeat each pet.</span></span>
* <span data-ttu-id="53d98-263">用於未排序清單中清單項目的內嵌範本。</span><span class="sxs-lookup"><span data-stu-id="53d98-263">Inline template to use for the list items of an unordered list.</span></span>

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

<span data-ttu-id="53d98-264">轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="53d98-264">Rendered output:</span></span>

```html
<ul>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>K-9</li>
    <li>K-9</li>
    <li>K-9</li>
</ul>
```

## <a name="tag-helpers"></a><span data-ttu-id="53d98-265">標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="53d98-265">Tag Helpers</span></span>

<span data-ttu-id="53d98-266">[標籤協助程式](xref:mvc/views/tag-helpers/intro)有三個相關的指示詞。</span><span class="sxs-lookup"><span data-stu-id="53d98-266">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="53d98-267">指示詞</span><span class="sxs-lookup"><span data-stu-id="53d98-267">Directive</span></span> | <span data-ttu-id="53d98-268">功能</span><span class="sxs-lookup"><span data-stu-id="53d98-268">Function</span></span> |
| --------- | -------- |
| [<span data-ttu-id="53d98-269">&commat;addTagHelper</span><span class="sxs-lookup"><span data-stu-id="53d98-269">&commat;addTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="53d98-270">使標籤協助程式可供檢視。</span><span class="sxs-lookup"><span data-stu-id="53d98-270">Makes Tag Helpers available to a view.</span></span> |
| [<span data-ttu-id="53d98-271">&commat;removeTagHelper</span><span class="sxs-lookup"><span data-stu-id="53d98-271">&commat;removeTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="53d98-272">移除先前從檢視新增的標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="53d98-272">Removes Tag Helpers previously added from a view.</span></span> |
| [<span data-ttu-id="53d98-273">&commat;tagHelperPrefix</span><span class="sxs-lookup"><span data-stu-id="53d98-273">&commat;tagHelperPrefix</span></span>](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="53d98-274">指定標籤前置字元，以啟用標籤協助程式支援，並將標籤協助程式使用方式設為明確。</span><span class="sxs-lookup"><span data-stu-id="53d98-274">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="53d98-275">Razor 保留關鍵字</span><span class="sxs-lookup"><span data-stu-id="53d98-275">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="53d98-276">Razor 關鍵字</span><span class="sxs-lookup"><span data-stu-id="53d98-276">Razor keywords</span></span>

* <span data-ttu-id="53d98-277">page (需要 ASP.NET Core 2.0 和更新版本)</span><span class="sxs-lookup"><span data-stu-id="53d98-277">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="53d98-278">namespace</span><span class="sxs-lookup"><span data-stu-id="53d98-278">namespace</span></span>
* <span data-ttu-id="53d98-279">函式</span><span class="sxs-lookup"><span data-stu-id="53d98-279">functions</span></span>
* <span data-ttu-id="53d98-280">繼承</span><span class="sxs-lookup"><span data-stu-id="53d98-280">inherits</span></span>
* <span data-ttu-id="53d98-281">model</span><span class="sxs-lookup"><span data-stu-id="53d98-281">model</span></span>
* <span data-ttu-id="53d98-282">section</span><span class="sxs-lookup"><span data-stu-id="53d98-282">section</span></span>
* <span data-ttu-id="53d98-283">helper (ASP.NET Core 目前不支援)</span><span class="sxs-lookup"><span data-stu-id="53d98-283">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="53d98-284">Razor 關鍵字會使用 `@(Razor Keyword)` (例如 `@(functions)`) 逸出。</span><span class="sxs-lookup"><span data-stu-id="53d98-284">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="53d98-285">C# Razor 關鍵字</span><span class="sxs-lookup"><span data-stu-id="53d98-285">C# Razor keywords</span></span>

* <span data-ttu-id="53d98-286">case</span><span class="sxs-lookup"><span data-stu-id="53d98-286">case</span></span>
* <span data-ttu-id="53d98-287">do</span><span class="sxs-lookup"><span data-stu-id="53d98-287">do</span></span>
* <span data-ttu-id="53d98-288">default</span><span class="sxs-lookup"><span data-stu-id="53d98-288">default</span></span>
* <span data-ttu-id="53d98-289">for</span><span class="sxs-lookup"><span data-stu-id="53d98-289">for</span></span>
* <span data-ttu-id="53d98-290">foreach</span><span class="sxs-lookup"><span data-stu-id="53d98-290">foreach</span></span>
* <span data-ttu-id="53d98-291">if</span><span class="sxs-lookup"><span data-stu-id="53d98-291">if</span></span>
* <span data-ttu-id="53d98-292">else</span><span class="sxs-lookup"><span data-stu-id="53d98-292">else</span></span>
* <span data-ttu-id="53d98-293">lock</span><span class="sxs-lookup"><span data-stu-id="53d98-293">lock</span></span>
* <span data-ttu-id="53d98-294">switch</span><span class="sxs-lookup"><span data-stu-id="53d98-294">switch</span></span>
* <span data-ttu-id="53d98-295">try</span><span class="sxs-lookup"><span data-stu-id="53d98-295">try</span></span>
* <span data-ttu-id="53d98-296">catch</span><span class="sxs-lookup"><span data-stu-id="53d98-296">catch</span></span>
* <span data-ttu-id="53d98-297">finally</span><span class="sxs-lookup"><span data-stu-id="53d98-297">finally</span></span>
* <span data-ttu-id="53d98-298">using</span><span class="sxs-lookup"><span data-stu-id="53d98-298">using</span></span>
* <span data-ttu-id="53d98-299">while</span><span class="sxs-lookup"><span data-stu-id="53d98-299">while</span></span>

<span data-ttu-id="53d98-300">C# Razor 關鍵字必須使用 `@(@C# Razor Keyword)` (例如 `@(@case)`) 雙重逸出。</span><span class="sxs-lookup"><span data-stu-id="53d98-300">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="53d98-301">第一個 `@` 會將 Razor 剖析器逸出。</span><span class="sxs-lookup"><span data-stu-id="53d98-301">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="53d98-302">第二個 `@` 會將 C# 剖析器逸出。</span><span class="sxs-lookup"><span data-stu-id="53d98-302">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="53d98-303">Razor 未使用的保留關鍵字</span><span class="sxs-lookup"><span data-stu-id="53d98-303">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="53d98-304">Class - 類別</span><span class="sxs-lookup"><span data-stu-id="53d98-304">class</span></span>

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="53d98-305">檢查針對檢視所產生的 Razor C# 類別</span><span class="sxs-lookup"><span data-stu-id="53d98-305">Inspect the Razor C# class generated for a view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="53d98-306">使用 .NET Core SDK 2.1 或更新版本，[Razor SDK](xref:razor-pages/sdk) 會處理 Razor 檔案的編譯。</span><span class="sxs-lookup"><span data-stu-id="53d98-306">With .NET Core SDK 2.1 or later, the [Razor SDK](xref:razor-pages/sdk) handles compilation of Razor files.</span></span> <span data-ttu-id="53d98-307">建置專案時，Razor SDK 會在專案根目錄中產生 *obj/<build_configuration>/<target_framework_moniker>/Razor* 目錄。</span><span class="sxs-lookup"><span data-stu-id="53d98-307">When building a project, the Razor SDK generates an *obj/<build_configuration>/<target_framework_moniker>/Razor* directory in the project root.</span></span> <span data-ttu-id="53d98-308">*Razor* 目錄內的目錄結構會鏡像專案目錄結構。</span><span class="sxs-lookup"><span data-stu-id="53d98-308">The directory structure within the *Razor* directory mirrors the project's directory structure.</span></span>

<span data-ttu-id="53d98-309">請考慮將目標設為 .NET Core 2.1 之 ASP.NET Core 2.1 Razor Pages 專案中的下列目錄結構：</span><span class="sxs-lookup"><span data-stu-id="53d98-309">Consider the following directory structure in an ASP.NET Core 2.1 Razor Pages project targeting .NET Core 2.1:</span></span>

* <span data-ttu-id="53d98-310">**Areas/**</span><span class="sxs-lookup"><span data-stu-id="53d98-310">**Areas/**</span></span>
  * <span data-ttu-id="53d98-311">**Admin/**</span><span class="sxs-lookup"><span data-stu-id="53d98-311">**Admin/**</span></span>
    * <span data-ttu-id="53d98-312">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="53d98-312">**Pages/**</span></span>
      * <span data-ttu-id="53d98-313">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="53d98-313">*Index.cshtml*</span></span>
      * <span data-ttu-id="53d98-314">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="53d98-314">*Index.cshtml.cs*</span></span>
* <span data-ttu-id="53d98-315">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="53d98-315">**Pages/**</span></span>
  * <span data-ttu-id="53d98-316">**Shared/**</span><span class="sxs-lookup"><span data-stu-id="53d98-316">**Shared/**</span></span>
    * <span data-ttu-id="53d98-317">*_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="53d98-317">*_Layout.cshtml*</span></span>
  * <span data-ttu-id="53d98-318">*_ViewImports.cshtml*</span><span class="sxs-lookup"><span data-stu-id="53d98-318">*_ViewImports.cshtml*</span></span>
  * <span data-ttu-id="53d98-319">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="53d98-319">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="53d98-320">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="53d98-320">*Index.cshtml*</span></span>
  * <span data-ttu-id="53d98-321">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="53d98-321">*Index.cshtml.cs*</span></span>

<span data-ttu-id="53d98-322">在 *Debug* 設定中建置專案會產生下列 *obj* 目錄：</span><span class="sxs-lookup"><span data-stu-id="53d98-322">Building the project in *Debug* configuration yields the following *obj* directory:</span></span>

* <span data-ttu-id="53d98-323">**obj/**</span><span class="sxs-lookup"><span data-stu-id="53d98-323">**obj/**</span></span>
  * <span data-ttu-id="53d98-324">**Debug/**</span><span class="sxs-lookup"><span data-stu-id="53d98-324">**Debug/**</span></span>
    * <span data-ttu-id="53d98-325">**netcoreapp2.1/**</span><span class="sxs-lookup"><span data-stu-id="53d98-325">**netcoreapp2.1/**</span></span>
      * <span data-ttu-id="53d98-326">**Razor/**</span><span class="sxs-lookup"><span data-stu-id="53d98-326">**Razor/**</span></span>
        * <span data-ttu-id="53d98-327">**Areas/**</span><span class="sxs-lookup"><span data-stu-id="53d98-327">**Areas/**</span></span>
          * <span data-ttu-id="53d98-328">**Admin/**</span><span class="sxs-lookup"><span data-stu-id="53d98-328">**Admin/**</span></span>
            * <span data-ttu-id="53d98-329">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="53d98-329">**Pages/**</span></span>
              * <span data-ttu-id="53d98-330">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="53d98-330">*Index.g.cshtml.cs*</span></span>
        * <span data-ttu-id="53d98-331">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="53d98-331">**Pages/**</span></span>
          * <span data-ttu-id="53d98-332">**Shared/**</span><span class="sxs-lookup"><span data-stu-id="53d98-332">**Shared/**</span></span>
            * <span data-ttu-id="53d98-333">*_Layout.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="53d98-333">*_Layout.g.cshtml.cs*</span></span>
          * <span data-ttu-id="53d98-334">*_ViewImports.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="53d98-334">*_ViewImports.g.cshtml.cs*</span></span>
          * <span data-ttu-id="53d98-335">*_ViewStart.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="53d98-335">*_ViewStart.g.cshtml.cs*</span></span>
          * <span data-ttu-id="53d98-336">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="53d98-336">*Index.g.cshtml.cs*</span></span>

<span data-ttu-id="53d98-337">若要檢視針對 *Pages/Index.cshtml* 所產生的類別，請開啟 *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="53d98-337">To view the generated class for *Pages/Index.cshtml*, open *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="53d98-338">請將下列類別新增至 ASP.NET Core MVC 專案：</span><span class="sxs-lookup"><span data-stu-id="53d98-338">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="53d98-339">在 `Startup.ConfigureServices` 中，將 MVC 所新增的 `RazorTemplateEngine` 覆寫為 `CustomTemplateEngine` 類別：</span><span class="sxs-lookup"><span data-stu-id="53d98-339">In `Startup.ConfigureServices`, override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="53d98-340">在 `CustomTemplateEngine` 的 `return csharpDocument;` 陳述式上設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="53d98-340">Set a breakpoint on the `return csharpDocument;` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="53d98-341">當程式在中斷點停止執行時，請檢視 `generatedCode` 的值。</span><span class="sxs-lookup"><span data-stu-id="53d98-341">When program execution stops at the breakpoint, view the value of `generatedCode`.</span></span>

![generatedCode 的文字視覺化檢視](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="53d98-343">檢視查閱和區分大小寫</span><span class="sxs-lookup"><span data-stu-id="53d98-343">View lookups and case sensitivity</span></span>

<span data-ttu-id="53d98-344">Razor 檢視引擎會針對檢視執行區分大小寫的查閱。</span><span class="sxs-lookup"><span data-stu-id="53d98-344">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="53d98-345">不過，實際查閱則取決於基礎檔案系統：</span><span class="sxs-lookup"><span data-stu-id="53d98-345">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="53d98-346">檔案式來源：</span><span class="sxs-lookup"><span data-stu-id="53d98-346">File based source:</span></span>
  * <span data-ttu-id="53d98-347">在具有不區分大小寫之檔案系統的作業系統上 (例如 Windows)，實體檔案提供者查閱不會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="53d98-347">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="53d98-348">例如，`return View("Test")` 針對 */Views/Home/Test.cshtml* 和 */Views/home/test.cshtml* (以及任何其他大小寫變體) 會有相符的結果。</span><span class="sxs-lookup"><span data-stu-id="53d98-348">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="53d98-349">在區分大小寫的檔案系統上 (例如 Linux、OSX 及使用 `EmbeddedFileProvider`)，查閱會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="53d98-349">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="53d98-350">例如，`return View("Test")` 會明確符合 */Views/Home/Test.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="53d98-350">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="53d98-351">先行編譯的檢視：在 ASP.NET Core 2.0 和更新版本中，在所有作業系統上查閱先行編譯的檢視不會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="53d98-351">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="53d98-352">此行為與 Windows 上之實體檔案提供者的行為相同。</span><span class="sxs-lookup"><span data-stu-id="53d98-352">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="53d98-353">如果兩個先行編譯的檢視只有大小寫不同，查閱的結果不會由此決定。</span><span class="sxs-lookup"><span data-stu-id="53d98-353">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="53d98-354">建議開發人員比對檔案和目錄的大小寫以及下列項目的大小寫：</span><span class="sxs-lookup"><span data-stu-id="53d98-354">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

* <span data-ttu-id="53d98-355">區域、控制器和動作名稱。</span><span class="sxs-lookup"><span data-stu-id="53d98-355">Area, controller, and action names.</span></span>
* <span data-ttu-id="53d98-356">Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="53d98-356">Razor Pages.</span></span>

<span data-ttu-id="53d98-357">比對大小寫可確保不論基礎檔案系統為何，部署作業都能夠找到其值。</span><span class="sxs-lookup"><span data-stu-id="53d98-357">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
