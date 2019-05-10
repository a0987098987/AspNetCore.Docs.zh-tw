---
title: 比較 gRPC 服務與 HTTP APIs
author: jamesnk
description: 了解如何使用 HTTP Api 和它所具有的 gRPC 比較建議的案例包括。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 03/31/2019
uid: grpc/comparison
ms.openlocfilehash: 8f4cefe1dedcf4cfd9650e73e6a1ba30dbbfeffa
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087404"
---
# <a name="comparing-grpc-services-with-http-apis"></a>比較 gRPC 服務與 HTTP APIs

藉由[James 牛頓國王](https://twitter.com/jamesnk)

這篇文章說明如何[gRPC services](https://grpc.io/docs/guides/)比較的 HTTP Api (包括 ASP.NET Core [Web Api](xref:web-api/index))。 用來提供的 API 應用程式的技術是很重要的選擇，和 gRPC 提供獨特的優點，相較於 HTTP Api。 這篇文章討論 gRPC 的優缺點，並建議使用 gRPC 透過其他技術的案例。

#### <a name="overview"></a>總覽

|    功能             |    gRPC                                                 |    使用 JSON 的 HTTP Api                       |
|------------------------|---------------------------------------------------------|----------------------------------------------|
|    合約            |    必要 (`*.proto`)                                 |    選擇性 (OpenAPI)                        |
|    Transport           |    HTTP/2                                               |    HTTP                                      |
|    承載             |    [Protobuf （小型、 二進位）](#performance)             |    JSON （很大，人們可讀取）              |
|    Prescriptiveness    |    [嚴格的規格](#strict-specification)        |    鬆散。 任何 HTTP 無效                  |
|    資料流           |    [用戶端、 伺服器、 雙向](#streaming)         |    用戶端、 伺服器                            |
|    瀏覽器支援     |    [否 （需要 grpc web）](#limited-browser-support)   |    是                                       |
|    安全性            |    傳輸 (HTTPS)                                    |    傳輸 (HTTPS)                         |
|    用戶端產生的程式碼     |    [是](#code-generation)                              |    OpenAPI + 協力廠商工具             |

## <a name="grpc-strengths"></a>gRPC 優點

### <a name="performance"></a>效能

使用序列化 gRPC 訊息[Protobuf](https://developers.google.com/protocol-buffers/docs/overview)，有效率的二進位訊息格式。 Protobuf 序列化非常快速地在伺服器和用戶端上。 在頻寬有限的案例中很重要的小型訊息承載中的 Protobuf 序列化結果，例如行動裝置應用程式。

gRPC 專為 HTTP/2，透過 HTTP 提供效能優勢明顯的 HTTP 的重大修訂 1.x:

* 二進位的框架和壓縮。 Compact 和效率，同時在傳送和接收 HTTP/2 通訊協定。
* 多工處理 HTTP/2 的多個呼叫透過單一的 TCP 連線。 多工消除[標頭的列封鎖](https://en.wikipedia.org/wiki/Head-of-line_blocking)。

### <a name="code-generation"></a>程式碼產生

所有 gRPC 架構都提供程式碼產生的第一級的支援。 GRPC 開發的核心檔案[`*.proto`檔案](https://developers.google.com/protocol-buffers/docs/proto3)，其定義 gRPC 服務和訊息合約。 從這個檔案 gRPC 架構將程式碼產生服務的基底類別、 訊息和完整的用戶端。

藉由共用`*.proto`伺服器和用戶端、 訊息與用戶端程式碼之間的檔案可以產生從端對端。 程式碼產生的用戶端會消除重複的用戶端與伺服器上的訊息，並建立為您的強型別用戶端。 不需要撰寫用戶端應用程式與多個服務中儲存大量的開發時間。

### <a name="strict-specification"></a>嚴格的規格

使用 JSON 的 HTTP api 正式規格不存在。 開發人員不斷辯論著最佳格式的 Url、 HTTP 動詞命令，並回應碼。

[GRPC 規格](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md)是 gRPC 服務必須遵循的格式規範。 gRPC 排除爭論的情況，並節省開發人員的時間，因為 gPRC 之間是一致的平台和實作。

### <a name="streaming"></a>資料流

HTTP/2 會提供長時間執行、 即時通訊資料流中的基礎。 gRPC 提供絕佳的支援，透過 HTTP/2。

GRPC 服務支援所有的資料流處理組合：

* 一元 (沒有 streaming)
* 伺服器到串流的用戶端
* 伺服器串流處理的用戶端
* 雙向資料流

### <a name="deadlinetimeouts-and-cancellation"></a>逾時期限/和取消

gRPC 可讓用戶端指定多久它們願意等候完成的 RPC。 [期限](https://grpc.io/blog/deadlines)傳送到伺服器，而且伺服器可以決定當它超過期限時要採取什麼動作。 比方說，伺服器可能會取消進行中 gRPC/HTTP/資料庫的要求逾時。

傳播的期限和取消子 gRPC 透過呼叫可協助強制執行資源使用量限制。

## <a name="grpc-recommended-scenarios"></a>gRPC 建議使用案例

gRPC 適合用於下列案例：

* **微服務** &ndash; gRPC 是設計的低延遲和高輸送量。 gRPC 也很適合輕量型的微服務效率是關鍵所在。
* **點對點的即時通訊** &ndash; gRPC 可以完全支援雙向資料流。 gRPC 服務可以推送的訊息而不需要輪詢即時。
* **Polygot 環境** &ndash; gRPC 工具支援所有熱門的開發語言，讓 gRPC 多國語言環境很好的選擇。
* **網路限制的環境** &ndash; gRPC 訊息會使用 Protobuf、 輕量級訊息格式序列化。 GRPC 訊息總是小於為相等的 JSON 訊息。

## <a name="grpc-weaknesses"></a>gRPC 弱點

### <a name="limited-browser-support"></a>有限的瀏覽器支援

就無法直接立即從瀏覽器呼叫 gRPC 服務。 gRPC 大量使用 HTTP/2 的功能和瀏覽器不提供透過 web 要求，以支援 gRPC 用戶端所需的控制層級。 例如，瀏覽器不允許呼叫者，要求使用 HTTP/2，或提供基礎的 HTTP/2 框架的存取。

[gRPC Web](https://grpc.io/docs/tutorials/basic/web.html)是 gRPC 小組所提供的有限的 gRPC 支援瀏覽器中的其他技術。 gRPC Web 是由兩個部分所組成： 支援所有現代瀏覽器和 gRPC Web proxy 伺服器的 JavaScript 用戶端。 GRPC Web 用戶端呼叫 proxy，proxy 會轉送 gRPC 要求 gRPC 伺服器上。

並非所有 gRPC 的功能都受到 gRPC Web。 不支援用戶端和雙向資料流，並進行串流處理伺服器的支援有限。

### <a name="not-human-readable"></a>不人類看得懂

HTTP API 的要求會以文字形式傳送和可以讀取及建立讓人判讀。

根據預設，gRPC 訊息被編碼使用 Protobuf。 Protobuf 有效率的方式傳送和接收時，其二進位格式不是人類可讀取。 Protobuf 要求中指定的訊息的介面描述`*.proto`正確還原序列化的檔案。 需要分析 Protobuf 裝載在網路上的，並以手動方式撰寫要求額外的工具。

這類功能[server 反映](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md)並[gRPC 命令列工具](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md)協助二進位 Protobuf 訊息存在。 此外，Protobuf 訊息支援[並進行 JSON 轉換](https://developers.google.com/protocol-buffers/docs/proto3#json)。 內建的 JSON 轉換提供有效率的方式，將 Protobuf 訊息，與人類可讀取的格式進行偵錯時。

## <a name="alternative-framework-scenarios"></a>替代架構案例

其他架構建議您針對 gRPC 在下列情況：

* **瀏覽器存取的 Api** &ndash; gRPC 完全不支援瀏覽器中。 gRPC Web 可以提供瀏覽器支援，但它有限制，並導入的伺服器 proxy。
* **廣播即時通訊** &ndash; gRPC 支援即時通訊透過串流處理，但概念的廣播已註冊的連接時的訊息不存在。 比方說在聊天室案例中，新的交談訊息應傳送至聊天室中的所有用戶端，每個 gRPC 呼叫才可個別串流處理至用戶端的新交談訊息。 [SignalR](xref:signalr/introduction)是有用的架構，在此案例。 SignalR 有持續連線和廣播訊息的內建支援的概念。
* **處理序間通訊**&ndash;程序必須裝載 HTTP/2 伺服器，以接受連入 gRPC 呼叫。 針對 Windows，處理序間通訊[管道](/dotnet/standard/io/pipe-operations)是快速、 輕量的通訊方法。

## <a name="additional-resources"></a>其他資源

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
