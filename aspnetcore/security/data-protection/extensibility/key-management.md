---
title: ASP.NET Core 中的金鑰管理擴充性
author: rick-anderson
description: 深入瞭解 ASP.NET Core 資料保護金鑰管理擴充性。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665876"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="cd570-103">ASP.NET Core 中的金鑰管理擴充性</span><span class="sxs-lookup"><span data-stu-id="cd570-103">Key management extensibility in ASP.NET Core</span></span>

> [!TIP]
> <span data-ttu-id="cd570-104">閱讀本節之前，請先閱讀[金鑰管理](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)一節，因為它會說明這些 api 背後的一些基本概念。</span><span class="sxs-lookup"><span data-stu-id="cd570-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="cd570-105">實作為下列任何介面的型別應該是多個呼叫端的安全線程。</span><span class="sxs-lookup"><span data-stu-id="cd570-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="cd570-106">Key</span><span class="sxs-lookup"><span data-stu-id="cd570-106">Key</span></span>

<span data-ttu-id="cd570-107">`IKey` 介面是 cryptosystem 中索引鍵的基本標記法。</span><span class="sxs-lookup"><span data-stu-id="cd570-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="cd570-108">這裡的關鍵字是用在抽象概念中，而不是「密碼編譯金鑰內容」的常值意義。</span><span class="sxs-lookup"><span data-stu-id="cd570-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="cd570-109">金鑰具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="cd570-109">A key has the following properties:</span></span>

* <span data-ttu-id="cd570-110">啟用、建立和到期日期</span><span class="sxs-lookup"><span data-stu-id="cd570-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="cd570-111">撤銷狀態</span><span class="sxs-lookup"><span data-stu-id="cd570-111">Revocation status</span></span>

* <span data-ttu-id="cd570-112">金鑰識別碼（GUID）</span><span class="sxs-lookup"><span data-stu-id="cd570-112">Key identifier (a GUID)</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="cd570-113">此外，`IKey` 會公開 `CreateEncryptor` 方法，可用來建立與此索引鍵系結的[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)實例。</span><span class="sxs-lookup"><span data-stu-id="cd570-113">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="cd570-114">此外，`IKey` 會公開 `CreateEncryptorInstance` 方法，可用來建立與此索引鍵系結的[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)實例。</span><span class="sxs-lookup"><span data-stu-id="cd570-114">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="cd570-115">沒有 API 可從 `IKey` 實例中抓取未經處理的密碼編譯內容。</span><span class="sxs-lookup"><span data-stu-id="cd570-115">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="cd570-116">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="cd570-116">IKeyManager</span></span>

<span data-ttu-id="cd570-117">`IKeyManager` 介面代表負責一般金鑰儲存、抓取和操作的物件。</span><span class="sxs-lookup"><span data-stu-id="cd570-117">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="cd570-118">它會公開三個高階作業：</span><span class="sxs-lookup"><span data-stu-id="cd570-118">It exposes three high-level operations:</span></span>

* <span data-ttu-id="cd570-119">建立新的金鑰，並將它保存在儲存體中。</span><span class="sxs-lookup"><span data-stu-id="cd570-119">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="cd570-120">從儲存體取得所有金鑰。</span><span class="sxs-lookup"><span data-stu-id="cd570-120">Get all keys from storage.</span></span>

* <span data-ttu-id="cd570-121">撤銷一或多個金鑰，並將撤銷資訊保存到儲存體。</span><span class="sxs-lookup"><span data-stu-id="cd570-121">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="cd570-122">撰寫 `IKeyManager` 是一項非常先進的工作，大部分的開發人員都不應該嘗試這麼做。</span><span class="sxs-lookup"><span data-stu-id="cd570-122">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="cd570-123">相反地，大部分的開發人員都應該利用[XmlKeyManager](#xmlkeymanager)類別所提供的功能。</span><span class="sxs-lookup"><span data-stu-id="cd570-123">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](#xmlkeymanager) class.</span></span>

## <a name="xmlkeymanager"></a><span data-ttu-id="cd570-124">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="cd570-124">XmlKeyManager</span></span>

<span data-ttu-id="cd570-125">`XmlKeyManager` 類型是 `IKeyManager`的內建實體執行。</span><span class="sxs-lookup"><span data-stu-id="cd570-125">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="cd570-126">它提供數個實用的功能，包括待用金鑰的金鑰委付和加密。</span><span class="sxs-lookup"><span data-stu-id="cd570-126">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="cd570-127">此系統中的索引鍵會以 XML 元素（具體而言是[system.xml.linq.xelement>](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)）來表示。</span><span class="sxs-lookup"><span data-stu-id="cd570-127">Keys in this system are represented as XML elements (specifically, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="cd570-128">`XmlKeyManager` 相依于完成其工作的過程中的數個其他元件：</span><span class="sxs-lookup"><span data-stu-id="cd570-128">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="cd570-129">`AlgorithmConfiguration`，其規定新索引鍵所使用的演算法。</span><span class="sxs-lookup"><span data-stu-id="cd570-129">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="cd570-130">`IXmlRepository`，控制金鑰在儲存體中的保存位置。</span><span class="sxs-lookup"><span data-stu-id="cd570-130">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="cd570-131">`IXmlEncryptor` [選用]，允許待用加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="cd570-131">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="cd570-132">`IKeyEscrowSink` [選用]，提供金鑰委付服務。</span><span class="sxs-lookup"><span data-stu-id="cd570-132">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="cd570-133">`IXmlRepository`，控制金鑰在儲存體中的保存位置。</span><span class="sxs-lookup"><span data-stu-id="cd570-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="cd570-134">`IXmlEncryptor` [選用]，允許待用加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="cd570-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="cd570-135">`IKeyEscrowSink` [選用]，提供金鑰委付服務。</span><span class="sxs-lookup"><span data-stu-id="cd570-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

<span data-ttu-id="cd570-136">以下是高階圖表，指出如何在 `XmlKeyManager`內將這些元件連結在一起。</span><span class="sxs-lookup"><span data-stu-id="cd570-136">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

::: moniker range=">= aspnetcore-2.0"

![建立金鑰](key-management/_static/keycreation2.png)

<span data-ttu-id="cd570-138">*金鑰建立/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="cd570-138">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="cd570-139">在 `CreateNewKey`的執行中，`AlgorithmConfiguration` 元件會用來建立唯一的 `IAuthenticatedEncryptorDescriptor`，然後再序列化為 XML。</span><span class="sxs-lookup"><span data-stu-id="cd570-139">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="cd570-140">如果有金鑰委付接收，則會提供原始（未加密）的 XML 給接收以進行長期儲存。</span><span class="sxs-lookup"><span data-stu-id="cd570-140">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="cd570-141">未加密的 XML 接著會透過 `IXmlEncryptor` （如有必要）執行，以產生加密的 XML 檔。</span><span class="sxs-lookup"><span data-stu-id="cd570-141">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="cd570-142">此加密檔會透過 `IXmlRepository`保存到長期儲存體。</span><span class="sxs-lookup"><span data-stu-id="cd570-142">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="cd570-143">（如果未設定任何 `IXmlEncryptor`，未加密的檔會保存在 `IXmlRepository`中）。</span><span class="sxs-lookup"><span data-stu-id="cd570-143">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![金鑰抓取](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![建立金鑰](key-management/_static/keycreation1.png)

<span data-ttu-id="cd570-146">*金鑰建立/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="cd570-146">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="cd570-147">在 `CreateNewKey`的執行中，`IAuthenticatedEncryptorConfiguration` 元件會用來建立唯一的 `IAuthenticatedEncryptorDescriptor`，然後再序列化為 XML。</span><span class="sxs-lookup"><span data-stu-id="cd570-147">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="cd570-148">如果有金鑰委付接收，則會提供原始（未加密）的 XML 給接收以進行長期儲存。</span><span class="sxs-lookup"><span data-stu-id="cd570-148">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="cd570-149">未加密的 XML 接著會透過 `IXmlEncryptor` （如有必要）執行，以產生加密的 XML 檔。</span><span class="sxs-lookup"><span data-stu-id="cd570-149">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="cd570-150">此加密檔會透過 `IXmlRepository`保存到長期儲存體。</span><span class="sxs-lookup"><span data-stu-id="cd570-150">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="cd570-151">（如果未設定任何 `IXmlEncryptor`，未加密的檔會保存在 `IXmlRepository`中）。</span><span class="sxs-lookup"><span data-stu-id="cd570-151">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![金鑰抓取](key-management/_static/keyretrieval1.png)

::: moniker-end

<span data-ttu-id="cd570-153">*金鑰抓取/GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="cd570-153">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="cd570-154">在 `GetAllKeys`的執行中，會從基礎 `IXmlRepository`讀取代表索引鍵和撤銷的 XML 檔。</span><span class="sxs-lookup"><span data-stu-id="cd570-154">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="cd570-155">如果這些檔已加密，系統將會自動將其解密。</span><span class="sxs-lookup"><span data-stu-id="cd570-155">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="cd570-156">`XmlKeyManager` 會建立適當的 `IAuthenticatedEncryptorDescriptorDeserializer` 實例，將檔案還原序列化回 `IAuthenticatedEncryptorDescriptor` 實例中，然後再包裝在個別的 `IKey` 實例中。</span><span class="sxs-lookup"><span data-stu-id="cd570-156">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="cd570-157">這個 `IKey` 實例的集合會傳回給呼叫者。</span><span class="sxs-lookup"><span data-stu-id="cd570-157">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="cd570-158">如需特定 XML 元素的進一步資訊，請參閱[金鑰儲存體格式檔](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format)。</span><span class="sxs-lookup"><span data-stu-id="cd570-158">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="cd570-159">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="cd570-159">IXmlRepository</span></span>

<span data-ttu-id="cd570-160">`IXmlRepository` 介面代表可以保存 XML 並從備份存放區抓取 XML 的類型。</span><span class="sxs-lookup"><span data-stu-id="cd570-160">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="cd570-161">它會公開兩個 Api：</span><span class="sxs-lookup"><span data-stu-id="cd570-161">It exposes two APIs:</span></span>

* <span data-ttu-id="cd570-162">`GetAllElements`：`IReadOnlyCollection<XElement>`</span><span class="sxs-lookup"><span data-stu-id="cd570-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span></span>

* `StoreElement(XElement element, string friendlyName)`

<span data-ttu-id="cd570-163">`IXmlRepository` 的執行不需要剖析傳遞的 XML。</span><span class="sxs-lookup"><span data-stu-id="cd570-163">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="cd570-164">它們應該將 XML 檔視為不透明，並讓較高層的使用者擔心產生和剖析檔。</span><span class="sxs-lookup"><span data-stu-id="cd570-164">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="cd570-165">有四個內建的具象類型，可實作為 `IXmlRepository`：</span><span class="sxs-lookup"><span data-stu-id="cd570-165">There are four built-in concrete types which implement `IXmlRepository`:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="cd570-166">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="cd570-166">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="cd570-167">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="cd570-167">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="cd570-168">AzureStorage. AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="cd570-168">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="cd570-169">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="cd570-169">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="cd570-170">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="cd570-170">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="cd570-171">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="cd570-171">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="cd570-172">AzureStorage. AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="cd570-172">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="cd570-173">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="cd570-173">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

<span data-ttu-id="cd570-174">如需詳細資訊，請參閱[金鑰儲存提供者檔](xref:security/data-protection/implementation/key-storage-providers)。</span><span class="sxs-lookup"><span data-stu-id="cd570-174">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers) for more information.</span></span>

<span data-ttu-id="cd570-175">使用不同的備份存放區（例如 Azure 表格儲存體）時，註冊自訂 `IXmlRepository` 是適當的。</span><span class="sxs-lookup"><span data-stu-id="cd570-175">Registering a custom `IXmlRepository` is appropriate when using a different backing store (for example, Azure Table Storage).</span></span>

<span data-ttu-id="cd570-176">若要變更預設的儲存機制應用程式範圍，請註冊自訂的 `IXmlRepository` 實例：</span><span class="sxs-lookup"><span data-stu-id="cd570-176">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

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

## <a name="ixmlencryptor"></a><span data-ttu-id="cd570-177">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="cd570-177">IXmlEncryptor</span></span>

<span data-ttu-id="cd570-178">`IXmlEncryptor` 介面代表可以加密純文字 XML 元素的類型。</span><span class="sxs-lookup"><span data-stu-id="cd570-178">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="cd570-179">它會公開單一 API：</span><span class="sxs-lookup"><span data-stu-id="cd570-179">It exposes a single API:</span></span>

* <span data-ttu-id="cd570-180">Encrypt （System.xml.linq.xelement> plaiNtextElement）： EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="cd570-180">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="cd570-181">如果序列化 `IAuthenticatedEncryptorDescriptor` 包含標記為「需要加密」的任何元素，則 `XmlKeyManager` 會透過所設定 `IXmlEncryptor`的 `Encrypt` 方法來執行這些專案，而它會將 enciphered 專案（而不是純文字元素）保存至 `IXmlRepository`。</span><span class="sxs-lookup"><span data-stu-id="cd570-181">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="cd570-182">`Encrypt` 方法的輸出是 `EncryptedXmlInfo` 物件。</span><span class="sxs-lookup"><span data-stu-id="cd570-182">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="cd570-183">這個物件是一個包裝函式，其中同時包含結果 enciphered `XElement` 和類型，其代表可用來解密對應元素的 `IXmlDecryptor`。</span><span class="sxs-lookup"><span data-stu-id="cd570-183">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="cd570-184">有四個內建的具象類型，可實作為 `IXmlEncryptor`：</span><span class="sxs-lookup"><span data-stu-id="cd570-184">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>

* [<span data-ttu-id="cd570-185">CertificateXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="cd570-185">CertificateXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [<span data-ttu-id="cd570-186">DpapiNGXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="cd570-186">DpapiNGXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [<span data-ttu-id="cd570-187">DpapiXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="cd570-187">DpapiXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [<span data-ttu-id="cd570-188">NullXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="cd570-188">NullXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

<span data-ttu-id="cd570-189">如需詳細資訊，請參閱待用[金鑰加密檔](xref:security/data-protection/implementation/key-encryption-at-rest)。</span><span class="sxs-lookup"><span data-stu-id="cd570-189">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="cd570-190">若要變更整個應用程式的預設金鑰加密機制，請註冊自訂的 `IXmlEncryptor` 實例：</span><span class="sxs-lookup"><span data-stu-id="cd570-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

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

## <a name="ixmldecryptor"></a><span data-ttu-id="cd570-191">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="cd570-191">IXmlDecryptor</span></span>

<span data-ttu-id="cd570-192">`IXmlDecryptor` 介面代表的類型，知道如何解密透過 `IXmlEncryptor`enciphered 的 `XElement`。</span><span class="sxs-lookup"><span data-stu-id="cd570-192">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="cd570-193">它會公開單一 API：</span><span class="sxs-lookup"><span data-stu-id="cd570-193">It exposes a single API:</span></span>

* <span data-ttu-id="cd570-194">解密（System.xml.linq.xelement> encryptedElement）： System.xml.linq.xelement></span><span class="sxs-lookup"><span data-stu-id="cd570-194">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="cd570-195">`Decrypt` 方法會復原 `IXmlEncryptor.Encrypt`所執行的加密。</span><span class="sxs-lookup"><span data-stu-id="cd570-195">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="cd570-196">一般來說，每個具體 `IXmlEncryptor` 的執行都會有對應的具體 `IXmlDecryptor` 執行。</span><span class="sxs-lookup"><span data-stu-id="cd570-196">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="cd570-197">實 `IXmlDecryptor` 的型別應該具有下列兩個公用函式的其中一個：</span><span class="sxs-lookup"><span data-stu-id="cd570-197">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="cd570-198">.ctor （IServiceProvider）</span><span class="sxs-lookup"><span data-stu-id="cd570-198">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="cd570-199">.ctor （）</span><span class="sxs-lookup"><span data-stu-id="cd570-199">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="cd570-200">傳遞至此函式的 `IServiceProvider` 可能是 null。</span><span class="sxs-lookup"><span data-stu-id="cd570-200">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="cd570-201">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="cd570-201">IKeyEscrowSink</span></span>

<span data-ttu-id="cd570-202">`IKeyEscrowSink` 介面代表可執行機密資訊的類型。</span><span class="sxs-lookup"><span data-stu-id="cd570-202">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="cd570-203">回想一下，序列化的描述項可能包含機密資訊（例如密碼編譯內容），而這就是第一次引進[IXmlEncryptor](#ixmlencryptor)類型的地方。</span><span class="sxs-lookup"><span data-stu-id="cd570-203">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](#ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="cd570-204">不過，發生事故，而且可以刪除或損毀金鑰環。</span><span class="sxs-lookup"><span data-stu-id="cd570-204">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="cd570-205">「證書處理」介面提供緊急的 escape 影線，允許在任何已設定的[IXmlEncryptor](#ixmlencryptor)轉換原始序列化的 XML 之前，先對其進行存取。</span><span class="sxs-lookup"><span data-stu-id="cd570-205">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](#ixmlencryptor).</span></span> <span data-ttu-id="cd570-206">介面會公開單一 API：</span><span class="sxs-lookup"><span data-stu-id="cd570-206">The interface exposes a single API:</span></span>

* <span data-ttu-id="cd570-207">Store （Guid keyId，System.xml.linq.xelement> 元素）</span><span class="sxs-lookup"><span data-stu-id="cd570-207">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="cd570-208">以安全的方式處理提供的專案是以與商務原則一致的方法來完成。 `IKeyEscrowSink`</span><span class="sxs-lookup"><span data-stu-id="cd570-208">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="cd570-209">使用已知的公司 x.509 憑證（其中憑證的私密金鑰已委付）時，可能的執行方式之一是`CertificateXmlEncryptor` 類型可以協助這種情況。</span><span class="sxs-lookup"><span data-stu-id="cd570-209">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="cd570-210">`IKeyEscrowSink` 的執行也會負責適當保存提供的元素。</span><span class="sxs-lookup"><span data-stu-id="cd570-210">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="cd570-211">預設不會啟用任何證書機制，雖然伺服器管理員可以[全域進行設定](xref:security/data-protection/configuration/machine-wide-policy)。</span><span class="sxs-lookup"><span data-stu-id="cd570-211">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="cd570-212">也可以透過 `IDataProtectionBuilder.AddKeyEscrowSink` 方法以程式設計方式進行設定，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="cd570-212">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="cd570-213">`AddKeyEscrowSink` 的方法多載會鏡像 `IServiceCollection.AddSingleton` 和 `IServiceCollection.AddInstance` 多載，因為 `IKeyEscrowSink` 實例會單次個體。</span><span class="sxs-lookup"><span data-stu-id="cd570-213">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="cd570-214">如果註冊了多個 `IKeyEscrowSink` 實例，則會在金鑰產生期間呼叫每一個實例，因此可以同時將金鑰委付到多個機制。</span><span class="sxs-lookup"><span data-stu-id="cd570-214">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="cd570-215">沒有 API 可從 `IKeyEscrowSink` 實例讀取材質。</span><span class="sxs-lookup"><span data-stu-id="cd570-215">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="cd570-216">這與證書處理機制的設計理論一致：其目的是要讓受信任的授權單位能夠存取金鑰材料，而且因為應用程式本身不是受信任的授權單位，所以不應該存取自己的委付資料。</span><span class="sxs-lookup"><span data-stu-id="cd570-216">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="cd570-217">下列範例程式碼示範如何建立和註冊 `IKeyEscrowSink`，其中委付金鑰，讓只有 "CONTOSODomain Admins" 的成員可以復原它們。</span><span class="sxs-lookup"><span data-stu-id="cd570-217">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="cd570-218">若要執行此範例，您必須位於已加入網域的 Windows 8/Windows Server 2012 電腦，且網域控制站必須是 Windows Server 2012 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="cd570-218">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
