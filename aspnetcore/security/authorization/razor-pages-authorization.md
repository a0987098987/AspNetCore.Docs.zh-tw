---
title: "在 ASP.NET Core razor 頁面授權慣例"
author: guardrex
description: "了解如何控制存取頁面慣例授權使用者，並允許匿名使用者存取網頁的資料夾。"
manager: wpickett
ms.author: riande
ms.date: 10/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: bbef653c6cf968527e753df9c853f5972640cc03
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="39f19-103">在 ASP.NET Core razor 頁面授權慣例</span><span class="sxs-lookup"><span data-stu-id="39f19-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="39f19-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="39f19-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="39f19-105">控制存取 Razor 網頁應用程式中的一種方式是在啟動時使用授權的慣例。</span><span class="sxs-lookup"><span data-stu-id="39f19-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="39f19-106">這些慣例可讓您以授權使用者，並允許匿名使用者存取個別頁面的資料夾。</span><span class="sxs-lookup"><span data-stu-id="39f19-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="39f19-107">自動本主題所述的慣例套用[授權篩選條件](xref:mvc/controllers/filters#authorization-filters)來控制存取。</span><span class="sxs-lookup"><span data-stu-id="39f19-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="39f19-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="39f19-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="39f19-109">需要授權存取的網頁</span><span class="sxs-lookup"><span data-stu-id="39f19-109">Require authorization to access a page</span></span>

<span data-ttu-id="39f19-110">使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)新增[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)至位於指定路徑頁面：</span><span class="sxs-lookup"><span data-stu-id="39f19-110">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="39f19-111">指定的路徑是檢視引擎路徑，也就是 Razor 頁面根相對路徑，而不需要擴充功能，並包含只正斜線。</span><span class="sxs-lookup"><span data-stu-id="39f19-111">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="39f19-112">[AuthorizePage 多載](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)時才能使用您需要指定授權原則。</span><span class="sxs-lookup"><span data-stu-id="39f19-112">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="39f19-113">需要授權存取網頁的資料夾</span><span class="sxs-lookup"><span data-stu-id="39f19-113">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="39f19-114">使用[AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)新增[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)所有位於指定路徑的資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="39f19-114">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="39f19-115">指定的路徑是檢視引擎路徑，也就是 Razor 頁面根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="39f19-115">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="39f19-116">[AuthorizeFolder 多載](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)時才能使用您需要指定授權原則。</span><span class="sxs-lookup"><span data-stu-id="39f19-116">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="39f19-117">允許匿名存取頁面</span><span class="sxs-lookup"><span data-stu-id="39f19-117">Allow anonymous access to a page</span></span>

<span data-ttu-id="39f19-118">使用[AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)新增[AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)位於指定路徑的頁面：</span><span class="sxs-lookup"><span data-stu-id="39f19-118">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="39f19-119">指定的路徑是檢視引擎路徑，也就是 Razor 頁面根相對路徑，而不需要擴充功能，並包含只正斜線。</span><span class="sxs-lookup"><span data-stu-id="39f19-119">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="39f19-120">允許匿名存取頁面的資料夾</span><span class="sxs-lookup"><span data-stu-id="39f19-120">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="39f19-121">使用[AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)新增[AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)所有位於指定路徑的資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="39f19-121">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="39f19-122">指定的路徑是檢視引擎路徑，也就是 Razor 頁面根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="39f19-122">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="39f19-123">請注意上結合授權及匿名存取</span><span class="sxs-lookup"><span data-stu-id="39f19-123">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="39f19-124">它也可以指定網頁的資料夾需要授權，並指定該資料夾內的頁面可讓匿名存取：</span><span class="sxs-lookup"><span data-stu-id="39f19-124">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="39f19-125">相反地，不過，這並不正確。</span><span class="sxs-lookup"><span data-stu-id="39f19-125">The reverse, however, isn't true.</span></span> <span data-ttu-id="39f19-126">您無法宣告的匿名存取頁面的資料夾，並指定授權中的頁面：</span><span class="sxs-lookup"><span data-stu-id="39f19-126">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="39f19-127">需要授權私用的頁面上將無法運作，因為當同時`AllowAnonymousFilter`和`AuthorizeFilter`篩選會套用到頁面上，`AllowAnonymousFilter`獲勝] 和 [控制存取。</span><span class="sxs-lookup"><span data-stu-id="39f19-127">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="see-also"></a><span data-ttu-id="39f19-128">另請參閱</span><span class="sxs-lookup"><span data-stu-id="39f19-128">See also</span></span>

* [<span data-ttu-id="39f19-129">Razor 頁面自訂路由和頁面模型提供者</span><span class="sxs-lookup"><span data-stu-id="39f19-129">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* <span data-ttu-id="39f19-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection)類別</span><span class="sxs-lookup"><span data-stu-id="39f19-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
