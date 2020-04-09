---
title: 比較 gRPC 服務與 HTTP API
author: jamesnk
description: 瞭解 gRPC 與 HTTP API 的比較情況,以及建議的方案是什麼。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 12/05/2019
no-loc:
- SignalR
uid: grpc/comparison
ms.openlocfilehash: 2dff64f1f2d67b8a1e676acf6cf131b684099750
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80405875"
---
# <a name="compare-grpc-services-with-http-apis"></a>比較 gRPC 服務與 HTTP API

由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)

本文介紹了[gRPC 服務](https://grpc.io/docs/guides/)如何與 HTTP API(包括 ASP.NET 核心[Web API](xref:web-api/index)進行比較。 用於為應用提供 API 的技術是一個重要的選擇,gRPC 與 HTTP API 相比提供了獨特的優勢。 本文討論了 gRPC 的優點和缺點,並推薦了與其他技術使用 gRPC 的方案。

## <a name="high-level-comparison"></a>進階比較

下表提供了使用 JSON 的 gRPC 和 HTTP API 之間的功能的進階比較。

| 功能          | gRPC                                               | 帶有 JSON 的 HTTP API           |
| ---------------- | -------------------------------------------------- | ----------------------------- |
| 合約         | 必要 (*.proto*)                                | 選擇(開啟 API)            |
| 通訊協定         | HTTP/2                                             | HTTP                          |
| Payload          | [原發(小,二進位)](#performance)           | JSON(大,人類可讀)  |
| 規定性 | [嚴格的規格](#strict-specification)      | 鬆散。 任何 HTTP 都有效。     |
| 串流        | [用戶端、伺服器、雙向](#streaming)       | 用戶端、伺服器                |
| 瀏覽器支援  | [否(需要 grpc-web)](#limited-browser-support) | 是                           |
| 安全性         | 傳輸(TLS)                                    | 傳輸(TLS)               |
| 用戶端代碼產生 | [是](#code-generation)                      | OpenAPI + 第三方工具 |

## <a name="grpc-strengths"></a>gRPC 優勢

### <a name="performance"></a>效能

gRPC 訊息使用[Protobuf(](https://developers.google.com/protocol-buffers/docs/overview)一種高效的二進位訊息格式)進行序列化。 Protobuf 在伺服器和用戶端上非常快速地序列化。 Protobuf 序列化會導致小消息負載,在有限的頻寬方案中(如移動應用)非常重要。

gRPC 專為 HTTP/2 設計,這是 HTTP 的主要修訂版,與 HTTP 1.x 具有顯著的性能優勢:

* 二進位框架和壓縮。 HTTP/2 協定在發送和接收方面緊湊且高效。
* 通過單個 TCP 連接對多個 HTTP/2 調用進行多路複用。 多路複用消除了[線頭阻塞](https://en.wikipedia.org/wiki/Head-of-line_blocking)。

### <a name="code-generation"></a>產生程式碼

所有 gRPC 框架都為代碼生成提供一流的支援。 gRPC 開發的核心檔案是[.proto 檔案](https://developers.google.com/protocol-buffers/docs/proto3),它定義了 gRPC 服務和消息的協定。 從這個檔gRPC框架將編寫代碼生成服務基類、消息和完整的用戶端。

通過在伺服器和客戶端之間共用 *.proto*檔案,可以端到端生成消息和客戶端代碼。 用戶端的代碼生成消除了用戶端和伺服器上的消息重複,並為您創建了一個強類型用戶端。 無需編寫用戶端可在具有許多服務的應用程式中節省大量開發時間。

### <a name="strict-specification"></a>嚴格的規格

JSON 的 HTTP API 的正式規範不存在。 開發人員辯論 URL、HTTP 謂詞和回應代碼的最佳格式。

[gRPC 規範](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md)對 gRPC 服務必須遵循的格式有規定性。 gRPC 消除了爭論並節省了開發人員時間,因為 gRPC 跨平臺和實現是一致的。

### <a name="streaming"></a>串流

HTTP/2 為長期即時通信流提供了基礎。 gRPC 為 HTTP/2 流式處理提供一流支援。

gRPC 服務支援所有串流式處理組合:

* 一元(無流)
* 伺服器到用戶端串流式處理
* 用戶端到伺服器流式處理
* 雙向流式處理

### <a name="deadlinetimeouts-and-cancellation"></a>截止日期/超時和取消

gRPC 允許用戶端指定他們願意等待 RPC 完成的時間。 [截止時間](https://grpc.io/blog/deadlines)將發送到伺服器,伺服器可以決定,如果超過截止時間,該伺服器將執行什麼操作。 例如,伺服器可能會取消在超時時正在進行的 gRPC/HTTP/資料庫請求。

通過子 gRPC 調用傳播截止時間並取消有助於強制實施資源使用限制。

## <a name="grpc-recommended-scenarios"></a>gRPC 建議的機制

gRPC 非常適合以下機制:

* **微服務**&ndash;gRPC 專為低延遲和高吞吐量通信而設計。 gRPC 非常適合效率至關重要的輕量級微服務。
* **點對點即時通信**&ndash;gRPC 對雙向流流具有出色的支援。 gRPC 服務無需輪詢即可即時推送消息。
* **多格樂環境**&ndash;gRPC 工具支援所有流行的開發語言,使 gRPC 成為多語言環境的最佳選擇。
* **網路受限環境**&ndash;gRPC 消息使用 Protobuf(一種輕量級消息格式)進行序列化。 gRPC 消息始終小於等效的 JSON 消息。

## <a name="grpc-weaknesses"></a>gRPC 弱點

### <a name="limited-browser-support"></a>有限的瀏覽器支援

今天不可能直接從瀏覽器調用 gRPC 服務。 gRPC 大量使用 HTTP/2 功能,沒有瀏覽器提供支援 gRPC 用戶端的 Web 請求所需的控制等級。 例如,瀏覽器不允許調用方要求使用 HTTP/2,或提供對基礎 HTTP/2 幀的訪問。

[gRPC-Web](https://grpc.io/docs/tutorials/basic/web.html)是 gRPC 團隊的附加技術,在瀏覽器中提供有限的 gRPC 支援。 gRPC-Web 由兩部分組成:支援所有現代瀏覽器的 JavaScript 用戶端和伺服器上的 gRPC-Web 代理。 gRPC-Web 用戶端呼叫代理,代理將在 gRPC 請求上轉寄到 gRPC 伺服器。

並非所有 gRPC 的功能都由 gRPC-Web 支援。 不支援用戶端和雙向流,並且對伺服器流的支援有限。

> [!TIP]
> .NET 核心對 gRPC-Web 具有實驗性支援。 請訪問<xref:grpc/browser>瞭解更多資訊。

### <a name="not-human-readable"></a>不可人類可讀

HTTP API 請求以文本形式發送,可以由人類讀取和創建。

默認情況下,gRPC 消息使用 Protobuf 進行編碼。 雖然 Protobuf 是有效的傳送和接收, 其二進位格式不是人類可讀的. Protobuf 要求消息在 *.proto*檔中指定的介面描述才能正確反序列化。 需要額外的工具來分析導線上的 Protobuf 有效負載,並手動撰寫請求。

[存在伺服器反射](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md)和[gRPC 命令列工具](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md)等功能,以幫助處理二進制 Protobuf 消息。 此外,Protobuf 訊息支援[轉換和從 JSON](https://developers.google.com/protocol-buffers/docs/proto3#json)。 內建的 JSON 轉換提供了一種在調試時將 Protobuf 消息轉換為和從人類可讀形式轉換的有效方法。

## <a name="alternative-framework-scenarios"></a>替代框架機制

在以下情況下,建議透過 gRPC 使用其他框架:

* 瀏覽器中不完全支援**瀏覽器可訪問的 API** &ndash; gRPC。 gRPC-Web 可以提供瀏覽器支援,但它有限制並引入了伺服器代理。
* **廣播即時通信**&ndash;gRPC 支援通過流進行即時通信,但不存在將消息廣播到已註冊連接的概念。 例如,在聊天室方案中,應向聊天室中的所有用戶端發送新的聊天訊息,需要每個 gRPC 調用才能單獨將新的聊天訊息流式傳輸到用戶端。 [SignalR](xref:signalr/introduction)是此方案的有用框架。 SignalR具有持久連接和對廣播消息的內置支援的概念。
* **進程間通信**&ndash;進程必須承載 HTTP/2 伺服器才能接受傳入的 gRPC 調用。 對於 Windows,進程間通信[管道](/dotnet/standard/io/pipe-operations)是一種快速、輕量級的通信方法。

## <a name="additional-resources"></a>其他資源

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
