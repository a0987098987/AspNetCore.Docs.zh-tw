---
title: ASP.NET Core 中的金鑰管理擴充性
author: rick-anderson
description: 深入了解 ASP.NET Core 資料保護金鑰管理擴充性。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284600"
---
# <a name="key-management-extensibility-in-aspnet-core"></a>ASP.NET Core 中的金鑰管理擴充性

> [!TIP]
> 讀取[金鑰管理](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)閱讀本節中，因為它會說明這些 Api 的基本概念的一些之前的一節。

> [!WARNING]
> 會實作下列介面的任何的類型應該是安全執行緒的多個呼叫端。

## <a name="key"></a>Key

`IKey`介面是加密系統中的索引鍵的基本表示法。 詞彙索引鍵此處用抽象的意義而言，不在 「 密碼編譯金鑰的內容 」 的常值的意義。 索引鍵具有下列屬性：

* 啟用、 建立和到期日期

* 撤銷狀態

* 索引鍵識別項 (GUID)

::: moniker range=">= aspnetcore-2.0"

此外，`IKey`公開`CreateEncryptor`方法可用來建立[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)執行個體繫結至這個索引鍵。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

此外，`IKey`公開`CreateEncryptorInstance`方法可用來建立[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)執行個體繫結至這個索引鍵。

::: moniker-end

> [!NOTE]
> 不沒有要擷取的未經處理的加密編譯內容，從任何 API`IKey`執行個體。

## <a name="ikeymanager"></a>IKeyManager

`IKeyManager`介面代表負責進行一般的金鑰儲存、 擷取及操作的物件。 它會公開三個高層級的作業：

* 建立新的金鑰，並將它保存到儲存體。

* 從儲存體中取得所有索引鍵。

* 撤銷一或多個金鑰，並保存至儲存體的撤銷資訊。

>[!WARNING]
> 撰寫`IKeyManager`非常進階的工作中，而且大部分的開發人員不應該嘗試它。 相反地，大部分的開發人員應該利用所提供的功能[XmlKeyManager](#xmlkeymanager)類別。

## <a name="xmlkeymanager"></a>XmlKeyManager

`XmlKeyManager`型別是內建的具體實作`IKeyManager`。 它提供數個實用的功能，包括金鑰委付和待用的金鑰加密。 在此系統中的索引鍵會表示為 XML 項目 (具體而言， [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview))。

`XmlKeyManager` 在完成其工作的過程中的數個其他元件而定：

::: moniker range=">= aspnetcore-2.0"

* `AlgorithmConfiguration`指出新的索引鍵所使用的演算法。

* `IXmlRepository`其中索引鍵會保存在儲存體中的控制項。

* `IXmlEncryptor` [選擇性]，可讓加密待用的金鑰。

* `IKeyEscrowSink` [選擇性]，它會提供金鑰委付服務。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`其中索引鍵會保存在儲存體中的控制項。

* `IXmlEncryptor` [選擇性]，可讓加密待用的金鑰。

* `IKeyEscrowSink` [選擇性]，它會提供金鑰委付服務。

::: moniker-end

以下是高階的圖表指出如何這些元件連接起來內`XmlKeyManager`。

::: moniker range=">= aspnetcore-2.0"

![建立金鑰](key-management/_static/keycreation2.png)

*索引鍵建立 / CreateNewKey*

在實作`CreateNewKey`，則`AlgorithmConfiguration`元件用來建立唯一`IAuthenticatedEncryptorDescriptor`，這接著會序列化為 XML。 如果金鑰委付接收，則原始 （未加密） 的 XML 可接收長期的儲存體。 未加密的 XML 接著會執行`IXmlEncryptor`（如有必要） 來產生加密的 XML 文件。 這個加密的文件會保存到長期儲存體透過`IXmlRepository`。 (如果有任何`IXmlEncryptor`已設定，未加密的文件會保存在`IXmlRepository`。)

![金鑰擷取](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![建立金鑰](key-management/_static/keycreation1.png)

*索引鍵建立 / CreateNewKey*

在實作`CreateNewKey`，則`IAuthenticatedEncryptorConfiguration`元件用來建立唯一`IAuthenticatedEncryptorDescriptor`，這接著會序列化為 XML。 如果金鑰委付接收，則原始 （未加密） 的 XML 可接收長期的儲存體。 未加密的 XML 接著會執行`IXmlEncryptor`（如有必要） 來產生加密的 XML 文件。 這個加密的文件會保存到長期儲存體透過`IXmlRepository`。 (如果有任何`IXmlEncryptor`已設定，未加密的文件會保存在`IXmlRepository`。)

![金鑰擷取](key-management/_static/keyretrieval1.png)

::: moniker-end

*索引鍵擷取 / GetAllKeys*

在實作`GetAllKeys`、 XML 文件表示的金鑰和撤銷讀取基礎`IXmlRepository`。 如果這些文件已加密，系統會自動解密。 `XmlKeyManager` 建立適當`IAuthenticatedEncryptorDescriptorDeserializer`還原序列化文件的執行個體回到`IAuthenticatedEncryptorDescriptor`執行個體，然後將包裝在個別`IKey`執行個體。 這個集合`IKey`執行個體傳回給呼叫端。

在特定的 XML 項目上的進一步資訊可在[金鑰儲存格式的文件](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format)。

## <a name="ixmlrepository"></a>IXmlRepository

`IXmlRepository`介面代表可保存到 XML，並從備份存放區中擷取 XML 類型。 它會公開兩個 Api:

* `GetAllElements` :`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

實作`IXmlRepository`不需要剖析 XML 傳遞給它們。 它們應該視為不透明的 XML 文件，並讓擔心產生和剖析的文件的更高層。

有四種內建的具象類型實作`IXmlRepository`:

::: moniker range=">= aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

請參閱[金鑰儲存提供者文件](xref:security/data-protection/implementation/key-storage-providers)如需詳細資訊。

註冊自訂`IXmlRepository`適合使用不同的備份存放區 （例如 Azure 資料表儲存體） 時。

若要變更預設存放庫整個應用程式，註冊自訂`IXmlRepository`執行個體：

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
```

::: moniker-end

## <a name="ixmlencryptor"></a>IXmlEncryptor

`IXmlEncryptor`介面代表一種類型，可以將加密的純文字 XML 項目。 它會公開單一的 API:

* Encrypt(XElement plaintextElement):EncryptedXmlInfo

如果序列化`IAuthenticatedEncryptorDescriptor`包含任何項目，然後標示為 「 需要加密 」`XmlKeyManager`會透過已設定執行這些項目`IXmlEncryptor`的`Encrypt`方法，且其會保存已譯成密碼的項目而非以純文字項目`IXmlRepository`。 輸出`Encrypt`方法是`EncryptedXmlInfo`物件。 這個物件是其中包含已譯成密碼的這兩個結果的包裝函式`XElement`代表的類型和`IXmlDecryptor`這可用來解密對應的項目。

有四種內建的具象類型實作`IXmlEncryptor`:

* [CertificateXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [DpapiNGXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [DpapiXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [NullXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

請參閱[rest 文件的金鑰加密](xref:security/data-protection/implementation/key-encryption-at-rest)如需詳細資訊。

若要變更預設的索引鍵--待用加密機制整個應用程式，註冊自訂`IXmlEncryptor`執行個體：

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
```

::: moniker-end

## <a name="ixmldecryptor"></a>IXmlDecryptor

`IXmlDecryptor`介面表示型別，以便將解密`XElement`會是已譯成密碼透過`IXmlEncryptor`。 它會公開單一的 API:

* 解密 (XElement encryptedElement):XElement

`Decrypt`方法復原所執行的加密`IXmlEncryptor.Encrypt`。 一般而言，每個具體`IXmlEncryptor`實作會有相對應的具體`IXmlDecryptor`實作。

型別，能夠實作`IXmlDecryptor`應該有下列兩個公用建構函式的其中一個：

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> `IServiceProvider`傳遞至建構函式可能是 null。

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

`IKeyEscrowSink`介面代表可執行的機密資訊的委付的型別。 前文提過，序列化的項目中可能包含機密資訊 （例如密碼編譯的內容），而這就是導致引進[IXmlEncryptor](#ixmlencryptor)一開始輸入。 不過，無處不在而金鑰環可以刪除或損毀。

委付介面提供的緊急出口，允許存取原始的序列化 XML，它由任何設定轉換前[IXmlEncryptor](#ixmlencryptor)。 此介面會公開單一的 API:

* 存放區 （XElement 項目中的 Guid keyId）

最多是`IKeyEscrowSink`實作，以處理提供的項目，以安全的方式與商務原則一致。 一個可能的實作可能有無法委付接收加密使用已知的公司 X.509 憑證的 XML 項目，其中憑證的私密金鑰已委付;`CertificateXmlEncryptor`可以協助進行此設定類型。 `IKeyEscrowSink`實作也會負責適當地保存提供的項目。

預設沒有委付機制會啟用，但伺服器系統管理員可以[全域設定此](xref:security/data-protection/configuration/machine-wide-policy)。 它也可以設定以程式設計方式透過`IDataProtectionBuilder.AddKeyEscrowSink`方法，如下列範例所示。 `AddKeyEscrowSink`方法多載鏡像`IServiceCollection.AddSingleton`並`IServiceCollection.AddInstance`多載，為`IKeyEscrowSink`主要執行個體做為單一子句。 如果有多個`IKeyEscrowSink`註冊執行個體，因此索引鍵可以委付至多個機制同時，將金鑰在產生期間，呼叫每一個。

沒有任何 API，以讀取來自資料`IKeyEscrowSink`執行個體。 這是設計理論的委付機制的一致： 其目的是讓金鑰的內容是受信任的授權單位，可存取，因為應用程式本身不是受信任的授權單位，就不能存取它自己委付的內容。

下列範例程式碼示範如何建立和註冊`IKeyEscrowSink`其中，只有 「 CONTOSODomain 系統管理員 」 的成員可以復原已委付金鑰。

> [!NOTE]
> 若要執行此範例中，您必須在已加入網域的 Windows 8 / Windows Server 2012 電腦和網域控制站必須是 Windows Server 2012 或更新版本。

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
