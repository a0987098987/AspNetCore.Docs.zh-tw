---
title: 開始使用 ASP.NET Core 中的資料保護 Api
author: rick-anderson
description: 了解如何保護和解除資料的應用程式中使用 ASP.NET Core 資料保護 Api。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 25bf099a3d9edd7e6e0872725cbc3707750314e6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087644"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>開始使用 ASP.NET Core 中的資料保護 Api

<a name="security-data-protection-getting-started"></a>

在其最簡單、 保護資料包含下列步驟：

1. 從資料保護提供者建立資料保護裝置。

2. 呼叫`Protect`與您想要保護的資料的方法。

3. 呼叫`Unprotect`要變成純文字資料的方法。

大部分的架構和應用程式模型，例如 ASP.NET Core 或 SignalR，已設定資料保護系統，並將它新增至您存取透過相依性插入服務容器。 下列範例會示範設定相依性插入的服務容器和註冊資料保護堆疊、 接收透過 DI 的資料保護提供者、 建立保護裝置和保護，則正在解除保護的資料。

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

當您建立的保護裝置時您必須提供一或多個[目的字串](xref:security/data-protection/consumer-apis/purpose-strings)。 目的字串提供取用者之間的隔離。 比方說，使用 「 綠色 」 的目的字串建立的保護裝置將無法取消保護 「 紫色 」 的目的在於提供保護的資料。

>[!TIP]
> 執行個體`IDataProtectionProvider`和`IDataProtector`是安全執行緒的多個呼叫端。 其目的是，一旦元件取得的參考`IDataProtector`透過呼叫`CreateProtector`，它會使用該參考的多個呼叫`Protect`和`Unprotect`。
>
>呼叫`Unprotect`將會擲回 CryptographicException，如果無法驗證或用來解密受保護的內容。 某些元件可能會想要忽略錯誤期間取消保護作業;讀取驗證 cookie 的元件可能會處理此錯誤和處理要求，如同它有完全沒有 cookie 而非直接讓要求失敗。 想要此行為元件特別應該攔截 CryptographicException，而不是抑制所有的例外狀況。
