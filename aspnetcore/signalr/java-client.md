---
title: ASP.NET Core SignalR JAVA 用戶端
author: mikaelm12
description: 瞭解如何使用 ASP.NET Core SignalR的 JAVA 用戶端。
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/java-client
ms.openlocfilehash: 33c1e3b9b2b8990c811f3b49a978cbc630294c81
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82777250"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="10f3f-103">ASP.NET Core SignalR JAVA 用戶端</span><span class="sxs-lookup"><span data-stu-id="10f3f-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="10f3f-104">依[Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="10f3f-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="10f3f-105">JAVA 用戶端可讓您從 JAVA SignalR程式碼（包括 Android 應用程式）連接到 ASP.NET Core 伺服器。</span><span class="sxs-lookup"><span data-stu-id="10f3f-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="10f3f-106">就像[JavaScript 用戶端](xref:signalr/javascript-client)和[.net 客戶](xref:signalr/dotnet-client)端一樣，JAVA 用戶端可讓您即時接收和傳送訊息至中樞。</span><span class="sxs-lookup"><span data-stu-id="10f3f-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="10f3f-107">JAVA 用戶端會在 ASP.NET Core 2.2 和更新版本中提供。</span><span class="sxs-lookup"><span data-stu-id="10f3f-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="10f3f-108">本文中所參考的範例 JAVA 主控台應用程式會SignalR使用 JAVA 用戶端。</span><span class="sxs-lookup"><span data-stu-id="10f3f-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="10f3f-109">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="10f3f-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="10f3f-110">安裝SignalR JAVA 用戶端套件</span><span class="sxs-lookup"><span data-stu-id="10f3f-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="10f3f-111">*Signalr-1.0.0* JAR 檔案可讓用戶端連線到SignalR中樞。</span><span class="sxs-lookup"><span data-stu-id="10f3f-111">The *signalr-1.0.0* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="10f3f-112">若要尋找最新的 JAR 檔案版本號碼，請參閱[Maven 搜尋結果](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr)。</span><span class="sxs-lookup"><span data-stu-id="10f3f-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="10f3f-113">如果使用 Gradle，請在`dependencies` *Gradle*檔案的區段中新增下列程式程式碼：</span><span class="sxs-lookup"><span data-stu-id="10f3f-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

<span data-ttu-id="10f3f-114">如果使用 Maven，請在您的*pom*檔案`<dependencies>`的元素內新增下列幾行：</span><span class="sxs-lookup"><span data-stu-id="10f3f-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="10f3f-115">連接到中樞</span><span class="sxs-lookup"><span data-stu-id="10f3f-115">Connect to a hub</span></span>

<span data-ttu-id="10f3f-116">若要建立`HubConnection`， `HubConnectionBuilder`應該使用。</span><span class="sxs-lookup"><span data-stu-id="10f3f-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="10f3f-117">建立連接時，可以設定中樞 URL 和記錄層級。</span><span class="sxs-lookup"><span data-stu-id="10f3f-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="10f3f-118">在之前`HubConnectionBuilder` `build`呼叫任何方法，以設定任何必要的選項。</span><span class="sxs-lookup"><span data-stu-id="10f3f-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="10f3f-119">開始與`start`的連接。</span><span class="sxs-lookup"><span data-stu-id="10f3f-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="10f3f-120">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="10f3f-120">Call hub methods from client</span></span>

<span data-ttu-id="10f3f-121">呼叫會叫`send`用中樞方法。</span><span class="sxs-lookup"><span data-stu-id="10f3f-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="10f3f-122">將中樞方法名稱和 hub 方法中定義的任何引數傳遞`send`至。</span><span class="sxs-lookup"><span data-stu-id="10f3f-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> <span data-ttu-id="10f3f-123">如果您是在SignalR *無伺服器模式*中使用 Azure 服務，則無法從用戶端呼叫中樞方法。</span><span class="sxs-lookup"><span data-stu-id="10f3f-123">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="10f3f-124">如需詳細資訊，請參閱[ SignalR服務檔](/azure/azure-signalr/signalr-concept-serverless-development-config)。</span><span class="sxs-lookup"><span data-stu-id="10f3f-124">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="10f3f-125">從中樞呼叫用戶端方法</span><span class="sxs-lookup"><span data-stu-id="10f3f-125">Call client methods from hub</span></span>

<span data-ttu-id="10f3f-126">使用`hubConnection.on`定義中樞可以呼叫之用戶端上的方法。</span><span class="sxs-lookup"><span data-stu-id="10f3f-126">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="10f3f-127">在建立之後，但在開始連接之前，定義方法。</span><span class="sxs-lookup"><span data-stu-id="10f3f-127">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="10f3f-128">新增記錄</span><span class="sxs-lookup"><span data-stu-id="10f3f-128">Add logging</span></span>

<span data-ttu-id="10f3f-129">SignalR JAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="10f3f-129">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="10f3f-130">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="10f3f-130">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="10f3f-131">下列程式碼片段顯示如何搭配使用`java.util.logging`與SignalR JAVA 用戶端。</span><span class="sxs-lookup"><span data-stu-id="10f3f-131">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="10f3f-132">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="10f3f-132">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="10f3f-133">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="10f3f-133">This can safely be ignored.</span></span>

## <a name="android-development-notes"></a><span data-ttu-id="10f3f-134">Android 開發注意事項</span><span class="sxs-lookup"><span data-stu-id="10f3f-134">Android development notes</span></span>

<span data-ttu-id="10f3f-135">在SignalR Android SDK 用戶端功能的相容性方面，指定目標 Android SDK 版本時，請考慮下列專案：</span><span class="sxs-lookup"><span data-stu-id="10f3f-135">With regards to Android SDK compatibility for the SignalR client features, consider the following items when specifying your target Android SDK version:</span></span>

* <span data-ttu-id="10f3f-136">SignalR JAVA 用戶端會在 Android API 層級16和更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="10f3f-136">The SignalR Java Client will run on Android API Level 16 and later.</span></span>
* <span data-ttu-id="10f3f-137">透過 Azure SignalR服務連線會需要 Android API 層級20和更新版本，[因為SignalR Azure 服務](/azure/azure-signalr/signalr-overview)需要 TLS 1.2，而且不支援以 sha-1 為基礎的加密套件。</span><span class="sxs-lookup"><span data-stu-id="10f3f-137">Connecting through the Azure SignalR Service will require Android API Level 20 and later because the [Azure SignalR Service](/azure/azure-signalr/signalr-overview) requires TLS 1.2 and doesn't support SHA-1-based cipher suites.</span></span> <span data-ttu-id="10f3f-138">Android 已在 API 層級20中[新增對 SHA-256 （和更新版本）加密套件的支援](https://developer.android.com/reference/javax/net/ssl/SSLSocket)。</span><span class="sxs-lookup"><span data-stu-id="10f3f-138">Android [added support for SHA-256 (and above) cipher suites](https://developer.android.com/reference/javax/net/ssl/SSLSocket) in API Level 20.</span></span>

## <a name="configure-bearer-token-authentication"></a><span data-ttu-id="10f3f-139">設定持有人權杖驗證</span><span class="sxs-lookup"><span data-stu-id="10f3f-139">Configure bearer token authentication</span></span>

<span data-ttu-id="10f3f-140">在SignalR JAVA 用戶端中，您可以藉由提供[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)的「存取權杖處理站」，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="10f3f-140">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an "access token factory" to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="10f3f-141">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava) [單一\<字串>](https://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="10f3f-141">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="10f3f-142">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="10f3f-142">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a><span data-ttu-id="10f3f-143">已知限制</span><span class="sxs-lookup"><span data-stu-id="10f3f-143">Known limitations</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="10f3f-144">僅支援 JSON 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="10f3f-144">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="10f3f-145">不支援傳輸回溯和伺服器傳送的事件傳輸。</span><span class="sxs-lookup"><span data-stu-id="10f3f-145">Transport fallback and the Server Sent Events transport aren't supported.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="10f3f-146">僅支援 JSON 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="10f3f-146">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="10f3f-147">僅支援 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="10f3f-147">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="10f3f-148">尚不支援串流。</span><span class="sxs-lookup"><span data-stu-id="10f3f-148">Streaming isn't supported yet.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="10f3f-149">其他資源</span><span class="sxs-lookup"><span data-stu-id="10f3f-149">Additional resources</span></span>

* [<span data-ttu-id="10f3f-150">Java API 參考</span><span class="sxs-lookup"><span data-stu-id="10f3f-150">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* <span data-ttu-id="10f3f-151">[Azure SignalR服務無伺服器檔](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="10f3f-151">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
