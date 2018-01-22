---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: "在 ASP.NET Web API 2.1 BSON 支援 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 53ad705fad6d2225cecca4d73355bd6ebfcf56d5
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2018
---
<a name="bson-support-in-aspnet-web-api-21"></a>在 ASP.NET Web API 2.1 BSON 支援
====================
由[Mike Wasson](https://github.com/MikeWasson)

Web API 2.1 引進了 BSON 的支援。 本主題說明如何使用 BSON 和.NET 用戶端應用程式中 Web API 控制器 （伺服器端）。

## <a name="what-is-bson"></a>什麼是 BSON？

[BSON](http://bsonspec.org/)是二進位序列化格式。 「 BSON"代表"二進位 JSON"，但 BSON 和 JSON 序列化非常不同。 BSON 為"JSON-like"，因為物件以名稱 / 值組，類似於 JSON 形式表示。 不同於 JSON，數值資料類型會儲存為位元組，而不是字串

BSON 被設計成是輕量、 能夠輕鬆瀏覽，以及快速編碼/解碼。

- BSON 相當的大小為 JSON。 根據資料，BSON 內容可能會小於或大於 JSON 裝載。 序列化二進位資料，映像檔案，例如 BSON 小於 JSON，因為二進位資料不是 base64 編碼。
- BSON 文件不容易閱讀，因為項目會加上長度欄位，讓剖析器可以略過項目而不需要加以解碼。
- 編碼和解碼是有效率，因為數值資料類型會儲存成數字，不是字串。

原生用戶端，例如.NET 用戶端應用程式，可使用獲益 BSON 取代文字為基礎的格式，例如 JSON 或 XML。 瀏覽器用戶端，您可能要盡可能使用 JSON，因為 JavaScript 可以直接將轉換的 JSON 裝載。

幸運的是，Web API 會使用[內容交涉](content-negotiation.md)，因此您的 API 可支援兩種格式，並且可讓用戶端選擇。

## <a name="enabling-bson-on-the-server"></a>在伺服器上啟用 BSON

在 Web API 設定中，加入**BsonMediaTypeFormatter**格式器集合。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

現在如果用戶端要求 」 應用程式/bson"，Web API 會使用 BSON 格式器。

若要關聯 BSON 與其他媒體類型，將它們加入 SupportedMediaTypes 集合。 下列程式碼會將"application/vnd.contoso"加入至支援的媒體類型：

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>範例 HTTP 工作階段

此範例中，我們將使用下列模型類別加上簡單的 Web API 控制器：

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

用戶端可能會傳送下列 HTTP 要求：

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

以下是回應：

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

這裡我已被取代的二進位資料， &quot;。&quot;字元。 下列的螢幕擷取畫面所示 Fiddler 未經處理的十六進位值。

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>使用 HttpClient BSON

.NET 用戶端應用程式可以使用 BSON 格式器與**HttpClient**。 如需有關**HttpClient**，請參閱[呼叫 Web API 的.NET 用戶端](../advanced/calling-a-web-api-from-a-net-client.md)。

下列程式碼會傳送 GET 要求接受 BSON，，然後還原序列化回應的 BSON 裝載。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

若要從伺服器要求 BSON，設定 「 應用程式/bson"Accept 標頭：

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

若要還原序列化回應主體，請使用**BsonMediaTypeFormatter**。 此格式器不在預設的格式器集合，所以您不必指定當您讀取回應主體：

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

下一個範例示範如何將傳送 POST 要求，其中包含 BSON。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

大部分的這段程式碼是與前一個範例相同。 但在**PostAsync**方法，指定**BsonMediaTypeFormatter**格式器為：

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>序列化的最上層基本型別

每一個 BSON 文件是索引鍵/值組的清單。BSON 規格並未定義序列化單一的原始值，例如整數或字串的語法。

若要解決這項限制， **BsonMediaTypeFormatter**視為特殊案例中的基本型別。 先序列化，它將值轉換成索引鍵/值組與"Value"的索引鍵。 例如，假設您的 API 控制器會傳回一個整數：

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

先序列化，BSON 格式器將這下列索引鍵/值組：

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

當您還原序列化時，格式器會將資料轉換回原始值。 不過，用戶端使用不同的 BSON 剖析器需要處理此情況下，如果您的 web API 傳回未經處理的值。 一般情況下，您應該考慮傳回結構化的資料，而不是原始值。

## <a name="additional-resources"></a>其他資源

[Web 應用程式開發介面 BSON 範例](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[媒體格式器](media-formatters.md)
