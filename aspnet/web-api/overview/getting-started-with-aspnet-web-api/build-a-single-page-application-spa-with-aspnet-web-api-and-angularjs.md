---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 實習實驗室： 建置單一頁面應用程式 (SPA) 使用 ASP.NET Web API 和 Angular.js |Microsoft Docs
author: rick-anderson
description: 在傳統的 web 應用程式，用戶端 （瀏覽器） 會起始與伺服器通訊，藉由要求的頁面。 伺服器接著會處理要求...
ms.author: riande
ms.date: 09/30/2015
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 3c3557bb2be2807b11874937fcc629b5b773e463
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912250"
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>實習實驗室： 建置單一頁面應用程式 (SPA) 使用 ASP.NET Web API 和 Angular.js
====================
藉由[Web Camp 小組](https://twitter.com/webcamps)

[下載 Web 研討會訓練套件](http://aka.ms/webcamps-training-kit)

> 在傳統的 web 應用程式，用戶端 （瀏覽器） 會起始與伺服器通訊，藉由要求的頁面。 伺服器接著會處理要求，並傳送給用戶端網頁的 HTML。 在後續的互動與頁面 – 例如使用者瀏覽至連結或送出表單資料 – 新的要求會傳送到伺服器，並重新啟動流程： 伺服器處理要求，並將新的頁面傳送至新的動作要求回應中的瀏覽器ed 用戶端。
> 
> 在單一頁面應用程式 (Spa) 整頁瀏覽器中之後載入初始要求中，但後續的互動進行透過 Ajax 要求。 這表示瀏覽器具有更新的頁面已變更; 部份沒有需要重新載入整個頁面。 SPA 方法可減少應用程式以回應使用者動作，導致更流暢的經驗所花費的時間。
> 
> SPA 架構牽涉到某些不會出現在 傳統 web 應用程式的挑戰。 不過，新興技術，例如 ASP.NET Web API，例如 AngularJS JavaScript 架構，並提供 CSS3 的新樣式功能會使它很容易設計和建置 Spa。
> 
> 在此手動在實驗室中，您會利用這些技術來實作 Geek 測驗，SPA 概念為基礎的邏輯網站。 您第一次會實作 ASP.NET Web API，可擷取測驗問題，並儲存回應所需的端點公開 （expose） 服務層。 然後，您將建置豐富且回應迅速的 UI，使用 AngularJS 和 CSS3 轉換效果。
> 
> 所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit)。


## <a name="overview"></a>總覽

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將了解如何：

- 建立 ASP.NET Web API 服務，以傳送和接收 JSON 資料
- 建立回應式 UI，使用 AngularJS
- 增強 CSS3 轉換的 UI 體驗

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

需要下列項目才能完成這個實際操作實驗室：

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更新版本

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

若要在這個實際操作實驗室中執行的練習，您必須先設定您的環境。

1. 開啟 Windows Explorer 並瀏覽至實驗室**來源**資料夾。
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

1. [建立 Web API](#Exercise1)
2. [建立 SPA 介面](#Exercise2)

估計的時間才能完成這個實驗室： **60 分鐘**

> [!NOTE]
> 當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。 每個預先定義的集合以符合特定的開發樣式設計，並決定視窗版面配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。 在這個實驗室中的程序說明完成指定的工作，在 Visual Studio 中使用時所需的動作**一般開發設定**集合。 如果您選擇不同的設定集合，您的開發環境，可能會有在步驟中，您應該考慮到的差異。


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>練習 1： 建立 Web API

SPA 的主要部分是服務層。 它會負責處理 UI 和該呼叫回應中的傳回資料所傳送的 Ajax 呼叫。 擷取的資料應該會看到，才能進行剖析與耗用用戶端電腦可讀取的格式。

Web API 架構是 ASP.NET 堆疊的一部分，旨在讓您輕鬆地實作 HTTP 服務，通常傳送和接收 JSON 或 XML 格式的資料，透過 RESTful API。 在這個練習中，您將建立網站，以裝載 Geek 測驗應用程式，然後再實作後端服務公開 （expose），並使用 ASP.NET Web API 的測驗資料保存。

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>工作 1-Geek 測驗建立初始專案

在這項工作會開始使用支援建立新的 ASP.NET MVC 專案，ASP.NET Web API 是依據**One ASP.NET**專案類型隨附於 Visual Studio。 **One ASP.NET**統一所有 ASP.NET 技術，並可讓您混合及比對它們所需的選項。 您接著會新增 Entity Framework 的模型類別和資料庫 initializator，若要插入的測驗問題。

1. 開啟**Visual Studio Express 2013 for Web** ，然後選取**檔案 |新增專案...** 啟動新的解決方案。

    ![建立新的專案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "建立新的專案")

    *建立新的專案*
2. 在 [**新的專案**對話方塊中，選取**ASP.NET Web 應用程式**下**Visual C# |Web** ] 索引標籤。請確定 **.NET Framework 4.5**是選取，其命名*GeekQuiz*，選擇**位置**然後按一下**確定**。

    ![建立新的 ASP.NET Web 應用程式專案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "建立新的 ASP.NET Web 應用程式專案")

    *建立新的 ASP.NET Web 應用程式專案*
3. 在 **新的 ASP.NET 專案**對話方塊中，選取**MVC**範本，然後選取**Web API**選項。 此外，請確定**驗證**選項設定為**個別使用者帳戶**。 按一下 [確定]  繼續操作。

    ![使用 MVC 範本，包括 Web API 元件建立新的專案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *使用 MVC 範本，包括 Web API 元件建立新的專案*
4. 在 [**方案總管] 中**，以滑鼠右鍵按一下**模型**資料夾**GeekQuiz**專案，然後選取**新增 |現有的項目...**.

    ![加入現有項目](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "加入現有項目")

    *加入現有項目*
5. 在 **加入現有項目**對話方塊方塊中，瀏覽至**來源/資產/模型**資料夾，然後選取所有檔案。 按一下 [加入] 。

    ![新增模型資產](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "加入模型資產")

    *新增模型資產*

    > [!NOTE]
    > 藉由新增這些檔案，您會加入資料模型、 Entity Framework 資料庫內容和玩家測驗應用程式的資料庫初始設定式。
    > 
    > **Entity Framework (EF)** 是物件關聯式對應程式 (ORM)，可讓您使用而不是直接使用關聯式儲存結構描述的程式設計概念應用程式模型進行程式設計來建立資料存取應用程式。 您可以深入了解 Entity Framework[此處](../../../entity-framework.md)。
    > 
    > 以下是您剛才新增的類別的描述：
    > 
    > - **TriviaOption:** 表示測驗問題相關聯的單一選項
    > - **TriviaQuestion:** 代表測驗問題，並公開 （expose） 相關聯的選項，透過**選項**屬性
    > - **TriviaAnswer:** 表示測驗問題的回應中的使用者所選取的選項
    > - **TriviaContext:** 代表玩家測驗應用程式的 Entity Framework 資料庫內容。 這個類別衍生自**DContext** ，並公開**DbSet**代表上面所述的實體集合的屬性。
    > - **TriviaDatabaseInitializer:** Entity Framework 的初始設定式的實作**TriviaContext**類別繼承自**CreateDatabaseIfNotExists**。 這個類別的預設行為是建立資料庫，它不存在時，才，插入的實體中指定**種子**方法。
6. 開啟**Global.asax.cs**檔案，並新增下列 using 陳述式。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. 新增下列程式碼的開頭**應用程式\_開始**方法來設定**TriviaDatabaseInitializer**為資料庫初始設定式。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. 修改**首頁**来限制存取的控制器已驗證的使用者。 若要這樣做，請開啟**HomeController.cs**檔案內**控制站**資料夾，並新增**授權**屬性設定為**HomeController**類別定義。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > **授權**篩選檢查，以查看是否已驗證使用者。 如果使用者未經過驗證，它會傳回 HTTP 狀態碼 401 （未經授權），而不叫用動作。 您可以套用全域，在控制器層級，或個別動作的層級的篩選器。
9. 現在，您將自訂網頁和商標版面的配置。 若要這樣做，請開啟 **\_Layout.cshtml**檔案**檢視 |共用**資料夾，並更新的內容**&lt;標題&gt;** 取代的項目*My ASP.NET Application*與*Geek 測驗*.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. 在相同的檔案中，更新導覽列藉由移除*關於*並*連絡人*連結和重新命名*首頁*連結至*播放*。 此外，重新命名*應用程式名稱*連結至*Geek 測驗*。 在導覽列 HTML 看起來應該類似下列的程式碼。

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. 藉由取代更新的版面配置頁的頁尾*My ASP.NET Application*具有*Geek 測驗*。 若要這樣做，請將取代的內容**&lt;頁尾&gt;** 具有下列醒目提示的程式碼項目。

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>工作 2-建立 TriviaController Web API

在先前工作中，您會建立初始結構的玩家測驗的 web 應用程式。 現在，您將建置一個簡單的 Web API 服務，與測驗資料模型互動，而且會公開下列動作：

- **GET/api/邏輯**： 測驗清單來驗證的使用者回應中擷取下一個問題。
- **POST/api/邏輯**： 儲存已驗證的使用者所指定的測驗解答。

您將使用 Visual Studio 所提供的 ASP.NET Scaffolding 工具來建立 Web API 控制器類別的基準。

1. 開啟**WebApiConfig.cs**檔案內**應用程式\_啟動**資料夾。 此檔案會定義 Web API 服務，例如路由如何對應至 Web API 控制器動作的設定。
2. 新增下列 using 陳述式在檔案開頭。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. 將下列反白顯示的程式碼，加入**註冊**全域設定 Web API 動作方法所擷取的 JSON 資料的格式器的方法。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > **CamelCasePropertyNamesContractResolver**會自動轉換到的屬性名稱*依照 camel 命名法*情況下，也就是在 JavaScript 中的屬性名稱的一般慣例。
4. 在 [**方案總管] 中**，以滑鼠右鍵按一下**控制站**資料夾**GeekQuiz**專案，然後選取**新增 |新增 Scaffold 項目...**.

    ![建立新的 scaffold 項目](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "建立新的 scaffold 項目")

    *建立新的 scaffold 項目*
5. 在 **新增 Scaffold**對話方塊方塊中，請確定**常見**的左窗格中選取節點。 然後，選取**Web API 2 控制器-空白**中央窗格中按一下範本**新增**。

    ![選取 Web API 2 空白控制器樣板](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "選取 Web API 2 空白控制器的範本")

    *選取 Web API 2 空白控制器的範本*

    > [!NOTE]
    > **ASP.NET Scaffolding**是 ASP.NET Web 應用程式的程式碼產生架構。 Visual Studio 2013 包含預先安裝的程式碼產生器，適用於 MVC 和 Web API 專案。 當您想要快速地加入開發標準的資料作業所需的程式碼與資料模型互動，以降低的時間量，您應該在專案中使用 scaffolding。
    > 
    > Scaffolding 程序也可確保在專案中安裝的所有必要的相依性。 比方說，如果您開始 空白的 ASP.NET 專案，然後使用 scaffolding Web API 控制器，將必要的 Web API NuGet 套件和參考會加入至您的專案自動。
6. 在 [**新增控制器**] 對話方塊中，輸入*TriviaController*中**控制器名稱**文字方塊中，然後按一下**新增**。

    ![新增邏輯控制站](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "新增邏輯控制站")

    *新增邏輯控制站*
7. **TriviaController.cs**檔案接著會新增至**控制站**資料夾**GeekQuiz**包含空的專案**TriviaController**類別。 新增下列 using 陳述式在檔案開頭。

    (程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. 新增下列程式碼的開頭**TriviaController**類別來定義、 初始化和處置**TriviaContext**控制器中的執行個體。

    (程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > **處置**方法**TriviaController**叫用**處置**方法**TriviaContext**執行個體，這可確保所有發行的內容物件所使用的資源時**TriviaContext**執行個體已遭處置或記憶體回收。 這包括關閉所有開啟的 Entity Framework 的資料庫連線。
9. 新增下列 helper 方法的結尾**TriviaController**類別。 這個方法會擷取下列測驗問題，從指定的使用者來回應的資料庫。

    (程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. 新增下列**取得**動作方法，來**TriviaController**類別。 這個動作方法會呼叫**NextQuestionAsync** helper 方法來擷取下一個問題，已驗證之使用者的上一個步驟中所定義。

    (程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. 新增下列 helper 方法的結尾**TriviaController**類別。 這個方法會在資料庫中儲存指定的回應，並傳回布林值，指出是正確解答。

    (程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. 新增下列**Post**動作方法，來**TriviaController**類別。 這個動作方法產生關聯的已驗證的使用者和呼叫的答案**StoreAsync** helper 方法。 然後，它會傳送回應與協助程式方法所傳回的布林值。

    (程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. 修改 Web API 控制器來限制存取已驗證的使用者，藉由新增**Authorize**屬性設定為**TriviaController**類別定義。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>工作 3-執行解決方案

在這項工作中，您將會驗證您在上一個工作中建立的 Web API 服務運作正常。 您將使用 Internet Explorer **F12 開發人員工具**擷取網路流量，並檢查 Web API 服務的完整回應。

> [!NOTE]
> 請確定**Internet Explorer**中選取**開始**位於 Visual Studio 工具列上的按鈕。
> 
> ![Internet Explorer 選項](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. 按下**F5**執行方案。 **登入**頁面應該會出現在瀏覽器。

    > [!NOTE]
    > 當應用程式啟動時，預設 MVC 路由觸發時，預設會對應至**Index**動作**HomeController**類別。 由於**HomeController**限制為已驗證的使用者 (請記住您裝飾具有該類別**授權**在練習 1 中的屬性)，而且沒有任何已驗證的使用者，應用程式將原始的要求重新導向至登入頁面。

    ![執行解決方案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "執行解決方案")

    *執行解決方案*
2. 按一下 **註冊**來建立新的使用者。

    ![註冊新的使用者](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "註冊新的使用者")

    *註冊新的使用者*
3. 在**註冊**頁面上，輸入**使用者名稱**並**密碼**，然後按一下**註冊**。

    ![註冊頁面](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "註冊 頁面")

    *註冊頁面*
4. 應用程式註冊新的帳戶，並驗證使用者並將其重新導向回到首頁。

    ![使用者驗證](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "已驗證的使用者")

    *使用者驗證*
5. 在瀏覽器中，按下**F12**來開啟**開發人員工具**面板。 按下**CTRL + 4**或按**網路**圖示，然後按一下綠色箭號按鈕以開始擷取網路流量。

    ![起始 Web API 網路擷取](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "起始 Web API 網路擷取")

    *起始的 Web API 網路擷取*
6. 附加**api/邏輯**至瀏覽器的網址列中的 URL。 您現在會檢查來自回應的詳細資料**取得**中的動作方法**TriviaController**。

    ![擷取下一個問題資料透過 Web API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "擷取下一個問題資料透過 Web API")

    *擷取下一個問題資料透過 Web API*

    > [!NOTE]
    > 下載完成後，系統會提示您使用下載的檔案進行的動作。 讓對話方塊保持開啟，才能夠觀賞回應內容，透過開發人員工具視窗。
7. 現在，您將會檢查回應的主體。 若要這樣做，請按一下**詳細資料**索引標籤，然後按一下**回應主體**。 您可以檢查已下載的資料是否具有屬性的物件**選項**(這是一份**TriviaOption**物件)，**識別碼**並**標題**對應到**TriviaQuestion**類別。

    ![檢視 Web API 回應內文](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "檢視 Web API 回應主體")

    *檢視 Web API 回應主體*
8. 請返回 Visual Studio，然後按**SHIFT + F5**停止偵錯。

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>練習 2： 建立 SPA 介面

在這個練習中您需要先建立的 web 前端部分 Geek 測驗，將焦點放在單一頁面應用程式互動使用**AngularJS**。 然後，您將會增強 CSS3 執行豐富的動畫，並提供內容切換時從一個問題轉換到下一個視覺效果的使用者體驗。

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>工作 1-建立使用 AngularJS SPA 介面

在這項工作會使用**AngularJS**實作 Geek 測驗應用程式的用戶端。 **AngularJS**是一種開放原始碼 JavaScript 架構，可增強使用瀏覽器為基礎的應用程式*模型-檢視-控制器*(MVC) 功能，可促進開發和測試。

您會藉由從 Visual Studio Package Manager Console 安裝 AngularJS 來開始。 然後，您將建立的控制站，以提供玩家測驗應用程式和檢視來呈現的測驗問題與解答使用 AngularJS 範本引擎的行為。

> [!NOTE]
> 如需 AngularJS 的詳細資訊，請參閱[ [ http://angularjs.org/ ](http://angularjs.org/) ](http://angularjs.org/)。


1. 開啟**Visual Studio Express 2013 for Web** ，然後開啟**GeekQuiz.sln**解決方案位於**來源/Ex2-CreatingASPAInterface/開始**資料夾。 或者，您可以繼續使用解決方案您在前一個練習中取得。
2. 開啟**Package Manager Console**從**工具** > **NuGet 套件管理員**。 輸入下列命令以安裝**AngularJS.Core** NuGet 套件。

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. 在 [**方案總管] 中**，以滑鼠右鍵按一下**指令碼**資料夾**GeekQuiz**專案，然後選取**新增 |新的資料夾**。 將資料夾命名**應用程式**然後按**Enter**。
4. 以滑鼠右鍵按一下**應用程式**資料夾，您剛建立，並選取**新增 |JavaScript 檔案**。

    ![建立新的 JavaScript 檔案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *建立新的 JavaScript 檔案*
5. 在 [**指定項目的名稱**] 對話方塊中，輸入*測驗控制器*中**項目名稱**文字方塊中，然後按一下 **[確定]**。

    ![命名新的 JavaScript 檔案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *命名新的 JavaScript 檔案*
6. 在 **測驗 controller.js**檔案中，新增下列程式碼來宣告和初始化 AngularJS **QuizCtrl**控制站。

    (程式碼片段- *AspNetWebApiSpa-Ex2-AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > 建構函式的**QuizCtrl**控制器需要名為 injectable 參數 **$scope**。 範圍的初始狀態應該設定在建構函式藉由附加屬性，以 **$scope**物件。 屬性包含**檢視模型**，並可讓範本註冊控制器時。
    > 
    > **QuizCtrl**名為模組內定義控制器**QuizApp**。 模組是工作的可讓您單位會將您的應用程式分成個別的元件。 使用模組的主要優點是容易了解的程式碼，並有助於單元測試、 可重複使用性和可維護性。
7. 您現在會新增至範圍的行為，以回應事件觸發從檢視。 在結尾新增下列程式碼**QuizCtrl**定義的控制站**nextQuestion**函式中 **$scope**物件。

    (程式碼片段- *AspNetWebApiSpa-Ex2-AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > 此函式會擷取下一個問題，從**邏輯**Web API 在前一個練習中建立並附加到的問題資料 **$scope**物件。
8. 插入下列程式碼結尾處**QuizCtrl**定義的控制站**sendAnswer**函式中 **$scope**物件。

    (程式碼片段- *AspNetWebApiSpa-Ex2-AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > 此函式會將傳送到使用者所選取的答案**邏輯**Web API，並將結果 – 也就是如果答案是否正確 – 在 **$scope**物件。
    > 
    > **NextQuestion**並**sendAnswer**上述的函式會使用 AngularJS **$http**抽象化的通訊使用透過 XMLHttpRequest 的 Web API 的物件從瀏覽器的 JavaScript 物件。 AngularJS 支援另一項服務，讓較高層級的抽象概念，來執行 CRUD 作業，針對透過 RESTful Api 的資源。 AngularJS **$resource**物件具有動作方法提供高階的行為，而不需要互動 **$http**物件。 請考慮使用 **$resource**案例中需要 CRUD 模型的物件 (前景的詳細資訊，請參閱[$resource 文件](https://docs.angularjs.org/api/ngResource/service/$resource))。
9. 下一個步驟是建立定義的檢視測驗的 AngularJS 範本。 若要這樣做，請開啟**Index.cshtml**檔案內**檢視 |首頁**資料夾，並將下列程式碼的內容。

    (程式碼片段- *AspNetWebApiSpa-Ex2-GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > AngularJS 範本是宣告式的規格，會使用從模型和控制器的資訊，將靜態標記轉換成瀏覽器中的使用者會看到動態檢視。 AngularJS 元素和可用範本中的項目屬性的範例如下︰
    > 
    > - **Ng 應用程式**指示詞會告訴 AngularJS DOM 項目，表示應用程式的根項目。
    > - **Ng 控制器**指示詞會將控制器附加至 DOM 的點指示詞宣告的位置。
    > - 大括號標記法 **{{}}** 表示控制器中定義的領域屬性的繫結。
    > - **Ng 按一下**指示詞用來叫用在以回應使用者按一下範圍中定義的函式。
10. 開啟**Site.css**檔案內**內容**資料夾和檔案，以提供測驗檢視的外觀與風格的結尾加入下列反白顯示的樣式。

    (程式碼片段- *AspNetWebApiSpa-Ex2-GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>工作 2-執行解決方案

在您將執行此工作中使用新的使用者方案介面您建置使用 AngularJS 來回答的一些測驗問題。

1. 按下**F5**執行方案。
2. 註冊新的使用者帳戶。 若要這樣做，請依照下列練習 1 中，工作 3 中所述的註冊步驟。

    > [!NOTE]
    > 如果您使用的解決方案，從上一個練習，您可以登入您在之前建立的使用者帳戶。
3. **首頁**頁面應該會出現，顯示測驗的第一個問題。 按一下其中一個選項，以回答問題。 這會觸發**sendAnswer**稍早定義函式會將傳送至選取的選項**邏輯**Web API。

    ![回答](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "回答的問題")

    *回答的問題*
4. 按一下其中一個按鈕之後, 應該會出現答案。 按一下 **下一個問題**以顯示下列問題。 這會觸發**nextQuestion**此控制器中定義的函式。

    ![要求的下一個問題](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "要求下一個問題")

    *要求的下一個問題*
5. 下一個問題應該會出現。 繼續執行視需要多次回答的問題。 完成所有問題之後，您應該傳回第一個問題。

    ![另一個問題](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "另一個問題")

    *下一個問題*
6. 請返回 Visual Studio，然後按**SHIFT + F5**停止偵錯。

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>工作 3-建立翻頁動畫的動畫使用 CSS3

在這項工作中，您會使用 CSS3 屬性來加入翻頁動畫的效果，以及擷取下一個問題時在解答的問題，以執行豐富的動畫。

1. 在 [**方案總管] 中**，以滑鼠右鍵按一下**內容**資料夾**GeekQuiz**專案，然後選取**新增 |現有的項目...**.

    ![將現有的項目新增至內容的資料夾](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "Content 資料夾加入現有項目")

    *將現有的項目新增至 [Content] 資料夾*
2. 在 **加入現有項目**對話方塊方塊中，瀏覽至**來源/資產**資料夾，然後選取**Flip.css**。 按一下 [加入] 。

    ![從資產新增 Flip.css 檔案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "從資產新增 Flip.css 檔案")

    *從資產新增 Flip.css 檔案*
3. 開啟**Flip.css**您剛才新增的檔案，並檢查其內容。
4. 找出**翻轉轉換**註解。 下方的註解樣式使用 CSS**觀點來看**並**rotateY**轉換，以產生&quot;卡片 flip&quot;效果。

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. 找出**flip 期間隱藏窗格背面**註解。 當面臨遠離檢視器藉由設定時，下方的註解的樣式會隱藏臉部在後端**背面可視性**CSS 屬性*隱藏*。

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. 開啟**BundleConfig.cs**檔案內**應用程式\_開始**資料夾並將參考加入**Flip.css**檔案**&quot;~/Content/css&quot;** 樣式套件組合

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. 按下**F5**執行方案，然後登入您的認證。
8. 按一下其中一個選項回答的問題。 檢視之間轉換時，請注意翻頁動畫的效果。

    ![翻頁動畫效果的回答](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "回答翻頁動畫的效果")

    *回答翻頁動畫的效果*
9. 按一下 **下一個問題**擷取下列問題。 翻頁動畫的效果應會再次出現。

    ![正在擷取下列翻頁動畫效果的問題](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "擷取下列翻頁動畫效果的問題")

    *正在擷取下列翻頁動畫效果的問題*

* * *

<a id="Summary"></a>
## <a name="summary"></a>總結

藉由完成這個實際操作實驗室，您已經學會如何：

- 建立 ASP.NET Web API 控制器使用的 ASP.NET Scaffold
- 實作 Web API 取得動作來擷取下一個測驗問題
- 實作 Web API 後動作來儲存測驗的答案
- 從 Visual Studio 套件管理員主控台安裝 AngularJS
- 實作的 AngularJS 範本及控制器
- 使用 CSS3 轉換執行動畫效果
