---
title: "使用角度的專案範本"
author: SteveSandersonMS
description: "了解如何開始使用 ASP.NET Core 單一頁面應用程式 (SPA) 專案範本 Angular 和有角度的方向 CLI。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/angular
ms.openlocfilehash: 07cfd20809acb67bdae6561b6ccd6edf1e70a3fe
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="use-the-angular-project-template"></a><span data-ttu-id="b8cad-103">使用角度的專案範本</span><span class="sxs-lookup"><span data-stu-id="b8cad-103">Use the Angular project template</span></span>

> [!NOTE]
> <span data-ttu-id="b8cad-104">這份文件不需角度的專案範本包含 ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="b8cad-104">This documentation isn't about the Angular project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="b8cad-105">所以，您可以手動更新較新的角度範本的相關。</span><span class="sxs-lookup"><span data-stu-id="b8cad-105">It's about the newer Angular template to which you can update manually.</span></span> <span data-ttu-id="b8cad-106">預設範本包含 ASP.NET Core 2.1。</span><span class="sxs-lookup"><span data-stu-id="b8cad-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

<span data-ttu-id="b8cad-107">已更新的角度專案範本提供方便的起點 ASP.NET Core 應用程式使用 Angular 和有角度的方向 CLI 來實作豐富的用戶端使用者介面 (UI)。</span><span class="sxs-lookup"><span data-stu-id="b8cad-107">The updated Angular project template provides a convenient starting point for ASP.NET Core apps using Angular and the Angular CLI to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="b8cad-108">範本就相當於建立 ASP.NET Core 專案作為 API 後端和有角度的方向 CLI 專案做為使用者介面。</span><span class="sxs-lookup"><span data-stu-id="b8cad-108">The template is equivalent to creating an ASP.NET Core project to act as an API backend and an Angular CLI project to act as a UI.</span></span> <span data-ttu-id="b8cad-109">範本提供了裝載單一應用程式專案中的這兩種專案類型。</span><span class="sxs-lookup"><span data-stu-id="b8cad-109">The template offers the convenience of hosting both project types in a single app project.</span></span> <span data-ttu-id="b8cad-110">因此，應用程式專案可以建立並發佈成單一單位。</span><span class="sxs-lookup"><span data-stu-id="b8cad-110">Consequently, the app project can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="b8cad-111">建立新的應用程式</span><span class="sxs-lookup"><span data-stu-id="b8cad-111">Create a new app</span></span>

<span data-ttu-id="b8cad-112">如果使用 ASP.NET Core 2.0，請確定您已經[安裝更新的角度專案範本](xref:spa/index#installation)。</span><span class="sxs-lookup"><span data-stu-id="b8cad-112">If using ASP.NET Core 2.0, ensure you've [installed the updated Angular project template](xref:spa/index#installation).</span></span> <span data-ttu-id="b8cad-113">如果您有 ASP.NET Core 2.1，沒有需要安裝它。</span><span class="sxs-lookup"><span data-stu-id="b8cad-113">If you have ASP.NET Core 2.1, there's no need to install it.</span></span>

<span data-ttu-id="b8cad-114">建立新的專案，從命令提示字元使用命令`dotnet new angular`空的目錄中。</span><span class="sxs-lookup"><span data-stu-id="b8cad-114">Create a new project from a command prompt using the command `dotnet new angular` in an empty directory.</span></span> <span data-ttu-id="b8cad-115">例如，下列命令會建立應用程式在*my-新的應用程式*目錄並切換到該目錄：</span><span class="sxs-lookup"><span data-stu-id="b8cad-115">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new angular -o my-new-app
cd my-new-app
```

<span data-ttu-id="b8cad-116">執行應用程式，從 Visual Studio 或.NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="b8cad-116">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b8cad-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8cad-117">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b8cad-118">開啟產生*.csproj*檔案，並從該處，像平常一樣執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b8cad-118">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="b8cad-119">在建置程序還原第一次執行，可能需要幾分鐘的 npm 相依性。</span><span class="sxs-lookup"><span data-stu-id="b8cad-119">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="b8cad-120">後續建置會更快。</span><span class="sxs-lookup"><span data-stu-id="b8cad-120">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b8cad-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b8cad-121">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b8cad-122">請確定您有環境變數呼叫`ASPNETCORE_Environment`值是`Development`。</span><span class="sxs-lookup"><span data-stu-id="b8cad-122">Ensure you have an environment variable called `ASPNETCORE_Environment` with a value of `Development`.</span></span> <span data-ttu-id="b8cad-123">在 Windows 上 （在非 PowerShell 提示） 中，執行`SET ASPNETCORE_Environment=Development`。</span><span class="sxs-lookup"><span data-stu-id="b8cad-123">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="b8cad-124">在 Linux 或 macOS 上，執行`export ASPNETCORE_Environment=Development`。</span><span class="sxs-lookup"><span data-stu-id="b8cad-124">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="b8cad-125">執行[dotnet 組建](/dotnet/core/tools/dotnet-build)以確認應用程式建置正確。</span><span class="sxs-lookup"><span data-stu-id="b8cad-125">Run [dotnet build](/dotnet/core/tools/dotnet-build) to verify the app builds correctly.</span></span> <span data-ttu-id="b8cad-126">在第一個執行中，在建置程序還原 npm 相依性，這可能要花費幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="b8cad-126">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="b8cad-127">後續建置會更快。</span><span class="sxs-lookup"><span data-stu-id="b8cad-127">Subsequent builds are much faster.</span></span>

<span data-ttu-id="b8cad-128">執行[dotnet 執行](/dotnet/core/tools/dotnet-run)啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="b8cad-128">Run [dotnet run](/dotnet/core/tools/dotnet-run) to start the app.</span></span> <span data-ttu-id="b8cad-129">會記錄類似下列訊息：</span><span class="sxs-lookup"><span data-stu-id="b8cad-129">A message similar to the following is logged:</span></span>

```console
Now listening on: http://localhost:<port>
```

<span data-ttu-id="b8cad-130">瀏覽至此 url 在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="b8cad-130">Navigate to this URL in a browser.</span></span>

<span data-ttu-id="b8cad-131">應用程式啟動的角度 CLI 伺服器在背景中執行個體設定。</span><span class="sxs-lookup"><span data-stu-id="b8cad-131">The app starts up an instance of the Angular CLI server in the background.</span></span> <span data-ttu-id="b8cad-132">會記錄類似下列的訊息： *NG 即時程式開發伺服器正在接聽 localhost:&lt;otherport&gt;，開啟瀏覽器上 http://localhost:&lt;otherport&gt; /*.</span><span class="sxs-lookup"><span data-stu-id="b8cad-132">A message similar to the following is logged: *NG Live Development Server is listening on localhost:&lt;otherport&gt;, open your browser on http://localhost:&lt;otherport&gt;/*.</span></span> <span data-ttu-id="b8cad-133">忽略此訊息&mdash;它有**不**結合的 ASP.NET Core 和有角度的方向 CLI 應用程式的 URL。</span><span class="sxs-lookup"><span data-stu-id="b8cad-133">Ignore this message&mdash;it's **not** the URL for the combined ASP.NET Core and Angular CLI app.</span></span>

---

<span data-ttu-id="b8cad-134">專案範本建立 ASP.NET Core 應用程式與角度的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b8cad-134">The project template creates an ASP.NET Core app and an Angular app.</span></span> <span data-ttu-id="b8cad-135">ASP.NET Core 應用程式被要用於資料存取、 授權和其他伺服器端的問題。</span><span class="sxs-lookup"><span data-stu-id="b8cad-135">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="b8cad-136">角度的應用程式，並位於*ClientApp*子目錄，要用於所有 UI 疑慮。</span><span class="sxs-lookup"><span data-stu-id="b8cad-136">The Angular app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="b8cad-137">新增頁面、 影像、 樣式、 模組、 等等。</span><span class="sxs-lookup"><span data-stu-id="b8cad-137">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="b8cad-138">*ClientApp*目錄包含一般的角度 CLI 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b8cad-138">The *ClientApp* directory contains a standard Angular CLI app.</span></span> <span data-ttu-id="b8cad-139">請參閱正式[角度的文件](https://github.com/angular/angular-cli/wiki)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="b8cad-139">See the official [Angular documentation](https://github.com/angular/angular-cli/wiki) for more information.</span></span>

<span data-ttu-id="b8cad-140">兩者的些微不同角度 CLI 本身所建立的一個角度此範本所建立的應用程式之間 (透過`ng new`); 不過，應用程式的功能，維持不變。</span><span class="sxs-lookup"><span data-stu-id="b8cad-140">There are slight differences between the Angular app created by this template and the one created by Angular CLI itself (via `ng new`); however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="b8cad-141">範本所建立的應用程式包含[Bootstrap](https://getbootstrap.com/)為基礎的配置和基本的路由範例。</span><span class="sxs-lookup"><span data-stu-id="b8cad-141">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="run-ng-commands"></a><span data-ttu-id="b8cad-142">執行 ng 命令</span><span class="sxs-lookup"><span data-stu-id="b8cad-142">Run ng commands</span></span>

<span data-ttu-id="b8cad-143">在命令提示字元切換至*ClientApp*子目錄：</span><span class="sxs-lookup"><span data-stu-id="b8cad-143">In a command prompt, switch to the *ClientApp* subdirectory:</span></span>

```console
cd ClientApp
```

<span data-ttu-id="b8cad-144">如果您有`ng`全域安裝的工具，您可以執行任何其命令。</span><span class="sxs-lookup"><span data-stu-id="b8cad-144">If you have the `ng` tool installed globally, you can run any of its commands.</span></span> <span data-ttu-id="b8cad-145">例如，您可以執行`ng lint`， `ng test`，或任何其他[角度 CLI 命令](https://github.com/angular/angular-cli/wiki#additional-commands)。</span><span class="sxs-lookup"><span data-stu-id="b8cad-145">For example, you can run `ng lint`, `ng test`, or any of the other [Angular CLI commands](https://github.com/angular/angular-cli/wiki#additional-commands).</span></span> <span data-ttu-id="b8cad-146">若要執行不需要`ng serve`，因為您的 ASP.NET Core 應用程式會處理提供伺服器端和用戶端應用程式部分。</span><span class="sxs-lookup"><span data-stu-id="b8cad-146">There's no need to run `ng serve` though, because your ASP.NET Core app deals with serving both server-side and client-side parts of your app.</span></span> <span data-ttu-id="b8cad-147">就內部而言，它會使用`ng serve`開發工作。</span><span class="sxs-lookup"><span data-stu-id="b8cad-147">Internally, it uses `ng serve` in development.</span></span>

<span data-ttu-id="b8cad-148">如果您沒有`ng`工具安裝，執行`npm run ng`改為。</span><span class="sxs-lookup"><span data-stu-id="b8cad-148">If you don't have the `ng` tool installed, run `npm run ng` instead.</span></span> <span data-ttu-id="b8cad-149">例如，您可以執行`npm run ng lint`或`npm run ng test`。</span><span class="sxs-lookup"><span data-stu-id="b8cad-149">For example, you can run `npm run ng lint` or `npm run ng test`.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="b8cad-150">安裝 npm 封裝</span><span class="sxs-lookup"><span data-stu-id="b8cad-150">Install npm packages</span></span>

<span data-ttu-id="b8cad-151">若要安裝協力廠商 npm 封裝，請使用 命令提示字元中*ClientApp*子目錄。</span><span class="sxs-lookup"><span data-stu-id="b8cad-151">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="b8cad-152">例如: </span><span class="sxs-lookup"><span data-stu-id="b8cad-152">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="b8cad-153">發行與部署</span><span class="sxs-lookup"><span data-stu-id="b8cad-153">Publish and deploy</span></span>

<span data-ttu-id="b8cad-154">在開發中，應用程式以方便開發人員最佳化模式執行。</span><span class="sxs-lookup"><span data-stu-id="b8cad-154">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="b8cad-155">例如，JavaScript 組合會包含來源對應 （以便偵錯時，您可以看到原始的 TypeScript 程式碼）。</span><span class="sxs-lookup"><span data-stu-id="b8cad-155">For example, JavaScript bundles include source maps (so that when debugging, you can see your original TypeScript code).</span></span> <span data-ttu-id="b8cad-156">應用程式監控的 TypeScript、 HTML 和 CSS 檔案變更，在磁碟上自動重新編譯並重新載入時看到這些變更的檔案。</span><span class="sxs-lookup"><span data-stu-id="b8cad-156">The app watches for TypeScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="b8cad-157">在生產環境中，提供您已針對效能最佳化的應用程式的版本的服務。</span><span class="sxs-lookup"><span data-stu-id="b8cad-157">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="b8cad-158">這會設定為自動進行。</span><span class="sxs-lookup"><span data-stu-id="b8cad-158">This is configured to happen automatically.</span></span> <span data-ttu-id="b8cad-159">當您發行時，組建組態發出縮短前, 時間 (AoT) 編譯您的用戶端程式碼的組建。</span><span class="sxs-lookup"><span data-stu-id="b8cad-159">When you publish, the build configuration emits a minified, ahead-of-time (AoT) compiled build of your client-side code.</span></span> <span data-ttu-id="b8cad-160">不同於開發組建中，在正式組建不需要在伺服器上安裝 Node.js (除非您已啟用[伺服器端尚未](#server-side-rendering))。</span><span class="sxs-lookup"><span data-stu-id="b8cad-160">Unlike the development build, the production build doesn't require Node.js to be installed on the server (unless you have enabled [server-side prerendering](#server-side-rendering)).</span></span>

<span data-ttu-id="b8cad-161">您可以使用標準[ASP.NET Core 裝載和部署方法](xref:host-and-deploy/index)。</span><span class="sxs-lookup"><span data-stu-id="b8cad-161">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-ng-serve-independently"></a><span data-ttu-id="b8cad-162">獨立執行 「 ng serve"</span><span class="sxs-lookup"><span data-stu-id="b8cad-162">Run "ng serve" independently</span></span>

<span data-ttu-id="b8cad-163">專案已設定為在背景中啟動自己的角度 CLI 伺服器執行個體，ASP.NET Core 應用程式開發模式中啟動。</span><span class="sxs-lookup"><span data-stu-id="b8cad-163">The project is configured to start its own instance of the Angular CLI server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="b8cad-164">這是很方便，因為您不需要手動執行不同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="b8cad-164">This is convenient because you don't have to run a separate server manually.</span></span>

<span data-ttu-id="b8cad-165">沒有這個預設安裝的缺點。</span><span class="sxs-lookup"><span data-stu-id="b8cad-165">There's a drawback to this default setup.</span></span> <span data-ttu-id="b8cad-166">每當您修改 C# 程式碼和您的 ASP.NET 核心應用程式需要重新啟動，角度 CLI 伺服器重新啟動。</span><span class="sxs-lookup"><span data-stu-id="b8cad-166">Each time you modify your C# code and your ASP.NET Core app needs to restart, the Angular CLI server restarts.</span></span> <span data-ttu-id="b8cad-167">大約 10 秒鐘，才能重新啟動。</span><span class="sxs-lookup"><span data-stu-id="b8cad-167">Around 10 seconds is required to start back up.</span></span> <span data-ttu-id="b8cad-168">如果您正在經常 C# 程式碼編輯，而且不想要等到重新啟動的角度 CLI，角度 CLI 伺服器外部外獨立執行的 ASP.NET 核心程序。</span><span class="sxs-lookup"><span data-stu-id="b8cad-168">If you're making frequent C# code edits and don't want to wait for Angular CLI to restart, run the Angular CLI server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="b8cad-169">若要這樣做：</span><span class="sxs-lookup"><span data-stu-id="b8cad-169">To do so:</span></span>

1. <span data-ttu-id="b8cad-170">在命令提示字元切換至*ClientApp*子目錄，並啟動角度 CLI 程式開發伺服器：</span><span class="sxs-lookup"><span data-stu-id="b8cad-170">In a command prompt, switch to the *ClientApp* subdirectory, and launch the Angular CLI development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="b8cad-171">使用`npm start`啟動角度 CLI 程式開發伺服器，不`ng serve`，以便在設定*package.json*遵守。</span><span class="sxs-lookup"><span data-stu-id="b8cad-171">Use `npm start` to launch the Angular CLI development server, not `ng serve`, so that the configuration in *package.json* is respected.</span></span> <span data-ttu-id="b8cad-172">要將其他參數傳遞給角度 CLI 伺服器，將它們加入相關`scripts`中一行您*package.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="b8cad-172">To pass additional parameters to the Angular CLI server, add them to the relevant `scripts` line in your *package.json* file.</span></span>

2. <span data-ttu-id="b8cad-173">修改您的 ASP.NET Core 應用程式，而不是啟動其自己的其中一個使用外部的角度 CLI 執行個體。</span><span class="sxs-lookup"><span data-stu-id="b8cad-173">Modify your ASP.NET Core app to use the external Angular CLI instance instead of launching one of its own.</span></span> <span data-ttu-id="b8cad-174">在您*啟動*類別中，取代`spa.UseAngularCliServer`引動過程為下列：</span><span class="sxs-lookup"><span data-stu-id="b8cad-174">In your *Startup* class, replace the `spa.UseAngularCliServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

<span data-ttu-id="b8cad-175">當您啟動 ASP.NET Core 應用程式，它將不會啟動角度 CLI 伺服器。</span><span class="sxs-lookup"><span data-stu-id="b8cad-175">When you start your ASP.NET Core app, it won't launch an Angular CLI server.</span></span> <span data-ttu-id="b8cad-176">改為使用您以手動方式啟動的執行個體。</span><span class="sxs-lookup"><span data-stu-id="b8cad-176">The instance you started manually is used instead.</span></span> <span data-ttu-id="b8cad-177">這可讓它啟動並重新啟動速度。</span><span class="sxs-lookup"><span data-stu-id="b8cad-177">This enables it to start and restart faster.</span></span> <span data-ttu-id="b8cad-178">不會再等候每次重建您的用戶端應用程式的角度 CLI。</span><span class="sxs-lookup"><span data-stu-id="b8cad-178">It's no longer waiting for Angular CLI to rebuild your client app each time.</span></span>

## <a name="server-side-rendering"></a><span data-ttu-id="b8cad-179">伺服器端轉譯</span><span class="sxs-lookup"><span data-stu-id="b8cad-179">Server-side rendering</span></span>

<span data-ttu-id="b8cad-180">效能的功能，您可以選擇預先呈現的伺服器，以及執行在用戶端上的應用程式有角度的方向。</span><span class="sxs-lookup"><span data-stu-id="b8cad-180">As a performance feature, you can choose to pre-render your Angular app on the server as well as running it on the client.</span></span> <span data-ttu-id="b8cad-181">這表示瀏覽器中接收 HTML 標記，表示您的應用程式起始 UI，讓它們顯示甚至必須下載及執行 JavaScript 組合。</span><span class="sxs-lookup"><span data-stu-id="b8cad-181">This means that browsers receive HTML markup representing your app's initial UI, so they display it even before downloading and executing your JavaScript bundles.</span></span> <span data-ttu-id="b8cad-182">這個實作的大部分來自稱為角度功能[角度通用](https://universal.angular.io/)。</span><span class="sxs-lookup"><span data-stu-id="b8cad-182">Most of the implementation of this comes from an Angular feature called [Angular Universal](https://universal.angular.io/).</span></span>

> [!TIP]
> <span data-ttu-id="b8cad-183">啟用伺服器端轉譯 (SSR) 導入額外複雜性，同時在開發和部署期間的數的字。</span><span class="sxs-lookup"><span data-stu-id="b8cad-183">Enabling server-side rendering (SSR) introduces a number of extra complications both during development and deployment.</span></span> <span data-ttu-id="b8cad-184">讀取[缺點 SSR](#drawbacks-of-ssr)來判斷 SSR 是否適合您的需求。</span><span class="sxs-lookup"><span data-stu-id="b8cad-184">Read [drawbacks of SSR](#drawbacks-of-ssr) to determine if SSR is a good fit for your requirements.</span></span>

<span data-ttu-id="b8cad-185">若要啟用 SSR，您必須先新增您的專案項目數目。</span><span class="sxs-lookup"><span data-stu-id="b8cad-185">To enable SSR, you need to make a number of additions to your project.</span></span>

<span data-ttu-id="b8cad-186">在*啟動*類別*之後*設定列`spa.Options.SourcePath`，和*之前*呼叫`UseAngularCliServer`或`UseProxyToSpaDevelopmentServer`，加入下列：</span><span class="sxs-lookup"><span data-stu-id="b8cad-186">In the *Startup* class, *after* the line that configures `spa.Options.SourcePath`, and *before* the call to `UseAngularCliServer` or `UseProxyToSpaDevelopmentServer`, add the following:</span></span>

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

<span data-ttu-id="b8cad-187">在開發模式中，這段程式碼會嘗試執行指令碼建立 SSR 配套`build:ssr`，定義在*ClientApp\package.json*。</span><span class="sxs-lookup"><span data-stu-id="b8cad-187">In development mode, this code attempts to build the SSR bundle by running the script `build:ssr`, which is defined in *ClientApp\package.json*.</span></span> <span data-ttu-id="b8cad-188">這會建置角度的應用程式，名稱為`ssr`，但未定義。</span><span class="sxs-lookup"><span data-stu-id="b8cad-188">This builds an Angular app named `ssr`, which isn't yet defined.</span></span> 

<span data-ttu-id="b8cad-189">在結尾`apps`陣列中*ClientApp/.angular-cli.json*，定義額外的應用程式，但名稱`ssr`。</span><span class="sxs-lookup"><span data-stu-id="b8cad-189">At the end of the `apps` array in *ClientApp/.angular-cli.json*, define an extra app with name `ssr`.</span></span> <span data-ttu-id="b8cad-190">使用下列選項：</span><span class="sxs-lookup"><span data-stu-id="b8cad-190">Use the following options:</span></span>

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

<span data-ttu-id="b8cad-191">這個新的 SSR 啟用應用程式設定需要進一步的兩個檔案： *tsconfig.server.json*和*main.server.ts*。</span><span class="sxs-lookup"><span data-stu-id="b8cad-191">This new SSR-enabled app configuration requires two further files: *tsconfig.server.json* and *main.server.ts*.</span></span> <span data-ttu-id="b8cad-192">*Tsconfig.server.json*檔案會指定 TypeScript 編譯選項。</span><span class="sxs-lookup"><span data-stu-id="b8cad-192">The *tsconfig.server.json* file specifies TypeScript compilation options.</span></span> <span data-ttu-id="b8cad-193">*Main.server.ts*期間 SSR 檔將做為程式碼進入點。</span><span class="sxs-lookup"><span data-stu-id="b8cad-193">The *main.server.ts* file serves as the code entry point during SSR.</span></span>

<span data-ttu-id="b8cad-194">加入新的檔案稱為*tsconfig.server.json*內*ClientApp/src* (與現有*tsconfig.app.json*)，其中包含下列：</span><span class="sxs-lookup"><span data-stu-id="b8cad-194">Add a new file called *tsconfig.server.json* inside *ClientApp/src* (alongside the existing *tsconfig.app.json*), containing the following:</span></span>

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

<span data-ttu-id="b8cad-195">這個檔案會設定要尋找模組呼叫 Angular 的 AoT 編譯器`app.server.module`。</span><span class="sxs-lookup"><span data-stu-id="b8cad-195">This file configures Angular's AoT compiler to look for a module called `app.server.module`.</span></span> <span data-ttu-id="b8cad-196">藉由建立新的檔案，在加入此*ClientApp/src/app/app.server.module.ts* (連同現有*app.module.ts*) 包含下列：</span><span class="sxs-lookup"><span data-stu-id="b8cad-196">Add this by creating a new file at *ClientApp/src/app/app.server.module.ts* (alongside the existing *app.module.ts*) containing the following:</span></span> 

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

<span data-ttu-id="b8cad-197">此模組會繼承您的用戶端`app.module`和定義的額外 Angular 模組可以使用 SSR 期間。</span><span class="sxs-lookup"><span data-stu-id="b8cad-197">This module inherits from your client-side `app.module` and defines which extra Angular modules are available during SSR.</span></span>

<span data-ttu-id="b8cad-198">請記得，新`ssr`中的項目*.angular cli.json*參考呼叫的進入點檔案*main.server.ts*。</span><span class="sxs-lookup"><span data-stu-id="b8cad-198">Recall that the new `ssr` entry in *.angular-cli.json* referenced an entry point file called *main.server.ts*.</span></span> <span data-ttu-id="b8cad-199">您尚未新增該檔案中，，和現在是時候這樣做。</span><span class="sxs-lookup"><span data-stu-id="b8cad-199">You haven't yet added that file, and now is time to do so.</span></span> <span data-ttu-id="b8cad-200">建立新的檔案在*ClientApp/src/main.server.ts* (連同現有*main.ts*)，其中包含下列：</span><span class="sxs-lookup"><span data-stu-id="b8cad-200">Create a new file at *ClientApp/src/main.server.ts* (alongside the existing *main.ts*), containing the following:</span></span>

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

<span data-ttu-id="b8cad-201">此檔案的程式碼是 ASP.NET Core 時所執行的每個要求執行`UseSpaPrerendering`您加入的中介軟體*啟動*類別。</span><span class="sxs-lookup"><span data-stu-id="b8cad-201">This file's code is what ASP.NET Core executes for each request when it runs the `UseSpaPrerendering` middleware that you added to the *Startup* class.</span></span> <span data-ttu-id="b8cad-202">它會處理接收`params`從.NET 程式碼 （例如所要求的 URL)，以及取得產生的 HTML 角度 SSR Api 呼叫。</span><span class="sxs-lookup"><span data-stu-id="b8cad-202">It deals with receiving `params` from the .NET code (such as the URL being requested), and making calls to Angular SSR APIs to get the resulting HTML.</span></span> 

<span data-ttu-id="b8cad-203">嚴格地-說，就足夠了 SSR 啟用開發模式。</span><span class="sxs-lookup"><span data-stu-id="b8cad-203">Strictly-speaking, this is sufficient to enable SSR in development mode.</span></span> <span data-ttu-id="b8cad-204">請務必讓一項的最後一個變更，以便發行時，您的應用程式正常運作。</span><span class="sxs-lookup"><span data-stu-id="b8cad-204">It's essential to make one final change so that your app works correctly when published.</span></span> <span data-ttu-id="b8cad-205">在您的應用程式的 main *.csproj*檔案中，設定`BuildServerSideRenderer`屬性值設定為`true`:</span><span class="sxs-lookup"><span data-stu-id="b8cad-205">In your app's main *.csproj* file, set the `BuildServerSideRenderer` property value to `true`:</span></span>

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

<span data-ttu-id="b8cad-206">這會設定建置流程以執行`build:ssr`發行期間和 SSR 檔案部署至伺服器。</span><span class="sxs-lookup"><span data-stu-id="b8cad-206">This configures the build process to run `build:ssr` during publishing and deploy the SSR files to the server.</span></span> <span data-ttu-id="b8cad-207">如果您不要啟用此功能，SSR 會失敗，在生產環境中。</span><span class="sxs-lookup"><span data-stu-id="b8cad-207">If you don't enable this, SSR fails in production.</span></span>

<span data-ttu-id="b8cad-208">開發或實際執行模式中，執行您的應用程式，角度的程式碼前會呈現為 HTML 的伺服器上。</span><span class="sxs-lookup"><span data-stu-id="b8cad-208">When your app runs in either development or production mode, the Angular code pre-renders as HTML on the server.</span></span> <span data-ttu-id="b8cad-209">用戶端程式碼執行以正常的。</span><span class="sxs-lookup"><span data-stu-id="b8cad-209">The client-side code executes as normal.</span></span>

### <a name="pass-data-from-net-code-into-typescript-code"></a><span data-ttu-id="b8cad-210">.NET 程式碼中的資料，傳遞到 TypeScript 程式碼</span><span class="sxs-lookup"><span data-stu-id="b8cad-210">Pass data from .NET code into TypeScript code</span></span>

<span data-ttu-id="b8cad-211">期間 SSR，您可能想要從 ASP.NET Core 應用程式的每個要求資料傳入角度的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b8cad-211">During SSR, you might want to pass per-request data from your ASP.NET Core app into your Angular app.</span></span> <span data-ttu-id="b8cad-212">比方說，您無法將 cookie 資訊或傳遞的項目從資料庫讀取。</span><span class="sxs-lookup"><span data-stu-id="b8cad-212">For example, you could pass cookie information or something read from a database.</span></span> <span data-ttu-id="b8cad-213">若要這樣做，請編輯您*啟動*類別。</span><span class="sxs-lookup"><span data-stu-id="b8cad-213">To do this, edit your *Startup* class.</span></span> <span data-ttu-id="b8cad-214">中的回呼`UseSpaPrerendering`，設定的值`options.SupplyData`如下所示：</span><span class="sxs-lookup"><span data-stu-id="b8cad-214">In the callback for `UseSpaPrerendering`, set a value for `options.SupplyData` such as the following:</span></span>

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

<span data-ttu-id="b8cad-215">`SupplyData`回呼可讓您傳遞任意、 每個要求、 可序列化 JSON 資料 （例如字串、 布林值或數字）。</span><span class="sxs-lookup"><span data-stu-id="b8cad-215">The `SupplyData` callback lets you pass arbitrary, per-request, JSON-serializable data (for example, strings, booleans, or numbers).</span></span> <span data-ttu-id="b8cad-216">您*main.server.ts*程式碼會接收為`params.data`。</span><span class="sxs-lookup"><span data-stu-id="b8cad-216">Your *main.server.ts* code receives this as `params.data`.</span></span> <span data-ttu-id="b8cad-217">例如，上述程式碼範例會傳遞做為布林值`params.data.isHttpsRequest`到`createServerRenderer`回呼。</span><span class="sxs-lookup"><span data-stu-id="b8cad-217">For example, the preceding code sample passes a boolean value as `params.data.isHttpsRequest` into the `createServerRenderer` callback.</span></span> <span data-ttu-id="b8cad-218">您可以將它傳遞給您的應用程式，以支援 Angular 任何方式的其他組件。</span><span class="sxs-lookup"><span data-stu-id="b8cad-218">You can pass this to other parts of your app in any way supported by Angular.</span></span> <span data-ttu-id="b8cad-219">例如，請參閱如何*main.server.ts*傳遞`BASE_URL`接收該宣告的建構函式的任何元件的值。</span><span class="sxs-lookup"><span data-stu-id="b8cad-219">For example, see how *main.server.ts* passes the `BASE_URL` value to any component whose constructor is declared to receive it.</span></span>

### <a name="drawbacks-of-ssr"></a><span data-ttu-id="b8cad-220">SSR 的缺點</span><span class="sxs-lookup"><span data-stu-id="b8cad-220">Drawbacks of SSR</span></span>

<span data-ttu-id="b8cad-221">並非所有應用程式會從 SSR 獲益。</span><span class="sxs-lookup"><span data-stu-id="b8cad-221">Not all apps benefit from SSR.</span></span> <span data-ttu-id="b8cad-222">主要優點是看得見的效能。</span><span class="sxs-lookup"><span data-stu-id="b8cad-222">The primary benefit is perceived performance.</span></span> <span data-ttu-id="b8cad-223">訪客到達您的應用程式，透過低速網路連線或慢速的行動裝置上快速，看到初始 UI，即使花時間來擷取或剖析 JavaScript 組合。</span><span class="sxs-lookup"><span data-stu-id="b8cad-223">Visitors reaching your app over a slow network connection or on slow mobile devices see the initial UI quickly, even if it takes a while to fetch or parse the JavaScript bundles.</span></span> <span data-ttu-id="b8cad-224">不過，許多 SPAs 主要用在快速的電腦上的快速、 內部公司網路上的應用程式幾乎會立即出現的位置。</span><span class="sxs-lookup"><span data-stu-id="b8cad-224">However, many SPAs are mainly used over fast, internal company networks on fast computers where the app appears almost instantly.</span></span>

<span data-ttu-id="b8cad-225">在相同的時間，有很大的缺點，以啟用 SSR。</span><span class="sxs-lookup"><span data-stu-id="b8cad-225">At the same time, there are significant drawbacks to enabling SSR.</span></span> <span data-ttu-id="b8cad-226">將複雜性加入您的開發程序。</span><span class="sxs-lookup"><span data-stu-id="b8cad-226">It adds complexity to your development process.</span></span> <span data-ttu-id="b8cad-227">您的程式碼必須在兩個不同環境中執行： 用戶端和伺服器端 （在 Node.js 環境中從 ASP.NET Core 叫用）。</span><span class="sxs-lookup"><span data-stu-id="b8cad-227">Your code must run in two different environments: client-side and server-side (in a Node.js environment invoked from ASP.NET Core).</span></span> <span data-ttu-id="b8cad-228">以下是要記住的一些事項：</span><span class="sxs-lookup"><span data-stu-id="b8cad-228">Here are some things to bear in mind:</span></span>

* <span data-ttu-id="b8cad-229">SSR 需要安裝在實際執行伺服器上的 Node.js。</span><span class="sxs-lookup"><span data-stu-id="b8cad-229">SSR requires a Node.js installation on your production servers.</span></span> <span data-ttu-id="b8cad-230">這會自動針對某些部署案例，例如 Azure 應用程式服務，但不適用於其他如 Azure Service Fabric 的情況。</span><span class="sxs-lookup"><span data-stu-id="b8cad-230">This is automatically the case for some deployment scenarios, such as Azure App Services, but not for others, such as Azure Service Fabric.</span></span>
* <span data-ttu-id="b8cad-231">啟用`BuildServerSideRenderer`建置旗標原因您*node_modules*目錄發行。</span><span class="sxs-lookup"><span data-stu-id="b8cad-231">Enabling the `BuildServerSideRenderer` build flag causes your *node_modules* directory to publish.</span></span> <span data-ttu-id="b8cad-232">此資料夾包含 20,000 + 檔案，這會增加部署的時間。</span><span class="sxs-lookup"><span data-stu-id="b8cad-232">This folder contains 20,000+ files, which increases deployment time.</span></span>
* <span data-ttu-id="b8cad-233">若要在 Node.js 環境中執行您的程式碼，它不能依賴特定瀏覽器的 JavaScript Api 的存在這類`window`或`localStorage`。</span><span class="sxs-lookup"><span data-stu-id="b8cad-233">To run your code in a Node.js environment, it can't rely on the existence of browser-specific JavaScript APIs such as `window` or `localStorage`.</span></span> <span data-ttu-id="b8cad-234">如果您的程式碼 （或您參考某些協力廠商程式庫） 便會嘗試使用這些 Api，您會得到錯誤 SSR 期間。</span><span class="sxs-lookup"><span data-stu-id="b8cad-234">If your code (or some third-party library you reference) tries to use these APIs, you'll get an error during SSR.</span></span> <span data-ttu-id="b8cad-235">例如，請勿使用 jQuery 因為它參考了瀏覽器專用的 Api，在許多地方。</span><span class="sxs-lookup"><span data-stu-id="b8cad-235">For example, don't use jQuery because it references browser-specific APIs in many places.</span></span> <span data-ttu-id="b8cad-236">若要避免這個錯誤，您必須避免 SSR 或避免特定瀏覽器的應用程式開發介面或程式庫。</span><span class="sxs-lookup"><span data-stu-id="b8cad-236">To prevent errors, you must either avoid SSR or avoid browser-specific APIs or libraries.</span></span> <span data-ttu-id="b8cad-237">您可以將任何這類 Api 的呼叫包裝在檢查，以確保它們不 SSR 期間會叫用。</span><span class="sxs-lookup"><span data-stu-id="b8cad-237">You can wrap any calls to such APIs in checks to ensure they aren't invoked during SSR.</span></span> <span data-ttu-id="b8cad-238">比方說，在 JavaScript 或 TypeScript 程式碼中使用的檢查，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b8cad-238">For example, use a check such as the following in JavaScript or TypeScript code:</span></span>

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
