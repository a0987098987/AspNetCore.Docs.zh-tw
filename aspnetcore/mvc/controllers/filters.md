---
title: ASP.NET Core 中的篩選條件
author: Rick-Anderson
description: 了解篩選條件的運作方式，以及如何在 ASP.NET Core 中使用它們。
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/controllers/filters
ms.openlocfilehash: 96d24940af6c591e3c02bfa26ed9d7d6ea60d27d
ms.sourcegitcommit: d00a200bc8347af794b24184da14ad5c8b6bba9a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/21/2020
ms.locfileid: "86869974"
---
# <a name="filters-in-aspnet-core"></a>ASP.NET Core 中的篩選條件

::: moniker range=">= aspnetcore-3.0"

作者：[Kirk Larkin](https://github.com/serpent5) \(英文\)、[Rick Anderson](https://twitter.com/RickAndMSFT) \(英文\)、[Tom Dykstra](https://github.com/tdykstra/) \(英文\) 及 [Steve Smith](https://ardalis.com/) \(英文\)

ASP.NET Core 中的「篩選條件」** 可讓程式碼在要求處理管線中的特定階段之前或之後執行。

內建篩選條件會處理工作，例如：

* 授權 (避免存取使用者未獲授權的資源)。
* 回應快取 (縮短要求管線，傳回快取的回應)。

可以建立自訂篩選條件來處理跨領域關注。 跨領域關注的範例包括錯誤處理、快取、設定、授權及記錄。  篩選能避免重複的程式碼。 例如，錯誤處理例外狀況篩選條件中可以合併錯誤處理。

本檔適用于 Razor 具有 views 的頁面、API 控制器和控制器。 篩選器無法直接與[ Razor 元件](xref:blazor/components/index)搭配使用。 篩選器只會在下列情況時間接影響元件：

* 此元件內嵌在頁面或視圖中。
* 頁面或控制器/視圖會使用篩選準則。

[檢視或下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))。

## <a name="how-filters-work"></a>篩選條件如何運作

篩選條件會在「ASP.NET Core 動作引動過程管線」** 中執行，其有時也被稱為「篩選條件管線」**。 在 ASP.NET Core 之後執行的篩選條件管線會選取要執行的動作。

![此要求會透過其他中介軟體、路由中介軟體、動作選取和動作調用管線來處理。 要求處理繼續往回歷經動作選取、路由中介軟體，以及各種其他中介軟體，然後才能成為傳送至用戶端的回應。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>篩選條件類型

每個篩選類型會在篩選條件管線中的不同階段執行：

* [授權篩選條件](#authorization-filters)會先執行，用來判斷使用者是否已針對要求取得授權。 如果要求未獲授權，授權篩選器會將管線短路。

* [資源篩選器](#resource-filters)：

  * 會在授權之後執行。  
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*>在篩選器管線的其餘部分之前執行程式碼。 例如， `OnResourceExecuting` 在模型系結之前執行程式碼。
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*>在管線的其餘部分完成之後執行程式碼。

* [動作篩選](#action-filters)條件：

  * 在呼叫動作方法之前和之後，立即執行程式碼。
  * 可以變更傳遞至動作的引數。
  * 可以變更從動作傳回的結果。
  * 在頁面中**不**支援 Razor 。

* [例外狀況篩選準則](#exception-filters)會將全域原則套用至在寫入回應主體之前發生的未處理例外狀況。

* [結果篩選準則](#result-filters)會在動作結果執行前後立即執行程式碼。 它們只有在動作方法執行成功時才執行。 它們適用於必須包圍檢視或格式器執行的邏輯。

下圖顯示篩選條件類型如何在篩選條件管線中互動。

![要求處理會歷經授權篩選條件、資源篩選條件、模型繫結、動作篩選條件、動作執行和動作結果轉換、例外狀況篩選條件、結果篩選條件，以及結果執行。 在送出的過程，要求只會經過結果篩選條件和資源篩選條件的處理，然後便成為傳送至用戶端的回應。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>實作

篩選條件同時支援透過不同介面定義的同步和非同步實作。

同步篩選會在其管線階段之前和之後執行程式碼。 例如，系統會在呼叫動作方法之前呼叫 <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*>。 系統會在傳回動作方法之後呼叫 <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*>。

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

在上述程式碼中， [MyDebug](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/controllers/filters/3.1sample/FiltersSample/Helper/MyDebug.cs)是[範例下載](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/controllers/filters/3.1sample/FiltersSample/Helper/MyDebug.cs)中的公用程式函式。

非同步篩選會定義 `On-Stage-ExecutionAsync` 方法。 例如，<xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>：

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

在上述程式碼中， `SampleAsyncActionFilter` 具有 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> `next` 執行動作方法的（）。

### <a name="multiple-filter-stages"></a>多個篩選條件階段

可以在單一類別中實作多個篩選條件階段的介面。 例如，類別會 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> 實行：

* 同步： <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> 和<xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter>
* 非同步： <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> 和<xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>

請實作同步**或**非同步版本的篩選條件介面，而**不要**同時實作這兩者。 執行階段會先檢查以查看篩選條件是否會實作非同步介面，如果是，便會呼叫該介面。 如果沒有，它會呼叫同步介面的方法。 如果同時在單一類別中實作非同步和同步介面，系統只會呼叫非同步方法。 使用之類的抽象類別時 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> ，只會覆寫同步方法或每個篩選準則類型的非同步方法。

### <a name="built-in-filter-attributes"></a>內建篩選條件屬性

ASP.NET Core 包含內建的屬性型篩選條件，可對其進行子類別化和自訂。 例如，下列結果篩選條件會將標頭新增至回應：

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

屬性可讓篩選條件接受引數，如上述範例所示。 將 `AddHeaderAttribute` 新增至控制器或動作方法，並指定 HTTP 標頭的名稱和值：

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

使用[瀏覽器開發人員工具](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools)之類的工具來檢查標頭。 在 [**回應標頭**] 底下， `author: Rick Anderson` 會顯示。

下列程式碼 `ActionFilterAttribute` 會執行：

* 讀取設定系統中的標題和名稱。 與先前的範例不同的是，下列程式碼不需要將篩選參數新增至程式碼。
* 將標題和名稱新增至回應標頭。

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyActionFilterAttribute.cs?name=snippet)]

設定選項是使用[選項模式](xref:fundamentals/configuration/options)從設定[系統](xref:fundamentals/configuration/index)提供。 例如，從*appsettings.js*檔案：

[!code-json[](filters/3.1sample/FiltersSample/appsettings.json)]

在 `StartUp.ConfigureServices` 中：

* `PositionOptions`類別會使用設定區域新增至服務容器 `"Position"` 。
* `MyActionFilterAttribute`會新增至服務容器。

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupAF.cs?name=snippet)]

下列程式碼顯示 `PositionOptions` 類別：

[!code-csharp[](filters/3.1sample/FiltersSample/Helper/PositionOptions.cs?name=snippet)]

下列程式碼會將套用 `MyActionFilterAttribute` 至 `Index2` 方法：

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet2&highlight=9)]

**Response Headers** `author: Rick Anderson` `Editor: Joe Smith` 當呼叫端點時，會顯示回應標頭、和下的 `Sample/Index2` 。

下列程式碼會將 `MyActionFilterAttribute` 和套用 `AddHeaderAttribute` 至 Razor 頁面：

[!code-csharp[](filters/3.1sample/FiltersSample/Pages/Movies/Index.cshtml.cs?name=snippet)]

篩選準則無法套用至 Razor 頁面處理常式方法。 它們可以套用至 Razor 頁面模型或全域套用。

有幾個篩選條件介面有對應的屬性，可用來作為自訂實作的基底類別。

篩選條件屬性：

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a>篩選條件範圍和執行的順序

篩選條件能以三個「範圍」** 之一新增至管線：

* 在控制器動作上使用屬性。 篩選屬性無法套用至 Razor 頁面處理常式方法。
* 使用控制器或頁面上的屬性 Razor 。
* 全域適用于所有的控制器、動作和 Razor 頁面，如下列程式碼所示：

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

### <a name="default-order-of-execution"></a>預設執行順序

當管線的特定階段有多個篩選條件時，範圍會決定篩選條件的預設執行順序。  全域篩選條件會圍繞類別篩選條件，後者又圍繞方法篩選條件。

因為篩選條件巢狀結構的原因，篩選條件的「之後」** 程式碼的執行順序會與「之前」** 程式碼相反。 篩選條件序列：

* 全域篩選條件的「之前」** 程式碼。
  * 控制器和頁面篩選器的*前面*程式碼 Razor 。
    * 動作方法篩選條件的「之前」** 程式碼。
    * 動作方法篩選條件的「之後」** 程式碼。
  * 控制器和頁面篩選器的*之後*程式碼 Razor 。
* 全域篩選條件的「之後」** 程式碼。
  
下列範例說明針對同步動作篩選條件呼叫篩選條件方法的順序。

| 順序 | 篩選條件範圍 | 篩選條件方法 |
|:--------:|:------------:|:-------------:|
| 1 | 全球 | `OnActionExecuting` |
| 2 | 控制器或 Razor 頁面| `OnActionExecuting` |
| 3 | 方法 | `OnActionExecuting` |
| 4 | 方法 | `OnActionExecuted` |
| 5 | 控制器或 Razor 頁面 | `OnActionExecuted` |
| 6 | 全球 | `OnActionExecuted` |

### <a name="controller-level-filters"></a>控制器層級篩選

繼承自 <xref:Microsoft.AspNetCore.Mvc.Controller> 基底類別的每個控制器都會包含 [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*)、[Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)，以及 [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` 方法。 這些方法會：

* 包裝針對指定動作執行的篩選條件。
* 系統會在呼叫任何動作的篩選條件之前呼叫 `OnActionExecuting`。
* 系統會在呼叫所有動作篩選條件之後呼叫 `OnActionExecuted`。
* 系統會在呼叫任何動作的篩選條件之前呼叫 `OnActionExecutionAsync`。 在動作方法之後執行 `next` 後，位於篩選條件中的程式碼。

例如，在下載範例中，`MySampleActionFilter` 會在啟動時全域套用。

`TestController`：

* 將 `SampleActionFilterAttribute` （ `[SampleActionFilter]` ）套用至 `FilterTest2` 動作。
* 覆寫 `OnActionExecuting` 和 `OnActionExecuted`。

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

[!INCLUDE[](~/includes/MyDisplayRouteInfo.md)]

<!-- test via  webBuilder.UseStartup<Startup>(); -->

瀏覽至 `https://localhost:5001/Test2/FilterTest2` 執行下列程式碼：

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

控制器層級篩選會將[Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17)屬性設定為 `int.MinValue` 。 在套用至方法的篩選器之後，**無法**將控制器層級篩選設定為執行。 下一節將說明順序。

如需 Razor 頁面，請參閱藉[由覆 Razor 寫篩選方法來執行頁面篩選](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods)。

### <a name="overriding-the-default-order"></a>覆寫預設順序

可以藉由實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter> 來覆寫預設執行序列。 `IOrderedFilter` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> 屬性，其優先順序會高於範圍以決定執行順序。 具有較低 `Order` 值的篩選條件：

* 在具有較高 `Order` 值的篩選條件之前執行「之前」** 程式碼。
* 在具有較高 `Order` 值的篩選條件之後執行「之後」** 程式碼。

`Order`屬性是以「函式參數設定：

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test3Controller.cs?name=snippet)]

請考慮下列控制器中的兩個動作篩選準則：

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet)]

全域篩選器會在中新增 `StartUp.ConfigureServices` ：

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

3個篩選準則會依照下列循序執行：

* `Test2Controller.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `MyAction2FilterAttribute.OnActionExecuting`
      * `Test2Controller.FilterTest2`
    * `MySampleActionFilter.OnActionExecuted`
  * `MyAction2FilterAttribute.OnResultExecuting`
* `Test2Controller.OnActionExecuted`

`Order` 屬性在決定篩選條件執行的順序時會覆寫範圍。 篩選條件會先依照順序排序，然後使用範圍來打破僵局。 所有內建的篩選條件都會實作 `IOrderedFilter` 並將預設 `Order` 值設為 0。 如先前所述，控制器層級篩選會針對內建篩選準則將[order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17)屬性設定為 `int.MinValue` ，除非 `Order` 設定為非零值，否則範圍會決定順序。

在上述程式碼中， `MySampleActionFilter` 具有全域範圍，因此它會在 `MyAction2FilterAttribute` 具有控制器範圍的之前執行。 若要 `MyAction2FilterAttribute` 先進行執行，請將順序設定為 `int.MinValue` ：

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet2)]

若要先執行全域篩選器 `MySampleActionFilter` ，請將設定 `Order` 為 `int.MinValue` ：

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder2.cs?name=snippet&highlight=6)]

## <a name="cancellation-and-short-circuiting"></a>取消和縮短

可以設定提供給篩選條件方法之 <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> 參數上的 <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> 屬性，以縮短篩選條件管線。 比方說，下列資源篩選條件可防止管線的其餘部分執行：

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

在下列程式碼中，`ShortCircuitingResourceFilter` 和 `AddHeader` 篩選條件都以 `SomeResource` 動作方法為目標。 `ShortCircuitingResourceFilter`：

* 最先執行，因為它是資源篩選條件，且 `AddHeader` 是動作篩選條件。
* 縮短管線的其餘部分。

因此，`SomeResource` 動作的 `AddHeader` 篩選條件永遠不會執行。 如果這兩個篩選條件都套用在動作方法層級，此行為也會相同，假設 `ShortCircuitingResourceFilter` 先執行的話。 `ShortCircuitingResourceFilter` 會因為其篩選器類型而先執行，或藉由明確使用 `Order` 屬性而先執行。

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

## <a name="dependency-injection"></a>相依性插入

可以依類型或執行個體新增篩選條件。 如果新增執行個體，該執行個體將用於每個要求。 如果新增類型，其將會由類型啟動。 由類型啟動的篩選條件表示：

* 系統會針對每個要求建立執行個體。
* 任何建構函式相依性都會由[相依性插入](xref:fundamentals/dependency-injection) (DI) 所填入。

實作為屬性並直接新增至控制器類別或動作方法的篩選條件，不能由[相依性插入](xref:fundamentals/dependency-injection) (DI) 提供建構函式相依性。 DI 無法提供建構函式相依性，因為：

* 必須在提供屬性之建構函式參數的地方提供它們。 
* 這是屬性運作方式的限制。

下列篩選條件支援由 DI 所提供的建構函式相依性：

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* 在屬性上實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。

上述篩選條件可以被套用至控制器或動作方法：

DI 會提供記錄器。 不過，請避免僅針對記錄目的建立並使用篩選條件。 [內建架構記錄](xref:fundamentals/logging/index)通常便可以提供記錄所需的項目。 新增至篩選條件的記錄：

* 應該專注在篩選條件特定的商務領域考量或行為。
* **不**應該記錄動作或其他架構事件。 內建篩選記錄動作和架構事件。

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

服務篩選條件實作類型會註冊在 `ConfigureServices` 中。 <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> 會從 DI 擷取篩選條件的執行個體。

下面程式碼會說明 `AddHeaderResultServiceFilter`：

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

在下列程式碼中，會將 `AddHeaderResultServiceFilter` 新增至 DI 容器：

[!code-csharp[](./filters/3.1sample/FiltersSample/Startup.cs?name=snippet&highlight=4)]

在下列程式碼中，`ServiceFilter` 屬性會從 DI 擷取 `AddHeaderResultServiceFilter` 篩選條件的執行個體：

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

使用 `ServiceFilterAttribute` 時，設定 [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable)：

* 能提供篩選條件執行個體「可能」** 可以在其建立的要求範圍之外重複使用的提示。 ASP.NET Core 執行階段並不保證：

  * 將會建立篩選條件的單一執行個體。
  * 將不會於稍後的時間從 DI 容器重新要求篩選條件。

* 不應該搭配仰賴具有非單一資料庫存留期之服務的篩選條件使用。

 <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。 `IFilterFactory` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> 執行個體。 `CreateInstance` 會從 DI 載入指定的類型。

### <a name="typefilterattribute"></a>TypeFilterAttribute

<xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> 類似於 <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>，但其類型不會直接從 DI 容器解析。 它會使用 <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName> 來具現化類型。

由於 `TypeFilterAttribute` 類型不會直接從 DI 容器解析：

* 使用 `TypeFilterAttribute` 參考的類型不需要向 DI 容器註冊。  不過它們的相依性會由 DI 容器滿足。
* `TypeFilterAttribute` 可以選擇性地接受類型的建構函式引數。

使用 `TypeFilterAttribute` 時，設定 [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable)：
* 能提供篩選條件執行個體「可能」** 可以在其建立的要求範圍之外重複使用的提示。 ASP.NET Core 執行階段不保證將建立篩選條件的單一執行個體。

* 不應該搭配仰賴具有非單一資料庫存留期之服務的篩選條件使用。

下列範例示範如何使用 `TypeFilterAttribute` 將引數傳遞至類型：

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a>授權篩選條件

授權篩選條件：

* 是篩選條件管線內最先執行的篩選條件。
* 控制動作方法的存取。
* 有之前的方法，但沒有之後的方法。

自訂授權篩選條件需要自訂的授權架構。 最好是設定授權原則或撰寫自訂授權原則，而不要撰寫自訂篩選條件。 內建的授權篩選條件：

* 呼叫授權系統。
* 不會授權要求。

**不會**在授權篩選條件內擲回例外狀況：

* 該例外狀況將不會被處理。
* 例外狀況篩選條件將不會處理例外狀況。

請考慮在例外狀況於授權篩選條件中發生時發出挑戰。

深入了解[授權](xref:security/authorization/introduction)。

## <a name="resource-filters"></a>資源篩選條件

資源篩選條件：

* 實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> 介面。
* 執行會包裝大部分的篩選條件管線。
* 只有[授權篩選條件](#authorization-filters)會在資源篩選條件之前執行。

資源篩選條件很適合用來縮短大部分的管線。 比方說，快取篩選條件可以避免快取命中上的其餘管線。

資源篩選條件範例：

* 先前所示的[縮短資源篩選條件](#short-circuiting-resource-filter)。
* [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs) \(英文\)：

  * 防止模型繫結存取表單資料。
  * 用於大型檔案上傳，以避免將表單資料讀入記憶體。

## <a name="action-filters"></a>動作篩選條件

動作篩選準則**不會**套用至 Razor 頁面。 Razor頁面支援 <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> 。 如需詳細資訊，請參閱[ Razor 頁面的篩選方法](xref:razor-pages/filter)。

動作篩選條件：

* 實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> 介面。
* 它們執行會包圍動作方法的執行。

下列程式碼會顯示範例動作篩選條件：

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> 提供下列屬性：

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>-啟用讀取動作方法的輸入。
* <xref:Microsoft.AspNetCore.Mvc.Controller> - 提供對控制器執行個體的管理。
* <xref:System.Web.Mvc.ActionExecutingContext.Result> - 設定 `Result` 會縮短動作方法和後續動作篩選條件的執行。

在動作方法中擲回例外狀況：

* 防止執行後續篩選條件。
* 和設定 `Result` 不同，會被視為失敗而非成功結果。

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> 提供 `Controller` 與 `Result`，加上下列屬性：

* <xref:System.Web.Mvc.ActionExecutedContext.Canceled> - 如果動作執行被另一個篩選條件縮短，則為 true。
* <xref:System.Web.Mvc.ActionExecutedContext.Exception> - 如果動作或先前所執行的動作篩選條件擲回例外狀況，則為非 Null。 將此屬性設定為 Null：

  * 實際上會「處理」例外狀況。
  * 會執行 `Result`，有如它是從動作方法傳回一般。

對於 `IAsyncActionFilter`，呼叫 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> 會：

* 執行任何後續的動作篩選條件和動作方法。
* 傳回 `ActionExecutedContext`。

若要縮短，請指派 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> 給某個結果執行個體，並且不要呼叫 `next` (`ActionExecutionDelegate`)。

架構提供抽象 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>，其可被子類別化。

`OnActionExecuting` 動作篩選條件可以用來：

* 驗證模型狀態。
* 如果狀態無效，則傳回錯誤。

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

> [!NOTE]
> 以屬性標注的控制器 `[ApiController]` 會自動驗證模型狀態，並傳回400回應。 如需詳細資訊，請參閱[自動 HTTP 400 回應](xref:web-api/index#automatic-http-400-responses)。

`OnActionExecuted` 方法會在動作方法之後執行：

* 並可以透過 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> 屬性查看及操作動作的結果。
* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> 會在動作執行被另一個篩選條件縮短時被設為 true。
* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> 會在動作或後續的動作篩選條件擲回例外狀況時被設為非 Null 值。 將 `Exception` 設定為 null：

  * 實際上會「處理」例外狀況。
  * 執行 `ActionExecutedContext.Result`，彷彿它已正常地從動作方法傳回。

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a>例外狀況篩選條件

例外狀況篩選條件：

* 實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>。
* 可以用來實作常見的錯誤處理原則。

下列範例例外狀況篩選條件會使用自訂的錯誤檢視，以顯示在開發應用程式時發生的例外狀況詳細資料：

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

下列程式碼會測試例外狀況篩選準則：

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/FailingController.cs?name=snippet)]

例外狀況篩選條件：

* 沒有之前和之後的事件。
* 實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>。
* 處理在 Razor 頁面或控制器建立、[模型](xref:mvc/models/model-binding)系結、動作篩選準則或動作方法中發生的未處理例外狀況。
* **不會**攔截在資源篩選條件、結果篩選條件或 MVC 結果執行中發生的例外狀況。

若要處理例外狀況，請將 <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> 屬性設為 `true`，或撰寫回應。 這樣會阻止傳播例外狀況。 例外狀況篩選條件無法將例外狀況變成「成功」。 只有動作篩選條件可以這麼做。

例外狀況篩選條件：

* 適合用於設陷在動作內發生的例外狀況。
* 不像錯誤處理中介軟體那麼有彈性。

偏好使用中介軟體進行例外狀況處理。 只有在錯誤處理會根據所呼叫的動作方法而「不同」** 時才使用例外狀況篩選條件。 比方說，應用程式可能會有適用於 API 端點及檢視/HTML 的動作方法。 API 端點可能將錯誤資訊傳回為 JSON，而以檢視為基礎的動作則可能將錯誤頁面傳回為 HTML。

## <a name="result-filters"></a>結果篩選條件

結果篩選條件：

* 實作介面：
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter>
* 它們執行會包圍動作結果的執行。

### <a name="iresultfilter-and-iasyncresultfilter"></a>IResultFilter 和 IAsyncResultFilter

以下程式碼會顯示能新增 HTTP 標頭的結果篩選條件：

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

執行的結果類型會取決於動作。 傳回視圖的動作會在執行中包含所有 razor 處理 <xref:Microsoft.AspNetCore.Mvc.ViewResult> 。 API 方法可能在結果執行當中執行某種序列化。 深入瞭解[動作結果](xref:mvc/controllers/actions)。

只有當動作或動作篩選準則產生動作結果時，才會執行結果篩選準則。 當下列情況時，不會執行結果篩選：

* 授權篩選或資源篩選器會將管線短路。
* 例外狀況篩選條件會產生動作結果來處理例外狀況。

藉由將 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> 設為 `true`，<xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> 方法可以縮短動作結果和後續結果篩選條件的執行。 在縮短時寫入至回應物件，以避免產生空的回應。 在中擲回例外狀況 `IResultFilter.OnResultExecuting` ：

* 防止執行動作結果和後續的篩選準則。
* 會被視為失敗，而不是成功的結果。

當 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> 方法執行時，回應可能已經傳送到用戶端。 如果回應已經傳送到用戶端，就無法變更。

如果動作結果執行被另一個篩選條件縮短，則 `ResultExecutedContext.Canceled` 會被設為 `true`。

如果動作結果或後續的結果篩選條件擲回例外狀況，則 `ResultExecutedContext.Exception` 會被設為非 Null 值。 將設定 `Exception` 為 null 會有效率地處理例外狀況，並防止稍後在管線中擲回例外狀況。 並沒有可以在處理結果篩選條件中的例外狀況時，將資料寫入回應的可靠方式。 當動作結果擲回例外狀況時，如果標頭已清除至用戶端，則沒有任何可靠的機制能傳送失敗碼。

針對 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter> 而言，對 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> 上的 `await next` 的呼叫會執行任何後續的結果篩選條件和動作結果。 若要縮短，請將 [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) 設定為 `true`，並且不要呼叫 `ResultExecutionDelegate`：

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

架構提供抽象 `ResultFilterAttribute`，其可被子類別化。 先前所示的 [AddHeaderAttribute](#add-header-attribute) 類別是結果篩選條件屬性的範例。

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a>IAlwaysRunResultFilter 和 IAsyncAlwaysRunResultFilter

<xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> 介面會宣告針對所有動作結果執行的 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 實作。 這包括由產生的動作結果：

* 最少迴圈的授權篩選準則和資源篩選器。
* 例外狀況篩選條件。

例如，下列篩選一律會執行，並會在內容交涉失敗時，為動作結果 (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) 設定「422 無法處理的實體」** 狀態碼：

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a>IFilterFactory

<xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>。 因此，`IFilterFactory` 執行個體可用來在篩選條件管線中任何位置作為 `IFilterMetadata` 執行個體。 當執行階段準備要叫用篩選條件時，它會嘗試將它轉換成 `IFilterFactory`。 如果該轉換成功，系統會呼叫 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立被叫用的 `IFilterMetadata` 執行個體。 因為在應用程式啟動時不需要明確設定精確的篩選條件管線，所以這提供了具有彈性的設計。

可以使用自訂屬性實作作為另一種建立篩選條件的方法，來實作 `IFilterFactory`：

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

此篩選準則會套用至下列程式碼：

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet3&highlight=21)]

執行[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample)來測試上述程式碼：

* 叫用 F12 開發人員工具。
* 瀏覽至 `https://localhost:5001/Sample/HeaderWithFactory`。

F12 開發人員工具會顯示由範例程式碼所加入的下列回應標頭：

* **作者：**`Rick Anderson`
* **globaladdheader:** `Result filter added to MvcOptions.Filters`
* **內部：**`My header`

上述程式碼會建立 **internal:** `My header` 回應標頭。

### <a name="ifilterfactory-implemented-on-an-attribute"></a>在屬性上實作的 IFilterFactory

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

實作 `IFilterFactory` 的篩選條件很適合用於下列類型的篩選條件：

* 不需要傳遞參數。
* 有需要由 DI 滿足的建構函式相依性。

<xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。 `IFilterFactory` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> 執行個體。 `CreateInstance` 會從服務容器 (DI) 載入指定的類型。

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

下列程式碼會顯示套用 `[SampleActionFilter]` 的三種方式：

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

在上述程式碼中，使用 `[SampleActionFilter]` 來裝飾方法是套用 `SampleActionFilter` 的建議方法。

## <a name="using-middleware-in-the-filter-pipeline"></a>在篩選條件管線中使用中介軟體

資源篩選條件的運作與[中介軟體](xref:fundamentals/middleware/index)相似之處在於，它們會圍繞管線中稍後的所有項目執行。 但是篩選器與中介軟體不同之處在于它們是執行時間的一部分，這表示它們可以存取內容和結構。

若要使用中介軟體作為篩選條件，請建立一個具有 `Configure` 方法的類型，其能指定要插入到篩選條件管線的中介軟體。 以下範例會使用當地語系化中介軟體來建立某個要求目前的文化特性：

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

使用 <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> 來執行中介軟體：

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

中介軟體篩選條件與資源篩選條件在相同的篩選條件管線階段執行：模型繫結之前和管線的其餘部分之後。

## <a name="next-actions"></a>後續動作

* 請參閱[ Razor 頁面的篩選方法](xref:razor-pages/filter)。
* 若要試驗篩選準則，請[下載、測試及修改 GitHub 範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample)。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

作者：[Kirk Larkin](https://github.com/serpent5) \(英文\)、[Rick Anderson](https://twitter.com/RickAndMSFT) \(英文\)、[Tom Dykstra](https://github.com/tdykstra/) \(英文\) 及 [Steve Smith](https://ardalis.com/) \(英文\)

ASP.NET Core 中的「篩選條件」** 可讓程式碼在要求處理管線中的特定階段之前或之後執行。

內建篩選條件會處理工作，例如：

* 授權 (避免存取使用者未獲授權的資源)。
* 回應快取 (縮短要求管線，傳回快取的回應)。

可以建立自訂篩選條件來處理跨領域關注。 跨領域關注的範例包括錯誤處理、快取、設定、授權及記錄。  篩選能避免重複的程式碼。 例如，錯誤處理例外狀況篩選條件中可以合併錯誤處理。

本檔適用于 Razor 具有 views 的頁面、API 控制器和控制器。

[檢視或下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))。

## <a name="how-filters-work"></a>篩選條件如何運作

篩選條件會在「ASP.NET Core 動作引動過程管線」** 中執行，其有時也被稱為「篩選條件管線」**。  在 ASP.NET Core 之後執行的篩選條件管線會選取要執行的動作。

![要求處理會歷經其他中介軟體、路由中介軟體、動作選取和 ASP.NET Core 動作引動過程管線。 要求處理繼續往回歷經動作選取、路由中介軟體，以及各種其他中介軟體，然後才能成為傳送至用戶端的回應。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>篩選條件類型

每個篩選類型會在篩選條件管線中的不同階段執行：

* [授權篩選條件](#authorization-filters)會先執行，用來判斷使用者是否已針對要求取得授權。 如果要求未經授權，授權篩選條件便會縮短管線。

* [資源篩選器](#resource-filters)：

  * 會在授權之後執行。  
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> 可以在其餘的篩選條件管線之前執行程式碼。 例如，`OnResourceExecuting` 可以在模型繫結之前執行程式碼。
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> 可以在其餘的管線皆已完成之後執行程式碼。

* [動作篩選條件](#action-filters)可以緊接在呼叫個別動作方法之前和之後執行程式碼。 它們可以用來處理傳遞至動作的引數，和從動作傳回的結果。 頁面**不**支援動作篩選準則 Razor 。

* [例外狀況篩選條件](#exception-filters)用來將通用原則套用到在任何項目寫入回應主體之前，發生的未處理例外狀況。

* [結果篩選條件](#result-filters)可以緊接在執行個別動作結果之前和之後執行程式碼。 它們只有在動作方法執行成功時才執行。 它們適用於必須包圍檢視或格式器執行的邏輯。

下圖顯示篩選條件類型如何在篩選條件管線中互動。

![要求處理會歷經授權篩選條件、資源篩選條件、模型繫結、動作篩選條件、動作執行和動作結果轉換、例外狀況篩選條件、結果篩選條件，以及結果執行。 在送出的過程，要求只會經過結果篩選條件和資源篩選條件的處理，然後便成為傳送至用戶端的回應。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>實作

篩選條件同時支援透過不同介面定義的同步和非同步實作。

同步篩選條件可以在其管線階段之前 (`On-Stage-Executing`) 和之後 (`On-Stage-Executed`) 執行程式碼。 例如，系統會在呼叫動作方法之前呼叫 `OnActionExecuting`。 系統會在傳回動作方法之後呼叫 `OnActionExecuted`。

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

非同步篩選條件會定義 `On-Stage-ExecutionAsync` 方法：

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

在上述程式碼中，`SampleAsyncActionFilter` 具有會執行動作方法的 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)。  每個 `On-Stage-ExecutionAsync` 都會接受能執行篩選條件之管線階段的 `FilterType-ExecutionDelegate`。

### <a name="multiple-filter-stages"></a>多個篩選條件階段

可以在單一類別中實作多個篩選條件階段的介面。 例如，<xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> 類別會實作 `IActionFilter`、`IResultFilter` 與其非同步對等項目。

請實作同步**或**非同步版本的篩選條件介面，而**不要**同時實作這兩者。 執行階段會先檢查以查看篩選條件是否會實作非同步介面，如果是，便會呼叫該介面。 如果沒有，它會呼叫同步介面的方法。 如果同時在單一類別中實作非同步和同步介面，系統只會呼叫非同步方法。 使用抽象類別 (例如 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>) 時，請僅覆寫每個篩選條件類型的同步方法或非同步方法。

### <a name="built-in-filter-attributes"></a>內建篩選條件屬性

ASP.NET Core 包含內建的屬性型篩選條件，可對其進行子類別化和自訂。 例如，下列結果篩選條件會將標頭新增至回應：

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

屬性可讓篩選條件接受引數，如上述範例所示。 將 `AddHeaderAttribute` 新增至控制器或動作方法，並指定 HTTP 標頭的名稱和值：

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

有幾個篩選條件介面有對應的屬性，可用來作為自訂實作的基底類別。

篩選條件屬性：

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a>篩選條件範圍和執行的順序

篩選條件能以三個「範圍」** 之一新增至管線：

* 針對動作使用屬性。
* 針對控制器使用屬性。
* 全域地針對所有控制器和動作，如下列程式碼所示：

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

上述程式碼會使用 [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) 集合來全域地新增三個篩選條件。

### <a name="default-order-of-execution"></a>預設執行順序

當有多個*相同類型*的篩選時，範圍會決定篩選執行的預設順序。  全域篩選準則會括住類別篩選。 類別篩選準則環繞方法篩選準則。

因為篩選條件巢狀結構的原因，篩選條件的「之後」** 程式碼的執行順序會與「之前」** 程式碼相反。 篩選條件序列：

* 全域篩選條件的「之前」** 程式碼。
  * 控制器篩選條件的「之前」** 程式碼。
    * 動作方法篩選條件的「之前」** 程式碼。
    * 動作方法篩選條件的「之後」** 程式碼。
  * 控制器篩選條件的「之後」** 程式碼。
* 全域篩選條件的「之後」** 程式碼。
  
下列範例說明針對同步動作篩選條件呼叫篩選條件方法的順序。

| 順序 | 篩選條件範圍 | 篩選條件方法 |
|:--------:|:------------:|:-------------:|
| 1 | 全球 | `OnActionExecuting` |
| 2 | 控制器 | `OnActionExecuting` |
| 3 | 方法 | `OnActionExecuting` |
| 4 | 方法 | `OnActionExecuted` |
| 5 | 控制器 | `OnActionExecuted` |
| 6 | 全球 | `OnActionExecuted` |

此順序顯示：

* 方法篩選條件巢狀位於控制器篩選條件內。
* 控制器篩選條件巢狀位於全域篩選條件內。

### <a name="controller-and-razor-page-level-filters"></a>控制器和 Razor 頁面層級篩選

繼承自 <xref:Microsoft.AspNetCore.Mvc.Controller> 基底類別的每個控制器都會包含 [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*)、[Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)，以及 [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` 方法。 這些方法會：

* 包裝針對指定動作執行的篩選條件。
* 系統會在呼叫任何動作的篩選條件之前呼叫 `OnActionExecuting`。
* 系統會在呼叫所有動作篩選條件之後呼叫 `OnActionExecuted`。
* 系統會在呼叫任何動作的篩選條件之前呼叫 `OnActionExecutionAsync`。 在動作方法之後執行 `next` 後，位於篩選條件中的程式碼。

例如，在下載範例中，`MySampleActionFilter` 會在啟動時全域套用。

`TestController`：

* 將 `SampleActionFilterAttribute` （ `[SampleActionFilter]` ）套用至 `FilterTest2` 動作。
* 覆寫 `OnActionExecuting` 和 `OnActionExecuted`。

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

瀏覽至 `https://localhost:5001/Test/FilterTest2` 執行下列程式碼：

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

如需 Razor 頁面，請參閱藉[由覆 Razor 寫篩選方法來執行頁面篩選](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods)。

### <a name="overriding-the-default-order"></a>覆寫預設順序

可以藉由實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter> 來覆寫預設執行序列。 `IOrderedFilter` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> 屬性，其優先順序會高於範圍以決定執行順序。 具有較低 `Order` 值的篩選條件：

* 在具有較高 `Order` 值的篩選條件之前執行「之前」** 程式碼。
* 在具有較高 `Order` 值的篩選條件之後執行「之後」** 程式碼。

`Order` 屬性可以搭配建構函式參數設定：

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

請考慮先前範例所示的同樣 3 個動作篩選條件。 如果控制器和全域篩選條件的 `Order` 屬性被個別設定為 1 和 2，執行順序將會反轉。

| 順序 | 篩選條件範圍 | `Order` 屬性 | 篩選條件方法 |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | 方法 | 0 | `OnActionExecuting` |
| 2 | 控制器 | 1  | `OnActionExecuting` |
| 3 | 全球 | 2  | `OnActionExecuting` |
| 4 | 全球 | 2  | `OnActionExecuted` |
| 5 | 控制器 | 1  | `OnActionExecuted` |
| 6 | 方法 | 0  | `OnActionExecuted` |

`Order` 屬性在決定篩選條件執行的順序時會覆寫範圍。 篩選條件會先依照順序排序，然後使用範圍來打破僵局。 所有內建的篩選條件都會實作 `IOrderedFilter` 並將預設 `Order` 值設為 0。 針對內建篩選條件，除非將 `Order` 設定為非零值，否則範圍會決定順序。

## <a name="cancellation-and-short-circuiting"></a>取消和縮短

可以設定提供給篩選條件方法之 <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> 參數上的 <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> 屬性，以縮短篩選條件管線。 比方說，下列資源篩選條件可防止管線的其餘部分執行：

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

在下列程式碼中，`ShortCircuitingResourceFilter` 和 `AddHeader` 篩選條件都以 `SomeResource` 動作方法為目標。 `ShortCircuitingResourceFilter`：

* 最先執行，因為它是資源篩選條件，且 `AddHeader` 是動作篩選條件。
* 縮短管線的其餘部分。

因此，`SomeResource` 動作的 `AddHeader` 篩選條件永遠不會執行。 如果這兩個篩選條件都套用在動作方法層級，此行為也會相同，假設 `ShortCircuitingResourceFilter` 先執行的話。 `ShortCircuitingResourceFilter` 會因為其篩選器類型而先執行，或藉由明確使用 `Order` 屬性而先執行。

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>相依性插入

可以依類型或執行個體新增篩選條件。 如果新增執行個體，該執行個體將用於每個要求。 如果新增類型，其將會由類型啟動。 由類型啟動的篩選條件表示：

* 系統會針對每個要求建立執行個體。
* 任何建構函式相依性都會由[相依性插入](xref:fundamentals/dependency-injection) (DI) 所填入。

實作為屬性並直接新增至控制器類別或動作方法的篩選條件，不能由[相依性插入](xref:fundamentals/dependency-injection) (DI) 提供建構函式相依性。 DI 無法提供建構函式相依性，因為：

* 必須在提供屬性之建構函式參數的地方提供它們。 
* 這是屬性運作方式的限制。

下列篩選條件支援由 DI 所提供的建構函式相依性：

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* 在屬性上實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。

上述篩選條件可以被套用至控制器或動作方法：

DI 會提供記錄器。 不過，請避免僅針對記錄目的建立並使用篩選條件。 [內建架構記錄](xref:fundamentals/logging/index)通常便可以提供記錄所需的項目。 新增至篩選條件的記錄：

* 應該專注在篩選條件特定的商務領域考量或行為。
* **不**應該記錄動作或其他架構事件。 內建篩選條件能記錄動作和架構事件。

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

服務篩選條件實作類型會註冊在 `ConfigureServices` 中。 <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> 會從 DI 擷取篩選條件的執行個體。

下面程式碼會說明 `AddHeaderResultServiceFilter`：

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

在下列程式碼中，會將 `AddHeaderResultServiceFilter` 新增至 DI 容器：

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

在下列程式碼中，`ServiceFilter` 屬性會從 DI 擷取 `AddHeaderResultServiceFilter` 篩選條件的執行個體：

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

使用 `ServiceFilterAttribute` 時，設定 [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable)：

* 能提供篩選條件執行個體「可能」** 可以在其建立的要求範圍之外重複使用的提示。 ASP.NET Core 執行階段並不保證：

  * 將會建立篩選條件的單一執行個體。
  * 將不會於稍後的時間從 DI 容器重新要求篩選條件。

* 不應該搭配仰賴具有非單一資料庫存留期之服務的篩選條件使用。

 <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。 `IFilterFactory` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> 執行個體。 `CreateInstance` 會從 DI 載入指定的類型。

### <a name="typefilterattribute"></a>TypeFilterAttribute

<xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> 類似於 <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>，但其類型不會直接從 DI 容器解析。 它會使用 <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName> 來具現化類型。

由於 `TypeFilterAttribute` 類型不會直接從 DI 容器解析：

* 使用 `TypeFilterAttribute` 參考的類型不需要向 DI 容器註冊。  不過它們的相依性會由 DI 容器滿足。
* `TypeFilterAttribute` 可以選擇性地接受類型的建構函式引數。

使用 `TypeFilterAttribute` 時，設定 [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable)：
* 能提供篩選條件執行個體「可能」** 可以在其建立的要求範圍之外重複使用的提示。 ASP.NET Core 執行階段不保證將建立篩選條件的單一執行個體。

* 不應該搭配仰賴具有非單一資料庫存留期之服務的篩選條件使用。

下列範例示範如何使用 `TypeFilterAttribute` 將引數傳遞至類型：

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a>授權篩選條件

授權篩選條件：

* 是篩選條件管線內最先執行的篩選條件。
* 控制動作方法的存取。
* 有之前的方法，但沒有之後的方法。

自訂授權篩選條件需要自訂的授權架構。 最好是設定授權原則或撰寫自訂授權原則，而不要撰寫自訂篩選條件。 內建的授權篩選條件：

* 呼叫授權系統。
* 不會授權要求。

**不會**在授權篩選條件內擲回例外狀況：

* 該例外狀況將不會被處理。
* 例外狀況篩選條件將不會處理例外狀況。

請考慮在例外狀況於授權篩選條件中發生時發出挑戰。

深入了解[授權](xref:security/authorization/introduction)。

## <a name="resource-filters"></a>資源篩選條件

資源篩選條件：

* 實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> 介面。
* 執行會包裝大部分的篩選條件管線。
* 只有[授權篩選條件](#authorization-filters)會在資源篩選條件之前執行。

資源篩選條件很適合用來縮短大部分的管線。 比方說，快取篩選條件可以避免快取命中上的其餘管線。

資源篩選條件範例：

* 先前所示的[縮短資源篩選條件](#short-circuiting-resource-filter)。
* [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs) \(英文\)：

  * 防止模型繫結存取表單資料。
  * 用於大型檔案上傳，以避免將表單資料讀入記憶體。

## <a name="action-filters"></a>動作篩選條件

> [!IMPORTANT]
> 動作篩選準則**不會**套用至 Razor 頁面。 Razor頁面支援 <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> 。 如需詳細資訊，請參閱[ Razor 頁面的篩選方法](xref:razor-pages/filter)。

動作篩選條件：

* 實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> 介面。
* 它們執行會包圍動作方法的執行。

下列程式碼會顯示範例動作篩選條件：

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> 提供下列屬性：

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - 讓針對某個動作方法的輸入可以被讀取。
* <xref:Microsoft.AspNetCore.Mvc.Controller> - 提供對控制器執行個體的管理。
* <xref:System.Web.Mvc.ActionExecutingContext.Result> - 設定 `Result` 會縮短動作方法和後續動作篩選條件的執行。

在動作方法中擲回例外狀況：

* 防止執行後續篩選條件。
* 和設定 `Result` 不同，會被視為失敗而非成功結果。

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> 提供 `Controller` 與 `Result`，加上下列屬性：

* <xref:System.Web.Mvc.ActionExecutedContext.Canceled> - 如果動作執行被另一個篩選條件縮短，則為 true。
* <xref:System.Web.Mvc.ActionExecutedContext.Exception> - 如果動作或先前所執行的動作篩選條件擲回例外狀況，則為非 Null。 將此屬性設定為 Null：

  * 實際上會「處理」例外狀況。
  * 會執行 `Result`，有如它是從動作方法傳回一般。

對於 `IAsyncActionFilter`，呼叫 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> 會：

* 執行任何後續的動作篩選條件和動作方法。
* 傳回 `ActionExecutedContext`。

若要縮短，請指派 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> 給某個結果執行個體，並且不要呼叫 `next` (`ActionExecutionDelegate`)。

架構提供抽象 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>，其可被子類別化。

`OnActionExecuting` 動作篩選條件可以用來：

* 驗證模型狀態。
* 如果狀態無效，則傳回錯誤。

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

`OnActionExecuted` 方法會在動作方法之後執行：

* 並可以透過 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> 屬性查看及操作動作的結果。
* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> 會在動作執行被另一個篩選條件縮短時被設為 true。
* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> 會在動作或後續的動作篩選條件擲回例外狀況時被設為非 Null 值。 將 `Exception` 設定為 null：

  * 實際上會「處理」例外狀況。
  * 執行 `ActionExecutedContext.Result`，彷彿它已正常地從動作方法傳回。

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a>例外狀況篩選條件

例外狀況篩選條件：

* 實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>。 
* 可以用來實作常見的錯誤處理原則。

下列範例例外狀況篩選條件會使用自訂的錯誤檢視，以顯示在開發應用程式時發生的例外狀況詳細資料：

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

例外狀況篩選條件：

* 沒有之前和之後的事件。
* 實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>。
* 處理在 Razor 頁面或控制器建立、[模型](xref:mvc/models/model-binding)系結、動作篩選準則或動作方法中發生的未處理例外狀況。
* **不會**攔截在資源篩選條件、結果篩選條件或 MVC 結果執行中發生的例外狀況。

若要處理例外狀況，請將 <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> 屬性設為 `true`，或撰寫回應。 這樣會阻止傳播例外狀況。 例外狀況篩選條件無法將例外狀況變成「成功」。 只有動作篩選條件可以這麼做。

例外狀況篩選條件：

* 適合用於設陷在動作內發生的例外狀況。
* 不像錯誤處理中介軟體那麼有彈性。

偏好使用中介軟體進行例外狀況處理。 只有在錯誤處理會根據所呼叫的動作方法而「不同」** 時才使用例外狀況篩選條件。 比方說，應用程式可能會有適用於 API 端點及檢視/HTML 的動作方法。 API 端點可能將錯誤資訊傳回為 JSON，而以檢視為基礎的動作則可能將錯誤頁面傳回為 HTML。

## <a name="result-filters"></a>結果篩選條件

結果篩選條件：

* 實作介面：
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter>
* 它們執行會包圍動作結果的執行。

### <a name="iresultfilter-and-iasyncresultfilter"></a>IResultFilter 和 IAsyncResultFilter

以下程式碼會顯示能新增 HTTP 標頭的結果篩選條件：

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

執行的結果類型會取決於動作。 傳回檢視的動作會在執行中的 <xref:Microsoft.AspNetCore.Mvc.ViewResult> 裡包含處理中的所有 Razor。 API 方法可能在結果執行當中執行某種序列化。 深入瞭解[動作結果](xref:mvc/controllers/actions)。

只有當動作或動作篩選準則產生動作結果時，才會執行結果篩選準則。 當下列情況時，不會執行結果篩選：

* 授權篩選或資源篩選器會將管線短路。
* 例外狀況篩選條件會產生動作結果來處理例外狀況。

藉由將 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> 設為 `true`，<xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> 方法可以縮短動作結果和後續結果篩選條件的執行。 在縮短時寫入至回應物件，以避免產生空的回應。 在 `IResultFilter.OnResultExecuting` 中擲回例外狀況會：

* 導致無法執行動作結果和後續的篩選條件。
* 視為失敗，而不是成功的結果。

當 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> 方法執行時，回應可能已經傳送到用戶端。 如果回應已傳送給用戶端，則無法進一步變更。

如果動作結果執行被另一個篩選條件縮短，則 `ResultExecutedContext.Canceled` 會被設為 `true`。

如果動作結果或後續的結果篩選條件擲回例外狀況，則 `ResultExecutedContext.Exception` 會被設為非 Null 值。 將 `Exception` 設為 Null 實際上會「處理」例外狀況，並且使 ASP.NET Core 稍後不會在管線中重新擲回例外狀況。 並沒有可以在處理結果篩選條件中的例外狀況時，將資料寫入回應的可靠方式。 當動作結果擲回例外狀況時，如果標頭已清除至用戶端，則沒有任何可靠的機制能傳送失敗碼。

針對 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter> 而言，對 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> 上的 `await next` 的呼叫會執行任何後續的結果篩選條件和動作結果。 若要縮短，請將 [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) 設定為 `true`，並且不要呼叫 `ResultExecutionDelegate`：

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

架構提供抽象 `ResultFilterAttribute`，其可被子類別化。 先前所示的 [AddHeaderAttribute](#add-header-attribute) 類別是結果篩選條件屬性的範例。

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a>IAlwaysRunResultFilter 和 IAsyncAlwaysRunResultFilter

<xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> 介面會宣告針對所有動作結果執行的 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 實作。 這包括由產生的動作結果：

* 最少迴圈的授權篩選準則和資源篩選器。
* 例外狀況篩選條件。

例如，下列篩選一律會執行，並會在內容交涉失敗時，為動作結果 (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) 設定「422 無法處理的實體」** 狀態碼：

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a>IFilterFactory

<xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>。 因此，`IFilterFactory` 執行個體可用來在篩選條件管線中任何位置作為 `IFilterMetadata` 執行個體。 當執行階段準備要叫用篩選條件時，它會嘗試將它轉換成 `IFilterFactory`。 如果該轉換成功，系統會呼叫 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立被叫用的 `IFilterMetadata` 執行個體。 因為在應用程式啟動時不需要明確設定精確的篩選條件管線，所以這提供了具有彈性的設計。

可以使用自訂屬性實作作為另一種建立篩選條件的方法，來實作 `IFilterFactory`：

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

上述程式碼可以透過執行[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) \(英文\) 來進行測試：

* 叫用 F12 開發人員工具。
* 瀏覽至 `https://localhost:5001/Sample/HeaderWithFactory`。

F12 開發人員工具會顯示由範例程式碼所加入的下列回應標頭：

* **作者：**`Joe Smith`
* **globaladdheader:** `Result filter added to MvcOptions.Filters`
* **內部：**`My header`

上述程式碼會建立 **internal:** `My header` 回應標頭。

### <a name="ifilterfactory-implemented-on-an-attribute"></a>在屬性上實作的 IFilterFactory

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

實作 `IFilterFactory` 的篩選條件很適合用於下列類型的篩選條件：

* 不需要傳遞參數。
* 有需要由 DI 滿足的建構函式相依性。

<xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。 `IFilterFactory` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> 執行個體。 `CreateInstance` 會從服務容器 (DI) 載入指定的類型。

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

下列程式碼會顯示套用 `[SampleActionFilter]` 的三種方式：

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

在上述程式碼中，使用 `[SampleActionFilter]` 來裝飾方法是套用 `SampleActionFilter` 的建議方法。

## <a name="using-middleware-in-the-filter-pipeline"></a>在篩選條件管線中使用中介軟體

資源篩選條件的運作與[中介軟體](xref:fundamentals/middleware/index)相似之處在於，它們會圍繞管線中稍後的所有項目執行。 但篩選條件與中介軟體不同之處在於它們是 ASP.NET Core 執行階段的一部分，這表示它們能存取 ASP.NET Core 內容和建構。

若要使用中介軟體作為篩選條件，請建立一個具有 `Configure` 方法的類型，其能指定要插入到篩選條件管線的中介軟體。 以下範例會使用當地語系化中介軟體來建立某個要求目前的文化特性：

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

使用 <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> 來執行中介軟體：

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

中介軟體篩選條件與資源篩選條件在相同的篩選條件管線階段執行：模型繫結之前和管線的其餘部分之後。

## <a name="next-actions"></a>後續動作

* 請參閱[ Razor 頁面的篩選方法](xref:razor-pages/filter)。
* 若要試驗篩選準則，請[下載、測試及修改 GitHub 範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)。

::: moniker-end
