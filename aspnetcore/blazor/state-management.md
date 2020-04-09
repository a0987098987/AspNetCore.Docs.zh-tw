---
title: ASP.NETBlazor核心狀態管理
author: guardrex
description: 瞭解如何在伺服器應用中Blazor保持狀態。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/state-management
ms.openlocfilehash: e8a1959a8fc05ea59362bb5824181a9d2e418811
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80218865"
---
# <a name="aspnet-core-opno-locblazor-state-management"></a><span data-ttu-id="28b1d-103">ASP.NETBlazor核心狀態管理</span><span class="sxs-lookup"><span data-stu-id="28b1d-103">ASP.NET Core Blazor state management</span></span>

<span data-ttu-id="28b1d-104">作者：[Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="28b1d-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="28b1d-105">伺服器是一個有狀態的應用框架。</span><span class="sxs-lookup"><span data-stu-id="28b1d-105"> Server is a stateful app framework.</span></span> <span data-ttu-id="28b1d-106">大多數時候,應用程式保持與伺服器的持續連接。</span><span class="sxs-lookup"><span data-stu-id="28b1d-106">Most of the time, the app maintains an ongoing connection to the server.</span></span> <span data-ttu-id="28b1d-107">用戶的狀態在*電路*中保持在伺服器的記憶體中。</span><span class="sxs-lookup"><span data-stu-id="28b1d-107">The user's state is held in the server's memory in a *circuit*.</span></span> 

<span data-ttu-id="28b1d-108">使用者電路的狀態範例包括:</span><span class="sxs-lookup"><span data-stu-id="28b1d-108">Examples of state held for a user's circuit include:</span></span>

* <span data-ttu-id="28b1d-109">呈現的&mdash;UI 是元件實例的層次結構及其最新的呈現輸出。</span><span class="sxs-lookup"><span data-stu-id="28b1d-109">The rendered UI&mdash;the hierarchy of component instances and their most recent render output.</span></span>
* <span data-ttu-id="28b1d-110">元件實例中任何欄位和屬性的值。</span><span class="sxs-lookup"><span data-stu-id="28b1d-110">The values of any fields and properties in component instances.</span></span>
* <span data-ttu-id="28b1d-111">在[依賴項注入 (DI)](xref:fundamentals/dependency-injection)服務實例中持有的數據,這些實例範圍可限定在電路中。</span><span class="sxs-lookup"><span data-stu-id="28b1d-111">Data held in [dependency injection (DI)](xref:fundamentals/dependency-injection) service instances that are scoped to the circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="28b1d-112">本文介紹伺服器應用中Blazor的狀態持久性。</span><span class="sxs-lookup"><span data-stu-id="28b1d-112">This article addresses state persistence in Blazor Server apps.</span></span> Blazor<span data-ttu-id="28b1d-113">WebAssembly 應用可以利用[瀏覽器中的用戶端狀態持久性,](#client-side-in-the-browser)但需要超出本文範圍的自定義解決方案或第三方包。</span><span class="sxs-lookup"><span data-stu-id="28b1d-113"> WebAssembly apps can take advantage of [client-side state persistence in the browser](#client-side-in-the-browser) but require custom solutions or 3rd party packages beyond the scope of this article.</span></span>

## <a name="opno-locblazor-circuits"></a>Blazor<span data-ttu-id="28b1d-114">電路</span><span class="sxs-lookup"><span data-stu-id="28b1d-114"> circuits</span></span>

<span data-ttu-id="28b1d-115">如果使用者遇到臨時網路連接丟失Blazor, 嘗試將使用者重新連接到其原始電路,以便他們可以繼續使用該應用程式。</span><span class="sxs-lookup"><span data-stu-id="28b1d-115">If a user experiences a temporary network connection loss, Blazor attempts to reconnect the user to their original circuit so they can continue to use the app.</span></span> <span data-ttu-id="28b1d-116">但是,在伺服器記憶體中將使用者重新連接到其原始電路並不總是可能的:</span><span class="sxs-lookup"><span data-stu-id="28b1d-116">However, reconnecting a user to their original circuit in the server's memory isn't always possible:</span></span>

* <span data-ttu-id="28b1d-117">伺服器無法永久保留斷開連接的電路。</span><span class="sxs-lookup"><span data-stu-id="28b1d-117">The server can't retain a disconnected circuit forever.</span></span> <span data-ttu-id="28b1d-118">伺服器必須在超時後或伺服器處於記憶體壓力下釋放斷開連接的電路。</span><span class="sxs-lookup"><span data-stu-id="28b1d-118">The server must release a disconnected circuit after a timeout or when the server is under memory pressure.</span></span>
* <span data-ttu-id="28b1d-119">在多伺服器負載均衡部署環境中,任何伺服器處理請求在任意給定時間都可能不可用。</span><span class="sxs-lookup"><span data-stu-id="28b1d-119">In multiserver, load-balanced deployment environments, any server processing requests may become unavailable at any given time.</span></span> <span data-ttu-id="28b1d-120">當不再需要處理請求的總量時,單個伺服器可能會失敗或自動刪除。</span><span class="sxs-lookup"><span data-stu-id="28b1d-120">Individual servers may fail or be automatically removed when no longer required to handle the overall volume of requests.</span></span> <span data-ttu-id="28b1d-121">當用戶嘗試重新連接時,原始伺服器可能不可用。</span><span class="sxs-lookup"><span data-stu-id="28b1d-121">The original server may not be available when the user attempts to reconnect.</span></span>
* <span data-ttu-id="28b1d-122">使用者可能會關閉並重新打開瀏覽器或重新載入頁面,從而刪除瀏覽器記憶體中保持的任何狀態。</span><span class="sxs-lookup"><span data-stu-id="28b1d-122">The user might close and re-open their browser or reload the page, which removes any state held in the browser's memory.</span></span> <span data-ttu-id="28b1d-123">例如,透過 JAVAScript 互通調用設置的值將丟失。</span><span class="sxs-lookup"><span data-stu-id="28b1d-123">For example, values set through JavaScript interop calls are lost.</span></span>

<span data-ttu-id="28b1d-124">當使用者無法重新連接到其原始電路時,使用者將收到一個狀態為空的新電路。</span><span class="sxs-lookup"><span data-stu-id="28b1d-124">When a user can't be reconnected to their original circuit, the user receives a new circuit with an empty state.</span></span> <span data-ttu-id="28b1d-125">這相當於關閉和重新打開桌面應用。</span><span class="sxs-lookup"><span data-stu-id="28b1d-125">This is equivalent to closing and re-opening a desktop app.</span></span>

## <a name="preserve-state-across-circuits"></a><span data-ttu-id="28b1d-126">跨電路保留狀態</span><span class="sxs-lookup"><span data-stu-id="28b1d-126">Preserve state across circuits</span></span>

<span data-ttu-id="28b1d-127">在某些情況下,需要保留跨電路的狀態。</span><span class="sxs-lookup"><span data-stu-id="28b1d-127">In some scenarios, preserving state across circuits is desirable.</span></span> <span data-ttu-id="28b1d-128">如果:</span><span class="sxs-lookup"><span data-stu-id="28b1d-128">An app can retain important data for a user if:</span></span>

* <span data-ttu-id="28b1d-129">Web 伺服器將不可用。</span><span class="sxs-lookup"><span data-stu-id="28b1d-129">The web server becomes unavailable.</span></span>
* <span data-ttu-id="28b1d-130">用戶的瀏覽器被迫使用新的 Web 伺服器啟動新電路。</span><span class="sxs-lookup"><span data-stu-id="28b1d-130">The user's browser is forced to start a new circuit with a new web server.</span></span>

<span data-ttu-id="28b1d-131">通常,跨電路維護狀態適用於使用者積極創建數據的情況,而不僅僅是讀取已經存在的數據。</span><span class="sxs-lookup"><span data-stu-id="28b1d-131">In general, maintaining state across circuits applies to scenarios where users are actively creating data, not simply reading data that already exists.</span></span>

<span data-ttu-id="28b1d-132">為了在單一電路之外保留狀態,*不要只將資料儲存在伺服器的記憶體中*。</span><span class="sxs-lookup"><span data-stu-id="28b1d-132">To preserve state beyond a single circuit, *don't merely store the data in the server's memory*.</span></span> <span data-ttu-id="28b1d-133">應用必須將數據保存到其他存儲位置。</span><span class="sxs-lookup"><span data-stu-id="28b1d-133">The app must persist the data to some other storage location.</span></span> <span data-ttu-id="28b1d-134">狀態持久性不是自動&mdash;的,在開發應用以實現有狀態的數據持久性時,您必須採取措施。</span><span class="sxs-lookup"><span data-stu-id="28b1d-134">State persistence isn't automatic&mdash;you must take steps when developing the app to implement stateful data persistence.</span></span>

<span data-ttu-id="28b1d-135">數據持久性通常只針對用戶花費努力創建的高價值狀態。</span><span class="sxs-lookup"><span data-stu-id="28b1d-135">Data persistence is typically only required for high-value state that users have expended effort to create.</span></span> <span data-ttu-id="28b1d-136">在以下範例中,持續狀態可以節省時間或幫助商務工作:</span><span class="sxs-lookup"><span data-stu-id="28b1d-136">In the following examples, persisting state either saves time or aids in commercial activities:</span></span>

* <span data-ttu-id="28b1d-137">多步驟 Webform&ndash;如果使用者的狀態丟失,則使用者重新輸入多步驟過程幾個已完成步驟的數據非常耗時。</span><span class="sxs-lookup"><span data-stu-id="28b1d-137">Multistep webform &ndash; It's time-consuming for a user to re-enter data for several completed steps of a multistep process if their state is lost.</span></span> <span data-ttu-id="28b1d-138">如果使用者從多步驟窗體導航,並在以後返回到窗體,則使用者將在此方案中失去狀態。</span><span class="sxs-lookup"><span data-stu-id="28b1d-138">A user loses state in this scenario if they navigate away from the multistep form and return to the form later.</span></span>
* <span data-ttu-id="28b1d-139">購物車&ndash;可以維護代表潛在收入的應用的任何商業重要元件。</span><span class="sxs-lookup"><span data-stu-id="28b1d-139">Shopping cart &ndash; Any commercially important component of an app that represents potential revenue can be maintained.</span></span> <span data-ttu-id="28b1d-140">失去狀態的使用者,因此他們的購物車,在稍後返回網站時可能會購買較少的產品或服務。</span><span class="sxs-lookup"><span data-stu-id="28b1d-140">A user who loses their state, and thus their shopping cart, may purchase fewer products or services when they return to the site later.</span></span>

<span data-ttu-id="28b1d-141">通常不需要保留易於重新建立的狀態,例如輸入到尚未提交的登錄對話方塊中的使用者名。</span><span class="sxs-lookup"><span data-stu-id="28b1d-141">It's usually not necessary to preserve easily-recreated state, such as the username entered into a sign-in dialog that hasn't been submitted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="28b1d-142">套用只能保留*套用狀態*。</span><span class="sxs-lookup"><span data-stu-id="28b1d-142">An app can only persist *app state*.</span></span> <span data-ttu-id="28b1d-143">無法持久化 U,例如元件實例及其呈現樹。</span><span class="sxs-lookup"><span data-stu-id="28b1d-143">UIs can't be persisted, such as component instances and their render trees.</span></span> <span data-ttu-id="28b1d-144">元件和呈現樹通常不可序列化。</span><span class="sxs-lookup"><span data-stu-id="28b1d-144">Components and render trees aren't generally serializable.</span></span> <span data-ttu-id="28b1d-145">要保留類似 UI 狀態的內容(如 TreeView 的擴展節點),應用必須具有自定義代碼,才能將行為建模為可序列化應用狀態。</span><span class="sxs-lookup"><span data-stu-id="28b1d-145">To persist something similar to UI state, such as the expanded nodes of a TreeView, the app must have custom code to model the behavior as serializable app state.</span></span>

## <a name="where-to-persist-state"></a><span data-ttu-id="28b1d-146">在何處保持狀態</span><span class="sxs-lookup"><span data-stu-id="28b1d-146">Where to persist state</span></span>

<span data-ttu-id="28b1d-147">存在三個Blazor常見位置,用於在伺服器應用中保持狀態。</span><span class="sxs-lookup"><span data-stu-id="28b1d-147">Three common locations exist for persisting state in a Blazor Server app.</span></span> <span data-ttu-id="28b1d-148">每種方法最適合不同的方案,並有不同的注意事項:</span><span class="sxs-lookup"><span data-stu-id="28b1d-148">Each approach is best suited to different scenarios and has different caveats:</span></span>

* [<span data-ttu-id="28b1d-149">資料庫中的伺服器端</span><span class="sxs-lookup"><span data-stu-id="28b1d-149">Server-side in a database</span></span>](#server-side-in-a-database)
* [<span data-ttu-id="28b1d-150">URL</span><span class="sxs-lookup"><span data-stu-id="28b1d-150">URL</span></span>](#url)
* [<span data-ttu-id="28b1d-151">瀏覽器中的用戶端</span><span class="sxs-lookup"><span data-stu-id="28b1d-151">Client-side in the browser</span></span>](#client-side-in-the-browser)

### <a name="server-side-in-a-database"></a><span data-ttu-id="28b1d-152">資料庫中的伺服器端</span><span class="sxs-lookup"><span data-stu-id="28b1d-152">Server-side in a database</span></span>

<span data-ttu-id="28b1d-153">對於永久數據持久性或任何必須跨越多個使用者或設備的數據,獨立的伺服器端資料庫幾乎肯定是最佳選擇。</span><span class="sxs-lookup"><span data-stu-id="28b1d-153">For permanent data persistence or for any data that must span multiple users or devices, an independent server-side database is almost certainly the best choice.</span></span> <span data-ttu-id="28b1d-154">選項包括：</span><span class="sxs-lookup"><span data-stu-id="28b1d-154">Options include:</span></span>

* <span data-ttu-id="28b1d-155">關聯 SQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="28b1d-155">Relational SQL database</span></span>
* <span data-ttu-id="28b1d-156">索引鍵-值存放區</span><span class="sxs-lookup"><span data-stu-id="28b1d-156">Key-value store</span></span>
* <span data-ttu-id="28b1d-157">Blob 儲存</span><span class="sxs-lookup"><span data-stu-id="28b1d-157">Blob store</span></span>
* <span data-ttu-id="28b1d-158">表儲存</span><span class="sxs-lookup"><span data-stu-id="28b1d-158">Table store</span></span>

<span data-ttu-id="28b1d-159">將數據保存到資料庫中后,用戶可以隨時啟動新電路。</span><span class="sxs-lookup"><span data-stu-id="28b1d-159">After data is saved in the database, a new circuit can be started by a user at any time.</span></span> <span data-ttu-id="28b1d-160">用戶的數據將保留並在任何新電路中可用。</span><span class="sxs-lookup"><span data-stu-id="28b1d-160">The user's data is retained and available in any new circuit.</span></span>

<span data-ttu-id="28b1d-161">關於 Azure 資料儲存選項的詳細資訊,請參考[Azure 儲存文件](/azure/storage/)與[Azure 資料庫](https://azure.microsoft.com/product-categories/databases/)。</span><span class="sxs-lookup"><span data-stu-id="28b1d-161">For more information on Azure data storage options, see the [Azure Storage Documentation](/azure/storage/) and [Azure Databases](https://azure.microsoft.com/product-categories/databases/).</span></span>

### <a name="url"></a><span data-ttu-id="28b1d-162">URL</span><span class="sxs-lookup"><span data-stu-id="28b1d-162">URL</span></span>

<span data-ttu-id="28b1d-163">對於表示導航狀態的瞬態數據,將資料建模為網址的一部分。</span><span class="sxs-lookup"><span data-stu-id="28b1d-163">For transient data representing navigation state, model the data as a part of the URL.</span></span> <span data-ttu-id="28b1d-164">在網址中建模的狀態範例包括:</span><span class="sxs-lookup"><span data-stu-id="28b1d-164">Examples of state modeled in the URL include:</span></span>

* <span data-ttu-id="28b1d-165">已查看實體的 ID。</span><span class="sxs-lookup"><span data-stu-id="28b1d-165">The ID of a viewed entity.</span></span>
* <span data-ttu-id="28b1d-166">分頁網格中的當前頁碼。</span><span class="sxs-lookup"><span data-stu-id="28b1d-166">The current page number in a paged grid.</span></span>

<span data-ttu-id="28b1d-167">保存瀏覽器位址列的內容:</span><span class="sxs-lookup"><span data-stu-id="28b1d-167">The contents of the browser's address bar are retained:</span></span>

* <span data-ttu-id="28b1d-168">如果用戶手動重新載入頁面。</span><span class="sxs-lookup"><span data-stu-id="28b1d-168">If the user manually reloads the page.</span></span>
* <span data-ttu-id="28b1d-169">如果 Web 伺服器&mdash;不可用 ,則使用者被迫重新載入頁面以連接到其他伺服器。</span><span class="sxs-lookup"><span data-stu-id="28b1d-169">If the web server becomes unavailable&mdash;the user is forced to reload the page in order to connect to a different server.</span></span>

<span data-ttu-id="28b1d-170">有關使用`@page`指令定義網址的資訊,請<xref:blazor/routing>參閱 。</span><span class="sxs-lookup"><span data-stu-id="28b1d-170">For information on defining URL patterns with the `@page` directive, see <xref:blazor/routing>.</span></span>

### <a name="client-side-in-the-browser"></a><span data-ttu-id="28b1d-171">瀏覽器中的用戶端</span><span class="sxs-lookup"><span data-stu-id="28b1d-171">Client-side in the browser</span></span>

<span data-ttu-id="28b1d-172">對於使用者正在積極創建的瞬態數據,常見的備份存儲是瀏覽器`localStorage`和`sessionStorage`集合。</span><span class="sxs-lookup"><span data-stu-id="28b1d-172">For transient data that the user is actively creating, a common backing store is the browser's `localStorage` and `sessionStorage` collections.</span></span> <span data-ttu-id="28b1d-173">如果電路被放棄,則不需要應用來管理或清除存儲狀態,這與伺服器端存儲具有優勢。</span><span class="sxs-lookup"><span data-stu-id="28b1d-173">The app isn't required to manage or clear the stored state if the circuit is abandoned, which is an advantage over server-side storage.</span></span>

> [!NOTE]
> <span data-ttu-id="28b1d-174">本節中的「用戶端」是指瀏覽器中的用戶端方案,而不是[BlazorWebAssembly 託管模型](xref:blazor/hosting-models#blazor-webassembly)。</span><span class="sxs-lookup"><span data-stu-id="28b1d-174">"Client-side" in this section refers to client-side scenarios in the browser, not the [Blazor WebAssembly hosting model](xref:blazor/hosting-models#blazor-webassembly).</span></span> <span data-ttu-id="28b1d-175">`localStorage``sessionStorage`和可以在BlazorWebAssembly 應用中使用,但只能透過編寫自訂代碼或使用第三方包。</span><span class="sxs-lookup"><span data-stu-id="28b1d-175">`localStorage` and `sessionStorage` can be used in Blazor WebAssembly apps but only by writing custom code or using a 3rd party package.</span></span>

<span data-ttu-id="28b1d-176">`localStorage`和`sessionStorage`不同的如下:</span><span class="sxs-lookup"><span data-stu-id="28b1d-176">`localStorage` and `sessionStorage` differ as follows:</span></span>

* <span data-ttu-id="28b1d-177">`localStorage`範圍限定為用戶的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="28b1d-177">`localStorage` is scoped to the user's browser.</span></span> <span data-ttu-id="28b1d-178">如果使用者重新載入頁面或關閉並重新打開瀏覽器,則狀態將保持不變。</span><span class="sxs-lookup"><span data-stu-id="28b1d-178">If the user reloads the page or closes and re-opens the browser, the state persists.</span></span> <span data-ttu-id="28b1d-179">如果用戶打開多個瀏覽器選項卡,則狀態將在選項卡之間共用。</span><span class="sxs-lookup"><span data-stu-id="28b1d-179">If the user opens multiple browser tabs, the state is shared across the tabs.</span></span> <span data-ttu-id="28b1d-180">資料會保留,`localStorage`直到顯式清除。</span><span class="sxs-lookup"><span data-stu-id="28b1d-180">Data persists in `localStorage` until explicitly cleared.</span></span>
* <span data-ttu-id="28b1d-181">`sessionStorage`限定到使用者的瀏覽器選項卡。如果使用者重新載入選項卡,則狀態將保持不變。</span><span class="sxs-lookup"><span data-stu-id="28b1d-181">`sessionStorage` is scoped to the user's browser tab. If the user reloads the tab, the state persists.</span></span> <span data-ttu-id="28b1d-182">如果使用者關閉選項卡或瀏覽器,狀態將丟失。</span><span class="sxs-lookup"><span data-stu-id="28b1d-182">If the user closes the tab or the browser, the state is lost.</span></span> <span data-ttu-id="28b1d-183">如果使用者打開多個瀏覽器選項卡,則每個選項卡都有其自己的獨立版本的數據。</span><span class="sxs-lookup"><span data-stu-id="28b1d-183">If the user opens multiple browser tabs, each tab has its own independent version of the data.</span></span>

<span data-ttu-id="28b1d-184">通常,`sessionStorage`使用更安全。</span><span class="sxs-lookup"><span data-stu-id="28b1d-184">Generally, `sessionStorage` is safer to use.</span></span> <span data-ttu-id="28b1d-185">`sessionStorage`避免了使用者打開多個選項卡並遇到以下情況的風險:</span><span class="sxs-lookup"><span data-stu-id="28b1d-185">`sessionStorage` avoids the risk that a user opens multiple tabs and encounters the following:</span></span>

* <span data-ttu-id="28b1d-186">跨選項卡的狀態存儲中的 Bug。</span><span class="sxs-lookup"><span data-stu-id="28b1d-186">Bugs in state storage across tabs.</span></span>
* <span data-ttu-id="28b1d-187">當選項卡覆蓋其他選項卡的狀態時,混淆行為。</span><span class="sxs-lookup"><span data-stu-id="28b1d-187">Confusing behavior when a tab overwrites the state of other tabs.</span></span>

<span data-ttu-id="28b1d-188">`localStorage`如果應用必須在關閉和重新打開瀏覽器時保持狀態,則是更好的選擇。</span><span class="sxs-lookup"><span data-stu-id="28b1d-188">`localStorage` is the better choice if the app must persist state across closing and re-opening the browser.</span></span>

<span data-ttu-id="28b1d-189">使用瀏覽器儲存的注意事項:</span><span class="sxs-lookup"><span data-stu-id="28b1d-189">Caveats for using browser storage:</span></span>

* <span data-ttu-id="28b1d-190">與使用伺服器端資料庫類似,載入和保存數據是異步的。</span><span class="sxs-lookup"><span data-stu-id="28b1d-190">Similar to the use of a server-side database, loading and saving data are asynchronous.</span></span>
* <span data-ttu-id="28b1d-191">與伺服器端資料庫不同,在預渲染期間存儲不可用,因為在預渲染階段瀏覽器中不存在請求的頁面。</span><span class="sxs-lookup"><span data-stu-id="28b1d-191">Unlike a server-side database, storage isn't available during prerendering because the requested page doesn't exist in the browser during the prerendering stage.</span></span>
* <span data-ttu-id="28b1d-192">存儲幾千位元組的數據Blazor對於伺服器應用是合理的。</span><span class="sxs-lookup"><span data-stu-id="28b1d-192">Storage of a few kilobytes of data is reasonable to persist for Blazor Server apps.</span></span> <span data-ttu-id="28b1d-193">除了幾千位元組之外,您還必須考慮性能影響,因為數據透過網路載入和保存。</span><span class="sxs-lookup"><span data-stu-id="28b1d-193">Beyond a few kilobytes, you must consider the performance implications because the data is loaded and saved across the network.</span></span>
* <span data-ttu-id="28b1d-194">用戶可以查看或篡改數據。</span><span class="sxs-lookup"><span data-stu-id="28b1d-194">Users may view or tamper with the data.</span></span> <span data-ttu-id="28b1d-195">ASP.NET核心[數據保護](xref:security/data-protection/introduction)可以降低風險。</span><span class="sxs-lookup"><span data-stu-id="28b1d-195">ASP.NET Core [Data Protection](xref:security/data-protection/introduction) can mitigate the risk.</span></span>

## <a name="third-party-browser-storage-solutions"></a><span data-ttu-id="28b1d-196">第三方瀏覽器儲存解決方案</span><span class="sxs-lookup"><span data-stu-id="28b1d-196">Third-party browser storage solutions</span></span>

<span data-ttu-id="28b1d-197">第三方 NuGet 套件提供`localStorage`用於`sessionStorage`使用和的 API。</span><span class="sxs-lookup"><span data-stu-id="28b1d-197">Third-party NuGet packages provide APIs for working with `localStorage` and `sessionStorage`.</span></span>

<span data-ttu-id="28b1d-198">值得考慮選擇透明地使用 ASP.NET 核心[資料保護的套件](xref:security/data-protection/introduction)。</span><span class="sxs-lookup"><span data-stu-id="28b1d-198">It's worth considering choosing a package that transparently uses ASP.NET Core's [Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="28b1d-199">ASP.NET核心數據保護對存儲的數據進行加密,並降低篡改存儲數據的潛在風險。</span><span class="sxs-lookup"><span data-stu-id="28b1d-199">ASP.NET Core Data Protection encrypts stored data and reduces the potential risk of tampering with stored data.</span></span> <span data-ttu-id="28b1d-200">如果 JSON 序列化數據儲存在純文本中,則使用者可以使用瀏覽器開發人員工具查看數據,還可以修改儲存的資料。</span><span class="sxs-lookup"><span data-stu-id="28b1d-200">If JSON-serialized data is stored in plaintext, users can see the data using browser developer tools and also modify the stored data.</span></span> <span data-ttu-id="28b1d-201">保護數據並不總是問題,因為數據在本質上可能微不足道。</span><span class="sxs-lookup"><span data-stu-id="28b1d-201">Securing data isn't always a problem because the data might be trivial in nature.</span></span> <span data-ttu-id="28b1d-202">例如,讀取或修改 UI 元素的儲存顏色對使用者或組織來說並不構成重大安全風險。</span><span class="sxs-lookup"><span data-stu-id="28b1d-202">For example, reading or modifying the stored color of a UI element isn't a significant security risk to the user or the organization.</span></span> <span data-ttu-id="28b1d-203">避免允許使用者檢查或篡改*敏感資料*。</span><span class="sxs-lookup"><span data-stu-id="28b1d-203">Avoid allowing users to inspect or tamper with *sensitive data*.</span></span>

## <a name="protected-browser-storage-experimental-package"></a><span data-ttu-id="28b1d-204">受保護的瀏覽器儲存實驗套件</span><span class="sxs-lookup"><span data-stu-id="28b1d-204">Protected Browser Storage experimental package</span></span>

<span data-ttu-id="28b1d-205">為 微軟提供[資料保護](xref:security/data-protection/introduction)`localStorage`的 NuGet`sessionStorage`套件範例 是[.AspNetCore.受保護的瀏覽器儲存](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage)。</span><span class="sxs-lookup"><span data-stu-id="28b1d-205">An example of a NuGet package that provides [Data Protection](xref:security/data-protection/introduction) for `localStorage` and `sessionStorage` is [Microsoft.AspNetCore.ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span></span>

> [!WARNING]
> <span data-ttu-id="28b1d-206">`Microsoft.AspNetCore.ProtectedBrowserStorage`是一個不支持的實驗包,不適合生產在這個時候使用。</span><span class="sxs-lookup"><span data-stu-id="28b1d-206">`Microsoft.AspNetCore.ProtectedBrowserStorage` is an unsupported experimental package unsuitable for production use at this time.</span></span>

### <a name="installation"></a><span data-ttu-id="28b1d-207">安裝</span><span class="sxs-lookup"><span data-stu-id="28b1d-207">Installation</span></span>

<span data-ttu-id="28b1d-208">要安裝`Microsoft.AspNetCore.ProtectedBrowserStorage`套件:</span><span class="sxs-lookup"><span data-stu-id="28b1d-208">To install the `Microsoft.AspNetCore.ProtectedBrowserStorage` package:</span></span>

1. <span data-ttu-id="28b1d-209">在Blazor「伺服器」應用專案中,添加對[Microsoft 的](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage)包引用。</span><span class="sxs-lookup"><span data-stu-id="28b1d-209">In the Blazor Server app project, add a package reference to [Microsoft.AspNetCore.ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span></span>
1. <span data-ttu-id="28b1d-210">在頂級 HTML 中(例如,在預設項目樣本中的*Pages/_Host.cshtml*檔案中),新增以下`<script>`標籤:</span><span class="sxs-lookup"><span data-stu-id="28b1d-210">In the top-level HTML (for example, in the *Pages/_Host.cshtml* file in the default project template), add the following `<script>` tag:</span></span>

   ```html
   <script src="_content/Microsoft.AspNetCore.ProtectedBrowserStorage/protectedBrowserStorage.js"></script>
   ```

1. <span data-ttu-id="28b1d-211">在`Startup.ConfigureServices`方法中,`AddProtectedBrowserStorage`呼`localStorage`叫`sessionStorage`以 新增和服務到服務集合:</span><span class="sxs-lookup"><span data-stu-id="28b1d-211">In the `Startup.ConfigureServices` method, call `AddProtectedBrowserStorage` to add `localStorage` and `sessionStorage` services to the service collection:</span></span>

   ```csharp
   services.AddProtectedBrowserStorage();
   ```

### <a name="save-and-load-data-within-a-component"></a><span data-ttu-id="28b1d-212">儲存在元件中儲存及載入資料</span><span class="sxs-lookup"><span data-stu-id="28b1d-212">Save and load data within a component</span></span>

<span data-ttu-id="28b1d-213">在需要將資料載入或儲存到瀏覽器儲存的任何元件中,使用[`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component)注入以下任一的實體:</span><span class="sxs-lookup"><span data-stu-id="28b1d-213">In any component that requires loading or saving data to browser storage, use [`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component) to inject an instance of either of the following:</span></span>

* `ProtectedLocalStorage`
* `ProtectedSessionStorage`

<span data-ttu-id="28b1d-214">選擇取決於您希望使用哪個後備存儲。</span><span class="sxs-lookup"><span data-stu-id="28b1d-214">The choice depends on which backing store you wish to use.</span></span> <span data-ttu-id="28b1d-215">在下面的範例中,`sessionStorage`使用:</span><span class="sxs-lookup"><span data-stu-id="28b1d-215">In the following example, `sessionStorage` is used:</span></span>

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore
```

<span data-ttu-id="28b1d-216">語句`@using`可以放置在 *_Imports.razor*檔中,而不是元件中。</span><span class="sxs-lookup"><span data-stu-id="28b1d-216">The `@using` statement can be placed into an *_Imports.razor* file instead of in the component.</span></span> <span data-ttu-id="28b1d-217">使用 *_Imports.razor*檔案使命名空間可用於應用或整個應用的較大部分。</span><span class="sxs-lookup"><span data-stu-id="28b1d-217">Use of the *_Imports.razor* file makes the namespace available to larger segments of the app or the whole app.</span></span>

<span data-ttu-id="28b1d-218">在`_currentCount`專案樣本的`Counter`元件中保留該值,`IncrementCount`請修改要`ProtectedSessionStore.SetAsync`使用的方法:</span><span class="sxs-lookup"><span data-stu-id="28b1d-218">To persist the `_currentCount` value in the `Counter` component of the project template, modify the `IncrementCount` method to use `ProtectedSessionStore.SetAsync`:</span></span>

```csharp
private async Task IncrementCount()
{
    _currentCount++;
    await ProtectedSessionStore.SetAsync("count", _currentCount);
}
```

<span data-ttu-id="28b1d-219">在更大、更現實的應用中,單個字段的存儲不太可能發生。</span><span class="sxs-lookup"><span data-stu-id="28b1d-219">In larger, more realistic apps, storage of individual fields is an unlikely scenario.</span></span> <span data-ttu-id="28b1d-220">應用更有可能存儲包含複雜狀態的整個模型物件。</span><span class="sxs-lookup"><span data-stu-id="28b1d-220">Apps are more likely to store entire model objects that include complex state.</span></span> <span data-ttu-id="28b1d-221">`ProtectedSessionStore`自動序列化和脫序列化 JSON 數據。</span><span class="sxs-lookup"><span data-stu-id="28b1d-221">`ProtectedSessionStore` automatically serializes and deserializes JSON data.</span></span>

<span data-ttu-id="28b1d-222">在前面的代碼範例中,`_currentCount`資料儲存`sessionStorage['count']`在用戶的瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="28b1d-222">In the preceding code example, the `_currentCount` data is stored as `sessionStorage['count']` in the user's browser.</span></span> <span data-ttu-id="28b1d-223">資料不是以純文字儲存的,而是使用ASP.NET核心的[資料保護進行保護](xref:security/data-protection/introduction)。</span><span class="sxs-lookup"><span data-stu-id="28b1d-223">The data isn't stored in plaintext but rather is protected using ASP.NET Core's [Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="28b1d-224">如果在`sessionStorage['count']`瀏覽器的開發人員控制台中評估加密數據,則可以看到加密數據。</span><span class="sxs-lookup"><span data-stu-id="28b1d-224">The encrypted data can be seen if `sessionStorage['count']` is evaluated in the browser's developer console.</span></span>

<span data-ttu-id="28b1d-225">要復`_currentCount`原資料,如果使用者稍後傳`Counter`回到 元件(包括如果他們在一個全新的電路上),請`ProtectedSessionStore.GetAsync`使用 :</span><span class="sxs-lookup"><span data-stu-id="28b1d-225">To recover the `_currentCount` data if the user returns to the `Counter` component later (including if they're on an entirely new circuit), use `ProtectedSessionStore.GetAsync`:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    _currentCount = await ProtectedSessionStore.GetAsync<int>("count");
}
```

<span data-ttu-id="28b1d-226">如果元件的參數包括導覽狀態,請在`ProtectedSessionStore.GetAsync``OnParametersSetAsync`中呼叫並分配結果,`OnInitializedAsync`而不是 。</span><span class="sxs-lookup"><span data-stu-id="28b1d-226">If the component's parameters include navigation state, call `ProtectedSessionStore.GetAsync` and assign the result in `OnParametersSetAsync`, not `OnInitializedAsync`.</span></span> <span data-ttu-id="28b1d-227">`OnInitializedAsync`僅在首次實例化元件時調用一次。</span><span class="sxs-lookup"><span data-stu-id="28b1d-227">`OnInitializedAsync` is only called one time when the component is first instantiated.</span></span> <span data-ttu-id="28b1d-228">`OnInitializedAsync`如果使用者在同一頁上導航到其他 URL,則以後不會再次調用。</span><span class="sxs-lookup"><span data-stu-id="28b1d-228">`OnInitializedAsync` isn't called again later if the user navigates to a different URL while remaining on the same page.</span></span> <span data-ttu-id="28b1d-229">如需詳細資訊，請參閱 <xref:blazor/lifecycle>。</span><span class="sxs-lookup"><span data-stu-id="28b1d-229">For more information, see <xref:blazor/lifecycle>.</span></span>

> [!WARNING]
> <span data-ttu-id="28b1d-230">僅當伺服器未啟用預渲染時,本節中的示例才起作用。</span><span class="sxs-lookup"><span data-stu-id="28b1d-230">The examples in this section only work if the server doesn't have prerendering enabled.</span></span> <span data-ttu-id="28b1d-231">開啟預成像後,將產生類似於以下錯誤:</span><span class="sxs-lookup"><span data-stu-id="28b1d-231">With prerendering enabled, an error is generated similar to:</span></span>
>
> > <span data-ttu-id="28b1d-232">此時無法發出 JavaScript 互通呼叫。</span><span class="sxs-lookup"><span data-stu-id="28b1d-232">JavaScript interop calls cannot be issued at this time.</span></span> <span data-ttu-id="28b1d-233">這是因為元件正在預呈現。</span><span class="sxs-lookup"><span data-stu-id="28b1d-233">This is because the component is being prerendered.</span></span>
>
> <span data-ttu-id="28b1d-234">禁用預渲染或添加其他代碼以處理預渲染。</span><span class="sxs-lookup"><span data-stu-id="28b1d-234">Either disable prerendering or add additional code to work with prerendering.</span></span> <span data-ttu-id="28b1d-235">要瞭解有關編寫適用於預算的代碼的更多詳細資訊,請參閱[「處理預渲染」](#handle-prerendering)部分。</span><span class="sxs-lookup"><span data-stu-id="28b1d-235">To learn more about writing code that works with prerendering, see the [Handle prerendering](#handle-prerendering) section.</span></span>

### <a name="handle-the-loading-state"></a><span data-ttu-id="28b1d-236">處理載入狀態</span><span class="sxs-lookup"><span data-stu-id="28b1d-236">Handle the loading state</span></span>

<span data-ttu-id="28b1d-237">由於瀏覽器存儲是異步的(通過網路連接訪問),因此在載入數據並可供元件使用之前,始終有一段時間。</span><span class="sxs-lookup"><span data-stu-id="28b1d-237">Since browser storage is asynchronous (accessed over a network connection), there's always a period of time before the data is loaded and available for use by a component.</span></span> <span data-ttu-id="28b1d-238">為獲得最佳結果,在載入正在進行時呈現載入狀態消息,而不是顯示空白或默認資料。</span><span class="sxs-lookup"><span data-stu-id="28b1d-238">For the best results, render a loading-state message while loading is in progress instead of displaying blank or default data.</span></span>

<span data-ttu-id="28b1d-239">一種方法是跟蹤數據是否正在`null`載入(仍在載入)。</span><span class="sxs-lookup"><span data-stu-id="28b1d-239">One approach is to track whether the data is `null` (still loading) or not.</span></span> <span data-ttu-id="28b1d-240">在預設`Counter`元件中,計數在中`int`保持。</span><span class="sxs-lookup"><span data-stu-id="28b1d-240">In the default `Counter` component, the count is held in an `int`.</span></span> <span data-ttu-id="28b1d-241">通過將`_currentCount`問號 (`?`)`int`添加到 類型 ( ) 使空。</span><span class="sxs-lookup"><span data-stu-id="28b1d-241">Make `_currentCount` nullable by adding a question mark (`?`) to the type (`int`):</span></span>

```csharp
private int? _currentCount;
```

<span data-ttu-id="28b1d-242">選擇僅在載入資料時顯示這些元素,而不是無條件顯示計數和**增量**按鈕:</span><span class="sxs-lookup"><span data-stu-id="28b1d-242">Instead of unconditionally displaying the count and **Increment** button, choose to display these elements only if the data is loaded:</span></span>

```razor
@if (_currentCount.HasValue)
{
    <p>Current count: <strong>@_currentCount</strong></p>

    <button @onclick="IncrementCount">Increment</button>
}
else
{
    <p>Loading...</p>
}
```

### <a name="handle-prerendering"></a><span data-ttu-id="28b1d-243">處理預像</span><span class="sxs-lookup"><span data-stu-id="28b1d-243">Handle prerendering</span></span>

<span data-ttu-id="28b1d-244">在預成成的紀錄期間:</span><span class="sxs-lookup"><span data-stu-id="28b1d-244">During prerendering:</span></span>

* <span data-ttu-id="28b1d-245">與用戶瀏覽器的互動式連接不存在。</span><span class="sxs-lookup"><span data-stu-id="28b1d-245">An interactive connection to the user's browser doesn't exist.</span></span>
* <span data-ttu-id="28b1d-246">瀏覽器還沒有一個頁面,它可以運行JAVAScript代碼。</span><span class="sxs-lookup"><span data-stu-id="28b1d-246">The browser doesn't yet have a page in which it can run JavaScript code.</span></span>

<span data-ttu-id="28b1d-247">`localStorage`或`sessionStorage`在預渲染期間不可用。</span><span class="sxs-lookup"><span data-stu-id="28b1d-247">`localStorage` or `sessionStorage` aren't available during prerendering.</span></span> <span data-ttu-id="28b1d-248">如果元件嘗試與儲存互動,則產生錯誤類似於:</span><span class="sxs-lookup"><span data-stu-id="28b1d-248">If the component attempts to interact with storage, an error is generated similar to:</span></span>

> <span data-ttu-id="28b1d-249">此時無法發出 JavaScript 互通呼叫。</span><span class="sxs-lookup"><span data-stu-id="28b1d-249">JavaScript interop calls cannot be issued at this time.</span></span> <span data-ttu-id="28b1d-250">這是因為元件正在預呈現。</span><span class="sxs-lookup"><span data-stu-id="28b1d-250">This is because the component is being prerendered.</span></span>

<span data-ttu-id="28b1d-251">解決錯誤的一種方法是禁用預渲染。</span><span class="sxs-lookup"><span data-stu-id="28b1d-251">One way to resolve the error is to disable prerendering.</span></span> <span data-ttu-id="28b1d-252">如果應用大量使用基於瀏覽器的存儲,這通常是最佳選擇。</span><span class="sxs-lookup"><span data-stu-id="28b1d-252">This is usually the best choice if the app makes heavy use of browser-based storage.</span></span> <span data-ttu-id="28b1d-253">預渲染會增加複雜性,並且對應用沒有好處,因為應用在可用或`localStorage``sessionStorage`可用之前無法預算任何有用的內容。</span><span class="sxs-lookup"><span data-stu-id="28b1d-253">Prerendering adds complexity and doesn't benefit the app because the app can't prerender any useful content until `localStorage` or `sessionStorage` are available.</span></span>

<span data-ttu-id="28b1d-254">要關閉預先成,請開啟*頁面/_Host.cshtml*檔`render-mode`並將[元件標記說明器](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server>變更為 。</span><span class="sxs-lookup"><span data-stu-id="28b1d-254">To disable prerendering, open the *Pages/_Host.cshtml* file and change the `render-mode` of the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper) to <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server>.</span></span>

<span data-ttu-id="28b1d-255">預渲染對於不使用`localStorage``sessionStorage`或的其他頁面可能很有用。</span><span class="sxs-lookup"><span data-stu-id="28b1d-255">Prerendering might be useful for other pages that don't use `localStorage` or `sessionStorage`.</span></span> <span data-ttu-id="28b1d-256">要保持預渲染啟用,請延遲載入操作,直到瀏覽器連接到電路。</span><span class="sxs-lookup"><span data-stu-id="28b1d-256">To keep prerendering enabled, defer the loading operation until the browser is connected to the circuit.</span></span> <span data-ttu-id="28b1d-257">以下是儲存計數器值的範例:</span><span class="sxs-lookup"><span data-stu-id="28b1d-257">The following is an example for storing a counter value:</span></span>

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedLocalStorage ProtectedLocalStore

... rendering code goes here ...

@code {
    private int? _currentCount;
    private bool _isConnected = false;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // When execution reaches this point, the first *interactive* render
            // is complete. The component has an active connection to the browser.
            _isConnected = true;
            await LoadStateAsync();
            StateHasChanged();
        }
    }

    private async Task LoadStateAsync()
    {
        _currentCount = await ProtectedLocalStore.GetAsync<int>("prerenderedCount");
    }

    private async Task IncrementCount()
    {
        _currentCount++;
        await ProtectedSessionStore.SetAsync("count", _currentCount);
    }
}
```

### <a name="factor-out-the-state-preservation-to-a-common-location"></a><span data-ttu-id="28b1d-258">將狀態儲存分解到公共位置</span><span class="sxs-lookup"><span data-stu-id="28b1d-258">Factor out the state preservation to a common location</span></span>

<span data-ttu-id="28b1d-259">如果許多元件依賴於基於瀏覽器的存儲,則多次重新實現狀態提供程式代碼會創建代碼重複。</span><span class="sxs-lookup"><span data-stu-id="28b1d-259">If many components rely on browser-based storage, re-implementing state provider code many times creates code duplication.</span></span> <span data-ttu-id="28b1d-260">避免程式碼重複的一個選項是建立封裝狀態*提供程式邏輯的狀態提供程式父元件*。</span><span class="sxs-lookup"><span data-stu-id="28b1d-260">One option for avoiding code duplication is to create a *state provider parent component* that encapsulates the state provider logic.</span></span> <span data-ttu-id="28b1d-261">子元件可以使用持久數據,而不考慮狀態持久性機制。</span><span class="sxs-lookup"><span data-stu-id="28b1d-261">Child components can work with persisted data without regard to the state persistence mechanism.</span></span>

<span data-ttu-id="28b1d-262">在以下`CounterStateProvider`元件範例中,將保留計數器資料:</span><span class="sxs-lookup"><span data-stu-id="28b1d-262">In the following example of a `CounterStateProvider` component, counter data is persisted:</span></span>

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore

@if (_hasLoaded)
{
    <CascadingValue Value="@this">
        @ChildContent
    </CascadingValue>
}
else
{
    <p>Loading...</p>
}

@code {
    private bool _hasLoaded;

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    public int CurrentCount { get; set; }

    protected override async Task OnInitializedAsync()
    {
        CurrentCount = await ProtectedSessionStore.GetAsync<int>("count");
        _hasLoaded = true;
    }

    public async Task SaveChangesAsync()
    {
        await ProtectedSessionStore.SetAsync("count", CurrentCount);
    }
}
```

<span data-ttu-id="28b1d-263">元件`CounterStateProvider`在載入完成之前不呈現其子內容來處理載入階段。</span><span class="sxs-lookup"><span data-stu-id="28b1d-263">The `CounterStateProvider` component handles the loading phase by not rendering its child content until loading is complete.</span></span>

<span data-ttu-id="28b1d-264">要使用該`CounterStateProvider`元件,將元件的實例環繞到需要存取計數器狀態的任何其他元件周圍。</span><span class="sxs-lookup"><span data-stu-id="28b1d-264">To use the `CounterStateProvider` component, wrap an instance of the component around any other component that requires access to the counter state.</span></span> <span data-ttu-id="28b1d-265">要使應用中的所有元件都能存取`CounterStateProvider`狀態,將元件環繞`Router``App`在 元件中 *(App.razor*):</span><span class="sxs-lookup"><span data-stu-id="28b1d-265">To make the state accessible to all components in an app, wrap the `CounterStateProvider` component around the `Router` in the `App` component (*App.razor*):</span></span>

```razor
<CounterStateProvider>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CounterStateProvider>
```

<span data-ttu-id="28b1d-266">包裝元件接收並可以修改持久化計數器狀態。</span><span class="sxs-lookup"><span data-stu-id="28b1d-266">Wrapped components receive and can modify the persisted counter state.</span></span> <span data-ttu-id="28b1d-267">以下`Counter`元件實現模式:</span><span class="sxs-lookup"><span data-stu-id="28b1d-267">The following `Counter` component implements the pattern:</span></span>

```razor
@page "/counter"

<p>Current count: <strong>@CounterStateProvider.CurrentCount</strong></p>

<button @onclick="IncrementCount">Increment</button>

@code {
    [CascadingParameter]
    private CounterStateProvider CounterStateProvider { get; set; }

    private async Task IncrementCount()
    {
        CounterStateProvider.CurrentCount++;
        await CounterStateProvider.SaveChangesAsync();
    }
}
```

<span data-ttu-id="28b1d-268">不需要上述元件與`ProtectedBrowserStorage`交互,也不處理"載入"階段。</span><span class="sxs-lookup"><span data-stu-id="28b1d-268">The preceding component isn't required to interact with `ProtectedBrowserStorage`, nor does it deal with a "loading" phase.</span></span>

<span data-ttu-id="28b1d-269">要處理前面所述的預算,`CounterStateProvider`可以修改,以便使用計數器數據的所有元件自動使用預算。</span><span class="sxs-lookup"><span data-stu-id="28b1d-269">To deal with prerendering as described earlier, `CounterStateProvider` can be amended so that all of the components that consume the counter data automatically work with prerendering.</span></span> <span data-ttu-id="28b1d-270">有關詳細資訊,請參閱["處理預渲染"](#handle-prerendering)部分。</span><span class="sxs-lookup"><span data-stu-id="28b1d-270">See the [Handle prerendering](#handle-prerendering) section for details.</span></span>

<span data-ttu-id="28b1d-271">通常,建議*狀態提供程式父元件*模式:</span><span class="sxs-lookup"><span data-stu-id="28b1d-271">In general, *state provider parent component* pattern is recommended:</span></span>

* <span data-ttu-id="28b1d-272">在許多其他元件中使用狀態。</span><span class="sxs-lookup"><span data-stu-id="28b1d-272">To consume state in many other components.</span></span>
* <span data-ttu-id="28b1d-273">如果只有一個頂級狀態物件要持久化。</span><span class="sxs-lookup"><span data-stu-id="28b1d-273">If there's just one top-level state object to persist.</span></span>

<span data-ttu-id="28b1d-274">要在不同位置保留許多不同的狀態物件並使用不同的物件子集,最好避免全域處理狀態的載入和保存。</span><span class="sxs-lookup"><span data-stu-id="28b1d-274">To persist many different state objects and consume different subsets of objects in different places, it's better to avoid handling the loading and saving of state globally.</span></span>
