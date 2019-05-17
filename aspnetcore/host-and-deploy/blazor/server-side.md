---
title: 裝載和部署 Blazor 伺服器端
author: guardrex
description: 了解如何使用 ASP.NET Core 來裝載和部署 Blazor 伺服器端應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/26/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8e44be09a4cceba2509f3e86abf3ce5fd2d077bd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887763"
---
# <a name="host-and-deploy-blazor-server-side"></a>裝載和部署 Blazor 伺服器端

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>主機組態值

使用[伺服器端裝載模型](xref:blazor/hosting-models#server-side)的伺服器端應用程式可以接受[一般主機組態值](xref:fundamentals/host/generic-host#host-configuration)。

## <a name="deployment"></a>部署

使用[伺服器端裝載模型](xref:blazor/hosting-models#server-side)，Blazor 會從 ASP.NET Core 應用程式內在伺服器上執行。 UI 更新、事件處理及 JavaScript 呼叫會透過 [SignalR](xref:signalr/introduction) 連線處理。

需要能夠裝載 ASP.NET Core 應用程式的網路伺服器。 Visual Studio 會包含 **Blazor (伺服器端)** 專案範本 (使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時為 `blazorserverside` 範本)。

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

## <a name="additional-resources"></a>其他資源

* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [將 ASP.NET Core 預覽版本部署至 Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
