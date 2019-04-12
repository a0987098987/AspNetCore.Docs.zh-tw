---
title: 裝載模型的 razor 元件
author: guardrex
description: 了解用戶端 Blazor 和裝載模型的伺服器端 ASP.NET Core Razor 元件。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: razor-components/hosting-models
ms.openlocfilehash: 8ed70117c94bf1a3e4c208f70310bbf0473bae44
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515414"
---
# <a name="razor-components-hosting-models"></a><span data-ttu-id="93350-103">裝載模型的 razor 元件</span><span class="sxs-lookup"><span data-stu-id="93350-103">Razor Components hosting models</span></span>

<span data-ttu-id="93350-104">藉由[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="93350-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="93350-105">Razor 元件是設計用來執行用戶端的 web 架構中的瀏覽器[WebAssembly](http://webassembly.org/)-根據.NET 執行階段 (*Blazor*) 或 ASP.NET Core 中的伺服器端 (*ASP.NET Core Razor元件*)。</span><span class="sxs-lookup"><span data-stu-id="93350-105">Razor Components is a web framework designed to run client-side in the browser on a [WebAssembly](http://webassembly.org/)-based .NET runtime (*Blazor*) or server-side in ASP.NET Core (*ASP.NET Core Razor Components*).</span></span> <span data-ttu-id="93350-106">無論裝載模型、 應用程式和元件模型*維持不變*。</span><span class="sxs-lookup"><span data-stu-id="93350-106">Regardless of the hosting model, the app and component models *remain the same*.</span></span> <span data-ttu-id="93350-107">這篇文章會討論可用的裝載模型：</span><span class="sxs-lookup"><span data-stu-id="93350-107">This article discusses the available hosting models:</span></span>

* [<span data-ttu-id="93350-108">用戶端 Blazor</span><span class="sxs-lookup"><span data-stu-id="93350-108">Client-side Blazor</span></span>](#client-side-hosting-model)
* [<span data-ttu-id="93350-109">伺服器端 ASP.NET Core Razor 元件</span><span class="sxs-lookup"><span data-stu-id="93350-109">Server-side ASP.NET Core Razor Components</span></span>](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a><span data-ttu-id="93350-110">用戶端裝載模型</span><span class="sxs-lookup"><span data-stu-id="93350-110">Client-side hosting model</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="93350-111">Blazor 的主要裝載模型是 WebAssembly 上的瀏覽器中的執行用戶端。</span><span class="sxs-lookup"><span data-stu-id="93350-111">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="93350-112">Blazor 應用程式、其相依性，和 .NET 執行階段會下載至瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="93350-112">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="93350-113">應用程式會直接在瀏覽器 UI 執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="93350-113">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="93350-114">UI 更新及事件處理會發生相同的程序中。</span><span class="sxs-lookup"><span data-stu-id="93350-114">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="93350-115">應用程式的資產會部署為 web 伺服器或服務能夠提供靜態內容給用戶端的靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="93350-115">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor 用戶端：Blazor 應用程式會在瀏覽器 UI 執行緒上執行。](hosting-models/_static/client-side.png)

<span data-ttu-id="93350-117">若要建立一個使用的用戶端的裝載模型的 Blazor 應用程式，請使用下列範本：</span><span class="sxs-lookup"><span data-stu-id="93350-117">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="93350-118">**Blazor** ([dotnet 新 blazor](/dotnet/core/tools/dotnet-new))&ndash;部署為一組靜態的檔案。</span><span class="sxs-lookup"><span data-stu-id="93350-118">**Blazor** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="93350-119">**（ASP.NET Core 裝載） 的 Blazor** ([dotnet 新 blazorhosted](/dotnet/core/tools/dotnet-new))&ndash;裝載 ASP.NET Core 伺服器。</span><span class="sxs-lookup"><span data-stu-id="93350-119">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="93350-120">ASP.NET Core 應用程式提供給用戶端的 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="93350-120">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="93350-121">與伺服器互動的用戶端 Blazor 應用程式，可以將它透過網路使用的 web API 呼叫或[SignalR](xref:signalr/introduction)。</span><span class="sxs-lookup"><span data-stu-id="93350-121">The client-side Blazor app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="93350-122">範本包含*components.webassembly.js*處理的指令碼：</span><span class="sxs-lookup"><span data-stu-id="93350-122">The templates include the *components.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="93350-123">下載.NET 執行階段、 應用程式和應用程式的相依性。</span><span class="sxs-lookup"><span data-stu-id="93350-123">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="93350-124">要執行應用程式的執行階段初始化。</span><span class="sxs-lookup"><span data-stu-id="93350-124">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="93350-125">用戶端的裝載模型提供幾項優點。</span><span class="sxs-lookup"><span data-stu-id="93350-125">The client-side hosting model offers several benefits.</span></span> <span data-ttu-id="93350-126">用戶端 Blazor:</span><span class="sxs-lookup"><span data-stu-id="93350-126">Client-side Blazor:</span></span>

* <span data-ttu-id="93350-127">有任何的.NET 伺服器端相依性。</span><span class="sxs-lookup"><span data-stu-id="93350-127">Has no .NET server-side dependency.</span></span>
* <span data-ttu-id="93350-128">充分利用用戶端資源和功能。</span><span class="sxs-lookup"><span data-stu-id="93350-128">Fully leverages client resources and capabilities.</span></span>
* <span data-ttu-id="93350-129">卸載工作從伺服器到用戶端。</span><span class="sxs-lookup"><span data-stu-id="93350-129">Offloads work from the server to the client.</span></span>
* <span data-ttu-id="93350-130">支援離線案例。</span><span class="sxs-lookup"><span data-stu-id="93350-130">Supports offline scenarios.</span></span>

<span data-ttu-id="93350-131">有裝載用戶端的缺點。</span><span class="sxs-lookup"><span data-stu-id="93350-131">There are downsides to client-side hosting.</span></span> <span data-ttu-id="93350-132">用戶端 Blazor:</span><span class="sxs-lookup"><span data-stu-id="93350-132">Client-side Blazor:</span></span>

* <span data-ttu-id="93350-133">將應用程式限制的瀏覽器的功能。</span><span class="sxs-lookup"><span data-stu-id="93350-133">Restricts the app to the capabilities of the browser.</span></span>
* <span data-ttu-id="93350-134">需要支援用戶端硬體和軟體 （例如 WebAssembly 支援）。</span><span class="sxs-lookup"><span data-stu-id="93350-134">Requires capable client hardware and software (for example, WebAssembly support).</span></span>
* <span data-ttu-id="93350-135">具有較大的下載大小和較長載入時間的應用程式。</span><span class="sxs-lookup"><span data-stu-id="93350-135">Has a larger download size and longer app load time.</span></span>
* <span data-ttu-id="93350-136">較少成熟的.NET 執行階段和工具支援 (例如，限制[.NET Standard](/dotnet/standard/net-standard)支援和偵錯)。</span><span class="sxs-lookup"><span data-stu-id="93350-136">Has less mature .NET runtime and tooling support (for example, limitations in [.NET Standard](/dotnet/standard/net-standard) support and debugging).</span></span>

## <a name="server-side-hosting-model"></a><span data-ttu-id="93350-137">伺服器端裝載模型</span><span class="sxs-lookup"><span data-stu-id="93350-137">Server-side hosting model</span></span>

<span data-ttu-id="93350-138">使用 ASP.NET Core Razor 元件伺服器端裝載模型，從 ASP.NET Core 應用程式中的伺服器上執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="93350-138">With the ASP.NET Core Razor Components server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="93350-139">UI 更新、 事件處理及 JavaScript 呼叫透過處理[SignalR](xref:signalr/introduction)連接。</span><span class="sxs-lookup"><span data-stu-id="93350-139">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![ASP.NET Core Razor 元件伺服器端：瀏覽器互動 （裝載 ASP.NET Core 應用程式內） 的應用程式伺服器上透過 SignalR 的連線。](hosting-models/_static/server-side.png)

<span data-ttu-id="93350-141">若要建立一個使用的伺服器端的主控模型的 Razor 元件應用程式，請使用 ASP.NET Core **Razor 元件**範本 ([dotnet 新 razorcomponents](/dotnet/core/tools/dotnet-new))。</span><span class="sxs-lookup"><span data-stu-id="93350-141">To create a Razor Components app using the server-side hosting model, use the ASP.NET Core **Razor Components** template ([dotnet new razorcomponents](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="93350-142">ASP.NET Core 應用程式裝載 Razor 元件伺服器端應用程式，並設定 SignalR 端點的用戶端連線的位置。</span><span class="sxs-lookup"><span data-stu-id="93350-142">The ASP.NET Core app hosts the Razor Components server-side app and sets up the SignalR endpoint where clients connect.</span></span> <span data-ttu-id="93350-143">ASP.NET Core 應用程式參考應用程式的`Startup`来加入類別：</span><span class="sxs-lookup"><span data-stu-id="93350-143">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="93350-144">伺服器端 Razor 元件服務。</span><span class="sxs-lookup"><span data-stu-id="93350-144">Server-side Razor Components services.</span></span>
* <span data-ttu-id="93350-145">應用程式，以在要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="93350-145">The app to the request handling pipeline.</span></span>

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

<span data-ttu-id="93350-146">*Components.server.js*指令碼&dagger;會建立用戶端連線。</span><span class="sxs-lookup"><span data-stu-id="93350-146">The *components.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="93350-147">它是保存和還原應用程式狀態為 必要 （例如，如果遺失的網路連線） 的應用程式的責任。</span><span class="sxs-lookup"><span data-stu-id="93350-147">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="93350-148">伺服器端的主控模型會提供數個優點。</span><span class="sxs-lookup"><span data-stu-id="93350-148">The server-side hosting model offers several benefits.</span></span> <span data-ttu-id="93350-149">伺服器端 Razor 的元件：</span><span class="sxs-lookup"><span data-stu-id="93350-149">Server-side Razor Components:</span></span>

* <span data-ttu-id="93350-150">有非常小的應用程式大小比用戶端 Blazor 應用程式，而且速度更快載入。</span><span class="sxs-lookup"><span data-stu-id="93350-150">Have a significantly smaller app size than a client-side Blazor app and load much faster.</span></span>
* <span data-ttu-id="93350-151">可以充分利用 server 功能，包括使用任何.NET Core 相容的 Api。</span><span class="sxs-lookup"><span data-stu-id="93350-151">Can take full advantage of server capabilities, including using any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="93350-152">在.NET Core 上在伺服器上執行，因此現有的.NET 工具，例如偵錯時，如預期運作。</span><span class="sxs-lookup"><span data-stu-id="93350-152">Run on .NET Core on the server, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="93350-153">使用精簡型用戶端 （例如，不支援 WebAssembly 和資源的瀏覽器受限的裝置）。</span><span class="sxs-lookup"><span data-stu-id="93350-153">Works with thin clients (for example, browsers that don't support WebAssembly and resource constrained devices).</span></span>
* <span data-ttu-id="93350-154">.NET /C#程式碼基底，包括應用程式的元件程式碼，不提供給用戶端。</span><span class="sxs-lookup"><span data-stu-id="93350-154">.NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="93350-155">有伺服器端裝載的缺點。</span><span class="sxs-lookup"><span data-stu-id="93350-155">There are downsides to server-side hosting.</span></span> <span data-ttu-id="93350-156">伺服器端 Razor 的元件：</span><span class="sxs-lookup"><span data-stu-id="93350-156">Server-side Razor Components:</span></span>

* <span data-ttu-id="93350-157">具有較高的延遲：每個使用者互動牽涉到的網路躍點。</span><span class="sxs-lookup"><span data-stu-id="93350-157">Have higher latency: Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="93350-158">提供任何離線的支援：如果用戶端連線失敗，應用程式會停止運作。</span><span class="sxs-lookup"><span data-stu-id="93350-158">Offer no offline support: If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="93350-159">較低的延展性：伺服器必須管理多個用戶端連線，並處理用戶端狀態。</span><span class="sxs-lookup"><span data-stu-id="93350-159">Have reduced scalability: The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="93350-160">需要 ASP.NET Core 伺服器來提供應用程式。</span><span class="sxs-lookup"><span data-stu-id="93350-160">Require an ASP.NET Core server to serve the app.</span></span> <span data-ttu-id="93350-161">不使用伺服器 （例如，從 CDN) 的部署不可行。</span><span class="sxs-lookup"><span data-stu-id="93350-161">Deployment without a server (for example, from a CDN) isn't possible.</span></span>

<span data-ttu-id="93350-162">&dagger;*Components.server.js*指令碼發行至下列路徑： *bin / {偵錯 |發行} / {目標 FRAMEWORK} /publish/ {應用程式名稱}。應用程式/dist/架構 （_f)*。</span><span class="sxs-lookup"><span data-stu-id="93350-162">&dagger;The *components.server.js* script is published to the following path: *bin/{Debug|Release}/{TARGET FRAMEWORK}/publish/{APPLICATION NAME}.App/dist/_framework*.</span></span>
