---
title: "強制執行的 ASP.NET Core 應用程式中的 SSL"
author: rick-anderson
description: "示範如何要求使用 SSL 在 ASP.NET Core web 應用程式"
keywords: "ASP.NET Core、 SSL、 HTTPS、 RequireHttpsAttribute、 IIS Express"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 6f2755a606000717ca8a57f045b1ef613c7f14f6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="099f0-104">強制執行的 ASP.NET Core 應用程式中的 SSL</span><span class="sxs-lookup"><span data-stu-id="099f0-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="099f0-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="099f0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="099f0-106">本文件說明如何：</span><span class="sxs-lookup"><span data-stu-id="099f0-106">This document shows how to:</span></span>

- <span data-ttu-id="099f0-107">需要 SSL （HTTPS 要求） 的所有要求。</span><span class="sxs-lookup"><span data-stu-id="099f0-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="099f0-108">將所有 HTTP 要求重新都導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="099f0-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="099f0-109">需要 SSL</span><span class="sxs-lookup"><span data-stu-id="099f0-109">Require SSL</span></span>

<span data-ttu-id="099f0-110">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute)用來要求 SSL。</span><span class="sxs-lookup"><span data-stu-id="099f0-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="099f0-111">可以用來裝飾控制器或具有此屬性的方法，或者您可以將它套用全域如下所示：</span><span class="sxs-lookup"><span data-stu-id="099f0-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="099f0-112">將下列程式碼加入`ConfigureServices`中`Startup`:</span><span class="sxs-lookup"><span data-stu-id="099f0-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="099f0-113">上述反白顯示的程式碼需要的所有要求都使用`HTTPS`，因此 HTTP 要求都會被忽略。</span><span class="sxs-lookup"><span data-stu-id="099f0-113">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="099f0-114">下列反白顯示的程式碼會將所有 HTTP 要求重新都導向至 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="099f0-114">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="099f0-115">請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="099f0-115">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="099f0-116">全域需要 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是安全性最佳作法。</span><span class="sxs-lookup"><span data-stu-id="099f0-116">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="099f0-117">套用`[RequireHttps]`所有控制器的屬性不會視為為全域需要 HTTPS 的安全。</span><span class="sxs-lookup"><span data-stu-id="099f0-117">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="099f0-118">您無法保證新的控制站新增至您的應用程式將會記住要套用`[RequireHttps]`屬性。</span><span class="sxs-lookup"><span data-stu-id="099f0-118">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
