---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON 和 ASP.NET Web API 中的 XML 序列化 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: b1fcaf70cc38d73da0a454764520197b97f34b26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28038097"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="31478-102">JSON 和 ASP.NET Web API 中的 XML 序列化</span><span class="sxs-lookup"><span data-stu-id="31478-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="31478-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="31478-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="31478-104">本文說明 ASP.NET Web API 中的 JSON 和 XML 格式器。</span><span class="sxs-lookup"><span data-stu-id="31478-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="31478-105">在 ASP.NET Web API 中，*媒體類型格式器*是一個物件，可以：</span><span class="sxs-lookup"><span data-stu-id="31478-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="31478-106">從 HTTP 讀取 CLR 物件訊息主體</span><span class="sxs-lookup"><span data-stu-id="31478-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="31478-107">在 HTTP 訊息內容中寫入 CLR 物件</span><span class="sxs-lookup"><span data-stu-id="31478-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="31478-108">Web 應用程式開發介面提供 JSON 和 XML 的媒體類型格式器。</span><span class="sxs-lookup"><span data-stu-id="31478-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="31478-109">架構會依預設，將這些格式器插入至管線。</span><span class="sxs-lookup"><span data-stu-id="31478-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="31478-110">用戶端可以在 HTTP 要求的 Accept 標頭中要求 JSON 或 XML。</span><span class="sxs-lookup"><span data-stu-id="31478-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="31478-111">內容</span><span class="sxs-lookup"><span data-stu-id="31478-111">Contents</span></span>

- [<span data-ttu-id="31478-112">JSON 媒體類型格式器</span><span class="sxs-lookup"><span data-stu-id="31478-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="31478-113">唯讀屬性</span><span class="sxs-lookup"><span data-stu-id="31478-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="31478-114">日期</span><span class="sxs-lookup"><span data-stu-id="31478-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="31478-115">縮排</span><span class="sxs-lookup"><span data-stu-id="31478-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="31478-116">Camel 命名法的大小寫</span><span class="sxs-lookup"><span data-stu-id="31478-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="31478-117">匿名和弱型別物件</span><span class="sxs-lookup"><span data-stu-id="31478-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="31478-118">XML 媒體類型格式器</span><span class="sxs-lookup"><span data-stu-id="31478-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="31478-119">唯讀屬性</span><span class="sxs-lookup"><span data-stu-id="31478-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="31478-120">日期</span><span class="sxs-lookup"><span data-stu-id="31478-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="31478-121">縮排</span><span class="sxs-lookup"><span data-stu-id="31478-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="31478-122">設定每個類型的 XML 序列化程式</span><span class="sxs-lookup"><span data-stu-id="31478-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="31478-123">移除 JSON 或 XML 格式器</span><span class="sxs-lookup"><span data-stu-id="31478-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="31478-124">處理循環參考</span><span class="sxs-lookup"><span data-stu-id="31478-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="31478-125">測試物件序列化</span><span class="sxs-lookup"><span data-stu-id="31478-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="31478-126">JSON 媒體類型格式器</span><span class="sxs-lookup"><span data-stu-id="31478-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="31478-127">JSON 格式設定由提供**JsonMediaTypeFormatter**類別。</span><span class="sxs-lookup"><span data-stu-id="31478-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="31478-128">根據預設， **JsonMediaTypeFormatter**使用[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)執行序列化的程式庫。</span><span class="sxs-lookup"><span data-stu-id="31478-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="31478-129">Json.NET 是第三方開放原始碼專案。</span><span class="sxs-lookup"><span data-stu-id="31478-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="31478-130">如果您想要的話，您可以設定**JsonMediaTypeFormatter**類別，以使用**DataContractJsonSerializer**而不是 Json.NET。</span><span class="sxs-lookup"><span data-stu-id="31478-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="31478-131">若要這樣做，請設定**UseDataContractJsonSerializer**屬性**true**:</span><span class="sxs-lookup"><span data-stu-id="31478-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="31478-132">JSON 序列化</span><span class="sxs-lookup"><span data-stu-id="31478-132">JSON Serialization</span></span>

<span data-ttu-id="31478-133">本節說明一些特定的行為，使用預設值的 JSON 格式子的[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)序列化程式。</span><span class="sxs-lookup"><span data-stu-id="31478-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="31478-134">這不是用來 Json.NET library; 的完整文件如需詳細資訊，請參閱[Json.NET 文件](http://james.newtonking.com/projects/json/help/)。</span><span class="sxs-lookup"><span data-stu-id="31478-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="31478-135">取得序列化項目的？</span><span class="sxs-lookup"><span data-stu-id="31478-135">What Gets Serialized?</span></span>

<span data-ttu-id="31478-136">根據預設，所有公用屬性和欄位會包含在序列化的 JSON。</span><span class="sxs-lookup"><span data-stu-id="31478-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="31478-137">若要省略的屬性或欄位，裝飾它與**JsonIgnore**屬性。</span><span class="sxs-lookup"><span data-stu-id="31478-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="31478-138">如果您偏好&quot;選擇加入&quot;方法、 裝飾與類別**DataContract**屬性。</span><span class="sxs-lookup"><span data-stu-id="31478-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="31478-139">如果這個屬性，則會忽略成員，除非他們擁有**DataMember**。</span><span class="sxs-lookup"><span data-stu-id="31478-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="31478-140">您也可以使用**DataMember**序列化私用成員。</span><span class="sxs-lookup"><span data-stu-id="31478-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="31478-141">唯讀屬性</span><span class="sxs-lookup"><span data-stu-id="31478-141">Read-Only Properties</span></span>

<span data-ttu-id="31478-142">根據預設，會序列化唯讀屬性。</span><span class="sxs-lookup"><span data-stu-id="31478-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="31478-143">日期</span><span class="sxs-lookup"><span data-stu-id="31478-143">Dates</span></span>

<span data-ttu-id="31478-144">根據預設，Json.NET 寫入日期[ISO 8601](http://www.w3.org/TR/NOTE-datetime)格式。</span><span class="sxs-lookup"><span data-stu-id="31478-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="31478-145">採用 UTC （國際標準時間） 的日期是以"Z"後置字元。</span><span class="sxs-lookup"><span data-stu-id="31478-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="31478-146">日期，以本地時間包含時區位移。</span><span class="sxs-lookup"><span data-stu-id="31478-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="31478-147">例如: </span><span class="sxs-lookup"><span data-stu-id="31478-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="31478-148">根據預設，Json.NET 會保留時區。</span><span class="sxs-lookup"><span data-stu-id="31478-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="31478-149">您可以藉由設定 DateTimeZoneHandling 屬性來覆寫此設定：</span><span class="sxs-lookup"><span data-stu-id="31478-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="31478-150">如果您想要使用[Microsoft JSON 日期格式](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb)(`"\/Date(ticks)\/"`) 而不是 ISO 8601 設定**DateFormatHandling**上序列化程式設定屬性：</span><span class="sxs-lookup"><span data-stu-id="31478-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="31478-151">縮排</span><span class="sxs-lookup"><span data-stu-id="31478-151">Indenting</span></span>

<span data-ttu-id="31478-152">若要撰寫縮排的 JSON，設定**格式**設**Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="31478-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="31478-153">Camel 命名法的大小寫</span><span class="sxs-lookup"><span data-stu-id="31478-153">Camel Casing</span></span>

<span data-ttu-id="31478-154">若要撰寫 JSON 屬性名稱與 camel 命名法的大小寫，而不需要變更您的資料模型，設定**CamelCasePropertyNamesContractResolver**上序列化程式：</span><span class="sxs-lookup"><span data-stu-id="31478-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="31478-155">匿名和弱型別物件</span><span class="sxs-lookup"><span data-stu-id="31478-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="31478-156">動作方法可以傳回匿名物件，並將其序列化為 JSON。</span><span class="sxs-lookup"><span data-stu-id="31478-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="31478-157">例如: </span><span class="sxs-lookup"><span data-stu-id="31478-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="31478-158">回應訊息內文將會包含下列 JSON:</span><span class="sxs-lookup"><span data-stu-id="31478-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="31478-159">如果您的 web 應用程式開發介面接收鬆散結構化從用戶端的 JSON 物件，您可以還原序列化的要求內文**Newtonsoft.Json.Linq.JObject**型別。</span><span class="sxs-lookup"><span data-stu-id="31478-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="31478-160">不過，它通常最好是使用強型別的資料物件。</span><span class="sxs-lookup"><span data-stu-id="31478-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="31478-161">然後，您不必自行，剖析資料，並取得模型驗證的優點。</span><span class="sxs-lookup"><span data-stu-id="31478-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="31478-162">XML 序列化程式不支援匿名型別或**JObject**執行個體。</span><span class="sxs-lookup"><span data-stu-id="31478-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="31478-163">如果您使用這些功能的 JSON 資料，您應該移除 XML 格式器管線，如本文稍後所述。</span><span class="sxs-lookup"><span data-stu-id="31478-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="31478-164">XML 媒體類型格式器</span><span class="sxs-lookup"><span data-stu-id="31478-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="31478-165">XML 格式由提供**XmlMediaTypeFormatter**類別。</span><span class="sxs-lookup"><span data-stu-id="31478-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="31478-166">根據預設， **XmlMediaTypeFormatter**使用**DataContractSerializer**執行序列化的類別。</span><span class="sxs-lookup"><span data-stu-id="31478-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="31478-167">如果您想要的話，您可以設定**XmlMediaTypeFormatter**使用**XmlSerializer**而不是**DataContractSerializer**。</span><span class="sxs-lookup"><span data-stu-id="31478-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="31478-168">若要這樣做，請設定 **/usexmlserializer**屬性**true**:</span><span class="sxs-lookup"><span data-stu-id="31478-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="31478-169">**XmlSerializer**類別支援的類型與窄集**DataContractSerializer**，但讓更充分掌控所產生的 XML。</span><span class="sxs-lookup"><span data-stu-id="31478-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="31478-170">請考慮使用**XmlSerializer**如果您需要以符合現有的 XML 結構描述。</span><span class="sxs-lookup"><span data-stu-id="31478-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="31478-171">XML 序列化</span><span class="sxs-lookup"><span data-stu-id="31478-171">XML Serialization</span></span>

<span data-ttu-id="31478-172">本節說明一些特定的行為，使用預設的 XML 格式子的**DataContractSerializer**。</span><span class="sxs-lookup"><span data-stu-id="31478-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="31478-173">根據預設，DataContractSerializer 的行為，如下所示：</span><span class="sxs-lookup"><span data-stu-id="31478-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="31478-174">所有公用讀取/寫入屬性和欄位都會序列化。</span><span class="sxs-lookup"><span data-stu-id="31478-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="31478-175">若要省略的屬性或欄位，裝飾它與**IgnoreDataMember**屬性。</span><span class="sxs-lookup"><span data-stu-id="31478-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="31478-176">Private 和 protected 成員不會序列化。</span><span class="sxs-lookup"><span data-stu-id="31478-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="31478-177">唯讀屬性不會序列化。</span><span class="sxs-lookup"><span data-stu-id="31478-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="31478-178">（不過，會序列化的內容是唯讀的集合屬性）。</span><span class="sxs-lookup"><span data-stu-id="31478-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="31478-179">類別和成員名稱是以 XML 撰寫完全依照其出現在類別宣告。</span><span class="sxs-lookup"><span data-stu-id="31478-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="31478-180">會使用預設 XML 命名空間。</span><span class="sxs-lookup"><span data-stu-id="31478-180">A default XML namespace is used.</span></span>

<span data-ttu-id="31478-181">如果您需要進一步控制要序列化，您可以裝飾類別**DataContract**屬性。</span><span class="sxs-lookup"><span data-stu-id="31478-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="31478-182">當這個屬性存在時，此類別序列化，如下所示：</span><span class="sxs-lookup"><span data-stu-id="31478-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="31478-183">&quot;參加&quot;方法： 屬性和欄位不會序列化預設。</span><span class="sxs-lookup"><span data-stu-id="31478-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="31478-184">若要序列化的屬性或欄位，以加以裝飾**DataMember**屬性。</span><span class="sxs-lookup"><span data-stu-id="31478-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="31478-185">若要序列化的私用或受保護成員，它與裝飾**DataMember**屬性。</span><span class="sxs-lookup"><span data-stu-id="31478-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="31478-186">唯讀屬性不會序列化。</span><span class="sxs-lookup"><span data-stu-id="31478-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="31478-187">若要變更的類別名稱在 XML 中的顯示方式，設定*名稱*中的參數**DataContract**屬性。</span><span class="sxs-lookup"><span data-stu-id="31478-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="31478-188">若要變更成員名稱在 XML 中的顯示方式，設定*名稱*中的參數**DataMember**屬性。</span><span class="sxs-lookup"><span data-stu-id="31478-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="31478-189">若要變更的 XML 命名空間，設定*命名空間*中的參數**DataContract**類別。</span><span class="sxs-lookup"><span data-stu-id="31478-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="31478-190">唯讀屬性</span><span class="sxs-lookup"><span data-stu-id="31478-190">Read-Only Properties</span></span>

<span data-ttu-id="31478-191">唯讀屬性不會序列化。</span><span class="sxs-lookup"><span data-stu-id="31478-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="31478-192">如果唯讀屬性都有支援私用欄位，您可以將標記與私用欄位**DataMember**屬性。</span><span class="sxs-lookup"><span data-stu-id="31478-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="31478-193">這個方法會要求**DataContract**類別上的屬性。</span><span class="sxs-lookup"><span data-stu-id="31478-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="31478-194">日期</span><span class="sxs-lookup"><span data-stu-id="31478-194">Dates</span></span>

<span data-ttu-id="31478-195">日期會採用 ISO 8601 格式。</span><span class="sxs-lookup"><span data-stu-id="31478-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="31478-196">例如， &quot;2012年-05-23T20:21:37.9116538Z&quot;。</span><span class="sxs-lookup"><span data-stu-id="31478-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="31478-197">縮排</span><span class="sxs-lookup"><span data-stu-id="31478-197">Indenting</span></span>

<span data-ttu-id="31478-198">若要撰寫縮排的 XML，設定**縮排**屬性**true**:</span><span class="sxs-lookup"><span data-stu-id="31478-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="31478-199">設定每個類型的 XML 序列化程式</span><span class="sxs-lookup"><span data-stu-id="31478-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="31478-200">您可以設定不同的 CLR 型別不同的 XML 序列化程式。</span><span class="sxs-lookup"><span data-stu-id="31478-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="31478-201">例如，您可能需要的特定資料物件**XmlSerializer**回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="31478-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="31478-202">您可以使用**XmlSerializer**這個物件，並繼續使用**DataContractSerializer**適用於其他類型。</span><span class="sxs-lookup"><span data-stu-id="31478-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="31478-203">若要設定特定類型的 XML 序列化程式，請呼叫**SetSerializer**。</span><span class="sxs-lookup"><span data-stu-id="31478-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="31478-204">您可以指定**XmlSerializer**或衍生自的任何物件**XmlObjectSerializer**。</span><span class="sxs-lookup"><span data-stu-id="31478-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="31478-205">移除 JSON 或 XML 格式器</span><span class="sxs-lookup"><span data-stu-id="31478-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="31478-206">您可以移除 JSON 格式器或 XML 格式器清單中的格式器，如果您不想要使用它們。</span><span class="sxs-lookup"><span data-stu-id="31478-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="31478-207">若要這樣做的主要原因是：</span><span class="sxs-lookup"><span data-stu-id="31478-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="31478-208">若要限制在特定的媒體類型的 web API 回應。</span><span class="sxs-lookup"><span data-stu-id="31478-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="31478-209">例如，您可能決定支援只有 JSON 回應，並移除 XML 格式器。</span><span class="sxs-lookup"><span data-stu-id="31478-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="31478-210">表示與自訂格式子取代預設的格式器。</span><span class="sxs-lookup"><span data-stu-id="31478-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="31478-211">例如，您無法將 JSON 格式器取代您自己的自訂實作的 JSON 格式器。</span><span class="sxs-lookup"><span data-stu-id="31478-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="31478-212">下列程式碼會示範如何移除預設格式器。</span><span class="sxs-lookup"><span data-stu-id="31478-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="31478-213">呼叫這個從您**應用程式\_啟動**Global.asax 中所定義的方法。</span><span class="sxs-lookup"><span data-stu-id="31478-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="31478-214">處理循環參考</span><span class="sxs-lookup"><span data-stu-id="31478-214">Handling Circular Object References</span></span>

<span data-ttu-id="31478-215">根據預設，JSON 和 XML 格式器會寫入所有的物件做為值。</span><span class="sxs-lookup"><span data-stu-id="31478-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="31478-216">如果兩個屬性都參考相同的物件，或相同的物件會出現兩次集合中，格式器會將物件序列化兩次。</span><span class="sxs-lookup"><span data-stu-id="31478-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="31478-217">如果這是對特定問題物件圖形包含循環，因為序列化程式將會擲回例外狀況，會在偵測到迴圈圖形中。</span><span class="sxs-lookup"><span data-stu-id="31478-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="31478-218">請考慮下列物件模型和控制站。</span><span class="sxs-lookup"><span data-stu-id="31478-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="31478-219">叫用這個動作會導致要格式器擲回例外狀況，它會轉譯為狀態碼 500 （內部伺服器錯誤） 回應至用戶端。</span><span class="sxs-lookup"><span data-stu-id="31478-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="31478-220">若要保留物件參考，在 JSON 中的，加入下列程式碼加入**應用程式\_啟動**Global.asax 檔中的方法：</span><span class="sxs-lookup"><span data-stu-id="31478-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="31478-221">現在控制器動作將會傳回 JSON 看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="31478-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="31478-222">請注意，序列化程式將&quot;$id&quot;這兩個物件的屬性。</span><span class="sxs-lookup"><span data-stu-id="31478-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="31478-223">此外，它會偵測，Employee.Department 屬性就會建立迴圈，以便將值取代的物件參考: {&quot;$ref&quot;:&quot;1&quot;}。</span><span class="sxs-lookup"><span data-stu-id="31478-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="31478-224">無法在 JSON 中標準物件參考。</span><span class="sxs-lookup"><span data-stu-id="31478-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="31478-225">之前使用這項功能，請考慮您的用戶端是否為無法剖析結果。</span><span class="sxs-lookup"><span data-stu-id="31478-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="31478-226">它可能只是為了從圖形移除循環比較好。</span><span class="sxs-lookup"><span data-stu-id="31478-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="31478-227">例如，回到部門員工的連結不真的需要在此範例中。</span><span class="sxs-lookup"><span data-stu-id="31478-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="31478-228">若要保留在 XML 中的物件參考，您有兩個選項。</span><span class="sxs-lookup"><span data-stu-id="31478-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="31478-229">更簡單的選項是加入`[DataContract(IsReference=true)]`模型類別。</span><span class="sxs-lookup"><span data-stu-id="31478-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="31478-230">*IsReference*參數可讓物件參考。</span><span class="sxs-lookup"><span data-stu-id="31478-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="31478-231">請記住， **DataContract**選擇加入的可序列化，因此您也必須新增**DataMember**屬性的屬性：</span><span class="sxs-lookup"><span data-stu-id="31478-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="31478-232">現在這個格式器將會產生 XML 類似下列：</span><span class="sxs-lookup"><span data-stu-id="31478-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="31478-233">如果您想要避免上模型類別的屬性，還有另一個選項： 建立新的型別而異**DataContractSerializer**執行個體，並設定*preserveObjectReferences*至 **，則為 true**建構函式中。</span><span class="sxs-lookup"><span data-stu-id="31478-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="31478-234">XML 媒體類型格式器上，然後為每個型別序列化程式設定這個執行個體。</span><span class="sxs-lookup"><span data-stu-id="31478-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="31478-235">下列程式碼顯示如何執行這項操作：</span><span class="sxs-lookup"><span data-stu-id="31478-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="31478-236">測試物件序列化</span><span class="sxs-lookup"><span data-stu-id="31478-236">Testing Object Serialization</span></span>

<span data-ttu-id="31478-237">設計您的 web 應用程式開發介面時，會很有用來測試將如何序列化資料物件。</span><span class="sxs-lookup"><span data-stu-id="31478-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="31478-238">您可以不需要建立控制站，或叫用控制器動作。</span><span class="sxs-lookup"><span data-stu-id="31478-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
