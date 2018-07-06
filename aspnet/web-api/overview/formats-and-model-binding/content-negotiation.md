---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: 內容交涉，ASP.NET Web API 中的 |Microsoft Docs
author: MikeWasson
description: 說明 ASP.NET Web API 實作 HTTP 內容交涉的方式。
ms.author: aspnetcontent
ms.date: 05/20/2012
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: 2314a263a12c74e80c08391ae03425955a82458a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810313"
---
<a name="content-negotiation-in-aspnet-web-api"></a>ASP.NET Web API 中的內容交涉
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

這篇文章說明 ASP.NET Web API 實作內容交涉的方式。

HTTP 規格 (RFC 2616) 定義為 「 程序時有多種表示選取最佳的表示法，針對給定的回應。 」 的內容交涉 在 HTTP 內容交涉的主要機制都是這些要求標頭：

- **接受：** 哪些媒體類型是可接受的回應，例如"application/json"，"application/xml"或自訂的媒體類型，例如&quot;application/vnd.example+xml&quot;
- **Accept-charset:** 字元集是可接受的例如 utf-8 或 ISO 8859-1。
- **編碼接受：** 內容編碼可接受的例如 gzip。
- **接受語言：** 慣用的自然語言，例如 「 en-us-我們"。

伺服器也可以查看 HTTP 要求的其他部分。 例如，如果要求包含 X 要求與標頭，指出 AJAX 要求，伺服器可能會預設為 JSON 如果沒有 Accept 標頭。

在本文中，我們將探討 Web API 使用的接受和 Accept-charset 標頭的方式。 （在此階段中，接受編碼 」 或 「 接受語言沒有內建支援。)

## <a name="serialization"></a>序列化

如果 Web API 控制器會傳回做為 CLR 類型的資源，管線就會序列化傳回的值，並將它寫入至 HTTP 回應主體。

例如，請考慮下列控制器動作：

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

用戶端可能會傳送這個 HTTP 要求：

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

在回應中，可能會傳送伺服器：

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

在此範例中，用戶端要求 JSON、 Javascript 或 「 任何 」 (\*/\*)。 伺服器回應的 JSON 表示法的`Product`物件。 請注意，在回應中的 Content-type 標頭設定為&quot;application/json&quot;。

也可以傳回一個控制站**HttpResponseMessage**物件。 若要指定，回應主體的 CLR 物件，呼叫**CreateResponse**擴充方法：

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

此選項可讓您更充分掌控回應詳細資料。 您可以設定狀態的程式碼中，新增 HTTP 標頭，等等。

將序列化資源物件稱為*媒體格式器*。 媒體格式器衍生自**MediaTypeFormatter**類別。 Web API for XML 和 JSON，提供媒體格式器，您可以建立自訂的格式器，以支援其他類型的媒體。 如需撰寫自訂的格式器的詳細資訊，請參閱 <<c0> [ 媒體格式器](media-formatters.md)。

## <a name="how-content-negotiation-works"></a>如何內容交涉運作方式

首先，管線會取得**IContentNegotiator**服務**HttpConfiguration**物件。 它也會取得中的媒體格式器的清單**HttpConfiguration.Formatters**集合。

接下來，管線會呼叫**IContentNegotiatior.Negotiate**，並傳入：

- 要序列化的物件類型
- 媒體格式器的集合
- HTTP 要求

**交涉**方法會傳回兩項資訊：

- 若要使用的格式器
- 回應的媒體類型

找不到，如果**交涉**方法會傳回**null**，和用戶端接收 HTTP 錯誤 406 （無法接受）。

下列程式碼顯示如何控制站可以直接叫用內容交涉：

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

此程式碼相當於管線會自動執行。

## <a name="default-content-negotiator"></a>預設內容 Negotiator

**DefaultContentNegotiator**類別提供的預設實作**IContentNegotiator**。 它會使用數個條件選取格式器。

首先，必須能夠將類型序列化格式器。 此乃藉由呼叫**MediaTypeFormatter.CanWriteType**。

接下來，器會查看每個格式器，並評估其符合 HTTP 要求的程度。 若要評估相符項目，器會查看兩件事上格式器中：

- **SupportedMediaTypes**集合，其中包含一份支援的媒體類型。 器會嘗試比對這份清單，針對要求 Accept 標頭。 請注意 Accept 標頭可以包含範圍。 例如，"text/plain"是文字的相符項目 /\*或是\* / \*。
- **MediaTypeMappings**集合，其中包含一份**MediaTypeMapping**物件。 **MediaTypeMapping**類別提供的泛型的方法，以符合 HTTP 要求的媒體類型。 比方說，它可以將自訂的 HTTP 標頭對應至特定的媒體類型。

如果有多個比對，最高的品質因素 wins 的相符結果。 例如: 

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

在此範例中，application/json 會有為 1.0，隱含的品質係數，因此最好是透過 application/xml。

如果找不到任何相符項目，器會嘗試比對媒體類型的要求主體中，如果有的話。 例如，如果要求包含 JSON 資料，器會尋找 JSON 格式器。

如果仍不有任何相符項目，則器只會挑選可以將類型序列化的第一個格式器。

## <a name="selecting-a-character-encoding"></a>選取的字元編碼

選取格式器之後，會選擇器的最佳字元編碼來看看**SupportedEncodings**屬性格式器，並比對要求 Accept-charset 標頭 （如果有的話）。
