---
title: ASP.NET Core 中的金鑰管理擴充性
author: rick-anderson
description: 深入瞭解 ASP.NET Core 資料保護金鑰管理擴充性。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: e319872799ef4994b55ba941956836f0848dd76d
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408535"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="0188a-103">ASP.NET Core 中的金鑰管理擴充性</span><span class="sxs-lookup"><span data-stu-id="0188a-103">Key management extensibility in ASP.NET Core</span></span>

> [!TIP]
> <span data-ttu-id="0188a-104">閱讀本節之前，請先閱讀[金鑰管理](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)一節，因為它會說明這些 api 背後的一些基本概念。</span><span class="sxs-lookup"><span data-stu-id="0188a-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="0188a-105">實作為下列任何介面的型別應該是多個呼叫端的安全線程。</span><span class="sxs-lookup"><span data-stu-id="0188a-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="0188a-106">Key</span><span class="sxs-lookup"><span data-stu-id="0188a-106">Key</span></span>

<span data-ttu-id="0188a-107">`IKey`介面是 cryptosystem 中索引鍵的基本標記法。</span><span class="sxs-lookup"><span data-stu-id="0188a-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="0188a-108">這裡的關鍵字是用在抽象概念中，而不是「密碼編譯金鑰內容」的常值意義。</span><span class="sxs-lookup"><span data-stu-id="0188a-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="0188a-109">金鑰具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="0188a-109">A key has the following properties:</span></span>

* <span data-ttu-id="0188a-110">啟用、建立和到期日期</span><span class="sxs-lookup"><span data-stu-id="0188a-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="0188a-111">撤銷狀態</span><span class="sxs-lookup"><span data-stu-id="0188a-111">Revocation status</span></span>

* <span data-ttu-id="0188a-112">金鑰識別碼（GUID）</span><span class="sxs-lookup"><span data-stu-id="0188a-112">Key identifier (a GUID)</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0188a-113">此外， `IKey` 會公開 `CreateEncryptor` 方法，可用來建立與此索引鍵系結的[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)實例。</span><span class="sxs-lookup"><span data-stu-id="0188a-113">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0188a-114">此外， `IKey` 會公開 `CreateEncryptorInstance` 方法，可用來建立與此索引鍵系結的[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)實例。</span><span class="sxs-lookup"><span data-stu-id="0188a-114">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="0188a-115">沒有 API 可從實例中抓取未經處理的密碼編譯內容 `IKey` 。</span><span class="sxs-lookup"><span data-stu-id="0188a-115">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="0188a-116">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="0188a-116">IKeyManager</span></span>

<span data-ttu-id="0188a-117">`IKeyManager`介面代表負責一般金鑰儲存、抓取和操作的物件。</span><span class="sxs-lookup"><span data-stu-id="0188a-117">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="0188a-118">它會公開三個高階作業：</span><span class="sxs-lookup"><span data-stu-id="0188a-118">It exposes three high-level operations:</span></span>

* <span data-ttu-id="0188a-119">建立新的金鑰，並將它保存在儲存體中。</span><span class="sxs-lookup"><span data-stu-id="0188a-119">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="0188a-120">從儲存體取得所有金鑰。</span><span class="sxs-lookup"><span data-stu-id="0188a-120">Get all keys from storage.</span></span>

* <span data-ttu-id="0188a-121">撤銷一或多個金鑰，並將撤銷資訊保存到儲存體。</span><span class="sxs-lookup"><span data-stu-id="0188a-121">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="0188a-122">撰寫 `IKeyManager` 非常先進的工作，大部分的開發人員都不應該嘗試這麼做。</span><span class="sxs-lookup"><span data-stu-id="0188a-122">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="0188a-123">相反地，大部分的開發人員都應該利用[XmlKeyManager](#xmlkeymanager)類別所提供的功能。</span><span class="sxs-lookup"><span data-stu-id="0188a-123">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](#xmlkeymanager) class.</span></span>

## <a name="xmlkeymanager"></a><span data-ttu-id="0188a-124">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="0188a-124">XmlKeyManager</span></span>

<span data-ttu-id="0188a-125">`XmlKeyManager`類型是的內建實體執行 `IKeyManager` 。</span><span class="sxs-lookup"><span data-stu-id="0188a-125">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="0188a-126">它提供數個實用的功能，包括待用金鑰的金鑰委付和加密。</span><span class="sxs-lookup"><span data-stu-id="0188a-126">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="0188a-127">此系統中的索引鍵會以 XML 元素（具體而言是[system.xml.linq.xelement>](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)）來表示。</span><span class="sxs-lookup"><span data-stu-id="0188a-127">Keys in this system are represented as XML elements (specifically, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="0188a-128">`XmlKeyManager`在完成其工作的過程中，相依于數個其他元件：</span><span class="sxs-lookup"><span data-stu-id="0188a-128">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="0188a-129">`AlgorithmConfiguration`，其規定新索引鍵所使用的演算法。</span><span class="sxs-lookup"><span data-stu-id="0188a-129">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="0188a-130">`IXmlRepository`，可控制金鑰在儲存體中的保存位置。</span><span class="sxs-lookup"><span data-stu-id="0188a-130">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="0188a-131">`IXmlEncryptor`[選用]，允許待用加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="0188a-131">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="0188a-132">`IKeyEscrowSink`[選用]，提供金鑰委付服務。</span><span class="sxs-lookup"><span data-stu-id="0188a-132">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="0188a-133">`IXmlRepository`，可控制金鑰在儲存體中的保存位置。</span><span class="sxs-lookup"><span data-stu-id="0188a-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="0188a-134">`IXmlEncryptor`[選用]，允許待用加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="0188a-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="0188a-135">`IKeyEscrowSink`[選用]，提供金鑰委付服務。</span><span class="sxs-lookup"><span data-stu-id="0188a-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

<span data-ttu-id="0188a-136">以下為高階圖表，指出這些元件在內如何連接在一起 `XmlKeyManager` 。</span><span class="sxs-lookup"><span data-stu-id="0188a-136">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

::: moniker range=">= aspnetcore-2.0"

![建立金鑰](key-management/_static/keycreation2.png)

<span data-ttu-id="0188a-138">*金鑰建立/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="0188a-138">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="0188a-139">在的執行中 `CreateNewKey` ， `AlgorithmConfiguration` 會使用元件來建立唯一的 `IAuthenticatedEncryptorDescriptor` ，然後將它序列化為 XML。</span><span class="sxs-lookup"><span data-stu-id="0188a-139">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="0188a-140">如果有金鑰委付接收，則會提供原始（未加密）的 XML 給接收以進行長期儲存。</span><span class="sxs-lookup"><span data-stu-id="0188a-140">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="0188a-141">未加密的 XML 接著會透過 `IXmlEncryptor` （如有需要）執行，以產生加密的 xml 檔。</span><span class="sxs-lookup"><span data-stu-id="0188a-141">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="0188a-142">此加密檔會透過保存到長期儲存體 `IXmlRepository` 。</span><span class="sxs-lookup"><span data-stu-id="0188a-142">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="0188a-143">（如果未 `IXmlEncryptor` 設定，則未加密的檔會保存在中 `IXmlRepository` ）。</span><span class="sxs-lookup"><span data-stu-id="0188a-143">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![金鑰抓取](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![建立金鑰](key-management/_static/keycreation1.png)

<span data-ttu-id="0188a-146">*金鑰建立/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="0188a-146">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="0188a-147">在的執行中 `CreateNewKey` ， `IAuthenticatedEncryptorConfiguration` 會使用元件來建立唯一的 `IAuthenticatedEncryptorDescriptor` ，然後將它序列化為 XML。</span><span class="sxs-lookup"><span data-stu-id="0188a-147">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="0188a-148">如果有金鑰委付接收，則會提供原始（未加密）的 XML 給接收以進行長期儲存。</span><span class="sxs-lookup"><span data-stu-id="0188a-148">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="0188a-149">未加密的 XML 接著會透過 `IXmlEncryptor` （如有需要）執行，以產生加密的 xml 檔。</span><span class="sxs-lookup"><span data-stu-id="0188a-149">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="0188a-150">此加密檔會透過保存到長期儲存體 `IXmlRepository` 。</span><span class="sxs-lookup"><span data-stu-id="0188a-150">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="0188a-151">（如果未 `IXmlEncryptor` 設定，則未加密的檔會保存在中 `IXmlRepository` ）。</span><span class="sxs-lookup"><span data-stu-id="0188a-151">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![金鑰抓取](key-management/_static/keyretrieval1.png)

::: moniker-end

<span data-ttu-id="0188a-153">*金鑰抓取/GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="0188a-153">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="0188a-154">在的執行中 `GetAllKeys` ，會從基礎讀取代表索引鍵和撤銷的 XML 檔 `IXmlRepository` 。</span><span class="sxs-lookup"><span data-stu-id="0188a-154">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="0188a-155">如果這些檔已加密，系統將會自動將其解密。</span><span class="sxs-lookup"><span data-stu-id="0188a-155">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="0188a-156">`XmlKeyManager`建立適當的 `IAuthenticatedEncryptorDescriptorDeserializer` 實例，將檔案還原序列化回 `IAuthenticatedEncryptorDescriptor` 實例，然後包裝在個別的 `IKey` 實例中。</span><span class="sxs-lookup"><span data-stu-id="0188a-156">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="0188a-157">這個實例集合 `IKey` 會傳回給呼叫者。</span><span class="sxs-lookup"><span data-stu-id="0188a-157">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="0188a-158">如需特定 XML 元素的進一步資訊，請參閱[金鑰儲存體格式檔](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format)。</span><span class="sxs-lookup"><span data-stu-id="0188a-158">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="0188a-159">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="0188a-159">IXmlRepository</span></span>

<span data-ttu-id="0188a-160">`IXmlRepository`介面代表可以保存 xml 並從備份存放區抓取 xml 的類型。</span><span class="sxs-lookup"><span data-stu-id="0188a-160">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="0188a-161">它會公開兩個 Api：</span><span class="sxs-lookup"><span data-stu-id="0188a-161">It exposes two APIs:</span></span>

* <span data-ttu-id="0188a-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span><span class="sxs-lookup"><span data-stu-id="0188a-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span></span>

* `StoreElement(XElement element, string friendlyName)`

<span data-ttu-id="0188a-163">的 `IXmlRepository` 執行不需要剖析傳遞的 XML。</span><span class="sxs-lookup"><span data-stu-id="0188a-163">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="0188a-164">它們應該將 XML 檔視為不透明，並讓較高層的使用者擔心產生和剖析檔。</span><span class="sxs-lookup"><span data-stu-id="0188a-164">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="0188a-165">有四個內建的具象型別，可實行 `IXmlRepository` ：</span><span class="sxs-lookup"><span data-stu-id="0188a-165">There are four built-in concrete types which implement `IXmlRepository`:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="0188a-166">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="0188a-166">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="0188a-167">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="0188a-167">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="0188a-168">AzureStorage. AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="0188a-168">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="0188a-169">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="0188a-169">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="0188a-170">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="0188a-170">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="0188a-171">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="0188a-171">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="0188a-172">AzureStorage. AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="0188a-172">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="0188a-173">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="0188a-173">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

<span data-ttu-id="0188a-174">如需詳細資訊，請參閱[金鑰儲存提供者檔](xref:security/data-protection/implementation/key-storage-providers)。</span><span class="sxs-lookup"><span data-stu-id="0188a-174">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers) for more information.</span></span>

<span data-ttu-id="0188a-175">`IXmlRepository`使用不同的備份存放區（例如 Azure 表格儲存體）時，註冊自訂是適當的。</span><span class="sxs-lookup"><span data-stu-id="0188a-175">Registering a custom `IXmlRepository` is appropriate when using a different backing store (for example, Azure Table Storage).</span></span>

<span data-ttu-id="0188a-176">若要變更預設的儲存機制應用程式範圍，請註冊自訂 `IXmlRepository` 實例：</span><span class="sxs-lookup"><span data-stu-id="0188a-176">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
```

::: moniker-end

## <a name="ixmlencryptor"></a><span data-ttu-id="0188a-177">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="0188a-177">IXmlEncryptor</span></span>

<span data-ttu-id="0188a-178">`IXmlEncryptor`介面代表可以加密純文字 XML 元素的類型。</span><span class="sxs-lookup"><span data-stu-id="0188a-178">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="0188a-179">它會公開單一 API：</span><span class="sxs-lookup"><span data-stu-id="0188a-179">It exposes a single API:</span></span>

* <span data-ttu-id="0188a-180">Encrypt （System.xml.linq.xelement> plaiNtextElement）： EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="0188a-180">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="0188a-181">如果序列化 `IAuthenticatedEncryptorDescriptor` 包含標記為「需要加密」的任何元素，則 `XmlKeyManager` 會透過所設定的方法來執行這些專案， `IXmlEncryptor` `Encrypt` 而且它會將 enciphered 專案（而不是純文字元素）保存至 `IXmlRepository` 。</span><span class="sxs-lookup"><span data-stu-id="0188a-181">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="0188a-182">方法的輸出 `Encrypt` 是 `EncryptedXmlInfo` 物件。</span><span class="sxs-lookup"><span data-stu-id="0188a-182">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="0188a-183">這個物件是一個包裝函式，其中包含結果 enciphered `XElement` 和代表 `IXmlDecryptor` 可用來解密對應元素的類型。</span><span class="sxs-lookup"><span data-stu-id="0188a-183">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="0188a-184">有四個內建的具象型別，可實行 `IXmlEncryptor` ：</span><span class="sxs-lookup"><span data-stu-id="0188a-184">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>

* [<span data-ttu-id="0188a-185">CertificateXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="0188a-185">CertificateXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [<span data-ttu-id="0188a-186">DpapiNGXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="0188a-186">DpapiNGXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [<span data-ttu-id="0188a-187">DpapiXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="0188a-187">DpapiXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [<span data-ttu-id="0188a-188">NullXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="0188a-188">NullXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

<span data-ttu-id="0188a-189">如需詳細資訊，請參閱待用[金鑰加密檔](xref:security/data-protection/implementation/key-encryption-at-rest)。</span><span class="sxs-lookup"><span data-stu-id="0188a-189">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="0188a-190">若要變更整個應用程式的預設金鑰加密機制，請註冊自訂 `IXmlEncryptor` 實例：</span><span class="sxs-lookup"><span data-stu-id="0188a-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
```

::: moniker-end

## <a name="ixmldecryptor"></a><span data-ttu-id="0188a-191">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="0188a-191">IXmlDecryptor</span></span>

<span data-ttu-id="0188a-192">`IXmlDecryptor`介面代表的類型，知道如何解密透過 `XElement` enciphered 的 `IXmlEncryptor` 。</span><span class="sxs-lookup"><span data-stu-id="0188a-192">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="0188a-193">它會公開單一 API：</span><span class="sxs-lookup"><span data-stu-id="0188a-193">It exposes a single API:</span></span>

* <span data-ttu-id="0188a-194">解密（System.xml.linq.xelement> encryptedElement）： System.xml.linq.xelement></span><span class="sxs-lookup"><span data-stu-id="0188a-194">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="0188a-195">`Decrypt`方法會復原所執行的加密 `IXmlEncryptor.Encrypt` 。</span><span class="sxs-lookup"><span data-stu-id="0188a-195">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="0188a-196">一般來說，每個具體 `IXmlEncryptor` 的執行都有對應的具體 `IXmlDecryptor` 執行。</span><span class="sxs-lookup"><span data-stu-id="0188a-196">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="0188a-197">要執行的類型 `IXmlDecryptor` 應具有下列兩個公用函式的其中一個：</span><span class="sxs-lookup"><span data-stu-id="0188a-197">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="0188a-198">.ctor （IServiceProvider）</span><span class="sxs-lookup"><span data-stu-id="0188a-198">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="0188a-199">.ctor （）</span><span class="sxs-lookup"><span data-stu-id="0188a-199">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="0188a-200">`IServiceProvider`傳遞給此函式的可能是 null。</span><span class="sxs-lookup"><span data-stu-id="0188a-200">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="0188a-201">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="0188a-201">IKeyEscrowSink</span></span>

<span data-ttu-id="0188a-202">`IKeyEscrowSink`介面代表可執行機密資訊的類型。</span><span class="sxs-lookup"><span data-stu-id="0188a-202">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="0188a-203">回想一下，序列化的描述項可能包含機密資訊（例如密碼編譯內容），而這就是第一次引進[IXmlEncryptor](#ixmlencryptor)類型的地方。</span><span class="sxs-lookup"><span data-stu-id="0188a-203">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](#ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="0188a-204">不過，發生事故，而且可以刪除或損毀金鑰環。</span><span class="sxs-lookup"><span data-stu-id="0188a-204">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="0188a-205">「證書處理」介面提供緊急的 escape 影線，允許在任何已設定的[IXmlEncryptor](#ixmlencryptor)轉換原始序列化的 XML 之前，先對其進行存取。</span><span class="sxs-lookup"><span data-stu-id="0188a-205">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](#ixmlencryptor).</span></span> <span data-ttu-id="0188a-206">介面會公開單一 API：</span><span class="sxs-lookup"><span data-stu-id="0188a-206">The interface exposes a single API:</span></span>

* <span data-ttu-id="0188a-207">Store （Guid keyId，System.xml.linq.xelement> 元素）</span><span class="sxs-lookup"><span data-stu-id="0188a-207">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="0188a-208">以安全的方式處理提供的專案是由 `IKeyEscrowSink` 商務原則一致的。</span><span class="sxs-lookup"><span data-stu-id="0188a-208">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="0188a-209">使用已知的公司 x.509 憑證（其中憑證的私密金鑰已委付）時，可能的執行方式之一是`CertificateXmlEncryptor`類型可以協助此。</span><span class="sxs-lookup"><span data-stu-id="0188a-209">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="0188a-210">`IKeyEscrowSink`執行也會負責適當保存提供的元素。</span><span class="sxs-lookup"><span data-stu-id="0188a-210">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="0188a-211">預設不會啟用任何證書機制，雖然伺服器管理員可以[全域進行設定](xref:security/data-protection/configuration/machine-wide-policy)。</span><span class="sxs-lookup"><span data-stu-id="0188a-211">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="0188a-212">它也可以透過方法以程式設計方式進行設定， `IDataProtectionBuilder.AddKeyEscrowSink` 如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="0188a-212">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="0188a-213">方法多載 `AddKeyEscrowSink` 會鏡像 `IServiceCollection.AddSingleton` 和多載 `IServiceCollection.AddInstance` ，因為 `IKeyEscrowSink` 實例是要單次個體的。</span><span class="sxs-lookup"><span data-stu-id="0188a-213">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="0188a-214">如果 `IKeyEscrowSink` 註冊了多個實例，每一個都會在金鑰產生期間呼叫，因此可以同時將金鑰委付到多個機制。</span><span class="sxs-lookup"><span data-stu-id="0188a-214">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="0188a-215">沒有 API 可從實例讀取材質 `IKeyEscrowSink` 。</span><span class="sxs-lookup"><span data-stu-id="0188a-215">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="0188a-216">這與證書處理機制的設計理論一致：其目的是要讓受信任的授權單位能夠存取金鑰材料，而且因為應用程式本身不是受信任的授權單位，所以不應該存取自己的委付資料。</span><span class="sxs-lookup"><span data-stu-id="0188a-216">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="0188a-217">下列範例程式碼示範如何建立和註冊 `IKeyEscrowSink` 委付機碼，讓只有 "CONTOSODomain Admins" 的成員可以復原它們。</span><span class="sxs-lookup"><span data-stu-id="0188a-217">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="0188a-218">若要執行此範例，您必須位於已加入網域的 Windows 8/Windows Server 2012 電腦，且網域控制站必須是 Windows Server 2012 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="0188a-218">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
