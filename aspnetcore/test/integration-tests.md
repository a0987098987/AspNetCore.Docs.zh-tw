---
title: 整合測試中 ASP.NET Core
author: guardrex
description: 了解整合測試如何確保應用程式的元件在基礎結構層級 (包括資料庫、檔案系統和網路) 正確運作。
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: test/integration-tests
ms.openlocfilehash: 1895b06f1af9a9eb66c14aa5c7834497fc95d583
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277692"
---
# <a name="integration-tests-in-aspnet-core"></a>整合測試中 ASP.NET Core

由[Luke Latham](https://github.com/guardrex)和[Steve Smith](https://ardalis.com/)

整合測試，確保應用程式的元件正確運作，其中包含應用程式的支援基礎結構，例如資料庫、 檔案系統和網路層級。 ASP.NET Core 支援整合測試與測試 web 主機和記憶體中的測試伺服器使用的單元測試架構。

本主題假設單元測試的基本了的解。 如果不熟悉測試概念，請參閱[.NET Core 和標準.NET 中的單元測試](/dotnet/core/testing/)主題和連結的內容。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

範例應用程式是 Razor 頁面的應用程式，並假設 Razor 頁面的基本知識。 如果不熟悉使用 Razor 頁面，請參閱下列主題：

* [Razor 頁面簡介](xref:razor-pages/index)
* [開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Razor 頁面單元測試](xref:test/razor-pages-tests)

## <a name="introduction-to-integration-tests"></a>整合測試的簡介

整合測試評估應用程式的元件，比在廣泛層級[單元測試](/dotnet/core/testing/)。 單元測試用來測試隔離的軟體元件，例如個別類別方法。 整合測試確認兩個或多個應用程式元件一起運作以產生預期的結果，可能會包含每個完整處理要求所需的元件。

這些較廣泛測試用來測試應用程式的基礎結構與整個架構，通常包括下列元件：

* 資料庫
* 檔案系統
* 網路應用裝置
* 要求-回應管線

單元測試使用傳遞元件，又稱為*fakes*或*模擬物件*，來取代基礎結構的元件。

相較於單元測試，測試整合：

* 使用應用程式在生產環境中使用的實際元件。
* 需要更多的程式碼及資料處理。
* 需要更長時間才能執行。

因此，會限制使用的整合測試，將最重要的基礎結構案例。 如果可以使用單元測試或整合測試，測試行為，請選擇 單元測試。

> [!TIP]
> 不要撰寫每個可能的排列的資料與檔案存取資料庫和檔案系統的整合測試。 不論多少會放在應用程式互動的資料庫和檔案系統時，已取得焦點的讀取、 寫入、 更新和刪除整合測試通常能夠適當測試資料庫和檔案系統元件的集合。 使用單元測試方法的邏輯的常式與這些元件互動的測試。 在單元測試中的基礎結構使用 fakes/模擬更快的測試執行中的結果。

> [!NOTE]
> 在討論中的整合測試，測試的專案經常稱為*待測系統*，或簡稱為 「 SUT"。

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core 整合測試

整合測試中 ASP.NET Core 需要以下各項：

* 測試專案用來包含及執行測試。 測試專案有已測試的 ASP.NET Core 專案，稱為參考*待測系統*(SUT)。 _"SUT 」 在本主題中用於測試應用程式的參考。_
* 測試專案建立 SUT 的測試 web 主機，並處理要求和回應傳給 SUT 使用測試伺服器的用戶端。
* 測試執行器用來執行測試和報告測試結果。

整合測試，請依照下列一連串的事件，其中包含一般*排列*， *Act*，和*Assert*測試步驟：

1. SUT web 主機進行設定。
1. 測試伺服器的用戶端會提交要求給應用程式建立。
1. *排列*測試步驟執行： 測試應用程式會準備要求。
1. *Act*測試步驟執行： 用戶端會提交要求並接收回應。
1. *Assert*測試步驟執行：*實際*回應驗證為*傳遞*或*失敗*根據*預期*回應。
1. 所有測試執行之前，會繼續處理程序。
1. 會報告測試結果。

通常，測試 web 主機已設定以不同的方式比執行測試的應用程式的一般 web 主機。 例如，不同的資料庫或不同的應用程式設定可能會用來測試。

基礎結構元件，例如測試 web 主機和記憶體中的測試伺服器 ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver))、 提供或由[Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)封裝。 使用此封裝來簡化測試建立和執行。

`Microsoft.AspNetCore.Mvc.Testing`封裝處理下列工作：

* 複製相依性的檔案 (*\*.deps*) 從測試專案中的 SUT *bin*資料夾。
* 設定內容的根為 SUT 專案根目錄上，如此靜態檔案和頁面/檢視表位於測試執行時。
* 提供[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類別來簡化啟動載入與 SUT `TestServer`。

[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)文件說明如何設定測試專案和測試執行器，以及如何執行測試和建議的方式來測試名稱和測試類別的詳細指示。

> [!NOTE]
> 建立應用程式的測試專案時，分開單元測試整合測試分成不同的專案。 這有助於確保基礎結構測試元件是的意外地包括在單元測試。 隔離的單位和整合測試也允許控制哪些資料集的測試會執行。

不沒有幾乎任何 Razor 網頁應用程式的測試組態和 MVC 應用程式之間的差異。 唯一的差異是在測試命名方式。 Razor 網頁應用程式中測試的頁面端點通常命名為之後的頁面模型類別 (例如，`IndexPageTests`來測試索引頁的元件整合)。 在 MVC 應用程式中測試通常依控制器類別，而且之後的控制站，其會測試名為 (例如，`HomeControllerTests`測試主控制器元件整合)。

## <a name="test-app-prerequisites"></a>測試應用程式的必要條件

測試專案必須：

* 針對封裝參考[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)。
* 在專案檔中使用 Web SDK (`<Project Sdk="Microsoft.NET.Sdk.Web">`)。

中可以看到這些 prerequesities[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/)。 檢查*tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj*檔案。

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>預設值 WebApplicationFactory 基本測試

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)用來建立[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)整合測試。 `TEntryPoint` 通常是 SUT 的進入點類別`Startup`類別。

測試類別會實作*類別裝置*介面 (`IClassFixture`) 來指示此類別包含測試，並且提供跨不同測試類別中的共用的物件執行個體。

### <a name="basic-test-of-app-endpoints"></a>基本測試應用程式端點

下列測試類別， `BasicTests`，會使用`WebApplicationFactory`啟動 SUT 並提供[HttpClient](/dotnet/api/system.net.http.httpclient)至測試方法， `Get_EndpointsReturnSuccessAndCorrectContentType`。 這個方法會檢查是否成功的回應狀態碼 （200-299 範圍中的狀態碼） 和`Content-Type`標頭是`text/html; charset=utf-8`數個應用程式頁面。

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)建立的執行個體`HttpClient`，會自動遵循重新導向，以及處理 cookie。

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>測試的安全端點

中的另一個測試`BasicTests`類別會檢查安全端點重新導向未驗證的使用者，應用程式的登入頁面。

在 SUT`/SecurePage`頁面使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)慣例，以套用[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)至頁面。 如需詳細資訊，請參閱[Razor 頁面授權慣例](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

在`Get_SecurePageRequiresAnAuthenticatedUser`測試， [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)設為禁止重新導向設定[AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)至`false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

不允許用戶端，請遵循重新導向，您可以進行下列檢查：

* SUT 所傳回的狀態碼可以檢查針對預期[HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode)結果，不最終狀態的程式碼之後重新導向至登入頁面，方式是[HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* `Location`回應標頭中的標頭值會檢查以確認它開頭`http://localhost/Identity/Account/Login`，不是最後登入頁面的回應，其中`Location`標頭不會出現。

如需有關`WebApplicationFactoryClientOptions`，請參閱[用戶端選項](#client-options)> 一節。

## <a name="customize-webapplicationfactory"></a>自訂 WebApplicationFactory

Web 主機組態可以建立獨立的測試類別繼承自`WebApplicationFactory`建立一或多個自訂處理站：

1. 繼承自`WebApplicationFactory`並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許的服務集合設定[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   資料庫在植入[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)由執行`InitializeDbForTests`方法。 中所述的方法是[整合測試的範例： 測試應用程式的組織](#test-app-organization)> 一節。

2. 使用自訂`CustomWebApplicationFactory`測試類別中。 下列範例會使用中的處理站`IndexPageTests`類別：

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   範例應用程式的用戶端設定以防止`HttpClient`從下列重新導向。 中所述[測試安全端點](#test-a-secure-endpoint) 區段中，這可讓測試以檢查應用程式的第一個回應的結果。 第一個回應是在許多測試，以重新導向`Location`標頭。

3. 一般測試會使用`HttpClient`和 helper 方法來處理要求和回應：

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

任何 SUT 的 POST 要求都必須滿足的應用程式會自動成為 antiforgery 核取[資料保護 antiforgery 系統](xref:security/data-protection/introduction)。 若要排列測試的 POST 要求，必須測試應用程式：

1. 提出要求的頁面。
1. 剖析 antiforgery cookie 和來自回應的要求驗證語彙基元。
1. 在位置進行 antiforgery cookie 和要求驗證的 POST 要求語彙基元。

`SendAsync` Helper 擴充方法 (*Helpers/HttpClientExtensions.cs*) 和`GetDocumentAsync`helper 方法 (*Helpers/HtmlHelpers.cs*) 中[的範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/)使用[AngleSharp](https://anglesharp.github.io/)剖析器來處理 antiforgery 核取，使用下列方法：

* `GetDocumentAsync` &ndash; 接收[HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage)並傳回`IHtmlDocument`。 `GetDocumentAsync` 使用準備處理站*虛擬回應*根據原始`HttpResponseMessage`。 如需詳細資訊，請參閱[AngleSharp 文件](https://github.com/AngleSharp/AngleSharp#documentation)。
* `SendAsync` 擴充方法`HttpClient`撰寫[HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)呼叫[SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)提交要求給 SUT。 多載`SendAsync`接受 HTML 表單 (`IHtmlFormElement`) 和下列：
  - 送出表單的按鈕 (`IHtmlElement`)
  - 表單值集合 (`IEnumerable<KeyValuePair<string, string>>`)
  - 送出按鈕 (`IHtmlElement`) 和表單值 (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/)第三方剖析用於示範用途，本主題中的範例應用程式的程式庫。 AngleSharp 不支援或所需的整合測試的 ASP.NET Core 應用程式。 其他剖析器可以使用，例如[Html 靈活度組件 (HAP)](http://html-agility-pack.net/)。 另一個方法是撰寫程式碼來直接處理 antiforgery 系統的要求驗證語彙基元和 antiforgery cookie。

## <a name="customize-the-client-with-withwebhostbuilder"></a>自訂 WithWebHostBuilder 與用戶端

在測試方法中，需要額外的設定時[WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)新建`WebApplicationFactory`與[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ，進一步自訂組態。

`Post_DeleteMessageHandler_ReturnsRedirectToRoot`測試方法的[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)示範如何使用`WithWebHostBuilder`。 此測試會在資料庫中執行記錄刪除，透過觸發 SUT 中的送出表單。

因為另一個測試中`IndexPageTests`類別執行的作業，刪除所有資料庫中記錄和可能執行之前`Post_DeleteMessageHandler_ReturnsRedirectToRoot`方法時，資料庫在這個測試方法，以確保有一筆記錄的刪除 SUT 植入。 選取`deleteBtn1`按鈕`messages`SUT 的要求中模擬 SUT 中的表單時：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>用戶端選項

下表顯示預設[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)可用時建立`HttpClient`執行個體。

| 選項 | 描述 | 預設 |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | 取得或設定是否`HttpClient`執行個體應該自動追蹤重新導向回應。 | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | 取得或設定基底地址`HttpClient`執行個體。 | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | 取得或設定是否`HttpClient`執行個體應該處理 cookie。 | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | 取得或設定最大重新導向回應數目`HttpClient`應遵循的執行個體。 | 7 |

建立`WebApplicationFactoryClientOptions`類別，並將它傳遞給[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)方法 （預設值以在程式碼範例所示）：

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>如何測試基礎結構會推斷的應用程式內容的根路徑

`WebApplicationFactory`建構函式會藉由搜尋推斷的應用程式內容的根路徑[WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)包含具有索引鍵等於整合測試的組件上`TEntryPoint`的組件`System.Reflection.Assembly.FullName`. 萬一找不到具有正確的索引鍵的屬性，`WebApplicationFactory`會回復為搜尋的方案檔 (*\*.sln*)，並將附加`TEntryPoint`的方案目錄的組件名稱。 應用程式根目錄 （內容的根路徑） 用來探索檢視和內容檔案。

在大部分情況下，它就不需要明確地將應用程式內容的根目錄中，設定如下的搜尋邏輯通常在執行階段尋找正確內容的根。 在找到內容的根目錄不是特殊案例中使用內建的搜尋演算法，內容可以在明確或使用自訂邏輯，指定根應用程式。 若要設定應用程式內容的根目錄中的情況下，呼叫`UseSolutionRelativeContentRoot`擴充方法，從[Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost)封裝。 提供方案的相對路徑和選用的方案檔案名稱或 glob 模式 (預設 = `*.sln`)。

呼叫[UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot)擴充方法使用*一個*的方法包括：

* 設定與測試類別時`WebApplicationFactory`，提供的自訂組態[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):

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

* 當利用自訂設定的測試類別時`WebApplicationFactory`，繼承自`WebApplicationFactory`並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

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

陰影複製會導致在不同的輸出資料夾的資料夾中執行測試。 測試中才能正常運作，陰影複製必須停用。 [範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)使用 xUnit，並停用所包括的 xUnit 陰影複製*xunit.runner.json*具有正確的組態設定檔。 如需詳細資訊，請參閱[使用 JSON 設定 xUnit.net](https://xunit.github.io/docs/configuring-with-json.html)。

新增*xunit.runner.json*具有下列內容在測試專案的根檔案：

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>整合測試範例

[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)是兩個應用程式所組成：

| 應用程式 | 專案資料夾 | 描述 |
| --- | -------------- | ----------- |
| 訊息應用程式 (SUT) | *src/RazorPagesProject* | 允許使用者加入、 刪除其中一個，刪除所有項目，以及分析訊息。 |
| 測試應用程式 | *tests/RazorPagesProject.Tests* | 用來整合測試 SUT。 |

可以執行測試，例如使用內建測試功能的 IDE， [Visual Studio](https://www.visualstudio.com/vs/)。 如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列中，執行下列命令，在命令提示字元中*tests/RazorPagesProject.Tests*資料夾：

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>訊息應用程式 (SUT) 組織

SUT 是 Razor 頁面訊息系統具有下列特性：

* 索引頁的應用程式 (*Pages/Index.cshtml*和*Pages/Index.cshtml.cs*) 提供 UI 和頁面來控制的新增、 刪除和訊息 （每個訊息的平均字） 的分析模型方法.
* 所描述的訊息`Message`類別 (*Data/Message.cs*) 使用兩個屬性： `Id` （金鑰） 和`Text`（訊息）。 `Text`屬性所需，且限制為 200 個字元。
* 訊息會儲存使用[Entity Framework 的記憶體中資料庫](/ef/core/providers/in-memory/)&#8224;。
* 應用程式在其資料庫內容類別，包含資料存取層 (DAL) `AppDbContext` (*Data/AppDbContext.cs*)。
* 如果資料庫是空的應用程式啟動時，訊息存放區會使用三個訊息中初始化。
* 應用程式包含`/SecurePage`，只能存取已驗證使用者。

&#8224;EF 主題[測試與 InMemory](/ef/core/miscellaneous/testing/in-memory)，說明如何使用記憶體中資料庫 MSTest 的測試。 本主題使用[xUnit](https://xunit.github.io/)測試架構。 測試概念與測試實作跨不同測試架構很類似的但不是完全相同。

雖然此應用程式不會使用[儲存機制模式](http://martinfowler.com/eaaCatalog/repository.html)並不是有效的範例[工作單位 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)，Razor 頁面支援這些模式開發。 如需詳細資訊，請參閱[設計基礎結構的持續性層級](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)， [ASP.NET MVC 應用程式中實作的儲存機制和工作單元模式](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)，和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（此範例會實作儲存機制模式）。

### <a name="test-app-organization"></a>測試應用程式的組織

測試應用程式是主控台應用程式內*tests/RazorPagesProject.Tests*資料夾。

| 測試應用程式的資料夾 | 描述 |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs*包含路由、 存取安全網頁由未經驗證的使用者，並取得 GitHub 使用者設定檔和檢查設定檔的使用者登入的測試方法。 |
| *IntegrationTests* | *IndexPageTests.cs*包含使用自訂的索引頁的整合測試`WebApplicationFactory`類別。 |
| *Helper/公用程式* | <ul><li>*Utilities.cs*包含`InitializeDbForTests`方法用來植入的測試資料的資料庫。</li><li>*HtmlHelpers.cs*提供方法來傳回 AngleSharp`IHtmlDocument`以供測試方法。</li><li>*HttpClientExtensions.cs*提供的多載`SendAsync`提交要求給 SUT。</li></ul> |

測試架構是[xUnit](https://xunit.github.io/)。 搜尋整合測試都會使用[Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost)，其中包括[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。 因為[Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)封裝用來設定測試主應用程式和測試伺服器`TestHost`和`TestServer`套件不需要直接將封裝中測試應用程式的專案檔的參考或開發人員在測試應用程式的組態。

**植入資料庫中的測試**

整合測試通常需要在測試執行前的資料庫中的小型資料集。 比方說，刪除測試會呼叫為資料庫記錄的刪除，所以資料庫必須至少一個記錄刪除要求，才能成功。

範例應用程式植入的資料庫中的三個訊息*Utilities.cs*它們執行時，可以使用測試：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>其他資源

* [單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Razor 頁面單元測試](xref:test/razor-pages-tests)
* [中介軟體](xref:fundamentals/middleware/index)
* [測試控制器](xref:mvc/controllers/testing)
