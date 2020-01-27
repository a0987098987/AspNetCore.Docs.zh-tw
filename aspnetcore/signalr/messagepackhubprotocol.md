---
title: 在 SignalR 中使用適用于 ASP.NET Core 的 MessagePack Hub 通訊協定
author: bradygaster
description: 將 MessagePack 中樞通訊協定新增至 ASP.NET Core SignalR。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 1b01357233a9b95a5da052d92e30232c94e78a78
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727230"
---
# <a name="use-messagepack-hub-protocol-in-opno-locsignalr-for-aspnet-core"></a><span data-ttu-id="ee4a3-103">在 SignalR 中使用適用于 ASP.NET Core 的 MessagePack Hub 通訊協定</span><span class="sxs-lookup"><span data-stu-id="ee4a3-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="ee4a3-104">依[Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="ee4a3-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="ee4a3-105">本文假設讀者已熟悉[開始](xref:tutorials/signalr)使用中所涵蓋的主題。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="ee4a3-106">什麼是 MessagePack？</span><span class="sxs-lookup"><span data-stu-id="ee4a3-106">What is MessagePack?</span></span>

<span data-ttu-id="ee4a3-107">[MessagePack](https://msgpack.org/index.html)是一種快速且精簡的二進位序列化格式。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="ee4a3-108">這在效能和頻寬有所顧慮時很有用，因為它會建立比[JSON](https://www.json.org/)更小的訊息。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="ee4a3-109">因為這是一種二進位格式，所以在查看網路追蹤和記錄檔時，除非透過 MessagePack 剖析器傳遞位元組，否則無法讀取訊息。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> SignalR<span data-ttu-id="ee4a3-110"> 具有 MessagePack 格式的內建支援，並提供 Api 讓用戶端和伺服器使用。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-110"> has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="ee4a3-111">在伺服器上設定 MessagePack</span><span class="sxs-lookup"><span data-stu-id="ee4a3-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="ee4a3-112">若要在伺服器上啟用 MessagePack Hub 通訊協定，請在您的應用程式中安裝 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 套件。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="ee4a3-113">在 Startup.cs 檔案中，將 `AddMessagePackProtocol` 新增至 `AddSignalR` 呼叫，以在伺服器上啟用 MessagePack 支援。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="ee4a3-114">預設會啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-114">JSON is enabled by default.</span></span> <span data-ttu-id="ee4a3-115">新增 MessagePack 可同時支援 JSON 和 MessagePack 用戶端。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="ee4a3-116">若要自訂 MessagePack 將資料格式化的方式，請 `AddMessagePackProtocol` 取得設定選項的委派。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="ee4a3-117">在該委派中，`FormatterResolvers` 屬性可以用來設定 MessagePack 序列化選項。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="ee4a3-118">如需解決器如何工作的詳細資訊，請造訪[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)上的 MessagePack 程式庫。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="ee4a3-119">屬性可以在您要序列化的物件上使用，以定義它們的處理方式。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="ee4a3-120">設定用戶端上的 MessagePack</span><span class="sxs-lookup"><span data-stu-id="ee4a3-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="ee4a3-121">根據預設，支援的用戶端會啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="ee4a3-122">用戶端只能支援單一通訊協定。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="ee4a3-123">新增 MessagePack 支援將會取代任何先前設定的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="ee4a3-124">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="ee4a3-124">.NET client</span></span>

<span data-ttu-id="ee4a3-125">若要在 .NET 用戶端中啟用 MessagePack，請安裝 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 封裝，並在 `HubConnectionBuilder`上呼叫 `AddMessagePackProtocol`。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="ee4a3-126">這個 `AddMessagePackProtocol` 呼叫會採用委派來設定選項，就像伺服器一樣。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="ee4a3-127">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="ee4a3-127">JavaScript client</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ee4a3-128">`@microsoft/signalr-protocol-msgpack` npm 套件會提供 JavaScript 用戶端的 MessagePack 支援。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-128">MessagePack support for the JavaScript client is provided by the `@microsoft/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="ee4a3-129">在命令 shell 中執行下列命令，以安裝套件：</span><span class="sxs-lookup"><span data-stu-id="ee4a3-129">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ee4a3-130">`@aspnet/signalr-protocol-msgpack` npm 套件會提供 JavaScript 用戶端的 MessagePack 支援。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-130">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="ee4a3-131">在命令 shell 中執行下列命令，以安裝套件：</span><span class="sxs-lookup"><span data-stu-id="ee4a3-131">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

<span data-ttu-id="ee4a3-132">安裝 npm 套件之後，您可以直接透過 JavaScript 模組載入器使用模組，或藉由參考下列檔案，將其匯入至瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="ee4a3-132">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the following file:</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ee4a3-133">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="ee4a3-133">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ee4a3-134">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="ee4a3-134">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

<span data-ttu-id="ee4a3-135">在瀏覽器中，也必須參考 `msgpack5` 程式庫。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-135">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="ee4a3-136">使用 `<script>` 標記來建立參考。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-136">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="ee4a3-137">您可以在*node_modules \msgpack5\dist\msgpack5.js*找到此程式庫。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-137">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="ee4a3-138">使用 `<script>` 元素時，順序很重要。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-138">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="ee4a3-139">如果在*msgpack5*之前參考*signalr-protocol-msgpack* ，則嘗試使用 MessagePack 連接時，會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-139">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="ee4a3-140">*signalr-protocol-msgpack*之前也需要*signalr* 。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-140">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="ee4a3-141">將 `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` 新增至 `HubConnectionBuilder` 會將用戶端設定為在連接到伺服器時使用 MessagePack 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-141">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="ee4a3-142">在此階段中，JavaScript 用戶端上沒有 MessagePack 通訊協定的設定選項。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-142">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="ee4a3-143">MessagePack 的相容</span><span class="sxs-lookup"><span data-stu-id="ee4a3-143">MessagePack quirks</span></span>

<span data-ttu-id="ee4a3-144">使用 MessagePack Hub 通訊協定時，有幾個要注意的問題。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-144">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="ee4a3-145">MessagePack 區分大小寫</span><span class="sxs-lookup"><span data-stu-id="ee4a3-145">MessagePack is case-sensitive</span></span>

<span data-ttu-id="ee4a3-146">MessagePack 通訊協定會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-146">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="ee4a3-147">例如，請考慮下列C#類別：</span><span class="sxs-lookup"><span data-stu-id="ee4a3-147">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="ee4a3-148">從 JavaScript 用戶端傳送時，您必須使用 `PascalCased` 的屬性名稱，因為大小寫必須完全C#符合類別。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-148">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="ee4a3-149">例如：</span><span class="sxs-lookup"><span data-stu-id="ee4a3-149">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="ee4a3-150">使用 `camelCased` 名稱並不會正確地C#系結至類別。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-150">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="ee4a3-151">您可以使用 `Key` 屬性來指定不同的 MessagePack 屬性名稱，藉此解決此情況。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-151">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="ee4a3-152">如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-152">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="ee4a3-153">序列化/還原序列化時，不會保留 DateTime. Kind</span><span class="sxs-lookup"><span data-stu-id="ee4a3-153">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="ee4a3-154">MessagePack 通訊協定不會提供方法來編碼 `DateTime`的 `Kind` 值。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-154">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="ee4a3-155">因此，在還原序列化日期時，MessagePack 中樞通訊協定會假設內送日期是 UTC 格式。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-155">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="ee4a3-156">如果您要在當地時間使用 `DateTime` 值，建議您在傳送之前先轉換成 UTC。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-156">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="ee4a3-157">當您收到時，將它們從 UTC 轉換為當地時間。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-157">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="ee4a3-158">如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632)。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-158">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="ee4a3-159">JavaScript 中的 MessagePack 不支援 MinValue</span><span class="sxs-lookup"><span data-stu-id="ee4a3-159">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="ee4a3-160">SignalR JavaScript 用戶端所使用的[msgpack5](https://github.com/mcollina/msgpack5)程式庫不支援 MessagePack 中的 `timestamp96` 類型。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-160">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="ee4a3-161">這個型別是用來編碼非常大的日期值（在過去或未來很久的時候）。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-161">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="ee4a3-162">`DateTime.MinValue` 的值 `January 1, 0001` 必須以 `timestamp96` 值編碼。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-162">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="ee4a3-163">因此，不支援將 `DateTime.MinValue` 傳送至 JavaScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-163">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="ee4a3-164">當 JavaScript 用戶端收到 `DateTime.MinValue` 時，會擲回下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="ee4a3-164">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="ee4a3-165">通常會使用 `DateTime.MinValue` 來編碼「遺漏」或 `null` 值。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-165">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="ee4a3-166">如果您需要在 MessagePack 中編碼該值，請使用可為 null 的 `DateTime` 值（`DateTime?`），或編碼個別的 `bool` 值，以指出日期是否存在。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-166">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="ee4a3-167">如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228)。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-167">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="ee4a3-168">「預先編譯」環境中的 MessagePack 支援</span><span class="sxs-lookup"><span data-stu-id="ee4a3-168">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="ee4a3-169">.NET 用戶端和伺服器使用的[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8)程式庫會使用程式碼產生來優化序列化。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-169">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="ee4a3-170">因此，在使用「預先」編譯的環境（例如 Xamarin iOS 或 Unity）上，預設不支援它。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-170">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="ee4a3-171">在這些環境中，可以藉由「預先產生」序列化程式/還原序列化程式碼來使用 MessagePack。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-171">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="ee4a3-172">如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8#pre-code-generationunityxamarin-supports)。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-172">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="ee4a3-173">預先產生序列化程式之後，您可以使用傳遞至 `AddMessagePackProtocol`的設定委派來註冊它們：</span><span class="sxs-lookup"><span data-stu-id="ee4a3-173">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.GeneratedResolver.Instance,
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="ee4a3-174">MessagePack 中的類型檢查較嚴格</span><span class="sxs-lookup"><span data-stu-id="ee4a3-174">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="ee4a3-175">JSON 中樞通訊協定會在還原序列化期間執行類型轉換。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-175">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="ee4a3-176">例如，如果內送物件的屬性值是數位（`{ foo: 42 }`），但是 .NET 類別上的屬性是 `string`類型，則會轉換此值。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-176">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="ee4a3-177">不過，MessagePack 不會執行這項轉換，而且會擲回例外狀況，可在伺服器端記錄（和主控台）中看到：</span><span class="sxs-lookup"><span data-stu-id="ee4a3-177">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="ee4a3-178">如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937)。</span><span class="sxs-lookup"><span data-stu-id="ee4a3-178">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="ee4a3-179">相關資源</span><span class="sxs-lookup"><span data-stu-id="ee4a3-179">Related resources</span></span>

* [<span data-ttu-id="ee4a3-180">開始使用</span><span class="sxs-lookup"><span data-stu-id="ee4a3-180">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="ee4a3-181">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="ee4a3-181">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="ee4a3-182">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="ee4a3-182">JavaScript client</span></span>](xref:signalr/javascript-client)
