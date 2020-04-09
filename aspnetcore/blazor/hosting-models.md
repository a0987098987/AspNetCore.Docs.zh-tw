---
title: ASP.NET核心Blazor託管模型
author: guardrex
description: 瞭解BlazorBlazorWeb 組裝和伺服器託管模型。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: 0dfc991f76acb227ce9ea27a07fbae50571f0117
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80471824"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a>ASP.NET核心Blazor託管模型

由[丹尼爾·羅斯](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor是一個 Web 架構,旨在在瀏覽器中執行用戶端在基於[Webassembly](https://webassembly.org/)的 .NET 執行時*Blazor(WebAssembly)* 或伺服器端 ASP.NET 核心 (*Blazor伺服器*) 中。 無論託管模型如何,應用程式和元件模型*都是相同的*。

要為本文中描述的託管模型建立專案,請參閱<xref:blazor/get-started>。

有關進階設定,請參<xref:blazor/hosting-model-configuration>閱 。

## <a name="opno-locblazor-webassembly"></a>Blazor網路組裝

的主要Blazor託管模型是在 WebAssembly 上的瀏覽器中運行用戶端。 應用Blazor、其依賴項和 .NET 運行時將下載到瀏覽器。 應用程式會直接在瀏覽器 UI 執行緒上執行。 UI 更新和事件處理在同一進程中進行。 應用的資產作為靜態檔部署到能夠向用戶端提供靜態內容的 Web 伺服器或服務。

![BlazorWeb 組裝Blazor: 應用在瀏覽器內的 UI 線程上運行。](hosting-models/_static/blazor-webassembly.png)

要使用用戶端Blazor託管模型創建應用,請使用**BlazorWebAssembly 應用程式**樣本[(dotnet 新 blazorwasm](/dotnet/core/tools/dotnet-new))。

選擇**BlazorWebAssembly 應用**樣本後,您可以選擇透過選擇**ASP.NET 核心託管**複選框[(dotnet new blazorwasm -- 託管](/dotnet/core/tools/dotnet-new))來配置應用以使用ASP.NET核心後端介面。 ASP.NET核心應用將Blazor應用服務到用戶端。 WebAssemblyBlazor應用可以使用 Web API[SignalR](xref:signalr/introduction)呼<xref:tutorials/signalr-blazor-webassembly>叫或 () 透過網路與伺服器進行互動。

樣本包括處理以下情況`blazor.webassembly.js`的腳本:

* 下載 .NET 運行時、應用和應用的依賴項。
* 初始化運行應用的運行時。

Blazor WebAssembly 託管模型具有以下幾個優點:

* 沒有 .NET 伺服器端依賴項。 應用程式在下載到用戶端後已完全正常運行。
* 充分利用客戶端資源和功能。
* 工作從伺服器卸載到用戶端。
* ASP.NET 酷網路伺服器不需要託管應用。 可以使用無伺服器部署方案(例如,從CDN為應用提供服務)。

Blazor Web 組裝託管有缺點:

* 該應用程式僅限於瀏覽器的功能。
* 需要有能力的用戶端硬體和軟體(例如,Web 組裝支援)。
* 下載大小較大,並且載入應用需要更長的時間。
* .NET 運行時和工具支援不太成熟。 例如[,.NET 標準](/dotnet/standard/net-standard)支持和調試中存在限制。

管理系統Blazor應用程式模型支援[Docker 容器](/dotnet/standard/microservices-architecture/container-docker-introduction/index)。 右鍵按一下可視化工作室中的伺服器項目,然後選擇 **「添加** > **Docker 支援**」。。

## <a name="opno-locblazor-server"></a>Blazor伺服器

使用Blazor伺服器託管模型,應用將在伺服器上從ASP.NET核心應用內執行。 UI 更新、事件處理和 JAVAScript[SignalR](xref:signalr/introduction)呼叫透過連接處理。

![瀏覽器透過SignalR連接與伺服器上的應用(託管在ASP.NET核心應用內)進行交互。](hosting-models/_static/blazor-server.png)

Blazor要使用Blazor伺服器託管模型創建應用,請使用ASP.NET核心**Blazor伺服器應用程式**範本[(dotnet 新的 blazorserver)。](/dotnet/core/tools/dotnet-new) ASP.NET核心應用託管Blazor伺服器應用並SignalR創建用戶端連接的終結點。

ASP.NET核心應用引用應用的`Startup`類別以新增:

* 伺服器端服務。
* 到請求處理管道的應用。

文本`blazor.server.js`建立用戶端連接。 應用有責任根據需要保持和恢復應用狀態(例如,在網路連接丟失時)。 文本`blazor.server.js`從ASP.NET核心共用框架中的嵌入式資源提供。

伺服器Blazor託管模型具有以下幾個優點:

* 下載大小明顯小於BlazorWebAssembly 應用,並且該應用載入速度更快。
* 該應用程式充分利用了伺服器功能,包括使用任何.NET Core 相容 API。
* 伺服器上的 .NET Core 用於運行應用,因此現有的 .NET 工具(如調試)按預期工作。
* 支援精簡用戶端。 例如,Blazor伺服器應用適用於不支援 WebAssembly 和資源受限的設備上的瀏覽器。
* 應用程式的 .NET/C++ 代碼庫(包括應用的元件代碼)不提供給用戶端。

Blazor伺服器託管有缺點:

* 通常存在更高的延遲。 每個使用者交互都涉及網路躍點。
* 沒有離線支援。 如果用戶端連接失敗,應用將停止工作。
* 對於許多用戶的應用來說,可擴充性具有挑戰性。 伺服器必須管理多個用戶端連接並處理用戶端狀態。
* 需要ASP.NET核心伺服器才能為應用提供服務。 無法使用無伺服器部署方案(例如,從CDN為應用提供服務)。

伺服器Blazor應用模型支援[Docker 容器](/dotnet/standard/microservices-architecture/container-docker-introduction/index)。 右鍵單擊可視化工作室中的項目,然後選擇 **「添加** > **Docker 支援**」。。

### <a name="comparison-to-server-rendered-ui"></a>與伺服器呈現的 UI 進行比較

瞭解Blazor伺服器應用的一種方法是瞭解它與使用 Razor 檢視或 Razor 頁面在 ASP.NET 核心應用中呈現 UI 的傳統模型有何不同。 這兩種模型都使用 Razor 語言來描述 HTML 內容,但它們在呈現標記的方式上存在顯著差異。

呈現 Razor 頁面或檢視時,每行 Razor 代碼都會以文本形式發出 HTML。 呈現後,伺服器將釋放頁面或視圖實例,包括生成的任何狀態。 當發生另一個頁面請求時,例如伺服器驗證失敗並顯示驗證摘要時:

* 整個頁面將再次重新呈現為 HTML 文本。
* 該頁將發送到用戶端。

應用Blazor由稱為*元件*的 UI 的可重用元素組成。 元件包含 C# 代碼、標記和其他元件。 顯示元件時,Blazor產生類似於 HTML 或 XML 文件物件模型 (DOM) 的包含元件的圖形。 此圖形包括屬性和欄位中持有的元件狀態。 Blazor計算元件圖以生成標記的二進位表示形式。 二進位格式可以是:

* 轉換為 HTML 文本(&dagger;在預渲染期間)。
* 用於在常規渲染期間有效地更新標記。

&dagger;*預成*&ndash;一個使用者的 Razor 元件在伺服器上編譯為靜態 HTML 並發送到用戶端,並呈現給使用者。 在用戶端和伺服器之間建立連接后,元件的靜態預渲染元素將被互動式元素替換。 預渲染使應用對用戶的反應更快。

的BlazorUI 更新由:

* 使用者互動,例如選擇按鈕。
* 應用觸發器,如計時器。

重新渲染圖形,並計算 UI*差異*(差異)。 此差異是更新用戶端上的 UI 所需的最小 DOM 編輯集。 差異以二進位格式發送到用戶端,並由瀏覽器應用。

使用者在用戶端上導航后釋放元件。 當使用者與元件交互時,元件的狀態(服務、資源)必須保持在伺服器的記憶體中。 由於伺服器可能同時維護許多元件的狀態,因此記憶體耗盡是必須解決的問題。 有關如何創作Blazor伺服器應用以確保最佳使用伺服器記憶體的指導,請參閱<xref:security/blazor/server>。

### <a name="circuits"></a>電路

伺服器應用程式構建在[ASP.NETSignalR核心](xref:signalr/introduction)之上Blazor。 每個客戶端透過一個或多個稱為電SignalR路 連線與伺服器*通訊*。 電路是Blazor可以容忍臨時網路中SignalR斷 的連接的抽象。 當Blazor用戶端看到SignalR連接斷開連接時,它會嘗試使用SignalR新 連接重新連接到伺服器。

連接到Blazor伺服器應用的每個瀏覽器螢幕(瀏覽器選項卡或 iframe)SignalR都使用 連接。 與典型的伺服器呈現應用相比,這是另一個重要區別。 在伺服器呈現的應用中,在多個瀏覽器螢幕中打開同一應用通常不會轉化為伺服器上的其他資源需求。 在BlazorServer 應用中,每個瀏覽器螢幕都需要由伺服器管理的單獨的電路和元件狀態的單獨實例。

Blazor考慮關閉瀏覽器選項卡或以*正常*終止形式導航到外部 URL。 如果正常終止,電路和相關資源將立即釋放。 用戶端也可能非正常斷開連接,例如由於網路中斷。 Blazor伺服器在可配置的間隔記憶體儲存斷開連接的電路,以允許用戶端重新連接。

Blazor伺服器允許代碼定義*電路處理程式*,它允許在更改使用者電路的狀態時運行代碼。 如需詳細資訊，請參閱 <xref:blazor/advanced-scenarios#blazor-server-circuit-handler>。

### <a name="ui-latency"></a>UI 延遲

UI 延遲是從啟動的操作到更新 UI 所需的時間。 對於應用來說,UI 延遲的較小值對於用戶進行回應是勢在必行的。 在BlazorServer 應用中,每個操作都發送到伺服器,處理併發送回 UI 差異。 因此,UI 延遲是處理操作時網路延遲和伺服器延遲的總和。

對於僅限於私有公司網路的業務線應用,通常無法察覺到網路延遲對使用者感知延遲的影響。 對於通過 Internet 部署的應用,延遲可能會對使用者造成明顯注意,尤其是在使用者在地理位置上分佈廣泛時。

記憶體使用還會導致應用延遲。 記憶體使用量增加會導致頻繁的垃圾回收或將記憶體分到磁碟,這兩者都降低了應用性能,從而增加了 UI 延遲。 如需詳細資訊，請參閱 <xref:security/blazor/server>。

Blazor應優化伺服器應用,通過減少網路延遲和記憶體使用量來最大程度地減少 UI 延遲。 有關測量網路延遲的方法,請參閱<xref:host-and-deploy/blazor/server#measure-network-latency>。 有關BlazorSignalR和 的詳細資訊,請參閱:

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a>連線到伺服器

Blazor伺服器應用需要與伺服器SignalR的有效連接。 如果連接丟失,應用將嘗試重新連接到伺服器。 只要客戶端的狀態仍在記憶體中,用戶端會話將恢復而不丟失狀態。

伺服器Blazor應用預先呈現以回應第一個用戶端請求,該請求在伺服器上設置 UI 狀態。 當用戶端嘗試建立連接時,SignalR客戶端必須重新連接到同一伺服器。 Blazor使用多後端伺服器的伺服器套用應該SignalR為 連線來*黏性作業階段*。

我們建議將[AzureSignalR服務](/azure/azure-signalr)用於Blazor伺服器應用。 該服務允許將Blazor伺服器應用擴展到大量併SignalR發 連接。 透過此選項SignalR`ServerStickyMode`或設定值設定為,為 Azure 服務啟用黏滯工作階段`Required`。 如需詳細資訊，請參閱 <xref:host-and-deploy/blazor/server#signalr-configuration>。

使用 IIS 時,將使用應用程式請求路由啟用粘滯會話。 有關詳細資訊,請參閱[使用應用程式請求路由](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)的 HTTP 負載平衡。

## <a name="additional-resources"></a>其他資源

* <xref:blazor/get-started>
* <xref:signalr/introduction>
* <xref:blazor/hosting-model-configuration>
* <xref:tutorials/signalr-blazor-webassembly>
