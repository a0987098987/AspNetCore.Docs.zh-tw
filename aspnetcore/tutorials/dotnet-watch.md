---
title: "使用 dotnet watch 開發 ASP.NET Core 應用程式"
author: rick-anderson
description: "本教學課程會示範如何在 ASP.NET Core 應用程式中安裝及使用 .NET Core CLI 檔案監看員 (dotnet 監看式) 工具。"
manager: wpickett
ms.author: riande
ms.date: 10/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: cb15e28cb98ea82091cf5ddeed12df8926079e52
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="ff8e7-103">使用 dotnet watch 開發 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="ff8e7-103">Developing ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="ff8e7-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="ff8e7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="ff8e7-105">`dotnet watch` 是一種工具，會在來源檔案變更時執行 [.NET Core CLI](/dotnet/core/tools) 命令。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="ff8e7-106">例如，檔案變更會觸發編譯、測試執行或部署。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="ff8e7-107">在本教學課程中，我們會使用現有的 Web API 應用程式與兩個端點：一個傳回加總，另一個傳回產品。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-107">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="ff8e7-108">Product 方法包含一個 Bug，我們會在本教學課程中一併修正。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-108">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="ff8e7-109">下載[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample)。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="ff8e7-110">它包含兩個專案：*WebApp* (ASP.NET Core Web API) 和 *WebAppTests* (Web API 的單元測試)。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-110">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="ff8e7-111">在命令殼層中，巡覽至 *WebApp* 資料夾並執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ff8e7-111">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="ff8e7-112">主控台輸出會顯示類似如下的訊息 (指出應用程式正在執行，並等待要求)：</span><span class="sxs-lookup"><span data-stu-id="ff8e7-112">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="ff8e7-113">在網頁瀏覽器中，瀏覽至 `http://localhost:<port number>/api/math/sum?a=4&b=5`。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-113">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="ff8e7-114">您應該會看到 `9` 的結果。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-114">You should see the result of `9`.</span></span>

<span data-ttu-id="ff8e7-115">瀏覽至產品 API (`http://localhost:<port number>/api/math/product?a=4&b=5`)。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-115">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="ff8e7-116">它會傳回 `9`，而非您預期的 `20`。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-116">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="ff8e7-117">我們稍後將在本教學課程中對此進行修正。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-117">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="ff8e7-118">將 `dotnet watch` 新增至專案</span><span class="sxs-lookup"><span data-stu-id="ff8e7-118">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="ff8e7-119">將 `Microsoft.DotNet.Watcher.Tools` 套件參考新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="ff8e7-119">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="ff8e7-120">執行下列命令來安裝 `Microsoft.DotNet.Watcher.Tools` 套件：</span><span class="sxs-lookup"><span data-stu-id="ff8e7-120">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="ff8e7-121">使用 `dotnet watch` 執行 .NET Core CLI 命令</span><span class="sxs-lookup"><span data-stu-id="ff8e7-121">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="ff8e7-122">任何 [.NET Core CLI 命令](/dotnet/core/tools#cli-commands)都可以使用 `dotnet watch` 執行。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-122">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="ff8e7-123">例如: </span><span class="sxs-lookup"><span data-stu-id="ff8e7-123">For example:</span></span>

| <span data-ttu-id="ff8e7-124">命令</span><span class="sxs-lookup"><span data-stu-id="ff8e7-124">Command</span></span> | <span data-ttu-id="ff8e7-125">使用監看式的命令</span><span class="sxs-lookup"><span data-stu-id="ff8e7-125">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="ff8e7-126">dotnet run</span><span class="sxs-lookup"><span data-stu-id="ff8e7-126">dotnet run</span></span> | <span data-ttu-id="ff8e7-127">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="ff8e7-127">dotnet watch run</span></span> |
| <span data-ttu-id="ff8e7-128">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="ff8e7-128">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="ff8e7-129">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="ff8e7-129">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="ff8e7-130">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="ff8e7-130">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="ff8e7-131">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="ff8e7-131">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="ff8e7-132">dotnet test</span><span class="sxs-lookup"><span data-stu-id="ff8e7-132">dotnet test</span></span> | <span data-ttu-id="ff8e7-133">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="ff8e7-133">dotnet watch test</span></span> |

<span data-ttu-id="ff8e7-134">執行 *WebApp* 資料夾中的 `dotnet watch run`。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-134">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="ff8e7-135">主控台輸出指出 `watch` 已啟動。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-135">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="ff8e7-136">使用 `dotnet watch` 來變更資料</span><span class="sxs-lookup"><span data-stu-id="ff8e7-136">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="ff8e7-137">請確認 `dotnet watch` 正在執行。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-137">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="ff8e7-138">修正 *MathController.cs* 之 `Product` 方法的 Bug，使其傳回產品而非加總：</span><span class="sxs-lookup"><span data-stu-id="ff8e7-138">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="ff8e7-139">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-139">Save the file.</span></span> <span data-ttu-id="ff8e7-140">主控台輸出指出 `dotnet watch` 已偵測到檔案變更，並重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-140">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="ff8e7-141">驗證 `http://localhost:<port number>/api/math/product?a=4&b=5` 是否傳回正確的結果。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-141">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="ff8e7-142">使用 `dotnet watch` 來執行測試</span><span class="sxs-lookup"><span data-stu-id="ff8e7-142">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="ff8e7-143">將 *MathController.cs* 的 `Product` 方法變更回傳回加總，然後儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-143">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="ff8e7-144">在命令殼層中，瀏覽至 *WebAppTests* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-144">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="ff8e7-145">執行 `dotnet restore`。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-145">Run `dotnet restore`.</span></span>
1. <span data-ttu-id="ff8e7-146">執行 `dotnet watch test`。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-146">Run `dotnet watch test`.</span></span> <span data-ttu-id="ff8e7-147">其輸出指出測試失敗，且監看員正在等候檔案變更：</span><span class="sxs-lookup"><span data-stu-id="ff8e7-147">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="ff8e7-148">修正 `Product` 方法程式碼，使其傳回產品。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-148">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="ff8e7-149">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-149">Save the file.</span></span>

<span data-ttu-id="ff8e7-150">`dotnet watch` 會偵測檔案變更，並重新執行測試。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-150">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="ff8e7-151">主控台輸出指出測試成功。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-151">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="ff8e7-152">GitHub 中的 dotnet-watch</span><span class="sxs-lookup"><span data-stu-id="ff8e7-152">dotnet-watch in GitHub</span></span>

<span data-ttu-id="ff8e7-153">dotnet-watch 是 GitHub [DotNetTools 存放庫](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch)的一部分。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-153">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="ff8e7-154">[dotnet-watch 讀我檔案](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md)的 [MSBuild 區段](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild)說明如何從要監控的 MSBuild 專案檔設定 dotnet-watch。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-154">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="ff8e7-155">[dotnet-watch 讀我檔案](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md)包含本教學課程中未涵蓋的 dotnet-watch 相關資訊。</span><span class="sxs-lookup"><span data-stu-id="ff8e7-155">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
