---
title: "資料保護簡介"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4542cd37-b47c-454c-be19-d1b5810d67fe
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/introduction
ms.openlocfilehash: b7391fffd5d512c01af5d709755a925f739b59ba
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-data-protection"></a>資料保護簡介

Web 應用程式通常需要儲存機密資料。 Windows 桌面應用程式提供 DPAPI，但這並不適合 web 應用程式。 ASP.NET Core 資料保護堆疊提供簡單、 更容易使用密碼編譯 API 為開發人員可用來保護資料，包括金鑰管理和旋轉。

ASP.NET Core 資料保護堆疊設計來做為長期取代<machineKey>ASP.NET 中的項目 1.x-4.x。 它被設計成同時提供適用於大部分的現代應用程式都可能會遇到的使用案例的全新解決方案可以提供許多舊的密碼編譯堆疊的缺點。

## <a name="problem-statement"></a>問題陳述式

整體問題陳述式可以簡潔說明中的單一句子： 我需要保存受信任的資訊供日後擷取，但不是信任的持續性機制。 在 web 詞彙中，這可能會寫入如 「 我需要透過不受信任的用戶端的來回行程信任狀態 」。

標準範例是驗證 cookie 或持有人權杖。 伺服器會產生 「 我 Groot 而且的使用權限 xyz"語彙基元，並將它交付給用戶端。 在未來的某個日期，用戶端會出示該權杖至伺服器，但伺服器需要某種保證用戶端尚未偽造 token。 因此第一項需求： 真實性 （也稱為 完整性、 竄改）。

因為伺服器所信任的保存的狀態，我們預期此狀態可能包含作業系統的環境特有的資訊。 這可能是格式的檔案路徑、 權限，控制代碼或其他的間接參考，或其他一些伺服器特定資料。 這類資訊應該通常不會透露給未受信任的用戶端。 因此第二項需求： 機密性。

最後，在現代應用程式元件化，因為我們已經看到是個別的元件會想要在系統中充分利用此系統，而不考慮其他元件。 比方說，如果承載語彙基元的元件會使用此堆疊，它應該作業不受干擾，從一種反 CSRF 機制，也可能使用相同的堆疊。 因此最後的需求： 隔離。

我們可以提供進一步的條件約束以縮小範圍我們的需求。 我們假設內加密系統都運作的所有服務都都同樣地受到信任，並產生或取用外部我們直接控制下的服務不需要的資料。 此外，我們需要作業會盡快，因為每個要求至 web 服務可能會加密系統都有一或多次。 這會使得對稱式密碼編譯適合我們的案例中，與我們可以折扣非對稱密碼編譯之前如有需要的時間。

## <a name="design-philosophy"></a>設計原理

我們已開始藉由識別現有的堆疊中的問題。 一旦我們必須的我們會接受問卷調查現有解決方案的橫向，卻沒有現有的方案會相當有我們要在其中搜尋的功能。 然後，我們工程數個指導原則為基礎的解決方案。

* 系統應該提供簡化的組態。 在理想情況下會是零組態系統和開發人員無法叫用地面執行。 在開發人員必須設定的特定層面 （例如金鑰的儲存機制） 的情況下，應該給予考量進行簡單的這些特定的設定。

* 提供一個簡單的消費者導向 API。 應用程式開發介面應該可以輕鬆地使用正確的而且難以使用方式不正確。

* 開發人員不應該了解金鑰管理原則。 系統應該處理演算法選取項目和開發人員的代表金鑰存留期。 在理想情況下開發人員應該永遠不會甚至需要存取原始的金鑰內容。

* 索引鍵都應該在靜止時可能受到保護。 系統應該找出適當的預設保護機制，並將它自動套用。

這些原則，請注意，我們開發簡單、[易用](using-data-protection.md)資料保護堆疊。

ASP.NET Core 資料保護 Api 主要被不適用於機密裝載的無限期持續性。 其他技術喜歡[Windows CNG api，DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)和[Azure Rights Management](https://docs.microsoft.com/rights-management/)更適合的案例是無限期的儲存體，而且必須跟著強式金鑰的管理功能。 話雖如此，沒有任何開發人員會禁止從 ASP.NET Core 資料保護應用程式開發介面使用的長期保護的機密資料。

## <a name="audience"></a>適用對象

資料保護系統分為五個主要的封裝。 這些 Api 的各個層面目標三個主要對象。

1. [取用者應用程式開發介面概觀](consumer-apis/overview.md)目標應用程式和 framework 開發人員。

   「 我不想要了解有關堆疊的運作方式或其設定方式。 我只想要執行中做為簡單的某些作業的方式盡可能高機率的成功使用應用程式開發介面。 」

2. [組態 Api](configuration/overview.md)目標應用程式開發人員和系統管理員。

   「 我需要告訴我的環境需要非預設路徑或設定資料保護系統 」。

3. 擴充性 Api 目標開發人員負責實作自訂原則。 使用這些 Api 會限制為罕見的情況下，發生，安全性知道開發人員。

   「 我需要取代系統內的整個元件，因為有唯一真正的行為需求。 我願意將了解 uncommonly 使用的 API 介面的組件，才能建立外掛程式，可滿足我的需求。 」

## <a name="package-layout"></a>封裝配置

資料保護堆疊包含五個封裝。

* Microsoft.AspNetCore.DataProtection.Abstractions 包含基本 IDataProtectionProvider 和 IDataProtector 的介面。 它也包含實用的擴充方法，可協助處理這些類型 （例如，多載的 IDataProtector.Protect）。 請參閱取用者介面 > 一節，如需詳細資訊。 如果其他人負責具現化的資料保護系統，而且您只需使用應用程式開發介面，您會想要參考 Microsoft.AspNetCore.DataProtection.Abstractions。

* Microsoft.AspNetCore.DataProtection 包含包括核心密碼編譯作業、 金鑰管理、 組態和擴充性的資料保護系統的核心實作。 如果您負責具現化的資料保護系統 （例如，將它加入 IServiceCollection） 或修改或擴充其行為，您會想要參考 Microsoft.AspNetCore.DataProtection。

* Microsoft.AspNetCore.DataProtection.Extensions 包含其他應用程式開發介面，開發人員會很有用，但其不屬於此核心套件中。 比方說，此套件包含簡單"具現化的系統沒有相依性資料隱碼安裝程式的特定金鑰的儲存目錄指向 「 應用程式開發介面 （詳細資訊）。 它也包含擴充方法，來限制受保護的內容 （詳細資訊） 的存留期。

* Microsoft.AspNetCore.DataProtection.SystemWeb 可以安裝到現有的 ASP.NET 4.x 應用程式重新導向其<machineKey>改為使用新的資料保護堆疊的作業。 請參閱[相容性](compatibility/replacing-machinekey.md#compatibility-replacing-machinekey)如需詳細資訊。

* Microsoft.AspNetCore.Cryptography.KeyDerivation 提供 PBKDF2 密碼雜湊常式的實作，並可供系統需要安全地處理使用者密碼。 請參閱[密碼雜湊](consumer-apis/password-hashing.md)如需詳細資訊。
