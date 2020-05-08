---
title: 適用于 .NET 設定的 gRPC
author: jamesnk
description: 瞭解如何設定 .NET 應用程式的 gRPC。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 02/26/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: grpc/configuration
ms.openlocfilehash: 6e4259b538d32490d5281f189a04ab5a04dbcce5
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82774687"
---
# <a name="grpc-for-net-configuration"></a>適用于 .NET 設定的 gRPC

## <a name="configure-services-options"></a>設定服務選項

gRPC 服務會在`AddGrpc` *Startup.cs*中以設定。 下表說明設定 gRPC 服務的選項：

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| MaxSendMessageSize | `null` | 可以從伺服器傳送的訊息大小上限（以位元組為單位）。 如果嘗試傳送的訊息超過設定的訊息大小上限，就會產生例外狀況。 當設定為`null`時，訊息大小不受限制。 |
| MaxReceiveMessageSize | 4 MB | 伺服器可以接收的訊息大小上限（以位元組為單位）。 如果伺服器收到超過此限制的訊息，就會擲回例外狀況。 增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。 當設定為`null`時，訊息大小不受限制。 |
| EnableDetailedErrors | `false` | 如果`true`為，則在服務方法中擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。 預設值為 `false`。 將`EnableDetailedErrors`設定`true`為可能會洩漏機密資訊。 |
| CompressionProviders | gzip | 用來壓縮和解壓縮訊息的壓縮提供者集合。 您可以建立自訂壓縮提供者，並將其新增至集合。 預設設定的提供者支援**gzip**壓縮。 |
| <span style="word-break:normal;word-wrap:normal">ResponseCompressionAlgorithm</span> | `null` | 壓縮演算法，用來壓縮從伺服器傳送的訊息。 演算法必須符合中`CompressionProviders`的壓縮提供者。 若要讓演算法壓縮回應，用戶端必須在**grpc-accept-encoding**標頭中傳送它，以指示它支援演算法。 |
| ResponseCompressionLevel | `null` | 壓縮層級，用來壓縮從伺服器傳送的訊息。 |
| 攔截器 | 無 | 以每個 gRPC 呼叫執行的攔截器集合。 攔截器會依照其註冊的循序執行。 全域設定的攔截器會在設定單一服務的攔截器之前執行。 如需 gRPC 攔截器的詳細資訊，請參閱[GRPC 攔截器與中介軟體](xref:grpc/migration#grpc-interceptors-vs-middleware)。 |

您可以為所有服務設定選項，方法是提供選項委派給`AddGrpc`中`Startup.ConfigureServices`的呼叫：

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

單一服務的選項會覆寫中`AddGrpc`提供的全域選項，而且可以使用`AddServiceOptions<TService>`來設定：

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>設定用戶端選項

gRPC client configuration 已設定為`GrpcChannelOptions`on。 下表說明設定 gRPC 通道的選項：

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| HttpClient | 新增實例 | 用`HttpClient`來進行 gRPC 呼叫的。 用戶端可以設定為設定自訂`HttpClientHandler`，或將額外的處理常式新增至 HTTP 管線以進行 gRPC 呼叫。 `HttpClient`如果未指定，則會為通道`HttpClient`建立新的實例。 它會自動被處置。 |
| DisposeHttpClient | `false` | 如果`true`指定了和`HttpClient` ，則在處置時`HttpClient` `GrpcChannel` ，將會處置實例。 |
| Server.loggerfactory | `null` | 客戶`LoggerFactory`端用來記錄 gRPC 呼叫相關資訊的。 `LoggerFactory`實例可以從相依性插入解析，或使用`LoggerFactory.Create`建立。 如需設定記錄的範例， <xref:grpc/diagnostics#grpc-client-logging>請參閱。 |
| MaxSendMessageSize | `null` | 可以從用戶端傳送的訊息大小上限（以位元組為單位）。 如果嘗試傳送的訊息超過設定的訊息大小上限，就會產生例外狀況。 當設定為`null`時，訊息大小不受限制。 |
| <span style="word-break:normal;word-wrap:normal">MaxReceiveMessageSize</span> | 4 MB | 用戶端可以接收的訊息大小上限（以位元組為單位）。 如果用戶端收到超過此限制的訊息，就會擲回例外狀況。 增加此值可讓用戶端接收較大的訊息，但可能會對記憶體耗用量造成負面影響。 當設定為`null`時，訊息大小不受限制。 |
| 認證 | `null` | `ChannelCredentials` 執行個體。 認證是用來將驗證中繼資料新增至 gRPC 呼叫。 |
| CompressionProviders | gzip | 用來壓縮和解壓縮訊息的壓縮提供者集合。 您可以建立自訂壓縮提供者，並將其新增至集合。 預設設定的提供者支援**gzip**壓縮。 |

下列程式碼：

* 設定通道上的傳送和接收訊息大小上限。
* 建立用戶端。

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>其他資源

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:grpc/diagnostics>
* <xref:tutorials/grpc/grpc-start>
