---
title: "在 ASP.NET Core razor 頁面授權慣例"
author: guardrex
description: "了解如何控制存取頁面慣例在啟動時，授權使用者，並允許匿名使用者存取個別頁面的資料夾。"
ms.author: riande
manager: wpickett
ms.date: 10/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 72b558816e687c30d0c60f2fd85227d0d803219b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>在 ASP.NET Core razor 頁面授權慣例

作者：[Luke Latham](https://github.com/guardrex)

控制存取 Razor 網頁應用程式中的一種方式是在啟動時使用授權的慣例。 這些慣例可讓您以授權使用者，並允許匿名使用者存取個別頁面的資料夾。 自動本主題所述的慣例套用[授權篩選條件](xref:mvc/controllers/filters#authorization-filters)來控制存取。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="require-authorization-to-access-a-page"></a>需要授權存取的網頁

使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)新增[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)至位於指定路徑頁面：

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

指定的路徑是檢視引擎路徑，也就是 Razor 頁面根相對路徑，而不需要擴充功能，並包含只正斜線。

[AuthorizePage 多載](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)時才能使用您需要指定授權原則。

## <a name="require-authorization-to-access-a-folder-of-pages"></a>需要授權存取網頁的資料夾

使用[AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)新增[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)所有位於指定路徑的資料夾中的頁面：

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

指定的路徑是檢視引擎路徑，也就是 Razor 頁面根目錄的相對路徑。

[AuthorizeFolder 多載](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)時才能使用您需要指定授權原則。

## <a name="allow-anonymous-access-to-a-page"></a>允許匿名存取頁面

使用[AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)新增[AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)位於指定路徑的頁面：

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

指定的路徑是檢視引擎路徑，也就是 Razor 頁面根相對路徑，而不需要擴充功能，並包含只正斜線。

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>允許匿名存取頁面的資料夾

使用[AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)新增[AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)所有位於指定路徑的資料夾中的頁面：

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

指定的路徑是檢視引擎路徑，也就是 Razor 頁面根目錄的相對路徑。

## <a name="note-on-combining-authorized-and-anonymous-access"></a>請注意上結合授權及匿名存取

它也可以指定網頁的資料夾需要授權，並指定該資料夾內的頁面可讓匿名存取：

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

相反地，不過，這並不正確。 您無法宣告的匿名存取頁面的資料夾，並指定授權中的頁面：

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

需要授權私用的頁面上將無法運作，因為當同時`AllowAnonymousFilter`和`AuthorizeFilter`篩選會套用到頁面上，`AllowAnonymousFilter`獲勝] 和 [控制存取。

## <a name="see-also"></a>另請參閱

* [Razor 頁面自訂路由和頁面模型提供者](xref:mvc/razor-pages/razor-pages-convention-features)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection)類別
