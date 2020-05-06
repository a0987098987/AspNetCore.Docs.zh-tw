---
title: ASP.NET Core 中已驗證的加密詳細資料
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護已驗證加密的執行詳細資料。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 3066cd505781ed2ddad46626dda9d9ce35307877
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776964"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a>ASP.NET Core 中已驗證的加密詳細資料

<a name="data-protection-implementation-authenticated-encryption-details"></a>

Idataprotector 加密的呼叫是已驗證的加密作業。 保護方法提供機密性和真實性，並系結至用來從根 IDataProtectionProvider 衍生這個特定 Idataprotector 加密實例的目的鏈。

Idataprotector 加密。保護採用 byte [] 純文字參數，並產生 byte [] 受保護的承載，其格式如下所述。 （還有一個擴充方法多載，它採用字串純文字參數，並傳回字串保護的裝載。 如果使用此 API，受保護的承載格式仍然會有下列結構，但它將會[base64url 編碼](https://tools.ietf.org/html/rfc4648#section-5)。）

## <a name="protected-payload-format"></a>受保護的承載格式

受保護的承載格式是由三個主要元件所組成：

* 32位的神奇標頭，可識別資料保護系統的版本。

* 128位金鑰識別碼，用來識別用來保護此特定裝載的金鑰。

* 受保護承載的其餘部分是[此金鑰所封裝之加密](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation)程式的特定。 在下列範例中，金鑰代表 AES-256-CBC + HMACSHA256 加密程式，而裝載會進一步細分，如下所示：
  * 128位金鑰修飾詞。
  * 128位初始化向量。
  * 48個位元組的 AES-256-CBC 輸出。
  * HMACSHA256 authentication 標記。

範例受保護的承載如下所示。

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

從高於前32位的承載格式，或4個位元組是識別版本的魔術標頭（09 F0 C9 F0）

下一個128位或16個位元組是金鑰識別碼（80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57）

餘數包含裝載，且為所用格式的特定內容。

> [!WARNING]
> 受指定金鑰保護的所有承載都將以相同的20位元組（魔術值、金鑰識別碼）標頭開頭。 系統管理員可以使用此事實來進行診斷，以估計承載的產生時間。 例如，上述裝載對應于 key {0c819c80-6619-4019-9536-53f8aaffee57}。 如果在檢查金鑰存放庫之後，發現此特定金鑰的啟用日期為2015-01-01，而其到期日為2015-03-01，則合理假設該視窗內產生的承載（如果未遭修改），則在任一側提供或採用小型巧克力因素。
