---
title: ASP.NET Core 中的 Razor Pages 授權慣例
author: guardrex
description: 瞭解如何以授權使用者的慣例控制頁面的存取權, 並允許匿名使用者存取頁面的頁面或資料夾。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: e0102ff64921a83f0330acb6f5d9bfd90f64ca7a
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994039"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="542c9-103">ASP.NET Core 中的 Razor Pages 授權慣例</span><span class="sxs-lookup"><span data-stu-id="542c9-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="542c9-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="542c9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="542c9-105">在您的 Razor Pages 應用程式中控制存取的一種方式是在啟動時使用授權慣例。</span><span class="sxs-lookup"><span data-stu-id="542c9-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="542c9-106">這些慣例可讓您授權使用者, 並允許匿名使用者存取頁面的個別頁面或資料夾。</span><span class="sxs-lookup"><span data-stu-id="542c9-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="542c9-107">本主題中所述的慣例會自動套用[授權篩選](xref:mvc/controllers/filters#authorization-filters)來控制存取。</span><span class="sxs-lookup"><span data-stu-id="542c9-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="542c9-108">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="542c9-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="542c9-109">範例應用程式[會使用 cookie 驗證, 而不 ASP.NET Core 身分識別](xref:security/authentication/cookie)。</span><span class="sxs-lookup"><span data-stu-id="542c9-109">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="542c9-110">本主題所顯示的概念和範例同樣適用于使用 ASP.NET Core 身分識別的應用程式。</span><span class="sxs-lookup"><span data-stu-id="542c9-110">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="542c9-111">若要使用 ASP.NET Core 身分識別, 請遵循<xref:security/authentication/identity>中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="542c9-111">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="542c9-112">需要授權才能存取頁面</span><span class="sxs-lookup"><span data-stu-id="542c9-112">Require authorization to access a page</span></span>

<span data-ttu-id="542c9-113">透過使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>慣例, 將新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>至頁面的指定路徑:</span><span class="sxs-lookup"><span data-stu-id="542c9-113">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="542c9-114">指定的路徑為 View Engine path, 這是不含副檔名且只包含正斜線的 Razor Pages 根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="542c9-114">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="542c9-115">若要指定[授權原則](xref:security/authorization/policies), 請使用[AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*)多載:</span><span class="sxs-lookup"><span data-stu-id="542c9-115">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="542c9-116">可以套用至`[Authorize]`具有 filter 屬性的頁面模型類別。 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter></span><span class="sxs-lookup"><span data-stu-id="542c9-116">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="542c9-117">如需詳細資訊, 請參閱[授權篩選屬性](xref:razor-pages/filter#authorize-filter-attribute)。</span><span class="sxs-lookup"><span data-stu-id="542c9-117">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="542c9-118">需要授權才能存取頁面的資料夾</span><span class="sxs-lookup"><span data-stu-id="542c9-118">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="542c9-119">透過使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>慣例, 將新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>至資料夾中指定路徑的所有頁面:</span><span class="sxs-lookup"><span data-stu-id="542c9-119">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="542c9-120">指定的路徑為 View Engine path, 這是 Razor Pages 根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="542c9-120">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="542c9-121">若要指定[授權原則](xref:security/authorization/policies), 請使用[AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*)多載:</span><span class="sxs-lookup"><span data-stu-id="542c9-121">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="542c9-122">需要授權才能存取區域頁面</span><span class="sxs-lookup"><span data-stu-id="542c9-122">Require authorization to access an area page</span></span>

<span data-ttu-id="542c9-123">透過使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>慣例, 將新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>至位於指定路徑的 [區域] 頁面:</span><span class="sxs-lookup"><span data-stu-id="542c9-123">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="542c9-124">[頁面名稱] 是檔案的路徑, 沒有副檔名相對於指定區域的頁面根目錄。</span><span class="sxs-lookup"><span data-stu-id="542c9-124">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="542c9-125">例如, 檔案*區域/身分識別/頁面/管理/帳戶. cshtml*的頁面名稱是 */Manage/Accounts*。</span><span class="sxs-lookup"><span data-stu-id="542c9-125">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="542c9-126">若要指定[授權原則](xref:security/authorization/policies), 請使用[AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*)多載:</span><span class="sxs-lookup"><span data-stu-id="542c9-126">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="542c9-127">需要授權才能存取區域的資料夾</span><span class="sxs-lookup"><span data-stu-id="542c9-127">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="542c9-128">透過使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>慣例, 將新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>至資料夾中指定路徑的所有區域:</span><span class="sxs-lookup"><span data-stu-id="542c9-128">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="542c9-129">資料夾路徑是相對於指定區域的頁面根目錄的資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="542c9-129">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="542c9-130">例如, [*區域/身分識別/頁面/管理/* ] 底下的檔案資料夾路徑是 */Manage*。</span><span class="sxs-lookup"><span data-stu-id="542c9-130">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="542c9-131">若要指定[授權原則](xref:security/authorization/policies), 請使用[AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*)多載:</span><span class="sxs-lookup"><span data-stu-id="542c9-131">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="542c9-132">允許匿名存取頁面</span><span class="sxs-lookup"><span data-stu-id="542c9-132">Allow anonymous access to a page</span></span>

<span data-ttu-id="542c9-133">透過使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>慣例, 將新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>至位於指定路徑的頁面:</span><span class="sxs-lookup"><span data-stu-id="542c9-133">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="542c9-134">指定的路徑為 View Engine path, 這是不含副檔名且只包含正斜線的 Razor Pages 根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="542c9-134">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="542c9-135">允許匿名存取頁面資料夾</span><span class="sxs-lookup"><span data-stu-id="542c9-135">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="542c9-136">透過使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>慣例, 將新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>至資料夾中指定路徑的所有頁面:</span><span class="sxs-lookup"><span data-stu-id="542c9-136">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="542c9-137">指定的路徑為 View Engine path, 這是 Razor Pages 根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="542c9-137">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="542c9-138">結合已授權和匿名存取的注意事項</span><span class="sxs-lookup"><span data-stu-id="542c9-138">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="542c9-139">有效的方式是指定需要授權的頁面資料夾, 並指定該資料夾內的頁面允許匿名存取:</span><span class="sxs-lookup"><span data-stu-id="542c9-139">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="542c9-140">不過, 相反的, 這是不正確。</span><span class="sxs-lookup"><span data-stu-id="542c9-140">The reverse, however, isn't valid.</span></span> <span data-ttu-id="542c9-141">您無法針對匿名存取宣告頁面的資料夾, 然後在該資料夾內指定需要授權的頁面:</span><span class="sxs-lookup"><span data-stu-id="542c9-141">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="542c9-142">在私用頁面上要求授權失敗。</span><span class="sxs-lookup"><span data-stu-id="542c9-142">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="542c9-143"><xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>當和<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>都套用<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>至頁面時, 會優先使用並控制存取。</span><span class="sxs-lookup"><span data-stu-id="542c9-143">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="542c9-144">其他資源</span><span class="sxs-lookup"><span data-stu-id="542c9-144">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="542c9-145">在您的 Razor Pages 應用程式中控制存取的一種方式是在啟動時使用授權慣例。</span><span class="sxs-lookup"><span data-stu-id="542c9-145">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="542c9-146">這些慣例可讓您授權使用者, 並允許匿名使用者存取頁面的個別頁面或資料夾。</span><span class="sxs-lookup"><span data-stu-id="542c9-146">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="542c9-147">本主題中所述的慣例會自動套用[授權篩選](xref:mvc/controllers/filters#authorization-filters)來控制存取。</span><span class="sxs-lookup"><span data-stu-id="542c9-147">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="542c9-148">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="542c9-148">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="542c9-149">範例應用程式[會使用 cookie 驗證, 而不 ASP.NET Core 身分識別](xref:security/authentication/cookie)。</span><span class="sxs-lookup"><span data-stu-id="542c9-149">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="542c9-150">本主題所顯示的概念和範例同樣適用于使用 ASP.NET Core 身分識別的應用程式。</span><span class="sxs-lookup"><span data-stu-id="542c9-150">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="542c9-151">若要使用 ASP.NET Core 身分識別, 請遵循<xref:security/authentication/identity>中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="542c9-151">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="542c9-152">需要授權才能存取頁面</span><span class="sxs-lookup"><span data-stu-id="542c9-152">Require authorization to access a page</span></span>

<span data-ttu-id="542c9-153">透過使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>慣例, 將新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>至頁面的指定路徑:</span><span class="sxs-lookup"><span data-stu-id="542c9-153">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="542c9-154">指定的路徑為 View Engine path, 這是不含副檔名且只包含正斜線的 Razor Pages 根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="542c9-154">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="542c9-155">若要指定[授權原則](xref:security/authorization/policies), 請使用[AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*)多載:</span><span class="sxs-lookup"><span data-stu-id="542c9-155">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="542c9-156">可以套用至`[Authorize]`具有 filter 屬性的頁面模型類別。 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter></span><span class="sxs-lookup"><span data-stu-id="542c9-156">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="542c9-157">如需詳細資訊, 請參閱[授權篩選屬性](xref:razor-pages/filter#authorize-filter-attribute)。</span><span class="sxs-lookup"><span data-stu-id="542c9-157">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="542c9-158">需要授權才能存取頁面的資料夾</span><span class="sxs-lookup"><span data-stu-id="542c9-158">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="542c9-159">透過使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>慣例, 將新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>至資料夾中指定路徑的所有頁面:</span><span class="sxs-lookup"><span data-stu-id="542c9-159">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="542c9-160">指定的路徑為 View Engine path, 這是 Razor Pages 根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="542c9-160">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="542c9-161">若要指定[授權原則](xref:security/authorization/policies), 請使用[AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*)多載:</span><span class="sxs-lookup"><span data-stu-id="542c9-161">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="542c9-162">需要授權才能存取區域頁面</span><span class="sxs-lookup"><span data-stu-id="542c9-162">Require authorization to access an area page</span></span>

<span data-ttu-id="542c9-163">透過使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>慣例, 將新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>至位於指定路徑的 [區域] 頁面:</span><span class="sxs-lookup"><span data-stu-id="542c9-163">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="542c9-164">[頁面名稱] 是檔案的路徑, 沒有副檔名相對於指定區域的頁面根目錄。</span><span class="sxs-lookup"><span data-stu-id="542c9-164">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="542c9-165">例如, 檔案*區域/身分識別/頁面/管理/帳戶. cshtml*的頁面名稱是 */Manage/Accounts*。</span><span class="sxs-lookup"><span data-stu-id="542c9-165">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="542c9-166">若要指定[授權原則](xref:security/authorization/policies), 請使用[AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*)多載:</span><span class="sxs-lookup"><span data-stu-id="542c9-166">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="542c9-167">需要授權才能存取區域的資料夾</span><span class="sxs-lookup"><span data-stu-id="542c9-167">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="542c9-168">透過使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>慣例, 將新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>至資料夾中指定路徑的所有區域:</span><span class="sxs-lookup"><span data-stu-id="542c9-168">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="542c9-169">資料夾路徑是相對於指定區域的頁面根目錄的資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="542c9-169">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="542c9-170">例如, [*區域/身分識別/頁面/管理/* ] 底下的檔案資料夾路徑是 */Manage*。</span><span class="sxs-lookup"><span data-stu-id="542c9-170">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="542c9-171">若要指定[授權原則](xref:security/authorization/policies), 請使用[AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*)多載:</span><span class="sxs-lookup"><span data-stu-id="542c9-171">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="542c9-172">允許匿名存取頁面</span><span class="sxs-lookup"><span data-stu-id="542c9-172">Allow anonymous access to a page</span></span>

<span data-ttu-id="542c9-173">透過使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>慣例, 將新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>至位於指定路徑的頁面:</span><span class="sxs-lookup"><span data-stu-id="542c9-173">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="542c9-174">指定的路徑為 View Engine path, 這是不含副檔名且只包含正斜線的 Razor Pages 根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="542c9-174">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="542c9-175">允許匿名存取頁面資料夾</span><span class="sxs-lookup"><span data-stu-id="542c9-175">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="542c9-176">透過使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>慣例, 將新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>至資料夾中指定路徑的所有頁面:</span><span class="sxs-lookup"><span data-stu-id="542c9-176">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="542c9-177">指定的路徑為 View Engine path, 這是 Razor Pages 根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="542c9-177">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="542c9-178">結合已授權和匿名存取的注意事項</span><span class="sxs-lookup"><span data-stu-id="542c9-178">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="542c9-179">有效的方式是指定需要授權的頁面資料夾, 並指定該資料夾內的頁面允許匿名存取:</span><span class="sxs-lookup"><span data-stu-id="542c9-179">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="542c9-180">不過, 相反的, 這是不正確。</span><span class="sxs-lookup"><span data-stu-id="542c9-180">The reverse, however, isn't valid.</span></span> <span data-ttu-id="542c9-181">您無法針對匿名存取宣告頁面的資料夾, 然後在該資料夾內指定需要授權的頁面:</span><span class="sxs-lookup"><span data-stu-id="542c9-181">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="542c9-182">在私用頁面上要求授權失敗。</span><span class="sxs-lookup"><span data-stu-id="542c9-182">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="542c9-183"><xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>當和<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>都套用<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>至頁面時, 會優先使用並控制存取。</span><span class="sxs-lookup"><span data-stu-id="542c9-183">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="542c9-184">其他資源</span><span class="sxs-lookup"><span data-stu-id="542c9-184">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
