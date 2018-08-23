---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: ASP.NET Web API 2.1 中的 BSON 支援 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 709fb0266c0725176358a1bd0d08b3e07fa6e2a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833892"
---
<a name="bson-support-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 中的 BSON 支援
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

Web API 2.1 引進了 BSON 支援。 本主題說明如何使用.NET 用戶端應用程式和 Web API 控制器中 （伺服器端） 中的 BSON。

## <a name="what-is-bson"></a>BSON 是什麼？

[BSON](http://bsonspec.org/)是二進位序列化格式。 "BSON"代表"二進位 JSON"，但 BSON 和 JSON 序列化非常不同。 BSON 是 「 JSON 類似 」，因為物件以類似 JSON 的名稱 / 值組。 不同於 JSON 中，數值資料類型會儲存為位元組，而不是字串

BSON 設計成輕量型、 容易掃描，且速度快，可以編碼/解碼。

- BSON 相當的大小為 JSON。 根據資料，BSON 承載可能小於或大於 JSON 承載。 序列化二進位資料，例如影像檔，因為二進位資料不是 base64 編碼，會小於 JSON、 BSON。
- BSON 文件很容易就能掃描，因為項目會加上 [長度] 欄位中，因此剖析器可以略過項目而不需要加以解碼。
- 編碼和解碼會有效率，因為數值資料類型會儲存成數字，不是字串。

原生用戶端，例如.NET 用戶端應用程式，可以受益於使用 BSON 取代文字為基礎的格式，例如 JSON 或 XML。 瀏覽器用戶端，您可能要堅持使用 JSON，因為 JavaScript 可以直接將轉換的 JSON 承載。

幸運的是，Web API 會使用[內容交涉](content-negotiation.md)，因此您的 API 可以支援這兩種格式，並讓選擇的用戶端。

## <a name="enabling-bson-on-the-server"></a>啟用在伺服器上的 BSON

在 Web API 組態中，新增**BsonMediaTypeFormatter**格式器集合。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

現在如果用戶端要求為"application/bson"，則 Web API 會使用 BSON 格式器。

若要關聯 BSON 與其他媒體類型，將它們加入 SupportedMediaTypes 集合。 下列程式碼會將 「 application/vnd.contoso"新增至支援的媒體類型：

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>範例 HTTP 工作階段

此範例中，我們將使用下列模型類別加上簡單的 Web API 控制器：

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

用戶端可能會傳送下列 HTTP 要求：

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

以下是回應：

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

此處我已被取代的二進位資料， &quot;。&quot;字元。 下列螢幕擷取畫面所示 Fiddler 的未經處理的十六進位值。

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>BSON 使用 HttpClient

.NET 用戶端應用程式可以使用 BSON 格式器，與**HttpClient**。 如需詳細資訊**HttpClient**，請參閱[呼叫 Web API 從.NET 用戶端](../advanced/calling-a-web-api-from-a-net-client.md)。

下列程式碼會傳送 GET 要求，以便接受 BSON，並接著會在回應中的 BSON 承載還原序列化。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

若要從伺服器要求 BSON，設定為"application/bson"Accept 標頭：

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

若要還原序列化回應主體，請使用**BsonMediaTypeFormatter**。 此格式器不是在預設格式器集合中，因此您必須指定它時讀取回應主體：

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

下一個範例示範如何傳送 POST 要求，其中包含 BSON。

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

此程式碼大多是與前一個範例相同。 但在**PostAsync**方法，指定**BsonMediaTypeFormatter**格式器為：

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>序列化的最上層基本型別

每個 BSON 文件是索引鍵/值組清單。BSON 規格並未定義序列化單一的原始值，例如整數或字串的語法。

若要解決此限制，請**BsonMediaTypeFormatter**視為特殊案例中的基本型別。 序列化之前, 它會將值轉換的索引鍵/值組與 「 值 」 索引鍵。 例如，假設您的 API 控制器會傳回一個整數：

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

序列化之前, 的 BSON 格式器會將這下列索引鍵/值組：

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

當您還原序列化時，格式器會將資料轉換回原始的值。 不過，使用不同的 BSON 剖析器用戶端必須處理這種情況，如果您的 web API 會傳回原始值。 一般情況下，您應該考慮傳回結構化的資料，而不是原始的值。

## <a name="additional-resources"></a>其他資源

[Web API BSON 範例](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[媒體格式器](media-formatters.md)
