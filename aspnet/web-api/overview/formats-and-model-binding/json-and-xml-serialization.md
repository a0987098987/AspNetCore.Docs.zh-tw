---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON 和 ASP.NET Web API 中的 XML 序列化 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: e7fbcd41d64651255763c7629f0232788dcb3d30
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400917"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="b54fb-102">JSON 和 ASP.NET Web API 中的 XML 序列化</span><span class="sxs-lookup"><span data-stu-id="b54fb-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="b54fb-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b54fb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b54fb-104">這篇文章說明 ASP.NET Web API 中的 JSON 和 XML 格式器。</span><span class="sxs-lookup"><span data-stu-id="b54fb-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="b54fb-105">在 ASP.NET Web API 中， *media-type 格式器*是一個物件，可以：</span><span class="sxs-lookup"><span data-stu-id="b54fb-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="b54fb-106">讀取 CLR 物件，從 HTTP 訊息內文</span><span class="sxs-lookup"><span data-stu-id="b54fb-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="b54fb-107">將 CLR 物件寫入至 HTTP 訊息本文</span><span class="sxs-lookup"><span data-stu-id="b54fb-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="b54fb-108">Web API 提供以 JSON 和 XML 的媒體類型格式器。</span><span class="sxs-lookup"><span data-stu-id="b54fb-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="b54fb-109">架構會依預設，將這些格式器插入至管線。</span><span class="sxs-lookup"><span data-stu-id="b54fb-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="b54fb-110">用戶端可以在 HTTP 要求的 Accept 標頭中要求 JSON 或 XML。</span><span class="sxs-lookup"><span data-stu-id="b54fb-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="b54fb-111">內容</span><span class="sxs-lookup"><span data-stu-id="b54fb-111">Contents</span></span>

- [<span data-ttu-id="b54fb-112">JSON Media-type 格式器</span><span class="sxs-lookup"><span data-stu-id="b54fb-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="b54fb-113">唯讀屬性</span><span class="sxs-lookup"><span data-stu-id="b54fb-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="b54fb-114">日期</span><span class="sxs-lookup"><span data-stu-id="b54fb-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="b54fb-115">縮排</span><span class="sxs-lookup"><span data-stu-id="b54fb-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="b54fb-116">駝峰式命名法大小寫</span><span class="sxs-lookup"><span data-stu-id="b54fb-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="b54fb-117">匿名和弱型別物件</span><span class="sxs-lookup"><span data-stu-id="b54fb-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="b54fb-118">XML 媒體類型格式器</span><span class="sxs-lookup"><span data-stu-id="b54fb-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="b54fb-119">唯讀屬性</span><span class="sxs-lookup"><span data-stu-id="b54fb-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="b54fb-120">日期</span><span class="sxs-lookup"><span data-stu-id="b54fb-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="b54fb-121">縮排</span><span class="sxs-lookup"><span data-stu-id="b54fb-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="b54fb-122">設定每個類型的 XML 序列化程式</span><span class="sxs-lookup"><span data-stu-id="b54fb-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="b54fb-123">移除 JSON 或 XML 格式器</span><span class="sxs-lookup"><span data-stu-id="b54fb-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="b54fb-124">處理循環物件參考</span><span class="sxs-lookup"><span data-stu-id="b54fb-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="b54fb-125">測試物件序列化</span><span class="sxs-lookup"><span data-stu-id="b54fb-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="b54fb-126">JSON Media-type 格式器</span><span class="sxs-lookup"><span data-stu-id="b54fb-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="b54fb-127">JSON 格式提供**JsonMediaTypeFormatter**類別。</span><span class="sxs-lookup"><span data-stu-id="b54fb-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="b54fb-128">根據預設， **JsonMediaTypeFormatter**會使用[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)執行序列化的程式庫。</span><span class="sxs-lookup"><span data-stu-id="b54fb-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="b54fb-129">Json.NET 是第三方開放原始碼專案。</span><span class="sxs-lookup"><span data-stu-id="b54fb-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="b54fb-130">如果您想，您可以設定**JsonMediaTypeFormatter**類別，以使用**DataContractJsonSerializer**而不是 Json.NET。</span><span class="sxs-lookup"><span data-stu-id="b54fb-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="b54fb-131">若要這樣做，請設定**UseDataContractJsonSerializer**屬性設 **，則為 true**:</span><span class="sxs-lookup"><span data-stu-id="b54fb-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="b54fb-132">JSON 序列化</span><span class="sxs-lookup"><span data-stu-id="b54fb-132">JSON Serialization</span></span>

<span data-ttu-id="b54fb-133">本章節描述的 JSON 格式器，使用預設的一些特定行為[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)序列化程式。</span><span class="sxs-lookup"><span data-stu-id="b54fb-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="b54fb-134">這不是用來完整的文件的 Json.NET 程式庫;如需詳細資訊，請參閱 < [Json.NET 文件](http://james.newtonking.com/projects/json/help/)。</span><span class="sxs-lookup"><span data-stu-id="b54fb-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="b54fb-135">序列化什麼？</span><span class="sxs-lookup"><span data-stu-id="b54fb-135">What Gets Serialized?</span></span>

<span data-ttu-id="b54fb-136">根據預設，所有公用屬性和欄位會包含在序列化的 JSON。</span><span class="sxs-lookup"><span data-stu-id="b54fb-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="b54fb-137">若要省略屬性或欄位，裝飾它與**JsonIgnore**屬性。</span><span class="sxs-lookup"><span data-stu-id="b54fb-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="b54fb-138">如果您偏好&quot;參加&quot;方法，來裝飾類別**DataContract**屬性。</span><span class="sxs-lookup"><span data-stu-id="b54fb-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="b54fb-139">如果此屬性，則會忽略成員，除非**DataMember**。</span><span class="sxs-lookup"><span data-stu-id="b54fb-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="b54fb-140">您也可以使用**DataMember**序列化私用成員。</span><span class="sxs-lookup"><span data-stu-id="b54fb-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="b54fb-141">唯讀屬性</span><span class="sxs-lookup"><span data-stu-id="b54fb-141">Read-Only Properties</span></span>

<span data-ttu-id="b54fb-142">根據預設，會序列化唯讀屬性。</span><span class="sxs-lookup"><span data-stu-id="b54fb-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="b54fb-143">日期</span><span class="sxs-lookup"><span data-stu-id="b54fb-143">Dates</span></span>

<span data-ttu-id="b54fb-144">根據預設，Json.NET 將寫入日期[ISO 8601](http://www.w3.org/TR/NOTE-datetime)格式。</span><span class="sxs-lookup"><span data-stu-id="b54fb-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="b54fb-145">以 UTC （國際標準時間） 的日期是以"Z"後置詞。</span><span class="sxs-lookup"><span data-stu-id="b54fb-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="b54fb-146">當地時間日期包含時區時差。</span><span class="sxs-lookup"><span data-stu-id="b54fb-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="b54fb-147">例如: </span><span class="sxs-lookup"><span data-stu-id="b54fb-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="b54fb-148">根據預設，Json.NET 會保留時區。</span><span class="sxs-lookup"><span data-stu-id="b54fb-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="b54fb-149">您可以藉由設定 DateTimeZoneHandling 屬性來覆寫此設定：</span><span class="sxs-lookup"><span data-stu-id="b54fb-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="b54fb-150">如果您想要使用[Microsoft JSON 日期格式](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb)(`"\/Date(ticks)\/"`) 設定而不是 ISO 8601 **DateFormatHandling**的序列化程式設定的屬性：</span><span class="sxs-lookup"><span data-stu-id="b54fb-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="b54fb-151">縮排</span><span class="sxs-lookup"><span data-stu-id="b54fb-151">Indenting</span></span>

<span data-ttu-id="b54fb-152">若要撰寫縮排的 JSON，設定**格式化**設為**Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="b54fb-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="b54fb-153">駝峰式命名法大小寫</span><span class="sxs-lookup"><span data-stu-id="b54fb-153">Camel Casing</span></span>

<span data-ttu-id="b54fb-154">若要寫入 JSON 屬性名稱使用駝峰式命名法大小寫，而不需要變更您的資料模型，設定**CamelCasePropertyNamesContractResolver**上序列化程式：</span><span class="sxs-lookup"><span data-stu-id="b54fb-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="b54fb-155">匿名和弱型別物件</span><span class="sxs-lookup"><span data-stu-id="b54fb-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="b54fb-156">動作方法可以傳回的匿名物件，並將其序列化為 JSON。</span><span class="sxs-lookup"><span data-stu-id="b54fb-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="b54fb-157">例如: </span><span class="sxs-lookup"><span data-stu-id="b54fb-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="b54fb-158">回應訊息內文會包含下列 JSON:</span><span class="sxs-lookup"><span data-stu-id="b54fb-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="b54fb-159">如果您的 web API 收到的鬆散結構化 JSON 物件，從用戶端，您可以還原序列化的要求內文**Newtonsoft.Json.Linq.JObject**型別。</span><span class="sxs-lookup"><span data-stu-id="b54fb-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="b54fb-160">不過，最好是通常使用強型別的的資料物件。</span><span class="sxs-lookup"><span data-stu-id="b54fb-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="b54fb-161">然後，您不需要剖析資料，並取得模型驗證的優點。</span><span class="sxs-lookup"><span data-stu-id="b54fb-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="b54fb-162">XML 序列化程式不支援匿名型別或**JObject**執行個體。</span><span class="sxs-lookup"><span data-stu-id="b54fb-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="b54fb-163">如果您使用這些功能，為您的 JSON 資料，您應該移除 XML 格式器管線，在本文稍後所述。</span><span class="sxs-lookup"><span data-stu-id="b54fb-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="b54fb-164">XML 媒體類型格式器</span><span class="sxs-lookup"><span data-stu-id="b54fb-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="b54fb-165">XML 格式由提供**XmlMediaTypeFormatter**類別。</span><span class="sxs-lookup"><span data-stu-id="b54fb-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="b54fb-166">根據預設， **XmlMediaTypeFormatter**會使用**DataContractSerializer**執行序列化的類別。</span><span class="sxs-lookup"><span data-stu-id="b54fb-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="b54fb-167">如果您想，您可以設定**XmlMediaTypeFormatter**使用**XmlSerializer**而非**DataContractSerializer**。</span><span class="sxs-lookup"><span data-stu-id="b54fb-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="b54fb-168">若要這樣做，請設定 **/usexmlserializer**屬性設 **，則為 true**:</span><span class="sxs-lookup"><span data-stu-id="b54fb-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="b54fb-169">**XmlSerializer**類別支援一組範圍較小的類型，比**DataContractSerializer**，但提供更充分掌控所產生的 XML。</span><span class="sxs-lookup"><span data-stu-id="b54fb-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="b54fb-170">請考慮使用**XmlSerializer**如果您需要以符合現有的 XML 結構描述。</span><span class="sxs-lookup"><span data-stu-id="b54fb-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="b54fb-171">XML 序列化</span><span class="sxs-lookup"><span data-stu-id="b54fb-171">XML Serialization</span></span>

<span data-ttu-id="b54fb-172">本章節描述的 XML 格式器，使用預設的一些特定行為**DataContractSerializer**。</span><span class="sxs-lookup"><span data-stu-id="b54fb-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="b54fb-173">根據預設，DataContractSerializer 的行為，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b54fb-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="b54fb-174">會序列化所有公用讀取/寫入屬性和欄位。</span><span class="sxs-lookup"><span data-stu-id="b54fb-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="b54fb-175">若要省略屬性或欄位，裝飾它與**IgnoreDataMember**屬性。</span><span class="sxs-lookup"><span data-stu-id="b54fb-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="b54fb-176">Private 和 protected 成員不會序列化。</span><span class="sxs-lookup"><span data-stu-id="b54fb-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="b54fb-177">唯讀屬性不會序列化。</span><span class="sxs-lookup"><span data-stu-id="b54fb-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="b54fb-178">（不過，會序列化唯讀集合屬性的內容）。</span><span class="sxs-lookup"><span data-stu-id="b54fb-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="b54fb-179">類別和成員名稱是以 XML 撰寫的完全依照它們出現在類別宣告。</span><span class="sxs-lookup"><span data-stu-id="b54fb-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="b54fb-180">會使用預設 XML 命名空間。</span><span class="sxs-lookup"><span data-stu-id="b54fb-180">A default XML namespace is used.</span></span>

<span data-ttu-id="b54fb-181">如果您需要更充分掌控序列化時，您可以裝飾具有類別**DataContract**屬性。</span><span class="sxs-lookup"><span data-stu-id="b54fb-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="b54fb-182">當這個屬性時，類別序列化，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b54fb-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="b54fb-183">&quot;參加&quot;方法： 屬性與欄位沒有預設序列化。</span><span class="sxs-lookup"><span data-stu-id="b54fb-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="b54fb-184">若要序列化的屬性或欄位，裝飾它與**DataMember**屬性。</span><span class="sxs-lookup"><span data-stu-id="b54fb-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="b54fb-185">若要序列化的私用或受保護成員，裝飾它與**DataMember**屬性。</span><span class="sxs-lookup"><span data-stu-id="b54fb-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="b54fb-186">唯讀屬性不會序列化。</span><span class="sxs-lookup"><span data-stu-id="b54fb-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="b54fb-187">若要變更的類別名稱在 XML 中的顯示方式，設定*名稱*中的參數**DataContract**屬性。</span><span class="sxs-lookup"><span data-stu-id="b54fb-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="b54fb-188">若要變更的成員名稱在 XML 中的顯示方式，設定*名稱*中的參數**DataMember**屬性。</span><span class="sxs-lookup"><span data-stu-id="b54fb-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="b54fb-189">若要變更的 XML 命名空間，設定*命名空間*中的參數**DataContract**類別。</span><span class="sxs-lookup"><span data-stu-id="b54fb-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="b54fb-190">唯讀屬性</span><span class="sxs-lookup"><span data-stu-id="b54fb-190">Read-Only Properties</span></span>

<span data-ttu-id="b54fb-191">唯讀屬性不會序列化。</span><span class="sxs-lookup"><span data-stu-id="b54fb-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="b54fb-192">如果唯讀屬性的支援私用欄位，您可以將標記與私用欄位**DataMember**屬性。</span><span class="sxs-lookup"><span data-stu-id="b54fb-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="b54fb-193">這個方法會要求**DataContract**類別上的屬性。</span><span class="sxs-lookup"><span data-stu-id="b54fb-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="b54fb-194">日期</span><span class="sxs-lookup"><span data-stu-id="b54fb-194">Dates</span></span>

<span data-ttu-id="b54fb-195">日期是以 ISO 8601 格式。</span><span class="sxs-lookup"><span data-stu-id="b54fb-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="b54fb-196">例如， &quot;2012年-05-23T20:21:37.9116538Z&quot;。</span><span class="sxs-lookup"><span data-stu-id="b54fb-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="b54fb-197">縮排</span><span class="sxs-lookup"><span data-stu-id="b54fb-197">Indenting</span></span>

<span data-ttu-id="b54fb-198">若要撰寫時縮排的 XML，將**縮排**屬性設 **，則為 true**:</span><span class="sxs-lookup"><span data-stu-id="b54fb-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="b54fb-199">設定每個類型的 XML 序列化程式</span><span class="sxs-lookup"><span data-stu-id="b54fb-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="b54fb-200">您可以設定不同的 CLR 類型不同的 XML 序列化程式。</span><span class="sxs-lookup"><span data-stu-id="b54fb-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="b54fb-201">例如，您可能需要的特定資料物件**XmlSerializer**回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="b54fb-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="b54fb-202">您可以使用**XmlSerializer**這個物件，並繼續使用**DataContractSerializer**其他類型。</span><span class="sxs-lookup"><span data-stu-id="b54fb-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="b54fb-203">若要設定特定類型的 XML 序列化程式，請呼叫**SetSerializer**。</span><span class="sxs-lookup"><span data-stu-id="b54fb-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="b54fb-204">您可以指定**XmlSerializer**或任何衍生自的物件**XmlObjectSerializer**。</span><span class="sxs-lookup"><span data-stu-id="b54fb-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="b54fb-205">移除 JSON 或 XML 格式器</span><span class="sxs-lookup"><span data-stu-id="b54fb-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="b54fb-206">您可以移除 JSON 格式器或 XML 格式器清單中的格式器，如果您不想要使用它們。</span><span class="sxs-lookup"><span data-stu-id="b54fb-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="b54fb-207">若要這樣做的主要原因是：</span><span class="sxs-lookup"><span data-stu-id="b54fb-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="b54fb-208">若要限制您的 web API 回應特定的媒體類型。</span><span class="sxs-lookup"><span data-stu-id="b54fb-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="b54fb-209">例如，您可能會決定支援只有 JSON 回應，並移除 XML 格式器。</span><span class="sxs-lookup"><span data-stu-id="b54fb-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="b54fb-210">若要使用自訂格式器取代預設的格式器。</span><span class="sxs-lookup"><span data-stu-id="b54fb-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="b54fb-211">例如，您可以將 JSON 格式器取代您自己的 JSON 格式器的自訂實作。</span><span class="sxs-lookup"><span data-stu-id="b54fb-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="b54fb-212">下列程式碼示範如何移除預設格式器。</span><span class="sxs-lookup"><span data-stu-id="b54fb-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="b54fb-213">呼叫這個屬性從您**應用程式\_啟動**Global.asax 中所定義的方法。</span><span class="sxs-lookup"><span data-stu-id="b54fb-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="b54fb-214">處理循環物件參考</span><span class="sxs-lookup"><span data-stu-id="b54fb-214">Handling Circular Object References</span></span>

<span data-ttu-id="b54fb-215">根據預設，JSON 和 XML 格式器會寫入做為值的所有物件。</span><span class="sxs-lookup"><span data-stu-id="b54fb-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="b54fb-216">如果兩個屬性都指向相同物件，或如果相同的物件會出現兩次在集合中，格式器將會序列化物件兩次。</span><span class="sxs-lookup"><span data-stu-id="b54fb-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="b54fb-217">這是特定的問題如果您的物件圖形包含循環，因為序列化程式將會擲回例外狀況，在圖形中偵測到迴圈。</span><span class="sxs-lookup"><span data-stu-id="b54fb-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="b54fb-218">請考慮下列物件模型和控制器。</span><span class="sxs-lookup"><span data-stu-id="b54fb-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="b54fb-219">叫用此動作會導致格式器擲回例外狀況，進而轉譯成狀態碼 500 （內部伺服器錯誤） 回應至用戶端。</span><span class="sxs-lookup"><span data-stu-id="b54fb-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="b54fb-220">若要保留 JSON 中的物件參考，新增下列程式碼**應用程式\_啟動**Global.asax 檔案中的方法：</span><span class="sxs-lookup"><span data-stu-id="b54fb-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="b54fb-221">現在控制器動作會傳回 JSON 看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="b54fb-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="b54fb-222">請注意，序列化程式加入&quot;$id&quot;這兩個物件的屬性。</span><span class="sxs-lookup"><span data-stu-id="b54fb-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="b54fb-223">此外，它會偵測 Employee.Department 屬性建立迴圈，因此會將值取代的物件參考: {&quot;$ref&quot;:&quot;1&quot;}。</span><span class="sxs-lookup"><span data-stu-id="b54fb-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="b54fb-224">物件參考不是標準 json 格式。</span><span class="sxs-lookup"><span data-stu-id="b54fb-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="b54fb-225">才能使用此功能，請考慮您的用戶端是否都能夠剖析結果。</span><span class="sxs-lookup"><span data-stu-id="b54fb-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="b54fb-226">最好是單純是為了從圖形移除循環。</span><span class="sxs-lookup"><span data-stu-id="b54fb-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="b54fb-227">比方說，回到部門員工的連結是並不真的需要在此範例中。</span><span class="sxs-lookup"><span data-stu-id="b54fb-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="b54fb-228">若要保留在 XML 中的物件參考，您會有兩個選項。</span><span class="sxs-lookup"><span data-stu-id="b54fb-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="b54fb-229">更簡單的做法是新增`[DataContract(IsReference=true)]`到您的模型類別。</span><span class="sxs-lookup"><span data-stu-id="b54fb-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="b54fb-230">*IsReference*參數可讓物件參考。</span><span class="sxs-lookup"><span data-stu-id="b54fb-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="b54fb-231">請記住**DataContract**選用功能，可序列化，因此您也必須新增**DataMember**屬性的屬性：</span><span class="sxs-lookup"><span data-stu-id="b54fb-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="b54fb-232">現在，格式器將會產生類似的 XML 如下：</span><span class="sxs-lookup"><span data-stu-id="b54fb-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="b54fb-233">如果您想要避免在您的模型類別上的屬性，還有另一個選項： 建立新的型別特有**DataContractSerializer**執行個體，並設定*preserveObjectReferences*到 **，則為 true**建構函式。</span><span class="sxs-lookup"><span data-stu-id="b54fb-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="b54fb-234">在 XML 媒體類型格式器，然後為每個型別序列化程式設定這個執行個體。</span><span class="sxs-lookup"><span data-stu-id="b54fb-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="b54fb-235">下列程式碼示範如何執行這項操作：</span><span class="sxs-lookup"><span data-stu-id="b54fb-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="b54fb-236">測試物件序列化</span><span class="sxs-lookup"><span data-stu-id="b54fb-236">Testing Object Serialization</span></span>

<span data-ttu-id="b54fb-237">當您設計您的 web API 時，最好要測試您的資料物件序列化的方式。</span><span class="sxs-lookup"><span data-stu-id="b54fb-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="b54fb-238">您可以不需要建立一個控制站，或叫用控制器動作來這樣做。</span><span class="sxs-lookup"><span data-stu-id="b54fb-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
