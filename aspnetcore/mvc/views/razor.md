---
title: "適用於 ASP.NET Core razor 語法參考"
author: rick-anderson
description: "Razor 語法的詳細資料"
keywords: ASP.NET Core Razor
ms.author: riande
manager: wpickett
ms.date: 07/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 066fe3b2486c63bd4de2ccb865ad432a67846d77
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="a6cd9-104">Razor 語法的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6cd9-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="a6cd9-105">由[Taylor Mullen](https://twitter.com/ntaylormullen)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a6cd9-105">By [Taylor Mullen](https://twitter.com/ntaylormullen) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-is-razor"></a><span data-ttu-id="a6cd9-106">Razor 是什麼？</span><span class="sxs-lookup"><span data-stu-id="a6cd9-106">What is Razor?</span></span>

<span data-ttu-id="a6cd9-107">Razor 是伺服器為基礎的程式碼內嵌在網頁標記語法。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-107">Razor is a markup syntax for embedding server based code into web pages.</span></span> <span data-ttu-id="a6cd9-108">Razor 語法包含 Razor 標記，C# 和 HTML。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-108">The Razor syntax consists of Razor markup, C# and HTML.</span></span> <span data-ttu-id="a6cd9-109">通常包含 Razor 檔案有*.cshtml*檔案副檔名。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-109">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="a6cd9-110">將 HTML 呈現</span><span class="sxs-lookup"><span data-stu-id="a6cd9-110">Rendering HTML</span></span>

<span data-ttu-id="a6cd9-111">預設 Razor 語言為 HTML。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-111">The default Razor language is HTML.</span></span> <span data-ttu-id="a6cd9-112">將 HTML 呈現從 Razor 並無不同 HTML 檔案中。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-112">Rendering HTML from Razor is no different than in an HTML file.</span></span> <span data-ttu-id="a6cd9-113">Razor 檔案中以下列標記：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-113">A Razor file with the following markup:</span></span>

```html
<p>Hello World</p>
   ```

<span data-ttu-id="a6cd9-114">會轉譯為未變更`<p>Hello World</p>`伺服器。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-114">Is rendered unchanged as `<p>Hello World</p>` by the server.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="a6cd9-115">Razor 語法</span><span class="sxs-lookup"><span data-stu-id="a6cd9-115">Razor syntax</span></span>

<span data-ttu-id="a6cd9-116">Razor 支援 C#，並使用`@`轉換來自 HTML，以 C# 中的符號。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-116">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="a6cd9-117">Razor 評估 C# 運算式，並將它們呈現的 HTML 輸出中。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-117">Razor evaluates C# expressions and renders them in the HTML output.</span></span> <span data-ttu-id="a6cd9-118">Razor 可以從 HTML 轉換成 C# 或 Razor 特定標記。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-118">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="a6cd9-119">當`@`符號後面[Razor 保留關鍵字](#razor-reserved-keywords)會轉換成特定的 Razor 標記，否則它會轉換成一般的 C#。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-119">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords) it transitions into Razor-specific markup, otherwise it transitions into plain C#.</span></span>

<a name=escape-at-label></a>

<span data-ttu-id="a6cd9-120">包含 HTML`@`符號可能需要逸出的第二個`@`符號。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-120">HTML containing `@` symbols may need to be escaped with a second `@` symbol.</span></span> <span data-ttu-id="a6cd9-121">例如: </span><span class="sxs-lookup"><span data-stu-id="a6cd9-121">For example:</span></span>

```html
<p>@@Username</p>
   ```

<span data-ttu-id="a6cd9-122">會轉譯下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="a6cd9-122">would render the following HTML:</span></span>

```html
<p>@Username</p>
   ```

<a name=razor-email-ref></a>

<span data-ttu-id="a6cd9-123">HTML 屬性和內容包含電子郵件地址不會將`@`符號為轉換字元。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-123">HTML attributes and content containing email addresses don’t treat the `@` symbol as a transition character.</span></span>

   `<a href="mailto:Support@contoso.com">Support@contoso.com</a>`

## <a name="implicit-razor-expressions"></a><span data-ttu-id="a6cd9-124">Razor 的隱含運算式</span><span class="sxs-lookup"><span data-stu-id="a6cd9-124">Implicit Razor expressions</span></span>

<span data-ttu-id="a6cd9-125">Razor 的隱含運算式開頭`@`後面接著 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-125">Implicit Razor expressions start with `@` followed by C# code.</span></span> <span data-ttu-id="a6cd9-126">例如: </span><span class="sxs-lookup"><span data-stu-id="a6cd9-126">For example:</span></span>

```html
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="a6cd9-127">除了 C#`await`關鍵字的隱含運算式不能包含空格。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-127">With the exception of the C# `await` keyword implicit expressions must not contain spaces.</span></span> <span data-ttu-id="a6cd9-128">比方說，您也可以只要 C# 陳述式已清除結束 intermingle 空格：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-128">For example, you can intermingle spaces as long as the C# statement has a clear ending:</span></span>

```html
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a><span data-ttu-id="a6cd9-129">Razor 的明確運算式</span><span class="sxs-lookup"><span data-stu-id="a6cd9-129">Explicit Razor expressions</span></span>

<span data-ttu-id="a6cd9-130">Razor 的明確運算式包含 @ 有對稱的括號的符號。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-130">Explicit Razor expressions consists of an @ symbol with balanced parenthesis.</span></span> <span data-ttu-id="a6cd9-131">例如，若要顯示上週的時間：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-131">For example, to render last week's time:</span></span>

```html
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="a6cd9-132">內的任何內容 @ （） 括號會評估並呈現至輸出。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-132">Any content within the @() parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="a6cd9-133">通常隱含運算式不可包含空格。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-133">Implicit expressions generally cannot contain spaces.</span></span> <span data-ttu-id="a6cd9-134">例如，下列程式碼，一週會不減去目前的時間：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-134">For example, in the code below, one week is not subtracted from the current time:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact.cshtml?range=20)]

<span data-ttu-id="a6cd9-135">這會轉譯下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="a6cd9-135">Which renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
   ```

<span data-ttu-id="a6cd9-136">您可以使用明確的運算式來串連文字與運算式結果：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-136">You can use an explicit expression to concatenate text with an expression result:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [5]}} -->

```none
@{
    var joe = new Person("Joe", 33);
 }

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="a6cd9-137">沒有明確的運算式，`<p>Age@joe.Age</p>`會被視為電子郵件地址和`<p>Age@joe.Age</p>`都會呈現。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-137">Without the explicit expression, `<p>Age@joe.Age</p>` would be treated as an email address and `<p>Age@joe.Age</p>` would be rendered.</span></span> <span data-ttu-id="a6cd9-138">做為明確的運算式，寫入時`<p>Age33</p>`轉譯。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-138">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<a name=expression-encoding-label></a>

## <a name="expression-encoding"></a><span data-ttu-id="a6cd9-139">運算式的編碼方式</span><span class="sxs-lookup"><span data-stu-id="a6cd9-139">Expression encoding</span></span>

<span data-ttu-id="a6cd9-140">C# 運算式，評估為字串會以 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-140">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="a6cd9-141">C# 運算式會評估得出`IHtmlContent`轉譯直接透過*IHtmlContent.WriteTo*。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-141">C# expressions that evaluate to `IHtmlContent` are rendered directly through *IHtmlContent.WriteTo*.</span></span> <span data-ttu-id="a6cd9-142">C# 運算式未評估成*IHtmlContent*轉換成字串 (由*ToString*)，而且編碼在呈現之前。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-142">C# expressions that don't evaluate to *IHtmlContent* are converted to a string (by *ToString*) and encoded before they are rendered.</span></span> <span data-ttu-id="a6cd9-143">例如，下列的 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-143">For example, the following Razor markup:</span></span>

```html
@("<span>Hello World</span>")
   ```

<span data-ttu-id="a6cd9-144">將 HTML 呈現此：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-144">Renders this HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
   ```

<span data-ttu-id="a6cd9-145">其瀏覽器會呈現為：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-145">Which the browser renders as:</span></span>

`<span>Hello World</span>`

<span data-ttu-id="a6cd9-146">`HtmlHelper``Raw`輸出不會編碼，但不會轉譯為 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-146">`HtmlHelper` `Raw` output is not encoded but rendered as HTML markup.</span></span>

>[!WARNING]
> <span data-ttu-id="a6cd9-147">使用`HtmlHelper.Raw`unsanitized 使用者輸入會造成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-147">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="a6cd9-148">使用者輸入可能包含惡意的 JavaScript 或其他入侵。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-148">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="a6cd9-149">免於使用者輸入很難，避免使用`HtmlHelper.Raw`使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-149">Sanitizing user input is difficult, avoid using `HtmlHelper.Raw` on user input.</span></span>

<span data-ttu-id="a6cd9-150">下列的 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-150">The following Razor markup:</span></span>

```html
@Html.Raw("<span>Hello World</span>")
   ```

<span data-ttu-id="a6cd9-151">將 HTML 呈現此：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-151">Renders this HTML:</span></span>

```html
<span>Hello World</span>
   ```

<a name=razor-code-blocks-label></a>

## <a name="razor-code-blocks"></a><span data-ttu-id="a6cd9-152">Razor 程式碼區塊</span><span class="sxs-lookup"><span data-stu-id="a6cd9-152">Razor code blocks</span></span>

<span data-ttu-id="a6cd9-153">Razor 程式碼區塊的開頭`@`和由`{}`。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-153">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="a6cd9-154">不同於運算式中，C# 程式碼區塊內的程式碼不會轉譯。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-154">Unlike expressions, C# code inside code blocks is not rendered.</span></span> <span data-ttu-id="a6cd9-155">程式碼區塊和 Razor 頁面中的運算式共用相同的範圍，以及定義順序 （也就是在程式碼區塊中的宣告會有的更新版本的程式碼區塊和運算式的範圍內）。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-155">Code blocks and expressions in a Razor page share the same scope and are defined in order (that is, declarations in a code block will be in scope for later code blocks and expressions).</span></span>

```none
@{
    var output = "Hello World";
}

<p>The rendered result: @output</p>
```

<span data-ttu-id="a6cd9-156">會轉譯：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-156">Would render:</span></span>

```html
<p>The rendered result: Hello World</p>
   ```

<a name=implicit-transitions-label></a>

### <a name="implicit-transitions"></a><span data-ttu-id="a6cd9-157">隱含轉換</span><span class="sxs-lookup"><span data-stu-id="a6cd9-157">Implicit transitions</span></span>

<span data-ttu-id="a6cd9-158">程式碼區塊中的預設語言是 C# 中，但您可以轉換回 HTML。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-158">The default language in a code block is C#, but you can transition back to HTML.</span></span> <span data-ttu-id="a6cd9-159">HTML 程式碼區塊內就會轉換成 HTML 呈現：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-159">HTML within a code block will transition back into rendering HTML:</span></span>

```none
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

<a name=explicit-delimited-transition-label></a>

### <a name="explicit-delimited-transition"></a><span data-ttu-id="a6cd9-160">明確分隔的轉換</span><span class="sxs-lookup"><span data-stu-id="a6cd9-160">Explicit delimited transition</span></span>

<span data-ttu-id="a6cd9-161">若要定義應該呈現的 HTML 程式碼區塊的子區段，括住的字元來呈現使用 Razor`<text>`標記：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-161">To define a sub-section of a code block that should render HTML, surround the characters to be rendered with the Razor `<text>` tag:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="a6cd9-162">當您想要將不會圍繞著之 HTML 標記的 HTML 轉譯時，通常會使用此方法。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-162">You generally use this approach when you want to render HTML that is not surrounded by an HTML tag.</span></span> <span data-ttu-id="a6cd9-163">沒有 HTML 或 Razor 的標籤，您可以取得 Razor 執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-163">Without an HTML or Razor tag, you get a Razor runtime error.</span></span>

<a name=explicit-line-transition-with-label></a>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="a6cd9-164">使用明確列轉換`@:`</span><span class="sxs-lookup"><span data-stu-id="a6cd9-164">Explicit Line Transition with `@:`</span></span>

<span data-ttu-id="a6cd9-165">若要轉譯為 HTML 的整行的其餘部分的程式碼區塊內，使用`@:`語法：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-165">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="a6cd9-166">不含`@:`上述程式碼，您會取得 Razor，執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-166">Without the `@:` in the code above, you'd get a Razor run time error.</span></span>

<a name=control-structures-razor-label></a>

## <a name="control-structures"></a><span data-ttu-id="a6cd9-167">控制結構</span><span class="sxs-lookup"><span data-stu-id="a6cd9-167">Control Structures</span></span>

<span data-ttu-id="a6cd9-168">控制結構是一種擴充的程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-168">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="a6cd9-169">所有層面的程式碼區塊 （轉換到標記中，內嵌 C#） 也適都用於下列的結構。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-169">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures.</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="a6cd9-170">條件式`@if`， `else if`，`else`和`@switch`</span><span class="sxs-lookup"><span data-stu-id="a6cd9-170">Conditionals `@if`, `else if`, `else` and `@switch`</span></span>

<span data-ttu-id="a6cd9-171">`@if`系列可讓您控制執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-171">The `@if` family controls when code runs:</span></span>

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
```

<span data-ttu-id="a6cd9-172">`else`和`else if`不需要`@`符號：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-172">`else` and `else if` don't require the `@` symbol:</span></span>

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value was not large and is odd.</p>
}
```

<span data-ttu-id="a6cd9-173">您可以使用 switch 陳述式如下：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-173">You can use a switch statement like this:</span></span>

```none
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number was not 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="a6cd9-174">迴圈`@for`， `@foreach`， `@while`，和`@do while`</span><span class="sxs-lookup"><span data-stu-id="a6cd9-174">Looping `@for`, `@foreach`, `@while`, and `@do while`</span></span>

<span data-ttu-id="a6cd9-175">您可以將迴圈控制陳述式的樣板化 HTML 轉譯。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-175">You can render templated HTML with looping control statements.</span></span> <span data-ttu-id="a6cd9-176">例如，若要呈現的人員清單：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-176">For example, to render a list of people:</span></span>

```none
@{
    var people = new Person[]
    {
          new Person("John", 33),
          new Person("Doe", 41),
    };
}
```

<span data-ttu-id="a6cd9-177">您可以使用任何下列的迴圈陳述式：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-177">You can use any of the following looping statements:</span></span>

`@for`

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```none
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```none
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

```none
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a><span data-ttu-id="a6cd9-178">複合`@using`</span><span class="sxs-lookup"><span data-stu-id="a6cd9-178">Compound `@using`</span></span>

<span data-ttu-id="a6cd9-179">在 C# using 陳述式用來確保處置物件。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-179">In C# a using statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="a6cd9-180">Razor 中相同的機制可用來建立包含其他內容的 HTML helper。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-180">In Razor this same mechanism can be used to create HTML helpers that contain additional content.</span></span> <span data-ttu-id="a6cd9-181">比方說，我們可以利用 HTML Helper 呈現表單標記與`@using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-181">For instance, we can utilize HTML Helpers to render a form tag with the `@using` statement:</span></span>

```none
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" name="Email" value="" />
        <button type="submit"> Register </button>
    </div>
}
```

<span data-ttu-id="a6cd9-182">您也可以執行範圍層級的動作，如上所示使用[標記協助程式](tag-helpers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-182">You can also perform scope level actions like the above with [Tag Helpers](tag-helpers/index.md).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="a6cd9-183">`@try`, `catch`, `finally`</span><span class="sxs-lookup"><span data-stu-id="a6cd9-183">`@try`, `catch`, `finally`</span></span>

<span data-ttu-id="a6cd9-184">例外狀況處理會類似於 C#:</span><span class="sxs-lookup"><span data-stu-id="a6cd9-184">Exception handling is similar to  C#:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

<span data-ttu-id="a6cd9-185">Razor 具有保護與 lock 陳述式的關鍵區段的功能：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-185">Razor has the capability to protect critical sections with lock statements:</span></span>

```none
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="a6cd9-186">註解</span><span class="sxs-lookup"><span data-stu-id="a6cd9-186">Comments</span></span>

<span data-ttu-id="a6cd9-187">Razor 支援 C# 和 HTML 註解。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-187">Razor supports C# and HTML comments.</span></span> <span data-ttu-id="a6cd9-188">下列標記：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-188">The following markup:</span></span>

```none
@{
    /* C# comment. */
    // Another C# comment.
}
<!-- HTML comment -->
```

<span data-ttu-id="a6cd9-189">轉譯伺服器：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-189">Is rendered by the server as:</span></span>

```none
<!-- HTML comment -->
```

<span data-ttu-id="a6cd9-190">呈現頁面之前，伺服器會移除 razor 註解。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-190">Razor comments are removed by the server before the page is rendered.</span></span> <span data-ttu-id="a6cd9-191">使用 razor`@*  *@`來分隔註解。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-191">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="a6cd9-192">下列程式碼會標記為註解，所以伺服器將不會呈現任何標記：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-192">The following code is commented out, so the server will not render any markup:</span></span>

```none
 @*
 @{
     /* C# comment. */
     // Another C# comment.
 }
 <!-- HTML comment -->
*@
```

<a name=razor-directives-label></a>

## <a name="directives"></a><span data-ttu-id="a6cd9-193">指示詞</span><span class="sxs-lookup"><span data-stu-id="a6cd9-193">Directives</span></span>

<span data-ttu-id="a6cd9-194">Razor 指示詞都由下列保留的關鍵字的隱含運算式`@`符號。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-194">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="a6cd9-195">指示詞通常會變更網頁剖析的方式，或是啟用 Razor 頁面內的不同功能。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-195">A directive will typically change the way a page is parsed or enable different functionality within your Razor page.</span></span>

<span data-ttu-id="a6cd9-196">了解如何 Razor 產生檢視的程式碼將更易於了解指示詞的運作方式。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-196">Understanding how Razor generates code for a view will make it easier to understand how directives work.</span></span> <span data-ttu-id="a6cd9-197">Razor 頁面用來產生 C# 檔案。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-197">A Razor page is used to generate a C# file.</span></span> <span data-ttu-id="a6cd9-198">例如，此 Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-198">For example, this Razor page:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="a6cd9-199">產生的類別如下所示：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-199">Generates a class similar to the following:</span></span>

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Hello World";

        WriteLiteral("/r/n<div>Output: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

<span data-ttu-id="a6cd9-200">[檢視檢視產生的 Razor C# 類別](#razor-customcompilationservice-label)說明如何檢視這個產生的類別。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-200">[Viewing the Razor C# class generated for a view](#razor-customcompilationservice-label) explains how to view this generated class.</span></span>

### `@using`

<span data-ttu-id="a6cd9-201">`@using`指示詞會將 c#`using`產生的 razor 頁面指示詞：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-201">The `@using` directive will add the c# `using` directive to the generated razor page:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact9.cshtml)]

### `@model`

<span data-ttu-id="a6cd9-202">`@model`指示詞會指定傳遞至 Razor 頁面的模型型別。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-202">The `@model` directive specifies the type of the model passed to the Razor page.</span></span> <span data-ttu-id="a6cd9-203">其使用下列語法：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-203">It uses the following syntax:</span></span>

```none
@model TypeNameOfModel
   ```

<span data-ttu-id="a6cd9-204">例如，如果您建立的 ASP.NET Core MVC 應用程式與個別使用者帳戶*Views/Account/Login.cshtml* Razor 檢視包含下列模型宣告：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-204">For example, if you create an ASP.NET Core MVC app with individual user accounts, the *Views/Account/Login.cshtml* Razor view contains the following model declaration:</span></span>

```csharp
@model LoginViewModel
   ```

<span data-ttu-id="a6cd9-205">在上述的類別範例中，產生的類別繼承自`RazorPage<dynamic>`。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-205">In the preceding class example, the class generated inherits from `RazorPage<dynamic>`.</span></span> <span data-ttu-id="a6cd9-206">藉由新增`@model`您控制所繼承。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-206">By adding an `@model` you control what’s inherited.</span></span> <span data-ttu-id="a6cd9-207">例如</span><span class="sxs-lookup"><span data-stu-id="a6cd9-207">For example</span></span>

```csharp
@model LoginViewModel
   ```

<span data-ttu-id="a6cd9-208">會產生下列類別</span><span class="sxs-lookup"><span data-stu-id="a6cd9-208">Generates the following class</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
   ```

<span data-ttu-id="a6cd9-209">Razor 頁面公開`Model`屬性，以存取模型傳遞至網頁。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-209">Razor pages expose a `Model` property for accessing the model passed to the page.</span></span>

```html
<div>The Login Email: @Model.Email</div>
   ```

<span data-ttu-id="a6cd9-210">`@model`指示詞會指定此屬性的型別 (藉由指定`T`中`RazorPage<T>`對您的頁面產生的類別衍生自)。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-210">The `@model` directive specified the type of this property (by specifying the `T` in `RazorPage<T>` that the generated class for your page derives from).</span></span> <span data-ttu-id="a6cd9-211">如果您未指定`@model`指示詞`Model`屬性都屬於類型`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-211">If you don't specify the `@model` directive the `Model` property will be of type `dynamic`.</span></span> <span data-ttu-id="a6cd9-212">模型值會從控制器到檢視。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-212">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="a6cd9-213">請參閱[強類型模型和@model關鍵字](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-213">See [Strongly typed models and the @model keyword](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) for more information.</span></span>

### `@inherits`

<span data-ttu-id="a6cd9-214">`@inherits`指示詞讓您完整控制 Razor 頁面所繼承的類別：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-214">The `@inherits` directive gives you full control of the class your Razor page inherits:</span></span>

```none
@inherits TypeNameOfClassToInheritFrom
   ```

<span data-ttu-id="a6cd9-215">比方說，假設我們有下列自訂 Razor 頁面類型：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-215">For instance, let’s say we had the following custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="a6cd9-216">會產生下列 Razor `<div>Custom text: Hello World</div>`。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-216">The following Razor would generate `<div>Custom text: Hello World</div>`.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="a6cd9-217">您無法使用`@model`和`@inherits`相同頁面上。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-217">You can't use `@model` and `@inherits` on the same page.</span></span> <span data-ttu-id="a6cd9-218">您可以擁有`@inherits`中*_ViewImports.cshtml* Razor 網頁匯入的檔案。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-218">You can have `@inherits` in a *_ViewImports.cshtml* file that the Razor page imports.</span></span> <span data-ttu-id="a6cd9-219">例如，如果您的 Razor 檢視匯入下列*_ViewImports.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-219">For example, if your Razor view imported the following *_ViewImports.cshtml* file:</span></span>

[!code-html[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="a6cd9-220">下列的強型別的 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="a6cd9-220">The following strongly typed Razor page</span></span>

[!code-html[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="a6cd9-221">會產生這個 HTML 標記：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-221">Generates this HTML markup:</span></span>

```none
<div>The Login Email: Rick@contoso.com</div>
<div>Custom text: Hello World</div>
```

<span data-ttu-id="a6cd9-222">當傳遞"[Rick@contoso.com](mailto:Rick@contoso.com)"在模型中：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-222">When passed "[Rick@contoso.com](mailto:Rick@contoso.com)" in the model:</span></span>

   <span data-ttu-id="a6cd9-223">如需詳細資訊，請參閱 [Layout](layout.md)。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-223">See [Layout](layout.md) for more information.</span></span>

### `@inject`

<span data-ttu-id="a6cd9-224">`@inject`指示詞可讓您將從服務程式[服務容器](../../fundamentals/dependency-injection.md)插入 Razor 網頁使用。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-224">The `@inject` directive enables you to inject a service from your [service container](../../fundamentals/dependency-injection.md)  into your Razor page for use.</span></span> <span data-ttu-id="a6cd9-225">請參閱[檢視的相依性插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-225">See [Dependency injection into views](dependency-injection.md).</span></span>

<a name="functions"></a>

### `@functions`

<span data-ttu-id="a6cd9-226">`@functions`指示詞可讓您將函式層級內容加入至您的 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-226">The `@functions` directive enables you to add function level content to your Razor page.</span></span> <span data-ttu-id="a6cd9-227">其語法為：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-227">The syntax is:</span></span>

```none
@functions { // C# Code }
   ```

<span data-ttu-id="a6cd9-228">例如: </span><span class="sxs-lookup"><span data-stu-id="a6cd9-228">For example:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="a6cd9-229">會產生下列的 HTML 標記：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-229">Generates the following HTML markup:</span></span>

```none
<div>From method: Hello</div>
   ```

<span data-ttu-id="a6cd9-230">產生 Razor 的 C# 看起來像：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-230">The generated Razor C# looks like:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### `@section`

<span data-ttu-id="a6cd9-231">`@section`指示詞用於搭配[版面配置頁](layout.md)啟用檢視，以呈現在呈現的 HTML 頁面的不同部分的內容。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-231">The `@section` directive is used in conjunction with the [layout page](layout.md) to enable views to render content in different parts of the rendered HTML page.</span></span> <span data-ttu-id="a6cd9-232">請參閱[區段](layout.md#layout-sections-label)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-232">See [Sections](layout.md#layout-sections-label) for more information.</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="a6cd9-233">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a6cd9-233">Tag Helpers</span></span>

<span data-ttu-id="a6cd9-234">下列[標記協助程式](tag-helpers/index.md)指示詞的詳細說明所提供的連結。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-234">The following [Tag Helpers](tag-helpers/index.md) directives are detailed in the links provided.</span></span>

* [@addTagHelper](tag-helpers/intro.md#add-helper-label)
* [@removeTagHelper](tag-helpers/intro.md#remove-razor-directives-label)
* [@tagHelperPrefix](tag-helpers/intro.md#prefix-razor-directives-label)

<a name=razor-reserved-keywords-label></a>

## <a name="razor-reserved-keywords"></a><span data-ttu-id="a6cd9-235">Razor 保留關鍵字</span><span class="sxs-lookup"><span data-stu-id="a6cd9-235">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="a6cd9-236">Razor 關鍵字</span><span class="sxs-lookup"><span data-stu-id="a6cd9-236">Razor keywords</span></span>

* <span data-ttu-id="a6cd9-237">頁面 （需要 ASP.NET Core 2.0 和更新版本）</span><span class="sxs-lookup"><span data-stu-id="a6cd9-237">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="a6cd9-238">函式</span><span class="sxs-lookup"><span data-stu-id="a6cd9-238">functions</span></span>
* <span data-ttu-id="a6cd9-239">繼承</span><span class="sxs-lookup"><span data-stu-id="a6cd9-239">inherits</span></span>
* <span data-ttu-id="a6cd9-240">model</span><span class="sxs-lookup"><span data-stu-id="a6cd9-240">model</span></span>
* <span data-ttu-id="a6cd9-241">section</span><span class="sxs-lookup"><span data-stu-id="a6cd9-241">section</span></span>
* <span data-ttu-id="a6cd9-242">協助程式 （不支援 ASP.NET Core。）</span><span class="sxs-lookup"><span data-stu-id="a6cd9-242">helper   (Not supported by ASP.NET Core.)</span></span>

<span data-ttu-id="a6cd9-243">Razor 關鍵字可以以逸出`@(Razor Keyword)`，例如`@(functions)`。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-243">Razor keywords can be escaped with `@(Razor Keyword)`, for example `@(functions)`.</span></span> <span data-ttu-id="a6cd9-244">請參閱完整的範例。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-244">See the complete sample below.</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="a6cd9-245">C# Razor 關鍵字</span><span class="sxs-lookup"><span data-stu-id="a6cd9-245">C# Razor keywords</span></span>

* <span data-ttu-id="a6cd9-246">case</span><span class="sxs-lookup"><span data-stu-id="a6cd9-246">case</span></span>
* <span data-ttu-id="a6cd9-247">do</span><span class="sxs-lookup"><span data-stu-id="a6cd9-247">do</span></span>
* <span data-ttu-id="a6cd9-248">default</span><span class="sxs-lookup"><span data-stu-id="a6cd9-248">default</span></span>
* <span data-ttu-id="a6cd9-249">for</span><span class="sxs-lookup"><span data-stu-id="a6cd9-249">for</span></span>
* <span data-ttu-id="a6cd9-250">foreach</span><span class="sxs-lookup"><span data-stu-id="a6cd9-250">foreach</span></span>
* <span data-ttu-id="a6cd9-251">if</span><span class="sxs-lookup"><span data-stu-id="a6cd9-251">if</span></span>
* <span data-ttu-id="a6cd9-252">else</span><span class="sxs-lookup"><span data-stu-id="a6cd9-252">else</span></span>
* <span data-ttu-id="a6cd9-253">lock</span><span class="sxs-lookup"><span data-stu-id="a6cd9-253">lock</span></span>
* <span data-ttu-id="a6cd9-254">switch</span><span class="sxs-lookup"><span data-stu-id="a6cd9-254">switch</span></span>
* <span data-ttu-id="a6cd9-255">try</span><span class="sxs-lookup"><span data-stu-id="a6cd9-255">try</span></span>
* <span data-ttu-id="a6cd9-256">catch</span><span class="sxs-lookup"><span data-stu-id="a6cd9-256">catch</span></span>
* <span data-ttu-id="a6cd9-257">finally</span><span class="sxs-lookup"><span data-stu-id="a6cd9-257">finally</span></span>
* <span data-ttu-id="a6cd9-258">使用</span><span class="sxs-lookup"><span data-stu-id="a6cd9-258">using</span></span>
* <span data-ttu-id="a6cd9-259">while</span><span class="sxs-lookup"><span data-stu-id="a6cd9-259">while</span></span>

<span data-ttu-id="a6cd9-260">C# Razor 關鍵字必須為 double 以逸出`@(@C# Razor Keyword)`，例如`@(@case)`。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-260">C# Razor keywords need to be double escaped with `@(@C# Razor Keyword)`, for example `@(@case)`.</span></span> <span data-ttu-id="a6cd9-261">第一個`@`逸出 Razor 剖析器，第二個`@`逸出的 C# 剖析器。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-261">The first `@` escapes the Razor parser, the second `@` escapes the C# parser.</span></span> <span data-ttu-id="a6cd9-262">請參閱完整的範例。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-262">See the complete sample below.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="a6cd9-263">不使用 Razor 的保留的關鍵字</span><span class="sxs-lookup"><span data-stu-id="a6cd9-263">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="a6cd9-264">namespace</span><span class="sxs-lookup"><span data-stu-id="a6cd9-264">namespace</span></span>
* <span data-ttu-id="a6cd9-265">Class - 類別</span><span class="sxs-lookup"><span data-stu-id="a6cd9-265">class</span></span>

<a name=razor-customcompilationservice-label></a>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="a6cd9-266">檢視檢視產生的 Razor C# 類別</span><span class="sxs-lookup"><span data-stu-id="a6cd9-266">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="a6cd9-267">將下列類別加入至您的 ASP.NET Core MVC 專案：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-267">Add the following class to your ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Services/CustomCompilationService.cs)]

<span data-ttu-id="a6cd9-268">覆寫`ICompilationService`MVC 加上述類別; 事件類別</span><span class="sxs-lookup"><span data-stu-id="a6cd9-268">Override the `ICompilationService` added by MVC with the above class;</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=29-33)]

<span data-ttu-id="a6cd9-269">在 設定中斷點`Compile`方法`CustomCompilationService`和檢視`compilationContent`。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-269">Set a break point on the `Compile` method of `CustomCompilationService` and view `compilationContent`.</span></span>

![CompilationContent 文字視覺化檢視](razor/_static/tvr.png)

<a name="case"></a>
## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="a6cd9-271">檢視對應及區分大小寫</span><span class="sxs-lookup"><span data-stu-id="a6cd9-271">View lookups and case sensitivity</span></span>

<span data-ttu-id="a6cd9-272">Razor 檢視引擎會執行檢視的區分大小寫的查閱。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-272">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="a6cd9-273">然而，實際的查詢會決定基礎來源：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-273">However, the actual lookup is determined by the underlying source:</span></span>

* <span data-ttu-id="a6cd9-274">根據的來源檔案：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-274">File based source:</span></span> 

    * <span data-ttu-id="a6cd9-275">在作業系統上使用不區分大小寫的檔案系統 （例如 Windows)，實體檔案提供者對應是不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-275">On operating systems with case insensitive file systems (like Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="a6cd9-276">例如`return View("Test")`會導致`/Views/Home/Test.cshtml`，`/Views/home/test.cshtml`並會探索所有其他大小寫變體。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-276">For example `return View("Test")` would result in `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` and all other casing variants would be discovered.</span></span>
    * <span data-ttu-id="a6cd9-277">在區分大小寫的檔案系統中，包括 Linux、 OSX 和`EmbeddedFileProvider`，查閱會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-277">On case sensitive file systems, which includes Linux, OSX and `EmbeddedFileProvider`, lookups are case sensitive.</span></span> <span data-ttu-id="a6cd9-278">例如，`return View("Test")`會特別尋找`/Views/Home/Test.cshtml`。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-278">For example, `return View("Test")` would specifically look for `/Views/Home/Test.cshtml`.</span></span>
        
* <span data-ttu-id="a6cd9-279">先行編譯的檢視：</span><span class="sxs-lookup"><span data-stu-id="a6cd9-279">Precompiled views:</span></span>

   * <span data-ttu-id="a6cd9-280">ASP.Net Core 2.0 和更新版本中，查閱先行編譯的檢視是不區分大小寫在所有作業系統上。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-280">With ASP.Net Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="a6cd9-281">行為是在 Windows 上的實體檔案提供者的行為相同。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-281">The behavior is identical to physical file provider's behavior on Windows.</span></span> 
   <span data-ttu-id="a6cd9-282">注意： 如果兩個先行編譯的檢視只有大小寫不同，查詢的結果是不具決定性。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-282">Note: If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="a6cd9-283">開發人員都要比對的區域中，控制器和動作名稱的大小寫的檔案和目錄名稱的大小寫。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-283">Developers are encouraged to match the casing of file and directory names to the casing of area, controller and action names.</span></span> <span data-ttu-id="a6cd9-284">這樣會確保您的部署仍然無從驗證的基礎檔案系統。</span><span class="sxs-lookup"><span data-stu-id="a6cd9-284">This would ensure your deployments remain agnostic of the underlying file system.</span></span>
