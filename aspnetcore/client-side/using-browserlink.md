---
title: ASP.NET核心中的瀏覽器連結
author: ncarandini
description: 說明瀏覽器連結如何是一個可視化工作室功能,該功能將開發環境與一個或多個 Web 瀏覽器連結。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/09/2020
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: 19cc3c2ed91bd9e05df3c036123c78ecbf81fcc0
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78658848"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="3d1a2-103">ASP.NET核心中的瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="3d1a2-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="3d1a2-104">由[尼科萊·卡蘭迪尼](https://github.com/ncarandini)、[邁克·瓦森](https://github.com/MikeWasson)和[湯姆·戴克斯特拉](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="3d1a2-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="3d1a2-105">瀏覽器連結是視覺工作室功能。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-105">Browser Link is a Visual Studio feature.</span></span> <span data-ttu-id="3d1a2-106">它在開發環境與一個或多個 Web 瀏覽器之間創建通信通道。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-106">It creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="3d1a2-107">您可以使用瀏覽器連結同時刷新多個瀏覽器中的 Web 應用,這對於跨瀏覽器測試非常有用。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-107">You can use Browser Link to refresh your web app in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="3d1a2-108">瀏覽器連結設定</span><span class="sxs-lookup"><span data-stu-id="3d1a2-108">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3d1a2-109">將[Microsoft.VisualStudio.Web.瀏覽器連結](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)包添加到您的專案中。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-109">Add the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package to your project.</span></span> <span data-ttu-id="3d1a2-110">對於ASP.NET核心剃刀頁面或MVC專案,也啟用剃刀 *(.cshtml)* 檔的<xref:mvc/views/view-compilation>運行時編譯,如中所述。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-110">For ASP.NET Core Razor Pages or MVC projects, also enable runtime compilation of Razor (*.cshtml*) files as described in <xref:mvc/views/view-compilation>.</span></span> <span data-ttu-id="3d1a2-111">僅當啟用運行時編譯時,才會應用 Razor 語法更改。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-111">Razor syntax changes are applied only when runtime compilation has been enabled.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="3d1a2-112">將ASP.NET酷睿2.0專案轉換為ASP.NET酷睿2.1並過渡到[微軟.AspNetCore.App元包](xref:fundamentals/metapackage-app)時,安裝[微軟.VisualStudio.Web.瀏覽器鏈接](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)包,用於瀏覽器連結功能。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-112">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for Browser Link functionality.</span></span> <span data-ttu-id="3d1a2-113">默認情況下,ASP.NET Core 2.1 專案`Microsoft.AspNetCore.App`範本使用元包。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-113">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="3d1a2-114">ASP.NET核心2.0 **Web應用程式**、**空**和**Web API**專案範本使用[微軟.AspNetCore.所有元包](xref:fundamentals/metapackage),其中包含[微軟的](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)套件引用。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-114">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="3d1a2-115">因此,`Microsoft.AspNetCore.All`使用元包不需要進一步的操作,以使瀏覽器連結可供使用。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-115">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="3d1a2-116">ASP.NET核心 1.x **Web 應用程式**專案範本具有[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)包的包引用。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-116">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="3d1a2-117">其他項目類型要求您添加對`Microsoft.VisualStudio.Web.BrowserLink`的包引用。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-117">Other project types require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="3d1a2-118">組態</span><span class="sxs-lookup"><span data-stu-id="3d1a2-118">Configuration</span></span>

<span data-ttu-id="3d1a2-119">呼叫 `Startup.Configure` 方法中的 `UseBrowserLink`：</span><span class="sxs-lookup"><span data-stu-id="3d1a2-119">Call `UseBrowserLink` in the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="3d1a2-120">`UseBrowserLink`調用通常放置在僅在開發環境中啟用`if`瀏覽器鏈接的塊內。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-120">The `UseBrowserLink` call is typically placed inside an `if` block that only enables Browser Link in the Development environment.</span></span> <span data-ttu-id="3d1a2-121">例如：</span><span class="sxs-lookup"><span data-stu-id="3d1a2-121">For example:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="3d1a2-122">如需詳細資訊，請參閱 <xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-122">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="3d1a2-123">如何使用瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="3d1a2-123">How to use Browser Link</span></span>

<span data-ttu-id="3d1a2-124">開啟 ASP.NET 核心項目時,Visual Studio 會在**除錯目標**工具列控制者旁邊顯示瀏覽器連結工具列控制項:</span><span class="sxs-lookup"><span data-stu-id="3d1a2-124">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![瀏覽器連結下拉選單](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="3d1a2-126">從瀏覽器連結工具列控制件中,您可以:</span><span class="sxs-lookup"><span data-stu-id="3d1a2-126">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="3d1a2-127">一次在多個瀏覽器中刷新 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-127">Refresh the web app in several browsers at once.</span></span>
* <span data-ttu-id="3d1a2-128">開啟**瀏覽器連結儀表板**。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-128">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="3d1a2-129">開啟或停用**瀏覽器連結**。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-129">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="3d1a2-130">注意:默認情況下,在可視化工作室中禁用瀏覽器連結。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-130">Note: Browser Link is disabled by default in Visual Studio.</span></span>
* <span data-ttu-id="3d1a2-131">開啟或關閉[CSS 自動同步](#enable-or-disable-css-auto-sync)。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-131">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="3d1a2-132">在多個瀏覽器中刷新 Web 應用</span><span class="sxs-lookup"><span data-stu-id="3d1a2-132">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="3d1a2-133">要選擇啟動項目時要啟動的單一網頁瀏覽器,請使用**除錯目標**工具列控制項中的下拉選單:</span><span class="sxs-lookup"><span data-stu-id="3d1a2-133">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![F5 下拉選單](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="3d1a2-135">要同時開啟多個瀏覽器,請從同一下拉清單中選擇 **「使用...**</span><span class="sxs-lookup"><span data-stu-id="3d1a2-135">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="3d1a2-136">按住<kbd>Ctrl</kbd>鍵以選擇所需的瀏覽器,然後按下 **::**</span><span class="sxs-lookup"><span data-stu-id="3d1a2-136">Hold down the <kbd>Ctrl</kbd> key to select the browsers you want, and then click **Browse**:</span></span>

![一次開啟多個瀏覽器](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="3d1a2-138">以下螢幕截圖顯示了開啟索引檢視和兩個打開瀏覽器的可視化工作室:</span><span class="sxs-lookup"><span data-stu-id="3d1a2-138">The following screenshot shows Visual Studio with the Index view open and two open browsers:</span></span>

![與兩個瀏覽器同步範例](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="3d1a2-140">將滑鼠的移動在瀏覽器連結工具列控制程式上以檢視連接到專案的瀏覽器:</span><span class="sxs-lookup"><span data-stu-id="3d1a2-140">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![暫停提示](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="3d1a2-142">變更索引檢視,按下瀏覽器連結刷新按鈕時,將更新所有連接的瀏覽器:</span><span class="sxs-lookup"><span data-stu-id="3d1a2-142">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![瀏覽器同步到變更](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="3d1a2-144">瀏覽器連結還適用於從 Visual Studio 外部啟動並導航到應用 URL 的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-144">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the app URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="3d1a2-145">瀏覽器連結儀表板</span><span class="sxs-lookup"><span data-stu-id="3d1a2-145">The Browser Link Dashboard</span></span>

<span data-ttu-id="3d1a2-146">從瀏覽器連結下拉選單開啟**瀏覽器連結儀表板**視窗,以管理與開啟瀏覽器的連接:</span><span class="sxs-lookup"><span data-stu-id="3d1a2-146">Open the **Browser Link Dashboard** window from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![開啟瀏覽器連結-儀表板](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="3d1a2-148">如果沒有連接瀏覽器,則可以通過選擇 **「瀏覽器中的檢視」** 連結啟動非調試會話:</span><span class="sxs-lookup"><span data-stu-id="3d1a2-148">If no browser is connected, you can start a non-debugging session by selecting the **View in Browser** link:</span></span>

![瀏覽器連結-儀表板-無連線](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="3d1a2-150">否則,連接的瀏覽器將顯示每個瀏覽器顯示的頁面路徑:</span><span class="sxs-lookup"><span data-stu-id="3d1a2-150">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![瀏覽器連結-儀錶板-雙連接](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="3d1a2-152">您還可以按一下單個瀏覽器名稱以僅刷新該瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-152">You can also click on an individual browser name to refresh only that browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="3d1a2-153">開啟或停用瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="3d1a2-153">Enable or disable Browser Link</span></span>

<span data-ttu-id="3d1a2-154">禁用瀏覽器連結後重新啟用瀏覽器連結時,必須刷新瀏覽器以重新連接它們。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-154">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="3d1a2-155">開啟或關閉 CSS 自動同步</span><span class="sxs-lookup"><span data-stu-id="3d1a2-155">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="3d1a2-156">啟用 CSS 自動同步後,當您對 CSS 檔進行任何更改時,將自動刷新已連接的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-156">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="3d1a2-157">運作方式</span><span class="sxs-lookup"><span data-stu-id="3d1a2-157">How it works</span></span>

<span data-ttu-id="3d1a2-158">瀏覽器連結用於[SignalR](xref:signalr/introduction)在可視化工作室和瀏覽器之間創建通信通道。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-158">Browser Link uses [SignalR](xref:signalr/introduction) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="3d1a2-159">啟用瀏覽器連結後,Visual Studio 充SignalR當多個用戶端(瀏覽器)可以連接到的伺服器。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-159">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="3d1a2-160">瀏覽器連結還在ASP.NET核心請求管道中註冊中間件元件。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-160">Browser Link also registers a middleware component in the ASP.NET Core request pipeline.</span></span> <span data-ttu-id="3d1a2-161">此元件向來自伺服器`<script>`的每個頁面請求注入特殊引用。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-161">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="3d1a2-162">透過瀏覽器中選擇 **「檢視來源」** 並捲軸到標記內容`<body>`的末尾,可以查看文稿引用:</span><span class="sxs-lookup"><span data-stu-id="3d1a2-162">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="3d1a2-163">源檔未修改。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-163">Your source files aren't modified.</span></span> <span data-ttu-id="3d1a2-164">中間件元件動態注入腳本引用。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-164">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="3d1a2-165">因為瀏覽器端代碼是所有 JAvaScript,因此它適用於所有支援的瀏覽器SignalR, 而無需瀏覽器外掛程式。</span><span class="sxs-lookup"><span data-stu-id="3d1a2-165">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
