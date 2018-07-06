---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: 設計存留失敗 （使用 Azure 建置真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 29a430a223fb62d9530ea00a60d458a6dbc5199a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820651"
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>設計存留失敗 （使用 Azure 建置真實世界的雲端應用程式）
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。 它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。 電子書的相關資訊，請參閱[第 1 章](introduction.md)。


您需要思考如何當您建置任何應用程式，但特別是在其中有許多人會使用到它，雲端中執行的其中一種的事情之一是如何設計應用程式，讓它可以正常處理失敗，並繼續提供價值高達可能的。 有足夠的時間，項目會在任何環境或任何軟體系統中發生錯誤。 您的應用程式如何處理這些情況下決定您的客戶會收到如何不滿，以及多少時間花費分析及修正問題。

## <a name="types-of-failures"></a>失敗類型

有兩種基本的類別，您會想要以不同方式處理的失敗：

- 暫時性、 自我修復失敗，例如間歇性的網路連線問題。
- 根需要介入。

針對暫時性失敗，您可以實作重試原則，以確保大部分的應用程式在快速且自動復原的時間。 您的客戶可能會注意到略長回應時間，但否則它們不會受到影響。 我們將示範如何處理這些錯誤[暫時性錯誤處理的一章](transient-fault-handling.md)。

針對持續發生失敗，您可以實作監視和記錄功能，當發生問題，並進行根本原因分析時立即通知您。 我們將示範可協助您掌握這類錯誤，在某些方面[監視和遙測章節](monitoring-and-telemetry.md)。

## <a name="failure-scope"></a>失敗的範圍

您也必須考慮失敗範圍 – 是否會影響單一機器，或完整的服務，例如 SQL Database、 儲存體或整個區域。

![失敗的範圍](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>機器失敗

在 Azure 中，新的連線，都會自動取代故障的伺服器，並從這種失敗會自動並快速復原的設計良好的雲端應用程式。 我們稍早強調的無狀態 web 層的延展性優勢，並輕鬆復原，從失敗的伺服器是無狀態的另一個優點。 輕鬆復原也是其中一個平台為-即服務 (PaaS) 功能，例如 SQL Database 和 Azure App Service Web Apps 的優點。 硬體失敗較為罕見，但發生這些服務，而處理它們您甚至不必撰寫程式碼來處理機器失敗，當您使用上述其中一個服務。

### <a name="service-failures"></a>服務失敗

雲端應用程式通常會使用多個服務。 比方說，它修正應用程式使用 SQL Database 服務，儲存體服務和 web 應用程式部署至 Azure App Service。 將您的應用程式做什麼如果其中一個您依賴的服務失敗？ 某些服務失敗的易記 「 很抱歉，稍後再試 」 訊息可能是的最好的方式。 但在許多情況下您可以執行更好。 比方說，當您的後端資料存放區已關閉，您可以接受使用者輸入、 顯示 「 已收到您的要求 」，並儲存其他某處輸入暫時;然後服務時，您需要 operational 同樣地，您可以擷取輸入，並加以處理。

[佇列為中心的工作模式](queue-centric-work-pattern.md)章節示範一種方式處理這種情況。 修正其應用程式會將工作儲存在 SQL Database 中，但不一定要結束使用 SQL Database 已關閉時。 在該章節中，我們將了解如何在佇列中，儲存工作的使用者輸入，並使用背景工作處理序讀取佇列，並更新的工作。 如果 SQL 已關閉，建立 It 工作的能力不受影響;背景工作處理序可以等候，並使用 SQL Database 時，處理新的工作。

### <a name="region-failures"></a>區域失敗

整個區域可能會失敗。 天然災害可能會損毀的資料中心，它可能會取得壓平合併的 meteor，埋藏 backhoe 等等 cow farmer 無法剪下主幹行的資料中心。如果應的資料中心內裝載您的應用程式該怎麼辦？ 可以設定您的應用程式，在 Azure 中多個區域中同時執行，以便如果在一個沒有嚴重損壞，您就會繼續在另一個區域中執行。 這類失敗極為罕見的事件，而且大部分的應用程式不會跳過必須確保不中斷的服務，透過這類失敗關卡。 請參閱 [資源] 區段，如需有關如何讓您的應用程式甚至透過區域失敗資訊的章節的結尾。

Azure 的目標是要處理所有這類問題來得簡單許多，，您會看到我們的表現如何，在以下章節中的一些範例。

## <a name="slas"></a>Sla

人們通常會聽聽雲端環境中的服務等級協定 (Sla)。 基本上，這些是如何可靠服務的相關公司做出的承諾。 99.9 %sla 意謂著您應該會正常運作時間達 99.9%的服務。 這是相當常見的 sla 值和聽起來很高的數字，但您可能不知道多少停機時間。 實際總計為的 1%。 以下是資料表，其中顯示各種 SLA 百分比相當透過一年、 月和週多少停機時間。

![SLA 資料表](design-to-survive-failures/_static/image2.png)

因此 99.9 %sla 表示您的服務可能已關閉 8.76 個小時一年或每月 43.2 分鐘。 這是更停機時間比大多數人。 因此，身為開發人員要注意一段停機時間，很有可能以正常方式處理它。 在某個時間點有人會想要使用您的應用程式和服務即將停止運作，而且您想將客戶的負面影響降至最低。

您應該了解 SLA 的一點是它參考到哪些時間範圍： 沒有時鐘會重設每週、 每個月或每年嗎？ 在 Azure 中我們重設時鐘每個月，這是您比每年的 SLA，更好的因為每年的 SLA 可以隱藏不好的月份的表徵它們使用一系列的很好的幾個月。

當然我們永遠渴望比 SLA; 更好嗎通常，您會關閉，遠不如。 承諾是如果我們曾向下的時間停機時間的最大值超過您可以要求退款。 您會取得可能的金額不全額補償您的停機時間，過多的商務影響，但 SLA 的這個部分做為 強制執行原則，並可讓您知道，我們執行非常認真看待。

### <a name="composite-slas"></a>複合 Sla

考慮您要尋找在 Sla 的重點是在應用程式中使用多個服務，每個服務具有不同的 SLA 的影響。 比方說，它修正應用程式所使用的 Azure App Service Web Apps、 Azure 儲存體和 SQL Database。 以下是本電子書正在寫入在 2013 年 12 月，日其 SLA 號碼：

![SLA 的網站，儲存體、 SQL Database](design-to-survive-failures/_static/image3.png)

停機時間會預期這些服務的 Sla 為基礎的應用程式的最大值是什麼？ 您可能會認為，您的停機時間會等於最差的 SLA 百分比或有 99.9%在此情況下。 如果這三種服務永遠無法在此同時，但這並不一定是實際發生什麼事，那就是，則為 true。 在不同的時間，因此您需要個別的 SLA 個數字相乘的複合 SLA 來計算每個服務可能獨立失敗。

![複合 SLA](design-to-survive-failures/_static/image4.png)

讓您的應用程式可能會關閉不只是 43.2 分鐘的時間的月份，3 次數量，一個月 – 108 分鐘，並仍是 Azure SLA 的限制範圍內。

這個問題並非 Azure 獨有的。 實際上，我們提供最佳的雲端，任何雲端服務的 Sla，而且也有類似的問題，如果您使用任何廠商的雲端服務處理。 這所強調是思考如何設計您的應用程式，以依正常程序，處理無可避免的服務失敗，因為它們可能經常會影響客戶或使用者的重要性。

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>相較於企業的停機時間體驗雲端 Sla

人們有時說 「 我的企業應用程式在我永遠不會有這些問題。 」 如果您問多少停機時間，一個月他們實際上有，他們通常會說 「，它會偶爾。 」 如果您要求頻率，請他們承認和 「 有時候我們需要備份或安裝新的伺服器或更新軟體 」。 當然，計算在停機時間。 大部分的企業應用程式除非特別任務關鍵性實際上是時間的向下多個服務 Sla 所允許量。 但當它是您的伺服器和基礎結構，您必須負責它，然後在它的控制項，通常覺得較少的秘密，停機時間。 在雲端環境中您是相依於其他人，而且您不知道發生什麼情況，因此您可能會變得更擔心。

當企業達成更高的執行時間百分比比從雲端 SLA 時，請勿在硬體上花費更多的錢。 雲端服務可以進行，但會有費用為其服務的更多。 相反地，您會利用符合成本效益的服務，並設計您的軟體，讓無可避免的失敗會造成最小的中斷，給您的客戶。 您為雲端應用程式的設計工具的作業不是這麼避免失敗，避免發生災難，您的做法是將焦點放在不是在硬體上的軟體。 平均失敗時間發揮最大努力企業應用程式，而雲端應用程式會努力復原的平均時間降至最低。

### <a name="not-all-cloud-services-have-slas"></a>並非所有的雲端服務會有 Sla

也請注意，並非每個雲端服務甚至會有 SLA。 如果您的應用程式相依於具有無法正常運作時間保證的服務，您可能會關閉比您想像的還要長。 比方說，如果您啟用登入您使用 Facebook 或 Twitter 等社交提供者的網站時，檢查以了解，是否有 SLA，而且您可能會發現有不是其中一個服務提供者。 但是，如果驗證服務停止運作或無法支援您在它擲回的要求數量，您的客戶會鎖定您的應用程式。 天或更久，可能是向下。 一個新的應用程式的建立者必須處理數億個下載項目和 Facebook 驗證 – 不採取相依性但未向 Facebook 再即時與探索到太晚發生該服務不提供任何 SLA。

### <a name="not-all-downtime-counts-toward-slas"></a>並非所有的停機時間項目計 Sla

如果您的應用程式過度使用，某些雲端服務可能會刻意拒絕服務。 這就叫做*節流*。 如果服務有 SLA，它應該狀態下您可能會進行節流處理，以及您應用程式的設計應該避免這種情況，並做出適當回應節流，如果發生的條件。 比方說，如果服務要求會開始失敗，當您超過每秒特定數目時，您要確定不要太快，它們會導致節流來繼續發生自動重試。 我們必須進一步中的節流[暫時性錯誤處理的一章](transient-fault-handling.md)。

## <a name="summary"></a>總結

這一章已嘗試協助您了解為什麼真實世界的雲端應用程式必須設計成依正常程序存留失敗。 開頭[下一步 一章](monitoring-and-telemetry.md)，其餘的模式，在這一系列移到一些策略，若要這樣做，您可以使用更多詳細資料：

- 有好[監視和遙測](monitoring-and-telemetry.md)，以便您快速找出有關失敗需要介入，並有足夠的資訊來解決這些問題。
- [處理暫時性失敗](transient-fault-handling.md)藉由實作智慧性重試邏輯，使您的應用程式會自動復原時可以並回到[斷路器](transient-fault-handling.md#circuitbreakers)邏輯時它無法。
- 使用[分散式快取](distributed-caching.md)以便與資料庫存取權的輸送量、 延遲和連線問題降至最低。
- 鬆散耦合透過實作[佇列為中心的工作模式](queue-centric-work-pattern.md)，如此一來，您的應用程式後端可以繼續使用後端已關閉時。

## <a name="resources"></a>資源

如需詳細資訊，請參閱本電子書和下列資源中的後續章節。

文件：

- [Failsafe︰ 恢復雲端架構指引](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx)。 Marc Mercuri、 Ulrich Homann 和 Andrew Townhill 詘躩裛。 FailSafe 影片系列的網頁版本。
- [Azure 雲端服務上大規模服務設計的最佳作法](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)。 Mark Simms 和 Michael Thomassy 詘躩裛。
- [Azure 業務續航力技術指引](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx)。 Patrick Wickline 和 Jason Roth 詘躩裛。
- [災害復原和高可用性 Azure 應用程式的](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx)。 Michael McKeown、 Hanu Kommalapati 和 Jason Roth 詘躩裛。
- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/library/dn568099.aspx)。 多資料中心部署指導方針，斷路器模式，請參閱。
- [Azure 支援-服務等級協定](https://azure.microsoft.com/support/legal/sla/)。
- [Azure SQL Database 的業務續航力](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx)。 SQL Database 高可用性和災害復原功能相關的文件。
- [高可用性和災害復原 Azure 虛擬機器中的 SQL Server](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx)。

影片：

- [FailSafe︰ 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 Ulrich Homann、 Marc Mercuri 和 Mark Simms、 包含九部組件系列。 提供高階的概念和架構原則非常可存取且有趣的方式，取自 Microsoft 客戶諮詢團隊 (CAT) 體驗，與實際客戶的劇本。 1 到 8 影片深入了解進入設計雲端應用程式不受失敗的原因。 另請參閱後續追蹤的第 2 集開始 49:57 的失敗點和失敗模式，在第 2 集開始 56:05，討論，在第 3 集開始 40:55 斷路器的討論中的節流討論。
- [建置大型： 從 Azure 的客戶-第 ii 部分 Lessons learned](https://channel9.msdn.com/Events/Build/2012/3-030)。 Mark Simms 將討論如何針對失敗設計及檢測所有項目。 類似於保全數列但作法的詳細說明。

> [!div class="step-by-step"]
> [上一頁](unstructured-blob-storage.md)
> [下一頁](monitoring-and-telemetry.md)
