---
title: 在 Docker 容器中裝載 ASP.NET Core
author: rick-anderson
description: 探索資源連結以了解如何在 Docker 容器中裝載 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/docker/index
ms.openlocfilehash: 94996f279929f6e3837efb613c99927edbd9143e
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82774583"
---
# <a name="host-aspnet-core-in-docker-containers"></a><span data-ttu-id="23e78-103">在 Docker 容器中裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23e78-103">Host ASP.NET Core in Docker containers</span></span>

<span data-ttu-id="23e78-104">下列文章可用來了解如何在 Docker 中裝載 ASP.NET Core 應用程式：</span><span class="sxs-lookup"><span data-stu-id="23e78-104">The following articles are available for learning about hosting ASP.NET Core apps in Docker:</span></span>

[<span data-ttu-id="23e78-105">容器和 Docker 簡介</span><span class="sxs-lookup"><span data-stu-id="23e78-105">Introduction to Containers and Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
<span data-ttu-id="23e78-106">了解容器化是一種軟體開發方法，在此方法中，應用程式或服務、其相依性及其組態會封裝在一起，成為一個容器映像。</span><span class="sxs-lookup"><span data-stu-id="23e78-106">See how containerization is an approach to software development in which an application or service, its dependencies, and its configuration are packaged together as a container image.</span></span> <span data-ttu-id="23e78-107">映像可進行測試並部署到主機。</span><span class="sxs-lookup"><span data-stu-id="23e78-107">The image can be tested and then deployed to a host.</span></span>

[<span data-ttu-id="23e78-108">什麼是 Docker</span><span class="sxs-lookup"><span data-stu-id="23e78-108">What is Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
<span data-ttu-id="23e78-109">探索 Docker 開放原始碼專案，將應用程式自動化部署為可攜式且可自足的容器，在雲端或內部部署上執行。</span><span class="sxs-lookup"><span data-stu-id="23e78-109">Discover how Docker is an open-source project for automating the deployment of apps as portable, self-sufficient containers that can run on the cloud or on-premises.</span></span>

[<span data-ttu-id="23e78-110">Docker 術語</span><span class="sxs-lookup"><span data-stu-id="23e78-110">Docker Terminology</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
<span data-ttu-id="23e78-111">了解 Docker 技術的詞彙和定義。</span><span class="sxs-lookup"><span data-stu-id="23e78-111">Learn terms and definitions for Docker technology.</span></span>

[<span data-ttu-id="23e78-112">Docker 容器、映像和登錄</span><span class="sxs-lookup"><span data-stu-id="23e78-112">Docker containers, images, and registries</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
<span data-ttu-id="23e78-113">了解 Docker 容器映像儲存在映像登錄的方式，取得跨環境部署的一致性。</span><span class="sxs-lookup"><span data-stu-id="23e78-113">Find out how Docker container images are stored in an image registry for consistent deployment across environments.</span></span>

<span data-ttu-id="23e78-114"><xref:host-and-deploy/docker/building-net-docker-images> 了解如何建置和 Docker 化 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="23e78-114"><xref:host-and-deploy/docker/building-net-docker-images> Learn how to build and dockerize an ASP.NET Core app.</span></span> <span data-ttu-id="23e78-115">瀏覽 Microsoft 維護的 Docker 映像，並檢查使用案例。</span><span class="sxs-lookup"><span data-stu-id="23e78-115">Explore Docker images maintained by Microsoft and examine use cases.</span></span>

[<span data-ttu-id="23e78-116">Visual Studio 容器工具</span><span class="sxs-lookup"><span data-stu-id="23e78-116">Visual Studio Container Tools</span></span>](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
<span data-ttu-id="23e78-117">探索 Visual Studio 如何在 Docker for Windows 上支援建置、偵錯和執行以 .NET Framework 或 .NET Core 為目標的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="23e78-117">Discover how Visual Studio supports building, debugging, and running ASP.NET Core apps targeting either .NET Framework or .NET Core on Docker for Windows.</span></span> <span data-ttu-id="23e78-118">同時支援 Windows 和 Linux 容器。</span><span class="sxs-lookup"><span data-stu-id="23e78-118">Both Windows and Linux containers are supported.</span></span>

[<span data-ttu-id="23e78-119">發佈至 Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="23e78-119">Publish to Azure Container Registry</span></span>](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
<span data-ttu-id="23e78-120">了解如何使用 Visual Studio Container Tools 延伸模組，透過PowerShell 將 ASP.NET Core 應用程式部署到 Azure 上的 Docker 主機。</span><span class="sxs-lookup"><span data-stu-id="23e78-120">Find out how to use the Visual Studio Container Tools extension to deploy an ASP.NET Core app to a Docker host on Azure using PowerShell.</span></span>

[<span data-ttu-id="23e78-121">設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器</span><span class="sxs-lookup"><span data-stu-id="23e78-121">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)  
<span data-ttu-id="23e78-122">Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="23e78-122">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="23e78-123">透過 Proxy 傳遞要求通常會遮住原始要求的相關資訊，例如配置和用戶端 IP。</span><span class="sxs-lookup"><span data-stu-id="23e78-123">Passing requests through a proxy often obscures information about the original request, such as the scheme and client IP.</span></span> <span data-ttu-id="23e78-124">可能必須以手動方式將一些關於要求的資訊轉送至應用程式。</span><span class="sxs-lookup"><span data-stu-id="23e78-124">It might be necessary to forwarded some information about the request manually to the app.</span></span>

<span data-ttu-id="23e78-125">[使用 Docker 和小型容器的 GC](xref:performance/memory#sc)討論具有小型容器的 GC 選取範圍。</span><span class="sxs-lookup"><span data-stu-id="23e78-125">[GC using Docker and small containers](xref:performance/memory#sc) Discusses GC selection with small containers.</span></span>