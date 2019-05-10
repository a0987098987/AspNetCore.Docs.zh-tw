---
title: ASP.NET Core 組態 gRPC
author: jamesnk
description: 了解如何設定 gRPC 的 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 66dfb9ec136616f10c1b7aaad766e18813b87de4
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087333"
---
# <a name="grpc-for-aspnet-core-configuration"></a>ASP.NET Core 組態 gRPC

## <a name="configure-services-options"></a>設定服務選項

下表說明設定 gRPC 服務的選項：

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | 最大訊息大小可以從伺服器傳送的位元組數。 嘗試傳送的訊息長度超過設定的最大訊息大小會導致例外狀況。 |
| `ReceiveMaxMessageSize` | 4 MB | 最大訊息大小可以由伺服器接收的位元組數。 如果伺服器收到的訊息超過此限制，它會擲回例外狀況。 增加此值可讓伺服器接收較大的訊息，但可能會造成負面影響記憶體耗用量。 |
| `EnableDetailedErrors` | `false` | 如果`true`詳細服務方法擲回例外狀況時，將會傳回給用戶端的例外狀況訊息。 預設為 `false`。 這個設定設為`true`可能會導致洩漏機密資訊。 |
| `CompressionProviders` | gzip | 壓縮來壓縮和解壓縮訊息使用的提供者集合。 可以建立自訂壓縮提供者，並加入至集合。 預設值設定提供者會支援**gzip**壓縮。 |
| `ResponseCompressionAlgorithm` | `null` | 壓縮演算法，用來壓縮從伺服器傳送的訊息。 Algorithm 必須符合壓縮提供者中的`CompressionProviders`。 壓縮回應 algorthm 用戶端必須指出它傳送給它，以支援的演算法**grpc-接受編碼**標頭。 |
| `ResponseCompressionLevel` | `null` | 會壓縮從伺服器傳送的訊息所使用的壓縮層級。 |

可以為所有服務設定選項，藉由提供選項委派`AddGrpc`呼叫中`Startup.ConfigureServices`。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

單一服務的選項會覆寫中提供的全域選項`AddGrpc`，並可使用設定`AddServiceOptions<TService>`:

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="configure-kestrel-options"></a>設定 Kestrel 選項

Kestrel 伺服器的組態選項，asp.net 會影響 gRPC 的行為。

### <a name="request-body-data-rate-limit"></a>要求主體資料速率限制

根據預設，Kestrel 伺服器加諸[要求主體資料速率下限](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>)。 用戶端串流處理和串流處理呼叫的雙工，可能不符合此速率，並連接可能會逾時。最小要求內文資料流的用戶端和資料流處理呼叫的雙工，包括 gRPC 服務時，必須停用資料速率限制：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
         Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate = null;
        });
    });
}
```

## <a name="additional-resources"></a>其他資源

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
