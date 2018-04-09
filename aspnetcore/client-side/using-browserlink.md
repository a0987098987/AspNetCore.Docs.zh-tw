---
title: 在 ASP.NET Core 瀏覽器連結
author: ncarandini
description: 說明瀏覽器連結的連結與一或多個 web 瀏覽器的開發環境的 Visual Studio 功能的方式。
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-browserlink
ms.openlocfilehash: a75a896dd7ebc488e3e9344ec705c24201924375
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="2b886-103">在 ASP.NET Core 瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="2b886-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="2b886-104">由[Nicolò Carandini](https://github.com/ncarandini)， [Mike Wasson](https://github.com/MikeWasson)，和[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2b886-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="2b886-105">瀏覽器連結是在 Visual Studio 開發環境和一或多個網頁瀏覽器之間的通訊通道所建立的功能。</span><span class="sxs-lookup"><span data-stu-id="2b886-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="2b886-106">您可以使用瀏覽器連結，以重新整理 web 應用程式在數個瀏覽器中的，這是適用於跨瀏覽器測試。</span><span class="sxs-lookup"><span data-stu-id="2b886-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="2b886-107">瀏覽器連結安裝</span><span class="sxs-lookup"><span data-stu-id="2b886-107">Browser Link setup</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2b886-108">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2b886-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2b886-109">ASP.NET Core 2.x **Web 應用程式**，**空**，和**Web API**範本專案使用[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)中繼套件，其中包含的封裝參考[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)。</span><span class="sxs-lookup"><span data-stu-id="2b886-109">The ASP.NET Core 2.x **Web Application**, **Empty**, and **Web API** template projects use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) meta-package, which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="2b886-110">因此，使用`Microsoft.AspNetCore.All`中繼套件不需要任何進一步的動作，讓瀏覽器連結可供使用。</span><span class="sxs-lookup"><span data-stu-id="2b886-110">Therefore, using the `Microsoft.AspNetCore.All` meta-package requires no further action to make Browser Link available for use.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2b886-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2b886-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2b886-112">ASP.NET Core 1.x **Web 應用程式**專案範本內含的封裝參考[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)封裝。</span><span class="sxs-lookup"><span data-stu-id="2b886-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="2b886-113">**空**或**Web API**範本專案要求您新增的封裝參考`Microsoft.VisualStudio.Web.BrowserLink`。</span><span class="sxs-lookup"><span data-stu-id="2b886-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="2b886-114">因為這是 Visual Studio 功能，最簡單的方式將此封裝加入**空**或**Web API**範本專案是開啟**Package Manager Console** (**檢視** > **其他視窗** > **Package Manager Console**)，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2b886-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="2b886-115">或者，您可以使用**NuGet 套件管理員**。</span><span class="sxs-lookup"><span data-stu-id="2b886-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="2b886-116">以滑鼠右鍵按一下專案名稱中的**方案總管 中**選擇**管理 NuGet 封裝**:</span><span class="sxs-lookup"><span data-stu-id="2b886-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![開啟 NuGet 封裝管理員](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="2b886-118">尋找並安裝套件：</span><span class="sxs-lookup"><span data-stu-id="2b886-118">Find and install the package:</span></span>

![新增封裝使用 NuGet 封裝管理員](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a><span data-ttu-id="2b886-120">組態</span><span class="sxs-lookup"><span data-stu-id="2b886-120">Configuration</span></span>

<span data-ttu-id="2b886-121">在`Configure`方法*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="2b886-121">In the `Configure` method of the *Startup.cs* file:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="2b886-122">程式碼通常是內部`if`區塊可讓瀏覽器連結只在開發環境中，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2b886-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="2b886-123">如需詳細資訊，請參閱[搭配多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="2b886-123">For more information, see [Work with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="2b886-124">如何使用瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="2b886-124">How to use Browser Link</span></span>

<span data-ttu-id="2b886-125">當您開啟 ASP.NET Core 專案時，Visual Studio 顯示瀏覽器連結工具列控制項旁**偵錯目標**工具列控制項：</span><span class="sxs-lookup"><span data-stu-id="2b886-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![瀏覽器連結下拉式功能表](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="2b886-127">從瀏覽器連結工具列控制項中，您可以：</span><span class="sxs-lookup"><span data-stu-id="2b886-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="2b886-128">重新整理 web 應用程式在數個瀏覽器中的一次。</span><span class="sxs-lookup"><span data-stu-id="2b886-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="2b886-129">開啟**瀏覽器連結儀表板**。</span><span class="sxs-lookup"><span data-stu-id="2b886-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="2b886-130">啟用或停用**瀏覽器連結**。</span><span class="sxs-lookup"><span data-stu-id="2b886-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="2b886-131">附註： 預設會在 Visual Studio 2017 (15.3)，停用瀏覽器連結。</span><span class="sxs-lookup"><span data-stu-id="2b886-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="2b886-132">啟用或停用[CSS 自動同步](#enable-or-disable-css-auto-sync)。</span><span class="sxs-lookup"><span data-stu-id="2b886-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="2b886-133">某些 Visual Studio 外掛程式，最值得注意的是*Web 擴充功能組件 2015年*和*Web 擴充功能組件 2017年*、 瀏覽器連結提供擴充的功能，但與 ASP 不搭配使用的一些其他功能.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="2b886-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="2b886-134">Web 應用程式在數個瀏覽器中的一次重新整理</span><span class="sxs-lookup"><span data-stu-id="2b886-134">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="2b886-135">若要選擇單一網頁瀏覽器啟動專案時，使用下拉式功能表中的**偵錯目標**工具列控制項：</span><span class="sxs-lookup"><span data-stu-id="2b886-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![F5 下拉式功能表](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="2b886-137">若要一次開啟多個瀏覽器，請選擇**與瀏覽...**相同的下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="2b886-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="2b886-138">按住 CTRL 鍵以選取您想，瀏的覽器，然後按一下 **瀏覽**:</span><span class="sxs-lookup"><span data-stu-id="2b886-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![一次開啟許多瀏覽器](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="2b886-140">以下是顯示與索引檢視的 Visual Studio 開啟螢幕擷取畫面和兩個開啟的瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="2b886-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![同步處理兩個瀏覽器範例](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="2b886-142">將滑鼠停留在瀏覽器連結工具列控制項，若要查看連接到專案的瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="2b886-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![將滑鼠停留提示](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="2b886-144">變更索引檢視，並按一下 瀏覽器連結的 重新整理 按鈕時，系統會更新所有已連線的瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="2b886-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![browsers-sync-to-changes](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="2b886-146">瀏覽器連結也可以搭配您從 Visual Studio 外部啟動，並瀏覽至應用程式 URL 的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="2b886-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="2b886-147">瀏覽器連結儀表板</span><span class="sxs-lookup"><span data-stu-id="2b886-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="2b886-148">從瀏覽器連結下拉式功能表來管理與開啟的瀏覽器連線，請開啟瀏覽器連結儀表板：</span><span class="sxs-lookup"><span data-stu-id="2b886-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="2b886-150">如果連線沒有瀏覽器，您可以選取來啟動非偵錯工作階段*瀏覽器中的檢視*連結：</span><span class="sxs-lookup"><span data-stu-id="2b886-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="2b886-152">否則，已連線的瀏覽器會顯示每個瀏覽器顯示頁面的路徑：</span><span class="sxs-lookup"><span data-stu-id="2b886-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="2b886-154">如果需要，您可以按一下列出的瀏覽器的名稱，以重新整理該單一瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="2b886-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="2b886-155">啟用或停用瀏覽器連結</span><span class="sxs-lookup"><span data-stu-id="2b886-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="2b886-156">當您重新啟用瀏覽器連結停用它之後時，您必須重新整理瀏覽器再重新連線。</span><span class="sxs-lookup"><span data-stu-id="2b886-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="2b886-157">啟用或停用 CSS 自動同步處理</span><span class="sxs-lookup"><span data-stu-id="2b886-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="2b886-158">啟用 CSS 自動同步處理時，CSS 檔案中進行任何變更時，會自動重新整理連接的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="2b886-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="2b886-159">它的運作狀況如何？</span><span class="sxs-lookup"><span data-stu-id="2b886-159">How does it work?</span></span>

<span data-ttu-id="2b886-160">瀏覽器連結會使用 SignalR 建立 Visual Studio 和瀏覽器之間的通訊通道。</span><span class="sxs-lookup"><span data-stu-id="2b886-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="2b886-161">啟用瀏覽器連結時，Visual Studio 就會當做多個用戶端 （瀏覽器使用） 可以連接到的 SignalR 伺服器。</span><span class="sxs-lookup"><span data-stu-id="2b886-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="2b886-162">瀏覽器連結也會註冊 ASP.NET 要求管線中的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="2b886-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="2b886-163">此元件會插入特殊`<script>`到每個頁面要求來自伺服器的參考。</span><span class="sxs-lookup"><span data-stu-id="2b886-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="2b886-164">您可以看到選取的指令碼參考**檢視原始檔**瀏覽器和向下捲動到結尾`<body>`標記的內容：</span><span class="sxs-lookup"><span data-stu-id="2b886-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="2b886-165">不修改原始程式檔。</span><span class="sxs-lookup"><span data-stu-id="2b886-165">Your source files aren't modified.</span></span> <span data-ttu-id="2b886-166">中介軟體元件會以動態方式插入指令碼參考。</span><span class="sxs-lookup"><span data-stu-id="2b886-166">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="2b886-167">因為瀏覽器端程式碼是所有 JavaScript，所以它適用於所有瀏覽器 SignalR 支援而不需要瀏覽器外掛程式。</span><span class="sxs-lookup"><span data-stu-id="2b886-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
