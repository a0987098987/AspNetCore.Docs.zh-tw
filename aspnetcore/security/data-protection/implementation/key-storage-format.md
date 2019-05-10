---
title: ASP.NET Core 中的金鑰儲存體格式
author: rick-anderson
description: 了解 ASP.NET Core 資料保護金鑰的儲存格式的實作詳細資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 81df124f3dd0cadf8fd895ab55f66eec6415705f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897475"
---
# <a name="key-storage-format-in-aspnet-core"></a>ASP.NET Core 中的金鑰儲存體格式

<a name="data-protection-implementation-key-storage-format"></a>

物件會儲存在 XML 表示法中的其餘部分。 金鑰的儲存體的預設目錄為 %localappdata%\asp.net\dataprotection-keys\。

## <a name="the-key-element"></a>\<金鑰 > 項目

索引鍵存在做為索引鍵的存放庫中的最上層物件。 依照慣例，索引鍵有檔名**金鑰-{guid}.xml**，其中 {guid} 是金鑰的識別碼。 每個這類檔案包含單一索引鍵。 檔案的格式如下所示。

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

\<金鑰 > 項目包含下列屬性和子項目：

* 金鑰的識別碼。這個值會視為可靠;檔案名稱是只以提高可讀性 nicety。

* 版本\<金鑰 > 項目，目前固定為 1。

* 金鑰的建立、 啟用和到期日期。

* A\<描述元 > 元素，其中包含已驗證的加密實作包含在此機碼的詳細資訊。

在上述範例中，索引鍵的識別碼是 {80732141-ec8f-4b80-af9c-c4d2d1ff8901}、 它所建立及啟動在 2015 年 3 月 19 日起，以及具有 90 天的存留期。 （有時候啟用日期可能會稍有如此範例所示的建立日期之前。 這是因為在 Api 的運作方式，並不會有影響實際上 nit。）

## <a name="the-descriptor-element"></a>\<描述元 > 項目

外部\<描述元 > 項目包含屬性 deserializerType，也就是它會實作 IAuthenticatedEncryptorDescriptorDeserializer 類型的組件限定名稱。 此類型是負責讀取內部\<描述元 > 項目和剖析內所包含的資訊。

特定的格式\<描述元 > 項目取決於已驗證的加密程式實作封裝依索引鍵，和每個還原序列化程式型別預期這稍微不同的格式。 一般情況下，不過，這個項目會包含演算法的資訊 (名稱、 型別，Oid，或類似) 和祕密金鑰的資料。 在上述範例中，描述元會指定此金鑰包裝 AES-256-CBC 加密 + HMACSHA256 驗證。

## <a name="the-encryptedsecret-element"></a>\<EncryptedSecret > 項目

**&lt;EncryptedSecret&gt;** 項目，其中包含加密的形式的祕密金鑰的內容可能會存在於如果[加密靜止的祕密已啟用](xref:security/data-protection/implementation/key-encryption-at-rest)。 屬性`decryptorType`是實作型別組件限定名稱[IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor)。 此類型是負責讀取內部 **&lt;encryptedKey&gt;** 項目和解密來復原原始的純文字。

如同`<descriptor>`，特定的格式`<encryptedSecret>`項目取決於使用中的待用加密機制。 在上述範例中，為每個註解中使用 Windows DPAPI 加密主要金鑰。

## <a name="the-revocation-element"></a>\<撤銷 > 項目

撤銷金鑰的儲存機制中的最上層物件的形式存在。 依照慣例撤銷有檔名**撤銷-{timestamp}.xml** （適用於特定日期之前撤銷所有索引鍵） 或**撤銷-{guid}.xml** （適用於撤銷特定的索引鍵）。 每個檔案包含單一\<撤銷 > 項目。

對於撤銷的個別索引鍵，會將檔案內容，如下所示。

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

在此情況下，只有指定的索引鍵已被撤銷。 金鑰識別碼是否"*"，不過，如下列範例中，其建立日期會指定的撤銷日期之前的所有索引鍵已撤銷。

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

\<原因 > 項目永遠不會讀取系統。 它是只是方便的地方來儲存撤銷的人類看得懂的原因。
