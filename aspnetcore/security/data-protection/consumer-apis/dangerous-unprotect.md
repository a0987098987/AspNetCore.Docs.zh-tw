---
title: 取消保護的金鑰已遭撤銷，ASP.NET Core 中裝載
author: rick-anderson
description: 了解如何取消保護受保護之後已撤銷，ASP.NET Core 應用程式中的索引鍵的資料。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 26061d048dcd9c1e3d8909e9388d8b565376fa2f
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208027"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>取消保護的金鑰已遭撤銷，ASP.NET Core 中裝載

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

無限期的持續性的機密承載不主要是 ASP.NET Core 資料保護 Api。 等其他技術[Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)並[Azure Rights Management](/rights-management/)更適合的案例是無限制的儲存體，而且必須跟著強式金鑰管理功能。 話雖如此，沒有任何禁止開發人員使用 ASP.NET Core 資料保護 Api 進行長期保護的機密資料。 永遠不會移除金鑰從金鑰環，因此`IDataProtector.Unprotect`，只要索引鍵可供使用且有效，則一律可以復原現有的裝載。

不過，就會發生問題而開發人員嘗試解除已為保護以已撤銷的索引鍵的資料時`IDataProtector.Unprotect`在此情況下將會擲回例外狀況。 這類的承載可以輕鬆重新建立系統，以及最糟的情況，網站訪客可能必須再次登入，這可能是適合用於短期或非暫時性的承載 （例如驗證權杖）。 保存的承載，有的但`Unprotect`throw 可能會導致無法接受資料遺失。

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

若要支援的案例是讓裝載為未受到保護，即使已撤銷的索引鍵，包含資料保護系統`IPersistedDataProtector`型別。 若要取得的執行個體`IPersistedDataProtector`，直接取得的執行個體`IDataProtector`在正常的方式，請嘗試轉型`IDataProtector`至`IPersistedDataProtector`。

> [!NOTE]
> 並非所有`IDataProtector`執行個體可以轉換成`IPersistedDataProtector`。 開發人員應該使用C#運算子或類似以避免執行階段例外狀況造成無效的轉型，而且應該有準備好適當地處理失敗的情況。

`IPersistedDataProtector` 會公開下列的 API 介面：

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

此 API 會採用受保護的內容 （以位元組陣列），並傳回未受保護的內容。 沒有任何字串為基礎的多載。 這兩個 out 參數如下所示。

* `requiresMigration`： 會設定為 true，如果用來保護此承載的金鑰不再使用中的預設索引鍵，例如，用來保護此承載的金鑰是舊金鑰輪換作業以來進行的地方。 呼叫端可能要考慮重新保護的承載，根據其商務需求而定。

* `wasRevoked`： 將會設為 true，如果用來保護此承載的索引鍵已撤銷。

>[!WARNING]
> 特別小心謹慎時傳遞`ignoreRevocationErrors: true`至`DangerousUnprotect`方法。 如果呼叫這個方法之後`wasRevoked`值為 true，則用來保護此承載的索引鍵已撤銷，且承載的真確性應該被視為有疑問。 在此情況下，只有繼續操作未受保護的內容，如果您有一些不同的保證，它的真實性，例如它來自安全的資料庫，而不是受信任的 web 用戶端所傳送。

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
