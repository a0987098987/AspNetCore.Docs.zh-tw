---
title: ASP.NET Core Web API 中的自訂格式器
author: rick-anderson
description: 了解如何建立和使用 ASP.NET Core 中的 Web API 自訂格式器。
ms.author: tdykstra
ms.date: 02/08/2017
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: 2861a15a80725dcc237d33313a24822cf8aa9c7e
ms.sourcegitcommit: e1cc4c1ef6c9e07918a609d5ad7fadcb6abe3e12
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2019
ms.locfileid: "53997288"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a><span data-ttu-id="436cf-103">ASP.NET Core Web API 中的自訂格式器</span><span class="sxs-lookup"><span data-stu-id="436cf-103">Custom formatters in ASP.NET Core Web API</span></span>

<span data-ttu-id="436cf-104">作者：[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="436cf-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="436cf-105">ASP.NET Core MVC 內建支援在 Web API 中使用 JSON 或 XML 的資料交換。</span><span class="sxs-lookup"><span data-stu-id="436cf-105">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON or XML.</span></span> <span data-ttu-id="436cf-106">本文說明如何藉由建立自訂的格式器來新增對其他格式的支援。</span><span class="sxs-lookup"><span data-stu-id="436cf-106">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="436cf-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="436cf-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="436cf-108">自訂格式器的使用時機</span><span class="sxs-lookup"><span data-stu-id="436cf-108">When to use custom formatters</span></span>

<span data-ttu-id="436cf-109">如果您希望[內容交涉](xref:web-api/advanced/formatting#content-negotiation)程序支援某些內容類型，但內建的格式器 (JSON 和 XML) 不支援這些內容類型時，即可使用自訂的格式器。</span><span class="sxs-lookup"><span data-stu-id="436cf-109">Use a custom formatter when you want the [content negotiation](xref:web-api/advanced/formatting#content-negotiation) process to support a content type that isn't supported by the built-in formatters (JSON and XML).</span></span>

<span data-ttu-id="436cf-110">例如，您有些 Web API 用戶端可以處理 [Protobuf](https://github.com/google/protobuf) 格式，因此您希望搭配使用 Protobuf 與這些用戶端，以便更有效率。</span><span class="sxs-lookup"><span data-stu-id="436cf-110">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span> <span data-ttu-id="436cf-111">或者，您可能會想要 Web API 以 [vCard](https://wikipedia.org/wiki/VCard) 格式 (其為交換連絡人資料的常用格式) 來傳送連絡人名稱和地址。</span><span class="sxs-lookup"><span data-stu-id="436cf-111">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="436cf-112">本文提供的範例應用程式會實作簡單的 vCard 格式器。</span><span class="sxs-lookup"><span data-stu-id="436cf-112">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="436cf-113">如何使用自訂格式器的概觀</span><span class="sxs-lookup"><span data-stu-id="436cf-113">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="436cf-114">自訂格式器的建立與使用步驟如下：</span><span class="sxs-lookup"><span data-stu-id="436cf-114">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="436cf-115">如果您想要將資料序列化以傳送到用戶端，請建立輸出格式器類別。</span><span class="sxs-lookup"><span data-stu-id="436cf-115">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="436cf-116">如果您想要將從用戶端接收的資料還原序列化，請建立輸入格式器類別。</span><span class="sxs-lookup"><span data-stu-id="436cf-116">Create an input formatter class if you want to deserialize data received from the client.</span></span>
* <span data-ttu-id="436cf-117">將您的格式器執行個體新增至 [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions) 中的 `InputFormatters` 和 `OutputFormatters` 集合。</span><span class="sxs-lookup"><span data-stu-id="436cf-117">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="436cf-118">下列各節將提供每個步驟的指引與程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="436cf-118">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="436cf-119">如何建立自訂格式器類別</span><span class="sxs-lookup"><span data-stu-id="436cf-119">How to create a custom formatter class</span></span>

<span data-ttu-id="436cf-120">若要建立格式器：</span><span class="sxs-lookup"><span data-stu-id="436cf-120">To create a formatter:</span></span>

* <span data-ttu-id="436cf-121">請從適當的基底類別衍生類別。</span><span class="sxs-lookup"><span data-stu-id="436cf-121">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="436cf-122">在建構函式中指定有效的媒體類型和編碼方式。</span><span class="sxs-lookup"><span data-stu-id="436cf-122">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="436cf-123">覆寫 `CanReadType`/`CanWriteType` 方法</span><span class="sxs-lookup"><span data-stu-id="436cf-123">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="436cf-124">覆寫 `ReadRequestBodyAsync`/`WriteResponseBodyAsync` 方法</span><span class="sxs-lookup"><span data-stu-id="436cf-124">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="436cf-125">從適當的基底類別衍生</span><span class="sxs-lookup"><span data-stu-id="436cf-125">Derive from the appropriate base class</span></span>

<span data-ttu-id="436cf-126">如需文字媒體類型 (例如 vCard)，請從 [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) 或 [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) 基底類別來衍生。</span><span class="sxs-lookup"><span data-stu-id="436cf-126">For text media types (for example, vCard), derive from the [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="436cf-127">若需要輸入格式器範例，請參考[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)。</span><span class="sxs-lookup"><span data-stu-id="436cf-127">For an input formatter example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

<span data-ttu-id="436cf-128">如需二進位類型，請從 [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) 或 [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) 基底類別來衍生。</span><span class="sxs-lookup"><span data-stu-id="436cf-128">For binary types, derive from the [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="436cf-129">指定有效的媒體類型和編碼方式</span><span class="sxs-lookup"><span data-stu-id="436cf-129">Specify valid media types and encodings</span></span>

<span data-ttu-id="436cf-130">在建構函式中，您可以新增 `SupportedMediaTypes` 和 `SupportedEncodings` 集合，以指定有效的媒體類型和編碼方式。</span><span class="sxs-lookup"><span data-stu-id="436cf-130">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

<span data-ttu-id="436cf-131">若需要輸入格式器範例，請參考[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)。</span><span class="sxs-lookup"><span data-stu-id="436cf-131">For an input formatter example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

> [!NOTE]
> <span data-ttu-id="436cf-132">您無法在格式器類別中執行建構函式的相依性插入。</span><span class="sxs-lookup"><span data-stu-id="436cf-132">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="436cf-133">舉例來說，您無法藉由將記錄器參數新增至建構函式，而取得記錄器。</span><span class="sxs-lookup"><span data-stu-id="436cf-133">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="436cf-134">若要存取服務，您必須使用可傳入方法的內容物件。</span><span class="sxs-lookup"><span data-stu-id="436cf-134">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="436cf-135">[下列](#read-write)程式碼範例會示範這項操作。</span><span class="sxs-lookup"><span data-stu-id="436cf-135">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="436cf-136">覆寫 CanReadType/CanWriteType</span><span class="sxs-lookup"><span data-stu-id="436cf-136">Override CanReadType/CanWriteType</span></span>

<span data-ttu-id="436cf-137">您可以覆寫 `CanReadType` 或 `CanWriteType` 方法，以指定要序列化或還原序列化的類型。</span><span class="sxs-lookup"><span data-stu-id="436cf-137">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="436cf-138">例如，您可能只能透過 `Contact` 類型建立 vCard 文字，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="436cf-138">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

<span data-ttu-id="436cf-139">若需要輸入格式器範例，請參考[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)。</span><span class="sxs-lookup"><span data-stu-id="436cf-139">For an input formatter example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="436cf-140">CanWriteResult 方法</span><span class="sxs-lookup"><span data-stu-id="436cf-140">The CanWriteResult method</span></span>

<span data-ttu-id="436cf-141">在某些情況下，您必須覆寫 `CanWriteResult` 而不是 `CanWriteType`。</span><span class="sxs-lookup"><span data-stu-id="436cf-141">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="436cf-142">如果符合下列所有條件，請使用 `CanWriteResult`：</span><span class="sxs-lookup"><span data-stu-id="436cf-142">Use `CanWriteResult` if the following conditions are true:</span></span>

* <span data-ttu-id="436cf-143">您的動作方法會傳回模型類別。</span><span class="sxs-lookup"><span data-stu-id="436cf-143">Your action method returns a model class.</span></span>
* <span data-ttu-id="436cf-144">在執行階段期間，可能會傳回衍生的類別。</span><span class="sxs-lookup"><span data-stu-id="436cf-144">There are derived classes which might be returned at runtime.</span></span>
* <span data-ttu-id="436cf-145">您必須知道在執行階段期間，動作傳回的衍生類別是哪一個。</span><span class="sxs-lookup"><span data-stu-id="436cf-145">You need to know at runtime which derived class was returned by the action.</span></span>

<span data-ttu-id="436cf-146">例如，假設您的動作方法簽章傳回 `Person` 類型，但它可能會傳回衍生自 `Person` 的 `Student` 或 `Instructor` 類型。</span><span class="sxs-lookup"><span data-stu-id="436cf-146">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="436cf-147">如果您希望格式器只處理 `Student` 物件，請檢查您提供給 `CanWriteResult` 方法之內容物件中的[物件](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object)類型。</span><span class="sxs-lookup"><span data-stu-id="436cf-147">If you want your formatter to handle only `Student` objects, check the type of [Object](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="436cf-148">請注意，當動作方法傳回 `IActionResult` 時，您不需要使用 `CanWriteResult`；在此情況下，`CanWriteType` 方法會接收執行階段類型。</span><span class="sxs-lookup"><span data-stu-id="436cf-148">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="436cf-149">覆寫 ReadRequestBodyAsync/WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="436cf-149">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span>

<span data-ttu-id="436cf-150">您可以在 `ReadRequestBodyAsync` 或 `WriteResponseBodyAsync` 中進行還原序列化或序列化的實際工作。</span><span class="sxs-lookup"><span data-stu-id="436cf-150">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span> <span data-ttu-id="436cf-151">下列範例中，醒目標示的程式碼行示範如何透過相依性插入容器以取得服務 (您無法透過建構函式參數來取得)。</span><span class="sxs-lookup"><span data-stu-id="436cf-151">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

<span data-ttu-id="436cf-152">若需要輸入格式器範例，請參考[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)。</span><span class="sxs-lookup"><span data-stu-id="436cf-152">For an input formatter example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="436cf-153">如何設定 MVC 以使用自訂格式器</span><span class="sxs-lookup"><span data-stu-id="436cf-153">How to configure MVC to use a custom formatter</span></span>

<span data-ttu-id="436cf-154">若要使用自訂格式器，請將格式器類別的執行個體新增至 `InputFormatters` 或 `OutputFormatters` 集合。</span><span class="sxs-lookup"><span data-stu-id="436cf-154">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="436cf-155">系統會依據您插入格式器的順序進行評估。</span><span class="sxs-lookup"><span data-stu-id="436cf-155">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="436cf-156">第一個會優先使用。</span><span class="sxs-lookup"><span data-stu-id="436cf-156">The first one takes precedence.</span></span>

## <a name="next-steps"></a><span data-ttu-id="436cf-157">後續步驟</span><span class="sxs-lookup"><span data-stu-id="436cf-157">Next steps</span></span>

* [<span data-ttu-id="436cf-158">GitHub 上的純文字格式器範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="436cf-158">Plain text formatter sample code on GitHub.</span></span>](https://github.com/aspnet/Entropy/tree/master/samples/Mvc.Formatters)
* <span data-ttu-id="436cf-159">[此文件的範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)，其會實作簡單的 vCard 輸入和輸出格式器。</span><span class="sxs-lookup"><span data-stu-id="436cf-159">[Sample app for this doc](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span> <span data-ttu-id="436cf-160">應用程式會讀取並寫入 vCard，如下範例所示：</span><span class="sxs-lookup"><span data-stu-id="436cf-160">The apps reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="436cf-161">若要查看 vCard 的輸出，請執行應用程式，並使用 Accept 標頭 "text/vcard" 將 Get 要求傳送給 `http://localhost:63313/api/contacts/` (從 Visual Studio 執行時) 或 `http://localhost:5000/api/contacts/` (從命令列執行時)。</span><span class="sxs-lookup"><span data-stu-id="436cf-161">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="436cf-162">若要將 vCard 新增至連絡人的記憶體內部集合，請使用 Content-Type 標頭 "text/vcard" 與主體中的 vCard 文字 (如上述範例的格式)，將 Post 要求傳送至相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="436cf-162">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
