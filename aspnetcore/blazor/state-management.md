---
title: ASP.NET Core Blazor 狀態管理
author: guardrex
description: 瞭解如何在 Blazor 伺服器應用程式中保存狀態。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/state-management
ms.openlocfilehash: 59adcce972b503a6aa6e596bc9bff63225961f84
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243196"
---
# <a name="aspnet-core-blazor-state-management"></a><span data-ttu-id="5bad8-103">ASP.NET Core Blazor 狀態管理</span><span class="sxs-lookup"><span data-stu-id="5bad8-103">ASP.NET Core Blazor state management</span></span>

<span data-ttu-id="5bad8-104">作者：[Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="5bad8-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

Blazor<span data-ttu-id="5bad8-105">伺服器是可設定狀態的應用程式架構。</span><span class="sxs-lookup"><span data-stu-id="5bad8-105"> Server is a stateful app framework.</span></span> <span data-ttu-id="5bad8-106">在大部分的情況下，應用程式會維護與伺服器之間的持續連接。</span><span class="sxs-lookup"><span data-stu-id="5bad8-106">Most of the time, the app maintains an ongoing connection to the server.</span></span> <span data-ttu-id="5bad8-107">使用者的狀態會保留在*伺服器的記憶體中。*</span><span class="sxs-lookup"><span data-stu-id="5bad8-107">The user's state is held in the server's memory in a *circuit*.</span></span> 

<span data-ttu-id="5bad8-108">保留給使用者線路的狀態範例包括：</span><span class="sxs-lookup"><span data-stu-id="5bad8-108">Examples of state held for a user's circuit include:</span></span>

* <span data-ttu-id="5bad8-109">呈現的 UI：元件實例及其最新轉譯輸出的階層。</span><span class="sxs-lookup"><span data-stu-id="5bad8-109">The rendered UI: The hierarchy of component instances and their most recent render output.</span></span>
* <span data-ttu-id="5bad8-110">元件實例中任何欄位和屬性的值。</span><span class="sxs-lookup"><span data-stu-id="5bad8-110">The values of any fields and properties in component instances.</span></span>
* <span data-ttu-id="5bad8-111">保留在範圍為線路之相依性[插入（DI）](xref:fundamentals/dependency-injection)服務實例中的資料。</span><span class="sxs-lookup"><span data-stu-id="5bad8-111">Data held in [dependency injection (DI)](xref:fundamentals/dependency-injection) service instances that are scoped to the circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="5bad8-112">本文解決伺服器應用程式中的狀態持續性 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="5bad8-112">This article addresses state persistence in Blazor Server apps.</span></span> Blazor<span data-ttu-id="5bad8-113">WebAssembly apps 可以利用[瀏覽器中的用戶端狀態持續](#client-side-in-the-browser)性，但需要的自訂解決方案或協力廠商套件超出本文的範圍。</span><span class="sxs-lookup"><span data-stu-id="5bad8-113"> WebAssembly apps can take advantage of [client-side state persistence in the browser](#client-side-in-the-browser) but require custom solutions or 3rd party packages beyond the scope of this article.</span></span>

## <a name="blazor-circuits"></a>Blazor<span data-ttu-id="5bad8-114">獲得</span><span class="sxs-lookup"><span data-stu-id="5bad8-114"> circuits</span></span>

<span data-ttu-id="5bad8-115">如果使用者遇到暫時性的網路連線遺失， Blazor 會嘗試將使用者重新連接到其原始線路，讓他們可以繼續使用應用程式。</span><span class="sxs-lookup"><span data-stu-id="5bad8-115">If a user experiences a temporary network connection loss, Blazor attempts to reconnect the user to their original circuit so they can continue to use the app.</span></span> <span data-ttu-id="5bad8-116">不過，不一定能夠將使用者重新連接到伺服器記憶體中的原始線路：</span><span class="sxs-lookup"><span data-stu-id="5bad8-116">However, reconnecting a user to their original circuit in the server's memory isn't always possible:</span></span>

* <span data-ttu-id="5bad8-117">伺服器無法永久保留中斷連線的線路。</span><span class="sxs-lookup"><span data-stu-id="5bad8-117">The server can't retain a disconnected circuit forever.</span></span> <span data-ttu-id="5bad8-118">伺服器必須在超時之後或伺服器處於記憶體不足的壓力時，釋放中斷連線的線路。</span><span class="sxs-lookup"><span data-stu-id="5bad8-118">The server must release a disconnected circuit after a timeout or when the server is under memory pressure.</span></span>
* <span data-ttu-id="5bad8-119">在多伺服器、負載平衡的部署環境中，任何指定的時間都可能無法使用任何伺服器處理要求。</span><span class="sxs-lookup"><span data-stu-id="5bad8-119">In multiserver, load-balanced deployment environments, any server processing requests may become unavailable at any given time.</span></span> <span data-ttu-id="5bad8-120">當不再需要處理要求的整體量時，個別伺服器可能會失敗或自動移除。</span><span class="sxs-lookup"><span data-stu-id="5bad8-120">Individual servers may fail or be automatically removed when no longer required to handle the overall volume of requests.</span></span> <span data-ttu-id="5bad8-121">當使用者嘗試重新連線時，源伺服器可能無法使用。</span><span class="sxs-lookup"><span data-stu-id="5bad8-121">The original server may not be available when the user attempts to reconnect.</span></span>
* <span data-ttu-id="5bad8-122">使用者可能會關閉並重新開啟其瀏覽器或重載頁面，這會移除瀏覽器記憶體中保留的任何狀態。</span><span class="sxs-lookup"><span data-stu-id="5bad8-122">The user might close and re-open their browser or reload the page, which removes any state held in the browser's memory.</span></span> <span data-ttu-id="5bad8-123">例如，透過 JavaScript interop 呼叫所設定的值會遺失。</span><span class="sxs-lookup"><span data-stu-id="5bad8-123">For example, values set through JavaScript interop calls are lost.</span></span>

<span data-ttu-id="5bad8-124">當使用者無法重新連線到其原始線路時，使用者會收到空白狀態的新線路。</span><span class="sxs-lookup"><span data-stu-id="5bad8-124">When a user can't be reconnected to their original circuit, the user receives a new circuit with an empty state.</span></span> <span data-ttu-id="5bad8-125">這相當於關閉和重新開啟桌面應用程式。</span><span class="sxs-lookup"><span data-stu-id="5bad8-125">This is equivalent to closing and re-opening a desktop app.</span></span>

## <a name="preserve-state-across-circuits"></a><span data-ttu-id="5bad8-126">跨線路保留狀態</span><span class="sxs-lookup"><span data-stu-id="5bad8-126">Preserve state across circuits</span></span>

<span data-ttu-id="5bad8-127">在某些情況下，您需要保留各線路的狀態。</span><span class="sxs-lookup"><span data-stu-id="5bad8-127">In some scenarios, preserving state across circuits is desirable.</span></span> <span data-ttu-id="5bad8-128">應用程式可以保留使用者的重要資料，如果：</span><span class="sxs-lookup"><span data-stu-id="5bad8-128">An app can retain important data for a user if:</span></span>

* <span data-ttu-id="5bad8-129">網頁伺服器變得無法使用。</span><span class="sxs-lookup"><span data-stu-id="5bad8-129">The web server becomes unavailable.</span></span>
* <span data-ttu-id="5bad8-130">使用者的瀏覽器會強制使用新的 web 伺服器來啟動新的線路。</span><span class="sxs-lookup"><span data-stu-id="5bad8-130">The user's browser is forced to start a new circuit with a new web server.</span></span>

<span data-ttu-id="5bad8-131">一般而言，跨線路維護狀態適用于使用者主動建立資料的情況，而不只是讀取已經存在的資料。</span><span class="sxs-lookup"><span data-stu-id="5bad8-131">In general, maintaining state across circuits applies to scenarios where users are actively creating data, not simply reading data that already exists.</span></span>

<span data-ttu-id="5bad8-132">若要保留超過單一線路的狀態，*請勿只將資料儲存在伺服器的記憶體中*。</span><span class="sxs-lookup"><span data-stu-id="5bad8-132">To preserve state beyond a single circuit, *don't merely store the data in the server's memory*.</span></span> <span data-ttu-id="5bad8-133">應用程式必須將資料保存到其他儲存位置。</span><span class="sxs-lookup"><span data-stu-id="5bad8-133">The app must persist the data to some other storage location.</span></span> <span data-ttu-id="5bad8-134">狀態持續性不是自動的。</span><span class="sxs-lookup"><span data-stu-id="5bad8-134">State persistence isn't automatic.</span></span> <span data-ttu-id="5bad8-135">開發應用程式時，您必須採取步驟來執行具狀態的資料持續性。</span><span class="sxs-lookup"><span data-stu-id="5bad8-135">You must take steps when developing the app to implement stateful data persistence.</span></span>

<span data-ttu-id="5bad8-136">通常只有在高價值的狀態下，使用者才需要建立資料持續性。</span><span class="sxs-lookup"><span data-stu-id="5bad8-136">Data persistence is typically only required for high-value state that users have expended effort to create.</span></span> <span data-ttu-id="5bad8-137">在下列範例中，保存狀態可以節省商務工作的時間或輔助：</span><span class="sxs-lookup"><span data-stu-id="5bad8-137">In the following examples, persisting state either saves time or aids in commercial activities:</span></span>

* <span data-ttu-id="5bad8-138">多步驟 webform：使用者在其狀態遺失時，針對多步驟程式的數個已完成步驟重新輸入資料相當耗時。</span><span class="sxs-lookup"><span data-stu-id="5bad8-138">Multistep webform: It's time-consuming for a user to re-enter data for several completed steps of a multistep process if their state is lost.</span></span> <span data-ttu-id="5bad8-139">如果使用者離開多步驟表單，並在稍後返回表單，則會在此案例中失去狀態。</span><span class="sxs-lookup"><span data-stu-id="5bad8-139">A user loses state in this scenario if they navigate away from the multistep form and return to the form later.</span></span>
* <span data-ttu-id="5bad8-140">購物車：可以維護應用程式中代表潛在收益的任何商業重要元件。</span><span class="sxs-lookup"><span data-stu-id="5bad8-140">Shopping cart: Any commercially important component of an app that represents potential revenue can be maintained.</span></span> <span data-ttu-id="5bad8-141">失去其狀態的使用者，因此其購物車，可能會在日後返回網站時購買較少的產品或服務。</span><span class="sxs-lookup"><span data-stu-id="5bad8-141">A user who loses their state, and thus their shopping cart, may purchase fewer products or services when they return to the site later.</span></span>

<span data-ttu-id="5bad8-142">通常不需要保留容易重新建立的狀態，例如輸入登入對話方塊中未提交的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="5bad8-142">It's usually not necessary to preserve easily-recreated state, such as the username entered into a sign-in dialog that hasn't been submitted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5bad8-143">應用程式只能保存*應用程式狀態*。</span><span class="sxs-lookup"><span data-stu-id="5bad8-143">An app can only persist *app state*.</span></span> <span data-ttu-id="5bad8-144">Ui 無法保存，例如元件實例和其呈現樹狀結構。</span><span class="sxs-lookup"><span data-stu-id="5bad8-144">UIs can't be persisted, such as component instances and their render trees.</span></span> <span data-ttu-id="5bad8-145">元件和轉譯樹狀結構通常不是可序列化的。</span><span class="sxs-lookup"><span data-stu-id="5bad8-145">Components and render trees aren't generally serializable.</span></span> <span data-ttu-id="5bad8-146">若要保存類似于 UI 狀態的內容（例如 TreeView 的展開節點），應用程式必須有自訂程式碼，以將行為模型化為可序列化的應用程式狀態。</span><span class="sxs-lookup"><span data-stu-id="5bad8-146">To persist something similar to UI state, such as the expanded nodes of a TreeView, the app must have custom code to model the behavior as serializable app state.</span></span>

## <a name="where-to-persist-state"></a><span data-ttu-id="5bad8-147">保存狀態的位置</span><span class="sxs-lookup"><span data-stu-id="5bad8-147">Where to persist state</span></span>

<span data-ttu-id="5bad8-148">在伺服器應用程式中保存狀態有三個常見的位置 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="5bad8-148">Three common locations exist for persisting state in a Blazor Server app.</span></span> <span data-ttu-id="5bad8-149">每個方法最適合不同的案例，而且有不同的注意事項：</span><span class="sxs-lookup"><span data-stu-id="5bad8-149">Each approach is best suited to different scenarios and has different caveats:</span></span>

* [<span data-ttu-id="5bad8-150">資料庫中的伺服器端</span><span class="sxs-lookup"><span data-stu-id="5bad8-150">Server-side in a database</span></span>](#server-side-in-a-database)
* [<span data-ttu-id="5bad8-151">URL</span><span class="sxs-lookup"><span data-stu-id="5bad8-151">URL</span></span>](#url)
* [<span data-ttu-id="5bad8-152">瀏覽器中的用戶端</span><span class="sxs-lookup"><span data-stu-id="5bad8-152">Client-side in the browser</span></span>](#client-side-in-the-browser)

### <a name="server-side-in-a-database"></a><span data-ttu-id="5bad8-153">資料庫中的伺服器端</span><span class="sxs-lookup"><span data-stu-id="5bad8-153">Server-side in a database</span></span>

<span data-ttu-id="5bad8-154">針對永久資料保存或任何必須跨越多個使用者或裝置的資料，獨立的伺服器端資料庫幾乎都是最好的選擇。</span><span class="sxs-lookup"><span data-stu-id="5bad8-154">For permanent data persistence or for any data that must span multiple users or devices, an independent server-side database is almost certainly the best choice.</span></span> <span data-ttu-id="5bad8-155">這些選項包括：</span><span class="sxs-lookup"><span data-stu-id="5bad8-155">Options include:</span></span>

* <span data-ttu-id="5bad8-156">關係 SQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="5bad8-156">Relational SQL database</span></span>
* <span data-ttu-id="5bad8-157">索引鍵-值存放區</span><span class="sxs-lookup"><span data-stu-id="5bad8-157">Key-value store</span></span>
* <span data-ttu-id="5bad8-158">Blob 存放區</span><span class="sxs-lookup"><span data-stu-id="5bad8-158">Blob store</span></span>
* <span data-ttu-id="5bad8-159">資料表存放區</span><span class="sxs-lookup"><span data-stu-id="5bad8-159">Table store</span></span>

<span data-ttu-id="5bad8-160">將資料儲存在資料庫之後，使用者可以隨時啟動新的電路。</span><span class="sxs-lookup"><span data-stu-id="5bad8-160">After data is saved in the database, a new circuit can be started by a user at any time.</span></span> <span data-ttu-id="5bad8-161">使用者的資料會保留在任何新的線路中並可供使用。</span><span class="sxs-lookup"><span data-stu-id="5bad8-161">The user's data is retained and available in any new circuit.</span></span>

<span data-ttu-id="5bad8-162">如需 Azure 資料儲存體選項的詳細資訊，請參閱[Azure 儲存體檔](/azure/storage/)和[azure 資料庫](https://azure.microsoft.com/product-categories/databases/)。</span><span class="sxs-lookup"><span data-stu-id="5bad8-162">For more information on Azure data storage options, see the [Azure Storage Documentation](/azure/storage/) and [Azure Databases](https://azure.microsoft.com/product-categories/databases/).</span></span>

### <a name="url"></a><span data-ttu-id="5bad8-163">URL</span><span class="sxs-lookup"><span data-stu-id="5bad8-163">URL</span></span>

<span data-ttu-id="5bad8-164">對於代表導覽狀態的暫時性資料，請將資料模型為 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="5bad8-164">For transient data representing navigation state, model the data as a part of the URL.</span></span> <span data-ttu-id="5bad8-165">在 URL 中模型化的狀態範例包括：</span><span class="sxs-lookup"><span data-stu-id="5bad8-165">Examples of state modeled in the URL include:</span></span>

* <span data-ttu-id="5bad8-166">已查看之實體的識別碼。</span><span class="sxs-lookup"><span data-stu-id="5bad8-166">The ID of a viewed entity.</span></span>
* <span data-ttu-id="5bad8-167">分頁方格中的目前頁碼。</span><span class="sxs-lookup"><span data-stu-id="5bad8-167">The current page number in a paged grid.</span></span>

<span data-ttu-id="5bad8-168">瀏覽器的網址列內容會保留：</span><span class="sxs-lookup"><span data-stu-id="5bad8-168">The contents of the browser's address bar are retained:</span></span>

* <span data-ttu-id="5bad8-169">如果使用者手動重載頁面。</span><span class="sxs-lookup"><span data-stu-id="5bad8-169">If the user manually reloads the page.</span></span>
* <span data-ttu-id="5bad8-170">如果網頁伺服器變得無法使用，而且會強制使用者重載頁面，以便連接到不同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="5bad8-170">If the web server becomes unavailable, and the user is forced to reload the page in order to connect to a different server.</span></span>

<span data-ttu-id="5bad8-171">如需使用指示詞定義 URL 模式的詳細資訊 `@page` ，請參閱 <xref:blazor/fundamentals/routing> 。</span><span class="sxs-lookup"><span data-stu-id="5bad8-171">For information on defining URL patterns with the `@page` directive, see <xref:blazor/fundamentals/routing>.</span></span>

### <a name="client-side-in-the-browser"></a><span data-ttu-id="5bad8-172">瀏覽器中的用戶端</span><span class="sxs-lookup"><span data-stu-id="5bad8-172">Client-side in the browser</span></span>

<span data-ttu-id="5bad8-173">對於使用者主動建立的暫時性資料，常見的備份存放區是瀏覽器的 `localStorage` 和 `sessionStorage` 集合。</span><span class="sxs-lookup"><span data-stu-id="5bad8-173">For transient data that the user is actively creating, a common backing store is the browser's `localStorage` and `sessionStorage` collections.</span></span> <span data-ttu-id="5bad8-174">若已放棄迴圈，則不需要應用程式來管理或清除已儲存的狀態，這是優於伺服器端儲存體的優點。</span><span class="sxs-lookup"><span data-stu-id="5bad8-174">The app isn't required to manage or clear the stored state if the circuit is abandoned, which is an advantage over server-side storage.</span></span>

> [!NOTE]
> <span data-ttu-id="5bad8-175">本節中的「用戶端」指的是瀏覽器中的用戶端案例，而不是[ Blazor WebAssembly 裝載模型](xref:blazor/hosting-models#blazor-webassembly)。</span><span class="sxs-lookup"><span data-stu-id="5bad8-175">"Client-side" in this section refers to client-side scenarios in the browser, not the [Blazor WebAssembly hosting model](xref:blazor/hosting-models#blazor-webassembly).</span></span> <span data-ttu-id="5bad8-176">`localStorage`和 `sessionStorage` 可以在 WebAssembly apps 中使用， Blazor 但只能透過撰寫自訂程式碼或使用協力廠商套件。</span><span class="sxs-lookup"><span data-stu-id="5bad8-176">`localStorage` and `sessionStorage` can be used in Blazor WebAssembly apps but only by writing custom code or using a 3rd party package.</span></span>

<span data-ttu-id="5bad8-177">`localStorage`和 `sessionStorage` 的差異如下：</span><span class="sxs-lookup"><span data-stu-id="5bad8-177">`localStorage` and `sessionStorage` differ as follows:</span></span>

* <span data-ttu-id="5bad8-178">`localStorage`的範圍設定為使用者的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="5bad8-178">`localStorage` is scoped to the user's browser.</span></span> <span data-ttu-id="5bad8-179">如果使用者重載頁面，或關閉並重新開啟瀏覽器，則狀態會保持不變。</span><span class="sxs-lookup"><span data-stu-id="5bad8-179">If the user reloads the page or closes and re-opens the browser, the state persists.</span></span> <span data-ttu-id="5bad8-180">如果使用者開啟多個瀏覽器索引標籤，則狀態會在索引標籤之間共用。</span><span class="sxs-lookup"><span data-stu-id="5bad8-180">If the user opens multiple browser tabs, the state is shared across the tabs.</span></span> <span data-ttu-id="5bad8-181">資料會保存在中， `localStorage` 直到明確清除為止。</span><span class="sxs-lookup"><span data-stu-id="5bad8-181">Data persists in `localStorage` until explicitly cleared.</span></span>
* <span data-ttu-id="5bad8-182">`sessionStorage`的範圍設定為使用者的瀏覽器索引標籤。如果使用者重載索引標籤，則狀態會保持不變。</span><span class="sxs-lookup"><span data-stu-id="5bad8-182">`sessionStorage` is scoped to the user's browser tab. If the user reloads the tab, the state persists.</span></span> <span data-ttu-id="5bad8-183">如果使用者關閉索引標籤或瀏覽器，則狀態會遺失。</span><span class="sxs-lookup"><span data-stu-id="5bad8-183">If the user closes the tab or the browser, the state is lost.</span></span> <span data-ttu-id="5bad8-184">如果使用者開啟多個瀏覽器索引標籤，每個索引標籤都有自己獨立的資料版本。</span><span class="sxs-lookup"><span data-stu-id="5bad8-184">If the user opens multiple browser tabs, each tab has its own independent version of the data.</span></span>

<span data-ttu-id="5bad8-185">一般而言， `sessionStorage` 使用較安全。</span><span class="sxs-lookup"><span data-stu-id="5bad8-185">Generally, `sessionStorage` is safer to use.</span></span> <span data-ttu-id="5bad8-186">`sessionStorage`避免使用者開啟多個索引標籤並遇到下列問題的風險：</span><span class="sxs-lookup"><span data-stu-id="5bad8-186">`sessionStorage` avoids the risk that a user opens multiple tabs and encounters the following:</span></span>

* <span data-ttu-id="5bad8-187">索引標籤上狀態儲存體中的 bug。</span><span class="sxs-lookup"><span data-stu-id="5bad8-187">Bugs in state storage across tabs.</span></span>
* <span data-ttu-id="5bad8-188">當索引標籤覆寫其他索引標籤的狀態時，會造成混淆的行為。</span><span class="sxs-lookup"><span data-stu-id="5bad8-188">Confusing behavior when a tab overwrites the state of other tabs.</span></span>

<span data-ttu-id="5bad8-189">`localStorage`如果應用程式必須在關閉並重新開啟瀏覽器時保存狀態，這是較好的選擇。</span><span class="sxs-lookup"><span data-stu-id="5bad8-189">`localStorage` is the better choice if the app must persist state across closing and re-opening the browser.</span></span>

<span data-ttu-id="5bad8-190">使用瀏覽器儲存的注意事項：</span><span class="sxs-lookup"><span data-stu-id="5bad8-190">Caveats for using browser storage:</span></span>

* <span data-ttu-id="5bad8-191">類似于使用伺服器端資料庫，載入和儲存資料是非同步。</span><span class="sxs-lookup"><span data-stu-id="5bad8-191">Similar to the use of a server-side database, loading and saving data are asynchronous.</span></span>
* <span data-ttu-id="5bad8-192">與伺服器端資料庫不同的是，預先呈現期間無法使用儲存體，因為在預先呈現階段期間，瀏覽器中不存在要求的頁面。</span><span class="sxs-lookup"><span data-stu-id="5bad8-192">Unlike a server-side database, storage isn't available during prerendering because the requested page doesn't exist in the browser during the prerendering stage.</span></span>
* <span data-ttu-id="5bad8-193">儲存幾 kb 的資料可合理保存 Blazor 伺服器應用程式。</span><span class="sxs-lookup"><span data-stu-id="5bad8-193">Storage of a few kilobytes of data is reasonable to persist for Blazor Server apps.</span></span> <span data-ttu-id="5bad8-194">除了幾 kb 以外，您還必須考慮效能上的影響，因為資料是透過網路載入和儲存。</span><span class="sxs-lookup"><span data-stu-id="5bad8-194">Beyond a few kilobytes, you must consider the performance implications because the data is loaded and saved across the network.</span></span>
* <span data-ttu-id="5bad8-195">使用者可能會看到或篡改資料。</span><span class="sxs-lookup"><span data-stu-id="5bad8-195">Users may view or tamper with the data.</span></span> <span data-ttu-id="5bad8-196">ASP.NET Core[資料保護](xref:security/data-protection/introduction)可以降低風險。</span><span class="sxs-lookup"><span data-stu-id="5bad8-196">ASP.NET Core [Data Protection](xref:security/data-protection/introduction) can mitigate the risk.</span></span>

## <a name="third-party-browser-storage-solutions"></a><span data-ttu-id="5bad8-197">協力廠商瀏覽器儲存解決方案</span><span class="sxs-lookup"><span data-stu-id="5bad8-197">Third-party browser storage solutions</span></span>

<span data-ttu-id="5bad8-198">協力廠商 NuGet 套件提供使用和的 Api `localStorage` `sessionStorage` 。</span><span class="sxs-lookup"><span data-stu-id="5bad8-198">Third-party NuGet packages provide APIs for working with `localStorage` and `sessionStorage`.</span></span>

<span data-ttu-id="5bad8-199">值得考慮選擇以透明方式使用 ASP.NET Core[資料保護](xref:security/data-protection/introduction)的套件。</span><span class="sxs-lookup"><span data-stu-id="5bad8-199">It's worth considering choosing a package that transparently uses ASP.NET Core's [Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="5bad8-200">ASP.NET Core 資料保護會將儲存的資料加密，並降低篡改已儲存資料的潛在風險。</span><span class="sxs-lookup"><span data-stu-id="5bad8-200">ASP.NET Core Data Protection encrypts stored data and reduces the potential risk of tampering with stored data.</span></span> <span data-ttu-id="5bad8-201">如果 JSON 序列化資料是以純文字儲存，則使用者可以使用瀏覽器開發人員工具來查看資料，同時也會修改儲存的資料。</span><span class="sxs-lookup"><span data-stu-id="5bad8-201">If JSON-serialized data is stored in plaintext, users can see the data using browser developer tools and also modify the stored data.</span></span> <span data-ttu-id="5bad8-202">保護資料不一定會有問題，因為資料在本質上可能是很簡單的。</span><span class="sxs-lookup"><span data-stu-id="5bad8-202">Securing data isn't always a problem because the data might be trivial in nature.</span></span> <span data-ttu-id="5bad8-203">例如，讀取或修改 UI 專案的預存色彩對使用者或組織而言並不會有嚴重的安全性風險。</span><span class="sxs-lookup"><span data-stu-id="5bad8-203">For example, reading or modifying the stored color of a UI element isn't a significant security risk to the user or the organization.</span></span> <span data-ttu-id="5bad8-204">避免允許使用者檢查或篡改*敏感性資料*。</span><span class="sxs-lookup"><span data-stu-id="5bad8-204">Avoid allowing users to inspect or tamper with *sensitive data*.</span></span>

## <a name="protected-browser-storage-experimental-package"></a><span data-ttu-id="5bad8-205">受保護的瀏覽器儲存體實驗性封裝</span><span class="sxs-lookup"><span data-stu-id="5bad8-205">Protected Browser Storage experimental package</span></span>

<span data-ttu-id="5bad8-206">提供和[資料保護](xref:security/data-protection/introduction)的 NuGet 套件範例 `localStorage` `sessionStorage` 是 [`Microsoft.AspNetCore.ProtectedBrowserStorage`](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage) 。</span><span class="sxs-lookup"><span data-stu-id="5bad8-206">An example of a NuGet package that provides [Data Protection](xref:security/data-protection/introduction) for `localStorage` and `sessionStorage` is [`Microsoft.AspNetCore.ProtectedBrowserStorage`](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span></span>

> [!WARNING]
> <span data-ttu-id="5bad8-207">`Microsoft.AspNetCore.ProtectedBrowserStorage`是不支援的實驗性封裝，目前不適用於生產環境使用。</span><span class="sxs-lookup"><span data-stu-id="5bad8-207">`Microsoft.AspNetCore.ProtectedBrowserStorage` is an unsupported experimental package unsuitable for production use at this time.</span></span>

### <a name="installation"></a><span data-ttu-id="5bad8-208">安裝</span><span class="sxs-lookup"><span data-stu-id="5bad8-208">Installation</span></span>

<span data-ttu-id="5bad8-209">若要安裝 `Microsoft.AspNetCore.ProtectedBrowserStorage` 套件：</span><span class="sxs-lookup"><span data-stu-id="5bad8-209">To install the `Microsoft.AspNetCore.ProtectedBrowserStorage` package:</span></span>

1. <span data-ttu-id="5bad8-210">在 Blazor 伺服器應用程式專案中，將套件參考新增至 [`Microsoft.AspNetCore.ProtectedBrowserStorage`](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage) 。</span><span class="sxs-lookup"><span data-stu-id="5bad8-210">In the Blazor Server app project, add a package reference to [`Microsoft.AspNetCore.ProtectedBrowserStorage`](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span></span>
1. <span data-ttu-id="5bad8-211">在最上層 HTML 中（例如，在 `Pages/_Host.cshtml` [預設專案] 範本的檔案中），加入下列 `<script>` 標記：</span><span class="sxs-lookup"><span data-stu-id="5bad8-211">In the top-level HTML (for example, in the `Pages/_Host.cshtml` file in the default project template), add the following `<script>` tag:</span></span>

   ```html
   <script src="_content/Microsoft.AspNetCore.ProtectedBrowserStorage/protectedBrowserStorage.js"></script>
   ```

1. <span data-ttu-id="5bad8-212">在 `Startup.ConfigureServices` 方法中，呼叫 `AddProtectedBrowserStorage` 以將 `localStorage` 和 `sessionStorage` 服務新增至服務集合：</span><span class="sxs-lookup"><span data-stu-id="5bad8-212">In the `Startup.ConfigureServices` method, call `AddProtectedBrowserStorage` to add `localStorage` and `sessionStorage` services to the service collection:</span></span>

   ```csharp
   services.AddProtectedBrowserStorage();
   ```

### <a name="save-and-load-data-within-a-component"></a><span data-ttu-id="5bad8-213">儲存和載入元件中的資料</span><span class="sxs-lookup"><span data-stu-id="5bad8-213">Save and load data within a component</span></span>

<span data-ttu-id="5bad8-214">在需要將資料載入或儲存至瀏覽器儲存體的任何元件中，使用 [`@inject`](xref:mvc/views/razor#inject) 來插入下列任一項的實例：</span><span class="sxs-lookup"><span data-stu-id="5bad8-214">In any component that requires loading or saving data to browser storage, use [`@inject`](xref:mvc/views/razor#inject) to inject an instance of either of the following:</span></span>

* `ProtectedLocalStorage`
* `ProtectedSessionStorage`

<span data-ttu-id="5bad8-215">選擇取決於您想要使用的備份存放區。</span><span class="sxs-lookup"><span data-stu-id="5bad8-215">The choice depends on which backing store you wish to use.</span></span> <span data-ttu-id="5bad8-216">在下列範例中， `sessionStorage` 會使用：</span><span class="sxs-lookup"><span data-stu-id="5bad8-216">In the following example, `sessionStorage` is used:</span></span>

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore
```

<span data-ttu-id="5bad8-217">`@using`語句可以放在檔案中， `_Imports.razor` 而不是放置在元件中。</span><span class="sxs-lookup"><span data-stu-id="5bad8-217">The `@using` statement can be placed into an `_Imports.razor` file instead of in the component.</span></span> <span data-ttu-id="5bad8-218">使用檔案 `_Imports.razor` 會讓應用程式或整個應用程式的較大區段可以使用命名空間。</span><span class="sxs-lookup"><span data-stu-id="5bad8-218">Use of the `_Imports.razor` file makes the namespace available to larger segments of the app or the whole app.</span></span>

<span data-ttu-id="5bad8-219">若要將 `currentCount` 值保存在 `Counter` 專案範本的元件中，請修改 `IncrementCount` 要使用的方法 `ProtectedSessionStore.SetAsync` ：</span><span class="sxs-lookup"><span data-stu-id="5bad8-219">To persist the `currentCount` value in the `Counter` component of the project template, modify the `IncrementCount` method to use `ProtectedSessionStore.SetAsync`:</span></span>

```csharp
private async Task IncrementCount()
{
    currentCount++;
    await ProtectedSessionStore.SetAsync("count", currentCount);
}
```

<span data-ttu-id="5bad8-220">在更大、更實際的應用程式中，個別欄位的儲存是不太可能發生的情況。</span><span class="sxs-lookup"><span data-stu-id="5bad8-220">In larger, more realistic apps, storage of individual fields is an unlikely scenario.</span></span> <span data-ttu-id="5bad8-221">應用程式較可能儲存包含複雜狀態的整個模型物件。</span><span class="sxs-lookup"><span data-stu-id="5bad8-221">Apps are more likely to store entire model objects that include complex state.</span></span> <span data-ttu-id="5bad8-222">`ProtectedSessionStore`自動序列化和還原序列化 JSON 資料。</span><span class="sxs-lookup"><span data-stu-id="5bad8-222">`ProtectedSessionStore` automatically serializes and deserializes JSON data.</span></span>

<span data-ttu-id="5bad8-223">在上述程式碼範例中， `currentCount` 資料會 `sessionStorage['count']` 在使用者的瀏覽器中儲存為。</span><span class="sxs-lookup"><span data-stu-id="5bad8-223">In the preceding code example, the `currentCount` data is stored as `sessionStorage['count']` in the user's browser.</span></span> <span data-ttu-id="5bad8-224">資料不會以純文字儲存，而是使用 ASP.NET Core 的[資料保護](xref:security/data-protection/introduction)來保護。</span><span class="sxs-lookup"><span data-stu-id="5bad8-224">The data isn't stored in plaintext but rather is protected using ASP.NET Core's [Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="5bad8-225">如果 `sessionStorage['count']` 是在瀏覽器的開發人員主控台中評估，則可以看到加密的資料。</span><span class="sxs-lookup"><span data-stu-id="5bad8-225">The encrypted data can be seen if `sessionStorage['count']` is evaluated in the browser's developer console.</span></span>

<span data-ttu-id="5bad8-226">若要 `currentCount` 在使用者稍後回到元件時復原資料 `Counter` （包括它們是否在全新的線路上），請使用 `ProtectedSessionStore.GetAsync` ：</span><span class="sxs-lookup"><span data-stu-id="5bad8-226">To recover the `currentCount` data if the user returns to the `Counter` component later (including if they're on an entirely new circuit), use `ProtectedSessionStore.GetAsync`:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    currentCount = await ProtectedSessionStore.GetAsync<int>("count");
}
```

<span data-ttu-id="5bad8-227">如果元件的參數包含導覽狀態，請呼叫 `ProtectedSessionStore.GetAsync` 並在中指派結果 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync%2A> ，而不是 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> 。</span><span class="sxs-lookup"><span data-stu-id="5bad8-227">If the component's parameters include navigation state, call `ProtectedSessionStore.GetAsync` and assign the result in <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync%2A>, not <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A>.</span></span> <span data-ttu-id="5bad8-228"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A>只有在第一次具現化元件時，才會呼叫一次。</span><span class="sxs-lookup"><span data-stu-id="5bad8-228"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> is only called one time when the component is first instantiated.</span></span> <span data-ttu-id="5bad8-229"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A>如果使用者流覽至不同的 URL，但仍在相同頁面上，則不會再呼叫一次。</span><span class="sxs-lookup"><span data-stu-id="5bad8-229"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> isn't called again later if the user navigates to a different URL while remaining on the same page.</span></span> <span data-ttu-id="5bad8-230">如需詳細資訊，請參閱 <xref:blazor/components/lifecycle>。</span><span class="sxs-lookup"><span data-stu-id="5bad8-230">For more information, see <xref:blazor/components/lifecycle>.</span></span>

> [!WARNING]
> <span data-ttu-id="5bad8-231">此章節中的範例僅適用于伺服器未啟用預先安裝的情況。</span><span class="sxs-lookup"><span data-stu-id="5bad8-231">The examples in this section only work if the server doesn't have prerendering enabled.</span></span> <span data-ttu-id="5bad8-232">啟用預入功能時，會產生類似下列的錯誤：</span><span class="sxs-lookup"><span data-stu-id="5bad8-232">With prerendering enabled, an error is generated similar to:</span></span>
>
> > <span data-ttu-id="5bad8-233">目前無法發出 JavaScript interop 呼叫。</span><span class="sxs-lookup"><span data-stu-id="5bad8-233">JavaScript interop calls cannot be issued at this time.</span></span> <span data-ttu-id="5bad8-234">這是因為正在資源清單元件。</span><span class="sxs-lookup"><span data-stu-id="5bad8-234">This is because the component is being prerendered.</span></span>
>
> <span data-ttu-id="5bad8-235">請停用已選擇的，或加入額外的程式碼以使用可處理的。</span><span class="sxs-lookup"><span data-stu-id="5bad8-235">Either disable prerendering or add additional code to work with prerendering.</span></span> <span data-ttu-id="5bad8-236">若要深入瞭解如何撰寫可搭配已預呈現運作的程式碼，請參閱[處理預呈現](#handle-prerendering)一節。</span><span class="sxs-lookup"><span data-stu-id="5bad8-236">To learn more about writing code that works with prerendering, see the [Handle prerendering](#handle-prerendering) section.</span></span>

### <a name="handle-the-loading-state"></a><span data-ttu-id="5bad8-237">處理載入狀態</span><span class="sxs-lookup"><span data-stu-id="5bad8-237">Handle the loading state</span></span>

<span data-ttu-id="5bad8-238">由於瀏覽器儲存體是非同步（透過網路連線存取），因此在載入資料並可供元件使用之前，一律會有一段時間。</span><span class="sxs-lookup"><span data-stu-id="5bad8-238">Since browser storage is asynchronous (accessed over a network connection), there's always a period of time before the data is loaded and available for use by a component.</span></span> <span data-ttu-id="5bad8-239">若要獲得最佳結果，請在載入進行時轉譯載入狀態訊息，而不是顯示空白或預設的資料。</span><span class="sxs-lookup"><span data-stu-id="5bad8-239">For the best results, render a loading-state message while loading is in progress instead of displaying blank or default data.</span></span>

<span data-ttu-id="5bad8-240">其中一種方法是追蹤資料是否為 `null` （仍在載入中）。</span><span class="sxs-lookup"><span data-stu-id="5bad8-240">One approach is to track whether the data is `null` (still loading) or not.</span></span> <span data-ttu-id="5bad8-241">在預設 `Counter` 元件中，計數會保留在中 `int` 。</span><span class="sxs-lookup"><span data-stu-id="5bad8-241">In the default `Counter` component, the count is held in an `int`.</span></span> <span data-ttu-id="5bad8-242">將 `currentCount` 問號（ `?` ）新增至類型（），使其成為可為 null `int` ：</span><span class="sxs-lookup"><span data-stu-id="5bad8-242">Make `currentCount` nullable by adding a question mark (`?`) to the type (`int`):</span></span>

```csharp
private int? currentCount;
```

<span data-ttu-id="5bad8-243">不是無條件顯示計數和 **`Increment`** 按鈕，而是只在載入資料時，選擇顯示這些元素：</span><span class="sxs-lookup"><span data-stu-id="5bad8-243">Instead of unconditionally displaying the count and **`Increment`** button, choose to display these elements only if the data is loaded:</span></span>

```razor
@if (currentCount.HasValue)
{
    <p>Current count: <strong>@currentCount</strong></p>

    <button @onclick="IncrementCount">Increment</button>
}
else
{
    <p>Loading...</p>
}
```

### <a name="handle-prerendering"></a><span data-ttu-id="5bad8-244">處理已預呈現</span><span class="sxs-lookup"><span data-stu-id="5bad8-244">Handle prerendering</span></span>

<span data-ttu-id="5bad8-245">在預做期間：</span><span class="sxs-lookup"><span data-stu-id="5bad8-245">During prerendering:</span></span>

* <span data-ttu-id="5bad8-246">與使用者的瀏覽器之間的互動連接不存在。</span><span class="sxs-lookup"><span data-stu-id="5bad8-246">An interactive connection to the user's browser doesn't exist.</span></span>
* <span data-ttu-id="5bad8-247">瀏覽器還沒有可執行 JavaScript 程式碼的頁面。</span><span class="sxs-lookup"><span data-stu-id="5bad8-247">The browser doesn't yet have a page in which it can run JavaScript code.</span></span>

<span data-ttu-id="5bad8-248">`localStorage`或 `sessionStorage` 在預做期間無法使用。</span><span class="sxs-lookup"><span data-stu-id="5bad8-248">`localStorage` or `sessionStorage` aren't available during prerendering.</span></span> <span data-ttu-id="5bad8-249">如果元件嘗試與存放裝置互動，則會產生類似下列的錯誤：</span><span class="sxs-lookup"><span data-stu-id="5bad8-249">If the component attempts to interact with storage, an error is generated similar to:</span></span>

> <span data-ttu-id="5bad8-250">目前無法發出 JavaScript interop 呼叫。</span><span class="sxs-lookup"><span data-stu-id="5bad8-250">JavaScript interop calls cannot be issued at this time.</span></span> <span data-ttu-id="5bad8-251">這是因為正在資源清單元件。</span><span class="sxs-lookup"><span data-stu-id="5bad8-251">This is because the component is being prerendered.</span></span>

<span data-ttu-id="5bad8-252">解決錯誤的其中一種方法是停用已處理的。</span><span class="sxs-lookup"><span data-stu-id="5bad8-252">One way to resolve the error is to disable prerendering.</span></span> <span data-ttu-id="5bad8-253">如果應用程式大量使用以瀏覽器為基礎的存放裝置，這通常是最佳的選擇。</span><span class="sxs-lookup"><span data-stu-id="5bad8-253">This is usually the best choice if the app makes heavy use of browser-based storage.</span></span> <span data-ttu-id="5bad8-254">因為應用程式無法提供任何有用的內容，直到 `localStorage` 或可供使用，因此會增加複雜性並不會讓應用程式受益 `sessionStorage` 。</span><span class="sxs-lookup"><span data-stu-id="5bad8-254">Prerendering adds complexity and doesn't benefit the app because the app can't prerender any useful content until `localStorage` or `sessionStorage` are available.</span></span>

<span data-ttu-id="5bad8-255">若要停用預呈現，請開啟檔案， `Pages/_Host.cshtml` 然後將 `render-mode` [元件標記](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)協助程式的變更為 <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> 。</span><span class="sxs-lookup"><span data-stu-id="5bad8-255">To disable prerendering, open the `Pages/_Host.cshtml` file and change the `render-mode` of the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper) to <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server>.</span></span>

<span data-ttu-id="5bad8-256">如果是其他未使用或的頁面，則會進行預呈現 `localStorage` `sessionStorage` 。</span><span class="sxs-lookup"><span data-stu-id="5bad8-256">Prerendering might be useful for other pages that don't use `localStorage` or `sessionStorage`.</span></span> <span data-ttu-id="5bad8-257">若要保持已啟用的已啟用狀態，請延遲載入作業，直到瀏覽器連線到線路為止。</span><span class="sxs-lookup"><span data-stu-id="5bad8-257">To keep prerendering enabled, defer the loading operation until the browser is connected to the circuit.</span></span> <span data-ttu-id="5bad8-258">以下是儲存計數器值的範例：</span><span class="sxs-lookup"><span data-stu-id="5bad8-258">The following is an example for storing a counter value:</span></span>

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedLocalStorage ProtectedLocalStore

... rendering code goes here ...

@code {
    private int? currentCount;
    private bool isConnected = false;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // When execution reaches this point, the first *interactive* render
            // is complete. The component has an active connection to the browser.
            isConnected = true;
            await LoadStateAsync();
            StateHasChanged();
        }
    }

    private async Task LoadStateAsync()
    {
        currentCount = await ProtectedLocalStore.GetAsync<int>("prerenderedCount");
    }

    private async Task IncrementCount()
    {
        currentCount++;
        await ProtectedSessionStore.SetAsync("count", currentCount);
    }
}
```

### <a name="factor-out-the-state-preservation-to-a-common-location"></a><span data-ttu-id="5bad8-259">將狀態保留分解為一般位置</span><span class="sxs-lookup"><span data-stu-id="5bad8-259">Factor out the state preservation to a common location</span></span>

<span data-ttu-id="5bad8-260">如果許多元件都依賴以瀏覽器為基礎的儲存體，則重新執行狀態提供者程式碼很多次會建立程式碼重複。</span><span class="sxs-lookup"><span data-stu-id="5bad8-260">If many components rely on browser-based storage, re-implementing state provider code many times creates code duplication.</span></span> <span data-ttu-id="5bad8-261">避免程式碼重複的其中一個選項是建立一個可封裝狀態提供者邏輯的*狀態供應器父元件*。</span><span class="sxs-lookup"><span data-stu-id="5bad8-261">One option for avoiding code duplication is to create a *state provider parent component* that encapsulates the state provider logic.</span></span> <span data-ttu-id="5bad8-262">子元件可以使用持續性資料，而不考慮狀態持續性機制。</span><span class="sxs-lookup"><span data-stu-id="5bad8-262">Child components can work with persisted data without regard to the state persistence mechanism.</span></span>

<span data-ttu-id="5bad8-263">在下列元件範例中 `CounterStateProvider` ，會保存計數器資料：</span><span class="sxs-lookup"><span data-stu-id="5bad8-263">In the following example of a `CounterStateProvider` component, counter data is persisted:</span></span>

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore

@if (hasLoaded)
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
    private bool hasLoaded;

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    public int CurrentCount { get; set; }

    protected override async Task OnInitializedAsync()
    {
        CurrentCount = await ProtectedSessionStore.GetAsync<int>("count");
        hasLoaded = true;
    }

    public async Task SaveChangesAsync()
    {
        await ProtectedSessionStore.SetAsync("count", CurrentCount);
    }
}
```

<span data-ttu-id="5bad8-264">`CounterStateProvider`元件在載入完成之前，不會呈現其子內容來處理載入階段。</span><span class="sxs-lookup"><span data-stu-id="5bad8-264">The `CounterStateProvider` component handles the loading phase by not rendering its child content until loading is complete.</span></span>

<span data-ttu-id="5bad8-265">若要使用 `CounterStateProvider` 元件，請將元件的實例包裝在需要存取計數器狀態的任何其他元件周圍。</span><span class="sxs-lookup"><span data-stu-id="5bad8-265">To use the `CounterStateProvider` component, wrap an instance of the component around any other component that requires access to the counter state.</span></span> <span data-ttu-id="5bad8-266">若要讓應用程式中的所有元件都能存取該狀態，請將 `CounterStateProvider` 元件包裝 <xref:Microsoft.AspNetCore.Components.Routing.Router> 在元件中的周圍 `App` （ `App.razor` ）：</span><span class="sxs-lookup"><span data-stu-id="5bad8-266">To make the state accessible to all components in an app, wrap the `CounterStateProvider` component around the <xref:Microsoft.AspNetCore.Components.Routing.Router> in the `App` component (`App.razor`):</span></span>

```razor
<CounterStateProvider>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CounterStateProvider>
```

<span data-ttu-id="5bad8-267">包裝的元件會接收並可修改保存的計數器狀態。</span><span class="sxs-lookup"><span data-stu-id="5bad8-267">Wrapped components receive and can modify the persisted counter state.</span></span> <span data-ttu-id="5bad8-268">下列 `Counter` 元件會執行模式：</span><span class="sxs-lookup"><span data-stu-id="5bad8-268">The following `Counter` component implements the pattern:</span></span>

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

<span data-ttu-id="5bad8-269">先前的元件不需要與互動 `ProtectedBrowserStorage` ，也不會處理「載入」階段。</span><span class="sxs-lookup"><span data-stu-id="5bad8-269">The preceding component isn't required to interact with `ProtectedBrowserStorage`, nor does it deal with a "loading" phase.</span></span>

<span data-ttu-id="5bad8-270">如先前所述，若要處理已進行的已 `CounterStateProvider` 修訂，可以修改，讓取用計數器資料的所有元件都能自動以可處理方式使用。</span><span class="sxs-lookup"><span data-stu-id="5bad8-270">To deal with prerendering as described earlier, `CounterStateProvider` can be amended so that all of the components that consume the counter data automatically work with prerendering.</span></span> <span data-ttu-id="5bad8-271">如需詳細資訊，請參閱處理預進行[處理](#handle-prerendering)一節。</span><span class="sxs-lookup"><span data-stu-id="5bad8-271">See the [Handle prerendering](#handle-prerendering) section for details.</span></span>

<span data-ttu-id="5bad8-272">一般情況下，建議使用*狀態供應器父元件*模式：</span><span class="sxs-lookup"><span data-stu-id="5bad8-272">In general, *state provider parent component* pattern is recommended:</span></span>

* <span data-ttu-id="5bad8-273">使用許多其他元件中的狀態。</span><span class="sxs-lookup"><span data-stu-id="5bad8-273">To consume state in many other components.</span></span>
* <span data-ttu-id="5bad8-274">如果只有一個最上層狀態物件要保存，則為。</span><span class="sxs-lookup"><span data-stu-id="5bad8-274">If there's just one top-level state object to persist.</span></span>

<span data-ttu-id="5bad8-275">若要保存許多不同的狀態物件，並在不同位置取用不同的物件子集，最好避免在全域處理狀態的載入和儲存。</span><span class="sxs-lookup"><span data-stu-id="5bad8-275">To persist many different state objects and consume different subsets of objects in different places, it's better to avoid handling the loading and saving of state globally.</span></span>
