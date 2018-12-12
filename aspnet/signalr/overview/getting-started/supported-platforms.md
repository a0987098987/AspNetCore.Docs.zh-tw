---
uid: signalr/overview/getting-started/supported-platforms
title: 支援的平台 |Microsoft Docs
author: pfletcher
description: 此文章說明 SignalR 支援哪些用戶端和伺服器。
ms.author: riande
ms.date: 04/18/2018
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: e270f9a328f36854fdfb3e23b78e0b40cdda6411
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287353"
---
<a name="supported-platforms"></a>支援的平台
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> 此文章說明 SignalR 支援哪些用戶端和伺服器。 
> 
> ## <a name="questions-and-comments"></a>提出問題或意見
> 
> 您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。

SignalR 支援各種不同的伺服器和用戶端設定底下。 此外，每個傳輸選項有一組自己的; 的需求如果無法使用的傳輸的系統需求，則 SignalR 也能流暢地容錯移轉至其他傳輸。 如需有關支援 SignalR 的傳輸的詳細資訊，請參閱[傳輸和後援](introduction-to-signalr.md#transports)。

## <a name="server-system-requirements"></a>伺服器系統需求

SignalR 伺服器元件可裝載於各種不同的伺服器設定。 本章節描述支援的作業系統、.NET framework、 Internet Information Server，以及其他元件的版本。

### <a name="supported-server-operating-systems"></a>支援的伺服器作業系統

SignalR 伺服器元件可裝載於下列伺服器或用戶端作業系統。 請注意，signalr 使用 WebSockets，Windows Server 2012、 Windows Server 2016 或 Windows 8 需要 （WebSocket 可以使用 Windows Azure 網站上，只要設定站台的.NET framework 版本為 4.5，而且在網站中啟用 Web 通訊端設定頁面）。

- Windows Server 2016
- Windows Server 2012
- Windows Server 2008 r2
- Windows 10
- Windows 8
- Windows 7
- Microsoft Azure

### <a name="supported-server-net-framework-version"></a>支援的伺服器的.NET Framework 版本

.NET Framework 4.5 上才支援 SignalR 2。 請參閱[建議的更新](#updates)增強可靠性、 相容性、 穩定性和效能的更新 區段。

### <a name="supported-server-iis-versions"></a>支援的伺服器的 IIS 版本

SignalR 裝載在 IIS 中，會支援下列版本。 請注意，是否用戶端作業系統，例如適用於開發 （Windows 8 或 Windows 7 中），完整的 IIS 或 Cassini 版本不應，因為會有 10 個同時連線加諸，因為連線將會非常快速地達到限制暫時性，經常重新建立，而且不會處置立即時無法再使用。 用戶端作業系統上時，應該會使用 IIS Express。

也請注意，signalr 使用 WebSocket，必須使用 IIS 8 或 IIS 8 Express，伺服器必須使用 Windows 8，Windows Server 2012 或更新版本，必須在 IIS 中啟用 WebSocket。 如需如何啟用 WebSocket，在 IIS 中的資訊，請參閱[IIS 8.0 WebSocket 通訊協定支援](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)。

- IIS 8 的 IIS Express。
- IIS 7 和 7.5。 支援[無副檔名 Url](https://support.microsoft.com/kb/980368)需要。
- IIS 必須執行整合式模式，不支援傳統模式。 如果使用 Server-Sent 事件傳輸以傳統模式執行 IIS，可能會遇到最多 30 秒的訊息延遲。
- 裝載的應用程式必須在完全信任模式下執行。

## <a name="client-system-requirements"></a>用戶端系統需求

SignalR 可以用於各種不同的用戶端平台。 本章節描述使用 SignalR 的網頁瀏覽器、 Windows 桌面應用程式、 Silverlight 應用程式和行動裝置的系統需求。

### <a name="web-browsers"></a>網頁瀏覽器

SignalR 可以用各種不同的網頁瀏覽器，但一般而言，只有最新的兩個版本支援。

在瀏覽器中使用 SignalR 的應用程式必須使用 jQuery 版本 1.6.4 或重大更新的版本 （例如 1.7.2、 1.8.2 或 1.9.1）。

SignalR 可以用於下列瀏覽器：

- Microsoft Internet Explorer 版本 8、 9、 10 和 11。 現代化、 桌面和行動裝置版支援。
- Mozilla Firefox： 目前的版本-1，Windows 和 Mac 的版本。
- Google Chrome： 目前的版本-1，Windows 和 Mac 的版本。
- Safari： 目前的版本-1，Mac 和 iOS 版本。
- Opera： 目前的版本-1，只有 Windows。
- Android 的瀏覽器

除了需要某些瀏覽器，SignalR 使用的各種傳輸會有自己的需求。 下列傳輸所支援的下列設定：

<a id="browser"></a>

**網頁瀏覽器傳輸需求**

| Transport | Internet Explorer | Chrome （Windows 或 iOS） | Firefox | Safari （OSX 或 iOS） | Android |
| --- | --- | --- | --- | --- | --- |
| WebSockets | 10+ | 目前-1 | 目前-1 | 目前-1 | N/A |
| 伺服器傳送事件 | N/A | 目前-1 | 目前-1 | 目前-1 | N/A |
| ForeverFrame | 8+ | N/A | N/A | N/A | 4.1 |
| 長輪詢 | 8+ | 目前-1 | 目前-1 | 目前-1 | 4.1 |

\*：6 + 所需的完整功能。

#### <a name="unsupported-browsers"></a>不支援的瀏覽器

雖然 SignalR*可能*執行而不會在較舊的瀏覽器版本的主要問題，我們不要主動測試 SignalR 中和通常不會修正可能會出現在他們的 bug。

### <a name="windows-desktop-and-silverlight-applications"></a>Windows 桌面和 Silverlight 應用程式

除了在網頁瀏覽器中執行，SignalR 可以裝載於獨立的 Windows 用戶端或 Silverlight 應用程式。 Windows 桌面和 Silverlight 的 SignalR 應用程式有下列系統需求。

- 在 Windows XP SP3 或更新版本支援使用.NET 4 的應用程式。
- 在 Windows Vista 或更新版本支援使用.NET Framework 4.5 的應用程式。

除了作業系統和.NET framework 需求，使用 signalr 的傳輸會有自己的需求。 下列傳輸所支援的下列設定：

**Windows 桌面和 Silverlight 傳輸需求**

| Transport | .NET 應用程式 | Silverlight |
| --- | --- | --- |
| Web 通訊端 | Windows 8 及更新版本和.NET 4.5 + | N/A |
| 不限次數的框架 | N/A | N/A |
| 伺服器傳送事件 | .NET 4 + | 5+ |
| 長輪詢 | .NET 4 + | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Windows 市集和 Windows Phone 應用程式

SignalR 可以用於 Windows 市集應用程式和 Windows Phone 8 應用程式。 下列傳輸所支援的下列設定：

**Windows 市集和 Windows Phone 傳輸需求**

| Transport | Windows 市集 /.NET | Windows 市集 / JavaScript | Windows Phone / IE | Windows Phone /.NET |
| --- | --- | --- | --- | --- |
| WebSockets | N/A | Win8 + | 8+ | N/A |
| 不限次數的框架 | N/A | Win8 + | 7.5+ | N/A |
| 伺服器傳送事件 | Win8 + | N/A | N/A | 8+ |
| 長輪詢 | Win8 + | Win8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>建議的更新

SignalR 伺服器建議下列更新：

- 有可用的更新，針對.NET Framework 4.5[此處](https://support.microsoft.com/kb/2750149)。
- Microsoft 會定期發行 ASP.NET Qfe。 這些應該套用為可用。
