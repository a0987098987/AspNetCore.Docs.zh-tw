---
title: ASP.NET核心Blazor路由
author: guardrex
description: 瞭解如何在應用中路由請求以及 NavLink 元件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/routing
ms.openlocfilehash: 87579c88a37e0258921e199db2b5d8c7627f5499
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80218891"
---
# <a name="aspnet-core-blazor-routing"></a>ASP.NET核心布拉佐爾路由

作者：[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

瞭解如何路由請求以及如何使用`NavLink`元件在 Blazor 應用中創建導航連結。

## <a name="aspnet-core-endpoint-routing-integration"></a>ASP.NET核心終結點路由整合

布拉佐伺服器整合[到 ASP.NET 核心端點路由](xref:fundamentals/routing)。 ASP.NET核心應用設定為接受`MapBlazorHub`具有`Startup.Configure`的 互動式元件的傳入連線:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

最典型的配置是將所有請求路由到 Razor 頁面,該頁面充當 Blazor Server 應用伺服器端部分的主機。 依慣例,*主機*頁通常命名為 *_Host.cshtml*。 在主機檔中指定的路由稱為*回退路由*,因為它在路由匹配中優先順序較低。 當其他路由不匹配時,將考慮回退路由。 這允許應用程式使用其他控制器和頁面,而不會干擾 Blazor 伺服器應用程式。

## <a name="route-templates"></a>路由範本

該`Router`元件允許使用指定路由路由路由到每個元件。 元件`Router`將顯示在*App.razor*檔中:

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

編譯帶有`@page`指令的 *.razor*檔時,<xref:Microsoft.AspNetCore.Components.RouteAttribute>將提供指定 路由範本的生成類。

在執行時,`RouteView`元件:

* 從`RouteData`任何任何參數`Router`的參數接收 。
* 使用指定的參數渲染指定元件的佈局(或可選的預設佈局)。

可以選擇使用佈局類指定參數`DefaultLayout`,用於不指定佈局的元件。 預設的 Blazor 樣本`MainLayout`指定 元件。 *MainLayout.razor*位於範本專案的 *「共用」* 資料夾中。 有關佈局的詳細資訊,請參閱<xref:blazor/layouts>。

多個工藝路線範本可以應用於元件。 以下元件回應和`/BlazorRoute``/DifferentBlazorRoute`的請求。

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

> [!IMPORTANT]
> 要正確解析 URL,應用必須在其*wwwroot/index.html*檔(Blazor WebAssembly)或*頁面/_Host.cshtml*檔(Blazor Server)中包含一個`href``<base href="/">``<base>`標記,並在屬性 () 中指定應用基本路徑。 如需詳細資訊，請參閱 <xref:host-and-deploy/blazor/index#app-base-path>。

## <a name="provide-custom-content-when-content-isnt-found"></a>在找不到內容時提供自訂內容

如果`Router`未找到請求的路由的內容,則元件允許應用指定自定義內容。

在*App.razor*檔中`NotFound``Router`,在 元件的樣本參數中設定自訂內容:

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

`<NotFound>`標記的內容可以包括任意專案,如其他互動式元件。 要將默認佈局應用於`NotFound`內容,請參閱<xref:blazor/layouts>。

## <a name="route-to-components-from-multiple-assemblies"></a>從多個程式集路由到元件

使用`AdditionalAssemblies`參數`Router`為 元件指定在搜尋可路由元件時要考慮的其他程式集。 除了`AppAssembly`指定的程式集之外,還考慮指定程式集。 在下面的範例中,`Component1`是在引用的類庫中定義的可路由元件。 以下`AdditionalAssemblies`範例導致路`Component1`由 支援 :

```razor
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a>路由參數

路由器使用路由參數使用同名(不區分大小寫)填充相應的元件參數:

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

不支援可選參數。 在前面`@page`的示例中應用了兩個指令。 第一個允許在沒有參數的情況下導航到元件。 第二`@page`個指令以`{text}`由參數並將值分配給屬性`Text`。

## <a name="route-constraints"></a>路由約束

路由約束強制路由段上與元件的類型匹配。

在以下範例中,到元件的`Users`路由僅在以下情況匹配時匹配:

* `Id`請求 URL 上存在路由段。
* 段`Id`是整數`int`( 。

[!code-razor[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

下表中顯示的路由約束可用。 有關與不變區域性匹配的路由約束,請參閱下表下面的警告以瞭解更多資訊。

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

### <a name="routing-with-urls-that-contain-dots"></a>包含點的網址路由

在 Blazor Server 應用中 *,_Host.cshtml*中的`/`預設`@page "/"`路由為 ( 。 包含點`.`( ) 的請求網址 與預設路由不匹配,因為 URL 似乎請求檔。 Blazor 應用返回*404 - 未找到*不存在的靜態檔的回應。 要使用包含點的路由,請使用以下路由範本設定 *_Host.cshtml:*

```cshtml
@page "/{**path}"
```

該`"/{**path}"`樣本包括:

* 雙星號*捕獲所有*語`**`法 ( ) 用於跨多個資料夾邊界捕獲路徑,而無需編碼`/`正向斜杠 ( )。
* `path`路由參數名稱。

> [!NOTE]
> Razor 元件`*`/`**`(*.razor*)**不支援***「 全部捕捉*」參數語法 ( )

如需詳細資訊，請參閱 <xref:fundamentals/routing>。

## <a name="navlink-component"></a>導覽連結元件

建立瀏`NavLink`覽 連結時,使用元件代替`<a>`HTML 超連結元素 ( ) 。 元件`NavLink`行為類似於`<a>`元素,只不過它根據是否`active``href`與當前 URL 匹配來切換 CSS 類。 該`active`類可幫助使用者了解顯示的導航連結中哪個頁面是活動頁面。

以下`NavMenu`元件建立[Bootstrap](https://getbootstrap.com/docs/)導航列,示範如何使用`NavLink`元件:

[!code-razor[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

有兩`NavLinkMatch`個選項可以分配給`Match``<NavLink>`元素的屬性:

* `NavLinkMatch.All`&ndash;與`NavLink`整個當前網址匹配時處於活動狀態。
* `NavLinkMatch.Prefix`(*預設*)&ndash;與`NavLink`當前URL的任何首元匹配時處於活動狀態。

在前面的示例中,Home`NavLink``href=""`與主 URL 匹配,並且`active`僅在應用的預設基本路徑`https://localhost:5001/`URL(例如, ) 接收 CSS 類。 當使用者存`NavLink`取 具有`active`前置碼的任何網址時,`MyComponent`第二個 接收`https://localhost:5001/MyComponent`類別`https://localhost:5001/MyComponent/AnotherSegment`(例如, 與 。

其他`NavLink`元件屬性將傳遞到呈現的錨點標記。 在下面的範例中,`NavLink`元件包括以下`target`屬性:

```razor
<NavLink href="my-page" target="_blank">My page</NavLink>
```

顯示以下 HTML 標籤:

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>URI 與導覽狀態說明器

用於<xref:Microsoft.AspNetCore.Components.NavigationManager>在 C# 代碼中處理 URI 和導航。 `NavigationManager`提供了下表中顯示的事件和方法。

| member | 描述 |
| ------ | ----------- |
| Uri | 獲取當前絕對URI。 |
| BaseUri | 獲取基礎 URI(帶有尾隨斜杠),該URI可以預置到相對 URI 路徑以生成絕對 URI。 通常,`BaseUri`對應於`href``<base>`文檔 元素上的屬性(wwwroot/index.html(WebAssembly)*wwwroot/index.html*Blazor或*頁面/_Host.cshtml(*Blazor伺服器)。 |
| 導覽到 | 導航到指定的 URI。 如果是`forceLoad``true`:<ul><li>繞過用戶端路由。</li><li>無論URI通常由用戶端路由器處理,瀏覽器都被迫從伺服器載入新頁面。</li></ul> |
| 位置已變更 | 當導航位置更改時觸發的事件。 |
| 到絕對 | 將相對 URI 轉換為絕對 URI。 |
| <span style="word-break:normal;word-wrap:normal">到基礎相對路徑</span> | 給定基本 URI(例如,以前`GetBaseUri`由返回的 URI)將絕對 URI 轉換為相對於基本 URI 前綴的 URI。 |

選擇按鈕時,以下元件將導航到應用`Counter`的元件:

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

以下元件處理位置更改的事件。 當`HandleLocationChanged`框架調用該方法`Dispose`時 ,該方法將取消掛號。 取消掛接該方法允許對元件進行垃圾回收。

```razor
@implement IDisposable
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

<xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs>提供有關該事件的以下資訊:

* <xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.Location>&ndash;新位置的 URL。
* <xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.IsNavigationIntercepted>&ndash;`true`如果Blazor,從瀏覽器截獲了導航。 如果`false`,[導航管理器.導航到](xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A)導致導航發生。

有關元件處理的詳細資訊,請參閱<xref:blazor/lifecycle#component-disposal-with-idisposable>。
