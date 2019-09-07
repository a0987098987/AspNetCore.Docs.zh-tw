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
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="d48f2-103">裝載和部署 Blazor 伺服器端</span><span class="sxs-lookup"><span data-stu-id="d48f2-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="d48f2-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="d48f2-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="d48f2-105">主機組態值</span><span class="sxs-lookup"><span data-stu-id="d48f2-105">Host configuration values</span></span>

<span data-ttu-id="d48f2-106">使用[伺服器端裝載模型](xref:blazor/hosting-models#server-side)的伺服器端應用程式可以接受[一般主機組態值](xref:fundamentals/host/generic-host#host-configuration)。</span><span class="sxs-lookup"><span data-stu-id="d48f2-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="d48f2-107">部署</span><span class="sxs-lookup"><span data-stu-id="d48f2-107">Deployment</span></span>

<span data-ttu-id="d48f2-108">使用[伺服器端裝載模型](xref:blazor/hosting-models#server-side)，Blazor 會從 ASP.NET Core 應用程式內在伺服器上執行。</span><span class="sxs-lookup"><span data-stu-id="d48f2-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="d48f2-109">UI 更新、事件處理及 JavaScript 呼叫會透過 [SignalR](xref:signalr/introduction) 連線處理。</span><span class="sxs-lookup"><span data-stu-id="d48f2-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="d48f2-110">需要能夠裝載 ASP.NET Core 應用程式的網路伺服器。</span><span class="sxs-lookup"><span data-stu-id="d48f2-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="d48f2-111">Visual Studio 會包含 **Blazor 伺服器應用程式**專案範本 (使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時為 `blazorserverside` 範本)。</span><span class="sxs-lookup"><span data-stu-id="d48f2-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="connection-scale-out"></a><span data-ttu-id="d48f2-112">連線相應放大</span><span class="sxs-lookup"><span data-stu-id="d48f2-112">Connection scale out</span></span>

<span data-ttu-id="d48f2-113">針對每個使用者，Blazor 伺服器端應用程式需要一個有效的 SignalR 連線。</span><span class="sxs-lookup"><span data-stu-id="d48f2-113">Blazor server-side apps require one active SignalR connection for each user.</span></span> <span data-ttu-id="d48f2-114">生產 Blazor 伺服器端部署需要一個解決方案以支援應用程式所需的同時連線數目。</span><span class="sxs-lookup"><span data-stu-id="d48f2-114">A production Blazor server-side deployment requires a solution for supporting as many concurrent connections as required by the app.</span></span> <span data-ttu-id="d48f2-115">[Azure SignalR Service](/azure/azure-signalr/) 會處理連線的規模調整，而且我們建議您使用它來作為 Blazor 伺服器端應用程式的規模調整解決方案。</span><span class="sxs-lookup"><span data-stu-id="d48f2-115">The [Azure SignalR Service](/azure/azure-signalr/) handles the scaling of connections and is recommended as a scaling solution for Blazor server-side apps.</span></span> <span data-ttu-id="d48f2-116">如需詳細資訊，請參閱 <xref:signalr/publish-to-azure-web-app>。</span><span class="sxs-lookup"><span data-stu-id="d48f2-116">For more information, see <xref:signalr/publish-to-azure-web-app>.</span></span>

## <a name="signalr-configuration"></a><span data-ttu-id="d48f2-117">SignalR 設定</span><span class="sxs-lookup"><span data-stu-id="d48f2-117">SignalR configuration</span></span>

<span data-ttu-id="d48f2-118">SignalR 是由 ASP.NET Core 針對大部分的常見 Blazor 伺服器端案例設定的。</span><span class="sxs-lookup"><span data-stu-id="d48f2-118">SignalR is configured by ASP.NET Core for the most common Blazor server-side scenarios.</span></span> <span data-ttu-id="d48f2-119">針對自訂與進階案例，請參閱[其他資源](#additional-resources) 區段中的 SignalR 文章。</span><span class="sxs-lookup"><span data-stu-id="d48f2-119">For custom and advanced scenarios, consult the SignalR articles in the [Additional resources](#additional-resources) section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d48f2-120">其他資源</span><span class="sxs-lookup"><span data-stu-id="d48f2-120">Additional resources</span></span>

* <xref:signalr/introduction>
* [<span data-ttu-id="d48f2-121">Azure SignalR Service 文件</span><span class="sxs-lookup"><span data-stu-id="d48f2-121">Azure SignalR Service Documentation</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="d48f2-122">快速入門：使用 SignalR Service 建立聊天室</span><span class="sxs-lookup"><span data-stu-id="d48f2-122">Quickstart: Create a chat room by using SignalR Service</span></span>](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="d48f2-123">將 ASP.NET Core 預覽版本部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d48f2-123">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
