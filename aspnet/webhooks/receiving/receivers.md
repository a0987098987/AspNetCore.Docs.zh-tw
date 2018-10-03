---
uid: webhooks/receiving/receivers
title: ASP.NET Webhook 接收器 |Microsoft Docs
author: rick-anderson
description: ASP.NET Webhook 接收器
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: d771a588b23abcd7b1b33e694af17b219683fc48
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860910"
---
# <a name="aspnet-webhooks-receivers"></a>ASP.NET Webhook 接收器

接收 Webhook 寄件者是誰而定。 有時會註冊 WebHook，驗證 「 訂閱者 」 心聲的額外步驟。 某些 Webhook 提供推播到提取模型的 HTTP POST 要求只包含且獨立擷取已將事件資訊的參考。 通常的安全性模型而稍微有所不同。

Microsoft ASP.NET Webhook 的目的是使其更簡單和更一致，才能接到您的 API，而不需花費大量時間找出如何處理 Webhook 的任何特定變數。

WebHook 的接收者負責認可和驗證來自特定寄件者的 Webhook。 WebHook 接收器可支援任意數目的 Webhook，每個都有自己的組態。 比方說，GitHub WebHook 接收器可以接受任意數目的 GitHub 存放庫中的 Webhook。

## <a name="webhook-receiver-uris"></a>WebHook 接收器 Uri

藉由安裝 Microsoft ASP.NET Webhook，您會取得一般的 WebHook 控制器可接受來自服務的開放式數目的 WebHook 要求。 當要求抵達時，它會挑選適當的接收者，您已安裝用於處理特定的 WebHook 寄件者。

此控制器的 URI 是您向服務註冊的 WebHook URI，並屬於表單：

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

基於安全性理由，需要的 URI 是許多的 WebHook 接收器*https* URI 且在某些情況下也必須包含的其他查詢參數來強制執行，只有預定的合作對象可以傳送給上述 URI 的 Webhook.

`<receiver>`元件是名稱的接收者，例如`github`或`slack`。

*{Id}* 是選擇性的識別碼，可用來識別特定的 WebHook 接收器組態。 這可用來向特定接收者的 n 個 Webhook。 比方說，下列三個 Uri 可用來對三個獨立的 Webhook 註冊：

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>安裝 WebHook 接收器

若要接收使用 Microsoft ASP.NET Webhook 的 Webhook，您會先安裝 WebHook 提供者或您想要接收從 Webhook 提供者 Nuget 封裝。 Nuget 封裝的名稱[Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)的最後一個部分指示服務支援的位置。 例如

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub)提供支援 Webhook 接收 GitHub 並[Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom)接收 ASP 所產生的 Webhook 提供支援。NET Webhook。

內建中，您可以找到 Dropbox、 GitHub、 MailChimp、 PayPal、 Pusher、 Salesforce、 Slack、 等量磁碟區、 Trello 和 WordPress 的支援，但可支援任意數目的其他提供者。

## <a name="configuring-a-webhook-receiver"></a>設定 WebHook 接收器

已透過 WebHook 接收器[IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs)介面和特定實作該介面可以使用註冊任何相依性插入模式。 預設實作會使用應用程式設定可以在 Web.config 檔案中，設定，或如果使用 Azure Web 應用程式，可以透過設定[Azure 入口網站](https://portal.azure.com/)。

![Azure 應用程式設定](_static/AzureAppSettings.png)

應用程式設定索引鍵的格式如下所示：

```
MS_WebHookReceiverSecret_<receiver>
```

值是逗號分隔的清單相符的值 *{id}* 值的 Webhook 已註冊，例如：

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>初始化 WebHook 接收器

通常在中註冊它們，會初始化的 WebHook 接收器*WebApiConfig*靜態類別，例如：

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
