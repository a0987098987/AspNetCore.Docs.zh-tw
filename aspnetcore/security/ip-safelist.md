---
title: 適用於 ASP.NET Core 的用戶端 IP 安全清單
author: damienbod
description: 了解如何撰寫中介軟體或動作篩選條件來驗證遠端 IP 位址，根據核准的 IP 位址的清單。
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: ca05989efabea3a71c6912e98055a6746e0f5966
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223925"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>適用於 ASP.NET Core 的用戶端 IP 安全清單

藉由[Damien Bowden](https://twitter.com/damien_bod)和[Tom Dykstra](https://github.com/tdykstra)
 
本文說明三種方式可在 ASP.NET Core 應用程式中實作 IP 安全清單 （也稱為允許清單）。 您可以使用：

* 若要檢查每個要求的遠端 IP 位址的中介軟體。
* 若要檢查遠端 IP 位址的要求特定的控制器或動作方法的動作篩選條件。
* Razor 頁面的篩選條件來檢查遠端 IP 位址的 Razor 頁面的要求。

在每個案例中，包含已核准的用戶端 IP 位址的字串會儲存在應用程式設定。 中介軟體或篩選器會將字串剖析成清單，並檢查 遠端 IP 是否在清單中。 如果沒有，則會傳回 HTTP 403 禁止狀態碼。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="the-safelist"></a>安全清單

在設定的清單*appsettings.json*檔案。 它是以分號分隔的清單，並可包含 IPv4 和 IPv6 位址。

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a>中介軟體

`Configure`方法新增中介軟體，並在建構函式參數傳遞給它的安全清單的字串。

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=10)]

中介軟體會將字串剖析成陣列，並尋找陣列中的遠端 IP 位址。 如果找不到的遠端 IP 位址中, 介軟體會傳回 HTTP 401 禁止。 HTTP Get 要求，會略過此驗證程序。

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>動作篩選條件

如果您想安全清單僅適用於特定的控制器或動作方法時，請使用動作篩選條件。 以下為範例： 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

動作篩選條件加入至服務容器。

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

篩選可用的控制器或動作方法上。

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

在範例應用程式中，篩選會套用至`Get`方法。 因此，當您測試應用程式傳送`Get`API 要求，該屬性，就會檢查用戶端 IP 位址。 當您測試與任何其他 HTTP 方法呼叫 API 時中, 介軟體就驗證的用戶端 IP。

## <a name="razor-pages-filter"></a>Razor 頁面篩選 

如果您想要的 Razor 頁面應用程式安全清單，請使用 Razor 頁面篩選條件。 以下為範例： 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

將它新增至 MVC 篩選條件集合時，會啟用此篩選器。

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

當您執行應用程式，並要求 Razor 頁面時，Razor 頁面篩選條件就驗證的用戶端 IP。

## <a name="next-steps"></a>後續步驟

[深入了解 ASP.NET Core 中介軟體](xref:fundamentals/middleware/index)。
