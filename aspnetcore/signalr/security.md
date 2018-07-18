---
title: ASP.NET Core SignalR 中的安全性考量
author: tdykstra
description: 了解如何在 ASP.NET Core SignalR 使用驗證和授權。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: b66c7fbfbaee4c70a68f3132875fbc81018c3e20
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095128"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>ASP.NET Core SignalR 中的安全性考量

藉由[Andrew Stanton-nurse](https://twitter.com/anurse)

## <a name="overview"></a>總覽

SignalR 預設提供安全性保護的數的字。 請務必了解如何設定這些保護設定。

### <a name="cross-origin-resource-sharing"></a>跨原始資源共用

[跨原始資源共用 (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)可用來允許跨原始來源 SignalR 連線的瀏覽器中。 如果您的 JavaScript 程式碼裝載在不同的網域名稱從 SignalR 應用程式，您必須啟用[ASP.NET Core CORS 中介軟體](xref:security/cors)才能允許連線。 一般情況下，允許跨原始來源要求，只能從您所控制的網域。 例如，如果您的網站裝載於 `http://www.example.com` 和您的 SignalR 應用程式裝載於 `http://signalr.example.com`，您應該在您的 SignalR 應用程式，只允許來源中設定 CORS `www.example.com`。

如需有關如何設定 CORS 的詳細資訊，請參閱 < [ASP.NET Core CORS 的相關文件](xref:security/cors)。 SignalR 需要下列的 CORS 原則，才能正確運作：

* 原則必須允許特定的原始來源，您預期，或允許任何來源 （不建議）。
* HTTP 方法`GET`和`POST`必須允許。
* 必須啟用認證，即使您未使用驗證。

例如，下列的 CORS 原則允許 SignalR 瀏覽器的用戶端，裝載於 `http://example.com` 存取 SignalR 應用程式：

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> SignalR 與不相容的內建的 CORS 功能在 Azure App Service 中。

### <a name="access-token-logging"></a>存取權杖的記錄

使用 WebSockets 或 Server-Sent 事件時，瀏覽器用戶端會查詢字串中傳送存取權杖。 這是使用標準通常一樣安全`Authorization`標頭，不過許多網頁伺服器會記錄每個要求的 URL，包括查詢字串。 這表示記錄檔中，可能包含存取權杖。 請考慮檢閱 網頁伺服器的記錄設定，以避免記錄這項資訊。

### <a name="exceptions"></a>例外狀況

例外狀況訊息通常會被認為不應該洩漏給用戶端的敏感性資料。 根據預設，SignalR 不會傳送至用戶端中樞方法擲回的例外狀況詳細資料。 相反地，用戶端會收到一般的訊息，指出發生錯誤。 您可以藉由設定覆寫這個行為[ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options)設定。

### <a name="buffer-management"></a>緩衝區管理

SignalR 使用每個連線的緩衝區，以管理內送和外寄訊息。 根據預設，SignalR 會限制為 32 KB 的緩衝區。 這表示用戶端或伺服器可以傳送最大可能的訊息為 32 KB。 這也表示的訊息的連線所耗用的記憶體數量上限為 32 KB。 如果您知道您的訊息都小於此限制，您可以減少此大小，以防止用戶端可以傳送較大的訊息，並強制伺服器配置的記憶體來接受它。 同樣地，如果您知道您的訊息大小超過此限制，您可以增加它。 不過，請注意，增加此限制表示用戶端可導致伺服器配置額外的記憶體，而且可能會降低您的應用程式可以處理的並行連線數目。

內送和外寄訊息的個別限制，都可以在設定成[ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options)物件中設定`MapHub`:

* `ApplicationMaxBufferSize` 表示從用戶端的最大位元組數，伺服器緩衝區。 如果用戶端會嘗試傳送訊息的大小超過此限制，可能會關閉連線。
* `TransportMaxBufferSize` 代表伺服器可以傳送的位元組的數目上限。 如果伺服器會嘗試傳送訊息 （包括從中樞方法的傳回值） 大於此限制，將會擲回例外狀況。

若要設定的限制`0`完全停用限制。 不過，這應該格外小心。 移除此限制可讓用戶端傳送任何大小的訊息。 這可由惡意用戶端會造成過多的記憶體配置，因而大幅減少您的應用程式可支援的並行連線數目。
