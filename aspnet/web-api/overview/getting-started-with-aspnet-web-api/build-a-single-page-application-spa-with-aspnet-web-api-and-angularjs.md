---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: "實習： 建置含有 ASP.NET Web API 和 Angular.js 的單一頁面應用程式 (SPA) |Microsoft 文件"
author: rick-anderson
description: "傳統 web 應用程式中，用戶端 （瀏覽器） 會啟始與伺服器通訊，方式是要求的頁面。 然後伺服器會處理要求..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2015
ms.topic: article
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 9a748628d53878be380869ac5327de0111d2284d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>實習： 建置含有 ASP.NET Web API 和 Angular.js 的單一頁面應用程式 (SPA)
====================
由[Web 營小組](https://twitter.com/webcamps)

[下載 Web 營訓練套件](http://aka.ms/webcamps-training-kit)

> 傳統 web 應用程式中，用戶端 （瀏覽器） 會啟始與伺服器通訊，方式是要求的頁面。 伺服器處理要求，然後將頁面的 HTML 傳送至用戶端。 在後續的互動 （例如使用者巡覽至連結或送出的表單具有資料） 頁面與新的要求會傳送到伺服器，並再次啟動流程： 伺服器處理要求，並將新的頁面傳送至新的動作要求的回應中的瀏覽器ed 用戶端。
> 
> 在單一頁面應用程式 (SPAs) 整個網頁瀏覽器中之後載入初始的要求，但後續的互動透過 Ajax 要求。 這表示瀏覽器具有更新的頁面已變更; 部份沒有需要重新載入整個頁面。 SPA 方法可減少應用程式，以回應使用者動作，導致更流暢的經驗所花費的時間。
> 
> SPA 架構牽涉到某些不會出現在傳統 web 應用程式的挑戰。 不過，新的 ASP.NET Web API 這類技術，JavaScript 架構 AngularJS 等 CSS3 所提供的新樣式功能讓您設計和建置 SPAs 真正輕鬆。
> 
> 在此手上實驗室中，您會利用這些技術來實作玩家測驗 」，瑣事網站 SPA 概念為基礎。 您先將實作與 ASP.NET Web API 來擷取測驗問題，並儲存答案的必要的端點公開 （expose） 服務層。 然後，您將建立豐富且回應迅速的 UI 使用 AngularJS 和 CSS3 轉換效果。
> 
> 所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)。


## <a name="overview"></a>概觀

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將學會如何：

- 建立 ASP.NET Web API 服務，將傳送和接收 JSON 資料
- 建立使用 AngularJS 互動式 UI
- 增強使用 CSS3 轉換其 UI 使用經驗

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

需要下列項目才能完成這個實際操作實驗室：

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更新版本

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

若要執行這個實際操作實驗室練習，您必須先設定您的環境。

1. 開啟 Windows 檔案總管並瀏覽至實驗室**來源**資料夾。
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

1. [建立 Web 應用程式開發介面](#Exercise1)
2. [建立 SPA 介面](#Exercise2)

完成本實驗室估計時間： **60 分鐘的時間**

> [!NOTE]
> 當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。 每個預先定義的集合設計為比對特定開發樣式，並判斷視窗配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。 在這個實驗室的程序說明完成指定的工作，Visual Studio 中使用時所需的動作**一般開發設定**集合。 如果您選擇不同的設定集合的開發環境，可能是您應該考慮的步驟中的差異。


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>練習 1： 建立 Web 應用程式開發介面

服務層的主要部分 SPA 的其中一個。 它會負責處理 UI 和該呼叫的回應中傳回資料所傳送的 Ajax 呼叫。 擷取的資料應該會看到才能進行剖析與耗用用戶端電腦可讀取的格式。

Web API framework 是 ASP.NET 堆疊的一部分，並設計旨在讓您輕鬆地實作 HTTP 的服務，通常傳送和接收 JSON 或 XML 格式的資料，透過 rest 式 API。 在這個練習中，您將建立網站來裝載玩家受測應用程式，然後實作 後端服務公開並保存使用 ASP.NET Web API 的測驗資料。

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>工作 1 – 建立初始專案玩家測驗

在這項工作會啟動 ASP.NET Web API 架構上，建立新的 ASP.NET MVC 專案以支援**One ASP.NET**專案隨附於 Visual Studio 中的類型。 **一個 ASP.NET**統一所有 ASP.NET 技術，並提供您混搭它們所需的選項。 然後，您將加入 Entity Framework 模型類別和資料庫 initializator 插入測驗問題。

1. 開啟**Visual Studio Express 2013 for Web**選取**檔案 |新增專案...**啟動新的解決方案。

    ![建立新的專案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "建立新的專案")

    *建立新的專案*
2. 在**新專案**對話方塊中，選取**ASP.NET Web 應用程式**下**Visual C# |Web**  索引標籤。請確定**.NET Framework 4.5**是選取，其命名*GeekQuiz*，選擇**位置**按一下**確定**。

    ![建立新的 ASP.NET Web 應用程式專案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "建立新的 ASP.NET Web 應用程式專案")

    *建立新的 ASP.NET Web 應用程式專案*
3. 在**新增 ASP.NET 專案**對話方塊中，選取**MVC**範本，然後選取**Web API**選項。 另外，請確定**驗證**選項設定為**個別使用者帳戶**。 按一下 [確定]  繼續操作。

    ![這個 MVC 範本，包括 Web API 元件建立新的專案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *這個 MVC 範本，包括 Web API 元件建立新的專案*
4. 在**方案總管 中**，以滑鼠右鍵按一下**模型**資料夾**GeekQuiz**專案，然後選取**新增 |現有的項目...**.

    ![加入現有項目](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "加入現有項目")

    *加入現有項目*
5. 在**加入現有項目**對話方塊方塊中，瀏覽至**來源/資產模型**資料夾，然後選取所有檔案。 按一下 [加入] 。

    ![加入模型資產](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "加入模型資產")

    *加入模型資產*

    > [!NOTE]
    > 藉由加入這些檔案，您會加入資料模型、 Entity Framework 資料庫內容和玩家測驗應用程式的資料庫初始設定式。
    > 
    > **Entity Framework (EF)**是物件關聯式對應 (ORM)，可讓您使用而不是直接使用關聯式儲存結構描述的程式設計概念應用程式模型進行程式設計來建立資料存取應用程式。 您可以進一步了解 Entity Framework[這裡](../../../entity-framework.md)。
    > 
    > 以下是您剛才加入的類別的描述：
    > 
    > - **TriviaOption:**表示測驗問題相關的單一選項
    > - **TriviaQuestion:**代表測驗問題，並公開 （expose） 相關的選項，透過**選項**屬性
    > - **TriviaAnswer:**表示中的測驗問題回應使用者選取的選項
    > - **TriviaContext:**表示玩家測驗應用程式的 Entity Framework 資料庫內容。 此類別衍生自**DContext**公開**DbSet**代表上面所述的實體集合的屬性。
    > - **TriviaDatabaseInitializer:**的 Entity Framework 初始設定式的實作**TriviaContext**類別繼承自**CreateDatabaseIfNotExists**。 這個類別的預設行為是建立資料庫，只有當它不存在，插入項目中指定**種子**方法。
6. 開啟**Global.asax.cs**檔案，然後加入下列 using 陳述式。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. 加入下列程式碼的開頭**應用程式\_啟動**方法，以設定**TriviaDatabaseInitializer**為資料庫初始設定式。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. 修改**首頁**限制存取權的控制站驗證的使用者。 若要這樣做，請開啟**HomeController.cs**檔案內**控制器**資料夾並加入**授權**屬性**HomeController**類別定義。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > **授權**篩選檢查會驗證使用者。 如果使用者未經過驗證，它會傳回 HTTP 狀態碼 401 （未經授權），而不叫用動作。 您可以套用全域，在控制器層級，或個別動作層級的篩選條件。
9. 現在，您將自訂網頁和品牌的配置。 若要這樣做，請開啟 **\_Layout.cshtml**檔案內**檢視 |共用**資料夾和更新的內容**&lt;標題&gt;**取代的項目*我的 ASP.NET 應用程式*與*玩家測驗*.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. 在相同的檔案中，更新導覽列藉由移除*有關*和*連絡人*連結和重新命名*首頁*連結至*播放*。 此外，重新命名*應用程式名稱*連結至*玩家測驗*。 在導覽列 HTML 看起來應該類似下列程式碼。

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. 透過取代更新的版面配置頁的頁尾*我的 ASP.NET 應用程式*與*玩家測驗*。 若要這樣做，取代的內容**&lt;頁尾&gt;**具有下列反白顯示的程式碼項目。

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>工作 2 – 建立 TriviaController Web 應用程式開發介面

在先前工作中，您在建立初始結構的一些測驗 web 應用程式。 現在，您將建置簡單的 Web API 服務，與測驗資料模型互動，而且會公開下列動作：

- **GET/api/瑣事**： 從要驗證的使用者來回應的測驗清單擷取下一個問題。
- **POST/api/瑣事**： 儲存已驗證的使用者所指定的測驗解答。

您將使用 Visual Studio 所提供的 ASP.NET Scaffolding 工具來建立 Web API 控制器類別的基準。

1. 開啟**WebApiConfig.cs**檔案內**應用程式\_啟動**資料夾。 此檔案會定義 Web API 服務，例如路由如何對應到 Web API 控制器動作的組態。
2. 加入下列 using 陳述式開頭的檔案。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. 將下列反白顯示的程式碼加入**註冊**全域設定之 Web API 的動作方法所擷取的 JSON 資料格式器的方法。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > **CamelCasePropertyNamesContractResolver**會自動將屬性名稱來轉換*依照 camel 命名法*情況下，這是在 JavaScript 中的屬性名稱的一般慣例。
4. 在**方案總管 中**，以滑鼠右鍵按一下**控制器**資料夾**GeekQuiz**專案，然後選取**新增 |新的 Scaffold 項目...**.

    ![建立新的 scaffold 項目](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "建立新的 scaffold 項目")

    *建立新的 scaffold 項目*
5. 在**新增 Scaffold**對話方塊方塊中，請確定**常見**的左窗格中選取節點。 然後，選取**Web API 2 控制器空白**中央窗格中按一下範本**新增**。

    ![選取 Web API 2 空白控制器範本](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "選取 Web API 2 空白控制器的範本")

    *選取 Web API 2 空白控制器的範本*

    > [!NOTE]
    > **ASP.NET Scaffolding**是 ASP.NET Web 應用程式的程式碼產生架構。 Visual Studio 2013 包含預先安裝的程式碼產生器適用於 MVC 和 Web API 專案。 當您想要快速加入開發一般資料作業所需的程式碼與資料模型互動，以減少的時間量，您應該在專案中使用 scaffolding。
    > 
    > Scaffolding 程序也可確保在專案中安裝的所有必要的相依性。 比方說，如果您開頭空白的 ASP.NET 專案，然後使用 scaffolding 新增 Web API 控制器需要的 Web API NuGet 套件和參考會加入至您的專案會自動。
6. 在**加入控制器** 對話方塊中，輸入*TriviaController*中**控制器名稱**文字方塊中，然後按一下**新增**。

    ![新增瑣事控制器](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "新增瑣事控制器")

    *新增瑣事控制器*
7. **TriviaController.cs**檔案然後加入到**控制器**資料夾**GeekQuiz**包含空白的專案**TriviaController**類別。 加入下列 using 陳述式開頭的檔案。

    (程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. 加入下列程式碼的開頭**TriviaController**類別來定義、 初始化及處置**TriviaContext**控制器中的執行個體。

    (程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > **處置**方法**TriviaController**叫用**處置**方法**TriviaContext**執行個體，這可確保所有發行的內容物件所使用的資源時**TriviaContext**處置或回收執行個體。 這包括關閉所有開啟的 Entity Framework 的資料庫連接。
9. 新增下列 helper 方法的結尾**TriviaController**類別。 這個方法會擷取下列測驗問題回答所指定的使用者資料庫中。

    (程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. 加入下列**取得**至動作方法**TriviaController**類別。 這個動作方法會呼叫**NextQuestionAsync**擷取已驗證使用者的下一個問題在先前步驟中所定義的 helper 方法。

    (程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. 新增下列 helper 方法的結尾**TriviaController**類別。 這個方法會將指定的回應儲存在資料庫中，並傳回布林值，指出答案正確。

    (程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. 加入下列**Post**至動作方法**TriviaController**類別。 此動作方法產生關聯的已驗證的使用者和呼叫回應**StoreAsync** helper 方法。 接著，它會傳送回應與協助程式方法所傳回的布林值。

    (程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. 修改 Web API 控制器，以限制存取已驗證的使用者加入**授權**屬性**TriviaController**類別定義。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>工作 3-執行解決方案

在這項工作中，您將驗證您在先前的工作所建立的 Web API 服務的運作正常。 您將使用 Internet Explorer **F12 開發人員工具**擷取網路流量，並檢查 Web API 服務的完整回應。

> [!NOTE]
> 請確定**Internet Explorer**中選取**啟動**位於 Visual Studio 工具列上的按鈕。
> 
> ![Internet Explorer 選項](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. 按**F5**執行解決方案。 **登入**頁面應該會出現在瀏覽器中。

    > [!NOTE]
    > 當應用程式啟動時，預設 MVC 路由觸發時，預設會對應到**索引**動作**HomeController**類別。 因為**HomeController**限制為已驗證的使用者 (請記住您裝飾與該類別**授權**練習 1 中的屬性)，而且沒有任何已驗證的使用者尚未，應用程式將原始要求重新導向至登入頁面。

    ![執行解決方案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "執行解決方案")

    *執行解決方案*
2. 按一下**註冊**來建立新的使用者。

    ![註冊新的使用者](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "註冊新的使用者")

    *註冊新的使用者*
3. 在**註冊**頁面上，輸入**使用者名**和**密碼**，然後按一下 **註冊**。

    ![註冊頁面](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "註冊頁面")

    *註冊頁面*
4. 應用程式註冊新帳戶和使用者驗證，並且重新導向至首頁。

    ![使用者通過驗證](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "已驗證的使用者")

    *使用者驗證*
5. 在瀏覽器中，按**F12**開啟**開發人員工具**面板。 按**CTRL + 4**或按一下**網路**圖示，，然後按一下綠色箭頭按鈕，開始擷取網路流量。

    ![初始化 Web API 網路擷取](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "起始 Web API 網路擷取")

    *初始化 Web API 網路擷取*
6. 附加**api/瑣事**至瀏覽器的網址列中的 URL。 您現在將會檢查回應的詳細資料**取得**中的動作方法**TriviaController**。

    ![擷取下一個問題資料透過 Web API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "擷取下一個問題資料透過 Web API")

    *擷取下一個問題資料透過 Web API*

    > [!NOTE]
    > 一旦下載完成時，系統會提示您讓動作與下載的檔案。 讓對話方塊保持開啟，才能監看透過開發人員工具視窗的回應內容。
7. 現在您將會檢查回應的主體。 若要這樣做，請按一下**詳細資料**索引標籤，然後按一下 **回應主體**。 您可以下載的資料是物件的屬性來檢查**選項**(這是一份**TriviaOption**物件)，**識別碼**和**標題**對應到**TriviaQuestion**類別。

    ![檢視 Web API 回應主體](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "檢視 Web API 回應主體")

    *檢視 Web API 回應主體*
8. 返回 Visual Studio 並再按**SHIFT + F5**停止偵錯。

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>練習 2： 建立 SPA 介面

在這個練習中您需要先建立的 web 前端部份玩家測驗 」，將焦點放在單一頁面應用程式互動使用**AngularJS**。 然後，您會增強 CSS3 執行豐富的動畫，並提供內容切換時從一個問題轉換到下一個視覺效果的使用者體驗。

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>工作 1 – 建立使用 AngularJS SPA 介面

在這項工作會使用**AngularJS**實作玩家測驗應用程式的用戶端。 **AngularJS**是開放原始碼 JavaScript 架構，加強瀏覽器為基礎的應用程式與*模型-檢視-控制器*(MVC) 功能，加速這兩種開發和測試。

您一開始會安裝 Visual Studio Package Manager Console 中的 AngularJS。 然後，您將建立可提供一些受測應用程式，並且要呈現的測驗問題與解答使用 AngularJS 範本引擎的檢視行為的控制站。

> [!NOTE]
> 如需 AngularJS 的詳細資訊，請參閱[ [http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/)。


1. 開啟**Visual Studio Express 2013 for Web**並開啟**GeekQuiz.sln**方案位於**來源/Ex2 CreatingASPAInterface/開始**資料夾。 或者，您可以繼續使用解決方案您在上一個練習中取得。
2. 開啟**Package Manager Console**從**工具** | **程式庫套件管理員**。 輸入下列命令以安裝**AngularJS.Core** NuGet 封裝。

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. 在**方案總管 中**，以滑鼠右鍵按一下**指令碼**資料夾**GeekQuiz**專案，然後選取**新增 |新的資料夾**。 將資料夾命名為**應用程式**按**Enter**。
4. 以滑鼠右鍵按一下**應用程式**您剛才建立和選取的資料夾**新增 |JavaScript 檔案**。

    ![建立新的 JavaScript 檔](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *建立新的 JavaScript 檔*
5. 在**指定項目的名稱** 對話方塊中，輸入*測驗控制器*中**項目名稱**文字方塊中，然後按一下**確定**。

    ![命名新的 JavaScript 檔案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *命名新的 JavaScript 檔案*
6. 在**測驗 controller.js**檔案中加入下列程式碼，來宣告和初始化 AngularJS **QuizCtrl**控制站。

    (程式碼片段- *AspNetWebApiSpa-Ex2-AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > 建構函式的**QuizCtrl**控制站必須要有名稱為 injectable 參數**$scope**。 範圍的初始狀態應該進行設定的建構函式中附加至屬性**$scope**物件。 屬性包含**檢視模型**，並註冊控制器時將可存取範本。
    > 
    > **QuizCtrl**名為模組內定義控制器**QuizApp**。 模組是工作的可讓您單位的應用程式分成不同的元件。 使用模組的主要優點是，程式碼更易於了解，而且有助於進行單元測試、 可重複使用性和可維護性。
7. 您現在要加入至範圍的行為以回應從檢視所觸發的事件。 加入下列程式碼的結尾**QuizCtrl**控制站，以定義**nextQuestion**函式在**$scope**物件。

    (程式碼片段- *AspNetWebApiSpa-Ex2-AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > 此函式會擷取從下一個問題**瑣事**Web API 在上一個練習中建立並附加至問題資料**$scope**物件。
8. 插入下列程式碼的結尾**QuizCtrl**控制站，以定義**sendAnswer**函式在**$scope**物件。

    (程式碼片段- *AspNetWebApiSpa-Ex2-AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > 此函式會將以使用者選取回應**瑣事**Web API，並將儲存中的結果 – 也就是如果答案不正確，或未 – **$scope**物件。
    > 
    > **NextQuestion**和**sendAnswer**上述函式會使用 AngularJS **$http**抽象與 Web API，透過 XMLHttpRequest 通訊的物件從瀏覽器的 JavaScript 物件。 AngularJS 支援其他服務，讓較高層級的抽象概念，來執行 CRUD 作業，針對透過 rest 式 Api 資源。 AngularJS **$resource**物件有提供高層級的行為，而不需要互動的動作方法**$http**物件。 請考慮使用**$resource**案例中需要 CRUD 模型的物件 (fore 的詳細資訊，請參閱[$resource 文件](https://docs.angularjs.org/api/ngResource/service/$resource))。
9. 下一個步驟是建立定義檢視測驗的 AngularJS 範本。 若要這樣做，請開啟**Index.cshtml**檔案內**檢視 |首頁**資料夾，然後取代為下列程式碼的內容。

    (程式碼片段- *AspNetWebApiSpa-Ex2-GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > AngularJS 範本是使用資訊模型與控制器來轉換在瀏覽器中的使用者會看到動態檢視的 static 標記的宣告式規格。 AngularJS 項目和可用範本中的項目屬性的範例如下：
    > 
    > - **Ng 應用程式**指示詞會告訴 AngularJS 代表應用程式的根元素的 DOM 項目。
    > - **Ng 控制器**指示詞會附加至指示詞宣告的位置處 DOM 的控制器。
    > - 大括號標記法**{{}}**代表領域內容，此控制器中定義的繫結。
    > - **Ng 按一下**指示詞用來叫用在以回應使用者按一下範圍中定義的函式。
10. 開啟**Site.css**檔案內**內容**資料夾和檔案，以提供測驗檢視的外觀與風格的結尾加入下列反白顯示的樣式。

    (程式碼片段- *AspNetWebApiSpa-Ex2-GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>工作 2-執行解決方案

在這項工作會執行方案中使用新的使用者介面使用 AngularJS 來回答的一些測驗問題建置您。

1. 按**F5**執行解決方案。
2. 註冊新的使用者帳戶。 若要這樣做，請依照下列練習 1，工作 3 中所述的註冊步驟。

    > [!NOTE]
    > 如果您使用的解決方案中的上一個練習中，您可以登入之前所建立的使用者帳戶。
3. **首頁**頁面應該會出現，顯示測驗的第一個問題。 按一下其中一個選項，以回答這個問題。 如此將會觸發**sendAnswer**函數之前，定義傳送到選取的選項**瑣事**Web API。

    ![回答](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "回答的問題。")

    *回答問題。*
4. 按一下其中一個按鈕之後, 應該會出現答案。 按一下**下一個問題**以顯示下列問題。 如此將會觸發**nextQuestion**此控制器中定義的函式。

    ![要求下一個問題](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "要求下一個問題")

    *要求下一個問題*
5. 下一個問題應該會出現。 繼續回應的問題，您想要的次數。 完成的所有問題之後，您應該回復第一個問題。

    ![另一個問題](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "另一個問題")

    *下一個問題*
6. 返回 Visual Studio 並再按**SHIFT + F5**停止偵錯。

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>工作 3： 建立使用 CSS3 翻轉動畫

在這項工作中，您會使用 CSS3 屬性來將加入翻轉的效果，以及擷取下一個問題時的問題的回答執行豐富的動畫。

1. 在**方案總管 中**，以滑鼠右鍵按一下**內容**資料夾**GeekQuiz**專案，然後選取**新增 |現有的項目...**.

    ![將現有的項目加入至內容的資料夾](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "內容的資料夾加入現有項目")

    *將現有的項目加入至內容的資料夾*
2. 在**加入現有項目**對話方塊方塊中，瀏覽至**來源/資產**資料夾，然後選取**Flip.css**。 按一下 [加入] 。

    ![從資產加入 Flip.css 檔案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "從資產加入 Flip.css 檔案")

    *從資產加入 Flip.css 檔案*
3. 開啟**Flip.css**您剛才加入檔案，並檢查其內容。
4. 找出**翻轉轉換**註解。 該註解下列樣式使用 CSS**觀點來看**和**rotateY**轉換，以產生&quot;卡片翻轉&quot;效果。

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. 找出**翻轉期間隱藏窗格背面**註解。 該註解下列樣式會藉由設定面臨遠離檢視器時，隱藏面後端**backface 可視性**CSS 屬性*隱藏*。

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. 開啟**BundleConfig.cs**檔案內**應用程式\_啟動**資料夾並加入參考**Flip.css**檔案 **&quot;~/Content/css&quot;** 樣式組合

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. 按**F5**執行方案並登入您的認證。
8. 按一下其中一個選項回答問題。 檢視之間轉換時，請注意翻轉的效果。

    ![翻轉效果的問題要回答](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "回答的翻轉的效果")

    *回答的翻轉的效果*
9. 按一下**下一個問題**擷取下列問題。 翻轉效果應該會再次出現。

    ![擷取下列問題的翻轉效果](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "擷取下列問題的翻轉的效果")

    *擷取下列問題的翻轉的效果*

* * *

<a id="Summary"></a>
## <a name="summary"></a>總結

藉由完成這個實際操作實驗室，您已經學會如何：

- 建立使用 ASP.NET Scaffold ASP.NET Web API 控制器
- 實作 Web API 取得的動作，來擷取下一個測驗問題
- 實作 Web API 後動作來儲存測驗的答案
- 從 Visual Studio 封裝管理員主控台安裝 AngularJS
- 實作的 AngularJS 範本和控制站
- 若要執行動畫效果使用 CSS3 轉換
