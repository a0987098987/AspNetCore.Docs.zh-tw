---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 實習實驗室：使用 SignalR 即時 Web 應用程式 |Microsoft Docs
author: bradygaster
description: 即時 Web 應用程式功能的伺服器端將內容推至連線的用戶端時，即時的能力。 適用於 ASP.NET 開發人員，ASP...
ms.author: bradyg
ms.date: 07/16/2014
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: d4998c8b739b4b1a06699a17464a7399a87a8595
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837503"
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>實習實驗室：使用 SignalR 即時 Web 應用程式
====================

藉由[Web Camp 小組](https://twitter.com/webcamps)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[下載 Web 研討會訓練套件](http://aka.ms/webcamps-training-kit)

> 即時 Web 應用程式功能的伺服器端將內容推至連線的用戶端時，即時的能力。 ASP.NET 開發人員，如**ASP.NET SignalR**是將即時 web 功能新增至他們的應用程式的程式庫。 它會利用數個傳輸，會自動選取最佳可用的傳輸用戶端和伺服器的最佳可用的傳輸。 它會善用**WebSocket**，HTML5 API，可讓瀏覽器與伺服器之間的雙向通訊。
> 
> **SignalR**也提供簡單、 高階 API，以進行用戶端 RPC 的伺服器 （在用戶端的瀏覽器，從伺服器端.NET 程式碼中呼叫 JavaScript 函式） 在您的 ASP.NET 應用程式，以及新增實用的攔截程序進行連線管理例如連接/中斷連接事件、 分組連線和授權。
> 
> **SignalR**是一些需要執行用戶端與伺服器之間的即時工作傳輸的抽象概念。 A **SignalR**連接一開始為 HTTP，並再升級至**WebSocket**連接，如果有的話。 **WebSocket**是的理想傳輸**SignalR**，因為這樣會讓伺服器記憶體最有效率地使用具有最低的延遲，而且具有最基礎的功能 (例如完整的雙工用戶端之間的通訊和伺服器），但它也會有最嚴苛的需求：**WebSocket**需要使用伺服器**Windows Server 2012**或是**Windows 8**，連同 **.NET Framework 4.5**。 如果不符合這些需求， **SignalR**會嘗試使用其他傳輸進行其連線 (例如*Ajax 長時間輪詢*)。
> 
> **SignalR** API 包含用戶端和伺服器之間進行通訊的兩個模型：**持續性連線**並**中樞**。 A**連線**代表簡單的端點用來傳送單一位收件者，分組，或廣播訊息。 A**中樞**連接 API，可讓您的用戶端與伺服器彼此直接呼叫方法為基礎建置更高層級的管線。
> 
> ![SignalR 架構](real-time-web-applications-with-signalr/_static/image1.png)
> 
> 所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit)。


<a id="Overview"></a>
## <a name="overview"></a>總覽

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將了解如何：

- 若要使用 SignalR 用戶端，從伺服器傳送通知。
- 向外延展您 SignalR 應用程式使用**SQL Server**。

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

需要下列項目才能完成這個實際操作實驗室：

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更新版本

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

若要在這個實際操作實驗室中執行的練習，您必須先設定您的環境。

1. 開啟 Windows 檔案總管視窗並瀏覽至實驗室**來源**資料夾。
2. 以滑鼠右鍵按一下**Setup.cmd** ，然後選取**系統管理員身分執行**來啟動安裝程序，將會設定您的環境並安裝這個實驗室的 Visual Studio 程式碼片段。
3. 如果會顯示 [使用者帳戶控制] 對話方塊中，確認要繼續進行的動作。

> [!NOTE]
> 請確定您執行安裝程式之前檢查這個實驗室的所有相依性。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>使用程式碼片段

整個實驗室文件中，您將會指示要插入程式碼區塊。 為了方便起見，大部分的這段程式碼提供 Visual Studio 程式碼片段，您可以從 Visual Studio 2013，若要避免以手動方式新增內存取。

> [!NOTE]
> 每個練習會伴隨起始方案，位於**開始**練習，可讓您依照每個練習，獨立於其他的資料夾。 請留意練習期間新增的程式碼片段缺少這些啟動解決方案，並可能無法運作，直到您已完成練習。 在練習的原始程式碼，您也可以找到**結束**資料夾包含 Visual Studio 方案，以程式碼所產生的相對應的練習中的步驟。 如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案與指引。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室還包含下列練習：

1. [有關使用 SignalR 即時資料](#Exercise1)
2. [向外延展使用 SQL Server](#Exercise2)

估計的時間才能完成這個實驗室：**60 分鐘**

> [!NOTE]
> 當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。 每個預先定義的集合以符合特定的開發樣式設計，並決定視窗版面配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。 在這個實驗室中的程序說明完成指定的工作，在 Visual Studio 中使用時所需的動作**一般開發設定**集合。 如果您選擇不同的設定集合，您的開發環境，可能會有在步驟中，您應該考慮到的差異。


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>練習 1:有關使用 SignalR 即時資料

交談通常是用做為範例，您可以執行整個即時 Web 功能更大。 每當使用者重新整理網頁，以查看新的資料或此頁面會實作 Ajax 長時間輪詢來擷取新的資料，您可以使用 SignalR。

支援 SignalR**伺服器推播**或是**廣播**功能; 它會自動處理連接管理。 在 用戶端與伺服器通訊的傳統 HTTP 連線，針對每個要求，重新建立連接時，但 SignalR 提供用戶端與伺服器之間的持續連線。 Signalr 伺服器程式碼呼叫中使用遠端程序呼叫 (RPC) 的瀏覽器用戶端程式碼，而不是要求-回應模型我們知道今天。

在這個練習中，您會設定**Geek 測驗**使用 SignalR 來顯示更新的計量，而不需要重新整理整個頁面的統計資料儀表板的應用程式。

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>工作 1 – 瀏覽 Geek 測驗統計資料 頁面

在這個工作中，您會瀏覽應用程式，並確認 [統計資料] 頁面的顯示方式及如何改善資訊的方式更新。

1. 開啟**Visual Studio Express 2013 for Web** ，然後開啟**GeekQuiz.sln**解決方案位於**Source\Ex1 WorkingWithRealTimeData\Begin**資料夾。
2. 按下**F5**執行方案。 **登入**頁面應該會出現在瀏覽器。

    ![執行解決方案](real-time-web-applications-with-signalr/_static/image2.png "執行解決方案")

    *執行解決方案*
3. 按一下 **註冊**應用程式中建立新的使用者頁面右上角。

    ![註冊連結](real-time-web-applications-with-signalr/_static/image3.png "註冊連結")

    *註冊連結*
4. 在**註冊**頁面上，輸入**使用者名稱**並**密碼**，然後按一下**註冊**。

    ![註冊使用者](real-time-web-applications-with-signalr/_static/image4.png "註冊使用者")

    *註冊使用者*
5. 應用程式註冊新的帳戶，並驗證使用者並將其重新導向回到首頁上顯示的第一個測驗問題。
6. 開啟**統計資料**頁面上，在新視窗中，並將放**首頁**頁面並**統計資料**頁面上並排顯示。

    ![並排顯示視窗](real-time-web-applications-with-signalr/_static/image5.png "側邊 windows")

    *並排顯示視窗*
7. 在 **首頁**頁面上，按一下其中一個選項，以回答問題。

    ![回答](real-time-web-applications-with-signalr/_static/image6.png "回答的問題")

    *回答的問題*
8. 按一下其中一個按鈕之後, 應該會出現答案。

    ![回答問題，正確](real-time-web-applications-with-signalr/_static/image7.png "解答正確的問題")

    *正確回答的問題*
9. 請注意 [統計資料] 頁面中提供的資訊已過期。 重新整理頁面以查看更新的結果。

    ![統計資料 頁面](real-time-web-applications-with-signalr/_static/image8.png "統計資料 頁面")

    *統計資料 頁面*
10. 返回 Visual Studio，並停止偵錯。

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>工作 2-新增 SignalR 來 Geek 測驗，以顯示線上的圖表

在這個工作中，您將加入方案中的 SignalR，並將更新傳送至用戶端會自動新增回應傳送至伺服器時。

1. 從**工具**功能表，在 Visual Studio 中，選取**NuGet 套件管理員**，然後按一下**Package Manager Console**。
2. 在 [ **Package Manager Console** ] 視窗中，執行下列命令：

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![SignalR 套件安裝](real-time-web-applications-with-signalr/_static/image9.png "SignalR 套件安裝")

    *SignalR 套件安裝*

   > [!NOTE]
   > 安裝時**SignalR** NuGet 套件版本 2.0.2 從全新的 MVC 5 應用程式，您必須手動更新**OWIN**封裝以版本 2.0.1 （或更新版本） 安裝 SignalR 之前。 若要這樣做，您可以執行下列指令碼**Package Manager Console**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > 中的 SignalR 未來版本中，將會自動更新 OWIN 相依性。
3. 在 [**方案總管] 中**，展開**指令碼**資料夾，請注意，SignalR *js*檔案已加入至方案。

    ![SignalR JavaScript 參考](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript 參考")

    *SignalR JavaScript 參考*
4. 在 [**方案總管] 中**，以滑鼠右鍵按一下**GeekQuiz**專案，然後選取**新增** | **新資料夾**，並將它命名**中樞**。
5. 以滑鼠右鍵按一下**集線器**資料夾，然後選取**新增 |新的項目**。

    ![加入新項目](real-time-web-applications-with-signalr/_static/image11.png "加入新項目")

    *加入新項目*
6. 在 **加入新項目**對話方塊中，選取**Visual C# |Web |SignalR**的左窗格中，選取的節點**SignalR Hub 類別 (v2)** 從中間窗格中，將檔案命名**StatisticsHub.cs** ，按一下 **新增**。

    ![加入新項目 對話方塊](real-time-web-applications-with-signalr/_static/image12.png "加入新項目對話方塊")

    *加入新項目對話方塊*
7. 中的程式碼取代**StatisticsHub**為下列程式碼的類別。

    (程式碼片段- *RealTimeSignalR-Ex1-StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. 開啟**Startup.cs** ，並在結尾新增下面這一行**組態**方法。

    (程式碼片段- *RealTimeSignalR-Ex1-MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. 開啟**StatisticsService.cs**頁面內**服務**資料夾，並新增下列 using 指示詞。

    (程式碼片段- *RealTimeSignalR-Ex1-UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. 若要通知連線的用戶端的更新，您先擷取**內容**目前連接的物件。 **中樞**物件包含方法，將訊息傳送至單一用戶端或廣播到所有已連線的用戶端。 將下列方法加入**StatisticsService**廣播統計資料的類別。

    (程式碼片段- *RealTimeSignalR-Ex1-NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > 上面的程式碼，您會使用任意的方法名稱在用戶端上呼叫的函式 (也就是： *updateStatistics*)。 您指定的方法名稱會解譯為動態物件，這表示沒有任何 IntelliSense 或它的編譯時間驗證。 在執行階段會評估運算式。 當方法呼叫執行時，SignalR 就會傳送給用戶端的方法名稱和參數值。 如果用戶端有符合的名稱，就會呼叫方法的參數值傳遞給它的方法。 如果用戶端上找不到任何相符的方法，不會引發錯誤。 如需詳細資訊，請參閱[ASP.NET SignalR 中樞 API 指南](../guide-to-the-api/hubs-api-guide-server.md)。
11. 開啟**TriviaController.cs**頁面內**控制站**資料夾，並新增下列 using 指示詞。

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. 將下列反白顯示的程式碼，加入**Post**動作方法。

    (程式碼片段- *RealTimeSignalR-Ex1-NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. 開啟**Statistics.cshtml**頁面內**檢視 |首頁**資料夾。 找出**指令碼**區段，並在區段的開頭新增下列指令碼參考。

    (程式碼片段- *RealTimeSignalR-Ex1-SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > 當您將 SignalR 和其他指令碼程式庫加入您的 Visual Studio 專案時，套件管理員可能會安裝的版本比本主題中所顯示的版本還新的 SignalR 指令碼檔案。 請確定您的程式碼中的指令碼參考符合安裝在您專案的指令碼程式庫的版本。
14. 新增下列醒目提示的程式碼，以連線到 SignalR 中樞的用戶端，並從中樞收到新訊息時，更新統計資料。

    (Code Snippet - *RealTimeSignalR - Ex1 - SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    在此程式碼中，您建立中樞 Proxy 和註冊事件處理常式來接聽的伺服器所傳送的訊息。 您在此情況下，接聽訊息透過傳送*updateStatistics*方法。

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>工作 3-執行解決方案

在這個工作中，您將執行解決方案，以驗證使用 SignalR 接聽新的問題之後自動更新統計資料檢視。

1. 按下**F5**執行方案。

    > [!NOTE]
    > 如果您尚未登入應用程式，進行登入您在工作 1 中建立的使用者。
2. 開啟**統計資料**頁面上，在新視窗中，並將放**首頁**頁面並**統計資料**頁面上並排顯示，如同在工作 1。
3. 在 **首頁**頁面上，按一下其中一個選項，以回答問題。

    ![另一個問題的回答](real-time-web-applications-with-signalr/_static/image13.png "回答其他問題")

    *回答其他問題*
4. 按一下其中一個按鈕之後, 應該會出現答案。 請注意頁面上的統計資料資訊會自動更新後回答的問題，以更新的資訊，而不需要重新整理整個頁面。

    ![回應後的統計資料 頁面重新整理](real-time-web-applications-with-signalr/_static/image14.png "回應之後重新整理統計資料頁面")

    *統計資料重新整理之後回應 頁面*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>練習 2:向外延展使用 SQL Server

當調整 web 應用程式，您通常可以選擇之間*向上*並*相應放大*選項。 *相應增加*表示使用更大的伺服器，使用更多資源 （CPU、 RAM 等），同時*相應放大*表示新增更多伺服器來處理負載。 後者的問題在於用戶端可以取得路由傳送至不同的伺服器。 連線到一部伺服器的用戶端不會接收從另一部伺服器傳送的訊息。

您可以使用稱為的元件，以解決這些問題*後擋板*、 轉送給伺服器之間的訊息。 後擋板，已啟用，與每個應用程式執行個體傳送訊息至後擋板，並後擋板將它們轉送到其他應用程式執行個體。

目前有三種類型的 SignalR 的背板：

- **Windows Azure 服務匯流排**。 服務匯流排是可讓元件，以將鬆散結合的訊息傳送的訊息基礎結構。
- **SQL Server**。 SQL Server 後擋板會將訊息寫入 SQL 資料表。 後擋板會使用 Service Broker 有效率之傳訊。 不過，它也適用於如果未啟用 Service Broker。
- **Redis**。 Redis 是記憶體中索引鍵-值存放區。 Redis 支援發行/訂閱 ("pub/sub") 的模式來傳送訊息。

每個訊息會透過訊息匯流排傳送。 訊息匯流排實作[IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)介面，可提供發佈/訂閱抽象概念。 藉由取代預設的背板運作**IMessageBus**與專為該後擋板匯流排。

每個伺服器執行個體連線到透過匯流排後擋板。 當訊息傳送時，會先移至後擋板，並後擋板將它傳送至每一部伺服器。 當伺服器收到一則訊息，從後擋板時，它會在其本機快取中儲存訊息。 伺服器再將訊息傳遞至用戶端從其本機快取。

如需有關 SignalR 後擋板的運作方式，請閱讀本[文章](../performance/scaleout-in-signalr.md)。

> [!NOTE]
> 有一些案例，其中的後擋板可能成為瓶頸。 以下是一些典型的 SignalR 案例：
> 
> - [伺服器廣播](tutorial-server-broadcast-with-signalr.md)（例如，股票行情指示器）：背板適用於此案例中，因為伺服器控制傳送訊息的速率。
> - [用戶端到用戶端](tutorial-getting-started-with-signalr.md)（例如聊天）：在此案例中後, 擋板如果可能會發生瓶頸的訊息數目隨著; 的用戶端數目也就是說，如果訊息的速率成長按比例越多的用戶端加入。
> - [高頻率即時](tutorial-high-frequency-realtime-with-signalr.md)（例如，即時遊戲）：後擋板不建議此案例中。


在這個練習中，您將使用**SQL Server**散發郵件，跨越**Geek 測驗**應用程式。 您將這些工作的電腦上執行單一測試以了解如何設定組態，但是為了取得完整的效果，您必須部署兩個或多部伺服器的 SignalR 應用程式。 其中一部伺服器，或個別的專用伺服器上，您也必須安裝 SQL Server。

![向外延展您可以使用 SQL Server 的圖表](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>工作 1-了解案例

在這個工作中，您將執行 2 個執行個體**Geek 測驗**本機電腦上模擬多個 IIS 執行個體。 在此案例中，回答某一個應用程式邏輯問題時，更新將不會收到通知的第二個執行個體的統計資料 頁面上。 這個模擬類似於在多個執行個體部署您的應用程式所在的環境和使用負載平衡器與他們溝通。

1. 開啟**Begin.sln**解決方案位於**來源/Ex2-ScalingOutWithSQLServer/開始**資料夾。 載入之後，您會注意到在**伺服器總管**解決方案有兩個專案，具有相同結構但不同的名稱。 這會模擬您的本機電腦上執行相同的應用程式的兩個執行個體。

    ![開始模擬 Geek 測驗的 2 個執行個體的解決方案](real-time-web-applications-with-signalr/_static/image16.png "開始模擬 Geek 測驗的 2 個執行個體的解決方案")

    *開始模擬 Geek 測驗的 2 個執行個體的解決方案*
2. 開啟方案的 [屬性] 頁面，以滑鼠右鍵按一下方案節點，然後選取**屬性**。 底下**啟始專案**，選取**多個啟始專案**並變更**動作**這兩個專案到值*啟動*。

    ![啟動多個專案](real-time-web-applications-with-signalr/_static/image17.png "啟動多個專案")

    *啟動多個專案*
3. 按下**F5**執行方案。 應用程式將會啟動兩個執行個體**Geek 測驗**在不同的連接埠，來模擬多個相同的應用程式執行個體。 在左邊和右邊的螢幕上其他將其中一個瀏覽器釘選。 您的認證登入或註冊新的使用者。 登入之後，保留在左側的邏輯頁面，並移至**統計資料**右邊的瀏覽器頁面。

    ![玩家測驗並排顯示](real-time-web-applications-with-signalr/_static/image18.png)

    *玩家測驗並排顯示*

    ![在不同的連接埠的玩家測驗](real-time-web-applications-with-signalr/_static/image19.png)

    *在不同的連接埠的玩家測驗*
4. 左側的瀏覽器中啟動的問題，您將會發現**統計資料**未更新的右邊的瀏覽器頁面。 這是因為**SignalR**使用本機快取將訊息散發到其用戶端和此案例中模擬多個執行個體，因此快取不會共用它們之間。 您可以確認**SignalR**測試相同的步驟，但使用單一的應用程式正常運作。 在下列工作中，您將設定的後擋板以複寫執行個體之間的訊息。
5. 返回 Visual Studio，並停止偵錯。

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>工作 2-建立 SQL Server 後擋板

在這個工作中，您將建立的資料庫，將做為的後擋板**Geek 測驗**應用程式。 您將使用**SQL Server 物件總管**瀏覽您的伺服器，並初始化資料庫。 此外，您將會啟用**Service Broker**。

1. 在  **Visual Studio**，開啟功能表**檢視**，然後選取**SQL Server 物件總管**。
2. 以滑鼠右鍵按一下連接到您的 LocalDB 執行個體**SQL Server**節點，然後選取**加入 SQL Server...** 選項。

    ![加入 SQL Server 執行個體](real-time-web-applications-with-signalr/_static/image20.png "加入 SQL Server 執行個體")

    *加入 SQL Server 物件總管 中的 SQL Server 執行個體*
3. 設定**伺服器名稱**要 *(localdb) \v11.0* ，並將**Windows 驗證**當做驗證模式。 按一下  **Connect**以繼續。

    ![連接到 LocalDB](real-time-web-applications-with-signalr/_static/image21.png "連接到 LocalDB")

    *連接到 LocalDB*
4. 現在，您會連接到 LocalDB 執行個體，您必須建立將代表 signalr 的 SQL Server 後擋板的資料庫。 若要這樣做，請以滑鼠右鍵按一下**資料庫**節點，然後選取**加入新的資料庫**。

    ![加入新的資料庫](real-time-web-applications-with-signalr/_static/image22.png "中加入新的資料庫")

    *加入新的資料庫*
5. 若要設定的資料庫名稱*SignalR* ，按一下  **確定**來建立它。

    ![建立 SignalR 資料庫](real-time-web-applications-with-signalr/_static/image23.png "建立 SignalR 資料庫")

    *建立 SignalR 資料庫*

    > [!NOTE]
    > 您可以選擇任何資料庫的名稱。
6. 若要從後擋板，更有效率地接收更新，建議啟用 Service Broker 資料庫。 Service Broker 提供傳訊和佇列在 SQL Server 原生支援。 後擋板也適用於沒有 Service Broker。 開啟新的查詢，以滑鼠右鍵按一下資料庫，然後選取**新的查詢**。

    ![開啟新的查詢](real-time-web-applications-with-signalr/_static/image24.png "開啟新的查詢")

    *開啟新的查詢*
7. 若要檢查是否啟用 Service Broker 時，請查詢**已\_broker\_啟用**中的資料行**sys.databases**目錄檢視。 在最近開啟的查詢視窗中執行下列指令碼。

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![查詢服務訊息代理程式狀態](real-time-web-applications-with-signalr/_static/image25.png "查詢服務訊息代理程式狀態")

    *查詢服務訊息代理程式狀態*
8. 如果值**已\_broker\_啟用**資料庫中的資料行是&quot;0&quot;，使用下列命令來啟用它。 取代**&lt;您資料庫&gt;** 建立資料庫時，您設定的名稱 (例如：SignalR)。

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![啟用 Service Broker](real-time-web-applications-with-signalr/_static/image26.png "啟用 Service Broker")

    *啟用 Service Broker*

    > [!NOTE]
    > 如果此查詢似乎發生死結，請確定沒有連線到資料庫的應用程式。

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>工作 3-設定 SignalR 應用程式

在這個工作中，您會設定**Geek 測驗**連接到 SQL Server 後擋板。 您會先新增**SignalR.SqlServer** NuGet 套件並設定連線到您的後擋板資料庫的字串。

1. 開啟**Package Manager Console**從**工具** > **NuGet 套件管理員**。 請確定**GeekQuiz**中選取專案**預設專案**下拉式清單。 輸入下列命令以安裝**Microsoft.AspNet.SignalR.SqlServer** NuGet 套件。

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. 針對專案重複上一個步驟，但這次**GeekQuiz2**。
3. 若要設定 SQL Server 後擋板，開啟**Startup.cs**的檔案**GeekQuiz**專案，並新增下列程式碼**設定**方法。 取代**&lt;您資料庫&gt;** 與您建立 SQL Server 後擋板時使用的資料庫名稱。 重複這個步驟**GeekQuiz2**專案。

    (程式碼片段- *RealTimeSignalR-Ex2-StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. 現在，這兩個專案都設定為使用 SQL Server 後擋板，按**F5**同時執行它們。
5. 同樣地， **Visual Studio**將會啟動兩個執行個體**Geek 測驗**在不同的連接埠。 將其中一個瀏覽器釘選在左側及右側螢幕上的其他和您的認證登入。 保留在左側的邏輯頁面，並移至**統計資料**pagein 右邊的瀏覽器。
6. 開始回答問題，在左邊的瀏覽器中。 此時**統計資料**感謝後擋板更新頁面。 應用程式之間切換 (**統計資料**現在位於左邊，和**邏輯**位於右邊)，並重複進行測試以驗證它是否運作的兩個執行個體。 後擋板做*共用快取*訊息的每個連接的伺服器，且每個伺服器會將訊息儲存在他們自己的本機快取，以散發給連線的用戶端。
7. 返回 Visual Studio，並停止偵錯。
8. SQL Server 後擋板元件會自動產生所需的資料表上指定的資料庫。 在 [ **SQL Server 物件總管**] 面板中，開啟您建立的後擋板的資料庫 (例如：SignalR) 展開一個資料表。 您應該會看到下列資料表：

    ![後擋板產生資料表](real-time-web-applications-with-signalr/_static/image27.png)

    *後擋板產生資料表*
9. 以滑鼠右鍵按一下**SignalR.Messages\_0**資料表，然後選取**檢視資料**。

    ![檢視 SignalR 後擋板訊息資料表](real-time-web-applications-with-signalr/_static/image28.png)

    *檢視 SignalR 後擋板訊息資料表*
10. 您可以看到不同的訊息傳送至**中樞**時回答邏輯問題。 後擋板會將這些訊息，任何連接的執行個體。

    ![後擋板訊息資料表](real-time-web-applications-with-signalr/_static/image29.png)

    *後擋板訊息資料表*

* * *

<a id="Summary"></a>
## <a name="summary"></a>總結

在這個實際操作實驗室中，您已了解如何新增**SignalR**您的應用程式和傳送通知從伺服器以使用您已連線的用戶端**中樞**。 此外，您已了解如何透過使用相應放大您的應用程式*後擋板*元件多個 IIS 執行個體中部署您的應用程式時。
