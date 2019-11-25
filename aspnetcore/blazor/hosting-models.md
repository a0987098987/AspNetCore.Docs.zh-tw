---
title: ASP.NET Core Blazor 裝載模型
author: guardrex
description: 瞭解 Blazor WebAssembly 和 Blazor 伺服器裝載模型。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: a017737eacd93ac776afe7ee8024eed602d7edcc
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317219"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a><span data-ttu-id="83e5c-103">ASP.NET Core Blazor 裝載模型</span><span class="sxs-lookup"><span data-stu-id="83e5c-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="83e5c-104">依[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="83e5c-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="83e5c-105"> 是一種 web 架構，設計用來在瀏覽器中以[WebAssembly](https://webassembly.org/)為基礎的 .net 執行時間（ *Blazor WebAssembly*）或伺服器端在 ASP.NET Core （ *Blazor server*）中執行用戶端。</span><span class="sxs-lookup"><span data-stu-id="83e5c-105"> is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor WebAssembly*) or server-side in ASP.NET Core (*Blazor Server*).</span></span> <span data-ttu-id="83e5c-106">無論裝載模型為何，應用程式和元件模型*都相同*。</span><span class="sxs-lookup"><span data-stu-id="83e5c-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="83e5c-107">若要為本文所述的主控模型建立專案，請參閱 <xref:blazor/get-started>。</span><span class="sxs-lookup"><span data-stu-id="83e5c-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="83e5c-108"> WebAssembly</span><span class="sxs-lookup"><span data-stu-id="83e5c-108"> WebAssembly</span></span>

<span data-ttu-id="83e5c-109">Blazor 的主要裝載模型會在 WebAssembly 的瀏覽器中執行用戶端。</span><span class="sxs-lookup"><span data-stu-id="83e5c-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="83e5c-110">Blazor 應用程式、其相依性和 .NET 執行時間會下載至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="83e5c-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="83e5c-111">應用程式會直接在瀏覽器 UI 執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="83e5c-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="83e5c-112">UI 更新和事件處理會在同一個進程中進行。</span><span class="sxs-lookup"><span data-stu-id="83e5c-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="83e5c-113">應用程式的資產會以靜態檔案的形式部署至 web 伺服器或服務，以提供靜態內容給用戶端。</span><span class="sxs-lookup"><span data-stu-id="83e5c-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![[!OP.無-LOC （Blazor）]<span data-ttu-id="83e5c-114"> WebAssembly： [！OP.無 LOC （Blazor）] 應用程式會在瀏覽器內的 UI 執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="83e5c-114"> WebAssembly: The Blazor app runs on a UI thread inside the browser.</span></span>](hosting-models/_static/blazor-webassembly.png)

<span data-ttu-id="83e5c-115">若要使用用戶端裝載模型來建立 Blazor 應用程式，請使用 **Blazor WebAssembly 應用程式**範本（[dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)）。</span><span class="sxs-lookup"><span data-stu-id="83e5c-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="83e5c-116">選取 [ **Blazor WebAssembly 應用程式**] 範本之後，您可以選擇將應用程式設定為使用 ASP.NET Core 後端，方法是選取 [裝載的**ASP.NET Core** ] 核取方塊（[dotnet] [新增] [blazorwasm] [[託管](/dotnet/core/tools/dotnet-new)]）。</span><span class="sxs-lookup"><span data-stu-id="83e5c-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="83e5c-117">ASP.NET Core 應用程式會將 Blazor 應用程式提供給用戶端。</span><span class="sxs-lookup"><span data-stu-id="83e5c-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="83e5c-118">Blazor WebAssembly 應用程式可以使用 Web API 呼叫或[SignalR](xref:signalr/introduction)，透過網路與伺服器互動。</span><span class="sxs-lookup"><span data-stu-id="83e5c-118">The Blazor WebAssembly app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="83e5c-119">這些範本包含處理下列內容的*blazor. webassembly*腳本：</span><span class="sxs-lookup"><span data-stu-id="83e5c-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="83e5c-120">下載 .NET 執行時間、應用程式和應用程式的相依性。</span><span class="sxs-lookup"><span data-stu-id="83e5c-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="83e5c-121">初始化執行時間以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="83e5c-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="83e5c-122">Blazor WebAssembly 裝載模型提供數個優點：</span><span class="sxs-lookup"><span data-stu-id="83e5c-122">The Blazor WebAssembly hosting model offers several benefits:</span></span>

* <span data-ttu-id="83e5c-123">沒有 .NET 伺服器端相依性。</span><span class="sxs-lookup"><span data-stu-id="83e5c-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="83e5c-124">應用程式會在下載至用戶端之後完全正常運作。</span><span class="sxs-lookup"><span data-stu-id="83e5c-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="83e5c-125">用戶端資源和功能都可以充分運用。</span><span class="sxs-lookup"><span data-stu-id="83e5c-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="83e5c-126">工作會從伺服器卸載至用戶端。</span><span class="sxs-lookup"><span data-stu-id="83e5c-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="83e5c-127">不需要 ASP.NET Core web 伺服器來裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="83e5c-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="83e5c-128">無伺服器部署案例可行（例如，從 CDN 提供應用程式）。</span><span class="sxs-lookup"><span data-stu-id="83e5c-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="83e5c-129">Blazor WebAssembly 裝載有缺點：</span><span class="sxs-lookup"><span data-stu-id="83e5c-129">There are downsides to Blazor WebAssembly hosting:</span></span>

* <span data-ttu-id="83e5c-130">應用程式僅限於瀏覽器的功能。</span><span class="sxs-lookup"><span data-stu-id="83e5c-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="83e5c-131">需要支援的用戶端硬體和軟體（例如 WebAssembly 支援）。</span><span class="sxs-lookup"><span data-stu-id="83e5c-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="83e5c-132">下載大小較大，且應用程式需要較長的時間來載入。</span><span class="sxs-lookup"><span data-stu-id="83e5c-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="83e5c-133">.NET 執行時間和工具支援較不成熟。</span><span class="sxs-lookup"><span data-stu-id="83e5c-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="83e5c-134">例如， [.NET Standard](/dotnet/standard/net-standard)支援和偵錯工具中有限制。</span><span class="sxs-lookup"><span data-stu-id="83e5c-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="opno-locblazor-server"></a>Blazor<span data-ttu-id="83e5c-135"> 伺服器</span><span class="sxs-lookup"><span data-stu-id="83e5c-135"> Server</span></span>

<span data-ttu-id="83e5c-136">在 Blazor 伺服器裝載模型中，應用程式會從 ASP.NET Core 應用程式內的伺服器上執行。</span><span class="sxs-lookup"><span data-stu-id="83e5c-136">With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="83e5c-137">UI 更新、事件處理及 JavaScript 呼叫會透過[SignalR](xref:signalr/introduction)連接來處理。</span><span class="sxs-lookup"><span data-stu-id="83e5c-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![瀏覽器會透過 [！，與伺服器上的應用程式互動（裝載于 ASP.NET Core 應用程式內）。OP.無-LOC （SignalR）] 連接。](hosting-models/_static/blazor-server.png)

<span data-ttu-id="83e5c-139">若要使用 Blazor 伺服器裝載模型來建立 Blazor 應用程式，請使用 ASP.NET Core **Blazor 伺服器應用程式**範本（[dotnet new blazorserver](/dotnet/core/tools/dotnet-new)）。</span><span class="sxs-lookup"><span data-stu-id="83e5c-139">To create a Blazor app using the Blazor Server hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="83e5c-140">ASP.NET Core 應用程式會裝載 Blazor 伺服器應用程式，並建立用戶端連接的 SignalR 端點。</span><span class="sxs-lookup"><span data-stu-id="83e5c-140">The ASP.NET Core app hosts the Blazor Server app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="83e5c-141">ASP.NET Core 應用程式會參考要新增的應用程式 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="83e5c-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="83e5c-142">伺服器端服務。</span><span class="sxs-lookup"><span data-stu-id="83e5c-142">Server-side services.</span></span>
* <span data-ttu-id="83e5c-143">應用程式至要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="83e5c-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="83e5c-144">*Blazor*腳本&dagger; 會建立用戶端連接。</span><span class="sxs-lookup"><span data-stu-id="83e5c-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="83e5c-145">應用程式會負責保存和還原所需的應用程式狀態（例如，萬一網路連線中斷時）。</span><span class="sxs-lookup"><span data-stu-id="83e5c-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="83e5c-146">Blazor 伺服器裝載模型提供數個優點：</span><span class="sxs-lookup"><span data-stu-id="83e5c-146">The Blazor Server hosting model offers several benefits:</span></span>

* <span data-ttu-id="83e5c-147">下載大小明顯小於 Blazor WebAssembly 應用程式，而應用程式載入速度會更快。</span><span class="sxs-lookup"><span data-stu-id="83e5c-147">Download size is significantly smaller than a Blazor WebAssembly app, and the app loads much faster.</span></span>
* <span data-ttu-id="83e5c-148">應用程式會充分利用伺服器功能，包括使用任何 .NET Core 相容的 Api。</span><span class="sxs-lookup"><span data-stu-id="83e5c-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="83e5c-149">伺服器上的 .NET Core 是用來執行應用程式，因此現有的 .NET 工具（例如，「偵測」）會如預期般運作。</span><span class="sxs-lookup"><span data-stu-id="83e5c-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="83e5c-150">支援瘦用戶端。</span><span class="sxs-lookup"><span data-stu-id="83e5c-150">Thin clients are supported.</span></span> <span data-ttu-id="83e5c-151">例如，Blazor 伺服器應用程式使用不支援 WebAssembly 的瀏覽器，以及在資源限制的裝置上。</span><span class="sxs-lookup"><span data-stu-id="83e5c-151">For example, Blazor Server apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="83e5c-152">應用程式的 .NET/C#程式碼基底（包括應用程式的元件代碼）不會提供給用戶端。</span><span class="sxs-lookup"><span data-stu-id="83e5c-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="83e5c-153">Blazor 伺服器裝載有缺點：</span><span class="sxs-lookup"><span data-stu-id="83e5c-153">There are downsides to Blazor Server hosting:</span></span>

* <span data-ttu-id="83e5c-154">通常會有較高的延遲。</span><span class="sxs-lookup"><span data-stu-id="83e5c-154">Higher latency usually exists.</span></span> <span data-ttu-id="83e5c-155">每個使用者互動都牽涉到網路躍點。</span><span class="sxs-lookup"><span data-stu-id="83e5c-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="83e5c-156">沒有離線支援。</span><span class="sxs-lookup"><span data-stu-id="83e5c-156">There's no offline support.</span></span> <span data-ttu-id="83e5c-157">如果用戶端連線失敗，應用程式就會停止運作。</span><span class="sxs-lookup"><span data-stu-id="83e5c-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="83e5c-158">對於具有許多使用者的應用程式而言，擴充性是極具挑戰性的。</span><span class="sxs-lookup"><span data-stu-id="83e5c-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="83e5c-159">伺服器必須管理多個用戶端連接並處理用戶端狀態。</span><span class="sxs-lookup"><span data-stu-id="83e5c-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="83e5c-160">需要 ASP.NET Core 伺服器才能服務應用程式。</span><span class="sxs-lookup"><span data-stu-id="83e5c-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="83e5c-161">無伺服器部署案例不可行（例如，從 CDN 提供應用程式）。</span><span class="sxs-lookup"><span data-stu-id="83e5c-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="83e5c-162">&dagger;從 ASP.NET Core 共用架構中的內嵌資源來提供*blazor*腳本。</span><span class="sxs-lookup"><span data-stu-id="83e5c-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="83e5c-163">與伺服器呈現的 UI 比較</span><span class="sxs-lookup"><span data-stu-id="83e5c-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="83e5c-164">瞭解 Blazor Server 應用程式的其中一種方式，就是了解它與傳統模型有何不同，可以使用 Razor views 或 Razor Pages，在 ASP.NET Core 應用程式中呈現 UI。</span><span class="sxs-lookup"><span data-stu-id="83e5c-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="83e5c-165">這兩種模型都使用 Razor 語言來描述 HTML 內容，但在標記呈現的方式上有很大的差異。</span><span class="sxs-lookup"><span data-stu-id="83e5c-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="83e5c-166">當 Razor 頁面或視圖呈現時，每一行 Razor 程式碼都會以文字格式發出 HTML。</span><span class="sxs-lookup"><span data-stu-id="83e5c-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="83e5c-167">呈現之後，伺服器會處置頁面或 view 實例，包括任何已產生的狀態。</span><span class="sxs-lookup"><span data-stu-id="83e5c-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="83e5c-168">當頁面的另一個要求發生時（例如，當伺服器驗證失敗且顯示驗證摘要時）：</span><span class="sxs-lookup"><span data-stu-id="83e5c-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="83e5c-169">整個頁面會再次重新顯示為 HTML 文字。</span><span class="sxs-lookup"><span data-stu-id="83e5c-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="83e5c-170">此頁面會傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="83e5c-170">The page is sent to the client.</span></span>

<span data-ttu-id="83e5c-171">Blazor 應用程式是由可重複使用的 UI 元素（稱為「*元件*」）所組成。</span><span class="sxs-lookup"><span data-stu-id="83e5c-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="83e5c-172">元件包含C#程式碼、標記和其他元件。</span><span class="sxs-lookup"><span data-stu-id="83e5c-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="83e5c-173">當元件呈現時，Blazor 會產生類似于 HTML 或 XML 檔物件模型（DOM）的內含元件圖形。</span><span class="sxs-lookup"><span data-stu-id="83e5c-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="83e5c-174">此圖表包含屬性和欄位中保留的元件狀態。</span><span class="sxs-lookup"><span data-stu-id="83e5c-174">This graph includes component state held in properties and fields.</span></span> Blazor<span data-ttu-id="83e5c-175"> 會評估元件圖形，以產生標記的二進位標記法。</span><span class="sxs-lookup"><span data-stu-id="83e5c-175"> evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="83e5c-176">二進位格式可以是：</span><span class="sxs-lookup"><span data-stu-id="83e5c-176">The binary format can be:</span></span>

* <span data-ttu-id="83e5c-177">已轉換成 HTML 文字（在預建期間）。</span><span class="sxs-lookup"><span data-stu-id="83e5c-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="83e5c-178">用來在一般轉譯期間有效率地更新標記。</span><span class="sxs-lookup"><span data-stu-id="83e5c-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="83e5c-179">會觸發 Blazor 中的 UI 更新：</span><span class="sxs-lookup"><span data-stu-id="83e5c-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="83e5c-180">使用者互動，例如選取按鈕。</span><span class="sxs-lookup"><span data-stu-id="83e5c-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="83e5c-181">應用程式觸發程式，例如計時器。</span><span class="sxs-lookup"><span data-stu-id="83e5c-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="83e5c-182">圖形為重新顯示，且會計算 UI*差異*（差異）。</span><span class="sxs-lookup"><span data-stu-id="83e5c-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="83e5c-183">這項差異是更新用戶端上的 UI 所需的最小一組 DOM 編輯。</span><span class="sxs-lookup"><span data-stu-id="83e5c-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="83e5c-184">差異會以二進位格式傳送至用戶端，並由瀏覽器套用。</span><span class="sxs-lookup"><span data-stu-id="83e5c-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="83e5c-185">元件會在使用者從用戶端導覽出去之後處置。</span><span class="sxs-lookup"><span data-stu-id="83e5c-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="83e5c-186">當使用者與元件互動時，元件的狀態（服務、資源）必須保留在伺服器的記憶體中。</span><span class="sxs-lookup"><span data-stu-id="83e5c-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="83e5c-187">因為許多元件的狀態可能會由伺服器同時維護，所以記憶體耗盡是必須解決的問題。</span><span class="sxs-lookup"><span data-stu-id="83e5c-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="83e5c-188">如需如何撰寫 Blazor Server 應用程式以確保最佳使用伺服器記憶體的指引，請參閱 <xref:security/blazor/server>。</span><span class="sxs-lookup"><span data-stu-id="83e5c-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server>.</span></span>

### <a name="circuits"></a><span data-ttu-id="83e5c-189">獲得</span><span class="sxs-lookup"><span data-stu-id="83e5c-189">Circuits</span></span>

<span data-ttu-id="83e5c-190">Blazor 伺服器應用程式建置於[ASP.NET Core SignalR](xref:signalr/introduction)上。</span><span class="sxs-lookup"><span data-stu-id="83e5c-190">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="83e5c-191">每個用戶端會透過一或多個稱為*線路*的 SignalR 連線來與伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="83e5c-191">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="83e5c-192">線路是 Blazor的抽象概念，而不是可容忍暫時網路中斷的 SignalR 連接。</span><span class="sxs-lookup"><span data-stu-id="83e5c-192">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="83e5c-193">當 Blazor 用戶端發現 SignalR 連線已中斷連線時，它會嘗試使用新的 SignalR 連接重新連接到伺服器。</span><span class="sxs-lookup"><span data-stu-id="83e5c-193">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="83e5c-194">連接到 Blazor 伺服器應用程式的每個瀏覽器畫面（瀏覽器索引標籤或 iframe）都會使用 SignalR 連接。</span><span class="sxs-lookup"><span data-stu-id="83e5c-194">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="83e5c-195">相較于一般伺服器呈現的應用程式，這還是另一項重要的差異。</span><span class="sxs-lookup"><span data-stu-id="83e5c-195">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="83e5c-196">在伺服器呈現的應用程式中，在多個瀏覽器畫面中開啟相同的應用程式，通常不會轉譯成伺服器上的其他資源需求。</span><span class="sxs-lookup"><span data-stu-id="83e5c-196">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="83e5c-197">在 Blazor 伺服器應用程式中，每個瀏覽器畫面都需要個別的線路，且元件狀態的個別實例會由伺服器管理。</span><span class="sxs-lookup"><span data-stu-id="83e5c-197">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

Blazor<span data-ttu-id="83e5c-198"> 考慮關閉瀏覽器索引標籤，或流覽至外部 URL 的*正常*終止。</span><span class="sxs-lookup"><span data-stu-id="83e5c-198"> considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="83e5c-199">在正常終止的事件中，會立即釋放線路和相關聯的資源。</span><span class="sxs-lookup"><span data-stu-id="83e5c-199">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="83e5c-200">用戶端也可能會因為網路中斷而無法正常地中斷連線。</span><span class="sxs-lookup"><span data-stu-id="83e5c-200">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> Blazor<span data-ttu-id="83e5c-201"> Server 會儲存已中斷連線的線路，以取得可設定的間隔，以允許用戶端重新連線。</span><span class="sxs-lookup"><span data-stu-id="83e5c-201"> Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="83e5c-202">如需詳細資訊，請參閱重新[連接到相同的伺服器](#reconnection-to-the-same-server)一節。</span><span class="sxs-lookup"><span data-stu-id="83e5c-202">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="83e5c-203">UI 延遲</span><span class="sxs-lookup"><span data-stu-id="83e5c-203">UI Latency</span></span>

<span data-ttu-id="83e5c-204">UI 延遲是從起始的動作到 UI 更新時間所花費的時間。</span><span class="sxs-lookup"><span data-stu-id="83e5c-204">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="83e5c-205">較小的 UI 延遲值是應用程式對使用者進行回應的必要行為。</span><span class="sxs-lookup"><span data-stu-id="83e5c-205">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="83e5c-206">在 Blazor 伺服器應用程式中，會將每個動作傳送至伺服器、進行處理，並傳回 UI 差異。</span><span class="sxs-lookup"><span data-stu-id="83e5c-206">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="83e5c-207">因此，UI 延遲是網路延遲和處理動作之伺服器延遲的總和。</span><span class="sxs-lookup"><span data-stu-id="83e5c-207">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="83e5c-208">對於僅限於私人商業網路的企業營運應用程式，通常會 imperceptible 因網路延遲而對使用者的延遲所造成的影響。</span><span class="sxs-lookup"><span data-stu-id="83e5c-208">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="83e5c-209">對於透過網際網路部署的應用程式，使用者的延遲可能會很明顯，尤其是在地理位置廣泛散佈的使用者時。</span><span class="sxs-lookup"><span data-stu-id="83e5c-209">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="83e5c-210">記憶體使用量也會導致應用程式延遲。</span><span class="sxs-lookup"><span data-stu-id="83e5c-210">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="83e5c-211">增加記憶體使用量會導致頻繁的垃圾收集或將記憶體分頁到磁片，這兩者都會降低應用程式效能，因而增加 UI 延遲。</span><span class="sxs-lookup"><span data-stu-id="83e5c-211">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="83e5c-212">如需詳細資訊，請參閱 <xref:security/blazor/server>。</span><span class="sxs-lookup"><span data-stu-id="83e5c-212">For more information, see <xref:security/blazor/server>.</span></span>

Blazor<span data-ttu-id="83e5c-213"> 伺服器應用程式應該經過優化，藉由減少網路延遲和記憶體使用量，將 UI 延遲降到最低。</span><span class="sxs-lookup"><span data-stu-id="83e5c-213"> Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="83e5c-214">如需測量網路延遲的方法，請參閱 <xref:host-and-deploy/blazor/server#measure-network-latency>。</span><span class="sxs-lookup"><span data-stu-id="83e5c-214">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server#measure-network-latency>.</span></span> <span data-ttu-id="83e5c-215">如需 SignalR 和 Blazor的詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="83e5c-215">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a><span data-ttu-id="83e5c-216">連接到伺服器</span><span class="sxs-lookup"><span data-stu-id="83e5c-216">Connection to the server</span></span>

Blazor<span data-ttu-id="83e5c-217"> 伺服器應用程式需要伺服器的作用中 SignalR 連接。</span><span class="sxs-lookup"><span data-stu-id="83e5c-217"> Server apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="83e5c-218">如果連接中斷，應用程式會嘗試重新連線到伺服器。</span><span class="sxs-lookup"><span data-stu-id="83e5c-218">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="83e5c-219">只要用戶端的狀態仍在記憶體中，用戶端會話就會繼續，而不會失去狀態。</span><span class="sxs-lookup"><span data-stu-id="83e5c-219">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

#### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="83e5c-220">重新連接到相同的伺服器</span><span class="sxs-lookup"><span data-stu-id="83e5c-220">Reconnection to the same server</span></span>

<span data-ttu-id="83e5c-221">Blazor 伺服器應用程式會 prerenders，以回應第一個用戶端要求，這會在伺服器上設定 UI 狀態。</span><span class="sxs-lookup"><span data-stu-id="83e5c-221">A Blazor Server app prerenders in response to the first client request, which sets up the UI state on the server.</span></span> <span data-ttu-id="83e5c-222">當用戶端嘗試建立 SignalR 連接時，用戶端必須重新連線到相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="83e5c-222">When the client attempts to create a SignalR connection, the client must reconnect to the same server.</span></span> <span data-ttu-id="83e5c-223">使用多個後端伺服器的 Blazor 伺服器應用程式應該為 SignalR 連線執行*粘滯會話*。</span><span class="sxs-lookup"><span data-stu-id="83e5c-223">Blazor Server apps that use more than one backend server should implement *sticky sessions* for SignalR connections.</span></span>

<span data-ttu-id="83e5c-224">我們建議使用適用于 Blazor 伺服器應用程式的[Azure SignalR 服務](/azure/azure-signalr)。</span><span class="sxs-lookup"><span data-stu-id="83e5c-224">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="83e5c-225">此服務可將 Blazor 伺服器應用程式相應增加為大量的並行 SignalR 連線。</span><span class="sxs-lookup"><span data-stu-id="83e5c-225">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="83e5c-226">您可以將服務的 `ServerStickyMode` 選項或設定值設為 [`Required`]，以針對 Azure SignalR 服務啟用 [粘滯會話]。</span><span class="sxs-lookup"><span data-stu-id="83e5c-226">Sticky sessions are enabled for the Azure SignalR Service by setting the service's `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="83e5c-227">如需詳細資訊，請參閱 <xref:host-and-deploy/blazor/server#signalr-configuration>。</span><span class="sxs-lookup"><span data-stu-id="83e5c-227">For more information, see <xref:host-and-deploy/blazor/server#signalr-configuration>.</span></span>

<span data-ttu-id="83e5c-228">使用 IIS 時，會使用應用程式要求路由來啟用「粘滯會話」。</span><span class="sxs-lookup"><span data-stu-id="83e5c-228">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="83e5c-229">如需詳細資訊，請參閱[使用應用程式要求路由的 HTTP 負載平衡](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)。</span><span class="sxs-lookup"><span data-stu-id="83e5c-229">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="83e5c-230">反映 UI 中的連接狀態</span><span class="sxs-lookup"><span data-stu-id="83e5c-230">Reflect the connection state in the UI</span></span>

<span data-ttu-id="83e5c-231">當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。</span><span class="sxs-lookup"><span data-stu-id="83e5c-231">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="83e5c-232">如果重新連線失敗，則會提供使用者重試的選項。</span><span class="sxs-lookup"><span data-stu-id="83e5c-232">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="83e5c-233">若要自訂 UI，請在 *_Host*的 [Razor 頁面] `<body>` 中，定義具有 `components-reconnect-modal` `id` 的元素：</span><span class="sxs-lookup"><span data-stu-id="83e5c-233">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```html
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="83e5c-234">下表描述套用至 `components-reconnect-modal` 元素的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="83e5c-234">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="83e5c-235">CSS 類別</span><span class="sxs-lookup"><span data-stu-id="83e5c-235">CSS class</span></span>                       | <span data-ttu-id="83e5c-236">表示&hellip;</span><span class="sxs-lookup"><span data-stu-id="83e5c-236">Indicates&hellip;</span></span> |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | <span data-ttu-id="83e5c-237">遺失的連接。</span><span class="sxs-lookup"><span data-stu-id="83e5c-237">A lost connection.</span></span> <span data-ttu-id="83e5c-238">用戶端正在嘗試重新連線。</span><span class="sxs-lookup"><span data-stu-id="83e5c-238">The client is attempting to reconnect.</span></span> <span data-ttu-id="83e5c-239">顯示強制回應。</span><span class="sxs-lookup"><span data-stu-id="83e5c-239">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="83e5c-240">系統會將使用中的連接重新建立至伺服器。</span><span class="sxs-lookup"><span data-stu-id="83e5c-240">An active connection is re-established to the server.</span></span> <span data-ttu-id="83e5c-241">隱藏強制回應。</span><span class="sxs-lookup"><span data-stu-id="83e5c-241">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="83e5c-242">重新連接失敗，可能是因為網路失敗。</span><span class="sxs-lookup"><span data-stu-id="83e5c-242">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="83e5c-243">若要嘗試重新連接，請呼叫 `window.Blazor.reconnect()`。</span><span class="sxs-lookup"><span data-stu-id="83e5c-243">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="83e5c-244">已拒絕重新連接。</span><span class="sxs-lookup"><span data-stu-id="83e5c-244">Reconnection rejected.</span></span> <span data-ttu-id="83e5c-245">已達到伺服器但拒絕連線，而且伺服器上的使用者狀態遺失。</span><span class="sxs-lookup"><span data-stu-id="83e5c-245">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="83e5c-246">若要重載應用程式，請呼叫 `location.reload()`。</span><span class="sxs-lookup"><span data-stu-id="83e5c-246">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="83e5c-247">當下列情況發生時，可能會產生此連接狀態：</span><span class="sxs-lookup"><span data-stu-id="83e5c-247">This connection state may result when:</span></span><ul><li><span data-ttu-id="83e5c-248">伺服器端線路發生損毀。</span><span class="sxs-lookup"><span data-stu-id="83e5c-248">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="83e5c-249">用戶端中斷連線的時間夠長，伺服器才會捨棄使用者的狀態。</span><span class="sxs-lookup"><span data-stu-id="83e5c-249">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="83e5c-250">系統會處置與使用者互動之元件的實例。</span><span class="sxs-lookup"><span data-stu-id="83e5c-250">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="83e5c-251">伺服器會重新開機，或回收應用程式的背景工作進程。</span><span class="sxs-lookup"><span data-stu-id="83e5c-251">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="83e5c-252">預呈現後的具狀態重新連接</span><span class="sxs-lookup"><span data-stu-id="83e5c-252">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="83e5c-253">在建立伺服器的用戶端連接之前，預設會設定 Blazor 伺服器應用程式，以預先呈現伺服器上的 UI。</span><span class="sxs-lookup"><span data-stu-id="83e5c-253">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="83e5c-254">這會在 *_Host. cshtml* Razor 頁面中設定：</span><span class="sxs-lookup"><span data-stu-id="83e5c-254">This is set up in the *_Host.cshtml* Razor page:</span></span>

::: moniker range=">= aspnetcore-3.1"

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

<span data-ttu-id="83e5c-255">`RenderMode` 設定元件是否：</span><span class="sxs-lookup"><span data-stu-id="83e5c-255">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="83e5c-256">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="83e5c-256">Is prerendered into the page.</span></span>
* <span data-ttu-id="83e5c-257">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="83e5c-257">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

::: moniker range=">= aspnetcore-3.1"

| `RenderMode`        | <span data-ttu-id="83e5c-258">描述</span><span class="sxs-lookup"><span data-stu-id="83e5c-258">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="83e5c-259">將元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="83e5c-259">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="83e5c-260">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="83e5c-260">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="83e5c-261">呈現 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="83e5c-261">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="83e5c-262">不包含來自元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="83e5c-262">Output from the component isn't included.</span></span> <span data-ttu-id="83e5c-263">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="83e5c-263">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="83e5c-264">將元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="83e5c-264">Renders the component into static HTML.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.1"

| `RenderMode`        | <span data-ttu-id="83e5c-265">描述</span><span class="sxs-lookup"><span data-stu-id="83e5c-265">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="83e5c-266">將元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="83e5c-266">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="83e5c-267">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="83e5c-267">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="83e5c-268">不支援參數。</span><span class="sxs-lookup"><span data-stu-id="83e5c-268">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="83e5c-269">呈現 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="83e5c-269">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="83e5c-270">不包含來自元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="83e5c-270">Output from the component isn't included.</span></span> <span data-ttu-id="83e5c-271">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="83e5c-271">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="83e5c-272">不支援參數。</span><span class="sxs-lookup"><span data-stu-id="83e5c-272">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="83e5c-273">將元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="83e5c-273">Renders the component into static HTML.</span></span> <span data-ttu-id="83e5c-274">支援參數。</span><span class="sxs-lookup"><span data-stu-id="83e5c-274">Parameters are supported.</span></span> |

::: moniker-end

<span data-ttu-id="83e5c-275">不支援從靜態 HTML 網頁轉譯伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="83e5c-275">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="83e5c-276">當 `RenderMode` `ServerPrerendered`時，元件一開始會以靜態方式轉譯為頁面的一部分。</span><span class="sxs-lookup"><span data-stu-id="83e5c-276">When `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="83e5c-277">當瀏覽器建立回到伺服器的連接後，就會*再次*轉譯該元件，而且該元件現在是互動式的。</span><span class="sxs-lookup"><span data-stu-id="83e5c-277">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="83e5c-278">如果有用來初始化元件的[生命週期方法](xref:blazor/components#lifecycle-methods)（`OnInitialized{Async}`），則會執行*兩次*方法：</span><span class="sxs-lookup"><span data-stu-id="83e5c-278">If a [lifecycle method](xref:blazor/components#lifecycle-methods) for initializing the component (`OnInitialized{Async}`) is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="83e5c-279">當元件以靜態方式資源清單時。</span><span class="sxs-lookup"><span data-stu-id="83e5c-279">When the component is prerendered statically.</span></span>
* <span data-ttu-id="83e5c-280">建立伺服器連接之後。</span><span class="sxs-lookup"><span data-stu-id="83e5c-280">After the server connection has been established.</span></span>

<span data-ttu-id="83e5c-281">這可能會導致在最後呈現元件時，UI 中顯示的資料有明顯的變更。</span><span class="sxs-lookup"><span data-stu-id="83e5c-281">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="83e5c-282">若要避免 Blazor 伺服器應用程式中的雙呈現案例：</span><span class="sxs-lookup"><span data-stu-id="83e5c-282">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="83e5c-283">傳入識別碼，可在自動處理期間用來快取狀態，並在應用程式重新開機之後，取得狀態。</span><span class="sxs-lookup"><span data-stu-id="83e5c-283">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="83e5c-284">在預入期間使用識別碼來儲存元件狀態。</span><span class="sxs-lookup"><span data-stu-id="83e5c-284">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="83e5c-285">在可呈現後使用識別碼，以取得快取的狀態。</span><span class="sxs-lookup"><span data-stu-id="83e5c-285">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="83e5c-286">下列程式碼示範以範本為基礎 Blazor 伺服器應用程式中，可避免雙重呈現的更新 `WeatherForecastService`：</span><span class="sxs-lookup"><span data-stu-id="83e5c-286">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] Summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = Summaries[rng.Next(Summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="83e5c-287">從 Razor 頁面和 views 轉譯具狀態的互動式元件</span><span class="sxs-lookup"><span data-stu-id="83e5c-287">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="83e5c-288">可設定狀態的互動式元件可以新增至 Razor 頁面或視圖。</span><span class="sxs-lookup"><span data-stu-id="83e5c-288">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="83e5c-289">當頁面或視圖呈現時：</span><span class="sxs-lookup"><span data-stu-id="83e5c-289">When the page or view renders:</span></span>

* <span data-ttu-id="83e5c-290">此元件是使用頁面或視圖所資源清單。</span><span class="sxs-lookup"><span data-stu-id="83e5c-290">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="83e5c-291">用來進行預呈現的初始元件狀態會遺失。</span><span class="sxs-lookup"><span data-stu-id="83e5c-291">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="83e5c-292">建立 SignalR 連接時，會建立新的元件狀態。</span><span class="sxs-lookup"><span data-stu-id="83e5c-292">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="83e5c-293">下列 Razor 頁面會呈現 `Counter` 元件：</span><span class="sxs-lookup"><span data-stu-id="83e5c-293">The following Razor page renders a `Counter` component:</span></span>

::: moniker range=">= aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="83e5c-294">從 Razor 頁面和 views 轉譯非互動式元件</span><span class="sxs-lookup"><span data-stu-id="83e5c-294">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="83e5c-295">在下列 Razor 頁面中，`MyComponent` 元件會以靜態方式轉譯，並使用以表單指定的初始值：</span><span class="sxs-lookup"><span data-stu-id="83e5c-295">In the following Razor page, the `MyComponent` component is statically rendered with an initial value that's specified using a form:</span></span>

::: moniker range=">= aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

@(await Html.RenderComponentAsync<MyComponent>(RenderMode.Static, 
    new { InitialValue = InitialValue }))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

<span data-ttu-id="83e5c-296">由於 `MyComponent` 是以靜態方式呈現，因此元件不能是互動式的。</span><span class="sxs-lookup"><span data-stu-id="83e5c-296">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="83e5c-297">偵測應用程式何時已進行預呈現</span><span class="sxs-lookup"><span data-stu-id="83e5c-297">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="83e5c-298">設定 Blazor 伺服器應用程式的 SignalR 用戶端</span><span class="sxs-lookup"><span data-stu-id="83e5c-298">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="83e5c-299">有時候，您需要設定 Blazor 伺服器應用程式所使用的 SignalR 用戶端。</span><span class="sxs-lookup"><span data-stu-id="83e5c-299">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="83e5c-300">例如，您可能會想要在 SignalR 用戶端上設定記錄，以診斷連接問題。</span><span class="sxs-lookup"><span data-stu-id="83e5c-300">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="83e5c-301">若要設定*Pages/_Host. cshtml*檔案中的 SignalR 用戶端：</span><span class="sxs-lookup"><span data-stu-id="83e5c-301">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="83e5c-302">將 `autostart="false"` 屬性加入至*blazor*腳本的 `<script>` 標記。</span><span class="sxs-lookup"><span data-stu-id="83e5c-302">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="83e5c-303">呼叫 `Blazor.start` 並傳入指定 SignalR 產生器的設定物件。</span><span class="sxs-lookup"><span data-stu-id="83e5c-303">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a><span data-ttu-id="83e5c-304">其他資源</span><span class="sxs-lookup"><span data-stu-id="83e5c-304">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
