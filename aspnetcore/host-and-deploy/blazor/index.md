---
title: 管理管及部署ASP.NET核心Blazor
author: guardrex
description: 瞭解如何託管和部署Blazor應用。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/index
ms.openlocfilehash: ddf70da29a82d462422c1bdf74ff45b92bb10b56
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79434261"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="64d32-103">裝載及部署 ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="64d32-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="64d32-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="64d32-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="64d32-105">發佈應用程式</span><span class="sxs-lookup"><span data-stu-id="64d32-105">Publish the app</span></span>

<span data-ttu-id="64d32-106">應用程式會在發行組態中發佈以供部署。</span><span class="sxs-lookup"><span data-stu-id="64d32-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="64d32-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64d32-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="64d32-108">從導航欄中選擇**生成** > **發佈 [應用]。**</span><span class="sxs-lookup"><span data-stu-id="64d32-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="64d32-109">選取「發佈目標」\*\*。</span><span class="sxs-lookup"><span data-stu-id="64d32-109">Select the *publish target*.</span></span> <span data-ttu-id="64d32-110">若要在本機發佈，請選取 [資料夾]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="64d32-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="64d32-111">接受 [選擇資料夾]\*\*\*\* 欄位中的預設位置，或指定不同的位置。</span><span class="sxs-lookup"><span data-stu-id="64d32-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="64d32-112">選取 [發佈]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="64d32-112">Select the **Publish** button.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="64d32-113">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="64d32-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="64d32-114">使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，搭配發行組態來發佈應用程式：</span><span class="sxs-lookup"><span data-stu-id="64d32-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```dotnetcli
dotnet publish -c Release
```

---

<span data-ttu-id="64d32-115">發佈應用程式會觸發專案相依性的[還原](/dotnet/core/tools/dotnet-restore)並[建置](/dotnet/core/tools/dotnet-build)專案，然後才會建立資產以供部署。</span><span class="sxs-lookup"><span data-stu-id="64d32-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="64d32-116">在建置程序中，會移除未使用的方法和組件，減少應用程式的下載大小和載入時間。</span><span class="sxs-lookup"><span data-stu-id="64d32-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="64d32-117">發佈位置:</span><span class="sxs-lookup"><span data-stu-id="64d32-117">Publish locations:</span></span>

* Blazor<span data-ttu-id="64d32-118">網路組裝</span><span class="sxs-lookup"><span data-stu-id="64d32-118"> WebAssembly</span></span>
  * <span data-ttu-id="64d32-119">獨立&ndash;應用程式發佈到 */bin/發佈/[目標框架]/發佈/wwwroot*檔夾中。</span><span class="sxs-lookup"><span data-stu-id="64d32-119">Standalone &ndash; The app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder.</span></span> <span data-ttu-id="64d32-120">要將應用部署為靜態網站,請將*wwwroot*資料夾的內容複製到靜態網站主機。</span><span class="sxs-lookup"><span data-stu-id="64d32-120">To deploy the app as a static site, copy the contents of the *wwwroot* folder to the static site host.</span></span>
  * <span data-ttu-id="64d32-121">託管&ndash;客戶Blazor端 WebAssembly 應用與伺服器應用的任何其他靜態 Web 資源一起發布到伺服器應用的 */bin/發佈/目標框架\/發佈/wwwroot*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="64d32-121">Hosted &ndash; The client Blazor WebAssembly app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder of the server app, along with any other static web assets of the server app.</span></span> <span data-ttu-id="64d32-122">將*發佈*資料夾的內容部署到主機。</span><span class="sxs-lookup"><span data-stu-id="64d32-122">Deploy the contents of the *publish* folder to the host.</span></span>
* Blazor<span data-ttu-id="64d32-123">伺服器&ndash;應用發佈到 */bin/發佈/[目標框架]/發佈*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="64d32-123"> Server &ndash; The app is published into the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="64d32-124">將*發佈*資料夾的內容部署到主機。</span><span class="sxs-lookup"><span data-stu-id="64d32-124">Deploy the contents of the *publish* folder to the host.</span></span>

<span data-ttu-id="64d32-125">該資料夾中的資產均會部署至 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="64d32-125">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="64d32-126">部署可能是手動或自動的程序，視使用中的開發工具而定。</span><span class="sxs-lookup"><span data-stu-id="64d32-126">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="64d32-127">應用程式基底路徑</span><span class="sxs-lookup"><span data-stu-id="64d32-127">App base path</span></span>

<span data-ttu-id="64d32-128">*應用基本路徑*是應用的根 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="64d32-128">The *app base path* is the app's root URL path.</span></span> <span data-ttu-id="64d32-129">請考慮以下ASP.NET核心應用和Blazor子應用:</span><span class="sxs-lookup"><span data-stu-id="64d32-129">Consider the following ASP.NET Core app and Blazor sub-app:</span></span>

* <span data-ttu-id="64d32-130">ASP.NET核心應用程式命名為`MyApp`:</span><span class="sxs-lookup"><span data-stu-id="64d32-130">The ASP.NET Core app is named `MyApp`:</span></span>
  * <span data-ttu-id="64d32-131">該應用程式實際駐留在*d:/MyApp*。</span><span class="sxs-lookup"><span data-stu-id="64d32-131">The app physically resides at *d:/MyApp*.</span></span>
  * <span data-ttu-id="64d32-132">要求在`https://www.contoso.com/{MYAPP RESOURCE}`接收 。</span><span class="sxs-lookup"><span data-stu-id="64d32-132">Requests are received at `https://www.contoso.com/{MYAPP RESOURCE}`.</span></span>
* <span data-ttu-id="64d32-133">名為Blazor的`CoolApp`應用程式 是`MyApp`: 的 子應用:</span><span class="sxs-lookup"><span data-stu-id="64d32-133">A Blazor app named `CoolApp` is a sub-app of `MyApp`:</span></span>
  * <span data-ttu-id="64d32-134">子應用物理駐留在*d:/MyApp/CoolApp*。</span><span class="sxs-lookup"><span data-stu-id="64d32-134">The sub-app physically resides at *d:/MyApp/CoolApp*.</span></span>
  * <span data-ttu-id="64d32-135">要求在`https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`接收 。</span><span class="sxs-lookup"><span data-stu-id="64d32-135">Requests are received at `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span></span>

<span data-ttu-id="64d32-136">如果不為`CoolApp`指定的其他配置,此方案中的子應用不知道它駐留在伺服器上的位置。</span><span class="sxs-lookup"><span data-stu-id="64d32-136">Without specifying additional configuration for `CoolApp`, the sub-app in this scenario has no knowledge of where it resides on the server.</span></span> <span data-ttu-id="64d32-137">例如,如果應用程式位於相對 URL`/CoolApp/`路徑中,則無法為其資源構造正確的相對 URL。</span><span class="sxs-lookup"><span data-stu-id="64d32-137">For example, the app can't construct correct relative URLs to its resources without knowing that it resides at the relative URL path `/CoolApp/`.</span></span>

<span data-ttu-id="64d32-138">Blazor要為 應用程式的基本路徑提供`https://www.contoso.com/CoolApp/`設定`<base>`, 標`href`記的屬性 設定為*Pages/_Host.cshtml*檔案(Blazor伺服器)或*wwwroot/index.html*Blazor檔案(WebAssembly) 的相對根路徑:</span><span class="sxs-lookup"><span data-stu-id="64d32-138">To provide configuration for the Blazor app's base path of `https://www.contoso.com/CoolApp/`, the `<base>` tag's `href` attribute is set to the relative root path in the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly):</span></span>

```html
<base href="/CoolApp/">
```

Blazor<span data-ttu-id="64d32-139">伺服器套用在應用程式的要求導<xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*>`Startup.Configure`管中呼叫 :</span><span class="sxs-lookup"><span data-stu-id="64d32-139"> Server apps additionally set the server-side base path by calling <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> in the app's request pipeline of `Startup.Configure`:</span></span>

```csharp
app.UsePathBase("/CoolApp");
```

<span data-ttu-id="64d32-140">通過提供相對 URL 路徑,不在根目錄中的元件可以構造相對於應用根路徑的 URL。</span><span class="sxs-lookup"><span data-stu-id="64d32-140">By providing the relative URL path, a component that isn't in the root directory can construct URLs relative to the app's root path.</span></span> <span data-ttu-id="64d32-141">目錄結構不同級別的元件可以在整個應用的位置生成指向其他資源的連結。</span><span class="sxs-lookup"><span data-stu-id="64d32-141">Components at different levels of the directory structure can build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="64d32-142">應用基本路徑還用於攔截連結`href`目標位於應用基本路徑URI空間內的選定超連結。</span><span class="sxs-lookup"><span data-stu-id="64d32-142">The app base path is also used to intercept selected hyperlinks where the `href` target of the link is within the app base path URI space.</span></span> <span data-ttu-id="64d32-143">路由器Blazor處理內部導航。</span><span class="sxs-lookup"><span data-stu-id="64d32-143">The Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="64d32-144">在許多託管方案中,應用的相對 URL 路徑是應用的根目錄。</span><span class="sxs-lookup"><span data-stu-id="64d32-144">In many hosting scenarios, the relative URL path to the app is the root of the app.</span></span> <span data-ttu-id="64d32-145">在這些情況下,應用的相對 URL 基本路徑是前進斜杠`<base href="/" />`(), 這是Blazor應用的默認配置。</span><span class="sxs-lookup"><span data-stu-id="64d32-145">In these cases, the app's relative URL base path is a forward slash (`<base href="/" />`), which is the default configuration for a Blazor app.</span></span> <span data-ttu-id="64d32-146">在其他託管方案中(如 GitHub 主頁和 IIS 子應用)中,應用基本路徑必須設置為應用的伺服器相對 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="64d32-146">In other hosting scenarios, such as GitHub Pages and IIS sub-apps, the app base path must be set to the server's relative URL path of the app.</span></span>

<span data-ttu-id="64d32-147">要設置應用的基本路徑,請更新*Pages/_Host.cshtml*檔(Blazor伺服器) 或*wwwroot/index.html*檔(WebAssembly)Blazor`<base>``<head>`的標記元素中的標記。</span><span class="sxs-lookup"><span data-stu-id="64d32-147">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly).</span></span> <span data-ttu-id="64d32-148">將`href`屬性值設置`/{RELATIVE URL PATH}/`為 (需要尾隨斜杠),其中`{RELATIVE URL PATH}`應用的完整相對 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="64d32-148">Set the `href` attribute value to `/{RELATIVE URL PATH}/` (the trailing slash is required), where `{RELATIVE URL PATH}` is the app's full relative URL path.</span></span>

<span data-ttu-id="64d32-149">對於具有Blazor非根相對 URL 路徑的 WebAssembly`<base href="/CoolApp/">`應用(例如 ),該應用*在本地運行時*找不到其資源。</span><span class="sxs-lookup"><span data-stu-id="64d32-149">For an Blazor WebAssembly app with a non-root relative URL path (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="64d32-150">若要在本機開發和測試期間解決這個問題，您可以提供「基底路徑」\*\* 引數，讓它在執行時符合 `<base>` 標籤的 `href` 值。</span><span class="sxs-lookup"><span data-stu-id="64d32-150">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span> <span data-ttu-id="64d32-151">不要包括尾隨斜杠。</span><span class="sxs-lookup"><span data-stu-id="64d32-151">Don't include a trailing slash.</span></span> <span data-ttu-id="64d32-152">要在本地端執行應用程式時傳遞路徑基參數`dotnet run`,`--pathbase`請使用以下選項從應用程式的目錄中執行命令:</span><span class="sxs-lookup"><span data-stu-id="64d32-152">To pass the path base argument when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

<span data-ttu-id="64d32-153">對於相對BlazorURL`/CoolApp/`路徑的 WebAssembly 應用`<base href="/CoolApp/">`, 指令是:</span><span class="sxs-lookup"><span data-stu-id="64d32-153">For a Blazor WebAssembly app with a relative URL path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```dotnetcli
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="64d32-154">WebAssemblyBlazor應用在`http://localhost:port/CoolApp`本地回應。</span><span class="sxs-lookup"><span data-stu-id="64d32-154">The Blazor WebAssembly app responds locally at `http://localhost:port/CoolApp`.</span></span>

## <a name="deployment"></a><span data-ttu-id="64d32-155">部署</span><span class="sxs-lookup"><span data-stu-id="64d32-155">Deployment</span></span>

<span data-ttu-id="64d32-156">如需部署指導方針，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="64d32-156">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
