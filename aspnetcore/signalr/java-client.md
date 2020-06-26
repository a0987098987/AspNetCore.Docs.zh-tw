---
title: ASP.NET Core SignalR JAVA 用戶端
author: mikaelm12
description: 瞭解如何使用 ASP.NET Core 的 SignalR JAVA 用戶端。
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/java-client
ms.openlocfilehash: 27ab8cc1b6e419b59aadb97a8a1fbdddc3579276
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408795"
---
# <a name="aspnet-core-signalr-java-client"></a>ASP.NET Core SignalR JAVA 用戶端

依[Mikael Mengistu](https://twitter.com/MikaelM_12)

JAVA 用戶端可讓您 SignalR 從 java 程式碼（包括 Android 應用程式）連接到 ASP.NET Core 伺服器。 就像[JavaScript 用戶端](xref:signalr/javascript-client)和[.net 客戶](xref:signalr/dotnet-client)端一樣，JAVA 用戶端可讓您即時接收和傳送訊息至中樞。 JAVA 用戶端會在 ASP.NET Core 2.2 和更新版本中提供。

本文中所參考的範例 JAVA 主控台應用程式會使用 SignalR JAVA 用戶端。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="install-the-signalr-java-client-package"></a>安裝 SignalR JAVA 用戶端套件

*Signalr-1.0.0* JAR 檔案可讓用戶端連線到 SignalR 中樞。 若要尋找最新的 JAR 檔案版本號碼，請參閱[Maven 搜尋結果](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr)。

如果使用 Gradle，請在 Gradle 檔案的區段中新增下列 `dependencies` 程式程式碼： *build.gradle*

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

如果使用 Maven，請在pom.xml檔案的元素內新增下列幾行 `<dependencies>` ： *pom.xml*

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>連接到中樞

若要建立 `HubConnection` ， `HubConnectionBuilder` 應該使用。 建立連接時，可以設定中樞 URL 和記錄層級。 在之前呼叫任何方法，以設定任何必要的選項 `HubConnectionBuilder` `build` 。 開始與的連接 `start` 。

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中樞方法

呼叫會叫 `send` 用中樞方法。 將中樞方法名稱和 hub 方法中定義的任何引數傳遞至 `send` 。

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> 如果您是 SignalR 在*無伺服器模式*中使用 Azure 服務，則無法從用戶端呼叫中樞方法。 如需詳細資訊，請參閱[ SignalR 服務檔](/azure/azure-signalr/signalr-concept-serverless-development-config)。

## <a name="call-client-methods-from-hub"></a>從中樞呼叫用戶端方法

使用 `hubConnection.on` 定義中樞可以呼叫之用戶端上的方法。 在建立之後，但在開始連接之前，定義方法。

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>新增記錄

SignalRJAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。 它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。 下列程式碼片段顯示如何搭配使用 `java.util.logging` 與 SignalR JAVA 用戶端。

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

在 Android SDK 用戶端功能的相容性方面 SignalR ，指定目標 Android SDK 版本時，請考慮下列專案：

* SignalRJAVA 用戶端會在 ANDROID API 層級16和更新版本上執行。
* 透過 Azure 服務連線 SignalR 會需要 ANDROID API 層級20和更新版本，因為[Azure SignalR 服務](/azure/azure-signalr/signalr-overview)需要 TLS 1.2，而且不支援以 sha-1 為基礎的加密套件。 Android 已在 API 層級20中[新增對 SHA-256 （和更新版本）加密套件的支援](https://developer.android.com/reference/javax/net/ssl/SSLSocket)。

## <a name="configure-bearer-token-authentication"></a>設定持有人權杖驗證

在 SignalR JAVA 用戶端中，您可以藉由提供[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)的「存取權杖處理站」，設定要用於驗證的持有人權杖。 使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[單一 \<String> ](https://reactivex.io/documentation/single.html) [RxJAVA](https://github.com/ReactiveX/RxJava) 。 只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。

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
