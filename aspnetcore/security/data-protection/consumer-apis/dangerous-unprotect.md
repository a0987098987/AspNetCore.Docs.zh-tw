---
title: "取消保護裝載之索引鍵已被撤銷。"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 6c4e6591-45d2-4d25-855e-062ad352d648
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 082fd69769dd0ef000b39ec148c12719d66f7aac
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2017
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a>取消保護裝載之索引鍵已被撤銷。

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

ASP.NET Core 資料保護 Api 主要被不適用於機密裝載的無限期持續性。 其他技術喜歡[Windows CNG api，DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)和[Azure Rights Management](https://docs.microsoft.com/rights-management/)更適合的案例是無限期的儲存體，而且必須跟著強式金鑰的管理功能。 話雖如此，沒有任何開發人員會禁止從 ASP.NET Core 資料保護應用程式開發介面使用的長期保護的機密資料。 永遠不會移除金鑰從金鑰環，因此 IDataProtector.Unprotect 一律可以復原現有的承載，只要索引鍵可供使用且有效。

不過，當開發人員嘗試因為 IDataProtector.Unprotect 將會擲回例外狀況在此情況下，取消保護使用已撤銷金鑰時，受保護的資料時，就會發生問題。 裝載這類可以輕鬆地重新建立系統和站台訪客最糟的情況可能需要再次登入，這可能是問題 （例如驗證語彙基元），需短暫存在或暫時性的裝載。 但為保存的裝載具有 Unprotect 擲回可能導致無法接受資料遺失。

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

若要支援的案例是讓裝載為未受到保護，即使遇到已撤銷的索引鍵時，資料保護系統包含 IPersistedDataProtector 類型。 若要取得 IPersistedDataProtector 的執行個體，只要取得 IDataProtector 以正常方式執行個體，並轉型至 IPersistedDataProtector IDataProtector 再試一次。

> [!NOTE]
> 並非所有 IDataProtector 執行個體都可轉換成 IPersistedDataProtector。 開發人員應該使用 C# 運算子或類似以避免執行階段例外狀況起因是無效的轉型，而且應該有準備好要適當地處理失敗的情況。

IPersistedDataProtector 會公開下列 API 介面：

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
   ```

這個 API 會接受受保護的內容 （以位元組陣列），並傳回未受保護的內容。 沒有任何字串為基礎的多載。 這兩個 out 參數如下所示。

* requiresMigration： 將設定為 true，如果用來保護此裝載金鑰不再使用中的預設索引鍵，例如，用來保護此內容的索引鍵是舊的復原作業的索引鍵已自採取的地方。 呼叫端可能要考慮重新保護根據商務需求的裝載。

* wasRevoked： 將設為 true，如果用來保護此內容的索引鍵已被撤銷。

>[!WARNING]
> 傳遞 ignoreRevocationErrors 時小心： true DangerousUnprotect 方法。 如果呼叫這個方法後 wasRevoked 值為 true，然後用來保護此內容的索引鍵已被撤銷，並裝載的真實性應該被視為有疑問。 在此情況下只繼續作業上未受保護的內容，如果您有一些個別的保證，它是真確，例如它的來自安全的資料庫，而不是受信任的 web 用戶端所傳送。

[!code-none[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]
