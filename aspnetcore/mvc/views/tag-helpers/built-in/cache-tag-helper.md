---
title: ASP.NET Core MVC 中的快取標籤協助程式
author: pkellner
description: 了解如何使用快取標籤協助程式。
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 65d8bbcdaed76a308b924ba024219e8f520bb585
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85399279"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>ASP.NET Core MVC 中的快取標籤協助程式

由 [Peter Kellner](https://peterkellner.net) 提供

快取標籤協助程式可將 ASP.NET Core 應用程式內容快取至內部 ASP.NET Core 快取提供者，以提升應用程式的效能。

如需標籤協助程式的概觀，請參閱 <xref:mvc/views/tag-helpers/intro>。

下列標記會快取 Razor 目前的日期：

```cshtml
<cache>@DateTime.Now</cache>
```

第一個對包含標籤協助程式之頁面的要求會顯示目前的日期/時間。 其他要求則會顯示在快取過期 (預設為 20 分鐘) 之前或快取日期從快取撤銷之前的快取值。

## <a name="cache-tag-helper-attributes"></a>快取標籤協助程式屬性

### <a name="enabled"></a>已啟用

| 屬性類型  | 範例        | 預設 |
| --------------- | --------------- | ------- |
| Boolean         | `true`, `false` | `true`  |

`enabled` 會決定是否快取以快取標籤協助程式括住的內容。 預設值為 `true`。 如果設定為 `false`，則轉譯輸出**不會**被快取。

範例：

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-on"></a>expires-on

| 屬性類型   | 範例                            |
| ---------------- | ---------------------------------- |
| `DateTimeOffset` | `@new DateTime(2025,1,29,17,02,0)` |

`expires-on` 可設定快取項目的絕對到期日。

下列範例會快取 2025 年 1 月 29 日下午 5:02 以前的快取標籤協助程式內容：

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-after"></a>expires-after

| 屬性類型 | 範例                      | 預設    |
| -------------- | ---------------------------- | ---------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(120)` | 20 分鐘 |

`expires-after` 可設定從第一個要求時間到快取內容的時間長度。

範例：

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

RazorView 引擎會將預設 `expires-after` 值設定為20分鐘。

### <a name="expires-sliding"></a>expires-sliding

| 屬性類型 | 範例                     |
| -------------- | --------------------------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(60)` |

設定快取項目的值多久未被存取時應該撤出。

範例：

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-header"></a>vary-by-header

| 屬性類型 | 範例                                    |
| -------------- | ------------------------------------------- |
| String         | `User-Agent`, `User-Agent,content-encoding` |

`vary-by-header` 接受標頭值的逗號分隔清單，在標頭值變更時，會觸發快取重新整理。

下列範例會監視標頭值 `User-Agent`。 此範例會快取展示給網頁伺服器之每個不同 `User-Agent` 的內容：

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-query"></a>vary-by-query

| 屬性類型 | 範例             |
| -------------- | -------------------- |
| String         | `Make`, `Make,Model` |

`vary-by-query` 接受查詢字串 (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>) 中 <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> 的逗點分隔清單，當任何所列索引碼的值變更時，會觸發快取重新整理。

下列範例會監視 `Make` 與 `Model` 的值。 此範例會快取展示給網頁伺服器之每個不同 `Make` 與 `Model` 的內容：

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-route"></a>vary-by-route

| 屬性類型 | 範例             |
| -------------- | -------------------- |
| String         | `Make`, `Make,Model` |

`vary-by-route` 接受路由參數名稱的逗點分隔清單，當路由資料參數值變更時，這些路由參數名稱會觸發快取重新整理。

範例：

*Startup.cs*：

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

*Index. cshtml*：

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-cookie"></a>vary-by-cookie

| 屬性類型 | 範例                                                                         |
| -------------- | -------------------------------------------------------------------------------- |
| String         | `.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor` |

`vary-by-cookie` 接受 Cookie 名稱的逗點分隔清單，當 Cookie 值變更時，這些 Cookie 名稱會觸發快取重新整理。

下列範例會監視與 ASP.NET Core 相關聯的 cookie Identity 。 當使用者通過驗證時，cookie 中的變更會觸發快取重新整理 Identity ：

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-user"></a>vary-by-user

| 屬性類型  | 範例        | 預設 |
| --------------- | --------------- | ------- |
| Boolean         | `true`, `false` | `true`  |

`vary-by-user` 可指定當登入的使用者 (或內容主體) 變更時，是否重設快取。 目前的使用者也稱為要求內容主體，而且可以藉 Razor 由參考在視圖中查看 `@User.Identity.Name` 。

下列範例會監視目前登入的使用者，以觸發快取重新整理：

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

使用此屬性可保留登入與登出週期中的快取內容。 當此值設定為 `true` 時，驗證週期將使已驗證之使用者的快取無效。 快取失效是由於使用者通過驗證時會產生新的唯一 Cookie 值所致。 如果沒有任何 Cookie 或 Cookie 已過期，則會以匿名狀態保留快取。 如果使用者**未**通過驗證，則會維護快取。

### <a name="vary-by"></a>vary-by

| 屬性類型 | 範例  |
| -------------- | -------- |
| String         | `@Model` |

`vary-by` 可自訂要快取哪些資料。 當屬性字串值所參考的物件變更時，就會更新快取標籤協助程式的內容。 通常會對此屬性指派模型值的字串串連。 實際上，這會導致更新任何串連值都會使快取失效的情節。

下列範例假設轉譯檢視的控制器方法加總兩個路由參數 (`myParam1` 與 `myParam2`) 的整數值，並以單一模型屬性傳回加總。 當此總和變更時，快取標籤協助程式的內容會重新轉譯和快取。  

動作：

```csharp
public IActionResult Index(string myParam1, string myParam2, string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

*Index. cshtml*：

```cshtml
<cache vary-by="@Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="priority"></a>priority

| 屬性類型      | 範例                               | 預設  |
| ------------------- | -------------------------------------- | -------- |
| `CacheItemPriority` | `High`, `Low`, `NeverRemove`, `Normal` | `Normal` |

`priority` 可提供快取撤銷指導方針給內建快取提供者。 當記憶體不足時，網頁伺服器會先撤出 `Low` 快取項目。

範例：

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority` 屬性並不保證特定快取保留層級。 `CacheItemPriority` 僅供建議。 將此屬性設定為 `NeverRemove` 並不保證一定會保留快取項目。 請參閱[其他資源](#additional-resources)一節中的主題，以取得詳細資訊。

快取標籤協助程式相依於[記憶體快取服務](xref:performance/caching/memory)。 如果尚未新增此服務，快取標籤協助程式會予以新增。

## <a name="additional-resources"></a>其他資源

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
