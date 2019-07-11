---
title: ASP.NET Core 中的資料繫結
author: tdykstra
description: 了解 ASP.NET Core 中的模型繫結如何運作，以及如何自訂其行為。
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: tdykstra
ms.date: 05/31/2019
uid: mvc/models/model-binding
ms.openlocfilehash: 10a9f8327bf16d11ec1e04ac3888d701f1ab1778
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724536"
---
# <a name="model-binding-in-aspnet-core"></a>ASP.NET Core 中的資料繫結

本文會說明何謂模型繫結、其運作方式，以及如何自訂其行為。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([如何下載](xref:index#how-to-download-a-sample))。

## <a name="what-is-model-binding"></a>何謂模型繫結

控制器和 Razor Pages 使用來自 HTTP 要求的資料。 例如，路由資料可能會提供記錄索引鍵，而已張貼的表單欄位可能會提供模型屬性的值。 撰寫程式碼來擷取這些值的每一個並將它們從字串轉換成 .NET 類型，不但繁瑣又容易發生錯誤。 模型繫結會自動化此程序。 模型繫結系統：

* 擷取來自各種來源的資料，例如路由資料、表單欄位和查詢字串。
* 在方法參數和公用屬性中，將資料提供給控制器和 Razor Pages。
* 將字串資料轉換成 .NET 類型。
* 更新複雜類型的屬性。

## <a name="example"></a>範例

假設您有下列動作方法：

[!code-csharp[](model-binding/samples/2.x/Controllers/PetsController.cs?name=snippet_DogsOnly)]

而應用程式收到具有此 URL 的要求：

```
http://contoso.com/api/pets/2?DogsOnly=true
```

在路由系統選取動作方法之後，模型繫結會透過下列步驟：

* 尋找第一個參數 `GetByID`，它是名為 `id` 的整數。
* 查看 HTTP 要求中所有可用的來源，在路由資料中找到 `id` = "2"。
* 將字串 "2" 轉換成整數 2。
* 尋找下一個 `GetByID` 的參數，它是名為 `dogsOnly` 的布林值。
* 查看來源，在查詢字串中找到 "DogsOnly=true"。 名稱比對不區分大小寫。
* 將字串 "true" 轉換成布林值 `true`。

架構接著會呼叫 `GetById` 方法，針對 `id` 參數傳送 2、`dogsOnly` 參數傳送 `true`。

在上例中，模型繫結目標都是簡單型別的方法參數。 目標也可能是複雜類型的屬性。 成功繫結每一個屬性後，該屬性就會發生[模型驗證](xref:mvc/models/validation)。 哪些資料繫結至模型，以及任何繫結或驗證錯誤的記錄，都會儲存在 [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) 或 [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState)。 為了解此程序是否成功，應用程式會檢查 [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) 旗標。

## <a name="targets"></a>目標

模型繫結會嘗試尋找下列幾種目標的值：

* 要求路由目標的控制器動作方法參數。
* 路由要求之目標 Razor Pages 處理常式方法的參數。 
* 控制站的公用屬性或 `PageModel` 類別，如由屬性指定。

### <a name="bindproperty-attribute"></a>[BindProperty] 屬性

可以套用至控制器公用屬性或 `PageModel` 類別，讓模型繫結以該屬性為目標：

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=7-8)]

### <a name="bindpropertiesattribute"></a>[BindProperties] 屬性

支援 ASP.NET Core 2.1 和更新版本。  可以套用至控制器或 `PageModel` 類別，通知模型繫結以該類別的所有公用屬性為目標：

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a>適用於 HTTP GET 要求的模型繫結

根據預設，屬性不會針對 HTTP GET 要求繫結。 一般而言，GET 要求只需要記錄識別碼參數。 此記錄識別碼用來查詢資料庫中的項目。 因此，不需要繫結保留模型執行個體的屬性。 在您要從 GET 要求將屬性繫結至資料的案例中，請將 `SupportsGet` 屬性設為 `true`：

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a>來源

根據預設，模型繫結會從下列 HTTP 要求的來源中，取得索引鍵/值組形式的資料：

1. 表單欄位 
1. 要求本文 (適用於[具有 [ApiController] 屬性的控制器](xref:web-api/index#binding-source-parameter-inference)。)
1. 路由資料
1. 查詢字串參數
1. 已上傳的檔案 

針對每個目標參數或屬性，會依照本清單顯示的順序掃描來源。 但也有一些例外：

* 路由資料和查詢字串值只用於簡單型別。
* 所上傳檔案只會繫結至實作 `IFormFile` 或 `IEnumerable<IFormFile>` 的目標類型。

如果預設行為不提供正確的結果，您可以使用下列其中一個屬性來指定用於任何所指定目標的來源。 

* [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - 從查詢字串取得值。 
* [[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - 從路由資料取得值。
* [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - 從已張貼的表單欄位取得值。
* [[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - 從要求本文取得值。
* [[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - 從 HTTP 標頭取得值。

這些屬性：

* 會個別新增至模型屬性 (不是模型類別)，如下列範例所示：

  [!code-csharp[](model-binding/samples/2.x/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* 選擇性地接受建構函式中的模型名稱值。 如果屬性名稱不符合要求中的值，則提供此選項。 例如，要求中的值可能是名稱中有連字號的標頭，如下列範例所示：

  [!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a>[FromBody] 屬性

要求本文資料會使用要求內容類型特定的輸入格式器來剖析。 [本文稍後](#input-formatters)會說明輸入格式器。

針對每個動作方法，請不要將 `[FromBody]` 套用至多個參數。 ASP.NET Core 執行階段會將讀取要求資料流的責任委派給輸入格式器。 要求資料經讀取後，即無法再次讀取以用來繫結其他 `[FromBody]` 參數。

### <a name="additional-sources"></a>其他來源

來源資料由「值提供者」  提供給模型繫結系統。 您可以撰寫並註冊自訂值提供者，其從其他來源取得模型繫結資料。 例如，您可能想要 Cookie 或工作階段狀態的資料。 從新來源取得資料：

* 建立會實作 `IValueProvider` 的類別。
* 建立會實作 `IValueProviderFactory` 的類別。
* 在 `Startup.ConfigureServices` 中註冊 Factory 類別。

範例應用程式包含[值提供者](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs)和從 Cookie 取得值的 [actory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) 範例。 以下是 `Startup.ConfigureServices` 中的註冊碼：

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=3)]

顯示的程式碼會將自訂值提供者放在所有內建值提供者的後面。  若要讓它在清單中排位第一，請呼叫 `Insert(0, new CookieValueProviderFactory())` 而非 `Add`。

## <a name="no-source-for-a-model-property"></a>無模型屬性的來源

根據預設，如果找不到模型屬性值，就不會建立模型狀態錯誤。 屬性設定為 null 或預設值：

* 可為 Null 的簡單型別會設為 `null`。
* 不可為 Null 的實值型別會設為 `default(T)`。 例如，參數 `int id` 設為 0。
* 針對複雜類型，模型繫結會使用預設建構函式建立執行個體，但不設定屬性。
* 陣列設為 `Array.Empty<T>()`，但 `byte[]` 陣列設為 `null`。

如果模型狀態在模型屬性表單欄位中找不到任何項目時應該失效，則請使用 [[BindRequired] 屬性](#bindrequired-attribute)。

請注意，此 `[BindRequired]` 行為適用於來自已張貼表單資料的模型繫結，不適合要求本文中的 JSON 或 XML 資料。 要求本文資料由[輸入格式器](#input-formatters)處理。

## <a name="type-conversion-errors"></a>類型轉換錯誤

如果找到來源，但無法轉換成目標型別，則模型狀態會標記為無效。 目標參數或屬性會設為 null 或預設值，如上一節中所述。

在具有 `[ApiController]` 屬性的 API 控制器中，無效的模型狀態會導致自動 HTTP 400 回應。

在 Razor 頁面中，重新顯示有錯誤訊息的頁面：

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

用戶端驗證會攔截大部分不正確的資料，否則會被提交至 Razor Pages 表單。 此驗證會讓您更難觸發上述醒目提示的程式碼。 範例應用程式包含 [Submit with Invalid Date] \(以無效日期送出\)  按鈕，將不正確的資料放入 [雇用日期]  欄位並提交表單。 此按鈕會顯示當資料轉換錯誤發生時，重新顯示頁面的程式碼如何運作。

當上述程式碼重新顯示頁面時，無效的輸入不會顯示在表單欄位中。 這是因為模型屬性已設為 null 或預設值。 無效的輸入確實會出現在錯誤訊息中。 但是，如果您想要在表單欄位中重新顯示不正確的資料，請考慮讓模型屬性成為字串，以手動方式執行資料轉換。

如果您不希望類型轉換錯誤導致模型狀態錯誤，建議使用相同的策略。 在此情況下，讓模型屬性成為字串。

## <a name="simple-types"></a>簡單型別

模型繫結器可將來源字串轉換成的簡單型別包括：

* [布林值](xref:System.ComponentModel.BooleanConverter)
* [Byte](xref:System.ComponentModel.ByteConverter)、[SByte](xref:System.ComponentModel.SByteConverter)
* [Char](xref:System.ComponentModel.CharConverter)
* [DateTime](xref:System.ComponentModel.DateTimeConverter)
* [DateTimeOffset](xref:System.ComponentModel.DateTimeOffsetConverter)
* [Decimal](xref:System.ComponentModel.DecimalConverter)
* [Double](xref:System.ComponentModel.DoubleConverter)
* [Enum](xref:System.ComponentModel.EnumConverter)
* [Guid](xref:System.ComponentModel.GuidConverter)
* [Int16](xref:System.ComponentModel.Int16Converter)、[Int32](xref:System.ComponentModel.Int32Converter)、[Int64](xref:System.ComponentModel.Int64Converter)
* [Single](xref:System.ComponentModel.SingleConverter)
* [TimeSpan](xref:System.ComponentModel.TimeSpanConverter)
* [UInt16](xref:System.ComponentModel.UInt16Converter)、[UInt32](xref:System.ComponentModel.UInt32Converter)、[UInt64](xref:System.ComponentModel.UInt64Converter)
* [Uri](xref:System.UriTypeConverter)
* [版本](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a>複雜類型

複雜類型必須具有要繫結的公用預設建構函式及公用可寫入屬性。 發生模型繫結時，類別會使用公用預設建構函式具現化。 

針對複雜類型的每個屬性，模型繫結會查看名稱模式 *prefix.property_name* 的來源。 如果找不到，它會只尋找沒有前置詞的 *property_name*。

若要繫結至參數，則前置詞是參數名稱。 若要繫結至 `PageModel` 公用屬性，則前置詞為公用屬性名稱。 某些屬性 (Attribute) 具有 `Prefix` 屬性 (Property)，其可讓您覆寫參數或屬性名稱的預設使用方法。

例如，假設複雜類型是下列 `Instructor` 類別：

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a>前置詞 = 參數名稱

如果要繫結的模型是名為 `instructorToUpdate` 的參數：

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

模型繫結從查看索引鍵 `instructorToUpdate.ID` 的來源開始。 如果找不到，則它會尋找不含前置詞的 `ID`。

### <a name="prefix--property-name"></a>前置詞 = 屬性名稱

如果要繫結的模型是控制器或 `PageModel` 類別名為 `Instructor` 的屬性：

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

模型繫結從查看索引鍵 `Instructor.ID` 的來源開始。 如果找不到，則它會尋找不含前置詞的 `ID`。

### <a name="custom-prefix"></a>自訂前置詞

如果要繫結的模型是名為 `instructorToUpdate` 的參數，且 `Bind` 屬性指定 `Instructor` 為前置詞：

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

模型繫結從查看索引鍵 `Instructor.ID` 的來源開始。 如果找不到，則它會尋找不含前置詞的 `ID`。

### <a name="attributes-for-complex-type-targets"></a>複雜類型目標的屬性

有數個內建屬性可供控制複雜類型的模型繫結：

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> 當張貼的表單資料為值來源時，這些屬性會影響模型繫結。 它們不會影響處理已張貼 JSON 和 XML 要求本文的輸入格式器。 [本文稍後](#input-formatters)會說明輸入格式器。
>
> 另請參閱[模型驗證](xref:mvc/models/validation#required-attribute)中的 `[Required]` 屬性討論。

### <a name="bindrequired-attribute"></a>[BindRequired] 屬性

只能套用至模型屬性，不能套用到方法參數。 如果模型的屬性不能發生繫結，則會造成模型繫結新增模型狀態錯誤。 以下為範例：

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a>[BindNever] 屬性

只能套用至模型屬性，不能套用到方法參數。 避免模型繫結設定模型的屬性。 以下為範例：

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a>[Bind] 屬性

可以套用至類別或方法參數。 指定模型繫結應包含哪些模型屬性。

在下列範例中，當呼叫任何處理常式或動作方法時，只會繫結 `Instructor` 模型的指定屬性：

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

在下列範例中，當呼叫 `OnPost` 方法時，只會繫結 `Instructor` 模型的指定屬性：

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

`[Bind]` 屬性可用來防止「建立」  案例中的大量指派。 它在編輯案例中運作不良，因為排除的屬性都設定為 null 或預設值，而非保持不變。 建議使用檢視模型而非 `[Bind]` 屬性來防禦大量指派。 如需詳細資訊，請參閱[關於大量指派的安全性注意事項](xref:data/ef-mvc/crud#security-note-about-overposting)。

## <a name="collections"></a>集合

針對簡單型別集合的目標，模型繫結會尋找符合 *parameter_name* 或 *property_name* 的項目。 如果找不到相符項目，它會尋找其中一種沒有前置詞的受支援格式。 例如：

* 假設要繫結的參數是名為 `selectedCourses` 的陣列：

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* 表單或查詢字串資料可以是下列其中一種格式：
   
  ```
  selectedCourses=1050&selectedCourses=2000 
  ```

  ```
  selectedCourses[0]=1050&selectedCourses[1]=2000
  ```

  ```
  [0]=1050&[1]=2000
  ```

  ```
  selectedCourses[a]=1050&selectedCourses[b]=2000&selectedCourses.index=a&selectedCourses.index=b
  ```

  ```
  [a]=1050&[b]=2000&index=a&index=b
  ```

* 只有表單資料支援下列格式：

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* 針對前列所有範例格式，模型繫結會將有兩個項目的陣列傳遞至 `selectedCourses` 參數：

  * selectedCourses[0]=1050
  * selectedCourses[1]=2000

  使用下標數字 (... [0] ... [1] ...) 的資料格式，必須確定從零開始依序編號。 下標編號中如有任何間距，則會忽略間隔後的所有項目。 例如，如果下標是 0 和 2，而不是 0 和 1，則忽略第二個項目。

## <a name="dictionaries"></a>字典

針對 `Dictionary` 目標，模型繫結會尋找符合 *parameter_name* 或 *property_name* 的項目。 如果找不到相符項目，它會尋找其中一種沒有前置詞的受支援格式。 例如：

* 假設目標參數是名為 `selectedCourses` 的 `Dictionary<int, string>`：

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* 已張貼的表單或查詢字串資料看起來會像下列其中一個範例：

  ```
  selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  [1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&
  selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
  ```

  ```
  [0].Key=1050&[0].Value=Chemistry&[1].Key=2000&[1].Value=Economics
  ```

* 針對前列所有範例格式，模型繫結會將有兩個項目的字典傳遞至 `selectedCourses` 參數：

  * selectedCourses["1050"]="Chemistry"
  * selectedCourses["2000"]="Economics"

## <a name="special-data-types"></a>特殊資料類型

模型繫結可以處理某些特殊資料類型。

### <a name="iformfile-and-iformfilecollection"></a>IFormFile 和 IFormFileCollection

HTTP 要求包含上傳的檔案。  也支援多個檔案的 `IEnumerable<IFormFile>`。

### <a name="cancellationtoken"></a>CancellationToken

用來取消非同步控制器中的活動。

### <a name="formcollection"></a>FormCollection

用來擷取已張貼表單資料中的所有值。

## <a name="input-formatters"></a>輸入格式器

要求本文中的資料可以是 JSON、XML 或某些其他格式。 模型繫結會使用設定處理特定內容類型的「輸入格式器」  ，來剖析此資料。 根據預設，ASP.NET Core 包含 JSON 型輸入格式器以處理 JSON 資料。 您可以新增其他內容類型的其他格式器。

ASP.NET Core 選取以 [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) 屬性為基礎的輸入格式器。 若無任何屬性，則它會使用 [Content-Type 標頭](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html)。

使用內建的 XML 輸入格式器：

* 安裝 `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet 套件。

* 在 `Startup.ConfigureServices` 中，呼叫 <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> 或 <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>。

  [!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* 將 `Consumes` 屬性套用至要求本文應為 XML 的控制器類別或動作方法。

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  如需詳細資訊，請參閱 [XML 序列化簡介](https://docs.microsoft.com/en-us/dotnet/standard/serialization/introducing-xml-serialization)。

## <a name="exclude-specified-types-from-model-binding"></a>排除模型繫結中的指定類型

模型繫結和驗證系統的行為是由 [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata) 所驅動。 您可以將詳細資料提供者新增至 [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders)，藉以自訂 `ModelMetadata`。 內建的詳細資料提供者可用於停用模型繫結或驗證所指定類型。

若要停用指定類型之所有模型的模型繫結，請在 `Startup.ConfigureServices` 中新增 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider>。 例如，若要對類型為 `System.Version` 的所有模型停用模型繫結：

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

若要停用指定類型屬性的驗證，請在 `Startup.ConfigureServices` 中新增 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider>。 例如，若要針對類型為 `System.Guid` 的屬性停用驗證：

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a>自訂模型繫結器

您可以撰寫自訂的模型繫結器來擴充模型繫結，並使用 `[ModelBinder]` 屬性以針對特定目標來選取它。 深入了解[自訂模型繫結](xref:mvc/advanced/custom-model-binding)。

## <a name="manual-model-binding"></a>手動模型繫結

使用 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> 方法即可手動叫用模型繫結。 此方法已於 `ControllerBase` 和 `PageModel` 類別中定義。 方法多載可讓您指定要使用的前置詞和值提供者。 如果模型繫結失敗，此方法會傳回 `false`。 以下為範例：

[!code-csharp[](model-binding/samples/2.x/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a>[FromServices] 屬性

這個屬性名稱會遵循指定資料來源的模型繫結屬性模式。 但它與來自值提供者的資料繫結無關。 它從[相依性插入](xref:fundamentals/dependency-injection)容器取得類型的執行個體。 其目的是只有在呼叫特定方法時，才在您需要服務時提供建構函式插入的替代項目。

## <a name="additional-resources"></a>其他資源

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>
