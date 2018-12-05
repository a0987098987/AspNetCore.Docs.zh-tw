---
title: ASP.NET Core SignalR Java 用戶端
author: mikaelm12
description: 了解如何使用 ASP.NET Core SignalR Java 用戶端。
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 11/07/2018
uid: signalr/java-client
ms.openlocfilehash: d0eff38c1f622b896ed1dc3002238aec7b6bfd38
ms.sourcegitcommit: 8a65f6c2cbe290fb2418eed58f60fb74c95392c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "52892090"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="f6da6-103">ASP.NET Core SignalR Java 用戶端</span><span class="sxs-lookup"><span data-stu-id="f6da6-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="f6da6-104">藉由[Mikael 馬力](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="f6da6-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="f6da6-105">Java 用戶端可讓您從 Java 程式碼，包括 Android 應用程式連接至 ASP.NET Core SignalR 伺服器。</span><span class="sxs-lookup"><span data-stu-id="f6da6-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="f6da6-106">像是[JavaScript 用戶端](xref:signalr/javascript-client)並[.NET 用戶端](xref:signalr/dotnet-client)，Java 用戶端可讓您接收和傳送訊息至即時中樞。</span><span class="sxs-lookup"><span data-stu-id="f6da6-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="f6da6-107">用於 ASP.NET Core 2.2 和更新版本的 Java 用戶端。</span><span class="sxs-lookup"><span data-stu-id="f6da6-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="f6da6-108">此文章中參照的範例 Java 主控台應用程式會使用 SignalR Java 用戶端。</span><span class="sxs-lookup"><span data-stu-id="f6da6-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="f6da6-109">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f6da6-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="f6da6-110">SignalR Java 用戶端封裝安裝</span><span class="sxs-lookup"><span data-stu-id="f6da6-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="f6da6-111">*Signalr 1.0.0* JAR 檔案可讓用戶端連線到 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="f6da6-111">The *signalr-1.0.0* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="f6da6-112">若要尋找最新的 JAR 檔案版本號碼，請參閱[Maven 搜尋結果](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr)。</span><span class="sxs-lookup"><span data-stu-id="f6da6-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="f6da6-113">如果使用 Gradle，加入下列這一行加入`dependencies`一節您*build.gradle*檔案：</span><span class="sxs-lookup"><span data-stu-id="f6da6-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

<span data-ttu-id="f6da6-114">如果使用 Maven，新增下列幾行內`<dependencies>`項目您*pom.xml*檔案：</span><span class="sxs-lookup"><span data-stu-id="f6da6-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="f6da6-115">連線至中樞</span><span class="sxs-lookup"><span data-stu-id="f6da6-115">Connect to a hub</span></span>

<span data-ttu-id="f6da6-116">若要建立`HubConnection`，則`HubConnectionBuilder`應該使用。</span><span class="sxs-lookup"><span data-stu-id="f6da6-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="f6da6-117">建立連接時，您可以設定中樞 URL 和記錄層級。</span><span class="sxs-lookup"><span data-stu-id="f6da6-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="f6da6-118">設定任何所需的選項，藉由呼叫任一`HubConnectionBuilder`方法之前`build`。</span><span class="sxs-lookup"><span data-stu-id="f6da6-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="f6da6-119">啟動與連線`start`。</span><span class="sxs-lookup"><span data-stu-id="f6da6-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="f6da6-120">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="f6da6-120">Call hub methods from client</span></span>

<span data-ttu-id="f6da6-121">呼叫`send`叫用中樞方法。</span><span class="sxs-lookup"><span data-stu-id="f6da6-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="f6da6-122">將中樞方法的名稱和任何定義於中樞方法的引數傳遞`send`。</span><span class="sxs-lookup"><span data-stu-id="f6da6-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="f6da6-123">用戶端方法呼叫來自中樞</span><span class="sxs-lookup"><span data-stu-id="f6da6-123">Call client methods from hub</span></span>

<span data-ttu-id="f6da6-124">使用`hubConnection.on`中樞可以呼叫用戶端上定義的方法。</span><span class="sxs-lookup"><span data-stu-id="f6da6-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="f6da6-125">在建置之後，但開始連接之前，請定義的方法。</span><span class="sxs-lookup"><span data-stu-id="f6da6-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="f6da6-126">新增記錄</span><span class="sxs-lookup"><span data-stu-id="f6da6-126">Add logging</span></span>

<span data-ttu-id="f6da6-127">SignalR Java 用戶端會使用[SLF4J](https://www.slf4j.org/)記錄的程式庫。</span><span class="sxs-lookup"><span data-stu-id="f6da6-127">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="f6da6-128">它是高層級的記錄 API，可讓程式庫的使用者選擇他們自己的特定記錄實作，藉由將特定記錄相依性。</span><span class="sxs-lookup"><span data-stu-id="f6da6-128">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="f6da6-129">下列程式碼片段示範如何使用`java.util.logging`與 SignalR Java 用戶端。</span><span class="sxs-lookup"><span data-stu-id="f6da6-129">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="f6da6-130">如果您未設定登入您的相依性，SLF4J 會載入預設的無作業記錄器，並出現下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="f6da6-130">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="f6da6-131">這可以放心地忽略。</span><span class="sxs-lookup"><span data-stu-id="f6da6-131">This can safely be ignored.</span></span>

## <a name="android-development-notes"></a><span data-ttu-id="f6da6-132">Android 開發附註</span><span class="sxs-lookup"><span data-stu-id="f6da6-132">Android development notes</span></span>

<span data-ttu-id="f6da6-133">與 SignalR 用戶端功能的 Android SDK 相容性，請考慮下列項目，指定您的目標 Android SDK 版本時：</span><span class="sxs-lookup"><span data-stu-id="f6da6-133">With regards to Android SDK compatibility for the SignalR client features, consider the following items when specifying your target Android SDK version:</span></span>

* <span data-ttu-id="f6da6-134">SignalR Java 用戶端會執行 Android API 層級 16 及更新版本。</span><span class="sxs-lookup"><span data-stu-id="f6da6-134">The SignalR Java Client will run on Android API Level 16 and later.</span></span>
* <span data-ttu-id="f6da6-135">Azure SignalR 服務透過連接 Android API 層級 20 和更新版本需要因為[Azure SignalR 服務](/azure/azure-signalr/signalr-overview)要求 TLS 1.2，且不支援 SHA-1 為基底的加密套件。</span><span class="sxs-lookup"><span data-stu-id="f6da6-135">Connecting through the Azure SignalR Service will require Android API Level 20 and later because the [Azure SignalR Service](/azure/azure-signalr/signalr-overview) requires TLS 1.2 and doesn't support SHA-1-based cipher suites.</span></span> <span data-ttu-id="f6da6-136">Android[新增支援 SHA-256 （和更新版本） 的加密套件](https://developer.android.com/reference/javax/net/ssl/SSLSocket)在 API 層級 20。</span><span class="sxs-lookup"><span data-stu-id="f6da6-136">Android [added support for SHA-256 (and above) cipher suites](https://developer.android.com/reference/javax/net/ssl/SSLSocket) in API Level 20.</span></span>

## <a name="configure-bearer-token-authentication"></a><span data-ttu-id="f6da6-137">設定持有人權杖驗證</span><span class="sxs-lookup"><span data-stu-id="f6da6-137">Configure bearer token authentication</span></span>

<span data-ttu-id="f6da6-138">SignalR Java 用戶端，在您可以設定要用於驗證所提供的 「 存取權杖 factory 「 持有人權杖來[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)。</span><span class="sxs-lookup"><span data-stu-id="f6da6-138">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an "access token factory" to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="f6da6-139">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJava](https://github.com/ReactiveX/RxJava) [單一<String>](http://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="f6da6-139">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single<String>](http://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="f6da6-140">藉由呼叫[Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您可以撰寫邏輯，以針對您的用戶端產生存取權杖。</span><span class="sxs-lookup"><span data-stu-id="f6da6-140">With a call to [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a><span data-ttu-id="f6da6-141">已知限制</span><span class="sxs-lookup"><span data-stu-id="f6da6-141">Known limitations</span></span>

* <span data-ttu-id="f6da6-142">支援 JSON 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="f6da6-142">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="f6da6-143">支援 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="f6da6-143">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="f6da6-144">資料流尚未支援。</span><span class="sxs-lookup"><span data-stu-id="f6da6-144">Streaming isn't supported yet.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f6da6-145">其他資源</span><span class="sxs-lookup"><span data-stu-id="f6da6-145">Additional resources</span></span>

* [<span data-ttu-id="f6da6-146">Java API 參考</span><span class="sxs-lookup"><span data-stu-id="f6da6-146">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
