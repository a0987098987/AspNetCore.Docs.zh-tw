---
title: 在 ASP.NET 核心SignalR中使用 訊息套件集線器協定
author: bradygaster
description: 將訊息套件集線器協定加入SignalRASP.NET 核心 。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/13/2020
no-loc:
- SignalR
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: bbc34d790387a96bb3b6f75e841b45685eb137ce
ms.sourcegitcommit: 5af16166977da598953f82da3ed3b7712d38f6cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81277232"
---
# <a name="use-messagepack-hub-protocol-in-opno-locsignalr-for-aspnet-core"></a><span data-ttu-id="e18ce-103">在 ASP.NET 核心SignalR中使用 訊息套件集線器協定</span><span class="sxs-lookup"><span data-stu-id="e18ce-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="e18ce-104">本文假定讀者[熟悉入門中](xref:tutorials/signalr)介紹的主題。</span><span class="sxs-lookup"><span data-stu-id="e18ce-104">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="e18ce-105">什麼是消息包?</span><span class="sxs-lookup"><span data-stu-id="e18ce-105">What is MessagePack?</span></span>

<span data-ttu-id="e18ce-106">[MessagePack](https://msgpack.org/index.html)是一種快速緊湊的二進位序列化格式。</span><span class="sxs-lookup"><span data-stu-id="e18ce-106">[MessagePack](https://msgpack.org/index.html) is a fast and compact binary serialization format.</span></span> <span data-ttu-id="e18ce-107">當性能和頻寬是一個問題時,它很有用,因為它創建的消息比[JSON](https://www.json.org/)少。</span><span class="sxs-lookup"><span data-stu-id="e18ce-107">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="e18ce-108">二進位訊息在查看網路追蹤和日誌時不可讀,除非位元組透過 MessagePack 解析器傳遞。</span><span class="sxs-lookup"><span data-stu-id="e18ce-108">The binary messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> SignalR<span data-ttu-id="e18ce-109">具有對 MessagePack 格式的內建支援,並為用戶端和伺服器提供了要使用的 API。</span><span class="sxs-lookup"><span data-stu-id="e18ce-109"> has built-in support for the MessagePack format and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="e18ce-110">在伺服器上設定訊息套件</span><span class="sxs-lookup"><span data-stu-id="e18ce-110">Configure MessagePack on the server</span></span>

<span data-ttu-id="e18ce-111">要在伺服器上啟用 MessagePack 中心協定,`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`請在應用中安裝包。</span><span class="sxs-lookup"><span data-stu-id="e18ce-111">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="e18ce-112">在`Startup.ConfigureServices`方法中,`AddMessagePackProtocol`添加`AddSignalR`到調用以在伺服器上啟用 MessagePack 支援。</span><span class="sxs-lookup"><span data-stu-id="e18ce-112">In the `Startup.ConfigureServices` method, add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="e18ce-113">默認情況下啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="e18ce-113">JSON is enabled by default.</span></span> <span data-ttu-id="e18ce-114">添加消息包支援 JSON 和 MessagePack 用戶端。</span><span class="sxs-lookup"><span data-stu-id="e18ce-114">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="e18ce-115">要自訂 MessagePack 如何格式化`AddMessagePackProtocol`資料, 請取得用於設定選項的委託。</span><span class="sxs-lookup"><span data-stu-id="e18ce-115">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="e18ce-116">在該委託中,`SerializerOptions`該屬性可用於配置 MessagePack 序列化選項。</span><span class="sxs-lookup"><span data-stu-id="e18ce-116">In that delegate, the `SerializerOptions` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="e18ce-117">有關解析器如何工作的詳細資訊,請造[訪 MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)中的 MessagePack 庫。</span><span class="sxs-lookup"><span data-stu-id="e18ce-117">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="e18ce-118">屬性可用於要序列化的物件,以定義如何處理它們。</span><span class="sxs-lookup"><span data-stu-id="e18ce-118">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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
> <span data-ttu-id="e18ce-119">我們強烈建議查看[CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf)並應用建議的修補程式。</span><span class="sxs-lookup"><span data-stu-id="e18ce-119">We strongly recommend reviewing [CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) and applying the recommended patches.</span></span> <span data-ttu-id="e18ce-120">例如,取代`.WithSecurity(MessagePackSecurity.UntrustedData)``SerializerOptions`時呼叫 。</span><span class="sxs-lookup"><span data-stu-id="e18ce-120">For example, calling `.WithSecurity(MessagePackSecurity.UntrustedData)` when replacing the `SerializerOptions`.</span></span>

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="e18ce-121">在用戶端上設定訊息套件</span><span class="sxs-lookup"><span data-stu-id="e18ce-121">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="e18ce-122">默認情況下,為支援的用戶端啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="e18ce-122">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="e18ce-123">用戶端只能支援單個協定。</span><span class="sxs-lookup"><span data-stu-id="e18ce-123">Clients can only support a single protocol.</span></span> <span data-ttu-id="e18ce-124">添加 MessagePack 支援將替換任何以前配置的協定。</span><span class="sxs-lookup"><span data-stu-id="e18ce-124">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="e18ce-125">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="e18ce-125">.NET client</span></span>

<span data-ttu-id="e18ce-126">在 .NET 客戶端開啟 MessagePack,`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`請安裝`AddMessagePackProtocol`套件`HubConnectionBuilder`並呼叫 。</span><span class="sxs-lookup"><span data-stu-id="e18ce-126">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="e18ce-127">此`AddMessagePackProtocol`調用需要一個委託來配置類似於伺服器的選項。</span><span class="sxs-lookup"><span data-stu-id="e18ce-127">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="e18ce-128">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="e18ce-128">JavaScript client</span></span>

<span data-ttu-id="e18ce-129">JavaScript 用戶端的消息包支援[@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack)由 npm 包提供。</span><span class="sxs-lookup"><span data-stu-id="e18ce-129">MessagePack support for the JavaScript client is provided by the [@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack) npm package.</span></span> <span data-ttu-id="e18ce-130">透過在命令外殼中執行以下命令來安裝套件:</span><span class="sxs-lookup"><span data-stu-id="e18ce-130">Install the package by executing the following command in a command shell:</span></span>

```bash
npm install @microsoft/signalr-protocol-msgpack
```

<span data-ttu-id="e18ce-131">安裝 npm 套件後,模組可以直接透過 JavaScript 模組載入程式使用,或者透過參考以下檔案匯入瀏覽器:</span><span class="sxs-lookup"><span data-stu-id="e18ce-131">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the following file:</span></span>

<span data-ttu-id="e18ce-132">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="e18ce-132">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

<span data-ttu-id="e18ce-133">在瀏覽器中,`msgpack5`還必須引用庫。</span><span class="sxs-lookup"><span data-stu-id="e18ce-133">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="e18ce-134">使用標記`<script>`創建引用。</span><span class="sxs-lookup"><span data-stu-id="e18ce-134">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="e18ce-135">庫可以在*node_modules_msgpack5\dist_msgpack5.js*中找到。</span><span class="sxs-lookup"><span data-stu-id="e18ce-135">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="e18ce-136">使用元素`<script>`時,順序很重要。</span><span class="sxs-lookup"><span data-stu-id="e18ce-136">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="e18ce-137">如果在*msgpack5.js*之前引用*訊號器協定-msgpack.js,* 則嘗試與 MessagePack 連接時會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="e18ce-137">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="e18ce-138">*信號器.js*在*信號器協定-msgpack.js*之前也需要信號。</span><span class="sxs-lookup"><span data-stu-id="e18ce-138">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="e18ce-139">將`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())``HubConnectionBuilder`用戶端配置為在連接到伺服器時使用 MessagePack 協定。</span><span class="sxs-lookup"><span data-stu-id="e18ce-139">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="e18ce-140">此時,JavaScript 用戶端上的 MessagePack 協定沒有配置選項。</span><span class="sxs-lookup"><span data-stu-id="e18ce-140">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="e18ce-141">訊息包怪癖</span><span class="sxs-lookup"><span data-stu-id="e18ce-141">MessagePack quirks</span></span>

<span data-ttu-id="e18ce-142">使用 MessagePack 中心協定時,需要注意一些問題。</span><span class="sxs-lookup"><span data-stu-id="e18ce-142">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="e18ce-143">訊息套件區分大小寫</span><span class="sxs-lookup"><span data-stu-id="e18ce-143">MessagePack is case-sensitive</span></span>

<span data-ttu-id="e18ce-144">MessagePack 協定區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="e18ce-144">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="e18ce-145">例如,請考慮以下 C# 類:</span><span class="sxs-lookup"><span data-stu-id="e18ce-145">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="e18ce-146">從 JavaScript 客戶端發送時`PascalCased`,必須使用 屬性名稱,因為大小寫必須與 C# 類完全匹配。</span><span class="sxs-lookup"><span data-stu-id="e18ce-146">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="e18ce-147">例如：</span><span class="sxs-lookup"><span data-stu-id="e18ce-147">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="e18ce-148">使用`camelCased`名稱不會正確綁定到 C# 類。</span><span class="sxs-lookup"><span data-stu-id="e18ce-148">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="e18ce-149">可以使用`Key`屬性為 MessagePack 屬性指定其他名稱來解決此問題。</span><span class="sxs-lookup"><span data-stu-id="e18ce-149">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="e18ce-150">有關詳細資訊,請參閱[MessagePack-CSharp 文件](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-150">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="e18ce-151">序列化/反序列化時不保留日期時間.Kind</span><span class="sxs-lookup"><span data-stu-id="e18ce-151">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="e18ce-152">MessagePack 協定不提供`Kind`對 的值進行編碼`DateTime`的方法。</span><span class="sxs-lookup"><span data-stu-id="e18ce-152">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="e18ce-153">因此,在取消序列化日期時,MessagePack 中心協定將轉換為 UTC`DateTime.Kind``DateTimeKind.Local`格式, 否則不會觸控時間並按現在的方式傳遞它。</span><span class="sxs-lookup"><span data-stu-id="e18ce-153">As a result, when deserializing a date, the MessagePack Hub Protocol will convert to the UTC format if the `DateTime.Kind` is `DateTimeKind.Local` otherwise it will not touch the time and pass it as is.</span></span> <span data-ttu-id="e18ce-154">如果您正在使用`DateTime`值,我們建議您在發送值之前轉換為 UTC。</span><span class="sxs-lookup"><span data-stu-id="e18ce-154">If you're working with `DateTime` values, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="e18ce-155">當您收到它們時,將它們從UTC轉換為本地時間。</span><span class="sxs-lookup"><span data-stu-id="e18ce-155">Convert them from UTC to local time when you receive them.</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="e18ce-156">JavaScript 中的消息套件不支援日期時間.MinValue</span><span class="sxs-lookup"><span data-stu-id="e18ce-156">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="e18ce-157">JavaScript 用戶端使用的[msgpack5](https://github.com/mcollina/msgpack5)庫不支援 MessagePack`timestamp96`中的類型。 SignalR</span><span class="sxs-lookup"><span data-stu-id="e18ce-157">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="e18ce-158">此類型用於編碼非常大的日期值(過去很早就或將來非常遠)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-158">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="e18ce-159">的值`DateTime.MinValue``January 1, 0001`必須 編`timestamp96`碼在 值中。</span><span class="sxs-lookup"><span data-stu-id="e18ce-159">The value of `DateTime.MinValue` is `January 1, 0001`, which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="e18ce-160">因此,不支援發送到`DateTime.MinValue`JAVAScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="e18ce-160">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="e18ce-161">當`DateTime.MinValue`JavaScript 用戶端收到時,將引發以下錯誤:</span><span class="sxs-lookup"><span data-stu-id="e18ce-161">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="e18ce-162">通常,`DateTime.MinValue`用於編碼"缺失"`null`或值。</span><span class="sxs-lookup"><span data-stu-id="e18ce-162">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="e18ce-163">如果需要在 MessagePack 中對該值進行編碼,請`DateTime`使用空`DateTime?`值 (`bool`) 或編碼 單獨的值,指示日期是否存在。</span><span class="sxs-lookup"><span data-stu-id="e18ce-163">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="e18ce-164">有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-164">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="e18ce-165">"提前"編譯環境中的消息包支援</span><span class="sxs-lookup"><span data-stu-id="e18ce-165">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="e18ce-166">.NET 用戶端和伺服器使用的[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v2.1.90)庫使用代碼生成來優化序列化。</span><span class="sxs-lookup"><span data-stu-id="e18ce-166">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v2.1.90) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="e18ce-167">因此,預設情況下不支援使用"提前"編譯(如 Xamarin iOS 或 Unity)的環境。</span><span class="sxs-lookup"><span data-stu-id="e18ce-167">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="e18ce-168">可以通過「預生成」序列化器/反序列化器代碼在這些環境中使用 MessagePack。</span><span class="sxs-lookup"><span data-stu-id="e18ce-168">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="e18ce-169">有關詳細資訊,請參閱[MessagePack-CSharp 文件](https://github.com/neuecc/MessagePack-CSharp/tree/v2.1.90#aot-code-generation-to-support-unityxamarin)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-169">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp/tree/v2.1.90#aot-code-generation-to-support-unityxamarin).</span></span> <span data-ttu-id="e18ce-170">預先生成序列化器後,可以使用傳遞給 的設定委託註冊`AddMessagePackProtocol`它們 。</span><span class="sxs-lookup"><span data-stu-id="e18ce-170">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="e18ce-171">訊息套件中的類型檢查更加嚴格</span><span class="sxs-lookup"><span data-stu-id="e18ce-171">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="e18ce-172">JSON 中心協定將在反序列化期間執行類型轉換。</span><span class="sxs-lookup"><span data-stu-id="e18ce-172">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="e18ce-173">例如,如果傳入物件的屬性值為數位 (),`{ foo: 42 }`但 .NET 類`string`上的屬性為類型 ,則將轉換該值。</span><span class="sxs-lookup"><span data-stu-id="e18ce-173">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="e18ce-174">但是,MessagePack 不執行此轉換,並且將引發在伺服器端日誌(和控制台中)中可以看到的異常:</span><span class="sxs-lookup"><span data-stu-id="e18ce-174">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="e18ce-175">有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-175">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="e18ce-176">相關資源</span><span class="sxs-lookup"><span data-stu-id="e18ce-176">Related resources</span></span>

* [<span data-ttu-id="e18ce-177">入門</span><span class="sxs-lookup"><span data-stu-id="e18ce-177">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="e18ce-178">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="e18ce-178">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="e18ce-179">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="e18ce-179">JavaScript client</span></span>](xref:signalr/javascript-client)

::: moniker-end

::: moniker range=">= aspnetcore-3.0 < aspnetcore-5.0"

<span data-ttu-id="e18ce-180">本文假定讀者[熟悉入門中](xref:tutorials/signalr)介紹的主題。</span><span class="sxs-lookup"><span data-stu-id="e18ce-180">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="e18ce-181">什麼是消息包?</span><span class="sxs-lookup"><span data-stu-id="e18ce-181">What is MessagePack?</span></span>

<span data-ttu-id="e18ce-182">[MessagePack](https://msgpack.org/index.html)是一種快速緊湊的二進位序列化格式。</span><span class="sxs-lookup"><span data-stu-id="e18ce-182">[MessagePack](https://msgpack.org/index.html) is a fast and compact binary serialization format.</span></span> <span data-ttu-id="e18ce-183">當性能和頻寬是一個問題時,它很有用,因為它創建的消息比[JSON](https://www.json.org/)少。</span><span class="sxs-lookup"><span data-stu-id="e18ce-183">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="e18ce-184">二進位訊息在查看網路追蹤和日誌時不可讀,除非位元組透過 MessagePack 解析器傳遞。</span><span class="sxs-lookup"><span data-stu-id="e18ce-184">The binary messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> SignalR<span data-ttu-id="e18ce-185">具有對 MessagePack 格式的內建支援,並為用戶端和伺服器提供了要使用的 API。</span><span class="sxs-lookup"><span data-stu-id="e18ce-185"> has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="e18ce-186">在伺服器上設定訊息套件</span><span class="sxs-lookup"><span data-stu-id="e18ce-186">Configure MessagePack on the server</span></span>

<span data-ttu-id="e18ce-187">要在伺服器上啟用 MessagePack 中心協定,`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`請在應用中安裝包。</span><span class="sxs-lookup"><span data-stu-id="e18ce-187">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="e18ce-188">在`Startup.ConfigureServices`方法中,`AddMessagePackProtocol`添加`AddSignalR`到調用以在伺服器上啟用 MessagePack 支援。</span><span class="sxs-lookup"><span data-stu-id="e18ce-188">In the `Startup.ConfigureServices` method, add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="e18ce-189">默認情況下啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="e18ce-189">JSON is enabled by default.</span></span> <span data-ttu-id="e18ce-190">添加消息包支援 JSON 和 MessagePack 用戶端。</span><span class="sxs-lookup"><span data-stu-id="e18ce-190">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="e18ce-191">要自訂 MessagePack 如何格式化`AddMessagePackProtocol`資料, 請取得用於設定選項的委託。</span><span class="sxs-lookup"><span data-stu-id="e18ce-191">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="e18ce-192">在該委託中,`FormatterResolvers`該屬性可用於配置 MessagePack 序列化選項。</span><span class="sxs-lookup"><span data-stu-id="e18ce-192">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="e18ce-193">有關解析器如何工作的詳細資訊,請造[訪 MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)中的 MessagePack 庫。</span><span class="sxs-lookup"><span data-stu-id="e18ce-193">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="e18ce-194">屬性可用於要序列化的物件,以定義如何處理它們。</span><span class="sxs-lookup"><span data-stu-id="e18ce-194">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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
> <span data-ttu-id="e18ce-195">我們強烈建議查看[CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf)並應用建議的修補程式。</span><span class="sxs-lookup"><span data-stu-id="e18ce-195">We strongly recommend reviewing [CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) and applying the recommended patches.</span></span> <span data-ttu-id="e18ce-196">例如,將`MessagePackSecurity.Active`靜態屬性設定`MessagePackSecurity.UntrustedData`為 。</span><span class="sxs-lookup"><span data-stu-id="e18ce-196">For example, setting the `MessagePackSecurity.Active` static property to `MessagePackSecurity.UntrustedData`.</span></span> <span data-ttu-id="e18ce-197">設定`MessagePackSecurity.Active`需要手動安裝[1.9.x 版本的 MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-197">Setting the `MessagePackSecurity.Active` requires manually installing a [1.9.x version of MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3).</span></span> <span data-ttu-id="e18ce-198">安裝`MessagePack`1.9.xSignalR升級 版本使用。</span><span class="sxs-lookup"><span data-stu-id="e18ce-198">Installing `MessagePack` 1.9.x upgrades the version SignalR uses.</span></span> <span data-ttu-id="e18ce-199">當`MessagePackSecurity.Active`未設置`MessagePackSecurity.UntrustedData`為 時,惡意用戶端可能會導致拒絕服務。</span><span class="sxs-lookup"><span data-stu-id="e18ce-199">When `MessagePackSecurity.Active` is not set to `MessagePackSecurity.UntrustedData`, a malicious client could cause a denial of service.</span></span> <span data-ttu-id="e18ce-200">在`MessagePackSecurity.Active``Program.Main`中設定,如以下代碼所示:</span><span class="sxs-lookup"><span data-stu-id="e18ce-200">Set `MessagePackSecurity.Active` in `Program.Main`, as shown in the following code:</span></span>

```csharp
public static void Main(string[] args)
{
  MessagePackSecurity.Active = MessagePackSecurity.UntrustedData;

  CreateHostBuilder(args).Build().Run();
}
```

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="e18ce-201">在用戶端上設定訊息套件</span><span class="sxs-lookup"><span data-stu-id="e18ce-201">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="e18ce-202">默認情況下,為支援的用戶端啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="e18ce-202">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="e18ce-203">用戶端只能支援單個協定。</span><span class="sxs-lookup"><span data-stu-id="e18ce-203">Clients can only support a single protocol.</span></span> <span data-ttu-id="e18ce-204">添加 MessagePack 支援將替換任何以前配置的協定。</span><span class="sxs-lookup"><span data-stu-id="e18ce-204">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="e18ce-205">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="e18ce-205">.NET client</span></span>

<span data-ttu-id="e18ce-206">在 .NET 客戶端開啟 MessagePack,`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`請安裝`AddMessagePackProtocol`套件`HubConnectionBuilder`並呼叫 。</span><span class="sxs-lookup"><span data-stu-id="e18ce-206">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="e18ce-207">此`AddMessagePackProtocol`調用需要一個委託來配置類似於伺服器的選項。</span><span class="sxs-lookup"><span data-stu-id="e18ce-207">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="e18ce-208">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="e18ce-208">JavaScript client</span></span>

<span data-ttu-id="e18ce-209">JavaScript 用戶端的消息包支援[@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack)由 npm 包提供。</span><span class="sxs-lookup"><span data-stu-id="e18ce-209">MessagePack support for the JavaScript client is provided by the [@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack) npm package.</span></span> <span data-ttu-id="e18ce-210">透過在命令外殼中執行以下命令來安裝套件:</span><span class="sxs-lookup"><span data-stu-id="e18ce-210">Install the package by executing the following command in a command shell:</span></span>

```bash
npm install @microsoft/signalr-protocol-msgpack
```

<span data-ttu-id="e18ce-211">安裝 npm 套件後,模組可以直接透過 JavaScript 模組載入程式使用,或者透過參考以下檔案匯入瀏覽器:</span><span class="sxs-lookup"><span data-stu-id="e18ce-211">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the following file:</span></span>

<span data-ttu-id="e18ce-212">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="e18ce-212">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

<span data-ttu-id="e18ce-213">在瀏覽器中,`msgpack5`還必須引用庫。</span><span class="sxs-lookup"><span data-stu-id="e18ce-213">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="e18ce-214">使用標記`<script>`創建引用。</span><span class="sxs-lookup"><span data-stu-id="e18ce-214">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="e18ce-215">庫可以在*node_modules_msgpack5\dist_msgpack5.js*中找到。</span><span class="sxs-lookup"><span data-stu-id="e18ce-215">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="e18ce-216">使用元素`<script>`時,順序很重要。</span><span class="sxs-lookup"><span data-stu-id="e18ce-216">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="e18ce-217">如果在*msgpack5.js*之前引用*訊號器協定-msgpack.js,* 則嘗試與 MessagePack 連接時會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="e18ce-217">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="e18ce-218">*信號器.js*在*信號器協定-msgpack.js*之前也需要信號。</span><span class="sxs-lookup"><span data-stu-id="e18ce-218">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="e18ce-219">將`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())``HubConnectionBuilder`用戶端配置為在連接到伺服器時使用 MessagePack 協定。</span><span class="sxs-lookup"><span data-stu-id="e18ce-219">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="e18ce-220">此時,JavaScript 用戶端上的 MessagePack 協定沒有配置選項。</span><span class="sxs-lookup"><span data-stu-id="e18ce-220">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="e18ce-221">訊息包怪癖</span><span class="sxs-lookup"><span data-stu-id="e18ce-221">MessagePack quirks</span></span>

<span data-ttu-id="e18ce-222">使用 MessagePack 中心協定時,需要注意一些問題。</span><span class="sxs-lookup"><span data-stu-id="e18ce-222">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="e18ce-223">訊息套件區分大小寫</span><span class="sxs-lookup"><span data-stu-id="e18ce-223">MessagePack is case-sensitive</span></span>

<span data-ttu-id="e18ce-224">MessagePack 協定區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="e18ce-224">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="e18ce-225">例如,請考慮以下 C# 類:</span><span class="sxs-lookup"><span data-stu-id="e18ce-225">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="e18ce-226">從 JavaScript 客戶端發送時`PascalCased`,必須使用 屬性名稱,因為大小寫必須與 C# 類完全匹配。</span><span class="sxs-lookup"><span data-stu-id="e18ce-226">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="e18ce-227">例如：</span><span class="sxs-lookup"><span data-stu-id="e18ce-227">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="e18ce-228">使用`camelCased`名稱不會正確綁定到 C# 類。</span><span class="sxs-lookup"><span data-stu-id="e18ce-228">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="e18ce-229">可以使用`Key`屬性為 MessagePack 屬性指定其他名稱來解決此問題。</span><span class="sxs-lookup"><span data-stu-id="e18ce-229">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="e18ce-230">有關詳細資訊,請參閱[MessagePack-CSharp 文件](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-230">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="e18ce-231">序列化/反序列化時不保留日期時間.Kind</span><span class="sxs-lookup"><span data-stu-id="e18ce-231">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="e18ce-232">MessagePack 協定不提供`Kind`對 的值進行編碼`DateTime`的方法。</span><span class="sxs-lookup"><span data-stu-id="e18ce-232">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="e18ce-233">因此,當對日期進行反序列化時,MessagePack 中心協定假定傳入日期為 UTC 格式。</span><span class="sxs-lookup"><span data-stu-id="e18ce-233">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="e18ce-234">如果您在本地時間使用`DateTime`值,我們建議您在發送之前轉換為 UTC。</span><span class="sxs-lookup"><span data-stu-id="e18ce-234">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="e18ce-235">當您收到它們時,將它們從UTC轉換為本地時間。</span><span class="sxs-lookup"><span data-stu-id="e18ce-235">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="e18ce-236">有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/#2632SignalR](https://github.com/aspnet/SignalR/issues/2632)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-236">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="e18ce-237">JavaScript 中的消息套件不支援日期時間.MinValue</span><span class="sxs-lookup"><span data-stu-id="e18ce-237">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="e18ce-238">JavaScript 用戶端使用的[msgpack5](https://github.com/mcollina/msgpack5)庫不支援 MessagePack`timestamp96`中的類型。 SignalR</span><span class="sxs-lookup"><span data-stu-id="e18ce-238">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="e18ce-239">此類型用於編碼非常大的日期值(過去很早就或將來非常遠)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-239">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="e18ce-240">的值`DateTime.MinValue``January 1, 0001`必須 編`timestamp96`碼在 值中。</span><span class="sxs-lookup"><span data-stu-id="e18ce-240">The value of `DateTime.MinValue` is `January 1, 0001`, which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="e18ce-241">因此,不支援發送到`DateTime.MinValue`JAVAScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="e18ce-241">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="e18ce-242">當`DateTime.MinValue`JavaScript 用戶端收到時,將引發以下錯誤:</span><span class="sxs-lookup"><span data-stu-id="e18ce-242">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="e18ce-243">通常,`DateTime.MinValue`用於編碼"缺失"`null`或值。</span><span class="sxs-lookup"><span data-stu-id="e18ce-243">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="e18ce-244">如果需要在 MessagePack 中對該值進行編碼,請`DateTime`使用空`DateTime?`值 (`bool`) 或編碼 單獨的值,指示日期是否存在。</span><span class="sxs-lookup"><span data-stu-id="e18ce-244">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="e18ce-245">有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-245">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="e18ce-246">"提前"編譯環境中的消息包支援</span><span class="sxs-lookup"><span data-stu-id="e18ce-246">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="e18ce-247">.NET 用戶端和伺服器使用的[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80)庫使用代碼生成來優化序列化。</span><span class="sxs-lookup"><span data-stu-id="e18ce-247">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="e18ce-248">因此,預設情況下不支援使用"提前"編譯(如 Xamarin iOS 或 Unity)的環境。</span><span class="sxs-lookup"><span data-stu-id="e18ce-248">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="e18ce-249">可以通過「預生成」序列化器/反序列化器代碼在這些環境中使用 MessagePack。</span><span class="sxs-lookup"><span data-stu-id="e18ce-249">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="e18ce-250">有關詳細資訊,請參閱[MessagePack-CSharp 文件](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80#pre-code-generationunityxamarin-supports)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-250">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="e18ce-251">預先生成序列化器後,可以使用傳遞給 的設定委託註冊`AddMessagePackProtocol`它們 。</span><span class="sxs-lookup"><span data-stu-id="e18ce-251">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="e18ce-252">訊息套件中的類型檢查更加嚴格</span><span class="sxs-lookup"><span data-stu-id="e18ce-252">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="e18ce-253">JSON 中心協定將在反序列化期間執行類型轉換。</span><span class="sxs-lookup"><span data-stu-id="e18ce-253">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="e18ce-254">例如,如果傳入物件的屬性值為數位 (),`{ foo: 42 }`但 .NET 類`string`上的屬性為類型 ,則將轉換該值。</span><span class="sxs-lookup"><span data-stu-id="e18ce-254">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="e18ce-255">但是,MessagePack 不執行此轉換,並且將引發在伺服器端日誌(和控制台中)中可以看到的異常:</span><span class="sxs-lookup"><span data-stu-id="e18ce-255">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="e18ce-256">有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-256">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="e18ce-257">相關資源</span><span class="sxs-lookup"><span data-stu-id="e18ce-257">Related resources</span></span>

* [<span data-ttu-id="e18ce-258">入門</span><span class="sxs-lookup"><span data-stu-id="e18ce-258">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="e18ce-259">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="e18ce-259">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="e18ce-260">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="e18ce-260">JavaScript client</span></span>](xref:signalr/javascript-client)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e18ce-261">本文假定讀者[熟悉入門中](xref:tutorials/signalr)介紹的主題。</span><span class="sxs-lookup"><span data-stu-id="e18ce-261">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="e18ce-262">什麼是消息包?</span><span class="sxs-lookup"><span data-stu-id="e18ce-262">What is MessagePack?</span></span>

<span data-ttu-id="e18ce-263">[MessagePack](https://msgpack.org/index.html)是一種快速緊湊的二進位序列化格式。</span><span class="sxs-lookup"><span data-stu-id="e18ce-263">[MessagePack](https://msgpack.org/index.html) is a fast and compact binary serialization format.</span></span> <span data-ttu-id="e18ce-264">當性能和頻寬是一個問題時,它很有用,因為它創建的消息比[JSON](https://www.json.org/)少。</span><span class="sxs-lookup"><span data-stu-id="e18ce-264">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="e18ce-265">二進位訊息在查看網路追蹤和日誌時不可讀,除非位元組透過 MessagePack 解析器傳遞。</span><span class="sxs-lookup"><span data-stu-id="e18ce-265">The binary messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> SignalR<span data-ttu-id="e18ce-266">具有對 MessagePack 格式的內建支援,並為用戶端和伺服器提供了要使用的 API。</span><span class="sxs-lookup"><span data-stu-id="e18ce-266"> has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="e18ce-267">在伺服器上設定訊息套件</span><span class="sxs-lookup"><span data-stu-id="e18ce-267">Configure MessagePack on the server</span></span>

<span data-ttu-id="e18ce-268">要在伺服器上啟用 MessagePack 中心協定,`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`請在應用中安裝包。</span><span class="sxs-lookup"><span data-stu-id="e18ce-268">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="e18ce-269">在`Startup.ConfigureServices`方法中,`AddMessagePackProtocol`添加`AddSignalR`到調用以在伺服器上啟用 MessagePack 支援。</span><span class="sxs-lookup"><span data-stu-id="e18ce-269">In the `Startup.ConfigureServices` method, add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="e18ce-270">默認情況下啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="e18ce-270">JSON is enabled by default.</span></span> <span data-ttu-id="e18ce-271">添加消息包支援 JSON 和 MessagePack 用戶端。</span><span class="sxs-lookup"><span data-stu-id="e18ce-271">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="e18ce-272">要自訂 MessagePack 如何格式化`AddMessagePackProtocol`資料, 請取得用於設定選項的委託。</span><span class="sxs-lookup"><span data-stu-id="e18ce-272">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="e18ce-273">在該委託中,`FormatterResolvers`該屬性可用於配置 MessagePack 序列化選項。</span><span class="sxs-lookup"><span data-stu-id="e18ce-273">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="e18ce-274">有關解析器如何工作的詳細資訊,請造[訪 MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)中的 MessagePack 庫。</span><span class="sxs-lookup"><span data-stu-id="e18ce-274">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="e18ce-275">屬性可用於要序列化的物件,以定義如何處理它們。</span><span class="sxs-lookup"><span data-stu-id="e18ce-275">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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
> <span data-ttu-id="e18ce-276">我們強烈建議查看[CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf)並應用建議的修補程式。</span><span class="sxs-lookup"><span data-stu-id="e18ce-276">We strongly recommend reviewing [CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) and applying the recommended patches.</span></span> <span data-ttu-id="e18ce-277">例如,將`MessagePackSecurity.Active`靜態屬性設定`MessagePackSecurity.UntrustedData`為 。</span><span class="sxs-lookup"><span data-stu-id="e18ce-277">For example, setting the `MessagePackSecurity.Active` static property to `MessagePackSecurity.UntrustedData`.</span></span> <span data-ttu-id="e18ce-278">設定`MessagePackSecurity.Active`需要手動安裝[1.9.x 版本的 MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-278">Setting the `MessagePackSecurity.Active` requires manually installing a [1.9.x version of MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3).</span></span> <span data-ttu-id="e18ce-279">安裝`MessagePack`1.9.xSignalR升級 版本使用。</span><span class="sxs-lookup"><span data-stu-id="e18ce-279">Installing `MessagePack` 1.9.x upgrades the version SignalR uses.</span></span> <span data-ttu-id="e18ce-280">當`MessagePackSecurity.Active`未設置`MessagePackSecurity.UntrustedData`為 時,惡意用戶端可能會導致拒絕服務。</span><span class="sxs-lookup"><span data-stu-id="e18ce-280">When `MessagePackSecurity.Active` is not set to `MessagePackSecurity.UntrustedData`, a malicious client could cause a denial of service.</span></span> <span data-ttu-id="e18ce-281">在`MessagePackSecurity.Active``Program.Main`中設定,如以下代碼所示:</span><span class="sxs-lookup"><span data-stu-id="e18ce-281">Set `MessagePackSecurity.Active` in `Program.Main`, as shown in the following code:</span></span>

```csharp
public static void Main(string[] args)
{
  MessagePackSecurity.Active = MessagePackSecurity.UntrustedData;

  CreateHostBuilder(args).Build().Run();
}
```

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="e18ce-282">在用戶端上設定訊息套件</span><span class="sxs-lookup"><span data-stu-id="e18ce-282">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="e18ce-283">默認情況下,為支援的用戶端啟用 JSON。</span><span class="sxs-lookup"><span data-stu-id="e18ce-283">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="e18ce-284">用戶端只能支援單個協定。</span><span class="sxs-lookup"><span data-stu-id="e18ce-284">Clients can only support a single protocol.</span></span> <span data-ttu-id="e18ce-285">添加 MessagePack 支援將替換任何以前配置的協定。</span><span class="sxs-lookup"><span data-stu-id="e18ce-285">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="e18ce-286">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="e18ce-286">.NET client</span></span>

<span data-ttu-id="e18ce-287">在 .NET 客戶端開啟 MessagePack,`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`請安裝`AddMessagePackProtocol`套件`HubConnectionBuilder`並呼叫 。</span><span class="sxs-lookup"><span data-stu-id="e18ce-287">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="e18ce-288">此`AddMessagePackProtocol`調用需要一個委託來配置類似於伺服器的選項。</span><span class="sxs-lookup"><span data-stu-id="e18ce-288">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="e18ce-289">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="e18ce-289">JavaScript client</span></span>

<span data-ttu-id="e18ce-290">JavaScript 用戶端的消息包支援[@aspnet/signalr-protocol-msgpack](https://www.npmjs.com/package/@aspnet/signalr-protocol-msgpack)由 npm 包提供。</span><span class="sxs-lookup"><span data-stu-id="e18ce-290">MessagePack support for the JavaScript client is provided by the [@aspnet/signalr-protocol-msgpack](https://www.npmjs.com/package/@aspnet/signalr-protocol-msgpack) npm package.</span></span> <span data-ttu-id="e18ce-291">透過在命令外殼中執行以下命令來安裝套件:</span><span class="sxs-lookup"><span data-stu-id="e18ce-291">Install the package by executing the following command in a command shell:</span></span>

```bash
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="e18ce-292">安裝 npm 套件後,模組可以直接透過 JavaScript 模組載入程式使用,或者透過參考以下檔案匯入瀏覽器:</span><span class="sxs-lookup"><span data-stu-id="e18ce-292">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the following file:</span></span>

<span data-ttu-id="e18ce-293">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="e18ce-293">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span>

<span data-ttu-id="e18ce-294">在瀏覽器中,`msgpack5`還必須引用庫。</span><span class="sxs-lookup"><span data-stu-id="e18ce-294">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="e18ce-295">使用標記`<script>`創建引用。</span><span class="sxs-lookup"><span data-stu-id="e18ce-295">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="e18ce-296">庫可以在*node_modules_msgpack5\dist_msgpack5.js*中找到。</span><span class="sxs-lookup"><span data-stu-id="e18ce-296">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="e18ce-297">使用元素`<script>`時,順序很重要。</span><span class="sxs-lookup"><span data-stu-id="e18ce-297">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="e18ce-298">如果在*msgpack5.js*之前引用*訊號器協定-msgpack.js,* 則嘗試與 MessagePack 連接時會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="e18ce-298">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="e18ce-299">*信號器.js*在*信號器協定-msgpack.js*之前也需要信號。</span><span class="sxs-lookup"><span data-stu-id="e18ce-299">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="e18ce-300">將`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())``HubConnectionBuilder`用戶端配置為在連接到伺服器時使用 MessagePack 協定。</span><span class="sxs-lookup"><span data-stu-id="e18ce-300">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="e18ce-301">此時,JavaScript 用戶端上的 MessagePack 協定沒有配置選項。</span><span class="sxs-lookup"><span data-stu-id="e18ce-301">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="e18ce-302">訊息包怪癖</span><span class="sxs-lookup"><span data-stu-id="e18ce-302">MessagePack quirks</span></span>

<span data-ttu-id="e18ce-303">使用 MessagePack 中心協定時,需要注意一些問題。</span><span class="sxs-lookup"><span data-stu-id="e18ce-303">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="e18ce-304">訊息套件區分大小寫</span><span class="sxs-lookup"><span data-stu-id="e18ce-304">MessagePack is case-sensitive</span></span>

<span data-ttu-id="e18ce-305">MessagePack 協定區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="e18ce-305">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="e18ce-306">例如,請考慮以下 C# 類:</span><span class="sxs-lookup"><span data-stu-id="e18ce-306">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="e18ce-307">從 JavaScript 客戶端發送時`PascalCased`,必須使用 屬性名稱,因為大小寫必須與 C# 類完全匹配。</span><span class="sxs-lookup"><span data-stu-id="e18ce-307">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="e18ce-308">例如：</span><span class="sxs-lookup"><span data-stu-id="e18ce-308">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="e18ce-309">使用`camelCased`名稱不會正確綁定到 C# 類。</span><span class="sxs-lookup"><span data-stu-id="e18ce-309">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="e18ce-310">可以使用`Key`屬性為 MessagePack 屬性指定其他名稱來解決此問題。</span><span class="sxs-lookup"><span data-stu-id="e18ce-310">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="e18ce-311">有關詳細資訊,請參閱[MessagePack-CSharp 文件](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-311">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="e18ce-312">序列化/反序列化時不保留日期時間.Kind</span><span class="sxs-lookup"><span data-stu-id="e18ce-312">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="e18ce-313">MessagePack 協定不提供`Kind`對 的值進行編碼`DateTime`的方法。</span><span class="sxs-lookup"><span data-stu-id="e18ce-313">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="e18ce-314">因此,當對日期進行反序列化時,MessagePack 中心協定假定傳入日期為 UTC 格式。</span><span class="sxs-lookup"><span data-stu-id="e18ce-314">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="e18ce-315">如果您在本地時間使用`DateTime`值,我們建議您在發送之前轉換為 UTC。</span><span class="sxs-lookup"><span data-stu-id="e18ce-315">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="e18ce-316">當您收到它們時,將它們從UTC轉換為本地時間。</span><span class="sxs-lookup"><span data-stu-id="e18ce-316">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="e18ce-317">有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/#2632SignalR](https://github.com/aspnet/SignalR/issues/2632)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-317">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="e18ce-318">JavaScript 中的消息套件不支援日期時間.MinValue</span><span class="sxs-lookup"><span data-stu-id="e18ce-318">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="e18ce-319">JavaScript 用戶端使用的[msgpack5](https://github.com/mcollina/msgpack5)庫不支援 MessagePack`timestamp96`中的類型。 SignalR</span><span class="sxs-lookup"><span data-stu-id="e18ce-319">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="e18ce-320">此類型用於編碼非常大的日期值(過去很早就或將來非常遠)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-320">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="e18ce-321">的值`DateTime.MinValue``January 1, 0001`必須編碼`timestamp96`在 值中。</span><span class="sxs-lookup"><span data-stu-id="e18ce-321">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="e18ce-322">因此,不支援發送到`DateTime.MinValue`JAVAScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="e18ce-322">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="e18ce-323">當`DateTime.MinValue`JavaScript 用戶端收到時,將引發以下錯誤:</span><span class="sxs-lookup"><span data-stu-id="e18ce-323">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="e18ce-324">通常,`DateTime.MinValue`用於編碼"缺失"`null`或值。</span><span class="sxs-lookup"><span data-stu-id="e18ce-324">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="e18ce-325">如果需要在 MessagePack 中對該值進行編碼,請`DateTime`使用空`DateTime?`值 (`bool`) 或編碼 單獨的值,指示日期是否存在。</span><span class="sxs-lookup"><span data-stu-id="e18ce-325">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="e18ce-326">有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-326">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="e18ce-327">"提前"編譯環境中的消息包支援</span><span class="sxs-lookup"><span data-stu-id="e18ce-327">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="e18ce-328">.NET 用戶端和伺服器使用的[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80)庫使用代碼生成來優化序列化。</span><span class="sxs-lookup"><span data-stu-id="e18ce-328">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="e18ce-329">因此,預設情況下不支援使用"提前"編譯(如 Xamarin iOS 或 Unity)的環境。</span><span class="sxs-lookup"><span data-stu-id="e18ce-329">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="e18ce-330">可以通過「預生成」序列化器/反序列化器代碼在這些環境中使用 MessagePack。</span><span class="sxs-lookup"><span data-stu-id="e18ce-330">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="e18ce-331">有關詳細資訊,請參閱[MessagePack-CSharp 文件](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80#pre-code-generationunityxamarin-supports)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-331">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="e18ce-332">預先生成序列化器後,可以使用傳遞給 的設定委託註冊`AddMessagePackProtocol`它們 。</span><span class="sxs-lookup"><span data-stu-id="e18ce-332">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="e18ce-333">訊息套件中的類型檢查更加嚴格</span><span class="sxs-lookup"><span data-stu-id="e18ce-333">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="e18ce-334">JSON 中心協定將在反序列化期間執行類型轉換。</span><span class="sxs-lookup"><span data-stu-id="e18ce-334">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="e18ce-335">例如,如果傳入物件的屬性值為數位 (),`{ foo: 42 }`但 .NET 類`string`上的屬性為類型 ,則將轉換該值。</span><span class="sxs-lookup"><span data-stu-id="e18ce-335">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="e18ce-336">但是,MessagePack 不執行此轉換,並且將引發在伺服器端日誌(和控制台中)中可以看到的異常:</span><span class="sxs-lookup"><span data-stu-id="e18ce-336">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="e18ce-337">有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937)。</span><span class="sxs-lookup"><span data-stu-id="e18ce-337">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="e18ce-338">相關資源</span><span class="sxs-lookup"><span data-stu-id="e18ce-338">Related resources</span></span>

* [<span data-ttu-id="e18ce-339">入門</span><span class="sxs-lookup"><span data-stu-id="e18ce-339">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="e18ce-340">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="e18ce-340">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="e18ce-341">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="e18ce-341">JavaScript client</span></span>](xref:signalr/javascript-client)

::: moniker-end
