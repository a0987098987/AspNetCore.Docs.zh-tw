---
title: "使用 dotnet watch 開發 ASP.NET Core 應用程式"
author: rick-anderson
description: "本教學課程會示範如何在 ASP.NET Core 應用程式中安裝及使用 .NET Core CLI 檔案監看員 (dotnet 監看式) 工具。"
keywords: "ASP.NET Core, 使用 dotnet watch"
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 1b26beaa41f4b38e0cfd2c8300cb521a3dcce47d
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="3b19b-104">使用 dotnet watch 開發 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="3b19b-104">Developing ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="3b19b-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="3b19b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="3b19b-106">`dotnet watch` 是一種工具，會在來源檔案變更時執行 [.NET Core CLI](/dotnet/core/tools) 命令。</span><span class="sxs-lookup"><span data-stu-id="3b19b-106">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="3b19b-107">例如，檔案變更會觸發編譯、測試執行或部署。</span><span class="sxs-lookup"><span data-stu-id="3b19b-107">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="3b19b-108">在本教學課程中，我們會使用現有的 Web API 應用程式與兩個端點：一個傳回加總，另一個傳回產品。</span><span class="sxs-lookup"><span data-stu-id="3b19b-108">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="3b19b-109">Product 方法包含一個 Bug，我們會在本教學課程中一併修正。</span><span class="sxs-lookup"><span data-stu-id="3b19b-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="3b19b-110">下載[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample)。</span><span class="sxs-lookup"><span data-stu-id="3b19b-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="3b19b-111">它包含兩個專案：*WebApp* (ASP.NET Core Web API) 和 *WebAppTests* (Web API 的單元測試)。</span><span class="sxs-lookup"><span data-stu-id="3b19b-111">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="3b19b-112">在命令殼層中，巡覽至 *WebApp* 資料夾並執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3b19b-112">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="3b19b-113">主控台輸出會顯示類似如下的訊息 (指出應用程式正在執行，並等待要求)：</span><span class="sxs-lookup"><span data-stu-id="3b19b-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="3b19b-114">在網頁瀏覽器中，瀏覽至 `http://localhost:<port number>/api/math/sum?a=4&b=5`。</span><span class="sxs-lookup"><span data-stu-id="3b19b-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="3b19b-115">您應該會看到 `9` 的結果。</span><span class="sxs-lookup"><span data-stu-id="3b19b-115">You should see the result of `9`.</span></span>

<span data-ttu-id="3b19b-116">瀏覽至產品 API (`http://localhost:<port number>/api/math/product?a=4&b=5`)。</span><span class="sxs-lookup"><span data-stu-id="3b19b-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="3b19b-117">它會傳回 `9`，而非您預期的 `20`。</span><span class="sxs-lookup"><span data-stu-id="3b19b-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="3b19b-118">我們稍後將在本教學課程中對此進行修正。</span><span class="sxs-lookup"><span data-stu-id="3b19b-118">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="3b19b-119">將 `dotnet watch` 新增至專案</span><span class="sxs-lookup"><span data-stu-id="3b19b-119">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="3b19b-120">將 `Microsoft.DotNet.Watcher.Tools` 套件參考新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="3b19b-120">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="3b19b-121">執行下列命令來安裝 `Microsoft.DotNet.Watcher.Tools` 套件：</span><span class="sxs-lookup"><span data-stu-id="3b19b-121">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="3b19b-122">使用 `dotnet watch` 執行 .NET Core CLI 命令</span><span class="sxs-lookup"><span data-stu-id="3b19b-122">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="3b19b-123">任何 [.NET Core CLI 命令](/dotnet/core/tools#cli-commands)都可以使用 `dotnet watch` 執行。</span><span class="sxs-lookup"><span data-stu-id="3b19b-123">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="3b19b-124">例如: </span><span class="sxs-lookup"><span data-stu-id="3b19b-124">For example:</span></span>

| <span data-ttu-id="3b19b-125">命令</span><span class="sxs-lookup"><span data-stu-id="3b19b-125">Command</span></span> | <span data-ttu-id="3b19b-126">使用監看式的命令</span><span class="sxs-lookup"><span data-stu-id="3b19b-126">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="3b19b-127">dotnet run</span><span class="sxs-lookup"><span data-stu-id="3b19b-127">dotnet run</span></span> | <span data-ttu-id="3b19b-128">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="3b19b-128">dotnet watch run</span></span> |
| <span data-ttu-id="3b19b-129">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="3b19b-129">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="3b19b-130">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="3b19b-130">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="3b19b-131">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="3b19b-131">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="3b19b-132">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="3b19b-132">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="3b19b-133">dotnet test</span><span class="sxs-lookup"><span data-stu-id="3b19b-133">dotnet test</span></span> | <span data-ttu-id="3b19b-134">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="3b19b-134">dotnet watch test</span></span> |

<span data-ttu-id="3b19b-135">執行 *WebApp* 資料夾中的 `dotnet watch run`。</span><span class="sxs-lookup"><span data-stu-id="3b19b-135">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="3b19b-136">主控台輸出指出 `watch` 已啟動。</span><span class="sxs-lookup"><span data-stu-id="3b19b-136">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="3b19b-137">使用 `dotnet watch` 來變更資料</span><span class="sxs-lookup"><span data-stu-id="3b19b-137">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="3b19b-138">請確認 `dotnet watch` 正在執行。</span><span class="sxs-lookup"><span data-stu-id="3b19b-138">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="3b19b-139">修正 *MathController.cs* 之 `Product` 方法的 Bug，使其傳回產品而非加總：</span><span class="sxs-lookup"><span data-stu-id="3b19b-139">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="3b19b-140">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="3b19b-140">Save the file.</span></span> <span data-ttu-id="3b19b-141">主控台輸出指出 `dotnet watch` 已偵測到檔案變更，並重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b19b-141">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="3b19b-142">驗證 `http://localhost:<port number>/api/math/product?a=4&b=5` 是否傳回正確的結果。</span><span class="sxs-lookup"><span data-stu-id="3b19b-142">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="3b19b-143">使用 `dotnet watch` 來執行測試</span><span class="sxs-lookup"><span data-stu-id="3b19b-143">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="3b19b-144">將 *MathController.cs* 的 `Product` 方法變更回傳回加總，然後儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="3b19b-144">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="3b19b-145">在命令殼層中，瀏覽至 *WebAppTests* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3b19b-145">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="3b19b-146">執行 `dotnet restore`。</span><span class="sxs-lookup"><span data-stu-id="3b19b-146">Run `dotnet restore`.</span></span>
1. <span data-ttu-id="3b19b-147">執行 `dotnet watch test`。</span><span class="sxs-lookup"><span data-stu-id="3b19b-147">Run `dotnet watch test`.</span></span> <span data-ttu-id="3b19b-148">其輸出指出測試失敗，且監看員正在等候檔案變更：</span><span class="sxs-lookup"><span data-stu-id="3b19b-148">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="3b19b-149">修正 `Product` 方法程式碼，使其傳回產品。</span><span class="sxs-lookup"><span data-stu-id="3b19b-149">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="3b19b-150">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="3b19b-150">Save the file.</span></span>

<span data-ttu-id="3b19b-151">`dotnet watch` 會偵測檔案變更，並重新執行測試。</span><span class="sxs-lookup"><span data-stu-id="3b19b-151">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="3b19b-152">主控台輸出指出測試成功。</span><span class="sxs-lookup"><span data-stu-id="3b19b-152">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="3b19b-153">GitHub 中的 dotnet-watch</span><span class="sxs-lookup"><span data-stu-id="3b19b-153">dotnet-watch in GitHub</span></span>

<span data-ttu-id="3b19b-154">dotnet-watch 是 GitHub [DotNetTools 存放庫](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch)的一部分。</span><span class="sxs-lookup"><span data-stu-id="3b19b-154">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="3b19b-155">[dotnet-watch 讀我檔案](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md)的 [MSBuild 區段](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild)說明如何從要監控的 MSBuild 專案檔設定 dotnet-watch。</span><span class="sxs-lookup"><span data-stu-id="3b19b-155">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="3b19b-156">[dotnet-watch 讀我檔案](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md)包含本教學課程中未涵蓋的 dotnet-watch 相關資訊。</span><span class="sxs-lookup"><span data-stu-id="3b19b-156">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
