---
title: "取消保護裝載之索引鍵已被撤銷。"
author: rick-anderson
description: "本文件說明如何取消保護受保護，因為已撤銷，ASP.NET Core 應用程式中的索引鍵的資料。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 08a8ad9b3b3cc2de48751d4149bf39c58954fd90
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a>取消保護裝載之索引鍵已被撤銷。

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

ASP.NET Core 資料保護 Api 主要被不適用於機密裝載的無限期持續性。 其他技術喜歡[Windows CNG api，DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)和[Azure Rights Management](https://docs.microsoft.com/rights-management/)更適合的案例是無限期的儲存體，而且必須跟著強式金鑰的管理功能。 話雖如此，沒有任何開發人員會禁止從 ASP.NET Core 資料保護應用程式開發介面使用的長期保護的機密資料。 永遠不會移除金鑰從金鑰環，因此`IDataProtector.Unprotect`，只要索引鍵可供使用且有效，則一律可以復原現有的裝載。

不過，發生問題時，開發人員嘗試取消保護已受到保護以已撤銷的索引鍵成為的資料時`IDataProtector.Unprotect`在此情況下將會擲回例外狀況。 裝載這類可以輕鬆地重新建立系統和站台訪客最糟的情況可能需要再次登入，這可能是問題 （例如驗證語彙基元），需短暫存在或暫時性的裝載。 但具有的保存裝載`Unprotect`無法接受資料遺失可能會導致擲回。

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

若要支援的案例是讓裝載為未受到保護，即使遇到已撤銷的索引鍵時，包含資料保護系統`IPersistedDataProtector`型別。 若要取得的執行個體`IPersistedDataProtector`，只會收到的執行個體`IDataProtector`正常方式並再試一次轉換中`IDataProtector`至`IPersistedDataProtector`。

> [!NOTE]
> 並非所有`IDataProtector`執行個體都可以轉換成`IPersistedDataProtector`。 開發人員應該使用 C# 運算子或類似以避免執行階段例外狀況起因是無效的轉型，而且應該有準備好要適當地處理失敗的情況。

`IPersistedDataProtector`會公開下列 API 介面：

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

這個 API 會接受受保護的內容 （以位元組陣列），並傳回未受保護的內容。 沒有任何字串為基礎的多載。 這兩個 out 參數如下所示。

* `requiresMigration`： 將設定為 true，如果用來保護此裝載金鑰不再使用中的預設索引鍵，例如，用來保護此內容的索引鍵是舊的復原作業的索引鍵已自採取的地方。 呼叫端可能要考慮重新保護根據商務需求的裝載。

* `wasRevoked`： 將設為 true，如果用來保護此內容的索引鍵已被撤銷。

>[!WARNING]
> 傳遞時小心`ignoreRevocationErrors: true`至`DangerousUnprotect`方法。 如果呼叫這個方法後的`wasRevoked`值為 true，則用來保護此內容的索引鍵已被撤銷，並裝載的真實性應該被視為有疑問。 在此情況下，只有繼續上未受保護的內容操作，如果您有一些個別的保證，它是真確的例如它來自安全的資料庫，而不是受信任的 web 用戶端所傳送。

[!code-csharp[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]
