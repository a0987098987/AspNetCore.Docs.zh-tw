---
title: ASP.NET Core 中的金鑰儲存體格式
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護金鑰儲存格式的執行詳細資料。
ms.author: riande
ms.date: 04/08/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: d284927e8ff4315b813fe36b9c335d8bd75ece11
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776860"
---
# <a name="key-storage-format-in-aspnet-core"></a>ASP.NET Core 中的金鑰儲存體格式

<a name="data-protection-implementation-key-storage-format"></a>

物件會以靜態方式儲存在 XML 標記法中。 金鑰儲存區的預設目錄是：

* Windows： *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys\*
* macOS/Linux： *$HOME/.aspnet/dataprotection-keys*

## <a name="the-key-element"></a>> \<元素的索引鍵

金鑰是以最上層物件的形式存在於金鑰存放庫中。 依照慣例，索引鍵會有 filename**金鑰-{guid} .xml**，其中 {guid} 是金鑰的識別碼。 每個這類檔案都包含一個金鑰。 檔案的格式如下所示。

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

\<Key> 元素包含下列屬性和子專案：

* 金鑰識別碼。這個值會被視為授權;檔案名只是方便人類閱讀的 nicety。

* > 專案的索引\<鍵版本，目前已修正為1。

* 金鑰的建立、啟用和到期日。

* > \<專案的描述項，其中包含此金鑰中包含之已驗證加密執行的資訊。

在上述範例中，金鑰的識別碼是 {80732141-ec8f-4b80-af9c-c4d2d1ff8901}，它是在2015年3月19日建立和啟用，而且它的存留期為90天。 （在此範例中，有時啟用日期可能會稍早于建立日期之前。 這是因為 Api 的工作方式 nit，在實務上無害。）

## <a name="the-descriptor-element"></a>> \<元素的描述項

外部\<描述項> 元素包含屬性 deserializerType，這是實作為 IAuthenticatedEncryptorDescriptorDeserializer 之類型的元件限定名稱。 此類型負責讀取內部\<描述元> 專案，以及剖析包含在內的資訊。

\<描述元> 專案的特定格式取決於索引鍵所封裝的驗證加密程式實作為，而每個還原序列化程式類型預期會有稍微不同的格式。 不過，一般而言，此專案會包含演算法資訊（名稱、類型、Oid 或類似的）和秘密金鑰內容。 在上述範例中，描述元會指定此金鑰包裝 AES-256-CBC 加密 + HMACSHA256 驗證。

## <a name="the-encryptedsecret-element"></a>\<EncryptedSecret> 元素

如果[已啟用待用密碼加密，](xref:security/data-protection/implementation/key-encryption-at-rest)則包含加密形式的秘密金鑰材料的** &lt;encryptedSecret&gt; **元素可能會存在。 屬性`decryptorType`是實[IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor)之型別的元件限定名稱。 此類型負責讀取內部** &lt;encryptedKey&gt; **專案，並將其解密以復原原始的純文字。

如同`<descriptor>`， `<encryptedSecret>`元素的特定格式取決於使用中的待用加密機制。 在上述範例中，主要金鑰是使用每個批註的 Windows DPAPI 加密。

## <a name="the-revocation-element"></a>\<撤銷> 元素

撤銷在金鑰存放庫中是以最上層物件的形式存在。 依照慣例，撤銷的檔案名為**撤銷-{timestamp} .xml** （用於撤銷特定日期之前的所有金鑰）或**撤銷-{guid} .xml** （用於撤銷特定金鑰）。 每個檔案都包含\<一個撤銷> 元素。

針對個別金鑰的撤銷，檔案內容將如下所示。

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

在此情況下，只會撤銷指定的金鑰。 不過，如果金鑰識別碼是 "*"，如下列範例所示，則會撤銷其建立日期在指定撤銷日期之前的所有金鑰。

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

系統\<永遠不會讀取> 元素的原因。 這只是一個方便的位置，可供您儲存撤銷的人們可讀取原因。
