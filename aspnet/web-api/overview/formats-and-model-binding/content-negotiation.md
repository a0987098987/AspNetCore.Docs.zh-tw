---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: "內容交涉，ASP.NET Web API 中的 |Microsoft 文件"
author: MikeWasson
description: "說明 ASP.NET Web API 的 HTTP 內容交涉的實作方式。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="content-negotiation-in-aspnet-web-api"></a>ASP.NET Web API 中的內容交涉
====================
由[Mike Wasson](https://github.com/MikeWasson)

本文說明 ASP.NET Web API 實作內容交涉的方式。

HTTP 規格 (RFC 2616) 定義為 [有多種表示時選取最佳的表示法指定回應的處理。] 內容交涉 在 HTTP 內容交涉的主要機制都是這些要求標頭：

- **接受：**媒體類型的可接受的回應，例如"應用程式/json"，"application/xml"或自訂的媒體類型，例如&quot;application/vnd.example+xml&quot;
- **Accept-charset:**字元集都可接受的例如 utf-8 或 ISO 8859-1。
- **編碼接受：**的內容編碼可接受的例如 gzip。
- **接受語言：**慣用的自然語言，例如 「 en-us-我們"。

伺服器也可以查看其他部分的 HTTP 要求。 例如，如果要求包含 X 要求的標頭，指出 AJAX 要求，伺服器可能會預設為 JSON 如果沒有 Accept 標頭。

在本文中，我們將探討如何 Web API 所使用的接受和 Accept-charset 標頭。 （在此階段中，沒有接受編碼 」 或 「 接受語言的內建支援。）

## <a name="serialization"></a>序列化

如果 Web API 控制器會傳回資源的 CLR 型別，管線將傳回值序列化，並將它寫入至 HTTP 回應主體。

例如，請考慮下列的控制器動作：

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

此 HTTP 要求時，可能會傳送用戶端：

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

在回應時，伺服器可能會將傳送：

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

在此範例中，用戶端要求 JSON、 Javascript 或 「 任何 」 (\*/\*)。 伺服器回應的 JSON 表示法的`Product`物件。 請注意，在回應中的內容類型標頭設定為&quot;應用程式 /json&quot;。

在控制站也可以傳回**HttpResponseMessage**物件。 若要指定，回應主體的 CLR 物件，呼叫**CreateResponse**擴充方法：

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

此選項可讓您更充分掌控回應的詳細資料。 您可以設定狀態碼，將 HTTP 標頭，等等。

將序列化資源的物件稱為*媒體格式器*。 媒體格式器衍生自**MediaTypeFormatter**類別。 Web API for XML 和 JSON，提供媒體格式器，而且您可以建立自訂的格式器以支援其他類型的媒體。 如需撰寫自訂的格式器的詳細資訊，請參閱[媒體格式器](media-formatters.md)。

## <a name="how-content-negotiation-works"></a>如何在內容交涉運作方式

首先，管線取得**IContentNegotiator**服務從**HttpConfiguration**物件。 它也會取得清單中的媒體格式器**HttpConfiguration.Formatters**集合。

接著，會呼叫管線**IContentNegotiatior.Negotiate**傳遞給：

- 要序列化的物件類型
- 媒體格式器集合
- HTTP 要求

**交涉**方法會傳回兩個資訊片段：

- 若要使用的格式器
- 回應的媒體類型

如果找到不到格式器，**交涉**方法會傳回**null**，和用戶端接收 HTTP 錯誤 406 （無法接受）。

下列程式碼會示範如何在控制站可以直接叫用內容交涉：

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

此程式碼相當於管線會自動執行。

## <a name="default-content-negotiator"></a>預設內容交涉器

**DefaultContentNegotiator**類別提供的預設實作**IContentNegotiator**。 它會使用數個條件選取格式器。

首先，必須能夠將類型序列化格式子。 這藉由呼叫驗證**MediaTypeFormatter.CanWriteType**。

接下來，內容交涉器會查看每個格式器，並評估其符合 HTTP 要求的程度。 若要評估的相符項目，內容交涉器會查看兩件事格式器上：

- **SupportedMediaTypes**集合，其中包含支援的媒體類型的清單。 內容交涉器會嘗試比對這份清單，針對要求 Accept 標頭。 請注意 Accept 標頭可以包含範圍。 例如，"text/plain"是文字的相符項目 /\*或\* / \*。
- **MediaTypeMappings**集合，其中包含一份**MediaTypeMapping**物件。 **MediaTypeMapping**類別會提供 HTTP 要求符合媒體類型的一般方式。 例如，它無法在特定的媒體類型對應自訂 HTTP 標頭。

如果有多個比對，具有最高品質因素 wins 相符項目。 例如: 

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

在此範例中，應用程式 /json 都有為 1.0，是隱含的品質的因素，因此它更為優先應用程式/xml。

如果找不到相符的項目，內容交涉器嘗試比對媒體類型的要求主體中，如果有的話。 例如，如果要求包含 JSON 資料，JSON 格式器會尋找內容交涉器。

如果仍然沒有相符的項目，內容交涉器只會挑選第一個可以將類型序列化的格式器。

## <a name="selecting-a-character-encoding"></a>選取的字元編碼

內容交涉器選取格式器後，選擇的最佳字元編碼方式查看**SupportedEncodings**等格式器，比針對要求中的 Accept-charset 標頭 （如果有的話） 的屬性。
