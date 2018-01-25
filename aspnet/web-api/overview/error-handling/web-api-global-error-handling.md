---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: "全域錯誤處理中 ASP.NET Web API 2 |Microsoft 文件"
author: davidmatson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: c593c56ba3d0ee8ebf6dc425408d2c3b91c83f93
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>全域錯誤處理中 ASP.NET Web API 2
====================
由[David Matson](https://github.com/davidmatson)， [Rick Anderson](https://github.com/Rick-Anderson)

現今記錄或全域處理錯誤的 Web API 中沒有簡單的方式。 可以透過處理一些未處理的例外狀況[例外狀況篩選條件](exception-handling.md)，但無法處理的例外狀況篩選條件的案例數目。 例如: 

1. 從控制器建構函式擲回的例外狀況。
2. 從訊息處理常式擲回的例外狀況。
3. 在路由期間擲回的例外狀況。
4. 回應內容序列化期間擲回的例外狀況。

我們想要提供簡單且一致的方式，來記錄並處理 （如果可能的話） 這些例外狀況。 

有兩個主要的情況下處理例外狀況，我們就能傳送錯誤回應，並我們可以在其中執行該案例會記錄例外狀況的情況。 後者的情況下一個範例是當中間串流處理回應的內容; 擲回例外狀況在此情況下它是太晚傳送新的回應訊息，因為狀態碼、 標頭和部分內容已經透過網路，因此我們只是中止連接。 即使無法處理此例外狀況，以產生新的回應訊息，我們仍然支援記錄例外狀況。 在我們可以在此偵測到錯誤的情況下，我們可以傳回適當的錯誤回應，如下列所示：

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>現有的選項

除了[例外狀況篩選條件](exception-handling.md)，[訊息處理常式](../advanced/http-message-handlers.md)可以立即用來觀察所有 500 層級回應，但這些回應所引發的動作是很困難，他們缺乏原始錯誤的相關內容。 訊息處理常式也有一些相同的限制相關的情況下，它們能夠處理的例外狀況篩選條件。雖然 Web API 有擷取錯誤狀況的追蹤基礎結構追蹤基礎結構供診斷之用和未設計或適合在生產環境中執行。 全域例外狀況處理和記錄應該可以在實際執行期間執行，並插入到現有的監視解決方案服務 (例如， [ELMAH](https://code.google.com/p/elmah/) )。

### <a name="solution-overview"></a>解決方案概觀

 我們提供兩個新的使用者可取代服務， [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md)和 IExceptionHandler，記錄並處理未處理例外狀況。 服務是非常類似，但有兩個主要差異：

1. 我們支援註冊多個例外狀況記錄器，但只有單一例外狀況處理常式。
2. 例外狀況記錄器一律取得呼叫，即使我們即將中止連接。 只有當我們仍然可以選擇要傳送的回應訊息時，取得呼叫例外狀況處理常式。

這兩項服務提供存取權包含來自例外狀況已偵測到，點的相關資訊的例外狀況內容特別[HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx)、 [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx)，則會擲回例外狀況和例外狀況來源 （下面的詳細資料）。

### <a name="design-principles"></a>設計原則

1. **沒有重大變更**因為在次要版本中，其中一個重要影響方案的條件約束是，不會有任何重大變更，請輸入合約或行為，要加入這項功能。 我們想要根據現有開啟 500 個回應例外狀況的 catch 區塊完成某些清除並非這個條件約束。 此額外清除是我們可能會考慮針對後續的主要版本。 如果這是重要請投票上在[ASP.NET Web API 使用者語音](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception)。
2. **維護與 Web API 的一致性建構**Web API 的篩選器管線是套用在特定的動作、 控制器專用或全域範圍的邏輯的彈性處理跨碼橫切入顧慮的好方法。 篩選條件，包括例外狀況篩選條件，一定要有動作和控制器內容，即使是在全域範圍中註冊。 篩選中，合約意義，但這不表示例外狀況篩選條件，即使全域範圍的項目，並不適合用來處理其中任何動作或控制器內容的情況下，例如例外狀況訊息處理常式，從某些例外狀況存在。 如果我們想要使用的例外狀況處理篩選條件所提供之彈性範圍，我們仍需要例外狀況篩選條件。 但是，如果我們需要處理例外狀況的控制器內容之外，我們也需要不同的建構 （項目不含控制器內容和動作內容的條件約束） 的完整全域錯誤處理。

### <a name="when-to-use"></a>使用時機

- 例外狀況記錄器是解決方案，可查看所有 Web 應用程式開發介面攔截到的未處理例外狀況。
- 例外狀況處理常式會攔截之 Web API 的未處理例外狀況的所有可能回應，自訂方案。
- 例外狀況篩選條件是最簡單的解決方案，以處理特定動作或控制器相關的子集未處理的例外狀況。

### <a name="service-details"></a>服務詳細資料

 例外狀況記錄器和處理常式服務介面是簡單的非同步方法都會個別的內容： 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 我們也會提供針對這兩種介面的基底類別。 覆寫核心 （同步或非同步） 方法是所有記錄，或在建議處理所需的時間。 如需記錄，`ExceptionLogger`基底類別可確保，核心記錄方法只能呼叫一次針對每個例外狀況 （即使稍後會傳播進一步呼叫堆疊和攔截到一次）。 `ExceptionHandler`基底類別會呼叫處理方法，只會針對上方的呼叫堆疊，並忽略舊版巢狀例外狀況的 catch 區塊的核心。 （這些基底類別的簡化的版本是在下列附錄中）。同時`IExceptionLogger`和`IExceptionHandler`接收透過例外狀況的相關資訊`ExceptionContext`。

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

時，架構會呼叫例外狀況記錄器或例外狀況處理常式，它一律會提供`Exception`和`Request`。 除了單元測試，可能會也永遠提供`RequestContext`。 它很少會提供`ControllerContext`和`ActionContext`（只有在呼叫 catch 區塊中的例外狀況篩選條件）。 它很少會提供`Response`（只在特定 IIS 情況下，當中間嘗試寫入回應）。 請注意，其中部分屬性可能是因為`null`檢查取用者負責決定是否`null`之前存取例外狀況類別的成員。`CatchBlock` 這字串，表示哪些 catch 區塊所看到的例外狀況。 Catch 區塊字串如下所示：

- HttpServer （SendAsync 方法）
- HttpControllerDispatcher （SendAsync 方法）
- HttpBatchHandler （SendAsync 方法）
- IExceptionFilter （ApiController 的處理的例外狀況篩選條件中管線 ExecuteAsync）
- OWIN 主機：

    - HttpMessageHandlerAdapter.BufferResponseContentAsync （緩衝的輸出）
    - HttpMessageHandlerAdapter.CopyResponseContentAsync （適用於資料流輸出）
- Web 主機：

    - HttpControllerHandler.WriteBufferedResponseContentAsync （緩衝的輸出）
    - HttpControllerHandler.WriteStreamedResponseContentAsync （適用於資料流輸出）
    - HttpControllerHandler.WriteErrorResponseContentAsync （適用於在緩衝的輸出模式下的錯誤復原的失敗）

也可以透過靜態唯讀屬性的 catch 區塊的字串清單。 （核心 catch 區塊字串位於靜態 ExceptionCatchBlocks; 其餘部分會顯示一個靜態類別上每個 OWIN 和 web 主機）。`IsTopLevelCatchBlock` 可協助遵循建議的模式處理例外狀況只能在呼叫堆疊的頂端。 而不是開啟隨處巢狀的 catch 區塊，就會發生 500 個回應例外狀況，例外狀況處理常式可以讓例外狀況傳播直到它們即將主控件無法看到。

除了`ExceptionContext`，記錄器取得更多的某項資訊透過完整`ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

第二個屬性， `CanBeHandled`，允許以識別無法處理的例外狀況記錄器。 當連接即將中止和可以傳送任何新的回應訊息，記錄器就會呼叫但此處理常式會***不***呼叫，而且記錄器可以識別此案例中的從這個屬性。

在進行其他`ExceptionContext`，處理常式會取得一個屬性可以設定完整`ExceptionHandlerContext`處理此例外狀況：

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

例外狀況處理常式會指出它已設定處理例外狀況`Result`的動作結果的屬性 (例如， [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx)， [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx)， [StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx)，或自訂的結果)。 如果`Result`屬性為 null、 例外狀況未處理，而且會重新擲回原始例外狀況。

對於例外狀況呼叫堆疊的頂端，我們採用額外的步驟，以確保應用程式開發介面呼叫的適當回應。 如果例外狀況會傳播到主機，呼叫端會看到的黃色死亡畫面，或某個其他主機提供回應通常是 HTML，而且通常不適當的 API 錯誤回應。 在這些情況下，不是 null，而且只有自訂例外狀況處理常式明確設定出結果開始回`null`（未處理的） 將例外狀況傳播到主機。 設定`Result`至`null`在這類情況下可適用於兩個案例：

1. OWIN 裝載 Web 應用程式開發介面使用自訂的例外狀況處理登錄之前/外部 Web API 的中介軟體。
2. 本機偵錯透過瀏覽器，其中的黃色死亡畫面是實際上很有幫助回應未處理例外狀況。

例外狀況記錄器和例外狀況處理常式中，我們不執行任何動作來復原如果記錄器或處理常式本身會擲回例外狀況。 （不讓例外狀況傳播，請保留此頁面底部的意見反應，如果您有更好的方法）。例外狀況記錄器和處理常式的合約是它們不應該讓例外狀況傳播至其呼叫端;否則，例外狀況將只會傳播，通常到主機導致 HTML 錯誤 （例如 ASP。網路的黃色螢幕） 傳送回用戶端 （這通常不會預期 JSON 或 XML 的應用程式開發介面呼叫端的慣用的選項）。

## <a name="examples"></a>範例

### <a name="tracing-exception-logger"></a>追蹤例外狀況記錄器

例外狀況，將資料傳送至設定的追蹤來源 （包括 Visual Studio 中的 [偵錯輸出] 視窗） 如下的例外狀況記錄器。

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>自訂錯誤訊息的例外狀況處理常式

下列會產生用戶端，包括電子郵件地址連絡支援人員的自訂錯誤回應。

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>註冊例外狀況篩選條件

如果您使用 「 ASP.NET MVC 4 Web 應用程式 」 專案範本來建立專案時，將 Web API 組態程式碼內`WebApiConfig`類別中*啟動應用程式/（_s)*資料夾：

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>附錄： 基底類別的詳細資料

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
