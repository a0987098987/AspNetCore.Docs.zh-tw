---
title: ASP.NET Core 的用戶端 IP 安全
author: damienbod
description: 瞭解如何撰寫中介軟體或動作篩選器, 以根據核准的 IP 位址清單來驗證遠端 IP 位址。
ms.author: riande
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 02e44135ab1742d44691cfda8c4167f21d6efa4e
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975652"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>ASP.NET Core 的用戶端 IP 安全

By [Damien Bowden](https://twitter.com/damien_bod)和[Tom 作者: dykstra](https://github.com/tdykstra)
 
本文說明三種在 ASP.NET Core 應用程式中執行 IP 安全清單 (也稱為「白名單」) 的方式。 您可以使用:

* 中介軟體, 以檢查每個要求的遠端 IP 位址。
* 動作篩選準則, 以檢查特定控制器或動作方法的遠端 IP 位址要求。
* Razor Pages 篩選準則, 以檢查 Razor 頁面要求的遠端 IP 位址。

在每個案例中, 包含已核准用戶端 IP 位址的字串會儲存在應用程式設定中。 中介軟體或篩選器會將字串剖析為清單, 並檢查遠端 IP 是否在清單中。 如果不是, 則會傳回 HTTP 403 禁止狀態碼。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="the-safelist"></a>安全的

此清單會在*appsettings*中設定。 它是以分號分隔的清單, 而且可以包含 IPv4 和 IPv6 位址。

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a>中介軟體

`Configure`方法會新增中介軟體, 並在函式參數中將安全檔案字串傳遞給它。

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=10)]

中介軟體會將字串剖析為數組, 並在陣列中尋找遠端 IP 位址。 如果找不到遠端 IP 位址, 中介軟體會傳回 HTTP 401 禁止。 針對 HTTP Get 要求, 會略過此驗證程式。

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>動作篩選準則

如果您只想要特定的控制器或動作方法的安全, 請使用動作篩選準則。 以下為範例： 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

動作篩選準則會新增至服務容器。

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

然後, 您可以在控制器或動作方法上使用此篩選器。

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

在範例應用程式中, 篩選準則會套用至`Get`方法。 因此, 當您藉由傳送`Get` API 要求來測試應用程式時, 屬性會驗證用戶端 IP 位址。 當您使用任何其他 HTTP 方法呼叫 API 來進行測試時, 中介軟體會驗證用戶端 IP。

## <a name="razor-pages-filter"></a>Razor Pages 篩選 

如果您想要 Razor Pages 應用程式的安全, 請使用 Razor Pages 篩選器。 以下為範例： 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

藉由將此篩選準則新增至 MVC 篩選器集合, 即可加以啟用。

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

當您執行應用程式並要求 Razor 頁面時, Razor Pages 篩選器會驗證用戶端 IP。

## <a name="next-steps"></a>後續步驟

[深入瞭解 ASP.NET Core 中介軟體](xref:fundamentals/middleware/index)。
