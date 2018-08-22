---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 實習實驗室： 一個 ASP.NET： 整合 ASP.NET Web Form、 MVC 和 Web API |Microsoft Docs
author: rick-anderson
description: ASP.NET 是用於建置網站、 應用程式和服務使用特製化的技術，例如 MVC、 Web API 等的架構。 與延伸 ASP.NET h...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: b6ac0dca92ab3d75eb871099882dcea549264354
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835030"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>實習實驗室： 一個 ASP.NET： 整合 ASP.NET Web Form、 MVC 和 Web API
====================
藉由[Web Camp 小組](https://twitter.com/webcamps)

[下載 Web 研討會訓練套件](http://aka.ms/webcamps-training-kit)

> ASP.NET 是用於建置網站、 應用程式和服務使用特製化的技術，例如 MVC、 Web API 等的架構。 與延伸 ASP.NET 已經看到自建立後，表示需要有整合這些技術中已經有最新的工作來運作**One ASP.NET**。
> 
> Visual Studio 2013 導入了新的統一的專案系統可讓您建置應用程式和一個專案中使用所有的 ASP.NET 技術。 這項功能就不需要在開始專案，並隨身碟，挑選一種技術，並改為鼓勵使用一個專案中的多個 ASP.NET 架構。
> 
> 所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit)。


<a id="Overview"></a>
## <a name="overview"></a>總覽

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將了解如何：

- 建立網站，根據**One ASP.NET**專案類型
- 使用不同**ASP.NET**架構喜歡**MVC**並**Web API**相同專案中
- 識別的主要元件**ASP.NET**應用程式
- 善用**ASP.NET Scaffolding**架構，來自動建立控制器和檢視來執行 CRUD 作業會根據您的模型類別
- 公開 （expose) 相同的機器和人類看得懂的格式為每個工作中使用正確的工具中的資訊集

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

需要下列項目才能完成這個實際操作實驗室：

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更新版本
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

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

1. [建立新的 Web Form 專案](#Exercise1)
2. [建立使用 Scaffolding MVC 控制器](#Exercise2)
3. [建立使用 Scaffolding Web API 控制器](#Exercise3)

估計的時間才能完成這個實驗室： **60 分鐘**

> [!NOTE]
> 當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。 每個預先定義的集合以符合特定的開發樣式設計，並決定視窗版面配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。 在這個實驗室中的程序說明完成指定的工作，在 Visual Studio 中使用時所需的動作**一般開發設定**集合。 如果您選擇不同的設定集合，您的開發環境，可能會有在步驟中，您應該考慮到的差異。


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>練習 1： 建立新的 Web Form 專案

在此練習中，您將建立新的 Web Form 網站，在 Visual Studio 2013 中使用**One ASP.NET**統一專案體驗，可讓您輕鬆地將相同的應用程式的 Web Form、 MVC 和 Web API 元件整合。 您接著會探索產生的方案，並找出其組件，和最後您會看到動作中的網站。

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>工作 1-建立新的站台使用一個 ASP.NET 使用經驗

在這項工作會啟動 Visual Studio 中建立新的網站為基礎**One ASP.NET**專案類型。 **One ASP.NET**統一所有 ASP.NET 技術，並可讓您混合及比對它們所需的選項。 然後辨識 live Web Form、 MVC 和 Web API 的不同元件並存應用程式內。

1. 開啟**Visual Studio Express 2013 for Web** ，然後選取**檔案 |新增專案...** 啟動新的解決方案。

    ![建立新專案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *建立新的專案*
2. 在 **新的專案**對話方塊中，選取**ASP.NET Web 應用程式**下**Visual C# |Web**索引標籤，並確定 **.NET Framework 4.5**已選取。 將專案命名為*MyHybridSite*，選擇**位置**然後按一下**確定**。

    ![新的 ASP.NET Web 應用程式專案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *建立新的 ASP.NET Web 應用程式專案*
3. 在 **新增 ASP.NET 專案**對話方塊中，選取**Web Form**範本，然後選取**MVC**並**Web API**選項。 此外，請確定**驗證**選項設定為**個別使用者帳戶**。 按一下 [確定]  繼續操作。

    ![使用 Web Form 範本，包括 Web API 和 MVC 元件建立新的專案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *使用 Web Form 範本，包括 Web API 和 MVC 元件建立新的專案*
4. 您現在可以探索產生的方案結構。

    ![探索產生的方案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *探索產生的方案*

    1. **帳戶：** 這個資料夾包含 Web Form 網頁，以註冊、 登入，以及管理應用程式的使用者帳戶。 此資料夾時就會加入**個別使用者帳戶**Web Form 專案範本在設定期間選取驗證選項。
    2. **模型：** 這個資料夾將包含代表您的應用程式資料的類別。
    3. **控制站**並**檢視**： 這些資料夾所需**ASP.NET MVC**並**ASP.NET Web API**元件。 您將探索的 MVC 和 Web API 的技術，在下一個練習中。
    4. **Default.aspx**， **Contact.aspx**並**about.aspx 的網頁**檔案是預先定義的 Web 表單頁面，您可以使用做為起點來建立特定頁面您應用程式。 這些檔案的程式設計邏輯位於個別的檔案，稱為&quot;程式碼後置&quot;檔案，其有&quot;。 aspx.cs&quot;或&quot;。 aspx.cs&quot;延伸模組 (取決於使用語言）。 程式碼後置邏輯伺服器上執行，並以動態方式產生您頁面的 HTML 輸出。
    5. **Site.Master**並**Site.Mobile.Master**頁面定義應用程式中的外觀與風格及標準行為的所有頁面。
5. 按兩下**Default.aspx**瀏覽網頁的內容檔案。

    ![瀏覽 Default.aspx 頁面](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *瀏覽 Default.aspx 頁面*

    > [!NOTE]
    > **網頁**在檔案頂端的指示詞定義的 Web Form 網頁的屬性。 例如， **MasterPageFile**屬性會指定為主要路徑頁面-在此情況下， *Site.Master*頁面和**Inherits**屬性定義[繼承] 頁面的程式碼後置類別。 此類別位於檔案取決於**程式碼後置**屬性。
    > 
    > **Asp: Content**控制項包含頁面 （文字、 標記和控制項） 的實際內容，且對應至**asp: ContentPlaceHolder**主版頁面上的控制項。 在此情況下，將呈現頁面內容內*MainContent*中所定義的控制項*Site.Master*頁面。
6. 依序展開**應用程式\_開始**資料夾，請注意**WebApiConfig.cs**檔案。 Visual Studio 中產生的方案包含該檔案，因為 One ASP.NET 範本與設定您的專案時，包含 Web API。
7. 開啟**WebApiConfig.cs**檔案。 在  *WebApiConfig*類別，您會發現與 Web API，相關聯的對應 HTTP 組態將路由傳送至**Web API 控制器**。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. 開啟**RouteConfig.cs**檔案。 內部*RegisterRoutes*方法，您會發現與 MVC，這會對應至 HTTP 路由相關聯的組態**MVC 控制器**。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>工作 2-執行解決方案

在這項工作中，您將會執行產生的方案，瀏覽應用程式和其部分功能，例如重新撰寫 URL 和內建的驗證。

1. 若要執行此方案，請按**F5**或按**開始**按鈕位於工具列上。 應用程式首頁應該會在瀏覽器中開啟。

    ![執行解決方案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. 確認 Web Form 網頁，會叫用。 若要這樣做，請附加 **/contact.aspx** url 網址列，然後按下**Enter**。

    ![易記 Url](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *易記 Url*

    > [!NOTE]
    > 如您所見，URL 變更為 **/連絡**。 從開始**ASP.NET 4**、 URL 路由的功能已新增至 Web Form、 Url，您可以撰寫喜歡*[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* 而不是 *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. 如需詳細資訊請參閱[URL 路由](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md)。
3. 現在，您將探索整合到應用程式的驗證流程。 若要這樣做，請按一下**註冊**頁面右上角。

    ![註冊新的使用者](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *註冊新的使用者*
4. 在**註冊**頁面上，輸入**使用者名稱**並**密碼**，然後按一下**註冊**。

    ![註冊頁面](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *註冊頁面*
5. 應用程式註冊新的帳戶，並驗證使用者。

    ![已驗證的使用者](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *已驗證的使用者*
6. 請返回 Visual Studio，然後按**SHIFT + F5**停止偵錯。

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>練習 2： 建立使用 Scaffolding MVC 控制器

在這個練習中您會利用 Visual Studio 建立 ASP.NET MVC 5 控制器與動作和 Razor 檢視，以執行 CRUD 作業，而不需要撰寫一行程式碼提供的 ASP.NET Scaffold 架構。 Scaffolding 程序將使用 Entity Framework Code First 來產生 SQL database 中的 資料內容和資料庫結構描述。

**有關 Entity Framework Code First**

Entity Framework (EF) 是物件關聯式對應程式 (ORM)，可讓您使用而不是直接使用關聯式儲存結構描述的程式設計概念應用程式模型進行程式設計來建立資料存取應用程式。

Entity Framework Code First 模型化工作流程可讓您使用您自己的網域類別代表模型執行查詢時，EF 相依於變更追蹤和更新函式。 使用 Code First 開發工作流程，您不需要藉由建立資料庫，或指定的結構描述開始您的應用程式。 相反地，您可以撰寫標準的.NET 類別可定義應用程式最適當的網域模型物件和 Entity Framework 會為您建立資料庫。

> [!NOTE]
> 您可以深入了解 Entity Framework[此處](../../../entity-framework.md)。


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>工作 1-建立新模型

您現在將會定義**人員**類別，將會用來建立 MVC 控制器和檢視的 scaffolding 程序的模型。 您會先建立**人員**模型類別，在控制器中的 CRUD 作業將會自動建立和使用 scaffolding 功能。

1. 開啟**Visual Studio Express 2013 for Web**並**MyHybridSite.sln**解決方案位於**來源/Ex2-MvcScaffolding/開始**資料夾。 或者，您可以繼續使用解決方案您在前一個練習中取得。
2. 在 [**方案總管] 中**，以滑鼠右鍵按一下**模型**資料夾**MyHybridSite**專案，然後選取**新增 |類別...**.

    ![新增人員模型類別](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *新增人員模型類別*
3. 在 [**加入新項目**] 對話方塊中，將檔案命名*Person.cs*然後按一下**新增**。

    ![建立 Person 模型類別](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *建立 Person 模型類別*
4. 取代的內容**Person.cs**為下列程式碼的檔案。 按下**CTRL + S**以儲存變更。

    (程式碼片段- *BringingTogetherOneAspNet-Ex2-PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. 在 [**方案總管] 中**，以滑鼠右鍵按一下**MyHybridSite**專案，然後選取**建置**，或按**CTRL + SHIFT + B**來建置專案。

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>工作 2-建立 MVC 控制器

既然**Person**建立模型，您將使用 Entity Framework 使用 ASP.NET MVC scaffolding，若要建立的 CRUD 控制器動作和檢視**人員**。

1. 在 [**方案總管] 中**，以滑鼠右鍵按一下**控制站**資料夾**MyHybridSite**專案，然後選取**新增 |新增 Scaffold 項目...**.

    ![建立新的 scaffold 的控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *建立新的 Scaffold 控制器*
2. 在 **新增 Scaffold**對話方塊中，選取**使用 MVC 5 控制器與檢視，Entity Framework** ，然後按一下 **新增。**

    ![選取具有檢視和 Entity Framework 的 MVC 5 控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *選取具有檢視和 Entity Framework 的 MVC 5 控制器*
3. 設定*MvcPersonController*作為**控制站名稱**，選取**使用非同步控制器動作**選項，然後選取**人員 (MyHybridSite.Models)** 作為**模型類別**。

    ![新增 scaffolding MVC 控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *新增 scaffolding MVC 控制器*
4. 底下**資料內容類別**，按一下 **新資料內容...**.

    ![建立新的資料內容](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *建立新的資料內容*
5. 在 **新的資料內容** 對話方塊中，名稱的新資料內容*PersonContext* ，按一下 **新增**。

    ![建立新的 PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *建立新的 PersonContext 類型*
6. 按一下 **新增**若要建立的新控制器**人員**使用 scaffolding。 Visual Studio 接著會產生控制器動作、 個人資料內容和 Razor 檢視。

    ![在之後使用 scaffolding 建立 MVC 控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *在之後使用 scaffolding 建立 MVC 控制器*
7. 開啟**MvcPersonController.cs**中的檔案**控制站**資料夾。 請注意有自動產生的 CRUD 動作方法。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > 藉由選取**使用非同步控制器動作**從 scaffolding 的核取方塊選項，在先前步驟中，Visual Studio 產生的所有動作涉及之存取權的個人資料內容的非同步動作方法。 建議您用於長時間執行非同步動作方法，不受限於 CPU 以避免封鎖在處理要求時執行工作的 Web 伺服器的要求。

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>工作 3-執行解決方案

在這個工作中，您將執行一次以確認解決方案的檢視**人員**會如預期般運作。 您將加入新的對方，以確認它已成功儲存到資料庫。

1. 按下**F5**執行方案。
2. 瀏覽至 **/MvcPerson**。 包含 scaffold 的檢視，其顯示的人員清單應該會出現。
3. 按一下 **新建**來新增新的人員。

    ![瀏覽至包含 scaffold 的 MVC 檢視](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *瀏覽至包含 scaffold 的 MVC 檢視*
4. 在**建立**檢視中，提供**名稱**並**年齡**人，然後按一下 **建立**。

    ![加入新的人員](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *加入新的人員*
5. 新的人員新增至清單。 在 [項目] 清單中，按一下**詳細資料**顯示連絡人的詳細資料檢視。 然後，在**詳細資料**檢視中，按一下**回到清單**返回清單檢視。

    ![人員詳細資料檢視](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *人員詳細資料檢視*
6. 按一下 **刪除**若要刪除之人員的連結。 在 **刪除**檢視中，按一下**刪除**以確認此作業。

    ![刪除使用者](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *刪除使用者*
7. 請返回 Visual Studio，然後按**SHIFT + F5**停止偵錯。

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>練習 3： 建立使用 Scaffolding Web API 控制器

Web API 架構是 ASP.NET 堆疊的一部分，並可輕鬆實作 HTTP 服務，通常傳送和接收 JSON 或 XML 格式的資料，透過 RESTful API。

在此練習中，您將使用 ASP.NET Scaffolding 一次產生 Web API 控制器。 您將使用相同**Person**並**PersonContext**從先前的練習，以提供相同的個人資料，以 JSON 格式的類別。 您會看到如何公開相同的資源，以及在相同的 ASP.NET 應用程式內不同的方式。

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>工作 1-建立 Web API 控制器

在這項工作會建立新**Web API 控制器**，將會公開 （expose) 電腦可使用格式 JSON 等個人資料。

1. 如果尚未開啟，請開啟**Visual Studio Express 2013 for Web** ，然後開啟**MyHybridSite.sln**解決方案位於**來源/Ex3-WebAPI/開始**資料夾。 或者，您可以繼續使用解決方案您在前一個練習中取得。

    > [!NOTE]
    > 如果您開始從練習 3 開始方案時，請按**CTRL + SHIFT + B**建置方案。
2. 在 [**方案總管] 中**，以滑鼠右鍵按一下**控制站**資料夾**MyHybridSite**專案，然後選取**新增 |新增 Scaffold 項目...**.

    ![建立新的 scaffold 的控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *建立新的 scaffold 的控制器*
3. 在**新增 Scaffold**對話方塊中，選取**Web API**左窗格中，然後**Web API 2 控制器與動作，使用 Entity Framework**在中間窗格中，然後按一下  **新增。**

    ![選取 Web API 2 控制器與動作和 Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Entity Framework 與動作選取 Web API 2 控制器")

    *選取 Web API 2 控制器與動作和 Entity Framework*
4. 設定*ApiPersonController*作為**控制站名稱**，選取**使用非同步控制器動作**選項，然後選取**人員 (MyHybridSite.Models)** 並**PersonContext (MyHybridSite.Models)** 作為**模型**並**資料內容**分別為類別。 然後按一下 [加入] 。

    ![新增 scaffolding Web API 控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "新增 scaffolding Web API 控制器")

    *新增 scaffolding Web API 控制器*
5. Visual Studio 接著會產生**ApiPersonController**執行四個的 CRUD 動作，以處理資料的類別。

    ![使用 scaffolding 建立 Web API 控制器之後](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "之後使用 scaffolding 建立 Web API 控制器")

    *在之後使用 scaffolding 建立 Web API 控制器*
6. 開啟**ApiPersonController.cs**檔案，並檢查*GetPeople*動作方法。 這個方法會查詢的資料庫欄位**PersonContext**以取得使用者的資料類型。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. 現在注意到方法定義上方的註解。 它會提供您將在下一個工作使用此動作公開 （expose) 的 URI。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > 根據預設，Web API 已攔截到的查詢 */api*路徑，以避免與 MVC 控制器的衝突。 如果您需要變更此設定，請參閱[ASP.NET Web API 中的路由](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md)。

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>工作 2-執行解決方案

在這項工作中，您將使用 Internet Explorer **F12 開發人員工具**檢查 Web API 控制器的完整回應。 您會看到如何您也可以擷取網路流量，以取得您的應用程式資料的更多見解。

> [!NOTE]
> 請確定**Internet Explorer**中選取**開始**位於 Visual Studio 工具列上的按鈕。
> 
> ![Internet Explorer 選項](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> **F12 開發人員工具**有一組廣泛的並未涵蓋此實際操作實驗室的功能。 如果您想要深入了解，請參閱[使用 F12 開發人員工具](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85))。


1. 按下**F5**執行方案。

    > [!NOTE]
    > 若要正確遵循這項工作中，您的應用程式需要有資料。 如果您的資料庫是空的您可以返回練習 2 中的工作 3，並遵循上如何建立使用 MVC 檢視的新使用者的步驟。
2. 在瀏覽器中，按下**F12**來開啟**開發人員工具**面板。 按下**CTRL** + **4** ，或按一下**網路**圖示，然後按一下綠色箭號按鈕以開始擷取網路流量。

    ![起始 Web API 網路擷取](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "起始 Web API 網路擷取")

    *起始的 Web API 網路擷取*
3. 附加**api/ApiPerson**至瀏覽器的網址列中的 URL。 您現在會檢查來自回應的詳細資料**ApiPersonController**。

    ![擷取使用者資料透過 Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "擷取透過 Web API 的個人資料")

    *擷取透過 Web API 的個人資料*

    > [!NOTE]
    > 下載完成後，系統會提示您使用下載的檔案進行的動作。 讓對話方塊保持開啟，才能夠觀賞回應內容，透過開發人員工具視窗。
4. 現在，您將會檢查回應的主體。 若要這樣做，請按一下**詳細資料**索引標籤，然後按一下**回應主體**。 您可以檢查已下載的資料是否具有屬性的物件清單**識別碼**，**名稱**並**年齡**對應到**人員**類別。

    ![檢視 Web API 回應主體](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "檢視 Web API 回應主體")

    *檢視 Web API 回應主體*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>工作 3-新增 Web API 說明頁面

當您建立 Web API 時，最好建立說明頁面，讓其他開發人員都知道如何呼叫您的 API。 您可以建立並手動更新文件頁面，但最好是自動產生以避免需要進行的維護工作。 在這項工作中，您將使用的 Nuget 套件來自動產生方案的 Web API 說明頁面。

1. 從**工具**功能表，在 Visual Studio 中，選取**程式庫套件管理員**，然後按一下**Package Manager Console**。
2. 在 [ **Package Manager Console** ] 視窗中，執行下列命令：

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > **Microsoft.AspNet.WebApi.HelpPage**套件會安裝必要的組件，並且新增 MVC 檢視底下的 [說明] 頁**區域/HelpPage**資料夾。

    ![HelpPage 區域](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage 區域")

    *HelpPage 區域*
3. 根據預設，說明 頁面都有文件的預留位置字串。 若要建立文件，您可以使用 XML 文件註解。 若要啟用這項功能，請開啟**HelpPageConfig.cs**檔案位於**應用程式的區域/HelpPage\_啟動**資料夾並取消註解下面這一行：

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. 在 [**方案總管] 中**，以滑鼠右鍵按一下專案**MyHybridSite**，選取**屬性**，按一下 [**建置**] 索引標籤。

    ![建置索引標籤](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "建置區段")

    *建置索引標籤*
5. 底下**輸出**，選取**XML 文件檔案**。 在 [編輯] 方塊中，鍵入**應用程式\_Data/XmlDocument.xml**。

    ![輸出 [建置] 索引標籤中的一節](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "輸出區段中 [建置] 索引標籤")

    *在 [建置] 索引標籤的 [輸出] 區段*
6. 按下**CTRL** + **S**以儲存變更。
7. 開啟**ApiPersonController.cs**檔案**控制站**資料夾。
8. 輸入新的一行之間*GetPeople*方法簽章並有 */ / 取得 api/ApiPerson*註解，然後輸入 三個正斜線。

    > [!NOTE]
    > Visual Studio 會自動插入的 XML 項目會定義方法的文件。
9. 新增摘要的文字和傳回值*GetPeople*方法。 它看起來應該如下所示。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. 按下**F5**執行方案。
11. 附加 **/help** [網址] 列中的 url，瀏覽至 [說明] 頁面。

    ![ASP.NET Web API 說明頁面](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API 說明頁面")

    *ASP.NET Web API 說明頁面*

    > [!NOTE]
    > 主頁面的內容是依控制站的 api 的資料表。 資料表項目使用，以動態方式產生**IApiExplorer**介面。 如果您新增或更新 API 控制器，資料表會自動更新您建置應用程式在下一次。
    > 
    > **API**欄會列出的 HTTP 方法和相對 URI。 **描述**資料行包含已從方法的文件中擷取的資訊。
12. 請注意您新增上述的方法定義的描述會顯示在 [描述] 欄中。

    ![API 方法描述](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API 方法的描述")

    *API 方法的描述*
13. 按一下其中一個 API 方法，以便瀏覽至含有更多詳細資訊，包括範例回應主體的頁面。

    ![詳細資料 頁](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "詳細資訊 頁面")

    *詳細的資訊 頁面*

* * *

<a id="Summary"></a>
## <a name="summary"></a>總結

藉由完成這個實際操作實驗室，您已經學會如何：

- 建立新的 Web 應用程式，Visual Studio 2013 中使用一個 ASP.NET 使用經驗
- 將多個 ASP.NET 技術整合到一個單一專案
- 從您的模型類別使用的 ASP.NET Scaffold 產生 MVC 控制器和檢視
- 產生使用功能，例如非同步程式設計，並透過 Entity Framework 資料存取的 Web API 控制器
- 自動產生 Web API 說明頁面，您的控制站
