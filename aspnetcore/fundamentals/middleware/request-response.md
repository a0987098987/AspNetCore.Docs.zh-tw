---
title: ASP.NET Core 中的要求和回應作業
author: jkotalik
description: 了解如何在 ASP.NET Core 中讀取要求本文及寫入回應本文。
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 08/29/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: b473fa02e1d23f02bc5d2e15fa54ab7b1dbbb17c
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667213"
---
# <a name="request-and-response-operations-in-aspnet-core"></a>ASP.NET Core 中的要求和回應作業

作者：[Justin Kotalik](https://github.com/jkotalik)

此文章說明如何讀取要求本文及寫入回應本文。 編寫中間件時可能需要這些操作的代碼。 除了編寫中間件之外,通常不需要自定義代碼,因為操作由 MVC 和 Razor Pages 處理。

要求與回應正文有兩個抽象:<xref:System.IO.Stream><xref:System.IO.Pipelines.Pipe>與 。 對於要求讀取，[HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) 是 <xref:System.IO.Stream>，而 `HttpRequest.BodyReader` 是 <xref:System.IO.Pipelines.PipeReader>。 對於回應編寫[,HTTPResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body)`HttpResponse.BodyWriter`是<xref:System.IO.Pipelines.PipeWriter><xref:System.IO.Stream>, 是 。

建議透過串流[進行導管](/dotnet/standard/io/pipelines)。 資料流可以更容易地用於一些簡單的作業，但管線具有效能優勢，並且更容易在大部分情況下使用。 ASP.NET核心開始使用管道而不是內部流。 範例包括：

* `FormReader`
* `TextReader`
* `TextWriter`
* `HttpResponse.WriteAsync`

不會從框架中刪除流。 串流繼續在整個 .NET 中使用,許多流類型沒有管道等效`FileStreams`項`ResponseCompression`,如與 。

## <a name="stream-examples"></a>資料流範例

假設目標是創建一個中間件,將整個請求正文讀取為字串清單,在新行上拆分。 簡單的資料流實作看起來可能像下列範例這樣：

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

此程式碼確實有用，但有一些問題：

* 附加到 `StringBuilder` 之前，此範例會建立另一個會立即棄置的字串 (`encodedString`)。 系統會對資料流中的所有位元組執行此處理序，因此整個要求本文會額外耗用記憶體。
* 此範例會在於新行上進行分割之前讀取整個字串。 在位元組中檢查新行更高效。

下面是一個修復上述問題的範例:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

前面的範例:

* 不會針對 `StringBuilder` 中的整個要求本文進行緩衝處理，除非沒有任何新行字元。
* 不會在字串上呼叫 `Split`。

但是仍有幾個問題：

* 如果換行字元是稀疏的,則大部分請求正文將緩衝在字串中。
* 代碼繼續建立字串 (`remainingString`, 並將它們添加到字串緩衝區, 這將導致額外的分配。

這些問題是可以解決的,但代碼正變得越來越複雜,幾乎沒有改進。 管線提供解決這些問題的方法，且具有最低的程式碼複雜度。

## <a name="pipelines"></a>管線

下列範例示範如何使用 `PipeReader` 處理相同的狀況：

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

此範例可修正資料流實作的許多問題：

* 不需要字串緩衝區,`PipeReader`因為句柄位元組尚未使用。
* 編碼的字串會直接新增至所傳回字串的清單。
* 除了字串 (除了 `ToArray()` 呼叫之外) 所使用的記憶體，系統不會配置資源給字串建立作業。

## <a name="adapters"></a>配接器

屬性`Body``BodyReader/BodyWriter`都可用於`HttpRequest`與`HttpResponse`。 當您設置為`Body`其他流時,一組新的適配器會自動將每種類型調整到另一種。 如果設置為`HttpRequest.Body`新流`HttpRequest.BodyReader`, 則會自動設置為換`PipeReader``HttpRequest.Body`行的新流。

## <a name="startasync"></a>StartAsync

`HttpResponse.StartAsync`用於指示標頭不可修改並運行`OnStarting`回調。 使用 Kestrel 作為伺服器`StartAsync`時,`PipeReader``GetMemory`在使用傳回 的記憶體的保證之前調用屬於 Kestrel 的內部<xref:System.IO.Pipelines.Pipe>緩衝區,而不是外部緩衝區。

## <a name="additional-resources"></a>其他資源

* [System.IO.Pipelines 簡介](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/) \(英文\)
* <xref:fundamentals/middleware/write>
