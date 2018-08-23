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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>ASP.NET Core 中的 razor Pages 授權慣例

作者：[Luke Latham](https://github.com/guardrex)

控制 Razor Pages 應用程式中的存取的一個方式是在啟動時使用授權慣例。 這些慣例可讓您授權的使用者，並允許匿名使用者存取個別頁面的資料夾。 自動在本主題中所述的慣例套用[授權篩選條件](xref:mvc/controllers/filters#authorization-filters)來控制存取。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

範例應用程式會使用[沒有 ASP.NET Core 身分識別的 Cookie 驗證](xref:security/authentication/cookie)。 假設使用者林麗莉 Rodriguez 的使用者帳戶是硬式編碼到應用程式。 使用電子郵件使用者名稱 」maria.rodriguez@contoso.com」 和任何登入使用者的密碼。 使用者通過驗證`AuthenticateUser`方法中的*Pages/Account/Login.cshtml.cs*檔案。 在真實世界範例中，您會驗證使用者，對資料庫。 若要使用 ASP.NET Core 身分識別，請依照下列中的指導方針[ASP.NET core 身分識別簡介](xref:security/authentication/identity)主題。 本主題所示的範例與概念同樣適用於使用 ASP.NET Core 身分識別的應用程式。

## <a name="require-authorization-to-access-a-page"></a>需要授權，才能存取頁面

使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)加入[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)至位於指定路徑的頁面：

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

指定的路徑是檢視引擎路徑，也就是 Razor 頁面根相對路徑，而不需要擴充功能和包含只正斜線。

[AuthorizePage 多載](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)是如果您需要指定授權原則可使用。

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> `AuthorizeFilter`可以套用至頁面模型類別`[Authorize]`篩選條件屬性。 如需詳細資訊，請參閱 <<c0> [ 授權篩選條件屬性](xref:razor-pages/filter#authorize-filter-attribute)。

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>需要授權，才能存取頁面的資料夾

使用[AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)加入[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)所有指定的路徑在資料夾中的頁面：

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

指定的路徑是檢視引擎路徑，也就是 Razor 頁面的根相對路徑。

[AuthorizeFolder 多載](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)是如果您需要指定授權原則可使用。

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a>需要授權，才能存取 [區域] 頁面

使用[AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)加入[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)至位於指定路徑的 [區域] 頁面：

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

頁面名稱是檔案的指定的區域不含副檔名，相對於頁面根目錄的路徑。 例如，檔案的頁面名稱*Areas/Identity/Pages/Manage/Accounts.cshtml*是 */管理/帳戶*。

[AuthorizeAreaPage 多載](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_)是如果您需要指定授權原則可使用。

## <a name="require-authorization-to-access-a-folder-of-areas"></a>需要授權，才能存取區域的資料夾

使用[AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)加入[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)所有位於指定路徑的資料夾中的領域：

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

資料夾路徑是相對於指定的區域頁面根目錄之資料夾的路徑。 例如，底下的檔案的資料夾路徑*領域/身分識別/網頁/管理/* 是 */管理*。

[AuthorizeAreaFolder 多載](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_)是如果您需要指定授權原則可使用。

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a>允許匿名存取 頁面

使用[AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)加入[AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)至位於指定路徑的頁面：

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

指定的路徑是檢視引擎路徑，也就是 Razor 頁面根相對路徑，而不需要擴充功能和包含只正斜線。

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>允許匿名存取頁面的資料夾

使用[AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder)透過慣例[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)加入[AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)所有指定的路徑在資料夾中的頁面：

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

指定的路徑是檢視引擎路徑，也就是 Razor 頁面的根相對路徑。

## <a name="note-on-combining-authorized-and-anonymous-access"></a>請注意在結合授權及匿名存取

它是完全有效，無法指定頁面的資料夾需要授權，並指定該資料夾內的頁面允許匿名存取：

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

反之，不過，則不然。 您無法宣告的匿名存取頁面的資料夾，並指定授權的頁面中：

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

需要私用的頁面上的授權將無法運作，因為當同時`AllowAnonymousFilter`並`AuthorizeFilter`篩選會套用到頁面上，`AllowAnonymousFilter`獲勝] 和 [控制存取。

## <a name="additional-resources"></a>其他資源

* [Razor Pages 自訂路由和頁面模型提供者](xref:razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection)類別
