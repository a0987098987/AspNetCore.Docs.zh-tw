---
title: "篩選條件"
author: ardalis
description: "深入了解如何 * 篩選 * 工作，以及如何在 ASP.NET Core MVC 中使用它們。"
keywords: "ASP.NET Core 篩選、 mvc 篩選條件、 動作篩選條件，授權篩選條件、 資源篩選器、 結果篩選條件、 例外狀況篩選條件"
ms.author: tdykstra
manager: wpickett
ms.date: 12/12/2016
ms.topic: article
ms.assetid: 531bda08-aa5b-4471-8f08-96add22c8683
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/filters
ms.openlocfilehash: b96a70a2446cab7b1af9bd689469584868980595
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="filters"></a>篩選條件

由[Tom Dykstra](https://github.com/tdykstra/)和[Steve Smith](https://ardalis.com/)

*篩選*中 ASP.NET Core MVC 可讓您執行程式碼之前或之後的要求處理管線中的特定階段。

 內建篩選器處理工作，例如授權 （防止未獲授權使用者的資源的存取），確保所有要求都使用 HTTPS 和快取 （最少運算的快取的回應傳回的要求管線） 的回應。 

您可以建立自訂篩選來處理跨領域考量您的應用程式。 每當您想要避免在動作之間複製程式碼，篩選是解決方案。 例如，您可以合併錯誤處理程式碼例外狀況篩選條件。

[檢視或從 GitHub 下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)。

## <a name="how-do-filters-work"></a>篩選如何運作？

篩選執行*MVC 動作引動過程管線*，有時又稱為*篩選管線*。  篩選管線執行之後 MVC 選取要執行的動作。

![要求會透過其他中介軟體、 路由的中介軟體、 動作選取和 MVC 動作引動過程管線處理。 要求處理會繼續通過動作選取、 路由中介軟體，以及各種其他中介軟體，才能成為傳送至用戶端的回應。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>篩選類型

每個篩選類型是在篩選管線中的不同階段中執行。

* [授權篩選條件](#authorization-filters)會先執行，用來判斷目前使用者是否目前要求的授權。 如果是未經授權的要求，它們就可以縮短管線。 

* [資源的篩選器](#resource-filters)是優先要授權之後處理要求。  他們可以執行前篩選管線，並完成其餘的管線之後的其餘部分的程式碼。 它們很有用來實作快取或否則最短路徑篩選管線，基於效能的考量。 因為它們執行模型繫結之前，它們會很有用來影響模型繫結所需的任何項目。

* [動作篩選條件](#action-filters)立即之前和之後執行個別的動作方法會呼叫可執行程式碼。 它們可以用來處理引數傳遞至動作和動作傳回的結果。

* [例外狀況篩選條件](#exception-filters)用來將通用的原則套用到任何項目已寫入回應主體之前，發生未處理例外狀況。

* [產生篩選](#result-filters)立即之前和之後執行個別的動作結果的可執行程式碼。 這些動作方法執行成功時，才執行，而且可用於必須住檢視或格式器執行的邏輯。

下圖顯示這些篩選器型別篩選管線中的互動方式。

![要求會透過授權篩選條件、 資源篩選器、 模型繫結、 動作篩選條件、 動作執行和動作結果轉換、 例外狀況篩選條件、 結果篩選條件，以及結果執行處理。 外的方式，是只處理要求結果篩選條件和資源的篩選條件才能成為傳送至用戶端的回應。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>實作

篩選器支援透過不同的介面定義的同步和非同步實作。 選擇您要執行的工作類型而定的同步或非同步 variant。 

同步的篩選器可執行程式碼兩者上定義其管線階段前後*階段*執行以及*階段*執行方法。 例如，`OnActionExecuting`之前呼叫動作方法時，呼叫和`OnActionExecuted`動作方法傳回之後呼叫。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?highlight=6,8,13)]

非同步的篩選會定義單一上*階段*ExecutionAsync 方法。 這個方法會採用*FilterType*ExecutionDelegate 委派，它會執行篩選條件的管線階段。 例如，`ActionExecutionDelegate`呼叫動作方法，而您可以執行程式碼之前和之後呼叫它。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

您可以實作介面的單一類別中的多個篩選器階段。 例如， [Actionfilterattribut](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute)抽象類別會同時實作`IActionFilter`和`IResultFilter`，以及其非同步對等用法。

> [!NOTE]
> 實作**任一**同步或非同步版本的篩選器的介面，不可同時為兩者。 架構會先檢查以查看如果篩選條件會實作非同步介面，而且如果是的話，它會呼叫的。 如果沒有，它會呼叫同步介面的方法。 如果您是在一個類別上實作這兩個介面，則會呼叫非同步方法。 當使用抽象類別，例如[Actionfilterattribut](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute)同步的方法或其非同步方法的每個篩選類型，就會覆寫。

### <a name="ifilterfactory"></a>IFilterFactory

`IFilterFactory` 會實作 `IFilter`。 因此，`IFilterFactory`執行個體可用來當做`IFilter`篩選管線中的任何位置執行個體。 當架構準備要叫用的篩選時，會嘗試進行轉換， `IFilterFactory`。 如果該轉換成功，`CreateInstance`呼叫方法來建立`IFilter`會叫用的執行個體。 這會提供相當有彈性的設計，因為不需要應用程式啟動時明確設定精確的篩選器管線。

您可以實作`IFilterFactory`對您自己做為另一種方法來建立篩選的屬性實作：

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>內建的篩選條件屬性

此架構包含內建屬性型篩選，您可以子類別和自訂。 例如，下列結果篩選條件會將標頭加入回應。

<a name=add-header-attribute></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

屬性可讓篩選器，以接受引數，如上述範例所示。 您會將此屬性加入至控制器或動作方法，並指定名稱和 HTTP 標頭的值：

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

結果`Index`如下所示的動作-回應標頭會顯示在右下方。

![開發人員工具的 Microsoft Edge 顯示回應標頭，包括作者 Steve Smith@ardalis](filters/_static/add-header.png)

有幾個篩選條件介面有對應的屬性，可用來當作基底類別的自訂實作。

篩選屬性：

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute`和`ServiceFilterAttribute`說明，請參閱[本文稍後](#dependency-injection)。

## <a name="filter-scopes-and-order-of-execution"></a>篩選範圍和執行的順序

可以加入篩選器，在三個的其中一個管線*範圍*。 您可以使用屬性特定的動作方法或控制器類別加入篩選。 您可以將它加入至註冊全域 （適用於所有控制器和動作） 篩選器或`MvcOptions.Filters`集合`ConfigureServices`方法中的`Startup`類別：

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>預設的執行順序

當管線的特定階段如有多個篩選條件時，範圍會決定執行篩選條件的預設順序。  全域篩選括住類別篩選，又圍繞方法篩選器。 這有時候稱為"俄文布偶 「 巢狀結構，因為每次加大範圍圍繞前一個範圍，例如[巢狀布偶](https://wikipedia.org/wiki/Matryoshka_doll)。 您通常不必明確地判斷順序取得想要覆寫的行為。

因為這個巢狀結構、*之後*的篩選條件的程式碼會執行相反的順序*之前*程式碼。 順序看起來像這樣：

* *之前*全域套用的篩選器的程式碼
  * *之前*套用至控制器的篩選條件的程式碼
    * *之前*篩選器套用至動作方法的程式碼
    * *之後*篩選器套用至動作方法的程式碼
  * *之後*套用至控制器的篩選條件的程式碼
* *之後*全域套用的篩選器的程式碼
  
以下是範例，說明中的篩選方法呼叫同步動作篩選條件的順序。

| 序列 | 篩選範圍 | Filter 方法 |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | 控制器 | `OnActionExecuting` |
| 3 | 方法 | `OnActionExecuting` |
| 4 | 方法 | `OnActionExecuted` |
| 5 | 控制器 | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

此順序顯示方法篩選巢狀在控制器的篩選器，並控制站篩選巢狀在全域篩選。 若要將其放另一個方式，在非同步處理篩選條件的*階段*ExecutionAsync 方法的所有具有更緊密的範圍篩選器時執行您的程式碼在堆疊上。

> [!NOTE]
> 繼承自每個控制站`Controller`基底類別包含`OnActionExecuting`和`OnActionExecuted`方法。 這些方法會包裝執行指定動作的篩選：`OnActionExecuting`之前，任何篩選器，呼叫和`OnActionExecuted`在所有的篩選後呼叫。

### <a name="overriding-the-default-order"></a>覆寫預設順序

您可以藉由實作覆寫預設的序列執行`IOrderedFilter`。 此介面會公開`Order`優先順序高於範圍來決定執行順序的屬性。 較低的篩選器`Order`值將會有其*之前*，以較高值的篩選條件之前執行的程式碼`Order`。 較低的篩選器`Order`值將會有其*之後*，較高的篩選器之後執行的程式碼`Order`值。 您可以設定`Order`屬性使用建構函式參數：

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

如果您擁有相同 3 顯示在前面的範例，但集合中的動作篩選條件`Order`控制站和通用屬性分別篩選到 1 和 2 時，會反轉的執行順序。

| 序列 | 篩選範圍 | `Order` 屬性 | Filter 方法 |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | 方法 | 0 | `OnActionExecuting` |
| 2 | 控制器 | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | 控制器 | 1  | `OnActionExecuted` |
| 6 | 方法 | 0  | `OnActionExecuted` |

`Order`屬性更勝於範圍時決定篩選條件會執行的順序。 篩選條件會依照先順序，則範圍用於切割繫結。 所有內建的篩選器實作`IOrderedFilter`和設定預設`Order`值設為 0，因此除非您將設定領域可決定順序`Order`為非零值。

## <a name="cancellation-and-short-circuiting"></a>取消作業和短最運算

您可以縮短在任何時間點的篩選管線設定`Result`屬性`context`提供給篩選方法的參數。 比方說，下列資源的篩選條件可防止其他管線執行。

<a name=short-circuiting-resource-filter></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

下列程式碼，同時`ShortCircuitingResourceFilter`和`AddHeader`篩選目標`SomeResource`動作方法。 不過，因為`ShortCircuitingResourceFilter`會先執行 (因為它是資源的篩選器和`AddHeader`是動作篩選條件) 和 short-circuits 管線的其餘部分`AddHeader`永遠不會執行篩選`SomeResource`動作。 此行為也會相同，如果這兩個篩選套用在動作方法層級提供`ShortCircuitingResourceFilter`執行第一個 (因為其篩選器類型，或明確使用`Order`屬性，例如)。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>相依性插入

依類型或執行個體，可以加入篩選條件。 如果您新增執行個體，該執行個體將用於每個要求。 如果您加入類型，它將型別啟動，即會建立執行個體，每個要求，並將會填入任何建構函式相依性[相依性插入](../../fundamentals/dependency-injection.md)(DI)。 加入篩選條件類型相當於`filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`。

篩選器，都實作為屬性直接加入至控制器類別或動作方法不能有建構函式所提供的相依性[相依性插入](../../fundamentals/dependency-injection.md)(DI)。 這是因為屬性都必須將它們以套用所提供的建構函式參數。 這是屬性的運作方式的限制。

如果您的篩選沒有相依性，您必須從 DI 存取，有數種支援的方法。 您可以將篩選器套用至類別或動作方法，使用下列其中一項：

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory`在您的屬性上實作

> [!NOTE]
> 一種相依性，您可能想要取得從 DI 是記錄器。 不過，不需要建立及單純地進行記錄，使用篩選器，因為[內建架構記錄功能](../../fundamentals/logging.md)可能已提供您的需要。 如果您要將記錄加入至您的篩選，它應該專注於商務網域疑慮或篩選條件，而不是 MVC 動作或其他架構事件的特定行為。

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

A `ServiceFilter` DI 從擷取執行個體的篩選器。 您將篩選加入至容器中`ConfigureServices`，並將它在參考`ServiceFilter`屬性

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

使用`ServiceFilter`而不註冊例外狀況的篩選器類型的結果：

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute`實作`IFilterFactory`，而這會公開單一方法來建立`IFilter`執行個體。 如果是`ServiceFilterAttribute`、`IFilterFactory`介面的`CreateInstance`從服務容器 (DI) 載入指定的型別實作方法。

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute`非常類似於`ServiceFilterAttribute`(也會實作`IFilterFactory`)，但其類型未解析直接從 DI 容器。 相反地，它會具現化型別使用`Microsoft.Extensions.DependencyInjection.ObjectFactory`。

由於此差別，使用參考的型別`TypeFilterAttribute`不需要先註冊與容器 （但仍必須由容器及其相依性）。 此外，`TypeFilterAttribute`可以選擇性地接受問題類型的建構函式引數。 下列範例示範如何將引數傳遞至型別，使用`TypeFilterAttribute`:

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

如果您有篩選，不需要任何引數，但其中有要填入的 DI 需要的建構函式相依性，您可以使用您自己的具名的屬性類別和方法，而非`[TypeFilter(typeof(FilterType))]`)。 下列的篩選條件會顯示如何實作：

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

此篩選條件可以套用至類別或方法使用`[SampleActionFilter]`語法，而不需使用`[TypeFilter]`或`[ServiceFilter]`。

## <a name="authorization-filters"></a>授權篩選條件

*授權篩選條件*控制存取動作方法，並且會篩選管線內執行的第一個篩選條件。 它們只有方法，不同於大部分的篩選器可支援在方法前後之前。 您只應該撰寫自訂的授權篩選條件如果您要撰寫您自己的授權架構。 偏好設定授權原則，或透過撰寫自訂的篩選條件中撰寫自訂授權原則。 內建篩選器實作是只負責呼叫授權系統。

請注意，您不應該擲回例外狀況中授權篩選條件，因為不會處理此例外狀況 （例外狀況篩選條件將不會處理這類）。 相反地，發出一項挑戰，或尋找另一種方式。

深入了解[授權](../../security/authorization/index.md)。

## <a name="resource-filters"></a>資源篩選

*資源的篩選器*實作`IResourceFilter`或`IAsyncResourceFilter`介面和其執行包裝大部分的篩選器管線。 (只有[授權篩選條件](#authorization-filters)之前執行。)資源篩選就特別有用，如果您需要最少運算的大部分工作的要求正在進行的。 比方說，如果回應已快取中快取的篩選條件可以避免管線的其餘部分。

[簡短短路的資源篩選](#short-circuiting-resource-filter)稍早所示為資源的篩選器的其中一個範例。 另一個例子是[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs)，這樣就不會存取表單資料的模型繫結。 它可用於您知道，您會收到大型檔案上傳，並想要防止表單讀入記憶體的情況。

## <a name="action-filters"></a>動作篩選條件

*動作篩選條件*實作`IActionFilter`或`IAsyncActionFilter`介面和其執行括住的動作方法執行。

以下是範例動作篩選條件：

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

[ActionExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext)提供下列屬性：

* `ActionArguments`-可讓您管理動作的輸入。
* `Controller`-可讓您管理控制器執行個體。 
* `Result`-將此設定 short-circuits 執行的動作方法和後續的動作篩選條件。 擲回例外狀況也可避免執行的動作方法和後續的篩選條件，但會被視為失敗，而不是成功的結果。

[ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext)提供`Controller`和`Result`再加上下列屬性：

* `Canceled`-將如果動作執行的最少運算的另一個篩選，則為 true。
* `Exception`-將會為非 null 的動作或後續的動作篩選條件項擲回例外狀況。 設定這個屬性為 null，有效地 'handles' 例外狀況，以及`Result`會執行，如同它已正常傳回從動作方法。

如`IAsyncActionFilter`，呼叫`ActionExecutionDelegate`執行任何後續的動作篩選條件和動作方法，傳回`ActionExecutedContext`。 最短路徑，請指派`ActionExecutingContext.Result`某些結果執行個體，並沒有呼叫`ActionExecutionDelegate`。

架構提供一個抽象`ActionFilterAttribute`可以進行子類別。 

您可以使用動作篩選條件會自動驗證模型狀態，並傳回任何錯誤，如果狀態不正確：

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

`OnActionExecuted`方法執行之後的動作方法和可以看到及管理透過動作的結果`ActionExecutedContext.Result`屬性。 `ActionExecutedContext.Canceled`將會設為 true 如果動作執行的最少運算的另一個篩選條件。 `ActionExecutedContext.Exception`將設定為非 null 值的動作或後續的動作篩選條件項擲回例外狀況。 設定`ActionExecutedContext.Exception`為 null，有效地 'handles' 例外狀況，以及`ActionExectedContext.Result`就如同它已正常傳回從動作方法再執行。

## <a name="exception-filters"></a>例外狀況篩選條件

*例外狀況篩選條件*實作`IExceptionFilter`或`IAsyncExceptionFilter`介面。 它們可以用來實作常見的錯誤處理應用程式的原則。 

下列範例例外狀況篩選條件會顯示有關開發應用程式時，會發生的例外狀況詳細資料使用自訂的開發人員檢視時發生錯誤：

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

例外狀況篩選條件不會有兩個事件 （如之前和之後）-只實作`OnException`(或`OnExceptionAsync`)。 

例外狀況篩選條件處理未處理的例外狀況發生在控制站建立[模型繫結](../models/model-binding.md)，動作篩選條件或動作方法。 它們不會攔截例外狀況是發生在資源篩選器、 結果篩選條件，還是 MVC 結果執行。

若要處理的例外狀況，將`ExceptionContext.ExceptionHandled`屬性設為 true，或撰寫回應。 這樣會阻止傳播例外狀況。 請注意例外狀況篩選條件無法開啟到 「 成功 」 例外狀況。 只有動作篩選條件可以這麼做。

> [!NOTE]
> 在 ASP.NET 1.1 中，會傳送回應您設定如果`ExceptionHandled`為 true**和**撰寫回應。 在該案例中，ASP.NET Core 1.0 並傳送回應，且 ASP.NET Core 1.1.2 會傳回至 1.0 的行為。 如需詳細資訊，請參閱[發出 #5594](https://github.com/aspnet/Mvc/issues/5594) GitHub 儲存機制中。 

例外狀況篩選條件則適合用於設陷 MVC 動作中發生的例外狀況，但是它們並不具有彈性，錯誤處理中介軟體。 偏好以一般的情況下中, 介軟體，並使用篩選器只需要執行錯誤處理*不同*根據選擇的 MVC 動作。 比方說，您的應用程式可能會有兩個應用程式開發介面端點及檢視/HTML，動作方法。 雖然檢視為基礎的動作會傳回以 HTML 錯誤頁面 API 端點可能會傳回為 JSON，資訊時發生錯誤。

架構提供一個抽象`ExceptionFilterAttribute`可以進行子類別。 

## <a name="result-filters"></a>結果篩選條件

*產生篩選*實作`IResultFilter`或`IAsyncResultFilter`介面和其執行括住的動作結果執行。 

以下是結果篩選條件，將 HTTP 標頭的範例。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

正在執行的結果類型取決於有問題的動作。 傳回檢視的 MVC 動作會包含所有處理一部分的 razor`ViewResult`正在執行。 API 方法可能會執行一些序列化的執行結果的一部分。 深入了解[動作結果](actions.md)

動作篩選條件的動作產生的動作結果時，結果篩選條件才會執行成功的結果集。 當例外狀況篩選條件處理例外狀況時，不會執行結果篩選條件。

`OnResultExecuting`方法可以最少運算執行的動作結果和後續的結果篩選條件設定`ResultExecutingContext.Cancel`為 true。 最少運算，以避免產生空的回應時，您通常應該寫入至回應物件。 擲回例外狀況也會防止執行的動作結果和後續的篩選條件，但會被視為失敗，而不是成功的結果。

當`OnResultExecuted`方法執行時，回應可能傳送至用戶端，且 （除非發生例外狀況） 無法進一步變更。 `ResultExecutedContext.Canceled`將會設為 true 如果動作結果執行的最少運算的另一個篩選條件。

`ResultExecutedContext.Exception`如果動作結果或接下來的結果篩選器擲回例外狀況將會設定為非 null 值。 設定`Exception`到 null 有效地處理例外狀況，並可避免從正在由 MVC 管線中，稍後重新擲回例外狀況。 當您處理結果篩選條件中的例外狀況時，您可能無法寫入至回應的任何資料。 如果在動作結果會擲回推出透過其執行中，標頭已清除至用戶端，沒有任何可靠的機制，傳送失敗碼。

如`IAsyncResultFilter`呼叫`await next()`上`ResultExecutionDelegate`執行任何後續的結果篩選條件和動作結果。 最短路徑，請將`ResultExecutingContext.Cancel`至 true 並沒有呼叫`ResultExectionDelegate`。

架構提供一個抽象`ResultFilterAttribute`可以進行子類別。 [AddHeaderAttribute](#add-header-attribute)稍早所示的類別是結果篩選條件屬性的範例。

## <a name="using-middleware-in-the-filter-pipeline"></a>篩選管線中使用中介軟體

資源的篩選器一樣[中介軟體](../../fundamentals/middleware.md)在於它們圍繞稍後在管線中出現的所有項目執行。 但與中介軟體，因為它們屬於的 MVC 中，這表示他們擁有存取權 MVC 內容和建構不同的篩選器。

在 ASP.NET Core 1.1 中，您可以使用篩選器管線中的中介軟體。 您可能想要這樣做，您如有需要存取 MVC 路由資料，或其中一個控制器或動作應該僅針對特定執行中介軟體元件。

若要使用做為篩選條件的中介軟體，建立一個具有型別`Configure`方法，可指定您想要將篩選管線插入的中介軟體。 以下是範例會使用來建立要求的目前文化特性的當地語系化的中介軟體：

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

然後您可以使用`MiddlewareFilterAttribute`執行選取的控制器或動作中介軟體或全域：

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

中介軟體篩選條件執行篩選管線做為資源的篩選，模型繫結之前和之後，管線的其餘部分的相同階段。

## <a name="next-actions"></a>下一步動作

若要嘗試使用篩選器，[下載、 測試及修改範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)。
