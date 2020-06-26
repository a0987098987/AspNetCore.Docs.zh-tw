---
title: ASP.NET Core 資料保護
author: rick-anderson
description: 瞭解資料保護的概念，以及 ASP.NET Core 資料保護 Api 的設計原則。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/introduction
ms.openlocfilehash: 60cf659c720012d05bb2a6f1433c18d347469462
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85399526"
---
# <a name="aspnet-core-data-protection"></a>ASP.NET Core 資料保護

Web 應用程式通常需要儲存安全性敏感的資料。 Windows 為傳統型應用程式提供 DPAPI，但這不適用於 web 應用程式。 ASP.NET Core 資料保護堆疊提供簡單、容易使用的密碼編譯 API，開發人員可用來保護資料，包括金鑰管理和輪替。

ASP.NET Core 資料保護堆疊的設計，是用來做為 &lt; ASP.NET 1.x-4.x 中 machineKey 元素的長期取代 &gt; 。 其設計目的是要解決舊版密碼編譯堆疊的許多缺點，同時為現代應用程式可能遇到的大多數使用案例提供現成的解決方案。

## <a name="problem-statement"></a>問題陳述

整體問題聲明可以在單一句子中簡潔地陳述：我需要保存信任的資訊以供日後抓取，但我不信任持續性機制。 在 web 詞彙中，這可能會撰寫為「我需要透過不受信任的用戶端來回存取受信任的狀態」。

這是驗證 cookie 或持有人權杖的標準範例。 伺服器會產生「我的 Groot，並擁有 xyz 許可權」權杖，並將其交給用戶端。 在未來的某個日期，用戶端會將該權杖呈現給伺服器，但伺服器需要某種程度的保證，讓用戶端無法偽造權杖。 因此，第一項需求：真實性（也稱為 完整性、篡改檢查）。

由於伺服器會信任保存的狀態，因此我們預期此狀態可能包含作業環境的特定資訊。 這可能是檔案路徑的形式、許可權、控制碼或其他間接參考，或是伺服器特定資料的其他部分。 這類資訊通常不會洩漏給不受信任的用戶端。 因此第二個需求：機密性。

最後，由於現代化應用程式是元件化的，我們看到的是個別元件會想要利用此系統，而不考慮系統中的其他元件。 例如，如果持有人權杖元件使用此堆疊，它應該不會干擾可能也會使用相同堆疊的防 CSRF 機制。 因此，最終的需求如下：隔離。

我們可以提供進一步的條件約束，以便縮小需求的範圍。 我們假設在 cryptosystem 內運作的所有服務都同樣受到信任，而且不需要在直接控制下的服務以外產生或使用資料。 此外，因為每個 web 服務要求可能會經歷 cryptosystem 一次或多次，所以我們需要盡可能快速地執行作業。 這讓對稱式密碼編譯非常適合我們的案例，我們可以在需要的時間之前，為非對稱式密碼編譯提供折扣。

## <a name="design-philosophy"></a>設計原理

我們從找出現有堆疊的問題開始。 一旦這麼做，我們會對現有解決方案的發展進行調查，並結束現有的解決方案並不具備我們所尋求的功能。 然後，我們會根據數個指導原則來設計解決方案。

* 系統應該提供簡單的設定。 在理想的情況下，系統會是零設定，而開發人員可能會遇到執行的基礎。 在開發人員需要設定特定方面（例如金鑰存放庫）的情況下，應提供考慮，讓這些特定設定變得簡單。

* 提供簡單取用者面向的 API。 Api 應該很容易使用，而且很難正確使用。

* 開發人員不應該學習金鑰管理原則。 系統應代表開發人員處理演算法選取和金鑰存留期。 在理想的情況下，開發人員永遠都不能存取未經處理的金鑰內容。

* 金鑰應盡可能在待用時受到保護。 系統應找出適當的預設保護機制，並自動套用。

考慮這些原則之後，我們開發了簡單、[容易使用](xref:security/data-protection/using-data-protection)的資料保護堆疊。

ASP.NET Core 的資料保護 Api 主要不適用於機密承載的無限持續性。 其他技術（如[WINDOWS CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)和[Azure Rights Management](/rights-management/) ）更適用于不限數量的儲存體案例，而且它們已有更強的金鑰管理功能。 話雖如此，開發人員也不會使用 ASP.NET Core 的資料保護 Api 來長期保護機密資料。

## <a name="audience"></a>適用對象

資料保護系統分成五個主要的封裝。 這些 Api 的各個層面都是以三個主要的物件為目標;

1. 取用[者 Api 概述](xref:security/data-protection/consumer-apis/overview)目標應用程式和架構開發人員。

   「我不想要瞭解堆疊的運作方式，或是它的設定方式。 我只是想要以簡單的方式在中執行一些作業，以成功使用 Api 的機率很高。」

2. 設定[api](xref:security/data-protection/configuration/overview)會以應用程式開發人員和系統管理員為目標。

   「我需要告訴資料保護系統，我的環境需要非預設的路徑或設定。」

3. 擴充性 Api 是以開發人員為目標，負責執行自訂原則。 這些 Api 的使用方式僅限於少數情況，以及經驗豐富的安全性感知開發人員。

   「我需要更換系統內的整個元件，因為我有真正獨特的行為需求。 我願意學習 API 介面的不常用使用部分，以建立可滿足我的需求的外掛程式。」

## <a name="package-layout"></a>封裝版面配置

資料保護堆疊是由五個封裝所組成。

* [AspNetCore. DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/)包含 <xref:Microsoft.AspNetCore.DataProtection.IDataProtectionProvider> <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> 建立資料保護服務的和介面。 它也包含有用的擴充方法來使用這些類型（例如， [idataprotector 加密](xref:Microsoft.AspNetCore.DataProtection.DataProtectionCommonExtensions.Protect*)）。 如果資料保護系統在別處具現化，而且您正在使用 API，請參考 `Microsoft.AspNetCore.DataProtection.Abstractions` 。

* [AspNetCore. DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/)包含資料保護系統的核心實行，包括核心密碼編譯作業、金鑰管理、設定和擴充性。 若要具現化資料保護系統（例如，將其新增至 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> ），或修改或擴充其行為，請參考 `Microsoft.AspNetCore.DataProtection` 。

* [AspNetCore. DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)包含額外的 api，開發人員可能會發現有用但不屬於核心套件。 例如，此套件包含 factory 方法，可將資料保護系統具現化，以將金鑰儲存在檔案系統上的位置，而不需要相依性插入（請參閱 <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider> ）。 它也包含限制受保護裝載之存留期的擴充方法（請參閱 <xref:Microsoft.AspNetCore.DataProtection.ITimeLimitedDataProtector> ）。

* [Microsoft.AspNetCore.DataProtection.SystemWeb](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.SystemWeb/)可以安裝在現有的 ASP.NET 4.x 應用程式中，以將其 `<machineKey>` 作業重新導向至使用新的 ASP.NET Core 資料保護堆疊。 如需詳細資訊，請參閱 <xref:security/data-protection/compatibility/replacing-machinekey> 。

* [AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Cryptography.KeyDerivation/)會提供 PBKDF2 密碼雜湊常式的執行，並可供必須安全處理使用者密碼的系統使用。 如需詳細資訊，請參閱 <xref:security/data-protection/consumer-apis/password-hashing> 。

## <a name="additional-resources"></a>其他資源

<xref:host-and-deploy/web-farm>
