---
title: "設定可攜式物件當地語系化"
author: sebastienros
description: "本文介紹可攜式物件檔案，並且摘要說明 Orchard 核心架構的 ASP.NET Core 應用程式中使用它們的步驟。"
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: ad68c8a7df5a8ea0f7ef42137c29cd3b37657052
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="configure-portable-object-localization-with-orchard-core"></a>設定可攜式物件當地語系化 Orchard 核心

由[Sébastien Ros](https://github.com/sebastienros)和[Scott Addie](https://twitter.com/Scott_Addie)

本文逐步使用可攜式物件 (PO) 檔案的 ASP.NET Core 應用程式中的步驟[Orchard 核心](https://github.com/OrchardCMS/OrchardCore)架構。

**注意：** Orchard 核心不是一項 Microsoft 產品。 因此，Microsoft 不提供支援這項功能。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-po-file"></a>什麼是 PO 檔案？

發佈 PO 檔案為文字檔案，其中包含給定語言的翻譯的字串。 改為使用 PO 檔案的一些優點*.resx*檔案包括：
- PO 檔案支援複數表示。*.resx*檔案不支援複數表示。
- PO 檔案不像編譯*.resx*檔案。 因此，特製化的工具與建置步驟不需要。
- PO 檔案適用於的共同作業的線上編輯工具。

### <a name="example"></a>範例

以下是範例 PO 檔案包含兩個字串以法文顯示，包括另一個複數形式的翻譯：

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

這個範例會使用下列語法：

- `#:`: 指出要翻譯的字串內容的註解。 相同的字串可能會以不同的方式轉譯，其中它會依據所使用。
- `msgid`: 轉譯的字串。
- `msgstr`： 用來轉換的字串。

在複數表示支援的情況下可以定義多個項目。

- `msgid_plural`： 用來轉譯複數字串。
- `msgstr[0]`： 0 的情況已翻譯的字串。
- `msgstr[N]`: 大小 n 翻譯的字串

您可以找到 PO 檔案規格[這裡](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html)。

## <a name="configuring-po-file-support-in-aspnet-core"></a>設定在 ASP.NET Core PO 檔案支援

這個範例根據從 Visual Studio 2017 專案範本產生 ASP.NET Core MVC 應用程式。

### <a name="referencing-the-package"></a>參考封裝

將參考加入`OrchardCore.Localization.Core`NuGet 封裝。 它是用於[MyGet](https://www.myget.org/)在下列的封裝來源： https://www.myget.org/F/orchardcore-preview/api/v3/index.json

*.Csproj*檔案現在會包含與下列相似的一行 （版本號碼可能不同）：

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a>註冊服務

加入必要的服務，可`ConfigureServices`方法*Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

加入必要的中介軟體到`Configure`方法*Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

將下列程式碼加入至所選擇的 Razor 檢視。 *About.cshtml*此範例中使用。

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

`IViewLocalizer`執行個體是插入的程式，用於翻譯文字"Hello world ！"。

### <a name="creating-a-po-file"></a>建立 PO 檔案

建立名為 *<culture code>.po*應用程式根資料夾中。 在此範例中，檔案名稱是*fr.po*因為法文語言用來：

[!code-text[Main](localization/sample/POLocalization/fr.po)]

這個檔案會儲存要翻譯的字串和法文翻譯的字串。 翻譯會還原為其父文化特性，如有必要的範圍。 在此範例中， *fr.po*如果要求的文化特性，則會使用檔案`fr-FR`或`fr-CA`。

### <a name="testing-the-application"></a>測試應用程式

執行您的應用程式，並瀏覽至 URL `/Home/About`。 文字**Hello world ！** 隨即出現。

瀏覽至 URL `/Home/About?culture=fr-FR`。 文字**le、 全球、 宇宙 Bonjour ！** 隨即出現。

## <a name="pluralization"></a>複數表示

PO 檔案支援複數表示表單會在相同的字串必須轉譯根據基數時很有用。 這項工作中進行複雜的每一種語言定義自訂規則以選取要使用的字串會根據基數的事實。

Orchard 當地語系化套件會提供 API，以自動叫用這些不同的複數形式。

### <a name="creating-pluralization-po-files"></a>建立複數表示 PO 檔案

將下列內容加入至先前所述*fr.po*檔案：

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

請參閱[PO 檔案是什麼？](#what-is-a-po-file)需在此範例中的每個項目所代表的意義的說明。

### <a name="adding-a-language-using-different-pluralization-forms"></a>新增語言，使用不同的複數表示表單

在上述範例中，已使用英文和法文字串。 英文和法文有只有兩個複數表示表單而共用相同的格式規則，這是基數為一會對應到第一個複數形式。 任何其他基數被對應到第二個複數形式。

並非所有語言都共用相同的規則。 以下是使用捷克文語言有三個複數形式。

建立`cs.po`檔案，如下所示，並記下複數表示需要三個不同的轉譯的方式：

[!code-text[Main](localization/sample/POLocalization/cs.po)]

若要接受捷克文的當地語系化資源，請加入`"cs"`中支援的文化特性的清單`ConfigureServices`方法：

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

編輯*Views/Home/About.cshtml*來呈現數個基數的當地語系化的複數字串的檔案：

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

**注意：**在真實世界案例中，變數會用來代表的計數。 在這裡，我們會重複相同的程式碼具有三個不同的值，以公開非常特定的大小寫。

切換文化特性而有所不同，您可以看到下列：

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

請注意，捷克文文化特性中，三個翻譯不同。 法文和英文的文化特性會共用相同的兩個的最後一個翻譯字串的建構。

## <a name="advanced-tasks"></a>進階的工作

### <a name="contextualizing-strings"></a>Contextualizing 字串

應用程式通常會包含要在幾個位置中轉譯的字串。 相同的字串中 （Razor 檢視或類別檔案） 的應用程式內的特定位置，可能有不同的轉譯。 PO 檔案支援的檔案內容，可用來分類所表示的字串。 使用檔案內容，字串將不同的轉譯，根據檔案內容 （或缺乏的檔案內容）。

PO 當地語系化服務使用完整的類別或檢視轉譯的字串時所使用的名稱。 這是上設定的值`msgctxt`項目。

考慮先前的次要新增*fr.po*範例。 Razor 檢視位於*Views/Home/About.cshtml*可以藉由設定保留定義的檔案內容為`msgctxt`項目的值：

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

與`msgctxt`設定在這種情況，文字轉譯時發生瀏覽至`/Home/About?culture=fr-FR`。 若要瀏覽時，就不會發生轉譯`/Home/Contact?culture=fr-FR`。

沒有特定的項目會與指定的檔案內容比對，Orchard 核心後援機制會尋找適當的 PO 檔案沒有內容。 假設有已定義的任何特定的檔案內容*Views/Home/Contact.cshtml*、 瀏覽至`/Home/Contact?culture=fr-FR`載入 PO 檔案，例如：

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a>變更 PO 檔案的位置

PO 檔案的預設位置可以變更在`ConfigureServices`:

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

在此範例中，從載入 PO 檔案*當地語系化*資料夾。

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a>實作自訂的邏輯，以尋找當地語系化檔案

要尋找 PO 檔案需要更複雜的邏輯時`OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider`介面可以實作，並註冊為服務。 當 PO 檔案可以儲存在不同的位置或檔案具有要尋找的資料夾階層內時，這非常有用。

### <a name="using-a-different-default-pluralized-language"></a>使用 pluralized 不同預設語言

封裝包含`Plural`屬於兩個複數形式的擴充方法。 對於需要更多的複數形式的語言，建立擴充方法。 使用擴充方法，您不需要提供任何預設語言的當地語系化檔案&mdash;的原始字串已直接在程式碼中，您可以使用。

您可以使用多個泛型`Plural(int count, string[] pluralForms, params object[] arguments)`多載會接受字串陣列中的翻譯。
