---
title: ASP.NET Core Blazor 路由
author: guardrex
description: 瞭解如何在應用程式中路由傳送要求，以及關於 NavLink 元件。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/routing
ms.openlocfilehash: d348908261c51b477aa698a407266d05c0df5a33
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800338"
---
# <a name="aspnet-core-blazor-routing"></a>ASP.NET Core Blazor 路由

作者：[Luke Latham](https://github.com/guardrex)

瞭解如何路由傳送要求，以及如何使用`NavLink`元件在 Blazor apps 中建立導覽連結。

## <a name="aspnet-core-endpoint-routing-integration"></a>ASP.NET Core 端點路由整合

Blazor 伺服器端會整合到[ASP.NET Core 端點路由](xref:fundamentals/routing)中。 ASP.NET Core 應用程式已設定為接受`MapBlazorHub`中`Startup.Configure`的互動式元件連入連線：

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a>路由範本

此`Router`元件可讓您使用指定的路由來路由傳送至每個元件。 此`Router`元件會出現在*應用程式 razor*檔案中：

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

編譯含有 `@page`指示詞的 razor 檔案時，會提供<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>指定路由範本的所產生類別。

在執行時間， `RouteView`元件：

* 從接收，連同任何想要的參數。 `Router` `RouteData`
* 使用指定的參數，以配置（或選擇性的預設版面配置）呈現指定的元件。

您可以選擇性地指定`DefaultLayout`具有版面配置類別的參數，以用於未指定版面配置的元件。 預設的 Blazor 範本會指定`MainLayout`元件。 *MainLayout*是在範本專案的*共用*資料夾中。 如需版面配置的詳細資訊<xref:blazor/layouts>，請參閱。

多個路由範本可以套用至元件。 下列元件會回應和`/BlazorRoute` `/DifferentBlazorRoute`的要求：

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> 為了讓 url 正確解析，應用程式必須在其`<base>` *wwwroot/index.html*檔案（Blazor 用戶端）或*Pages/_Host*檔（Blazor 伺服器端）中包含標記，並在`href`屬性中指定應用程式基底路徑（`<base href="/">`). 如需詳細資訊，請參閱 <xref:host-and-deploy/blazor/index#app-base-path>。

## <a name="provide-custom-content-when-content-isnt-found"></a>在找不到內容時提供自訂內容

如果`Router`找不到所要求路由的內容，此元件可讓應用程式指定自訂內容。

在*應用程式的 razor*檔案中，于`NotFound` `Router`元件的範本參數中設定自訂內容：

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFound>
</Router>
```

`<NotFound>`標記的內容可以包含任意專案，例如其他互動式元件。 若要將預設版面配置`NotFound`套用至內容<xref:blazor/layouts>，請參閱。

## <a name="route-to-components-from-multiple-assemblies"></a>從多個元件路由至元件

使用參數來指定搜尋可路由的元件`Router`時，要考慮的元件的其他元件。 `AdditionalAssemblies` 除了指定的`AppAssembly`元件以外，還會考慮指定的元件。 在下列範例中， `Component1`是在參考的類別庫中定義的可路由元件。 下列`AdditionalAssemblies`範例會產生的`Component1`路由支援：

< 路由器 AppAssembly = "typeof （程式）。元件 "AdditionalAssemblies =" new [] {typeof （Component1）。Assembly} > .。。</Router>

## <a name="route-parameters"></a>路由參數

路由器會使用路由參數來填入具有相同名稱的對應元件參數（不區分大小寫）：

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

ASP.NET Core 3.0 Preview 中的 Blazor 應用程式不支援選擇性參數。 在`@page`上述範例中會套用兩個指示詞。 第一個則允許不使用參數導覽至元件。 第二`@page`個指示詞`{text}`會採用 route 參數，並將值`Text`指派給屬性。

## <a name="route-constraints"></a>路由條件約束

路由條件約束會將路由區段上的類型比對強制套用至元件。

在下列範例中，對`Users`元件的路由只會符合下列條件：

* 要求 URL 上有路由區段。`Id`
* 區段是一個整數（`int`）。 `Id`

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

下表所示的路由條件約束可供使用。 若為符合不因文化特性而異的路由條件約束，請參閱表格下方的警告以取得詳細資訊。

| 條件約束 | 範例           | 範例相符項目                                                                  | 非變異值<br>文化特性<br>比對 |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`、 `FALSE`                                                                  | 否                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`、 `2016-12-31 7:32pm`                                                | 是                              |
| `decimal`  | `{price:decimal}` | `49.99`、 `-1,000.01`                                                             | 是                              |
| `double`   | `{weight:double}` | `1.234`、 `-1,001.01e8`                                                           | 是                              |
| `float`    | `{weight:float}`  | `1.234`、 `-1,001.01e8`                                                           | 是                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`、 `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | 否                               |
| `int`      | `{id:int}`        | `123456789`、 `-123456789`                                                        | 是                              |
| `long`     | `{ticks:long}`    | `123456789`、 `-123456789`                                                        | 是                              |

> [!WARNING]
> 確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。 這些條件約束假設 URL 不可當地語系化。

### <a name="routing-with-urls-that-contain-dots"></a>包含點的 Url 路由

在 Blazor 伺服器端應用程式中， *_Host*中的預設路由是`/` （`@page "/"`）。 包含點（`.`）的要求 URL 不符合預設路由，因為 URL 會顯示要求檔案。 如果靜態檔案不存在，Blazor 應用程式會傳回*404-找不*到的回應。 若要使用包含點的路由，請使用下列路由範本來設定 *_Host* ：

```cshtml
@page "/{**path}"
```

此`"/{**path}"`範本包含：

* 雙星號*catch-all*語法（`**`）可跨多個資料夾界限捕捉路徑，而不需要編碼正`/`斜線（）。
* `path`路由參數名稱。

如需詳細資訊，請參閱 <xref:fundamentals/routing>。

## <a name="navlink-component"></a>NavLink 元件

建立導覽連結時，請使用`<a>`元件來取代HTML超連結元素（）。`NavLink` 元件`NavLink`的行為`<a>`類似元素`active` ，但它會根據其`href`是否符合目前的 URL 來切換 CSS 類別。 `active`類別可協助使用者瞭解在顯示的導覽連結中，哪個頁面是使用中的頁面。

下列`NavMenu`元件會建立[啟動](https://getbootstrap.com/docs/)程式導覽列，示範如何使用`NavLink`元件：

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

有兩個`NavLinkMatch`選項可`Match`供您指派給`<NavLink>`元素的屬性：

* `NavLinkMatch.All`當符合整個目前的 URL 時，就會作用中`NavLink`。 &ndash;
* `NavLinkMatch.Prefix`（*預設值*）當符合目前 URL 的任何前置詞時，就會作用中`NavLink`。 &ndash;

在上述`NavLink`範例中，home `href=""`會符合 home `active` URL，而且只會在應用程式的預設基底路徑 URL （例如， `https://localhost:5001/`）接收 CSS 類別。 第二`NavLink`個會`active`在使用者`MyComponent`造訪具有前置詞的任何 URL 時收到類別（例如`https://localhost:5001/MyComponent` ， `https://localhost:5001/MyComponent/AnotherSegment`和）。

其他`NavLink`元件屬性會傳遞至呈現的錨點標記。 在下列範例中， `NavLink`元件`target`包含屬性：

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

會轉譯下列 HTML 標籤：

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>URI 和流覽狀態協助程式

用`Microsoft.AspNetCore.Components.NavigationManager`來處理常式代碼中C#的 uri 和導覽。 `NavigationManager`提供下表所示的事件和方法。

| 成員 | 描述 |
| ------ | ----------- |
| `Uri` | 取得目前的絕對 URI。 |
| `BaseUri` | 取得可在相對 URI 路徑前面加上的基底 URI （含尾端斜線），以產生絕對 URI。 `href` `<base>`通常會對應至wwwroot/index.html（Blazor用戶端）或Pages/_Host（Blazor伺服器端）中檔元素上的屬性。`BaseUri` |
| `NavigateTo` | 導覽至指定的 URI。 如果`forceLoad`為`true`：<ul><li>已略過用戶端路由。</li><li>瀏覽器會強制從伺服器載入新頁面，無論 URI 是否通常由用戶端路由器處理。</li></ul> |
| `LocationChanged` | 導覽位置變更時引發的事件。 |
| `ToAbsoluteUri` | 將相對 URI 轉換為絕對 URI。 |
| `ToBaseRelativePath` | 假設基底 uri （例如先前傳回的`GetBaseUri`uri），會將絕對 uri 轉換成相對於基底 uri 前置詞的 uri。 |

下列元件會在選取按鈕時， `Counter`流覽至應用程式的元件：

```cshtml
@page "/navigate"
@inject NavigationManager NavigationManager

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        NavigationManager.NavigateTo("counter");
    }
}
```
