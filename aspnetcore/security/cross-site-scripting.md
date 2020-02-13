---
title: 防止 ASP.NET Core 中的跨網站腳本（XSS）
author: rick-anderson
description: 深入瞭解跨網站腳本（XSS）和技術，以在 ASP.NET Core 應用程式中解決此弱點。
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: 1d6f605dc336d8768b8a47e4995f119d198a61af
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172639"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a>防止 ASP.NET Core 中的跨網站腳本（XSS）

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

跨網站腳本（XSS）是一種安全性弱點，可讓攻擊者將用戶端腳本（通常是 JavaScript）放入網頁中。 當其他使用者載入受影響的頁面時，會執行攻擊者的腳本，讓攻擊者竊取 cookie 和會話權杖，透過 DOM 操作變更網頁的內容，或將瀏覽器重新導向至另一個頁面。 當應用程式接受使用者輸入並將其輸出至頁面，而未進行驗證、編碼或將它加以轉義時，通常會發生 XSS 弱點。

## <a name="protecting-your-application-against-xss"></a>保護您的應用程式免于 XSS

在基本層級 XSS 的運作方式是將您的應用程式，藉由將 `<script>` 標記插入轉譯的頁面中，或將 `On*` 事件插入元素中。 開發人員應該使用下列預防步驟來避免將 XSS 引入其應用程式。

1. 絕對不要將不受信任的資料放入您的 HTML 輸入，除非您遵循下列其餘步驟。 不受信任的資料是任何可能受到攻擊者控制的資料、HTML 表單輸入、查詢字串、HTTP 標頭，甚至是以攻擊者的形式來自資料庫的資料，即使無法違反您的應用程式，也可能會破壞您的資料庫。

2. 將不受信任的資料放在 HTML 專案中之前，請確定它是以 HTML 編碼。 HTML 編碼會採用 &lt; 之類的字元，並將它們變更為安全形式，例如 &amp;lt;

3. 將不受信任的資料放入 HTML 屬性之前，請確定它是以 HTML 編碼。 HTML 屬性編碼是 HTML 編碼的超集合，並會將其他字元編碼，例如 "and"。

4. 將不受信任的資料放入 JavaScript 之前，請將資料放入您在執行時間中取得其內容的 HTML 專案。 如果無法這麼做，請確定資料已進行 JavaScript 編碼。 JavaScript 編碼會針對 JavaScript 採取危險的字元，並以其十六進位加以取代，例如 &lt; 會編碼為 `\u003C`。

5. 將不受信任的資料放入 URL 查詢字串之前，請確定其 URL 已編碼。

## <a name="html-encoding-using-razor"></a>使用 Razor 的 HTML 編碼

在 MVC 中使用的 Razor 引擎會自動將來引數的所有輸出編碼，除非您真的難以避免它執行此作業。 當您使用 *@* 指示詞時，它會使用 HTML 屬性編碼規則。 HTML 屬性編碼是 HTML 編碼的超集合，這表示您不需要擔心您是否應該使用 HTML 編碼或 HTML 屬性編碼。 您必須確定只在 HTML 內容中使用 @，而不是在嘗試將不受信任的輸入直接插入 JavaScript 中。 標記協助程式也會將您在標記參數中使用的輸入編碼。

採取下列 Razor 視圖：

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

此視圖會輸出*untrustedInput*變數的內容。 此變數包含一些在 XSS 攻擊中使用的字元，也就是 &lt;、"和 &gt;。 檢查來源會顯示編碼為的轉譯輸出：

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET Core MVC 提供的 `HtmlString` 類別不會在輸出時自動編碼。 這不應該與不受信任的輸入結合使用，因為這會公開 XSS 弱點。

## <a name="javascript-encoding-using-razor"></a>使用 Razor 的 JavaScript 編碼

有時候，您可能會想要將值插入 JavaScript 中，以便在您的視圖中處理。 有兩種方式可以達成這個目的， 插入值最安全的方式是將值放在標記的資料屬性中，然後在您的 JavaScript 中加以取出。 例如，

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

當它執行時，會轉譯下列內容：

```
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

這會在瀏覽器中呈現，如下所示：

```html
<script>
    document.write("\u003C\u0022123\u0022\u003E");
</script>
```

>[!WARNING]
> 請勿在 JavaScript 中串連不受信任的輸入，以建立 DOM 元素。 您應該使用 `createElement()` 並適當地指派屬性值（例如 `node.TextContent=`），或使用 `element.SetAttribute()`/`element[attribute]=` 否則會向您公開至以 DOM 為基礎的 XSS。

## <a name="accessing-encoders-in-code"></a>在程式碼中存取編碼器

HTML、JavaScript 和 URL 編碼器可透過兩種方式提供給您的程式碼，您可以透過相依性[插入](xref:fundamentals/dependency-injection)來插入它們，也可以使用 `System.Text.Encodings.Web` 命名空間中包含的預設編碼器。 如果您使用預設編碼器，則套用至字元範圍的任何都將不會生效-預設編碼器會使用最安全的編碼規則。

若要透過 DI 使用可設定的編碼器，您的函式應該適當地採用*HtmlEncoder*、 *JavaScriptEncoder*和*UrlEncoder*參數。 例如：

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

## <a name="encoding-url-parameters"></a>編碼 URL 參數

如果您想要以不受信任的輸入建立 URL 查詢字串做為值，請使用 `UrlEncoder` 來編碼值。 例如：

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

編碼之後，Url-encodedvalue 變數會包含 `%22Quoted%20Value%20with%20spaces%20and%20%26%22`。 空格、引號、標點符號和其他不安全的字元會以百分比編碼為其十六進位值，例如空白字元將會變成 %20。

>[!WARNING]
> 請勿使用不受信任的輸入做為 URL 路徑的一部分。 一律傳遞不受信任的輸入做為查詢字串值。

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>自訂編碼器

根據預設，編碼器會使用限制為基本拉丁 Unicode 範圍的安全清單，並將該範圍以外的所有字元編碼為其對等的字元碼。 這種行為也會影響 Razor TagHelper 和 HtmlHelper 轉譯，因為它會使用編碼器來輸出您的字串。

其背後的原因是要防範未知或未來的瀏覽器錯誤（先前的瀏覽器 bug 已根據非英文字元的處理來剖析）。 如果您的網站大量使用非拉丁字元，例如中文、斯拉夫或其他，這可能不是您想要的行為。

在 `ConfigureServices()`中，您可以自訂編碼器安全清單，以包含您的應用程式在啟動期間適用的 Unicode 範圍。

例如，使用預設設定時，您可能會使用 Razor HtmlHelper，如下所示;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

當您查看網頁的來源時，您會看到其呈現方式如下：以中文文字編碼;

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

若要加寬編碼器視為安全的字元，您可以在 `startup.cs`的 `ConfigureServices()` 方法中插入下面這一行;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

這個範例會擴大安全清單，以包含 Unicode 範圍 CjkUnifiedIdeographs。 呈現的輸出現在會變成

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

安全清單範圍會指定為 Unicode 程式碼圖表，而不是語言。 [Unicode 標準](https://unicode.org/)有一份程式[代碼圖表](https://www.unicode.org/charts/index.html)清單，您可以用來尋找包含您字元的圖表。 每個編碼器、Html、JavaScript 和 Url 都必須分別設定。

> [!NOTE]
> 安全清單的自訂只會影響透過 DI 來源的編碼器。 如果您透過 `System.Text.Encodings.Web.*Encoder.Default` 直接存取編碼器，則會使用預設的 [僅限基本的拉丁] 安全功能。

## <a name="where-should-encoding-take-place"></a>應該在哪裡進行編碼？

一般接受的作法是，編碼會在輸出點進行，而且編碼的值永遠不會儲存在資料庫中。 在輸出點進行編碼可讓您將資料的使用方式（例如，從 HTML 變更為查詢字串值）。 它也可讓您輕鬆地搜尋資料，而不需要在搜尋之前編碼值，並可讓您利用對編碼器所做的任何變更或錯誤修正。

## <a name="validation-as-an-xss-prevention-technique"></a>驗證為 XSS 防護技術

驗證可能是限制 XSS 攻擊的有用工具。 例如，只包含0-9 個字元的數值字串不會觸發 XSS 攻擊。 接受使用者輸入中的 HTML 時，驗證會變得更複雜。 如果不可能，剖析 HTML 輸入很難。 Markdown 結合了內嵌 HTML 的剖析器，是接受豐富輸入的更安全選項。 切勿單獨依賴驗證。 無論執行何種驗證或清理，一律會在輸出前編碼不受信任的輸入。
