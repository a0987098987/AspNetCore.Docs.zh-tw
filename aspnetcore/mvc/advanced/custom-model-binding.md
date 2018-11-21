---
title: ASP.NET Core 中的自訂模型繫結
author: ardalis
description: 了解模型繫結如何讓控制器動作直接使用 ASP.NET Core 中的模型類型。
ms.author: riande
ms.date: 11/13/2018
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 1da42829270e8ff4a626a45aec4d4e825062bd4f
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635286"
---
# <a name="custom-model-binding-in-aspnet-core"></a>ASP.NET Core 中的自訂模型繫結

作者：[Steve Smith](https://ardalis.com/)

模型繫結可直接透過模型類型 (傳入作為方法引數) 來執行控制器動作，而不用透過 HTTP 要求。 內送要求資料與應用程式模型之間的對應是由模型繫結器來處理。 開發人員可以透過實作自訂模型繫結器，來擴充內建模型繫結功能 (不過一般而言，您並不需要撰寫自己的提供者)。

[從 GitHub 檢視或下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>預設模型繫結器限制

預設模型繫結器支援大多數的常見 .NET Core 資料類型，因此應該符合大部分開發人員的需求。 這些開發人員希望能夠將要求中以文字為主的輸入直接繫結至模型類型。 輸入可能需要進行轉換才能繫結。 例如，當您有可用來查詢模型資料的索引鍵時， 您可以使用自訂模型繫結器，根據索引鍵來擷取資料。

## <a name="model-binding-review"></a>模型繫結檢閱

模型繫結使用特定定義來描述其作業類型。 「簡單型別」是指從輸入中的單一字串進行轉換。 「複雜類型」是指從多個輸入值進行轉換。 架構會根據是否有 `TypeConverter` 來判斷是否為不同類型。 如果您有不需要外部資源的簡單 `string` -> `SomeType` 對應，建議您建立型別轉換器。

在您建立自己的自訂模型繫結器之前，建議您先檢閱現有模型繫結器的實作方式。 請考慮使用 [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder)，將 Base64 編碼字串轉換成位元組陣列。 位元組陣列通常會儲存為檔案或資料庫 BLOB 欄位。

### <a name="working-with-the-bytearraymodelbinder"></a>使用 ByteArrayModelBinder

Base64 編碼字串可用來代表二進位資料。 例如，下列影像可編碼為字串。

![DotNet Bot](custom-model-binding/images/bot.png "DotNet Bot")

下圖顯示一小部分的編碼字串：

![DotNet Bot 編碼](custom-model-binding/images/encoded-bot.png "DotNet Bot 編碼")

請遵循[範例的讀我檔案](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md)中的指示，將 Base64 編碼字串轉換成檔案。

ASP.NET Core MVC 接受 Base64 編碼字串，並使用 `ByteArrayModelBinder` 將其轉換成位元組陣列。 實作 [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) 的 [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) 會將 `byte[]` 引數對應至 `ByteArrayModelBinder`：

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

當您建立自己的自訂模型繫結器時，您可以實作自己的 `IModelBinderProvider` 類型，或使用 [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute)。

下列範例示範如何使用 `ByteArrayModelBinder`，將 Base64 編碼字串轉換成 `byte[]`，並將結果儲存至檔案：

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

您可以使用 [Postman](https://www.getpostman.com/) 等工具，將 Base64 編碼字串張貼至此 API 方法：

![Postman](custom-model-binding/images/postman.png "Postman")

只要繫結器可以將要求資料繫結至適當命名的屬性或引數，模型繫結就會成功。 下列範例示範如何使用具有檢視模型的 `ByteArrayModelBinder`：

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>自訂模型繫結器範例

在本節中，我們會實作自訂模型繫結：

- 將內送要求資料轉換成強型別索引鍵引數。
- 使用 Entity Framework Core 來擷取相關聯的實體。
- 將相關聯的實體當作引數傳遞至動作方法。

下列範例會在 `Author` 模型上使用 `ModelBinder` 屬性：

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

在上述程式碼中，`ModelBinder` 屬性指定應該用來繫結 `Author` 動作參數的 `IModelBinder` 類型。

下列 `AuthorEntityBinder` 類別可繫結 `Author` 參數，做法是使用 Entity Framework Core 和 `authorId` 從資料來源擷取實體：

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

> [!NOTE]
> 上述 `AuthorEntityBinder` 類別會說明自訂模型繫結器。 此類別不會說明查閱情節的最佳做法。 若要進行查閱，請繫結 `authorId`，並在動作方法中查詢資料庫。 此方法會將模型繫結失敗從 `NotFound` 案例中分離。

下列程式碼示範如何在動作方法中使用 `AuthorEntityBinder`：

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

您可以使用 `ModelBinder` 屬性，將 `AuthorEntityBinder` 套用至未使用預設慣例的參數：

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

在此範例中，由於引數名稱不是預設的 `authorId`，因此會使用 `ModelBinder` 屬性在參數上指定。 請注意，控制器和動作方法相較於查詢動作方法中的實體，都更為簡化。 使用 Entity Framework Core 擷取作者的邏輯已移至模型繫結器。 當您有數個繫結至 `Author` 模型的方法時，這樣做會明顯簡化許多，而且可協助您遵循 [DRY 原則](http://deviq.com/don-t-repeat-yourself/)。

您可以將 `ModelBinder` 屬性套用至個別模型屬性 (例如在 ViewModel 上) 或動作方法參數，只指定該類型或動作的特定模型繫結器或模型名稱。

### <a name="implementing-a-modelbinderprovider"></a>實作 ModelBinderProvider

除了套用屬性，您還可以實作 `IModelBinderProvider`。 這是內建架構繫結器的實作方式。 當您指定繫結器的作業類型時，您會指定其所產生的引數類型，而**不是**繫結器接受的輸入。 下列繫結器提供者可搭配 `AuthorEntityBinder` 使用。 當它新增至提供者的 MVC 集合時，您不需要在 `Author` 或 `Author` 型別參數上使用 `ModelBinder` 屬性。

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> 注意：上述程式碼會傳回 `BinderTypeModelBinder`。 `BinderTypeModelBinder` 會作為模型繫結器的 Factory，並提供相依性插入 (DI)。 `AuthorEntityBinder` 需要 DI 能夠存取 EF Core。 如果您的模型繫結器需要來自 DI 的服務，請使用 `BinderTypeModelBinder`。

若要使用自訂模型繫結器提供者，將它新增 `ConfigureServices`：

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

評估模型繫結器時，會依序查看提供者集合， 並使用第一個傳回繫結器的提供者。

下圖顯示偵錯工具中的預設模型繫結器。

![預設模型繫結器](custom-model-binding/images/default-model-binders.png "預設模型繫結器")

將您的提供者新增集合結尾可能會導致在有機會呼叫自訂繫結器之前，即呼叫內建模型繫結器。 在此範例中，會將自訂提供者新增集合開頭，以確保其用於 `Author` 動作引數。

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>建議與最佳做法

自訂模型繫結器：

- 不應該嘗試設定狀態碼或傳回結果 (例如 404 找不到)。 如果模型繫結失敗，[動作篩選](xref:mvc/controllers/filters)或動作方法本身內的邏輯應該會處理失敗。
- 最適合用來排除動作方法中的重複程式碼和交叉關注。
- 一般而言，不應該用來將字串轉換成自訂類型，[`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) 通常是較好的選擇。
