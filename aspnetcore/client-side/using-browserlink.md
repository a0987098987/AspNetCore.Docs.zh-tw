---
title: ASP.NET Core 中的瀏覽器連結
author: ncarandini
description: 說明瀏覽器連結是一種 Visual Studio 功能，可將開發環境與一或多個網頁瀏覽器連結在一起。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/09/2020
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: 19cc3c2ed91bd9e05df3c036123c78ecbf81fcc0
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828266"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="d9c25-103">ASP.NET Core 中的瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="d9c25-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="d9c25-104">By [Nicolò Carandini](https://github.com/ncarandini)、 [Mike Wasson](https://github.com/MikeWasson)和[Tom 作者: dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d9c25-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d9c25-105">瀏覽器連結是 Visual Studio 功能。</span><span class="sxs-lookup"><span data-stu-id="d9c25-105">Browser Link is a Visual Studio feature.</span></span> <span data-ttu-id="d9c25-106">它會在開發環境與一或多個網頁瀏覽器之間建立通道。</span><span class="sxs-lookup"><span data-stu-id="d9c25-106">It creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="d9c25-107">您可以使用瀏覽器連結，一次在數個瀏覽器中重新整理您的 web 應用程式，這對於跨瀏覽器測試很有用。</span><span class="sxs-lookup"><span data-stu-id="d9c25-107">You can use Browser Link to refresh your web app in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="d9c25-108">瀏覽器連結設定</span><span class="sxs-lookup"><span data-stu-id="d9c25-108">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d9c25-109">將[BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)套件新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="d9c25-109">Add the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package to your project.</span></span> <span data-ttu-id="d9c25-110">針對 ASP.NET Core Razor Pages 或 MVC 專案，也請啟用 Razor （*cshtml*）檔案的執行時間編譯，如 <xref:mvc/views/view-compilation>中所述。</span><span class="sxs-lookup"><span data-stu-id="d9c25-110">For ASP.NET Core Razor Pages or MVC projects, also enable runtime compilation of Razor (*.cshtml*) files as described in <xref:mvc/views/view-compilation>.</span></span> <span data-ttu-id="d9c25-111">只有在已啟用執行時間編譯時，才會套用 Razor 語法變更。</span><span class="sxs-lookup"><span data-stu-id="d9c25-111">Razor syntax changes are applied only when runtime compilation has been enabled.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="d9c25-112">將 ASP.NET Core 2.0 專案轉換成 ASP.NET Core 2.1 並轉換成[AspNetCore 應用程式](xref:fundamentals/metapackage-app)時，請安裝 VisualStudio 瀏覽器連結功能的[BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)套件。</span><span class="sxs-lookup"><span data-stu-id="d9c25-112">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for Browser Link functionality.</span></span> <span data-ttu-id="d9c25-113">ASP.NET Core 2.1 專案範本預設會使用 `Microsoft.AspNetCore.App` 中繼套件。</span><span class="sxs-lookup"><span data-stu-id="d9c25-113">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="d9c25-114">ASP.NET Core 2.0 **Web 應用程式**、**空**的和**Web API**專案範本都會使用[AspNetCore 中繼套件](xref:fundamentals/metapackage)，其中包含[VisualStudio](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)的套件參考。</span><span class="sxs-lookup"><span data-stu-id="d9c25-114">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="d9c25-115">因此，使用 `Microsoft.AspNetCore.All` 中繼套件不需要進一步的動作，即可讓瀏覽器連結可供使用。</span><span class="sxs-lookup"><span data-stu-id="d9c25-115">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="d9c25-116">ASP.NET Core 1.x **Web 應用程式**專案範本具有 BrowserLink 套件的套件參考。（ [VisualStudio](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) ）。</span><span class="sxs-lookup"><span data-stu-id="d9c25-116">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="d9c25-117">其他專案類型則需要您將套件參考新增至 `Microsoft.VisualStudio.Web.BrowserLink`。</span><span class="sxs-lookup"><span data-stu-id="d9c25-117">Other project types require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="d9c25-118">組態</span><span class="sxs-lookup"><span data-stu-id="d9c25-118">Configuration</span></span>

<span data-ttu-id="d9c25-119">呼叫 `Startup.Configure` 方法中的 `UseBrowserLink`：</span><span class="sxs-lookup"><span data-stu-id="d9c25-119">Call `UseBrowserLink` in the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="d9c25-120">`UseBrowserLink` 呼叫通常會放在只在開發環境中啟用瀏覽器連結的 `if` 區塊內。</span><span class="sxs-lookup"><span data-stu-id="d9c25-120">The `UseBrowserLink` call is typically placed inside an `if` block that only enables Browser Link in the Development environment.</span></span> <span data-ttu-id="d9c25-121">例如：</span><span class="sxs-lookup"><span data-stu-id="d9c25-121">For example:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="d9c25-122">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="d9c25-122">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="d9c25-123">如何使用瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="d9c25-123">How to use Browser Link</span></span>

<span data-ttu-id="d9c25-124">當您開啟 ASP.NET Core 專案時，Visual Studio 會在 [ **Debug Target** ] 工具列控制項旁顯示瀏覽器連結工具列控制項：</span><span class="sxs-lookup"><span data-stu-id="d9c25-124">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![瀏覽器連結下拉式功能表](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="d9c25-126">從瀏覽器連結工具列控制項，您可以：</span><span class="sxs-lookup"><span data-stu-id="d9c25-126">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="d9c25-127">一次在數個瀏覽器中重新整理 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d9c25-127">Refresh the web app in several browsers at once.</span></span>
* <span data-ttu-id="d9c25-128">開啟**瀏覽器連結儀表板**。</span><span class="sxs-lookup"><span data-stu-id="d9c25-128">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="d9c25-129">啟用或停用**瀏覽器連結**。</span><span class="sxs-lookup"><span data-stu-id="d9c25-129">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="d9c25-130">注意： Visual Studio 中預設會停用瀏覽器連結。</span><span class="sxs-lookup"><span data-stu-id="d9c25-130">Note: Browser Link is disabled by default in Visual Studio.</span></span>
* <span data-ttu-id="d9c25-131">啟用或停用[CSS 自動同步](#enable-or-disable-css-auto-sync)處理。</span><span class="sxs-lookup"><span data-stu-id="d9c25-131">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="d9c25-132">一次在數個瀏覽器中重新整理 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="d9c25-132">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="d9c25-133">若要在啟動專案時選擇要啟動的單一 web 瀏覽器，請使用 [ **Debug Target** ] 工具列控制項中的下拉式功能表：</span><span class="sxs-lookup"><span data-stu-id="d9c25-133">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![F5 下拉式功能表](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="d9c25-135">若要一次開啟多個瀏覽器，請從相同的下拉式選單選擇 **[流覽方式 ...]** 。</span><span class="sxs-lookup"><span data-stu-id="d9c25-135">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="d9c25-136">按住<kbd>Ctrl</kbd>鍵以選取您要的瀏覽器，然後按一下 **[流覽]** ：</span><span class="sxs-lookup"><span data-stu-id="d9c25-136">Hold down the <kbd>Ctrl</kbd> key to select the browsers you want, and then click **Browse**:</span></span>

![一次開啟許多瀏覽器](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="d9c25-138">下列螢幕擷取畫面顯示索引視圖開啟和兩個開啟的瀏覽器的 Visual Studio：</span><span class="sxs-lookup"><span data-stu-id="d9c25-138">The following screenshot shows Visual Studio with the Index view open and two open browsers:</span></span>

![與兩個瀏覽器同步處理範例](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="d9c25-140">將滑鼠停留在瀏覽器連結工具列控制項上，以查看已連接到專案的瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="d9c25-140">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![暫留提示](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="d9c25-142">變更 [索引] 視圖，當您按一下瀏覽器連結的 [重新整理] 按鈕時，就會更新所有連線的瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="d9c25-142">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![瀏覽器-同步至變更](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="d9c25-144">瀏覽器連結也適用于從外部 Visual Studio 啟動的瀏覽器，並流覽至應用程式 URL。</span><span class="sxs-lookup"><span data-stu-id="d9c25-144">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the app URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="d9c25-145">瀏覽器連結儀表板</span><span class="sxs-lookup"><span data-stu-id="d9c25-145">The Browser Link Dashboard</span></span>

<span data-ttu-id="d9c25-146">從瀏覽器連結下拉式功能表開啟**瀏覽器連結儀表板**視窗，以管理與開放式瀏覽器的連接：</span><span class="sxs-lookup"><span data-stu-id="d9c25-146">Open the **Browser Link Dashboard** window from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![開啟-browserslink-儀表板](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="d9c25-148">如果沒有連接的瀏覽器，您可以藉由選取 [**在瀏覽器中查看**] 連結來啟動非偵錯工具的會話：</span><span class="sxs-lookup"><span data-stu-id="d9c25-148">If no browser is connected, you can start a non-debugging session by selecting the **View in Browser** link:</span></span>

![browserlink-儀表板-無連接](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="d9c25-150">否則，會顯示已連線的瀏覽器，其中包含每個瀏覽器所顯示頁面的路徑：</span><span class="sxs-lookup"><span data-stu-id="d9c25-150">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-儀表板-兩個連接](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="d9c25-152">您也可以按一下個別的瀏覽器名稱，只重新整理該瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="d9c25-152">You can also click on an individual browser name to refresh only that browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="d9c25-153">啟用或停用瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="d9c25-153">Enable or disable Browser Link</span></span>

<span data-ttu-id="d9c25-154">停用瀏覽器連結之後，您必須重新整理瀏覽器，才能重新連線。</span><span class="sxs-lookup"><span data-stu-id="d9c25-154">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="d9c25-155">啟用或停用 CSS 自動同步處理</span><span class="sxs-lookup"><span data-stu-id="d9c25-155">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="d9c25-156">啟用 CSS 自動同步處理時，會在您對 CSS 檔案進行任何變更時，自動重新整理連接的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="d9c25-156">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="d9c25-157">運作方式</span><span class="sxs-lookup"><span data-stu-id="d9c25-157">How it works</span></span>

<span data-ttu-id="d9c25-158">瀏覽器連結會使用[SignalR](xref:signalr/introduction)來建立 Visual Studio 與瀏覽器之間的通道。</span><span class="sxs-lookup"><span data-stu-id="d9c25-158">Browser Link uses [SignalR](xref:signalr/introduction) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="d9c25-159">啟用瀏覽器連結時，Visual Studio 會作為多個用戶端（瀏覽器）可以連接的 SignalR 伺服器。</span><span class="sxs-lookup"><span data-stu-id="d9c25-159">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="d9c25-160">瀏覽器連結也會在 ASP.NET Core 要求管線中註冊中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="d9c25-160">Browser Link also registers a middleware component in the ASP.NET Core request pipeline.</span></span> <span data-ttu-id="d9c25-161">此元件會將特殊的 `<script>` 參考插入伺服器中的每個頁面要求。</span><span class="sxs-lookup"><span data-stu-id="d9c25-161">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="d9c25-162">您可以在瀏覽器中選取 [ **View source** ]，然後在 `<body>` 標記內容的結尾處，看到腳本參考：</span><span class="sxs-lookup"><span data-stu-id="d9c25-162">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="d9c25-163">您的原始程式檔不會遭到修改。</span><span class="sxs-lookup"><span data-stu-id="d9c25-163">Your source files aren't modified.</span></span> <span data-ttu-id="d9c25-164">中介軟體元件會動態插入腳本參考。</span><span class="sxs-lookup"><span data-stu-id="d9c25-164">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="d9c25-165">因為瀏覽器端程式碼是所有的 JavaScript，所以它適用于所有 SignalR 支援的瀏覽器，而不需要瀏覽器外掛程式。</span><span class="sxs-lookup"><span data-stu-id="d9c25-165">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
