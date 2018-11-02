---
title: ASP.NET Core SignalR 支援的平台
author: tdykstra
description: 了解支援的平台 ASP.NET Core SignalR。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/31/2018
uid: signalr/supported-platforms
ms.openlocfilehash: 773f6c020dbb2982911e177b55855473c750d52a
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758176"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>ASP.NET Core SignalR 支援的平台

## <a name="server-system-requirements"></a>伺服器系統需求

適用於 ASP.NET Core SignalR 支援 ASP.NET Core 支援的任何伺服器平台。

## <a name="javascript-client"></a>JavaScript 用戶端

[JavaScript 用戶端](https://www.npmjs.com/package/@aspnet/signalr)NodeJS 8 和更新版本與下列瀏覽器上執行：

| 瀏覽器                         | 版本 |
| ------------------------------- | ------- |
| Microsoft Edge                  | 目前的 |
| Mozilla Firefox                 | 目前的 |
| Google Chrome;包含 Android | 目前的 |
| Safari;包含 iOS            | 目前的 |
| Microsoft Internet Explorer     | 11      |
 
## <a name="net-client"></a>.NET 用戶端

[.NET 用戶端](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)ASP.NET Core 支援的任何伺服器平台上執行。

如果伺服器是執行 IIS，Websocket 傳輸需要 IIS 8.0 或更新版本的 Windows Server 2012 或更新版本。 所有平台都支援其他傳輸。

## <a name="java-client"></a>Java 用戶端

[Java 用戶端](https://search.maven.org/artifact/com.microsoft.aspnet/signalr)支援 Java 8 和更新版本。

## <a name="unsupported-clients"></a>不支援的用戶端

下列用戶端可用，但會實驗性或非官方。 它們目前不支援，且可能永遠無法。

* [C + + 用戶端](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Swift 的用戶端](https://github.com/moozzyk/SignalR-Client-Swift)
