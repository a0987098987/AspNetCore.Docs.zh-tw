---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: ASP.NET Web API 2 中的媒體格式器 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 824fea5a8837ff8b09af832b7d2a094c7d82907b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368912"
---
<a name="media-formatters-in-aspnet-web-api-2"></a>ASP.NET Web API 2 中的媒體格式器
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

本教學課程會示範如何在 ASP.NET Web API 中支援額外的媒體格式。

## <a name="internet-media-types"></a>網際網路媒體類型

媒體類型，也稱為 MIME 類型，識別某份資料的格式。 在 HTTP、 媒體類型會說明訊息內文的格式。 媒體類型是由兩個字串、 類型和子類型所組成。 例如: 

- text/html
- image/png
- application/json

當 HTTP 訊息包含實體內容時，內容類型標頭會指定訊息主體的格式。 這會告訴接收者如何剖析訊息內文的內容。

比方說，如果 HTTP 回應會包含為 PNG 影像，則回應可能會有下列標頭。

[!code-console[Main](media-formatters/samples/sample1.cmd)]

當用戶端傳送要求訊息時，它可以包含 Accept 標頭。 Accept 標頭會告訴您的伺服器的媒體類型在用戶端想要從伺服器。 例如: 

[!code-console[Main](media-formatters/samples/sample2.cmd)]

此標頭會要求伺服器在用戶端想 HTML、 XHTML 或 XML。

媒體類型會決定 Web API 如何序列化和還原序列化 HTTP 訊息本文。 Web API 有 XML、 JSON、 BSON，和 form-urlencoded 資料的內建支援，您可以支援額外的媒體類型，藉由撰寫*媒體格式器*。

若要建立的媒體格式器，衍生自這些類別的其中一個：

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx)。 這個類別會使用非同步的讀取和寫入方法。
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx)。 這個類別衍生自**MediaTypeFormatter** ，但使用 sychronous 讀取/寫入方法。

衍生自**BufferedMediaTypeFormatter**較為簡單，因為沒有非同步程式碼，但是這也表示呼叫的執行緒可封鎖 i/o。

## <a name="example-creating-a-csv-media-formatter"></a>範例： 建立 CSV 的媒體格式器

下列範例示範可序列化成以逗號分隔值 (CSV) 格式的 Product 物件的媒體類型格式器。 此範例會使用在本教學課程中定義的產品類型[建立 Web API 的支援 CRUD 作業](../older-versions/creating-a-web-api-that-supports-crud-operations.md)。 以下是產品物件的定義：

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

若要實作的 CSV 格式器，請定義衍生自類別**BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

在建構函式，新增的媒體類型格式器支援。 在此範例中，格式器支援單一媒體類型， &quot;text/csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

覆寫**CanWriteType**方法，以表示其類型於格式子可序列化：

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

在此範例中，格式器可以序列化單一`Product`物件的集合以及`Product`物件。

同樣地，覆寫**CanReadType**方法，以表示其類型於格式子能夠還原序列化。 在此範例中，格式器不支援還原序列化，因此方法只會傳回**false**。

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

最後，覆寫**WriteToStream**方法。 這個方法會序列化型別，將它寫入至資料流。 如果您的格式器支援還原序列化，也會覆寫**ReadFromStream**方法。

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>加入 Web API 管線中的媒體格式器

若要新增的媒體類型格式器的 Web API 管線，請使用**格式器**屬性上的**HttpConfiguration**物件。

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>字元編碼

（選擇性） 的媒體格式器可以支援多個字元編碼，例如 utf-8 或 ISO 8859-1。

在建構函式，新增一或多個[System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx)型別**SupportedEncodings**集合。 將使用預設編碼方式的第一個。

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

在**WriteToStream**並**ReadFromStream**方法，呼叫[MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx)到選取的慣用的字元編碼方式。 這個方法會比對要求標頭，針對支援的編碼方式清單。 使用傳回**編碼**當您從讀取或寫入資料流：

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
