---
title: "全球化和當地語系化的 ASP.NET Core"
author: rick-anderson
description: "了解如何 ASP.NET Core 提供服務和中介軟體將內容當地語系化成不同的語言和文化特性。"
keywords: "ASP.NET Core 當地語系化，文化特性、 語言、 資源檔、 全球化、 國際化、 地區設定"
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 7f275a09-f118-41c9-88d1-8de52d6a5aa1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/localization
ms.openlocfilehash: 85a192bf0b2eb245ecdaaa8ffa1c8dd2f43b45b0
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="globalization-and-localization-in-aspnet-core"></a>全球化和當地語系化的 ASP.NET Core

由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Damien Bowden](https://twitter.com/damien_bod)， [Bart Calixto](https://twitter.com/bartmax)， [Nadeem Afana](https://twitter.com/NadeemAfana)，和[Hisham Bin Ateya](https://twitter.com/hishambinateya)

與 ASP.NET Core 建立多國語言的網站，可讓您的網站才能達到更多觀眾。 ASP.NET Core 提供服務和中介軟體將當地語系化成不同的語言和文化特性。

國際化牽涉到[全球化](https://docs.microsoft.com/dotnet/api/system.globalization)和[當地語系化](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization)。 全球化是設計支援不同的文化特性的應用程式的程序。 全球化加入支援輸入、 顯示和一組定義的特定地理區域與相關聯的語言指令碼的輸出。

當地語系化是調整您已針對特定文化特性/地區設定可當地語系化的全球化應用程式的程序。  如需詳細資訊，請參閱**全球化和當地語系化條款**這份文件結尾附近。

應用程式當地語系化涉及下列作業：

1. 讓應用程式的內容可當地語系化

2. 提供的語言和您所支援的文化特性的當地語系化的資源

3. 選取每個要求的語言/文化特性的策略實作

## <a name="make-the-apps-content-localizable"></a>讓應用程式的內容可當地語系化

ASP.NET Core 中導入`IStringLocalizer`和`IStringLocalizer<T>`已設計成開發當地語系化應用程式時提高生產力。 `IStringLocalizer`使用[ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager)和[ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader)在執行階段提供特定文化特性的資源。 簡單的介面有索引子和`IEnumerable`傳回當地語系化的字串。 `IStringLocalizer`不需要您將預設語言字串儲存在資源檔。 您可以開發應用程式當地語系化為目標，並不需要在開發的早期建立資源檔。 下列程式碼會示範如何包裝當地語系化的字串"有關 Title"。

[!code-csharp[Main](localization/sample/Localization/Controllers/AboutController.cs)]

在上述程式碼中`IStringLocalizer<T>`實作來自[相依性插入](dependency-injection.md)。 如果找不到當地語系化"有關 Title"的值，則索引子機碼會傳回，也就是"有關 Title"的字串。 您可以保留預設的應用程式中的語言常值字串，並將其包裝在定位器，您可以專注於開發應用程式。 使用您的預設語言進行開發您的應用程式，並做好當地語系化步驟中沒有先建立預設資源檔。 或者，您可以使用的傳統方法，並提供要擷取的預設語言字串索引鍵。 許多開發人員將新的工作流程不會有預設語言的*.resx*檔案和簡單地包裝字串常值可以降低當地語系化應用程式的額外負荷。 其他開發人員會偏好傳統的工作流程，因為它可以讓您更輕鬆地使用較長的字串常值和更輕鬆地更新的當地語系化的字串。

使用`IHtmlLocalizer<T>`包含 HTML 資源的實作。 `IHtmlLocalizer`HTML 編碼格式的資源字串，但不是資源字串的引數。 在此範例中反白顯示的值以下`name`參數是 HTML 編碼。

[!code-csharp[Main](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

注意： 您通常想要只當地語系化文字和沒有 HTML。

最低層級，您可以取得`IStringLocalizerFactory`超出[相依性插入](dependency-injection.md):

[!code-csharp[Main](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

上述程式碼示範兩個處理站的每個建立方法。

您可以控制站，區域中，依分割當地語系化的字串，或具有一個容器。 範例應用程式中的虛擬類別名稱為`SharedResource`用於共用資源。

[!code-csharp[Main](localization/sample/Localization/Resources/SharedResource.cs)]

有些開發人員使用`Startup`類別可以包含全域或共用的字串。  在下列範例，`InfoController`和`SharedResource`當地語系化人員使用：

[!code-csharp[Main](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>檢視當地語系化

`IViewLocalizer`服務提供的當地語系化的字串[檢視](https://docs.microsoft.com/aspnet/core)。 `ViewLocalizer`類別會實作這個介面，並尋找檢視的檔案路徑的資源位置。 下列程式碼示範如何使用的預設實作`IViewLocalizer`:

[!code-HTML[Main](localization/sample/Localization/Views/Home/About.cshtml)]

預設實作`IViewLocalizer`尋找檢視的檔案名稱為基礎的資源檔。 沒有任何使用共用的全域資源檔案的選項。 `ViewLocalizer`實作使用當地語系化`IHtmlLocalizer`，因此 Razor 不 HTML 編碼的當地語系化的字串。 您可以參數化資源字串和`IViewLocalizer`將 HTML 編碼的參數，但不是資源字串。 請考慮下列 Razor 標記：

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

法文資源檔可以包含下列內容：

| Key | 值 |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

呈現的檢視可能包含從資源檔的 HTML 標記。

附註：
- 檢視當地語系化需要 「 Localization.AspNetCore.TagHelpers"NuGet 封裝。
- 您通常想要只當地語系化文字和沒有 HTML。

若要使用的共用的資源檔案，在檢視中，插入`IHtmlLocalizer<T>`:

[!code-HTML[Main](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>DataAnnotations 當地語系化

DataAnnotations 錯誤訊息會翻與`IStringLocalizer<T>`。 使用選項`ResourcesPath = "Resources"`，錯誤訊息中`RegisterViewModel`可以儲存在下列路徑：

* Resources/ViewModels.Account.RegisterViewModel.fr.resx
* Resources/ViewModels/Account/RegisterViewModel.fr.resx

[!code-csharp[Main](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

在 ASP.NET Core MVC 1.1.0 和更高版本、 非驗證屬性會當地語系化。 ASP.NET Core MVC 1.0 未**不**查閱 非驗證屬性的當地語系化字串。

<a name=one-resource-string-multiple-classes></a>
### <a name="using-one-resource-string-for-multiple-classes"></a>多個類別使用一個資源字串

下列程式碼會示範如何使用資源字串，有多個類別的驗證屬性：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

在先前程式碼中，`SharedResource`是對應到 resx 類別儲存驗證訊息。 使用這個方法，DataAnnotations 只會使用`SharedResource`，而不是每個類別的資源。 

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>提供的語言和您所支援的文化特性的當地語系化的資源  

### <a name="supportedcultures-and-supporteduicultures"></a>SupportedCultures 和 SupportedUICultures

ASP.NET Core 可讓您指定兩個文化特性值，`SupportedCultures`和`SupportedUICultures`。 [CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo)物件`SupportedCultures`判斷文化特性相關的函數，例如日期、 時間、 數字和貨幣格式的結果。 `SupportedCultures`也會決定文字、 大小寫慣例和字串比較的排序順序。 請參閱[CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)如需詳細資訊，在伺服器取得文化特性的方式。 `SupportedUICultures`決定會轉譯為字串 (從*.resx*檔案) 依查閱[ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager)。 `ResourceManager`查閱所決定的特定文化特性的字串，只需`CurrentUICulture`。 在.NET 中的每個執行緒都`CurrentCulture`和`CurrentUICulture`物件。 ASP.NET Core 呈現文化特性相關函式時，會檢查這些值。 比方說，如果目前執行緒的文化特性設定為"EN-US"英文 (美國），`DateTime.Now.ToLongDateString()`顯示 「 星期四年 2 月 18，2016 年 」，但是如果`CurrentCulture`設定"ES-ES"（西班牙文，西班牙） 的輸出會是"jueves，18 febrero de-de 2016"。

## <a name="working-with-resource-files"></a>使用資源檔

資源檔是有用的機制，來從程式碼分隔可當地語系化的字串。 非預設語言的翻譯的字串會隔離*.resx*資源檔。 例如，您可能想要建立名為西班牙文的資源檔*Welcome.es.resx*包含翻譯的字串。 "es"是西班牙文語言程式碼。 若要在 Visual Studio 中建立此資源檔：

1. 在**方案總管 中**，其中將包含資源檔的資料夾上按一下滑鼠右鍵 >**新增** > **新項目**。

    ![巢狀的內容功能表： 在方案總管中，內容功能表已開啟的資源。 第二個內容功能表會開啟以供加入顯示反白顯示的新項目命令。](localization/_static/newi.png)

2. 在**搜尋已安裝的範本**，輸入 「 資源 」，並將檔案命名。

    ![[新增項目] 對話方塊](localization/_static/res.png)

3. 輸入索引鍵的值 （原生字串）**名稱**資料行和中的已翻譯的字串**值**資料行。

    ![名稱資料行中的 Hello 這個字與 值 欄位中的單字 Hola (Hello 西班牙文) Welcome.es.resx 檔案 （西班牙文的 歡迎使用資源檔）](localization/_static/hola.png)

    Visual Studio 會顯示*Welcome.es.resx*檔案。

    ![方案總管] 中顯示 [歡迎使用西班牙文 (es) 資源檔](localization/_static/se.png)

<a name="error"></a>

如果您使用 Visual Studio 2017 Preview 版本 15.3，您會取得資源編輯器中的錯誤指標。 移除*ResXFileCodeGenerator*值*自訂工具*屬性方格裡，若要避免這個錯誤訊息：

![Resx 編輯器](localization/_static/err.png)

或者，您可以忽略此錯誤。 我們希望的下一個版本中修正這個問題。

## <a name="resource-file-naming"></a>資源檔命名

資源是減去的組件名稱與其類別的完整類型名稱命名。 例如，法文資源在其主要組件是的專案中`LocalizationWebsite.Web.dll`類別`LocalizationWebsite.Web.Startup`就會命名為*Startup.fr.resx*。 類別資源`LocalizationWebsite.Web.Controllers.HomeController`就會命名為*Controllers.HomeController.fr.resx*。 如果您的目標的類別的命名空間不是相同的組件名稱必須完整類型名稱。 比方說，在範例專案的資源類型`ExtraNamespace.Tools`就會命名為*ExtraNamespace.Tools.fr.resx*。

在範例專案中，`ConfigureServices`方法會設定`ResourcesPath`至 [資源]，主控制器的法文資源檔之專案相對路徑，所以*Resources/Controllers.HomeController.fr.resx*。 或者，您可以使用資料夾來組織資源檔案。 主控制器，路徑就是*Resources/Controllers/HomeController.fr.resx*。 如果您不使用`ResourcesPath`選項， *.resx*檔案會放在專案的基底目錄。 資源檔`HomeController`就會命名為*Controllers.HomeController.fr.resx*。 使用點或路徑的命名慣例的選擇取決於您想要組織您的資源檔的方式。

| 資源名稱 | 點或路徑命名 |
| ------------   | ------------- |
| Resources/Controllers.HomeController.fr.resx | 點  |
| Resources/Controllers/HomeController.fr.resx  | 路徑 |
|    |     |

使用的資源檔`@inject IViewLocalizer`Razor 檢視中，請遵循類似的模式。 如需檢視的資源檔可以使用點命名或路徑命名命名。 Razor 檢視資源檔模仿其相關聯的檢視檔案的路徑。 假設我們設定`ResourcesPath`「 資源 」，法文資源檔相關聯*Views/Home/About.cshtml*檢視可能是下列其中一項：

* Resources/Views/Home/About.fr.resx

* Resources/Views.Home.About.fr.resx

如果您不使用`ResourcesPath`選項， *.resx*檔案檢視會位於相同的資料夾檢視。

如果您移除 「.fr 」 文化特性指示項，且您必須設定為 「 法文 （透過 cookie 或其他機制） 的文化特性，在讀取預設資源檔，以及字串已當地語系化。 資源管理員會指定預設值或後援資源，不符合您要求的文化特性，您要提供文化特性指示項不 *.resx 檔案時。 如果您想要針對您的要求文化特性缺少資源，必須沒有預設資源檔時，只傳回索引鍵。

### <a name="generating-resource-files-with-visual-studio"></a>使用 Visual Studio 產生的資源檔

如果在 Visual Studio 中建立資源檔，而不是檔案名稱中的文化特性 (例如， *Welcome.resx*)，Visual Studio 會針對每個字串的屬性建立 C# 類別。 這通常不是您想要使用 ASP.NET Core;您通常不會有預設值*.resx*資源檔 (A *.resx*沒有的文化特性名稱的檔案)。 我們建議您建立*.resx*文化特性名稱的檔案 (例如*Welcome.fr.resx*)。 當您建立*.resx*文化特性名稱，Visual Studio 檔案將不會產生類別檔案。 我們預期的許多開發人員**不**建立預設語言資源檔案。

### <a name="adding-other-cultures"></a>加入其他文化特性

每個語言和文化特性的組合 （非預設的語言） 需要唯一的資源檔。 您藉由建立新的資源檔中的 ISO 語言代碼是檔案名稱的一部分建立不同的文化特性與地區設定的資源檔 (例如， **en-我們**， **fr ca**，和**en gb**)。 這些 ISO 碼放置於之間的檔案名稱和*.resx*副檔名，像是*Welcome.es MX.resx* （墨西哥西班牙文/）。 若要指定的文化特性中性語言，移除 國家/地區的程式碼 (`MX`在上述範例中)。 西班牙文的文化特性中性資源檔案名稱是*Welcome.es.resx*。

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>選取每個要求的語言/文化特性的策略實作  

### <a name="configuring-localization"></a>設定當地語系化

設定當地語系化`ConfigureServices`方法：

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet1)]

* `AddLocalization`將當地語系化服務加入至服務容器。 上方的程式碼也會設定 「 資源 」 的資源路徑。

* `AddViewLocalization`新增支援當地語系化的檢視檔案。 在此範例檢視當地語系化為基礎的檢視檔案後置詞。 例如"fr"中的*Index.fr.cshtml*檔案。

* `AddDataAnnotationsLocalization`新增支援當地語系化`DataAnnotations`驗證訊息透過`IStringLocalizer`抽象概念。

### <a name="localization-middleware"></a>當地語系化的中介軟體

在要求上目前的文化特性設定當地語系化的[中介軟體](middleware.md)。 當地語系化的中介軟體中已啟用`Configure`方法*Program.cs*檔案。 請注意，必須設定當地語系化的中介軟體，可能會檢查要求文化特性任何中介軟體之前 (例如， `app.UseMvcWithDefaultRoute()`)。

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet2)]

`UseRequestLocalization`初始化`RequestLocalizationOptions`物件。 在每次要求清單的`RequestCultureProvider`中`RequestLocalizationOptions`列舉並用可以成功地決定要求文化特性的第一個提供者。 預設的提供者來自`RequestLocalizationOptions`類別：

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

預設清單會從最特定至最不特定。 在本文稍後我們會看到如何變更順序和甚至加入自訂文化特性的提供者。 如果沒有提供者可以判斷要求的文化特性，`DefaultRequestCulture`用。

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

某些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。 針對使用 cookie 或 Accept-encoding 標頭方法的應用程式，將查詢字串新增至 URL 是用於偵錯和測試程式碼。 根據預設，`QueryStringRequestCultureProvider`登錄中的第一個當地語系化提供者為`RequestCultureProvider`清單。 您將查詢字串參數`culture`和`ui-culture`。 下列範例會設定要每次墨西哥的西班牙文 （語言和地區） 的特定文化特性：

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

如果您只傳入以其中兩個 (`culture`或`ui-culture`)，查詢字串提供者會設定使用了傳入的兩個值。 例如，設定的文化特性會同時設定`Culture`和`UICulture`:

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

實際執行應用程式通常會提供一個機制來設定與 ASP.NET Core 文化特性 cookie 文化特性。 使用`MakeCookieValue`方法來建立 cookie。

`CookieRequestCultureProvider` `DefaultCookieName`傳回用來追蹤使用者的預設 cookie 名稱的慣用文化特性資訊。 預設的 cookie 名稱是"。AspNetCore.Culture"。

Cookie 格式是`c=%LANGCODE%|uic=%LANGCODE%`，其中`c`是`Culture`和`uic`是`UICulture`，例如：

    c=en-UK|uic=en-US

如果您只指定文化特性資訊和 UI 文化特性的其中一個，指定的文化特性將使用的文化特性資訊和 UI 文化特性。

### <a name="the-accept-language-http-header"></a>Accept-encoding HTTP 標頭

[Accept-encoding 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)是可在大部分的瀏覽器設定和原本要用來指定使用者的語言。 這個設定會指出瀏覽器功能已設定為傳送或已繼承自基礎作業系統。 從瀏覽器要求的接受語言 HTTP 標頭不是百分之百的方法，可偵測使用者的慣用的語言 (請參閱[瀏覽器中設定語言喜好設定](https://www.w3.org/International/questions/qa-lang-priorities.en.php))。 在生產應用程式應該包含方法，讓使用者可以自訂自己選擇的文化特性。

### <a name="setting-the-accept-language-http-header-in-ie"></a>在 IE 中設定 Accept-encoding HTTP 標頭

1. 從齒輪圖示，點選**網際網路選項**。

2. 點選**語言**。

    ![網際網路選項](localization/_static/lang.png)

3. 點選**設定語言喜好設定**。

4. 點選**新增語言**。

5. 新增的語言。

6. 點選語言，然後點選 **移**。

### <a name="using-a-custom-provider"></a>使用自訂提供者

假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫。 您可以撰寫查詢使用者，這些值提供者。 下列程式碼會示範如何加入自訂提供者：

```csharp
private const string enUSCulture = "en-US";

services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo(enUSCulture),
        new CultureInfo("fr")
    };

    options.DefaultRequestCulture = new RequestCulture(culture: enUSCulture, uiCulture: enUSCulture);
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;

    options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

使用`RequestLocalizationOptions`新增或移除當地語系化提供者。

### <a name="setting-the-culture-programmatically"></a>以程式設計方式設定文化特性

這個範例**Localization.StarterWeb**投影上[GitHub](https://github.com/aspnet/entropy)包含設定的 UI `Culture`。 *Views/Shared/_SelectLanguagePartial.cshtml*檔可讓您從支援的文化特性的清單中選取的文化特性：

[!code-HTML[Main](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

*Views/Shared/_SelectLanguagePartial.cshtml*檔案加入至`footer`區段的配置檔案，因此將予以提供至所有的檢視：

[!code-HTML[Main](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

`SetLanguage`方法設定的文化特性的 cookie。

[!code-csharp[Main](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

您無法插入*_SelectLanguagePartial.cshtml*這個專案的範例程式碼。 **Localization.StarterWeb**投影上[GitHub](https://github.com/aspnet/entropy)有程式碼流向`RequestLocalizationOptions`來透過部分 Razor[相依性插入](dependency-injection.md)容器。

## <a name="globalization-and-localization-terms"></a>全球化和當地語系化之條款

當地語系化您的應用程式的程序也需要相關的字元集中的現代軟體開發中常用的基本了解並了解與它們相關聯的問題。 雖然所有的電腦會將文字儲存為數字 （程式碼），不同的系統存放區使用不同的數字的相同文字。 轉譯特定的文化特性/地區設定的應用程式使用者介面 (UI) 是指在當地語系化過程。

[當地語系化能力](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review)會確認全球化應用程式已準備好進行當地語系化的中繼程序。

[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt)格式的文化特性名稱是"<languagecode2>-< country/regioncode2 >"，其中<languagecode2>的語言代碼，而 < country/regioncode2 > 子文化特性代碼。 例如，`es-CL`西班牙文 （智利），`en-US`英文 （美國） 和`en-AU`英文 （澳大利亞）。 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt)是兩個字母小寫文化特性代碼相關聯的語言的 ISO 639 和兩個字母的大寫子程式碼相關聯的國家或地區的 ISO 3166 的組合。  請參閱[語言文化特性名稱](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx)。

國際化通常縮寫為"I18N"。 縮寫採用第一個和最後一個字母和字母它們，因此 18 之間的數字代表數目之間的第一個字母"I"及最後"N"。 同樣適用於全球化 (G11N)，與當地語系化 (L10N)。

詞彙：

* 全球化 (G11N): 程序可確保應用程式支援不同語言和區域。
* 當地語系化 (L10N): 自訂特定的語言和地區的應用程式的過程。
* 國際化 (I18N): 描述全球化和當地語系化。
* 文化特性： 它是一種語言和 （選擇性） 地區。
* 中性文化特性： 具有指定的語言，而不是地區的文化特性。 (例如"en"，"es")
* 特定文化特性： 具有指定的語言和地區的文化特性。 （適用於範例"EN-US"、"EN-GB"、"es CL"）
* 地區設定： 地區設定是文化特性相同。

## <a name="additional-resources"></a>其他資源

* [Localization.StarterWeb 專案](https://github.com/aspnet/entropy)用於發行項。
* [Visual Studio 中的資源檔](https://docs.microsoft.com/cpp/windows/resource-files-visual-studio)
* [.Resx 檔中的資源](https://docs.microsoft.com/dotnet/framework/resources/working-with-resx-files-programmatically)