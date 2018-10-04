---
title: ASP.NET Core SignalR 支援的平台
author: tdykstra
description: 支援的平台為 ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/26/2018
uid: signalr/supported-platforms
ms.openlocfilehash: d6d74a55d35ddb34a6f66a171bfe3f343dd61b63
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577622"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>ASP.NET Core SignalR 支援的平台

## <a name="server-system-requirements"></a>伺服器系統需求

適用於 ASP.NET Core SignalR 支援 ASP.NET Core 支援的任何伺服器平台。

## <a name="javascript-client"></a>JavaScript 用戶端

[JavaScript 用戶端](https://www.npmjs.com/package/@aspnet/signalr)NodeJS 8 和更新版本與下列瀏覽器上執行：

| 瀏覽器 | 版本 |
| ------- | ------- |
| Microsoft Edge | 目前的 |
| Mozilla Firefox | 目前的 |
| Google Chrome;包含 Android | 目前的 |
| Safari;包含 iOS | 目前的 |
| Microsoft Internet Explorer | 11 |
 
## <a name="net-client"></a>.NET 用戶端

[.NET 用戶端](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)ASP.NET Core 支援的任何伺服器平台上執行。

當伺服器是執行 IIS 時，Websocket 傳輸就會需要 IIS 8.0 或更新版本，在 Windows Server 2012 或更新版本。 所有平台都支援其他傳輸。

## <a name="java-client"></a>Java 用戶端

[Java 用戶端](https://search.maven.org/artifact/com.microsoft.aspnet/signalr)支援 Java 8 和更新版本。

## <a name="unsupported-clients"></a>不支援的用戶端

下列用戶端可用，但會實驗性或非官方。 它們目前不支援，並不一定曾經會支援。

* [C + + 用戶端](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Swift 的用戶端](https://github.com/moozzyk/SignalR-Client-Swift)
