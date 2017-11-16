---
title: "ASP.NET Core 2.0 的新功能"
author: rick-anderson
description: "ASP.NET Core 2.0 的新功能"
keywords: "ASP.NET Core, 版本資訊, 新功能"
ms.author: riande
manager: wpickett
ms.date: 07/10/2017
ms.topic: article
ms.assetid: 08c9f457-9c24-40f9-a08b-47dc251e4cec
ms.technology: aspnet
ms.prod: aspnet-core
uid: aspnetcore-2.0
ms.openlocfilehash: 98af3788652e87f6222551cb4a8e5427b312660c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="whats-new-in-aspnet-core-20"></a>ASP.NET Core 2.0 的新功能

本文會重點說明 ASP.NET Core 2.0 最重要的變更，附有相關文件的連結。

## <a name="razor-pages"></a>Razor 頁面

Razor 頁面是 ASP.NET Core MVC 的新功能，更容易編寫以頁面為焦點的案例程式碼，也更具生產力。

如需詳細資訊，請參閱簡介與教學課程：

* [Razor 頁面簡介](xref:mvc/razor-pages/index)
* [開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a>ASP.NET Core 中繼套件

新的 ASP.NET Core 中繼套件包含 ASP.NET Core 與 Entity Framework Core 小組建立與支援的所有套件，以及其內部和協力廠商的相依性。 您不再需要依套件選擇個別的 ASP.NET Core 功能。 所有功能都包含在 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 套件中。 預設範本會使用此套件。

如需詳細資訊，請參閱 [ASP.NET Core 2.0 的 Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)。

## <a name="runtime-store"></a>執行階段存放區

使用 `Microsoft.AspNetCore.All` 中繼套件的應用程式會自動利用新的 .NET Core 執行階段存放區。 此存放區包含執行 ASP.NET Core 2.0 應用程式所需的所有執行階段資產。 當您使用 `Microsoft.AspNetCore.All` 中繼套件時，不會使用應用程式部署參考 ASP.NET Core NuGet 套件的任何資產，因為它們已經位於目標系統上。 執行階段存放區中的資產也會先行編譯，以改善應用程式啟動時間。

如需詳細資訊，請參閱[執行階段存放區](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)。

## <a name="net-standard-20"></a>.NET Standard 2.0

ASP.NET Core 2.0 套件以 .NET Standard 2.0 為目標。 套件可供其他 .NET Standard 2.0 程式庫參考，並可在與 .NET Standard 2.0 相容的 .NET 實作上執行，包括 .NET Core 2.0 和 .NET Framework 4.6.1。 

`Microsoft.AspNetCore.All` 中繼套件只以 .NET Core 2.0 為目標，因為它原本就要搭配 .NET Core 2.0 執行階段存放區。

## <a name="configuration-update"></a>組態更新

`IConfiguration` 執行個體預設新增至 ASP.NET Core 2.0 的服務容器中。 服務容器中的 `IConfiguration` 可讓應用程式從容器輕鬆擷取組態值。

如需已規劃文件狀態的相關資訊，請參閱 [GitHub 問題](https://github.com/aspnet/Docs/issues/3387)。

## <a name="logging-update"></a>記錄更新

在 ASP.NET Core 2.0 中，記錄預設會合併至相依性插入 (DI) 系統。 您要在 *Program.cs* 檔案中新增提供者並設定篩選，不是在 *Startup.cs* 檔案。 而預設的 `ILoggerFactory` 支援篩選的方式，可讓您使用一個彈性的方法同時應對跨提供者的篩選和特定提供者的篩選。

如需詳細資訊，請參閱[記錄簡介](xref:fundamentals/logging/index)。

## <a name="authentication-update"></a>驗證更新

新的驗證模型讓使用 DI 的應用程式更容易設定驗證。

使用 [Azure AD B2C] 的 Web 應用程式和 Web API 可以使用新範本來設定驗證 (https://azure.microsoft.com/services/active-directory-b2c/)。

如需已規劃文件狀態的資訊，請參閱 [GitHub 問題](https://github.com/aspnet/Docs/issues/3054)。

## <a name="identity-update"></a>身分識別更新

我們讓建置使用 ASP.NET Core 2.0 身分識別的安全 Web API 變得更容易。 您可以取得存取權杖來存取使用 [Microsoft 驗證程式庫 (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client) 的 Web API。

如需 2.0 驗證變更的詳細資訊，請參閱下列資源：

* [ASP.NET Core 中的帳戶確認和密碼復原](xref:security/authentication/accconfirm)
* [啟用 ASP.NET Core 驗證器應用程式的 QR 代碼產生](xref:security/authentication/identity-enable-qrcodes)
* [將驗證和身分識別移轉至 ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a>SPA 範本

有具有 Redux 的 Angular、Aurelia、Knockout.js、React.js 和 React.js 單一頁面應用程式 (SPA) 專案範本可用。 Angular 範本已更新至 Angular 4。 預設有 Angular 和 React 範本可用，如需如何取得其他範本的資訊，請參閱[建立新的 SPA 專案](xref:client-side/spa-services#creating-a-new-project)。 如需如何在 ASP.NET Core 中建置 SPA 的資訊，請參閱[使用建立單一頁面應用程式的 JavaScriptServices](xref:client-side/spa-services)。

## <a name="kestrel-improvements"></a>Kestrel 改善

Kestrel 網頁伺服器的新功能，讓它更適合作為網際網路對向伺服器。 我們已在 `KestrelServerOptions` 類別的新 `Limits` 屬性中新增多個伺服器條件約束組態選項。 您現在可以新增下列限制：

- 用戶端連線數目上限
- 要求主體大小上限
- 要求主體資料速率下限

如需詳細資訊，請參閱[在 ASP.NET Core 中實作 Kestrel 網頁伺服器](xref:fundamentals/servers/kestrel)。

## <a name="weblistener-renamed-to-httpsys"></a>WebListener 已重新命名為 HTTP.sys

套件 `Microsoft.AspNetCore.Server.WebListener` 和 `Microsoft.Net.Http.Server` 已合併至新的套件 `Microsoft.AspNetCore.Server.HttpSys`。 命名空間已更新成符合項目。

如需詳細資訊，請參閱[在 ASP.NET Core 中實作 HTTP.sys 網頁伺服器](xref:fundamentals/servers/httpsys)。

## <a name="enhanced-http-header-support"></a>增強的 HTTP 標頭支援

使用 MVC 傳輸 `FileStreamResult` 或 `FileContentResult` 時，您現在可以選擇在傳輸內容上設定 `ETag` 或 `LastModified` 日期。 您可以在傳回的內容上設定這些值，使用與下例類似的程式碼：

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

傳回給訪客的檔案，`ETag` 和 `LastModified` 值會以適當的 HTTP 標頭裝飾。

如果應用程式訪客要求內容與範圍要求標頭，ASP.NET 會辨識並處理該標頭。 如果要求的內容可以部分傳送，ASP.NET 會適當略過，並只傳回要求的位元組集合。  您不需要將任何特殊的處理常式寫入方法，以調整或處理這項功能；它會為您自動處理。

## <a name="hosting-startup-and-application-insights"></a>裝載啟動和 Application Insights

裝載環境現在可以在應用程式啟動時，插入額外的套件相依性並執行程式碼，應用程式不需要明確接受相依性或呼叫任何方法。 這項功能可以用來啟用特定的環境以「點亮」該環境特有的功能，應用程式不需要事先知道。 

在 ASP.NET Core 2.0 中，在 Visual Studio 中偵錯以及 (加入後) 在 Azure 應用程式服務中執行時，此功能可用來自動啟用 Application Insights 診斷。 如此一來，專案範本預設不會再新增 Application Insights 套件和程式碼。

如需已規劃文件狀態的資訊，請參閱 [GitHub 問題](https://github.com/aspnet/Docs/issues/3389)。

## <a name="automatic-use-of-anti-forgery-tokens"></a>自動使用防偽權杖

根據預設，ASP.NET Core 一律協助以 HTML 編碼內容，但在新版本中，我們要採用額外的步驟，協助防止跨網站要求偽造 (XSRF) 攻擊。 ASP.NET Core 現在預設會發出防偽權杖，並對表單 POST 動作和頁面驗證它們，不需要額外組態。

如需詳細資訊，請參閱[防止 ASP.NET Core 中的跨網站要求偽造 (XSRF/CSRF) 攻擊](xref:security/anti-request-forgery)。

## <a name="automatic-precompilation"></a>自動先行編譯

預設在發佈期間即已啟用 Razor 檢視預先編譯，減少發佈輸出大小和應用程式啟動時間。

## <a name="razor-support-for-c-71"></a>針對 C# 7.1 的 Razor 支援

Razor 檢視引擎已更新，可使用新的 Roslyn 編譯器 。 這包括支援預設運算式、推斷 Tuple 名稱和使用泛型比對模式等 C# 7.1 功能。 若要在專案中使用 C# 7.1，請在專案檔中新增下列屬性，然後重新載入方案：

```xml
<LangVersion>latest</LangVersion>
```

如需 C# 7.1 功能狀態的資訊，請參閱 [Roslyn GitHub 存放庫](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md)。

## <a name="other-documentation-updates-for-20"></a>針對 2.0 的其他文件更新

* [建立 Visual Studio 和 MSBuild 的發行設定檔，以部署 ASP.NET Core 應用程式](xref:publishing/web-publishing-vs)
* [金鑰管理](xref:security/data-protection/implementation/key-management)
* [設定 Facebook 驗證](xref:security/authentication/facebook-logins)
* [設定 Twitter 驗證](xref:security/authentication/twitter-logins)
* [設定 Google 驗證](xref:security/authentication/google-logins)
* [設定 Microsoft 帳戶驗證](xref:security/authentication/microsoft-logins)

## <a name="migration-guidance"></a>移轉指引

如需如何將 ASP.NET Core 1.x 應用程式移轉至 ASP.NET Core 2.0的指引，請參閱下列資源：

* [從 ASP.NET Core 1.x 移轉至 ASP.NET Core 2.0](xref:migration/1x-to-2x/index)
* [將驗證和身分識別移轉至 ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a>其他資訊

如需完整的變更清單，請參閱 [ASP.NET Core 2.0 版本資訊](https://github.com/aspnet/Home/releases/tag/2.0.0)。

如果您想要了解 ASP.NET Core 開發小組的進度和計劃，請收聽每週的 [ASP.NET Community Standup](https://live.asp.net/) (ASP.NET 社群之聲)。
