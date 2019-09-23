---
title: 使用 OpenAPI 開發 ASP.NET Core 應用程式
author: ryanbrandenburg
description: 示範如何使用 ' dotnet-openapi ' 工具來加入 OpenAPI 檔案的參考。
ms.author: rybrande
ms.date: 08/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: a9b38bb7e69744d72867bf69cecf1fa92d7c15b3
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187457"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="b9e5a-103">使用 OpenAPI 工具開發 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="b9e5a-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="b9e5a-104">依 Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="b9e5a-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="b9e5a-105">`Microsoft.dotnet-openapi`是一種 .NET Core 通用工具，可用於管理專案中的[OpenAPI](https://github.com/OAI/OpenAPI-Specification)參考。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-105">`Microsoft.dotnet-openapi` is a .NET Core global tool for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="b9e5a-106">安裝</span><span class="sxs-lookup"><span data-stu-id="b9e5a-106">Installation</span></span>

<span data-ttu-id="b9e5a-107">若要`Microsoft.dotnet-openapi`安裝，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b9e5a-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-openapi
```

<span data-ttu-id="b9e5a-108">`Microsoft.dotnet-openapi`是[.Net Core 通用工具](/dotnet/core/tools/global-tools)。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-108">`Microsoft.dotnet-openapi` is a [.NET Core Global Tool](/dotnet/core/tools/global-tools).</span></span>

## <a name="add"></a><span data-ttu-id="b9e5a-109">新增</span><span class="sxs-lookup"><span data-stu-id="b9e5a-109">Add</span></span>

<span data-ttu-id="b9e5a-110">使用此頁面上的任何命令新增 OpenAPI 參考時，會將`<OpenApiReference />`類似下列的專案新增至 *.csproj*檔案：</span><span class="sxs-lookup"><span data-stu-id="b9e5a-110">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />`  element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="b9e5a-111">需要先前的參考，應用程式才能呼叫所產生的用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-111">The preceding reference is required for the app to call the generated client code.</span></span>

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

### <a name="add-file"></a><span data-ttu-id="b9e5a-112">新增檔案</span><span class="sxs-lookup"><span data-stu-id="b9e5a-112">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="b9e5a-113">選項</span><span class="sxs-lookup"><span data-stu-id="b9e5a-113">Options</span></span>

| <span data-ttu-id="b9e5a-114">Short 選項</span><span class="sxs-lookup"><span data-stu-id="b9e5a-114">Short option</span></span>| <span data-ttu-id="b9e5a-115">Long 選項</span><span class="sxs-lookup"><span data-stu-id="b9e5a-115">Long option</span></span>| <span data-ttu-id="b9e5a-116">描述</span><span class="sxs-lookup"><span data-stu-id="b9e5a-116">Description</span></span> | <span data-ttu-id="b9e5a-117">範例</span><span class="sxs-lookup"><span data-stu-id="b9e5a-117">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="b9e5a-118">-v</span><span class="sxs-lookup"><span data-stu-id="b9e5a-118">-v</span></span>|<span data-ttu-id="b9e5a-119">--verbose</span><span class="sxs-lookup"><span data-stu-id="b9e5a-119">--verbose</span></span> | <span data-ttu-id="b9e5a-120">顯示詳細資訊輸出。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-120">Show verbose output.</span></span> |<span data-ttu-id="b9e5a-121">dotnet openapi add file *-v* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="b9e5a-121">dotnet openapi add file *-v* .\OpenAPI.json</span></span> |
| <span data-ttu-id="b9e5a-122">-p</span><span class="sxs-lookup"><span data-stu-id="b9e5a-122">-p</span></span>|<span data-ttu-id="b9e5a-123">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b9e5a-123">--updateProject</span></span> | <span data-ttu-id="b9e5a-124">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-124">The project to operate on.</span></span> |<span data-ttu-id="b9e5a-125">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="b9e5a-125">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="b9e5a-126">-c</span><span class="sxs-lookup"><span data-stu-id="b9e5a-126">-c</span></span>|<span data-ttu-id="b9e5a-127">--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="b9e5a-127">--code-generator</span></span>| <span data-ttu-id="b9e5a-128">要套用至參考的程式碼產生器。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-128">The code generator to apply to the reference.</span></span> <span data-ttu-id="b9e5a-129">選項為`NSwagCSharp`和`NSwagTypeScript`。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-129">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="b9e5a-130">如果`--code-generator`未指定，則工具會預設`NSwagCSharp`為。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-130">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="b9e5a-131">dotnet openapi add file .\OpenApi.json--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="b9e5a-131">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="b9e5a-132">-h</span><span class="sxs-lookup"><span data-stu-id="b9e5a-132">-h</span></span>|<span data-ttu-id="b9e5a-133">--help</span><span class="sxs-lookup"><span data-stu-id="b9e5a-133">--help</span></span>|<span data-ttu-id="b9e5a-134">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="b9e5a-134">Show help information</span></span>|<span data-ttu-id="b9e5a-135">dotnet openapi add file--help</span><span class="sxs-lookup"><span data-stu-id="b9e5a-135">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="b9e5a-136">引數</span><span class="sxs-lookup"><span data-stu-id="b9e5a-136">Arguments</span></span>

|  <span data-ttu-id="b9e5a-137">引數</span><span class="sxs-lookup"><span data-stu-id="b9e5a-137">Argument</span></span>  | <span data-ttu-id="b9e5a-138">描述</span><span class="sxs-lookup"><span data-stu-id="b9e5a-138">Description</span></span> | <span data-ttu-id="b9e5a-139">範例</span><span class="sxs-lookup"><span data-stu-id="b9e5a-139">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="b9e5a-140">來源檔案</span><span class="sxs-lookup"><span data-stu-id="b9e5a-140">source-file</span></span> | <span data-ttu-id="b9e5a-141">要從中建立參考的來源。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-141">The source to create a reference from.</span></span> <span data-ttu-id="b9e5a-142">必須是 OpenAPI 檔案。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-142">Must be an OpenAPI file.</span></span> |<span data-ttu-id="b9e5a-143">dotnet openapi add file *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="b9e5a-143">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="b9e5a-144">新增 URL</span><span class="sxs-lookup"><span data-stu-id="b9e5a-144">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="b9e5a-145">選項</span><span class="sxs-lookup"><span data-stu-id="b9e5a-145">Options</span></span>

| <span data-ttu-id="b9e5a-146">Short 選項</span><span class="sxs-lookup"><span data-stu-id="b9e5a-146">Short option</span></span>| <span data-ttu-id="b9e5a-147">Long 選項</span><span class="sxs-lookup"><span data-stu-id="b9e5a-147">Long option</span></span>| <span data-ttu-id="b9e5a-148">描述</span><span class="sxs-lookup"><span data-stu-id="b9e5a-148">Description</span></span> | <span data-ttu-id="b9e5a-149">範例</span><span class="sxs-lookup"><span data-stu-id="b9e5a-149">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="b9e5a-150">-v</span><span class="sxs-lookup"><span data-stu-id="b9e5a-150">-v</span></span>|<span data-ttu-id="b9e5a-151">--verbose</span><span class="sxs-lookup"><span data-stu-id="b9e5a-151">--verbose</span></span> | <span data-ttu-id="b9e5a-152">顯示詳細資訊輸出。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-152">Show verbose output.</span></span> |<span data-ttu-id="b9e5a-153">dotnet openapi add url *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b9e5a-153">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="b9e5a-154">-p</span><span class="sxs-lookup"><span data-stu-id="b9e5a-154">-p</span></span>|<span data-ttu-id="b9e5a-155">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b9e5a-155">--updateProject</span></span> | <span data-ttu-id="b9e5a-156">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-156">The project to operate on.</span></span> |<span data-ttu-id="b9e5a-157">dotnet openapi add url *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b9e5a-157">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="b9e5a-158">-o</span><span class="sxs-lookup"><span data-stu-id="b9e5a-158">-o</span></span>|<span data-ttu-id="b9e5a-159">--output-file</span><span class="sxs-lookup"><span data-stu-id="b9e5a-159">--output-file</span></span> | <span data-ttu-id="b9e5a-160">放置 OpenAPI 檔案本機複本的位置。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-160">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="b9e5a-161">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient. json*</span><span class="sxs-lookup"><span data-stu-id="b9e5a-161">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="b9e5a-162">-c</span><span class="sxs-lookup"><span data-stu-id="b9e5a-162">-c</span></span>|<span data-ttu-id="b9e5a-163">--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="b9e5a-163">--code-generator</span></span>| <span data-ttu-id="b9e5a-164">要套用至參考的程式碼產生器。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-164">The code generator to apply to the reference.</span></span> <span data-ttu-id="b9e5a-165">選項為`NSwagCSharp`和`NSwagTypeScript`。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-165">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="b9e5a-166">dotnet openapi add file .\OpenApi.json--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="b9e5a-166">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="b9e5a-167">-h</span><span class="sxs-lookup"><span data-stu-id="b9e5a-167">-h</span></span>|<span data-ttu-id="b9e5a-168">--help</span><span class="sxs-lookup"><span data-stu-id="b9e5a-168">--help</span></span>|<span data-ttu-id="b9e5a-169">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="b9e5a-169">Show help information</span></span>|<span data-ttu-id="b9e5a-170">dotnet openapi add url--help</span><span class="sxs-lookup"><span data-stu-id="b9e5a-170">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="b9e5a-171">引數</span><span class="sxs-lookup"><span data-stu-id="b9e5a-171">Arguments</span></span>

|  <span data-ttu-id="b9e5a-172">引數</span><span class="sxs-lookup"><span data-stu-id="b9e5a-172">Argument</span></span>  | <span data-ttu-id="b9e5a-173">描述</span><span class="sxs-lookup"><span data-stu-id="b9e5a-173">Description</span></span> | <span data-ttu-id="b9e5a-174">範例</span><span class="sxs-lookup"><span data-stu-id="b9e5a-174">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="b9e5a-175">來源-URL</span><span class="sxs-lookup"><span data-stu-id="b9e5a-175">source-URL</span></span> | <span data-ttu-id="b9e5a-176">要從中建立參考的來源。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-176">The source to create a reference from.</span></span> <span data-ttu-id="b9e5a-177">必須是 URL。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-177">Must be a URL.</span></span> |<span data-ttu-id="b9e5a-178">dotnet openapi 新增 url`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b9e5a-178">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="b9e5a-179">移除</span><span class="sxs-lookup"><span data-stu-id="b9e5a-179">Remove</span></span>

<span data-ttu-id="b9e5a-180">從 *.csproj*檔案中移除符合指定檔案名的 OpenAPI 參考。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-180">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="b9e5a-181">移除 OpenAPI 參考時，將不會產生用戶端。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-181">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="b9e5a-182">已刪除*yaml*檔案。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-182">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="b9e5a-183">選項</span><span class="sxs-lookup"><span data-stu-id="b9e5a-183">Options</span></span>

| <span data-ttu-id="b9e5a-184">Short 選項</span><span class="sxs-lookup"><span data-stu-id="b9e5a-184">Short option</span></span>| <span data-ttu-id="b9e5a-185">Long 選項</span><span class="sxs-lookup"><span data-stu-id="b9e5a-185">Long option</span></span>| <span data-ttu-id="b9e5a-186">描述</span><span class="sxs-lookup"><span data-stu-id="b9e5a-186">Description</span></span>| <span data-ttu-id="b9e5a-187">範例</span><span class="sxs-lookup"><span data-stu-id="b9e5a-187">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="b9e5a-188">-v</span><span class="sxs-lookup"><span data-stu-id="b9e5a-188">-v</span></span>|<span data-ttu-id="b9e5a-189">--verbose</span><span class="sxs-lookup"><span data-stu-id="b9e5a-189">--verbose</span></span> | <span data-ttu-id="b9e5a-190">顯示詳細資訊輸出。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-190">Show verbose output.</span></span> |<span data-ttu-id="b9e5a-191">dotnet openapi remove *-v*</span><span class="sxs-lookup"><span data-stu-id="b9e5a-191">dotnet openapi remove *-v*</span></span>|
| <span data-ttu-id="b9e5a-192">-p</span><span class="sxs-lookup"><span data-stu-id="b9e5a-192">-p</span></span>|<span data-ttu-id="b9e5a-193">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b9e5a-193">--updateProject</span></span> | <span data-ttu-id="b9e5a-194">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-194">The project to operate on.</span></span> |<span data-ttu-id="b9e5a-195">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="b9e5a-195">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="b9e5a-196">-h</span><span class="sxs-lookup"><span data-stu-id="b9e5a-196">-h</span></span>|<span data-ttu-id="b9e5a-197">--help</span><span class="sxs-lookup"><span data-stu-id="b9e5a-197">--help</span></span>|<span data-ttu-id="b9e5a-198">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="b9e5a-198">Show help information</span></span>|<span data-ttu-id="b9e5a-199">dotnet openapi remove--help</span><span class="sxs-lookup"><span data-stu-id="b9e5a-199">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="b9e5a-200">引數</span><span class="sxs-lookup"><span data-stu-id="b9e5a-200">Arguments</span></span>

|  <span data-ttu-id="b9e5a-201">引數</span><span class="sxs-lookup"><span data-stu-id="b9e5a-201">Argument</span></span>  | <span data-ttu-id="b9e5a-202">描述</span><span class="sxs-lookup"><span data-stu-id="b9e5a-202">Description</span></span>| <span data-ttu-id="b9e5a-203">範例</span><span class="sxs-lookup"><span data-stu-id="b9e5a-203">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="b9e5a-204">來源檔案</span><span class="sxs-lookup"><span data-stu-id="b9e5a-204">source-file</span></span> | <span data-ttu-id="b9e5a-205">要移除其參考的來源。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-205">The source to remove the reference to.</span></span> |<span data-ttu-id="b9e5a-206">dotnet openapi remove *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="b9e5a-206">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="b9e5a-207">重新整理</span><span class="sxs-lookup"><span data-stu-id="b9e5a-207">Refresh</span></span>

<span data-ttu-id="b9e5a-208">重新整理使用下載 URL 的最新內容下載之檔案的本機版本。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-208">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="b9e5a-209">選項</span><span class="sxs-lookup"><span data-stu-id="b9e5a-209">Options</span></span>

| <span data-ttu-id="b9e5a-210">Short 選項</span><span class="sxs-lookup"><span data-stu-id="b9e5a-210">Short option</span></span>| <span data-ttu-id="b9e5a-211">Long 選項</span><span class="sxs-lookup"><span data-stu-id="b9e5a-211">Long option</span></span>| <span data-ttu-id="b9e5a-212">描述</span><span class="sxs-lookup"><span data-stu-id="b9e5a-212">Description</span></span> | <span data-ttu-id="b9e5a-213">範例</span><span class="sxs-lookup"><span data-stu-id="b9e5a-213">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="b9e5a-214">-v</span><span class="sxs-lookup"><span data-stu-id="b9e5a-214">-v</span></span>|<span data-ttu-id="b9e5a-215">--verbose</span><span class="sxs-lookup"><span data-stu-id="b9e5a-215">--verbose</span></span> | <span data-ttu-id="b9e5a-216">顯示詳細資訊輸出。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-216">Show verbose output.</span></span> | <span data-ttu-id="b9e5a-217">dotnet openapi refresh *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b9e5a-217">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="b9e5a-218">-p</span><span class="sxs-lookup"><span data-stu-id="b9e5a-218">-p</span></span>|<span data-ttu-id="b9e5a-219">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b9e5a-219">--updateProject</span></span> | <span data-ttu-id="b9e5a-220">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-220">The project to operate on.</span></span> | <span data-ttu-id="b9e5a-221">dotnet openapi refresh *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b9e5a-221">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="b9e5a-222">-h</span><span class="sxs-lookup"><span data-stu-id="b9e5a-222">-h</span></span>|<span data-ttu-id="b9e5a-223">--help</span><span class="sxs-lookup"><span data-stu-id="b9e5a-223">--help</span></span>|<span data-ttu-id="b9e5a-224">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="b9e5a-224">Show help information</span></span>|<span data-ttu-id="b9e5a-225">dotnet openapi refresh--help</span><span class="sxs-lookup"><span data-stu-id="b9e5a-225">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="b9e5a-226">引數</span><span class="sxs-lookup"><span data-stu-id="b9e5a-226">Arguments</span></span>

|  <span data-ttu-id="b9e5a-227">引數</span><span class="sxs-lookup"><span data-stu-id="b9e5a-227">Argument</span></span>  | <span data-ttu-id="b9e5a-228">描述</span><span class="sxs-lookup"><span data-stu-id="b9e5a-228">Description</span></span> | <span data-ttu-id="b9e5a-229">範例</span><span class="sxs-lookup"><span data-stu-id="b9e5a-229">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="b9e5a-230">來源-URL</span><span class="sxs-lookup"><span data-stu-id="b9e5a-230">source-URL</span></span> | <span data-ttu-id="b9e5a-231">重新整理參考的來源 URL。</span><span class="sxs-lookup"><span data-stu-id="b9e5a-231">The URL to refresh the reference from.</span></span> | <span data-ttu-id="b9e5a-232">dotnet openapi refresh`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b9e5a-232">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
