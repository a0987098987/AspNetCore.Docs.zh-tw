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
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="1a32f-103">裝載和部署 Blazor 伺服器端</span><span class="sxs-lookup"><span data-stu-id="1a32f-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="1a32f-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="1a32f-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="1a32f-105">主機組態值</span><span class="sxs-lookup"><span data-stu-id="1a32f-105">Host configuration values</span></span>

<span data-ttu-id="1a32f-106">使用[伺服器端裝載模型](xref:blazor/hosting-models#server-side)的伺服器端應用程式可以接受[一般主機組態值](xref:fundamentals/host/generic-host#host-configuration)。</span><span class="sxs-lookup"><span data-stu-id="1a32f-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="1a32f-107">部署</span><span class="sxs-lookup"><span data-stu-id="1a32f-107">Deployment</span></span>

<span data-ttu-id="1a32f-108">使用[伺服器端裝載模型](xref:blazor/hosting-models#server-side)，Blazor 會從 ASP.NET Core 應用程式內在伺服器上執行。</span><span class="sxs-lookup"><span data-stu-id="1a32f-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="1a32f-109">UI 更新、事件處理及 JavaScript 呼叫會透過 [SignalR](xref:signalr/introduction) 連線處理。</span><span class="sxs-lookup"><span data-stu-id="1a32f-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="1a32f-110">需要能夠裝載 ASP.NET Core 應用程式的網路伺服器。</span><span class="sxs-lookup"><span data-stu-id="1a32f-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="1a32f-111">Visual Studio 會包含 **Blazor (伺服器端)** 專案範本 (使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時為 `blazorserverside` 範本)。</span><span class="sxs-lookup"><span data-stu-id="1a32f-111">Visual Studio includes the **Blazor (server-side)** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

## <a name="additional-resources"></a><span data-ttu-id="1a32f-112">其他資源</span><span class="sxs-lookup"><span data-stu-id="1a32f-112">Additional resources</span></span>

* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="1a32f-113">將 ASP.NET Core 預覽版本部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1a32f-113">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
