---
title: 防止跨網站指令碼 (XSS) ASP.NET Core 中
author: rick-anderson
description: 了解跨網站指令碼 (XSS) 和解決此一漏洞的 ASP.NET Core 應用程式中的技術。
ms.author: riande
ms.date: 10/14/2016
uid: security/cross-site-scripting
ms.openlocfilehash: 4784b1775d955f0ef00526e50b960fc873ea218d
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342207"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a><span data-ttu-id="c8905-103">防止跨網站指令碼 (XSS) ASP.NET Core 中</span><span class="sxs-lookup"><span data-stu-id="c8905-103">Prevent Cross-Site Scripting (XSS) in ASP.NET Core</span></span>

<span data-ttu-id="c8905-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c8905-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c8905-105">跨網站指令碼 (XSS) 是可讓攻擊者將用戶端指令碼 (通常是 JavaScript) 放入網頁的安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="c8905-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="c8905-106">當其他使用者載入攻擊者的指令碼會執行受影響的頁面時，讓攻擊者竊取 cookie 和工作階段權杖變更透過 DOM 操作之 web 網頁內容，或瀏覽器重新導向至其他頁面。</span><span class="sxs-lookup"><span data-stu-id="c8905-106">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="c8905-107">應用程式接受使用者輸入，並將其輸出在頁面中，而不需要驗證、 編碼或逸出它時，通常會發生 XSS 弱點。</span><span class="sxs-lookup"><span data-stu-id="c8905-107">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="c8905-108">保護您的應用程式免遭 XSS</span><span class="sxs-lookup"><span data-stu-id="c8905-108">Protecting your application against XSS</span></span>

<span data-ttu-id="c8905-109">在基本層級 XSS 的運作方式是矇騙應用程式插入`<script>`標記為您呈現的頁面上，或藉由插入`On*`事件加入項目。</span><span class="sxs-lookup"><span data-stu-id="c8905-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="c8905-110">開發人員應該使用下列預防步驟，以避免產生 XSS 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8905-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="c8905-111">除非您是遵循下列步驟的其餘部分，永遠不會放入 HTML 輸入之用，不受信任的資料。</span><span class="sxs-lookup"><span data-stu-id="c8905-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="c8905-112">任何可能受到攻擊者、 HTML 表單的輸入、 查詢字串、 HTTP 標頭，甚至是資料來自攻擊者可能會破壞您的資料庫，即使它們不能違反您的應用程式資料庫的資料不受信任的資料。</span><span class="sxs-lookup"><span data-stu-id="c8905-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="c8905-113">HTML 項目內的不信任的資料之前，請先確認其為 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="c8905-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="c8905-114">這類 HTML 編碼會採用字元&lt;變成安全的表單和&amp;l t;</span><span class="sxs-lookup"><span data-stu-id="c8905-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="c8905-115">之前將不受信任的資料放入 HTML 屬性，請確定它是編碼的 HTML 屬性。</span><span class="sxs-lookup"><span data-stu-id="c8905-115">Before putting untrusted data into an HTML attribute ensure it's HTML attribute encoded.</span></span> <span data-ttu-id="c8905-116">HTML 屬性編碼是 HTML 編碼的超集，例如將額外的字元編碼成"和 '。</span><span class="sxs-lookup"><span data-stu-id="c8905-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="c8905-117">之前將不受信任的資料放入 JavaScript 將您在執行階段擷取其內容的 HTML 項目中的資料。</span><span class="sxs-lookup"><span data-stu-id="c8905-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="c8905-118">如果這不可行，請確定資料是 JavaScript 編碼。</span><span class="sxs-lookup"><span data-stu-id="c8905-118">If this isn't possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="c8905-119">JavaScript 的編碼方式會適用於 JavaScript 的危險的字元，取代成其 hex，比方說&lt;會編碼為`\u003C`。</span><span class="sxs-lookup"><span data-stu-id="c8905-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="c8905-120">將不受信任的資料放入的 URL 查詢字串之前，請先確認其為 URL 編碼。</span><span class="sxs-lookup"><span data-stu-id="c8905-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="c8905-121">使用 Razor 的 HTML 編碼</span><span class="sxs-lookup"><span data-stu-id="c8905-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="c8905-122">Razor 引擎會自動使用在 MVC 中編碼所有輸出源自變數，除非您真的很努力避免這種方式。</span><span class="sxs-lookup"><span data-stu-id="c8905-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="c8905-123">它會使用 HTML 編碼規則，每當您使用的屬性*@* 指示詞。</span><span class="sxs-lookup"><span data-stu-id="c8905-123">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="c8905-124">為 HTML 屬性編碼會是這表示您不必擔心自己是否應使用 HTML 編碼或 HTML 屬性編碼的 HTML 編碼的超集。</span><span class="sxs-lookup"><span data-stu-id="c8905-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="c8905-125">您必須確定您只使用在 HTML 內容中，不會在嘗試直接插入 JavaScript 不受信任的輸入。</span><span class="sxs-lookup"><span data-stu-id="c8905-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="c8905-126">標籤協助程式也會將編碼的輸入您在 tag 參數中使用。</span><span class="sxs-lookup"><span data-stu-id="c8905-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="c8905-127">採取下列 Razor 檢視中;</span><span class="sxs-lookup"><span data-stu-id="c8905-127">Take the following Razor view;</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="c8905-128">此檢視輸出的內容*untrustedInput*變數。</span><span class="sxs-lookup"><span data-stu-id="c8905-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="c8905-129">此變數包含中 XSS 攻擊，也就是使用某些字元&lt;，"和&gt;。</span><span class="sxs-lookup"><span data-stu-id="c8905-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="c8905-130">檢查來源顯示轉譯的輸出編碼為：</span><span class="sxs-lookup"><span data-stu-id="c8905-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="c8905-131">ASP.NET Core MVC 提供`HtmlString`類別可在輸出時不會自動編碼。</span><span class="sxs-lookup"><span data-stu-id="c8905-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="c8905-132">這應該永遠不會用於具有不受信任的輸入組合，這將會公開 XSS 的安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="c8905-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="c8905-133">使用 Razor Javascript 編碼</span><span class="sxs-lookup"><span data-stu-id="c8905-133">Javascript Encoding using Razor</span></span>

<span data-ttu-id="c8905-134">有時候可能會想要將值插入您的檢視中要處理的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="c8905-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="c8905-135">執行這項作業的方法有兩種。</span><span class="sxs-lookup"><span data-stu-id="c8905-135">There are two ways to do this.</span></span> <span data-ttu-id="c8905-136">將值插入最安全的方法是將值放在標記的資料屬性，並擷取在 JavaScript 中。</span><span class="sxs-lookup"><span data-stu-id="c8905-136">The safest way to insert  values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="c8905-137">例如: </span><span class="sxs-lookup"><span data-stu-id="c8905-137">For example:</span></span>

```none
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

<span data-ttu-id="c8905-138">這會產生下列 HTML</span><span class="sxs-lookup"><span data-stu-id="c8905-138">This will produce the following HTML</span></span>

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

<span data-ttu-id="c8905-139">它執行時，會轉譯下列作業：</span><span class="sxs-lookup"><span data-stu-id="c8905-139">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="c8905-140">您也可以直接呼叫 JavaScript 編碼器</span><span class="sxs-lookup"><span data-stu-id="c8905-140">You can also call the JavaScript encoder directly,</span></span>

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="c8905-141">這會如下所示; 呈現在瀏覽器</span><span class="sxs-lookup"><span data-stu-id="c8905-141">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="c8905-142">不受信任的輸入，在 JavaScript 中建立 DOM 項目不串連。</span><span class="sxs-lookup"><span data-stu-id="c8905-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="c8905-143">您應該使用`createElement()`，並適當地例如指派屬性值`node.TextContent=`，或使用`element.SetAttribute()` / `element[attribute]=`否則您會面臨 DOM 式 XSS。</span><span class="sxs-lookup"><span data-stu-id="c8905-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="c8905-144">存取程式碼中的編碼器</span><span class="sxs-lookup"><span data-stu-id="c8905-144">Accessing encoders in code</span></span>

<span data-ttu-id="c8905-145">HTML、 JavaScript 和 URL 編碼器可有兩種程式碼，您可以將之插入透過[相依性插入](xref:fundamentals/dependency-injection)或您可以使用包含在預設編碼器`System.Text.Encodings.Web`命名空間。</span><span class="sxs-lookup"><span data-stu-id="c8905-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](xref:fundamentals/dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="c8905-146">如果您使用的預設編碼器，則您套用至任何處理為安全的字元範圍才會生效-預設編碼器使用最安全的編碼規則可能。</span><span class="sxs-lookup"><span data-stu-id="c8905-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="c8905-147">若要使用的可設定的編碼器，透過您的建構函式應該採用的 DI *HtmlEncoder*， *JavaScriptEncoder*並*UrlEncoder*適當的參數。</span><span class="sxs-lookup"><span data-stu-id="c8905-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="c8905-148">例如，</span><span class="sxs-lookup"><span data-stu-id="c8905-148">For example;</span></span>

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

## <a name="encoding-url-parameters"></a><span data-ttu-id="c8905-149">編碼的 URL 參數</span><span class="sxs-lookup"><span data-stu-id="c8905-149">Encoding URL Parameters</span></span>

<span data-ttu-id="c8905-150">如果您想要建置具有不受信任的輸入，當作值使用的 URL 查詢字串`UrlEncoder`編碼值。</span><span class="sxs-lookup"><span data-stu-id="c8905-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="c8905-151">例如，套用至物件的</span><span class="sxs-lookup"><span data-stu-id="c8905-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="c8905-152">變數會包含編碼 encodedValue 之後`%22Quoted%20Value%20with%20spaces%20and%20%26%22`。</span><span class="sxs-lookup"><span data-stu-id="c8905-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="c8905-153">空格、 引號、 標點符號和其他不安全字元就是百分比編碼成其十六進位值，例如空格字元會變成 %20。</span><span class="sxs-lookup"><span data-stu-id="c8905-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="c8905-154">請勿使用不受信任的輸入的 URL 路徑的一部分。</span><span class="sxs-lookup"><span data-stu-id="c8905-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="c8905-155">一律傳遞不受信任的輸入作為查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="c8905-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="c8905-156">自訂編碼器</span><span class="sxs-lookup"><span data-stu-id="c8905-156">Customizing the Encoders</span></span>

<span data-ttu-id="c8905-157">預設編碼器會使用基本拉丁文 Unicode 範圍僅限安全清單，並為其對等的字元碼超出該範圍的所有字元都編碼。</span><span class="sxs-lookup"><span data-stu-id="c8905-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="c8905-158">此行為也會影響 Razor TagHelper 與 HtmlHelper 轉譯，因為它會使用編碼器，輸出字串。</span><span class="sxs-lookup"><span data-stu-id="c8905-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="c8905-159">背後的原因是要防止未知或未來的瀏覽器錯誤 （先前的瀏覽器錯誤有挫敗剖析以非英文字元的處理）。</span><span class="sxs-lookup"><span data-stu-id="c8905-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="c8905-160">如果您的網站會大量使用非拉丁字元，例如中文，斯拉夫文或其他人不可能您想要的行為。</span><span class="sxs-lookup"><span data-stu-id="c8905-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="c8905-161">您可以自訂編碼器安全清單，以包含在您的應用程式在啟動期間，適合範圍的 Unicode `ConfigureServices()`。</span><span class="sxs-lookup"><span data-stu-id="c8905-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="c8905-162">例如，使用預設組態，您可以使用 Razor HtmlHelper 就像這樣;</span><span class="sxs-lookup"><span data-stu-id="c8905-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="c8905-163">當您檢視網頁的原始檔時您會看到轉譯，如下所示，與編碼; 的中文文字</span><span class="sxs-lookup"><span data-stu-id="c8905-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="c8905-164">若要擴大字元視為安全的編碼器中您會插入下列行插入`ConfigureServices()`方法中的`startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="c8905-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="c8905-165">此範例中，將擴展安全清單，以包含 Unicode 範圍 CjkUnifiedIdeographs。</span><span class="sxs-lookup"><span data-stu-id="c8905-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="c8905-166">現在就會變成轉譯的輸出</span><span class="sxs-lookup"><span data-stu-id="c8905-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="c8905-167">安全清單範圍指定為 Unicode 字碼圖表，不是語言。</span><span class="sxs-lookup"><span data-stu-id="c8905-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="c8905-168">[Unicode 標準](http://unicode.org/)有一份[程式碼圖表](http://www.unicode.org/charts/index.html)可用來尋找圖表，其中包含您的字元。</span><span class="sxs-lookup"><span data-stu-id="c8905-168">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="c8905-169">每個編碼器、 Html、 JavaScript 和 Url，必須個別設定。</span><span class="sxs-lookup"><span data-stu-id="c8905-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="c8905-170">自訂安全清單只會影響透過 DI 取得資料來源的編碼器。</span><span class="sxs-lookup"><span data-stu-id="c8905-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="c8905-171">如果您直接存取透過編碼器`System.Text.Encodings.Web.*Encoder.Default`然後預設值、 基本拉丁文將用於只安全清單。</span><span class="sxs-lookup"><span data-stu-id="c8905-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="c8905-172">編碼的 take 應放置的地方？</span><span class="sxs-lookup"><span data-stu-id="c8905-172">Where should encoding take place?</span></span>

<span data-ttu-id="c8905-173">所接受的一般作法是，編碼會發生在輸出時，並編碼的值應該永遠不會儲存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="c8905-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="c8905-174">在輸出的編碼方式，可讓您變更的資料，例如，使用從查詢字串值的 HTML。</span><span class="sxs-lookup"><span data-stu-id="c8905-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="c8905-175">它也可讓您輕鬆地搜尋您的資料，而不需要編碼之前搜尋的值，並可讓您充分任何的利用變更或對編碼器的 bug 修正。</span><span class="sxs-lookup"><span data-stu-id="c8905-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="c8905-176">XSS 防護技術以及驗證</span><span class="sxs-lookup"><span data-stu-id="c8905-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="c8905-177">驗證可以是一個有用的工具，在限制 XSS 攻擊。</span><span class="sxs-lookup"><span data-stu-id="c8905-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="c8905-178">例如，數字的字串，包含只有字元 0-9 不會觸發 XSS 攻擊。</span><span class="sxs-lookup"><span data-stu-id="c8905-178">For example, a numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="c8905-179">驗證變得更複雜的是如果您想要接受使用者輸入層中的 HTML 剖析 HTML 輸入是很困難，不可能的。</span><span class="sxs-lookup"><span data-stu-id="c8905-179">Validation becomes more complicated should you wish to accept HTML in user input - parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="c8905-180">MarkDown 和其他文字格式會是比較安全的選項為豐富的輸入。</span><span class="sxs-lookup"><span data-stu-id="c8905-180">MarkDown and other text formats would be a safer option for rich input.</span></span> <span data-ttu-id="c8905-181">您永遠不應該依賴單獨的驗證。</span><span class="sxs-lookup"><span data-stu-id="c8905-181">You should never rely on validation alone.</span></span> <span data-ttu-id="c8905-182">一律將編碼不受信任的輸入，輸出之前，無論何種驗證您已執行。</span><span class="sxs-lookup"><span data-stu-id="c8905-182">Always encode untrusted input before output, no matter what validation you have performed.</span></span>
