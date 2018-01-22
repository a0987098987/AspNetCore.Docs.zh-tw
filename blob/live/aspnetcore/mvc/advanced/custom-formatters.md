---
title: "ASP.NET Core MVC web 應用程式開發介面中的自訂格式器"
author: tdykstra
description: "了解如何建立和使用 ASP.NET Core 中的 web Api 的自訂格式器。"
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 3a6474fdae29b170978226de74d523b20a16cd0c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a><span data-ttu-id="893ab-103">ASP.NET Core MVC web 應用程式開發介面中的自訂格式器</span><span class="sxs-lookup"><span data-stu-id="893ab-103">Custom formatters in ASP.NET Core MVC web APIs</span></span>

<span data-ttu-id="893ab-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="893ab-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="893ab-105">ASP.NET Core MVC web 應用程式開發介面中有內建支援的資料交換，使用 JSON、 XML 或純文字格式。</span><span class="sxs-lookup"><span data-stu-id="893ab-105">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="893ab-106">本文將說明如何藉由建立自訂的格式器加入其他格式的支援。</span><span class="sxs-lookup"><span data-stu-id="893ab-106">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="893ab-107">[檢視或從 GitHub 下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample)。</span><span class="sxs-lookup"><span data-stu-id="893ab-107">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="893ab-108">使用自訂的格式器的時機</span><span class="sxs-lookup"><span data-stu-id="893ab-108">When to use custom formatters</span></span>

<span data-ttu-id="893ab-109">當您想使用自訂的格式器[內容交涉](xref:mvc/models/formatting)支援內容類型不支援的內建的格式器 （JSON、 XML 和純文字） 的程序。</span><span class="sxs-lookup"><span data-stu-id="893ab-109">Use a custom formatter when you want the [content negotiation](xref:mvc/models/formatting) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="893ab-110">例如，如果您的 web API 的用戶端的部分可以處理[Protobuf](https://github.com/google/protobuf)格式，您可以使用 Protobuf 這些用戶端，因為它是更有效率。</span><span class="sxs-lookup"><span data-stu-id="893ab-110">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span>  <span data-ttu-id="893ab-111">您可能會想要傳送連絡人的姓名和地址中的 web API 或者[vCard](https://wikipedia.org/wiki/VCard) ，常用的格式來交換連絡人的資料格式。</span><span class="sxs-lookup"><span data-stu-id="893ab-111">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="893ab-112">本文提供的範例應用程式會實作簡單的 vCard 格式器。</span><span class="sxs-lookup"><span data-stu-id="893ab-112">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="893ab-113">如何使用自訂的格式器的概觀</span><span class="sxs-lookup"><span data-stu-id="893ab-113">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="893ab-114">建立及使用自訂格式子，步驟如下：</span><span class="sxs-lookup"><span data-stu-id="893ab-114">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="893ab-115">如果您想要將資料序列化至傳送到用戶端，請建立輸出格式子類別。</span><span class="sxs-lookup"><span data-stu-id="893ab-115">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="893ab-116">如果您想要從用戶端收到的資料還原序列化，請建立輸入格式子類別。</span><span class="sxs-lookup"><span data-stu-id="893ab-116">Create an input formatter class if you want to deserialize data received from the client.</span></span> 
* <span data-ttu-id="893ab-117">您要的格式器的執行個體加入`InputFormatters`和`OutputFormatters`中的集合[MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions)。</span><span class="sxs-lookup"><span data-stu-id="893ab-117">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="893ab-118">下列章節將提供指導方針與範例程式碼的每個步驟。</span><span class="sxs-lookup"><span data-stu-id="893ab-118">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="893ab-119">如何建立自訂格式子類別</span><span class="sxs-lookup"><span data-stu-id="893ab-119">How to create a custom formatter class</span></span>

<span data-ttu-id="893ab-120">若要建立格式器：</span><span class="sxs-lookup"><span data-stu-id="893ab-120">To create a formatter:</span></span>

* <span data-ttu-id="893ab-121">衍生自適當的基底類別的類別。</span><span class="sxs-lookup"><span data-stu-id="893ab-121">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="893ab-122">建構函式中指定有效的媒體類型和編碼。</span><span class="sxs-lookup"><span data-stu-id="893ab-122">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="893ab-123">覆寫`CanReadType` / `CanWriteType`方法</span><span class="sxs-lookup"><span data-stu-id="893ab-123">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="893ab-124">覆寫`ReadRequestBodyAsync` / `WriteResponseBodyAsync`方法</span><span class="sxs-lookup"><span data-stu-id="893ab-124">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="893ab-125">衍生自適當的基底類別</span><span class="sxs-lookup"><span data-stu-id="893ab-125">Derive from the appropriate base class</span></span>

<span data-ttu-id="893ab-126">文字媒體類型 (例如，vCard)，衍生自[TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter)或[TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter)基底類別。</span><span class="sxs-lookup"><span data-stu-id="893ab-126">For text media types (for example, vCard), derive from the [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="893ab-127">二進位型別，衍生自[InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter)或[OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter)基底類別。</span><span class="sxs-lookup"><span data-stu-id="893ab-127">For binary types, derive from the [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="893ab-128">指定有效的媒體類型和編碼</span><span class="sxs-lookup"><span data-stu-id="893ab-128">Specify valid media types and encodings</span></span>

<span data-ttu-id="893ab-129">在建構函式，方法是加入指定有效的媒體類型和編碼`SupportedMediaTypes`和`SupportedEncodings`集合。</span><span class="sxs-lookup"><span data-stu-id="893ab-129">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> <span data-ttu-id="893ab-130">您無法在格式子類別的建構函式相依性插入。</span><span class="sxs-lookup"><span data-stu-id="893ab-130">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="893ab-131">例如，您無法取得記錄器所加入的記錄器參數的建構函式。</span><span class="sxs-lookup"><span data-stu-id="893ab-131">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="893ab-132">若要存取服務，您必須使用取得傳遞至方法的內容物件。</span><span class="sxs-lookup"><span data-stu-id="893ab-132">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="893ab-133">程式碼範例[下方](#read-write)示範如何執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="893ab-133">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="893ab-134">覆寫 CanReadType/CanWriteType</span><span class="sxs-lookup"><span data-stu-id="893ab-134">Override CanReadType/CanWriteType</span></span> 

<span data-ttu-id="893ab-135">指定的類型，您可以還原序列化，或藉由覆寫序列化從`CanReadType`或`CanWriteType`方法。</span><span class="sxs-lookup"><span data-stu-id="893ab-135">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="893ab-136">例如，您可能只能夠建立從 vCard 文字`Contact`型別，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="893ab-136">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="893ab-137">CanWriteResult 方法</span><span class="sxs-lookup"><span data-stu-id="893ab-137">The CanWriteResult method</span></span>

<span data-ttu-id="893ab-138">在某些情況下，您必須覆寫`CanWriteResult`而不是`CanWriteType`。</span><span class="sxs-lookup"><span data-stu-id="893ab-138">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="893ab-139">使用`CanWriteResult`如果下列條件成立：</span><span class="sxs-lookup"><span data-stu-id="893ab-139">Use `CanWriteResult` if the following conditions are true:</span></span>

  * <span data-ttu-id="893ab-140">動作方法傳回的模型類別。</span><span class="sxs-lookup"><span data-stu-id="893ab-140">Your action method returns a model class.</span></span>
  * <span data-ttu-id="893ab-141">有可能會在執行階段傳回的衍生的類別。</span><span class="sxs-lookup"><span data-stu-id="893ab-141">There are derived classes which might be returned at runtime.</span></span>
  * <span data-ttu-id="893ab-142">您需要知道在執行階段，衍生類別的動作所傳回。</span><span class="sxs-lookup"><span data-stu-id="893ab-142">You need to know at runtime which derived class was returned by the action.</span></span>  

<span data-ttu-id="893ab-143">例如，假設您的動作方法簽章傳回`Person`類型，但它可能會傳回`Student`或`Instructor`衍生自型別`Person`。</span><span class="sxs-lookup"><span data-stu-id="893ab-143">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="893ab-144">如果您想要您的格式子，只處理`Student`物件，請檢查類型[物件](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object)提供給內容物件中`CanWriteResult`方法。</span><span class="sxs-lookup"><span data-stu-id="893ab-144">If you want your formatter to handle only `Student` objects, check the type of [Object](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="893ab-145">請注意，不需要使用`CanWriteResult`動作方法傳回時`IActionResult`; 在此情況下，`CanWriteType`方法會接收的執行階段類型。</span><span class="sxs-lookup"><span data-stu-id="893ab-145">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="893ab-146">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="893ab-146">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span> 

<span data-ttu-id="893ab-147">進行還原序列化，或序列化中的實際工作`ReadRequestBodyAsync`或`WriteResponseBodyAsync`。</span><span class="sxs-lookup"><span data-stu-id="893ab-147">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span>  <span data-ttu-id="893ab-148">反白顯示的行，在下列範例示範如何取得服務從相依性插入容器 （您無法從取得建構函式參數）。</span><span class="sxs-lookup"><span data-stu-id="893ab-148">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="893ab-149">如何設定要使用自訂的格式器的 MVC</span><span class="sxs-lookup"><span data-stu-id="893ab-149">How to configure MVC to use a custom formatter</span></span>
 
<span data-ttu-id="893ab-150">若要使用自訂格式子，新增到格式器類別的執行個體`InputFormatters`或`OutputFormatters`集合。</span><span class="sxs-lookup"><span data-stu-id="893ab-150">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="893ab-151">格式器會將其插入的順序進行評估。</span><span class="sxs-lookup"><span data-stu-id="893ab-151">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="893ab-152">第一個會優先使用。</span><span class="sxs-lookup"><span data-stu-id="893ab-152">The first one takes precedence.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="893ab-153">後續步驟</span><span class="sxs-lookup"><span data-stu-id="893ab-153">Next steps</span></span>

<span data-ttu-id="893ab-154">請參閱[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample)，它會實作簡單的 vCard 輸入和輸出格式器。</span><span class="sxs-lookup"><span data-stu-id="893ab-154">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span>  <span data-ttu-id="893ab-155">應用程式讀取並寫入 vCards 看起來像下列的範例：</span><span class="sxs-lookup"><span data-stu-id="893ab-155">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="893ab-156">若要查看 vCard 輸出、 執行應用程式和傳送 Get 要求的接受標頭 」 文字/vcard" `http://localhost:63313/api/contacts/` （執行時從 Visual Studio） 或`http://localhost:5000/api/contacts/`（當從命令列執行）。</span><span class="sxs-lookup"><span data-stu-id="893ab-156">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="893ab-157">若要加入 vCard 連絡人的記憶體中集合，Post 要求傳送至相同的 URL，Content-type 標頭 」 文字/vcard"與 vCard 在主體中，如上述範例格式化的文字。</span><span class="sxs-lookup"><span data-stu-id="893ab-157">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
