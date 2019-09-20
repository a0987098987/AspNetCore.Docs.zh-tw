---
title: ASP.NET Core 中的整合測試
author: guardrex
description: 了解整合測試如何確保應用程式的元件在基礎結構層級 (包括資料庫、檔案系統和網路) 正確運作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2019
uid: test/integration-tests
ms.openlocfilehash: eba54b891c4f2d9876f9db5a3d0394990584c146
ms.sourcegitcommit: a11f09c10ef3d4eeab7ae9ce993e7f30427741c1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149412"
---
# <a name="integration-tests-in-aspnet-core"></a>ASP.NET Core 中的整合測試

作者： [Luke Latham](https://github.com/guardrex)和[Steve Smith](https://ardalis.com/)

::: moniker range=">= aspnetcore-3.0"

整合測試可確保應用程式的元件在包含應用程式支援基礎結構的層級上正確運作，例如資料庫、檔案系統和網路。 ASP.NET Core 支援使用單元測試架構搭配測試 web 主機和記憶體內部測試伺服器的整合測試。

本主題假設您對單元測試知識有基本了解。 如果不熟悉測試概念，請參閱 [.NET Core 與 .NET Standard 中的單元測試](/dotnet/core/testing/)主題和其連結的內容。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

範例應用程式是 Razor Pages 應用程式，並假設對 Razor Pages 有基本瞭解。 如果不熟悉 Razor Pages，請參閱下列主題：

* [Razor 頁面簡介](xref:razor-pages/index)
* [開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Razor 頁面單元測試](xref:test/razor-pages-tests)

> [!NOTE]
> 針對測試 Spa，建議使用[Selenium](https://www.seleniumhq.org/)這類工具，這可將瀏覽器自動化。

## <a name="introduction-to-integration-tests"></a>整合測試簡介

相較于[單元測試](/dotnet/core/testing/)，整合測試會在更廣泛的層級上評估應用程式的元件。 單元測試是用來測試隔離的軟體元件，例如個別的類別方法。 整合測試會確認兩個或多個應用程式元件一起運作，以產生預期的結果，可能包括完整處理要求所需的每個元件。

這些較廣泛的測試可用來測試應用程式的基礎結構和整個架構，通常包含下列元件：

* 資料庫
* 檔案系統
* 網路設備
* 要求-回應管線

單元測試會使用製造的元件（稱為*fakes*或模擬*物件*）來取代基礎結構元件。

相對於單元測試，整合測試：

* 使用應用程式在生產環境中使用的實際元件。
* 需要更多程式碼和資料處理。
* 執行較長的時間。

因此，請將整合測試的使用限制為最重要的基礎結構案例。 如果可以使用單元測試或整合測試來測試行為，請選擇單元測試。

> [!TIP]
> 請勿針對每個可能的資料和檔案存取，使用資料庫與檔案系統來撰寫整合測試。 無論應用程式之間的多少個位置與資料庫和檔案系統互動，一組專門的讀取、寫入、更新和刪除整合測試，通常都能夠適當地測試資料庫和檔案系統元件。 針對與這些元件互動的方法邏輯，使用單元測試進行常式測試。 在單元測試中的基礎結構使用 fakes/模擬 (mock) 更快速的測試執行中的結果。

> [!NOTE]
> 在整合測試的討論中，測試過的專案經常稱為受測*系統*，或簡稱「SUT」。
>
> *本主題中會使用「SUT」來參考已測試的 ASP.NET Core 應用程式。*

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core 整合測試

ASP.NET Core 中的整合測試需要下列各項：

* 測試專案會用來包含和執行測試。 測試專案具有該 SUT 的參考。
* 測試專案會建立適用于 SUT 的測試 web 主機，並使用測試伺服器用戶端來處理與 SUT 的要求和回應。
* 測試執行器是用來執行測試並報告測試結果。

整合測試會遵循包含一般*排列*、 *Act*和判斷*提示測試步驟*的一連串事件：

1. 已設定了 SUT 的 web 主機。
1. 建立測試伺服器用戶端，以提交要求給應用程式。
1. 執行*排列*測試步驟：測試應用程式會準備要求。
1. 執行*Act*測試步驟：用戶端會提交要求並接收回應。
1. 會執行*Assert*測試步驟：*實際*的回應會根據*預期*的回應而驗證為*通過*或*失敗*。
1. 程式會繼續進行，直到執行所有測試為止。
1. 測試結果會報告。

測試 web 主機的設定通常與測試回合的應用程式一般 web 主機不同。 例如，測試可能會使用不同的資料庫或不同的應用程式設定。

基礎結構元件（例如測試 web 主機和記憶體內部測試伺服器（[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)））是由[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)所提供或管理。 使用此封裝會簡化建立和執行的測試。

此`Microsoft.AspNetCore.Mvc.Testing`套件會處理下列工作：

* 將相依性檔案（ *. .deps.json*）從 SUT 複製到測試專案的*bin*目錄。
* 將內容根目錄設定為 SUT 的專案根目錄，以便在執行測試時找到靜態檔案和頁面/瀏覽器。
* 提供[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類別，以簡化使用`TestServer`來啟動的 SUT。

[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)檔描述如何設定測試專案和測試執行器，以及如何執行測試和建議如何命名測試和測試類別的詳細指示。

> [!NOTE]
> 建立應用程式的測試專案時，請將單元測試從整合測試分成不同的專案。 這有助於確保基礎結構測試元件不會不小心包含在單元測試中。 區隔單元和整合測試也可以控制要執行哪一組測試。

Razor Pages 應用程式和 MVC 應用程式的測試設定之間幾乎沒有任何差異。 唯一的差異在於測試的命名方式。 在 Razor Pages 應用程式中，頁面端點的測試通常是在頁面模型類別之後命名（例如， `IndexPageTests`用來測試索引頁面的元件整合）。 在 MVC 應用程式中，通常會以控制器類別來組織測試，並在其測試的控制器之後命名`HomeControllerTests` （例如，測試主控制器的元件整合）。

## <a name="test-app-prerequisites"></a>測試應用程式必要條件

測試專案必須：

* 參考[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)的封裝。
* 在專案檔中指定 Web SDK （`<Project Sdk="Microsoft.NET.Sdk.Web">`）。

這些必要條件可在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中看到。 檢查 [*測試]/[RazorPagesProject]/* [測試]/[RazorPagesProject]。 範例應用程式會使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)剖析器程式庫，因此範例應用程式也會參考：

* [xunit](https://www.nuget.org/packages/xunit)
* [xunit.runner.visualstudio](https://www.nuget.org/packages/xunit.runner.visualstudio)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp)

Entity Framework Core 也會用於測試中。 應用程式參考：

* [AspNetCore. Microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore)
* [AspNetCore. Identity. Microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity.EntityFrameworkCore)
* [Microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore)
* [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)
* [Microsoft.entityframeworkcore 工具](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)

## <a name="sut-environment"></a>SUT 環境

如果未設定了 SUT 的[環境](xref:fundamentals/environments)，則環境會預設為開發。

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>使用預設 WebApplicationFactory 的基本測試

[WebApplicationFactory\<TEntryPoint >](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)用來建立整合測試的[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) 。 `TEntryPoint`是 SUT 的進入點類別，通常`Startup`是類別。

測試類別會實作為類別*裝置*介面（[IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)），以指示類別包含測試，並在類別中的所有測試中提供共用物件實例。

### <a name="basic-test-of-app-endpoints"></a>應用程式端點的基本測試

下列測試類別`BasicTests`會`WebApplicationFactory`使用來啟動 SUT 並提供`Get_EndpointsReturnSuccessAndCorrectContentType` [HttpClient](/dotnet/api/system.net.http.httpclient)給測試方法。 方法會檢查回應狀態碼是否成功（狀態碼的範圍是200-299），而`Content-Type`標頭則`text/html; charset=utf-8`適用于數個應用程式頁面。

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)會建立自動遵循`HttpClient`重新導向和處理 cookie 的實例。

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

根據預設，當啟用[GDPR 同意原則](xref:security/gdpr)時，不會在要求之間保留非必要的 cookie。 若要保留非必要的 cookie （例如 TempData 提供者所使用的 cookie），請在您的測試中將它們標示為必要。 如需將 cookie 標示為必要的指示，請參閱[基本 cookie](xref:security/gdpr#essential-cookies)。

### <a name="test-a-secure-endpoint"></a>測試安全端點

類別中的`BasicTests`另一項測試會檢查安全端點是否將未驗證的使用者重新導向至應用程式的登入頁面。

在 SUT 中， `/SecurePage`頁面會使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)慣例將[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)套用至頁面。 如需詳細資訊，請參閱  [Razor Pages 授權慣例](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

在測試中, [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)會設定為不允許重新導向, 方法是`false`將 [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) 設為:  `Get_SecurePageRequiresAnAuthenticatedUser`

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

藉由不允許用戶端遵循重新導向，您可以進行下列檢查：

* 您可以根據預期的 HttpStatusCode 檢查由 SUT 所傳回的狀態碼。重新導向至登入頁面（可能是[HttpStatusCode](/dotnet/api/system.net.httpstatuscode)）之後，重新[導向](/dotnet/api/system.net.httpstatuscode)結果，而不是最終狀態代碼。
* 系統`Location`會檢查回應標頭中的標頭值`http://localhost/Identity/Account/Login`，以確認其開頭為，而不是最終的登入`Location`頁面回應，其中標頭不存在。

如需詳細資訊`WebApplicationFactoryClientOptions`，請參閱 <c2> [ 用戶端選項](#client-options)一節。

## <a name="customize-webapplicationfactory"></a>自訂 WebApplicationFactory

藉由繼承`WebApplicationFactory`自來建立一或多個自訂處理站，可以獨立于測試類別建立 Web 主機設定：

1. 繼承自`WebApplicationFactory` ，並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)來設定服務集合：

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   [範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)中的`InitializeDbForTests`資料庫植入是由方法執行。 [整合測試範例中會說明方法：測試應用程式](#test-app-organization)組織區段。

2. 使用測試類別`CustomWebApplicationFactory`中的自訂。 下列範例會在`IndexPageTests`類別中使用 factory：

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   範例應用程式的用戶端設定為防止`HttpClient`下列重新導向。 如[測試安全端點](#test-a-secure-endpoint)一節中所述，這會允許測試檢查應用程式第一個回應的結果。 第一個回應是其中許多測試中具有`Location`標頭的重新導向。

3. 一般測試會使用`HttpClient`和 helper 方法來處理要求和回應：

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

任何對 SUT 的 POST 要求都必須滿足應用程式的[資料保護 antiforgery 系統](xref:security/data-protection/introduction)自動進行的 antiforgery 檢查。 為了安排測試的 POST 要求，測試應用程式必須：

1. 提出頁面的要求。
1. 剖析 antiforgery cookie，並從回應要求驗證權杖。
1. 提出具有 antiforgery cookie 的 POST 要求，並要求驗證權杖。

`GetDocumentAsync`  在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中, helper擴充方法(helper/HttpClientExtensions)和helper方法(helper/HtmlHelpers)會使用[AngleSharp](https://anglesharp.github.io/)剖析器來`SendAsync`處理 antiforgery請使用下列方法進行檢查:

* `GetDocumentAsync`接收 [HttpResponseMessage ](/dotnet/api/system.net.http.httpresponsemessage) , 並`IHtmlDocument`傳回&ndash;。 `GetDocumentAsync`使用可根據原始`HttpResponseMessage`的來準備*虛擬回應*的 factory。 如需詳細資訊，請參閱 [AngleSharp 文件](https://github.com/AngleSharp/AngleSharp#documentation)。
* `SendAsync``HttpClient`撰寫[HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)和呼叫[SendAsync （HttpRequestMessage）](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)的擴充方法，以將要求提交至 SUT。 的多載會`IHtmlFormElement`接受HTML表單（）和下列內容：`SendAsync`
  * 表單的 [提交]`IHtmlElement`按鈕（）
  * 表單值集合`IEnumerable<KeyValuePair<string, string>>`（）
  * 提交按鈕（`IHtmlElement`）和表單值`IEnumerable<KeyValuePair<string, string>>`（）

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/)是協力廠商剖析程式庫，用於本主題和範例應用程式中的示範用途。 ASP.NET Core 應用程式的整合測試不支援或不需要 AngleSharp。 您可以使用其他剖析器，例如[Html 靈活性套件（HAP）](https://html-agility-pack.net/)。 另一種方法是撰寫程式碼，直接處理 antiforgery 系統的要求驗證權杖和 antiforgery cookie。

## <a name="customize-the-client-with-withwebhostbuilder"></a>使用 WithWebHostBuilder 自訂用戶端

當測試方法中需要額外的設定時， [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)會使用設定`WebApplicationFactory`進一步自訂的[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ，建立新的。

`Post_DeleteMessageHandler_ReturnsRedirectToRoot` [範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)的測試方法會`WithWebHostBuilder`示範的用法。 這項測試會藉由觸發在 SUT 中提交表單的方式，在資料庫中執行記錄刪除。

因為`IndexPageTests`類別中的另一項測試會執行刪除資料庫中所有記錄的作業，而且可能會在此`Post_DeleteMessageHandler_ReturnsRedirectToRoot`方法之前執行，所以會在此測試方法中重新植入資料庫，以確保有記錄存在，供 SUT 刪除。 在 sut 的要求中，選取`messages`該表單的第一個 [刪除] 按鈕會進行模擬：

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>用戶端選項

下表顯示建立`HttpClient`實例時可用的預設[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) 。

| 選項 | 描述 | 預設 |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | 取得或設定`HttpClient`實例是否應該自動遵循重新導向回應。 | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | 取得或設定`HttpClient`實例的基底位址。 | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | 取得或設定實例`HttpClient`是否應處理 cookie。 | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | 取得或設定`HttpClient`實例應遵循的重新導向回應數目上限。 | 7 |

建立類別, 並將它傳遞給 [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) 方法 (程式碼範例中會顯示預設值):  `WebApplicationFactoryClientOptions`

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>插入 mock 服務

您可以在主機建立器上呼叫[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) ，以在測試中覆寫服務。 **若要插入模擬服務，SUT 必須具有`Startup` `Startup.ConfigureServices`具有方法的類別。**

範例 SUT 包含會傳回報價的範圍服務。 當要求索引頁時，引號會內嵌在 [索引] 頁面上的隱藏欄位中。

*Services/IQuoteService .cs*：

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Services/QuoteService .cs*：

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*：

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index.cshtml.cs*：

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Pages/Index .cs*：

[!code-cshtml[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

執行 SUT 應用程式時，會產生下列標記：

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

若要測試服務並在整合測試中加上引號，則測試會將模擬服務插入至 SUT。 Mock 服務會將應用程式的`QuoteService`取代為測試應用程式所提供的服務， `TestQuoteService`稱為：

*IntegrationTests.IndexPageTests.cs*：

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices`會呼叫，並註冊已設定範圍的服務：

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

在測試執行期間產生的標記會反映所提供`TestQuoteService`的引號文字，因此判斷提示會通過：

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>測試基礎結構如何推斷應用程式內容根路徑

此`WebApplicationFactory`函式會藉由搜尋元件上的[WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)來推斷應用程式內容根路徑，其中包含的索引`TEntryPoint`鍵等於元件`System.Reflection.Assembly.FullName`的整合測試。 如果找不到具有正確索引鍵的屬性， `WebApplicationFactory`就會回到搜尋方案檔（ *.sln* `TEntryPoint` ），並將元件名稱附加至方案目錄。 應用程式根目錄（內容根路徑）是用來探索 views 和內容檔案。

## <a name="disable-shadow-copying"></a>停用陰影複製

陰影複製會使測試在與輸出目錄不同的目錄中執行。 若要讓測試正常運作，必須停用陰影複製。 [範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)會使用 xUnit，並藉由包含具有正確設定的*xUnit*檔案，來停用 xUnit 的陰影複製。 如需詳細資訊，請參閱 <c0> [ 設定使用 JSON 的 xUnit.net](https://xunit.github.io/docs/configuring-with-json.html)。

將*xunit*檔案新增至測試專案的根目錄，並包含下列內容：

```json
{
  "shadowCopy": false
}
```

## <a name="disposal-of-objects"></a>物件的處置

執行`IClassFixture`此實作為測試之後，當 xUnit 處置[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)時，會處置[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)和[HttpClient](/dotnet/api/system.net.http.httpclient) 。 如果開發人員所具現化的物件需要處置，請在`IClassFixture`執行時處置它們。 如需詳細資訊，請參閱[執行 Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。

## <a name="integration-tests-sample"></a>整合測試範例

[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)是由兩個應用程式所組成：

| App | 專案目錄 | 描述 |
| --- | ----------------- | ----------- |
| 訊息應用程式（SUT） | *src/RazorPagesProject* | 可讓使用者加入、刪除一個、刪除全部及分析訊息。 |
| 測試應用程式 | *tests/RazorPagesProject.Tests* | 用來整合測試 SUT。 |

測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](https://visualstudio.microsoft.com)。 如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [測試]/[RazorPagesProject] 的命令提示字元中執行下列命令 *。測試*目錄：

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>訊息應用程式（SUT）組織

SUT 是具有下列特性的 Razor Pages 訊息系統：

* 應用程式的 [索引] 頁面（*pages/index. cshtml*和*pages/Index. CSHTML*）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（每個訊息的平均單字）。
* 訊息是由`Message`類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （key）和`Text` （message）。 屬性`Text`是必要的，而且限制為200個字元。
* 訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224;來儲存。
* 應用程式在其資料庫內容類別`AppDbContext` （*data/AppDbCoNtext .cs*）中包含資料存取層（DAL）。
* 如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。
* 應用程式包含`/SecurePage`只能由已驗證的使用者存取的。

&#8224;EF 主題「[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)」說明如何使用記憶體內部資料庫，以搭配 MSTest 進行測試。 本主題使用[xUnit](https://xunit.github.io/)測試架構。 跨不同測試架構的測試概念和測試執行類似，但不完全相同。

雖然應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，Razor Pages 支援這些開發模式。 如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（範例會執行存放庫模式）。

### <a name="test-app-organization"></a>測試應用程式組織

測試應用程式是 [*測試/RazorPagesProject* ] 目錄中的主控台應用程式。

| 測試應用程式目錄 | 說明 |
| ------------------ | ----------- |
| *BasicTests* | *BasicTests.cs*包含用於路由的測試方法、透過未經驗證的使用者存取安全頁面，以及取得 GitHub 使用者設定檔，並檢查設定檔的使用者登入。 |
| *IntegrationTests* | *IndexPageTests.cs*包含使用自訂`WebApplicationFactory`類別之索引頁面的整合測試。 |
| *Helper/公用程式* | <ul><li>*Utilities.cs*包含`InitializeDbForTests`用來植入具有測試資料之資料庫的方法。</li><li>*HtmlHelpers.cs*提供方法來傳回 AngleSharp `IHtmlDocument`供測試方法使用。</li><li>*HttpClientExtensions.cs*提供的`SendAsync`多載，以將要求提交至 SUT。</li></ul> |

測試架構為[xUnit](https://xunit.github.io/)。 整合測試會使用[AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost)進行，其中包含[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。 由於[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)會使用測試封裝來設定測試主機和測試伺服器， `TestHost`因此和`TestServer`封裝不需要在測試應用程式的專案檔或開發人員中直接封裝參考測試應用程式中的設定。

**植入資料庫進行測試**

在測試執行之前，整合測試通常需要資料庫中的小型資料集。 例如，刪除測試會呼叫資料庫記錄，因此資料庫必須至少有一筆記錄，刪除要求才會成功。

範例應用程式會在*Utilities.cs*中植入具有三個訊息的資料庫，測試可以在執行時使用：

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

整合測試可確保應用程式的元件在包含應用程式支援基礎結構的層級上正確運作，例如資料庫、檔案系統和網路。 ASP.NET Core 支援使用單元測試架構搭配測試 web 主機和記憶體內部測試伺服器的整合測試。

本主題假設您對單元測試知識有基本了解。 如果不熟悉測試概念，請參閱 [.NET Core 與 .NET Standard 中的單元測試](/dotnet/core/testing/)主題和其連結的內容。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

範例應用程式是 Razor Pages 應用程式，並假設對 Razor Pages 有基本瞭解。 如果不熟悉 Razor Pages，請參閱下列主題：

* [Razor 頁面簡介](xref:razor-pages/index)
* [開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Razor 頁面單元測試](xref:test/razor-pages-tests)

> [!NOTE]
> 針對測試 Spa，建議使用[Selenium](https://www.seleniumhq.org/)這類工具，這可將瀏覽器自動化。

## <a name="introduction-to-integration-tests"></a>整合測試簡介

相較于[單元測試](/dotnet/core/testing/)，整合測試會在更廣泛的層級上評估應用程式的元件。 單元測試是用來測試隔離的軟體元件，例如個別的類別方法。 整合測試會確認兩個或多個應用程式元件一起運作，以產生預期的結果，可能包括完整處理要求所需的每個元件。

這些較廣泛的測試可用來測試應用程式的基礎結構和整個架構，通常包含下列元件：

* 資料庫
* 檔案系統
* 網路設備
* 要求-回應管線

單元測試會使用製造的元件（稱為*fakes*或模擬*物件*）來取代基礎結構元件。

相對於單元測試，整合測試：

* 使用應用程式在生產環境中使用的實際元件。
* 需要更多程式碼和資料處理。
* 執行較長的時間。

因此，請將整合測試的使用限制為最重要的基礎結構案例。 如果可以使用單元測試或整合測試來測試行為，請選擇單元測試。

> [!TIP]
> 請勿針對每個可能的資料和檔案存取，使用資料庫與檔案系統來撰寫整合測試。 無論應用程式之間的多少個位置與資料庫和檔案系統互動，一組專門的讀取、寫入、更新和刪除整合測試，通常都能夠適當地測試資料庫和檔案系統元件。 針對與這些元件互動的方法邏輯，使用單元測試進行常式測試。 在單元測試中的基礎結構使用 fakes/模擬 (mock) 更快速的測試執行中的結果。

> [!NOTE]
> 在整合測試的討論中，測試過的專案經常稱為受測*系統*，或簡稱「SUT」。
>
> *本主題中會使用「SUT」來參考已測試的 ASP.NET Core 應用程式。*

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core 整合測試

ASP.NET Core 中的整合測試需要下列各項：

* 測試專案會用來包含和執行測試。 測試專案具有該 SUT 的參考。
* 測試專案會建立適用于 SUT 的測試 web 主機，並使用測試伺服器用戶端來處理與 SUT 的要求和回應。
* 測試執行器是用來執行測試並報告測試結果。

整合測試會遵循包含一般*排列*、 *Act*和判斷*提示測試步驟*的一連串事件：

1. 已設定了 SUT 的 web 主機。
1. 建立測試伺服器用戶端，以提交要求給應用程式。
1. 執行*排列*測試步驟：測試應用程式會準備要求。
1. 執行*Act*測試步驟：用戶端會提交要求並接收回應。
1. 會執行*Assert*測試步驟：*實際*的回應會根據*預期*的回應而驗證為*通過*或*失敗*。
1. 程式會繼續進行，直到執行所有測試為止。
1. 測試結果會報告。

測試 web 主機的設定通常與測試回合的應用程式一般 web 主機不同。 例如，測試可能會使用不同的資料庫或不同的應用程式設定。

基礎結構元件（例如測試 web 主機和記憶體內部測試伺服器（[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)））是由[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)所提供或管理。 使用此封裝會簡化建立和執行的測試。

此`Microsoft.AspNetCore.Mvc.Testing`套件會處理下列工作：

* 將相依性檔案（ *. .deps.json*）從 SUT 複製到測試專案的*bin*目錄。
* 將內容根目錄設定為 SUT 的專案根目錄，以便在執行測試時找到靜態檔案和頁面/瀏覽器。
* 提供[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類別，以簡化使用`TestServer`來啟動的 SUT。

[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)檔描述如何設定測試專案和測試執行器，以及如何執行測試和建議如何命名測試和測試類別的詳細指示。

> [!NOTE]
> 建立應用程式的測試專案時，請將單元測試從整合測試分成不同的專案。 這有助於確保基礎結構測試元件不會不小心包含在單元測試中。 區隔單元和整合測試也可以控制要執行哪一組測試。

Razor Pages 應用程式和 MVC 應用程式的測試設定之間幾乎沒有任何差異。 唯一的差異在於測試的命名方式。 在 Razor Pages 應用程式中，頁面端點的測試通常是在頁面模型類別之後命名（例如， `IndexPageTests`用來測試索引頁面的元件整合）。 在 MVC 應用程式中，通常會以控制器類別來組織測試，並在其測試的控制器之後命名`HomeControllerTests` （例如，測試主控制器的元件整合）。

## <a name="test-app-prerequisites"></a>測試應用程式必要條件

測試專案必須：

* 參考下列套件：
  * [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* 在專案檔中指定 Web SDK （`<Project Sdk="Microsoft.NET.Sdk.Web">`）。 參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)時，需要 Web SDK。

這些必要條件可在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中看到。 檢查 [*測試]/[RazorPagesProject]/* [測試]/[RazorPagesProject]。 範例應用程式會使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)剖析器程式庫，因此範例應用程式也會參考：

* [xunit](https://www.nuget.org/packages/xunit/)
* [xunit.runner.visualstudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a>SUT 環境

如果未設定了 SUT 的[環境](xref:fundamentals/environments)，則環境會預設為開發。

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>使用預設 WebApplicationFactory 的基本測試

[WebApplicationFactory\<TEntryPoint >](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)用來建立整合測試的[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) 。 `TEntryPoint`是 SUT 的進入點類別，通常`Startup`是類別。

測試類別會實作為類別*裝置*介面（[IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)），以指示類別包含測試，並在類別中的所有測試中提供共用物件實例。

### <a name="basic-test-of-app-endpoints"></a>應用程式端點的基本測試

下列測試類別`BasicTests`會`WebApplicationFactory`使用來啟動 SUT 並提供`Get_EndpointsReturnSuccessAndCorrectContentType` [HttpClient](/dotnet/api/system.net.http.httpclient)給測試方法。 方法會檢查回應狀態碼是否成功（狀態碼的範圍是200-299），而`Content-Type`標頭則`text/html; charset=utf-8`適用于數個應用程式頁面。

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)會建立自動遵循`HttpClient`重新導向和處理 cookie 的實例。

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

根據預設，當啟用[GDPR 同意原則](xref:security/gdpr)時，不會在要求之間保留非必要的 cookie。 若要保留非必要的 cookie （例如 TempData 提供者所使用的 cookie），請在您的測試中將它們標示為必要。 如需將 cookie 標示為必要的指示，請參閱[基本 cookie](xref:security/gdpr#essential-cookies)。

### <a name="test-a-secure-endpoint"></a>測試安全端點

類別中的`BasicTests`另一項測試會檢查安全端點是否將未驗證的使用者重新導向至應用程式的登入頁面。

在 SUT 中， `/SecurePage`頁面會使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)慣例將[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)套用至頁面。 如需詳細資訊，請參閱  [Razor Pages 授權慣例](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

在測試中, [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)會設定為不允許重新導向, 方法是`false`將 [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) 設為:  `Get_SecurePageRequiresAnAuthenticatedUser`

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

藉由不允許用戶端遵循重新導向，您可以進行下列檢查：

* 您可以根據預期的 HttpStatusCode 檢查由 SUT 所傳回的狀態碼。重新導向至登入頁面（可能是[HttpStatusCode](/dotnet/api/system.net.httpstatuscode)）之後，重新[導向](/dotnet/api/system.net.httpstatuscode)結果，而不是最終狀態代碼。
* 系統`Location`會檢查回應標頭中的標頭值`http://localhost/Identity/Account/Login`，以確認其開頭為，而不是最終的登入`Location`頁面回應，其中標頭不存在。

如需詳細資訊`WebApplicationFactoryClientOptions`，請參閱 <c2> [ 用戶端選項](#client-options)一節。

## <a name="customize-webapplicationfactory"></a>自訂 WebApplicationFactory

藉由繼承`WebApplicationFactory`自來建立一或多個自訂處理站，可以獨立于測試類別建立 Web 主機設定：

1. 繼承自`WebApplicationFactory` ，並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)來設定服務集合：

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   [範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)中的`InitializeDbForTests`資料庫植入是由方法執行。 [整合測試範例中會說明方法：測試應用程式](#test-app-organization)組織區段。

2. 使用測試類別`CustomWebApplicationFactory`中的自訂。 下列範例會在`IndexPageTests`類別中使用 factory：

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   範例應用程式的用戶端設定為防止`HttpClient`下列重新導向。 如[測試安全端點](#test-a-secure-endpoint)一節中所述，這會允許測試檢查應用程式第一個回應的結果。 第一個回應是其中許多測試中具有`Location`標頭的重新導向。

3. 一般測試會使用`HttpClient`和 helper 方法來處理要求和回應：

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

任何對 SUT 的 POST 要求都必須滿足應用程式的[資料保護 antiforgery 系統](xref:security/data-protection/introduction)自動進行的 antiforgery 檢查。 為了安排測試的 POST 要求，測試應用程式必須：

1. 提出頁面的要求。
1. 剖析 antiforgery cookie，並從回應要求驗證權杖。
1. 提出具有 antiforgery cookie 的 POST 要求，並要求驗證權杖。

`GetDocumentAsync`  在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中, helper擴充方法(helper/HttpClientExtensions)和helper方法(helper/HtmlHelpers)會使用[AngleSharp](https://anglesharp.github.io/)剖析器來`SendAsync`處理 antiforgery請使用下列方法進行檢查:

* `GetDocumentAsync`接收 [HttpResponseMessage ](/dotnet/api/system.net.http.httpresponsemessage) , 並`IHtmlDocument`傳回&ndash;。 `GetDocumentAsync`使用可根據原始`HttpResponseMessage`的來準備*虛擬回應*的 factory。 如需詳細資訊，請參閱 [AngleSharp 文件](https://github.com/AngleSharp/AngleSharp#documentation)。
* `SendAsync``HttpClient`撰寫[HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)和呼叫[SendAsync （HttpRequestMessage）](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)的擴充方法，以將要求提交至 SUT。 的多載會`IHtmlFormElement`接受HTML表單（）和下列內容：`SendAsync`
  * 表單的 [提交]`IHtmlElement`按鈕（）
  * 表單值集合`IEnumerable<KeyValuePair<string, string>>`（）
  * 提交按鈕（`IHtmlElement`）和表單值`IEnumerable<KeyValuePair<string, string>>`（）

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/)是協力廠商剖析程式庫，用於本主題和範例應用程式中的示範用途。 ASP.NET Core 應用程式的整合測試不支援或不需要 AngleSharp。 您可以使用其他剖析器，例如[Html 靈活性套件（HAP）](https://html-agility-pack.net/)。 另一種方法是撰寫程式碼，直接處理 antiforgery 系統的要求驗證權杖和 antiforgery cookie。

## <a name="customize-the-client-with-withwebhostbuilder"></a>使用 WithWebHostBuilder 自訂用戶端

當測試方法中需要額外的設定時， [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)會使用設定`WebApplicationFactory`進一步自訂的[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ，建立新的。

`Post_DeleteMessageHandler_ReturnsRedirectToRoot` [範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)的測試方法會`WithWebHostBuilder`示範的用法。 這項測試會藉由觸發在 SUT 中提交表單的方式，在資料庫中執行記錄刪除。

因為`IndexPageTests`類別中的另一項測試會執行刪除資料庫中所有記錄的作業，而且可能會在此`Post_DeleteMessageHandler_ReturnsRedirectToRoot`方法之前執行，所以會在此測試方法中重新植入資料庫，以確保有記錄存在，供 SUT 刪除。 在 sut 的要求中，選取`messages`該表單的第一個 [刪除] 按鈕會進行模擬：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>用戶端選項

下表顯示建立`HttpClient`實例時可用的預設[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) 。

| 選項 | 描述 | 預設 |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | 取得或設定`HttpClient`實例是否應該自動遵循重新導向回應。 | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | 取得或設定`HttpClient`實例的基底位址。 | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | 取得或設定實例`HttpClient`是否應處理 cookie。 | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | 取得或設定`HttpClient`實例應遵循的重新導向回應數目上限。 | 7 |

建立類別, 並將它傳遞給 [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) 方法 (程式碼範例中會顯示預設值):  `WebApplicationFactoryClientOptions`

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>插入 mock 服務

您可以在主機建立器上呼叫[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) ，以在測試中覆寫服務。 **若要插入模擬服務，SUT 必須具有`Startup` `Startup.ConfigureServices`具有方法的類別。**

範例 SUT 包含會傳回報價的範圍服務。 當要求索引頁時，引號會內嵌在 [索引] 頁面上的隱藏欄位中。

*Services/IQuoteService .cs*：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Services/QuoteService .cs*：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index.cshtml.cs*：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Pages/Index .cs*：

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

執行 SUT 應用程式時，會產生下列標記：

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

若要測試服務並在整合測試中加上引號，則測試會將模擬服務插入至 SUT。 Mock 服務會將應用程式的`QuoteService`取代為測試應用程式所提供的服務， `TestQuoteService`稱為：

*IntegrationTests.IndexPageTests.cs*：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices`會呼叫，並註冊已設定範圍的服務：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

在測試執行期間產生的標記會反映所提供`TestQuoteService`的引號文字，因此判斷提示會通過：

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>測試基礎結構如何推斷應用程式內容根路徑

此`WebApplicationFactory`函式會藉由搜尋元件上的[WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)來推斷應用程式內容根路徑，其中包含的索引`TEntryPoint`鍵等於元件`System.Reflection.Assembly.FullName`的整合測試。 如果找不到具有正確索引鍵的屬性， `WebApplicationFactory`就會回到搜尋方案檔（ *.sln* `TEntryPoint` ），並將元件名稱附加至方案目錄。 應用程式根目錄（內容根路徑）是用來探索 views 和內容檔案。

## <a name="disable-shadow-copying"></a>停用陰影複製

陰影複製會使測試在與輸出目錄不同的目錄中執行。 若要讓測試正常運作，必須停用陰影複製。 [範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)會使用 xUnit，並藉由包含具有正確設定的*xUnit*檔案，來停用 xUnit 的陰影複製。 如需詳細資訊，請參閱 <c0> [ 設定使用 JSON 的 xUnit.net](https://xunit.github.io/docs/configuring-with-json.html)。

將*xunit*檔案新增至測試專案的根目錄，並包含下列內容：

```json
{
  "shadowCopy": false
}
```

如果使用 Visual Studio，請將檔案的 [**複製到輸出目錄**] 屬性設定為 [**永遠複製**]。 如果未使用 Visual Studio，請將`Content`目標新增至測試應用程式的專案檔：

```xml
<ItemGroup>
  <Content Update="xunit.runner.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </Content>
</ItemGroup>
```

## <a name="disposal-of-objects"></a>物件的處置

執行`IClassFixture`此實作為測試之後，當 xUnit 處置[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)時，會處置[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)和[HttpClient](/dotnet/api/system.net.http.httpclient) 。 如果開發人員所具現化的物件需要處置，請在`IClassFixture`執行時處置它們。 如需詳細資訊，請參閱[執行 Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。

## <a name="integration-tests-sample"></a>整合測試範例

[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)是由兩個應用程式所組成：

| App | 專案目錄 | 描述 |
| --- | ----------------- | ----------- |
| 訊息應用程式（SUT） | *src/RazorPagesProject* | 可讓使用者加入、刪除一個、刪除全部及分析訊息。 |
| 測試應用程式 | *tests/RazorPagesProject.Tests* | 用來整合測試 SUT。 |

測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](https://visualstudio.microsoft.com)。 如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [測試]/[RazorPagesProject] 的命令提示字元中執行下列命令 *。測試*目錄：

```dotnetcli
dotnet test
```

### <a name="message-app-sut-organization"></a>訊息應用程式（SUT）組織

SUT 是具有下列特性的 Razor Pages 訊息系統：

* 應用程式的 [索引] 頁面（*pages/index. cshtml*和*pages/Index. CSHTML*）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（每個訊息的平均單字）。
* 訊息是由`Message`類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （key）和`Text` （message）。 屬性`Text`是必要的，而且限制為200個字元。
* 訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224;來儲存。
* 應用程式在其資料庫內容類別`AppDbContext` （*data/AppDbCoNtext .cs*）中包含資料存取層（DAL）。
* 如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。
* 應用程式包含`/SecurePage`只能由已驗證的使用者存取的。

&#8224;EF 主題「[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)」說明如何使用記憶體內部資料庫，以搭配 MSTest 進行測試。 本主題使用[xUnit](https://xunit.github.io/)測試架構。 跨不同測試架構的測試概念和測試執行類似，但不完全相同。

雖然應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，Razor Pages 支援這些開發模式。 如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（範例會執行存放庫模式）。

### <a name="test-app-organization"></a>測試應用程式組織

測試應用程式是 [*測試/RazorPagesProject* ] 目錄中的主控台應用程式。

| 測試應用程式目錄 | 說明 |
| ------------------ | ----------- |
| *BasicTests* | *BasicTests.cs*包含用於路由的測試方法、透過未經驗證的使用者存取安全頁面，以及取得 GitHub 使用者設定檔，並檢查設定檔的使用者登入。 |
| *IntegrationTests* | *IndexPageTests.cs*包含使用自訂`WebApplicationFactory`類別之索引頁面的整合測試。 |
| *Helper/公用程式* | <ul><li>*Utilities.cs*包含`InitializeDbForTests`用來植入具有測試資料之資料庫的方法。</li><li>*HtmlHelpers.cs*提供方法來傳回 AngleSharp `IHtmlDocument`供測試方法使用。</li><li>*HttpClientExtensions.cs*提供的`SendAsync`多載，以將要求提交至 SUT。</li></ul> |

測試架構為[xUnit](https://xunit.github.io/)。 整合測試會使用[AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost)進行，其中包含[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。 由於[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)會使用測試封裝來設定測試主機和測試伺服器， `TestHost`因此和`TestServer`封裝不需要在測試應用程式的專案檔或開發人員中直接封裝參考測試應用程式中的設定。

**植入資料庫進行測試**

在測試執行之前，整合測試通常需要資料庫中的小型資料集。 例如，刪除測試會呼叫資料庫記錄，因此資料庫必須至少有一筆記錄，刪除要求才會成功。

範例應用程式會在*Utilities.cs*中植入具有三個訊息的資料庫，測試可以在執行時使用：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Razor 頁面單元測試](xref:test/razor-pages-tests)
* [中介軟體](xref:fundamentals/middleware/index)
* [測試控制器](xref:mvc/controllers/testing)
