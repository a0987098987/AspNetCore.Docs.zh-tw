---
title: ASP.NET Core 的 razor 語法參考
author: rick-anderson
description: 瞭解將 Razor 以伺服器為基礎的程式碼內嵌到網頁中的標記語法。
ms.author: riande
ms.date: 02/12/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/views/razor
ms.openlocfilehash: ba778c45c9a6f608b50a17a9f8c6d50484dbf07c
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85405896"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a>Razor<span data-ttu-id="5e1be-103">ASP.NET Core 的語法參考</span><span class="sxs-lookup"><span data-stu-id="5e1be-103"> syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="5e1be-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Taylor Mullen](https://twitter.com/ntaylormullen)和[Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="5e1be-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

Razor<span data-ttu-id="5e1be-105">是將伺服器程式碼內嵌到網頁中的標記語法。</span><span class="sxs-lookup"><span data-stu-id="5e1be-105"> is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="5e1be-106">Razor語法包含 Razor 標記、c # 和 HTML。</span><span class="sxs-lookup"><span data-stu-id="5e1be-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="5e1be-107">通常包含 Razor 的檔案副檔名為 *.* #。</span><span class="sxs-lookup"><span data-stu-id="5e1be-107">Files containing Razor generally have a *.cshtml* file extension.</span></span> Razor<span data-ttu-id="5e1be-108">也可以在[ Razor 元件](xref:blazor/components/index)檔案（*razor*）中找到。</span><span class="sxs-lookup"><span data-stu-id="5e1be-108"> is also found in [Razor components](xref:blazor/components/index) files (*.razor*).</span></span>

## <a name="rendering-html"></a><span data-ttu-id="5e1be-109">轉譯 HTML</span><span class="sxs-lookup"><span data-stu-id="5e1be-109">Rendering HTML</span></span>

<span data-ttu-id="5e1be-110">預設 Razor 語言為 HTML。</span><span class="sxs-lookup"><span data-stu-id="5e1be-110">The default Razor language is HTML.</span></span> <span data-ttu-id="5e1be-111">從標記呈現 HTML 與從 HTML 檔案轉譯 html Razor 並無不同。</span><span class="sxs-lookup"><span data-stu-id="5e1be-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="5e1be-112">伺服器會以不變的方式轉譯 *. cshtml*檔案中的 HTML 標籤 Razor 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-112">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a>Razor<span data-ttu-id="5e1be-113"> 語法</span><span class="sxs-lookup"><span data-stu-id="5e1be-113"> syntax</span></span>

Razor<span data-ttu-id="5e1be-114">支援 c #，並使用 `@` 符號從 HTML 轉換成 c #。</span><span class="sxs-lookup"><span data-stu-id="5e1be-114"> supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> Razor<span data-ttu-id="5e1be-115">評估 c # 運算式，並在 HTML 輸出中呈現它們。</span><span class="sxs-lookup"><span data-stu-id="5e1be-115"> evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="5e1be-116">當 `@` 符號後面接著[ Razor 保留關鍵字](#razor-reserved-keywords)時，它會轉換成 Razor 特定的標記。</span><span class="sxs-lookup"><span data-stu-id="5e1be-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="5e1be-117">否則會轉換成一般 C#。</span><span class="sxs-lookup"><span data-stu-id="5e1be-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="5e1be-118">若要 `@` 在標記中將符號換 Razor 用，請使用第二個 `@` 符號：</span><span class="sxs-lookup"><span data-stu-id="5e1be-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="5e1be-119">此程式碼在 HTML 中是使用單一 `@` 符號轉譯：</span><span class="sxs-lookup"><span data-stu-id="5e1be-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="5e1be-120">HTML 屬性及含有電子郵件地址的內容不會將 `@` 符號視為轉換字元。</span><span class="sxs-lookup"><span data-stu-id="5e1be-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="5e1be-121">下列範例中的電子郵件地址不會藉由剖析而改變 Razor ：</span><span class="sxs-lookup"><span data-stu-id="5e1be-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="5e1be-122">隱含 Razor 運算式</span><span class="sxs-lookup"><span data-stu-id="5e1be-122">Implicit Razor expressions</span></span>

<span data-ttu-id="5e1be-123">隱含 Razor 運算式的開頭為， `@` 後面接著 c # 程式碼：</span><span class="sxs-lookup"><span data-stu-id="5e1be-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="5e1be-124">除了 C# `await` 關鍵字以外，隱含運算式不能包含空格。</span><span class="sxs-lookup"><span data-stu-id="5e1be-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="5e1be-125">如果 C# 陳述式具有明確的結束字元，則可以混合空格：</span><span class="sxs-lookup"><span data-stu-id="5e1be-125">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="5e1be-126">隱含運算式「不能」\*\*\*\* 包含 C# 泛型，因為括弧 (`<>`) 內的字元會解譯為 HTML 標籤。</span><span class="sxs-lookup"><span data-stu-id="5e1be-126">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="5e1be-127">下列程式碼**無效**：</span><span class="sxs-lookup"><span data-stu-id="5e1be-127">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="5e1be-128">上述程式碼會產生類似下列其中一項的編譯器錯誤：</span><span class="sxs-lookup"><span data-stu-id="5e1be-128">The preceding code generates a compiler error similar to one of the following:</span></span>

* <span data-ttu-id="5e1be-129">"Int" 項目未關閉。</span><span class="sxs-lookup"><span data-stu-id="5e1be-129">The "int" element wasn't closed.</span></span> <span data-ttu-id="5e1be-130">所有項目都必須自行結束或具有相對應的結束標籤。</span><span class="sxs-lookup"><span data-stu-id="5e1be-130">All elements must be either self-closing or have a matching end tag.</span></span>
* <span data-ttu-id="5e1be-131">無法將方法群組 'GenericMethod' 轉換成非委派類型 'type'。</span><span class="sxs-lookup"><span data-stu-id="5e1be-131">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="5e1be-132">您是否想要叫用方法？</span><span class="sxs-lookup"><span data-stu-id="5e1be-132">Did you intend to invoke the method?\`</span></span>

<span data-ttu-id="5e1be-133">泛型方法呼叫必須包裝在[明確 Razor 運算式](#explicit-razor-expressions)或程式[ Razor 代碼區塊](#razor-code-blocks)中。</span><span class="sxs-lookup"><span data-stu-id="5e1be-133">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="5e1be-134">明確 Razor 運算式</span><span class="sxs-lookup"><span data-stu-id="5e1be-134">Explicit Razor expressions</span></span>

<span data-ttu-id="5e1be-135">明確 Razor 運算式是由 `@` 具有對稱括弧的符號所組成。</span><span class="sxs-lookup"><span data-stu-id="5e1be-135">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="5e1be-136">若要呈現上周的時間，則 Razor 會使用下列標記：</span><span class="sxs-lookup"><span data-stu-id="5e1be-136">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="5e1be-137">`@()` 括弧內的任何內容都會經過評估並轉譯成輸出。</span><span class="sxs-lookup"><span data-stu-id="5e1be-137">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="5e1be-138">上一節中所述的隱含運算式通常不能包含空格。</span><span class="sxs-lookup"><span data-stu-id="5e1be-138">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="5e1be-139">在下列程式碼中，不會從目前的時間減去一週：</span><span class="sxs-lookup"><span data-stu-id="5e1be-139">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="5e1be-140">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="5e1be-140">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="5e1be-141">明確運算式可用來串連文字與運算式結果：</span><span class="sxs-lookup"><span data-stu-id="5e1be-141">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="5e1be-142">若沒有明確運算式，`<p>Age@joe.Age</p>` 會視為電子郵件地址，並轉譯 `<p>Age@joe.Age</p>`。</span><span class="sxs-lookup"><span data-stu-id="5e1be-142">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="5e1be-143">當撰寫為明確運算式時，會轉譯 `<p>Age33</p>`。</span><span class="sxs-lookup"><span data-stu-id="5e1be-143">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="5e1be-144">您可以使用明確運算式，透過 *.cshtml* 檔案中的泛型方法轉譯輸出。</span><span class="sxs-lookup"><span data-stu-id="5e1be-144">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="5e1be-145">下列標記會顯示如何修正稍早顯示的錯誤，此錯誤是由 C# 泛型的括弧所造成。</span><span class="sxs-lookup"><span data-stu-id="5e1be-145">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="5e1be-146">程式碼會撰寫為明確運算式：</span><span class="sxs-lookup"><span data-stu-id="5e1be-146">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="5e1be-147">運算式編碼</span><span class="sxs-lookup"><span data-stu-id="5e1be-147">Expression encoding</span></span>

<span data-ttu-id="5e1be-148">評估為字串的 C# 運算式會以 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="5e1be-148">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="5e1be-149">評估為 `IHtmlContent` 的 C# 運算式會透過 `IHtmlContent.WriteTo` 直接轉譯。</span><span class="sxs-lookup"><span data-stu-id="5e1be-149">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="5e1be-150">未評估為 `IHtmlContent` 的 C# 運算式會由 `ToString` 轉換成字串，並經過編碼再轉譯。</span><span class="sxs-lookup"><span data-stu-id="5e1be-150">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="5e1be-151">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="5e1be-151">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="5e1be-152">此 HTML 在瀏覽器中會顯示為：</span><span class="sxs-lookup"><span data-stu-id="5e1be-152">The HTML is shown in the browser as:</span></span>

```html
<span>Hello World</span>
```

<span data-ttu-id="5e1be-153">`HtmlHelper.Raw` 輸出不會經過編碼，而是轉譯為 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="5e1be-153">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="5e1be-154">對未清理的使用者輸入使用 `HtmlHelper.Raw` 會造成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="5e1be-154">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="5e1be-155">使用者輸入可能包含惡意的 JavaScript 或其他惡意探索。</span><span class="sxs-lookup"><span data-stu-id="5e1be-155">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="5e1be-156">清理使用者輸入並不容易。</span><span class="sxs-lookup"><span data-stu-id="5e1be-156">Sanitizing user input is difficult.</span></span> <span data-ttu-id="5e1be-157">請避免對使用者輸入使用 `HtmlHelper.Raw`。</span><span class="sxs-lookup"><span data-stu-id="5e1be-157">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="5e1be-158">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="5e1be-158">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Razor<span data-ttu-id="5e1be-159">程式碼區塊</span><span class="sxs-lookup"><span data-stu-id="5e1be-159"> code blocks</span></span>

Razor<span data-ttu-id="5e1be-160">程式碼區塊會以開頭 `@` ，並以括住 `{}` 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-160"> code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="5e1be-161">不同於運算式，程式碼區塊內的 C# 程式碼不會轉譯。</span><span class="sxs-lookup"><span data-stu-id="5e1be-161">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="5e1be-162">一個檢視中的程式碼區塊和運算式會共用相同的範圍並依序定義：</span><span class="sxs-lookup"><span data-stu-id="5e1be-162">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="5e1be-163">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="5e1be-163">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5e1be-164">在程式碼區塊中，使用標記宣告[區域函式](/dotnet/csharp/programming-guide/classes-and-structs/local-functions)作為樣板化方法：</span><span class="sxs-lookup"><span data-stu-id="5e1be-164">In code blocks, declare [local functions](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) with markup to serve as templating methods:</span></span>

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

<span data-ttu-id="5e1be-165">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="5e1be-165">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="implicit-transitions"></a><span data-ttu-id="5e1be-166">隱含轉換</span><span class="sxs-lookup"><span data-stu-id="5e1be-166">Implicit transitions</span></span>

<span data-ttu-id="5e1be-167">程式碼區塊中的預設語言是 c #，但 Razor 頁面可以轉換回 HTML：</span><span class="sxs-lookup"><span data-stu-id="5e1be-167">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="5e1be-168">明確分隔的轉換</span><span class="sxs-lookup"><span data-stu-id="5e1be-168">Explicit delimited transition</span></span>

<span data-ttu-id="5e1be-169">若要定義應該呈現 HTML 之程式碼區塊的子區段，請使用標記來括住要呈現的字元 Razor `<text>` ：</span><span class="sxs-lookup"><span data-stu-id="5e1be-169">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor `<text>` tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="5e1be-170">使用此方法可轉譯未以 HTML 標籤括住的 HTML。</span><span class="sxs-lookup"><span data-stu-id="5e1be-170">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="5e1be-171">如果沒有 HTML 或 Razor 標記， Razor 就會發生執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="5e1be-171">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="5e1be-172">`<text>` 標籤可在轉譯內容時用來控制空白字元：</span><span class="sxs-lookup"><span data-stu-id="5e1be-172">The `<text>` tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="5e1be-173">只會轉譯 `<text>` 標籤之間的內容。</span><span class="sxs-lookup"><span data-stu-id="5e1be-173">Only the content between the `<text>` tag is rendered.</span></span>
* <span data-ttu-id="5e1be-174">在 HTML 輸出中，`<text>` 標籤前後都不能出現任何空白字元。</span><span class="sxs-lookup"><span data-stu-id="5e1be-174">No whitespace before or after the `<text>` tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition"></a><span data-ttu-id="5e1be-175">明確行轉換</span><span class="sxs-lookup"><span data-stu-id="5e1be-175">Explicit line transition</span></span>

<span data-ttu-id="5e1be-176">若要在程式碼區塊內將整行的其餘部分轉譯為 HTML，請使用 `@:` 語法：</span><span class="sxs-lookup"><span data-stu-id="5e1be-176">To render the rest of an entire line as HTML inside a code block, use `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="5e1be-177">如果 `@:` 程式碼中沒有，則 Razor 會產生執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="5e1be-177">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="5e1be-178">檔案 `@` 中的額外字元 Razor 可能會導致稍後在區塊的語句中發生編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="5e1be-178">Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="5e1be-179">這些編譯器錯誤可能很難了解，因為實際錯誤發生在回報的錯誤之前。</span><span class="sxs-lookup"><span data-stu-id="5e1be-179">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="5e1be-180">將多個明確/隱含運算式合併成單一程式碼區塊之後，經常會出現此錯誤。</span><span class="sxs-lookup"><span data-stu-id="5e1be-180">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="5e1be-181">控制結構</span><span class="sxs-lookup"><span data-stu-id="5e1be-181">Control structures</span></span>

<span data-ttu-id="5e1be-182">控制結構是程式碼區塊的延伸。</span><span class="sxs-lookup"><span data-stu-id="5e1be-182">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="5e1be-183">程式碼區塊的所有層面 (轉換成標記、內嵌 C#) 也適用於下列結構：</span><span class="sxs-lookup"><span data-stu-id="5e1be-183">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="5e1be-184">句式`@if, else if, else, and @switch`</span><span class="sxs-lookup"><span data-stu-id="5e1be-184">Conditionals `@if, else if, else, and @switch`</span></span>

<span data-ttu-id="5e1be-185">`@if` 控制何時執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="5e1be-185">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="5e1be-186">`else` 和 `else if` 不需要 `@` 符號：</span><span class="sxs-lookup"><span data-stu-id="5e1be-186">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="5e1be-187">下列標記示範如何使用 switch 陳述式：</span><span class="sxs-lookup"><span data-stu-id="5e1be-187">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="5e1be-188">遍歷`@for, @foreach, @while, and @do while`</span><span class="sxs-lookup"><span data-stu-id="5e1be-188">Looping `@for, @foreach, @while, and @do while`</span></span>

<span data-ttu-id="5e1be-189">樣板化 HTML 可以透過迴圈控制陳述式轉譯。</span><span class="sxs-lookup"><span data-stu-id="5e1be-189">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="5e1be-190">若要轉譯人員清單：</span><span class="sxs-lookup"><span data-stu-id="5e1be-190">To render a list of people:</span></span>

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

<span data-ttu-id="5e1be-191">支援的迴圈陳述式如下：</span><span class="sxs-lookup"><span data-stu-id="5e1be-191">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="5e1be-192">複合 `@using`</span><span class="sxs-lookup"><span data-stu-id="5e1be-192">Compound `@using`</span></span>

<span data-ttu-id="5e1be-193">在 C# 中，`using` 陳述式可用來確保物件經過處置。</span><span class="sxs-lookup"><span data-stu-id="5e1be-193">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="5e1be-194">在中 Razor ，您可以使用相同的機制來建立包含其他內容的 HTML 協助程式。</span><span class="sxs-lookup"><span data-stu-id="5e1be-194">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="5e1be-195">在下列程式碼中，HTML 協助程式會使用 `@using` 陳述式來轉譯 `<form>` 標籤：</span><span class="sxs-lookup"><span data-stu-id="5e1be-195">In the following code, HTML Helpers render a `<form>` tag with the `@using` statement:</span></span>

```cshtml
@using (Html.BeginForm())
{
    <div>
        Email: <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

### `@try, catch, finally`

<span data-ttu-id="5e1be-196">例外狀況處理會類似於 C#：</span><span class="sxs-lookup"><span data-stu-id="5e1be-196">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

Razor<span data-ttu-id="5e1be-197">具有使用 lock 語句來保護重要區段的功能：</span><span class="sxs-lookup"><span data-stu-id="5e1be-197"> has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="5e1be-198">註解</span><span class="sxs-lookup"><span data-stu-id="5e1be-198">Comments</span></span>

Razor<span data-ttu-id="5e1be-199">支援 c # 和 HTML 批註：</span><span class="sxs-lookup"><span data-stu-id="5e1be-199"> supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="5e1be-200">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="5e1be-200">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

Razor<span data-ttu-id="5e1be-201">在轉譯網頁之前，伺服器會移除批註。</span><span class="sxs-lookup"><span data-stu-id="5e1be-201"> comments are removed by the server before the webpage is rendered.</span></span> Razor<span data-ttu-id="5e1be-202">用 `@*  *@` 來分隔批註。</span><span class="sxs-lookup"><span data-stu-id="5e1be-202"> uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="5e1be-203">下列程式碼會標記為註解，以確保伺服器不會轉譯任何標記：</span><span class="sxs-lookup"><span data-stu-id="5e1be-203">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="5e1be-204">指示詞</span><span class="sxs-lookup"><span data-stu-id="5e1be-204">Directives</span></span>

Razor<span data-ttu-id="5e1be-205">指示詞是以隱含運算式來表示，並在符號後面加上保留關鍵字 `@` 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-205"> directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="5e1be-206">指示詞通常會變更檢視的剖析方式或啟用不同的功能。</span><span class="sxs-lookup"><span data-stu-id="5e1be-206">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="5e1be-207">瞭解如何 Razor 產生視圖的程式碼，可讓您更輕鬆地瞭解指示詞的工作方式。</span><span class="sxs-lookup"><span data-stu-id="5e1be-207">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="5e1be-208">程式碼會產生類似如下的類別：</span><span class="sxs-lookup"><span data-stu-id="5e1be-208">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="5e1be-209">在本文稍後，[檢查 Razor 針對視圖產生的 c # 類別](#inspect-the-razor-c-class-generated-for-a-view)一節會說明如何查看這個產生的類別。</span><span class="sxs-lookup"><span data-stu-id="5e1be-209">Later in this article, the section [Inspect the Razor C# class generated for a view](#inspect-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### `@attribute`

<span data-ttu-id="5e1be-210">`@attribute` 指示詞會將指定屬性新增至所產生頁面或檢視的類別。</span><span class="sxs-lookup"><span data-stu-id="5e1be-210">The `@attribute` directive adds the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="5e1be-211">下列範例會新增 `[Authorize]` 屬性：</span><span class="sxs-lookup"><span data-stu-id="5e1be-211">The following example adds the `[Authorize]` attribute:</span></span>

```cshtml
@attribute [Authorize]
```

::: moniker range=">= aspnetcore-3.0"

### `@code`

<span data-ttu-id="5e1be-212">*此案例僅適用于 Razor 元件（razor）。*</span><span class="sxs-lookup"><span data-stu-id="5e1be-212">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="5e1be-213">`@code`區塊可讓[ Razor 元件](xref:blazor/components/index)將 c # 成員（欄位、屬性和方法）新增至元件：</span><span class="sxs-lookup"><span data-stu-id="5e1be-213">The `@code` block enables a [Razor component](xref:blazor/components/index) to add C# members (fields, properties, and methods) to a component:</span></span>

```razor
@code {
    // C# members (fields, properties, and methods)
}
```

<span data-ttu-id="5e1be-214">針對 Razor 元件， `@code` 是的別名， [`@functions`](#functions) 並建議使用 `@functions` 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-214">For Razor components, `@code` is an alias of [`@functions`](#functions) and recommended over `@functions`.</span></span> <span data-ttu-id="5e1be-215">允許一個以上的 `@code` 區塊。</span><span class="sxs-lookup"><span data-stu-id="5e1be-215">More than one `@code` block is permissible.</span></span>

::: moniker-end

### `@functions`

<span data-ttu-id="5e1be-216">`@functions` 指示詞能夠將 C# 成員 (欄位、屬性和方法) 新增至產生的類別：</span><span class="sxs-lookup"><span data-stu-id="5e1be-216">The `@functions` directive enables adding C# members (fields, properties, and methods) to the generated class:</span></span>

```cshtml
@functions {
    // C# members (fields, properties, and methods)
}
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5e1be-217">在[ Razor 元件](xref:blazor/components/index)中，請使用 `@code` Over `@functions` 來新增 c # 成員。</span><span class="sxs-lookup"><span data-stu-id="5e1be-217">In [Razor components](xref:blazor/components/index), use `@code` over `@functions` to add C# members.</span></span>

::: moniker-end

<span data-ttu-id="5e1be-218">例如：</span><span class="sxs-lookup"><span data-stu-id="5e1be-218">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="5e1be-219">程式碼會產生下列 HTML 標記：</span><span class="sxs-lookup"><span data-stu-id="5e1be-219">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="5e1be-220">下列程式碼是產生的 Razor c # 類別：</span><span class="sxs-lookup"><span data-stu-id="5e1be-220">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5e1be-221">具有標記時，`@functions` 方法會作為樣板化方法：</span><span class="sxs-lookup"><span data-stu-id="5e1be-221">`@functions` methods serve as templating methods when they have markup:</span></span>

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

<span data-ttu-id="5e1be-222">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="5e1be-222">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

### `@implements`

<span data-ttu-id="5e1be-223">`@implements` 指示詞會針對產生的類別實作介面。</span><span class="sxs-lookup"><span data-stu-id="5e1be-223">The `@implements` directive implements an interface for the generated class.</span></span>

<span data-ttu-id="5e1be-224">下列範例會實作 <xref:System.IDisposable?displayProperty=fullName>，如此便能呼叫 <xref:System.IDisposable.Dispose*> 方法：</span><span class="sxs-lookup"><span data-stu-id="5e1be-224">The following example implements <xref:System.IDisposable?displayProperty=fullName> so that the <xref:System.IDisposable.Dispose*> method can be called:</span></span>

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

### `@inherits`

<span data-ttu-id="5e1be-225">`@inherits` 指示詞可讓您完全控制檢視所繼承的類別：</span><span class="sxs-lookup"><span data-stu-id="5e1be-225">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="5e1be-226">下列程式碼是自訂 Razor 頁面類型：</span><span class="sxs-lookup"><span data-stu-id="5e1be-226">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="5e1be-227">`CustomText` 會顯示在檢視中：</span><span class="sxs-lookup"><span data-stu-id="5e1be-227">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="5e1be-228">程式碼會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="5e1be-228">The code renders the following HTML:</span></span>

```html
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

 <span data-ttu-id="5e1be-229">`@model` 和 `@inherits` 可用於相同的檢視。</span><span class="sxs-lookup"><span data-stu-id="5e1be-229">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="5e1be-230">`@inherits` 可能位於檢視匯入的 *_ViewImports.cshtml* 檔案中：</span><span class="sxs-lookup"><span data-stu-id="5e1be-230">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="5e1be-231">下列程式碼是強型別檢視的範例：</span><span class="sxs-lookup"><span data-stu-id="5e1be-231">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="5e1be-232">如果模型中傳遞了 "rick@contoso.com"，檢視會產生下列 HTML 標記：</span><span class="sxs-lookup"><span data-stu-id="5e1be-232">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

### `@inject`

<span data-ttu-id="5e1be-233">指示詞 `@inject` 可讓 Razor 頁面將服務從[服務容器](xref:fundamentals/dependency-injection)插入到視圖中。</span><span class="sxs-lookup"><span data-stu-id="5e1be-233">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="5e1be-234">如需詳細資訊，請參閱[在檢視中插入相依性](xref:mvc/views/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="5e1be-234">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

### `@layout`

<span data-ttu-id="5e1be-235">*此案例僅適用于 Razor 元件（razor）。*</span><span class="sxs-lookup"><span data-stu-id="5e1be-235">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="5e1be-236">指示詞會 `@layout` 指定元件的版面配置 Razor 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-236">The `@layout` directive specifies a layout for a Razor component.</span></span> <span data-ttu-id="5e1be-237">版面配置元件可用來避免程式碼重複和不一致。</span><span class="sxs-lookup"><span data-stu-id="5e1be-237">Layout components are used to avoid code duplication and inconsistency.</span></span> <span data-ttu-id="5e1be-238">如需詳細資訊，請參閱 <xref:blazor/layouts> 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-238">For more information, see <xref:blazor/layouts>.</span></span>

::: moniker-end

### `@model`

<span data-ttu-id="5e1be-239">*此案例僅適用于 MVC 視圖和 Razor 頁面（. cshtml）。*</span><span class="sxs-lookup"><span data-stu-id="5e1be-239">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="5e1be-240">`@model` 指示詞會指定傳遞至檢視或頁面的模型類型：</span><span class="sxs-lookup"><span data-stu-id="5e1be-240">The `@model` directive specifies the type of the model passed to a view or page:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="5e1be-241">在以 Razor 個別使用者帳戶建立的 ASP.NET CORE MVC 或 Pages 應用程式中， *Views/Account/Login。 cshtml*包含下列模型宣告：</span><span class="sxs-lookup"><span data-stu-id="5e1be-241">In an ASP.NET Core MVC or Razor Pages app created with individual user accounts, *Views/Account/Login.cshtml* contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="5e1be-242">產生的類別繼承自 `RazorPage<dynamic>`：</span><span class="sxs-lookup"><span data-stu-id="5e1be-242">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor<span data-ttu-id="5e1be-243">公開 `Model` 屬性，以存取傳遞至視圖的模型：</span><span class="sxs-lookup"><span data-stu-id="5e1be-243"> exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="5e1be-244">`@model` 指示詞會指定 `Model` 屬性的類型。</span><span class="sxs-lookup"><span data-stu-id="5e1be-244">The `@model` directive specifies the type of the `Model` property.</span></span> <span data-ttu-id="5e1be-245">該指示詞會將 `RazorPage<T>` 中的 `T` 指定為檢視從中衍生的產生類別。</span><span class="sxs-lookup"><span data-stu-id="5e1be-245">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="5e1be-246">若未指定 `@model` 指示詞，`Model` 屬性的類型為 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="5e1be-246">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="5e1be-247">如需詳細資訊，請參閱強型別[模型和 @model 關鍵字](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword)。</span><span class="sxs-lookup"><span data-stu-id="5e1be-247">For more information, see [Strongly typed models and the @model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### `@namespace`

<span data-ttu-id="5e1be-248">`@namespace` 指示詞：</span><span class="sxs-lookup"><span data-stu-id="5e1be-248">The `@namespace` directive:</span></span>

* <span data-ttu-id="5e1be-249">設定所產生 Razor 頁面、MVC 視圖或元件之類別的命名空間 Razor 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-249">Sets the namespace of the class of the generated Razor page, MVC view, or Razor component.</span></span>
* <span data-ttu-id="5e1be-250">從目錄樹狀結構中最接近的匯入檔案，設定頁面、views 或 components 類別的根衍生命名空間， *_ViewImports. cshtml* （views 或 pages）或 *_Imports razor* （ Razor 元件）。</span><span class="sxs-lookup"><span data-stu-id="5e1be-250">Sets the root derived namespaces of a pages, views, or components classes from the closest imports file in the directory tree, *_ViewImports.cshtml* (views or pages) or *_Imports.razor* (Razor components).</span></span>

```cshtml
@namespace Your.Namespace.Here
```

<span data-ttu-id="5e1be-251">針對 Razor 下表所示的頁面範例：</span><span class="sxs-lookup"><span data-stu-id="5e1be-251">For the Razor Pages example shown in the following table:</span></span>

* <span data-ttu-id="5e1be-252">每個頁面都會匯入 *Pages/_ViewImports.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="5e1be-252">Each page imports *Pages/_ViewImports.cshtml*.</span></span>
* <span data-ttu-id="5e1be-253">*Pages/_ViewImports.cshtml* 包含 `@namespace Hello.World`。</span><span class="sxs-lookup"><span data-stu-id="5e1be-253">*Pages/_ViewImports.cshtml* contains `@namespace Hello.World`.</span></span>
* <span data-ttu-id="5e1be-254">每個頁面都有 `Hello.World` 作為其命名空間的根目錄。</span><span class="sxs-lookup"><span data-stu-id="5e1be-254">Each page has `Hello.World` as the root of it's namespace.</span></span>

| <span data-ttu-id="5e1be-255">頁面</span><span class="sxs-lookup"><span data-stu-id="5e1be-255">Page</span></span>                                        | <span data-ttu-id="5e1be-256">命名空間</span><span class="sxs-lookup"><span data-stu-id="5e1be-256">Namespace</span></span>                             |
| ------------------------------------------- | ------------------------------------- |
| <span data-ttu-id="5e1be-257">*Pages/Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e1be-257">*Pages/Index.cshtml*</span></span>                        | `Hello.World`                         |
| <span data-ttu-id="5e1be-258">*Pages/MorePages/Page.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e1be-258">*Pages/MorePages/Page.cshtml*</span></span>               | `Hello.World.MorePages`               |
| <span data-ttu-id="5e1be-259">*Pages/MorePages/EvenMorePages/Page.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e1be-259">*Pages/MorePages/EvenMorePages/Page.cshtml*</span></span> | `Hello.World.MorePages.EvenMorePages` |

<span data-ttu-id="5e1be-260">前述關聯性適用于匯入用於 MVC 視圖和元件的檔案 Razor 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-260">The preceding relationships apply to import files used with MVC views and Razor components.</span></span>

<span data-ttu-id="5e1be-261">當多個匯入檔案具有 `@namespace` 指示詞時，會使用目錄樹狀結構中最接近頁面、檢視或元件的檔案來設定根命名空間。</span><span class="sxs-lookup"><span data-stu-id="5e1be-261">When multiple import files have a `@namespace` directive, the file closest to the page, view, or component in the directory tree is used to set the root namespace.</span></span>

<span data-ttu-id="5e1be-262">如果上述範例中的 *EvenMorePages* 資料夾具備含 `@namespace Another.Planet` 的匯入檔案 (或者 *Pages/MorePages/EvenMorePages/Page.cshtml* 檔案包含 `@namespace Another.Planet`)，則結果如下表所示。</span><span class="sxs-lookup"><span data-stu-id="5e1be-262">If the *EvenMorePages* folder in the preceding example has an imports file with `@namespace Another.Planet` (or the *Pages/MorePages/EvenMorePages/Page.cshtml* file contains `@namespace Another.Planet`), the result is shown in the following table.</span></span>

| <span data-ttu-id="5e1be-263">頁面</span><span class="sxs-lookup"><span data-stu-id="5e1be-263">Page</span></span>                                        | <span data-ttu-id="5e1be-264">命名空間</span><span class="sxs-lookup"><span data-stu-id="5e1be-264">Namespace</span></span>               |
| ------------------------------------------- | ----------------------- |
| <span data-ttu-id="5e1be-265">*Pages/Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e1be-265">*Pages/Index.cshtml*</span></span>                        | `Hello.World`           |
| <span data-ttu-id="5e1be-266">*Pages/MorePages/Page.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e1be-266">*Pages/MorePages/Page.cshtml*</span></span>               | `Hello.World.MorePages` |
| <span data-ttu-id="5e1be-267">*Pages/MorePages/EvenMorePages/Page.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5e1be-267">*Pages/MorePages/EvenMorePages/Page.cshtml*</span></span> | `Another.Planet`        |

### `@page`

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5e1be-268">`@page` 指示詞會根據其出現的檔案類型而有不同的效果。</span><span class="sxs-lookup"><span data-stu-id="5e1be-268">The `@page` directive has different effects depending on the type of the file where it appears.</span></span> <span data-ttu-id="5e1be-269">指示詞：</span><span class="sxs-lookup"><span data-stu-id="5e1be-269">The directive:</span></span>

* <span data-ttu-id="5e1be-270">在中， *cshtml*檔案表示檔案是 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="5e1be-270">In in a *.cshtml* file indicates that the file is a Razor Page.</span></span> <span data-ttu-id="5e1be-271">如需詳細資訊，請參閱[自訂路由](xref:razor-pages/index#custom-routes)和 <xref:razor-pages/index> 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-271">For more information, see [Custom routes](xref:razor-pages/index#custom-routes) and <xref:razor-pages/index>.</span></span>
* <span data-ttu-id="5e1be-272">指定 Razor 元件應直接處理要求。</span><span class="sxs-lookup"><span data-stu-id="5e1be-272">Specifies that a Razor component should handle requests directly.</span></span> <span data-ttu-id="5e1be-273">如需詳細資訊，請參閱 <xref:blazor/fundamentals/routing> 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-273">For more information, see <xref:blazor/fundamentals/routing>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5e1be-274">在 `@page` *cshtml*檔案第一行的指示詞表示檔案是 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="5e1be-274">The `@page` directive on the first line of a *.cshtml* file indicates that the file is a Razor Page.</span></span> <span data-ttu-id="5e1be-275">如需詳細資訊，請參閱 <xref:razor-pages/index> 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-275">For more information, see <xref:razor-pages/index>.</span></span>

::: moniker-end

### `@section`

<span data-ttu-id="5e1be-276">*此案例僅適用于 MVC 視圖和 Razor 頁面（. cshtml）。*</span><span class="sxs-lookup"><span data-stu-id="5e1be-276">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="5e1be-277">指示詞 `@section` 會與[MVC 和 Razor 頁面配置](xref:mvc/views/layout)搭配使用，讓視圖或頁面能夠在 HTML 網頁的不同部分呈現內容。</span><span class="sxs-lookup"><span data-stu-id="5e1be-277">The `@section` directive is used in conjunction with [MVC and Razor Pages layouts](xref:mvc/views/layout) to enable views or pages to render content in different parts of the HTML page.</span></span> <span data-ttu-id="5e1be-278">如需詳細資訊，請參閱 <xref:mvc/views/layout> 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-278">For more information, see <xref:mvc/views/layout>.</span></span>

### `@using`

<span data-ttu-id="5e1be-279">`@using` 指示詞會將 C# `using` 指示詞新增至產生的檢視：</span><span class="sxs-lookup"><span data-stu-id="5e1be-279">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5e1be-280">在[ Razor 元件](xref:blazor/components/index)中， `@using` 也會控制哪些元件在範圍內。</span><span class="sxs-lookup"><span data-stu-id="5e1be-280">In [Razor components](xref:blazor/components/index), `@using` also controls which components are in scope.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="directive-attributes"></a><span data-ttu-id="5e1be-281">指示詞屬性</span><span class="sxs-lookup"><span data-stu-id="5e1be-281">Directive attributes</span></span>

Razor<span data-ttu-id="5e1be-282">指示詞屬性會以隱含運算式表示，並在符號後面加上保留關鍵字 `@` 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-282"> directive attributes are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="5e1be-283">指示詞屬性通常會變更剖析元素或啟用不同功能的方式。</span><span class="sxs-lookup"><span data-stu-id="5e1be-283">A directive attribute typically changes the way an element is parsed or enables different functionality.</span></span>

### `@attributes`

<span data-ttu-id="5e1be-284">*此案例僅適用于 Razor 元件（razor）。*</span><span class="sxs-lookup"><span data-stu-id="5e1be-284">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="5e1be-285">`@attributes` 允許元件轉譯非宣告的屬性。</span><span class="sxs-lookup"><span data-stu-id="5e1be-285">`@attributes` allows a component to render non-declared attributes.</span></span> <span data-ttu-id="5e1be-286">如需詳細資訊，請參閱 <xref:blazor/components/index#attribute-splatting-and-arbitrary-parameters> 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-286">For more information, see <xref:blazor/components/index#attribute-splatting-and-arbitrary-parameters>.</span></span>

### `@bind`

<span data-ttu-id="5e1be-287">*此案例僅適用于 Razor 元件（razor）。*</span><span class="sxs-lookup"><span data-stu-id="5e1be-287">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="5e1be-288">元件中的資料繫結會使用 `@bind` 屬性來完成。</span><span class="sxs-lookup"><span data-stu-id="5e1be-288">Data binding in components is accomplished with the `@bind` attribute.</span></span> <span data-ttu-id="5e1be-289">如需詳細資訊，請參閱 <xref:blazor/components/data-binding> 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-289">For more information, see <xref:blazor/components/data-binding>.</span></span>

### `@on{EVENT}`

<span data-ttu-id="5e1be-290">*此案例僅適用于 Razor 元件（razor）。*</span><span class="sxs-lookup"><span data-stu-id="5e1be-290">*This scenario only applies to Razor components (.razor).*</span></span>

Razor<span data-ttu-id="5e1be-291">提供元件的事件處理功能。</span><span class="sxs-lookup"><span data-stu-id="5e1be-291"> provides event handling features for components.</span></span> <span data-ttu-id="5e1be-292">如需詳細資訊，請參閱 <xref:blazor/components/event-handling> 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-292">For more information, see <xref:blazor/components/event-handling>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.1"

### `@on{EVENT}:preventDefault`

<span data-ttu-id="5e1be-293">*此案例僅適用于 Razor 元件（razor）。*</span><span class="sxs-lookup"><span data-stu-id="5e1be-293">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="5e1be-294">防止事件的預設動作。</span><span class="sxs-lookup"><span data-stu-id="5e1be-294">Prevents the default action for the event.</span></span>

### `@on{EVENT}:stopPropagation`

<span data-ttu-id="5e1be-295">*此案例僅適用于 Razor 元件（razor）。*</span><span class="sxs-lookup"><span data-stu-id="5e1be-295">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="5e1be-296">停止事件的事件傳播。</span><span class="sxs-lookup"><span data-stu-id="5e1be-296">Stops event propagation for the event.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### `@key`

<span data-ttu-id="5e1be-297">*此案例僅適用于 Razor 元件（razor）。*</span><span class="sxs-lookup"><span data-stu-id="5e1be-297">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="5e1be-298">`@key` 指示詞屬性導致元件差異比較演算法會根據索引鍵的值來保證元素或元件的保留。</span><span class="sxs-lookup"><span data-stu-id="5e1be-298">The `@key` directive attribute causes the components diffing algorithm to guarantee preservation of elements or components based on the key's value.</span></span> <span data-ttu-id="5e1be-299">如需詳細資訊，請參閱 <xref:blazor/components/index#use-key-to-control-the-preservation-of-elements-and-components> 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-299">For more information, see <xref:blazor/components/index#use-key-to-control-the-preservation-of-elements-and-components>.</span></span>

### `@ref`

<span data-ttu-id="5e1be-300">*此案例僅適用于 Razor 元件（razor）。*</span><span class="sxs-lookup"><span data-stu-id="5e1be-300">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="5e1be-301">元件參考 (`@ref`) 提供一種方式來參考元件執行個體，讓您可以對該執行個體發出命令。</span><span class="sxs-lookup"><span data-stu-id="5e1be-301">Component references (`@ref`) provide a way to reference a component instance so that you can issue commands to that instance.</span></span> <span data-ttu-id="5e1be-302">如需詳細資訊，請參閱 <xref:blazor/components/index#capture-references-to-components> 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-302">For more information, see <xref:blazor/components/index#capture-references-to-components>.</span></span>

### `@typeparam`

<span data-ttu-id="5e1be-303">*此案例僅適用于 Razor 元件（razor）。*</span><span class="sxs-lookup"><span data-stu-id="5e1be-303">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="5e1be-304">指示詞會 `@typeparam` 為所產生的元件類別宣告泛型型別參數。</span><span class="sxs-lookup"><span data-stu-id="5e1be-304">The `@typeparam` directive declares a generic type parameter for the generated component class.</span></span> <span data-ttu-id="5e1be-305">如需詳細資訊，請參閱 <xref:blazor/components/templated-components#generic-typed-components> 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-305">For more information, see <xref:blazor/components/templated-components#generic-typed-components>.</span></span>

::: moniker-end

## <a name="templated-razor-delegates"></a><span data-ttu-id="5e1be-306">樣板化 Razor 委派</span><span class="sxs-lookup"><span data-stu-id="5e1be-306">Templated Razor delegates</span></span>

Razor<span data-ttu-id="5e1be-307">範本可讓您定義具有下列格式的 UI 程式碼片段：</span><span class="sxs-lookup"><span data-stu-id="5e1be-307"> templates allow you to define a UI snippet with the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="5e1be-308">下列範例說明如何將樣板化委派指定 Razor 為 <xref:System.Func%602> 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-308">The following example illustrates how to specify a templated Razor delegate as a <xref:System.Func%602>.</span></span> <span data-ttu-id="5e1be-309">該範例會指定 [dynamic 類型](/dotnet/csharp/programming-guide/types/using-type-dynamic)作為委派所封裝方法的參數。</span><span class="sxs-lookup"><span data-stu-id="5e1be-309">The [dynamic type](/dotnet/csharp/programming-guide/types/using-type-dynamic) is specified for the parameter of the method that the delegate encapsulates.</span></span> <span data-ttu-id="5e1be-310">並指定 [object 類型](/dotnet/csharp/language-reference/keywords/object)作為委派的傳回值。</span><span class="sxs-lookup"><span data-stu-id="5e1be-310">An [object type](/dotnet/csharp/language-reference/keywords/object) is specified as the return value of the delegate.</span></span> <span data-ttu-id="5e1be-311">此範本會搭配具有 `Name` 屬性之 `Pet` 的 <xref:System.Collections.Generic.List%601> 來使用。</span><span class="sxs-lookup"><span data-stu-id="5e1be-311">The template is used with a <xref:System.Collections.Generic.List%601> of `Pet` that has a `Name` property.</span></span>

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

<span data-ttu-id="5e1be-312">此範本使用 `foreach` 陳述式所提供的 `pets` 進行轉譯：</span><span class="sxs-lookup"><span data-stu-id="5e1be-312">The template is rendered with `pets` supplied by a `foreach` statement:</span></span>

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

<span data-ttu-id="5e1be-313">轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="5e1be-313">Rendered output:</span></span>

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

<span data-ttu-id="5e1be-314">您也可以提供內嵌 Razor 範本做為方法的引數。</span><span class="sxs-lookup"><span data-stu-id="5e1be-314">You can also supply an inline Razor template as an argument to a method.</span></span> <span data-ttu-id="5e1be-315">在下列範例中， `Repeat` 方法會接收 Razor 範本。</span><span class="sxs-lookup"><span data-stu-id="5e1be-315">In the following example, the `Repeat` method receives a Razor template.</span></span> <span data-ttu-id="5e1be-316">此方法使用範本來產生 HTML 內容，並重複出現清單所提供的項目：</span><span class="sxs-lookup"><span data-stu-id="5e1be-316">The method uses the template to produce HTML content with repeats of items supplied from a list:</span></span>

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

<span data-ttu-id="5e1be-317">使用先前範例中的寵物清單，呼叫 `Repeat` 方法並指定：</span><span class="sxs-lookup"><span data-stu-id="5e1be-317">Using the list of pets from the prior example, the `Repeat` method is called with:</span></span>

* <span data-ttu-id="5e1be-318"><xref:System.Collections.Generic.List%601> 的 `Pet`。</span><span class="sxs-lookup"><span data-stu-id="5e1be-318"><xref:System.Collections.Generic.List%601> of `Pet`.</span></span>
* <span data-ttu-id="5e1be-319">每個寵物的重複次數。</span><span class="sxs-lookup"><span data-stu-id="5e1be-319">Number of times to repeat each pet.</span></span>
* <span data-ttu-id="5e1be-320">用於未排序清單中清單項目的內嵌範本。</span><span class="sxs-lookup"><span data-stu-id="5e1be-320">Inline template to use for the list items of an unordered list.</span></span>

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

<span data-ttu-id="5e1be-321">轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="5e1be-321">Rendered output:</span></span>

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

## <a name="tag-helpers"></a><span data-ttu-id="5e1be-322">標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="5e1be-322">Tag Helpers</span></span>

<span data-ttu-id="5e1be-323">*此案例僅適用于 MVC 視圖和 Razor 頁面（. cshtml）。*</span><span class="sxs-lookup"><span data-stu-id="5e1be-323">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="5e1be-324">[標籤協助程式](xref:mvc/views/tag-helpers/intro)有三個相關的指示詞。</span><span class="sxs-lookup"><span data-stu-id="5e1be-324">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="5e1be-325">指示詞</span><span class="sxs-lookup"><span data-stu-id="5e1be-325">Directive</span></span> | <span data-ttu-id="5e1be-326">函式</span><span class="sxs-lookup"><span data-stu-id="5e1be-326">Function</span></span> |
| --------- | -------- |
| [`@addTagHelper`](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="5e1be-327">使標籤協助程式可供檢視。</span><span class="sxs-lookup"><span data-stu-id="5e1be-327">Makes Tag Helpers available to a view.</span></span> |
| [`@removeTagHelper`](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="5e1be-328">移除先前從檢視新增的標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="5e1be-328">Removes Tag Helpers previously added from a view.</span></span> |
| [`@tagHelperPrefix`](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="5e1be-329">指定標籤前置字元，以啟用標籤協助程式支援，並將標籤協助程式使用方式設為明確。</span><span class="sxs-lookup"><span data-stu-id="5e1be-329">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a>Razor<span data-ttu-id="5e1be-330">保留的關鍵字</span><span class="sxs-lookup"><span data-stu-id="5e1be-330"> reserved keywords</span></span>

### <a name="razor-keywords"></a>Razor<span data-ttu-id="5e1be-331">字</span><span class="sxs-lookup"><span data-stu-id="5e1be-331"> keywords</span></span>

* <span data-ttu-id="5e1be-332">`page`（需要 ASP.NET Core 2.1 或更新版本）</span><span class="sxs-lookup"><span data-stu-id="5e1be-332">`page` (Requires ASP.NET Core 2.1 or later)</span></span>
* `namespace`
* `functions`
* `inherits`
* `model`
* `section`
* <span data-ttu-id="5e1be-333">`helper`（目前不支援 ASP.NET Core）</span><span class="sxs-lookup"><span data-stu-id="5e1be-333">`helper` (Not currently supported by ASP.NET Core)</span></span>

Razor<span data-ttu-id="5e1be-334">關鍵字會以進行轉義 `@(Razor Keyword)` （例如， `@(functions)` ）。</span><span class="sxs-lookup"><span data-stu-id="5e1be-334"> keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="5e1be-335">C # Razor 關鍵字</span><span class="sxs-lookup"><span data-stu-id="5e1be-335">C# Razor keywords</span></span>

* `case`
* `do`
* `default`
* `for`
* `foreach`
* `if`
* `else`
* `lock`
* `switch`
* `try`
* `catch`
* `finally`
* `using`
* `while`

<span data-ttu-id="5e1be-336">C # Razor 關鍵字必須使用（例如）進行雙重轉義 `@(@C# Razor Keyword)` `@(@case)` 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-336">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="5e1be-337">第一個會將剖析器進行 `@` 轉義 Razor 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-337">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="5e1be-338">第二個 `@` 會將 C# 剖析器逸出。</span><span class="sxs-lookup"><span data-stu-id="5e1be-338">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="5e1be-339">未使用的保留關鍵字Razor</span><span class="sxs-lookup"><span data-stu-id="5e1be-339">Reserved keywords not used by Razor</span></span>

* `class`

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="5e1be-340">檢查 Razor 為視圖產生的 c # 類別</span><span class="sxs-lookup"><span data-stu-id="5e1be-340">Inspect the Razor C# class generated for a view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5e1be-341">在 .NET Core SDK 2.1 或更新版本中， [ Razor SDK](xref:razor-pages/sdk)會處理檔案的編譯 Razor 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-341">With .NET Core SDK 2.1 or later, the [Razor SDK](xref:razor-pages/sdk) handles compilation of Razor files.</span></span> <span data-ttu-id="5e1be-342">建立專案時，SDK 會 Razor 在專案根目錄中產生*obj/<build_configuration>/<target_framework_moniker Razor>/* 目錄。</span><span class="sxs-lookup"><span data-stu-id="5e1be-342">When building a project, the Razor SDK generates an *obj/<build_configuration>/<target_framework_moniker>/Razor* directory in the project root.</span></span> <span data-ttu-id="5e1be-343">目錄中的目錄結構會 *Razor* 反映專案的目錄結構。</span><span class="sxs-lookup"><span data-stu-id="5e1be-343">The directory structure within the *Razor* directory mirrors the project's directory structure.</span></span>

<span data-ttu-id="5e1be-344">在以 .NET Core 2.1 為目標的 ASP.NET Core 2.1 頁面專案中，請考慮下列目錄結構 Razor ：</span><span class="sxs-lookup"><span data-stu-id="5e1be-344">Consider the following directory structure in an ASP.NET Core 2.1 Razor Pages project targeting .NET Core 2.1:</span></span>

```
 Areas/
   Admin/
     Pages/
       Index.cshtml
       Index.cshtml.cs
 Pages/
   Shared/
     _Layout.cshtml
   _ViewImports.cshtml
   _ViewStart.cshtml
   Index.cshtml
   Index.cshtml.cs
  ```

<span data-ttu-id="5e1be-345">在 *Debug* 設定中建置專案會產生下列 *obj* 目錄：</span><span class="sxs-lookup"><span data-stu-id="5e1be-345">Building the project in *Debug* configuration yields the following *obj* directory:</span></span>

```
 obj/
   Debug/
     netcoreapp2.1/
       Razor/
         Areas/
           Admin/
             Pages/
               Index.g.cshtml.cs
         Pages/
           Shared/
             _Layout.g.cshtml.cs
           _ViewImports.g.cshtml.cs
           _ViewStart.g.cshtml.cs
           Index.g.cshtml.cs
```

<span data-ttu-id="5e1be-346">若要查看針對*Pages/Index*所產生的類別，請開啟*obj/Debug/netcoreapp 2.1/ Razor /Pages/Index.g.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="5e1be-346">To view the generated class for *Pages/Index.cshtml*, open *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="5e1be-347">請將下列類別新增至 ASP.NET Core MVC 專案：</span><span class="sxs-lookup"><span data-stu-id="5e1be-347">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="5e1be-348">在 `Startup.ConfigureServices` 中，將 MVC 所新增的 `RazorTemplateEngine` 覆寫為 `CustomTemplateEngine` 類別：</span><span class="sxs-lookup"><span data-stu-id="5e1be-348">In `Startup.ConfigureServices`, override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="5e1be-349">在 `CustomTemplateEngine` 的 `return csharpDocument;` 陳述式上設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="5e1be-349">Set a breakpoint on the `return csharpDocument;` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="5e1be-350">當程式在中斷點停止執行時，請檢視 `generatedCode` 的值。</span><span class="sxs-lookup"><span data-stu-id="5e1be-350">When program execution stops at the breakpoint, view the value of `generatedCode`.</span></span>

![generatedCode 的文字視覺化檢視](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="5e1be-352">檢視查閱和區分大小寫</span><span class="sxs-lookup"><span data-stu-id="5e1be-352">View lookups and case sensitivity</span></span>

<span data-ttu-id="5e1be-353">RazorView engine 會針對 views 執行區分大小寫的查閱。</span><span class="sxs-lookup"><span data-stu-id="5e1be-353">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="5e1be-354">不過，實際查閱則取決於基礎檔案系統：</span><span class="sxs-lookup"><span data-stu-id="5e1be-354">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="5e1be-355">檔案式來源：</span><span class="sxs-lookup"><span data-stu-id="5e1be-355">File based source:</span></span>
  * <span data-ttu-id="5e1be-356">在具有不區分大小寫之檔案系統的作業系統上 (例如 Windows)，實體檔案提供者查閱不會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="5e1be-356">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="5e1be-357">例如，`return View("Test")` 針對 */Views/Home/Test.cshtml* 和 */Views/home/test.cshtml* (以及任何其他大小寫變體) 會有相符的結果。</span><span class="sxs-lookup"><span data-stu-id="5e1be-357">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="5e1be-358">在區分大小寫的檔案系統上 (例如 Linux、OSX 及使用 `EmbeddedFileProvider`)，查閱會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="5e1be-358">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="5e1be-359">例如，`return View("Test")` 會明確符合 */Views/Home/Test.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="5e1be-359">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="5e1be-360">先行編譯的檢視：在 ASP.NET Core 2.0 和更新版本中，在所有作業系統上查閱先行編譯的檢視不會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="5e1be-360">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="5e1be-361">此行為與 Windows 上之實體檔案提供者的行為相同。</span><span class="sxs-lookup"><span data-stu-id="5e1be-361">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="5e1be-362">如果兩個先行編譯的檢視只有大小寫不同，查閱的結果不會由此決定。</span><span class="sxs-lookup"><span data-stu-id="5e1be-362">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="5e1be-363">建議開發人員比對檔案和目錄的大小寫以及下列項目的大小寫：</span><span class="sxs-lookup"><span data-stu-id="5e1be-363">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

* <span data-ttu-id="5e1be-364">區域、控制器和動作名稱。</span><span class="sxs-lookup"><span data-stu-id="5e1be-364">Area, controller, and action names.</span></span>
* Razor<span data-ttu-id="5e1be-365">頁面.</span><span class="sxs-lookup"><span data-stu-id="5e1be-365"> Pages.</span></span>

<span data-ttu-id="5e1be-366">比對大小寫可確保不論基礎檔案系統為何，部署作業都能夠找到其值。</span><span class="sxs-lookup"><span data-stu-id="5e1be-366">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5e1be-367">其他資源</span><span class="sxs-lookup"><span data-stu-id="5e1be-367">Additional resources</span></span>

<span data-ttu-id="5e1be-368">[使用 ASP.NET Web 程式設計的 Razor 簡介語法](/aspnet/web-pages/overview/getting-started/introducing-razor-syntax-c)提供許多使用語法進行程式設計的範例 Razor 。</span><span class="sxs-lookup"><span data-stu-id="5e1be-368">[Introduction to ASP.NET Web Programming Using the Razor Syntax](/aspnet/web-pages/overview/getting-started/introducing-razor-syntax-c) provides many samples of programming with Razor syntax.</span></span>
