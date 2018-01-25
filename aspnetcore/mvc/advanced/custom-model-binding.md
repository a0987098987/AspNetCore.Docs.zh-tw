---
title: "自訂的模型繫結"
author: ardalis
description: "自訂 ASP.NET Core MVC 中的模型繫結。"
ms.author: riande
manager: wpickett
ms.date: 04/10/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 85d5ca18944e774d1f2577459c6c45acde01e4d9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="custom-model-binding"></a><span data-ttu-id="96e87-103">自訂的模型繫結</span><span class="sxs-lookup"><span data-stu-id="96e87-103">Custom Model Binding</span></span>

<span data-ttu-id="96e87-104">由[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="96e87-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="96e87-105">模型繫結可讓模型類型 （傳入當做方法引數），而不是 HTTP 要求比直接使用非同步控制器動作。</span><span class="sxs-lookup"><span data-stu-id="96e87-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="96e87-106">模型繫結器會處理內送要求資料和應用程式模型之間的對應。</span><span class="sxs-lookup"><span data-stu-id="96e87-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="96e87-107">開發人員可以實作自訂的模型繫結器 （雖然一般而言，您不需要撰寫您自己的提供者），以擴充的內建的模型繫結功能。</span><span class="sxs-lookup"><span data-stu-id="96e87-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="96e87-108">檢視或從 GitHub 下載範例</span><span class="sxs-lookup"><span data-stu-id="96e87-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="96e87-109">預設模型繫結器限制</span><span class="sxs-lookup"><span data-stu-id="96e87-109">Default model binder limitations</span></span>

<span data-ttu-id="96e87-110">支援最常用的.NET Core 資料類型的預設模型繫結器，並應該能符合大部分的開發人員的需求。</span><span class="sxs-lookup"><span data-stu-id="96e87-110">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="96e87-111">他們希望直接繫結以文字為基礎的輸入要求的模型型別。</span><span class="sxs-lookup"><span data-stu-id="96e87-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="96e87-112">您可能需要轉換才能繫結它的輸入。</span><span class="sxs-lookup"><span data-stu-id="96e87-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="96e87-113">例如，當您有可以用來查詢模型資料的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="96e87-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="96e87-114">您可以使用自訂的模型繫結來擷取索引鍵為基礎的資料。</span><span class="sxs-lookup"><span data-stu-id="96e87-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="96e87-115">模型繫結檢閱</span><span class="sxs-lookup"><span data-stu-id="96e87-115">Model binding review</span></span>

<span data-ttu-id="96e87-116">模型繫結會使用特定的定義，它作用於類型。</span><span class="sxs-lookup"><span data-stu-id="96e87-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="96e87-117">A*簡單型別*轉換輸入中的單一字串。</span><span class="sxs-lookup"><span data-stu-id="96e87-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="96e87-118">A*複雜型別*轉換從多個輸入的值。</span><span class="sxs-lookup"><span data-stu-id="96e87-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="96e87-119">架構決定根據存在的差異`TypeConverter`。</span><span class="sxs-lookup"><span data-stu-id="96e87-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="96e87-120">我們建議您建立型別轉換子，如果您有簡單`string`  ->  `SomeType`不需要外部資源的對應。</span><span class="sxs-lookup"><span data-stu-id="96e87-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="96e87-121">建立您自己自訂的模型繫結器之前, 是值得檢閱如何在現有的模型繫結器的實作。</span><span class="sxs-lookup"><span data-stu-id="96e87-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="96e87-122">請考慮[ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder)這可用來將 base64 編碼字串轉換成位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="96e87-122">Consider the [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="96e87-123">位元組陣列通常會儲存成檔案或資料庫 BLOB 欄位。</span><span class="sxs-lookup"><span data-stu-id="96e87-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="96e87-124">使用 ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="96e87-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="96e87-125">Base64 編碼的字串可以用來代表二進位資料。</span><span class="sxs-lookup"><span data-stu-id="96e87-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="96e87-126">例如，下列影像可以編碼為字串。</span><span class="sxs-lookup"><span data-stu-id="96e87-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="96e87-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span><span class="sxs-lookup"><span data-stu-id="96e87-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="96e87-128">在下圖顯示一小部分的編碼的字串：</span><span class="sxs-lookup"><span data-stu-id="96e87-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="96e87-129">![dotnet bot 編碼](custom-model-binding/images/encoded-bot.png "dotnet bot 編碼")</span><span class="sxs-lookup"><span data-stu-id="96e87-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="96e87-130">請依照下列中的指示[範例的讀我檔案](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md)將 base64 編碼字串轉換成檔案。</span><span class="sxs-lookup"><span data-stu-id="96e87-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="96e87-131">ASP.NET Core MVC 可以採用 base64 編碼的字串，並使用`ByteArrayModelBinder`將它轉換成位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="96e87-131">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="96e87-132">[ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider)實作[IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider)對應`byte[]`引數`ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="96e87-132">The [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="96e87-133">當建立您自己自訂的模型繫結器，您可以實作您自己`IModelBinderProvider`類型，或使用[ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute)。</span><span class="sxs-lookup"><span data-stu-id="96e87-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="96e87-134">下列範例示範如何使用`ByteArrayModelBinder`將以 base64 編碼字串轉換`byte[]`，並將結果儲存至檔案：</span><span class="sxs-lookup"><span data-stu-id="96e87-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="96e87-135">您可以張貼至這個應用程式開發介面方法，使用這類工具 base64 編碼字串[郵差](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="96e87-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="96e87-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="96e87-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="96e87-137">只要繫結器可以適當命名的屬性或引數繫結要求資料，模型繫結將會成功。</span><span class="sxs-lookup"><span data-stu-id="96e87-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="96e87-138">下列範例示範如何使用`ByteArrayModelBinder`檢視模型：</span><span class="sxs-lookup"><span data-stu-id="96e87-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="96e87-139">自訂的模型繫結器範例</span><span class="sxs-lookup"><span data-stu-id="96e87-139">Custom model binder sample</span></span>

<span data-ttu-id="96e87-140">這一節中，我們會實作自訂的模型繫結的：</span><span class="sxs-lookup"><span data-stu-id="96e87-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="96e87-141">將內送要求資料轉換成強類型索引鍵引數。</span><span class="sxs-lookup"><span data-stu-id="96e87-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="96e87-142">使用 Entity Framework Core 來提取相關聯的實體。</span><span class="sxs-lookup"><span data-stu-id="96e87-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="96e87-143">會將相關聯的實體做為引數傳遞至動作方法。</span><span class="sxs-lookup"><span data-stu-id="96e87-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="96e87-144">下列範例會使用`ModelBinder`屬性`Author`模型：</span><span class="sxs-lookup"><span data-stu-id="96e87-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="96e87-145">在上述程式碼，`ModelBinder`屬性指定的型別`IModelBinder`應該用來繫結`Author`動作參數。</span><span class="sxs-lookup"><span data-stu-id="96e87-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="96e87-146">`AuthorEntityBinder`用來繫結`Author`參數從資料來源使用 Entity Framework Core 擷取實體和`authorId`:</span><span class="sxs-lookup"><span data-stu-id="96e87-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="96e87-147">下列程式碼示範如何使用`AuthorEntityBinder`動作方法中：</span><span class="sxs-lookup"><span data-stu-id="96e87-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="96e87-148">`ModelBinder`屬性可以用來套用`AuthorEntityBinder`不使用預設慣例的參數：</span><span class="sxs-lookup"><span data-stu-id="96e87-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="96e87-149">在此範例中，不預設引數的名稱。 自`authorId`，同時指定參數使用`ModelBinder`屬性。</span><span class="sxs-lookup"><span data-stu-id="96e87-149">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="96e87-150">請注意，控制器和動作方法會簡化相較於查詢中的動作方法的實體。</span><span class="sxs-lookup"><span data-stu-id="96e87-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="96e87-151">若要擷取作者使用 Entity Framework 核心的邏輯會移至模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="96e87-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="96e87-152">當您有數種方法，繫結至在作者模式下，並可協助您進行時，這可以是相當大的簡化[乾原則](http://deviq.com/don-t-repeat-yourself/)。</span><span class="sxs-lookup"><span data-stu-id="96e87-152">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="96e87-153">您可以套用`ModelBinder`屬性設定為個別的模型屬性 (例如 viewmodel 上) 或動作方法參數來指定特定模型繫結器或模型名稱，只要該類型或動作。</span><span class="sxs-lookup"><span data-stu-id="96e87-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="96e87-154">實作 ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="96e87-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="96e87-155">而不是套用屬性，您可以實作`IModelBinderProvider`。</span><span class="sxs-lookup"><span data-stu-id="96e87-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="96e87-156">這是內建架構繫結器的實作方式。</span><span class="sxs-lookup"><span data-stu-id="96e87-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="96e87-157">指定類型時您繫結器上運作，您指定的類型引數，它會產生，**不**輸入您的繫結器接受。</span><span class="sxs-lookup"><span data-stu-id="96e87-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="96e87-158">下列繫結器提供者搭配`AuthorEntityBinder`。</span><span class="sxs-lookup"><span data-stu-id="96e87-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="96e87-159">當它加入至 MVC 的提供者集合時，您不需要使用`ModelBinder`屬性`Author`或`Author`型別參數。</span><span class="sxs-lookup"><span data-stu-id="96e87-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="96e87-160">注意： 上述程式碼會傳回`BinderTypeModelBinder`。</span><span class="sxs-lookup"><span data-stu-id="96e87-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="96e87-161">`BinderTypeModelBinder`當做的 factory 的模型繫結器，並提供相依性插入 (DI)。</span><span class="sxs-lookup"><span data-stu-id="96e87-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="96e87-162">`AuthorEntityBinder`需要 DI 存取 EF 核心。</span><span class="sxs-lookup"><span data-stu-id="96e87-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="96e87-163">使用`BinderTypeModelBinder`如果您的模型繫結器需要從 DI 服務。</span><span class="sxs-lookup"><span data-stu-id="96e87-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="96e87-164">若要使用自訂的模型繫結器提供者，將它加入`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="96e87-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="96e87-165">當評估模型繫結器，順序會檢查提供者的集合。</span><span class="sxs-lookup"><span data-stu-id="96e87-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="96e87-166">使用傳回的繫結器第一個提供者。</span><span class="sxs-lookup"><span data-stu-id="96e87-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="96e87-167">下圖顯示的預設模型繫結器，從 偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="96e87-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="96e87-168">![預設模型繫結器](custom-model-binding/images/default-model-binders.png "預設模型繫結器")</span><span class="sxs-lookup"><span data-stu-id="96e87-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="96e87-169">將您的提供者加入至集合結尾，可能會導致您的自訂繫結器有機會之前呼叫內建的模型繫結。</span><span class="sxs-lookup"><span data-stu-id="96e87-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="96e87-170">在此範例中，以確保它用於集合的開頭加入自訂提供者`Author`動作引數。</span><span class="sxs-lookup"><span data-stu-id="96e87-170">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="96e87-171">建議事項和最佳作法</span><span class="sxs-lookup"><span data-stu-id="96e87-171">Recommendations and best practices</span></span>

<span data-ttu-id="96e87-172">自訂模型繫結器：</span><span class="sxs-lookup"><span data-stu-id="96e87-172">Custom model binders:</span></span>
- <span data-ttu-id="96e87-173">不應嘗試設定狀態碼，或傳回的結果 （例如，404 找不到）。</span><span class="sxs-lookup"><span data-stu-id="96e87-173">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="96e87-174">如果模型繫結失敗，[動作篩選條件](xref:mvc/controllers/filters)或邏輯在動作方法本身內應該處理失敗。</span><span class="sxs-lookup"><span data-stu-id="96e87-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="96e87-175">是最適用於不重複的程式碼和跨領域考量的動作方法。</span><span class="sxs-lookup"><span data-stu-id="96e87-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="96e87-176">通常不應該用來將字串轉換成自訂型別， [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter)通常是較好的選擇。</span><span class="sxs-lookup"><span data-stu-id="96e87-176">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
