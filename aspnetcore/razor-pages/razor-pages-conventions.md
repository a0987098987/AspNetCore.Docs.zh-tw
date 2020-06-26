---
title: RazorASP.NET Core 中的頁面路由和應用程式慣例
author: rick-anderson
description: 探索路由和應用程式模型提供者慣例如何協助您控制頁面路由、探索與處理。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: 308ca4401289a55e5dba8d61de50644cb2a53433
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85405246"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>RazorASP.NET Core 中的頁面路由和應用程式慣例

::: moniker range=">= aspnetcore-3.0"

瞭解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，在 Razor 頁面應用程式中控制頁面路由、探索和處理。

當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。

若要指定頁面路由、加入路由區段，或將參數加入至路由，請使用頁面的指示詞 `@page` 。 如需詳細資訊，請參閱[自訂路由](xref:razor-pages/index#custom-routes)。

有保留字無法做為路由區段或參數名稱使用。 如需詳細資訊，請參閱[路由：保留的路由名稱](xref:mvc/controllers/routing#reserved-routing-names)。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)（[如何下載](xref:index#how-to-download-a-sample)）

| 狀況 | 範例會示範 ... |
| -------- | --------------------------- |
| [模型慣例](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | 將路由範本和標頭新增至應用程式的頁面。 |
| [頁面路由動作慣例](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | 將路由範本新增至資料夾中的頁面，以及新增至單一頁面。 |
| [頁面模型動作慣例](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</li></ul> | 將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。 |

Razor在 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> 類別中的服務集合上，會使用擴充方法來新增和設定頁面慣例 `Startup` 。 稍後在本主題會說明下列慣例範例：

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

## <a name="route-order"></a>路由順序

路由會指定 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 進行處理（路由對應）。

| 單            | 行為 |
| :--------------: | -------- |
| -1               | 在處理其他路由之前，會先處理路由。 |
| 0                | 未指定順序（預設值）。 Not 指派 `Order` （ `Order = null` ）預設會將路由 `Order` 設為0（零）以進行處理。 |
| 1、2、 &hellip; n | 指定路由處理順序。 |

路由處理是依照慣例所建立：

* 路由會依序處理（-1、0、1、2、 &hellip; n）。
* 當路由具有相同的時 `Order` ，最特定的路由會先進行比對，後面接著較少的特定路由。
* 當具有相同 `Order` 和相同數目之參數的路由符合要求 URL 時，路由會依其加入至的順序進行處理 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection> 。

可能的話，請根據已建立的路由處理順序來避免。 一般而言，路由會選取 URL 相符的正確路由。 如果您必須設定路由 `Order` 屬性以正確地路由要求，則應用程式的路由配置可能會使用戶端變得困惑，而難以維護。 搜尋以簡化應用程式的路由配置。 範例應用程式需要明確的路由處理順序，才能使用單一應用程式來示範數個路由案例。 不過，您應該嘗試避免在 `Order` 生產應用程式中設定路由的做法。

Razor頁面路由和 MVC 控制器路由會共用一個執行。 如需有關 MVC 主題中路由順序的資訊，請前往[路由至控制器動作：排序屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。

## <a name="model-conventions"></a>模型慣例

新增的委派， <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 以加入適用于頁面的[模型慣例](xref:mvc/controllers/application-model#conventions) Razor 。

### <a name="add-a-route-model-convention-to-all-pages"></a>將路由模型慣例新增至所有頁面

使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> 至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面路由模型結構期間所套用的實例集合。

範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。 這可確保範例應用程式中的下列路由符合行為：

* `TheContactPage/{text?}`稍後會在主題中新增的路由範本。 連絡人頁面路由的預設順序為 `null` （ `Order = 0` ），因此它會符合 `{globalTemplate?}` 路由範本。
* `{aboutTemplate?}`稍後會在主題中新增路由範本。 `{aboutTemplate?}` 範本會指定 `Order` 為 `2`。 在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。
* `{otherPagesTemplate?}`稍後會在主題中新增路由範本。 `{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。 當使用路由參數要求*Pages/OtherPages*資料夾中的任何頁面時（例如， `/OtherPages/Page1/RouteDataValue` ），會將 "因此 routedatavalue" 載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["otherPagesTemplate"]` （）， `Order = 2` 因為設定了 `Order` 屬性。

盡可能不要設定 `Order` ，這會導致 `Order = 0` 。 依賴路由來選取正確的路由。

Razor<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>當 MVC 加入至中的服務集合時，會加入 [頁面] 選項，例如 [加入] `Startup.ConfigureServices` 。 如需範例，請參閱[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：

![使用路由區段 GlobalRouteValue 要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>將應用程式模型慣例新增至所有頁面

使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> 其新增至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面應用程式模型結構期間所套用的實例集合。

為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。 類別建構函式接受 `name` 字串和 `values` 字串陣列。 在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。 完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。

範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>將處理常式模型慣例新增至所有頁面

使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> 至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面處理常式模型結構中所套用的實例集合。

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

*Startup.cs*：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a>頁面路由動作慣例

衍生自的預設路由模型提供者 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> 會叫用慣例，其設計目的是為了提供設定頁面路由的擴充點。

### <a name="folder-route-model-convention"></a>資料夾路由模型慣例

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 針對指定資料夾下的所有頁面，叫用上的動作。

範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保 `{globalTemplate?}` 當提供單一路由值時，的範本（在主題中稍早設定為 `1` ）會獲得第一個路由資料值位置的優先權。 如果使用路由參數值要求*Pages/OtherPages*資料夾中的頁面（例如 `/OtherPages/Page1/RouteDataValue` ），則會將 "因此 routedatavalue" 載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["otherPagesTemplate"]` （）， `Order = 2` 因為設定了 `Order` 屬性。

盡可能不要設定 `Order` ，這會導致 `Order = 0` 。 依賴路由來選取正確的路由。

在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>頁面路由模型慣例

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 針對具有指定名稱的頁面，在上叫用動作。

範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保 `{globalTemplate?}` 當提供單一路由值時，的範本（在主題中稍早設定為 `1` ）會獲得第一個路由資料值位置的優先權。 如果要求的是「關於」頁面，其中的路由參數值為 `/About/RouteDataValue` ，則 "因此 routedatavalue" 會載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["aboutTemplate"]` （ `Order = 2` ），因為設定了 `Order` 屬性。

盡可能不要設定 `Order` ，這會導致 `Order = 0` 。 依賴路由來選取正確的路由。

在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>使用參數轉換器來自訂頁面路由

ASP.NET Core 所產生的頁面路由可使用參數轉換器進行自訂。 參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。 例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。

`PageRouteTransformerConvention`頁面路由模型慣例會將參數轉換器套用至應用程式中自動產生之頁面路由的資料夾和檔案名區段。 例如， Razor 位於 */Pages/SubscriptionManagement/ViewAll.cshtml*的分頁檔案會將其路由從重新寫入 `/SubscriptionManagement/ViewAll` 至 `/subscription-management/view-all` 。

`PageRouteTransformerConvention`只會轉換來自 Razor Pages 資料夾和檔案名的頁面路由自動產生的區段。 它不會轉換以指示詞新增的路由區段 `@page` 。 慣例也不會轉換所加入的路由 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> 。

`PageRouteTransformerConvention`會註冊為中的選項 `Startup.ConfigureServices` ：

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
```

[!code-csharp[](~/mvc/controllers/routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet2)]

[!INCLUDE[](~/includes/regex.md)]

## <a name="configure-a-page-route"></a>設定頁面路由

使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> ，在指定的頁面路徑上設定頁面的路由。 頁面的所產生連結會使用您指定的路由。 `AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。

範例應用程式為 *Contact.cshtml* 建立 `/TheContactPage` 的路由：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。

範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。 該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。 如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。 呈現的頁面會顯示 'text' 區段值。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>頁面模型動作慣例

執行的預設頁面模型提供者 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> 會叫用慣例，其設計目的是為了提供設定頁面模型的擴充點。 在建置和修改頁面探索與處理案例時，這些慣例很有用。

針對本節中的範例，範例應用程式 `AddHeaderAttribute` 會使用類別，也就是 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute> ，它會套用回應標頭：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。

**資料夾應用程式模型慣例**

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> 建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 針對指定資料夾下的所有頁面，在實例上叫用動作。

此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

**頁面應用程式模型慣例**

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 針對具有指定名稱的頁面，在上叫用動作。

此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

**設定篩選條件**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>設定要套用的指定篩選。 您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。 如果通過條件，則會新增標頭。 如果沒有，則會套用 `EmptyFilter`。

`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。 由於頁面會忽略動作篩選準則 Razor ， `EmptyFilter` 因此如果路徑不包含，就不會有任何作用 `OtherPages/Page2` 。

在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

**設定篩選條件 Factory**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>設定指定的 factory，將[篩選](xref:mvc/controllers/filters)套用至所有 Razor 頁面。

範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC 篩選條件和頁面篩選條件 (IPageFilter)

頁面會忽略 MVC[動作篩選準則](xref:mvc/controllers/filters#action-filters) Razor ，因為 Razor 頁面會使用處理程式方法。 其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。 如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。

頁面篩選準則（ <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ）是適用于頁面的篩選 Razor 。 如需詳細資訊，請參閱[ Razor 頁面的篩選方法](xref:razor-pages/filter)。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

瞭解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，在 Razor 頁面應用程式中控制頁面路由、探索和處理。

當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。

若要指定頁面路由、加入路由區段，或將參數加入至路由，請使用頁面的指示詞 `@page` 。 如需詳細資訊，請參閱[自訂路由](xref:razor-pages/index#custom-routes)。

有保留字無法做為路由區段或參數名稱使用。 如需詳細資訊，請參閱[路由：保留的路由名稱](xref:fundamentals/routing#reserved-routing-names)。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)（[如何下載](xref:index#how-to-download-a-sample)）

| 狀況 | 範例會示範 ... |
| -------- | --------------------------- |
| [模型慣例](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | 將路由範本和標頭新增至應用程式的頁面。 |
| [頁面路由動作慣例](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | 將路由範本新增至資料夾中的頁面，以及新增至單一頁面。 |
| [頁面模型動作慣例](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</li></ul> | 將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。 |

Razor在 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> 類別中的服務集合上，會使用擴充方法來新增和設定頁面慣例 `Startup` 。 稍後在本主題會說明下列慣例範例：

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

## <a name="route-order"></a>路由順序

路由會指定 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 進行處理（路由對應）。

| 單            | 行為 |
| :--------------: | -------- |
| -1               | 在處理其他路由之前，會先處理路由。 |
| 0                | 未指定順序（預設值）。 Not 指派 `Order` （ `Order = null` ）預設會將路由 `Order` 設為0（零）以進行處理。 |
| 1、2、 &hellip; n | 指定路由處理順序。 |

路由處理是依照慣例所建立：

* 路由會依序處理（-1、0、1、2、 &hellip; n）。
* 當路由具有相同的時 `Order` ，最特定的路由會先進行比對，後面接著較少的特定路由。
* 當具有相同 `Order` 和相同數目之參數的路由符合要求 URL 時，路由會依其加入至的順序進行處理 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection> 。

可能的話，請根據已建立的路由處理順序來避免。 一般而言，路由會選取 URL 相符的正確路由。 如果您必須設定路由 `Order` 屬性以正確地路由要求，則應用程式的路由配置可能會使用戶端變得困惑，而難以維護。 搜尋以簡化應用程式的路由配置。 範例應用程式需要明確的路由處理順序，才能使用單一應用程式來示範數個路由案例。 不過，您應該嘗試避免在 `Order` 生產應用程式中設定路由的做法。

Razor頁面路由和 MVC 控制器路由會共用一個執行。 如需有關 MVC 主題中路由順序的資訊，請前往[路由至控制器動作：排序屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。

## <a name="model-conventions"></a>模型慣例

新增的委派， <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 以加入適用于頁面的[模型慣例](xref:mvc/controllers/application-model#conventions) Razor 。

### <a name="add-a-route-model-convention-to-all-pages"></a>將路由模型慣例新增至所有頁面

使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> 至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面路由模型結構期間所套用的實例集合。

範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。 這可確保範例應用程式中的下列路由符合行為：

* `TheContactPage/{text?}`稍後會在主題中新增的路由範本。 連絡人頁面路由的預設順序為 `null` （ `Order = 0` ），因此它會符合 `{globalTemplate?}` 路由範本。
* `{aboutTemplate?}`稍後會在主題中新增路由範本。 `{aboutTemplate?}` 範本會指定 `Order` 為 `2`。 在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。
* `{otherPagesTemplate?}`稍後會在主題中新增路由範本。 `{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。 當使用路由參數要求*Pages/OtherPages*資料夾中的任何頁面時（例如， `/OtherPages/Page1/RouteDataValue` ），會將 "因此 routedatavalue" 載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["otherPagesTemplate"]` （）， `Order = 2` 因為設定了 `Order` 屬性。

盡可能不要設定 `Order` ，這會導致 `Order = 0` 。 依賴路由來選取正確的路由。

Razor<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>當 MVC 加入至中的服務集合時，會加入 [頁面] 選項，例如 [加入] `Startup.ConfigureServices` 。 如需範例，請參閱[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：

![使用路由區段 GlobalRouteValue 要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>將應用程式模型慣例新增至所有頁面

使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> 其新增至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面應用程式模型結構期間所套用的實例集合。

為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。 類別建構函式接受 `name` 字串和 `values` 字串陣列。 在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。 完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。

範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>將處理常式模型慣例新增至所有頁面

使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> 至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面處理常式模型結構中所套用的實例集合。

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

*Startup.cs*：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a>頁面路由動作慣例

衍生自的預設路由模型提供者 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> 會叫用慣例，其設計目的是為了提供設定頁面路由的擴充點。

### <a name="folder-route-model-convention"></a>資料夾路由模型慣例

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 針對指定資料夾下的所有頁面，叫用上的動作。

範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保 `{globalTemplate?}` 當提供單一路由值時，的範本（在主題中稍早設定為 `1` ）會獲得第一個路由資料值位置的優先權。 如果使用路由參數值要求*Pages/OtherPages*資料夾中的頁面（例如 `/OtherPages/Page1/RouteDataValue` ），則會將 "因此 routedatavalue" 載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["otherPagesTemplate"]` （）， `Order = 2` 因為設定了 `Order` 屬性。

盡可能不要設定 `Order` ，這會導致 `Order = 0` 。 依賴路由來選取正確的路由。

在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>頁面路由模型慣例

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 針對具有指定名稱的頁面，在上叫用動作。

範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保 `{globalTemplate?}` 當提供單一路由值時，的範本（在主題中稍早設定為 `1` ）會獲得第一個路由資料值位置的優先權。 如果要求的是「關於」頁面，其中的路由參數值為 `/About/RouteDataValue` ，則 "因此 routedatavalue" 會載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["aboutTemplate"]` （ `Order = 2` ），因為設定了 `Order` 屬性。

盡可能不要設定 `Order` ，這會導致 `Order = 0` 。 依賴路由來選取正確的路由。

在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>使用參數轉換器來自訂頁面路由

ASP.NET Core 所產生的頁面路由可使用參數轉換器進行自訂。 參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。 例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。

`PageRouteTransformerConvention`頁面路由模型慣例會將參數轉換器套用至應用程式中自動產生之頁面路由的資料夾和檔案名區段。 例如， Razor 位於 */Pages/SubscriptionManagement/ViewAll.cshtml*的分頁檔案會將其路由從重新寫入 `/SubscriptionManagement/ViewAll` 至 `/subscription-management/view-all` 。

`PageRouteTransformerConvention`只會轉換來自 Razor Pages 資料夾和檔案名的頁面路由自動產生的區段。 它不會轉換以指示詞新增的路由區段 `@page` 。 慣例也不會轉換所加入的路由 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> 。

`PageRouteTransformerConvention`會註冊為中的選項 `Startup.ConfigureServices` ：

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

## <a name="configure-a-page-route"></a>設定頁面路由

使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> ，在指定的頁面路徑上設定頁面的路由。 頁面的所產生連結會使用您指定的路由。 `AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。

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

執行的預設頁面模型提供者 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> 會叫用慣例，其設計目的是為了提供設定頁面模型的擴充點。 在建置和修改頁面探索與處理案例時，這些慣例很有用。

針對本節中的範例，範例應用程式 `AddHeaderAttribute` 會使用類別，也就是 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute> ，它會套用回應標頭：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。

**資料夾應用程式模型慣例**

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> 建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 針對指定資料夾下的所有頁面，在實例上叫用動作。

此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

**頁面應用程式模型慣例**

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 針對具有指定名稱的頁面，在上叫用動作。

此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

**設定篩選條件**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>設定要套用的指定篩選。 您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。 如果通過條件，則會新增標頭。 如果沒有，則會套用 `EmptyFilter`。

`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。 由於頁面會忽略動作篩選準則 Razor ， `EmptyFilter` 因此如果路徑不包含，就不會有任何作用 `OtherPages/Page2` 。

在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

**設定篩選條件 Factory**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>設定指定的 factory，將[篩選](xref:mvc/controllers/filters)套用至所有 Razor 頁面。

範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC 篩選條件和頁面篩選條件 (IPageFilter)

頁面會忽略 MVC[動作篩選準則](xref:mvc/controllers/filters#action-filters) Razor ，因為 Razor 頁面會使用處理程式方法。 其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。 如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。

頁面篩選準則（ <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ）是適用于頁面的篩選 Razor 。 如需詳細資訊，請參閱[ Razor 頁面的篩選方法](xref:razor-pages/filter)。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

瞭解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，在 Razor 頁面應用程式中控制頁面路由、探索和處理。

當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。

若要指定頁面路由、加入路由區段，或將參數加入至路由，請使用頁面的指示詞 `@page` 。 如需詳細資訊，請參閱[自訂路由](xref:razor-pages/index#custom-routes)。

有保留字無法做為路由區段或參數名稱使用。 如需詳細資訊，請參閱[路由：保留的路由名稱](xref:fundamentals/routing#reserved-routing-names)。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)（[如何下載](xref:index#how-to-download-a-sample)）

| 狀況 | 範例會示範 ... |
| -------- | --------------------------- |
| [模型慣例](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | 將路由範本和標頭新增至應用程式的頁面。 |
| [頁面路由動作慣例](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | 將路由範本新增至資料夾中的頁面，以及新增至單一頁面。 |
| [頁面模型動作慣例](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</li></ul> | 將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。 |

Razor在 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> 類別中的服務集合上，會使用擴充方法來新增和設定頁面慣例 `Startup` 。 稍後在本主題會說明下列慣例範例：

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

## <a name="route-order"></a>路由順序

路由會指定 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 進行處理（路由對應）。

| 單            | 行為 |
| :--------------: | -------- |
| -1               | 在處理其他路由之前，會先處理路由。 |
| 0                | 未指定順序（預設值）。 Not 指派 `Order` （ `Order = null` ）預設會將路由 `Order` 設為0（零）以進行處理。 |
| 1、2、 &hellip; n | 指定路由處理順序。 |

路由處理是依照慣例所建立：

* 路由會依序處理（-1、0、1、2、 &hellip; n）。
* 當路由具有相同的時 `Order` ，最特定的路由會先進行比對，後面接著較少的特定路由。
* 當具有相同 `Order` 和相同數目之參數的路由符合要求 URL 時，路由會依其加入至的順序進行處理 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection> 。

可能的話，請根據已建立的路由處理順序來避免。 一般而言，路由會選取 URL 相符的正確路由。 如果您必須設定路由 `Order` 屬性以正確地路由要求，則應用程式的路由配置可能會使用戶端變得困惑，而難以維護。 搜尋以簡化應用程式的路由配置。 範例應用程式需要明確的路由處理順序，才能使用單一應用程式來示範數個路由案例。 不過，您應該嘗試避免在 `Order` 生產應用程式中設定路由的做法。

Razor頁面路由和 MVC 控制器路由會共用一個執行。 如需有關 MVC 主題中路由順序的資訊，請前往[路由至控制器動作：排序屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。

## <a name="model-conventions"></a>模型慣例

新增的委派， <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 以加入適用于頁面的[模型慣例](xref:mvc/controllers/application-model#conventions) Razor 。

### <a name="add-a-route-model-convention-to-all-pages"></a>將路由模型慣例新增至所有頁面

使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> 至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面路由模型結構期間所套用的實例集合。

範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。 這可確保範例應用程式中的下列路由符合行為：

* `TheContactPage/{text?}`稍後會在主題中新增的路由範本。 連絡人頁面路由的預設順序為 `null` （ `Order = 0` ），因此它會符合 `{globalTemplate?}` 路由範本。
* `{aboutTemplate?}`稍後會在主題中新增路由範本。 `{aboutTemplate?}` 範本會指定 `Order` 為 `2`。 在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。
* `{otherPagesTemplate?}`稍後會在主題中新增路由範本。 `{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。 當使用路由參數要求*Pages/OtherPages*資料夾中的任何頁面時（例如， `/OtherPages/Page1/RouteDataValue` ），會將 "因此 routedatavalue" 載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["otherPagesTemplate"]` （）， `Order = 2` 因為設定了 `Order` 屬性。

盡可能不要設定 `Order` ，這會導致 `Order = 0` 。 依賴路由來選取正確的路由。

Razor<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>當 MVC 加入至中的服務集合時，會加入 [頁面] 選項，例如 [加入] `Startup.ConfigureServices` 。 如需範例，請參閱[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：

![使用路由區段 GlobalRouteValue 要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>將應用程式模型慣例新增至所有頁面

使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> 其新增至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面應用程式模型結構期間所套用的實例集合。

為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。 類別建構函式接受 `name` 字串和 `values` 字串陣列。 在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。 完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。

範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>將處理常式模型慣例新增至所有頁面

使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> 至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面處理常式模型結構中所套用的實例集合。

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

*Startup.cs*：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a>頁面路由動作慣例

衍生自的預設路由模型提供者 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> 會叫用慣例，其設計目的是為了提供設定頁面路由的擴充點。

### <a name="folder-route-model-convention"></a>資料夾路由模型慣例

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 針對指定資料夾下的所有頁面，叫用上的動作。

範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保 `{globalTemplate?}` 當提供單一路由值時，的範本（在主題中稍早設定為 `1` ）會獲得第一個路由資料值位置的優先權。 如果使用路由參數值要求*Pages/OtherPages*資料夾中的頁面（例如 `/OtherPages/Page1/RouteDataValue` ），則會將 "因此 routedatavalue" 載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["otherPagesTemplate"]` （）， `Order = 2` 因為設定了 `Order` 屬性。

盡可能不要設定 `Order` ，這會導致 `Order = 0` 。 依賴路由來選取正確的路由。

在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>頁面路由模型慣例

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 針對具有指定名稱的頁面，在上叫用動作。

範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保 `{globalTemplate?}` 當提供單一路由值時，的範本（在主題中稍早設定為 `1` ）會獲得第一個路由資料值位置的優先權。 如果要求的是「關於」頁面，其中的路由參數值為 `/About/RouteDataValue` ，則 "因此 routedatavalue" 會載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["aboutTemplate"]` （ `Order = 2` ），因為設定了 `Order` 屬性。

盡可能不要設定 `Order` ，這會導致 `Order = 0` 。 依賴路由來選取正確的路由。

在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>設定頁面路由

使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> ，在指定的頁面路徑上設定頁面的路由。 頁面的所產生連結會使用您指定的路由。 `AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。

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

執行的預設頁面模型提供者 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> 會叫用慣例，其設計目的是為了提供設定頁面模型的擴充點。 在建置和修改頁面探索與處理案例時，這些慣例很有用。

針對本節中的範例，範例應用程式 `AddHeaderAttribute` 會使用類別，也就是 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute> ，它會套用回應標頭：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。

**資料夾應用程式模型慣例**

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> 建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 針對指定資料夾下的所有頁面，在實例上叫用動作。

此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

**頁面應用程式模型慣例**

使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 針對具有指定名稱的頁面，在上叫用動作。

此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

**設定篩選條件**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>設定要套用的指定篩選。 您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。 如果通過條件，則會新增標頭。 如果沒有，則會套用 `EmptyFilter`。

`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。 由於頁面會忽略動作篩選準則 Razor ， `EmptyFilter` 因此如果路徑不包含，就不會有任何作用 `OtherPages/Page2` 。

在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

**設定篩選條件 Factory**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>設定指定的 factory，將[篩選](xref:mvc/controllers/filters)套用至所有 Razor 頁面。

範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC 篩選條件和頁面篩選條件 (IPageFilter)

頁面會忽略 MVC[動作篩選準則](xref:mvc/controllers/filters#action-filters) Razor ，因為 Razor 頁面會使用處理程式方法。 其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。 如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。

頁面篩選準則（ <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ）是適用于頁面的篩選 Razor 。 如需詳細資訊，請參閱[ Razor 頁面的篩選方法](xref:razor-pages/filter)。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end
