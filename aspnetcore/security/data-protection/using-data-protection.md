---
title: "開始使用資料保護應用程式開發介面"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 9489b55b1de626b77bcbe21cb9678e561b559099
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a>開始使用資料保護應用程式開發介面

<a name="security-data-protection-getting-started"></a>

在其最簡單的保護資料包含下列步驟：

1. 從資料保護提供者建立資料保護裝置。

2. 呼叫您要保護的資料保護方法。

3. 呼叫 Unprotect 方法與您想要變成純文字資料。

大部分的架構，例如 ASP.NET 或 SignalR 已設定資料保護系統，並將它加入至您存取透過相依性插入的服務容器。 下列範例將示範如何設定服務容器相依性插入和註冊資料保護堆疊、 接收資料保護提供者，透過 DI、 建立保護裝置和保護，然後取消保護的資料

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

當您建立保護裝置時必須提供一個或多個[目的字串](consumer-apis/purpose-strings.md)。 目的字串之間提供隔離取用者，例如建立目的字串 「 綠色 」 的保護裝置就無法取消保護的保護裝置所提供的目的為 「 紫色"的資料。

>[!TIP]
> IDataProtectionProvider 和 IDataProtector 的執行個體是安全執行緒的多個呼叫端。 是，一旦元件取得透過呼叫 CreateProtector IDataProtector 的參考，它會使用該參考的多個呼叫保護且取消保護。
>
>取消保護的呼叫將會擲回 CryptographicException，如果無法驗證或是用來解密受保護的內容。 某些元件可能會想要忽略的錯誤時取消保護作業;元件會讀取驗證 cookie，可能會處理這種錯誤和任何 cookie 已完全處理要求而徹底要求失敗。 元件的這個行為特別應該攔截 CryptographicException，而不是抑制所有例外狀況。
