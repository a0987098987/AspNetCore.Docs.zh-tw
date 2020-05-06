---
title: 使用 Web API 分析器
author: pranavkm
description: 瞭解 ASP.NET Core MVC Web API 分析器套件。
monikerRange: '>= aspnetcore-2.2'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/05/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: web-api/advanced/analyzers
ms.openlocfilehash: 530ce2d2a7f67f549f6d188a0c571a5d58518377
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776242"
---
# <a name="use-web-api-analyzers"></a><span data-ttu-id="1032e-103">使用 Web API 分析器</span><span class="sxs-lookup"><span data-stu-id="1032e-103">Use web API analyzers</span></span>

<span data-ttu-id="1032e-104">ASP.NET Core 2.2 和更新版本提供 MVC 分析器套件，適用于 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="1032e-104">ASP.NET Core 2.2 and later provides an MVC analyzers package intended for use with web API projects.</span></span> <span data-ttu-id="1032e-105">分析器會處理以標注的<xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>控制器，同時根據[Web API 慣例](xref:web-api/advanced/conventions)來建立。</span><span class="sxs-lookup"><span data-stu-id="1032e-105">The analyzers work with controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, while building on [web API conventions](xref:web-api/advanced/conventions).</span></span>

<span data-ttu-id="1032e-106">分析器套件會通知您有任何控制器動作：</span><span class="sxs-lookup"><span data-stu-id="1032e-106">The analyzers package notifies you of any controller action that:</span></span>

* <span data-ttu-id="1032e-107">傳回未宣告的狀態碼。</span><span class="sxs-lookup"><span data-stu-id="1032e-107">Returns an undeclared status code.</span></span>
* <span data-ttu-id="1032e-108">傳回未宣告的成功結果。</span><span class="sxs-lookup"><span data-stu-id="1032e-108">Returns an undeclared success result.</span></span>
* <span data-ttu-id="1032e-109">記錄不會傳回的狀態碼。</span><span class="sxs-lookup"><span data-stu-id="1032e-109">Documents a status code that isn't returned.</span></span>
* <span data-ttu-id="1032e-110">包含明確的模型驗證檢查。</span><span class="sxs-lookup"><span data-stu-id="1032e-110">Includes an explicit model validation check.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="reference-the-analyzer-package"></a><span data-ttu-id="1032e-111">參考分析器套件</span><span class="sxs-lookup"><span data-stu-id="1032e-111">Reference the analyzer package</span></span>

<span data-ttu-id="1032e-112">在 ASP.NET Core 3.0 或更新版本中，分析器會包含在 .NET Core SDK 中。</span><span class="sxs-lookup"><span data-stu-id="1032e-112">In ASP.NET Core 3.0 or later, the analyzers are included in the .NET Core SDK.</span></span> <span data-ttu-id="1032e-113">若要在專案中啟用分析器，請在`IncludeOpenAPIAnalyzers`專案檔中包含屬性：</span><span class="sxs-lookup"><span data-stu-id="1032e-113">To enable the analyzer in your project, include the `IncludeOpenAPIAnalyzers` property in the project file:</span></span>

```xml
<PropertyGroup>
 <IncludeOpenAPIAnalyzers>true</IncludeOpenAPIAnalyzers>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

## <a name="package-installation"></a><span data-ttu-id="1032e-114">套件安裝</span><span class="sxs-lookup"><span data-stu-id="1032e-114">Package installation</span></span>

<span data-ttu-id="1032e-115">使用下列其中一種方法來安裝[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="1032e-115">Install the [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet package with one of the following approaches:</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="1032e-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1032e-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1032e-117">從 [套件管理員主控台]\*\*\*\* 視窗中：</span><span class="sxs-lookup"><span data-stu-id="1032e-117">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="1032e-118">移至 [**查看** > **其他 Windows** > **套件管理員主控台**]。</span><span class="sxs-lookup"><span data-stu-id="1032e-118">Go to **View** > **Other Windows** > **Package Manager Console**.</span></span>
  * <span data-ttu-id="1032e-119">巡覽至 *ApiConventions.csproj* 檔案所在的目錄。</span><span class="sxs-lookup"><span data-stu-id="1032e-119">Navigate to the directory in which the *ApiConventions.csproj* file exists.</span></span>
  * <span data-ttu-id="1032e-120">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="1032e-120">Execute the following command:</span></span>

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

### <a name="visual-studio-for-mac"></a>[<span data-ttu-id="1032e-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1032e-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1032e-122">以滑鼠右鍵按一下**Solution Pad** > **新增套件 ...**] 中的 [*套件*] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="1032e-122">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**.</span></span>
* <span data-ttu-id="1032e-123">將 [**新增套件**] 視窗的 [**來源**] 下拉式下拉設定為 [nuget.org]。</span><span class="sxs-lookup"><span data-stu-id="1032e-123">Set the **Add Packages** window's **Source** drop-down to "nuget.org".</span></span>
* <span data-ttu-id="1032e-124">在搜尋方塊中輸入 "Microsoft.AspNetCore.Mvc.Api.Analyzers"。</span><span class="sxs-lookup"><span data-stu-id="1032e-124">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
* <span data-ttu-id="1032e-125">從結果窗格中選取 "Microsoft.AspNetCore.Mvc.Api.Analyzers" 套件，然後按一下 [新增套件]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="1032e-125">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the results pane and click **Add Package**.</span></span>

### <a name="visual-studio-code"></a>[<span data-ttu-id="1032e-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1032e-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1032e-127">從 [整合式終端機]\*\*\*\* 執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="1032e-127">Run the following command from the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-cli"></a>[<span data-ttu-id="1032e-128">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1032e-128">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1032e-129">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="1032e-129">Run the following command:</span></span>

```dotnetcli
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

::: moniker-end

## <a name="analyzers-for-web-api-conventions"></a><span data-ttu-id="1032e-130">Web API 慣例的分析器</span><span class="sxs-lookup"><span data-stu-id="1032e-130">Analyzers for web API conventions</span></span>

<span data-ttu-id="1032e-131">OpenAPI 文件包含動作可能傳回的狀態碼及回應類型。</span><span class="sxs-lookup"><span data-stu-id="1032e-131">OpenAPI documents contain status codes and response types that an action may return.</span></span> <span data-ttu-id="1032e-132">在 ASP.NET Core MVC 中，如 <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> 和 <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> 等屬性會用以記載動作。</span><span class="sxs-lookup"><span data-stu-id="1032e-132">In ASP.NET Core MVC, attributes such as <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> and <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> are used to document an action.</span></span> <span data-ttu-id="1032e-133"><xref:tutorials/web-api-help-pages-using-swagger>詳細說明如何記錄您的 Web API。</span><span class="sxs-lookup"><span data-stu-id="1032e-133"><xref:tutorials/web-api-help-pages-using-swagger> goes into further detail on documenting your web API.</span></span>

<span data-ttu-id="1032e-134">套件中的其中一個分析器會檢查以 <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> 標註的控制器，並辨識未完全記載其回應的動作。</span><span class="sxs-lookup"><span data-stu-id="1032e-134">One of the analyzers in the package inspects controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> and identifies actions that don't entirely document their responses.</span></span> <span data-ttu-id="1032e-135">請考慮下列範例：</span><span class="sxs-lookup"><span data-stu-id="1032e-135">Consider the following example:</span></span>

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=10)]

<span data-ttu-id="1032e-136">上述動作記載 HTTP 200 成功傳回型別，但未記載 HTTP 404 失敗狀態碼。</span><span class="sxs-lookup"><span data-stu-id="1032e-136">The preceding action documents the HTTP 200 success return type but doesn't document the HTTP 404 failure status code.</span></span> <span data-ttu-id="1032e-137">分析器會回報缺少文件的 HTTP 404 狀態碼作為警告。</span><span class="sxs-lookup"><span data-stu-id="1032e-137">The analyzer reports the missing documentation for the HTTP 404 status code as a warning.</span></span> <span data-ttu-id="1032e-138">提供修正問題的選項。</span><span class="sxs-lookup"><span data-stu-id="1032e-138">An option to fix the problem is provided.</span></span>

![報告警告的分析器](conventions/_static/Analyzer.gif)

## <a name="additional-resources"></a><span data-ttu-id="1032e-140">其他資源</span><span class="sxs-lookup"><span data-stu-id="1032e-140">Additional resources</span></span>

* <xref:web-api/advanced/conventions>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:web-api/index>
