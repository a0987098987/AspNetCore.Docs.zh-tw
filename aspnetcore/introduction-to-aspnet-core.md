---
title: ASP.NET Core 簡介
author: rick-anderson
description: 取得 ASP.NET Core 的簡介，這是一種跨平臺、高效能、開放原始碼的架構，可用於建立現代化、具備雲端功能的網際網路連線應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 04/17/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: index
ms.openlocfilehash: 2ad97dd7eb38b4cb69fa7af5ae1e1d1837a97443
ms.sourcegitcommit: 66fca14611eba141d455fe0bd2c37803062e439c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2020
ms.locfileid: "85944561"
---
# <a name="introduction-to-aspnet-core"></a>ASP.NET Core 簡介

由 [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Shaun Luttin](https://twitter.com/dicshaunary) 提供

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core 是一種跨平臺、高效能、[開放原始](https://github.com/dotnet/aspnetcore)碼的架構，可用於建立現代化、具備雲端功能的網際網路連線應用程式。 利用 ASP.NET Core，您可以：

* 建立 web 應用程式和服務、[物聯網（IoT）](https://www.microsoft.com/internet-of-things/)應用程式，以及行動後端。
* 在 Windows、macOS 和 Linux 上使用您最愛的開發工具。
* 部署到雲端或在內部部署。
* 在[.Net Core](/dotnet/core/introduction)上執行。

## <a name="why-choose-aspnet-core"></a>為什麼要選擇 ASP.NET Core？

數百萬名開發人員使用或已使用[ASP.NET](/aspnet/overview) 4.x 來建立 web 應用程式。 ASP.NET Core 是 ASP.NET 4.x 的重新設計，包括會導致更精簡、更模組化架構的架構變更。

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>使用 ASP.NET Core MVC 建置 Web API 和 Web UI

ASP.NET Core MVC 提供了建置 [Web API](xref:tutorials/first-web-api) 和 [Web 應用程式](xref:tutorials/razor-pages/index)的功能：

* [模型檢視控制器 (MVC) 模式](xref:mvc/overview)有助於讓您的 Web API 和 Web 應用程式可測試。
* [ Razor 頁面](xref:razor-pages/index)是以頁面為基礎的程式設計模型，可讓建立 web UI 變得更容易且更具生產力。
* [ Razor 標記](xref:mvc/views/razor)可為[ Razor 頁面](xref:razor-pages/index)和[MVC 視圖](xref:mvc/views/overview)提供高生產力的語法。
* [標記](xref:mvc/views/tag-helpers/intro)協助程式可讓伺服器端程式碼參與建立和轉譯檔案中的 HTML 元素 Razor 。
* [多個資料格式和內容交涉](xref:web-api/advanced/formatting)的內建支援可讓您的 Web API 連線到各種用戶端，包括瀏覽器和行動裝置。
* [模型繫結](xref:mvc/models/model-binding)會自動將 HTTP 要求中的資料對應至動作方法參數。
* [模型驗證](xref:mvc/models/validation)會自動執行用戶端和伺服器端驗證。

## <a name="client-side-development"></a>用戶端開發

ASP.NET Core 與熱門的用戶端架構和程式庫緊密整合，包括 [Blazor](xref:blazor/index) 、[角度](xref:spa/angular)、[反應](xref:spa/react)和[啟動](https://getbootstrap.com/)程式。 如需詳細資訊，請參閱 <xref:blazor/index> 及*用戶端開發*下的相關主題。

<a name="target-framework"></a>

## <a name="aspnet-core-target-frameworks"></a>ASP.NET Core 目標 framework

ASP.NET Core 3.x 和更新版本只能以 .NET Core 為目標。 一般來說，ASP.NET Core 是由[.NET Standard](/dotnet/standard/net-standard)程式庫所組成。 使用 .NET Standard 2.0 撰寫的程式庫可在[任何實作 .NET Standard 2.0 的 .NET 平台](/dotnet/standard/net-standard#net-implementation-support)上執行。

將目標指向 .NET Core 有多個好處，而這些好處也隨著版本更新越來越多。 NET Core 較 .NET Framework 多的好處包含：

* 跨平台。 在 Windows、macOS 和 Linux 上執行。
* 提升效能
* [並存版本控制](/dotnet/standard/choosing-core-framework-server#side-by-side-net-versions-per-application-level)
* 新的 API
* 開放原始碼

## <a name="recommended-learning-path"></a>建議學習路徑

如需開發 ASP.NET Core 應用程式的簡介，建議使用下列教學課程的順序：

1. 遵循您想要開發或維護之應用程式類型的教學課程。

   |應用程式類型  |狀況  |教學課程  |
   |----------|----------|----------|
   |Web 應用程式                   | 新的伺服器端 web UI 開發 |[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start) |
   |Web 應用程式                   | 維護 MVC 應用程式 |[開始使用 MVC](xref:tutorials/first-mvc-app/start-mvc)|
   |Web 應用程式                   | 用戶端 web UI 開發 |[開始使用Blazor](https://dotnet.microsoft.com/learn/aspnet/blazor-tutorial/intro) |
   |Web API                   | RESTful HTTP 服務 |[建立 Web API](xref:tutorials/first-web-api)&dagger; |
   |遠端程序呼叫應用程式 | 使用通訊協定緩衝區的合約優先服務 |[開始使用 gRPC 服務](xref:tutorials/grpc/grpc-start) |
   |即時應用程式             | 伺服器和連線用戶端之間的雙向通訊 |[開始使用SignalR](xref:tutorials/signalr) |

1. 遵循示範如何執行基本資料存取的教學課程。

   |狀況  |教學課程  |
   |----------|----------|
   |新開發        |[Razor具有 Entity Framework Core 的頁面](xref:data/ef-rp/intro) |
   |維護 MVC 應用程式 |[搭配 Entity Framework Core 的 MVC](xref:data/ef-mvc/intro) |

1. 閱讀適用于所有應用程式類型之 ASP.NET Core[基本](xref:fundamentals/index)概念的總覽。

1. 流覽目錄中的其他相關主題。

&dagger;另外還有[互動式 Web API 教學](/learn/modules/build-web-api-net-core)課程。 不需要任何本機安裝的開發工具。 程式碼會在瀏覽器的[Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/)中執行，而[捲曲](https://curl.haxx.se/)則用來進行測試。

## <a name="migrate-from-net-framework"></a>從 .NET Framework 遷移

如需將 ASP.NET 4.x 應用程式遷移至 ASP.NET Core 的參考指南，請參閱 <xref:migration/proper-to-2x/index> 。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 是一種跨平臺、高效能、[開放原始](https://github.com/dotnet/aspnetcore)碼的架構，可用於建立現代化、具備雲端功能的網際網路連線應用程式。 利用 ASP.NET Core，您可以：

* 建立 web 應用程式和服務、[物聯網（IoT）](https://www.microsoft.com/internet-of-things/)應用程式，以及行動後端。
* 在 Windows、macOS 和 Linux 上使用您最愛的開發工具。
* 部署到雲端或在內部部署。
* 在 [.NET Core 或 .NET Framework](/dotnet/articles/standard/choosing-core-framework-server) 上執行。

## <a name="why-choose-aspnet-core"></a>為什麼要選擇 ASP.NET Core？

數百萬名開發人員使用或已使用[ASP.NET](/aspnet/overview) 4.x 來建立 web 應用程式。 ASP.NET Core 是 ASP.NET 4.x 的重新設計，其架構變更可產生更為精簡且更加模組化的架構。

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>使用 ASP.NET Core MVC 建置 Web API 和 Web UI

ASP.NET Core MVC 提供了建置 [Web API](xref:tutorials/first-web-api) 和 [Web 應用程式](xref:tutorials/razor-pages/index)的功能：

* [模型檢視控制器 (MVC) 模式](xref:mvc/overview)有助於讓您的 Web API 和 Web 應用程式可測試。
* [ Razor 頁面](xref:razor-pages/index)是以頁面為基礎的程式設計模型，可讓建立 web UI 變得更容易且更具生產力。
* [ Razor 標記](xref:mvc/views/razor)可為[ Razor 頁面](xref:razor-pages/index)和[MVC 視圖](xref:mvc/views/overview)提供高生產力的語法。
* [標記](xref:mvc/views/tag-helpers/intro)協助程式可讓伺服器端程式碼參與建立和轉譯檔案中的 HTML 元素 Razor 。
* [多個資料格式和內容交涉](xref:web-api/advanced/formatting)的內建支援可讓您的 Web API 連線到各種用戶端，包括瀏覽器和行動裝置。
* [模型繫結](xref:mvc/models/model-binding)會自動將 HTTP 要求中的資料對應至動作方法參數。
* [模型驗證](xref:mvc/models/validation)會自動執行用戶端和伺服器端驗證。

## <a name="client-side-development"></a>用戶端開發

ASP.NET Core 與熱門的用戶端架構和程式庫緊密整合，包括 [Blazor](xref:blazor/index) 、[角度](xref:spa/angular)、[反應](xref:spa/react)和[啟動](https://getbootstrap.com/)程式。 如需詳細資訊，請參閱 <xref:blazor/index> 及*用戶端開發*下的相關主題。

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a>將目標指向 .NET Framework 的 ASP.NET Core

ASP.NET Core 2.x 的目標可以是 NET Core 或 .NET Framework。 將目標指向 .NET Framework 的 ASP.NET Core 應用程式無法跨平台&mdash;而只能在 Windows 上執行。 ASP.NET Core 2.x 通常會包含 [.NET Standard](/dotnet/standard/net-standard) 程式庫。 使用 .NET Standard 2.0 撰寫的程式庫可在[任何實作 .NET Standard 2.0 的 .NET 平台](/dotnet/standard/net-standard#net-implementation-support)上執行。

實作 .NET Standard 2.0 的 .NET Framework 版本支援 ASP.NET Core 2.x：

* 建議使用 .NET Framework 最新版本。
* .NET Framework 4.6.1 和更新版本。

ASP.NET Core 3.0 及更新版本只可在.NET Core 上執行。 如需此變更的詳細資料，請參閱[A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/) (搶先看 ASP.NET Core 3.0 的變更)。

將目標指向 .NET Core 有多個好處，而這些好處也隨著版本更新越來越多。 NET Core 較 .NET Framework 多的好處包含：

* 跨平台。 可在 macOS、Linux 及 Windows 上執行。
* 提升效能
* [並存版本控制](/dotnet/standard/choosing-core-framework-server#side-by-side-net-versions-per-application-level)
* 新的 API
* 開放原始碼

為了協助關閉從 .NET Framework 到 .NET Core 的 API 差距， [Windows 相容性套件](/dotnet/core/porting/windows-compat-pack)在 .net core 中提供了數千種僅限 Windows 的 api。 這些 API 並不適用於 .NET Core 1.x。

## <a name="recommended-learning-path"></a>建議學習路徑

我們建議遵循一系列的教學課程和文章，取得開發 ASP.NET Core 應用程式的簡介：

1. 遵循教學課程以瞭解您想要開發或維護的應用程式類型。

   |應用程式類型  |狀況  |教學課程  |
   |----------|----------|----------|
   |Web 應用程式                   | 針對全新開發        |[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start) |
   |Web 應用程式                   | 針對維護 MVC 應用程式 |[開始使用 MVC](xref:tutorials/first-mvc-app/start-mvc)|
   |Web API                   |                            |[建立 Web API](xref:tutorials/first-web-api)&dagger; |
   |即時應用程式             |                            |[開始使用SignalR](xref:tutorials/signalr) |

1. 遵循示範如何執行基本資料存取的教學課程。

   |狀況  |教學課程  |
   |----------|----------|
   | 針對全新開發        |[Razor具有 Entity Framework Core 的頁面](xref:data/ef-rp/intro) |
   | 針對維護 MVC 應用程式 |[搭配 Entity Framework Core 的 MVC](xref:data/ef-mvc/intro) |

1. 閱讀適用于所有應用程式類型之 ASP.NET Core[基本](xref:fundamentals/index)概念的總覽。

1. 瀏覽其他您感興趣主題的目錄。

&dagger;另外還有一個 Web API 的教學課程，[您會在瀏覽器中完全遵循](/learn/modules/build-web-api-net-core)，不需要本機 IDE 安裝。 程式碼會在 [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) 中執行，且會使用 [curl](https://curl.haxx.se/) 來進行測試。

## <a name="migrate-from-net-framework"></a>從 .NET Framework 遷移

如需將 ASP.NET 應用程式遷移至 ASP.NET Core 的參考指南，請參閱 <xref:migration/proper-to-2x/index> 。

::: moniker-end

## <a name="how-to-download-a-sample"></a>如何下載範例

許多文章及教學課程都有包含範例程式碼的連結。

1. [下載 ASP.NET 存放庫 ZIP 檔案](https://codeload.github.com/dotnet/AspNetCore.Docs/zip/master)。
1. 解壓縮 *Docs-master.zip* 檔案。
1. 使用範例連結中的 URL，協助您巡覽至範例目錄。

### <a name="preprocessor-directives-in-sample-code"></a>範例程式碼中的前置處理器指示詞

為了示範多個案例，範例應用程式會使用 `#define` 和預處理器指示詞， `#if-#else/#elif-#endif` 選擇性地編譯和執行範例程式碼的不同區段。 針對使用這種方法的範例，請在 c # 檔案頂端設定指示詞， `#define` 以定義與您要執行之情節相關聯的符號。 有些範例需要在多個檔案的頂端定義符號，才能執行案例。

例如下列 `#define` 符號清單指出其提供四個情節 (每個符號一個情節)。 目前的範例設定會執行 `TemplateCode` 情節：

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

若要變更此範例，以執行 `ExpandDefault` 情節，請定義 `ExpandDefault` 符號，並將剩餘的符號設為註解：

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

如需如何使用 [C# 前置處理器指示詞](/dotnet/csharp/language-reference/preprocessor-directives/)，以選擇編譯不同之程式碼區段的詳細資訊，請參閱 [#define (C# 參考)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) 及 [#if (C# 參考)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)。

### <a name="regions-in-sample-code"></a>範例程式碼中的區域

部分範例應用程式包含[#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region)和[#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) c # 指示詞括住的程式碼區段。 文件建置系統會將這些區域插入轉譯的文件主題中。  

區域名稱通常包含字組 "snippet"。 下列範例顯示了名為 `snippet_WebHostDefaults` 的區域：

```csharp
#region snippet_WebHostDefaults
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
    });
#endregion
```

上述的 C# 程式碼片段會在主題的 Markdown 檔案中，透過以下程式行加以參考：

```md
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_WebHostDefaults)]
```

您可以放心地忽略（或移除） `#region` `#endregion` 程式碼周圍的和指示詞。 如果您打算執行主題中所述的範例案例，請不要更改這些指示詞內的程式碼。 您可以在試驗其他案例時自由改變程式碼。

如需詳細資訊，請參閱 [Contribute to the ASP.NET documentation: Code snippets](https://github.com/dotnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets) (參與 ASP.NET 文件：程式碼片段)。

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱下列資源：

* <xref:getting-started>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [ASP.NET Core 基本概念](xref:fundamentals/index)
* [每週的 ASP.NET 社群之聲](https://live.asp.net/) \(英文\) 涵蓋了小組的進度和計劃， 並提供新的部落格和協力廠商軟體。
