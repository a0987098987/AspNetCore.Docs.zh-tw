---
title: ASP.NET Core 中的路由
author: rick-anderson
description: 探索 ASP.NET Core 路由功能如何負責將要求 URI 對應至端點選取器，並將傳入要求分派給端點。
ms.author: riande
ms.custom: mvc
ms.date: 12/29/2018
uid: fundamentals/routing
ms.openlocfilehash: c57b309e4474f9aff5c0594a3d9d1c796990d31e
ms.sourcegitcommit: e1cc4c1ef6c9e07918a609d5ad7fadcb6abe3e12
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2019
ms.locfileid: "53997353"
---
# <a name="routing-in-aspnet-core"></a>ASP.NET Core 中的路由

作者：[Ryan Nowak](https://github.com/rynowak)、[Steve Smith](https://ardalis.com/) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="<= aspnetcore-1.1"

如需本主題的 1.1 版，請下載 [ASP.NET Core 中的路由 (1.1 版，PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Routing_1.x.pdf)。

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

路由功能負責將要求 URI 對應至端點選取器，並將傳入要求分派給端點。 路由定義於應用程式，並在該應用程式啟動時進行設定。 路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。 使用應用程式中的路由資訊，路由功能也能夠產生對應至端點選取器的 URL。

::: moniker-end

::: moniker range="= aspnetcore-2.2"

若要使用 ASP.NET Core 2.2 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

`EnableEndpointRouting` 選項會決定路由功能應該在內部使用以端點為基礎的邏輯，或以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的邏輯 (適用於 ASP.NET Core 2.1 或更舊版本)。 當相容性版本設定為 2.2 或更新版本時，預設值為 `true`。 將值設定為 `false` 可使用舊版路由邏輯：

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

如需以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎之路由的詳細資訊，請參閱[本主題的 ASP.NET Core 2.1 版本](xref:fundamentals/routing?view=aspnetcore-2.1)。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

路由功能負責將要求 URI 對應至路由處理常式，並分派傳入要求。 路由定義於應用程式，並在該應用程式啟動時進行設定。 路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。 使用應用程式中已設定的路由，路由功能將能夠產生對應至路由處理常式的 URL。

::: moniker-end

::: moniker range="= aspnetcore-2.1"

若要使用 ASP.NET Core 2.1 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

::: moniker-end

> [!IMPORTANT]
> 本文件涵蓋低階的 ASP.NET Core 路由。 如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。 如需 Razor Pages 中路由慣例的資訊，請參閱 <xref:razor-pages/razor-pages-conventions>。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>路由的基本概念

大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。 預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：

* 支援基本的描述性路由配置。
* 適合作為 UI 型應用程式的起點。

在特殊情況下，開發人員通常會使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專屬的慣例路由，新增額外的簡潔路由到應用程式的高流量區域 (例如，部落格和電子商務端點)。

Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。 這表示相同邏輯資源上的許多作業 (例如，GET、POST) 都會使用相同的 URL。 屬性路由提供仔細設計 API 公用端點配置所需的控制層級。

Razor Pages 應用程式使用預設慣例路由，來提供應用程式 *Pages* 資料夾中的具名資源。 還有其他慣例可讓您自訂 Razor Pages 路由行為。 如需詳細資訊，請參閱 <xref:razor-pages/index> 與 <xref:razor-pages/razor-pages-conventions>。

URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。 這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。

::: moniker range=">= aspnetcore-2.2"

路由功能使用「端點」(`Endpoint`) 來表示應用程式中的邏輯端點。

端點會定義用來處理要求的一項委派及一個任意中繼資料集合。 中繼資料可根據附加至每個端點的原則和組態，來實作跨領域關注。

路由系統具有下列特性：

* 使用路由範本語法以 Token 化路由參數來定義路由。
* 允許傳統式和屬性式端點組態。
* `IRouteConstraint` 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。
* 應用程式模型 (例如 MVC/Razor Pages) 會註冊其所有端點，這些端點的路由情節實作符合預期。
* 路由實作會在中介軟體管線需要時制定路由決策。
* 在路由中介軟體之後出現的中介軟體可以檢查指定要求 URI 的路由中介軟體端點決策。
* 您可以針對中介軟體管線中任何位置的應用程式，列舉其中的所有端點。
* 應用程式可以根據端點資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。
* URL 是根據支援任意擴充性的位址所產生：

  * 您可以在任何位置使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 來解析連結產生器 API (`LinkGenerator`)，以產生 URL。
  * 如果無法透過 DI 使用連結產生器 API，`IUrlHelper` 會提供方法來建立 URL。

> [!NOTE]
> 在 ASP.NET Core 2.2 中發行端點路由時，端點連結限制在 MVC/Razor Pages 動作和頁面。 未來版本將規劃擴充端點連結功能。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

路由 (routing) 功能會使用「路由」(*route*) (<xref:Microsoft.AspNetCore.Routing.IRouter> 的實作) 來：

* 將傳入要求對應至「路由處理常式」。
* 產生用於回應的 URL。

根據預設，應用程式有一個路由集合。 當要求抵達時，集合中的路由會依其存在於集合的順序進行處理。 此架構會嘗試對集合中的每個路由呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法，藉以比對傳入要求 URL 與集合中的路由。 回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。

路由系統具有下列特性：

* 使用路由範本語法以 Token 化路由參數來定義路由。
* 允許傳統式和屬性式端點組態。
* `IRouteConstraint` 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。
* 應用程式模型 (例如 MVC/Razor Pages) 會註冊其所有路由，這些路由的路由情節實作符合預期。
* 回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。
* URL 是根據支援任意擴充性的路由所產生。 `IUrlHelper` 提供方法來建立 URL。

::: moniker-end

路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。 [ASP.NET Core MVC](xref:mvc/overview) 會將路由新增至中介軟體管線，作為其組態的一部分，並處理 MVC 和 Razor Pages 應用程式中的路由。 若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。

### <a name="url-matching"></a>URL 比對

::: moniker range=">= aspnetcore-2.2"

URL 比對是路由用來將傳入要求分派給「端點」的處理序。 這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。 分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。

端點路由中的路由系統負責制定所有分派決策。 由於中介軟體會根據所選取的端點來套用原則，因此請務必在路由系統內制定可能影響分派或應用安全性原則的任何決策。

執行端點委派時，請依據到目前為止執行的要求處理，將 `RouteContext.RouteData` 的屬性設定為適當值。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

URL 比對是路由用來將傳入要求分派給「處理常式」的處理序。 這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。 分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。

傳入要求將進入 `RouterMiddleware`，而後者會依序在每個路由上呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法。 <xref:Microsoft.AspNetCore.Routing.IRouter> 執行個體可將 [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) 設定為非 Null 的 <xref:Microsoft.AspNetCore.Http.RequestDelegate>，來選擇是否要「處理」要求。 如果路由為要求設定了處理常式，則路由處理會停止，且會叫用該處理常式來處理要求。 如果找不到處理要求的路由處理常式，中介軟體會將要求傳遞給要求管線中的下一個中介軟體。

`RouteAsync` 的主要輸入是與目前要求建立關聯的 [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*)。 `RouteContext.Handler` 和 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) 是路由相符之後設定的輸出。

呼叫 `RouteAsync` 的相符項目也會依據到目前為止執行的要求處理，將 `RouteContext.RouteData` 的屬性設定為適當值。

::: moniker-end

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」的字典，而路由值產生自路由。 這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。 提供了 `DataTokens` 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。 這些是開發人員定義的值，**不會**以任何方式影響路由的行為。 此外，隱藏於 `RouteData.DataTokens` 的值可以為任何類型，相較於 `RouteData.Values`，其必須能夠來回轉換字串。

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers*) 是成功符合要求的參與路由清單。 路由可以用巢狀方式置於彼此內部。 `Routers` 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。 一般而言，`Routers` 中的第一個項目是路由集合，應該用於產生 URL。 `Routers` 中的最後一個項目是相符的路由處理常式。

### <a name="url-generation"></a>URL 產生

::: moniker range=">= aspnetcore-2.2"

URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。 這可讓您在端點和存取它們的 URL 之間建立邏輯分隔。

端點路由包含連結產生器 API (`LinkGenerator`)。 `LinkGenerator` 是可從 DI 擷取的單一服務。 您可以在執行要求內容外部使用此 API。 MVC 的 `IUrlHelper` 及依賴 `IUrlHelper` 的情節 (例如[標籤協助程式](xref:mvc/views/tag-helpers/intro)、HTML 協助程式和[動作結果](xref:mvc/controllers/actions)) 均使用連結產生器來提供連結產生功能。

連結產生器背後支援的概念為「位址」和「位址配置」。 位址配置可讓您判斷應考慮用於連結產生的端點。 例如，MVC/Razor Pages 中許多使用者所熟悉的路由名稱和路由值情節，都會實作為位址配置。

連結產生器可以透過下列擴充方法，連結至 MVC/Razor Pages 動作和頁面：

* `GetPathByAction`
* `GetUriByAction`
* `GetPathByPage`
* `GetUriByPage`

這些方法的多載接受包含 `HttpContext` 的引數。 這些方法的功能等同於 `Url.Action` 和 `Url.Page`，但提供更多彈性和選項。

`GetPath*` 方法與 `Url.Action` 和 `Url.Page` 最類似，因為它們會產生包含絕對路徑的 URI。 `GetUri*` 方法一律會產生包含配置和主機的絕對 URI。 接受 `HttpContext` 的方法會在執行要求的內容中產生 URI。 除非遭到覆寫，否則會使用來自執行要求的環境路由值、URL 基底路徑、配置和主機。

呼叫 `LinkGenerator` 並指定一個位址。 執行下列兩個步驟來產生 URI：

1. 將位址繫結至符合該位址的端點清單。
1. 評估每個端點的 `RoutePattern`，直到找到符合所提供值的路由模式。 產生的輸出會與提供給連結產生器的其他 URI 組件合併並傳回。

`LinkGenerator` 提供的方法支援適用於任何位址類型的標準連結產生功能。 使用連結產生器的最便利方式是透過執行特定位址類型作業的擴充方法。

| 擴充方法   | 說明                                                         |
| ------------------ | ------------------------------------------------------------------- |
| `GetPathByAddress` | 根據提供的值產生具有絕對路徑的 URI。 |
| `GetUriByAddress`  | 根據提供的值產生絕對 URI。             |

> [!WARNING]
> 注意呼叫 `LinkGenerator` 方法的下列影響：
>
> * 使用 `GetUri*` 擴充方法，並注意應用程式組態不會驗證傳入要求的 `Host` 標頭。 如果傳入要求的 `Host` 標頭未經驗證，則可能將未受信任的要求輸入傳回檢視/頁面 URI 中的用戶端。 建議所有生產應用程式將其伺服器設定為驗證 `Host` 標頭是否為已知有效值。
>
> * 使用 `LinkGenerator`，並注意與 `Map` 或 `MapWhen` 搭配使用的中介軟體。 `Map*` 會變更執行要求的基底路徑，這會影響連結產生的輸出。 所有的 `LinkGenerator` API 都允許指定基底路徑。 請一律指定空白基底路徑來恢復 `Map*` 對連結產生的影響。

## <a name="differences-from-earlier-versions-of-routing"></a>與舊版路由的差異

ASP.NET Core 2.2 或更新版本中的端點路由與 ASP.NET Core 中的舊版路由之間有一些差異：

* 端點路由系統不支援以 `IRouter` 為基礎的擴充性，包括從 `Route` 繼承。

* 端點路由不支援 [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)。 請使用 2.1 [相容性版本](xref:mvc/compatibility-version) (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) 以繼續使用相容性填充碼。

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

  使用以 `IRouter` 為基礎的路由，此程式碼會產生 `/blog/ReadPost/17` 的 URI，其遵守所提供路由值的大小寫。 ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17` ("Blog" 為大寫)。 端點路由提供 `IOutboundParameterTransformer` 介面，可用來全域自訂此行為，或為對應 URL 套用不同的慣例。

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

  使用以 `IRouter` 為基礎的路由，結果一律為 `/Blog/ReadPost/17`，即使 `BlogController` 不存在或沒有 `ReadPost` 動作方法也一樣。 如預期，如果動作方法存在，則 ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17`。 不過，如果動作不存在，則端點路由會產生空字串。 就概念而言，如果動作不存在，則端點路由不會假設端點存在。

* 連結產生「環境值失效演算法」在搭配端點路由使用時會有不同的行為。

  「環境值失效」是一種演算法，會從目前執行的要求 (環境值) 決定可用於連結產生作業的路由值。 傳統路由一律會在連結至其他動作時，使額外的路由值失效。 在 ASP.NET Core 2.2 版以前，屬性路由沒有此行為。 在舊版的 ASP.NET Core 中，連結至使用相同路由參數名稱的其他動作會導致連結產生錯誤。 在 ASP.NET Core 2.2 或更新版本中，這兩種路由形式都會在連結至其他動作時使值失效。

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

在下列範例中，中介軟體使用 `LinkGenerator` API 建立列出市集產品的動作方法連結。 應用程式中的任何類別都可以使用連結產生器，方法是將它插入類別並呼叫 `GenerateLink`。

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

::: moniker-end

::: moniker range="< aspnetcore-2.2"

URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。 這可讓您在路由處理常式和存取它們的 URL 之間建立邏輯分隔。

URL 產生遵循類似的反覆執行處理序，但開頭是呼叫路由集合 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 方法的使用者或架構程式碼。 每個「路由」會依序呼叫其 `GetVirtualPath` 方法，直到傳回非 Null 的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData> 為止。

`GetVirtualPath` 的主要輸入是：

* [VirtualPathContext.HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext*)
* [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*)

路由主要使用 `Values` 和 `AmbientValues` 所提供的路由值，以決定是否可能產生 URL，以及要包含哪些值。 `AmbientValues` 是比對目前要求所產生的路由值集合。 相反地，`Values` 是指定如何產生目前作業所需之 URL 的路由值。 提供了 `HttpContext`，以防路由應該取得服務或與目前內容建立關聯的其他資料。

> [!TIP]
> 將 [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) 視為 [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*) 的覆寫項目集合。 URL 產生會嘗試重複使用來自目前要求的路由值，讓您使用相同路由或路由值產生連結的 URL。

`GetVirtualPath` 的輸出是 `VirtualPathData`。 `VirtualPathData` 是 `RouteData` 的平行處理。 `VirtualPathData` 包含用於輸出 URL 的 `VirtualPath`，以及一些路由應該設定的其他屬性。

[VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) 屬性包含路由所產生的「虛擬路徑」。 視您需求的不同，可能需要進一步處理路徑。 如果您想要以 HTML 呈現產生的 URL，請在前面加上應用程式的基底路徑。

[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) 是成功產生 URL 的路由參考。

[VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) 屬性是其他資料的字典，而這些資料與產生 URL 的路由相關。 這是 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 的平行處理。

::: moniker-end

### <a name="create-routes"></a>建立路由

::: moniker range="< aspnetcore-2.2"

路由提供 <xref:Microsoft.AspNetCore.Routing.Route> 類別作為 <xref:Microsoft.AspNetCore.Routing.IRouter> 的標準實作。 在呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 時，`Route` 會使用「路由範本」語法來定義將比對 URL 路徑的模式。 在呼叫 `GetVirtualPath` 時，`Route` 會使用相同的路由範本來產生 URL。

::: moniker-end

大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。 任何 `IRouteBuilder` 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。

::: moniker range=">= aspnetcore-2.2"

`MapRoute` 不接受路由處理常式參數。 `MapRoute` 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。 若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

`MapRoute` 不接受路由處理常式參數。 `MapRoute` 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。 預設處理常式為 `IRouter`，該處理常式可能無法處理要求。 例如，ASP.NET Core MVC 通常會設定為預設處理常式，只處理符合可用控制器和動作的要求。 若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。

::: moniker-end

下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 `MapRoute` 呼叫範例：

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

此範本會比對 URL 路徑，並擷取路由值。 例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。

路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」名稱來判定。 路由參數為具名。 參數是透過以括弧 `{ ... }` 括住參數名稱來定義。

上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。 發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。 路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。 路由參數名稱之後的問號 (`?`) 會定義選擇性參數。

在路由相符時，具有預設值的路由參數一定會產生路由值。 如果沒有任何對應的 URL 路徑區段，選擇性參數不會產生路由值。 如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。

在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。 路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。 在此範例中，路由值 `id` 必須可以轉換為整數。 如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。

`MapRoute` 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。 這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。

下列 `MapRoute` 範例會建立對等的路由：

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

::: moniker range=">= aspnetcore-2.2"

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。 即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。 預設值可以在路由範本中指定。 `article` 路由參數透過在路由參數名稱之前加上雙星號 (`**`) 來定義為 *catch-all*。 全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。 即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。 預設值可以在路由範本中指定。 `article` 路由參數透過在路由參數名稱之前加上一個星號 (`*`) 來定義為 *catch-all*。 全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。

::: moniker-end

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

`Route` 類別也可以結合一組路由值與其路由範本來產生 URL。 這在邏輯上是比對 URL 路徑的反向處理序。

> [!TIP]
> 若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。 產生的值為何？ 這與 URL 產生在 `Route` 類別中的運作方式大致相同。

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
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; 僅符合 HTTP GET 要求。
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

::: moniker range="< aspnetcore-2.2"

如果您要設定單一路由，請呼叫傳入 `IRouter` 執行個體的 <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>。 您不需要使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder>。

::: moniker-end

架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：

* `MapDelete`
* `MapGet`
* `MapMiddlewareDelete`
* `MapMiddlewareGet`
* `MapMiddlewarePost`
* `MapMiddlewarePut`
* `MapMiddlewareRoute`
* `MapMiddlewareVerb`
* `MapPost`
* `MapPut`
* `MapRoute`
* `MapVerb`

::: moniker range="< aspnetcore-2.2"

其中一些列出的方法 (例如 `MapGet`) 需要 `RequestDelegate`。 路由相符時，`RequestDelegate` 會作為「路由處理常式」使用。 此系列中的其他方法允許設定中介軟體管線，以作為路由處理常式使用。 如果 `Map*` 方法不接受處理常式 (例如 `MapRoute`)，則會使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>。

::: moniker-end

`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。 如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。

## <a name="route-template-reference"></a>路由範本參考

大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」。 您可以在路由區段中定義多個路由參數，但其必須以常值分隔。 例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。 這些路由參數必須有一個名稱，並且可以指定其他屬性。

路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。 文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。 若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。

URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。 以範本 `files/{filename}.{ext?}` 為例。 當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。 如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。 下列 URL 符合此路由：

* `/files/myFile.txt`
* `/files/myFile`

::: moniker range=">= aspnetcore-2.2"

您可以使用一個星號 (`*`) 或雙星號 (`**`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。 這稱為 *catch-all* 參數。 例如，`blog/{**slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。 全部擷取參數也可以符合空字串。

當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。 例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。 請注意逸出的斜線。 若要反覆存取路徑分隔符號字元，請使用 `**` 路由參數前置詞。 具有 `{ path = "my/path" }` 的路由 `foo/{**path}` 會產生 `foo/my/path`。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

您可以使用星號 (`*`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。 這稱為「全部擷取」參數。 例如，`blog/{*slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。 全部擷取參數也可以符合空字串。

當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。 例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。 請注意逸出的斜線。

::: moniker-end

路由參數可能有「預設值」，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。 例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。 如果 URL 中沒有用於參數的任何值，則會使用預設值。 路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。 選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。

路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。 在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」。 如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。 指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。

條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。 例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。 如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。

::: moniker range=">= aspnetcore-2.2"

路由參數也可以具有參數轉換器，以在產生連結及根據 URL 比對動作和頁面時，轉換參數的值。 與條件約束類似，可以透過在路徑參數名稱後面新增冒號 (`:`) 和轉換器名稱，將參數轉換器內嵌新增至路徑參數。 例如，路由範本 `blog/{article:slugify}` 會指定 `slugify` 轉換器。 如需參數轉換器的詳細資訊，請參閱[參數轉器參考](#parameter-transformer-reference)一節。

::: moniker-end

下表示範範例路由範本及其行為。

| 路由範本                           | 範例比對 URI    | 要求 URI&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | 只比對單一路徑 `/hello`。                                     |
| `{Page=Home}`                            | `/`                     | 比對並將 `Page` 設定為 `Home`。                                         |
| `{Page=Home}`                            | `/Contact`              | 比對並將 `Page` 設定為 `Contact`。                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | 對應至 `Products` 控制器和 `List` 動作。                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | 對應至 `Products` 控制器和 `Details` 動作 (`id` 設定為 123)。 |
| `{controller=Home}/{action=Index}/{id?`} | `/`                     | 對應至 `Home` 控制器和 `Index` 方法 (會忽略 `id`)。        |

使用範本通常是最簡單的路由方式。 條件約束和預設值也可以在路由範本外部指定。

> [!TIP]
> 啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 `Route`) 如何符合要求。

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
> 請勿針對**輸入驗證**使用條件約束。 如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」回應，而不是「400 - 錯誤要求」與適當的錯誤訊息。 路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。

下表示範範例路由條件約束及其預期行為。

| 條件約束 | 範例 | 範例相符項目 | 注意 |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`、 `-123456789` | 符合任何整數 |
| `bool` | `{active:bool}` | `true`、 `FALSE` | 符合 `true` 或 `false` (不區分大小寫) |
| `datetime` | `{dob:datetime}` | `2016-12-31`、 `2016-12-31 7:32pm` | 符合有效的 `DateTime` 值 (在不因國別而異的文化特性中 - 請參閱警告) |
| `decimal` | `{price:decimal}` | `49.99`、 `-1,000.01` | 符合有效的 `decimal` 值 (在不因國別而異的文化特性中 - 請參閱警告) |
| `double` | `{weight:double}` | `1.234`、 `-1,001.01e8` | 符合有效的 `double` 值 (在不因國別而異的文化特性中 - 請參閱警告) |
| `float` | `{weight:float}` | `1.234`、 `-1,001.01e8` | 符合有效的 `float` 值 (在不因國別而異的文化特性中 - 請參閱警告) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`、 `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | 符合有效的 `Guid` 值 |
| `long` | `{ticks:long}` | `123456789`、 `-123456789` | 符合有效的 `long` 值 |
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

| 運算式   | String    | 比對 | 註解               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | hello     | 是   | 子字串相符項目     |
| `[a-z]{2}`   | 123abc456 | 是   | 子字串相符項目     |
| `[a-z]{2}`   | mz        | 是   | 符合運算式    |
| `[a-z]{2}`   | MZ        | 是   | 不區分大小寫    |
| `^[a-z]{2}$` | hello     | 否    | 請參閱上述的 `^` 和 `$` |
| `^[a-z]{2}$` | 123abc456 | 否    | 請參閱上述的 `^` 和 `$` |

如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。

若要將參數限制為一組已知的可能值，請使用規則運算式。 例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。 如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。 已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。

::: moniker range=">= aspnetcore-2.2"

## <a name="parameter-transformer-reference"></a>參數轉換器參考

參數轉換程式：

* 會在為 `Route` 產生連結時執行。
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

使用上述的路由，動作 `SubscriptionManagementController.GetAll()` 會與URI `/subscription-management/get-all` 比對。 參數轉換器不會變更用來產生連結的路由值。 例如，`Url.Action("GetAll", "SubscriptionManagement")` 會輸出 `/subscription-management/get-all`。

ASP.NET Core 針對搭配產生的路由使用參數轉換程式提供了 API 慣例：

* ASP.NET Core MVC 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API 慣例。 此慣例會將所指定參數轉換程式套用到應用程式中的所有屬性路由。 參數轉換程式會在被取代時轉換屬性路由語彙基元。 如需詳細資訊，請參閱[使用參數轉換程式自訂語彙基元取代](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement)。
* Razor Pages 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API 慣例。 此慣例會將所指定參數轉換器套用至所有自動探索到的 Razor Pages。 參數轉換器會轉換 Razor Pages 路由的資料夾與檔案名稱區段。 如需詳細資訊，請參閱[使用參數轉換程式自訂頁面路由](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes)。

::: moniker-end

## <a name="url-generation-reference"></a>URL 產生參考

下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

在上述範例的結尾產生的 `VirtualPath` 是 `/package/create/123`。 字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。 如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。

`VirtualPathContext` 建構函式的第二個參數是「環境值」的集合。 環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。 目前要求的目前路由值被視為用於連結產生的環境值。 在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。

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
