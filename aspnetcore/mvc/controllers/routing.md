---
title: ASP.NET Core 中的路由至控制器動作
author: rick-anderson
description: 了解 ASP.NET Core MVC 如何使用路由中介軟體來比對內送要求的 URL，並將這些 URL 對應至動作。
ms.author: riande
ms.date: 3/25/2020
uid: mvc/controllers/routing
ms.openlocfilehash: c63313ec060c5be368fcbd20edf5f0d557046d2e
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977209"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a>ASP.NET Core 中的路由至控制器動作

由[里安·諾瓦克](https://github.com/rynowak),[柯克·拉金](https://twitter.com/serpent5)和[里克·安德森](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

ASP.NET核心控制器使用路由[中間件](xref:fundamentals/middleware/index)來符合傳入請求的網址 並將其對應到[操作](#action)。  路由範本:

* 在啟動代碼或屬性中定義。
* 描述 URL 路徑如何與[操作](#action)匹配。
* 用於生成連結的 URL。 生成的連結通常在回應中返回。

操作是[一般路由](#cr)的,要麼是[屬性路由的](#ar)。 在控制器或[操作](#action)上放置路由可使其屬性路由。 如需詳細資訊，請參閱[混合路由](#routing-mixed-ref-label)。

本文件:

* 解釋 MVC 和路由之間的互動:
  * 典型的MVC應用如何使用路由功能。
  * 涵蓋兩者:
    * [常規路由](#cr)通常與控制器和視圖一起使用。
    * 與 REST API 一起使用*的屬性路由*。 如果您主要對 REST API 的路由感興趣,請跳轉到[REST API 部份的屬性路由](#ar)。
  * 有關進階路由詳細資訊,請參閱[路由](xref:fundamentals/routing)。
* 指在ASP.NET Core 3.0 中添加的預設路由系統,稱為終結點路由。 出於相容性目的,可以使用控制器與早期版本的路由。 有關說明,請參閱[2.2-3.0 遷移指南](xref:migration/22-to-30)。 有關舊路由系統的參考資料,請參閱[本文件的 2.2 版本](xref:mvc/controllers/routing?view=aspnetcore-2.2)。

<a name="cr"></a>

## <a name="set-up-conventional-route"></a>設定一般路線

`Startup.Configure`使用[一般路由](#crd)時,通常具有類似於以下內容的代碼:

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet)]

調用<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>中<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>用於創建單個路由。 單個路由名為`default`路由。 大多數具有控制器和視圖的應用使用與`default`路由類似的路由範本。 REST API 應使用[屬性路由](#ar)。

路由樣本`"{controller=Home}/{action=Index}/{id?}"`:

* 符合網址路徑,如`/Products/Details/5`
* 通過標記路徑來提取`{ controller = Products, action = Details, id = 5 }`路由值。 如果應用程式具有名為`ProductsController`控制器`Details`和 操作的控制器,則路由值的提取將生成匹配:

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippetA)]

  [!INCLUDE[](~/includes/MyDisplayRouteInfo.md)]

* `/Products/Details/5`模型將 的`id = 5`值 繫結`id`為 將`5`參數設定為 。 有關詳細資訊,請參閱[模型繫結](xref:mvc/models/model-binding)。
* `{controller=Home}`定義為`Home`預設`controller`值 。
* `{action=Index}`定義為`Index`預設`action`值 。
*  中的`?``{id?}`字`id`元 定義為可選。
  * 預設和選擇性路由參數不一定要全部出現在 URL 路徑中才算相符。 如需路由範本語法的詳細描述，請參閱[路由範本參考](xref:fundamentals/routing#route-template-reference)。
* 符合網`/`址路徑 。
* 產生路由值`{ controller = Home, action = Index }`。

的值`controller``action`和 預設值的使用。 `id`不會生成值,因為 URL 路徑中沒有相應的段。 `/`只當存在與`HomeController``Index`操作時符合:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

使用前面控制器定義與路由樣本,`HomeController.Index`操作會針對以下網址路徑執行:

* `/Home/Index/17`
* `/Home/Index`
* `/Home`
* `/`

URL`/`路徑 使用路由範`Home`本預設`Index`控制器和 操作。 URL`/Home`路徑 使用路由範`Index`本預設 操作。

<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*> 方法很方便：

```csharp
endpoints.MapDefaultControllerRoute();
```

取代:

```csharp
endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

路由使用<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*>和<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>中間件進行配置。 要使用控制器:

* 調用<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*>`UseEndpoints`內部 以映射[屬性路由](#ar)控制器。
* 呼叫<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>或,以映射[一般路由](#cr)的控制器。

<a name="routing-conventional-ref-label"></a>
<a name="crd"></a>

## <a name="conventional-routing"></a>慣例路由

常規路由與控制器和視圖一起使用。 `default` 路由：

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet2)]

即為「慣例路由」** 的範例。 它被稱為*傳統路由*,因為它為 URL 路徑建立了*約定*:

* 第一個路徑段`{controller=Home}`映射到控制器名稱。
* 第二個段`{action=Index}`, 映射到[操作](#action)名稱。
* 第三個段`{id?}`可以選擇`id`。 in`?``{id?}`使其可選。 `id`用於映射到模型實體。

使用此`default`路由,URL 路徑:

* `/Products/List`映射到`ProductsController.List`操作。
* `/Blog/Article/17`映射到`BlogController.Article`和通常模型`id`將 參數綁定到 17。

此對應:

* **僅**基於控制器和[操作](#action)名稱。
* 不基於命名空間、源檔位置或方法參數。

使用常規路由與預設路由允許創建應用,而無需為每個操作提出新的 URL 模式。 對於具有[CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)樣式操作的應用,具有跨控制器的網址的一致性:

* 有助於簡化代碼。
* 使 UI 更加可預測。

> [!WARNING]
> 前面的`id`代碼中由工藝路線範本定義為可選。 操作可以在沒有作為 URL 的一部分提供的可選 ID 的情況下執行。 通常,從`id`網址中省略時:
>
> * `id`由模型綁定`0`設置為。
> * 在資料庫匹配`id == 0`中找不到任何實體。
>
> [屬性路由](#ar)提供細粒度控制項,使某些操作不需要 ID,而不是其他操作的 ID。 按照慣例,文檔包括可選參數,`id`例如它們可能以正確使用的方式出現。

大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。 預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：

* 支援基本的描述性路由配置。
* 適合作為 UI 型應用程式的起點。
* 是許多 Web UI 應用所需的唯一路由範本。 對於較大的 Web UI 應用,使用[「區域](#areas)」的另一個路由(如果經常是所有所需的內容)。

<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>與<xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*>:

* 根據調用其終結點的順序自動為其終結點分配**訂單**值。

ASP.NET Core 3.0 及更高版本中的終結點路由:

* 沒有路線的概念。
* 不為執行擴充性提供命令保證,所有終結點都一次性處理。

啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。

[屬性路由](#ar)將在本文中稍後介紹。

<a name="mr"></a>

### <a name="multiple-conventional-routes"></a>多條常規路線

通過向<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>和添加更多調用`UseEndpoints`,可以添加多個[常規路由。](#cr) <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> 這樣做允許定義多個約定,或添加專用於特定[操作](#action)的傳統路由,例如:

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<a name="dcr"></a>

前面`blog`代碼中的路由是**專用的一般路由**。 它被稱為專用常規路由,因為:

* 它使用[傳統的路由](#cr)。
* 它致力於一個具體[的行動](#action)。

因為`controller``action`並且不以參數顯示在工藝`"blog/{*article}"`路線 樣本中:

* 它們只能具有預設值`{ controller = "Blog", action = "Article" }`。
* 這條路線總是映射到操作`BlogController.Article`。

`/Blog``/Blog/Article`,並且`/Blog/{any-string}`是唯一與博客路由匹配的 URL 路徑。

前面的範例:

* `blog`路由的匹配優先順序高於路由,`default`因為它首先添加。
* 是[Slug](https://developer.mozilla.org/docs/Glossary/Slug)樣式路由的範例,其中通常將文章名稱作為網址的一部分。

> [!WARNING]
> 在ASP.NET核心 3.0 及更高版本中,路由不會:
> * 定義一個稱為*路由*的概念。 `UseRouting`將路由匹配添加到中間件管道。 中間`UseRouting`件查看應用中定義的終結點集,並根據請求選擇最佳終結點匹配。
> * 提供有關擴充性執行順序的保證,例如<xref:Microsoft.AspNetCore.Routing.IRouteConstraint>或<xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>。
>
>有關路由的參考資料,請參閱[工藝路線](xref:fundamentals/routing)。

<a name="cro"></a>

### <a name="conventional-routing-order"></a>一般路由順序

常規路由僅與應用定義的操作和控制器的組合匹配。 這旨在簡化傳統路由重疊的情況。
使用<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>添加路由,並<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>根據調用其終結點的順序自動為其終結點分配訂單值。 前面顯示的路由的匹配具有更高的優先順序。 慣例路由與順序息息相關。 通常,具有區域的路由應更早放置,因為它們比沒有區域的路由更具體。 具有 Catch 所有路由`{*article}`參數的[專用常規路由](#dcr)會使路由過於[貪婪](xref:fundamentals/routing#greedy),這意味著它匹配了您打算與其他路由匹配的 URL。 稍後將貪婪路由放在路由表中,以防止貪婪匹配。

<a name="best"></a>

### <a name="resolving-ambiguous-actions"></a>解決模稜兩可的操作

當兩個終結點通過路由匹配時,路由必須執行以下操作之一:

* 選擇最佳候選人。
* 擲回例外狀況。

例如：

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet9)]

前面控制器定義了兩個符合的操作:

* 網址路徑`/Products33/Edit/17`
* 路由資料`{ controller = Products33, action = Edit, id = 17 }`。

這是 MVC 控制器的典型模式:

* `Edit(int)`顯示用於編輯產品的窗體。
* `Edit(int, Product)`處理過帳的表單。

要解析正確的路由:

* `Edit(int, Product)`當要求為`POST`HTTP 時,將選擇 。
* `Edit(int)`當 HTTP[謂詞](#verb)是任何其他內容時,將選中。 `Edit(int)`通常透過`GET`呼叫 。

提供<xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>`[HttpPost]`到路由,以便它可以根據請求的 HTTP 方法進行選擇。 比比符合`Edit(int)` `HttpPostAttribute` `Edit(int, Product)`

瞭解屬性(如`HttpPostAttribute`)的作用非常重要。 其他[HTTP 謂詞](#verb)定義了類似的屬性。 [在常規路由](#cr)中,當操作是顯示表單的一部分時,通常使用相同的操作名稱,提交表單工作流。 例如,請參閱[檢查兩種編輯操作方法](xref:tutorials/first-mvc-app/controller-methods-views#get-post)。

如果路由無法選擇最佳候選項,則引發<xref:System.Reflection.AmbiguousMatchException>,列出多個匹配的終結點。

<a name="routing-route-name-ref-label"></a>

### <a name="conventional-route-names"></a>一般路由名稱

字串`"blog"`與`"default"`以下範例中是一般路由名稱:

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

路由名稱為路由指定一個邏輯名稱。 命名路由可用於 URL 生成。 當路由的順序會使 URL 生成變得複雜時,使用命名路由可簡化 URL 創建。 路由名稱必須是唯一的應用程式寬。

路由名稱:

* 對 URL 匹配或請求處理沒有影響。
* 僅用於 URL 生成。

路由名稱概念在路由中表示為[IEndpointName 中繼資料](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata)。 字語**路由名稱**與**終結點名稱**:

* 可互換。
* 在文件和代碼中使用的哪個取決於描述的 API。

<a name="attribute-routing-ref-label"></a>
<a name="ar"></a>

## <a name="attribute-routing-for-rest-apis"></a>REST API 屬性路由

REST API 應使用屬性路由將應用的功能建模為一組資源,其中操作由[HTTP 謂詞](#verb)表示。

屬性路由使用一組屬性，將動作直接對應至路由範本。 以下`StartUp.Configure`代碼是 REST API 的典型代碼,在下一個範例中使用:

[!code-csharp[](routing/samples/3.x/main/StartupApi.cs?name=snippet)]

在前面的代碼中,<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*>在`UseEndpoints`內部 調用映射屬性路由控制器。

在下例中︰

* 使用上述`Configure`方法。
* `HomeController`匹配一組類似於預設常規路由`{controller=Home}/{action=Index}/{id?}`匹配的 URL。

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

操作`HomeController.Index`為任何`/``/Home`URL`/Home/Index``/Home/Index/3`路徑 、或運行。

此示例突出顯示屬性路由[和傳統路由](#cr)之間的關鍵程式設計差異。 屬性路由需要更多的輸入來指定路由。 傳統的預設路由處理路由更簡潔。 但是,屬性路由允許並且需要精確控制應用於每個[操作](#action)的路由範本。

使用屬性路由時,控制器名稱和操作名稱在匹配操作時**不扮演任何**角色。 以下範例與上一範例符合相同的網址:

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

以下代碼使用和`action``controller`的權杖替換。

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet22)]

以下代碼適用於`[Route("[controller]/[action]")]`控制器:

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

在前面的代碼中,`Index`方法樣本必須準備`/``~/`或 到工藝路線範本。 套用至開頭為 `/` 或 `~/` 之動作的路由範本，無法與套用至控制器的路由範本合併。

有關工藝路線範本選擇的資訊,請參閱[工藝路線範本優先順序](xref:fundamentals/routing#rtp)。

## <a name="reserved-routing-names"></a>保留的路由名稱

使用控制器或剃刀頁時,以下關鍵字為保留路由參數名稱:

* `action`
* `area`
* `controller`
* `handler`
* `page`

使用`page`作為具有屬性路由的路由參數是一個常見的錯誤。 這樣做會導致與 URL 生成不一致且令人困惑的行為。

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo2Controller.cs?name=snippet)]

URL 生成使用特殊參數名稱來確定網址生成操作是引用 Razor 頁還是控制器。

<a name="verb"></a>

## <a name="http-verb-templates"></a>HTTP 動詞樣本

ASP.NET核心具有以下 HTTP 謂詞範本:

* [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)
* [[ HTTPPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)
* [[HttpPut]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)
* [[ HttpDelete]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)
* [[HTTPHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)
* [[HTTPPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)

<a name="rt"></a>

### <a name="route-templates"></a>路由範本

ASP.NET核心具有以下路由範本:

* 所有[HTTP 謂詞範本](#verb)都是路由範本。
* [[路線]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)

<a name="arx"></a>

### <a name="attribute-routing-with-http-verb-attributes"></a>具有 HTTP 謂詞屬性的屬性路由

請考慮以下控制器:

[!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet)]

在上述程式碼中：

* 每個操作都包含該`[HttpGet]`屬性,該屬性僅約束與 HTTP GET 請求的匹配。
* 該`GetProduct`操作`"{id}"`包括 範本`id`,因此將追加到控制`"api/[controller]"`器上的 範本。 方法樣本為`"api/[controller]/"{id}""`。 因此,此操作僅匹配窗體`/api/test2/xyz`的`/api/test2/123`GET`/api/test2/{any string}`請求, 、 等。
  [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet2)]
* 該`GetIntProduct`操作`"int/{id:int}")`包含範本。 `:int`樣本部分將`id`路由值約束到可以轉換為整數的字串。 到`/api/test2/int/abc`的 GET 要求:
  * 與此操作不匹配。
  * 返回[404 未找到](https://developer.mozilla.org/docs/Web/HTTP/Status/404)錯誤。
    [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet3)]
* 該`GetInt2Product`操作包含`{id}`在 範本中,但`id`不限制 為可轉換為整數的值。 到`/api/test2/int2/abc`的 GET 要求:
  * 匹配此路由。
  * 模型綁定無法轉換為`abc`整數。 該方法`id`的參數為整數。
  * 傳[回 400 錯誤請求](https://developer.mozilla.org/docs/Web/HTTP/Status/400),因為`abc`模型綁定無法 轉換為整數。
      [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet4)]

屬性路由可以使用<xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute>屬性,<xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute><xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>如<xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>。 與 。 所有[HTTP 謂詞](#verb)屬性都接受路由範本。 下面的範例顯示了與同一路由樣本符合的兩個操作:

[!code-csharp[](routing/samples/3.x/main/Controllers/MyProductsController.cs?name=snippet1)]

使用網`/products3`址路徑 :

* 當`MyProductsController.ListProducts` [HTTP 謂詞](#verb)為`GET`時,操作將運行。
* 當`MyProductsController.CreateProduct` [HTTP 謂詞](#verb)為`POST`時,操作將運行。

構建 REST API 時,很少需要在操作`[Route(...)]`方法上使用 ,因為操作接受所有 HTTP 方法。 最好使用更具體的[HTTP 動詞屬性](#verb)來精確瞭解 API 支援的內容。 REST API 的用戶端必須知道哪些路徑和 HTTP 動詞命令對應至特定邏輯作業。

REST API 應使用屬性路由將應用的功能建模為一組資源,其中操作由 HTTP 謂詞表示。 這意味著,同一邏輯資源上的許多操作(例如,GET 和 POST)使用相同的 URL。 屬性路由提供仔細設計 API 公用端點配置所需的控制層級。

由於屬性路由會套用至特定動作，因此輕鬆就能將參數設為路由範本定義的必要部分。 在以下範例中,`id`作為網址路徑的一部分是必要的:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

操作`Products2ApiController.GetProduct(int)`:

* 使用 URL 路徑執行,如`/products2/3`
* 不使用 URL`/products2`路徑執行。

[[消耗]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)屬性允許操作限制受支援的請求內容類型。 有關詳細資訊,請參閱使用[使用 屬性定義受支援的要求內容類型](xref:web-api/index#consumes)。

 如需路由範本和相關選項的完整描述，請參閱[路由](xref:fundamentals/routing)。

有關 的詳細`[ApiController]`資訊 ,請參閱[ApiController 屬性](xref:web-api/index##apicontroller-attribute)。

## <a name="route-name"></a>路由名稱

下列程式碼會定義  的「路由名稱」`Products_List`：

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

您可以使用路由名稱根據特定路由來產生 URL。 路由名稱:

* 對路由的 URL 匹配行為沒有影響。
* 僅用於 URL 生成。

在整個應用程式內路由名稱必須是唯一的。

將前面的代碼與傳統預設路由進行對比,後者將`id`參數定義為可選`{id?}`( 。 精確指定 API 的能力具有優勢,`/products`例如`/products/5`允許 並發送到不同的操作。

<a name="routing-combining-ref-label"></a>

## <a name="combining-attribute-routes"></a>組合屬性路由

為了避免屬性路由過於重複，控制器上的路由屬性可與個別動作上的路由屬性合併。 控制器上定義的任何路由範本都會附加到動作上的路由範本之前。 將路由屬性放在控制器上，即可讓控制器中的**所有**動作使用屬性路由。

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet)]

在上述範例中：

* 網址`/products`路徑可以符合`ProductsApi.ListProducts`
* 網址`/products/5`路徑`ProductsApi.GetProduct(int)`可以符合 。

這兩個操作都僅與`GET`HTTP 匹配,因為`[HttpGet]`它們被標記為屬性。

套用至開頭為 `/` 或 `~/` 之動作的路由範本，無法與套用至控制器的路由範本合併。 下面的範例匹配一組類似於預設路由的 URL 路徑。

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet)]

下表解釋了前面代碼中`[Route]`的屬性:

| 屬性               | 與`[Route("Home")]` | 定義式路線範本 |
| ----------------- | ------------ | --------- |
| `[Route("")]` | 是 | `"Home"` |
| `[Route("Index")]` | 是 | `"Home/Index"` |
| `[Route("/")]` | **否** | `""` |
| `[Route("About")]` | 是 | `"Home/About"` |

<a name="routing-ordering-ref-label"></a>
<a name="oar"></a>

### <a name="attribute-route-order"></a>屬性路由順序

路由產生樹並同時符合所有終結點:

* 路由條目的行像放置在理想的順序中一樣。
* 最具體的路由有機會在更常規的路由之前執行。

例如,這樣的`blog/search/{topic}`屬性路由比 屬性路由`blog/{*article}`(如 )更具體。 默認情況下`blog/search/{topic}`,路由具有更高的優先順序,因為它更具體。 使用[常規路由](#cr),開發人員負責按所需順序放置路由。

屬性路由可以使用<xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order>屬性 配置訂單。 提供的所有框架[路由屬性](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)都`Order`包括 。 路由會依 `Order` 屬性的遞增排序來處理。 預設順序為 `0`。 使用`Order = -1`未設置訂單的路由之前使用運行設置路由。 在預設路由排序`Order = 1`后使用運行設置路由。

**避免**`Order`。 如果應用的 URL 空間需要顯式順序值才能正確路由,則用戶端也可能感到困惑。 通常,屬性路由選擇具有 URL 匹配的正確路由。 如果用於 URL 生成的替代預設順序不起作用,則使用路由名稱作為重寫通常比應用`Order`屬性 更簡單。

請考慮以下兩個控制器,它們都定義了路由符合`/home`:

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

`/home`使用上述代碼要求將引發類似於以下內容的異常:

```text
AmbiguousMatchException: The request matched multiple endpoints. Matches:

 WebMvcRouting.Controllers.HomeController.Index
 WebMvcRouting.Controllers.MyDemoController.MyIndex
```

新增到`Order`其中一個路由屬性可解決歧義:

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo3Controller.cs?name=snippet3& highlight=2)]

使用前面的代碼,`/home``HomeController.Index`運行 終結點。 要存取`MyDemoController.MyIndex``/home/MyIndex`要求 。 **注意**：

* 前面的代碼是一個範例或糟糕的路由設計。 它被用來說明屬性`Order`。
* 該`Order`屬性僅解決歧義,該範本無法匹配。 最好刪除`[Route("Home")]`範本。

請參閱[Razor 頁面路由和應用約定:使用](xref:razor-pages/razor-pages-conventions#route-order)Razor 頁面獲取有關工藝路線順序的資訊的路由順序。

在某些情況下,HTTP 500 錯誤返回與不明確的路由。 使用[日誌記錄](xref:fundamentals/logging/index)查看導致`AmbiguousMatchException`的終結點。

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>路由範本中的權杖替換 [控制器]、[操作]、[區域]

為方便起見,屬性路由通過將令牌包裝在以下其中一個來支援保留路由參數的權杖替換:

* 方形大括號:`[]`
* 大括弧:`{}`

標記`[action]`,`[area]``[controller]`與取代為操作名稱、區域名稱與控制器名稱的值,這些值來自定義路由的操作:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet)]

在上述程式碼中：

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet10)]

  * 比賽`/Products0/List`

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet11)]

  * 比賽`/Products0/Edit/{id}`

語彙基元取代會在建立屬性路由的最後一個步驟發生。 前面的範例的行為與以下代碼相同:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet20)]

[!INCLUDE[](~/includes/MTcomments.md)]

屬性路由也可以透過繼承來合併。 這是強大的結合權杖更換。 語彙基元取代也適用於屬性路由所定義的路由名稱。
`[Route("[controller]/[action]", Name="[controller]_[action]")]`為每個操作產生唯一的路由名稱:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet5)]

語彙基元取代也適用於屬性路由所定義的路由名稱。
`[Route("[controller]/[action]", Name="[controller]_[action]")]`
 會針對每個動作產生唯一的路由名稱。

若要比對常值語彙基元取代分隔符號 `[` 或 `]`，請重複字元 (`[[` 或 `]]`) 來將它逸出。

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a>使用參數轉換程式自訂語彙基元取代

可以使用參數轉換程式自訂語彙基元取代。 參數轉換程式會實作 <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> 並轉換參數值。 例如,自訂`SlugifyParameterTransformer`參數轉換器`SubscriptionManagement`將 路由值`subscription-management`更改為 :

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet2)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention> 是一個應用程式模型慣例，它會：

* 將參數轉換程式套用到應用程式中的所有屬性路由。
* 在取代屬性路由語彙基元值時自訂它們。

[!code-csharp[](routing/samples/3.x/main/Controllers/SubscriptionManagementController.cs?name=snippet)]

前面的`ListAll`方法`/subscription-management/list-all`符合 。

`RouteTokenTransformerConvention` 會在 `ConfigureServices` 中註冊為選項。

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet)]

有關 Slug 的定義,請參閱[Slug 上的 MDN Web 文件](https://developer.mozilla.org/docs/Glossary/Slug)。

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-attribute-routes"></a>多屬性路由

屬性路由支援定義多個路由來達到相同的動作。 最常見的用法是模擬「預設慣例路由」的行為，如下列範例所示：

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6x)]

在控制器上放置多個路由屬性意味著每個路由屬性都與操作方法上的每個路由屬性結合在一起:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6)]

所有[HTTP 謂詞](#verb)路由`IActionConstraint`約束實現 。

將實現<xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>實現的多個路由屬性放置在操作上時:

* 每個操作約束與應用於控制器的路由範本結合使用。

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet7)]

在操作上使用多個路由可能看起來有用且功能強大,最好保持應用的 URL 空間基本且定義良好。 僅在需要時**對**操作使用多個路由,例如,支援現有用戶端。

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>指定屬性路由的選擇性參數、預設值和條件約束

屬性路由支援使用與慣例路由相同的內嵌語法，來指定選擇性參數、預設值和條件約束。

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet8&highlight=3)]

在前面的代碼中,`[HttpPost("product/{id:int}")]`應用路由約束。 操作`ProductsController.ShowProduct`僅由 URL`/product/3`路徑(如 )匹配。 工藝路線範本部分`{id:int}`僅將該段限制為整數。

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

如需路由範本語法的詳細描述，請參閱[路由範本參考](xref:fundamentals/routing#route-template-reference)。

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>使用 IRouteTemplate 提供者的自訂路由屬性

所有[路由屬性](#rt)。<xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider> ASP.NET核心執行時:

* 在應用啟動時查找控制器類和操作方法的屬性。
* 使用實現`IRouteTemplateProvider`的屬性生成初始路由集。

實現`IRouteTemplateProvider`定義自定義路由屬性。 每個 `IRouteTemplateProvider` 都可讓您定義具有自訂路由範本、順序和名稱的單一路由：

[!code-csharp[](routing/samples/3.x/main/Controllers/MyTestApiController.cs?name=snippet&highlight=1-10)]

前面的`Get`方法`Order = 2, Template = api/MyTestApi`傳回 。

<a name="routing-app-model-ref-label"></a>

### <a name="use-application-model-to-customize-attribute-routes"></a>使用應用程式模型自訂屬性路由

應用程式模型:

* 是在啟動時創建的物件模型。
* 包含ASP.NET核心用於路由和執行應用中操作的所有元數據。

應用程式模型包括從路由屬性收集的所有數據。 路由屬性中的數據由`IRouteTemplateProvider`實現提供。 約定:

* 可以編寫以修改應用程式模型以自定義路由的行為。
* 在應用啟動時讀取。

本節顯示了使用應用程式模型自定義路由的基本示例。 以下代碼使路由大致與專案的文件結構一致。

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet)]

以下代碼可防止將`namespace`約定應用於屬性路由的控制器:

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet2)]

例如,以下控制器不使用`NamespaceRoutingConvention`:

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/ManagersController.cs?name=snippet&highlight=1)]

`NamespaceRoutingConvention.Apply` 方法：

* 如果控制器是屬性路由的,則不執行任何操作。
* 根據設定控制器樣本,`namespace`刪除底座。 `namespace`

`NamespaceRoutingConvention`可套用`Startup.ConfigureServices`於 :

[!code-csharp[](routing/samples/3.x/nsrc/Startup.cs?name=snippet&highlight=1,14-18)]

例如,請考慮以下控制器:

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/UsersController.cs)]

在上述程式碼中：

* 基地`namespace``My.Application`為 。
* 前面的控制器的全名是`My.Application.Admin.Controllers.UsersController`。
* 將`NamespaceRoutingConvention`控制器樣本設定到`Admin/Controllers/Users/[action]/{id?`。

`NamespaceRoutingConvention`也可以作為屬性應用於控制器:

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/TestController.cs?name=snippet&highlight=1)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>混合路由：屬性路由與慣例路由

ASP.NET核心應用可以混合使用傳統的路由和屬性路由。 控制器通常會使用慣例路由來提供 HTML 頁面給瀏覽器，並使用屬性路由來提供 REST API。

動作可以使用慣例路由或屬性路由。 將路由放在控制器或動作上，即可讓它使用屬性路由。 定義屬性路由的動作無法透過慣例路由到達，反之亦然。 控制器上**的任何**路由**屬性使控制器**屬性中的所有操作都路由。

屬性路由和傳統路由使用相同的路由引擎。

<a name="routing-url-gen-ref-label"></a>
<a name="ambient"></a>

## <a name="url-generation-and-ambient-values"></a>網址產生與環境值

應用可以使用路由 URL 生成功能生成指向操作的 URL 連結。 生成 URL 消除了硬編碼 URL,使代碼更加可靠且可維護。 本節重點介紹 MVC 提供的 URL 生成功能,僅介紹 URL 生成工作原理的基礎知識。 如需 URL 產生的詳細描述，請參閱[路由](xref:fundamentals/routing)。

該<xref:Microsoft.AspNetCore.Mvc.IUrlHelper>介面是 MVC 和 URL 生成路由之間的基礎結構的基礎元素。 中的`IUrlHelper`實例可通過控制器、檢視和`Url`檢視元件中的屬性提供。

在下面的範例中,`IUrlHelper`介面`Controller.Url`透過屬性生成另一個操作的網址。

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

如果使用預設的一般路由,`url`則變數的值是網址 路徑字`/UrlGeneration/Destination`串 。 此網址路徑是透過合併路由建立的:

* 目前請求的路由值,稱為**環境值**。
* 傳遞給`Url.Action`這些值並將這些值取代到製程的範本中的值:

``` text
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

路由範本中每個路由參數的值都會以相符名稱的值和環境值所取代。 沒有值的路由參數可以:

* 如果預設值有預設值,請使用預設值。
* 如果可選,則跳過。 例如,`id`從路由樣本`{controller}/{action}/{id?}`。

如果任何必需的路由參數沒有相應的值,則 URL 生成將失敗。 如果某個路由的 URL 產生失敗，則會嘗試下一個路由，直到嘗試所有路由或找到相符項目為止。

前面的假設`Url.Action`[路由](#cr)示例。 URL 生成與[屬性路由](#ar)類似,儘管概念不同。 使用傳統路由:

* 工藝路線值用於展開範本。
* 和`action`通常`controller`出現在 該範本中的路由值。 這之所以有效,是因為路由匹配的 URL 符合約定。

以下範例使用屬性路由:

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationAttrController.cs?name=snippet_1)]

前面的`Source`程式碼中的操作會`custom/url/to/destination`產生 。

<xref:Microsoft.AspNetCore.Routing.LinkGenerator>在 ASP.NET 酷 3.0 中添加`IUrlHelper`,作為的替代方法。 `LinkGenerator`提供類似但更靈活的功能。 上`IUrlHelper`的每種方法都有相應的方法`LinkGenerator`系列。

### <a name="generating-urls-by-action-name"></a>由動作名稱產生 URL

[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) [Url.action,LinkGenerator.GetPathByAction,](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*)以及所有相關重載都旨在通過指定控制器名稱和操作名稱來生成目標終結點。

使用`Url.Action`時,目前的目前路由`controller``action`值 由執行時提供:

* 的值`controller``action`是[環境值和值](#ambient)的一部分。 該方法`Url.Action`始終使用`action``controller`和的當前值,並生成路由到當前操作的 URL 路徑。

路由嘗試使用環境值中的值來填充生成 URL 時未提供的資訊。 考慮環境值`{ a = Alice, b = Bob, c = Carol, d = David }``{a}/{b}/{c}/{d}`這樣的 路由:

* 路由有足夠的資訊來生成 URL,而不需要任何其他值。
* 路由具有足夠的資訊,因為所有路由參數都有一個值。

如果新增該`{ d = Donovan }`值:

* 該值`{ d = David }`將被忽略。
* 產生的網址`Alice/Bob/Carol/Donovan`路徑為 。

**警告**: 網址路徑是分層的。 在前面的範例中, 如果新增該`{ c = Cheryl }`值 :

* 將忽略這兩`{ c = Carol, d = David }`個值。
* 不再有值`d`,URL 產生失敗。
* 必須指定的`c``d`所需值才能生成 URL。  

您可能期望使用預設路由`{controller}/{action}/{id?}`遇到此問題。 此問題在實踐中很少見,因為`Url.Action`始終顯式指定`controller``action`和 值。

[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*)的多個重載採用路由值對象來為 路由`controller``action`參數提供 和 以外的值。 路由值物件經常與 一起`id`使用 。 例如： `Url.Action("Buy", "Products", new { id = 17 })` 。 路由值物件:

* 根據約定,通常是匿名類型的物件。
* 可以是 POCO`IDictionary<>`或[POCO。](https://wikipedia.org/wiki/Plain_old_CLR_object)

不符合路由參數的任何額外路由值都會放在查詢字串中。

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet)]

前面的代碼產生`/Products/Buy/17?color=red`。

以下代碼產生絕對網址:

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet2)]

要建立絕對網址,請使用以下網址之一:

* 接受的`protocol`重載。 例如,前面的代碼。
* [連結生成器.GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*),默認情況下生成絕對URI。

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generate-urls-by-route"></a>依路由產生網址

前面的代碼演示通過傳入控制器和操作名稱來生成 URL。 `IUrlHelper`還提供[Url.RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*)方法系列。 這些方法類似於[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*),但它們不會`action``controller`將和的當前值複製到路由值。 最常見的用法`Url.RouteUrl`:

* 指定要生成 URL 的路由名稱。
* 通常不指定控制器或操作名稱。

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGeneration2Controller.cs?name=snippet_1)]

以下 Razor 檔案產生`Destination_Route`指向的 HTML 連結:

[!code-cshtml[](routing/samples/3.x/main/Views/Shared/MyLink.cshtml)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generate-urls-in-html-and-razor"></a>在 HTML 與 Razor 中建立網址

<xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper>提供了分別<xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper>`<form>`生成`<a>`[的方法 Html.BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*)和[Html.ActionLink。](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) 這些方法使用[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*)方法生成 URL,並且它們接受類似的參數。 `HtmlHelper` 的成對 `Url.RouteUrl` 為 `Html.BeginRouteForm` 和 `Html.RouteLink`，這兩者的功能很類似。

TagHelper 透過 `form` TagHelper 和 `<a>` TagHelper 產生 URL。 這兩者使用 `IUrlHelper` 進行實作。 有關詳細資訊[,請參閱表單中標記幫助人員](xref:mvc/views/working-with-forms)。

在檢視中，可透過 `Url` 屬性使用 `IUrlHelper` 來產生上述未涵蓋的任何特定 URL。

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="url-generation-in-action-results"></a>操作結果中的網址產生

前面的示例顯示了`IUrlHelper`在控制器中使用。 控制器中最常見的用法是生成 URL 作為操作結果的一部分。

<xref:Microsoft.AspNetCore.Mvc.ControllerBase> 和 <xref:Microsoft.AspNetCore.Mvc.Controller> 基底類別提供便利的方法讓動作結果可參考其他動作。 一個典型的用法是在接受使用者輸入后重定向:

[!code-csharp[](routing/samples/3.x/main/Controllers/CustomerController.cs?name=snippet)]

操作結果工廠方法,如<xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>和遵循與`IUrlHelper`上 的方法類似的模式。

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>專用慣例路由的特殊案例

[常規路由](#cr)可以使用一種稱為[專用常規路由](#dcr)的特殊路由定義。 在下面的範例中,命名的`blog`路由是專用的常規路由:

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

使用前面的路由定義,`Url.Action("Index", "Home")``/``default`使用路由產生 URL 路徑,但為什麼? 您可能會猜想路由值 `{ controller = Home, action = Index }` 便足以使用 `blog` 來產生 URL，且結果會是 `/blog?action=Index&controller=Home`。

[專用常規路由](#dcr)依賴於預設值的特殊行為,這些預設值沒有相應的路由參數,以防止路由在 URL 產生時過於[貪婪](xref:fundamentals/routing#greedy)。 在本例中，預設值為 `{ controller = Blog, action = Article }`，`controller` 和 `action` 都不會顯示為路由參數。 當路由執行 URL 產生時，所提供的值必須符合預設值。 使用`blog`網址產生失敗`{ controller = Home, action = Index }`, 因為值`{ controller = Blog, action = Article }`不符合 。 路由會接著切換並嘗試 `default`，此時會成功。

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>區域

[區域](xref:mvc/controllers/areas)是 MVC 功能,用於將相關功能組織到群組中作為單獨的功能:

* 為控制器操作路由命名空間。
* 視圖的資料夾結構。

使用區域允許應用具有多個具有相同名稱的控制器,只要它們具有不同的區域。 使用區域可建立用於路由的階層，方法是將另一個路由參數 `area` 新增至 `controller` 和 `action`。 本節討論路由如何與區域交互。 有關如何使用檢視區域的詳細資訊,請參閱[區域](xref:mvc/controllers/areas)。

下面的範例將 MVC 設定為使用預設常規路`area``area`由與`Blog`命名路由 的路由:

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

在前面的代碼中,<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>呼叫以建立`"blog_route"`。 第二個`"Blog"`參數 , 是區域名稱。

符合網址`/Manage/Users/AddUser`路徑`"blog_route"`( 如 )時`{ area = Blog, controller = Users, action = AddUser }`,路由將生成路由值。 路由`area`值由`area`的 預設值生成。 由 建立`MapAreaControllerRoute`的 路由等效於以下內容:

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup2.cs?name=snippet2)]

`MapAreaControllerRoute` 會針對使用所提供之區域名稱 (在本例中為 `Blog`) 的 `area`，使用預設值和條件約束來建立路由。 預設值可確保路由一律會產生 `{ area = Blog, ... }`，而條件約束需要 `{ area = Blog, ... }` 值以產生 URL。

慣例路由與順序息息相關。 通常,具有區域的路由應更早放置,因為它們比沒有區域的路由更具體。

使用前面的範例,路由值`{ area = Blog, controller = Users, action = AddUser }`比以下操作:

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[[區域]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute)屬性表示控制器是區域的一部分。 此控制器位於該區域`Blog`。 沒有屬性的`[Area]`控制器不是任何區域的成員,並且**與**路由值由路由提供時`area`不匹配。 在下列範例中，只有列出的第一個控制器可能符合路由值 `{ area = Blog, controller = Users, action = AddUser }`。

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

此處顯示每個控制器的命名空間,以便完整性。 如果前面的控制器使用相同的命名空間,則將生成編譯器錯誤。 類別命名空間不會影響 MVC 的路由。

前兩個控制器是區域的成員，只有在 `area` 路由值提供其各自的區域名稱時才會符合。 第三個控制器不是任何區域的成員，只有路由未提供任何值給 `area` 時才會符合。

<a name="aa"></a>

在「未符合任何值」** 的情況下，缺少 `area` 值相當於 `area` 的值為 Null 或空字串。

在區域內執行作業時,for`area`的路由值可作為用於網址產生的路線[的環境值](#ambient)。 這表示區域預設會以「黏性」** 方式來處理 URL 產生，如下列範例所示。

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup3.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

以下代碼產生到的`/Zebra/Users/AddUser`網址:

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/HomeController.cs?name=snippet)]

<a name="action"></a>

## <a name="action-definition"></a>操作定義

控制器上的公共方法(具有[非操作](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute)屬性的這些方法除外)是操作。

## <a name="sample-code"></a>範例程式碼

 * [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs)方法包含在[示例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x)中,用於顯示路由資訊。
* [檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x)([如何下載](xref:index#how-to-download-a-sample))

[!INCLUDE[](~/includes/dbg-route.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core MVC 使用路由[中介軟體](xref:fundamentals/middleware/index)來比對內送要求的 URL，並將這些 URL 對應至動作。 路由是在啟動程式碼或屬性中定義。 路由描述 URL 路徑應該如何與動作進行比對。 路由也可用來產生回應中所送出的連結 URL。

動作可以使用慣例路由或屬性路由。 將路由放在控制器或動作上，即可讓它使用屬性路由。 如需詳細資訊，請參閱[混合路由](#routing-mixed-ref-label)。

本文件將說明 MVC 與路由之間的互動，並說明一般 MVC 應用程式如何使用路由功能。 如需進階路由的詳細資料，請參閱[路由](xref:fundamentals/routing)。

## <a name="setting-up-routing-middleware"></a>設定路由中介軟體

在 *Configure* 方法中，您可能會看到類似如下的程式碼：

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

在 `UseMvc` 呼叫內，`MapRoute` 是用來建立單一路由，稱為 `default` 路由。 大多數 MVC 應用程式會使用具有類似於 `default` 路由之範本的路由。

路由範本 `"{controller=Home}/{action=Index}/{id?}"` 可能符合某個 URL 路徑 (例如 `/Products/Details/5`)，並將透過 Token 化路徑來擷取路由值 `{ controller = Products, action = Details, id = 5 }`。 MVC 會嘗試尋找名為 `ProductsController` 的控制器，並執行動作 `Details`：

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

請注意，在此範例中，模型繫結會在叫用此動作時，使用 `id = 5` 值將 `id` 參數設定為 `5`。 如需詳細資料，請參閱[模型繫結](../models/model-binding.md)。

使用 `default` 路由：

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

路由範本：

* `{controller=Home}` 將 `Home` 定義為預設的 `controller`

* `{action=Index}` 將 `Index` 定義為預設的 `action`

* `{id?}` 將 `id` 定義為選擇性

預設和選擇性路由參數不一定要全部出現在 URL 路徑中才算相符。 如需路由範本語法的詳細描述，請參閱[路由範本參考](xref:fundamentals/routing#route-template-reference)。

`"{controller=Home}/{action=Index}/{id?}"` 可能符合 URL 路徑 `/`，並將產生路由值 `{ controller = Home, action = Index }`。 `controller` 和 `action` 的值使用預設值；`id` 不會產生值，因為 URL 路徑中沒有對應的區段。 MVC 會使用這些路由值來選取 `HomeController` 和 `Index` 動作：

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

使用此控制器定義和路由範本，就會對下列任何 URL 路徑執行 `HomeController.Index` 動作：

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

`UseMvcWithDefaultRoute` 方法很方便：

```csharp
app.UseMvcWithDefaultRoute();
```

可用來取代：

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`UseMvc` 和 `UseMvcWithDefaultRoute` 會將 `RouterMiddleware` 的執行個體新增至中介軟體管線。 MVC 不會直接與中介軟體互動，而是使用路由來處理要求。 MVC 會透過 `MvcRouteHandler` 的執行個體連線到路由。 `UseMvc` 內的程式碼類似如下：

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc` 不會直接定義任何路由，而是將預留位置新增至 `attribute` 路由的路由集合。 多載 `UseMvc(Action<IRouteBuilder>)` 可讓您新增自己的路由，同時支援屬性路由。  `UseMvc`其所有變體都為屬性路由新增佔位元 ─屬性路由始終可用,無論您如何`UseMvc`設定 。 `UseMvcWithDefaultRoute` 會定義預設路由並支援屬性路由。 [屬性路由](#attribute-routing-ref-label)一節包含屬性路由的詳細資料。

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a>慣例路由

`default` 路由：

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

前面的代碼是常規路由的範例。 此樣式稱為傳統路由,因為它為網址路徑建立了*約定*:

* 第一個路徑段映射到控制器名稱。
* 第二個映射到操作名稱。
* 第三個段用於選擇`id`。 `id`映射到模型實體。

使用此 `default` 路由，URL 路徑 `/Products/List` 會對應至 `ProductsController.List` 動作；而 `/Blog/Article/17` 會對應至 `BlogController.Article`。 此對應**只會**根據控制器和動作名稱，而不會根據命名空間、來源檔案位置或方法參數。

> [!TIP]
> 慣例路由與預設路由的搭配使用可讓您快速建置應用程式，而不需要針對每個定義的動作產生新的 URL 模式。 針對具有 CRUD 樣式動作的應用程式，在控制器之間保持一致的 URL 有助於簡化程式碼，並讓 UI 更容易預測。

> [!WARNING]
> 路由範本將 `id` 定義為選擇性，也就是您的動作可以直接執行，而不需要將識別碼當作 URL 的一部分提供。 如果省略 URL 中的 `id`，通常表示它會由模型繫結設定為 `0`，因此在符合 `id == 0` 的資料庫中找不到任何實體。 屬性路由可提供您更細微的控制，讓某些動作需要此識別碼，而其他動作則不需要。 依照慣例，本文件將會包含可能出現在正確使用中的選擇性參數 (例如 `id`)。

## <a name="multiple-routes"></a>多個路由

您可以將更多呼叫新增至 `MapRoute`，以在 `UseMvc` 內新增多個路由。 這樣做可讓您定義多個慣例，或新增特定動作專用的慣例路由，例如：

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

這裡的 `blog` 路由是「專用慣例路由」**，也就是它會使用慣例路由系統，但專用於特定動作。 由於 `controller` 和 `action` 並未作為參數出現在路由範本中，它們只能有預設值，因此此路由一律會對應至動作 `BlogController.Article`。

路由集合中的路由已經過排序，並將依其新增順序進行處理。 因此在此範例中，會先嘗試 `blog` 路由，再嘗試 `default` 路由。

> [!NOTE]
> *專用常規路由*通常使用**捕獲所有**路由`{*article}`參數,如捕獲 URL 路徑的剩餘部分。 這可能會讓路由變得「太窮盡」，也就是它會比對您想要讓其他路由比對的 URL。 將這些「窮盡」路由放在路由表後面可解決此問題。

### <a name="fallback"></a>後援

在要求處理過程中，MVC 會確認路由值是否可用來尋找應用程式中的控制器和動作。 如果路由值未符合任何動作，則不會將此路由視為一個相符項目，並將嘗試下一個路由。 這稱為「遞補」**，其目的在於簡化慣例路由重疊的情況。

### <a name="disambiguating-actions"></a>釐清動作

若透過路由符合兩個動作，MVC 必須釐清並選擇「最佳」候選項目，否則會擲回例外狀況。 例如：

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

此控制器定義兩個符合 URL 路徑 `/Products/Edit/17` 和路由資料 `{ controller = Products, action = Edit, id = 17 }` 的動作。 這是 MVC 控制器的典型模式，其中 `Edit(int)` 會顯示用以編輯產品的表單，而 `Edit(int, Product)` 會處理已張貼的表單。 若要執行這項操作，MVC 必須在要求為 HTTP `POST` 時選擇 `Edit(int, Product)`，並在 HTTP 動詞命令為任何其他項目時選擇 `Edit(int)`。

`HttpPostAttribute` ( `[HttpPost]` ) 是 `IActionConstraint` 的實作，只有在 HTTP 動詞命令為 `POST` 時，才能選取此動作。 `IActionConstraint` 的存在使 `Edit(int, Product)` 比 `Edit(int)`「更符合」，因此會先嘗試 `Edit(int, Product)`。

您只需要在特殊情況下撰寫自訂 `IActionConstraint` 實作，但請務必了解 `HttpPostAttribute` 等屬性的角色 (其他 HTTP 動詞命令會定義類似的屬性)。 在慣例路由中，當動作是 `show form -> submit form` 工作流程的一部分時，通常會使用相同的動作名稱。 檢閱[了解 IActionConstraint](#understanding-iactionconstraint) 一節之後，會更清楚此模式的便利性。

如果多個路由相符，而且 MVC 找不到「最佳」路由，則會擲回 `AmbiguousActionException`。

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a>路由名稱

下列範例中的字串 `"blog"` 和 `"default"` 是路由名稱：

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

路由名稱為路由提供邏輯名稱，因此可以使用具名路由來產生 URL。 當路由排序可能使 URL 產生作業變得複雜時，這樣做可大幅簡化 URL 產生作業。 在整個應用程式內路由名稱必須是唯一的。

路由名稱不會影響要求的 URL 比對或處理，只會用於 URL 產生。 [路由](xref:fundamentals/routing)提供 URL 產生的詳細資訊，包括 MVC 特定協助程式中的 URL 產生。

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a>屬性路由

屬性路由使用一組屬性，將動作直接對應至路由範本。 在下列範例中，在 `Configure` 方法中使用 `app.UseMvc();`，且未傳遞任何路由。 `HomeController` 會比對一組 類似於預設路由 `{controller=Home}/{action=Index}/{id?}` 所比對的 URL：

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

`HomeController.Index()` 動作會針對 URL 路徑 `/`、`/Home` 或 `/Home/Index` 的任何一個來執行。

> [!NOTE]
> 此範例醒目提示屬性路由與慣例路由之間的主要程式設計差異。 屬性路由需要更多輸入才能指定路由，而慣例預設路由處理路由的方式則更簡潔。 不過，屬性路由允許 (並需要) 精確地控制套用至每個動作的路由範本。

使用屬性路由，選取動作時「不會」**** 根據控制器名稱和動作名稱。 此範例會符合與上一個範例相同的 URL。

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> 上述路由範本未定義 `action`、`area` 和 `controller` 的路由參數。 事實上，屬性路由中不允許有這些路由參數。 由於路由範本已與一個動作建立關聯，因此剖析 URL 中的動作名稱並沒有任何意義。

## <a name="attribute-routing-with-httpverb-attributes"></a>使用 Http[Verb] 屬性的屬性路由

屬性路由也可以使用 `Http[Verb]` 屬性，例如 `HttpPostAttribute`。 這些屬性都會接受路由範本。 此範例顯示兩個符合相同路由範本的動作：

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

針對 `/products` 等 URL 路徑，當 HTTP 動詞命令為 `GET` 時，會執行 `ProductsApi.ListProducts`；當 HTTP 動詞命令為 `POST` 時，會執行 `ProductsApi.CreateProduct`。 屬性路由會先根據路由屬性所定義的一組路由範本來比對 URL。 一旦有路由範本相符，就會套用 `IActionConstraint` 條件約束以決定可執行的動作。

> [!TIP]
> 構建 REST API 時,`[Route(...)]`很少需要使用 操作方法,因為操作將接受所有 HTTP 方法。 最好是使用更明確的 `Http*Verb*Attributes`，以精確地指定 API 的支援項目。 REST API 的用戶端必須知道哪些路徑和 HTTP 動詞命令對應至特定邏輯作業。

由於屬性路由會套用至特定動作，因此輕鬆就能將參數設為路由範本定義的必要部分。 在此範例中，`id` 是 URL 路徑的必要部分。

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

`ProductsApi.GetProduct(int)` 動作會針對 `/products/3` 等 URL 路徑來執行，但不會針對 `/products` 等 URL 路徑來執行。 如需路由範本和相關選項的完整描述，請參閱[路由](xref:fundamentals/routing)。

## <a name="route-name"></a>路由名稱

下列程式碼會定義 `Products_List` 的「路由名稱」**：

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

您可以使用路由名稱根據特定路由來產生 URL。 路由名稱不會影響路由的 URL 比對行為，只會用於 URL 產生。 在整個應用程式內路由名稱必須是唯一的。

> [!NOTE]
> 慣例「預設路由」** 與此相反，只會將 `id` 參數定義為選擇性 (`{id?}`)。 能夠精確指定 API 有些優點，像是允許將 `/products` 和 `/products/5` 分派至不同的動作。

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a>合併路由

為了避免屬性路由過於重複，控制器上的路由屬性可與個別動作上的路由屬性合併。 控制器上定義的任何路由範本都會附加到動作上的路由範本之前。 將路由屬性放在控制器上，即可讓控制器中的**所有**動作使用屬性路由。

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

在此範例中，URL 路徑 `/products` 可能符合 `ProductsApi.ListProducts`，而 URL 路徑 `/products/5` 可能符合 `ProductsApi.GetProduct(int)`。 這兩個操作都僅與`GET`HTTP 符合,因為`HttpGetAttribute`它們被標記為 。

套用至開頭為 `/` 或 `~/` 之動作的路由範本，無法與套用至控制器的路由範本合併。 此範例會比對一組類似於「預設路由」** 的 URL 路徑。

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Doesn't combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a>排序屬性路由

與按定義順序執行的傳統路由不同,屬性路由生成樹並同時匹配所有路由。 此行為如同依理想的排序來排列路由項目，最明確的路由會有機會在較泛型的路由之前執行。

例如，`blog/search/{topic}` 等路由比 `blog/{*article}` 等路由更明確。 邏輯上來說，預設會先「執行」`blog/search/{topic}`，因為這是唯一合理的排序。 使用慣例路由，開發人員會負責依想要的順序來排列路由。

您可以使用架構提供之所有路由屬性的 `Order` 屬性，來設定屬性路由的順序。 路由會依 `Order` 屬性的遞增排序來處理。 預設順序為 `0`。 使用 `Order = -1` 設定的路由會在未設定順序的路由之前執行。 使用 `Order = 1` 設定的路由會在預設路由排序之後執行。

> [!TIP]
> 請避免依賴 `Order`。 如果您的 URL 空間需要明確的順序值才能正確地路由，則同樣也可能會使用戶端混淆。 一般而言，屬性路由會透過 URL 比對來選取正確的路由。 如果用於 URL 產生的預設順序無效，使用路由名稱作為覆寫通常會比套用 `Order` 屬性更簡單。

Razor Pages 路由和 MVC 控制器路由會共用實作。 如需 Razor Pages 主題中路由順序的資訊，請參閱 [Razor Pages 路由和應用程式慣例：路由順序](xref:razor-pages/razor-pages-conventions#route-order)。

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>路由範本中的語彙基元取代 ([controller]、[action]、[area])

為方便起見,屬性路由通過將權杖包裝在方形括弧 (,`[` `]`) 支援*權杖 。* 語彙基元 `[action]`、`[area]` 與 `[controller]` 會分別以定義路由之動作的動作名稱值、區域名稱值和控制器名稱值來取代。 在下列範例中，動作會符合註解中所述的 URL 路徑：

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

語彙基元取代會在建立屬性路由的最後一個步驟發生。 上述範例的運作方式與下列程式碼相同：

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

屬性路由也可以透過繼承來合併。 這與語彙基元取代結合會特別強大。

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPut("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

語彙基元取代也適用於屬性路由所定義的路由名稱。 `[Route("[controller]/[action]", Name="[controller]_[action]")]` 會針對每個動作產生唯一的路由名稱。

若要比對常值語彙基元取代分隔符號 `[` 或 `]`，請重複字元 (`[[` 或 `]]`) 來將它逸出。

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a>使用參數轉換程式自訂語彙基元取代

可以使用參數轉換程式自訂語彙基元取代。 參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。 例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。

`RouteTokenTransformerConvention` 是一個應用程式模型慣例，它會：

* 將參數轉換程式套用到應用程式中的所有屬性路由。
* 在取代屬性路由語彙基元值時自訂它們。

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

`RouteTokenTransformerConvention` 會在 `ConfigureServices` 中註冊為選項。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options =>
    {
        options.Conventions.Add(new RouteTokenTransformerConvention(
                                     new SlugifyParameterTransformer()));
    });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end


::: moniker range="< aspnetcore-3.0"
<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a>多個路由

屬性路由支援定義多個路由來達到相同的動作。 最常見的用法是模擬「預設慣例路由」** 的行為，如下列範例所示：

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

將多個路由屬性放在控制器上，表示這些屬性會各自與動作方法上的每個路由屬性合併。

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

當一個動作上放有多個路由屬性 (實作`IActionConstraint`) 時，則每個動作條件約束都會與屬性中用來定義的路由範本合併。

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> 雖然在動作上使用多個路由看似強大，但最好保持簡單且妥善定義的應用程式 URL 空間。 只有必要時，才在動作上使用多個路由；例如，為了支援現有的用戶端。

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>指定屬性路由的選擇性參數、預設值和條件約束

屬性路由支援使用與慣例路由相同的內嵌語法，來指定選擇性參數、預設值和條件約束。

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

如需路由範本語法的詳細描述，請參閱[路由範本參考](xref:fundamentals/routing#route-template-reference)。

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>使用 `IRouteTemplateProvider` 的自訂路由屬性

架構中提供的所有路由屬性 (`[Route(...)]`、`[HttpGet(...)]` 等) 都會實作 `IRouteTemplateProvider` 介面。 當應用程式啟動並使用實作 `IRouteTemplateProvider` 的屬性來建立初始路由集時，MVC 會尋找控制器類別和動作方法上的屬性。

您可以實作 `IRouteTemplateProvider` 來定義自己的路由屬性。 每個 `IRouteTemplateProvider` 都可讓您定義具有自訂路由範本、順序和名稱的單一路由：

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

上述範例中的屬性會在套用 `[MyApiController]` 時，自動將 `Template` 設定為 `"api/[controller]"`。

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>使用應用程式模型自訂屬性路由

「應用程式模型」** 是在啟動時建立的物件模型，其中包含 MVC 用來路由及執行動作的所有中繼資料。 「應用程式模型」** 包含從路由屬性收集的所有資料 (透過 `IRouteTemplateProvider`)。 您可以撰寫「慣例」** 來修改啟動時的應用程式模型，以自訂路由的運作方式。 本節簡單示範如何使用應用程式模型來自訂路由。

[!code-csharp[](routing/samples/2.x/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>混合路由：屬性路由與慣例路由

MVC 應用程式可以混用慣例路由與屬性路由。 控制器通常會使用慣例路由來提供 HTML 頁面給瀏覽器，並使用屬性路由來提供 REST API。

動作可以使用慣例路由或屬性路由。 將路由放在控制器或動作上，即可讓它使用屬性路由。 定義屬性路由的動作無法透過慣例路由到達，反之亦然。 控制器上的**任何**路由屬性可讓控制器中的所有動作使用屬性路由。

> [!NOTE]
> 這兩種路由系統的區別在於 URL 符合某個路由範本之後所套用的程序。 在慣例路由中，會使用相符項目中的路由值，從所有慣例路由動作的查閱資料表中選擇動作和控制器。 在屬性路由中，每個範本已與一個動作建立關聯，因此不需要進一步查閱。

## <a name="complex-segments"></a>複雜區段

複雜區段 (例如，`[Route("/dog{token}cat")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。 如需說明，請參閱[原始程式碼](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296)。 如需詳細資訊，請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/8197)。

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>URL 產生

MVC 應用程式可以使用路由的 URL 產生功能，來產生動作的 URL 連結。 產生 URL 可排除硬式編碼的 URL，讓程式碼更穩定且更容易維護。 本節著重於 MVC 所提供的 URL 產生功能，並只會涵蓋 URL 產生運作方式的基本概念。 如需 URL 產生的詳細描述，請參閱[路由](xref:fundamentals/routing)。

`IUrlHelper` 介面是 MVC 與用於產生 URL 的路由之間的基礎結構部分。 您將會透過控制器、檢視和檢視元件中 `Url` 屬性，來尋找可用的 `IUrlHelper` 執行個體。

在此範例中，會透過 `Controller.Url` 屬性使用 `IUrlHelper` 介面來產生另一個動作的 URL。

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

如果應用程式使用預設慣例路由，`url` 變數的值會是 URL 路徑字串 `/UrlGeneration/Destination`。 此 URL 路徑是透過路由所建立，結合了目前要求中的路由值 (環境值) 與傳遞至 `Url.Action` 的值，並在路由範本中取代這些值：

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

路由範本中每個路由參數的值都會以相符名稱的值和環境值所取代。 不具有任何值的路由參數可以使用預設值 (若有)；如果是選擇性，則可以略過 (如此範例中的 `id` 所示)。 如果任何必要的路由參數沒有對應的值，URL 產生會失敗。 如果某個路由的 URL 產生失敗，則會嘗試下一個路由，直到嘗試所有路由或找到相符項目為止。

上述 `Url.Action` 範例假設使用慣例路由，但 URL 產生的運作方式會類似屬性路由，雖然概念完全不同。 在慣例路由中，使用路由值來展開範本，而且 `controller` 和 `action` 的路由值通常會出現在該範本中 (這是因為路由比對的 URL 符合「慣例」**)。 在屬性路由中，`controller` 和 `action` 的路由值不可以出現在範本中，而是用來查閱要使用的範本。

此範例使用屬性路由：

[!code-csharp[](routing/samples/2.x/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC 建立所有屬性路由動作的查閱資料表，並將比對 `controller` 和 `action` 值，以選取要用於 URL 產生的路由範本。 在上述範例中，會產生 `custom/url/to/destination`。

### <a name="generating-urls-by-action-name"></a>由動作名稱產生 URL

`Url.Action` (`IUrlHelper` . `Action`) 及所有相關多載的基本概念，都是您想要透過指定控制器名稱和動作名稱，來指定連結的項目。

> [!NOTE]
> 使用 `Url.Action` 時，會為您指定`controller` 和 `action` 的目前路由值 (`controller` 和 `action` 的值都屬於「環境值」** **和「值」** **)。 方法 `Url.Action` 一律會使用 `action` 和 `controller` 的目前值，而且會產生路由至目前動作的 URL 路徑。

路由會嘗試使用環境值中的值，來填入您在產生 URL 時未提供的資訊。 使用 `{a}/{b}/{c}/{d}` 等路由和環境值 `{ a = Alice, b = Bob, c = Carol, d = David }`，路由就會有足夠的資訊來產生不含任何其他值的 URL (因為所有路由參數都具有值)。 如果您新增值 `{ d = Donovan }`，則會忽略值 `{ d = David }`，而且產生的 URL 路徑會是 `Alice/Bob/Carol/Donovan`。

> [!WARNING]
> URL 路徑是階層式。 在上述範例中，如果您新增了值 `{ c = Cheryl }`，則會忽略 `{ c = Carol, d = David }` 這兩個值。 在此情況下，不再有 `d` 的值，因此 URL 產生會失敗。 您必須指定 `c` 和 `d` 所需的值。  使用預設路由 (`{controller}/{action}/{id?}`) 可能會遇到此問題，但實際上很少會遇到此行為，因為 `Url.Action` 一律會明確指定 `controller` 和 `action` 值。

較長的 `Url.Action` 多載還會接受一個額外的「路由值」** 物件，來為 `controller` 和 `action` 以外的路由參數提供值。 您最常會在搭配 `id` 時看到此用法，例如 `Url.Action("Buy", "Products", new { id = 17 })`。 依照慣例，「路由值」** 物件通常是匿名型別的物件，但也可以是 `IDictionary<>` 或「純舊 .NET 物件」**。 不符合路由參數的任何額外路由值都會放在查詢字串中。

[!code-csharp[](routing/samples/2.x/main/Controllers/TestController.cs)]

> [!TIP]
> 若要建立絕對 URL，請使用接受 `protocol` 的多載：`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>由路由產生 URL

上述程式碼示範如何藉由傳入控制器和動作名稱來產生 URL。 `IUrlHelper` 也提供 `Url.RouteUrl` 系列方法。 這些方法類似於 `Url.Action`，但不會將 `action` 和 `controller` 的目前值複製到路由值。 最常見的用法是指定路由名稱使用特定路由來產生 URL，通常「不需要」** 指定控制器或動作名稱。

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a>在 HTML 中產生 URL

`IHtmlHelper` 提供 `HtmlHelper` 方法 `Html.BeginForm` 和 `Html.ActionLink`，以分別產生 `<form>` 和 `<a>` 項目。 這些方法使用 `Url.Action` 方法來產生 URL，並接受類似的引數。 `HtmlHelper` 的成對 `Url.RouteUrl` 為 `Html.BeginRouteForm` 和 `Html.RouteLink`，這兩者的功能很類似。

TagHelper 透過 `form` TagHelper 和 `<a>` TagHelper 產生 URL。 這兩者使用 `IUrlHelper` 進行實作。 如需詳細資訊，請參閱[使用表單](../views/working-with-forms.md)。

在檢視中，可透過 `Url` 屬性使用 `IUrlHelper` 來產生上述未涵蓋的任何特定 URL。

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a>在動作結果中產生 URL

上述範例示範如何在控制器中使用 `IUrlHelper`，但控制器中最常見的用法是產生 URL 以作為動作結果的一部分。

`ControllerBase` 和 `Controller` 基底類別提供便利的方法讓動作結果可參考其他動作。 一個典型的用法是在接受使用者輸入之後重新導向。

```csharp
public IActionResult Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
    return View(customer);
}
```

動作結果的 Factory 方法遵循類似於 `IUrlHelper` 上方法的模式。

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>專用慣例路由的特殊案例

慣例路由可使用一種特殊路由定義，稱為「專用慣例路由」**。 在下列範例中，名為 `blog` 的路由是專用慣例路由。

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

透過這些路由定義，`Url.Action("Index", "Home")` 會使用 `default` 路由產生 URL 路徑 `/`，但為什麼？ 您可能會猜想路由值 `{ controller = Home, action = Index }` 便足以使用 `blog` 來產生 URL，且結果會是 `/blog?action=Index&controller=Home`。

專用慣例路由依賴沒有對應路由參數之預設值的特殊行為，以防止用於 URL 產生的路由變得「太窮盡」。 在本例中，預設值為 `{ controller = Blog, action = Article }`，`controller` 和 `action` 都不會顯示為路由參數。 當路由執行 URL 產生時，所提供的值必須符合預設值。 使用 `blog` 產生 URL 會失敗，因為值 `{ controller = Home, action = Index }` 不符合 `{ controller = Blog, action = Article }`。 路由會接著切換並嘗試 `default`，此時會成功。

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>區域

[區域](areas.md)是 MVC 功能，可將相關功能組織成群組，作為個別路由命名空間 (適用於控制器動作) 和資料夾結構 (適用於檢視)。 使用區域可讓應用程式具有多個同名的控制器 (只要這些控制器具有不同的「區域」** 即可)。 使用區域可建立用於路由的階層，方法是將另一個路由參數 `area` 新增至 `controller` 和 `action`。 本節將討論路由如何與區域互動；如需區域如何與檢視搭配使用的詳細資料，請參閱[區域](areas.md)。

下列範例會設定 MVC 使用預設慣例路由，並為名為 `Blog` 的區域設定「區域路由」**：

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

當符合 `/Manage/Users/AddUser` 等 URL 路徑時，第一個路由會產生路由值 `{ area = Blog, controller = Users, action = AddUser }`。 `area` 路由值是由 `area` 的預設值所產生；事實上，`MapAreaRoute` 所建立的路由相當於：

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute` 會針對使用所提供之區域名稱 (在本例中為 `Blog`) 的 `area`，使用預設值和條件約束來建立路由。 預設值可確保路由一律會產生 `{ area = Blog, ... }`，而條件約束需要 `{ area = Blog, ... }` 值以產生 URL。

> [!TIP]
> 慣例路由與順序息息相關。 一般而言，具有區域的路由應該放在路由表前面，因為這些路由比沒有區域的路由更明確。

使用上述範例，路由值會符合下列動作：

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

`AreaAttribute` 可用來指定控制器為區域的一部分，假設此控制器在 `Blog` 區域中。 不含 `[Area]` 屬性的控制器不是任何區域的成員，因此當路由提供 `area` 路由值時**不會**符合。 在下列範例中，只有列出的第一個控制器可能符合路由值 `{ area = Blog, controller = Users, action = AddUser }`。

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> 為求完整起見，此處顯示每個控制器的命名空間，否則控制器會有命名衝突並產生編譯器錯誤。 類別命名空間不會影響 MVC 的路由。

前兩個控制器是區域的成員，只有在 `area` 路由值提供其各自的區域名稱時才會符合。 第三個控制器不是任何區域的成員，只有路由未提供任何值給 `area` 時才會符合。

> [!NOTE]
> 在「未符合任何值」** 的情況下，缺少 `area` 值相當於 `area` 的值為 Null 或空字串。

執行區域中的動作時，`area` 的路由值會作為路由用於 URL 產生的「環境值」**。 這表示區域預設會以「黏性」** 方式來處理 URL 產生，如下列範例所示。
[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a>了解 IActionConstraint

> [!NOTE]
> 本節深入探討架構內部及 MVC 如何選擇要執行的動作。 一般應用程式不需要自訂 `IActionConstraint`

即使您不熟悉 `IActionConstraint`，也可能已使用過此介面。 `[HttpGet]` 屬性和類似的 `[Http-VERB]` 屬性會實作 `IActionConstraint` 來限制動作方法的執行。

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

假設在預設慣例路由中，URL 路徑 `/Products/Edit` 會產生值 `{ controller = Products, action = Edit }`，該值符合此處所示的**兩個**動作。 在 `IActionConstraint` 術語中，我們會假設這兩個動作可視為候選項目 (因為它們都符合路由資料)。

當 `HttpGetAttribute` 執行時，*Edit()* 會符合 *GET*，但不符合任何其他 HTTP 動詞命令。 `Edit(...)` 動作未定義任何條件約束，因此會符合任何 HTTP 動詞命令。 假設 `POST` - 只有 `Edit(...)` 相符。 但對於 `GET`，這兩個動作仍然相符；不過，具有 `IActionConstraint` 的動作一律比沒有的動作「更符合」**。 由於 `Edit()` 具有 `[HttpGet]`，因此更明確；如果兩個動作都相符，就會選取此動作。

就概念而言，`IActionConstraint` 是一種「多載」** 形式，但它會在符合相同 URL　的動作之間進行多載，而不是多載具有相同名稱的方法。 屬性路由也會使用 `IActionConstraint`，因此可能導致來自不同控制器的動作被視為候選項目。

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a>實作 IActionConstraint

實作 `IActionConstraint` 的最簡單方式是建立衍生自 `System.Attribute` 的類別，並將它放在您的動作和控制器上。 MVC 會自動探索作為屬性套用的任何 `IActionConstraint`。 您可以使用應用程式模型來套用條件約束，由於您可以之後編寫其套用方式的程式，因此可能是最彈性的方法。

在下面的示例中,約束根據路由數據*中的國家/地區代碼*選擇操作。 [GitHub 上有完整範例](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs)。

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

您必須負責實作 `Accept` 方法，並選擇 'Order' 作為要執行的條件約束。 在本例中，`Accept` 方法會傳回 `true`，表示當 `country` 路由值相符時動作會符合。 這與 `RouteValueAttribute` 不同，後者允許切換回未使用屬性的動作。 此範例顯示如果您定義 `en-US` 動作，則 `fr-FR` 等國碼 (地區碼) 會切換回未套用 `[CountrySpecific(...)]` 的更泛型控制器。

`Order` 屬性決定條件約束所屬的「階段」**。 群組中的動作條件約束會根據 `Order` 來執行。 例如，架構提供的所有 HTTP 方法屬性都會使用相同的 `Order` 值，以便在同一個階段中執行。 您可以視需要擁有許多階段來實作所需的原則。

> [!TIP]
> 若要決定 `Order` 的值，請考慮是否應該在 HTTP 方法之前套用條件約束。 數值愈低會愈先執行。

::: moniker-end
