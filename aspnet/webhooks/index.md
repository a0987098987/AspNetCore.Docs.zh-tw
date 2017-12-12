---
uid: webhooks/index
title: "ASP.NET Webhook 概觀 |Microsoft 文件"
author: rick-anderson
description: "ASP.NET Webhook 簡介。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-overview"></a>ASP.NET Webhook 概觀

Webhook 為輕量型的 HTTP 模式，提供簡單的 pub/sub 模型一起接線 Web Api 和 SaaS 服務。 當事件發生在服務中時，會傳送通知形式的 HTTP POST 要求到已註冊的訂閱者。 POST 要求包含可讓收件者要據此採取行動的事件的相關資訊。

其簡化，因為 Webhook 已公開的服務，包括大量[Dropbox](http://dropbox.com/)， [GitHub](http://www.github.com/)， [Bitbucket](https://bitbucket.org/)， [MailChimp](http://www.mailchimp.com/)， [PayPal](http://www.paypal.com/)， [Slack](http://www.slack.com)，[等量磁碟區](http://www.stripe.com)， [Trello](http://www.trello.com/)，等等。 比方說，WebHook 可能表示檔案已在變更[Dropbox](http://dropbox.com/)，或在 GitHub 中的程式碼變更已認可或已起始付款中[PayPal](http://www.paypal.com/)，或已在中建立卡片[Trello](http://www.trello.com/)。 有無限的可能性 ！

Microsoft ASP.NET Webhook 可以更輕鬆地傳送和接收 Webhook 做為您的 ASP.NET 應用程式的一部分：

* 在接收端，它會提供通用模型用於接收和處理從任意數目的 WebHook 提供者的 Webhook。 這樣做，可以支援現成[Dropbox](http://dropbox.com/)， [GitHub](http://www.github.com/)， [Bitbucket](https://bitbucket.org/)， [MailChimp](http://www.mailchimp.com/)， [PayPal](http://www.paypal.com/)，[推送](http://www.pusher.com)， [Salesforce](http://www.salesforce.com)， [Slack](http://www.slack.com)，[等量磁碟區](http://www.stripe.com)， [Trello](http://www.trello.com/)，[WordPress](http://www.wordpress.com)和[Zendesk](https://www.zendesk.com/)但所以可以輕鬆地加入更多的支援。

* 在傳送端中，它會提供支援供管理和儲存訂閱也將事件通知傳送至正確的訂閱者集合。 這可讓您定義您自己的 「 訂閱者 」 可以訂閱並事情發生時予以通知的事件集。

兩個部分可以一起或分開使用根據您的案例。 如果您只需要 Webhook 接收來自其他服務，則您可以使用只有的接收者部分;如果您只想要公開 Webhook 供其他人使用，然後您可以這麼做。

ASP.NET Web API 2 和 ASP.NET MVC 5 為目標的程式碼，可做為[GitHub 上的 OSS](https://github.com/aspnet/WebHooks)。

## <a name="webhooks-overview"></a>Webhook 概觀

Webhook 為模式，這表示不同如何使用從服務到服務，但基本的概念是一樣。 您可以將 Webhook 視為簡單 pub/sub 模型其中使用者可以在其他地方發生的事件訂閱。 事件通知會傳送為 HTTP POST 要求，其中包含事件本身的相關資訊。

通常的 HTTP POST 要求包含 JSON 物件或觸發程序判斷 WebHook 寄件者包括造成 WebHook 事件的相關資訊的 HTML 表單資料。 例如，從的 WebHook POST 要求主體範例[GitHub](http://www.github.com/)看起來像這樣的結果的特定儲存機制中正在開啟新議題：

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

若要確保此 WebHook 確實是來自預定的傳送者，會在 POST 要求以某種方式安全，並驗證收件者。 例如， [GitHub Webhook](https://developer.github.com/webhooks/)包含*X 中樞簽章型*具有雜湊的要求主體，所以您不需擔心接收者實作所檢查的 HTTP 標頭。

WebHook 流程通常會像下面這樣：

* WebHook 寄件者會公開用戶端可以訂閱的事件。 事件描述可觀察變更到系統，例如新的資料項目已插入，已完成處理程序，或其他項目。

* WebHook 接收者可訂閱註冊 WebHook 四個項目所組成：

     1. URI 形式的 HTTP POST 要求; 其中應該公佈事件通知

     2. 一組篩選描述特定的事件，應該引發此 WebHook。

     3. 秘密金鑰用來簽署的 HTTP POST 要求。

     4. 這是要包含在 HTTP POST 要求中的其他資料。 這可以例如額外的 HTTP 標頭欄位或 HTTP POST 要求主體中包含的屬性。

* 事件發生時，一旦找到相符的 WebHook 註冊，並提交 HTTP POST 要求。 通常，產生的 HTTP POST 要求的是如果因某種原因收件者沒有回應的 HTTP POST 要求會導致錯誤回應重試數次。

## <a name="webhooks-processing-pipeline"></a>Webhook 處理管線

Microsoft ASP.NET Webhook 處理管線的內送的 Webhook 看起來像這樣：

![ASP.NET Webhook 處理管線](_static/WebHookReceivers.png)

兩個索引鍵這裡的概念*接收者*和*處理常式*:

* *接收者*必須負責處理來自特定寄件者的 WebHook 特定類別，以及強制執行安全性檢查，以確保 WebHook 要求確實是來自預定的傳送者。

* *處理常式*通常是使用者程式碼執行處理特定的 WebHook。

下列節點中的詳細說明這些概念。
