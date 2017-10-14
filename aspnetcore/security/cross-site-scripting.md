---
title: "防止跨網站指令碼"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 95790927-2bfe-445e-b1fd-429c2c7030ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cross-site-scripting
ms.openlocfilehash: d0880fda4ee726bd30a48cce0907a3887f2a4545
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2017
---
# <a name="preventing-cross-site-scripting"></a>防止跨網站指令碼

<a name="security-cross-site-scripting"></a>

跨網站指令碼 (XSS) 是可讓攻擊者將用戶端指令碼 (通常是 JavaScript) 放入網頁的安全性弱點。 當其他使用者載入受影響的頁面，攻擊者指令碼執行時，讓攻擊者竊取 cookie 和工作階段權杖變更透過 DOM 操作的 web 網頁內容，或瀏覽器重新導向至另一個頁面。 應用程式會接受使用者輸入，並將它在網頁輸出而不需要驗證、 編碼或逸出它時，通常會發生 XSS 弱點。

## <a name="protecting-your-application-against-xss"></a>保護您的應用程式對 XSS

在基本層級 XSS 運作方式是欺騙您的應用程式到插入`<script>`標記成轉譯的頁面上，或是藉由插入`On*`事件的項目。 若要避免引進 XSS 到他們的應用程式，開發人員應該使用下列預防步驟。

1. 除非您是遵循下列步驟的其餘部分，永遠不會放入您的 HTML 輸入未受信任的資料。 不受信任的資料是任何資料可能受攻擊者、 HTML 表單的輸入、 查詢字串、 HTTP 標頭，即使資料源自攻擊者可能會違反您的資料庫，即使它們不能違反您的應用程式資料庫。

2. HTML 項目內有不受信任的資料之前，請先確定它是 HTML 編碼。 例如 HTML 編碼方式會採用字元&lt;變成安全的表單和&amp;lt;

3. 請先將不受信任的資料放入 HTML 屬性確認它已編碼的 HTML 屬性。 HTML 屬性編碼為 HTML 編碼的超集，例如將額外的字元編碼成"和 '。

4. 之前將不受信任的資料放入 JavaScript 將資料放在 HTML 項目您在執行階段擷取其內容。 如果這不可能，請確定資料是 JavaScript 編碼。 JavaScript 編碼危險的字元所需的 JavaScript，並將它們取代為其十六進位，例如&lt;會編碼為`\u003C`。

5. 之前將不受信任的資料放入 URL 查詢字串，請確定它是 URL 編碼。

## <a name="html-encoding-using-razor"></a>使用 Razor 的 HTML 編碼方式

自動使用 MVC Razor 引擎會將編碼所有輸出來自於變數，除非您真的硬碟以防止它執行這項作業。 它會使用 HTML 屬性編碼規則，每當您使用 *@* 指示詞。 為 HTML 屬性編碼為 HTML 編碼這表示您不必擔心自己是否應使用 HTML 編碼或 HTML 屬性編碼的超集。 您必須確定您只使用在 HTML 內容中，不會在嘗試直接插入 JavaScript 不受信任的輸入。 標記協助程式也會編碼的輸入您在標記參數中使用。

採取下列 Razor 檢視;

```none
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
> ASP.NET Core MVC 提供`HtmlString`類別可在輸出時不會自動編碼。 這應該永遠不會用於搭配信任的輸入這將會公開 （expose） XSS 的安全性弱點。

## <a name="javascript-encoding-using-razor"></a>使用 Razor Javascript 編碼

有時候可能想要將值插入您的檢視中要處理的 JavaScript。 執行這項作業的方法有兩種。 將值放在資料屬性中，並擷取在 JavaScript 程式是標記的最安全的方式，可將簡單的值。 例如: 

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

其中，執行時，會轉譯下列工作;

```none
<"123">
   <"123">
   ```

您也可以直接呼叫 JavaScript 編碼器

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

這將瀏覽器中轉換，如下所示。

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> 請勿串連在 JavaScript 中建立 DOM 項目，不受信任的輸入。 您應該使用`createElement()`適當例如指派屬性值和`node.TextContent=`，或使用`element.SetAttribute()` / `element[attribute]=`否則 exception-declaration DOM 式 XSS。

## <a name="accessing-encoders-in-code"></a>存取程式碼中的編碼器

HTML、 JavaScript 和 URL 編碼器可以使用您的程式碼有兩種，您可以將它們透過插入[相依性插入](../fundamentals/dependency-injection.md#fundamentals-dependency-injection)或者您可以使用中所包含的預設編碼器`System.Text.Encodings.Web`命名空間。 如果您使用的預設編碼器，然後在套用至字元範圍的任何處理為安全不會生效-預設編碼器使用最安全的編碼規則可能。

若要使用的可設定的編碼器，透過 DI 您建構函式應該採用*HtmlEncoder*， *JavaScriptEncoder*和*UrlEncoder*適當的參數。 例如，

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

如果您想要建立具有不受信任的輸入，當作值使用的 URL 查詢字串`UrlEncoder`来編碼的值。 例如：

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

變數將包含編碼 encodedValue 之後`%22Quoted%20Value%20with%20spaces%20and%20%26%22`。 空格、 引號、 標點符號和其他 unsafe 字元將百分比編碼成其十六進位值，例如空格字元會變成 %20。

>[!WARNING]
> 請勿使用不受信任的輸入的 URL 路徑的一部分。 一律為查詢字串值傳遞未受信任的輸入。

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>自訂編碼器

根據預設編碼器使用安全清單限制為基本的拉丁 Unicode 範圍，而且其字元程式碼的對等用法為超出該範圍的所有字元都編碼。 此行為也會影響 Razor TagHelper 和 HtmlHelper 轉譯，因為它會使用編碼器來輸出字串。

這背後的原因是防範未知或未來的瀏覽器 （上一個瀏覽器 bug 有向上處理非英文字元為基礎的剖析錯誤） 的 bug。 如果您的網站可讓大量使用非拉丁字元，例如，中文文 （斯拉夫） 或其他人這不可能是您想要的行為。

您可以自訂編碼器安全清單，以包含範圍中適合您的應用程式在啟動期間，Unicode `ConfigureServices()`。

例如，使用預設組態，您可能會使用 Razor HtmlHelper 如下所示。

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

當您檢視網頁的來源時您會看到它已呈現，如下所示，以中文文字編碼。

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

若要擴大範圍的字元視為安全編碼器插入將下列行插入`ConfigureServices()`方法中的`startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

這個範例可擴展包含 Unicode 範圍 CjkUnifiedIdeographs 安全的清單。 現在就會變成轉譯的輸出

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

安全清單的範圍已指定為 Unicode 字碼表不語言。 [Unicode 標準](http://unicode.org/)有一份[程式碼圖表](http://www.unicode.org/charts/index.html)可用來尋找圖表，其中包含您的字元。 每個編碼器 Html、 JavaScript 和 Url，必須個別設定。

> [!NOTE]
> 自訂安全清單只會影響來源 DI 透過編碼器。 如果您直接存取透過編碼器`System.Text.Encodings.Web.*Encoder.Default`然後預設值、 基本拉丁文只安全清單將會使用。

## <a name="where-should-encoding-take-place"></a>編碼的 take 應放置的位置？

一般會接受作法是編碼發生在輸出，而編碼的值應該永遠不會儲存在資料庫中。 在輸出的編碼方式，可讓您變更使用的資料，例如，從查詢字串值的 HTML。 它也可讓您輕鬆地搜尋您的資料，而不需要將值編碼搜尋之前，並可讓您充分任何的利用變更或編碼器所作的 bug 修正。

## <a name="validation-as-an-xss-prevention-technique"></a>做為 XSS 防護技巧的驗證

驗證是相當有用的工具，在限制 XSS 攻擊。 例如，包含只有字元 0-9 的簡單數值字串不會觸發 XSS 攻擊。 驗證變得更複雜的是如果您想要接受使用者輸入層中的 HTML 剖析 HTML 輸入是很困難，不可能。 MarkDown 和其他的文字格式將豐富的輸入更安全的選項。 您永遠不應該依賴單獨的驗證。 不受信任的輸入，輸出前先編碼，無論何種驗證您已經執行。
