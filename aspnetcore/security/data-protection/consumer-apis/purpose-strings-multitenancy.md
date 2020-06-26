---
title: ASP.NET Core 中的目的階層和多租使用者
author: rick-anderson
description: 瞭解目的字串階層和多租使用者，因為它與 ASP.NET Core 資料保護 Api 相關。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: 8f069da500e7bc06e4b8712fbf7b86d90a815758
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85404375"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>ASP.NET Core 中的目的階層和多租使用者

由於 `IDataProtector` 也是隱含的 `IDataProtectionProvider` ，因此可以將目的連結在一起。 就這一點而言， `provider.CreateProtector([ "purpose1", "purpose2" ])` 相當於 `provider.CreateProtector("purpose1").CreateProtector("purpose2")` 。

這可讓您透過資料保護系統來進行一些有趣的階層式關聯性。 在先前的[SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose)範例中，SecureMessage 元件可以 `provider.CreateProtector("Contoso.Messaging.SecureMessage")` 事先呼叫一次，並將結果快取至私用 `_myProvider` 欄位。 接著，您可以透過對的呼叫來建立未來的保護裝置 `_myProvider.CreateProtector("User: username")` ，並使用這些保護裝置來保護個別訊息。

這也可以翻轉。 假設有一個裝載多個租使用者的單一邏輯應用程式（CMS 看似合理），而且每個租使用者都可以使用自己的驗證和狀態管理系統來設定。 傘應用程式具有單一主要提供者，它會呼叫 `provider.CreateProtector("Tenant 1")` 並 `provider.CreateProtector("Tenant 2")` 為每個租使用者提供自己的資料保護系統隔離配量。 然後，租使用者可以根據自己的需求來衍生自己的個別保護裝置，但不管他們嘗試如何建立與系統中任何其他租使用者相衝突的保護裝置。 以圖形方式呈現，如下所示。

![多租使用者用途](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> 這會假設您的「傘」應用程式會控制哪些 Api 可供個別租使用者使用，而且租使用者無法在伺服器上執行任意程式碼。 如果租使用者可以執行任意程式碼，他們可能會執行私用反映以中斷隔離保證，或直接讀取主要金鑰內容，並衍生所需的任何子機碼。

資料保護系統實際上會在預設的現成設定中使用一種多租使用者。 根據預設，主要金鑰材料會儲存在背景工作進程帳戶的使用者設定檔資料夾（或登錄中，適用于 IIS 應用程式集區身分識別）。 但是，使用單一帳戶來執行多個應用程式是很常見的，因此所有這些應用程式最後都會共用主要金鑰內容。 為了解決此問題，資料保護系統會自動插入每個應用程式唯一識別碼，做為整體目的鏈中的第一個元素。 這個隱含的目的是要將每個應用程式有效地視為系統內的唯一租使用者，以[隔離個別應用](xref:security/data-protection/configuration/overview#per-application-isolation)程式，而保護裝置的建立程式看起來與上圖相同。
