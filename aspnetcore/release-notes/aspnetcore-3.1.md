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
# <a name="whats-new-in-aspnet-core-31"></a><span data-ttu-id="bbc82-103">ASP.NET核心 3.1 中的新增功能</span><span class="sxs-lookup"><span data-stu-id="bbc82-103">What's new in ASP.NET Core 3.1</span></span>

<span data-ttu-id="bbc82-104">本文重點介紹了ASP.NET Core 3.1 中最重要的更改,並介紹了相關文檔的連結。</span><span class="sxs-lookup"><span data-stu-id="bbc82-104">This article highlights the most significant changes in ASP.NET Core 3.1 with links to relevant documentation.</span></span>

## <a name="partial-class-support-for-razor-components"></a><span data-ttu-id="bbc82-105">對 Razor 元件的部分類別支援</span><span class="sxs-lookup"><span data-stu-id="bbc82-105">Partial class support for Razor components</span></span>

<span data-ttu-id="bbc82-106">Razor 元件現在生成為部分類。</span><span class="sxs-lookup"><span data-stu-id="bbc82-106">Razor components are now generated as partial classes.</span></span> <span data-ttu-id="bbc82-107">Razor 元件的代碼可以使用定義為部分類的代碼後面檔編寫,而不是在單個檔中定義元件的所有代碼。</span><span class="sxs-lookup"><span data-stu-id="bbc82-107">Code for a Razor component can be written using a code-behind file defined as a partial class rather than defining all the code for the component in a single file.</span></span> <span data-ttu-id="bbc82-108">有關詳細資訊,請參閱[部分類支援](xref:blazor/components#partial-class-support)。</span><span class="sxs-lookup"><span data-stu-id="bbc82-108">For more information, see [Partial class support](xref:blazor/components#partial-class-support).</span></span>

## <a name="opno-locblazor-component-tag-helper-and-pass-parameters-to-top-level-components"></a>Blazor<span data-ttu-id="bbc82-109">元件標記說明器並將參數傳遞給頂級元件</span><span class="sxs-lookup"><span data-stu-id="bbc82-109"> Component Tag Helper and pass parameters to top-level components</span></span>

<span data-ttu-id="bbc82-110">在BlazorASP.NET酷3.0中,元件使用 HTML 說明器`Html.RenderComponentAsync`() 呈現為頁面和檢視。</span><span class="sxs-lookup"><span data-stu-id="bbc82-110">In Blazor with ASP.NET Core 3.0, components were rendered into pages and views using an HTML Helper (`Html.RenderComponentAsync`).</span></span> <span data-ttu-id="bbc82-111">在ASP.NET酷3.1中,使用新的元件標記幫助程式從頁面或檢視中呈現元件:</span><span class="sxs-lookup"><span data-stu-id="bbc82-111">In ASP.NET Core 3.1, render a component from a page or view with the new Component Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="bbc82-112">ASP.NET核心 3.1 中仍然支援 HTML 説明程式,但建議使用元件標記説明程式。</span><span class="sxs-lookup"><span data-stu-id="bbc82-112">The HTML Helper remains supported in ASP.NET Core 3.1, but the Component Tag Helper is recommended.</span></span>

Blazor<span data-ttu-id="bbc82-113">伺服器應用現在可以在初始渲染期間將參數傳遞給頂級元件。</span><span class="sxs-lookup"><span data-stu-id="bbc82-113"> Server apps can now pass parameters to top-level components during the initial render.</span></span> <span data-ttu-id="bbc82-114">以前,您只能將參數傳遞給具有[RenderMode.Static 的頂級](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static)元件。</span><span class="sxs-lookup"><span data-stu-id="bbc82-114">Previously you could only pass parameters to a top-level component with [RenderMode.Static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static).</span></span> <span data-ttu-id="bbc82-115">使用此版本,支援[RenderMode.Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server)和[RenderModel.伺服器預算](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered)。</span><span class="sxs-lookup"><span data-stu-id="bbc82-115">With this release, both [RenderMode.Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) and [RenderModel.ServerPrerendered](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) are supported.</span></span> <span data-ttu-id="bbc82-116">任何指定的參數值都序列化為 JSON,並包含在初始回應中。</span><span class="sxs-lookup"><span data-stu-id="bbc82-116">Any specified parameter values are serialized as JSON and included in the initial response.</span></span>

<span data-ttu-id="bbc82-117">例如,預先成`Counter`具有增量量的元件`IncrementAmount`( :</span><span class="sxs-lookup"><span data-stu-id="bbc82-117">For example, prerender a `Counter` component with an increment amount (`IncrementAmount`):</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="bbc82-118">有關詳細資訊,請參閱[將元件整合到 Razor 頁面和 MVC 應用中](xref:blazor/integrate-components)。</span><span class="sxs-lookup"><span data-stu-id="bbc82-118">For more information, see [Integrate components into Razor Pages and MVC apps](xref:blazor/integrate-components).</span></span>

## <a name="support-for-shared-queues-in-httpsys"></a><span data-ttu-id="bbc82-119">支援 HTTP.sys 中的共用佇列</span><span class="sxs-lookup"><span data-stu-id="bbc82-119">Support for shared queues in HTTP.sys</span></span>

<span data-ttu-id="bbc82-120">[HTTP.sys](xref:fundamentals/servers/httpsys)支援建立匿名請求佇列。</span><span class="sxs-lookup"><span data-stu-id="bbc82-120">[HTTP.sys](xref:fundamentals/servers/httpsys) supports creating anonymous request queues.</span></span> <span data-ttu-id="bbc82-121">在 ASP.NET 酷 3.1 中,我們添加了創建或附加到現有名為 HTTP.sys 請求佇列的能力。</span><span class="sxs-lookup"><span data-stu-id="bbc82-121">In ASP.NET Core 3.1, we've added to ability to create or attach to an existing named HTTP.sys request queue.</span></span> <span data-ttu-id="bbc82-122">創建或附加到現有的名為 HTTP.sys 請求佇列時,可以啟用擁有佇列的 HTTP.sys 控制器進程獨立於偵聽器進程的方案。</span><span class="sxs-lookup"><span data-stu-id="bbc82-122">Creating or attaching to an existing named HTTP.sys request queue enables scenarios where the HTTP.sys controller process that owns the queue is independent of the listener process.</span></span> <span data-ttu-id="bbc82-123">這種獨立性使得在偵聽器行程重新啟動之間保留現有連接和排隊的請求成為可能:</span><span class="sxs-lookup"><span data-stu-id="bbc82-123">This independence makes it possible to preserve existing connections and enqueued requests between listener process restarts:</span></span>

[!code-csharp[](sample/Program.cs?name=snippet)]

## <a name="breaking-changes-for-samesite-cookies"></a><span data-ttu-id="bbc82-124">同一網站 Cookie 的中斷變更</span><span class="sxs-lookup"><span data-stu-id="bbc82-124">Breaking changes for SameSite cookies</span></span>

<span data-ttu-id="bbc82-125">SameSite Cookie 的行為已更改,以反映即將進行的瀏覽器更改。</span><span class="sxs-lookup"><span data-stu-id="bbc82-125">The behavior of SameSite cookies has changed to reflect upcoming browser changes.</span></span> <span data-ttu-id="bbc82-126">這可能會影響身份驗證方案,如 AzureAd、OpenIdConnect 或 Ws-AI。</span><span class="sxs-lookup"><span data-stu-id="bbc82-126">This may affect authentication scenarios like AzureAd, OpenIdConnect, or WsFederation.</span></span> <span data-ttu-id="bbc82-127">如需詳細資訊，請參閱 <xref:security/samesite>。</span><span class="sxs-lookup"><span data-stu-id="bbc82-127">For more information, see <xref:security/samesite>.</span></span>

## <a name="prevent-default-actions-for-events-in-opno-locblazor-apps"></a><span data-ttu-id="bbc82-128">防止應用程式中事件的Blazor預設操作</span><span class="sxs-lookup"><span data-stu-id="bbc82-128">Prevent default actions for events in Blazor apps</span></span>

<span data-ttu-id="bbc82-129">使用`@on{EVENT}:preventDefault`指令屬性防止事件的預設操作。</span><span class="sxs-lookup"><span data-stu-id="bbc82-129">Use the `@on{EVENT}:preventDefault` directive attribute to prevent the default action for an event.</span></span> <span data-ttu-id="bbc82-130">在下面的範例中,阻止在文字框中顯示鍵字元的預設操作:</span><span class="sxs-lookup"><span data-stu-id="bbc82-130">In the following example, the default action of displaying the key's character in the text box is prevented:</span></span>

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

<span data-ttu-id="bbc82-131">有關詳細資訊,請參閱[封鎖預設操作](xref:blazor/event-handling#prevent-default-actions)。</span><span class="sxs-lookup"><span data-stu-id="bbc82-131">For more information, see [Prevent default actions](xref:blazor/event-handling#prevent-default-actions).</span></span>

## <a name="stop-event-propagation-in-opno-locblazor-apps"></a><span data-ttu-id="bbc82-132">停止應用中Blazor的事件傳播</span><span class="sxs-lookup"><span data-stu-id="bbc82-132">Stop event propagation in Blazor apps</span></span>

<span data-ttu-id="bbc82-133">使用`@on{EVENT}:stopPropagation`指令屬性停止事件傳播。</span><span class="sxs-lookup"><span data-stu-id="bbc82-133">Use the `@on{EVENT}:stopPropagation` directive attribute to stop event propagation.</span></span> <span data-ttu-id="bbc82-134">在下面的範例中,選擇選擇選擇選擇的視窗可防止`<div>`子級 的單擊事件傳播`<div>`到父級 :</span><span class="sxs-lookup"><span data-stu-id="bbc82-134">In the following example, selecting the check box prevents click events from the child `<div>` from propagating to the parent `<div>`:</span></span>

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

<span data-ttu-id="bbc82-135">有關詳細資訊,請參閱[停止事件傳播](xref:blazor/event-handling#stop-event-propagation)。</span><span class="sxs-lookup"><span data-stu-id="bbc82-135">For more information, see [Stop event propagation](xref:blazor/event-handling#stop-event-propagation).</span></span>

## <a name="detailed-errors-during-opno-locblazor-app-development"></a><span data-ttu-id="bbc82-136">套用開發Blazor過程中的詳細錯誤</span><span class="sxs-lookup"><span data-stu-id="bbc82-136">Detailed errors during Blazor app development</span></span>

<span data-ttu-id="bbc82-137">當應用Blazor在開發過程中無法正常運行時,從應用接收詳細的錯誤資訊有助於解決問題。</span><span class="sxs-lookup"><span data-stu-id="bbc82-137">When a Blazor app isn't functioning properly during development, receiving detailed error information from the app assists in troubleshooting and fixing the issue.</span></span> <span data-ttu-id="bbc82-138">發生錯誤時,Blazor應用在螢幕底部顯示金條:</span><span class="sxs-lookup"><span data-stu-id="bbc82-138">When an error occurs, Blazor apps display a gold bar at the bottom of the screen:</span></span>

* <span data-ttu-id="bbc82-139">在開發過程中,金條將引導您到瀏覽器控制台,您可以在其中看到異常。</span><span class="sxs-lookup"><span data-stu-id="bbc82-139">During development, the gold bar directs you to the browser console, where you can see the exception.</span></span>
* <span data-ttu-id="bbc82-140">在生產中,金條會通知使用者錯誤,並建議刷新瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="bbc82-140">In production, the gold bar notifies the user that an error has occurred and recommends refreshing the browser.</span></span>

<span data-ttu-id="bbc82-141">有關詳細資訊,請參閱[開發過程中的詳細錯誤](xref:blazor/handle-errors#detailed-errors-during-development)。</span><span class="sxs-lookup"><span data-stu-id="bbc82-141">For more information, see [Detailed errors during development](xref:blazor/handle-errors#detailed-errors-during-development).</span></span>
