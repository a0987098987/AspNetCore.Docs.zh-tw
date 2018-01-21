---
title: "快取中 ASP.NET Core MVC 標記協助程式"
author: pkellner
description: "示範如何使用快取標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: dfd9c3c0c4e50a99e4f8703b01bd9b384930b87a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>快取中 ASP.NET Core MVC 標記協助程式

由 [Peter Kellner](http://peterkellner.net) 提供 

快取標記協助程式提供了可大幅改善其內部的 ASP.NET Core 快取提供者的內容快取的 ASP.NET Core 應用程式的效能。

Razor 檢視引擎設定的預設`expires-after`為 20 分鐘。

下列的 Razor 標記會快取的日期/時間：

```cshtml
<cache>@DateTime.Now</cache>
```

包含頁面的第一個要求`CacheTagHelper`會顯示目前的日期/時間。 其他要求會顯示快取的值，直到快取到期 （預設值 20 分鐘），或者由記憶體不足的壓力移出記憶體。

您可以設定快取期間具有下列屬性：

## <a name="cache-tag-helper-attributes"></a>快取標記協助程式屬性

- - -

### <a name="enabled"></a>enabled    


| 屬性類型    | 有效值      |
|----------------   |----------------   |
| boolean           | "true"（預設值）  |
|                   | "false"   |


決定是否要快取內容快取標記協助程式 」 所括住。 預設值為 `true`。  如果設定為`false`此快取標記協助程式不會影響快取上轉譯的輸出。

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


設定絕對到期日。 下列範例將快取的快取標記協助程式內容，直到 2025 年 1 月 29，下午 5:02。

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


從快取內容的第一個要求時間設定時間的長度。 

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


設定應該收回快取項目，如果它尚未被存取的時間。

範例：

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>改變-依標頭

| 屬性類型    | 範例值                |
|----------------   |----------------               |
| String            | "User-Agent"                  |
|                   | "User-Agent,content-encoding" |

接受單一標頭值或其變更時觸發快取重新整理的標頭值的逗號分隔清單。 下列範例會監視的標頭值`User-Agent`。 此範例會將快取內容的每個不同`User-Agent`呈現給 web 伺服器。

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
| String            | 「 讓 」                |
|                   | 「 請模型 」 |

接受單一標頭值或以逗號分隔清單的標頭值變更時觸發快取重新整理的標頭值。 下列範例會查看值`Make`和`Model`。

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
| String            | 「 讓 」                |
|                   | 「 請模型 」 |

接受單一標頭值或路由資料的參數值變更時觸發快取重新整理的標頭值的逗號分隔清單。 範例：

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

接受單一標頭值或標頭值 (s) 變更時觸發快取重新整理的標頭值的逗號分隔清單。 下列範例會查看與 ASP.NET 識別相關聯的 cookie。 當使用者通過驗證設定要求 cookie 觸發快取重新整理。

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
|                     | "false"（預設值） |

指定已登入使用者 （或內容主體） 變更時，應該重設快取。 目前的使用者就是所謂的要求內容主體，而且可以藉由參考檢視 Razor 檢視`@User.Identity.Name`。

下列範例會查看目前的登入的使用者。  

範例：

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

使用這個屬性會維護透過在登入和登出循環的快取中的內容。  當使用`vary-by-user="true"`，登入和登出的動作會導致無效的快取驗證的使用者。  快取無效，因為登入，產生新的唯一的 cookie 值。 快取保留匿名狀態時沒有任何 cookie，或已過期。 這表示如果沒有使用者登入，仍會維護快取。

- - -

### <a name="vary-by"></a>vary-by

| 屬性類型    | 範例值                |
|----------------   |----------------               |
| String             | "@Model"                 |


可讓您自訂的哪些資料取得快取。 更新屬性的字串值的變更，快取標記協助程式的內容所參考的物件時。 通常模型值的字串串連會指派給這個屬性。  實際上，這表示，串連的值的任何更新的快取失效。

下列範例假設控制器方法轉譯檢視加總兩個路由參數的整數值`myParam1`和`myParam2`，並傳回，當做單一模型屬性。 當這個總和變更時，快取標記協助程式的內容轉譯，而且一次快取。  

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
| CacheItemPriority  | 「 高 」                   |
|                    | 「 低 」 |
|                    | "NeverRemove" |
|                    | "Normal" |

提供了內建的快取提供者快取收回的指引。 網頁伺服器將會收回`Low`時更新快取項目第一次記憶體不足壓力下。

範例：

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority`屬性並不保證快取保留特定層級。 `CacheItemPriority`是只是建議。 將此屬性設定為`NeverRemove`不保證一定會保留在快取。 請參閱[其他資源](#additional-resources)如需詳細資訊。

快取標記協助程式是依賴[記憶體快取服務](xref:performance/caching/memory)。 如果尚未加入快取標記協助程式就會加入服務。

## <a name="additional-resources"></a>其他資源

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
