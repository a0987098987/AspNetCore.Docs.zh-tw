---
title: 在類別庫中使用 ASP.NET Core Api
author: scottaddie
description: 瞭解如何在類別庫中使用 ASP.NET Core Api。
ms.author: scaddie
ms.custom: mvc
ms.date: 12/16/2019
no-loc:
- Blazor
uid: fundamentals/target-aspnetcore
ms.openlocfilehash: 72096fc2f03033dfe8325b5129e074913a2fbd1f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658064"
---
# <a name="use-aspnet-core-apis-in-a-class-library"></a><span data-ttu-id="33b99-103">在類別庫中使用 ASP.NET Core Api</span><span class="sxs-lookup"><span data-stu-id="33b99-103">Use ASP.NET Core APIs in a class library</span></span>

<span data-ttu-id="33b99-104">作者：[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="33b99-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="33b99-105">本檔提供在類別庫中使用 ASP.NET Core Api 的指引。</span><span class="sxs-lookup"><span data-stu-id="33b99-105">This document provides guidance for using ASP.NET Core APIs in a class library.</span></span> <span data-ttu-id="33b99-106">如需所有其他程式庫指引，請參閱[開放原始碼程式庫指引](/dotnet/standard/library-guidance/)。</span><span class="sxs-lookup"><span data-stu-id="33b99-106">For all other library guidance, see [Open-source library guidance](/dotnet/standard/library-guidance/).</span></span>

## <a name="determine-which-aspnet-core-versions-to-support"></a><span data-ttu-id="33b99-107">判斷支援的 ASP.NET Core 版本</span><span class="sxs-lookup"><span data-stu-id="33b99-107">Determine which ASP.NET Core versions to support</span></span>

<span data-ttu-id="33b99-108">ASP.NET Core 遵守[.Net Core 支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)。</span><span class="sxs-lookup"><span data-stu-id="33b99-108">ASP.NET Core adheres to the [.NET Core support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span> <span data-ttu-id="33b99-109">判斷要在程式庫中支援的 ASP.NET Core 版本時，請參閱支援原則。</span><span class="sxs-lookup"><span data-stu-id="33b99-109">Consult the support policy when determining which ASP.NET Core versions to support in a library.</span></span> <span data-ttu-id="33b99-110">程式庫應該：</span><span class="sxs-lookup"><span data-stu-id="33b99-110">A library should:</span></span>

* <span data-ttu-id="33b99-111">請致力於支援分類為*長期支援*（LTS）的所有 ASP.NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="33b99-111">Make an effort to support all ASP.NET Core versions classified as *Long-Term Support* (LTS).</span></span>
* <span data-ttu-id="33b99-112">不是為了支援分類為*生命週期結束*（EOL）的 ASP.NET Core 版本而感到義務。</span><span class="sxs-lookup"><span data-stu-id="33b99-112">Not feel obligated to support ASP.NET Core versions classified as *End of Life* (EOL).</span></span>

<span data-ttu-id="33b99-113">當 ASP.NET Core 的預覽版本可供使用時，會在[aspnet/公告](https://github.com/aspnet/Announcements/issues)GitHub 存放庫中公佈重大變更。</span><span class="sxs-lookup"><span data-stu-id="33b99-113">As preview releases of ASP.NET Core are made available, breaking changes are posted in the [aspnet/Announcements](https://github.com/aspnet/Announcements/issues) GitHub repository.</span></span> <span data-ttu-id="33b99-114">在開發架構功能時，可以執行程式庫的相容性測試。</span><span class="sxs-lookup"><span data-stu-id="33b99-114">Compatibility testing of libraries can be conducted as framework features are being developed.</span></span>

## <a name="use-the-aspnet-core-shared-framework"></a><span data-ttu-id="33b99-115">使用 ASP.NET Core 共用架構</span><span class="sxs-lookup"><span data-stu-id="33b99-115">Use the ASP.NET Core shared framework</span></span>

<span data-ttu-id="33b99-116">隨著 .NET Core 3.0 的發行，許多 ASP.NET Core 元件不再發佈至 NuGet 作為套件。</span><span class="sxs-lookup"><span data-stu-id="33b99-116">With the release of .NET Core 3.0, many ASP.NET Core assemblies are no longer published to NuGet as packages.</span></span> <span data-ttu-id="33b99-117">相反地，元件會包含在與 .NET Core SDK 和執行時間安裝程式一起安裝的 `Microsoft.AspNetCore.App` 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="33b99-117">Instead, the assemblies are included in the `Microsoft.AspNetCore.App` shared framework, which is installed with the .NET Core SDK and runtime installers.</span></span> <span data-ttu-id="33b99-118">如需不再發佈的封裝清單，請參閱[移除過時的套件參考](xref:migration/22-to-30#remove-obsolete-package-references)。</span><span class="sxs-lookup"><span data-stu-id="33b99-118">For a list of packages no longer being published, see [Remove obsolete package references](xref:migration/22-to-30#remove-obsolete-package-references).</span></span>

<span data-ttu-id="33b99-119">從 .NET Core 3.0，使用 `Microsoft.NET.Sdk.Web` MSBuild SDK 的專案會隱含地參考共用架構。</span><span class="sxs-lookup"><span data-stu-id="33b99-119">As of .NET Core 3.0, projects using the `Microsoft.NET.Sdk.Web` MSBuild SDK implicitly reference the shared framework.</span></span> <span data-ttu-id="33b99-120">使用 `Microsoft.NET.Sdk` 或 `Microsoft.NET.Sdk.Razor` SDK 的專案必須參考 ASP.NET Core，才能在共用架構中使用 ASP.NET Core Api。</span><span class="sxs-lookup"><span data-stu-id="33b99-120">Projects using the `Microsoft.NET.Sdk` or `Microsoft.NET.Sdk.Razor` SDK must reference ASP.NET Core to use ASP.NET Core APIs in the shared framework.</span></span>

<span data-ttu-id="33b99-121">若要參考 ASP.NET Core，請將下列 `<FrameworkReference>` 元素新增至您的專案檔：</span><span class="sxs-lookup"><span data-stu-id="33b99-121">To reference ASP.NET Core, add the following `<FrameworkReference>` element to your project file:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj?highlight=8)]

<span data-ttu-id="33b99-122">只有以 .NET Core 3.x 為目標的專案才支援以這種方式參考 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="33b99-122">Referencing ASP.NET Core in this manner is only supported for projects targeting .NET Core 3.x.</span></span>

## <a name="include-blazor-extensibility"></a><span data-ttu-id="33b99-123">包含 Blazor 擴充性</span><span class="sxs-lookup"><span data-stu-id="33b99-123">Include Blazor extensibility</span></span>

<span data-ttu-id="33b99-124">Blazor 支援 WebAssembly （WASM）和伺服器[裝載模型](xref:blazor/hosting-models)。</span><span class="sxs-lookup"><span data-stu-id="33b99-124">Blazor supports WebAssembly (WASM) and Server [hosting models](xref:blazor/hosting-models).</span></span> <span data-ttu-id="33b99-125">除非有特定的理由不這麼做，否則[Razor 元件](xref:blazor/components)程式庫應該支援這兩種裝載模型。</span><span class="sxs-lookup"><span data-stu-id="33b99-125">Unless there's a specific reason not to, a [Razor components](xref:blazor/components) library should support both hosting models.</span></span> <span data-ttu-id="33b99-126">Razor 元件程式庫必須使用[NET.TCP sdk](xref:razor-pages/sdk)。</span><span class="sxs-lookup"><span data-stu-id="33b99-126">A Razor components library must use the [Microsoft.NET.Sdk.Razor SDK](xref:razor-pages/sdk).</span></span>

### <a name="support-both-hosting-models"></a><span data-ttu-id="33b99-127">同時支援這兩種裝載模型</span><span class="sxs-lookup"><span data-stu-id="33b99-127">Support both hosting models</span></span>

<span data-ttu-id="33b99-128">若要支援來自[Blazor 伺服器](xref:blazor/hosting-models#blazor-server)和[Blazor WASM](xref:blazor/hosting-models#blazor-webassembly)專案的 Razor 元件耗用量，請針對您的編輯器使用下列指示。</span><span class="sxs-lookup"><span data-stu-id="33b99-128">To support Razor component consumption from both [Blazor Server](xref:blazor/hosting-models#blazor-server) and [Blazor WASM](xref:blazor/hosting-models#blazor-webassembly) projects, use the following instructions for your editor.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="33b99-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="33b99-129">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="33b99-130">使用 [ **Razor 類別庫**] 專案範本。</span><span class="sxs-lookup"><span data-stu-id="33b99-130">Use the **Razor Class Library** project template.</span></span> <span data-ttu-id="33b99-131">應取消選取範本的 [**支援頁面和流覽**器] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="33b99-131">The template's **Support pages and views** checkbox should be deselected.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="33b99-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="33b99-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="33b99-133">在整合式終端機中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="33b99-133">Run the following command in the integrated terminal:</span></span>

```dotnetcli
dotnet new razorclasslib
```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="33b99-134">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="33b99-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="33b99-135">使用 [ **Razor 類別庫**] 專案範本。</span><span class="sxs-lookup"><span data-stu-id="33b99-135">Use the **Razor Class Library** project template.</span></span>

---

<span data-ttu-id="33b99-136">從範本產生的專案會執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="33b99-136">The project generated from the template does the following things:</span></span>

* <span data-ttu-id="33b99-137">目標 .NET Standard 2.0。</span><span class="sxs-lookup"><span data-stu-id="33b99-137">Targets .NET Standard 2.0.</span></span>
* <span data-ttu-id="33b99-138">將 `RazorLangVersion` 屬性設定為 `3.0`。</span><span class="sxs-lookup"><span data-stu-id="33b99-138">Sets the `RazorLangVersion` property to `3.0`.</span></span> <span data-ttu-id="33b99-139">`3.0` 是 .NET Core 3.x 的預設值。</span><span class="sxs-lookup"><span data-stu-id="33b99-139">`3.0` is the default value for .NET Core 3.x.</span></span>
* <span data-ttu-id="33b99-140">新增下列套件參考：</span><span class="sxs-lookup"><span data-stu-id="33b99-140">Adds the following package references:</span></span>
  * [<span data-ttu-id="33b99-141">AspNetCore 元件</span><span class="sxs-lookup"><span data-stu-id="33b99-141">Microsoft.AspNetCore.Components</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Components)
  * [<span data-ttu-id="33b99-142">AspNetCore。 Web</span><span class="sxs-lookup"><span data-stu-id="33b99-142">Microsoft.AspNetCore.Components.Web</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Web)

<span data-ttu-id="33b99-143">例如：</span><span class="sxs-lookup"><span data-stu-id="33b99-143">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-razor-components-library.csproj)]

### <a name="support-a-specific-hosting-model"></a><span data-ttu-id="33b99-144">支援特定的主控模型</span><span class="sxs-lookup"><span data-stu-id="33b99-144">Support a specific hosting model</span></span>

<span data-ttu-id="33b99-145">支援單一 Blazor 裝載模型的情況較不常見。</span><span class="sxs-lookup"><span data-stu-id="33b99-145">It's far less common to support a single Blazor hosting model.</span></span> <span data-ttu-id="33b99-146">例如，僅支援來自[Blazor 伺服器](xref:blazor/hosting-models#blazor-server)專案的 Razor 元件耗用量：</span><span class="sxs-lookup"><span data-stu-id="33b99-146">As an example, to support Razor component consumption from [Blazor Server](xref:blazor/hosting-models#blazor-server) projects only:</span></span>

* <span data-ttu-id="33b99-147">以 .NET Core 3.x 為目標。</span><span class="sxs-lookup"><span data-stu-id="33b99-147">Target .NET Core 3.x.</span></span>
* <span data-ttu-id="33b99-148">新增共用架構的 `<FrameworkReference>` 元素。</span><span class="sxs-lookup"><span data-stu-id="33b99-148">Add a `<FrameworkReference>` element for the shared framework.</span></span>

<span data-ttu-id="33b99-149">例如：</span><span class="sxs-lookup"><span data-stu-id="33b99-149">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-razor-components-library.csproj)]

<span data-ttu-id="33b99-150">如需包含 Razor 元件之程式庫的詳細資訊，請參閱[ASP.NET Core Razor 元件類別庫](xref:blazor/class-libraries)。</span><span class="sxs-lookup"><span data-stu-id="33b99-150">For more information on libraries containing Razor components, see [ASP.NET Core Razor components class libraries](xref:blazor/class-libraries).</span></span>

## <a name="include-mvc-extensibility"></a><span data-ttu-id="33b99-151">包含 MVC 擴充性</span><span class="sxs-lookup"><span data-stu-id="33b99-151">Include MVC extensibility</span></span>

<span data-ttu-id="33b99-152">本節概述程式庫的建議，包括：</span><span class="sxs-lookup"><span data-stu-id="33b99-152">This section outlines recommendations for libraries that include:</span></span>

* [<span data-ttu-id="33b99-153">Razor views 或 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="33b99-153">Razor views or Razor Pages</span></span>](#razor-views-or-razor-pages)
* [<span data-ttu-id="33b99-154">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="33b99-154">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="33b99-155">檢視元件</span><span class="sxs-lookup"><span data-stu-id="33b99-155">View components</span></span>](#view-components)

<span data-ttu-id="33b99-156">本節不會討論多個目標，以支援多個版本的 MVC。</span><span class="sxs-lookup"><span data-stu-id="33b99-156">This section doesn't discuss multi-targeting to support multiple versions of MVC.</span></span> <span data-ttu-id="33b99-157">如需支援多個 ASP.NET Core 版本的指引，請參閱[支援多個 ASP.NET Core 版本](#support-multiple-aspnet-core-versions)。</span><span class="sxs-lookup"><span data-stu-id="33b99-157">For guidance on supporting multiple ASP.NET Core versions, see [Support multiple ASP.NET Core versions](#support-multiple-aspnet-core-versions).</span></span>

### <a name="razor-views-or-razor-pages"></a><span data-ttu-id="33b99-158">Razor views 或 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="33b99-158">Razor views or Razor Pages</span></span>

<span data-ttu-id="33b99-159">包含[Razor views](xref:mvc/views/overview)或[Razor Pages](xref:razor-pages/index)的專案必須使用[Microsoft. net.tcp sdk](xref:razor-pages/sdk)。</span><span class="sxs-lookup"><span data-stu-id="33b99-159">A project that includes [Razor views](xref:mvc/views/overview) or [Razor Pages](xref:razor-pages/index) must use the [Microsoft.NET.Sdk.Razor SDK](xref:razor-pages/sdk).</span></span>

<span data-ttu-id="33b99-160">如果專案是以 .NET Core 3.x 為目標，則需要：</span><span class="sxs-lookup"><span data-stu-id="33b99-160">If the project targets .NET Core 3.x, it requires:</span></span>

* <span data-ttu-id="33b99-161">`AddRazorSupportForMvc` MSBuild 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="33b99-161">An `AddRazorSupportForMvc` MSBuild property set to `true`.</span></span>
* <span data-ttu-id="33b99-162">共用架構的 `<FrameworkReference>` 元素。</span><span class="sxs-lookup"><span data-stu-id="33b99-162">A `<FrameworkReference>` element for the shared framework.</span></span>

<span data-ttu-id="33b99-163">**Razor 類別庫**專案範本滿足上述以 .net Core 3.x 為目標的專案需求。</span><span class="sxs-lookup"><span data-stu-id="33b99-163">The **Razor Class Library** project template satisfies the preceding requirements for projects targeting .NET Core 3.x.</span></span> <span data-ttu-id="33b99-164">針對您的編輯器使用下列指示。</span><span class="sxs-lookup"><span data-stu-id="33b99-164">Use the following instructions for your editor.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="33b99-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="33b99-165">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="33b99-166">使用 [ **Razor 類別庫**] 專案範本。</span><span class="sxs-lookup"><span data-stu-id="33b99-166">Use the **Razor Class Library** project template.</span></span> <span data-ttu-id="33b99-167">應選取範本的 [**支援頁面和流覽**器] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="33b99-167">The template's **Support pages and views** checkbox should be selected.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="33b99-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="33b99-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="33b99-169">在整合式終端機中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="33b99-169">Run the following command in the integrated terminal:</span></span>

```dotnetcli
dotnet new razorclasslib -s
```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="33b99-170">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="33b99-170">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="33b99-171">目前沒有任何專案範本支援。</span><span class="sxs-lookup"><span data-stu-id="33b99-171">No project template support at this time.</span></span>

---

<span data-ttu-id="33b99-172">例如：</span><span class="sxs-lookup"><span data-stu-id="33b99-172">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-razor-views-pages-library.csproj)]

<span data-ttu-id="33b99-173">如果專案是以 .NET Standard 為目標，則需要[AspNetCore 的 Mvc](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc)封裝參考。</span><span class="sxs-lookup"><span data-stu-id="33b99-173">If the project targets .NET Standard instead, a [Microsoft.AspNetCore.Mvc](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc) package reference is required.</span></span> <span data-ttu-id="33b99-174">`Microsoft.AspNetCore.Mvc` 套件已移至 ASP.NET Core 3.0 中的共用架構，因此不再發佈。</span><span class="sxs-lookup"><span data-stu-id="33b99-174">The `Microsoft.AspNetCore.Mvc` package moved into the shared framework in ASP.NET Core 3.0 and is therefore no longer published.</span></span> <span data-ttu-id="33b99-175">例如：</span><span class="sxs-lookup"><span data-stu-id="33b99-175">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-razor-views-pages-library.csproj?highlight=8)]

### <a name="tag-helpers"></a><span data-ttu-id="33b99-176">標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="33b99-176">Tag Helpers</span></span>

<span data-ttu-id="33b99-177">包含[標記](xref:mvc/views/tag-helpers/intro)協助程式的專案應該使用 `Microsoft.NET.Sdk` SDK。</span><span class="sxs-lookup"><span data-stu-id="33b99-177">A project that includes [Tag Helpers](xref:mvc/views/tag-helpers/intro) should use the `Microsoft.NET.Sdk` SDK.</span></span> <span data-ttu-id="33b99-178">如果目標是 .NET Core 3.x，請加入共用架構的 `<FrameworkReference>` 元素。</span><span class="sxs-lookup"><span data-stu-id="33b99-178">If targeting .NET Core 3.x, add a `<FrameworkReference>` element for the shared framework.</span></span> <span data-ttu-id="33b99-179">例如：</span><span class="sxs-lookup"><span data-stu-id="33b99-179">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj)]

<span data-ttu-id="33b99-180">如果目標 .NET Standard （以支援早于 ASP.NET Core 3.x 的版本），請將套件參考新增至[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor)。</span><span class="sxs-lookup"><span data-stu-id="33b99-180">If targeting .NET Standard (to support versions earlier than ASP.NET Core 3.x), add a package reference to [Microsoft.AspNetCore.Mvc.Razor](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor).</span></span> <span data-ttu-id="33b99-181">`Microsoft.AspNetCore.Mvc.Razor` 套件已移至共用架構中，因此不會再發佈。</span><span class="sxs-lookup"><span data-stu-id="33b99-181">The `Microsoft.AspNetCore.Mvc.Razor` package moved into the shared framework and is therefore no longer published.</span></span> <span data-ttu-id="33b99-182">例如：</span><span class="sxs-lookup"><span data-stu-id="33b99-182">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-tag-helpers-library.csproj)]

### <a name="view-components"></a><span data-ttu-id="33b99-183">檢視元件</span><span class="sxs-lookup"><span data-stu-id="33b99-183">View components</span></span>

<span data-ttu-id="33b99-184">包含[View 元件](xref:mvc/views/view-components)的專案應該使用 `Microsoft.NET.Sdk` SDK。</span><span class="sxs-lookup"><span data-stu-id="33b99-184">A project that includes [View components](xref:mvc/views/view-components) should use the `Microsoft.NET.Sdk` SDK.</span></span> <span data-ttu-id="33b99-185">如果目標是 .NET Core 3.x，請加入共用架構的 `<FrameworkReference>` 元素。</span><span class="sxs-lookup"><span data-stu-id="33b99-185">If targeting .NET Core 3.x, add a `<FrameworkReference>` element for the shared framework.</span></span> <span data-ttu-id="33b99-186">例如：</span><span class="sxs-lookup"><span data-stu-id="33b99-186">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj)]

<span data-ttu-id="33b99-187">如果目標 .NET Standard （以支援早于 ASP.NET Core 3.x 的版本），請將套件參考新增至[AspNetCore ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures)。</span><span class="sxs-lookup"><span data-stu-id="33b99-187">If targeting .NET Standard (to support versions earlier than ASP.NET Core 3.x), add a package reference to [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures).</span></span> <span data-ttu-id="33b99-188">`Microsoft.AspNetCore.Mvc.ViewFeatures` 套件已移至共用架構中，因此不會再發佈。</span><span class="sxs-lookup"><span data-stu-id="33b99-188">The `Microsoft.AspNetCore.Mvc.ViewFeatures` package moved into the shared framework and is therefore no longer published.</span></span> <span data-ttu-id="33b99-189">例如：</span><span class="sxs-lookup"><span data-stu-id="33b99-189">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-view-components-library.csproj)]

## <a name="support-multiple-aspnet-core-versions"></a><span data-ttu-id="33b99-190">支援多個 ASP.NET Core 版本</span><span class="sxs-lookup"><span data-stu-id="33b99-190">Support multiple ASP.NET Core versions</span></span>

<span data-ttu-id="33b99-191">撰寫支援多個 ASP.NET Core 變體的程式庫時，需要多目標。</span><span class="sxs-lookup"><span data-stu-id="33b99-191">Multi-targeting is required to author a library that supports multiple variants of ASP.NET Core.</span></span> <span data-ttu-id="33b99-192">假設有一個標記協助程式程式庫必須支援下列 ASP.NET Core 變體的情況：</span><span class="sxs-lookup"><span data-stu-id="33b99-192">Consider a scenario in which a Tag Helpers library must support the following ASP.NET Core variants:</span></span>

* <span data-ttu-id="33b99-193">ASP.NET Core 2.1 目標 .NET Framework 4.6。1</span><span class="sxs-lookup"><span data-stu-id="33b99-193">ASP.NET Core 2.1 targeting .NET Framework 4.6.1</span></span>
* <span data-ttu-id="33b99-194">以 .NET Core 2.x 為目標的 ASP.NET Core 2。x</span><span class="sxs-lookup"><span data-stu-id="33b99-194">ASP.NET Core 2.x targeting .NET Core 2.x</span></span>
* <span data-ttu-id="33b99-195">以 .NET Core 3.x 為目標的 ASP.NET Core 3。x</span><span class="sxs-lookup"><span data-stu-id="33b99-195">ASP.NET Core 3.x targeting .NET Core 3.x</span></span>

<span data-ttu-id="33b99-196">下列專案檔透過 `TargetFrameworks` 屬性來支援這些變體：</span><span class="sxs-lookup"><span data-stu-id="33b99-196">The following project file supports these variants via the `TargetFrameworks` property:</span></span>

[!code-xml[](target-aspnetcore/samples/multi-tfm/recommended-tag-helpers-library.csproj)]

<span data-ttu-id="33b99-197">使用上述專案檔：</span><span class="sxs-lookup"><span data-stu-id="33b99-197">With the preceding project file:</span></span>

* <span data-ttu-id="33b99-198">會為所有取用者新增 `Markdig` 套件。</span><span class="sxs-lookup"><span data-stu-id="33b99-198">The `Markdig` package is added for all consumers.</span></span>
* <span data-ttu-id="33b99-199">針對以 .NET Framework 4.6.1 或更新版本或 .NET Core 2.x 為目標的取用者，會加入[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor)的參考。</span><span class="sxs-lookup"><span data-stu-id="33b99-199">A reference to [Microsoft.AspNetCore.Mvc.Razor](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor) is added for consumers targeting .NET Framework 4.6.1 or later or .NET Core 2.x.</span></span> <span data-ttu-id="33b99-200">由於回溯相容性，套件的版本2.1.0 會與 ASP.NET Core 2.2 搭配運作。</span><span class="sxs-lookup"><span data-stu-id="33b99-200">Version 2.1.0 of the package works with ASP.NET Core 2.2 because of backwards compatibility.</span></span>
* <span data-ttu-id="33b99-201">針對以 .NET Core 3.x 為目標的取用者，會參考共用架構。</span><span class="sxs-lookup"><span data-stu-id="33b99-201">The shared framework is referenced for consumers targeting .NET Core 3.x.</span></span> <span data-ttu-id="33b99-202">`Microsoft.AspNetCore.Mvc.Razor` 套件包含在共用架構中。</span><span class="sxs-lookup"><span data-stu-id="33b99-202">The `Microsoft.AspNetCore.Mvc.Razor` package is included in the shared framework.</span></span>

<span data-ttu-id="33b99-203">或者，.NET Standard 2.0 可能會以為目標，而不是以 .NET Core 2.1 和 .NET Framework 4.6.1 為目標：</span><span class="sxs-lookup"><span data-stu-id="33b99-203">Alternatively, .NET Standard 2.0 could be targeted instead of targeting both .NET Core 2.1 and .NET Framework 4.6.1:</span></span>

[!code-xml[](target-aspnetcore/samples/multi-tfm/alternative-tag-helpers-library.csproj?highlight=4)]

<span data-ttu-id="33b99-204">使用上述專案檔時，會有下列注意事項：</span><span class="sxs-lookup"><span data-stu-id="33b99-204">With the preceding project file, the following caveats exist:</span></span>

* <span data-ttu-id="33b99-205">由於程式庫只包含標籤協助程式，因此以 ASP.NET Core 執行的特定平臺為目標會更直接： .NET Core 和 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="33b99-205">Since the library only contains Tag Helpers, it's more straightforward to target the specific platforms on which ASP.NET Core runs: .NET Core and .NET Framework.</span></span> <span data-ttu-id="33b99-206">標籤協助程式無法供其他 .NET Standard 2.0 相容的目標架構（例如 Unity、UWP 和 Xamarin）使用。</span><span class="sxs-lookup"><span data-stu-id="33b99-206">Tag Helpers can't be used by other .NET Standard 2.0-compliant target frameworks such as Unity, UWP, and Xamarin.</span></span>
* <span data-ttu-id="33b99-207">使用 .NET Framework 中的 .NET Standard 2.0，會有一些在 .NET Framework 4.7.2 中已經解決的問題。</span><span class="sxs-lookup"><span data-stu-id="33b99-207">Using .NET Standard 2.0 from .NET Framework has some issues that were addressed in .NET Framework 4.7.2.</span></span> <span data-ttu-id="33b99-208">您可以藉由將目標設為 .NET Framework 4.6.1，藉由使用 .NET Framework 4.6.1 透過4.7.1 來改善取用者的體驗。</span><span class="sxs-lookup"><span data-stu-id="33b99-208">You can improve the experience for consumers using .NET Framework 4.6.1 through 4.7.1 by targeting .NET Framework 4.6.1.</span></span>

<span data-ttu-id="33b99-209">如果您的程式庫需要呼叫平臺特定的 Api，請以特定的 .NET 部署為目標，而不是 .NET Standard。</span><span class="sxs-lookup"><span data-stu-id="33b99-209">If your library needs to call platform-specific APIs, target specific .NET implementations instead of .NET Standard.</span></span> <span data-ttu-id="33b99-210">如需詳細資訊，請參閱[多目標](/dotnet/standard/library-guidance/cross-platform-targeting#multi-targeting)。</span><span class="sxs-lookup"><span data-stu-id="33b99-210">For more information, see [Multi-targeting](/dotnet/standard/library-guidance/cross-platform-targeting#multi-targeting).</span></span>

## <a name="use-an-api-that-hasnt-changed"></a><span data-ttu-id="33b99-211">使用未變更的 API</span><span class="sxs-lookup"><span data-stu-id="33b99-211">Use an API that hasn't changed</span></span>

<span data-ttu-id="33b99-212">想像您要將中介軟體程式庫從 .NET Core 2.2 升級至3.0 的案例。</span><span class="sxs-lookup"><span data-stu-id="33b99-212">Imagine a scenario in which you're upgrading a middleware library from .NET Core 2.2 to 3.0.</span></span> <span data-ttu-id="33b99-213">程式庫中使用的 ASP.NET Core 中介軟體 Api 未在 ASP.NET Core 2.2 和3.0 之間變更。</span><span class="sxs-lookup"><span data-stu-id="33b99-213">The ASP.NET Core middleware APIs being used in the library haven't changed between ASP.NET Core 2.2 and 3.0.</span></span> <span data-ttu-id="33b99-214">若要繼續支援 .NET Core 3.0 中的中介軟體程式庫，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="33b99-214">To continue supporting the middleware library in .NET Core 3.0, take the following steps:</span></span>

* <span data-ttu-id="33b99-215">遵循[標準程式庫指引](/dotnet/standard/library-guidance/)。</span><span class="sxs-lookup"><span data-stu-id="33b99-215">Follow the [standard library guidance](/dotnet/standard/library-guidance/).</span></span>
* <span data-ttu-id="33b99-216">如果對應的元件不存在於共用架構中，則為每個 API 的 NuGet 套件新增套件參考。</span><span class="sxs-lookup"><span data-stu-id="33b99-216">Add a package reference for each API's NuGet package if the corresponding assembly doesn't exist in the shared framework.</span></span>

## <a name="use-an-api-that-changed"></a><span data-ttu-id="33b99-217">使用已變更的 API</span><span class="sxs-lookup"><span data-stu-id="33b99-217">Use an API that changed</span></span>

<span data-ttu-id="33b99-218">假設您要將程式庫從 .NET Core 2.2 升級至 .NET Core 3.0。</span><span class="sxs-lookup"><span data-stu-id="33b99-218">Imagine a scenario in which you're upgrading a library from .NET Core 2.2 to .NET Core 3.0.</span></span> <span data-ttu-id="33b99-219">程式庫中使用的 ASP.NET Core API 在 ASP.NET Core 3.0 中有[重大變更](/dotnet/core/compatibility/breaking-changes)。</span><span class="sxs-lookup"><span data-stu-id="33b99-219">An ASP.NET Core API being used in the library has a [breaking change](/dotnet/core/compatibility/breaking-changes) in ASP.NET Core 3.0.</span></span> <span data-ttu-id="33b99-220">請考慮是否可以將程式庫改寫成不要在所有版本中使用中斷的 API。</span><span class="sxs-lookup"><span data-stu-id="33b99-220">Consider whether the library can be rewritten to not use the broken API in all versions.</span></span>

<span data-ttu-id="33b99-221">如果您可以重新撰寫程式庫，請執行此動作，並繼續以較早的目標架構（例如，.NET Standard 2.0 或 .NET Framework 4.6.1）為目標，並搭配套件參考。</span><span class="sxs-lookup"><span data-stu-id="33b99-221">If you can rewrite the library, do so and continue to target an earlier target framework (for example, .NET Standard 2.0 or .NET Framework 4.6.1) with package references.</span></span>

<span data-ttu-id="33b99-222">如果您無法重寫程式庫，請採取下列步驟：</span><span class="sxs-lookup"><span data-stu-id="33b99-222">If you can't rewrite the library, take the following steps:</span></span>

* <span data-ttu-id="33b99-223">新增 .NET Core 3.0 的目標。</span><span class="sxs-lookup"><span data-stu-id="33b99-223">Add a target for .NET Core 3.0.</span></span>
* <span data-ttu-id="33b99-224">新增共用架構的 `<FrameworkReference>` 元素。</span><span class="sxs-lookup"><span data-stu-id="33b99-224">Add a `<FrameworkReference>` element for the shared framework.</span></span>
* <span data-ttu-id="33b99-225">使用[#if 預處理器](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)指示詞搭配適當的目標 framework 符號，以有條件地編譯器代碼。</span><span class="sxs-lookup"><span data-stu-id="33b99-225">Use the [#if preprocessor directive](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if) with the appropriate target framework symbol to conditionally compile code.</span></span>

<span data-ttu-id="33b99-226">例如，HTTP 要求和回應資料流程的同步讀取和寫入預設會停用，從 ASP.NET Core 3.0。</span><span class="sxs-lookup"><span data-stu-id="33b99-226">For example, synchronous reads and writes on HTTP request and response streams are disabled by default as of ASP.NET Core 3.0.</span></span> <span data-ttu-id="33b99-227">ASP.NET Core 2.2 預設支援同步行為。</span><span class="sxs-lookup"><span data-stu-id="33b99-227">ASP.NET Core 2.2 supports the synchronous behavior by default.</span></span> <span data-ttu-id="33b99-228">假設有一個中介軟體程式庫，在發生 IO 時應該啟用同步讀取和寫入。</span><span class="sxs-lookup"><span data-stu-id="33b99-228">Consider a middleware library in which synchronous reads and writes should be enabled where IO is occurring.</span></span> <span data-ttu-id="33b99-229">程式庫應該括住程式碼，以在適當的預處理器指示詞中啟用同步功能。</span><span class="sxs-lookup"><span data-stu-id="33b99-229">The library should enclose the code to enable synchronous features in the appropriate preprocessor directive.</span></span> <span data-ttu-id="33b99-230">例如：</span><span class="sxs-lookup"><span data-stu-id="33b99-230">For example:</span></span>

[!code-csharp[](target-aspnetcore/samples/middleware.cs?highlight=9-24)]

## <a name="use-an-api-introduced-in-30"></a><span data-ttu-id="33b99-231">使用3.0 中引進的 API</span><span class="sxs-lookup"><span data-stu-id="33b99-231">Use an API introduced in 3.0</span></span>

<span data-ttu-id="33b99-232">假設您想要使用 ASP.NET Core 3.0 中引進的 ASP.NET Core API。</span><span class="sxs-lookup"><span data-stu-id="33b99-232">Imagine that you want to use an ASP.NET Core API that was introduced in ASP.NET Core 3.0.</span></span> <span data-ttu-id="33b99-233">請考慮下列問題：</span><span class="sxs-lookup"><span data-stu-id="33b99-233">Consider the following questions:</span></span>

1. <span data-ttu-id="33b99-234">程式庫的功能是否需要新的 API？</span><span class="sxs-lookup"><span data-stu-id="33b99-234">Does the library functionally require the new API?</span></span>
1. <span data-ttu-id="33b99-235">程式庫可以用不同的方式來執行這項功能嗎？</span><span class="sxs-lookup"><span data-stu-id="33b99-235">Can the library implement this feature in a different way?</span></span>

<span data-ttu-id="33b99-236">如果程式庫的功能需要 API，而且沒有任何方法可將它執行于下層：</span><span class="sxs-lookup"><span data-stu-id="33b99-236">If the library functionally requires the API and there's no way to implement it down-level:</span></span>

* <span data-ttu-id="33b99-237">僅以 .NET Core 2.x 為目標。</span><span class="sxs-lookup"><span data-stu-id="33b99-237">Target .NET Core 3.x only.</span></span>
* <span data-ttu-id="33b99-238">新增共用架構的 `<FrameworkReference>` 元素。</span><span class="sxs-lookup"><span data-stu-id="33b99-238">Add a `<FrameworkReference>` element for the shared framework.</span></span>

<span data-ttu-id="33b99-239">如果程式庫可以使用不同的方式來執行功能：</span><span class="sxs-lookup"><span data-stu-id="33b99-239">If the library can implement the feature in a different way:</span></span>

* <span data-ttu-id="33b99-240">新增 .NET Core 3.x 做為目標架構。</span><span class="sxs-lookup"><span data-stu-id="33b99-240">Add .NET Core 3.x as a target framework.</span></span>
* <span data-ttu-id="33b99-241">新增共用架構的 `<FrameworkReference>` 元素。</span><span class="sxs-lookup"><span data-stu-id="33b99-241">Add a `<FrameworkReference>` element for the shared framework.</span></span>
* <span data-ttu-id="33b99-242">使用[#if 預處理器](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)指示詞搭配適當的目標 framework 符號，以有條件地編譯器代碼。</span><span class="sxs-lookup"><span data-stu-id="33b99-242">Use the [#if preprocessor directive](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if) with the appropriate target framework symbol to conditionally compile code.</span></span>

<span data-ttu-id="33b99-243">例如，下列標記協助程式會使用 ASP.NET Core 3.0 中引進的 <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 介面。</span><span class="sxs-lookup"><span data-stu-id="33b99-243">For example, the following Tag Helper uses the <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> interface introduced in ASP.NET Core 3.0.</span></span> <span data-ttu-id="33b99-244">以 .NET Core 3.0 為目標的取用者，會執行 `NETCOREAPP3_0` 目標 framework 符號所定義的程式碼路徑。</span><span class="sxs-lookup"><span data-stu-id="33b99-244">Consumers targeting .NET Core 3.0 execute the code path defined by the `NETCOREAPP3_0` target framework symbol.</span></span> <span data-ttu-id="33b99-245">標籤協助程式的函式參數類型會變更為 .NET Core 2.1 和 .NET Framework 4.6.1 取用者的 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment>。</span><span class="sxs-lookup"><span data-stu-id="33b99-245">The Tag Helper's constructor parameter type changes to <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> for .NET Core 2.1 and .NET Framework 4.6.1 consumers.</span></span> <span data-ttu-id="33b99-246">這是必要的變更，因為 ASP.NET Core 3.0 標示 `IHostingEnvironment` 為過時，並建議 `IWebHostEnvironment` 取代。</span><span class="sxs-lookup"><span data-stu-id="33b99-246">This change was necessary because ASP.NET Core 3.0 marked `IHostingEnvironment` as obsolete and recommended `IWebHostEnvironment` as the replacement.</span></span>

```csharp
[HtmlTargetElement("script", Attributes = "asp-inline")]
public class ScriptInliningTagHelper : TagHelper
{
    private readonly IFileProvider _wwwroot;

#if NETCOREAPP3_0
    public ScriptInliningTagHelper(IWebHostEnvironment env)
#else
    public ScriptInliningTagHelper(IHostingEnvironment env)
#endif
    {
        _wwwroot = env.WebRootFileProvider;
    }

    // code omitted for brevity
}
```

<span data-ttu-id="33b99-247">下列多目標專案檔支援此標記協助程式案例：</span><span class="sxs-lookup"><span data-stu-id="33b99-247">The following multi-targeted project file supports this Tag Helper scenario:</span></span>

[!code-xml[](target-aspnetcore/samples/multi-tfm/recommended-tag-helpers-library.csproj)]

## <a name="use-an-api-removed-from-the-shared-framework"></a><span data-ttu-id="33b99-248">使用從共用架構移除的 API</span><span class="sxs-lookup"><span data-stu-id="33b99-248">Use an API removed from the shared framework</span></span>

<span data-ttu-id="33b99-249">若要使用已從共用架構中移除的 ASP.NET Core 元件，請新增適當的套件參考。</span><span class="sxs-lookup"><span data-stu-id="33b99-249">To use an ASP.NET Core assembly that was removed from the shared framework, add the appropriate package reference.</span></span> <span data-ttu-id="33b99-250">如需 ASP.NET Core 3.0 中從共用架構移除的套件清單，請參閱[移除過時的套件參考](xref:migration/22-to-30#remove-obsolete-package-references)。</span><span class="sxs-lookup"><span data-stu-id="33b99-250">For a list of packages removed from the shared framework in ASP.NET Core 3.0, see [Remove obsolete package references](xref:migration/22-to-30#remove-obsolete-package-references).</span></span>

<span data-ttu-id="33b99-251">例如，若要新增 Web API 用戶端：</span><span class="sxs-lookup"><span data-stu-id="33b99-251">For example, to add the web API client:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netcoreapp3.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <FrameworkReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNet.WebApi.Client" Version="5.2.7" />
  </ItemGroup>

</Project>
```

## <a name="additional-resources"></a><span data-ttu-id="33b99-252">其他資源</span><span class="sxs-lookup"><span data-stu-id="33b99-252">Additional resources</span></span>

* <xref:razor-pages/ui-class>
* <xref:blazor/class-libraries>
* [<span data-ttu-id="33b99-253">.NET 執行支援</span><span class="sxs-lookup"><span data-stu-id="33b99-253">.NET implementation support</span></span>](/dotnet/standard/net-standard#net-implementation-support)
* [<span data-ttu-id="33b99-254">.NET 支援原則</span><span class="sxs-lookup"><span data-stu-id="33b99-254">.NET support policies</span></span>](https://dotnet.microsoft.com/platform/support/policy)
