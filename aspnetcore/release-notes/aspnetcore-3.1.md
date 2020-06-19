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
ms.openlocfilehash: 6a03e35495e2ae545dc0a3cdd38578b433d8df6b
ms.sourcegitcommit: 490434a700ba8c5ed24d849bd99d8489858538e3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85102582"
---
# <a name="whats-new-in-aspnet-core-31"></a><span data-ttu-id="9d25f-103">3.1 ASP.NET Core 的新功能</span><span class="sxs-lookup"><span data-stu-id="9d25f-103">What's new in ASP.NET Core 3.1</span></span>

<span data-ttu-id="9d25f-104">本文將重點放在 ASP.NET Core 3.1 中最重要的變更，並提供相關檔的連結。</span><span class="sxs-lookup"><span data-stu-id="9d25f-104">This article highlights the most significant changes in ASP.NET Core 3.1 with links to relevant documentation.</span></span>

## <a name="partial-class-support-for-razor-components"></a><span data-ttu-id="9d25f-105">元件的部分類別支援 Razor</span><span class="sxs-lookup"><span data-stu-id="9d25f-105">Partial class support for Razor components</span></span>

Razor<span data-ttu-id="9d25f-106">元件現在會以部分類別的形式產生。</span><span class="sxs-lookup"><span data-stu-id="9d25f-106"> components are now generated as partial classes.</span></span> <span data-ttu-id="9d25f-107">元件的程式碼 Razor 可以使用定義為部分類別的程式碼後置檔案來撰寫，而不是在單一檔案中定義元件的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="9d25f-107">Code for a Razor component can be written using a code-behind file defined as a partial class rather than defining all the code for the component in a single file.</span></span> <span data-ttu-id="9d25f-108">如需詳細資訊，請參閱[部分類別支援](xref:blazor/components/index#partial-class-support)。</span><span class="sxs-lookup"><span data-stu-id="9d25f-108">For more information, see [Partial class support](xref:blazor/components/index#partial-class-support).</span></span>

## <a name="blazor-component-tag-helper-and-pass-parameters-to-top-level-components"></a>Blazor<span data-ttu-id="9d25f-109">元件標記協助程式，並將參數傳遞至最上層元件</span><span class="sxs-lookup"><span data-stu-id="9d25f-109"> Component Tag Helper and pass parameters to top-level components</span></span>

<span data-ttu-id="9d25f-110">在 Blazor 具有 ASP.NET Core 3.0 的中，會使用 HTML Helper （）將元件轉譯成頁面和視圖 `Html.RenderComponentAsync` 。</span><span class="sxs-lookup"><span data-stu-id="9d25f-110">In Blazor with ASP.NET Core 3.0, components were rendered into pages and views using an HTML Helper (`Html.RenderComponentAsync`).</span></span> <span data-ttu-id="9d25f-111">在 ASP.NET Core 3.1 中，使用新的元件標記協助程式，從頁面或視圖轉譯元件：</span><span class="sxs-lookup"><span data-stu-id="9d25f-111">In ASP.NET Core 3.1, render a component from a page or view with the new Component Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="9d25f-112">ASP.NET Core 3.1 仍然支援 HTML Helper，但建議使用元件標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="9d25f-112">The HTML Helper remains supported in ASP.NET Core 3.1, but the Component Tag Helper is recommended.</span></span>

Blazor<span data-ttu-id="9d25f-113">伺服器應用程式現在可以在初始轉譯期間，將參數傳遞至最上層元件。</span><span class="sxs-lookup"><span data-stu-id="9d25f-113"> Server apps can now pass parameters to top-level components during the initial render.</span></span> <span data-ttu-id="9d25f-114">先前您只能將參數傳遞至具有[RenderMode](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static)的最上層元件。</span><span class="sxs-lookup"><span data-stu-id="9d25f-114">Previously you could only pass parameters to a top-level component with [RenderMode.Static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static).</span></span> <span data-ttu-id="9d25f-115">在此版本中，支援[RenderMode](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server)和[RenderMode](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) 。</span><span class="sxs-lookup"><span data-stu-id="9d25f-115">With this release, both [RenderMode.Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) and [RenderMode.ServerPrerendered](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) are supported.</span></span> <span data-ttu-id="9d25f-116">任何指定的參數值會序列化為 JSON，並包含在初始回應中。</span><span class="sxs-lookup"><span data-stu-id="9d25f-116">Any specified parameter values are serialized as JSON and included in the initial response.</span></span>

<span data-ttu-id="9d25f-117">例如， `Counter` 已建立具有遞增量（）的元件 `IncrementAmount` ：</span><span class="sxs-lookup"><span data-stu-id="9d25f-117">For example, prerender a `Counter` component with an increment amount (`IncrementAmount`):</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="9d25f-118">如需詳細資訊，請參閱將[元件整合至 Razor 頁面和 MVC 應用程式](xref:blazor/components/integrate-components-into-razor-pages-and-mvc-apps)。</span><span class="sxs-lookup"><span data-stu-id="9d25f-118">For more information, see [Integrate components into Razor Pages and MVC apps](xref:blazor/components/integrate-components-into-razor-pages-and-mvc-apps).</span></span>

## <a name="support-for-shared-queues-in-httpsys"></a><span data-ttu-id="9d25f-119">支援 HTTP.sys 中的共用佇列</span><span class="sxs-lookup"><span data-stu-id="9d25f-119">Support for shared queues in HTTP.sys</span></span>

<span data-ttu-id="9d25f-120">[HTTP.sys](xref:fundamentals/servers/httpsys)支援建立匿名要求佇列。</span><span class="sxs-lookup"><span data-stu-id="9d25f-120">[HTTP.sys](xref:fundamentals/servers/httpsys) supports creating anonymous request queues.</span></span> <span data-ttu-id="9d25f-121">在 ASP.NET Core 3.1 中，我們新增了建立或附加至現有已命名 HTTP.sys 要求佇列的功能。</span><span class="sxs-lookup"><span data-stu-id="9d25f-121">In ASP.NET Core 3.1, we've added to ability to create or attach to an existing named HTTP.sys request queue.</span></span> <span data-ttu-id="9d25f-122">建立或附加至現有的已命名 HTTP.sys 要求佇列，可啟用擁有佇列的 HTTP.sys 控制器進程與接聽程式進程無關的案例。</span><span class="sxs-lookup"><span data-stu-id="9d25f-122">Creating or attaching to an existing named HTTP.sys request queue enables scenarios where the HTTP.sys controller process that owns the queue is independent of the listener process.</span></span> <span data-ttu-id="9d25f-123">這種獨立性讓您能夠在接聽程式進程重新開機之間保留現有的連接和排入佇列的要求：</span><span class="sxs-lookup"><span data-stu-id="9d25f-123">This independence makes it possible to preserve existing connections and enqueued requests between listener process restarts:</span></span>

[!code-csharp[](sample/Program.cs?name=snippet)]

## <a name="breaking-changes-for-samesite-cookies"></a><span data-ttu-id="9d25f-124">SameSite cookie 的重大變更</span><span class="sxs-lookup"><span data-stu-id="9d25f-124">Breaking changes for SameSite cookies</span></span>

<span data-ttu-id="9d25f-125">SameSite cookie 的行為已變更，以反映即將進行的瀏覽器變更。</span><span class="sxs-lookup"><span data-stu-id="9d25f-125">The behavior of SameSite cookies has changed to reflect upcoming browser changes.</span></span> <span data-ttu-id="9d25f-126">這可能會影響驗證案例，例如 AzureAd、OpenIdConnect 或 WsFederation。</span><span class="sxs-lookup"><span data-stu-id="9d25f-126">This may affect authentication scenarios like AzureAd, OpenIdConnect, or WsFederation.</span></span> <span data-ttu-id="9d25f-127">如需詳細資訊，請參閱 <xref:security/samesite> 。</span><span class="sxs-lookup"><span data-stu-id="9d25f-127">For more information, see <xref:security/samesite>.</span></span>

## <a name="prevent-default-actions-for-events-in-blazor-apps"></a><span data-ttu-id="9d25f-128">防止應用程式中的事件進行預設動作 Blazor</span><span class="sxs-lookup"><span data-stu-id="9d25f-128">Prevent default actions for events in Blazor apps</span></span>

<span data-ttu-id="9d25f-129">使用指示詞 `@on{EVENT}:preventDefault` 屬性可防止事件的預設動作。</span><span class="sxs-lookup"><span data-stu-id="9d25f-129">Use the `@on{EVENT}:preventDefault` directive attribute to prevent the default action for an event.</span></span> <span data-ttu-id="9d25f-130">在下列範例中，會防止在文字方塊中顯示金鑰字元的預設動作：</span><span class="sxs-lookup"><span data-stu-id="9d25f-130">In the following example, the default action of displaying the key's character in the text box is prevented:</span></span>

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

<span data-ttu-id="9d25f-131">如需詳細資訊，請參閱[防止預設動作](xref:blazor/components/event-handling#prevent-default-actions)。</span><span class="sxs-lookup"><span data-stu-id="9d25f-131">For more information, see [Prevent default actions](xref:blazor/components/event-handling#prevent-default-actions).</span></span>

## <a name="stop-event-propagation-in-blazor-apps"></a><span data-ttu-id="9d25f-132">停止應用程式中的事件傳播 Blazor</span><span class="sxs-lookup"><span data-stu-id="9d25f-132">Stop event propagation in Blazor apps</span></span>

<span data-ttu-id="9d25f-133">使用指示詞 `@on{EVENT}:stopPropagation` 屬性來停止事件傳播。</span><span class="sxs-lookup"><span data-stu-id="9d25f-133">Use the `@on{EVENT}:stopPropagation` directive attribute to stop event propagation.</span></span> <span data-ttu-id="9d25f-134">在下列範例中，選取核取方塊可防止從子系的 click 事件 `<div>` 傳播到父系 `<div>` ：</span><span class="sxs-lookup"><span data-stu-id="9d25f-134">In the following example, selecting the check box prevents click events from the child `<div>` from propagating to the parent `<div>`:</span></span>

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

<span data-ttu-id="9d25f-135">如需詳細資訊，請參閱[停止事件傳播](xref:blazor/components/event-handling#stop-event-propagation)。</span><span class="sxs-lookup"><span data-stu-id="9d25f-135">For more information, see [Stop event propagation](xref:blazor/components/event-handling#stop-event-propagation).</span></span>

## <a name="detailed-errors-during-blazor-app-development"></a><span data-ttu-id="9d25f-136">應用程式開發期間的詳細錯誤 Blazor</span><span class="sxs-lookup"><span data-stu-id="9d25f-136">Detailed errors during Blazor app development</span></span>

<span data-ttu-id="9d25f-137">當 Blazor 應用程式在開發期間無法正常運作時，從應用程式接收詳細的錯誤資訊有助於疑難排解和修正問題。</span><span class="sxs-lookup"><span data-stu-id="9d25f-137">When a Blazor app isn't functioning properly during development, receiving detailed error information from the app assists in troubleshooting and fixing the issue.</span></span> <span data-ttu-id="9d25f-138">發生錯誤時， Blazor 應用程式會在畫面底部顯示 [金色] 橫條：</span><span class="sxs-lookup"><span data-stu-id="9d25f-138">When an error occurs, Blazor apps display a gold bar at the bottom of the screen:</span></span>

* <span data-ttu-id="9d25f-139">在開發期間，「金級」列會將您導向至瀏覽器主控台，您可以在其中看到例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9d25f-139">During development, the gold bar directs you to the browser console, where you can see the exception.</span></span>
* <span data-ttu-id="9d25f-140">在生產環境中，「金級」列會通知使用者發生錯誤，並建議重新整理瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="9d25f-140">In production, the gold bar notifies the user that an error has occurred and recommends refreshing the browser.</span></span>

<span data-ttu-id="9d25f-141">如需詳細資訊，請參閱[開發期間的詳細錯誤](xref:blazor/fundamentals/handle-errors#detailed-errors-during-development)。</span><span class="sxs-lookup"><span data-stu-id="9d25f-141">For more information, see [Detailed errors during development](xref:blazor/fundamentals/handle-errors#detailed-errors-during-development).</span></span>
