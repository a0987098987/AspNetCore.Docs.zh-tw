---
title: Versioning gRPC 服務
author: jamesnk
description: 瞭解如何對 gRPC 服務進行版本控制。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/09/2020
uid: grpc/versioning
ms.openlocfilehash: 9bd76009ba28a1abef25a98686afea6753d4a8f4
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664112"
---
# <a name="versioning-grpc-services"></a><span data-ttu-id="ee27e-103">Versioning gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="ee27e-103">Versioning gRPC services</span></span>

<span data-ttu-id="ee27e-104">由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="ee27e-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="ee27e-105">添加到應用的新功能可能需要向用戶端提供的 gRPC 服務進行更改,有時以意外和中斷的方式更改。</span><span class="sxs-lookup"><span data-stu-id="ee27e-105">New features added to an app can require gRPC services provided to clients to change, sometimes in unexpected and breaking ways.</span></span> <span data-ttu-id="ee27e-106">當 gRPC 服務變更時:</span><span class="sxs-lookup"><span data-stu-id="ee27e-106">When gRPC services change:</span></span>

* <span data-ttu-id="ee27e-107">應考慮更改如何影響客戶。</span><span class="sxs-lookup"><span data-stu-id="ee27e-107">Consideration should be given on how changes impact clients.</span></span>
* <span data-ttu-id="ee27e-108">應實施支援更改的版本控制策略。</span><span class="sxs-lookup"><span data-stu-id="ee27e-108">A versioning strategy to support changes should be implemented.</span></span>

## <a name="backwards-compatibility"></a><span data-ttu-id="ee27e-109">回溯相容性</span><span class="sxs-lookup"><span data-stu-id="ee27e-109">Backwards compatibility</span></span>

<span data-ttu-id="ee27e-110">gRPC 協定旨在支援隨時間變化的服務。</span><span class="sxs-lookup"><span data-stu-id="ee27e-110">The gRPC protocol is designed to support services that change over time.</span></span> <span data-ttu-id="ee27e-111">通常,gRPC 服務和方法的添加是非突破的。</span><span class="sxs-lookup"><span data-stu-id="ee27e-111">Generally, additions to gRPC services and methods are non-breaking.</span></span> <span data-ttu-id="ee27e-112">非重大更改允許現有用戶端在不進行更改的情況下繼續工作。</span><span class="sxs-lookup"><span data-stu-id="ee27e-112">Non-breaking changes allow existing clients to continue working without changes.</span></span> <span data-ttu-id="ee27e-113">更改或刪除 gRPC 服務正在中斷更改。</span><span class="sxs-lookup"><span data-stu-id="ee27e-113">Changing or deleting gRPC services are breaking changes.</span></span> <span data-ttu-id="ee27e-114">當 gRPC 服務發生重大更改時,必須更新和重新部署使用該服務的用戶端。</span><span class="sxs-lookup"><span data-stu-id="ee27e-114">When gRPC services have breaking changes, clients using that service have to be updated and redeployed.</span></span>

<span data-ttu-id="ee27e-115">對服務進行不重大變更具有許多好處:</span><span class="sxs-lookup"><span data-stu-id="ee27e-115">Making non-breaking changes to a service has a number of benefits:</span></span>

* <span data-ttu-id="ee27e-116">現有客戶端繼續運行。</span><span class="sxs-lookup"><span data-stu-id="ee27e-116">Existing clients continue to run.</span></span>
* <span data-ttu-id="ee27e-117">避免將更改通知用戶端並更新它們所涉及的工作。</span><span class="sxs-lookup"><span data-stu-id="ee27e-117">Avoids work involved with notifying clients of breaking changes, and updating them.</span></span>
* <span data-ttu-id="ee27e-118">只需要記錄和維護一個版本的服務。</span><span class="sxs-lookup"><span data-stu-id="ee27e-118">Only one version of the service needs to be documented and maintained.</span></span>

### <a name="non-breaking-changes"></a><span data-ttu-id="ee27e-119">非中斷性變更</span><span class="sxs-lookup"><span data-stu-id="ee27e-119">Non-breaking changes</span></span>

<span data-ttu-id="ee27e-120">這些更改在 gRPC 協定等級和 .NET 二進位級上是非突破的。</span><span class="sxs-lookup"><span data-stu-id="ee27e-120">These changes are non-breaking at a gRPC protocol level and .NET binary level.</span></span>

* <span data-ttu-id="ee27e-121">**新增服務**</span><span class="sxs-lookup"><span data-stu-id="ee27e-121">**Adding a new service**</span></span>
* <span data-ttu-id="ee27e-122">**新增新方法**</span><span class="sxs-lookup"><span data-stu-id="ee27e-122">**Adding a new method to a service**</span></span>
* <span data-ttu-id="ee27e-123">**將欄位加入到請求訊息**- 新增到請求訊息的欄位在未設定時使用伺服器上的[預設值](https://developers.google.com/protocol-buffers/docs/proto3#default)進行反序列化。</span><span class="sxs-lookup"><span data-stu-id="ee27e-123">**Adding a field to a request message** - Fields added to a request message are deserialized with the [default value](https://developers.google.com/protocol-buffers/docs/proto3#default) on the server when not set.</span></span> <span data-ttu-id="ee27e-124">要進行非重大更改,當舊用戶端未設置新欄位時,服務必須成功。</span><span class="sxs-lookup"><span data-stu-id="ee27e-124">To be a non-breaking change, the service must succeed when the new field isn't set by older clients.</span></span>
* <span data-ttu-id="ee27e-125">**傳回回應訊息加入欄位**- 加入到回應訊息的欄位將反序列化到用戶端上訊息的[未知欄位](https://developers.google.com/protocol-buffers/docs/proto3#unknowns)集合中。</span><span class="sxs-lookup"><span data-stu-id="ee27e-125">**Adding a field to a response message** - Fields added to a response message are deserialized into the message's [unknown fields](https://developers.google.com/protocol-buffers/docs/proto3#unknowns) collection on the client.</span></span>
* <span data-ttu-id="ee27e-126">**向枚舉**- 枚舉添加值將序列化為數值。</span><span class="sxs-lookup"><span data-stu-id="ee27e-126">**Adding a value to an enum** - Enums are serialized as a numeric value.</span></span> <span data-ttu-id="ee27e-127">新的枚舉值在用戶端上反序列化到沒有枚舉名稱的枚舉值。</span><span class="sxs-lookup"><span data-stu-id="ee27e-127">New enum values are deserialized on the client to the enum value without an enum name.</span></span> <span data-ttu-id="ee27e-128">要進行非重大更改,較舊的用戶端在接收新的枚舉值時必須正確運行。</span><span class="sxs-lookup"><span data-stu-id="ee27e-128">To be a non-breaking change, older clients must run correctly when receiving the new enum value.</span></span>

### <a name="binary-breaking-changes"></a><span data-ttu-id="ee27e-129">二進位中斷變更</span><span class="sxs-lookup"><span data-stu-id="ee27e-129">Binary breaking changes</span></span>

<span data-ttu-id="ee27e-130">以下更改在 gRPC 協定等級不會中斷,但如果用戶端升級到最新的 *.proto*協定或用戶端 .NET 程式集,則需要更新用戶端。</span><span class="sxs-lookup"><span data-stu-id="ee27e-130">The following changes are non-breaking at a gRPC protocol level, but the client needs to be updated if it upgrades to the latest *.proto* contract or client .NET assembly.</span></span> <span data-ttu-id="ee27e-131">如果您計劃將 gRPC 庫發佈到 NuGet,則二進位兼容性非常重要。</span><span class="sxs-lookup"><span data-stu-id="ee27e-131">Binary compatibility is important if you plan to publish a gRPC library to NuGet.</span></span>

* <span data-ttu-id="ee27e-132">**從刪除的欄位中移除欄位**值將反序列化為訊息的[未知欄位](https://developers.google.com/protocol-buffers/docs/proto3#unknowns)。</span><span class="sxs-lookup"><span data-stu-id="ee27e-132">**Removing a field** - Values from a removed field are deserialized to a message's [unknown fields](https://developers.google.com/protocol-buffers/docs/proto3#unknowns).</span></span> <span data-ttu-id="ee27e-133">這不是 gRPC 協定中斷更改,但如果用戶端升級到最新協定,則需要更新用戶端。</span><span class="sxs-lookup"><span data-stu-id="ee27e-133">This isn't a gRPC protocol breaking change, but the client needs to be updated if it upgrades to the latest contract.</span></span> <span data-ttu-id="ee27e-134">將來不會意外重用已刪除的欄位編號,這一點很重要。</span><span class="sxs-lookup"><span data-stu-id="ee27e-134">It's important that a removed field number isn't accidentally reused in the future.</span></span> <span data-ttu-id="ee27e-135">為確保這種情況不會發生,請使用 Protobuf 的[保留](https://developers.google.com/protocol-buffers/docs/proto3#reserved)關鍵字在郵件上指定已刪除的欄位號和名稱。</span><span class="sxs-lookup"><span data-stu-id="ee27e-135">To ensure this doesn't happen, specify deleted field numbers and names on the message using Protobuf's [reserved](https://developers.google.com/protocol-buffers/docs/proto3#reserved) keyword.</span></span>
* <span data-ttu-id="ee27e-136">**重新命名訊息**- 訊息名稱通常不在網路上發送,因此這不是 gRPC 協定中斷更改。</span><span class="sxs-lookup"><span data-stu-id="ee27e-136">**Renaming a message** - Message names aren't typically sent on the network, so this isn't a gRPC protocol breaking change.</span></span> <span data-ttu-id="ee27e-137">如果用戶端升級到最新合同,則需要更新該用戶端。</span><span class="sxs-lookup"><span data-stu-id="ee27e-137">The client will need to be updated if it upgrades to the latest contract.</span></span> <span data-ttu-id="ee27e-138">在網路上**發送消息名稱**的一種方式是使用[Any](https://developers.google.com/protocol-buffers/docs/proto3#any)欄位,當消息名稱用於識別訊息類型時。</span><span class="sxs-lookup"><span data-stu-id="ee27e-138">One situation where message names **are** sent on the network is with [Any](https://developers.google.com/protocol-buffers/docs/proto3#any) fields, when the message name is used to identify the message type.</span></span>
* <span data-ttu-id="ee27e-139">**變更csharp_namespace** `csharp_namespace`- 更改將更改產生的 .NET 類型的命名空間。</span><span class="sxs-lookup"><span data-stu-id="ee27e-139">**Changing csharp_namespace** - Changing `csharp_namespace` will change the namespace of generated .NET types.</span></span> <span data-ttu-id="ee27e-140">這不是 gRPC 協定中斷更改,但如果用戶端升級到最新協定,則需要更新用戶端。</span><span class="sxs-lookup"><span data-stu-id="ee27e-140">This isn't a gRPC protocol breaking change, but the client needs to be updated if it upgrades to the latest contract.</span></span>

### <a name="protocol-breaking-changes"></a><span data-ttu-id="ee27e-141">協定中斷變更</span><span class="sxs-lookup"><span data-stu-id="ee27e-141">Protocol breaking changes</span></span>

<span data-ttu-id="ee27e-142">以下項目是協定與二進制變更:</span><span class="sxs-lookup"><span data-stu-id="ee27e-142">The following items are protocol and binary breaking changes:</span></span>

* <span data-ttu-id="ee27e-143">**重命名字段**- 使用 Protobuf 內容時,欄位名稱僅在生成的代碼中使用。</span><span class="sxs-lookup"><span data-stu-id="ee27e-143">**Renaming a field** - With Protobuf content, the field names are only used in generated code.</span></span> <span data-ttu-id="ee27e-144">欄位編號用於標識網路上的欄位。</span><span class="sxs-lookup"><span data-stu-id="ee27e-144">The field number is used to identify fields on the network.</span></span> <span data-ttu-id="ee27e-145">重命名字段不是 Protobuf 的中斷協定更改。</span><span class="sxs-lookup"><span data-stu-id="ee27e-145">Renaming a field isn't a protocol breaking change for Protobuf.</span></span> <span data-ttu-id="ee27e-146">但是,如果伺服器正在使用 JSON 內容,則重命名字段是一個重大更改。</span><span class="sxs-lookup"><span data-stu-id="ee27e-146">However, if a server is using JSON content then renaming a field is a breaking change.</span></span>
* <span data-ttu-id="ee27e-147">**變更欄位資料類型**- 將欄位的資料類型更改為[不相容的類型](https://developers.google.com/protocol-buffers/docs/proto3#updating)將導致消息序列化時出錯。</span><span class="sxs-lookup"><span data-stu-id="ee27e-147">**Changing a field data type** - Changing a field's data type to an [incompatible type](https://developers.google.com/protocol-buffers/docs/proto3#updating) will cause errors when deserializing the message.</span></span> <span data-ttu-id="ee27e-148">即使新數據類型相容,如果用戶端升級到最新協定,它也可能需要更新以支援新類型。</span><span class="sxs-lookup"><span data-stu-id="ee27e-148">Even if the new data type is compatible, it's likely the client needs to be updated to support the new type if it upgrades to the latest contract.</span></span>
* <span data-ttu-id="ee27e-149">**變更欄位編號**- 使用 Protobuf 有效負載時,欄位編號用於識別網路上的欄位。</span><span class="sxs-lookup"><span data-stu-id="ee27e-149">**Changing a field number** - With Protobuf payloads, the field number is used to identify fields on the network.</span></span>
* <span data-ttu-id="ee27e-150">**重新命名套件、服務或方法**- gRPC 使用套件名稱、服務名稱和方法名稱來生成 URL。</span><span class="sxs-lookup"><span data-stu-id="ee27e-150">**Renaming a package, service or method** - gRPC uses the package name, service name, and method name to build the URL.</span></span> <span data-ttu-id="ee27e-151">用戶端從伺服器獲取 *「未執行」* 狀態。</span><span class="sxs-lookup"><span data-stu-id="ee27e-151">The client gets an *UNIMPLEMENTED* status from the server.</span></span>
* <span data-ttu-id="ee27e-152">**刪除服務或方法**- 用戶端在呼叫已刪除的方法時從伺服器獲取*UN實現*狀態。</span><span class="sxs-lookup"><span data-stu-id="ee27e-152">**Removing a service or method** - The client gets an *UNIMPLEMENTED* status from the server when calling the removed method.</span></span>

### <a name="behavior-breaking-changes"></a><span data-ttu-id="ee27e-153">行為中斷變更</span><span class="sxs-lookup"><span data-stu-id="ee27e-153">Behavior breaking changes</span></span>

<span data-ttu-id="ee27e-154">進行非重大更改時,還必須考慮較舊的用戶端是否可以繼續使用新的服務行為。</span><span class="sxs-lookup"><span data-stu-id="ee27e-154">When making non-breaking changes, you must also consider whether older clients can continue working with the new service behavior.</span></span> <span data-ttu-id="ee27e-155">例如,向要求訊息加入新欄位:</span><span class="sxs-lookup"><span data-stu-id="ee27e-155">For example, adding a new field to a request message:</span></span>

* <span data-ttu-id="ee27e-156">不是協定崩潰的更改嗎?</span><span class="sxs-lookup"><span data-stu-id="ee27e-156">Isn't a protocol breaking change.</span></span>
* <span data-ttu-id="ee27e-157">如果未設置新欄位,則在伺服器上返回錯誤狀態,將使其成為舊用戶端的重大變化。</span><span class="sxs-lookup"><span data-stu-id="ee27e-157">Returning an error status on the server if the new field isn't set makes it a breaking change for old clients.</span></span>

<span data-ttu-id="ee27e-158">行為相容性由特定於應用的代碼決定。</span><span class="sxs-lookup"><span data-stu-id="ee27e-158">Behavior compatibility is determined by your app-specific code.</span></span>

## <a name="version-number-services"></a><span data-ttu-id="ee27e-159">版本號碼</span><span class="sxs-lookup"><span data-stu-id="ee27e-159">Version number services</span></span>

<span data-ttu-id="ee27e-160">服務應努力與舊用戶端保持向後相容。</span><span class="sxs-lookup"><span data-stu-id="ee27e-160">Services should strive to remain backwards compatible with old clients.</span></span> <span data-ttu-id="ee27e-161">最終,對應用的更改可能需要重大更改。</span><span class="sxs-lookup"><span data-stu-id="ee27e-161">Eventually changes to your app may require breaking changes.</span></span> <span data-ttu-id="ee27e-162">中斷舊用戶端並強制它們隨服務一起更新並不是一個好的用戶體驗。</span><span class="sxs-lookup"><span data-stu-id="ee27e-162">Breaking old clients and forcing them to be updated along with your service isn't a good user experience.</span></span> <span data-ttu-id="ee27e-163">在進行重大更改時保持向後相容性的一種方法是發佈服務的多個版本。</span><span class="sxs-lookup"><span data-stu-id="ee27e-163">A way to maintain backwards compatibility while making breaking changes is to publish multiple versions of a service.</span></span>

<span data-ttu-id="ee27e-164">gRPC 支援可選[的包](https://developers.google.com/protocol-buffers/docs/proto3#packages)指定器,它的功能很像 .NET 命名空間。</span><span class="sxs-lookup"><span data-stu-id="ee27e-164">gRPC supports an optional [package](https://developers.google.com/protocol-buffers/docs/proto3#packages) specifier, which functions much like a .NET namespace.</span></span> <span data-ttu-id="ee27e-165">事實上,如果未`option csharp_namespace`在`package` *.proto*檔中設置 ,則將用作生成的 .NET 類型的 .NET 命名空間。</span><span class="sxs-lookup"><span data-stu-id="ee27e-165">In fact, the `package` will be used as the .NET namespace for generated .NET types if `option csharp_namespace` is not set in the *.proto* file.</span></span> <span data-ttu-id="ee27e-166">該套件可用於為服務及其訊息指定版本號:</span><span class="sxs-lookup"><span data-stu-id="ee27e-166">The package can be used to specify a version number for your service and its messages:</span></span>

[!code-protobuf[](versioning/sample/greet.v1.proto?highlight=3)]

<span data-ttu-id="ee27e-167">包名稱與服務名稱相結合以標識服務位址。</span><span class="sxs-lookup"><span data-stu-id="ee27e-167">The package name is combined with the service name to identify a service address.</span></span> <span data-ttu-id="ee27e-168">服務位址允許並行託管服務的多個版本:</span><span class="sxs-lookup"><span data-stu-id="ee27e-168">A service address allows multiple versions of a service to be hosted side-by-side:</span></span>

* `greet.v1.Greeter`
* `greet.v2.Greeter`

<span data-ttu-id="ee27e-169">版本化服務的實現在*Startup.cs*註冊:</span><span class="sxs-lookup"><span data-stu-id="ee27e-169">Implementations of the versioned service are registered in *Startup.cs*:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    // Implements greet.v1.Greeter
    endpoints.MapGrpcService<GreeterServiceV1>();

    // Implements greet.v2.Greeter
    endpoints.MapGrpcService<GreeterServiceV2>();
});
```

<span data-ttu-id="ee27e-170">在包名稱中包含版本號後,您有機會發佈具有重大更改的服務的*v2*版本,同時繼續支援調用*v1*版本的舊用戶端。</span><span class="sxs-lookup"><span data-stu-id="ee27e-170">Including a version number in the package name gives you the opportunity to publish a *v2* version of your service with breaking changes, while continuing to support older clients who call the *v1* version.</span></span> <span data-ttu-id="ee27e-171">用戶端更新後使用*v2*服務後,您可以選擇刪除舊版本。</span><span class="sxs-lookup"><span data-stu-id="ee27e-171">Once clients have updated to use the *v2* service, you can choose to remove the old version.</span></span> <span data-ttu-id="ee27e-172">排程發佈服務的多個版本時:</span><span class="sxs-lookup"><span data-stu-id="ee27e-172">When planning to publish multiple versions of a service:</span></span>

* <span data-ttu-id="ee27e-173">如果合理,避免進行重大更改。</span><span class="sxs-lookup"><span data-stu-id="ee27e-173">Avoid breaking changes if reasonable.</span></span>
* <span data-ttu-id="ee27e-174">除非進行重大更改,否則不要更新版本號。</span><span class="sxs-lookup"><span data-stu-id="ee27e-174">Don't update the version number unless making breaking changes.</span></span>
* <span data-ttu-id="ee27e-175">進行重大更改時,請更新版本號。</span><span class="sxs-lookup"><span data-stu-id="ee27e-175">Do update the version number when you make breaking changes.</span></span>

<span data-ttu-id="ee27e-176">發佈服務的多個版本會複製它。</span><span class="sxs-lookup"><span data-stu-id="ee27e-176">Publishing multiple versions of a service duplicates it.</span></span> <span data-ttu-id="ee27e-177">為了減少重複,請考慮將業務邏輯從服務實現移動到可由新舊實現重用的集中位置:</span><span class="sxs-lookup"><span data-stu-id="ee27e-177">To reduce duplication, consider moving business logic from the service implementations to a centralized location that can be reused by the old and new implementations:</span></span>

[!code-csharp[](versioning/sample/GreeterServiceV1.cs?highlight=10,19)]

<span data-ttu-id="ee27e-178">使用不同套件名稱產生的服務和訊息是不同的 **.NET 型態**。</span><span class="sxs-lookup"><span data-stu-id="ee27e-178">Services and messages generated with different package names are **different .NET types**.</span></span> <span data-ttu-id="ee27e-179">將業務邏輯移動到集中位置需要將消息映射到常見類型。</span><span class="sxs-lookup"><span data-stu-id="ee27e-179">Moving business logic to a centralized location requires mapping messages to common types.</span></span>
