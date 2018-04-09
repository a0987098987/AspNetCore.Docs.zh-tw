---
uid: webhooks/receiving/receivers
title: ASP.NET Webhook 接收者 |Microsoft 文件
author: rick-anderson
description: ASP.NET Webhook 接收者
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: a8e42521f201f88b0ed433550e8786411b4487b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-webhooks-receivers"></a>ASP.NET Webhook 接收者

接收 Webhook 取決於寄件者是誰。 有時候會有額外的步驟註冊驗證訂閱者端真的接聽 WebHook。 某些 Webhook 提供推入來提取模型的 HTTP POST 要求只包含再為獨立擷取的事件資訊的參考。 通常的安全性模型會稍微有所不同。

Microsoft ASP.NET Webhook 的目的是讓您更簡單且更一致，才能連接您的 API 而不需花許多時間了解如何處理的 Webhook 任何特定變數。

WebHook 接收者負責接受和驗證來自特定寄件者的 Webhook。 WebHook 接收者可以支援任意數目的 Webhook 各有自己的組態。 比方說，GitHub WebHook 接收者可以接受任意數目的 GitHub 儲存機制從 Webhook。

## <a name="webhook-receiver-uris"></a>WebHook 收件者 Uri

安裝 Microsoft ASP.NET Webhook 就是一般的 WebHook 控制站可接受來自服務的開放數目的 WebHook 要求。 當要求到達時，它會挑選適當的收件者來處理特定的 WebHook 寄件者一起安裝。

此控制器的 URI 是您註冊服務的 WebHook URI 和格式：

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

基於安全性理由，許多 WebHook 接收者需要的 URI 是*https* URI，在某些情況下它必須也包含其他查詢參數可用來加強僅預定的合作對象可以傳送到上述 URI 的 Webhook.

<em> <receiver> </em>元件是接收器的名稱，例如<em>github</em>或<em>slack</em>。

*{Id}*是可以用來識別特定的 WebHook 接收者組態的選擇性識別碼。 這可以用來向特定的收件者的 n 個 Webhook。 例如，下列三個 Uri 可用來為三個獨立的 Webhook 註冊：

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>安裝 WebHook 接收者

若要接收使用 Microsoft ASP.NET Webhook Webhook，先安裝 Nuget 套件的 WebHook 提供者或您想要收到 Webhook 從提供者。 Nuget 封裝會命名為[Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)的最後一個部分指示服務支援的位置。 例如

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) Webhook 接收從 GitHub 提供支援和[Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom)接收 ASP 所產生的 Webhook 提供支援。NET Webhook。

根據預設，您可以找到 Dropbox、 GitHub、 MailChimp、 PayPal、 推送、 Salesforce、 Slack、 等量磁碟區、 Trello 和 WordPress 的支援，但可支援任意數目的其他提供者。

## <a name="configuring-a-webhook-receiver"></a>設定 WebHook 接收者

透過設定 WebHook 接收者[IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs)介面，該介面的特定實作可以使用註冊相依性插入的任何模型。 預設實作會使用應用程式設定可以在 Web.config 檔案中，設定，或如果使用 Azure Web 應用程式，可以透過設定[Azure 入口網站](https://portal.azure.com/)。

![Azure 應用程式設定](_static/AzureAppSettings.png)

應用程式設定索引鍵的格式如下所示：

```
MS_WebHookReceiverSecret_<receiver>
```

值是以逗號分隔清單的值符合*{id}*值的 Webhook 已註冊，例如：

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>初始化 WebHook 接收者

WebHook 接收者會藉由註冊，通常在初始化*到 WebApiConfig*靜態類別，例如：

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
