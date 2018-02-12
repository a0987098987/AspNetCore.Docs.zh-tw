---
title: "ASP.NET Core MVC 中的快取標籤協助程式"
author: pkellner
description: "示範如何使用快取標籤協助程式"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 51811ee1669a24a0fc4ce9bc67e782b61bff655c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>ASP.NET Core MVC 中的快取標籤協助程式

由 [Peter Kellner](http://peterkellner.net) 提供 

快取標籤協助程式可將 ASP.NET Core 應用程式內容快取至內部 ASP.NET Core 快取提供者，因此大幅提升應用程式的效能。

Razor 檢視引擎將預設的 `expires-after` 設定為 20 分鐘。

下列 Razor 標記會快取日期/時間：

```cshtml
<cache>@DateTime.Now</cache>
```

第一個對包含 `CacheTagHelper` 之頁面的要求會顯示目前的日期/時間。 其他要求則會顯示在快取過期 (預設為 20 分鐘) 或因記憶體不足而撤銷之前的快取值。

您可以使用下列屬性來設定快取期間：

## <a name="cache-tag-helper-attributes"></a>快取標籤協助程式屬性

- - -

### <a name="enabled"></a>enabled    


| 屬性類型    | 有效值      |
|----------------   |----------------   |
| boolean           | "true" (預設)  |
|                   | "false"   |


判斷是否快取以快取標籤協助程式括住的內容。 預設為 `true`。  如果設定為 `false`，此快取標籤協助程式將不會對轉譯輸出有任何快取影響。

範例：

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>expires-on 

| 屬性類型    | 範例值     |
|----------------   |----------------   |
| DateTimeOffset    | "@new DateTime(2025,1,29,17,02,0)"    |


設定絕對到期日。 下列範例會快取 2025 年 1 月 29 日下午 5:02 以前的快取標籤協助程式內容。

範例：

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>expires-after

| 屬性類型    | 範例值     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(120)"    |


設定從第一個要求時間到快取內容的時間長度。 

範例：

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>expires-sliding

| 屬性類型    | 範例值     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(60)"     |


設定快取項目多久未被存取時應該撤銷。

範例：

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>vary-by-header

| 屬性類型    | 範例值                |
|----------------   |----------------               |
| String            | "User-Agent"                  |
|                   | "User-Agent,content-encoding" |

接受單一標頭值或標頭值的逗號分隔清單在變更時，觸發快取重新整理。 下列範例會監視標頭值 `User-Agent`。 此範例會快取展示給網頁伺服器之每個不同 `User-Agent` 的內容。

範例：

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a>vary-by-query

| 屬性類型    | 範例值                |
|----------------   |----------------               |
| String            | "Make"                |
|                   | "Make,Model" |

接受單一標頭值或標頭值的逗號分隔清單在標頭值變更時，觸發快取重新整理。 下列範例會查看 `Make` 和 `Model` 的值。

範例：

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>vary-by-route

| 屬性類型    | 範例值                |
|----------------   |----------------               |
| String            | "Make"                |
|                   | "Make,Model" |

接受單一標頭值或標頭值的逗號分隔清單在路由資料參數值變更時，觸發快取重新整理。 範例：

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
*Index.cshtml*

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a>vary-by-cookie

| 屬性類型    | 範例值                |
|----------------   |----------------               |
| String            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

接受單一標頭值或標頭值的逗號分隔清單在標頭值變更時，觸發快取重新整理。 下列範例會查看與 ASP.NET 識別建立關聯的 Cookie。 當使用者經過驗證時，待設定的要求 Cookie 會觸發快取重新整理。

範例：

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a>vary-by-user

| 屬性類型    | 範例值                |
|----------------   |----------------               |
| Boolean             | "true"                  |
|                     | "false" (預設) |

指定當登入的使用者 (或內容主體) 變更時，是否應該重設快取。 目前的使用者也稱為要求內容主體，可在 Razor 檢視中藉由參考 `@User.Identity.Name` 進行檢視。

下列範例會查看目前登入的使用者。  

範例：

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

使用此屬性可保留登入再登出週期內的快取內容。  使用 `vary-by-user="true"` 時，經過驗證之使用者的登入再登出動作會使快取失效。  快取失效是由於登入時會產生新的唯一 Cookie 值所致。 如果沒有任何 Cookie 或 Cookie 已過期，則會以匿名狀態保留快取。 這表示如果沒有任何使用者登入，則會保留快取。

- - -

### <a name="vary-by"></a>vary-by

| 屬性類型    | 範例值                |
|----------------   |----------------               |
| String             | "@Model"                 |


可自訂要快取哪些資料。 當屬性字串值所參考的物件變更時，就會更新快取標籤協助程式的內容。 通常會對此屬性指派模型值的字串串連。  實際上，這表示更新任何串連值都會使快取失效。

下列範例假設轉譯檢視的控制器方法加總兩個路由參數 `myParam1` 和 `myParam2` 的整數值，並傳回一個模型屬性。 當此總和變更時，快取標籤協助程式的內容會重新轉譯和快取。  

範例：

動作：

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

*Index.cshtml*

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a>priority

| 屬性類型    | 範例值                |
|----------------   |----------------               |
| CacheItemPriority  | "High"                   |
|                    | "Low" |
|                    | "NeverRemove" |
|                    | "Normal" |

提供快取撤銷指引給內建快取提供者。 當記憶體不足時，網頁伺服器會先撤銷 `Low` 快取項目。

範例：

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority` 屬性並不保證特定快取保留層級。 `CacheItemPriority` 僅供建議。 將此屬性設定為 `NeverRemove` 並不保證一定會保留快取。 如需詳細資訊，請參閱[其他資源](#additional-resources)。

快取標籤協助程式相依於[記憶體快取服務](xref:performance/caching/memory)。 如果尚未新增此服務，快取標籤協助程式會予以新增。

## <a name="additional-resources"></a>其他資源

* [記憶體內部快取](xref:performance/caching/memory)
* [身分識別簡介](xref:security/authentication/identity)
