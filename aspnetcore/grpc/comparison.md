---
title: 比較 gRPC 服務與 HTTP API
author: jamesnk
description: 瞭解 gRPC 與 HTTP Api 的比較，以及它的建議案例。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 12/05/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: grpc/comparison
ms.openlocfilehash: f622a1518781c255d36762dc651f975625dabf7c
ms.sourcegitcommit: 6a71b560d897e13ad5b61d07afe4fcb57f8ef6dc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/09/2020
ms.locfileid: "84106126"
---
# <a name="compare-grpc-services-with-http-apis"></a>比較 gRPC 服務與 HTTP API

依[James 牛頓-王](https://twitter.com/jamesnk)

本文說明[gRPC 服務](https://grpc.io/docs/guides/)如何與 JSON （包括 ASP.NET Core [web api](xref:web-api/index)）中的 HTTP api 進行比較。 用來為您的應用程式提供 API 的技術是一項重要的選擇，gRPC 提供與 HTTP Api 相較之下的獨特優點。 本文討論 gRPC 的優點和缺點，並建議在其他技術上使用 gRPC 的案例。

## <a name="high-level-comparison"></a>高階比較

下表提供 gRPC 和 HTTP Api 與 JSON 之間功能的高階比較。

| 功能          | gRPC                                               | HTTP Api 與 JSON           |
| ---------------- | -------------------------------------------------- | ----------------------------- |
| 合約         | 必要（*proto*）                                | 選擇性（OpenAPI）            |
| 通訊協定         | HTTP/2                                             | HTTP                          |
| Payload          | [Protobuf （小型，二進位）](#performance)           | JSON （大型、人類可讀取）  |
| Prescriptiveness | [嚴格規格](#strict-specification)      | 鬆動. 任何 HTTP 都是有效的。     |
| 串流        | [用戶端，伺服器，雙向](#streaming)       | 用戶端，伺服器                |
| 瀏覽器支援  | [否（需要 grpc-web）](#limited-browser-support) | 是                           |
| 安全性         | 傳輸（TLS）                                    | 傳輸（TLS）               |
| 用戶端程式代碼產生 | [是](#code-generation)                      | OpenAPI + 協力廠商工具 |

## <a name="grpc-strengths"></a>gRPC 的優點

### <a name="performance"></a>效能

gRPC 訊息會使用[Protobuf](https://developers.google.com/protocol-buffers/docs/overview)序列化，這是一種有效率的二進位訊息格式。 Protobuf 會非常快速地在伺服器和用戶端上序列化。 在小型訊息承載中 Protobuf 序列化結果，這在有限的頻寬案例（例如行動應用程式）中很重要。

gRPC 是針對 HTTP/2 所設計，這是一種可透過 HTTP 1.x 提供顯著效能優勢的 HTTP 主要修訂版：

* 二進位框架和壓縮。 HTTP/2 通訊協定在傳送和接收時都是精簡且有效率的。
* 透過單一 TCP 連線進行多個 HTTP/2 呼叫的多工處理。 多工化可避免[行標頭封鎖](https://en.wikipedia.org/wiki/Head-of-line_blocking)。

HTTP/2 不是 gRPC 專屬的。 許多要求類型，包括具有 JSON 的 HTTP Api，都可以使用 HTTP/2，並受益于其效能改進。

### <a name="code-generation"></a>程式碼產生

所有的 gRPC 架構都提供第一級的程式碼產生支援。 GRPC 開發的核心檔案是定義 gRPC 服務和訊息合約的[proto](https://developers.google.com/protocol-buffers/docs/proto3)檔案。 在此檔案中，gRPC 架構會產生服務基類、訊息和完整用戶端的程式碼。

藉由共用伺服器和用戶端之間的*proto*檔案，可以從端對端產生訊息和用戶端程式代碼。 用戶端的程式碼產生不會在用戶端和伺服器上排除重複的訊息，並為您建立強型別用戶端。 不需要撰寫用戶端，就能在具有許多服務的應用程式中節省大量的開發時間。

### <a name="strict-specification"></a>嚴格規格

具有 JSON 的 HTTP API 的正式規格不存在。 開發人員會爭論 Url、HTTP 動詞命令和回應碼的最佳格式。

[GRPC 規格](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md)規定了 gRPC 服務必須遵循的格式。 gRPC 可排除爭論並節省開發人員的時間，因為 gRPC 在平臺和實現之間是一致的。

### <a name="streaming"></a>串流

HTTP/2 提供長時間即時通訊資料流程的基礎。 gRPC 提供透過 HTTP/2 進行串流的第一級支援。

GRPC 服務支援所有串流組合：

* 一元（無串流）
* 伺服器對用戶端串流
* 用戶端對伺服器的串流處理
* 雙向串流

### <a name="deadlinetimeouts-and-cancellation"></a>期限/超時和取消

gRPC 可讓用戶端指定他們願意等待 RPC 完成的時間長度。 [期限](https://grpc.io/blog/deadlines)會傳送至伺服器，而伺服器可以決定在超過期限時要採取的動作。 例如，伺服器可能會在超時時取消進行中的 gRPC/HTTP/資料庫要求。

透過子 gRPC 呼叫來傳播期限和取消，有助於強制執行資源使用限制。

## <a name="grpc-recommended-scenarios"></a>gRPC 建議的案例

gRPC 適用于下列案例：

* **微服務**： gRPC 是針對低延遲和高輸送量通訊所設計。 gRPC 非常適合用於效率非常重要的輕量微服務。
* **點對點即時通訊**： gRPC 有絕佳的雙向串流支援。 gRPC 服務可以即時推送訊息，而不需要輪詢。
* **多語言環境**： gRPC 工具支援所有熱門的開發語言，讓 gRPC 成為多語言環境的理想選擇。
* **網路限制環境**： gRPC 訊息會使用 Protobuf （輕量訊息格式）進行序列化。 GRPC 訊息一律會小於對等的 JSON 訊息。

## <a name="grpc-weaknesses"></a>gRPC 弱點

### <a name="limited-browser-support"></a>有限的瀏覽器支援

目前無法從瀏覽器直接呼叫 gRPC 服務。 gRPC 大量使用 HTTP/2 功能，而且沒有瀏覽器提供 web 要求所需的控制層級來支援 gRPC 用戶端。 例如，瀏覽器不允許呼叫端要求使用 HTTP/2，或提供基礎 HTTP/2 框架的存取權。

[gRPC-Web](https://grpc.io/docs/tutorials/basic/web.html)是 gRPC 團隊提供的額外技術，可在瀏覽器中提供有限的 gRPC 支援。 gRPC 是由兩個部分組成：支援所有新式瀏覽器的 JavaScript 用戶端，以及伺服器上的 gRPC Web proxy。 GRPC-Web 用戶端會呼叫 proxy，而 proxy 會在 gRPC 要求上轉送至 gRPC 伺服器。

GRPC-Web 不支援所有 gRPC 的功能。 用戶端和雙向串流不受支援，而且對伺服器串流的支援有限。

> [!TIP]
> .NET Core 具有 gRPC Web 的實驗性支援。 如需詳細資訊，請造訪 <xref:grpc/browser> 。

### <a name="not-human-readable"></a>不是人類看得懂

HTTP API 要求會以文字傳送，並可供人類讀取和建立。

根據預設，gRPC 訊息會以 Protobuf 編碼。 雖然 Protobuf 的傳送和接收效率較高，但其二進位格式卻不容易閱讀。 Protobuf 需要指定于*proto*檔案中的訊息介面描述，才能正確還原序列化。 需要額外的工具，才能在網路上分析 Protobuf 承載，並以手動方式撰寫要求。

[伺服器反映](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md)和[gRPC 命令列工具](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md)等功能都存在，以協助進行二進位 Protobuf 訊息。 此外，Protobuf 訊息支援[JSON 的轉換](https://developers.google.com/protocol-buffers/docs/proto3#json)。 內建的 JSON 轉換提供了一種有效率的方式，可在進行偵錯工具時，將 Protobuf 訊息轉換成人類看得懂的格式。

## <a name="alternative-framework-scenarios"></a>替代架構案例

在下列案例中，建議您透過 gRPC 使用其他架構：

* **瀏覽器可存取的 api**：瀏覽器中未完全支援 gRPC。 gRPC-Web 可以提供瀏覽器支援，但它有一些限制，而且引進了伺服器 proxy。
* **廣播即時通訊**： gRPC 支援透過串流進行即時通訊，但將訊息廣播到已註冊連線的概念並不存在。 例如，在聊天室案例中，新的聊天訊息應傳送至聊天室中的所有用戶端時，每個 gRPC 呼叫都需要個別將新的聊天訊息串流至用戶端。 [SignalR](xref:signalr/introduction)在此案例中是有用的架構。 SignalR具有持續性連接的概念，以及廣播訊息的內建支援。
* **處理序間通訊**：處理常式必須裝載 HTTP/2 伺服器，以接受傳入的 gRPC 呼叫。 對於 Windows 而言，處理序間通訊[管道](/dotnet/standard/io/pipe-operations)是快速、輕量的通訊方法。

## <a name="additional-resources"></a>其他資源

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
