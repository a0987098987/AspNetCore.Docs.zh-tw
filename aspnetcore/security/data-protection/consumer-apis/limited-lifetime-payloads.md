---
title: ASP.NET Core 中的受保護承載的存留期的限制
author: rick-anderson
description: 了解如何限制使用 ASP.NET Core 資料保護 Api 的受保護承載的存留期。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897115"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a>ASP.NET Core 中的受保護承載的存留期的限制

有些應用程式開發人員要建立會在一段時間後到期的受保護的承載的情況。 比方說，受保護的內容可能代表應該有效時間是一小時的密碼重設語彙基元。 當然可以建立自己裝載格式，其中包含內嵌的到期日，讓開發人員和進階開發人員可能會想要這樣做，但對於大部分的開發人員管理這些到期期限可以成長單調乏味。

為了簡化起見針對我們的開發人員對象，封裝[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)包含公用程式 Api 來建立裝載在一段時間後自動過期。 這些 Api 的停止回應登出`ITimeLimitedDataProtector`型別。

## <a name="api-usage"></a>API 使用量

`ITimeLimitedDataProtector`介面是保護和解除期限 / 即將過期自我裝載的核心介面。 若要建立的執行個體`ITimeLimitedDataProtector`，您必須先執行個體的一般[IDataProtector](xref:security/data-protection/consumer-apis/overview)建構具有特定用途。 一次`IDataProtector`執行個體可用，請呼叫`IDataProtector.ToTimeLimitedDataProtector`內建到期功能取回保護裝置的擴充方法。

`ITimeLimitedDataProtector` 會公開下列的 API 介面和擴充方法：

* CreateProtector （字串用途）：ITimeLimitedDataProtector-此 API 是類似於現有`IDataProtectionProvider.CreateProtector`在於它可以用來建立[用途鏈結](xref:security/data-protection/consumer-apis/purpose-strings)從根期限保護裝置。

* Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]

* Protect(byte[] plaintext, TimeSpan lifetime) : byte[]

* Protect(byte[] plaintext) : byte[]

* 保護 （字串純文字，DateTimeOffset 到期）： 字串

* 保護 （字串的純文字、 時間範圍存留期）： 字串

* 保護 （字串純文字）： 字串

除了核心`Protect`方法這需要僅純文字，有新的多載，可指定內容的到期日。 可以指定到期日為的絕對日期 (透過`DateTimeOffset`) 或相對的時間 (從目前系統時間，透過`TimeSpan`)。 如果呼叫多載，其不會到期，承載會假設永遠不會過期。

* Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]

* Unprotect(byte[] protectedData) : byte[]

* 取消保護 (DateTimeOffset 到期時，字串 protectedData): 字串

* 取消保護 (字串 protectedData): 字串

`Unprotect`方法會傳回原始未受保護的資料。 如果裝載尚未尚未過期，絕對期限會傳回做為選擇性的 out 參數，以及原始的未受保護資料。 如果裝載已過期，所有多載的 Unprotect 方法會擲回 CryptographicException。

>[!WARNING]
> 不建議使用這些 Api 來保護裝載需要長期或無限期的持續性。 「 我經得起受保護的承載是一個月之後的 永久無法復原的？ 」 可做為法則;如果答案是沒有然後開發人員應該考慮替代的 Api。

下列範例使用[非 DI 的程式碼路徑](xref:security/data-protection/configuration/non-di-scenarios)具現化的資料保護系統。 若要執行此範例，請確定您已先新增 Microsoft.AspNetCore.DataProtection.Extensions 套件的參考。

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
