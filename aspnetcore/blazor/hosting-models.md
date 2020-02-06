---
title: ASP.NET Core Blazor 裝載模型
author: guardrex
description: 瞭解 Blazor WebAssembly 和 Blazor 伺服器裝載模型。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: 7b4d4aca0bc4650c31bc8e5c4a84ecbad6a49b09
ms.sourcegitcommit: 0e21d4f8111743bcb205a2ae0f8e57910c3e8c25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2020
ms.locfileid: "77034079"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="f2474-103">ASP.NET Core Blazor 裝載模型</span><span class="sxs-lookup"><span data-stu-id="f2474-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="f2474-104">依[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f2474-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="f2474-105">Blazor 是一種 web 架構，設計用來在瀏覽器中以[WebAssembly](https://webassembly.org/)為基礎的 .net 執行時間（*Blazor WebAssembly*）或伺服器端在 ASP.NET Core （*Blazor server*）中執行用戶端。</span><span class="sxs-lookup"><span data-stu-id="f2474-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor WebAssembly*) or server-side in ASP.NET Core (*Blazor Server*).</span></span> <span data-ttu-id="f2474-106">無論裝載模型為何，應用程式和元件模型*都相同*。</span><span class="sxs-lookup"><span data-stu-id="f2474-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="f2474-107">若要為本文所述的主控模型建立專案，請參閱 <xref:blazor/get-started>。</span><span class="sxs-lookup"><span data-stu-id="f2474-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="f2474-108">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="f2474-108">Blazor WebAssembly</span></span>

<span data-ttu-id="f2474-109">Blazor 的主要裝載模型是在 WebAssembly 的瀏覽器中執行用戶端。</span><span class="sxs-lookup"><span data-stu-id="f2474-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="f2474-110">Blazor 應用程式、其相依性，和 .NET 執行階段會下載至瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="f2474-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="f2474-111">應用程式會直接在瀏覽器 UI 執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="f2474-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="f2474-112">UI 更新和事件處理會在同一個進程中進行。</span><span class="sxs-lookup"><span data-stu-id="f2474-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="f2474-113">應用程式的資產會以靜態檔案的形式部署至 web 伺服器或服務，以提供靜態內容給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f2474-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor WebAssembly： Blazor 應用程式會在瀏覽器內的 UI 執行緒上執行。](hosting-models/_static/blazor-webassembly.png)

<span data-ttu-id="f2474-115">若要使用用戶端裝載模型來建立 Blazor 應用程式，請使用**Blazor WebAssembly 應用程式**範本（[dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)）。</span><span class="sxs-lookup"><span data-stu-id="f2474-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="f2474-116">選取 [ **Blazor WebAssembly 應用程式**] 範本之後，您可以選擇將應用程式設定為使用 ASP.NET Core 後端，方法是選取 [ **ASP.NET Core**裝載] 核取方塊（[dotnet] [[新增 blazorwasm-](/dotnet/core/tools/dotnet-new)裝載）]。</span><span class="sxs-lookup"><span data-stu-id="f2474-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="f2474-117">ASP.NET Core 應用程式會將 Blazor 應用程式提供給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f2474-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="f2474-118">Blazor WebAssembly 應用程式可以使用 Web API 呼叫或[SignalR](xref:signalr/introduction) （<xref:tutorials/signalr-blazor-webassembly>），透過網路與伺服器互動。</span><span class="sxs-lookup"><span data-stu-id="f2474-118">The Blazor WebAssembly app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction) (<xref:tutorials/signalr-blazor-webassembly>).</span></span>

<span data-ttu-id="f2474-119">這些範本包含的 `blazor.webassembly.js` 腳本會處理：</span><span class="sxs-lookup"><span data-stu-id="f2474-119">The templates include the `blazor.webassembly.js` script that handles:</span></span>

* <span data-ttu-id="f2474-120">下載 .NET 執行時間、應用程式和應用程式的相依性。</span><span class="sxs-lookup"><span data-stu-id="f2474-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="f2474-121">初始化執行時間以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f2474-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="f2474-122">Blazor WebAssembly 裝載模型提供數個優點：</span><span class="sxs-lookup"><span data-stu-id="f2474-122">The Blazor WebAssembly hosting model offers several benefits:</span></span>

* <span data-ttu-id="f2474-123">沒有 .NET 伺服器端相依性。</span><span class="sxs-lookup"><span data-stu-id="f2474-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="f2474-124">應用程式會在下載至用戶端之後完全正常運作。</span><span class="sxs-lookup"><span data-stu-id="f2474-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="f2474-125">用戶端資源和功能都可以充分運用。</span><span class="sxs-lookup"><span data-stu-id="f2474-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="f2474-126">工作會從伺服器卸載至用戶端。</span><span class="sxs-lookup"><span data-stu-id="f2474-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="f2474-127">不需要 ASP.NET Core web 伺服器來裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="f2474-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="f2474-128">無伺服器部署案例可行（例如，從 CDN 提供應用程式）。</span><span class="sxs-lookup"><span data-stu-id="f2474-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="f2474-129">Blazor WebAssembly 裝載有缺點：</span><span class="sxs-lookup"><span data-stu-id="f2474-129">There are downsides to Blazor WebAssembly hosting:</span></span>

* <span data-ttu-id="f2474-130">應用程式僅限於瀏覽器的功能。</span><span class="sxs-lookup"><span data-stu-id="f2474-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="f2474-131">需要支援的用戶端硬體和軟體（例如 WebAssembly 支援）。</span><span class="sxs-lookup"><span data-stu-id="f2474-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="f2474-132">下載大小較大，且應用程式需要較長的時間來載入。</span><span class="sxs-lookup"><span data-stu-id="f2474-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="f2474-133">.NET 執行時間和工具支援較不成熟。</span><span class="sxs-lookup"><span data-stu-id="f2474-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="f2474-134">例如， [.NET Standard](/dotnet/standard/net-standard)支援和偵錯工具中有限制。</span><span class="sxs-lookup"><span data-stu-id="f2474-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="f2474-135">Blazor 伺服器</span><span class="sxs-lookup"><span data-stu-id="f2474-135">Blazor Server</span></span>

<span data-ttu-id="f2474-136">透過 Blazor 伺服器裝載模型，應用程式會在伺服器上從 ASP.NET Core 應用程式中執行。</span><span class="sxs-lookup"><span data-stu-id="f2474-136">With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="f2474-137">UI 更新、事件處理及 JavaScript 呼叫會透過 [SignalR](xref:signalr/introduction) 連線處理。</span><span class="sxs-lookup"><span data-stu-id="f2474-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![瀏覽器會透過 SignalR 連線，與伺服器上的應用程式互動（裝載于 ASP.NET Core 應用程式內）。](hosting-models/_static/blazor-server.png)

<span data-ttu-id="f2474-139">若要使用 Blazor 伺服器裝載模型來建立 Blazor 應用程式，請使用 ASP.NET Core **Blazor Server 應用程式**範本（[dotnet new blazorserver](/dotnet/core/tools/dotnet-new)）。</span><span class="sxs-lookup"><span data-stu-id="f2474-139">To create a Blazor app using the Blazor Server hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="f2474-140">ASP.NET Core 應用程式會裝載 Blazor 伺服器應用程式，並建立用戶端連接的 SignalR 端點。</span><span class="sxs-lookup"><span data-stu-id="f2474-140">The ASP.NET Core app hosts the Blazor Server app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="f2474-141">ASP.NET Core 應用程式會參考要新增的應用程式 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="f2474-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="f2474-142">伺服器端服務。</span><span class="sxs-lookup"><span data-stu-id="f2474-142">Server-side services.</span></span>
* <span data-ttu-id="f2474-143">應用程式至要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="f2474-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="f2474-144">`blazor.server.js` 腳本&dagger; 會建立用戶端連接。</span><span class="sxs-lookup"><span data-stu-id="f2474-144">The `blazor.server.js` script&dagger; establishes the client connection.</span></span> <span data-ttu-id="f2474-145">應用程式會負責保存和還原所需的應用程式狀態（例如，萬一網路連線中斷時）。</span><span class="sxs-lookup"><span data-stu-id="f2474-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="f2474-146">Blazor 伺服器裝載模型提供數個優點：</span><span class="sxs-lookup"><span data-stu-id="f2474-146">The Blazor Server hosting model offers several benefits:</span></span>

* <span data-ttu-id="f2474-147">下載大小明顯小於 Blazor WebAssembly 應用程式，而應用程式載入速度會更快。</span><span class="sxs-lookup"><span data-stu-id="f2474-147">Download size is significantly smaller than a Blazor WebAssembly app, and the app loads much faster.</span></span>
* <span data-ttu-id="f2474-148">應用程式會充分利用伺服器功能，包括使用任何 .NET Core 相容的 Api。</span><span class="sxs-lookup"><span data-stu-id="f2474-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="f2474-149">伺服器上的 .NET Core 是用來執行應用程式，因此現有的 .NET 工具（例如，「偵測」）會如預期般運作。</span><span class="sxs-lookup"><span data-stu-id="f2474-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="f2474-150">支援瘦用戶端。</span><span class="sxs-lookup"><span data-stu-id="f2474-150">Thin clients are supported.</span></span> <span data-ttu-id="f2474-151">例如，Blazor 伺服器應用程式適用于不支援 WebAssembly 的瀏覽器，以及在資源受限的裝置上。</span><span class="sxs-lookup"><span data-stu-id="f2474-151">For example, Blazor Server apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="f2474-152">應用程式的 .NET/C#程式碼基底（包括應用程式的元件代碼）不會提供給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f2474-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="f2474-153">有 Blazor 伺服器裝載的缺點：</span><span class="sxs-lookup"><span data-stu-id="f2474-153">There are downsides to Blazor Server hosting:</span></span>

* <span data-ttu-id="f2474-154">通常會有較高的延遲。</span><span class="sxs-lookup"><span data-stu-id="f2474-154">Higher latency usually exists.</span></span> <span data-ttu-id="f2474-155">每個使用者互動都牽涉到網路躍點。</span><span class="sxs-lookup"><span data-stu-id="f2474-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="f2474-156">沒有離線支援。</span><span class="sxs-lookup"><span data-stu-id="f2474-156">There's no offline support.</span></span> <span data-ttu-id="f2474-157">如果用戶端連線失敗，應用程式就會停止運作。</span><span class="sxs-lookup"><span data-stu-id="f2474-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="f2474-158">對於具有許多使用者的應用程式而言，擴充性是極具挑戰性的。</span><span class="sxs-lookup"><span data-stu-id="f2474-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="f2474-159">伺服器必須管理多個用戶端連接並處理用戶端狀態。</span><span class="sxs-lookup"><span data-stu-id="f2474-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="f2474-160">需要 ASP.NET Core 伺服器才能服務應用程式。</span><span class="sxs-lookup"><span data-stu-id="f2474-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="f2474-161">無伺服器部署案例不可行（例如，從 CDN 提供應用程式）。</span><span class="sxs-lookup"><span data-stu-id="f2474-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="f2474-162">&dagger;`blazor.server.js` 腳本是從 ASP.NET Core 共用架構中的內嵌資源所提供。</span><span class="sxs-lookup"><span data-stu-id="f2474-162">&dagger;The `blazor.server.js` script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="f2474-163">與伺服器呈現的 UI 比較</span><span class="sxs-lookup"><span data-stu-id="f2474-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="f2474-164">瞭解 Blazor 伺服器應用程式的其中一種方式，就是了解它與傳統模型有何不同，可以使用 Razor views 或 Razor Pages ASP.NET Core 應用程式中呈現 UI。</span><span class="sxs-lookup"><span data-stu-id="f2474-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="f2474-165">這兩種模型都使用 Razor 語言來描述 HTML 內容，但在標記呈現的方式上有很大的差異。</span><span class="sxs-lookup"><span data-stu-id="f2474-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="f2474-166">當 Razor 頁面或視圖呈現時，每一行 Razor 程式碼都會以文字格式發出 HTML。</span><span class="sxs-lookup"><span data-stu-id="f2474-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="f2474-167">呈現之後，伺服器會處置頁面或 view 實例，包括任何已產生的狀態。</span><span class="sxs-lookup"><span data-stu-id="f2474-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="f2474-168">當頁面的另一個要求發生時（例如，當伺服器驗證失敗且顯示驗證摘要時）：</span><span class="sxs-lookup"><span data-stu-id="f2474-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="f2474-169">整個頁面會再次重新顯示為 HTML 文字。</span><span class="sxs-lookup"><span data-stu-id="f2474-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="f2474-170">此頁面會傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="f2474-170">The page is sent to the client.</span></span>

<span data-ttu-id="f2474-171">Blazor 應用程式是由可重複使用的 UI 元素（稱為「*元件*」）所組成。</span><span class="sxs-lookup"><span data-stu-id="f2474-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="f2474-172">元件包含C#程式碼、標記和其他元件。</span><span class="sxs-lookup"><span data-stu-id="f2474-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="f2474-173">呈現元件時，Blazor 會產生類似于 HTML 或 XML 檔物件模型（DOM）的內含元件圖形。</span><span class="sxs-lookup"><span data-stu-id="f2474-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="f2474-174">此圖表包含屬性和欄位中保留的元件狀態。</span><span class="sxs-lookup"><span data-stu-id="f2474-174">This graph includes component state held in properties and fields.</span></span> <span data-ttu-id="f2474-175">Blazor 會評估元件圖形，以產生標記的二進位標記法。</span><span class="sxs-lookup"><span data-stu-id="f2474-175">Blazor evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="f2474-176">二進位格式可以是：</span><span class="sxs-lookup"><span data-stu-id="f2474-176">The binary format can be:</span></span>

* <span data-ttu-id="f2474-177">已轉換成 HTML 文字（在預建期間）。</span><span class="sxs-lookup"><span data-stu-id="f2474-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="f2474-178">用來在一般轉譯期間有效率地更新標記。</span><span class="sxs-lookup"><span data-stu-id="f2474-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="f2474-179">Blazor 中的 UI 更新會由下列觸發：</span><span class="sxs-lookup"><span data-stu-id="f2474-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="f2474-180">使用者互動，例如選取按鈕。</span><span class="sxs-lookup"><span data-stu-id="f2474-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="f2474-181">應用程式觸發程式，例如計時器。</span><span class="sxs-lookup"><span data-stu-id="f2474-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="f2474-182">圖形為重新顯示，且會計算 UI*差異*（差異）。</span><span class="sxs-lookup"><span data-stu-id="f2474-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="f2474-183">這項差異是更新用戶端上的 UI 所需的最小一組 DOM 編輯。</span><span class="sxs-lookup"><span data-stu-id="f2474-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="f2474-184">差異會以二進位格式傳送至用戶端，並由瀏覽器套用。</span><span class="sxs-lookup"><span data-stu-id="f2474-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="f2474-185">元件會在使用者從用戶端導覽出去之後處置。</span><span class="sxs-lookup"><span data-stu-id="f2474-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="f2474-186">當使用者與元件互動時，元件的狀態（服務、資源）必須保留在伺服器的記憶體中。</span><span class="sxs-lookup"><span data-stu-id="f2474-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="f2474-187">因為許多元件的狀態可能會由伺服器同時維護，所以記憶體耗盡是必須解決的問題。</span><span class="sxs-lookup"><span data-stu-id="f2474-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="f2474-188">如需有關如何撰寫 Blazor 伺服器應用程式以確保最佳使用伺服器記憶體的指引，請參閱 <xref:security/blazor/server>。</span><span class="sxs-lookup"><span data-stu-id="f2474-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server>.</span></span>

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="f2474-189">將 Razor 元件整合到 Razor Pages 和 MVC 應用程式中</span><span class="sxs-lookup"><span data-stu-id="f2474-189">Integrate Razor components into Razor Pages and MVC apps</span></span>

#### <a name="use-components-in-pages-and-views"></a><span data-ttu-id="f2474-190">使用頁面和視圖中的元件</span><span class="sxs-lookup"><span data-stu-id="f2474-190">Use components in pages and views</span></span>

<span data-ttu-id="f2474-191">現有的 Razor Pages 或 MVC 應用程式可以將 Razor 元件整合至頁面和 views：</span><span class="sxs-lookup"><span data-stu-id="f2474-191">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="f2474-192">在應用程式的版面配置檔案（ *_Layout. cshtml*）中：</span><span class="sxs-lookup"><span data-stu-id="f2474-192">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="f2474-193">將下列 `<base>` 標記新增至 `<head>` 元素：</span><span class="sxs-lookup"><span data-stu-id="f2474-193">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="f2474-194">上述範例中的 `href` 值（*應用程式基底路徑*）假設應用程式位於根 URL 路徑（`/`）。</span><span class="sxs-lookup"><span data-stu-id="f2474-194">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="f2474-195">如果應用程式是子應用程式，請遵循 <xref:host-and-deploy/blazor/index#app-base-path> 文章的*應用程式基底路徑*一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="f2474-195">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="f2474-196">*_Layout. cshtml*檔案位於 MVC 應用程式的 Razor Pages 應用程式或*Views/shared*資料夾中的*Pages/shared*資料夾內。</span><span class="sxs-lookup"><span data-stu-id="f2474-196">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="f2474-197">在結尾 `</body>` 標記內，新增*blazor*的 `<script>` 標記：</span><span class="sxs-lookup"><span data-stu-id="f2474-197">Add a `<script>` tag for the *blazor.server.js* script inside of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="f2474-198">架構會將*blazor*新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="f2474-198">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="f2474-199">不需要手動將腳本新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="f2474-199">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="f2474-200">使用下列內容，將 *_Imports razor*檔案新增至專案的根資料夾（將最後一個命名空間 `MyAppNamespace`變更為應用程式的命名空間）：</span><span class="sxs-lookup"><span data-stu-id="f2474-200">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```csharp
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="f2474-201">在 `Startup.ConfigureServices`中，新增 Blazor 伺服器服務：</span><span class="sxs-lookup"><span data-stu-id="f2474-201">In `Startup.ConfigureServices`, add the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="f2474-202">在 `Startup.Configure`中，將 Blazor 中樞端點新增至 `app.UseEndpoints`：</span><span class="sxs-lookup"><span data-stu-id="f2474-202">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="f2474-203">將元件整合到任何頁面或視圖中。</span><span class="sxs-lookup"><span data-stu-id="f2474-203">Integrate components into any page or view.</span></span> <span data-ttu-id="f2474-204">如需詳細資訊，請參閱 <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> 文章的將*元件整合到 Razor Pages 和 MVC 應用程式*一節。</span><span class="sxs-lookup"><span data-stu-id="f2474-204">For more information, see the *Integrate components into Razor Pages and MVC apps* section of the <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> article.</span></span>

#### <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="f2474-205">在 Razor Pages 應用程式中使用可路由的元件</span><span class="sxs-lookup"><span data-stu-id="f2474-205">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="f2474-206">在 Razor Pages 應用程式中支援可路由的 Razor 元件：</span><span class="sxs-lookup"><span data-stu-id="f2474-206">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="f2474-207">請遵循[使用頁面和 views 中的元件](#use-components-in-pages-and-views)一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="f2474-207">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="f2474-208">使用下列內容，將*應用程式 razor*檔案新增至專案的根目錄：</span><span class="sxs-lookup"><span data-stu-id="f2474-208">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="f2474-209">將 *_Host. cshtml*檔案新增至*Pages*資料夾，其中包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="f2474-209">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="f2474-210">元件會使用共用的 *_Layout. cshtml*檔案作為其版面配置。</span><span class="sxs-lookup"><span data-stu-id="f2474-210">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="f2474-211">在 `Startup.Configure`中，將 *_Host. cshtml*頁面的低優先順序路由新增至端點設定：</span><span class="sxs-lookup"><span data-stu-id="f2474-211">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="f2474-212">將可路由的元件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="f2474-212">Add routable components to the app.</span></span> <span data-ttu-id="f2474-213">例如，</span><span class="sxs-lookup"><span data-stu-id="f2474-213">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="f2474-214">使用自訂資料夾來保存應用程式的元件時，請將代表資料夾的命名空間新增至*Pages/_ViewImports. cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="f2474-214">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Pages/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="f2474-215">如需詳細資訊，請參閱 <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>。</span><span class="sxs-lookup"><span data-stu-id="f2474-215">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

#### <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="f2474-216">在 MVC 應用程式中使用可路由的元件</span><span class="sxs-lookup"><span data-stu-id="f2474-216">Use routable components in an MVC app</span></span>

<span data-ttu-id="f2474-217">在 MVC 應用程式中支援可路由的 Razor 元件：</span><span class="sxs-lookup"><span data-stu-id="f2474-217">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="f2474-218">請遵循[使用頁面和 views 中的元件](#use-components-in-pages-and-views)一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="f2474-218">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="f2474-219">使用下列內容，將*應用程式 razor*檔案新增至專案的根目錄：</span><span class="sxs-lookup"><span data-stu-id="f2474-219">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="f2474-220">使用下列內容，將 *_Host. cshtml*檔案新增至*Views/Home*資料夾：</span><span class="sxs-lookup"><span data-stu-id="f2474-220">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="f2474-221">元件會使用共用的 *_Layout. cshtml*檔案作為其版面配置。</span><span class="sxs-lookup"><span data-stu-id="f2474-221">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="f2474-222">將動作新增至主控制器：</span><span class="sxs-lookup"><span data-stu-id="f2474-222">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="f2474-223">為控制器動作新增低優先順序的路由，以將 *_Host. cshtml*視圖傳回 `Startup.Configure`中的端點設定：</span><span class="sxs-lookup"><span data-stu-id="f2474-223">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="f2474-224">建立*Pages*資料夾，並將可路由的元件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="f2474-224">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="f2474-225">例如，</span><span class="sxs-lookup"><span data-stu-id="f2474-225">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="f2474-226">使用自訂資料夾來存放應用程式的元件時，請將代表資料夾的命名空間新增至*Views/_ViewImports. cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="f2474-226">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="f2474-227">如需詳細資訊，請參閱 <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>。</span><span class="sxs-lookup"><span data-stu-id="f2474-227">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

### <a name="circuits"></a><span data-ttu-id="f2474-228">獲得</span><span class="sxs-lookup"><span data-stu-id="f2474-228">Circuits</span></span>

<span data-ttu-id="f2474-229">Blazor 伺服器應用程式建置於[ASP.NET Core SignalR](xref:signalr/introduction)之上。</span><span class="sxs-lookup"><span data-stu-id="f2474-229">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="f2474-230">每個用戶端會透過一或多個稱為*線路*的 SignalR 連線來與伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="f2474-230">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="f2474-231">線路是 Blazor 的抽象概念，SignalR 連線可容忍暫時的網路中斷。</span><span class="sxs-lookup"><span data-stu-id="f2474-231">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="f2474-232">當 Blazor 用戶端看到 SignalR 連線已中斷連線時，它會嘗試使用新的 SignalR 連線重新連線到伺服器。</span><span class="sxs-lookup"><span data-stu-id="f2474-232">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="f2474-233">連線至 Blazor 伺服器應用程式的每個瀏覽器畫面（瀏覽器索引標籤或 iframe）都會使用 SignalR 連接。</span><span class="sxs-lookup"><span data-stu-id="f2474-233">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="f2474-234">相較于一般伺服器呈現的應用程式，這還是另一項重要的差異。</span><span class="sxs-lookup"><span data-stu-id="f2474-234">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="f2474-235">在伺服器呈現的應用程式中，在多個瀏覽器畫面中開啟相同的應用程式，通常不會轉譯成伺服器上的其他資源需求。</span><span class="sxs-lookup"><span data-stu-id="f2474-235">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="f2474-236">在 Blazor 伺服器應用程式中，每個瀏覽器畫面都需要個別的線路，且元件狀態的個別實例會由伺服器管理。</span><span class="sxs-lookup"><span data-stu-id="f2474-236">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

<span data-ttu-id="f2474-237">Blazor 會考慮關閉瀏覽器索引標籤，或流覽至外部 URL 的*正常*終止。</span><span class="sxs-lookup"><span data-stu-id="f2474-237">Blazor considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="f2474-238">在正常終止的事件中，會立即釋放線路和相關聯的資源。</span><span class="sxs-lookup"><span data-stu-id="f2474-238">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="f2474-239">用戶端也可能會因為網路中斷而無法正常地中斷連線。</span><span class="sxs-lookup"><span data-stu-id="f2474-239">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> <span data-ttu-id="f2474-240">Blazor 伺服器會儲存已中斷連線的線路，以取得可設定的間隔，以允許用戶端重新連線。</span><span class="sxs-lookup"><span data-stu-id="f2474-240">Blazor Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="f2474-241">如需詳細資訊，請參閱重新[連接到相同的伺服器](#reconnection-to-the-same-server)一節。</span><span class="sxs-lookup"><span data-stu-id="f2474-241">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="f2474-242">UI 延遲</span><span class="sxs-lookup"><span data-stu-id="f2474-242">UI Latency</span></span>

<span data-ttu-id="f2474-243">UI 延遲是從起始的動作到 UI 更新時間所花費的時間。</span><span class="sxs-lookup"><span data-stu-id="f2474-243">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="f2474-244">較小的 UI 延遲值是應用程式對使用者進行回應的必要行為。</span><span class="sxs-lookup"><span data-stu-id="f2474-244">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="f2474-245">在 Blazor 伺服器應用程式中，會將每個動作傳送至伺服器、進行處理，並傳回 UI 差異。</span><span class="sxs-lookup"><span data-stu-id="f2474-245">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="f2474-246">因此，UI 延遲是網路延遲和處理動作之伺服器延遲的總和。</span><span class="sxs-lookup"><span data-stu-id="f2474-246">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="f2474-247">對於僅限於私人商業網路的企業營運應用程式，通常會 imperceptible 因網路延遲而對使用者的延遲所造成的影響。</span><span class="sxs-lookup"><span data-stu-id="f2474-247">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="f2474-248">對於透過網際網路部署的應用程式，使用者的延遲可能會很明顯，尤其是在地理位置廣泛散佈的使用者時。</span><span class="sxs-lookup"><span data-stu-id="f2474-248">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="f2474-249">記憶體使用量也會導致應用程式延遲。</span><span class="sxs-lookup"><span data-stu-id="f2474-249">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="f2474-250">增加記憶體使用量會導致頻繁的垃圾收集或將記憶體分頁到磁片，這兩者都會降低應用程式效能，因而增加 UI 延遲。</span><span class="sxs-lookup"><span data-stu-id="f2474-250">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="f2474-251">如需詳細資訊，請參閱 <xref:security/blazor/server>。</span><span class="sxs-lookup"><span data-stu-id="f2474-251">For more information, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="f2474-252">Blazor 伺服器應用程式應該經過優化，藉由減少網路延遲和記憶體使用量，將 UI 延遲降到最低。</span><span class="sxs-lookup"><span data-stu-id="f2474-252">Blazor Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="f2474-253">如需測量網路延遲的方法，請參閱 <xref:host-and-deploy/blazor/server#measure-network-latency>。</span><span class="sxs-lookup"><span data-stu-id="f2474-253">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server#measure-network-latency>.</span></span> <span data-ttu-id="f2474-254">如需 SignalR 和 Blazor 的詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="f2474-254">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a><span data-ttu-id="f2474-255">連接到伺服器</span><span class="sxs-lookup"><span data-stu-id="f2474-255">Connection to the server</span></span>

<span data-ttu-id="f2474-256">Blazor 伺服器應用程式需要伺服器的作用中 SignalR 連接。</span><span class="sxs-lookup"><span data-stu-id="f2474-256">Blazor Server apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="f2474-257">如果連接中斷，應用程式會嘗試重新連線到伺服器。</span><span class="sxs-lookup"><span data-stu-id="f2474-257">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="f2474-258">只要用戶端的狀態仍在記憶體中，用戶端會話就會繼續，而不會失去狀態。</span><span class="sxs-lookup"><span data-stu-id="f2474-258">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

#### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="f2474-259">重新連接到相同的伺服器</span><span class="sxs-lookup"><span data-stu-id="f2474-259">Reconnection to the same server</span></span>

<span data-ttu-id="f2474-260">Blazor 伺服器應用程式會 prerenders，以回應第一個用戶端要求，這會在伺服器上設定 UI 狀態。</span><span class="sxs-lookup"><span data-stu-id="f2474-260">A Blazor Server app prerenders in response to the first client request, which sets up the UI state on the server.</span></span> <span data-ttu-id="f2474-261">當用戶端嘗試建立 SignalR 連接時，用戶端必須重新連線到相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="f2474-261">When the client attempts to create a SignalR connection, the client must reconnect to the same server.</span></span> <span data-ttu-id="f2474-262">使用一部以上後端伺服器的 Blazor 伺服器應用程式應該針對 SignalR 連線執行*粘滯會話*。</span><span class="sxs-lookup"><span data-stu-id="f2474-262">Blazor Server apps that use more than one backend server should implement *sticky sessions* for SignalR connections.</span></span>

<span data-ttu-id="f2474-263">我們建議使用 Blazor 伺服器應用程式的[Azure SignalR Service](/azure/azure-signalr) 。</span><span class="sxs-lookup"><span data-stu-id="f2474-263">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="f2474-264">此服務可讓您將 Blazor 伺服器應用程式相應增加為大量的並行 SignalR 連接。</span><span class="sxs-lookup"><span data-stu-id="f2474-264">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="f2474-265">藉由將服務的 `ServerStickyMode` 選項或設定值設為 [`Required`]，來啟用 Azure SignalR Service 的 [粘滯會話]。</span><span class="sxs-lookup"><span data-stu-id="f2474-265">Sticky sessions are enabled for the Azure SignalR Service by setting the service's `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="f2474-266">如需詳細資訊，請參閱 <xref:host-and-deploy/blazor/server#signalr-configuration>。</span><span class="sxs-lookup"><span data-stu-id="f2474-266">For more information, see <xref:host-and-deploy/blazor/server#signalr-configuration>.</span></span>

<span data-ttu-id="f2474-267">使用 IIS 時，會使用應用程式要求路由來啟用「粘滯會話」。</span><span class="sxs-lookup"><span data-stu-id="f2474-267">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="f2474-268">如需詳細資訊，請參閱[使用應用程式要求路由的 HTTP 負載平衡](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)。</span><span class="sxs-lookup"><span data-stu-id="f2474-268">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="f2474-269">反映 UI 中的連接狀態</span><span class="sxs-lookup"><span data-stu-id="f2474-269">Reflect the connection state in the UI</span></span>

<span data-ttu-id="f2474-270">當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。</span><span class="sxs-lookup"><span data-stu-id="f2474-270">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="f2474-271">如果重新連線失敗，則會提供使用者重試的選項。</span><span class="sxs-lookup"><span data-stu-id="f2474-271">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="f2474-272">若要自訂 UI，請在 *_Host*的 [Razor 頁面] `<body>` 中，定義具有 `components-reconnect-modal` `id` 的元素：</span><span class="sxs-lookup"><span data-stu-id="f2474-272">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="f2474-273">下表描述套用至 `components-reconnect-modal` 元素的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="f2474-273">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="f2474-274">CSS 類別</span><span class="sxs-lookup"><span data-stu-id="f2474-274">CSS class</span></span>                       | <span data-ttu-id="f2474-275">表示&hellip;</span><span class="sxs-lookup"><span data-stu-id="f2474-275">Indicates&hellip;</span></span> |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | <span data-ttu-id="f2474-276">遺失的連接。</span><span class="sxs-lookup"><span data-stu-id="f2474-276">A lost connection.</span></span> <span data-ttu-id="f2474-277">用戶端正在嘗試重新連線。</span><span class="sxs-lookup"><span data-stu-id="f2474-277">The client is attempting to reconnect.</span></span> <span data-ttu-id="f2474-278">顯示強制回應。</span><span class="sxs-lookup"><span data-stu-id="f2474-278">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="f2474-279">系統會將使用中的連接重新建立至伺服器。</span><span class="sxs-lookup"><span data-stu-id="f2474-279">An active connection is re-established to the server.</span></span> <span data-ttu-id="f2474-280">隱藏強制回應。</span><span class="sxs-lookup"><span data-stu-id="f2474-280">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="f2474-281">重新連接失敗，可能是因為網路失敗。</span><span class="sxs-lookup"><span data-stu-id="f2474-281">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="f2474-282">若要嘗試重新連接，請呼叫 `window.Blazor.reconnect()`。</span><span class="sxs-lookup"><span data-stu-id="f2474-282">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="f2474-283">已拒絕重新連接。</span><span class="sxs-lookup"><span data-stu-id="f2474-283">Reconnection rejected.</span></span> <span data-ttu-id="f2474-284">已達到伺服器但拒絕連線，而且伺服器上的使用者狀態遺失。</span><span class="sxs-lookup"><span data-stu-id="f2474-284">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="f2474-285">若要重載應用程式，請呼叫 `location.reload()`。</span><span class="sxs-lookup"><span data-stu-id="f2474-285">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="f2474-286">當下列情況發生時，可能會產生此連接狀態：</span><span class="sxs-lookup"><span data-stu-id="f2474-286">This connection state may result when:</span></span><ul><li><span data-ttu-id="f2474-287">伺服器端線路發生損毀。</span><span class="sxs-lookup"><span data-stu-id="f2474-287">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="f2474-288">用戶端中斷連線的時間夠長，伺服器才會捨棄使用者的狀態。</span><span class="sxs-lookup"><span data-stu-id="f2474-288">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="f2474-289">系統會處置與使用者互動之元件的實例。</span><span class="sxs-lookup"><span data-stu-id="f2474-289">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="f2474-290">伺服器會重新開機，或回收應用程式的背景工作進程。</span><span class="sxs-lookup"><span data-stu-id="f2474-290">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="f2474-291">預呈現後的具狀態重新連接</span><span class="sxs-lookup"><span data-stu-id="f2474-291">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="f2474-292">在建立伺服器的用戶端連接之前，預設會設定 Blazor 伺服器應用程式，以預先呈現伺服器上的 UI。</span><span class="sxs-lookup"><span data-stu-id="f2474-292">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="f2474-293">這會在 *_Host. cshtml* Razor 頁面中設定：</span><span class="sxs-lookup"><span data-stu-id="f2474-293">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="f2474-294">`RenderMode` 設定元件是否：</span><span class="sxs-lookup"><span data-stu-id="f2474-294">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="f2474-295">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="f2474-295">Is prerendered into the page.</span></span>
* <span data-ttu-id="f2474-296">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="f2474-296">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="f2474-297">描述</span><span class="sxs-lookup"><span data-stu-id="f2474-297">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="f2474-298">將元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="f2474-298">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="f2474-299">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f2474-299">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="f2474-300">呈現 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="f2474-300">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="f2474-301">不包含來自元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="f2474-301">Output from the component isn't included.</span></span> <span data-ttu-id="f2474-302">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f2474-302">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="f2474-303">將元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="f2474-303">Renders the component into static HTML.</span></span> |

<span data-ttu-id="f2474-304">不支援從靜態 HTML 網頁轉譯伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="f2474-304">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="f2474-305">當 `RenderMode` `ServerPrerendered`時，元件一開始會以靜態方式轉譯為頁面的一部分。</span><span class="sxs-lookup"><span data-stu-id="f2474-305">When `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="f2474-306">當瀏覽器建立回到伺服器的連接後，就會*再次*轉譯該元件，而且該元件現在是互動式的。</span><span class="sxs-lookup"><span data-stu-id="f2474-306">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="f2474-307">如果存在用於初始化元件的[OnInitialized {Async}](xref:blazor/lifecycle#component-initialization-methods)生命週期方法，則會執行*兩次*方法：</span><span class="sxs-lookup"><span data-stu-id="f2474-307">If the [OnInitialized{Async}](xref:blazor/lifecycle#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="f2474-308">當元件以靜態方式資源清單時。</span><span class="sxs-lookup"><span data-stu-id="f2474-308">When the component is prerendered statically.</span></span>
* <span data-ttu-id="f2474-309">建立伺服器連接之後。</span><span class="sxs-lookup"><span data-stu-id="f2474-309">After the server connection has been established.</span></span>

<span data-ttu-id="f2474-310">這可能會導致在最後呈現元件時，UI 中顯示的資料有明顯的變更。</span><span class="sxs-lookup"><span data-stu-id="f2474-310">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="f2474-311">若要避免 Blazor 伺服器應用程式中的雙呈現案例：</span><span class="sxs-lookup"><span data-stu-id="f2474-311">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="f2474-312">傳入識別碼，可在自動處理期間用來快取狀態，並在應用程式重新開機之後，取得狀態。</span><span class="sxs-lookup"><span data-stu-id="f2474-312">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="f2474-313">在預入期間使用識別碼來儲存元件狀態。</span><span class="sxs-lookup"><span data-stu-id="f2474-313">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="f2474-314">在可呈現後使用識別碼，以取得快取的狀態。</span><span class="sxs-lookup"><span data-stu-id="f2474-314">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="f2474-315">下列程式碼示範以範本為基礎 Blazor 伺服器應用程式中，可避免雙重呈現的更新 `WeatherForecastService`：</span><span class="sxs-lookup"><span data-stu-id="f2474-315">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] _summaries = new[]
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
                Summary = _summaries[rng.Next(_summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="f2474-316">從 Razor 頁面和 views 轉譯具狀態的互動式元件</span><span class="sxs-lookup"><span data-stu-id="f2474-316">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="f2474-317">可設定狀態的互動式元件可以新增至 Razor 頁面或視圖。</span><span class="sxs-lookup"><span data-stu-id="f2474-317">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="f2474-318">當頁面或視圖呈現時：</span><span class="sxs-lookup"><span data-stu-id="f2474-318">When the page or view renders:</span></span>

* <span data-ttu-id="f2474-319">此元件是使用頁面或視圖所資源清單。</span><span class="sxs-lookup"><span data-stu-id="f2474-319">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="f2474-320">用來進行預呈現的初始元件狀態會遺失。</span><span class="sxs-lookup"><span data-stu-id="f2474-320">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="f2474-321">建立 SignalR 連接時，會建立新的元件狀態。</span><span class="sxs-lookup"><span data-stu-id="f2474-321">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="f2474-322">下列 Razor 頁面會呈現 `Counter` 元件：</span><span class="sxs-lookup"><span data-stu-id="f2474-322">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="f2474-323">從 Razor 頁面和 views 轉譯非互動式元件</span><span class="sxs-lookup"><span data-stu-id="f2474-323">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="f2474-324">在下列 Razor 頁面中，`Counter` 元件會以靜態方式轉譯，並使用以表單指定的初始值：</span><span class="sxs-lookup"><span data-stu-id="f2474-324">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="f2474-325">由於 `MyComponent` 是以靜態方式呈現，因此元件不能是互動式的。</span><span class="sxs-lookup"><span data-stu-id="f2474-325">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="f2474-326">偵測應用程式何時已進行預呈現</span><span class="sxs-lookup"><span data-stu-id="f2474-326">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="f2474-327">設定 Blazor 伺服器應用程式的 SignalR 用戶端</span><span class="sxs-lookup"><span data-stu-id="f2474-327">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="f2474-328">有時候，您需要設定 Blazor 伺服器應用程式所使用的 SignalR 用戶端。</span><span class="sxs-lookup"><span data-stu-id="f2474-328">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="f2474-329">例如，您可能會想要在 SignalR 用戶端上設定記錄，以診斷連接問題。</span><span class="sxs-lookup"><span data-stu-id="f2474-329">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="f2474-330">若要設定*Pages/_Host. cshtml*檔案中的 SignalR 用戶端：</span><span class="sxs-lookup"><span data-stu-id="f2474-330">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="f2474-331">將 `autostart="false"` 屬性加入至 `blazor.server.js` 腳本的 `<script>` 標記。</span><span class="sxs-lookup"><span data-stu-id="f2474-331">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="f2474-332">呼叫 `Blazor.start` 並傳入指定 SignalR 產生器的設定物件。</span><span class="sxs-lookup"><span data-stu-id="f2474-332">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="f2474-333">其他資源</span><span class="sxs-lookup"><span data-stu-id="f2474-333">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
* <xref:tutorials/signalr-blazor-webassembly>
