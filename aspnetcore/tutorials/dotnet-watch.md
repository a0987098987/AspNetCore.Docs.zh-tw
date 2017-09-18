---
title: "使用 dotnet watch 開發 ASP.NET Core 應用程式"
author: rick-anderson
description: "示範如何使用 dotnet watch。"
keywords: "ASP.NET Core, 使用 dotnet watch"
ms.author: riande
manager: wpickett
ms.date: 03/09/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 2ddcbfc30a839ed8dd72a632644bf73dcea777ac
ms.sourcegitcommit: b02db6da115e55140da91b67355aaf56aae1703f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/11/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="7a77a-104">使用 dotnet watch 開發 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="7a77a-104">Developing ASP.NET Core apps using dotnet watch</span></span>


<span data-ttu-id="7a77a-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="7a77a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="7a77a-106">`dotnet watch` 是一種工具，其會在來源檔案變更時執行 `dotnet` 命令。</span><span class="sxs-lookup"><span data-stu-id="7a77a-106">`dotnet watch` is a tool that runs a `dotnet` command when source files change.</span></span> <span data-ttu-id="7a77a-107">例如，檔案變更可能會觸發編譯、測試或部署。</span><span class="sxs-lookup"><span data-stu-id="7a77a-107">For example, a file change can trigger compilation, tests, or deployment.</span></span>

<span data-ttu-id="7a77a-108">在本教學課程中，我們會使用現有的 Web API 應用程式與兩個端點：一個傳回加總，另一個傳回產品。</span><span class="sxs-lookup"><span data-stu-id="7a77a-108">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="7a77a-109">Product 方法包含一個 Bug，我們會在本教學課程中一併修正。</span><span class="sxs-lookup"><span data-stu-id="7a77a-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="7a77a-110">下載[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample)。</span><span class="sxs-lookup"><span data-stu-id="7a77a-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="7a77a-111">它包含 `WebApp` (Web 應用程式) 和 `WebAppTests` (Web 應用程式的單元測試) 這兩個專案。</span><span class="sxs-lookup"><span data-stu-id="7a77a-111">It contains two projects, `WebApp` (a web app) and `WebAppTests` (unit tests for the web app).</span></span>

<span data-ttu-id="7a77a-112">在主控台中，巡覽至 WebApp 資料夾並執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="7a77a-112">In a console, navigate to the WebApp folder and run the following commands:</span></span>

- `dotnet restore`
- `dotnet run`

<span data-ttu-id="7a77a-113">主控台輸出會顯示類似如下的訊息 (指出應用程式正在執行，並等待要求)：</span><span class="sxs-lookup"><span data-stu-id="7a77a-113">The console output will show messages similar to the following (indicating that the app is running and waiting for requests):</span></span>

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="7a77a-114">在 Web 瀏覽器中，巡覽至 `http://localhost:5000/api/math/sum?a=4&b=5`，您應該會看到結果 `9`。</span><span class="sxs-lookup"><span data-stu-id="7a77a-114">In a web browser, navigate to `http://localhost:5000/api/math/sum?a=4&b=5`, you should see the result `9`.</span></span>

<span data-ttu-id="7a77a-115">巡覽至產品 API (`http://localhost:5000/api/math/product?a=4&b=5`)，其會傳回 `9`，而非您所預期的 `20`。</span><span class="sxs-lookup"><span data-stu-id="7a77a-115">Navigate to the product API (`http://localhost:5000/api/math/product?a=4&b=5`), it returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="7a77a-116">我們稍後將在本教學課程中對此進行修正。</span><span class="sxs-lookup"><span data-stu-id="7a77a-116">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="7a77a-117">將 `dotnet watch` 新增至專案</span><span class="sxs-lookup"><span data-stu-id="7a77a-117">Add `dotnet watch` to a project</span></span>

- <span data-ttu-id="7a77a-118">將 `Microsoft.DotNet.Watcher.Tools` 新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="7a77a-118">Add `Microsoft.DotNet.Watcher.Tools` to the *.csproj* file:</span></span>
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
 </ItemGroup> 
 ```

- <span data-ttu-id="7a77a-119">執行 `dotnet restore`。</span><span class="sxs-lookup"><span data-stu-id="7a77a-119">Run `dotnet restore`.</span></span>

## <a name="running-dotnet-commands-using-dotnet-watch"></a><span data-ttu-id="7a77a-120">使用 `dotnet watch` 來執行 `dotnet` 命令</span><span class="sxs-lookup"><span data-stu-id="7a77a-120">Running `dotnet` commands using `dotnet watch`</span></span>

<span data-ttu-id="7a77a-121">您可以使用 `dotnet watch` 來執行任何 `dotnet` 命令，例如：</span><span class="sxs-lookup"><span data-stu-id="7a77a-121">Any `dotnet` command can be run with `dotnet watch`, for example:</span></span>

| <span data-ttu-id="7a77a-122">命令</span><span class="sxs-lookup"><span data-stu-id="7a77a-122">Command</span></span> | <span data-ttu-id="7a77a-123">使用監看式的命令</span><span class="sxs-lookup"><span data-stu-id="7a77a-123">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="7a77a-124">dotnet run</span><span class="sxs-lookup"><span data-stu-id="7a77a-124">dotnet run</span></span> | <span data-ttu-id="7a77a-125">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="7a77a-125">dotnet watch run</span></span> |
| <span data-ttu-id="7a77a-126">dotnet run -f net451</span><span class="sxs-lookup"><span data-stu-id="7a77a-126">dotnet run -f net451</span></span> | <span data-ttu-id="7a77a-127">dotnet watch run -f net451</span><span class="sxs-lookup"><span data-stu-id="7a77a-127">dotnet watch run -f net451</span></span> |
| <span data-ttu-id="7a77a-128">dotnet run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="7a77a-128">dotnet run -f net451 -- --arg1</span></span> | <span data-ttu-id="7a77a-129">dotnet watch run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="7a77a-129">dotnet watch run -f net451 -- --arg1</span></span> |
| <span data-ttu-id="7a77a-130">dotnet test</span><span class="sxs-lookup"><span data-stu-id="7a77a-130">dotnet test</span></span> | <span data-ttu-id="7a77a-131">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="7a77a-131">dotnet watch test</span></span> |

<span data-ttu-id="7a77a-132">在 `WebApp` 資料夾中執行 `dotnet watch run`。</span><span class="sxs-lookup"><span data-stu-id="7a77a-132">Run `dotnet watch run` in the `WebApp` folder.</span></span> <span data-ttu-id="7a77a-133">主控台輸出會指出 `watch` 已啟動。</span><span class="sxs-lookup"><span data-stu-id="7a77a-133">The console output will indicate `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="7a77a-134">使用 `dotnet watch` 來變更資料</span><span class="sxs-lookup"><span data-stu-id="7a77a-134">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="7a77a-135">請確認 `dotnet watch` 正在執行。</span><span class="sxs-lookup"><span data-stu-id="7a77a-135">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="7a77a-136">修正 `MathController` 之 `Product` 方法的 Bug，使其傳回產品而非加總。</span><span class="sxs-lookup"><span data-stu-id="7a77a-136">Fix the bug in the `Product` method of the `MathController` so it returns the product and not the sum.</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="7a77a-137">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="7a77a-137">Save the file.</span></span> <span data-ttu-id="7a77a-138">主控台輸出會顯示訊息，指出 `dotnet watch` 已偵測到檔案變更，並重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="7a77a-138">The console output will show messages indicating that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="7a77a-139">驗證 `http://localhost:5000/api/math/product?a=4&b=5` 是否傳回正確的結果。</span><span class="sxs-lookup"><span data-stu-id="7a77a-139">Verify `http://localhost:5000/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="7a77a-140">使用 `dotnet watch` 來執行測試</span><span class="sxs-lookup"><span data-stu-id="7a77a-140">Running tests using `dotnet watch`</span></span>

- <span data-ttu-id="7a77a-141">將 `MathController` 的 `Product` 方法變更為傳回加總，然後儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="7a77a-141">Change the `Product` method of the `MathController` back to returning the sum and save the file.</span></span>
- <span data-ttu-id="7a77a-142">在命令視窗中，巡覽到 `WebAppTests` 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7a77a-142">In a command window, naviagate to the `WebAppTests` folder.</span></span>
- <span data-ttu-id="7a77a-143">執行 `dotnet restore`</span><span class="sxs-lookup"><span data-stu-id="7a77a-143">Run `dotnet restore`</span></span>
- <span data-ttu-id="7a77a-144">執行 `dotnet watch test`。</span><span class="sxs-lookup"><span data-stu-id="7a77a-144">Run `dotnet watch test`.</span></span> <span data-ttu-id="7a77a-145">您會看到一項輸出，指出測試失敗，且監看員正在等候檔案變更：</span><span class="sxs-lookup"><span data-stu-id="7a77a-145">You see output indicating that a test failed and that watcher is waiting for file changes:</span></span>

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- <span data-ttu-id="7a77a-146">修正 `Product` 方法程式碼，使其傳回產品。</span><span class="sxs-lookup"><span data-stu-id="7a77a-146">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="7a77a-147">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="7a77a-147">Save the file.</span></span>

<span data-ttu-id="7a77a-148">`dotnet watch` 會偵測檔案變更，並重新執行測試。</span><span class="sxs-lookup"><span data-stu-id="7a77a-148">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="7a77a-149">主控台輸出會顯示測試成功。</span><span class="sxs-lookup"><span data-stu-id="7a77a-149">The console output will show the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="7a77a-150">GitHub 中的 dotnet-watch</span><span class="sxs-lookup"><span data-stu-id="7a77a-150">dotnet-watch in GitHub</span></span>

<span data-ttu-id="7a77a-151">dotnet-watch 是 GitHub [DotNetTools 存放庫](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools)的一部分。</span><span class="sxs-lookup"><span data-stu-id="7a77a-151">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span></span>

<span data-ttu-id="7a77a-152">[dotnet-watch 讀我檔案](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md)的 [MSBuild 區段](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild)說明如何從要監控的 MSBuild 專案檔設定 dotnet-watch。</span><span class="sxs-lookup"><span data-stu-id="7a77a-152">The [MSBuild section](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="7a77a-153">[dotnet-watch 讀我檔案](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md)包含本教學課程中未涵蓋的 dotnet-watch 相關資訊。</span><span class="sxs-lookup"><span data-stu-id="7a77a-153">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
