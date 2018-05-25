---
title: ASP.NET Core 中的 Razor 頁面路由和應用程式慣例
author: guardrex
description: 探索路由和應用程式模型提供者慣例如何協助您控制頁面路由、探索與處理。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 04/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/razor-pages/razor-pages-conventions
ms.openlocfilehash: 15bb0687ffef777b82ea9374fdc3b92f3af7818b
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/10/2018
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>ASP.NET Core 中的 Razor 頁面路由和應用程式慣例

作者：[Luke Latham](https://github.com/guardrex)

了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。 當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-conventions/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

::: moniker range="= aspnetcore-2.0"
| 情節 | 範例會示範 ... |
| -------- | --------------------------- |
| [模型慣例](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | 將路由範本和標頭新增至應用程式的頁面。 |
| [頁面路由動作慣例](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | 將路由範本新增至資料夾中的頁面，以及新增至單一頁面。 |
| [頁面模型動作慣例](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</li></ul> | 將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。 |
| [預設頁面應用程式模型提供者](#replace-the-default-page-app-model-provider) | 取代預設頁面模型提供者，以變更處理常式名稱的慣例。 |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| 情節 | 範例會示範 ... |
| -------- | --------------------------- |
| [模型慣例](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | 將路由範本和標頭新增至應用程式的頁面。 |
| [頁面路由動作慣例](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | 將路由範本新增至資料夾中的頁面，以及新增至單一頁面。 |
| [頁面模型動作慣例](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</li></ul> | 將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。 |
| [預設頁面應用程式模型提供者](#replace-the-default-page-app-model-provider) | 取代預設頁面模型提供者，以變更處理常式名稱的慣例。 |
::: moniker-end

Razor 頁面慣例會使用 `Startup` 類別的服務集合上 [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) 的 [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) 擴充方法進行新增和設定。 稍後在本主題會說明下列慣例範例：

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

## <a name="model-conventions"></a>模型慣例

新增 [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) 的委派，以新增套用至 Razor 頁面的[模型慣例](xref:mvc/controllers/application-model#conventions)。

**將路由模型慣例新增至所有頁面**

使用[慣例](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)來建立 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)，並將其新增至頁面路由模型建構期間所套用的 [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) 執行個體的集合。

範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> `AttributeRouteModel` 的 `Order` 屬性設定為 `-1`。 這可確保在提供單一路由值時，此範本是第一個路由資料值位置的指定優先順序，而且也確保它會優先於自動產生 Razor 頁面路由。 例如，範例會新增本主題稍後的 `{aboutTemplate?}` 路由範本。 `{aboutTemplate?}` 範本會指定 `Order` 為 `1`。 在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = -1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 1`)。

將 MVC 新增至 `Startup.ConfigureServices` 中的服務集合時，會新增 Razor 頁面選項，例如新增[慣例](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)。 如需範例，請參閱[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-conventions/sample/)。

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet1)]

在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：

![使用路由區段 GlobalRouteValue 要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-template.png)

**將應用程式模型慣例新增至所有頁面**

使用[慣例](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)來建立 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)，並將其新增至頁面模型建構期間所套用之 [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) 執行個體的集合。

為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。 類別建構函式接受 `name` 字串和 `values` 字串陣列。 在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。 完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。

範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*：

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet2)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

::: moniker range=">= aspnetcore-2.1"
**將處理常式模型慣例新增至所有頁面**



使用[慣例](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)來建立 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention)，並將其新增至頁面模型建構期間所套用之 [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) 執行個體的集合。

```csharp
public class GlobalPageHandlerModelConvention 
    : IPageHandlerModelConvention
{
    public void Apply(PageHandlerModel model)
    {
        ...
    }
}
```

`Startup.ConfigureServices`：

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(new GlobalPageHandlerModelConvention());
        });
```
::: moniker-end

## <a name="page-route-action-conventions"></a>頁面路由動作慣例

衍生自 [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) 的預設路由模型提供者會叫用慣例，這些慣例的設計目的是要提供設定頁面路由的擴充點。

**資料夾路由模型慣例**

使用 [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) 來建立並新增 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)，它會針對指定資料夾下的所有頁面在 [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) 上叫用動作。

範例應用程式會使用 `AddFolderRouteModelConvention`，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> `AttributeRouteModel` 的 `Order` 屬性設定為 `1`。 這可確保當提供單一路由值時，`{globalTemplate?}` 的範本 (稍早在本主題中設定) 會指定第一個路由資料值位置的優先權。 如果在 `/OtherPages/Page1/RouteDataValue` 上要求 Page1 頁面，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = -1`)，而不是 `RouteData.Values["otherPagesTemplate"]` (`Order = 1`)。

在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

**頁面路由模型慣例**

使用 [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) 來建立並新增 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)，它會針對指定資料夾下的所有頁面在 [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) 上叫用動作。

範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> `AttributeRouteModel` 的 `Order` 屬性設定為 `1`。 這可確保當提供單一路由值時，`{globalTemplate?}` 的範本 (稍早在本主題中設定) 會指定第一個路由資料值位置的優先權。 如果在 `/About/RouteDataValue` 上要求 About 頁面，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = -1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 1`)。

在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。 呈現的頁面會顯示在頁面的 OnGet 方法中擷取了路由資料值。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>設定頁面路由

使用 [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) 以針對指定頁面路徑上的頁面設定路由。 頁面的所產生連結會使用您指定的路由。 `AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。

範例應用程式為 *Contact.cshtml* 建立 `/TheContactPage` 的路由：

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet5)]

Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。

範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。 該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：

[!code-cshtml[](razor-pages-conventions/sample/Pages/Contact.cshtml?highlight=1)]

請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。 如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。 呈現的頁面會顯示 'text' 區段值。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>頁面模型動作慣例

實作 [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) 的預設頁面模型提供者會叫用慣例，這些慣例的設計目的是要提供設定頁面模型的擴充點。 在建置和修改頁面探索與處理案例時，這些慣例很有用。

對於本節中的範例，範例應用程式會使用 `AddHeaderAttribute` 類別，這是可套用回應標頭的 [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute)：

[!code-csharp[](razor-pages-conventions/sample/Filters/AddHeader.cs?name=snippet1)]

透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。

**資料夾應用程式模型慣例**

使用 [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) 來建立並新增 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)，它會針對指定資料夾下的所有頁面在 [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) 執行個體上叫用動作。

此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet6)]

在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

**頁面應用程式模型慣例**

使用 [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) 來建立並新增 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)，它會針對具有指定名稱的頁面在 [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) 上叫用動作。

此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet7)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

**設定篩選條件**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) 可設定要套用的指定篩選條件。 您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet8)]

使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。 如果通過條件，則會新增標頭。 如果沒有，則會套用 `EmptyFilter`。

`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。 由於 Razor Pages 會忽略動作篩選條件，因此如果路徑未包含 `OtherPages/Page2`，則 `EmptyFilter` 不會如預期般生效。

在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

**設定篩選條件 Factory**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) 可設定指定的 Factory，以將[篩選條件](xref:mvc/controllers/filters)套用至所有 Razor Pages。

範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*：

[!code-csharp[](razor-pages-conventions/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>取代預設頁面應用程式模型提供者

Razor Pages 使用 `IPageApplicationModelProvider` 介面來建立 [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider)。 您可以繼承自預設模型提供者，以提供您自己的實作邏輯來進行處理常式探索和處理。 預設實作 ([參考來源](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) 會建立「未具名」和「具名」處理常式命名的慣例，如下所述。

**預設的未具名處理常式方法**

HTTP 指令動詞的處理常式方法 (「未具名」處理常式方法) 遵循一個慣例：`On<HTTP verb>[Async]` (附加 `Async` 是選擇性項目，但建議用於非同步方法)。

| 未具名處理常式方法     | 運算                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | 初始化頁面狀態。     |
| `OnPost`/`OnPostAsync`     | 處理 POST 要求。          |
| `OnDelete`/`OnDeleteAsync` | 處理 DELETE 要求&#8224;。 |
| `OnPut`/`OnPutAsync`       | 處理 PUT 要求&#8224;。    |
| `OnPatch`/`OnPatchAsync`   | 處理 PATCH 要求&#8224;。  |

&#8224;用於對頁面進行 API 呼叫。

**預設的具名處理常式方法**

開發人員所提供的處理常式方法 (「具名」處理常式方法) 遵循類似的慣例。 處理常式名稱會出現在 HTTP 指令動詞之後，或 HTTP 指令動詞與 `Async` 之間：`On<HTTP verb><handler name>[Async]` (附加 `Async` 是選擇性項目，但建議用於非同步方法)。 例如，處理訊息的方法可能會採用下表顯示的命名。

| 範例具名處理常式方法             | 範例作業        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | 取得訊息。        |
| `OnPostMessage`/`OnPostMessageAsync`     | POST (張貼) 訊息。          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | DELETE (刪除) 訊息&#8224;。 |
| `OnPutMessage`/`OnPutMessageAsync`       | PUT (放置) 訊息&#8224;。    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | PPATCH (修補) 訊息&#8224;。  |

&#8224;用於對頁面進行 API 呼叫。

**自訂處理常式方法名稱**

假設您想要變更未具名和具名處理常式方法的命名方式。 另一種命名配置是避免以 "On" 作為方法名稱開頭，並使用第一個文字區段來判斷 HTTP 指令動詞。 您可以進行其他變更，例如將 DELETE、PUT 和 PATCH 的指令動詞轉換為 POST。 這類配置會提供下表顯示的方法名稱。

| 處理常式方法                       | 運算                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | 初始化頁面狀態。     |
| `Post`/`PostAsync`                   | 處理 POST 要求。          |
| `Delete`/`DeleteAsync`               | 處理 DELETE 要求&#8224;。 |
| `Put`/`PutAsync`                     | 處理 PUT 要求&#8224;。    |
| `Patch`/`PatchAsync`                 | 處理 PATCH 要求&#8224;。  |
| `GetMessage`                         | 取得訊息。              |
| `PostMessage`/`PostMessageAsync`     | POST (張貼) 訊息。                |
| `DeleteMessage`/`DeleteMessageAsync` | POST (張貼) 要刪除的訊息。      |
| `PutMessage`/`PutMessageAsync`       | POST (張貼) 要放置的訊息。         |
| `PatchMessage`/`PatchMessageAsync`   | POST (張貼) 要修補的訊息。       |

&#8224;用於對頁面進行 API 呼叫。

若要建立這種配置，請自 `DefaultPageApplicationModelProvider` 類別繼承並覆寫 [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) 方法，以提供自訂邏輯來解析 [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 處理常式名稱。 範例應用程式示範如何在 `CustomPageApplicationModelProvider` 類別中完成這項處理：

[!code-csharp[](razor-pages-conventions/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

類別的反白顯示項目包括：

* 類別繼承自 `DefaultPageApplicationModelProvider`。
* 在建立 `PageHandlerModel` 時，`TryParseHandlerMethod` 處理一個處理常式來判斷 HTTP 指令動詞 (`httpMethod`) 和具名處理常式名稱 (`handlerName`)。
  * 會忽略 `Async` 後置詞 (如果有的話)。
  * 大小寫用來剖析方法名稱中的 HTTP 指令動詞。
  * 如果方法名稱 (不含 `Async`) 等於 HTTP 指令動詞名稱，則沒有具名處理常式。 `handlerName` 會設定為 `null`，而方法名稱為 `Get`、`Post`、`Delete`、`Put` 或 `Patch`。
  * 如果方法名稱 (不含 `Async`) 長度超過 HTTP 指令動詞名稱，則有具名處理常式。 `handlerName` 已設為 `<method name (less 'Async', if present)>`。 例如，"GetMessage" 和 "GetMessageAsync" 都會產生 "GetMessage" 的處理常式名稱。
  * DELETE、PUT 和 PATCH 的 HTTP 指令動詞會轉換為 POST。

在 `Startup` 類別中註冊 `CustomPageApplicationModelProvider`：

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet10)]

*Index.cshtml.cs* 中的頁面模型顯示一般處理常式方法的命名慣例如何針對應用程式中的頁面變更。 移除了與 Razor Pages 搭配使用的一般 "On" 前置詞命名。 初始化頁面狀態的方法現在已命名為 `Get`。 如果您針對任一頁面開啟任何頁面模型，則可以看到在整個應用程式中使用的這個慣例。

每個其他方法會以描述其處理的 HTTP 指令動詞為開頭。 以 `Delete` 開頭的兩個方法通常會視為 DELETE HTTP 指令動詞，但 `TryParseHandlerMethod` 中的邏輯會針對這兩個處理常式將指令動詞明確設定為 POST。

請注意，在 `DeleteAllMessages` 和 `DeleteMessageAsync` 之間，`Async` 是選擇性項目。 它們都是非同步方法，但是您可以選擇使用 `Async` 後置詞或不使用；建議您使用。 `DeleteAllMessages` 在這裡作為示範用途，但建議您將這種方法命名為 `DeleteAllMessagesAsync`。 它不會影響範例實作的處理，但是使用 `Async` 後置詞表明它是非同步方法。

[!code-csharp[](razor-pages-conventions/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

請注意，*Index.cshtml* 中提供的處理常式名稱　符合 `DeleteAllMessages` 和 `DeleteMessageAsync` 處理常式方法：

[!code-cshtml[](razor-pages-conventions/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`TryParseHandlerMethod` 會針對 POST 要求與方法的處理常式比對排除 `DeleteMessageAsync` 中的 `Async`。 `DeleteMessage` 的 `asp-page-handler` 名稱會與處理常式方法 `DeleteMessageAsync` 相符。

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC 篩選條件和頁面篩選條件 (IPageFilter)

Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。 其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。 如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。

頁面篩選條件 ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) 是可套用至 Razor 頁面的篩選條件。 如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:mvc/razor-pages/filter)。

## <a name="see-also"></a>另請參閱

* [Razor Pages 授權慣例](xref:security/authorization/razor-pages-authorization)
