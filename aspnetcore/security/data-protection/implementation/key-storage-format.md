---
title: "金鑰的儲存體格式"
author: tdykstra
description: "本文件說明 ASP.NET Core 資料保護的金鑰儲存格式的實作詳細資料。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 66783eb7264a4551eafdd9d5c7d99b014701a6de
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="key-storage-format"></a>金鑰的儲存體格式

<a name="data-protection-implementation-key-storage-format"></a>

物件會儲存在 XML 表示法中的其餘部分。 金鑰儲存的預設目錄是 %localappdata%\asp.net\dataprotection-keys\。

## <a name="the-key-element"></a>\<金鑰 > 項目

索引鍵做為索引鍵的儲存機制中的最上層物件。 依慣例有檔名**金鑰-{guid}.xml**，其中 {guid} 是金鑰的識別碼。 每個這類檔案包含單一索引鍵。 檔案的格式如下所示。

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

\<金鑰 > 元素包含下列屬性和子項目：

* 索引鍵的識別碼。這個值會被視為可靠;檔名不只是人力可讀性 nicety。

* 版本\<金鑰 > 項目，目前固定為 1。

* 金鑰的建立、 啟用和到期日期。

* A\<描述元 > 元素，其包含已驗證的加密實作包含在此機碼的相關資訊。

在上述範例中，索引鍵的識別碼是 {80732141-ec8f-4b80-af9c-c4d2d1ff8901}、 用來建立並啟動於 2015 年 3 月 19 日，以及具有的 90 天的存留期。 （有時啟用日期可能會稍微如此範例所示的建立日期之前。 這是因為 n Api 的運作方式，並是無害的在實務中。）

## <a name="the-descriptor-element"></a>\<描述元 > 項目

外部\<描述元 > 項目包含屬性 deserializerType，這是實作 IAuthenticatedEncryptorDescriptorDeserializer 型別的組件限定名稱。 此類型是負責讀取內部\<描述元 > 項目和剖析內所包含的資訊。

特定的格式\<描述元 > 項目取決於已驗證的加密程式實作由索引鍵，封裝和這個預期稍有不同格式的每個還原序列化程式型別。 一般情況下，不過，這個項目會包含演算法的資訊 (名稱、 型別，Oid，或類似) 和秘密金鑰內容。 在上述範例中，描述元會指定此金鑰包裝 AES-CBC-256 加密 + HMACSHA256 驗證。

## <a name="the-encryptedsecret-element"></a>\<EncryptedSecret > 項目

<encryptedSecret>包含加密的秘密金鑰材料表單項目可能會存在於如果[啟用加密的密碼在靜止](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)。 屬性 decryptorType 會實作 IXmlDecryptor 類型的組件限定名稱。 此類型是負責讀取內部<encryptedKey>項目，來復原原始的純文字解密。

如同\<描述元 >，特定的格式<encryptedSecret>項目相依於使用中的靜止加密機制。 在上述範例中，為每個註解中使用 Windows DPAPI 加密主要金鑰。

## <a name="the-revocation-element"></a>\<撤銷 > 項目

撤銷做為索引鍵的儲存機制中的最上層物件。 依照慣例撤銷有檔名**撤銷-{timestamp}.xml** （適用於特定日期之前撤銷所有索引鍵） 或**撤銷-{guid}.xml** （適用於撤銷特定的索引鍵）。 每個檔案包含單一\<撤銷 > 項目。

針對撤銷的個別索引鍵，會將檔案內容如下。

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

在此情況下，指定索引鍵已被撤銷。 金鑰識別碼是否"*"，不過，在下列範例中，會撤銷其建立日期會指定的撤銷日期之前的所有索引鍵。

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

\<原因 > 系統永遠不會讀取項目。 它是只是方便的位置來儲存人類看得懂的撤銷原因。
