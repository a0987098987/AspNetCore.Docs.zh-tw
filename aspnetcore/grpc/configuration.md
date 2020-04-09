---
title: 用於 .NET 設定的 gRPC
author: jamesnk
description: 瞭解如何為 .NET 應用配置 gRPC。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 02/26/2020
uid: grpc/configuration
ms.openlocfilehash: cabe2d86f535bf3063dd7ede9e8a3bc5de70e244
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78666898"
---
# <a name="grpc-for-net-configuration"></a>用於 .NET 設定的 gRPC

## <a name="configure-services-options"></a>設定服務選項

gRPC`AddGrpc`服務 在*Startup.cs*中配置。 下表描述了用於設定 gRPC 服務的選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| 最大訊息大小 | `null` | 可以從伺服器發送的最大消息大小(以位元組為單位)。 嘗試發送超過配置的最大消息大小的消息會導致異常。 |
| 最大接收訊息大小 | 4 MB | 伺服器可以接收的最大消息大小(以位元組為單位)。 如果伺服器收到的消息超過此限制,則會引發異常。 增加此值允許伺服器接收較大的消息,但可能會對記憶體消耗產生負面影響。 |
| 開啟詳細錯誤 | `false` | 如果在`true`服務方法中引發異常時,將返回到用戶端的詳細異常消息。 預設值為 `false`。 設置為`EnableDetailedErrors``true`可能會洩漏敏感資訊。 |
| 壓縮提供者 | gzip | 用於壓縮和解壓縮消息的壓縮提供程式的集合。 可以創建自定義壓縮提供程式並將其添加到集合中。 默認配置的提供者支援**gzip**壓縮。 |
| <span style="word-break:normal;word-wrap:normal">回應壓縮演演算法</span> | `null` | 用於壓縮從伺服器發送的消息的壓縮演演演算法。 該演演演算法`CompressionProviders`必須與中的壓縮提供程式匹配。 對於演演演算法要壓縮回應,客戶端必須通過在**grpc 接受編碼**標頭中發送該演演演算法來指示它支援該演演演算法。 |
| 回應壓縮層級 | `null` | 用於壓縮從伺服器發送的消息的壓縮級別。 |
| 攔截器 | None | 每個 gRPC 調用一起運行的攔截器的集合。 攔截器按註冊順序運行。 全域配置的攔截器在為單個服務配置的攔截器之前運行。 有關 gRPC 攔截器的詳細資訊,請參閱[gRPC 攔截器與中間件](xref:grpc/migration#grpc-interceptors-vs-middleware)。 |

可以通過在`AddGrpc``Startup.ConfigureServices`中 提供委託給調用的選項來為所有服務配置選項。

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

單個服務的選項覆蓋`AddGrpc`中 提供的全域選項,並且可以使用`AddServiceOptions<TService>`設定 :

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>設定客戶端選項

gRPC 客戶端設定`GrpcChannelOptions`設定值值 。 下表描述了用於設定 gRPC 通道的選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| HttpClient | 新實體 | `HttpClient`用於進行 gRPC 調用。 可以將用戶端設置為配置自定義`HttpClientHandler`,或向 gRPC 調用的 HTTP 管道添加其他處理程式。 `HttpClient`如果未指定,則為通道創建新`HttpClient`實例。 它將自動釋放。 |
| 處置客戶 | `false` | 如果`true`指定`HttpClient`了 和`HttpClient`, 則實例將在`GrpcChannel`釋放時 釋放。 |
| 記錄器工廠 | `null` | 用戶端`LoggerFactory`用於記錄有關 gRPC 調用的資訊。 `LoggerFactory`實例可以從依賴項注入解析或使用`LoggerFactory.Create`創建。 有關設定紀錄紀錄的範例,請參閱<xref:grpc/diagnostics#grpc-client-logging>。 |
| 最大訊息大小 | `null` | 可以從用戶端發送的最大消息大小(以位元組為單位)。 嘗試發送超過配置的最大消息大小的消息會導致異常。 |
| <span style="word-break:normal;word-wrap:normal">最大接收訊息大小</span> | 4 MB | 用戶端可以接收的最大消息大小(以位元組為單位)。 如果用戶端收到的消息超過此限制,它將引發異常。 增加此值允許用戶端接收較大的消息,但可能會對記憶體消耗產生負面影響。 |
| 認證 | `null` | `ChannelCredentials` 執行個體。 憑據用於向 gRPC 調用添加身份驗證中繼資料。 |
| 壓縮提供者 | gzip | 用於壓縮和解壓縮消息的壓縮提供程式的集合。 可以創建自定義壓縮提供程式並將其添加到集合中。 默認配置的提供者支援**gzip**壓縮。 |

下列程式碼：

* 設置通道上的最大發送和接收消息大小。
* 創建用戶端。

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>其他資源

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:grpc/diagnostics>
* <xref:tutorials/grpc/grpc-start>
