---
title: "ASP.NET Core 應用程式中強制使用 HTTPS"
author: rick-anderson
description: "示範如何要求 HTTPS/TLS 中 ASP.NET Core web 應用程式。"
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 636077ea21581716308384ebf8d47c1e417a256a
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a><span data-ttu-id="f38ff-103">ASP.NET Core 應用程式中強制使用 HTTPS</span><span class="sxs-lookup"><span data-stu-id="f38ff-103">Enforcing HTTPS in an ASP.NET Core app</span></span>

<span data-ttu-id="f38ff-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f38ff-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f38ff-105">本文件說明如何：</span><span class="sxs-lookup"><span data-stu-id="f38ff-105">This document shows how to:</span></span>

- <span data-ttu-id="f38ff-106">所有要求需要 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="f38ff-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="f38ff-107">將所有 HTTP 要求重新都導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="f38ff-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="f38ff-108">請勿**不**使用`RequireHttpsAttribute`上接收機密資訊的 Web Api。</span><span class="sxs-lookup"><span data-stu-id="f38ff-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="f38ff-109">`RequireHttpsAttribute`從 HTTP 至 HTTPS 的瀏覽器重新導向會使用 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f38ff-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="f38ff-110">API 用戶端可能不了解，或是遵循從 HTTP 重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="f38ff-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="f38ff-111">這類用戶端可能會透過 HTTP 傳送資訊。</span><span class="sxs-lookup"><span data-stu-id="f38ff-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="f38ff-112">Web 應用程式開發介面應執行下列之一：</span><span class="sxs-lookup"><span data-stu-id="f38ff-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="f38ff-113">不在 HTTP 上接聽。</span><span class="sxs-lookup"><span data-stu-id="f38ff-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="f38ff-114">關閉與狀態碼 400 （不正確的要求） 的連線，並不會處理要求。</span><span class="sxs-lookup"><span data-stu-id="f38ff-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="f38ff-115">需要 HTTPS</span><span class="sxs-lookup"><span data-stu-id="f38ff-115">Require HTTPS</span></span>

<span data-ttu-id="f38ff-116">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute)用來要求 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="f38ff-116">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="f38ff-117">`[RequireHttpsAttribute]`可以裝飾控制器或方法，或可以全域套用。</span><span class="sxs-lookup"><span data-stu-id="f38ff-117">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="f38ff-118">若要全域套用的屬性，加入下列程式碼加入`ConfigureServices`中`Startup`:</span><span class="sxs-lookup"><span data-stu-id="f38ff-118">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="f38ff-119">上述的反白顯示程式碼需要的所有要求都使用`HTTPS`; 因此，HTTP 要求會被忽略。</span><span class="sxs-lookup"><span data-stu-id="f38ff-119">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="f38ff-120">而下列反白顯示的程式碼會將所有 HTTP 要求都重新導向至 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="f38ff-120">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="f38ff-121">如需詳細資訊，請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="f38ff-121">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="f38ff-122">全域使用 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是安全性最佳作法。</span><span class="sxs-lookup"><span data-stu-id="f38ff-122">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="f38ff-123">套用`[RequireHttps]`所有控制器/Razor 頁面的屬性不被視為安全全域需要 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="f38ff-123">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="f38ff-124">您無法保證`[RequireHttps]`加入新的控制器和 Razor 頁面時，屬性會套用。</span><span class="sxs-lookup"><span data-stu-id="f38ff-124">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>