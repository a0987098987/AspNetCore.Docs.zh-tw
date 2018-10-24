---
title: 在 ASP.NET 4.x 和 ASP.NET Core 之間進行選擇
author: rick-anderson
description: 說明 ASP.NET Core 與ASP.NET 4.x，以及如何在兩者之間進行選擇。
ms.author: riande
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: f046491e2ec68b6beaad581e2b04e6688a81f2d1
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911041"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a>在 ASP.NET 4.x 和 ASP.NET Core 之間進行選擇

ASP.NET Core 是 ASP.NET 4.x 的重新設計。 本文列出兩者之間的差異。

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core 是一種開放原始碼、跨平台的架構，用於在 Windows、macOS 或 Linux 上建置現代化的雲端式 Web 應用程式。

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a>ASP.NET 4.x

ASP.NET 4.x 是一個成熟的架構，其提供在 Windows 上建置企業級伺服器端 Web 應用程式所需的服務。

## <a name="framework-selection"></a>選取 Framework

下表將比較 ASP.NET Core 與 ASP.NET 4.x。

| ASP.NET Core | ASP.NET 4.x |
|---|---|
|為 Windows、macOS 或 Linux 建置|為 Windows 建置|
|從 ASP.NET Core 2.x 開始，[Razor 頁面](xref:razor-pages/index)是建立 Web UI 的建議方法。 另請參閱 [MVC](xref:mvc/overview)、[Web API](xref:tutorials/first-web-api) 和 [SignalR](xref:signalr/introduction)。|使用 [Web Forms](/aspnet/web-forms)、[SignalR](/aspnet/signalr)、[MVC](/aspnet/mvc)、[Web API](/aspnet/web-api/)、[WebHooks](/aspnet/webhooks/) 或 [Web Pages](/aspnet/web-pages)|
|每部電腦多個版本|每部電腦一個版本|
|在 Visual Studio、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) 或 [Visual Studio Code](https://code.visualstudio.com/) 中使用 C# 或 F# 進行開發|在 Visual Studio 中使用 C#、VB 或 F# 進行開發|
|效能比 ASP.NET 4.x 更高|效能良好|
|[選擇 .NET Framework 或.NET Core 執行階段](/dotnet/articles/standard/choosing-core-framework-server)|使用 .NET Framework 執行階段|

如需 .NET Framework 上的 ASP.NET Core 2.x 支援資訊，請參閱[將目標指向 .NET Framework 的 ASP.NET Core](xref:index#target-framework)。

## <a name="aspnet-core-scenarios"></a>ASP.NET Core 案例

* 從 ASP.NET Core 2.x 開始，[Razor 頁面](xref:razor-pages/index)是建立 Web UI 的建議方法。
* [網站](xref:tutorials/first-mvc-app/index)
* [API](xref:tutorials/first-web-api)
* [即時](xref:signalr/index)
* [將 ASP.NET Core 應用程式部署到 Azure](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a>ASP.NET 4.x 案例

* [網站](/aspnet/mvc)
* [API](/aspnet/web-api)
* [即時](/aspnet/signalr)
* [在 Azure 中建立 ASP.NET 4.x Web 應用程式](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a>其他資源

* [ASP.NET 簡介](/aspnet/overview)
* [ASP.NET Core 簡介](xref:index)
* <xref:host-and-deploy/azure-apps/index>
