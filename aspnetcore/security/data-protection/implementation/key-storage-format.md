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
# <a name="key-storage-format-in-aspnet-core"></a>ASP.NET核心中的關鍵儲存格式

<a name="data-protection-implementation-key-storage-format"></a>

物件以 XML 表示形式保存。 金鑰儲存的預設目錄是:

* 視窗: _%本地應用資料%%\ASP.NET_資料保護-金鑰\*
* macOS / Linux: *$HOME/.aspnet/資料保護-密鑰*

## <a name="the-key-element"></a>秒\<>元素

金鑰作為頂級物件存在於密鑰存儲庫中。 根據約定鍵具有檔名**鍵-{guid}.xml,** 其中{guid} 是密鑰的 ID。 每個此類檔都包含一個鍵。 檔的格式如下。

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

\<鍵>元素包含以下屬性和子元素:

* 金鑰識別碼。此值被視為權威值;檔名對於人類的可讀性來說簡直就是一個不錯的。

* \<鍵>元素的版本,當前固定在 1。

* 密鑰的創建、啟動和到期日期。

* \<描述符>元素,其中包含有關此密鑰中包含的經過身份驗證的加密實現的資訊。

在上面的示例中,密鑰的 ID 是 {80732141-ec8f-4b80-af9c-c4d2d1ff8901},它於 2015 年 3 月 19 日創建並啟動,其生存期為 90 天。 (有時啟動日期可能稍微早於創建日期,如本示例所示。 這是由於 API 的工作方式中一點原因,並且在實踐中是無害的。

## <a name="the-descriptor-element"></a>\<描述器>元素

外部\<描述符>元素包含一個屬性反序列化類型,該類型是實現 IAuthenticatedEncryptor 描述器解序列化的類型的程式集限定名稱。 此類型負責讀取內部\<描述符>元素並分析 中包含的資訊。

\<描述符>元素的特定格式取決於由密鑰封裝的經過身份驗證的加密器實現,並且每種反序列化器類型都期望為此採用略有不同的格式。 但是,通常,此元素將包含演演演算法資訊(名稱、類型、OID 或類似)和密鑰材料。 在上面的示例中,描述符指定此密鑰包裝 AES-256-CBC 加密 + HMACSHA256 驗證。

## <a name="the-encryptedsecret-element"></a>加密\<的機密>元素

如果[啟用靜態機密加密](xref:security/data-protection/implementation/key-encryption-at-rest)加密,則可能存在包含密鑰材料加密形式的**&lt;&gt;加密機密**元素。 該屬性`decryptorType`是實現[IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor)的類型的程式集限定名稱。 此類型負責讀取內部**&lt;加密密&gt;鑰**元素並解密它以復原原始純文本。

與`<descriptor>`,`<encryptedSecret>`元素的特定格式取決於正在使用的靜態加密機制。 在上面的示例中,主密鑰根據註釋使用 Windows DPAPI 進行加密。

## <a name="the-revocation-element"></a>吊銷\<>元素

吊銷作為頂級物件存在於密鑰存儲庫中。 根據約定吊銷具有檔名**吊銷-[時間戳].xml(** 用於在特定日期之前撤銷所有密鑰)或**吊銷-{guid_.xml(** 用於撤銷特定密鑰)。 每個檔包含一個\<吊銷>元素。

對於單個鍵的吊銷,文件內容如下。

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

在這種情況下,僅吊銷指定的密鑰。 但是,如果密鑰 ID 為"*",則如以下示例所示,創建日期早於指定吊銷日期的所有密鑰將被吊銷。

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

系統\<永遠不會讀取>元素的原因。 它只是一個方便的地方,存儲一個人類可讀的理由的撤銷。
