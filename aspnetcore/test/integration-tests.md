---
title: ASP.NET Core 中的整合測試
author: guardrex
description: 了解整合測試如何確保應用程式的元件在基礎結構層級 (包括資料庫、檔案系統和網路) 正確運作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: test/integration-tests
ms.openlocfilehash: 8d304397fb7f218b395374c2b8c696fef9d9f8ad
ms.sourcegitcommit: 571d76fbbff05e84406b6d909c8fe9cbea2c8ff1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/01/2018
ms.locfileid: "39410178"
---
# <a name="integration-tests-in-aspnet-core"></a>ASP.NET Core 中的整合測試

藉由[Luke Latham](https://github.com/guardrex)和[Steve Smith](https://ardalis.com/)

整合測試可確保應用程式的元件正確運作，其中包含應用程式的支援基礎結構，例如資料庫、 檔案系統和網路層級。 ASP.NET Core 支援使用測試 web 主機和記憶體測試伺服器中的單元測試架構的整合測試。

本主題假設對單元測試知識有基本了解。 如果熟悉測試概念，請參閱[.NET Core 和.NET Standard 中的單元測試](/dotnet/core/testing/)主題和其連結的內容。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

範例應用程式是 Razor 頁面應用程式，並假設 Razor 頁面的基本知識。 如果不熟悉使用 Razor 頁面，請參閱下列主題：

* [Razor 頁面簡介](xref:razor-pages/index)
* [開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Razor 頁面單元測試](xref:test/razor-pages-tests)

## <a name="introduction-to-integration-tests"></a>整合測試的簡介

整合測試評估比更廣泛的層級上的應用程式的元件[單元測試](/dotnet/core/testing/)。 單元測試用來測試隔離的軟體元件，例如個別類別方法。 整合測試確認兩個或多個應用程式元件一起運作以產生預期的結果，可能包括 完整處理要求所需的每個元件。

這些更廣泛的測試用來測試應用程式的基礎結構和整個 framework，通常包括下列元件：

* 資料庫
* 檔案系統
* 網路設備
* 要求-回應管線

單元測試使用傳遞元件，稱為*fakes*或是*模擬物件*，來取代基礎結構元件。

相較於單元測試，整合測試：

* 使用實際的應用程式在生產環境中使用的元件。
* 需要更多的程式碼及資料處理。
* 需要較長的時間。

因此，限制使用的最重要的基礎結構案例的整合測試。 如果可以使用單元測試或整合測試，測試行為，請選擇 單元測試。

> [!TIP]
> 不要撰寫每個可能的排列的資料與檔案存取資料庫和檔案系統的整合測試。 不論多少位數跨應用程式會與資料庫和檔案系統、 特定的一份讀取、 寫入、 更新和刪除的整合測試通常能夠適當測試資料庫和檔案系統元件互動。 使用單元測試的方法的邏輯與這些元件互動的例行性測試。 在單元測試中的基礎結構使用 fakes/模擬 (mock) 更快速的測試執行中的結果。

> [!NOTE]
> 在討論中的整合測試，測試的專案時經常會呼叫*待測系統*，或簡稱為「 SUT」。

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core 整合測試

ASP.NET Core 中的整合測試需要下列各項：

* 測試專案用來包含及執行測試。 測試專案已測試的 ASP.NET Core 專案，稱為參考*待測系統*(SUT)。 _「 SUT 」 在本主題中用以參考該測試的應用程式。_
* 測試專案建立 SUT 測試 web 主機，並使用測試伺服器用戶端處理要求和回應 SUT。
* 測試執行器用來執行測試和報告測試結果。

整合測試會遵循包含一般的事件序列*排列*， *Act*，並*Assert*測試步驟：

1. SUT 的 web 主機已設定。
1. 測試伺服器用戶端會提交要求給應用程式。
1. *排列*測試步驟執行： 測試應用程式會準備要求。
1. *Act*測試步驟執行： 用戶端送出要求並接收回應。
1. *Assert*測試步驟執行：*實際*做為驗證回應*傳遞*或*失敗*根據*預期*回應。
1. 處理程序會繼續直到所有的測試會執行。
1. 報告測試結果。

通常，測試 web 主機設定以不同的方式比執行測試的應用程式的一般的 web 主機。 比方說，不同的資料庫或不同的應用程式設定可能會用來測試。

基礎結構元件，例如測試 web 主機和記憶體測試伺服器 ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver))、 提供或由[Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)封裝。 使用此封裝可以簡化測試建立和執行。

`Microsoft.AspNetCore.Mvc.Testing`封裝處理下列工作：

* 將相依性檔案複製 (*\*.deps*) 從到測試專案的 SUT *bin*資料夾。
* 設定 SUT 專案根目錄的內容根目錄，因此，靜態檔案和頁面/檢視表位於測試執行時。
* 提供[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類別來簡化啟動程序與 SUT `TestServer`。

[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)文件說明如何設定測試專案和測試執行器，以及如何執行測試和建議方法，名稱測試和測試類別的詳細指示。

> [!NOTE]
> 建立測試專案的應用程式時，分開的單元測試整合測試分成不同的專案。 這有助於確保，測試基礎結構的元件不小心包含在單元測試。 區隔單元和整合測試也可讓控制哪些資料集的測試的執行。

沒有幾乎任何的 Razor Pages 應用程式的測試組態和 MVC 應用程式之間的差異。 唯一的差別是在測試命名的方式。 在 Razor 頁面應用程式中測試的頁面端點通常命名為之後的頁面模型類別 (例如`IndexPageTests`來測試索引頁面的元件整合)。 在 MVC 應用程式中，測試會通常依控制器類別並命名這些測試的控制站 (例如`HomeControllerTests`測試針對 Home 控制器元件整合)。

## <a name="test-app-prerequisites"></a>測試應用程式的必要條件

測試專案必須：

* 請參考下列套件：
  - [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  - [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* 專案檔中指定 Web SDK (`<Project Sdk="Microsoft.NET.Sdk.Web">`)。 Web SDK 時，必須參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。

這些必要條件中所見[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/)。 檢查*tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj*檔案。 範例應用程式會使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)剖析器程式庫，因此範例應用程式也會參考：

* [xunit](https://www.nuget.org/packages/xunit/)
* [xunit.runner.visualstudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp/)

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>預設值 WebApplicationFactory 的基本測試

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)用來建立[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)整合測試。 `TEntryPoint` 通常是 SUT 的進入點類別`Startup`類別。

測試類別會實作*類別的裝置*介面 (`IClassFixture`) 以指出包含測試類別，且提供共用的物件執行個體的類別中的測試。

### <a name="basic-test-of-app-endpoints"></a>基本測試的應用程式端點

下列測試類別， `BasicTests`，使用`WebApplicationFactory`SUT 啟動程序，並提供[HttpClient](/dotnet/api/system.net.http.httpclient)至測試方法， `Get_EndpointsReturnSuccessAndCorrectContentType`。 方法會檢查是否成功的回應狀態碼 （在 200-299 的範圍內的狀態碼） 和`Content-Type`標頭是`text/html; charset=utf-8`數個應用程式頁面。

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)建立的執行個體`HttpClient`，會自動遵循重新導向，並會處理 cookie。

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>測試安全的端點

中的另一個測試`BasicTests`類別會檢查安全端點重新導向未驗證的使用者，應用程式的登入頁面。

SUT，在`/SecurePage`頁面上使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)來套用慣例[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)至頁面。 如需詳細資訊，請參閱  [Razor Pages 授權慣例](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

在 `Get_SecurePageRequiresAnAuthenticatedUser`測試，請[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)設為禁止重新導向設定[AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)至`false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

不允許用戶端以遵循重新導向，您可以進行下列檢查：

* SUT 所傳回的狀態碼可以檢查對預期[HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode)結果，不登入頁面，會重新導向後的最終狀態程式碼[HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* `Location`回應標頭中的標頭值會檢查以確認它開頭`http://localhost/Identity/Account/Login`，不是最後登入頁面的回應，其中`Location`標頭不會出現。

如需詳細資訊`WebApplicationFactoryClientOptions`，請參閱 <c2> [ 用戶端選項](#client-options)一節。

## <a name="customize-webapplicationfactory"></a>自訂 WebApplicationFactory

Web 主機組態可以建立獨立的測試類別，藉由繼承自`WebApplicationFactory`建立一或多個自訂的處理站：

1. 繼承自`WebApplicationFactory`，並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)可讓服務集合的組態[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   在植入的資料庫[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)由執行`InitializeDbForTests`方法。 此方法所述[整合測試範例： 測試應用程式的組織](#test-app-organization)一節。

2. 使用自訂`CustomWebApplicationFactory`測試類別中。 下列範例會使用中的處理站`IndexPageTests`類別：

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   範例應用程式的用戶端設定為防止`HttpClient`從下列重新導向。 中所述[測試安全的端點](#test-a-secure-endpoint) 區段中，這可讓測試，以檢查應用程式的第一個回應的結果。 第一個回應是，有許多使用這些測試中的重新導向`Location`標頭。

3. 一般測試會使用`HttpClient`和 helper 方法來處理要求和回應：

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

任何 SUT 的 POST 要求都必須滿足的應用程式時，會自動成為 antiforgery 核取[資料保護防偽系統](xref:security/data-protection/introduction)。 若要排列測試的 POST 要求，測試應用程式必須：

1. 提出要求的頁面。
1. 剖析的防偽 cookie 和回應的要求驗證權杖。
1. 在位置進行 POST 要求，使用防偽 cookie 和要求驗證權杖。

`SendAsync`協助程式的擴充方法 (*Helpers/HttpClientExtensions.cs*) 和`GetDocumentAsync`helper 方法 (*Helpers/HtmlHelpers.cs*) 中[的範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/)使用[AngleSharp](https://anglesharp.github.io/)剖析器來處理 antiforgery 檢查使用下列方法：

* `GetDocumentAsync` &ndash; 接收[HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) ，並傳回`IHtmlDocument`。 `GetDocumentAsync` 使用準備的處理站*虛擬回應*根據原始`HttpResponseMessage`。 如需詳細資訊，請參閱 [AngleSharp 文件](https://github.com/AngleSharp/AngleSharp#documentation)。
* `SendAsync` 擴充方法`HttpClient`compose [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)然後呼叫[SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)提交要求給 SUT。 多載`SendAsync`接受 HTML 表單 (`IHtmlFormElement`) 和下列：
  - 提交按鈕的表單 (`IHtmlElement`)
  - 表單值集合 (`IEnumerable<KeyValuePair<string, string>>`)
  - 提交按鈕 (`IHtmlElement`)，從而形成值 (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/)第三方剖析供示範之用，本主題中的範例應用程式的程式庫。 AngleSharp 不支援，或整合測試的 ASP.NET Core 應用程式所需。 其他剖析器可以使用，例如[Html 靈活度組件 (HAP)](http://html-agility-pack.net/)。 另一種方法是撰寫程式碼來直接處理防偽系統的要求驗證語彙基元和防偽 cookie。

## <a name="customize-the-client-with-withwebhostbuilder"></a>自訂 WithWebHostBuilder 的用戶端

在測試方法中，需要額外的設定時[WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)新建`WebApplicationFactory`具有[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ，進一步自訂組態。

`Post_DeleteMessageHandler_ReturnsRedirectToRoot`測試方法[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)示範如何使用`WithWebHostBuilder`。 這項測試觸發 SUT 中的送出表單，在資料庫中執行記錄刪除。

因為另一個測試中`IndexPageTests`類別執行作業會刪除所有資料庫中的記錄，並可能執行之前`Post_DeleteMessageHandler_ReturnsRedirectToRoot`方法中，資料庫就會在這個測試方法，以確保有一筆記錄的刪除 SUT 植入。 選取`deleteBtn1`按鈕的`messages`SUT 中的表單模擬 SUT 的要求中：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>用戶端選項

下表顯示的預設[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)建立時可以使用`HttpClient`執行個體。

| 選項 | 描述 | 預設 |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | 取得或設定是否`HttpClient`執行個體應該會自動遵循重新導向回應。 | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | 取得或設定基底位址`HttpClient`執行個體。 | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | 取得或設定是否`HttpClient`執行個體應該處理 cookie。 | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | 取得或設定最大重新導向回應數目`HttpClient`應該遵循執行個體。 | 7 |

建立`WebApplicationFactoryClientOptions`類別，並將它傳遞給[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)方法 （預設值在程式碼範例所示）：

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>插入模擬 （mock） 的服務

服務可以藉由呼叫測試中覆寫[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)主應用程式產生器。 **若要插入模擬 （mock） 的服務，必須有 SUT`Startup`類別搭配`Startup.ConfigureServices`方法。**

此範例 SUT 包含範圍的服務會傳回報價。 要求 索引 頁面時索引 頁面上的隱藏欄位中內嵌引號。

*Services/IQuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Services/QuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index.cshtml.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Pages/Index.cs*:

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

SUT 應用程式執行時，會產生下列標記：

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

若要測試服務和引號插入在整合測試，模擬 （mock） 的服務會插入 SUT 測試。 模擬 （mock） 的服務會取代應用程式的`QuoteService`測試應用程式所提供的服務，使用稱為`TestQuoteService`:

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices` 呼叫時，並已設定領域的服務註冊：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

測試執行期間所產生的標記會反映所提供的報價文字`TestQuoteService`，因此判斷提示的階段：

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>如何測試基礎結構會推斷的應用程式內容根路徑

`WebApplicationFactory`建構函式會藉由搜尋來推斷應用程式內容根路徑[WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)等於索引鍵包含整合測試的組件上`TEntryPoint`的組件`System.Reflection.Assembly.FullName`. 如果找不到具有正確金鑰的屬性，`WebApplicationFactory`回到搜尋解決方案檔案 (*\*.sln*)，並將附加`TEntryPoint`的方案目錄的組件名稱。 應用程式根目錄 （內容根路徑） 用來探索檢視和內容檔案。

在大部分情況下，不需要明確地將應用程式內容根目錄設定，因為通常在執行階段尋找正確的內容根目錄的搜尋邏輯。 未找到的內容根目錄的特殊案例中使用內建的搜尋演算法，內容可以指定根目錄，明確或使用自訂的邏輯應用程式。 若要設定應用程式的內容根目錄，在這些情況下，呼叫`UseSolutionRelativeContentRoot`擴充方法，從[Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost)封裝。 提供方案的相對路徑和選擇性的方案檔案名稱或 glob 模式 (預設 = `*.sln`)。

呼叫[UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot)擴充方法使用*一個*下列其中一個方法：

* 設定測試類別時`WebApplicationFactory`，提供的自訂組態[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):

   ```csharp
   public IndexPageTests(
       WebApplicationFactory<RazorPagesProject.Startup> factory)
   {
       var _factory = factory.WithWebHostBuilder(builder =>
       {
           builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

           ...
       });
   }
   ```

* 使用自訂設定測試類別時`WebApplicationFactory`，繼承自`WebApplicationFactory`，並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

   ```csharp
   public class CustomWebApplicationFactory<TStartup>
       : WebApplicationFactory<RazorPagesProject.Startup>
   {
       protected override void ConfigureWebHost(IWebHostBuilder builder)
       {
           builder.ConfigureServices(services =>
           {
               builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

               ...
           });
       }
   }
   ```

## <a name="disable-shadow-copying"></a>停用陰影複製

陰影複製會導致在不同的輸出資料夾的資料夾中執行測試。 針對測試才能正常運作，陰影複製必須停用。 [範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)xunit，並停用所包括的 xUnit 陰影複製*xunit.runner.json*檔案使用的是正確的組態設定。 如需詳細資訊，請參閱 <c0> [ 設定使用 JSON 的 xUnit.net](https://xunit.github.io/docs/configuring-with-json.html)。

新增*xunit.runner.json*檔案根目錄的測試專案，使用下列內容：

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>整合測試範例

[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)是兩個應用程式所組成：

| 應用程式 | 專案資料夾 | 描述 |
| --- | -------------- | ----------- |
| 訊息應用程式 (SUT) | *src/RazorPagesProject* | 允許使用者加入、 刪除其中一個、 全部刪除，以及分析訊息。 |
| 測試應用程式 | *tests/RazorPagesProject.Tests* | 用來整合測試 SUT。 |

可以使用內建的測試功能的 IDE，例如執行測試[Visual Studio](https://www.visualstudio.com/vs/)。 如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列中，執行下列命令在命令提示字元中*tests/RazorPagesProject.Tests*資料夾：

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>訊息應用程式 (SUT) 組織

SUT 是 Razor Pages 訊息系統具有下列特性：

* 應用程式的 [索引] 頁面 (*pages/Index.cshtml*並*Pages/Index.cshtml.cs*) 提供 UI 和頁面模型的方法，來控制新增、 刪除和分析的訊息 （每個訊息的平均字）.
* 所描述的訊息`Message`類別 (*Data/Message.cs*) 使用兩個屬性： `Id` （索引鍵） 和`Text`（訊息）。 `Text`屬性是必要的且限制為 200 個字元。
* 使用儲存的訊息[Entity Framework 的記憶體中的資料庫](/ef/core/providers/in-memory/)&#8224;。
* 應用程式在其資料庫內容類別中，包含資料存取層 (DAL) `AppDbContext` (*Data/AppDbContext.cs*)。
* 如果資料庫是空的應用程式啟動時，就會使用三個訊息初始化訊息存放區。
* 應用程式包含`/SecurePage`，只能存取已驗證使用者。

&#8224;EF 主題[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)，說明如何使用記憶體中資料庫的 mstest 執行測試。 本主題會使用[xUnit](https://xunit.github.io/)測試架構。 測試概念和跨不同的測試架構的測試實作都類似，但不是完全相同。

雖然不會使用應用程式[儲存機制模式](xref:fundamentals/repository-pattern)並不是有效的範例[工作單位 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)，Razor 頁面支援的開發這些模式。 如需詳細資訊，請參閱 <c0> [ 設計基礎結構持續層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)， [ASP.NET MVC 應用程式中實作存放庫和工作單元模式](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)，和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（此範例會實作儲存機制模式）。

### <a name="test-app-organization"></a>測試應用程式的組織

測試應用程式是主控台應用程式內*tests/RazorPagesProject.Tests*資料夾。

| 測試應用程式資料夾 | 描述 |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs*包含路由、 存取安全的頁面未驗證的使用者，並取得 GitHub 使用者設定檔和檢查設定檔的使用者登入的測試方法。 |
| *IntegrationTests* | *IndexPageTests.cs*包含 [索引] 頁面使用自訂的整合測試`WebApplicationFactory`類別。 |
| *協助程式/公用程式* | <ul><li>*Utilities.cs*包含`InitializeDbForTests`方法用來植入測試資料的資料庫。</li><li>*HtmlHelpers.cs*提供方法來傳回 AngleSharp`IHtmlDocument`以供測試方法。</li><li>*HttpClientExtensions.cs*提供的多載`SendAsync`提交要求給 SUT。</li></ul> |

測試架構[xUnit](https://xunit.github.io/)。 使用執行整合測試[Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost)，其中包括[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。 因為[Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)封裝用來設定測試主應用程式和測試伺服器`TestHost`和`TestServer`套件不需要在測試應用程式的專案檔中直接將封裝參考或在測試應用程式的開發人員設定。

**植入資料庫中的測試**

整合測試通常需要在測試執行前的資料庫中的小型資料集。 比方說，刪除測試會呼叫資料庫記錄的刪除，所以資料庫必須至少一個記錄才會成功的刪除要求。

範例應用程式會植入資料庫中的三個訊息*Utilities.cs*它們執行時，可以使用測試：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>其他資源

* [單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Razor 頁面單元測試](xref:test/razor-pages-tests)
* [中介軟體](xref:fundamentals/middleware/index)
* [測試控制器](xref:mvc/controllers/testing)
