---
title: 使用 ASP.NET Core SignalR 中 MessagePack 中樞通訊協定
author: rachelappel
description: 加入 ASP.NET Core SignalR MessagePack 中樞通訊協定。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: b6c33c4da47a19d67bffbaf84f54d59013edadbe
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252474"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="69f9a-103">使用 ASP.NET Core SignalR 中 MessagePack 中樞通訊協定</span><span class="sxs-lookup"><span data-stu-id="69f9a-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="69f9a-104">由[Brennan 瑜吉](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="69f9a-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="69f9a-105">本文假設讀者已熟悉所涵蓋的主題[開始](xref:signalr/get-started)。</span><span class="sxs-lookup"><span data-stu-id="69f9a-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:signalr/get-started).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="69f9a-106">什麼是 MessagePack？</span><span class="sxs-lookup"><span data-stu-id="69f9a-106">What is MessagePack?</span></span>

<span data-ttu-id="69f9a-107">[MessagePack](https://msgpack.org/index.html)是非常快速且壓縮的二進位序列化格式。</span><span class="sxs-lookup"><span data-stu-id="69f9a-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="69f9a-108">因此，適用於效能和頻寬是一項考量因為它會建立較小的訊息相較於[JSON](https://www.json.org/)。</span><span class="sxs-lookup"><span data-stu-id="69f9a-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="69f9a-109">因為它是一種二進位格式時，如果除非位元組都會通過 MessagePack 剖析器，查看網路追蹤和記錄檔訊息就無法讀取。</span><span class="sxs-lookup"><span data-stu-id="69f9a-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="69f9a-110">SignalR 具有 MessagePack 格式，內建支援，並提供用戶端和伺服器使用的 Api。</span><span class="sxs-lookup"><span data-stu-id="69f9a-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="69f9a-111">在伺服器上設定 MessagePack</span><span class="sxs-lookup"><span data-stu-id="69f9a-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="69f9a-112">若要啟用 MessagePack 中樞通訊協定，在伺服器上，安裝`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`應用程式中的封裝。</span><span class="sxs-lookup"><span data-stu-id="69f9a-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="69f9a-113">在 Startup.cs 檔案中加入`AddMessagePackProtocol`至`AddSignalR`呼叫 MessagePack 伺服器上啟用支援。</span><span class="sxs-lookup"><span data-stu-id="69f9a-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="69f9a-114">預設會啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="69f9a-114">JSON is enabled by default.</span></span> <span data-ttu-id="69f9a-115">新增 MessagePack 可讓支援以 JSON 和 MessagePack 用戶端。</span><span class="sxs-lookup"><span data-stu-id="69f9a-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="69f9a-116">若要自訂如何 MessagePack 將會格式化您的資料，`AddMessagePackProtocol`會針對設定選項進行委派。</span><span class="sxs-lookup"><span data-stu-id="69f9a-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="69f9a-117">在該委派中，`FormatterResolvers`屬性可用來設定 MessagePack 序列化選項。</span><span class="sxs-lookup"><span data-stu-id="69f9a-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="69f9a-118">如需有關 「 解決者 」 的運作方式的詳細資訊，請瀏覽 MessagePack 程式庫[MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp)。</span><span class="sxs-lookup"><span data-stu-id="69f9a-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="69f9a-119">屬性可以用於您想要定義它們應如何處理序列化的物件。</span><span class="sxs-lookup"><span data-stu-id="69f9a-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="69f9a-120">設定用戶端的 MessagePack</span><span class="sxs-lookup"><span data-stu-id="69f9a-120">Configure MessagePack on the client</span></span>

### <a name="net-client"></a><span data-ttu-id="69f9a-121">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="69f9a-121">.NET client</span></span>

<span data-ttu-id="69f9a-122">若要啟用 MessagePack.NET 用戶端中的，安裝`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`封裝並呼叫`AddMessagePackProtocol`上`HubConnectionBuilder`。</span><span class="sxs-lookup"><span data-stu-id="69f9a-122">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="69f9a-123">這`AddMessagePackProtocol`呼叫採用委派的設定選項，就像伺服器一樣。</span><span class="sxs-lookup"><span data-stu-id="69f9a-123">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="69f9a-124">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="69f9a-124">JavaScript client</span></span>

<span data-ttu-id="69f9a-125">Javascript 用戶端的 MessagePack 支援由提供`@aspnet/signalr-protocol-msgpack`NPM 封裝。</span><span class="sxs-lookup"><span data-stu-id="69f9a-125">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="69f9a-126">安裝之後的 npm 封裝，模組可以直接透過 JavaScript 模組載入器或藉由參考匯入至瀏覽器*node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 檔案。</span><span class="sxs-lookup"><span data-stu-id="69f9a-126">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="69f9a-127">在瀏覽器`msgpack5`也必須參考程式庫。</span><span class="sxs-lookup"><span data-stu-id="69f9a-127">In a browser the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="69f9a-128">使用`<script>`建立參考的標記。</span><span class="sxs-lookup"><span data-stu-id="69f9a-128">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="69f9a-129">程式庫，請參閱*node_modules\msgpack5\dist\msgpack5.js*。</span><span class="sxs-lookup"><span data-stu-id="69f9a-129">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="69f9a-130">當使用`<script>`元素的順序很重要。</span><span class="sxs-lookup"><span data-stu-id="69f9a-130">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="69f9a-131">如果*signalr-通訊協定-msgpack.js*參考之前*msgpack5.js*，嘗試與 MessagePack 連接時，會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="69f9a-131">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="69f9a-132">*signalr.js*之前也需要*signalr-通訊協定-msgpack.js*。</span><span class="sxs-lookup"><span data-stu-id="69f9a-132">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="69f9a-133">加入`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`至`HubConnectionBuilder`會設定用戶端連接到伺服器時使用 MessagePack 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="69f9a-133">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="69f9a-134">在這個階段中，沒有 MessagePack 通訊協定上的 JavaScript 用戶端組態選項。</span><span class="sxs-lookup"><span data-stu-id="69f9a-134">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="69f9a-135">相關資源</span><span class="sxs-lookup"><span data-stu-id="69f9a-135">Related resources</span></span>

* [<span data-ttu-id="69f9a-136">開始使用</span><span class="sxs-lookup"><span data-stu-id="69f9a-136">Get Started</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="69f9a-137">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="69f9a-137">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="69f9a-138">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="69f9a-138">JavaScript client</span></span>](xref:signalr/javascript-client)
