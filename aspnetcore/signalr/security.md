---
title: ASP.NET Core SignalR 中的安全性考量
author: tdykstra
description: 了解如何在 ASP.NET Core SignalR 使用驗證和授權。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 10/17/2018
uid: signalr/security
ms.openlocfilehash: be1dd24c40327d9a0d8f91bf75300128d3d52725
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225365"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>ASP.NET Core SignalR 中的安全性考量

藉由[Andrew Stanton-nurse](https://twitter.com/anurse)

這篇文章會提供資訊保護 SignalR。

## <a name="cross-origin-resource-sharing"></a>跨原始資源共用

[跨原始資源共用 (CORS)](https://www.w3.org/TR/cors/)可用來允許跨原始來源 SignalR 連線的瀏覽器中。 如果 JavaScript 程式碼裝載在不同的網域從 SignalR 應用程式[CORS 中介軟體](xref:security/cors)必須允許 JavaScript 才可連線到 SignalR 應用程式啟用。 允許跨原始來源要求，只能從您信任的網域或控制項。 例如: 

* 您的網站裝載於 `http://www.example.com`
* 您的 SignalR 應用程式裝載於 `http://signalr.example.com`

應該只允許原點的 SignalR 應用程式中設定 CORS `www.example.com`。

如需有關如何設定 CORS 的詳細資訊，請參閱 <<c0> [ 啟用跨源要求 (CORS)](xref:security/cors)。 SignalR**需要**下列的 CORS 原則：

* 允許特定的預期的來源。 允許任何來源可以但**不**安全或建議。
* HTTP 方法`GET`和`POST`必須允許。
* 必須啟用認證，即使在不使用驗證。

例如，下列的 CORS 原則允許 SignalR 瀏覽器的用戶端，裝載於`http://example.com`存取 SignalR 應用程式裝載於`http://signalr.example.com`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> SignalR 與不相容的內建的 CORS 功能在 Azure App Service 中。

## <a name="websocket-origin-restriction"></a>WebSocket 原始限制

::: moniker range=">= aspnetcore-2.2"

Websocket 不適用於 CORS 所提供的保護。 如原始 WebSockets 限制，請參閱[WebSockets 原始限制](xref:fundamentals/websockets#websocket-origin-restriction)。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Websocket 不適用於 CORS 所提供的保護。 瀏覽器執行**不**:

* 執行 CORS 事前要求。
* 採用指定的限制`Access-Control`進行 WebSocket 要求的標頭。

不過，請勿傳送瀏覽器`Origin`時發出 WebSocket 要求的標頭。 應用程式應該設定為驗證這些標頭，以確保允許來自預期的原始來源的 Websocket。

在 ASP.NET Core 2.1 和更新版本，標頭驗證，可透過自訂的中介軟體放**之前`UseSignalR`，並驗證中介軟體**在`Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> `Origin`標頭控制用戶端和 like`Referer`標頭，都可能假冒。 這些標頭應該**不**用作為驗證機制。

::: moniker-end

## <a name="access-token-logging"></a>存取權杖的記錄

使用 WebSockets 或 Server-Sent 事件時，瀏覽器用戶端會查詢字串中傳送存取權杖。 接收存取權杖，透過查詢字串是使用標準通常一樣安全`Authorization`標頭。 不過，許多網頁伺服器會記錄每個要求，包括查詢字串的 URL。 記錄的 Url 可能會記錄的存取權杖。 最佳做法是設定 web 伺服器的記錄設定，以避免記錄的存取權杖。

## <a name="exceptions"></a>例外狀況

例外狀況訊息通常會被認為不應該洩漏給用戶端的敏感性資料。 根據預設，SignalR 不會傳送至用戶端中樞方法擲回的例外狀況詳細資料。 相反地，用戶端會收到一般的訊息，指出發生錯誤。 例外狀況訊息傳遞至用戶端可以覆寫 （例如在開發或測試） 與[ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options)。 例外狀況訊息不應該在生產環境應用程式中的用戶端上公開。

## <a name="buffer-management"></a>緩衝區管理

SignalR 使用來管理內送和外寄訊息的每個連線的緩衝區。 根據預設，SignalR 會限制為 32 KB 的緩衝區。 用戶端或伺服器可以傳送的最大訊息為 32 KB。 訊息的連線所耗用的記憶體上限為 32 KB。 如果您的訊息永遠小於 32 KB，您可以減少此限制，其中：

* 用戶端可防止傳送較大的訊息。
* 伺服器會永遠不需要配置大型緩衝區來接受訊息。

如果您的訊息大於 32 KB，您可以提高限制。 增加此限制表示：

* 用戶端可能會導致伺服器配置大量記憶體緩衝區。
* Server 配置大型緩衝區可能會降低並行連線數目。

有內送和外寄訊息的限制，都可以在設定成[ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options)物件中設定`MapHub`:

* `ApplicationMaxBufferSize` 表示從用戶端的最大位元組數，伺服器緩衝區。 如果用戶端會嘗試傳送訊息的大小超過此限制，可能會關閉連線。
* `TransportMaxBufferSize` 代表伺服器可以傳送的位元組的數目上限。 如果伺服器會嘗試傳送訊息 （從中樞方法的包括傳回值） 的大小超過此限制，將會擲回例外狀況。

若要設定的限制`0`停用限制。 移除此限制可讓用戶端傳送任何大小的訊息。 惡意用戶端傳送大型訊息可能會導致過多的記憶體配置。 過多的記憶體使用量可以大幅減少並行連線數目。
