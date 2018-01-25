---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: "JSON 和 ASP.NET Web API 中的 XML 序列化 |Microsoft 文件"
author: MikeWasson
description: 
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
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON 和 ASP.NET Web API 中的 XML 序列化
====================
由[Mike Wasson](https://github.com/MikeWasson)

本文說明 ASP.NET Web API 中的 JSON 和 XML 格式器。

在 ASP.NET Web API 中，*媒體類型格式器*是一個物件，可以：

- 從 HTTP 讀取 CLR 物件訊息主體
- 在 HTTP 訊息內容中寫入 CLR 物件

Web 應用程式開發介面提供 JSON 和 XML 的媒體類型格式器。 架構會依預設，將這些格式器插入至管線。 用戶端可以在 HTTP 要求的 Accept 標頭中要求 JSON 或 XML。

## <a name="contents"></a>內容

- [JSON 媒體類型格式器](#json_media_type_formatter)

    - [唯讀屬性](#json_readonly)
    - [日期](#json_dates)
    - [縮排](#json_indenting)
    - [Camel 命名法的大小寫](#json_camelcasing)
    - [匿名和弱型別物件](#json_anon)
- [XML 媒體類型格式器](#xml_media_type_formatter)

    - [唯讀屬性](#xml_readonly)
    - [日期](#xml_dates)
    - [縮排](#xml_indenting)
    - [設定每個類型的 XML 序列化程式](#xml_pertype)
- [移除 JSON 或 XML 格式器](#removing_the_json_or_xml_formatter)
- [處理循環參考](#handling_circular_object_references)
- [測試物件序列化](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>JSON 媒體類型格式器

JSON 格式設定由提供**JsonMediaTypeFormatter**類別。 根據預設， **JsonMediaTypeFormatter**使用[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)執行序列化的程式庫。 Json.NET 是第三方開放原始碼專案。

如果您想要的話，您可以設定**JsonMediaTypeFormatter**類別，以使用**DataContractJsonSerializer**而不是 Json.NET。 若要這樣做，請設定**UseDataContractJsonSerializer**屬性**true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>JSON 序列化

本節說明一些特定的行為，使用預設值的 JSON 格式子的[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)序列化程式。 這不是用來 Json.NET library; 的完整文件如需詳細資訊，請參閱[Json.NET 文件](http://james.newtonking.com/projects/json/help/)。

#### <a name="what-gets-serialized"></a>取得序列化項目的？

根據預設，所有公用屬性和欄位會包含在序列化的 JSON。 若要省略的屬性或欄位，裝飾它與**JsonIgnore**屬性。

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

如果您偏好&quot;選擇加入&quot;方法、 裝飾與類別**DataContract**屬性。 如果這個屬性，則會忽略成員，除非他們擁有**DataMember**。 您也可以使用**DataMember**序列化私用成員。

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>唯讀屬性

根據預設，會序列化唯讀屬性。

<a id="json_dates"></a>
### <a name="dates"></a>日期

根據預設，Json.NET 寫入日期[ISO 8601](http://www.w3.org/TR/NOTE-datetime)格式。 採用 UTC （國際標準時間） 的日期是以"Z"後置字元。 日期，以本地時間包含時區位移。 例如: 

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

根據預設，Json.NET 會保留時區。 您可以藉由設定 DateTimeZoneHandling 屬性來覆寫此設定：

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

如果您想要使用[Microsoft JSON 日期格式](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb)(`"\/Date(ticks)\/"`) 而不是 ISO 8601 設定**DateFormatHandling**上序列化程式設定屬性：

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>縮排

若要撰寫縮排的 JSON，設定**格式**設**Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Camel 命名法的大小寫

若要撰寫 JSON 屬性名稱與 camel 命名法的大小寫，而不需要變更您的資料模型，設定**CamelCasePropertyNamesContractResolver**上序列化程式：

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>匿名和弱型別物件

動作方法可以傳回匿名物件，並將其序列化為 JSON。 例如: 

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

回應訊息內文將會包含下列 JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

如果您的 web 應用程式開發介面接收鬆散結構化從用戶端的 JSON 物件，您可以還原序列化的要求內文**Newtonsoft.Json.Linq.JObject**型別。

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

不過，它通常最好是使用強型別的資料物件。 然後，您不必自行，剖析資料，並取得模型驗證的優點。

XML 序列化程式不支援匿名型別或**JObject**執行個體。 如果您使用這些功能的 JSON 資料，您應該移除 XML 格式器管線，如本文稍後所述。

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>XML 媒體類型格式器

XML 格式由提供**XmlMediaTypeFormatter**類別。 根據預設， **XmlMediaTypeFormatter**使用**DataContractSerializer**執行序列化的類別。

如果您想要的話，您可以設定**XmlMediaTypeFormatter**使用**XmlSerializer**而不是**DataContractSerializer**。 若要這樣做，請設定**/usexmlserializer**屬性**true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

**XmlSerializer**類別支援的類型與窄集**DataContractSerializer**，但讓更充分掌控所產生的 XML。 請考慮使用**XmlSerializer**如果您需要以符合現有的 XML 結構描述。

### <a name="xml-serialization"></a>XML 序列化

本節說明一些特定的行為，使用預設的 XML 格式子的**DataContractSerializer**。

根據預設，DataContractSerializer 的行為，如下所示：

- 所有公用讀取/寫入屬性和欄位都會序列化。 若要省略的屬性或欄位，裝飾它與**IgnoreDataMember**屬性。
- Private 和 protected 成員不會序列化。
- 唯讀屬性不會序列化。 （不過，會序列化的內容是唯讀的集合屬性）。
- 類別和成員名稱是以 XML 撰寫完全依照其出現在類別宣告。
- 會使用預設 XML 命名空間。

如果您需要進一步控制要序列化，您可以裝飾類別**DataContract**屬性。 當這個屬性存在時，此類別序列化，如下所示：

- &quot;參加&quot;方法： 屬性和欄位不會序列化預設。 若要序列化的屬性或欄位，以加以裝飾**DataMember**屬性。
- 若要序列化的私用或受保護成員，它與裝飾**DataMember**屬性。
- 唯讀屬性不會序列化。
- 若要變更的類別名稱在 XML 中的顯示方式，設定*名稱*中的參數**DataContract**屬性。
- 若要變更成員名稱在 XML 中的顯示方式，設定*名稱*中的參數**DataMember**屬性。
- 若要變更的 XML 命名空間，設定*命名空間*中的參數**DataContract**類別。

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>唯讀屬性

唯讀屬性不會序列化。 如果唯讀屬性都有支援私用欄位，您可以將標記與私用欄位**DataMember**屬性。 這個方法會要求**DataContract**類別上的屬性。

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>日期

日期會採用 ISO 8601 格式。 例如， &quot;2012年-05-23T20:21:37.9116538Z&quot;。

<a id="xml_indenting"></a>
### <a name="indenting"></a>縮排

若要撰寫縮排的 XML，設定**縮排**屬性**true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>設定每個類型的 XML 序列化程式

您可以設定不同的 CLR 型別不同的 XML 序列化程式。 例如，您可能需要的特定資料物件**XmlSerializer**回溯相容性。 您可以使用**XmlSerializer**這個物件，並繼續使用**DataContractSerializer**適用於其他類型。

若要設定特定類型的 XML 序列化程式，請呼叫**SetSerializer**。

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

您可以指定**XmlSerializer**或衍生自的任何物件**XmlObjectSerializer**。

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>移除 JSON 或 XML 格式器

您可以移除 JSON 格式器或 XML 格式器清單中的格式器，如果您不想要使用它們。 若要這樣做的主要原因是：

- 若要限制在特定的媒體類型的 web API 回應。 例如，您可能決定支援只有 JSON 回應，並移除 XML 格式器。
- 表示與自訂格式子取代預設的格式器。 例如，您無法將 JSON 格式器取代您自己的自訂實作的 JSON 格式器。

下列程式碼會示範如何移除預設格式器。 呼叫這個從您**應用程式\_啟動**Global.asax 中所定義的方法。

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>處理循環參考

根據預設，JSON 和 XML 格式器會寫入所有的物件做為值。 如果兩個屬性都參考相同的物件，或相同的物件會出現兩次集合中，格式器會將物件序列化兩次。 如果這是對特定問題物件圖形包含循環，因為序列化程式將會擲回例外狀況，會在偵測到迴圈圖形中。

請考慮下列物件模型和控制站。

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

叫用這個動作會導致要格式器擲回例外狀況，它會轉譯為狀態碼 500 （內部伺服器錯誤） 回應至用戶端。

若要保留物件參考，在 JSON 中的，加入下列程式碼加入**應用程式\_啟動**Global.asax 檔中的方法：

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

現在控制器動作將會傳回 JSON 看起來像這樣：

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

請注意，序列化程式將&quot;$id&quot;這兩個物件的屬性。 此外，它會偵測，Employee.Department 屬性就會建立迴圈，以便將值取代的物件參考: {&quot;$ref&quot;:&quot;1&quot;}。

> [!NOTE]
> 無法在 JSON 中標準物件參考。 之前使用這項功能，請考慮您的用戶端是否為無法剖析結果。 它可能只是為了從圖形移除循環比較好。 例如，回到部門員工的連結不真的需要在此範例中。


若要保留在 XML 中的物件參考，您有兩個選項。 更簡單的選項是加入`[DataContract(IsReference=true)]`模型類別。 *IsReference*參數可讓物件參考。 請記住， **DataContract**選擇加入的可序列化，因此您也必須新增**DataMember**屬性的屬性：

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

現在這個格式器將會產生 XML 類似下列：

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

如果您想要避免上模型類別的屬性，還有另一個選項： 建立新的型別而異**DataContractSerializer**執行個體，並設定*preserveObjectReferences*至**，則為 true**建構函式中。 XML 媒體類型格式器上，然後為每個型別序列化程式設定這個執行個體。 下列程式碼顯示如何執行這項操作：

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>測試物件序列化

設計您的 web 應用程式開發介面時，會很有用來測試將如何序列化資料物件。 您可以不需要建立控制站，或叫用控制器動作。

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
