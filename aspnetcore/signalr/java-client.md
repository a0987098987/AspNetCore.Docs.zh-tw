---
title: ASP.NET Core SignalR Java 用戶端
author: mikaelm12
description: 了解如何使用 ASP.NET Core SignalR Java 用戶端。
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 10/18/2018
uid: signalr/java-client
ms.openlocfilehash: 3d2cfe5f58cc41741512c68cebbbc90e8f714cba
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148781"
---
# <a name="aspnet-core-signalr-java-client"></a>ASP.NET Core SignalR Java 用戶端

藉由[Mikael 馬力](https://twitter.com/MikaelM_12)

Java 用戶端可讓您從 Java 程式碼，包括 Android 應用程式連接至 ASP.NET Core SignalR 伺服器。 像是[JavaScript 用戶端](xref:signalr/javascript-client)並[.NET 用戶端](xref:signalr/dotnet-client)，Java 用戶端可讓您接收和傳送訊息至即時中樞。 用於 ASP.NET Core 2.2 和更新版本的 Java 用戶端。

此文章中參照的範例 Java 主控台應用程式會使用 SignalR Java 用戶端。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>SignalR Java 用戶端封裝安裝

*Signalr 1.0.0-preview3 35501* JAR 檔案可讓用戶端連線到 SignalR 中樞。 若要尋找最新的 JAR 檔案版本號碼，請參閱[Maven 搜尋結果](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr)。

如果使用 Gradle，加入下列這一行加入`dependencies`一節您*build.gradle*檔案：

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

如果使用 Maven，新增下列幾行內`<dependencies>`項目您*pom.xml*檔案：

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>連線至中樞

若要建立`HubConnection`，則`HubConnectionBuilder`應該使用。 建立連接時，您可以設定中樞 URL 和記錄層級。 設定任何所需的選項，藉由呼叫任一`HubConnectionBuilder`方法之前`build`。 啟動與連線`start`。

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中樞方法

呼叫`send`叫用中樞方法。 將中樞方法的名稱和任何定義於中樞方法的引數傳遞`send`。

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a>用戶端方法呼叫來自中樞

使用`hubConnection.on`中樞可以呼叫用戶端上定義的方法。 在建置之後，但開始連接之前，請定義的方法。

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>新增記錄

SignalR Java 用戶端會使用[SLF4J](https://www.slf4j.org/)記錄的程式庫。 它是高層級的記錄 API，可讓程式庫的使用者選擇他們自己的特定記錄實作，藉由將特定記錄相依性。 下列程式碼片段示範如何使用`java.util.logging`與 SignalR Java 用戶端。

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

如果您未設定登入您的相依性，SLF4J 會載入預設的無作業記錄器，並出現下列警告訊息：

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

這可以放心地忽略。

## <a name="known-limitations"></a>已知的限制

這是預覽版本的 Java 用戶端。 不支援某些功能：

* 支援 JSON 通訊協定。
* 支援 Websocket 傳輸。
* 資料流尚未支援。

## <a name="additional-resources"></a>其他資源

* [Java API 參考](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
