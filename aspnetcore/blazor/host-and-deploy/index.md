---
title: 裝載和部署 ASP.NET CoreBlazor
author: guardrex
description: 探索如何裝載和部署 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/15/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/host-and-deploy/index
ms.openlocfilehash: 77202cd60d357c27237cdb925e0adc00e66d2e56
ms.sourcegitcommit: 6fb27ea41a92f6d0e91dfd0eba905d2ac1a707f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2020
ms.locfileid: "86407706"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="97012-103">裝載和部署 ASP.NET CoreBlazor</span><span class="sxs-lookup"><span data-stu-id="97012-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="97012-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="97012-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="97012-105">發佈應用程式</span><span class="sxs-lookup"><span data-stu-id="97012-105">Publish the app</span></span>

<span data-ttu-id="97012-106">應用程式會在發行組態中發佈以供部署。</span><span class="sxs-lookup"><span data-stu-id="97012-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="97012-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="97012-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="97012-108">**Build**  >  從巡覽列選取 [組建] [**發佈 {應用程式}** ]。</span><span class="sxs-lookup"><span data-stu-id="97012-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="97012-109">選取「發佈目標」\*\*。</span><span class="sxs-lookup"><span data-stu-id="97012-109">Select the *publish target*.</span></span> <span data-ttu-id="97012-110">若要在本機發佈，請選取 [資料夾]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="97012-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="97012-111">接受 [選擇資料夾]\*\*\*\* 欄位中的預設位置，或指定不同的位置。</span><span class="sxs-lookup"><span data-stu-id="97012-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="97012-112">選取 [] **`Publish`** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="97012-112">Select the **`Publish`** button.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="97012-113">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="97012-113">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="97012-114">選取 [**組建**] [  >  **發行至資料夾**]。</span><span class="sxs-lookup"><span data-stu-id="97012-114">Select **Build** > **Publish to Folder**.</span></span>
1. <span data-ttu-id="97012-115">確認要接收已發佈資產的資料夾，然後選取 [] **`Publish`** 。</span><span class="sxs-lookup"><span data-stu-id="97012-115">Confirm the folder to receive the published assets and select **`Publish`**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="97012-116">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="97012-116">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="97012-117">使用 [`dotnet publish`](/dotnet/core/tools/dotnet-publish) 命令來發佈具有發行設定的應用程式：</span><span class="sxs-lookup"><span data-stu-id="97012-117">Use the [`dotnet publish`](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```dotnetcli
dotnet publish -c Release
```

---

<span data-ttu-id="97012-118">發佈應用程式會觸發專案相依性的[還原](/dotnet/core/tools/dotnet-restore)並[建置](/dotnet/core/tools/dotnet-build)專案，然後才會建立資產以供部署。</span><span class="sxs-lookup"><span data-stu-id="97012-118">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="97012-119">在建置程序中，會移除未使用的方法和組件，減少應用程式的下載大小和載入時間。</span><span class="sxs-lookup"><span data-stu-id="97012-119">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="97012-120">發行位置：</span><span class="sxs-lookup"><span data-stu-id="97012-120">Publish locations:</span></span>

* Blazor WebAssembly
  * <span data-ttu-id="97012-121">獨立：應用程式會發佈到 `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="97012-121">Standalone: The app is published into the `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` folder.</span></span> <span data-ttu-id="97012-122">若要將應用程式部署為靜態網站，請將該資料夾的內容複寫 `wwwroot` 到靜態網站主機。</span><span class="sxs-lookup"><span data-stu-id="97012-122">To deploy the app as a static site, copy the contents of the `wwwroot` folder to the static site host.</span></span>
  * <span data-ttu-id="97012-123">已裝載：用戶端 Blazor WebAssembly 應用程式會發佈到 `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` 伺服器應用程式的資料夾，以及伺服器應用程式的任何其他靜態 web 資產。</span><span class="sxs-lookup"><span data-stu-id="97012-123">Hosted: The client Blazor WebAssembly app is published into the `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` folder of the server app, along with any other static web assets of the server app.</span></span> <span data-ttu-id="97012-124">將資料夾的內容部署 `publish` 至主機。</span><span class="sxs-lookup"><span data-stu-id="97012-124">Deploy the contents of the `publish` folder to the host.</span></span>
* Blazor Server<span data-ttu-id="97012-125">：應用程式會發佈到 `/bin/Release/{TARGET FRAMEWORK}/publish` 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="97012-125">: The app is published into the `/bin/Release/{TARGET FRAMEWORK}/publish` folder.</span></span> <span data-ttu-id="97012-126">將資料夾的內容部署 `publish` 至主機。</span><span class="sxs-lookup"><span data-stu-id="97012-126">Deploy the contents of the `publish` folder to the host.</span></span>

<span data-ttu-id="97012-127">該資料夾中的資產均會部署至 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="97012-127">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="97012-128">部署可能是手動或自動的程序，視使用中的開發工具而定。</span><span class="sxs-lookup"><span data-stu-id="97012-128">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="97012-129">應用程式基底路徑</span><span class="sxs-lookup"><span data-stu-id="97012-129">App base path</span></span>

<span data-ttu-id="97012-130">*應用程式基底路徑*是應用程式的根 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="97012-130">The *app base path* is the app's root URL path.</span></span> <span data-ttu-id="97012-131">請考慮下列 ASP.NET Core 應用程式和 Blazor 子應用程式：</span><span class="sxs-lookup"><span data-stu-id="97012-131">Consider the following ASP.NET Core app and Blazor sub-app:</span></span>

* <span data-ttu-id="97012-132">ASP.NET Core 應用程式的名稱為 `MyApp` ：</span><span class="sxs-lookup"><span data-stu-id="97012-132">The ASP.NET Core app is named `MyApp`:</span></span>
  * <span data-ttu-id="97012-133">應用程式實際上位於 `d:/MyApp` 。</span><span class="sxs-lookup"><span data-stu-id="97012-133">The app physically resides at `d:/MyApp`.</span></span>
  * <span data-ttu-id="97012-134">會在收到要求 `https://www.contoso.com/{MYAPP RESOURCE}` 。</span><span class="sxs-lookup"><span data-stu-id="97012-134">Requests are received at `https://www.contoso.com/{MYAPP RESOURCE}`.</span></span>
* <span data-ttu-id="97012-135">名為的 Blazor 應用程式 `CoolApp` 是的子應用程式 `MyApp` ：</span><span class="sxs-lookup"><span data-stu-id="97012-135">A Blazor app named `CoolApp` is a sub-app of `MyApp`:</span></span>
  * <span data-ttu-id="97012-136">子應用程式實際上位於 `d:/MyApp/CoolApp` 。</span><span class="sxs-lookup"><span data-stu-id="97012-136">The sub-app physically resides at `d:/MyApp/CoolApp`.</span></span>
  * <span data-ttu-id="97012-137">會在收到要求 `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}` 。</span><span class="sxs-lookup"><span data-stu-id="97012-137">Requests are received at `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span></span>

<span data-ttu-id="97012-138">若未指定的其他設定 `CoolApp` ，此案例中的子應用程式就不會知道其位於伺服器上的位置。</span><span class="sxs-lookup"><span data-stu-id="97012-138">Without specifying additional configuration for `CoolApp`, the sub-app in this scenario has no knowledge of where it resides on the server.</span></span> <span data-ttu-id="97012-139">例如，應用程式無法在不知道其位於相對 URL 路徑的情況下，對其資源建立正確的相對 Url `/CoolApp/` 。</span><span class="sxs-lookup"><span data-stu-id="97012-139">For example, the app can't construct correct relative URLs to its resources without knowing that it resides at the relative URL path `/CoolApp/`.</span></span>

<span data-ttu-id="97012-140">若要為 Blazor 應用程式的基底路徑提供 `https://www.contoso.com/CoolApp/` 設定， `<base>` 標記的 `href` 屬性會設為檔案 `Pages/_Host.cshtml` （ Blazor Server ）或檔案 `wwwroot/index.html` （）中的相對根路徑 Blazor WebAssembly ：</span><span class="sxs-lookup"><span data-stu-id="97012-140">To provide configuration for the Blazor app's base path of `https://www.contoso.com/CoolApp/`, the `<base>` tag's `href` attribute is set to the relative root path in the `Pages/_Host.cshtml` file (Blazor Server) or `wwwroot/index.html` file (Blazor WebAssembly):</span></span>

```html
<base href="/CoolApp/">
```

Blazor Server<span data-ttu-id="97012-141">應用程式會 <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> 在的應用程式要求管線中呼叫，以額外設定伺服器端基底路徑 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="97012-141"> apps additionally set the server-side base path by calling <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> in the app's request pipeline of `Startup.Configure`:</span></span>

```csharp
app.UsePathBase("/CoolApp");
```

<span data-ttu-id="97012-142">藉由提供相對的 URL 路徑，不在根目錄中的元件可以針對應用程式的根路徑來建立 Url。</span><span class="sxs-lookup"><span data-stu-id="97012-142">By providing the relative URL path, a component that isn't in the root directory can construct URLs relative to the app's root path.</span></span> <span data-ttu-id="97012-143">不同目錄結構層級的元件可以在整個應用程式的位置建立其他資源的連結。</span><span class="sxs-lookup"><span data-stu-id="97012-143">Components at different levels of the directory structure can build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="97012-144">應用程式基底路徑也會用來攔截選取的超連結，其中 `href` 連結的目標是在應用程式基底路徑 URI 空間內。</span><span class="sxs-lookup"><span data-stu-id="97012-144">The app base path is also used to intercept selected hyperlinks where the `href` target of the link is within the app base path URI space.</span></span> <span data-ttu-id="97012-145">Blazor路由器會處理內部導覽。</span><span class="sxs-lookup"><span data-stu-id="97012-145">The Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="97012-146">在許多裝載案例中，應用程式的相對 URL 路徑是應用程式的根目錄。</span><span class="sxs-lookup"><span data-stu-id="97012-146">In many hosting scenarios, the relative URL path to the app is the root of the app.</span></span> <span data-ttu-id="97012-147">在這些情況下，應用程式的相對 URL 基底路徑是正斜線（ `<base href="/" />` ），這是應用程式的預設設定 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="97012-147">In these cases, the app's relative URL base path is a forward slash (`<base href="/" />`), which is the default configuration for a Blazor app.</span></span> <span data-ttu-id="97012-148">在其他裝載案例（例如 GitHub 頁面和 IIS 子應用程式）中，應用程式基底路徑必須設定為應用程式的伺服器相對 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="97012-148">In other hosting scenarios, such as GitHub Pages and IIS sub-apps, the app base path must be set to the server's relative URL path of the app.</span></span>

<span data-ttu-id="97012-149">若要設定應用程式的基底路徑，請更新檔案 `<base>` `<head>` `Pages/_Host.cshtml` （ Blazor Server ）或檔案 `wwwroot/index.html` （）之標記元素內的標記 Blazor WebAssembly 。</span><span class="sxs-lookup"><span data-stu-id="97012-149">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the `Pages/_Host.cshtml` file (Blazor Server) or `wwwroot/index.html` file (Blazor WebAssembly).</span></span> <span data-ttu-id="97012-150">將 `href` 屬性值設定為 `/{RELATIVE URL PATH}/` （需要尾端斜線），其中 `{RELATIVE URL PATH}` 是應用程式的完整相對 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="97012-150">Set the `href` attribute value to `/{RELATIVE URL PATH}/` (the trailing slash is required), where `{RELATIVE URL PATH}` is the app's full relative URL path.</span></span>

<span data-ttu-id="97012-151">針對 Blazor WebAssembly 具有非根相對 URL 路徑的應用程式（例如 `<base href="/CoolApp/">` ），在*本機執行時*，應用程式無法找到其資源。</span><span class="sxs-lookup"><span data-stu-id="97012-151">For an Blazor WebAssembly app with a non-root relative URL path (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="97012-152">若要在本機開發和測試期間解決這個問題，您可以提供「基底路徑」\*\* 引數，讓它在執行時符合 `<base>` 標籤的 `href` 值。</span><span class="sxs-lookup"><span data-stu-id="97012-152">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span> <span data-ttu-id="97012-153">不要包含尾端斜線。</span><span class="sxs-lookup"><span data-stu-id="97012-153">Don't include a trailing slash.</span></span> <span data-ttu-id="97012-154">若要在本機執行應用程式時傳遞 path base 引數，請 `dotnet run` 使用下列選項從應用程式的目錄執行命令 `--pathbase` ：</span><span class="sxs-lookup"><span data-stu-id="97012-154">To pass the path base argument when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

<span data-ttu-id="97012-155">針對 Blazor WebAssembly 具有（）之相對 URL 路徑的應用程式 `/CoolApp/` `<base href="/CoolApp/">` ，命令為：</span><span class="sxs-lookup"><span data-stu-id="97012-155">For a Blazor WebAssembly app with a relative URL path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```dotnetcli
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="97012-156">Blazor WebAssembly應用程式會在本機回應，網址為 `http://localhost:port/CoolApp` 。</span><span class="sxs-lookup"><span data-stu-id="97012-156">The Blazor WebAssembly app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="97012-157">**Blazor Server設定 `MapFallbackToPage`**</span><span class="sxs-lookup"><span data-stu-id="97012-157">**Blazor Server `MapFallbackToPage` configuration**</span></span>

<span data-ttu-id="97012-158">將下列路徑傳遞至 <xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage%2A> 中的 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="97012-158">Pass the following path to <xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage%2A> in `Startup.Configure`:</span></span>

```csharp
endpoints.MapFallbackToPage("/{RELATIVE PATH}/{**path:nonfile}");
```

<span data-ttu-id="97012-159">預留位置 `{RELATIVE PATH}` 是伺服器上的非根路徑。</span><span class="sxs-lookup"><span data-stu-id="97012-159">The placeholder `{RELATIVE PATH}` is the non-root path on the server.</span></span> <span data-ttu-id="97012-160">例如， `CoolApp` 如果應用程式的非根 URL 為，則為預留位置區段 `https://{HOST}:{PORT}/CoolApp/` ：</span><span class="sxs-lookup"><span data-stu-id="97012-160">For example, `CoolApp` is the placeholder segment if the non-root URL to the app is `https://{HOST}:{PORT}/CoolApp/`):</span></span>

```csharp
endpoints.MapFallbackToPage("/CoolApp/{**path:nonfile}");
```

## <a name="deployment"></a><span data-ttu-id="97012-161">部署</span><span class="sxs-lookup"><span data-stu-id="97012-161">Deployment</span></span>

<span data-ttu-id="97012-162">如需部署指導方針，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="97012-162">For deployment guidance, see the following topics:</span></span>

* <xref:blazor/host-and-deploy/webassembly>
* <xref:blazor/host-and-deploy/server>
