---
title: 3.1 ASP.NET Core 的新功能
author: rick-anderson
description: 深入瞭解 ASP.NET Core 3.1 中的新功能。
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: aspnetcore-3.1
ms.openlocfilehash: 5b6ae8173ca3d968e220faa4a060e1b42b14f8bb
ms.sourcegitcommit: 05490855e0c70565f0c4b509d392b0828bcfd141
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507240"
---
# <a name="whats-new-in-aspnet-core-31"></a>3.1 ASP.NET Core 的新功能

本文將重點放在 ASP.NET Core 3.1 中最重要的變更，並提供相關檔的連結。

## <a name="partial-class-support-for-razor-components"></a>元件的部分類別支援 Razor

Razor元件現在會以部分類別的形式產生。 元件的程式碼 Razor 可以使用定義為部分類別的程式碼後置檔案來撰寫，而不是在單一檔案中定義元件的所有程式碼。 如需詳細資訊，請參閱[部分類別支援](xref:blazor/components#partial-class-support)。

## <a name="blazor-component-tag-helper-and-pass-parameters-to-top-level-components"></a>Blazor元件標記協助程式，並將參數傳遞至最上層元件

在 Blazor 具有 ASP.NET Core 3.0 的中，會使用 HTML Helper （）將元件轉譯成頁面和視圖 `Html.RenderComponentAsync` 。 在 ASP.NET Core 3.1 中，使用新的元件標記協助程式，從頁面或視圖轉譯元件：

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

ASP.NET Core 3.1 仍然支援 HTML Helper，但建議使用元件標記協助程式。

Blazor伺服器應用程式現在可以在初始轉譯期間，將參數傳遞至最上層元件。 先前您只能將參數傳遞至具有[RenderMode](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static)的最上層元件。 在此版本中，支援[RenderMode](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server)和[RenderMode](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) 。 任何指定的參數值會序列化為 JSON，並包含在初始回應中。

例如， `Counter` 已建立具有遞增量（）的元件 `IncrementAmount` ：

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

如需詳細資訊，請參閱將[元件整合至 Razor 頁面和 MVC 應用程式](xref:blazor/integrate-components)。

## <a name="support-for-shared-queues-in-httpsys"></a>支援 HTTP.sys 中的共用佇列

[Http.sys 支援建立](xref:fundamentals/servers/httpsys)匿名要求佇列。 在 ASP.NET Core 3.1 中，我們新增了建立或附加至現有已命名 HTTP.SYS 要求佇列的功能。 建立或附加至現有的已命名 HTTP.SYS 要求佇列，可讓擁有佇列的 HTTP.SYS 控制器進程與接聽程式進程無關的情況。 這種獨立性讓您能夠在接聽程式進程重新開機之間保留現有的連接和排入佇列的要求：

[!code-csharp[](sample/Program.cs?name=snippet)]

## <a name="breaking-changes-for-samesite-cookies"></a>SameSite cookie 的重大變更

SameSite cookie 的行為已變更，以反映即將進行的瀏覽器變更。 這可能會影響驗證案例，例如 AzureAd、OpenIdConnect 或 WsFederation。 如需詳細資訊，請參閱 <xref:security/samesite> 。

## <a name="prevent-default-actions-for-events-in-blazor-apps"></a>防止應用程式中的事件進行預設動作 Blazor

使用指示詞 `@on{EVENT}:preventDefault` 屬性可防止事件的預設動作。 在下列範例中，會防止在文字方塊中顯示金鑰字元的預設動作：

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

如需詳細資訊，請參閱[防止預設動作](xref:blazor/event-handling#prevent-default-actions)。

## <a name="stop-event-propagation-in-blazor-apps"></a>停止應用程式中的事件傳播 Blazor

使用指示詞 `@on{EVENT}:stopPropagation` 屬性來停止事件傳播。 在下列範例中，選取核取方塊可防止從子系的 click 事件 `<div>` 傳播到父系 `<div>` ：

```razor
<input @bind="_stopPropagation" type="checkbox" />

<div @onclick="OnSelectParentDiv">
    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        ...
    </div>
</div>

@code {
    private bool _stopPropagation = false;
}
```

如需詳細資訊，請參閱[停止事件傳播](xref:blazor/event-handling#stop-event-propagation)。

## <a name="detailed-errors-during-blazor-app-development"></a>應用程式開發期間的詳細錯誤 Blazor

當 Blazor 應用程式在開發期間無法正常運作時，從應用程式接收詳細的錯誤資訊有助於疑難排解和修正問題。 發生錯誤時， Blazor 應用程式會在畫面底部顯示 [金色] 橫條：

* 在開發期間，「金級」列會將您導向至瀏覽器主控台，您可以在其中看到例外狀況。
* 在生產環境中，「金級」列會通知使用者發生錯誤，並建議重新整理瀏覽器。

如需詳細資訊，請參閱[開發期間的詳細錯誤](xref:blazor/handle-errors#detailed-errors-during-development)。
