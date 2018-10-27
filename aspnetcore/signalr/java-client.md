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
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="af25a-103">ASP.NET Core SignalR Java 用戶端</span><span class="sxs-lookup"><span data-stu-id="af25a-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="af25a-104">藉由[Mikael 馬力](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="af25a-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="af25a-105">Java 用戶端可讓您從 Java 程式碼，包括 Android 應用程式連接至 ASP.NET Core SignalR 伺服器。</span><span class="sxs-lookup"><span data-stu-id="af25a-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="af25a-106">像是[JavaScript 用戶端](xref:signalr/javascript-client)並[.NET 用戶端](xref:signalr/dotnet-client)，Java 用戶端可讓您接收和傳送訊息至即時中樞。</span><span class="sxs-lookup"><span data-stu-id="af25a-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="af25a-107">用於 ASP.NET Core 2.2 和更新版本的 Java 用戶端。</span><span class="sxs-lookup"><span data-stu-id="af25a-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="af25a-108">此文章中參照的範例 Java 主控台應用程式會使用 SignalR Java 用戶端。</span><span class="sxs-lookup"><span data-stu-id="af25a-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="af25a-109">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="af25a-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="af25a-110">SignalR Java 用戶端封裝安裝</span><span class="sxs-lookup"><span data-stu-id="af25a-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="af25a-111">*Signalr 1.0.0-preview3 35501* JAR 檔案可讓用戶端連線到 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="af25a-111">The *signalr-1.0.0-preview3-35501* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="af25a-112">若要尋找最新的 JAR 檔案版本號碼，請參閱[Maven 搜尋結果](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr)。</span><span class="sxs-lookup"><span data-stu-id="af25a-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="af25a-113">如果使用 Gradle，加入下列這一行加入`dependencies`一節您*build.gradle*檔案：</span><span class="sxs-lookup"><span data-stu-id="af25a-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

<span data-ttu-id="af25a-114">如果使用 Maven，新增下列幾行內`<dependencies>`項目您*pom.xml*檔案：</span><span class="sxs-lookup"><span data-stu-id="af25a-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="af25a-115">連線至中樞</span><span class="sxs-lookup"><span data-stu-id="af25a-115">Connect to a hub</span></span>

<span data-ttu-id="af25a-116">若要建立`HubConnection`，則`HubConnectionBuilder`應該使用。</span><span class="sxs-lookup"><span data-stu-id="af25a-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="af25a-117">建立連接時，您可以設定中樞 URL 和記錄層級。</span><span class="sxs-lookup"><span data-stu-id="af25a-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="af25a-118">設定任何所需的選項，藉由呼叫任一`HubConnectionBuilder`方法之前`build`。</span><span class="sxs-lookup"><span data-stu-id="af25a-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="af25a-119">啟動與連線`start`。</span><span class="sxs-lookup"><span data-stu-id="af25a-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="af25a-120">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="af25a-120">Call hub methods from client</span></span>

<span data-ttu-id="af25a-121">呼叫`send`叫用中樞方法。</span><span class="sxs-lookup"><span data-stu-id="af25a-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="af25a-122">將中樞方法的名稱和任何定義於中樞方法的引數傳遞`send`。</span><span class="sxs-lookup"><span data-stu-id="af25a-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="af25a-123">用戶端方法呼叫來自中樞</span><span class="sxs-lookup"><span data-stu-id="af25a-123">Call client methods from hub</span></span>

<span data-ttu-id="af25a-124">使用`hubConnection.on`中樞可以呼叫用戶端上定義的方法。</span><span class="sxs-lookup"><span data-stu-id="af25a-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="af25a-125">在建置之後，但開始連接之前，請定義的方法。</span><span class="sxs-lookup"><span data-stu-id="af25a-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="af25a-126">新增記錄</span><span class="sxs-lookup"><span data-stu-id="af25a-126">Add logging</span></span>

<span data-ttu-id="af25a-127">SignalR Java 用戶端會使用[SLF4J](https://www.slf4j.org/)記錄的程式庫。</span><span class="sxs-lookup"><span data-stu-id="af25a-127">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="af25a-128">它是高層級的記錄 API，可讓程式庫的使用者選擇他們自己的特定記錄實作，藉由將特定記錄相依性。</span><span class="sxs-lookup"><span data-stu-id="af25a-128">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="af25a-129">下列程式碼片段示範如何使用`java.util.logging`與 SignalR Java 用戶端。</span><span class="sxs-lookup"><span data-stu-id="af25a-129">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="af25a-130">如果您未設定登入您的相依性，SLF4J 會載入預設的無作業記錄器，並出現下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="af25a-130">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="af25a-131">這可以放心地忽略。</span><span class="sxs-lookup"><span data-stu-id="af25a-131">This can safely be ignored.</span></span>

## <a name="known-limitations"></a><span data-ttu-id="af25a-132">已知的限制</span><span class="sxs-lookup"><span data-stu-id="af25a-132">Known limitations</span></span>

<span data-ttu-id="af25a-133">這是預覽版本的 Java 用戶端。</span><span class="sxs-lookup"><span data-stu-id="af25a-133">This is a preview release of the Java client.</span></span> <span data-ttu-id="af25a-134">不支援某些功能：</span><span class="sxs-lookup"><span data-stu-id="af25a-134">Some features aren't supported:</span></span>

* <span data-ttu-id="af25a-135">支援 JSON 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="af25a-135">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="af25a-136">支援 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="af25a-136">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="af25a-137">資料流尚未支援。</span><span class="sxs-lookup"><span data-stu-id="af25a-137">Streaming isn't supported yet.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="af25a-138">其他資源</span><span class="sxs-lookup"><span data-stu-id="af25a-138">Additional resources</span></span>

* [<span data-ttu-id="af25a-139">Java API 參考</span><span class="sxs-lookup"><span data-stu-id="af25a-139">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
