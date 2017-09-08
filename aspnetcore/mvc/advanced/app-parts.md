---
title: "在 ASP.NET Core 應用程式組件"
author: ardalis
description: "了解如何使用應用程式組件，也就是 abstrations 透過應用程式的資源，設定您的應用程式探索，或避免功能載入的組件。"
keywords: "ASP.NET Core 應用程式組件、 應用程式組件"
ms.author: riande
manager: wpickett
ms.date: 1/4/2017
ms.topic: article
ms.assetid: b355a48e-a15c-4d58-b69c-899963613a98
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: a5205ebab6c827b4e6af63287e56fe2b8f72c933
ms.sourcegitcommit: 418e6aa4ab79474ecc4d0a6af573a3759b113fe4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2017
---
# <a name="application-parts-in-aspnet-core"></a>在 ASP.NET Core 應用程式組件

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample)

*應用程式部分*是的抽象概念的應用程式中，然後再從中 MVC 等功能的控制站，檢視元件的資源，或可能會發現標記協助程式。 應用程式一部分的其中一個範例是 AssemblyPart，封裝組件參考和公開型別和編譯的參考。 *功能提供者*使用應用程式組件，以填入 ASP.NET Core MVC 應用程式的功能。 主要使用案例的應用程式組件可讓您設定您的應用程式探索 （或避免載入） MVC 組件中的功能。

## <a name="introducing-application-parts"></a>介紹應用程式組件

MVC 應用程式載入從其功能[應用程式組件](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart)。 特別是， [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart)類別代表組件所支援的應用程式部分。 您可以使用這些類別來探索及載入 MVC 功能，例如控制器、 檢視元件、 標記協助程式和 razor 編譯來源。 [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager)負責追蹤 MVC 應用程式的應用程式組件和功能提供者。 您可以互動`ApplicationPartManager`中`Startup`MVC 的設定時：

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

根據預設 MVC 會搜尋相依性樹狀結構，並尋找控制站 （即使在其他組件）。 若要載入的任意組件 （比方說，外掛程式不是參考在編譯時期），您可以使用應用程式部分。

您可以使用應用程式組件，以便*避免*找尋特定組件或位置中控制站。 您可以控制哪些組件 （或組件） 可用應用程式藉由修改`ApplicationParts`集合`ApplicationPartManager`。 中的項目順序`ApplicationParts`集合並不重要。 請務必完全設定`ApplicationPartManager`再用來設定服務容器中。 例如，您應該完全設定`ApplicationPartManager`叫用之前`AddControllersAsServices`。 這麼做，代表將會確認應用程式組件中的控制站新增之後將不會影響方法呼叫 （將未取得註冊為服務） 這可能會造成不正確的 bevavior 應用程式。

如果您有包含控制站，您不想要使用的組件，將它從移除`ApplicationPartManager`:

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

除了專案的組件和其相依的組件，`ApplicationPartManager`將包含組件，以便`Microsoft.AspNetCore.Mvc.TagHelpers`和`Microsoft.AspNetCore.Mvc.Razor`預設。

## <a name="application-feature-providers"></a>應用程式功能提供者

應用程式功能提供者會檢查應用程式組件，並對這些組件提供的功能。 有下列的 MVC 功能內建功能提供者：

* [控制站](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [中繼資料參考](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [標記協助程式](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [檢視元件](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

功能提供者繼承自`IApplicationFeatureProvider<T>`，其中`T`功能的類型。 您可以實作您自己上面所列的任何 MVC 的功能類型提供者的功能。 功能中的提供者順序`ApplicationPartManager.FeatureProviders`集合可能會很重要，因為更新的提供者可以做出回應之前的提供者所採取的動作。

### <a name="sample-generic-controller-feature"></a>範例： 泛型控制器功能

根據預設，ASP.NET Core MVC 會忽略一般控制站 (例如， `SomeController<T>`)。 這個範例會使用預設提供者之後執行，並將指定之清單的類型的泛型控制器執行個體的控制站功能提供者 (定義於`EntityTypes.Types`):

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

實體類型：

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

功能提供者就會加入`Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

根據預設，用於路由的泛用的控制器名稱可能會在表單的*GenericController'1 [Widget]*而不是*Widget*。 下列屬性用來修改要對應至控制器所使用的泛型類型的名稱：

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController`類別：

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

結果中，要求相符的路由時：

![輸出範例應用程式中的範例會讀取的 'Hello 從泛型 Sproket 控制器'。](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>範例： 顯示可用的功能

您可以逐一填入可用的功能加入至應用程式要求`ApplicationPartManager`透過[相依性插入](../../fundamentals/dependency-injection.md)並使用它來填入適當的功能的執行個體：

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

範例輸出：

![範例應用程式中的範例輸出](app-parts/_static/available-features.png)
