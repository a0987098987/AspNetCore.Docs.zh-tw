---
title: 取消保護已在 ASP.NET Core 中撤銷其金鑰的承載
author: rick-anderson
description: 瞭解如何在 ASP.NET Core 的應用程式中，取消保護已撤銷的金鑰所保護的資料。
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
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: a0b5bb29c509e8cc999b998776da3ab4ec27ec29
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408392"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>取消保護已在 ASP.NET Core 中撤銷其金鑰的承載

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

ASP.NET Core 的資料保護 Api 主要不適用於機密承載的無限持續性。 其他技術（如[WINDOWS CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)和[Azure Rights Management](/rights-management/) ）更適用于不限數量的儲存體案例，而且它們已有更強的金鑰管理功能。 話雖如此，開發人員也不會使用 ASP.NET Core 的資料保護 Api 來長期保護機密資料。 金鑰永遠不會從金鑰環中移除，因此 `IDataProtector.Unprotect` 只要金鑰可供使用且有效，一律可以復原現有的裝載。

不過，當開發人員嘗試取消保護已撤銷的金鑰的資料時，就會發生問題，因為 `IDataProtector.Unprotect` 在此情況下會擲回例外狀況。 這可能適用于短期或暫時性的承載（例如驗證權杖），因為這種類型的裝載可輕易地由系統重新建立，而且在最糟的情況下，網站訪客可能需要再次登入。 但是對於保存的承載，具有 `Unprotect` throw 可能會導致無法接受的資料遺失。

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

為了支援即使在臉部已撤銷金鑰的情況下允許裝載不受保護的案例，資料保護系統會包含 `IPersistedDataProtector` 類型。 若要取得的實例 `IPersistedDataProtector` ，只要以正常方式取得的實例， `IDataProtector` 然後嘗試將轉換 `IDataProtector` 成 `IPersistedDataProtector` 。

> [!NOTE]
> 並非所有 `IDataProtector` 的實例都可以轉換成 `IPersistedDataProtector` 。 開發人員應該使用 c # as 運算子，或類似于避免無效轉換所造成的執行時間例外狀況，而且應該準備好適當地處理失敗的情況。

`IPersistedDataProtector`公開下列 API 介面：

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

此 API 會採用受保護的承載（做為位元組陣列），並傳回未受保護的裝載。 沒有以字串為基礎的多載。 這兩個輸出參數如下所示。

* `requiresMigration`：如果用來保護此承載的金鑰已不再是作用中的預設金鑰（例如，用來保護此承載的金鑰已過時），而且已發生金鑰變換作業，則會設定為 true。 呼叫端可能會想要考慮根據其商務需求來重新保護裝載。

* `wasRevoked`：如果用來保護此承載的金鑰已被撤銷，則會設定為 true。

>[!WARNING]
> 在傳遞至方法時，請格外小心 `ignoreRevocationErrors: true` `DangerousUnprotect` 。 如果在呼叫這個方法之後，此 `wasRevoked` 值為 true，則會撤銷用來保護此承載的金鑰，而且應該將承載的真實性視為可疑。 在這種情況下，只有在未受保護的裝載上有一些不同的保證可供使用時，才繼續運作，例如，它是來自安全資料庫，而不是由不受信任的 web 用戶端所傳送。

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
