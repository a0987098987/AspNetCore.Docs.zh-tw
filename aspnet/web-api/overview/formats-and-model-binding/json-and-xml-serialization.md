---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON 和 ASP.NET Web API 中的 XML 序列化 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 05/30/2012
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: c6610eebbc6bd03426771087f0112c1ffa63aa15
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836317"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON 和 ASP.NET Web API 中的 XML 序列化
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

這篇文章說明 ASP.NET Web API 中的 JSON 和 XML 格式器。

在 ASP.NET Web API 中， *media-type 格式器*是一個物件，可以：

- 讀取 CLR 物件，從 HTTP 訊息內文
- 將 CLR 物件寫入至 HTTP 訊息本文

Web API 提供以 JSON 和 XML 的媒體類型格式器。 架構會依預設，將這些格式器插入至管線。 用戶端可以在 HTTP 要求的 Accept 標頭中要求 JSON 或 XML。

## <a name="contents"></a>內容

- [JSON Media-type 格式器](#json_media_type_formatter)

    - [唯讀屬性](#json_readonly)
    - [日期](#json_dates)
    - [縮排](#json_indenting)
    - [駝峰式命名法大小寫](#json_camelcasing)
    - [匿名和弱型別物件](#json_anon)
- [XML 媒體類型格式器](#xml_media_type_formatter)

    - [唯讀屬性](#xml_readonly)
    - [日期](#xml_dates)
    - [縮排](#xml_indenting)
    - [設定每個類型的 XML 序列化程式](#xml_pertype)
- [移除 JSON 或 XML 格式器](#removing_the_json_or_xml_formatter)
- [處理循環物件參考](#handling_circular_object_references)
- [測試物件序列化](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>JSON Media-type 格式器

JSON 格式提供**JsonMediaTypeFormatter**類別。 根據預設， **JsonMediaTypeFormatter**會使用[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)執行序列化的程式庫。 Json.NET 是第三方開放原始碼專案。

如果您想，您可以設定**JsonMediaTypeFormatter**類別，以使用**DataContractJsonSerializer**而不是 Json.NET。 若要這樣做，請設定**UseDataContractJsonSerializer**屬性設 **，則為 true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>JSON 序列化

本章節描述的 JSON 格式器，使用預設的一些特定行為[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)序列化程式。 這不是用來完整的文件的 Json.NET 程式庫;如需詳細資訊，請參閱 < [Json.NET 文件](http://james.newtonking.com/projects/json/help/)。

#### <a name="what-gets-serialized"></a>序列化什麼？

根據預設，所有公用屬性和欄位會包含在序列化的 JSON。 若要省略屬性或欄位，裝飾它與**JsonIgnore**屬性。

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

如果您偏好&quot;參加&quot;方法，來裝飾類別**DataContract**屬性。 如果此屬性，則會忽略成員，除非**DataMember**。 您也可以使用**DataMember**序列化私用成員。

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>唯讀屬性

根據預設，會序列化唯讀屬性。

<a id="json_dates"></a>
### <a name="dates"></a>日期

根據預設，Json.NET 將寫入日期[ISO 8601](http://www.w3.org/TR/NOTE-datetime)格式。 以 UTC （國際標準時間） 的日期是以"Z"後置詞。 當地時間日期包含時區時差。 例如: 

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

根據預設，Json.NET 會保留時區。 您可以藉由設定 DateTimeZoneHandling 屬性來覆寫此設定：

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

如果您想要使用[Microsoft JSON 日期格式](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb)(`"\/Date(ticks)\/"`) 設定而不是 ISO 8601 **DateFormatHandling**的序列化程式設定的屬性：

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>縮排

若要撰寫縮排的 JSON，設定**格式化**設為**Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>駝峰式命名法大小寫

若要寫入 JSON 屬性名稱使用駝峰式命名法大小寫，而不需要變更您的資料模型，設定**CamelCasePropertyNamesContractResolver**上序列化程式：

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>匿名和弱型別物件

動作方法可以傳回的匿名物件，並將其序列化為 JSON。 例如: 

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

回應訊息內文會包含下列 JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

如果您的 web API 收到的鬆散結構化 JSON 物件，從用戶端，您可以還原序列化的要求內文**Newtonsoft.Json.Linq.JObject**型別。

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

不過，最好是通常使用強型別的的資料物件。 然後，您不需要剖析資料，並取得模型驗證的優點。

XML 序列化程式不支援匿名型別或**JObject**執行個體。 如果您使用這些功能，為您的 JSON 資料，您應該移除 XML 格式器管線，在本文稍後所述。

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>XML 媒體類型格式器

XML 格式由提供**XmlMediaTypeFormatter**類別。 根據預設， **XmlMediaTypeFormatter**會使用**DataContractSerializer**執行序列化的類別。

如果您想，您可以設定**XmlMediaTypeFormatter**使用**XmlSerializer**而非**DataContractSerializer**。 若要這樣做，請設定 **/usexmlserializer**屬性設 **，則為 true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

**XmlSerializer**類別支援一組範圍較小的類型，比**DataContractSerializer**，但提供更充分掌控所產生的 XML。 請考慮使用**XmlSerializer**如果您需要以符合現有的 XML 結構描述。

### <a name="xml-serialization"></a>XML 序列化

本章節描述的 XML 格式器，使用預設的一些特定行為**DataContractSerializer**。

根據預設，DataContractSerializer 的行為，如下所示：

- 會序列化所有公用讀取/寫入屬性和欄位。 若要省略屬性或欄位，裝飾它與**IgnoreDataMember**屬性。
- Private 和 protected 成員不會序列化。
- 唯讀屬性不會序列化。 （不過，會序列化唯讀集合屬性的內容）。
- 類別和成員名稱是以 XML 撰寫的完全依照它們出現在類別宣告。
- 會使用預設 XML 命名空間。

如果您需要更充分掌控序列化時，您可以裝飾具有類別**DataContract**屬性。 當這個屬性時，類別序列化，如下所示：

- &quot;參加&quot;方法： 屬性與欄位沒有預設序列化。 若要序列化的屬性或欄位，裝飾它與**DataMember**屬性。
- 若要序列化的私用或受保護成員，裝飾它與**DataMember**屬性。
- 唯讀屬性不會序列化。
- 若要變更的類別名稱在 XML 中的顯示方式，設定*名稱*中的參數**DataContract**屬性。
- 若要變更的成員名稱在 XML 中的顯示方式，設定*名稱*中的參數**DataMember**屬性。
- 若要變更的 XML 命名空間，設定*命名空間*中的參數**DataContract**類別。

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>唯讀屬性

唯讀屬性不會序列化。 如果唯讀屬性的支援私用欄位，您可以將標記與私用欄位**DataMember**屬性。 這個方法會要求**DataContract**類別上的屬性。

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>日期

日期是以 ISO 8601 格式。 例如， &quot;2012年-05-23T20:21:37.9116538Z&quot;。

<a id="xml_indenting"></a>
### <a name="indenting"></a>縮排

若要撰寫時縮排的 XML，將**縮排**屬性設 **，則為 true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>設定每個類型的 XML 序列化程式

您可以設定不同的 CLR 類型不同的 XML 序列化程式。 例如，您可能需要的特定資料物件**XmlSerializer**回溯相容性。 您可以使用**XmlSerializer**這個物件，並繼續使用**DataContractSerializer**其他類型。

若要設定特定類型的 XML 序列化程式，請呼叫**SetSerializer**。

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

您可以指定**XmlSerializer**或任何衍生自的物件**XmlObjectSerializer**。

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>移除 JSON 或 XML 格式器

您可以移除 JSON 格式器或 XML 格式器清單中的格式器，如果您不想要使用它們。 若要這樣做的主要原因是：

- 若要限制您的 web API 回應特定的媒體類型。 例如，您可能會決定支援只有 JSON 回應，並移除 XML 格式器。
- 若要使用自訂格式器取代預設的格式器。 例如，您可以將 JSON 格式器取代您自己的 JSON 格式器的自訂實作。

下列程式碼示範如何移除預設格式器。 呼叫這個屬性從您**應用程式\_啟動**Global.asax 中所定義的方法。

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>處理循環物件參考

根據預設，JSON 和 XML 格式器會寫入做為值的所有物件。 如果兩個屬性都指向相同物件，或如果相同的物件會出現兩次在集合中，格式器將會序列化物件兩次。 這是特定的問題如果您的物件圖形包含循環，因為序列化程式將會擲回例外狀況，在圖形中偵測到迴圈。

請考慮下列物件模型和控制器。

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

叫用此動作會導致格式器擲回例外狀況，進而轉譯成狀態碼 500 （內部伺服器錯誤） 回應至用戶端。

若要保留 JSON 中的物件參考，新增下列程式碼**應用程式\_啟動**Global.asax 檔案中的方法：

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

現在控制器動作會傳回 JSON 看起來像這樣：

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

請注意，序列化程式加入&quot;$id&quot;這兩個物件的屬性。 此外，它會偵測 Employee.Department 屬性建立迴圈，因此會將值取代的物件參考: {&quot;$ref&quot;:&quot;1&quot;}。

> [!NOTE]
> 物件參考不是標準 json 格式。 才能使用此功能，請考慮您的用戶端是否都能夠剖析結果。 最好是單純是為了從圖形移除循環。 比方說，回到部門員工的連結是並不真的需要在此範例中。


若要保留在 XML 中的物件參考，您會有兩個選項。 更簡單的做法是新增`[DataContract(IsReference=true)]`到您的模型類別。 *IsReference*參數可讓物件參考。 請記住**DataContract**選用功能，可序列化，因此您也必須新增**DataMember**屬性的屬性：

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

現在，格式器將會產生類似的 XML 如下：

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

如果您想要避免在您的模型類別上的屬性，還有另一個選項： 建立新的型別特有**DataContractSerializer**執行個體，並設定*preserveObjectReferences*到 **，則為 true**建構函式。 在 XML 媒體類型格式器，然後為每個型別序列化程式設定這個執行個體。 下列程式碼示範如何執行這項操作：

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>測試物件序列化

當您設計您的 web API 時，最好要測試您的資料物件序列化的方式。 您可以不需要建立一個控制站，或叫用控制器動作來這樣做。

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
