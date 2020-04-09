---
title: ASP.NET核心的 gRPC 中的安全注意事項
author: jamesnk
description: 瞭解 gRPC ASP.NET核心的安全注意事項。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 07/07/2019
uid: grpc/security
ms.openlocfilehash: f84bec0ef485b701b2be36384a2e49b9b28e473d
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667318"
---
# <a name="security-considerations-in-grpc-for-aspnet-core"></a>ASP.NET核心的 gRPC 中的安全注意事項

由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)

本文提供有關使用 .NET Core 保護 gRPC 的資訊。

## <a name="transport-security"></a>傳輸安全性

使用 HTTP/2 發送和接收 gRPC 消息。 我們建議:

* [傳輸層安全 (TLS)](https://tools.ietf.org/html/rfc5246)用於保護生產 gRPC 應用中的消息。
* gRPC 服務應僅偵聽和回應安全埠。

TLS 在 Kestrel 中配置。 有關設定 Kestrel 終結點的詳細資訊,請參閱[Kestrel 終結點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)。

## <a name="exceptions"></a>例外狀況

異常消息通常被視為不應向客戶端顯示的敏感數據。 默認情況下,gRPC不會將 gRPC 服務引發的異常的詳細資訊發送到用戶端。 相反,用戶端會收到一條泛型消息,指示發生錯誤。 例外訊息傳遞到客戶端可以重寫(例如,在開發或測試中)與[啟用詳細錯誤](xref:grpc/configuration#configure-services-options)。 異常消息不應在生產應用中向客戶端公開。

## <a name="message-size-limits"></a>訊息大小限制

傳入到 gRPC 用戶端和服務的消息將載入到記憶體中。 消息大小限制是一種機制,有助於防止 gRPC 消耗過多的資源。

gRPC 使用每封郵件大小限制來管理傳入和傳出消息。 默認情況下,gRPC將傳入消息限制為 4 MB。 傳出郵件沒有限制。

在伺服器上,gRPC 訊息限制可以設定為應用中的所有服務`AddGrpc`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 1 * 1024 * 1024; // 1 MB
        options.MaxSendMessageSize = 1 * 1024 * 1024; // 1 MB
    });
}
```

也可以為使用`AddServiceOptions<TService>`的單個服務配置限制。 有關設定訊息大小限制的詳細資訊,請參考[gRPC 設定](xref:grpc/configuration)。

## <a name="client-certificate-validation"></a>用戶端憑證驗證

建立連線時,最初將驗證[客戶端憑證](https://tools.ietf.org/html/rfc5246#section-7.4.4)。 默認情況下,Kestrel 不執行連接客戶端證書的其他驗證。

我們建議由用戶端證書保護的 gRPC 服務使用[Microsoft.AspNetCore.身份驗證.證書](xref:security/authentication/certauth)包。 ASP.NET核心認證認證將對用戶端憑證執行其他驗證,包括:

* 憑證具有有效的延伸金鑰使用 (EKU)
* 在其有效期內
* 檢查憑證撤銷
