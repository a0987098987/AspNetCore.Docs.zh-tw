---
title: 使用 OpenAPI 開發 ASP.NET Core 應用程式
author: ryanbrandenburg
description: 示範如何使用 ' dotnet-openapi ' 工具來加入 OpenAPI 檔案的參考。
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: eb8d6a1dc70b2aabf495bdb359e243c91e94289f
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85404791"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="b3988-103">使用 OpenAPI 工具開發 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="b3988-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="b3988-104">依 Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="b3988-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="b3988-105">[Dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi)是一種[.Net Core 通用工具](/dotnet/core/tools/global-tools)，可用於管理專案中的[openapi](https://github.com/OAI/OpenAPI-Specification)參考。</span><span class="sxs-lookup"><span data-stu-id="b3988-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="b3988-106">安裝</span><span class="sxs-lookup"><span data-stu-id="b3988-106">Installation</span></span>

<span data-ttu-id="b3988-107">若要安裝 `Microsoft.dotnet-openapi` ，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b3988-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="b3988-108">新增</span><span class="sxs-lookup"><span data-stu-id="b3988-108">Add</span></span>

<span data-ttu-id="b3988-109">使用此頁面上的任何命令新增 OpenAPI 參考時，會將 `<OpenApiReference />` 類似下列的專案新增至 *.csproj*檔案：</span><span class="sxs-lookup"><span data-stu-id="b3988-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="b3988-110">需要先前的參考，應用程式才能呼叫所產生的用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="b3988-110">The preceding reference is required for the app to call the generated client code.</span></span>

<!-- TODO: Restore after https://github.com/dotnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a><span data-ttu-id="b3988-111">新增檔案</span><span class="sxs-lookup"><span data-stu-id="b3988-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="b3988-112">選項</span><span class="sxs-lookup"><span data-stu-id="b3988-112">Options</span></span>

| <span data-ttu-id="b3988-113">Short 選項</span><span class="sxs-lookup"><span data-stu-id="b3988-113">Short option</span></span>| <span data-ttu-id="b3988-114">Long 選項</span><span class="sxs-lookup"><span data-stu-id="b3988-114">Long option</span></span>| <span data-ttu-id="b3988-115">說明</span><span class="sxs-lookup"><span data-stu-id="b3988-115">Description</span></span> | <span data-ttu-id="b3988-116">範例</span><span class="sxs-lookup"><span data-stu-id="b3988-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="b3988-117">-p</span><span class="sxs-lookup"><span data-stu-id="b3988-117">-p</span></span>|<span data-ttu-id="b3988-118">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b3988-118">--updateProject</span></span> | <span data-ttu-id="b3988-119">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="b3988-119">The project to operate on.</span></span> |<span data-ttu-id="b3988-120">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="b3988-120">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="b3988-121">-c</span><span class="sxs-lookup"><span data-stu-id="b3988-121">-c</span></span>|<span data-ttu-id="b3988-122">--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="b3988-122">--code-generator</span></span>| <span data-ttu-id="b3988-123">要套用至參考的程式碼產生器。</span><span class="sxs-lookup"><span data-stu-id="b3988-123">The code generator to apply to the reference.</span></span> <span data-ttu-id="b3988-124">選項為 `NSwagCSharp` 和 `NSwagTypeScript` 。</span><span class="sxs-lookup"><span data-stu-id="b3988-124">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="b3988-125">如果 `--code-generator` 未指定，則工具會預設為 `NSwagCSharp` 。</span><span class="sxs-lookup"><span data-stu-id="b3988-125">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="b3988-126">dotnet openapi add file .\OpenApi.json--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="b3988-126">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="b3988-127">-H</span><span class="sxs-lookup"><span data-stu-id="b3988-127">-h</span></span>|<span data-ttu-id="b3988-128">--help</span><span class="sxs-lookup"><span data-stu-id="b3988-128">--help</span></span>|<span data-ttu-id="b3988-129">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="b3988-129">Show help information</span></span>|<span data-ttu-id="b3988-130">dotnet openapi add file--help</span><span class="sxs-lookup"><span data-stu-id="b3988-130">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="b3988-131">引數</span><span class="sxs-lookup"><span data-stu-id="b3988-131">Arguments</span></span>

|  <span data-ttu-id="b3988-132">引數</span><span class="sxs-lookup"><span data-stu-id="b3988-132">Argument</span></span>  | <span data-ttu-id="b3988-133">說明</span><span class="sxs-lookup"><span data-stu-id="b3988-133">Description</span></span> | <span data-ttu-id="b3988-134">範例</span><span class="sxs-lookup"><span data-stu-id="b3988-134">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="b3988-135">來源檔案</span><span class="sxs-lookup"><span data-stu-id="b3988-135">source-file</span></span> | <span data-ttu-id="b3988-136">要從中建立參考的來源。</span><span class="sxs-lookup"><span data-stu-id="b3988-136">The source to create a reference from.</span></span> <span data-ttu-id="b3988-137">必須是 OpenAPI 檔案。</span><span class="sxs-lookup"><span data-stu-id="b3988-137">Must be an OpenAPI file.</span></span> |<span data-ttu-id="b3988-138">dotnet openapi add file *.\OpenAPI.js于*</span><span class="sxs-lookup"><span data-stu-id="b3988-138">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="b3988-139">新增 URL</span><span class="sxs-lookup"><span data-stu-id="b3988-139">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="b3988-140">選項</span><span class="sxs-lookup"><span data-stu-id="b3988-140">Options</span></span>

| <span data-ttu-id="b3988-141">Short 選項</span><span class="sxs-lookup"><span data-stu-id="b3988-141">Short option</span></span>| <span data-ttu-id="b3988-142">Long 選項</span><span class="sxs-lookup"><span data-stu-id="b3988-142">Long option</span></span>| <span data-ttu-id="b3988-143">說明</span><span class="sxs-lookup"><span data-stu-id="b3988-143">Description</span></span> | <span data-ttu-id="b3988-144">範例</span><span class="sxs-lookup"><span data-stu-id="b3988-144">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="b3988-145">-p</span><span class="sxs-lookup"><span data-stu-id="b3988-145">-p</span></span>|<span data-ttu-id="b3988-146">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b3988-146">--updateProject</span></span> | <span data-ttu-id="b3988-147">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="b3988-147">The project to operate on.</span></span> |<span data-ttu-id="b3988-148">dotnet openapi add url *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b3988-148">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="b3988-149">-o</span><span class="sxs-lookup"><span data-stu-id="b3988-149">-o</span></span>|<span data-ttu-id="b3988-150">--output-file</span><span class="sxs-lookup"><span data-stu-id="b3988-150">--output-file</span></span> | <span data-ttu-id="b3988-151">放置 OpenAPI 檔案本機複本的位置。</span><span class="sxs-lookup"><span data-stu-id="b3988-151">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="b3988-152">dotnet openapi 新增 url `https://contoso.com/openapi.json` *--輸出-檔案 myclient.js于*</span><span class="sxs-lookup"><span data-stu-id="b3988-152">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="b3988-153">-c</span><span class="sxs-lookup"><span data-stu-id="b3988-153">-c</span></span>|<span data-ttu-id="b3988-154">--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="b3988-154">--code-generator</span></span>| <span data-ttu-id="b3988-155">要套用至參考的程式碼產生器。</span><span class="sxs-lookup"><span data-stu-id="b3988-155">The code generator to apply to the reference.</span></span> <span data-ttu-id="b3988-156">選項為 `NSwagCSharp` 和 `NSwagTypeScript` 。</span><span class="sxs-lookup"><span data-stu-id="b3988-156">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="b3988-157">dotnet openapi add file .\OpenApi.json--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="b3988-157">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="b3988-158">-H</span><span class="sxs-lookup"><span data-stu-id="b3988-158">-h</span></span>|<span data-ttu-id="b3988-159">--help</span><span class="sxs-lookup"><span data-stu-id="b3988-159">--help</span></span>|<span data-ttu-id="b3988-160">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="b3988-160">Show help information</span></span>|<span data-ttu-id="b3988-161">dotnet openapi add url--help</span><span class="sxs-lookup"><span data-stu-id="b3988-161">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="b3988-162">引數</span><span class="sxs-lookup"><span data-stu-id="b3988-162">Arguments</span></span>

|  <span data-ttu-id="b3988-163">引數</span><span class="sxs-lookup"><span data-stu-id="b3988-163">Argument</span></span>  | <span data-ttu-id="b3988-164">說明</span><span class="sxs-lookup"><span data-stu-id="b3988-164">Description</span></span> | <span data-ttu-id="b3988-165">範例</span><span class="sxs-lookup"><span data-stu-id="b3988-165">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="b3988-166">來源-URL</span><span class="sxs-lookup"><span data-stu-id="b3988-166">source-URL</span></span> | <span data-ttu-id="b3988-167">要從中建立參考的來源。</span><span class="sxs-lookup"><span data-stu-id="b3988-167">The source to create a reference from.</span></span> <span data-ttu-id="b3988-168">必須是 URL。</span><span class="sxs-lookup"><span data-stu-id="b3988-168">Must be a URL.</span></span> |<span data-ttu-id="b3988-169">dotnet openapi 新增 url`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b3988-169">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="b3988-170">移除</span><span class="sxs-lookup"><span data-stu-id="b3988-170">Remove</span></span>

<span data-ttu-id="b3988-171">從 *.csproj*檔案中移除符合指定檔案名的 OpenAPI 參考。</span><span class="sxs-lookup"><span data-stu-id="b3988-171">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="b3988-172">移除 OpenAPI 參考時，將不會產生用戶端。</span><span class="sxs-lookup"><span data-stu-id="b3988-172">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="b3988-173">已 *.json*刪除*yaml*檔案。</span><span class="sxs-lookup"><span data-stu-id="b3988-173">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="b3988-174">選項</span><span class="sxs-lookup"><span data-stu-id="b3988-174">Options</span></span>

| <span data-ttu-id="b3988-175">Short 選項</span><span class="sxs-lookup"><span data-stu-id="b3988-175">Short option</span></span>| <span data-ttu-id="b3988-176">Long 選項</span><span class="sxs-lookup"><span data-stu-id="b3988-176">Long option</span></span>| <span data-ttu-id="b3988-177">說明</span><span class="sxs-lookup"><span data-stu-id="b3988-177">Description</span></span>| <span data-ttu-id="b3988-178">範例</span><span class="sxs-lookup"><span data-stu-id="b3988-178">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="b3988-179">-p</span><span class="sxs-lookup"><span data-stu-id="b3988-179">-p</span></span>|<span data-ttu-id="b3988-180">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b3988-180">--updateProject</span></span> | <span data-ttu-id="b3988-181">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="b3988-181">The project to operate on.</span></span> |<span data-ttu-id="b3988-182">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.js開啟</span><span class="sxs-lookup"><span data-stu-id="b3988-182">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="b3988-183">-H</span><span class="sxs-lookup"><span data-stu-id="b3988-183">-h</span></span>|<span data-ttu-id="b3988-184">--help</span><span class="sxs-lookup"><span data-stu-id="b3988-184">--help</span></span>|<span data-ttu-id="b3988-185">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="b3988-185">Show help information</span></span>|<span data-ttu-id="b3988-186">dotnet openapi remove--help</span><span class="sxs-lookup"><span data-stu-id="b3988-186">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="b3988-187">引數</span><span class="sxs-lookup"><span data-stu-id="b3988-187">Arguments</span></span>

|  <span data-ttu-id="b3988-188">引數</span><span class="sxs-lookup"><span data-stu-id="b3988-188">Argument</span></span>  | <span data-ttu-id="b3988-189">說明</span><span class="sxs-lookup"><span data-stu-id="b3988-189">Description</span></span>| <span data-ttu-id="b3988-190">範例</span><span class="sxs-lookup"><span data-stu-id="b3988-190">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="b3988-191">來源檔案</span><span class="sxs-lookup"><span data-stu-id="b3988-191">source-file</span></span> | <span data-ttu-id="b3988-192">要移除其參考的來源。</span><span class="sxs-lookup"><span data-stu-id="b3988-192">The source to remove the reference to.</span></span> |<span data-ttu-id="b3988-193">dotnet openapi 移除 *.\OpenAPI.js*</span><span class="sxs-lookup"><span data-stu-id="b3988-193">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="b3988-194">重新整理</span><span class="sxs-lookup"><span data-stu-id="b3988-194">Refresh</span></span>

<span data-ttu-id="b3988-195">重新整理使用下載 URL 的最新內容下載之檔案的本機版本。</span><span class="sxs-lookup"><span data-stu-id="b3988-195">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="b3988-196">選項</span><span class="sxs-lookup"><span data-stu-id="b3988-196">Options</span></span>

| <span data-ttu-id="b3988-197">Short 選項</span><span class="sxs-lookup"><span data-stu-id="b3988-197">Short option</span></span>| <span data-ttu-id="b3988-198">Long 選項</span><span class="sxs-lookup"><span data-stu-id="b3988-198">Long option</span></span>| <span data-ttu-id="b3988-199">說明</span><span class="sxs-lookup"><span data-stu-id="b3988-199">Description</span></span> | <span data-ttu-id="b3988-200">範例</span><span class="sxs-lookup"><span data-stu-id="b3988-200">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="b3988-201">-p</span><span class="sxs-lookup"><span data-stu-id="b3988-201">-p</span></span>|<span data-ttu-id="b3988-202">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b3988-202">--updateProject</span></span> | <span data-ttu-id="b3988-203">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="b3988-203">The project to operate on.</span></span> | <span data-ttu-id="b3988-204">dotnet openapi refresh *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b3988-204">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="b3988-205">-H</span><span class="sxs-lookup"><span data-stu-id="b3988-205">-h</span></span>|<span data-ttu-id="b3988-206">--help</span><span class="sxs-lookup"><span data-stu-id="b3988-206">--help</span></span>|<span data-ttu-id="b3988-207">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="b3988-207">Show help information</span></span>|<span data-ttu-id="b3988-208">dotnet openapi refresh--help</span><span class="sxs-lookup"><span data-stu-id="b3988-208">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="b3988-209">引數</span><span class="sxs-lookup"><span data-stu-id="b3988-209">Arguments</span></span>

|  <span data-ttu-id="b3988-210">引數</span><span class="sxs-lookup"><span data-stu-id="b3988-210">Argument</span></span>  | <span data-ttu-id="b3988-211">說明</span><span class="sxs-lookup"><span data-stu-id="b3988-211">Description</span></span> | <span data-ttu-id="b3988-212">範例</span><span class="sxs-lookup"><span data-stu-id="b3988-212">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="b3988-213">來源-URL</span><span class="sxs-lookup"><span data-stu-id="b3988-213">source-URL</span></span> | <span data-ttu-id="b3988-214">重新整理參考的來源 URL。</span><span class="sxs-lookup"><span data-stu-id="b3988-214">The URL to refresh the reference from.</span></span> | <span data-ttu-id="b3988-215">dotnet openapi refresh`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b3988-215">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
