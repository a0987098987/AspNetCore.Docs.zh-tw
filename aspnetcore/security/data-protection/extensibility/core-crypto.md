---
title: "核心加密擴充性"
author: rick-anderson
description: "說明 IAuthenticatedEncryptor、 IAuthenticatedEncryptorDescriptor、 IAuthenticatedEncryptorDescriptorDeserializer 和最上層的 factory。"
keywords: "ASP.NET Core，IAuthenticatedEncryptor，IAuthenticatedEncryptorDescriptor IAuthenticatedEncryptorDescriptorDeserializer"
ms.author: riande
manager: wpickett
ms.date: 8/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: 69839562c39ab83531085e20dac1bd56e8d13d3f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="core-cryptography-extensibility"></a><span data-ttu-id="5ab8a-104">核心加密擴充性</span><span class="sxs-lookup"><span data-stu-id="5ab8a-104">Core cryptography extensibility</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="5ab8a-105">實作下列介面的型別應該是安全執行緒的多個呼叫端。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="5ab8a-106">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="5ab8a-106">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="5ab8a-107">**IAuthenticatedEncryptor**介面是密碼編譯子系統的基本建置區塊。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-107">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="5ab8a-108">通常是每個索引鍵，一個 IAuthenticatedEncryptor 和 IAuthenticatedEncryptor 執行個體包裝所有密碼編譯金鑰的資料和執行密碼編譯作業所需的演算法資訊。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-108">There is generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="5ab8a-109">如其名所示，此類型為負責提供已驗證的加密和解密服務。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-109">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="5ab8a-110">它會公開下列兩個 Api。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-110">It exposes the following two APIs.</span></span>

* <span data-ttu-id="5ab8a-111">解密 (ArraySegment<byte>加密文字，ArraySegment<byte> additionalAuthenticatedData): byte]</span><span class="sxs-lookup"><span data-stu-id="5ab8a-111">Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

* <span data-ttu-id="5ab8a-112">加密 (ArraySegment<byte>純文字、 ArraySegment<byte> additionalAuthenticatedData): byte]</span><span class="sxs-lookup"><span data-stu-id="5ab8a-112">Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

<span data-ttu-id="5ab8a-113">加密方法會傳回包含 enciphered 純文字與驗證標記的 blob。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-113">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="5ab8a-114">驗證標記必須包含其他的已驗證的資料 (AAD) 時，雖然 AAD 本身不需要從最終的裝載可復原。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-114">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="5ab8a-115">解密方法驗證的驗證標籤，並傳回被破解的裝載。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-115">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="5ab8a-116">（不含 ArgumentNullException 和類似） 的所有失敗應該都 homogenized CryptographicException 至。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-116">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="5ab8a-117">IAuthenticatedEncryptor 執行個體本身實際上不需要包含金鑰資料。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-117">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="5ab8a-118">例如，實作可以委派給 HSM 的所有作業。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-118">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="5ab8a-119">如何建立 IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="5ab8a-119">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5ab8a-120">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5ab8a-120">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5ab8a-121">**IAuthenticatedEncryptorFactory**介面代表知道如何建立類型[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)執行個體。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-121">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="5ab8a-122">它的 API 如下所示。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-122">Its API is as follows.</span></span>

* <span data-ttu-id="5ab8a-123">CreateEncryptorInstance （IKey 索引鍵）： IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="5ab8a-123">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="5ab8a-124">對於任何給定的 IKey 執行個體，其 CreateEncryptorInstance 方法所建立的任何已驗證的加密程式應該被視為相等，如下列程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-124">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5ab8a-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5ab8a-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5ab8a-126">**IAuthenticatedEncryptorDescriptor**介面代表知道如何建立類型[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)執行個體。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-126">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="5ab8a-127">它的 API 如下所示。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-127">Its API is as follows.</span></span>

* <span data-ttu-id="5ab8a-128">CreateEncryptorInstance(): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="5ab8a-128">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="5ab8a-129">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="5ab8a-129">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="5ab8a-130">IAuthenticatedEncryptor，一樣會假設 IAuthenticatedEncryptorDescriptor 的執行個體包裝一個特定的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-130">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="5ab8a-131">這表示，對於任何給定的 IAuthenticatedEncryptorDescriptor 執行個體，其 CreateEncryptorInstance 方法所建立的任何已驗證的加密程式應視為相等，如下列程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-131">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

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

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="5ab8a-132">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x 只)</span><span class="sxs-lookup"><span data-stu-id="5ab8a-132">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5ab8a-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5ab8a-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5ab8a-134">**IAuthenticatedEncryptorDescriptor**介面代表知道如何將本身匯出到 XML 的類型。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-134">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="5ab8a-135">它的 API 如下所示。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-135">Its API is as follows.</span></span>

* <span data-ttu-id="5ab8a-136">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="5ab8a-136">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5ab8a-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5ab8a-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="5ab8a-138">XML 序列化</span><span class="sxs-lookup"><span data-stu-id="5ab8a-138">XML Serialization</span></span>

<span data-ttu-id="5ab8a-139">IAuthenticatedEncryptor 與 IAuthenticatedEncryptorDescriptor 之間的主要差異是描述元知道如何建立加密程式，並提供有效的引數。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-139">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="5ab8a-140">請考慮 IAuthenticatedEncryptor 其實作依賴 SymmetricAlgorithm 和 KeyedHashAlgorithm。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-140">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="5ab8a-141">加密程式的作業是使用這些型別，但是它不一定知道這些型別來自何處，讓它無法真正將寫出適當描述如何重新啟動應用程式時重新建立本身。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-141">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="5ab8a-142">描述元做為較高的層級，在這個基礎之上。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-142">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="5ab8a-143">由於描述項會知道如何建立加密程式執行個體 （例如，它知道如何建立必要的演算法），它可以將該知識以 XML 格式序列化，讓應用程式重設之後，可以重新建立加密程式執行個體。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-143">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="5ab8a-144">描述元可以透過其 ExportToXml 常式進行序列化。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-144">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="5ab8a-145">這個常式會傳回包含兩個屬性 XmlSerializedDescriptorInfo： 描述項和類型表示的 XElement 表示[IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer)它可以是用來重新啟動指定對應的 XElement 此描述元。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-145">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="5ab8a-146">序列化的描述元可能包含機密資訊，例如密碼編譯金鑰內容。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-146">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="5ab8a-147">資料保護系統沒有內建支援來之前它已經保存到儲存體加密的資訊。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-147">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="5ab8a-148">若要利用這個，描述元應該將標記包含機密資訊為屬性名稱"requiresEncryption 」 (xmlns"http://schemas.asp.net/2015/03/dataProtection")，值"true"的項目。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-148">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "http://schemas.asp.net/2015/03/dataProtection"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="5ab8a-149">沒有適用於設定這個屬性的協助程式 API。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-149">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="5ab8a-150">呼叫 XElement.MarkAsRequiresEncryption() 位於命名空間 Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-150">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="5ab8a-151">也可以序列化的描述元其中不包含機密資訊的情況。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-151">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="5ab8a-152">請再次考量 HSM 中所儲存的密碼編譯金鑰的大小寫。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-152">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="5ab8a-153">序列化本身，因為 HSM 不會公開以純文字形式的資料時，無法寫入出金鑰的內容描述元。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-153">The descriptor cannot write out the key material when serializing itself since the HSM will not expose the material in plaintext form.</span></span> <span data-ttu-id="5ab8a-154">相反地，描述元可以撰寫出金鑰包裝的版本 （如果 HSM 會以這種方式允許匯出） 的索引鍵或索引鍵的 HSM 自己唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-154">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="5ab8a-155">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="5ab8a-155">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="5ab8a-156">**IAuthenticatedEncryptorDescriptorDeserializer**介面代表知道如何還原序列化 IAuthenticatedEncryptorDescriptor 從 XElement 執行個體的類型。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-156">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="5ab8a-157">它會公開單一方法：</span><span class="sxs-lookup"><span data-stu-id="5ab8a-157">It exposes a single method:</span></span>

* <span data-ttu-id="5ab8a-158">ImportFromXml （XElement 項目）： IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="5ab8a-158">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="5ab8a-159">ImportFromXml 方法會傳回 XElement [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml)並建立原始 IAuthenticatedEncryptorDescriptor 的對等項目。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-159">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="5ab8a-160">實作 IAuthenticatedEncryptorDescriptorDeserializer 類型應該具有下列兩個公用建構函式的其中一個：</span><span class="sxs-lookup"><span data-stu-id="5ab8a-160">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="5ab8a-161">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="5ab8a-161">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="5ab8a-162">.ctor</span><span class="sxs-lookup"><span data-stu-id="5ab8a-162">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="5ab8a-163">傳遞至建構函式的 IServiceProvider 可能是 null。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-163">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="5ab8a-164">最上層的處理站</span><span class="sxs-lookup"><span data-stu-id="5ab8a-164">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5ab8a-165">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5ab8a-165">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5ab8a-166">**AlgorithmConfiguration**類別代表的類型而知道如何建立[IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor)執行個體。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-166">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="5ab8a-167">它會公開單一 API。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-167">It exposes a single API.</span></span>

* <span data-ttu-id="5ab8a-168">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="5ab8a-168">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="5ab8a-169">AlgorithmConfiguration 視為最上層的 factory。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-169">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="5ab8a-170">設定做為範本。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-170">The configuration serves as a template.</span></span> <span data-ttu-id="5ab8a-171">它會包裝的演算法資訊 （例如，此組態產生描述元的 AES-128-GCM 主要金鑰），但是它尚未與特定的索引鍵相關聯。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-171">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it is not yet associated with a specific key.</span></span>

<span data-ttu-id="5ab8a-172">當 CreateNewDescriptor 稱為，全新金錀會建立僅供此呼叫，而且會產生新的 IAuthenticatedEncryptorDescriptor 包裝此金鑰的內容和取用所需的資料所需的演算法資訊。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-172">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="5ab8a-173">金鑰內容可能會在軟體中建立 （和保留在記憶體中），所以無法建立且保留在 HSM，依此類推。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-173">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="5ab8a-174">重要的重點是 CreateNewDescriptor 任何兩個呼叫應該永遠不會建立對等 IAuthenticatedEncryptorDescriptor 執行個體。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-174">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="5ab8a-175">AlgorithmConfiguration 類型做為索引鍵建立常式的進入點例如[自動金鑰輪換](../implementation/key-management.md#key-expiration-and-rolling)。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-175">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](../implementation/key-management.md#key-expiration-and-rolling).</span></span> <span data-ttu-id="5ab8a-176">若要變更的所有未來的索引鍵的實作，請在 KeyManagementOptions 中設定 AuthenticatedEncryptorConfiguration 屬性。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-176">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5ab8a-177">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5ab8a-177">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5ab8a-178">**IAuthenticatedEncryptorConfiguration**介面代表的類型而知道如何建立[IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor)執行個體。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-178">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="5ab8a-179">它會公開單一 API。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-179">It exposes a single API.</span></span>

* <span data-ttu-id="5ab8a-180">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="5ab8a-180">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="5ab8a-181">IAuthenticatedEncryptorConfiguration 視為最上層的 factory。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-181">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="5ab8a-182">設定做為範本。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-182">The configuration serves as a template.</span></span> <span data-ttu-id="5ab8a-183">它會包裝的演算法資訊 （例如，此組態產生描述元的 AES-128-GCM 主要金鑰），但是它尚未與特定的索引鍵相關聯。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-183">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it is not yet associated with a specific key.</span></span>

<span data-ttu-id="5ab8a-184">當 CreateNewDescriptor 稱為，全新金錀會建立僅供此呼叫，而且會產生新的 IAuthenticatedEncryptorDescriptor 包裝此金鑰的內容和取用所需的資料所需的演算法資訊。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-184">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="5ab8a-185">金鑰內容可能會在軟體中建立 （和保留在記憶體中），所以無法建立且保留在 HSM，依此類推。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-185">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="5ab8a-186">重要的重點是 CreateNewDescriptor 任何兩個呼叫應該永遠不會建立對等 IAuthenticatedEncryptorDescriptor 執行個體。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-186">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="5ab8a-187">IAuthenticatedEncryptorConfiguration 類型做為索引鍵建立常式的進入點例如[自動金鑰輪換](../implementation/key-management.md#key-expiration-and-rolling)。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-187">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](../implementation/key-management.md#key-expiration-and-rolling).</span></span> <span data-ttu-id="5ab8a-188">若要變更的所有未來的索引鍵的實作，請在服務容器中註冊單一 IAuthenticatedEncryptorConfiguration。</span><span class="sxs-lookup"><span data-stu-id="5ab8a-188">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
