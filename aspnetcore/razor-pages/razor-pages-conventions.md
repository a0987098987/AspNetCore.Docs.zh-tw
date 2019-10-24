---
title: ASP.NET Core 中的 Razor 頁面路由和應用程式慣例
author: guardrex
description: 探索路由和應用程式模型提供者慣例如何協助您控制頁面路由、探索與處理。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/22/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: a0a1eda69da34d1865fd11ef464c3697bcd01eff
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779210"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>ASP.NET Core 中的 Razor 頁面路由和應用程式慣例

作者：[Luke Latham](https://github.com/guardrex)

了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。

當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。

若要指定頁面路由、加入路由區段，或將參數加入至路由，請使用頁面的 `@page` 指示詞。 如需詳細資訊，請參閱[自訂路由](xref:razor-pages/index#custom-routes)。

有保留字無法做為路由區段或參數名稱使用。 如需詳細資訊，請參閱[路由：保留的路由名稱](xref:fundamentals/routing#reserved-routing-names)。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

| 情節 | 範例會示範 ... |
| -------- | --------------------------- |
| [模型慣例](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | 將路由範本和標頭新增至應用程式的頁面。 |
| [頁面路由動作慣例](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | 將路由範本新增至資料夾中的頁面，以及新增至單一頁面。 |
| [頁面模型動作慣例](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</li></ul> | 將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。 |

Razor Pages 慣例會使用 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> 擴充方法來新增和設定，以在 `Startup` 類別的服務集合上 <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>。 稍後在本主題會說明下列慣例範例：

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

## <a name="route-order"></a>路由順序

路由會指定處理的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> （路由對應）。

| 順序            | 行為 |
| :--------------: | -------- |
| -1               | 在處理其他路由之前，會先處理路由。 |
| 0                | 未指定順序（預設值）。 未指派 `Order` （`Order = null`）預設會將路由 `Order` 為0（零）以進行處理。 |
| 1、2、&hellip; n | 指定路由處理順序。 |

路由處理是依照慣例所建立：

* 路由會依序處理（-1、0、1、2、&hellip; n）。
* 當路由具有相同的 `Order` 時，最特定的路由會先進行比對，後面接著較少的特定路由。
* 當具有相同 `Order` 且參數數目相同的路由符合要求 URL 時，會依將路由新增至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection> 的順序來進行處理。

可能的話，請根據已建立的路由處理順序來避免。 一般而言，路由會選取 URL 相符的正確路由。 如果您必須將路由 `Order` 屬性設定為正確路由要求，則應用程式的路由配置可能會使用戶端變得困惑，而難以維護。 搜尋以簡化應用程式的路由配置。 範例應用程式需要明確的路由處理順序，才能使用單一應用程式來示範數個路由案例。 不過，您應該嘗試避免在生產應用程式中設定路由 `Order` 的做法。

Razor Pages 路由和 MVC 控制器路由會共用實作。 如需有關 MVC 主題中路由順序的資訊，請前往[路由至控制器動作：排序屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。

## <a name="model-conventions"></a>模型慣例

新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 的委派，以加入適用于 Razor Pages 的[模型慣例](xref:mvc/controllers/application-model#conventions)。

### <a name="add-a-route-model-convention-to-all-pages"></a>將路由模型慣例新增至所有頁面

使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，並將其加入至頁面路由模型結構期間所套用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例的集合。

範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。 這可確保範例應用程式中的下列路由符合行為：

* 本主題稍後會新增 `TheContactPage/{text?}` 的路由範本。 連絡人頁面路由的預設順序是 `null` （`Order = 0`），因此它會符合 `{globalTemplate?}` 的路由範本。
* 本主題稍後會新增 `{aboutTemplate?}` 的路由範本。 `{aboutTemplate?}` 範本會指定 `Order` 為 `2`。 在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。
* 本主題稍後會新增 `{otherPagesTemplate?}` 的路由範本。 `{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。 當使用路由參數要求*Pages/OtherPages*資料夾中的任何頁面時（例如 `/OtherPages/Page1/RouteDataValue`），會將 "因此 routedatavalue" 載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["otherPagesTemplate"]` （`Order = 2`），因為設定了 `Order` 屬性。

盡可能不要設定 `Order`，這會導致 `Order = 0`。 依賴路由來選取正確的路由。

當 MVC 新增至 `Startup.ConfigureServices` 中的服務集合時，會加入 Razor Pages 選項，例如新增 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>。 如需範例，請參閱[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：

![使用路由區段 GlobalRouteValue 要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>將應用程式模型慣例新增至所有頁面

使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，並將其新增至頁面應用程式模型結構期間所套用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例的集合。

為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。 類別建構函式接受 `name` 字串和 `values` 字串陣列。 在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。 完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。

範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

*Startup.cs*：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>將處理常式模型慣例新增至所有頁面

使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention>，並將其新增至在頁面處理常式模型結構中套用的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例集合。

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

*Startup.cs*：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

## <a name="page-route-action-conventions"></a>頁面路由動作慣例

衍生自 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> 的預設路由模型提供者會叫用慣例，其設計目的是為了提供設定頁面路由的擴充點。

### <a name="folder-route-model-convention"></a>資料夾路由模型慣例

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> 來建立和新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，以針對指定資料夾下的所有頁面，在 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 上叫用動作。

範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保當提供單一路由值時，第一個路由資料值位置的 `{globalTemplate?}` （在主題中稍早設定 `1`）的範本會獲得優先權。 如果*Pages/OtherPages*資料夾中的頁面是以路由參數值（例如 `/OtherPages/Page1/RouteDataValue`）要求，則 "因此 routedatavalue" 會載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["otherPagesTemplate"]` （`Order = 2`）中，因為設定了 `Order` 屬性。

盡可能不要設定 `Order`，這會導致 `Order = 0`。 依賴路由來選取正確的路由。

在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>頁面路由模型慣例

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> 建立和加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，以在具有指定名稱的頁面上，叫用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 上的動作。

範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保當提供單一路由值時，第一個路由資料值位置的 `{globalTemplate?}` （在主題中稍早設定 `1`）的範本會獲得優先權。 如果在 `/About/RouteDataValue` 使用路由參數值要求 [關於] 頁面，則會將 "因此 routedatavalue" 載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["aboutTemplate"]` （`Order = 2`），因為設定了 `Order` 屬性。

盡可能不要設定 `Order`，這會導致 `Order = 0`。 依賴路由來選取正確的路由。

在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>使用參數轉換器來自訂頁面路由

ASP.NET Core 所產生的頁面路由可使用參數轉換器進行自訂。 參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。 例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。

@No__t_0 頁面路由模型慣例會將參數轉換器套用至應用程式中自動產生之頁面路由的資料夾和檔案名區段。 例如，位於 */Pages/SubscriptionManagement/ViewAll.cshtml*的 Razor Pages 檔案會將其路由從 `/SubscriptionManagement/ViewAll` 重寫至 `/subscription-management/view-all`。

`PageRouteTransformerConvention` 只會轉換來自 Razor Pages 資料夾和檔案名的頁面路由自動產生的區段。 它不會轉換使用 `@page` 指示詞新增的路由區段。 此慣例也不會轉換 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> 所新增的路由。

@No__t_0 會註冊為 `Startup.ConfigureServices` 中的選項：

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
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

::: moniker range="= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
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

## <a name="configure-a-page-route"></a>設定頁面路由

使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> 在指定的頁面路徑上設定頁面的路由。 頁面的所產生連結會使用您指定的路由。 `AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。

範例應用程式為 *Contact.cshtml* 建立 `/TheContactPage` 的路由：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。

範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。 該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：

::: moniker range=">= aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。 如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。 呈現的頁面會顯示 'text' 區段值。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>頁面模型動作慣例

實 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> 的預設頁面模型提供者會叫用慣例，其設計目的是為了提供設定頁面模型的擴充點。 在建置和修改頁面探索與處理案例時，這些慣例很有用。

在本節的範例中，範例應用程式會使用 `AddHeaderAttribute` 類別，也就是會套用回應標頭的 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。

**資料夾應用程式模型慣例**

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> 來建立和新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，以針對指定資料夾下的所有頁面，在 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 實例上叫用動作。

此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

**頁面應用程式模型慣例**

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> 建立和加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，以在具有指定名稱的頁面上，叫用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 上的動作。

此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

**設定篩選條件**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> 將指定的篩選準則設定為 [套用]。 您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。 如果通過條件，則會新增標頭。 如果沒有，則會套用 `EmptyFilter`。

`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。 由於 Razor Pages 會忽略動作篩選準則，因此如果路徑未包含 `OtherPages/Page2`，`EmptyFilter` 就不會有任何作用。

在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

**設定篩選條件 Factory**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> 設定指定的 factory，將[篩選](xref:mvc/controllers/filters)套用至所有 Razor Pages。

範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

*AddHeaderWithFactory.cs*：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC 篩選條件和頁面篩選條件 (IPageFilter)

Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。 其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。 如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。

頁面篩選準則（<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>）是適用于 Razor Pages 的篩選準則。 如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
