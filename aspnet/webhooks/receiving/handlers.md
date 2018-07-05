---
uid: webhooks/receiving/handlers
title: ASP.NET Webhook 處理常式 |Microsoft Docs
author: rick-anderson
description: 如何處理 ASP.NET Webhook 的要求。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: ''
ms.openlocfilehash: 7e45a97ac9d61b2d046984e5ede3be158b741b7d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372695"
---
# <a name="aspnet-webhooks-handlers"></a>ASP.NET Webhook 處理常式

一旦 Webhook 要求由 WebHook 接收器已驗證，就準備好處理使用者程式碼。 這正是*處理常式*進來。 處理常式衍生自[IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)介面，但通常會使用[WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)而不是直接從介面衍生的類別。

WebHook 要求可以由一或多個處理常式處理。 根據其各自的順序呼叫處理常式*順序*屬性會從最低到最高順序為簡單整數 （建議為 1 到 100 之間） 的位置：

![WebHook 處理常式的順序屬性圖表](_static/Handlers.png)

可選擇性地設定處理常式*回應*WebHookHandlerContext 停止並回復為 WebHook 的 HTTP 回應傳送回應導致處理上的屬性。 在上述案例中，因為它具有較高的順序，比 B 且 B 設定的回應，將不會取得呼叫處理常式 C。

設定回應通常是只與相關的 Webhook 回應可以在其中執行資訊傳回到原始的 API。 比方說，這是傳回給 WebHook 的來源的通道公佈回應 Slack Webhook 的情況。 處理常式可以設定接收者屬性，如果他們只想要接收來自該特定接收者的 Webhook。 如果它們不需要設定的接收者針對它們全部呼叫它們。

回應的其他常見用法是使用*410 不存在*回應指出 WebHook 不再為作用中，且應該提交任何進一步的要求。

根據預設處理常式會呼叫所有的 WebHook 接收器。 不過，如果*接收者*屬性設定為處理常式的名稱，則該處理常式只會從該接收者收到 WebHook 要求。

## <a name="processing-a-webhook"></a>處理 WebHook

呼叫處理常式時，它會取得[WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs)包含 WebHook 要求的相關資訊。 的資料，通常是 HTTP 要求本文中，可從*資料*屬性。

資料類型通常是 JSON 或 HTML 表單資料，但可以轉換成更明確的類型，如有需要。 例如，ASP.NET Webhook 所產生的自訂 Webhook 可以轉換成類型[CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) ，如下所示：

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

  ## <a name="queued-processing"></a>已排入佇列的處理

如果回應不產生在數秒內，大部分的 WebHook 寄件者會重新傳送 WebHook。 這表示您的處理常式必須完成處理，為了不讓它再次呼叫該時間範圍內。

如果處理所花費的時間，或最好在個別處理然後[WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)可用來將 WebHook 要求提交至佇列，例如[Azure 儲存體佇列](https://msdn.microsoft.com/library/azure/dd179353.aspx)。

概述[WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)這裡提供實作：

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
