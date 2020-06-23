---
title: ASP.NET Core Blazor 裝載模型
author: guardrex
description: 瞭解 Blazor WebAssembly 和 Blazor 伺服器裝載模型。
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
uid: blazor/hosting-models
ms.openlocfilehash: a54f92d1c951792e599992b82e6b6d5c85549292
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243495"
---
# <a name="aspnet-core-blazor-hosting-models"></a>ASP.NET Core Blazor 裝載模型

依[Daniel Roth](https://github.com/danroth27)

Blazor是一種 web 架構，設計用來在[WebAssembly](https://webassembly.org/)型 .net 執行時間（* Blazor WebAssembly*）或伺服器端的 ASP.NET Core （* Blazor 伺服器*）中，于瀏覽器中執行用戶端。 無論裝載模型為何，應用程式和元件模型*都相同*。

若要為本文所述的主控模型建立專案，請參閱 <xref:blazor/get-started> 。

如需 advanced configuration，請參閱 <xref:blazor/fundamentals/configuration> 。

## <a name="blazor-webassembly"></a>BlazorWebAssembly

的主要裝載模型 Blazor 是在 WebAssembly 的瀏覽器中執行用戶端。 Blazor應用程式、其相依性和 .net 執行時間會下載至瀏覽器。 應用程式會直接在瀏覽器 UI 執行緒上執行。 UI 更新和事件處理會在同一個進程中進行。 應用程式的資產會以靜態檔案的形式部署至 web 伺服器或服務，以提供靜態內容給用戶端。

![BlazorWebAssembly： Blazor 應用程式會在瀏覽器內的 UI 執行緒上執行。](hosting-models/_static/blazor-webassembly.png)

若要 Blazor 使用用戶端裝載模型來建立應用程式，請使用** Blazor WebAssembly 應用程式**範本（ [`dotnet new blazorwasm`](/dotnet/core/tools/dotnet-new) ）。

選取** Blazor WebAssembly 應用程式**範本之後，您可以選擇將應用程式設定為使用 ASP.NET Core 後端，方法是選取 [ **ASP.NET Core**裝載] 核取方塊（ [`dotnet new blazorwasm --hosted`](/dotnet/core/tools/dotnet-new) ）。 ASP.NET Core 應用程式會將 Blazor 應用程式提供給用戶端。 BlazorWebAssembly 應用程式可以使用 Web API 呼叫或（），透過網路與伺服器 [SignalR](xref:signalr/introduction) 互動 <xref:tutorials/signalr-blazor-webassembly> 。

這些範本包含 `blazor.webassembly.js` 處理的腳本：

* 下載 .NET 執行時間、應用程式和應用程式的相依性。
* 初始化執行時間以執行應用程式。

BlazorWebAssembly 裝載模型提供數個優點：

* 沒有 .NET 伺服器端相依性。 應用程式會在下載至用戶端之後完全正常運作。
* 用戶端資源和功能都可以充分運用。
* 工作會從伺服器卸載至用戶端。
* 不需要 ASP.NET Core web 伺服器來裝載應用程式。 無伺服器部署案例可行（例如，從 CDN 提供應用程式）。

WebAssembly 裝載有缺點 Blazor ：

* 應用程式僅限於瀏覽器的功能。
* 需要支援的用戶端硬體和軟體（例如 WebAssembly 支援）。
* 下載大小較大，且應用程式需要較長的時間來載入。
* .NET 執行時間和工具支援較不成熟。 例如， [.NET Standard](/dotnet/standard/net-standard)支援和偵錯工具中有限制。

Blazor託管應用程式模型支援[Docker 容器](/dotnet/standard/microservices-architecture/container-docker-introduction/index)。 以滑鼠右鍵按一下 Visual Studio 中的伺服器專案，然後選取 [**新增**  >  **Docker 支援**]。

## <a name="blazor-server"></a>Blazor伺服器

使用 Blazor 伺服器裝載模型，應用程式會在伺服器上從 ASP.NET Core 應用程式中執行。 UI 更新、事件處理及 JavaScript 呼叫會透過連接來處理 [SignalR](xref:signalr/introduction) 。

![瀏覽器會透過連接，與伺服器上的應用程式互動（裝載于 ASP.NET Core 應用程式內） SignalR 。](hosting-models/_static/blazor-server.png)

若要 Blazor 使用伺服器裝載模型來建立應用程式 Blazor ，請使用 ASP.NET Core ** Blazor 伺服器應用程式**範本（ [`dotnet new blazorserver`](/dotnet/core/tools/dotnet-new) ）。 ASP.NET Core 應用程式會裝載 Blazor 伺服器應用程式，並建立 SignalR 用戶端連接的端點。

ASP.NET Core 應用程式會參考要新增的應用程式 `Startup` 類別：

* 伺服器端服務。
* 應用程式至要求處理管線。

此 `blazor.server.js` 腳本會建立用戶端連接。 應用程式會負責保存和還原所需的應用程式狀態（例如，萬一網路連線中斷時）。 `blazor.server.js`腳本是從 ASP.NET Core 共用架構中的內嵌資源提供。

Blazor伺服器裝載模型提供數個優點：

* 下載大小明顯小於 Blazor WebAssembly 應用程式，且應用程式載入速度會更快。
* 應用程式會充分利用伺服器功能，包括使用任何 .NET Core 相容的 Api。
* 伺服器上的 .NET Core 是用來執行應用程式，因此現有的 .NET 工具（例如，「偵測」）會如預期般運作。
* 支援瘦用戶端。 例如， Blazor 伺服器應用程式會使用不支援 WebAssembly 的瀏覽器，以及在資源限制的裝置上。
* 應用程式的 .NET/c # 程式碼基底（包括應用程式的元件代碼）不會提供給用戶端。

伺服器裝載有缺點 Blazor ：

* 通常會有較高的延遲。 每個使用者互動都牽涉到網路躍點。
* 沒有離線支援。 如果用戶端連線失敗，應用程式就會停止運作。
* 對於具有許多使用者的應用程式而言，擴充性是極具挑戰性的。 伺服器必須管理多個用戶端連接並處理用戶端狀態。
* 需要 ASP.NET Core 伺服器才能服務應用程式。 無伺服器部署案例不可行（例如，從 CDN 提供應用程式）。

Blazor伺服器應用程式模型支援[Docker 容器](/dotnet/standard/microservices-architecture/container-docker-introduction/index)。 以滑鼠右鍵按一下 Visual Studio 中的專案，然後選取 [**新增**  >  **Docker 支援**]。

### <a name="comparison-to-server-rendered-ui"></a>與伺服器呈現的 UI 比較

瞭解伺服器應用程式的其中一種方式 Blazor ，就是了解它與傳統模型之間的差異，以使用 Razor 視圖或頁面在 ASP.NET Core 應用程式中呈現 UI Razor 。 這兩種模型都使用 Razor 語言來描述 HTML 內容，但在標記的呈現方式上有很大的差異。

當 Razor 頁面或視圖呈現時，每一行程式 Razor 代碼都會以文字格式發出 HTML。 呈現之後，伺服器會處置頁面或 view 實例，包括任何已產生的狀態。 當頁面的另一個要求發生時（例如，當伺服器驗證失敗且顯示驗證摘要時）：

* 整個頁面會再次重新顯示為 HTML 文字。
* 此頁面會傳送至用戶端。

Blazor應用程式是由可重複使用的 UI 元素（稱為「*元件*」）所組成。 元件包含 c # 程式碼、標記和其他元件。 當元件呈現時，會 Blazor 產生類似于 HTML 或 XML 檔物件模型（DOM）的內含元件圖形。 此圖表包含屬性和欄位中保留的元件狀態。 Blazor評估元件圖形，以產生標記的二進位標記法。 二進位格式可以是：

* 已轉換成 HTML 文字（在預建期間 &dagger; ）。
* 用來在一般轉譯期間有效率地更新標記。

&dagger;預先*呈現*：要求的 Razor 元件會在伺服器上編譯成靜態 HTML，並傳送至用戶端，並在其中呈現給使用者。 在用戶端與伺服器之間建立連接之後，元件的靜態資源清單元素會取代為互動式元素。 進行預進行，讓應用程式更能回應使用者。

中的 UI 更新 Blazor 會由下列觸發：

* 使用者互動，例如選取按鈕。
* 應用程式觸發程式，例如計時器。

圖形為重新顯示，且會計算 UI*差異*（差異）。 這項差異是更新用戶端上的 UI 所需的最小一組 DOM 編輯。 差異會以二進位格式傳送至用戶端，並由瀏覽器套用。

元件會在使用者從用戶端導覽出去之後處置。 當使用者與元件互動時，元件的狀態（服務、資源）必須保留在伺服器的記憶體中。 因為許多元件的狀態可能會由伺服器同時維護，所以記憶體耗盡是必須解決的問題。 如需有關如何撰寫 Blazor 伺服器應用程式以確保最佳使用伺服器記憶體的指引，請參閱 <xref:blazor/security/server/threat-mitigation> 。

### <a name="circuits"></a>獲得

Blazor伺服器應用程式是以[ASP.NET Core SignalR ](xref:signalr/introduction)為基礎。 每個用戶端會透過一或多個 SignalR 稱為*線路*的連線來與伺服器通訊。 線路是 Blazor SignalR 可容忍暫時網路中斷的連線抽象化。 當 Blazor 用戶端看到連線中斷連線時 SignalR ，它會嘗試使用新的連線重新連線到伺服器 SignalR 。

連接到伺服器應用程式的每個瀏覽器畫面（瀏覽器索引標籤或 iframe）都會 Blazor 使用 SignalR 連接。 相較于一般伺服器呈現的應用程式，這還是另一項重要的差異。 在伺服器呈現的應用程式中，在多個瀏覽器畫面中開啟相同的應用程式，通常不會轉譯成伺服器上的其他資源需求。 在 Blazor 伺服器應用程式中，每個瀏覽器畫面都需要個別的線路，且元件狀態的個別實例會由伺服器管理。

Blazor考慮關閉瀏覽器索引標籤或流覽至外部 URL 的*正常*終止。 在正常終止的事件中，會立即釋放線路和相關聯的資源。 用戶端也可能會因為網路中斷而無法正常地中斷連線。 Blazor伺服器會儲存已中斷連線的線路，以取得可設定的間隔，以允許用戶端重新連線。

Blazor伺服器允許程式碼定義*電路處理常式*，允許對使用者線路狀態的變更執行程式碼。 如需詳細資訊，請參閱 <xref:blazor/advanced-scenarios#blazor-server-circuit-handler>。

### <a name="ui-latency"></a>UI 延遲

UI 延遲是從起始的動作到 UI 更新時間所花費的時間。 較小的 UI 延遲值是應用程式對使用者進行回應的必要行為。 在 Blazor 伺服器應用程式中，每個動作都會傳送至伺服器、進行處理，並傳回 UI 差異。 因此，UI 延遲是網路延遲和處理動作之伺服器延遲的總和。

對於僅限於私人商業網路的企業營運應用程式，通常會 imperceptible 因網路延遲而對使用者的延遲所造成的影響。 對於透過網際網路部署的應用程式，使用者的延遲可能會很明顯，尤其是在地理位置廣泛散佈的使用者時。

記憶體使用量也會導致應用程式延遲。 增加記憶體使用量會導致頻繁的垃圾收集或將記憶體分頁到磁片，這兩者都會降低應用程式效能，因而增加 UI 延遲。

Blazor伺服器應用程式應該經過優化，藉由減少網路延遲和記憶體使用量，將 UI 延遲降到最低。 如需測量網路延遲的方法，請參閱 <xref:blazor/host-and-deploy/server#measure-network-latency> 。 如需和的詳細資訊 SignalR Blazor ，請參閱：

* <xref:blazor/host-and-deploy/server>
* <xref:blazor/security/server/threat-mitigation>

### <a name="connection-to-the-server"></a>連接到伺服器

Blazor伺服器應用程式需要伺服器的使用中 SignalR 連接。 如果連接中斷，應用程式會嘗試重新連線到伺服器。 只要用戶端的狀態仍在記憶體中，用戶端會話就會繼續，而不會失去狀態。

Blazor伺服器應用程式會 prerenders，以回應第一個用戶端要求，這會在伺服器上設定 UI 狀態。 當用戶端嘗試建立連接時 SignalR ，用戶端必須重新連線到相同的伺服器。 Blazor使用一部以上後端伺服器的伺服器應用程式應該執行連線的*粘滯會話* SignalR 。

我們建議使用適用于伺服器應用程式的[Azure SignalR 服務](/azure/azure-signalr) Blazor 。 此服務可讓您將 Blazor 伺服器應用程式相應增加至大量的並行 SignalR 連接。 將 SignalR 服務的選項或設定值設為，即可為 Azure 服務啟用「粘滯會話」 `ServerStickyMode` `Required` 。 如需詳細資訊，請參閱 <xref:blazor/host-and-deploy/server#signalr-configuration>。

使用 IIS 時，會使用應用程式要求路由來啟用「粘滯會話」。 如需詳細資訊，請參閱[使用應用程式要求路由的 HTTP 負載平衡](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)。

## <a name="additional-resources"></a>其他資源

* <xref:blazor/get-started>
* <xref:signalr/introduction>
* <xref:blazor/fundamentals/additional-scenarios>
* <xref:tutorials/signalr-blazor-webassembly>
