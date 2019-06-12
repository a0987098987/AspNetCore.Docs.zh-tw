---
title: ASP.NET Core 的取用者 Api 概觀
author: rick-anderson
description: 收到不同的取用者 Api ASP.NET Core 資料保護的文件庫中可用的簡短概觀。
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: ff9badb55813cae0aa72d3a95dc53792332f109b
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837372"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>ASP.NET Core 的取用者 Api 概觀

`IDataProtectionProvider`和`IDataProtector`介面是透過此取用者會使用資料保護系統的基本介面。 它們位於[Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/)封裝。

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

提供者介面代表資料保護系統的根目錄。 它不能直接用於保護或解除保護資料。 相反地，取用者必須取得的參考`IDataProtector`藉由呼叫`IDataProtectionProvider.CreateProtector(purpose)`，其中的目的是描述預定的取用者使用案例的字串。 請參閱[目的字串](xref:security/data-protection/consumer-apis/purpose-strings)為目標的這個參數，以及如何選擇適當的值更多的資訊。

## <a name="idataprotector"></a>IDataProtector

保護裝置介面由呼叫`CreateProtector`，和它的取用者可以用來執行此介面保護，且取消保護作業。

若要保護的資料，將資料傳遞至`Protect`方法。 基本的介面會定義哪些將 byte []]-> [byte []，方法，但沒有也的多載 （提供擴充方法） 將字串轉換-> 字串。 提供兩種方法的安全性是相同的。開發人員應選擇何種多載是最方便其使用狀況。 不論的多載選擇，保護所傳回的值為何方法受到保護 （已譯成密碼和竄改已經過證明），和應用程式可以將它傳送至不受信任的用戶端。

若要解除保護先前受保護資料片段，將傳遞至受保護的資料`Unprotect`方法。 (有 byte []-基礎和以字串為基礎的多載，開發人員為了方便起見。)如果受保護的內容由先前呼叫產生`Protect`在此相同`IDataProtector`，則`Unprotect`方法會傳回原始未受保護的內容。 如果受保護的內容已遭竄改，或者由不同`IDataProtector`，則`Unprotect`方法會擲回 CryptographicException。

與不同的相同概念`IDataProtector`繫結回用途的概念。 如果兩個`IDataProtector`從相同的根產生執行個體`IDataProtectionProvider`透過不同的用途的呼叫中的字串，但`IDataProtectionProvider.CreateProtector`，則被視為[不同的保護裝置](xref:security/data-protection/consumer-apis/purpose-strings)，而且其中一個無法取消保護產生其他的承載。

## <a name="consuming-these-interfaces"></a>使用這些介面

如需 DI 感知的元件，預定的使用方式是元件會`IDataProtectionProvider`其建構函式中的參數和元件具現化時，DI 系統會自動提供這項服務。

> [!NOTE]
> 有些應用程式 （例如主控台應用程式或 ASP.NET 4.x 應用程式） 可能無法 DI 感知因此這裡不能使用所述的機制。 如這些案例，請參閱[非 DI 感知案例](xref:security/data-protection/configuration/non-di-scenarios)如需取得的執行個體的詳細資訊的文件`IDataProtection`不透過 DI 提供者。

下列範例將示範三個概念：

1. [新增資料保護系統](xref:security/data-protection/configuration/overview)至服務容器，

2. 若要接收的執行個體使用 DI `IDataProtectionProvider`，及

3. 建立`IDataProtector`從`IDataProtectionProvider`並用它來保護且取消保護資料。

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

封裝 Microsoft.AspNetCore.DataProtection.Abstractions 包含擴充方法`IServiceProvider.GetDataProtector`開發人員為了方便起見。 它會封裝為單一作業這兩個擷取`IDataProtectionProvider`為服務提供者並呼叫從`IDataProtectionProvider.CreateProtector`。 下列範例會示範其使用方式。

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> 執行個體`IDataProtectionProvider`和`IDataProtector`是安全執行緒的多個呼叫端。 其目的是，一旦元件取得的參考`IDataProtector`透過呼叫`CreateProtector`，它會使用該參考的多個呼叫`Protect`和`Unprotect`。 呼叫`Unprotect`將會擲回 CryptographicException，如果無法驗證或用來解密受保護的內容。 某些元件可能會想要忽略錯誤期間取消保護作業;讀取驗證 cookie 的元件可能會處理此錯誤和處理要求，如同它有完全沒有 cookie 而非直接讓要求失敗。 想要此行為元件特別應該攔截 CryptographicException，而不是抑制所有的例外狀況。
