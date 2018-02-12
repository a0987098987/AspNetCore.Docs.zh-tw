---
title: "ASP.NET Core 的應用程式組件"
author: ardalis
description: "了解如何使用應用程式組件 (也就是應用程式資源的抽象概念)、設定您的應用程式以探索或避免載入組件的功能。"
manager: wpickett
ms.author: riande
ms.date: 01/04/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 6b855f8725dacc89a7e0607224ef3c19ab9f5676
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="application-parts-in-aspnet-core"></a>ASP.NET Core 的應用程式組件

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

「應用程式組件」是應用程式資源的抽象概念，其中您可以探索到控制器、檢視元件或標籤協助程式等 MVC 功能。 應用程式組件的其中一個範例是 AssemblyPart，其會封裝組件參考，並將類型與編譯參考公開。 「功能提供者」會使用應用程式組件，來填入 ASP.NET Core MVC 應用程式的功能。 應用程式組件的主要使用案例是讓您設定應用程式，以探索 (或避免載入) 組件中的 MVC 功能。

## <a name="introducing-application-parts"></a>應用程式組件簡介

MVC 應用程式會從[應用程式組件](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart)載入其功能。 值得注意的是，[AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) 類別代表組件所支援的應用程式組件。 您可以使用這些類別來探索及載入 MVC 功能，例如控制器、檢視元件、標籤協助程式和 Razor 編譯來源。 [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) 負責追蹤適用於 MVC 應用程式的應用程式組件和功能提供者。 您可以在設定 MVC 時與 `Startup` 中的 `ApplicationPartManager` 互動：

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

MVC 預設會搜尋相依性樹狀結構，並尋找控制器 (即使位於其他組件中)。 您可以使用應用程式組件，來載入任意組件 (例如來自未於編譯時期參考的外掛程式)。

您可以使用應用程式組件，來「避免」尋找特定組件或位置中的控制器。 藉由修改 `ApplicationPartManager` 的 `ApplicationParts` 集合，您可以控制要提供給應用程式哪些組件。 `ApplicationParts` 集合中的項目順序並不重要。 請務必完全設定 `ApplicationPartManager` 之後，再用它來設定容器中的服務。 例如，您應該完全設定 `ApplicationPartManager` 之後，再叫用 `AddControllersAsServices`。 如果您沒有這麼做，於呼叫該方法之後才新增的應用程式組件控制器就不會受到影響 (亦無法註冊為服務)，而這可能會造成應用程式的行為異常。

如果組件包含您不想使用的控制器，請將它從 `ApplicationPartManager` 移除：

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

除了專案的組件和其相依組件，`ApplicationPartManager` 還預設包含 `Microsoft.AspNetCore.Mvc.TagHelpers` 和 `Microsoft.AspNetCore.Mvc.Razor` 的組件。

## <a name="application-feature-providers"></a>應用程式功能提供者

應用程式功能提供者會檢查應用程式組件，並對這些組件提供功能。 下列 MVC 功能均內建功能提供者：

* [控制器](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [中繼資料參考](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [標記協助程式](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [檢視元件](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

繼承自 `IApplicationFeatureProvider<T>` 的功能提供者，其中 `T` 是功能的類型。 針對上述所列的任何 MVC 功能類型，您可以實作自己的功能提供者。 在 `ApplicationPartManager.FeatureProviders` 集合中，功能提供者順序可能相當重要，因為更新版本的提供者可以依據舊版提供者所採取的動作做出回應。

### <a name="sample-generic-controller-feature"></a>範例：泛型控制器功能

根據預設，ASP.NET Core MVC 會忽略泛型控制器 (例如 `SomeController<T>`)。 這個範例使用的控制器功能提供者，會在預設提供者之後執行，並新增指定類型清單 (定義於 `EntityTypes.Types`) 的泛型控制器執行個體：

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

實體類型：

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

系統會將功能提供者新增至 `Startup`：

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

根據預設，用於路由的泛用控制器名稱格式為 *GenericController'1[Widget]* 而不是 *Widget*。 您可以使用下列屬性修改名稱，以對應至控制器所使用的泛型型別：

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` 類別：

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

要求相符路由時的結果為：

![來自範例應用程式的輸出範例會顯示 'Hello from a generic Sproket controller'。](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>範例：顯示可用的功能

您可以透過[相依性插入](../../fundamentals/dependency-injection.md)要求 `ApplicationPartManager`，並使用它來填入適當的功能執行個體，以逐一查看適用於應用程式的已填入功能：

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

範例輸出：

![來自範例應用程式的範例輸出](app-parts/_static/available-features.png)
