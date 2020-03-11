---
title: ASP.NET Core 中的金鑰管理擴充性
author: rick-anderson
description: 深入瞭解 ASP.NET Core 資料保護金鑰管理擴充性。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665876"
---
# <a name="key-management-extensibility-in-aspnet-core"></a>ASP.NET Core 中的金鑰管理擴充性

> [!TIP]
> 閱讀本節之前，請先閱讀[金鑰管理](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)一節，因為它會說明這些 api 背後的一些基本概念。

> [!WARNING]
> 實作為下列任何介面的型別應該是多個呼叫端的安全線程。

## <a name="key"></a>Key

`IKey` 介面是 cryptosystem 中索引鍵的基本標記法。 這裡的關鍵字是用在抽象概念中，而不是「密碼編譯金鑰內容」的常值意義。 金鑰具有下列屬性：

* 啟用、建立和到期日期

* 撤銷狀態

* 金鑰識別碼（GUID）

::: moniker range=">= aspnetcore-2.0"

此外，`IKey` 會公開 `CreateEncryptor` 方法，可用來建立與此索引鍵系結的[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)實例。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

此外，`IKey` 會公開 `CreateEncryptorInstance` 方法，可用來建立與此索引鍵系結的[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)實例。

::: moniker-end

> [!NOTE]
> 沒有 API 可從 `IKey` 實例中抓取未經處理的密碼編譯內容。

## <a name="ikeymanager"></a>IKeyManager

`IKeyManager` 介面代表負責一般金鑰儲存、抓取和操作的物件。 它會公開三個高階作業：

* 建立新的金鑰，並將它保存在儲存體中。

* 從儲存體取得所有金鑰。

* 撤銷一或多個金鑰，並將撤銷資訊保存到儲存體。

>[!WARNING]
> 撰寫 `IKeyManager` 是一項非常先進的工作，大部分的開發人員都不應該嘗試這麼做。 相反地，大部分的開發人員都應該利用[XmlKeyManager](#xmlkeymanager)類別所提供的功能。

## <a name="xmlkeymanager"></a>XmlKeyManager

`XmlKeyManager` 類型是 `IKeyManager`的內建實體執行。 它提供數個實用的功能，包括待用金鑰的金鑰委付和加密。 此系統中的索引鍵會以 XML 元素（具體而言是[system.xml.linq.xelement>](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)）來表示。

`XmlKeyManager` 相依于完成其工作的過程中的數個其他元件：

::: moniker range=">= aspnetcore-2.0"

* `AlgorithmConfiguration`，其規定新索引鍵所使用的演算法。

* `IXmlRepository`，控制金鑰在儲存體中的保存位置。

* `IXmlEncryptor` [選用]，允許待用加密金鑰。

* `IKeyEscrowSink` [選用]，提供金鑰委付服務。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`，控制金鑰在儲存體中的保存位置。

* `IXmlEncryptor` [選用]，允許待用加密金鑰。

* `IKeyEscrowSink` [選用]，提供金鑰委付服務。

::: moniker-end

以下是高階圖表，指出如何在 `XmlKeyManager`內將這些元件連結在一起。

::: moniker range=">= aspnetcore-2.0"

![建立金鑰](key-management/_static/keycreation2.png)

*金鑰建立/CreateNewKey*

在 `CreateNewKey`的執行中，`AlgorithmConfiguration` 元件會用來建立唯一的 `IAuthenticatedEncryptorDescriptor`，然後再序列化為 XML。 如果有金鑰委付接收，則會提供原始（未加密）的 XML 給接收以進行長期儲存。 未加密的 XML 接著會透過 `IXmlEncryptor` （如有必要）執行，以產生加密的 XML 檔。 此加密檔會透過 `IXmlRepository`保存到長期儲存體。 （如果未設定任何 `IXmlEncryptor`，未加密的檔會保存在 `IXmlRepository`中）。

![金鑰抓取](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![建立金鑰](key-management/_static/keycreation1.png)

*金鑰建立/CreateNewKey*

在 `CreateNewKey`的執行中，`IAuthenticatedEncryptorConfiguration` 元件會用來建立唯一的 `IAuthenticatedEncryptorDescriptor`，然後再序列化為 XML。 如果有金鑰委付接收，則會提供原始（未加密）的 XML 給接收以進行長期儲存。 未加密的 XML 接著會透過 `IXmlEncryptor` （如有必要）執行，以產生加密的 XML 檔。 此加密檔會透過 `IXmlRepository`保存到長期儲存體。 （如果未設定任何 `IXmlEncryptor`，未加密的檔會保存在 `IXmlRepository`中）。

![金鑰抓取](key-management/_static/keyretrieval1.png)

::: moniker-end

*金鑰抓取/GetAllKeys*

在 `GetAllKeys`的執行中，會從基礎 `IXmlRepository`讀取代表索引鍵和撤銷的 XML 檔。 如果這些檔已加密，系統將會自動將其解密。 `XmlKeyManager` 會建立適當的 `IAuthenticatedEncryptorDescriptorDeserializer` 實例，將檔案還原序列化回 `IAuthenticatedEncryptorDescriptor` 實例中，然後再包裝在個別的 `IKey` 實例中。 這個 `IKey` 實例的集合會傳回給呼叫者。

如需特定 XML 元素的進一步資訊，請參閱[金鑰儲存體格式檔](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format)。

## <a name="ixmlrepository"></a>IXmlRepository

`IXmlRepository` 介面代表可以保存 XML 並從備份存放區抓取 XML 的類型。 它會公開兩個 Api：

* `GetAllElements`：`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

`IXmlRepository` 的執行不需要剖析傳遞的 XML。 它們應該將 XML 檔視為不透明，並讓較高層的使用者擔心產生和剖析檔。

有四個內建的具象類型，可實作為 `IXmlRepository`：

::: moniker range=">= aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage. AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage. AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

如需詳細資訊，請參閱[金鑰儲存提供者檔](xref:security/data-protection/implementation/key-storage-providers)。

使用不同的備份存放區（例如 Azure 表格儲存體）時，註冊自訂 `IXmlRepository` 是適當的。

若要變更預設的儲存機制應用程式範圍，請註冊自訂的 `IXmlRepository` 實例：

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

`IXmlEncryptor` 介面代表可以加密純文字 XML 元素的類型。 它會公開單一 API：

* Encrypt （System.xml.linq.xelement> plaiNtextElement）： EncryptedXmlInfo

如果序列化 `IAuthenticatedEncryptorDescriptor` 包含標記為「需要加密」的任何元素，則 `XmlKeyManager` 會透過所設定 `IXmlEncryptor`的 `Encrypt` 方法來執行這些專案，而它會將 enciphered 專案（而不是純文字元素）保存至 `IXmlRepository`。 `Encrypt` 方法的輸出是 `EncryptedXmlInfo` 物件。 這個物件是一個包裝函式，其中同時包含結果 enciphered `XElement` 和類型，其代表可用來解密對應元素的 `IXmlDecryptor`。

有四個內建的具象類型，可實作為 `IXmlEncryptor`：

* [CertificateXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [DpapiNGXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [DpapiXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [NullXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

如需詳細資訊，請參閱待用[金鑰加密檔](xref:security/data-protection/implementation/key-encryption-at-rest)。

若要變更整個應用程式的預設金鑰加密機制，請註冊自訂的 `IXmlEncryptor` 實例：

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

`IXmlDecryptor` 介面代表的類型，知道如何解密透過 `IXmlEncryptor`enciphered 的 `XElement`。 它會公開單一 API：

* 解密（System.xml.linq.xelement> encryptedElement）： System.xml.linq.xelement>

`Decrypt` 方法會復原 `IXmlEncryptor.Encrypt`所執行的加密。 一般來說，每個具體 `IXmlEncryptor` 的執行都會有對應的具體 `IXmlDecryptor` 執行。

實 `IXmlDecryptor` 的型別應該具有下列兩個公用函式的其中一個：

* .ctor （IServiceProvider）
* .ctor （）

> [!NOTE]
> 傳遞至此函式的 `IServiceProvider` 可能是 null。

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

`IKeyEscrowSink` 介面代表可執行機密資訊的類型。 回想一下，序列化的描述項可能包含機密資訊（例如密碼編譯內容），而這就是第一次引進[IXmlEncryptor](#ixmlencryptor)類型的地方。 不過，發生事故，而且可以刪除或損毀金鑰環。

「證書處理」介面提供緊急的 escape 影線，允許在任何已設定的[IXmlEncryptor](#ixmlencryptor)轉換原始序列化的 XML 之前，先對其進行存取。 介面會公開單一 API：

* Store （Guid keyId，System.xml.linq.xelement> 元素）

以安全的方式處理提供的專案是以與商務原則一致的方法來完成。 `IKeyEscrowSink` 使用已知的公司 x.509 憑證（其中憑證的私密金鑰已委付）時，可能的執行方式之一是`CertificateXmlEncryptor` 類型可以協助這種情況。 `IKeyEscrowSink` 的執行也會負責適當保存提供的元素。

預設不會啟用任何證書機制，雖然伺服器管理員可以[全域進行設定](xref:security/data-protection/configuration/machine-wide-policy)。 也可以透過 `IDataProtectionBuilder.AddKeyEscrowSink` 方法以程式設計方式進行設定，如下列範例所示。 `AddKeyEscrowSink` 的方法多載會鏡像 `IServiceCollection.AddSingleton` 和 `IServiceCollection.AddInstance` 多載，因為 `IKeyEscrowSink` 實例會單次個體。 如果註冊了多個 `IKeyEscrowSink` 實例，則會在金鑰產生期間呼叫每一個實例，因此可以同時將金鑰委付到多個機制。

沒有 API 可從 `IKeyEscrowSink` 實例讀取材質。 這與證書處理機制的設計理論一致：其目的是要讓受信任的授權單位能夠存取金鑰材料，而且因為應用程式本身不是受信任的授權單位，所以不應該存取自己的委付資料。

下列範例程式碼示範如何建立和註冊 `IKeyEscrowSink`，其中委付金鑰，讓只有 "CONTOSODomain Admins" 的成員可以復原它們。

> [!NOTE]
> 若要執行此範例，您必須位於已加入網域的 Windows 8/Windows Server 2012 電腦，且網域控制站必須是 Windows Server 2012 或更新版本。

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
