---
uid: webhooks/receiving/receivers
title: "ASP.NET Webhook 接收者 |Microsoft 文件"
author: rick-anderson
description: "ASP.NET Webhook 接收者"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 8c42db4056dd7a6ef77c7bcbc0eca3b5bf7c87e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="4bff7-103">ASP.NET Webhook 接收者</span><span class="sxs-lookup"><span data-stu-id="4bff7-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="4bff7-104">接收 Webhook 取決於寄件者是誰。</span><span class="sxs-lookup"><span data-stu-id="4bff7-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="4bff7-105">有時候會有額外的步驟註冊驗證訂閱者端真的接聽 WebHook。</span><span class="sxs-lookup"><span data-stu-id="4bff7-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="4bff7-106">某些 Webhook 提供推入來提取模型的 HTTP POST 要求只包含再為獨立擷取的事件資訊的參考。</span><span class="sxs-lookup"><span data-stu-id="4bff7-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="4bff7-107">通常的安全性模型會稍微有所不同。</span><span class="sxs-lookup"><span data-stu-id="4bff7-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="4bff7-108">Microsoft ASP.NET Webhook 的目的是讓您更簡單且更一致，才能連接您的 API 而不需花許多時間了解如何處理的 Webhook 任何特定變數。</span><span class="sxs-lookup"><span data-stu-id="4bff7-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="4bff7-109">WebHook 接收者負責接受和驗證來自特定寄件者的 Webhook。</span><span class="sxs-lookup"><span data-stu-id="4bff7-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="4bff7-110">WebHook 接收者可以支援任意數目的 Webhook 各有自己的組態。</span><span class="sxs-lookup"><span data-stu-id="4bff7-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="4bff7-111">比方說，GitHub WebHook 接收者可以接受任意數目的 GitHub 儲存機制從 Webhook。</span><span class="sxs-lookup"><span data-stu-id="4bff7-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="4bff7-112">WebHook 收件者 Uri</span><span class="sxs-lookup"><span data-stu-id="4bff7-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="4bff7-113">安裝 Microsoft ASP.NET Webhook 就是一般的 WebHook 控制站可接受來自服務的開放數目的 WebHook 要求。</span><span class="sxs-lookup"><span data-stu-id="4bff7-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="4bff7-114">當要求到達時，它會挑選適當的收件者來處理特定的 WebHook 寄件者一起安裝。</span><span class="sxs-lookup"><span data-stu-id="4bff7-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="4bff7-115">此控制器的 URI 是您註冊服務的 WebHook URI 和格式：</span><span class="sxs-lookup"><span data-stu-id="4bff7-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="4bff7-116">基於安全性理由，許多 WebHook 接收者需要的 URI 是*https* URI，在某些情況下它必須也包含其他查詢參數可用來加強僅預定的合作對象可以傳送到上述 URI 的 Webhook.</span><span class="sxs-lookup"><span data-stu-id="4bff7-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="4bff7-117"> *<receiver>* 元件是接收器的名稱，例如*github*或*slack*。</span><span class="sxs-lookup"><span data-stu-id="4bff7-117">The *<receiver>* component is the name of the receiver, for example *github* or *slack*.</span></span>

<span data-ttu-id="4bff7-118">*{Id}*是可以用來識別特定的 WebHook 接收者組態的選擇性識別碼。</span><span class="sxs-lookup"><span data-stu-id="4bff7-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="4bff7-119">這可以用來向特定的收件者的 n 個 Webhook。</span><span class="sxs-lookup"><span data-stu-id="4bff7-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="4bff7-120">例如，下列三個 Uri 可用來為三個獨立的 Webhook 註冊：</span><span class="sxs-lookup"><span data-stu-id="4bff7-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="4bff7-121">安裝 WebHook 接收者</span><span class="sxs-lookup"><span data-stu-id="4bff7-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="4bff7-122">若要接收使用 Microsoft ASP.NET Webhook Webhook，先安裝 Nuget 套件的 WebHook 提供者或您想要收到 Webhook 從提供者。</span><span class="sxs-lookup"><span data-stu-id="4bff7-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="4bff7-123">Nuget 封裝會命名為[Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)的最後一個部分指示服務支援的位置。</span><span class="sxs-lookup"><span data-stu-id="4bff7-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="4bff7-124">例如</span><span class="sxs-lookup"><span data-stu-id="4bff7-124">For example</span></span>

<span data-ttu-id="4bff7-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) Webhook 接收從 GitHub 提供支援和[Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom)接收 ASP 所產生的 Webhook 提供支援。NET Webhook。</span><span class="sxs-lookup"><span data-stu-id="4bff7-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="4bff7-126">根據預設，您可以找到 Dropbox、 GitHub、 MailChimp、 PayPal、 推送、 Salesforce、 Slack、 等量磁碟區、 Trello 和 WordPress 的支援，但可支援任意數目的其他提供者。</span><span class="sxs-lookup"><span data-stu-id="4bff7-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="4bff7-127">設定 WebHook 接收者</span><span class="sxs-lookup"><span data-stu-id="4bff7-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="4bff7-128">透過設定 WebHook 接收者[IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs)介面，該介面的特定實作可以使用註冊相依性插入的任何模型。</span><span class="sxs-lookup"><span data-stu-id="4bff7-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="4bff7-129">預設實作會使用應用程式設定可以在 Web.config 檔案中，設定，或如果使用 Azure Web 應用程式，可以透過設定[Azure 入口網站](https://portal.azure.com/)。</span><span class="sxs-lookup"><span data-stu-id="4bff7-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Azure 應用程式設定](_static/AzureAppSettings.png)

<span data-ttu-id="4bff7-131">應用程式設定索引鍵的格式如下所示：</span><span class="sxs-lookup"><span data-stu-id="4bff7-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="4bff7-132">值是以逗號分隔清單的值符合*{id}*值的 Webhook 已註冊，例如：</span><span class="sxs-lookup"><span data-stu-id="4bff7-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="4bff7-133">初始化 WebHook 接收者</span><span class="sxs-lookup"><span data-stu-id="4bff7-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="4bff7-134">WebHook 接收者會藉由註冊，通常在初始化*到 WebApiConfig*靜態類別，例如：</span><span class="sxs-lookup"><span data-stu-id="4bff7-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
