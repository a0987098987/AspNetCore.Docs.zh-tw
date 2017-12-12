---
title: "使用應用程式模型"
author: ardalis
description: 
keywords: "ASP.NET Core,ASP.NET 核心 MVC 應用程式模型"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4eb7e52f-5665-41a4-a3e3-e348d07337f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/application-model
ms.openlocfilehash: 3c35184921dbe26cde100fd3d5124e38ea0d06cf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="working-with-the-application-model"></a>使用應用程式模型

由[Steve Smith](https://ardalis.com/)

定義 ASP.NET Core MVC*應用程式模型*代表 MVC 應用程式的元件。 您可以讀取及操作此模型來修改 MVC 元件的行為方式。 根據預設，MVC 會遵循特定慣例，來判斷哪些類別會被視為控制站，這些類別的方法是動作，以及參數和路由的行為方式。 您可以自訂此行為，以符合您的應用程式需求建立您自己的慣例，並將其套用全域方式或屬性為根據。

## <a name="models-and-providers"></a>模型和提供者

ASP.NET Core MVC 應用程式模型包含抽象介面和說明 MVC 應用程式的具象實作類別。 此模型是 MVC 應用程式的控制器、 動作、 動作參數、 路由和篩選器，根據預設慣例探索結果。 藉由使用應用程式模型，您可以修改您的應用程式遵循預設 MVC 行為的不同慣例。 參數、 名稱、 路由和篩選所有可用做為組態資料的動作與控制器。

ASP.NET Core MVC 應用程式模型具有下列結構：

* ApplicationModel
    * 控制站 (ControllerModel)
        * 動作 (ActionModel)
            * 參數 (ParameterModel)

模型的每個層級對一般存取`Properties`集合中，而較低層級可以存取，並覆寫階層架構中較高的層級所設定的屬性值。 屬性會保存到`ActionDescriptor.Properties`建立動作時。 然後時在處理要求時，任何屬性的加入或修改的慣例可以透過存取`ActionContext.ActionDescriptor.Properties`。 使用屬性會設定您的篩選器、 模型繫結器等每個動作為基礎的好方法。

> [!NOTE]
> `ActionDescriptor.Properties`集合不是安全執行緒 （適用於寫入） 應用程式啟動完成。 慣例會安全地將資料加入至這個集合的最佳方式。

### <a name="iapplicationmodelprovider"></a>IApplicationModelProvider

ASP.NET Core MVC 載入應用程式模型使用定義的提供者模式[IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider)介面。 本節涵蓋一些內部實作詳細資料的方式這個提供者的功能。 這是進階的主題-大部分應用程式使用之應用程式模型都應該這樣做所使用的慣例。

實作`IApplicationModelProvider`介面 「 包裝 」，與每個實作呼叫`OnProvidersExecuting`以遞增順序根據其`Order`屬性。 `OnProvidersExecuted`然後以相反順序呼叫方法。 架構會定義數個提供者：

第一個 (`Order=-1000`):

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

然後 (`Order=-990`):

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> 具有相同值的兩個提供者中的順序`Order`稱為未定義，並因此不應依賴時。

> [!NOTE]
> `IApplicationModelProvider`是擴充的架構作者進階的概念。 一般情況下，應用程式應該使用的慣例，因此架構應該使用提供者。 主要的差別是提供者，一律執行之前慣例。

`DefaultApplicationModelProvider`建立許多使用 ASP.NET Core MVC 的預設行為。 其負責的部分包括：

* 將全域篩選條件加入至內容
* 將控制站新增至內容
* 新增公用控制器方法當做動作
* 將動作方法參數加入至內容
* 套用路由和其他屬性

某些內建行為由實作`DefaultApplicationModelProvider`。 此提供者會負責建構[ `ControllerModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel)，會接著參考[ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel)， [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel)，和[`ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel)執行個體。 `DefaultApplicationModelProvider`類別是可以的會在未來變更的內部架構實作詳細資料。 

`AuthorizationApplicationModelProvider`會負責套用與相關聯的行為`AuthorizeFilter`和`AllowAnonymousFilter`屬性。 [深入了解這些屬性](xref:security/authorization/simple)。

`CorsApplicationModelProvider`實作相關聯的行為`IEnableCorsAttribute`和`IDisableCorsAttribute`，而`DisableCorsAuthorizationFilter`。 [深入了解 CORS](xref:security/cors)。

## <a name="conventions"></a>慣例

應用程式模型定義慣例的抽象概念，提供更簡單的方式，自訂的模型，而不覆寫整個模型或提供者的行為。 這些抽象化是建議用來修改您的應用程式行為。 慣例會提供您撰寫程式碼，將會動態地套用自訂的方式。 雖然[篩選](xref:mvc/controllers/filters)提供方法來修改架構的行為，自訂項目可讓您控制如何整個應用程式會連接在一起。

可使用下列慣例：

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

將它們加入至 MVC 選項，或實作，會套用慣例`Attribute`s，並將其套用至控制器、 動作或動作的參數 (類似於[ `Filters` ](xref:mvc/controllers/filters))。 與篩選不同的是應用程式啟動時，不會為每個要求的一部分時，才會執行慣例。

### <a name="sample-modifying-the-applicationmodel"></a>範例： 修改 ApplicationModel

慣例用來將屬性加入至應用程式模型。 

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

MVC 加入時，將會套用做為選項的應用程式模型慣例`ConfigureServices`中`Startup`。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

屬性是從存取`ActionDescriptor`控制器動作中的屬性集合：

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a>範例： 修改 ControllerModel 描述

如同先前的範例中，控制器模型也可以修改為包含自訂屬性。 這些會與應用程式模型中指定的相同名稱，覆寫現有的屬性。 下列慣例屬性新增了描述，在控制器層級：

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

這個慣例會套用做為控制站上的屬性。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

如先前範例所示的相同方式存取 [描述] 屬性。

### <a name="sample-modifying-the-actionmodel-description"></a>範例： 修改 ActionModel 描述

個別的屬性規格可以套用至個別的動作，覆寫已套用到應用程式或控制器層級的行為。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

將此套用到先前的範例控制器內的動作將示範如何覆寫的控制器層級慣例：

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a>範例： 修改 ParameterModel

下列的慣例可以套用至動作參數，若要修改其`BindingInfo`。 下列慣例需要的參數是路由參數;其他可能的繫結來源 （例如查詢字串值） 都會被忽略。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

屬性會套用至任何動作參數：

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a>範例： 修改 ActionModel 名稱

修改下列慣例`ActionModel`更新*名稱*来套用的動作。 提供新的名稱做為參數的屬性。 這個新名稱會使用路由，因此它會影響用來連線到此動作方法的路由。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

這個屬性會套用至動作方法中`HomeController`:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

即使方法名稱是`SomeName`，屬性會使用方法名稱的 MVC 慣例覆寫並取代的動作名稱與`MyCoolAction`。 因此，用來連線到此動作的路由是`/Home/MyCoolAction`。

> [!NOTE]
> 這個範例基本上就是使用內建[ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute)屬性。

### <a name="sample-custom-routing-convention"></a>範例： 自訂路由慣例

您可以使用`IApplicationModelConvention`自訂如何路由的運作。 例如，下列慣例會併入控制站的命名空間的路由，取代`.`以命名空間中`/`路由中：

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

慣例是在啟動的選項加入。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> 您可以加入慣例，以便您[中介軟體](xref:fundamentals/middleware)藉由存取`MvcOptions`使用`services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`

這個範例適用於這個慣例不使用屬性路由控制器之 「 命名空間 」 在其名稱的路由。 下列的控制器會示範這個慣例：

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a>WebApiCompatShim 中的應用程式模型使用方式

ASP.NET Core MVC 會使用一組不同的慣例從 ASP.NET Web API 2。 您可以使用自訂的慣例，來修改 ASP.NET Core MVC 應用程式的行為與 Web API 應用程式的一致。 Microsoft 隨附[WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/)專用於此用途。

> [!NOTE]
> 深入了解[從 ASP.NET Web API 移轉](xref:migration/webapi)。

若要使用 Web 應用程式開發介面相容性填充碼，您必須將封裝加入您的專案，然後新增藉由呼叫 MVC 慣例`AddWebApiConventions`中`Startup`:

```c#
services.AddMvc().AddWebApiConventions();
```

提供的填充碼的慣例僅適用於應用程式都已經套用至其特定屬性的組件。 下列四個屬性可用來控制哪些控制站應該有其填充碼的慣例所修改的慣例：

* [UseWebApiActionConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [UseWebApiOverloadingAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [UseWebApiParameterConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [UseWebApiRoutesAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a>動作慣例

`UseWebApiActionConventionsAttribute`用來根據其名稱的動作對應的 HTTP 方法 (例如，`Get`會將對應至`HttpGet`)。 它只適用於不使用路由屬性的動作。

### <a name="overloading"></a>多載化

`UseWebApiOverloadingAttribute`用於套用`WebApiOverloadingApplicationModelConvention`慣例。 這個慣例新增`OverloadActionConstraint`動作選取程序，以其限制為其要求符合所有的非選擇性參數的候選項目動作。

### <a name="parameter-conventions"></a>參數慣例

`UseWebApiParameterConventionsAttribute`用於套用`WebApiParameterConventionsApplicationModelConvention`動作慣例。 這個慣例指定簡單型別做為動作參數會繫結從 URI 根據預設，而複雜型別會從要求主體繫結。

### <a name="routes"></a>路由

`UseWebApiRoutesAttribute`控制項是否`WebApiApplicationModelConvention`控制器慣例會套用。 啟用時，這個慣例可用來將支援[區域](xref:mvc/controllers/areas)路由。

相容性封裝包括一組慣例，除了`System.Web.Http.ApiController`基底類別，以取代提供的 Web API。 這可讓您撰寫的 Web API 和繼承的控制站從其`ApiController`運作，如同它們設計，ASP.NET Core MVC 上執行時運作。 這個基底控制器類別使用的所有裝飾`UseWebApi*`上面所列的屬性。 `ApiController`公開屬性、 方法和相容的 Web API 中的結果型別。

## <a name="using-apiexplorer-to-document-your-app"></a>使用 ApiExplorer 記載您的應用程式

應用程式模型會公開[ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel)可用來周遊應用程式的結構，每個層級的屬性。 這可以用來[您 Web 應用程式開發介面使用 Swagger 等工具產生的說明頁面](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger)。 `ApiExplorer`屬性會公開`IsVisible`可以指定應該公開您的應用程式模型中哪些部分設定的屬性。 您可以設定這項設定使用的慣例：

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

使用此方法 （和其他必要的慣例），您可以啟用或停用應用程式內的任何層級的應用程式開發介面可見性。 
