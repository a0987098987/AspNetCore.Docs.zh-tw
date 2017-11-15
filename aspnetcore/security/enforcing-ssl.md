---
title: "強制 ASP.NET Core 應用程式執行 SSL 連線"
author: rick-anderson
description: "示範如何在 ASP.NET Core web 應用程式使用 SSL 連線"
keywords: "ASP.NET Core、 SSL、 HTTPS、 RequireHttpsAttribute、 IIS Express"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 6f2755a606000717ca8a57f045b1ef613c7f14f6
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>強制 ASP.NET Core 應用程式執行 SSL 連線

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本文件說明如何：

- 設定所有請求都需要 SSL （HTTPS 要求）。
- 將所有 HTTP 要求重新都導向至 HTTPS。

## <a name="require-ssl"></a>需要 SSL

使用[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute)可將有裝飾此屬性的控制器或方法，設定成需要使用 SSL 連線才能執行，或者您也可以將它套用至全域，如下所示：

將下列程式碼加入`ConfigureServices`中的`Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

上述反白顯示的程式碼將設定所有要求都需要使用`HTTPS`，因此 HTTP 要求都會被忽略。下列反白顯示的程式碼會將所有 HTTP 要求重新都導向至 HTTPS:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

如需詳細資訊，請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)。

全域使用 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是安全性最佳作法。 套用`[RequireHttps]`屬性至所有控制器，不會比設定全域需要 HTTPS 來的安全，因為您無法保證新增控制器至您的應用程式時，會記住要套用`[RequireHttps]`屬性。
