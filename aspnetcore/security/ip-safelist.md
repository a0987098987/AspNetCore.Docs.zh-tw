---
title: ASP.NET Core 的用戶端 IP 安全
author: damienbod
description: 瞭解如何撰寫中介軟體或動作篩選器，以根據核准的 IP 位址清單來驗證遠端 IP 位址。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/12/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/ip-safelist
ms.openlocfilehash: 7923a81e72124cfb0e11e3c1ac327c1e32194b21
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776496"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>ASP.NET Core 的用戶端 IP 安全

By [Damien Bowden](https://twitter.com/damien_bod)和[Tom 作者: dykstra](https://github.com/tdykstra)
 
本文說明三種在 ASP.NET Core 應用程式中執行 IP 位址安全清單（也稱為允許清單）的方式。 隨附的範例應用程式會示範這三種方法。 您可以使用：

* 中介軟體，以檢查每個要求的遠端 IP 位址。
* MVC 動作篩選準則，以檢查特定控制器或動作方法的遠端 IP 位址要求。
* Razor頁面篩選以檢查Razor頁面要求的遠端 IP 位址。

在每個案例中，包含已核准用戶端 IP 位址的字串會儲存在應用程式設定中。 中介軟體或篩選：

* 將字串剖析為數組。 
* 檢查陣列中是否有遠端 IP 位址。

如果陣列包含 IP 位址，則允許存取。 否則，會傳回 HTTP 403 禁止狀態碼。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="ip-address-safelist"></a>IP 位址的安全

在範例應用程式中，IP 位址的「安全」是：

* 由 appsettings 中`AdminSafeList`的屬性所*appsettings.json*定義。
* 以分號分隔的字串，其中可能包含[網際網路通訊協定第4版（IPv4）](https://wikipedia.org/wiki/IPv4)和[網際網路通訊協定第6版（IPv6）](https://wikipedia.org/wiki/IPv6)位址。

[!code-json[](ip-safelist/samples/3.x/ClientIpAspNetCore/appsettings.json?range=1-3&highlight=2)]

在上述範例中， `127.0.0.1`允許和`192.168.1.5`的 IPv4 位址和的 IPv6 回送位址`::1` （的壓縮格式`0:0:0:0:0:0:0:1`）。

## <a name="middleware"></a>中介軟體

`Startup.Configure`方法會將自訂`AdminSafeListMiddleware`中介軟體類型新增至應用程式的要求管線。 您可使用 .NET Core 設定提供者來抓取安全，並將其當做「函式參數」傳遞。

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureAddMiddleware)]

中介軟體會將字串剖析為數組，並在陣列中搜尋遠端 IP 位址。 如果找不到遠端 IP 位址，中介軟體會傳回 HTTP 403 禁止。 針對 HTTP GET 要求，會略過此驗證程式。

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Middlewares/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>動作篩選準則

如果您想要針對特定的 MVC 控制器或動作方法，使用以安全的方式導向的存取控制，請使用動作篩選準則。 例如：

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Filters/ClientIpCheckActionFilter.cs?name=snippet_ClassOnly)]

在`Startup.ConfigureServices`中，將動作篩選準則新增至 MVC 篩選集合。 在下列範例中，會`ClientIpCheckActionFilter`加入動作篩選準則。 「安全性記錄檔」和「主控台記錄器」實例會當做「函式參數」傳遞。

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesActionFilter)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesActionFilter)]

::: moniker-end

接著，您可以使用[[ServiceFilter]](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute)屬性，將動作篩選套用至控制器或動作方法：

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_ActionFilter&highlight=1)]

在範例應用程式中，動作篩選準則會套用至控制器的`Get`動作方法。 當您藉由傳送來測試應用程式時：

* HTTP GET 要求， `[ServiceFilter]`屬性會驗證用戶端 IP 位址。 如果允許存取`Get`動作方法，則動作篩選和動作方法會產生下列主控台輸出的變化：

    ```
    dbug: ClientIpSafelistComponents.Filters.ClientIpCheckActionFilter[0]
          Remote IpAddress: ::1
    dbug: ClientIpAspNetCore.Controllers.ValuesController[0]
          successful HTTP GET    
    ```

* GET 以外的`AdminSafeListMiddleware` HTTP 要求動詞，中介軟體會驗證用戶端 IP 位址。

## <a name="razor-pages-filter"></a>Razor頁面篩選

如果您想要使用Razor網頁應用程式的 [以安全] 為導向Razor的存取控制，請使用 [頁面] 篩選準則。 例如：

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Filters/ClientIpCheckPageFilter.cs?name=snippet_ClassOnly)]

在`Startup.ConfigureServices`中，藉Razor由將頁面篩選新增至 MVC 篩選器集合來啟用它們。 在下列範例中， `ClientIpCheckPageFilter` Razor會加入頁面篩選。 「安全性記錄檔」和「主控台記錄器」實例會當做「函式參數」傳遞。

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesPageFilter)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesPageFilter)]

::: moniker-end

要求範例應用程式的 [*索引* Razor ] 頁面時， Razor [頁面] 篩選準則會驗證用戶端 IP 位址。 此篩選會產生下列主控台輸出的變化：

```
dbug: ClientIpSafelistComponents.Filters.ClientIpCheckPageFilter[0]
      Remote IpAddress: ::1
```

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/middleware/index>
* [動作篩選條件](xref:mvc/controllers/filters#action-filters)
* <xref:razor-pages/filter>
