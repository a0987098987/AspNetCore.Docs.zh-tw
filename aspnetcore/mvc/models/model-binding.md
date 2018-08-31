---
title: ASP.NET Core 中的資料繫結
author: tdykstra
description: 了解 ASP.NET Core MVC 中的模型繫結如何將 HTTP 要求中的資料對應至動作方法參數。
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: tdykstra
ms.date: 08/14/2018
uid: mvc/models/model-binding
ms.openlocfilehash: 0ce20a8040c6b19da1f57e1c053a7ef81d8bcb23
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751668"
---
# <a name="model-binding-in-aspnet-core"></a>ASP.NET Core 中的資料繫結

作者：[Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>模型繫結簡介

ASP.NET Core MVC 中的模型繫結會將 HTTP 要求中的資料對應至動作方法參數。 參數可能是簡單類型，例如字串、整數或浮點數，或者可能是複雜類型。 這是 MVC 的絕佳功能，因為將內送資料對應至對應項目是經常重複的情況，而不論資料的大小或複雜度。 MVC 解決此問題的方式是抽離繫結，讓開發人員不需要持續重寫每個應用程式中相同程式碼的略稍不同版本。 撰寫您自己的文字類型轉換器程式碼十分冗長，而且容易發生錯誤。

## <a name="how-model-binding-works"></a>模型繫結運作方式

MVC 在收到 HTTP 要求時，會將它路由至控制器的特定動作方法。 它會根據路由資料中的內容來判斷要執行的動作方法，然後將 HTTP 要求中的值繫結至該動作方法的參數。 例如，請考慮下列 URL：

`http://contoso.com/movies/edit/2`

因為路由範本與 `{controller=Home}/{action=Index}/{id?}` 類似，所以 `movies/edit/2` 會路由至 `Movies` 控制器和其 `Edit` 動作方法。 它也接受稱為 `id` 的選擇性參數。 動作方法的程式碼應該如下：

```csharp
public IActionResult Edit(int? id)
   ```

注意：URL 路由中的字串不區分大小寫。

MVC 會嘗試依名稱將要求資料繫結至動作參數。 MVC 會使用參數名稱以及其公用 settable 屬性的名稱來尋找每個參數的值。 在上述範例中，唯一的動作參數命名為 `id`，而 MVC 會繫結至路由值中同名的值。 除了路由值之外，MVC 會繫結要求各部分的資料，並且依設定的順序執行。 以下是資料來源清單，而其順序就是模型繫結查看它們的順序：

1. `Form values`：這些是使用 POST 方法查看 HTTP 要求的表單值  (包括 jQuery POST 要求)。

2. `Route values`：[路由](xref:fundamentals/routing)所提供的一組路由值

3. `Query strings`：URI 的查詢字串組件。

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

注意：表單值、路由資料和查詢字串都是儲存為名稱/值組。

因為模型繫結要求名為 `id` 的索引鍵，而且表單值中沒有名為 `id` 的項目，所以會繼續尋找該索引鍵的路由值。 在我們的範例中，它是相符項目。 進行繫結，並且將值轉換成整數 2。 使用 Edit(string id) 的相同要求會轉換成字串 "2"。

到目前為止，範例會使用簡單類型。 在 MVC 中，簡單類型是任何 .NET 基本類型或是包含字串類型轉換子的類型。 如果動作方法的參數是類別 (例如將簡單和複雜類型包含為屬性的 `Movie` 類型)，則仍會妥善地處理 MVC 的模型繫結。 它會使用反映和遞迴以周遊尋找相符項目之複雜類型的屬性。 模型繫結會尋找模式 *parameter_name.property_name*，以將值繫結至屬性。 如果找不到此表單的相符值，則會嘗試只使用屬性名稱進行繫結。 針對這些類型 (例如 `Collection` 類型)，模型繫結會尋找 *parameter_name[index]* 或只尋找 *[index]* 的相符項目。 模型繫結會以同樣地方式處理 `Dictionary` 類型，並要求 *parameter_name[key]* 或只要求 *[key]*，只要索引鍵是簡單類型即可。 所支援的索引鍵符合針對相同模型類型所產生的欄位名稱 HTML 和標籤協助程式。 這會啟用反覆存取值，讓表單欄位持續填入使用者輸入，以方便使用；例如，從建立或編輯所繫結的資料未通過驗證時。

若要進行繫結，類別必須具有公用預設建構函式，而要繫結的成員必須是公用可寫入屬性。 發生模型繫結時，只會使用公用預設建構函式來具現化此類別，接著可以設定屬性。

繫結參數時，模型繫結會停止尋找具有該名稱的值，並繼續繫結下一個參數。 否則，預設模型繫結行為會將參數設定為其預設值 (視其類型而定)：

* `T[]`：繫結會將類型 `T[]` 的參數設定為 `Array.Empty<T>()`，但類型 `byte[]` 陣列除外。 類型 `byte[]` 陣列會設定為 `null`。

* 參考類型：繫結會建立具有預設建構函式的類別執行個體，但未設定屬性。 不過，模型繫結會將 `string` 參數設定為 `null`。

* 可為 Null 的類型：可為 Null 的類型設定為 `null`。 在上述範例中，模型繫結會將 `id` 設定為 `null`，因為它的類型為 `int?`。

* 實值型別：類型為 `T` 的不可為 Null 的實值型別會設定為 `default(T)`。 例如，模型繫結會將參數 `int id` 設定為 0。 請考慮使用模型驗證或可為 Null 的類型，而不要依賴預設值。

如果繫結失敗，則 MVC 不會擲回錯誤。 接受使用者輸入的每一個動作都應該檢查 `ModelState.IsValid` 屬性。

注意：控制器 `ModelState` 屬性中的每個項目都是包含 `Errors` 屬性的 `ModelStateEntry`。 很少需要自行查詢此集合。 請改用 `ModelState.IsValid`。

此外，MVC 必須在執行模型繫結時考量一些特殊資料類型：

* `IFormFile`、`IEnumerable<IFormFile>`：屬於 HTTP 要求一部分的一或多個已上傳檔案。

* `CancellationToken`：用來取消非同步控制器中的活動。

這些類型可以繫結至類別類型上的動作參數或屬性。

完成模型繫結之後，會進行[驗證](validation.md)。 預設模型繫結最適用於大部分的開發案例。 它也可以擴充，因此，如果您有唯一的需求，則可以自訂內建行為。

## <a name="customize-model-binding-behavior-with-attributes"></a>使用屬性來自訂模型繫結行為

MVC 會包含數個屬性，可用來將其預設模型繫結行為導向至不同的來源。 例如，您可以指定屬性是否需要繫結，或者使用 `[BindRequired]` 或 `[BindNever]` 屬性，讓它根本不應該發生。 或者，您可以覆寫預設資料來源，並指定模型繫結器的資料來源。 以下是模型繫結屬性清單：

* `[BindRequired]`：如果無法繫結，則此屬性會新增模型狀態錯誤。

* `[BindNever]`：告知模型繫結器永遠不要繫結至此參數。

* `[FromHeader]`、`[FromQuery]`、`[FromRoute]`、`[FromForm]`：使用這些來指定您想要套用的確切繫結來源。

* `[FromServices]`：此屬性會使用[相依性插入](../../fundamentals/dependency-injection.md)，以從服務繫結參數。

* `[FromBody]`：使用已設定的格式器，來繫結要求本文中的資料。 格式器是根據要求的內容類型進行選取。

* `[ModelBinder]`：用來覆寫預設模型繫結器、繫結來源及名稱。

當您需要覆寫模型繫結的預設行為時，屬性是很有幫助的工具。

## <a name="customize-model-binding-and-validation-globally"></a>全域自訂模型繫結和驗證

模型繫結和驗證系統的行為是由描述下列內容的 [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata) 所驅動：

* 如何繫結模型。
* 如何對類型和其屬性進行驗證。

藉由將詳細資料提供者新增至 [MvcOptions.ModelMetadataDetailsProviders](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions.modelmetadatadetailsproviders#Microsoft_AspNetCore_Mvc_MvcOptions_ModelMetadataDetailsProviders)，即可全域設定系統行為的各個層面。 MVC 有一些內建的詳細資料提供者，其允許設定如停用特定類型的模型繫結或驗證等行為。

若要對特定類型的所有模型停用模型繫結，請在 `Startup.ConfigureServices` 中新增 [ExcludeBindingMetadataProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.excludebindingmetadataprovider)。 例如，若要對類型為 `System.Version` 的所有模型停用模型繫結：

```csharp
services.AddMvc().AddMvcOptions(options =>
    options.ModelMetadataDetailsProviders.Add(
        new ExcludeBindingMetadataProvider(typeof(System.Version))));
```

若要對特定類型的屬性停用驗證，請在 `Startup.ConfigureServices` 中新增 [SuppressChildValidationMetadataProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.suppresschildvalidationmetadataprovider)。 例如，若要針對類型為 `System.Guid` 的屬性停用驗證：

```csharp
services.AddMvc().AddMvcOptions(options =>
    options.ModelMetadataDetailsProviders.Add(
        new SuppressChildValidationMetadataProvider(typeof(System.Guid))));
```

## <a name="bind-formatted-data-from-the-request-body"></a>繫結要求本文中的格式化資料

要求資料可以有各種不同的格式，包括 JSON、XML 及其他項目。 當您使用 [FromBody] 屬性指出您想要將參數繫結至要求本文中的資料時，MVC 會使用一組已設定的格式器，以根據其內容類型來處理要求資料。 根據預設，MVC 包括 `JsonInputFormatter` 類別來處理 JSON 資料，但是您可以新增其他格式器來處理 XML 及其他自訂格式。

> [!NOTE]
> 一個以 `[FromBody]` 裝飾的動作最多可以有一個參數。 ASP.NET Core MVC 執行階段會將讀取要求資料流的責任委派給格式器。 讀取參數的要求資料流之後，一般無法重新讀取要求資料流，以繫結其他 `[FromBody]` 參數。

> [!NOTE]
> `JsonInputFormatter` 是預設格式器，並且根據 [Json.NET](https://www.newtonsoft.com/json)。

除非另外指定套用屬性，否則 ASP.NET Core 會根據 [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) 標頭和參數類型來選取輸入格式器。 如果您想要使用 XML 或另一種格式，則必須在 *Startup.cs* 檔案中設定它，但您可能需要先使用 NuGet 來取得 `Microsoft.AspNetCore.Mvc.Formatters.Xml` 的參考。 啟動程式碼應該如下：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

*Startup.cs* 檔案中的程式碼包含具有 `services` 引數的 `ConfigureServices` 方法，可用來建置 ASP.NET Core 應用程式的服務。 在範例中，我們會將 XML 格式器新增為 MVC 針對此應用程式所提供的服務。 傳入 `AddMvc` 方法的 `options` 引數，可讓您在應用程式啟動時從 MVC 新增和管理篩選、格式器以及其他系統選項。 然後將 `Consumes` 屬性套用至控制器類別或動作方法，以使用您想要的格式。

### <a name="custom-model-binding"></a>自訂模型繫結

您可以撰寫自己的自訂模型繫結器來擴充模型繫結。 深入了解[自訂模型繫結](../advanced/custom-model-binding.md)。
