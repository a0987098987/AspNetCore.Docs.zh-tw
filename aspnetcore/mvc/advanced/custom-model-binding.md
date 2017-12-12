---
title: "自訂的模型繫結"
author: ardalis
description: "自訂 ASP.NET Core MVC 中的模型繫結。"
keywords: "ASP.NET Core、 模型繫結、 自訂的模型繫結器"
ms.author: riande
manager: wpickett
ms.date: 04/10/2017
ms.topic: article
ms.assetid: ebd98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: f3fc3d624c3b79d49a886dd85ca8b19147631e39
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="custom-model-binding"></a>自訂的模型繫結

由[Steve Smith](https://ardalis.com/)

模型繫結可讓模型類型 （傳入當做方法引數），而不是 HTTP 要求比直接使用非同步控制器動作。 模型繫結器會處理內送要求資料和應用程式模型之間的對應。 開發人員可以實作自訂的模型繫結器 （雖然一般而言，您不需要撰寫您自己的提供者），以擴充的內建的模型繫結功能。

[檢視或從 GitHub 下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>預設模型繫結器限制

支援最常用的.NET Core 資料類型的預設模型繫結器，並應該能符合大部分的開發人員的需求。 他們希望直接繫結以文字為基礎的輸入要求的模型型別。 您可能需要轉換才能繫結它的輸入。 例如，當您有可以用來查詢模型資料的索引鍵。 您可以使用自訂的模型繫結來擷取索引鍵為基礎的資料。

## <a name="model-binding-review"></a>模型繫結檢閱

模型繫結會使用特定的定義，它作用於類型。 A*簡單型別*轉換輸入中的單一字串。 A*複雜型別*轉換從多個輸入的值。 架構決定根據存在的差異`TypeConverter`。 我們建議您建立型別轉換子，如果您有簡單`string`  ->  `SomeType`不需要外部資源的對應。

建立您自己自訂的模型繫結器之前, 是值得檢閱如何在現有的模型繫結器的實作。 請考慮[ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder)這可用來將 base64 編碼字串轉換成位元組陣列。 位元組陣列通常會儲存成檔案或資料庫 BLOB 欄位。

### <a name="working-with-the-bytearraymodelbinder"></a>使用 ByteArrayModelBinder

Base64 編碼的字串可以用來代表二進位資料。 例如，下列影像可以編碼為字串。

![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")

在下圖顯示一小部分的編碼的字串：

![dotnet bot 編碼](custom-model-binding/images/encoded-bot.png "dotnet bot 編碼")

請依照下列中的指示[範例的讀我檔案](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md)將 base64 編碼字串轉換成檔案。

ASP.NET Core MVC 可以採用 base64 編碼的字串，並使用`ByteArrayModelBinder`將它轉換成位元組陣列。 [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider)實作[IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider)對應`byte[]`引數`ByteArrayModelBinder`:

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

當建立您自己自訂的模型繫結器，您可以實作您自己`IModelBinderProvider`類型，或使用[ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute)。

下列範例示範如何使用`ByteArrayModelBinder`將以 base64 編碼字串轉換`byte[]`，並將結果儲存至檔案：

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

您可以張貼至這個應用程式開發介面方法，使用這類工具 base64 編碼字串[郵差](https://www.getpostman.com/):

![郵差](custom-model-binding/images/postman.png "郵差")

只要繫結器可以適當命名的屬性或引數繫結要求資料，模型繫結將會成功。 下列範例示範如何使用`ByteArrayModelBinder`檢視模型：

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>自訂的模型繫結器範例

這一節中，我們會實作自訂的模型繫結的：

- 將內送要求資料轉換成強類型索引鍵引數。
- 使用 Entity Framework Core 來提取相關聯的實體。
- 會將相關聯的實體做為引數傳遞至動作方法。

下列範例會使用`ModelBinder`屬性`Author`模型：

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

在上述程式碼，`ModelBinder`屬性指定的型別`IModelBinder`應該用來繫結`Author`動作參數。 

`AuthorEntityBinder`用來繫結`Author`參數從資料來源使用 Entity Framework Core 擷取實體和`authorId`:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

下列程式碼示範如何使用`AuthorEntityBinder`動作方法中：

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

`ModelBinder`屬性可以用來套用`AuthorEntityBinder`參數不是使用預設慣例：

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

在此範例中，不是預設值的引數名稱。 自`authorId`，同時指定參數使用`ModelBinder`屬性。 請注意，控制器和動作方法會簡化相較於查詢中的動作方法的實體。 若要擷取作者使用 Entity Framework 核心的邏輯會移至模型繫結器。 當您有數種方法，繫結至在作者模式下，並可協助您進行時，這可以是相當大的簡化[乾原則](http://deviq.com/don-t-repeat-yourself/)。

您可以套用`ModelBinder`屬性設定為個別的模型屬性 (例如 viewmodel 上) 或動作方法參數來指定特定模型繫結器或模型名稱，只要該類型或動作。

### <a name="implementing-a-modelbinderprovider"></a>實作 ModelBinderProvider

而不是套用屬性，您可以實作`IModelBinderProvider`。 這是內建架構繫結器的實作方式。 指定類型時您繫結器上運作，您指定的類型引數，它會產生，**不**輸入您的繫結器接受。 下列繫結器提供者搭配`AuthorEntityBinder`。 當它加入至 MVC 的提供者集合時，您不需要使用`ModelBinder`屬性`Author`或`Author`型別參數。

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> 注意： 上述程式碼會傳回`BinderTypeModelBinder`。 `BinderTypeModelBinder`當做的 factory 的模型繫結器，並提供相依性插入 (DI)。 `AuthorEntityBinder`需要 DI 存取 EF 核心。 使用`BinderTypeModelBinder`如果您的模型繫結器需要從 DI 服務。

若要使用自訂的模型繫結器提供者，將它加入`ConfigureServices`:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

當評估模型繫結器，順序會檢查提供者的集合。 使用傳回的繫結器第一個提供者。

下圖顯示的預設模型繫結器，從 偵錯工具。

![預設模型繫結器](custom-model-binding/images/default-model-binders.png "預設模型繫結器")

將您的提供者加入至集合結尾，可能會導致您的自訂繫結器有機會之前呼叫內建的模型繫結。 在此範例中，以確保它用於集合的開頭加入自訂提供者`Author`動作引數。

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>建議事項和最佳作法

自訂模型繫結器：
- 不應嘗試設定狀態碼，或傳回的結果 （例如，404 找不到）。 如果模型繫結失敗，[動作篩選條件](xref:mvc/controllers/filters)或邏輯在動作方法本身內應該處理失敗。
- 是最適用於不重複的程式碼和跨領域考量的動作方法。
- 通常不應將字串轉換成自訂型別， [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter)通常是較好的選擇。
