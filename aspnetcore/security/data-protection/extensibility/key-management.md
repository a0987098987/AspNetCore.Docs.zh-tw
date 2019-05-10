---
title: ASP.NET Core 中的金鑰管理擴充性
author: rick-anderson
description: 深入了解 ASP.NET Core 資料保護金鑰管理擴充性。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896905"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="953dd-103">ASP.NET Core 中的金鑰管理擴充性</span><span class="sxs-lookup"><span data-stu-id="953dd-103">Key management extensibility in ASP.NET Core</span></span>

> [!TIP]
> <span data-ttu-id="953dd-104">讀取[金鑰管理](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)閱讀本節中，因為它會說明這些 Api 的基本概念的一些之前的一節。</span><span class="sxs-lookup"><span data-stu-id="953dd-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="953dd-105">會實作下列介面的任何的類型應該是安全執行緒的多個呼叫端。</span><span class="sxs-lookup"><span data-stu-id="953dd-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="953dd-106">Key</span><span class="sxs-lookup"><span data-stu-id="953dd-106">Key</span></span>

<span data-ttu-id="953dd-107">`IKey`介面是加密系統中的索引鍵的基本表示法。</span><span class="sxs-lookup"><span data-stu-id="953dd-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="953dd-108">詞彙索引鍵此處用抽象的意義而言，不在 「 密碼編譯金鑰的內容 」 的常值的意義。</span><span class="sxs-lookup"><span data-stu-id="953dd-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="953dd-109">索引鍵具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="953dd-109">A key has the following properties:</span></span>

* <span data-ttu-id="953dd-110">啟用、 建立和到期日期</span><span class="sxs-lookup"><span data-stu-id="953dd-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="953dd-111">撤銷狀態</span><span class="sxs-lookup"><span data-stu-id="953dd-111">Revocation status</span></span>

* <span data-ttu-id="953dd-112">索引鍵識別項 (GUID)</span><span class="sxs-lookup"><span data-stu-id="953dd-112">Key identifier (a GUID)</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="953dd-113">此外，`IKey`公開`CreateEncryptor`方法可用來建立[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)執行個體繫結至這個索引鍵。</span><span class="sxs-lookup"><span data-stu-id="953dd-113">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="953dd-114">此外，`IKey`公開`CreateEncryptorInstance`方法可用來建立[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)執行個體繫結至這個索引鍵。</span><span class="sxs-lookup"><span data-stu-id="953dd-114">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="953dd-115">不沒有要擷取的未經處理的加密編譯內容，從任何 API`IKey`執行個體。</span><span class="sxs-lookup"><span data-stu-id="953dd-115">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="953dd-116">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="953dd-116">IKeyManager</span></span>

<span data-ttu-id="953dd-117">`IKeyManager`介面代表負責進行一般的金鑰儲存、 擷取及操作的物件。</span><span class="sxs-lookup"><span data-stu-id="953dd-117">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="953dd-118">它會公開三個高層級的作業：</span><span class="sxs-lookup"><span data-stu-id="953dd-118">It exposes three high-level operations:</span></span>

* <span data-ttu-id="953dd-119">建立新的金鑰，並將它保存到儲存體。</span><span class="sxs-lookup"><span data-stu-id="953dd-119">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="953dd-120">從儲存體中取得所有索引鍵。</span><span class="sxs-lookup"><span data-stu-id="953dd-120">Get all keys from storage.</span></span>

* <span data-ttu-id="953dd-121">撤銷一或多個金鑰，並保存至儲存體的撤銷資訊。</span><span class="sxs-lookup"><span data-stu-id="953dd-121">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="953dd-122">撰寫`IKeyManager`非常進階的工作中，而且大部分的開發人員不應該嘗試它。</span><span class="sxs-lookup"><span data-stu-id="953dd-122">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="953dd-123">相反地，大部分的開發人員應該利用所提供的功能[XmlKeyManager](#xmlkeymanager)類別。</span><span class="sxs-lookup"><span data-stu-id="953dd-123">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](#xmlkeymanager) class.</span></span>

## <a name="xmlkeymanager"></a><span data-ttu-id="953dd-124">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="953dd-124">XmlKeyManager</span></span>

<span data-ttu-id="953dd-125">`XmlKeyManager`型別是內建的具體實作`IKeyManager`。</span><span class="sxs-lookup"><span data-stu-id="953dd-125">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="953dd-126">它提供數個實用的功能，包括金鑰委付和待用的金鑰加密。</span><span class="sxs-lookup"><span data-stu-id="953dd-126">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="953dd-127">在此系統中的索引鍵會表示為 XML 項目 (具體而言， [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview))。</span><span class="sxs-lookup"><span data-stu-id="953dd-127">Keys in this system are represented as XML elements (specifically, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="953dd-128">`XmlKeyManager` 在完成其工作的過程中的數個其他元件而定：</span><span class="sxs-lookup"><span data-stu-id="953dd-128">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="953dd-129">`AlgorithmConfiguration`指出新的索引鍵所使用的演算法。</span><span class="sxs-lookup"><span data-stu-id="953dd-129">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="953dd-130">`IXmlRepository`其中索引鍵會保存在儲存體中的控制項。</span><span class="sxs-lookup"><span data-stu-id="953dd-130">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="953dd-131">`IXmlEncryptor` [選擇性]，可讓加密待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="953dd-131">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="953dd-132">`IKeyEscrowSink` [選擇性]，它會提供金鑰委付服務。</span><span class="sxs-lookup"><span data-stu-id="953dd-132">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="953dd-133">`IXmlRepository`其中索引鍵會保存在儲存體中的控制項。</span><span class="sxs-lookup"><span data-stu-id="953dd-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="953dd-134">`IXmlEncryptor` [選擇性]，可讓加密待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="953dd-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="953dd-135">`IKeyEscrowSink` [選擇性]，它會提供金鑰委付服務。</span><span class="sxs-lookup"><span data-stu-id="953dd-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

<span data-ttu-id="953dd-136">以下是高階的圖表指出如何這些元件連接起來內`XmlKeyManager`。</span><span class="sxs-lookup"><span data-stu-id="953dd-136">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

::: moniker range=">= aspnetcore-2.0"

![建立金鑰](key-management/_static/keycreation2.png)

<span data-ttu-id="953dd-138">*索引鍵建立 / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="953dd-138">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="953dd-139">在實作`CreateNewKey`，則`AlgorithmConfiguration`元件用來建立唯一`IAuthenticatedEncryptorDescriptor`，這接著會序列化為 XML。</span><span class="sxs-lookup"><span data-stu-id="953dd-139">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="953dd-140">如果金鑰委付接收，則原始 （未加密） 的 XML 可接收長期的儲存體。</span><span class="sxs-lookup"><span data-stu-id="953dd-140">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="953dd-141">未加密的 XML 接著會執行`IXmlEncryptor`（如有必要） 來產生加密的 XML 文件。</span><span class="sxs-lookup"><span data-stu-id="953dd-141">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="953dd-142">這個加密的文件會保存到長期儲存體透過`IXmlRepository`。</span><span class="sxs-lookup"><span data-stu-id="953dd-142">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="953dd-143">(如果有任何`IXmlEncryptor`已設定，未加密的文件會保存在`IXmlRepository`。)</span><span class="sxs-lookup"><span data-stu-id="953dd-143">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![金鑰擷取](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![建立金鑰](key-management/_static/keycreation1.png)

<span data-ttu-id="953dd-146">*索引鍵建立 / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="953dd-146">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="953dd-147">在實作`CreateNewKey`，則`IAuthenticatedEncryptorConfiguration`元件用來建立唯一`IAuthenticatedEncryptorDescriptor`，這接著會序列化為 XML。</span><span class="sxs-lookup"><span data-stu-id="953dd-147">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="953dd-148">如果金鑰委付接收，則原始 （未加密） 的 XML 可接收長期的儲存體。</span><span class="sxs-lookup"><span data-stu-id="953dd-148">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="953dd-149">未加密的 XML 接著會執行`IXmlEncryptor`（如有必要） 來產生加密的 XML 文件。</span><span class="sxs-lookup"><span data-stu-id="953dd-149">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="953dd-150">這個加密的文件會保存到長期儲存體透過`IXmlRepository`。</span><span class="sxs-lookup"><span data-stu-id="953dd-150">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="953dd-151">(如果有任何`IXmlEncryptor`已設定，未加密的文件會保存在`IXmlRepository`。)</span><span class="sxs-lookup"><span data-stu-id="953dd-151">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![金鑰擷取](key-management/_static/keyretrieval1.png)

::: moniker-end

<span data-ttu-id="953dd-153">*索引鍵擷取 / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="953dd-153">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="953dd-154">在實作`GetAllKeys`、 XML 文件表示的金鑰和撤銷讀取基礎`IXmlRepository`。</span><span class="sxs-lookup"><span data-stu-id="953dd-154">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="953dd-155">如果這些文件已加密，系統會自動解密。</span><span class="sxs-lookup"><span data-stu-id="953dd-155">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="953dd-156">`XmlKeyManager` 建立適當`IAuthenticatedEncryptorDescriptorDeserializer`還原序列化文件的執行個體回到`IAuthenticatedEncryptorDescriptor`執行個體，然後將包裝在個別`IKey`執行個體。</span><span class="sxs-lookup"><span data-stu-id="953dd-156">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="953dd-157">這個集合`IKey`執行個體傳回給呼叫端。</span><span class="sxs-lookup"><span data-stu-id="953dd-157">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="953dd-158">在特定的 XML 項目上的進一步資訊可在[金鑰儲存格式的文件](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format)。</span><span class="sxs-lookup"><span data-stu-id="953dd-158">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="953dd-159">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="953dd-159">IXmlRepository</span></span>

<span data-ttu-id="953dd-160">`IXmlRepository`介面代表可保存到 XML，並從備份存放區中擷取 XML 類型。</span><span class="sxs-lookup"><span data-stu-id="953dd-160">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="953dd-161">它會公開兩個 Api:</span><span class="sxs-lookup"><span data-stu-id="953dd-161">It exposes two APIs:</span></span>

* <span data-ttu-id="953dd-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span><span class="sxs-lookup"><span data-stu-id="953dd-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span></span>

* `StoreElement(XElement element, string friendlyName)`

<span data-ttu-id="953dd-163">實作`IXmlRepository`不需要剖析 XML 傳遞給它們。</span><span class="sxs-lookup"><span data-stu-id="953dd-163">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="953dd-164">它們應該視為不透明的 XML 文件，並讓擔心產生和剖析的文件的更高層。</span><span class="sxs-lookup"><span data-stu-id="953dd-164">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="953dd-165">有四種內建的具象類型實作`IXmlRepository`:</span><span class="sxs-lookup"><span data-stu-id="953dd-165">There are four built-in concrete types which implement `IXmlRepository`:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="953dd-166">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="953dd-166">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="953dd-167">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="953dd-167">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="953dd-168">AzureStorage.AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="953dd-168">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="953dd-169">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="953dd-169">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="953dd-170">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="953dd-170">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="953dd-171">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="953dd-171">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="953dd-172">AzureStorage.AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="953dd-172">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="953dd-173">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="953dd-173">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

<span data-ttu-id="953dd-174">請參閱[金鑰儲存提供者文件](xref:security/data-protection/implementation/key-storage-providers)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="953dd-174">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers) for more information.</span></span>

<span data-ttu-id="953dd-175">註冊自訂`IXmlRepository`適合使用不同的備份存放區 （例如 Azure 資料表儲存體） 時。</span><span class="sxs-lookup"><span data-stu-id="953dd-175">Registering a custom `IXmlRepository` is appropriate when using a different backing store (for example, Azure Table Storage).</span></span>

<span data-ttu-id="953dd-176">若要變更預設存放庫整個應用程式，註冊自訂`IXmlRepository`執行個體：</span><span class="sxs-lookup"><span data-stu-id="953dd-176">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

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

## <a name="ixmlencryptor"></a><span data-ttu-id="953dd-177">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="953dd-177">IXmlEncryptor</span></span>

<span data-ttu-id="953dd-178">`IXmlEncryptor`介面代表一種類型，可以將加密的純文字 XML 項目。</span><span class="sxs-lookup"><span data-stu-id="953dd-178">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="953dd-179">它會公開單一的 API:</span><span class="sxs-lookup"><span data-stu-id="953dd-179">It exposes a single API:</span></span>

* <span data-ttu-id="953dd-180">Encrypt(XElement plaintextElement):EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="953dd-180">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="953dd-181">如果序列化`IAuthenticatedEncryptorDescriptor`包含任何項目，然後標示為 「 需要加密 」`XmlKeyManager`會透過已設定執行這些項目`IXmlEncryptor`的`Encrypt`方法，且其會保存已譯成密碼的項目而非以純文字項目`IXmlRepository`。</span><span class="sxs-lookup"><span data-stu-id="953dd-181">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="953dd-182">輸出`Encrypt`方法是`EncryptedXmlInfo`物件。</span><span class="sxs-lookup"><span data-stu-id="953dd-182">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="953dd-183">這個物件是其中包含已譯成密碼的這兩個結果的包裝函式`XElement`代表的類型和`IXmlDecryptor`這可用來解密對應的項目。</span><span class="sxs-lookup"><span data-stu-id="953dd-183">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="953dd-184">有四種內建的具象類型實作`IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="953dd-184">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>

* [<span data-ttu-id="953dd-185">CertificateXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="953dd-185">CertificateXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [<span data-ttu-id="953dd-186">DpapiNGXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="953dd-186">DpapiNGXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [<span data-ttu-id="953dd-187">DpapiXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="953dd-187">DpapiXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [<span data-ttu-id="953dd-188">NullXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="953dd-188">NullXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

<span data-ttu-id="953dd-189">請參閱[rest 文件的金鑰加密](xref:security/data-protection/implementation/key-encryption-at-rest)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="953dd-189">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="953dd-190">若要變更預設的索引鍵--待用加密機制整個應用程式，註冊自訂`IXmlEncryptor`執行個體：</span><span class="sxs-lookup"><span data-stu-id="953dd-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

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

## <a name="ixmldecryptor"></a><span data-ttu-id="953dd-191">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="953dd-191">IXmlDecryptor</span></span>

<span data-ttu-id="953dd-192">`IXmlDecryptor`介面表示型別，以便將解密`XElement`會是已譯成密碼透過`IXmlEncryptor`。</span><span class="sxs-lookup"><span data-stu-id="953dd-192">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="953dd-193">它會公開單一的 API:</span><span class="sxs-lookup"><span data-stu-id="953dd-193">It exposes a single API:</span></span>

* <span data-ttu-id="953dd-194">Decrypt(XElement encryptedElement) :XElement</span><span class="sxs-lookup"><span data-stu-id="953dd-194">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="953dd-195">`Decrypt`方法復原所執行的加密`IXmlEncryptor.Encrypt`。</span><span class="sxs-lookup"><span data-stu-id="953dd-195">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="953dd-196">一般而言，每個具體`IXmlEncryptor`實作會有相對應的具體`IXmlDecryptor`實作。</span><span class="sxs-lookup"><span data-stu-id="953dd-196">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="953dd-197">型別，能夠實作`IXmlDecryptor`應該有下列兩個公用建構函式的其中一個：</span><span class="sxs-lookup"><span data-stu-id="953dd-197">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="953dd-198">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="953dd-198">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="953dd-199">.ctor()</span><span class="sxs-lookup"><span data-stu-id="953dd-199">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="953dd-200">`IServiceProvider`傳遞至建構函式可能是 null。</span><span class="sxs-lookup"><span data-stu-id="953dd-200">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="953dd-201">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="953dd-201">IKeyEscrowSink</span></span>

<span data-ttu-id="953dd-202">`IKeyEscrowSink`介面代表可執行的機密資訊的委付的型別。</span><span class="sxs-lookup"><span data-stu-id="953dd-202">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="953dd-203">前文提過，序列化的項目中可能包含機密資訊 （例如密碼編譯的內容），而這就是導致引進[IXmlEncryptor](#ixmlencryptor)一開始輸入。</span><span class="sxs-lookup"><span data-stu-id="953dd-203">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](#ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="953dd-204">不過，無處不在而金鑰環可以刪除或損毀。</span><span class="sxs-lookup"><span data-stu-id="953dd-204">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="953dd-205">委付介面提供的緊急出口，允許存取原始的序列化 XML，它由任何設定轉換前[IXmlEncryptor](#ixmlencryptor)。</span><span class="sxs-lookup"><span data-stu-id="953dd-205">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](#ixmlencryptor).</span></span> <span data-ttu-id="953dd-206">此介面會公開單一的 API:</span><span class="sxs-lookup"><span data-stu-id="953dd-206">The interface exposes a single API:</span></span>

* <span data-ttu-id="953dd-207">存放區 （XElement 項目中的 Guid keyId）</span><span class="sxs-lookup"><span data-stu-id="953dd-207">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="953dd-208">最多是`IKeyEscrowSink`實作，以處理提供的項目，以安全的方式與商務原則一致。</span><span class="sxs-lookup"><span data-stu-id="953dd-208">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="953dd-209">一個可能的實作可能有無法委付接收加密使用已知的公司 X.509 憑證的 XML 項目，其中憑證的私密金鑰已委付;`CertificateXmlEncryptor`可以協助進行此設定類型。</span><span class="sxs-lookup"><span data-stu-id="953dd-209">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="953dd-210">`IKeyEscrowSink`實作也會負責適當地保存提供的項目。</span><span class="sxs-lookup"><span data-stu-id="953dd-210">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="953dd-211">預設沒有委付機制會啟用，但伺服器系統管理員可以[全域設定此](xref:security/data-protection/configuration/machine-wide-policy)。</span><span class="sxs-lookup"><span data-stu-id="953dd-211">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="953dd-212">它也可以設定以程式設計方式透過`IDataProtectionBuilder.AddKeyEscrowSink`方法，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="953dd-212">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="953dd-213">`AddKeyEscrowSink`方法多載鏡像`IServiceCollection.AddSingleton`並`IServiceCollection.AddInstance`多載，為`IKeyEscrowSink`主要執行個體做為單一子句。</span><span class="sxs-lookup"><span data-stu-id="953dd-213">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="953dd-214">如果有多個`IKeyEscrowSink`註冊執行個體，因此索引鍵可以委付至多個機制同時，將金鑰在產生期間，呼叫每一個。</span><span class="sxs-lookup"><span data-stu-id="953dd-214">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="953dd-215">沒有任何 API，以讀取來自資料`IKeyEscrowSink`執行個體。</span><span class="sxs-lookup"><span data-stu-id="953dd-215">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="953dd-216">這是設計理論的委付機制的一致： 其目的是讓金鑰的內容是受信任的授權單位，可存取，因為應用程式本身不是受信任的授權單位，就不能存取它自己委付的內容。</span><span class="sxs-lookup"><span data-stu-id="953dd-216">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="953dd-217">下列範例程式碼示範如何建立和註冊`IKeyEscrowSink`其中，只有 「 CONTOSODomain 系統管理員 」 的成員可以復原已委付金鑰。</span><span class="sxs-lookup"><span data-stu-id="953dd-217">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="953dd-218">若要執行此範例中，您必須在已加入網域的 Windows 8 / Windows Server 2012 電腦和網域控制站必須是 Windows Server 2012 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="953dd-218">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
