---
title: ASP.NET Core 中的要求和回應作業
author: jkotalik
description: 了解如何在 ASP.NET Core 中讀取要求本文及寫入回應本文。
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 5/29/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/middleware/request-response
ms.openlocfilehash: b6fc7a115cb0f4696d10bf036eadb59028dfb605
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85404128"
---
# <a name="request-and-response-operations-in-aspnet-core"></a>ASP.NET Core 中的要求和回應作業

作者：[Justin Kotalik](https://github.com/jkotalik)

此文章說明如何讀取要求本文及寫入回應本文。 撰寫中介軟體時，可能需要這些作業的程式碼。 在撰寫中介軟體之外，自訂程式碼通常不是必要的，因為作業是由 MVC 和 Razor 頁面處理。

要求和回應主體有兩個抽象概念： <xref:System.IO.Stream> 和 <xref:System.IO.Pipelines.Pipe> 。 針對要求讀取， <xref:Microsoft.AspNetCore.Http.HttpRequest.Body?displayProperty=nameWithType> 是 <xref:System.IO.Stream> ，而 `HttpRequest.BodyReader` 是 <xref:System.IO.Pipelines.PipeReader> 。 針對回應寫入， <xref:Microsoft.AspNetCore.Http.HttpResponse.Body?displayProperty=nameWithType> 是 <xref:System.IO.Stream> ，而 `HttpResponse.BodyWriter` 是 <xref:System.IO.Pipelines.PipeWriter> 。

建議在資料流程上使用[管線](/dotnet/standard/io/pipelines)。 資料流可以更容易地用於一些簡單的作業，但管線具有效能優勢，並且更容易在大部分情況下使用。 ASP.NET Core 開始使用管線，而不是內部的資料流程。 範例包括：

* `FormReader`
* `TextReader`
* `TextWriter`
* `HttpResponse.WriteAsync`

未從架構中移除資料流程。 資料流程會繼續在整個 .NET 中使用，而且許多資料流程類型沒有對等的管道，例如 `FileStreams` 和 `ResponseCompression` 。

## <a name="stream-examples"></a>資料流範例

假設目標是要建立一個中介軟體，將整個要求本文當做字串清單來讀取，並在新行上分割。 簡單的資料流實作看起來可能像下列範例這樣：

> [!WARNING]
> 下列程式碼：
> * 用來示範不使用管線讀取要求本文的問題。
> * 不適合用于生產應用程式。

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

此程式碼確實有用，但有一些問題：

* 附加到 `StringBuilder` 之前，此範例會建立另一個會立即棄置的字串 (`encodedString`)。 系統會對資料流中的所有位元組執行此處理序，因此整個要求本文會額外耗用記憶體。
* 此範例會在於新行上進行分割之前讀取整個字串。 檢查位元組陣列中的新行會更有效率。

以下是修正上述幾個問題的範例：

> [!WARNING]
> 下列程式碼：
> * 是用來示範上述程式碼中某些問題的解決方案，同時不會解決所有問題。
> * 不適合用于生產應用程式。

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

前述範例：

* 不會針對 `StringBuilder` 中的整個要求本文進行緩衝處理，除非沒有任何新行字元。
* 不會在字串上呼叫 `Split`。

但是仍有幾個問題：

* 如果換行字元是稀疏的，則會在字串中緩衝處理大部分的要求主體。
* 程式碼會繼續建立字串（ `remainingString` ），並將它們新增至字串緩衝區，這會導致額外的配置。

這些問題都是可修復的，但程式碼變得越來越複雜，而且沒有任何改善。 管線提供解決這些問題的方法，且具有最低的程式碼複雜度。

## <a name="pipelines"></a>管線

下列範例顯示如何使用[PipeReader](/dotnet/standard/io/pipelines#pipe)來處理相同的案例：

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

此範例可修正資料流實作的許多問題：

* 不需要字串緩衝區，因為 `PipeReader` 會處理尚未使用的位元組。
* 編碼的字串會直接新增至所傳回字串的清單。
* 除了呼叫之外， `ToArray` 以及字串所使用的記憶體，字串建立是免費的配置。

## <a name="adapters"></a>配接器

`Body`、和 `BodyReader` `BodyWriter` 屬性適用于 `HttpRequest` 和 `HttpResponse` 。 當您將設定 `Body` 為不同的資料流程時，一組新的介面卡會自動將每個類型調整成另一種。 如果您將設定 `HttpRequest.Body` 為新的資料流程， `HttpRequest.BodyReader` 會自動設為 `PipeReader` 包裝的新 `HttpRequest.Body` 。

## <a name="startasync"></a>StartAsync

`HttpResponse.StartAsync`用來表示標頭是不可修改的，也可以執行 `OnStarting` 回呼。 使用 Kestrel 做為伺服器時，在 `StartAsync` 使用之前呼叫 `PipeReader` 會保證所傳回的記憶體 `GetMemory` 屬於 Kestrel 的內部， <xref:System.IO.Pipelines.Pipe> 而不是外部緩衝區。

## <a name="additional-resources"></a>其他資源

* [.NET 中的 system.object](/dotnet/standard/io/pipelines)
* <xref:fundamentals/middleware/write>

<!-- Test with Postman or other tool. See image in static directory. -->
