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
ms.openlocfilehash: 0f4022e46993c6a957d3d0c133b3db56fa650edc
ms.sourcegitcommit: d9ae1f352d372a20534b57e23646c1a1d9171af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/21/2020
ms.locfileid: "86568817"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>RazorASP.NET Core 中的頁面授權慣例

::: moniker range=">= aspnetcore-3.0"

控制頁面應用程式存取權的其中一種方式 Razor 是在啟動時使用授權慣例。 這些慣例可讓您授權使用者，並允許匿名使用者存取頁面的個別頁面或資料夾。 本主題中所述的慣例會自動套用[授權篩選](xref:mvc/controllers/filters#authorization-filters)來控制存取。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples)（[如何下載](xref:index#how-to-download-a-sample)）

範例應用程式[會使用 cookie 驗證， Identity 而不 ASP.NET Core ](xref:security/authentication/cookie)。 本主題所顯示的概念和範例同樣適用于使用 ASP.NET Core 的應用程式 Identity 。 若要使用 ASP.NET Core Identity ，請遵循中的指導方針 <xref:security/authentication/identity> 。

## <a name="require-authorization-to-access-a-page"></a>需要授權才能存取頁面

使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> 慣例，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至頁面的指定路徑：

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=3)]

指定的路徑為 View Engine path，這是 Razor 頁面根相對路徑，沒有副檔名，而且只包含正斜線。

若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*)多載：

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>可以套用至具有 filter 屬性的頁面模型類別 `[Authorize]` 。 如需詳細資訊，請參閱[授權篩選屬性](xref:razor-pages/filter#authorize-filter-attribute)。

## <a name="require-authorization-to-access-a-folder-of-pages"></a>需要授權才能存取頁面的資料夾

使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> 慣例，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至資料夾中指定路徑的所有頁面：

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=4)]

指定的路徑是 View Engine path，這是 Razor 頁面根相對路徑。

若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*)多載：

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>需要授權才能存取區域頁面

使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> 慣例，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至位於指定路徑的 [區域] 頁面：

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

[頁面名稱] 是檔案的路徑，沒有副檔名相對於指定區域的頁面根目錄。 例如，檔案*區域/ Identity /Pages/Manage/Accounts.cshtml*的頁面名稱是 */Manage/Accounts*。

若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*)多載：

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>需要授權才能存取區域的資料夾

使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> 慣例，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至資料夾中指定路徑的所有區域：

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

資料夾路徑是相對於指定區域的頁面根目錄的資料夾路徑。 例如，*區域/ Identity /Pages/Manage/* 下檔案的資料夾路徑為 */Manage*。

若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*)多載：

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>允許匿名存取頁面

使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> 慣例，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 至位於指定路徑的頁面：

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=5)]

指定的路徑為 View Engine path，這是 Razor 頁面根相對路徑，沒有副檔名，而且只包含正斜線。

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>允許匿名存取頁面資料夾

使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> 慣例，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 至資料夾中指定路徑的所有頁面：

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=6)]

指定的路徑是 View Engine path，這是 Razor 頁面根相對路徑。

## <a name="note-on-combining-authorized-and-anonymous-access"></a>結合已授權和匿名存取的注意事項

指定頁面的資料夾需要授權，然後指定該資料夾內的頁面允許匿名存取，是有效的：

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

不過，相反的，這是不正確。 您無法針對匿名存取宣告頁面的資料夾，然後在該資料夾內指定需要授權的頁面：

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

在私用頁面上要求授權失敗。 當 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 和都套用 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至頁面時，會優先使用 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 並控制存取。

## <a name="additional-resources"></a>其他資源

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

控制頁面應用程式存取權的其中一種方式 Razor 是在啟動時使用授權慣例。 這些慣例可讓您授權使用者，並允許匿名使用者存取頁面的個別頁面或資料夾。 本主題中所述的慣例會自動套用[授權篩選](xref:mvc/controllers/filters#authorization-filters)來控制存取。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples)（[如何下載](xref:index#how-to-download-a-sample)）

範例應用程式[會使用 cookie 驗證， Identity 而不 ASP.NET Core ](xref:security/authentication/cookie)。 本主題所顯示的概念和範例同樣適用于使用 ASP.NET Core 的應用程式 Identity 。 若要使用 ASP.NET Core Identity ，請遵循中的指導方針 <xref:security/authentication/identity> 。

## <a name="require-authorization-to-access-a-page"></a>需要授權才能存取頁面

透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至頁面的指定路徑：

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

指定的路徑為 View Engine path，這是 Razor 頁面根相對路徑，沒有副檔名，而且只包含正斜線。

若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*)多載：

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>可以套用至具有 filter 屬性的頁面模型類別 `[Authorize]` 。 如需詳細資訊，請參閱[授權篩選屬性](xref:razor-pages/filter#authorize-filter-attribute)。

## <a name="require-authorization-to-access-a-folder-of-pages"></a>需要授權才能存取頁面的資料夾

透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至資料夾中指定路徑的所有頁面：

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

指定的路徑是 View Engine path，這是 Razor 頁面根相對路徑。

若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*)多載：

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>需要授權才能存取區域頁面

透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至位於指定路徑的 [區域] 頁面：

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

[頁面名稱] 是檔案的路徑，沒有副檔名相對於指定區域的頁面根目錄。 例如，檔案*區域/ Identity /Pages/Manage/Accounts.cshtml*的頁面名稱是 */Manage/Accounts*。

若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*)多載：

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>需要授權才能存取區域的資料夾

透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至資料夾中指定路徑的所有區域：

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

資料夾路徑是相對於指定區域的頁面根目錄的資料夾路徑。 例如，*區域/ Identity /Pages/Manage/* 下檔案的資料夾路徑為 */Manage*。

若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*)多載：

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>允許匿名存取頁面

透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 至位於指定路徑的頁面：

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

指定的路徑為 View Engine path，這是 Razor 頁面根相對路徑，沒有副檔名，而且只包含正斜線。

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>允許匿名存取頁面資料夾

透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 至資料夾中指定路徑的所有頁面：

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

指定的路徑是 View Engine path，這是 Razor 頁面根相對路徑。

## <a name="note-on-combining-authorized-and-anonymous-access"></a>結合已授權和匿名存取的注意事項

有效的方式是指定需要授權的頁面資料夾，並指定該資料夾內的頁面允許匿名存取：

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

不過，相反的，這是不正確。 您無法針對匿名存取宣告頁面的資料夾，然後在該資料夾內指定需要授權的頁面：

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

在私用頁面上要求授權失敗。 當 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 和都套用 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至頁面時，會優先使用 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 並控制存取。

## <a name="additional-resources"></a>其他資源

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
