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
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="74a17-103">ASP.NET Core SignalR Java 用戶端</span><span class="sxs-lookup"><span data-stu-id="74a17-103">ASP.NET Core SignalR Java Client</span></span>

<span data-ttu-id="74a17-104">藉由[Mikael 馬力](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="74a17-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="74a17-105">Java 用戶端可讓您從 Java 程式碼，包括 Android 應用程式連接至 ASP.NET Core SignalR 伺服器。</span><span class="sxs-lookup"><span data-stu-id="74a17-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="74a17-106">像是[JavaScript 用戶端](xref:signalr/javascript-client)並[.NET 用戶端](xref:signalr/dotnet-client)，Java 用戶端可讓您接收和傳送訊息至即時中樞。</span><span class="sxs-lookup"><span data-stu-id="74a17-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="74a17-107">用於 ASP.NET Core 2.2 和更新版本的 Java 用戶端。</span><span class="sxs-lookup"><span data-stu-id="74a17-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="74a17-108">此文章中參照的範例 Java 主控台應用程式會使用 SignalR Java 用戶端。</span><span class="sxs-lookup"><span data-stu-id="74a17-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="74a17-109">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="74a17-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="74a17-110">SignalR Java 用戶端封裝安裝</span><span class="sxs-lookup"><span data-stu-id="74a17-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="74a17-111">*Signalr 0.1.0-preview 1 35029* JAR 檔案可讓用戶端連線到 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="74a17-111">The *signalr-0.1.0-preview1-35029* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="74a17-112">若要尋找最新的 JAR 檔案版本號碼，請參閱[Maven 搜尋結果](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav)。</span><span class="sxs-lookup"><span data-stu-id="74a17-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span></span>

<span data-ttu-id="74a17-113">如果使用 Gradle，加入下列這一行加入`dependencies`一節您*build.gradle*檔案：</span><span class="sxs-lookup"><span data-stu-id="74a17-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview1-35029'
```

<span data-ttu-id="74a17-114">如果使用 Maven，新增下列幾行內`<dependencies>`項目您*pom.xml*檔案：</span><span class="sxs-lookup"><span data-stu-id="74a17-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="74a17-115">連線至中樞</span><span class="sxs-lookup"><span data-stu-id="74a17-115">Connect to a hub</span></span>

<span data-ttu-id="74a17-116">若要建立`HubConnection`，則`HubConnectionBuilder`應該使用。</span><span class="sxs-lookup"><span data-stu-id="74a17-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="74a17-117">建立連接時，您可以設定中樞 URL 和記錄層級。</span><span class="sxs-lookup"><span data-stu-id="74a17-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="74a17-118">設定任何所需的選項，藉由呼叫任一`HubConnectionBuilder`方法之前`build`。</span><span class="sxs-lookup"><span data-stu-id="74a17-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="74a17-119">啟動與連線`start`。</span><span class="sxs-lookup"><span data-stu-id="74a17-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="74a17-120">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="74a17-120">Call hub methods from client</span></span>

<span data-ttu-id="74a17-121">呼叫`send`叫用中樞方法。</span><span class="sxs-lookup"><span data-stu-id="74a17-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="74a17-122">將中樞方法的名稱和任何定義於中樞方法的引數傳遞`send`。</span><span class="sxs-lookup"><span data-stu-id="74a17-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="74a17-123">用戶端方法呼叫來自中樞</span><span class="sxs-lookup"><span data-stu-id="74a17-123">Call client methods from hub</span></span>

<span data-ttu-id="74a17-124">使用`hubConnection.on`中樞可以呼叫用戶端上定義的方法。</span><span class="sxs-lookup"><span data-stu-id="74a17-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="74a17-125">在建置之後，但開始連接之前，請定義的方法。</span><span class="sxs-lookup"><span data-stu-id="74a17-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a><span data-ttu-id="74a17-126">已知的限制</span><span class="sxs-lookup"><span data-stu-id="74a17-126">Known limitations</span></span>

<span data-ttu-id="74a17-127">這是早期預覽版本的 Java 用戶端。</span><span class="sxs-lookup"><span data-stu-id="74a17-127">This is an early preview release of the Java client.</span></span> <span data-ttu-id="74a17-128">有許多尚不支援的功能。</span><span class="sxs-lookup"><span data-stu-id="74a17-128">There are many features that aren't supported yet.</span></span> <span data-ttu-id="74a17-129">下列的間距是正在進行未來的版本：</span><span class="sxs-lookup"><span data-stu-id="74a17-129">The following gaps are being worked on for future releases:</span></span>

* <span data-ttu-id="74a17-130">只有基本類型可接受的參數和傳回型別。</span><span class="sxs-lookup"><span data-stu-id="74a17-130">Only primitive types can be accepted as parameters and return types.</span></span>
* <span data-ttu-id="74a17-131">Api 均為同步。</span><span class="sxs-lookup"><span data-stu-id="74a17-131">The APIs are synchronous.</span></span>
* <span data-ttu-id="74a17-132">目前支援僅 「 傳送 」 呼叫類型。</span><span class="sxs-lookup"><span data-stu-id="74a17-132">Only the "Send" call type is supported at this time.</span></span> <span data-ttu-id="74a17-133">不支援 「 叫用 」，並傳回值的資料流。</span><span class="sxs-lookup"><span data-stu-id="74a17-133">"Invoke" and streaming of return values aren't supported.</span></span>
* <span data-ttu-id="74a17-134">目前不支援用戶端[Azure SignalR 服務](/azure/azure-signalr/)。</span><span class="sxs-lookup"><span data-stu-id="74a17-134">The client doesn't currently support the [Azure SignalR Service](/azure/azure-signalr/).</span></span>
* <span data-ttu-id="74a17-135">支援 JSON 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="74a17-135">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="74a17-136">支援 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="74a17-136">Only the WebSockets transport is supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="74a17-137">其他資源</span><span class="sxs-lookup"><span data-stu-id="74a17-137">Additional resources</span></span>

* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
