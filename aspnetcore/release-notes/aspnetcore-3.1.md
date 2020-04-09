---
title: ASP.NET核心 3.1 中的新增功能
author: rick-anderson
description: 瞭解 ASP.NET 酷 3.1 中的新功能。
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: aspnetcore-3.1
ms.openlocfilehash: f375022ad3ebdea2990f626320ef295926f88c22
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78662705"
---
# <a name="whats-new-in-aspnet-core-31"></a>ASP.NET核心 3.1 中的新增功能

本文重點介紹了ASP.NET Core 3.1 中最重要的更改,並介紹了相關文檔的連結。

## <a name="partial-class-support-for-razor-components"></a>對 Razor 元件的部分類別支援

Razor 元件現在生成為部分類。 Razor 元件的代碼可以使用定義為部分類的代碼後面檔編寫,而不是在單個檔中定義元件的所有代碼。 有關詳細資訊,請參閱[部分類支援](xref:blazor/components#partial-class-support)。

## <a name="opno-locblazor-component-tag-helper-and-pass-parameters-to-top-level-components"></a>Blazor元件標記說明器並將參數傳遞給頂級元件

在BlazorASP.NET酷3.0中,元件使用 HTML 說明器`Html.RenderComponentAsync`() 呈現為頁面和檢視。 在ASP.NET酷3.1中,使用新的元件標記幫助程式從頁面或檢視中呈現元件:

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

ASP.NET核心 3.1 中仍然支援 HTML 説明程式,但建議使用元件標記説明程式。

Blazor伺服器應用現在可以在初始渲染期間將參數傳遞給頂級元件。 以前,您只能將參數傳遞給具有[RenderMode.Static 的頂級](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static)元件。 使用此版本,支援[RenderMode.Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server)和[RenderModel.伺服器預算](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered)。 任何指定的參數值都序列化為 JSON,並包含在初始回應中。

例如,預先成`Counter`具有增量量的元件`IncrementAmount`( :

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

有關詳細資訊,請參閱[將元件整合到 Razor 頁面和 MVC 應用中](xref:blazor/integrate-components)。

## <a name="support-for-shared-queues-in-httpsys"></a>支援 HTTP.sys 中的共用佇列

[HTTP.sys](xref:fundamentals/servers/httpsys)支援建立匿名請求佇列。 在 ASP.NET 酷 3.1 中,我們添加了創建或附加到現有名為 HTTP.sys 請求佇列的能力。 創建或附加到現有的名為 HTTP.sys 請求佇列時,可以啟用擁有佇列的 HTTP.sys 控制器進程獨立於偵聽器進程的方案。 這種獨立性使得在偵聽器行程重新啟動之間保留現有連接和排隊的請求成為可能:

[!code-csharp[](sample/Program.cs?name=snippet)]

## <a name="breaking-changes-for-samesite-cookies"></a>同一網站 Cookie 的中斷變更

SameSite Cookie 的行為已更改,以反映即將進行的瀏覽器更改。 這可能會影響身份驗證方案,如 AzureAd、OpenIdConnect 或 Ws-AI。 如需詳細資訊，請參閱 <xref:security/samesite>。

## <a name="prevent-default-actions-for-events-in-opno-locblazor-apps"></a>防止應用程式中事件的Blazor預設操作

使用`@on{EVENT}:preventDefault`指令屬性防止事件的預設操作。 在下面的範例中,阻止在文字框中顯示鍵字元的預設操作:

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

有關詳細資訊,請參閱[封鎖預設操作](xref:blazor/event-handling#prevent-default-actions)。

## <a name="stop-event-propagation-in-opno-locblazor-apps"></a>停止應用中Blazor的事件傳播

使用`@on{EVENT}:stopPropagation`指令屬性停止事件傳播。 在下面的範例中,選擇選擇選擇選擇的視窗可防止`<div>`子級 的單擊事件傳播`<div>`到父級 :

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

有關詳細資訊,請參閱[停止事件傳播](xref:blazor/event-handling#stop-event-propagation)。

## <a name="detailed-errors-during-opno-locblazor-app-development"></a>套用開發Blazor過程中的詳細錯誤

當應用Blazor在開發過程中無法正常運行時,從應用接收詳細的錯誤資訊有助於解決問題。 發生錯誤時,Blazor應用在螢幕底部顯示金條:

* 在開發過程中,金條將引導您到瀏覽器控制台,您可以在其中看到異常。
* 在生產中,金條會通知使用者錯誤,並建議刷新瀏覽器。

有關詳細資訊,請參閱[開發過程中的詳細錯誤](xref:blazor/handle-errors#detailed-errors-during-development)。
