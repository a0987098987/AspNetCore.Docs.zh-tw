---
title: 在 ASP.NET Core 金鑰的儲存體格式
author: rick-anderson
description: 了解 ASP.NET Core 資料保護金鑰的儲存格式的實作詳細資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: bb2bcdff3ac2b17623a67f51fd27b29bb928a2fb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274514"
---
# <a name="key-storage-format-in-aspnet-core"></a><span data-ttu-id="af8fd-103">在 ASP.NET Core 金鑰的儲存體格式</span><span class="sxs-lookup"><span data-stu-id="af8fd-103">Key storage format in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="af8fd-104">物件會儲存在 XML 表示法中的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="af8fd-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="af8fd-105">金鑰儲存的預設目錄是 %localappdata%\asp.net\dataprotection-keys\。</span><span class="sxs-lookup"><span data-stu-id="af8fd-105">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="af8fd-106">\<金鑰 > 項目</span><span class="sxs-lookup"><span data-stu-id="af8fd-106">The \<key> element</span></span>

<span data-ttu-id="af8fd-107">索引鍵做為索引鍵的儲存機制中的最上層物件。</span><span class="sxs-lookup"><span data-stu-id="af8fd-107">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="af8fd-108">依慣例有檔名**金鑰-{guid}.xml**，其中 {guid} 是金鑰的識別碼。</span><span class="sxs-lookup"><span data-stu-id="af8fd-108">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="af8fd-109">每個這類檔案包含單一索引鍵。</span><span class="sxs-lookup"><span data-stu-id="af8fd-109">Each such file contains a single key.</span></span> <span data-ttu-id="af8fd-110">檔案的格式如下所示。</span><span class="sxs-lookup"><span data-stu-id="af8fd-110">The format of the file is as follows.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

<span data-ttu-id="af8fd-111">\<金鑰 > 元素包含下列屬性和子項目：</span><span class="sxs-lookup"><span data-stu-id="af8fd-111">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="af8fd-112">索引鍵的識別碼。這個值會被視為可靠;檔名不只是人力可讀性 nicety。</span><span class="sxs-lookup"><span data-stu-id="af8fd-112">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="af8fd-113">版本\<金鑰 > 項目，目前固定為 1。</span><span class="sxs-lookup"><span data-stu-id="af8fd-113">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="af8fd-114">金鑰的建立、 啟用和到期日期。</span><span class="sxs-lookup"><span data-stu-id="af8fd-114">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="af8fd-115">A\<描述元 > 元素，其包含已驗證的加密實作包含在此機碼的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="af8fd-115">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="af8fd-116">在上述範例中，索引鍵的識別碼是 {80732141-ec8f-4b80-af9c-c4d2d1ff8901}、 用來建立並啟動於 2015 年 3 月 19 日，以及具有的 90 天的存留期。</span><span class="sxs-lookup"><span data-stu-id="af8fd-116">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="af8fd-117">（有時啟用日期可能會稍微如此範例所示的建立日期之前。</span><span class="sxs-lookup"><span data-stu-id="af8fd-117">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="af8fd-118">這是因為 n Api 的運作方式，並是無害的在實務中。）</span><span class="sxs-lookup"><span data-stu-id="af8fd-118">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="af8fd-119">\<描述元 > 項目</span><span class="sxs-lookup"><span data-stu-id="af8fd-119">The \<descriptor> element</span></span>

<span data-ttu-id="af8fd-120">外部\<描述元 > 項目包含屬性 deserializerType，這是實作 IAuthenticatedEncryptorDescriptorDeserializer 型別的組件限定名稱。</span><span class="sxs-lookup"><span data-stu-id="af8fd-120">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="af8fd-121">此類型是負責讀取內部\<描述元 > 項目和剖析內所包含的資訊。</span><span class="sxs-lookup"><span data-stu-id="af8fd-121">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="af8fd-122">特定的格式\<描述元 > 項目取決於已驗證的加密程式實作由索引鍵，封裝和這個預期稍有不同格式的每個還原序列化程式型別。</span><span class="sxs-lookup"><span data-stu-id="af8fd-122">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="af8fd-123">一般情況下，不過，這個項目會包含演算法的資訊 (名稱、 型別，Oid，或類似) 和秘密金鑰內容。</span><span class="sxs-lookup"><span data-stu-id="af8fd-123">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="af8fd-124">在上述範例中，描述元會指定此金鑰包裝 AES-CBC-256 加密 + HMACSHA256 驗證。</span><span class="sxs-lookup"><span data-stu-id="af8fd-124">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="af8fd-125">\<EncryptedSecret > 項目</span><span class="sxs-lookup"><span data-stu-id="af8fd-125">The \<encryptedSecret> element</span></span>

<span data-ttu-id="af8fd-126"><encryptedSecret>包含加密的秘密金鑰材料表單項目可能會存在於如果[啟用加密的密碼在靜止](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)。</span><span class="sxs-lookup"><span data-stu-id="af8fd-126">An <encryptedSecret> element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest).</span></span> <span data-ttu-id="af8fd-127">屬性 decryptorType 會實作 IXmlDecryptor 類型的組件限定名稱。</span><span class="sxs-lookup"><span data-stu-id="af8fd-127">The attribute decryptorType will be the assembly-qualified name of a type which implements IXmlDecryptor.</span></span> <span data-ttu-id="af8fd-128">此類型是負責讀取內部<encryptedKey>項目，來復原原始的純文字解密。</span><span class="sxs-lookup"><span data-stu-id="af8fd-128">This type is responsible for reading the inner <encryptedKey> element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="af8fd-129">如同\<描述元 >，特定的格式<encryptedSecret>項目相依於使用中的靜止加密機制。</span><span class="sxs-lookup"><span data-stu-id="af8fd-129">As with \<descriptor>, the particular format of the <encryptedSecret> element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="af8fd-130">在上述範例中，為每個註解中使用 Windows DPAPI 加密主要金鑰。</span><span class="sxs-lookup"><span data-stu-id="af8fd-130">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="af8fd-131">\<撤銷 > 項目</span><span class="sxs-lookup"><span data-stu-id="af8fd-131">The \<revocation> element</span></span>

<span data-ttu-id="af8fd-132">撤銷做為索引鍵的儲存機制中的最上層物件。</span><span class="sxs-lookup"><span data-stu-id="af8fd-132">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="af8fd-133">依照慣例撤銷有檔名**撤銷-{timestamp}.xml** （適用於特定日期之前撤銷所有索引鍵） 或**撤銷-{guid}.xml** （適用於撤銷特定的索引鍵）。</span><span class="sxs-lookup"><span data-stu-id="af8fd-133">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="af8fd-134">每個檔案包含單一\<撤銷 > 項目。</span><span class="sxs-lookup"><span data-stu-id="af8fd-134">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="af8fd-135">針對撤銷的個別索引鍵，會將檔案內容如下。</span><span class="sxs-lookup"><span data-stu-id="af8fd-135">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="af8fd-136">在此情況下，指定索引鍵已被撤銷。</span><span class="sxs-lookup"><span data-stu-id="af8fd-136">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="af8fd-137">金鑰識別碼是否"\*"，不過，在下列範例中，會撤銷其建立日期會指定的撤銷日期之前的所有索引鍵。</span><span class="sxs-lookup"><span data-stu-id="af8fd-137">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="af8fd-138">\<原因 > 系統永遠不會讀取項目。</span><span class="sxs-lookup"><span data-stu-id="af8fd-138">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="af8fd-139">它是只是方便的位置來儲存人類看得懂的撤銷原因。</span><span class="sxs-lookup"><span data-stu-id="af8fd-139">It's simply a convenient place to store a human-readable reason for revocation.</span></span>
