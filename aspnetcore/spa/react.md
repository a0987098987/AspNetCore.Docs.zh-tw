---
title: "使用 React 專案範本"
author: SteveSandersonMS
description: "了解如何開始使用 ASP.NET Core 單一頁面應用程式 (SPA) 發行候選版本專案範本 React，以及建立 react 應用程式。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: 5978094083a098a771f5dca103434ea8fcce7777
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-react-project-template-release-candidate"></a><span data-ttu-id="4301e-103">使用 React 專案範本 （發行候選版本）</span><span class="sxs-lookup"><span data-stu-id="4301e-103">Use the React project template (release candidate)</span></span>

> [!NOTE]
> <span data-ttu-id="4301e-104">這份文件不需發行 React 專案範本。</span><span class="sxs-lookup"><span data-stu-id="4301e-104">This documentation isn't about the released React project template.</span></span> <span data-ttu-id="4301e-105">**這份文件是關於 React 範本的發行候選版本。**</span><span class="sxs-lookup"><span data-stu-id="4301e-105">**This documentation is about the release candidate of the React template.**</span></span> <span data-ttu-id="4301e-106">我們希望在早期 2018年出貨的發行的版本。</span><span class="sxs-lookup"><span data-stu-id="4301e-106">We hope to ship the released version in early 2018.</span></span>

<span data-ttu-id="4301e-107">更新的 React 專案範本提供方便的起點適用於 ASP.NET Core 應用程式使用 React 和[建立 react 應用程式](https://github.com/facebookincubator/create-react-app)(CRA) 慣例，來實作豐富的用戶端使用者介面 (UI)。</span><span class="sxs-lookup"><span data-stu-id="4301e-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="4301e-108">範本就相當於建立做為 API 後端的 ASP.NET Core 專案和標準 CRA 反應專案，以做為 UI，但方便裝載兩者可以建置並當做單一單位所發行的單一應用程式專案中。</span><span class="sxs-lookup"><span data-stu-id="4301e-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="4301e-109">建立新的應用程式</span><span class="sxs-lookup"><span data-stu-id="4301e-109">Create a new app</span></span>

<span data-ttu-id="4301e-110">若要開始使用，請確定您已經[安裝更新後的 React 專案範本](xref:spa/index#installation)。</span><span class="sxs-lookup"><span data-stu-id="4301e-110">To get started, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span> <span data-ttu-id="4301e-111">這些指示不適用於先前的 React 專案範本包含在.NET Core 2.0.x 版 SDK。</span><span class="sxs-lookup"><span data-stu-id="4301e-111">These instructions don't apply to the previous React project template included in the .NET Core 2.0.x SDK.</span></span>

<span data-ttu-id="4301e-112">建立新的專案，從命令提示字元使用命令`dotnet new react`空的目錄中。</span><span class="sxs-lookup"><span data-stu-id="4301e-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="4301e-113">例如，下列命令會建立應用程式在*my-新的應用程式*目錄並切換到該目錄：</span><span class="sxs-lookup"><span data-stu-id="4301e-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new react -o my-new-app
cd my-new-app
```

<span data-ttu-id="4301e-114">執行應用程式，從 Visual Studio 或.NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="4301e-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4301e-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4301e-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4301e-116">開啟產生*.csproj*檔案，並從該處，像平常一樣執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="4301e-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="4301e-117">在建置程序還原第一次執行，可能需要幾分鐘的 npm 相依性。</span><span class="sxs-lookup"><span data-stu-id="4301e-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="4301e-118">後續建置會更快。</span><span class="sxs-lookup"><span data-stu-id="4301e-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4301e-119">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4301e-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4301e-120">請確定您有環境變數呼叫`ASPNETCORE_Environment`值是`Development`。</span><span class="sxs-lookup"><span data-stu-id="4301e-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="4301e-121">在 Windows 上 （在非 PowerShell 提示） 中，執行`SET ASPNETCORE_Environment=Development`。</span><span class="sxs-lookup"><span data-stu-id="4301e-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="4301e-122">在 Linux 或 macOS 上，執行`export ASPNETCORE_Environment=Development`。</span><span class="sxs-lookup"><span data-stu-id="4301e-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="4301e-123">執行`dotnet build`以確認您的應用程式建置正確。</span><span class="sxs-lookup"><span data-stu-id="4301e-123">Run `dotnet build` to verify your app builds correctly.</span></span> <span data-ttu-id="4301e-124">在第一個執行中，在建置程序還原 npm 相依性，這可能要花費幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="4301e-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="4301e-125">後續建置會更快。</span><span class="sxs-lookup"><span data-stu-id="4301e-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="4301e-126">執行`dotnet run`啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="4301e-126">Run `dotnet run` to start the app.</span></span>

---

<span data-ttu-id="4301e-127">專案範本建立 ASP.NET Core 應用程式與 React 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4301e-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="4301e-128">ASP.NET Core 應用程式被要用於資料存取、 授權和其他伺服器端的問題。</span><span class="sxs-lookup"><span data-stu-id="4301e-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="4301e-129">回應應用程式時，位於*ClientApp*子目錄，要用於所有 UI 疑慮。</span><span class="sxs-lookup"><span data-stu-id="4301e-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="4301e-130">新增頁面、 影像、 樣式、 模組、 等等。</span><span class="sxs-lookup"><span data-stu-id="4301e-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="4301e-131">*ClientApp*目錄是標準 CRA 回應應用程式。</span><span class="sxs-lookup"><span data-stu-id="4301e-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="4301e-132">請參閱正式[CRA 文件](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="4301e-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="4301e-133">有此範本所建立的 React 應用程式和屬性之間建立 CRA 本身; 有些微的差異不過，應用程式的功能並不會變更。</span><span class="sxs-lookup"><span data-stu-id="4301e-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="4301e-134">範本所建立的應用程式包含[Bootstrap](https://getbootstrap.com/)為基礎的配置和基本的路由範例。</span><span class="sxs-lookup"><span data-stu-id="4301e-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="4301e-135">安裝 npm 封裝</span><span class="sxs-lookup"><span data-stu-id="4301e-135">Install npm packages</span></span>

<span data-ttu-id="4301e-136">若要安裝協力廠商 npm 封裝，請使用 命令提示字元中*ClientApp*子目錄。</span><span class="sxs-lookup"><span data-stu-id="4301e-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="4301e-137">例如: </span><span class="sxs-lookup"><span data-stu-id="4301e-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="4301e-138">發行與部署</span><span class="sxs-lookup"><span data-stu-id="4301e-138">Publish and deploy</span></span>

<span data-ttu-id="4301e-139">在開發中，應用程式以方便開發人員最佳化模式執行。</span><span class="sxs-lookup"><span data-stu-id="4301e-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="4301e-140">例如，JavaScript 組合會包含來源對應 （以便偵錯時，您可以看到原始的原始程式碼）。</span><span class="sxs-lookup"><span data-stu-id="4301e-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="4301e-141">應用程式監看 JavaScript、 HTML 和 CSS 檔案在磁碟上的變更，並自動重新編譯並重新載入時看到這些變更的檔案。</span><span class="sxs-lookup"><span data-stu-id="4301e-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="4301e-142">在生產環境中，提供您已針對效能最佳化的應用程式的版本的服務。</span><span class="sxs-lookup"><span data-stu-id="4301e-142">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="4301e-143">這會設定為自動進行。</span><span class="sxs-lookup"><span data-stu-id="4301e-143">This is configured to happen automatically.</span></span> <span data-ttu-id="4301e-144">當您發行時，組建組態就會發出縮短，transpiled 建置您的用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="4301e-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="4301e-145">不同於開發組建中，在正式組建並不需要在伺服器上安裝 Node.js。</span><span class="sxs-lookup"><span data-stu-id="4301e-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="4301e-146">您可以使用標準[ASP.NET Core 裝載和部署方法](xref:host-and-deploy/index)。</span><span class="sxs-lookup"><span data-stu-id="4301e-146">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="4301e-147">獨立執行 CRA 伺服器</span><span class="sxs-lookup"><span data-stu-id="4301e-147">Run the CRA server independently</span></span>

<span data-ttu-id="4301e-148">專案已設定為在背景中啟動 CRA 程式開發伺服器執行個體各自獨立，ASP.NET Core 應用程式開發模式中啟動。</span><span class="sxs-lookup"><span data-stu-id="4301e-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="4301e-149">這是很方便，因為這表示您不必手動執行不同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="4301e-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="4301e-150">沒有這個預設安裝的缺點。</span><span class="sxs-lookup"><span data-stu-id="4301e-150">There's a drawback to this default setup.</span></span> <span data-ttu-id="4301e-151">每當您修改 C# 程式碼，您必須重新啟動，應用程式的 ASP.NET 核心 CRA 伺服器重新啟動。</span><span class="sxs-lookup"><span data-stu-id="4301e-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="4301e-152">幾秒鐘的時間才能重新啟動。</span><span class="sxs-lookup"><span data-stu-id="4301e-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="4301e-153">如果您正在經常 C# 程式碼編輯，而且不想等候 CRA 伺服器重新啟動，CRA 伺服器外部外獨立執行的 ASP.NET 核心程序。</span><span class="sxs-lookup"><span data-stu-id="4301e-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="4301e-154">若要這樣做：</span><span class="sxs-lookup"><span data-stu-id="4301e-154">To do so:</span></span>

1. <span data-ttu-id="4301e-155">在命令提示字元切換至*ClientApp*子目錄，並啟動 CRA 程式開發伺服器：</span><span class="sxs-lookup"><span data-stu-id="4301e-155">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

2. <span data-ttu-id="4301e-156">修改您的 ASP.NET Core 應用程式，而不是啟動其自己的其中一個使用外部 CRA 伺服器執行個體。</span><span class="sxs-lookup"><span data-stu-id="4301e-156">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="4301e-157">在您*啟動*類別中，取代`spa.UseReactDevelopmentServer`引動過程為下列：</span><span class="sxs-lookup"><span data-stu-id="4301e-157">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="4301e-158">當您啟動 ASP.NET Core 應用程式，它將不會啟動 CRA 伺服器。</span><span class="sxs-lookup"><span data-stu-id="4301e-158">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="4301e-159">改為使用您以手動方式啟動的執行個體。</span><span class="sxs-lookup"><span data-stu-id="4301e-159">The instance you started manually is used instead.</span></span> <span data-ttu-id="4301e-160">這可讓它啟動並重新啟動速度。</span><span class="sxs-lookup"><span data-stu-id="4301e-160">This enables it to start and restart faster.</span></span> <span data-ttu-id="4301e-161">不會再等候 React 應用程式以重建每一次。</span><span class="sxs-lookup"><span data-stu-id="4301e-161">It's no longer waiting for your React app to rebuild each time.</span></span>
