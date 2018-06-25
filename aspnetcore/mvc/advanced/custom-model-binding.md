---
title: ASP.NET Core 中的自訂模型繫結
author: ardalis
description: 了解模型繫結如何讓控制器動作直接使用 ASP.NET Core 中的模型類型。
ms.author: riande
ms.date: 04/10/2017
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: f5bd9a3eefb1fd9c1534e8767ad8e8af37514adb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275389"
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="257dc-103">ASP.NET Core 中的自訂模型繫結</span><span class="sxs-lookup"><span data-stu-id="257dc-103">Custom Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="257dc-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="257dc-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="257dc-105">模型繫結可直接透過模型類型 (傳入作為方法引數) 來執行控制器動作，而不用透過 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="257dc-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="257dc-106">內送要求資料與應用程式模型之間的對應是由模型繫結器來處理。</span><span class="sxs-lookup"><span data-stu-id="257dc-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="257dc-107">開發人員可以透過實作自訂模型繫結器，來擴充內建模型繫結功能 (不過一般而言，您並不需要撰寫自己的提供者)。</span><span class="sxs-lookup"><span data-stu-id="257dc-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="257dc-108">從 GitHub 檢視或下載範例</span><span class="sxs-lookup"><span data-stu-id="257dc-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="257dc-109">預設模型繫結器限制</span><span class="sxs-lookup"><span data-stu-id="257dc-109">Default model binder limitations</span></span>

<span data-ttu-id="257dc-110">預設模型繫結器支援大多數常見的 .NET Core 資料類型，因此應該符合大部分開發人員的需求。</span><span class="sxs-lookup"><span data-stu-id="257dc-110">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="257dc-111">這些開發人員希望能夠將要求中以文字為主的輸入直接繫結至模型類型。</span><span class="sxs-lookup"><span data-stu-id="257dc-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="257dc-112">輸入可能需要進行轉換才能繫結。</span><span class="sxs-lookup"><span data-stu-id="257dc-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="257dc-113">例如，當您有可用來查詢模型資料的索引鍵時，</span><span class="sxs-lookup"><span data-stu-id="257dc-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="257dc-114">您可以使用自訂模型繫結器，根據索引鍵來擷取資料。</span><span class="sxs-lookup"><span data-stu-id="257dc-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="257dc-115">模型繫結檢閱</span><span class="sxs-lookup"><span data-stu-id="257dc-115">Model binding review</span></span>

<span data-ttu-id="257dc-116">模型繫結使用特定定義來描述其作業類型。</span><span class="sxs-lookup"><span data-stu-id="257dc-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="257dc-117">「簡單型別」是指從輸入中的單一字串進行轉換。</span><span class="sxs-lookup"><span data-stu-id="257dc-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="257dc-118">「複雜類型」是指從多個輸入值進行轉換。</span><span class="sxs-lookup"><span data-stu-id="257dc-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="257dc-119">架構會根據是否有 `TypeConverter` 來判斷是否為不同類型。</span><span class="sxs-lookup"><span data-stu-id="257dc-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="257dc-120">如果您有不需要外部資源的簡單 `string` -> `SomeType` 對應，建議您建立型別轉換器。</span><span class="sxs-lookup"><span data-stu-id="257dc-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="257dc-121">在您建立自己的自訂模型繫結器之前，建議您先檢閱現有模型繫結器的實作方式。</span><span class="sxs-lookup"><span data-stu-id="257dc-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="257dc-122">請考慮使用 [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder)，將 Base64 編碼字串轉換成位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="257dc-122">Consider the [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="257dc-123">位元組陣列通常會儲存為檔案或資料庫 BLOB 欄位。</span><span class="sxs-lookup"><span data-stu-id="257dc-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="257dc-124">使用 ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="257dc-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="257dc-125">Base64 編碼字串可用來代表二進位資料。</span><span class="sxs-lookup"><span data-stu-id="257dc-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="257dc-126">例如，下列影像可編碼為字串。</span><span class="sxs-lookup"><span data-stu-id="257dc-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="257dc-127">![DotNet Bot](custom-model-binding/images/bot.png "DotNet Bot")</span><span class="sxs-lookup"><span data-stu-id="257dc-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="257dc-128">下圖顯示一小部分的編碼字串：</span><span class="sxs-lookup"><span data-stu-id="257dc-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="257dc-129">![DotNet Bot 編碼](custom-model-binding/images/encoded-bot.png "DotNet Bot 編碼")</span><span class="sxs-lookup"><span data-stu-id="257dc-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="257dc-130">請遵循[範例的讀我檔案](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md)中的指示，將 Base64 編碼字串轉換成檔案。</span><span class="sxs-lookup"><span data-stu-id="257dc-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="257dc-131">ASP.NET Core MVC 接受 Base64 編碼字串，並使用 `ByteArrayModelBinder` 將它轉換成位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="257dc-131">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="257dc-132">實作 [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) 的 [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) 會將 `byte[]` 引數對應至 `ByteArrayModelBinder`：</span><span class="sxs-lookup"><span data-stu-id="257dc-132">The [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="257dc-133">當您建立自己的自訂模型繫結器時，您可以實作自己的 `IModelBinderProvider` 類型，或使用 [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute)。</span><span class="sxs-lookup"><span data-stu-id="257dc-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="257dc-134">下列範例示範如何使用 `ByteArrayModelBinder`，將 Base64 編碼字串轉換成 `byte[]`，並將結果儲存至檔案：</span><span class="sxs-lookup"><span data-stu-id="257dc-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="257dc-135">您可以使用 [Postman](https://www.getpostman.com/) 等工具，將 Base64 編碼字串張貼至此 API 方法：</span><span class="sxs-lookup"><span data-stu-id="257dc-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="257dc-136">![Postman](custom-model-binding/images/postman.png "Postman")</span><span class="sxs-lookup"><span data-stu-id="257dc-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="257dc-137">只要繫結器可以將要求資料繫結至適當命名的屬性或引數，模型繫結就會成功。</span><span class="sxs-lookup"><span data-stu-id="257dc-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="257dc-138">下列範例示範如何使用具有檢視模型的 `ByteArrayModelBinder`：</span><span class="sxs-lookup"><span data-stu-id="257dc-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="257dc-139">自訂模型繫結器範例</span><span class="sxs-lookup"><span data-stu-id="257dc-139">Custom model binder sample</span></span>

<span data-ttu-id="257dc-140">在本節中，我們會實作自訂模型繫結：</span><span class="sxs-lookup"><span data-stu-id="257dc-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="257dc-141">將內送要求資料轉換成強型別索引鍵引數。</span><span class="sxs-lookup"><span data-stu-id="257dc-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="257dc-142">使用 Entity Framework Core 來擷取相關聯的實體。</span><span class="sxs-lookup"><span data-stu-id="257dc-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="257dc-143">將相關聯的實體當作引數傳遞至動作方法。</span><span class="sxs-lookup"><span data-stu-id="257dc-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="257dc-144">下列範例會在 `Author` 模型上使用 `ModelBinder` 屬性：</span><span class="sxs-lookup"><span data-stu-id="257dc-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="257dc-145">在上述程式碼中，`ModelBinder` 屬性指定應該用來繫結 `Author` 動作參數的 `IModelBinder` 類型。</span><span class="sxs-lookup"><span data-stu-id="257dc-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="257dc-146">`AuthorEntityBinder` 可用來繫結 `Author` 參數，做法是使用 Entity Framework Core 和 `authorId` 從資料來源擷取實體：</span><span class="sxs-lookup"><span data-stu-id="257dc-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="257dc-147">下列程式碼示範如何在動作方法中使用 `AuthorEntityBinder`：</span><span class="sxs-lookup"><span data-stu-id="257dc-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="257dc-148">您可以使用 `ModelBinder` 屬性，將 `AuthorEntityBinder` 套用至未使用預設慣例的參數：</span><span class="sxs-lookup"><span data-stu-id="257dc-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="257dc-149">在此範例中，由於引數名稱不是預設的 `authorId`，因此會使用 `ModelBinder` 屬性在參數上指定。</span><span class="sxs-lookup"><span data-stu-id="257dc-149">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="257dc-150">請注意，控制器和動作方法相較於查詢動作方法中的實體，都更為簡化。</span><span class="sxs-lookup"><span data-stu-id="257dc-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="257dc-151">使用 Entity Framework Core 擷取作者的邏輯已移至模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="257dc-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="257dc-152">當您有數個繫結至作者模型的方法時，這樣做明顯簡化許多，而且可協助您遵循 [DRY 原則](http://deviq.com/don-t-repeat-yourself/)。</span><span class="sxs-lookup"><span data-stu-id="257dc-152">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="257dc-153">您可以將 `ModelBinder` 屬性套用至個別模型屬性 (例如在 ViewModel 上) 或動作方法參數，只指定該類型或動作的特定模型繫結器或模型名稱。</span><span class="sxs-lookup"><span data-stu-id="257dc-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="257dc-154">實作 ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="257dc-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="257dc-155">除了套用屬性，您還可以實作 `IModelBinderProvider`。</span><span class="sxs-lookup"><span data-stu-id="257dc-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="257dc-156">這是內建架構繫結器的實作方式。</span><span class="sxs-lookup"><span data-stu-id="257dc-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="257dc-157">當您指定繫結器的作業類型時，您會指定其所產生的引數類型，而**不是**繫結器接受的輸入。</span><span class="sxs-lookup"><span data-stu-id="257dc-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="257dc-158">下列繫結器提供者可搭配 `AuthorEntityBinder` 使用。</span><span class="sxs-lookup"><span data-stu-id="257dc-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="257dc-159">當它新增 MVC 的提供者集合時，您不需要在 `Author` 或 `Author` 類型參數上使用 `ModelBinder` 屬性。</span><span class="sxs-lookup"><span data-stu-id="257dc-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="257dc-160">注意：上述程式碼會傳回 `BinderTypeModelBinder`。</span><span class="sxs-lookup"><span data-stu-id="257dc-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="257dc-161">`BinderTypeModelBinder` 會作為模型繫結器的 Factory，並提供相依性插入 (DI)。</span><span class="sxs-lookup"><span data-stu-id="257dc-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="257dc-162">`AuthorEntityBinder` 需要 DI 能夠存取 EF Core。</span><span class="sxs-lookup"><span data-stu-id="257dc-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="257dc-163">如果您的模型繫結器需要來自 DI 的服務，請使用 `BinderTypeModelBinder`。</span><span class="sxs-lookup"><span data-stu-id="257dc-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="257dc-164">若要使用自訂模型繫結器提供者，將它新增 `ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="257dc-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="257dc-165">評估模型繫結器時，會依序查看提供者集合，</span><span class="sxs-lookup"><span data-stu-id="257dc-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="257dc-166">並使用第一個傳回繫結器的提供者。</span><span class="sxs-lookup"><span data-stu-id="257dc-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="257dc-167">下圖顯示偵錯工具中的預設模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="257dc-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="257dc-168">![預設模型繫結器](custom-model-binding/images/default-model-binders.png "預設模型繫結器")</span><span class="sxs-lookup"><span data-stu-id="257dc-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="257dc-169">將您的提供者新增集合結尾可能會導致在有機會呼叫自訂繫結器之前，即呼叫內建模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="257dc-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="257dc-170">在此範例中，會將自訂提供者新增集合開頭，以確保其用於 `Author` 動作引數。</span><span class="sxs-lookup"><span data-stu-id="257dc-170">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="257dc-171">建議與最佳做法</span><span class="sxs-lookup"><span data-stu-id="257dc-171">Recommendations and best practices</span></span>

<span data-ttu-id="257dc-172">自訂模型繫結器：</span><span class="sxs-lookup"><span data-stu-id="257dc-172">Custom model binders:</span></span>
- <span data-ttu-id="257dc-173">不應該嘗試設定狀態碼或傳回結果 (例如 404 找不到)。</span><span class="sxs-lookup"><span data-stu-id="257dc-173">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="257dc-174">如果模型繫結失敗，[動作篩選](xref:mvc/controllers/filters)或動作方法本身內的邏輯應該會處理失敗。</span><span class="sxs-lookup"><span data-stu-id="257dc-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="257dc-175">最適合用來排除動作方法中的重複程式碼和交叉關注。</span><span class="sxs-lookup"><span data-stu-id="257dc-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="257dc-176">一般而言，不應該用來將字串轉換成自訂類型，[`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) 通常是較好的選擇。</span><span class="sxs-lookup"><span data-stu-id="257dc-176">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
