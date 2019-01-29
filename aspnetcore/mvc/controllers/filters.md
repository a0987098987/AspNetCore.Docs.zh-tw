---
title: ASP.NET Core 中的篩選條件
author: ardalis
description: 深入了解「篩選條件」的運作方式，以及如何在 ASP.NET Core MVC 中使用它們。
ms.author: riande
ms.custom: mvc
ms.date: 1/15/2019
uid: mvc/controllers/filters
ms.openlocfilehash: fe3082481b51c968fd361dbcc9553c4e35a36f2a
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444346"
---
# <a name="filters-in-aspnet-core"></a>ASP.NET Core 中的篩選條件

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Tom Dykstra](https://github.com/tdykstra/) 以及 [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC 中的「篩選條件」可讓您在要求處理管線的特定階段之前或之後執行程式碼。

 內建篩選條件會處理工作，例如：

 * 授權 (避免存取使用者未獲授權的資源)。
 * 確定所有要求都使用 HTTPS。
 * 回應快取 (縮短要求管線，傳回快取的回應)。 

可以建立自訂篩選條件來處理跨領域關注。 篩選條件可以避免在動作之間複製程式碼。 例如，錯誤處理例外狀況篩選條件中可以合併錯誤處理。

[從 GitHub 檢視或下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)。

## <a name="how-filters-work"></a>篩選條件如何運作

篩選條件在「MVC 動作引動過程管線」中執行，這有時又稱為「篩選條件管線」。  MVC 之後的篩選條件管線執行會選取要執行的動作。

![要求處理會歷經其他中介軟體、路由中介軟體、動作選取和 MVC 動作引動過程管線。 要求處理繼續往回歷經動作選取、路由中介軟體，以及各種其他中介軟體，然後才能成為傳送至用戶端的回應。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>篩選條件類型

每個篩選類型會在篩選條件管線中的不同階段執行。

* [授權篩選條件](#authorization-filters)會先執行，用來判斷目前使用者是否被授權提出目前的要求。 如果要求未經授權，它們可以縮短管線。 

* [資源篩選條件](#resource-filters)是授權之後最先處理要求的。  它們可以在篩選條件管線的其餘部分之前執行程式碼，以及在管線的其餘部分完成之後執行程式碼。 基於效能的考量，它們適合用來實作快取或以其他方式縮短篩選條件管線。 它們在模型繫結之前執行，因此可能會影響模型繫結。

* [動作篩選條件](#action-filters)可以緊接在呼叫個別動作方法之前和之後執行程式碼。 它們可以用來處理傳遞至動作的引數，和從動作傳回的結果。 Razor Pages 中不支援動作篩選條件。

* [例外狀況篩選條件](#exception-filters)用來將通用原則套用到在任何項目寫入回應主體之前，發生的未處理例外狀況。

* [結果篩選條件](#result-filters)可以緊接在執行個別動作結果之前和之後執行程式碼。 它們只有在動作方法執行成功時才執行。 它們適用於必須包圍檢視或格式器執行的邏輯。

下圖顯示這些篩選條件類型如何在篩選條件管線中互動。

![要求處理會歷經授權篩選條件、資源篩選條件、模型繫結、動作篩選條件、動作執行和動作結果轉換、例外狀況篩選條件、結果篩選條件，以及結果執行。 在送出的過程，要求只會經過結果篩選條件和資源篩選條件的處理，然後便成為傳送至用戶端的回應。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>實作

篩選條件同時支援透過不同介面定義的同步和非同步實作。 

同步篩選條件可以在管線階段之前和之後執行程式碼，它們定義 On*Stage*Executing 以及 On*Stage*Executed 方法。 例如，`OnActionExecuting` 會在呼叫動作方法之前呼叫，而 `OnActionExecuted` 則在動作方法傳回之後呼叫。

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet1)]

非同步篩選條件會定義單一的 On*Stage*ExecutionAsync 方法。 這個方法會接受 *FilterType*ExecutionDelegate 委派，它會執行篩選條件的管線階段。 例如，`ActionExecutionDelegate` 會呼叫動作方法或下一個動作篩選條件，而且您可以在呼叫之前和之後執行程式碼。

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

您可以為單一類別中的多個篩選條件階段實作介面。 例如，<xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> 類別會實作 `IActionFilter`、`IResultFilter` 與其非同步對等項目。

> [!NOTE]
> 請實作同步**或**非同步版本的篩選條件介面，但不要兩者同時實作。 架構會先檢查以查看篩選條件是否實作非同步介面，如果是的話，便呼叫該介面。 如果沒有，它會呼叫同步介面的方法。 如果您要在一個類別上同時實作兩種介面，則只會呼叫非同步方法。 使用抽象類別時，例如 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>，您只會覆寫同步方法或每個篩選條件類型的非同步方法。

### <a name="ifilterfactory"></a>IFilterFactory
[IFilterFactory](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory) 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>。 因此，`IFilterFactory` 執行個體可用來在篩選條件管線中任何位置作為 `IFilterMetadata` 執行個體。 當架構準備要叫用篩選條件時，會嘗試將它轉換成 `IFilterFactory`。 如果該轉換成功，則會呼叫 [CreateInstance](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory.createinstance) 方法來建立將被叫用的 `IFilterMetadata` 執行個體。 因為在應用程式啟動時不需要明確設定精確的篩選條件管線，所以這提供了具有彈性的設計。

您可以對您自己的屬性實作，實作 `IFilterFactory` 以作為另一種建立篩選條件的方法：

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>內建篩選條件屬性

架構包含內建的屬性型篩選條件，您可以進行子分類和自訂。 例如，下列結果篩選條件會將標頭新增至回應。

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

屬性可讓篩選條件接受引數，如上述範例所示。 您會將此屬性新增至控制器或動作方法，並指定 HTTP 標頭的名稱和值：

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

`Index` 動作的結果如下所示 - 回應標頭顯示在右下方。

![顯示回應標頭的 Microsoft Edge 開發人員工具，包括作者 Steve Smith @ardalis](filters/_static/add-header.png)

有幾個篩選條件介面有對應的屬性，可用來作為自訂實作的基底類別。

篩選條件屬性：

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

[本文稍後](#dependency-injection)將說明 `TypeFilterAttribute` 和 `ServiceFilterAttribute`。

## <a name="filter-scopes-and-order-of-execution"></a>篩選條件範圍和執行的順序

篩選條件可以新增至管線的三個「範圍」之一。 您可以使用屬性將篩選條件新增至特定的動作方法或控制器類別。 或者您可以為所有控制器和動作全域地註冊篩選條件。 藉由將篩選條件新增至 `ConfigureServices` 中的 `MvcOptions.Filters` 集合，即可全域新增篩選條件：

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>預設執行順序

當管線的特定階段有多個篩選條件時，範圍會決定篩選條件的預設執行順序。  全域篩選條件會圍繞類別篩選條件，後者又圍繞方法篩選條件。 這有時候稱為「俄羅斯娃娃」巢狀結構，因為範圍的每次增加都圍繞著前一個範圍，就像[俄羅斯娃娃](https://wikipedia.org/wiki/Matryoshka_doll)一樣。 您通常不必明確決定順序，即可得到想要的覆寫行為。

因為這個巢狀結構，篩選條件的「之後」程式碼，執行的順序會與「之前」程式碼相反。 順序看起來像這樣：

* 全域套用的篩選條件「之前」程式碼
  * 套用至控制器的篩選條件「之前」程式碼
    * 套用至動作方法的篩選條件「之前」程式碼
    * 套用至動作方法的篩選條件「之後」程式碼
  * 套用至控制器的篩選條件「之後」程式碼
* 全域套用的篩選條件「之後」程式碼
  
下列範例說明同步動作篩選條件的篩選條件方法呼叫順序。

| 序列 | 篩選條件範圍 | 篩選條件方法 |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | 控制器 | `OnActionExecuting` |
| 3 | 方法 | `OnActionExecuting` |
| 4 | 方法 | `OnActionExecuted` |
| 5 | 控制器 | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

此順序顯示：

* 方法篩選條件巢狀位於控制器篩選條件內。
* 控制器篩選條件巢狀位於全域篩選條件內。 

換句話說，如果您在非同步篩選條件的 On*Stage*ExecutionAsync 方法內，具有較緊密範圍的所有篩選條件會在您的程式碼在堆疊上時執行。

> [!NOTE]
> 繼承自 `Controller` 基底類別的每個控制器，都包含 `OnActionExecuting` 和 `OnActionExecuted` 方法。 這些方法會包裝針對指定動作執行的篩選條件：`OnActionExecuting` 在任何篩選條件之前呼叫，而 `OnActionExecuted` 則在所有篩選條件之後呼叫。

### <a name="overriding-the-default-order"></a>覆寫預設順序

您可以藉由實作 `IOrderedFilter` 來覆寫預設執行序列。 此介面會公開 `Order` 屬性，其優先順序高於範圍，可決定執行順序。 `Order` 值較低的篩選條件，會在 `Order` 值較高的篩選條件之前執行其「之前」程式碼。 `Order` 值較低的篩選條件，會在 `Order` 值較高的篩選條件之後執行其「之後」程式碼。 您可以使用建構函式參數來設定 `Order` 屬性：

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

如果您擁有相同 3 個顯示在前面範例中的動作篩選條件，但將控制器的 `Order` 屬性和全域篩選條件分別設到 1 和 2，則執行順序會反轉。

| 序列 | 篩選條件範圍 | `Order` 屬性 | 篩選條件方法 |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | 方法 | 0 | `OnActionExecuting` |
| 2 | 控制器 | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | 控制器 | 1  | `OnActionExecuted` |
| 6 | 方法 | 0  | `OnActionExecuted` |

`Order` 屬性在決定篩選條件執行的順序時以範圍取勝。 篩選條件會先依照順序排序，然後使用範圍來打破僵局。 所有內建的篩選條件都會實作 `IOrderedFilter` 並將預設 `Order` 值設為 0。 對於內建篩選條件，除非您將 `Order` 設定為非零值，否則範圍決定了順序。

## <a name="cancellation-and-short-circuiting"></a>取消和縮短

您可以設定提供給篩選條件方法之 `context` 參數上的 `Result` 屬性，以在任何時間點縮短篩選條件管線。 比方說，下列資源篩選條件可防止管線的其餘部分執行。

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

在下列程式碼中，`ShortCircuitingResourceFilter` 和 `AddHeader` 篩選條件都以 `SomeResource` 動作方法為目標。 `ShortCircuitingResourceFilter`：

* 最先執行，因為它是資源篩選條件，且 `AddHeader` 是動作篩選條件。
* 縮短管線的其餘部分。

因此，`SomeResource` 動作的 `AddHeader` 篩選條件永遠不會執行。 如果這兩個篩選條件都套用在動作方法層級，此行為也會相同，假設 `ShortCircuitingResourceFilter` 先執行的話。 `ShortCircuitingResourceFilter` 會因為其篩選器類型而先執行，或藉由明確使用 `Order` 屬性而先執行。

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>相依性插入

可以依類型或執行個體新增篩選條件。 如果您新增執行個體，該執行個體將用於每個要求。 如果您新增類型，它將會由類型啟動，意思是會為每個要求建立執行個體，並且將會藉由[相依性插入](../../fundamentals/dependency-injection.md)(DI) 填入任何建構函式相依性。 依類型新增篩選條件相當於 `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`。

實作為屬性並直接新增至控制器類別或動作方法的篩選條件，不能由[相依性插入](../../fundamentals/dependency-injection.md) (DI) 提供建構函式相依性。 這是因為屬性都必須在套用它們的地方提供其建構函式參數。 這是屬性運作方式的限制。

如果您的篩選條件有必須從 DI 存取的相依性，會有數種支援的方法。 您可以使用下列其中一項將篩選條件套用至類別或動作方法：

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* 在您的屬性上實作 `IFilterFactory`

> [!NOTE]
> 您可能想要從 DI 取得的一種相依性是記錄器。 不過，不需要單純為了記錄便建立和使用篩選條件，因為[內建的架構記錄功能](xref:fundamentals/logging/index)可能已提供您需要的功能。 如果您要將記錄新增至您的篩選條件，它應該專注於商務網域考量或您篩選條件的特定行為，而不是 MVC 動作或其他架構事件。

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

在 DI 中註冊服務篩選條件實作類型。 `ServiceFilterAttribute` 會從 DI 擷取篩選條件的執行個體。 將 `ServiceFilterAttribute` 新增至 `Startup.ConfigureServices` 的容器中，並在 `[ServiceFilter]` 屬性參考它：

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

使用 `ServiceFilterAttribute` 時，設定 `IsReusable` 是一個提示，篩選條件執行個體「可能」會在其建立的要求範圍之外重複使用。 該架構不保證將在稍後的某個時間點建立篩選條件的單一執行個體，或者不會從 DI 容器重新要求篩選條件。 在使用仰賴具有非單一存留期之服務的篩選條件時，請避免使用 `IsReusable`。

使用 `ServiceFilterAttribute` 而不註冊篩選條件類型會導致例外狀況：

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute` 會實作 `IFilterFactory`。 `IFilterFactory` 會公開 `CreateInstance` 方法來建立 `IFilterMetadata` 執行個體。 `CreateInstance` 方法會從服務容器 (DI) 載入指定的類型。

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` 類似於 `ServiceFilterAttribute`，但其類型不會直接從 DI 容器解析。 它會使用 `Microsoft.Extensions.DependencyInjection.ObjectFactory` 來具現化類型。

由於此差異：

* 使用 `TypeFilterAttribute` 參考的類型不需要先向容器註冊。  它們的相依性由容器滿足。 
* `TypeFilterAttribute` 可以選擇性地接受類型的建構函式引數。

使用 `TypeFilterAttribute` 時，設定 `IsReusable` 是一個提示，篩選條件執行個體「可能」會在其建立的要求範圍之外重複使用。 該架構不保證將建立篩選條件的單一執行個體。 在使用仰賴具有非單一存留期之服務的篩選條件時，請避免使用 `IsReusable`。

下列範例示範如何使用 `TypeFilterAttribute` 將引數傳遞至類型：

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

### <a name="ifilterfactory-implemented-on-your-attribute"></a>在您屬性上實作的 IFilterFactory

如果您的篩選條件：

* 不需要任何引數。
* 有需要由 DI 滿足的建構函式相依性。

您可以在類別和方法上使用自己的具名屬性，而非 `[TypeFilter(typeof(FilterType))]`)。 下列篩選條件顯示如何實作這點：

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

此篩選條件可以使用 `[SampleActionFilter]` 語法套用至類別或方法，而不需使用 `[TypeFilter]` 或 `[ServiceFilter]`。

## <a name="authorization-filters"></a>授權篩選條件

授權篩選條件：

* 控制動作方法的存取。
* 是篩選條件管線內最先執行的篩選條件。 
* 有之前的方法，但沒有之後的方法。 

您應該唯有在撰寫自己的授權架構時才撰寫自訂授權篩選條件。 最好是設定授權原則或撰寫自訂授權原則，而不要撰寫自訂篩選條件。 內建篩選條件實作只負責呼叫授權系統。

您不應該在授權篩選條件內擲回例外狀況，因為不會處理例外狀況 (例外狀況篩選條件將不會處理它們)。 請考慮在例外狀況發生時發出挑戰。

深入了解[授權](xref:security/authorization/introduction)。

## <a name="resource-filters"></a>資源篩選條件

* 實作 `IResourceFilter` 或 `IAsyncResourceFilter` 介面，
* 它們的執行會包裝大部分的篩選條件管線。 
* 只有[授權篩選條件](#authorization-filters)會在資源篩選條件之前執行。

資源篩選條件適用於縮短要求正在進行的大部分工作。 比方說，如果回應在快取中，快取篩選條件可以避免管線的其餘部分。

稍早所示的[縮短資源篩選條件](#short-circuiting-resource-filter)是資源篩選條件的一個範例。 另一個例子是 [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs)：

* 它會導致模型繫結無法存取表單資料。 
* 它適用於大型檔案上傳，並且想要避免表單被讀入到記憶體。

## <a name="action-filters"></a>動作篩選條件

> [!IMPORTANT]
> 動作篩選條件**不會**套用到 Razor Pages。 Razor Pages 支援 <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> 與 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>。 如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。

*動作篩選條件*：

* 實作 `IActionFilter` 或 `IAsyncActionFilter` 介面。
* 它們執行會包圍動作方法的執行。

以下是動作篩選條件範例：

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> 提供下列屬性：

* `ActionArguments` - 可讓您管理動作的輸入。
* `Controller` - 可讓您管理控制器執行個體。 
* `Result` - 設定動作方法和後續動作篩選條件的縮短執行。 擲回例外狀況也會導致無法執行動作方法和後續的篩選條件，但會被視為失敗，而不是成功的結果。

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> 提供 `Controller` 與 `Result`，加上下列屬性：

* `Canceled` - 如果動作執行被另一個篩選條件縮短，則為 true。
* `Exception` - 如果動作或後續的動作篩選條件擲回例外狀況，則為非 Null。 將這個屬性設為 Null，實際上會「處理」例外狀況，並且會執行 `Result`，彷彿它已正常地從動作方法傳回。

對於 `IAsyncActionFilter`，呼叫 `ActionExecutionDelegate` 會：

* 執行任何後續的動作篩選條件和動作方法。
* 傳回 `ActionExecutedContext`。 

若要縮短，請指派 `ActionExecutingContext.Result` 給某個結果執行個體，並且不要呼叫 `ActionExecutionDelegate`。

架構提供一個抽象 `ActionFilterAttribute`，您可以進行子分類。 

您可以使用動作篩選條件來驗證模型狀態，並在狀態無效時傳回任何錯誤：

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

`OnActionExecuted` 方法在動作方法之後執行，且可以透過 `ActionExecutedContext.Result` 屬性看到及管理動作的結果。 `ActionExecutedContext.Canceled` - 如果動作執行被另一個篩選條件縮短，則將設為 true。 `ActionExecutedContext.Exception` - 如果動作或後續的動作篩選條件擲回例外狀況，則將設為非 Null 值。 將 `ActionExecutedContext.Exception` 設定為 null：

* 實際「處理」例外狀況。
* 執行 `ActionExectedContext.Result`，彷彿它已正常地從動作方法傳回。

## <a name="exception-filters"></a>例外狀況篩選條件

「例外狀況篩選條件」會實作 `IExceptionFilter` 或 `IAsyncExceptionFilter` 介面。 它們可以用來實作常見的應用程式錯誤處理原則。 

下列範例例外狀況篩選條件會使用自訂的開發人員檢視，以顯示在開發應用程式時發生的例外狀況詳細資料：

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

例外狀況篩選條件：

* 沒有之前和之後的事件。 
* 實作 `OnException` 或 `OnExceptionAsync`。 
* 處理在控制器建立、[模型繫結](../models/model-binding.md)、動作篩選條件或動作方法發生的未處理例外狀況。 
* 不會攔截在資源篩選條件、結果篩選條件或 MVC 結果執行發生的例外狀況。

若要處理例外狀況，請將 `ExceptionContext.ExceptionHandled` 屬性設為 true，或撰寫回應。 這樣會阻止傳播例外狀況。 例外狀況篩選條件無法將例外狀況變成「成功」。 只有動作篩選條件可以這麼做。

> [!NOTE]
> 在 ASP.NET Core 1.1 中，如果您將 `ExceptionHandled` 設為 true，**並且**撰寫回應，則不會傳送回應。 在該情況中，ASP.NET Core 1.0 會傳送回應，而 ASP.NET Core 1.1.2 則會回到 1.0 的行為。 如需詳細資訊，請參閱 GitHub 儲存機制中的[問題 #5594](https://github.com/aspnet/Mvc/issues/5594)。 

例外狀況篩選條件：

* 適合用於設陷 MVC 動作內發生的例外狀況。
* 不像錯誤處理中介軟體那麼有彈性。 

偏好使用中介軟體進行例外狀況處理。 只有當您需要根據選擇的 MVC 動作執行「不同的」錯誤處理時，才使用例外狀況篩選條件。 比方說，您的應用程式可能會有 API 端點及檢視/HTML 的動作方法。 API 端點可能將錯誤資訊傳回為 JSON，而以檢視為基礎的動作則可能將錯誤頁面傳回為 HTML。

`ExceptionFilterAttribute` 可以子類別化。 

## <a name="result-filters"></a>結果篩選條件

* 實作 `IResultFilter` 或 `IAsyncResultFilter` 介面。
* 它們執行會包圍動作結果的執行。 

以下是結果篩選條件的範例，它會新增 HTTP 標頭。

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

正在執行的結果類型取決於討論中的動作。 傳回檢視的 MVC 動作會在執行中的 `ViewResult` 裡包含處理中的所有 Razor。 API 方法可能在結果執行當中執行某種序列化。 深入了解[動作結果](actions.md)

動作篩選條件只會針對成功的結果執行 - 動作或動作篩選條件會執行動作結果。 當例外狀況篩選條件處理例外狀況時，不會執行結果篩選條件。

藉由將 `ResultExecutingContext.Cancel` 設為 true，`OnResultExecuting` 方法可以縮短動作結果和後續結果篩選條件的執行。 在縮短時您通常應該寫入至回應物件，以避免產生空的回應。 擲回例外狀況會：

* 導致無法執行動作結果和後續的篩選條件。
* 視為失敗，而不是成功的結果。

當 `OnResultExecuted` 方法執行時，回應可能傳送至用戶端，且無法進一步變更 (除非擲回例外狀況)。 如果動作結果執行被另一個篩選條件縮短，則 `ResultExecutedContext.Canceled` 將設為 true。

如果動作結果或後續的結果篩選條件擲回例外狀況，則 `ResultExecutedContext.Exception` 將設為非 Null 值。 將 `Exception` 設為 Null，實際上會「處理」例外狀況，並且使管線中稍後的 MVC 不會重新擲回例外狀況。 當您處理結果篩選條件的例外狀況時，您可能無法將任何資料寫入至回應。 如果動作結果在執行中途擲回，且標頭已清除至用戶端，則沒有任何可靠的機制能傳送失敗碼。

針對 `IAsyncResultFilter`，對 `ResultExecutionDelegate` 呼叫 `await next` 會執行任何後續的結果篩選條件和動作結果。 若要縮短，請將 `ResultExecutingContext.Cancel` 設為 true，且不要呼叫 `ResultExectionDelegate`。

架構提供一個抽象 `ResultFilterAttribute`，您可以進行子分類。 稍早所示的 [AddHeaderAttribute](#add-header-attribute)類別是結果篩選條件屬性的範例。

## <a name="using-middleware-in-the-filter-pipeline"></a>在篩選條件管線中使用中介軟體

資源篩選條件的運作與[中介軟體](xref:fundamentals/middleware/index)相似之處在於，它們會圍繞管線中稍後的所有項目執行。 但篩選條件與中介軟體不同之處在於，它們是 MVC 的一部分，這表示它們能存取 MVC 內容和建構。

在 ASP.NET Core 1.1 中，您可以在篩選條件管線中使用中介軟體。 如果您有需要存取 MVC 路由資料的中介軟體元件，或只應該針對特定控制器或動作執行的中介軟體元件，則可能會想要這樣做。

若要使用中介軟體作為篩選條件，請建立一個具有 `Configure` 方法的類型，指定您要插入到篩選條件管線的中介軟體。 以下是使用當地語系化中介軟體，建立要求之目前文化特性的範例：

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

然後您可以使用 `MiddlewareFilterAttribute` 針對選取的控制器或動作執行中介軟體，或是全域地執行中介軟體：

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

中介軟體篩選條件與資源篩選條件在相同的篩選條件管線階段執行：模型繫結之前和管線的其餘部分之後。

## <a name="next-actions"></a>後續動作

* 請參閱[Razor Pages 的篩選方法](xref:razor-pages/filter)
* 若要嘗試使用篩選條件，請[下載、測試及修改 Github 範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)。
