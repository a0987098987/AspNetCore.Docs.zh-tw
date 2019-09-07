---
title: 裝載及部署 ASP.NET Core Blazor
author: guardrex
description: 探索如何裝載和部署 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 5a56bbda5bb7727c7dbeaed7f2a91d0dcb6e7e71
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773592"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="97d79-103">裝載及部署 ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="97d79-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="97d79-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="97d79-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="97d79-105">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="97d79-105">Publish the app</span></span>

<span data-ttu-id="97d79-106">應用程式會在發行組態中發佈以供部署。</span><span class="sxs-lookup"><span data-stu-id="97d79-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="97d79-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="97d79-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="97d79-108">從巡覽列中選取 [建置] > [發佈 {應用程式}]。</span><span class="sxs-lookup"><span data-stu-id="97d79-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="97d79-109">選取「發佈目標」。</span><span class="sxs-lookup"><span data-stu-id="97d79-109">Select the *publish target*.</span></span> <span data-ttu-id="97d79-110">若要在本機發佈，請選取 [資料夾]。</span><span class="sxs-lookup"><span data-stu-id="97d79-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="97d79-111">接受 [選擇資料夾] 欄位中的預設位置，或指定不同的位置。</span><span class="sxs-lookup"><span data-stu-id="97d79-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="97d79-112">選取 [發行] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="97d79-112">Select the **Publish** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="97d79-113">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="97d79-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="97d79-114">使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，搭配發行組態來發佈應用程式：</span><span class="sxs-lookup"><span data-stu-id="97d79-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```console
dotnet publish -c Release
```

---

<span data-ttu-id="97d79-115">發佈應用程式會觸發專案相依性的[還原](/dotnet/core/tools/dotnet-restore)並[建置](/dotnet/core/tools/dotnet-build)專案，然後才會建立資產以供部署。</span><span class="sxs-lookup"><span data-stu-id="97d79-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="97d79-116">在建置程序中，會移除未使用的方法和組件，減少應用程式的下載大小和載入時間。</span><span class="sxs-lookup"><span data-stu-id="97d79-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="97d79-117">Blazor 用戶端應用程式會發佈至 /bin/Release/{目標 FRAMEWORK}/publish/{組件名稱}/dist 資料夾。</span><span class="sxs-lookup"><span data-stu-id="97d79-117">A Blazor client-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span> <span data-ttu-id="97d79-118">Blazor 伺服器端應用程式會發佈至 */bin/Release/{目標 FRAMEWORK}/publish* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="97d79-118">A Blazor server-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="97d79-119">該資料夾中的資產均會部署至 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="97d79-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="97d79-120">部署可能是手動或自動的程序，視使用中的開發工具而定。</span><span class="sxs-lookup"><span data-stu-id="97d79-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="97d79-121">應用程式基底路徑</span><span class="sxs-lookup"><span data-stu-id="97d79-121">App base path</span></span>

<span data-ttu-id="97d79-122">*應用程式基底路徑*是應用程式的根 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="97d79-122">The *app base path* is the app's root URL path.</span></span> <span data-ttu-id="97d79-123">請考慮下列主要應用程式和 Blazor 應用程式：</span><span class="sxs-lookup"><span data-stu-id="97d79-123">Consider the following main app and Blazor app:</span></span>

* <span data-ttu-id="97d79-124">主要的應用程式稱為`MyApp`：</span><span class="sxs-lookup"><span data-stu-id="97d79-124">The main app is called `MyApp`:</span></span>
  * <span data-ttu-id="97d79-125">應用程式實際位於 *\\d： MyApp*。</span><span class="sxs-lookup"><span data-stu-id="97d79-125">The app physically resides at *d:\\MyApp*.</span></span>
  * <span data-ttu-id="97d79-126">會在`https://www.contoso.com/{MYAPP RESOURCE}`收到要求。</span><span class="sxs-lookup"><span data-stu-id="97d79-126">Requests are received at `https://www.contoso.com/{MYAPP RESOURCE}`.</span></span>
* <span data-ttu-id="97d79-127">名`CoolApp`為的 Blazor 應用程式是的`MyApp`子應用程式：</span><span class="sxs-lookup"><span data-stu-id="97d79-127">A Blazor app called `CoolApp` is a sub-app of `MyApp`:</span></span>
  * <span data-ttu-id="97d79-128">子應用程式實際上位於*d：\\MyApp\\CoolApp*。</span><span class="sxs-lookup"><span data-stu-id="97d79-128">The sub-app physically resides at *d:\\MyApp\\CoolApp*.</span></span>
  * <span data-ttu-id="97d79-129">會在`https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`收到要求。</span><span class="sxs-lookup"><span data-stu-id="97d79-129">Requests are received at `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span></span>

<span data-ttu-id="97d79-130">若未指定的`CoolApp`其他設定，此案例中的子應用程式就不會知道其位於伺服器上的位置。</span><span class="sxs-lookup"><span data-stu-id="97d79-130">Without specifying additional configuration for `CoolApp`, the sub-app in this scenario has no knowledge of where it resides on the server.</span></span> <span data-ttu-id="97d79-131">例如，應用程式無法在不知道其位於相對 URL 路徑`/CoolApp/`的情況下，對其資源建立正確的相對 url。</span><span class="sxs-lookup"><span data-stu-id="97d79-131">For example, the app can't construct correct relative URLs to its resources without knowing that it resides at the relative URL path `/CoolApp/`.</span></span>

<span data-ttu-id="97d79-132">若要為 Blazor 應用程式的`https://www.contoso.com/CoolApp/`基底路徑提供設定`<base>` ，標記的`href`屬性會設為*wwwroot/index.html*檔案中的相對根路徑：</span><span class="sxs-lookup"><span data-stu-id="97d79-132">To provide configuration for the Blazor app's base path of `https://www.contoso.com/CoolApp/`, the `<base>` tag's `href` attribute is set to the relative root path in the *wwwroot/index.html* file:</span></span>

```html
<base href="/CoolApp/">
```

<span data-ttu-id="97d79-133">藉由提供相對的 URL 路徑，不在根目錄中的元件可以針對應用程式的根路徑來建立 Url。</span><span class="sxs-lookup"><span data-stu-id="97d79-133">By providing the relative URL path, a component that isn't in the root directory can construct URLs relative to the app's root path.</span></span> <span data-ttu-id="97d79-134">不同目錄結構層級的元件可以在整個應用程式的位置建立其他資源的連結。</span><span class="sxs-lookup"><span data-stu-id="97d79-134">Components at different levels of the directory structure can build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="97d79-135">應用程式基底路徑也會用來攔截超連結點擊，其中連結的 `href` 目標是在應用程式基底路徑 URI 空間內&mdash;Blazor 路由器會處理內部瀏覽。</span><span class="sxs-lookup"><span data-stu-id="97d79-135">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="97d79-136">在許多裝載案例中，應用程式的相對 URL 路徑是應用程式的根目錄。</span><span class="sxs-lookup"><span data-stu-id="97d79-136">In many hosting scenarios, the relative URL path to the app is the root of the app.</span></span> <span data-ttu-id="97d79-137">在這些情況下，應用程式的相對 URL 基底路徑是正斜線`<base href="/" />`（），這是 Blazor 應用程式的預設設定。</span><span class="sxs-lookup"><span data-stu-id="97d79-137">In these cases, the app's relative URL base path is a forward slash (`<base href="/" />`), which is the default configuration for a Blazor app.</span></span> <span data-ttu-id="97d79-138">在其他裝載案例（例如 GitHub 頁面和 IIS 子應用程式）中，應用程式基底路徑必須設定為應用程式的伺服器相對 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="97d79-138">In other hosting scenarios, such as GitHub Pages and IIS sub-apps, the app base path must be set to the server's relative URL path to the app.</span></span>

<span data-ttu-id="97d79-139">若要設定應用程式的基底路徑，請更新 *wwwroot/index.html* 檔案 `<head>` 標籤項目內的 `<base>` 標籤。</span><span class="sxs-lookup"><span data-stu-id="97d79-139">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *wwwroot/index.html* file.</span></span> <span data-ttu-id="97d79-140">將屬性值設定為`/{RELATIVE URL PATH}/` （需要尾端斜線），其中`{RELATIVE URL PATH}`是應用程式的完整相對 URL 路徑。 `href`</span><span class="sxs-lookup"><span data-stu-id="97d79-140">Set the `href` attribute value to `/{RELATIVE URL PATH}/` (the trailing slash is required), where `{RELATIVE URL PATH}` is the app's full relative URL path.</span></span>

<span data-ttu-id="97d79-141">針對具有非根相對 URL 路徑的應用程式（ `<base href="/CoolApp/">`例如），在*本機執行時*，應用程式無法找到其資源。</span><span class="sxs-lookup"><span data-stu-id="97d79-141">For an app with a non-root relative URL path (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="97d79-142">若要在本機開發和測試期間解決這個問題，您可以提供「基底路徑」引數，讓它在執行時符合 `<base>` 標籤的 `href` 值。</span><span class="sxs-lookup"><span data-stu-id="97d79-142">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span> <span data-ttu-id="97d79-143">若要在本機執行應用程式時傳遞 path base 引數， `dotnet run`請`--pathbase`使用下列選項從應用程式的目錄執行命令：</span><span class="sxs-lookup"><span data-stu-id="97d79-143">To pass the path base argument when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```console
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

<span data-ttu-id="97d79-144">針對具有（ `/CoolApp/` `<base href="/CoolApp/">`）之相對 URL 路徑的應用程式，命令為：</span><span class="sxs-lookup"><span data-stu-id="97d79-144">For an app with a relative URL path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="97d79-145">應用程式在 `http://localhost:port/CoolApp` 本機回應。</span><span class="sxs-lookup"><span data-stu-id="97d79-145">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

## <a name="deployment"></a><span data-ttu-id="97d79-146">部署</span><span class="sxs-lookup"><span data-stu-id="97d79-146">Deployment</span></span>

<span data-ttu-id="97d79-147">如需部署指導方針，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="97d79-147">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
