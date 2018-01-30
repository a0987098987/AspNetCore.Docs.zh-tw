---
uid: webhooks/receiving/handlers
title: "ASP.NET Webhook 處理常式 |Microsoft 文件"
author: rick-anderson
description: "如何處理要求中 ASP.NET Webhook。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 4cf5770a731ef77842eb29b0a66ee0aac5d85d85
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-webhooks-handlers"></a>ASP.NET Webhook 處理常式

一旦 Webhook 要求已通過驗證 WebHook 接收者，它可由使用者程式碼所處理。 這是 where*處理常式*出現。 處理常式是衍生自[IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)介面，但通常會使用[WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)而不是直接從介面衍生的類別。

WebHook 要求可以處理由一或多個處理常式。 呼叫處理常式的順序根據其各自*順序*屬性會從最低到最高順序其中簡單的整數 （介於 1 到 100 之間的建議）：

![WebHook 處理常式的順序屬性圖表](_static/Handlers.png)

處理常式可以選擇性地設定*回應*WebHookHandlerContext 會停止與要傳回的 HTTP 回應對 WebHook 送回應會導致處理上的屬性。 在上述情況中，因為它具有較高的順序比 B，而 B 設定的回應，將不會取得呼叫處理常式的 C。

設定回應通常是只有相關的回應可以在其中包含資訊傳回到原始的應用程式開發介面的 Webhook。 比方說，這是與 Slack 的 Webhook 回應回傳至通道 WebHook 的來源位置的情況。 處理常式可以設定接收者屬性，如果他們只想要從該特定接收者接收 Webhook。 如果它們不需要設定全部都在呼叫的接收器。

其他常見用法之一的回應是使用*410 已移走*表示 WebHook 不再是使用中，以及任何進一步的要求應該提交的回應。

根據預設，會由所有 WebHook 接收者呼叫的處理常式。 不過，如果*接收者*屬性設定為處理常式的名稱，則該處理常式只會接收來自該收件者的 WebHook 要求。

## <a name="processing-a-webhook"></a>處理 WebHook

呼叫處理常式時，它會取得[WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs)包含 WebHook 要求的相關資訊。 資料，通常是 HTTP 要求主體中，都可以從*資料*屬性。

資料類型通常是 JSON 或 HTML 表單資料，但可能有需要，轉型為更特定的類型。 例如，可以自訂 ASP.NET Webhook 所產生的 Webhook 轉換成類型[CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) ，如下所示：

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a>排入佇列的處理

如果不產生回應，少數幾個秒內，大多數的 WebHook 寄件者將會重新傳送 WebHook。 這表示您的處理常式必須完成才能讓它再次呼叫不在該時間範圍內處理。

如果處理所花的時間，或最好在個別處理則[WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)可用 WebHook 將要求提交到佇列中，例如[Azure 儲存體佇列](https://msdn.microsoft.com/library/azure/dd179353.aspx)。

外的框[WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)此處提供的實作：

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
