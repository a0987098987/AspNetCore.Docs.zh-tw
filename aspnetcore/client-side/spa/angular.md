---
title: 搭配 ASP.NET Core 使用 Angular 專案範本
author: SteveSandersonMS
description: 了解如何開始使用適用於 Angular 與 Angular CLI 的 ASP.NET Core 單頁應用程式 (SPA) 專案範本。
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/angular
ms.openlocfilehash: 763b4fff7c64432328af660c66e6ff3f8f697f71
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011350"
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a><span data-ttu-id="cc29a-103">搭配 ASP.NET Core 使用 Angular 專案範本</span><span class="sxs-lookup"><span data-stu-id="cc29a-103">Use the Angular project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="cc29a-104">本文件與包含於 ASP.NET Core 2.0 中的 Angular 專案範本無關。</span><span class="sxs-lookup"><span data-stu-id="cc29a-104">This documentation isn't about the Angular project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="cc29a-105">其內容是關於您可以手動更新的新版 Angular 範本。</span><span class="sxs-lookup"><span data-stu-id="cc29a-105">It's about the newer Angular template to which you can update manually.</span></span> <span data-ttu-id="cc29a-106">ASP.NET Core 2.1 預設會包含此範本。</span><span class="sxs-lookup"><span data-stu-id="cc29a-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="cc29a-107">更新的 Angular 專案範本很適合作為 ASP.NET Core 應用程式的建置起點，並利用 Angular 和 Angular CLI 來實作功能豐富的用戶端使用者介面 (UI)。</span><span class="sxs-lookup"><span data-stu-id="cc29a-107">The updated Angular project template provides a convenient starting point for ASP.NET Core apps using Angular and the Angular CLI to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="cc29a-108">此範本等同於建立 ASP.NET Core 專案作為 API 後端，並建立 Angular CLI 專案作為 UI。</span><span class="sxs-lookup"><span data-stu-id="cc29a-108">The template is equivalent to creating an ASP.NET Core project to act as an API backend and an Angular CLI project to act as a UI.</span></span> <span data-ttu-id="cc29a-109">此範本可讓使用者輕鬆地將這兩種專案類型裝載至單一應用程式專案中。</span><span class="sxs-lookup"><span data-stu-id="cc29a-109">The template offers the convenience of hosting both project types in a single app project.</span></span> <span data-ttu-id="cc29a-110">因此，應用程式專案便可以單一單位的方式進行建置與發佈。</span><span class="sxs-lookup"><span data-stu-id="cc29a-110">Consequently, the app project can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="cc29a-111">建立新的應用程式</span><span class="sxs-lookup"><span data-stu-id="cc29a-111">Create a new app</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="cc29a-112">如果使用 ASP.NET Core 2.0，請確定您已經[安裝更新的 React 專案範本](xref:spa/index#installation)。</span><span class="sxs-lookup"><span data-stu-id="cc29a-112">If using ASP.NET Core 2.0, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="cc29a-113">如已安裝 ASP.NET Core 2.1，就沒有必要安裝 Angular 專案範本。</span><span class="sxs-lookup"><span data-stu-id="cc29a-113">If you have ASP.NET Core 2.1 installed, there's no need to install the Angular project template.</span></span>

::: moniker-end

<span data-ttu-id="cc29a-114">進入命令提示字元，然後在空目錄中使用 `dotnet new angular` 命令以建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="cc29a-114">Create a new project from a command prompt using the command `dotnet new angular` in an empty directory.</span></span> <span data-ttu-id="cc29a-115">例如，下列命令會在 *my-new-app* 目錄中建立應用程式並切換到該目錄：</span><span class="sxs-lookup"><span data-stu-id="cc29a-115">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new angular -o my-new-app
cd my-new-app
```

<span data-ttu-id="cc29a-116">從 Visual Studio 或 .NET Core CLI 執行該應用程式：</span><span class="sxs-lookup"><span data-stu-id="cc29a-116">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cc29a-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc29a-117">Visual Studio</span></span>](#tab/visual-studio/)

<span data-ttu-id="cc29a-118">開啟產生的 *.csproj* 檔案，然後在該處以一般方式來執行該應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc29a-118">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="cc29a-119">建置程序會在首次執行時還原 npm 相依性，這可能需要幾分鐘時間才能完成。</span><span class="sxs-lookup"><span data-stu-id="cc29a-119">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="cc29a-120">後續建置所需的時間將會縮短。</span><span class="sxs-lookup"><span data-stu-id="cc29a-120">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cc29a-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="cc29a-121">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="cc29a-122">請確定您有名為 `ASPNETCORE_Environment` 的環境變數，且其值為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="cc29a-122">Ensure you have an environment variable called `ASPNETCORE_Environment` with a value of `Development`.</span></span> <span data-ttu-id="cc29a-123">在 Windows 上 (於非 PowerShell 提示字元中)，執行 `SET ASPNETCORE_Environment=Development`。</span><span class="sxs-lookup"><span data-stu-id="cc29a-123">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="cc29a-124">在 Linux 或 macOS 上，執行 `export ASPNETCORE_Environment=Development`。</span><span class="sxs-lookup"><span data-stu-id="cc29a-124">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="cc29a-125">執行 [dotnet build](/dotnet/core/tools/dotnet-build) 以確認應用程式有正確建置。</span><span class="sxs-lookup"><span data-stu-id="cc29a-125">Run [dotnet build](/dotnet/core/tools/dotnet-build) to verify the app builds correctly.</span></span> <span data-ttu-id="cc29a-126">建置程序會在首次執行時還原 npm 相依性，這可能需要幾分鐘時間才能完成。</span><span class="sxs-lookup"><span data-stu-id="cc29a-126">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="cc29a-127">後續建置所需的時間將會縮短。</span><span class="sxs-lookup"><span data-stu-id="cc29a-127">Subsequent builds are much faster.</span></span>

<span data-ttu-id="cc29a-128">執行 [dotnet run](/dotnet/core/tools/dotnet-run) 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc29a-128">Run [dotnet run](/dotnet/core/tools/dotnet-run) to start the app.</span></span> <span data-ttu-id="cc29a-129">系統會記錄一則類似下面的訊息：</span><span class="sxs-lookup"><span data-stu-id="cc29a-129">A message similar to the following is logged:</span></span>

```console
Now listening on: http://localhost:<port>
```

<span data-ttu-id="cc29a-130">在瀏覽器中開啟這個 URL。</span><span class="sxs-lookup"><span data-stu-id="cc29a-130">Navigate to this URL in a browser.</span></span>

<span data-ttu-id="cc29a-131">應用程式會在背景啟動 Angular CLI 伺服器的執行個體。</span><span class="sxs-lookup"><span data-stu-id="cc29a-131">The app starts up an instance of the Angular CLI server in the background.</span></span> <span data-ttu-id="cc29a-132">系統會記錄一則類似下列的訊息：*NG Live Development Server is listening on localhost:&lt;otherport&gt;, open your browser on http://localhost:&lt;otherport&gt;/*.</span><span class="sxs-lookup"><span data-stu-id="cc29a-132">A message similar to the following is logged: *NG Live Development Server is listening on localhost:&lt;otherport&gt;, open your browser on http://localhost:&lt;otherport&gt;/*.</span></span> <span data-ttu-id="cc29a-133">請忽略此訊息&mdash;這**不是** ASP.NET Core 與 Angular CLI 應用程式的合併 URL。</span><span class="sxs-lookup"><span data-stu-id="cc29a-133">Ignore this message&mdash;it's **not** the URL for the combined ASP.NET Core and Angular CLI app.</span></span>

---

<span data-ttu-id="cc29a-134">專案範本會建立 ASP.NET Core 應用程式及 Angular 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc29a-134">The project template creates an ASP.NET Core app and an Angular app.</span></span> <span data-ttu-id="cc29a-135">ASP.NET Core 應用程式主要用於存取資料、授權以及其他伺服器端事項。</span><span class="sxs-lookup"><span data-stu-id="cc29a-135">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="cc29a-136">Angular 應用程式 (位於 *ClientApp* 子目錄中) 的目的是用於處理所有 UI 事項。</span><span class="sxs-lookup"><span data-stu-id="cc29a-136">The Angular app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="cc29a-137">新增頁面、影像、樣式、模組等等。</span><span class="sxs-lookup"><span data-stu-id="cc29a-137">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="cc29a-138">*ClientApp* 目錄包含標準 Angular CLI 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc29a-138">The *ClientApp* directory contains a standard Angular CLI app.</span></span> <span data-ttu-id="cc29a-139">如需詳細資訊，請參閱 [Angular 文件](https://github.com/angular/angular-cli/wiki) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="cc29a-139">See the official [Angular documentation](https://github.com/angular/angular-cli/wiki) for more information.</span></span>

<span data-ttu-id="cc29a-140">由此範本所建立的 Angular 應用程式，以及由 Angular CLI 本身所建立 (透過 `ng new`) 的 Angular 應用程式之間存在些許不同，不過應用程式的功能是一樣的。</span><span class="sxs-lookup"><span data-stu-id="cc29a-140">There are slight differences between the Angular app created by this template and the one created by Angular CLI itself (via `ng new`); however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="cc29a-141">由範本所建立的應用程式包含以 [Bootstrap](https://getbootstrap.com/) \(英文\) 為基礎的配置，以及基本的路由範例。</span><span class="sxs-lookup"><span data-stu-id="cc29a-141">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="run-ng-commands"></a><span data-ttu-id="cc29a-142">執行 ng 命令</span><span class="sxs-lookup"><span data-stu-id="cc29a-142">Run ng commands</span></span>

<span data-ttu-id="cc29a-143">在命令提示字元中，切換至 *ClientApp* 子目錄：</span><span class="sxs-lookup"><span data-stu-id="cc29a-143">In a command prompt, switch to the *ClientApp* subdirectory:</span></span>

```console
cd ClientApp
```

<span data-ttu-id="cc29a-144">如果您已經以全域的方式安裝 `ng` 工具，則可以執行它的任何命令。</span><span class="sxs-lookup"><span data-stu-id="cc29a-144">If you have the `ng` tool installed globally, you can run any of its commands.</span></span> <span data-ttu-id="cc29a-145">例如，您可以執行 `ng lint`、`ng test` 或任何其他 [Angular CLI 命令](https://github.com/angular/angular-cli/wiki#additional-commands) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="cc29a-145">For example, you can run `ng lint`, `ng test`, or any of the other [Angular CLI commands](https://github.com/angular/angular-cli/wiki#additional-commands).</span></span> <span data-ttu-id="cc29a-146">您並不需要執行 `ng serve`，因為 ASP.NET Core 應用程式會負責提供應用程式的伺服器端和用戶端部分。</span><span class="sxs-lookup"><span data-stu-id="cc29a-146">There's no need to run `ng serve` though, because your ASP.NET Core app deals with serving both server-side and client-side parts of your app.</span></span> <span data-ttu-id="cc29a-147">它會在內部針對開發使用 `ng serve`。</span><span class="sxs-lookup"><span data-stu-id="cc29a-147">Internally, it uses `ng serve` in development.</span></span>

<span data-ttu-id="cc29a-148">如果您尚未安裝 `ng` 工具，請改為執行 `npm run ng`。</span><span class="sxs-lookup"><span data-stu-id="cc29a-148">If you don't have the `ng` tool installed, run `npm run ng` instead.</span></span> <span data-ttu-id="cc29a-149">例如，您可以選擇執行 `npm run ng lint` 或 `npm run ng test`。</span><span class="sxs-lookup"><span data-stu-id="cc29a-149">For example, you can run `npm run ng lint` or `npm run ng test`.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="cc29a-150">安裝 npm 套件</span><span class="sxs-lookup"><span data-stu-id="cc29a-150">Install npm packages</span></span>

<span data-ttu-id="cc29a-151">若要安裝協力廠商 npm 套件，請在 *ClientApp* 子目錄中使用命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="cc29a-151">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="cc29a-152">例如: </span><span class="sxs-lookup"><span data-stu-id="cc29a-152">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="cc29a-153">發行與部署</span><span class="sxs-lookup"><span data-stu-id="cc29a-153">Publish and deploy</span></span>

<span data-ttu-id="cc29a-154">在開發時，應用程式會以方便開發人員操作的模式執行。</span><span class="sxs-lookup"><span data-stu-id="cc29a-154">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="cc29a-155">例如，JavaScript 組合會包含來源對應 (以便在偵錯時，您可以看到原始的 TypeScript 程式碼)。</span><span class="sxs-lookup"><span data-stu-id="cc29a-155">For example, JavaScript bundles include source maps (so that when debugging, you can see your original TypeScript code).</span></span> <span data-ttu-id="cc29a-156">應用程式會監看磁碟上的 TypeScript、HTML 和 CSS 檔案變更，並在發現檔案變更時，自動重新編譯並重新載入。</span><span class="sxs-lookup"><span data-stu-id="cc29a-156">The app watches for TypeScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="cc29a-157">在實際執行環境中，請提供具有最佳效能的應用程式版本。</span><span class="sxs-lookup"><span data-stu-id="cc29a-157">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="cc29a-158">這是設定為自動進行的。</span><span class="sxs-lookup"><span data-stu-id="cc29a-158">This is configured to happen automatically.</span></span> <span data-ttu-id="cc29a-159">當您發行時，組建組態會發出精簡、預先 (AoT) 編譯的用戶端程式碼組建。</span><span class="sxs-lookup"><span data-stu-id="cc29a-159">When you publish, the build configuration emits a minified, ahead-of-time (AoT) compiled build of your client-side code.</span></span> <span data-ttu-id="cc29a-160">不同於開發組建，實際執行組建不需要在伺服器上安裝 Node.js (除非您已啟用[伺服器端預先轉譯](#server-side-rendering))。</span><span class="sxs-lookup"><span data-stu-id="cc29a-160">Unlike the development build, the production build doesn't require Node.js to be installed on the server (unless you have enabled [server-side prerendering](#server-side-rendering)).</span></span>

<span data-ttu-id="cc29a-161">您可以使用標準 [ASP.NET Core 裝載和部署方法](xref:host-and-deploy/index)。</span><span class="sxs-lookup"><span data-stu-id="cc29a-161">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-ng-serve-independently"></a><span data-ttu-id="cc29a-162">獨立執行 "ng serve"</span><span class="sxs-lookup"><span data-stu-id="cc29a-162">Run "ng serve" independently</span></span>

<span data-ttu-id="cc29a-163">專案已設定為會在 ASP.NET Core 應用程式於開發模式中啟動時，在背景啟動屬於自己的 Angular CLI 伺服器執行個體。</span><span class="sxs-lookup"><span data-stu-id="cc29a-163">The project is configured to start its own instance of the Angular CLI server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="cc29a-164">這很方便，因為您不需要手動執行個別的伺服器。</span><span class="sxs-lookup"><span data-stu-id="cc29a-164">This is convenient because you don't have to run a separate server manually.</span></span>

<span data-ttu-id="cc29a-165">此預設設定有一個缺點。</span><span class="sxs-lookup"><span data-stu-id="cc29a-165">There's a drawback to this default setup.</span></span> <span data-ttu-id="cc29a-166">每當您修改 C# 程式碼，且需要重新啟動 ASP.NET Core 應用程式時，Angular CLI 伺服器都會重新啟動。</span><span class="sxs-lookup"><span data-stu-id="cc29a-166">Each time you modify your C# code and your ASP.NET Core app needs to restart, the Angular CLI server restarts.</span></span> <span data-ttu-id="cc29a-167">開始備份需要大約 10 秒鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="cc29a-167">Around 10 seconds is required to start back up.</span></span> <span data-ttu-id="cc29a-168">如果您經常編輯 C# 程式碼，但又不想要等候 Angular CLI 重新啟動，請獨立於 ASP.NET Core 處理序，於外部執行 Angular CLI 伺服器。</span><span class="sxs-lookup"><span data-stu-id="cc29a-168">If you're making frequent C# code edits and don't want to wait for Angular CLI to restart, run the Angular CLI server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="cc29a-169">方法如下：</span><span class="sxs-lookup"><span data-stu-id="cc29a-169">To do so:</span></span>

1. <span data-ttu-id="cc29a-170">在命令提示字元中，切換至 *ClientApp* 子目錄，然後啟動 Angular CLI 程式開發伺服器：</span><span class="sxs-lookup"><span data-stu-id="cc29a-170">In a command prompt, switch to the *ClientApp* subdirectory, and launch the Angular CLI development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="cc29a-171">使用 `npm start` 來啟動 Angular CLI 程式開發伺服器，而不是 `ng serve`，以遵守 *package.json* 中的設定。</span><span class="sxs-lookup"><span data-stu-id="cc29a-171">Use `npm start` to launch the Angular CLI development server, not `ng serve`, so that the configuration in *package.json* is respected.</span></span> <span data-ttu-id="cc29a-172">若要將其他參數傳遞給 Angular CLI 伺服器，請將它們新增至 *package.json* 檔案中相關的 `scripts` 行。</span><span class="sxs-lookup"><span data-stu-id="cc29a-172">To pass additional parameters to the Angular CLI server, add them to the relevant `scripts` line in your *package.json* file.</span></span>

2. <span data-ttu-id="cc29a-173">修改您的 ASP.NET Core 應用程式來使用外部的 Angular CLI 執行個體，而非自行啟動執行個體。</span><span class="sxs-lookup"><span data-stu-id="cc29a-173">Modify your ASP.NET Core app to use the external Angular CLI instance instead of launching one of its own.</span></span> <span data-ttu-id="cc29a-174">在 *Startup* 類別中，將 `spa.UseAngularCliServer` 引動過程取代為：</span><span class="sxs-lookup"><span data-stu-id="cc29a-174">In your *Startup* class, replace the `spa.UseAngularCliServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

<span data-ttu-id="cc29a-175">當您啟動 ASP.NET Core 應用程式時，它並不會啟動 Angular CLI 伺服器。</span><span class="sxs-lookup"><span data-stu-id="cc29a-175">When you start your ASP.NET Core app, it won't launch an Angular CLI server.</span></span> <span data-ttu-id="cc29a-176">而是改為使用您手動啟動的執行個體。</span><span class="sxs-lookup"><span data-stu-id="cc29a-176">The instance you started manually is used instead.</span></span> <span data-ttu-id="cc29a-177">這樣可以加快它的啟動和重新啟動速度。</span><span class="sxs-lookup"><span data-stu-id="cc29a-177">This enables it to start and restart faster.</span></span> <span data-ttu-id="cc29a-178">它再也不必每次都得等候 Angular CLI 重建您的用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc29a-178">It's no longer waiting for Angular CLI to rebuild your client app each time.</span></span>

## <a name="server-side-rendering"></a><span data-ttu-id="cc29a-179">伺服器端轉譯</span><span class="sxs-lookup"><span data-stu-id="cc29a-179">Server-side rendering</span></span>

<span data-ttu-id="cc29a-180">作為效能功能，您可以選擇在伺服器上預先轉譯 Angular 應用程式，以及在用戶端上執行它。</span><span class="sxs-lookup"><span data-stu-id="cc29a-180">As a performance feature, you can choose to pre-render your Angular app on the server as well as running it on the client.</span></span> <span data-ttu-id="cc29a-181">這表示瀏覽器接收到代表應用程式起始 UI 的 HTML 標記，因此瀏覽器會在下載及執行您的 JavaScript 組合之前先顯示 UI。</span><span class="sxs-lookup"><span data-stu-id="cc29a-181">This means that browsers receive HTML markup representing your app's initial UI, so they display it even before downloading and executing your JavaScript bundles.</span></span> <span data-ttu-id="cc29a-182">這些實作大部分是來自稱為 [Angular Universal](https://universal.angular.io/) \(英文\) 的 Angular 功能。</span><span class="sxs-lookup"><span data-stu-id="cc29a-182">Most of the implementation of this comes from an Angular feature called [Angular Universal](https://universal.angular.io/).</span></span>

> [!TIP]
> <span data-ttu-id="cc29a-183">啟用伺服器端轉譯 (SSR) 會在開發和部署期間引入數個額外的複雜性。</span><span class="sxs-lookup"><span data-stu-id="cc29a-183">Enabling server-side rendering (SSR) introduces a number of extra complications both during development and deployment.</span></span> <span data-ttu-id="cc29a-184">請參閱 [SSR 的缺點 ](#drawbacks-of-ssr) 以判斷 SSR 是否適合您的需求。</span><span class="sxs-lookup"><span data-stu-id="cc29a-184">Read [drawbacks of SSR](#drawbacks-of-ssr) to determine if SSR is a good fit for your requirements.</span></span>

<span data-ttu-id="cc29a-185">若要啟用 SSR，您必須對專案進行數個額外的處理。</span><span class="sxs-lookup"><span data-stu-id="cc29a-185">To enable SSR, you need to make a number of additions to your project.</span></span>

<span data-ttu-id="cc29a-186">在 *Startup* 類別中，在設定 `spa.Options.SourcePath` 的行*之後*，以及在呼叫 `UseAngularCliServer` 或 `UseProxyToSpaDevelopmentServer` 的行*之前*，新增下列內容：</span><span class="sxs-lookup"><span data-stu-id="cc29a-186">In the *Startup* class, *after* the line that configures `spa.Options.SourcePath`, and *before* the call to `UseAngularCliServer` or `UseProxyToSpaDevelopmentServer`, add the following:</span></span>

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

<span data-ttu-id="cc29a-187">在開發模式中，此程式碼會嘗試透過執行指令碼 `build:ssr` (於 *ClientApp\package.json* 中定義) 來建置 SSR 組合。</span><span class="sxs-lookup"><span data-stu-id="cc29a-187">In development mode, this code attempts to build the SSR bundle by running the script `build:ssr`, which is defined in *ClientApp\package.json*.</span></span> <span data-ttu-id="cc29a-188">這會建置名為 `ssr`的 Angular 應用程式 (其尚未定義)。</span><span class="sxs-lookup"><span data-stu-id="cc29a-188">This builds an Angular app named `ssr`, which isn't yet defined.</span></span>

<span data-ttu-id="cc29a-189">在 *ClientApp/.angular-cli.json* 的 `apps` 陣列尾端，定義名稱為 `ssr` 的額外應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc29a-189">At the end of the `apps` array in *ClientApp/.angular-cli.json*, define an extra app with name `ssr`.</span></span> <span data-ttu-id="cc29a-190">使用下列選項：</span><span class="sxs-lookup"><span data-stu-id="cc29a-190">Use the following options:</span></span>

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

<span data-ttu-id="cc29a-191">這個已啟用 SSR 的新應用程式組態需要兩個額外的檔案：*tsconfig.server.json* 和 *main.server.ts*。</span><span class="sxs-lookup"><span data-stu-id="cc29a-191">This new SSR-enabled app configuration requires two further files: *tsconfig.server.json* and *main.server.ts*.</span></span> <span data-ttu-id="cc29a-192">*tsconfig.server.json* 檔案會指定 TypeScript 編譯選項。</span><span class="sxs-lookup"><span data-stu-id="cc29a-192">The *tsconfig.server.json* file specifies TypeScript compilation options.</span></span> <span data-ttu-id="cc29a-193">*main.server.ts* 檔案會在 SSR 期間作為程式碼進入點。</span><span class="sxs-lookup"><span data-stu-id="cc29a-193">The *main.server.ts* file serves as the code entry point during SSR.</span></span>

<span data-ttu-id="cc29a-194">在 *ClientApp/src* 內新增名為 *tsconfig.server.json* (伴隨現有的 *tsconfig.app.json*)，其中包含下列項目：</span><span class="sxs-lookup"><span data-stu-id="cc29a-194">Add a new file called *tsconfig.server.json* inside *ClientApp/src* (alongside the existing *tsconfig.app.json*), containing the following:</span></span>

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

<span data-ttu-id="cc29a-195">此檔案會設定 Angular 的 AoT 編譯器，以尋找名為 `app.server.module` 的模組。</span><span class="sxs-lookup"><span data-stu-id="cc29a-195">This file configures Angular's AoT compiler to look for a module called `app.server.module`.</span></span> <span data-ttu-id="cc29a-196">透過在 *ClientApp/src/app/app.server.module.ts* 建立新檔案 (伴隨現有的 *app.module.ts*) 並包含下列項目來新增它：</span><span class="sxs-lookup"><span data-stu-id="cc29a-196">Add this by creating a new file at *ClientApp/src/app/app.server.module.ts* (alongside the existing *app.module.ts*) containing the following:</span></span>

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

<span data-ttu-id="cc29a-197">此模組會繼承自您的用戶端 `app.module`，並定義 SSR 期間可用的額外 Angular 模組。</span><span class="sxs-lookup"><span data-stu-id="cc29a-197">This module inherits from your client-side `app.module` and defines which extra Angular modules are available during SSR.</span></span>

<span data-ttu-id="cc29a-198">回想 *.angular-cli.json* 中的新 `ssr` 項目，有參考名為 *main.server.ts* 的進入點檔案。</span><span class="sxs-lookup"><span data-stu-id="cc29a-198">Recall that the new `ssr` entry in *.angular-cli.json* referenced an entry point file called *main.server.ts*.</span></span> <span data-ttu-id="cc29a-199">您尚未新增該檔案，現在就是新增它的時機。</span><span class="sxs-lookup"><span data-stu-id="cc29a-199">You haven't yet added that file, and now is time to do so.</span></span> <span data-ttu-id="cc29a-200">在 *ClientApp/src/main.server.ts* 建立新檔案 (伴隨現有的 *main.ts*)，其中包含下列項目：</span><span class="sxs-lookup"><span data-stu-id="cc29a-200">Create a new file at *ClientApp/src/main.server.ts* (alongside the existing *main.ts*), containing the following:</span></span>

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

<span data-ttu-id="cc29a-201">此檔案的程式碼便是在 ASP.NET Core 執行您新增至 *Startup* 類別的 `UseSpaPrerendering` 中介軟體時，會針對每個要求執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="cc29a-201">This file's code is what ASP.NET Core executes for each request when it runs the `UseSpaPrerendering` middleware that you added to the *Startup* class.</span></span> <span data-ttu-id="cc29a-202">它負責處理接收來自 .NET 程式碼的 `params` (例如要求的 URL)，以及呼叫 Angular SSR API 來取得產生的 HTML。</span><span class="sxs-lookup"><span data-stu-id="cc29a-202">It deals with receiving `params` from the .NET code (such as the URL being requested), and making calls to Angular SSR APIs to get the resulting HTML.</span></span>

<span data-ttu-id="cc29a-203">嚴格來說，這已足夠在開發模式中啟用 SSR。</span><span class="sxs-lookup"><span data-stu-id="cc29a-203">Strictly-speaking, this is sufficient to enable SSR in development mode.</span></span> <span data-ttu-id="cc29a-204">請務必做出最後一項變更，以確保應用程式能在發行時正常執行。</span><span class="sxs-lookup"><span data-stu-id="cc29a-204">It's essential to make one final change so that your app works correctly when published.</span></span> <span data-ttu-id="cc29a-205">在應用程式的主要 *.csproj* 檔案中，將 `BuildServerSideRenderer` 屬性值設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="cc29a-205">In your app's main *.csproj* file, set the `BuildServerSideRenderer` property value to `true`:</span></span>

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

<span data-ttu-id="cc29a-206">這樣會設定建置流程以在發行期間執行 `build:ssr`，並將 SSR 檔案部署至伺服器。</span><span class="sxs-lookup"><span data-stu-id="cc29a-206">This configures the build process to run `build:ssr` during publishing and deploy the SSR files to the server.</span></span> <span data-ttu-id="cc29a-207">如果您未啟用此功能，SSR 將會在實際執行時失敗。</span><span class="sxs-lookup"><span data-stu-id="cc29a-207">If you don't enable this, SSR fails in production.</span></span>

<span data-ttu-id="cc29a-208">當您的應用程式於開發或實際執行模式中執行時，Angular 程式碼會預先在伺服器上轉譯為 HTML。</span><span class="sxs-lookup"><span data-stu-id="cc29a-208">When your app runs in either development or production mode, the Angular code pre-renders as HTML on the server.</span></span> <span data-ttu-id="cc29a-209">用戶端程式碼會以一般的方式執行。</span><span class="sxs-lookup"><span data-stu-id="cc29a-209">The client-side code executes as normal.</span></span>

### <a name="pass-data-from-net-code-into-typescript-code"></a><span data-ttu-id="cc29a-210">將資料從.NET 程式碼傳遞至 TypeScript 程式碼</span><span class="sxs-lookup"><span data-stu-id="cc29a-210">Pass data from .NET code into TypeScript code</span></span>

<span data-ttu-id="cc29a-211">在 SSR 期間，您可能會想要將針對每個要求的資料從 ASP.NET Core 應用程式傳遞至 Angular 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc29a-211">During SSR, you might want to pass per-request data from your ASP.NET Core app into your Angular app.</span></span> <span data-ttu-id="cc29a-212">例如，您可以傳遞 cookie 資訊，或是從資料庫讀取的某些資料。</span><span class="sxs-lookup"><span data-stu-id="cc29a-212">For example, you could pass cookie information or something read from a database.</span></span> <span data-ttu-id="cc29a-213">若要這樣做，請編輯您的 *Startup* 類別。</span><span class="sxs-lookup"><span data-stu-id="cc29a-213">To do this, edit your *Startup* class.</span></span> <span data-ttu-id="cc29a-214">在 `UseSpaPrerendering` 的回呼中，請將 `options.SupplyData` 的值設定為下方所示的內容：</span><span class="sxs-lookup"><span data-stu-id="cc29a-214">In the callback for `UseSpaPrerendering`, set a value for `options.SupplyData` such as the following:</span></span>

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

<span data-ttu-id="cc29a-215">`SupplyData` 回呼可讓您傳遞任意、針對每個要求、JSON 可序列化的資料 (例如，字串、布林值或數字)。</span><span class="sxs-lookup"><span data-stu-id="cc29a-215">The `SupplyData` callback lets you pass arbitrary, per-request, JSON-serializable data (for example, strings, booleans, or numbers).</span></span> <span data-ttu-id="cc29a-216">您的 *main.server.ts* 程式碼會以 `params.data` 的形式接收此資料。</span><span class="sxs-lookup"><span data-stu-id="cc29a-216">Your *main.server.ts* code receives this as `params.data`.</span></span> <span data-ttu-id="cc29a-217">例如，上述程式碼範例會將布林值以 `params.data.isHttpsRequest` 的形式傳遞至 `createServerRenderer` 回呼。</span><span class="sxs-lookup"><span data-stu-id="cc29a-217">For example, the preceding code sample passes a boolean value as `params.data.isHttpsRequest` into the `createServerRenderer` callback.</span></span> <span data-ttu-id="cc29a-218">您可以透過 Angular 所支援的任何方式，將此傳遞至應用程式的其他部分。</span><span class="sxs-lookup"><span data-stu-id="cc29a-218">You can pass this to other parts of your app in any way supported by Angular.</span></span> <span data-ttu-id="cc29a-219">例如，請查看 *main.server.ts* 如何將 `BASE_URL` 值傳遞至任何其建構函式已宣告要接收此值的元件。</span><span class="sxs-lookup"><span data-stu-id="cc29a-219">For example, see how *main.server.ts* passes the `BASE_URL` value to any component whose constructor is declared to receive it.</span></span>

### <a name="drawbacks-of-ssr"></a><span data-ttu-id="cc29a-220">SSR 的缺點</span><span class="sxs-lookup"><span data-stu-id="cc29a-220">Drawbacks of SSR</span></span>

<span data-ttu-id="cc29a-221">並非所有應用程式都能從 SSR 獲益。</span><span class="sxs-lookup"><span data-stu-id="cc29a-221">Not all apps benefit from SSR.</span></span> <span data-ttu-id="cc29a-222">主要優點為可感知的效能。</span><span class="sxs-lookup"><span data-stu-id="cc29a-222">The primary benefit is perceived performance.</span></span> <span data-ttu-id="cc29a-223">透過低速網路連線或低速行動裝置連線至您應用程式的訪客，將能很快地看到初始的 UI，即便需要花上一點時間來擷取或剖析 JavaScript 組合。</span><span class="sxs-lookup"><span data-stu-id="cc29a-223">Visitors reaching your app over a slow network connection or on slow mobile devices see the initial UI quickly, even if it takes a while to fetch or parse the JavaScript bundles.</span></span> <span data-ttu-id="cc29a-224">不過，很多 SPA 主要都是透過使用高速內部公司網路的高速電腦來使用，因此應用程式幾乎會立即出現。</span><span class="sxs-lookup"><span data-stu-id="cc29a-224">However, many SPAs are mainly used over fast, internal company networks on fast computers where the app appears almost instantly.</span></span>

<span data-ttu-id="cc29a-225">同時，啟用 SSR 會有很多明顯的缺點。</span><span class="sxs-lookup"><span data-stu-id="cc29a-225">At the same time, there are significant drawbacks to enabling SSR.</span></span> <span data-ttu-id="cc29a-226">它會增加開發程序的複雜性。</span><span class="sxs-lookup"><span data-stu-id="cc29a-226">It adds complexity to your development process.</span></span> <span data-ttu-id="cc29a-227">您的程式碼必須在兩個不同環境中執行：用戶端和伺服器端 (在一個從 ASP.NET Core 叫用的 Node.js 環境中)。</span><span class="sxs-lookup"><span data-stu-id="cc29a-227">Your code must run in two different environments: client-side and server-side (in a Node.js environment invoked from ASP.NET Core).</span></span> <span data-ttu-id="cc29a-228">以下是需要考量的事項：</span><span class="sxs-lookup"><span data-stu-id="cc29a-228">Here are some things to bear in mind:</span></span>

* <span data-ttu-id="cc29a-229">SSR 需要在您的實際執行伺服器上安裝 Node.js。</span><span class="sxs-lookup"><span data-stu-id="cc29a-229">SSR requires a Node.js installation on your production servers.</span></span> <span data-ttu-id="cc29a-230">這針對某些部署案例 (例如 Azure App Service) 是會自動成立的，不過並不適用於其他如 Azure Service Fabric 的案例。</span><span class="sxs-lookup"><span data-stu-id="cc29a-230">This is automatically the case for some deployment scenarios, such as Azure App Services, but not for others, such as Azure Service Fabric.</span></span>
* <span data-ttu-id="cc29a-231">啟用 `BuildServerSideRenderer` 組建旗標會使您的 *node_modules* 目錄進行發行。</span><span class="sxs-lookup"><span data-stu-id="cc29a-231">Enabling the `BuildServerSideRenderer` build flag causes your *node_modules* directory to publish.</span></span> <span data-ttu-id="cc29a-232">此資料夾包含超過 20,000 個檔案，這將會增加部署時間。</span><span class="sxs-lookup"><span data-stu-id="cc29a-232">This folder contains 20,000+ files, which increases deployment time.</span></span>
* <span data-ttu-id="cc29a-233">若要在 Node.js 環境中執行您的程式碼，它不能仰賴瀏覽器特定 JavaScript API (例如 `window` 或 `localStorage`) 的存在。</span><span class="sxs-lookup"><span data-stu-id="cc29a-233">To run your code in a Node.js environment, it can't rely on the existence of browser-specific JavaScript APIs such as `window` or `localStorage`.</span></span> <span data-ttu-id="cc29a-234">如果您的程式碼 (或是您參考的某些協力廠商程式庫) 嘗試使用這些 API，則會在 SSR 期間出現錯誤。</span><span class="sxs-lookup"><span data-stu-id="cc29a-234">If your code (or some third-party library you reference) tries to use these APIs, you'll get an error during SSR.</span></span> <span data-ttu-id="cc29a-235">例如，不要使用 jQuery，因為它會在許多位置參考瀏覽器特定 API。</span><span class="sxs-lookup"><span data-stu-id="cc29a-235">For example, don't use jQuery because it references browser-specific APIs in many places.</span></span> <span data-ttu-id="cc29a-236">若要防止錯誤，您必須避免使用 SSR，或是避免使用瀏覽器特定 API 或程式庫。</span><span class="sxs-lookup"><span data-stu-id="cc29a-236">To prevent errors, you must either avoid SSR or avoid browser-specific APIs or libraries.</span></span> <span data-ttu-id="cc29a-237">您可以將針對這類 API 的呼叫包裝於檢查中，以確保它們不會在 SSR 期間被叫用。</span><span class="sxs-lookup"><span data-stu-id="cc29a-237">You can wrap any calls to such APIs in checks to ensure they aren't invoked during SSR.</span></span> <span data-ttu-id="cc29a-238">例如，在 JavaScript 或 TypeScript 程式碼中使用類似下列的檢查：</span><span class="sxs-lookup"><span data-stu-id="cc29a-238">For example, use a check such as the following in JavaScript or TypeScript code:</span></span>

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
