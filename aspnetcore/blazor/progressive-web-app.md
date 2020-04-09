---
title: 使用ASP.NET核心BlazorWeb 組裝建構漸進式 Web 應用程式
author: guardrex
description: 瞭解如何建構Blazor基於 漸進式 Web 應用程式 (PWA),該應用程式使用現代瀏覽器功能來像桌面應用一樣運行。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/23/2020
no-loc:
- Blazor
- SignalR
uid: blazor/progressive-web-app
ms.openlocfilehash: fe69e51aefae9c80e5bb4b78151d384ce25d41a7
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80218943"
---
# <a name="build-progressive-web-applications-with-aspnet-core-blazor-webassembly"></a><span data-ttu-id="549f6-103">使用ASP.NET核心 Blazor Web 組裝建構漸進式 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="549f6-103">Build Progressive Web Applications with ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="549f6-104">作者：[Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="549f6-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="549f6-105">漸進式 Web 應用程式 (PWA) 是單頁應用程式 (SPA),它使用現代瀏覽器 API 和功能來像桌面應用程式一樣執行。</span><span class="sxs-lookup"><span data-stu-id="549f6-105">A Progressive Web Application (PWA) is a Single Page Application (SPA) that uses modern browser APIs and capabilities to behave like a desktop app.</span></span> <span data-ttu-id="549f6-106">Blazor WebAssembly 是一個基於標準的用戶端 Web 應用平臺,因此可以使用任何瀏覽器 API,包括以下功能所需的 PWA API:</span><span class="sxs-lookup"><span data-stu-id="549f6-106">Blazor WebAssembly is a standards-based client-side web app platform, so it can use any browser API, including PWA APIs required for the following capabilities:</span></span>

* <span data-ttu-id="549f6-107">離線工作並立即載入,與網路速度無關。</span><span class="sxs-lookup"><span data-stu-id="549f6-107">Working offline and loading instantly, independent of network speed.</span></span>
* <span data-ttu-id="549f6-108">在其自己的應用視窗中運行,而不僅僅是瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="549f6-108">Running in its own app window, not just a browser window.</span></span>
* <span data-ttu-id="549f6-109">從主機的操作系統啟動功能表、銜接站或主螢幕啟動。</span><span class="sxs-lookup"><span data-stu-id="549f6-109">Being launched from the host's operating system start menu, dock, or home screen.</span></span>
* <span data-ttu-id="549f6-110">從後端伺服器接收推送通知,即使使用者不使用應用也是如此。</span><span class="sxs-lookup"><span data-stu-id="549f6-110">Receiving push notifications from a backend server, even while the user isn't using the app.</span></span>
* <span data-ttu-id="549f6-111">在後台自動更新。</span><span class="sxs-lookup"><span data-stu-id="549f6-111">Automatically updating in the background.</span></span>

<span data-ttu-id="549f6-112">"*累進"* 一詞用於描述此類應用,因為:</span><span class="sxs-lookup"><span data-stu-id="549f6-112">The word *progressive* is used to describe such apps because:</span></span>

* <span data-ttu-id="549f6-113">使用者可能首先發現並使用其 Web 瀏覽器中的應用,就像任何其他 SPA 一樣。</span><span class="sxs-lookup"><span data-stu-id="549f6-113">A user might first discover and use the app within their web browser like any other SPA.</span></span>
* <span data-ttu-id="549f6-114">稍後,用戶繼續將其安裝到其操作系統中並啟用推送通知。</span><span class="sxs-lookup"><span data-stu-id="549f6-114">Later, the user progresses to installing it in their OS and enabling push notifications.</span></span>

## <a name="create-a-project-from-the-pwa-template"></a><span data-ttu-id="549f6-115">從 PWA 樣本建立項目</span><span class="sxs-lookup"><span data-stu-id="549f6-115">Create a project from the PWA template</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="549f6-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="549f6-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="549f6-117">在 **「建立新項目**」對話框中建立新的**Blazor WebAssembly 應用程式**時,請選擇 **「進度 Web 應用程式**」 選單方塊:</span><span class="sxs-lookup"><span data-stu-id="549f6-117">When creating a new **Blazor WebAssembly App** in the **Create a New Project** dialog, select the **Progress Web Application** check box:</span></span>

!["逐步 Web 應用程式"複選框在可視化工作室新項目對話方塊中被選中。](progressive-web-app/_static/image1.png)

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

-->

# <a name="visual-studio-code--net-core-cli"></a>[<span data-ttu-id="549f6-119">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="549f6-119">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="549f6-120">使用`--pwa`交換機在指令 shell 中建立 PWA 專案:</span><span class="sxs-lookup"><span data-stu-id="549f6-120">Create a PWA project in a command shell with the `--pwa` switch:</span></span>

```dotnetcli
dotnet new blazorwasm -o MyNewProject --pwa
```

---

<span data-ttu-id="549f6-121">或者,PWA 可以配置為從 ASP.NET核心託管範本創建的應用。</span><span class="sxs-lookup"><span data-stu-id="549f6-121">Optionally, PWA can be configured for an app created from the ASP.NET Core Hosted template.</span></span> <span data-ttu-id="549f6-122">PWA 方案獨立於託管模型。</span><span class="sxs-lookup"><span data-stu-id="549f6-122">The PWA scenario is independent of the hosting model.</span></span>

## <a name="installation-and-app-manifest"></a><span data-ttu-id="549f6-123">安裝和應用程式清單</span><span class="sxs-lookup"><span data-stu-id="549f6-123">Installation and app manifest</span></span>

<span data-ttu-id="549f6-124">訪問使用 PWA 樣本建立的應用時,用戶可以選擇將應用安裝到其作業系統的啟動功能表、擴充站或主螢幕中。</span><span class="sxs-lookup"><span data-stu-id="549f6-124">When visiting an app created using the PWA template, users have the option of installing the app into their OS's start menu, dock, or home screen.</span></span> <span data-ttu-id="549f6-125">顯示此選項的方式取決於用戶的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="549f6-125">The way this option is presented depends on the user's browser.</span></span> <span data-ttu-id="549f6-126">使用基於鉻的桌面瀏覽器(如邊緣瀏覽器或 Chrome 瀏覽器)時,網址欄中將顯示 **「添加**」按鈕。</span><span class="sxs-lookup"><span data-stu-id="549f6-126">When using desktop Chromium-based browsers, such as Edge or Chrome, an **Add** button appears within the URL bar.</span></span> <span data-ttu-id="549f6-127">使用者選擇「**新增**」按鈕後,他們將收到確認對話框:</span><span class="sxs-lookup"><span data-stu-id="549f6-127">After the user selects the **Add** button, they receive a confirmation dialog:</span></span>

![Google Chrome 中的確認記錄向"MyBlazorPwa"應用的用戶顯示"安裝"按鈕。](progressive-web-app/_static/image2.png)

<span data-ttu-id="549f6-129">在 iOS 上,訪問者可以使用 Safari 的 **「共用」** 按鈕及其 **「添加到主螢幕」** 選項安裝 PWA。</span><span class="sxs-lookup"><span data-stu-id="549f6-129">On iOS, visitors can install the PWA using Safari's **Share** button and its **Add to Homescreen** option.</span></span> <span data-ttu-id="549f6-130">在 Android 的 Chrome 上,用戶應選擇右上角的 **「功能表」** 按鈕,然後選擇 **「添加到主螢幕**」。。</span><span class="sxs-lookup"><span data-stu-id="549f6-130">On Chrome for Android, users should select the **Menu** button in the upper-right corner, followed by **Add to Home screen**.</span></span>

<span data-ttu-id="549f6-131">安裝後,應用將顯示在自己的視窗中,沒有位址列:</span><span class="sxs-lookup"><span data-stu-id="549f6-131">Once installed, the app appears in its own window without an address bar:</span></span>

!["MyBlazorPwa"應用程式在谷歌Chrome中運行,沒有位址欄。](progressive-web-app/_static/image3.png)

<span data-ttu-id="549f6-133">要自定義視窗的標題、色彩配置、圖示或其他詳細資訊,請參閱專案的*wwwroot*目錄中的*清單.json*檔。</span><span class="sxs-lookup"><span data-stu-id="549f6-133">To customize the window's title, color scheme, icon, or other details, see the *manifest.json* file in the project's *wwwroot* directory.</span></span> <span data-ttu-id="549f6-134">此文件的架構由 Web 標準定義。</span><span class="sxs-lookup"><span data-stu-id="549f6-134">The schema of this file is defined by web standards.</span></span> <span data-ttu-id="549f6-135">有關詳細資訊,請參閱[MDN Web 文件:Web 應用清單](https://developer.mozilla.org/docs/Web/Manifest)。</span><span class="sxs-lookup"><span data-stu-id="549f6-135">For more information, see [MDN web docs: Web App Manifest](https://developer.mozilla.org/docs/Web/Manifest).</span></span>

## <a name="offline-support"></a><span data-ttu-id="549f6-136">離線支援</span><span class="sxs-lookup"><span data-stu-id="549f6-136">Offline support</span></span>

<span data-ttu-id="549f6-137">預設情況下,使用 PWA 樣本選項創建的應用支援離線運行。</span><span class="sxs-lookup"><span data-stu-id="549f6-137">By default, apps created using the PWA template option have support for running offline.</span></span> <span data-ttu-id="549f6-138">用戶必須首次訪問應用,而他們處於連線狀態。</span><span class="sxs-lookup"><span data-stu-id="549f6-138">A user must first visit the app while they're online.</span></span> <span data-ttu-id="549f6-139">瀏覽器會自動下載並緩存離線操作所需的所有資源。</span><span class="sxs-lookup"><span data-stu-id="549f6-139">The browser automatically downloads and caches all of the resources required to operate offline.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="549f6-140">發展支援將干擾通常的變革和測試週期。</span><span class="sxs-lookup"><span data-stu-id="549f6-140">Development support would interfere with the usual development cycle of making changes and testing them.</span></span> <span data-ttu-id="549f6-141">因此,僅為*已發佈的*應用啟用脫機支援。</span><span class="sxs-lookup"><span data-stu-id="549f6-141">Therefore, offline support is only enabled for *published* apps.</span></span> 

> [!WARNING]
> <span data-ttu-id="549f6-142">如果要分發啟用離線的 PWA,有幾個[重要的警告和注意事項](#caveats-for-offline-pwas)。</span><span class="sxs-lookup"><span data-stu-id="549f6-142">If you intend to distribute an offline-enabled PWA, there are [several important warnings and caveats](#caveats-for-offline-pwas).</span></span> <span data-ttu-id="549f6-143">這些方案是離線 PWA 固有的,不Blazor特定於 。</span><span class="sxs-lookup"><span data-stu-id="549f6-143">These scenarios are inherent to offline PWAs and not specific to Blazor.</span></span> <span data-ttu-id="549f6-144">在假設啟用脫機的應用如何工作之前,請務必閱讀並理解這些注意事項。</span><span class="sxs-lookup"><span data-stu-id="549f6-144">Be sure to read and understand these caveats before making assumptions about how your offline-enabled app will work.</span></span>

<span data-ttu-id="549f6-145">要查看離線支援的工作原理:</span><span class="sxs-lookup"><span data-stu-id="549f6-145">To see how offline support works:</span></span>

1. <span data-ttu-id="549f6-146">發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="549f6-146">Publish the app.</span></span> <span data-ttu-id="549f6-147">如需詳細資訊，請參閱 <xref:host-and-deploy/blazor/index#publish-the-app>。</span><span class="sxs-lookup"><span data-stu-id="549f6-147">For more information, see <xref:host-and-deploy/blazor/index#publish-the-app>.</span></span>
1. <span data-ttu-id="549f6-148">將應用部署到支援 HTTPS 的伺服器,並在瀏覽器中在其安全的 HTTPS 位址存取應用。</span><span class="sxs-lookup"><span data-stu-id="549f6-148">Deploy the app to a server that supports HTTPS, and access the app in a browser at its secure HTTPS address.</span></span>
1. <span data-ttu-id="549f6-149">開啟瀏覽器的開發人員工具,並驗證 **「應用程式**」選項卡上的主機是否註冊*了服務輔助角色*:</span><span class="sxs-lookup"><span data-stu-id="549f6-149">Open the browser's dev tools and verify that a *Service Worker* is registered for the host on the **Application** tab:</span></span>

   ![Google Chrome 開發者工具"應用程式"選項卡顯示服務工作人員啟動並運行。](progressive-web-app/_static/image4.png)

1. <span data-ttu-id="549f6-151">重新載入頁面並檢查 **「網路**」選項卡。**服務輔助角色**或**記憶體快取**的檔案產生式設定 :</span><span class="sxs-lookup"><span data-stu-id="549f6-151">Reload the page and examine the **Network** tab. **Service Worker** or **memory cache** are listed as the sources for all of the page's assets:</span></span>

   ![Google Chrome 開發者工具"網络"選項卡,顯示網頁所有資產的來源。](progressive-web-app/_static/image5.png)

1. <span data-ttu-id="549f6-153">要驗證瀏覽器是否依賴於網路存取來載入應用,請:</span><span class="sxs-lookup"><span data-stu-id="549f6-153">To verify that the browser isn't dependent on network access to load the app, either:</span></span>

   * <span data-ttu-id="549f6-154">關閉 Web 伺服器,查看應用如何繼續正常運行,其中包括頁面重新載入。</span><span class="sxs-lookup"><span data-stu-id="549f6-154">Shut down the web server and see how the app continues to function normally, which includes page reloads.</span></span> <span data-ttu-id="549f6-155">同樣,當網路連接速度較慢時,應用程式將繼續正常運行。</span><span class="sxs-lookup"><span data-stu-id="549f6-155">Likewise, the app continues to function normally when there's a slow network connection.</span></span>
   * <span data-ttu-id="549f6-156">指示瀏覽器在 **「網路」** 選項卡中模擬離線模式:</span><span class="sxs-lookup"><span data-stu-id="549f6-156">Instruct the browser to simulate offline mode in the **Network** tab:</span></span>

   ![Google Chrome 開發者工具"網络"選項卡,瀏覽器模式下拉即從"在線"更改為"離線"。](progressive-web-app/_static/image6.png)

<span data-ttu-id="549f6-158">使用服務輔助角色進行離線支援是 Web 標準Blazor,不特定於 。</span><span class="sxs-lookup"><span data-stu-id="549f6-158">Offline support using a service worker is a web standard, not specific to Blazor.</span></span> <span data-ttu-id="549f6-159">有關服務輔助角色的詳細資訊,請參閱[MDN Web 文件:服務輔助角色 API](https://developer.mozilla.org/docs/Web/API/Service_Worker_API)。</span><span class="sxs-lookup"><span data-stu-id="549f6-159">For more information on service workers, see [MDN web docs: Service Worker API](https://developer.mozilla.org/docs/Web/API/Service_Worker_API).</span></span> <span data-ttu-id="549f6-160">要瞭解有關服務工作人員常見使用模式的更多資訊,請參閱[Google Web:服務工作人員生命週期](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle)。</span><span class="sxs-lookup"><span data-stu-id="549f6-160">To learn more about common usage patterns for service workers, see [Google Web: The Service Worker Lifecycle](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle).</span></span>

Blazor<span data-ttu-id="549f6-161">PWA 樣本產生兩個服務輔助檔案:</span><span class="sxs-lookup"><span data-stu-id="549f6-161">'s PWA template produces two service worker files:</span></span>

* <span data-ttu-id="549f6-162">*wwwroot/服務工作者.js,* 在開發過程中使用。</span><span class="sxs-lookup"><span data-stu-id="549f6-162">*wwwroot/service-worker.js*, which is used during development.</span></span>
* <span data-ttu-id="549f6-163">*wwwroot/服務工作者.published.js,* 在發佈應用程式後使用。</span><span class="sxs-lookup"><span data-stu-id="549f6-163">*wwwroot/service-worker.published.js*, which is used after the app is published.</span></span>

<span data-ttu-id="549f6-164">要在兩個服務輔助檔案之間共用邏輯,請考慮以下方法:</span><span class="sxs-lookup"><span data-stu-id="549f6-164">To share logic between the two service worker files, consider the following approach:</span></span>

* <span data-ttu-id="549f6-165">添加第三個 JavaScript 檔案以保存通用邏輯。</span><span class="sxs-lookup"><span data-stu-id="549f6-165">Add a third JavaScript file to hold the common logic.</span></span>
* <span data-ttu-id="549f6-166">使用[self.importScripts](https://developer.mozilla.org/docs/Web/API/WorkerGlobalScope/importScripts)將通用邏輯載入到兩個服務輔助檔中。</span><span class="sxs-lookup"><span data-stu-id="549f6-166">Use [self.importScripts](https://developer.mozilla.org/docs/Web/API/WorkerGlobalScope/importScripts) to load the common logic into both service worker files.</span></span>

### <a name="cache-first-fetch-strategy"></a><span data-ttu-id="549f6-167">快取優先提取原則</span><span class="sxs-lookup"><span data-stu-id="549f6-167">Cache-first fetch strategy</span></span>

<span data-ttu-id="549f6-168">內置*服務輔助人員.published.js*服務工作線程使用*緩存優先*策略解析請求。</span><span class="sxs-lookup"><span data-stu-id="549f6-168">The built-in *service-worker.published.js* service worker resolves requests using a *cache-first* strategy.</span></span> <span data-ttu-id="549f6-169">這意味著服務工作人員更喜歡返回緩存的內容,而不管使用者是否具有網路訪問許可權或伺服器上是否有較新的內容。</span><span class="sxs-lookup"><span data-stu-id="549f6-169">This means that the service worker prefers to return cached content, regardless of whether the user has network access or newer content is available on the server.</span></span>

<span data-ttu-id="549f6-170">快取優先策略非常有價值,因為:</span><span class="sxs-lookup"><span data-stu-id="549f6-170">The cache-first strategy is valuable because:</span></span>

* <span data-ttu-id="549f6-171">**它確保可靠性。**</span><span class="sxs-lookup"><span data-stu-id="549f6-171">**It ensures reliability.**</span></span> <span data-ttu-id="549f6-172">&ndash;網路訪問不是布爾狀態。</span><span class="sxs-lookup"><span data-stu-id="549f6-172">&ndash; Network access isn't a boolean state.</span></span> <span data-ttu-id="549f6-173">使用者不只是連線或離線:</span><span class="sxs-lookup"><span data-stu-id="549f6-173">A user isn't simply online or offline:</span></span>

  * <span data-ttu-id="549f6-174">使用者的設備可能假定它是連線的,但網路可能太慢,以至於無法等待。</span><span class="sxs-lookup"><span data-stu-id="549f6-174">The user's device may assume it's online, but the network might be so slow as to be impractical to wait for.</span></span>
  * <span data-ttu-id="549f6-175">網路可能會返回某些 URL 無效的結果,例如,當前存在阻止或重定向某些請求的強制 WIFI 門戶。</span><span class="sxs-lookup"><span data-stu-id="549f6-175">The network might return invalid results for certain URLs, such as when there's a captive WIFI portal that's currently blocking or redirecting certain requests.</span></span>
  
  <span data-ttu-id="549f6-176">這就是為什麼瀏覽器的`navigator.onLine`API 不可靠,不應依賴。</span><span class="sxs-lookup"><span data-stu-id="549f6-176">This is why the browser's `navigator.onLine` API isn't reliable and shouldn't be depended upon.</span></span>

* <span data-ttu-id="549f6-177">**它確保正確性。**</span><span class="sxs-lookup"><span data-stu-id="549f6-177">**It ensures correctness.**</span></span> <span data-ttu-id="549f6-178">&ndash;構建離線資源的緩存時,服務工作人員使用內容哈希來保證它在一個瞬間獲取了完整且自我一致的資源快照。</span><span class="sxs-lookup"><span data-stu-id="549f6-178">&ndash; When building a cache of offline resources, the service worker uses content hashing to guarantee it has fetched a complete and self-consistent snapshot of resources at a single instant in time.</span></span> <span data-ttu-id="549f6-179">然後,此緩存用作原子單元。</span><span class="sxs-lookup"><span data-stu-id="549f6-179">This cache is then used as an atomic unit.</span></span> <span data-ttu-id="549f6-180">向網路詢問更新資源是沒有意義的,因為唯一需要的版本是已緩存的版本。</span><span class="sxs-lookup"><span data-stu-id="549f6-180">There's no point asking the network for newer resources, since the only versions required are the ones already cached.</span></span> <span data-ttu-id="549f6-181">任何其他內容都可能導致不一致和不相容(例如,嘗試使用未一起編譯的 .NET 程式集的版本)。</span><span class="sxs-lookup"><span data-stu-id="549f6-181">Anything else risks inconsistency and incompatibility (for example, trying to use versions of .NET assemblies that weren't compiled together).</span></span>

### <a name="background-updates"></a><span data-ttu-id="549f6-182">背景更新</span><span class="sxs-lookup"><span data-stu-id="549f6-182">Background updates</span></span>

<span data-ttu-id="549f6-183">作為一種心理模型,您可以將離線優先 PWA 視為可以安裝的移動應用。</span><span class="sxs-lookup"><span data-stu-id="549f6-183">As a mental model, you can think of an offline-first PWA as behaving like a mobile app that can be installed.</span></span> <span data-ttu-id="549f6-184">無論網路連接如何,應用都會立即啟動,但已安裝的應用邏輯來自可能不是最新版本的時間點快照。</span><span class="sxs-lookup"><span data-stu-id="549f6-184">The app starts up immediately regardless of network connectivity, but the installed app logic comes from a point-in-time snapshot that might not be the latest version.</span></span>

<span data-ttu-id="549f6-185">Blazor PWA 樣本產生的應用程式,當使用者訪問並且具有正常工作的網路連接時,這些應用會自動嘗試在後台更新自己。</span><span class="sxs-lookup"><span data-stu-id="549f6-185">The Blazor PWA template produces apps that automatically try to update themselves in the background whenever the user visits and has a working network connection.</span></span> <span data-ttu-id="549f6-186">其工作方式如下:</span><span class="sxs-lookup"><span data-stu-id="549f6-186">The way this works is as follows:</span></span>

* <span data-ttu-id="549f6-187">在編譯過程中,專案產生*服務工人資產清單*。</span><span class="sxs-lookup"><span data-stu-id="549f6-187">During compilation, the project generates a *service worker assets manifest*.</span></span> <span data-ttu-id="549f6-188">默認情況下,這稱為*服務輔助資產。*</span><span class="sxs-lookup"><span data-stu-id="549f6-188">By default, this is called *service-worker-assets.js*.</span></span> <span data-ttu-id="549f6-189">清單列出了應用離線工作所需的所有靜態資源,如 .NET 程式集、JAvaScript 檔和 CSS,包括其內容哈希。</span><span class="sxs-lookup"><span data-stu-id="549f6-189">The manifest lists all the static resources that the app requires to function offline, such as .NET assemblies, JavaScript files, and CSS, including their content hashes.</span></span> <span data-ttu-id="549f6-190">服務工作人員載入資源清單,以便知道要緩存的資源。</span><span class="sxs-lookup"><span data-stu-id="549f6-190">The resource list is loaded by the service worker so that it knows which resources to cache.</span></span>
* <span data-ttu-id="549f6-191">每次使用者訪問應用時,瀏覽器都會在後台重新請求*服務輔助人員.js* *和服務輔助資產.js。*</span><span class="sxs-lookup"><span data-stu-id="549f6-191">Each time the user visits the app, the browser re-requests *service-worker.js* and *service-worker-assets.js* in the background.</span></span> <span data-ttu-id="549f6-192">將檔按位元組與現有已安裝的服務輔助角色進行比較。</span><span class="sxs-lookup"><span data-stu-id="549f6-192">The files are compared byte-for-byte with the existing installed service worker.</span></span> <span data-ttu-id="549f6-193">如果伺服器返回其中任一檔更改的內容,則服務輔助角色將嘗試安裝其自身的新版本。</span><span class="sxs-lookup"><span data-stu-id="549f6-193">If the server returns changed content for either of these files, the service worker attempts to install a new version of itself.</span></span>
* <span data-ttu-id="549f6-194">安裝其自身的新版本時,服務輔助角色會為脫機資源創建新的單獨緩存,並開始將緩存與*服務輔助資源*中列出的資源填充。</span><span class="sxs-lookup"><span data-stu-id="549f6-194">When installing a new version of itself, the service worker creates a new, separate cache for offline resources and starts populating the cache with resources listed in *service-worker-assets.js*.</span></span> <span data-ttu-id="549f6-195">此邏輯在`onInstall`*服務輔助人員*內部的函數中實現。</span><span class="sxs-lookup"><span data-stu-id="549f6-195">This logic is implemented in the `onInstall` function inside *service-worker.published.js*.</span></span>
* <span data-ttu-id="549f6-196">當載入所有資源時,該過程將成功完成,並且所有內容哈希匹配。</span><span class="sxs-lookup"><span data-stu-id="549f6-196">The process completes successfully when all of the resources are loaded without error and all content hashes match.</span></span> <span data-ttu-id="549f6-197">如果成功,新服務工作人員將進入*等待激活*狀態。</span><span class="sxs-lookup"><span data-stu-id="549f6-197">If successful, the new service worker enters a *waiting for activation* state.</span></span> <span data-ttu-id="549f6-198">使用者關閉應用(沒有剩餘的應用選項卡或視窗)后,新服務輔助角色將*變為活動*狀態,並用於後續應用訪問。</span><span class="sxs-lookup"><span data-stu-id="549f6-198">As soon as the user closes the app (no remaining app tabs or windows), the new service worker becomes *active* and is used for subsequent app visits.</span></span> <span data-ttu-id="549f6-199">將刪除舊服務輔助角色及其緩存。</span><span class="sxs-lookup"><span data-stu-id="549f6-199">The old service worker and its cache are deleted.</span></span>
* <span data-ttu-id="549f6-200">如果行程未成功完成,則丟棄新的服務輔助角色實例。</span><span class="sxs-lookup"><span data-stu-id="549f6-200">If the process doesn't complete successfully, the new service worker instance is discarded.</span></span> <span data-ttu-id="549f6-201">在使用者下次訪問時,如果希望用戶端具有更好的網路連接,可以完成請求,則再次嘗試更新過程。</span><span class="sxs-lookup"><span data-stu-id="549f6-201">The update process is attempted again on the user's next visit, when hopefully the client has a better network connection that can complete the requests.</span></span>

<span data-ttu-id="549f6-202">通過編輯服務輔助角色邏輯自定義此過程。</span><span class="sxs-lookup"><span data-stu-id="549f6-202">Customize this process by editing the service worker logic.</span></span> <span data-ttu-id="549f6-203">上述行為都不是特定於的,Blazor而只是 PWA 樣本選項提供的默認體驗。</span><span class="sxs-lookup"><span data-stu-id="549f6-203">None of the preceding behavior is specific to Blazor but is merely the default experience provided by the PWA template option.</span></span> <span data-ttu-id="549f6-204">有關詳細資訊,請參閱[MDN Web 文件:服務輔助角色 API](https://developer.mozilla.org/docs/Web/API/Service_Worker_API)。</span><span class="sxs-lookup"><span data-stu-id="549f6-204">For more information, see [MDN web docs: Service Worker API](https://developer.mozilla.org/docs/Web/API/Service_Worker_API).</span></span>

### <a name="how-requests-are-resolved"></a><span data-ttu-id="549f6-205">如何解決要求</span><span class="sxs-lookup"><span data-stu-id="549f6-205">How requests are resolved</span></span>

<span data-ttu-id="549f6-206">如[快取優先提取策略](#cache-first-fetch-strategy)部分所述,預設服務輔助角色使用*快取優先*策略,這意味著它嘗試在可用時提供緩存的內容。</span><span class="sxs-lookup"><span data-stu-id="549f6-206">As described in the [Cache-first fetch strategy](#cache-first-fetch-strategy) section, the default service worker uses a *cache-first* strategy, meaning that it tries to serve cached content when available.</span></span> <span data-ttu-id="549f6-207">如果特定 URL 沒有快取的內容(例如,從後端 API 請求數據時)服務輔助角色會返回常規網路請求。</span><span class="sxs-lookup"><span data-stu-id="549f6-207">If there is no content cached for a certain URL, for example when requesting data from a backend API, the service worker falls back on a regular network request.</span></span> <span data-ttu-id="549f6-208">如果伺服器可以訪問,則網路請求將成功。</span><span class="sxs-lookup"><span data-stu-id="549f6-208">The network request succeeds if the server is reachable.</span></span> <span data-ttu-id="549f6-209">此邏輯在`onFetch`*服務輔助角色*內部實現。</span><span class="sxs-lookup"><span data-stu-id="549f6-209">This logic is implemented inside `onFetch` function within *service-worker.published.js*.</span></span>

<span data-ttu-id="549f6-210">如果應用的 Razor 元件依賴於從後端 API 請求數據,並且希望為由於網路不可用而導致的失敗請求提供友好的使用者體驗,請在應用的元件中實現邏輯。</span><span class="sxs-lookup"><span data-stu-id="549f6-210">If the app's Razor components rely on requesting data from backend APIs and you want to provide a friendly user experience for failed requests due to network unavailability, implement logic within the app's components.</span></span> <span data-ttu-id="549f6-211">例如,圍繞`HttpClient`請求`try/catch`使用。</span><span class="sxs-lookup"><span data-stu-id="549f6-211">For example, use `try/catch` around `HttpClient` requests.</span></span>

### <a name="support-server-rendered-pages"></a><span data-ttu-id="549f6-212">支援伺服器呈現的頁面</span><span class="sxs-lookup"><span data-stu-id="549f6-212">Support server-rendered pages</span></span>

<span data-ttu-id="549f6-213">考慮當使用者首次導航到 URL(如應用中的任何其他深層連結`/counter`) 時會發生什麼情況。</span><span class="sxs-lookup"><span data-stu-id="549f6-213">Consider what happens when the user first navigates to a URL such as `/counter` or any other deep link in the app.</span></span> <span data-ttu-id="549f6-214">在這些情況下,您不希望返回緩存為`/counter`的內容,而是需要瀏覽器載入緩存的內容`/index.html`以Blazor啟動 WebAssembly 應用。</span><span class="sxs-lookup"><span data-stu-id="549f6-214">In these cases, you don't want to return content cached as `/counter`, but instead need the browser to load the content cached as `/index.html` to start up your Blazor WebAssembly app.</span></span> <span data-ttu-id="549f6-215">這些初始請求稱為*導航*請求,而不是:</span><span class="sxs-lookup"><span data-stu-id="549f6-215">These initial requests are known as *navigation* requests, as opposed to:</span></span>

* <span data-ttu-id="549f6-216">對圖像、樣式表或其他檔的*子資源*請求。</span><span class="sxs-lookup"><span data-stu-id="549f6-216">*subresource* requests for images, stylesheets, or other files.</span></span>
* <span data-ttu-id="549f6-217">*獲取/XHR* API 資料請求。</span><span class="sxs-lookup"><span data-stu-id="549f6-217">*fetch/XHR* requests for API data.</span></span>

<span data-ttu-id="549f6-218">默認服務輔助角色包含導航請求的特殊情況邏輯。</span><span class="sxs-lookup"><span data-stu-id="549f6-218">The default service worker contains special-case logic for navigation requests.</span></span> <span data-ttu-id="549f6-219">服務工作人員通過返回的`/index.html`緩存內容來解決請求,而不考慮請求的 URL。</span><span class="sxs-lookup"><span data-stu-id="549f6-219">The service worker resolves the requests by returning the cached content for `/index.html`, regardless of the requested URL.</span></span> <span data-ttu-id="549f6-220">此邏輯在`onFetch`*服務輔助人員*內部的函數中實現。</span><span class="sxs-lookup"><span data-stu-id="549f6-220">This logic is implemented in the `onFetch` function inside *service-worker.published.js*.</span></span>

<span data-ttu-id="549f6-221">如果應用的某些 URL 必須返回伺服器呈現的 HTML,而不是從快取`/index.html`中提供服務 ,則需要編輯服務輔助角色中的邏輯。</span><span class="sxs-lookup"><span data-stu-id="549f6-221">If your app has certain URLs that must return server-rendered HTML, and not serve `/index.html` from the cache, then you need to edit the logic in your service worker.</span></span> <span data-ttu-id="549f6-222">如果`/Identity/`包含的所有 URL 都需要作為定期僅連線請求處理到伺服器,則修改*服務輔助人員.published.js*`onFetch`邏輯。</span><span class="sxs-lookup"><span data-stu-id="549f6-222">If all URLs containing `/Identity/` need to be handled as regular online-only requests to the server, then modify *service-worker.published.js* `onFetch` logic.</span></span> <span data-ttu-id="549f6-223">找出下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="549f6-223">Locate the following code:</span></span>

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate';
```

<span data-ttu-id="549f6-224">將代碼變更為以下內容:</span><span class="sxs-lookup"><span data-stu-id="549f6-224">Change the code to the following:</span></span>

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate'
    && !event.request.url.includes('/Identity/');
```

<span data-ttu-id="549f6-225">如果不執行此操作,則無論網路連接如何,服務工作人員都會攔截此類 URL 的請求,並使用`/index.html`解析這些 URL 的請求。</span><span class="sxs-lookup"><span data-stu-id="549f6-225">If you don't do this, then regardless of network connectivity, the service worker intercepts requests for such URLs and resolves them using `/index.html`.</span></span>

### <a name="control-asset-caching"></a><span data-ttu-id="549f6-226">控制資產快取</span><span class="sxs-lookup"><span data-stu-id="549f6-226">Control asset caching</span></span>

<span data-ttu-id="549f6-227">如果專案定義了`ServiceWorkerAssetsManifest`MSBuildBlazor屬性, 則生成工具將生成具有指定名稱的服務輔助角色資產清單。</span><span class="sxs-lookup"><span data-stu-id="549f6-227">If your project defines the `ServiceWorkerAssetsManifest` MSBuild property, Blazor's build tooling generates a service worker assets manifest with the specified name.</span></span> <span data-ttu-id="549f6-228">預設 PWA 樣本產生包含以下屬性的項目檔:</span><span class="sxs-lookup"><span data-stu-id="549f6-228">The default PWA template produces a project file containing the following property:</span></span>

```xml
<ServiceWorkerAssetsManifest>service-worker-assets.js</ServiceWorkerAssetsManifest>
```

<span data-ttu-id="549f6-229">該檔被放置在*wwwroot*輸出目錄中,因此瀏覽器`/service-worker-assets.js`可以通過請求 來檢索此檔。</span><span class="sxs-lookup"><span data-stu-id="549f6-229">The file is placed in the *wwwroot* output directory, so the browser can retrieve this file by requesting `/service-worker-assets.js`.</span></span> <span data-ttu-id="549f6-230">要查看此文件的內容,請打開 */bin/除錯/[目標框架]/wwwroot/服務-輔助資源.js*在文字編輯器中。</span><span class="sxs-lookup"><span data-stu-id="549f6-230">To see the contents of this file, open */bin/Debug/{TARGET FRAMEWORK}/wwwroot/service-worker-assets.js* in a text editor.</span></span> <span data-ttu-id="549f6-231">但是,不要編輯文件,因為它在每個生成上重新生成。</span><span class="sxs-lookup"><span data-stu-id="549f6-231">However, don't edit the file, as it's regenerated on each build.</span></span>

<span data-ttu-id="549f6-232">預設情況下,此清單列出:</span><span class="sxs-lookup"><span data-stu-id="549f6-232">By default, this manifest lists:</span></span>

* <span data-ttu-id="549f6-233">任何Blazor託管資源,如 .NET 程式集和 .NET WebAssembly 運行時檔,都需要離線運行。</span><span class="sxs-lookup"><span data-stu-id="549f6-233">Any Blazor-managed resources, such as .NET assemblies and the .NET WebAssembly runtime files required to function offline.</span></span>
* <span data-ttu-id="549f6-234">用於發佈到應用*的 wwwroot*目錄的所有資源,如影像、樣式表和 JAvaScript 檔,包括外部專案和 NuGet 套件提供的靜態 Web 資源。</span><span class="sxs-lookup"><span data-stu-id="549f6-234">All resources for publishing to the app's *wwwroot* directory, such as images, stylesheets, and JavaScript files, including static web assets supplied by external projects and NuGet packages.</span></span>

<span data-ttu-id="549f6-235">您可以通過`onInstall`在*服務輔助角色*中編輯邏輯來控制服務工作人員提取和緩存這些資源中的哪些。</span><span class="sxs-lookup"><span data-stu-id="549f6-235">You can control which of these resources are fetched and cached by the service worker by editing the logic in `onInstall` in *service-worker.published.js*.</span></span> <span data-ttu-id="549f6-236">預設情況下,服務輔助角色獲取和緩存與典型 Web 檔Blazor名副檔名(如 .html、.css、.js 和 *.wasm)* 匹配的檔,以及特定於 *.js**.css**.html*WebAssembly 的檔案類型 *(.dll.pdb)。* *.pdb*</span><span class="sxs-lookup"><span data-stu-id="549f6-236">By default, the service worker fetches and caches files matching typical web filename extensions such as *.html*, *.css*, *.js*, and *.wasm*, plus file types specific to Blazor WebAssembly (*.dll*, *.pdb*).</span></span>

<span data-ttu-id="549f6-237">要包括應用*的 wwwroot*目錄中不存在的其他資源,請定義額外的`ItemGroup`MSBuild 條目,如以下範例所示:</span><span class="sxs-lookup"><span data-stu-id="549f6-237">To include additional resources that aren't present in the app's *wwwroot* directory, define extra MSBuild `ItemGroup` entries, as shown in the following example:</span></span>

```xml
<ItemGroup>
  <ServiceWorkerAssetsManifestItem Include="MyDirectory\AnotherFile.json"
    RelativePath="MyDirectory\AnotherFile.json" AssetUrl="files/AnotherFile.json" />
</ItemGroup>
```

<span data-ttu-id="549f6-238">中繼`AssetUrl`資料指定瀏覽器在提取要快取的資源時應使用的與基礎相關的網址。</span><span class="sxs-lookup"><span data-stu-id="549f6-238">The `AssetUrl` metadata specifies the base-relative URL that the browser should use when fetching the resource to cache.</span></span> <span data-ttu-id="549f6-239">這可以獨立於磁碟上的原始源檔名。</span><span class="sxs-lookup"><span data-stu-id="549f6-239">This can be independent of its original source file name on disk.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="549f6-240">添加`ServiceWorkerAssetsManifestItem`不會導致檔在應用的*wwwroot*目錄中發布。</span><span class="sxs-lookup"><span data-stu-id="549f6-240">Adding a `ServiceWorkerAssetsManifestItem` doesn't cause the file to be published in the app's *wwwroot* directory.</span></span> <span data-ttu-id="549f6-241">發佈輸出必須單獨控制。</span><span class="sxs-lookup"><span data-stu-id="549f6-241">The publish output must be controlled separately.</span></span> <span data-ttu-id="549f6-242">唯`ServiceWorkerAssetsManifestItem`一會導致服務輔助角色資產清單中顯示的其他條目。</span><span class="sxs-lookup"><span data-stu-id="549f6-242">The `ServiceWorkerAssetsManifestItem` only causes an additional entry to appear in the service worker assets manifest.</span></span>

## <a name="push-notifications"></a><span data-ttu-id="549f6-243">推播通知</span><span class="sxs-lookup"><span data-stu-id="549f6-243">Push notifications</span></span>

<span data-ttu-id="549f6-244">與任何其他 PWABlazor一樣,WebAssembly PWA 可以從後端伺服器接收推送通知。</span><span class="sxs-lookup"><span data-stu-id="549f6-244">Like any other PWA, a Blazor WebAssembly PWA can receive push notifications from a backend server.</span></span> <span data-ttu-id="549f6-245">即使使用者未主動使用應用,伺服器也可以隨時發送推送通知。</span><span class="sxs-lookup"><span data-stu-id="549f6-245">The server can send push notifications at any time, even when the user isn't actively using the app.</span></span> <span data-ttu-id="549f6-246">例如,當其他使用者執行相關操作時,可以發送推送通知。</span><span class="sxs-lookup"><span data-stu-id="549f6-246">For example, push notifications can be sent when a different user performs a relevant action.</span></span>

<span data-ttu-id="549f6-247">發送推送通知的機制完全獨立於BlazorWebAssembly,因為它由可以使用任何技術的後端伺服器實現。</span><span class="sxs-lookup"><span data-stu-id="549f6-247">The mechanism for sending a push notification is entirely independent of Blazor WebAssembly, since it's implemented by the backend server which can use any technology.</span></span> <span data-ttu-id="549f6-248">如果要從ASP.NET核心伺服器傳送推送通知,請考慮[使用類似於在燃極比薩車間中採用的方法的技術](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#sending-push-notifications)。</span><span class="sxs-lookup"><span data-stu-id="549f6-248">If you want to send push notifications from an ASP.NET Core server, consider [using a technique similar to the approach taken in the Blazing Pizza workshop](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#sending-push-notifications).</span></span>

<span data-ttu-id="549f6-249">在用戶端上接收和顯示推送通知的機制也獨立於BlazorWebAssembly,因為它在服務輔助元件 JavaScript 檔中實現。</span><span class="sxs-lookup"><span data-stu-id="549f6-249">The mechanism for receiving and displaying a push notification on the client is also independent of Blazor WebAssembly, since it's implemented in the service worker JavaScript file.</span></span> <span data-ttu-id="549f6-250">例如,請參閱[「燃燒比薩」車間中使用的方法](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#displaying-notifications)。</span><span class="sxs-lookup"><span data-stu-id="549f6-250">For an example, see [the approach used in the Blazing Pizza workshop](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#displaying-notifications).</span></span>

## <a name="caveats-for-offline-pwas"></a><span data-ttu-id="549f6-251">離線 PWA 的注意事項</span><span class="sxs-lookup"><span data-stu-id="549f6-251">Caveats for offline PWAs</span></span>

<span data-ttu-id="549f6-252">並非所有應用都應嘗試支援脫機使用。</span><span class="sxs-lookup"><span data-stu-id="549f6-252">Not all apps should attempt to support offline use.</span></span> <span data-ttu-id="549f6-253">脫機支援增加了顯著的複雜性,但並不總是與所需的用例相關。</span><span class="sxs-lookup"><span data-stu-id="549f6-253">Offline support adds significant complexity, while not always being relevant for the use cases required.</span></span>

<span data-ttu-id="549f6-254">離線支援通常僅相關:</span><span class="sxs-lookup"><span data-stu-id="549f6-254">Offline support is usually relevant only:</span></span>

* <span data-ttu-id="549f6-255">如果主資料儲存是瀏覽器的本地。</span><span class="sxs-lookup"><span data-stu-id="549f6-255">If the primary data store is local to the browser.</span></span> <span data-ttu-id="549f6-256">例如,該方法在具有 UI 的應用中相關,該應用用於在或索引`localStorage`[DB](https://developer.mozilla.org/docs/Web/API/IndexedDB_API)中儲存數據的[IoT](https://en.wikipedia.org/wiki/Internet_of_things)設備。</span><span class="sxs-lookup"><span data-stu-id="549f6-256">For example, the approach is relevant in an app with a UI for an [IoT](https://en.wikipedia.org/wiki/Internet_of_things) device that stores data in `localStorage` or [IndexedDB](https://developer.mozilla.org/docs/Web/API/IndexedDB_API).</span></span>
* <span data-ttu-id="549f6-257">如果應用執行大量工作來提取和緩存與每個用戶相關的後端 API 數據,以便他們可以離線流覽數據。</span><span class="sxs-lookup"><span data-stu-id="549f6-257">If the app performs a significant amount of work to fetch and cache the backend API data relevant to each user so that they can navigate through the data offline.</span></span> <span data-ttu-id="549f6-258">如果應用必須支援編輯,則必須構建用於跟蹤更改和將資料與後端同步的系統。</span><span class="sxs-lookup"><span data-stu-id="549f6-258">If the app must support editing, a system for tracking changes and synchronizing data with the backend must be built.</span></span>
* <span data-ttu-id="549f6-259">如果目標是保證應用立即載入,而不考慮網路條件。</span><span class="sxs-lookup"><span data-stu-id="549f6-259">If the goal is to guarantee that the app loads immediately regardless of network conditions.</span></span> <span data-ttu-id="549f6-260">圍繞後端 API 請求實現適當的使用者體驗,以顯示請求的進度,並在請求因網路不可用而失敗時正常執行。</span><span class="sxs-lookup"><span data-stu-id="549f6-260">Implement a suitable user experience around backend API requests to show the progress of requests and behave gracefully when requests fail due to network unavailability.</span></span>

<span data-ttu-id="549f6-261">此外,支援離線的 PWA 必須處理一系列額外的併發症。</span><span class="sxs-lookup"><span data-stu-id="549f6-261">Additionally, offline-capable PWAs must deal with a range of additional complications.</span></span> <span data-ttu-id="549f6-262">開發人員應仔細熟悉以下各節中的注意事項。</span><span class="sxs-lookup"><span data-stu-id="549f6-262">Developers should carefully familiarize themselves with the caveats in the following sections.</span></span>

### <a name="offline-support-only-when-published"></a><span data-ttu-id="549f6-263">僅在發佈時提供離線支援</span><span class="sxs-lookup"><span data-stu-id="549f6-263">Offline support only when published</span></span>

<span data-ttu-id="549f6-264">在開發過程中,您通常希望看到每個更改立即反映在瀏覽器中,而無需經歷後台更新過程。</span><span class="sxs-lookup"><span data-stu-id="549f6-264">During development you typically want to see each change reflected immediately in the browser without going through a background update process.</span></span> <span data-ttu-id="549f6-265">因此,PWABlazor範本僅在發佈時啟用脫機支援。</span><span class="sxs-lookup"><span data-stu-id="549f6-265">Therefore, Blazor's PWA template enables offline support only when published.</span></span>

<span data-ttu-id="549f6-266">構建支援離線的應用時,在開發環境中測試應用是不夠的。</span><span class="sxs-lookup"><span data-stu-id="549f6-266">When building an offline-capable app, it's not enough to test the app in the Development environment.</span></span> <span data-ttu-id="549f6-267">您必須以已發佈狀態測試應用,以瞭解它如何回應不同的網路條件。</span><span class="sxs-lookup"><span data-stu-id="549f6-267">You must test the app in its published state to understand how it responds to different network conditions.</span></span>

### <a name="update-completion-after-user-navigation-away-from-app"></a><span data-ttu-id="549f6-268">使用者導覽離開應用程式後更新完成</span><span class="sxs-lookup"><span data-stu-id="549f6-268">Update completion after user navigation away from app</span></span>

<span data-ttu-id="549f6-269">在使用者從所有選項卡中導航遠離應用之前,更新不會完成。</span><span class="sxs-lookup"><span data-stu-id="549f6-269">Updates don't complete until the user has navigated away from the app in all tabs.</span></span> <span data-ttu-id="549f6-270">如["後台更新](#background-updates)"部分所述,在將更新部署到應用後,瀏覽器將提取更新的服務輔助檔以開始更新過程。</span><span class="sxs-lookup"><span data-stu-id="549f6-270">As explained in the [Background updates](#background-updates) section, after you deploy an update to the app, the browser fetches the updated service worker files to begin the update process.</span></span>

<span data-ttu-id="549f6-271">令許多開發人員驚訝的是,即使此更新完成,它**也不會**生效,直到使用者在所有選項卡中導航離開。</span><span class="sxs-lookup"><span data-stu-id="549f6-271">What surprises many developers is that, even when this update completes, it does **not** take effect until the user has navigated away in all tabs.</span></span> <span data-ttu-id="549f6-272">刷新顯示應用的選項卡**是不夠的**,即使它是唯一顯示應用的選項卡。</span><span class="sxs-lookup"><span data-stu-id="549f6-272">It is **not** sufficient to refresh the tab displaying the app, even if it's the only tab displaying the app.</span></span> <span data-ttu-id="549f6-273">在應用完全關閉之前,新的服務工作人員將保持*等待激活*狀態。</span><span class="sxs-lookup"><span data-stu-id="549f6-273">Until your app is completely closed, the new service worker remains in the *waiting to activate* status.</span></span> <span data-ttu-id="549f6-274">**這不是特定於的Blazor,而是標準 Web 平台行為。**</span><span class="sxs-lookup"><span data-stu-id="549f6-274">**This is not specific to Blazor, but rather is a standard web platform behavior.**</span></span>

<span data-ttu-id="549f6-275">這通常會困擾嘗試測試其服務輔助角色或脫機緩存資源的更新的開發人員。</span><span class="sxs-lookup"><span data-stu-id="549f6-275">This commonly troubles developers who are trying to test updates to their service worker or offline cached resources.</span></span> <span data-ttu-id="549f6-276">如果簽入瀏覽器的開發人員工具,您可能會看到以下內容:</span><span class="sxs-lookup"><span data-stu-id="549f6-276">If you check in the browser's developer tools, you may see something like the following:</span></span>

![Google Chrome"應用程式"選項卡顯示,應用的服務工作人員正在"等待啟動"。](progressive-web-app/_static/image7.png)

<span data-ttu-id="549f6-278">只要顯示應用的選項卡或視窗的「用戶端」清單為非空,工作人員將繼續等待。</span><span class="sxs-lookup"><span data-stu-id="549f6-278">For as long as the list of "clients," which are tabs or windows displaying your app, is nonempty, the worker continues waiting.</span></span> <span data-ttu-id="549f6-279">服務人員這樣做的原因是為了保證一致性。</span><span class="sxs-lookup"><span data-stu-id="549f6-279">The reason service workers do this is to guarantee consistency.</span></span> <span data-ttu-id="549f6-280">一致性意味著所有資源都從同一原子緩存獲取。</span><span class="sxs-lookup"><span data-stu-id="549f6-280">Consistency means that all resources are fetched from the same atomic cache.</span></span>

<span data-ttu-id="549f6-281">測試更改時,您可能會發現按一下「跳過等待」連結(如上圖所示)以方便,然後重新載入頁面。</span><span class="sxs-lookup"><span data-stu-id="549f6-281">When testing changes, you may find it convenient to click the "skipWaiting" link as shown in the preceding screenshot, then reload the page.</span></span> <span data-ttu-id="549f6-282">您可以通過編碼服務工作人員跳過[「等待」階段並立即在更新時啟動](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#skip_the_waiting_phase),為所有使用者自動執行此功能。</span><span class="sxs-lookup"><span data-stu-id="549f6-282">You can automate this for all users by coding your service worker to [skip the "waiting" phase and immediately activate on update](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#skip_the_waiting_phase).</span></span> <span data-ttu-id="549f6-283">如果跳過等待階段,則放棄始終從同一緩存實例獲取資源的保證。</span><span class="sxs-lookup"><span data-stu-id="549f6-283">If you skip the waiting phase, you're giving up the guarantee that resources are always fetched consistently from the same cache instance.</span></span>

### <a name="users-may-run-any-historical-version-of-the-app"></a><span data-ttu-id="549f6-284">使用者可以執行應用程式的任何歷史版本</span><span class="sxs-lookup"><span data-stu-id="549f6-284">Users may run any historical version of the app</span></span>

<span data-ttu-id="549f6-285">Web 開發人員習慣性地希望使用者只運行其 Web 應用的最新部署版本,因為這是傳統 Web 分發模型中的常態。</span><span class="sxs-lookup"><span data-stu-id="549f6-285">Web developers habitually expect that users only run the latest deployed version of their web app, since that's normal within the traditional web distribution model.</span></span> <span data-ttu-id="549f6-286">但是,離線優先 PWA 更類似於本機行動應用,使用者不一定運行最新版本。</span><span class="sxs-lookup"><span data-stu-id="549f6-286">However, an offline-first PWA is more akin to a native mobile app, where users aren't necessarily running the latest version.</span></span>

<span data-ttu-id="549f6-287">如[「背景更新」](#background-updates)部分所述,在將更新部署到應用後,**每個現有使用者將繼續使用以前的版本進行至少一次進一次訪問**,因為更新發生在後台,並且在使用者之後導航離開之前不會啟動。</span><span class="sxs-lookup"><span data-stu-id="549f6-287">As explained in the [Background updates](#background-updates) section, after you deploy an update to your app, **each existing user continues to use a previous version for at least one further visit** because the update occurs in the background and isn't activated until the user thereafter navigates away.</span></span> <span data-ttu-id="549f6-288">此外,正在使用的早期版本不一定是您部署的前一個版本。</span><span class="sxs-lookup"><span data-stu-id="549f6-288">Plus, the previous version being used isn't necessarily the previous one you deployed.</span></span> <span data-ttu-id="549f6-289">以前的版本可以是*任何*歷史版本,具體取決於使用者上次完成更新的次。</span><span class="sxs-lookup"><span data-stu-id="549f6-289">The previous version can be *any* historical version, depending on when the user last completed an update.</span></span>

<span data-ttu-id="549f6-290">如果應用的前端和後端部分需要就 API 請求的架構達成一致,則這可能是一個問題。</span><span class="sxs-lookup"><span data-stu-id="549f6-290">This can be an issue if the frontend and backend parts of your app require agreement about the schema for API requests.</span></span> <span data-ttu-id="549f6-291">在確保所有使用者都已升級之前,不得部署向後不相容的 API 架構更改。</span><span class="sxs-lookup"><span data-stu-id="549f6-291">You must not deploy backward-incompatible API schema changes until you can be sure that all users have upgraded.</span></span> <span data-ttu-id="549f6-292">或者,阻止使用者使用不相容的舊版本的應用。</span><span class="sxs-lookup"><span data-stu-id="549f6-292">Alternatively, block users from using incompatible older versions of the app.</span></span> <span data-ttu-id="549f6-293">此方案要求與本機移動應用相同。</span><span class="sxs-lookup"><span data-stu-id="549f6-293">This scenario requirement is the same as for native mobile apps.</span></span> <span data-ttu-id="549f6-294">如果部署伺服器 API 中的重大更改,則尚未更新的使用者的用戶端應用將中斷。</span><span class="sxs-lookup"><span data-stu-id="549f6-294">If you deploy a breaking change in server APIs, the client app is broken for users who haven't yet updated.</span></span>

<span data-ttu-id="549f6-295">如果可能,不要將重大更改部署到後端 API。</span><span class="sxs-lookup"><span data-stu-id="549f6-295">If possible, don't deploy breaking changes to your backend APIs.</span></span> <span data-ttu-id="549f6-296">如果必須這樣做,請考慮使用[標準服務輔助角色 API(如服務輔助角色註冊](https://developer.mozilla.org/docs/Web/API/ServiceWorkerRegistration))來確定應用是否是最新的應用,如果沒有,則防止使用。</span><span class="sxs-lookup"><span data-stu-id="549f6-296">If you must do so, consider using [standard Service Worker APIs such as ServiceWorkerRegistration](https://developer.mozilla.org/docs/Web/API/ServiceWorkerRegistration) to determine whether the app is up-to-date, and if not, to prevent usage.</span></span>

### <a name="interference-with-server-rendered-pages"></a><span data-ttu-id="549f6-297">干擾伺服器呈現的頁面</span><span class="sxs-lookup"><span data-stu-id="549f6-297">Interference with server-rendered pages</span></span>

<span data-ttu-id="549f6-298">如[支援伺服器呈現的頁面](#support-server-rendered-pages)部分所述,如果要繞過服務工作人員為所有導航`/index.html`請求返回 內容的行為,請編輯服務輔助角色中的邏輯。</span><span class="sxs-lookup"><span data-stu-id="549f6-298">As described in the [Support server-rendered pages](#support-server-rendered-pages) section, if you want to bypass the service worker's behavior of returning `/index.html` contents for all navigation requests, edit the logic in your service worker.</span></span>

### <a name="all-service-worker-asset-manifest-contents-are-cached-by-default"></a><span data-ttu-id="549f6-299">預設情況下快取所有服務輔助角色資產清單內容</span><span class="sxs-lookup"><span data-stu-id="549f6-299">All service worker asset manifest contents are cached by default</span></span>

<span data-ttu-id="549f6-300">如[控制資產緩存](#control-asset-caching)「部分所述,檔*服務-輔助角色資產.js*是在生成期間生成的,並列出服務工作人員應提取和緩存的所有資產。</span><span class="sxs-lookup"><span data-stu-id="549f6-300">As described in the [Control asset caching](#control-asset-caching) section, the file *service-worker-assets.js* is generated during build and lists all assets the service worker should fetch and cache.</span></span>

<span data-ttu-id="549f6-301">由於此清單預設包含發送到*wwwroot*的所有內容,包括外部包和專案提供的內容,因此必須小心不要將太多內容放在其中。</span><span class="sxs-lookup"><span data-stu-id="549f6-301">Since this list by default includes everything emitted to *wwwroot*, including content supplied by external packages and projects, you must be careful not to put too much content there.</span></span> <span data-ttu-id="549f6-302">如果*wwwroot*目錄包含數百萬個圖像,則服務工作人員將嘗試獲取和緩存它們,消耗過多的頻寬,並且很可能無法成功完成。</span><span class="sxs-lookup"><span data-stu-id="549f6-302">If the *wwwroot* directory contains millions of images, the service worker tries to fetch and cache them all, consuming excessive bandwidth and most likely not completing successfully.</span></span>

<span data-ttu-id="549f6-303">實現任意邏輯,通過編輯`onInstall`*服務輔助角色*中的函數來控制應提取和緩存清單內容的哪個子集。</span><span class="sxs-lookup"><span data-stu-id="549f6-303">Implement arbitrary logic to control which subset of the manifest's contents should be fetched and cached by editing the `onInstall` function in *service-worker.published.js*.</span></span>

### <a name="interaction-with-authentication"></a><span data-ttu-id="549f6-304">與認證的互動</span><span class="sxs-lookup"><span data-stu-id="549f6-304">Interaction with authentication</span></span>

<span data-ttu-id="549f6-305">可以結合身份驗證選項使用 PWA 樣本選項。</span><span class="sxs-lookup"><span data-stu-id="549f6-305">It's possible to use the PWA template option in conjunction with the authentication options.</span></span> <span data-ttu-id="549f6-306">當使用者具有網路連接時,支援離線的 PWA 也可以支援身份驗證。</span><span class="sxs-lookup"><span data-stu-id="549f6-306">An offline-capable PWA can also support authentication when the user has network connectivity.</span></span>

<span data-ttu-id="549f6-307">當用戶沒有網路連接時,他們無法驗證或獲取訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="549f6-307">When a user doesn't have network connectivity, they can't authenticate or obtain access tokens.</span></span> <span data-ttu-id="549f6-308">默認情況下,嘗試在沒有網路訪問的情況下訪問登錄頁會導致"網路錯誤"消息。</span><span class="sxs-lookup"><span data-stu-id="549f6-308">By default, attempting to visit the login page without network access results in a "network error" message.</span></span>

<span data-ttu-id="549f6-309">您必須設計一個 UI 流,允許使用者在離線時執行有用操作,而無需嘗試驗證或獲取存取權杖。</span><span class="sxs-lookup"><span data-stu-id="549f6-309">You must design a UI flow that lets the user do useful things while offline without attempting to authenticate or obtain access tokens.</span></span> <span data-ttu-id="549f6-310">或者,您可以將應用設計為在網路不可用時以正常方式失敗。</span><span class="sxs-lookup"><span data-stu-id="549f6-310">Alternatively, you can design the app to fail in a graceful way when the network isn't available.</span></span> <span data-ttu-id="549f6-311">如果應用中無法啟用此功能,則可能不希望啟用脫機支援。</span><span class="sxs-lookup"><span data-stu-id="549f6-311">If this isn't possible in your app, you might not want to enable offline support.</span></span>
