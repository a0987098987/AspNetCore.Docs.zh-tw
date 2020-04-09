---
title: 在類別庫中使用ASP.NET核心 API
author: scottaddie
description: 瞭解如何在類庫中使用ASP.NET核心 API。
ms.author: scaddie
ms.custom: mvc
ms.date: 12/16/2019
no-loc:
- Blazor
uid: fundamentals/target-aspnetcore
ms.openlocfilehash: 5374d7eec4334223a4bba7ee26cb6e2f208ed20b
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977193"
---
# <a name="use-aspnet-core-apis-in-a-class-library"></a><span data-ttu-id="25f2e-103">在類別庫中使用ASP.NET核心 API</span><span class="sxs-lookup"><span data-stu-id="25f2e-103">Use ASP.NET Core APIs in a class library</span></span>

<span data-ttu-id="25f2e-104">作者：[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="25f2e-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="25f2e-105">本文件提供有關在類庫中使用ASP.NET核心 API 的指導。</span><span class="sxs-lookup"><span data-stu-id="25f2e-105">This document provides guidance for using ASP.NET Core APIs in a class library.</span></span> <span data-ttu-id="25f2e-106">有關所有其他庫指南,請參閱[開源庫指南](/dotnet/standard/library-guidance/)。</span><span class="sxs-lookup"><span data-stu-id="25f2e-106">For all other library guidance, see [Open-source library guidance](/dotnet/standard/library-guidance/).</span></span>

## <a name="determine-which-aspnet-core-versions-to-support"></a><span data-ttu-id="25f2e-107">確定哪些ASP.NET核心版本要支援</span><span class="sxs-lookup"><span data-stu-id="25f2e-107">Determine which ASP.NET Core versions to support</span></span>

<span data-ttu-id="25f2e-108">ASP.NET核心遵守[.NET 核心支援策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)。</span><span class="sxs-lookup"><span data-stu-id="25f2e-108">ASP.NET Core adheres to the [.NET Core support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span> <span data-ttu-id="25f2e-109">在確定庫中要支援哪些ASP.NET核心版本時,請參閱支援策略。</span><span class="sxs-lookup"><span data-stu-id="25f2e-109">Consult the support policy when determining which ASP.NET Core versions to support in a library.</span></span> <span data-ttu-id="25f2e-110">庫應:</span><span class="sxs-lookup"><span data-stu-id="25f2e-110">A library should:</span></span>

* <span data-ttu-id="25f2e-111">努力支援分類為*長期支援*(LTS) 的所有 ASP.NET 核心版本。</span><span class="sxs-lookup"><span data-stu-id="25f2e-111">Make an effort to support all ASP.NET Core versions classified as *Long-Term Support* (LTS).</span></span>
* <span data-ttu-id="25f2e-112">不覺得有義務支援ASP.NET核心版本歸類為*生命終止*(EOL)。</span><span class="sxs-lookup"><span data-stu-id="25f2e-112">Not feel obligated to support ASP.NET Core versions classified as *End of Life* (EOL).</span></span>

<span data-ttu-id="25f2e-113">當ASP.NET核心的預覽版本可用時,重大更改將張貼在[aspnet/公告](https://github.com/aspnet/Announcements/issues)GitHub 儲存庫中。</span><span class="sxs-lookup"><span data-stu-id="25f2e-113">As preview releases of ASP.NET Core are made available, breaking changes are posted in the [aspnet/Announcements](https://github.com/aspnet/Announcements/issues) GitHub repository.</span></span> <span data-ttu-id="25f2e-114">在開發框架功能時,可以對庫進行相容性測試。</span><span class="sxs-lookup"><span data-stu-id="25f2e-114">Compatibility testing of libraries can be conducted as framework features are being developed.</span></span>

## <a name="use-the-aspnet-core-shared-framework"></a><span data-ttu-id="25f2e-115">使用ASP.NET核心共用框架</span><span class="sxs-lookup"><span data-stu-id="25f2e-115">Use the ASP.NET Core shared framework</span></span>

<span data-ttu-id="25f2e-116">隨著 .NET Core 3.0 的發佈,許多ASP.NET核心程式集不再作為包發佈到 NuGet。</span><span class="sxs-lookup"><span data-stu-id="25f2e-116">With the release of .NET Core 3.0, many ASP.NET Core assemblies are no longer published to NuGet as packages.</span></span> <span data-ttu-id="25f2e-117">相反,程式集包含在共用框架中`Microsoft.AspNetCore.App`,該框架與 .NET Core SDK 和運行時安裝程式一起安裝。</span><span class="sxs-lookup"><span data-stu-id="25f2e-117">Instead, the assemblies are included in the `Microsoft.AspNetCore.App` shared framework, which is installed with the .NET Core SDK and runtime installers.</span></span> <span data-ttu-id="25f2e-118">有關不再發布的套件的清單,請參閱[刪除過時的套件參考](xref:migration/22-to-30#remove-obsolete-package-references)。</span><span class="sxs-lookup"><span data-stu-id="25f2e-118">For a list of packages no longer being published, see [Remove obsolete package references](xref:migration/22-to-30#remove-obsolete-package-references).</span></span>

<span data-ttu-id="25f2e-119">自 .NET Core 3.0 起`Microsoft.NET.Sdk.Web`,使用 MSBuild SDK 的專案隱式引用共用框架。</span><span class="sxs-lookup"><span data-stu-id="25f2e-119">As of .NET Core 3.0, projects using the `Microsoft.NET.Sdk.Web` MSBuild SDK implicitly reference the shared framework.</span></span> <span data-ttu-id="25f2e-120">使用`Microsoft.NET.Sdk`或`Microsoft.NET.Sdk.Razor`SDK 的項目必須引用 ASP.NET 核心在共用框架中使用 ASP.NET 核心 API。</span><span class="sxs-lookup"><span data-stu-id="25f2e-120">Projects using the `Microsoft.NET.Sdk` or `Microsoft.NET.Sdk.Razor` SDK must reference ASP.NET Core to use ASP.NET Core APIs in the shared framework.</span></span>

<span data-ttu-id="25f2e-121">要參考ASP.NET核心,請向專案`<FrameworkReference>`檔案加入以下元素:</span><span class="sxs-lookup"><span data-stu-id="25f2e-121">To reference ASP.NET Core, add the following `<FrameworkReference>` element to your project file:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj?highlight=8)]

<span data-ttu-id="25f2e-122">僅支援以這種方式引用ASP.NET核心。""專案的目標是 .NET Core 3.x。</span><span class="sxs-lookup"><span data-stu-id="25f2e-122">Referencing ASP.NET Core in this manner is only supported for projects targeting .NET Core 3.x.</span></span>

## <a name="include-blazor-extensibility"></a><span data-ttu-id="25f2e-123">包括布拉佐可擴充性</span><span class="sxs-lookup"><span data-stu-id="25f2e-123">Include Blazor extensibility</span></span>

<span data-ttu-id="25f2e-124">布拉佐爾支援網路組裝 (WASM) 與伺服器[託管模型](xref:blazor/hosting-models)。</span><span class="sxs-lookup"><span data-stu-id="25f2e-124">Blazor supports WebAssembly (WASM) and Server [hosting models](xref:blazor/hosting-models).</span></span> <span data-ttu-id="25f2e-125">除非有特定原因,否則[Razor 元件](xref:blazor/components)庫應支援這兩個託管模型。</span><span class="sxs-lookup"><span data-stu-id="25f2e-125">Unless there's a specific reason not to, a [Razor components](xref:blazor/components) library should support both hosting models.</span></span> <span data-ttu-id="25f2e-126">Razor 元件庫必須使用[Microsoft.NET.Sdk.Razor SDK](xref:razor-pages/sdk)。</span><span class="sxs-lookup"><span data-stu-id="25f2e-126">A Razor components library must use the [Microsoft.NET.Sdk.Razor SDK](xref:razor-pages/sdk).</span></span>

### <a name="support-both-hosting-models"></a><span data-ttu-id="25f2e-127">支援兩種託管模式</span><span class="sxs-lookup"><span data-stu-id="25f2e-127">Support both hosting models</span></span>

<span data-ttu-id="25f2e-128">要支援[Blazor 伺服器](xref:blazor/hosting-models#blazor-server)和[Blazor WASM](xref:blazor/hosting-models#blazor-webassembly)專案的 Razor 元件消耗,請使用以下說明為您的編輯器。</span><span class="sxs-lookup"><span data-stu-id="25f2e-128">To support Razor component consumption from both [Blazor Server](xref:blazor/hosting-models#blazor-server) and [Blazor WASM](xref:blazor/hosting-models#blazor-webassembly) projects, use the following instructions for your editor.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="25f2e-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="25f2e-129">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="25f2e-130">使用**Razor 類庫**專案範本。</span><span class="sxs-lookup"><span data-stu-id="25f2e-130">Use the **Razor Class Library** project template.</span></span> <span data-ttu-id="25f2e-131">應取消選中範本的支持**頁面和視圖**複選框。</span><span class="sxs-lookup"><span data-stu-id="25f2e-131">The template's **Support pages and views** checkbox should be deselected.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="25f2e-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="25f2e-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="25f2e-133">在整合終端機中執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="25f2e-133">Run the following command in the integrated terminal:</span></span>

```dotnetcli
dotnet new razorclasslib
```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="25f2e-134">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="25f2e-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="25f2e-135">使用**Razor 類庫**專案範本。</span><span class="sxs-lookup"><span data-stu-id="25f2e-135">Use the **Razor Class Library** project template.</span></span>

---

<span data-ttu-id="25f2e-136">從樣本產生的項目執行以下操作:</span><span class="sxs-lookup"><span data-stu-id="25f2e-136">The project generated from the template does the following things:</span></span>

* <span data-ttu-id="25f2e-137">目標 .NET 標準 2.0.</span><span class="sxs-lookup"><span data-stu-id="25f2e-137">Targets .NET Standard 2.0.</span></span>
* <span data-ttu-id="25f2e-138">將 `RazorLangVersion` 屬性設定為 `3.0`。</span><span class="sxs-lookup"><span data-stu-id="25f2e-138">Sets the `RazorLangVersion` property to `3.0`.</span></span> <span data-ttu-id="25f2e-139">`3.0`是 .NET 核心 3.x 的預設值。</span><span class="sxs-lookup"><span data-stu-id="25f2e-139">`3.0` is the default value for .NET Core 3.x.</span></span>
* <span data-ttu-id="25f2e-140">新增以下包引言:</span><span class="sxs-lookup"><span data-stu-id="25f2e-140">Adds the following package references:</span></span>
  * [<span data-ttu-id="25f2e-141">微軟.AspNetCore.元件</span><span class="sxs-lookup"><span data-stu-id="25f2e-141">Microsoft.AspNetCore.Components</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Components)
  * [<span data-ttu-id="25f2e-142">微軟.AspNetCore.元件.Web</span><span class="sxs-lookup"><span data-stu-id="25f2e-142">Microsoft.AspNetCore.Components.Web</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Web)

<span data-ttu-id="25f2e-143">例如：</span><span class="sxs-lookup"><span data-stu-id="25f2e-143">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-razor-components-library.csproj)]

### <a name="support-a-specific-hosting-model"></a><span data-ttu-id="25f2e-144">支援特定的託管模型</span><span class="sxs-lookup"><span data-stu-id="25f2e-144">Support a specific hosting model</span></span>

<span data-ttu-id="25f2e-145">支援單個 Blazor 託管模式並不常見。</span><span class="sxs-lookup"><span data-stu-id="25f2e-145">It's far less common to support a single Blazor hosting model.</span></span> <span data-ttu-id="25f2e-146">例如,要僅支援[Blazor Server](xref:blazor/hosting-models#blazor-server)專案中的 Razor 元件消耗:</span><span class="sxs-lookup"><span data-stu-id="25f2e-146">As an example, to support Razor component consumption from [Blazor Server](xref:blazor/hosting-models#blazor-server) projects only:</span></span>

* <span data-ttu-id="25f2e-147">目標 .NET 核心 3.x.</span><span class="sxs-lookup"><span data-stu-id="25f2e-147">Target .NET Core 3.x.</span></span>
* <span data-ttu-id="25f2e-148">為`<FrameworkReference>`共用框架添加元素。</span><span class="sxs-lookup"><span data-stu-id="25f2e-148">Add a `<FrameworkReference>` element for the shared framework.</span></span>

<span data-ttu-id="25f2e-149">例如：</span><span class="sxs-lookup"><span data-stu-id="25f2e-149">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-razor-components-library.csproj)]

<span data-ttu-id="25f2e-150">有關包含 Razor 元件的函式庫的詳細資訊,請參閱[ASP.NET 核心剃刀元件類庫](xref:blazor/class-libraries)。</span><span class="sxs-lookup"><span data-stu-id="25f2e-150">For more information on libraries containing Razor components, see [ASP.NET Core Razor components class libraries](xref:blazor/class-libraries).</span></span>

## <a name="include-mvc-extensibility"></a><span data-ttu-id="25f2e-151">包含 MVC 可擴充性</span><span class="sxs-lookup"><span data-stu-id="25f2e-151">Include MVC extensibility</span></span>

<span data-ttu-id="25f2e-152">本節概述了對庫的建議,其中包括:</span><span class="sxs-lookup"><span data-stu-id="25f2e-152">This section outlines recommendations for libraries that include:</span></span>

* [<span data-ttu-id="25f2e-153">剃刀檢視或剃刀頁面</span><span class="sxs-lookup"><span data-stu-id="25f2e-153">Razor views or Razor Pages</span></span>](#razor-views-or-razor-pages)
* [<span data-ttu-id="25f2e-154">標記說明器</span><span class="sxs-lookup"><span data-stu-id="25f2e-154">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="25f2e-155">檢視元件</span><span class="sxs-lookup"><span data-stu-id="25f2e-155">View components</span></span>](#view-components)

<span data-ttu-id="25f2e-156">本節不討論多目標以支援多個版本的 MVC。</span><span class="sxs-lookup"><span data-stu-id="25f2e-156">This section doesn't discuss multi-targeting to support multiple versions of MVC.</span></span> <span data-ttu-id="25f2e-157">有關支援多個ASP.NET核心版本的指南,請參閱[支援多個ASP.NET核心版本](#support-multiple-aspnet-core-versions)。</span><span class="sxs-lookup"><span data-stu-id="25f2e-157">For guidance on supporting multiple ASP.NET Core versions, see [Support multiple ASP.NET Core versions](#support-multiple-aspnet-core-versions).</span></span>

### <a name="razor-views-or-razor-pages"></a><span data-ttu-id="25f2e-158">剃刀檢視或剃刀頁面</span><span class="sxs-lookup"><span data-stu-id="25f2e-158">Razor views or Razor Pages</span></span>

<span data-ttu-id="25f2e-159">包含 Razor[檢視](xref:mvc/views/overview)或[Razor 頁面](xref:razor-pages/index)的項目必須使用[Microsoft.NET.Sdk.Razor SDK](xref:razor-pages/sdk)。</span><span class="sxs-lookup"><span data-stu-id="25f2e-159">A project that includes [Razor views](xref:mvc/views/overview) or [Razor Pages](xref:razor-pages/index) must use the [Microsoft.NET.Sdk.Razor SDK](xref:razor-pages/sdk).</span></span>

<span data-ttu-id="25f2e-160">如果項目的目標是 .NET Core 3.x,則需要:</span><span class="sxs-lookup"><span data-stu-id="25f2e-160">If the project targets .NET Core 3.x, it requires:</span></span>

* <span data-ttu-id="25f2e-161">設定為`AddRazorSupportForMvc``true`的 MSBuild 屬性設定為 。</span><span class="sxs-lookup"><span data-stu-id="25f2e-161">An `AddRazorSupportForMvc` MSBuild property set to `true`.</span></span>
* <span data-ttu-id="25f2e-162">`<FrameworkReference>`共用框架的元素。</span><span class="sxs-lookup"><span data-stu-id="25f2e-162">A `<FrameworkReference>` element for the shared framework.</span></span>

<span data-ttu-id="25f2e-163">**Razor 函式庫**專案樣本滿足針對 .NET Core 3.x 的專案的上述要求。</span><span class="sxs-lookup"><span data-stu-id="25f2e-163">The **Razor Class Library** project template satisfies the preceding requirements for projects targeting .NET Core 3.x.</span></span> <span data-ttu-id="25f2e-164">對編輯器使用以下說明。</span><span class="sxs-lookup"><span data-stu-id="25f2e-164">Use the following instructions for your editor.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="25f2e-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="25f2e-165">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="25f2e-166">使用**Razor 類庫**專案範本。</span><span class="sxs-lookup"><span data-stu-id="25f2e-166">Use the **Razor Class Library** project template.</span></span> <span data-ttu-id="25f2e-167">應選擇範本的支持**頁面和視圖**複選框。</span><span class="sxs-lookup"><span data-stu-id="25f2e-167">The template's **Support pages and views** checkbox should be selected.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="25f2e-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="25f2e-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="25f2e-169">在整合終端機中執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="25f2e-169">Run the following command in the integrated terminal:</span></span>

```dotnetcli
dotnet new razorclasslib -s
```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="25f2e-170">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="25f2e-170">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="25f2e-171">目前不支援專案範本。</span><span class="sxs-lookup"><span data-stu-id="25f2e-171">No project template support at this time.</span></span>

---

<span data-ttu-id="25f2e-172">例如：</span><span class="sxs-lookup"><span data-stu-id="25f2e-172">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-razor-views-pages-library.csproj)]

<span data-ttu-id="25f2e-173">如果專案以 .NET 標準為目標,則需要[Microsoft.AspNetCore.Mvc](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc)包引用。</span><span class="sxs-lookup"><span data-stu-id="25f2e-173">If the project targets .NET Standard instead, a [Microsoft.AspNetCore.Mvc](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc) package reference is required.</span></span> <span data-ttu-id="25f2e-174">該`Microsoft.AspNetCore.Mvc`包在ASP.NET Core 3.0 中進入共用框架,因此不再發佈。</span><span class="sxs-lookup"><span data-stu-id="25f2e-174">The `Microsoft.AspNetCore.Mvc` package moved into the shared framework in ASP.NET Core 3.0 and is therefore no longer published.</span></span> <span data-ttu-id="25f2e-175">例如：</span><span class="sxs-lookup"><span data-stu-id="25f2e-175">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-razor-views-pages-library.csproj?highlight=8)]

### <a name="tag-helpers"></a><span data-ttu-id="25f2e-176">標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="25f2e-176">Tag Helpers</span></span>

<span data-ttu-id="25f2e-177">包含[標記說明器](xref:mvc/views/tag-helpers/intro)的專案應`Microsoft.NET.Sdk`使用 SDK。</span><span class="sxs-lookup"><span data-stu-id="25f2e-177">A project that includes [Tag Helpers](xref:mvc/views/tag-helpers/intro) should use the `Microsoft.NET.Sdk` SDK.</span></span> <span data-ttu-id="25f2e-178">如果以 .NET Core`<FrameworkReference>`3.x 為目標,則為共用框架添加一個元素。</span><span class="sxs-lookup"><span data-stu-id="25f2e-178">If targeting .NET Core 3.x, add a `<FrameworkReference>` element for the shared framework.</span></span> <span data-ttu-id="25f2e-179">例如：</span><span class="sxs-lookup"><span data-stu-id="25f2e-179">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj)]

<span data-ttu-id="25f2e-180">如果定位為 .NET 標準(以支援ASP.NET Core 3.x 之前的版本),則添加對[Microsoft.AspNetCore.Mvc.Razor](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor)的包引用。</span><span class="sxs-lookup"><span data-stu-id="25f2e-180">If targeting .NET Standard (to support versions earlier than ASP.NET Core 3.x), add a package reference to [Microsoft.AspNetCore.Mvc.Razor](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor).</span></span> <span data-ttu-id="25f2e-181">該`Microsoft.AspNetCore.Mvc.Razor`包移動到共用框架中,因此不再發佈。</span><span class="sxs-lookup"><span data-stu-id="25f2e-181">The `Microsoft.AspNetCore.Mvc.Razor` package moved into the shared framework and is therefore no longer published.</span></span> <span data-ttu-id="25f2e-182">例如：</span><span class="sxs-lookup"><span data-stu-id="25f2e-182">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-tag-helpers-library.csproj)]

### <a name="view-components"></a><span data-ttu-id="25f2e-183">檢視元件</span><span class="sxs-lookup"><span data-stu-id="25f2e-183">View components</span></span>

<span data-ttu-id="25f2e-184">包含[檢視元件](xref:mvc/views/view-components)的專案應使用`Microsoft.NET.Sdk`SDK。</span><span class="sxs-lookup"><span data-stu-id="25f2e-184">A project that includes [View components](xref:mvc/views/view-components) should use the `Microsoft.NET.Sdk` SDK.</span></span> <span data-ttu-id="25f2e-185">如果以 .NET Core`<FrameworkReference>`3.x 為目標,則為共用框架添加一個元素。</span><span class="sxs-lookup"><span data-stu-id="25f2e-185">If targeting .NET Core 3.x, add a `<FrameworkReference>` element for the shared framework.</span></span> <span data-ttu-id="25f2e-186">例如：</span><span class="sxs-lookup"><span data-stu-id="25f2e-186">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj)]

<span data-ttu-id="25f2e-187">如果針對 .NET 標準(以支援早於 ASP.NET Core 3.x 的版本),則添加指向[Microsoft.AspNetCore.Mvc.View 功能](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures)的包引用。</span><span class="sxs-lookup"><span data-stu-id="25f2e-187">If targeting .NET Standard (to support versions earlier than ASP.NET Core 3.x), add a package reference to [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures).</span></span> <span data-ttu-id="25f2e-188">該`Microsoft.AspNetCore.Mvc.ViewFeatures`包移動到共用框架中,因此不再發佈。</span><span class="sxs-lookup"><span data-stu-id="25f2e-188">The `Microsoft.AspNetCore.Mvc.ViewFeatures` package moved into the shared framework and is therefore no longer published.</span></span> <span data-ttu-id="25f2e-189">例如：</span><span class="sxs-lookup"><span data-stu-id="25f2e-189">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-view-components-library.csproj)]

## <a name="support-multiple-aspnet-core-versions"></a><span data-ttu-id="25f2e-190">支援多個ASP.NET核心版本</span><span class="sxs-lookup"><span data-stu-id="25f2e-190">Support multiple ASP.NET Core versions</span></span>

<span data-ttu-id="25f2e-191">需要多目標來編寫支援ASP.NET核心的多個變體的庫。</span><span class="sxs-lookup"><span data-stu-id="25f2e-191">Multi-targeting is required to author a library that supports multiple variants of ASP.NET Core.</span></span> <span data-ttu-id="25f2e-192">請考慮標記說明器庫必須支援以下ASP.NET核心變體的情況:</span><span class="sxs-lookup"><span data-stu-id="25f2e-192">Consider a scenario in which a Tag Helpers library must support the following ASP.NET Core variants:</span></span>

* <span data-ttu-id="25f2e-193">ASP.NET核心 2.1 目標 .NET 框架 4.6.1</span><span class="sxs-lookup"><span data-stu-id="25f2e-193">ASP.NET Core 2.1 targeting .NET Framework 4.6.1</span></span>
* <span data-ttu-id="25f2e-194">ASP.NET核心 2.x 目標 .NET 核心 2.x</span><span class="sxs-lookup"><span data-stu-id="25f2e-194">ASP.NET Core 2.x targeting .NET Core 2.x</span></span>
* <span data-ttu-id="25f2e-195">ASP.NET核心 3.x 目標 .NET 核心 3.x</span><span class="sxs-lookup"><span data-stu-id="25f2e-195">ASP.NET Core 3.x targeting .NET Core 3.x</span></span>

<span data-ttu-id="25f2e-196">以下項目檔使用`TargetFrameworks`屬性支援這些變體:</span><span class="sxs-lookup"><span data-stu-id="25f2e-196">The following project file supports these variants via the `TargetFrameworks` property:</span></span>

[!code-xml[](target-aspnetcore/samples/multi-tfm/recommended-tag-helpers-library.csproj)]

<span data-ttu-id="25f2e-197">使用前面的項目檔:</span><span class="sxs-lookup"><span data-stu-id="25f2e-197">With the preceding project file:</span></span>

* <span data-ttu-id="25f2e-198">該`Markdig`包將針對所有消費者添加。</span><span class="sxs-lookup"><span data-stu-id="25f2e-198">The `Markdig` package is added for all consumers.</span></span>
* <span data-ttu-id="25f2e-199">為面向 .NET 框架 4.6.1 或更高版本或 .NET Core 2.x 的消費者添加了對[Microsoft.AspNetCore.Mvc.Razor](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor)的引用。</span><span class="sxs-lookup"><span data-stu-id="25f2e-199">A reference to [Microsoft.AspNetCore.Mvc.Razor](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor) is added for consumers targeting .NET Framework 4.6.1 or later or .NET Core 2.x.</span></span> <span data-ttu-id="25f2e-200">由於向後相容性,包的版本 2.1.0 與 ASP.NET Core 2.2 相容。</span><span class="sxs-lookup"><span data-stu-id="25f2e-200">Version 2.1.0 of the package works with ASP.NET Core 2.2 because of backwards compatibility.</span></span>
* <span data-ttu-id="25f2e-201">面向 .NET Core 3.x 的消費者引用了共用框架。</span><span class="sxs-lookup"><span data-stu-id="25f2e-201">The shared framework is referenced for consumers targeting .NET Core 3.x.</span></span> <span data-ttu-id="25f2e-202">該`Microsoft.AspNetCore.Mvc.Razor`包包含在共用框架中。</span><span class="sxs-lookup"><span data-stu-id="25f2e-202">The `Microsoft.AspNetCore.Mvc.Razor` package is included in the shared framework.</span></span>

<span data-ttu-id="25f2e-203">或者,.NET 標準 2.0 可以針對目標,而不是同時定位 .NET 核心 2.1 和 .NET 框架 4.6.1:</span><span class="sxs-lookup"><span data-stu-id="25f2e-203">Alternatively, .NET Standard 2.0 could be targeted instead of targeting both .NET Core 2.1 and .NET Framework 4.6.1:</span></span>

[!code-xml[](target-aspnetcore/samples/multi-tfm/alternative-tag-helpers-library.csproj?highlight=4)]

<span data-ttu-id="25f2e-204">使用前面的項目檔,存在以下注意事項:</span><span class="sxs-lookup"><span data-stu-id="25f2e-204">With the preceding project file, the following caveats exist:</span></span>

* <span data-ttu-id="25f2e-205">由於庫僅包含標記説明器,因此將目標ASP.NET Core 運行的特定平臺定位起來更為簡單:.NET Core 和 .NET 框架。</span><span class="sxs-lookup"><span data-stu-id="25f2e-205">Since the library only contains Tag Helpers, it's more straightforward to target the specific platforms on which ASP.NET Core runs: .NET Core and .NET Framework.</span></span> <span data-ttu-id="25f2e-206">標記説明器不能由其他符合 .NET 標準 2.0 的目標框架(如 Unity、UWP 和 Xamarin)使用。</span><span class="sxs-lookup"><span data-stu-id="25f2e-206">Tag Helpers can't be used by other .NET Standard 2.0-compliant target frameworks such as Unity, UWP, and Xamarin.</span></span>
* <span data-ttu-id="25f2e-207">使用 .NET Framework 中的 .NET Standard 2.0，會有一些在 .NET Framework 4.7.2 中已經解決的問題。</span><span class="sxs-lookup"><span data-stu-id="25f2e-207">Using .NET Standard 2.0 from .NET Framework has some issues that were addressed in .NET Framework 4.7.2.</span></span> <span data-ttu-id="25f2e-208">您可以通過定位 .NET 框架 4.6.1 來改善使用 .NET 框架 4.6.1 到 4.7.1 的消費者體驗。</span><span class="sxs-lookup"><span data-stu-id="25f2e-208">You can improve the experience for consumers using .NET Framework 4.6.1 through 4.7.1 by targeting .NET Framework 4.6.1.</span></span>

<span data-ttu-id="25f2e-209">如果您的庫需要調用特定於平臺的 API,請針對特定的 .NET 實現而不是 .NET 標準。</span><span class="sxs-lookup"><span data-stu-id="25f2e-209">If your library needs to call platform-specific APIs, target specific .NET implementations instead of .NET Standard.</span></span> <span data-ttu-id="25f2e-210">有關詳細資訊,請參閱[多目標](/dotnet/standard/library-guidance/cross-platform-targeting#multi-targeting)。</span><span class="sxs-lookup"><span data-stu-id="25f2e-210">For more information, see [Multi-targeting](/dotnet/standard/library-guidance/cross-platform-targeting#multi-targeting).</span></span>

## <a name="use-an-api-that-hasnt-changed"></a><span data-ttu-id="25f2e-211">使用未變更的 API</span><span class="sxs-lookup"><span data-stu-id="25f2e-211">Use an API that hasn't changed</span></span>

<span data-ttu-id="25f2e-212">設想一個方案,您將中間件庫從 .NET Core 2.2 升級到 3.0。</span><span class="sxs-lookup"><span data-stu-id="25f2e-212">Imagine a scenario in which you're upgrading a middleware library from .NET Core 2.2 to 3.0.</span></span> <span data-ttu-id="25f2e-213">庫中正在使用的ASP.NET核心中間件 API 在 ASP.NET 酷 2.2 和 3.0 之間沒有變化。</span><span class="sxs-lookup"><span data-stu-id="25f2e-213">The ASP.NET Core middleware APIs being used in the library haven't changed between ASP.NET Core 2.2 and 3.0.</span></span> <span data-ttu-id="25f2e-214">要繼續支援 .NET Core 3.0 中的中間件庫,請執行以下步驟:</span><span class="sxs-lookup"><span data-stu-id="25f2e-214">To continue supporting the middleware library in .NET Core 3.0, take the following steps:</span></span>

* <span data-ttu-id="25f2e-215">遵循[標準庫指南](/dotnet/standard/library-guidance/)。</span><span class="sxs-lookup"><span data-stu-id="25f2e-215">Follow the [standard library guidance](/dotnet/standard/library-guidance/).</span></span>
* <span data-ttu-id="25f2e-216">如果共用框架中不存在相應的程式集,則為每個 API 的 NuGet 包添加包引用。</span><span class="sxs-lookup"><span data-stu-id="25f2e-216">Add a package reference for each API's NuGet package if the corresponding assembly doesn't exist in the shared framework.</span></span>

## <a name="use-an-api-that-changed"></a><span data-ttu-id="25f2e-217">使用已變更的 API</span><span class="sxs-lookup"><span data-stu-id="25f2e-217">Use an API that changed</span></span>

<span data-ttu-id="25f2e-218">設想一個將庫從 .NET Core 2.2 升級到 .NET Core 3.0 的方案。</span><span class="sxs-lookup"><span data-stu-id="25f2e-218">Imagine a scenario in which you're upgrading a library from .NET Core 2.2 to .NET Core 3.0.</span></span> <span data-ttu-id="25f2e-219">函式庫中正在使用ASP.NET核心 API 在 ASP.NET Core 3.0[中發生重大變更](/dotnet/core/compatibility/breaking-changes)。</span><span class="sxs-lookup"><span data-stu-id="25f2e-219">An ASP.NET Core API being used in the library has a [breaking change](/dotnet/core/compatibility/breaking-changes) in ASP.NET Core 3.0.</span></span> <span data-ttu-id="25f2e-220">考慮是否可以重寫庫,以便在所有版本中不使用損壞的 API。</span><span class="sxs-lookup"><span data-stu-id="25f2e-220">Consider whether the library can be rewritten to not use the broken API in all versions.</span></span>

<span data-ttu-id="25f2e-221">如果可以重寫庫,請重寫庫,然後繼續使用包引用來定位較早的目標框架(例如 .NET 標準 2.0 或 .NET 框架 4.6.1)。</span><span class="sxs-lookup"><span data-stu-id="25f2e-221">If you can rewrite the library, do so and continue to target an earlier target framework (for example, .NET Standard 2.0 or .NET Framework 4.6.1) with package references.</span></span>

<span data-ttu-id="25f2e-222">如果無法重寫庫,請執行以下步驟:</span><span class="sxs-lookup"><span data-stu-id="25f2e-222">If you can't rewrite the library, take the following steps:</span></span>

* <span data-ttu-id="25f2e-223">添加 .NET 核心 3.0 的目標。</span><span class="sxs-lookup"><span data-stu-id="25f2e-223">Add a target for .NET Core 3.0.</span></span>
* <span data-ttu-id="25f2e-224">為`<FrameworkReference>`共用框架添加元素。</span><span class="sxs-lookup"><span data-stu-id="25f2e-224">Add a `<FrameworkReference>` element for the shared framework.</span></span>
* <span data-ttu-id="25f2e-225">使用帶有相應目標框架符號[#if预处理器指令](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)有條件地編譯代碼。</span><span class="sxs-lookup"><span data-stu-id="25f2e-225">Use the [#if preprocessor directive](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if) with the appropriate target framework symbol to conditionally compile code.</span></span>

<span data-ttu-id="25f2e-226">例如,默認情況下,從ASP.NET Core 3.0 起,HTTP 請求和回應流的同步讀取和寫入將禁用。</span><span class="sxs-lookup"><span data-stu-id="25f2e-226">For example, synchronous reads and writes on HTTP request and response streams are disabled by default as of ASP.NET Core 3.0.</span></span> <span data-ttu-id="25f2e-227">默認情況下,ASP.NET Core 2.2 支援同步行為。</span><span class="sxs-lookup"><span data-stu-id="25f2e-227">ASP.NET Core 2.2 supports the synchronous behavior by default.</span></span> <span data-ttu-id="25f2e-228">考慮一個中間件庫,其中應啟用同步讀取和寫入,其中發生I/O。</span><span class="sxs-lookup"><span data-stu-id="25f2e-228">Consider a middleware library in which synchronous reads and writes should be enabled where I/O is occurring.</span></span> <span data-ttu-id="25f2e-229">庫應包含代碼,以便在相應的預處理器指令中啟用同步功能。</span><span class="sxs-lookup"><span data-stu-id="25f2e-229">The library should enclose the code to enable synchronous features in the appropriate preprocessor directive.</span></span> <span data-ttu-id="25f2e-230">例如：</span><span class="sxs-lookup"><span data-stu-id="25f2e-230">For example:</span></span>

[!code-csharp[](target-aspnetcore/samples/middleware.cs?highlight=9-24)]

## <a name="use-an-api-introduced-in-30"></a><span data-ttu-id="25f2e-231">使用 3.0 中引入的 API</span><span class="sxs-lookup"><span data-stu-id="25f2e-231">Use an API introduced in 3.0</span></span>

<span data-ttu-id="25f2e-232">假設您要使用ASP.NET Core 3.0 中引入的ASP.NET核心 API。</span><span class="sxs-lookup"><span data-stu-id="25f2e-232">Imagine that you want to use an ASP.NET Core API that was introduced in ASP.NET Core 3.0.</span></span> <span data-ttu-id="25f2e-233">請考慮下列問題：</span><span class="sxs-lookup"><span data-stu-id="25f2e-233">Consider the following questions:</span></span>

1. <span data-ttu-id="25f2e-234">庫在功能上是否需要新的 API?</span><span class="sxs-lookup"><span data-stu-id="25f2e-234">Does the library functionally require the new API?</span></span>
1. <span data-ttu-id="25f2e-235">庫能否以不同的方式實現此功能?</span><span class="sxs-lookup"><span data-stu-id="25f2e-235">Can the library implement this feature in a different way?</span></span>

<span data-ttu-id="25f2e-236">如果庫在功能上需要 API,並且無法在級別下實現它:</span><span class="sxs-lookup"><span data-stu-id="25f2e-236">If the library functionally requires the API and there's no way to implement it down-level:</span></span>

* <span data-ttu-id="25f2e-237">僅目標 .NET 核心 3.x。</span><span class="sxs-lookup"><span data-stu-id="25f2e-237">Target .NET Core 3.x only.</span></span>
* <span data-ttu-id="25f2e-238">為`<FrameworkReference>`共用框架添加元素。</span><span class="sxs-lookup"><span data-stu-id="25f2e-238">Add a `<FrameworkReference>` element for the shared framework.</span></span>

<span data-ttu-id="25f2e-239">如果庫可以以不同的方式實現該功能:</span><span class="sxs-lookup"><span data-stu-id="25f2e-239">If the library can implement the feature in a different way:</span></span>

* <span data-ttu-id="25f2e-240">將 .NET Core 3.x 添加為目標框架。</span><span class="sxs-lookup"><span data-stu-id="25f2e-240">Add .NET Core 3.x as a target framework.</span></span>
* <span data-ttu-id="25f2e-241">為`<FrameworkReference>`共用框架添加元素。</span><span class="sxs-lookup"><span data-stu-id="25f2e-241">Add a `<FrameworkReference>` element for the shared framework.</span></span>
* <span data-ttu-id="25f2e-242">使用帶有相應目標框架符號[#if预处理器指令](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)有條件地編譯代碼。</span><span class="sxs-lookup"><span data-stu-id="25f2e-242">Use the [#if preprocessor directive](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if) with the appropriate target framework symbol to conditionally compile code.</span></span>

<span data-ttu-id="25f2e-243">例如,以下標記説明程式使用<xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>ASP.NET Core 3.0 中引入的介面。</span><span class="sxs-lookup"><span data-stu-id="25f2e-243">For example, the following Tag Helper uses the <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> interface introduced in ASP.NET Core 3.0.</span></span> <span data-ttu-id="25f2e-244">面向 .NET Core 3.0`NETCOREAPP3_0`的使用者執行 目標框架符號定義的代碼路徑。</span><span class="sxs-lookup"><span data-stu-id="25f2e-244">Consumers targeting .NET Core 3.0 execute the code path defined by the `NETCOREAPP3_0` target framework symbol.</span></span> <span data-ttu-id="25f2e-245">標記説明器的建構函數參數類型更改為<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment>.NET Core 2.1 和 .NET 框架 4.6.1 消費者。</span><span class="sxs-lookup"><span data-stu-id="25f2e-245">The Tag Helper's constructor parameter type changes to <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> for .NET Core 2.1 and .NET Framework 4.6.1 consumers.</span></span> <span data-ttu-id="25f2e-246">這一更改是必要的,因為ASP.NET Core `IHostingEnvironment` 3.0`IWebHostEnvironment`標記為過時,建議作為替換。</span><span class="sxs-lookup"><span data-stu-id="25f2e-246">This change was necessary because ASP.NET Core 3.0 marked `IHostingEnvironment` as obsolete and recommended `IWebHostEnvironment` as the replacement.</span></span>

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

<span data-ttu-id="25f2e-247">以下多目標項目檔案支援此標記説明程式方案:</span><span class="sxs-lookup"><span data-stu-id="25f2e-247">The following multi-targeted project file supports this Tag Helper scenario:</span></span>

[!code-xml[](target-aspnetcore/samples/multi-tfm/recommended-tag-helpers-library.csproj)]

## <a name="use-an-api-removed-from-the-shared-framework"></a><span data-ttu-id="25f2e-248">使用從共享框架中移除的 API</span><span class="sxs-lookup"><span data-stu-id="25f2e-248">Use an API removed from the shared framework</span></span>

<span data-ttu-id="25f2e-249">要使用從共用框架中刪除的ASP.NET核心程式集,添加相應的包引用。</span><span class="sxs-lookup"><span data-stu-id="25f2e-249">To use an ASP.NET Core assembly that was removed from the shared framework, add the appropriate package reference.</span></span> <span data-ttu-id="25f2e-250">有關從 ASP.NET Core 3.0 中的共用框架中移除的套件的清單,請參閱[刪除過時的包參考](xref:migration/22-to-30#remove-obsolete-package-references)。</span><span class="sxs-lookup"><span data-stu-id="25f2e-250">For a list of packages removed from the shared framework in ASP.NET Core 3.0, see [Remove obsolete package references](xref:migration/22-to-30#remove-obsolete-package-references).</span></span>

<span data-ttu-id="25f2e-251">例如,要新增 Web API 用戶端:</span><span class="sxs-lookup"><span data-stu-id="25f2e-251">For example, to add the web API client:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="25f2e-252">其他資源</span><span class="sxs-lookup"><span data-stu-id="25f2e-252">Additional resources</span></span>

* <xref:razor-pages/ui-class>
* <xref:blazor/class-libraries>
* [<span data-ttu-id="25f2e-253">.NET 實作支援</span><span class="sxs-lookup"><span data-stu-id="25f2e-253">.NET implementation support</span></span>](/dotnet/standard/net-standard#net-implementation-support)
* [<span data-ttu-id="25f2e-254">.NET 支援原則</span><span class="sxs-lookup"><span data-stu-id="25f2e-254">.NET support policies</span></span>](https://dotnet.microsoft.com/platform/support/policy)
