---
title: "金鑰管理的擴充性"
author: rick-anderson
description: "本文概述 ASP.NET Core 資料保護金鑰管理的擴充性。"
ms.author: riande
manager: wpickett
ms.date: 11/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: d748ee9d3edf9eed4285fab447d5b379dfcd937c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="key-management-extensibility"></a>金鑰管理的擴充性

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> 讀取[金鑰管理](../implementation/key-management.md#data-protection-implementation-key-management)閱讀本節中，因為它會說明這些 Api 的基本概念的某些之前 > 一節。

>[!WARNING]
> 實作下列介面的型別應該是安全執行緒的多個呼叫端。

## <a name="key"></a>Key

`IKey`介面是加密系統都中的索引鍵的基本表示法。 這裡使用的詞彙的索引鍵是抽象的意義而言，不是處於 「 密碼編譯金鑰內容 」 的常值的意義。 索引鍵具有下列屬性：

* 啟用、 建立和到期日期

* 撤銷狀態

* 金鑰識別碼 (GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

此外，`IKey`公開`CreateEncryptor`方法可以用來建立[IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor)這個索引鍵繫結的執行個體。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

此外，`IKey`公開`CreateEncryptorInstance`方法可以用來建立[IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor)這個索引鍵繫結的執行個體。

---

> [!NOTE]
> 沒有任何 API 來擷取未經處理的密碼編譯內容，從`IKey`執行個體。

## <a name="ikeymanager"></a>IKeyManager

`IKeyManager`介面代表負責一般金鑰儲存、 擷取及管理的物件。 它會公開三個高階的作業：

* 建立新的金鑰，並將它保存到儲存體。

* 從存放區中取得所有索引鍵。

* 撤銷一或多個金鑰，並將保存到儲存體的撤銷資訊。

>[!WARNING]
> 撰寫`IKeyManager`的非常進階的工作，而且大部分的開發人員不應該嘗試它。 相反地，大部分的開發人員應利用所提供的功能[XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager)類別。

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

`XmlKeyManager`型別是內建的具象實作`IKeyManager`。 它提供數個實用的功能，包括金鑰委付和加密在靜止的索引鍵。 在此系統中的索引鍵會表示為 XML 項目 (具體而言， [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview))。

`XmlKeyManager`在完成其工作過程中的其他數個元件而定：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* `AlgorithmConfiguration`其中指出新的金鑰所使用的演算法。

* `IXmlRepository`其中索引鍵會保存在儲存體中的控制項。

* `IXmlEncryptor`[選用]，可讓加密在靜止的索引鍵。

* `IKeyEscrowSink`[選用]，以提供金鑰委付服務。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `IXmlRepository`其中索引鍵會保存在儲存體中的控制項。

* `IXmlEncryptor`[選用]，可讓加密在靜止的索引鍵。

* `IKeyEscrowSink`[選用]，以提供金鑰委付服務。

---

以下是高層級圖，這表示如何這些元件有線內的一起`XmlKeyManager`。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![建立金鑰](key-management/_static/keycreation2.png)

   *金鑰建立 / CreateNewKey*

實作中`CreateNewKey`、`AlgorithmConfiguration`元件用來建立唯一`IAuthenticatedEncryptorDescriptor`，其接著會序列化為 XML。 如果金鑰委付接收存在時，原始 （未加密） 的 XML 供接收長期儲存。 然後透過執行未加密的 XML `IXmlEncryptor` （如有必要） 來產生加密的 XML 文件。 這個加密的文件會保存長期的儲存體透過`IXmlRepository`。 (如果有任何`IXmlEncryptor`是設定，未加密的文件會持續保留在`IXmlRepository`。)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![建立金鑰](key-management/_static/keycreation1.png)

   *金鑰建立 / CreateNewKey*

實作中`CreateNewKey`、`IAuthenticatedEncryptorConfiguration`元件用來建立唯一`IAuthenticatedEncryptorDescriptor`，其接著會序列化為 XML。 如果金鑰委付接收存在時，原始 （未加密） 的 XML 供接收長期儲存。 然後透過執行未加密的 XML `IXmlEncryptor` （如有必要） 來產生加密的 XML 文件。 這個加密的文件會保存長期的儲存體透過`IXmlRepository`。 (如果有任何`IXmlEncryptor`是設定，未加密的文件會持續保留在`IXmlRepository`。)

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![金鑰擷取](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![金鑰擷取](key-management/_static/keyretrieval1.png)

---

   *索引鍵擷取 / GetAllKeys*

在實作`GetAllKeys`、 XML 文件代表索引鍵和撤銷讀取基礎`IXmlRepository`。 如果要加密這些文件，系統會自動解密這些檔案。 `XmlKeyManager`建立適當`IAuthenticatedEncryptorDescriptorDeserializer`還原序列化文件的執行個體放回`IAuthenticatedEncryptorDescriptor`執行個體，然後將包裝在個別`IKey`執行個體。 此集合`IKey`執行個體傳回給呼叫者。

中可以找到特定的 XML 項目上的進一步資訊[金鑰儲存格式的文件](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format)。

## <a name="ixmlrepository"></a>IXmlRepository

`IXmlRepository`介面代表可保存到 XML 和 XML 擷取備份存放區的類型。 它會公開兩個 Api:

* GetAllElements(): IReadOnlyCollection<XElement>

* StoreElement （XElement 項目，字串 friendlyName）

實作`IXmlRepository`不需要剖析 XML 傳遞給它們。 它們應該將視為不透明的 XML 文件，並讓較高層級擔心產生及剖析的文件。

有兩種內建的具象類型實作`IXmlRepository`:`FileSystemXmlRepository`和`RegistryXmlRepository`。 請參閱[金鑰儲存提供者文件](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers)如需詳細資訊。 註冊自訂`IXmlRepository`會是適當的方式來使用不同的備份存放區，例如 Azure Blob 儲存體。

若要變更預設儲存機制整個應用程式，註冊自訂`IXmlRepository`執行個體：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

`IXmlEncryptor`介面代表可以加密純文字 XML 項目類型。 它會公開單一 API:

* 加密 (XElement plaintextElement): EncryptedXmlInfo

如果序列化`IAuthenticatedEncryptorDescriptor`包含任何項目，然後標示為 「 需要加密 」`XmlKeyManager`會執行這些項目，透過設定`IXmlEncryptor`的`Encrypt`方法，而且它將會保存 enciphered 項目而非以純文字項目`IXmlRepository`。 輸出`Encrypt`方法`EncryptedXmlInfo`物件。 此物件是包裝函式包含兩個結果 enciphered`XElement`代表的類型和`IXmlDecryptor`這可以用來解密的對應元素。

有四種內建的具象類型實作`IXmlEncryptor`:
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

請參閱[在其他文件的金鑰加密](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)如需詳細資訊。

若要變更預設的索引鍵---靜態加密機制整個應用程式，註冊自訂`IXmlEncryptor`執行個體：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a>IXmlDecryptor

`IXmlDecryptor`介面代表知道如何解密類型`XElement`，已透過 enciphered `IXmlEncryptor`。 它會公開單一 API:

* 解密 (XElement encryptedElement): XElement

`Decrypt`方法復原所執行的加密`IXmlEncryptor.Encrypt`。 一般而言，每個實體`IXmlEncryptor`實作會有對應的具體`IXmlDecryptor`實作。

型別實作哪些`IXmlDecryptor`應該有下列兩個公用建構函式的其中一個：

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> `IServiceProvider`傳遞至建構函式可能是 null。

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

`IKeyEscrowSink`介面代表可執行委付機密資訊的類型。 請注意，序列化的項目中可能包含機密資訊 （例如密碼編譯的內容），這會導致所有的簡介[IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor)輸入在第一次。 不過，事故發生，而且索引鍵的環形才能刪除或損毀。

委付介面提供緊急逸出之轉換的任何設定前，允許存取原始的序列化 XML 規劃[IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor)。 介面會公開單一 API:

* 存放區 （Guid keyId、 XElement 項目）

它會`IKeyEscrowSink`實作以商務原則與一致的安全方式處理所提供項目。 一個可能的實作可用於委付接收加密使用已知的公司 X.509 憑證的 XML 項目，其中憑證的私密金鑰已委付;`CertificateXmlEncryptor`可以協助進行此設定類型。 `IKeyEscrowSink`實作也會負責適當地保存所提供項目。

預設沒有委付機制會啟用，但伺服器系統管理員可以[全域設定此](xref:security/data-protection/configuration/machine-wide-policy)。 它也可以設定以程式設計方式透過`IDataProtectionBuilder.AddKeyEscrowSink`方法，如下列範例所示。 `AddKeyEscrowSink`方法多載鏡像`IServiceCollection.AddSingleton`和`IServiceCollection.AddInstance`多載，為`IKeyEscrowSink`執行個體要作為 singleton。 若為多個`IKeyEscrowSink`註冊執行個體，將金鑰在產生期間，呼叫每個，因此可以委付金鑰至多個機制同時。

沒有任何 API 來讀取資料，從`IKeyEscrowSink`執行個體。 這是一致的委付機制的設計理論： 它已用來讓受信任的授權單位，都能存取金鑰的內容和應用程式本身不是受信任的授權單位，因此不應該有它自己附帶資料的存取。

下列程式碼範例示範建立和註冊`IKeyEscrowSink`，使得只有 「 CONTOSODomain 系統管理員 」 的成員可以復原他們委付金鑰。

> [!NOTE]
> 若要執行此範例中，您必須是在已加入網域的 Windows 8 / Windows Server 2012 電腦和網域控制站必須是 Windows Server 2012 或更新版本。

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
