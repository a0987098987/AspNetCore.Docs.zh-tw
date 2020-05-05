---
title: ASP.NET Core 中的自訂模型繫結
author: ardalis
description: 了解模型繫結如何讓控制器動作直接使用 ASP.NET Core 中的模型類型。
ms.author: riande
ms.date: 01/06/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 109bebe79c9e77d26b02ca27367b8ff33191a4b4
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776691"
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="75c78-103">ASP.NET Core 中的自訂模型繫結</span><span class="sxs-lookup"><span data-stu-id="75c78-103">Custom Model Binding in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="75c78-104">作者： [Steve Smith](https://ardalis.com/)和[Kirk Larkin](https://twitter.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="75c78-104">By [Steve Smith](https://ardalis.com/) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

<span data-ttu-id="75c78-105">模型繫結可直接透過模型類型 (傳入作為方法引數) 來執行控制器動作，而不用透過 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="75c78-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="75c78-106">內送要求資料與應用程式模型之間的對應是由模型繫結器來處理。</span><span class="sxs-lookup"><span data-stu-id="75c78-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="75c78-107">開發人員可以透過實作自訂模型繫結器，來擴充內建模型繫結功能 (不過一般而言，您並不需要撰寫自己的提供者)。</span><span class="sxs-lookup"><span data-stu-id="75c78-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

<span data-ttu-id="75c78-108">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="75c78-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="default-model-binder-limitations"></a><span data-ttu-id="75c78-109">預設模型繫結器限制</span><span class="sxs-lookup"><span data-stu-id="75c78-109">Default model binder limitations</span></span>

<span data-ttu-id="75c78-110">預設模型繫結器支援大多數的常見 .NET Core 資料類型，因此應該符合大部分開發人員的需求。</span><span class="sxs-lookup"><span data-stu-id="75c78-110">The default model binders support most of the common .NET Core data types and should meet most developers' needs.</span></span> <span data-ttu-id="75c78-111">這些開發人員希望能夠將要求中以文字為主的輸入直接繫結至模型類型。</span><span class="sxs-lookup"><span data-stu-id="75c78-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="75c78-112">輸入可能需要進行轉換才能繫結。</span><span class="sxs-lookup"><span data-stu-id="75c78-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="75c78-113">例如，當您有可用來查詢模型資料的索引鍵時，</span><span class="sxs-lookup"><span data-stu-id="75c78-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="75c78-114">您可以使用自訂模型繫結器，根據索引鍵來擷取資料。</span><span class="sxs-lookup"><span data-stu-id="75c78-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="75c78-115">模型繫結檢閱</span><span class="sxs-lookup"><span data-stu-id="75c78-115">Model binding review</span></span>

<span data-ttu-id="75c78-116">模型繫結使用特定定義來描述其作業類型。</span><span class="sxs-lookup"><span data-stu-id="75c78-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="75c78-117">「簡單型別」\*\* 是指從輸入中的單一字串進行轉換。</span><span class="sxs-lookup"><span data-stu-id="75c78-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="75c78-118">「複雜類型」\*\* 是指從多個輸入值進行轉換。</span><span class="sxs-lookup"><span data-stu-id="75c78-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="75c78-119">架構會根據是否有 `TypeConverter` 來判斷是否為不同類型。</span><span class="sxs-lookup"><span data-stu-id="75c78-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="75c78-120">如果您有不需要外部資源的簡單 `string` -> `SomeType` 對應，建議您建立型別轉換器。</span><span class="sxs-lookup"><span data-stu-id="75c78-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="75c78-121">在您建立自己的自訂模型繫結器之前，建議您先檢閱現有模型繫結器的實作方式。</span><span class="sxs-lookup"><span data-stu-id="75c78-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="75c78-122">請考慮<xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinder>可以用來將 base64 編碼的字串轉換成位元組陣列的。</span><span class="sxs-lookup"><span data-stu-id="75c78-122">Consider the <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinder> which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="75c78-123">位元組陣列通常會儲存為檔案或資料庫 BLOB 欄位。</span><span class="sxs-lookup"><span data-stu-id="75c78-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="75c78-124">使用 ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="75c78-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="75c78-125">Base64 編碼字串可用來代表二進位資料。</span><span class="sxs-lookup"><span data-stu-id="75c78-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="75c78-126">例如，影像可以編碼為字串。</span><span class="sxs-lookup"><span data-stu-id="75c78-126">For example, an image can be encoded as a string.</span></span> <span data-ttu-id="75c78-127">此範例會在[Base64String](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/samples/3.x/CustomModelBindingSample/Base64String.txt)中將影像包含為 base64 編碼的字串。</span><span class="sxs-lookup"><span data-stu-id="75c78-127">The sample includes an image as a base64-encoded string in [Base64String.txt](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/samples/3.x/CustomModelBindingSample/Base64String.txt).</span></span>

<span data-ttu-id="75c78-128">ASP.NET Core MVC 接受 Base64 編碼字串，並使用 `ByteArrayModelBinder` 將其轉換成位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="75c78-128">ASP.NET Core MVC can take a base64-encoded string and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="75c78-129">會<xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinderProvider>將`byte[]`引數`ByteArrayModelBinder`對應至：</span><span class="sxs-lookup"><span data-stu-id="75c78-129">The <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinderProvider> maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        var loggerFactory = context.Services.GetRequiredService<ILoggerFactory>();
        return new ByteArrayModelBinder(loggerFactory);
    }

    return null;
}
```

<span data-ttu-id="75c78-130">建立您自己的自訂模型系結器時，您`IModelBinderProvider`可以執行自己的<xref:Microsoft.AspNetCore.Mvc.ModelBinderAttribute>型別，或使用。</span><span class="sxs-lookup"><span data-stu-id="75c78-130">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the <xref:Microsoft.AspNetCore.Mvc.ModelBinderAttribute>.</span></span>

<span data-ttu-id="75c78-131">下列範例示範如何使用 `ByteArrayModelBinder`，將 Base64 編碼字串轉換成 `byte[]`，並將結果儲存至檔案：</span><span class="sxs-lookup"><span data-stu-id="75c78-131">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Controllers/ImageController.cs?name=snippet_Post)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="75c78-132">您可以使用 [Postman](https://www.getpostman.com/) 等工具，將 Base64 編碼字串張貼至此 API 方法：</span><span class="sxs-lookup"><span data-stu-id="75c78-132">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="75c78-133">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="75c78-133">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="75c78-134">只要繫結器可以將要求資料繫結至適當命名的屬性或引數，模型繫結就會成功。</span><span class="sxs-lookup"><span data-stu-id="75c78-134">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="75c78-135">下列範例示範如何使用具有檢視模型的 `ByteArrayModelBinder`：</span><span class="sxs-lookup"><span data-stu-id="75c78-135">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Controllers/ImageController.cs?name=snippet_SaveProfile&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="75c78-136">自訂模型繫結器範例</span><span class="sxs-lookup"><span data-stu-id="75c78-136">Custom model binder sample</span></span>

<span data-ttu-id="75c78-137">在本節中，我們會實作自訂模型繫結：</span><span class="sxs-lookup"><span data-stu-id="75c78-137">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="75c78-138">將內送要求資料轉換成強型別索引鍵引數。</span><span class="sxs-lookup"><span data-stu-id="75c78-138">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="75c78-139">使用 Entity Framework Core 來擷取相關聯的實體。</span><span class="sxs-lookup"><span data-stu-id="75c78-139">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="75c78-140">將相關聯的實體當作引數傳遞至動作方法。</span><span class="sxs-lookup"><span data-stu-id="75c78-140">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="75c78-141">下列範例會在 `Author` 模型上使用 `ModelBinder` 屬性：</span><span class="sxs-lookup"><span data-stu-id="75c78-141">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Data/Author.cs?highlight=6)]

<span data-ttu-id="75c78-142">在上述程式碼中，`ModelBinder` 屬性指定應該用來繫結 `Author` 動作參數的 `IModelBinder` 類型。</span><span class="sxs-lookup"><span data-stu-id="75c78-142">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span>

<span data-ttu-id="75c78-143">下列 `AuthorEntityBinder` 類別可繫結 `Author` 參數，做法是使用 Entity Framework Core 和 `authorId` 從資料來源擷取實體：</span><span class="sxs-lookup"><span data-stu-id="75c78-143">The following `AuthorEntityBinder` class binds an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=snippet_Class)]

> [!NOTE]
> <span data-ttu-id="75c78-144">上述 `AuthorEntityBinder` 類別會說明自訂模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="75c78-144">The preceding `AuthorEntityBinder` class is intended to illustrate a custom model binder.</span></span> <span data-ttu-id="75c78-145">此類別不會說明查閱情節的最佳做法。</span><span class="sxs-lookup"><span data-stu-id="75c78-145">The class isn't intended to illustrate best practices for a lookup scenario.</span></span> <span data-ttu-id="75c78-146">若要進行查閱，請繫結 `authorId`，並在動作方法中查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="75c78-146">For lookup, bind the `authorId` and query the database in an action method.</span></span> <span data-ttu-id="75c78-147">此方法會將模型繫結失敗從 `NotFound` 案例中分離。</span><span class="sxs-lookup"><span data-stu-id="75c78-147">This approach separates model binding failures from `NotFound` cases.</span></span>

<span data-ttu-id="75c78-148">下列程式碼示範如何在動作方法中使用 `AuthorEntityBinder`：</span><span class="sxs-lookup"><span data-stu-id="75c78-148">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=snippet_Get&highlight=2)]

<span data-ttu-id="75c78-149">您可以使用 `ModelBinder` 屬性，將 `AuthorEntityBinder` 套用至未使用預設慣例的參數：</span><span class="sxs-lookup"><span data-stu-id="75c78-149">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=snippet_GetById&highlight=2)]

<span data-ttu-id="75c78-150">在此範例中，由於引數名稱不是預設的 `authorId`，因此會使用 `ModelBinder` 屬性在參數上指定。</span><span class="sxs-lookup"><span data-stu-id="75c78-150">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using the `ModelBinder` attribute.</span></span> <span data-ttu-id="75c78-151">相較于在動作方法中查閱實體，控制器和動作方法都已簡化。</span><span class="sxs-lookup"><span data-stu-id="75c78-151">Both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="75c78-152">使用 Entity Framework Core 擷取作者的邏輯已移至模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="75c78-152">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="75c78-153">當您有數個繫結至 `Author` 模型的方法時，這樣做會明顯簡化許多。</span><span class="sxs-lookup"><span data-stu-id="75c78-153">This can be a considerable simplification when you have several methods that bind to the `Author` model.</span></span>

<span data-ttu-id="75c78-154">您可以將 `ModelBinder` 屬性套用至個別模型屬性 (例如在 ViewModel 上) 或動作方法參數，只指定該類型或動作的特定模型繫結器或模型名稱。</span><span class="sxs-lookup"><span data-stu-id="75c78-154">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="75c78-155">實作 ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="75c78-155">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="75c78-156">除了套用屬性，您還可以實作 `IModelBinderProvider`。</span><span class="sxs-lookup"><span data-stu-id="75c78-156">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="75c78-157">這是內建架構繫結器的實作方式。</span><span class="sxs-lookup"><span data-stu-id="75c78-157">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="75c78-158">當您指定繫結器的作業類型時，您會指定其所產生的引數類型，而**不是**繫結器接受的輸入。</span><span class="sxs-lookup"><span data-stu-id="75c78-158">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="75c78-159">下列繫結器提供者可搭配 `AuthorEntityBinder` 使用。</span><span class="sxs-lookup"><span data-stu-id="75c78-159">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="75c78-160">當它新增至提供者的 MVC 集合時，您不需要在 `Author` 或 `Author` 型別參數上使用 `ModelBinder` 屬性。</span><span class="sxs-lookup"><span data-stu-id="75c78-160">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author`-typed parameters.</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="75c78-161">注意：上述程式碼會傳回 `BinderTypeModelBinder`。</span><span class="sxs-lookup"><span data-stu-id="75c78-161">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="75c78-162">`BinderTypeModelBinder` 會作為模型繫結器的 Factory，並提供相依性插入 (DI)。</span><span class="sxs-lookup"><span data-stu-id="75c78-162">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="75c78-163">`AuthorEntityBinder` 需要 DI 能夠存取 EF Core。</span><span class="sxs-lookup"><span data-stu-id="75c78-163">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="75c78-164">如果您的模型繫結器需要來自 DI 的服務，請使用 `BinderTypeModelBinder`。</span><span class="sxs-lookup"><span data-stu-id="75c78-164">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="75c78-165">若要使用自訂模型繫結器提供者，將它新增 `ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="75c78-165">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

<span data-ttu-id="75c78-166">評估模型繫結器時，會依序查看提供者集合，</span><span class="sxs-lookup"><span data-stu-id="75c78-166">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="75c78-167">並使用第一個傳回繫結器的提供者。</span><span class="sxs-lookup"><span data-stu-id="75c78-167">The first provider that returns a binder is used.</span></span> <span data-ttu-id="75c78-168">將您的提供者新增集合結尾可能會導致在有機會呼叫自訂繫結器之前，即呼叫內建模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="75c78-168">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="75c78-169">在此範例中，會將自訂提供者新增集合開頭，以確保其用於 `Author` 動作引數。</span><span class="sxs-lookup"><span data-stu-id="75c78-169">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

### <a name="polymorphic-model-binding"></a><span data-ttu-id="75c78-170">多型模型系結</span><span class="sxs-lookup"><span data-stu-id="75c78-170">Polymorphic model binding</span></span>

<span data-ttu-id="75c78-171">系結至衍生類型的不同模型，稱為多型模型系結。</span><span class="sxs-lookup"><span data-stu-id="75c78-171">Binding to different models of derived types is known as polymorphic model binding.</span></span> <span data-ttu-id="75c78-172">當要求值必須系結至特定衍生模型類型時，需要多型自訂模型系結。</span><span class="sxs-lookup"><span data-stu-id="75c78-172">Polymorphic custom model binding is required when the request value must be bound to the specific derived model type.</span></span> <span data-ttu-id="75c78-173">多型模型系結：</span><span class="sxs-lookup"><span data-stu-id="75c78-173">Polymorphic model binding:</span></span>

* <span data-ttu-id="75c78-174">對於設計來與所有語言互通的 REST API 而言並不常見。</span><span class="sxs-lookup"><span data-stu-id="75c78-174">Isn't typical for a REST API that's designed to interoperate with all languages.</span></span>
* <span data-ttu-id="75c78-175">使其難以瞭解系結模型的原因。</span><span class="sxs-lookup"><span data-stu-id="75c78-175">Makes it difficult to reason about the bound models.</span></span>

<span data-ttu-id="75c78-176">不過，如果應用程式需要多型模型系結，則執行可能看起來像下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="75c78-176">However, if an app requires polymorphic model binding, an implementation might look like the following code:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/PolymorphicModelBindingSample/ModelBinders/PolymorphicModelBinder.cs?name=snippet)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="75c78-177">建議與最佳做法</span><span class="sxs-lookup"><span data-stu-id="75c78-177">Recommendations and best practices</span></span>

<span data-ttu-id="75c78-178">自訂模型繫結器：</span><span class="sxs-lookup"><span data-stu-id="75c78-178">Custom model binders:</span></span>

- <span data-ttu-id="75c78-179">不應該嘗試設定狀態碼或傳回結果 (例如 404 找不到)。</span><span class="sxs-lookup"><span data-stu-id="75c78-179">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="75c78-180">如果模型繫結失敗，[動作篩選](xref:mvc/controllers/filters)或動作方法本身內的邏輯應該會處理失敗。</span><span class="sxs-lookup"><span data-stu-id="75c78-180">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="75c78-181">最適合用來排除動作方法中的重複程式碼和交叉關注。</span><span class="sxs-lookup"><span data-stu-id="75c78-181">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="75c78-182">一般而言，不應該用來將字串轉換成自訂類型，<xref:System.ComponentModel.TypeConverter> 通常是較好的選擇。</span><span class="sxs-lookup"><span data-stu-id="75c78-182">Typically shouldn't be used to convert a string into a custom type, a <xref:System.ComponentModel.TypeConverter> is usually a better option.</span></span>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="75c78-183">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="75c78-183">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="75c78-184">模型繫結可直接透過模型類型 (傳入作為方法引數) 來執行控制器動作，而不用透過 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="75c78-184">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="75c78-185">內送要求資料與應用程式模型之間的對應是由模型繫結器來處理。</span><span class="sxs-lookup"><span data-stu-id="75c78-185">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="75c78-186">開發人員可以透過實作自訂模型繫結器，來擴充內建模型繫結功能 (不過一般而言，您並不需要撰寫自己的提供者)。</span><span class="sxs-lookup"><span data-stu-id="75c78-186">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

<span data-ttu-id="75c78-187">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="75c78-187">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="default-model-binder-limitations"></a><span data-ttu-id="75c78-188">預設模型繫結器限制</span><span class="sxs-lookup"><span data-stu-id="75c78-188">Default model binder limitations</span></span>

<span data-ttu-id="75c78-189">預設模型繫結器支援大多數的常見 .NET Core 資料類型，因此應該符合大部分開發人員的需求。</span><span class="sxs-lookup"><span data-stu-id="75c78-189">The default model binders support most of the common .NET Core data types and should meet most developers' needs.</span></span> <span data-ttu-id="75c78-190">這些開發人員希望能夠將要求中以文字為主的輸入直接繫結至模型類型。</span><span class="sxs-lookup"><span data-stu-id="75c78-190">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="75c78-191">輸入可能需要進行轉換才能繫結。</span><span class="sxs-lookup"><span data-stu-id="75c78-191">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="75c78-192">例如，當您有可用來查詢模型資料的索引鍵時，</span><span class="sxs-lookup"><span data-stu-id="75c78-192">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="75c78-193">您可以使用自訂模型繫結器，根據索引鍵來擷取資料。</span><span class="sxs-lookup"><span data-stu-id="75c78-193">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="75c78-194">模型繫結檢閱</span><span class="sxs-lookup"><span data-stu-id="75c78-194">Model binding review</span></span>

<span data-ttu-id="75c78-195">模型繫結使用特定定義來描述其作業類型。</span><span class="sxs-lookup"><span data-stu-id="75c78-195">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="75c78-196">「簡單型別」\*\* 是指從輸入中的單一字串進行轉換。</span><span class="sxs-lookup"><span data-stu-id="75c78-196">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="75c78-197">「複雜類型」\*\* 是指從多個輸入值進行轉換。</span><span class="sxs-lookup"><span data-stu-id="75c78-197">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="75c78-198">架構會根據是否有 `TypeConverter` 來判斷是否為不同類型。</span><span class="sxs-lookup"><span data-stu-id="75c78-198">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="75c78-199">如果您有不需要外部資源的簡單 `string` -> `SomeType` 對應，建議您建立型別轉換器。</span><span class="sxs-lookup"><span data-stu-id="75c78-199">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="75c78-200">在您建立自己的自訂模型繫結器之前，建議您先檢閱現有模型繫結器的實作方式。</span><span class="sxs-lookup"><span data-stu-id="75c78-200">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="75c78-201">請考慮<xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinder>可以用來將 base64 編碼的字串轉換成位元組陣列的。</span><span class="sxs-lookup"><span data-stu-id="75c78-201">Consider the <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinder> which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="75c78-202">位元組陣列通常會儲存為檔案或資料庫 BLOB 欄位。</span><span class="sxs-lookup"><span data-stu-id="75c78-202">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="75c78-203">使用 ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="75c78-203">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="75c78-204">Base64 編碼字串可用來代表二進位資料。</span><span class="sxs-lookup"><span data-stu-id="75c78-204">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="75c78-205">例如，影像可以編碼為字串。</span><span class="sxs-lookup"><span data-stu-id="75c78-205">For example, an image can be encoded as a string.</span></span> <span data-ttu-id="75c78-206">此範例會在[Base64String](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/samples/2.x/CustomModelBindingSample/Base64String.txt)中將影像包含為 base64 編碼的字串。</span><span class="sxs-lookup"><span data-stu-id="75c78-206">The sample includes an image as a base64-encoded string in [Base64String.txt](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/samples/2.x/CustomModelBindingSample/Base64String.txt).</span></span>

<span data-ttu-id="75c78-207">ASP.NET Core MVC 接受 Base64 編碼字串，並使用 `ByteArrayModelBinder` 將其轉換成位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="75c78-207">ASP.NET Core MVC can take a base64-encoded string and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="75c78-208">會<xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinderProvider>將`byte[]`引數`ByteArrayModelBinder`對應至：</span><span class="sxs-lookup"><span data-stu-id="75c78-208">The <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinderProvider> maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="75c78-209">建立您自己的自訂模型系結器時，您`IModelBinderProvider`可以執行自己的<xref:Microsoft.AspNetCore.Mvc.ModelBinderAttribute>型別，或使用。</span><span class="sxs-lookup"><span data-stu-id="75c78-209">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the <xref:Microsoft.AspNetCore.Mvc.ModelBinderAttribute>.</span></span>

<span data-ttu-id="75c78-210">下列範例示範如何使用 `ByteArrayModelBinder`，將 Base64 編碼字串轉換成 `byte[]`，並將結果儲存至檔案：</span><span class="sxs-lookup"><span data-stu-id="75c78-210">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Controllers/ImageController.cs?name=post1)]

<span data-ttu-id="75c78-211">您可以使用 [Postman](https://www.getpostman.com/) 等工具，將 Base64 編碼字串張貼至此 API 方法：</span><span class="sxs-lookup"><span data-stu-id="75c78-211">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="75c78-212">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="75c78-212">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="75c78-213">只要繫結器可以將要求資料繫結至適當命名的屬性或引數，模型繫結就會成功。</span><span class="sxs-lookup"><span data-stu-id="75c78-213">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="75c78-214">下列範例示範如何使用具有檢視模型的 `ByteArrayModelBinder`：</span><span class="sxs-lookup"><span data-stu-id="75c78-214">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="75c78-215">自訂模型繫結器範例</span><span class="sxs-lookup"><span data-stu-id="75c78-215">Custom model binder sample</span></span>

<span data-ttu-id="75c78-216">在本節中，我們會實作自訂模型繫結：</span><span class="sxs-lookup"><span data-stu-id="75c78-216">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="75c78-217">將內送要求資料轉換成強型別索引鍵引數。</span><span class="sxs-lookup"><span data-stu-id="75c78-217">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="75c78-218">使用 Entity Framework Core 來擷取相關聯的實體。</span><span class="sxs-lookup"><span data-stu-id="75c78-218">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="75c78-219">將相關聯的實體當作引數傳遞至動作方法。</span><span class="sxs-lookup"><span data-stu-id="75c78-219">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="75c78-220">下列範例會在 `Author` 模型上使用 `ModelBinder` 屬性：</span><span class="sxs-lookup"><span data-stu-id="75c78-220">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Data/Author.cs?highlight=6)]

<span data-ttu-id="75c78-221">在上述程式碼中，`ModelBinder` 屬性指定應該用來繫結 `Author` 動作參數的 `IModelBinder` 類型。</span><span class="sxs-lookup"><span data-stu-id="75c78-221">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span>

<span data-ttu-id="75c78-222">下列 `AuthorEntityBinder` 類別可繫結 `Author` 參數，做法是使用 Entity Framework Core 和 `authorId` 從資料來源擷取實體：</span><span class="sxs-lookup"><span data-stu-id="75c78-222">The following `AuthorEntityBinder` class binds an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

> [!NOTE]
> <span data-ttu-id="75c78-223">上述 `AuthorEntityBinder` 類別會說明自訂模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="75c78-223">The preceding `AuthorEntityBinder` class is intended to illustrate a custom model binder.</span></span> <span data-ttu-id="75c78-224">此類別不會說明查閱情節的最佳做法。</span><span class="sxs-lookup"><span data-stu-id="75c78-224">The class isn't intended to illustrate best practices for a lookup scenario.</span></span> <span data-ttu-id="75c78-225">若要進行查閱，請繫結 `authorId`，並在動作方法中查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="75c78-225">For lookup, bind the `authorId` and query the database in an action method.</span></span> <span data-ttu-id="75c78-226">此方法會將模型繫結失敗從 `NotFound` 案例中分離。</span><span class="sxs-lookup"><span data-stu-id="75c78-226">This approach separates model binding failures from `NotFound` cases.</span></span>

<span data-ttu-id="75c78-227">下列程式碼示範如何在動作方法中使用 `AuthorEntityBinder`：</span><span class="sxs-lookup"><span data-stu-id="75c78-227">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="75c78-228">您可以使用 `ModelBinder` 屬性，將 `AuthorEntityBinder` 套用至未使用預設慣例的參數：</span><span class="sxs-lookup"><span data-stu-id="75c78-228">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="75c78-229">在此範例中，由於引數名稱不是預設的 `authorId`，因此會使用 `ModelBinder` 屬性在參數上指定。</span><span class="sxs-lookup"><span data-stu-id="75c78-229">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using the `ModelBinder` attribute.</span></span> <span data-ttu-id="75c78-230">相較于在動作方法中查閱實體，控制器和動作方法都已簡化。</span><span class="sxs-lookup"><span data-stu-id="75c78-230">Both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="75c78-231">使用 Entity Framework Core 擷取作者的邏輯已移至模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="75c78-231">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="75c78-232">當您有數個繫結至 `Author` 模型的方法時，這樣做會明顯簡化許多。</span><span class="sxs-lookup"><span data-stu-id="75c78-232">This can be a considerable simplification when you have several methods that bind to the `Author` model.</span></span>

<span data-ttu-id="75c78-233">您可以將 `ModelBinder` 屬性套用至個別模型屬性 (例如在 ViewModel 上) 或動作方法參數，只指定該類型或動作的特定模型繫結器或模型名稱。</span><span class="sxs-lookup"><span data-stu-id="75c78-233">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="75c78-234">實作 ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="75c78-234">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="75c78-235">除了套用屬性，您還可以實作 `IModelBinderProvider`。</span><span class="sxs-lookup"><span data-stu-id="75c78-235">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="75c78-236">這是內建架構繫結器的實作方式。</span><span class="sxs-lookup"><span data-stu-id="75c78-236">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="75c78-237">當您指定繫結器的作業類型時，您會指定其所產生的引數類型，而**不是**繫結器接受的輸入。</span><span class="sxs-lookup"><span data-stu-id="75c78-237">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="75c78-238">下列繫結器提供者可搭配 `AuthorEntityBinder` 使用。</span><span class="sxs-lookup"><span data-stu-id="75c78-238">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="75c78-239">當它新增至提供者的 MVC 集合時，您不需要在 `Author` 或 `Author` 型別參數上使用 `ModelBinder` 屬性。</span><span class="sxs-lookup"><span data-stu-id="75c78-239">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author`-typed parameters.</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="75c78-240">注意：上述程式碼會傳回 `BinderTypeModelBinder`。</span><span class="sxs-lookup"><span data-stu-id="75c78-240">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="75c78-241">`BinderTypeModelBinder` 會作為模型繫結器的 Factory，並提供相依性插入 (DI)。</span><span class="sxs-lookup"><span data-stu-id="75c78-241">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="75c78-242">`AuthorEntityBinder` 需要 DI 能夠存取 EF Core。</span><span class="sxs-lookup"><span data-stu-id="75c78-242">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="75c78-243">如果您的模型繫結器需要來自 DI 的服務，請使用 `BinderTypeModelBinder`。</span><span class="sxs-lookup"><span data-stu-id="75c78-243">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="75c78-244">若要使用自訂模型繫結器提供者，將它新增 `ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="75c78-244">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-10)]

<span data-ttu-id="75c78-245">評估模型繫結器時，會依序查看提供者集合，</span><span class="sxs-lookup"><span data-stu-id="75c78-245">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="75c78-246">並使用第一個傳回繫結器的提供者。</span><span class="sxs-lookup"><span data-stu-id="75c78-246">The first provider that returns a binder is used.</span></span> <span data-ttu-id="75c78-247">將您的提供者新增集合結尾可能會導致在有機會呼叫自訂繫結器之前，即呼叫內建模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="75c78-247">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="75c78-248">在此範例中，會將自訂提供者新增集合開頭，以確保其用於 `Author` 動作引數。</span><span class="sxs-lookup"><span data-stu-id="75c78-248">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

### <a name="polymorphic-model-binding"></a><span data-ttu-id="75c78-249">多型模型系結</span><span class="sxs-lookup"><span data-stu-id="75c78-249">Polymorphic model binding</span></span>

<span data-ttu-id="75c78-250">系結至衍生類型的不同模型，稱為多型模型系結。</span><span class="sxs-lookup"><span data-stu-id="75c78-250">Binding to different models of derived types is known as polymorphic model binding.</span></span> <span data-ttu-id="75c78-251">當要求值必須系結至特定衍生模型類型時，需要多型自訂模型系結。</span><span class="sxs-lookup"><span data-stu-id="75c78-251">Polymorphic custom model binding is required when the request value must be bound to the specific derived model type.</span></span> <span data-ttu-id="75c78-252">多型模型系結：</span><span class="sxs-lookup"><span data-stu-id="75c78-252">Polymorphic model binding:</span></span>

* <span data-ttu-id="75c78-253">對於設計來與所有語言互通的 REST API 而言並不常見。</span><span class="sxs-lookup"><span data-stu-id="75c78-253">Isn't typical for a REST API that's designed to interoperate with all languages.</span></span>
* <span data-ttu-id="75c78-254">使其難以瞭解系結模型的原因。</span><span class="sxs-lookup"><span data-stu-id="75c78-254">Makes it difficult to reason about the bound models.</span></span>

<span data-ttu-id="75c78-255">不過，如果應用程式需要多型模型系結，則執行可能看起來像下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="75c78-255">However, if an app requires polymorphic model binding, an implementation might look like the following code:</span></span>

[!code-csharp[](custom-model-binding/samples/3.x/PolymorphicModelBindingSample/ModelBinders/PolymorphicModelBinder.cs?name=snippet)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="75c78-256">建議與最佳做法</span><span class="sxs-lookup"><span data-stu-id="75c78-256">Recommendations and best practices</span></span>

<span data-ttu-id="75c78-257">自訂模型繫結器：</span><span class="sxs-lookup"><span data-stu-id="75c78-257">Custom model binders:</span></span>

- <span data-ttu-id="75c78-258">不應該嘗試設定狀態碼或傳回結果 (例如 404 找不到)。</span><span class="sxs-lookup"><span data-stu-id="75c78-258">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="75c78-259">如果模型繫結失敗，[動作篩選](xref:mvc/controllers/filters)或動作方法本身內的邏輯應該會處理失敗。</span><span class="sxs-lookup"><span data-stu-id="75c78-259">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="75c78-260">最適合用來排除動作方法中的重複程式碼和交叉關注。</span><span class="sxs-lookup"><span data-stu-id="75c78-260">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="75c78-261">一般而言，不應該用來將字串轉換成自訂類型，<xref:System.ComponentModel.TypeConverter> 通常是較好的選擇。</span><span class="sxs-lookup"><span data-stu-id="75c78-261">Typically shouldn't be used to convert a string into a custom type, a <xref:System.ComponentModel.TypeConverter> is usually a better option.</span></span>

::: moniker-end
