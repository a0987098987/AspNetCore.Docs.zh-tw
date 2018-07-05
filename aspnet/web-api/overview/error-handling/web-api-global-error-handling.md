---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: 全域錯誤處理中 ASP.NET Web API 2 |Microsoft Docs
author: davidmatson
description: ''
ms.author: aspnetcontent
ms.date: 02/03/2014
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: d1d1a081713da0771981229db8e8bd67a5103bf9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815859"
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>全域錯誤處理 ASP.NET Web API 2 中
====================
藉由[David Matson](https://github.com/davidmatson)， [Rick Anderson](https://github.com/Rick-Anderson)

立即記錄或全域處理錯誤的 Web API 中沒有任何簡單的方法。 可以透過處理一些未處理的例外狀況[例外狀況篩選條件](exception-handling.md)，但有一些無法處理例外狀況篩選條件的案例。 例如: 

1. 從控制器建構函式擲回的例外狀況。
2. 從訊息處理常式擲回的例外狀況。
3. 在路由期間擲回的例外狀況。
4. 在 回應內容序列化期間擲回的例外狀況。

我們想要提供簡單、 一致的方式記錄並處理 （如果可能的話） 這些例外狀況。 

有兩個主要的案例來處理例外狀況，我們就能夠傳送錯誤回應，並所有我們可以在其中執行的案例是記錄例外狀況的情況。 後者的情況下一個範例是當發生例外狀況的中間資料流處理回應的內容;在此情況下就無法再傳送新的回應訊息，因為狀態碼、 標頭和部分內容已經透過網路，因此我們只是中止連線。 即使無法處理的例外狀況，以產生新的回應訊息，我們仍支援記錄例外狀況。 在我們可以在其中偵測到錯誤的情況下，我們可以傳回適當的錯誤回應，如下列所示：

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>現有的選項

除了[例外狀況篩選條件](exception-handling.md)，[訊息處理常式](../advanced/http-message-handlers.md)可以立即用來觀察所有 500 層級的回應，但這些回應處理相當困難，因為它們缺少原始錯誤的相關內容。 訊息處理常式也有一些限制與所能處理的案例相關的例外狀況篩選條件的限制相同。雖然 Web API 有擷取錯誤狀況的追蹤基礎結構追蹤基礎結構供診斷之用，未設計或適用於生產環境中執行。 全域例外狀況處理和記錄應該可以在實際執行期間執行，並插入現有的監視解決方案的服務 (例如[ELMAH](https://code.google.com/p/elmah/) )。

### <a name="solution-overview"></a>解決方案概觀

 我們提供兩個新的使用者可取代服務， [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md)和 IExceptionHandler，記錄並處理未處理例外狀況。 服務有非常類似，但有兩個主要的差異：

1. 我們支援多個例外狀況記錄器，但只有單一例外狀況處理常式註冊。
2. 例外狀況記錄器一律取得呼叫，即使我們即將中止連接。 只有當我們仍然可以選擇要傳送的回應訊息時，取得呼叫例外狀況處理常式。

這兩項服務提供存取權包含來自其中偵測到例外狀況，點的相關資訊的例外狀況內容特別[HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx)，則[HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx)，則會擲回例外狀況和例外狀況來源 （下面的詳細資料）。

### <a name="design-principles"></a>設計原則

1. **沒有重大變更**因為次要版本，其中一個影響解決方案的重要限制是，不會有任何重大變更，請輸入合約或行為，要加入這項功能。 這個條件約束會排除某些清除工作，我們想要完成根據現有開啟到 500 個回應的例外狀況的 catch 區塊。 這個額外的清除作業是我們可能要考慮在下一個主要版本。 如果這是一定要您請它在投票[ASP.NET Web API 使用者心聲](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception)。
2. **維護與 Web API 的一致性建構**Web API 的篩選條件管線是適合用來處理跨領域關注套用的邏輯在特定動作、 控制器特定或全域範圍的彈性。 篩選條件，包括例外狀況篩選條件，一律具有動作和控制器內容，即使在已註冊在全域範圍。 合約最適合篩選，但這不表示例外狀況篩選條件，甚至是全域範圍的項目，不適合用於處理其中任何動作或控制器內容的情況下，例如例外狀況訊息處理常式，從某些例外狀況存在。 如果我們想要使用之例外狀況處理的篩選器所提供的彈性範圍，我們仍需要例外狀況篩選條件。 但是，如果我們需要處理例外狀況的控制器內容之外，我們還需要不同的建構 （項目不含的控制器內容和動作內容條件約束） 的完整全域錯誤處理。

### <a name="when-to-use"></a>使用時機

- 例外狀況記錄器是解決方案，可看到 Web API 所攔截到的所有未處理例外狀況。
- 例外狀況處理常式是自訂 Web API 所攔截到的未處理例外狀況的所有可能回應的解決方案。
- 例外狀況篩選條件是最簡單的解決方案，來處理相關的特定動作或控制器子集未處理的例外狀況。

### <a name="service-details"></a>服務詳細資料

 例外狀況記錄器和處理常式服務介面是簡單的非同步方法採取個別的內容： 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 我們也會提供針對這兩種介面的基底類別。 覆寫核心 （同步或非同步） 方法是所有所需的記錄，或在建議的處理時間。 如需記錄，`ExceptionLogger`基底類別可確保的核心記錄方法只呼叫一次每個例外狀況 （即使它之後會傳播進一步呼叫堆疊和攔截到一次）。 `ExceptionHandler`基底類別會呼叫處理方法，只針對上方的呼叫堆疊，並忽略舊版巢狀例外狀況 catch 區塊的核心。 （這些基底類別的簡化的版本是下面的 「 附錄 」 中）。兩者`IExceptionLogger`並`IExceptionHandler`接收透過例外狀況的相關資訊`ExceptionContext`。

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

時，架構會呼叫例外狀況記錄器或例外狀況處理常式，它一律會提供`Exception`和`Request`。 除了單元測試中，它也總是能提供`RequestContext`。 它很少會提供`ControllerContext`和`ActionContext`（僅限時的例外狀況篩選條件，以呼叫 catch 區塊中）。 它很少會提供`Response`（只在 IIS 有時候時中間嘗試寫入回應）。 請注意，因為其中部分屬性可能`null`負責檢查取用者`null`才能存取例外狀況類別的成員。`CatchBlock` 這字串，表示哪一個 catch 區塊中看到例外狀況。 Catch 區塊字串如下所示：

- HttpServer （SendAsync 方法）
- HttpControllerDispatcher （SendAsync 方法）
- HttpBatchHandler （SendAsync 方法）
- IExceptionFilter （ApiController 的處理的例外狀況篩選條件管線中 ExecuteAsync）
- OWIN 主機：

    - HttpMessageHandlerAdapter.BufferResponseContentAsync （適用於緩衝處理輸出）
    - HttpMessageHandlerAdapter.CopyResponseContentAsync （適用於資料流輸出）
- Web 主機：

    - HttpControllerHandler.WriteBufferedResponseContentAsync （適用於緩衝處理輸出）
    - HttpControllerHandler.WriteStreamedResponseContentAsync （適用於資料流輸出）
    - HttpControllerHandler.WriteErrorResponseContentAsync （適用於在緩衝的輸出模式下的錯誤復原的失敗）

也可透過靜態唯讀屬性的 catch 區塊的字串清單。 （core catch 區塊字串位於靜態 ExceptionCatchBlocks; 其餘部分會出現一個靜態類別上每個 OWIN 和 web 主機）。`IsTopLevelCatchBlock` 可協助您遵循建議的模式處理例外狀況只能在呼叫堆疊的頂端。 而不是開啟到 500 個回應任何地方的巢狀的 catch 區塊，就會發生的例外狀況，例外狀況處理常式可以讓例外狀況傳播，直到它們即將看到主應用程式。

除了`ExceptionContext`，記錄器會取得一項資訊透過完整`ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

第二個屬性， `CanBeHandled`，允許記錄器以識別無法處理的例外狀況。 當即將中止連接和可以傳送任何新的回應訊息中，記錄器就會呼叫但處理常式會***不***呼叫，而且記錄器可以識別這種情況下，從這個屬性。

在進行其他`ExceptionContext`，處理常式會取得一個屬性可以設定完整`ExceptionHandlerContext`處理例外狀況：

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

例外狀況處理常式會指出它已設定來處理例外狀況`Result`的動作結果的屬性 (例如[ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx)， [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx)， [StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx)，或自訂的結果)。 如果`Result`屬性為 null、 例外狀況無法處理，因此會重新擲回原始的例外狀況。

對於上方的呼叫堆疊的例外狀況，我們會採用額外的步驟，以確保回應是適用於 API 呼叫端。 如果例外狀況會傳播至主應用程式中，呼叫端會看到黃色的死亡畫面，或一些其他的主機提供通常是 HTML 回應和通常不適當的 API 錯誤回應。 在這些情況下，不是 null，而且只有自訂例外狀況處理常式明確設定結果開始回`null`（未處理的） 將例外狀況傳播到主機。 設定`Result`至`null`在此情況下可以是適用於兩個案例：

1. OWIN 裝載 Web API 與自訂的例外狀況處理登錄之前/外部 Web API 的中介軟體。
2. 本機偵錯透過瀏覽器，其中的黃色死亡畫面是實際上很有幫助回應未處理的例外狀況。

如需例外狀況記錄器和例外狀況處理常式中，我們不會有記錄器或處理常式本身會擲回例外狀況時復原。 （以外讓例外狀況傳播，請將保留在此頁面底部提供意見反應，如果您有更好的方法）。例外狀況記錄器和處理常式的合約是它們不應該讓例外狀況傳播到呼叫者;否則，例外狀況會只傳播，通常，一直到主機導致 HTML 錯誤 （例如 ASP。NET 的黃色螢幕） 傳送回用戶端 （這通常不會預期 JSON 或 XML 的 API 呼叫端的慣用的選項）。

## <a name="examples"></a>範例

### <a name="tracing-exception-logger"></a>追蹤例外狀況記錄器

下面的例外狀況記錄器會將例外狀況資料傳送至已設定的追蹤來源 （包括 Visual Studio 中的偵錯輸出視窗）。

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>自訂錯誤訊息的例外狀況處理常式

請遵循下列產生自訂錯誤回應給用戶端，包括電子郵件地址連絡支援人員。

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>註冊的例外狀況篩選條件

如果您使用 「 ASP.NET MVC 4 Web 應用程式 」 專案範本來建立專案時，將您的 Web API 組態程式碼內`WebApiConfig`類別，在*應用程式/（_s)* 資料夾：

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>附錄： 基底類別的詳細資料

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
