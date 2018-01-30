---
title: "開始使用資料保護應用程式開發介面"
author: rick-anderson
description: "本文件說明如何保護和解除資料的應用程式中使用的 ASP.NET Core 資料保護 Api。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/using-data-protection
ms.openlocfilehash: e8c4183f5c47d8ffec8edf163eb1e4d6f2757d9d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-the-data-protection-apis"></a>開始使用資料保護應用程式開發介面

<a name="security-data-protection-getting-started"></a>

在其最簡單的保護資料包含下列步驟：

1. 從資料保護提供者建立資料保護裝置。

2. 呼叫`Protect`方法與您想要保護的資料。

3. 呼叫`Unprotect`要變成純文字資料的方法。

大部分的架構和應用程式模型，例如 ASP.NET 或 SignalR 已設定資料保護系統，並將它加入至您存取透過相依性插入的服務容器。 下列範例將示範如何設定服務容器相依性插入和註冊資料保護堆疊、 接收資料保護提供者，透過 DI、 建立保護裝置和保護，然後取消保護的資料

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

當您建立保護裝置時必須提供一個或多個[目的字串](consumer-apis/purpose-strings.md)。 目的字串之間提供隔離取用者。 例如，使用 「 綠色 」 的目的字串建立的保護裝置就無法取消保護的保護裝置所提供的目的為 「 紫色"的資料。

>[!TIP]
> 執行個體`IDataProtectionProvider`和`IDataProtector`是多個呼叫的執行緒安全。 它具有一旦元件取得的參考用的`IDataProtector`透過呼叫`CreateProtector`，它會使用該參考的多個呼叫`Protect`和`Unprotect`。
>
>呼叫`Unprotect`將會擲回 CryptographicException，如果無法驗證或是用來解密受保護的內容。 某些元件可能會想要忽略的錯誤時取消保護作業;元件會讀取驗證 cookie，可能會處理這種錯誤和任何 cookie 已完全處理要求而徹底要求失敗。 元件的這個行為特別應該攔截 CryptographicException，而不是抑制所有例外狀況。
