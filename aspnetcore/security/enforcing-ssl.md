---
title: "強制執行的 ASP.NET Core 應用程式中的 SSL"
author: rick-anderson
description: "示範如何要求使用 SSL 在 ASP.NET Core web 應用程式"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 42d8767fda2d3f3545876f8ca18e0f6fbe6741b8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="15527-103">強制執行的 ASP.NET Core 應用程式中的 SSL</span><span class="sxs-lookup"><span data-stu-id="15527-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="15527-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="15527-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="15527-105">本文件說明如何：</span><span class="sxs-lookup"><span data-stu-id="15527-105">This document shows how to:</span></span>

- <span data-ttu-id="15527-106">需要 SSL （HTTPS 要求） 的所有要求。</span><span class="sxs-lookup"><span data-stu-id="15527-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="15527-107">將所有 HTTP 要求重新都導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="15527-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="15527-108">需要 SSL</span><span class="sxs-lookup"><span data-stu-id="15527-108">Require SSL</span></span>

<span data-ttu-id="15527-109">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute)用來要求 SSL。</span><span class="sxs-lookup"><span data-stu-id="15527-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="15527-110">可以用來裝飾控制器或具有此屬性的方法，或者您可以將它套用全域如下所示：</span><span class="sxs-lookup"><span data-stu-id="15527-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="15527-111">將下列程式碼加入`ConfigureServices`中`Startup`:</span><span class="sxs-lookup"><span data-stu-id="15527-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="15527-112">上述反白顯示的程式碼需要的所有要求都使用`HTTPS`，因此 HTTP 要求都會被忽略。</span><span class="sxs-lookup"><span data-stu-id="15527-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="15527-113">下列反白顯示的程式碼會將所有 HTTP 要求重新都導向至 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="15527-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="15527-114">請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="15527-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="15527-115">全域需要 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是安全性最佳作法。</span><span class="sxs-lookup"><span data-stu-id="15527-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="15527-116">套用`[RequireHttps]`所有控制器的屬性不會視為為全域需要 HTTPS 的安全。</span><span class="sxs-lookup"><span data-stu-id="15527-116">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="15527-117">您無法保證新的控制站新增至您的應用程式將會記住要套用`[RequireHttps]`屬性。</span><span class="sxs-lookup"><span data-stu-id="15527-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
