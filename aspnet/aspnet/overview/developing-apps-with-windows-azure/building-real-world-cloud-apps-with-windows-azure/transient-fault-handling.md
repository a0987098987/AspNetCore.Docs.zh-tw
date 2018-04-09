---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: 暫時性錯誤處理 （建置真實世界雲端應用程式與 Azure） |Microsoft 文件
author: MikeWasson
description: Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/03/2015
ms.topic: article
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: 86bd67b04931ae2452f6e063e6475a434a0125bc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>暫時性錯誤處理 （建置真實世界雲端應用程式與 Azure）
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 E 書籍的相關資訊，請參閱[第一章](introduction.md)。


當您設計真實世界雲端應用程式時，其中一項操作需要思考如何是如何處理服務暫時中斷。 這個問題是雲端應用程式唯一重要，因為您是依賴網路連線和外部服務。 您可以得到很少會通常自我修復的問題，如果您尚未準備好處理它們以聰明的方式，它們會導致不正確的經驗提供給客戶。

## <a name="causes-of-transient-failures"></a>暫時性失敗的原因

在雲端環境中，您會發現的失敗，並卸除的資料庫的連接會定期發生。 這是部分是因為您將在相較於內部部署環境的多個負載平衡器透過您的 web 伺服器和資料庫伺服器上具有的直接實體連線。 此外，有時當您準備依存於多租用戶服務時，才會看到較慢的服務 get 或逾時呼叫因為其他人會使用服務會經常遇到。 在其他情況下，您可能會頻率太高，會叫用服務的使用者，並在服務刻意節流 – 拒絕連線 – 您以避免造成負面影響其他租用戶的服務。

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>使用智慧的重試/撤退邏輯來減少暫時性失敗的影響

而不是擲回例外狀況，並顯示您的客戶無法使用或錯誤的頁面，您可以辨識通常屬於暫時性的錯誤，並自動重試錯誤，導致此作業希望，之前長時間，您將會成功。 作業會成功在第二次嘗試，大部分的情況下，會從錯誤復原而不需要曾經注意時發生問題的客戶。

有數種方式，您可以實作智慧重試邏輯。

- Microsoft Patterns&amp;作法群組具有[暫時性錯誤處理應用程式區塊](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx)執行的所有項目，如果您使用 ADO.NET 的 SQL 資料庫存取權 （不透過 Entity Framework)。 您剛才設定的原則，重試 – 多少次重試查詢或命令和等待的時間之間嘗試 – 和包裝您的 SQL 程式碼中*使用*區塊。

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    也支援 TFH [Azure 角色中快取](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx)和[Service Bus](https://azure.microsoft.com/services/service-bus/)。
- 當您使用 Entity Framework，您通常不直接使用 SQL 連線，讓您無法使用此模式和實務的封裝，但是 Entity Framework 6 建立這種類型的重試邏輯，請直接將架構。 類似的方式，您指定的重試策略，，然後會 EF 使用這個策略，只要它存取資料庫。

    若要修正它應用程式中使用這項功能，我們只需要已加入的類別，衍生自*DbConfiguration*開啟重試邏輯。

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    對於 SQL Database 架構識別為通常屬於暫時性錯誤的例外狀況，顯示的程式碼會指示 EF 重試作業最多 3 次，使用指數型撤退延遲重試次數和延遲時間上限為 5 秒之間。 指數型退讓表示，每個失敗的重試之後它將會等候較長的一段時間後再重試。 如果資料列中的三次嘗試失敗，則會擲回例外狀況。 下列關於斷路器章節說明為何要指數型撤退及有限的數目的重試次數。

    您可以有類似的問題，當您使用 Azure 儲存體服務，它修正應用程式會為 Blob，以及.NET 儲存體用戶端 API 已經實作相同類型的邏輯。 您只需要指定重試原則，或甚至不需要這樣做，如果您很滿意預設設定。

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>斷路器

有幾個原因，您不想要重試次數太多段太長：

- 持續重試失敗的要求太多的使用者，可能降低其他使用者的體驗。 如果進行重複所有重試要求上百萬的人，可以是佔用 IIS 分派佇列，否則處理成功的要求提供服務時，防止您的應用程式。
- 如果每個人都正在重試因服務失敗，那里可能將所有的要求排入佇列，會服務時，取得湧入開始復原。
- 如果此錯誤是因為節流，而且服務會使用節流的時間的視窗，一直持續不斷地重試無法將該視窗移出，並會造成的節流設定，才能繼續。
- 您可能必須等待 web 網頁來呈現使用者。 製作人等候太長可能會更惱人的該相當快速地建議您，稍後再試。

指數型退讓解決這些問題的某些限制服務可以從您的應用程式取得的重試的頻率。 但您也需要有*斷路器*： 這表示，在特定重試您的應用程式停止重試及接受某些其他動作，例如下列其中一種臨界值：

- 自訂後援。 如果您無法從路透社取得股票價格，也許您可以從取得 Bloomberg;或者，如果您無法從資料庫取得資料，或許您可以取得它從快取。
- 無訊息會失敗。 如果您需要從服務不全應用程式中，只傳回 null 時無法取得資料。 如果您顯示它修正工作，而且 Blob 服務沒有回應，您可以顯示工作詳細資料，而不將映像。
- 快速失敗。 為了避免大量具有服務使用者出錯誤重試要求可能會導致其他使用者的服務中斷或擴充節流時段。 您可以顯示易懂的 」 再試一次 」 訊息。

沒有一體適用的重試原則。 您可以重試多次，並等候時間非同步背景工作處理序中比在使用者正在等待回應同步 web 應用程式中。 您可以等候時間之間的關聯式資料庫服務的重試次數比您快取服務。 以下是一些樣本，建議使用重試原則，以讓您了解如何數字可能會不同。 （「 快速第一個"表示第一次重試之前沒有延遲。

![範例的重試原則](transient-fault-handling/_static/image1.png)

SQL Database 重試原則指導方針，請參閱[暫時性錯誤和 SQL Database 連接錯誤的疑難排解](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/)。

## <a name="summary"></a>總結

重試/輪詢策略可協助讓暫時錯誤看不到客戶大部分的情況下，以及 Microsoft 提供的架構可讓您減少您實作策略，無論您使用 ADO.NET、 Entity Framework 或 Azure 的工作儲存體服務。

在[下一章](distributed-caching.md)，我們將探討如何改善效能和可靠性使用分散式快取。

## <a name="resources"></a>資源

如需詳細資訊，請參閱下列資源：

文件

- [Azure 雲端服務中大規模服務設計的最佳作法](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)。 Mark Simms 和 Michael Thomassy 詘躩裛。 類似於保全數列但進入詳細的使用說明。 請參閱遙測和診斷 > 一節。
- [Failsafe： 具有恢復功能雲端架構指引](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx)。 Marc Mercuri、 Ulrich Homann 和 Andrew Townhill 詘躩裛。 FailSafe 影片系列網頁版本。
- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/library/dn568099.aspx)。 請參閱 < 重試模式、 排程器代理程式監督員模式。
- [Azure SQL Database 中的容錯](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx)。 Tong Petrossian 部落格文章。
- [Entity Framework-連接恢復功能 / 重試邏輯](https://msdn.microsoft.com/data/dn456835)。 如何使用和自訂暫時性錯誤處理功能的 Entity Framework 6。
- [連接恢復功能和 Entity framework，ASP.NET MVC 應用程式中的命令攔截](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)。 第四個九部分的教學課程系列，示範如何設定 SQL 資料庫的 EF 6 連接恢復功能。

視訊

- [FailSafe： 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 九部分 Ulrich Homann、 Marc Mercuri 和 Mark Simms 的系列。 高層級概念與架構原則非常可存取且有趣的方式，呈現劇本取自與實際客戶的 Microsoft 客戶諮詢團隊 (CAT) 體驗。 請參閱斷路器 3 的時段開始 40:55 中的討論。
- [建置大型： 學到來自 Azure 客戶-II](https://channel9.msdn.com/Events/Build/2012/3-030)。 Mark Simms 討論有關設計的失敗，暫時性錯誤處理和追蹤記錄的所有項目。

程式碼範例

- [雲端服務在 Azure 中的基本概念](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)。 範例應用程式由 Microsoft Azure 客戶諮詢小組，示範如何使用[Enterprise Library 暫時性錯誤處理區塊](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)(TFH)。 如需詳細資訊，請參閱[雲端服務基本概念資料存取層-暫時性錯誤處理](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx)。 TFH 建議使用 ADO.NET 直接 （不使用 Entity Framework） 的資料庫存取權。

> [!div class="step-by-step"]
> [上一頁](monitoring-and-telemetry.md)
> [下一頁](distributed-caching.md)
