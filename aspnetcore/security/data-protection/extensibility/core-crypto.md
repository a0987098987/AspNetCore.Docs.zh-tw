---
title: ASP.NET Core 中的核心加密擴充性
author: rick-anderson
description: 深入了解 IAuthenticatedEncryptor、 IAuthenticatedEncryptorDescriptor、 IAuthenticatedEncryptorDescriptorDeserializer 和最上層的 factory。
ms.author: riande
ms.date: 8/11/2017
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: cf4a142992efe5b00a75285ef9ad9735fe7be411
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209066"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a><span data-ttu-id="40d6f-103">ASP.NET Core 中的核心加密擴充性</span><span class="sxs-lookup"><span data-stu-id="40d6f-103">Core cryptography extensibility in ASP.NET Core</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="40d6f-104">會實作下列介面的任何的類型應該是安全執行緒的多個呼叫端。</span><span class="sxs-lookup"><span data-stu-id="40d6f-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="40d6f-105">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="40d6f-105">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="40d6f-106">**IAuthenticatedEncryptor**介面是密碼編譯子系統基本建置組塊。</span><span class="sxs-lookup"><span data-stu-id="40d6f-106">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="40d6f-107">通常是每個索引鍵，一個 IAuthenticatedEncryptor 和 IAuthenticatedEncryptor 執行個體包裝所有密碼編譯金鑰資料和執行密碼編譯作業所需的演算法資訊。</span><span class="sxs-lookup"><span data-stu-id="40d6f-107">There's generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="40d6f-108">正如其名，此類型為負責提供已驗證的加密和解密服務。</span><span class="sxs-lookup"><span data-stu-id="40d6f-108">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="40d6f-109">它會公開下列兩個 Api。</span><span class="sxs-lookup"><span data-stu-id="40d6f-109">It exposes the following two APIs.</span></span>

* `Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

* `Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

<span data-ttu-id="40d6f-110">加密方法會傳回包含已譯成密碼的純文字，以及驗證標籤的 blob。</span><span class="sxs-lookup"><span data-stu-id="40d6f-110">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="40d6f-111">驗證標記必須包含額外的驗證的資料 (AAD)，但不是可復原的最後一個裝載從 AAD 自身。</span><span class="sxs-lookup"><span data-stu-id="40d6f-111">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="40d6f-112">解密方法驗證的驗證標籤，並傳回被破解的承載。</span><span class="sxs-lookup"><span data-stu-id="40d6f-112">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="40d6f-113">所有失敗 （不含 ArgumentNullException 和類似） 應該都 homogenized CryptographicException 到。</span><span class="sxs-lookup"><span data-stu-id="40d6f-113">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="40d6f-114">IAuthenticatedEncryptor 執行個體本身實際上並不需要包含金鑰資料。</span><span class="sxs-lookup"><span data-stu-id="40d6f-114">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="40d6f-115">比方說，實作可以委派給 HSM 的所有作業。</span><span class="sxs-lookup"><span data-stu-id="40d6f-115">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="40d6f-116">如何建立 IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="40d6f-116">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40d6f-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40d6f-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="40d6f-118">**IAuthenticatedEncryptorFactory**介面代表一種類型，知道如何建立[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)執行個體。</span><span class="sxs-lookup"><span data-stu-id="40d6f-118">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="40d6f-119">它的 API 如下所示。</span><span class="sxs-lookup"><span data-stu-id="40d6f-119">Its API is as follows.</span></span>

* <span data-ttu-id="40d6f-120">CreateEncryptorInstance （IKey 索引鍵）：IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="40d6f-120">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="40d6f-121">對於任何給定的 IKey 執行個體，其 CreateEncryptorInstance 方法所建立的任何已驗證的加密程式應視為相等，如下列程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="40d6f-121">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorFactory instance and an IKey instance
IAuthenticatedEncryptorFactory factory = ...;
IKey key = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = factory.CreateEncryptorInstance(key);
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = factory.CreateEncryptorInstance(key);
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40d6f-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40d6f-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="40d6f-123">**IAuthenticatedEncryptorDescriptor**介面代表一種類型，知道如何建立[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)執行個體。</span><span class="sxs-lookup"><span data-stu-id="40d6f-123">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="40d6f-124">它的 API 如下所示。</span><span class="sxs-lookup"><span data-stu-id="40d6f-124">Its API is as follows.</span></span>

* <span data-ttu-id="40d6f-125">CreateEncryptorInstance():IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="40d6f-125">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="40d6f-126">ExportToXml():XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="40d6f-126">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="40d6f-127">IAuthenticatedEncryptor，例如假設 IAuthenticatedEncryptorDescriptor 的執行個體包裝一個特定的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="40d6f-127">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="40d6f-128">這表示，對於任何給定的 IAuthenticatedEncryptorDescriptor 執行個體，其 CreateEncryptorInstance 方法所建立的任何已驗證的加密程式應視為相等，如下列程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="40d6f-128">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorDescriptor instance
IAuthenticatedEncryptorDescriptor descriptor = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = descriptor.CreateEncryptorInstance();
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = descriptor.CreateEncryptorInstance();
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

---

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="40d6f-129">IAuthenticatedEncryptorDescriptor (ASP.NET Core 僅限 2.x)</span><span class="sxs-lookup"><span data-stu-id="40d6f-129">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40d6f-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40d6f-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="40d6f-131">**IAuthenticatedEncryptorDescriptor**介面代表知道如何將本身匯出到 XML 的型別。</span><span class="sxs-lookup"><span data-stu-id="40d6f-131">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="40d6f-132">它的 API 如下所示。</span><span class="sxs-lookup"><span data-stu-id="40d6f-132">Its API is as follows.</span></span>

* <span data-ttu-id="40d6f-133">ExportToXml():XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="40d6f-133">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40d6f-134">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40d6f-134">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="40d6f-135">XML 序列化</span><span class="sxs-lookup"><span data-stu-id="40d6f-135">XML Serialization</span></span>

<span data-ttu-id="40d6f-136">IAuthenticatedEncryptor 和 IAuthenticatedEncryptorDescriptor 的主要差異在於描述元知道如何建立加密程式，並提供有效的引數。</span><span class="sxs-lookup"><span data-stu-id="40d6f-136">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="40d6f-137">請考慮其實作依賴 SymmetricAlgorithm 和 KeyedHashAlgorithm IAuthenticatedEncryptor。</span><span class="sxs-lookup"><span data-stu-id="40d6f-137">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="40d6f-138">加密程式的工作就是要使用這些類型，但它並不一定知道這些型別來自何處，因此它無法真正會寫出適當描述如何重新建立本身的應用程式重新啟動時。</span><span class="sxs-lookup"><span data-stu-id="40d6f-138">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="40d6f-139">描述元做為較高的層級，在這個基礎之上。</span><span class="sxs-lookup"><span data-stu-id="40d6f-139">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="40d6f-140">由於描述項會知道如何建立加密程式執行個體 （例如，它知道如何建立必要的演算法），以便可以重新建立加密程式執行個體，應用程式重設之後，它可以序列化 XML 表單中的知識。</span><span class="sxs-lookup"><span data-stu-id="40d6f-140">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="40d6f-141">描述元可以透過其 ExportToXml 常式進行序列化。</span><span class="sxs-lookup"><span data-stu-id="40d6f-141">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="40d6f-142">此常式會傳回包含兩個屬性 XmlSerializedDescriptorInfo: 描述項和其代表的型別 XElement 表示法[IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer)可以是用來重新啟動指定對應的 XElement 這個描述元。</span><span class="sxs-lookup"><span data-stu-id="40d6f-142">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="40d6f-143">序列化的描述項可能包含機密資訊，例如密碼編譯金鑰的內容。</span><span class="sxs-lookup"><span data-stu-id="40d6f-143">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="40d6f-144">資料保護系統的內建支援已保存至儲存體之前加密資訊。</span><span class="sxs-lookup"><span data-stu-id="40d6f-144">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="40d6f-145">若要利用這個，描述元應該標記包含機密資訊與屬性名稱"requiresEncryption"元素 (xmlns"<http://schemas.asp.net/2015/03/dataProtection>」)，值"true"。</span><span class="sxs-lookup"><span data-stu-id="40d6f-145">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="40d6f-146">沒有設定這個屬性的協助程式 API。</span><span class="sxs-lookup"><span data-stu-id="40d6f-146">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="40d6f-147">呼叫 XElement.MarkAsRequiresEncryption() 位於命名空間 Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel; 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="40d6f-147">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="40d6f-148">也可以序列化的描述元其中不包含機密資訊的情況。</span><span class="sxs-lookup"><span data-stu-id="40d6f-148">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="40d6f-149">請再次考量 HSM 中所儲存的密碼編譯金鑰的大小寫。</span><span class="sxs-lookup"><span data-stu-id="40d6f-149">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="40d6f-150">序列化本身，因為 HSM 不會公開純文字形式的資料時，無法寫入出金鑰的內容描述元。</span><span class="sxs-lookup"><span data-stu-id="40d6f-150">The descriptor cannot write out the key material when serializing itself since the HSM won't expose the material in plaintext form.</span></span> <span data-ttu-id="40d6f-151">相反地，描述項可能撰寫出的金鑰包裝版本 （如果 HSM 會允許匯出，以這種方式） 的索引鍵或索引鍵的 HSM 自己的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="40d6f-151">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="40d6f-152">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="40d6f-152">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="40d6f-153">**IAuthenticatedEncryptorDescriptorDeserializer**介面代表知道如何還原序列化從 XElement IAuthenticatedEncryptorDescriptor 執行個體的型別。</span><span class="sxs-lookup"><span data-stu-id="40d6f-153">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="40d6f-154">它會公開單一方法：</span><span class="sxs-lookup"><span data-stu-id="40d6f-154">It exposes a single method:</span></span>

* <span data-ttu-id="40d6f-155">ImportFromXml （XElement 項目）：IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="40d6f-155">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="40d6f-156">ImportFromXml 方法會傳回的 XElement [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml)並建立原始 IAuthenticatedEncryptorDescriptor 的對等項目。</span><span class="sxs-lookup"><span data-stu-id="40d6f-156">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="40d6f-157">實作 IAuthenticatedEncryptorDescriptorDeserializer 類型應該有下列兩個公用建構函式的其中一個：</span><span class="sxs-lookup"><span data-stu-id="40d6f-157">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="40d6f-158">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="40d6f-158">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="40d6f-159">.ctor()</span><span class="sxs-lookup"><span data-stu-id="40d6f-159">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="40d6f-160">傳遞至建構函式的 IServiceProvider 可能是 null。</span><span class="sxs-lookup"><span data-stu-id="40d6f-160">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="40d6f-161">最上層的處理站</span><span class="sxs-lookup"><span data-stu-id="40d6f-161">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40d6f-162">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40d6f-162">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="40d6f-163">**AlgorithmConfiguration**類別代表知道如何建立的型別[IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor)執行個體。</span><span class="sxs-lookup"><span data-stu-id="40d6f-163">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="40d6f-164">它會公開單一的 API。</span><span class="sxs-lookup"><span data-stu-id="40d6f-164">It exposes a single API.</span></span>

* <span data-ttu-id="40d6f-165">CreateNewDescriptor():IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="40d6f-165">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="40d6f-166">您可以將 AlgorithmConfiguration 想像成最上層的 factory。</span><span class="sxs-lookup"><span data-stu-id="40d6f-166">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="40d6f-167">設定做為範本。</span><span class="sxs-lookup"><span data-stu-id="40d6f-167">The configuration serves as a template.</span></span> <span data-ttu-id="40d6f-168">它會包裝演算法的資訊 （例如，此組態會產生使用 AES-128-GCM 主要金鑰的描述項），但它尚未與特定索引鍵相關聯。</span><span class="sxs-lookup"><span data-stu-id="40d6f-168">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="40d6f-169">當 CreateNewDescriptor 稱為，全新的金鑰內容會建立僅供此呼叫，而且會產生新的 IAuthenticatedEncryptorDescriptor 包裝此金鑰的資料和取用資料所需的演算法資訊。</span><span class="sxs-lookup"><span data-stu-id="40d6f-169">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="40d6f-170">金鑰的內容可能會在軟體中建立 （和保留在記憶體中），所以無法建立且內含 HSM，依此類推。</span><span class="sxs-lookup"><span data-stu-id="40d6f-170">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="40d6f-171">重點是，CreateNewDescriptor 任何兩個呼叫應該永遠不會建立對等的 IAuthenticatedEncryptorDescriptor 執行個體。</span><span class="sxs-lookup"><span data-stu-id="40d6f-171">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="40d6f-172">AlgorithmConfiguration 類型做為索引鍵建立常式的進入點這類[自動金鑰輪換](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)。</span><span class="sxs-lookup"><span data-stu-id="40d6f-172">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="40d6f-173">若要變更的所有未來的索引鍵的實作，請在 KeyManagementOptions 中設定 AuthenticatedEncryptorConfiguration 屬性。</span><span class="sxs-lookup"><span data-stu-id="40d6f-173">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40d6f-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40d6f-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="40d6f-175">**IAuthenticatedEncryptorConfiguration**介面代表知道如何建立的型別[IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor)執行個體。</span><span class="sxs-lookup"><span data-stu-id="40d6f-175">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="40d6f-176">它會公開單一的 API。</span><span class="sxs-lookup"><span data-stu-id="40d6f-176">It exposes a single API.</span></span>

* <span data-ttu-id="40d6f-177">CreateNewDescriptor():IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="40d6f-177">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="40d6f-178">您可以將 IAuthenticatedEncryptorConfiguration 想像成最上層的 factory。</span><span class="sxs-lookup"><span data-stu-id="40d6f-178">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="40d6f-179">設定做為範本。</span><span class="sxs-lookup"><span data-stu-id="40d6f-179">The configuration serves as a template.</span></span> <span data-ttu-id="40d6f-180">它會包裝演算法的資訊 （例如，此組態會產生使用 AES-128-GCM 主要金鑰的描述項），但它尚未與特定索引鍵相關聯。</span><span class="sxs-lookup"><span data-stu-id="40d6f-180">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="40d6f-181">當 CreateNewDescriptor 稱為，全新的金鑰內容會建立僅供此呼叫，而且會產生新的 IAuthenticatedEncryptorDescriptor 包裝此金鑰的資料和取用資料所需的演算法資訊。</span><span class="sxs-lookup"><span data-stu-id="40d6f-181">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="40d6f-182">金鑰的內容可能會在軟體中建立 （和保留在記憶體中），所以無法建立且內含 HSM，依此類推。</span><span class="sxs-lookup"><span data-stu-id="40d6f-182">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="40d6f-183">重點是，CreateNewDescriptor 任何兩個呼叫應該永遠不會建立對等的 IAuthenticatedEncryptorDescriptor 執行個體。</span><span class="sxs-lookup"><span data-stu-id="40d6f-183">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="40d6f-184">IAuthenticatedEncryptorConfiguration 類型做為索引鍵建立常式的進入點這類[自動金鑰輪換](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)。</span><span class="sxs-lookup"><span data-stu-id="40d6f-184">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="40d6f-185">若要變更的所有未來的索引鍵的實作，請在服務容器註冊單一 IAuthenticatedEncryptorConfiguration。</span><span class="sxs-lookup"><span data-stu-id="40d6f-185">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
