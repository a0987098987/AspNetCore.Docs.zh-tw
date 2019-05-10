---
title: ASP.NET Core 中的 razor Pages 授權慣例
author: guardrex
description: 了解如何控制存取，授權使用者，並允許匿名使用者存取頁面的資料夾慣例的頁面。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: ff061f96f30cd893b903403de760a172c924cf06
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895415"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="ffa01-103">ASP.NET Core 中的 razor Pages 授權慣例</span><span class="sxs-lookup"><span data-stu-id="ffa01-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="ffa01-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ffa01-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ffa01-105">控制 Razor Pages 應用程式中的存取的一個方式是在啟動時使用授權慣例。</span><span class="sxs-lookup"><span data-stu-id="ffa01-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="ffa01-106">這些慣例可讓您授權的使用者，並允許匿名使用者存取個別頁面的資料夾。</span><span class="sxs-lookup"><span data-stu-id="ffa01-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="ffa01-107">自動在本主題中所述的慣例套用[授權篩選條件](xref:mvc/controllers/filters#authorization-filters)來控制存取。</span><span class="sxs-lookup"><span data-stu-id="ffa01-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="ffa01-108">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ffa01-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ffa01-109">範例應用程式會使用[沒有 ASP.NET Core 身分識別的 cookie 驗證](xref:security/authentication/cookie)。</span><span class="sxs-lookup"><span data-stu-id="ffa01-109">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="ffa01-110">本主題所示的範例與概念同樣適用於使用 ASP.NET Core 身分識別的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ffa01-110">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="ffa01-111">若要使用 ASP.NET Core 身分識別，請依照下列中的指導方針<xref:security/authentication/identity>。</span><span class="sxs-lookup"><span data-stu-id="ffa01-111">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="ffa01-112">需要授權，才能存取頁面</span><span class="sxs-lookup"><span data-stu-id="ffa01-112">Require authorization to access a page</span></span>

<span data-ttu-id="ffa01-113">使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*>透過慣例<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>以新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>至位於指定路徑的頁面：</span><span class="sxs-lookup"><span data-stu-id="ffa01-113">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="ffa01-114">指定的路徑是檢視引擎路徑，也就是 Razor 頁面根相對路徑，而不需要擴充功能和包含只正斜線。</span><span class="sxs-lookup"><span data-stu-id="ffa01-114">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="ffa01-115">若要指定[授權原則](xref:security/authorization/policies)，使用[AuthorizePage 多載](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="ffa01-115">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="ffa01-116"><xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>可以套用至頁面模型類別`[Authorize]`篩選條件屬性。</span><span class="sxs-lookup"><span data-stu-id="ffa01-116">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="ffa01-117">如需詳細資訊，請參閱 <<c0> [ 授權篩選條件屬性](xref:razor-pages/filter#authorize-filter-attribute)。</span><span class="sxs-lookup"><span data-stu-id="ffa01-117">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="ffa01-118">需要授權，才能存取頁面的資料夾</span><span class="sxs-lookup"><span data-stu-id="ffa01-118">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="ffa01-119">使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*>透過慣例<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>以新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>所有指定的路徑在資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="ffa01-119">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="ffa01-120">指定的路徑是檢視引擎路徑，也就是 Razor 頁面的根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="ffa01-120">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="ffa01-121">若要指定[授權原則](xref:security/authorization/policies)，使用[AuthorizeFolder 多載](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="ffa01-121">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="ffa01-122">需要授權，才能存取 [區域] 頁面</span><span class="sxs-lookup"><span data-stu-id="ffa01-122">Require authorization to access an area page</span></span>

<span data-ttu-id="ffa01-123">使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*>透過慣例<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>以新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>至位於指定路徑的 [區域] 頁面：</span><span class="sxs-lookup"><span data-stu-id="ffa01-123">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="ffa01-124">頁面名稱是檔案的指定的區域不含副檔名，相對於頁面根目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="ffa01-124">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="ffa01-125">例如，檔案的頁面名稱*Areas/Identity/Pages/Manage/Accounts.cshtml*是 */管理/帳戶*。</span><span class="sxs-lookup"><span data-stu-id="ffa01-125">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="ffa01-126">若要指定[授權原則](xref:security/authorization/policies)，使用[AuthorizeAreaPage 多載](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="ffa01-126">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="ffa01-127">需要授權，才能存取區域的資料夾</span><span class="sxs-lookup"><span data-stu-id="ffa01-127">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="ffa01-128">使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*>透過慣例<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>以新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>所有位於指定路徑的資料夾中的領域：</span><span class="sxs-lookup"><span data-stu-id="ffa01-128">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="ffa01-129">資料夾路徑是相對於指定的區域頁面根目錄之資料夾的路徑。</span><span class="sxs-lookup"><span data-stu-id="ffa01-129">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="ffa01-130">例如，底下的檔案的資料夾路徑*領域/身分識別/網頁/管理/* 是 */管理*。</span><span class="sxs-lookup"><span data-stu-id="ffa01-130">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="ffa01-131">若要指定[授權原則](xref:security/authorization/policies)，使用[AuthorizeAreaFolder 多載](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="ffa01-131">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="ffa01-132">允許匿名存取 頁面</span><span class="sxs-lookup"><span data-stu-id="ffa01-132">Allow anonymous access to a page</span></span>

<span data-ttu-id="ffa01-133">使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*>透過慣例<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>以新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>至位於指定路徑的頁面：</span><span class="sxs-lookup"><span data-stu-id="ffa01-133">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="ffa01-134">指定的路徑是檢視引擎路徑，也就是 Razor 頁面根相對路徑，而不需要擴充功能和包含只正斜線。</span><span class="sxs-lookup"><span data-stu-id="ffa01-134">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="ffa01-135">允許匿名存取頁面的資料夾</span><span class="sxs-lookup"><span data-stu-id="ffa01-135">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="ffa01-136">使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*>透過慣例<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>以新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>所有指定的路徑在資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="ffa01-136">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="ffa01-137">指定的路徑是檢視引擎路徑，也就是 Razor 頁面的根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="ffa01-137">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="ffa01-138">請注意在結合授權及匿名存取</span><span class="sxs-lookup"><span data-stu-id="ffa01-138">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="ffa01-139">指定有效的頁面需要授權的資料夾並指定該資料夾內的頁面允許匿名存取：</span><span class="sxs-lookup"><span data-stu-id="ffa01-139">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="ffa01-140">相反地，不過，不是有效的。</span><span class="sxs-lookup"><span data-stu-id="ffa01-140">The reverse, however, isn't valid.</span></span> <span data-ttu-id="ffa01-141">您無法宣告的匿名存取頁面的資料夾，然後指定 需要授權該資料夾內的頁面：</span><span class="sxs-lookup"><span data-stu-id="ffa01-141">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="ffa01-142">在 [私人] 頁面上的要求授權失敗。</span><span class="sxs-lookup"><span data-stu-id="ffa01-142">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="ffa01-143">當同時<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>並<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>套用至網頁，<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>會優先使用並控制存取。</span><span class="sxs-lookup"><span data-stu-id="ffa01-143">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ffa01-144">其他資源</span><span class="sxs-lookup"><span data-stu-id="ffa01-144">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
