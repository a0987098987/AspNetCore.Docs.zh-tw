---
title: 在 ASP.NET Core razor 頁面授權慣例
author: guardrex
description: 了解如何控制存取頁面慣例授權使用者，並允許匿名使用者存取網頁的資料夾。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 35a21156c001d8703e09e604129c4c2c500fe25f
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734649"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="5fd94-103">在 ASP.NET Core razor 頁面授權慣例</span><span class="sxs-lookup"><span data-stu-id="5fd94-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="5fd94-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5fd94-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5fd94-105">控制存取 Razor 網頁應用程式中的一種方式是在啟動時使用授權的慣例。</span><span class="sxs-lookup"><span data-stu-id="5fd94-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="5fd94-106">這些慣例可讓您以授權使用者，並允許匿名使用者存取個別頁面的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5fd94-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="5fd94-107">自動本主題所述的慣例套用[授權篩選條件](xref:mvc/controllers/filters#authorization-filters)來控制存取。</span><span class="sxs-lookup"><span data-stu-id="5fd94-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="5fd94-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5fd94-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5fd94-109">範例應用程式會使用[沒有 ASP.NET Core 身分識別的 Cookie 驗證](xref:security/authentication/cookie)。</span><span class="sxs-lookup"><span data-stu-id="5fd94-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="5fd94-110">假設使用者 Maria Rodriguez 的使用者帳戶是在應用程式的硬式編碼。</span><span class="sxs-lookup"><span data-stu-id="5fd94-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="5fd94-111">使用電子郵件使用者名稱 」maria.rodriguez@contoso.com"和任何登入使用者的密碼。</span><span class="sxs-lookup"><span data-stu-id="5fd94-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="5fd94-112">使用者通過驗證`AuthenticateUser`方法中的*Pages/Account/Login.cshtml.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="5fd94-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="5fd94-113">在真實世界範例中，您會驗證使用者，對資料庫。</span><span class="sxs-lookup"><span data-stu-id="5fd94-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="5fd94-114">若要使用 ASP.NET Core 身分識別，請依照下列中的指導方針[ASP.NET Core 上的識別簡介](xref:security/authentication/identity)主題。</span><span class="sxs-lookup"><span data-stu-id="5fd94-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="5fd94-115">概念和本主題中範例所示適用於使用 ASP.NET Core 身分識別的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5fd94-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="5fd94-116">需要授權存取的網頁</span><span class="sxs-lookup"><span data-stu-id="5fd94-116">Require authorization to access a page</span></span>

<span data-ttu-id="5fd94-117">使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)新增[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)至位於指定路徑頁面：</span><span class="sxs-lookup"><span data-stu-id="5fd94-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="5fd94-118">指定的路徑是檢視引擎路徑，也就是 Razor 頁面根相對路徑，而不需要擴充功能，並包含只正斜線。</span><span class="sxs-lookup"><span data-stu-id="5fd94-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="5fd94-119">[AuthorizePage 多載](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)時才能使用您需要指定授權原則。</span><span class="sxs-lookup"><span data-stu-id="5fd94-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="5fd94-120">`AuthorizeFilter`可以套用至頁面模型類別與`[Authorize]`篩選條件屬性。</span><span class="sxs-lookup"><span data-stu-id="5fd94-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="5fd94-121">如需詳細資訊，請參閱[授權篩選條件屬性](xref:mvc/razor-pages/filter#authorize-filter-attribute)。</span><span class="sxs-lookup"><span data-stu-id="5fd94-121">For more information, see [Authorize filter attribute](xref:mvc/razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="5fd94-122">需要授權存取網頁的資料夾</span><span class="sxs-lookup"><span data-stu-id="5fd94-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="5fd94-123">使用[AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)新增[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)所有位於指定路徑的資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="5fd94-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="5fd94-124">指定的路徑是檢視引擎路徑，也就是 Razor 頁面根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="5fd94-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="5fd94-125">[AuthorizeFolder 多載](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)時才能使用您需要指定授權原則。</span><span class="sxs-lookup"><span data-stu-id="5fd94-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="5fd94-126">允許匿名存取頁面</span><span class="sxs-lookup"><span data-stu-id="5fd94-126">Allow anonymous access to a page</span></span>

<span data-ttu-id="5fd94-127">使用[AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)新增[AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)位於指定路徑的頁面：</span><span class="sxs-lookup"><span data-stu-id="5fd94-127">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="5fd94-128">指定的路徑是檢視引擎路徑，也就是 Razor 頁面根相對路徑，而不需要擴充功能，並包含只正斜線。</span><span class="sxs-lookup"><span data-stu-id="5fd94-128">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="5fd94-129">允許匿名存取頁面的資料夾</span><span class="sxs-lookup"><span data-stu-id="5fd94-129">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="5fd94-130">使用[AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)新增[AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)所有位於指定路徑的資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="5fd94-130">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="5fd94-131">指定的路徑是檢視引擎路徑，也就是 Razor 頁面根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="5fd94-131">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="5fd94-132">請注意上結合授權及匿名存取</span><span class="sxs-lookup"><span data-stu-id="5fd94-132">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="5fd94-133">它也可以指定網頁的資料夾需要授權，並指定該資料夾內的頁面可讓匿名存取：</span><span class="sxs-lookup"><span data-stu-id="5fd94-133">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="5fd94-134">相反地，不過，這並不正確。</span><span class="sxs-lookup"><span data-stu-id="5fd94-134">The reverse, however, isn't true.</span></span> <span data-ttu-id="5fd94-135">您無法宣告的匿名存取頁面的資料夾，並指定授權中的頁面：</span><span class="sxs-lookup"><span data-stu-id="5fd94-135">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="5fd94-136">需要授權私用的頁面上將無法運作，因為當同時`AllowAnonymousFilter`和`AuthorizeFilter`篩選會套用到頁面上，`AllowAnonymousFilter`獲勝] 和 [控制存取。</span><span class="sxs-lookup"><span data-stu-id="5fd94-136">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5fd94-137">其他資源</span><span class="sxs-lookup"><span data-stu-id="5fd94-137">Additional resources</span></span>

* [<span data-ttu-id="5fd94-138">Razor Pages 自訂路由和頁面模型提供者</span><span class="sxs-lookup"><span data-stu-id="5fd94-138">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-conventions)
* <span data-ttu-id="5fd94-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection)類別</span><span class="sxs-lookup"><span data-stu-id="5fd94-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
