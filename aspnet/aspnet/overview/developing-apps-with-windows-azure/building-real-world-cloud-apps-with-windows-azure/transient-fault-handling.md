---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: 暫時性錯誤處理 （建置使用 Azure 的真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: riande
ms.date: 11/03/2015
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: 26f1d040e4857899ddcff79b778d0af62b66134d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830172"
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>暫時性錯誤處理 （建置使用 Azure 的真實世界的雲端應用程式）
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。 它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。 電子書的相關資訊，請參閱[第 1 章](introduction.md)。


當您設計的真實世界的雲端應用程式時，您必須考慮的事情之一是如何處理暫時性服務中斷。 此問題是在雲端應用程式中唯一重要的因為您依賴網路連線和外部服務。 您可以經常取得通常自我修復的小問題，如果您尚未準備好以智慧方式處理它們，它們將會產生不愉快的體驗中為您的客戶。

## <a name="causes-of-transient-failures"></a>暫時性失敗的原因

在雲端環境中，您會發現，失敗，並卸除的資料庫的連線會定期發生。 這是部分是因為您將會透過相較於內部部署環境的多個負載平衡器您的 web 伺服器和資料庫伺服器上具有的直接實體連線。 此外，有時當您準備相依於多租用戶服務時，才會看到呼叫的服務變慢或逾時時間因為其他人使用 「 服務 」 達到高度。 在其他情況下您可能太過頻繁，達到服務的使用者，並服務刻意節流 – 拒絕連線 – 您為了防止造成負面影響其他租用戶的服務。

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>使用智慧型的重試/撤退邏輯來減少暫時性失敗的影響

而不是擲回例外狀況，並顯示您的客戶無法使用 」 或 「 錯誤頁面，您可以辨識錯誤，通常屬於暫時性的並自動重試錯誤，導致此作業希望，之前長時間，您將會成功。 作業會成功在第二次嘗試中，大部分的情況，並將從錯誤復原而不需要客戶曾未曾注意時發生問題。

有數種方式，您可以實作智慧的重試邏輯。

- Microsoft Patterns &amp; Practices 群組擁有[暫時性錯誤處理應用程式區塊](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx)執行的所有項目，如果您使用 ADO.NET 的 SQL 資料庫 （不透過 Entity Framework) 的存取權。 您只設定原則的重試 – 多少次重試查詢或命令和等待的時間之間嘗試 – 和包裝您的 SQL 程式碼中*使用*區塊。

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    也支援 TFH [Azure In-role Cache](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx)並[Service Bus](https://azure.microsoft.com/services/service-bus/)。
- 當您使用 Entity Framework 時您通常不直接使用 SQL 連線，因此您無法使用此模式和實務的套件，但 Entity Framework 6 至 framework 建置這類的重試邏輯。 類似的方式，您指定的重試策略，，然後會 EF 使用該策略，每當它存取資料庫。

    若要修正其應用程式中使用這項功能，我們只需要是新增類別衍生自*DbConfiguration*並開啟 重試邏輯。

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    對於 SQL Database 的 framework 識別為通常是暫時性錯誤的例外狀況，顯示的程式碼會指示 EF 來重試此作業最多 3 次，指數退避法延遲重試延遲時間上限為 5 秒之間。 指數退避法表示每個失敗的重試後就會等候較長的一段時間後再試。 如果資料列中的三次嘗試失敗，則會擲回例外狀況。 斷路器的下列小節說明您為何想指數退避法及有限的數目的重試次數。

    當您使用 Azure 儲存體服務，它修正應用程式會為 Blob，以及.NET 儲存體用戶端 API 已經實作相同類型的邏輯，您可以有類似的問題。 您只需要指定重試原則，或甚至不需要這麼做，如果您滿意預設值。

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>斷路器

有幾個原因，您不想要重試次數太多太長一段：

- 太多的使用者持續重試失敗的要求可能會降低其他使用者的體驗。 如果數以百萬計的人員會進行重複所有重試要求，可以會佔用 IIS 分派佇列，否則可處理成功的要求提供服務時，防止您的應用程式。
- 如果每個人都正在重試由於服務失敗，那里可能會這麼多的要求排入佇列服務取得大量湧入，啟動復原時。
- 如果錯誤是因為節流，而且有的服務會使用節流的時間範圍，一直持續不斷地重試，可以將該視窗移出，促使節流繼續。
- 您可能必須等待 web 網頁的使用者，來呈現。 製作人等候太長可能會更惱人的該相當快速地為客戶提供建議他們稍後再試。

指數退避解決這些問題的一些限制服務可以從您的應用程式取得的重試的頻率。 但您也必須能夠*斷路器*： 這表示，在特定重試您的應用程式會停止重試，並接受某些其他動作，例如下列其中之一的臨界值：

- 自訂後援。 如果您無法從路透社取得股票的價格，也許您可以從取得 Bloomberg;或者，如果您無法從資料庫取得資料，或許您可以取得它從快取。
- 無訊息會失敗。 如果您需要從服務不是孤注一擲應用程式，只傳回 null 時，您無法取得資料。 如果您顯示 It 工作，而且 Blob 服務沒有回應，您可以顯示工作詳細資料，而不需要將映像。
- 快速失敗。 若要避免資料大量湧入的服務，將使用者的錯誤重試要求可能會導致服務中斷，其他使用者，或擴充節流時段。 您可以顯示的易記 「 稍後再試 」 訊息。

沒有一體適用的重試原則。 您可以重試一次和非同步的背景工作處理序中再等待，比您在同步的 web 應用程式中使用者等待回應的位置。 您可以再重試之間等待的關聯式資料庫服務比您的快取服務。 以下是一些範例，建議使用重試原則以讓您了解的數字的有所不同。 （「 快速第一個"表示第一次重試之前的任何延遲。

![範例重試原則](transient-fault-handling/_static/image1.png)

SQL Database 重試原則的指引，請參閱[針對暫時性錯誤和 SQL database 的連線錯誤進行疑難排解](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/)。

## <a name="summary"></a>總結

重試/退避策略可協助讓暫存錯誤看不到的客戶大多時候，而且 Microsoft 提供您用來減少您的工作實作的策略，無論您使用 ADO.NET、 Entity Framework 或 Azure 的架構儲存體服務。

在 [[下一步] 一章](distributed-caching.md)，我們將探討如何改善效能和可靠性，使用分散式快取。

## <a name="resources"></a>資源

如需詳細資訊，請參閱下列資源：

文件

- [Azure 雲端服務上大規模服務設計的最佳作法](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)。 Mark Simms 和 Michael Thomassy 詘躩裛。 類似於保全數列但作法的詳細說明。 請參閱 [遙測和診斷] 區段。
- [Failsafe︰ 恢復雲端架構指引](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx)。 Marc Mercuri、 Ulrich Homann 和 Andrew Townhill 詘躩裛。 FailSafe 影片系列的網頁版本。
- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/library/dn568099.aspx)。 請參閱 < 重試模式、 排程器代理程式監督員模式。
- [Azure SQL Database 中的容錯](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx)。 Tony Petrossian 部落格文章。
- [Entity Framework-連接恢復 / 重試邏輯](https://msdn.microsoft.com/data/dn456835)。 如何使用和自訂暫時性錯誤處理功能的 Entity Framework 6。
- [連線恢復功能和命令攔截與 ASP.NET MVC 應用程式中的 Entity Framework](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)。 第四個九段式教學課程系列中，示範如何設定 SQL database 的 EF 6 連接恢復功能。

視訊

- [FailSafe︰ 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 Ulrich Homann、 Marc Mercuri 和 Mark Simms、 包含九部組件系列。 提供高階的概念和架構原則非常可存取且有趣的方式，取自 Microsoft 客戶諮詢團隊 (CAT) 體驗，與實際客戶的劇本。 在第 3 集開始 40:55 斷路器的討論，請參閱。
- [建置大型： 從 Azure 的客戶-第 ii 部分 Lessons learned](https://channel9.msdn.com/Events/Build/2012/3-030)。 Mark Simms 討論有關設計的失敗，暫時性錯誤處理與檢測所有項目。

程式碼範例

- [雲端服務基本概念，在 Azure 中的](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)。 Microsoft Azure 客戶諮詢小組會示範如何使用所建立的應用程式範例[Enterprise Library 暫時性錯誤處理區塊](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)(TFH)。 如需詳細資訊，請參閱 <<c0> [ 雲端服務基礎資料存取層 – 暫時性錯誤處理](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx)。 TFH 建議用於直接 （不使用 Entity Framework） 中使用 ADO.NET 進行資料庫存取。

> [!div class="step-by-step"]
> [上一頁](monitoring-and-telemetry.md)
> [下一頁](distributed-caching.md)
