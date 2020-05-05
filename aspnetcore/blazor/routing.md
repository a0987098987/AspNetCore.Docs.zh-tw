---
title: ASP.NET Core Blazor路由
author: guardrex
description: 瞭解如何在應用程式中路由傳送要求，以及關於 NavLink 元件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/routing
ms.openlocfilehash: 017fd4d3ab45b75355dabb400ff0e5cbf7009d82
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82771203"
---
# <a name="aspnet-core-blazor-routing"></a>ASP.NET Core Blazor 路由

作者：[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

瞭解如何路由傳送要求，以及如何使用`NavLink`元件在 Blazor apps 中建立導覽連結。

## <a name="aspnet-core-endpoint-routing-integration"></a>ASP.NET Core 端點路由整合

Blazor 伺服器已整合到[ASP.NET Core 端點路由](xref:fundamentals/routing)中。 ASP.NET Core 應用程式已設定為接受`MapBlazorHub`中`Startup.Configure`的互動式元件連入連線：

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

最常見的設定是將所有要求路由傳送至 Razor 頁面，做為 Blazor 伺服器應用程式的伺服器端部分的主機。 依照慣例，[*主機*] 頁面通常會命名為 *_Host. cshtml*。 主機檔案中指定的路由稱為「回溯*路由*」，因為它在路由比對中以低優先順序運作。 當其他路由不相符時，就會考慮回退路由。 這可讓應用程式使用其他控制器和頁面，而不會干擾 Blazor 伺服器應用程式。

## <a name="route-templates"></a>路由範本

此`Router`元件可讓您使用指定的路由來路由傳送至每個元件。 此`Router`元件會出現在*應用程式 razor*檔案中：

```razor
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

編譯含有 *.razor* `@page`指示詞的 razor 檔案時，會提供<xref:Microsoft.AspNetCore.Components.RouteAttribute>指定路由範本的所產生類別。

在執行時間， `RouteView`元件：

* `RouteData`從接收， `Router`連同任何想要的參數。
* 使用指定的參數，以配置（或選擇性的預設版面配置）呈現指定的元件。

您可以選擇性地指定`DefaultLayout`具有版面配置類別的參數，以用於未指定版面配置的元件。 預設的 Blazor 範本會指定`MainLayout`元件。 *MainLayout*是在範本專案的*共用*資料夾中。 如需版面配置的詳細資訊<xref:blazor/layouts>，請參閱。

多個路由範本可以套用至元件。 下列元件會回應和`/BlazorRoute` `/DifferentBlazorRoute`的要求：

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

> [!IMPORTANT]
> 若要讓 Url 正確解析，應用程式必須在`<base>`其*Wwwroot/index.html*檔（Blazor WebAssembly）中包含標記，或在`href`屬性（`<base href="/">`）中指定應用程式基底路徑的*Pages/_Host. cshtml*檔案（Blazor 伺服器）。 如需詳細資訊，請參閱<xref:host-and-deploy/blazor/index#app-base-path>。

## <a name="provide-custom-content-when-content-isnt-found"></a>在找不到內容時提供自訂內容

如果`Router`找不到所要求路由的內容，此元件可讓應用程式指定自訂內容。

在*應用程式的 razor*檔案中，于`NotFound` `Router`元件的範本參數中設定自訂內容：

```razor
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

使用`AdditionalAssemblies`參數來指定搜尋可路由的元件`Router`時，要考慮的元件的其他元件。 除了指定的`AppAssembly`元件以外，還會考慮指定的元件。 在下列範例中， `Component1`是在參考的類別庫中定義的可路由元件。 下列`AdditionalAssemblies`範例會產生的`Component1`路由支援：

```razor
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a>路由參數

路由器會使用路由參數來填入具有相同名稱的對應元件參數（不區分大小寫）：

```razor
@page "/RouteParameter"
@page "/RouteParameter/{text}"

<h1>Blazor is @Text!</h1>

@code {
    [Parameter]
    public string Text { get; set; }

    protected override void OnInitialized()
    {
        Text = Text ?? "fantastic";
    }
}
```

不支援選擇性參數。 在`@page`上述範例中會套用兩個指示詞。 第一個則允許不使用參數導覽至元件。 第二`@page`個指示詞`{text}`會採用 route 參數，並將值`Text`指派給屬性。

## <a name="route-constraints"></a>路由條件約束

路由條件約束會將路由區段上的類型比對強制套用至元件。

在下列範例中，對`Users`元件的路由只會符合下列條件：

* 要求`Id` URL 上有路由區段。
* `Id`區段是一個整數（`int`）。

[!code-razor[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

下表所示的路由條件約束可供使用。 若為符合不因文化特性而異的路由條件約束，請參閱表格下方的警告以取得詳細資訊。

| 條件約束 | 範例           | 範例相符項目                                                                  | 非變異值<br>culture<br>比對 |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | 否                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | 是                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | 是                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | 是                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | 是                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | 否                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | 是                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | 是                              |

> [!WARNING]
> 確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。 這些條件約束假設 URL 不可當地語系化。

### <a name="routing-with-urls-that-contain-dots"></a>包含點的 Url 路由

在 Blazor 伺服器應用程式中， *_Host*中的預設路由是`/` （`@page "/"`）。 包含點（`.`）的要求 URL 不符合預設路由，因為 URL 會顯示要求檔案。 如果靜態檔案不存在，Blazor 應用程式會傳回*404-找不*到的回應。 若要使用包含點的路由，請使用下列路由範本來設定 *_Host. cshtml* ：

```cshtml
@page "/{**path}"
```

此`"/{**path}"`範本包含：

* 雙星號*catch-all*語法（`**`）可跨多個資料夾界限捕捉路徑，而不需要編碼正`/`斜線（）。
* `path`路由參數名稱。

> [!NOTE]
> Razor 元件（*razor*）中`*`/`**`**不**支援*Catch-all*參數語法（）。

如需詳細資訊，請參閱<xref:fundamentals/routing>。

## <a name="navlink-component"></a>NavLink 元件

建立導覽`NavLink`連結時，請使用元件來取代`<a>`HTML 超連結元素（）。 元件`NavLink`的行為類似`<a>`元素，但它會根據其`active` `href`是否符合目前的 URL 來切換 CSS 類別。 `active`類別可協助使用者瞭解在顯示的導覽連結中，哪個頁面是使用中的頁面。

下列`NavMenu`元件會建立[啟動](https://getbootstrap.com/docs/)程式導覽列，示範如何使用`NavLink`元件：

[!code-razor[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

有兩個`NavLinkMatch`選項可供您指派給`Match` `<NavLink>`元素的屬性：

* `NavLinkMatch.All`&ndash;當`NavLink`符合整個目前的 URL 時，就會作用中。
* `NavLinkMatch.Prefix`（*預設值*）&ndash;當`NavLink`符合目前 URL 的任何前置詞時，就會作用中。

在上述範例中，home `NavLink` `href=""`會符合 home URL，而且只會在`active`應用程式的預設基底路徑 URL （例如， `https://localhost:5001/`）接收 CSS 類別。 第二`NavLink`個會`active`在使用者造訪具有`MyComponent`前置詞的任何 URL 時收到類別（例如`https://localhost:5001/MyComponent` ， `https://localhost:5001/MyComponent/AnotherSegment`和）。

其他`NavLink`元件屬性會傳遞至呈現的錨點標記。 在下列範例中， `NavLink`元件包含`target`屬性：

```razor
<NavLink href="my-page" target="_blank">My page</NavLink>
```

會轉譯下列 HTML 標籤：

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>URI 和流覽狀態協助程式

用於<xref:Microsoft.AspNetCore.Components.NavigationManager>在 c # 程式碼中處理 uri 和導覽。 `NavigationManager`提供下表所示的事件和方法。

| member | 說明 |
| ------ | ----------- |
| Uri | 取得目前的絕對 URI。 |
| BaseUri | 取得可在相對 URI 路徑前面加上的基底 URI （含尾端斜線），以產生絕對 URI。 通常會`BaseUri`對應至`href` *wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. cshtml* （Blazor Server）中檔`<base>`元素的屬性。 |
| NavigateTo | 導覽至指定的 URI。 如果`forceLoad`為`true`：<ul><li>已略過用戶端路由。</li><li>瀏覽器會強制從伺服器載入新頁面，無論 URI 是否通常由用戶端路由器處理。</li></ul> |
| LocationChanged | 導覽位置變更時引發的事件。 |
| ToAbsoluteUri | 將相對 URI 轉換為絕對 URI。 |
| <span style="word-break:normal;word-wrap:normal">ToBaseRelativePath</span> | 假設基底 URI （例如先前傳回的`GetBaseUri`uri），會將絕對 uri 轉換成相對於基底 uri 前置詞的 uri。 |

下列元件會在選取按鈕時， `Counter`流覽至應用程式的元件：

```razor
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

下列元件會處理位置已變更事件。 當`HandleLocationChanged`架構呼叫時`Dispose` ，方法會解除掛鉤。 Unhooking 方法允許元件的垃圾收集。

```razor
@implements IDisposable
@inject NavigationManager NavigationManager

...

protected override void OnInitialized()
{
    NavigationManager.LocationChanged += HandleLocationChanged;
}

private void HandleLocationChanged(object sender, LocationChangedEventArgs e)
{
    ...
}

public void Dispose()
{
    NavigationManager.LocationChanged -= HandleLocationChanged;
}
```

<xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs>提供事件的下列相關資訊：

* <xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.Location>&ndash;新位置的 URL。
* <xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.IsNavigationIntercepted>&ndash;如果`true`為Blazor ，則會攔截瀏覽器的導覽。 如果`false`為，則[NavigationManager NavigateTo](xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A)會造成導覽。

如需有關元件處置的詳細資訊<xref:blazor/lifecycle#component-disposal-with-idisposable>，請參閱。
