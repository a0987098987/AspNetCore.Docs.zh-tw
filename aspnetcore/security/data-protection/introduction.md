---
title: ASP.NET Core 資料保護
author: rick-anderson
description: 了解資料保護的概念和 ASP.NET Core 資料保護 Api 的設計原則。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/introduction
ms.openlocfilehash: 37f170a3e8a46ef2215b0999358d46dd402636df
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089544"
---
# <a name="aspnet-core-data-protection"></a>ASP.NET Core 資料保護

Web 應用程式通常需要將機密資料。 Windows 桌面應用程式提供 DPAPI，但這並不適用於 web 應用程式。 ASP.NET Core 資料保護堆疊提供開發人員可用來保護資料，包括金鑰管理與輪替簡單、 容易使用的密碼編譯 API。

ASP.NET Core 資料保護堆疊旨在做為長期取代&lt;machineKey&gt;項目在 ASP.NET 1.x-4.x。 它的設計目標包括許多舊的密碼編譯堆疊的缺點時用於大部分的現代應用程式可能會遇到的使用案例中提供的立即可用的解決方案。

## <a name="problem-statement"></a>問題陳述

整體問題陳述式可以簡潔中所述的單一句子： 我需要保存值得信賴的資訊供日後擷取，但是我不信任的持續性機制。 Web 而言，這可能會寫入 「 我需要反覆存取受信任的狀態，透過不受信任的用戶端 」。

標準範例就是驗證 cookie 或持有人權杖。 伺服器會產生 「 我是 Groot 和 xyz 使用權限 」 權杖，並將它交付給用戶端。 在未來的某個日期，用戶端會出示該權杖至伺服器，但伺服器需要某種形式的用戶端尚未偽造語彙基元的保證。 因此第一項需求： 真確性 （也稱為 完整性、 竄改）。

伺服器信任保存的狀態，因為我們預期這個狀態可能包含作業系統的環境特有的資訊。 這可能是在表單的檔案路徑、 權限，控制代碼或其他的間接參考或某些其他伺服器特定資料片段。 這類資訊通常不應公開至不受信任的用戶端。 因此第二個需求： 機密性。

最後，由於元件化的現代化應用程式，我們已了解是個別的元件會想要利用此系統，而不考慮其他元件的系統中。 比方說，如果持有人權杖的元件會使用這個堆疊，它應該運作不受干擾，從一種防 CSRF 機制，也可能會使用相同的堆疊。 因此最終的需求： 隔離。

我們可以提供進一步的條件約束以我們的需求的範圍縮小。 我們假設加密系統內操作的所有服務都皆為同樣信任，而且資料不需要產生或取用外部我們直接控制下的服務。 此外，我們需要的作業是盡量快速，因為每個 web 服務的要求可能會經過加密系統一次以上。 這使得對稱密碼編譯非常適合我們的案例中，和我們可以將非對稱密碼編譯折扣之前如有需要請將它的時間。

## <a name="design-philosophy"></a>設計原理

我們已開始所識別的現有堆疊的問題。 一旦我們有的我們會接受問卷調查現有解決方案的全貌，並結束任何現有的方案會相當有我們要搜尋的功能。 然後，我們會設計數個指導原則為基礎的解決方案。

* 系統應該提供簡單的組態。 在理想情況下，系統會是零設定，而且開發人員可以快速展開工作。 在開發人員必須設定的特定層面 （例如金鑰的儲存機制） 的情況下，應該提供考量進行簡單的這些特定的設定。

* 提供簡單的消費者端 API。 Api 應該是容易使用的正確和不正確地使用。

* 開發人員不應該了解金鑰管理的原則。 可供選取演算法和金鑰的存留期，代替開發人員，應該處理系統。 在理想情況下，開發人員從未應有的未經處理的重要資料的存取。

* 索引鍵應在待用時可能受到保護。 系統應該找出適當的預設保護機制，並將它自動套用。

記住這些原則中，我們開發的簡單[易用](xref:security/data-protection/using-data-protection)資料保護堆疊。

無限期的持續性的機密承載不主要是 ASP.NET Core 資料保護 Api。 等其他技術[Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)並[Azure Rights Management](/rights-management/)更適合的案例是無限制的儲存體，而且必須跟著強式金鑰管理功能。 話雖如此，沒有任何禁止開發人員使用 ASP.NET Core 資料保護 Api 進行長期保護的機密資料。

## <a name="audience"></a>適用對象

資料保護系統分為五個主要的套件。 這些 Api 的各個層面目標三個主要的對象;

1. [取用者 Api 概觀](xref:security/data-protection/consumer-apis/overview)目標應用程式和架構開發人員。

   「 我不想了解有關堆疊的運作方式或在設定的方式。 我只想要執行中一樣簡單的某些作業的方式盡可能使用機率很高的成功地使用 Api。 」

2. [組態 Api](xref:security/data-protection/configuration/overview)目標應用程式開發人員和系統管理員。

   「 我需要告訴我的環境需要非預設路徑或設定值的資料保護系統 」。

3. 擴充性 Api 目標開發人員負責執行自訂的原則。 這些 api 的使用方式可能會限制為罕見的情況下，而且發生，安全性感知的開發人員。

   「 我需要更換系統內整個元件，因為我有真正獨一無二的行為需求。 我想學習不常用使用的 API 介面的組件，才能建立外掛程式，可滿足我的需求。 」

## <a name="package-layout"></a>套件配置

資料保護堆疊是由五個套件所組成。

* [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/)包含<xref:Microsoft.AspNetCore.DataProtection.IDataProtectionProvider>和<xref:Microsoft.AspNetCore.DataProtection.IDataProtector>来建立的資料保護服務介面。 它也包含實用的擴充方法使用這些型別 (例如[IDataProtector.Protect](xref:Microsoft.AspNetCore.DataProtection.DataProtectionCommonExtensions.Protect*))。 如果資料保護系統具現化在其他地方，而且您正在使用的 API，參考`Microsoft.AspNetCore.DataProtection.Abstractions`。

* [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/)包含資料保護系統，包括核心密碼編譯作業、 金鑰管理、 組態和擴充性的核心實作。 具現化的資料保護系統 (例如，將它加入至<xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>)，或是修改或擴充其行為，參考`Microsoft.AspNetCore.DataProtection`。

* [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)包含開發人員認為非常有用，但它們不屬於核心封裝中的其他 Api。 比方說，此套件包含 factory 方法，來具現化的資料保護系統上沒有相依性插入的檔案系統儲存位置的索引鍵 (請參閱<xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>)。 它也包含擴充方法，來限制受保護承載的存留期 (請參閱<xref:Microsoft.AspNetCore.DataProtection.ITimeLimitedDataProtector>)。

* [Microsoft.AspNetCore.DataProtection.SystemWeb](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.SystemWeb/)可以安裝到現有的 ASP.NET 4.x 應用程式重新導向其`<machineKey>`作業，以使用新的 ASP.NET Core 資料保護堆疊。 如需詳細資訊，請參閱<xref:security/data-protection/compatibility/replacing-machinekey>。

* [Microsoft.AspNetCore.Cryptography.KeyDerivation](https://www.nuget.org/packages/Microsoft.AspNetCore.Cryptography.KeyDerivation/)提供 PBKDF2 密碼雜湊常式的實作，而且可供必須安全地處理使用者密碼的系統。 如需詳細資訊，請參閱<xref:security/data-protection/consumer-apis/password-hashing>。

## <a name="additional-resources"></a>其他資源

<xref:host-and-deploy/web-farm>
