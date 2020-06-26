---
title: ASP.NET Core 全球化和當地語系化
author: rick-anderson
description: 了解 ASP.NET Core 如何提供服務與中介軟體，以將內容當地語系化成不同的語言與文化特性。
ms.author: riande
ms.date: 11/30/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/localization
ms.openlocfilehash: cc30cedd51af06ffc7e17d36d4426fa45c452015
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85407742"
---
# <a name="globalization-and-localization-in-aspnet-core"></a>ASP.NET Core 全球化和當地語系化

::: moniker range=">= aspnetcore-3.0 < aspnetcore-5.0"

由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Damien Bowden](https://twitter.com/damien_bod)、[Bart Calixto](https://twitter.com/bartmax)、[Nadeem Afana](https://afana.me/) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya) 提供

多語系網站可讓網站觸及更多物件。 ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。

國際化包含[全球化](/dotnet/api/system.globalization)和[當地語系化](/dotnet/standard/globalization-localization/localization)這兩部分。 全球化是指設計出可支援不同文化特性之應用程式的程序。 透過全球化，您可新增支援與特定地區相關之已定義語言指令碼的輸入、顯示及輸出作業。

當地語系化是指針對全球化應用程式進行調整的程序，且您已順應特定文化特性/地區設定對這些全球化應用程式進行可當地語系化處理。 如需詳細資訊，請參閱本文件結尾處的**全球化和當地語系化詞彙**。

應用程式當地語系化包含下列作業：

1. 讓應用程式的內容可當地語系化
1. 針對您支援的語言和文化特性提供當地語系化資源
1. 實作可依據每項要求選取語言/文化特性的策略

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/3.x/Localization)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="make-the-apps-content-localizable"></a>讓應用程式的內容可當地語系化

<xref:Microsoft.Extensions.Localization.IStringLocalizer>和 <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> 已架構，可在開發當地語系化應用程式時改善生產力。 `IStringLocalizer`會使用 <xref:System.Resources.ResourceManager> 和， <xref:System.Resources.ResourceReader> 在執行時間提供特定文化特性的資源。 介面具有索引子和， `IEnumerable` 用於傳回當地語系化的字串。 `IStringLocalizer`不需要將預設語言字串儲存在資源檔中。 您不必在開發初期建立資源檔，即可開發以當地語系化為目標的應用程式。 下列程式碼會示範如何包裝 "About Title" 字串以進行當地語系化。

[!code-csharp[](localization/sample/3.x/Localization/Controllers/AboutController.cs)]

在上述程式碼中， `IStringLocalizer<T>` 執行是來自相依性[插入](dependency-injection.md)。 如果找不到 "About Title" 的當地語系化值，即會傳回索引子的索引鍵，也就是 "About Title" 字串。 您可以保留應用程式中的預設語言常值字串，並將其包裝在當地語系化工具中，以便專注於開發應用程式。 您不用先建立預設資源檔，即可使用預設語言來開發應用程式，並針對當地語系化步驟進行應用程式的準備。 或者，您可以使用傳統方法，並提供索引鍵以擷取預設語言字串。 對許多開發人員來說，新的工作流程 (單純包裝字串常值而不使用預設語言的 *.resx* 檔案) 可以降低當地語系化應用程式的額外負荷。 其他開發人員則偏好傳統的工作流程，因為這種方法更便於使用較長的字串常值，且更易於更新當地語系化的字串。

若是包含 HTML 的資源，請使用 `IHtmlLocalizer<T>` 實作。 `IHtmlLocalizer` 會對資源字串中經過格式化的引數進行 HTML 編碼，但不會對資源字串本身進行 HTML 編碼。 在下列醒目提示的範例中，只有 `name` 參數的值經過 HTML 編碼。

[!code-csharp[](~/fundamentals/localization/sample/3.x/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。

您可以在最底層的[相依性插入](dependency-injection.md)中，將 `IStringLocalizerFactory` 移出：

[!code-csharp[](localization/sample/3.x/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

上述程式碼會示範這兩個 Factory Create 方法。

您可以依據控制器或區域來分割當地語系化的字串，也可以只用一個容器。 在範例應用程式中，會針對共用資源使用名為 `SharedResource` 的虛擬類別。

[!code-csharp[](localization/sample/3.x/Localization/Resources/SharedResource.cs)]

有些開發人員會使用 `Startup` 類別來包含全域或共用字串。 下列範例會使用 `InfoController` 和 `SharedResource` 當地語系化工具：

[!code-csharp[](localization/sample/3.x/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>檢視當地語系化

`IViewLocalizer` 服務可提供[檢視](xref:mvc/views/overview)的當地語系化字串。 `ViewLocalizer` 類別會實作這個介面，並透過檢視檔案路徑來找出資源的位置。 下列程式碼示範如何使用 `IViewLocalizer` 的預設實作：

[!code-cshtml[](localization/sample/3.x/Localization/Views/Home/About.cshtml)]

`IViewLocalizer` 的預設實作會依據檢視的檔案名稱來找出資源檔。 其中並沒有任何選項可以使用全域共用的資源檔。 `ViewLocalizer`使用來執行當地語系化工具 `IHtmlLocalizer` ，因此 Razor 不會對當地語系化字串進行 HTML 編碼。 您可以參數化資源字串，`IViewLocalizer` 即會對參數 (而不是資源字串) 進行 HTML 編碼。 請考慮下列 Razor 標記：

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

法文資源檔可能包含下列內容：

| Key | 值 |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |

轉譯的檢視內容可能包含來自資源檔的 HTML 標記。

**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。

若要在檢視中使用共用的資源檔，請插入 `IHtmlLocalizer<T>`：

[!code-cshtml[](~/fundamentals/localization/sample/3.x/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>DataAnnotations 當地語系化

DataAnnotations 錯誤訊息會使用 `IStringLocalizer<T>` 來當地語系化。 使用 `ResourcesPath = "Resources"` 選項時，`RegisterViewModel` 中的錯誤訊息會儲存在下列路徑之一：

* *Resources/Viewmodel. RegisterViewModel. fr .resx*
* *Resources/ViewModels/Account/RegisterViewModel.fr.resx*

[!code-csharp[](localization/sample/3.x/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

在 ASP.NET Core MVC 1.1.0 和更高版本中，系統會將非驗證屬性當地語系化。 ASP.NET Core MVC 1.0 **不會**查閱非驗證屬性的當地語系化字串。

<a name="one-resource-string-multiple-classes"></a>

### <a name="using-one-resource-string-for-multiple-classes"></a>針對多個類別使用同一個資源字串

下列程式碼會示範如何針對含有多個類別的驗證屬性使用同一個資源字串：

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

在先前的程式碼中，`SharedResource` 是與儲存驗證訊息的 resx 對應的類別。 使用這個方法時，DataAnnotations 只會使用 `SharedResource`，而不是每個類別的資源。

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>針對您支援的語言和文化特性提供當地語系化資源

### <a name="supportedcultures-and-supporteduicultures"></a>SupportedCultures 和 SupportedUICultures

ASP.NET Core 可讓您指定 `SupportedCultures` 和 `SupportedUICultures` 這兩個文化特性值。 `SupportedCultures` 的 [CultureInfo](/dotnet/api/system.globalization.cultureinfo) 物件可決定文化特性相依函式的結果，例如日期、時間、數字及貨幣格式。 `SupportedCultures` 也可決定文字排列順序、大小寫慣例和字串比較。 如需伺服器如何取得文化特性的詳細資訊，請參閱 [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)。 `SupportedUICultures`會決定[ResourceManager](/dotnet/api/system.resources.resourcemanager)會查閱哪些翻譯的字串（來自 *.resx*檔案）。 `ResourceManager` 僅會查閱 `CurrentUICulture` 所決定之文化特性特有的字串。 .NET 中的每個執行緒都有 `CurrentCulture` 和 `CurrentUICulture` 物件。 ASP.NET Core 會在轉譯文化特性相依函式時檢查這些值。 比方說，如果目前執行緒的文化特性設定為 "en-US" (英文 - 美國)，`DateTime.Now.ToLongDateString()` 會顯示 "Thursday, February 18, 2016"，但如果 `CurrentCulture` 設定為 "es-ES" (西班牙文 - 西班牙)，則輸出會是 "jueves, 18 de febrero de 2016"。

## <a name="resource-files"></a>資源檔

資源檔是一種實用的機制，可讓您將可當地語系化的字串與代碼區隔開來。 非預設語言的翻譯字串會在 *.resx*資源檔中隔離。 例如，您可以建立名為 *Welcome.es.resx* 的西班牙文資源檔，以包含翻譯的字串。 "es" 是西班牙文的語言代碼。 若要在 Visual Studio 中建立這個資源檔：

1. 在方案總管**** 中，以滑鼠右鍵按一下要放置資源檔的資料夾 > [新增]**[新增項目]** > ****。

    ![巢狀特色選單：方案總管會開啟 [資源] 的特色選單， 接著針對 [新增] 開啟第二個特色選單，並反白顯示 [新增項目] 命令。](localization/_static/newi.png)

2. 在 [Search installed templates] (搜尋已安裝的範本)**** 方塊中，輸入「資源」，並命名檔案。

    ![[新增項目] 對話方塊](localization/_static/res.png)

3. 在 [名稱]**** 資料行中輸入索引鍵值 (原生字串)，並在 [值]**** 資料行中輸入已翻譯的字串。

    ![Welcome.es.resx 檔案 (西班牙文的「歡迎使用」資源檔)，其中 [名稱] 資料行的文字為 Hello，而 [值] 資料行的文字為 Hola (Hello 的西班牙文)](localization/_static/hola.png)

    Visual Studio 會顯示 *Welcome.es.resx* 檔案。

    ![方案總管，其中顯示「歡迎使用」的西班牙文 (es) 資源檔](localization/_static/se.png)

## <a name="resource-file-naming"></a>資源檔命名

資源的命名方式是以其類別的完整類型名稱去掉組件名稱而得。 例如，假設專案中的法文資源是 `LocalizationWebsite.Web.Startup` 類別、主要組件為 `LocalizationWebsite.Web.dll`，就會命名為 *Startup.fr.resx*。 若是 `LocalizationWebsite.Web.Controllers.HomeController` 類別的資源，則應命名為 *Controllers.HomeController.fr.resx*。 如果目標類別的命名空間和組件名稱不相同，則需要使用完整類型名稱。 比方說，範例專案中 `ExtraNamespace.Tools` 類型的資源會命名為 *ExtraNamespace.Tools.fr.resx*。

在範例專案中，`ConfigureServices` 方法會將 `ResourcesPath` 設為 "Resources"，因此首頁控制器的法文資源檔專案相對路徑即為 *Resources/Controllers.HomeController.fr.resx*。 或者，您可以使用資料夾來收集資源檔。 若是首頁控制器，路徑就是 *Resources/Controllers/HomeController.fr.resx*。 如果您不使用 `ResourcesPath` 選項，*.resx* 檔案即會放置在專案的基底目錄中。 `HomeController` 的資源檔會命名為 *Controllers.HomeController.fr.resx*。 您可依據自己的資源檔收集方式，來選擇要使用點或路徑的命名慣例。

| 資源名稱 | 點或路徑命名 |
| ------------   | ------------- |
| Resources/Controllers.HomeController.fr.resx | 點  |
| Resources/Controllers/HomeController.fr.resx  | 路徑 |
|    |     |

在 views 中使用的資源檔會 `@inject IViewLocalizer` Razor 遵循類似的模式。 您可以使用點命名或路徑命名方式，來命名檢視的資源檔。 Razor查看資源檔模擬其相關聯之視圖檔案的路徑。 假設我們將 `ResourcesPath` 設為 "Resources"，與 *Views/Home/About.cshtml* 檢視建立關聯的法文資源檔可為下列其一：

* Resources/Views/Home/About.fr.resx

* Resources/Views.Home.About.fr.resx

如果您不使用 `ResourcesPath` 選項，則檢視的 *.resx* 檔案會與檢視位於相同資料夾中。

### <a name="rootnamespaceattribute"></a>RootNamespaceAttribute 

[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) 屬性會在組件的根命名空間與組件名稱不同時，提供組件的根命名空間。 

> [!WARNING]
> 當專案的名稱不是有效的 .NET 識別碼時，就可能發生這種情況。 例如， `my-project-name.csproj` 會使用根命名空間 `my_project_name` ，以及 `my-project-name` 導致此錯誤的元件名稱。 

如果組件的根命名空間與組件名稱不同：

* 根據預設，當地語系化無法運作。
* 在組件中搜尋資源的方式造成當地語系化失敗。 `RootNamespace` 是建置時間值，無法用於執行處理序。 

如果 `RootNamespace` 與 `AssemblyName` 不同，請將下列內容納入 *AssemblyInfo.cs* (參數值取代為實際值)：

```csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

上述程式碼可成功解析 resx 檔案。

## <a name="culture-fallback-behavior"></a>文化特性後援行為

搜尋資源時，當地語系化會使用「文化特性後援」。 從所要求的文化特性開始，如果找不到，就會還原成該文化特性的父文化特性。 另外，[CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) 屬性代表父文化特性。 這通常 (但並非一定) 表示從 ISO 中移除國家意符 (Signifier)。 例如，墨西哥的西班牙文方言是 "es-MX"。 它具有父系 "es" &mdash; 西班牙文不是任何國家/地區的特定項目。

假設您的網站收到使用文化特性 "fr-CA" 之 "Welcome" 資源的要求。 當地語系化系統會依照順序尋找下列資源，並選取第一個相符項目：

* *Welcome.fr-CA.resx*
* *Welcome.fr.resx*
* *Welcome.resx* (如果 `NeutralResourcesLanguage` 是 "fr-CA")

舉例來說，如果您移除 ".fr" 文化特性指示項，並將文化特性設定為法文，則系統會讀取預設資源檔，並將字串當地語系化。 當沒有任何項目符合您要求的文化特性時，資源管理員即會指定預設資源或後援資源。 如果您只想在要求的文化特性缺少資源時傳回索引鍵，就不能使用預設資源檔。

### <a name="generate-resource-files-with-visual-studio"></a>使用 Visual Studio 產生資源檔

如果您在 Visual Studio 中建立資源檔，但檔案名稱中不含文化特性 (例如 *Welcome.resx*)，則 Visual Studio 會針對每個字串的屬性建立 C# 類別。 但這通常不是您使用 ASP.NET Core 的初衷。 一般來說，您不會有預設的 *.resx* 資源檔 (不含文化特性名稱的 *.resx* 檔案)。 因此，建議您建立含有文化特性名稱的 *.resx* 檔案 (例如 *Welcome.fr.resx*)。 當您建立含有文化特性名稱的 *.resx* 檔案時，Visual Studio 就不會產生類別檔案。

### <a name="add-other-cultures"></a>新增其他文化特性

每種語言和文化特性的組合 (非預設語言) 都需要唯一的資源檔。 若要建立不同文化特性和地區設定的資源檔，您可以建立新的資源檔並將 ISO 語言代碼作為檔名的一部分 (例如 **en-us**、**fr-ca** 和 **en-gb**)。 您應將 ISO 代碼置於檔案名稱和 *.resx* 副檔名之間，例如 *Welcome.es-MX.resx* (西班牙文/墨西哥)。

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>實作可依據每項要求選取語言/文化特性的策略

### <a name="configure-localization"></a>設定當地語系化

您可以在 `Startup.ConfigureServices` 方法中設定當地語系化：

[!code-csharp[](localization/sample/3.x/Localization/Startup.cs?name=snippet1)]

* `AddLocalization` 可將當地語系化服務新增至服務容器。 上方的程式碼也會將資源路徑設為 "Resources"。

* `AddViewLocalization` 可支援當地語系化的檢視檔案。 在此範例中，檢視的當地語系化會以檢視檔案的後置詞為依據。 例如 *Index.fr.cshtml* 檔案中的 "fr"。

* `AddDataAnnotationsLocalization` 可支援透過 `IStringLocalizer` 抽象概念而來的當地語系化 `DataAnnotations` 驗證訊息。

### <a name="localization-middleware"></a>當地語系化中介軟體

您可以在當地語系化[中介軟體](xref:fundamentals/middleware/index)中，設定要求目前的文化特性。 已在 `Startup.Configure` 方法中啟用當地語系化中介軟體。 您必須在可能檢查要求文化特性的任何中介軟體之前，設定當地語系化中介軟體 (例如 `app.UseMvcWithDefaultRoute()`)。

[!code-csharp[](localization/sample/3.x/Localization/Startup.cs?name=snippet2)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

`UseRequestLocalization` 會初始化 `RequestLocalizationOptions` 物件。 在每次要求時，系統會列舉 `RequestLocalizationOptions` 中的 `RequestCultureProvider` 清單，並使用能成功判斷要求的文化特性的第一個提供者。 預設的提供者是來自 `RequestLocalizationOptions` 類別：

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

預設清單會由針對性高到低來排列。 在本文稍後，我們會說明如何變更順序，甚至新增自訂的文化特性提供者。 如果沒有任何提供者可以判斷要求的文化特性，即會使用 `DefaultRequestCulture`。

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

有些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。 若是使用 Cookie 或 Accept-Language 標頭方法的應用程式，您可以將查詢字串新增至 URL 以偵錯和測試程式碼。 系統預設會將 `QueryStringRequestCultureProvider` 登錄為 `RequestCultureProvider` 清單中的第一個當地語系化提供者。 您應傳遞查詢字串參數 `culture` 和 `ui-culture`。 下列範例會設定西班牙文/墨西哥的特定文化特性 (語言和地區)：

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

如果您只傳入兩者其一 (`culture` 或 `ui-culture`)，查詢字串提供者就會使用您傳入的項目來設定這兩個值。 例如，若只設定文化特性，即會同時設定 `Culture` 和 `UICulture`：

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

生產環境應用程式通常會提供一個機制，來設定 ASP.NET Core 文化特性 Cookie 的文化特性。 若要建立 Cookie，請使用 `MakeCookieValue` 方法。

會傳回 `CookieRequestCultureProvider` `DefaultCookieName` 用來追蹤使用者慣用文化特性資訊的預設 cookie 名稱。 預設 Cookie 名稱為 `.AspNetCore.Culture`。

Cookie 格式為 `c=%LANGCODE%|uic=%LANGCODE%`，其中 `c` 是 `Culture` 而 `uic` 是 `UICulture`，例如：

    c=en-UK|uic=en-US

如果您只指定文化特性資訊和 UI 文化特性其一，系統就會將您所指定的文化特性用於文化特性資訊和 UI 文化特性。

### <a name="the-accept-language-http-header"></a>Accept-Language HTTP 標頭

您可在大多數的瀏覽器中設定 [Accept-Language 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)，其最初的設計目的是用來指定使用者的語言。 這項設定可指出瀏覽器已設好要傳送哪些項目，或已從基礎作業系統繼承哪些項目。 透過瀏覽器要求的 Accept-Language HTTP 標頭來偵測使用者的慣用語言，並非萬無一失 (請參閱 [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php) (在瀏覽器中設定語言喜好設定)。 生產環境應用程式應該包含可讓使用者自訂文化特性的選擇方式。

### <a name="set-the-accept-language-http-header-in-ie"></a>在 IE 中設定 Accept-Language HTTP 標頭

1. 從齒輪圖示，點選 [網際網路選項]****。

2. 點選 [語言]****。

    ![網際網路選項](localization/_static/lang.png)

3. 點選 [設定語言喜好設定]****。

4. 點選 [新增語言]****。

5. 新增語言。

6. 點選語言，然後點選 [上移]****。

### <a name="use-a-custom-provider"></a>使用自訂提供者

假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫中。 您可以撰寫提供者，以供使用者查詢這些值。 下列程式碼會示範如何新增自訂提供者：

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

    options.AddInitialRequestCultureProvider(new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

使用 `RequestLocalizationOptions` 新增或移除當地語系化提供者。

### <a name="set-the-culture-programmatically"></a>以程式設計方式來設定文化特性

[GitHub](https://github.com/aspnet/entropy) 上的這個範例 **Localization.StarterWeb** 專案包含可設定 `Culture` 的 UI。 *Views/Shared/_SelectLanguagePartial.cshtml* 檔可讓您從支援的文化特性清單中選取文化特性：

[!code-cshtml[](localization/sample/3.x/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

系統會將 *Views/Shared/_SelectLanguagePartial.cshtml* 檔案新增至配置檔案的 `footer` 區段，以供所有檢視使用：

[!code-cshtml[](localization/sample/3.x/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

`SetLanguage` 方法會設定文化特性的 Cookie。

[!code-csharp[](localization/sample/3.x/Localization/Controllers/HomeController.cs?range=57-67)]

您無法將 *_SelectLanguagePartial.cshtml* 插入這個專案的範例程式碼。 [GitHub](https://github.com/aspnet/entropy)上的**localization.starterweb**專案具有程式碼，可透過相依性 `RequestLocalizationOptions` 插入容器將傳遞至 Razor 部分。 [Dependency Injection](dependency-injection.md)

## <a name="model-binding-route-data-and-query-strings"></a>模型系結路由資料和查詢字串

請參閱模型系結[路由資料和查詢字串的全球化行為](xref:mvc/models/model-binding#glob)。

## <a name="globalization-and-localization-terms"></a>全球化和當地語系化詞彙

在進行應用程式的當地語系化程序時，您也需要具備現代軟體開發中常用的相關字元集基本知識，並了解與它們建立關聯的問題。 雖然所有電腦都會將文字儲存為數字 (程式碼)，但不同系統會使用不同數字來儲存相同的文字。 當地語系化是指針對特定文化特性/地區設定，轉譯應用程式使用者介面 (UI) 的程序。

[可當地語系化](/dotnet/standard/globalization-localization/localizability-review)是確認全球化應用程式已準備好進行當地語系化的中繼程序。

文化特性名稱的 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 格式是 `<languagecode2>-<country/regioncode2>`，其中 `<languagecode2>` 是語言代碼，而 `<country/regioncode2>` 是子文化特性代碼。 例如，`es-CL` 是指西班牙文 (智利)，`en-US` 是指英文 (美國)，而 `en-AU` 是指英文 (澳大利亞)。 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 是 ISO 639 (兩個小寫字母，為與某個語言建立關聯的文化特性代碼) 及 ISO 3166 (兩個大寫字母，為與某個國家或地區建立關聯的子文化特性代碼) 的組合。 請參閱 [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx) (語言的文化特性名稱)。

國際化通常縮寫為 "I18N"。 這個縮寫是採用該詞彙的第一個和最後一個字母，以及這兩個字母間的字母數組成，因此 18 代表第一個字母 "I" 及最後 "N" 中間的字母數。 同樣的原則也適用於全球化 (G11N) 與當地語系化 (L10N) 的縮寫。

詞彙：

* 全球化 (G11N)：讓應用程式支援不同語言和區域的程序。
* 當地語系化 (L10N)：針對特定語言和區域，自訂應用程式的程序。
* 國際化 (I18N)：包含全球化和當地語系化這兩部分的描述。
* 文化特性：它是一種語言和/或地區。
* 中性文化特性：具有指定的語言但不限地區的文化特性  (例如 "en"、"es")。
* 特定文化特性：具有指定的語言和地區的文化特性  (例如 "en-US"、"en-GB"、"es-CL")。
* 父文化特性：包含特定文化特性的中性文化特性  (例如，"en" 是 "en-US" 和 "en-GB" 的父文化特性)。
* 地區設定：地區設定與文化特性相同。

[!INCLUDE[](~/includes/localization/currency.md)]

[!INCLUDE[](~/includes/localization/unsupported-culture-log-level.md)]

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/troubleshoot-aspnet-core-localization>
* 本文使用的 [Localization.StarterWeb 專案](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb)。
* [全球化與當地語系化 .NET 應用程式](/dotnet/standard/globalization-localization/index)
* [.Resx 檔案中的資源](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [Microsoft 多語應用程式工具組](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [當地語系化和泛型](http://hishambinateya.com/localization-and-generics)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Damien Bowden](https://twitter.com/damien_bod)、[Bart Calixto](https://twitter.com/bartmax)、[Nadeem Afana](https://afana.me/) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya) 提供

多語系網站可讓網站觸及更多物件。 ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。

國際化包含[全球化](/dotnet/api/system.globalization)和[當地語系化](/dotnet/standard/globalization-localization/localization)這兩部分。 全球化是指設計出可支援不同文化特性之應用程式的程序。 透過全球化，您可新增支援與特定地區相關之已定義語言指令碼的輸入、顯示及輸出作業。

當地語系化是指針對全球化應用程式進行調整的程序，且您已順應特定文化特性/地區設定對這些全球化應用程式進行可當地語系化處理。 如需詳細資訊，請參閱本文件結尾處的**全球化和當地語系化詞彙**。

應用程式當地語系化包含下列作業：

1. 讓應用程式的內容可當地語系化
1. 針對您支援的語言和文化特性提供當地語系化資源
1. 實作可依據每項要求選取語言/文化特性的策略

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="make-the-apps-content-localizable"></a>讓應用程式的內容可當地語系化

<xref:Microsoft.Extensions.Localization.IStringLocalizer>和 <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> 已架構，可在開發當地語系化應用程式時改善生產力。 `IStringLocalizer`會使用 <xref:System.Resources.ResourceManager> 和， <xref:System.Resources.ResourceReader> 在執行時間提供特定文化特性的資源。 介面具有索引子和， `IEnumerable` 用於傳回當地語系化的字串。 `IStringLocalizer`不需要將預設語言字串儲存在資源檔中。 您不必在開發初期建立資源檔，即可開發以當地語系化為目標的應用程式。 下列程式碼會示範如何包裝 "About Title" 字串以進行當地語系化。

[!code-csharp[](localization/sample/3.x/Localization/Controllers/AboutController.cs)]

在上述程式碼中， `IStringLocalizer<T>` 執行是來自相依性[插入](dependency-injection.md)。 如果找不到 "About Title" 的當地語系化值，即會傳回索引子的索引鍵，也就是 "About Title" 字串。 您可以保留應用程式中的預設語言常值字串，並將其包裝在當地語系化工具中，以便專注於開發應用程式。 您不用先建立預設資源檔，即可使用預設語言來開發應用程式，並針對當地語系化步驟進行應用程式的準備。 或者，您可以使用傳統方法，並提供索引鍵以擷取預設語言字串。 對許多開發人員來說，新的工作流程 (單純包裝字串常值而不使用預設語言的 *.resx* 檔案) 可以降低當地語系化應用程式的額外負荷。 其他開發人員則偏好傳統的工作流程，因為這種方法更便於使用較長的字串常值，且更易於更新當地語系化的字串。

若是包含 HTML 的資源，請使用 `IHtmlLocalizer<T>` 實作。 `IHtmlLocalizer` 會對資源字串中經過格式化的引數進行 HTML 編碼，但不會對資源字串本身進行 HTML 編碼。 在下列醒目提示的範例中，只有 `name` 參數的值經過 HTML 編碼。

[!code-csharp[](~/fundamentals/localization/sample/3.x/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。

您可以在最底層的[相依性插入](dependency-injection.md)中，將 `IStringLocalizerFactory` 移出：

[!code-csharp[](localization/sample/3.x/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

上述程式碼會示範這兩個 Factory Create 方法。

您可以依據控制器或區域來分割當地語系化的字串，也可以只用一個容器。 在範例應用程式中，會針對共用資源使用名為 `SharedResource` 的虛擬類別。

[!code-csharp[](localization/sample/3.x/Localization/Resources/SharedResource.cs)]

有些開發人員會使用 `Startup` 類別來包含全域或共用字串。 下列範例會使用 `InfoController` 和 `SharedResource` 當地語系化工具：

[!code-csharp[](localization/sample/3.x/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>檢視當地語系化

`IViewLocalizer` 服務可提供[檢視](xref:mvc/views/overview)的當地語系化字串。 `ViewLocalizer` 類別會實作這個介面，並透過檢視檔案路徑來找出資源的位置。 下列程式碼示範如何使用 `IViewLocalizer` 的預設實作：

[!code-cshtml[](localization/sample/3.x/Localization/Views/Home/About.cshtml)]

`IViewLocalizer` 的預設實作會依據檢視的檔案名稱來找出資源檔。 其中並沒有任何選項可以使用全域共用的資源檔。 `ViewLocalizer`使用來執行當地語系化工具 `IHtmlLocalizer` ，因此 Razor 不會對當地語系化字串進行 HTML 編碼。 您可以參數化資源字串，`IViewLocalizer` 即會對參數 (而不是資源字串) 進行 HTML 編碼。 請考慮下列 Razor 標記：

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

法文資源檔可能包含下列內容：

| Key | 值 |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |

轉譯的檢視內容可能包含來自資源檔的 HTML 標記。

**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。

若要在檢視中使用共用的資源檔，請插入 `IHtmlLocalizer<T>`：

[!code-cshtml[](~/fundamentals/localization/sample/3.x/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>DataAnnotations 當地語系化

DataAnnotations 錯誤訊息會使用 `IStringLocalizer<T>` 來當地語系化。 使用 `ResourcesPath = "Resources"` 選項時，`RegisterViewModel` 中的錯誤訊息會儲存在下列路徑之一：

* *Resources/Viewmodel. RegisterViewModel. fr .resx*
* *Resources/ViewModels/Account/RegisterViewModel.fr.resx*

[!code-csharp[](localization/sample/3.x/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

在 ASP.NET Core MVC 1.1.0 和更高版本中，系統會將非驗證屬性當地語系化。 ASP.NET Core MVC 1.0 **不會**查閱非驗證屬性的當地語系化字串。

<a name="one-resource-string-multiple-classes"></a>

### <a name="using-one-resource-string-for-multiple-classes"></a>針對多個類別使用同一個資源字串

下列程式碼會示範如何針對含有多個類別的驗證屬性使用同一個資源字串：

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

在先前的程式碼中，`SharedResource` 是與儲存驗證訊息的 resx 對應的類別。 使用這個方法時，DataAnnotations 只會使用 `SharedResource`，而不是每個類別的資源。

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>針對您支援的語言和文化特性提供當地語系化資源

### <a name="supportedcultures-and-supporteduicultures"></a>SupportedCultures 和 SupportedUICultures

ASP.NET Core 可讓您指定 `SupportedCultures` 和 `SupportedUICultures` 這兩個文化特性值。 `SupportedCultures` 的 [CultureInfo](/dotnet/api/system.globalization.cultureinfo) 物件可決定文化特性相依函式的結果，例如日期、時間、數字及貨幣格式。 `SupportedCultures` 也可決定文字排列順序、大小寫慣例和字串比較。 如需伺服器如何取得文化特性的詳細資訊，請參閱 [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)。 `SupportedUICultures`會決定[ResourceManager](/dotnet/api/system.resources.resourcemanager)會查閱哪些翻譯的字串（來自 *.resx*檔案）。 `ResourceManager` 僅會查閱 `CurrentUICulture` 所決定之文化特性特有的字串。 .NET 中的每個執行緒都有 `CurrentCulture` 和 `CurrentUICulture` 物件。 ASP.NET Core 會在轉譯文化特性相依函式時檢查這些值。 比方說，如果目前執行緒的文化特性設定為 "en-US" (英文 - 美國)，`DateTime.Now.ToLongDateString()` 會顯示 "Thursday, February 18, 2016"，但如果 `CurrentCulture` 設定為 "es-ES" (西班牙文 - 西班牙)，則輸出會是 "jueves, 18 de febrero de 2016"。

## <a name="resource-files"></a>資源檔

資源檔是一種實用的機制，可讓您將可當地語系化的字串與代碼區隔開來。 非預設語言的翻譯字串會在 *.resx*資源檔中隔離。 例如，您可以建立名為 *Welcome.es.resx* 的西班牙文資源檔，以包含翻譯的字串。 "es" 是西班牙文的語言代碼。 若要在 Visual Studio 中建立這個資源檔：

1. 在方案總管**** 中，以滑鼠右鍵按一下要放置資源檔的資料夾 > [新增]**[新增項目]** > ****。

    ![巢狀特色選單：方案總管會開啟 [資源] 的特色選單， 接著針對 [新增] 開啟第二個特色選單，並反白顯示 [新增項目] 命令。](localization/_static/newi.png)

2. 在 [Search installed templates] (搜尋已安裝的範本)**** 方塊中，輸入「資源」，並命名檔案。

    ![[新增項目] 對話方塊](localization/_static/res.png)

3. 在 [名稱]**** 資料行中輸入索引鍵值 (原生字串)，並在 [值]**** 資料行中輸入已翻譯的字串。

    ![Welcome.es.resx 檔案 (西班牙文的「歡迎使用」資源檔)，其中 [名稱] 資料行的文字為 Hello，而 [值] 資料行的文字為 Hola (Hello 的西班牙文)](localization/_static/hola.png)

    Visual Studio 會顯示 *Welcome.es.resx* 檔案。

    ![方案總管，其中顯示「歡迎使用」的西班牙文 (es) 資源檔](localization/_static/se.png)

## <a name="resource-file-naming"></a>資源檔命名

資源的命名方式是以其類別的完整類型名稱去掉組件名稱而得。 例如，假設專案中的法文資源是 `LocalizationWebsite.Web.Startup` 類別、主要組件為 `LocalizationWebsite.Web.dll`，就會命名為 *Startup.fr.resx*。 若是 `LocalizationWebsite.Web.Controllers.HomeController` 類別的資源，則應命名為 *Controllers.HomeController.fr.resx*。 如果目標類別的命名空間和組件名稱不相同，則需要使用完整類型名稱。 比方說，範例專案中 `ExtraNamespace.Tools` 類型的資源會命名為 *ExtraNamespace.Tools.fr.resx*。

在範例專案中，`ConfigureServices` 方法會將 `ResourcesPath` 設為 "Resources"，因此首頁控制器的法文資源檔專案相對路徑即為 *Resources/Controllers.HomeController.fr.resx*。 或者，您可以使用資料夾來收集資源檔。 若是首頁控制器，路徑就是 *Resources/Controllers/HomeController.fr.resx*。 如果您不使用 `ResourcesPath` 選項，*.resx* 檔案即會放置在專案的基底目錄中。 `HomeController` 的資源檔會命名為 *Controllers.HomeController.fr.resx*。 您可依據自己的資源檔收集方式，來選擇要使用點或路徑的命名慣例。

| 資源名稱 | 點或路徑命名 |
| ------------   | ------------- |
| Resources/Controllers.HomeController.fr.resx | 點  |
| Resources/Controllers/HomeController.fr.resx  | 路徑 |
|    |     |

在 views 中使用的資源檔會 `@inject IViewLocalizer` Razor 遵循類似的模式。 您可以使用點命名或路徑命名方式，來命名檢視的資源檔。 Razor查看資源檔模擬其相關聯之視圖檔案的路徑。 假設我們將 `ResourcesPath` 設為 "Resources"，與 *Views/Home/About.cshtml* 檢視建立關聯的法文資源檔可為下列其一：

* Resources/Views/Home/About.fr.resx

* Resources/Views.Home.About.fr.resx

如果您不使用 `ResourcesPath` 選項，則檢視的 *.resx* 檔案會與檢視位於相同資料夾中。

### <a name="rootnamespaceattribute"></a>RootNamespaceAttribute 

[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) 屬性會在組件的根命名空間與組件名稱不同時，提供組件的根命名空間。 

> [!WARNING]
> 當專案的名稱不是有效的 .NET 識別碼時，就可能發生這種情況。 例如， `my-project-name.csproj` 會使用根命名空間 `my_project_name` ，以及 `my-project-name` 導致此錯誤的元件名稱。 

如果組件的根命名空間與組件名稱不同：

* 根據預設，當地語系化無法運作。
* 在組件中搜尋資源的方式造成當地語系化失敗。 `RootNamespace` 是建置時間值，無法用於執行處理序。 

如果 `RootNamespace` 與 `AssemblyName` 不同，請將下列內容納入 *AssemblyInfo.cs* (參數值取代為實際值)：

```csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

上述程式碼可成功解析 resx 檔案。

## <a name="culture-fallback-behavior"></a>文化特性後援行為

搜尋資源時，當地語系化會使用「文化特性後援」。 從所要求的文化特性開始，如果找不到，就會還原成該文化特性的父文化特性。 另外，[CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) 屬性代表父文化特性。 這通常 (但並非一定) 表示從 ISO 中移除國家意符 (Signifier)。 例如，墨西哥的西班牙文方言是 "es-MX"。 它具有父系 "es" &mdash; 西班牙文不是任何國家/地區的特定項目。

假設您的網站收到使用文化特性 "fr-CA" 之 "Welcome" 資源的要求。 當地語系化系統會依照順序尋找下列資源，並選取第一個相符項目：

* *Welcome.fr-CA.resx*
* *Welcome.fr.resx*
* *Welcome.resx* (如果 `NeutralResourcesLanguage` 是 "fr-CA")

舉例來說，如果您移除 ".fr" 文化特性指示項，並將文化特性設定為法文，則系統會讀取預設資源檔，並將字串當地語系化。 當沒有任何項目符合您要求的文化特性時，資源管理員即會指定預設資源或後援資源。 如果您只想在要求的文化特性缺少資源時傳回索引鍵，就不能使用預設資源檔。

### <a name="generate-resource-files-with-visual-studio"></a>使用 Visual Studio 產生資源檔

如果您在 Visual Studio 中建立資源檔，但檔案名稱中不含文化特性 (例如 *Welcome.resx*)，則 Visual Studio 會針對每個字串的屬性建立 C# 類別。 但這通常不是您使用 ASP.NET Core 的初衷。 一般來說，您不會有預設的 *.resx* 資源檔 (不含文化特性名稱的 *.resx* 檔案)。 因此，建議您建立含有文化特性名稱的 *.resx* 檔案 (例如 *Welcome.fr.resx*)。 當您建立含有文化特性名稱的 *.resx* 檔案時，Visual Studio 就不會產生類別檔案。

### <a name="add-other-cultures"></a>新增其他文化特性

每種語言和文化特性的組合 (非預設語言) 都需要唯一的資源檔。 若要建立不同文化特性和地區設定的資源檔，您可以建立新的資源檔並將 ISO 語言代碼作為檔名的一部分 (例如 **en-us**、**fr-ca** 和 **en-gb**)。 您應將 ISO 代碼置於檔案名稱和 *.resx* 副檔名之間，例如 *Welcome.es-MX.resx* (西班牙文/墨西哥)。

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>實作可依據每項要求選取語言/文化特性的策略

### <a name="configure-localization"></a>設定當地語系化

您可以在 `Startup.ConfigureServices` 方法中設定當地語系化：

[!code-csharp[](localization/sample/3.x/Localization/Startup.cs?name=snippet1)]

* `AddLocalization` 可將當地語系化服務新增至服務容器。 上方的程式碼也會將資源路徑設為 "Resources"。

* `AddViewLocalization` 可支援當地語系化的檢視檔案。 在此範例中，檢視的當地語系化會以檢視檔案的後置詞為依據。 例如 *Index.fr.cshtml* 檔案中的 "fr"。

* `AddDataAnnotationsLocalization` 可支援透過 `IStringLocalizer` 抽象概念而來的當地語系化 `DataAnnotations` 驗證訊息。

### <a name="localization-middleware"></a>當地語系化中介軟體

您可以在當地語系化[中介軟體](xref:fundamentals/middleware/index)中，設定要求目前的文化特性。 已在 `Startup.Configure` 方法中啟用當地語系化中介軟體。 您必須在可能檢查要求文化特性的任何中介軟體之前，設定當地語系化中介軟體 (例如 `app.UseMvcWithDefaultRoute()`)。

[!code-csharp[](localization/sample/3.x/Localization/Startup.cs?name=snippet2)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

`UseRequestLocalization` 會初始化 `RequestLocalizationOptions` 物件。 在每次要求時，系統會列舉 `RequestLocalizationOptions` 中的 `RequestCultureProvider` 清單，並使用能成功判斷要求的文化特性的第一個提供者。 預設的提供者是來自 `RequestLocalizationOptions` 類別：

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

預設清單會由針對性高到低來排列。 在本文稍後，我們會說明如何變更順序，甚至新增自訂的文化特性提供者。 如果沒有任何提供者可以判斷要求的文化特性，即會使用 `DefaultRequestCulture`。

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

有些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。 若是使用 Cookie 或 Accept-Language 標頭方法的應用程式，您可以將查詢字串新增至 URL 以偵錯和測試程式碼。 系統預設會將 `QueryStringRequestCultureProvider` 登錄為 `RequestCultureProvider` 清單中的第一個當地語系化提供者。 您應傳遞查詢字串參數 `culture` 和 `ui-culture`。 下列範例會設定西班牙文/墨西哥的特定文化特性 (語言和地區)：

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

如果您只傳入兩者其一 (`culture` 或 `ui-culture`)，查詢字串提供者就會使用您傳入的項目來設定這兩個值。 例如，若只設定文化特性，即會同時設定 `Culture` 和 `UICulture`：

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

生產環境應用程式通常會提供一個機制，來設定 ASP.NET Core 文化特性 Cookie 的文化特性。 若要建立 Cookie，請使用 `MakeCookieValue` 方法。

會傳回 `CookieRequestCultureProvider` `DefaultCookieName` 用來追蹤使用者慣用文化特性資訊的預設 cookie 名稱。 預設 Cookie 名稱為 `.AspNetCore.Culture`。

Cookie 格式為 `c=%LANGCODE%|uic=%LANGCODE%`，其中 `c` 是 `Culture` 而 `uic` 是 `UICulture`，例如：

    c=en-UK|uic=en-US

如果您只指定文化特性資訊和 UI 文化特性其一，系統就會將您所指定的文化特性用於文化特性資訊和 UI 文化特性。

### <a name="the-accept-language-http-header"></a>Accept-Language HTTP 標頭

您可在大多數的瀏覽器中設定 [Accept-Language 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)，其最初的設計目的是用來指定使用者的語言。 這項設定可指出瀏覽器已設好要傳送哪些項目，或已從基礎作業系統繼承哪些項目。 透過瀏覽器要求的 Accept-Language HTTP 標頭來偵測使用者的慣用語言，並非萬無一失 (請參閱 [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php) (在瀏覽器中設定語言喜好設定)。 生產環境應用程式應該包含可讓使用者自訂文化特性的選擇方式。

### <a name="set-the-accept-language-http-header-in-ie"></a>在 IE 中設定 Accept-Language HTTP 標頭

1. 從齒輪圖示，點選 [網際網路選項]****。

2. 點選 [語言]****。

    ![網際網路選項](localization/_static/lang.png)

3. 點選 [設定語言喜好設定]****。

4. 點選 [新增語言]****。

5. 新增語言。

6. 點選語言，然後點選 [上移]****。

### <a name="use-a-custom-provider"></a>使用自訂提供者

假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫中。 您可以撰寫提供者，以供使用者查詢這些值。 下列程式碼會示範如何新增自訂提供者：

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

使用 `RequestLocalizationOptions` 新增或移除當地語系化提供者。

### <a name="set-the-culture-programmatically"></a>以程式設計方式來設定文化特性

[GitHub](https://github.com/aspnet/entropy) 上的這個範例 **Localization.StarterWeb** 專案包含可設定 `Culture` 的 UI。 *Views/Shared/_SelectLanguagePartial.cshtml* 檔可讓您從支援的文化特性清單中選取文化特性：

[!code-cshtml[](localization/sample/3.x/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

系統會將 *Views/Shared/_SelectLanguagePartial.cshtml* 檔案新增至配置檔案的 `footer` 區段，以供所有檢視使用：

[!code-cshtml[](localization/sample/3.x/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

`SetLanguage` 方法會設定文化特性的 Cookie。

[!code-csharp[](localization/sample/3.x/Localization/Controllers/HomeController.cs?range=57-67)]

您無法將 *_SelectLanguagePartial.cshtml* 插入這個專案的範例程式碼。 [GitHub](https://github.com/aspnet/entropy)上的**localization.starterweb**專案具有程式碼，可透過相依性 `RequestLocalizationOptions` 插入容器將傳遞至 Razor 部分。 [Dependency Injection](dependency-injection.md)

## <a name="model-binding-route-data-and-query-strings"></a>模型系結路由資料和查詢字串

請參閱模型系結[路由資料和查詢字串的全球化行為](xref:mvc/models/model-binding#glob)。

## <a name="globalization-and-localization-terms"></a>全球化和當地語系化詞彙

在進行應用程式的當地語系化程序時，您也需要具備現代軟體開發中常用的相關字元集基本知識，並了解與它們建立關聯的問題。 雖然所有電腦都會將文字儲存為數字 (程式碼)，但不同系統會使用不同數字來儲存相同的文字。 當地語系化是指針對特定文化特性/地區設定，轉譯應用程式使用者介面 (UI) 的程序。

[可當地語系化](/dotnet/standard/globalization-localization/localizability-review)是確認全球化應用程式已準備好進行當地語系化的中繼程序。

文化特性名稱的 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 格式是 `<languagecode2>-<country/regioncode2>`，其中 `<languagecode2>` 是語言代碼，而 `<country/regioncode2>` 是子文化特性代碼。 例如，`es-CL` 是指西班牙文 (智利)，`en-US` 是指英文 (美國)，而 `en-AU` 是指英文 (澳大利亞)。 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 是 ISO 639 (兩個小寫字母，為與某個語言建立關聯的文化特性代碼) 及 ISO 3166 (兩個大寫字母，為與某個國家或地區建立關聯的子文化特性代碼) 的組合。 請參閱 [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx) (語言的文化特性名稱)。

國際化通常縮寫為 "I18N"。 這個縮寫是採用該詞彙的第一個和最後一個字母，以及這兩個字母間的字母數組成，因此 18 代表第一個字母 "I" 及最後 "N" 中間的字母數。 同樣的原則也適用於全球化 (G11N) 與當地語系化 (L10N) 的縮寫。

詞彙：

* 全球化 (G11N)：讓應用程式支援不同語言和區域的程序。
* 當地語系化 (L10N)：針對特定語言和區域，自訂應用程式的程序。
* 國際化 (I18N)：包含全球化和當地語系化這兩部分的描述。
* 文化特性：它是一種語言和/或地區。
* 中性文化特性：具有指定的語言但不限地區的文化特性  (例如 "en"、"es")。
* 特定文化特性：具有指定的語言和地區的文化特性  (例如 "en-US"、"en-GB"、"es-CL")。
* 父文化特性：包含特定文化特性的中性文化特性  (例如，"en" 是 "en-US" 和 "en-GB" 的父文化特性)。
* 地區設定：地區設定與文化特性相同。

[!INCLUDE[](~/includes/localization/currency.md)]

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/troubleshoot-aspnet-core-localization>
* 本文使用的 [Localization.StarterWeb 專案](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb)。
* [全球化與當地語系化 .NET 應用程式](/dotnet/standard/globalization-localization/index)
* [.Resx 檔案中的資源](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [Microsoft 多語應用程式工具組](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [當地語系化和泛型](http://hishambinateya.com/localization-and-generics)

::: moniker-end

<!-- ASP.NET Core 5.x starts here -->
::: moniker range="> aspnetcore-3.1"

由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Damien Bowden](https://twitter.com/damien_bod)、[Bart Calixto](https://twitter.com/bartmax)、[Nadeem Afana](https://afana.me/) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya) 提供

多語系網站可讓網站觸及更多物件。 ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。

國際化包含[全球化](/dotnet/api/system.globalization)和[當地語系化](/dotnet/standard/globalization-localization/localization)這兩部分。 全球化是指設計出可支援不同文化特性之應用程式的程序。 透過全球化，您可新增支援與特定地區相關之已定義語言指令碼的輸入、顯示及輸出作業。

當地語系化是指針對全球化應用程式進行調整的程序，且您已順應特定文化特性/地區設定對這些全球化應用程式進行可當地語系化處理。 如需詳細資訊，請參閱本文件結尾處的**全球化和當地語系化詞彙**。

應用程式當地語系化包含下列作業：

1. 讓應用程式的內容可當地語系化
1. 針對您支援的語言和文化特性提供當地語系化資源
1. 實作可依據每項要求選取語言/文化特性的策略

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/2.x/)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="make-the-apps-content-localizable"></a>讓應用程式的內容可當地語系化

<xref:Microsoft.Extensions.Localization.IStringLocalizer>和 <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> 已架構，可在開發當地語系化應用程式時改善生產力。 `IStringLocalizer`會使用 <xref:System.Resources.ResourceManager> 和， <xref:System.Resources.ResourceReader> 在執行時間提供特定文化特性的資源。 介面具有索引子和， `IEnumerable` 用於傳回當地語系化的字串。 `IStringLocalizer`不需要將預設語言字串儲存在資源檔中。 您不必在開發初期建立資源檔，即可開發以當地語系化為目標的應用程式。 下列程式碼會示範如何包裝 "About Title" 字串以進行當地語系化。

[!code-csharp[](localization/sample/3.x/Localization/Controllers/AboutController.cs)]

在上述程式碼中， `IStringLocalizer<T>` 執行是來自相依性[插入](dependency-injection.md)。 如果找不到 "About Title" 的當地語系化值，即會傳回索引子的索引鍵，也就是 "About Title" 字串。 您可以保留應用程式中的預設語言常值字串，並將其包裝在當地語系化工具中，以便專注於開發應用程式。 您不用先建立預設資源檔，即可使用預設語言來開發應用程式，並針對當地語系化步驟進行應用程式的準備。 或者，您可以使用傳統方法，並提供索引鍵以擷取預設語言字串。 對許多開發人員來說，新的工作流程 (單純包裝字串常值而不使用預設語言的 *.resx* 檔案) 可以降低當地語系化應用程式的額外負荷。 其他開發人員則偏好傳統的工作流程，因為這種方法更便於使用較長的字串常值，且更易於更新當地語系化的字串。

若是包含 HTML 的資源，請使用 `IHtmlLocalizer<T>` 實作。 `IHtmlLocalizer` 會對資源字串中經過格式化的引數進行 HTML 編碼，但不會對資源字串本身進行 HTML 編碼。 在下列醒目提示的範例中，只有 `name` 參數的值經過 HTML 編碼。

[!code-csharp[](~/fundamentals/localization/sample/3.x/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。

您可以在最底層的[相依性插入](dependency-injection.md)中，將 `IStringLocalizerFactory` 移出：

[!code-csharp[](localization/sample/3.x/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

上述程式碼會示範這兩個 Factory Create 方法。

您可以依據控制器或區域來分割當地語系化的字串，也可以只用一個容器。 在範例應用程式中，會針對共用資源使用名為 `SharedResource` 的虛擬類別。

[!code-csharp[](localization/sample/3.x/Localization/Resources/SharedResource.cs)]

有些開發人員會使用 `Startup` 類別來包含全域或共用字串。 下列範例會使用 `InfoController` 和 `SharedResource` 當地語系化工具：

[!code-csharp[](localization/sample/3.x/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>檢視當地語系化

`IViewLocalizer` 服務可提供[檢視](xref:mvc/views/overview)的當地語系化字串。 `ViewLocalizer` 類別會實作這個介面，並透過檢視檔案路徑來找出資源的位置。 下列程式碼示範如何使用 `IViewLocalizer` 的預設實作：

[!code-cshtml[](localization/sample/3.x/Localization/Views/Home/About.cshtml)]

`IViewLocalizer` 的預設實作會依據檢視的檔案名稱來找出資源檔。 其中並沒有任何選項可以使用全域共用的資源檔。 `ViewLocalizer`使用來執行當地語系化工具 `IHtmlLocalizer` ，因此 Razor 不會對當地語系化字串進行 HTML 編碼。 您可以參數化資源字串，`IViewLocalizer` 即會對參數 (而不是資源字串) 進行 HTML 編碼。 請考慮下列 Razor 標記：

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

法文資源檔可能包含下列內容：

| Key | 值 |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |

轉譯的檢視內容可能包含來自資源檔的 HTML 標記。

**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。

若要在檢視中使用共用的資源檔，請插入 `IHtmlLocalizer<T>`：

[!code-cshtml[](~/fundamentals/localization/sample/3.x/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>DataAnnotations 當地語系化

DataAnnotations 錯誤訊息會使用 `IStringLocalizer<T>` 來當地語系化。 使用 `ResourcesPath = "Resources"` 選項時，`RegisterViewModel` 中的錯誤訊息會儲存在下列路徑之一：

* *Resources/Viewmodel. RegisterViewModel. fr .resx*
* *Resources/ViewModels/Account/RegisterViewModel.fr.resx*

[!code-csharp[](localization/sample/3.x/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

在 ASP.NET Core MVC 1.1.0 和更高版本中，系統會將非驗證屬性當地語系化。 ASP.NET Core MVC 1.0 **不會**查閱非驗證屬性的當地語系化字串。

<a name="one-resource-string-multiple-classes"></a>

### <a name="using-one-resource-string-for-multiple-classes"></a>針對多個類別使用同一個資源字串

下列程式碼會示範如何針對含有多個類別的驗證屬性使用同一個資源字串：

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

在先前的程式碼中，`SharedResource` 是與儲存驗證訊息的 resx 對應的類別。 使用這個方法時，DataAnnotations 只會使用 `SharedResource`，而不是每個類別的資源。

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>針對您支援的語言和文化特性提供當地語系化資源

### <a name="supportedcultures-and-supporteduicultures"></a>SupportedCultures 和 SupportedUICultures

ASP.NET Core 可讓您指定 `SupportedCultures` 和 `SupportedUICultures` 這兩個文化特性值。 `SupportedCultures` 的 [CultureInfo](/dotnet/api/system.globalization.cultureinfo) 物件可決定文化特性相依函式的結果，例如日期、時間、數字及貨幣格式。 `SupportedCultures` 也可決定文字排列順序、大小寫慣例和字串比較。 如需伺服器如何取得文化特性的詳細資訊，請參閱 [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)。 `SupportedUICultures`會決定[ResourceManager](/dotnet/api/system.resources.resourcemanager)會查閱哪些翻譯的字串（來自 *.resx*檔案）。 `ResourceManager` 僅會查閱 `CurrentUICulture` 所決定之文化特性特有的字串。 .NET 中的每個執行緒都有 `CurrentCulture` 和 `CurrentUICulture` 物件。 ASP.NET Core 會在轉譯文化特性相依函式時檢查這些值。 比方說，如果目前執行緒的文化特性設定為 "en-US" (英文 - 美國)，`DateTime.Now.ToLongDateString()` 會顯示 "Thursday, February 18, 2016"，但如果 `CurrentCulture` 設定為 "es-ES" (西班牙文 - 西班牙)，則輸出會是 "jueves, 18 de febrero de 2016"。

## <a name="resource-files"></a>資源檔

資源檔是一種實用的機制，可讓您將可當地語系化的字串與代碼區隔開來。 非預設語言的翻譯字串會在 *.resx*資源檔中隔離。 例如，您可以建立名為 *Welcome.es.resx* 的西班牙文資源檔，以包含翻譯的字串。 "es" 是西班牙文的語言代碼。 若要在 Visual Studio 中建立這個資源檔：

1. 在方案總管**** 中，以滑鼠右鍵按一下要放置資源檔的資料夾 > [新增]**[新增項目]** > ****。

    ![巢狀特色選單：方案總管會開啟 [資源] 的特色選單， 接著針對 [新增] 開啟第二個特色選單，並反白顯示 [新增項目] 命令。](localization/_static/newi.png)

2. 在 [Search installed templates] (搜尋已安裝的範本)**** 方塊中，輸入「資源」，並命名檔案。

    ![[新增項目] 對話方塊](localization/_static/res.png)

3. 在 [名稱]**** 資料行中輸入索引鍵值 (原生字串)，並在 [值]**** 資料行中輸入已翻譯的字串。

    ![Welcome.es.resx 檔案 (西班牙文的「歡迎使用」資源檔)，其中 [名稱] 資料行的文字為 Hello，而 [值] 資料行的文字為 Hola (Hello 的西班牙文)](localization/_static/hola.png)

    Visual Studio 會顯示 *Welcome.es.resx* 檔案。

    ![方案總管，其中顯示「歡迎使用」的西班牙文 (es) 資源檔](localization/_static/se.png)

## <a name="resource-file-naming"></a>資源檔命名

資源的命名方式是以其類別的完整類型名稱去掉組件名稱而得。 例如，假設專案中的法文資源是 `LocalizationWebsite.Web.Startup` 類別、主要組件為 `LocalizationWebsite.Web.dll`，就會命名為 *Startup.fr.resx*。 若是 `LocalizationWebsite.Web.Controllers.HomeController` 類別的資源，則應命名為 *Controllers.HomeController.fr.resx*。 如果目標類別的命名空間和組件名稱不相同，則需要使用完整類型名稱。 比方說，範例專案中 `ExtraNamespace.Tools` 類型的資源會命名為 *ExtraNamespace.Tools.fr.resx*。

在範例專案中，`ConfigureServices` 方法會將 `ResourcesPath` 設為 "Resources"，因此首頁控制器的法文資源檔專案相對路徑即為 *Resources/Controllers.HomeController.fr.resx*。 或者，您可以使用資料夾來收集資源檔。 若是首頁控制器，路徑就是 *Resources/Controllers/HomeController.fr.resx*。 如果您不使用 `ResourcesPath` 選項，*.resx* 檔案即會放置在專案的基底目錄中。 `HomeController` 的資源檔會命名為 *Controllers.HomeController.fr.resx*。 您可依據自己的資源檔收集方式，來選擇要使用點或路徑的命名慣例。

| 資源名稱 | 點或路徑命名 |
| ------------   | ------------- |
| Resources/Controllers.HomeController.fr.resx | 點  |
| Resources/Controllers/HomeController.fr.resx  | 路徑 |
|    |     |

在 views 中使用的資源檔會 `@inject IViewLocalizer` Razor 遵循類似的模式。 您可以使用點命名或路徑命名方式，來命名檢視的資源檔。 Razor查看資源檔模擬其相關聯之視圖檔案的路徑。 假設我們將 `ResourcesPath` 設為 "Resources"，與 *Views/Home/About.cshtml* 檢視建立關聯的法文資源檔可為下列其一：

* Resources/Views/Home/About.fr.resx

* Resources/Views.Home.About.fr.resx

如果您不使用 `ResourcesPath` 選項，則檢視的 *.resx* 檔案會與檢視位於相同資料夾中。

### <a name="rootnamespaceattribute"></a>RootNamespaceAttribute 

[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) 屬性會在組件的根命名空間與組件名稱不同時，提供組件的根命名空間。 

> [!WARNING]
> 當專案的名稱不是有效的 .NET 識別碼時，就可能發生這種情況。 例如， `my-project-name.csproj` 會使用根命名空間 `my_project_name` ，以及 `my-project-name` 導致此錯誤的元件名稱。 

如果組件的根命名空間與組件名稱不同：

* 根據預設，當地語系化無法運作。
* 在組件中搜尋資源的方式造成當地語系化失敗。 `RootNamespace` 是建置時間值，無法用於執行處理序。 

如果 `RootNamespace` 與 `AssemblyName` 不同，請將下列內容納入 *AssemblyInfo.cs* (參數值取代為實際值)：

```csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

上述程式碼可成功解析 resx 檔案。

## <a name="culture-fallback-behavior"></a>文化特性後援行為

搜尋資源時，當地語系化會使用「文化特性後援」。 從所要求的文化特性開始，如果找不到，就會還原成該文化特性的父文化特性。 另外，[CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) 屬性代表父文化特性。 這通常 (但並非一定) 表示從 ISO 中移除國家意符 (Signifier)。 例如，墨西哥的西班牙文方言是 "es-MX"。 它具有父系 "es" &mdash; 西班牙文不是任何國家/地區的特定項目。

假設您的網站收到使用文化特性 "fr-CA" 之 "Welcome" 資源的要求。 當地語系化系統會依照順序尋找下列資源，並選取第一個相符項目：

* *Welcome.fr-CA.resx*
* *Welcome.fr.resx*
* *Welcome.resx* (如果 `NeutralResourcesLanguage` 是 "fr-CA")

舉例來說，如果您移除 ".fr" 文化特性指示項，並將文化特性設定為法文，則系統會讀取預設資源檔，並將字串當地語系化。 當沒有任何項目符合您要求的文化特性時，資源管理員即會指定預設資源或後援資源。 如果您只想在要求的文化特性缺少資源時傳回索引鍵，就不能使用預設資源檔。

### <a name="generate-resource-files-with-visual-studio"></a>使用 Visual Studio 產生資源檔

如果您在 Visual Studio 中建立資源檔，但檔案名稱中不含文化特性 (例如 *Welcome.resx*)，則 Visual Studio 會針對每個字串的屬性建立 C# 類別。 但這通常不是您使用 ASP.NET Core 的初衷。 一般來說，您不會有預設的 *.resx* 資源檔 (不含文化特性名稱的 *.resx* 檔案)。 因此，建議您建立含有文化特性名稱的 *.resx* 檔案 (例如 *Welcome.fr.resx*)。 當您建立含有文化特性名稱的 *.resx* 檔案時，Visual Studio 就不會產生類別檔案。

### <a name="add-other-cultures"></a>新增其他文化特性

每種語言和文化特性的組合 (非預設語言) 都需要唯一的資源檔。 若要建立不同文化特性和地區設定的資源檔，您可以建立新的資源檔並將 ISO 語言代碼作為檔名的一部分 (例如 **en-us**、**fr-ca** 和 **en-gb**)。 您應將 ISO 代碼置於檔案名稱和 *.resx* 副檔名之間，例如 *Welcome.es-MX.resx* (西班牙文/墨西哥)。

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>實作可依據每項要求選取語言/文化特性的策略

### <a name="configure-localization"></a>設定當地語系化

您可以在 `Startup.ConfigureServices` 方法中設定當地語系化：

[!code-csharp[](localization/sample/3.x/Localization/Startup.cs?name=snippet1)]

* `AddLocalization` 可將當地語系化服務新增至服務容器。 上方的程式碼也會將資源路徑設為 "Resources"。

* `AddViewLocalization` 可支援當地語系化的檢視檔案。 在此範例中，檢視的當地語系化會以檢視檔案的後置詞為依據。 例如 *Index.fr.cshtml* 檔案中的 "fr"。

* `AddDataAnnotationsLocalization` 可支援透過 `IStringLocalizer` 抽象概念而來的當地語系化 `DataAnnotations` 驗證訊息。

### <a name="localization-middleware"></a>當地語系化中介軟體

您可以在當地語系化[中介軟體](xref:fundamentals/middleware/index)中，設定要求目前的文化特性。 已在 `Startup.Configure` 方法中啟用當地語系化中介軟體。 您必須在可能檢查要求文化特性的任何中介軟體之前，設定當地語系化中介軟體 (例如 `app.UseMvcWithDefaultRoute()`)。

[!code-csharp[](localization/sample/3.x/Localization/Startup.cs?name=snippet2)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

`UseRequestLocalization` 會初始化 `RequestLocalizationOptions` 物件。 在每次要求時，系統會列舉 `RequestLocalizationOptions` 中的 `RequestCultureProvider` 清單，並使用能成功判斷要求的文化特性的第一個提供者。 預設的提供者是來自 `RequestLocalizationOptions` 類別：

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

預設清單會由針對性高到低來排列。 在本文稍後，我們會說明如何變更順序，甚至新增自訂的文化特性提供者。 如果沒有任何提供者可以判斷要求的文化特性，即會使用 `DefaultRequestCulture`。

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

有些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。 若是使用 Cookie 或 Accept-Language 標頭方法的應用程式，您可以將查詢字串新增至 URL 以偵錯和測試程式碼。 系統預設會將 `QueryStringRequestCultureProvider` 登錄為 `RequestCultureProvider` 清單中的第一個當地語系化提供者。 您應傳遞查詢字串參數 `culture` 和 `ui-culture`。 下列範例會設定西班牙文/墨西哥的特定文化特性 (語言和地區)：

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

如果您只傳入兩者其一 (`culture` 或 `ui-culture`)，查詢字串提供者就會使用您傳入的項目來設定這兩個值。 例如，若只設定文化特性，即會同時設定 `Culture` 和 `UICulture`：

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

生產環境應用程式通常會提供一個機制，來設定 ASP.NET Core 文化特性 Cookie 的文化特性。 若要建立 Cookie，請使用 `MakeCookieValue` 方法。

會傳回 `CookieRequestCultureProvider` `DefaultCookieName` 用來追蹤使用者慣用文化特性資訊的預設 cookie 名稱。 預設 Cookie 名稱為 `.AspNetCore.Culture`。

Cookie 格式為 `c=%LANGCODE%|uic=%LANGCODE%`，其中 `c` 是 `Culture` 而 `uic` 是 `UICulture`，例如：

    c=en-UK|uic=en-US

如果您只指定文化特性資訊和 UI 文化特性其一，系統就會將您所指定的文化特性用於文化特性資訊和 UI 文化特性。

### <a name="the-accept-language-http-header"></a>Accept-Language HTTP 標頭

您可在大多數的瀏覽器中設定 [Accept-Language 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)，其最初的設計目的是用來指定使用者的語言。 這項設定可指出瀏覽器已設好要傳送哪些項目，或已從基礎作業系統繼承哪些項目。 透過瀏覽器要求的 Accept-Language HTTP 標頭來偵測使用者的慣用語言，並非萬無一失 (請參閱 [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php) (在瀏覽器中設定語言喜好設定)。 生產環境應用程式應該包含可讓使用者自訂文化特性的選擇方式。

### <a name="set-the-accept-language-http-header-in-ie"></a>在 IE 中設定 Accept-Language HTTP 標頭

1. 從齒輪圖示，點選 [網際網路選項]****。

2. 點選 [語言]****。

    ![網際網路選項](localization/_static/lang.png)

3. 點選 [設定語言喜好設定]****。

4. 點選 [新增語言]****。

5. 新增語言。

6. 點選語言，然後點選 [上移]****。

### <a name="the-content-language-http-header"></a>內容語言 HTTP 標頭

[內容語言](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language)實體標頭：

 - 用來描述適用于物件的語言。
 - 可讓使用者根據使用者的慣用語言來區分。

實體標頭會同時用於 HTTP 要求和回應。

您 `Content-Language` 可以藉由設定屬性來新增標頭 `ApplyCurrentCultureToResponseHeaders` 。

新增 `Content-Language` 標頭：

 - 允許 RequestLocalizationMiddleware 使用來設定 `Content-Language` 標頭 `CurrentUICulture` 。
 - 不需要明確地設定回應標頭 `Content-Language` 。

```csharp
app.UseRequestLocalization(new RequestLocalizationOptions
{
    ApplyCurrentCultureToResponseHeaders = true
});
```

### <a name="use-a-custom-provider"></a>使用自訂提供者

假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫中。 您可以撰寫提供者，以供使用者查詢這些值。 下列程式碼會示範如何新增自訂提供者：

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

    options.AddInitialRequestCultureProvider(new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

使用 `RequestLocalizationOptions` 新增或移除當地語系化提供者。

### <a name="set-the-culture-programmatically"></a>以程式設計方式來設定文化特性

[GitHub](https://github.com/aspnet/entropy) 上的這個範例 **Localization.StarterWeb** 專案包含可設定 `Culture` 的 UI。 *Views/Shared/_SelectLanguagePartial.cshtml* 檔可讓您從支援的文化特性清單中選取文化特性：

[!code-cshtml[](localization/sample/3.x/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

系統會將 *Views/Shared/_SelectLanguagePartial.cshtml* 檔案新增至配置檔案的 `footer` 區段，以供所有檢視使用：

[!code-cshtml[](localization/sample/3.x/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

`SetLanguage` 方法會設定文化特性的 Cookie。

[!code-csharp[](localization/sample/3.x/Localization/Controllers/HomeController.cs?range=57-67)]

您無法將 *_SelectLanguagePartial.cshtml* 插入這個專案的範例程式碼。 [GitHub](https://github.com/aspnet/entropy)上的**localization.starterweb**專案具有程式碼，可透過相依性 `RequestLocalizationOptions` 插入容器將傳遞至 Razor 部分。 [Dependency Injection](dependency-injection.md)

## <a name="model-binding-route-data-and-query-strings"></a>模型系結路由資料和查詢字串

請參閱模型系結[路由資料和查詢字串的全球化行為](xref:mvc/models/model-binding#glob)。

## <a name="globalization-and-localization-terms"></a>全球化和當地語系化詞彙

在進行應用程式的當地語系化程序時，您也需要具備現代軟體開發中常用的相關字元集基本知識，並了解與它們建立關聯的問題。 雖然所有電腦都會將文字儲存為數字 (程式碼)，但不同系統會使用不同數字來儲存相同的文字。 當地語系化是指針對特定文化特性/地區設定，轉譯應用程式使用者介面 (UI) 的程序。

[可當地語系化](/dotnet/standard/globalization-localization/localizability-review)是確認全球化應用程式已準備好進行當地語系化的中繼程序。

文化特性名稱的 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 格式是 `<languagecode2>-<country/regioncode2>`，其中 `<languagecode2>` 是語言代碼，而 `<country/regioncode2>` 是子文化特性代碼。 例如，`es-CL` 是指西班牙文 (智利)，`en-US` 是指英文 (美國)，而 `en-AU` 是指英文 (澳大利亞)。 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 是 ISO 639 (兩個小寫字母，為與某個語言建立關聯的文化特性代碼) 及 ISO 3166 (兩個大寫字母，為與某個國家或地區建立關聯的子文化特性代碼) 的組合。 請參閱 [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx) (語言的文化特性名稱)。

國際化通常縮寫為 "I18N"。 這個縮寫是採用該詞彙的第一個和最後一個字母，以及這兩個字母間的字母數組成，因此 18 代表第一個字母 "I" 及最後 "N" 中間的字母數。 同樣的原則也適用於全球化 (G11N) 與當地語系化 (L10N) 的縮寫。

詞彙：

* 全球化 (G11N)：讓應用程式支援不同語言和區域的程序。
* 當地語系化 (L10N)：針對特定語言和區域，自訂應用程式的程序。
* 國際化 (I18N)：包含全球化和當地語系化這兩部分的描述。
* 文化特性：它是一種語言和/或地區。
* 中性文化特性：具有指定的語言但不限地區的文化特性  (例如 "en"、"es")。
* 特定文化特性：具有指定的語言和地區的文化特性  (例如 "en-US"、"en-GB"、"es-CL")。
* 父文化特性：包含特定文化特性的中性文化特性  (例如，"en" 是 "en-US" 和 "en-GB" 的父文化特性)。
* 地區設定：地區設定與文化特性相同。

[!INCLUDE[](~/includes/localization/currency.md)]

[!INCLUDE[](~/includes/localization/unsupported-culture-log-level.md)]

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/troubleshoot-aspnet-core-localization>
* 本文使用的 [Localization.StarterWeb 專案](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb)。
* [全球化與當地語系化 .NET 應用程式](/dotnet/standard/globalization-localization/index)
* [.Resx 檔案中的資源](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [Microsoft 多語應用程式工具組](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [當地語系化和泛型](http://hishambinateya.com/localization-and-generics)

::: moniker-end
