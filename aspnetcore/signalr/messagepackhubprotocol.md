---
title: 在中使用 MessagePack 中樞通訊協定 SignalR 來 ASP.NET Core
author: bradygaster
description: 將 MessagePack 中樞通訊協定新增至 ASP.NET Core SignalR 。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/13/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: ff78321cba11b1c91a12b5c777c505b4c9b85309
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408314"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="17849-103">在中使用 MessagePack 中樞通訊協定 SignalR 來 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="17849-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="17849-104">本文假設讀者已熟悉[開始](xref:tutorials/signalr)使用中所涵蓋的主題。</span><span class="sxs-lookup"><span data-stu-id="17849-104">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="17849-105">什麼是 MessagePack？</span><span class="sxs-lookup"><span data-stu-id="17849-105">What is MessagePack?</span></span>

<span data-ttu-id="17849-106">[MessagePack](https://msgpack.org/index.html)是快速且精簡的二進位序列化格式。</span><span class="sxs-lookup"><span data-stu-id="17849-106">[MessagePack](https://msgpack.org/index.html) is a fast and compact binary serialization format.</span></span> <span data-ttu-id="17849-107">這在效能和頻寬有所顧慮時很有用，因為它會建立比[JSON](https://www.json.org/)更小的訊息。</span><span class="sxs-lookup"><span data-stu-id="17849-107">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="17849-108">在查看網路追蹤和記錄檔時，二進位訊息無法讀取，除非是透過 MessagePack 剖析器傳遞位元組。</span><span class="sxs-lookup"><span data-stu-id="17849-108">The binary messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> SignalR<span data-ttu-id="17849-109">具有 MessagePack 格式的內建支援，並提供 Api 供用戶端和伺服器使用。</span><span class="sxs-lookup"><span data-stu-id="17849-109"> has built-in support for the MessagePack format and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="17849-110">在伺服器上設定 MessagePack</span><span class="sxs-lookup"><span data-stu-id="17849-110">Configure MessagePack on the server</span></span>

<span data-ttu-id="17849-111">若要在伺服器上啟用 MessagePack Hub 通訊協定，請 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 在您的應用程式中安裝套件。</span><span class="sxs-lookup"><span data-stu-id="17849-111">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="17849-112">在 `Startup.ConfigureServices` 方法中，將加入 `AddMessagePackProtocol` 至 `AddSignalR` 呼叫以在伺服器上啟用 MessagePack 支援。</span><span class="sxs-lookup"><span data-stu-id="17849-112">In the `Startup.ConfigureServices` method, add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="17849-113">預設會啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="17849-113">JSON is enabled by default.</span></span> <span data-ttu-id="17849-114">新增 MessagePack 可同時支援 JSON 和 MessagePack 用戶端。</span><span class="sxs-lookup"><span data-stu-id="17849-114">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="17849-115">若要自訂 MessagePack 如何將資料格式化，請 `AddMessagePackProtocol` 採用委派來設定選項。</span><span class="sxs-lookup"><span data-stu-id="17849-115">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="17849-116">在該委派中， `SerializerOptions` 屬性可以用來設定 MessagePack 序列化選項。</span><span class="sxs-lookup"><span data-stu-id="17849-116">In that delegate, the `SerializerOptions` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="17849-117">如需解決器如何工作的詳細資訊，請造訪[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)上的 MessagePack 程式庫。</span><span class="sxs-lookup"><span data-stu-id="17849-117">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="17849-118">屬性可以在您要序列化的物件上使用，以定義它們的處理方式。</span><span class="sxs-lookup"><span data-stu-id="17849-118">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.SerializerOptions = MessagePackSerializerOptions.Standard
            .WithResolver(new CustomResolver())
            .WithSecurity(MessagePackSecurity.UntrustedData);
    });
```

> [!WARNING]
> <span data-ttu-id="17849-119">我們強烈建議您查看[CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) ，並套用建議的修補程式。</span><span class="sxs-lookup"><span data-stu-id="17849-119">We strongly recommend reviewing [CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) and applying the recommended patches.</span></span> <span data-ttu-id="17849-120">例如， `.WithSecurity(MessagePackSecurity.UntrustedData)` 在取代時呼叫 `SerializerOptions` 。</span><span class="sxs-lookup"><span data-stu-id="17849-120">For example, calling `.WithSecurity(MessagePackSecurity.UntrustedData)` when replacing the `SerializerOptions`.</span></span>

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="17849-121">設定用戶端上的 MessagePack</span><span class="sxs-lookup"><span data-stu-id="17849-121">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="17849-122">根據預設，支援的用戶端會啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="17849-122">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="17849-123">用戶端只能支援單一通訊協定。</span><span class="sxs-lookup"><span data-stu-id="17849-123">Clients can only support a single protocol.</span></span> <span data-ttu-id="17849-124">新增 MessagePack 支援將會取代任何先前設定的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="17849-124">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="17849-125">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="17849-125">.NET client</span></span>

<span data-ttu-id="17849-126">若要在 .NET 用戶端中啟用 MessagePack，請安裝 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 封裝，並 `AddMessagePackProtocol` 在上呼叫 `HubConnectionBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="17849-126">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chathub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="17849-127">此 `AddMessagePackProtocol` 呼叫會使用委派來設定選項，就像伺服器一樣。</span><span class="sxs-lookup"><span data-stu-id="17849-127">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="17849-128">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="17849-128">JavaScript client</span></span>

<span data-ttu-id="17849-129">Npm 套件會提供 JavaScript 用戶端的 MessagePack 支援 [@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack) 。</span><span class="sxs-lookup"><span data-stu-id="17849-129">MessagePack support for the JavaScript client is provided by the [@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack) npm package.</span></span> <span data-ttu-id="17849-130">在命令 shell 中執行下列命令，以安裝套件：</span><span class="sxs-lookup"><span data-stu-id="17849-130">Install the package by executing the following command in a command shell:</span></span>

```bash
npm install @microsoft/signalr-protocol-msgpack
```

<span data-ttu-id="17849-131">安裝 npm 套件之後，您可以直接透過 JavaScript 模組載入器使用模組，或藉由參考下列檔案，將其匯入至瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="17849-131">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the following file:</span></span>

<span data-ttu-id="17849-132">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="17849-132">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

<span data-ttu-id="17849-133">在瀏覽器中， `msgpack5` 也必須參考程式庫。</span><span class="sxs-lookup"><span data-stu-id="17849-133">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="17849-134">使用 `<script>` 標記來建立參考。</span><span class="sxs-lookup"><span data-stu-id="17849-134">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="17849-135">您可以在*node_modules\msgpack5\dist\msgpack5.js*找到此程式庫。</span><span class="sxs-lookup"><span data-stu-id="17849-135">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="17849-136">使用元素時 `<script>` ，順序很重要。</span><span class="sxs-lookup"><span data-stu-id="17849-136">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="17849-137">如果在*msgpack5.js*之前參考*signalr-protocol-msgpack.js* ，嘗試使用 MessagePack 連接時就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="17849-137">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="17849-138">*signalr-protocol-msgpack.js*之前也需要*signalr.js* 。</span><span class="sxs-lookup"><span data-stu-id="17849-138">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="17849-139">`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`將加入至 `HubConnectionBuilder` 會將用戶端設定為在連接到伺服器時使用 MessagePack 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="17849-139">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="17849-140">在此階段中，JavaScript 用戶端上沒有 MessagePack 通訊協定的設定選項。</span><span class="sxs-lookup"><span data-stu-id="17849-140">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="17849-141">MessagePack 的相容</span><span class="sxs-lookup"><span data-stu-id="17849-141">MessagePack quirks</span></span>

<span data-ttu-id="17849-142">使用 MessagePack Hub 通訊協定時，有幾個要注意的問題。</span><span class="sxs-lookup"><span data-stu-id="17849-142">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="17849-143">MessagePack 區分大小寫</span><span class="sxs-lookup"><span data-stu-id="17849-143">MessagePack is case-sensitive</span></span>

<span data-ttu-id="17849-144">MessagePack 通訊協定會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="17849-144">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="17849-145">例如，請考慮下列 c # 類別：</span><span class="sxs-lookup"><span data-stu-id="17849-145">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="17849-146">從 JavaScript 用戶端傳送時，您必須使用 `PascalCased` 屬性名稱，因為大小寫必須完全符合 c # 類別。</span><span class="sxs-lookup"><span data-stu-id="17849-146">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="17849-147">例如：</span><span class="sxs-lookup"><span data-stu-id="17849-147">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="17849-148">使用 `camelCased` 名稱並不會正確地系結至 c # 類別。</span><span class="sxs-lookup"><span data-stu-id="17849-148">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="17849-149">您可以使用 `Key` 屬性來指定不同的 MessagePack 屬性名稱，藉此解決此情況。</span><span class="sxs-lookup"><span data-stu-id="17849-149">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="17849-150">如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。</span><span class="sxs-lookup"><span data-stu-id="17849-150">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="17849-151">序列化/還原序列化時，不會保留 DateTime. Kind</span><span class="sxs-lookup"><span data-stu-id="17849-151">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="17849-152">MessagePack 通訊協定不會提供方法來編碼的 `Kind` 值 `DateTime` 。</span><span class="sxs-lookup"><span data-stu-id="17849-152">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="17849-153">如此一來，當還原序列化日期時，如果是，MessagePack 中樞通訊協定將會轉換成 UTC 格式， `DateTime.Kind` `DateTimeKind.Local` 否則將不會觸及時間並依原本傳遞。</span><span class="sxs-lookup"><span data-stu-id="17849-153">As a result, when deserializing a date, the MessagePack Hub Protocol will convert to the UTC format if the `DateTime.Kind` is `DateTimeKind.Local` otherwise it will not touch the time and pass it as is.</span></span> <span data-ttu-id="17849-154">如果您要使用值，建議您在傳送 `DateTime` 之前先轉換成 UTC。</span><span class="sxs-lookup"><span data-stu-id="17849-154">If you're working with `DateTime` values, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="17849-155">當您收到時，將它們從 UTC 轉換為當地時間。</span><span class="sxs-lookup"><span data-stu-id="17849-155">Convert them from UTC to local time when you receive them.</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="17849-156">JavaScript 中的 MessagePack 不支援 MinValue</span><span class="sxs-lookup"><span data-stu-id="17849-156">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="17849-157">JavaScript 用戶端所使用的[msgpack5](https://github.com/mcollina/msgpack5)程式庫 SignalR 不支援 `timestamp96` MessagePack 中的類型。</span><span class="sxs-lookup"><span data-stu-id="17849-157">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="17849-158">這個型別是用來編碼非常大的日期值（在過去或未來很久的時候）。</span><span class="sxs-lookup"><span data-stu-id="17849-158">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="17849-159">的值 `DateTime.MinValue` 是 `January 1, 0001` ，必須以 `timestamp96` 值編碼。</span><span class="sxs-lookup"><span data-stu-id="17849-159">The value of `DateTime.MinValue` is `January 1, 0001`, which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="17849-160">因此， `DateTime.MinValue` 不支援傳送至 JavaScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="17849-160">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="17849-161">當 `DateTime.MinValue` JavaScript 用戶端收到時，會擲回下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="17849-161">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="17849-162">通常 `DateTime.MinValue` 會使用來編碼「遺漏」或 `null` 值。</span><span class="sxs-lookup"><span data-stu-id="17849-162">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="17849-163">如果您需要在 MessagePack 中將該值編碼，請使用可為 null 的 `DateTime` 值（ `DateTime?` ），或編碼個別的 `bool` 值，以指出日期是否存在。</span><span class="sxs-lookup"><span data-stu-id="17849-163">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="17849-164">如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2228](https://github.com/aspnet/SignalR/issues/2228)。</span><span class="sxs-lookup"><span data-stu-id="17849-164">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="17849-165">「預先編譯」環境中的 MessagePack 支援</span><span class="sxs-lookup"><span data-stu-id="17849-165">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="17849-166">.NET 用戶端和伺服器使用的[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v2.1.90)程式庫會使用程式碼產生來優化序列化。</span><span class="sxs-lookup"><span data-stu-id="17849-166">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v2.1.90) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="17849-167">因此，在使用「預先」編譯的環境（例如 Xamarin iOS 或 Unity）上，預設不支援它。</span><span class="sxs-lookup"><span data-stu-id="17849-167">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="17849-168">在這些環境中，可以藉由「預先產生」序列化程式/還原序列化程式碼來使用 MessagePack。</span><span class="sxs-lookup"><span data-stu-id="17849-168">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="17849-169">如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp/tree/v2.1.90#aot-code-generation-to-support-unityxamarin)。</span><span class="sxs-lookup"><span data-stu-id="17849-169">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp/tree/v2.1.90#aot-code-generation-to-support-unityxamarin).</span></span> <span data-ttu-id="17849-170">當您預先產生序列化程式之後，您可以使用傳遞至的設定委派來進行註冊 `AddMessagePackProtocol` ：</span><span class="sxs-lookup"><span data-stu-id="17849-170">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        StaticCompositeResolver.Instance.Register(
            MessagePack.Resolvers.GeneratedResolver.Instance,
            MessagePack.Resolvers.StandardResolver.Instance
        );
        options.SerializerOptions = MessagePackSerializerOptions.Standard
            .WithResolver(StaticCompositeResolver.Instance)
            .WithSecurity(MessagePackSecurity.UntrustedData);
    });
```

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="17849-171">MessagePack 中的類型檢查較嚴格</span><span class="sxs-lookup"><span data-stu-id="17849-171">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="17849-172">JSON 中樞通訊協定會在還原序列化期間執行類型轉換。</span><span class="sxs-lookup"><span data-stu-id="17849-172">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="17849-173">例如，如果內送物件的屬性值是數位（ `{ foo: 42 }` ），但 .net 類別上的屬性類型為 `string` ，則會轉換此值。</span><span class="sxs-lookup"><span data-stu-id="17849-173">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="17849-174">不過，MessagePack 不會執行這項轉換，而且會擲回例外狀況，可在伺服器端記錄（和主控台）中看到：</span><span class="sxs-lookup"><span data-stu-id="17849-174">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="17849-175">如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2937](https://github.com/aspnet/SignalR/issues/2937)。</span><span class="sxs-lookup"><span data-stu-id="17849-175">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="17849-176">相關資源</span><span class="sxs-lookup"><span data-stu-id="17849-176">Related resources</span></span>

* [<span data-ttu-id="17849-177">開始使用</span><span class="sxs-lookup"><span data-stu-id="17849-177">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="17849-178">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="17849-178">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="17849-179">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="17849-179">JavaScript client</span></span>](xref:signalr/javascript-client)

::: moniker-end

::: moniker range=">= aspnetcore-3.0 < aspnetcore-5.0"

<span data-ttu-id="17849-180">本文假設讀者已熟悉[開始](xref:tutorials/signalr)使用中所涵蓋的主題。</span><span class="sxs-lookup"><span data-stu-id="17849-180">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="17849-181">什麼是 MessagePack？</span><span class="sxs-lookup"><span data-stu-id="17849-181">What is MessagePack?</span></span>

<span data-ttu-id="17849-182">[MessagePack](https://msgpack.org/index.html)是快速且精簡的二進位序列化格式。</span><span class="sxs-lookup"><span data-stu-id="17849-182">[MessagePack](https://msgpack.org/index.html) is a fast and compact binary serialization format.</span></span> <span data-ttu-id="17849-183">這在效能和頻寬有所顧慮時很有用，因為它會建立比[JSON](https://www.json.org/)更小的訊息。</span><span class="sxs-lookup"><span data-stu-id="17849-183">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="17849-184">在查看網路追蹤和記錄檔時，二進位訊息無法讀取，除非是透過 MessagePack 剖析器傳遞位元組。</span><span class="sxs-lookup"><span data-stu-id="17849-184">The binary messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> SignalR<span data-ttu-id="17849-185">具有 MessagePack 格式的內建支援，並提供 Api 供用戶端和伺服器使用。</span><span class="sxs-lookup"><span data-stu-id="17849-185"> has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="17849-186">在伺服器上設定 MessagePack</span><span class="sxs-lookup"><span data-stu-id="17849-186">Configure MessagePack on the server</span></span>

<span data-ttu-id="17849-187">若要在伺服器上啟用 MessagePack Hub 通訊協定，請 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 在您的應用程式中安裝套件。</span><span class="sxs-lookup"><span data-stu-id="17849-187">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="17849-188">在 `Startup.ConfigureServices` 方法中，將加入 `AddMessagePackProtocol` 至 `AddSignalR` 呼叫以在伺服器上啟用 MessagePack 支援。</span><span class="sxs-lookup"><span data-stu-id="17849-188">In the `Startup.ConfigureServices` method, add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="17849-189">預設會啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="17849-189">JSON is enabled by default.</span></span> <span data-ttu-id="17849-190">新增 MessagePack 可同時支援 JSON 和 MessagePack 用戶端。</span><span class="sxs-lookup"><span data-stu-id="17849-190">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="17849-191">若要自訂 MessagePack 如何將資料格式化，請 `AddMessagePackProtocol` 採用委派來設定選項。</span><span class="sxs-lookup"><span data-stu-id="17849-191">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="17849-192">在該委派中， `FormatterResolvers` 屬性可以用來設定 MessagePack 序列化選項。</span><span class="sxs-lookup"><span data-stu-id="17849-192">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="17849-193">如需解決器如何工作的詳細資訊，請造訪[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)上的 MessagePack 程式庫。</span><span class="sxs-lookup"><span data-stu-id="17849-193">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="17849-194">屬性可以在您要序列化的物件上使用，以定義它們的處理方式。</span><span class="sxs-lookup"><span data-stu-id="17849-194">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

> [!WARNING]
> <span data-ttu-id="17849-195">我們強烈建議您查看[CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) ，並套用建議的修補程式。</span><span class="sxs-lookup"><span data-stu-id="17849-195">We strongly recommend reviewing [CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) and applying the recommended patches.</span></span> <span data-ttu-id="17849-196">例如，將 `MessagePackSecurity.Active` 靜態屬性設定為 `MessagePackSecurity.UntrustedData` 。</span><span class="sxs-lookup"><span data-stu-id="17849-196">For example, setting the `MessagePackSecurity.Active` static property to `MessagePackSecurity.UntrustedData`.</span></span> <span data-ttu-id="17849-197">設定 `MessagePackSecurity.Active` 需要以手動方式安裝[1.9. x 版的 MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3)。</span><span class="sxs-lookup"><span data-stu-id="17849-197">Setting the `MessagePackSecurity.Active` requires manually installing a [1.9.x version of MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3).</span></span> <span data-ttu-id="17849-198">安裝 `MessagePack` 1.9. x 升級版本 SignalR 使用。</span><span class="sxs-lookup"><span data-stu-id="17849-198">Installing `MessagePack` 1.9.x upgrades the version SignalR uses.</span></span> <span data-ttu-id="17849-199">當不 `MessagePackSecurity.Active` 是設定為時 `MessagePackSecurity.UntrustedData` ，惡意用戶端可能會導致拒絕服務。</span><span class="sxs-lookup"><span data-stu-id="17849-199">When `MessagePackSecurity.Active` is not set to `MessagePackSecurity.UntrustedData`, a malicious client could cause a denial of service.</span></span> <span data-ttu-id="17849-200">`MessagePackSecurity.Active`在中設定 `Program.Main` ，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="17849-200">Set `MessagePackSecurity.Active` in `Program.Main`, as shown in the following code:</span></span>

```csharp
public static void Main(string[] args)
{
  MessagePackSecurity.Active = MessagePackSecurity.UntrustedData;

  CreateHostBuilder(args).Build().Run();
}
```

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="17849-201">設定用戶端上的 MessagePack</span><span class="sxs-lookup"><span data-stu-id="17849-201">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="17849-202">根據預設，支援的用戶端會啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="17849-202">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="17849-203">用戶端只能支援單一通訊協定。</span><span class="sxs-lookup"><span data-stu-id="17849-203">Clients can only support a single protocol.</span></span> <span data-ttu-id="17849-204">新增 MessagePack 支援將會取代任何先前設定的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="17849-204">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="17849-205">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="17849-205">.NET client</span></span>

<span data-ttu-id="17849-206">若要在 .NET 用戶端中啟用 MessagePack，請安裝 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 封裝，並 `AddMessagePackProtocol` 在上呼叫 `HubConnectionBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="17849-206">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chathub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="17849-207">此 `AddMessagePackProtocol` 呼叫會使用委派來設定選項，就像伺服器一樣。</span><span class="sxs-lookup"><span data-stu-id="17849-207">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="17849-208">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="17849-208">JavaScript client</span></span>

<span data-ttu-id="17849-209">Npm 套件會提供 JavaScript 用戶端的 MessagePack 支援 [@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack) 。</span><span class="sxs-lookup"><span data-stu-id="17849-209">MessagePack support for the JavaScript client is provided by the [@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack) npm package.</span></span> <span data-ttu-id="17849-210">在命令 shell 中執行下列命令，以安裝套件：</span><span class="sxs-lookup"><span data-stu-id="17849-210">Install the package by executing the following command in a command shell:</span></span>

```bash
npm install @microsoft/signalr-protocol-msgpack
```

<span data-ttu-id="17849-211">安裝 npm 套件之後，您可以直接透過 JavaScript 模組載入器使用模組，或藉由參考下列檔案，將其匯入至瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="17849-211">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the following file:</span></span>

<span data-ttu-id="17849-212">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="17849-212">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

<span data-ttu-id="17849-213">在瀏覽器中， `msgpack5` 也必須參考程式庫。</span><span class="sxs-lookup"><span data-stu-id="17849-213">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="17849-214">使用 `<script>` 標記來建立參考。</span><span class="sxs-lookup"><span data-stu-id="17849-214">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="17849-215">您可以在*node_modules\msgpack5\dist\msgpack5.js*找到此程式庫。</span><span class="sxs-lookup"><span data-stu-id="17849-215">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="17849-216">使用元素時 `<script>` ，順序很重要。</span><span class="sxs-lookup"><span data-stu-id="17849-216">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="17849-217">如果在*msgpack5.js*之前參考*signalr-protocol-msgpack.js* ，嘗試使用 MessagePack 連接時就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="17849-217">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="17849-218">*signalr-protocol-msgpack.js*之前也需要*signalr.js* 。</span><span class="sxs-lookup"><span data-stu-id="17849-218">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="17849-219">`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`將加入至 `HubConnectionBuilder` 會將用戶端設定為在連接到伺服器時使用 MessagePack 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="17849-219">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="17849-220">在此階段中，JavaScript 用戶端上沒有 MessagePack 通訊協定的設定選項。</span><span class="sxs-lookup"><span data-stu-id="17849-220">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="17849-221">MessagePack 的相容</span><span class="sxs-lookup"><span data-stu-id="17849-221">MessagePack quirks</span></span>

<span data-ttu-id="17849-222">使用 MessagePack Hub 通訊協定時，有幾個要注意的問題。</span><span class="sxs-lookup"><span data-stu-id="17849-222">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="17849-223">MessagePack 區分大小寫</span><span class="sxs-lookup"><span data-stu-id="17849-223">MessagePack is case-sensitive</span></span>

<span data-ttu-id="17849-224">MessagePack 通訊協定會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="17849-224">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="17849-225">例如，請考慮下列 c # 類別：</span><span class="sxs-lookup"><span data-stu-id="17849-225">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="17849-226">從 JavaScript 用戶端傳送時，您必須使用 `PascalCased` 屬性名稱，因為大小寫必須完全符合 c # 類別。</span><span class="sxs-lookup"><span data-stu-id="17849-226">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="17849-227">例如：</span><span class="sxs-lookup"><span data-stu-id="17849-227">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="17849-228">使用 `camelCased` 名稱並不會正確地系結至 c # 類別。</span><span class="sxs-lookup"><span data-stu-id="17849-228">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="17849-229">您可以使用 `Key` 屬性來指定不同的 MessagePack 屬性名稱，藉此解決此情況。</span><span class="sxs-lookup"><span data-stu-id="17849-229">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="17849-230">如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。</span><span class="sxs-lookup"><span data-stu-id="17849-230">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="17849-231">序列化/還原序列化時，不會保留 DateTime. Kind</span><span class="sxs-lookup"><span data-stu-id="17849-231">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="17849-232">MessagePack 通訊協定不會提供方法來編碼的 `Kind` 值 `DateTime` 。</span><span class="sxs-lookup"><span data-stu-id="17849-232">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="17849-233">因此，在還原序列化日期時，MessagePack 中樞通訊協定會假設內送日期是 UTC 格式。</span><span class="sxs-lookup"><span data-stu-id="17849-233">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="17849-234">如果您使用 `DateTime` 本機時間的值，建議您在傳送之前先轉換成 UTC。</span><span class="sxs-lookup"><span data-stu-id="17849-234">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="17849-235">當您收到時，將它們從 UTC 轉換為當地時間。</span><span class="sxs-lookup"><span data-stu-id="17849-235">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="17849-236">如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2632](https://github.com/aspnet/SignalR/issues/2632)。</span><span class="sxs-lookup"><span data-stu-id="17849-236">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="17849-237">JavaScript 中的 MessagePack 不支援 MinValue</span><span class="sxs-lookup"><span data-stu-id="17849-237">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="17849-238">JavaScript 用戶端所使用的[msgpack5](https://github.com/mcollina/msgpack5)程式庫 SignalR 不支援 `timestamp96` MessagePack 中的類型。</span><span class="sxs-lookup"><span data-stu-id="17849-238">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="17849-239">這個型別是用來編碼非常大的日期值（在過去或未來很久的時候）。</span><span class="sxs-lookup"><span data-stu-id="17849-239">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="17849-240">的值 `DateTime.MinValue` 是 `January 1, 0001` ，必須以 `timestamp96` 值編碼。</span><span class="sxs-lookup"><span data-stu-id="17849-240">The value of `DateTime.MinValue` is `January 1, 0001`, which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="17849-241">因此， `DateTime.MinValue` 不支援傳送至 JavaScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="17849-241">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="17849-242">當 `DateTime.MinValue` JavaScript 用戶端收到時，會擲回下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="17849-242">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="17849-243">通常 `DateTime.MinValue` 會使用來編碼「遺漏」或 `null` 值。</span><span class="sxs-lookup"><span data-stu-id="17849-243">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="17849-244">如果您需要在 MessagePack 中將該值編碼，請使用可為 null 的 `DateTime` 值（ `DateTime?` ），或編碼個別的 `bool` 值，以指出日期是否存在。</span><span class="sxs-lookup"><span data-stu-id="17849-244">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="17849-245">如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2228](https://github.com/aspnet/SignalR/issues/2228)。</span><span class="sxs-lookup"><span data-stu-id="17849-245">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="17849-246">「預先編譯」環境中的 MessagePack 支援</span><span class="sxs-lookup"><span data-stu-id="17849-246">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="17849-247">.NET 用戶端和伺服器使用的[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80)程式庫會使用程式碼產生來優化序列化。</span><span class="sxs-lookup"><span data-stu-id="17849-247">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="17849-248">因此，在使用「預先」編譯的環境（例如 Xamarin iOS 或 Unity）上，預設不支援它。</span><span class="sxs-lookup"><span data-stu-id="17849-248">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="17849-249">在這些環境中，可以藉由「預先產生」序列化程式/還原序列化程式碼來使用 MessagePack。</span><span class="sxs-lookup"><span data-stu-id="17849-249">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="17849-250">如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80#pre-code-generationunityxamarin-supports)。</span><span class="sxs-lookup"><span data-stu-id="17849-250">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="17849-251">當您預先產生序列化程式之後，您可以使用傳遞至的設定委派來進行註冊 `AddMessagePackProtocol` ：</span><span class="sxs-lookup"><span data-stu-id="17849-251">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="17849-252">MessagePack 中的類型檢查較嚴格</span><span class="sxs-lookup"><span data-stu-id="17849-252">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="17849-253">JSON 中樞通訊協定會在還原序列化期間執行類型轉換。</span><span class="sxs-lookup"><span data-stu-id="17849-253">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="17849-254">例如，如果內送物件的屬性值是數位（ `{ foo: 42 }` ），但 .net 類別上的屬性類型為 `string` ，則會轉換此值。</span><span class="sxs-lookup"><span data-stu-id="17849-254">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="17849-255">不過，MessagePack 不會執行這項轉換，而且會擲回例外狀況，可在伺服器端記錄（和主控台）中看到：</span><span class="sxs-lookup"><span data-stu-id="17849-255">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="17849-256">如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2937](https://github.com/aspnet/SignalR/issues/2937)。</span><span class="sxs-lookup"><span data-stu-id="17849-256">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="17849-257">相關資源</span><span class="sxs-lookup"><span data-stu-id="17849-257">Related resources</span></span>

* [<span data-ttu-id="17849-258">開始使用</span><span class="sxs-lookup"><span data-stu-id="17849-258">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="17849-259">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="17849-259">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="17849-260">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="17849-260">JavaScript client</span></span>](xref:signalr/javascript-client)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="17849-261">本文假設讀者已熟悉[開始](xref:tutorials/signalr)使用中所涵蓋的主題。</span><span class="sxs-lookup"><span data-stu-id="17849-261">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="17849-262">什麼是 MessagePack？</span><span class="sxs-lookup"><span data-stu-id="17849-262">What is MessagePack?</span></span>

<span data-ttu-id="17849-263">[MessagePack](https://msgpack.org/index.html)是快速且精簡的二進位序列化格式。</span><span class="sxs-lookup"><span data-stu-id="17849-263">[MessagePack](https://msgpack.org/index.html) is a fast and compact binary serialization format.</span></span> <span data-ttu-id="17849-264">這在效能和頻寬有所顧慮時很有用，因為它會建立比[JSON](https://www.json.org/)更小的訊息。</span><span class="sxs-lookup"><span data-stu-id="17849-264">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="17849-265">在查看網路追蹤和記錄檔時，二進位訊息無法讀取，除非是透過 MessagePack 剖析器傳遞位元組。</span><span class="sxs-lookup"><span data-stu-id="17849-265">The binary messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> SignalR<span data-ttu-id="17849-266">具有 MessagePack 格式的內建支援，並提供 Api 供用戶端和伺服器使用。</span><span class="sxs-lookup"><span data-stu-id="17849-266"> has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="17849-267">在伺服器上設定 MessagePack</span><span class="sxs-lookup"><span data-stu-id="17849-267">Configure MessagePack on the server</span></span>

<span data-ttu-id="17849-268">若要在伺服器上啟用 MessagePack Hub 通訊協定，請 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 在您的應用程式中安裝套件。</span><span class="sxs-lookup"><span data-stu-id="17849-268">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="17849-269">在 `Startup.ConfigureServices` 方法中，將加入 `AddMessagePackProtocol` 至 `AddSignalR` 呼叫以在伺服器上啟用 MessagePack 支援。</span><span class="sxs-lookup"><span data-stu-id="17849-269">In the `Startup.ConfigureServices` method, add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="17849-270">預設會啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="17849-270">JSON is enabled by default.</span></span> <span data-ttu-id="17849-271">新增 MessagePack 可同時支援 JSON 和 MessagePack 用戶端。</span><span class="sxs-lookup"><span data-stu-id="17849-271">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="17849-272">若要自訂 MessagePack 如何將資料格式化，請 `AddMessagePackProtocol` 採用委派來設定選項。</span><span class="sxs-lookup"><span data-stu-id="17849-272">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="17849-273">在該委派中， `FormatterResolvers` 屬性可以用來設定 MessagePack 序列化選項。</span><span class="sxs-lookup"><span data-stu-id="17849-273">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="17849-274">如需解決器如何工作的詳細資訊，請造訪[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)上的 MessagePack 程式庫。</span><span class="sxs-lookup"><span data-stu-id="17849-274">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="17849-275">屬性可以在您要序列化的物件上使用，以定義它們的處理方式。</span><span class="sxs-lookup"><span data-stu-id="17849-275">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

> [!WARNING]
> <span data-ttu-id="17849-276">我們強烈建議您查看[CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) ，並套用建議的修補程式。</span><span class="sxs-lookup"><span data-stu-id="17849-276">We strongly recommend reviewing [CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) and applying the recommended patches.</span></span> <span data-ttu-id="17849-277">例如，將 `MessagePackSecurity.Active` 靜態屬性設定為 `MessagePackSecurity.UntrustedData` 。</span><span class="sxs-lookup"><span data-stu-id="17849-277">For example, setting the `MessagePackSecurity.Active` static property to `MessagePackSecurity.UntrustedData`.</span></span> <span data-ttu-id="17849-278">設定 `MessagePackSecurity.Active` 需要以手動方式安裝[1.9. x 版的 MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3)。</span><span class="sxs-lookup"><span data-stu-id="17849-278">Setting the `MessagePackSecurity.Active` requires manually installing a [1.9.x version of MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3).</span></span> <span data-ttu-id="17849-279">安裝 `MessagePack` 1.9. x 升級版本 SignalR 使用。</span><span class="sxs-lookup"><span data-stu-id="17849-279">Installing `MessagePack` 1.9.x upgrades the version SignalR uses.</span></span> <span data-ttu-id="17849-280">當不 `MessagePackSecurity.Active` 是設定為時 `MessagePackSecurity.UntrustedData` ，惡意用戶端可能會導致拒絕服務。</span><span class="sxs-lookup"><span data-stu-id="17849-280">When `MessagePackSecurity.Active` is not set to `MessagePackSecurity.UntrustedData`, a malicious client could cause a denial of service.</span></span> <span data-ttu-id="17849-281">`MessagePackSecurity.Active`在中設定 `Program.Main` ，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="17849-281">Set `MessagePackSecurity.Active` in `Program.Main`, as shown in the following code:</span></span>

```csharp
public static void Main(string[] args)
{
  MessagePackSecurity.Active = MessagePackSecurity.UntrustedData;

  CreateHostBuilder(args).Build().Run();
}
```

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="17849-282">設定用戶端上的 MessagePack</span><span class="sxs-lookup"><span data-stu-id="17849-282">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="17849-283">根據預設，支援的用戶端會啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="17849-283">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="17849-284">用戶端只能支援單一通訊協定。</span><span class="sxs-lookup"><span data-stu-id="17849-284">Clients can only support a single protocol.</span></span> <span data-ttu-id="17849-285">新增 MessagePack 支援將會取代任何先前設定的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="17849-285">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="17849-286">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="17849-286">.NET client</span></span>

<span data-ttu-id="17849-287">若要在 .NET 用戶端中啟用 MessagePack，請安裝 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 封裝，並 `AddMessagePackProtocol` 在上呼叫 `HubConnectionBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="17849-287">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chathub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="17849-288">此 `AddMessagePackProtocol` 呼叫會使用委派來設定選項，就像伺服器一樣。</span><span class="sxs-lookup"><span data-stu-id="17849-288">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="17849-289">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="17849-289">JavaScript client</span></span>

<span data-ttu-id="17849-290">Npm 套件會提供 JavaScript 用戶端的 MessagePack 支援 [@aspnet/signalr-protocol-msgpack](https://www.npmjs.com/package/@aspnet/signalr-protocol-msgpack) 。</span><span class="sxs-lookup"><span data-stu-id="17849-290">MessagePack support for the JavaScript client is provided by the [@aspnet/signalr-protocol-msgpack](https://www.npmjs.com/package/@aspnet/signalr-protocol-msgpack) npm package.</span></span> <span data-ttu-id="17849-291">在命令 shell 中執行下列命令，以安裝套件：</span><span class="sxs-lookup"><span data-stu-id="17849-291">Install the package by executing the following command in a command shell:</span></span>

```bash
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="17849-292">安裝 npm 套件之後，您可以直接透過 JavaScript 模組載入器使用模組，或藉由參考下列檔案，將其匯入至瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="17849-292">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the following file:</span></span>

<span data-ttu-id="17849-293">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="17849-293">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span>

<span data-ttu-id="17849-294">在瀏覽器中， `msgpack5` 也必須參考程式庫。</span><span class="sxs-lookup"><span data-stu-id="17849-294">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="17849-295">使用 `<script>` 標記來建立參考。</span><span class="sxs-lookup"><span data-stu-id="17849-295">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="17849-296">您可以在*node_modules\msgpack5\dist\msgpack5.js*找到此程式庫。</span><span class="sxs-lookup"><span data-stu-id="17849-296">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="17849-297">使用元素時 `<script>` ，順序很重要。</span><span class="sxs-lookup"><span data-stu-id="17849-297">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="17849-298">如果在*msgpack5.js*之前參考*signalr-protocol-msgpack.js* ，嘗試使用 MessagePack 連接時就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="17849-298">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="17849-299">*signalr-protocol-msgpack.js*之前也需要*signalr.js* 。</span><span class="sxs-lookup"><span data-stu-id="17849-299">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="17849-300">`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`將加入至 `HubConnectionBuilder` 會將用戶端設定為在連接到伺服器時使用 MessagePack 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="17849-300">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="17849-301">在此階段中，JavaScript 用戶端上沒有 MessagePack 通訊協定的設定選項。</span><span class="sxs-lookup"><span data-stu-id="17849-301">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="17849-302">MessagePack 的相容</span><span class="sxs-lookup"><span data-stu-id="17849-302">MessagePack quirks</span></span>

<span data-ttu-id="17849-303">使用 MessagePack Hub 通訊協定時，有幾個要注意的問題。</span><span class="sxs-lookup"><span data-stu-id="17849-303">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="17849-304">MessagePack 區分大小寫</span><span class="sxs-lookup"><span data-stu-id="17849-304">MessagePack is case-sensitive</span></span>

<span data-ttu-id="17849-305">MessagePack 通訊協定會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="17849-305">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="17849-306">例如，請考慮下列 c # 類別：</span><span class="sxs-lookup"><span data-stu-id="17849-306">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="17849-307">從 JavaScript 用戶端傳送時，您必須使用 `PascalCased` 屬性名稱，因為大小寫必須完全符合 c # 類別。</span><span class="sxs-lookup"><span data-stu-id="17849-307">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="17849-308">例如：</span><span class="sxs-lookup"><span data-stu-id="17849-308">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="17849-309">使用 `camelCased` 名稱並不會正確地系結至 c # 類別。</span><span class="sxs-lookup"><span data-stu-id="17849-309">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="17849-310">您可以使用 `Key` 屬性來指定不同的 MessagePack 屬性名稱，藉此解決此情況。</span><span class="sxs-lookup"><span data-stu-id="17849-310">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="17849-311">如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。</span><span class="sxs-lookup"><span data-stu-id="17849-311">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="17849-312">序列化/還原序列化時，不會保留 DateTime. Kind</span><span class="sxs-lookup"><span data-stu-id="17849-312">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="17849-313">MessagePack 通訊協定不會提供方法來編碼的 `Kind` 值 `DateTime` 。</span><span class="sxs-lookup"><span data-stu-id="17849-313">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="17849-314">因此，在還原序列化日期時，MessagePack 中樞通訊協定會假設內送日期是 UTC 格式。</span><span class="sxs-lookup"><span data-stu-id="17849-314">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="17849-315">如果您使用 `DateTime` 本機時間的值，建議您在傳送之前先轉換成 UTC。</span><span class="sxs-lookup"><span data-stu-id="17849-315">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="17849-316">當您收到時，將它們從 UTC 轉換為當地時間。</span><span class="sxs-lookup"><span data-stu-id="17849-316">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="17849-317">如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2632](https://github.com/aspnet/SignalR/issues/2632)。</span><span class="sxs-lookup"><span data-stu-id="17849-317">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="17849-318">JavaScript 中的 MessagePack 不支援 MinValue</span><span class="sxs-lookup"><span data-stu-id="17849-318">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="17849-319">JavaScript 用戶端所使用的[msgpack5](https://github.com/mcollina/msgpack5)程式庫 SignalR 不支援 `timestamp96` MessagePack 中的類型。</span><span class="sxs-lookup"><span data-stu-id="17849-319">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="17849-320">這個型別是用來編碼非常大的日期值（在過去或未來很久的時候）。</span><span class="sxs-lookup"><span data-stu-id="17849-320">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="17849-321">的值 `DateTime.MinValue` `January 1, 0001` 必須以 `timestamp96` 值編碼。</span><span class="sxs-lookup"><span data-stu-id="17849-321">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="17849-322">因此， `DateTime.MinValue` 不支援傳送至 JavaScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="17849-322">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="17849-323">當 `DateTime.MinValue` JavaScript 用戶端收到時，會擲回下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="17849-323">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="17849-324">通常 `DateTime.MinValue` 會使用來編碼「遺漏」或 `null` 值。</span><span class="sxs-lookup"><span data-stu-id="17849-324">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="17849-325">如果您需要在 MessagePack 中將該值編碼，請使用可為 null 的 `DateTime` 值（ `DateTime?` ），或編碼個別的 `bool` 值，以指出日期是否存在。</span><span class="sxs-lookup"><span data-stu-id="17849-325">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="17849-326">如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2228](https://github.com/aspnet/SignalR/issues/2228)。</span><span class="sxs-lookup"><span data-stu-id="17849-326">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="17849-327">「預先編譯」環境中的 MessagePack 支援</span><span class="sxs-lookup"><span data-stu-id="17849-327">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="17849-328">.NET 用戶端和伺服器使用的[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80)程式庫會使用程式碼產生來優化序列化。</span><span class="sxs-lookup"><span data-stu-id="17849-328">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="17849-329">因此，在使用「預先」編譯的環境（例如 Xamarin iOS 或 Unity）上，預設不支援它。</span><span class="sxs-lookup"><span data-stu-id="17849-329">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="17849-330">在這些環境中，可以藉由「預先產生」序列化程式/還原序列化程式碼來使用 MessagePack。</span><span class="sxs-lookup"><span data-stu-id="17849-330">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="17849-331">如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80#pre-code-generationunityxamarin-supports)。</span><span class="sxs-lookup"><span data-stu-id="17849-331">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="17849-332">當您預先產生序列化程式之後，您可以使用傳遞至的設定委派來進行註冊 `AddMessagePackProtocol` ：</span><span class="sxs-lookup"><span data-stu-id="17849-332">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="17849-333">MessagePack 中的類型檢查較嚴格</span><span class="sxs-lookup"><span data-stu-id="17849-333">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="17849-334">JSON 中樞通訊協定會在還原序列化期間執行類型轉換。</span><span class="sxs-lookup"><span data-stu-id="17849-334">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="17849-335">例如，如果內送物件的屬性值是數位（ `{ foo: 42 }` ），但 .net 類別上的屬性類型為 `string` ，則會轉換此值。</span><span class="sxs-lookup"><span data-stu-id="17849-335">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="17849-336">不過，MessagePack 不會執行這項轉換，而且會擲回例外狀況，可在伺服器端記錄（和主控台）中看到：</span><span class="sxs-lookup"><span data-stu-id="17849-336">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="17849-337">如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2937](https://github.com/aspnet/SignalR/issues/2937)。</span><span class="sxs-lookup"><span data-stu-id="17849-337">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="17849-338">相關資源</span><span class="sxs-lookup"><span data-stu-id="17849-338">Related resources</span></span>

* [<span data-ttu-id="17849-339">開始使用</span><span class="sxs-lookup"><span data-stu-id="17849-339">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="17849-340">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="17849-340">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="17849-341">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="17849-341">JavaScript client</span></span>](xref:signalr/javascript-client)

::: moniker-end
