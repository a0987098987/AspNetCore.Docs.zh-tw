---
title: ASP.NET Core 中的要求和回應作業
author: jkotalik
description: 了解如何在 ASP.NET Core 中讀取要求本文及寫入回應本文。
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: c9f6509738ef6290666a58268fbb0584913db9d6
ms.sourcegitcommit: 357a7120632b20465801c093e4e5bd4a315496a8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/08/2019
ms.locfileid: "67649234"
---
# <a name="request-and-response-operations-in-aspnet-core"></a>ASP.NET Core 中的要求和回應作業

作者：[Justin Kotalik](https://github.com/jkotalik)

此文章說明如何讀取要求本文及寫入回應本文。 當您撰寫中介軟體時，可能需要撰寫這些作業的程式碼。 否則，通常不需要撰寫此程式碼，因為作業會由 MVC 和 Razor Pages 處理。

在 ASP.NET Core 3.0 中，有要求和回應主體的兩個抽象概念：<xref:System.IO.Stream> 和 <xref:System.IO.Pipelines.Pipe>。 對於要求讀取，[HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) 是 <xref:System.IO.Stream>，而 `HttpRequest.BodyPipe` 是 <xref:System.IO.Pipelines.PipeReader>。 對於要求讀取，[HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) 是 `HttpResponse.BodyPipe` 是 <xref:System.IO.Pipelines.PipeWriter>。

我們建議在資料流上使用管線。 資料流可以更容易地用於一些簡單的作業，但管線具有效能優勢，並且更容易在大部分情況下使用。 在 3.0 中，ASP.NET Core 正在啟動，以在內部使用管線，而非使用資料流。 範例包括：

- `FormReader`
- `TextReader`
- `TextWriter`
- `HttpResponse.WriteAsync`

資料流並不會消失。 它們會繼續在整個 .NET 中使用，且許多資料流類型沒有管道對等項目，例如 `FileStreams` 和 `ResponseCompression`。

## <a name="stream-examples"></a>資料流範例

假設您想要建立一個以字串清單 (以新行區隔) 方式，讀取整個要求本文的中介軟體。 簡單的資料流實作看起來可能像下列範例這樣：

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

此程式碼確實有用，但有一些問題：

- 附加到 `StringBuilder` 之前，此範例會建立另一個會立即棄置的字串 (`encodedString`)。 系統會對資料流中的所有位元組執行此處理序，因此整個要求本文會額外耗用記憶體。
- 此範例會在於新行上進行分割之前讀取整個字串。 檢查位元組陣列中的新行會是比較有效率的方式。

以下是這些問題其中一部分的修正範例：

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

此範例：

- 不會針對 `StringBuilder` 中的整個要求本文進行緩衝處理，除非沒有任何新行字元。
- 不會在字串上呼叫 `Split`。

但是仍有幾個問題：

- 如果新行字元是疏鬆的，大部分要求本文都會在字串中進行緩衝處理。
- 它仍然會建立字串 (`remainingString`) 並將它們新增至字串緩衝區，這會額外耗用資源。

這些問題是可以修正的，但程式碼變得越來越複雜，而且改善幅度很有限。 管線提供解決這些問題的方法，且具有最低的程式碼複雜度。

## <a name="pipelines"></a>管線

下列範例示範如何使用 `PipeReader` 處理相同的狀況：

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

此範例可修正資料流實作的許多問題：

- 這並不需要字串緩衝區，因為 `PipeReader` 會處理未使用的位元組。
- 編碼的字串會直接新增至所傳回字串的清單。
- 除了字串 (除了 `ToArray()` 呼叫之外) 所使用的記憶體，系統不會配置資源給字串建立作業。

## <a name="adapters"></a>配接器

現在 `Body` 和 `BodyPipe` 屬性都可供 `HttpRequest` 和 `HttpResponse` 使用，那麼將 `Body` 設至其他資料流時，會發生什麼情況？ 在 3.0 中有一組新的配接器可自動將每種類型與其他類型配接。 例如，如果將 `HttpRequest.Body` 設定至新的資料流，`HttpRequest.BodyPipe` 會自動設為包裝 `HttpRequest.Body` 的新 `PipeReader`。 相同的行為會套用至設定 `BodyPipe` 屬性。 如果將 `HttpResponse.BodyPipe` 設定至新的 `PipeWriter`，`HttpResponse.Body` 會自動設為包裝 `HttpResponse.BodyPipe` 的新資料流。

## <a name="startasync"></a>StartAsync

3\.0 中 `HttpResponse.StartAsync` 是新的。 它可以用來指示標題是不可修改的，以及執行 `OnStarting` 回撥。 在 3.0-preview3 中，您必須先呼叫 `StartAsync` 再使用 `HttpRequest.BodyPipe`，而且在未來版本中，它將會是一個建議。 使用 Kestrel 作為伺服器時，請先呼叫 StartAsync 再使用 `PipeReader`，以保證 `GetMemory` 傳回的記憶體將屬於 Kestrel 的內部 <xref:System.IO.Pipelines.Pipe>，而不是外部緩衝區。

## <a name="additional-resources"></a>其他資源

- [System.IO.Pipelines 簡介](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/) \(英文\)
- <xref:fundamentals/middleware/write>
