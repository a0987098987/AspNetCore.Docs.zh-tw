---
title: 在 ASP.NET Core 核心加密擴充性
author: rick-anderson
description: 深入了解 IAuthenticatedEncryptor、 IAuthenticatedEncryptorDescriptor、 IAuthenticatedEncryptorDescriptorDeserializer 和最上層的 factory。
manager: wpickett
ms.author: riande
ms.date: 8/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: b5a0dbc9120a8032dbb8d8eee74684495a982ac1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896822"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a>在 ASP.NET Core 核心加密擴充性

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> 實作下列介面的型別應該是安全執行緒的多個呼叫端。

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a>IAuthenticatedEncryptor

**IAuthenticatedEncryptor**介面是密碼編譯子系統的基本建置區塊。 通常是每個索引鍵，一個 IAuthenticatedEncryptor 和 IAuthenticatedEncryptor 執行個體包裝所有密碼編譯金鑰的資料和執行密碼編譯作業所需的演算法資訊。

如其名所示，此類型為負責提供已驗證的加密和解密服務。 它會公開下列兩個 Api。

* 解密 (ArraySegment<byte>加密文字，ArraySegment<byte> additionalAuthenticatedData): byte]

* 加密 (ArraySegment<byte>純文字、 ArraySegment<byte> additionalAuthenticatedData): byte]

加密方法會傳回包含 enciphered 純文字與驗證標記的 blob。 驗證標記必須包含其他的已驗證的資料 (AAD) 時，雖然 AAD 本身不需要從最終的裝載可復原。 解密方法驗證的驗證標籤，並傳回被破解的裝載。 （不含 ArgumentNullException 和類似） 的所有失敗應該都 homogenized CryptographicException 至。

> [!NOTE]
> IAuthenticatedEncryptor 執行個體本身實際上不需要包含金鑰資料。 例如，實作可以委派給 HSM 的所有作業。

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a>如何建立 IAuthenticatedEncryptor

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorFactory**介面代表知道如何建立類型[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)執行個體。 它的 API 如下所示。

* CreateEncryptorInstance （IKey 索引鍵）： IAuthenticatedEncryptor

對於任何給定的 IKey 執行個體，其 CreateEncryptorInstance 方法所建立的任何已驗證的加密程式應該被視為相等，如下列程式碼範例。

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorDescriptor**介面代表知道如何建立類型[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)執行個體。 它的 API 如下所示。

* CreateEncryptorInstance(): IAuthenticatedEncryptor

* ExportToXml() : XmlSerializedDescriptorInfo

IAuthenticatedEncryptor，一樣會假設 IAuthenticatedEncryptorDescriptor 的執行個體包裝一個特定的索引鍵。 這表示，對於任何給定的 IAuthenticatedEncryptorDescriptor 執行個體，其 CreateEncryptorInstance 方法所建立的任何已驗證的加密程式應視為相等，如下列程式碼範例。

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

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a>IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x 只)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorDescriptor**介面代表知道如何將本身匯出到 XML 的類型。 它的 API 如下所示。

* ExportToXml() : XmlSerializedDescriptorInfo

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a>XML 序列化

IAuthenticatedEncryptor 與 IAuthenticatedEncryptorDescriptor 之間的主要差異是描述元知道如何建立加密程式，並提供有效的引數。 請考慮 IAuthenticatedEncryptor 其實作依賴 SymmetricAlgorithm 和 KeyedHashAlgorithm。 加密程式的作業是使用這些型別，但是它不一定知道這些型別來自何處，讓它無法真正將寫出適當描述如何重新啟動應用程式時重新建立本身。 描述元做為較高的層級，在這個基礎之上。 由於描述項會知道如何建立加密程式執行個體 （例如，它知道如何建立必要的演算法），它可以將該知識以 XML 格式序列化，讓應用程式重設之後，可以重新建立加密程式執行個體。

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

描述元可以透過其 ExportToXml 常式進行序列化。 這個常式會傳回包含兩個屬性 XmlSerializedDescriptorInfo： 描述項和類型表示的 XElement 表示[IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer)它可以是用來重新啟動指定對應的 XElement 此描述元。

序列化的描述元可能包含機密資訊，例如密碼編譯金鑰內容。 資料保護系統沒有內建支援來之前它已經保存到儲存體加密的資訊。 若要利用這個，描述元應該將標記包含機密資訊的屬性名稱"requiresEncryption"的元素 (xmlns"<http://schemas.asp.net/2015/03/dataProtection>")，值"true"。

>[!TIP]
> 沒有適用於設定這個屬性的協助程式 API。 呼叫 XElement.MarkAsRequiresEncryption() 位於命名空間 Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel 擴充方法。

也可以序列化的描述元其中不包含機密資訊的情況。 請再次考量 HSM 中所儲存的密碼編譯金鑰的大小寫。 序列化本身，因為 HSM 不會公開以純文字形式的資料時，無法寫入出金鑰的內容描述元。 相反地，描述元可以撰寫出金鑰包裝的版本 （如果 HSM 會以這種方式允許匯出） 的索引鍵或索引鍵的 HSM 自己唯一識別碼。

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a>IAuthenticatedEncryptorDescriptorDeserializer

**IAuthenticatedEncryptorDescriptorDeserializer**介面代表知道如何還原序列化 IAuthenticatedEncryptorDescriptor 從 XElement 執行個體的類型。 它會公開單一方法：

* ImportFromXml （XElement 項目）： IAuthenticatedEncryptorDescriptor

ImportFromXml 方法會傳回 XElement [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml)並建立原始 IAuthenticatedEncryptorDescriptor 的對等項目。

實作 IAuthenticatedEncryptorDescriptorDeserializer 類型應該具有下列兩個公用建構函式的其中一個：

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> 傳遞至建構函式的 IServiceProvider 可能是 null。

## <a name="the-top-level-factory"></a>最上層的處理站

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**AlgorithmConfiguration**類別代表的類型而知道如何建立[IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor)執行個體。 它會公開單一 API。

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

AlgorithmConfiguration 視為最上層的 factory。 設定做為範本。 它會包裝的演算法資訊 （例如，此組態產生描述元的 AES-128-GCM 主要金鑰），但是它尚未與特定的索引鍵相關聯。

當 CreateNewDescriptor 稱為，全新金錀會建立僅供此呼叫，而且會產生新的 IAuthenticatedEncryptorDescriptor 包裝此金鑰的內容和取用所需的資料所需的演算法資訊。 金鑰內容可能會在軟體中建立 （和保留在記憶體中），所以無法建立且保留在 HSM，依此類推。 重要的重點是 CreateNewDescriptor 任何兩個呼叫應該永遠不會建立對等 IAuthenticatedEncryptorDescriptor 執行個體。

AlgorithmConfiguration 類型做為索引鍵建立常式的進入點例如[自動金鑰輪換](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)。 若要變更的所有未來的索引鍵的實作，請在 KeyManagementOptions 中設定 AuthenticatedEncryptorConfiguration 屬性。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorConfiguration**介面代表的類型而知道如何建立[IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor)執行個體。 它會公開單一 API。

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

IAuthenticatedEncryptorConfiguration 視為最上層的 factory。 設定做為範本。 它會包裝的演算法資訊 （例如，此組態產生描述元的 AES-128-GCM 主要金鑰），但是它尚未與特定的索引鍵相關聯。

當 CreateNewDescriptor 稱為，全新金錀會建立僅供此呼叫，而且會產生新的 IAuthenticatedEncryptorDescriptor 包裝此金鑰的內容和取用所需的資料所需的演算法資訊。 金鑰內容可能會在軟體中建立 （和保留在記憶體中），所以無法建立且保留在 HSM，依此類推。 重要的重點是 CreateNewDescriptor 任何兩個呼叫應該永遠不會建立對等 IAuthenticatedEncryptorDescriptor 執行個體。

IAuthenticatedEncryptorConfiguration 類型做為索引鍵建立常式的進入點例如[自動金鑰輪換](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)。 若要變更的所有未來的索引鍵的實作，請在服務容器中註冊單一 IAuthenticatedEncryptorConfiguration。

---
