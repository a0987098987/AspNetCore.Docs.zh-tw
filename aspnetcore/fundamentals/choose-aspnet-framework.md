---
title: 在 ASP.NET 4.x 和 ASP.NET Core 之間進行選擇
author: rick-anderson
description: 說明 ASP.NET Core 與 ASP.NET 4.x 的比較，以及如何在兩者之間進行選擇。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 02/12/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 1fb81d5a54cf332ca473af8fbe1841813a127be7
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775878"
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
|頁面是建立 Web UI 的建議方法，從 ASP.NET Core 2.x. x。 [ Razor ](xref:razor-pages/index) 另請參閱[MVC](xref:mvc/overview)、 [Web API](xref:tutorials/first-web-api)和[SignalR](xref:signalr/introduction)。|使用[web Forms](/aspnet/web-forms)、 [SignalR](/aspnet/signalr)、 [MVC](/aspnet/mvc)、 [web API](/aspnet/web-api/)、 [webhook](/aspnet/webhooks/)或[Web Pages](/aspnet/web-pages)|
|每部電腦多個版本|每部電腦一個版本|
|使用 c # 或 F 以[Visual Studio](https://visualstudio.microsoft.com/vs/)、 [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)或[Visual Studio Code](https://code.visualstudio.com/)進行開發#|使用 c #、VB 或 F [Visual Studio](https://visualstudio.microsoft.com/vs/)進行開發#|
|效能比 ASP.NET 4.x 更高|效能良好|
|[使用 .NET Core 執行階段](/dotnet/standard/choosing-core-framework-server)|使用 .NET Framework 執行階段|

如需 .NET Framework 上的 ASP.NET Core 2.x 支援資訊，請參閱[將目標指向 .NET Framework 的 ASP.NET Core](xref:index#target-framework)。

## <a name="aspnet-core-scenarios"></a>ASP.NET Core 案例

* [網站](xref:tutorials/first-mvc-app/index)
* [API](xref:tutorials/first-web-api)
* [即時](xref:signalr/introduction)
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
