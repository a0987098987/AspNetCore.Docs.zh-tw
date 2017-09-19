---
title: "撰寫 ASP.NET Core 中的標記協助程式"
author: rick-anderson
description: "了解如何撰寫 ASP.NET Core 中標記協助程式。"
keywords: "ASP.NET Core，標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 06/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1a5222da1380c2fe768b287bfa1a49b300c02f2b
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a>撰寫 ASP.NET Core，範例與逐步解說中的標記協助程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample)

## <a name="getting-started-with-tag-helpers"></a>開始使用標記協助程式

本教學課程介紹程式設計標記協助程式。 [標記協助程式簡介](intro.md)描述標記協助程式提供的優點。

標記協助程式是實作任何類別`ITagHelper`介面。 不過，當您撰寫標記協助程式，您通常衍生自`TagHelper`，如此可讓您存取執行動作`Process`方法。

1. 建立新的 ASP.NET Core 專案，稱為**AuthoringTagHelpers**。 您不需要驗證這個專案。

2. 建立資料夾來存放呼叫標記協助程式*TagHelpers*。 *TagHelpers*資料夾是*不*必要，但它是合理的慣例。 現在讓我們開始撰寫一些簡單標記協助程式。

## <a name="a-minimal-tag-helper"></a>最少的標記協助程式

在本節中，您可以撰寫更新電子郵件標籤標記協助程式。 例如: 

```html
<email>Support</email>
   ```

伺服器將使用我們的電子郵件標記協助程式，將該標記轉換成下列：

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

也就是錨點標籤，可讓此電子郵件 連結。 您可以執行這項操作，如果您要撰寫的部落格引擎，並且需要其所有以相同的網域傳送電子郵件行銷、 支援和其他連絡人。

1.  加入下列`EmailTagHelper`類別*TagHelpers*資料夾。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    **注意：**
    
    * 標記協助程式使用根類別名稱的項目為目標的命名慣例 (減去*TagHelper*類別名稱的一部分)。 在此範例中的根名稱**電子郵件**TagHelper 是*電子郵件*，因此`<email>`目標標記。 此命名慣例應該適用於大部分的標記協助程式，我將示範如何覆寫它的更新版本上。
    
    * `EmailTagHelper` 類別衍生自 `TagHelper`。 `TagHelper`寫入標記協助程式類別提供方法和屬性。
    
    * 覆寫`Process`方法控制標記協助程式的用途時執行。 `TagHelper`類別也提供的非同步版本 (`ProcessAsync`) 具有相同參數。
    
    * 內容參數`Process`(和`ProcessAsync`) 包含與目前的 HTML 標記的執行相關聯的資訊。
    
    * 輸出參數`Process`(和`ProcessAsync`) 包含具狀態之 HTML 項目代表用來產生的 HTML 標記和內容的原始來源。
    
    * 我們的類別名稱的後的置字元**TagHelper**，也就是*不*必要，但會被視為最佳的作法慣例。 您可以宣告為類別：
    
    ```csharp
    public class Email : TagHelper
    ```

2.  若要讓`EmailTagHelper`類別提供至所有我們 Razor 檢視中，加入`addTagHelper`指示詞加入*Views/_ViewImports.cshtml*檔案：[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]
    
    上述程式碼會使用的萬用字元語法來指定將可使用我們的組件中的所有標記協助程式。 之後的第一個字串`@addTagHelper`指定載入標記協助程式 (使用"*"的所有標記協助程式)，而第二個字串"AuthoringTagHelpers 」 指定標記協助程式組件。 此外，請注意，第二行帶來中使用的萬用字元語法的 ASP.NET Core MVC 標記協助程式 (這些 helper 會討論[標記協助程式簡介](intro.md)。)它是`@addTagHelper`使標記協助程式可使用 Razor 檢視的指示詞。 或者，您可以提供完整的名稱 (FQN) 標記協助程式，如下所示：
    
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
    
    若要新增的檢視使用 FQN 標記協助程式，您先新增 FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)，並接著組件名稱 (*AuthoringTagHelpers*)。 大部分的開發人員會想要使用的萬用字元語法。 [標記協助程式簡介](intro.md)進入 標記協助程式加入、 移除、 階層及萬用字元語法中的 詳細資料。
    
3.  更新中的標記*Views/Home/Contact.cshtml*這些變更的檔案：

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  執行應用程式，並使用您最愛的瀏覽器檢視的 HTML 原始檔，以便您可以確認電子郵件標籤都會取代成錨定標記 (例如， `<a>Support</a>`)。 *支援*和*行銷*會轉譯為連結，但是沒有`href`屬性，使其功能。 我們將修正的下一節。

注意： 如 HTML 標記和屬性、 標記、 類別名稱和 Razor，和 C# 中的屬性不區分大小寫。

## <a name="setattribute-and-setcontent"></a>SetAttribute 和 SetContent

在本節中，我們會將更新`EmailTagHelper`，這樣才會建立電子郵件的有效錨定標記。 我們會將更新其需要從 Razor 檢視的資訊 (的形式`mail-to`屬性) 並使用該產生錨點。

更新`EmailTagHelper`具有下列類別：

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

**注意：**

* 依照 pascal 命名法的大小寫慣例類別和屬性名稱標記協助程式會轉譯成其[降低 kebab 案例](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)。 因此，若要使用`MailTo`屬性，您將使用`<email mail-to="value"/>`相等。

* 最後一行會設定已完成的內容，我們使用最低限度功能標記協助程式。

* 反白顯示的線條將會顯示加入屬性的語法：

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

該方法的效果 」 href"的屬性，只要它目前並不存在於屬性集合。 您也可以使用`output.Attributes.Add`方法，以加入標記屬性集合的結尾標記協助程式屬性。

1.  更新中的標記*Views/Home/Contact.cshtml*這些變更的檔案：[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

2.  執行應用程式，並確認它會產生正確的連結。
    
    > [!NOTE]
    >如果您要撰寫電子郵件標籤自我結尾 (`<email mail-to="Rick" />`)，最後的輸出也會自我結尾。 若要啟用寫入將標記與開始標記的能力 (`<email mail-to="Rick">`) 您也必須裝飾具有下列類別：
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    具有自我結尾的電子郵件標記協助程式，輸出會`<a href="mailto:Rick@contoso.com" />`。 自我結尾的錨定標記不是有效的 HTML，因此您不想要建立一個，但是您可能想要建立已自我結尾標記協助程式。 標記協助程式設定的類型`TagMode`讀取標記之後的屬性。
    
### <a name="processasync"></a>ProcessAsync

本節中，我們會撰寫的非同步的電子郵件協助程式。

1.  取代`EmailTagHelper`類別為下列程式碼：

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    **注意：**

    * 此版本使用非同步`ProcessAsync`方法。 非同步`GetChildContentAsync`傳回`Task`包含`TagHelperContent`。

    * 使用`output`參數，以取得 HTML 項目的內容。

2.  請進行下列變更*Views/Home/Contact.cshtml*檔案標記協助程式可以取得目標電子郵件。

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  執行應用程式，並確認它會產生有效的電子郵件連結。

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll、 PreContent.SetHtmlContent 和 PostContent.SetHtmlContent

1.  加入下列`BoldTagHelper`類別*TagHelpers*資料夾。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    **注意：**
    
    * `[HtmlTargetElement]`屬性的將屬性參數會指定任何包含的 HTML 屬性的 HTML 項目名稱為"bold"會比對，而`Process`類別中的覆寫方法會執行。 在我們的範例，`Process`方法移除 「 粗體 」 屬性，而且括住包含標記與`<strong></strong>`。
    
    * 因為您不想要取代現有的標籤內容，您必須撰寫開啟`<strong>`標記`PreContent.SetHtmlContent`方法和關閉`</strong>`標記`PostContent.SetHtmlContent`方法。
    
2.  修改*About.cshtml*檢視，以包含`bold`屬性值。 已完成的程式碼如下所示。

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  執行應用程式。 您可以使用您最愛的瀏覽器檢查來源，並確認標記。

    `[HtmlTargetElement]`上述的屬性只能為目標的 HTML 標記，提供的 「 粗體 」 的屬性名稱。 `<bold>`標記協助程式 」 所未修改項目。

4. 標記為註解`[HtmlTargetElement]`屬性行，它會預設為目標`<bold>`標記，也就是表單的 HTML 標記`<bold>`。 請記住，預設命名慣例將符合類別名稱**粗體**至 TagHelper`<bold>`標記。

5. 執行應用程式，並確認`<bold>`標記協助程式 」 所處理標記。

裝飾的類別具有多個`[HtmlTargetElement]`屬性的目標在邏輯 OR 結果。 例如，使用下列程式碼，粗體顯示標記或粗體屬性會比對。

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

當多個屬性加入至相同的陳述式時，執行階段會將它們視為邏輯 and。 例如，下列程式碼，HTML 項目必須命名為"bold"與屬性名稱為"bold"(`<bold bold />`) 來比對。

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

您也可以使用`[HtmlTargetElement]`若要變更目標項目的名稱。 例如，如果您想`BoldTagHelper`目標`<MyBold>`標記，您可以使用下列屬性：

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a>將模型傳遞至標記協助程式

1.  新增*模型*資料夾。

2.  將下列 `WebsiteContext` 類別新增至 *Models* 資料夾：

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  加入下列`WebsiteInformationTagHelper`類別*TagHelpers*資料夾。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    **注意：**
    
    * 如前所述，標記協助程式將使用 Pascal 大小寫 C# 類別名稱和屬性為標記協助程式至[降低 kebab 案例](http://wiki.c2.com/?KebabCase)。 因此，若要使用`WebsiteInformationTagHelper`Razor，在您將撰寫`<website-information />`。
    
    * 明確地不識別目標項目與`[HtmlTargetElement]`屬性，因此預設值是`website-information`的目標。 如果您套用下列屬性 （請注意不 kebab 這種情況，但符合類別名稱）：
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    較低的 kebab case 標籤`<website-information />`也不會相符。 如果您想使用`[HtmlTargetElement]`屬性，您會使用 kebab 案例，如下所示：
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * 不能自我結尾的項目有內容。 此範例中，Razor 標記將會使用自我結尾標記，但標記協助程式會建立[區段](http://www.w3.org/TR/html5/sections.html#the-section-element)項目 (不是自我結尾，而且您要撰寫內部內容`section`項目)。 因此，您需要設定`TagMode`至`StartTagAndEndTag`來寫入輸出。 或者，您可以註解線條設定`TagMode`和寫入結尾標記的標記。 （稍後在本教學課程會提供範例標記）。
    
    * `$` （貨幣符號） 下面這一行會使用[以內插值取代字串](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  加入下列標記以*About.cshtml*檢視。 反白顯示的標記會顯示網站的資訊。
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > 在 Razor 標記，如下所示：
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    >Razor 知道`info`屬性類別，而不是字串，並且您想要撰寫 C# 程式碼。 任何非字串標記協助程式屬性應該寫入不含`@`字元。
    
5.  執行應用程式，並瀏覽到關於檢視來查看網站資訊。

    >[!NOTE]
    >您可以使用下列標記與結束標記，並移除那一行`TagMode.StartTagAndEndTag`標記協助程式中：
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a>條件標記協助程式

條件標記協助程式轉譯輸出時，則為 true 的值傳遞。

1.  加入下列`ConditionTagHelper`類別*TagHelpers*資料夾。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  取代內容*Views/Home/Index.cshtml*檔案以下列標記：

    <!-- literal_block {"xml:space": "preserve", "source": "mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml", "ids": [], "linenos": false, "highlight_args": {"linenostart": 1}} -->
    
    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=Model />
        <div condition="Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  取代`Index`方法中的`Home`控制器會執行下列程式碼：

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  執行應用程式，並瀏覽至首頁。 在條件中的標記`div`將不會轉譯。 將查詢字串附加`?approved=true`的 url (例如， `http://localhost:1235/Home/Index?approved=true`)。 `approved`設定為 true，且條件會顯示標記。

>[!NOTE]
>使用[nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof)運算子，來指定目標，而不是指定字串，您可以如同使用粗體顯示標記協助程式屬性：
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
>[Nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof)運算子將保護的程式碼應該它曾經重構 (我們可能會想要將名稱變更為`RedCondition`)。

### <a name="avoiding-tag-helper-conflicts"></a>避免衝突標記協助程式

本節中，您可以撰寫一組的自動連結標記協助程式。 第一個將會取代標記包含 HTML 錨定標記包含相同的 URL （並因而產生連結至 URL） 從 HTTP URL。 第二個會執行相同動作 URL 的開頭為 WWW。

由於這些兩個 helper 密切相關，而且可能重構它們在未來，我們會讓它們保持在相同的檔案。

1.  加入下列`AutoLinkerHttpTagHelper`類別*TagHelpers*資料夾。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    >`AutoLinkerHttpTagHelper`類別目標`p`項目，並使用[Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)建立錨點。

2.  加入下列標記的結尾*Views/Home/Contact.cshtml*檔案：

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  執行應用程式，並確認標記協助程式會正確轉譯錨點。

4.  更新`AutoLinker`類別，以包含`AutoLinkerWwwTagHelper`這會將 www 文字轉換也包含原始 www 文字的錨點標籤。 更新的程式碼會反白顯示，如下：

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  執行應用程式。 請注意 www 文字呈現為連結，但不是 HTTP 文字。 如果您將中斷點放在這兩個類別時，您可以看到第一個 HTTP 標記協助程式類別執行程式。 快取標記協助程式輸出，而 WWW 標記協助程式執行時，它會覆寫快取的輸出從 HTTP 標記協助程式問題。 稍後在教學課程中，我們會看到如何控制標記協助程式執行中的順序。 我們將修正程式碼以下列：

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    >在第一個版本中的自動連結標記協助程式，您會取得以下列程式碼目標的內容：
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    >也就是說，呼叫`GetChildContentAsync`使用`TagHelperOutput`傳入`ProcessAsync`方法。 如所述以前，因為快取輸出，則最後一個標記協助程式 」 執行 wins。 您已修正此問題，下列程式碼：
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    >上述程式碼會檢查已修改的內容，如果有，從輸出緩衝區取得內容。

6.  執行應用程式，並確認兩個連結在如預期般運作。 雖然可能會顯示為我們自動連結器標記協助程式即會正確且完整，它有微妙的問題。 如果 WWW 標記協助程式會先執行，將無法正確 www 連結。 更新程式碼加入`Order`控制標記以執行順序的多載。 `Order`屬性會決定相對於其他標記協助程式，以相同的項目為目標的執行順序。 預設順序值為 0，且執行個體具有較低值會優先執行。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    上述程式碼將會保證 HTTP 標記協助程式執行之前 WWW 標記協助程式。 變更`Order`至`MaxValue`並確認產生 WWW 標記的標記不正確。

## <a name="inspecting-and-retrieving-child-content"></a>檢查及擷取子內容

標記協助程式提供數個屬性，擷取內容。

-  結果`GetChildContentAsync`可以附加至`output.Content`。
-  您可以檢查的結果`GetChildContentAsync`與`GetContent`。
-  如果您修改`output.Content`，不會執行或轉譯，除非您呼叫 TagHelper 主體`GetChildContentAsync`如我們自動連結器範例所示：

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  多個呼叫`GetChildContentAsync`會傳回相同的值，而不會重新執行`TagHelper`主體，除非您參數中傳遞 false 表示不使用快取的結果。
