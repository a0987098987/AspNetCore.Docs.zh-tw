---
title: ASP.NET Core SignalR Java 用戶端
author: mikaelm12
description: 了解如何使用 ASP.NET Core SignalR Java 用戶端。
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 06/27/2019
uid: signalr/java-client
ms.openlocfilehash: eea1dfb7d8afcd34c0dacd8315ad196d7235c9f7
ms.sourcegitcommit: 6d9cf728465cdb0de1037633a8b7df9a8989cccb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2019
ms.locfileid: "67463277"
---
# <a name="aspnet-core-signalr-java-client"></a>ASP.NET Core SignalR Java 用戶端

藉由[Mikael 馬力](https://twitter.com/MikaelM_12)

Java 用戶端可讓您從 Java 程式碼，包括 Android 應用程式連接至 ASP.NET Core SignalR 伺服器。 像是[JavaScript 用戶端](xref:signalr/javascript-client)並[.NET 用戶端](xref:signalr/dotnet-client)，Java 用戶端可讓您接收和傳送訊息至即時中樞。 用於 ASP.NET Core 2.2 和更新版本的 Java 用戶端。

此文章中參照的範例 Java 主控台應用程式會使用 SignalR Java 用戶端。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>SignalR Java 用戶端封裝安裝

*Signalr 1.0.0* JAR 檔案可讓用戶端連線到 SignalR 中樞。 若要尋找最新的 JAR 檔案版本號碼，請參閱[Maven 搜尋結果](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr)。

如果使用 Gradle，加入下列這一行加入`dependencies`一節您*build.gradle*檔案：

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

如果使用 Maven，新增下列幾行內`<dependencies>`項目您*pom.xml*檔案：

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>連線至中樞

若要建立`HubConnection`，則`HubConnectionBuilder`應該使用。 建立連接時，您可以設定中樞 URL 和記錄層級。 設定任何所需的選項，藉由呼叫任一`HubConnectionBuilder`方法之前`build`。 啟動與連線`start`。

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中樞方法

呼叫`send`叫用中樞方法。 將中樞方法的名稱和任何定義於中樞方法的引數傳遞`send`。

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> 如果您使用 Azure SignalR 服務中*無伺服器模式*，您無法從用戶端呼叫中樞方法。 如需詳細資訊，請參閱 < [SignalR 服務文件](/azure/azure-signalr/signalr-concept-serverless-development-config)。

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

## <a name="android-development-notes"></a>Android 開發附註

與 SignalR 用戶端功能的 Android SDK 相容性，請考慮下列項目，指定您的目標 Android SDK 版本時：

* SignalR Java 用戶端會執行 Android API 層級 16 及更新版本。
* Azure SignalR 服務透過連接 Android API 層級 20 和更新版本需要因為[Azure SignalR 服務](/azure/azure-signalr/signalr-overview)要求 TLS 1.2，且不支援 SHA-1 為基底的加密套件。 Android[新增支援 SHA-256 （和更新版本） 的加密套件](https://developer.android.com/reference/javax/net/ssl/SSLSocket)在 API 層級 20。

## <a name="configure-bearer-token-authentication"></a>設定持有人權杖驗證

SignalR Java 用戶端，在您可以設定要用於驗證所提供的 「 存取權杖 factory 「 持有人權杖來[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)。 使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJava](https://github.com/ReactiveX/RxJava) [單一\<字串 >](http://reactivex.io/documentation/single.html)。 藉由呼叫[Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您可以撰寫邏輯，以針對您的用戶端產生存取權杖。

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a>已知限制

::: moniker range=">= aspnetcore-3.0"

* 支援 JSON 通訊協定。
* 不支援傳輸後援和伺服器傳送事件傳輸。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* 支援 JSON 通訊協定。
* 支援 Websocket 傳輸。
* 資料流尚未支援。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [Java API 參考](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* [Azure SignalR 服務的無伺服器文件](/azure/azure-signalr/signalr-concept-serverless-development-config)
