---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: "設計存留失敗 （使用 Azure 建置實際的雲端應用程式） |Microsoft 文件"
author: MikeWasson
description: "Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: a0ee790da07c99cdb1279a6bca637a4ce8076e84
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>設計存留失敗 （使用 Azure 建置實際的雲端應用程式）
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 E 書籍的相關資訊，請參閱[第一章](introduction.md)。


其中一項操作，您必須考慮當您建置任何類型的應用程式，但特別是其中一個會在有許多人將使用它，在雲端中執行的是如何設計應用程式，讓它可以正常處理失敗並繼續傳遞值最多可達可能的。 有足夠時間，項目將在任何環境或任何軟體系統中發生錯誤。 您的應用程式如何處理這些情況下會決定您的客戶會收到如何感到生氣，多少時間花費分析和修正問題。

## <a name="types-of-failures"></a>失敗類型

有兩種基本的類別，您會想要以不同方式處理的失敗：

- 暫時性，自我修復失敗，例如間歇性的網路連線問題。
- 持續的失敗需要介入。

針對暫時失敗，您可以實作重試原則以確保，大部分的應用程式會快速且自動復原的時間。 您的客戶可能會注意到稍微更長的回應時間，但否則它們不會受到影響。 我們將示範一些方法來處理這些錯誤在[暫時性錯誤處理章](transient-fault-handling.md)。

根進行分析，您可以實作監視和記錄功能，會發生問題，並進行根原因分析時通知您。 我們將示範一些可協助您保持在這類錯誤，在頂端[監視和遙測章節](monitoring-and-telemetry.md)。

## <a name="failure-scope"></a>失敗的範圍

您也需要思考如何失敗範圍-是否會影響一部電腦，或完整的服務，例如 SQL Database 或存放裝置或整個區域。

![失敗的範圍](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>電腦的失敗

在 Azure 中，失敗的伺服器會自動取代為由一個新和快速地自動從這類的失敗復原設計良好的雲端應用程式。 我們稍早強調無狀態 web 層，延展性的優勢，並輕鬆復原，從失敗的伺服器是無狀態的另一個優點。 輕鬆復原也是其中一個平台做為服務 (PaaS) 功能，例如 SQL Database 和 Azure App Service Web 應用程式的優點。 硬體故障很少見，但這些服務處理它們，而發生時您甚至不需要撰寫程式碼以處理電腦失敗，當您使用其中一個服務。

### <a name="service-failures"></a>服務失敗

雲端應用程式通常會使用多個服務。 比方說，它修正應用程式中使用 SQL Database 服務、 儲存體服務，並部署至 Azure App Service web 應用程式。 您的應用程式將要如果其中一個服務依存於失敗？ 某些服務失敗的易記 「 很抱歉，稍後再試一次 」 訊息可能是最好的方式。 但在許多案例中您可以做得更好。 比方說，當您的後端資料存放區已關閉，您可以接受使用者輸入、 顯示 「 您已收到要求，"並儲存其他某處輸入暫時;當您需要的服務再次運作為止，然後您可以擷取輸入，並處理它。

[佇列為主的工作模式](queue-centric-work-pattern.md)一章會示範一個方式來處理此案例。 修正它應用程式會將工作儲存在 SQL Database 中，但不需要結束處理 SQL Database 已關閉時。 在該章節，我們看到如何儲存在佇列中，工作的使用者輸入，並使用的工作者處理序讀取佇列並更新工作。 如果 SQL 已關閉，讓您建立修正它的工作不受影響;工作者處理序可以等候，並使用 SQL Database 時處理新的工作。

### <a name="region-failures"></a>區域失敗

整個區域可能會失敗。 天然災害可能會破壞資料中心、 簡維它可能會取得 meteor、 主幹行，資料中心，無法由埋藏與 backhoe 等 cow farmer 剪下。如果您的應用程式應的資料中心裝載該怎麼辦？ 很可能您的應用程式在 Azure 中設定多個區域中同時執行，以便如果其中一個沒有損毀，您就會繼續執行另一個區域中。 這類失敗的情況很罕見的相符項目，和大部分的應用程式不透過確保不會中斷的服務，透過這種失敗 hoops 跳。 請參閱 [資源] 區段，如需有關如何保持應用程式就算是經由區域失敗資訊章節的結尾。

Azure 的目標是要讓所有的失敗中來得簡單許多，這類的處理，您會看到我們如何所做的下列章節中的一些範例。

## <a name="slas"></a>Sla

人員通常會反應在雲端環境中的服務等級協定 (Sla)。 基本上，這些是關於如何可靠的服務是可讓公司承諾。 99.9 %sla 表示您應該預期服務以正確地運作 99.9%的時間。 這是相當一般的 SLA 值和聽起來很高的數字，但您可能不知道有多少停機時間。 %1 實際數量來。 以下是示範各種 SLA 百分比金額以透過一年、 月和週多少停機時間的資料表。

![SLA 資料表](design-to-survive-failures/_static/image2.png)

因此 99.9 %sla 表示您的服務可能已關閉 8.76 小時年份或月份 43.2 分鐘。 這是更停機時間比想像大部分的人。 因此，身為開發人員要注意一段時間，則可能處理的非失誤性的方式。 在某個時間點人正在使用您的應用程式和服務將會關閉，而且您想要該客戶的負面影響降到最低。

您應該了解 SLA 的一件事是哪一個時段則是指： 無法取得重設時鐘每週、 每個月或每年嗎？ 在 Azure 中我們重設時鐘每個月，這是比每年的 SLA，好讓您，因為每年的 SLA 無法移轉它們使用一系列的很好的月份來隱藏不好的月份。

當然我們一律渴望做得更好比 SLA;通常您會向下，多小於。 承諾時，如果我們曾向下的時間超過關閉時間的最大值可以要求退款。 您會回到可能的總金額不會完全彌補貴用戶停機時間，過多的商務影響的但 SLA 的這個部分做為強制執行原則，並可讓您知道我們需要它很嚴重。

### <a name="composite-slas"></a>複合 Sla

思考時您所看到的 Sla 很重要的事是，應用程式中使用多個服務，與每個服務需要不同的 SLA 的影響。 例如，它修正應用程式使用 Azure App Service Web 應用程式、 Azure 儲存體和 SQL Database。 以下是在 2013 年 12 月，正在寫入此的電子書的日期為準，其 SLA 號碼：

![SLA 的網站，儲存體、 SQL 資料庫](design-to-survive-failures/_static/image3.png)

停機時間如預期般，這些服務的 Sla 為基礎的應用程式的最大值是什麼？ 您可能會認為您向下的時間會是最差的 SLA 百分比或 99.9%相等在此情況下。 如果這三種服務永遠無法在相同的時間，但這不一定實際發生的狀況，也就是，則為 true。 每個服務可能會失敗獨立在不同的時間，所以您不必個別 SLA 數字乘以計算複合 SLA。

![複合 SLA](design-to-survive-failures/_static/image4.png)

因此您的應用程式可能已關機不只是 43.2 分鐘月份，3 次數量，一個月 – 108 分鐘，仍然可以在 Azure SLA 的限制範圍內。

這個問題並非 Azure 獨有的。 我們實際上會提供最佳的雲端可用，任何雲端服務的 Sla，而且也有類似的問題，如果您使用任何廠商雲端服務處理。 這醒目提示是思考如何設計您的應用程式依正常程序，處理無法避免服務失敗，因為它們可能影響客戶或使用者經常發生的重要性。

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>相較於企業的停機時間體驗雲端 Sla

人員有時說 「 我的企業應用程式在我永遠不會有這些問題。 」 如果您要求一個月的時間量向下他們實際上有，他們通常說"，它會偶爾。 」 如果頻率要求，請他們承認和"有時候我們需要備份，或安裝新的伺服器或更新軟體 」。 當然，計算在停機時間。 大部分企業應用程式，除非它們是時間的特別重要的實際上是時間的向下超過我們服務的 Sla 所允許量。 但當它是您的伺服器和基礎結構，您是負責它，並且在它的控制項時通常較少 angst 覺得，停機時間。 在雲端環境中您是依存於其他人，且您不知道如何運作，因此您可能會變得更擔心。

當企業會達成更大的執行時間百分比比您取得 SLA 的雲端時，執行所花費更多錢硬體上。 雲端服務無法這樣做，但可能會收取更多的服務。 相反地，您會利用符合成本效益的服務和設計您的軟體，因此無法避免的失敗會導致給您的客戶的最小中斷。 您為雲端應用程式的設計工具的工作不這麼避免失敗，避免發生災難，而且您執行此動作將焦點放在硬體上找不到的軟體。 企業應用程式，盡量以最大化失敗之間的平均時間，而雲端應用程式會盡可能減少復原時間。

### <a name="not-all-cloud-services-have-slas"></a>並非所有的雲端服務有 Sla

也請注意，並非每個雲端服務有 SLA。 如果您的應用程式會依據執行時間保證的服務，您可能已關閉比您想像的最長。 例如，如果您啟用登入您的網站使用如 Facebook 或 Twitter 社交提供者時，檢查來找出，是否沒有 SLA，而且您可能會發現有不是其中一個服務提供者。 但是，如果驗證服務當機或無法支援您在其擲回的要求數量，將您的客戶鎖定您的應用程式。 您可能已關閉天或更久。 一個新的應用程式的建立者預期數百萬個下載和具有相依性 Facebook 驗證 –，但未向 Facebook 移至即時、 發現太晚之前時發生沒有該服務的 SLA。

### <a name="not-all-downtime-counts-toward-slas"></a>並非所有的停機時間計算 Sla

如果您的應用程式過度使用，某些雲端服務可能會刻意拒絕服務。 這稱為*節流*。 如果服務的 SLA，它應該清楚您可能會進行節流處理，以及條件您應用程式的設計應該避免這種情況，並適當地回應的節流設定，如果發生。 比方說，如果服務要求啟動失敗超過每秒的特定數字時，您要確定不太快，它們會導致繼續節流會發生自動重試。 我們將會有更多關於節流中[暫時性錯誤處理章](transient-fault-handling.md)。

## <a name="summary"></a>總結

本章嘗試協助您了解為什麼真實世界雲端應用程式具有設計繼續正常運作失敗。 從開始[下一章](monitoring-and-telemetry.md)，在這一系列的其餘模式移入更多詳細的一些策略，您可以使用執行此作業：

- 具有良好[監控與遙測](monitoring-and-telemetry.md)，以便您快速找出有關失敗需要使用者介入，並有足夠的資訊加以解決。
- [處理暫時性失敗](transient-fault-handling.md)藉由實作智慧型重試邏輯，使您的應用程式會自動復原時它，並會回復為[斷路器](transient-fault-handling.md#circuitbreakers)時它無法邏輯。
- 使用[分散式快取](distributed-caching.md)以便與資料庫存取權的輸送量、 延遲和連線問題降至最低。
- 鬆散耦合，透過實作[佇列為主的工作模式](queue-centric-work-pattern.md)，如此一來，您的應用程式前端可以繼續工作關閉後端時。

## <a name="resources"></a>資源

如需詳細資訊，請參閱此的電子書和下列資源中的後續章節。

文件集：

- [Failsafe： 具有恢復功能雲端架構指引](https://msdn.microsoft.com/en-us/library/windowsazure/jj853352.aspx)。 Marc Mercuri、 Ulrich Homann 和 Andrew Townhill 詘躩裛。 FailSafe 影片系列網頁版本。
- [Azure 雲端服務中大規模服務設計的最佳作法](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx)。 Mark Simms 和 Michael Thomassy 詘躩裛。
- [Azure 業務續航力技術指引](https://msdn.microsoft.com/en-us/library/windowsazure/hh873027.aspx)。 Patrick Wickline 和 Jason Roth 詘躩裛。
- [災害復原與高可用性 Azure 應用程式](https://msdn.microsoft.com/en-us/library/windowsazure/dn251004.aspx)。 Michael McKeown、 Kommalapati 和 Jason Roth 詘躩裛。
- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/en-us/library/dn568099.aspx)。 請參閱多資料中心部署指導方針，斷路器模式。
- [Azure 支援的服務等級協定](https://azure.microsoft.com/support/legal/sla/)。
- [Azure SQL Database 的業務續航力](https://msdn.microsoft.com/en-us/library/windowsazure/hh852669.aspx)。 SQL 資料庫高可用性和災害復原功能的相關文件。
- [高可用性和災害復原 Azure 虛擬機器中的 SQL Server](https://msdn.microsoft.com/en-us/library/windowsazure/jj870962.aspx)。

影片：

- [FailSafe： 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 九部分 Ulrich Homann、 Marc Mercuri 和 Mark Simms 的系列。 高層級概念與架構原則非常可存取且有趣的方式，呈現劇本取自與實際客戶的 Microsoft 客戶諮詢團隊 (CAT) 體驗。 1 到 8 個劇集深度進入設計雲端應用程式不受失敗的原因。 另請參閱節流時段 2 開始 49:57、 的失敗點和失敗模式中的片段 2 開始 56:05，討論和斷路器中的時段 3 開始 40:55 討論中的待處理的討論。
- [建置大型： 學到來自 Azure 客戶-II](https://channel9.msdn.com/Events/Build/2012/3-030)。 Mark Simms 交談有關失敗的設計和檢測的所有項目。 類似於保全數列但進入詳細的使用說明。

>[!div class="step-by-step"]
[上一頁](unstructured-blob-storage.md)
[下一頁](monitoring-and-telemetry.md)
