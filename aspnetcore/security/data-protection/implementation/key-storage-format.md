---
title: ASP.NET Core 中的金鑰儲存體格式
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護金鑰儲存格式的執行詳細資料。
ms.author: riande
ms.date: 04/08/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 032b3f9ccea2ae361a8f2fd12538ffb901310247
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408899"
---
# <a name="key-storage-format-in-aspnet-core"></a><span data-ttu-id="c9774-103">ASP.NET Core 中的金鑰儲存體格式</span><span class="sxs-lookup"><span data-stu-id="c9774-103">Key storage format in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="c9774-104">物件會以靜態方式儲存在 XML 標記法中。</span><span class="sxs-lookup"><span data-stu-id="c9774-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="c9774-105">金鑰儲存區的預設目錄是：</span><span class="sxs-lookup"><span data-stu-id="c9774-105">The default directory for key storage is:</span></span>

* <span data-ttu-id="c9774-106">Windows： \*%LOCALAPPDATA%\ASP.NET\DataProtection-Keys\*</span><span class="sxs-lookup"><span data-stu-id="c9774-106">Windows: \*%LOCALAPPDATA%\ASP.NET\DataProtection-Keys\*</span></span>
* <span data-ttu-id="c9774-107">macOS/Linux： *$HOME/.aspnet/dataprotection-keys*</span><span class="sxs-lookup"><span data-stu-id="c9774-107">macOS / Linux: *$HOME/.aspnet/DataProtection-Keys*</span></span>

## <a name="the-key-element"></a><span data-ttu-id="c9774-108">\<key> 項目</span><span class="sxs-lookup"><span data-stu-id="c9774-108">The \<key> element</span></span>

<span data-ttu-id="c9774-109">金鑰是以最上層物件的形式存在於金鑰存放庫中。</span><span class="sxs-lookup"><span data-stu-id="c9774-109">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="c9774-110">依照慣例，索引鍵會有 filename**金鑰-{guid} .xml**，其中 {guid} 是金鑰的識別碼。</span><span class="sxs-lookup"><span data-stu-id="c9774-110">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="c9774-111">每個這類檔案都包含一個金鑰。</span><span class="sxs-lookup"><span data-stu-id="c9774-111">Each such file contains a single key.</span></span> <span data-ttu-id="c9774-112">檔案的格式如下所示。</span><span class="sxs-lookup"><span data-stu-id="c9774-112">The format of the file is as follows.</span></span>

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

<span data-ttu-id="c9774-113">\<key>元素包含下列屬性和子項目：</span><span class="sxs-lookup"><span data-stu-id="c9774-113">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="c9774-114">金鑰識別碼。這個值會被視為授權;檔案名只是方便人類閱讀的 nicety。</span><span class="sxs-lookup"><span data-stu-id="c9774-114">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="c9774-115">元素的版本 \<key> ，目前已修正為1。</span><span class="sxs-lookup"><span data-stu-id="c9774-115">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="c9774-116">金鑰的建立、啟用和到期日。</span><span class="sxs-lookup"><span data-stu-id="c9774-116">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="c9774-117">\<descriptor>元素，其中包含此金鑰中包含之已驗證的加密執行的資訊。</span><span class="sxs-lookup"><span data-stu-id="c9774-117">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="c9774-118">在上述範例中，金鑰的識別碼是 {80732141-ec8f-4b80-af9c-c4d2d1ff8901}，它是在2015年3月19日建立和啟用，而且它的存留期為90天。</span><span class="sxs-lookup"><span data-stu-id="c9774-118">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="c9774-119">（在此範例中，有時啟用日期可能會稍早于建立日期之前。</span><span class="sxs-lookup"><span data-stu-id="c9774-119">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="c9774-120">這是因為 Api 的工作方式 nit，在實務上無害。）</span><span class="sxs-lookup"><span data-stu-id="c9774-120">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="c9774-121">\<descriptor> 項目</span><span class="sxs-lookup"><span data-stu-id="c9774-121">The \<descriptor> element</span></span>

<span data-ttu-id="c9774-122">外部 \<descriptor> 元素包含屬性 deserializerType，這是實作為 IAuthenticatedEncryptorDescriptorDeserializer 之類型的元件限定名稱。</span><span class="sxs-lookup"><span data-stu-id="c9774-122">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="c9774-123">此類型負責讀取內部專案 \<descriptor> ，以及用來剖析包含在內的資訊。</span><span class="sxs-lookup"><span data-stu-id="c9774-123">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="c9774-124">元素的特定格式 \<descriptor> 取決於索引鍵所封裝的已驗證加密程式實作為，而每個還原序列化程式類型預期會有稍微不同的格式。</span><span class="sxs-lookup"><span data-stu-id="c9774-124">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="c9774-125">不過，一般而言，此專案會包含演算法資訊（名稱、類型、Oid 或類似的）和秘密金鑰內容。</span><span class="sxs-lookup"><span data-stu-id="c9774-125">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="c9774-126">在上述範例中，描述元會指定此金鑰包裝 AES-256-CBC 加密 + HMACSHA256 驗證。</span><span class="sxs-lookup"><span data-stu-id="c9774-126">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="c9774-127">\<encryptedSecret> 項目</span><span class="sxs-lookup"><span data-stu-id="c9774-127">The \<encryptedSecret> element</span></span>

<span data-ttu-id="c9774-128">如果[已啟用待用密碼加密，](xref:security/data-protection/implementation/key-encryption-at-rest)則包含加密形式的秘密金鑰材料的\*\* &lt; &gt; encryptedSecret\*\*元素可能會存在。</span><span class="sxs-lookup"><span data-stu-id="c9774-128">An **&lt;encryptedSecret&gt;** element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](xref:security/data-protection/implementation/key-encryption-at-rest).</span></span> <span data-ttu-id="c9774-129">屬性 `decryptorType` 是實[IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor)之型別的元件限定名稱。</span><span class="sxs-lookup"><span data-stu-id="c9774-129">The attribute `decryptorType` is the assembly-qualified name of a type which implements [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span></span> <span data-ttu-id="c9774-130">此類型負責讀取內部\*\* &lt; encryptedKey &gt; \*\*專案，並將其解密以復原原始的純文字。</span><span class="sxs-lookup"><span data-stu-id="c9774-130">This type is responsible for reading the inner **&lt;encryptedKey&gt;** element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="c9774-131">如同 `<descriptor>` ，元素的特定格式 `<encryptedSecret>` 取決於使用中的待用加密機制。</span><span class="sxs-lookup"><span data-stu-id="c9774-131">As with `<descriptor>`, the particular format of the `<encryptedSecret>` element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="c9774-132">在上述範例中，主要金鑰是使用每個批註的 Windows DPAPI 加密。</span><span class="sxs-lookup"><span data-stu-id="c9774-132">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="c9774-133">\<revocation> 項目</span><span class="sxs-lookup"><span data-stu-id="c9774-133">The \<revocation> element</span></span>

<span data-ttu-id="c9774-134">撤銷在金鑰存放庫中是以最上層物件的形式存在。</span><span class="sxs-lookup"><span data-stu-id="c9774-134">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="c9774-135">依照慣例，撤銷的檔案名為**撤銷-{timestamp} .xml** （用於撤銷特定日期之前的所有金鑰）或**撤銷-{guid} .xml** （用於撤銷特定金鑰）。</span><span class="sxs-lookup"><span data-stu-id="c9774-135">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="c9774-136">每個檔案都包含單一 \<revocation> 元素。</span><span class="sxs-lookup"><span data-stu-id="c9774-136">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="c9774-137">針對個別金鑰的撤銷，檔案內容將如下所示。</span><span class="sxs-lookup"><span data-stu-id="c9774-137">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="c9774-138">在此情況下，只會撤銷指定的金鑰。</span><span class="sxs-lookup"><span data-stu-id="c9774-138">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="c9774-139">不過，如果金鑰識別碼是 "\*"，如下列範例所示，則會撤銷其建立日期在指定撤銷日期之前的所有金鑰。</span><span class="sxs-lookup"><span data-stu-id="c9774-139">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="c9774-140">\<reason>系統永遠不會讀取元素。</span><span class="sxs-lookup"><span data-stu-id="c9774-140">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="c9774-141">這只是一個方便的位置，可供您儲存撤銷的人們可讀取原因。</span><span class="sxs-lookup"><span data-stu-id="c9774-141">It's simply a convenient place to store a human-readable reason for revocation.</span></span>
