---
title: ASP.NET Core 中的瀏覽器連結
author: ncarandini
description: 說明瀏覽器連結如何連結一或多個網頁瀏覽器的開發環境的 Visual Studio 功能。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 452ba5149563c186750466f471c7b950f0017614
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894705"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="db4a5-103">ASP.NET Core 中的瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="db4a5-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="db4a5-104">藉由[Nicolò Carandini](https://github.com/ncarandini)， [Mike Wasson](https://github.com/MikeWasson)，和[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="db4a5-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="db4a5-105">瀏覽器連結是在 Visual Studio 中建立開發環境與一或多個網頁瀏覽器之間的通訊通道的功能。</span><span class="sxs-lookup"><span data-stu-id="db4a5-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="db4a5-106">您可以使用瀏覽器連結，以重新整理 web 應用程式在數個瀏覽器中的，這是適用於跨瀏覽器測試。</span><span class="sxs-lookup"><span data-stu-id="db4a5-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="db4a5-107">瀏覽器連結設定</span><span class="sxs-lookup"><span data-stu-id="db4a5-107">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="db4a5-108">將 ASP.NET Core 2.0 專案轉換成 ASP.NET Core 2.1 並移轉到時[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，安裝[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)封裝BrowserLink 功能。</span><span class="sxs-lookup"><span data-stu-id="db4a5-108">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for BrowserLink functionality.</span></span> <span data-ttu-id="db4a5-109">ASP.NET Core 2.1 專案範本會使用`Microsoft.AspNetCore.App`預設的中繼套件。</span><span class="sxs-lookup"><span data-stu-id="db4a5-109">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="db4a5-110">ASP.NET Core 2.0 **Web 應用程式**，**空白**，並**Web API**專案範本會使用[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)其中包含的套件參考[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)。</span><span class="sxs-lookup"><span data-stu-id="db4a5-110">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="db4a5-111">因此，使用`Microsoft.AspNetCore.All`中繼套件不需要任何進一步的動作，若要使瀏覽器連結可供使用。</span><span class="sxs-lookup"><span data-stu-id="db4a5-111">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="db4a5-112">ASP.NET Core 1.x **Web 應用程式**專案範本有的套件參考[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)封裝。</span><span class="sxs-lookup"><span data-stu-id="db4a5-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="db4a5-113">**空**或是**Web API**範本專案會要求您新增的套件參考`Microsoft.VisualStudio.Web.BrowserLink`。</span><span class="sxs-lookup"><span data-stu-id="db4a5-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="db4a5-114">因為這是 Visual Studio 功能，最簡單的方式將封裝加入**空**或是**Web API**範本專案是以開啟**Package Manager Console** (**檢視** > **其他 Windows** > **Package Manager Console**)，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="db4a5-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="db4a5-115">或者，您可以使用**NuGet 套件管理員**。</span><span class="sxs-lookup"><span data-stu-id="db4a5-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="db4a5-116">中的專案名稱上按一下滑鼠右鍵**方案總管**，然後選擇**管理 NuGet 套件**:</span><span class="sxs-lookup"><span data-stu-id="db4a5-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![開啟 NuGet 套件管理員](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="db4a5-118">尋找並安裝套件：</span><span class="sxs-lookup"><span data-stu-id="db4a5-118">Find and install the package:</span></span>

![新增套件使用 NuGet 封裝管理員](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="db4a5-120">組態</span><span class="sxs-lookup"><span data-stu-id="db4a5-120">Configuration</span></span>

<span data-ttu-id="db4a5-121">在 `Startup.Configure` 方法中：</span><span class="sxs-lookup"><span data-stu-id="db4a5-121">In the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="db4a5-122">程式碼通常是內部`if`區塊可讓瀏覽器連結只在開發環境中，如下所示：</span><span class="sxs-lookup"><span data-stu-id="db4a5-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="db4a5-123">如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="db4a5-123">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="db4a5-124">如何使用瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="db4a5-124">How to use Browser Link</span></span>

<span data-ttu-id="db4a5-125">當您有 ASP.NET Core 專案開啟時，Visual Studio 顯示瀏覽器連結工具列控制項旁**偵錯目標**工具列控制項：</span><span class="sxs-lookup"><span data-stu-id="db4a5-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![瀏覽器連結下拉式選單](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="db4a5-127">從瀏覽器連結 工具列控制項中，您可以：</span><span class="sxs-lookup"><span data-stu-id="db4a5-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="db4a5-128">重新整理 web 應用程式在數個瀏覽器中的一次。</span><span class="sxs-lookup"><span data-stu-id="db4a5-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="db4a5-129">開啟**瀏覽器連結儀表板**。</span><span class="sxs-lookup"><span data-stu-id="db4a5-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="db4a5-130">啟用或停用**瀏覽器連結**。</span><span class="sxs-lookup"><span data-stu-id="db4a5-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="db4a5-131">注意:在 Visual Studio 2017 (15.3) 中的預設會停用瀏覽器連結。</span><span class="sxs-lookup"><span data-stu-id="db4a5-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="db4a5-132">啟用或停用[CSS 自動同步](#enable-or-disable-css-auto-sync)。</span><span class="sxs-lookup"><span data-stu-id="db4a5-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="db4a5-133">某些 Visual Studio 外掛程式，最值得注意的是*Web 延伸模組組件 2015年*並*Web 延伸模組組件 2017年*，針對瀏覽器連結，提供擴充的功能，但某些額外的功能不適用於 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="db4a5-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="db4a5-134">一次重新整理在數個瀏覽器中的 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="db4a5-134">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="db4a5-135">若要選擇啟動專案時要啟動的單一網頁瀏覽器，請使用 [偵錯目標] 工具列控制項中的下拉式選單：</span><span class="sxs-lookup"><span data-stu-id="db4a5-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![F5 下拉式選單](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="db4a5-137">若要一次開啟多個瀏覽器，請從相同的下拉式清單選擇 [瀏覽] 。</span><span class="sxs-lookup"><span data-stu-id="db4a5-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="db4a5-138">按住 CTRL 鍵以選取您想要的瀏覽器，然後按一下 [瀏覽]：</span><span class="sxs-lookup"><span data-stu-id="db4a5-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![一次開啟許多瀏覽器](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="db4a5-140">以下是顯示 Visual Studio 開啟 Index 檢視和兩個開啟瀏覽器的螢幕擷取畫面：</span><span class="sxs-lookup"><span data-stu-id="db4a5-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![同步處理兩個瀏覽器範例](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="db4a5-142">將滑鼠游標停留在 [瀏覽器連結] 工具列控制項上方以查看連結到專案的瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="db4a5-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![暫留時的秘訣](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="db4a5-144">變更 Index 檢視，當您按一下 [瀏覽器連結] 重新整理按鈕時，將更新所有連結的瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="db4a5-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![瀏覽器-同步處理-至-變更](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="db4a5-146">瀏覽器連結也可以搭配您從 Visual Studio 外部啟動，然後瀏覽至應用程式 URL 的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="db4a5-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="db4a5-147">瀏覽器連結儀表板</span><span class="sxs-lookup"><span data-stu-id="db4a5-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="db4a5-148">從瀏覽器連結 下拉式功能表來管理連線開啟的瀏覽器中開啟瀏覽器連結儀表板：</span><span class="sxs-lookup"><span data-stu-id="db4a5-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="db4a5-150">如果連線沒有瀏覽器，您可以選取來啟動非偵錯工作階段*瀏覽器中的檢視*連結：</span><span class="sxs-lookup"><span data-stu-id="db4a5-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="db4a5-152">否則，連線的瀏覽器會顯示每個瀏覽器會顯示頁面的路徑：</span><span class="sxs-lookup"><span data-stu-id="db4a5-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="db4a5-154">如有需要，您可以按一下列出的瀏覽器的名稱，以重新整理該單一瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="db4a5-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="db4a5-155">啟用或停用瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="db4a5-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="db4a5-156">當您重新啟用瀏覽器連結停用它之後時，您必須重新整理瀏覽器再重新連線。</span><span class="sxs-lookup"><span data-stu-id="db4a5-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="db4a5-157">啟用或停用 CSS 自動同步處理</span><span class="sxs-lookup"><span data-stu-id="db4a5-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="db4a5-158">啟用 CSS 自動同步處理時，CSS 檔案中進行任何變更時，會自動重新整理連線的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="db4a5-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="db4a5-159">它的運作方式</span><span class="sxs-lookup"><span data-stu-id="db4a5-159">How it works</span></span>

<span data-ttu-id="db4a5-160">瀏覽器連結會使用 SignalR 來建立 Visual Studio 和瀏覽器之間的通訊通道。</span><span class="sxs-lookup"><span data-stu-id="db4a5-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="db4a5-161">啟用瀏覽器連結時，Visual Studio 就會作為多個用戶端 （瀏覽器） 可以連線到 SignalR 伺服器。</span><span class="sxs-lookup"><span data-stu-id="db4a5-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="db4a5-162">瀏覽器連結也會註冊在 ASP.NET Core 要求管線的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="db4a5-162">Browser Link also registers a middleware component in the ASP.NET Core request pipeline.</span></span> <span data-ttu-id="db4a5-163">此元件會插入特殊`<script>`到每一個網頁要求從伺服器中的參考。</span><span class="sxs-lookup"><span data-stu-id="db4a5-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="db4a5-164">您可以選取，以查看指令碼參考**檢視原始檔**在瀏覽器，並向下捲動到結尾`<body>`標記的內容：</span><span class="sxs-lookup"><span data-stu-id="db4a5-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="db4a5-165">您的原始程式檔不會被修改。</span><span class="sxs-lookup"><span data-stu-id="db4a5-165">Your source files aren't modified.</span></span> <span data-ttu-id="db4a5-166">中介層元件會以動態方式插入指令碼參考。</span><span class="sxs-lookup"><span data-stu-id="db4a5-166">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="db4a5-167">由於瀏覽器端的程式碼全部都是 JavaScript，它適用於 SignalR 支援的所有瀏覽器，而不需要瀏覽器外掛程式。</span><span class="sxs-lookup"><span data-stu-id="db4a5-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
