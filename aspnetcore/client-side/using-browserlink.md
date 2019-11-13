---
title: ASP.NET Core 中的瀏覽器連結
author: ncarandini
description: 說明瀏覽器連結是一種 Visual Studio 功能，可將開發環境與一或多個網頁瀏覽器連結在一起。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: b21b698d49e72b559cd9cd3753c48a38c99db24d
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962789"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="366f4-103">ASP.NET Core 中的瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="366f4-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="366f4-104">By [Nicolò Carandini](https://github.com/ncarandini)、 [Mike Wasson](https://github.com/MikeWasson)和[Tom 作者: dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="366f4-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="366f4-105">瀏覽器連結是 Visual Studio 中的一項功能，可在開發環境與一或多個網頁瀏覽器之間建立通道。</span><span class="sxs-lookup"><span data-stu-id="366f4-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="366f4-106">您可以使用瀏覽器連結，一次在數個瀏覽器中重新整理 web 應用程式，這對於跨瀏覽器測試很有用。</span><span class="sxs-lookup"><span data-stu-id="366f4-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="366f4-107">瀏覽器連結設定</span><span class="sxs-lookup"><span data-stu-id="366f4-107">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="366f4-108">將 ASP.NET Core 2.0 專案轉換成 ASP.NET Core 2.1 並轉換成[AspNetCore 應用程式](xref:fundamentals/metapackage-app)時，請安裝 VisualStudio BrowserLink 功能的[BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)套件。</span><span class="sxs-lookup"><span data-stu-id="366f4-108">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for BrowserLink functionality.</span></span> <span data-ttu-id="366f4-109">ASP.NET Core 2.1 專案範本預設會使用 `Microsoft.AspNetCore.App` 中繼套件。</span><span class="sxs-lookup"><span data-stu-id="366f4-109">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="366f4-110">ASP.NET Core 2.0 **Web 應用程式**、**空**的和**Web API**專案範本都會使用[AspNetCore 中繼套件](xref:fundamentals/metapackage)，其中包含[VisualStudio](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)的套件參考。</span><span class="sxs-lookup"><span data-stu-id="366f4-110">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="366f4-111">因此，使用 `Microsoft.AspNetCore.All` 中繼套件不需要進一步的動作，即可讓瀏覽器連結可供使用。</span><span class="sxs-lookup"><span data-stu-id="366f4-111">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="366f4-112">ASP.NET Core 1.x **Web 應用程式**專案範本具有 BrowserLink 套件的套件參考。（ [VisualStudio](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) ）。</span><span class="sxs-lookup"><span data-stu-id="366f4-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="366f4-113">**空**的或**Web API**範本專案會要求您將套件參考新增至 `Microsoft.VisualStudio.Web.BrowserLink`。</span><span class="sxs-lookup"><span data-stu-id="366f4-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="366f4-114">由於這是 Visual Studio 的功能，將封裝新增至**空白**或**Web API**範本專案的最簡單方式，就是開啟 [**套件管理員主控台**] （**View** >**其他 Windows** > **package Manager console**），然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="366f4-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="366f4-115">或者，您可以使用**NuGet 套件管理員**。</span><span class="sxs-lookup"><span data-stu-id="366f4-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="366f4-116">以滑鼠右鍵按一下**方案總管**中的專案名稱，然後選擇 [**管理 NuGet 套件**]：</span><span class="sxs-lookup"><span data-stu-id="366f4-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![開啟 NuGet 套件管理員](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="366f4-118">尋找並安裝套件：</span><span class="sxs-lookup"><span data-stu-id="366f4-118">Find and install the package:</span></span>

![使用 NuGet 套件管理員新增套件](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="366f4-120">Configuration</span><span class="sxs-lookup"><span data-stu-id="366f4-120">Configuration</span></span>

<span data-ttu-id="366f4-121">在 `Startup.Configure` 方法中：</span><span class="sxs-lookup"><span data-stu-id="366f4-121">In the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="366f4-122">通常程式碼位於只在開發環境中啟用瀏覽器連結的 `if` 區塊內，如下所示：</span><span class="sxs-lookup"><span data-stu-id="366f4-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="366f4-123">如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="366f4-123">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="366f4-124">如何使用瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="366f4-124">How to use Browser Link</span></span>

<span data-ttu-id="366f4-125">當您開啟 ASP.NET Core 專案時，Visual Studio 會在 [ **Debug Target** ] 工具列控制項旁顯示瀏覽器連結工具列控制項：</span><span class="sxs-lookup"><span data-stu-id="366f4-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![瀏覽器連結下拉式功能表](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="366f4-127">從瀏覽器連結工具列控制項，您可以：</span><span class="sxs-lookup"><span data-stu-id="366f4-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="366f4-128">一次在數個瀏覽器中重新整理 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="366f4-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="366f4-129">開啟**瀏覽器連結儀表板**。</span><span class="sxs-lookup"><span data-stu-id="366f4-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="366f4-130">啟用或停用**瀏覽器連結**。</span><span class="sxs-lookup"><span data-stu-id="366f4-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="366f4-131">注意： Visual Studio 2017 （15.3）中，預設會停用瀏覽器連結。</span><span class="sxs-lookup"><span data-stu-id="366f4-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="366f4-132">啟用或停用[CSS 自動同步](#enable-or-disable-css-auto-sync)處理。</span><span class="sxs-lookup"><span data-stu-id="366f4-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="366f4-133">有些 Visual Studio 外掛程式（最值得注意的是， *Web Extension pack 2015*和*web extension pack 2017*）提供瀏覽器連結的擴充功能，但某些其他功能無法與 ASP.NET Core 專案搭配使用。</span><span class="sxs-lookup"><span data-stu-id="366f4-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="366f4-134">一次在數個瀏覽器中重新整理 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="366f4-134">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="366f4-135">若要在啟動專案時選擇要啟動的單一 web 瀏覽器，請使用 [ **Debug Target** ] 工具列控制項中的下拉式功能表：</span><span class="sxs-lookup"><span data-stu-id="366f4-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![F5 下拉式功能表](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="366f4-137">若要一次開啟多個瀏覽器，請從相同的下拉式選單選擇 **[流覽方式 ...]** 。</span><span class="sxs-lookup"><span data-stu-id="366f4-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="366f4-138">按住 CTRL 鍵以選取您要的瀏覽器，然後按一下 **[流覽]** ：</span><span class="sxs-lookup"><span data-stu-id="366f4-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![一次開啟許多瀏覽器](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="366f4-140">以下螢幕擷取畫面顯示索引視圖開啟和兩個開啟的瀏覽器的 Visual Studio：</span><span class="sxs-lookup"><span data-stu-id="366f4-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![與兩個瀏覽器同步處理範例](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="366f4-142">將滑鼠停留在瀏覽器連結工具列控制項上，以查看已連接到專案的瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="366f4-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![暫留提示](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="366f4-144">變更 [索引] 視圖，當您按一下瀏覽器連結的 [重新整理] 按鈕時，就會更新所有連線的瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="366f4-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![瀏覽器-同步至變更](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="366f4-146">瀏覽器連結也適用于從外部 Visual Studio 啟動的瀏覽器，並流覽至應用程式 URL。</span><span class="sxs-lookup"><span data-stu-id="366f4-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="366f4-147">瀏覽器連結儀表板</span><span class="sxs-lookup"><span data-stu-id="366f4-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="366f4-148">從瀏覽器連結下拉式功能表開啟瀏覽器連結儀表板，以管理與開放式瀏覽器的連接：</span><span class="sxs-lookup"><span data-stu-id="366f4-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![開啟-browserslink-儀表板](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="366f4-150">如果沒有連接的瀏覽器，您可以藉由選取 [*在瀏覽器中查看*] 連結來啟動非偵錯工具的會話：</span><span class="sxs-lookup"><span data-stu-id="366f4-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-儀表板-無連接](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="366f4-152">否則，會顯示已連線的瀏覽器，其中包含每個瀏覽器所顯示頁面的路徑：</span><span class="sxs-lookup"><span data-stu-id="366f4-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-儀表板-兩個連接](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="366f4-154">如有需要，您可以按一下列出的瀏覽器名稱，以重新整理該單一瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="366f4-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="366f4-155">啟用或停用瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="366f4-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="366f4-156">停用瀏覽器連結之後，您必須重新整理瀏覽器，才能重新連線。</span><span class="sxs-lookup"><span data-stu-id="366f4-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="366f4-157">啟用或停用 CSS 自動同步處理</span><span class="sxs-lookup"><span data-stu-id="366f4-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="366f4-158">啟用 CSS 自動同步處理時，會在您對 CSS 檔案進行任何變更時，自動重新整理連接的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="366f4-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="366f4-159">運作方式</span><span class="sxs-lookup"><span data-stu-id="366f4-159">How it works</span></span>

<span data-ttu-id="366f4-160">瀏覽器連結會使用 SignalR 來建立 Visual Studio 與瀏覽器之間的通道。</span><span class="sxs-lookup"><span data-stu-id="366f4-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="366f4-161">啟用瀏覽器連結時，Visual Studio 會作為多個用戶端（瀏覽器）可以連接的 SignalR 伺服器。</span><span class="sxs-lookup"><span data-stu-id="366f4-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="366f4-162">瀏覽器連結也會在 ASP.NET Core 要求管線中註冊中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="366f4-162">Browser Link also registers a middleware component in the ASP.NET Core request pipeline.</span></span> <span data-ttu-id="366f4-163">此元件會將特殊的 `<script>` 參考插入伺服器中的每個頁面要求。</span><span class="sxs-lookup"><span data-stu-id="366f4-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="366f4-164">您可以在瀏覽器中選取 [ **View source** ]，然後在 `<body>` 標記內容的結尾處，看到腳本參考：</span><span class="sxs-lookup"><span data-stu-id="366f4-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="366f4-165">您的原始程式檔不會遭到修改。</span><span class="sxs-lookup"><span data-stu-id="366f4-165">Your source files aren't modified.</span></span> <span data-ttu-id="366f4-166">中介軟體元件會動態插入腳本參考。</span><span class="sxs-lookup"><span data-stu-id="366f4-166">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="366f4-167">因為瀏覽器端程式碼是所有的 JavaScript，所以它適用于所有 SignalR 支援的瀏覽器，而不需要瀏覽器外掛程式。</span><span class="sxs-lookup"><span data-stu-id="366f4-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
