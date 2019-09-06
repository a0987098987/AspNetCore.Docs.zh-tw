---
title: ASP.NET Core 設定的 gRPC
author: jamesnk
description: 瞭解如何設定 ASP.NET Core 應用程式的 gRPC。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/21/2019
uid: grpc/configuration
ms.openlocfilehash: 34eb598211c87fbb2c68ae5e041da50d02f543f7
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310321"
---
# <a name="grpc-for-aspnet-core-configuration"></a>ASP.NET Core 設定的 gRPC

## <a name="configure-services-options"></a>設定服務選項

下表說明設定 gRPC 服務的選項：

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `MaxSendMessageSize` | `null` | 可以從伺服器傳送的訊息大小上限（以位元組為單位）。 如果嘗試傳送的訊息超過設定的訊息大小上限，就會產生例外狀況。 |
| `MaxReceiveMessageSize` | 4 MB | 伺服器可以接收的訊息大小上限（以位元組為單位）。 如果伺服器收到超過此限制的訊息，就會擲回例外狀況。 增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。 |
| `EnableDetailedErrors` | `false` | 如果`true`為，則在服務方法中擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。 預設為 `false`。 將`EnableDetailedErrors`設定`true`為可能會洩漏機密資訊。 |
| `CompressionProviders` | gzip、deflate | 用來壓縮和解壓縮訊息的壓縮提供者集合。 您可以建立自訂壓縮提供者，並將其新增至集合。 預設設定的提供者支援**gzip**和**deflate**壓縮。 |
| `ResponseCompressionAlgorithm` | `null` | 壓縮演算法，用來壓縮從伺服器傳送的訊息。 演算法必須符合中`CompressionProviders`的壓縮提供者。 若要讓演算法壓縮回應，用戶端必須在**grpc-accept-encoding**標頭中傳送它，以指示它支援演算法。 |
| `ResponseCompressionLevel` | `null` | 壓縮層級，用來壓縮從伺服器傳送的訊息。 |

您可以為所有服務設定選項，方法是提供選項委派給`AddGrpc`中`Startup.ConfigureServices`的呼叫：

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

單一服務的選項會覆寫中`AddGrpc`提供的全域選項，而且可以使用`AddServiceOptions<TService>`來設定：

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>設定用戶端選項

下表說明設定 gRPC 通道的選項：

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `HttpClient` | 新增實例 | 用`HttpClient`來進行 gRPC 呼叫的。 用戶端可以設定為設定自訂`HttpClientHandler`，或將額外的處理常式新增至 HTTP 管線以進行 gRPC 呼叫。 預設會建立新`HttpClient`的實例。 |
| `MaxSendMessageSize` | `null` | 可以從用戶端傳送的訊息大小上限（以位元組為單位）。 如果嘗試傳送的訊息超過設定的訊息大小上限，就會產生例外狀況。 |
| `MaxReceiveMessageSize` | 4 MB | 用戶端可以接收的訊息大小上限（以位元組為單位）。 如果伺服器收到超過此限制的訊息，就會擲回例外狀況。 增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。 |
| `TransportOptions` | `null` | 傳輸選項會設定通道呼叫 gRPC 服務的方式。 目前唯一的實`HttpClientTransport`作為選項可讓您指定 gRPC 所使用的。 `HttpClient` |
| `Credentials` | `null` | `ChannelCredentials` 執行個體。 認證是用來將驗證中繼資料新增至 gRPC 呼叫。 |
| `CompressionProviders` | gzip、deflate | 用來壓縮和解壓縮訊息的壓縮提供者集合。 您可以建立自訂壓縮提供者，並將其新增至集合。 預設設定的提供者支援**gzip**和**deflate**壓縮。 |

下列程式碼範例：

* 設定通道上的傳送和接收訊息大小上限。
* 建立用戶端。

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

## <a name="additional-resources"></a>其他資源

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:tutorials/grpc/grpc-start>
