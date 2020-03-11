---
title: 限制 ASP.NET Core 中受保護承載的存留期
author: rick-anderson
description: 瞭解如何使用 ASP.NET Core 的資料保護 Api 來限制受保護承載的存留期。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656055"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a>限制 ASP.NET Core 中受保護承載的存留期

在某些情況下，應用程式開發人員想要建立在一段時間後過期的受保護承載。 比方說，受保護的承載可能代表密碼重設權杖，其應只在一小時內有效。 開發人員當然可以建立自己的裝載格式，其中包含內嵌的到期日，而先進的開發人員也可能想要這麼做，但對於管理這些到期的大多數開發人員來說，這種情況會變得很繁瑣。

為了讓開發人員更容易使用， [AspNetCore. DataProtection 副檔名](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)包含公用程式 api，可用於建立在一段時間後自動到期的裝載。 這些 Api 會使 `ITimeLimitedDataProtector` 類型停止回應。

## <a name="api-usage"></a>API 使用方式

`ITimeLimitedDataProtector` 介面是用來保護和解除保護時間限制/自我過期承載的核心介面。 若要建立 `ITimeLimitedDataProtector`的實例，您必須先使用以特定目的所建立的一般[idataprotector 加密](xref:security/data-protection/consumer-apis/overview)實例。 一旦 `IDataProtector` 實例可供使用，請呼叫 `IDataProtector.ToTimeLimitedDataProtector` 擴充方法，以使用內建的到期功能來取回保護裝置。

`ITimeLimitedDataProtector` 會公開下列 API 介面和擴充方法：

* CreateProtector （字串用途）： ITimeLimitedDataProtector-此 API 類似現有的 `IDataProtectionProvider.CreateProtector`，可用來從根時間限制的保護裝置建立[目的鏈](xref:security/data-protection/consumer-apis/purpose-strings)。

* 保護（byte [] 純文字，DateTimeOffset 過期）： byte []

* 保護（byte [] 純文字，TimeSpan 存留期）： byte []

* 保護（byte [] 純文字）： byte []

* 保護（字串純文字、DateTimeOffset 過期）：字串

* 保護（字串純文字，TimeSpan 存留期）：字串

* 保護（字串純文字）：字串

除了只採用純文字的核心 `Protect` 方法以外，還有新的多載可讓您指定承載的到期日。 到期日可指定為絕對日期（透過 `DateTimeOffset`）或做為相對時間（從目前的系統時間，透過 `TimeSpan`）。 如果呼叫不接受到期的多載，則會假設承載永不過期。

* 取消保護（byte [] protectedData，out DateTimeOffset 過期）： byte []

* 取消保護（byte [] protectedData）： byte []

* 取消保護（字串 protectedData，輸出 DateTimeOffset 到期）：字串

* 取消保護（字串 protectedData）：字串

`Unprotect` 方法會傳回原始未受保護的資料。 如果裝載尚未過期，則會以選擇性的 out 參數以及原始未受保護的資料來傳回絕對到期。 如果承載已過期，則取消保護方法的所有多載都會擲回 System.security.cryptography.cryptographicexception。

>[!WARNING]
> 不建議使用這些 Api 來保護需要長期或無限持續性的承載。 「我可以承受受保護的承載在一個月後永久無法復原嗎？」 可以做為良好的經驗法則;如果答案為否，則開發人員應該考慮替代 Api。

下列範例會使用[非 DI 程式碼路徑](xref:security/data-protection/configuration/non-di-scenarios)來具現化資料保護系統。 若要執行這個範例，請確定您已先加入 AspNetCore. DataProtection 封裝的參考。

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
