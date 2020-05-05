---
title: 使用 OpenAPI 開發 ASP.NET Core 應用程式
author: ryanbrandenburg
description: 示範如何使用 ' dotnet-openapi ' 工具來加入 OpenAPI 檔案的參考。
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: 1924fb8ee5ac1ba8dc31d2175a336c8333c81fb2
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775709"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="a8a80-103">使用 OpenAPI 工具開發 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="a8a80-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="a8a80-104">依 Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="a8a80-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="a8a80-105">[Dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi)是一種[.Net Core 通用工具](/dotnet/core/tools/global-tools)，可用於管理專案中的[openapi](https://github.com/OAI/OpenAPI-Specification)參考。</span><span class="sxs-lookup"><span data-stu-id="a8a80-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="a8a80-106">安裝</span><span class="sxs-lookup"><span data-stu-id="a8a80-106">Installation</span></span>

<span data-ttu-id="a8a80-107">若要`Microsoft.dotnet-openapi`安裝，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a8a80-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="a8a80-108">新增</span><span class="sxs-lookup"><span data-stu-id="a8a80-108">Add</span></span>

<span data-ttu-id="a8a80-109">使用此頁面上的任何命令新增 OpenAPI 參考時，會將`<OpenApiReference />`類似下列的專案新增至 *.csproj*檔案：</span><span class="sxs-lookup"><span data-stu-id="a8a80-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="a8a80-110">需要先前的參考，應用程式才能呼叫所產生的用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="a8a80-110">The preceding reference is required for the app to call the generated client code.</span></span>

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

### <a name="add-file"></a><span data-ttu-id="a8a80-111">新增檔案</span><span class="sxs-lookup"><span data-stu-id="a8a80-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="a8a80-112">選項。</span><span class="sxs-lookup"><span data-stu-id="a8a80-112">Options</span></span>

| <span data-ttu-id="a8a80-113">Short 選項</span><span class="sxs-lookup"><span data-stu-id="a8a80-113">Short option</span></span>| <span data-ttu-id="a8a80-114">Long 選項</span><span class="sxs-lookup"><span data-stu-id="a8a80-114">Long option</span></span>| <span data-ttu-id="a8a80-115">說明</span><span class="sxs-lookup"><span data-stu-id="a8a80-115">Description</span></span> | <span data-ttu-id="a8a80-116">範例</span><span class="sxs-lookup"><span data-stu-id="a8a80-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="a8a80-117">-p</span><span class="sxs-lookup"><span data-stu-id="a8a80-117">-p</span></span>|<span data-ttu-id="a8a80-118">--updateProject</span><span class="sxs-lookup"><span data-stu-id="a8a80-118">--updateProject</span></span> | <span data-ttu-id="a8a80-119">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="a8a80-119">The project to operate on.</span></span> |<span data-ttu-id="a8a80-120">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="a8a80-120">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="a8a80-121">-c</span><span class="sxs-lookup"><span data-stu-id="a8a80-121">-c</span></span>|<span data-ttu-id="a8a80-122">--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="a8a80-122">--code-generator</span></span>| <span data-ttu-id="a8a80-123">要套用至參考的程式碼產生器。</span><span class="sxs-lookup"><span data-stu-id="a8a80-123">The code generator to apply to the reference.</span></span> <span data-ttu-id="a8a80-124">選項為`NSwagCSharp`和`NSwagTypeScript`。</span><span class="sxs-lookup"><span data-stu-id="a8a80-124">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="a8a80-125">如果`--code-generator`未指定，則工具會預設`NSwagCSharp`為。</span><span class="sxs-lookup"><span data-stu-id="a8a80-125">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="a8a80-126">dotnet openapi add file .\OpenApi.json--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="a8a80-126">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="a8a80-127">-H</span><span class="sxs-lookup"><span data-stu-id="a8a80-127">-h</span></span>|<span data-ttu-id="a8a80-128">--help</span><span class="sxs-lookup"><span data-stu-id="a8a80-128">--help</span></span>|<span data-ttu-id="a8a80-129">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="a8a80-129">Show help information</span></span>|<span data-ttu-id="a8a80-130">dotnet openapi add file--help</span><span class="sxs-lookup"><span data-stu-id="a8a80-130">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="a8a80-131">引數</span><span class="sxs-lookup"><span data-stu-id="a8a80-131">Arguments</span></span>

|  <span data-ttu-id="a8a80-132">引數</span><span class="sxs-lookup"><span data-stu-id="a8a80-132">Argument</span></span>  | <span data-ttu-id="a8a80-133">描述</span><span class="sxs-lookup"><span data-stu-id="a8a80-133">Description</span></span> | <span data-ttu-id="a8a80-134">範例</span><span class="sxs-lookup"><span data-stu-id="a8a80-134">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="a8a80-135">來源檔案</span><span class="sxs-lookup"><span data-stu-id="a8a80-135">source-file</span></span> | <span data-ttu-id="a8a80-136">要從中建立參考的來源。</span><span class="sxs-lookup"><span data-stu-id="a8a80-136">The source to create a reference from.</span></span> <span data-ttu-id="a8a80-137">必須是 OpenAPI 檔案。</span><span class="sxs-lookup"><span data-stu-id="a8a80-137">Must be an OpenAPI file.</span></span> |<span data-ttu-id="a8a80-138">dotnet openapi add file *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="a8a80-138">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="a8a80-139">新增 URL</span><span class="sxs-lookup"><span data-stu-id="a8a80-139">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="a8a80-140">選項。</span><span class="sxs-lookup"><span data-stu-id="a8a80-140">Options</span></span>

| <span data-ttu-id="a8a80-141">Short 選項</span><span class="sxs-lookup"><span data-stu-id="a8a80-141">Short option</span></span>| <span data-ttu-id="a8a80-142">Long 選項</span><span class="sxs-lookup"><span data-stu-id="a8a80-142">Long option</span></span>| <span data-ttu-id="a8a80-143">說明</span><span class="sxs-lookup"><span data-stu-id="a8a80-143">Description</span></span> | <span data-ttu-id="a8a80-144">範例</span><span class="sxs-lookup"><span data-stu-id="a8a80-144">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="a8a80-145">-p</span><span class="sxs-lookup"><span data-stu-id="a8a80-145">-p</span></span>|<span data-ttu-id="a8a80-146">--updateProject</span><span class="sxs-lookup"><span data-stu-id="a8a80-146">--updateProject</span></span> | <span data-ttu-id="a8a80-147">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="a8a80-147">The project to operate on.</span></span> |<span data-ttu-id="a8a80-148">dotnet openapi add url *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="a8a80-148">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="a8a80-149">-o</span><span class="sxs-lookup"><span data-stu-id="a8a80-149">-o</span></span>|<span data-ttu-id="a8a80-150">--output-file</span><span class="sxs-lookup"><span data-stu-id="a8a80-150">--output-file</span></span> | <span data-ttu-id="a8a80-151">放置 OpenAPI 檔案本機複本的位置。</span><span class="sxs-lookup"><span data-stu-id="a8a80-151">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="a8a80-152">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient. json*</span><span class="sxs-lookup"><span data-stu-id="a8a80-152">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="a8a80-153">-c</span><span class="sxs-lookup"><span data-stu-id="a8a80-153">-c</span></span>|<span data-ttu-id="a8a80-154">--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="a8a80-154">--code-generator</span></span>| <span data-ttu-id="a8a80-155">要套用至參考的程式碼產生器。</span><span class="sxs-lookup"><span data-stu-id="a8a80-155">The code generator to apply to the reference.</span></span> <span data-ttu-id="a8a80-156">選項為`NSwagCSharp`和`NSwagTypeScript`。</span><span class="sxs-lookup"><span data-stu-id="a8a80-156">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="a8a80-157">dotnet openapi add file .\OpenApi.json--程式碼產生器</span><span class="sxs-lookup"><span data-stu-id="a8a80-157">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="a8a80-158">-H</span><span class="sxs-lookup"><span data-stu-id="a8a80-158">-h</span></span>|<span data-ttu-id="a8a80-159">--help</span><span class="sxs-lookup"><span data-stu-id="a8a80-159">--help</span></span>|<span data-ttu-id="a8a80-160">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="a8a80-160">Show help information</span></span>|<span data-ttu-id="a8a80-161">dotnet openapi add url--help</span><span class="sxs-lookup"><span data-stu-id="a8a80-161">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="a8a80-162">引數</span><span class="sxs-lookup"><span data-stu-id="a8a80-162">Arguments</span></span>

|  <span data-ttu-id="a8a80-163">引數</span><span class="sxs-lookup"><span data-stu-id="a8a80-163">Argument</span></span>  | <span data-ttu-id="a8a80-164">描述</span><span class="sxs-lookup"><span data-stu-id="a8a80-164">Description</span></span> | <span data-ttu-id="a8a80-165">範例</span><span class="sxs-lookup"><span data-stu-id="a8a80-165">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="a8a80-166">來源-URL</span><span class="sxs-lookup"><span data-stu-id="a8a80-166">source-URL</span></span> | <span data-ttu-id="a8a80-167">要從中建立參考的來源。</span><span class="sxs-lookup"><span data-stu-id="a8a80-167">The source to create a reference from.</span></span> <span data-ttu-id="a8a80-168">必須是 URL。</span><span class="sxs-lookup"><span data-stu-id="a8a80-168">Must be a URL.</span></span> |<span data-ttu-id="a8a80-169">dotnet openapi 新增 url`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="a8a80-169">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="a8a80-170">移除</span><span class="sxs-lookup"><span data-stu-id="a8a80-170">Remove</span></span>

<span data-ttu-id="a8a80-171">從 *.csproj*檔案中移除符合指定檔案名的 OpenAPI 參考。</span><span class="sxs-lookup"><span data-stu-id="a8a80-171">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="a8a80-172">移除 OpenAPI 參考時，將不會產生用戶端。</span><span class="sxs-lookup"><span data-stu-id="a8a80-172">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="a8a80-173">已 *.json*刪除*yaml*檔案。</span><span class="sxs-lookup"><span data-stu-id="a8a80-173">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="a8a80-174">選項。</span><span class="sxs-lookup"><span data-stu-id="a8a80-174">Options</span></span>

| <span data-ttu-id="a8a80-175">Short 選項</span><span class="sxs-lookup"><span data-stu-id="a8a80-175">Short option</span></span>| <span data-ttu-id="a8a80-176">Long 選項</span><span class="sxs-lookup"><span data-stu-id="a8a80-176">Long option</span></span>| <span data-ttu-id="a8a80-177">說明</span><span class="sxs-lookup"><span data-stu-id="a8a80-177">Description</span></span>| <span data-ttu-id="a8a80-178">範例</span><span class="sxs-lookup"><span data-stu-id="a8a80-178">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="a8a80-179">-p</span><span class="sxs-lookup"><span data-stu-id="a8a80-179">-p</span></span>|<span data-ttu-id="a8a80-180">--updateProject</span><span class="sxs-lookup"><span data-stu-id="a8a80-180">--updateProject</span></span> | <span data-ttu-id="a8a80-181">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="a8a80-181">The project to operate on.</span></span> |<span data-ttu-id="a8a80-182">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="a8a80-182">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="a8a80-183">-H</span><span class="sxs-lookup"><span data-stu-id="a8a80-183">-h</span></span>|<span data-ttu-id="a8a80-184">--help</span><span class="sxs-lookup"><span data-stu-id="a8a80-184">--help</span></span>|<span data-ttu-id="a8a80-185">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="a8a80-185">Show help information</span></span>|<span data-ttu-id="a8a80-186">dotnet openapi remove--help</span><span class="sxs-lookup"><span data-stu-id="a8a80-186">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="a8a80-187">引數</span><span class="sxs-lookup"><span data-stu-id="a8a80-187">Arguments</span></span>

|  <span data-ttu-id="a8a80-188">引數</span><span class="sxs-lookup"><span data-stu-id="a8a80-188">Argument</span></span>  | <span data-ttu-id="a8a80-189">描述</span><span class="sxs-lookup"><span data-stu-id="a8a80-189">Description</span></span>| <span data-ttu-id="a8a80-190">範例</span><span class="sxs-lookup"><span data-stu-id="a8a80-190">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="a8a80-191">來源檔案</span><span class="sxs-lookup"><span data-stu-id="a8a80-191">source-file</span></span> | <span data-ttu-id="a8a80-192">要移除其參考的來源。</span><span class="sxs-lookup"><span data-stu-id="a8a80-192">The source to remove the reference to.</span></span> |<span data-ttu-id="a8a80-193">dotnet openapi remove *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="a8a80-193">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="a8a80-194">重新整理</span><span class="sxs-lookup"><span data-stu-id="a8a80-194">Refresh</span></span>

<span data-ttu-id="a8a80-195">重新整理使用下載 URL 的最新內容下載之檔案的本機版本。</span><span class="sxs-lookup"><span data-stu-id="a8a80-195">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="a8a80-196">選項。</span><span class="sxs-lookup"><span data-stu-id="a8a80-196">Options</span></span>

| <span data-ttu-id="a8a80-197">Short 選項</span><span class="sxs-lookup"><span data-stu-id="a8a80-197">Short option</span></span>| <span data-ttu-id="a8a80-198">Long 選項</span><span class="sxs-lookup"><span data-stu-id="a8a80-198">Long option</span></span>| <span data-ttu-id="a8a80-199">說明</span><span class="sxs-lookup"><span data-stu-id="a8a80-199">Description</span></span> | <span data-ttu-id="a8a80-200">範例</span><span class="sxs-lookup"><span data-stu-id="a8a80-200">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="a8a80-201">-p</span><span class="sxs-lookup"><span data-stu-id="a8a80-201">-p</span></span>|<span data-ttu-id="a8a80-202">--updateProject</span><span class="sxs-lookup"><span data-stu-id="a8a80-202">--updateProject</span></span> | <span data-ttu-id="a8a80-203">要操作的專案。</span><span class="sxs-lookup"><span data-stu-id="a8a80-203">The project to operate on.</span></span> | <span data-ttu-id="a8a80-204">dotnet openapi refresh *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="a8a80-204">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="a8a80-205">-H</span><span class="sxs-lookup"><span data-stu-id="a8a80-205">-h</span></span>|<span data-ttu-id="a8a80-206">--help</span><span class="sxs-lookup"><span data-stu-id="a8a80-206">--help</span></span>|<span data-ttu-id="a8a80-207">顯示說明資訊</span><span class="sxs-lookup"><span data-stu-id="a8a80-207">Show help information</span></span>|<span data-ttu-id="a8a80-208">dotnet openapi refresh--help</span><span class="sxs-lookup"><span data-stu-id="a8a80-208">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="a8a80-209">引數</span><span class="sxs-lookup"><span data-stu-id="a8a80-209">Arguments</span></span>

|  <span data-ttu-id="a8a80-210">引數</span><span class="sxs-lookup"><span data-stu-id="a8a80-210">Argument</span></span>  | <span data-ttu-id="a8a80-211">描述</span><span class="sxs-lookup"><span data-stu-id="a8a80-211">Description</span></span> | <span data-ttu-id="a8a80-212">範例</span><span class="sxs-lookup"><span data-stu-id="a8a80-212">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="a8a80-213">來源-URL</span><span class="sxs-lookup"><span data-stu-id="a8a80-213">source-URL</span></span> | <span data-ttu-id="a8a80-214">重新整理參考的來源 URL。</span><span class="sxs-lookup"><span data-stu-id="a8a80-214">The URL to refresh the reference from.</span></span> | <span data-ttu-id="a8a80-215">dotnet openapi refresh`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="a8a80-215">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
