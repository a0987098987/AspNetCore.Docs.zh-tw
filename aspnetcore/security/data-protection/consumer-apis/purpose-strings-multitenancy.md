---
title: 目的階層和 ASP.NET Core 中的多租用戶
author: rick-anderson
description: 了解目的字串階層和多租用戶與 ASP.NET Core 資料保護 Api。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: f0c39d54c164595c2135e0eb0d911796e215dd66
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273607"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>目的階層和 ASP.NET Core 中的多租用戶

因為`IDataProtector`也是隱含`IDataProtectionProvider`，用途可以鏈結在一起。 就這個意義而言，`provider.CreateProtector([ "purpose1", "purpose2" ])`相當於`provider.CreateProtector("purpose1").CreateProtector("purpose2")`。

這可讓您透過資料保護系統一些有趣的階層式關聯性。 在先前範例中的[Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose)，SecureMessage 元件可以呼叫`provider.CreateProtector("Contoso.Messaging.SecureMessage")`一次前期和私用到快取結果`_myProvide`欄位。 未來的保護裝置，就可以建立透過呼叫`_myProvider.CreateProtector("User: username")`，而且會用於這些保護裝置保護個別訊息。

這可以也翻轉。 請考慮單一邏輯應用程式與它自己的驗證和狀態管理系統，則可以設定多個租用戶 （CMS 似乎很合理） 和每個租用戶的主機。 概括性應用程式具有單一主要提供者，而且它會呼叫`provider.CreateProtector("Tenant 1")`和`provider.CreateProtector("Tenant 2")`來提供每個租用戶的資料保護系統自己隔離配量。 租用戶無法再衍生自己個別的保護裝置根據其自己的需求，但是無論他們嘗試手動他們無法建立相衝突的保護裝置與任何其他租用戶系統中。 以圖形方式，這被表示如下。

![多組織用戶管理用途](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> 這是假設概括性的應用程式開發介面可供個別租用戶和租用戶，無法在伺服器上執行任意程式碼的應用程式控制項。 如果租用戶可以執行任意程式碼中，使用者無法執行中斷的隔離保證，私用反映或只無法直接讀取主要金鑰材料並衍生任何子機碼想。

資料保護系統實際上會使用預設的方塊超出設定的多租用戶的排序。 根據預設主要金鑰的資料會儲存在工作處理序帳戶的使用者設定檔資料夾 （或登錄中的，為 IIS 應用程式集區身分識別）。 但使用的單一帳戶來執行多個應用程式，實際上很常見，因此所有這些應用程式都會結束共用主要金鑰材料。 若要解決這個問題，資料保護系統會自動為整體目的鏈結中的第一個項目插入唯一-針對應用程式識別項。 這個隱含的目的是用來[隔離個別的應用程式](xref:security/data-protection/configuration/overview#per-application-isolation)彼此有效地將每個應用程式，因為唯一的租用戶系統，並保護裝置建立程序內看起來與上圖相同。
