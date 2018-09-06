---
title: ASP.NET Core SignalR Java 用戶端
author: mikaelm12
description: 了解如何使用 ASP.NET Core SignalR Java 用戶端。
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: f110f5391ac34f5cb4a72f64c16d86c8a37369a2
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2018
ms.locfileid: "43995410"
---
# <a name="aspnet-core-signalr-java-client"></a>ASP.NET Core SignalR Java 用戶端

藉由[Mikael 馬力](https://twitter.com/MikaelM_12)

Java 用戶端可讓您從 Java 程式碼，包括 Android 應用程式連接至 ASP.NET Core SignalR 伺服器。 像是[JavaScript 用戶端](xref:signalr/javascript-client)並[.NET 用戶端](xref:signalr/dotnet-client)，Java 用戶端可讓您接收和傳送訊息至即時中樞。 用於 ASP.NET Core 2.2 和更新版本的 Java 用戶端。

此文章中參照的範例 Java 主控台應用程式會使用 SignalR Java 用戶端。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>SignalR Java 用戶端封裝安裝

*Signalr 0.1.0-preview 1 35029* JAR 檔案可讓用戶端連線到 SignalR 中樞。 若要尋找最新的 JAR 檔案版本號碼，請參閱[Maven 搜尋結果](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav)。

如果使用 Gradle，加入下列這一行加入`dependencies`一節您*build.gradle*檔案：

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview1-35029'
```

如果使用 Maven，新增下列幾行內`<dependencies>`項目您*pom.xml*檔案：

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>連線至中樞

若要建立`HubConnection`，則`HubConnectionBuilder`應該使用。 建立連接時，您可以設定中樞 URL 和記錄層級。 設定任何所需的選項，藉由呼叫任一`HubConnectionBuilder`方法之前`build`。 啟動與連線`start`。

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中樞方法

呼叫`send`叫用中樞方法。 將中樞方法的名稱和任何定義於中樞方法的引數傳遞`send`。

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a>用戶端方法呼叫來自中樞

使用`hubConnection.on`中樞可以呼叫用戶端上定義的方法。 在建置之後，但開始連接之前，請定義的方法。

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a>已知的限制

這是早期預覽版本的 Java 用戶端。 有許多尚不支援的功能。 下列的間距是正在進行未來的版本：

* 只有基本類型可接受的參數和傳回型別。
* Api 均為同步。
* 目前支援僅 「 傳送 」 呼叫類型。 不支援 「 叫用 」，並傳回值的資料流。
* 目前不支援用戶端[Azure SignalR 服務](/azure/azure-signalr/)。
* 支援 JSON 通訊協定。
* 支援 Websocket 傳輸。

## <a name="additional-resources"></a>其他資源

* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
