---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: "實習： 與 SignalR 即時 Web 應用程式 |Microsoft 文件"
author: rick-anderson
description: "即時 Web 應用程式功能的伺服器端將內容推至連線的用戶端時，即時的能力。 ASP.NET 開發人員，ASP..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 96d3b8b82f78d8f6da85012aac8a1411cf297e26
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>與 SignalR 實習： 即時 Web 應用程式
====================
由[Web 營小組](https://twitter.com/webcamps)

[下載 Web 營訓練套件](http://aka.ms/webcamps-training-kit)

> 即時 Web 應用程式功能的伺服器端將內容推至連線的用戶端時，即時的能力。 ASP.NET 開發人員， **ASP.NET SignalR**是將即時 web 功能新增至其應用程式的程式庫。 它會利用數種傳輸，會自動選取最佳可用的傳輸用戶端和伺服器的最佳可用的傳輸。 它會利用**WebSocket**，HTML5 API，可讓瀏覽器和伺服器之間的雙向通訊。
> 
> **SignalR**也提供簡單、 高層級 API，以進行用戶端 RPC 伺服器 （在用戶端的瀏覽器中從伺服器端.NET 程式碼會呼叫 JavaScript 函式） 中 ASP.NET 應用程式，以及新增有用的攔截程序，用於連接管理例如連接/中斷連接事件、 分組連線及授權。
> 
> **SignalR**部分所使用的傳輸所需執行用戶端與伺服器之間的即時工作是一個抽象概念。 A **SignalR**連接啟動為 HTTP，並再提升為**WebSocket**連接，如果有的話。 **WebSocket**是理想的傳輸**SignalR**，因為它會有效率地使用伺服器記憶體的最低的延遲，而且具有最基本的功能 (例如完整雙工用戶端之間的通訊和伺服器），但它也會有最嚴苛的需求： **WebSocket**需要使用伺服器**Windows Server 2012**或**Windows 8**，連同**.NET framework 4.5**。 如果不符合這些需求， **SignalR**會嘗試使用其他傳輸進行其連線 (例如*Ajax 長期輪詢*)。
> 
> **SignalR** API 包含用戶端和伺服器之間進行通訊的兩個模型：**持續連線**和**集線器**。 A**連接**代表簡單端點傳送單一收件者，分組，或廣播訊息。 A**中樞**更高層級管線基礎連接 API，可讓您的用戶端與伺服器彼此直接呼叫方法。
> 
> ![SignalR 架構](real-time-web-applications-with-signalr/_static/image1.png)
> 
> 所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)。


<a id="Overview"></a>
## <a name="overview"></a>概觀

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將學會如何：

- 使用 SignalR 用戶端，從伺服器傳送通知。
- 向外延展 SignalR 應用程式使用**SQL Server**。

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

需要下列項目才能完成這個實際操作實驗室：

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更新版本

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

若要執行這個實際操作實驗室練習，您必須先設定您的環境。

1. 開啟 Windows 檔案總管 視窗並瀏覽至實驗室**來源**資料夾。
2. 以滑鼠右鍵按一下**Setup.cmd**選取**系統管理員身分執行**啟動安裝程序，將會設定您的環境，並安裝這個實驗室的 Visual Studio 程式碼片段。
3. 如果顯示 [使用者帳戶控制] 對話方塊，確認要繼續進行的動作。

> [!NOTE]
> 請確定執行安裝程式之前已在本實驗室的所有相依性。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>使用程式碼片段

在實驗室文件，您會指示要插入的程式碼區塊。 為了方便起見，大部分的這段程式碼是依現狀 Visual Studio 程式碼片段，您可以從 Visual Studio 2013，不必以手動方式新增內存取。

> [!NOTE]
> 每個練習會伴隨起始方案位於**開始**練習，可讓您依照每個練習各自的資料夾。 請注意練習期間加入的程式碼片段遺失從中啟動方案，並可能無法運作，直到您已經完成本練習。 內部練習的原始程式碼，您也可以找到**結束**資料夾包含在 Visual Studio 方案中使用的程式碼所產生對應的練習中的步驟。 如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案做為指導。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室包含下列練習：

1. [使用 SignalR 的即時資料搭配使用](#Exercise1)
2. [向外延展使用 SQL Server](#Exercise2)

完成本實驗室估計時間： **60 分鐘的時間**

> [!NOTE]
> 當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。 每個預先定義的集合設計為比對特定開發樣式，並判斷視窗配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。 在這個實驗室的程序說明完成指定的工作，Visual Studio 中使用時所需的動作**一般開發設定**集合。 如果您選擇不同的設定集合的開發環境，可能是您應該考慮的步驟中的差異。


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>練習 1： 使用使用 SignalR 的即時資料

雖然交談通常會使用做為範例中，您可以執行整個更多即時 Web 功能。 每當使用者重新整理網頁即可看到新的資料或頁面實作 Ajax 長期輪詢來擷取新的資料，您可以使用 SignalR。

支援 SignalR**伺服器發送**或**廣播**功能; 它會自動處理連接管理。 在 傳統 HTTP 連線用戶端與伺服器通訊的每個要求，重新建立連接時，但 SignalR 提供用戶端與伺服器之間的持續連線。 SignalR 的伺服器程式碼呼叫的用戶端程式碼中使用遠端程序呼叫 (RPC) 的瀏覽器，而不是要求-回應模型我們知道在今天。

在此練習中，您將設定**玩家測驗**用來顯示包含更新的度量，而不需要重新整理整個頁面的統計資料儀表板的 SignalR 應用程式。

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>工作 1 – 瀏覽一些測驗統計資料頁面

在這項工作，您會瀏覽應用程式，並確認統計資料 頁面會顯示如何以及如何改善資訊的方式。

1. 開啟**Visual Studio Express 2013 for Web**並開啟**GeekQuiz.sln**方案位於**Source\Ex1 WorkingWithRealTimeData\Begin**資料夾。
2. 按**F5**執行解決方案。 **登入**頁面應該會出現在瀏覽器中。

    ![執行解決方案](real-time-web-applications-with-signalr/_static/image2.png "執行解決方案")

    *執行解決方案*
3. 按一下**註冊**頁面以建立新的使用者在應用程式右上角。

    ![註冊連結](real-time-web-applications-with-signalr/_static/image3.png "註冊連結")

    *註冊連結*
4. 在**註冊**頁面上，輸入**使用者名**和**密碼**，然後按一下 **註冊**。

    ![註冊使用者](real-time-web-applications-with-signalr/_static/image4.png "註冊使用者")

    *註冊使用者*
5. 應用程式註冊新帳戶，並驗證使用者，並且重新導向至首頁上顯示的第一個測驗問題。
6. 開啟**統計資料**頁面上，在新視窗中，並放**首頁**頁面和**統計資料**頁面上的並行。

    ![並存 windows](real-time-web-applications-with-signalr/_static/image5.png "側邊 windows")

    *並存的 windows*
7. 在**首頁**頁面上，按一下其中一個選項回答的問題。

    ![回答](real-time-web-applications-with-signalr/_static/image6.png "回答的問題。")

    *回答問題。*
8. 按一下其中一個按鈕之後, 應該會出現答案。

    ![正確回答的問題](real-time-web-applications-with-signalr/_static/image7.png "正確回答的問題")

    *正確解答的問題*
9. 請注意統計資料 頁面中提供的資訊已過期。 重新整理頁面，以查看更新的結果。

    ![統計資料 頁面](real-time-web-applications-with-signalr/_static/image8.png "統計資料 頁面")

    *統計資料 頁面*
10. 返回 Visual Studio，並停止偵錯。

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>工作 2-加入至顯示線上圖表玩家測驗 SignalR

在這項工作，您將 SignalR 加入至方案，並將更新傳送至用戶端自動當新的回應傳送到伺服器。

1. 從**工具**在 Visual Studio 中，選取功能表**程式庫套件管理員**，然後按一下  **Package Manager Console**。
2. 在**Package Manager Console**視窗中，執行下列命令：

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![SignalR 套件安裝](real-time-web-applications-with-signalr/_static/image9.png "SignalR 套件安裝")

    *SignalR 套件安裝*

    > [!NOTE]
    > 安裝時**SignalR** NuGet 封裝版本 2.0.2 從全新的 MVC 5 應用程式，您必須手動更新**OWIN** 2.0.1 版的封裝 （或更新版本） 安裝 SignalR 之前。 若要這樣做，您可以執行下列指令碼中的**Package Manager Console**:
    > 
    > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
    > 
    > 在 SignalR 的未來版本中，將會自動更新 OWIN 相依性。
3. 在**方案總管 中**，依序展開**指令碼**資料夾，請注意，SignalR *js*檔案加入至方案。

    ![SignalR JavaScript 參考](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript 參考")

    *SignalR JavaScript 參考*
4. 在**方案總管 中**，以滑鼠右鍵按一下**GeekQuiz**專案，然後選取**新增** | **新資料夾**，並將它命名**集線器**。
5. 以滑鼠右鍵按一下**集線器**資料夾，然後選取**新增 |新項目**。

    ![加入新項目](real-time-web-applications-with-signalr/_static/image11.png "加入新項目")

    *加入新項目*
6. 在**加入新項目**對話方塊中，選取**Visual C# |Web |SignalR**的左窗格中，選取的節點**SignalR 中樞類別 (v2)**從中間窗格中，將檔案命名**StatisticsHub.cs**按一下**新增**。

    ![加入新項目對話方塊](real-time-web-applications-with-signalr/_static/image12.png "加入新項目對話方塊")

    *加入新項目對話方塊*
7. 中的程式碼取代**StatisticsHub**為下列程式碼的類別。

    (程式碼片段- *RealTimeSignalR-Ex1-StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. 開啟**Startup.cs**和結尾加入下列行**組態**方法。

    (程式碼片段- *RealTimeSignalR-Ex1-MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. 開啟**StatisticsService.cs**頁面內**服務**資料夾並加入下列 using 指示詞。

    (程式碼片段- *RealTimeSignalR-Ex1-UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. 若要通知連線的用戶端的更新，請您第一次擷取**內容**目前連接的物件。 **中樞**物件包含方法，將訊息傳送至單一用戶端或廣播到所有已連線的用戶端。 將下列方法加入**StatisticsService**廣播統計資料的類別。

    (程式碼片段- *RealTimeSignalR-Ex1-NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > 上述程式碼，您正在使用任意的方法名稱，用戶端上呼叫的函式 (也就是： *updateStatistics*)。 您指定的方法名稱會解譯為動態的物件，表示沒有任何 IntelliSense 或它的編譯時間驗證。 在執行階段會評估運算式。 當執行方法呼叫時，SignalR 就會傳送給用戶端的方法名稱和參數值。 如果用戶端具有相符名稱的方法，會呼叫該方法，且參數值會傳遞給它。 如果用戶端上找到沒有對應的方法，就不會引發錯誤。 如需詳細資訊，請參閱[ASP.NET SignalR 中樞 API 指南](../guide-to-the-api/hubs-api-guide-server.md)。
11. 開啟**TriviaController.cs**頁面內**控制器**資料夾並加入下列 using 指示詞。

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. 將下列反白顯示的程式碼加入**Post**動作方法。

    (程式碼片段- *RealTimeSignalR-Ex1-NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. 開啟**Statistics.cshtml**頁面內**檢視 |首頁**資料夾。 找出**指令碼**區段，區段的開頭加入下列指令碼參考。

    (程式碼片段- *RealTimeSignalR-Ex1-SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > 當您將 SignalR 和其他指令碼程式庫加入您的 Visual Studio 專案時，封裝管理員可能會安裝的版本比本主題所顯示的版本還新的 SignalR 指令碼檔案。 請確定您的程式碼中的指令碼參考符合安裝在您的專案中的指令碼程式庫的版本。
14. 加入下列反白顯示的程式碼，來連接到 SignalR 中樞的用戶端從中樞接收新訊息時，更新統計資料。

    (程式碼片段- *RealTimeSignalR-Ex1-SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    在此程式碼中，您建立的中樞 Proxy 和註冊事件處理常式來接聽伺服器所傳送的訊息。 您在此情況下，接聽訊息透過傳送*updateStatistics*方法。

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>工作 3-執行解決方案

在這個工作中，您將執行方案來驗證新問題要回答之後使用 SignalR 自動更新統計資料檢視。

1. 按**F5**執行解決方案。

    > [!NOTE]
    > 如果尚未登入應用程式，登入您在工作 1 中建立的使用者。
2. 開啟**統計資料**頁面上，在新視窗中，並放**首頁**頁面和**統計資料**頁面來並行工作 1 中一樣。
3. 在**首頁**頁面上，按一下其中一個選項回答的問題。

    ![另一個問題要回答](real-time-web-applications-with-signalr/_static/image13.png "回答另一個問題。")

    *另一個問題要回答*
4. 按一下其中一個按鈕之後, 應該會出現答案。 請注意頁面上的統計資料資訊會自動更新後回答的問題。 更新的資訊，而不需要重新整理整個頁面。

    ![統計資料 頁面重新整理回應之後](real-time-web-applications-with-signalr/_static/image14.png "回應之後重新整理統計資料頁面")

    *統計資料重新整理之後回應 頁面*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>練習 2： 向外延展使用 SQL Server

當調整 web 應用程式，您通常可以選擇之間*向上擴充*和*向外延展*選項。 *向上延展*表示較多資源 （CPU、 RAM 等），同時使用更大的伺服器，*向外延展*表示加入更多伺服器來處理負載。 後者的問題在於，用戶端可以取得路由傳送到不同的伺服器。 連接到一部伺服器的用戶端不會接收從另一部伺服器傳送的訊息。

您可以使用名為的元件，以解決這些問題*後擋板*、 將伺服器之間的訊息轉送。 後擋板，已啟用，與每個應用程式執行個體將訊息傳送至後擋板，並後擋板，已轉送至其他應用程式執行個體。

目前有三種類型的 SignalR 的背板：

- **Windows Azure 服務匯流排**。 Service Bus 是訊息的基礎結構可讓鬆散偶合的訊息傳送的元件。
- **SQL Server**。 SQL Server 後擋板訊息寫入 SQL 資料表。 後擋板有效率的通訊使用 Service Broker。 不過，它也適用於未啟用 Service Broker。
- **Redis**。 Redis 是記憶體中索引鍵-值存放區。 Redis 支援發行/訂閱 (「 pub/sub") 的模式來傳送訊息。

每個訊息是透過訊息匯流排傳送。 訊息匯流排實作[IMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)介面，可提供發佈/訂閱抽象的。 運作方式是取代預設的背板**IMessageBus**與針對該後擋板匯流排。

每個伺服器執行個體連接到後擋板，已透過匯流排。 當訊息傳送時，它會移至後擋板，並後擋板，已將它傳送至每一部伺服器。 當伺服器從後擋板接收訊息時，它會在其本機快取中儲存訊息。 伺服器再將訊息傳遞至用戶端從其本機快取。

如需有關 SignalR 後擋板的運作方式，請閱讀本[文章](../performance/scaleout-in-signalr.md)。

> [!NOTE]
> 有一些案例後擋板可能會變成瓶頸。 以下是一些典型的 SignalR 案例：
> 
> - [伺服器廣播](tutorial-server-broadcast-with-signalr.md)（例如，股票行情指示器）： 背板很適合此案例中，因為伺服器控制傳送訊息的速率。
> - [用戶端](tutorial-getting-started-with-signalr.md)（例如聊天）： 在此案例中後, 擋板可能是效能瓶頸如果用戶端數目的訊息數目縮放比例; 也就是說，如果訊息的速率增加按比例越多的用戶端加入。
> - [高頻率即時](tutorial-high-frequency-realtime-with-signalr.md)（例如，即時遊戲）： 後擋板建議您不要在此案例。


在此練習中，您將使用**SQL Server**發佈訊息**玩家測驗**應用程式。 您要了解如何設定組態，但若要取得完整的效果的單一測試機器上執行這些工作，您將需要部署兩個或多部伺服器的 SignalR 應用程式。 您也必須在一部伺服器上或不同的專用伺服器上安裝 SQL Server。

![向外延展使用 SQL Server 的圖表](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>工作 1-了解案例

在這個工作中，您將執行 2 個執行個體的**玩家測驗**模擬多個 IIS 在本機電腦上執行個體。 在此案例中，回應瑣事某一個應用程式的問題時，更新將不會在統計資料 頁面上的第二個執行個體收到通知。 這個模擬類似於您的應用程式部署在多個執行個體所在的環境和使用負載平衡器與它們通訊。

1. 開啟**Begin.sln**方案位於**來源/Ex2 ScalingOutWithSQLServer/開始**資料夾。 載入之後，您會注意到**伺服器總管**方案中有兩個專案具有相同結構但不同的名稱。 這會模擬在本機電腦上執行相同的應用程式的兩個執行個體。

    ![開始模擬玩家測驗的 2 個執行個體的方案](real-time-web-applications-with-signalr/_static/image16.png "開始方案模擬玩家測驗的 2 個執行個體")

    *開始模擬玩家測驗的 2 個執行個體的方案*
2. 開啟方案的屬性頁面，以滑鼠右鍵按一下方案節點，然後選取**屬性**。 下**啟始專案**，選取**多個啟始專案**並變更**動作**值這兩個專案到*啟動*。

    ![啟動多個專案](real-time-web-applications-with-signalr/_static/image17.png "啟動多個專案")

    *啟動多個專案*
3. 按**F5**執行解決方案。 應用程式會啟動兩個執行個體**玩家測驗**在不同的連接埠，並模擬多個相同的應用程式執行個體。 在左方和其他螢幕的右側釘選的瀏覽器上。 您的認證登入或註冊新的使用者。 登入之後，保留瑣事頁面左側，並移至**統計資料**右邊的瀏覽器中的頁面。

    ![玩家測驗並存](real-time-web-applications-with-signalr/_static/image18.png)

    *玩家測驗並存*

    ![在不同的連接埠的玩家測驗](real-time-web-applications-with-signalr/_static/image19.png)

    *在不同的連接埠的玩家測驗*
4. 在左邊的瀏覽器中啟動回應問題，您會注意到**統計資料**右邊的瀏覽器頁面仍未更新。 這是因為**SignalR**使用本機快取可以分散其用戶端的訊息和此案例會模擬多個執行個體，因此快取不它們之間會共用。 您可以確認**SignalR**測試相同的步驟，但是使用單一的應用程式是否正常運作。 在下列工作中，您將設定複寫執行個體之間的訊息後擋板。
5. 返回 Visual Studio，並停止偵錯。

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>工作 2 – 建立 SQL Server 後擋板

在這項工作，您將建立的資料庫，將做為的後擋板**玩家測驗**應用程式。 您將使用**SQL Server 物件總管**瀏覽您的伺服器，並初始化資料庫。 此外，您將會啟用**Service Broker**。

1. 在**Visual Studio**，開啟功能表**檢視**選取**SQL Server 物件總管**。
2. 連接到 LocalDB 執行個體，以滑鼠右鍵按一下**SQL Server**節點，然後選取**加入 SQL Server...**選項。

    ![加入 SQL Server 執行個體](real-time-web-applications-with-signalr/_static/image20.png "加入 SQL Server 執行個體")

    *將 SQL Server 執行個體加入至 SQL Server 物件總管*
3. 設定**伺服器名稱**至*(localdb) \v11.0*留下**Windows 驗證**當做驗證模式。 按一下**連接**才能繼續。

    ![連接到 LocalDB](real-time-web-applications-with-signalr/_static/image21.png "連接到 LocalDB")

    *連接到 LocalDB*
4. 現在，您會連接到 LocalDB 執行個體，您必須建立用來表示 SQL Server 後擋板 SignalR 的資料庫。 若要這樣做，請以滑鼠右鍵按一下**資料庫**節點，然後選取**加入新的資料庫**。

    ![將新的資料庫](real-time-web-applications-with-signalr/_static/image22.png "加入新的資料庫")

    *將新的資料庫*
5. 資料庫名稱設定為*SignalR*按一下**確定**加以建立。

    ![建立 SignalR 資料庫](real-time-web-applications-with-signalr/_static/image23.png "建立 SignalR 資料庫")

    *建立 SignalR 資料庫*

    > [!NOTE]
    > 您可以選擇任何資料庫的名稱。
6. 若要從後擋板更有效率地接收更新，建議啟用 Service Broker 資料庫。 Service Broker 提供傳訊和佇列在 SQL Server 中的原生支援。 後擋板也就不會 Service Broker。 開啟新的查詢，以滑鼠右鍵按一下資料庫，然後選取**新查詢**。

    ![開啟新查詢](real-time-web-applications-with-signalr/_static/image24.png "開啟新查詢")

    *開啟新查詢*
7. 若要檢查是否啟用 Service Broker，查詢**是\_broker\_啟用**中的資料行**sys.databases**目錄檢視。 在最近開啟的查詢視窗中執行下列指令碼。

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![查詢 Service Broker 狀態](real-time-web-applications-with-signalr/_static/image25.png "查詢服務 Broker 狀態")

    *查詢 Service Broker 狀態*
8. 如果值**是\_broker\_啟用**資料庫中的資料行是&quot;0&quot;，使用下列命令來啟用它。 取代**&lt;您資料庫&gt;**建立資料庫時，您設定的名稱 (例如： SignalR)。

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![啟用 Service Broker](real-time-web-applications-with-signalr/_static/image26.png "啟用 Service Broker")

    *啟用 Service Broker*

    > [!NOTE]
    > 如果此查詢似乎發生死結，請確定沒有應用程式連接到資料庫。

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>工作 3-設定 SignalR 應用程式

在這個工作中，您將設定**玩家測驗**連接到 SQL Server 後擋板。 您會先加入**SignalR.SqlServer** NuGet 套件和設定的連接字串至後擋板資料庫。

1. 開啟**Package Manager Console**從**工具** | **程式庫套件管理員**。 請確定**GeekQuiz**中選取專案**預設專案**下拉式清單。 輸入下列命令以安裝**Microsoft.AspNet.SignalR.SqlServer** NuGet 封裝。

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. 重複上一個步驟，但這次專案**GeekQuiz2**。
3. 若要設定 SQL Server 後擋板，開啟**Startup.cs**檔案**GeekQuiz**專案，並將下列程式碼加入**設定**方法。 取代**&lt;您資料庫&gt;**與您建立 SQL Server 後擋板時所使用的資料庫名稱。 重複這個步驟**GeekQuiz2**專案。

    (程式碼片段- *RealTimeSignalR-Ex2-StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. 現在，這兩個專案都設定為使用 SQL Server 後擋板，按**F5**同時執行它們。
5. 同樣地， **Visual Studio**將會啟動兩個執行個體**玩家測驗**在不同的連接埠。 釘選的瀏覽器，在左方和其他螢幕的右側，並登入您的認證。 保留瑣事頁面左側，並移至**統計資料**pagein 右邊的瀏覽器。
6. 啟動回答左邊的瀏覽器中的問題。 這次**統計資料**這點受惠後擋板，已更新頁面。 應用程式之間切換 (**統計資料**現在位於左邊，和**瑣事**位於右邊)，並重複測試，以驗證它是否運作的兩個執行個體。 後擋板做為*共用快取*訊息的每個連接的伺服器，且每個伺服器會將訊息儲存在他們自己的本機快取，以發佈至連線的用戶端。
7. 返回 Visual Studio，並停止偵錯。
8. SQL Server 後擋板元件會自動產生所需的資料表上指定的資料庫。 在**SQL Server 物件總管** 面板中，開啟您的後擋板，已建立的資料庫 (例如： SignalR) 並展開其資料表。 您應該會看到下列資料表：

    ![後擋板，已產生的資料表](real-time-web-applications-with-signalr/_static/image27.png)

    *後擋板，已產生的資料表*
9. 以滑鼠右鍵按一下**SignalR.Messages\_0**資料表，然後選取**檢視資料**。

    ![檢視 SignalR 後擋板訊息資料表](real-time-web-applications-with-signalr/_static/image28.png)

    *檢視 SignalR 後擋板訊息資料表*
10. 您可以看到不同的訊息傳送至**中樞**回答瑣事問題時。 後擋板會將這些訊息，任何連接的執行個體。

    ![後擋板訊息資料表](real-time-web-applications-with-signalr/_static/image29.png)

    *後擋板訊息資料表*

* * *

<a id="Summary"></a>
## <a name="summary"></a>總結

在這個實際操作實驗室中，您已經學會如何新增**SignalR**您應用程式，並將傳送通知，從伺服器以使用您已連線的用戶端**集線器**。 此外，您學會了如何以擴充您的應用程式使用*後擋板*元件時，您的應用程式部署在多個 IIS 執行個體。
