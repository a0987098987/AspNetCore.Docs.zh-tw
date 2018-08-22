---
title: 目的階層和 ASP.NET Core 中的多租用戶
author: rick-anderson
description: 當手寫筆與 ASP.NET Core 資料保護 Api，深入了解目的字串階層和多租用戶。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: 1133d40e7b325d58b3f70e7387494dae36ff8ac9
ms.sourcegitcommit: fd461c60b5e36c7019f81da0138cc859d0fddaa2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/10/2018
ms.locfileid: "41825093"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>目的階層和 ASP.NET Core 中的多租用戶

由於`IDataProtector`也會以隱含方式`IDataProtectionProvider`，目的可以鏈結在一起。 這方面`provider.CreateProtector([ "purpose1", "purpose2" ])`相當於`provider.CreateProtector("purpose1").CreateProtector("purpose2")`。

這可讓資料保護系統透過一些有趣的階層式關聯性。 在先前範例中的[Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose)，SecureMessage 元件可以呼叫`provider.CreateProtector("Contoso.Messaging.SecureMessage")`一次預付款和到私用快取結果`_myProvider`欄位。 未來的保護裝置，就可以建立透過呼叫`_myProvider.CreateProtector("User: username")`，而這些保護裝置會用於保護個別訊息。

這可以也翻轉。 請考慮單一邏輯應用程式與它自己的驗證和狀態管理系統，則可以設定 （CMS 看似合理） 的多個租用戶和每個租用戶的主機。 傘應用程式具有單一主要提供者，而且它會呼叫`provider.CreateProtector("Tenant 1")`和`provider.CreateProtector("Tenant 2")`給每個租用戶自己隔離的配量的資料保護系統。 租用戶無法再衍生自己個別的保護裝置根據其自己的需求，但是無論他們嘗試的嚴格程度他們無法建立相衝突的保護裝置與任何其他租用戶系統中。 以圖形方式，這表示，如下所示。

![多租用戶之用](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> 這是假設傘應用程式控制的 Api 可供個別租用戶和租用戶無法在伺服器上執行任意程式碼。 如果租用戶可以執行任意程式碼，他們無法執行中斷隔離保證的私用反映或它們可能只是直接讀取主要金鑰材料衍生任何子機碼他們想要。

資料保護系統實際上會使用一種以預設的立即可用設定的多租用戶。 根據預設會儲存主要金鑰材料，背景工作處理序帳戶的使用者設定檔資料夾 （或登錄中，為 IIS 應用程式集區身分識別）。 但它是使用單一帳戶來執行多個應用程式，其實相當常見，並因此所有這些應用程式會得到共用金鑰產製原料的主機。 若要解決這個問題，資料保護系統會自動插入唯一-個別應用程式識別碼的整體目的鏈結中的第一個元素。 這個隱含的目的是用來[隔離個別的應用程式](xref:security/data-protection/configuration/overview#per-application-isolation)彼此的有效相比，將每個應用程式視為唯一的租用戶中系統，以及保護裝置建立程序看起來與上圖相同。
