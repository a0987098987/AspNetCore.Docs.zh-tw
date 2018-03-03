---
title: "使用 React 專案範本"
author: SteveSandersonMS
description: "了解如何開始使用 ASP.NET Core 單一頁面應用程式 (SPA) 專案範本 React，以及建立 react 應用程式。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: cda9f52d1f5fa1d240e210488bf1bd5c76e49be7
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="use-the-react-project-template"></a><span data-ttu-id="598af-103">使用 React 專案範本</span><span class="sxs-lookup"><span data-stu-id="598af-103">Use the React project template</span></span>

> [!NOTE]
> <span data-ttu-id="598af-104">這份文件不需 React 專案範本包含 ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="598af-104">This documentation isn't about the React project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="598af-105">它是有關，您可以手動更新較新的回應範本。</span><span class="sxs-lookup"><span data-stu-id="598af-105">It's about the newer React template to which you can update manually.</span></span> <span data-ttu-id="598af-106">預設範本包含 ASP.NET Core 2.1。</span><span class="sxs-lookup"><span data-stu-id="598af-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

<span data-ttu-id="598af-107">更新的 React 專案範本提供方便的起點適用於 ASP.NET Core 應用程式使用 React 和[建立 react 應用程式](https://github.com/facebookincubator/create-react-app)(CRA) 慣例，來實作豐富的用戶端使用者介面 (UI)。</span><span class="sxs-lookup"><span data-stu-id="598af-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="598af-108">範本就相當於建立做為 API 後端的 ASP.NET Core 專案和標準 CRA 反應專案，以做為 UI，但方便裝載兩者可以建置並當做單一單位所發行的單一應用程式專案中。</span><span class="sxs-lookup"><span data-stu-id="598af-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="598af-109">建立新的應用程式</span><span class="sxs-lookup"><span data-stu-id="598af-109">Create a new app</span></span>

<span data-ttu-id="598af-110">如果使用 ASP.NET Core 2.0，請確定您已經[安裝更新後的 React 專案範本](xref:spa/index#installation)。</span><span class="sxs-lookup"><span data-stu-id="598af-110">If using ASP.NET Core 2.0, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span> <span data-ttu-id="598af-111">如果您有 ASP.NET Core 2.1，沒有需要安裝它。</span><span class="sxs-lookup"><span data-stu-id="598af-111">If you have ASP.NET Core 2.1, there's no need to install it.</span></span>

<span data-ttu-id="598af-112">建立新的專案，從命令提示字元使用命令`dotnet new react`空的目錄中。</span><span class="sxs-lookup"><span data-stu-id="598af-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="598af-113">例如，下列命令會建立應用程式在*my-新的應用程式*目錄並切換到該目錄：</span><span class="sxs-lookup"><span data-stu-id="598af-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new react -o my-new-app
cd my-new-app
```

<span data-ttu-id="598af-114">執行應用程式，從 Visual Studio 或.NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="598af-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="598af-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="598af-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="598af-116">開啟產生*.csproj*檔案，並從該處，像平常一樣執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="598af-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="598af-117">在建置程序還原第一次執行，可能需要幾分鐘的 npm 相依性。</span><span class="sxs-lookup"><span data-stu-id="598af-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="598af-118">後續建置會更快。</span><span class="sxs-lookup"><span data-stu-id="598af-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="598af-119">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="598af-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="598af-120">請確定您有環境變數呼叫`ASPNETCORE_Environment`值是`Development`。</span><span class="sxs-lookup"><span data-stu-id="598af-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="598af-121">在 Windows 上 （在非 PowerShell 提示） 中，執行`SET ASPNETCORE_Environment=Development`。</span><span class="sxs-lookup"><span data-stu-id="598af-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="598af-122">在 Linux 或 macOS 上，執行`export ASPNETCORE_Environment=Development`。</span><span class="sxs-lookup"><span data-stu-id="598af-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="598af-123">執行[dotnet 組建](/dotnet/core/tools/dotnet-build)以確認您的應用程式建置正確。</span><span class="sxs-lookup"><span data-stu-id="598af-123">Run [dotnet build](/dotnet/core/tools/dotnet-build) to verify your app builds correctly.</span></span> <span data-ttu-id="598af-124">在第一個執行中，在建置程序還原 npm 相依性，這可能要花費幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="598af-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="598af-125">後續建置會更快。</span><span class="sxs-lookup"><span data-stu-id="598af-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="598af-126">執行[dotnet 執行](/dotnet/core/tools/dotnet-run)啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="598af-126">Run [dotnet run](/dotnet/core/tools/dotnet-run) to start the app.</span></span>

---

<span data-ttu-id="598af-127">專案範本建立 ASP.NET Core 應用程式與 React 應用程式。</span><span class="sxs-lookup"><span data-stu-id="598af-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="598af-128">ASP.NET Core 應用程式被要用於資料存取、 授權和其他伺服器端的問題。</span><span class="sxs-lookup"><span data-stu-id="598af-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="598af-129">回應應用程式時，位於*ClientApp*子目錄，要用於所有 UI 疑慮。</span><span class="sxs-lookup"><span data-stu-id="598af-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="598af-130">新增頁面、 影像、 樣式、 模組、 等等。</span><span class="sxs-lookup"><span data-stu-id="598af-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="598af-131">*ClientApp*目錄是標準 CRA 回應應用程式。</span><span class="sxs-lookup"><span data-stu-id="598af-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="598af-132">請參閱正式[CRA 文件](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="598af-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="598af-133">有此範本所建立的 React 應用程式和屬性之間建立 CRA 本身; 有些微的差異不過，應用程式的功能並不會變更。</span><span class="sxs-lookup"><span data-stu-id="598af-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="598af-134">範本所建立的應用程式包含[Bootstrap](https://getbootstrap.com/)為基礎的配置和基本的路由範例。</span><span class="sxs-lookup"><span data-stu-id="598af-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="598af-135">安裝 npm 封裝</span><span class="sxs-lookup"><span data-stu-id="598af-135">Install npm packages</span></span>

<span data-ttu-id="598af-136">若要安裝協力廠商 npm 封裝，請使用 命令提示字元中*ClientApp*子目錄。</span><span class="sxs-lookup"><span data-stu-id="598af-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="598af-137">例如: </span><span class="sxs-lookup"><span data-stu-id="598af-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="598af-138">發行與部署</span><span class="sxs-lookup"><span data-stu-id="598af-138">Publish and deploy</span></span>

<span data-ttu-id="598af-139">在開發中，應用程式以方便開發人員最佳化模式執行。</span><span class="sxs-lookup"><span data-stu-id="598af-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="598af-140">例如，JavaScript 組合會包含來源對應 （以便偵錯時，您可以看到原始的原始程式碼）。</span><span class="sxs-lookup"><span data-stu-id="598af-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="598af-141">應用程式監看 JavaScript、 HTML 和 CSS 檔案在磁碟上的變更，並自動重新編譯並重新載入時看到這些變更的檔案。</span><span class="sxs-lookup"><span data-stu-id="598af-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="598af-142">在生產環境中，提供您已針對效能最佳化的應用程式的版本的服務。</span><span class="sxs-lookup"><span data-stu-id="598af-142">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="598af-143">這會設定為自動進行。</span><span class="sxs-lookup"><span data-stu-id="598af-143">This is configured to happen automatically.</span></span> <span data-ttu-id="598af-144">當您發行時，組建組態就會發出縮短，transpiled 建置您的用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="598af-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="598af-145">不同於開發組建中，在正式組建並不需要在伺服器上安裝 Node.js。</span><span class="sxs-lookup"><span data-stu-id="598af-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="598af-146">您可以使用標準[ASP.NET Core 裝載和部署方法](xref:host-and-deploy/index)。</span><span class="sxs-lookup"><span data-stu-id="598af-146">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="598af-147">獨立執行 CRA 伺服器</span><span class="sxs-lookup"><span data-stu-id="598af-147">Run the CRA server independently</span></span>

<span data-ttu-id="598af-148">專案已設定為在背景中啟動 CRA 程式開發伺服器執行個體各自獨立，ASP.NET Core 應用程式開發模式中啟動。</span><span class="sxs-lookup"><span data-stu-id="598af-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="598af-149">這是很方便，因為這表示您不必手動執行不同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="598af-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="598af-150">沒有這個預設安裝的缺點。</span><span class="sxs-lookup"><span data-stu-id="598af-150">There's a drawback to this default setup.</span></span> <span data-ttu-id="598af-151">每當您修改 C# 程式碼，您必須重新啟動，應用程式的 ASP.NET 核心 CRA 伺服器重新啟動。</span><span class="sxs-lookup"><span data-stu-id="598af-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="598af-152">幾秒鐘的時間才能重新啟動。</span><span class="sxs-lookup"><span data-stu-id="598af-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="598af-153">如果您正在經常 C# 程式碼編輯，而且不想等候 CRA 伺服器重新啟動，CRA 伺服器外部外獨立執行的 ASP.NET 核心程序。</span><span class="sxs-lookup"><span data-stu-id="598af-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="598af-154">若要這樣做：</span><span class="sxs-lookup"><span data-stu-id="598af-154">To do so:</span></span>

1. <span data-ttu-id="598af-155">在命令提示字元切換至*ClientApp*子目錄，並啟動 CRA 程式開發伺服器：</span><span class="sxs-lookup"><span data-stu-id="598af-155">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

2. <span data-ttu-id="598af-156">修改您的 ASP.NET Core 應用程式，而不是啟動其自己的其中一個使用外部 CRA 伺服器執行個體。</span><span class="sxs-lookup"><span data-stu-id="598af-156">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="598af-157">在您*啟動*類別中，取代`spa.UseReactDevelopmentServer`引動過程為下列：</span><span class="sxs-lookup"><span data-stu-id="598af-157">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="598af-158">當您啟動 ASP.NET Core 應用程式，它將不會啟動 CRA 伺服器。</span><span class="sxs-lookup"><span data-stu-id="598af-158">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="598af-159">改為使用您以手動方式啟動的執行個體。</span><span class="sxs-lookup"><span data-stu-id="598af-159">The instance you started manually is used instead.</span></span> <span data-ttu-id="598af-160">這可讓它啟動並重新啟動速度。</span><span class="sxs-lookup"><span data-stu-id="598af-160">This enables it to start and restart faster.</span></span> <span data-ttu-id="598af-161">不會再等候 React 應用程式以重建每一次。</span><span class="sxs-lookup"><span data-stu-id="598af-161">It's no longer waiting for your React app to rebuild each time.</span></span>
