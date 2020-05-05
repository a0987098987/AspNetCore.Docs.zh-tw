---
title: ASP.NET Core 中頁面Razor的篩選方法
author: Rick-Anderson
description: 瞭解如何在 ASP.NET Core 中建立頁面Razor的篩選準則方法。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 2/18/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: razor-pages/filter
ms.openlocfilehash: 68962d5a3a49e52510d72899e7dead2c1983d8b6
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775514"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a>ASP.NET Core 中頁面Razor的篩選方法

::: moniker range=">= aspnetcore-3.0"

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

Razor頁面篩選[IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0)和[IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0)允許Razor頁面在Razor頁面處理常式執行之前和之後執行程式碼。 Razor頁面篩選與[ASP.NET CORE MVC 動作篩選器](xref:mvc/controllers/filters#action-filters)相似，不同之處在于它們不能套用至個別的頁面處理常式方法。

Razor頁面篩選：

* 在選取處理常式方法之後，但在進行模型繫結之前執行程式碼。
* 在執行處理常式方法之前，並在完成模型繫結之後執行程式碼。
* 在執行處理常式方法之後執行程式碼。
* 可以在某個頁面或全域實作。
* 無法套用至特定頁面處理常式方法。
* 可以有相依性[插入](xref:fundamentals/dependency-injection)（DI）填入的函式相依性。 如需詳細資訊，請參閱[ServiceFilterAttribute](/aspnet/core/mvc/controllers/filters#servicefilterattribute)和[TypeFilterAttribute](/aspnet/core/mvc/controllers/filters#typefilterattribute)。

雖然頁面的程式碼和中介軟體能夠在處理常式方法執行之前執行Razor自訂程式碼， <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext>但只有頁面篩選可以存取和頁面。 中介軟體具有的`HttpContext`存取權，而不是「頁面內容」。 篩選具有<xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext>衍生的參數，可提供對的`HttpContext`存取。 例如，[實作篩選條件屬性](#ifa)範例會將標頭新增至回應，這是無法使用建構函式或中介軟體完成的作業。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/3.1sample)（[如何下載](xref:index#how-to-download-a-sample)）

Razor頁面篩選提供下列方法，可在全域套用或在頁面層級套用：

* 同步方法：

  * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0)：在選取處理常式方法之後，但在進行模型繫結之前呼叫。
  * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0)：在執行處理常式方法之前，並在完成模型繫結之後呼叫。
  * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0)：在執行處理常式方法之後，並在動作結果之前呼叫。

* 非同步方法：

  * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0)：在選取處理常式方法之後，但在進行模型繫結之前以非同步方式呼叫。
  * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0)：在叫用處理常式方法之前，並在完成模型繫結之後以非同步方式呼叫。

請實作同步**或**非同步版本的篩選條件介面，而**不要**同時實作這兩者。 架構會先檢查以查看篩選條件是否實作非同步介面，如果是的話，便呼叫該介面。 如果沒有，它會呼叫同步介面的方法。 如果同時實作為這兩個介面，則只會呼叫非同步方法。 相同的規則會套用至頁面中的覆寫，實作覆寫的同步或非同步版本，但不能同時實作。

## <a name="implement-razor-page-filters-globally"></a>全域Razor執行頁面篩選

下列程式碼會實作 `IAsyncPageFilter`：

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

在上述程式碼中`ProcessUserAgent.Write` ，是使用者提供的程式碼，可搭配使用者代理字串使用。

下列程式碼會啟用 `Startup` 類別中的 `SampleAsyncPageFilter`：

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup.cs?name=snippet2)]

下列程式碼會<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>呼叫，以`SampleAsyncPageFilter`將只套用至 */Movies*中的頁面：

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup2.cs?name=snippet2)]

下例程式碼會實作同步的 `IPageFilter`：

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

下列程式碼會啟用 `SamplePageFilter`：

[!code-csharp[Main](filter/3.1sample/PageFilter/StartupSync.cs?name=snippet2)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>藉Razor由覆寫篩選方法來執行頁面篩選

下列程式碼會覆寫Razor非同步頁面篩選：

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Index.cshtml.cs?name=snippet)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a>實作篩選條件屬性

內建以屬性為基礎的篩選<xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter.OnResultExecutionAsync*>篩選器可以子類別化。 下列篩選條件會將標頭新增至回應：

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/AddHeaderAttribute.cs)]

下列程式碼會套用 `AddHeader` 屬性：

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Movies/Test.cshtml.cs)]

使用瀏覽器開發人員工具之類的工具來檢查標頭。 在 [**回應標頭**] 底下， `author: Rick`會顯示。

如需覆寫順序的指示，請參閱[覆寫預設順序](xref:mvc/controllers/filters#overriding-the-default-order)。

如需從篩選條件縮短篩選管線的指示，請參閱[取消和縮短](xref:mvc/controllers/filters#cancellation-and-short-circuiting)。

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a>授權篩選條件屬性

[Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) 屬性可以套用至 `PageModel`：

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

Razor頁面篩選[IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0)和[IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0)允許Razor頁面在Razor頁面處理常式執行之前和之後執行程式碼。 Razor頁面篩選與[ASP.NET CORE MVC 動作篩選器](xref:mvc/controllers/filters#action-filters)相似，不同之處在于它們不能套用至個別的頁面處理常式方法。

Razor頁面篩選：

* 在選取處理常式方法之後，但在進行模型繫結之前執行程式碼。
* 在執行處理常式方法之前，並在完成模型繫結之後執行程式碼。
* 在執行處理常式方法之後執行程式碼。
* 可以在某個頁面或全域實作。
* 無法套用至特定頁面處理常式方法。

您可以在處理常式方法執行之前，使用頁面的函式或中介軟體， Razor但只有頁面篩選器才能夠存取[HttpCoNtext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext)。 篩選條件具有 [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) 衍生參數，可提供對 `HttpContext` 的存取。 例如，[實作篩選條件屬性](#ifa)範例會將標頭新增至回應，這是無法使用建構函式或中介軟體完成的作業。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter)（[如何下載](xref:index#how-to-download-a-sample)）

Razor頁面篩選提供下列方法，可在全域套用或在頁面層級套用：

* 同步方法：

  * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0)：在選取處理常式方法之後，但在進行模型繫結之前呼叫。
  * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0)：在執行處理常式方法之前，並在完成模型繫結之後呼叫。
  * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0)：在執行處理常式方法之後，並在動作結果之前呼叫。

* 非同步方法：

  * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0)：在選取處理常式方法之後，但在進行模型繫結之前以非同步方式呼叫。
  * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0)：在叫用處理常式方法之前，並在完成模型繫結之後以非同步方式呼叫。

> [!NOTE]
> 請實作同步**或**非同步版本的篩選條件介面，而不要同時實作這兩者。 架構會先檢查以查看篩選條件是否實作非同步介面，如果是的話，便呼叫該介面。 如果沒有，它會呼叫同步介面的方法。 如果同時實作為這兩個介面，則只會呼叫非同步方法。 相同的規則會套用至頁面中的覆寫，實作覆寫的同步或非同步版本，但不能同時實作。

## <a name="implement-razor-page-filters-globally"></a>全域Razor執行頁面篩選

下列程式碼會實作 `IAsyncPageFilter`：

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

在上述程式碼中，[ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) 並非必要。 它在範例中用來提供應用程式的追蹤資訊。

下列程式碼會啟用 `Startup` 類別中的 `SampleAsyncPageFilter`：

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

下列程式碼顯示完整的 `Startup` 類別：

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

下列程式碼會呼叫 `AddFolderApplicationModelConvention`，只將 `SampleAsyncPageFilter` 套用至 */subFolder* 中的頁面：

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

下例程式碼會實作同步的 `IPageFilter`：

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

下列程式碼會啟用 `SamplePageFilter`：

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>藉Razor由覆寫篩選方法來執行頁面篩選

下列程式碼會覆寫Razor同步頁面篩選：

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a>實作篩選條件屬性

以內建屬性為基礎的篩選條件 [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) 可以進行子類別化。 下列篩選條件會將標頭新增至回應：

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

下列程式碼會套用 `AddHeader` 屬性：

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

如需覆寫順序的指示，請參閱[覆寫預設順序](xref:mvc/controllers/filters#overriding-the-default-order)。

如需從篩選條件縮短篩選管線的指示，請參閱[取消和縮短](xref:mvc/controllers/filters#cancellation-and-short-circuiting)。 

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a>授權篩選條件屬性

[Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) 屬性可以套用至 `PageModel`：

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end
