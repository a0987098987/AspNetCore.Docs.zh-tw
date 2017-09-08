---
title: "取用者應用程式開發介面概觀"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f69beb9d-a519-43a8-857c-f6b01886a903
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: d23a6ce50eef71f393124b9420f4ba473904d8b4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="consumer-apis-overview"></a>取用者應用程式開發介面概觀

IDataProtectionProvider 和 IDataProtector 介面是透過此取用者會使用資料保護系統的基本介面。 它們位於 Microsoft.AspNetCore.DataProtection.Abstractions 封裝中。

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

提供者介面代表資料保護系統的根。 它無法直接用於保護或解除保護資料。 相反地，取用者必須藉由呼叫的 IDataProtectionProvider.CreateProtector(purpose)，其中的目的是描述預定的取用者使用案例的字串取得 IDataProtector 的參考。 請參閱[目的字串](purpose-strings.md)更多資訊的目的，這個參數，以及如何選擇適當的值。

## <a name="idataprotector"></a>IDataProtector

保護裝置介面由呼叫 CreateProtector，而且這個介面，取用者可用來執行保護且取消保護作業。

若要保護的資料，請將資料傳遞給保護方法。 基本的介面會定義哪些將 byte []-> byte []，方法，但另外還有的多載 （提供擴充方法） 將字串轉換]-> [字串。 提供兩種方法的安全性是相同的。開發人員應選擇何種多載版本是最方便的使用大小寫。 不論的多載選擇，保護所傳回的值是方法現在受到 （enciphered 和竄改校訂），並且在應用程式可以將它傳送至不受信任的用戶端。

若要取消保護先前受保護資料片段，將受保護的資料至 Unprotect 方法。 (沒有 byte [] 為基礎和以字串為基礎的多載，方便開發人員。)如果受保護的內容所產生的早期呼叫此相同 IDataProtector 上的保護，Unprotect 方法會傳回原始未受保護的內容。 如果受保護的內容已遭竄改，或者由不同 IDataProtector，Unprotect 方法會擲回 CryptographicException。

相同的概念與不同 IDataProtector 繫結合作對回到用途的概念。 如果兩個 IDataProtector 執行個體所產生相同的根 IDataProtectionProvider 但透過不同的用途 IDataProtectionProvider.CreateProtector，呼叫中的字串，則會將它們視為[不同的保護裝置](purpose-strings.md)，而且其中一個無法取消保護其他所產生的裝載。

## <a name="consuming-these-interfaces"></a>使用這些介面

DI 感知的元件，預定使用方式是元件採取 IDataProtectionProvider 參數，在其建構函式和具現化元件時，DI 系統會自動提供此服務。

> [!NOTE]
> 有些應用程式 （例如主控台應用程式或 ASP.NET 4.x 應用程式） 可能無法 DI 感知因此這裡不能使用所述的機制。 針對這些案例參閱[非 DI 注意案例](../configuration/non-di-scenarios.md)文件，如需詳細資訊，而不需要透過 DI 取得 IDataProtection 提供者執行個體。

下列範例將示範三個概念：

1. [新增資料保護系統](../configuration/overview.md)至服務容器，

2. 若要接收的 IDataProtectionProvider，執行個體使用 DI 和

3. 建立 IDataProtector IDataProtectionProvider 並使用它來保護且取消保護資料。

[!code-csharp[Main](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

封裝 Microsoft.AspNetCore.DataProtection.Abstractions 包含擴充方法 IServiceProvider.GetDataProtector 是開發人員方便。 它會封裝成單一作業和服務提供者擷取 IDataProtectionProvider 呼叫 IDataProtectionProvider.CreateProtector。 下列範例會示範其使用方式。

[!code-csharp[Main](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> IDataProtectionProvider 和 IDataProtector 的執行個體是安全執行緒的多個呼叫端。 它可能是一旦元件取得透過呼叫 CreateProtector IDataProtector 的參考，它會使用該參考的多個呼叫保護和解除 Unprotect.A 呼叫將會擲回 CryptographicException，如果受保護的內容不能是驗證或用來解密。 某些元件可能會想要忽略的錯誤時取消保護作業;元件會讀取驗證 cookie，可能會處理這種錯誤和任何 cookie 已完全處理要求而徹底要求失敗。 元件的這個行為特別應該攔截 CryptographicException，而不是抑制所有例外狀況。
