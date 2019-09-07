---
title: 裝載及部署 ASP.NET Core Blazor 伺服器端
author: guardrex
description: 了解如何使用 ASP.NET Core 來裝載和部署 Blazor 伺服器端應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8da71faf6abc5929d6cd43d42fd896e378d99ef6
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773579"
---
# <a name="host-and-deploy-blazor-server-side"></a>裝載和部署 Blazor 伺服器端

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>主機組態值

使用[伺服器端裝載模型](xref:blazor/hosting-models#server-side)的伺服器端應用程式可以接受[一般主機組態值](xref:fundamentals/host/generic-host#host-configuration)。

## <a name="deployment"></a>部署

使用[伺服器端裝載模型](xref:blazor/hosting-models#server-side)，Blazor 會從 ASP.NET Core 應用程式內在伺服器上執行。 UI 更新、事件處理及 JavaScript 呼叫會透過 [SignalR](xref:signalr/introduction) 連線處理。

需要能夠裝載 ASP.NET Core 應用程式的網路伺服器。 Visual Studio 會包含 **Blazor 伺服器應用程式**專案範本 (使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時為 `blazorserverside` 範本)。

## <a name="connection-scale-out"></a>連線相應放大

針對每個使用者，Blazor 伺服器端應用程式需要一個有效的 SignalR 連線。 生產 Blazor 伺服器端部署需要一個解決方案以支援應用程式所需的同時連線數目。 [Azure SignalR Service](/azure/azure-signalr/) 會處理連線的規模調整，而且我們建議您使用它來作為 Blazor 伺服器端應用程式的規模調整解決方案。 如需詳細資訊，請參閱 <xref:signalr/publish-to-azure-web-app>。

## <a name="signalr-configuration"></a>SignalR 設定

SignalR 是由 ASP.NET Core 針對大部分的常見 Blazor 伺服器端案例設定的。 針對自訂與進階案例，請參閱[其他資源](#additional-resources) 區段中的 SignalR 文章。

## <a name="additional-resources"></a>其他資源

* <xref:signalr/introduction>
* [Azure SignalR Service 文件](/azure/azure-signalr/)
* [快速入門：使用 SignalR Service 建立聊天室](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [將 ASP.NET Core 預覽版本部署至 Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
