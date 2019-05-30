---
title: 防止跨網站指令碼 (XSS) ASP.NET Core 中
author: rick-anderson
description: 了解跨網站指令碼 (XSS) 和解決此一漏洞的 ASP.NET Core 應用程式中的技術。
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: 50f0211a2c64708d9b788dd10ce9064e66014d55
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895345"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a>防止跨網站指令碼 (XSS) ASP.NET Core 中

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

跨網站指令碼 (XSS) 是可讓攻擊者將用戶端指令碼 (通常是 JavaScript) 放入網頁的安全性弱點。 當其他使用者載入攻擊者的指令碼會執行受影響的頁面時，讓攻擊者竊取 cookie 和工作階段權杖變更透過 DOM 操作之 web 網頁內容，或瀏覽器重新導向至其他頁面。 應用程式會接受使用者輸入，並將它輸出到頁面中，而不需要驗證、 編碼或逸出它時，通常會發生 XSS 弱點。

## <a name="protecting-your-application-against-xss"></a>保護您的應用程式免遭 XSS

在基本層級 XSS 的運作方式是矇騙應用程式插入`<script>`標記為您呈現的頁面上，或藉由插入`On*`事件加入項目。 開發人員應該使用下列預防步驟，以避免產生 XSS 應用程式。

1. 除非您是遵循下列步驟的其餘部分，永遠不會放入 HTML 輸入之用，不受信任的資料。 任何可能受到攻擊者、 HTML 表單的輸入、 查詢字串、 HTTP 標頭，甚至是資料來自攻擊者可能會破壞您的資料庫，即使它們不能違反您的應用程式資料庫的資料不受信任的資料。

2. HTML 項目內的不信任的資料之前，請先確認其為 HTML 編碼。 這類 HTML 編碼會採用字元&lt;變成安全的表單和&amp;l t;

3. 將不受信任的資料放入 HTML 屬性之前，請先確認已對它進行 HTML 編碼。 HTML 屬性編碼是 HTML 編碼的超集，而且會將額外的字元 (例如 " 與 ') 編碼。

4. 之前將不受信任的資料放入 JavaScript 將您在執行階段擷取其內容的 HTML 項目中的資料。 如果這不可行，請確定資料是 JavaScript 的編碼。 JavaScript 的編碼方式會適用於 JavaScript 的危險的字元，取代成其 hex，比方說&lt;會編碼為`\u003C`。

5. 將不受信任的資料放入 URL 查詢字串之前，請先確認已為它進行 URL 編碼。

## <a name="html-encoding-using-razor"></a>使用 Razor 的 HTML 編碼

Razor 引擎會自動使用在 MVC 中編碼所有輸出源自變數，除非您真的很努力避免這種方式。 它會使用 HTML 屬性編碼規則，每當您使用 *@* 指示詞。 為 HTML 屬性編碼會是這表示您不必擔心自己是否應使用 HTML 編碼或 HTML 屬性編碼的 HTML 編碼的超集。 您必須確定您只使用在 HTML 內容中，不會在嘗試直接插入 JavaScript 不受信任的輸入。 標籤協助程式也會將編碼的輸入您在 tag 參數中使用。

採取下列 Razor 檢視：

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

此檢視輸出的內容*untrustedInput*變數。 此變數包含中 XSS 攻擊，也就是使用某些字元&lt;，"和&gt;。 檢查來源顯示轉譯的輸出編碼為：

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET Core MVC 提供 `HtmlString` 類別，它不會在輸出時自動編碼。 這永遠不應該結合不受信任的輸入使用，因為這將會公開 XSS 弱點。

## <a name="javascript-encoding-using-razor"></a>使用 Razor JavaScript 編碼

有時候可能會想要將值插入您的檢視中要處理的 JavaScript。 執行這項作業的方法有兩種。 將值插入最安全的方法是將值放在標記的資料屬性，並擷取在 JavaScript 中。 例如: 

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

這會產生下列 HTML

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

它執行時，會轉譯下列：

```none
<"123">
   <"123">
   ```

您也可以直接呼叫 JavaScript 編碼器：

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

這將瀏覽器中轉換，如下所示：

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> 不受信任的輸入，在 JavaScript 中建立 DOM 項目不串連。 您應該使用`createElement()`，並適當地例如指派屬性值`node.TextContent=`，或使用`element.SetAttribute()` / `element[attribute]=`否則您會面臨 DOM 式 XSS。

## <a name="accessing-encoders-in-code"></a>存取程式碼中的編碼器

HTML、 JavaScript 和 URL 編碼器可有兩種程式碼，您可以將之插入透過[相依性插入](xref:fundamentals/dependency-injection)或您可以使用包含在預設編碼器`System.Text.Encodings.Web`命名空間。 如果您使用的預設編碼器，則您套用至任何處理為安全的字元範圍才會生效-預設編碼器使用最安全的編碼規則可能。

若要使用的可設定的編碼器，透過您的建構函式應該採用的 DI *HtmlEncoder*， *JavaScriptEncoder*並*UrlEncoder*適當的參數。 例如，

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

## <a name="encoding-url-parameters"></a>編碼的 URL 參數

如果您想要建置具有不受信任的輸入，當作值使用的 URL 查詢字串`UrlEncoder`編碼值。 例如，套用至物件的

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

變數會包含編碼 encodedValue 之後`%22Quoted%20Value%20with%20spaces%20and%20%26%22`。 空格、 引號、 標點符號和其他不安全字元就是百分比編碼成其十六進位值，例如空格字元會變成 %20。

>[!WARNING]
> 請勿使用不受信任的輸入作為 URL 路徑的一部分。 一律傳遞不受信任的輸入作為查詢字串值。

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>自訂編碼器

預設編碼器會使用基本拉丁文 Unicode 範圍僅限安全清單，並為其對等的字元碼超出該範圍的所有字元都編碼。 此行為也會影響 Razor TagHelper 與 HtmlHelper 轉譯，因為它會使用編碼器，輸出字串。

背後的原因是要防止未知或未來的瀏覽器錯誤 （先前的瀏覽器錯誤有挫敗剖析以非英文字元的處理）。 如果您的網站會大量使用非拉丁字元，例如中文，斯拉夫文或其他人不可能您想要的行為。

您可以自訂編碼器安全清單，以包含在您的應用程式在啟動期間，適合範圍的 Unicode `ConfigureServices()`。

例如，使用預設組態，您可以使用 Razor HtmlHelper 就像這樣;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

當您檢視網頁的原始檔時您會看到轉譯，如下所示，與編碼; 的中文文字

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

若要擴大字元視為安全的編碼器中您會插入下列行插入`ConfigureServices()`方法中的`startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

此範例中，將擴展安全清單，以包含 Unicode 範圍 CjkUnifiedIdeographs。 現在就會變成轉譯的輸出

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

安全清單範圍指定為 Unicode 字碼圖表，不是語言。 [Unicode 標準](http://unicode.org/)有一份[程式碼圖表](http://www.unicode.org/charts/index.html)可用來尋找圖表，其中包含您的字元。 每個編碼器、 Html、 JavaScript 和 Url，必須個別設定。

> [!NOTE]
> 自訂安全清單只會影響透過 DI 取得資料來源的編碼器。 如果您直接存取透過編碼器`System.Text.Encodings.Web.*Encoder.Default`然後預設值、 基本拉丁文將用於只安全清單。

## <a name="where-should-encoding-take-place"></a>編碼的 take 應放置的地方？

所接受的一般作法是，編碼會發生在輸出時，並編碼的值應該永遠不會儲存在資料庫中。 在輸出的編碼方式，可讓您變更的資料，例如，使用從查詢字串值的 HTML。 它也可讓您輕鬆地搜尋您的資料，而不需要編碼之前搜尋的值，並可讓您充分任何的利用變更或對編碼器的 bug 修正。

## <a name="validation-as-an-xss-prevention-technique"></a>XSS 防護技術以及驗證

驗證可以是一個有用的工具，在限制 XSS 攻擊。 例如，數字的字串，包含只有字元 0-9 不會觸發 XSS 攻擊。 接受使用者輸入中的 HTML 時，驗證將會變得更複雜。 剖析 HTML 輸入相當困難，不可能的。 搭配去除內嵌的 HTML，剖析器的 markdown，是比較安全的選項，以接受豐富的輸入。 絕不要依賴在單獨的驗證。 一律將編碼不受信任的輸入，輸出中，無論何種驗證或處理已執行前。
