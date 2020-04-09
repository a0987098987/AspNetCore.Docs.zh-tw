---
title: ASP.NET核心用戶端 IP 安全性清單
author: damienbod
description: 瞭解如何編寫中間件或操作篩選器,以便根據已批准的 IP 位址清單驗證遠端 IP 位址。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/12/2020
uid: security/ip-safelist
ms.openlocfilehash: 2db879a6918245cbacff8b1a5dc15786ffab6a34
ms.sourcegitcommit: 196e4a36df5be5b04fedcff484a4261f8046ec57
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/31/2020
ms.locfileid: "80471786"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>ASP.NET核心用戶端 IP 安全性清單

由[達米安·鮑登](https://twitter.com/damien_bod)和[湯姆·戴克斯特拉](https://github.com/tdykstra)
 
本文介紹了在ASP.NET核心應用中實現IP位址安全清單(也稱為允許清單)的三種方法。 隨附的示例應用演示了所有三種方法。 您可以使用：

* 檢查每個要求的遠端 IP 位址的中間件。
* MVC 操作篩選器,用於檢查特定控制器或操作方法的請求的遠端 IP 位址。
* Razor 頁面篩選器以檢查 Razor 頁面請求的遠端 IP 位址。

在每種情況下,包含已批准的用戶端 IP 位址的字串都存儲在應用設置中。 中間件或過濾器:

* 將字串解析為陣列。 
* 檢查陣列中是否存在遠端 IP 位址。

如果陣列包含 IP 位址,則允許訪問。 否則,將返回 HTTP 403 禁止狀態代碼。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples)([如何下載](xref:index#how-to-download-a-sample))

## <a name="ip-address-safelist"></a>IP 位址安全清單

在範例應用中,IP 位址安全清單為:

* 由`AdminSafeList`*appsettings.json*檔中的屬性定義。
* 一個分號分隔字串,可能包含 Internet[協定版本 4 (IPv4)](https://wikipedia.org/wiki/IPv4)和[Internet 協定版本 6 (IPv6)](https://wikipedia.org/wiki/IPv6)位址。

[!code-json[](ip-safelist/samples/3.x/ClientIpAspNetCore/appsettings.json?range=1-3&highlight=2)]

在前面的範例中,`127.0.0.1`允許`192.168.1.5`和的 IPv4`::1`位址和 IPv6 回`0:0:0:0:0:0:0:1`回位址(壓縮格式 )。

## <a name="middleware"></a>中介軟體

該方法`Startup.Configure`將自`AdminSafeListMiddleware`定義 中間件類型添加到應用的請求管道中。 使用 .NET Core 配置提供程式檢索安全清單,並作為建構函數參數傳遞。

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureAddMiddleware)]

中間件將字串解析為陣列並搜尋陣列中的遠端 IP 位址。 如果未找到遠端 IP 位址,中間件將返回 HTTP403 禁止。 此驗證過程是繞過 HTTP GET 請求的。

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Middlewares/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>操作篩選器

如果需要針對特定 MVC 控制器或操作方法的安全列表驅動存取控制,請使用操作篩選器。 例如：

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Filters/ClientIpCheckActionFilter.cs?name=snippet_ClassOnly)]

在`Startup.ConfigureServices`中,將操作篩選器添加到 MVC 篩選器集合。 在下面的示例中,將添加`ClientIpCheckActionFilter`操作篩選器。 安全清單和控制台記錄器實例作為構造函數參數傳遞。

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesActionFilter)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesActionFilter)]

::: moniker-end

然後,操作篩選器可以應用於具有[[服務篩選器]](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute)屬性的控制器或操作方法:

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_ActionFilter&highlight=1)]

在示例應用中,操作篩選器應用於控制器的操作`Get`方法。 透過送出測試應用程式時:

* HTTP GET`[ServiceFilter]`請求, 屬性驗證用戶端 IP 位址。 如果允許存`Get`取 操作方法,操作篩選器和操作方法將產生以下主控台輸出的變體:

    ```
    dbug: ClientIpSafelistComponents.Filters.ClientIpCheckActionFilter[0]
          Remote IpAddress: ::1
    dbug: ClientIpAspNetCore.Controllers.ValuesController[0]
          successful HTTP GET    
    ```

* 除了 GET`AdminSafeListMiddleware`之外, 中間件的 HTTP 請求謂詞將驗證用戶端 IP 位址。

## <a name="razor-pages-filter"></a>剃刀頁面過濾器

如果需要 Razor Pages 應用的安全清單驅動存取控制,請使用 Razor 頁面篩選器。 例如：

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Filters/ClientIpCheckPageFilter.cs?name=snippet_ClassOnly)]

在`Startup.ConfigureServices`中,通過將 Razor 頁面添加到 MVC 篩選器集合,使其啟用該篩選器。 在下面的範例中,將添加`ClientIpCheckPageFilter`Razor 頁面篩選器。 安全清單和控制台記錄器實例作為構造函數參數傳遞。

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesPageFilter)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesPageFilter)]

::: moniker-end

當請求範例應用的*索引*Razor 頁面時,"Razor 頁面"篩選器將驗證用戶端 IP 位址。 篩選器產生以下主控台輸出的變體:

```
dbug: ClientIpSafelistComponents.Filters.ClientIpCheckPageFilter[0]
      Remote IpAddress: ::1
```

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/middleware/index>
* [動作篩選條件](xref:mvc/controllers/filters#action-filters)
* <xref:razor-pages/filter>
