---
title: ASP.NET Core 的應用程式組件
author: rick-anderson
description: 與中的應用程式元件共用控制器、視圖、Razor Pages 等等 ASP.NET Core
ms.author: riande
ms.date: 11/10/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 92c408adfad8af3dfd2572b625ae28ef24f64948
ms.sourcegitcommit: a7bbe3890befead19440075b05b9674351f98872
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905757"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a>在 ASP.NET Core 中與應用程式元件共用控制器、視圖、Razor Pages 和更多

::: moniker range=">= aspnetcore-3.0"

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

*應用程式元件*是應用程式資源的抽象概念。 應用程式元件可讓 ASP.NET Core 探索控制器、視圖元件、標記協助程式、Razor Pages、Razor 編譯來源等等。 [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart)是一個應用程式元件。 `AssemblyPart` 封裝元件參考，並公開類型和編譯參考。

*功能提供者*會使用應用程式元件來填入 ASP.NET Core 應用程式的功能。 應用程式元件的主要使用案例是將應用程式設定為從元件中探索（或避免載入） ASP.NET Core 功能。 例如，您可能會想要在多個應用程式之間共用一般功能。 使用應用程式元件時，您可以與多個應用程式共用包含控制器、視圖、Razor Pages、Razor 編譯來源、標記協助程式等元件（DLL）。 在多個專案中複製程式碼時，偏好共用元件。

ASP.NET Core 應用程式會從 <xref:System.Web.WebPages.ApplicationPart>載入功能。 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> 類別代表元件所支援的應用程式元件。

## <a name="load-aspnet-core-features"></a>載入 ASP.NET Core 功能

使用 `ApplicationPart` 和 `AssemblyPart` 類別來探索和載入 ASP.NET Core 功能（控制器、視圖元件等等）。 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> 會追蹤可用的應用程式元件和功能提供者。 `ApplicationPartManager` 是在 `Startup.ConfigureServices`中設定：

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

下列程式碼提供使用 `AssemblyPart`設定 `ApplicationPartManager` 的替代方法：

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

上述兩個程式碼範例會從元件載入 `SharedController`。 `SharedController` 不在應用程式的專案中。 請參閱[WebAppParts 解決方案](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts)範例下載。

### <a name="include-views"></a>包含視圖

若要在元件中包含 views：

* 將下列標記新增至共用專案檔：

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* 將 <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> 新增至 <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>：

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a>防止載入資源

應用程式元件可以用來*避免*在特定元件或位置中載入資源。 新增或移除 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> 集合的成員，以隱藏或提供可用的資源。 `ApplicationParts` 集合中的項目順序並不重要。 先設定 `ApplicationPartManager`，然後再使用它來設定容器中的服務。 例如，先設定 `ApplicationPartManager`，再叫用 `AddControllersAsServices`。 呼叫 `ApplicationParts` 集合上的 `Remove` 以移除資源。

下列程式碼會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> 從應用程式移除 `MyDependentLibrary`： [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]

`ApplicationPartManager` 包括下列各部分：

* 應用程式的元件和相依元件。
* `Microsoft.AspNetCore.Mvc.TagHelpers`
* `Microsoft.AspNetCore.Mvc.Razor`

## <a name="application-feature-providers"></a>應用程式功能提供者

應用程式功能提供者會檢查應用程式元件，並為這些元件提供功能。 下列 ASP.NET Core 功能有內建的功能提供者：

* [控制器](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [標記協助程式](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [檢視元件](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

繼承自 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1> 的功能提供者，其中 `T` 是功能的類型。 功能提供者可以針對任何先前列出的功能類型來執行。 `ApplicationPartManager.FeatureProviders` 中的功能提供者順序可能會影響執行時間行為。 稍後新增的提供者可以回應先前新增的提供者所採取的動作。

### <a name="generic-controller-feature"></a>泛型控制器功能

ASP.NET Core 會忽略[泛型控制器](/dotnet/csharp/programming-guide/generics/generic-classes)。 泛型控制器具有型別參數（例如，`MyController<T>`）。 下列範例會針對指定的類型清單加入泛型控制器實例：

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

類型定義于 `EntityTypes.Types`中：

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

系統會將功能提供者新增至 `Startup`：

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

用於路由的一般控制器名稱的格式為*GenericController ' 1 [widget]* ，而不是*Widget*。 下列屬性會修改名稱，以對應至控制器所使用的泛型型別：

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` 類別：

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

例如，要求 `https://localhost:5001/Sprocket` 的 URL 會產生下列回應：

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a>顯示可用的功能

透過相依性[插入](../../fundamentals/dependency-injection.md)來要求 `ApplicationPartManager`，即可列舉應用程式可用的功能：

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2)會使用上述程式碼來顯示應用程式功能：

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

*應用程式元件*是應用程式資源的抽象概念。 應用程式元件可讓 ASP.NET Core 探索控制器、視圖元件、標記協助程式、Razor Pages、Razor 編譯來源等等。 [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart)是一個應用程式元件。 `AssemblyPart` 封裝元件參考，並公開類型和編譯參考。

*功能提供者*會使用應用程式元件來填入 ASP.NET Core 應用程式的功能。 應用程式元件的主要使用案例是將應用程式設定為從元件中探索（或避免載入） ASP.NET Core 功能。 例如，您可能會想要在多個應用程式之間共用一般功能。 使用應用程式元件時，您可以與多個應用程式共用包含控制器、視圖、Razor Pages、Razor 編譯來源、標記協助程式等元件（DLL）。 在多個專案中複製程式碼時，偏好共用元件。

ASP.NET Core 應用程式會從 <xref:System.Web.WebPages.ApplicationPart>載入功能。 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> 類別代表元件所支援的應用程式元件。

## <a name="load-aspnet-core-features"></a>載入 ASP.NET Core 功能

使用 `ApplicationPart` 和 `AssemblyPart` 類別來探索和載入 ASP.NET Core 功能（控制器、視圖元件等等）。 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> 會追蹤可用的應用程式元件和功能提供者。 `ApplicationPartManager` 是在 `Startup.ConfigureServices`中設定：

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

下列程式碼提供使用 `AssemblyPart`設定 `ApplicationPartManager` 的替代方法：

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

上述兩個程式碼範例會從元件載入 `SharedController`。 `SharedController` 不在應用程式的專案中。 請參閱[WebAppParts 解決方案](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts)範例下載。

### <a name="include-views"></a>包含視圖

若要在元件中包含 views：

* 將下列標記新增至共用專案檔：

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* 將 <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> 新增至 <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>：

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a>防止載入資源

應用程式元件可以用來*避免*在特定元件或位置中載入資源。 新增或移除 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> 集合的成員，以隱藏或提供可用的資源。 `ApplicationParts` 集合中的項目順序並不重要。 先設定 `ApplicationPartManager`，然後再使用它來設定容器中的服務。 例如，先設定 `ApplicationPartManager`，再叫用 `AddControllersAsServices`。 呼叫 `ApplicationParts` 集合上的 `Remove` 以移除資源。

下列程式碼會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> 從應用程式移除 `MyDependentLibrary`： [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]

`ApplicationPartManager` 包括下列各部分：

* 應用程式的元件和相依元件。
* `Microsoft.AspNetCore.Mvc.TagHelpers`
* `Microsoft.AspNetCore.Mvc.Razor`

## <a name="application-feature-providers"></a>應用程式功能提供者

應用程式功能提供者會檢查應用程式元件，並為這些元件提供功能。 下列 ASP.NET Core 功能有內建的功能提供者：

* [控制器](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [標記協助程式](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [檢視元件](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

繼承自 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1> 的功能提供者，其中 `T` 是功能的類型。 功能提供者可以針對任何先前列出的功能類型來執行。 `ApplicationPartManager.FeatureProviders` 中的功能提供者順序可能會影響執行時間行為。 稍後新增的提供者可以回應先前新增的提供者所採取的動作。

### <a name="generic-controller-feature"></a>泛型控制器功能

ASP.NET Core 會忽略[泛型控制器](/dotnet/csharp/programming-guide/generics/generic-classes)。 泛型控制器具有型別參數（例如，`MyController<T>`）。 下列範例會針對指定的類型清單加入泛型控制器實例：

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

類型定義于 `EntityTypes.Types`中：

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

系統會將功能提供者新增至 `Startup`：

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

用於路由的一般控制器名稱的格式為*GenericController ' 1 [widget]* ，而不是*Widget*。 下列屬性會修改名稱，以對應至控制器所使用的泛型型別：

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` 類別：

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

例如，要求 `https://localhost:5001/Sprocket` 的 URL 會產生下列回應：

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a>顯示可用的功能

透過相依性[插入](../../fundamentals/dependency-injection.md)來要求 `ApplicationPartManager`，即可列舉應用程式可用的功能：

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2)會使用上述程式碼來顯示應用程式功能：

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

::: moniker-end
