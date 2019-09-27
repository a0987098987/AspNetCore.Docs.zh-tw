---
title: 使用 OpenAPI 開發 ASP.NET Core 應用程式
author: ryanbrandenburg
description: 示範如何使用 ' dotnet-openapi ' 工具來加入 OpenAPI 檔案的參考。
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: f5eae9e871bc8efc30d500769adb845ff244a90c
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317779"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="f3133-103">使用 OpenAPI 工具開發 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="f3133-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="f3133-104">依 Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="f3133-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="f3133-105">[Dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi)是一種[.Net Core 通用工具](/dotnet/core/tools/global-tools)，可用於管理專案中的[openapi](https://github.com/OAI/OpenAPI-Specification)參考。</span><span class="sxs-lookup"><span data-stu-id="f3133-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="f3133-106">安裝</span><span class="sxs-lookup"><span data-stu-id="f3133-106">Installation</span></span>

<span data-ttu-id="f3133-107">若要`Microsoft.dotnet-openapi`安裝，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f3133-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="f3133-108">新增</span><span class="sxs-lookup"><span data-stu-id="f3133-108">Add</span></span>

<span data-ttu-id="f3133-109">使用此頁面上的任何命令新增 OpenAPI 參考時，會將`<OpenApiReference />`類似下列的專案新增至 *.csproj*檔案：</span><span class="sxs-lookup"><span data-stu-id="f3133-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="f3133-110">需要先前的參考，應用程式才能呼叫所產生的用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="f3133-110">The preceding reference is required for the app to call the generated client code.</span></span>

<!-- TODO: Restore after https://github.com/aspnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -v|--verbose | Show verbose output. |dotnet openapi add project *-v* ../Ref/ProjRef.csproj |
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a><span data-ttu-id="f3133-111">新增檔案</span><span class="sxs-lookup"><span data-stu-id="f3133-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="f3133-112">選項。</span><span class="sxs-lookup"><span data-stu-id="f3133-112">Options</span></span>

| <span data-ttu-id="f3133-113">Short 選項</span><span class="sxs-lookup"><span data-stu-id="f3133-113">Short option</span></span>| <span data-ttu-id="f3133-114">Long 選項</span><span class="sxs-lookup"><span data-stu-id="f3133-114">Long option</span></span>| <span data-ttu-id="f3133-115">描述</span><span class="sxs-lookup"><span data-stu-id="f3133-115">Description</span></span> | <span data-ttu-id="f3133-116">範例</span><span class="sxs-lookup"><span data-stu-id="f3133-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="f3133-117">-v</span><span class="sxs-lookup"><span data-stu-id="f3133-117">-v</span></span>|<span data-ttu-id="f3133-118">--verbose</span><span class="sxs-lookup"><span data-stu-id="f3133-118">--verbose</span></span> | <span data-ttu-id="f3133-119">顯示詳細資訊輸出。</span><span class="sxs-lookup"><span data-stu-id="f3133-119">Show verbose output.</span></span> |<span data-ttu-id="f3133-120">dotnet openapi add file *-v* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="f3133-120">dotnet openapi add file *-v* .\OpenAPI.json</span></span> |
| <span data-ttu-id="f3133-121">-p</span><span class="sxs-lookup"><span data-stu-id="f3133-121">-p</span></span>|<span data-ttu-id="f3133-122">--updateProject</span><span class="sxs-lookup"><span data-stu-id="f3133-122">--updateProject</span></span> | <span data-ttu-id="f3133-123">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="f3133-123">The project to operate on.</span></span> |<span data-ttu-id="f3133-124">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="f3133-124">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="f3133-125">-c</span><span class="sxs-lookup"><span data-stu-id="f3133-125">-c</span></span>|<span data-ttu-id="f3133-126">--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="f3133-126">--code-generator</span></span>| <span data-ttu-id="f3133-127">要套用至參考的程式碼產生器。</span><span class="sxs-lookup"><span data-stu-id="f3133-127">The code generator to apply to the reference.</span></span> <span data-ttu-id="f3133-128">選項為`NSwagCSharp`和`NSwagTypeScript`。</span><span class="sxs-lookup"><span data-stu-id="f3133-128">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="f3133-129">如果`--code-generator`未指定，則工具會預設`NSwagCSharp`為。</span><span class="sxs-lookup"><span data-stu-id="f3133-129">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="f3133-130">dotnet openapi add file .\OpenApi.json--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="f3133-130">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="f3133-131">-h</span><span class="sxs-lookup"><span data-stu-id="f3133-131">-h</span></span>|<span data-ttu-id="f3133-132">--help</span><span class="sxs-lookup"><span data-stu-id="f3133-132">--help</span></span>|<span data-ttu-id="f3133-133">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="f3133-133">Show help information</span></span>|<span data-ttu-id="f3133-134">dotnet openapi add file--help</span><span class="sxs-lookup"><span data-stu-id="f3133-134">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="f3133-135">引數</span><span class="sxs-lookup"><span data-stu-id="f3133-135">Arguments</span></span>

|  <span data-ttu-id="f3133-136">引數</span><span class="sxs-lookup"><span data-stu-id="f3133-136">Argument</span></span>  | <span data-ttu-id="f3133-137">描述</span><span class="sxs-lookup"><span data-stu-id="f3133-137">Description</span></span> | <span data-ttu-id="f3133-138">範例</span><span class="sxs-lookup"><span data-stu-id="f3133-138">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="f3133-139">來源檔案</span><span class="sxs-lookup"><span data-stu-id="f3133-139">source-file</span></span> | <span data-ttu-id="f3133-140">要從中建立參考的來源。</span><span class="sxs-lookup"><span data-stu-id="f3133-140">The source to create a reference from.</span></span> <span data-ttu-id="f3133-141">必須是 OpenAPI 檔案。</span><span class="sxs-lookup"><span data-stu-id="f3133-141">Must be an OpenAPI file.</span></span> |<span data-ttu-id="f3133-142">dotnet openapi add file *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="f3133-142">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="f3133-143">新增 URL</span><span class="sxs-lookup"><span data-stu-id="f3133-143">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="f3133-144">選項。</span><span class="sxs-lookup"><span data-stu-id="f3133-144">Options</span></span>

| <span data-ttu-id="f3133-145">Short 選項</span><span class="sxs-lookup"><span data-stu-id="f3133-145">Short option</span></span>| <span data-ttu-id="f3133-146">Long 選項</span><span class="sxs-lookup"><span data-stu-id="f3133-146">Long option</span></span>| <span data-ttu-id="f3133-147">描述</span><span class="sxs-lookup"><span data-stu-id="f3133-147">Description</span></span> | <span data-ttu-id="f3133-148">範例</span><span class="sxs-lookup"><span data-stu-id="f3133-148">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="f3133-149">-v</span><span class="sxs-lookup"><span data-stu-id="f3133-149">-v</span></span>|<span data-ttu-id="f3133-150">--verbose</span><span class="sxs-lookup"><span data-stu-id="f3133-150">--verbose</span></span> | <span data-ttu-id="f3133-151">顯示詳細資訊輸出。</span><span class="sxs-lookup"><span data-stu-id="f3133-151">Show verbose output.</span></span> |<span data-ttu-id="f3133-152">dotnet openapi add url *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="f3133-152">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="f3133-153">-p</span><span class="sxs-lookup"><span data-stu-id="f3133-153">-p</span></span>|<span data-ttu-id="f3133-154">--updateProject</span><span class="sxs-lookup"><span data-stu-id="f3133-154">--updateProject</span></span> | <span data-ttu-id="f3133-155">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="f3133-155">The project to operate on.</span></span> |<span data-ttu-id="f3133-156">dotnet openapi add url *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="f3133-156">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="f3133-157">-o</span><span class="sxs-lookup"><span data-stu-id="f3133-157">-o</span></span>|<span data-ttu-id="f3133-158">--output-file</span><span class="sxs-lookup"><span data-stu-id="f3133-158">--output-file</span></span> | <span data-ttu-id="f3133-159">放置 OpenAPI 檔案本機複本的位置。</span><span class="sxs-lookup"><span data-stu-id="f3133-159">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="f3133-160">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient. json*</span><span class="sxs-lookup"><span data-stu-id="f3133-160">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="f3133-161">-c</span><span class="sxs-lookup"><span data-stu-id="f3133-161">-c</span></span>|<span data-ttu-id="f3133-162">--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="f3133-162">--code-generator</span></span>| <span data-ttu-id="f3133-163">要套用至參考的程式碼產生器。</span><span class="sxs-lookup"><span data-stu-id="f3133-163">The code generator to apply to the reference.</span></span> <span data-ttu-id="f3133-164">選項為`NSwagCSharp`和`NSwagTypeScript`。</span><span class="sxs-lookup"><span data-stu-id="f3133-164">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="f3133-165">dotnet openapi add file .\OpenApi.json--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="f3133-165">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="f3133-166">-h</span><span class="sxs-lookup"><span data-stu-id="f3133-166">-h</span></span>|<span data-ttu-id="f3133-167">--help</span><span class="sxs-lookup"><span data-stu-id="f3133-167">--help</span></span>|<span data-ttu-id="f3133-168">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="f3133-168">Show help information</span></span>|<span data-ttu-id="f3133-169">dotnet openapi add url--help</span><span class="sxs-lookup"><span data-stu-id="f3133-169">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="f3133-170">引數</span><span class="sxs-lookup"><span data-stu-id="f3133-170">Arguments</span></span>

|  <span data-ttu-id="f3133-171">引數</span><span class="sxs-lookup"><span data-stu-id="f3133-171">Argument</span></span>  | <span data-ttu-id="f3133-172">描述</span><span class="sxs-lookup"><span data-stu-id="f3133-172">Description</span></span> | <span data-ttu-id="f3133-173">範例</span><span class="sxs-lookup"><span data-stu-id="f3133-173">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="f3133-174">來源-URL</span><span class="sxs-lookup"><span data-stu-id="f3133-174">source-URL</span></span> | <span data-ttu-id="f3133-175">要從中建立參考的來源。</span><span class="sxs-lookup"><span data-stu-id="f3133-175">The source to create a reference from.</span></span> <span data-ttu-id="f3133-176">必須是 URL。</span><span class="sxs-lookup"><span data-stu-id="f3133-176">Must be a URL.</span></span> |<span data-ttu-id="f3133-177">dotnet openapi 新增 url`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="f3133-177">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="f3133-178">移除</span><span class="sxs-lookup"><span data-stu-id="f3133-178">Remove</span></span>

<span data-ttu-id="f3133-179">從 *.csproj*檔案中移除符合指定檔案名的 OpenAPI 參考。</span><span class="sxs-lookup"><span data-stu-id="f3133-179">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="f3133-180">移除 OpenAPI 參考時，將不會產生用戶端。</span><span class="sxs-lookup"><span data-stu-id="f3133-180">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="f3133-181">已刪除*yaml*檔案。</span><span class="sxs-lookup"><span data-stu-id="f3133-181">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="f3133-182">選項。</span><span class="sxs-lookup"><span data-stu-id="f3133-182">Options</span></span>

| <span data-ttu-id="f3133-183">Short 選項</span><span class="sxs-lookup"><span data-stu-id="f3133-183">Short option</span></span>| <span data-ttu-id="f3133-184">Long 選項</span><span class="sxs-lookup"><span data-stu-id="f3133-184">Long option</span></span>| <span data-ttu-id="f3133-185">描述</span><span class="sxs-lookup"><span data-stu-id="f3133-185">Description</span></span>| <span data-ttu-id="f3133-186">範例</span><span class="sxs-lookup"><span data-stu-id="f3133-186">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="f3133-187">-v</span><span class="sxs-lookup"><span data-stu-id="f3133-187">-v</span></span>|<span data-ttu-id="f3133-188">--verbose</span><span class="sxs-lookup"><span data-stu-id="f3133-188">--verbose</span></span> | <span data-ttu-id="f3133-189">顯示詳細資訊輸出。</span><span class="sxs-lookup"><span data-stu-id="f3133-189">Show verbose output.</span></span> |<span data-ttu-id="f3133-190">dotnet openapi remove *-v*</span><span class="sxs-lookup"><span data-stu-id="f3133-190">dotnet openapi remove *-v*</span></span>|
| <span data-ttu-id="f3133-191">-p</span><span class="sxs-lookup"><span data-stu-id="f3133-191">-p</span></span>|<span data-ttu-id="f3133-192">--updateProject</span><span class="sxs-lookup"><span data-stu-id="f3133-192">--updateProject</span></span> | <span data-ttu-id="f3133-193">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="f3133-193">The project to operate on.</span></span> |<span data-ttu-id="f3133-194">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="f3133-194">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="f3133-195">-h</span><span class="sxs-lookup"><span data-stu-id="f3133-195">-h</span></span>|<span data-ttu-id="f3133-196">--help</span><span class="sxs-lookup"><span data-stu-id="f3133-196">--help</span></span>|<span data-ttu-id="f3133-197">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="f3133-197">Show help information</span></span>|<span data-ttu-id="f3133-198">dotnet openapi remove--help</span><span class="sxs-lookup"><span data-stu-id="f3133-198">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="f3133-199">引數</span><span class="sxs-lookup"><span data-stu-id="f3133-199">Arguments</span></span>

|  <span data-ttu-id="f3133-200">引數</span><span class="sxs-lookup"><span data-stu-id="f3133-200">Argument</span></span>  | <span data-ttu-id="f3133-201">描述</span><span class="sxs-lookup"><span data-stu-id="f3133-201">Description</span></span>| <span data-ttu-id="f3133-202">範例</span><span class="sxs-lookup"><span data-stu-id="f3133-202">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="f3133-203">來源檔案</span><span class="sxs-lookup"><span data-stu-id="f3133-203">source-file</span></span> | <span data-ttu-id="f3133-204">要移除其參考的來源。</span><span class="sxs-lookup"><span data-stu-id="f3133-204">The source to remove the reference to.</span></span> |<span data-ttu-id="f3133-205">dotnet openapi remove *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="f3133-205">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="f3133-206">重新整理</span><span class="sxs-lookup"><span data-stu-id="f3133-206">Refresh</span></span>

<span data-ttu-id="f3133-207">重新整理使用下載 URL 的最新內容下載之檔案的本機版本。</span><span class="sxs-lookup"><span data-stu-id="f3133-207">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="f3133-208">選項。</span><span class="sxs-lookup"><span data-stu-id="f3133-208">Options</span></span>

| <span data-ttu-id="f3133-209">Short 選項</span><span class="sxs-lookup"><span data-stu-id="f3133-209">Short option</span></span>| <span data-ttu-id="f3133-210">Long 選項</span><span class="sxs-lookup"><span data-stu-id="f3133-210">Long option</span></span>| <span data-ttu-id="f3133-211">描述</span><span class="sxs-lookup"><span data-stu-id="f3133-211">Description</span></span> | <span data-ttu-id="f3133-212">範例</span><span class="sxs-lookup"><span data-stu-id="f3133-212">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="f3133-213">-v</span><span class="sxs-lookup"><span data-stu-id="f3133-213">-v</span></span>|<span data-ttu-id="f3133-214">--verbose</span><span class="sxs-lookup"><span data-stu-id="f3133-214">--verbose</span></span> | <span data-ttu-id="f3133-215">顯示詳細資訊輸出。</span><span class="sxs-lookup"><span data-stu-id="f3133-215">Show verbose output.</span></span> | <span data-ttu-id="f3133-216">dotnet openapi refresh *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="f3133-216">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="f3133-217">-p</span><span class="sxs-lookup"><span data-stu-id="f3133-217">-p</span></span>|<span data-ttu-id="f3133-218">--updateProject</span><span class="sxs-lookup"><span data-stu-id="f3133-218">--updateProject</span></span> | <span data-ttu-id="f3133-219">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="f3133-219">The project to operate on.</span></span> | <span data-ttu-id="f3133-220">dotnet openapi refresh *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="f3133-220">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="f3133-221">-h</span><span class="sxs-lookup"><span data-stu-id="f3133-221">-h</span></span>|<span data-ttu-id="f3133-222">--help</span><span class="sxs-lookup"><span data-stu-id="f3133-222">--help</span></span>|<span data-ttu-id="f3133-223">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="f3133-223">Show help information</span></span>|<span data-ttu-id="f3133-224">dotnet openapi refresh--help</span><span class="sxs-lookup"><span data-stu-id="f3133-224">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="f3133-225">引數</span><span class="sxs-lookup"><span data-stu-id="f3133-225">Arguments</span></span>

|  <span data-ttu-id="f3133-226">引數</span><span class="sxs-lookup"><span data-stu-id="f3133-226">Argument</span></span>  | <span data-ttu-id="f3133-227">描述</span><span class="sxs-lookup"><span data-stu-id="f3133-227">Description</span></span> | <span data-ttu-id="f3133-228">範例</span><span class="sxs-lookup"><span data-stu-id="f3133-228">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="f3133-229">來源-URL</span><span class="sxs-lookup"><span data-stu-id="f3133-229">source-URL</span></span> | <span data-ttu-id="f3133-230">重新整理參考的來源 URL。</span><span class="sxs-lookup"><span data-stu-id="f3133-230">The URL to refresh the reference from.</span></span> | <span data-ttu-id="f3133-231">dotnet openapi refresh`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="f3133-231">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
