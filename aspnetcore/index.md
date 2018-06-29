---
title: ASP.NET Core 簡介
author: rick-anderson
description: 取得 ASP.NET Core 的簡介，ASP.NET Core 是一種跨平台且高效能的開放原始碼架構，用於建置現代化、雲端式、網際網路連線的應用程式。
ms.author: riande
ms.date: 02/28/2018
uid: index
ms.openlocfilehash: 6de7f1bc8229c5de519e4064dda0a7061cf8b9c6
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077708"
---
# <a name="introduction-to-aspnet-core"></a>ASP.NET Core 簡介

由 [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Shaun Luttin](https://twitter.com/dicshaunary) 提供

ASP.NET Core 是一種跨平台且高效能的[開放原始碼](https://github.com/aspnet/home)架構，用於建置現代化、雲端式、網際網路連線的應用程式。 利用 ASP.NET Core，您可以：

* 建置 Web 應用程式和服務、[IoT](https://www.microsoft.com/internet-of-things/) 應用程式，以及行動後端。
* 在 Windows、macOS 和 Linux 上使用您最愛的開發工具。
* 部署到雲端或在內部部署。
* 在 [.NET Core 或 .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) 上執行。

## <a name="why-use-aspnet-core"></a>為何使用 ASP.NET Core？

數百萬的開發人員已使用 (並持續使用) [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) 來建立 Web 應用程式。 ASP.NET Core 是 ASP.NET 4.x 的重新設計，其架構變更可產生更為精簡且更加模組化的架構。

ASP.NET Core 提供下列優點：

* 用於建置 Web UI 和 Web API 的統一劇本。
* 整合[現代化用戶端架構](xref:client-side/index)和開發工作流程。
* 雲端就緒、以環境為基礎的[組態系統](xref:fundamentals/configuration/index)。
* 內建的[相依性插入](xref:fundamentals/dependency-injection)。
* 輕量型、[高效能](https://github.com/aspnet/benchmarks) \(英文\) 且模組化的 HTTP 要求管線。
* 能夠在 [IIS](xref:host-and-deploy/iis/index)、[Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache)、[Docker](xref:host-and-deploy/docker/index) 上裝載，或自我裝載於您自己的處理序中。
* 以 [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) 為目標時，會有並存應用程式版本設定。
* 可簡化現代網頁程式開發的工具。
* 能夠在 Windows、macOS 和 Linux 上建置並執行。
* 開放原始碼和[社群導向](https://live.asp.net/) \(英文\)。

ASP.NET Core 完全以 [NuGet](https://www.nuget.org/) 套件的形式提供。 使用 NuGet 套件可讓您將應用程式最佳化，使其僅包含必要相依性。 事實上，以 .NET Core 為目標的 ASP.NET Core 2.x 應用程式只需要[單一 NuGet 套件](xref:fundamentals/metapackage)。 應用程式介面區較小的優點包括更嚴密的安全性、減少維護工作，以及提升效能。

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>使用 ASP.NET Core MVC 建置 Web API 和 Web UI

ASP.NET Core MVC 提供了建置 [Web API](xref:tutorials/index#build-web-apis) 和 [Web 應用程式](xref:tutorials/index#build-web-apps)的功能：

* [模型檢視控制器 (MVC) 模式](xref:mvc/overview)有助於讓您的 Web API 和 Web 應用程式[可測試](xref:test/index)。
* [Razor 頁面](xref:razor-pages/index) (ASP.NET Core 2.0 中的新功能) 是以頁面為基礎的程式設計模型，可讓建置 Web UI 更容易且更具工作效率。
* [Razor 標記](xref:mvc/views/razor)提供了適用於 [Razor 頁面](xref:razor-pages/index)和 [MVC 檢視](xref:mvc/views/overview)的高效率語法。
* [標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。
* [多個資料格式和內容交涉](xref:web-api/advanced/formatting)的內建支援可讓您的 Web API 連線到各種用戶端，包括瀏覽器和行動裝置。
* [模型繫結](xref:mvc/models/model-binding)會自動將 HTTP 要求中的資料對應至動作方法參數。
* [模型驗證](xref:mvc/models/validation)會自動執行用戶端和伺服器端驗證。

## <a name="client-side-development"></a>用戶端開發

ASP.NET Core 可完美整合常用的用戶端架構和程式庫，包括 [Angular](xref:spa/angular)、[React](xref:spa/react) 與 [Bootstrap](xref:client-side/bootstrap)。 如需詳細資訊，請參閱[用戶端開發](xref:client-side/index)。

## <a name="aspnet-core-targeting-net-framework"></a>將目標指向 .NET Framework 的 ASP.NET Core

ASP.NET Core 可將目標指向 NET Core 或 .NET Framework。 將目標指向 .NET Framework 的 ASP.NET Core 應用程式無法跨平台&mdash;而只能在 Windows 上執行。 目前沒有任何要將 ASP.NET Core 中，以 .NET Framework 為目標之支援移除的計畫。 一般來說，ASP.NET Core 以 [.NET Standard](/dotnet/standard/net-standard) 程式庫組成。 以 .NET Standard 2.0 編寫的應用程式可在任何支援 .NET Standard 2.0 的位置執行。

將目標指向 .NET Core 有多個好處，而這些好處也隨著版本更新越來越多。 NET Core 較 .NET Framework 多的好處包含：

* 跨平台。 可在 macOS、Linux 及 Windows 上執行。
* 提升效能
* 並存版本
* 新的 API
* 開啟原始檔

我們正致力於縮短 .NET Framework 與 .NET Core 之間的 API 差距。 [Windows 相容性套件](/dotnet/core/porting/windows-compat-pack)在 .NET Core 中發佈了上千個僅供 Windows 使用的 API。 這些 API 並不適用於 .NET Core 1.x。

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱下列資源：

* [開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [ASP.NET Core 教學課程](xref:tutorials/index)
* [ASP.NET Core 基本概念](xref:fundamentals/index)
* [每週的 ASP.NET 社群之聲](https://live.asp.net/) \(英文\) 涵蓋了小組的進度和計劃， 並提供新的部落格和協力廠商軟體。
