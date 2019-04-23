---
title: 裝載和部署 Blazor 伺服器端
author: guardrex
description: 了解如何使用 ASP.NET Core 來裝載和部署 Blazor 伺服器端應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 940020ee44d72d50395aad64bc924413c1bbecfb
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614660"
---
# <a name="host-and-deploy-blazor-server-side"></a>裝載和部署 Blazor 伺服器端

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>主機組態值

使用[伺服器端裝載模型](xref:blazor/hosting-models#server-side-hosting-model)的伺服器端應用程式可以接受[一般主機組態值](xref:fundamentals/host/generic-host#host-configuration)。

## <a name="deployment"></a>部署

使用[伺服器端裝載模型](xref:blazor/hosting-models#server-side-hosting-model)，Blazor 會從 ASP.NET Core 應用程式內在伺服器上執行。 UI 更新、事件處理及 JavaScript 呼叫會透過 [SignalR](xref:signalr/introduction) 連線處理。

應用程式隨附於發佈輸出中的 ASP.NET Core 應用程式，且兩個應用程式會一起部署。 需要有能夠裝載 ASP.NET Core 應用程式的網路伺服器。 對於伺服器端部署，Visual Studio 會包含 **Razor Components**  專案範本 (在使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時為 `razorcomponents` 範本)。

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。

如需部署至 Azure App Service 的相關資訊，請參閱 <xref:tutorials/publish-to-azure-webapp-using-vs>。
