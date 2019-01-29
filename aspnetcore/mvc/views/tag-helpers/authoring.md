---
title: ASP.NET Core 中的編寫標籤協助程式
author: rick-anderson
description: 了解如何在 ASP.NET Core 中編寫標籤協助程式。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: c21decd39b7855cf2eefb2bb482e5e91b9487863
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889934"
---
# <a name="author-tag-helpers-in-aspnet-core"></a>ASP.NET Core 中的編寫標籤協助程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="get-started-with-tag-helpers"></a>開始使用標籤協助程式

本教學課程介紹如何以程式設計標籤協助程式。 [標籤協助程式簡介](intro.md)描述標籤協助程式所提供的優點。

標籤協助程式是任何可實作 `ITagHelper` 介面的類別。 不過，當您編寫標籤協助程式時，通常是衍生自 `TagHelper`，如此可讓您存取 `Process` 方法。

1. 建立稱為 **AuthoringTagHelpers** 的新 ASP.NET Core 專案。 您不需要驗證此專案。

1. 建立資料夾以保存稱為 *TagHelpers* 的標籤協助程式。 *TagHelpers* 資料夾「不」是必要的，但是為合理的慣例。 現在開始撰寫一些簡單的標籤協助程式。

## <a name="a-minimal-tag-helper"></a>最精簡的標籤協助程式

在本節中，您會撰寫可更新電子郵件標籤的標籤協助程式。 例如：

```html
<email>Support</email>
```

伺服器將使用我們的電子郵件標籤 (tag) 協助程式，將該標籤 (markup) 轉換成下列項目：

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

也就是，將此設為電子郵件連結的錨點標籤。 如果您要撰寫部落格引擎，並且需要它才能傳送行銷、支援和其他連絡人 (全都位在相同的網域) 的電子郵件，則可能會想要執行這項操作。

1. 將下列 `EmailTagHelper` 類別新增至 *TagHelpers* 資料夾。

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]

   * 標籤協助程式使用以根類別名稱的項目為目標的命名慣例 (去掉類別名稱的 *TagHelper* 部分)。 在此範例中，**EmailTagHelper** 的根名稱是 *email*，因此將會以 `<email>` 標籤為目標。 此命名慣例應該適用於大部分的標籤協助程式，稍後將示範如何行覆寫它。

   * `EmailTagHelper` 類別衍生自 `TagHelper`。 `TagHelper` 類別提供撰寫標籤協助程式的方法和屬性。

   * 覆寫的 `Process` 方法控制標籤協助程式在其執行時的作用。 `TagHelper` 類別也會使用相同的參數來提供非同步版本 (`ProcessAsync`)。

   * `Process` (和 `ProcessAsync`) 的內容參數包含與目前 HTML 標籤執行建立關聯的資訊。

   * `Process` (和 `ProcessAsync`) 的輸出參數包含具狀態 HTML 項目，以呈現用來產生 HTML 標籤和內容的原始來源。

   * 類別名稱的尾碼為 **TagHelper**，這「不」是必要的，但視為最佳做法慣例。 您可以將類別宣告為：

   ```csharp
   public class Email : TagHelper
   ```

1. 若要讓 `EmailTagHelper` 類別可供所有 Razor 檢視使用，請將 `addTagHelper` 指示詞新增至 *Views/_ViewImports.cshtml* 檔案：[!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]

   上述程式碼使用萬用字元語法，指定將使用組件中的所有標籤協助程式。 `@addTagHelper` 後面的第一個字串指定要載入的標籤協助程式 (使用 "*" 表示所有標籤協助程式)，而第二個字串 "AuthoringTagHelpers" 指定標籤協助程式所在的組件。 另請注意，第二行使用萬用字元語法帶入 ASP.NET Core MVC 標籤協助程式 ([標籤協助程式簡介](intro.md)會討論這些協助程式)。它是 `@addTagHelper` 指示詞，讓標籤協助程式可供 Razor 檢視使用。 或者，您可以提供標籤協助程式的完整名稱 (FQN)，如下所示：

```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->

若要使用 FQN 將標籤協助程式新增至檢視，請依序新增 FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) 與**組件名稱** (*AuthoringTagHelpers*，它不一定是 `namespace`)。 大部分開發人員都會想要使用萬用字元語法。 [標籤協助程式簡介](intro.md)會詳述標籤協助程式新增、移除、階層和萬用字元語法。

1. 使用下列變更來更新 *Views/Home/Contact.cshtml* 檔案中的標記：

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. 執行應用程式，並使用慣用的瀏覽器來檢視 HTML 原始檔，讓您可以驗證電子郵件標籤已取代為錨點標籤 (例如，`<a>Support</a>`)。 *Support* 和 *Marketing* 會轉譯為連結，但沒有 `href` 屬性可讓它們運作。 我們將在下節修正該問題。

## <a name="setattribute-and-setcontent"></a>SetAttribute 和 SetContent

在本節中，我們將更新 `EmailTagHelper`，以建立電子郵件的有效錨點標籤。 我們將會更新它，以從 Razor 檢視取得資訊 (以 `mail-to` 屬性形式)，並使用這項資訊來產生錨點。

使用下列程式碼更新 `EmailTagHelper` 類別：

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

* 標籤協助程式依照 Pascal 命名法大小寫慣例的類別和屬性名稱會轉譯成其[小寫 kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)。 因此，若要使用 `MailTo` 屬性，您將使用 `<email mail-to="value"/>` 對等項目。

* 最後一行設定功能最小之標籤協助程式的已完成內容。

* 反白顯示的線條會顯示新增屬性的語法：

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

只要 attributes 集合中目前沒有 "href" 屬性，該方法就適用於 "href" 屬性。 您也可以使用 `output.Attributes.Add` 方法，將標籤協助程式屬性新增至標籤屬性集合結尾。

1. 使用下列變更來更新 *Views/Home/Contact.cshtml* 檔案中的標記：[!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

1. 執行應用程式，並驗證它會產生正確的連結。

<a name="self-closing"></a>

   > [!NOTE]
   > 如果您要撰寫電子郵件標籤自我結尾 (`<email mail-to="Rick" />`)，則最終輸出也會是自我結尾。 若要啟用可以寫入只有開始標籤的標籤 (`<email mail-to="Rick">`)，您必須以下列項目來裝飾此類別：
   >
   > [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]

   使用自我結尾的電子郵件標籤協助程式時，輸出會是 `<a href="mailto:Rick@contoso.com" />`。 自我結尾的錨點標籤不是有效的 HTML，因此您不會想要建立它；但您可能會想要建立自我結尾的標籤協助程式。 標籤協助程式會在讀取標籤之後設定 `TagMode` 屬性的類型。

### <a name="processasync"></a>ProcessAsync

在本節中，我們將撰寫非同步電子郵件協助程式。

1. 將 `EmailTagHelper` 類別取代為下列程式碼：

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

   **注意：**

   * 此版本使用非同步 `ProcessAsync` 方法。 非同步 `GetChildContentAsync` 會傳回包含 `TagHelperContent` 的 `Task`。

   * 使用 `output` 參數，以取得 HTML 項目的內容。

1. 對 *Views/Home/Contact.cshtml* 檔案進行下列變更，讓標籤協助程式可以取得目標電子郵件。

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. 執行應用程式，並驗證它產生有效的電子郵件連結。

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll、PreContent.SetHtmlContent 和 PostContent.SetHtmlContent

1. 將下列 `BoldTagHelper` 類別新增至 *TagHelpers* 資料夾。

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

   * `[HtmlTargetElement]` 屬性會傳遞屬性參數，以指定是否有任何包含名為 "bold" 之 HTML 屬性的 HTML 項目相符，並且執行類別中的 `Process` 覆寫方法。 在我們的範例中，`Process` 方法會移除 "bold" 屬性，並使用 `<strong></strong>` 括住包含標記。

   * 因為您不想要取代現有標籤內容，所以必須使用 `PreContent.SetHtmlContent` 方法來撰寫開頭 `<strong>` 標籤，並使用 `PostContent.SetHtmlContent` 方法來撰寫結尾 `</strong>` 標籤。

1. 修改 *About.cshtml* 檢視，以包含 `bold` 屬性值。 已完成的程式碼如下所示。

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

1. 執行應用程式。 您可以使用慣用的瀏覽器來檢查原始檔，並驗證標記。

   上面的 `[HtmlTargetElement]` 屬性只會將目標設為提供屬性名稱 "bold" 的 HTML 標記。 標籤協助程式未曾修改 `<bold>` 項目。

1. 將 `[HtmlTargetElement]` 屬性行標籤為註解，而且它會預設為目標 `<bold>` 標籤 (tag)，也就是表單 `<bold>` 的 HTML 標記 (markup)。 請記住，預設命名慣例將符合類別名稱 **Bold**TagHelper 與 `<bold>` 標籤。

1. 執行應用程式，並驗證標籤協助程式已處理 `<bold>` 標籤。

以多個 `[HtmlTargetElement]` 屬性裝飾類別，會導致目標的邏輯 OR。 例如，使用下列程式碼，粗體標籤或粗體屬性會相符。

[!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

將多個屬性新增至相同的陳述式時，執行階段會將它們視為邏輯 AND。 例如，在下列程式碼中，HTML 項目必須命名為具有 "bold" 屬性的 "bold" (`<bold bold />`) 才能相符。

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

您也可以使用 `[HtmlTargetElement]` 來變更目標項目的名稱。 例如，如果您想要 `BoldTagHelper` 將目標設為 `<MyBold>` 標籤，您可以使用下列屬性：

```csharp
[HtmlTargetElement("MyBold")]
```

## <a name="pass-a-model-to-a-tag-helper"></a>將模型傳遞至標籤協助程式

1. 新增 *Models* 資料夾。

1. 將下列 `WebsiteContext` 類別新增至 *Models* 資料夾：

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

1. 將下列 `WebsiteInformationTagHelper` 類別新增至 *TagHelpers* 資料夾。

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]

   * 如前所述，標籤協助程式會將標籤協助程式依照 Pascal 大小寫 C# 命名法大小寫慣例的類別名稱和屬性轉譯成[小寫 kebab](http://wiki.c2.com/?KebabCase)。 因此，若要在 Razor 中使用 `WebsiteInformationTagHelper`，您將撰寫 `<website-information />`。

   * 您未明確識別具有 `[HtmlTargetElement]` 屬性的目標項目，因此會將預設值 `website-information` 設為目標。 如果您已套用下列屬性 (請注意，它不是 Kebab 大小寫，但符合類別名稱)：

   ```csharp
   [HtmlTargetElement("WebsiteInformation")]
   ```

   小寫 kebab 標籤 `<website-information />` 將不相符。 如果您想要使用 `[HtmlTargetElement]` 屬性，請使用 Kebab 大小寫，如下所示：

   ```csharp
   [HtmlTargetElement("Website-Information")]
   ```

   * 自我結尾項目沒有內容。 在此範例中，Razor 標記 (markup) 將使用自我結尾標籤 (tag)，但標籤 (tag) 協助程式將會建立 [section](http://www.w3.org/TR/html5/sections.html#the-section-element) 項目 (此項目不是自我結尾的，而且您將在 `section` 項目內撰寫內容)。 因此，您需要將 `TagMode` 設定為 `StartTagAndEndTag`，才能撰寫輸出。 或者，您可以將設定 `TagMode` 的行設定為註解，並撰寫含結尾標籤 (tag) 的標籤 (markup)  (本教學課程稍後會提供範例標記)。

   * 下行中的 `$` (貨幣符號) 使用[插入字串](/dotnet/csharp/language-reference/keywords/interpolated-strings)：

   ```cshtml
   $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
   ```

1. 將下列標記新增至 *About.cshtml* 檢視。 反白顯示的標記會顯示網站資訊。

   [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-999)]

   > [!NOTE]
   > 在下面所顯示的 Razor 標記中：
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
   >
   > Razor 知道 `info` 屬性是類別，而非字串，而且您想要撰寫 C# 程式碼。 應該撰寫任何無 `@` 字元的非字串標籤協助程式屬性。

1. 執行應用程式，並巡覽至 About 檢視來查看網站資訊。

   > [!NOTE]
   > 您可以搭配使用下列標籤 (markup) 與結尾標籤 (tag)，並移除標籤 (tag) 協助程式中包含 `TagMode.StartTagAndEndTag` 的行：
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a>條件標籤協助程式

傳遞 true 值時，條件標籤協助程式會轉譯輸出。

1. 將下列 `ConditionTagHelper` 類別新增至 *TagHelpers* 資料夾。

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

1. 將 *Views/Home/Index.cshtml* 檔案的內容取代為下列標記：

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml)]

1. 將 `Home` 控制器中的 `Index` 方法取代為下列程式碼：

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

1. 執行應用程式，並瀏覽至首頁。 將不會轉譯條件式 `div` 中的標記。 將查詢字串 `?approved=true` 附加至 URL (例如，`http://localhost:1235/Home/Index?approved=true`)。 `approved` 設定為 true，並且顯示條件式標記。

> [!NOTE]
> 使用 [nameof](/dotnet/csharp/language-reference/keywords/nameof) 運算子來指定要設為目標的屬性，而不是指定字串，就像使用粗體標籤協助程式一樣：
>
> [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
> [nameof](/dotnet/csharp/language-reference/keywords/nameof) 運算子將會保護已重構的程式碼 (我們可能會想要將名稱變更為 `RedCondition`)。

### <a name="avoid-tag-helper-conflicts"></a>避免標籤協助程式衝突

在本節中，您會撰寫一對自動連結標籤協助程式。 第一個會將包含開頭為 HTTP 之 URL 的標籤 (markup) 取代為包含相同 URL 的 HTML 錨點標籤 (tag) (因此產生 URL 的連結)。 第二個會針對開頭為 WWW 的 URL 執行相同的動作。

因為這兩個協助程式密切相關，而且您未來可能會將它們重構，所以我們將它們保留在相同的檔案中。

1. 將下列 `AutoLinkerHttpTagHelper` 類別新增至 *TagHelpers* 資料夾。

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

   >[!NOTE]
   >`AutoLinkerHttpTagHelper` 類別將目標設為 `p` 項目，並使用 [Regex](/dotnet/standard/base-types/regular-expression-language-quick-reference) 來建立錨點。

1. 將下列標記新增至 *Views/Home/Contact.cshtml* 檔案結尾：

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

1. 執行應用程式，並驗證標籤協助程式正確地轉譯錨點。

1. 更新 `AutoLinker` 類別以包括 `AutoLinkerWwwTagHelper`，這會將 www 文字轉換成也包含原始 www 文字的錨點標籤。 已更新的程式碼反白顯示如下：

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

1. 執行應用程式。 請注意，www 文字會轉譯為連結，但 HTTP 文字則否。 如果您將中斷點放在兩個類別中，則可以看到先執行 HTTP 標籤協助程式類別。 問題在於已快取標籤協助程式輸出，而且執行 WWW 標籤協助程式時，會覆寫 HTTP 標籤協助程式的已快取輸出。 在教學課程稍後，將了解如何控制標籤協助程式的執行順序。 我們將使用下列內容來修正程式碼：

   [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

   > [!NOTE]
   > 在第一版的自動連結標籤協助程式中，您已使用下列程式碼取得目標的內容：
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
   >
   > 也就是說，使用傳入 `ProcessAsync` 方法的 `TagHelperOutput`，即可呼叫 `GetChildContentAsync`。 如前所述，因為已快取輸出，所以會執行最後一個標籤協助程式。 您已使用下列程式碼修正該問題：
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
   >
   > 上述程式碼會確認已修改內容；如果已修改，則會從輸出緩衝區中取得內容。

1. 執行應用程式，並驗證兩個連結如預期運作。 雖然可能會顯示自動連結器標籤協助程式正確且完整，但是它有些微小問題。 如果先執行 WWW 標籤協助程式，則 www 連結會不正確。 新增 `Order` 多載以控制標籤執行順序，來更新程式碼。 `Order` 屬性可決定相對於以相同項目為目標的其他標籤協助程式的執行順序。 預設順序值為零，而且會先執行值較小的執行個體。

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]

   上述程式碼保證先執行 HTTP 標籤協助程式，再執行 WWW 標籤協助程式。 將 `Order` 變更為 `MaxValue`，並驗證針對 WWW 標籤 (tag) 所產生的標籤 (markup) 不正確。

## <a name="inspect-and-retrieve-child-content"></a>檢查並擷取子內容

標籤協助程式提供數個屬性來擷取內容。

* `GetChildContentAsync` 的結果可以附加至 `output.Content`。
* 您可以使用 `GetContent` 來檢查 `GetChildContentAsync` 的結果。
* 如果您修改 `output.Content`，則除非您呼叫 `GetChildContentAsync` 作為自動連結器範例，否則不會執行或轉譯 TagHelper 本文：

[!code-csharp[](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

* 多次呼叫 `GetChildContentAsync` 會傳回相同的值，而且除非您傳入 false 參數以指出不使用已快取內容，否則不會重新執行 `TagHelper` 本文。
