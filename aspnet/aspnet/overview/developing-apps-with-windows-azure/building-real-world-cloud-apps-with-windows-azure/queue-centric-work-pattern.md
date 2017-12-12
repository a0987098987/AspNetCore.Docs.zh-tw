---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: "佇列為主的工作模式 （使用 Azure 建置實際的雲端應用程式） |Microsoft 文件"
author: MikeWasson
description: "Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: 125d555a9e170ef35dd99e0409a2442d5f9ae34a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>佇列為主的工作模式 （使用 Azure 建置實際的雲端應用程式）
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 E 書籍的相關資訊，請參閱[第一章](introduction.md)。


舊版中，我們所見，使用多個服務可能會導致 「 複合"SLA、 應用程式的有效 SLA 所在*產品*的個別的 Sla。 例如，它修正應用程式所使用的網站、 儲存和 SQL Database。 如果這些服務的任何一個失敗時，應用程式會傳回錯誤給使用者。

快取是好的方法來處理暫時性失敗的唯讀內容。 但如果您的應用程式需要來執行工作？ 比方說，當使用者提交新修正它的工作，應用程式無法只將工作放入快取。 應用程式需要修正它工作寫入至永續性資料存放區，因此可處理。

是以佇列為中心的工作模式的運作方式。 此模式可讓 web 層和後端服務之間的鬆散偶合。

模式的運作方式如下。 當應用程式取得要求時，它會將工作項目放入佇列，並立即傳回回應。 然後個別的後端程序提取從佇列的工作項目，並執行這項工作。

佇列為主的工作模式可用於：

- 工作所耗用的時間 （高延遲）。
- 需要的工作，可能永遠無法使用的外部服務。
- 也就是使用大量的資源 (CPU 高)。
- 而獲益率調節 （受限於突然負載暴增） 的工作。

## <a name="reduced-latency"></a>減少的延遲

佇列是的每當您在進行耗時的工作很有用。 如果工作需要幾秒鐘，或更久，而不會封鎖使用者，將工作項目放入佇列。 告訴使用者 「 我們正努力，"，然後處理在背景工作使用的佇列接聽程式。

例如，某項目在線上零售店購買之後，網站會確認您的訂單立即。 但並不表示您的資料已經傳遞中。 他們將工作放在佇列中，以及在背景中進行信用查核，準備用於出貨，您的項目和這些等。

短延遲的情況下，端對端時間總計可能較長的使用佇列，相較於以同步方式執行該工作。 但即使如此，其他好處可能會超過該缺點。

## <a name="increased-reliability"></a>增加的可靠性

修正它，我們探討了到目前為止的版本，在與 SQL 資料庫後端緊密結合的 web 前端。 如果 SQL database 服務無法使用，使用者會收到錯誤。 如果重試無法運作 （也就是失敗是多個暫時性），的唯一可以執行的是顯示錯誤，並要求使用者再試一次。

![圖表顯示的 web 前端失敗的 SQL 資料庫後端失敗時](queue-centric-work-pattern/_static/image1.png)

使用者提交修正其工作時，請使用佇列，應用程式可以將訊息寫入佇列。 訊息內容是[JSON](http://json.org/)工作的表示法。 將訊息寫入至佇列，因為應用程式會傳回，並立即向使用者顯示成功訊息。

如果任何 – 例如，SQL database 或佇列接聽程式--後端服務都離線時，使用者仍然可以提交新修正它的工作。 直到後端服務再度變成可用，只會將佇列訊息。 此時後, 端服務會跟上待處理項目。

![Web 前端繼續運作，SQL Database 錯誤時所顯示的圖表](queue-centric-work-pattern/_static/image2.png)

此外，現在您可以新增更多的後端邏輯而不需擔心前端的恢復功能。 例如，您可以將電子郵件或 SMS 訊息傳送至擁有者，每當新修正它被指派。 如果電子郵件或 SMS 服務變成無法使用，可處理其他所有項目，然後再將訊息放入不同的佇列來傳送電子郵件/SMS 訊息。

之前，我們有效的 SLA 是 Web 應用程式&times;儲存體&times;SQL Database = 99.7%。 (請參閱[存留失敗設計](design-to-survive-failures.md)。)

當我們變更應用程式使用的佇列時，web 前端只依賴 Web 應用程式和儲存體，複合應用 99.8%的 SLA。 （請注意佇列是 Azure 儲存體服務的一部分，因此它們會包含在相同的 SLA 為 blob 儲存體）。

如果您需要比 99.8%甚至更好，您可以在兩個不同區域中建立兩個佇列。 將一個當做主要資料庫，而另一個指定為次要資料庫。 在您的應用程式容錯移轉至次要佇列如果主要佇列無法使用。 這兩個無法在同一時間的機會便相當小。

## <a name="rate-leveling-and-independent-scaling"></a>速率均衡，以及獨立縮放比例

佇列也可用於所謂*速率調節*或*負載調節*。

Web 應用程式通常是容易量突然暴增的流量。 雖然您可以使用自動調整，以自動新增 web 伺服器來處理增加的 web 流量，自動調整可能無法迅速做出反應，來處理負載中的突然暴增。 如果網頁伺服器可以卸載部分他們必須將訊息寫入佇列所做的工作，他們可以處理更多流量。 後端服務可以從佇列讀取訊息並處理它們。 佇列的深度會擴張或縮小，連入負載而改變。

與大部分的耗時工作卸載至後端服務，web 層能更輕鬆地回應流量中突然出現尖峰。 和您節省成本，因為較少的 web 伺服器可以處理任何指定的數量的流量。

您可以獨立擴充，web 層和後端服務。 例如，您可能需要三個 web 伺服器，但只有一個伺服器處理佇列的訊息。 或者，如果您在背景中執行需要大量計算工作，您可能需要更多的後端伺服器。

![](queue-centric-work-pattern/_static/image3.png)

自動調整的運作方式的後端服務和 web 層。 您可以向上延展或縮小正在處理的工作在佇列中後, 端 Vm 的 CPU 使用量為基礎的 Vm 數目。 或者，您可以在佇列中有多少項目為基礎的自動調整規模。 例如，您可以分辨自動調整規模並再試一次在佇列中保留最多 10 個項目。 如果佇列擁有 10 個以上的項目，自動調整規模將會加入 Vm。 當它們趕上時，自動調整規模將會終止額外的 Vm。

## <a name="adding-queues-to-the-fix-it-application"></a>新增的修正會將它佇列應用程式

若要實作的佇列模式，我們需要修正它應用程式的兩個變更。

- 當使用者提交新修正它的工作時，請將工作放在佇列中，而不是寫入至資料庫。
- 建立後端服務處理之訊息佇列中。

佇列中，我們將使用[Azure 佇列儲存體服務](https://www.windowsazure.com/en-us/develop/net/how-to-guides/queue-service/)。 另一個選項是使用[Azure 服務匯流排](https://docs.microsoft.com/azure/service-bus/)。

若要決定要使用的佇列服務，請考慮您的應用程式需要的方式傳送和接收訊息佇列中：

- 如果您有合作的產生者和競爭取用者，請考慮使用 Azure 佇列儲存體服務。 「 Cooperating 生產者"表示多個處理序會加入佇列的訊息。 「 競爭取用者 」 表示多個處理序提取訊息從佇列處理它們，但任何指定的訊息只能處理一個 「 取用者。 」 如果您需要更多的輸送量比就可以單一佇列，請使用其他的佇列和/或其他儲存體帳戶。
- 如果您需要[發佈/訂閱模型](http://en.wikipedia.org/wiki/Publish/subscribe)，請考慮使用 Azure 服務匯流排佇列。

修正它應用程式非常適合合作的產生者和競爭消費者模型。

另一個考量是應用程式的可用性。 佇列儲存體服務是我們會使用 blob 儲存體，因此使用它不會影響我們的 SLA 的相同服務的一部分。 Azure Service Bus 是個別的服務，與它自己的 SLA。 如果使用 Service Bus 佇列，我們必須將納入其他的 SLA 百分比，並且我們複合 SLA 會是較低。 當您選擇的佇列服務時，請確定您了解您所選擇的應用程式可用性的影響。 如需詳細資訊，請參閱[資源](#resources)> 一節。

## <a name="creating-queue-messages"></a>正在建立 「 訊息佇列

若要修正該工作放入佇列中，web 前端，請執行下列步驟：

1. 建立[CloudQueueClient](https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx)執行個體。 `CloudQueueClient`執行個體用來執行對佇列服務的要求。
2. 如果尚未存在，請建立佇列。
3. 序列化修正其工作。
4. 呼叫[CloudQueue.AddMessageAsync](https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx)將放在佇列的訊息。

我們將會執行這項工作中建構函式和`SendMessageAsync`方法的新`FixItQueueManager`類別。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

這裡我們使用[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)序列化為 JSON 格式 fixit 的程式庫。 您可以使用任何您偏好的序列化方法。 JSON 的優點是能夠被人類看得懂，同時會比 XML。

生產環境品質的程式碼會加入錯誤處理邏輯、 暫停如果資料庫變成無法使用，更完全地處理復原、 應用程式啟動時建立佇列和管理 「[有害 」 訊息](https://msdn.microsoft.com/en-us/library/ms789028(v=vs.110).aspx)。 （有害訊息是因為某些原因無法處理的訊息。 您不想成放置在佇列中，其中背景工作角色會持續嘗試進行處理、 失敗、 再試一次、 失敗，等等的有害訊息。）

在前端的 MVC 應用程式中，我們需要更新建立新工作的程式碼。 而不是將工作放到儲存機制中，呼叫`SendMessageAsync`方法如上所示。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>處理佇列的訊息

若要處理佇列中的訊息，我們將建立後端服務。 後端服務將會執行無限迴圈，執行下列步驟：

1. 從佇列取得下一個訊息。
2. 還原序列化訊息至修正其工作。
3. 寫入資料庫中修正該工作。

若要裝載後端服務，我們將建立 Azure 雲端服務，其中包含*背景工作角色*。 背景工作角色可以執行後端處理的一或多個 Vm 所組成。 在這些 Vm 中執行的程式碼會以提取訊息從佇列可用時。 對於每個訊息，我們將會還原序列化 JSON 承載，修正它工作實體的執行個體寫入資料庫，使用我們稍早在 web 層中使用的相同儲存機制。

下列步驟顯示如何加入背景工作角色專案，以具有標準的 web 專案的方案。 已修正它在專案中，您可以下載完成這些步驟。

先將雲端服務專案加入 Visual Studio 方案。 以滑鼠右鍵按一下方案，然後選取**新增**，然後**新專案**。 在左窗格中，依序展開**Visual C#**選取**雲端**。

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

在**新的 Azure 雲端服務**] 對話方塊中，展開 [ **Visual C#**在左窗格中的節點。 選取**背景工作角色**，然後按一下向右箭號圖示。

![](queue-centric-work-pattern/_static/image6.png)

(請注意，您也可以加入*web 角色*。 我們無法執行修正它在相同的雲端服務，而非 Azure 網站中執行的前端。 有一些優點，讓您更輕鬆地協調前端與後端之間的連線。 不過，為了簡化此示範中，我們要保持前端在 Azure App Service Web 應用程式和只有在雲端服務中執行後端。）

將預設名稱被指派給背景工作角色。 若要變更名稱，將滑鼠放在右窗格中的背景工作角色，然後按一下鉛筆圖示。

![](queue-centric-work-pattern/_static/image7.png)

按一下**確定**完成對話方塊。 這會將兩個專案加入 Visual Studio 方案。

- Azure 專案定義雲端服務，包括組態資訊。
- 定義背景工作角色的背景工作角色專案。

![](queue-centric-work-pattern/_static/image8.png)

如需詳細資訊，請參閱[使用 Visual Studio 建立 Azure 專案。](https://msdn.microsoft.com/en-us/library/windowsazure/ee405487.aspx)

在背景工作角色中，我們輪詢訊息藉由呼叫`ProcessMessageAsync`方法`FixItQueueManager`我們之前看到的類別。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

`ProcessMessagesAsync`方法會檢查是否有訊息等待。 如果有的話，它會還原序列化成訊息`FixItTask`實體，並將實體儲存在資料庫中。 它會迴圈直到佇列是空的。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

輪詢佇列訊息會產生小型的交易收費，所以沒有任何訊息等待處理，在背景工作角色的`RunAsync`方法會等候第二個輪詢之前再次呼叫`Task.Delay(1000)`。

在 web 專案中，新增非同步程式碼可以自動改善效能，因為 IIS 管理有限的執行緒集區。 這不是背景工作角色專案中的案例。 若要改善延展性的背景工作角色，您可以撰寫多執行緒程式碼，或使用非同步程式碼來實作[平行程式設計](https://msdn.microsoft.com/en-us/library/ff963553.aspx)。 此範例不會實作平行程式設計，但會示範如何讓程式碼非同步，因此您可以實作平行程式設計。

## <a name="summary"></a>總結

在本章中，您已經看到如何改善應用程式回應性、 可靠性和延展性，藉由實作佇列為主的工作模式。

這是在此電子書，涵蓋的 13 模式的最後一個，但當然有許多其他的模式和作法，可協助您建置成功的雲端應用程式。 [最後一章](more-patterns-and-guidance.md)您尚未已涵蓋這些 13 模式中的主題提供資源的連結。

<a id="resources"></a>
## <a name="resources"></a>資源

如需有關佇列的詳細資訊，請參閱下列資源。

文件集：

- [Microsoft Azure 儲存體佇列第 1 部： 入門](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/)。 細明體 Schacherl 的文章。
- [執行背景工作](https://msdn.microsoft.com/en-us/library/ff803365.aspx)，第 5 章[應用程式移至雲端，第 3 版](https://msdn.microsoft.com/en-us/library/ff728592.aspx)從 Microsoft Patterns and Practices。 (特別是，區段[」 使用 Azure 儲存體佇列 」](https://msdn.microsoft.com/en-us/library/ff803365.aspx#sec7)。)
- [延展性和 Azure 上的佇列架構傳訊解決方案的成本的效益最大化的最佳作法](https://msdn.microsoft.com/en-us/library/windowsazure/hh697709.aspx)。 由 Valery Mizonov 詘躩裛。
- [比較 Azure 佇列和服務匯流排佇列](https://msdn.microsoft.com/en-us/magazine/jj159884.aspx)。 MSDN Magazine 文件，提供可協助您選擇要使用的佇列服務的其他資訊。 本文提及 Service Bus 是依存於 ACS 進行驗證，這表示無法使用 ACS 時 SB 佇列將無法再使用。 不過，因為發行項所撰寫，SB 已變更，您可以使用[SAS 權杖](https://msdn.microsoft.com/en-us/library/windowsazure/dn170477.aspx)作為 ACS 的替代方案。
- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/en-us/library/dn568099.aspx)。 請參閱非同步傳訊入門、 管線和篩選模式中，補償的交易模式、 競爭取用者模式、 CQRS 模式。
- [CQRS 旅程](https://msdn.microsoft.com/en-us/library/jj554200)。 由 Microsoft Patterns and Practices CQRS 相關的電子書。

影片：

- [FailSafe： 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 Ulrich Homann、 Marc Mercuri 和 Mark Simms 九部影片系列。 高層級概念與架構原則非常可存取且有趣的方式，呈現劇本取自與實際客戶的 Microsoft 客戶諮詢團隊 (CAT) 體驗。 如需 Azure 儲存體服務和佇列的簡介，請參閱時段 5 開始 35:13。

>[!div class="step-by-step"]
[上一頁](distributed-caching.md)
[下一頁](more-patterns-and-guidance.md)
