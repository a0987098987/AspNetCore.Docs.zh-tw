---
title: ASP.NET Core 中的 Razor 頁面路由和應用程式慣例
author: rick-anderson
description: 探索路由和應用程式模型提供者慣例如何協助您控制頁面路由、探索與處理。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: f45e327051aba54d1cab67148eb540fb1a5cc149
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667857"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>ASP.NET Core 中的 Razor 頁面路由和應用程式慣例

::: moniker range=">= aspnetcore-3.0"

了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。

當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。

要指定頁面路由、添加工藝路線段或向工藝路線添加參數,請使用頁面`@page`的指令。 有關詳細資訊,請參閱[自訂路由](xref:razor-pages/index#custom-routes)。

保留的單詞不能用作工藝路線段或參數名稱。 有關詳細資訊,請參閱[路由:保留路由名稱](xref:fundamentals/routing#reserved-routing-names)。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)([如何下載](xref:index#how-to-download-a-sample))

| 狀況 | 範例會示範 ... |
| -------- | --------------------------- |
| [模型約定](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | 將路由範本和標頭新增至應用程式的頁面。 |
| [頁面路由動作慣例](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | 將路由範本新增至資料夾中的頁面，以及新增至單一頁面。 |
| [頁面模型動作慣例](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</li></ul> | 將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。 |

使用<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>擴充方法<xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>`Startup`在類中的服務集合上添加和配置 Razor Pages 約定。 稍後在本主題會說明下列慣例範例：

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

## <a name="route-order"></a>工藝路線訂單

路由指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*>用於處理的 指定 (路由匹配)。

| 單            | 行為 |
| :--------------: | -------- |
| -1               | 在處理其他路由之前,將處理該路由。 |
| 0                | 未指定訂單(預設值)。 不分配`Order``Order = null`( )`Order`預設路由為 0 (零) 進行處理。 |
| 1, 2, &hellip; n | 指定工藝路線處理順序。 |

路由處理依慣例建立:

* 路由按順序處理(-1、0、1、2、n)。 &hellip;
* 當路由相同`Order`時,最具體的路由首先匹配,然後是不太具體的路由。
* 當具有相同`Order`與相同的參數的路由與請求網址, 則會將設定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>此選項 。

如果可能,請根據已建立的工藝路線處理順序避免。 通常,路由選擇具有 URL 匹配的正確路由。 如果必須正確設置路由`Order`屬性以路由請求,則應用的路由方案可能對用戶端造成混淆,並且難以維護。 尋求簡化應用程式的路由方案。 示例應用需要顯式路由處理順序來演示使用單個應用的多個路由方案。 但是,您應該嘗試避免在生產應用中設置工藝路線`Order`的做法。

Razor Pages 路由和 MVC 控制器路由會共用實作。 MVC 主題中有關路由順序的資訊可在[路由到控制器操作:訂購屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。

## <a name="model-conventions"></a>模型慣例

添加用於<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>添加應用於 Razor 頁面的[模型約定](xref:mvc/controllers/application-model#conventions)的委託。

### <a name="add-a-route-model-convention-to-all-pages"></a>將路由模型慣例新增至所有頁面

用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>到在頁面路由<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例集合。

範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。 這可確保範例的以下路由符合行為:

* 主題稍後將添加`TheContactPage/{text?}`的路由範本。 連絡人頁路由的預設順序`null`為`Order = 0`( ),`{globalTemplate?}`因此它與 工藝路線範本之前匹配。
* 主題`{aboutTemplate?}`稍後將添加路由範本。 `{aboutTemplate?}` 範本會指定 `Order` 為 `2`。 在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。
* 主題`{otherPagesTemplate?}`稍後將添加路由範本。 `{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。 當使用路由參數(例如,RouteDataValue)`/OtherPages/Page1/RouteDataValue`請求 *「頁面/其他頁面*」資料夾中的任何頁面時,`Order`由於設置 屬性,`RouteData.Values["globalTemplate"]`將`Order = 1`載`RouteData.Values["otherPagesTemplate"]`入`Order = 2`到 ( ) 而不是 ( ) 中。

只要可能,不要設置`Order``Order = 0`會導致的 。 依靠路由選擇正確的路由。

當將 MVC<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>添加`Startup.ConfigureServices`到 中的 服務集合時,將添加 Razor 頁面選項(如添加 )。 如需範例，請參閱[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：

![使用路由區段 GlobalRouteValue 要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>將應用程式模型慣例新增至所有頁面

用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>到在頁面應用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例集合。

為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。 類別建構函式接受 `name` 字串和 `values` 字串陣列。 在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。 完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。

範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>將處理常式模型慣例新增至所有頁面

用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention>到在頁面處理程式<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例的集合。

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a>頁面路由動作慣例

派生自<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider>調用的約定的預設路由模型提供者旨在為配置頁面路由提供擴展點。

### <a name="folder-route-model-convention"></a>資料夾路由模型慣例

建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>呼叫<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>指定 資料夾下所有頁面的作業的 。

範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保在提供單個路由`{globalTemplate?}`值時,`1`為 (在主題中設置為 ) 的範本為 第一個路由數據值位置授予優先順序。 如果請求頁面 */其他頁面*資料夾中的頁面具有路由參數值(例如`/OtherPages/Page1/RouteDataValue`),`RouteData.Values["globalTemplate"]``Order = 1``RouteData.Values["otherPagesTemplate"]``Order = 2``Order`則由於設置 屬性,將載入到 ( ) 而不是 ( ) 中。

只要可能,不要設置`Order``Order = 0`會導致的 。 依靠路由選擇正確的路由。

在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>頁面路由模型慣例

建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>呼叫 以指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>名稱的頁面上的作業的 。

範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保在提供單個路由`{globalTemplate?}`值時,`1`為 (在主題中設置為 ) 的範本為 第一個路由數據值位置授予優先順序。 如果"關於"頁`/About/RouteDataValue`的請求時路由參數值為 ,則由於`RouteData.Values["globalTemplate"]``Order = 1``RouteData.Values["aboutTemplate"]``Order = 2``Order`設置 屬性,將載入到 ( ) 而不是 ( ) 中。

只要可能,不要設置`Order``Order = 0`會導致的 。 依靠路由選擇正確的路由。

在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>使用參數轉換器自訂頁面路由

ASP.NET核心生成的頁面路由可以使用參數變壓器進行自定義。 參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。 例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。

`PageRouteTransformerConvention`頁面路由模型約定將參數轉換器應用於應用中自動生成的頁面路由的資料夾和檔名段。 例如,剃刀頁面檔在 */Pages/訂閱管理/ViewAll.cshtml*將有其路`/SubscriptionManagement/ViewAll`由從`/subscription-management/view-all`重寫到 。

`PageRouteTransformerConvention`僅轉換來自 Razor Pages 資料夾和檔名的頁面路由的自動生成的段。 它不會轉換隨指令添加的`@page`路由段。 約定也不會轉換 添加<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>的路由。

在`PageRouteTransformerConvention``Startup.ConfigureServices`中 註冊為選項:

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

## <a name="configure-a-page-route"></a>設定頁面路由

用於<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>指定的頁面路徑上配置到頁面的路由。 頁面的所產生連結會使用您指定的路由。 `AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。

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

實現<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider>的預設頁面模型提供程式調用旨在為配置頁面模型提供擴展點的約定。 在建置和修改頁面探索與處理案例時，這些慣例很有用。

對於本節中的示例,示例應用使用一個`AddHeaderAttribute`類,該類是<xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>應用 回應標頭的 類:

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。

**資料夾應用程式模型慣例**

建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>呼叫指定資料夾下所有頁面的<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>實體管理的 。

此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

**頁面應用程式模型慣例**

建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>呼叫 以指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>名稱的頁面上的作業的 。

此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

**設定篩選條件**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>配置要應用的指定篩選器。 您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。 如果通過條件，則會新增標頭。 如果沒有，則會套用 `EmptyFilter`。

`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。 由於 Razor Pages 會忽略操作`EmptyFilter`篩選器, 因此若路徑不包含`OtherPages/Page2`,則不起作用 。

在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

**設定篩選條件 Factory**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>將指定的工廠設定為對所有 Razor 頁面套[用篩選器](xref:mvc/controllers/filters)。

範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*：

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC 篩選條件和頁面篩選條件 (IPageFilter)

Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。 其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。 如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。

頁面篩選器<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>( ) 是應用於剃刀頁面的篩選器。 如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。

當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。

要指定頁面路由、添加工藝路線段或向工藝路線添加參數,請使用頁面`@page`的指令。 有關詳細資訊,請參閱[自訂路由](xref:razor-pages/index#custom-routes)。

保留的單詞不能用作工藝路線段或參數名稱。 有關詳細資訊,請參閱[路由:保留路由名稱](xref:fundamentals/routing#reserved-routing-names)。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)([如何下載](xref:index#how-to-download-a-sample))

| 狀況 | 範例會示範 ... |
| -------- | --------------------------- |
| [模型約定](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | 將路由範本和標頭新增至應用程式的頁面。 |
| [頁面路由動作慣例](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | 將路由範本新增至資料夾中的頁面，以及新增至單一頁面。 |
| [頁面模型動作慣例](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</li></ul> | 將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。 |

使用<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>擴充方法<xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>`Startup`在類中的服務集合上添加和配置 Razor Pages 約定。 稍後在本主題會說明下列慣例範例：

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

## <a name="route-order"></a>工藝路線訂單

路由指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*>用於處理的 指定 (路由匹配)。

| 單            | 行為 |
| :--------------: | -------- |
| -1               | 在處理其他路由之前,將處理該路由。 |
| 0                | 未指定訂單(預設值)。 不分配`Order``Order = null`( )`Order`預設路由為 0 (零) 進行處理。 |
| 1, 2, &hellip; n | 指定工藝路線處理順序。 |

路由處理依慣例建立:

* 路由按順序處理(-1、0、1、2、n)。 &hellip;
* 當路由相同`Order`時,最具體的路由首先匹配,然後是不太具體的路由。
* 當具有相同`Order`與相同的參數的路由與請求網址, 則會將設定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>此選項 。

如果可能,請根據已建立的工藝路線處理順序避免。 通常,路由選擇具有 URL 匹配的正確路由。 如果必須正確設置路由`Order`屬性以路由請求,則應用的路由方案可能對用戶端造成混淆,並且難以維護。 尋求簡化應用程式的路由方案。 示例應用需要顯式路由處理順序來演示使用單個應用的多個路由方案。 但是,您應該嘗試避免在生產應用中設置工藝路線`Order`的做法。

Razor Pages 路由和 MVC 控制器路由會共用實作。 MVC 主題中有關路由順序的資訊可在[路由到控制器操作:訂購屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。

## <a name="model-conventions"></a>模型慣例

添加用於<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>添加應用於 Razor 頁面的[模型約定](xref:mvc/controllers/application-model#conventions)的委託。

### <a name="add-a-route-model-convention-to-all-pages"></a>將路由模型慣例新增至所有頁面

用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>到在頁面路由<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例集合。

範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。 這可確保範例的以下路由符合行為:

* 主題稍後將添加`TheContactPage/{text?}`的路由範本。 連絡人頁路由的預設順序`null`為`Order = 0`( ),`{globalTemplate?}`因此它與 工藝路線範本之前匹配。
* 主題`{aboutTemplate?}`稍後將添加路由範本。 `{aboutTemplate?}` 範本會指定 `Order` 為 `2`。 在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。
* 主題`{otherPagesTemplate?}`稍後將添加路由範本。 `{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。 當使用路由參數(例如,RouteDataValue)`/OtherPages/Page1/RouteDataValue`請求 *「頁面/其他頁面*」資料夾中的任何頁面時,`Order`由於設置 屬性,`RouteData.Values["globalTemplate"]`將`Order = 1`載`RouteData.Values["otherPagesTemplate"]`入`Order = 2`到 ( ) 而不是 ( ) 中。

只要可能,不要設置`Order``Order = 0`會導致的 。 依靠路由選擇正確的路由。

當將 MVC<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>添加`Startup.ConfigureServices`到 中的 服務集合時,將添加 Razor 頁面選項(如添加 )。 如需範例，請參閱[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：

![使用路由區段 GlobalRouteValue 要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>將應用程式模型慣例新增至所有頁面

用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>到在頁面應用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例集合。

為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。 類別建構函式接受 `name` 字串和 `values` 字串陣列。 在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。 完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。

範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>將處理常式模型慣例新增至所有頁面

用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention>到在頁面處理程式<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例的集合。

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a>頁面路由動作慣例

派生自<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider>調用的約定的預設路由模型提供者旨在為配置頁面路由提供擴展點。

### <a name="folder-route-model-convention"></a>資料夾路由模型慣例

建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>呼叫<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>指定 資料夾下所有頁面的作業的 。

範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保在提供單個路由`{globalTemplate?}`值時,`1`為 (在主題中設置為 ) 的範本為 第一個路由數據值位置授予優先順序。 如果請求頁面 */其他頁面*資料夾中的頁面具有路由參數值(例如`/OtherPages/Page1/RouteDataValue`),`RouteData.Values["globalTemplate"]``Order = 1``RouteData.Values["otherPagesTemplate"]``Order = 2``Order`則由於設置 屬性,將載入到 ( ) 而不是 ( ) 中。

只要可能,不要設置`Order``Order = 0`會導致的 。 依靠路由選擇正確的路由。

在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>頁面路由模型慣例

建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>呼叫 以指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>名稱的頁面上的作業的 。

範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保在提供單個路由`{globalTemplate?}`值時,`1`為 (在主題中設置為 ) 的範本為 第一個路由數據值位置授予優先順序。 如果"關於"頁`/About/RouteDataValue`的請求時路由參數值為 ,則由於`RouteData.Values["globalTemplate"]``Order = 1``RouteData.Values["aboutTemplate"]``Order = 2``Order`設置 屬性,將載入到 ( ) 而不是 ( ) 中。

只要可能,不要設置`Order``Order = 0`會導致的 。 依靠路由選擇正確的路由。

在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>使用參數轉換器自訂頁面路由

ASP.NET核心生成的頁面路由可以使用參數變壓器進行自定義。 參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。 例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。

`PageRouteTransformerConvention`頁面路由模型約定將參數轉換器應用於應用中自動生成的頁面路由的資料夾和檔名段。 例如,剃刀頁面檔在 */Pages/訂閱管理/ViewAll.cshtml*將有其路`/SubscriptionManagement/ViewAll`由從`/subscription-management/view-all`重寫到 。

`PageRouteTransformerConvention`僅轉換來自 Razor Pages 資料夾和檔名的頁面路由的自動生成的段。 它不會轉換隨指令添加的`@page`路由段。 約定也不會轉換 添加<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>的路由。

在`PageRouteTransformerConvention``Startup.ConfigureServices`中 註冊為選項:

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

用於<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>指定的頁面路徑上配置到頁面的路由。 頁面的所產生連結會使用您指定的路由。 `AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。

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

實現<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider>的預設頁面模型提供程式調用旨在為配置頁面模型提供擴展點的約定。 在建置和修改頁面探索與處理案例時，這些慣例很有用。

對於本節中的示例,示例應用使用一個`AddHeaderAttribute`類,該類是<xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>應用 回應標頭的 類:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。

**資料夾應用程式模型慣例**

建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>呼叫指定資料夾下所有頁面的<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>實體管理的 。

此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

**頁面應用程式模型慣例**

建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>呼叫 以指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>名稱的頁面上的作業的 。

此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

**設定篩選條件**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>配置要應用的指定篩選器。 您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。 如果通過條件，則會新增標頭。 如果沒有，則會套用 `EmptyFilter`。

`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。 由於 Razor Pages 會忽略操作`EmptyFilter`篩選器, 因此若路徑不包含`OtherPages/Page2`,則不起作用 。

在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

**設定篩選條件 Factory**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>將指定的工廠設定為對所有 Razor 頁面套[用篩選器](xref:mvc/controllers/filters)。

範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC 篩選條件和頁面篩選條件 (IPageFilter)

Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。 其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。 如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。

頁面篩選器<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>( ) 是應用於剃刀頁面的篩選器。 如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。

當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。

要指定頁面路由、添加工藝路線段或向工藝路線添加參數,請使用頁面`@page`的指令。 有關詳細資訊,請參閱[自訂路由](xref:razor-pages/index#custom-routes)。

保留的單詞不能用作工藝路線段或參數名稱。 有關詳細資訊,請參閱[路由:保留路由名稱](xref:fundamentals/routing#reserved-routing-names)。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)([如何下載](xref:index#how-to-download-a-sample))

| 狀況 | 範例會示範 ... |
| -------- | --------------------------- |
| [模型約定](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | 將路由範本和標頭新增至應用程式的頁面。 |
| [頁面路由動作慣例](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | 將路由範本新增至資料夾中的頁面，以及新增至單一頁面。 |
| [頁面模型動作慣例](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</li></ul> | 將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。 |

使用<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>擴充方法<xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>`Startup`在類中的服務集合上添加和配置 Razor Pages 約定。 稍後在本主題會說明下列慣例範例：

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

## <a name="route-order"></a>工藝路線訂單

路由指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*>用於處理的 指定 (路由匹配)。

| 單            | 行為 |
| :--------------: | -------- |
| -1               | 在處理其他路由之前,將處理該路由。 |
| 0                | 未指定訂單(預設值)。 不分配`Order``Order = null`( )`Order`預設路由為 0 (零) 進行處理。 |
| 1, 2, &hellip; n | 指定工藝路線處理順序。 |

路由處理依慣例建立:

* 路由按順序處理(-1、0、1、2、n)。 &hellip;
* 當路由相同`Order`時,最具體的路由首先匹配,然後是不太具體的路由。
* 當具有相同`Order`與相同的參數的路由與請求網址, 則會將設定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>此選項 。

如果可能,請根據已建立的工藝路線處理順序避免。 通常,路由選擇具有 URL 匹配的正確路由。 如果必須正確設置路由`Order`屬性以路由請求,則應用的路由方案可能對用戶端造成混淆,並且難以維護。 尋求簡化應用程式的路由方案。 示例應用需要顯式路由處理順序來演示使用單個應用的多個路由方案。 但是,您應該嘗試避免在生產應用中設置工藝路線`Order`的做法。

Razor Pages 路由和 MVC 控制器路由會共用實作。 MVC 主題中有關路由順序的資訊可在[路由到控制器操作:訂購屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。

## <a name="model-conventions"></a>模型慣例

添加用於<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>添加應用於 Razor 頁面的[模型約定](xref:mvc/controllers/application-model#conventions)的委託。

### <a name="add-a-route-model-convention-to-all-pages"></a>將路由模型慣例新增至所有頁面

用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>到在頁面路由<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例集合。

範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。 這可確保範例的以下路由符合行為:

* 主題稍後將添加`TheContactPage/{text?}`的路由範本。 連絡人頁路由的預設順序`null`為`Order = 0`( ),`{globalTemplate?}`因此它與 工藝路線範本之前匹配。
* 主題`{aboutTemplate?}`稍後將添加路由範本。 `{aboutTemplate?}` 範本會指定 `Order` 為 `2`。 在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。
* 主題`{otherPagesTemplate?}`稍後將添加路由範本。 `{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。 當使用路由參數(例如,RouteDataValue)`/OtherPages/Page1/RouteDataValue`請求 *「頁面/其他頁面*」資料夾中的任何頁面時,`Order`由於設置 屬性,`RouteData.Values["globalTemplate"]`將`Order = 1`載`RouteData.Values["otherPagesTemplate"]`入`Order = 2`到 ( ) 而不是 ( ) 中。

只要可能,不要設置`Order``Order = 0`會導致的 。 依靠路由選擇正確的路由。

當將 MVC<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>添加`Startup.ConfigureServices`到 中的 服務集合時,將添加 Razor 頁面選項(如添加 )。 如需範例，請參閱[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：

![使用路由區段 GlobalRouteValue 要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>將應用程式模型慣例新增至所有頁面

用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>到在頁面應用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例集合。

為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。 類別建構函式接受 `name` 字串和 `values` 字串陣列。 在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。 完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。

範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>將處理常式模型慣例新增至所有頁面

用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention>到在頁面處理程式<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例的集合。

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a>頁面路由動作慣例

派生自<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider>調用的約定的預設路由模型提供者旨在為配置頁面路由提供擴展點。

### <a name="folder-route-model-convention"></a>資料夾路由模型慣例

建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>呼叫<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>指定 資料夾下所有頁面的作業的 。

範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保在提供單個路由`{globalTemplate?}`值時,`1`為 (在主題中設置為 ) 的範本為 第一個路由數據值位置授予優先順序。 如果請求頁面 */其他頁面*資料夾中的頁面具有路由參數值(例如`/OtherPages/Page1/RouteDataValue`),`RouteData.Values["globalTemplate"]``Order = 1``RouteData.Values["otherPagesTemplate"]``Order = 2``Order`則由於設置 屬性,將載入到 ( ) 而不是 ( ) 中。

只要可能,不要設置`Order``Order = 0`會導致的 。 依靠路由選擇正確的路由。

在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>頁面路由模型慣例

建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>呼叫 以指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>名稱的頁面上的作業的 。

範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。 這可確保在提供單個路由`{globalTemplate?}`值時,`1`為 (在主題中設置為 ) 的範本為 第一個路由數據值位置授予優先順序。 如果"關於"頁`/About/RouteDataValue`的請求時路由參數值為 ,則由於`RouteData.Values["globalTemplate"]``Order = 1``RouteData.Values["aboutTemplate"]``Order = 2``Order`設置 屬性,將載入到 ( ) 而不是 ( ) 中。

只要可能,不要設置`Order``Order = 0`會導致的 。 依靠路由選擇正確的路由。

在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>設定頁面路由

用於<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>指定的頁面路徑上配置到頁面的路由。 頁面的所產生連結會使用您指定的路由。 `AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。

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

實現<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider>的預設頁面模型提供程式調用旨在為配置頁面模型提供擴展點的約定。 在建置和修改頁面探索與處理案例時，這些慣例很有用。

對於本節中的示例,示例應用使用一個`AddHeaderAttribute`類,該類是<xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>應用 回應標頭的 類:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。

**資料夾應用程式模型慣例**

建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>呼叫指定資料夾下所有頁面的<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>實體管理的 。

此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

**頁面應用程式模型慣例**

建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>呼叫 以指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>名稱的頁面上的作業的 。

此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

**設定篩選條件**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>配置要應用的指定篩選器。 您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。 如果通過條件，則會新增標頭。 如果沒有，則會套用 `EmptyFilter`。

`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。 由於 Razor Pages 會忽略操作`EmptyFilter`篩選器, 因此若路徑不包含`OtherPages/Page2`,則不起作用 。

在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

**設定篩選條件 Factory**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>將指定的工廠設定為對所有 Razor 頁面套[用篩選器](xref:mvc/controllers/filters)。

範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*：

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC 篩選條件和頁面篩選條件 (IPageFilter)

Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。 其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。 如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。

頁面篩選器<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>( ) 是應用於剃刀頁面的篩選器。 如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end
