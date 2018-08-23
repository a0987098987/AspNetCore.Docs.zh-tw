---
title: ASP.NET Core 中的 razor Pages 授權慣例
author: guardrex
description: 了解如何控制存取，授權使用者，並允許匿名使用者存取頁面的資料夾慣例的頁面。
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: d3ecb41765da912df68aeb829350d27e4d087e3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832645"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="7f307-103">ASP.NET Core 中的 razor Pages 授權慣例</span><span class="sxs-lookup"><span data-stu-id="7f307-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="7f307-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7f307-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7f307-105">控制 Razor Pages 應用程式中的存取的一個方式是在啟動時使用授權慣例。</span><span class="sxs-lookup"><span data-stu-id="7f307-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="7f307-106">這些慣例可讓您授權的使用者，並允許匿名使用者存取個別頁面的資料夾。</span><span class="sxs-lookup"><span data-stu-id="7f307-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="7f307-107">自動在本主題中所述的慣例套用[授權篩選條件](xref:mvc/controllers/filters#authorization-filters)來控制存取。</span><span class="sxs-lookup"><span data-stu-id="7f307-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="7f307-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7f307-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7f307-109">範例應用程式會使用[沒有 ASP.NET Core 身分識別的 Cookie 驗證](xref:security/authentication/cookie)。</span><span class="sxs-lookup"><span data-stu-id="7f307-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="7f307-110">假設使用者林麗莉 Rodriguez 的使用者帳戶是硬式編碼到應用程式。</span><span class="sxs-lookup"><span data-stu-id="7f307-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="7f307-111">使用電子郵件使用者名稱 」maria.rodriguez@contoso.com」 和任何登入使用者的密碼。</span><span class="sxs-lookup"><span data-stu-id="7f307-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="7f307-112">使用者通過驗證`AuthenticateUser`方法中的*Pages/Account/Login.cshtml.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="7f307-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="7f307-113">在真實世界範例中，您會驗證使用者，對資料庫。</span><span class="sxs-lookup"><span data-stu-id="7f307-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="7f307-114">若要使用 ASP.NET Core 身分識別，請依照下列中的指導方針[ASP.NET core 身分識別簡介](xref:security/authentication/identity)主題。</span><span class="sxs-lookup"><span data-stu-id="7f307-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="7f307-115">本主題所示的範例與概念同樣適用於使用 ASP.NET Core 身分識別的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7f307-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="7f307-116">需要授權，才能存取頁面</span><span class="sxs-lookup"><span data-stu-id="7f307-116">Require authorization to access a page</span></span>

<span data-ttu-id="7f307-117">使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)加入[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)至位於指定路徑的頁面：</span><span class="sxs-lookup"><span data-stu-id="7f307-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="7f307-118">指定的路徑是檢視引擎路徑，也就是 Razor 頁面根相對路徑，而不需要擴充功能和包含只正斜線。</span><span class="sxs-lookup"><span data-stu-id="7f307-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="7f307-119">[AuthorizePage 多載](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)是如果您需要指定授權原則可使用。</span><span class="sxs-lookup"><span data-stu-id="7f307-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="7f307-120">`AuthorizeFilter`可以套用至頁面模型類別`[Authorize]`篩選條件屬性。</span><span class="sxs-lookup"><span data-stu-id="7f307-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="7f307-121">如需詳細資訊，請參閱 <<c0> [ 授權篩選條件屬性](xref:razor-pages/filter#authorize-filter-attribute)。</span><span class="sxs-lookup"><span data-stu-id="7f307-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="7f307-122">需要授權，才能存取頁面的資料夾</span><span class="sxs-lookup"><span data-stu-id="7f307-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="7f307-123">使用[AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)加入[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)所有指定的路徑在資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="7f307-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="7f307-124">指定的路徑是檢視引擎路徑，也就是 Razor 頁面的根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="7f307-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="7f307-125">[AuthorizeFolder 多載](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)是如果您需要指定授權原則可使用。</span><span class="sxs-lookup"><span data-stu-id="7f307-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="7f307-126">需要授權，才能存取 [區域] 頁面</span><span class="sxs-lookup"><span data-stu-id="7f307-126">Require authorization to access an area page</span></span>

<span data-ttu-id="7f307-127">使用[AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)加入[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)至位於指定路徑的 [區域] 頁面：</span><span class="sxs-lookup"><span data-stu-id="7f307-127">Use the [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="7f307-128">頁面名稱是檔案的指定的區域不含副檔名，相對於頁面根目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="7f307-128">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="7f307-129">例如，檔案的頁面名稱*Areas/Identity/Pages/Manage/Accounts.cshtml*是 */管理/帳戶*。</span><span class="sxs-lookup"><span data-stu-id="7f307-129">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="7f307-130">[AuthorizeAreaPage 多載](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_)是如果您需要指定授權原則可使用。</span><span class="sxs-lookup"><span data-stu-id="7f307-130">An [AuthorizeAreaPage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="7f307-131">需要授權，才能存取區域的資料夾</span><span class="sxs-lookup"><span data-stu-id="7f307-131">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="7f307-132">使用[AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)加入[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)所有位於指定路徑的資料夾中的領域：</span><span class="sxs-lookup"><span data-stu-id="7f307-132">Use the [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="7f307-133">資料夾路徑是相對於指定的區域頁面根目錄之資料夾的路徑。</span><span class="sxs-lookup"><span data-stu-id="7f307-133">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="7f307-134">例如，底下的檔案的資料夾路徑*領域/身分識別/網頁/管理/* 是 */管理*。</span><span class="sxs-lookup"><span data-stu-id="7f307-134">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="7f307-135">[AuthorizeAreaFolder 多載](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_)是如果您需要指定授權原則可使用。</span><span class="sxs-lookup"><span data-stu-id="7f307-135">An [AuthorizeAreaFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="7f307-136">允許匿名存取 頁面</span><span class="sxs-lookup"><span data-stu-id="7f307-136">Allow anonymous access to a page</span></span>

<span data-ttu-id="7f307-137">使用[AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)加入[AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)至位於指定路徑的頁面：</span><span class="sxs-lookup"><span data-stu-id="7f307-137">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="7f307-138">指定的路徑是檢視引擎路徑，也就是 Razor 頁面根相對路徑，而不需要擴充功能和包含只正斜線。</span><span class="sxs-lookup"><span data-stu-id="7f307-138">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="7f307-139">允許匿名存取頁面的資料夾</span><span class="sxs-lookup"><span data-stu-id="7f307-139">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="7f307-140">使用[AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)加入[AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)所有指定的路徑在資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="7f307-140">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="7f307-141">指定的路徑是檢視引擎路徑，也就是 Razor 頁面的根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="7f307-141">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="7f307-142">請注意在結合授權及匿名存取</span><span class="sxs-lookup"><span data-stu-id="7f307-142">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="7f307-143">它是完全有效，無法指定頁面的資料夾需要授權，並指定該資料夾內的頁面允許匿名存取：</span><span class="sxs-lookup"><span data-stu-id="7f307-143">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="7f307-144">反之，不過，則不然。</span><span class="sxs-lookup"><span data-stu-id="7f307-144">The reverse, however, isn't true.</span></span> <span data-ttu-id="7f307-145">您無法宣告的匿名存取頁面的資料夾，並指定授權的頁面中：</span><span class="sxs-lookup"><span data-stu-id="7f307-145">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="7f307-146">需要私用的頁面上的授權將無法運作，因為當同時`AllowAnonymousFilter`並`AuthorizeFilter`篩選會套用到頁面上，`AllowAnonymousFilter`獲勝] 和 [控制存取。</span><span class="sxs-lookup"><span data-stu-id="7f307-146">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7f307-147">其他資源</span><span class="sxs-lookup"><span data-stu-id="7f307-147">Additional resources</span></span>

* [<span data-ttu-id="7f307-148">Razor Pages 自訂路由和頁面模型提供者</span><span class="sxs-lookup"><span data-stu-id="7f307-148">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="7f307-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection)類別</span><span class="sxs-lookup"><span data-stu-id="7f307-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
