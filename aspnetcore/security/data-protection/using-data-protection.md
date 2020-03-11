---
title: 開始使用 ASP.NET Core 中的資料保護 Api
author: rick-anderson
description: 瞭解如何使用 ASP.NET Core 的資料保護 Api 來保護和解除保護應用程式中的資料。
ms.author: riande
ms.date: 11/12/2019
no-loc:
- SignalR
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 8c3f3c7fb21434cf335591c41741f0ce868df33e
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664434"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>開始使用 ASP.NET Core 中的資料保護 Api

<a name="security-data-protection-getting-started"></a>

最簡單的是，保護資料是由下列步驟所組成：

1. 從資料保護提供者建立資料保護裝置。

2. 使用您想要保護的資料，呼叫 `Protect` 方法。

3. 使用您想要轉換成純文字的資料，呼叫 `Unprotect` 方法。

大部分的架構和應用程式模型（例如 ASP.NET Core 或 SignalR）已經設定資料保護系統，並將它新增至您透過相依性插入存取的服務容器。 下列範例示範如何設定服務容器以進行相依性插入和註冊資料保護堆疊、透過 DI 接收資料保護提供者、建立保護裝置，以及保護然後解除保護資料。

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

當您建立保護裝置時，必須提供一或多個[目的字串](xref:security/data-protection/consumer-apis/purpose-strings)。 目的字串提供取用者之間的隔離。 例如，使用「綠色」目的字串所建立的保護裝置，將無法取消保護裝置提供的資料，目的為「紫色」。

>[!TIP]
> `IDataProtectionProvider` 和 `IDataProtector` 的實例對於多個呼叫端而言都是安全線程。 這是因為一旦元件透過呼叫 `CreateProtector`取得 `IDataProtector` 的參考，它就會使用該參考進行多個 `Protect` 和 `Unprotect`的呼叫。
>
>如果無法驗證或解密受保護的承載，`Unprotect` 的呼叫將會擲回 System.security.cryptography.cryptographicexception。 某些元件可能會想要在取消保護作業期間忽略錯誤;讀取驗證 cookie 的元件可能會處理此錯誤，並將要求視為完全沒有 cookie，而不是直接讓要求失敗。 需要此行為的元件應該特別捕捉 System.security.cryptography.cryptographicexception，而不是抑制所有例外狀況。
