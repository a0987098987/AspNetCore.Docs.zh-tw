---
title: 3\.1 ASP.NET Core 的新功能
author: rick-anderson
description: 深入瞭解 ASP.NET Core 3.1 中的新功能。
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: aspnetcore-3.1
ms.openlocfilehash: f375022ad3ebdea2990f626320ef295926f88c22
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447434"
---
# <a name="whats-new-in-aspnet-core-31"></a>3\.1 ASP.NET Core 的新功能

本文將重點放在 ASP.NET Core 3.1 中最重要的變更，並提供相關檔的連結。

## <a name="partial-class-support-for-razor-components"></a>Razor 元件的部分類別支援

Razor 元件現在會以部分類別的形式產生。 Razor 元件的程式碼可以使用定義為部分類別的程式碼後置檔案來撰寫，而不是在單一檔案中定義該元件的所有程式碼。 如需詳細資訊，請參閱[部分類別支援](xref:blazor/components#partial-class-support)。

## <a name="opno-locblazor-component-tag-helper-and-pass-parameters-to-top-level-components"></a>Blazor 元件標記協助程式，並將參數傳遞至最上層元件

在 ASP.NET Core 3.0 的 Blazor 中，會使用 HTML Helper （`Html.RenderComponentAsync`）將元件轉譯成頁面和 views。 在 ASP.NET Core 3.1 中，使用新的元件標記協助程式，從頁面或視圖轉譯元件：

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

ASP.NET Core 3.1 仍然支援 HTML Helper，但建議使用元件標記協助程式。

Blazor 伺服器應用程式現在可以在初始轉譯期間，將參數傳遞至最上層元件。 先前您只能將參數傳遞至具有[RenderMode](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static)的最上層元件。 在此版本中，支援[RenderMode](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server)和[RenderModel](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) 。 任何指定的參數值會序列化為 JSON，並包含在初始回應中。

例如，已建立一個具有遞增量（`IncrementAmount`）的 `Counter` 元件：

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

如需詳細資訊，請參閱將[元件整合到 Razor Pages 和 MVC 應用程式](xref:blazor/integrate-components)。

## <a name="support-for-shared-queues-in-httpsys"></a>支援 HTTP.sys 中的共用佇列

[Http.sys 支援建立](xref:fundamentals/servers/httpsys)匿名要求佇列。 在 ASP.NET Core 3.1 中，我們新增了建立或附加至現有已命名 HTTP.SYS 要求佇列的功能。 建立或附加至現有的已命名 HTTP.SYS 要求佇列，可讓擁有佇列的 HTTP.SYS 控制器進程與接聽程式進程無關的情況。 這種獨立性讓您能夠在接聽程式進程重新開機之間保留現有的連接和排入佇列的要求：

[!code-csharp[](sample/Program.cs?name=snippet)]

## <a name="breaking-changes-for-samesite-cookies"></a>SameSite cookie 的重大變更

SameSite cookie 的行為已變更，以反映即將進行的瀏覽器變更。 這可能會影響驗證案例，例如 AzureAd、OpenIdConnect 或 WsFederation。 如需詳細資訊，請參閱 <xref:security/samesite>。

## <a name="prevent-default-actions-for-events-in-opno-locblazor-apps"></a>防止 Blazor 應用程式中的事件的預設動作

使用 `@on{EVENT}:preventDefault` 指示詞屬性來防止事件的預設動作。 在下列範例中，會防止在文字方塊中顯示金鑰字元的預設動作：

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

如需詳細資訊，請參閱[防止預設動作](xref:blazor/event-handling#prevent-default-actions)。

## <a name="stop-event-propagation-in-opno-locblazor-apps"></a>在 Blazor 應用程式中停止事件傳播

使用 `@on{EVENT}:stopPropagation` 指示詞屬性來停止事件傳播。 在下列範例中，選取核取方塊可防止從子系 `<div>` 的 click 事件傳播到父 `<div>`：

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

## <a name="detailed-errors-during-opno-locblazor-app-development"></a>Blazor 應用程式開發期間的詳細錯誤

當 Blazor 應用程式在開發期間無法正常運作時，從應用程式接收詳細的錯誤資訊有助於疑難排解和修正問題。 發生錯誤時，Blazor 應用程式會在畫面底部顯示 [金色] 列：

* 在開發期間，「金級」列會將您導向至瀏覽器主控台，您可以在其中看到例外狀況。
* 在生產環境中，「金級」列會通知使用者發生錯誤，並建議重新整理瀏覽器。

如需詳細資訊，請參閱[開發期間的詳細錯誤](xref:blazor/handle-errors#detailed-errors-during-development)。
