---
title: "取用者應用程式開發介面概觀"
author: rick-anderson
description: "本文件提供各種消費者應用程式開發介面使用 ASP.NET Core 資料保護程式庫內的簡短概觀。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: 3aa0c4bc8d009147dd15571da4d7d63402e4c512
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="consumer-apis-overview"></a>取用者應用程式開發介面概觀

`IDataProtectionProvider`和`IDataProtector`介面是透過此取用者會使用資料保護系統的基本介面。 它們位於[Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/)封裝。

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

提供者介面代表資料保護系統的根。 它無法直接用於保護或解除保護資料。 相反地，取用者必須取得的參考`IDataProtector`藉由呼叫`IDataProtectionProvider.CreateProtector(purpose)`，其中的目的是描述預定的取用者使用案例的字串。 請參閱[目的字串](purpose-strings.md)更多資訊的目的，這個參數，以及如何選擇適當的值。

## <a name="idataprotector"></a>IDataProtector

保護裝置介面由呼叫`CreateProtector`，和它的取用者可以用來執行此介面保護且取消保護作業。

若要保護的資料，將資料傳遞給`Protect`方法。 基本的介面會定義哪些將 byte []-> byte []，方法，但沒有多載 （提供擴充方法） 將字串轉換也-> 字串。 提供兩種方法的安全性是相同的。開發人員應選擇何種多載版本是最方便的使用大小寫。 不論的多載選擇，保護所傳回的值是方法現在受到 （enciphered 和竄改校訂），並且在應用程式可以將它傳送至不受信任的用戶端。

若要取消保護先前受保護資料片段，將傳遞至受保護的資料`Unprotect`方法。 (沒有 byte [] 為基礎和以字串為基礎的多載，方便開發人員。)如果受保護的內容由先前呼叫所產生`Protect`此相同`IDataProtector`、`Unprotect`方法會傳回原始未受保護的內容。 如果受保護的內容已遭竄改，或者由不同`IDataProtector`、`Unprotect`方法會擲回 CryptographicException。

相同的概念與不同`IDataProtector`ties 回用途的概念。 如果兩個`IDataProtector`從相同的根產生執行個體`IDataProtectionProvider`透過不同的用途的呼叫中的字串，但`IDataProtectionProvider.CreateProtector`，則它們視為[不同的保護裝置](purpose-strings.md)，其中一個將無法取消保護其他產生的裝載。

## <a name="consuming-these-interfaces"></a>使用這些介面

DI 感知的元件，預定使用方式是元件採取`IDataProtectionProvider`其建構函式中的參數，並具現化元件時，DI 系統會自動提供此服務。

> [!NOTE]
> 有些應用程式 （例如主控台應用程式或 ASP.NET 4.x 應用程式） 可能無法 DI 感知因此這裡不能使用所述的機制。 針對這些案例參閱[非 DI 注意案例](../configuration/non-di-scenarios.md)文件以取得執行個體的詳細資訊`IDataProtection`而不需要透過 DI 的提供者。

下列範例將示範三個概念：

1. [新增資料保護系統](../configuration/overview.md)至服務容器，

2. 若要接收的執行個體使用 DI `IDataProtectionProvider`，和

3. 建立`IDataProtector`從`IDataProtectionProvider`並使用它來保護且取消保護資料。

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

封裝 Microsoft.AspNetCore.DataProtection.Abstractions 包含的擴充方法`IServiceProvider.GetDataProtector`是開發人員方便。 它是以單一作業封裝這兩個擷取`IDataProtectionProvider`為服務提供者，並呼叫從`IDataProtectionProvider.CreateProtector`。 下列範例會示範其使用方式。

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> 執行個體`IDataProtectionProvider`和`IDataProtector`是多個呼叫的執行緒安全。 它具有一旦元件取得的參考用的`IDataProtector`透過呼叫`CreateProtector`，它會使用該參考的多個呼叫`Protect`和`Unprotect`。 呼叫`Unprotect`將會擲回 CryptographicException，如果無法驗證或是用來解密受保護的內容。 某些元件可能會想要忽略的錯誤時取消保護作業;元件會讀取驗證 cookie，可能會處理這種錯誤和任何 cookie 已完全處理要求而徹底要求失敗。 元件的這個行為特別應該攔截 CryptographicException，而不是抑制所有例外狀況。
