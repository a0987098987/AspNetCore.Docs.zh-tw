---
title: ASP.NET Core Web API 中的自訂格式器
author: rick-anderson
description: 了解如何建立和使用 ASP.NET Core 中的 Web API 自訂格式器。
ms.author: riande
ms.date: 06/25/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: c6ec73c028c7003a40b2f09b631bdc2c976686fa
ms.sourcegitcommit: 895e952aec11c91d703fbdd3640a979307b8cc67
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85793362"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a><span data-ttu-id="2b3d6-103">ASP.NET Core Web API 中的自訂格式器</span><span class="sxs-lookup"><span data-stu-id="2b3d6-103">Custom formatters in ASP.NET Core Web API</span></span>

<span data-ttu-id="2b3d6-104">By [Kirk Larkin](https://twitter.com/serpent5)和[Tom 作者: dykstra](https://github.com/tdykstra)。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-104">By [Kirk Larkin](https://twitter.com/serpent5) and [Tom Dykstra](https://github.com/tdykstra).</span></span>

<span data-ttu-id="2b3d6-105">ASP.NET Core MVC 支援使用輸入和輸出格式器在 Web Api 中進行資料交換。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-105">ASP.NET Core MVC supports data exchange in Web APIs using input and output formatters.</span></span> <span data-ttu-id="2b3d6-106">[模型](xref:mvc/models/model-binding)系結會使用輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-106">Input formatters are used by [Model Binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="2b3d6-107">輸出格式器是用來[格式化回應](xref:web-api/advanced/formatting)。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-107">Output formatters are used to [format responses](xref:web-api/advanced/formatting).</span></span>

<span data-ttu-id="2b3d6-108">架構會為 JSON 和 XML 提供內建的輸入和輸出格式器。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-108">The framework provides built-in input and output formatters for JSON and XML.</span></span> <span data-ttu-id="2b3d6-109">它會為純文字提供內建的輸出格式器，但不會提供純文字的輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-109">It provides a built-in output formatter for plain text, but doesn't provide an input formatter for plain text.</span></span>

<span data-ttu-id="2b3d6-110">本文說明如何藉由建立自訂的格式器來新增對其他格式的支援。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-110">This article shows how to add support for additional formats by creating custom formatters.</span></span> <span data-ttu-id="2b3d6-111">如需自訂純文字輸入格式器的範例，請參閱 GitHub 上的[TextPlainInputFormatter](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.Formatters/TextPlainInputFormatter.cs) 。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-111">For an example of a custom plain text input formatter, see [TextPlainInputFormatter](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.Formatters/TextPlainInputFormatter.cs) on GitHub.</span></span>

<span data-ttu-id="2b3d6-112">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="2b3d6-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="2b3d6-113">自訂格式器的使用時機</span><span class="sxs-lookup"><span data-stu-id="2b3d6-113">When to use custom formatters</span></span>

<span data-ttu-id="2b3d6-114">使用自訂格式器來新增不是由內建格式器所處理之內容類型的支援。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-114">Use a custom formatter to add support for a content type that isn't handled by the built-in formatters.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="2b3d6-115">如何使用自訂格式器的概觀</span><span class="sxs-lookup"><span data-stu-id="2b3d6-115">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="2b3d6-116">若要建立自訂格式器：</span><span class="sxs-lookup"><span data-stu-id="2b3d6-116">To create a custom formatter:</span></span>

* <span data-ttu-id="2b3d6-117">若要將傳送至用戶端的資料序列化，請建立輸出格式器類別。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-117">For serializing data sent to the client, create an output formatter class.</span></span>
* <span data-ttu-id="2b3d6-118">若要將從用戶端接收的資料還原序列化，請建立輸入格式器類別。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-118">For deserializing data received from the client, create an input formatter class.</span></span>
* <span data-ttu-id="2b3d6-119">將格式器類別的實例加入 `InputFormatters` 至 `OutputFormatters` 中的和集合 <xref:Microsoft.AspNetCore.Mvc.MvcOptions> 。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-119">Add instances of formatter classes to the `InputFormatters` and `OutputFormatters` collections in <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="2b3d6-120">如何建立自訂格式器類別</span><span class="sxs-lookup"><span data-stu-id="2b3d6-120">How to create a custom formatter class</span></span>

<span data-ttu-id="2b3d6-121">若要建立格式器：</span><span class="sxs-lookup"><span data-stu-id="2b3d6-121">To create a formatter:</span></span>

* <span data-ttu-id="2b3d6-122">請從適當的基底類別衍生類別。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-122">Derive the class from the appropriate base class.</span></span> <span data-ttu-id="2b3d6-123">範例應用程式衍生自 <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter> 和 <xref:Microsoft.AspNetCore.Mvc.Formatters.TextInputFormatter> 。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-123">The sample app derives from <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter> and <xref:Microsoft.AspNetCore.Mvc.Formatters.TextInputFormatter>.</span></span>
* <span data-ttu-id="2b3d6-124">在建構函式中指定有效的媒體類型和編碼方式。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-124">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="2b3d6-125">覆寫 <xref:Microsoft.AspNetCore.Mvc.Formatters.InputFormatter.CanReadType%2A> 和 <xref:Microsoft.AspNetCore.Mvc.Formatters.OutputFormatter.CanWriteType%2A> 方法。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-125">Override the <xref:Microsoft.AspNetCore.Mvc.Formatters.InputFormatter.CanReadType%2A> and <xref:Microsoft.AspNetCore.Mvc.Formatters.OutputFormatter.CanWriteType%2A> methods.</span></span>
* <span data-ttu-id="2b3d6-126">覆寫 <xref:Microsoft.AspNetCore.Mvc.Formatters.InputFormatter.ReadRequestBodyAsync%2A> 和 `WriteResponseBodyAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-126">Override the <xref:Microsoft.AspNetCore.Mvc.Formatters.InputFormatter.ReadRequestBodyAsync%2A> and `WriteResponseBodyAsync` methods.</span></span>

<span data-ttu-id="2b3d6-127">下列程式碼顯示 `VcardOutputFormatter` [範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/samples)中的類別：</span><span class="sxs-lookup"><span data-stu-id="2b3d6-127">The following code shows the `VcardOutputFormatter` class from the [sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/samples):</span></span>

[!code-csharp[](custom-formatters/samples/3.x/CustomFormattersSample/Formatters/VcardOutputFormatter.cs?name=snippet_Class)]
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="2b3d6-128">從適當的基底類別衍生</span><span class="sxs-lookup"><span data-stu-id="2b3d6-128">Derive from the appropriate base class</span></span>

<span data-ttu-id="2b3d6-129">針對文字媒體類型（例如，vCard），衍生自 <xref:Microsoft.AspNetCore.Mvc.Formatters.TextInputFormatter> 或 <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter> 基類。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-129">For text media types (for example, vCard), derive from the <xref:Microsoft.AspNetCore.Mvc.Formatters.TextInputFormatter> or <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter> base class.</span></span>

[!code-csharp[](custom-formatters/samples/3.x/CustomFormattersSample/Formatters/VcardOutputFormatter.cs?name=snippet_ClassDeclaration)]

<span data-ttu-id="2b3d6-130">若為二進位類型，請從 <xref:Microsoft.AspNetCore.Mvc.Formatters.InputFormatter> 或 <xref:Microsoft.AspNetCore.Mvc.Formatters.OutputFormatter> 基類衍生。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-130">For binary types, derive from the <xref:Microsoft.AspNetCore.Mvc.Formatters.InputFormatter> or <xref:Microsoft.AspNetCore.Mvc.Formatters.OutputFormatter> base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="2b3d6-131">指定有效的媒體類型和編碼方式</span><span class="sxs-lookup"><span data-stu-id="2b3d6-131">Specify valid media types and encodings</span></span>

<span data-ttu-id="2b3d6-132">在建構函式中，您可以新增 `SupportedMediaTypes` 和 `SupportedEncodings` 集合，以指定有效的媒體類型和編碼方式。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-132">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[](custom-formatters/samples/3.x/CustomFormattersSample/Formatters/VcardOutputFormatter.cs?name=snippet_ctor)]

<span data-ttu-id="2b3d6-133">格式器類別**無法**針對其相依性使用函式插入。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-133">A formatter class can **not** use constructor injection for its dependencies.</span></span> <span data-ttu-id="2b3d6-134">例如， `ILogger<VcardOutputFormatter>` 無法將當做參數加入至函式。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-134">For example, `ILogger<VcardOutputFormatter>` cannot be added as a parameter to the constructor.</span></span> <span data-ttu-id="2b3d6-135">若要存取服務，請使用傳入方法的內容物件。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-135">To access services, use the context object that gets passed in to the methods.</span></span> <span data-ttu-id="2b3d6-136">本文中的程式碼範例和[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/samples)示範如何執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-136">A code example in this article and the [sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/samples) show how to do this.</span></span>

### <a name="override-canreadtype-and-canwritetype"></a><span data-ttu-id="2b3d6-137">覆寫 CanReadType 和 CanWriteType</span><span class="sxs-lookup"><span data-stu-id="2b3d6-137">Override CanReadType and CanWriteType</span></span>

<span data-ttu-id="2b3d6-138">藉由覆寫或方法，指定要還原序列化的型別或從中序列化 `CanReadType` `CanWriteType` 。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-138">Specify the type to deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="2b3d6-139">例如，從類型建立 vCard 文字 `Contact` ，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-139">For example, creating vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[](custom-formatters/samples/3.x/CustomFormattersSample/Formatters/VcardOutputFormatter.cs?name=snippet_CanWriteType)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="2b3d6-140">CanWriteResult 方法</span><span class="sxs-lookup"><span data-stu-id="2b3d6-140">The CanWriteResult method</span></span>

<span data-ttu-id="2b3d6-141">在某些情況下， `CanWriteResult` 必須覆寫而不是 `CanWriteType` 。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-141">In some scenarios, `CanWriteResult` must be overridden rather than `CanWriteType`.</span></span> <span data-ttu-id="2b3d6-142">如果符合下列所有條件，請使用 `CanWriteResult`：</span><span class="sxs-lookup"><span data-stu-id="2b3d6-142">Use `CanWriteResult` if the following conditions are true:</span></span>

* <span data-ttu-id="2b3d6-143">動作方法會傳回模型類別。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-143">The action method returns a model class.</span></span>
* <span data-ttu-id="2b3d6-144">在執行階段期間，可能會傳回衍生的類別。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-144">There are derived classes which might be returned at runtime.</span></span>
* <span data-ttu-id="2b3d6-145">動作所傳回的衍生類別在執行時間必須是已知的。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-145">The derived class returned by the action must be known at runtime.</span></span>

<span data-ttu-id="2b3d6-146">例如，假設動作方法：</span><span class="sxs-lookup"><span data-stu-id="2b3d6-146">For example, suppose the action method:</span></span>

* <span data-ttu-id="2b3d6-147">簽章會傳回 `Person` 類型。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-147">Signature returns a `Person` type.</span></span>
* <span data-ttu-id="2b3d6-148">可以傳回 `Student` `Instructor` 衍生自的或類型 `Person` 。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-148">Can return a `Student` or `Instructor` type that derives from `Person`.</span></span> 

<span data-ttu-id="2b3d6-149">若要讓格式器只處理 `Student` 物件，請 <xref:Microsoft.AspNetCore.Mvc.Formatters.OutputFormatterCanWriteContext.Object> 在提供給方法的內容物件中，檢查的類型 `CanWriteResult` 。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-149">For the formatter to handle only `Student` objects, check the type of <xref:Microsoft.AspNetCore.Mvc.Formatters.OutputFormatterCanWriteContext.Object> in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="2b3d6-150">當動作方法傳回時 `IActionResult` ：</span><span class="sxs-lookup"><span data-stu-id="2b3d6-150">When the action method returns `IActionResult`:</span></span>

* <span data-ttu-id="2b3d6-151">不需要使用 `CanWriteResult` 。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-151">It's not necessary to use `CanWriteResult`.</span></span>
* <span data-ttu-id="2b3d6-152">`CanWriteType`方法會接收執行時間類型。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-152">The `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>

### <a name="override-readrequestbodyasync-and-writeresponsebodyasync"></a><span data-ttu-id="2b3d6-153">覆寫 ReadRequestBodyAsync 和 WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="2b3d6-153">Override ReadRequestBodyAsync and WriteResponseBodyAsync</span></span>

<span data-ttu-id="2b3d6-154">還原序列化或序列化會在 `ReadRequestBodyAsync` 或中執行 `WriteResponseBodyAsync` 。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-154">Deserialization or serialization is performed in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span> <span data-ttu-id="2b3d6-155">下列範例顯示如何從相依性插入容器中取得服務。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-155">The following example shows how to get services from the dependency injection container.</span></span> <span data-ttu-id="2b3d6-156">無法從函數參數取得服務。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-156">Services can't be obtained from constructor parameters.</span></span>

[!code-csharp[](custom-formatters/samples/3.x/CustomFormattersSample/Formatters/VcardOutputFormatter.cs?name=snippet_WriteResponseBodyAsync)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="2b3d6-157">如何設定 MVC 以使用自訂格式器</span><span class="sxs-lookup"><span data-stu-id="2b3d6-157">How to configure MVC to use a custom formatter</span></span>

<span data-ttu-id="2b3d6-158">若要使用自訂格式器，請將格式器類別的執行個體新增至 `InputFormatters` 或 `OutputFormatters` 集合。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-158">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](custom-formatters/samples/3.x/CustomFormattersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](custom-formatters/samples/2.x/CustomFormattersSample/Startup.cs?name=mvcoptions&highlight=3-4)]

::: moniker-end

<span data-ttu-id="2b3d6-159">系統會依據您插入格式器的順序進行評估。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-159">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="2b3d6-160">第一個會優先使用。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-160">The first one takes precedence.</span></span>

## <a name="the-complete-vcardinputformatter-class"></a><span data-ttu-id="2b3d6-161">完整的 `VcardInputFormatter` 類別</span><span class="sxs-lookup"><span data-stu-id="2b3d6-161">The complete `VcardInputFormatter` class</span></span>

<span data-ttu-id="2b3d6-162">下列程式碼顯示 `VcardInputFormatter` [範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/samples)中的類別：</span><span class="sxs-lookup"><span data-stu-id="2b3d6-162">The following code shows the `VcardInputFormatter` class from the [sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/samples):</span></span>

[!code-csharp[](custom-formatters/samples/3.x/CustomFormattersSample/Formatters/VcardInputFormatter.cs?name=snippet_Class)]

## <a name="test-the-app"></a><span data-ttu-id="2b3d6-163">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="2b3d6-163">Test the app</span></span>

<span data-ttu-id="2b3d6-164">[執行本文的範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/samples)，它會實行基本的 vCard 輸入和輸出格式器。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-164">[Run the sample app for this article](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/samples), which implements basic vCard input and output formatters.</span></span> <span data-ttu-id="2b3d6-165">應用程式會讀取並寫入類似下列的電子名片：</span><span class="sxs-lookup"><span data-stu-id="2b3d6-165">The app reads and writes vCards similar to the following:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
END:VCARD
```

<span data-ttu-id="2b3d6-166">若要查看 vCard 輸出，請執行應用程式，並將具有 Accept 標頭的 Get 要求傳送 `text/vcard` 至 `https://localhost:5001/api/contacts` 。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-166">To see vCard output, run the app and send a Get request with Accept header `text/vcard` to `https://localhost:5001/api/contacts`.</span></span>

<span data-ttu-id="2b3d6-167">若要將 vCard 新增至連絡人的記憶體中集合：</span><span class="sxs-lookup"><span data-stu-id="2b3d6-167">To add a vCard to the in-memory collection of contacts:</span></span>

* <span data-ttu-id="2b3d6-168">`Post` `/api/contacts` 使用類似 Postman 的工具，將要求傳送至。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-168">Send a `Post` request to `/api/contacts` with a tool like Postman.</span></span>
* <span data-ttu-id="2b3d6-169">將 `Content-Type` 標頭設定為 `text/vcard`。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-169">Set the `Content-Type` header to `text/vcard`.</span></span>
* <span data-ttu-id="2b3d6-170">設定 `vCard` 主體中的文字，格式如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="2b3d6-170">Set `vCard` text in the body, formatted like the preceding example.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2b3d6-171">其他資源</span><span class="sxs-lookup"><span data-stu-id="2b3d6-171">Additional resources</span></span>

* <xref:web-api/advanced/formatting>
* <xref:grpc/dotnet-grpc>
