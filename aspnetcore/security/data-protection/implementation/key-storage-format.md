---
title: ASP.NET核心中的關鍵儲存格式
author: rick-anderson
description: 瞭解ASP.NET核心資料保護密鑰存儲格式的實現詳細資訊。
ms.author: riande
ms.date: 04/08/2020
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 3072c673791b589027a910b80eaba52052eb9311
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80976933"
---
# <a name="key-storage-format-in-aspnet-core"></a><span data-ttu-id="74788-103">ASP.NET核心中的關鍵儲存格式</span><span class="sxs-lookup"><span data-stu-id="74788-103">Key storage format in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="74788-104">物件以 XML 表示形式保存。</span><span class="sxs-lookup"><span data-stu-id="74788-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="74788-105">金鑰儲存的預設目錄是:</span><span class="sxs-lookup"><span data-stu-id="74788-105">The default directory for key storage is:</span></span>

* <span data-ttu-id="74788-106">視窗: _%本地應用資料%%\ASP.NET_資料保護-金鑰\*</span><span class="sxs-lookup"><span data-stu-id="74788-106">Windows: \*%LOCALAPPDATA%\ASP.NET\DataProtection-Keys\*</span></span>
* <span data-ttu-id="74788-107">macOS / Linux: *$HOME/.aspnet/資料保護-密鑰*</span><span class="sxs-lookup"><span data-stu-id="74788-107">macOS / Linux: *$HOME/.aspnet/DataProtection-Keys*</span></span>

## <a name="the-key-element"></a><span data-ttu-id="74788-108">秒\<>元素</span><span class="sxs-lookup"><span data-stu-id="74788-108">The \<key> element</span></span>

<span data-ttu-id="74788-109">金鑰作為頂級物件存在於密鑰存儲庫中。</span><span class="sxs-lookup"><span data-stu-id="74788-109">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="74788-110">根據約定鍵具有檔名**鍵-{guid}.xml,** 其中{guid} 是密鑰的 ID。</span><span class="sxs-lookup"><span data-stu-id="74788-110">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="74788-111">每個此類檔都包含一個鍵。</span><span class="sxs-lookup"><span data-stu-id="74788-111">Each such file contains a single key.</span></span> <span data-ttu-id="74788-112">檔的格式如下。</span><span class="sxs-lookup"><span data-stu-id="74788-112">The format of the file is as follows.</span></span>

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

<span data-ttu-id="74788-113">\<鍵>元素包含以下屬性和子元素:</span><span class="sxs-lookup"><span data-stu-id="74788-113">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="74788-114">金鑰識別碼。此值被視為權威值;檔名對於人類的可讀性來說簡直就是一個不錯的。</span><span class="sxs-lookup"><span data-stu-id="74788-114">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="74788-115">\<鍵>元素的版本,當前固定在 1。</span><span class="sxs-lookup"><span data-stu-id="74788-115">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="74788-116">密鑰的創建、啟動和到期日期。</span><span class="sxs-lookup"><span data-stu-id="74788-116">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="74788-117">\<描述符>元素,其中包含有關此密鑰中包含的經過身份驗證的加密實現的資訊。</span><span class="sxs-lookup"><span data-stu-id="74788-117">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="74788-118">在上面的示例中,密鑰的 ID 是 {80732141-ec8f-4b80-af9c-c4d2d1ff8901},它於 2015 年 3 月 19 日創建並啟動,其生存期為 90 天。</span><span class="sxs-lookup"><span data-stu-id="74788-118">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="74788-119">(有時啟動日期可能稍微早於創建日期,如本示例所示。</span><span class="sxs-lookup"><span data-stu-id="74788-119">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="74788-120">這是由於 API 的工作方式中一點原因,並且在實踐中是無害的。</span><span class="sxs-lookup"><span data-stu-id="74788-120">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="74788-121">\<描述器>元素</span><span class="sxs-lookup"><span data-stu-id="74788-121">The \<descriptor> element</span></span>

<span data-ttu-id="74788-122">外部\<描述符>元素包含一個屬性反序列化類型,該類型是實現 IAuthenticatedEncryptor 描述器解序列化的類型的程式集限定名稱。</span><span class="sxs-lookup"><span data-stu-id="74788-122">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="74788-123">此類型負責讀取內部\<描述符>元素並分析 中包含的資訊。</span><span class="sxs-lookup"><span data-stu-id="74788-123">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="74788-124">\<描述符>元素的特定格式取決於由密鑰封裝的經過身份驗證的加密器實現,並且每種反序列化器類型都期望為此採用略有不同的格式。</span><span class="sxs-lookup"><span data-stu-id="74788-124">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="74788-125">但是,通常,此元素將包含演演演算法資訊(名稱、類型、OID 或類似)和密鑰材料。</span><span class="sxs-lookup"><span data-stu-id="74788-125">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="74788-126">在上面的示例中,描述符指定此密鑰包裝 AES-256-CBC 加密 + HMACSHA256 驗證。</span><span class="sxs-lookup"><span data-stu-id="74788-126">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="74788-127">加密\<的機密>元素</span><span class="sxs-lookup"><span data-stu-id="74788-127">The \<encryptedSecret> element</span></span>

<span data-ttu-id="74788-128">如果[啟用靜態機密加密](xref:security/data-protection/implementation/key-encryption-at-rest)加密,則可能存在包含密鑰材料加密形式的**&lt;&gt;加密機密**元素。</span><span class="sxs-lookup"><span data-stu-id="74788-128">An **&lt;encryptedSecret&gt;** element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](xref:security/data-protection/implementation/key-encryption-at-rest).</span></span> <span data-ttu-id="74788-129">該屬性`decryptorType`是實現[IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor)的類型的程式集限定名稱。</span><span class="sxs-lookup"><span data-stu-id="74788-129">The attribute `decryptorType` is the assembly-qualified name of a type which implements [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span></span> <span data-ttu-id="74788-130">此類型負責讀取內部**&lt;加密密&gt;鑰**元素並解密它以復原原始純文本。</span><span class="sxs-lookup"><span data-stu-id="74788-130">This type is responsible for reading the inner **&lt;encryptedKey&gt;** element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="74788-131">與`<descriptor>`,`<encryptedSecret>`元素的特定格式取決於正在使用的靜態加密機制。</span><span class="sxs-lookup"><span data-stu-id="74788-131">As with `<descriptor>`, the particular format of the `<encryptedSecret>` element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="74788-132">在上面的示例中,主密鑰根據註釋使用 Windows DPAPI 進行加密。</span><span class="sxs-lookup"><span data-stu-id="74788-132">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="74788-133">吊銷\<>元素</span><span class="sxs-lookup"><span data-stu-id="74788-133">The \<revocation> element</span></span>

<span data-ttu-id="74788-134">吊銷作為頂級物件存在於密鑰存儲庫中。</span><span class="sxs-lookup"><span data-stu-id="74788-134">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="74788-135">根據約定吊銷具有檔名**吊銷-[時間戳].xml(** 用於在特定日期之前撤銷所有密鑰)或**吊銷-{guid_.xml(** 用於撤銷特定密鑰)。</span><span class="sxs-lookup"><span data-stu-id="74788-135">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="74788-136">每個檔包含一個\<吊銷>元素。</span><span class="sxs-lookup"><span data-stu-id="74788-136">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="74788-137">對於單個鍵的吊銷,文件內容如下。</span><span class="sxs-lookup"><span data-stu-id="74788-137">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="74788-138">在這種情況下,僅吊銷指定的密鑰。</span><span class="sxs-lookup"><span data-stu-id="74788-138">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="74788-139">但是,如果密鑰 ID 為"\*",則如以下示例所示,創建日期早於指定吊銷日期的所有密鑰將被吊銷。</span><span class="sxs-lookup"><span data-stu-id="74788-139">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="74788-140">系統\<永遠不會讀取>元素的原因。</span><span class="sxs-lookup"><span data-stu-id="74788-140">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="74788-141">它只是一個方便的地方,存儲一個人類可讀的理由的撤銷。</span><span class="sxs-lookup"><span data-stu-id="74788-141">It's simply a convenient place to store a human-readable reason for revocation.</span></span>
