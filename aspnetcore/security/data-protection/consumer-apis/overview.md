---
title: ASP.NET Core 的取用者 Api 總覽
author: rick-anderson
description: 取得 ASP.NET Core 資料保護程式庫中可用的各種取用者 Api 的簡要總覽。
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: ff9badb55813cae0aa72d3a95dc53792332f109b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666583"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>ASP.NET Core 的取用者 Api 總覽

`IDataProtectionProvider` 和 `IDataProtector` 介面是取用者用來使用資料保護系統的基本介面。 它們位於[AspNetCore. DataProtection. 抽象](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/)封裝中。

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

提供者介面代表資料保護系統的根。 它不能直接用來保護或解除保護資料。 相反地，取用者必須藉由呼叫 `IDataProtectionProvider.CreateProtector(purpose)`取得 `IDataProtector` 的參考，其中目的是描述預定取用者使用案例的字串。 如需此參數之意圖的詳細資訊，以及如何選擇適當的值，請參閱[目的字串](xref:security/data-protection/consumer-apis/purpose-strings)。

## <a name="idataprotector"></a>Idataprotector 加密

`CreateProtector`的呼叫會傳回保護裝置介面，而此介面可供取用者用來執行保護和取消保護作業。

若要保護資料片段，請將資料傳遞給 `Protect` 方法。 基本介面會定義轉換 byte []-> byte [] 的方法，但也有多載（提供為擴充方法），可轉換字串 > 字串。 這兩種方法所提供的安全性完全相同;開發人員應選擇最方便其使用案例的任何多載。 不論選擇的多載，保護方法所傳回的值現在都會受到保護（enciphered 和校訂），而且應用程式可以將它傳送至不受信任的用戶端。

若要將先前保護的資料片段解除保護，請將受保護的資料傳遞給 `Unprotect` 方法。 （為了方便開發人員，還有以位元組 [] 為基礎和以字串為基礎的多載）。如果受保護的承載是由先前的呼叫所產生 `Protect` 在相同的 `IDataProtector`上，則 `Unprotect` 方法會傳回原始未受保護的裝載。 如果受保護的內容已遭修改，或是由不同的 `IDataProtector`所產生，則 `Unprotect` 方法會擲回 System.security.cryptography.cryptographicexception。

相同與不同 `IDataProtector` 的概念會與目的概念相結合。 如果從相同的根 `IDataProtectionProvider` 產生兩個 `IDataProtector` 實例，但在對 `IDataProtectionProvider.CreateProtector`的呼叫中透過不同的目的字串，則會將它們視為[不同的保護](xref:security/data-protection/consumer-apis/purpose-strings)裝置，而且其中一個將無法取消保護另一個所產生的裝載。

## <a name="consuming-these-interfaces"></a>使用這些介面

對於 DI 感知元件，預期的用法是元件會在其程式中採用 `IDataProtectionProvider` 參數，而且當元件具現化時，DI 系統會自動提供此服務。

> [!NOTE]
> 某些應用程式（例如主控台應用程式或 ASP.NET 4.x 應用程式）可能不是 DI 感知，因此無法使用此處所述的機制。 針對這些案例，請參閱[非 Di 感知案例](xref:security/data-protection/configuration/non-di-scenarios)檔，以取得取得 `IDataProtection` 提供者實例而不透過 DI 的詳細資訊。

下列範例示範三個概念：

1. [將資料保護系統新增](xref:security/data-protection/configuration/overview)至服務容器，

2. 使用 DI 來接收 `IDataProtectionProvider`的實例，以及

3. 從 `IDataProtectionProvider` 建立 `IDataProtector`，並使用它來保護和解除保護資料。

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

AspNetCore 的 DataProtection 包含擴充方法，`IServiceProvider.GetDataProtector` 為開發人員的便利性。 它會封裝為單一作業，從服務提供者抓取 `IDataProtectionProvider` 並呼叫 `IDataProtectionProvider.CreateProtector`。 下列範例示範其使用方式。

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> `IDataProtectionProvider` 和 `IDataProtector` 的實例對於多個呼叫端而言都是安全線程。 這是因為一旦元件透過呼叫 `CreateProtector`取得 `IDataProtector` 的參考，它就會使用該參考進行多個 `Protect` 和 `Unprotect`的呼叫。 如果無法驗證或解密受保護的承載，`Unprotect` 的呼叫將會擲回 System.security.cryptography.cryptographicexception。 某些元件可能會想要在取消保護作業期間忽略錯誤;讀取驗證 cookie 的元件可能會處理此錯誤，並將要求視為完全沒有 cookie，而不是直接讓要求失敗。 需要此行為的元件應該特別捕捉 System.security.cryptography.cryptographicexception，而不是抑制所有例外狀況。
