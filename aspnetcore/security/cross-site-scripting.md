---
title: 防止 ASP.NET Core 中的跨網站腳本（XSS）
author: rick-anderson
description: 深入瞭解跨網站腳本（XSS）和技術，以在 ASP.NET Core 應用程式中解決此弱點。
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: 1d6f605dc336d8768b8a47e4995f119d198a61af
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667976"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a><span data-ttu-id="f384e-103">防止 ASP.NET Core 中的跨網站腳本（XSS）</span><span class="sxs-lookup"><span data-stu-id="f384e-103">Prevent Cross-Site Scripting (XSS) in ASP.NET Core</span></span>

<span data-ttu-id="f384e-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="f384e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f384e-105">跨網站腳本（XSS）是一種安全性弱點，可讓攻擊者將用戶端腳本（通常是 JavaScript）放入網頁中。</span><span class="sxs-lookup"><span data-stu-id="f384e-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="f384e-106">當其他使用者載入受影響的頁面時，會執行攻擊者的腳本，讓攻擊者竊取 cookie 和會話權杖，透過 DOM 操作變更網頁的內容，或將瀏覽器重新導向至另一個頁面。</span><span class="sxs-lookup"><span data-stu-id="f384e-106">When other users load affected pages the attacker's scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="f384e-107">當應用程式接受使用者輸入並將其輸出至頁面，而未進行驗證、編碼或將它加以轉義時，通常會發生 XSS 弱點。</span><span class="sxs-lookup"><span data-stu-id="f384e-107">XSS vulnerabilities generally occur when an application takes user input and outputs it to a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="f384e-108">保護您的應用程式免于 XSS</span><span class="sxs-lookup"><span data-stu-id="f384e-108">Protecting your application against XSS</span></span>

<span data-ttu-id="f384e-109">在基本層級 XSS 的運作方式是將您的應用程式，藉由將 `<script>` 標記插入轉譯的頁面中，或將 `On*` 事件插入元素中。</span><span class="sxs-lookup"><span data-stu-id="f384e-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="f384e-110">開發人員應該使用下列預防步驟來避免將 XSS 引入其應用程式。</span><span class="sxs-lookup"><span data-stu-id="f384e-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="f384e-111">絕對不要將不受信任的資料放入您的 HTML 輸入，除非您遵循下列其餘步驟。</span><span class="sxs-lookup"><span data-stu-id="f384e-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="f384e-112">不受信任的資料是任何可能受到攻擊者控制的資料、HTML 表單輸入、查詢字串、HTTP 標頭，甚至是以攻擊者的形式來自資料庫的資料，即使無法違反您的應用程式，也可能會破壞您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f384e-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="f384e-113">將不受信任的資料放在 HTML 專案中之前，請確定它是以 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f384e-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="f384e-114">HTML 編碼會採用 &lt; 之類的字元，並將它們變更為安全形式，例如 &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="f384e-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="f384e-115">將不受信任的資料放入 HTML 屬性之前，請確定它是以 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f384e-115">Before putting untrusted data into an HTML attribute ensure it's HTML encoded.</span></span> <span data-ttu-id="f384e-116">HTML 屬性編碼是 HTML 編碼的超集合，並會將其他字元編碼，例如 "and"。</span><span class="sxs-lookup"><span data-stu-id="f384e-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="f384e-117">將不受信任的資料放入 JavaScript 之前，請將資料放入您在執行時間中取得其內容的 HTML 專案。</span><span class="sxs-lookup"><span data-stu-id="f384e-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="f384e-118">如果無法這麼做，請確定資料已進行 JavaScript 編碼。</span><span class="sxs-lookup"><span data-stu-id="f384e-118">If this isn't possible, then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="f384e-119">JavaScript 編碼會針對 JavaScript 採取危險的字元，並以其十六進位加以取代，例如 &lt; 會編碼為 `\u003C`。</span><span class="sxs-lookup"><span data-stu-id="f384e-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="f384e-120">將不受信任的資料放入 URL 查詢字串之前，請確定其 URL 已編碼。</span><span class="sxs-lookup"><span data-stu-id="f384e-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="f384e-121">使用 Razor 的 HTML 編碼</span><span class="sxs-lookup"><span data-stu-id="f384e-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="f384e-122">在 MVC 中使用的 Razor 引擎會自動將來引數的所有輸出編碼，除非您真的難以避免它執行此作業。</span><span class="sxs-lookup"><span data-stu-id="f384e-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="f384e-123">當您使用 *@* 指示詞時，它會使用 HTML 屬性編碼規則。</span><span class="sxs-lookup"><span data-stu-id="f384e-123">It uses HTML attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="f384e-124">HTML 屬性編碼是 HTML 編碼的超集合，這表示您不需要擔心您是否應該使用 HTML 編碼或 HTML 屬性編碼。</span><span class="sxs-lookup"><span data-stu-id="f384e-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="f384e-125">您必須確定只在 HTML 內容中使用 @，而不是在嘗試將不受信任的輸入直接插入 JavaScript 中。</span><span class="sxs-lookup"><span data-stu-id="f384e-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="f384e-126">標記協助程式也會將您在標記參數中使用的輸入編碼。</span><span class="sxs-lookup"><span data-stu-id="f384e-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="f384e-127">採取下列 Razor 視圖：</span><span class="sxs-lookup"><span data-stu-id="f384e-127">Take the following Razor view:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="f384e-128">此視圖會輸出*untrustedInput*變數的內容。</span><span class="sxs-lookup"><span data-stu-id="f384e-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="f384e-129">此變數包含一些在 XSS 攻擊中使用的字元，也就是 &lt;、"和 &gt;。</span><span class="sxs-lookup"><span data-stu-id="f384e-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="f384e-130">檢查來源會顯示編碼為的轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="f384e-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="f384e-131">ASP.NET Core MVC 提供的 `HtmlString` 類別不會在輸出時自動編碼。</span><span class="sxs-lookup"><span data-stu-id="f384e-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="f384e-132">這不應該與不受信任的輸入結合使用，因為這會公開 XSS 弱點。</span><span class="sxs-lookup"><span data-stu-id="f384e-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="f384e-133">使用 Razor 的 JavaScript 編碼</span><span class="sxs-lookup"><span data-stu-id="f384e-133">JavaScript Encoding using Razor</span></span>

<span data-ttu-id="f384e-134">有時候，您可能會想要將值插入 JavaScript 中，以便在您的視圖中處理。</span><span class="sxs-lookup"><span data-stu-id="f384e-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="f384e-135">做法有二種。</span><span class="sxs-lookup"><span data-stu-id="f384e-135">There are two ways to do this.</span></span> <span data-ttu-id="f384e-136">插入值最安全的方式是將值放在標記的資料屬性中，然後在您的 JavaScript 中加以取出。</span><span class="sxs-lookup"><span data-stu-id="f384e-136">The safest way to insert values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="f384e-137">例如：</span><span class="sxs-lookup"><span data-stu-id="f384e-137">For example:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="f384e-138">這會產生下列 HTML</span><span class="sxs-lookup"><span data-stu-id="f384e-138">This will produce the following HTML</span></span>

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="f384e-139">當它執行時，會轉譯下列內容：</span><span class="sxs-lookup"><span data-stu-id="f384e-139">Which, when it runs, will render the following:</span></span>

```
<"123">
   <"123">
```

<span data-ttu-id="f384e-140">您也可以直接呼叫 JavaScript 編碼器：</span><span class="sxs-lookup"><span data-stu-id="f384e-140">You can also call the JavaScript encoder directly:</span></span>

```cshtml
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
```

<span data-ttu-id="f384e-141">這會在瀏覽器中呈現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="f384e-141">This will render in the browser as follows:</span></span>

```html
<script>
    document.write("\u003C\u0022123\u0022\u003E");
</script>
```

>[!WARNING]
> <span data-ttu-id="f384e-142">請勿在 JavaScript 中串連不受信任的輸入，以建立 DOM 元素。</span><span class="sxs-lookup"><span data-stu-id="f384e-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="f384e-143">您應該使用 `createElement()` 並適當地指派屬性值（例如 `node.TextContent=`），或使用 `element.SetAttribute()`/`element[attribute]=` 否則會向您公開至以 DOM 為基礎的 XSS。</span><span class="sxs-lookup"><span data-stu-id="f384e-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="f384e-144">在程式碼中存取編碼器</span><span class="sxs-lookup"><span data-stu-id="f384e-144">Accessing encoders in code</span></span>

<span data-ttu-id="f384e-145">HTML、JavaScript 和 URL 編碼器可透過兩種方式提供給您的程式碼，您可以透過相依性[插入](xref:fundamentals/dependency-injection)來插入它們，也可以使用 `System.Text.Encodings.Web` 命名空間中包含的預設編碼器。</span><span class="sxs-lookup"><span data-stu-id="f384e-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](xref:fundamentals/dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="f384e-146">如果您使用預設編碼器，則套用至字元範圍的任何都將不會生效-預設編碼器會使用最安全的編碼規則。</span><span class="sxs-lookup"><span data-stu-id="f384e-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="f384e-147">若要透過 DI 使用可設定的編碼器，您的函式應該適當地採用*HtmlEncoder*、 *JavaScriptEncoder*和*UrlEncoder*參數。</span><span class="sxs-lookup"><span data-stu-id="f384e-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="f384e-148">例如：</span><span class="sxs-lookup"><span data-stu-id="f384e-148">For example;</span></span>

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a><span data-ttu-id="f384e-149">編碼 URL 參數</span><span class="sxs-lookup"><span data-stu-id="f384e-149">Encoding URL Parameters</span></span>

<span data-ttu-id="f384e-150">如果您想要以不受信任的輸入建立 URL 查詢字串做為值，請使用 `UrlEncoder` 來編碼值。</span><span class="sxs-lookup"><span data-stu-id="f384e-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="f384e-151">例如，</span><span class="sxs-lookup"><span data-stu-id="f384e-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="f384e-152">編碼之後，Url-encodedvalue 變數會包含 `%22Quoted%20Value%20with%20spaces%20and%20%26%22`。</span><span class="sxs-lookup"><span data-stu-id="f384e-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="f384e-153">空格、引號、標點符號和其他不安全的字元會以百分比編碼為其十六進位值，例如空白字元將會變成 %20。</span><span class="sxs-lookup"><span data-stu-id="f384e-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="f384e-154">請勿使用不受信任的輸入做為 URL 路徑的一部分。</span><span class="sxs-lookup"><span data-stu-id="f384e-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="f384e-155">一律傳遞不受信任的輸入做為查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="f384e-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="f384e-156">自訂編碼器</span><span class="sxs-lookup"><span data-stu-id="f384e-156">Customizing the Encoders</span></span>

<span data-ttu-id="f384e-157">根據預設，編碼器會使用限制為基本拉丁 Unicode 範圍的安全清單，並將該範圍以外的所有字元編碼為其對等的字元碼。</span><span class="sxs-lookup"><span data-stu-id="f384e-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="f384e-158">這種行為也會影響 Razor TagHelper 和 HtmlHelper 轉譯，因為它會使用編碼器來輸出您的字串。</span><span class="sxs-lookup"><span data-stu-id="f384e-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="f384e-159">其背後的原因是要防範未知或未來的瀏覽器錯誤（先前的瀏覽器 bug 已根據非英文字元的處理來剖析）。</span><span class="sxs-lookup"><span data-stu-id="f384e-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="f384e-160">如果您的網站大量使用非拉丁字元，例如中文、斯拉夫或其他，這可能不是您想要的行為。</span><span class="sxs-lookup"><span data-stu-id="f384e-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="f384e-161">在 `ConfigureServices()`中，您可以自訂編碼器安全清單，以包含您的應用程式在啟動期間適用的 Unicode 範圍。</span><span class="sxs-lookup"><span data-stu-id="f384e-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="f384e-162">例如，使用預設設定時，您可能會使用 Razor HtmlHelper，如下所示;</span><span class="sxs-lookup"><span data-stu-id="f384e-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="f384e-163">當您查看網頁的來源時，您會看到其呈現方式如下：以中文文字編碼;</span><span class="sxs-lookup"><span data-stu-id="f384e-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="f384e-164">若要加寬編碼器視為安全的字元，您可以在 `startup.cs`的 `ConfigureServices()` 方法中插入下面這一行;</span><span class="sxs-lookup"><span data-stu-id="f384e-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="f384e-165">這個範例會擴大安全清單，以包含 Unicode 範圍 CjkUnifiedIdeographs。</span><span class="sxs-lookup"><span data-stu-id="f384e-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="f384e-166">呈現的輸出現在會變成</span><span class="sxs-lookup"><span data-stu-id="f384e-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="f384e-167">安全清單範圍會指定為 Unicode 程式碼圖表，而不是語言。</span><span class="sxs-lookup"><span data-stu-id="f384e-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="f384e-168">[Unicode 標準](https://unicode.org/)有一份程式[代碼圖表](https://www.unicode.org/charts/index.html)清單，您可以用來尋找包含您字元的圖表。</span><span class="sxs-lookup"><span data-stu-id="f384e-168">The [Unicode standard](https://unicode.org/) has a list of [code charts](https://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="f384e-169">每個編碼器、Html、JavaScript 和 Url 都必須分別設定。</span><span class="sxs-lookup"><span data-stu-id="f384e-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="f384e-170">安全清單的自訂只會影響透過 DI 來源的編碼器。</span><span class="sxs-lookup"><span data-stu-id="f384e-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="f384e-171">如果您透過 `System.Text.Encodings.Web.*Encoder.Default` 直接存取編碼器，則會使用預設的 [僅限基本的拉丁] 安全功能。</span><span class="sxs-lookup"><span data-stu-id="f384e-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="f384e-172">應該在哪裡進行編碼？</span><span class="sxs-lookup"><span data-stu-id="f384e-172">Where should encoding take place?</span></span>

<span data-ttu-id="f384e-173">一般接受的作法是，編碼會在輸出點進行，而且編碼的值永遠不會儲存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="f384e-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="f384e-174">在輸出點進行編碼可讓您將資料的使用方式（例如，從 HTML 變更為查詢字串值）。</span><span class="sxs-lookup"><span data-stu-id="f384e-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="f384e-175">它也可讓您輕鬆地搜尋資料，而不需要在搜尋之前編碼值，並可讓您利用對編碼器所做的任何變更或錯誤修正。</span><span class="sxs-lookup"><span data-stu-id="f384e-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="f384e-176">驗證為 XSS 防護技術</span><span class="sxs-lookup"><span data-stu-id="f384e-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="f384e-177">驗證可能是限制 XSS 攻擊的有用工具。</span><span class="sxs-lookup"><span data-stu-id="f384e-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="f384e-178">例如，只包含0-9 個字元的數值字串不會觸發 XSS 攻擊。</span><span class="sxs-lookup"><span data-stu-id="f384e-178">For example, a numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="f384e-179">接受使用者輸入中的 HTML 時，驗證會變得更複雜。</span><span class="sxs-lookup"><span data-stu-id="f384e-179">Validation becomes more complicated when accepting HTML in user input.</span></span> <span data-ttu-id="f384e-180">如果不可能，剖析 HTML 輸入很難。</span><span class="sxs-lookup"><span data-stu-id="f384e-180">Parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="f384e-181">Markdown 結合了內嵌 HTML 的剖析器，是接受豐富輸入的更安全選項。</span><span class="sxs-lookup"><span data-stu-id="f384e-181">Markdown, coupled with a parser that strips embedded HTML, is a safer option for accepting rich input.</span></span> <span data-ttu-id="f384e-182">切勿單獨依賴驗證。</span><span class="sxs-lookup"><span data-stu-id="f384e-182">Never rely on validation alone.</span></span> <span data-ttu-id="f384e-183">無論執行何種驗證或清理，一律會在輸出前編碼不受信任的輸入。</span><span class="sxs-lookup"><span data-stu-id="f384e-183">Always encode untrusted input before output, no matter what validation or sanitization has been performed.</span></span>
