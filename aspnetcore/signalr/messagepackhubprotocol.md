---
title: 使用 ASP.NET Core SignalR MessagePack 中樞通訊協定
author: bradygaster
description: 加入 ASP.NET Core SignalR MessagePack 中樞通訊協定。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/27/2019
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 7742f6f8bb53fb3c299ff98ae52a0da519ff396c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896515"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="b73a5-103">使用 ASP.NET Core SignalR MessagePack 中樞通訊協定</span><span class="sxs-lookup"><span data-stu-id="b73a5-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="b73a5-104">由[brennan Conroy](https://github.com/BrennanConroy)提供</span><span class="sxs-lookup"><span data-stu-id="b73a5-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="b73a5-105">本文假設讀者已熟悉所涵蓋的主題[開始](xref:tutorials/signalr)。</span><span class="sxs-lookup"><span data-stu-id="b73a5-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="b73a5-106">MessagePack 是什麼？</span><span class="sxs-lookup"><span data-stu-id="b73a5-106">What is MessagePack?</span></span>

<span data-ttu-id="b73a5-107">[MessagePack](https://msgpack.org/index.html)是快速且精簡的二進位序列化格式。</span><span class="sxs-lookup"><span data-stu-id="b73a5-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="b73a5-108">效能和頻寬是需要考量因為它會建立相較於較小的訊息時很有用[JSON](https://www.json.org/)。</span><span class="sxs-lookup"><span data-stu-id="b73a5-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="b73a5-109">因為它是一種二進位格式時，如果除非位元組都會通過 MessagePack 剖析器，查看網路追蹤和記錄檔訊息就無法讀取。</span><span class="sxs-lookup"><span data-stu-id="b73a5-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="b73a5-110">SignalR 的 MessagePack 格式中，內建支援，並提供用戶端和伺服器使用的 Api。</span><span class="sxs-lookup"><span data-stu-id="b73a5-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="b73a5-111">在伺服器上設定 MessagePack</span><span class="sxs-lookup"><span data-stu-id="b73a5-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="b73a5-112">若要啟用 MessagePack 中樞通訊協定，在伺服器上，安裝`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`應用程式中的封裝。</span><span class="sxs-lookup"><span data-stu-id="b73a5-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="b73a5-113">在 Startup.cs 檔案中新增`AddMessagePackProtocol`至`AddSignalR`啟用 MessagePack 支援在伺服器上的呼叫。</span><span class="sxs-lookup"><span data-stu-id="b73a5-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="b73a5-114">預設會啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="b73a5-114">JSON is enabled by default.</span></span> <span data-ttu-id="b73a5-115">新增 MessagePack 啟用 JSON 和 MessagePack 用戶端的支援。</span><span class="sxs-lookup"><span data-stu-id="b73a5-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="b73a5-116">若要自訂 MessagePack 會列印文件的格式，請您的資料，`AddMessagePackProtocol`會接受委派的設定選項。</span><span class="sxs-lookup"><span data-stu-id="b73a5-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="b73a5-117">在該委派，`FormatterResolvers`屬性可用來設定 MessagePack 序列化選項。</span><span class="sxs-lookup"><span data-stu-id="b73a5-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="b73a5-118">如需有關如何解析程式的運作方式的詳細資訊，請瀏覽 MessagePack library [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp)。</span><span class="sxs-lookup"><span data-stu-id="b73a5-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="b73a5-119">在您想要以定義他們應該如何處理序列化的物件上可用的屬性。</span><span class="sxs-lookup"><span data-stu-id="b73a5-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="b73a5-120">用戶端上設定 MessagePack</span><span class="sxs-lookup"><span data-stu-id="b73a5-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="b73a5-121">預設會啟用 JSON 支援的用戶端。</span><span class="sxs-lookup"><span data-stu-id="b73a5-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="b73a5-122">用戶端只能支援單一通訊協定。</span><span class="sxs-lookup"><span data-stu-id="b73a5-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="b73a5-123">新增 MessagePack 支援取代任何先前設定的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="b73a5-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="b73a5-124">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="b73a5-124">.NET client</span></span>

<span data-ttu-id="b73a5-125">若要啟用 MessagePack.NET 用戶端中的，安裝`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`封裝並呼叫`AddMessagePackProtocol`上`HubConnectionBuilder`。</span><span class="sxs-lookup"><span data-stu-id="b73a5-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="b73a5-126">這`AddMessagePackProtocol`呼叫會接受委派的設定選項，就像伺服器。</span><span class="sxs-lookup"><span data-stu-id="b73a5-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="b73a5-127">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="b73a5-127">JavaScript client</span></span>

<span data-ttu-id="b73a5-128">會提供 JavaScript 用戶端的 MessagePack 支援`@aspnet/signalr-protocol-msgpack`npm 套件。</span><span class="sxs-lookup"><span data-stu-id="b73a5-128">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="b73a5-129">安裝 npm 套件之後，此模組可以直接透過 JavaScript 模組載入器或藉由參考匯入至瀏覽器 *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b73a5-129">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="b73a5-130">在瀏覽器，`msgpack5`也必須參考程式庫。</span><span class="sxs-lookup"><span data-stu-id="b73a5-130">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="b73a5-131">使用`<script>`標記，以建立參考。</span><span class="sxs-lookup"><span data-stu-id="b73a5-131">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="b73a5-132">程式庫，請參閱*node_modules\msgpack5\dist\msgpack5.js*。</span><span class="sxs-lookup"><span data-stu-id="b73a5-132">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="b73a5-133">當使用`<script>`元素的順序很重要。</span><span class="sxs-lookup"><span data-stu-id="b73a5-133">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="b73a5-134">如果*signalr-protocol-msgpack.js*參考之前*msgpack5.js*，嘗試使用 MessagePack 連線時，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="b73a5-134">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="b73a5-135">*signalr.js*之前，也需要*signalr-protocol-msgpack.js*。</span><span class="sxs-lookup"><span data-stu-id="b73a5-135">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="b73a5-136">新增`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`至`HubConnectionBuilder`會設定用戶端連接到伺服器時，使用 MessagePack 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="b73a5-136">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="b73a5-137">在此階段中，有 MessagePack 通訊協定在 JavaScript 用戶端上的沒有組態選項。</span><span class="sxs-lookup"><span data-stu-id="b73a5-137">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="b73a5-138">MessagePack quirks</span><span class="sxs-lookup"><span data-stu-id="b73a5-138">MessagePack quirks</span></span>

<span data-ttu-id="b73a5-139">有幾個問題時要注意的使用 MessagePack 中樞通訊協定。</span><span class="sxs-lookup"><span data-stu-id="b73a5-139">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="b73a5-140">MessagePack 會區分大小寫</span><span class="sxs-lookup"><span data-stu-id="b73a5-140">MessagePack is case-sensitive</span></span>

<span data-ttu-id="b73a5-141">MessagePack 通訊協定會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="b73a5-141">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="b73a5-142">例如，請考慮下列C#類別：</span><span class="sxs-lookup"><span data-stu-id="b73a5-142">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="b73a5-143">當從 JavaScript 用戶端傳送，您必須使用`PascalCased`屬性的名稱，因為大小寫必須符合C#完全類別。</span><span class="sxs-lookup"><span data-stu-id="b73a5-143">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="b73a5-144">例如：</span><span class="sxs-lookup"><span data-stu-id="b73a5-144">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="b73a5-145">使用`camelCased`名稱不會正確地繫結至C#類別。</span><span class="sxs-lookup"><span data-stu-id="b73a5-145">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="b73a5-146">使用來解決這個`Key`屬性來指定 MessagePack 屬性不同的名稱。</span><span class="sxs-lookup"><span data-stu-id="b73a5-146">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="b73a5-147">如需詳細資訊，請參閱 < [MessagePack CSharp 文件](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。</span><span class="sxs-lookup"><span data-stu-id="b73a5-147">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="b73a5-148">當序列化/還原序列化時，不會保留 DateTime.Kind</span><span class="sxs-lookup"><span data-stu-id="b73a5-148">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="b73a5-149">MessagePack 通訊協定不提供編碼方式`Kind`的值`DateTime`。</span><span class="sxs-lookup"><span data-stu-id="b73a5-149">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="b73a5-150">如此一來，還原序列化時的日期，MessagePack 中樞通訊協定會假設連入的日期是以 UTC 格式。</span><span class="sxs-lookup"><span data-stu-id="b73a5-150">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="b73a5-151">如果您正在使用`DateTime`當地時間的值，我們建議您將轉換為 UTC，然後才傳送。</span><span class="sxs-lookup"><span data-stu-id="b73a5-151">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="b73a5-152">它們從 UTC 轉換為當地時間時接收它們。</span><span class="sxs-lookup"><span data-stu-id="b73a5-152">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="b73a5-153">如需有關這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/SignalR # 2632年](https://github.com/aspnet/SignalR/issues/2632)。</span><span class="sxs-lookup"><span data-stu-id="b73a5-153">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="b73a5-154">MessagePack 在 JavaScript 中不支援 DateTime.MinValue</span><span class="sxs-lookup"><span data-stu-id="b73a5-154">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="b73a5-155">[Msgpack5](https://github.com/mcollina/msgpack5) SignalR JavaScript 用戶端所使用的程式庫不支援`timestamp96`MessagePack 中的型別。</span><span class="sxs-lookup"><span data-stu-id="b73a5-155">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="b73a5-156">此類型用來編碼 （無論是使用極早期在過去或未來很遠） 非常大的日期值。</span><span class="sxs-lookup"><span data-stu-id="b73a5-156">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="b73a5-157">值`DateTime.MinValue`已`January 1, 0001`這必須在編碼`timestamp96`值。</span><span class="sxs-lookup"><span data-stu-id="b73a5-157">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="b73a5-158">因為此，傳送`DateTime.MinValue`javascript 用戶端不支援。</span><span class="sxs-lookup"><span data-stu-id="b73a5-158">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="b73a5-159">當`DateTime.MinValue`收到 JavaScript 用戶端，會擲回下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="b73a5-159">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="b73a5-160">通常`DateTime.MinValue`用來編碼的 「 遺漏 」 或`null`值。</span><span class="sxs-lookup"><span data-stu-id="b73a5-160">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="b73a5-161">如果您要編碼 MessagePack 中的值，請使用可為 null`DateTime`值 (`DateTime?`) 或編碼個別`bool`值，指出日期是否存在。</span><span class="sxs-lookup"><span data-stu-id="b73a5-161">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="b73a5-162">如需有關這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/SignalR # 2228年](https://github.com/aspnet/SignalR/issues/2228)。</span><span class="sxs-lookup"><span data-stu-id="b73a5-162">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="b73a5-163">MessagePack 「 預先-「 編譯環境中的支援</span><span class="sxs-lookup"><span data-stu-id="b73a5-163">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="b73a5-164">[MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp) .NET 用戶端和伺服器所使用的程式庫會使用程式碼產生最佳化序列化。</span><span class="sxs-lookup"><span data-stu-id="b73a5-164">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="b73a5-165">如此一來，它不支援使用 「 預先-「 編譯 （例如 Xamarin iOS 或 Unity） 的環境上的預設值。</span><span class="sxs-lookup"><span data-stu-id="b73a5-165">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="b73a5-166">可以在這些環境中使用 MessagePack，藉由 「 預先產生"序列化/還原序列化程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="b73a5-166">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="b73a5-167">如需詳細資訊，請參閱 < [MessagePack CSharp 文件](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports)。</span><span class="sxs-lookup"><span data-stu-id="b73a5-167">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="b73a5-168">一旦您預先產生序列化程式，您可以將它們註冊使用傳遞至設定委派`AddMessagePackProtocol`:</span><span class="sxs-lookup"><span data-stu-id="b73a5-168">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="b73a5-169">型別檢查為 MessagePack 中更嚴格</span><span class="sxs-lookup"><span data-stu-id="b73a5-169">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="b73a5-170">JSON 中樞通訊協定會在還原序列化期間執行型別轉換。</span><span class="sxs-lookup"><span data-stu-id="b73a5-170">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="b73a5-171">例如，如果連入物件的屬性值是數字 (`{ foo: 42 }`)，但在.NET 類別上的屬性的類型是`string`，值會轉換。</span><span class="sxs-lookup"><span data-stu-id="b73a5-171">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="b73a5-172">不過，MessagePack 不會執行這項轉換，而且將會擲回的例外狀況，您所見，在伺服器端記錄檔 （而在主控台中）：</span><span class="sxs-lookup"><span data-stu-id="b73a5-172">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="b73a5-173">如需有關這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/SignalR # 2937年](https://github.com/aspnet/SignalR/issues/2937)。</span><span class="sxs-lookup"><span data-stu-id="b73a5-173">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="b73a5-174">相關資源</span><span class="sxs-lookup"><span data-stu-id="b73a5-174">Related resources</span></span>

* [<span data-ttu-id="b73a5-175">開始使用</span><span class="sxs-lookup"><span data-stu-id="b73a5-175">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="b73a5-176">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="b73a5-176">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="b73a5-177">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="b73a5-177">JavaScript client</span></span>](xref:signalr/javascript-client)
