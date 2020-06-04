---
標題：在 ASP.NET Core 作者中設定可移植物件當地語系化： sebastienros 描述：本文介紹可移植的物件檔案，並概述在具有 Orchard 核心架構的 ASP.NET Core 應用程式中使用它們的步驟。
scaddie ms. date：09/26/2017 否-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid：基本概念/可攜性-物件-當地語系化

---
# <a name="configure-portable-object-localization-in-aspnet-core"></a>使用 ASP.NET Core 設定可攜式物件當地語系化

作者：[Sébastien Ros](https://github.com/sebastienros) 和 [Scott Addie](https://twitter.com/Scott_Addie)

本文將逐步說明在具有 [Orchard Core](https://github.com/OrchardCMS/OrchardCore) 架構的 ASP.NET Core 應用程式中使用可攜式物件 (PO) 檔案的步驟。

**注意：** Orchard Core 不是 Microsoft 產品。 因此，Microsoft 不提供這項功能的支援。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="what-is-a-po-file"></a>什麼是 PO 檔案？

PO 檔案以文字檔的形式散發，其中包含給定語言的翻譯字串。 使用 PO 檔案而不是使用 *.resx* 檔案的一些優點包括：
- PO 檔案支援複數表示；*.resx* 檔案不支援複數表示。
- PO 檔案不會像 *.resx* 檔案一樣進行編譯。 因此，不需要特殊化工具與建置步驟。
- PO 檔案適用於共同作業的線上編輯工具。

### <a name="example"></a>範例

以下是範例 PO 檔案，其中包含兩個字串的法文翻譯，包括其複數形式的字串：

*fr.po*

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

此範例使用下列語法：

- `#:`：指出所要翻譯之字串內容的註解。 相同的字串可能會根據其使用位置而翻譯為不同內容。
- `msgid`：未翻譯的字串。
- `msgstr`：已翻譯的字串。

在支援複數表示的情況下，可以定義多個項目。

- `msgid_plural`：未翻譯的複數字串。
- `msgstr[0]`：案例 0 的已翻譯字串。
- `msgstr[N]`：案例 N 的已翻譯字串。

您可以在[這裡](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html)找到 PO 檔案規格。

## <a name="configuring-po-file-support-in-aspnet-core"></a>在 ASP.NET Core 中設定 PO 檔案支援

這個範例是根據從 Visual Studio 2017 專案範本產生的 ASP.NET Core MVC 應用程式。

### <a name="referencing-the-package"></a>參考套件

將參考新增至 `OrchardCore.Localization.Core` NuGet 套件。 它可在 [MyGet](https://www.myget.org/) 的下列套件來源中取得： https://www.myget.org/F/orchardcore-preview/api/v3/index.json

*.csproj* 檔案現在包含與下列內容類似的一行 (版本號碼可能不同)：

[!code-xml[](localization/sample/2.x/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a>註冊服務

將所需的服務新增至 *Startup.cs* 的 `ConfigureServices` 方法：

[!code-csharp[](localization/sample/2.x/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

將必要的中介軟體新增至 *Startup.cs* 的 `Configure` 方法：

[!code-csharp[](localization/sample/2.x/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

將下列程式碼新增至您 Razor 選擇的視圖。 此範例中使用 *About.cshtml*。

[!code-cshtml[](localization/sample/2.x/POLocalization/Views/Home/About.cshtml)]

將會插入 `IViewLocalizer` 執行個體，用來翻譯文字 "Hello world!"。

### <a name="creating-a-po-file"></a>建立 PO 檔案

在您的應用程式根資料夾中，建立名為* \<culture code> po*的檔案。 在此範例中，檔案名稱是 *fr.po*，因為使用法文語言：

[!code-text[](localization/sample/2.x/POLocalization/fr.po)]

這個檔案會同時儲存要翻譯的字串和法文的翻譯字串。 如有必要，翻譯會還原為其父文化特性。 在此範例中，如果所要求的文化特性是 `fr-FR` 或 `fr-CA`，則會使用 *fr.po* 檔案。

### <a name="testing-the-application"></a>測試應用程式

執行您的應用程式，並巡覽至 `/Home/About` URL。 文字 **Hello world!** 就會出現。

巡覽至 `/Home/About?culture=fr-FR`RL。 文字 **Bonjour le monde!** 就會出現。

## <a name="pluralization"></a>複數表示

PO 檔案支援複數表示格式，如果相同的字串必須根據基數翻譯為不同的內容，這個檔案很有用。 由於每一種語言都會定義自訂規則來選取要根據基數使用的字串，因此這項工作變得複雜。

Orchard 當地語系化套件會提供 API，以自動叫用這些不同的複數形式。

### <a name="creating-pluralization-po-files"></a>建立複數表示 PO 檔案

將下列內容新增至先前所述的 *fr.po* 檔案：

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

如需此範例中每個項目所代表意義的說明，請參閱[什麼是 PO 檔案？](#what-is-a-po-file)。

### <a name="adding-a-language-using-different-pluralization-forms"></a>新增使用不同複數表示格式的語言

在上述範例中，已使用英文和法文字串。 英文和法文只有兩種複數表示格式，因此共用相同的格式規則，即基數一對應到第一個複數形式。 任何其他基數都對應到第二個複數形式。

並非所有語言都共用相同的規則。 以下是使用捷克文語言的說明，它有三種複數形式。

如下所示建立 `cs.po` 檔案，並記下複數表示需要三種不同翻譯的方式：

[!code-text[](localization/sample/2.x/POLocalization/cs.po)]

若要接受捷克文的當地語系化，請將 `"cs"` 新增至 `ConfigureServices` 方法所支援的文化特性清單中：

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

編輯 *Views/Home/About.cshtml* 檔案來呈現數個基數的當地語系化複數字串：

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

**注意：** 在現實世界的案例中，變數將用來代表計數。 在這裡，我們會重複使用具有三個不同值的相同程式碼，以公開非常特殊的情況。

切換文化特性後，您會看到下列內容：

針對 `/Home/About`：

```html
There is one item.
There are 2 items.
There are 5 items.
```

針對 `/Home/About?culture=fr`：

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

針對 `/Home/About?culture=cs`：

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

請注意，在捷克文文化特性中，這三種翻譯都不同。 法文和英文的文化特性則會對最後兩個翻譯字串共用相同的建構。

## <a name="advanced-tasks"></a>進階工作

### <a name="contextualizing-strings"></a>內容化字串

應用程式通常包含要在數個位置中翻譯的字串。 相同的字串在應用程式內的特定位置（views 或類別檔案）可能會有不同的翻譯 Razor 。 PO 檔案支援檔案內容的概念，可用來對所表示的字串進行分類。 使用檔案內容，字串可以根據檔案內容 (或缺乏檔案內容) 翻譯成不同的內容。

PO 當地語系化服務會使用翻譯字串時所使用的完整類別或檢視的名稱。 這是透過在 `msgctxt` 項目上設定值來完成的。

考慮對先前的 *fr.po* 範例進行微幅新增。 您 Razor 可以藉由設定保留專案的值，將位於*Views/Home/About. cshtml*的視圖定義為檔案內容 `msgctxt` ：

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

`msgctxt` 如此設定後，當巡覽至 `/Home/About?culture=fr-FR` 時，就會進行文字翻譯。 而巡覽至 `/Home/Contact?culture=fr-FR` 時，不會進行翻譯。

沒有特定項目與給定的檔案內容相符時，Orchard Core 的後援機制會在沒有內容的情況下尋找適當的 PO 檔案。 假設沒有針對 *Views/Home/Contact.cshtml* 所定義的特定檔案內容，請巡覽至 `/Home/Contact?culture=fr-FR` 以載入 PO 檔案，例如：

[!code-text[](localization/sample/2.x/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a>變更 PO 檔案的位置

PO 檔案的預設位置可以在 `ConfigureServices` 中變更：

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

在此範例中，從 *Localization* 資料夾載入 PO 檔案。

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a>實作自訂邏輯，以尋找當地語系化檔案

如果尋找 PO 檔案需要更複雜的邏輯，則可以實作 `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` 介面並將其註冊為服務。 當 PO 檔案可以儲存在不同位置或檔案必須位在資料夾階層內時，這非常有用。

### <a name="using-a-different-default-pluralized-language"></a>使用不同的預設複數化語言

此套件包含兩個複數形式特有的 `Plural` 擴充方法。 如果是需要更多複數形式的語言，請建立擴充方法。 利用擴充方法，您不需要提供預設語言的任何當地語系化檔案 &mdash; 原始字串已經可以直接在程式碼中使用。

您可以使用更通用的 `Plural(int count, string[] pluralForms, params object[] arguments)` 多載，其可接受翻譯的字串陣列。
