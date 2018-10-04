---
uid: webhooks/index
title: ASP.NET Webhook 概觀 |Microsoft Docs
author: rick-anderson
description: ASP.NET Webhook 簡介。
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 702cc0bf0d0bb887c64bec19e1faf249bd96617a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "48253680"
---
# <a name="aspnet-webhooks-overview"></a>ASP.NET Webhook 概觀

Webhook 是一起接線 Web Api 和 SaaS 服務提供簡單 pub/sub 模型的輕量型 HTTP 模式。 當事件發生在服務中時，會傳送通知的 HTTP POST 要求表單中已註冊的訂閱者。 POST 要求包含可讓收件者要據此採取行動之事件的相關資訊。

因為它們簡便易行，Webhook 已公開的服務包括一大堆[Dropbox](http://dropbox.com/)， [GitHub](http://www.github.com/)， [Bitbucket](https://bitbucket.org/)， [MailChimp](http://www.mailchimp.com/)， [PayPal](http://www.paypal.com/)， [Slack](http://www.slack.com)，[等量磁碟區](http://www.stripe.com)， [Trello](http://www.trello.com/)，和其他項目。 例如，WebHook 會指出檔案已在變更[Dropbox](http://dropbox.com/)，或已認可變更程式碼在 GitHub 中，或在已起始付款[PayPal](http://www.paypal.com/)，或已在中建立卡片[Trello](http://www.trello.com/)。 有無限的可能性 ！

Microsoft ASP.NET Webhook 可讓您更輕鬆地傳送和接收 Webhook 作為您 ASP.NET 應用程式的一部分：

* 在接收端，提供常見的模式接收及處理任何數量的 WebHook 提供者的 Webhook。 它有支援現成[Dropbox](http://dropbox.com/)， [GitHub](http://www.github.com/)， [Bitbucket](https://bitbucket.org/)， [MailChimp](http://www.mailchimp.com/)， [PayPal](http://www.paypal.com/)， [pusher](http://www.pusher.com)， [Salesforce](http://www.salesforce.com)， [Slack](http://www.slack.com)，[等量磁碟區](http://www.stripe.com)， [Trello](http://www.trello.com/)，[WordPress](http://www.wordpress.com)並[Zendesk](https://www.zendesk.com/)但就可以輕鬆地加入更多的支援。

* 在傳送端中，它會提供支援供管理和儲存訂閱也將事件通知傳送至 「 訂閱者 」 權限集。 這可讓您定義自己的訂閱者可以訂閱，並通知他們，事情發生時的事件集。

兩個部分可以一起或分開使用根據您的案例。 如果您只需要接收來自其他服務的 Webhook，則您可以使用只有的接收者部分;如果您只想要公開為其他人使用的 Webhook，然後您可以這麼輕鬆。

以 ASP.NET Web API 2 和 ASP.NET MVC 5 為目標的程式碼，並可從[GitHub 上的 OSS](https://github.com/aspnet/WebHooks)。

## <a name="webhooks-overview"></a>Webhook 概觀

Webhook 是表示不使用方式從服務至服務，但基本概念是相同的模式。 您可以想像 Webhook 的簡單 pub/sub 模型，使用者可以在其他地方發生的事件訂閱。 事件通知會傳播包含事件本身的相關資訊的 HTTP POST 要求。

通常 HTTP POST 要求所包含的 JSON 物件或 HTML 表單資料來觸發程序取決於 WebHook 寄件者包括造成 WebHook 事件的相關資訊。 例如，從 WebHook POST 要求主體的範例[GitHub](http://www.github.com/)看起來像這樣的結果在特定的存放庫中開啟新的問題：

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

若要確保 WebHook 確實是來自預定的傳送者，會在 POST 要求以某種方式安全，並再經過接收者。 例如， [GitHub Webhook](https://developer.github.com/webhooks/)包含*簽章 X 型中樞*的雜湊，這讓您不必擔心它會檢查接收器實作的要求主體的 HTTP 標頭。

WebHook 流程通常會類似如下：

* WebHook 寄件者會公開用戶端可以訂閱的事件。 事件描述可觀察變更系統，例如新的 「 資料 」 項目已插入，完成程序或其他項目。

* WebHook 接收器訂閱註冊 WebHook，其中包含四個項目：

     1. URI，以進行事件通知的 HTTP POST 要求; 形式的公佈

     2. 一組描述，應引發 WebHook; 的特定事件的篩選

     3. 用來簽署的 HTTP POST 要求; 祕密金鑰

     4. 這是要包含在 HTTP POST 要求中的其他資料。 比方說這就可以是其他 HTTP 標頭欄位或 HTTP POST 要求主體中包含的屬性。

* 當事件發生時，會找到相符的 WebHook 註冊，並送出 HTTP POST 要求。 通常，產生的 HTTP POST 要求的是如果基於某些原因，收件者沒有回應或錯誤回應的 HTTP POST 要求導致重試數次。

## <a name="webhooks-processing-pipeline"></a>Webhook 處理管線

傳入的 Webhook 的 Microsoft ASP.NET Webhook 處理管線看起來像這樣：

![ASP.NET Webhook 處理管線](_static/WebHookReceivers.png)

兩個金鑰這裡的概念*接收者*並*處理常式*:

* *接收者*負責處理 WebHook 的特定類別，從指定的寄件者，以及強制執行安全性檢查，以確保在 WebHook 要求確認確實是預定的傳送者。

* *處理常式*通常是使用者程式碼執行處理特定的 WebHook。

下列節點中的詳細說明這些概念。
