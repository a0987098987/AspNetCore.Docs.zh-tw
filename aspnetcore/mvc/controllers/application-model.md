---
title: 在 ASP.NET Core 中使用應用程式模型
author: ardalis
description: 了解如何讀取及操作應用程式模型，來修改 ASP.NET Core 中 MVC 項目的行為方式。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/application-model
ms.openlocfilehash: a0e38b041f428f8b519fd726643b3214761fb44e
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555348"
---
# <a name="work-with-the-application-model-in-aspnet-core"></a>在 ASP.NET Core 中使用應用程式模型

作者：[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC 定義了一個「應用程式模型」，代表 MVC 應用程式的元件。 您可以讀取及操作此模型來修改 MVC 項目的行為。 根據預設，MVC 遵循特定慣例來判斷哪些類別會被視為控制器、對這些類別的哪些方法是動作，以及參數和路由的行為方式。 您可以自訂此行為，以符合您的應用程式需求，方法是建立自己的慣例，並將其全域套用或作為屬性來套用。

## <a name="models-and-providers"></a>模型和提供者

ASP.NET Core MVC 應用程式模型包含抽象介面和描述 MVC 應用程式的具象實作類別。 此模型是 MVC 根據預設慣例來探索應用程式控制器、動作、動作參數、路由和篩選條件的結果。 藉由使用應用程式模型，您可以修改應用程式以遵循與預設 MVC 行為不同的慣例。 參數、名稱、路由和篩選條件全都用作動作與控制器的組態資料。

ASP.NET Core MVC 應用程式模型具有下列結構：

* ApplicationModel
    * 控制器 (ControllerModel)
        * 動作 (ActionModel)
            * 參數 (ParameterModel)

模型的每個層級都可存取共同的 `Properties` 集合，而較低層級可以存取並覆寫階層架構中較高層級所設定的屬性值。 屬性會在建立動作時保存到 `ActionDescriptor.Properties`。 然後當處理要求時，可以透過 `ActionContext.ActionDescriptor.Properties` 來存取慣例所新增或修改的任何屬性。 使用屬性是針對每個動作設定篩選條件及模型繫結器等的好方法。

> [!NOTE]
> 一旦完成應用程式啟動之後，`ActionDescriptor.Properties` 集合不具備執行緒安全 (對於寫入)。 慣例是安全地將資料新增至此集合的最佳方式。

### <a name="iapplicationmodelprovider"></a>IApplicationModelProvider

ASP.NET Core MVC 使用 [IApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) 介面定義的提供者模式來載入應用程式模型。 本節涵蓋此提供者運作方式的一些內部實作詳細資料。 這是進階的主題 - 大部分使用應用程式模型的應用程式都應該遵照慣例這樣做。

`IApplicationModelProvider` 介面的實作會彼此「包裝」，每個實作根據其 `Order` 屬性以遞增順序呼叫 `OnProvidersExecuting`。 然後以相反順序呼叫 `OnProvidersExecuted` 方法。 架構會定義數個提供者：

先是 (`Order=-1000`)：

* [`DefaultApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

然後 (`Order=-990`)：

* [`AuthorizationApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> 具有相同 `Order` 值的兩個提供者的呼叫順序未定義，因此不應依賴它。

> [!NOTE]
> `IApplicationModelProvider` 是讓架構作者擴充的進階概念。 一般情況下，應用程式應該使用慣例，而架構應該使用提供者。 主要的差別是提供者一律在慣例之前先執行。

`DefaultApplicationModelProvider` 建立了 ASP.NET Core MVC 使用的許多預設行為。 其負責的部分包括：

* 將全域篩選條件新增至內容
* 將控制器新增至內容
* 將公用控制器方法新增作為動作
* 將動作方法參數新增至內容
* 套用路由和其他屬性

某些內建行為由 `DefaultApplicationModelProvider` 實作。 此提供者負責建構 [`ControllerModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel)，而它則會參考 [`ActionModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel)、[`PropertyModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel) 和 [`ParameterModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) 執行個體。 `DefaultApplicationModelProvider` 類別是內部架構實作詳細資料，未來將會變更。 

`AuthorizationApplicationModelProvider` 負責套用與 `AuthorizeFilter` 和 `AllowAnonymousFilter` 屬性建立關聯的行為。 [進一步了解這些屬性](xref:security/authorization/simple)。

`CorsApplicationModelProvider` 實作與 `IEnableCorsAttribute` 和 `IDisableCorsAttribute` 建立關聯的行為，以及 `DisableCorsAuthorizationFilter`。 [進一步了解 CORS](xref:security/cors)。

## <a name="conventions"></a>慣例

應用程式模型定義了慣例抽象化，提供比覆寫整個模型或提供者更簡單的方式，來自訂模型的行為。 這些抽象化是用來修改您應用程式行為的建議方式。 慣例會提供方式讓您撰寫程式碼，動態地套用自訂。 雖然[篩選條件](xref:mvc/controllers/filters)提供方法來修改架構的行為，但自訂可讓您控制整個應用程式如何連接在一起。

可用的慣例如下：

* [`IApplicationModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

藉由將慣例新增至 MVC 選項或實作 `Attribute` 並將其套用至控制器、動作或動作參數 (類似於 [`Filters` ](xref:mvc/controllers/filters)) 來套用慣例。 與篩選條件不同的是，只有在應用程式啟動時才會執行慣例，而不會在每個要求當中執行。

### <a name="sample-modifying-the-applicationmodel"></a>範例：修改 ApplicationModel

下列慣例用來將屬性新增至應用程式模型。 

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

在 `Startup` 中的 `ConfigureServices` 中新增 MVC 時，應用程式模型慣例會套用為選項。

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

屬性可從控制器動作中的 `ActionDescriptor` 屬性集合存取：

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a>範例：修改 ControllerModel 描述

如同先前的範例，控制器模型也可以修改為包含自訂屬性。 這些會使用應用程式模型中指定的相同名稱，來覆寫現有的屬性。 下列慣例屬性會在控制器層級新增描述：

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

這個慣例會套用為控制器上的屬性。

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

"description" 屬性的存取方式與先前範例相同。

### <a name="sample-modifying-the-actionmodel-description"></a>範例：修改 ActionModel 描述

個別的屬性規格可以套用至個別的動作，且已在應用程式或控制器層級套用覆寫行為。

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

將此套用到先前範例控制器內的動作，將示範如何覆寫控制器層級慣例：

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a>範例：修改 ParameterModel

下列的慣例可以套用至動作參數，以修改其 `BindingInfo`。 下列慣例需要參數是路由參數；其他可能的繫結來源 (例如查詢字串值) 都會被忽略。

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

屬性可套用至任何動作參數：

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a>範例：修改 ActionModel 名稱

下列慣例會修改 `ActionModel` 以更新其所套用之動作的「名稱」。 新的名稱會當作傳給屬性的參數。 這個新名稱由路由使用，因此它會影響用來連線到此動作方法的路由。

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

這個屬性會套用至 `HomeController` 中的動作方法：

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

即使方法名稱是 `SomeName`，屬性仍會覆寫使用方法名稱的 MVC 慣例，並將動作名稱取代為 `MyCoolAction`。 因此，用來連線到此動作的路由是 `/Home/MyCoolAction`。

> [!NOTE]
> 這個範例基本上與使用內建 [ActionName](/dotnet/api/microsoft.aspnetcore.mvc.actionnameattribute) 屬性相同。

### <a name="sample-custom-routing-convention"></a>範例：自訂路由慣例

您可以使用 `IApplicationModelConvention` 來自訂路由的運作方式。 例如，下列慣例會在路由中併入控制器的命名空間，並將命名空間中的 `.` 在路由中取代為 `/`：

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

慣例會新增為 Startup 中的選項。

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> 您可以藉由使用 `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));` 存取 `MvcOptions`，將慣例新增到您的[中介軟體](xref:fundamentals/middleware/index)

這個範例將此慣例套用到不使用控制器名稱中包含 "Namespace" 之屬性路由的路由。 下列控制器示範此慣例：

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a>WebApiCompatShim 中的應用程式模型使用方式

ASP.NET Core MVC 會使用與 ASP.NET Web API 2 不同的一組慣例。 您可以使用自訂慣例，來修改 ASP.NET Core MVC 應用程式的行為，以便與 Web API 應用程式一致。 Microsoft 特別針對這個用途而隨附 [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/)。

> [!NOTE]
> 深入了解[從 ASP.NET Web API 移轉](xref:migration/webapi)。

若要使用 Web API 相容性填充碼，您必須將套件新增到您的專案，然後藉由在 `Startup` 中呼叫 `AddWebApiConventions` 來新增慣例：

```c#
services.AddMvc().AddWebApiConventions();
```

填充碼提供的慣例僅會套用到已套用特定屬性的應用程式組件。 下列四個屬性用來控制哪些控制器應該讓其慣例由填充碼修改：

* [UseWebApiActionConventionsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [UseWebApiOverloadingAttribute](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [UseWebApiParameterConventionsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [UseWebApiRoutesAttribute](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a>動作慣例

`UseWebApiActionConventionsAttribute` 用來根據名稱將 HTTP 方法對應到動作 (例如，`Get` 會對應至 `HttpGet`)。 它只適用於不使用屬性路由的動作。

### <a name="overloading"></a>多載化

`UseWebApiOverloadingAttribute` 用於套用 `WebApiOverloadingApplicationModelConvention` 慣例。 這個慣例會將 `OverloadActionConstraint` 新增至動作選取程序，這將候選項目動作限制為要求符合其所有非選擇性參數的動作。

### <a name="parameter-conventions"></a>參數慣例

`UseWebApiParameterConventionsAttribute` 用於套用 `WebApiParameterConventionsApplicationModelConvention` 動作慣例。 這個慣例指定用來作為動作參數的簡單類型預設會從 URL 繫結，而複雜類型則會從要求主體繫結。

### <a name="routes"></a>路由

`UseWebApiRoutesAttribute` 控制是否套用 `WebApiApplicationModelConvention` 控制器慣例。 啟用時，這個慣例會用來將[區域](xref:mvc/controllers/areas)的支援新增到路由。

除了一組慣例之外，相容性套件還包括 `System.Web.Http.ApiController` 基底類別，以取代 Web API 提供的類別。 這可讓您針對 Web API 撰寫且繼承自其 `ApiController` 的控制器如同設計般地運作，同時在 ASP.NET Core MVC 上執行。 這個基底控制器類別裝飾了上面所列的所有 `UseWebApi*` 屬性。 `ApiController` 會公開屬性、方法和與 Web API 中之結果類型相容的結果類型。

## <a name="using-apiexplorer-to-document-your-app"></a>使用 ApiExplorer 記載您的應用程式

應用程式模型會在可用來周遊應用程式結構的每個層級公開 [`ApiExplorer`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) 屬性。 這可以用來[使用 Swagger 等工具為您的 Web API 產生說明頁面](xref:tutorials/web-api-help-pages-using-swagger)。 `ApiExplorer` 屬性會公開 `IsVisible` 屬性，它可以設定來指定應該公開您應用程式模型中的哪些部分。 您可以使用慣例來設定這項設定：

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

使用此方法 (和其他必要的慣例)，您可以啟用或停用應用程式內任何層級的 API 可見度。 
