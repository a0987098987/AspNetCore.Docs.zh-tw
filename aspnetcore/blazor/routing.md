---
title: ASP.NET Core Blazor 路由
author: guardrex
description: 瞭解如何在應用程式中路由傳送要求，以及關於 NavLink 元件。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/routing
ms.openlocfilehash: 76266aedd4655161f1f50a8beb0936660d452912
ms.sourcegitcommit: 6d26ab647ede4f8e57465e29b03be5cb130fc872
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/07/2019
ms.locfileid: "71999823"
---
# <a name="aspnet-core-blazor-routing"></a>ASP.NET Core Blazor 路由

作者：[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

瞭解如何路由傳送要求，以及如何使用 @no__t 0 元件，在 Blazor apps 中建立導覽連結。

## <a name="aspnet-core-endpoint-routing-integration"></a>ASP.NET Core 端點路由整合

Blazor 伺服器已整合到[ASP.NET Core 端點路由](xref:fundamentals/routing)中。 ASP.NET Core 應用程式會設定為在 `Startup.Configure` 中接受具有 `MapBlazorHub` 之互動式元件的連入連線：

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

最常見的設定是將所有要求路由傳送至 Razor 頁面，做為 Blazor 伺服器應用程式的伺服器端部分的主機。 依照慣例，*主機*頁面通常會命名為 *_Host*。 主機檔案中指定的路由稱為「回溯*路由*」，因為它在路由比對中以低優先順序運作。 當其他路由不相符時，就會考慮回退路由。 這可讓應用程式使用其他控制器和頁面，而不會干擾 Blazor 伺服器應用程式。

## <a name="route-templates"></a>路由範本

@No__t 0 元件可讓您使用指定的路由來路由傳送至每個元件。 @No__t-0 元件會出現在*應用程式 razor*檔案中：

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

編譯具有 `@page` 指示詞的*razor*檔案時，會提供所產生的類別 <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> 指定路由範本。

在執行時間，@no__t 0 元件：

* 接收來自 `Router` 的 `RouteData` 以及任何想要的參數。
* 使用指定的參數，以配置（或選擇性的預設版面配置）呈現指定的元件。

您可以選擇性地指定具有版面配置類別的 `DefaultLayout` 參數，以用於未指定版面配置的元件。 預設的 Blazor 範本會指定 `MainLayout` 元件。 *MainLayout*是在範本專案的*共用*資料夾中。 如需版面配置的詳細資訊，請參閱 <xref:blazor/layouts>。

多個路由範本可以套用至元件。 下列元件會回應 `/BlazorRoute` 和 `/DifferentBlazorRoute` 的要求：

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> 若要讓 Url 正確解析，應用程式必須在其*wwwroot/index.html*檔案（Blazor WebAssembly）或*Pages/_Host. Cshtml*檔案（Blazor 伺服器）中包含 `href` 屬性（`<base href="/">`）中指定之應用程式基底路徑的 `<base>` 標記。 如需詳細資訊，請參閱<xref:host-and-deploy/blazor/index#app-base-path>。

## <a name="provide-custom-content-when-content-isnt-found"></a>在找不到內容時提供自訂內容

如果找不到所要求路由的內容，@no__t 0 元件可讓應用程式指定自訂內容。

在*應用程式 razor*檔案中，于 `Router` 元件的 `NotFound` 範本參數中設定自訂內容：

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

@No__t-0 標記的內容可以包含任意專案，例如其他互動式元件。 若要將預設版面配置套用至 `NotFound` 內容，請參閱 <xref:blazor/layouts>。

## <a name="route-to-components-from-multiple-assemblies"></a>從多個元件路由至元件

使用 `AdditionalAssemblies` 參數來指定搜尋可路由的元件時，要考慮的 `Router` 元件的其他元件。 除了 @no__t 的 4.9.0-指定元件以外，還會考慮指定的元件。 在下列範例中，`Component1` 是在參考的類別庫中定義的可路由元件。 下列 `AdditionalAssemblies` 範例會導致 `Component1` 的路由支援：

< 路由器 AppAssembly = "typeof （程式）。元件 "AdditionalAssemblies =" new [] {typeof （Component1）。Assembly} > ...</Router>

## <a name="route-parameters"></a>路由參數

路由器會使用路由參數來填入具有相同名稱的對應元件參數（不區分大小寫）：

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

ASP.NET Core 3.0 中的 Blazor 應用程式不支援選擇性參數。 上一個範例中會套用兩個 @no__t 0 的指示詞。 第一個則允許不使用參數導覽至元件。 第二個 `@page` 指示詞會採用 `{text}` 路由參數，並將值指派給 `Text` 屬性。

## <a name="route-constraints"></a>路由條件約束

路由條件約束會將路由區段上的類型比對強制套用至元件。

在下列範例中，`Users` 元件的路由只會符合下列條件：

* 要求 URL 上有 @no__t 0 的路由區段。
* @No__t-0 區段是一個整數（`int`）。

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

在 Blazor 伺服器應用程式中， *_Host*中的預設路由是 `/` （`@page "/"`）。 包含點（`.`）的要求 URL 不符合預設路由，因為 URL 會顯示要求檔案。 如果靜態檔案不存在，Blazor 應用程式會傳回*404-找不*到的回應。 若要使用包含點的路由，請使用下列路由範本來設定 *_Host* ：

```cshtml
@page "/{**path}"
```

@No__t-0 範本包含：

* 雙星號的*catch-all*語法（`**`），可跨多個資料夾界限捕捉路徑，而不需要編碼正斜線（`/`）。
* @No__t-0 路由參數名稱。

如需詳細資訊，請參閱<xref:fundamentals/routing>。

## <a name="navlink-component"></a>NavLink 元件

建立導覽連結時，請使用 `NavLink` 元件來取代 HTML 超連結元素（`<a>`）。 @No__t 0 元件的行為就像 @no__t 1 元素，但它會根據其 `href` 是否符合目前的 URL 來切換 @no__t 2 CSS 類別。 @No__t 0 類別可協助使用者瞭解在顯示的導覽連結中，哪個頁面是使用中的頁面。

下列 @no__t 0 元件會建立[啟動](https://getbootstrap.com/docs/)程式導覽列，示範如何使用 `NavLink` 元件：

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

有兩個 @no__t 0 選項可供您指派給 `<NavLink>` 元素的 `Match` 屬性：

* `NavLinkMatch.All` &ndash; 當它符合整個目前的 URL 時，`NavLink` 為作用中。
* `NavLinkMatch.Prefix` （*預設值*） &ndash; 當它符合目前 URL 的任何前置詞時，`NavLink` 為作用中。

在上述範例中，Home `NavLink` `href=""` 符合 home URL，而且只會在應用程式的預設基底路徑 URL （例如，`https://localhost:5001/`）接收 @no__t 2 CSS 類別。 第二個 `NavLink` 會在使用者造訪具有 @no__t 2 前置詞（例如，`https://localhost:5001/MyComponent` 和 `https://localhost:5001/MyComponent/AnotherSegment`）的任何 URL 時收到 @no__t 1 類別。

額外的 `NavLink` 元件屬性會傳遞至呈現的錨點標記。 在下列範例中，`NavLink` 元件包含 `target` 屬性：

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

會轉譯下列 HTML 標籤：

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>URI 和流覽狀態協助程式

使用 `Microsoft.AspNetCore.Components.NavigationManager` 來處理常式代碼中C#的 uri 和導覽。 `NavigationManager` 提供下表所示的事件和方法。

| 成員 | 描述 |
| ------ | ----------- |
| `Uri` | 取得目前的絕對 URI。 |
| `BaseUri` | 取得可在相對 URI 路徑前面加上的基底 URI （含尾端斜線），以產生絕對 URI。 一般來說，`BaseUri` 會對應至*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. Cshtml* （Blazor Server）中檔的 @no__t 2 元素上的 @no__t 1 屬性。 |
| `NavigateTo` | 導覽至指定的 URI。 如果 `forceLoad` 是 `true`：<ul><li>已略過用戶端路由。</li><li>瀏覽器會強制從伺服器載入新頁面，無論 URI 是否通常由用戶端路由器處理。</li></ul> |
| `LocationChanged` | 導覽位置變更時引發的事件。 |
| `ToAbsoluteUri` | 將相對 URI 轉換為絕對 URI。 |
| `ToBaseRelativePath` | 假設基底 URI （例如先前由 `GetBaseUri` 所傳回的 URI），會將絕對 URI 轉換成相對於基底 URI 前置詞的 URI。 |

下列元件會在選取按鈕時，流覽至應用程式的 @no__t 0 元件：

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
