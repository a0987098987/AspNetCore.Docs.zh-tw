---
title: ASP.NET Core 中的路由
author: rick-anderson
description: 瞭解ASP.NET核心路由如何負責匹配 HTTP 請求和調度到可執行終結點。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 4/1/2020
uid: fundamentals/routing
ms.openlocfilehash: 5742ac6879ce46e01247ddd2f8bfe3e3b8a2a02a
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80751156"
---
# <a name="routing-in-aspnet-core"></a>ASP.NET Core 中的路由

由[里安·諾瓦克](https://github.com/rynowak),[柯克·拉金](https://twitter.com/serpent5)和[里克·安德森](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

路由負責匹配傳入的 HTTP 請求並將這些請求發送到應用的可執行終結點。 [終結點](#endpoint)是應用可執行的請求處理代碼的單位。 終結點在應用中定義,並在應用啟動時配置。 終結點匹配過程可以從請求的 URL 中提取值,並提供這些值以進行請求處理。 使用來自應用的終結點資訊,路由還能夠生成映射到終結點的 URL。

套用可以使用以下功能設定路由:

- Controllers
- Razor 頁面
- SignalR
- gRPC 服務
- 開啟終結點[的中間件](xref:fundamentals/middleware/index),如[執行狀況檢查](xref:host-and-deploy/health-checks)。
- 在路由中註冊的委託和 lambdas。

本文檔介紹ASP.NET核心路由的低級詳細資訊。 有關設定路由的資訊:

* 有關控制器,請參閱<xref:mvc/controllers/routing>。
* 有關剃刀頁約定,<xref:razor-pages/razor-pages-conventions>請參閱 。

本文檔中描述的端點路由系統適用於 ASP.NET 酷 3.0 及更高版本。 有關基於<xref:Microsoft.AspNetCore.Routing.IRouter>的上一個路由系統的資訊,請使用以下方法之一選擇ASP.NET Core 2.1 版本:

* 早期版本的版本選擇器。
* 選擇[ASP.NET 核心 2.1 路由](https://docs.microsoft.com/aspnet/core/fundamentals/routing?view=aspnetcore-2.1)。

[檢視或下載範例代碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples/3.x)([如何下載](xref:index#how-to-download-a-sample))

此文件的下載示例由特定`Startup`類啟用。 要運行特定範例,請修改*Program.cs*以調用`Startup`所需的 類。

## <a name="routing-basics"></a>路由的基本概念

所有ASP.NET核心範本都包括在生成的代碼中路由。 路由在中[的中間件](xref:fundamentals/middleware/index)管道中`Startup.Configure`註冊。

以下代碼顯示了路由的基本範例:

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet&highlight=8,10)]

路由使用一對中間件,由和<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*><xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>註冊。

* `UseRouting`將路由匹配添加到中間件管道。 此中間件檢視應用中定義的終結點集,並根據請求選擇[最佳符合項](#urlm)。
* `UseEndpoints`將終結點執行添加到中間件管道。 它運行與所選終結點關聯的委託。

前面的範例包括使用[MapGet](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*)方法*對代碼*終結點的單個路由:

* 當`GET`HTTP 要求送出到`/`根網址時 :
  * 顯示的請求委託執行。
  * `Hello World!`寫入 HTTP 回應。 預設情況下,根網`/`址`https://localhost:5001/`為 。
* 如果請求方法不是`GET`或根`/`URL 不是 ,則沒有路由匹配,並且返回 HTTP 404。

### <a name="endpoint"></a>端點

<a name="endpoint"></a>

這個方法`MapGet`定義**的終結點**。 終結點可以是:

* 通過匹配 URL 和 HTTP 方法進行選擇。
* 通過運行委託執行。

可在中匹配和執行的應用的終結點在`UseEndpoints`中 配置。 例如,<xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*><xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapPost*>和[類似的方法](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions)將請求委託連接到路由系統。
其他方法可用於將ASP.NET核心框架功能連接到路由系統:
- [剃刀頁面的地圖剃刀頁面](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*)
- [控制器的地圖控制器](xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*)
- [用於訊\<號 R 的 MapHub THub>](xref:Microsoft.AspNetCore.SignalR.HubRouteBuilder.MapHub*) 
- [用於 gRPC 的 MapGrpc 服務\<服務>](xref:grpc/aspnetcore)

下面的範例顯示了具有更複雜的路由範本的路由:

[!code-csharp[](routing/samples/3.x/RoutingSample/RouteTemplateStartup.cs?name=snippet)]

這個字串`/hello/{name:alpha}`是**一個路由樣本**。 它用於配置終結點的匹配方式。 在這種情況下,範本符合:

* 網址`/hello/Ryan`
* 以`/hello/`字母字元序列開頭的任何 URL 路徑。  `:alpha`應用僅匹配字母字元的路由約束。 [此文件稍後會介紹路由約束](#route-constraint-reference)。

網址 路徑的第二`{name:alpha}`段:

* 繫結到參數`name`。
* 捕獲並存儲在[HTTPRequest.Routevalue](xref:Microsoft.AspNetCore.Http.HttpRequest.RouteValues*)中。

本文件中描述的端點路由系統ASP.NET Core 3.0 起是新的。 但是,ASP.NET Core 的所有版本都支援同一集路由範本功能和路由約束。

下面的範例顯示了具有[執行狀況檢查和](xref:host-and-deploy/health-checks)授權的路由:

[!code-csharp[](routing/samples/3.x/RoutingSample/AuthorizationStartup.cs?name=snippet)]

[!INCLUDE[request localized comments](~/includes/code-comments-loc.md)]

前面的範例展示如何:

* 授權中間件可與路由一起使用。
* 終結點可用於配置授權行為。

調用<xref:Microsoft.AspNetCore.Builder.HealthCheckEndpointRouteBuilderExtensions.MapHealthChecks*>添加運行狀況檢查終結點。 <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*>連結到此調用會將授權策略附加到終結點。

呼叫<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>並<xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*>添加身份驗證和授權中間件。 這些中間件放置在與<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*>`UseEndpoints`之間,以便它們可以:

* 查看由`UseRouting`選擇哪個終結點。
* 在調度到終結點之前<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>應用授權策略。

<a name="metadata"></a>

### <a name="endpoint-metadata"></a>端點中繼資料

在前面的示例中,有兩個終結點,但只有運行狀況檢查終結點附加了授權策略。 如果請求與運行狀況檢查終結點匹配,`/healthz`則執行授權檢查。 這說明終結點可以附加額外的數據。 此額外資料稱為終結點**中繼資料**:

* 元數據可以通過路由感知中間件進行處理。
* 元數據可以是任何 .NET 類型。

## <a name="routing-concepts"></a>路由概念

路由系統通過添加強大的**端點**概念構建在中間件管道之上。 端點表示應用功能在路由、授權和任意數量的ASP.NET Core 系統方面彼此不同的單位。

<a name="endpoint"></a>

### <a name="aspnet-core-endpoint-definition"></a>ASP.NET核心終結點定義

ASP.NET核心終結點是:

* 執行: 具有<xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate>。
* 可擴展:具有[元數據](xref:Microsoft.AspNetCore.Http.Endpoint.Metadata*)集合。
* 選擇:選擇,有[路由資訊](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern*)。
* 枚舉:可以通過<xref:Microsoft.AspNetCore.Routing.EndpointDataSource>從[DI](xref:fundamentals/dependency-injection)檢索 列出終結點的集合。

以下代碼展示如何檢索和檢查與目前要求符合的終結點:

[!code-csharp[](routing/samples/3.x/RoutingSample/EndpointInspectorStartup.cs?name=snippet)]

可以從 中檢索終結點(如果選`HttpContext`中 )。 可以檢查其屬性。 終結點物件不可變,在創建後無法修改。 最常見的終結點型態<xref:Microsoft.AspNetCore.Routing.RouteEndpoint>是 。 `RouteEndpoint`包括允許路由系統選擇的資訊。

在前面的代碼中,[應用。使用](xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*)設定一個內聯[中間元件](xref:fundamentals/middleware/index)。

<a name="mt"></a>

以下代碼顯示,根據導管中的呼叫`app.Use`位置 ,可能沒有終結點:

[!code-csharp[](routing/samples/3.x/RoutingSample/MiddlewareFlowStartup.cs?name=snippet)]

前面的範例添加`Console.WriteLine`顯示是否選擇了終結點的語句。 為清楚起見,該示例為提供的`/`終結點分配顯示名稱。

使用`/`網址執行此程式:

```txt
1. Endpoint: (null)
2. Endpoint: Hello
3. Endpoint: Hello
```

使用任何其他網址 執行此代碼會顯示:

```txt
1. Endpoint: (null)
2. Endpoint: (null)
4. Endpoint: (null)
```

此輸出表明:

* 在調用終結點之前`UseRouting`,終結點始終為空。
* 如果找到匹配項,則終結點在和`UseRouting`<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>之間為非空。
* 找到`UseEndpoints`符合項目時,中間件是**終端**機 。 [此文件稍後會定義終端中間件](#tm)。
* 僅在未找到匹配`UseEndpoints`項時執行中間件。

中間`UseRouting`件使用[SetEndpoint](xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.SetEndpoint*)方法將終結點附加到當前上下文。 有可能用自定義邏輯替換`UseRouting`中間件,並且仍然獲得使用端點的好處。 端點是低級基元(如中間件),不耦合到路由實現。 大多數應用不需要用自訂邏輯取代`UseRouting`。

中間`UseEndpoints`件設計為`UseRouting`與中間件同時使用。 執行終結點的核心邏輯並不複雜。 用於<xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*>檢索終結點,然後調用<xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate>其 屬性。

以下代碼演示了中間件如何影響路由或回應:

[!code-csharp[](routing/samples/3.x/RoutingSample/IntegratedMiddlewareStartup.cs?name=snippet)]

前面的範例演示了兩個重要概念:

* 中間件可以運行之前`UseRouting`修改路由操作的數據。
    * 通常,在路由之前出現的中間件會修改請求的某些屬性,例如<xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>,<xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions.UseHttpMethodOverride*><xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*>或 。
* 中間件可以在執行終結點`UseRouting`<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>之前 運行和處理路由結果。
    * 置`UseRouting`層與`UseEndpoints`:
      * 通常檢查元數據以瞭解端點。
      * 通常做出安全決策,如和`UseAuthorization``UseCors`執行的那樣。
    * 中間件和中繼資料的組合允許配置每個終結點的策略。

前面的代碼顯示了支援每個終結點策略的自定義中間件的範例。 中介件向主控台寫入存取敏感資料的*稽核紀錄*。 中間件可以配置為*審核*具有元數據`AuditPolicyAttribute`的 終結點。 此示例演示了*一種加入加入*模式,其中僅審核標記為敏感終結點。 可以反向定義此邏輯,例如審核所有未標記為安全的事物。 端點元數據系統是靈活的。 此邏輯可以以適合用例的任何方式進行設計。

前面的範例代碼旨在演示終結點的基本概念。 **該示例不適合生產。** *稽核紀錄*中間件的更完整版本將:

* 登錄到文件或資料庫。
* 包括使用者、IP 位址、敏感終結點的名稱等詳細資訊。

審核策略中繼`AuditPolicyAttribute`資料`Attribute`定義為 更易於使用基於類的框架(如控制器和 SignalR)。 使用*路由代碼*時 :

* 元數據與生成器 API 一起連接。
* 創建終結點時,基於類的框架包括相應方法和類上的所有屬性。

元數據類型的最佳做法是將它們定義為介面或屬性。 介面和屬性允許代碼重用。 元數據系統是靈活的,不會施加任何限制。

<a name="tm"></a>

### <a name="comparing-a-terminal-middleware-and-routing"></a>比較終端中間件與路由

以下代碼範例使用中間件與使用路由進行對比:

[!code-csharp[](routing/samples/3.x/RoutingSample/TerminalMiddlewareStartup.cs?name=snippet)]

中間元件的樣式`Approach 1:`是**終端中間件**。 它被稱為終端中間件,因為它執行匹配操作:

* 前面的範例中的匹配操作用於`Path == "/"`中間件和`Path == "/Movie"`路由。
* 當匹配成功時,它將執行一些功能並返回,而不是調用`next`中間件。

它被稱為終端中間件,因為它終止搜索,執行一些功能,然後返回。

比較終端中間件和路由:
* 這兩種方法都允許終止處理管道:
    * 中間件通過返回而不是調用`next`終止管道。
    * 端點始終是終端。
* 終端中間件允許將中間件放置在導管中的任意位置:
    * 在位置執行<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>。
* 終端中間件允許任意代碼確定中間件何時符合:
    * 自定義路由匹配代碼可能非常詳細且難以正確編寫。
    * 路由為典型應用提供了直接的解決方案。 大多數應用不需要自定義路由匹配代碼。
* 端點介面與中間件(如`UseAuthorization``UseCors`與 )
    * 使用具有`UseAuthorization``UseCors`或需要手動與授權系統介面的終端中間件。

[終結點](#endpoint)定義兩者:

* 處理請求的委託。
* 任意元數據的集合。 元數據用於根據附加到每個終結點的策略和配置實現跨領域問題。

終端中間件可以是一個有效的工具,但可能需要:

* 大量的編碼和測試。
* 與其他系統手動集成,實現所需的靈活性。

在編寫終端中間件之前,請考慮與路由集成。

與[Map](xref:fundamentals/middleware/index#branch-the-middleware-pipeline)<xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*>整合或通常可轉換為路由感知終結點的現有終端中間件。 [MapHealthChecks](https://github.com/aspnet/AspNetCore/blob/master/src/Middleware/HealthChecks/src/Builder/HealthCheckEndpointRouteBuilderExtensions.cs#L16)展示了路由器軟體的模式:
* 在<xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>上 寫入擴展方法。
* 使用<xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder.CreateApplicationBuilder*>創建嵌套中間件管道。
* 將中間件附加到新管道。 在此案例中為 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>。
* <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.Build*>中介元件導管進入<xref:Microsoft.AspNetCore.Http.RequestDelegate>。
* 調用`Map`並提供新的中間件管道。
* 返回擴展方法提供的`Map`生成器物件。

以下代碼顯示了[MapHealth 檢查](xref:host-and-deploy/health-checks)的使用 :

[!code-csharp[](routing/samples/3.x/RoutingSample/AuthorizationStartup.cs?name=snippet)]

前面的示例顯示了返回生成器物件的重要性。 返回生成器物件允許應用開發人員配置策略,如終結點的授權。 在此示例中,運行狀況檢查中間件與授權系統沒有直接集成。

創建元數據系統是為了回應擴展作者使用終端中間件遇到的問題。 每個中間件實現自己與授權系統的集成都是有問題的。

<a name="urlm"></a>

### <a name="url-matching"></a>URL 比對

* 路由將傳入請求與[終結點](#endpoint)匹配的過程。
* 基於 URL 路徑和標頭中的數據。
* 可以擴展以考慮請求中的任何數據。

當路由中間元件執行時,它會設定`Endpoint`與 路由值<xref:Microsoft.AspNetCore.Http.HttpContext>到 目前[要求上的請求功能](xref:fundamentals/request-features):

* 呼叫[HTTPContext.取得終結點](<xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*>)獲取終結點。
* `HttpRequest.RouteValues` 會取得路由值的集合。

路由中間件之後運行[的中間件](xref:fundamentals/middleware/index)可以檢查終結點並採取措施。 例如,授權中間件可以詢問終結點的元數據集合以用於授權策略。 執行要求處理管線中的所有中介軟體之後，會叫用所選端點的委派。

端點路由中的路由系統負責制定所有分派決策。 由於中間件應用基於所選終結點的策略,因此請務必:

* 任何可能影響調度或應用安全策略的決定都會在路由系統內做出。

> [!WARNING]
> 對於向後相容性,當執行控制器或 Razor Pages 終結點委託時[,RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData)的屬性將設置為基於迄今執行的請求處理的適當值。
>
> 該`RouteContext`類型將在將來的版本中標記為過時:
>
> * 移到`RouteData.Values``HttpRequest.RouteValues`。
> * 從`RouteData.DataTokens`終結點中繼資料移至檢索[IDataTokens 資料](xref:Microsoft.AspNetCore.Routing.IDataTokensMetadata)。

URL 匹配在一組可配置的階段中運行。 在每個階段,輸出是一組匹配項。 下一階段可以進一步縮小比賽範圍。 路由實現不保證匹配終結點的處理順序。 **所有**可能的匹配項都立即處理。 URL 匹配階段按以下順序發生。 ASP.NET Core：

1. 根據終結點集及其路由範本處理 URL 路徑,收集**所有**匹配項。
1. 獲取上述清單並刪除應用路由約束失敗匹配項。
1. 獲取上述清單並刪除使[MatcherPolicy](xref:Microsoft.AspNetCore.Routing.MatcherPolicy)實例集失敗的匹配項。
1. 使用[終結點選擇器](xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector)從上述清單中做出最終決定。

終結點列表的優先權根據:

* [路由終結點。順序](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order*)
* [路由樣本優先順序](#rtp)

在每個階段處理所有符合的終結點,直到達到<xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector>。 這是`EndpointSelector`最後階段。 它將從匹配中選擇最高優先順序終結點作為最佳匹配。 如果存在與最佳匹配項具有相同優先順序的其他匹配項,則引發不明確的匹配異常。

路由優先順序是根據給定更高優先順序**的更具體的**路由範本計算的。 例如,請考慮樣本`/hello`與`/{message}`:

* 兩者都與`/hello`URL 路徑匹配。
* `/hello`更具體,因此優先順序更高。

通常,路由優先順序非常適合實際使用的 URL 方案類型。 僅在<xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order>必要時使用,以避免模稜兩可。

由於路由提供的擴充性類型,路由系統無法提前計算不明確的路由。 考慮路由範本`/{message:alpha}``/{message:int}`與的範例:

* 約束`alpha`僅匹配字母字元。
* 約束`int`僅與數位匹配。
* 這些範本具有相同的路由優先順序,但沒有它們匹配的單個 URL。
* 如果路由系統在啟動時報告存在歧義錯誤,它將阻止此有效用例。

> [!WARNING]
>
> 內部<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>操作的順序不會影響路由的行為,但有一個例外。 <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>並根據<xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*>所調用的順序自動為其終結點分配訂單值。 這將模擬控制器的長期行為,而路由系統沒有提供與舊路由實現相同的保證。
>
> 在路由的舊實現中,可以實現依賴於路由處理順序的路由擴展性。 ASP.NET Core 3.0 及更高版本中的終結點路由:
> 
> * 沒有路線的概念。
> * 不提供訂購保證。 一次處理所有終結點。
>
> 如果這意味著您被卡住使用舊路由系統,[則開啟 GitHub 問題以獲得説明](https://github.com/dotnet/aspnetcore/issues)。

<a name="rtp"></a>

### <a name="route-template-precedence-and-endpoint-selection-order"></a>路由樣本優先權和終結點選擇順序

[路由範本優先順序](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L16)是一個系統,它根據每個工藝路線範本的特定於值分配了值。 路由樣本優先順序:

* 避免在常見情況下調整終結點的順序。
* 嘗試匹配路由行為的常識性期望。

例如,請考慮範本`/Products/List`和`/Products/{id}`。 假設`/Products/List`這比`/Products/{id}``/Products/List`URL 路徑 更適合。 之所以有效,是因為文本段`/List`被認為比參數`/{id}`段 具有更好的優先順序。

優先順序的工作原理與路由範本的定義方式相結合的詳細資訊:

* 具有更多段的範本被視為更具體。
* 具有文本文本的段被認為比參數段更具體。
* 具有約束的參數段被認為比沒有約束的參數段更具體。
* 複雜段被視為具有約束的參數段的特定段。
* 捕獲所有參數都是最不具體的。

有關精確值的引用,請參閱[GitHub 上的原始碼](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L189)。

<a name="lg"></a>

### <a name="url-generation-concepts"></a>網址產生概念

網址產生:

* 路由可以基於一組路由值創建 URL 路徑的過程。
* 允許在終結點和訪問它們的 URL 之間進行邏輯分離。

端點路由包括<xref:Microsoft.AspNetCore.Routing.LinkGenerator>API。 `LinkGenerator`是從[DI](xref:fundamentals/dependency-injection)提供的單例服務。 API`LinkGenerator`可以在執行請求的上下文之外使用。 [Mvc.IUrlHelper](xref:Microsoft.AspNetCore.Mvc.IUrlHelper)和依賴<xref:Microsoft.AspNetCore.Mvc.IUrlHelper>於的方案(如[標記説明程式](xref:mvc/views/tag-helpers/intro)、HTML 幫助器和[操作結果](xref:mvc/controllers/actions)`LinkGenerator`)在內部使用 API 來提供連結生成功能。

連結產生器背後支援的概念為「位址」**** 和「位址配置」****。 位址配置可讓您判斷應考慮用於連結產生的端點。 例如,許多使用者從控制器中熟悉的路由名稱和路由值方案,Razor Pages 作為位址方案實現。

鏈路產生器可以透過以下擴充方法連結到控制器和 Razor 頁面:

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

這些方法的重載接受包含的`HttpContext`參數。 這些方法在功能上等同於[Url.Action](xref:System.Web.Mvc.UrlHelper.Action*)和[Url.Page,](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*)但提供了額外的靈活性和選項。

這些方法`GetPath*`與`Url.Action`和`Url.Page`最相似 ,因為它們生成包含絕對路徑的 URI。 `GetUri*` 方法一律會產生包含配置和主機的絕對 URI。 接受 `HttpContext` 的方法會在執行要求的內容中產生 URI。 除非重寫,否則將使用執行請求[的環境](#ambient)路由值、URL 基本路徑、方案和主機。

呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 並指定一個位址。 執行下列兩個步驟來產生 URI：

1. 將位址繫結至符合該位址的端點清單。
1. 評估每個端點的 <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern>，直到找到符合所提供值的路由模式。 產生的輸出會與提供給連結產生器的其他 URI 組件合併並傳回。

<xref:Microsoft.AspNetCore.Routing.LinkGenerator> 提供的方法支援適用於任何位址類型的標準連結產生功能。 使用鏈路產生器的最便捷方法是透過擴充方法對特定位址類型執行操作:

| 擴充方法 | 描述 |
| ---------------- | ----------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | 根據提供的值產生具有絕對路徑的 URI。 |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | 根據提供的值產生絕對 URI。             |

> [!WARNING]
> 注意呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 方法的下列影響：
>
> * 使用 `GetUri*` 擴充方法，並注意應用程式組態不會驗證傳入要求的 `Host` 標頭。 如果未驗證`Host`傳入請求的標頭,則可以將不受信任的請求輸入發送回檢視或頁面中的 URI 中的用戶端。 建議所有生產應用程式將其伺服器設定為驗證 `Host` 標頭是否為已知有效值。
>
> * 使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator>，並注意與 `Map` 或 `MapWhen` 搭配使用的中介軟體。 `Map*` 會變更執行要求的基底路徑，這會影響連結產生的輸出。 所有的 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 都允許指定基底路徑。 指定一個空的基本路徑來撤銷對`Map*`連結生成的影響。

### <a name="middleware-example"></a>中介軟體範例

在下面的示例中,中間件使用<xref:Microsoft.AspNetCore.Routing.LinkGenerator>API 創建指向列出存儲產品的操作方法的連結。 使用連結產生器將其注入類,並且呼叫`GenerateLink`可用於應用中的任何類:

[!code-csharp[](routing/samples/3.x/RoutingSample/Middleware/ProductsLinkMiddleware.cs?name=snippet)]

<a name="rtr"></a>

## <a name="route-template-reference"></a>路由範本參考

其中`{}`的標記定義路由參數,這些參數在路由匹配時被綁定。 可以在工藝路線段中定義多個路由參數,但路由參數必須用文本值分隔。 例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。  路由參數必須具有名稱,並且可能具有指定的其他屬性。

路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。 文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。 要匹配文字路由參數分隔符`{``}`或 ,請通過重複該字元來轉義分隔符。 例如`{{`或`}}`。

星號`*`或雙星`**`號 :

* 可用作路由參數的首碼,以綁定到URI的其餘部分。
* 稱為 **「全部捕獲」** 參數。 例如，`blog/{**slug}`：
  * 匹配以`/blog`URI 開頭並具有任何值的任何 URI。
  * 以下值`/blog`分配給[slug](https://developer.mozilla.org/docs/Glossary/Slug)路由值。

全部擷取參數也可以符合空字串。

當路由用於生成 URL(包括路徑分隔`/`符 )時,捕獲所有參數將轉義相應的字元。 例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。 請注意逸出的斜線。 若要反覆存取路徑分隔符號字元，請使用 `**` 路由參數前置詞。 具有 `{ path = "my/path" }` 的路由 `foo/{**path}` 會產生 `foo/my/path`。

URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。 以範本 `files/{filename}.{ext?}` 為例。 當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。 如果 URL`filename`中 僅存在一個值,則路由`.`將匹配, 因為尾隨是可選的。 下列 URL 符合此路由：

* `/files/myFile.txt`
* `/files/myFile`

路由參數可能有「預設值」****，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。 例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。 如果 URL 中沒有用於參數的任何值，則會使用預設值。 通過將問號 (`?`) 添加到參數名稱的末尾,使路由參數成為可選的。 例如： `id?` 。 可選值和預設路由參數之間的區別是:

* 具有預設值的路由參數始終生成值。
* 僅當請求 URL 提供值時,可選參數才具有值。

路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。 路由`:`參數名稱之後的添加和約束名稱指定路由參數上的內聯約束。 如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 `(...)` 括住。 可以透過額外另一`:`個 規範名稱來指定多個*內聯約束*。

條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。 例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。 如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。

路由參數可能也有參數變壓器。 參數轉換器在生成連結並將操作和頁面與 URL 匹配時轉換參數的值。 與約束一樣,參數變壓器可以通過在路由參數名稱之後添加`:`和轉換器名稱來內聯地添加到路由參數中。 例如，路由範本 `blog/{article:slugify}` 會指定 `slugify` 轉換器。 如需參數轉換器的詳細資訊，請參閱[參數轉器參考](#parameter-transformer-reference)一節。

下表演示了範例路由範本及其行為:

| 路由範本                           | 範例比對 URI    | 要求 URI&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | 只比對單一路徑 `/hello`。                                     |
| `{Page=Home}`                            | `/`                     | 比對並將 `Page` 設定為 `Home`。                                         |
| `{Page=Home}`                            | `/Contact`              | 比對並將 `Page` 設定為 `Contact`。                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | 對應至 `Products` 控制器和 `List` 動作。                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | 映射到`Products`控制器,`Details`並將`id`操作設置為 123。 |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | 映射到`Home`控制器與`Index`方法 。 `id` 會被忽略。        |
| `{controller=Home}/{action=Index}/{id?}` | `/Products`         | 映射到`Products`控制器與`Index`方法 。 `id` 會被忽略。        |

使用範本通常是最簡單的路由方式。 條件約束和預設值也可以在路由範本外部指定。

### <a name="complex-segments"></a>複雜區段

通過以[非貪婪](#greedy)的方式從右向左匹配文本分隔符來處理複雜的段。 例如,`[Route("/a{b}c{d}")]`是一個複雜的段。
複雜段以特定的方式工作,必須理解這些方式才能成功使用它們。 本節中的示例演示了為什麼複雜段只有在分隔符文本未顯示在參數值內時才真正正常工作。 對於更複雜的情況,需要使用[正則表達式](/dotnet/standard/base-types/regular-expressions)並手動提取值。

這是路由使用範本`/a{b}c{d}`和 URL`/abcd`路徑執行的步驟的摘要。 協助`|`視覺化演演算法的工作原理:

* 第一個字面(從右至`c`左)是 。 因此`/abcd`,從右搜索並尋找`/ab|c|d`。
* 右側的所有內容現在`d`都與路由`{d}`參數 匹配。
* 下一個字面(從右至`a`左)是 。 因此`/ab|c|d`,從我們離開的地方開始搜索,然後`a`被`/|a|b|c|d`發現 。
* 右側的值`b`( ) 現在與`{b}`路由參數 匹配。
* 沒有剩餘的文本和剩餘的路由範本,因此這是匹配項。

下面是使用同一範本`/a{b}c{d}`和`/aabcd`URL 路徑的否定案例的示例。 `|`用於幫助可視化演演演算法的工作原理。 此情況不是匹配,由同一演算法解釋:
* 第一個字面(從右至`c`左)是 。 因此`/aabcd`,從右搜索並尋找`/aab|c|d`。
* 右側的所有內容現在`d`都與路由`{d}`參數 匹配。
* 下一個字面(從右至`a`左)是 。 因此`/aab|c|d`,從我們離開的地方開始搜索,然後`a`被`/a|a|b|c|d`發現 。
* 右側的值`b`( ) 現在與`{b}`路由參數 匹配。
* 此時還有剩餘的文本`a`,但演演演算法已用完路由範本進行解析,因此這不是匹配項。

由於符合演演算法[是非貪婪的](#greedy):

* 它匹配每個步驟中可能最少的文本量。
* 任何參數值內出現分隔符值的情況都會導致不匹配。

正則表達式可以更多地控制它們的匹配行為。

<a name="greedy"></a>

貪婪匹配,也知道為[懶惰匹配](https://wikipedia.org/wiki/Regular_expression#Lazy_matching),匹配最大的可能的字串。 非貪婪匹配盡可能小的字串。

## <a name="route-constraint-reference"></a>路由條件約束參考

路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。 工藝路線約束通常檢查通過工藝路線範本關聯的路由值,並做出正確或錯誤的決定是否該值是否可以接受。 某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。 例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。 條件約束可用於路由要求和連結產生。

> [!WARNING]
> 請勿針對輸入驗證使用條件約束。 如果約束用於輸入驗證,則無效輸入會導致`404`"未找到"回應。 無效輸入應生成帶有`400`相應錯誤消息的"錯誤請求」。 路由條件約束會用來釐清類似的路由，而不是用來驗證特定路由的輸入。

下表演示了示例路由約束及其預期行為:

| constraint (條件約束) | 範例 | 範例相符項目 | 注意 |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | 符合任何整數 |
| `bool` | `{active:bool}` | `true`, `FALSE` | 符合`true`項目`false`或 。 不區分大小寫 |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | 匹配不變區域性`DateTime`中的有效值。 請參閱前面的警告。 |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | 匹配不變區域性`decimal`中的有效值。 請參閱前面的警告。|
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | 匹配不變區域性`double`中的有效值。 請參閱前面的警告。|
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | 匹配不變區域性`float`中的有效值。 請參閱前面的警告。|
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638` | 符合有效的 `Guid` 值 |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | 符合有效的 `long` 值 |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | 字串必須至少有 4 個字元 |
| `maxlength(value)` | `{filename:maxlength(8)}` | `MyFile` | 字串不能超過 8 個字元 |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | 字串長度必須剛好是 12 個字元 |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | 字串長度必須至少有 8 個字元，但不能超過 16 個字元 |
| `min(value)` | `{age:min(18)}` | `19` | 整數值必須至少為 18 |
| `max(value)` | `{age:max(120)}` | `91` | 整數值不能超過 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | 整數值必須至少為 18，但不能超過 120 |
| `alpha` | `{name:alpha}` | `Rick` | 字串必須包含一個或多個字母字元,`a`-`z`並且不區分大小寫。 |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | 字串必須與正則表達式匹配。 請參閱有關定義正則表達式的提示。 |
| `required` | `{name:required}` | `Rick` | 用來強制執行在 URL 產生期間呈現非參數值 |

[!INCLUDE[](~/includes/regex.md)]

多個冒號分隔約束可以應用於單個參數。 例如，下列條件約束會將參數限制在 1 或更大的整數值：

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> 驗證 URL 並轉換為 CLR 類型的路由約束始終使用不變區域性。 例如,轉換為 CLR`int``DateTime`類型或 。 這些約束假定 URL 不可當地語系化。 架構提供的路由條件約束不會修改路由值中儲存的值。 所有從 URL 剖析而來的路由值會儲存為字串。 例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。

### <a name="regular-expressions-in-constraints"></a>限制中的正規表示式

[!INCLUDE[](~/includes/regex.md)]

可以使用`regex(...)`路由約束將正則表達式指定為內聯約束。 <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>族中的方法也接受約束的物件文本。 如果使用該窗體,字串值將解釋為正則表達式。

以下代碼使用內聯正則表示式約束:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRegex.cs?name=snippet)]

以下代碼使用物件文字指定正規表示式約束:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRegex2.cs?name=snippet)]

ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。 如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。

正規表示式使用與路由和 C# 語言類似的分隔符和權杖。 規則運算式的語彙基元必須逸出。 要在內聯約束`^\d{3}-\d{2}-\d{4}$`中使用正則運算式,請使用以下項之一:

* 將`\`字串中提供的字元替換為`\\`C# 原始檔檔中的字元,`\`以便轉義 字串轉義字元。
* [逐字串文字](/dotnet/csharp/language-reference/keywords/string)。

要轉義路由參數分隔符`{`, `}` `[` `]`、 , , 將表示式的字`{{``}}`元`[[``]]`加倍, 例如, 、 、 、 、 、 、 , , , 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 下表顯示了正規表示式及其轉義版本:

| 規則運算式    | 轉義正則運算式     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

路由中使用的正則表達式通常以`^`字元開頭,並匹配字串的起始位置。 運算式通常以`$`字元結尾,並匹配字串的末尾。 `^`和`$`字元確保正則表達式與整個路由參數值匹配。 如果沒有`^`和`$`字元,正則表達式匹配字串中的任何子字串,這通常是不可取的。 下表提供了示例,並解釋了為什麼它們匹配或不匹配:

| 運算是   | String    | 相符項目 | 註解               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | hello     | 是   | 子字串相符項目     |
| `[a-z]{2}`   | 123abc456 | 是   | 子字串相符項目     |
| `[a-z]{2}`   | mz        | 是   | 符合運算式    |
| `[a-z]{2}`   | MZ        | 是   | 不區分大小寫    |
| `^[a-z]{2}$` | hello     | 否    | 請參閱上述的 `^` 和 `$` |
| `^[a-z]{2}$` | 123abc456 | 否    | 請參閱上述的 `^` 和 `$` |

如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。

若要將參數限制為一組已知的可能值，請使用規則運算式。 例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。 如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。 在約束字典中傳遞的與已知約束之一不匹配的約束也被視為正則表達式。 在範本中傳遞的與已知約束之一不匹配的約束不被視為正則運算式。

### <a name="custom-route-constraints"></a>自訂路由約束

可以通過<xref:Microsoft.AspNetCore.Routing.IRouteConstraint>實現 介面來創建自定義路由約束。 介面`IRouteConstraint`<xref:System.Web.Routing.IRouteConstraint.Match*>包含 ,如果`true`滿足`false`約束 , 則傳回該介面。

很少需要自定義路由約束。 在實現自定義路由約束之前,請考慮其他替代方法,如模型綁定。

要使用自定義`IRouteConstraint`,路由約束類型必須註冊到服務<xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>容器 中的應用。 `ConstraintMap` 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 `IRouteConstraint` 實作。 更新應用程式的 `ConstraintMap` 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。 例如：

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint.cs?name=snippet)]

上述限制在以下代碼中應用:

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/TestController.cs?name=snippet&highlight=6,13)]

[!INCLUDE[](~/includes/MyDisplayRouteInfo.md)]

的`MyCustomConstraint`實`0`用防止 應用程式於路由參數:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint.cs?name=snippet2)]

[!INCLUDE[](~/includes/regex.md)]

上述程式碼：

* 防止`0``{id}`在 路徑段中。
* 顯示為提供實現自定義約束的基本範例。 它不應在生產應用中使用。

以下代碼是防止處理包含的更好的`id``0`方法:

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/TestController.cs?name=snippet2)]

與`MyCustomConstraint`該方法,上述代碼具有以下優點:

* 它不需要自定義約束。
* 當路由參數包括`0`時,它返回更具描述性的錯誤。

## <a name="parameter-transformer-reference"></a>參數轉換器參考

參數轉換程式：

* 使用<xref:Microsoft.AspNetCore.Routing.LinkGenerator>生成連結時執行。
* 實作 <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer?displayProperty=fullName>。
* 是使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定的。
* 採用參數的路由值，並將它轉換為新的字串值。
* 導致在所產生連結中使用已轉換的值。

例如，具有 `Url.Action(new { article = "MyTestArticle" })` 之路由模式 `blog\{article:slugify}` 中的自訂 `slugify` 參數轉換器，會產生 `blog\my-test-article`。

請考慮以下`IOutboundParameterTransformer`實現:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint2.cs?name=snippet2)]

要在路由模式中使用參數轉換器,請使用<xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>`Startup.ConfigureServices`

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint2.cs?name=snippet)]

ASP.NET核心框架使用參數轉換器來轉換端點解析的URI。 此參數`area`轉換器轉換用於符合 的`controller`路由值`action``page`。

```csharp
routes.MapControllerRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

使用前面的路由範本,操作`SubscriptionManagementController.GetAll`與URI`/subscription-management/get-all`匹配。 參數轉換器不會變更用來產生連結的路由值。 例如，`Url.Action("GetAll", "SubscriptionManagement")` 會輸出 `/subscription-management/get-all`。

ASP.NET核心提供 API 約定,用於使用具有生成路由的參數變壓器:

* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention?displayProperty=fullName> MVC 約定將指定的參數轉換器應用於應用中的所有屬性路由。 參數轉換程式會在被取代時轉換屬性路由語彙基元。 如需詳細資訊，請參閱[使用參數轉換程式自訂語彙基元取代](xref:mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement)。
* 剃刀頁面使用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention>API 約定。 此慣例會將所指定參數轉換器套用至所有自動探索到的 Razor Pages。 參數轉換器會轉換 Razor Pages 路由的資料夾與檔案名稱區段。 如需詳細資訊，請參閱[使用參數轉換程式自訂頁面路由](xref:razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes)。

<a name="ugr"></a>

## <a name="url-generation-reference"></a>URL 產生參考

本節包含 URL 生成實現的演演演算法的引用。 實際上,URL 生成的最複雜示例使用控制器或 Razor 頁面。 有關詳細資訊[,請參閱控制器中的路由](xref:mvc/controllers/routing)。

URL 生成過程從調用[LinkGenerator.GetPathByAddress](xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*)或類似方法開始。 該方法提供位址、一組路由值以及有關 當前請求`HttpContext`的可選資訊。

第一步是使用 與位址類型匹配的[`IEndpointAddressScheme<TAddress>`](xref:Microsoft.AspNetCore.Routing.IEndpointAddressScheme`1)解析 一組候選終結點。

位址方案找到一組候選項后,將反覆排序和處理終結點,直到 URL 生成操作成功。 URL 生成**不**檢查歧義,返回的第一個結果是最終結果。

### <a name="troubleshooting-url-generation-with-logging"></a>使用紀錄記錄對網址產生進行故障排除

此網址產生的變更, 並將紀錄紀錄`Microsoft.AspNetCore.Routing`等級設定為`TRACE`。 `LinkGenerator`記錄有關其處理的許多詳細資訊,這些詳細資訊可用於解決問題。

有關網址產生的詳細資訊,請參考[網址](#ugr)。

### <a name="addresses"></a>位址

位址是 URL 生成中用於將調用綁定到一組候選終結點的概念。

預設情況下,位址是一個可擴展的概念,附帶兩個實現:

* 使用*終結點名稱*`string`( ) 作為位址:
    * 提供與 MVC 路由名稱類似的功能。
    * 使用<xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata>元數據類型。
    * 根據所有已註冊終結點的元數據解析提供的字串。
    * 如果多個終結點使用相同的名稱,則在啟動時引發異常。
    * 建議用於控制器和剃刀頁以外的通用用途。
* 使用*路由值*<xref:Microsoft.AspNetCore.Routing.RouteValuesAddress>( ) 作為位址:
    * 提供與控制器和 Razor 頁面舊 URL 生成類似的功能。
    * 擴展和調試非常複雜。
    * 提供`IUrlHelper`、標記幫助器、HTML 幫助器、操作結果等使用的實現。

位址機制的使用要透過任何條件, 並使用您要顯示的檔案與符合的動作的關聯:

* 終結點名稱方案執行基本字典查找。
* 路由值方案具有複雜的最佳集演演演算法子集。

<a name="ambient"></a>

### <a name="ambient-values-and-explicit-values"></a>環境值與顯式值

從目前請求中,路由訪問當前請求`HttpContext.Request.RouteValues`的路由值。 與目前要求關聯的值稱為**環境值**。 為清楚起見,文件將傳遞給方法的路由值稱為**顯式值**。

下面的範例顯示環境值和顯式值。 它提供目前要求的環境值與顯式值: `{ id = 17, }`

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet)]

上述程式碼：

* 傳回 `/Widget/Index/17`。
* 透過<xref:Microsoft.AspNetCore.Routing.LinkGenerator> [DI](xref:fundamentals/dependency-injection)取得 。

以下代碼不提供環境值與顯式值: `{ controller = "Home", action = "Subscribe", id = 17, }`:

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet2)]

前面的方法傳回`/Home/Subscribe/17`

傳`WidgetController``/Widget/Subscribe/17`回以下代碼:

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet3)]

以下代碼提供目前要求中的環境值與顯式值中的控制器: `{ action = "Edit", id = 17, }`:

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/GadgetController.cs?name=snippet)]

在上述程式碼中：

* `/Gadget/Edit/17`返回。
* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Url>取得<xref:Microsoft.AspNetCore.Mvc.IUrlHelper>。
* <xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*>   
生成具有操作方法絕對路徑的 URL。 網址包含`action`指定的名稱`route`和值。

以下代碼提供目前要求的環境值與顯式值: `{ page = "./Edit, id = 17, }`:

[!code-csharp[](routing/samples/3.x/RoutingSample/Pages/Index.cshtml.cs?name=snippet)]

上述代碼集`url`於`/Edit/17`編輯 剃刀頁面包含以下頁面指令時:

 `@page "{id:int}"`

若編輯「 頁不包含`"{id:int}"`」 路由樣本`url`,`/Edit?id=17`則為 。

除了此處描述的規則之外,MVC<xref:Microsoft.AspNetCore.Mvc.IUrlHelper>的行為 還增加了一層複雜性:

* `IUrlHelper`始終提供當前請求中的路由值作為環境值。
* [IUrlHelper.Action](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*)始終將`action`當前`controller`值 和路由值複製為顯式值,除非開發人員重寫。
* [IUrlHelper.page](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*)始終將`page`當前 路由值複製為顯式值,除非被覆蓋。 <!--by the user-->
* `IUrlHelper.Page`始終將當前`handler`路由`null`值 作為顯式值覆蓋,除非被覆蓋。

使用者通常對環境值的行為細節感到驚訝,因為 MVC 似乎並不遵循其自己的規則。 出於歷史和相容性的原因,某些路由值(如`action`、、`page``handler``controller`和)具有自己的特殊情況行為。

提供的等效功能`LinkGenerator.GetPathByAction`和`LinkGenerator.GetPathByPage`複製這些`IUrlHelper`異常,以便進行相容性。

### <a name="url-generation-process"></a>網址產生程序

找到候選終結點集後,URL 產生演演算法:

* 反覆運算地處理端點。
* 返回第一個成功的結果。

此過程的第一步稱為**路由值失效**。  路由值失效是路由決定應使用環境值中的路由值以及應忽略的路由值的過程。 考慮每個環境值,並與顯式值組合,或忽略。

考慮環境值角色的最佳方法是嘗試保存應用程式開發人員鍵入,在某些常見情況下。 傳統上,環境值有用的方案與 MVC 相關:

* 連結到同一控制器中的另一個操作時,不需要指定控制器名稱。
* 連結到同一區域中的另一個控制器時,不需要指定區域名稱。
* 連結到同一操作方法時,不需要指定路由值。
* 連結到應用的另一部分時,您不希望傳遞在應用的該部分中沒有意義的路由值。

調用`LinkGenerator``IUrlHelper`或`null`返回 通常是由於不理解路由值失效引起的。 通過顯式指定更多路由值來排除路由值失效的問題,以查看這是否解決了問題。

路由值失效的工作原理是假定應用的 URL 方案是分層的,層次結構由從左到右形成。 考慮基本的控制器路由範本`{controller}/{action}/{id?}`,以便直觀地瞭解它在實踐中是如何工作的。 變更值的**變更**會使右方顯示的所有路由值 **。** 這反映了關於層次結構的假設。 如果應用具有的環境`id`值,並且操作為`controller`指定 不同的值:

* `id`不會重複使用,因為`{controller}`位於的`{id?}`左側。

展示此原則的一些範例:

* 如果顯式值包含的值`id`,則忽略`id`的環境 值。 的環境值`controller`,`action`可以使用。
* 如果顯式值包含的值`action`,則忽略`action`其的任何環境值。 可以使用的環境`controller`值。 如果`action`的 顯式值與`action`的環境 值不同,`id`則不會使用 該值。  如果`action`的 顯式值與`action`的環境 值相同,則`id`可以使用 該值。
* 如果顯式值包含的值`controller`,則忽略`controller`其的任何環境值。 如果`controller`的顯式值與`controller`的環境值不同,則不會`action`使用`id`和值。 如果`controller`的顯式值與 和的值的`controller`環境 值相同,`action`則`id`可以使用和值。

由於存在屬性路由和專用常規路由,此過程更加複雜。 控制器常規路由,例如`{controller}/{action}/{id?}`使用路由參數指定層次結構。 對控制器與 Razor 網頁[的傳統路由](xref:mvc/controllers/routing#dcr)與[屬性路由](xref:mvc/controllers/routing#ar):

* 存在路由值的層次結構。
* 它們不顯示在範本中。

在這些情況下,URL生成定義了**所需的值**概念。 由控制器和 Razor Pages 創建的終結點具有指定的值,這些值允許路由值失效。

路由值失效演算法的詳細資訊:

* 所需的值名稱與路由參數結合使用,然後從左至右處理。
* 對於每個參數,將比較環境值和顯式值:
    * 如果環境值和顯式值相同,則該過程將繼續。
    * 如果環境值存在,並且顯式值不存在,則生成 URL 時將使用環境值。
    * 如果環境值不存在且顯式值存在,則拒絕環境值和所有後續環境值。
    * 如果存在環境值和顯式值,並且兩個值不同,則拒絕環境值和所有後續環境值。

此時,URL 生成操作已準備好評估路由約束。 接受值集與參數預設值相結合,這些預設值提供給約束。 如果約束全部通過,則操作將繼續。

接下來,**接受的值**可用於展開路由範本。 處理式製程的樣本:

* 從左到右。
* 每個參數都代有其接受值。
* 以下特殊情況:
  * 如果接受的值缺少值,並且參數具有預設值,則使用預設值。
  * 如果接受的值缺少值,並且參數是可選的,則繼續處理。
  * 如果缺少的可選參數右側的任何路由參數具有值,則操作將失敗。
  * <!-- review default-valued parameters optional parameters --> 儘可能摺疊連續的預設值參數和可選參數。

顯式提供與路由段不匹配的值將添加到查詢字串中。 下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。

| 環境值                     | 明確值                        | 結果                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| controller = "Home"                | action = "About"                       | `/Home/About`           |
| controller = "Home"                | controller = "Order", action = "About" | `/Order/About`          |
| controller = "Home", color = "Red" | action = "About"                       | `/Home/About`           |
| controller = "Home"                | action = "About", color = "Red"        | `/Home/About?color=Red` |

### <a name="problems-with-route-value-invalidation"></a>路由值失效問題

從 ASP.NET Core 3.0 起,早期 ASP.NET Core 版本中使用的某些 URL 生成方案不能很好地與 URL 生成配合使用。 ASP.NET核心團隊計劃在未來版本中添加功能來滿足這些需求。 目前,最好的解決方案是使用舊路由。

以下代碼顯示了路由不支援的 URL 生成方案的範例。

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupUnsupported.cs?name=snippet)]

在前面的代碼中,`culture`路由參數用於當地語系化。 願望是始終將`culture`參數作為環境值接受。 但是,`culture`由於所需值的工作方式,該參數不被接受為環境值:

* 在`"default"`工藝路線範本中`culture`, 路由參數`controller`位於的左側,因此`controller`更改為 不會`culture`使 無效。
* 在`"blog"`工藝路線範本中`culture`, 路由參數被視為`controller`位於的右側,該參數顯示在所需的值中。

## <a name="configuring-endpoint-metadata"></a>設定終結點中繼資料

以下連結提供有關設定終結點中繼資料的資訊:

* [使用端點路由啟用 Cors](xref:security/cors#enable-cors-with-endpoint-routing)
* 使用自訂`[MinimumAgeAuthorize]`屬性的[I 授權政策提供者範例](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [使用授權設定測試認證](xref:security/authentication/identity#test-identity)
* <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*>
* [使用授權設定方案](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)
* [使用授權設定設定規則](xref:security/authorization/policies#applying-policies-to-mvc-controllers)
* <xref:security/authorization/roles>

<a name="hostmatch"></a>

## <a name="host-matching-in-routes-with-requirehost"></a>與「需要主機」在路由中匹配主機

<xref:Microsoft.AspNetCore.Builder.RoutingEndpointConventionBuilderExtensions.RequireHost*>將約束應用於需要指定主機的路由。 `RequireHost`或[[主機]](xref:Microsoft.AspNetCore.Routing.HostAttribute)參數可以是:

* 主機: `www.domain.com``www.domain.com`, 與任何埠匹配。
* 具有通配符的`*.domain.com``www.domain.com`主機`subdomain.domain.com`:、`www.subdomain.domain.com`匹配 、或在任何埠上。
* 連接埠: `*:5000`, 符合連接埠 5000 與任何主機。
* 主機和埠:`www.domain.com:5000`或`*.domain.com:5000`匹配主機和埠。

可以使用`RequireHost``[Host]`或指定多個參數。 約束匹配對任何參數有效的主機。 例如,`[Host("domain.com", "*.domain.com")]``domain.com`符合`www.domain.com`與`subdomain.domain.com`。

以下代碼用於`RequireHost`要求在路由上指定主機:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRequireHost.cs?name=snippet)]

以下代碼使用控制器上`[Host]`的屬性要求任何指定的主機:

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/ProductController.cs?name=snippet)]

將`[Host]`屬性同時套用於控制器和操作方法時:

* 使用操作的屬性。
* 將忽略控制器屬性。

## <a name="performance-guidance-for-routing"></a>路由效能指南

大多數路由在 ASP.NET 酷 3.0 中更新,以提高性能。

當應用存在性能問題時,通常懷疑路由是問題所在。 懷疑路由的原因是,控制器和 Razor Pages 等框架在其日誌記錄消息中報告框架內花費的時間。 當控制器報告的時間與請求的總時間之間存在顯著差異時:

* 開發人員會消除他們的應用代碼,作為問題的根源。
* 通常假設路由是原因。

路由使用數千個終結點進行性能測試。 典型的應用不太可能僅僅因為太大而遇到性能問題。 路由性能緩慢的最常見的根本原因通常是行為不端的自定義中間件。

以下代碼範例演示了縮小延遲源的基本技術:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupDelay.cs?name=snippet)]

到時間路由:

* 將每個中間件與上述代碼中顯示的計時中間件的副本進行交錯。
* 添加唯一標識碼以將計時數據與代碼相關聯。

這是縮小延遲的基本方法,當它很重要時,例如,超過`10ms`。  從`Time 2``Time 1`報表中減`UseRouting`去 中間件內花費的時間。

以下代碼對前面的計時代碼使用更緊湊的方法:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupSW.cs?name=snippetSW)]

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupSW.cs?name=snippet)]

### <a name="potentially-expensive-routing-features"></a>潛在的昂貴路由功能

以下清單提供了一些與基本路由範本相比相對昂貴的路由功能的見解:

* 正則運算式:可以編寫複雜或運行時間長且輸入量少的正則運算式。

* 複雜欄位`{x}-{y}-{z}`( ): 
  * 比分析常規 URL 路徑段要昂貴得多。
  * 導致分配更多子字串。
  * 在核心 3.0 路由性能更新ASP.NET未更新複雜的段邏輯。

* 同步數據訪問:許多複雜應用具有資料庫訪問許可權,作為路由的一部分。 ASP.NET Core 2.2 和早期路由可能無法提供正確的擴展點來支援資料庫存取路由。 例如,<xref:Microsoft.AspNetCore.Routing.IRouteConstraint><xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>和是同步的。 擴展點,如<xref:Microsoft.AspNetCore.Routing.MatcherPolicy>和<xref:Microsoft.AspNetCore.Routing.EndpointSelectorContext>是異步的。

## <a name="guidance-for-library-authors"></a>圖書館作者指南

本節包含在路由之上構建的庫作者指南。 這些詳細資訊旨在確保應用開發人員使用擴展路由的庫和框架獲得良好的體驗。

### <a name="define-endpoints"></a>定義點

要建立使用路由進行 URL 匹配的框架,請首先定義<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>基於的用戶體驗。

**在**上建<xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>構 。 這允許使用者使用其他ASP.NET核心功能組成您的框架,而不會造成混淆。 每個ASP.NET核心範本都包括路由。 假設路由是存在的,並且使用者很熟悉。

```csharp
app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...);

    endpoints.MapHealthChecks("/healthz");
});
```

**一些從**調用`MapMyFramework(...)`該實現<xref:Microsoft.AspNetCore.Builder.IEndpointConventionBuilder>器 返回密封的混凝土類型。 大多數框架`Map...`方法都遵循此模式。 介面`IEndpointConventionBuilder`:

* 允許元數據的可組合性。
* 以各種擴充方法為目標。

以聲明自己的類型,可以向生成器添加您自己的特定於框架的功能。 可以包裝一個框架聲明的生成器,並將調用轉發給它。

```csharp
app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...).RequrireAuthorization()
                                 .WithMyFrameworkFeature(awesome: true);

    endpoints.MapHealthChecks("/healthz");
});
```

**考慮**寫你<xref:Microsoft.AspNetCore.Routing.EndpointDataSource>自己的 。 `EndpointDataSource`是用於聲明和更新終結點集合的低級基元。 `EndpointDataSource`是控制器和剃刀頁使用的強大 API。

路由測試具有不更新資料來源[的基本範例](https://github.com/aspnet/AspNetCore/blob/master/src/Http/Routing/test/testassets/RoutingSandbox/Framework/FrameworkEndpointDataSource.cs#L17)。

預設情況下**不要**試著`EndpointDataSource`註冊 。 要求使用者在中<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>註冊您的框架。 路由的原理是,默認情況下不包含任何內容,這就是`UseEndpoints`註冊終結點的位置。

### <a name="creating-routing-integrated-middleware"></a>建立路由整合式中間件

**考慮**將中繼資料類型定義為介面。

**DO**使將元數據類型用作類和方法的屬性成為可能。

[!code-csharp[](routing/samples/3.x/RoutingSample/ICoolMetadata.cs?name=snippet2)]

控制器和 Razor Pages 等框架支援將中繼資料屬性應用於類型和方法。 若宣告中繼資料型態:

* 使它們作為[屬性](/dotnet/csharp/programming-guide/concepts/attributes/)可訪問。
* 大多數使用者都熟悉應用屬性。

將中繼資料型態聲明為介面會增加另一層靈活性:

* 介面是可組合的。
* 開發人員可以聲明自己的類型,這些類型結合了多個策略。

**這樣做**可以覆蓋元數據,如以下範例所示:

[!code-csharp[](routing/samples/3.x/RoutingSample/ICoolMetadata.cs?name=snippet)]

遵循這些準則的最佳方法是避免定義**標籤中繼資料**:

* 不要只查找元數據類型的存在。
* 在元數據上定義屬性並檢查該屬性。

元數據收集按優先順序排序和支援重寫。 對於控制器,操作方法的元數據最為具體。

**一些有**中間件有用,沒有路由。

```csharp
app.UseRouting();

app.UseAuthorization(new AuthorizationPolicy() { ... });

app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...).RequrireAuthorization();
});
```

作為本指南的範例,`UseAuthorization`請考慮中間件。 授權中間件允許您傳遞回退策略。 <!-- shown where?  (shown here) --> 遞迴退原則 (如果指定)適用於這兩種策略:

* 沒有指定策略的終結點。
* 與終結點不匹配的請求。

這使得授權中間件在路由上下文之外很有用。 授權中間件可用於傳統的中間件編程。

[!INCLUDE[](~/includes/dbg-route.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

路由負責將請求 URI 映射到終結點,並將傳入請求發送到這些終結點。 路由定義於應用程式，並在該應用程式啟動時進行設定。 路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。 使用來自應用的路由資訊,路由還能夠生成映射到終結點的 URL。

若要使用 ASP.NET Core 2.2 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> 選項會決定路由功能應該在內部使用以端點為基礎的邏輯，或以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的邏輯 (適用於 ASP.NET Core 2.1 或更舊版本)。 當相容性版本設定為 2.2 或更新版本時，預設值為 `true`。 將值設定為 `false` 可使用舊版路由邏輯：

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

如需以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎之路由的詳細資訊，請參閱[本主題的 ASP.NET Core 2.1 版本](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1)。

> [!IMPORTANT]
> 本文件涵蓋低階的 ASP.NET Core 路由。 如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。 如需 Razor Pages 中路由慣例的資訊，請參閱 <xref:razor-pages/razor-pages-conventions>。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)([如何下載](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>路由的基本概念

大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。 預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：

* 支援基本的描述性路由配置。
* 適合作為 UI 型應用程式的起點。

開發人員通常使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專用常規路由在特殊情況下向應用的高流量區域添加其他簡潔的路由。 特殊情況示例包括博客和電子商務終結點。

Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。 這意味著同一邏輯資源上的許多操作(例如 GET 和 POST)使用相同的 URL。 屬性路由提供仔細設計 API 公用端點配置所需的控制層級。

Razor Pages 應用程式使用預設慣例路由，來提供應用程式 *Pages* 資料夾中的具名資源。 還有其他慣例可讓您自訂 Razor Pages 路由行為。 如需詳細資訊，請參閱 <xref:razor-pages/index> 和 <xref:razor-pages/razor-pages-conventions>。

URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。 這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。

路由使用*終結點*(`Endpoint`) 表示應用中的邏輯終結點。

端點會定義用來處理要求的一項委派及一個任意中繼資料集合。 中繼資料可根據附加至每個端點的原則和組態，來實作跨領域關注。

路由系統具有下列特性：

* 使用路由範本語法以 Token 化路由參數來定義路由。
* 允許傳統式和屬性式端點組態。
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。
* 應用程式模型 (例如 MVC/Razor Pages) 會註冊其所有端點，這些端點的路由情節實作符合預期。
* 路由實作會在中介軟體管線需要時制定路由決策。
* 在路由中介軟體之後出現的中介軟體可以檢查指定要求 URI 的路由中介軟體端點決策。
* 您可以針對中介軟體管線中任何位置的應用程式，列舉其中的所有端點。
* 應用程式可以根據端點資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。
* URL 是根據支援任意擴充性的位址所產生：

  * 您可以在任何位置使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 來解析連結產生器 API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>)，以產生 URL。
  * 如果無法透過 DI 使用連結產生器 API，<xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 會提供方法來建立 URL。

> [!NOTE]
> 在 ASP.NET Core 2.2 中發行端點路由時，端點連結限制在 MVC/Razor Pages 動作和頁面。 未來版本將規劃擴充端點連結功能。

路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。 [ASP.NET Core MVC](xref:mvc/overview) 會將路由新增至中介軟體管線，作為其組態的一部分，並處理 MVC 和 Razor Pages 應用程式中的路由。 若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。

### <a name="url-matching"></a>URL 比對

URL 比對是路由用來將傳入要求分派給「端點」** 的處理序。 這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。 分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。

端點路由中的路由系統負責制定所有分派決策。 由於中介軟體會根據所選取的端點來套用原則，因此請務必在路由系統內制定可能影響分派或應用安全性原則的任何決策。

執行端點委派時，會根據到目前為止所執行的要求處理，將 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) 的屬性設定為適當的值。

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」** 的字典，而路由值產生自路由。 這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。 提供了 <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。 這些是開發人員定義的值，**不會**以任何方式影響路由的行為。 此外，儲藏在 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 中的值可以是任何類型，對比之下，[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) 則必須可轉換成字串或可從字串轉換。

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) 是成功符合要求的參與路由清單。 路由可以用巢狀方式置於彼此內部。 <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。 一般而言，<xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的第一個項目是路由集合，應該用於產生 URL。 <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的最後一個項目是相符的路由處理常式。

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a>使用 LinkGenerator 產生 URL

URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。 這可讓您在端點和存取它們的 URL 之間建立邏輯分隔。

端點路由包含連結產生器 API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>)。 <xref:Microsoft.AspNetCore.Routing.LinkGenerator>是從[DI](xref:fundamentals/dependency-injection)檢索的單例服務。 您可以在執行要求內容外部使用此 API。 MVC 的 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 及依賴 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 的情節 (例如[標籤協助程式](xref:mvc/views/tag-helpers/intro)、HTML 協助程式和[動作結果](xref:mvc/controllers/actions)) 均使用連結產生器來提供連結產生功能。

連結產生器背後支援的概念為「位址」** 和「位址配置」**。 位址配置可讓您判斷應考慮用於連結產生的端點。 例如，MVC/Razor Pages 中許多使用者所熟悉的路由名稱和路由值情節，都會實作為位址配置。

連結產生器可以透過下列擴充方法，連結至 MVC/Razor Pages 動作和頁面：

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

這些方法的多載接受包含 `HttpContext` 的引數。 這些方法的功能等同於 `Url.Action` 和 `Url.Page`，但提供更多彈性和選項。

`GetPath*` 方法與 `Url.Action` 和 `Url.Page` 最類似，因為它們會產生包含絕對路徑的 URI。 `GetUri*` 方法一律會產生包含配置和主機的絕對 URI。 接受 `HttpContext` 的方法會在執行要求的內容中產生 URI。 除非遭到覆寫，否則會使用來自執行要求的環境路由值、URL 基底路徑、配置和主機。

呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 並指定一個位址。 執行下列兩個步驟來產生 URI：

1. 將位址繫結至符合該位址的端點清單。
1. 評估每個端點的 `RoutePattern`，直到找到符合所提供值的路由模式。 產生的輸出會與提供給連結產生器的其他 URI 組件合併並傳回。

<xref:Microsoft.AspNetCore.Routing.LinkGenerator> 提供的方法支援適用於任何位址類型的標準連結產生功能。 使用連結產生器的最便利方式是透過執行特定位址類型作業的擴充方法。

| 擴充方法   | 描述                                                         |
| ------------------ | ------------------------------------------------------------------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | 根據提供的值產生具有絕對路徑的 URI。 |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | 根據提供的值產生絕對 URI。             |

> [!WARNING]
> 注意呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 方法的下列影響：
>
> * 使用 `GetUri*` 擴充方法，並注意應用程式組態不會驗證傳入要求的 `Host` 標頭。 如果傳入要求的 `Host` 標頭未經驗證，則可能將未受信任的要求輸入傳回檢視/頁面 URI 中的用戶端。 建議所有生產應用程式將其伺服器設定為驗證 `Host` 標頭是否為已知有效值。
>
> * 使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator>，並注意與 `Map` 或 `MapWhen` 搭配使用的中介軟體。 `Map*` 會變更執行要求的基底路徑，這會影響連結產生的輸出。 所有的 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 都允許指定基底路徑。 請一律指定空白基底路徑來恢復 `Map*` 對連結產生的影響。

## <a name="differences-from-earlier-versions-of-routing"></a>與舊版路由的差異

ASP.NET Core 2.2 或更新版本中的端點路由與 ASP.NET Core 中的舊版路由之間有一些差異：

* 端點路由系統不支援以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的擴充性，包括從 <xref:Microsoft.AspNetCore.Routing.Route> 繼承。

* 端點路由不支援 [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)。 使用 2.1[相容性版本](xref:mvc/compatibility-version)(`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) 繼續使用相容性分片。

* 端點路由與使用傳統路由時所產生 URI 大小寫的行為不同。

  請考慮下列預設路由範本：

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  假設您使用下列路由來產生動作連結：

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  使用以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的路由，此程式碼會產生 `/blog/ReadPost/17` 的 URI，其遵守所提供路由值的大小寫。 ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17` ("Blog" 為大寫)。 端點路由提供 `IOutboundParameterTransformer` 介面，可用來全域自訂此行為，或為對應 URL 套用不同的慣例。

  如需詳細資訊，請參閱[參數轉換器參考](#parameter-transformer-reference)一節。

* 當嘗試連結至不存在的控制器/動作或頁面時，MVC/Razor Pages 所使用的連結產生與傳統路由的行為不同。

  請考慮下列預設路由範本：

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  假設您使用預設範本和下列程式碼來產生動作連結：

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  使用以 `IRouter` 為基礎的路由，結果一律為 `/Blog/ReadPost/17`，即使 `BlogController` 不存在或沒有 `ReadPost` 動作方法也一樣。 如預期，如果動作方法存在，則 ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17`。 不過，如果動作不存在，則端點路由會產生空字串。** 就概念而言，如果動作不存在，則端點路由不會假設端點存在。

* 連結產生「環境值失效演算法」** 在搭配端點路由使用時會有不同的行為。

  「環境值失效」** 是一種演算法，會從目前執行的要求 (環境值) 決定可用於連結產生作業的路由值。 傳統路由一律會在連結至其他動作時，使額外的路由值失效。 在 ASP.NET Core 2.2 版以前，屬性路由沒有此行為。 在舊版的 ASP.NET Core 中，連結至使用相同路由參數名稱的其他動作會導致連結產生錯誤。 在 ASP.NET Core 2.2 或更新版本中，這兩種路由形式都會在連結至其他動作時使值失效。

  請考慮 ASP.NET Core 2.1 或更舊版本中的下列範例。 連結至其他動作 (或其他頁面) 時，路由值可能會不適當地重複使用。

  在 */Pages/Store/Product.cshtml* 中：

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  在 */Pages/Login.cshtml* 中：

  ```cshtml
  @page "{id?}"
  ```

  如果 URI 在 ASP.NET Core 2.1 或更舊版本中為 `/Store/Product/18`，則 `@Url.Page("/Login")` 在 Store/Info 頁面中產生的連結為 `/Login/18`。 這會重複使用 `id` 值 18，即使連結目的地是完全不同的應用程式組件也一樣。 `/Login` 頁面內容中的 `id` 路由值可能是使用者識別碼值，而不是市集產品識別碼值。

  在 ASP.NET Core 2.2 或更新版本中的端點路由中，結果為 `/Login`。 當連結的目的地是不同的動作或頁面時，不會重複使用環境值。

* 往返路由參數語法：使用雙星號 (`**`) catch-all 參數語法時，斜線不會經過編碼。

  在連結產生期間，除了斜線，路由系統還會對雙星號 (`**`) catch-all 參數 (例如 `{**myparametername}`) 中擷取的值進行編碼。 ASP.NET Core 2.2 或更新版本中以 `IRouter` 為基礎的路由支援雙星號 catch-all。

  舊版 ASP.NET Core 中的單一星號 catch-all (`{*myparametername}`) 參數語法仍會受到支援，而且斜線會經過編碼。

  | 路由              | 以下列項目產生的連結：<br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | `/search/admin%2Fproducts` (斜線會經過編碼)             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a>中介軟體範例

在下列範例中，中介軟體使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 建立列出市集產品的動作方法連結。 應用程式中的任何類別都可以使用連結產生器，方法是將它插入類別並呼叫 `GenerateLink`。

```csharp
using Microsoft.AspNetCore.Routing;

public class ProductsLinkMiddleware
{
    private readonly LinkGenerator _linkGenerator;

    public ProductsLinkMiddleware(RequestDelegate next, LinkGenerator linkGenerator)
    {
        _linkGenerator = linkGenerator;
    }

    public async Task InvokeAsync(HttpContext httpContext)
    {
        var url = _linkGenerator.GetPathByAction("ListProducts", "Store");

        httpContext.Response.ContentType = "text/plain";

        await httpContext.Response.WriteAsync($"Go to {url} to see our products.");
    }
}
```

### <a name="create-routes"></a>建立路由

大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。 任何 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 不接受路由處理常式參數。 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。 若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。

下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 呼叫範例：

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

此範本會比對 URL 路徑，並擷取路由值。 例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。

路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」** 名稱來判定。 路由參數為具名。 參數是透過以括弧 `{ ... }` 括住參數名稱來定義。

上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。 發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。 路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。 路由參數名稱之後的問號 (`?`) 會定義選擇性參數。

在路由相符時，具有預設值的路由參數一定** 會產生路由值。 如果沒有相應的 URL 路徑段,可選參數不會生成路由值。 如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。

在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。 路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。 在此範例中，路由值 `id` 必須可以轉換為整數。 如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。 這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。

下列 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 範例會建立對等的路由：

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> 對於簡單的路由而言，定義條件約束和預設的內嵌語法可能很方便。 不過，內嵌語法不支援某些情節 (例如資料語彙基元)。

下列範例將示範一些其他情節：

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。 即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。 預設值可以在路由範本中指定。 `article` 路由參數透過在路由參數名稱之前加上雙星號 (`**`) 來定義為 *catch-all*。 全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。

下列範例會新增路由條件約束和資料語彙基元：

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

上述範本會比對 `/en-US/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>路由類別 URL 產生

<xref:Microsoft.AspNetCore.Routing.Route> 類別也可以結合一組路由值與其路由範本來產生 URL。 這在邏輯上是比對 URL 路徑的反向處理序。

> [!TIP]
> 若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。 產生的值為何？ 這與 URL 產生在 <xref:Microsoft.AspNetCore.Routing.Route> 類別中的運作方式大致相同。

下列範例使用一般 ASP.NET Core MVC 預設路由：

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

路由值為 `{ controller = Products, action = List }` 時，會產生 URL `/Products/List`。 路由值會取代對應的路由參數，以形成 URL 路徑。 由於 `id` 是選擇性路由參數，因此沒有 `id` 值也可以成功產生 URL。

路由值為 `{ controller = Home, action = Index }` 時，會產生 URL `/`。 所提供的路由值符合預設值，因此可以放心地省略這些預設值對應的區段。

這兩個產生的 URL 會使用下列路由定義 (`/Home/Index` 和 `/`) 反覆存取，並產生用來產生 URL 的相同路由值。

> [!NOTE]
> 使用 ASP.NET Core MVC 的應用程式應該使用 <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> 來產生 URL，而不是直接呼叫路由。

如需 URL 產生的詳細資訊，請參閱 [URI 產生參考](#url-generation-reference)一節。

## <a name="use-routing-middleware"></a>使用路由中介軟體

參考應用程式專案檔中的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。

在 `Startup.ConfigureServices` 中，將路由新增至服務容器：

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

路由必須設定在 `Startup.Configure` 方法中。 範例應用程式使用下列 API：

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>&ndash;僅匹配 HTTP GET 請求。
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

下表顯示使用指定 URL 的回應。

| URI                    | 回應                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Hello! Route values: [operation, create], [id, 3] |
| `/package/track/-3`    | Hello! Route values: [operation, track], [id, -3] |
| `/package/track/-3/`   | Hello! Route values: [operation, track], [id, -3] |
| `/package/track/`      | 要求失敗，沒有相符項目。              |
| `GET /hello/Joe`       | Hi, Joe!                                          |
| `POST /hello/Joe`      | 要求失敗，僅符合 HTTP GET。 |
| `GET /hello/Joe/Smith` | 要求失敗，沒有相符項目。              |

架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。 如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。

## <a name="route-template-reference"></a>路由範本參考

大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」**。 您可以在路由區段中定義多個路由參數，但其必須以常值分隔。 例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。 這些路由參數必須有一個名稱，並且可以指定其他屬性。

路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。 文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。 若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。

URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。 以範本 `files/{filename}.{ext?}` 為例。 當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。 如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。 下列 URL 符合此路由：

* `/files/myFile.txt`
* `/files/myFile`

您可以使用一個星號 (`*`) 或雙星號 (`**`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。 這稱為 *catch-all* 參數。 例如，`blog/{**slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。 全部擷取參數也可以符合空字串。

當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。 例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。 請注意逸出的斜線。 若要反覆存取路徑分隔符號字元，請使用 `**` 路由參數前置詞。 具有 `{ path = "my/path" }` 的路由 `foo/{**path}` 會產生 `foo/my/path`。

路由參數可能有「預設值」**，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。 例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。 如果 URL 中沒有用於參數的任何值，則會使用預設值。 路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。 選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。

路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。 在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」**。 如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。 指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。

條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。 例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。 如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。

路由參數也可以具有參數轉換器，以在產生連結及根據 URL 比對動作和頁面時，轉換參數的值。 與條件約束類似，可以透過在路徑參數名稱後面新增冒號 (`:`) 和轉換器名稱，將參數轉換器內嵌新增至路徑參數。 例如，路由範本 `blog/{article:slugify}` 會指定 `slugify` 轉換器。 如需參數轉換器的詳細資訊，請參閱[參數轉器參考](#parameter-transformer-reference)一節。

下表示範範例路由範本及其行為。

| 路由範本                           | 範例比對 URI    | 要求 URI&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | 只比對單一路徑 `/hello`。                                     |
| `{Page=Home}`                            | `/`                     | 比對並將 `Page` 設定為 `Home`。                                         |
| `{Page=Home}`                            | `/Contact`              | 比對並將 `Page` 設定為 `Contact`。                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | 對應至 `Products` 控制器和 `List` 動作。                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | 對應至 `Products` 控制器和 `Details` 動作 (`id` 設定為 123)。 |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | 對應至 `Home` 控制器和 `Index` 方法 (會忽略 `id`)。        |

使用範本通常是最簡單的路由方式。 條件約束和預設值也可以在路由範本外部指定。

> [!TIP]
> 啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。

## <a name="reserved-routing-names"></a>保留的路由名稱

下列關鍵字是保留的名稱，不能用作路由名稱或參數：

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>路由條件約束參考

路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。 路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出是/否決策。 某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。 例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。 條件約束可用於路由要求和連結產生。

> [!WARNING]
> 請勿針對**輸入驗證**使用條件約束。 如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」** 回應，而不是「400 - 錯誤要求」** 與適當的錯誤訊息。 路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。

下表示範範例路由條件約束及其預期行為。

| constraint (條件約束) | 範例 | 範例相符項目 | 注意 |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | 匹配任何整數。 |
| `bool` | `{active:bool}` | `true`, `FALSE` | 匹配`true`或「假」。。 不區分大小寫。 |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | 匹配不變區域性`DateTime`中的有效值。 請參閱前面的警告。|
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | 匹配不變區域性`decimal`中的有效值。 請參閱前面的警告。|
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | 匹配不變區域性`double`中的有效值。 請參閱前面的警告。|
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | 匹配不變區域性`float`中的有效值。 請參閱前面的警告。|
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | 匹配有效`Guid`值。 |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | 匹配有效`long`值。 |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | 字串必須至少為 4 個字元。 |
| `maxlength(value)` | `{filename:maxlength(8)}` | `MyFile` | 字串最多有 8 個字元。 |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | 字串必須正好為 12 個字元長。 |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | 字串必須至少為 8,並且最多有 16 個字元。 |
| `min(value)` | `{age:min(18)}` | `19` | 整數值必須至少為 18。 |
| `max(value)` | `{age:max(120)}` | `91` | 整數值最大值為 120。 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | 整數值必須至少為 18,最大值必須為 120。 |
| `alpha` | `{name:alpha}` | `Rick` | 字串必須由一個或多個字母字元`a`-`z`組成。  不區分大小寫。 |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | 字串必須與正則表達式匹配。 請參閱有關定義正則表達式的提示。 |
| `required` | `{name:required}` | `Rick` | 用於強制在 URL 生成期間存在非參數值。 |

以冒號分隔的多個條件約束，可以套用至單一參數。 例如，下列條件約束會將參數限制在 1 或更大的整數值：

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> 確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。 這些條件約束假設 URL 不可當地語系化。 架構提供的路由條件約束不會修改路由值中儲存的值。 所有從 URL 剖析而來的路由值會儲存為字串。 例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。

## <a name="regular-expressions"></a>規則運算式

ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。 如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。

正規表示式使用與路由和 C# 語言類似的分隔符和權杖。 規則運算式的語彙基元必須逸出。 要在路由中使用正規`^\d{3}-\d{2}-\d{4}$`表示式:

* 表達式必須在字串中作為原始碼中的雙`\`反斜杠字元提供單個反斜杠`\\`字元。
* 要轉義字串轉`\\`義字元,`\`必須使用正則運算式。
* 正規表示式在使用`\\`[逐字字串文本](/dotnet/csharp/language-reference/keywords/string)時不需要。

要轉義路由參數分隔符`{` `}` `[`, `]`、 、`{{``}``[[`, 將表示式`]]`的字元加倍, 、 、 下表顯示了正規表示式和轉義版本:

| 規則運算式    | 逸出的規則運算式     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

路由中使用的正則表達式通常以串字開頭`^`,並匹配字串的起始位置。 表達式通常以美元符號`$`字元和字串的匹配端結束。 `^` 和 `$` 字元可確保規則運算式符合整個路由參數值。 若不使用 `^` 與 `$` 字元，規則運算式會比對字串內的所有部分字串，這通常不是您想要的結果。 下表提供範例，並說明它們符合或無法符合的原因。

| 運算是   | String    | 相符項目 | 註解               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | hello     | 是   | 子字串相符項目     |
| `[a-z]{2}`   | 123abc456 | 是   | 子字串相符項目     |
| `[a-z]{2}`   | mz        | 是   | 符合運算式    |
| `[a-z]{2}`   | MZ        | 是   | 不區分大小寫    |
| `^[a-z]{2}$` | hello     | 否    | 請參閱上述的 `^` 和 `$` |
| `^[a-z]{2}$` | 123abc456 | 否    | 請參閱上述的 `^` 和 `$` |

如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。

若要將參數限制為一組已知的可能值，請使用規則運算式。 例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。 如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。 已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。

## <a name="custom-route-constraints"></a>自訂路由約束

除了內建的路由限制式之外，自訂路由限制式也可以透過實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面來建立。 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面包含單一方法 `Match`，此方法會在滿足限制式時傳回 `true`，否則會傳回 `false`。

若要使用自訂 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，路由限制式型別必須必須向應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> (在應用程式的服務容器中) 註冊。 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 實作。 更新應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。 例如：

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

限制式接著能以一般方式套用到路由 (使用註冊限制式型別時使用名稱)。 例如：

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="parameter-transformer-reference"></a>參數轉換器參考

參數轉換程式：

* 會在為 <xref:Microsoft.AspNetCore.Routing.Route> 產生連結時執行。
* 實作 `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`。
* 是使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定的。
* 採用參數的路由值，並將它轉換為新的字串值。
* 導致在所產生連結中使用已轉換的值。

例如，具有 `Url.Action(new { article = "MyTestArticle" })` 之路由模式 `blog\{article:slugify}` 中的自訂 `slugify` 參數轉換器，會產生 `blog\my-test-article`。

若要在路由模式中使用參數轉換器，請先在 `Startup.ConfigureServices` 中使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定：

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

架構會使用參數轉換器來轉換端點解析的 URI。 例如，ASP.NET Core MVC 會使用參數轉換器，轉換用於比對 `area`、`controller`、`action` 和 `page` 的路由值。

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

使用上述的路由，動作 `SubscriptionManagementController.GetAll` 會與URI `/subscription-management/get-all` 比對。 參數轉換器不會變更用來產生連結的路由值。 例如，`Url.Action("GetAll", "SubscriptionManagement")` 會輸出 `/subscription-management/get-all`。

ASP.NET Core 針對搭配產生的路由使用參數轉換程式提供了 API 慣例：

* ASP.NET Core MVC 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API 慣例。 此慣例會將所指定參數轉換程式套用到應用程式中的所有屬性路由。 參數轉換程式會在被取代時轉換屬性路由語彙基元。 如需詳細資訊，請參閱[使用參數轉換程式自訂語彙基元取代](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement)。
* Razor Pages 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API 慣例。 此慣例會將所指定參數轉換器套用至所有自動探索到的 Razor Pages。 參數轉換器會轉換 Razor Pages 路由的資料夾與檔案名稱區段。 如需詳細資訊，請參閱[使用參數轉換程式自訂頁面路由](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes)。

## <a name="url-generation-reference"></a>URL 產生參考

下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

在上述範例的結尾產生的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> 是 `/package/create/123`。 字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。 如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。

<xref:Microsoft.AspNetCore.Routing.VirtualPathContext> 建構函式的第二個參數是「環境值」** 的集合。 環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。 目前要求的目前路由值被視為用於連結產生的環境值。 在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。

不符合參數的環境值會予以忽略。 當明確提供的值覆寫環境值時，也會忽略環境值。 URL 中的比對是從左到右。

明確提供但不符合路由區段的值會新增至查詢字串。 下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。

| 環境值                     | 明確值                        | 結果                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| controller = "Home"                | action = "About"                       | `/Home/About`           |
| controller = "Home"                | controller = "Order", action = "About" | `/Order/About`          |
| controller = "Home", color = "Red" | action = "About"                       | `/Home/About`           |
| controller = "Home"                | action = "About", color = "Red"        | `/Home/About?color=Red` |

如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值：

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

連結產生只有在提供了 `controller` 與 `action` 的相符值時，才會產生此路由的連結。

## <a name="complex-segments"></a>複雜區段

複雜區段 (例如，`[Route("/x{token}y")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。 請參閱[此程式碼](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)以了解如何比對複雜區段的詳細解釋。 [程式法範例](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)不是由 ASP.NET Core 使用，但它提供一個好的複雜區段解釋。
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/dotnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end

::: moniker range="< aspnetcore-2.2"

路由功能負責將要求 URI 對應至路由處理常式，並分派傳入要求。 路由定義於應用程式，並在該應用程式啟動時進行設定。 路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。 使用應用程式中已設定的路由，路由功能將能夠產生對應至路由處理常式的 URL。

若要使用 ASP.NET Core 2.1 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

> [!IMPORTANT]
> 本文件涵蓋低階的 ASP.NET Core 路由。 如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。 如需 Razor Pages 中路由慣例的資訊，請參閱 <xref:razor-pages/razor-pages-conventions>。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)([如何下載](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>路由的基本概念

大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。 預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：

* 支援基本的描述性路由配置。
* 適合作為 UI 型應用程式的起點。

在特殊情況下，開發人員通常會使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專屬的慣例路由，新增額外的簡潔路由到應用程式的高流量區域 (例如，部落格和電子商務端點)。

Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。 這表示相同邏輯資源上的許多作業 (例如，GET、POST) 都會使用相同的 URL。 屬性路由提供仔細設計 API 公用端點配置所需的控制層級。

Razor Pages 應用程式使用預設慣例路由，來提供應用程式 *Pages* 資料夾中的具名資源。 還有其他慣例可讓您自訂 Razor Pages 路由行為。 如需詳細資訊，請參閱 <xref:razor-pages/index> 和 <xref:razor-pages/razor-pages-conventions>。

URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。 這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。

路由使用路由實現<xref:Microsoft.AspNetCore.Routing.IRouter>:

* 將傳入要求對應至「路由處理常式」**。
* 產生用於回應的 URL。

根據預設，應用程式有一個路由集合。 當要求抵達時，集合中的路由會依其存在於集合的順序進行處理。 此架構會嘗試對集合中的每個路由呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法，藉以比對傳入要求 URL 與集合中的路由。 回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。

路由系統具有下列特性：

* 使用路由範本語法以 Token 化路由參數來定義路由。
* 允許傳統式和屬性式端點組態。
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。
* 應用程式模型 (例如 MVC/Razor Pages) 會註冊其所有路由，這些路由的路由情節實作符合預期。
* 回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。
* URL 是根據支援任意擴充性的路由所產生。 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 提供方法來建立 URL。
<!-- fix [middleware](xref:fundamentals/middleware/index) -->
路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。 [ASP.NET Core MVC](xref:mvc/overview) 會將路由新增至中介軟體管線，作為其組態的一部分，並處理 MVC 和 Razor Pages 應用程式中的路由。 若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。

### <a name="url-matching"></a>URL 比對

URL 比對是路由用來將傳入要求分派給「處理常式」** 的處理序。 這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。 分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。

傳入要求將進入 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>，而後者會依序在每個路由上呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法。 <xref:Microsoft.AspNetCore.Routing.IRouter> 執行個體可將 [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) 設定為非 Null 的 <xref:Microsoft.AspNetCore.Http.RequestDelegate>，來選擇是否要「處理」** 要求。 如果路由為要求設定了處理常式，則路由處理會停止，且會叫用該處理常式來處理要求。 如果找不到處理要求的路由處理常式，中介軟體會將要求傳遞給要求管線中的下一個中介軟體。

<xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 的主要輸入是與目前要求建立關聯的 [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*)。 [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) 和 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) 是在比對路由之後設定的輸出。

呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 的比對也會根據到目前為止所執行的要求處理，將 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) 的屬性設定為適當的值。

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」** 的字典，而路由值產生自路由。 這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。 提供了 <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。 這些是開發人員定義的值，**不會**以任何方式影響路由的行為。 此外，儲藏在 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 中的值可以是任何類型，對比之下，[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) 則必須可轉換成字串或可從字串轉換。

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) 是成功符合要求的參與路由清單。 路由可以用巢狀方式置於彼此內部。 <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。 一般而言，<xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的第一個項目是路由集合，應該用於產生 URL。 <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的最後一個項目是相符的路由處理常式。

<a name="lg"></a>

### <a name="url-generation"></a>URL 產生

URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。 這可讓您在路由處理常式和存取它們的 URL 之間建立邏輯分隔。

URL 產生遵循類似的反覆執行處理序，但開頭是呼叫路由集合 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 方法的使用者或架構程式碼。 每個「路由」** 會依序呼叫其 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 方法，直到傳回非 Null 的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData> 為止。

<xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 的主要輸入是：

* [VirtualPathContext.HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext)
* [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues)

路由主要使用 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> 和 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> 所提供的路由值，以決定是否可能產生 URL，以及要包含哪些值。 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> 是比對目前要求所產生的路由值集合。 相反地，<xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> 是指定如何產生目前作業所需之 URL 的路由值。 提供了 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext>，以防路由應該取得服務或與目前內容建立關聯的其他資料。

> [!TIP]
> 將 [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) 視為 [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*) 的覆寫項目集合。 URL 產生會嘗試重複使用來自目前要求的路由值，讓您使用相同路由或路由值產生連結的 URL。

<xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 的輸出是 <xref:Microsoft.AspNetCore.Routing.VirtualPathData>。 <xref:Microsoft.AspNetCore.Routing.VirtualPathData> 是 <xref:Microsoft.AspNetCore.Routing.RouteData> 的平行處理。 <xref:Microsoft.AspNetCore.Routing.VirtualPathData> 包含用於輸出 URL 的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath>，以及一些路由應該設定的其他屬性。

[VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) 屬性包含路由所產生的「虛擬路徑」**。 視您需求的不同，可能需要進一步處理路徑。 如果您想要以 HTML 呈現產生的 URL，請在前面加上應用程式的基底路徑。

[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) 是成功產生 URL 的路由參考。

[VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) 屬性是其他資料的字典，而這些資料與產生 URL 的路由相關。 這是 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 的平行處理。

### <a name="create-routes"></a>建立路由

路由提供 <xref:Microsoft.AspNetCore.Routing.Route> 類別作為 <xref:Microsoft.AspNetCore.Routing.IRouter> 的標準實作。 在呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 時，<xref:Microsoft.AspNetCore.Routing.Route> 會使用「路由範本」** 語法來定義將比對 URL 路徑的模式。 在呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 時，<xref:Microsoft.AspNetCore.Routing.Route> 會使用相同的路由範本來產生 URL。

大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。 任何 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 不接受路由處理常式參數。 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。 預設處理常式為 `IRouter`，該處理常式可能無法處理要求。 例如，ASP.NET Core MVC 通常會設定為預設處理常式，只處理符合可用控制器和動作的要求。 若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。

下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 呼叫範例：

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

此範本會比對 URL 路徑，並擷取路由值。 例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。

路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」** 名稱來判定。 路由參數為具名。 參數是透過以括弧 `{ ... }` 括住參數名稱來定義。

上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。 發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。 路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。 路由參數名稱之後的問號 (`?`) 會定義選擇性參數。

在路由相符時，具有預設值的路由參數一定** 會產生路由值。 如果沒有相應的 URL 路徑段,可選參數不會生成路由值。 如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。

在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。 路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。 在此範例中，路由值 `id` 必須可以轉換為整數。 如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。 這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。

下列 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 範例會建立對等的路由：

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> 對於簡單的路由而言，定義條件約束和預設的內嵌語法可能很方便。 不過，內嵌語法不支援某些情節 (例如資料語彙基元)。

下列範例將示範一些其他情節：

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。 即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。 預設值可以在路由範本中指定。 `article` 路由參數透過在路由參數名稱之前加上一個星號 (`*`) 來定義為 *catch-all*。 全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。

下列範例會新增路由條件約束和資料語彙基元：

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

上述範本會比對 `/en-US/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>路由類別 URL 產生

<xref:Microsoft.AspNetCore.Routing.Route> 類別也可以結合一組路由值與其路由範本來產生 URL。 這在邏輯上是比對 URL 路徑的反向處理序。

> [!TIP]
> 若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。 產生的值為何？ 這與 URL 產生在 <xref:Microsoft.AspNetCore.Routing.Route> 類別中的運作方式大致相同。

下列範例使用一般 ASP.NET Core MVC 預設路由：

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

路由值為 `{ controller = Products, action = List }` 時，會產生 URL `/Products/List`。 路由值會取代對應的路由參數，以形成 URL 路徑。 由於 `id` 是選擇性路由參數，因此沒有 `id` 值也可以成功產生 URL。

路由值為 `{ controller = Home, action = Index }` 時，會產生 URL `/`。 所提供的路由值符合預設值，因此可以放心地省略這些預設值對應的區段。

這兩個產生的 URL 會使用下列路由定義 (`/Home/Index` 和 `/`) 反覆存取，並產生用來產生 URL 的相同路由值。

> [!NOTE]
> 使用 ASP.NET Core MVC 的應用程式應該使用 <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> 來產生 URL，而不是直接呼叫路由。

如需 URL 產生的詳細資訊，請參閱 [URI 產生參考](#url-generation-reference)一節。

## <a name="use-routing-middleware"></a>使用路由中間件

參考應用程式專案檔中的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。

在 `Startup.ConfigureServices` 中，將路由新增至服務容器：

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

路由必須設定在 `Startup.Configure` 方法中。 範例應用程式使用下列 API：

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>&ndash;僅匹配 HTTP GET 請求。
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

下表顯示使用指定 URL 的回應。

| URI                    | 回應                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Hello! Route values: [operation, create], [id, 3] |
| `/package/track/-3`    | Hello! Route values: [operation, track], [id, -3] |
| `/package/track/-3/`   | Hello! Route values: [operation, track], [id, -3] |
| `/package/track/`      | 要求失敗，沒有相符項目。              |
| `GET /hello/Joe`       | Hi, Joe!                                          |
| `POST /hello/Joe`      | 要求失敗，僅符合 HTTP GET。 |
| `GET /hello/Joe/Smith` | 要求失敗，沒有相符項目。              |

如果您要設定單一路由，請呼叫傳入 `IRouter` 執行個體的 <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>。 您不需要使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder>。

架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

其中一些列出的方法 (例如 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>) 需要 <xref:Microsoft.AspNetCore.Http.RequestDelegate>。 路由相符時，<xref:Microsoft.AspNetCore.Http.RequestDelegate> 會作為「路由處理常式」** 使用。 此系列中的其他方法允許設定中介軟體管線，以作為路由處理常式使用。 如果 `Map*` 方法不接受處理常式 (例如 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>)，則會使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>。

`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。 如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。

## <a name="route-template-reference"></a>路由範本參考

大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」**。 您可以在路由區段中定義多個路由參數，但其必須以常值分隔。 例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。 這些路由參數必須有一個名稱，並且可以指定其他屬性。

路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。 文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。 若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。

URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。 以範本 `files/{filename}.{ext?}` 為例。 當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。 如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。 下列 URL 符合此路由：

* `/files/myFile.txt`
* `/files/myFile`

您可以使用星號 (`*`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。 這稱為「全部擷取」** 參數。 例如，`blog/{*slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。 全部擷取參數也可以符合空字串。

當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。 例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。 請注意逸出的斜線。

路由參數可能有「預設值」**，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。 例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。 如果 URL 中沒有用於參數的任何值，則會使用預設值。 路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。 選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。

路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。 在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」**。 如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。 指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。

條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。 例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。 如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。

下表示範範例路由範本及其行為。

| 路由範本                           | 範例比對 URI    | 要求 URI&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | 只比對單一路徑 `/hello`。                                     |
| `{Page=Home}`                            | `/`                     | 比對並將 `Page` 設定為 `Home`。                                         |
| `{Page=Home}`                            | `/Contact`              | 比對並將 `Page` 設定為 `Contact`。                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | 對應至 `Products` 控制器和 `List` 動作。                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | 對應至 `Products` 控制器和 `Details` 動作 (`id` 設定為 123)。 |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | 對應至 `Home` 控制器和 `Index` 方法 (會忽略 `id`)。        |

使用範本通常是最簡單的路由方式。 條件約束和預設值也可以在路由範本外部指定。

> [!TIP]
> 啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。

## <a name="route-constraint-reference"></a>路由條件約束參考

路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。 路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出是/否決策。 某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。 例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。 條件約束可用於路由要求和連結產生。

> [!WARNING]
> 請勿針對**輸入驗證**使用條件約束。 如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」** 回應，而不是「400 - 錯誤要求」** 與適當的錯誤訊息。 路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。

下表示範範例路由條件約束及其預期行為。

| constraint (條件約束) | 範例 | 範例相符項目 | 注意 |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | 符合任何整數 |
| `bool` | `{active:bool}` | `true`, `FALSE` | 符合 `true` 或 `false` (不區分大小寫) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | 匹配不變區域性`DateTime`中的有效值。 請參閱前面的警告。|
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | 匹配不變區域性`decimal`中的有效值。 請參閱前面的警告。|
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | 匹配不變區域性`double`中的有效值。 請參閱前面的警告。|
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | 匹配不變區域性`float`中的有效值。 請參閱前面的警告。|
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | 符合有效的 `Guid` 值 |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | 符合有效的 `long` 值 |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | 字串必須至少有 4 個字元 |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | 字串不能超過 8 個字元 |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | 字串長度必須剛好是 12 個字元 |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | 字串長度必須至少有 8 個字元，但不能超過 16 個字元 |
| `min(value)` | `{age:min(18)}` | `19` | 整數值必須至少為 18 |
| `max(value)` | `{age:max(120)}` | `91` | 整數值不能超過 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | 整數值必須至少為 18，但不能超過 120 |
| `alpha` | `{name:alpha}` | `Rick` | 字串必須包含一或多個字母字元 (`a`-`z`，不區分大小寫) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | 字串必須符合規則運算式 (請參閱有關定義規則運算式的提示) |
| `required` | `{name:required}` | `Rick` | 用來強制執行在 URL 產生期間呈現非參數值 |

以冒號分隔的多個條件約束，可以套用至單一參數。 例如，下列條件約束會將參數限制在 1 或更大的整數值：

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> 確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。 這些條件約束假設 URL 不可當地語系化。 架構提供的路由條件約束不會修改路由值中儲存的值。 所有從 URL 剖析而來的路由值會儲存為字串。 例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。

## <a name="regular-expressions"></a>規則運算式

ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。 如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。

規則運算式使用的分隔符號和語彙基元，類似於路由和 C# 語言所使用的分隔符號和語彙基元。 規則運算式的語彙基元必須逸出。 若要在路由中使用規則運算式 `^\d{3}-\d{2}-\d{4}$`，運算式必須以字串中所提供的 `\` (單一反斜線) 字元作為 C# 原始程式檔中的 `\\` (雙反斜線) 字元，才能逸出 `\` 字串逸出字元 (除非使用[逐字字串常值](/dotnet/csharp/language-reference/keywords/string))。 若要逸出路由參數分隔符號字元 (`{`、`}`、`[`、`]`)，請在運算式中使用雙字元 (`{{`、`}`、`[[`、`]]`). 下表顯示規則運算式和逸出的版本。

| 規則運算式    | 逸出的規則運算式     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

路由中所使用的規則運算式通常以插入號 (`^`) 字元開頭，並符合字串的開始位置。 運算式通常以貨幣符號 (`$`) 字元結尾，並符合字串的結尾。 `^` 和 `$` 字元可確保規則運算式符合整個路由參數值。 若不使用 `^` 與 `$` 字元，規則運算式會比對字串內的所有部分字串，這通常不是您想要的結果。 下表提供範例，並說明它們符合或無法符合的原因。

| 運算是   | String    | 相符項目 | 註解               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | hello     | 是   | 子字串相符項目     |
| `[a-z]{2}`   | 123abc456 | 是   | 子字串相符項目     |
| `[a-z]{2}`   | mz        | 是   | 符合運算式    |
| `[a-z]{2}`   | MZ        | 是   | 不區分大小寫    |
| `^[a-z]{2}$` | hello     | 否    | 請參閱上述的 `^` 和 `$` |
| `^[a-z]{2}$` | 123abc456 | 否    | 請參閱上述的 `^` 和 `$` |

如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。

若要將參數限制為一組已知的可能值，請使用規則運算式。 例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。 如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。 已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。

## <a name="custom-route-constraints"></a>自訂路由限制式

除了內建的路由限制式之外，自訂路由限制式也可以透過實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面來建立。 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面包含單一方法 `Match`，此方法會在滿足限制式時傳回 `true`，否則會傳回 `false`。

若要使用自訂 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，路由限制式型別必須必須向應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> (在應用程式的服務容器中) 註冊。 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 實作。 更新應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。 例如：

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

限制式接著能以一般方式套用到路由 (使用註冊限制式型別時使用名稱)。 例如：

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="url-generation-reference"></a>URL 產生參考

下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

在上述範例的結尾產生的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> 是 `/package/create/123`。 字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。 如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。

<xref:Microsoft.AspNetCore.Routing.VirtualPathContext> 建構函式的第二個參數是「環境值」** 的集合。 環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。 目前要求的目前路由值被視為用於連結產生的環境值。 在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。

不符合參數的環境值會予以忽略。 當明確提供的值覆寫環境值時，也會忽略環境值。 URL 中的比對是從左到右。

明確提供但不符合路由區段的值會新增至查詢字串。 下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。

| 環境值                     | 明確值                        | 結果                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| controller = "Home"                | action = "About"                       | `/Home/About`           |
| controller = "Home"                | controller = "Order", action = "About" | `/Order/About`          |
| controller = "Home", color = "Red" | action = "About"                       | `/Home/About`           |
| controller = "Home"                | action = "About", color = "Red"        | `/Home/About?color=Red` |

如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值：

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

連結產生只有在提供了 `controller` 與 `action` 的相符值時，才會產生此路由的連結。

## <a name="complex-segments"></a>複雜區段

複雜區段 (例如，`[Route("/x{token}y")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。 請參閱[此程式碼](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)以了解如何比對複雜區段的詳細解釋。 [程式法範例](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)不是由 ASP.NET Core 使用，但它提供一個好的複雜區段解釋。

::: moniker-end
