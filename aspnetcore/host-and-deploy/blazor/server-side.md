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
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="1dfd3-103">裝載和部署 Blazor 伺服器端</span><span class="sxs-lookup"><span data-stu-id="1dfd3-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="1dfd3-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="1dfd3-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="1dfd3-105">主機組態值</span><span class="sxs-lookup"><span data-stu-id="1dfd3-105">Host configuration values</span></span>

<span data-ttu-id="1dfd3-106">使用[伺服器端裝載模型](xref:blazor/hosting-models#server-side-hosting-model)的伺服器端應用程式可以接受[一般主機組態值](xref:fundamentals/host/generic-host#host-configuration)。</span><span class="sxs-lookup"><span data-stu-id="1dfd3-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side-hosting-model) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="1dfd3-107">部署</span><span class="sxs-lookup"><span data-stu-id="1dfd3-107">Deployment</span></span>

<span data-ttu-id="1dfd3-108">使用[伺服器端裝載模型](xref:blazor/hosting-models#server-side-hosting-model)，Blazor 會從 ASP.NET Core 應用程式內在伺服器上執行。</span><span class="sxs-lookup"><span data-stu-id="1dfd3-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side-hosting-model), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="1dfd3-109">UI 更新、事件處理及 JavaScript 呼叫會透過 [SignalR](xref:signalr/introduction) 連線處理。</span><span class="sxs-lookup"><span data-stu-id="1dfd3-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="1dfd3-110">應用程式隨附於發佈輸出中的 ASP.NET Core 應用程式，且兩個應用程式會一起部署。</span><span class="sxs-lookup"><span data-stu-id="1dfd3-110">The app is included with the ASP.NET Core app in the published output, and the two apps are deployed together.</span></span> <span data-ttu-id="1dfd3-111">需要有能夠裝載 ASP.NET Core 應用程式的網路伺服器。</span><span class="sxs-lookup"><span data-stu-id="1dfd3-111">A web server that's capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="1dfd3-112">對於伺服器端部署，Visual Studio 會包含 **Razor Components**  專案範本 (在使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時為 `razorcomponents` 範本)。</span><span class="sxs-lookup"><span data-stu-id="1dfd3-112">For a server-side deployment, Visual Studio includes the **Razor Components** project template (`razorcomponents` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="1dfd3-113">如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。</span><span class="sxs-lookup"><span data-stu-id="1dfd3-113">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="1dfd3-114">如需部署至 Azure App Service 的相關資訊，請參閱 <xref:tutorials/publish-to-azure-webapp-using-vs>。</span><span class="sxs-lookup"><span data-stu-id="1dfd3-114">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>
