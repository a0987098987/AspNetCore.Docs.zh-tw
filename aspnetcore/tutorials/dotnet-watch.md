---
title: 使用檔案監看員開發 ASP.NET Core 應用程式
author: rick-anderson
description: 本教學課程會示範如何在 ASP.NET Core 應用程式中安裝及使用 .NET Core CLI 檔案監看員 (dotnet 監看式) 工具。
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: bedb3e6a65839db915ca7bc821a267a14d34bf30
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667409"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="2b6b6-103">使用檔案監看員開發 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="2b6b6-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="2b6b6-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="2b6b6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="2b6b6-105">[點網監視](https://www.nuget.org/packages/dotnet-watch)是在源檔更改時運行[.NET Core CLI](/dotnet/core/tools)命令的工具。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-105">[dotnet watch](https://www.nuget.org/packages/dotnet-watch) is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="2b6b6-106">例如，檔案變更會觸發編譯、測試執行或部署。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="2b6b6-107">本教學課程使用現有的 Web API 與兩個端點：一個傳回加總，另一個傳回產品。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="2b6b6-108">本教學課程已修正產品方法的 Bug。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="2b6b6-109">下載[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample)。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-109">Download the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="2b6b6-110">它包含兩個專案：*WebApp* (ASP.NET Core Web API) 和 *WebAppTests* (Web API 的單元測試)。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="2b6b6-111">在命令殼層中，巡覽至 *WebApp* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="2b6b6-112">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="2b6b6-112">Run the following command:</span></span>

```dotnetcli
dotnet run
```

> [!NOTE]
> <span data-ttu-id="2b6b6-113">您可以使用 `dotnet run --project <PROJECT>` 來指定要執行的專案。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-113">You can use `dotnet run --project <PROJECT>` to specify a project to run.</span></span> <span data-ttu-id="2b6b6-114">例如，從範例應用程式的根目錄執行 `dotnet run --project WebApp` 同時也會執行 *WebApp* 專案。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-114">For example, running `dotnet run --project WebApp` from the root of the sample app will also run the *WebApp* project.</span></span>

<span data-ttu-id="2b6b6-115">主控台輸出會顯示類似如下的訊息 (指出應用程式正在執行，並等待要求)：</span><span class="sxs-lookup"><span data-stu-id="2b6b6-115">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="2b6b6-116">在網頁瀏覽器中，瀏覽至 `http://localhost:<port number>/api/math/sum?a=4&b=5`。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-116">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="2b6b6-117">您應該會看到 `9` 的結果。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-117">You should see the result of `9`.</span></span>

<span data-ttu-id="2b6b6-118">瀏覽至產品 API (`http://localhost:<port number>/api/math/product?a=4&b=5`)。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-118">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="2b6b6-119">它會傳回 `9`，而非您預期的 `20`。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-119">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="2b6b6-120">本教學課程稍後會修正該問題。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-120">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="2b6b6-121">將 `dotnet watch` 新增至專案</span><span class="sxs-lookup"><span data-stu-id="2b6b6-121">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="2b6b6-122">`dotnet watch` 檔案監看員工具隨附於 .NET Core SDK 2.1.300 版本。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-122">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="2b6b6-123">使用舊版的 .NET Core SDK 需要以下的步驟。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-123">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="2b6b6-124">將 `Microsoft.DotNet.Watcher.Tools` 套件參考新增至 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="2b6b6-124">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="2b6b6-125">執行下列命令來安裝 `Microsoft.DotNet.Watcher.Tools` 套件：</span><span class="sxs-lookup"><span data-stu-id="2b6b6-125">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```dotnetcli
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="2b6b6-126">使用 `dotnet watch` 執行 .NET Core CLI 命令</span><span class="sxs-lookup"><span data-stu-id="2b6b6-126">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="2b6b6-127">任何 [.NET Core CLI 命令](/dotnet/core/tools#cli-commands)都可以使用 `dotnet watch` 執行。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-127">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="2b6b6-128">例如：</span><span class="sxs-lookup"><span data-stu-id="2b6b6-128">For example:</span></span>

| <span data-ttu-id="2b6b6-129">Command</span><span class="sxs-lookup"><span data-stu-id="2b6b6-129">Command</span></span> | <span data-ttu-id="2b6b6-130">使用監看式的命令</span><span class="sxs-lookup"><span data-stu-id="2b6b6-130">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="2b6b6-131">dotnet run</span><span class="sxs-lookup"><span data-stu-id="2b6b6-131">dotnet run</span></span> | <span data-ttu-id="2b6b6-132">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="2b6b6-132">dotnet watch run</span></span> |
| <span data-ttu-id="2b6b6-133">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="2b6b6-133">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="2b6b6-134">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="2b6b6-134">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="2b6b6-135">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="2b6b6-135">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="2b6b6-136">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="2b6b6-136">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="2b6b6-137">dotnet test</span><span class="sxs-lookup"><span data-stu-id="2b6b6-137">dotnet test</span></span> | <span data-ttu-id="2b6b6-138">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="2b6b6-138">dotnet watch test</span></span> |

<span data-ttu-id="2b6b6-139">執行 *WebApp* 資料夾中的 `dotnet watch run`。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-139">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="2b6b6-140">主控台輸出指出 `watch` 已啟動。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-140">The console output indicates `watch` has started.</span></span>

> [!NOTE]
> <span data-ttu-id="2b6b6-141">您可以使用 `dotnet watch --project <PROJECT>` 來指定要監看的專案。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-141">You can use `dotnet watch --project <PROJECT>` to specify a project to watch.</span></span> <span data-ttu-id="2b6b6-142">例如，從範例應用程式的根目錄執行 `dotnet watch --project WebApp run` 同時也會執行並監看 *WebApp* 專案。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-142">For example, running `dotnet watch --project WebApp run` from the root of the sample app will also run and watch the *WebApp* project.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="2b6b6-143">以 `dotnet watch` 進行變更</span><span class="sxs-lookup"><span data-stu-id="2b6b6-143">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="2b6b6-144">請確認 `dotnet watch` 正在執行。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-144">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="2b6b6-145">修正 *MathController.cs* 之 `Product` 方法的 Bug，使其傳回產品而非加總：</span><span class="sxs-lookup"><span data-stu-id="2b6b6-145">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
    return a * b;
}
```

<span data-ttu-id="2b6b6-146">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-146">Save the file.</span></span> <span data-ttu-id="2b6b6-147">主控台輸出指出 `dotnet watch` 已偵測到檔案變更，並重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-147">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="2b6b6-148">驗證 `http://localhost:<port number>/api/math/product?a=4&b=5` 是否傳回正確的結果。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-148">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="2b6b6-149">使用 `dotnet watch` 執行測試</span><span class="sxs-lookup"><span data-stu-id="2b6b6-149">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="2b6b6-150">將 *MathController.cs* 的 `Product` 方法變更回傳回加總。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-150">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="2b6b6-151">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-151">Save the file.</span></span>
1. <span data-ttu-id="2b6b6-152">在命令殼層中，瀏覽至 *WebAppTests* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-152">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="2b6b6-153">執行 [dotnet restore](/dotnet/core/tools/dotnet-restore)。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-153">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="2b6b6-154">執行 `dotnet watch test`。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-154">Run `dotnet watch test`.</span></span> <span data-ttu-id="2b6b6-155">其輸出指出測試失敗，且監看員正在等候檔案變更：</span><span class="sxs-lookup"><span data-stu-id="2b6b6-155">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="2b6b6-156">修正 `Product` 方法程式碼，使其傳回產品。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-156">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="2b6b6-157">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-157">Save the file.</span></span>

<span data-ttu-id="2b6b6-158">`dotnet watch` 會偵測檔案變更，並重新執行測試。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-158">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="2b6b6-159">主控台輸出指出測試成功。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-159">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="2b6b6-160">自訂要監看的檔案清單</span><span class="sxs-lookup"><span data-stu-id="2b6b6-160">Customize files list to watch</span></span>

<span data-ttu-id="2b6b6-161">根據預設，`dotnet-watch` 會追蹤符合下列 Glob 模式的所有檔案：</span><span class="sxs-lookup"><span data-stu-id="2b6b6-161">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="2b6b6-162">編輯 *.csproj* 檔案可將更多的項目新增至監看清單。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-162">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="2b6b6-163">項目可以個別或使用 Glob 模式指定。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-163">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="2b6b6-164">選擇不使用要監看的檔案</span><span class="sxs-lookup"><span data-stu-id="2b6b6-164">Opt-out of files to be watched</span></span>

<span data-ttu-id="2b6b6-165">`dotnet-watch` 可以設定成忽略其預設設定。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-165">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="2b6b6-166">若要忽略特定的檔案，請將 `Watch="false"` 屬性新增至 *.csproj* 檔案的項目定義：</span><span class="sxs-lookup"><span data-stu-id="2b6b6-166">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

```xml
<ItemGroup>
    <!-- exclude Generated.cs from dotnet-watch -->
    <Compile Include="Generated.cs" Watch="false" />

    <!-- exclude Strings.resx from dotnet-watch -->
    <EmbeddedResource Include="Strings.resx" Watch="false" />

    <!-- exclude changes in this referenced project -->
    <ProjectReference Include="..\ClassLibrary1\ClassLibrary1.csproj" Watch="false" />
</ItemGroup>
```

## <a name="custom-watch-projects"></a><span data-ttu-id="2b6b6-167">自訂監看式專案</span><span class="sxs-lookup"><span data-stu-id="2b6b6-167">Custom watch projects</span></span>

<span data-ttu-id="2b6b6-168">`dotnet-watch` 不限制為 C# 專案。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-168">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="2b6b6-169">您可以建立自訂的監看式專案來處理不同的案例。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-169">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="2b6b6-170">請考慮下列專案配置：</span><span class="sxs-lookup"><span data-stu-id="2b6b6-170">Consider the following project layout:</span></span>

* <span data-ttu-id="2b6b6-171">**測試/**</span><span class="sxs-lookup"><span data-stu-id="2b6b6-171">**test/**</span></span>
  * <span data-ttu-id="2b6b6-172">*UnitTests/UnitTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="2b6b6-172">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="2b6b6-173">*IntegrationTests/IntegrationTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="2b6b6-173">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="2b6b6-174">如果目標是監看這兩個專案，請建立設定成監看這兩個專案的自訂專案檔：</span><span class="sxs-lookup"><span data-stu-id="2b6b6-174">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

```xml
<Project>
    <ItemGroup>
        <TestProjects Include="**\*.csproj" />
        <Watch Include="**\*.cs" />
    </ItemGroup>

    <Target Name="Test">
        <MSBuild Targets="VSTest" Projects="@(TestProjects)" />
    </Target>

    <Import Project="$(MSBuildExtensionsPath)\Microsoft.Common.targets" />
</Project>
```

<span data-ttu-id="2b6b6-175">若要開始監看兩個專案的檔案，請變更至 *test* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-175">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="2b6b6-176">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2b6b6-176">Execute the following command:</span></span>

```dotnetcli
dotnet watch msbuild /t:Test
```

<span data-ttu-id="2b6b6-177">任一測試專案中的任何檔案發生變更時，就會執行 VSTest。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-177">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="2b6b6-178">GitHub 中的 `dotnet-watch`</span><span class="sxs-lookup"><span data-stu-id="2b6b6-178">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="2b6b6-179">`dotnet-watch`是 GitHub[點網/ AspNetCore 儲存庫](https://github.com/dotnet/AspNetCore/tree/master/src/Tools/dotnet-watch)的一部分。</span><span class="sxs-lookup"><span data-stu-id="2b6b6-179">`dotnet-watch` is part of the GitHub [dotnet/AspNetCore repository](https://github.com/dotnet/AspNetCore/tree/master/src/Tools/dotnet-watch).</span></span>
