---
title: ASP.NET Core 中的 Razor 頁面路由和應用程式慣例
author: guardrex
description: 探索路由和應用程式模型提供者慣例如何協助您控制頁面路由、探索與處理。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/07/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: c160d93e22fc5b3511ba4e5539cce8576346898b
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665536"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>ASP.NET Core 中的 Razor 頁面路由和應用程式慣例

作者：[Luke Latham](https://github.com/guardrex)

了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。

當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。

若要指定頁面路由、 新增路由區段，或將參數新增至路由，請使用頁面的`@page`指示詞。 如需詳細資訊，請參閱 <<c0> [ 自訂路由](xref:razor-pages/index#custom-routes)。

有不能作為路由區段或參數名稱的保留的字。 如需詳細資訊，請參閱[路由：保留的路由名稱](xref:fundamentals/routing#reserved-routing-names)。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

| 情節 | 範例會示範 ... |
| -------- | --------------------------- |
| [模型慣例](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | 將路由範本和標頭新增至應用程式的頁面。 |
| [頁面路由動作慣例](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | 將路由範本新增至資料夾中的頁面，以及新增至單一頁面。 |
| [頁面模型動作慣例](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</li></ul> | 將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。 |

Razor 頁面慣例會新增及設定<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>擴充方法<xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>中的服務集合上`Startup`類別。 稍後在本主題會說明下列慣例範例：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention("/About", model => { ... });
                options.Conventions.AddPageRoute("/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention("/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a>路由順序

路由會指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*>處理 （路由比對）。

| 順序            | 行為 |
| :--------------: | -------- |
| -1               | 在處理其他路由之前，會處理路由。 |
| 0                | 未指定順序 （預設值）。 未指派`Order`(`Order = null`) 預設路由`Order`設為 0 （零） 進行處理。 |
| 1、 2、 &hellip; n | 指定路由處理順序。 |

路由處理被藉由慣例：

* 路由會循序處理 (-1、 0、 1、 2、 &hellip; n)。
* 當路由具有相同`Order`、 最明確的路由會比對，第一次後面較不明確的路由。
* 當具有相同的路由`Order`相同的參數數目符合要求 URL，將它們加入的順序來處理路由<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>。

可能的話，請避免根據已建立的路由的處理順序而定。 一般而言，路由會選取正確的路由，透過 URL 比對。 如果您必須設定路由`Order`屬性來路由要求是否正確，應用程式的路由傳送，進而可能造成混淆的用戶端和容易維護。 若要簡化應用程式的路由傳送，進而搜尋。 範例應用程式需要的處理順序，來示範數個使用單一的應用程式的路由案例的外顯路由。 不過，您應該嘗試避免設定路由的練習`Order`在生產環境應用程式。

Razor Pages 路由和 MVC 控制器路由會共用實作。 在 MVC 主題中的路由順序的詳細資訊位於[路由至控制器動作：排序屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。

## <a name="model-conventions"></a>模型慣例

加入的委派<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>來新增[模型慣例](xref:mvc/controllers/application-model#conventions)，套用至 Razor Pages。

### <a name="add-a-route-model-convention-to-all-pages"></a>將路由模型慣例新增至所有頁面

使用<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>來建立並新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>的集合<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>頁面路由期間所套用的執行個體模型建構。

範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。 這可確保下列路由比對範例應用程式的行為：

* 路由範本`TheContactPage/{text?}`本主題稍後會加入。 Contact 頁面路由會將預設順序`null`(`Order = 0`)，因此它會比對之前`{globalTemplate?}`路由範本。
* `{aboutTemplate?}`路由範本新增本主題稍後。 `{aboutTemplate?}` 範本會指定 `Order` 為 `2`。 在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。
* `{otherPagesTemplate?}`路由範本新增本主題稍後。 `{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。 當任何頁面*頁面/OtherPages*資料夾要求的路由參數 (例如`/OtherPages/Page1/RouteDataValue`)，"因此 RouteDataValue"會載入`RouteData.Values["globalTemplate"]`(`Order = 1`) 而非`RouteData.Values["otherPagesTemplate"]`(`Order = 2`)由於設定`Order`屬性。

可能的話，不需要設定`Order`，這會導致`Order = 0`。 依賴路由來選取正確的路由。

Razor 頁面選項，例如新增<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>，會新增至服務集合中新增 MVC 時`Startup.ConfigureServices`。 如需範例，請參閱[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：

![使用路由區段 GlobalRouteValue 要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>將應用程式模型慣例新增至所有頁面

使用<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>來建立並新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>的集合<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>期間所套用的執行個體頁面上的應用程式模型建構。

為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。 類別建構函式接受 `name` 字串和 `values` 字串陣列。 在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。 完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。

範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>將處理常式模型慣例新增至所有頁面

使用<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>來建立並新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention>的集合<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>期間所套用的執行個體頁面處理常式模型建構。

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

*Startup.cs*：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a>頁面路由動作慣例

預設路由模型提供者會衍生自<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider>叫用慣例的設計來提供設定頁面路由的擴充點。

### <a name="folder-route-model-convention"></a>資料夾路由模型慣例

使用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>來建立並新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，在叫用動作<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>所有指定的資料夾下的頁面。

範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保樣板`{globalTemplate?}`(設定主題中稍早`1`) 的第一個路由資料值的位置，提供單一路由值時，會指定優先權。 如果中的頁面*頁/OtherPages*資料夾要求的路由參數值 (例如`/OtherPages/Page1/RouteDataValue`)，"因此 RouteDataValue"會載入`RouteData.Values["globalTemplate"]`(`Order = 1`) 而非`RouteData.Values["otherPagesTemplate"]`(`Order = 2`)由於設定`Order`屬性。

可能的話，不需要設定`Order`，這會導致`Order = 0`。 依賴路由來選取正確的路由。

在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>頁面路由模型慣例

使用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>來建立並新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，在叫用動作<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>頁面具有指定名稱。

範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保樣板`{globalTemplate?}`(設定主題中稍早`1`) 的第一個路由資料值的位置，提供單一路由值時，會指定優先權。 如果 [關於] 頁面會要求使用路由參數值在`/About/RouteDataValue`，"因此 RouteDataValue"會載入`RouteData.Values["globalTemplate"]`(`Order = 1`) 而非`RouteData.Values["aboutTemplate"]`(`Order = 2`) 由於設定`Order`屬性。

可能的話，不需要設定`Order`，這會導致`Order = 0`。 依賴路由來選取正確的路由。

在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>使用自訂頁面路由的參數轉換程式

您可以使用參數的轉換程式自訂由 ASP.NET Core 所產生的頁面路由。 參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。 例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。

`PageRouteTransformerConvention`頁面路由模型慣例會將參數轉換程式用於應用程式中自動產生的頁面路由的資料夾和檔案名稱區段。 在 Razor 頁面的檔案，例如 */Pages/SubscriptionManagement/ViewAll.cshtml*就必須改寫其路由`/SubscriptionManagement/ViewAll`至`/subscription-management/view-all`。

`PageRouteTransformerConvention` 只會轉換來自的 Razor Pages 資料夾和檔案名稱自動產生的區段的頁面路由。 它不會轉換與新增的路由區段`@page`指示詞。 慣例也不會轉換所新增的路由<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>。

`PageRouteTransformerConvention`登錄中的選項為`Startup.ConfigureServices`:

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

使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>設定頁面的路由，在指定的頁面路徑。 頁面的所產生連結會使用您指定的路由。 `AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。

範例應用程式為 *Contact.cshtml* 建立 `/TheContactPage` 的路由：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。

範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。 該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。 如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。 呈現的頁面會顯示 'text' 區段值。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>頁面模型動作慣例

預設頁面模型提供者會實作<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider>叫用慣例的設計來設定頁面模型提供擴充點。 在建置和修改頁面探索與處理案例時，這些慣例很有用。

在本節中的範例，範例應用程式會使用`AddHeaderAttribute`類別，這是<xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>，套用回應標頭：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。

**資料夾應用程式模型慣例**

使用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>來建立並新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，在叫用動作<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>指定的資料夾下的所有頁面的執行個體。

此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

**頁面應用程式模型慣例**

使用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>來建立並新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，在叫用動作<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>頁面具有指定名稱。

此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

**設定篩選條件**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> 設定要套用指定的篩選條件。 您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。 如果通過條件，則會新增標頭。 如果沒有，則會套用 `EmptyFilter`。

`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。 由於 Razor Pages 會忽略動作篩選條件，因此如果路徑未包含 `OtherPages/Page2`，則 `EmptyFilter` 不會如預期般生效。

在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

**設定篩選條件 Factory**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> 設定要套用指定的 factory[篩選器](xref:mvc/controllers/filters)至所有 Razor Pages。

範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC 篩選條件和頁面篩選條件 (IPageFilter)

Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。 其他類型的 MVC 篩選條件可供您使用：[授權](xref:mvc/controllers/filters#authorization-filters)，[例外狀況](xref:mvc/controllers/filters#exception-filters)，[資源](xref:mvc/controllers/filters#resource-filters)，和[結果](xref:mvc/controllers/filters#result-filters)。 如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。

頁面篩選條件 (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) 篩選會套用至 Razor 頁面。 如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
