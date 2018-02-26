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
# <a name="enforcing-https-in-an-aspnet-core-app"></a>ASP.NET Core 應用程式中強制使用 HTTPS

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本文件說明如何：

- 所有要求需要 HTTPS。
- 將所有 HTTP 要求重新都導向至 HTTPS。

> [!WARNING]
> 請勿**不**使用`RequireHttpsAttribute`上接收機密資訊的 Web Api。 `RequireHttpsAttribute`從 HTTP 至 HTTPS 的瀏覽器重新導向會使用 HTTP 狀態碼。 API 用戶端可能不了解，或是遵循從 HTTP 重新導向至 HTTPS。 這類用戶端可能會透過 HTTP 傳送資訊。 Web 應用程式開發介面應執行下列之一：
>
>* 不在 HTTP 上接聽。
>* 關閉與狀態碼 400 （不正確的要求） 的連線，並不會處理要求。

## <a name="require-https"></a>需要 HTTPS

[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute)用來要求 HTTPS。 `[RequireHttpsAttribute]`可以裝飾控制器或方法，或可以全域套用。 若要全域套用的屬性，加入下列程式碼加入`ConfigureServices`中`Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

上述的反白顯示程式碼需要的所有要求都使用`HTTPS`; 因此，HTTP 要求會被忽略。 而下列反白顯示的程式碼會將所有 HTTP 要求都重新導向至 HTTPS:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

如需詳細資訊，請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)。

全域使用 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是安全性最佳作法。 將`[RequireHttps]`屬性套用至所有控制器，不會比全域使用 HTTPS 來的安全。 您無法保證`[RequireHttps]`加入新的控制器和 Razor 頁面時，屬性會套用。