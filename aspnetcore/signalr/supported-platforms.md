---
title: ASP.NET Core SignalR支援的平臺
author: bradygaster
description: 瞭解 ASP.NET Core SignalR支援的平臺。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/supported-platforms
ms.openlocfilehash: 70a05dabb95aaf561aa78d5c8b24b430c51bd973
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82772601"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>ASP.NET Core SignalR 支援的平臺

## <a name="server-system-requirements"></a>伺服器系統需求

SignalR for ASP.NET Core 支援 ASP.NET Core 支援的任何伺服器平臺。

## <a name="javascript-client"></a>JavaScript 用戶端

[JavaScript 用戶端](xref:signalr/javascript-client)會在 NodeJS 8 和更新版本以及下列瀏覽器上執行：

| 瀏覽器                         | 版本         |
| ------------------------------- | --------------- |
| Microsoft Edge                  | 目前&dagger; |
| Mozilla Firefox                 | 目前&dagger; |
| Google Chrome;包含 Android | 目前&dagger; |
| 免費包含 iOS            | 目前&dagger; |
| Microsoft Internet Explorer     | 11              |

&dagger;*Current*指的是最新版本的瀏覽器。

## <a name="net-client"></a>.NET 用戶端

[.Net 用戶端](xref:signalr/dotnet-client)會在 ASP.NET Core 支援的任何平臺上執行。 例如， [xamarin 開發人員可以使用SignalR ](https://github.com/aspnet/Announcements/issues/305)來建立使用 xamarin 的 android 應用程式8.4.0.1 和更新版本，以及使用11.14.0.4 和更新版本的 ios 應用程式。

如果伺服器執行 IIS，Websocket 傳輸需要 Windows Server 2012 或更新版本上的 IIS 8.0 或更新版本。 所有平臺都支援其他傳輸。

## <a name="java-client"></a>Java 用戶端

[JAVA 用戶端](xref:signalr/java-client)支援 java 8 和更新版本。

## <a name="unsupported-clients"></a>不支援的用戶端

下列用戶端可以使用，但為實驗性或非官方。 它們目前不受支援，而且可能永遠不會。

* [C + + 用戶端](https://github.com/aspnet/SignalR-Client-Cpp)

* [Swift 用戶端](https://github.com/moozzyk/SignalR-Client-Swift)
