---
title: ASP.NET Core 2.1 的新功能
author: isaac2004
description: 了解 ASP.NET Core 2.1 的新功能。
ms.author: riande
ms.custom: mvc
ms.date: 04/30/2019
uid: aspnetcore-2.1
ms.openlocfilehash: 359f961db768b9048427c8ab296ee3e035879408
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086398"
---
# <a name="whats-new-in-aspnet-core-21"></a>ASP.NET Core 2.1 的新功能

本文會重點說明 ASP.NET Core 2.1 最重要的變更，附有相關文件的連結。

## <a name="signalr"></a>SignalR

SignalR 已針對 ASP.NET Core 2.1 改寫。 ASP.NET Core SignalR 包含數項改善：

* 簡化的向外延展模型。
* 沒有 jQuery 相依性的新 JavaScript 用戶端。
* 以 MessagePack 為基礎的新精簡二進位通訊協定。
* 支援自訂通訊協定。
* 新的資料流處理回應模型。
* 支援以裸機 WebSockets 為基礎的用戶端。

如需詳細資訊，請參閱 [ASP.NET Core SignalR](xref:signalr/introduction)。

## <a name="razor-class-libraries"></a>Razor 類別庫

ASP.NET Core 2.1 讓您更容易在程式庫中建置並包含 Razor 型 UI，跨多個專案共用它。 新的 Razor SDK 能在可封裝到 NuGet 套件的類別庫專案中建置 Razor 檔案。 應用程式會自動探索並覆寫程式庫中的檢視和頁面。 藉由將 Razor 編譯整合至組建：

* 大幅縮短應用程式的啟動時間。
* 快速更新執行階段的 Razor 檢視和頁面仍然屬於反覆開發工作流程的一部分。

如需詳細資訊，請參閱[使用 Razor 類別庫專案建立可重複使用的 UI](xref:razor-pages/ui-class)。

## <a name="identity-ui-library--scaffolding"></a>身分識別 UI 程式庫與 Scaffolding

ASP.NET Core 2.1 將 [ASP.NET Core 身分識別](xref:security/authentication/identity)提供為 [Razor 類別庫](xref:razor-pages/ui-class)。 包含身分識別的應用程式可以套用新的身分識別 Scaffolder，以選擇性地新增包含在身分識別 Razor 類別庫 (RCL) 中的原始程式碼。 建議您產生原始程式碼，以便能夠修改程式碼並變更行為。 例如，您可以指示 Scaffolder 產生註冊使用的程式碼。 產生的程式碼優先於身分識別 RCL 中的相同程式碼。

**不**包含驗證的應用程式可以套用身分識別 Scaffolder 以新增 RCL 身分識別套件。 您可以選擇選取要產生的身分識別程式碼。

如需詳細資訊，請參閱 [ASP.NET Core 專案中的 Scaffold 身分識別](xref:security/authentication/scaffold-identity)。

## <a name="https"></a>HTTPS

隨著安全性與隱私權日益受到重視，為 Web 應用程式啟用 HTTPS 極其重要。 在網站上強制執行 HTTPS 變得日益嚴格。 不使用 HTTPS 的站台會被認為不安全。 瀏覽器 (Chromium、Mozilla) 開始強制執行必須從安全內容使用 Web 功能。 [GDPR](xref:security/gdpr) 需要使用 HTTPS 來保護使用者隱私。 雖然在生產環境中使用 HTTPS 非常重要，但在開發中使用 HTTPS 有助於避免發生部署問題 (例如不安全的連結)。 ASP.NET Core 2.1 包含多項改善，讓您更容易在開發中使用 HTTPS，以及在生產環境中設定 HTTPS。 如需詳細資訊，請參閱[強制執行 HTTPS](xref:security/enforcing-ssl)。

### <a name="on-by-default"></a>預設為開啟

為方便保護網站開發，HTTPS 現在預設為啟用。 自 2.1 開始，當有本機開發憑證存在時，Kestrel 會接聽 `https://localhost:5001`。 開發憑證會建立於：

* 您第一次使用 SDK，是 .NET Core SDK 首次執行體驗的一部分。
* 手動使用新的 `dev-certs` 工具。

執行 `dotnet dev-certs https --trust` 以信任憑證。

### <a name="https-redirection-and-enforcement"></a>HTTPS 的重新導向和強制執行

Web 應用程式通常需要同時接聽 HTTP 和 HTTPS，然後將所有 HTTP 流量重新導向至 HTTPS。 在 2.1 中，特殊化的 HTTPS 重新導向中介軟體，會根據有無組態或是否引入繫結的伺服器連接埠，以聰明的方式重新導向。

使用 [HTTP 嚴格傳輸安全性通訊協定 (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) 會進一步強制使用 HTTPS。 HSTS 指示瀏覽器一律透過 HTTPS 存取站台。 ASP.NET Core 2.1 會新增支援最大存留期、子網域和 HSTS 預先載入清單選項的 HSTS 中介軟體。

### <a name="configuration-for-production"></a>生產環境的組態

在生產環境中，必須明確設定 HTTPS。 在 2.1 中，已新增為 Kestrel 設定 HTTPS 的預設組態結構描述。 應用程式可以設定為使用下列：

* 包括 URL 在內的多個端點。 如需詳細資訊，請參閱 [Kestrel 網頁伺服器實作：端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)。
* 用於 HTTPS 的憑證來自磁碟檔案或憑證存放區。

## <a name="gdpr"></a>GDPR

ASP.NET Core 提供 API 和範本以利符合某些 [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) (EU 一般資料保護規定 (GDPR)) 需求。 如需詳細資訊，請參閱 [ASP.NET Core 的 GDPR 支援](xref:security/gdpr)。 [範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample)示範如何使用及讓您測試新增至 ASP.NET Core 2.1 範本的大部分 GDPR 擴充點和 API。

## <a name="integration-tests"></a>整合測試

引進新套件簡化測試的建立和執行。 [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) 套件處理下列工作：

* 將相依性檔案 (*\*.deps*) 從已測試的應用程式複製到測試專案的 *bin* 資料夾。
* 將內容的根目錄設定為經過測試之應用程式的專案根目錄，以便在執行測試時找到靜態檔案和頁面/檢視。
* 提供 [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) 類別來簡化以 [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) 啟動載入經過測試的應用程式。

下列測試使用 [xUnit](https://xunit.github.io/) 檢查具有成功狀態碼與正確 Content-type 標頭的 [索引] 頁面載入：

```csharp
public class BasicTests
    : IClassFixture<WebApplicationFactory<RazorPagesProject.Startup>>
{
    private readonly HttpClient _client;

    public BasicTests(WebApplicationFactory<RazorPagesProject.Startup> factory)
    {
        _client = factory.CreateClient();
    }

    [Fact]
    public async Task GetHomePage()
    {
        // Act
        var response = await _client.GetAsync("/");

        // Assert
        response.EnsureSuccessStatusCode(); // Status Code 200-299
        Assert.Equal("text/html; charset=utf-8",
            response.Content.Headers.ContentType.ToString());
    }
}
```

如需詳細資訊，請參閱[整合測試](xref:test/integration-tests)主題。

## <a name="apicontroller-actionresultt"></a>[ApiController], ActionResult\<T>

ASP.NET Core 2.1 新增新的程式設計慣例，讓您輕鬆建置全新和描述性的 Web API。 `ActionResult<T>` 是新增的新類型，允許應用程式傳回回應類型或任何其他動作的結果 (類似於 IActionResult)，同時仍然表示回應類型。 `[ApiController]` 屬性也以選擇加入的方式新增至 Web API 特定慣例和行為中。

如需詳細資訊，請參閱[建置 Web API 與 ASP.NET Core](xref:web-api/index)。

## <a name="ihttpclientfactory"></a>IHttpClientFactory

ASP.NET Core 2.1 包含新的 `IHttpClientFactory` 服務，讓您在應用程式中輕鬆設定和使用 `HttpClient` 的執行個體。 `HttpClient` 已經有委派可針對傳出 HTTP 要求連結在一起之處理常式的概念。 Factory：

* 讓每個具名用戶端以更直覺的方式註冊 `HttpClient` 執行個體。
* 實作允許 Polly 原則用於重試、CircuitBreakers 等等的 Polly 處理常式。

如需詳細資訊，請參閱[初始化 HTTP 要求](xref:fundamentals/http-requests)。

## <a name="kestrel-transport-configuration"></a>Kestrel 傳輸組態

隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。 如需詳細資訊，請參閱 [Kestrel 網頁伺服器實作：傳輸組態](xref:fundamentals/servers/kestrel#transport-configuration)。

## <a name="generic-host-builder"></a>泛型主機建立器

已引入泛型主機建立器 (`HostBuilder`)。 此建立器可用於不處理 HTTP 要求的應用程式 (傳訊、背景工作等等)。

如需詳細資訊，請參閱 [.NET 泛型主機](xref:fundamentals/host/generic-host)。

## <a name="updated-spa-templates"></a>已更新 SPA 範本

已更新適用於 Angular、React 和 React with Redux 的單頁應用程式範本，以使用標準的專案結構，並為每個架構建置系統。

Angular 範本以 Angular CLI 為基礎，React 範本以 create-react-app 為基礎。

如需詳細資訊，請參閱:

* <xref:spa/angular>
* <xref:spa/react>
* <xref:spa/react-with-redux>

## <a name="razor-pages-search-for-razor-assets"></a>Razor Pages 搜尋 Razor 資產

在 2.1 中，Razor Pages 會依所列順序在下列目錄中搜尋 Razor 資產 (如版面配置和部分)：

1. 目前的 Pages 資料夾。
1. */Pages/Shared/*
1. */Views/Shared/*

## <a name="razor-pages-in-an-area"></a>區域中的 Razor Pages

Razor Pages 現可支援[區域](xref:mvc/controllers/areas)。 若要查看區域範例，請使用個別的使用者帳戶建立新的 Razor Pages Web 應用程式。 具有個別使用者帳戶的 Razor Pages Web 應用程式包含 */Areas/Identity/Pages*。

## <a name="mvc-compatibility-version"></a>MVC 相容性版本

<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 方法可讓應用程式加入或退出 ASP.NET Core MVC 2.1 或更新版本所引入的可能重大行為變更。

如需詳細資訊，請參閱<xref:mvc/compatibility-version>。

## <a name="migrate-from-20-to-21"></a>從 2.0 遷移至 2.1

請參閱[從 ASP.NET Core 2.0 遷移到 2.1](xref:migration/20_21)。

## <a name="additional-information"></a>其他資訊

如需完整的變更清單，請參閱 [ASP.NET Core 2.1 版本資訊](https://github.com/aspnet/Home/releases/tag/2.1.0)。
