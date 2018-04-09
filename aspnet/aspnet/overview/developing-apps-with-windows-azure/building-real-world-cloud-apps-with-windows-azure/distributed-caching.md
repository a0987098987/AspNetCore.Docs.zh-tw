---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: 分散式快取 （使用 Azure 建置實際的雲端應用程式） |Microsoft 文件
author: MikeWasson
description: Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2015
ms.topic: article
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 3600200f9bb705ccf66c859547668bdf8e89d97a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>分散式快取 （建置真實世界雲端應用程式與 Azure）
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 E 書籍的相關資訊，請參閱[第一章](introduction.md)。


前一章探討了暫時性錯誤處理，而提及快取做為斷路器策略。 本章提供更多背景有關快取，包括何時使用它時，常見的模式使用，以及如何實作在 Azure 中。

## <a name="what-is-distributed-caching"></a>什麼被分散式快取

快取提供高輸送量、 低延遲存取常存取的應用程式資料，藉由將資料儲存在記憶體中。 雲端應用程式快取的最有用的型別是分散式快取，這表示資料不會儲存在個別 web 伺服器的記憶體，但在其他雲端資源，而且快取的資料可供所有的應用程式的 web 伺服器 （或其他雲端 Vm 的 are 應用程式使用)。

![顯示存取相同的快取伺服器的多部網頁伺服器的圖表](distributed-caching/_static/image1.png)

當新增或移除伺服器，來調整應用程式或伺服器會被取代，因為升級或錯誤時，快取的資料會保持可存取執行應用程式的每部伺服器。

藉由避免高延遲資料存取的永續性資料存放區，快取可大幅改善應用程式的回應。 例如，從快取中擷取資料的速度比從關聯式資料庫中擷取的。

減少快取的好處是永續性資料存放區，當沒有資料輸出時，可能會導致較低成本的流量費用的永續性資料存放區。

## <a name="when-to-use-distributed-caching"></a>使用分散式快取的時機

快取的工作最適合應用程式的工作負載，執行多個讀取與寫入資料，以及當資料模型支援索引鍵/值組織您用來儲存和擷取快取中的資料。 因此，也更適用於應用程式的使用者共用許多一般資料;比方說，如果每個使用者通常會擷取資料至該使用者的唯一快取會不提供許多優點。 快取位置可能很有幫助範例則是產品目錄，因為資料不會經常變更，以及所有客戶會都查看相同的資料。

快取的效益成為逐漸可測量應用程式比例的越多，輸送量限制和延遲時間延遲的持續性資料存放區會變成多個整體應用程式效能的限制。 不過，您可能會實作快取的效能，以及其他原因。 不一定要完全時向使用者顯示最新的資料，快取存取可做為斷路器的永續性資料存放區時沒有回應或無法使用。

## <a name="popular-cache-population-strategies"></a>受歡迎的快取擴展策略

為了要能夠從快取中擷取資料，您必須將它儲存那里第一次。 有幾種策略，您需要將資料放入快取：

- 視 / 另行快取

    應用程式會嘗試從快取中，擷取資料和快取沒有資料 （「 遺漏 」），應用程式會儲存的資料快取中，因此它會使用下一次。 它所找到的下一次應用程式嘗試取得相同的資料，它會尋找的快取 （「 叫用 」） 中。 若要避免在資料庫上擷取已變更的快取的資料，您會確認快取資料存放區進行變更時。
- 背景資料發送

    背景服務將資料推送至快取上定期的排程和應用程式一律會從快取。 高延遲資料來源不一定需要您使用這種方法效果很好傳回最新的資料。
- 斷路器

    應用程式通常會直接與持續性資料存放區通訊，但應用程式可用性問題的永續性資料存放區時，會從快取擷取資料。 資料可能已放入快取使用的快取保留 」 或 「 背景資料推入策略。 這是錯誤的處理策略，而不是效能增強的策略。

以保持資料快取中，您可以刪除相關的快取項目，您的應用程式建立、 更新或刪除資料。 如果它是造成您的應用程式有時會出現已稍微過期的資料，您可以依賴資料可以是舊的快取上設定的限制可設定的到期時間。

您可以設定絕對期限 （快取項目建立以來的時間量） 或滑動期限 （快取項目上次存取的時間以來的時間量）。 會視要防止資料變得過時的快取到期機制時，會使用絕對期限。 修正它在應用程式中，我們將會手動收回過時的快取項目，我們會將最新的資料保留在快取使用滑動期限。 不論您選擇的到期原則，快取會自動收回最舊的 （至少最近使用過或 LRU） 項目達到快取的記憶體限制時。

## <a name="sample-cache-aside-code-for-fix-it-app"></a>修正它的應用程式的程式碼範例另行快取

下列範例程式碼中，我們會檢查快取先擷取它修正工作時。 如果快取中找到工作，我們決定傳回它。如果找不到，我們從資料庫取得它，並將它儲存在快取中。 要加入至快取會做的變更`FindTaskByIdAsync`方法也會反白顯示。

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

當您更新或刪除修正它的工作時，您必須使 （移除） 的快取的工作。 否則，未來會嘗試讀取該工作會繼續從快取取得舊的資料。

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

這些是範例來說明簡單的快取程式碼;可下載修正它專案中快取尚未實作。

## <a name="azure-caching-services"></a>Azure 快取服務

Azure 提供下列快取服務： [Azure Redis 快取](https://msdn.microsoft.com/library/dn690523.aspx)和[Azure 受管理快取](https://msdn.microsoft.com/library/dn386094.aspx)。 Azure Redis 快取依據熱門[開放式 Redis 快取](http://redis.io/)和大部分的第一個選擇快取案例。

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>使用快取提供者的 ASP.NET 工作階段狀態

中所述[web 開發最佳作法章](web-development-best-practices.md)，最佳作法是避免使用工作階段狀態。 如果您的應用程式需要工作階段狀態下, 一步的最佳作法是避免因為未啟用向外延展 （web 伺服器的多個執行個體） 的預設記憶體中的提供者。 ASP.NET SQL Server 工作階段狀態提供者可讓使用工作階段狀態的多個 web 伺服器執行的站台，但它會產生高延遲成本相較於記憶體中提供者。 如果您必須使用工作階段狀態的最佳解決方案是使用快取提供者，例如[Azure 快取的工作階段狀態提供者](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx)。

## <a name="summary"></a>總結

您已經看到如何修正它應用程式可以實作快取以改善回應時間和延展性，並讓應用程式仍會繼續可供讀取作業的回應資料庫時，無法使用。 在[下一章](queue-centric-work-pattern.md)我們將示範如何進一步改善延展性，並使應用程式會繼續回應的寫入作業。

## <a name="resources"></a>資源

如需有關快取的詳細資訊，請參閱下列資源。

文件

- [Azure 快取](https://msdn.microsoft.com/library/gg278356.aspx)。 在 Azure 中的快取的官方 MSDN 文件。
- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/library/dn568099.aspx)。 請參閱快取指引和另行快取模式。
- [Failsafe： 具有恢復功能雲端架構指引](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx)。 Marc Mercuri、 Ulrich Homann 和 Andrew Townhill 詘躩裛。 在快取，請參閱 > 一節。
- [Azure 雲端服務中大規模服務設計的最佳作法](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)。 W. Mark Simms 和 Michael Thomassy 詘躩裛。 在分散式快取，請參閱 > 一節。
- [分散式快取延展性的路徑上](https://msdn.microsoft.com/magazine/dd942840.aspx)。 較舊的 (2009) MSDN Magazine 文件，但一般而言; 分散式的快取的清楚地寫入的簡介進入深入比保全和最佳作法的技術白皮書的快取區段。

視訊

- [FailSafe： 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 九部分 Ulrich Homann、 Marc Mercuri 和 Mark Simms 的系列。 提供如何設計雲端應用程式架構的 400 層級檢視。 這一系列著重在理論上，而原因為何。如需作法的詳細資訊，請參閱 Mark Simms 建置大數列。 請參閱在時段 3 開始 1:24:14 快取的討論。
- [建置大型： 學到來自 Azure 客戶的第 I 部分](https://channel9.msdn.com/Events/Build/2012/3-029)。Simon Davies 討論分散式快取開始 46:00。 類似於保全數列但進入詳細的使用說明。 簡報指定 2012 年 10 月 31 日，因此它未涵蓋的 2013年中所導入的 Azure App Service 中的 Web 應用程式的快取服務。

程式碼範例

- [雲端服務在 Azure 中的基本概念](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)。 會實作分散式快取的範例應用程式。 請參閱隨附的部落格文章[快取的基本概念 – 雲端服務基本概念](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx)。

> [!div class="step-by-step"]
> [上一頁](transient-fault-handling.md)
> [下一頁](queue-centric-work-pattern.md)
