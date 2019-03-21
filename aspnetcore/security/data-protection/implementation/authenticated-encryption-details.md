---
title: ASP.NET Core 中的已驗證的加密詳細資料
author: rick-anderson
description: 了解 ASP.NET Core 資料保護驗證加密的實作詳細資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 9def03e6b27e19fc34a839e923d6152e086889db
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208576"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a>ASP.NET Core 中的已驗證的加密詳細資料

<a name="data-protection-implementation-authenticated-encryption-details"></a>

呼叫 IDataProtector.Protect 是已驗證的加密作業。 保護方法提供機密性和真實性，以及它會連結到目的鏈結，用來從其根 IDataProtectionProvider 衍生這個特定的 IDataProtector 執行個體。

IDataProtector.Protect 採用 byte [] 的純文字參數，並且產生 byte [] 受保護的承載，其格式如下所述。 （還有它採用純文字字串參數，並傳回字串的受保護的承載的擴充方法多載。 如果使用此 API 的受保護的裝載格式仍然會以下結構，但將予以[base64url 編碼](https://tools.ietf.org/html/rfc4648#section-5)。)

## <a name="protected-payload-format"></a>受保護的裝載格式

受保護的承載的格式是由三個主要元件所組成：

* 32 位元 magic 的標頭會識別資料保護系統的版本。

* 128 位元金鑰的識別碼，可識別用來保護此特定的承載的金鑰。

* 受保護承載的其餘部分[封裝此金鑰的加密程式的特定](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation)。 在下列範例中，索引鍵所代表的 AES-256-CBC + HMACSHA256 加密程式，並裝載進一步細分，如下所示：
  * 128 位元金鑰的修飾詞。
  * 128 位元初始向量。
  * AES-256-CBC 輸出 48 個位元組。
  * HMACSHA256 驗證標記中。

範例的受保護的承載如下所示。

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

從上面的前 32 個位元或 4 個位元組的承載格式所識別的版本 (09 F0 C9 F0) magic 標頭

接下來的 128 位元或 16 個位元組是金鑰的識別碼 (80 9 C 81 0c 19 66 19 40 95 36 53 F8 AA FF EE 57)

其餘部分包含裝載，並且專屬於使用的格式。

> [!WARNING]
> 保護指定的索引鍵的所有裝載會使用相同的 20 個位元組 （magic 的值，金鑰識別碼） 標頭都開始。 管理員可以使用這項事實，供診斷之用來模擬時所產生的裝載。 例如，上述承載會對應至索引鍵 {0c819c80-6619-4019-9536-53f8aaffee57}。 如果您在檢查索引鍵的存放庫之後發現此特定索引鍵的啟動日期是 2015年-01-01，和它的到期日是 2015年-03-01，則很合理假設承載 （如果未遭竄改） 產生該視窗中，提供或採取一個小型任一端餘裕。
