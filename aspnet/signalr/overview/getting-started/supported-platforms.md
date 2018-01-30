---
uid: signalr/overview/getting-started/supported-platforms
title: "支援的平台 |Microsoft 文件"
author: pfletcher
description: "本文說明 SignalR 支援哪些用戶端和伺服器。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 1379b9fb638f67896d88d7aa4312d95280ef7318
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
<a name="supported-platforms"></a>支援的平台
====================
由[Patrick Fletcher](https://github.com/pfletcher)

> 本文說明 SignalR 支援哪些用戶端和伺服器。 
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


SignalR 被支援在各種不同的伺服器和用戶端設定。 此外，每個傳輸選項具有一組自己的; 需求如果無法使用的傳輸的系統需求，則 SignalR 依正常程序會容錯移轉至其他傳輸。 如需有關支援 SignalR 的傳輸的詳細資訊，請參閱[傳輸和後援](introduction-to-signalr.md#transports)。

## <a name="server-system-requirements"></a>伺服器系統需求

SignalR 的伺服器元件可裝載於各種不同的伺服器設定。 本章節描述支援的作業系統、.NET framework、 Internet Information Server，以及其他元件的版本。

### <a name="supported-server-operating-systems"></a>支援的伺服器作業系統

SignalR 的伺服器元件可以裝載於下列伺服器或用戶端作業系統。 請注意，適用於 SignalR 使用 WebSockets，Windows Server 2012 或 Windows 8 （WebSocket 可以使用 Windows Azure 網站上，只要設定站台的.NET framework 版本為 4.5，而且站台的組態頁面中已啟用 Web 通訊端）。

- Windows Server 2012
- Windows Server 2008 r2
- Windows 10
- Windows 8
- Windows 7
- Microsoft Azure

### <a name="supported-server-net-framework-version"></a>支援的伺服器.NET Framework 版本

SignalR 2 才支援在.NET Framework 4.5。 請參閱[建議更新](#updates)加強可靠性、 相容性、 穩定性和效能的更新 > 一節。

### <a name="supported-server-iis-versions"></a>支援的伺服器的 IIS 版本

當 SignalR 裝載在 IIS 中時，支援下列版本。 請注意，是否使用用戶端作業系統，例如開發 （Windows 8 或 Windows 7），完整版本的 IIS 或 Cassini 不應，因為會有 10 個同時連線數加諸，這會非常快速地到達後連線的限制暫時性，經常重新建立，而且不會處置立即時不再使用。 應該使用 IIS Express，用戶端作業系統上。

也請注意，SignalR 使用 WebSocket，必須使用 IIS 8 或 IIS 8 Express，伺服器必須使用 Windows 8、 Windows Server 2012 或更新版本，而且必須在 IIS 中啟用 WebSocket。 如需如何啟用 WebSocket，在 IIS 中的資訊，請參閱[IIS 8.0 WebSocket 通訊協定支援](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)。

- IIS 8 IIS Express。
- IIS 7 和 7.5。 支援[無副檔名 Url](https://support.microsoft.com/kb/980368)需要。
- IIS 必須執行整合式模式，不支援傳統模式。 如果 IIS 在使用 Server-Sent 事件傳輸的傳統模式中執行時可能會遇到的最多 30 秒的訊息延遲。
- 裝載的應用程式必須在完全信任模式下執行。

## <a name="client-system-requirements"></a>用戶端系統需求

SignalR 可用各種不同的用戶端平台。 本章節描述使用 SignalR 在網頁瀏覽器、 Windows 桌面應用程式、 Silverlight 應用程式和行動裝置的系統需求。

### <a name="web-browsers"></a>網頁瀏覽器

SignalR 可用於各種不同的網頁瀏覽器，但一般而言，只有最新的兩個版本支援。

使用瀏覽器中的 SignalR 應用程式必須使用 jQuery 版本 1.6.4 或重大更新的版本 （例如 1.7.2、 1.8.2 或 1.9.1）。

SignalR 可以用於下列瀏覽器：

- Microsoft Internet Explorer 版本 8、 9、 10 和 11。 現代化、 桌面和行動裝置的版本支援。
- Mozilla Firefox： 目前版本-1，Windows 和 Mac 的版本。
- Google Chrome： 目前版本-1，Windows 和 Mac 的版本。
- Safari： 目前版本-1，iOS 和 Mac 的版本。
- Opera： 目前版本-1，僅限 Windows。
- Android 瀏覽器

除了需要某些瀏覽器，SignalR 使用各種傳輸會具有自己的需求。 下列傳輸所支援的下列設定：

<a id="browser"></a>

**網頁瀏覽器傳輸需求**

| Transport | Internet Explorer | Chrome （Windows 或 iOS） | Firefox | Safari （OSX 或 iOS） | Android |
| --- | --- | --- | --- | --- | --- |
| WebSockets | 10+ | current-1 | current-1 | current-1 | N/A |
| 伺服器傳送事件 | N/A | current-1 | current-1 | current-1 | N/A |
| ForeverFrame | 8+ | N/A | N/A | N/A | 4.1 |
| 長輪詢 | 8+ | current-1 | current-1 | current-1 | 4.1 |

\*: 6 + 所需的完整功能。

#### <a name="unsupported-browsers"></a>不支援的瀏覽器

雖然 SignalR*可能*舊版瀏覽器中的主要問題未執行，我們不要主動測試 SignalR 的通常不會修正可能會出現在它們的 bug。

### <a name="windows-desktop-and-silverlight-applications"></a>Windows 桌面和 Silverlight 應用程式

除了在網頁瀏覽器中執行，SignalR 可以裝載於獨立的 Windows 用戶端或 Silverlight 應用程式。 Windows 桌面和 Silverlight SignalR 應用程式有下列的系統需求。

- 在 Windows XP SP3 或更新版本支援使用.NET 4 的應用程式。
- 在 Windows Vista 或更新版本支援使用.NET Framework 4.5 的應用程式。

除了作業系統和.NET framework 需求，SignalR 使用的傳輸會有自己的需求。 下列傳輸所支援的下列設定：

**Windows 桌面和 Silverlight 傳輸需求**

| Transport | .NET 應用程式 | Silverlight |
| --- | --- | --- |
| Web 通訊端 | Windows 8 + 和.NET 4.5 + | N/A |
| 永久框架 | N/A | N/A |
| 伺服器傳送事件 | .NET 4+ | 5+ |
| 長輪詢 | .NET 4+ | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Windows 市集和 Windows Phone 應用程式

SignalR 可以用於 Windows 市集應用程式和 Windows Phone 8 應用程式。 下列傳輸所支援的下列設定：

**Windows 市集和 Windows Phone 傳輸需求**

| Transport | Windows 市集 /.NET | Windows 市集 / JavaScript | Windows Phone/ IE | Windows Phone/ .NET |
| --- | --- | --- | --- | --- |
| WebSockets | N/A | Win8+ | 8+ | N/A |
| 永久框架 | N/A | Win8+ | 7.5+ | N/A |
| 伺服器傳送事件 | Win8+ | N/A | N/A | 8+ |
| 長輪詢 | Win8+ | Win8+ | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>建議的更新

對於 SignalR 伺服器建議下列更新：

- 可更新的.NET Framework 4.5 功能[這裡](https://support.microsoft.com/kb/2750149)。
- Microsoft asp.net，會定期發行 Qfe。 這些應該套用為可用。
