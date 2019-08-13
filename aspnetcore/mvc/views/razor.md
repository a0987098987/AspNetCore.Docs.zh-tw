---
title: ASP.NET Core 的 Razor 語法參考
author: rick-anderson
description: 了解將伺服器架構程式碼內嵌到網頁中的 Razor 標記語法。
ms.author: riande
ms.date: 08/05/2019
uid: mvc/views/razor
ms.openlocfilehash: 75bf0e792ff7975f03e0f7c2fa6a71ed74d813e1
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819791"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="62a88-103">ASP.NET Core 的 Razor 語法參考</span><span class="sxs-lookup"><span data-stu-id="62a88-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="62a88-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Luke Latham](https://github.com/guardrex)、[Taylor Mullen](https://twitter.com/ntaylormullen) 與 [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="62a88-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="62a88-105">Razor 是將伺服器架構程式碼內嵌到網頁中的標記語法。</span><span class="sxs-lookup"><span data-stu-id="62a88-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="62a88-106">Razor 語法是由 Razor 標記、C# 和 HTML 所組成。</span><span class="sxs-lookup"><span data-stu-id="62a88-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="62a88-107">含有 Razor 的檔案通常具有 *.cshtml* 副檔名。</span><span class="sxs-lookup"><span data-stu-id="62a88-107">Files containing Razor generally have a *.cshtml* file extension.</span></span> <span data-ttu-id="62a88-108">Razor 也可以在 [Razor 元件](xref:blazor/components)檔案 ( *.razor*) 中找到。</span><span class="sxs-lookup"><span data-stu-id="62a88-108">Razor is also found in [Razor components](xref:blazor/components) files (*.razor*).</span></span>

## <a name="rendering-html"></a><span data-ttu-id="62a88-109">轉譯 HTML</span><span class="sxs-lookup"><span data-stu-id="62a88-109">Rendering HTML</span></span>

<span data-ttu-id="62a88-110">Razor 語言預設為 HTML。</span><span class="sxs-lookup"><span data-stu-id="62a88-110">The default Razor language is HTML.</span></span> <span data-ttu-id="62a88-111">轉譯 Razor 標記中的 HTML 與轉譯 HTML 檔案中的 HTML 並無不同。</span><span class="sxs-lookup"><span data-stu-id="62a88-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="62a88-112">伺服器會依原狀轉譯 *.cshtml* Razor 檔案中的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="62a88-112">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="62a88-113">Razor 語法</span><span class="sxs-lookup"><span data-stu-id="62a88-113">Razor syntax</span></span>

<span data-ttu-id="62a88-114">Razor 支援 C#，並使用 `@` 符號從 HTML 轉換成 C#。</span><span class="sxs-lookup"><span data-stu-id="62a88-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="62a88-115">Razor 會評估 C# 運算式並轉譯成 HTML 輸出。</span><span class="sxs-lookup"><span data-stu-id="62a88-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="62a88-116">當 `@` 符號後面接著 [Razor 保留關鍵字](#razor-reserved-keywords)時，它會轉換成 Razor 特定標記；</span><span class="sxs-lookup"><span data-stu-id="62a88-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="62a88-117">否則會轉換成一般 C#。</span><span class="sxs-lookup"><span data-stu-id="62a88-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="62a88-118">若要將 Razor 標記中的 `@` 符號逸出，請使用第二個 `@` 符號：</span><span class="sxs-lookup"><span data-stu-id="62a88-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="62a88-119">此程式碼在 HTML 中是使用單一 `@` 符號轉譯：</span><span class="sxs-lookup"><span data-stu-id="62a88-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="62a88-120">HTML 屬性及含有電子郵件地址的內容不會將 `@` 符號視為轉換字元。</span><span class="sxs-lookup"><span data-stu-id="62a88-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="62a88-121">Razor 剖析不會處理下列範例中的電子郵件地址：</span><span class="sxs-lookup"><span data-stu-id="62a88-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="62a88-122">Razor 隱含運算式</span><span class="sxs-lookup"><span data-stu-id="62a88-122">Implicit Razor expressions</span></span>

<span data-ttu-id="62a88-123">Razor 隱含運算式會以 `@` 開頭，後面接著 C# 程式碼：</span><span class="sxs-lookup"><span data-stu-id="62a88-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="62a88-124">除了 C# `await` 關鍵字以外，隱含運算式不能包含空格。</span><span class="sxs-lookup"><span data-stu-id="62a88-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="62a88-125">如果 C# 陳述式具有明確的結束字元，則可以混合空格：</span><span class="sxs-lookup"><span data-stu-id="62a88-125">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="62a88-126">隱含運算式「不能」  包含 C# 泛型，因為括弧 (`<>`) 內的字元會解譯為 HTML 標籤。</span><span class="sxs-lookup"><span data-stu-id="62a88-126">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="62a88-127">下列程式碼**無效**：</span><span class="sxs-lookup"><span data-stu-id="62a88-127">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="62a88-128">上述程式碼會產生類似下列其中一項的編譯器錯誤：</span><span class="sxs-lookup"><span data-stu-id="62a88-128">The preceding code generates a compiler error similar to one of the following:</span></span>

* <span data-ttu-id="62a88-129">"Int" 項目未關閉。</span><span class="sxs-lookup"><span data-stu-id="62a88-129">The "int" element wasn't closed.</span></span> <span data-ttu-id="62a88-130">所有項目都必須自行結束或具有相對應的結束標籤。</span><span class="sxs-lookup"><span data-stu-id="62a88-130">All elements must be either self-closing or have a matching end tag.</span></span>
* <span data-ttu-id="62a88-131">無法將方法群組 'GenericMethod' 轉換成非委派類型 'type'。</span><span class="sxs-lookup"><span data-stu-id="62a88-131">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="62a88-132">您是否想要叫用方法？</span><span class="sxs-lookup"><span data-stu-id="62a88-132">Did you intend to invoke the method?\`</span></span>

<span data-ttu-id="62a88-133">泛型方法呼叫必須包裝在 [Razor 明確運算式](#explicit-razor-expressions)或 [Razor 程式碼區塊](#razor-code-blocks)中。</span><span class="sxs-lookup"><span data-stu-id="62a88-133">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="62a88-134">Razor 明確運算式</span><span class="sxs-lookup"><span data-stu-id="62a88-134">Explicit Razor expressions</span></span>

<span data-ttu-id="62a88-135">Razor 明確運算式是由 `@` 符號與對稱的括弧所組成。</span><span class="sxs-lookup"><span data-stu-id="62a88-135">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="62a88-136">為了轉譯上週的時間，使用了下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="62a88-136">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="62a88-137">`@()` 括弧內的任何內容都會經過評估並轉譯成輸出。</span><span class="sxs-lookup"><span data-stu-id="62a88-137">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="62a88-138">上一節中所述的隱含運算式通常不能包含空格。</span><span class="sxs-lookup"><span data-stu-id="62a88-138">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="62a88-139">在下列程式碼中，不會從目前的時間減去一週：</span><span class="sxs-lookup"><span data-stu-id="62a88-139">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="62a88-140">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="62a88-140">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="62a88-141">明確運算式可用來串連文字與運算式結果：</span><span class="sxs-lookup"><span data-stu-id="62a88-141">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="62a88-142">若沒有明確運算式，`<p>Age@joe.Age</p>` 會視為電子郵件地址，並轉譯 `<p>Age@joe.Age</p>`。</span><span class="sxs-lookup"><span data-stu-id="62a88-142">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="62a88-143">當撰寫為明確運算式時，會轉譯 `<p>Age33</p>`。</span><span class="sxs-lookup"><span data-stu-id="62a88-143">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="62a88-144">您可以使用明確運算式，透過 *.cshtml* 檔案中的泛型方法轉譯輸出。</span><span class="sxs-lookup"><span data-stu-id="62a88-144">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="62a88-145">下列標記會顯示如何修正稍早顯示的錯誤，此錯誤是由 C# 泛型的括弧所造成。</span><span class="sxs-lookup"><span data-stu-id="62a88-145">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="62a88-146">程式碼會撰寫為明確運算式：</span><span class="sxs-lookup"><span data-stu-id="62a88-146">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="62a88-147">運算式編碼</span><span class="sxs-lookup"><span data-stu-id="62a88-147">Expression encoding</span></span>

<span data-ttu-id="62a88-148">評估為字串的 C# 運算式會以 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="62a88-148">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="62a88-149">評估為 `IHtmlContent` 的 C# 運算式會透過 `IHtmlContent.WriteTo` 直接轉譯。</span><span class="sxs-lookup"><span data-stu-id="62a88-149">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="62a88-150">未評估為 `IHtmlContent` 的 C# 運算式會由 `ToString` 轉換成字串，並經過編碼再轉譯。</span><span class="sxs-lookup"><span data-stu-id="62a88-150">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="62a88-151">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="62a88-151">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="62a88-152">此 HTML 在瀏覽器中會顯示為：</span><span class="sxs-lookup"><span data-stu-id="62a88-152">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="62a88-153">`HtmlHelper.Raw` 輸出不會經過編碼，而是轉譯為 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="62a88-153">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="62a88-154">對未清理的使用者輸入使用 `HtmlHelper.Raw` 會造成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="62a88-154">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="62a88-155">使用者輸入可能包含惡意的 JavaScript 或其他惡意探索。</span><span class="sxs-lookup"><span data-stu-id="62a88-155">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="62a88-156">清理使用者輸入並不容易。</span><span class="sxs-lookup"><span data-stu-id="62a88-156">Sanitizing user input is difficult.</span></span> <span data-ttu-id="62a88-157">請避免對使用者輸入使用 `HtmlHelper.Raw`。</span><span class="sxs-lookup"><span data-stu-id="62a88-157">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="62a88-158">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="62a88-158">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="62a88-159">Razor 程式碼區塊</span><span class="sxs-lookup"><span data-stu-id="62a88-159">Razor code blocks</span></span>

<span data-ttu-id="62a88-160">Razor 程式碼區塊會以 `@` 開頭，並以 `{}` 括住。</span><span class="sxs-lookup"><span data-stu-id="62a88-160">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="62a88-161">不同於運算式，程式碼區塊內的 C# 程式碼不會轉譯。</span><span class="sxs-lookup"><span data-stu-id="62a88-161">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="62a88-162">一個檢視中的程式碼區塊和運算式會共用相同的範圍並依序定義：</span><span class="sxs-lookup"><span data-stu-id="62a88-162">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="62a88-163">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="62a88-163">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="62a88-164">在程式碼區塊中，使用標記宣告[區域函式](/dotnet/csharp/programming-guide/classes-and-structs/local-functions)作為樣板化方法：</span><span class="sxs-lookup"><span data-stu-id="62a88-164">In code blocks, declare [local functions](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) with markup to serve as templating methods:</span></span>

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

<span data-ttu-id="62a88-165">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="62a88-165">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="implicit-transitions"></a><span data-ttu-id="62a88-166">隱含轉換</span><span class="sxs-lookup"><span data-stu-id="62a88-166">Implicit transitions</span></span>

<span data-ttu-id="62a88-167">程式碼區塊中的預設語言是 C#，但 Razor Page 可以轉換回 HTML：</span><span class="sxs-lookup"><span data-stu-id="62a88-167">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="62a88-168">明確分隔的轉換</span><span class="sxs-lookup"><span data-stu-id="62a88-168">Explicit delimited transition</span></span>

<span data-ttu-id="62a88-169">若要定義程式碼區塊中應轉譯 HTML 的子區段，請使用 Razor `<text>` 標籤括住要轉譯的字元：</span><span class="sxs-lookup"><span data-stu-id="62a88-169">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor `<text>` tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="62a88-170">使用此方法可轉譯未以 HTML 標籤括住的 HTML。</span><span class="sxs-lookup"><span data-stu-id="62a88-170">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="62a88-171">若沒有 HTML 或 Razor 標籤，就會發生 Razor 執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="62a88-171">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="62a88-172">`<text>` 標籤可在轉譯內容時用來控制空白字元：</span><span class="sxs-lookup"><span data-stu-id="62a88-172">The `<text>` tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="62a88-173">只會轉譯 `<text>` 標籤之間的內容。</span><span class="sxs-lookup"><span data-stu-id="62a88-173">Only the content between the `<text>` tag is rendered.</span></span>
* <span data-ttu-id="62a88-174">在 HTML 輸出中，`<text>` 標籤前後都不能出現任何空白字元。</span><span class="sxs-lookup"><span data-stu-id="62a88-174">No whitespace before or after the `<text>` tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-colon"></a><span data-ttu-id="62a88-175">使用 \@&colon; 進行明確的行轉換</span><span class="sxs-lookup"><span data-stu-id="62a88-175">Explicit line transition with \@&colon;</span></span>

<span data-ttu-id="62a88-176">若要將一整行的其餘部分轉譯為程式碼區塊內的 HTML，請使用 `@:` 語法：</span><span class="sxs-lookup"><span data-stu-id="62a88-176">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="62a88-177">若程式碼中沒有 `@:`，就會產生 Razor 執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="62a88-177">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="62a88-178">Razor 檔案中的額外 `@` 字元可能會導致稍後在區塊的陳述式中發生編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="62a88-178">Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="62a88-179">這些編譯器錯誤可能很難了解，因為實際錯誤發生在回報的錯誤之前。</span><span class="sxs-lookup"><span data-stu-id="62a88-179">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="62a88-180">將多個明確/隱含運算式合併成單一程式碼區塊之後，經常會出現此錯誤。</span><span class="sxs-lookup"><span data-stu-id="62a88-180">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="62a88-181">控制結構</span><span class="sxs-lookup"><span data-stu-id="62a88-181">Control structures</span></span>

<span data-ttu-id="62a88-182">控制結構是程式碼區塊的延伸。</span><span class="sxs-lookup"><span data-stu-id="62a88-182">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="62a88-183">程式碼區塊的所有層面 (轉換成標記、內嵌 C#) 也適用於下列結構：</span><span class="sxs-lookup"><span data-stu-id="62a88-183">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="62a88-184">條件式 \@if、else if、else 和 \@switch</span><span class="sxs-lookup"><span data-stu-id="62a88-184">Conditionals \@if, else if, else, and \@switch</span></span>

<span data-ttu-id="62a88-185">`@if` 控制何時執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="62a88-185">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="62a88-186">`else` 和 `else if` 不需要 `@` 符號：</span><span class="sxs-lookup"><span data-stu-id="62a88-186">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="62a88-187">下列標記示範如何使用 switch 陳述式：</span><span class="sxs-lookup"><span data-stu-id="62a88-187">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="62a88-188">迴圈 \@for、\@foreach、\@while 和 \@do while</span><span class="sxs-lookup"><span data-stu-id="62a88-188">Looping \@for, \@foreach, \@while, and \@do while</span></span>

<span data-ttu-id="62a88-189">樣板化 HTML 可以透過迴圈控制陳述式轉譯。</span><span class="sxs-lookup"><span data-stu-id="62a88-189">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="62a88-190">若要轉譯人員清單：</span><span class="sxs-lookup"><span data-stu-id="62a88-190">To render a list of people:</span></span>

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

<span data-ttu-id="62a88-191">支援的迴圈陳述式如下：</span><span class="sxs-lookup"><span data-stu-id="62a88-191">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="62a88-192">複合 \@using</span><span class="sxs-lookup"><span data-stu-id="62a88-192">Compound \@using</span></span>

<span data-ttu-id="62a88-193">在 C# 中，`using` 陳述式可用來確保物件經過處置。</span><span class="sxs-lookup"><span data-stu-id="62a88-193">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="62a88-194">在 Razor 中，使用相同的機制來建立 HTML 協助程式，以包含其他內容。</span><span class="sxs-lookup"><span data-stu-id="62a88-194">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="62a88-195">在下列程式碼中，HTML 協助程式會使用 `@using` 陳述式來轉譯 `<form>` 標籤：</span><span class="sxs-lookup"><span data-stu-id="62a88-195">In the following code, HTML Helpers render a `<form>` tag with the `@using` statement:</span></span>

```cshtml
@using (Html.BeginForm())
{
    <div>
        Email: <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

### <a name="try-catch-finally"></a><span data-ttu-id="62a88-196">\@try、catch、finally</span><span class="sxs-lookup"><span data-stu-id="62a88-196">\@try, catch, finally</span></span>

<span data-ttu-id="62a88-197">例外狀況處理會類似於 C#：</span><span class="sxs-lookup"><span data-stu-id="62a88-197">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a><span data-ttu-id="62a88-198">\@lock</span><span class="sxs-lookup"><span data-stu-id="62a88-198">\@lock</span></span>

<span data-ttu-id="62a88-199">Razor 可以使用 lock 陳述式來保護關鍵區段：</span><span class="sxs-lookup"><span data-stu-id="62a88-199">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="62a88-200">註解</span><span class="sxs-lookup"><span data-stu-id="62a88-200">Comments</span></span>

<span data-ttu-id="62a88-201">Razor 支援 C# 和 HTML 註解：</span><span class="sxs-lookup"><span data-stu-id="62a88-201">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="62a88-202">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="62a88-202">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="62a88-203">伺服器會先移除 Razor 註解，再轉譯網頁。</span><span class="sxs-lookup"><span data-stu-id="62a88-203">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="62a88-204">Razor 使用 `@*  *@` 來分隔註解。</span><span class="sxs-lookup"><span data-stu-id="62a88-204">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="62a88-205">下列程式碼會標記為註解，以確保伺服器不會轉譯任何標記：</span><span class="sxs-lookup"><span data-stu-id="62a88-205">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="62a88-206">指示詞</span><span class="sxs-lookup"><span data-stu-id="62a88-206">Directives</span></span>

<span data-ttu-id="62a88-207">Razor 指示詞是以隱含運算式表示，這些運算式具有保留關鍵字，後面接著 `@` 符號。</span><span class="sxs-lookup"><span data-stu-id="62a88-207">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="62a88-208">指示詞通常會變更檢視的剖析方式或啟用不同的功能。</span><span class="sxs-lookup"><span data-stu-id="62a88-208">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="62a88-209">了解 Razor 如何針對檢視產生程式碼，可讓您更容易了解指示詞的運作方式。</span><span class="sxs-lookup"><span data-stu-id="62a88-209">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="62a88-210">程式碼會產生類似如下的類別：</span><span class="sxs-lookup"><span data-stu-id="62a88-210">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="62a88-211">本文稍後在[檢查針對檢視所產生的 Razor C# 類別](#inspect-the-razor-c-class-generated-for-a-view)一節中說明如何檢視這個產生的類別。</span><span class="sxs-lookup"><span data-stu-id="62a88-211">Later in this article, the section [Inspect the Razor C# class generated for a view](#inspect-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="attribute"></a><span data-ttu-id="62a88-212">\@attribute</span><span class="sxs-lookup"><span data-stu-id="62a88-212">\@attribute</span></span>

<span data-ttu-id="62a88-213">`@attribute` 指示詞會將指定屬性新增至所產生頁面或檢視的類別。</span><span class="sxs-lookup"><span data-stu-id="62a88-213">The `@attribute` directive adds the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="62a88-214">下列範例會新增 `[Authorize]` 屬性：</span><span class="sxs-lookup"><span data-stu-id="62a88-214">The following example adds the `[Authorize]` attribute:</span></span>

```cshtml
@attribute [Authorize]
```

::: moniker range=">= aspnetcore-3.0"

### <a name="code"></a><span data-ttu-id="62a88-215">\@code</span><span class="sxs-lookup"><span data-stu-id="62a88-215">\@code</span></span>

<span data-ttu-id="62a88-216">此案例僅適用於 Razor 元件 (.razor)。 </span><span class="sxs-lookup"><span data-stu-id="62a88-216">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="62a88-217">`@code` 區塊會啟用 [Razor 元件](xref:blazor/components)，來將 C# 成員 (欄位、屬性和方法) 新增至元件：</span><span class="sxs-lookup"><span data-stu-id="62a88-217">The `@code` block enables a [Razor component](xref:blazor/components) to add C# members (fields, properties, and methods) to a component:</span></span>

```cshtml
@code {
    // C# members (fields, properties, and methods)
}
```

<span data-ttu-id="62a88-218">針對 Razor 元件，`@code` 是 [@functions](#functions) 的別名，建議透過 `@functions` 使用。</span><span class="sxs-lookup"><span data-stu-id="62a88-218">For Razor components, `@code` is an alias of [@functions](#functions) and recommended over `@functions`.</span></span> <span data-ttu-id="62a88-219">允許一個以上的 `@code` 區塊。</span><span class="sxs-lookup"><span data-stu-id="62a88-219">More than one `@code` block is permissible.</span></span>

::: moniker-end

### <a name="functions"></a><span data-ttu-id="62a88-220">\@functions</span><span class="sxs-lookup"><span data-stu-id="62a88-220">\@functions</span></span>

<span data-ttu-id="62a88-221">`@functions` 指示詞能夠將 C# 成員 (欄位、屬性和方法) 新增至產生的類別：</span><span class="sxs-lookup"><span data-stu-id="62a88-221">The `@functions` directive enables adding C# members (fields, properties, and methods) to the generated class:</span></span>

```cshtml
@functions {
    // C# members (fields, properties, and methods)
}
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="62a88-222">在 [Razor 元件](xref:blazor/components)中，透過 `@functions` 使用 `@code` 來新增 C# 成員。</span><span class="sxs-lookup"><span data-stu-id="62a88-222">In [Razor components](xref:blazor/components), use `@code` over `@functions` to add C# members.</span></span>

::: moniker-end

<span data-ttu-id="62a88-223">例如：</span><span class="sxs-lookup"><span data-stu-id="62a88-223">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="62a88-224">程式碼會產生下列 HTML 標記：</span><span class="sxs-lookup"><span data-stu-id="62a88-224">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="62a88-225">下列程式碼是產生的 Razor C# 類別：</span><span class="sxs-lookup"><span data-stu-id="62a88-225">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="62a88-226">具有標記時，`@functions` 方法會作為樣板化方法：</span><span class="sxs-lookup"><span data-stu-id="62a88-226">`@functions` methods serve as templating methods when they have markup:</span></span>

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

<span data-ttu-id="62a88-227">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="62a88-227">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

### <a name="implements"></a><span data-ttu-id="62a88-228">\@implements</span><span class="sxs-lookup"><span data-stu-id="62a88-228">\@implements</span></span>

<span data-ttu-id="62a88-229">`@implements` 指示詞會針對產生的類別實作介面。</span><span class="sxs-lookup"><span data-stu-id="62a88-229">The `@implements` directive implements an interface for the generated class.</span></span>

<span data-ttu-id="62a88-230">下列範例會實作 <xref:System.IDisposable?displayProperty=fullName>，如此便能呼叫 <xref:System.IDisposable.Dispose*> 方法：</span><span class="sxs-lookup"><span data-stu-id="62a88-230">The following example implements <xref:System.IDisposable?displayProperty=fullName> so that the <xref:System.IDisposable.Dispose*> method can be called:</span></span>

```cshtml
@implements IDisposable

<h1>Example</h1>

@functions {
    private bool _isDisposed;

    ...

    public void Dispose() => _isDisposed = true;
}
```

::: moniker-end

### <a name="inherits"></a><span data-ttu-id="62a88-231">\@inherits</span><span class="sxs-lookup"><span data-stu-id="62a88-231">\@inherits</span></span>

<span data-ttu-id="62a88-232">`@inherits` 指示詞可讓您完全控制檢視所繼承的類別：</span><span class="sxs-lookup"><span data-stu-id="62a88-232">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="62a88-233">下列程式碼是自訂 Razor Page 類型：</span><span class="sxs-lookup"><span data-stu-id="62a88-233">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="62a88-234">`CustomText` 會顯示在檢視中：</span><span class="sxs-lookup"><span data-stu-id="62a88-234">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="62a88-235">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="62a88-235">The code renders the following HTML:</span></span>

```html
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

 <span data-ttu-id="62a88-236">`@model` 和 `@inherits` 可用於相同的檢視。</span><span class="sxs-lookup"><span data-stu-id="62a88-236">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="62a88-237">`@inherits` 可能位於檢視匯入的 *_ViewImports.cshtml* 檔案中：</span><span class="sxs-lookup"><span data-stu-id="62a88-237">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="62a88-238">下列程式碼是強型別檢視的範例：</span><span class="sxs-lookup"><span data-stu-id="62a88-238">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="62a88-239">如果模型中傳遞了 "rick@contoso.com"，檢視會產生下列 HTML 標記：</span><span class="sxs-lookup"><span data-stu-id="62a88-239">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

### <a name="inject"></a><span data-ttu-id="62a88-240">\@inject</span><span class="sxs-lookup"><span data-stu-id="62a88-240">\@inject</span></span>

<span data-ttu-id="62a88-241">`@inject` 指示詞可讓 Razor Page 從[服務容器](xref:fundamentals/dependency-injection)將服務插入至檢視。</span><span class="sxs-lookup"><span data-stu-id="62a88-241">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="62a88-242">如需詳細資訊，請參閱[在檢視中插入相依性](xref:mvc/views/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="62a88-242">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="layout"></a><span data-ttu-id="62a88-243">\@layout</span><span class="sxs-lookup"><span data-stu-id="62a88-243">\@layout</span></span>

<span data-ttu-id="62a88-244">此案例僅適用於 Razor 元件 (.razor)。 </span><span class="sxs-lookup"><span data-stu-id="62a88-244">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="62a88-245">`@layout` 指示詞會指定 Razor 元件的版面配置。</span><span class="sxs-lookup"><span data-stu-id="62a88-245">The `@layout` directive specifies a layout for a Razor component.</span></span> <span data-ttu-id="62a88-246">版面配置元件可用來避免程式碼重複和不一致。</span><span class="sxs-lookup"><span data-stu-id="62a88-246">Layout components are used to avoid code duplication and inconsistency.</span></span> <span data-ttu-id="62a88-247">如需詳細資訊，請參閱 <xref:blazor/layouts>。</span><span class="sxs-lookup"><span data-stu-id="62a88-247">For more information, see <xref:blazor/layouts>.</span></span>

::: moniker-end

### <a name="model"></a><span data-ttu-id="62a88-248">\@model</span><span class="sxs-lookup"><span data-stu-id="62a88-248">\@model</span></span>

<span data-ttu-id="62a88-249">此案例僅適用於 MVC 檢視和 Razor Pages (.cshtml)。 </span><span class="sxs-lookup"><span data-stu-id="62a88-249">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="62a88-250">`@model` 指示詞會指定傳遞至檢視或頁面的模型類型：</span><span class="sxs-lookup"><span data-stu-id="62a88-250">The `@model` directive specifies the type of the model passed to a view or page:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="62a88-251">在使用個別使用者帳戶建立的 ASP.NET Core MVC 或 Razor Pages 應用程式中，*Views/Account/Login.cshtml* 檢視會包含下列模型宣告：</span><span class="sxs-lookup"><span data-stu-id="62a88-251">In an ASP.NET Core MVC or Razor Pages app created with individual user accounts, *Views/Account/Login.cshtml* contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="62a88-252">產生的類別繼承自 `RazorPage<dynamic>`：</span><span class="sxs-lookup"><span data-stu-id="62a88-252">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="62a88-253">Razor 會公開 `Model` 屬性，以存取傳遞至檢視的模型：</span><span class="sxs-lookup"><span data-stu-id="62a88-253">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="62a88-254">`@model` 指示詞會指定 `Model` 屬性的類型。</span><span class="sxs-lookup"><span data-stu-id="62a88-254">The `@model` directive specifies the type of the `Model` property.</span></span> <span data-ttu-id="62a88-255">該指示詞會將 `RazorPage<T>` 中的 `T` 指定為檢視從中衍生的產生類別。</span><span class="sxs-lookup"><span data-stu-id="62a88-255">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="62a88-256">若未指定 `@model` 指示詞，`Model` 屬性的類型為 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="62a88-256">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="62a88-257">如需詳細資訊，請參閱[強型別模型和 @model 關鍵字](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword)。</span><span class="sxs-lookup"><span data-stu-id="62a88-257">For more information, see [Strongly typed models and the @model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="namespace"></a><span data-ttu-id="62a88-258">\@namespace</span><span class="sxs-lookup"><span data-stu-id="62a88-258">\@namespace</span></span>

<span data-ttu-id="62a88-259">`@namespace` 指示詞：</span><span class="sxs-lookup"><span data-stu-id="62a88-259">The `@namespace` directive:</span></span>

* <span data-ttu-id="62a88-260">設定所產生 Razor 頁面、MVC 檢視或 Razor 元件之類別的命名空間。</span><span class="sxs-lookup"><span data-stu-id="62a88-260">Sets the namespace of the class of the generated Razor page, MVC view, or Razor component.</span></span>
* <span data-ttu-id="62a88-261">從目錄樹狀結構中最接近的匯入檔案 *_ViewImports.cshtml* (檢視或頁面) 或 *_Imports.razor* (Razor 元件)，設定頁面、檢視或元件類別的根衍生命名空間。</span><span class="sxs-lookup"><span data-stu-id="62a88-261">Sets the root derived namespaces of a pages, views, or components classes from the closest imports file in the directory tree, *_ViewImports.cshtml* (views or pages) or *_Imports.razor* (Razor components).</span></span>

```cshtml
@namespace Your.Namespace.Here
```

<span data-ttu-id="62a88-262">針對下表所示的 Razor Pages 範例：</span><span class="sxs-lookup"><span data-stu-id="62a88-262">For the Razor Pages example shown in the following table:</span></span>

* <span data-ttu-id="62a88-263">每個頁面都會匯入 *Pages/_ViewImports.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="62a88-263">Each page imports *Pages/_ViewImports.cshtml*.</span></span>
* <span data-ttu-id="62a88-264">*Pages/_ViewImports.cshtml* 包含 `@namespace Hello.World`。</span><span class="sxs-lookup"><span data-stu-id="62a88-264">*Pages/_ViewImports.cshtml* contains `@namespace Hello.World`.</span></span>
* <span data-ttu-id="62a88-265">每個頁面都有 `Hello.World` 作為其命名空間的根目錄。</span><span class="sxs-lookup"><span data-stu-id="62a88-265">Each page has `Hello.World` as the root of it's namespace.</span></span>

| <span data-ttu-id="62a88-266">頁面</span><span class="sxs-lookup"><span data-stu-id="62a88-266">Page</span></span>                                        | <span data-ttu-id="62a88-267">命名空間</span><span class="sxs-lookup"><span data-stu-id="62a88-267">Namespace</span></span>                             |
| ------------------------------------------- | ------------------------------------- |
| <span data-ttu-id="62a88-268">*Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62a88-268">*Pages/Index.cshtml*</span></span>                        | `Hello.World`                         |
| <span data-ttu-id="62a88-269">*Pages/MorePages/Page.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62a88-269">*Pages/MorePages/Page.cshtml*</span></span>               | `Hello.World.MorePages`               |
| <span data-ttu-id="62a88-270">*Pages/MorePages/EvenMorePages/Page.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62a88-270">*Pages/MorePages/EvenMorePages/Page.cshtml*</span></span> | `Hello.World.MorePages.EvenMorePages` |

<span data-ttu-id="62a88-271">前述關聯性適用於匯入與 MVC 檢視和 Razor 元件搭配使用的檔案。</span><span class="sxs-lookup"><span data-stu-id="62a88-271">The preceding relationships apply to import files used with MVC views and Razor components.</span></span>

<span data-ttu-id="62a88-272">當多個匯入檔案具有 `@namespace` 指示詞時，會使用目錄樹狀結構中最接近頁面、檢視或元件的檔案來設定根命名空間。</span><span class="sxs-lookup"><span data-stu-id="62a88-272">When multiple import files have a `@namespace` directive, the file closest to the page, view, or component in the directory tree is used to set the root namespace.</span></span>

<span data-ttu-id="62a88-273">如果上述範例中的 *EvenMorePages* 資料夾具備含 `@namespace Another.Planet` 的匯入檔案 (或者 *Pages/MorePages/EvenMorePages/Page.cshtml* 檔案包含 `@namespace Another.Planet`)，則結果如下表所示。</span><span class="sxs-lookup"><span data-stu-id="62a88-273">If the *EvenMorePages* folder in the preceding example has an imports file with `@namespace Another.Planet` (or the *Pages/MorePages/EvenMorePages/Page.cshtml* file contains `@namespace Another.Planet`), the result is shown in the following table.</span></span>

| <span data-ttu-id="62a88-274">頁面</span><span class="sxs-lookup"><span data-stu-id="62a88-274">Page</span></span>                                        | <span data-ttu-id="62a88-275">命名空間</span><span class="sxs-lookup"><span data-stu-id="62a88-275">Namespace</span></span>               |
| ------------------------------------------- | ----------------------- |
| <span data-ttu-id="62a88-276">*Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62a88-276">*Pages/Index.cshtml*</span></span>                        | `Hello.World`           |
| <span data-ttu-id="62a88-277">*Pages/MorePages/Page.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62a88-277">*Pages/MorePages/Page.cshtml*</span></span>               | `Hello.World.MorePages` |
| <span data-ttu-id="62a88-278">*Pages/MorePages/EvenMorePages/Page.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62a88-278">*Pages/MorePages/EvenMorePages/Page.cshtml*</span></span> | `Another.Planet`        |

### <a name="page"></a><span data-ttu-id="62a88-279">\@page</span><span class="sxs-lookup"><span data-stu-id="62a88-279">\@page</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="62a88-280">`@page` 指示詞會根據其出現的檔案類型而有不同的效果。</span><span class="sxs-lookup"><span data-stu-id="62a88-280">The `@page` directive has different effects depending on the type of the file where it appears.</span></span> <span data-ttu-id="62a88-281">指示詞：</span><span class="sxs-lookup"><span data-stu-id="62a88-281">The directive:</span></span>

* <span data-ttu-id="62a88-282">*.cshtml* 檔案中的 in 表示檔案是 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="62a88-282">In in a *.cshtml* file indicates that the file is a Razor Page.</span></span> <span data-ttu-id="62a88-283">如需詳細資訊，請參閱 <xref:razor-pages/index>。</span><span class="sxs-lookup"><span data-stu-id="62a88-283">For more information, see <xref:razor-pages/index>.</span></span>
* <span data-ttu-id="62a88-284">指定 Razor 元件應該直接處理要求。</span><span class="sxs-lookup"><span data-stu-id="62a88-284">Specifies that a Razor component should handle requests directly.</span></span> <span data-ttu-id="62a88-285">如需詳細資訊，請參閱 <xref:blazor/routing>。</span><span class="sxs-lookup"><span data-stu-id="62a88-285">For more information, see <xref:blazor/routing>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="62a88-286">位於 *.cshtml* 檔案第一行的 `@page` 指示詞指出該檔案是 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="62a88-286">The `@page` directive on the first line of a *.cshtml* file indicates that the file is a Razor Page.</span></span> <span data-ttu-id="62a88-287">如需詳細資訊，請參閱 <xref:razor-pages/index>。</span><span class="sxs-lookup"><span data-stu-id="62a88-287">For more information, see <xref:razor-pages/index>.</span></span>

::: moniker-end

### <a name="section"></a><span data-ttu-id="62a88-288">\@section</span><span class="sxs-lookup"><span data-stu-id="62a88-288">\@section</span></span>

<span data-ttu-id="62a88-289">此案例僅適用於 MVC 檢視和 Razor Pages (.cshtml)。 </span><span class="sxs-lookup"><span data-stu-id="62a88-289">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="62a88-290">`@section` 指示詞會與 [MVC 和 Razor Pages 版面配置](xref:mvc/views/layout)搭配使用，讓檢視或頁面可以轉譯 HTML 頁面不同部分中的內容。</span><span class="sxs-lookup"><span data-stu-id="62a88-290">The `@section` directive is used in conjunction with [MVC and Razor Pages layouts](xref:mvc/views/layout) to enable views or pages to render content in different parts of the HTML page.</span></span> <span data-ttu-id="62a88-291">如需詳細資訊，請參閱 <xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="62a88-291">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="using"></a><span data-ttu-id="62a88-292">\@using</span><span class="sxs-lookup"><span data-stu-id="62a88-292">\@using</span></span>

<span data-ttu-id="62a88-293">`@using` 指示詞會將 C# `using` 指示詞新增至產生的檢視：</span><span class="sxs-lookup"><span data-stu-id="62a88-293">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="62a88-294">在 [Razor 元件](xref:blazor/components)中，`@using` 也會控制哪些元件位於範圍內。</span><span class="sxs-lookup"><span data-stu-id="62a88-294">In [Razor components](xref:blazor/components), `@using` also controls which components are in scope.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="directive-attributes"></a><span data-ttu-id="62a88-295">指示詞屬性</span><span class="sxs-lookup"><span data-stu-id="62a88-295">Directive attributes</span></span>

### <a name="attributes"></a><span data-ttu-id="62a88-296">\@attributes</span><span class="sxs-lookup"><span data-stu-id="62a88-296">\@attributes</span></span>

<span data-ttu-id="62a88-297">此案例僅適用於 Razor 元件 (.razor)。 </span><span class="sxs-lookup"><span data-stu-id="62a88-297">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="62a88-298">`@attributes` 允許元件轉譯非宣告的屬性。</span><span class="sxs-lookup"><span data-stu-id="62a88-298">`@attributes` allows a component to render non-declared attributes.</span></span> <span data-ttu-id="62a88-299">如需詳細資訊，請參閱 <xref:blazor/components#attribute-splatting-and-arbitrary-parameters>。</span><span class="sxs-lookup"><span data-stu-id="62a88-299">For more information, see <xref:blazor/components#attribute-splatting-and-arbitrary-parameters>.</span></span>

### <a name="bind"></a><span data-ttu-id="62a88-300">\@bind</span><span class="sxs-lookup"><span data-stu-id="62a88-300">\@bind</span></span>

<span data-ttu-id="62a88-301">此案例僅適用於 Razor 元件 (.razor)。 </span><span class="sxs-lookup"><span data-stu-id="62a88-301">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="62a88-302">元件中的資料繫結會使用 `@bind` 屬性來完成。</span><span class="sxs-lookup"><span data-stu-id="62a88-302">Data binding in components is accomplished with the `@bind` attribute.</span></span> <span data-ttu-id="62a88-303">如需詳細資訊，請參閱 <xref:blazor/components#data-binding>。</span><span class="sxs-lookup"><span data-stu-id="62a88-303">For more information, see <xref:blazor/components#data-binding>.</span></span>

### <a name="onevent"></a><span data-ttu-id="62a88-304">\@on{event}</span><span class="sxs-lookup"><span data-stu-id="62a88-304">\@on{event}</span></span>

<span data-ttu-id="62a88-305">此案例僅適用於 Razor 元件 (.razor)。 </span><span class="sxs-lookup"><span data-stu-id="62a88-305">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="62a88-306">Razor 提供元件的事件處理功能。</span><span class="sxs-lookup"><span data-stu-id="62a88-306">Razor provides event handling features for components.</span></span> <span data-ttu-id="62a88-307">如需詳細資訊，請參閱 <xref:blazor/components#event-handling>。</span><span class="sxs-lookup"><span data-stu-id="62a88-307">For more information, see <xref:blazor/components#event-handling>.</span></span>

### <a name="key"></a><span data-ttu-id="62a88-308">\@key</span><span class="sxs-lookup"><span data-stu-id="62a88-308">\@key</span></span>

<span data-ttu-id="62a88-309">此案例僅適用於 Razor 元件 (.razor)。 </span><span class="sxs-lookup"><span data-stu-id="62a88-309">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="62a88-310">`@key` 指示詞屬性導致元件差異比較演算法會根據索引鍵的值來保證元素或元件的保留。</span><span class="sxs-lookup"><span data-stu-id="62a88-310">The `@key` directive attribute causes the components diffing algorithm to guarantee preservation of elements or components based on the key's value.</span></span> <span data-ttu-id="62a88-311">如需詳細資訊，請參閱 <xref:blazor/components#use-key-to-control-the-preservation-of-elements-and-components>。</span><span class="sxs-lookup"><span data-stu-id="62a88-311">For more information, see <xref:blazor/components#use-key-to-control-the-preservation-of-elements-and-components>.</span></span>

### <a name="ref"></a><span data-ttu-id="62a88-312">\@ref</span><span class="sxs-lookup"><span data-stu-id="62a88-312">\@ref</span></span>

<span data-ttu-id="62a88-313">此案例僅適用於 Razor 元件 (.razor)。 </span><span class="sxs-lookup"><span data-stu-id="62a88-313">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="62a88-314">元件參考 (`@ref`) 提供一種方式來參考元件執行個體，讓您可以對該執行個體發出命令。</span><span class="sxs-lookup"><span data-stu-id="62a88-314">Component references (`@ref`) provide a way to reference a component instance so that you can issue commands to that instance.</span></span> <span data-ttu-id="62a88-315">如需詳細資訊，請參閱 <xref:blazor/components#capture-references-to-components>。</span><span class="sxs-lookup"><span data-stu-id="62a88-315">For more information, see <xref:blazor/components#capture-references-to-components>.</span></span>

::: moniker-end

## <a name="templated-razor-delegates"></a><span data-ttu-id="62a88-316">樣板化 Razor 委派</span><span class="sxs-lookup"><span data-stu-id="62a88-316">Templated Razor delegates</span></span>

<span data-ttu-id="62a88-317">Razor 範本可讓您使用下列格式定義 UI 程式碼片段：</span><span class="sxs-lookup"><span data-stu-id="62a88-317">Razor templates allow you to define a UI snippet with the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="62a88-318">下列範例說明如何以 <xref:System.Func%602> 的形式指定樣板化 Razor 委派。</span><span class="sxs-lookup"><span data-stu-id="62a88-318">The following example illustrates how to specify a templated Razor delegate as a <xref:System.Func%602>.</span></span> <span data-ttu-id="62a88-319">該範例會指定 [dynamic 類型](/dotnet/csharp/programming-guide/types/using-type-dynamic)作為委派所封裝方法的參數。</span><span class="sxs-lookup"><span data-stu-id="62a88-319">The [dynamic type](/dotnet/csharp/programming-guide/types/using-type-dynamic) is specified for the parameter of the method that the delegate encapsulates.</span></span> <span data-ttu-id="62a88-320">並指定 [object 類型](/dotnet/csharp/language-reference/keywords/object)作為委派的傳回值。</span><span class="sxs-lookup"><span data-stu-id="62a88-320">An [object type](/dotnet/csharp/language-reference/keywords/object) is specified as the return value of the delegate.</span></span> <span data-ttu-id="62a88-321">此範本會搭配具有 `Name` 屬性之 `Pet` 的 <xref:System.Collections.Generic.List%601> 來使用。</span><span class="sxs-lookup"><span data-stu-id="62a88-321">The template is used with a <xref:System.Collections.Generic.List%601> of `Pet` that has a `Name` property.</span></span>

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

<span data-ttu-id="62a88-322">此範本使用 `foreach` 陳述式所提供的 `pets` 進行轉譯：</span><span class="sxs-lookup"><span data-stu-id="62a88-322">The template is rendered with `pets` supplied by a `foreach` statement:</span></span>

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

<span data-ttu-id="62a88-323">轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="62a88-323">Rendered output:</span></span>

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

<span data-ttu-id="62a88-324">您也可以提供內嵌 Razor 範本作為方法的引數。</span><span class="sxs-lookup"><span data-stu-id="62a88-324">You can also supply an inline Razor template as an argument to a method.</span></span> <span data-ttu-id="62a88-325">在下列範例中，`Repeat` 方法會接收 Razor 範本。</span><span class="sxs-lookup"><span data-stu-id="62a88-325">In the following example, the `Repeat` method receives a Razor template.</span></span> <span data-ttu-id="62a88-326">此方法使用範本來產生 HTML 內容，並重複出現清單所提供的項目：</span><span class="sxs-lookup"><span data-stu-id="62a88-326">The method uses the template to produce HTML content with repeats of items supplied from a list:</span></span>

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

<span data-ttu-id="62a88-327">使用先前範例中的寵物清單，呼叫 `Repeat` 方法並指定：</span><span class="sxs-lookup"><span data-stu-id="62a88-327">Using the list of pets from the prior example, the `Repeat` method is called with:</span></span>

* <span data-ttu-id="62a88-328"><xref:System.Collections.Generic.List%601> 的 `Pet`。</span><span class="sxs-lookup"><span data-stu-id="62a88-328"><xref:System.Collections.Generic.List%601> of `Pet`.</span></span>
* <span data-ttu-id="62a88-329">每個寵物的重複次數。</span><span class="sxs-lookup"><span data-stu-id="62a88-329">Number of times to repeat each pet.</span></span>
* <span data-ttu-id="62a88-330">用於未排序清單中清單項目的內嵌範本。</span><span class="sxs-lookup"><span data-stu-id="62a88-330">Inline template to use for the list items of an unordered list.</span></span>

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

<span data-ttu-id="62a88-331">轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="62a88-331">Rendered output:</span></span>

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

## <a name="tag-helpers"></a><span data-ttu-id="62a88-332">標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="62a88-332">Tag Helpers</span></span>

<span data-ttu-id="62a88-333">此案例僅適用於 MVC 檢視和 Razor Pages (.cshtml)。 </span><span class="sxs-lookup"><span data-stu-id="62a88-333">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="62a88-334">[標籤協助程式](xref:mvc/views/tag-helpers/intro)有三個相關的指示詞。</span><span class="sxs-lookup"><span data-stu-id="62a88-334">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="62a88-335">指示詞</span><span class="sxs-lookup"><span data-stu-id="62a88-335">Directive</span></span> | <span data-ttu-id="62a88-336">功能</span><span class="sxs-lookup"><span data-stu-id="62a88-336">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="62a88-337">使標籤協助程式可供檢視。</span><span class="sxs-lookup"><span data-stu-id="62a88-337">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="62a88-338">移除先前從檢視新增的標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="62a88-338">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="62a88-339">指定標籤前置字元，以啟用標籤協助程式支援，並將標籤協助程式使用方式設為明確。</span><span class="sxs-lookup"><span data-stu-id="62a88-339">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="62a88-340">Razor 保留關鍵字</span><span class="sxs-lookup"><span data-stu-id="62a88-340">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="62a88-341">Razor 關鍵字</span><span class="sxs-lookup"><span data-stu-id="62a88-341">Razor keywords</span></span>

* <span data-ttu-id="62a88-342">page (需要 ASP.NET Core 2.1 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="62a88-342">page (Requires ASP.NET Core 2.1 or later)</span></span>
* <span data-ttu-id="62a88-343">namespace</span><span class="sxs-lookup"><span data-stu-id="62a88-343">namespace</span></span>
* <span data-ttu-id="62a88-344">函式</span><span class="sxs-lookup"><span data-stu-id="62a88-344">functions</span></span>
* <span data-ttu-id="62a88-345">繼承</span><span class="sxs-lookup"><span data-stu-id="62a88-345">inherits</span></span>
* <span data-ttu-id="62a88-346">model</span><span class="sxs-lookup"><span data-stu-id="62a88-346">model</span></span>
* <span data-ttu-id="62a88-347">section</span><span class="sxs-lookup"><span data-stu-id="62a88-347">section</span></span>
* <span data-ttu-id="62a88-348">helper (ASP.NET Core 目前不支援)</span><span class="sxs-lookup"><span data-stu-id="62a88-348">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="62a88-349">Razor 關鍵字會使用 `@(Razor Keyword)` (例如 `@(functions)`) 逸出。</span><span class="sxs-lookup"><span data-stu-id="62a88-349">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="62a88-350">C# Razor 關鍵字</span><span class="sxs-lookup"><span data-stu-id="62a88-350">C# Razor keywords</span></span>

* <span data-ttu-id="62a88-351">case</span><span class="sxs-lookup"><span data-stu-id="62a88-351">case</span></span>
* <span data-ttu-id="62a88-352">do</span><span class="sxs-lookup"><span data-stu-id="62a88-352">do</span></span>
* <span data-ttu-id="62a88-353">default</span><span class="sxs-lookup"><span data-stu-id="62a88-353">default</span></span>
* <span data-ttu-id="62a88-354">for</span><span class="sxs-lookup"><span data-stu-id="62a88-354">for</span></span>
* <span data-ttu-id="62a88-355">foreach</span><span class="sxs-lookup"><span data-stu-id="62a88-355">foreach</span></span>
* <span data-ttu-id="62a88-356">if</span><span class="sxs-lookup"><span data-stu-id="62a88-356">if</span></span>
* <span data-ttu-id="62a88-357">else</span><span class="sxs-lookup"><span data-stu-id="62a88-357">else</span></span>
* <span data-ttu-id="62a88-358">lock</span><span class="sxs-lookup"><span data-stu-id="62a88-358">lock</span></span>
* <span data-ttu-id="62a88-359">switch</span><span class="sxs-lookup"><span data-stu-id="62a88-359">switch</span></span>
* <span data-ttu-id="62a88-360">try</span><span class="sxs-lookup"><span data-stu-id="62a88-360">try</span></span>
* <span data-ttu-id="62a88-361">catch</span><span class="sxs-lookup"><span data-stu-id="62a88-361">catch</span></span>
* <span data-ttu-id="62a88-362">finally</span><span class="sxs-lookup"><span data-stu-id="62a88-362">finally</span></span>
* <span data-ttu-id="62a88-363">using</span><span class="sxs-lookup"><span data-stu-id="62a88-363">using</span></span>
* <span data-ttu-id="62a88-364">while</span><span class="sxs-lookup"><span data-stu-id="62a88-364">while</span></span>

<span data-ttu-id="62a88-365">C# Razor 關鍵字必須使用 `@(@C# Razor Keyword)` (例如 `@(@case)`) 雙重逸出。</span><span class="sxs-lookup"><span data-stu-id="62a88-365">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="62a88-366">第一個 `@` 會將 Razor 剖析器逸出。</span><span class="sxs-lookup"><span data-stu-id="62a88-366">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="62a88-367">第二個 `@` 會將 C# 剖析器逸出。</span><span class="sxs-lookup"><span data-stu-id="62a88-367">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="62a88-368">Razor 未使用的保留關鍵字</span><span class="sxs-lookup"><span data-stu-id="62a88-368">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="62a88-369">Class - 類別</span><span class="sxs-lookup"><span data-stu-id="62a88-369">class</span></span>

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="62a88-370">檢查針對檢視所產生的 Razor C# 類別</span><span class="sxs-lookup"><span data-stu-id="62a88-370">Inspect the Razor C# class generated for a view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="62a88-371">使用 .NET Core SDK 2.1 或更新版本，[Razor SDK](xref:razor-pages/sdk) 會處理 Razor 檔案的編譯。</span><span class="sxs-lookup"><span data-stu-id="62a88-371">With .NET Core SDK 2.1 or later, the [Razor SDK](xref:razor-pages/sdk) handles compilation of Razor files.</span></span> <span data-ttu-id="62a88-372">建置專案時，Razor SDK 會在專案根目錄中產生 *obj/<build_configuration>/<target_framework_moniker>/Razor* 目錄。</span><span class="sxs-lookup"><span data-stu-id="62a88-372">When building a project, the Razor SDK generates an *obj/<build_configuration>/<target_framework_moniker>/Razor* directory in the project root.</span></span> <span data-ttu-id="62a88-373">*Razor* 目錄內的目錄結構會鏡像專案目錄結構。</span><span class="sxs-lookup"><span data-stu-id="62a88-373">The directory structure within the *Razor* directory mirrors the project's directory structure.</span></span>

<span data-ttu-id="62a88-374">請考慮將目標設為 .NET Core 2.1 之 ASP.NET Core 2.1 Razor Pages 專案中的下列目錄結構：</span><span class="sxs-lookup"><span data-stu-id="62a88-374">Consider the following directory structure in an ASP.NET Core 2.1 Razor Pages project targeting .NET Core 2.1:</span></span>

* <span data-ttu-id="62a88-375">**Areas/**</span><span class="sxs-lookup"><span data-stu-id="62a88-375">**Areas/**</span></span>
  * <span data-ttu-id="62a88-376">**Admin/**</span><span class="sxs-lookup"><span data-stu-id="62a88-376">**Admin/**</span></span>
    * <span data-ttu-id="62a88-377">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="62a88-377">**Pages/**</span></span>
      * <span data-ttu-id="62a88-378">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62a88-378">*Index.cshtml*</span></span>
      * <span data-ttu-id="62a88-379">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="62a88-379">*Index.cshtml.cs*</span></span>
* <span data-ttu-id="62a88-380">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="62a88-380">**Pages/**</span></span>
  * <span data-ttu-id="62a88-381">**Shared/**</span><span class="sxs-lookup"><span data-stu-id="62a88-381">**Shared/**</span></span>
    * <span data-ttu-id="62a88-382">*_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62a88-382">*_Layout.cshtml*</span></span>
  * <span data-ttu-id="62a88-383">*_ViewImports.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62a88-383">*_ViewImports.cshtml*</span></span>
  * <span data-ttu-id="62a88-384">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62a88-384">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="62a88-385">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62a88-385">*Index.cshtml*</span></span>
  * <span data-ttu-id="62a88-386">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="62a88-386">*Index.cshtml.cs*</span></span>

<span data-ttu-id="62a88-387">在 *Debug* 設定中建置專案會產生下列 *obj* 目錄：</span><span class="sxs-lookup"><span data-stu-id="62a88-387">Building the project in *Debug* configuration yields the following *obj* directory:</span></span>

* <span data-ttu-id="62a88-388">**obj/**</span><span class="sxs-lookup"><span data-stu-id="62a88-388">**obj/**</span></span>
  * <span data-ttu-id="62a88-389">**Debug/**</span><span class="sxs-lookup"><span data-stu-id="62a88-389">**Debug/**</span></span>
    * <span data-ttu-id="62a88-390">**netcoreapp2.1/**</span><span class="sxs-lookup"><span data-stu-id="62a88-390">**netcoreapp2.1/**</span></span>
      * <span data-ttu-id="62a88-391">**Razor/**</span><span class="sxs-lookup"><span data-stu-id="62a88-391">**Razor/**</span></span>
        * <span data-ttu-id="62a88-392">**Areas/**</span><span class="sxs-lookup"><span data-stu-id="62a88-392">**Areas/**</span></span>
          * <span data-ttu-id="62a88-393">**Admin/**</span><span class="sxs-lookup"><span data-stu-id="62a88-393">**Admin/**</span></span>
            * <span data-ttu-id="62a88-394">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="62a88-394">**Pages/**</span></span>
              * <span data-ttu-id="62a88-395">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="62a88-395">*Index.g.cshtml.cs*</span></span>
        * <span data-ttu-id="62a88-396">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="62a88-396">**Pages/**</span></span>
          * <span data-ttu-id="62a88-397">**Shared/**</span><span class="sxs-lookup"><span data-stu-id="62a88-397">**Shared/**</span></span>
            * <span data-ttu-id="62a88-398">*_Layout.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="62a88-398">*_Layout.g.cshtml.cs*</span></span>
          * <span data-ttu-id="62a88-399">*_ViewImports.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="62a88-399">*_ViewImports.g.cshtml.cs*</span></span>
          * <span data-ttu-id="62a88-400">*_ViewStart.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="62a88-400">*_ViewStart.g.cshtml.cs*</span></span>
          * <span data-ttu-id="62a88-401">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="62a88-401">*Index.g.cshtml.cs*</span></span>

<span data-ttu-id="62a88-402">若要檢視針對 *Pages/Index.cshtml* 所產生的類別，請開啟 *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="62a88-402">To view the generated class for *Pages/Index.cshtml*, open *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="62a88-403">請將下列類別新增至 ASP.NET Core MVC 專案：</span><span class="sxs-lookup"><span data-stu-id="62a88-403">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="62a88-404">在 `Startup.ConfigureServices` 中，將 MVC 所新增的 `RazorTemplateEngine` 覆寫為 `CustomTemplateEngine` 類別：</span><span class="sxs-lookup"><span data-stu-id="62a88-404">In `Startup.ConfigureServices`, override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="62a88-405">在 `CustomTemplateEngine` 的 `return csharpDocument;` 陳述式上設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="62a88-405">Set a breakpoint on the `return csharpDocument;` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="62a88-406">當程式在中斷點停止執行時，請檢視 `generatedCode` 的值。</span><span class="sxs-lookup"><span data-stu-id="62a88-406">When program execution stops at the breakpoint, view the value of `generatedCode`.</span></span>

![generatedCode 的文字視覺化檢視](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="62a88-408">檢視查閱和區分大小寫</span><span class="sxs-lookup"><span data-stu-id="62a88-408">View lookups and case sensitivity</span></span>

<span data-ttu-id="62a88-409">Razor 檢視引擎會針對檢視執行區分大小寫的查閱。</span><span class="sxs-lookup"><span data-stu-id="62a88-409">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="62a88-410">不過，實際查閱則取決於基礎檔案系統：</span><span class="sxs-lookup"><span data-stu-id="62a88-410">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="62a88-411">檔案式來源：</span><span class="sxs-lookup"><span data-stu-id="62a88-411">File based source:</span></span>
  * <span data-ttu-id="62a88-412">在具有不區分大小寫之檔案系統的作業系統上 (例如 Windows)，實體檔案提供者查閱不會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="62a88-412">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="62a88-413">例如，`return View("Test")` 針對 */Views/Home/Test.cshtml* 和 */Views/home/test.cshtml* (以及任何其他大小寫變體) 會有相符的結果。</span><span class="sxs-lookup"><span data-stu-id="62a88-413">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="62a88-414">在區分大小寫的檔案系統上 (例如 Linux、OSX 及使用 `EmbeddedFileProvider`)，查閱會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="62a88-414">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="62a88-415">例如，`return View("Test")` 會明確符合 */Views/Home/Test.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="62a88-415">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="62a88-416">先行編譯的檢視：在 ASP.NET Core 2.0 和更新版本中，在所有作業系統上查閱先行編譯的檢視不會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="62a88-416">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="62a88-417">此行為與 Windows 上之實體檔案提供者的行為相同。</span><span class="sxs-lookup"><span data-stu-id="62a88-417">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="62a88-418">如果兩個先行編譯的檢視只有大小寫不同，查閱的結果不會由此決定。</span><span class="sxs-lookup"><span data-stu-id="62a88-418">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="62a88-419">建議開發人員比對檔案和目錄的大小寫以及下列項目的大小寫：</span><span class="sxs-lookup"><span data-stu-id="62a88-419">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

* <span data-ttu-id="62a88-420">區域、控制器和動作名稱。</span><span class="sxs-lookup"><span data-stu-id="62a88-420">Area, controller, and action names.</span></span>
* <span data-ttu-id="62a88-421">Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="62a88-421">Razor Pages.</span></span>

<span data-ttu-id="62a88-422">比對大小寫可確保不論基礎檔案系統為何，部署作業都能夠找到其值。</span><span class="sxs-lookup"><span data-stu-id="62a88-422">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
