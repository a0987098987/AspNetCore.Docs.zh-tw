---
title: 使用 ASP.NET Core signalr MessagePack 中樞通訊協定
author: tdykstra
description: 加入 ASP.NET Core SignalR MessagePack 中樞通訊協定。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 78b708c50ce7a8101c9eaa558171540e61c0d7f0
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39094991"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="77aaa-103">使用 ASP.NET Core signalr MessagePack 中樞通訊協定</span><span class="sxs-lookup"><span data-stu-id="77aaa-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="77aaa-104">藉由[brennan 第瑜吉](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="77aaa-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="77aaa-105">本文假設讀者已熟悉所涵蓋的主題[開始](xref:tutorials/signalr)。</span><span class="sxs-lookup"><span data-stu-id="77aaa-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="77aaa-106">MessagePack 是什麼？</span><span class="sxs-lookup"><span data-stu-id="77aaa-106">What is MessagePack?</span></span>

<span data-ttu-id="77aaa-107">[MessagePack](https://msgpack.org/index.html)是快速且精簡的二進位序列化格式。</span><span class="sxs-lookup"><span data-stu-id="77aaa-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="77aaa-108">效能和頻寬是需要考量因為它會建立相較於較小的訊息時很有用[JSON](https://www.json.org/)。</span><span class="sxs-lookup"><span data-stu-id="77aaa-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="77aaa-109">因為它是一種二進位格式時，如果除非位元組都會通過 MessagePack 剖析器，查看網路追蹤和記錄檔訊息就無法讀取。</span><span class="sxs-lookup"><span data-stu-id="77aaa-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="77aaa-110">SignalR 的 MessagePack 格式中，內建支援，並提供用戶端和伺服器使用的 Api。</span><span class="sxs-lookup"><span data-stu-id="77aaa-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="77aaa-111">在伺服器上設定 MessagePack</span><span class="sxs-lookup"><span data-stu-id="77aaa-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="77aaa-112">若要啟用 MessagePack 中樞通訊協定，在伺服器上，安裝`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`應用程式中的封裝。</span><span class="sxs-lookup"><span data-stu-id="77aaa-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="77aaa-113">在 Startup.cs 檔案中新增`AddMessagePackProtocol`至`AddSignalR`啟用 MessagePack 支援在伺服器上的呼叫。</span><span class="sxs-lookup"><span data-stu-id="77aaa-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="77aaa-114">預設會啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="77aaa-114">JSON is enabled by default.</span></span> <span data-ttu-id="77aaa-115">新增 MessagePack 啟用 JSON 和 MessagePack 用戶端的支援。</span><span class="sxs-lookup"><span data-stu-id="77aaa-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="77aaa-116">若要自訂 MessagePack 會列印文件的格式，請您的資料，`AddMessagePackProtocol`會接受委派的設定選項。</span><span class="sxs-lookup"><span data-stu-id="77aaa-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="77aaa-117">在該委派，`FormatterResolvers`屬性可用來設定 MessagePack 序列化選項。</span><span class="sxs-lookup"><span data-stu-id="77aaa-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="77aaa-118">如需有關如何解析程式的運作方式的詳細資訊，請瀏覽 MessagePack library [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp)。</span><span class="sxs-lookup"><span data-stu-id="77aaa-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="77aaa-119">在您想要以定義他們應該如何處理序列化的物件上可用的屬性。</span><span class="sxs-lookup"><span data-stu-id="77aaa-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="77aaa-120">用戶端上設定 MessagePack</span><span class="sxs-lookup"><span data-stu-id="77aaa-120">Configure MessagePack on the client</span></span>

### <a name="net-client"></a><span data-ttu-id="77aaa-121">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="77aaa-121">.NET client</span></span>

<span data-ttu-id="77aaa-122">若要啟用 MessagePack.NET 用戶端中的，安裝`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`封裝並呼叫`AddMessagePackProtocol`上`HubConnectionBuilder`。</span><span class="sxs-lookup"><span data-stu-id="77aaa-122">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="77aaa-123">這`AddMessagePackProtocol`呼叫會接受委派的設定選項，就像伺服器。</span><span class="sxs-lookup"><span data-stu-id="77aaa-123">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="77aaa-124">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="77aaa-124">JavaScript client</span></span>

<span data-ttu-id="77aaa-125">會提供 Javascript 用戶端的 MessagePack 支援`@aspnet/signalr-protocol-msgpack`NPM 套件。</span><span class="sxs-lookup"><span data-stu-id="77aaa-125">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="77aaa-126">安裝 npm 套件之後，此模組可以直接透過 JavaScript 模組載入器或藉由參考匯入至瀏覽器 *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 檔案。</span><span class="sxs-lookup"><span data-stu-id="77aaa-126">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="77aaa-127">在瀏覽器`msgpack5`也必須參考程式庫。</span><span class="sxs-lookup"><span data-stu-id="77aaa-127">In a browser the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="77aaa-128">使用`<script>`標記，以建立參考。</span><span class="sxs-lookup"><span data-stu-id="77aaa-128">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="77aaa-129">程式庫，請參閱*node_modules\msgpack5\dist\msgpack5.js*。</span><span class="sxs-lookup"><span data-stu-id="77aaa-129">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="77aaa-130">當使用`<script>`元素的順序很重要。</span><span class="sxs-lookup"><span data-stu-id="77aaa-130">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="77aaa-131">如果*signalr-通訊協定-msgpack.js*參考之前*msgpack5.js*，嘗試使用 MessagePack 連線時，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="77aaa-131">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="77aaa-132">*signalr.js*之前，也需要*signalr-通訊協定-msgpack.js*。</span><span class="sxs-lookup"><span data-stu-id="77aaa-132">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="77aaa-133">新增`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`至`HubConnectionBuilder`會設定用戶端連接到伺服器時，使用 MessagePack 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="77aaa-133">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="77aaa-134">在此階段中，有 MessagePack 通訊協定在 JavaScript 用戶端上的沒有組態選項。</span><span class="sxs-lookup"><span data-stu-id="77aaa-134">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="77aaa-135">相關資源</span><span class="sxs-lookup"><span data-stu-id="77aaa-135">Related resources</span></span>

* [<span data-ttu-id="77aaa-136">開始使用</span><span class="sxs-lookup"><span data-stu-id="77aaa-136">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="77aaa-137">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="77aaa-137">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="77aaa-138">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="77aaa-138">JavaScript client</span></span>](xref:signalr/javascript-client)
