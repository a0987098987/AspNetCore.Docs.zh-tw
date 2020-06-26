---
title: RazorASP.NET Core 中的頁面授權慣例
author: rick-anderson
description: 瞭解如何以授權使用者的慣例控制頁面的存取權，並允許匿名使用者存取頁面的頁面或資料夾。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 0492dd3d9b2aee7e844e944bea96259c3ddf18d0
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408717"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Razor<span data-ttu-id="be983-103">ASP.NET Core 中的頁面授權慣例</span><span class="sxs-lookup"><span data-stu-id="be983-103"> Pages authorization conventions in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="be983-104">控制頁面應用程式存取權的其中一種方式 Razor 是在啟動時使用授權慣例。</span><span class="sxs-lookup"><span data-stu-id="be983-104">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="be983-105">這些慣例可讓您授權使用者，並允許匿名使用者存取頁面的個別頁面或資料夾。</span><span class="sxs-lookup"><span data-stu-id="be983-105">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="be983-106">本主題中所述的慣例會自動套用[授權篩選](xref:mvc/controllers/filters#authorization-filters)來控制存取。</span><span class="sxs-lookup"><span data-stu-id="be983-106">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="be983-107">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="be983-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="be983-108">範例應用程式[會使用 cookie 驗證， Identity 而不 ASP.NET Core ](xref:security/authentication/cookie)。</span><span class="sxs-lookup"><span data-stu-id="be983-108">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="be983-109">本主題所顯示的概念和範例同樣適用于使用 ASP.NET Core 的應用程式 Identity 。</span><span class="sxs-lookup"><span data-stu-id="be983-109">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="be983-110">若要使用 ASP.NET Core Identity ，請遵循中的指導方針 <xref:security/authentication/identity> 。</span><span class="sxs-lookup"><span data-stu-id="be983-110">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="be983-111">需要授權才能存取頁面</span><span class="sxs-lookup"><span data-stu-id="be983-111">Require authorization to access a page</span></span>

<span data-ttu-id="be983-112">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至頁面的指定路徑：</span><span class="sxs-lookup"><span data-stu-id="be983-112">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="be983-113">指定的路徑為 View Engine path，這是 Razor 頁面根相對路徑，沒有副檔名，而且只包含正斜線。</span><span class="sxs-lookup"><span data-stu-id="be983-113">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="be983-114">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*)多載：</span><span class="sxs-lookup"><span data-stu-id="be983-114">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="be983-115"><xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>可以套用至具有 filter 屬性的頁面模型類別 `[Authorize]` 。</span><span class="sxs-lookup"><span data-stu-id="be983-115">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="be983-116">如需詳細資訊，請參閱[授權篩選屬性](xref:razor-pages/filter#authorize-filter-attribute)。</span><span class="sxs-lookup"><span data-stu-id="be983-116">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="be983-117">需要授權才能存取頁面的資料夾</span><span class="sxs-lookup"><span data-stu-id="be983-117">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="be983-118">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至資料夾中指定路徑的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="be983-118">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="be983-119">指定的路徑是 View Engine path，這是 Razor 頁面根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="be983-119">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="be983-120">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*)多載：</span><span class="sxs-lookup"><span data-stu-id="be983-120">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="be983-121">需要授權才能存取區域頁面</span><span class="sxs-lookup"><span data-stu-id="be983-121">Require authorization to access an area page</span></span>

<span data-ttu-id="be983-122">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至位於指定路徑的 [區域] 頁面：</span><span class="sxs-lookup"><span data-stu-id="be983-122">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="be983-123">[頁面名稱] 是檔案的路徑，沒有副檔名相對於指定區域的頁面根目錄。</span><span class="sxs-lookup"><span data-stu-id="be983-123">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="be983-124">例如，檔案*區域/ Identity /Pages/Manage/Accounts.cshtml*的頁面名稱是 */Manage/Accounts*。</span><span class="sxs-lookup"><span data-stu-id="be983-124">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="be983-125">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*)多載：</span><span class="sxs-lookup"><span data-stu-id="be983-125">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="be983-126">需要授權才能存取區域的資料夾</span><span class="sxs-lookup"><span data-stu-id="be983-126">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="be983-127">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至資料夾中指定路徑的所有區域：</span><span class="sxs-lookup"><span data-stu-id="be983-127">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="be983-128">資料夾路徑是相對於指定區域的頁面根目錄的資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="be983-128">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="be983-129">例如，*區域/ Identity /Pages/Manage/* 下檔案的資料夾路徑為 */Manage*。</span><span class="sxs-lookup"><span data-stu-id="be983-129">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="be983-130">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*)多載：</span><span class="sxs-lookup"><span data-stu-id="be983-130">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="be983-131">允許匿名存取頁面</span><span class="sxs-lookup"><span data-stu-id="be983-131">Allow anonymous access to a page</span></span>

<span data-ttu-id="be983-132">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 至位於指定路徑的頁面：</span><span class="sxs-lookup"><span data-stu-id="be983-132">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="be983-133">指定的路徑為 View Engine path，這是 Razor 頁面根相對路徑，沒有副檔名，而且只包含正斜線。</span><span class="sxs-lookup"><span data-stu-id="be983-133">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="be983-134">允許匿名存取頁面資料夾</span><span class="sxs-lookup"><span data-stu-id="be983-134">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="be983-135">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 至資料夾中指定路徑的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="be983-135">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="be983-136">指定的路徑是 View Engine path，這是 Razor 頁面根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="be983-136">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="be983-137">結合已授權和匿名存取的注意事項</span><span class="sxs-lookup"><span data-stu-id="be983-137">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="be983-138">指定頁面的資料夾需要授權，然後指定該資料夾內的頁面允許匿名存取，是有效的：</span><span class="sxs-lookup"><span data-stu-id="be983-138">It's valid to specify that a folder of pages requires authorization and then specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="be983-139">不過，相反的，這是不正確。</span><span class="sxs-lookup"><span data-stu-id="be983-139">The reverse, however, isn't valid.</span></span> <span data-ttu-id="be983-140">您無法針對匿名存取宣告頁面的資料夾，然後在該資料夾內指定需要授權的頁面：</span><span class="sxs-lookup"><span data-stu-id="be983-140">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="be983-141">在私用頁面上要求授權失敗。</span><span class="sxs-lookup"><span data-stu-id="be983-141">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="be983-142">當 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 和都套用 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至頁面時，會優先使用 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 並控制存取。</span><span class="sxs-lookup"><span data-stu-id="be983-142">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be983-143">其他資源</span><span class="sxs-lookup"><span data-stu-id="be983-143">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="be983-144">控制頁面應用程式存取權的其中一種方式 Razor 是在啟動時使用授權慣例。</span><span class="sxs-lookup"><span data-stu-id="be983-144">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="be983-145">這些慣例可讓您授權使用者，並允許匿名使用者存取頁面的個別頁面或資料夾。</span><span class="sxs-lookup"><span data-stu-id="be983-145">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="be983-146">本主題中所述的慣例會自動套用[授權篩選](xref:mvc/controllers/filters#authorization-filters)來控制存取。</span><span class="sxs-lookup"><span data-stu-id="be983-146">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="be983-147">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="be983-147">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="be983-148">範例應用程式[會使用 cookie 驗證， Identity 而不 ASP.NET Core ](xref:security/authentication/cookie)。</span><span class="sxs-lookup"><span data-stu-id="be983-148">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="be983-149">本主題所顯示的概念和範例同樣適用于使用 ASP.NET Core 的應用程式 Identity 。</span><span class="sxs-lookup"><span data-stu-id="be983-149">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="be983-150">若要使用 ASP.NET Core Identity ，請遵循中的指導方針 <xref:security/authentication/identity> 。</span><span class="sxs-lookup"><span data-stu-id="be983-150">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="be983-151">需要授權才能存取頁面</span><span class="sxs-lookup"><span data-stu-id="be983-151">Require authorization to access a page</span></span>

<span data-ttu-id="be983-152">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至頁面的指定路徑：</span><span class="sxs-lookup"><span data-stu-id="be983-152">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="be983-153">指定的路徑為 View Engine path，這是 Razor 頁面根相對路徑，沒有副檔名，而且只包含正斜線。</span><span class="sxs-lookup"><span data-stu-id="be983-153">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="be983-154">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*)多載：</span><span class="sxs-lookup"><span data-stu-id="be983-154">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="be983-155"><xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>可以套用至具有 filter 屬性的頁面模型類別 `[Authorize]` 。</span><span class="sxs-lookup"><span data-stu-id="be983-155">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="be983-156">如需詳細資訊，請參閱[授權篩選屬性](xref:razor-pages/filter#authorize-filter-attribute)。</span><span class="sxs-lookup"><span data-stu-id="be983-156">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="be983-157">需要授權才能存取頁面的資料夾</span><span class="sxs-lookup"><span data-stu-id="be983-157">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="be983-158">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至資料夾中指定路徑的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="be983-158">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="be983-159">指定的路徑是 View Engine path，這是 Razor 頁面根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="be983-159">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="be983-160">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*)多載：</span><span class="sxs-lookup"><span data-stu-id="be983-160">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="be983-161">需要授權才能存取區域頁面</span><span class="sxs-lookup"><span data-stu-id="be983-161">Require authorization to access an area page</span></span>

<span data-ttu-id="be983-162">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至位於指定路徑的 [區域] 頁面：</span><span class="sxs-lookup"><span data-stu-id="be983-162">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="be983-163">[頁面名稱] 是檔案的路徑，沒有副檔名相對於指定區域的頁面根目錄。</span><span class="sxs-lookup"><span data-stu-id="be983-163">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="be983-164">例如，檔案*區域/ Identity /Pages/Manage/Accounts.cshtml*的頁面名稱是 */Manage/Accounts*。</span><span class="sxs-lookup"><span data-stu-id="be983-164">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="be983-165">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*)多載：</span><span class="sxs-lookup"><span data-stu-id="be983-165">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="be983-166">需要授權才能存取區域的資料夾</span><span class="sxs-lookup"><span data-stu-id="be983-166">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="be983-167">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至資料夾中指定路徑的所有區域：</span><span class="sxs-lookup"><span data-stu-id="be983-167">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="be983-168">資料夾路徑是相對於指定區域的頁面根目錄的資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="be983-168">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="be983-169">例如，*區域/ Identity /Pages/Manage/* 下檔案的資料夾路徑為 */Manage*。</span><span class="sxs-lookup"><span data-stu-id="be983-169">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="be983-170">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*)多載：</span><span class="sxs-lookup"><span data-stu-id="be983-170">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="be983-171">允許匿名存取頁面</span><span class="sxs-lookup"><span data-stu-id="be983-171">Allow anonymous access to a page</span></span>

<span data-ttu-id="be983-172">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 至位於指定路徑的頁面：</span><span class="sxs-lookup"><span data-stu-id="be983-172">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="be983-173">指定的路徑為 View Engine path，這是 Razor 頁面根相對路徑，沒有副檔名，而且只包含正斜線。</span><span class="sxs-lookup"><span data-stu-id="be983-173">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="be983-174">允許匿名存取頁面資料夾</span><span class="sxs-lookup"><span data-stu-id="be983-174">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="be983-175">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 至資料夾中指定路徑的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="be983-175">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="be983-176">指定的路徑是 View Engine path，這是 Razor 頁面根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="be983-176">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="be983-177">結合已授權和匿名存取的注意事項</span><span class="sxs-lookup"><span data-stu-id="be983-177">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="be983-178">有效的方式是指定需要授權的頁面資料夾，並指定該資料夾內的頁面允許匿名存取：</span><span class="sxs-lookup"><span data-stu-id="be983-178">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="be983-179">不過，相反的，這是不正確。</span><span class="sxs-lookup"><span data-stu-id="be983-179">The reverse, however, isn't valid.</span></span> <span data-ttu-id="be983-180">您無法針對匿名存取宣告頁面的資料夾，然後在該資料夾內指定需要授權的頁面：</span><span class="sxs-lookup"><span data-stu-id="be983-180">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="be983-181">在私用頁面上要求授權失敗。</span><span class="sxs-lookup"><span data-stu-id="be983-181">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="be983-182">當 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 和都套用 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至頁面時，會優先使用 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 並控制存取。</span><span class="sxs-lookup"><span data-stu-id="be983-182">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be983-183">其他資源</span><span class="sxs-lookup"><span data-stu-id="be983-183">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
