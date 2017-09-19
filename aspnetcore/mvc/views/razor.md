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
ms.openlocfilehash: fff2f98592473a9baf6a2d4e360fec3026b7210d
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2017
---
# <a name="razor-syntax-for-aspnet-core"></a>Razor 語法的 ASP.NET Core

由[Taylor Mullen](https://twitter.com/ntaylormullen)和[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-is-razor"></a>Razor 是什麼？

Razor 是伺服器為基礎的程式碼內嵌在網頁標記語法。 Razor 語法包含 Razor 標記，C# 和 HTML。 通常包含 Razor 檔案有*.cshtml*檔案副檔名。

## <a name="rendering-html"></a>將 HTML 呈現

預設 Razor 語言為 HTML。 將 HTML 呈現從 Razor 並無不同 HTML 檔案中。 Razor 檔案中以下列標記：

```html
<p>Hello World</p>
   ```

會轉譯為未變更`<p>Hello World</p>`伺服器。

## <a name="razor-syntax"></a>Razor 語法

Razor 支援 C#，並使用`@`轉換來自 HTML，以 C# 中的符號。 Razor 評估 C# 運算式，並將它們呈現的 HTML 輸出中。 Razor 可以從 HTML 轉換成 C# 或 Razor 特定標記。 當`@`符號後面[Razor 保留關鍵字](#razor-reserved-keywords)會轉換成特定的 Razor 標記，否則它會轉換成一般的 C#。

<a name=escape-at-label></a>

包含 HTML`@`符號可能需要逸出的第二個`@`符號。 例如: 

```html
<p>@@Username</p>
   ```

會轉譯下列 HTML:

```html
<p>@Username</p>
   ```

<a name=razor-email-ref></a>

HTML 屬性和內容包含電子郵件地址不會將`@`符號為轉換字元。

   `<a href="mailto:Support@contoso.com">Support@contoso.com</a>`

## <a name="implicit-razor-expressions"></a>Razor 的隱含運算式

Razor 的隱含運算式開頭`@`後面接著 C# 程式碼。 例如: 

```html
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

除了 C#`await`關鍵字的隱含運算式不能包含空格。 比方說，您也可以只要 C# 陳述式已清除結束 intermingle 空格：

```html
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a>Razor 的明確運算式

Razor 的明確運算式包含 @ 有對稱的括號的符號。 例如，若要顯示上週的時間：

```html
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

內的任何內容 @ （） 括號會評估並呈現至輸出。

通常隱含運算式不可包含空格。 例如，下列程式碼，一週會不減去目前的時間：

[!code-html[Main](razor/sample/Views/Home/Contact.cshtml?range=20)]

這會轉譯下列 HTML:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
   ```

您可以使用明確的運算式來串連文字與運算式結果：

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [5]}} -->

```none
@{
    var joe = new Person("Joe", 33);
 }

<p>Age@(joe.Age)</p>
```

沒有明確的運算式，`<p>Age@joe.Age</p>`會被視為電子郵件地址和`<p>Age@joe.Age</p>`都會呈現。 做為明確的運算式，寫入時`<p>Age33</p>`轉譯。

<a name=expression-encoding-label></a>

## <a name="expression-encoding"></a>運算式的編碼方式

C# 運算式，評估為字串會以 HTML 編碼。 C# 運算式會評估得出`IHtmlContent`轉譯直接透過*IHtmlContent.WriteTo*。 C# 運算式未評估成*IHtmlContent*轉換成字串 (由*ToString*)，而且編碼在呈現之前。 例如，下列的 Razor 標記：

```html
@("<span>Hello World</span>")
   ```

將 HTML 呈現此：

```html
&lt;span&gt;Hello World&lt;/span&gt;
   ```

其瀏覽器會呈現為：

`<span>Hello World</span>`

`HtmlHelper``Raw`輸出不會編碼，但不會轉譯為 HTML 標記。

>[!WARNING]
> 使用`HtmlHelper.Raw`unsanitized 使用者輸入會造成安全性風險。 使用者輸入可能包含惡意的 JavaScript 或其他入侵。 免於使用者輸入很難，避免使用`HtmlHelper.Raw`使用者輸入。

下列的 Razor 標記：

```html
@Html.Raw("<span>Hello World</span>")
   ```

將 HTML 呈現此：

```html
<span>Hello World</span>
   ```

<a name=razor-code-blocks-label></a>

## <a name="razor-code-blocks"></a>Razor 程式碼區塊

Razor 程式碼區塊的開頭`@`和由`{}`。 不同於運算式中，C# 程式碼區塊內的程式碼不會轉譯。 程式碼區塊和 Razor 頁面中的運算式共用相同的範圍，以及定義順序 （也就是在程式碼區塊中的宣告會有的更新版本的程式碼區塊和運算式的範圍內）。

```none
@{
    var output = "Hello World";
}

<p>The rendered result: @output</p>
```

會轉譯：

```html
<p>The rendered result: Hello World</p>
   ```

<a name=implicit-transitions-label></a>

### <a name="implicit-transitions"></a>隱含轉換

程式碼區塊中的預設語言是 C# 中，但您可以轉換回 HTML。 HTML 程式碼區塊內就會轉換成 HTML 呈現：

```none
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

<a name=explicit-delimited-transition-label></a>

### <a name="explicit-delimited-transition"></a>明確分隔的轉換

若要定義應該呈現的 HTML 程式碼區塊的子區段，括住的字元來呈現使用 Razor`<text>`標記：

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

當您想要將不會圍繞著之 HTML 標記的 HTML 轉譯時，通常會使用此方法。 沒有 HTML 或 Razor 的標籤，您可以取得 Razor 執行階段錯誤。

<a name=explicit-line-transition-with-label></a>

### <a name="explicit-line-transition-with-"></a>使用明確列轉換`@:`

若要轉譯為 HTML 的整行的其餘部分的程式碼區塊內，使用`@:`語法：

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

不含`@:`上述程式碼，您會取得 Razor，執行階段錯誤。

<a name=control-structures-razor-label></a>

## <a name="control-structures"></a>控制結構

控制結構是一種擴充的程式碼區塊。 所有層面的程式碼區塊 （轉換到標記中，內嵌 C#） 也適都用於下列的結構。

### <a name="conditionals-if-else-if-else-and-switch"></a>條件式`@if`， `else if`，`else`和`@switch`

`@if`系列可讓您控制執行程式碼：

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
```

`else`和`else if`不需要`@`符號：

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

您可以使用 switch 陳述式如下：

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

### <a name="looping-for-foreach-while-and-do-while"></a>迴圈`@for`， `@foreach`， `@while`，和`@do while`

您可以將迴圈控制陳述式的樣板化 HTML 轉譯。 例如，若要呈現的人員清單：

```none
@{
    var people = new Person[]
    {
          new Person("John", 33),
          new Person("Doe", 41),
    };
}
```

您可以使用任何下列的迴圈陳述式：

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

### <a name="compound-using"></a>複合`@using`

在 C# using 陳述式用來確保處置物件。 Razor 中相同的機制可用來建立包含其他內容的 HTML helper。 比方說，我們可以利用 HTML Helper 呈現表單標記與`@using`陳述式：

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

您也可以執行範圍層級的動作，如上所示使用[標記協助程式](tag-helpers/index.md)。

### <a name="try-catch-finally"></a>`@try`, `catch`, `finally`

例外狀況處理會類似於 C#:

[!code-html[Main](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

Razor 具有保護與 lock 陳述式的關鍵區段的功能：

```none
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>註解

Razor 支援 C# 和 HTML 註解。 下列標記：

```none
@{
    /* C# comment. */
    // Another C# comment.
}
<!-- HTML comment -->
```

轉譯伺服器：

```none
<!-- HTML comment -->
```

呈現頁面之前，伺服器會移除 razor 註解。 使用 razor`@*  *@`來分隔註解。 下列程式碼會標記為註解，所以伺服器將不會呈現任何標記：

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

## <a name="directives"></a>指示詞

Razor 指示詞都由下列保留的關鍵字的隱含運算式`@`符號。 指示詞通常會變更網頁剖析的方式，或是啟用 Razor 頁面內的不同功能。

了解如何 Razor 產生檢視的程式碼將更易於了解指示詞的運作方式。 Razor 頁面用來產生 C# 檔案。 例如，此 Razor 頁面：

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

產生的類別如下所示：

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

[檢視檢視產生的 Razor C# 類別](#razor-customcompilationservice-label)說明如何檢視這個產生的類別。

### `@using`

`@using`指示詞會將 c#`using`產生的 razor 頁面指示詞：

[!code-html[Main](razor/sample/Views/Home/Contact9.cshtml)]

### `@model`

`@model`指示詞會指定傳遞至 Razor 頁面的模型型別。 其使用下列語法：

```none
@model TypeNameOfModel
   ```

例如，如果您建立的 ASP.NET Core MVC 應用程式與個別使用者帳戶*Views/Account/Login.cshtml* Razor 檢視包含下列模型宣告：

```csharp
@model LoginViewModel
   ```

在上述的類別範例中，產生的類別繼承自`RazorPage<dynamic>`。 藉由新增`@model`您控制所繼承。 例如

```csharp
@model LoginViewModel
   ```

會產生下列類別

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
   ```

Razor 頁面公開`Model`屬性，以存取模型傳遞至網頁。

```html
<div>The Login Email: @Model.Email</div>
   ```

`@model`指示詞會指定此屬性的型別 (藉由指定`T`中`RazorPage<T>`對您的頁面產生的類別衍生自)。 如果您未指定`@model`指示詞`Model`屬性都屬於類型`dynamic`。 模型值會從控制器到檢視。 請參閱[強類型模型和@model關鍵字](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label)如需詳細資訊。

### `@inherits`

`@inherits`指示詞讓您完整控制 Razor 頁面所繼承的類別：

```none
@inherits TypeNameOfClassToInheritFrom
   ```

比方說，假設我們有下列自訂 Razor 頁面類型：

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

會產生下列 Razor `<div>Custom text: Hello World</div>`。

[!code-html[Main](razor/sample/Views/Home/Contact10.cshtml)]

您無法使用`@model`和`@inherits`相同頁面上。 您可以擁有`@inherits`中*_ViewImports.cshtml* Razor 網頁匯入的檔案。 例如，如果您的 Razor 檢視匯入下列*_ViewImports.cshtml*檔案：

[!code-html[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

下列的強型別的 Razor 頁面

[!code-html[Main](razor/sample/Views/Home/Login1.cshtml)]

會產生這個 HTML 標記：

```none
<div>The Login Email: Rick@contoso.com</div>
<div>Custom text: Hello World</div>
```

當傳遞"[Rick@contoso.com](mailto:Rick@contoso.com)"在模型中：

   如需詳細資訊，請參閱 [Layout](layout.md)。

### `@inject`

`@inject`指示詞可讓您將從服務程式[服務容器](../../fundamentals/dependency-injection.md)插入 Razor 網頁使用。 請參閱[檢視的相依性插入](dependency-injection.md)。

<a name="functions"></a>

### `@functions`

`@functions`指示詞可讓您將函式層級內容加入至您的 Razor 頁面。 其語法為：

```none
@functions { // C# Code }
   ```

例如: 

[!code-html[Main](razor/sample/Views/Home/Contact6.cshtml)]

會產生下列的 HTML 標記：

```none
<div>From method: Hello</div>
   ```

產生 Razor 的 C# 看起來像：

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### `@section`

`@section`指示詞用於搭配[版面配置頁](layout.md)啟用檢視，以呈現在呈現的 HTML 頁面的不同部分的內容。 請參閱[區段](layout.md#layout-sections-label)如需詳細資訊。

## <a name="tag-helpers"></a>標記協助程式

下列[標記協助程式](tag-helpers/index.md)指示詞的詳細說明所提供的連結。

* [@addTagHelper](tag-helpers/intro.md#add-helper-label)
* [@removeTagHelper](tag-helpers/intro.md#remove-razor-directives-label)
* [@tagHelperPrefix](tag-helpers/intro.md#prefix-razor-directives-label)

<a name=razor-reserved-keywords-label></a>

## <a name="razor-reserved-keywords"></a>Razor 保留關鍵字

### <a name="razor-keywords"></a>Razor 關鍵字

* 頁面 （需要 ASP.NET Core 2.0 和更新版本）
* 函式
* 繼承
* model
* section
* 協助程式 （不支援 ASP.NET Core。）

Razor 關鍵字可以以逸出`@(Razor Keyword)`，例如`@(functions)`。 請參閱完整的範例。

### <a name="c-razor-keywords"></a>C# Razor 關鍵字

* case
* do
* default
* for
* foreach
* if
* else
* lock
* switch
* try
* catch
* finally
* 使用
* while

C# Razor 關鍵字必須為 double 以逸出`@(@C# Razor Keyword)`，例如`@(@case)`。 第一個`@`逸出 Razor 剖析器，第二個`@`逸出的 C# 剖析器。 請參閱完整的範例。

### <a name="reserved-keywords-not-used-by-razor"></a>不使用 Razor 的保留的關鍵字

* namespace
* Class - 類別

<a name=razor-customcompilationservice-label></a>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>檢視檢視產生的 Razor C# 類別

將下列類別加入至您的 ASP.NET Core MVC 專案：

[!code-csharp[Main](razor/sample/Services/CustomCompilationService.cs)]

覆寫`ICompilationService`MVC 加上述類別; 事件類別

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=29-33)]

在 設定中斷點`Compile`方法`CustomCompilationService`和檢視`compilationContent`。

![CompilationContent 文字視覺化檢視](razor/_static/tvr.png)

<a name="case"></a>
## <a name="view-lookups-and-case-sensitivity"></a>檢視對應及區分大小寫

Razor 檢視引擎會執行檢視的區分大小寫的查閱。 然而，實際的查詢會決定基礎來源：

* 根據的來源檔案： 

    * 在作業系統上使用不區分大小寫的檔案系統 （例如 Windows)，實體檔案提供者對應是不區分大小寫。 例如`return View("Test")`會導致`/Views/Home/Test.cshtml`，`/Views/home/test.cshtml`並會探索所有其他大小寫變體。
    * 在區分大小寫的檔案系統中，包括 Linux、 OSX 和`EmbeddedFileProvider`，查閱會區分大小寫。 例如，`return View("Test")`會特別尋找`/Views/Home/Test.cshtml`。
        
* 先行編譯的檢視：

   * ASP.Net Core 2.0 和更新版本中，查閱先行編譯的檢視是不區分大小寫在所有作業系統上。 行為是在 Windows 上的實體檔案提供者的行為相同。 
   注意： 如果兩個先行編譯的檢視只有大小寫不同，查詢的結果是不具決定性。

開發人員都要比對的區域中，控制器和動作名稱的大小寫的檔案和目錄名稱的大小寫。 這樣會確保您的部署仍然無從驗證的基礎檔案系統。
