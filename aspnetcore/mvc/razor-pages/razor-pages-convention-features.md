---
title: "Razor 頁面路由和應用程式慣例功能 ASP.NET Core"
author: guardrex
description: "探索如何路由和應用程式模型提供者慣例功能有助於控制頁面路由、 探索與處理。"
keywords: "ASP.NET Core、 Razor 頁面、 慣例，AddFolderRouteModelConvention、 AddPageRouteModelConvention、 AddPageRoute、 AddFolderApplicationModelConvention、 AddPageApplicationModelConvention，ConfigureFilter，篩選"
ms.author: riande
manager: wpickett
ms.date: 10/23/2017
ms.topic: article
ms.assetid: 6b60514c-81ad-485b-bb22-9b71416dff08
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: 81fe5198e25c4275f5cf0a123536a9130be8c1d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a>Razor 頁面路由和應用程式慣例功能 ASP.NET Core

作者：[Luke Latham](https://github.com/guardrex)

了解如何控制頁面路由、 探索與處理 Razor 網頁應用程式中的使用頁面路由和應用程式模型提供者慣例的功能。 當您需要設定自訂頁面路由的個別頁面時，設定路由傳送至含有 pages [AddPageRoute 慣例](#configure-a-page-route)本主題稍後所述。

使用[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample)([如何下載](xref:tutorials/index#how-to-download-a-sample)) 來瀏覽本主題中所述的功能。

| 功能 | 此範例會示範... |
| -------- | --------------------------- |
| [路由和應用程式模型慣例](#add-route-and-app-model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | 加入應用程式的網頁中的路由範本和標頭。 |
| [頁面路由動作慣例](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | 資料夾中的頁面，並在單一頁面，請新增路由範本。 |
| [頁面模型動作慣例](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter （篩選類別、 lambda 運算式或篩選器 factory）</li></ul> | 將標頭加入至資料夾中的頁面，將標頭加入至單一的頁面上，及設定[篩選 factory](xref:mvc/controllers/filters#ifilterfactory)將標頭加入至應用程式的頁面。 |
| [預設頁面應用程式模型提供者](#replace-the-default-page-app-model-provider) | 取代預設頁面模型提供者，若要變更的處理常式命名慣例。 |

## <a name="add-route-and-app-model-conventions"></a>新增路由和應用程式模型慣例

加入的委派[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention)將套用至網頁 Razor 的路由和應用程式模型慣例。

**將路由模型慣例新增至所有頁面**

使用[慣例](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)建立並新增[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)集合[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention)路由和頁面模型期間所套用的執行個體建構。

範例應用程式將`{globalTemplate?}`所有應用程式中頁面的路由範本：

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> `Order`屬性`AttributeRouteModel`設`0`（零）。 這可確保此範本指定的第一個路由資料值位置的優先順序時提供的單一路由值。 例如，將範例加入`{aboutTemplate?}`本主題稍後的路由範本。 `{aboutTemplate?}`範本提供`Order`的`1`。 在 [關於] 頁面要求時`/About/RouteDataValue`，"RouteDataValue 「 載入至`RouteData.Values["globalTemplate"]`(`Order = 0`) 而非`RouteData.Values["aboutTemplate"]`(`Order = 1`) 因為設定`Order`屬性。

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

要求在範例的相關網頁`localhost:5000/About/GlobalRouteValue`並檢查結果：

![關於頁面要求以 GlobalRouteValue 的路由區段。 轉譯的頁面會顯示網頁的 OnGet 方法內擷取了路由資料值。](razor-pages-convention-features/_static/about-page-global-template.png)

**所有網頁新增的應用程式模型慣例**

使用[慣例](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)建立並新增[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)集合[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention)路由和頁面期間所套用的執行個體建構模型。

為了示範這個及其他本主題稍後的慣例，包括範例應用程式`AddHeaderAttribute`類別。 類別建構函式接受`name`字串和`values`字串陣列。 這些值會以其`OnResultExecuting`方法來設定回應標頭。 完整的類別如下所示[頁面模型動作慣例](#page-model-action-conventions)本主題稍後的章節。

範例應用程式會使用`AddHeaderAttribute`類別，以新增標頭， `GlobalHeader`，所有的應用程式中的頁面：

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

要求在範例的相關網頁`localhost:5000/About`和檢查以檢視結果的標頭：

![關於網頁的回應標頭會顯示已加入 GlobalHeader。](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a>頁面路由動作慣例

衍生自預設路由模型提供者[IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider)叫用的設計來提供擴充性點的設定頁面的路由慣例。

**資料夾路徑模型慣例**

使用[AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention)建立並新增[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)會叫用動作上[PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel)所有下頁面指定的資料夾。

範例應用程式會使用`AddFolderRouteModelConvention`新增`{otherPagesTemplate?}`中頁面的路由範本*OtherPages*資料夾：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> `Order`屬性`AttributeRouteModel`設`1`。 如此可確保針對範本`{globalTemplate?}`（稍早在本主題中的設定） 的第一個路由資料值的位置，提供單一路由值時指定優先權。 如果在要求 Page1 頁面`/OtherPages/Page1/RouteDataValue`，"RouteDataValue 「 載入至`RouteData.Values["globalTemplate"]`(`Order = 0`) 而非`RouteData.Values["otherPagesTemplate"]`(`Order = 1`) 因為設定`Order`屬性。

要求在此範例 Page1 頁面`localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue`並檢查結果：

![以路由區段 GlobalRouteValue 和 OtherPagesRouteValue 要求 Page1 OtherPages 資料夾中。 轉譯的頁面會顯示網頁的 OnGet 方法會擷取路由資料值。](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

**頁面路由模型慣例**

使用[AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention)建立並新增[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)會叫用動作上[PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel)與指定頁面名稱。

範例應用程式會使用`AddPageRouteModelConvention`新增`{aboutTemplate?}`關於頁面的路由範本：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> `Order`屬性`AttributeRouteModel`設`1`。 如此可確保針對範本`{globalTemplate?}`（稍早在本主題中的設定） 的第一個路由資料值的位置，提供單一路由值時指定優先權。 如果在要求關於頁面`/About/RouteDataValue`，"RouteDataValue 「 載入至`RouteData.Values["globalTemplate"]`(`Order = 0`) 而非`RouteData.Values["aboutTemplate"]`(`Order = 1`) 因為設定`Order`屬性。

要求在範例的相關網頁`localhost:5000/About/GlobalRouteValue/AboutRouteValue`並檢查結果：

![有關頁面要求的路由區段 GlobalRouteValue 和 AboutRouteValue。 轉譯的頁面會顯示網頁的 OnGet 方法會擷取路由資料值。](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>設定頁面路由

使用[AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute)頁面的路由設定在指定的頁面的路徑。 產生的連結至頁面使用您指定的路由。 `AddPageRoute`使用`AddPageRouteModelConvention`建立路由。

範例應用程式建立的路由`/TheContactPage`如*Contact.cshtml*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

連絡人頁面也可以達到在`/Contact`透過其預設路由。

範例應用程式的自訂路由至 [連絡人] 頁面可讓選擇性`text`路由區段 (`{text?}`)。 這個頁面也包含在這個選擇性區段及其`@page`萬一訪客存取的頁面指示詞其`/Contact`路由：

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

請注意，針對產生 URL**連絡人**中呈現的網頁連結會反映更新的路由：

![導覽列中的範例應用程式，請連絡連結](razor-pages-convention-features/_static/contact-link.png)

![檢查呈現的 HTML 中的 [連絡人] 連結表示 href 設定為 ' / TheContactPage'](razor-pages-convention-features/_static/contact-link-source.png)

請瀏覽 [連絡人] 頁面，在其一般的路由， `/Contact`，或自訂的路由， `/TheContactPage`。 如果您提供額外`text`路由區段頁面會顯示的 HTML 編碼的區段，提供：

![Edge 瀏覽器範例提供的 URL 中的 'TextValue' 選擇性 'text' 的路由區段。 轉譯的頁面會顯示 'text' 區段值。](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>頁面模型動作慣例

預設頁面模型提供者實作[IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider)會叫用的慣例，這設計來提供擴充點設定的頁面模型。 這些慣例會建置和修改頁面探索和處理功能時很有用的。

本節中的範例，範例應用程式會使用`AddHeaderAttribute`類別，這是[ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute)，可套用的回應標頭：

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

使用慣例，此範例會示範如何將屬性套用所有資料夾中的頁面，並在單一頁面。

**資料夾的應用程式模型慣例**

使用[AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention)建立並新增[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)會叫用動作上[PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel)的執行個體指定的資料夾下的所有網頁。

此範例示範如何使用`AddFolderApplicationModelConvention`所新增的標頭， `OtherPagesHeader`，內部網頁*OtherPages*之應用程式資料夾：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

要求在此範例 Page1 頁面`localhost:5000/OtherPages/Page1`和檢查以檢視結果的標頭：

![OtherPages/Page1 頁面的回應標頭會顯示已加入 OtherPagesHeader。](razor-pages-convention-features/_static/page1-otherpages-header.png)

**網頁應用程式模型慣例**

使用[AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention)建立並新增[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)會叫用動作上[PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel)頁面speciifed 名稱。

此範例示範如何使用`AddPageApplicationModelConvention`所新增的標頭， `AboutHeader`，關於頁面：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

要求在範例的相關網頁`localhost:5000/About`和檢查以檢視結果的標頭：

![關於網頁的回應標頭會顯示已加入 AboutHeader。](razor-pages-convention-features/_static/about-page-about-header.png)

**設定的篩選條件**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter)設定指定要套用的篩選條件。 您可以實作的篩選器類別，但是範例應用程式示範如何在 lambda 運算式中，實作幕後作業以傳回篩選條件的處理站實作的篩選器：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

網頁應用程式模型用來檢查導致 Page2 頁面中的區段的相對路徑*OtherPages*資料夾。 如果通過條件，則會加入標頭。 如果沒有，則`EmptyFilter`套用。

`EmptyFilter`是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。 因為動作篩選條件會忽略 Razor 頁面`EmptyFilter`否 ops 如預期般如果路徑未包含`OtherPages/Page2`。

要求在此範例 Page2 頁面`localhost:5000/OtherPages/Page2`和檢查以檢視結果的標頭：

![Page2 OtherPagesPage2Header 新增至回應。](razor-pages-convention-features/_static/page2-filter-header.png)

**設定篩選器 factory**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__)設定要套用指定的 factory[篩選](xref:mvc/controllers/filters)所有 Razor 網頁。

範例應用程式提供的使用範例[篩選 factory](xref:mvc/controllers/filters#ifilterfactory)所新增的標頭， `FilterFactoryHeader`，具有兩個值的應用程式頁面：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

要求在範例的相關網頁`localhost:5000/About`和檢查以檢視結果的標頭：

![關於網頁的回應標頭會顯示已加入兩個 FilterFactoryHeader 標頭。](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>取代預設的網頁應用程式模型提供者

使用 razor 頁面`IPageApplicationModelProvider`介面來建立[DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider)。 您可以繼承自預設模型提供者，以提供您自己的實作邏輯處理常式探索和處理。 預設實作 ([參考來源](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) 建立的慣例*命名*和*名為*處理常式命名，如下所述。

**未命名的處理常式方法的預設值**

HTTP 指令動詞 （「 未命名的 」 的處理常式方法） 的處理常式方法遵循的慣例： `On<HTTP verb>[Async]` (附加`Async`是選用項但建議的非同步方法)。

| 未命名的處理常式方法     | 運算                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | 初始化頁面狀態。     |
| `OnPost`/`OnPostAsync`     | 處理 POST 要求。          |
| `OnDelete`/`OnDeleteAsync` | 處理 DELETE 要求 &#8224;。 |
| `OnPut`/`OnPutAsync`       | 處理 PUT 要求 &#8224;。    |
| `OnPatch`/`OnPatchAsync`   | 處理 PATCH 要求 &#8224;。  |

&#8224;用於 API 呼叫的頁面。

**預設處理常式方法的名稱**

（「 已命名 」 處理常式方法） 的開發人員所提供的處理常式方法會依照類似的慣例。 處理常式名稱會出現在 HTTP 動詞命令之後或之間的 HTTP 動詞命令和`Async`: `On<HTTP verb><handler name>[Async]` (附加`Async`是選用項但建議的非同步方法)。 例如，處理訊息的方法可能會接受命名如下表所示。

| 名為處理常式方法的範例             | 範例作業        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | 取得訊息。        |
| `OnPostMessage`/`OnPostMessageAsync`     | 張貼訊息。          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | 刪除訊息 &#8224;。 |
| `OnPutMessage`/`OnPutMessageAsync`       | 將訊息 &#8224;。    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | 修補訊息 &#8224;。  |

&#8224;用於 API 呼叫的頁面。

**自訂處理常式方法名稱**

假設您想要變更未具名和具名的處理常式方法的命名的方式。 避免方法名稱，以 「 On 」 開頭，並用於判斷 HTTP 動詞命令的第一個文字區段是替代的命名配置。 您可以進行其他變更，例如轉換動詞命令刪除時，PUT 和 POST 的修補程式。 這樣的配置會提供下表所示的方法名稱。

| 處理常式方法                       | 運算                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | 初始化頁面狀態。     |
| `Post`/`PostAsync`                   | 處理 POST 要求。          |
| `Delete`/`DeleteAsync`               | 處理 DELETE 要求 &#8224;。 |
| `Put`/`PutAsync`                     | 處理 PUT 要求 &#8224;。    |
| `Patch`/`PatchAsync`                 | 處理 PATCH 要求 &#8224;。  |
| `GetMessage`                         | 取得訊息。              |
| `PostMessage`/`PostMessageAsync`     | 張貼訊息。                |
| `DeleteMessage`/`DeleteMessageAsync` | 張貼訊息至刪除。      |
| `PutMessage`/`PutMessageAsync`       | 張貼訊息至放置。         |
| `PatchMessage`/`PatchMessageAsync`   | 張貼訊息至修補程式。       |

&#8224;用於 API 呼叫的頁面。

若要建立這種配置，繼承自`DefaultPageApplicationModelProvider`類別並覆寫[CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel)方法，以提供自訂邏輯來解析[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel)處理常式名稱。 範例應用程式會顯示您如何這是其`CustomPageApplicationModelProvider`類別：

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

反白顯示的類別包括：

* 類別繼承自`DefaultPageApplicationModelProvider`。
* `TryParseHandlerMethod`程序來判斷 HTTP 動詞命令的處理常式 (`httpMethod`) 以及具名的處理常式名稱 (`handlerName`) 時建立`PageHandlerModel`。
  * `Async`後置會被忽略，如果有的話。
  * 大小寫用來剖析 HTTP 指令動詞，與方法名稱。
  * 當方法名稱 (不含`Async`) 是 HTTP 動詞命令名稱相同，沒有任何具名的處理常式。 `handlerName`設`null`，方法名稱，且`Get`， `Post`， `Delete`， `Put`，或`Patch`。
  * 當方法名稱 (不含`Async`) 長度超過 HTTP 動詞命令的名稱，具名的處理常式。 `handlerName` 已設為 `<method name (less 'Async', if present)>`。 例如，"GetMessage"和"GetMessageAsync 」 產生 「 GetMessage"的處理常式名稱。
  * DELETE、 PUT 和修補程式的 HTTP 動詞命令會轉換為 POST。

註冊`CustomPageApplicationModelProvider`中`Startup`類別：

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

程式碼後置檔案*Index.cshtml.cs*顯示如何將一般處理常式方法的命名慣例變更應用程式中的頁面。 會移除一般 「 On 」 前置詞命名 Razor 頁面搭配使用。 初始化頁面狀態的方法現在已命名為`Get`。 您可以看到這個慣例用整個應用程式，如果您開啟的任何網頁的任何程式碼後置檔案。

每個其他方法的開頭描述其處理的 HTTP 動詞命令。 開頭為兩個方法`Delete`通常會當做刪除 HTTP 動詞命令，但在邏輯來處理`TryParseHandlerMethod`明確設定為 POST 動詞命令的這兩個處理常式。

請注意，`Async`是選擇性之間`DeleteAllMessages`和`DeleteMessageAsync`。 它們是兩個非同步方法，但您可以選擇使用`Async`後置或未; 我們建議您執行。 `DeleteAllMessages`這裡做為示範用途，但我們建議您命名此類方法`DeleteAllMessagesAsync`。 它不會影響處理的範例實作，但是使用`Async`後置的事實，它會是一個非同步方法的呼叫。

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

請注意中提供的處理常式名稱*Index.cshtml*符合`DeleteAllMessages`和`DeleteMessageAsync`處理常式方法：

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async`處理常式方法名稱中`DeleteMessageAsync`逾時由分解`TryParseHandlerMethod`的處理常式方法的 POST 要求比對。 `asp-page-handler`名稱`DeleteMessage`相符的處理常式方法`DeleteMessageAsync`。

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC 篩選條件和頁面篩選 (IPageFilter)

MVC[動作篩選條件](xref:mvc/controllers/filters#action-filters)Razor 頁面，因為 Razor 頁面使用的處理常式方法會忽略。 其他類型的 MVC 篩選條件可供您使用：[授權](xref:mvc/controllers/filters#authorization-filters)，[例外狀況](xref:mvc/controllers/filters#exception-filters)，[資源](xref:mvc/controllers/filters#resource-filters)，和[結果](xref:mvc/controllers/filters#result-filters)。 如需詳細資訊，請參閱[篩選](xref:mvc/controllers/filters)主題。

頁面篩選 ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) 套用到 Razor 頁面的篩選。 它會包圍網頁處理常式方法的執行。 它可讓您處理自訂程式碼的網頁處理常式方法的執行階段。 以下是範例應用程式中的範例：

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

此篩選條件會檢查`globalTemplate`"ReplacementValue 「 路由 」 TriggerValue"和交換的值。

`ReplaceRouteValueFilter`屬性直接套用`PageModel`中程式碼後置：

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

在使用範例應用程式從要求 Page3 頁面`localhost:5000/OtherPages/Page3/TriggerValue`。 請注意，篩選會路由值的取代：

![要求 OtherPages/Page3 TriggerValue 路由區段在中使用結果取代 ReplacementValue 路由值的篩選條件。](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a>請參閱

* [Razor 頁面授權慣例](xref:security/authorization/razor-pages-authorization)
