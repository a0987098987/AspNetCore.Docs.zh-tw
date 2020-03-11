---
title: ASP.NET Core 中的 Razor Pages 簡介
author: Rick-Anderson
description: 了解 ASP.NET Core 中的 Razor Pages 如何使注重頁面的案例編碼變得更輕鬆，並增加生產力，達到比使用 MVC 更好的成效。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 02/12/2020
uid: razor-pages/index
ms.openlocfilehash: 42ffb0d4d2e49663dd53ffeee5d9fa2a931ee5b7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667577"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a>ASP.NET Core 中的 Razor Pages 簡介

::: moniker range=">= aspnetcore-3.0"

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Ryan Nowak](https://github.com/rynowak)

Razor Pages 可以讓撰寫以頁面為焦點的案例更輕鬆且更具生產力，而不是使用控制器和視圖。

如果您在尋找使用模型檢視控制器方法的教學課程，請參閱[開始使用 ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)。

本文件提供 Razor Pages 簡介。 它不是逐步教學課程。 如果您覺得某些章節過於困難，可以參閱[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)。 如需 ASP.NET Core 的概觀，請參閱[ASP.NET Core 簡介](xref:index)。

## <a name="prerequisites"></a>Prerequisites

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a>建立 Razor Pages 專案

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

如需如何建立 Razor Pages 專案的詳細說明，請參閱[開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

從命令列執行 `dotnet new webapp`。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

從命令列執行 `dotnet new webapp`。

從 Visual Studio for Mac 開啟已產生的 *.csproj* 檔案。

---

## <a name="razor-pages"></a>Razor Pages

Razor Pages 是在 *Startup.cs* 中啟用：

[!code-cs[](index/3.0sample/RazorPagesIntro/Startup.cs?name=snippet_Startup&highlight=12,36)]

請考慮使用基本頁面：<a name="OnGet"></a>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index.cshtml?highlight=1)]

上述程式碼看起來很像用於 ASP.NET Core 應用程式的 [Razor 檢視檔案](xref:tutorials/first-mvc-app/adding-view)，含有控制器和檢視。 這會讓[`@page`](xref:mvc/views/razor#page)指示詞不同。 `@page` 會將檔案轉換成 MVC 動作，這表示它會直接處理要求，不用透過控制器。 `@page` 必須是頁面上的第一個 Razor 指示詞。 `@page` 會影響其他[Razor](xref:mvc/views/razor)結構的行為。 Razor Pages 檔案名具有 *. cshtml*尾碼。

使用`PageModel`類別的類似頁面，顯示於下列兩個檔案中。 *Pages/Index2.cshtml* 檔案：

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml)]

*Pages/Index2.cshtml.cs* 頁面模型：

[!code-cs[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

依照慣例，`PageModel` 類別檔和附加 *.cs* 檔名的 Razor Page 檔案名稱相同。 例如，前一個 Razor Page 是 *Pages/Index2.cshtml*。 包含 `PageModel` 類別的檔案名為 *Pages/Index2.cshtml.cs*。

頁面的 URL 路徑關聯是由頁面在檔案系統中的位置決定。 下表顯示 Razor 頁面路徑和相符的 URL：

| 檔案名稱和路徑               | 比對 URL |
| ----------------- | ------------ |
| */Pages/Index.cshtml* | `/` 或 `/Index` |
| */Pages/Contact.cshtml* | `/Contact` |
| */Pages/Store/Contact.cshtml* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store` 或 `/Store/Index` |

注意：

* 執行階段預設會在 *Pages* 資料夾中尋找 Razor Pages 的檔案。
* `Index` 是 URL 未包含頁面時的預設頁面。

## <a name="write-a-basic-form"></a>撰寫基本表單

Razor Pages 設計用於製作一般的模式，可搭配網頁瀏覽器一起使用，在建置應用程式時能易於實作。 [模型繫結](xref:mvc/models/model-binding)、[標記協助程式](xref:mvc/views/tag-helpers/intro)和 HTML 協助程式搭配 Razor Page 類別中定義的屬性「就這麼簡單」。 `Contact` 模型請考慮實作基本的「與我們連絡」格式頁面：

本文件中的範例，會在 `DbContext`Startup.cs[ 檔案中初始化 ](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24)。

[!code-cs[](index/3.0sample/RazorPagesContacts/Startup.cs?name=snippet)]

資料模型：

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

DB 內容：

[!code-cs[](index/3.0sample/RazorPagesContacts/Data/CustomerDbContext.cs)]

*Pages/Create.cshtml* 檢視檔案：

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

*Pages/Create.cshtml.cs* 頁面模型：

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_ALL)]

依照慣例，`PageModel` 類別稱之為 `<PageName>Model`，與頁面位於相同的命名空間。

`PageModel` 類別可以分離頁面邏輯與頁面展示。 此類別會定義頁面的處理常式，以處理傳送至頁面的要求與用於轉譯頁面的資料。 這種分隔允許：

* 透過相依性[插入](xref:fundamentals/dependency-injection)來管理頁面相依性。
* [單元測試](xref:test/razor-pages-tests)

此頁面具有 `OnPostAsync`*處理常式方法*，它會在 `POST` 要求上執行（當使用者張貼表單時）。 可以新增任何 HTTP 指令動詞的處理常式方法。 最常見的處理常式包括：

* `OnGet`，初始化頁所需要的狀態。 在上述程式碼中，`OnGet` 方法會顯示*CreateModel* Razor 頁面。
* `OnPost`，處理表單提交作業。

`Async` 命名尾碼是選擇性的，但依照慣例通常用於非同步函式。 上述程式碼一般用於 Razor 頁面。

如果您熟悉如何使用控制器和 views 來 ASP.NET 應用程式：

* 上述範例中的 `OnPostAsync` 程式碼看起來類似一般的控制器程式碼。
* 大部分的 MVC 基本類型（例如[模型](xref:mvc/models/model-binding)系結、[驗證](xref:mvc/models/validation)和動作結果）在控制器和 Razor Pages 中的運作方式都相同。 

前一個 `OnPostAsync` 方法：

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync)]

`OnPostAsync` 的基本流程：

檢查驗證錯誤。

* 如果沒有任何錯誤，會儲存資料並重新導向。
* 如果有錯誤，會再次顯示有驗證訊息的頁面。 在許多情況下，用戶端會偵測到驗證錯誤，但從不提交給伺服器。

*Pages/Create.cshtml* 檢視檔案：

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

從*Pages/Create. cshtml*轉譯的 HTML：

[!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.html)]

在先前的程式碼中，張貼表單：

* 具有有效的資料：

  * `OnPostAsync` 處理常式方法會呼叫 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper 方法。 `RedirectToPage` 傳回 <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult> 的執行個體。 `RedirectToPage`:

    * 是動作結果。
    * 類似于 `RedirectToAction` 或 `RedirectToRoute` （用於控制器和 views）。
    * 已針對頁面自訂。 在上述範例中，它會重新導向至根索引頁面 (`/Index`)。 `RedirectToPage`產生頁面 URL[一節會詳細說明 ](#url_gen)。

* 傳遞至伺服器的驗證錯誤：

  * `OnPostAsync` 處理常式方法會呼叫 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper 方法。 `Page` 傳回 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> 的執行個體。 傳回 `Page` 類似於控制站中的動作傳回 `View`。 `PageResult` 是處理常式方法的預設傳回型別。 傳回 `void` 的處理常式方法會呈現頁面。
  * 在上述範例中，張貼不含值的表單會導致[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid)傳回 false。 在此範例中，用戶端上不會顯示任何驗證錯誤。 本檔稍後會涵蓋驗證錯誤處理。

  [!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

* 用戶端驗證偵測到驗證錯誤：

  * 資料**未**張貼至伺服器。
  * 本檔稍後會說明用戶端驗證。

`Customer` 屬性會使用[`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute)屬性來加入宣告模型系結：

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

`[BindProperty]`**不**應用於包含不應由用戶端變更之屬性的模型。 如需詳細資訊，請參閱[防止大量指派](xref:data/ef-rp/crud#overposting)。

根據預設，Razor Pages 只會建立屬性與非 `GET` 動詞之間的繫結。 系結至屬性不需要撰寫程式碼來將 HTTP 資料轉換成模型型別。 透過使用相同的屬性呈現表單欄位 (`<input asp-for="Customer.Name">`) 並接受輸入，繫結可以減少程式碼。

[!INCLUDE[](~/includes/bind-get.md)]

查看*Pages/Create. cshtml* view file：

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml?highlight=3,9)]

* 在上述程式碼中，[輸入](xref:mvc/views/working-with-forms#the-input-tag-helper)標籤協助程式 `<input asp-for="Customer.Name" />` 會將 HTML `<input>` 元素系結至 `Customer.Name` 模型運算式。
* [`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available)讓標籤協助程式可供使用。

### <a name="the-home-page"></a>首頁

*Index. cshtml*是首頁：

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml)]

已建立關聯的 `PageModel` 類別 (*Index.cshtml.cs*)：

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet)]

*Index. cshtml*檔案包含下列標記：

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=21)]

`<a /a>`[錨點](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)標籤協助程式使用 `asp-route-{value}` 屬性來產生 [編輯] 頁面的連結。 該連結包含路由資料和連絡人識別碼。 例如： `https://localhost:5001/Edit/1` 。 [標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。

*Index. cshtml*檔案包含標記，以建立每個客戶連絡人的刪除按鈕：

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=22-23)]

呈現的 HTML：

```html
<button type="submit" formaction="/Customers?id=1&amp;handler=delete">delete</button>
```

當 [刪除] 按鈕以 HTML 轉譯時，其[formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction)會包含的參數：

* 由 `asp-route-id` 屬性指定的客戶連絡人識別碼。
* 由 `asp-page-handler` 屬性指定的 `handler`。

選取按鈕時，表單 `POST` 要求會傳送至伺服器。 依照慣例，會依據配置 `handler`，按 `OnPost[handler]Async` 參數的值來選取處理常式方法。

在此範例中，因為 `handler` 為 `delete`，所以會使用 `OnPostDeleteAsync` 處理常式方法來處理 `POST` 要求。 若 `asp-page-handler` 設為其他值 (例如 `remove`)，則會選取名為 `OnPostRemoveAsync` 的處理常式方法。

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet2)]

`OnPostDeleteAsync` 方法：

* 從查詢字串取得 `id`。
* 使用 `FindAsync` 在資料庫中查詢客戶連絡人。
* 如果找到客戶連絡人，就會將其移除並更新資料庫。
* 呼叫 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> 以重新導向至根索引頁 (`/Index`)。

### <a name="the-editcshtml-file"></a>編輯 cshtml 檔案

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml?highlight=1)]

第一行包含 `@page "{id:int}"` 指示詞。 路由條件約束 `"{id:int}"` 通知頁面接受包含 `int` 路由資料的頁面要求。 如果頁面要求不包含可以轉換成 `int` 的路由資料，執行階段會傳回 HTTP 404 (找不到) 錯誤。 若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：

 ```cshtml
@page "{id:int?}"
```

*Edit.cshtml.cs*檔案：

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml.cs?name=snippet)]

## <a name="validation"></a>驗證

驗證規則：

* 在模型類別中是以宣告方式指定。
* 會在應用程式的任何位置強制執行。

<xref:System.ComponentModel.DataAnnotations> 命名空間提供一組內建的驗證屬性，會以宣告方式套用至類別或屬性。 DataAnnotations 也包含格式屬性（例如[`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) ），可協助進行格式設定，而且不提供任何驗證。

請考慮 `Customer` 模型：

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

使用下列*建立. cshtml*視圖檔案：

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=3,8-9,15-99)]

上述程式碼：

* 包含 jQuery 和 jQuery 驗證腳本。
* 使用 `<div />` 和 `<span />`[標記](xref:mvc/views/tag-helpers/intro)協助程式來啟用：

  * 用戶端驗證。
  * 驗證錯誤呈現。

* 產生下列 HTML：

  [!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]

張貼不含名稱值的建立表單時，會顯示錯誤訊息「需要名稱欄位」。 在表單上。 如果在用戶端上啟用 JavaScript，瀏覽器會顯示錯誤，而不會張貼至伺服器。

`[StringLength(10)]` 屬性會在呈現的 HTML 上產生 `data-val-length-max="10"`。 `data-val-length-max` 防止瀏覽器輸入超過指定的最大長度。 如果使用[Fiddler](https://www.telerik.com/fiddler)之類的工具來編輯和重新播放文章：

* ，其名稱超過10。
* 錯誤訊息「功能變數名稱必須是最大長度為10的字串」。 」錯誤訊息。

請考慮下列 `Movie` 模型：

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

驗證屬性會指定要在其套用的模型屬性上強制執行的行為：

* `Required` 和 `MinimumLength` 屬性工作表示屬性必須具有值，但不會防止使用者輸入空白字元以滿足這種驗證。
* `RegularExpression` 屬性則用來限制可輸入的字元。 在上述程式碼中，"Genre"：

  * 必須指使用字母。
  * 第一個字母必須是大寫。 不允許使用空格、數字和特殊字元。

* `RegularExpression` "Rating"：

  * 第一個字元必須為大寫字母。
  * 允許後續空格中的特殊字元和數位。 "PG-13" 對分級而言有效，但不適用於 "Genre"。

* `Range` 屬性會將值限制在指定的範圍內。
* `StringLength` 屬性會設定字串屬性的最大長度，並選擇性地設定其最小長度。
* 實值型別 (如`decimal`、`int`、`float`、`DateTime`) 原本就是必要項目，而且不需要 `[Required]` 屬性。

`Movie` 模型的 [建立] 頁面會顯示具有無效值的錯誤：

![有多個 jQuery 用戶端驗證錯誤的電影檢視表單](~/tutorials/razor-pages/validation/_static/val.png)

如需詳細資訊，請參閱

* [將驗證新增至電影應用程式](xref:tutorials/razor-pages/validation)
* [ASP.NET Core 中的模型驗證](xref:mvc/models/validation)。

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a>使用 OnGet 處理常式後援來處理 HEAD 要求

`HEAD` 要求可讓您抓取特定資源的標頭。 不同於 `GET` 要求，`HEAD` 要求不會傳回回應主體。

一般來說，會為 `OnHead` 要求建立及呼叫 `HEAD` 處理常式：

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Privacy.cshtml.cs?name=snippet)]

如果未定義任何 `OnHead` 處理常式，Razor Pages 會回到呼叫 `OnGet` 處理常式。

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a>XSRF/CSRF 和 Razor Pages

Razor Pages 受到[Antiforgery 驗證](xref:security/anti-request-forgery)的保護。 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)會將 antiforgery TOKEN 插入 HTML 表單元素中。

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>搭配 Razor Pages 使用版面配置、部分、範本和標記協助程式。

Pages 可搭配 Razor 檢視引擎的所有功能一起使用。 版面配置、部分、範本、標籤協助程式、 *_ViewStart*和 *_ViewImports。 cshtml*的工作方式與傳統 Razor 視圖相同。

可利用這些功能的一部分來整理這個頁面。

將[版面配置頁面](xref:mvc/views/layout)新增至 *Pages/Shared/_Layout.cshtml*：

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Shared/_Layout2.cshtml?hightlight=12)]

[配置](xref:mvc/views/layout)：

* 控制每個頁面的版面配置 (除非頁面退出版面配置)。
* 匯入 HTML 結構，例如 JavaScript 和樣式表。
* 在呼叫 `@RenderBody()` 時，會轉譯 Razor 頁面的內容。

如需詳細資訊，請參閱[版面配置頁面](xref:mvc/views/layout)。

[版面配置](xref:mvc/views/layout#specifying-a-layout)屬性是在 *Pages/_ViewStart.cshtml* 中設定：

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

版面配置位於 *Pages/Shared* 資料夾。 頁面會以階層方式尋找其他檢視 (版面配置、範本、部分)，從目前頁面的相同資料夾開始。 您可以從任何 Razor 頁面中的 *Pages* 資料夾下，使用 *Pages/Shared* 資料夾中的版面配置。

版面配置頁面應位於 *Pages/Shared* 資料夾中。

我們**不**建議您將配置檔案放入 *Views/Shared* 資料夾。 *Views/Shared* 是 MVC 檢視模式。 Razor 頁面應該要依賴資料夾階層，不是路徑慣例。

Razor 頁面的檢視搜尋包括 *Pages* 資料夾。 與 MVC 控制器和傳統 Razor views 搭配使用的版面配置、範本和部分都*是可行*的。

新增 *Pages/_ViewImports.cshtml* 檔案：

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

本教學課程稍後會說明 `@namespace`。 `@addTagHelper` 指示詞會將[內建標記協助程式](xref:mvc/views/tag-helpers/builtin-th/Index)帶入 *Pages* 資料夾中的所有頁面。

<a name="namespace"></a>

在頁面上設定的 `@namespace` 指示詞：

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

`@namespace` 指示詞會設定頁面的命名空間。 `@model` 指示詞不需要包含命名空間。

當 `@namespace` 指示詞包含在 *_ViewImports.cshtml* 中時，指定的命名空間會在匯入 `@namespace` 指示詞的頁面中提供所產生之命名空間的前置詞。 所產生命名空間的其餘部分 (後置字元部分) 是包含 *_ViewImports.cshtml* 的資料夾和包含頁面的資料夾之間，以句點分隔的相對路徑。

例如，`PageModel` 類別 *Pages/Customers/Edit.cshtml.cs* 會明確地設定命名空間：

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

*Pages/_ViewImports.cshtml* 檔案會設定下列命名空間：

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

為 *Pages/Customers/Edit.cshtml* Razor 頁面產生的命名空間和 `PageModel` 類別相同。

`@namespace`*也適用于傳統的 Razor views。*

請考慮*Pages/Create. cshtml* view file：

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]

已更新的*Pages/Create. cshtml*視圖檔案，包含 *_ViewImports. cshtml*和先前的配置檔案：

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.cshtml?highlight=2)]

在上述程式碼中， *_ViewImports. cshtml*已匯入命名空間和標籤協助程式。 設定檔案已匯入 JavaScript 檔案。

[Razor Pages 入門專案](#rpvs17)包含 *Pages/_ValidationScriptsPartial.cshtml*，連結用戶端驗證。

如需部分檢視的詳細資訊，請參閱 <xref:mvc/views/partial>。

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>產生頁面 URL

前面出現過的 `Create` 頁面使用 `RedirectToPage`：

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=28)]

應用程式有下列檔案/資料夾結構：

* */Pages*

  * *Index.cshtml*
  * *隱私權. cshtml*
  * */Customers*

    * *Create.cshtml*
    * *Edit.cshtml*
    * *Index.cshtml*

*Pages/customers/Create. cshtml*和*pages/customers/Edit. cshtml*頁面會在成功之後重新導向至*Pages/customers/Index. cshtml* 。 字串 `./Index` 是用來存取前一頁的相對頁面名稱。 它可用來產生*Pages/Customers/Index. cshtml*頁面的 url。 例如：

* `Url.Page("./Index", ...)`
* `<a asp-page="./Index">Customers Index Page</a>`
* `RedirectToPage("./Index")`

[絕對頁面名稱] `/Index` 用來產生 [ *Pages/Index. cshtml* ] 頁面的 url。 例如：

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">Home Index Page</a>`
* `RedirectToPage("/Index")`

頁面名稱是從根 */Pages* 資料夾到該頁面的路徑 (包括前置的 `/`，例如 `/Index`)。 先前的 URL 產生範例提供了增強的選項和功能，透過硬式編碼 URL。 URL 產生使用[路由](xref:mvc/controllers/routing)，可以根據路由在目的地路徑中定義的方式，產生並且編碼參數。

產生頁面 URL 支援相關的名稱。 下表顯示在*Pages/Customers/Create. cshtml*中，使用不同的 `RedirectToPage` 參數選取了哪個索引頁。

| RedirectToPage(x)| 頁面 |
| ----------------- | ------------ |
| RedirectToPage("/Index") | *Pages/Index* |
| RedirectToPage("./Index"); | *Pages/Customers/Index* |
| RedirectToPage("../Index") | *Pages/Index* |
| RedirectToPage("Index")  | *Pages/Customers/Index* |

<!-- Test via ~/razor-pages/index/3.0sample/RazorPagesContacts/Pages/Customers/Details.cshtml.cs -->

`RedirectToPage("Index")`、`RedirectToPage("./Index")`和 `RedirectToPage("../Index")` 是*相對名稱*。 `RedirectToPage` 參數「結合」了目前頁面的路徑，以計算目的地頁面的名稱。

相對名稱連結在以複雜結構建置網站時很有用。 當使用相對名稱來連結資料夾中的頁面時：

* 重新命名資料夾並不會中斷相對連結。
* 連結不會中斷，因為它們不包含資料夾名稱。

若要重新導向到不同[區域](xref:mvc/controllers/areas)中的頁面，請指定區域：

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

如需詳細資訊，請參閱 <xref:mvc/controllers/areas> 和 <xref:razor-pages/razor-pages-conventions>。

## <a name="viewdata-attribute"></a>ViewData 屬性

資料可以傳遞至具有 <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>的頁面。 具有 `[ViewData]` 屬性的屬性會將其值儲存並從 <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>載入。

在下列範例中，`AboutModel` 會將 `[ViewData]` 屬性套用至 `Title` 屬性：

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

在 [關於] 頁面上，存取 `Title` 屬性作為模型屬性：

```cshtml
<h1>@Model.Title</h1>
```

在此配置中，標題會從 ViewData 字典中讀取：

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a>TempData

ASP.NET Core 會公開 <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>。 這個屬性會儲存資料，直到讀取為止。 <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> 和 <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> 方法可以用來檢查資料，不用刪除。 當需要多個單一要求的資料時，`TempData` 對重新導向很有用。

下列程式碼會設定使用 `Message` 的 `TempData` 值：

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

*Pages/Customers/Index.cshtml* 檔案中的下列標記會顯示使用 `Message` 的 `TempData` 值。

```cshtml
<h3>Msg: @Model.Message</h3>
```

*Pages/Customers/Index.cshtml.cs* 頁面模型會將 `[TempData]` 屬性 (attribute) 套用到 `Message` 屬性 (property)。

```csharp
[TempData]
public string Message { get; set; }
```

如需詳細資訊，請參閱[TempData](xref:fundamentals/app-state#tempdata)。

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a>每頁面有多個處理常式

下列頁面會使用 `asp-page-handler` 標記協助程式為兩個處理常式產生標記：

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

上例中的表單有兩個提交按鈕，每一個都使用 `FormActionTagHelper` 提交至不同的 URL。 `asp-page-handler` 屬性附隨於 `asp-page`。 `asp-page-handler` 產生的 URL 會提交至頁面所定義的每一個處理常式方法。 因為範例連結至目前的頁面，所以未指定 `asp-page`。

頁面模型：

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

上述程式碼使用「具名的處理常式方法」。 具名的處理常式方法的建立方式是採用名稱中在 `On<HTTP Verb>` 後面、`Async` 之前 (如有) 的文字。 在上例中，頁面方法是 OnPost**JoinList**Async 和 OnPost**JoinListUC**Async。 移除 *OnPost* 和 *Async*，處理常式名稱就是 `JoinList` 和 `JoinListUC`。

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

使用上述程式碼，提交至 `OnPostJoinListAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH?handler=JoinList`。 提交至 `OnPostJoinListUCAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`。

## <a name="custom-routes"></a>自訂路由

使用 `@page` 指示詞，可以：

* 指定頁面的自訂路由。 例如，[關於] 頁面的路由可使用 `/Some/Other/Path` 設為 `@page "/Some/Other/Path"`。
* 將區段附加到頁面的預設路由。 例如，使用 `@page "item"` 可將 "item" 區段新增到頁面的預設路由。
* 將參數附加到頁面的預設路由。 例如，具有 `id` 的頁面可要求識別碼參數 `@page "{id}"`。

支援在路徑開頭以波狀符號 (`~`) 指定根相對路徑。 例如，`@page "~/Some/Other/Path"` 與 `@page "/Some/Other/Path"` 相同。

如果您不喜歡 URL 中的查詢字串 `?handler=JoinList`，請變更路由，將處理常式名稱放在 URL 的路徑部分。 藉由在 `@page` 指示詞後面加上雙引號括住的路由範本，可以自訂路由。

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

使用上述程式碼，提交至 `OnPostJoinListAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH/JoinList`。 提交至 `OnPostJoinListUCAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH/JoinListUC`。

跟在 `?` 後面的 `handler` 表示路由參數為選擇性。

## <a name="advanced-configuration-and-settings"></a>先進的設定和設定

大部分的應用程式都不需要下列章節中的設定和設定。

若要設定 advanced 選項，請使用擴充方法 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>：

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupRPoptions.cs?name=snippet)]

使用 [<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions>] 來設定頁面的根目錄，或加入頁面的應用程式模型慣例。 如需有關慣例的詳細資訊，請參閱[Razor Pages 授權慣例](xref:security/authorization/razor-pages-authorization)。

若要先行編譯視圖，請參閱[Razor view 編譯](xref:mvc/views/view-compilation)。

### <a name="specify-that-razor-pages-are-at-the-content-root"></a>指定 Razor Pages 位於內容根目錄

根據預設，Razor Pages 位於 */Pages* 根目錄。 新增 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> 以指定您的 Razor Pages 位於應用程式的[內容根目錄](xref:fundamentals/index#content-root)（<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>）：

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesAtContentRoot.cs?name=snippet)]

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a>指定 Razor Pages 位於自訂根目錄

新增 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> 以指定 Razor Pages 位於應用程式中的自訂根目錄（提供相對路徑）：

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesRoot.cs?name=snippet)]

## <a name="additional-resources"></a>其他資源

* 請參閱[開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)，這是以本簡介為基礎
* [下載或查看範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample)
* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>
* <xref:blazor/integrate-components>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Ryan Nowak](https://github.com/rynowak)

Razor Pages 是 ASP.NET Core MVC 新的部分，更容易編寫以頁面為焦點的案例程式碼，也更具生產力。

如果您在尋找使用模型檢視控制器方法的教學課程，請參閱[開始使用 ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)。

本文件提供 Razor Pages 簡介。 它不是逐步教學課程。 如果您覺得某些章節過於困難，可以參閱[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)。 如需 ASP.NET Core 的概觀，請參閱[ASP.NET Core 簡介](xref:index)。

## <a name="prerequisites"></a>Prerequisites

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a>建立 Razor Pages 專案

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

如需如何建立 Razor Pages 專案的詳細說明，請參閱[開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

從命令列執行 `dotnet new webapp`。

從 Visual Studio for Mac 開啟已產生的 *.csproj* 檔案。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

從命令列執行 `dotnet new webapp`。

---

## <a name="razor-pages"></a>Razor Pages

Razor Pages 是在 *Startup.cs* 中啟用：

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

請考慮使用基本頁面：<a name="OnGet"></a>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

上述程式碼看起來很像用於 ASP.NET Core 應用程式的 [Razor 檢視檔案](xref:tutorials/first-mvc-app/adding-view)，含有控制器和檢視。 讓它不同的是 `@page` 指示詞。 `@page` 會將檔案轉換成 MVC 動作，這表示它會直接處理要求，不用透過控制器。 `@page` 必須是頁面上的第一個 Razor 指示詞。 `@page` 會影響其他的 Razor 建構行為。

使用`PageModel`類別的類似頁面，顯示於下列兩個檔案中。 *Pages/Index2.cshtml* 檔案：

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

*Pages/Index2.cshtml.cs* 頁面模型：

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

依照慣例，`PageModel` 類別檔和附加 *.cs* 檔名的 Razor Page 檔案名稱相同。 例如，前一個 Razor Page 是 *Pages/Index2.cshtml*。 包含 `PageModel` 類別的檔案名為 *Pages/Index2.cshtml.cs*。

頁面的 URL 路徑關聯是由頁面在檔案系統中的位置決定。 下表顯示 Razor 頁面路徑和相符的 URL：

| 檔案名稱和路徑               | 比對 URL |
| ----------------- | ------------ |
| */Pages/Index.cshtml* | `/` 或 `/Index` |
| */Pages/Contact.cshtml* | `/Contact` |
| */Pages/Store/Contact.cshtml* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store` 或 `/Store/Index` |

注意：

* 執行階段預設會在 *Pages* 資料夾中尋找 Razor Pages 的檔案。
* `Index` 是 URL 未包含頁面時的預設頁面。

## <a name="write-a-basic-form"></a>撰寫基本表單

Razor Pages 設計用於製作一般的模式，可搭配網頁瀏覽器一起使用，在建置應用程式時能易於實作。 [模型繫結](xref:mvc/models/model-binding)、[標記協助程式](xref:mvc/views/tag-helpers/intro)和 HTML 協助程式搭配 Razor Page 類別中定義的屬性「就這麼簡單」。 `Contact` 模型請考慮實作基本的「與我們連絡」格式頁面：

本文件中的範例，會在 `DbContext`Startup.cs[ 檔案中初始化 ](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16)。

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

資料模型：

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

DB 內容：

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

*Pages/Create.cshtml* 檢視檔案：

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

*Pages/Create.cshtml.cs* 頁面模型：

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

依照慣例，`PageModel` 類別稱之為 `<PageName>Model`，與頁面位於相同的命名空間。

`PageModel` 類別可以分離頁面邏輯與頁面展示。 此類別會定義頁面的處理常式，以處理傳送至頁面的要求與用於轉譯頁面的資料。 這種分隔允許：

* 透過相依性[插入](xref:fundamentals/dependency-injection)來管理頁面相依性。
* 對頁面進行[單元測試](xref:test/razor-pages-tests)。

此頁面具有 `OnPostAsync`*處理常式方法*，它會在 `POST` 要求上執行（當使用者張貼表單時）。 您可以新增任何 HTTP 指令動詞的處理常式方法。 最常見的處理常式包括：

* `OnGet`，初始化頁所需要的狀態。 [OnGet](#OnGet) 範例。
* `OnPost`，處理表單提交作業。

`Async` 命名尾碼是選擇性的，但依照慣例通常用於非同步函式。 上述程式碼一般用於 Razor 頁面。

如果您熟悉如何使用控制器和 views 來 ASP.NET 應用程式：

* 上述範例中的 `OnPostAsync` 程式碼看起來類似一般的控制器程式碼。
* 大部分的 MVC 基本類型（例如[模型](xref:mvc/models/model-binding)系結、[驗證](xref:mvc/models/validation)、[驗證](xref:mvc/models/validation)和動作結果）都是共用的。

前一個 `OnPostAsync` 方法：

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

`OnPostAsync` 的基本流程：

檢查驗證錯誤。

* 如果沒有任何錯誤，會儲存資料並重新導向。
* 如果有錯誤，會再次顯示有驗證訊息的頁面。 用戶端驗證和傳統的 ASP.NET Core MVC 應用程式完全相同。 在許多情況下，用戶端會偵測到驗證錯誤，但從不提交給伺服器。

成功輸入資料後，`OnPostAsync` 處理常式方法會呼叫 `RedirectToPage` 協助程式方法，傳回 `RedirectToPageResult` 的執行個體。 `RedirectToPage` 是新的動作結果，類似於 `RedirectToAction` 或 `RedirectToRoute`，但會針對頁面自訂。 在上述範例中，它會重新導向至根索引頁面 (`/Index`)。 `RedirectToPage`產生頁面 URL[一節會詳細說明 ](#url_gen)。

當提交的表單有驗證錯誤時 (傳遞至伺服器)，`OnPostAsync` 處理常式方法會呼叫 `Page` 協助程式方法。 `Page` 傳回 `PageResult` 的執行個體。 傳回 `Page` 類似於控制站中的動作傳回 `View`。 `PageResult` 是處理常式方法的預設傳回型別。 傳回 `void` 的處理常式方法會呈現頁面。

`Customer` 屬性 (property) 使用 `[BindProperty]` 屬性 (attribute) 加入模型繫結。

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

根據預設，Razor Pages 只會建立屬性與非 `GET` 動詞之間的繫結。 繫結至屬性可以減少您必須撰寫的程式碼數量。 透過使用相同的屬性呈現表單欄位 (`<input asp-for="Customer.Name">`) 並接受輸入，繫結可以減少程式碼。

[!INCLUDE[](~/includes/bind-get.md)]

首頁 (*Index.cshtml*)：

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

已建立關聯的 `PageModel` 類別 (*Index.cshtml.cs*)：

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

*Index.cshtml* 檔案包含下列標記可為每個連絡人建立編輯連結：

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

`<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>`[錨點](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)標籤協助程式使用 `asp-route-{value}` 屬性來產生 [編輯] 頁面的連結。 該連結包含路由資料和連絡人識別碼。 例如： `https://localhost:5001/Edit/1` 。 [標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。 標記協助程式是由 `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` 啟用

*Pages/Edit.cshtml* 檔案：

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

第一行包含 `@page "{id:int}"` 指示詞。 路由條件約束 `"{id:int}"` 通知頁面接受包含 `int` 路由資料的頁面要求。 如果頁面要求不包含可以轉換成 `int` 的路由資料，執行階段會傳回 HTTP 404 (找不到) 錯誤。 若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：

 ```cshtml
@page "{id:int?}"
```

*Pages/Edit.cshtml.cs* 檔案：

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

*Index.cshtml* 檔案也包含能夠為每個客戶連絡人建立刪除按鈕的標記：

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

使用 HTML 轉譯刪除按鈕時，其 `formaction` 會包含下列項目的參數：

* `asp-route-id` 屬性指定的客戶連絡人識別碼。
* `handler` 屬性指定的 `asp-page-handler`。

以下是轉譯的刪除按鈕範例，內含客戶連絡人識別碼 `1`：

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

選取按鈕時，表單 `POST` 要求會傳送至伺服器。 依照慣例，會依據配置 `handler`，按 `OnPost[handler]Async` 參數的值來選取處理常式方法。

在此範例中，因為 `handler` 為 `delete`，所以會使用 `OnPostDeleteAsync` 處理常式方法來處理 `POST` 要求。 若 `asp-page-handler` 設為其他值 (例如 `remove`)，則會選取名為 `OnPostRemoveAsync` 的處理常式方法。 下列程式碼會顯示 `OnPostDeleteAsync` 處理常式：

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

`OnPostDeleteAsync` 方法：

* 接受查詢字串的 `id`。 如果 `"{id:int?}"`中包含路由條件約束的*Index. cshtml*頁面指示詞，`id` 就會來自路由資料。 `id` 的路由資料是在 URI 中指定，例如 `https://localhost:5001/Customers/2`。
* 使用 `FindAsync` 在資料庫中查詢客戶連絡人。
* 若找到客戶連絡人，會從客戶連絡人清單中予以移除。 資料庫隨即更新。
* 呼叫 `RedirectToPage` 以重新導向至根索引頁 (`/Index`)。

## <a name="mark-page-properties-as-required"></a>將頁面屬性標示為必要

`PageModel` 上的屬性可以標記為[必要](/dotnet/api/system.componentmodel.dataannotations.requiredattribute)的屬性：

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

如需詳細資訊，請參閱[模型驗證](xref:mvc/models/validation)。

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a>使用 OnGet 處理常式後援來處理 HEAD 要求

`HEAD` 要求可讓您擷取特定資源的標頭。 不同於 `GET` 要求，`HEAD` 要求不會傳回回應主體。

一般來說，會為 `OnHead` 要求建立及呼叫 `HEAD` 處理常式： 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

在 ASP.NET Core 2.1 或更新版本中，若未定義任何 `OnGet` 處理常式，Razor Pages 會轉而呼叫 `OnHead` 處理常式。 這個行為藉由在 [ 中呼叫 ](xref:mvc/compatibility-version)SetCompatibilityVersion`Startup.ConfigureServices` 來啟用：

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

預設範本會在 ASP.NET Core 2.1 和 2.2 中產生 `SetCompatibilityVersion` 呼叫。 `SetCompatibilityVersion` 實際上是將 Razor 頁面選項 `AllowMappingHeadRequestsToGetHandler` 設為 `true`。

您可以明確選擇「特定」`SetCompatibilityVersion`*行為，而不必透過*  選擇所有行為。 下列程式碼會選擇讓 `HEAD` 要求對應到 `OnGet` 處理常式：

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a>XSRF/CSRF 和 Razor Pages

您不必撰寫任何[防偽驗證](xref:security/anti-request-forgery)程式碼。 防偽權杖的產生和驗證會自動包含在 Razor Pages 中。

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>搭配 Razor Pages 使用版面配置、部分、範本和標記協助程式。

Pages 可搭配 Razor 檢視引擎的所有功能一起使用。 版面配置、部分、範本、標記協助程式、 *_ViewStart.cshtml*、 *_ViewImports.cshtml* 運作方式一如它們在傳統 Razor 檢視中的方式。

可利用這些功能的一部分來整理這個頁面。

將[版面配置頁面](xref:mvc/views/layout)新增至 *Pages/Shared/_Layout.cshtml*：

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

[配置](xref:mvc/views/layout)：

* 控制每個頁面的版面配置 (除非頁面退出版面配置)。
* 匯入 HTML 結構，例如 JavaScript 和樣式表。

如需詳細資訊，請參閱[版面配置頁面](xref:mvc/views/layout)。

[版面配置](xref:mvc/views/layout#specifying-a-layout)屬性是在 *Pages/_ViewStart.cshtml* 中設定：

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

版面配置位於 *Pages/Shared* 資料夾。 頁面會以階層方式尋找其他檢視 (版面配置、範本、部分)，從目前頁面的相同資料夾開始。 您可以從任何 Razor 頁面中的 *Pages* 資料夾下，使用 *Pages/Shared* 資料夾中的版面配置。

版面配置頁面應位於 *Pages/Shared* 資料夾中。

我們**不**建議您將配置檔案放入 *Views/Shared* 資料夾。 *Views/Shared* 是 MVC 檢視模式。 Razor 頁面應該要依賴資料夾階層，不是路徑慣例。

Razor 頁面的檢視搜尋包括 *Pages* 資料夾。 搭配 MVC 控制器使用的版面配置、範本和部分以及傳統的 Razor 檢視「就這麼簡單」。

新增 *Pages/_ViewImports.cshtml* 檔案：

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

本教學課程稍後會說明 `@namespace`。 `@addTagHelper` 指示詞會將[內建標記協助程式](xref:mvc/views/tag-helpers/builtin-th/Index)帶入 *Pages* 資料夾中的所有頁面。

<a name="namespace"></a>

在頁面上明確使用 `@namespace` 指示詞時：

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

指示詞會設定頁面的命名空間。 `@model` 指示詞不需要包含命名空間。

當 `@namespace` 指示詞包含在 *_ViewImports.cshtml* 中時，指定的命名空間會在匯入 `@namespace` 指示詞的頁面中提供所產生之命名空間的前置詞。 所產生命名空間的其餘部分 (後置字元部分) 是包含 *_ViewImports.cshtml* 的資料夾和包含頁面的資料夾之間，以句點分隔的相對路徑。

例如，`PageModel` 類別 *Pages/Customers/Edit.cshtml.cs* 會明確地設定命名空間：

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

*Pages/_ViewImports.cshtml* 檔案會設定下列命名空間：

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

為 *Pages/Customers/Edit.cshtml* Razor 頁面產生的命名空間和 `PageModel` 類別相同。

`@namespace`*也適用于傳統的 Razor views。*

原始的 *Pages/Create.cshtml* 檢視檔案：

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

更新的 *Pages/Create.cshtml* 檢視檔案：

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

[Razor Pages 入門專案](#rpvs17)包含 *Pages/_ValidationScriptsPartial.cshtml*，連結用戶端驗證。

如需部分檢視的詳細資訊，請參閱 <xref:mvc/views/partial>。

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>產生頁面 URL

前面出現過的 `Create` 頁面使用 `RedirectToPage`：

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

應用程式有下列檔案/資料夾結構：

* */Pages*

  * *Index.cshtml*
  * */Customers*

    * *Create.cshtml*
    * *Edit.cshtml*
    * *Index.cshtml*

*Pages/Customers/Create.cshtml* 和 *Pages/Customers/Edit.cshtml* 頁面在成功後會重新導向至 *Pages/Index.cshtml*。 字串 `/Index` 為 URI 的一部分，可存取前一個頁面。 字串 `/Index` 可以用來產生 *Pages/Index.cshtml* 頁面的 URI。 例如：

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

頁面名稱是從根 */Pages* 資料夾到該頁面的路徑 (包括前置的 `/`，例如 `/Index`)。 上述 URL 產生範例，透過硬式編碼的 URL 提供更加優異的選項與功能。 URL 產生使用[路由](xref:mvc/controllers/routing)，可以根據路由在目的地路徑中定義的方式，產生並且編碼參數。

產生頁面 URL 支援相關的名稱。 下表顯示從 `RedirectToPage`Pages/Customers/Create.cshtml*以不同的* 參數選取的索引頁：

| RedirectToPage(x)| 頁面 |
| ----------------- | ------------ |
| RedirectToPage("/Index") | *Pages/Index* |
| RedirectToPage("./Index"); | *Pages/Customers/Index* |
| RedirectToPage("../Index") | *Pages/Index* |
| RedirectToPage("Index")  | *Pages/Customers/Index* |

`RedirectToPage("Index")`、`RedirectToPage("./Index")` 和 `RedirectToPage("../Index")` 是「相對名稱」。 `RedirectToPage` 參數「結合」了目前頁面的路徑，以計算目的地頁面的名稱。  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

相對名稱連結在以複雜結構建置網站時很有用。 如果您使用相對名稱連結資料夾中的頁面，您可以重新命名該資料夾。 所有連結仍可運作 (因為它們不包含資料夾名稱)。

若要重新導向到不同[區域](xref:mvc/controllers/areas)中的頁面，請指定區域：

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

如需詳細資訊，請參閱 <xref:mvc/controllers/areas>。

## <a name="viewdata-attribute"></a>ViewData 屬性

資料可以傳遞至具有 [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute) 的頁面。 控制器或 Razor 頁面模型上具有 `[ViewData]` 屬性的屬性，其值會儲存並從[ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)載入。

在下列範例中，`AboutModel` 包含以 `[ViewData]`標記的 `Title` 屬性。 `Title` 屬性會設定為 [關於] 頁面的標題：

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

在 [關於] 頁面上，存取 `Title` 屬性作為模型屬性：

```cshtml
<h1>@Model.Title</h1>
```

在此配置中，標題會從 ViewData 字典中讀取：

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a>TempData

ASP.NET Core 公開[控制器](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData)上的 [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller) 屬性。 這個屬性會儲存資料，直到讀取為止。 `Keep` 和 `Peek` 方法可以用來檢查資料，不用刪除。 當有多個要求需要資料時，`TempData` 對重新導向很有幫助。

下列程式碼會設定使用 `Message` 的 `TempData` 值：

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

*Pages/Customers/Index.cshtml* 檔案中的下列標記會顯示使用 `Message` 的 `TempData` 值。

```cshtml
<h3>Msg: @Model.Message</h3>
```

*Pages/Customers/Index.cshtml.cs* 頁面模型會將 `[TempData]` 屬性 (attribute) 套用到 `Message` 屬性 (property)。

```csharp
[TempData]
public string Message { get; set; }
```

如需詳細資訊，請參閱 [TempData](xref:fundamentals/app-state#tempdata)。

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a>每頁面有多個處理常式

下列頁面會使用 `asp-page-handler` 標記協助程式為兩個處理常式產生標記：

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

上例中的表單有兩個提交按鈕，每一個都使用 `FormActionTagHelper` 提交至不同的 URL。 `asp-page-handler` 屬性附隨於 `asp-page`。 `asp-page-handler` 產生的 URL 會提交至頁面所定義的每一個處理常式方法。 因為範例連結至目前的頁面，所以未指定 `asp-page`。

頁面模型：

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

上述程式碼使用「具名的處理常式方法」。 具名的處理常式方法的建立方式是採用名稱中在 `On<HTTP Verb>` 後面、`Async` 之前 (如有) 的文字。 在上例中，頁面方法是 OnPost**JoinList**Async 和 OnPost**JoinListUC**Async。 移除 *OnPost* 和 *Async*，處理常式名稱就是 `JoinList` 和 `JoinListUC`。

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

使用上述程式碼，提交至 `OnPostJoinListAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH?handler=JoinList`。 提交至 `OnPostJoinListUCAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`。

## <a name="custom-routes"></a>自訂路由

使用 `@page` 指示詞，可以：

* 指定頁面的自訂路由。 例如，[關於] 頁面的路由可使用 `/Some/Other/Path` 設為 `@page "/Some/Other/Path"`。
* 將區段附加到頁面的預設路由。 例如，使用 `@page "item"` 可將 "item" 區段新增到頁面的預設路由。
* 將參數附加到頁面的預設路由。 例如，具有 `id` 的頁面可要求識別碼參數 `@page "{id}"`。

支援在路徑開頭以波狀符號 (`~`) 指定根相對路徑。 例如，`@page "~/Some/Other/Path"` 與 `@page "/Some/Other/Path"` 相同。

如果您不喜歡 URL 中的查詢字串 `?handler=JoinList`，請變更路由，將處理常式名稱放在 URL 的路徑部分。 藉由在 `@page` 指示詞後面加上雙引號括住的路由範本，可以自訂路由。

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

使用上述程式碼，提交至 `OnPostJoinListAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH/JoinList`。 提交至 `OnPostJoinListUCAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH/JoinListUC`。

跟在 `?` 後面的 `handler` 表示路由參數為選擇性。

## <a name="configuration-and-settings"></a>組態與設定

若要設定進階選項，請在 MVC 產生器上使用擴充方法 `AddRazorPagesOptions`：

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

目前可以使用 `RazorPagesOptions` 設定頁面的根目錄，或新增頁面的應用程式模型慣例。 我們將在未來以這種方式獲得更多的擴充性。

若要先行編譯檢視，請參閱 [Razor 檢視編譯](xref:mvc/views/view-compilation)。

[下載或檢視範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample)。

請參閱根據本簡介編纂的[開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)。

### <a name="specify-that-razor-pages-are-at-the-content-root"></a>指定 Razor Pages 位於內容根目錄

根據預設，Razor Pages 位於 */Pages* 根目錄。 將[WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot)新增至[AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) ，以指定您的 Razor Pages 位於應用程式的[內容根目錄](xref:fundamentals/index#content-root)（[ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)）：

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a>指定 Razor Pages 位於自訂根目錄

將 [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) 新增至 [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 可指定 Razor Pages 位於應用程式的自訂根目錄 (提供相對路徑)：

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a>其他資源

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end
