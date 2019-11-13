---
title: ASP.NET Core SignalR JAVA 用戶端
author: mikaelm12
description: 瞭解如何使用 ASP.NET Core SignalR JAVA 用戶端。
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/java-client
ms.openlocfilehash: d7143b2c22ecdc4e68f484aa4c244e1c520beae0
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963788"
---
# <a name="aspnet-core-opno-locsignalr-java-client"></a>ASP.NET Core SignalR JAVA 用戶端

依[Mikael Mengistu](https://twitter.com/MikaelM_12)

JAVA 用戶端可讓您從 JAVA 程式碼連接到 ASP.NET Core 的 SignalR 伺服器，包括 Android 應用程式。 就像[JavaScript 用戶端](xref:signalr/javascript-client)和[.net 客戶](xref:signalr/dotnet-client)端一樣，JAVA 用戶端可讓您即時接收和傳送訊息至中樞。 JAVA 用戶端會在 ASP.NET Core 2.2 和更新版本中提供。

本文中所參考的 JAVA 主控台應用程式範例會使用 SignalR JAVA 用戶端。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="install-the-opno-locsignalr-java-client-package"></a>安裝 SignalR JAVA 用戶端套件

*Signalr-1.0.0* JAR 檔案可讓用戶端連接到 SignalR 中樞。 若要尋找最新的 JAR 檔案版本號碼，請參閱[Maven 搜尋結果](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr)。

如果使用 Gradle，請將下面這一行新增至*Gradle*檔案的 `dependencies` 區段：

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

如果使用 Maven，請在*pom*檔案的 `<dependencies>` 元素內新增下列幾行：

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>連接到中樞

若要建立 `HubConnection`，請使用 `HubConnectionBuilder`。 建立連接時，可以設定中樞 URL 和記錄層級。 設定任何必要的選項，方法是在 `build`之前呼叫任何 `HubConnectionBuilder` 方法。 開始與 `start`的連接。

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中樞方法

`send` 的呼叫會叫用中樞方法。 傳遞中樞方法名稱和中樞方法中所定義的任何引數，以 `send`。

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> 如果您使用*無伺服器模式*的 Azure SignalR 服務，則無法從用戶端呼叫中樞方法。 如需詳細資訊，請參閱[SignalR 服務檔](/azure/azure-signalr/signalr-concept-serverless-development-config)。

## <a name="call-client-methods-from-hub"></a>從中樞呼叫用戶端方法

使用 `hubConnection.on` 定義中樞可以呼叫之用戶端上的方法。 在建立之後，但在開始連接之前，定義方法。

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>新增記錄

SignalR JAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。 它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。 下列程式碼片段示範如何使用 `java.util.logging` 搭配 SignalR JAVA 用戶端。

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

可以放心地忽略這種情況。

## <a name="android-development-notes"></a>Android 開發注意事項

在 Android SDK SignalR 用戶端功能的相容性方面，指定目標 Android SDK 版本時，請考慮下列專案：

* SignalR JAVA 用戶端會在 Android API 層級16和更新版本上執行。
* 透過 Azure SignalR 服務連線將需要 Android API 層級20和更新版本，因為[Azure SignalR 服務](/azure/azure-signalr/signalr-overview)需要 TLS 1.2，而且不支援以 sha-1 為基礎的加密套件。 Android 已在 API 層級20中[新增對 SHA-256 （和更新版本）加密套件的支援](https://developer.android.com/reference/javax/net/ssl/SSLSocket)。

## <a name="configure-bearer-token-authentication"></a>設定持有人權杖驗證

在 SignalR JAVA 用戶端中，您可以藉由提供[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)的「存取權杖處理站」，設定要用於驗證的持有人權杖。 使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava) [單一\<字串 >](https://reactivex.io/documentation/single.html)。 只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a>已知限制

::: moniker range=">= aspnetcore-3.0"

* 僅支援 JSON 通訊協定。
* 不支援傳輸回溯和伺服器傳送的事件傳輸。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* 僅支援 JSON 通訊協定。
* 僅支援 Websocket 傳輸。
* 尚不支援串流。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [Java API 參考](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* [Azure SignalR 服務無伺服器檔](/azure/azure-signalr/signalr-concept-serverless-development-config)
