---
title: 在 ASP.NET 和 ASP.NET Core 之間進行選擇
author: rick-anderson
description: 了解如何在 ASP.NET 和 ASP.NET Core 之間進行選擇。
ms.author: riande
ms.date: 05/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 6d759c0bc5e5c7d32d6c14786db6ba9fe7a2f1e8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36297227"
---
# <a name="choose-between-aspnet-and-aspnet-core"></a>在 ASP.NET 和 ASP.NET Core 之間進行選擇

不論您要建立哪種 Web 應用程式，ASP.NET 都為您提供解決方案：從以 Windows Server 為目標的企業 Web 應用程式，到以 Linux 容器為目標的小型微服務，以及這兩者之間的所有項目。

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core 是一種開放原始碼、跨平台的架構，用於在 Windows、macOS 或 Linux 上建置現代化的雲端式 Web 應用程式。

## <a name="aspnet"></a>ASP.NET

ASP.NET 是一種成熟的架構，其提供建置企業級伺服器端 Web 應用程式所需的所有服務。

## <a name="framework-selection"></a>選取 Framework

請檢閱下表來判斷哪一個架構最符合您的需求。

| ASP.NET Core | ASP.NET |
|---|---|
|為 Windows、macOS 或 Linux 建置|為 Windows 建置|
|從 ASP.NET Core 2.x 開始，[Razor 頁面](xref:razor-pages/index)是建立 Web UI 的建議方法。 另請參閱 [MVC](xref:mvc/overview)、[Web API](xref:tutorials/first-web-api) 和 [SignalR](xref:signalr/introduction)。|使用 [Web Forms](/aspnet/web-forms)、[SignalR](/aspnet/signalr)、[MVC](/aspnet/mvc)、[Web API](/aspnet/web-api/)、[WebHooks](/aspnet/webhooks/) 或 [Web Pages](/aspnet/web-pages)|
|每部電腦多個版本|每部電腦一個版本|
|在 Visual Studio、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) 或 [Visual Studio Code](https://code.visualstudio.com/) 中使用 C# 或 F# 進行開發|在 Visual Studio 中使用 C#、VB 或 F# 進行開發|
|效能比 ASP.NET 更高|效能良好|
|[選擇 .NET Framework 或.NET Core 執行階段](/dotnet/articles/standard/choosing-core-framework-server)|使用 .NET Framework 執行階段|

## <a name="aspnet-core-scenarios"></a>ASP.NET Core 案例

* 從 ASP.NET Core 2.x 開始，[Razor 頁面](xref:razor-pages/index)是建立 Web UI 的建議方法。
* [網站](xref:tutorials/first-mvc-app/index)
* [API](xref:tutorials/first-web-api)
* [即時](xref:signalr/index)

## <a name="aspnet-scenarios"></a>ASP.NET 案例

* [網站](/aspnet/mvc)
* [API](/aspnet/web-api)
* [即時](/aspnet/signalr)

## <a name="resources"></a>資源

* [ASP.NET 簡介](/aspnet/overview)
* [ASP.NET Core 簡介](xref:index)
