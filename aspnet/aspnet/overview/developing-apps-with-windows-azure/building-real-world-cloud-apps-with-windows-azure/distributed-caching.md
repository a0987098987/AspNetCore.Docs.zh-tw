---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: 分散式快取 （使用 Azure 建置真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 26866ef9d13a198aab627ccf0f5e1ff3d2304427
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577537"
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>分散式快取 （建置真實世界的雲端應用程式與 Azure）
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson]((https://twitter.com/RickAndMSFT))， [Tom Dykstra](https://github.com/tdykstra)

[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。 它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。 電子書的相關資訊，請參閱[第 1 章](introduction.md)。


上一章探討了暫時性錯誤處理和快取的斷路器策略所述。 這一章提供更多的背景，有關快取，包括使用它時，使用它，常見模式的時機，以及如何在 Azure 中加以實作。

## <a name="what-is-distributed-caching"></a>什麼被分散式快取

快取提供高輸送量、 低延遲存取經常存取的應用程式資料，將資料儲存在記憶體中。 雲端應用程式最有用的一種快取是分散式快取，這表示資料不會儲存在個別的 web 伺服器的記憶體，但在其他雲端資源，並快取的資料可供所有的應用程式的 web 伺服器 （或其他雲端 Vm，are 應用程式使用)。

![此圖顯示存取相同的快取伺服器的多部網頁伺服器](distributed-caching/_static/image1.png)

當應用程式調整所新增或移除伺服器，或因為升級或將錯誤取代伺服器，快取的資料會保持可存取執行應用程式的每部伺服器。

藉由避免高延遲資料存取的永續性資料存放區，快取可大幅改善應用程式回應速度。 例如，從快取中擷取的資料是遠快於從關聯式資料庫中擷取。

快取的一項好處減少持續性資料存放區，而資料輸出時，可能導致成本較低的資料傳輸費用的永續性資料存放區。

## <a name="when-to-use-distributed-caching"></a>使用分散式快取的時機

快取的運作最適合應用程式工作負載，執行多個讀取與寫入的資料，以及當資料模型支援索引鍵/值組織您用來儲存和擷取快取中的資料。 它時，也更有用的應用程式使用者共用許多常見的資料;例如，快取會不會提供許多好處，如果每一位使用者通常會擷取使用者的唯一資料。 快取，可能很有幫助範例則是一個產品類別目錄中，因為資料不會經常變更，以及所有客戶都查看相同的資料。

快取的優點變得越來越重要應用程式相應的越多，隨著永續性資料存放區的潛在延遲與輸送量限制變得更整體的應用程式的效能限制。 不過，您可以實作快取的效能也比其他原因。 對於不一定要是完美地保持最新狀態時，向使用者顯示的資料，快取存取可做為斷路器的永續性資料存放區時沒有回應或無法使用。

## <a name="popular-cache-population-strategies"></a>熱門的快取擴展策略

若要能從快取中擷取資料，您需要將資料儲存那里第一次。 有數種策略，您需要的資料放入快取：

- 隨選 / 快取另行

    應用程式會嘗試從快取中擷取資料和快取沒有資料 ("miss")，當應用程式將資料儲存在快取，讓它可在下一次。 下一次應用程式嘗試取得相同的資料，找不到什麼它正在尋找快取 （"出擊"） 中。 若要避免在資料庫上擷取快取的資料已變更，您會確認快取，對資料存放區進行變更時。
- 背景的資料推播

    背景服務將資料推送至快取上定期的排程和應用程式一律提取從快取。 此方法效果很好，不一定需要您的高延遲資料來源傳回的最新的資料。
- 斷路器

    應用程式通常會直接與持續性資料存放區通訊，但應用程式時的永續性資料存放區發生可用性問題時，會從快取擷取資料。 資料可能已放入快取使用的快取保留多少 」 或 「 背景的資料推播策略。 這是錯誤處理策略，而不是效能增強的策略。

為了讓快取中的資料保持目前，您可以在您的應用程式會建立、 更新，或刪除資料時刪除相關的快取項目。 如果很好的應用程式有時候會收到已稍微過期的資料，您可以依賴來限制資料可以是舊的快取上設定到期時間。

您可以設定絕對到期 （建立快取項目之後的時間長度） 或滑動期限 （上次存取快取項目之後的時間長度）。 您需要用來防止資料變得太過時的快取到期機制時，會使用絕對期限。 修正其應用程式中，我們以手動方式將會收回過時的快取項目，以及我們將使用滑動期限，將最新的資料保留在快取。 不論您選擇的到期原則，快取會自動收回最舊的 （最少最近使用過或 LRU） 項目時達到快取的記憶體限制。

## <a name="sample-cache-aside-code-for-fix-it-app"></a>修正它的應用程式的範例另行快取程式碼

在下列範例程式碼中，我們檢查快取先擷取 Fix It 工作時。 如果工作在快取中找不到，我們會傳回它;如果找不到，我們從資料庫中取得，並將它儲存在快取。 要加入至快取會做的變更`FindTaskByIdAsync`方法會反白顯示。

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

當您更新或刪除它修正工作時，您必須使其失效 （移除） 的快取工作。 否則，未來會嘗試讀取這項工作會繼續從快取取得舊的資料。

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

這些是範例來說明簡單的快取程式碼;可下載 Fix It 專案中快取尚未實作。

## <a name="azure-caching-services"></a>Azure 快取服務

Azure 提供下列快取服務： [Azure Redis 快取](https://msdn.microsoft.com/library/dn690523.aspx)並[Azure 受控快取](https://msdn.microsoft.com/library/dn386094.aspx)。 Azure Redis 快取為基礎的熱門[開放原始碼 Redis 快取](http://redis.io/)和大部分的第一個選擇快取案例。

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>使用快取提供者的 ASP.NET 工作階段狀態

中所述[web 開發最佳作法章](web-development-best-practices.md)，最佳做法是避免使用工作階段狀態。 如果您的應用程式需要工作階段狀態下, 一步 的最佳作法是避免預設的記憶體提供者，因為不會啟用向外延展 （web 伺服器的多個執行個體）。 ASP.NET SQL Server 工作階段狀態提供者可使用工作階段狀態的多部 web 伺服器執行的站台，但它會產生高延遲成本，相較於記憶體提供者。 如果您必須使用工作階段狀態的最佳解決方案是使用快取提供者，例如[Azure 快取的工作階段狀態供應器](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx)。

## <a name="summary"></a>總結

您已了解如何修正它應用程式可以實作快取以改善回應時間和延展性，並啟用可繼續受到讀取作業的回應，當資料庫目前無法使用應用程式。 在 [[下一步] 一章](queue-centric-work-pattern.md)我們將示範如何進一步改善延展性，並讓應用程式會持續為有效回應的寫入作業。

## <a name="resources"></a>資源

如需有關快取的詳細資訊，請參閱下列資源。

文件

- [Azure 快取](https://msdn.microsoft.com/library/gg278356.aspx)。 在 Azure 中的快取的官方 MSDN 文件。
- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/library/dn568099.aspx)。 請參閱快取指引和另行快取模式。
- [Failsafe︰ 恢復雲端架構指引](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx)。 Marc Mercuri、 Ulrich Homann 和 Andrew Townhill 詘躩裛。 請參閱快取的章節。
- [Azure 雲端服務上大規模服務設計的最佳作法](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)。 W. Mark Simms 和 Michael Thomassy 詘躩裛。 在分散式快取，請參閱節。
- [分散式快取獲得延展性](https://msdn.microsoft.com/magazine/dd942840.aspx)。 較舊的 MSDN Magazine 文章 (2009)，但在一般情況下，分散式的快取的清楚地寫入的簡介進入深入程度高於保全和最佳作法的技術白皮書的快取區段。

視訊

- [FailSafe︰ 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 Ulrich Homann、 Marc Mercuri 和 Mark Simms、 包含九部組件系列。 顯示如何建構雲端應用程式的 400 層級檢視。 這一系列著重於理論上，原因為何;如需作法的詳細資訊，請參閱 Mark Simms 所建置巨量的系列。 在第 3 集開始 1:24:14 快取的討論，請參閱。
- [建置大型： 從第 I 部分 Azure 客戶 Lessons learned](https://channel9.msdn.com/Events/Build/2012/3-029)。Simon Davies 討論分散式快取開始 46:00。 類似於保全數列但作法的詳細說明。 簡報有在 2012 年 10 月 31 日，因此它未涵蓋的 2013年中導入的 Azure App Service 中的 Web 應用程式的快取服務。

程式碼範例

- [雲端服務基本概念，在 Azure 中的](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)。 實作分散式快取的範例應用程式。 請參閱隨附的部落格文章[雲端服務基本概念-快取的基本概念](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx)。

> [!div class="step-by-step"]
> [上一頁](transient-fault-handling.md)
> [下一頁](queue-centric-work-pattern.md)
