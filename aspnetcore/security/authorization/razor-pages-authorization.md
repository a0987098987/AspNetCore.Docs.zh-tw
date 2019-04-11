---
title: ASP.NET Core 中的 razor Pages 授權慣例
author: guardrex
description: 了解如何控制存取，授權使用者，並允許匿名使用者存取頁面的資料夾慣例的頁面。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 040d33eba7eaf7a3aece2eedcdef7343e52972af
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345495"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>ASP.NET Core 中的 razor Pages 授權慣例

作者：[Luke Latham](https://github.com/guardrex)

控制 Razor Pages 應用程式中的存取的一個方式是在啟動時使用授權慣例。 這些慣例可讓您授權的使用者，並允許匿名使用者存取個別頁面的資料夾。 自動在本主題中所述的慣例套用[授權篩選條件](xref:mvc/controllers/filters#authorization-filters)來控制存取。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

範例應用程式會使用[沒有 ASP.NET Core 身分識別的 cookie 驗證](xref:security/authentication/cookie)。 本主題所示的範例與概念同樣適用於使用 ASP.NET Core 身分識別的應用程式。 若要使用 ASP.NET Core 身分識別，請依照下列中的指導方針<xref:security/authentication/identity>。

## <a name="require-authorization-to-access-a-page"></a>需要授權，才能存取頁面

使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*>透過慣例<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>以新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>至位於指定路徑的頁面：

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

指定的路徑是檢視引擎路徑，也就是 Razor 頁面根相對路徑，而不需要擴充功能和包含只正斜線。

若要指定[授權原則](xref:security/authorization/policies)，使用[AuthorizePage 多載](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>可以套用至頁面模型類別`[Authorize]`篩選條件屬性。 如需詳細資訊，請參閱 <<c0> [ 授權篩選條件屬性](xref:razor-pages/filter#authorize-filter-attribute)。

## <a name="require-authorization-to-access-a-folder-of-pages"></a>需要授權，才能存取頁面的資料夾

使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*>透過慣例<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>以新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>所有指定的路徑在資料夾中的頁面：

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

指定的路徑是檢視引擎路徑，也就是 Razor 頁面的根相對路徑。

若要指定[授權原則](xref:security/authorization/policies)，使用[AuthorizeFolder 多載](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>需要授權，才能存取 [區域] 頁面

使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*>透過慣例<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>以新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>至位於指定路徑的 [區域] 頁面：

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

頁面名稱是檔案的指定的區域不含副檔名，相對於頁面根目錄的路徑。 例如，檔案的頁面名稱*Areas/Identity/Pages/Manage/Accounts.cshtml*是 */管理/帳戶*。

若要指定[授權原則](xref:security/authorization/policies)，使用[AuthorizeAreaPage 多載](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>需要授權，才能存取區域的資料夾

使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*>透過慣例<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>以新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>所有位於指定路徑的資料夾中的領域：

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

資料夾路徑是相對於指定的區域頁面根目錄之資料夾的路徑。 例如，底下的檔案的資料夾路徑*領域/身分識別/網頁/管理/* 是 */管理*。

若要指定[授權原則](xref:security/authorization/policies)，使用[AuthorizeAreaFolder 多載](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>允許匿名存取 頁面

使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*>透過慣例<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>以新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>至位於指定路徑的頁面：

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

指定的路徑是檢視引擎路徑，也就是 Razor 頁面根相對路徑，而不需要擴充功能和包含只正斜線。

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>允許匿名存取頁面的資料夾

使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*>透過慣例<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>以新增<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>所有指定的路徑在資料夾中的頁面：

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

指定的路徑是檢視引擎路徑，也就是 Razor 頁面的根相對路徑。

## <a name="note-on-combining-authorized-and-anonymous-access"></a>請注意在結合授權及匿名存取

指定有效的頁面需要授權的資料夾並指定該資料夾內的頁面允許匿名存取：

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

相反地，不過，不是有效的。 您無法宣告的匿名存取頁面的資料夾，然後指定 需要授權該資料夾內的頁面：

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

在 [私人] 頁面上的要求授權失敗。 當同時<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>並<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>套用至網頁，<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>會優先使用並控制存取。

## <a name="additional-resources"></a>其他資源

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
