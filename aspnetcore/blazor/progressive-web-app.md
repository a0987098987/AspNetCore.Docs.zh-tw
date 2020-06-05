---
title: 使用 ASP.NET Core WebAssembly 建立漸進式 Web 應用程式 Blazor
author: guardrex
description: 瞭解如何建立以新的 Blazor 瀏覽器功能為基礎的漸進式 Web 應用程式（PWA），其行為類似于桌面應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/progressive-web-app
ms.openlocfilehash: 274516014c027972166402abc70d22fa801898de
ms.sourcegitcommit: cd73744bd75fdefb31d25ab906df237f07ee7a0a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84451845"
---
# <a name="build-progressive-web-applications-with-aspnet-core-blazor-webassembly"></a><span data-ttu-id="54b45-103">使用 ASP.NET Core WebAssembly 建立漸進式 Web 應用程式 Blazor</span><span class="sxs-lookup"><span data-stu-id="54b45-103">Build Progressive Web Applications with ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="54b45-104">作者：[Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="54b45-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="54b45-105">漸進式 Web 應用程式（PWA）是單一頁面應用程式（SPA），其使用現代化瀏覽器 Api 和功能的行為，就像桌面應用程式一樣。</span><span class="sxs-lookup"><span data-stu-id="54b45-105">A Progressive Web Application (PWA) is a Single Page Application (SPA) that uses modern browser APIs and capabilities to behave like a desktop app.</span></span> Blazor<span data-ttu-id="54b45-106">WebAssembly 是以標準為基礎的用戶端 web 應用程式平臺，因此可以使用任何瀏覽器 API，包括下列功能所需的 PWA Api：</span><span class="sxs-lookup"><span data-stu-id="54b45-106"> WebAssembly is a standards-based client-side web app platform, so it can use any browser API, including PWA APIs required for the following capabilities:</span></span>

* <span data-ttu-id="54b45-107">離線工作並立即載入，獨立于網路速度。</span><span class="sxs-lookup"><span data-stu-id="54b45-107">Working offline and loading instantly, independent of network speed.</span></span>
* <span data-ttu-id="54b45-108">在自己的應用程式視窗中執行，而不只是瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="54b45-108">Running in its own app window, not just a browser window.</span></span>
* <span data-ttu-id="54b45-109">從主機的作業系統 [開始] 功能表、[停駐] 或 [主畫面] 啟動。</span><span class="sxs-lookup"><span data-stu-id="54b45-109">Being launched from the host's operating system start menu, dock, or home screen.</span></span>
* <span data-ttu-id="54b45-110">即使使用者未使用應用程式，也會從後端伺服器接收推播通知。</span><span class="sxs-lookup"><span data-stu-id="54b45-110">Receiving push notifications from a backend server, even while the user isn't using the app.</span></span>
* <span data-ttu-id="54b45-111">自動在背景中更新。</span><span class="sxs-lookup"><span data-stu-id="54b45-111">Automatically updating in the background.</span></span>

<span data-ttu-id="54b45-112">*漸進式*一詞是用來描述這類應用程式，原因如下：</span><span class="sxs-lookup"><span data-stu-id="54b45-112">The word *progressive* is used to describe such apps because:</span></span>

* <span data-ttu-id="54b45-113">使用者可能會先在網頁瀏覽器中探索並使用應用程式，就像任何其他 SPA 一樣。</span><span class="sxs-lookup"><span data-stu-id="54b45-113">A user might first discover and use the app within their web browser like any other SPA.</span></span>
* <span data-ttu-id="54b45-114">之後，使用者會在其 OS 中進行安裝，並啟用推播通知。</span><span class="sxs-lookup"><span data-stu-id="54b45-114">Later, the user progresses to installing it in their OS and enabling push notifications.</span></span>

## <a name="create-a-project-from-the-pwa-template"></a><span data-ttu-id="54b45-115">從 PWA 範本建立專案</span><span class="sxs-lookup"><span data-stu-id="54b45-115">Create a project from the PWA template</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="54b45-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="54b45-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="54b45-117">在 [**建立新專案**] 對話方塊中建立新的\*\* Blazor WebAssembly 應用程式**時，選取 [**漸進式 Web 應用程式\*\*] 核取方塊：</span><span class="sxs-lookup"><span data-stu-id="54b45-117">When creating a new **Blazor WebAssembly App** in the **Create a New Project** dialog, select the **Progressive Web Application** check box:</span></span>

![[Visual Studio 新增專案] 對話方塊中已選取 [漸進式 Web 應用程式] 核取方塊。](progressive-web-app/_static/image1.png)

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

-->

# <a name="visual-studio-code--net-core-cli"></a>[<span data-ttu-id="54b45-119">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="54b45-119">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="54b45-120">使用參數在命令 shell 中建立 PWA 專案 `--pwa` ：</span><span class="sxs-lookup"><span data-stu-id="54b45-120">Create a PWA project in a command shell with the `--pwa` switch:</span></span>

```dotnetcli
dotnet new blazorwasm -o MyNewProject --pwa
```

---

<span data-ttu-id="54b45-121">您可以選擇性地針對從 ASP.NET Core 裝載的範本所建立的應用程式，設定 PWA。</span><span class="sxs-lookup"><span data-stu-id="54b45-121">Optionally, PWA can be configured for an app created from the ASP.NET Core Hosted template.</span></span> <span data-ttu-id="54b45-122">PWA 案例與裝載模型無關。</span><span class="sxs-lookup"><span data-stu-id="54b45-122">The PWA scenario is independent of the hosting model.</span></span>

## <a name="installation-and-app-manifest"></a><span data-ttu-id="54b45-123">安裝和應用程式資訊清單</span><span class="sxs-lookup"><span data-stu-id="54b45-123">Installation and app manifest</span></span>

<span data-ttu-id="54b45-124">造訪使用 PWA 範本建立的應用程式時，使用者可以選擇將應用程式安裝到其作業系統的 [開始] 功能表、[停駐] 或 [主畫面]。</span><span class="sxs-lookup"><span data-stu-id="54b45-124">When visiting an app created using the PWA template, users have the option of installing the app into their OS's start menu, dock, or home screen.</span></span> <span data-ttu-id="54b45-125">此選項的呈現方式取決於使用者的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="54b45-125">The way this option is presented depends on the user's browser.</span></span> <span data-ttu-id="54b45-126">使用以桌面 Chromium 為基礎的瀏覽器（例如 Edge 或 Chrome）時，URL 列中會出現 [**新增**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="54b45-126">When using desktop Chromium-based browsers, such as Edge or Chrome, an **Add** button appears within the URL bar.</span></span> <span data-ttu-id="54b45-127">在使用者選取 [**新增**] 按鈕之後，他們會收到確認對話方塊：</span><span class="sxs-lookup"><span data-stu-id="54b45-127">After the user selects the **Add** button, they receive a confirmation dialog:</span></span>

![Google Chrome 中的確認 diaglog 會向使用者顯示「MyBlazorPwa」應用程式的 [安裝] 按鈕按鈕。](progressive-web-app/_static/image2.png)

<span data-ttu-id="54b45-129">在 iOS 上，訪客可以使用 Safari 的 [**共用**] 按鈕和其 [**新增至 Homescreen** ] 選項來安裝 PWA。</span><span class="sxs-lookup"><span data-stu-id="54b45-129">On iOS, visitors can install the PWA using Safari's **Share** button and its **Add to Homescreen** option.</span></span> <span data-ttu-id="54b45-130">在適用于 Android 的 Chrome 上，使用者應該選取右上角的 [**功能表**] 按鈕，然後按一下 [**新增到主畫面**]。</span><span class="sxs-lookup"><span data-stu-id="54b45-130">On Chrome for Android, users should select the **Menu** button in the upper-right corner, followed by **Add to Home screen**.</span></span>

<span data-ttu-id="54b45-131">安裝之後，應用程式會出現在其本身的視窗中，而不會有位址列：</span><span class="sxs-lookup"><span data-stu-id="54b45-131">Once installed, the app appears in its own window without an address bar:</span></span>

![' MyBlazorPwa ' 應用程式會在沒有網址列的 Google Chrome 中執行。](progressive-web-app/_static/image3.png)

<span data-ttu-id="54b45-133">若要自訂視窗的標題、色彩配置、圖示或其他詳細資料，請參閱專案的*wwwroot*目錄中的*資訊清單. json*檔案。</span><span class="sxs-lookup"><span data-stu-id="54b45-133">To customize the window's title, color scheme, icon, or other details, see the *manifest.json* file in the project's *wwwroot* directory.</span></span> <span data-ttu-id="54b45-134">此檔案的架構是由 web 標準所定義。</span><span class="sxs-lookup"><span data-stu-id="54b45-134">The schema of this file is defined by web standards.</span></span> <span data-ttu-id="54b45-135">如需詳細資訊，請參閱[MDN web 檔： Web 應用程式資訊清單](https://developer.mozilla.org/docs/Web/Manifest)。</span><span class="sxs-lookup"><span data-stu-id="54b45-135">For more information, see [MDN web docs: Web App Manifest](https://developer.mozilla.org/docs/Web/Manifest).</span></span>

## <a name="offline-support"></a><span data-ttu-id="54b45-136">離線支援</span><span class="sxs-lookup"><span data-stu-id="54b45-136">Offline support</span></span>

<span data-ttu-id="54b45-137">根據預設，使用 PWA 範本選項建立的應用程式支援離線執行。</span><span class="sxs-lookup"><span data-stu-id="54b45-137">By default, apps created using the PWA template option have support for running offline.</span></span> <span data-ttu-id="54b45-138">使用者必須先在線上流覽應用程式。</span><span class="sxs-lookup"><span data-stu-id="54b45-138">A user must first visit the app while they're online.</span></span> <span data-ttu-id="54b45-139">瀏覽器會自動下載並快取離線操作所需的所有資源。</span><span class="sxs-lookup"><span data-stu-id="54b45-139">The browser automatically downloads and caches all of the resources required to operate offline.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="54b45-140">開發支援會干擾進行變更和測試的一般開發週期。</span><span class="sxs-lookup"><span data-stu-id="54b45-140">Development support would interfere with the usual development cycle of making changes and testing them.</span></span> <span data-ttu-id="54b45-141">因此，只會針對*已發佈*的應用程式啟用離線支援。</span><span class="sxs-lookup"><span data-stu-id="54b45-141">Therefore, offline support is only enabled for *published* apps.</span></span> 

> [!WARNING]
> <span data-ttu-id="54b45-142">如果您想要散發已啟用離線的 PWA，有[幾個重要的警告和注意事項](#caveats-for-offline-pwas)。</span><span class="sxs-lookup"><span data-stu-id="54b45-142">If you intend to distribute an offline-enabled PWA, there are [several important warnings and caveats](#caveats-for-offline-pwas).</span></span> <span data-ttu-id="54b45-143">這些案例都是離線 Pwa 的固有，而不是特定的 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="54b45-143">These scenarios are inherent to offline PWAs and not specific to Blazor.</span></span> <span data-ttu-id="54b45-144">請務必閱讀並瞭解這些注意事項，然後才對啟用離線的應用程式的運作方式進行假設。</span><span class="sxs-lookup"><span data-stu-id="54b45-144">Be sure to read and understand these caveats before making assumptions about how your offline-enabled app will work.</span></span>

<span data-ttu-id="54b45-145">若要查看離線支援的運作方式：</span><span class="sxs-lookup"><span data-stu-id="54b45-145">To see how offline support works:</span></span>

1. <span data-ttu-id="54b45-146">發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="54b45-146">Publish the app.</span></span> <span data-ttu-id="54b45-147">如需詳細資訊，請參閱 <xref:host-and-deploy/blazor/index#publish-the-app> 。</span><span class="sxs-lookup"><span data-stu-id="54b45-147">For more information, see <xref:host-and-deploy/blazor/index#publish-the-app>.</span></span>
1. <span data-ttu-id="54b45-148">將應用程式部署至支援 HTTPS 的伺服器，並在瀏覽器中以安全的 HTTPS 位址存取應用程式。</span><span class="sxs-lookup"><span data-stu-id="54b45-148">Deploy the app to a server that supports HTTPS, and access the app in a browser at its secure HTTPS address.</span></span>
1. <span data-ttu-id="54b45-149">開啟瀏覽器的 [開發工具]，並確認已在 [**應用程式**] 索引標籤上註冊主機的*服務背景工作*：</span><span class="sxs-lookup"><span data-stu-id="54b45-149">Open the browser's dev tools and verify that a *Service Worker* is registered for the host on the **Application** tab:</span></span>

   ![Google Chrome 開發人員工具的 [應用程式] 索引標籤會顯示已啟動並執行的服務工作者。](progressive-web-app/_static/image4.png)

1. <span data-ttu-id="54b45-151">重載頁面，並檢查 [**網路**] 索引標籤。 [**服務工作者**] 或 [**記憶體**快取] 會列為所有頁面資產的來源：</span><span class="sxs-lookup"><span data-stu-id="54b45-151">Reload the page and examine the **Network** tab. **Service Worker** or **memory cache** are listed as the sources for all of the page's assets:</span></span>

   ![Google Chrome 開發人員工具 [網路] 索引標籤，顯示所有頁面資產的來源。](progressive-web-app/_static/image5.png)

1. <span data-ttu-id="54b45-153">若要確認瀏覽器不依賴網路存取來載入應用程式，請執行下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="54b45-153">To verify that the browser isn't dependent on network access to load the app, either:</span></span>

   * <span data-ttu-id="54b45-154">關閉網頁伺服器，並查看應用程式如何繼續正常運作，包括頁面重載。</span><span class="sxs-lookup"><span data-stu-id="54b45-154">Shut down the web server and see how the app continues to function normally, which includes page reloads.</span></span> <span data-ttu-id="54b45-155">同樣地，當網路連線緩慢時，應用程式仍會繼續正常運作。</span><span class="sxs-lookup"><span data-stu-id="54b45-155">Likewise, the app continues to function normally when there's a slow network connection.</span></span>
   * <span data-ttu-id="54b45-156">指示瀏覽器模擬 [**網路**] 索引標籤中的離線模式：</span><span class="sxs-lookup"><span data-stu-id="54b45-156">Instruct the browser to simulate offline mode in the **Network** tab:</span></span>

   ![Google Chrome 開發人員工具 [網路] 索引標籤，其中 [瀏覽器模式] 下拉狀態從 [線上] 變更為 [離線]。](progressive-web-app/_static/image6.png)

<span data-ttu-id="54b45-158">使用服務工作者的離線支援是 web 標準，而不是特有的 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="54b45-158">Offline support using a service worker is a web standard, not specific to Blazor.</span></span> <span data-ttu-id="54b45-159">如需服務工作者的詳細資訊，請參閱[MDN web 檔：服務工作者 API](https://developer.mozilla.org/docs/Web/API/Service_Worker_API)。</span><span class="sxs-lookup"><span data-stu-id="54b45-159">For more information on service workers, see [MDN web docs: Service Worker API](https://developer.mozilla.org/docs/Web/API/Service_Worker_API).</span></span> <span data-ttu-id="54b45-160">若要深入瞭解服務工作者的常見使用模式，請參閱[Google Web：服務工作者生命週期](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle)。</span><span class="sxs-lookup"><span data-stu-id="54b45-160">To learn more about common usage patterns for service workers, see [Google Web: The Service Worker Lifecycle](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle).</span></span>

Blazor<span data-ttu-id="54b45-161">的 PWA 範本會產生兩個服務工作者檔案：</span><span class="sxs-lookup"><span data-stu-id="54b45-161">'s PWA template produces two service worker files:</span></span>

* <span data-ttu-id="54b45-162">*wwwroot/service-worker*，會在開發期間使用。</span><span class="sxs-lookup"><span data-stu-id="54b45-162">*wwwroot/service-worker.js*, which is used during development.</span></span>
* <span data-ttu-id="54b45-163">*wwwroot/service-worker. 已發行的 .js*，會在發佈應用程式之後使用。</span><span class="sxs-lookup"><span data-stu-id="54b45-163">*wwwroot/service-worker.published.js*, which is used after the app is published.</span></span>

<span data-ttu-id="54b45-164">若要在兩個服務工作者檔案之間共用邏輯，請考慮下列方法：</span><span class="sxs-lookup"><span data-stu-id="54b45-164">To share logic between the two service worker files, consider the following approach:</span></span>

* <span data-ttu-id="54b45-165">新增第三個 JavaScript 檔案來保存通用邏輯。</span><span class="sxs-lookup"><span data-stu-id="54b45-165">Add a third JavaScript file to hold the common logic.</span></span>
* <span data-ttu-id="54b45-166">使用[importScripts](https://developer.mozilla.org/docs/Web/API/WorkerGlobalScope/importScripts)將通用邏輯載入兩個服務工作者檔案中。</span><span class="sxs-lookup"><span data-stu-id="54b45-166">Use [self.importScripts](https://developer.mozilla.org/docs/Web/API/WorkerGlobalScope/importScripts) to load the common logic into both service worker files.</span></span>

### <a name="cache-first-fetch-strategy"></a><span data-ttu-id="54b45-167">快取優先提取策略</span><span class="sxs-lookup"><span data-stu-id="54b45-167">Cache-first fetch strategy</span></span>

<span data-ttu-id="54b45-168">內建的*service-worker。已發行的 .js*服務工作者會使用快取*優先*策略來解析要求。</span><span class="sxs-lookup"><span data-stu-id="54b45-168">The built-in *service-worker.published.js* service worker resolves requests using a *cache-first* strategy.</span></span> <span data-ttu-id="54b45-169">這表示服務工作者偏好傳回快取的內容，不論使用者是否有網路存取權，或伺服器上是否提供較新的內容。</span><span class="sxs-lookup"><span data-stu-id="54b45-169">This means that the service worker prefers to return cached content, regardless of whether the user has network access or newer content is available on the server.</span></span>

<span data-ttu-id="54b45-170">快取優先策略非常重要，因為：</span><span class="sxs-lookup"><span data-stu-id="54b45-170">The cache-first strategy is valuable because:</span></span>

* <span data-ttu-id="54b45-171">**它可確保可靠性。**</span><span class="sxs-lookup"><span data-stu-id="54b45-171">**It ensures reliability.**</span></span> <span data-ttu-id="54b45-172">網路存取不是布林值狀態。</span><span class="sxs-lookup"><span data-stu-id="54b45-172">Network access isn't a boolean state.</span></span> <span data-ttu-id="54b45-173">使用者不只是線上或離線：</span><span class="sxs-lookup"><span data-stu-id="54b45-173">A user isn't simply online or offline:</span></span>

  * <span data-ttu-id="54b45-174">使用者的裝置可能會假設它已上線，但網路可能會很慢，而無法等待。</span><span class="sxs-lookup"><span data-stu-id="54b45-174">The user's device may assume it's online, but the network might be so slow as to be impractical to wait for.</span></span>
  * <span data-ttu-id="54b45-175">網路可能會針對特定的 Url 傳回不正確結果，例如，當有一個目前正在封鎖或重新導向特定要求的驗證 WIFI 入口網站時。</span><span class="sxs-lookup"><span data-stu-id="54b45-175">The network might return invalid results for certain URLs, such as when there's a captive WIFI portal that's currently blocking or redirecting certain requests.</span></span>
  
  <span data-ttu-id="54b45-176">這就是為什麼瀏覽器的 `navigator.onLine` API 不可靠，且不應依賴的原因。</span><span class="sxs-lookup"><span data-stu-id="54b45-176">This is why the browser's `navigator.onLine` API isn't reliable and shouldn't be depended upon.</span></span>

* <span data-ttu-id="54b45-177">**它可確保正確性。**</span><span class="sxs-lookup"><span data-stu-id="54b45-177">**It ensures correctness.**</span></span> <span data-ttu-id="54b45-178">建立離線資源的快取時，服務工作者會使用內容雜湊來確保它已在單一時刻取得完整且自我一致的資源快照集。</span><span class="sxs-lookup"><span data-stu-id="54b45-178">When building a cache of offline resources, the service worker uses content hashing to guarantee it has fetched a complete and self-consistent snapshot of resources at a single instant in time.</span></span> <span data-ttu-id="54b45-179">然後，此快取會當做不可部分完成的單位來使用。</span><span class="sxs-lookup"><span data-stu-id="54b45-179">This cache is then used as an atomic unit.</span></span> <span data-ttu-id="54b45-180">由於只需要已快取的版本，因此不會要求網路提供較新的資源。</span><span class="sxs-lookup"><span data-stu-id="54b45-180">There's no point asking the network for newer resources, since the only versions required are the ones already cached.</span></span> <span data-ttu-id="54b45-181">其他任何風險不一致和不相容（例如，嘗試使用未一起編譯的 .NET 元件版本）。</span><span class="sxs-lookup"><span data-stu-id="54b45-181">Anything else risks inconsistency and incompatibility (for example, trying to use versions of .NET assemblies that weren't compiled together).</span></span>

### <a name="background-updates"></a><span data-ttu-id="54b45-182">背景更新</span><span class="sxs-lookup"><span data-stu-id="54b45-182">Background updates</span></span>

<span data-ttu-id="54b45-183">作為心理模型，您可以將離線優先的 PWA 視為行為，就像可以安裝的行動應用程式。</span><span class="sxs-lookup"><span data-stu-id="54b45-183">As a mental model, you can think of an offline-first PWA as behaving like a mobile app that can be installed.</span></span> <span data-ttu-id="54b45-184">無論網路連線為何，應用程式都會立即啟動，但已安裝的應用程式邏輯來自可能不是最新版本的時間點快照集。</span><span class="sxs-lookup"><span data-stu-id="54b45-184">The app starts up immediately regardless of network connectivity, but the installed app logic comes from a point-in-time snapshot that might not be the latest version.</span></span>

<span data-ttu-id="54b45-185">BlazorPWA 範本會產生應用程式，以便在使用者造訪並具有正常運作的網路連線時，自動嘗試在背景中自行更新。</span><span class="sxs-lookup"><span data-stu-id="54b45-185">The Blazor PWA template produces apps that automatically try to update themselves in the background whenever the user visits and has a working network connection.</span></span> <span data-ttu-id="54b45-186">其運作方式如下：</span><span class="sxs-lookup"><span data-stu-id="54b45-186">The way this works is as follows:</span></span>

* <span data-ttu-id="54b45-187">在編譯期間，專案會產生*服務工作者資產資訊清單*。</span><span class="sxs-lookup"><span data-stu-id="54b45-187">During compilation, the project generates a *service worker assets manifest*.</span></span> <span data-ttu-id="54b45-188">根據預設，這稱為*service-worker-assets*。</span><span class="sxs-lookup"><span data-stu-id="54b45-188">By default, this is called *service-worker-assets.js*.</span></span> <span data-ttu-id="54b45-189">資訊清單會列出應用程式離線運作所需的所有靜態資源，例如 .NET 元件、JavaScript 檔案和 CSS，包括其內容雜湊。</span><span class="sxs-lookup"><span data-stu-id="54b45-189">The manifest lists all the static resources that the app requires to function offline, such as .NET assemblies, JavaScript files, and CSS, including their content hashes.</span></span> <span data-ttu-id="54b45-190">資源清單是由服務工作者載入，讓它知道要快取的資源。</span><span class="sxs-lookup"><span data-stu-id="54b45-190">The resource list is loaded by the service worker so that it knows which resources to cache.</span></span>
* <span data-ttu-id="54b45-191">每次使用者造訪應用程式時，瀏覽器會在背景重新要求*service-worker*和*service-worker-assets* 。</span><span class="sxs-lookup"><span data-stu-id="54b45-191">Each time the user visits the app, the browser re-requests *service-worker.js* and *service-worker-assets.js* in the background.</span></span> <span data-ttu-id="54b45-192">這些檔案會與現有已安裝的服務背景工作角色進行逐位元組比較。</span><span class="sxs-lookup"><span data-stu-id="54b45-192">The files are compared byte-for-byte with the existing installed service worker.</span></span> <span data-ttu-id="54b45-193">如果伺服器傳回上述任一檔案的變更內容，服務工作者會嘗試安裝新版的本身。</span><span class="sxs-lookup"><span data-stu-id="54b45-193">If the server returns changed content for either of these files, the service worker attempts to install a new version of itself.</span></span>
* <span data-ttu-id="54b45-194">安裝新版本的本身時，服務背景工作會為離線資源建立新的個別快取，並使用*service-worker-assets*中列出的資源開始擴展快取。</span><span class="sxs-lookup"><span data-stu-id="54b45-194">When installing a new version of itself, the service worker creates a new, separate cache for offline resources and starts populating the cache with resources listed in *service-worker-assets.js*.</span></span> <span data-ttu-id="54b45-195">這個邏輯會在 service-worker 內的函式中實作為 `onInstall` *已發行的 .js*。</span><span class="sxs-lookup"><span data-stu-id="54b45-195">This logic is implemented in the `onInstall` function inside *service-worker.published.js*.</span></span>
* <span data-ttu-id="54b45-196">載入所有資源時，如果沒有發生錯誤，且所有內容雜湊都相符，此程式就會順利完成。</span><span class="sxs-lookup"><span data-stu-id="54b45-196">The process completes successfully when all of the resources are loaded without error and all content hashes match.</span></span> <span data-ttu-id="54b45-197">如果成功，新的服務工作者會進入*等待啟用*狀態。</span><span class="sxs-lookup"><span data-stu-id="54b45-197">If successful, the new service worker enters a *waiting for activation* state.</span></span> <span data-ttu-id="54b45-198">一旦使用者關閉應用程式（沒有剩餘的應用程式索引標籤或 windows），新的服務背景工作角色就*會變成作用中，並*用於後續的應用程式造訪。</span><span class="sxs-lookup"><span data-stu-id="54b45-198">As soon as the user closes the app (no remaining app tabs or windows), the new service worker becomes *active* and is used for subsequent app visits.</span></span> <span data-ttu-id="54b45-199">系統會刪除舊的服務背景工作角色及其快取。</span><span class="sxs-lookup"><span data-stu-id="54b45-199">The old service worker and its cache are deleted.</span></span>
* <span data-ttu-id="54b45-200">如果進程未順利完成，則會捨棄新的服務背景工作實例。</span><span class="sxs-lookup"><span data-stu-id="54b45-200">If the process doesn't complete successfully, the new service worker instance is discarded.</span></span> <span data-ttu-id="54b45-201">使用者下次造訪時，會再次嘗試更新程式，希望用戶端有更好的網路連線可以完成要求。</span><span class="sxs-lookup"><span data-stu-id="54b45-201">The update process is attempted again on the user's next visit, when hopefully the client has a better network connection that can complete the requests.</span></span>

<span data-ttu-id="54b45-202">編輯服務背景工作邏輯以自訂此程式。</span><span class="sxs-lookup"><span data-stu-id="54b45-202">Customize this process by editing the service worker logic.</span></span> <span data-ttu-id="54b45-203">上述行為都不是特有的， Blazor 但只是 PWA 範本選項所提供的預設體驗。</span><span class="sxs-lookup"><span data-stu-id="54b45-203">None of the preceding behavior is specific to Blazor but is merely the default experience provided by the PWA template option.</span></span> <span data-ttu-id="54b45-204">如需詳細資訊，請參閱[MDN web 檔：服務工作者 API](https://developer.mozilla.org/docs/Web/API/Service_Worker_API)。</span><span class="sxs-lookup"><span data-stu-id="54b45-204">For more information, see [MDN web docs: Service Worker API](https://developer.mozilla.org/docs/Web/API/Service_Worker_API).</span></span>

### <a name="how-requests-are-resolved"></a><span data-ttu-id="54b45-205">如何解決要求</span><span class="sxs-lookup"><span data-stu-id="54b45-205">How requests are resolved</span></span>

<span data-ttu-id="54b45-206">如快[取優先提取策略](#cache-first-fetch-strategy)一節中所述，預設服務工作者會使用快取優先策略，這表示它會嘗試提供快取*的*內容（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="54b45-206">As described in the [Cache-first fetch strategy](#cache-first-fetch-strategy) section, the default service worker uses a *cache-first* strategy, meaning that it tries to serve cached content when available.</span></span> <span data-ttu-id="54b45-207">如果沒有針對特定 URL 快取任何內容（例如，從後端 API 要求資料時），服務工作者會回復一般網路要求。</span><span class="sxs-lookup"><span data-stu-id="54b45-207">If there is no content cached for a certain URL, for example when requesting data from a backend API, the service worker falls back on a regular network request.</span></span> <span data-ttu-id="54b45-208">如果可以連線到伺服器，網路要求就會成功。</span><span class="sxs-lookup"><span data-stu-id="54b45-208">The network request succeeds if the server is reachable.</span></span> <span data-ttu-id="54b45-209">這個邏輯會在 service-worker 內的函式中實作為 `onFetch` *已發行的 .js*。</span><span class="sxs-lookup"><span data-stu-id="54b45-209">This logic is implemented inside `onFetch` function within *service-worker.published.js*.</span></span>

<span data-ttu-id="54b45-210">如果應用程式的 Razor 元件依賴來自後端 api 的要求資料，而您想要為失敗的要求提供易記的使用者體驗，因為網路無法使用，請在應用程式的元件內執行邏輯。</span><span class="sxs-lookup"><span data-stu-id="54b45-210">If the app's Razor components rely on requesting data from backend APIs and you want to provide a friendly user experience for failed requests due to network unavailability, implement logic within the app's components.</span></span> <span data-ttu-id="54b45-211">例如，使用 [ `try/catch` 圍繞 <xref:System.Net.Http.HttpClient> 要求]。</span><span class="sxs-lookup"><span data-stu-id="54b45-211">For example, use `try/catch` around <xref:System.Net.Http.HttpClient> requests.</span></span>

### <a name="support-server-rendered-pages"></a><span data-ttu-id="54b45-212">支援伺服器呈現的頁面</span><span class="sxs-lookup"><span data-stu-id="54b45-212">Support server-rendered pages</span></span>

<span data-ttu-id="54b45-213">請考慮當使用者第一次流覽至 URL （例如） `/counter` 或應用程式中的任何其他深層連結時，會發生什麼情況。</span><span class="sxs-lookup"><span data-stu-id="54b45-213">Consider what happens when the user first navigates to a URL such as `/counter` or any other deep link in the app.</span></span> <span data-ttu-id="54b45-214">在這些情況下，您不想要傳回快取 `/counter` 的內容，而是需要瀏覽器載入快取的內容， `/index.html` 以啟動您的 Blazor WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="54b45-214">In these cases, you don't want to return content cached as `/counter`, but instead need the browser to load the content cached as `/index.html` to start up your Blazor WebAssembly app.</span></span> <span data-ttu-id="54b45-215">這些初始要求稱為「*導覽*要求」，而不是：</span><span class="sxs-lookup"><span data-stu-id="54b45-215">These initial requests are known as *navigation* requests, as opposed to:</span></span>

* <span data-ttu-id="54b45-216">*subresource*影像、樣式表單或其他檔案的要求。</span><span class="sxs-lookup"><span data-stu-id="54b45-216">*subresource* requests for images, stylesheets, or other files.</span></span>
* <span data-ttu-id="54b45-217">*提取/XHR* API 資料的要求。</span><span class="sxs-lookup"><span data-stu-id="54b45-217">*fetch/XHR* requests for API data.</span></span>

<span data-ttu-id="54b45-218">預設的服務背景工作包含導覽要求的特殊案例邏輯。</span><span class="sxs-lookup"><span data-stu-id="54b45-218">The default service worker contains special-case logic for navigation requests.</span></span> <span data-ttu-id="54b45-219">無論要求的 URL 為何，服務工作者都會藉由傳回的快取內容來解析要求 `/index.html` 。</span><span class="sxs-lookup"><span data-stu-id="54b45-219">The service worker resolves the requests by returning the cached content for `/index.html`, regardless of the requested URL.</span></span> <span data-ttu-id="54b45-220">這個邏輯會在 service-worker 內的函式中實作為 `onFetch` *已發行的 .js*。</span><span class="sxs-lookup"><span data-stu-id="54b45-220">This logic is implemented in the `onFetch` function inside *service-worker.published.js*.</span></span>

<span data-ttu-id="54b45-221">如果您的應用程式有特定 Url 必須傳回伺服器轉譯的 HTML，而不是從快取中提供 `/index.html` ，則您需要編輯服務背景工作中的邏輯。</span><span class="sxs-lookup"><span data-stu-id="54b45-221">If your app has certain URLs that must return server-rendered HTML, and not serve `/index.html` from the cache, then you need to edit the logic in your service worker.</span></span> <span data-ttu-id="54b45-222">如果包含的所有 Url 都必須 `/Identity/` 處理為一般僅線上的伺服器要求，則請修改*service-worker。已發行的 .js* `onFetch` 邏輯。</span><span class="sxs-lookup"><span data-stu-id="54b45-222">If all URLs containing `/Identity/` need to be handled as regular online-only requests to the server, then modify *service-worker.published.js* `onFetch` logic.</span></span> <span data-ttu-id="54b45-223">找出下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="54b45-223">Locate the following code:</span></span>

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate';
```

<span data-ttu-id="54b45-224">將程式碼變更為下列內容：</span><span class="sxs-lookup"><span data-stu-id="54b45-224">Change the code to the following:</span></span>

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate'
    && !event.request.url.includes('/Identity/');
```

<span data-ttu-id="54b45-225">如果您不這麼做，則不論網路連線為何，服務工作者都會攔截這類 Url 的要求，並使用加以解析 `/index.html` 。</span><span class="sxs-lookup"><span data-stu-id="54b45-225">If you don't do this, then regardless of network connectivity, the service worker intercepts requests for such URLs and resolves them using `/index.html`.</span></span>

### <a name="control-asset-caching"></a><span data-ttu-id="54b45-226">控制資產快取</span><span class="sxs-lookup"><span data-stu-id="54b45-226">Control asset caching</span></span>

<span data-ttu-id="54b45-227">如果您的專案定義 `ServiceWorkerAssetsManifest` MSBuild 屬性， Blazor 的組建工具會使用指定的名稱來產生服務背景工作資產資訊清單。</span><span class="sxs-lookup"><span data-stu-id="54b45-227">If your project defines the `ServiceWorkerAssetsManifest` MSBuild property, Blazor's build tooling generates a service worker assets manifest with the specified name.</span></span> <span data-ttu-id="54b45-228">預設 PWA 範本會產生包含下列屬性的專案檔：</span><span class="sxs-lookup"><span data-stu-id="54b45-228">The default PWA template produces a project file containing the following property:</span></span>

```xml
<ServiceWorkerAssetsManifest>service-worker-assets.js</ServiceWorkerAssetsManifest>
```

<span data-ttu-id="54b45-229">檔案會放在*wwwroot*輸出目錄中，因此瀏覽器可以藉由要求來抓取此檔案 `/service-worker-assets.js` 。</span><span class="sxs-lookup"><span data-stu-id="54b45-229">The file is placed in the *wwwroot* output directory, so the browser can retrieve this file by requesting `/service-worker-assets.js`.</span></span> <span data-ttu-id="54b45-230">若要查看此檔案的內容，請在文字編輯器中開啟 */BIN/DEBUG/{TARGET FRAMEWORK}/wwwroot/service-worker-assets.js* 。</span><span class="sxs-lookup"><span data-stu-id="54b45-230">To see the contents of this file, open */bin/Debug/{TARGET FRAMEWORK}/wwwroot/service-worker-assets.js* in a text editor.</span></span> <span data-ttu-id="54b45-231">不過，請不要編輯檔案，因為它會在每個組建上重新產生。</span><span class="sxs-lookup"><span data-stu-id="54b45-231">However, don't edit the file, as it's regenerated on each build.</span></span>

<span data-ttu-id="54b45-232">根據預設，此資訊清單會列出：</span><span class="sxs-lookup"><span data-stu-id="54b45-232">By default, this manifest lists:</span></span>

* <span data-ttu-id="54b45-233">任何 Blazor 管理的資源（例如 .net 元件和 .Net WebAssembly 執行時間檔案）都必須離線運作。</span><span class="sxs-lookup"><span data-stu-id="54b45-233">Any Blazor-managed resources, such as .NET assemblies and the .NET WebAssembly runtime files required to function offline.</span></span>
* <span data-ttu-id="54b45-234">所有用來發行至應用程式*wwwroot*目錄的資源，例如影像、樣式表單和 JavaScript 檔案，包括外部專案和 NuGet 套件所提供的靜態 web 資產。</span><span class="sxs-lookup"><span data-stu-id="54b45-234">All resources for publishing to the app's *wwwroot* directory, such as images, stylesheets, and JavaScript files, including static web assets supplied by external projects and NuGet packages.</span></span>

<span data-ttu-id="54b45-235">您可以藉由編輯 service-worker 中的邏輯，來控制服務工作者要提取和快取哪些 `onInstall` 資源*service-worker.published.js*。</span><span class="sxs-lookup"><span data-stu-id="54b45-235">You can control which of these resources are fetched and cached by the service worker by editing the logic in `onInstall` in *service-worker.published.js*.</span></span> <span data-ttu-id="54b45-236">根據預設，服務工作者會提取並快取符合一般 web 副檔名的檔案，例如 *.html*、 *.css*、 *.js*和*wasm*，再加上 Blazor WebAssembly （*.dll*， *.pdb*）特有的檔案類型。</span><span class="sxs-lookup"><span data-stu-id="54b45-236">By default, the service worker fetches and caches files matching typical web filename extensions such as *.html*, *.css*, *.js*, and *.wasm*, plus file types specific to Blazor WebAssembly (*.dll*, *.pdb*).</span></span>

<span data-ttu-id="54b45-237">若要加入不存在於應用程式*wwwroot*目錄中的其他資源，請定義額外 `ItemGroup` 的 MSBuild 專案，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="54b45-237">To include additional resources that aren't present in the app's *wwwroot* directory, define extra MSBuild `ItemGroup` entries, as shown in the following example:</span></span>

```xml
<ItemGroup>
  <ServiceWorkerAssetsManifestItem Include="MyDirectory\AnotherFile.json"
    RelativePath="MyDirectory\AnotherFile.json" AssetUrl="files/AnotherFile.json" />
</ItemGroup>
```

<span data-ttu-id="54b45-238">`AssetUrl`中繼資料會指定在提取要快取的資源時，瀏覽器應該使用的基底相對 URL。</span><span class="sxs-lookup"><span data-stu-id="54b45-238">The `AssetUrl` metadata specifies the base-relative URL that the browser should use when fetching the resource to cache.</span></span> <span data-ttu-id="54b45-239">這可以獨立于其在磁片上的原始來原始檔案名。</span><span class="sxs-lookup"><span data-stu-id="54b45-239">This can be independent of its original source file name on disk.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="54b45-240">新增 `ServiceWorkerAssetsManifestItem` 不會導致檔案在應用程式的*wwwroot*目錄中發行。</span><span class="sxs-lookup"><span data-stu-id="54b45-240">Adding a `ServiceWorkerAssetsManifestItem` doesn't cause the file to be published in the app's *wwwroot* directory.</span></span> <span data-ttu-id="54b45-241">發行輸出必須分開控制。</span><span class="sxs-lookup"><span data-stu-id="54b45-241">The publish output must be controlled separately.</span></span> <span data-ttu-id="54b45-242">`ServiceWorkerAssetsManifestItem`只會導致服務工作者資產資訊清單中出現額外的專案。</span><span class="sxs-lookup"><span data-stu-id="54b45-242">The `ServiceWorkerAssetsManifestItem` only causes an additional entry to appear in the service worker assets manifest.</span></span>

## <a name="push-notifications"></a><span data-ttu-id="54b45-243">推送通知</span><span class="sxs-lookup"><span data-stu-id="54b45-243">Push notifications</span></span>

<span data-ttu-id="54b45-244">就像任何其他 PWA 一樣， Blazor WebAssembly 的 pwa 也可以從後端伺服器接收推播通知。</span><span class="sxs-lookup"><span data-stu-id="54b45-244">Like any other PWA, a Blazor WebAssembly PWA can receive push notifications from a backend server.</span></span> <span data-ttu-id="54b45-245">伺服器可以隨時傳送推播通知，即使使用者未主動使用應用程式也一樣。</span><span class="sxs-lookup"><span data-stu-id="54b45-245">The server can send push notifications at any time, even when the user isn't actively using the app.</span></span> <span data-ttu-id="54b45-246">例如，當其他使用者執行相關動作時，可以傳送推播通知。</span><span class="sxs-lookup"><span data-stu-id="54b45-246">For example, push notifications can be sent when a different user performs a relevant action.</span></span>

<span data-ttu-id="54b45-247">傳送推播通知的機制完全獨立于 Blazor WebAssembly，因為它是由可使用任何技術的後端伺服器所執行。</span><span class="sxs-lookup"><span data-stu-id="54b45-247">The mechanism for sending a push notification is entirely independent of Blazor WebAssembly, since it's implemented by the backend server which can use any technology.</span></span> <span data-ttu-id="54b45-248">如果您想要從 ASP.NET Core 伺服器傳送推播通知，請考慮[使用類似于技術比薩研討會所採用之方法的技巧](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#sending-push-notifications)。</span><span class="sxs-lookup"><span data-stu-id="54b45-248">If you want to send push notifications from an ASP.NET Core server, consider [using a technique similar to the approach taken in the Blazing Pizza workshop](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#sending-push-notifications).</span></span>

<span data-ttu-id="54b45-249">在用戶端上接收和顯示推播通知的機制也獨立于 Blazor WebAssembly，因為它是在服務背景工作 JavaScript 檔案中執行。</span><span class="sxs-lookup"><span data-stu-id="54b45-249">The mechanism for receiving and displaying a push notification on the client is also independent of Blazor WebAssembly, since it's implemented in the service worker JavaScript file.</span></span> <span data-ttu-id="54b45-250">如需範例，請參閱在[進行中的比薩研討會中使用的方法](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#displaying-notifications)。</span><span class="sxs-lookup"><span data-stu-id="54b45-250">For an example, see [the approach used in the Blazing Pizza workshop](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#displaying-notifications).</span></span>

## <a name="caveats-for-offline-pwas"></a><span data-ttu-id="54b45-251">離線 Pwa 的注意事項</span><span class="sxs-lookup"><span data-stu-id="54b45-251">Caveats for offline PWAs</span></span>

<span data-ttu-id="54b45-252">並非所有應用程式都應該嘗試支援離線使用。</span><span class="sxs-lookup"><span data-stu-id="54b45-252">Not all apps should attempt to support offline use.</span></span> <span data-ttu-id="54b45-253">離線支援增加了很大的複雜性，而不一定會與所需的使用案例相關。</span><span class="sxs-lookup"><span data-stu-id="54b45-253">Offline support adds significant complexity, while not always being relevant for the use cases required.</span></span>

<span data-ttu-id="54b45-254">離線支援通常僅與相關：</span><span class="sxs-lookup"><span data-stu-id="54b45-254">Offline support is usually relevant only:</span></span>

* <span data-ttu-id="54b45-255">如果主要資料存放區是瀏覽器的本機。</span><span class="sxs-lookup"><span data-stu-id="54b45-255">If the primary data store is local to the browser.</span></span> <span data-ttu-id="54b45-256">例如，此方法與應用程式相關，其適用于[IoT](https://en.wikipedia.org/wiki/Internet_of_things)裝置的 UI，可將資料儲存在 `localStorage` 或[IndexedDB](https://developer.mozilla.org/docs/Web/API/IndexedDB_API)中。</span><span class="sxs-lookup"><span data-stu-id="54b45-256">For example, the approach is relevant in an app with a UI for an [IoT](https://en.wikipedia.org/wiki/Internet_of_things) device that stores data in `localStorage` or [IndexedDB](https://developer.mozilla.org/docs/Web/API/IndexedDB_API).</span></span>
* <span data-ttu-id="54b45-257">如果應用程式執行大量的工作來提取和快取與每個使用者相關的後端 API 資料，使其可以離線流覽資料。</span><span class="sxs-lookup"><span data-stu-id="54b45-257">If the app performs a significant amount of work to fetch and cache the backend API data relevant to each user so that they can navigate through the data offline.</span></span> <span data-ttu-id="54b45-258">如果應用程式必須支援編輯，則必須建立追蹤變更的系統，並將資料與後端同步處理。</span><span class="sxs-lookup"><span data-stu-id="54b45-258">If the app must support editing, a system for tracking changes and synchronizing data with the backend must be built.</span></span>
* <span data-ttu-id="54b45-259">如果目標是要保證應用程式會立即載入，而不考慮網路狀況。</span><span class="sxs-lookup"><span data-stu-id="54b45-259">If the goal is to guarantee that the app loads immediately regardless of network conditions.</span></span> <span data-ttu-id="54b45-260">針對後端 API 要求執行適當的使用者體驗，以顯示要求的進度，並在要求因網路無法使用而失敗時，正常運作。</span><span class="sxs-lookup"><span data-stu-id="54b45-260">Implement a suitable user experience around backend API requests to show the progress of requests and behave gracefully when requests fail due to network unavailability.</span></span>

<span data-ttu-id="54b45-261">此外，具備離線功能的 Pwa 必須處理一系列額外的複雜性。</span><span class="sxs-lookup"><span data-stu-id="54b45-261">Additionally, offline-capable PWAs must deal with a range of additional complications.</span></span> <span data-ttu-id="54b45-262">開發人員應該仔細熟悉下列各節中的注意事項。</span><span class="sxs-lookup"><span data-stu-id="54b45-262">Developers should carefully familiarize themselves with the caveats in the following sections.</span></span>

### <a name="offline-support-only-when-published"></a><span data-ttu-id="54b45-263">只有在發行時才支援離線</span><span class="sxs-lookup"><span data-stu-id="54b45-263">Offline support only when published</span></span>

<span data-ttu-id="54b45-264">在開發期間，您通常會想要查看在瀏覽器中立即反映的每項變更，而不需要進行背景更新程式。</span><span class="sxs-lookup"><span data-stu-id="54b45-264">During development you typically want to see each change reflected immediately in the browser without going through a background update process.</span></span> <span data-ttu-id="54b45-265">因此， Blazor 的 PWA 範本只有在發佈時才會啟用離線支援。</span><span class="sxs-lookup"><span data-stu-id="54b45-265">Therefore, Blazor's PWA template enables offline support only when published.</span></span>

<span data-ttu-id="54b45-266">建立具備離線功能的應用程式時，在開發環境中測試應用程式並不夠。</span><span class="sxs-lookup"><span data-stu-id="54b45-266">When building an offline-capable app, it's not enough to test the app in the Development environment.</span></span> <span data-ttu-id="54b45-267">您必須在其 [已發佈] 狀態中測試應用程式，以瞭解它如何回應不同的網路狀況。</span><span class="sxs-lookup"><span data-stu-id="54b45-267">You must test the app in its published state to understand how it responds to different network conditions.</span></span>

### <a name="update-completion-after-user-navigation-away-from-app"></a><span data-ttu-id="54b45-268">使用者流覽離開應用程式之後的更新完成</span><span class="sxs-lookup"><span data-stu-id="54b45-268">Update completion after user navigation away from app</span></span>

<span data-ttu-id="54b45-269">直到使用者在所有索引標籤中流覽完應用程式之後，更新才會完成。</span><span class="sxs-lookup"><span data-stu-id="54b45-269">Updates don't complete until the user has navigated away from the app in all tabs.</span></span> <span data-ttu-id="54b45-270">如 [[背景更新](#background-updates)] 區段中所述，在您將更新部署至應用程式之後，瀏覽器會提取已更新的服務背景工作檔案，以開始進行更新程式。</span><span class="sxs-lookup"><span data-stu-id="54b45-270">As explained in the [Background updates](#background-updates) section, after you deploy an update to the app, the browser fetches the updated service worker files to begin the update process.</span></span>

<span data-ttu-id="54b45-271">許多開發人員很驚訝，即使這項更新完成，在使用者離開所有索引標籤之前，都**不**會生效。</span><span class="sxs-lookup"><span data-stu-id="54b45-271">What surprises many developers is that, even when this update completes, it does **not** take effect until the user has navigated away in all tabs.</span></span> <span data-ttu-id="54b45-272">重新整理顯示應用程式的索引標籤並**不**足夠，即使它是顯示應用程式的唯一索引標籤也一樣。</span><span class="sxs-lookup"><span data-stu-id="54b45-272">It is **not** sufficient to refresh the tab displaying the app, even if it's the only tab displaying the app.</span></span> <span data-ttu-id="54b45-273">在您的應用程式完全關閉之前，新的服務背景工作會維持在*等候啟動*狀態。</span><span class="sxs-lookup"><span data-stu-id="54b45-273">Until your app is completely closed, the new service worker remains in the *waiting to activate* status.</span></span> <span data-ttu-id="54b45-274">**這不是特有的 Blazor ，而是標準的 web 平臺行為。**</span><span class="sxs-lookup"><span data-stu-id="54b45-274">**This is not specific to Blazor, but rather is a standard web platform behavior.**</span></span>

<span data-ttu-id="54b45-275">這通常是麻煩嘗試測試其服務工作者或離線快取資源更新的開發人員。</span><span class="sxs-lookup"><span data-stu-id="54b45-275">This commonly troubles developers who are trying to test updates to their service worker or offline cached resources.</span></span> <span data-ttu-id="54b45-276">如果您簽入瀏覽器的開發人員工具，您可能會看到類似下列的內容：</span><span class="sxs-lookup"><span data-stu-id="54b45-276">If you check in the browser's developer tools, you may see something like the following:</span></span>

![Google Chrome 的 [應用程式] 索引標籤會顯示應用程式的服務背景工作是「正在等候啟動」。](progressive-web-app/_static/image7.png)

<span data-ttu-id="54b45-278">只要「用戶端」清單（也就是顯示應用程式的索引標籤或視窗）不是空的，背景工作會繼續等待。</span><span class="sxs-lookup"><span data-stu-id="54b45-278">For as long as the list of "clients," which are tabs or windows displaying your app, is nonempty, the worker continues waiting.</span></span> <span data-ttu-id="54b45-279">服務工作者執行此動作的原因是要保證一致性。</span><span class="sxs-lookup"><span data-stu-id="54b45-279">The reason service workers do this is to guarantee consistency.</span></span> <span data-ttu-id="54b45-280">一致性表示會從相同的不可部分完成快取中提取所有資源。</span><span class="sxs-lookup"><span data-stu-id="54b45-280">Consistency means that all resources are fetched from the same atomic cache.</span></span>

<span data-ttu-id="54b45-281">測試變更時，您可能會發現按一下 [skipWaiting] 連結會很方便，如先前的螢幕擷取畫面所示，然後重載頁面。</span><span class="sxs-lookup"><span data-stu-id="54b45-281">When testing changes, you may find it convenient to click the "skipWaiting" link as shown in the preceding screenshot, then reload the page.</span></span> <span data-ttu-id="54b45-282">您可以藉由撰寫服務背景工作的程式碼來[略過「等待」階段，並在更新時立即啟用，](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#skip_the_waiting_phase)讓所有使用者都能自動化此作業。</span><span class="sxs-lookup"><span data-stu-id="54b45-282">You can automate this for all users by coding your service worker to [skip the "waiting" phase and immediately activate on update](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#skip_the_waiting_phase).</span></span> <span data-ttu-id="54b45-283">如果您略過等待階段，就會放棄保證一律會從相同的快取實例一致地提取資源。</span><span class="sxs-lookup"><span data-stu-id="54b45-283">If you skip the waiting phase, you're giving up the guarantee that resources are always fetched consistently from the same cache instance.</span></span>

### <a name="users-may-run-any-historical-version-of-the-app"></a><span data-ttu-id="54b45-284">使用者可以執行應用程式的任何歷程記錄版本</span><span class="sxs-lookup"><span data-stu-id="54b45-284">Users may run any historical version of the app</span></span>

<span data-ttu-id="54b45-285">Web 開發人員 habitually 預期使用者只會執行其 web 應用程式的最新部署版本，因為這在傳統 web 散發模型中是正常的。</span><span class="sxs-lookup"><span data-stu-id="54b45-285">Web developers habitually expect that users only run the latest deployed version of their web app, since that's normal within the traditional web distribution model.</span></span> <span data-ttu-id="54b45-286">不過，離線優先的 PWA 更類似于原生行動應用程式，使用者不一定要執行最新版本。</span><span class="sxs-lookup"><span data-stu-id="54b45-286">However, an offline-first PWA is more akin to a native mobile app, where users aren't necessarily running the latest version.</span></span>

<span data-ttu-id="54b45-287">如「[背景更新](#background-updates)」一節中所述，在您將更新部署至應用程式之後，**每個現有的使用者會繼續使用先前的版本至少一個進一步造訪**，因為更新會在背景中進行，而且在使用者之後離開時才會啟用。</span><span class="sxs-lookup"><span data-stu-id="54b45-287">As explained in the [Background updates](#background-updates) section, after you deploy an update to your app, **each existing user continues to use a previous version for at least one further visit** because the update occurs in the background and isn't activated until the user thereafter navigates away.</span></span> <span data-ttu-id="54b45-288">此外，先前使用的版本不一定是您所部署的舊版。</span><span class="sxs-lookup"><span data-stu-id="54b45-288">Plus, the previous version being used isn't necessarily the previous one you deployed.</span></span> <span data-ttu-id="54b45-289">先前的版本可以是*任何*歷程記錄版本，視使用者上次完成更新的時間而定。</span><span class="sxs-lookup"><span data-stu-id="54b45-289">The previous version can be *any* historical version, depending on when the user last completed an update.</span></span>

<span data-ttu-id="54b45-290">如果您應用程式的前端和後端部分需要 API 要求的架構相關合約，這可能會產生問題。</span><span class="sxs-lookup"><span data-stu-id="54b45-290">This can be an issue if the frontend and backend parts of your app require agreement about the schema for API requests.</span></span> <span data-ttu-id="54b45-291">您必須先確定所有使用者都已升級，才能部署回溯不相容的 API 架構變更。</span><span class="sxs-lookup"><span data-stu-id="54b45-291">You must not deploy backward-incompatible API schema changes until you can be sure that all users have upgraded.</span></span> <span data-ttu-id="54b45-292">或者，禁止使用者使用不相容的繼承應用程式。</span><span class="sxs-lookup"><span data-stu-id="54b45-292">Alternatively, block users from using incompatible older versions of the app.</span></span> <span data-ttu-id="54b45-293">此案例的需求與原生行動應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="54b45-293">This scenario requirement is the same as for native mobile apps.</span></span> <span data-ttu-id="54b45-294">如果您在伺服器 Api 中部署中斷性變更，用戶端應用程式就會中斷，供尚未更新的使用者使用。</span><span class="sxs-lookup"><span data-stu-id="54b45-294">If you deploy a breaking change in server APIs, the client app is broken for users who haven't yet updated.</span></span>

<span data-ttu-id="54b45-295">可能的話，請勿將中斷性變更部署至您的後端 Api。</span><span class="sxs-lookup"><span data-stu-id="54b45-295">If possible, don't deploy breaking changes to your backend APIs.</span></span> <span data-ttu-id="54b45-296">如果您必須這麼做，請考慮使用[標準服務背景工作角色 api （例如 ServiceWorkerRegistration](https://developer.mozilla.org/docs/Web/API/ServiceWorkerRegistration) ）來判斷應用程式是否為最新狀態，如果不是，則避免使用。</span><span class="sxs-lookup"><span data-stu-id="54b45-296">If you must do so, consider using [standard Service Worker APIs such as ServiceWorkerRegistration](https://developer.mozilla.org/docs/Web/API/ServiceWorkerRegistration) to determine whether the app is up-to-date, and if not, to prevent usage.</span></span>

### <a name="interference-with-server-rendered-pages"></a><span data-ttu-id="54b45-297">伺服器呈現頁面的干擾</span><span class="sxs-lookup"><span data-stu-id="54b45-297">Interference with server-rendered pages</span></span>

<span data-ttu-id="54b45-298">如[支援伺服器呈現的頁面](#support-server-rendered-pages)一節中所述，如果您想要略過服務工作者 `/index.html` 針對所有導覽要求傳回內容的行為，請編輯服務背景工作中的邏輯。</span><span class="sxs-lookup"><span data-stu-id="54b45-298">As described in the [Support server-rendered pages](#support-server-rendered-pages) section, if you want to bypass the service worker's behavior of returning `/index.html` contents for all navigation requests, edit the logic in your service worker.</span></span>

### <a name="all-service-worker-asset-manifest-contents-are-cached-by-default"></a><span data-ttu-id="54b45-299">預設會快取所有服務工作者資產資訊清單內容</span><span class="sxs-lookup"><span data-stu-id="54b45-299">All service worker asset manifest contents are cached by default</span></span>

<span data-ttu-id="54b45-300">如[控制資產](#control-asset-caching)快取一節中所述， *service-worker-assets*檔案會在組建期間產生，並列出服務工作者應提取和快取的所有資產。</span><span class="sxs-lookup"><span data-stu-id="54b45-300">As described in the [Control asset caching](#control-asset-caching) section, the file *service-worker-assets.js* is generated during build and lists all assets the service worker should fetch and cache.</span></span>

<span data-ttu-id="54b45-301">因為此清單預設包含所有發出至*wwwroot*的專案，包括外部封裝和專案所提供的內容，所以您必須小心不要在該處放入太多內容。</span><span class="sxs-lookup"><span data-stu-id="54b45-301">Since this list by default includes everything emitted to *wwwroot*, including content supplied by external packages and projects, you must be careful not to put too much content there.</span></span> <span data-ttu-id="54b45-302">如果*wwwroot*目錄包含數百萬個映射，服務工作者會嘗試提取並快取全部，耗用過多的頻寬，而且很可能無法順利完成。</span><span class="sxs-lookup"><span data-stu-id="54b45-302">If the *wwwroot* directory contains millions of images, the service worker tries to fetch and cache them all, consuming excessive bandwidth and most likely not completing successfully.</span></span>

<span data-ttu-id="54b45-303">藉由編輯 service-worker 中的函式，來執行任意邏輯以控制應提取和快取的資訊清單內容子集 `onInstall` 。 *service-worker.published.js*</span><span class="sxs-lookup"><span data-stu-id="54b45-303">Implement arbitrary logic to control which subset of the manifest's contents should be fetched and cached by editing the `onInstall` function in *service-worker.published.js*.</span></span>

### <a name="interaction-with-authentication"></a><span data-ttu-id="54b45-304">與驗證互動</span><span class="sxs-lookup"><span data-stu-id="54b45-304">Interaction with authentication</span></span>

<span data-ttu-id="54b45-305">您可以使用 [PWA 範本] 選項搭配驗證選項。</span><span class="sxs-lookup"><span data-stu-id="54b45-305">It's possible to use the PWA template option in conjunction with the authentication options.</span></span> <span data-ttu-id="54b45-306">具有離線功能的 PWA 也可以在使用者具有網路連線能力時支援驗證。</span><span class="sxs-lookup"><span data-stu-id="54b45-306">An offline-capable PWA can also support authentication when the user has network connectivity.</span></span>

<span data-ttu-id="54b45-307">當使用者沒有網路連線時，他們無法驗證或取得存取權杖。</span><span class="sxs-lookup"><span data-stu-id="54b45-307">When a user doesn't have network connectivity, they can't authenticate or obtain access tokens.</span></span> <span data-ttu-id="54b45-308">根據預設，嘗試在沒有網路存取的情況下流覽登入頁面會產生「網路錯誤」訊息。</span><span class="sxs-lookup"><span data-stu-id="54b45-308">By default, attempting to visit the login page without network access results in a "network error" message.</span></span>

<span data-ttu-id="54b45-309">您必須設計一個 UI 流程，讓使用者在離線時執行有用的工作，而不會嘗試驗證或取得存取權杖。</span><span class="sxs-lookup"><span data-stu-id="54b45-309">You must design a UI flow that lets the user do useful things while offline without attempting to authenticate or obtain access tokens.</span></span> <span data-ttu-id="54b45-310">或者，您可以設計應用程式在網路無法使用時，以正常方式失敗。</span><span class="sxs-lookup"><span data-stu-id="54b45-310">Alternatively, you can design the app to fail in a graceful way when the network isn't available.</span></span> <span data-ttu-id="54b45-311">如果您的應用程式中無法這麼做，您可能不會想要啟用離線支援。</span><span class="sxs-lookup"><span data-stu-id="54b45-311">If this isn't possible in your app, you might not want to enable offline support.</span></span>
