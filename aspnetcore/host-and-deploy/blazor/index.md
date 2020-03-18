---
title: 裝載和部署 ASP.NET Core Blazor
author: guardrex
description: 探索如何裝載和部署 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/index
ms.openlocfilehash: ddf70da29a82d462422c1bdf74ff45b92bb10b56
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434261"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="95e94-103">裝載及部署 ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="95e94-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="95e94-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="95e94-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="95e94-105">發佈應用程式</span><span class="sxs-lookup"><span data-stu-id="95e94-105">Publish the app</span></span>

<span data-ttu-id="95e94-106">應用程式會在發行組態中發佈以供部署。</span><span class="sxs-lookup"><span data-stu-id="95e94-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="95e94-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95e94-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="95e94-108">從巡覽列中選取 [建置] > [發佈 {應用程式}]。</span><span class="sxs-lookup"><span data-stu-id="95e94-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="95e94-109">選取「發佈目標」。</span><span class="sxs-lookup"><span data-stu-id="95e94-109">Select the *publish target*.</span></span> <span data-ttu-id="95e94-110">若要在本機發佈，請選取 [資料夾]。</span><span class="sxs-lookup"><span data-stu-id="95e94-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="95e94-111">接受 [選擇資料夾] 欄位中的預設位置，或指定不同的位置。</span><span class="sxs-lookup"><span data-stu-id="95e94-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="95e94-112">選取 [發佈] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="95e94-112">Select the **Publish** button.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="95e94-113">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="95e94-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="95e94-114">使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，搭配發行組態來發佈應用程式：</span><span class="sxs-lookup"><span data-stu-id="95e94-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```dotnetcli
dotnet publish -c Release
```

---

<span data-ttu-id="95e94-115">發佈應用程式會觸發專案相依性的[還原](/dotnet/core/tools/dotnet-restore)並[建置](/dotnet/core/tools/dotnet-build)專案，然後才會建立資產以供部署。</span><span class="sxs-lookup"><span data-stu-id="95e94-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="95e94-116">在建置程序中，會移除未使用的方法和組件，減少應用程式的下載大小和載入時間。</span><span class="sxs-lookup"><span data-stu-id="95e94-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="95e94-117">發行位置：</span><span class="sxs-lookup"><span data-stu-id="95e94-117">Publish locations:</span></span>

* Blazor<span data-ttu-id="95e94-118"> WebAssembly</span><span class="sxs-lookup"><span data-stu-id="95e94-118"> WebAssembly</span></span>
  * <span data-ttu-id="95e94-119">獨立 &ndash; 應用程式會發佈至 */BIN/RELEASE/{TARGET FRAMEWORK}/publish/wwwroot*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="95e94-119">Standalone &ndash; The app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder.</span></span> <span data-ttu-id="95e94-120">若要將應用程式部署為靜態網站，請將*wwwroot*資料夾的內容複寫到靜態網站主機。</span><span class="sxs-lookup"><span data-stu-id="95e94-120">To deploy the app as a static site, copy the contents of the *wwwroot* folder to the static site host.</span></span>
  * <span data-ttu-id="95e94-121">裝載 &ndash; 用戶端 Blazor WebAssembly 應用程式會發佈到伺服器應用程式的 */BIN/RELEASE/{TARGET FRAMEWORK}/publish/wwwroot*資料夾，以及伺服器應用程式的任何其他靜態 web 資產。</span><span class="sxs-lookup"><span data-stu-id="95e94-121">Hosted &ndash; The client Blazor WebAssembly app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder of the server app, along with any other static web assets of the server app.</span></span> <span data-ttu-id="95e94-122">將*publish*資料夾的內容部署至主機。</span><span class="sxs-lookup"><span data-stu-id="95e94-122">Deploy the contents of the *publish* folder to the host.</span></span>
* <span data-ttu-id="95e94-123">應用程式 &ndash; Blazor 伺服器會發佈到 */BIN/RELEASE/{TARGET FRAMEWORK}/publish*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="95e94-123">Blazor Server &ndash; The app is published into the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="95e94-124">將*publish*資料夾的內容部署至主機。</span><span class="sxs-lookup"><span data-stu-id="95e94-124">Deploy the contents of the *publish* folder to the host.</span></span>

<span data-ttu-id="95e94-125">該資料夾中的資產均會部署至 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="95e94-125">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="95e94-126">部署可能是手動或自動的程序，視使用中的開發工具而定。</span><span class="sxs-lookup"><span data-stu-id="95e94-126">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="95e94-127">應用程式基底路徑</span><span class="sxs-lookup"><span data-stu-id="95e94-127">App base path</span></span>

<span data-ttu-id="95e94-128">*應用程式基底路徑*是應用程式的根 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="95e94-128">The *app base path* is the app's root URL path.</span></span> <span data-ttu-id="95e94-129">請考慮下列 ASP.NET Core 應用程式和 Blazor 子應用程式：</span><span class="sxs-lookup"><span data-stu-id="95e94-129">Consider the following ASP.NET Core app and Blazor sub-app:</span></span>

* <span data-ttu-id="95e94-130">ASP.NET Core 應用程式的名稱 `MyApp`：</span><span class="sxs-lookup"><span data-stu-id="95e94-130">The ASP.NET Core app is named `MyApp`:</span></span>
  * <span data-ttu-id="95e94-131">應用程式實際位於*d：/MyApp*。</span><span class="sxs-lookup"><span data-stu-id="95e94-131">The app physically resides at *d:/MyApp*.</span></span>
  * <span data-ttu-id="95e94-132">`https://www.contoso.com/{MYAPP RESOURCE}`收到要求。</span><span class="sxs-lookup"><span data-stu-id="95e94-132">Requests are received at `https://www.contoso.com/{MYAPP RESOURCE}`.</span></span>
* <span data-ttu-id="95e94-133">名為 `CoolApp` Blazor 應用程式是 `MyApp`的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="95e94-133">A Blazor app named `CoolApp` is a sub-app of `MyApp`:</span></span>
  * <span data-ttu-id="95e94-134">子應用程式實際上位於*d：/MyApp/CoolApp*。</span><span class="sxs-lookup"><span data-stu-id="95e94-134">The sub-app physically resides at *d:/MyApp/CoolApp*.</span></span>
  * <span data-ttu-id="95e94-135">`https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`收到要求。</span><span class="sxs-lookup"><span data-stu-id="95e94-135">Requests are received at `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span></span>

<span data-ttu-id="95e94-136">若未指定 `CoolApp`的額外設定，此案例中的子應用程式就不會知道其位於伺服器上的位置。</span><span class="sxs-lookup"><span data-stu-id="95e94-136">Without specifying additional configuration for `CoolApp`, the sub-app in this scenario has no knowledge of where it resides on the server.</span></span> <span data-ttu-id="95e94-137">例如，應用程式無法在不知道其位於相對 URL 路徑 `/CoolApp/`的情況下，為其資源建立正確的相對 Url。</span><span class="sxs-lookup"><span data-stu-id="95e94-137">For example, the app can't construct correct relative URLs to its resources without knowing that it resides at the relative URL path `/CoolApp/`.</span></span>

<span data-ttu-id="95e94-138">若要為 Blazor 應用程式的基底路徑 `https://www.contoso.com/CoolApp/`提供設定，`<base>` 標記的 `href` 屬性會設為*Pages/_Host. cshtml*檔案（Blazor 伺服器）或*wwwroot/index.html*檔（Blazor WebAssembly）中的相對根路徑：</span><span class="sxs-lookup"><span data-stu-id="95e94-138">To provide configuration for the Blazor app's base path of `https://www.contoso.com/CoolApp/`, the `<base>` tag's `href` attribute is set to the relative root path in the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly):</span></span>

```html
<base href="/CoolApp/">
```

Blazor<span data-ttu-id="95e94-139"> 伺服器應用程式會在 `Startup.Configure`的應用程式要求管線中呼叫 <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*>，以額外設定伺服器端基底路徑：</span><span class="sxs-lookup"><span data-stu-id="95e94-139"> Server apps additionally set the server-side base path by calling <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> in the app's request pipeline of `Startup.Configure`:</span></span>

```csharp
app.UsePathBase("/CoolApp");
```

<span data-ttu-id="95e94-140">藉由提供相對的 URL 路徑，不在根目錄中的元件可以針對應用程式的根路徑來建立 Url。</span><span class="sxs-lookup"><span data-stu-id="95e94-140">By providing the relative URL path, a component that isn't in the root directory can construct URLs relative to the app's root path.</span></span> <span data-ttu-id="95e94-141">不同目錄結構層級的元件可以在整個應用程式的位置建立其他資源的連結。</span><span class="sxs-lookup"><span data-stu-id="95e94-141">Components at different levels of the directory structure can build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="95e94-142">應用程式基底路徑也會用來攔截選取的超連結，其中連結的 `href` 目標是在應用程式基底路徑 URI 空間內。</span><span class="sxs-lookup"><span data-stu-id="95e94-142">The app base path is also used to intercept selected hyperlinks where the `href` target of the link is within the app base path URI space.</span></span> <span data-ttu-id="95e94-143">Blazor 路由器會處理內部導覽。</span><span class="sxs-lookup"><span data-stu-id="95e94-143">The Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="95e94-144">在許多裝載案例中，應用程式的相對 URL 路徑是應用程式的根目錄。</span><span class="sxs-lookup"><span data-stu-id="95e94-144">In many hosting scenarios, the relative URL path to the app is the root of the app.</span></span> <span data-ttu-id="95e94-145">在這些情況下，應用程式的相對 URL 基底路徑是正斜線（`<base href="/" />`），這是 Blazor 應用程式的預設設定。</span><span class="sxs-lookup"><span data-stu-id="95e94-145">In these cases, the app's relative URL base path is a forward slash (`<base href="/" />`), which is the default configuration for a Blazor app.</span></span> <span data-ttu-id="95e94-146">在其他裝載案例（例如 GitHub 頁面和 IIS 子應用程式）中，應用程式基底路徑必須設定為應用程式的伺服器相對 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="95e94-146">In other hosting scenarios, such as GitHub Pages and IIS sub-apps, the app base path must be set to the server's relative URL path of the app.</span></span>

<span data-ttu-id="95e94-147">若要設定應用程式的基底路徑，請更新*Pages/_Host. cshtml*檔案（Blazor 伺服器）或*wwwroot/index.html*檔（Blazor WebAssembly）的 `<head>` 標記元素內的 `<base>` 標記。</span><span class="sxs-lookup"><span data-stu-id="95e94-147">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly).</span></span> <span data-ttu-id="95e94-148">將 `href` 屬性值設定為 `/{RELATIVE URL PATH}/` （需要尾端斜線），其中 `{RELATIVE URL PATH}` 是應用程式的完整相對 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="95e94-148">Set the `href` attribute value to `/{RELATIVE URL PATH}/` (the trailing slash is required), where `{RELATIVE URL PATH}` is the app's full relative URL path.</span></span>

<span data-ttu-id="95e94-149">針對具有非根相對 URL 路徑的 Blazor WebAssembly 應用程式（例如 `<base href="/CoolApp/">`），應用程式*在本機執行時*，無法找到其資源。</span><span class="sxs-lookup"><span data-stu-id="95e94-149">For an Blazor WebAssembly app with a non-root relative URL path (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="95e94-150">若要在本機開發和測試期間解決這個問題，您可以提供「基底路徑」引數，讓它在執行時符合 `href` 標籤的 `<base>` 值。</span><span class="sxs-lookup"><span data-stu-id="95e94-150">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span> <span data-ttu-id="95e94-151">不要包含尾端斜線。</span><span class="sxs-lookup"><span data-stu-id="95e94-151">Don't include a trailing slash.</span></span> <span data-ttu-id="95e94-152">若要在本機執行應用程式時傳遞 path base 引數，請使用 `--pathbase` 選項，從應用程式的目錄執行 `dotnet run` 命令：</span><span class="sxs-lookup"><span data-stu-id="95e94-152">To pass the path base argument when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

<span data-ttu-id="95e94-153">針對具有 `/CoolApp/` （`<base href="/CoolApp/">`）之相對 URL 路徑的 Blazor WebAssembly 應用程式，命令為：</span><span class="sxs-lookup"><span data-stu-id="95e94-153">For a Blazor WebAssembly app with a relative URL path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```dotnetcli
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="95e94-154">Blazor WebAssembly 應用程式會在 `http://localhost:port/CoolApp`本機回應。</span><span class="sxs-lookup"><span data-stu-id="95e94-154">The Blazor WebAssembly app responds locally at `http://localhost:port/CoolApp`.</span></span>

## <a name="deployment"></a><span data-ttu-id="95e94-155">部署</span><span class="sxs-lookup"><span data-stu-id="95e94-155">Deployment</span></span>

<span data-ttu-id="95e94-156">如需部署指導方針，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="95e94-156">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
