---
title: 路由的 razor 元件
author: guardrex
description: 了解如何在應用程式及有關 NavLink 元件，將要求路由傳送。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/routing
ms.openlocfilehash: ef82fa7e0d571979a43fd8ce712bf4f22c06f9a7
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515427"
---
# <a name="razor-components-routing"></a>路由的 razor 元件

作者：[Luke Latham](https://github.com/guardrex)

了解如何在應用程式及有關 NavLink 元件，將要求路由傳送。

## <a name="aspnet-core-endpoint-routing-integration"></a>ASP.NET Core 端點路由整合

Razor 元件都已整合至[ASP.NET Core 路由](xref:fundamentals/routing)。 將 ASP.NET Core 應用程式設定為接受連入連線使用的互動式 Razor 元件`MapComponentHub<TComponent>`在`Startup.Configure`。 `MapComponentHub` 指定的根元件`App`應該比對的選取器的 DOM 項目中呈現`app`:

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapComponentHub<App>("app");
});
```

## <a name="route-templates"></a>路由範本

`<Router>`元件可讓路由和路由範本會提供給每個可存取的元件。 `<Router>`元件會出現在*Components/App.razor*檔案：

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

當 *.razor*或 *.cshtml*檔案`@page`編譯指示詞，指定產生的類別<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>指定路由範本。 在執行階段，路由器會尋找使用的元件類別`RouteAttribute`並呈現任何元件有符合所要求的 URL 的路由範本。

多個路由範本可以套用至元件。 下列元件會回應要求`/BlazorRoute`和`/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

`<Router>` 支援的設定，轉譯所要求的路由時的後援元件仍未解析的。 藉由設定啟用此加入案例`FallbackComponent`後援的元件類別的型別參數。

下列範例會設定中所定義的元件*Pages/MyFallbackRazorComponent.cshtml*做為後援的元件，如`<Router>`:

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> 若要正確地產生路由，應用程式必須包含`<base>`標記中的其*wwwroot/index.html*檔案中指定的應用程式基底路徑`href`屬性 (`<base href="/">`)。 如需詳細資訊，請參閱<xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>。

## <a name="route-parameters"></a>路由參數

路由器會使用路由參數，來填入對應的元件參數具有相同的名稱 （不區分大小寫）：

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter&highlight=2,7-8)]

選擇性參數不支援，因此兩個`@page`指示詞會套用在上述範例中。 第一個允許瀏覽至不含參數的元件。 第二個`@page`指示詞會採用`{text}`路由參數，並將值指派給`Text`屬性。

## <a name="route-constraints"></a>路由條件約束

路由條件約束會強制執行比對到元件的路由區段的型別。

在下列範例中，使用者元件的路由才會相符：

* `Id`路由區段會出現在要求 URL。
* `Id`區段是一個整數 (`int`)。

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.cshtml?highlight=1)]

使用下表所示的路由條件約束。 符合文化特性而異的路由條件約束，請參閱下方的資料表，如需詳細資訊的警告。

| 條件約束 | 範例           | 範例相符項目                                                                  | 非變異值<br>文化特性<br>比對 |
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

## <a name="navlink-component"></a>NavLink 元件

使用 NavLink 元件取代 HTML`<a>`時建立導覽連結的項目。 NavLink 元件的行為類似`<a>`項目，但它會切換`active`CSS 類別根據其`href`符合目前的 URL。 `active`類別可協助使用者了解哪一頁是使用中的頁面之間導覽連結顯示。

下列 NavMenu 元件會建立[Bootstrap](https://getbootstrap.com/docs/)導覽列中，示範如何使用 NavLink 元件：

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?name=snippet_NavLinks&highlight=4-6,9-11)]

有兩個`NavLinkMatch`選項：

* `NavLinkMatch.All` &ndash; 指定 NavLink 應該是作用中時，它會比對整個目前的 URL。
* `NavLinkMatch.Prefix` &ndash; 指定 NavLink 應該作用中，當它符合目前 URL 之任何前置詞。

在上述範例中，首頁 NavLink (`href=""`) 比對所有的 Url，並一律會收到`active`CSS 類別。 只會收到第二個 NavLink`active`當使用者造訪 Blazor 路由元件類別 (`href="BlazorRoute"`)。
