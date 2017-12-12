---
title: "金鑰管理的擴充性"
author: rick-anderson
description: "本文概述 ASP.NET Core 資料保護金鑰管理的擴充性。"
keywords: "ASP.NET Core 資料保護、 金鑰管理"
ms.author: riande
manager: wpickett
ms.date: 11/22/2017
ms.topic: article
ms.assetid: 3606b251-8324-4485-8d52-582a2cd5cffb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 0702e13163c0208e9d2863e711b02ffb257f6260
ms.sourcegitcommit: e641c5794525f983485621860926d8ab4e7360c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2017
---
# <a name="key-management-extensibility"></a><span data-ttu-id="9865a-104">金鑰管理的擴充性</span><span class="sxs-lookup"><span data-stu-id="9865a-104">Key management extensibility</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="9865a-105">讀取[金鑰管理](../implementation/key-management.md#data-protection-implementation-key-management)閱讀本節中，因為它會說明這些 Api 的基本概念的某些之前 > 一節。</span><span class="sxs-lookup"><span data-stu-id="9865a-105">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="9865a-106">實作下列介面的型別應該是安全執行緒的多個呼叫端。</span><span class="sxs-lookup"><span data-stu-id="9865a-106">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="9865a-107">Key</span><span class="sxs-lookup"><span data-stu-id="9865a-107">Key</span></span>

<span data-ttu-id="9865a-108">`IKey`介面是加密系統都中的索引鍵的基本表示法。</span><span class="sxs-lookup"><span data-stu-id="9865a-108">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="9865a-109">這裡使用的詞彙的索引鍵是抽象的意義而言，不是處於 「 密碼編譯金鑰內容 」 的常值的意義。</span><span class="sxs-lookup"><span data-stu-id="9865a-109">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="9865a-110">索引鍵具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="9865a-110">A key has the following properties:</span></span>

* <span data-ttu-id="9865a-111">啟用、 建立和到期日期</span><span class="sxs-lookup"><span data-stu-id="9865a-111">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="9865a-112">撤銷狀態</span><span class="sxs-lookup"><span data-stu-id="9865a-112">Revocation status</span></span>

* <span data-ttu-id="9865a-113">金鑰識別碼 (GUID)</span><span class="sxs-lookup"><span data-stu-id="9865a-113">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9865a-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9865a-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9865a-115">此外，`IKey`公開`CreateEncryptor`方法可以用來建立[IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor)這個索引鍵繫結的執行個體。</span><span class="sxs-lookup"><span data-stu-id="9865a-115">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9865a-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9865a-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9865a-117">此外，`IKey`公開`CreateEncryptorInstance`方法可以用來建立[IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor)這個索引鍵繫結的執行個體。</span><span class="sxs-lookup"><span data-stu-id="9865a-117">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="9865a-118">沒有任何 API 來擷取未經處理的密碼編譯內容，從`IKey`執行個體。</span><span class="sxs-lookup"><span data-stu-id="9865a-118">There is no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="9865a-119">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="9865a-119">IKeyManager</span></span>

<span data-ttu-id="9865a-120">`IKeyManager`介面代表負責一般金鑰儲存、 擷取及管理的物件。</span><span class="sxs-lookup"><span data-stu-id="9865a-120">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="9865a-121">它會公開三個高階的作業：</span><span class="sxs-lookup"><span data-stu-id="9865a-121">It exposes three high-level operations:</span></span>

* <span data-ttu-id="9865a-122">建立新的金鑰，並將它保存到儲存體。</span><span class="sxs-lookup"><span data-stu-id="9865a-122">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="9865a-123">從存放區中取得所有索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9865a-123">Get all keys from storage.</span></span>

* <span data-ttu-id="9865a-124">撤銷一或多個金鑰，並將保存到儲存體的撤銷資訊。</span><span class="sxs-lookup"><span data-stu-id="9865a-124">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="9865a-125">撰寫`IKeyManager`非常進階的工作，並且大部分的開發人員不應嘗試它。</span><span class="sxs-lookup"><span data-stu-id="9865a-125">Writing an `IKeyManager` is a very advanced task, and the majority of developers should not attempt it.</span></span> <span data-ttu-id="9865a-126">相反地，大部分的開發人員應利用所提供的功能[XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager)類別。</span><span class="sxs-lookup"><span data-stu-id="9865a-126">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="9865a-127">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="9865a-127">XmlKeyManager</span></span>

<span data-ttu-id="9865a-128">`XmlKeyManager`型別是內建的具象實作`IKeyManager`。</span><span class="sxs-lookup"><span data-stu-id="9865a-128">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="9865a-129">它提供數個實用的功能，包括金鑰委付和加密在靜止的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9865a-129">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="9865a-130">在此系統中的索引鍵會表示為 XML 項目 (具體而言， [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview))。</span><span class="sxs-lookup"><span data-stu-id="9865a-130">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="9865a-131">`XmlKeyManager`在完成其工作過程中的其他數個元件而定：</span><span class="sxs-lookup"><span data-stu-id="9865a-131">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9865a-132">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9865a-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="9865a-133">`AlgorithmConfiguration`其中指出新的金鑰所使用的演算法。</span><span class="sxs-lookup"><span data-stu-id="9865a-133">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="9865a-134">`IXmlRepository`其中索引鍵會保存在儲存體中的控制項。</span><span class="sxs-lookup"><span data-stu-id="9865a-134">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="9865a-135">`IXmlEncryptor`[選用]，可讓加密在靜止的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9865a-135">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="9865a-136">`IKeyEscrowSink`[選用]，以提供金鑰委付服務。</span><span class="sxs-lookup"><span data-stu-id="9865a-136">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9865a-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9865a-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="9865a-138">`IXmlRepository`其中索引鍵會保存在儲存體中的控制項。</span><span class="sxs-lookup"><span data-stu-id="9865a-138">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="9865a-139">`IXmlEncryptor`[選用]，可讓加密在靜止的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9865a-139">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="9865a-140">`IKeyEscrowSink`[選用]，以提供金鑰委付服務。</span><span class="sxs-lookup"><span data-stu-id="9865a-140">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="9865a-141">以下是高層級圖，這表示如何這些元件有線內的一起`XmlKeyManager`。</span><span class="sxs-lookup"><span data-stu-id="9865a-141">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9865a-142">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9865a-142">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![建立金鑰](key-management/_static/keycreation2.png)

   <span data-ttu-id="9865a-144">*金鑰建立 / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="9865a-144">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="9865a-145">實作中`CreateNewKey`、`AlgorithmConfiguration`元件用來建立唯一`IAuthenticatedEncryptorDescriptor`，其接著會序列化為 XML。</span><span class="sxs-lookup"><span data-stu-id="9865a-145">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="9865a-146">如果金鑰委付接收存在時，原始 （未加密） 的 XML 供接收長期儲存。</span><span class="sxs-lookup"><span data-stu-id="9865a-146">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="9865a-147">然後透過執行未加密的 XML `IXmlEncryptor` （如有必要） 來產生加密的 XML 文件。</span><span class="sxs-lookup"><span data-stu-id="9865a-147">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="9865a-148">這個加密的文件會保存長期的儲存體透過`IXmlRepository`。</span><span class="sxs-lookup"><span data-stu-id="9865a-148">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="9865a-149">(如果有任何`IXmlEncryptor`是設定，未加密的文件會持續保留在`IXmlRepository`。)</span><span class="sxs-lookup"><span data-stu-id="9865a-149">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9865a-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9865a-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![建立金鑰](key-management/_static/keycreation1.png)

   <span data-ttu-id="9865a-152">*金鑰建立 / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="9865a-152">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="9865a-153">實作中`CreateNewKey`、`IAuthenticatedEncryptorConfiguration`元件用來建立唯一`IAuthenticatedEncryptorDescriptor`，其接著會序列化為 XML。</span><span class="sxs-lookup"><span data-stu-id="9865a-153">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="9865a-154">如果金鑰委付接收存在時，原始 （未加密） 的 XML 供接收長期儲存。</span><span class="sxs-lookup"><span data-stu-id="9865a-154">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="9865a-155">然後透過執行未加密的 XML `IXmlEncryptor` （如有必要） 來產生加密的 XML 文件。</span><span class="sxs-lookup"><span data-stu-id="9865a-155">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="9865a-156">這個加密的文件會保存長期的儲存體透過`IXmlRepository`。</span><span class="sxs-lookup"><span data-stu-id="9865a-156">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="9865a-157">(如果有任何`IXmlEncryptor`是設定，未加密的文件會持續保留在`IXmlRepository`。)</span><span class="sxs-lookup"><span data-stu-id="9865a-157">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9865a-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9865a-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![金鑰擷取](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9865a-160">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9865a-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![金鑰擷取](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="9865a-162">*索引鍵擷取 / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="9865a-162">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="9865a-163">在實作`GetAllKeys`、 XML 文件代表索引鍵和撤銷讀取基礎`IXmlRepository`。</span><span class="sxs-lookup"><span data-stu-id="9865a-163">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="9865a-164">如果要加密這些文件，系統會自動解密這些檔案。</span><span class="sxs-lookup"><span data-stu-id="9865a-164">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="9865a-165">`XmlKeyManager`建立適當`IAuthenticatedEncryptorDescriptorDeserializer`還原序列化文件的執行個體放回`IAuthenticatedEncryptorDescriptor`執行個體，然後將包裝在個別`IKey`執行個體。</span><span class="sxs-lookup"><span data-stu-id="9865a-165">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="9865a-166">此集合`IKey`執行個體傳回給呼叫者。</span><span class="sxs-lookup"><span data-stu-id="9865a-166">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="9865a-167">中可以找到特定的 XML 項目上的進一步資訊[金鑰儲存格式的文件](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format)。</span><span class="sxs-lookup"><span data-stu-id="9865a-167">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="9865a-168">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="9865a-168">IXmlRepository</span></span>

<span data-ttu-id="9865a-169">`IXmlRepository`介面代表可保存到 XML 和 XML 擷取備份存放區的類型。</span><span class="sxs-lookup"><span data-stu-id="9865a-169">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="9865a-170">它會公開兩個 Api:</span><span class="sxs-lookup"><span data-stu-id="9865a-170">It exposes two APIs:</span></span>

* <span data-ttu-id="9865a-171">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="9865a-171">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="9865a-172">StoreElement （XElement 項目，字串 friendlyName）</span><span class="sxs-lookup"><span data-stu-id="9865a-172">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="9865a-173">實作`IXmlRepository`不需要剖析 XML 傳遞給它們。</span><span class="sxs-lookup"><span data-stu-id="9865a-173">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="9865a-174">它們應該將視為不透明的 XML 文件，並讓較高層級擔心產生及剖析的文件。</span><span class="sxs-lookup"><span data-stu-id="9865a-174">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="9865a-175">有兩種內建的具象類型實作`IXmlRepository`:`FileSystemXmlRepository`和`RegistryXmlRepository`。</span><span class="sxs-lookup"><span data-stu-id="9865a-175">There are two built-in concrete types which implement `IXmlRepository`: `FileSystemXmlRepository` and `RegistryXmlRepository`.</span></span> <span data-ttu-id="9865a-176">請參閱[金鑰儲存提供者文件](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="9865a-176">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="9865a-177">註冊自訂`IXmlRepository`會是適當的方式來使用不同的備份存放區，例如 Azure Blob 儲存體。</span><span class="sxs-lookup"><span data-stu-id="9865a-177">Registering a custom `IXmlRepository` would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span>

<span data-ttu-id="9865a-178">若要變更預設儲存機制整個應用程式，註冊自訂`IXmlRepository`執行個體：</span><span class="sxs-lookup"><span data-stu-id="9865a-178">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9865a-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9865a-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9865a-180">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9865a-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="9865a-181">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="9865a-181">IXmlEncryptor</span></span>

<span data-ttu-id="9865a-182">`IXmlEncryptor`介面代表可以加密純文字 XML 項目類型。</span><span class="sxs-lookup"><span data-stu-id="9865a-182">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="9865a-183">它會公開單一 API:</span><span class="sxs-lookup"><span data-stu-id="9865a-183">It exposes a single API:</span></span>

* <span data-ttu-id="9865a-184">加密 (XElement plaintextElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="9865a-184">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="9865a-185">如果序列化`IAuthenticatedEncryptorDescriptor`包含任何項目，然後標示為 「 需要加密 」`XmlKeyManager`會執行這些項目，透過設定`IXmlEncryptor`的`Encrypt`方法，而且它將會保存 enciphered 項目而非以純文字項目`IXmlRepository`。</span><span class="sxs-lookup"><span data-stu-id="9865a-185">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="9865a-186">輸出`Encrypt`方法`EncryptedXmlInfo`物件。</span><span class="sxs-lookup"><span data-stu-id="9865a-186">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="9865a-187">此物件是包裝函式包含兩個結果 enciphered`XElement`代表的類型和`IXmlDecryptor`這可以用來解密的對應元素。</span><span class="sxs-lookup"><span data-stu-id="9865a-187">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="9865a-188">有四種內建的具象類型實作`IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="9865a-188">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

<span data-ttu-id="9865a-189">請參閱[在其他文件的金鑰加密](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="9865a-189">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="9865a-190">若要變更預設的索引鍵---靜態加密機制整個應用程式，註冊自訂`IXmlEncryptor`執行個體：</span><span class="sxs-lookup"><span data-stu-id="9865a-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9865a-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9865a-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9865a-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9865a-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a><span data-ttu-id="9865a-193">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="9865a-193">IXmlDecryptor</span></span>

<span data-ttu-id="9865a-194">`IXmlDecryptor`介面代表知道如何解密類型`XElement`，已透過 enciphered `IXmlEncryptor`。</span><span class="sxs-lookup"><span data-stu-id="9865a-194">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="9865a-195">它會公開單一 API:</span><span class="sxs-lookup"><span data-stu-id="9865a-195">It exposes a single API:</span></span>

* <span data-ttu-id="9865a-196">解密 (XElement encryptedElement): XElement</span><span class="sxs-lookup"><span data-stu-id="9865a-196">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="9865a-197">`Decrypt`方法復原所執行的加密`IXmlEncryptor.Encrypt`。</span><span class="sxs-lookup"><span data-stu-id="9865a-197">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="9865a-198">一般而言，每個實體`IXmlEncryptor`實作會有對應的具體`IXmlDecryptor`實作。</span><span class="sxs-lookup"><span data-stu-id="9865a-198">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="9865a-199">型別實作哪些`IXmlDecryptor`應該有下列兩個公用建構函式的其中一個：</span><span class="sxs-lookup"><span data-stu-id="9865a-199">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="9865a-200">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="9865a-200">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="9865a-201">.ctor</span><span class="sxs-lookup"><span data-stu-id="9865a-201">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="9865a-202">`IServiceProvider`傳遞至建構函式可能是 null。</span><span class="sxs-lookup"><span data-stu-id="9865a-202">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="9865a-203">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="9865a-203">IKeyEscrowSink</span></span>

<span data-ttu-id="9865a-204">`IKeyEscrowSink`介面代表可執行委付機密資訊的類型。</span><span class="sxs-lookup"><span data-stu-id="9865a-204">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="9865a-205">請注意，序列化的項目中可能包含機密資訊 （例如密碼編譯的內容），這會導致所有的簡介[IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor)輸入在第一次。</span><span class="sxs-lookup"><span data-stu-id="9865a-205">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="9865a-206">不過，事故發生，而且索引鍵的環形才能刪除或損毀。</span><span class="sxs-lookup"><span data-stu-id="9865a-206">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="9865a-207">委付介面提供緊急逸出之轉換的任何設定前，允許存取原始的序列化 XML 規劃[IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor)。</span><span class="sxs-lookup"><span data-stu-id="9865a-207">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it is transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="9865a-208">介面會公開單一 API:</span><span class="sxs-lookup"><span data-stu-id="9865a-208">The interface exposes a single API:</span></span>

* <span data-ttu-id="9865a-209">存放區 （Guid keyId、 XElement 項目）</span><span class="sxs-lookup"><span data-stu-id="9865a-209">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="9865a-210">它會`IKeyEscrowSink`實作以商務原則與一致的安全方式處理所提供項目。</span><span class="sxs-lookup"><span data-stu-id="9865a-210">It is up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="9865a-211">一個可能的實作可用於委付接收加密使用已知的公司 X.509 憑證的 XML 項目，其中憑證的私密金鑰已委付;`CertificateXmlEncryptor`可以協助進行此設定類型。</span><span class="sxs-lookup"><span data-stu-id="9865a-211">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="9865a-212">`IKeyEscrowSink`實作也會負責適當地保存所提供項目。</span><span class="sxs-lookup"><span data-stu-id="9865a-212">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="9865a-213">預設沒有委付機制會啟用，但伺服器系統管理員可以[全域設定此](xref:security/data-protection/configuration/machine-wide-policy)。</span><span class="sxs-lookup"><span data-stu-id="9865a-213">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="9865a-214">它也可以設定以程式設計方式透過`IDataProtectionBuilder.AddKeyEscrowSink`方法，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="9865a-214">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="9865a-215">`AddKeyEscrowSink`方法多載鏡像`IServiceCollection.AddSingleton`和`IServiceCollection.AddInstance`多載，為`IKeyEscrowSink`執行個體要作為 singleton。</span><span class="sxs-lookup"><span data-stu-id="9865a-215">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="9865a-216">若為多個`IKeyEscrowSink`註冊執行個體，將金鑰在產生期間，呼叫每個，因此可以委付金鑰至多個機制同時。</span><span class="sxs-lookup"><span data-stu-id="9865a-216">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="9865a-217">沒有任何 API 來讀取資料，從`IKeyEscrowSink`執行個體。</span><span class="sxs-lookup"><span data-stu-id="9865a-217">There is no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="9865a-218">這是一致的委付機制的設計理論： 它已用來讓受信任的授權單位，都能存取金鑰的內容和應用程式本身不是受信任的授權單位，因此不應該有它自己附帶資料的存取。</span><span class="sxs-lookup"><span data-stu-id="9865a-218">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="9865a-219">下列程式碼範例示範建立和註冊`IKeyEscrowSink`，使得只有 「 CONTOSODomain 系統管理員 」 的成員可以復原他們委付金鑰。</span><span class="sxs-lookup"><span data-stu-id="9865a-219">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="9865a-220">若要執行此範例中，您必須是在已加入網域的 Windows 8 / Windows Server 2012 電腦和網域控制站必須是 Windows Server 2012 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="9865a-220">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
