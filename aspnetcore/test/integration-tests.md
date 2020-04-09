---
title: ASP.NET核心中的整合測試
author: rick-anderson
description: 了解整合測試如何確保應用程式的元件在基礎結構層級 (包括資料庫、檔案系統和網路) 正確運作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/06/2019
uid: test/integration-tests
ms.openlocfilehash: 414e47b9c5a1c843755bd79d313f503b554945db
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664693"
---
# <a name="integration-tests-in-aspnet-core"></a>ASP.NET核心中的整合測試

由[哈威爾·卡爾瓦羅·納爾遜](https://github.com/javiercn)、[史蒂夫·史密斯](https://ardalis.com/)和[喬斯·范德蒂爾](https://jvandertil.nl)

::: moniker range=">= aspnetcore-3.0"

集成測試可確保應用元件在包含應用支援基礎結構(如資料庫、檔案系統和網路)的級別上正常運行。 ASP.NET Core 支援使用具有測試 Web 主機和記憶體中測試伺服器的單元測試框架進行集成測試。

本主題假定對單元測試有基本的瞭解。 如果不熟悉測試概念,請參閱[.NET 核心和 .NET 標準主題中的單元測試](/dotnet/core/testing/)及其鏈接內容。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)([如何下載](xref:index#how-to-download-a-sample))

示例應用是 Razor 頁面應用,並假定對 Razor 頁面有基本的瞭解。 如果不熟悉 Razor 頁面,請參閱以下主題:

* [Razor 頁面簡介](xref:razor-pages/index)
* [開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Razor 頁面單元測試](xref:test/razor-pages-tests)

> [!NOTE]
> 對於測試SpA,我們建議一個工具,如[硒](https://www.seleniumhq.org/),它可以自動化瀏覽器。

## <a name="introduction-to-integration-tests"></a>整合測試簡介

集成測試比[單元測試](/dotnet/core/testing/)在更廣泛的層面上評估應用的元件。 單元測試用於測試隔離的軟體元件,如單個類方法。 集成測試確認兩個或多個應用元件協同工作以產生預期結果,可能包括完全處理請求所需的每個元件。

這些更廣泛的測試用於測試應用程式的基礎結構和整個框架,通常包括以下元件:

* 資料庫
* 檔案系統
* 網路設備
* 請求-回應導管

單元測試使用製造元件(稱為*假*體或*類比物件*)代替基礎結構元件。

與單元測試相比,集成測試:

* 使用應用在生產中使用的實際元件。
* 需要更多的代碼和數據處理。
* 需要更長的時間才能運行。

因此,將集成測試的使用限制在最重要的基礎結構方案中。 如果可以使用單元測試或集成測試測試行為,請選擇單元測試。

> [!TIP]
> 不要為資料庫和檔案系統中數據和文件存取的每一種可能排列編寫集成測試。 無論應用中有多少位置與資料庫和檔案系統交互,集中讀取、寫入、更新和刪除集成測試集通常能夠充分測試資料庫和檔案系統元件。 使用單元測試對與這些元件交互的方法邏輯進行例行測試。 在單元測試中,使用基礎結構假/類比會導致更快的測試執行。

> [!NOTE]
> 在討論集成測試時,測試專案通常稱為 *「正在測試的系統*」,簡稱為「SUT」。。
>
> *本主題中使用"SUT"來參考經過測試的 ASP.NET酷睿應用。*

## <a name="aspnet-core-integration-tests"></a>ASP.NET核心整合測試

ASP.NET核心中的整合測試需要以下要求:

* 測試專案用於包含和執行測試。 測試專案具有對 SUT 的引用。
* 測試專案為 SUT 創建測試 Web 主機,並使用測試伺服器用戶端使用 SUT 處理請求和回應。
* 測試運行程式用於執行測試並報告測試結果。

整合測試遵循一系列事件,其中包括通常的*排列*、*操作*與*斷言*測試步驟:

1. SUT 的 Web 主機已配置。
1. 將創建測試伺服器用戶端以向應用提交請求。
1. 執行 *「排列*」測試步驟:測試應用準備請求。
1. *執行 Act*測試步驟:用戶端提交請求並接收回應。
1. *執行 Assert*測試步驟:*實際*回應根據*預期*回應驗證為*透過*或*失敗*。
1. 該過程將繼續,直到執行所有測試。
1. 報告測試結果。

通常,測試 Web 主機的配置方式與測試運行應用的正常 Web 主機不同。 例如,測試可能使用不同的資料庫或不同的應用設置。

基礎結構元件(如測試 Web 主機和記憶體中測試[伺服器 (TestServer)](/dotnet/api/microsoft.aspnetcore.testhost.testserver)由[Microsoft 提供](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)或管理。 使用此包可以簡化測試創建和執行。

該`Microsoft.AspNetCore.Mvc.Testing`套件處理以下工作:

* 將依賴項檔 *(.deps*) 從 SUT 複製到測試專案的*bin*目錄中。
* 將[內容根目錄](xref:fundamentals/index#content-root)設置到 SUT 的專案根目錄,以便在執行測試時找到靜態檔和頁面/檢視。
* 提供[Web 應用程式工廠](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類以`TestServer`簡化使用的 SUT 引導。

[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)文件介紹如何設置測試專案和測試運行程式,以及有關如何運行測試的詳細說明以及如何命名測試和測試類的建議。

> [!NOTE]
> 為應用創建測試專案時,將單元測試與集成測試分離到不同的專案中。 這有助於確保基礎結構測試元件不會意外包含在單元測試中。 單元和集成測試的分離還允許控制運行哪組測試。

Razor Pages 應用和 MVC 應用測試的配置之間幾乎沒有區別。 唯一的區別是如何命名測試。 在 Razor Pages 應用中,頁面終結點的測試通常以頁面模型類命名`IndexPageTests`(例如, 測試索引頁的元件整合)。 在 MVC 應用中,測試通常由控制器類組織,並基於它們測試的控制器命名(`HomeControllerTests`例如, 測試主控制器的元件整合)。

## <a name="test-app-prerequisites"></a>測試應用先決條件

測試項目必須:

* 參考[微軟.AspNetCore.Mvc.測試](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)包。
* 在專案檔中指定 Web SDK`<Project Sdk="Microsoft.NET.Sdk.Web">`( 。

這些先決條件可以在[示例應用中](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)看到。 檢查*測試/RazorPages專案.測試/RazorPages專案.測試.csproj*檔。 範例應用程式使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)解析器庫,因此範例應用還引用:

* [森特](https://www.nuget.org/packages/xunit)
* [xunit.runner.Visualstudio](https://www.nuget.org/packages/xunit.runner.visualstudio)
* [角度夏普](https://www.nuget.org/packages/AngleSharp)

實體框架核心也用於測試。 套用:

* [微軟.AspNetCore.診斷.實體框架核心](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore)
* [微軟.AspNetCore.身份.實體框架核心](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity.EntityFrameworkCore)
* [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore)
* [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)
* [微軟.實體框架核心.工具](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)

## <a name="sut-environment"></a>SUT 環境

如果未設置 SUT[的環境](xref:fundamentals/environments),則環境預設為"開發」。。

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>使用預設 Web 應用程式工廠的基本測試

[Web\<應用程式 工廠 TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)用於為整合測試建立[測試伺服器](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。 `TEntryPoint`是 SUT 的入口點類別,通常`Startup`是類 。

測試類實現*類韌體*介面[(IClassFixture)](https://xunit.github.io/docs/shared-context#class-fixture)以指示類包含測試並在類中的測試中提供共用物件實例。

以下`BasicTests`測試類`WebApplicationFactory`使用引導 SUT 並提供[HttpClient](/dotnet/api/system.net.http.httpclient)`Get_EndpointsReturnSuccessAndCorrectContentType`到測試方法 的 HTTPClient。 該方法檢查回應狀態代碼是否成功(範圍為 200-299)的狀態代碼,`Content-Type`並且標頭`text/html; charset=utf-8`是 多個應用頁。

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)創建一`HttpClient`個 實例,該實例會自動遵循重定向並處理 Cookie。

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

默認情況下,啟用[GDPR同意策略](xref:security/gdpr)時,不會跨請求保留非必需Cookie。 要保留非必需的 Cookie(如 TempData 提供者使用的 Cookie),請將其標記為測試中必不可少的 Cookie。 有關將 Cookie 標記為基本 Cookie 的說明,請參閱[基本 Cookie](xref:security/gdpr#essential-cookies)。

## <a name="customize-webapplicationfactory"></a>自訂 Web 應用程式工廠

可以通過繼承`WebApplicationFactory`來 創建一個或多個自定義工廠,從而獨立於測試類創建 Web 主機配置:

1. 繼承`WebApplicationFactory`並覆蓋[設定 WebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[設定服務](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)設定服務集合 :

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   [範例應用中的](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)資料庫種子設定由`InitializeDbForTests`方法執行。 該方法在[集成測試示例:測試應用組織](#test-app-organization)部分中進行了描述。

   SUT 的資料庫上下文`Startup.ConfigureServices`在其 方法中註冊。 測試應用的`builder.ConfigureServices`回調在執行應用`Startup.ConfigureServices`代碼*後*執行。 執行順序是[通用主機](xref:fundamentals/host/generic-host)的一個重大更改,ASP.NET核心 3.0 的發佈。 要對測試使用不同的資料庫,必須在中`builder.ConfigureServices`替換應用的資料庫上下文。

   範例應用查找資料庫上下文的服務描述符,並使用描述符刪除服務註冊。 接下來,工廠將添加新的`ApplicationDbContext`測試使用記憶體中資料庫。

   要連接到與記憶體中資料庫不同的資料庫,請更改`UseInMemoryDatabase`調用以將上下文連接到其他資料庫。 要使用 SQL Server 測試資料庫,請使用:

   * 在專案檔中引用[Microsoft.EntityFramework Core.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/) NuGet 包。
   * 使用`UseSqlServer`與資料庫的連接字串調用。

   ```csharp
   services.AddDbContext<ApplicationDbContext>((options, context) => 
   {
       context.UseSqlServer(
           Configuration.GetConnectionString("TestingDbConnectionString"));
   });
   ```

2. 在測試類`CustomWebApplicationFactory`中使用自定義。 下面的範例在`IndexPageTests`類別使用工廠:

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   示例應用的用戶端配置為防止以下`HttpClient`重定向。 如稍後在[Mock 身份驗證](#mock-authentication)部分中所述,這允許測試以檢查應用的第一個回應的結果。 第一個回應是許多具有`Location`標頭的測試中的重定向。

3. 典型的測試使用`HttpClient`與 說明器方法來處理請求和回應:

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

任何POST請求SUT必須滿足反偽造檢查,這是自動由應用程式的[資料保護反偽造系統](xref:security/data-protection/introduction)。 為了安排測試的開 POST 請求,測試應用必須:

1. 對頁面發出請求。
1. 分析反偽造 Cookie 並從回應請求驗證權杖。
1. 使用防偽造 Cookie 進行 POST 請求,並請求驗證權杖到位。

`SendAsync`[範例應用程式中](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)的協助器擴充方法 (*協助器/HTTPClient 擴充.cs*) 與`GetDocumentAsync`說明器方法 (*協助器/HtmlHelpers.cs*) 使用[AngleSharp](https://anglesharp.github.io/)解析器處理反偽造檢查的方法:

* `GetDocumentAsync`&ndash;接收[HTTPResponse](/dotnet/api/system.net.http.httpresponsemessage)訊息`IHtmlDocument`並傳回 。 `GetDocumentAsync`使用基於原始`HttpResponseMessage`的準備*虛擬回應*的工廠。 有關詳細資訊,請參閱[AngleSharp 文件](https://github.com/AngleSharp/AngleSharp#documentation)。
* `SendAsync``HttpClient`撰寫[HTTPRequestMessage](/dotnet/api/system.net.http.httprequestmessage)並呼叫[SendAsync(HTTPRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)的擴充方法向 SUT 提交請求。 `SendAsync`接受 HTML 窗型`IHtmlFormElement`( ) 的重載與以下內容:
  * 提交表格按鈕`IHtmlElement`( )
  * 表單值集合`IEnumerable<KeyValuePair<string, string>>`( )
  * 提交按鈕`IHtmlElement`( )`IEnumerable<KeyValuePair<string, string>>`與表單 ( )

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/)是一個第三方解析庫,用於本主題和示例應用中的演示目的。 ASP.NET核心應用的整合測試不支援或不需要 AngleSharp。 可以使用其他解析器,例如[Html 敏捷包 (HAP)。](https://html-agility-pack.net/) 另一種方法是編寫代碼,直接處理反偽造系統的請求驗證令牌和反偽造 Cookie。

## <a name="customize-the-client-with-withwebhostbuilder"></a>使用 WebHostBuilder 自訂客戶端

當測試方法中需要其他配置時,[與 WebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)一起`WebApplicationFactory`創建一個新的[IWebHostBuilder,該構建器](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)透過配置進一步自定義。

`Post_DeleteMessageHandler_ReturnsRedirectToRoot`[範例應用程式的](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)測試方法演示了`WithWebHostBuilder`的用法。 此測試通過在 SUT 中觸發表單提交來在資料庫中執行記錄刪除。

由於`IndexPageTests`類中的另一個測試執行一個操作,該操作將刪除資料庫中的所有記錄,並且可能`Post_DeleteMessageHandler_ReturnsRedirectToRoot`在方法之前運行,因此將在此測試方法中重新設定資料庫的種子,以確保存在要刪除的 SUT 的記錄。 在 SUT`messages`中選擇 表單的第一個刪除按鈕在 SUT 請求中模擬:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>用戶端選項

下表顯示了建立`HttpClient`實體時可用的預設[Web 應用程式工廠客戶端選項](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)。

| 選項 | 描述 | 預設 |
| ------ | ----------- | ------- |
| [允許自動重定向](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | 獲取或設置`HttpClient`實例是否應自動遵循重定向回應。 | `true` |
| [基本位址](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | 獲取或設置實例的基本`HttpClient`位址。 | `http://localhost` |
| [手柄餅乾](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | 獲取或設置實例`HttpClient`是否應處理 Cookie。 | `true` |
| [最大自動重定向](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | 獲取或設置`HttpClient`實例應遵循的最大重定向回應數。 | 7 |

建立`WebApplicationFactoryClientOptions`類別並將其傳遞給[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)方法(預設值顯示在代碼範例中):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>注入模擬服務

可以通過調用主機生成器上的[配置測試服務](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)在測試中覆蓋服務。 **要注入類比服務,SUT 必須`Startup`具有具有`Startup.ConfigureServices`方法的 類。**

示例 SUT 包括返回報價表的作用域服務。 當請求索引頁時,報價嵌入到索引頁上的隱藏欄位中。

*服務/IQuote服務.cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*服務/報價服務.cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index.cshtml.cs*：

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*頁面/索引.cs*:

[!code-cshtml[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

執行 SUT 應用程式時,將產生以下標記:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

為了在整合測試中測試服務和報價注入,測試將類比服務注入 SUT。 模擬服務`QuoteService`使用測試應用提供的服務取代應用,該服務`TestQuoteService`稱為 :

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices`呼叫,並註冊作用域服務:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

在測試執行期間生成的標記反映了`TestQuoteService`提供的報價文本,因此斷言通過:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="mock-authentication"></a>類比認證

類別測試`AuthTests`檢查安全終結點:

* 將未經身份驗證的用戶重定向到應用的登錄頁面。
* 返回經過身份驗證的用戶的內容。

在 SUT`/SecurePage`中 ,該頁使用[授權頁](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)約定對頁面應用[授權篩選器](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)。 有關詳細資訊,請參閱[Razor 頁面授權約定](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

測試`Get_SecurePageRedirectsAnUnauthenticatedUser`中[,Web 應用程式工廠客戶端選項](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)設定為設定[「允許自動重定向」](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)來`false`關閉:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet2)]

通過不允許用戶端遵循重定向,可以進行以下檢查:

* SUT 傳回的狀態代碼可以根據預期的[HTTPStatusCode.重定向](/dotnet/api/system.net.httpstatuscode)結果進行檢查,而不是重定向到登入頁面後的最終狀態代碼,該頁面將是[HTTPStatusCode.OK](/dotnet/api/system.net.httpstatuscode)。
* 檢查`Location`回應標頭中的標頭值以確認它`http://localhost/Identity/Account/Login`以 開頭,而不是最終登錄頁回應`Location`,其中標頭將不存在。

測試應用可以模擬<xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>[配置測試服務](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)中的 ,以便測試身份驗證和授權的方面。 最小專案傳回[身份認證結果.成功](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*):

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet4&highlight=11-18)]

當身份驗證方案設置為`Test`註冊`AddAuthentication`的位置 時,調用 的調用 以對用戶`ConfigureTestServices``TestAuthHandler`進行身份驗證。

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet3&highlight=7-12)]

有關的詳細資訊`WebApplicationFactoryClientOptions`,請參閱[用戶端選項](#client-options)部分。

## <a name="set-the-environment"></a>設定環境

默認情況下,SUT 的主機和應用環境配置為使用開發環境。 要覆蓋 SUT 的環境,

* 設定`ASPNETCORE_ENVIRONMENT`環境變數(例如`Staging`,`Production`, 或其他自訂`Testing`值, 如 .
* 在`CreateHostBuilder`測試應用中重寫以讀取預綴的環境`ASPNETCORE`變數。

```csharp
protected override IHostBuilder CreateHostBuilder() => 
    base.CreateHostBuilder()
        .ConfigureHostConfiguration(
            config => config.AddEnvironmentVariables("ASPNETCORE"));
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>測試基礎結構如何推斷應用內容根路徑

建構`WebApplicationFactory`函數透過在包含`TEntryPoint`與程式`System.Reflection.Assembly.FullName`集的整合測試的程式集上搜尋[WebAppFactoryContentRootattribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)來推斷應用[內容根](xref:fundamentals/index#content-root)路徑。 如果找不到具有正確鍵的屬性,`WebApplicationFactory`請回退到搜尋解決方案檔 *(.sln*) 並將`TEntryPoint`程式集名稱追加到解決方案目錄。 應用根目錄(內容根路徑)用於發現檢視和內容檔。

## <a name="disable-shadow-copying"></a>關閉陰影複製

捲影複製會導致測試在不同於輸出目錄的目錄中執行。 要使測試正常工作,必須禁用捲影複製。 [範例應用](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)使用 xUnit,並透過將*xunit.runner.json*檔與正確的設定設定包括,來禁用 xUnit 的捲影複製。 有關詳細資訊,請參閱使用[JSON 配置 xUnit。](https://xunit.github.io/docs/configuring-with-json.html)

將*xunit.runner.json*檔新增到測試項目的根目錄,內容如下:

```json
{
  "shadowCopy": false
}
```

## <a name="disposal-of-objects"></a>物品處理

執行`IClassFixture`實現測試後,當 xUnit 處置[Web 應用程式工廠](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)時,將釋放[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)和[HTTPClient。](/dotnet/api/system.net.http.httpclient) 如果開發人員實例化的物件需要處理,請在實施中`IClassFixture`釋放它們。 有關詳細資訊,請參閱實現[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。

## <a name="integration-tests-sample"></a>整合測試範例

[範例應用](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)由兩個應用組成:

| App | 專案目錄 | 描述 |
| --- | ----------------- | ----------- |
| 訊息應用(SUT) | *src/剃鬚刀專案* | 允許使用者添加、刪除一條、刪除所有郵件和分析郵件。 |
| 測試應用程式 | *測試/剃鬚刀項目測試* | 用於集成測試 SUT。 |

測試可以使用IDE的內置測試功能(如[Visual Studio)](https://visualstudio.microsoft.com)運行。 如果使用[Visual Studio 代碼](https://code.visualstudio.com/)或命令列,請在*測試/RazorPagesProject.測試*目錄中的指令提示符執行以下指令:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>訊息應用 (SUT) 組織

SUT 是一個剃刀頁消息系統,具有以下特徵:

* 應用程式的索引頁 (*頁面 / Index.cshtml*和*頁面 / Index.cshtml.cs*) 提供了一個 UI 和頁面模型方法來控制消息的添加、刪除和分析(每條消息的平均字數)。
* 消息由具有兩個屬性`Message``Id``Text`的類 (*Data/Message.cs*) 描述。 該`Text`屬性是必需的,限制為 200 個字元。
* 消息使用[實體框架的記憶體中資料庫](/ef/core/providers/in-memory/)&#8224;存储。
* 該應用程式在其資料庫上下文類中包含數據訪問層 (DAL) `AppDbContext` (*資料/AppDbContext.cs*)。
* 如果資料庫在應用啟動時為空,則消息存儲將用三條消息進行初始化。
* 該應用程式包括`/SecurePage`只能由經過身份驗證的使用者訪問的應用。

&#8224;EF 主題[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)「解釋了如何使用記憶體中資料庫進行 MSTest 測試。 本主題使用[xUnit](https://xunit.github.io/)測試框架。 不同測試框架的測試概念和測試實現相似但不相同。

雖然應用程式不使用儲存庫模式,並且不是[工作單位 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)的有效示例,Razor Pages 支援這些開發模式。 有關詳細資訊,請參閱[設計基礎結構持久性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)(示例實現存儲庫模式)。

### <a name="test-app-organization"></a>測試應用組織

測試應用是*測試/RazorPages Project.Test*目錄中的控制台應用。

| 測試應用程式目錄 | 描述 |
| ------------------ | ----------- |
| *奧斯特測試* | 包含用於:<ul><li>由未經身份驗證的用戶訪問安全頁面。</li><li>通過模擬<xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>訪問經過身份驗證的使用者的安全頁面。</li><li>取得 GitHub 使用者設定檔並檢查設定檔的使用者登錄名。</li></ul> |
| *基本測試* | 包含路由和內容類型的測試方法。 |
| *IntegrationTests* | 包含使用自定義`WebApplicationFactory`類的 Index 頁的集成測試。 |
| *說明者/公用程式* | <ul><li>*Utilities.cs*包含用於`InitializeDbForTests`使用 測試數據為資料庫設定種子的方法。</li><li>*HtmlHelpers.cs*提供了返回 AngleSharp`IHtmlDocument`的方法,供測試方法使用。</li><li>*HttpClientExtensions.cs*提供`SendAsync`向 SUT 提交請求的重載。</li></ul> |

測試框架是[xUnit](https://xunit.github.io/)。 整合測試使用[Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost)進行,其中包括[測試伺服器](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。 由於[Microsoft.AspNetCore.Mvc.測試](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)套件用於配置測試主機和測試伺服器,`TestHost``TestServer`因此和 包不需要在測試應用的專案檔或測試應用中的開發人員配置中直接引用包引用。

**設定資料庫進行測試的 torrent**

集成測試通常需要在測試執行之前在資料庫中建立小型數據集。 例如,刪除測試調用資料庫記錄刪除,因此資料庫必須至少有一個記錄才能成功刪除請求。

範例應用程式在*資料庫*種子Utilities.cs中包含三條訊息,測試在執行時可以使用這些消息:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

SUT 的資料庫上下文`Startup.ConfigureServices`在其 方法中註冊。 測試應用的`builder.ConfigureServices`回調在執行應用`Startup.ConfigureServices`代碼*後*執行。 要對測試使用不同的資料庫,必須在中`builder.ConfigureServices`替換應用程式的資料庫上下文。 有關詳細資訊,請參閱[自訂 Web 應用程式工廠](#customize-webapplicationfactory)部分。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

集成測試可確保應用元件在包含應用支援基礎結構(如資料庫、檔案系統和網路)的級別上正常運行。 ASP.NET Core 支援使用具有測試 Web 主機和記憶體中測試伺服器的單元測試框架進行集成測試。

本主題假定對單元測試有基本的瞭解。 如果不熟悉測試概念,請參閱[.NET 核心和 .NET 標準主題中的單元測試](/dotnet/core/testing/)及其鏈接內容。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)([如何下載](xref:index#how-to-download-a-sample))

示例應用是 Razor 頁面應用,並假定對 Razor 頁面有基本的瞭解。 如果不熟悉 Razor 頁面,請參閱以下主題:

* [Razor 頁面簡介](xref:razor-pages/index)
* [開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Razor 頁面單元測試](xref:test/razor-pages-tests)

> [!NOTE]
> 對於測試SpA,我們建議一個工具,如[硒](https://www.seleniumhq.org/),它可以自動化瀏覽器。

## <a name="introduction-to-integration-tests"></a>整合測試簡介

集成測試比[單元測試](/dotnet/core/testing/)在更廣泛的層面上評估應用的元件。 單元測試用於測試隔離的軟體元件,如單個類方法。 集成測試確認兩個或多個應用元件協同工作以產生預期結果,可能包括完全處理請求所需的每個元件。

這些更廣泛的測試用於測試應用程式的基礎結構和整個框架,通常包括以下元件:

* 資料庫
* 檔案系統
* 網路設備
* 請求-回應導管

單元測試使用製造元件(稱為*假*體或*類比物件*)代替基礎結構元件。

與單元測試相比,集成測試:

* 使用應用在生產中使用的實際元件。
* 需要更多的代碼和數據處理。
* 需要更長的時間才能運行。

因此,將集成測試的使用限制在最重要的基礎結構方案中。 如果可以使用單元測試或集成測試測試行為,請選擇單元測試。

> [!TIP]
> 不要為資料庫和檔案系統中數據和文件存取的每一種可能排列編寫集成測試。 無論應用中有多少位置與資料庫和檔案系統交互,集中讀取、寫入、更新和刪除集成測試集通常能夠充分測試資料庫和檔案系統元件。 使用單元測試對與這些元件交互的方法邏輯進行例行測試。 在單元測試中,使用基礎結構假/類比會導致更快的測試執行。

> [!NOTE]
> 在討論集成測試時,測試專案通常稱為 *「正在測試的系統*」,簡稱為「SUT」。。
>
> *本主題中使用"SUT"來參考經過測試的 ASP.NET酷睿應用。*

## <a name="aspnet-core-integration-tests"></a>ASP.NET核心整合測試

ASP.NET核心中的整合測試需要以下要求:

* 測試專案用於包含和執行測試。 測試專案具有對 SUT 的引用。
* 測試專案為 SUT 創建測試 Web 主機,並使用測試伺服器用戶端使用 SUT 處理請求和回應。
* 測試運行程式用於執行測試並報告測試結果。

整合測試遵循一系列事件,其中包括通常的*排列*、*操作*與*斷言*測試步驟:

1. SUT 的 Web 主機已配置。
1. 將創建測試伺服器用戶端以向應用提交請求。
1. 執行 *「排列*」測試步驟:測試應用準備請求。
1. *執行 Act*測試步驟:用戶端提交請求並接收回應。
1. *執行 Assert*測試步驟:*實際*回應根據*預期*回應驗證為*透過*或*失敗*。
1. 該過程將繼續,直到執行所有測試。
1. 報告測試結果。

通常,測試 Web 主機的配置方式與測試運行應用的正常 Web 主機不同。 例如,測試可能使用不同的資料庫或不同的應用設置。

基礎結構元件(如測試 Web 主機和記憶體中測試[伺服器 (TestServer)](/dotnet/api/microsoft.aspnetcore.testhost.testserver)由[Microsoft 提供](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)或管理。 使用此包可以簡化測試創建和執行。

該`Microsoft.AspNetCore.Mvc.Testing`套件處理以下工作:

* 將依賴項檔 *(.deps*) 從 SUT 複製到測試專案的*bin*目錄中。
* 將[內容根目錄](xref:fundamentals/index#content-root)設置到 SUT 的專案根目錄,以便在執行測試時找到靜態檔和頁面/檢視。
* 提供[Web 應用程式工廠](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類以`TestServer`簡化使用的 SUT 引導。

[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)文件介紹如何設置測試專案和測試運行程式,以及有關如何運行測試的詳細說明以及如何命名測試和測試類的建議。

> [!NOTE]
> 為應用創建測試專案時,將單元測試與集成測試分離到不同的專案中。 這有助於確保基礎結構測試元件不會意外包含在單元測試中。 單元和集成測試的分離還允許控制運行哪組測試。

Razor Pages 應用和 MVC 應用測試的配置之間幾乎沒有區別。 唯一的區別是如何命名測試。 在 Razor Pages 應用中,頁面終結點的測試通常以頁面模型類命名`IndexPageTests`(例如, 測試索引頁的元件整合)。 在 MVC 應用中,測試通常由控制器類組織,並基於它們測試的控制器命名(`HomeControllerTests`例如, 測試主控制器的元件整合)。

## <a name="test-app-prerequisites"></a>測試應用先決條件

測試項目必須:

* 參考以下套件:
  * [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [微軟.AspNetCore.Mvc.測試](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* 在專案檔中指定 Web SDK`<Project Sdk="Microsoft.NET.Sdk.Web">`( 。 引用[Microsoft.AspNetCore.App 元包](xref:fundamentals/metapackage-app)時需要 Web SDK。

這些先決條件可以在[示例應用中](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)看到。 檢查*測試/RazorPages專案.測試/RazorPages專案.測試.csproj*檔。 範例應用程式使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)解析器庫,因此範例應用還引用:

* [森特](https://www.nuget.org/packages/xunit/)
* [xunit.runner.Visualstudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [角度夏普](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a>SUT 環境

如果未設置 SUT[的環境](xref:fundamentals/environments),則環境預設為"開發」。。

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>使用預設 Web 應用程式工廠的基本測試

[Web\<應用程式 工廠 TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)用於為整合測試建立[測試伺服器](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。 `TEntryPoint`是 SUT 的入口點類別,通常`Startup`是類 。

測試類實現*類韌體*介面[(IClassFixture)](https://xunit.github.io/docs/shared-context#class-fixture)以指示類包含測試並在類中的測試中提供共用物件實例。

以下`BasicTests`測試類`WebApplicationFactory`使用引導 SUT 並提供[HttpClient](/dotnet/api/system.net.http.httpclient)`Get_EndpointsReturnSuccessAndCorrectContentType`到測試方法 的 HTTPClient。 該方法檢查回應狀態代碼是否成功(範圍為 200-299)的狀態代碼,`Content-Type`並且標頭`text/html; charset=utf-8`是 多個應用頁。

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)創建一`HttpClient`個 實例,該實例會自動遵循重定向並處理 Cookie。

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

默認情況下,啟用[GDPR同意策略](xref:security/gdpr)時,不會跨請求保留非必需Cookie。 要保留非必需的 Cookie(如 TempData 提供者使用的 Cookie),請將其標記為測試中必不可少的 Cookie。 有關將 Cookie 標記為基本 Cookie 的說明,請參閱[基本 Cookie](xref:security/gdpr#essential-cookies)。

## <a name="customize-webapplicationfactory"></a>自訂 Web 應用程式工廠

可以通過繼承`WebApplicationFactory`來 創建一個或多個自定義工廠,從而獨立於測試類創建 Web 主機配置:

1. 繼承`WebApplicationFactory`並覆蓋[設定 WebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[設定服務](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)設定服務集合 :

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   [範例應用中的](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)資料庫種子設定由`InitializeDbForTests`方法執行。 該方法在[集成測試示例:測試應用組織](#test-app-organization)部分中進行了描述。

2. 在測試類`CustomWebApplicationFactory`中使用自定義。 下面的範例在`IndexPageTests`類別使用工廠:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   示例應用的用戶端配置為防止以下`HttpClient`重定向。 如稍後在[Mock 身份驗證](#mock-authentication)部分中所述,這允許測試以檢查應用的第一個回應的結果。 第一個回應是許多具有`Location`標頭的測試中的重定向。

3. 典型的測試使用`HttpClient`與 說明器方法來處理請求和回應:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

任何POST請求SUT必須滿足反偽造檢查,這是自動由應用程式的[資料保護反偽造系統](xref:security/data-protection/introduction)。 為了安排測試的開 POST 請求,測試應用必須:

1. 對頁面發出請求。
1. 分析反偽造 Cookie 並從回應請求驗證權杖。
1. 使用防偽造 Cookie 進行 POST 請求,並請求驗證權杖到位。

`SendAsync`[範例應用程式中](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)的協助器擴充方法 (*協助器/HTTPClient 擴充.cs*) 與`GetDocumentAsync`說明器方法 (*協助器/HtmlHelpers.cs*) 使用[AngleSharp](https://anglesharp.github.io/)解析器處理反偽造檢查的方法:

* `GetDocumentAsync`&ndash;接收[HTTPResponse](/dotnet/api/system.net.http.httpresponsemessage)訊息`IHtmlDocument`並傳回 。 `GetDocumentAsync`使用基於原始`HttpResponseMessage`的準備*虛擬回應*的工廠。 有關詳細資訊,請參閱[AngleSharp 文件](https://github.com/AngleSharp/AngleSharp#documentation)。
* `SendAsync``HttpClient`撰寫[HTTPRequestMessage](/dotnet/api/system.net.http.httprequestmessage)並呼叫[SendAsync(HTTPRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)的擴充方法向 SUT 提交請求。 `SendAsync`接受 HTML 窗型`IHtmlFormElement`( ) 的重載與以下內容:
  * 提交表格按鈕`IHtmlElement`( )
  * 表單值集合`IEnumerable<KeyValuePair<string, string>>`( )
  * 提交按鈕`IHtmlElement`( )`IEnumerable<KeyValuePair<string, string>>`與表單 ( )

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/)是一個第三方解析庫,用於本主題和示例應用中的演示目的。 ASP.NET核心應用的整合測試不支援或不需要 AngleSharp。 可以使用其他解析器,例如[Html 敏捷包 (HAP)。](https://html-agility-pack.net/) 另一種方法是編寫代碼,直接處理反偽造系統的請求驗證令牌和反偽造 Cookie。

## <a name="customize-the-client-with-withwebhostbuilder"></a>使用 WebHostBuilder 自訂客戶端

當測試方法中需要其他配置時,[與 WebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)一起`WebApplicationFactory`創建一個新的[IWebHostBuilder,該構建器](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)透過配置進一步自定義。

`Post_DeleteMessageHandler_ReturnsRedirectToRoot`[範例應用程式的](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)測試方法演示了`WithWebHostBuilder`的用法。 此測試通過在 SUT 中觸發表單提交來在資料庫中執行記錄刪除。

由於`IndexPageTests`類中的另一個測試執行一個操作,該操作將刪除資料庫中的所有記錄,並且可能`Post_DeleteMessageHandler_ReturnsRedirectToRoot`在方法之前運行,因此將在此測試方法中重新設定資料庫的種子,以確保存在要刪除的 SUT 的記錄。 在 SUT`messages`中選擇 表單的第一個刪除按鈕在 SUT 請求中模擬:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>用戶端選項

下表顯示了建立`HttpClient`實體時可用的預設[Web 應用程式工廠客戶端選項](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)。

| 選項 | 描述 | 預設 |
| ------ | ----------- | ------- |
| [允許自動重定向](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | 獲取或設置`HttpClient`實例是否應自動遵循重定向回應。 | `true` |
| [基本位址](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | 獲取或設置實例的基本`HttpClient`位址。 | `http://localhost` |
| [手柄餅乾](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | 獲取或設置實例`HttpClient`是否應處理 Cookie。 | `true` |
| [最大自動重定向](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | 獲取或設置`HttpClient`實例應遵循的最大重定向回應數。 | 7 |

建立`WebApplicationFactoryClientOptions`類別並將其傳遞給[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)方法(預設值顯示在代碼範例中):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>注入模擬服務

可以通過調用主機生成器上的[配置測試服務](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)在測試中覆蓋服務。 **要注入類比服務,SUT 必須`Startup`具有具有`Startup.ConfigureServices`方法的 類。**

示例 SUT 包括返回報價表的作用域服務。 當請求索引頁時,報價嵌入到索引頁上的隱藏欄位中。

*服務/IQuote服務.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*服務/報價服務.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index.cshtml.cs*：

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*頁面/索引.cs*:

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

執行 SUT 應用程式時,將產生以下標記:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

為了在整合測試中測試服務和報價注入,測試將類比服務注入 SUT。 模擬服務`QuoteService`使用測試應用提供的服務取代應用,該服務`TestQuoteService`稱為 :

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices`呼叫,並註冊作用域服務:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

在測試執行期間生成的標記反映了`TestQuoteService`提供的報價文本,因此斷言通過:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="mock-authentication"></a>類比認證

類別測試`AuthTests`檢查安全終結點:

* 將未經身份驗證的用戶重定向到應用的登錄頁面。
* 返回經過身份驗證的用戶的內容。

在 SUT`/SecurePage`中 ,該頁使用[授權頁](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)約定對頁面應用[授權篩選器](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)。 有關詳細資訊,請參閱[Razor 頁面授權約定](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

測試`Get_SecurePageRedirectsAnUnauthenticatedUser`中[,Web 應用程式工廠客戶端選項](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)設定為設定[「允許自動重定向」](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)來`false`關閉:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet2)]

通過不允許用戶端遵循重定向,可以進行以下檢查:

* SUT 傳回的狀態代碼可以根據預期的[HTTPStatusCode.重定向](/dotnet/api/system.net.httpstatuscode)結果進行檢查,而不是重定向到登入頁面後的最終狀態代碼,該頁面將是[HTTPStatusCode.OK](/dotnet/api/system.net.httpstatuscode)。
* 檢查`Location`回應標頭中的標頭值以確認它`http://localhost/Identity/Account/Login`以 開頭,而不是最終登錄頁回應`Location`,其中標頭將不存在。

測試應用可以模擬<xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>[配置測試服務](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)中的 ,以便測試身份驗證和授權的方面。 最小專案傳回[身份認證結果.成功](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*):

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet4&highlight=11-18)]

當身份驗證方案設置為`Test`註冊`AddAuthentication`的位置 時,調用 的調用 以對用戶`ConfigureTestServices``TestAuthHandler`進行身份驗證。

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet3&highlight=7-12)]

有關的詳細資訊`WebApplicationFactoryClientOptions`,請參閱[用戶端選項](#client-options)部分。

## <a name="set-the-environment"></a>設定環境

默認情況下,SUT 的主機和應用環境配置為使用開發環境。 要覆蓋 SUT 的環境,

* 設定`ASPNETCORE_ENVIRONMENT`環境變數(例如`Staging`,`Production`, 或其他自訂`Testing`值, 如 .
* 在`CreateHostBuilder`測試應用中重寫以讀取預綴的環境`ASPNETCORE`變數。

```csharp
protected override IHostBuilder CreateHostBuilder() => 
    base.CreateHostBuilder()
        .ConfigureHostConfiguration(
            config => config.AddEnvironmentVariables("ASPNETCORE"));
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>測試基礎結構如何推斷應用內容根路徑

建構`WebApplicationFactory`函數透過在包含`TEntryPoint`與程式`System.Reflection.Assembly.FullName`集的整合測試的程式集上搜尋[WebAppFactoryContentRootattribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)來推斷應用[內容根](xref:fundamentals/index#content-root)路徑。 如果找不到具有正確鍵的屬性,`WebApplicationFactory`請回退到搜尋解決方案檔 *(.sln*) 並將`TEntryPoint`程式集名稱追加到解決方案目錄。 應用根目錄(內容根路徑)用於發現檢視和內容檔。

## <a name="disable-shadow-copying"></a>關閉陰影複製

捲影複製會導致測試在不同於輸出目錄的目錄中執行。 要使測試正常工作,必須禁用捲影複製。 [範例應用](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)使用 xUnit,並透過將*xunit.runner.json*檔與正確的設定設定包括,來禁用 xUnit 的捲影複製。 有關詳細資訊,請參閱使用[JSON 配置 xUnit。](https://xunit.github.io/docs/configuring-with-json.html)

將*xunit.runner.json*檔新增到測試項目的根目錄,內容如下:

```json
{
  "shadowCopy": false
}
```

如果使用 Visual Studio,請將檔案的 **「複製到輸出目錄」** 屬性設定為 **「始終複製**」。 如果不使用 Visual Studio,則`Content`向測試應用的專案檔新增目標:

```xml
<ItemGroup>
  <Content Update="xunit.runner.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </Content>
</ItemGroup>
```

## <a name="disposal-of-objects"></a>物品處理

執行`IClassFixture`實現測試後,當 xUnit 處置[Web 應用程式工廠](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)時,將釋放[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)和[HTTPClient。](/dotnet/api/system.net.http.httpclient) 如果開發人員實例化的物件需要處理,請在實施中`IClassFixture`釋放它們。 有關詳細資訊,請參閱實現[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。

## <a name="integration-tests-sample"></a>整合測試範例

[範例應用](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)由兩個應用組成:

| App | 專案目錄 | 描述 |
| --- | ----------------- | ----------- |
| 訊息應用(SUT) | *src/剃鬚刀專案* | 允許使用者添加、刪除一條、刪除所有郵件和分析郵件。 |
| 測試應用程式 | *測試/剃鬚刀項目測試* | 用於集成測試 SUT。 |

測試可以使用IDE的內置測試功能(如[Visual Studio)](https://visualstudio.microsoft.com)運行。 如果使用[Visual Studio 代碼](https://code.visualstudio.com/)或命令列,請在*測試/RazorPagesProject.測試*目錄中的指令提示符執行以下指令:

```dotnetcli
dotnet test
```

### <a name="message-app-sut-organization"></a>訊息應用 (SUT) 組織

SUT 是一個剃刀頁消息系統,具有以下特徵:

* 應用程式的索引頁 (*頁面 / Index.cshtml*和*頁面 / Index.cshtml.cs*) 提供了一個 UI 和頁面模型方法來控制消息的添加、刪除和分析(每條消息的平均字數)。
* 消息由具有兩個屬性`Message``Id``Text`的類 (*Data/Message.cs*) 描述。 該`Text`屬性是必需的,限制為 200 個字元。
* 消息使用[實體框架的記憶體中資料庫](/ef/core/providers/in-memory/)&#8224;存储。
* 該應用程式在其資料庫上下文類中包含數據訪問層 (DAL) `AppDbContext` (*資料/AppDbContext.cs*)。
* 如果資料庫在應用啟動時為空,則消息存儲將用三條消息進行初始化。
* 該應用程式包括`/SecurePage`只能由經過身份驗證的使用者訪問的應用。

&#8224;EF 主題[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)「解釋了如何使用記憶體中資料庫進行 MSTest 測試。 本主題使用[xUnit](https://xunit.github.io/)測試框架。 不同測試框架的測試概念和測試實現相似但不相同。

雖然應用程式不使用儲存庫模式,並且不是[工作單位 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)的有效示例,Razor Pages 支援這些開發模式。 有關詳細資訊,請參閱[設計基礎結構持久性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)(示例實現存儲庫模式)。

### <a name="test-app-organization"></a>測試應用組織

測試應用是*測試/RazorPages Project.Test*目錄中的控制台應用。

| 測試應用程式目錄 | 描述 |
| ------------------ | ----------- |
| *奧斯特測試* | 包含用於:<ul><li>由未經身份驗證的用戶訪問安全頁面。</li><li>通過模擬<xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>訪問經過身份驗證的使用者的安全頁面。</li><li>取得 GitHub 使用者設定檔並檢查設定檔的使用者登錄名。</li></ul> |
| *基本測試* | 包含路由和內容類型的測試方法。 |
| *IntegrationTests* | 包含使用自定義`WebApplicationFactory`類的 Index 頁的集成測試。 |
| *說明者/公用程式* | <ul><li>*Utilities.cs*包含用於`InitializeDbForTests`使用 測試數據為資料庫設定種子的方法。</li><li>*HtmlHelpers.cs*提供了返回 AngleSharp`IHtmlDocument`的方法,供測試方法使用。</li><li>*HttpClientExtensions.cs*提供`SendAsync`向 SUT 提交請求的重載。</li></ul> |

測試框架是[xUnit](https://xunit.github.io/)。 整合測試使用[Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost)進行,其中包括[測試伺服器](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。 由於[Microsoft.AspNetCore.Mvc.測試](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)套件用於配置測試主機和測試伺服器,`TestHost``TestServer`因此和 包不需要在測試應用的專案檔或測試應用中的開發人員配置中直接引用包引用。

**設定資料庫進行測試的 torrent**

集成測試通常需要在測試執行之前在資料庫中建立小型數據集。 例如,刪除測試調用資料庫記錄刪除,因此資料庫必須至少有一個記錄才能成功刪除請求。

範例應用程式在*資料庫*種子Utilities.cs中包含三條訊息,測試在執行時可以使用這些消息:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:test/razor-pages-tests>
* <xref:fundamentals/middleware/index>
* <xref:mvc/controllers/testing>
