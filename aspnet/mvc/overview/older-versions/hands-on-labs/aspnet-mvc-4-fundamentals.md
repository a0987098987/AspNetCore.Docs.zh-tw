---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 基本概念 |Microsoft Docs
author: rick-anderson
description: 這個實際操作實驗室根據 MVC （模型檢視控制器） 音樂市集，介紹，並逐步說明如何使用 ASP.NET MV 的教學課程應用程式...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 2b3f8916bdca1df0dd2855f02ae46f5e5d13311a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819967"
---
# <a name="aspnet-mvc-4-fundamentals"></a>ASP.NET MVC 4 基本概念

藉由[Web Camp 小組](https://twitter.com/webcamps)

[下載 Web 研討會訓練套件](https://aka.ms/webcamps-training-kit)

這個實際操作實驗室是以 MVC （模型檢視控制器） 音樂市集，教學課程的應用程式的介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 為基礎。 在實驗室中，您將學習簡單起見，尚未一起使用這些技術的電源。 您將使用簡單的應用程式啟動，而且將會建置它除非您有功能完整 ASP.NET MVC 4 Web 應用程式。

這個實驗室會搭配 ASP.NET MVC 4。

如果您想要探索 ASP.NET MVC 3 版的教學課程的應用程式，您可以找到它[MVC Music 市集](https://github.com/evilDave/MVC-Music-Store)。

這個實際操作實驗室會假設，開發人員會有 Web 開發技術，例如 HTML 和 JavaScript 的經驗。

> [!NOTE]
> 所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 這個實驗室中的特定專案將會位於[ASP.NET MVC 4 基本概念](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals)。

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>音樂市集應用程式

將會建置在本實驗室中的音樂市集 web 應用程式包含三個主要部分： 購物、 簽出，以及系統管理。 訪客可以依內容類型瀏覽專輯、 將專輯新增至購物車、 檢閱其選取項目和最後位進行結帳，登入並完成訂購。 此外，儲存區系統管理員將能夠管理可用的相簿，以及其主要屬性。

![音樂市集畫面](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store 畫面")

*Music Store 畫面*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 基本資訊

音樂市集 」 應用程式會使用來建置**模型檢視控制器 (MVC)**，分隔成三個主要元件的應用程式的架構模式：

- **模型**： 模型物件屬於實作網域邏輯應用程式的組件。 通常，模型物件也擷取，並將模型狀態儲存在資料庫中。
- **檢視：** 檢視是顯示應用程式的使用者介面 (UI) 的元件。 一般而言，此 UI 是從模型資料的方式建立。 範例會顯示文字方塊和依相簿物件的目前狀態的下拉式清單編輯檢視的專輯。
- **控制站：** 控制器就是元件，以處理使用者互動、 操作模型，並最終選取要轉譯的 UI 的檢視。 在 MVC 應用程式中，檢視只會顯示資訊，而控制器則會處理及回應使用者輸入和互動。

MVC 模式可協助您建立不同的應用程式 （輸入的邏輯、 商務邏輯和 UI 邏輯），同時提供這些項目之間的鬆散結合的不同層面的應用程式。 這種區隔可協助您管理複雜度，當您建置應用程式，因為它可讓您專注於一次實作的其中一個層面。 此外，MVC 模式可讓您輕鬆地測試應用程式，也鼓勵使用測試導向開發 (TDD)，來建立應用程式。

**ASP.NET MVC**架構建立 ASP.NET MVC 為基礎的 Web 應用程式提供的 ASP.NET Web Form 模式的替代方案。 **ASP.NET MVC** framework 是一種輕量型、 可高度測試的展示架構，（就像 Web form 架構應用程式） 整合了現有的 ASP.NET 功能，例如主版頁面和成員資格為基礎驗證讓您取得核心.NET framework 的所有功能。 這非常有用，如果您已熟悉 ASP.NET Web Form 因為您已使用的所有程式庫也可使用 ASP.NET MVC 4 中。

此外，MVC 應用程式的三個主要元件之間的鬆散結合也會提升平行開發。 比方說，開發人員能夠檢視、 控制器邏輯，可以處理第二個開發人員和第三個開發人員可以專注於商務邏輯模型中。

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將了解如何：

- 根據音樂市集 」 應用程式教學課程從頭開始建立 ASP.NET MVC 應用程式
- 新增控制站來處理至站台和瀏覽其主要功能的首頁的 Url
- 加入檢視，以自訂其樣式以及顯示的內容
- 新增模型類別來包含和管理資料和網域邏輯
- 將從控制器動作的資訊傳遞至檢視範本中使用檢視模型模式
- 瀏覽 ASP.NET MVC 4 的新範本的網際網路應用程式

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

您必須具備下列項目，即可完成此實驗室：

- [Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (讀取[附錄 A](#AppendixA)如需有關如何安裝)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

**安裝程式碼片段**

為了方便起見，大部分的程式碼，您將沿著這個實驗室管理可從 Visual Studio 程式碼片段。 若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。

如果您不熟悉 Visual Studio 程式碼片段，而且想要了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 c： 使用程式碼片段](#AppendixC)&quot;。

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室是由下列練習所組成的：

1. [練習 1： 建立 MusicStore ASP.NET MVC Web 應用程式專案](#Exercise1)
2. [練習 2： 建立控制器](#Exercise2)
3. [練習 3： 將參數傳遞至控制器](#Exercise3)
4. [練習 4： 建立檢視](#Exercise4)
5. [練習 5： 建立檢視模型](#Exercise5)
6. [練習 6： 在檢視中使用參數](#Exercise6)
7. [練習 7: 瀏覽 ASP.NET MVC 4 的新範本](#Exercise7)

> [!NOTE]
> 每個練習會伴隨**結束**包含完成練習之後，您應該取得所產生的方案資料夾。 如果您需要的所有練習所使用的其他說明，您可以使用此解決方案作為指南。


完成這個實驗室估計時間： **60 分鐘**。

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>練習 1： 建立 MusicStore ASP.NET MVC Web 應用程式專案

在此練習中，您將學習如何在 Visual Studio 2012 Express for Web，以及其主要資料夾組織建立 ASP.NET MVC 應用程式。 此外，您將學習如何加入新的控制器，並讓它在應用程式的首頁上顯示一個簡單的字串。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>工作 1-建立 ASP.NET MVC Web 應用程式專案

1. 在這個工作中，您將建立空白 ASP.NET MVC 應用程式專案中使用 MVC 的 Visual Studio 範本。 開始**VS Express for Web**。
2. 按一下 [檔案] 功能表上的 [新增專案]。
3. 在 [**新的專案**] 對話方塊中選取**ASP.NET MVC 4 Web 應用程式**專案類型，位於**Visual C#** **Web**範本清單。
4. 變更**名稱**要*MvcMusicStore*。
5. 設定內的新方案的位置**開始**在這個練習中的來源資料夾中，如範例的資料夾 **[您-HOL-PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**。 按一下 [確定 **Deploying Office Solutions**]。

    ![建立新專案 對話方塊](aspnet-mvc-4-fundamentals/_static/image2.png "建立新專案 對話方塊")

    *建立新專案 對話方塊*
6. 在 [**新的 ASP.NET MVC 4 專案**] 對話方塊中選取**基本**範本並確定**檢視引擎**選取**Razor**。 按一下 [確定 **Deploying Office Solutions**]。

    ![新 ASP.NET MVC 4 專案 對話方塊](aspnet-mvc-4-fundamentals/_static/image3.png "新 ASP.NET MVC 4 專案 對話方塊")

    *新 ASP.NET MVC 4 專案 對話方塊*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>工作 2-瀏覽方案結構

ASP.NET MVC 架構包括 Visual Studio 專案範本，可協助您建立支援 MVC 模式的 Web 應用程式。 此範本會建立新的 ASP.NET MVC Web 應用程式具有必要的資料夾、 項目範本和組態檔項目。

在這個工作中，您將檢查方案結構，以了解所涉及的項目和其關聯性。 下列資料夾會包含在所有 ASP.NET MVC 應用程式，因為預設的 ASP.NET MVC 架構會使用&quot;慣例優於組態&quot;方法，並讓一些預設假設根據資料夾名稱慣例。

1. 一旦建立專案時，檢閱已在右側的 [方案總管] 中建立的資料夾結構：

    ![在 [方案總管] 中的 ASP.NET MVC 資料夾結構](aspnet-mvc-4-fundamentals/_static/image4.png "方案總管 中的 ASP.NET MVC 資料夾結構")

    *在 [方案總管] 中的 ASP.NET MVC 資料夾結構*

   1. **控制站**。 這個資料夾將包含控制器類別。 MVC 型應用程式中，控制器是負責處理使用者互動、 操作模型，以及最終選擇一種檢視來呈現 UI。

       > [!NOTE]
       > MVC framework 需要結尾的所有控制器的名稱&quot;控制器&quot;-例如 HomeController、 LoginController 或 ProductController。
   2. **模型**。 代表 MVC Web 應用程式的應用程式模型會提供此資料夾。 這通常包含定義物件和資料存放區與互動的邏輯的程式碼。 一般而言，實際的模型物件會在不同的類別庫中。 不過，當您建立新的應用程式時，您可能會包含類別，並再將它們移到不同的類別庫，在稍後的時間點，在開發週期。
   3. **檢視**。 此資料夾是建議放置檢視，負責顯示應用程式的使用者介面元件的位置。 檢視使用.aspx、.ascx、.cshtml 和.master 檔案，除了與呈現檢視相關的任何其他檔案。 Views 資料夾會包含每個控制站; 所需的資料夾控制器名稱的前置詞的命名的資料夾。 例如，如果您有名稱為控制器**HomeController**，[Views] 資料夾會包含名為 Home 的資料夾。 根據預設，當 ASP.NET MVC 架構載入檢視時，它會尋找要求的檢視名稱 Views\controllerName 資料夾中的.aspx 檔案 (**檢視 [ControllerName] [Action].aspx**) 或 (**檢視 [ControllerName][動作].cshtml**) 的 Razor 檢視。

      > [!NOTE]
      > 除了先前所列的資料夾，MVC Web 應用程式會使用**Global.asax**檔案來設定全域 URL 路由預設值，並使用**Web.config**檔案來設定應用程式。

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>工作 3-新增 HomeController

在不使用 MVC framework 的 ASP.NET 應用程式，使用者互動會圍繞網頁以及引發和處理從這些網頁事件加以組織。 相反地，ASP.NET MVC 應用程式的使用者互動會圍繞控制器和動作方法加以組織。

相反地，ASP.NET MVC 架構會將 Url 對應至稱為控制器的類別。 控制站處理連入要求、 處理使用者輸入和互動、 執行適當的應用程式邏輯並決定要傳送回用戶端的回應 （顯示 HTML、 下載檔案、 重新導向至不同的 URL，依此類推）。 在顯示 HTML，控制器類別通常會呼叫不同的檢視元件來產生要求的 HTML 標記。 在 MVC 應用程式中，檢視只會顯示資訊，而控制器則會處理及回應使用者輸入和互動。

在這個工作中，您將加入的音樂市集 」 網站的 [首頁] 頁面來處理 Url 的控制器類別。

1. 以滑鼠右鍵按一下**控制器**在 [方案總管] 中選取的資料夾**新增**，然後**控制器**命令：

    ![新增控制器指令](aspnet-mvc-4-fundamentals/_static/image5.png "新增控制器命令")

    *新增控制器 命令*
2. **新增控制器**對話方塊隨即出現。 控制器*HomeController*然後按**新增**。

    ![新增控制器 對話方塊](aspnet-mvc-4-fundamentals/_static/image6.png "新增控制器 對話方塊")

    *新增控制器 對話方塊*
3. 檔案**HomeController.cs**中建立**控制站**資料夾。 才能**HomeController**其索引的動作傳回的字串，取代**索引**為下列程式碼的方法：

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex1 HomeController 索引*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>工作 4-執行應用程式

在這個工作中，您可以嘗試在網頁瀏覽器應用程式。

1. 按下**F5**執行應用程式。 編譯專案，並在本機 IIS Web 伺服器啟動。 本機 IIS Web 伺服器會自動開啟 web 瀏覽器並指向 Web 伺服器的 URL。

    ![在網頁瀏覽器中執行的應用程式](aspnet-mvc-4-fundamentals/_static/image7.png "在網頁瀏覽器中執行的應用程式")

    *在網頁瀏覽器中執行的應用程式*

    > [!NOTE]
    > 本機 IIS Web 伺服器會執行網站上的隨機的可用連接埠號碼。 在上圖中，站台執行在`http://localhost:50103/`，因此它使用連接埠 50103。 連接埠號碼可能有所不同。
2. 關閉瀏覽器。

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>練習 2： 建立控制器

在此練習中，您將學習如何控制站來實作簡單的音樂市集應用程式的功能更新。 該控制器會定義處理每個下列的特定要求的動作方法：

- 在 Music Store 中的音樂內容類型的清單頁面
- 列出所有針對特定的內容類型的音樂 album 瀏覽 頁面
- 詳細資料頁面，其中顯示有關特定音樂專輯資訊

本練習的範圍，這些動作只會傳回字串，到目前為止。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>工作 1-加入新的 StoreController 類別

在這個工作中，您將加入新的控制器。

1. 如果尚未開啟，啟動**VS Express for Web 2012**。
2. 在 **檔案**功能表上，選擇**開啟專案**。 在 [開啟專案] 對話方塊中，瀏覽至**Source\Ex02 CreatingAController\Begin**，選取**Begin.sln**然後按一下**開啟**。 或者，您可以繼續使用解決方案取得完成前一個練習之後。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
3. 加入新的控制器。 若要這樣做，請以滑鼠右鍵按一下**控制器**在 [方案總管] 中選取的資料夾**新增**，然後**控制器**命令。 變更**控制器名稱**要*StoreController*，然後按一下**新增**。

    ![新增控制器 對話方塊](aspnet-mvc-4-fundamentals/_static/image8.png "新增控制器 對話方塊")

    *新增控制器 對話方塊*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>工作 2-修改 StoreController 的動作

在這個工作中，您將修改呼叫控制器方法**動作**。 動作是負責處理 URL 要求，並判斷應該傳送回瀏覽器或叫用的 URL 的使用者的內容。

1. **StoreController**類別已經有**Index**方法。 您將會稍後在本實驗室中使用它來實作，其中列出所有的內容類型的音樂市集頁面。 現在，只要取代**Index**方法，以下列程式碼會傳回字串&quot;Hello from Store.Index()&quot;:

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex2 StoreController 索引*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. 新增**瀏覽**並**詳細資料**方法。 若要這樣做，請新增下列程式碼**StoreController**:

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>工作 3-執行應用程式

在這個工作中，您可以嘗試在網頁瀏覽器應用程式。

1. 按下**F5**執行應用程式。
2. 在專案開始**首頁**頁面。 變更 URL，以確認每個動作的實作。

    1. **/ 儲存**。 您會看到 **&quot;Hello from Store.Index()&quot;**。
    2. **/ Store/瀏覽**。 您會看到 **&quot;Hello from Store.Browse()&quot;**。
    3. **/ Store/Details**。 您會看到 **&quot;Hello from Store.Details()&quot;**。

        ![瀏覽 StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "瀏覽 StoreBrowse")

        *瀏覽 /Store/Browse*
3. 關閉瀏覽器。

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>練習 3： 將參數傳遞至控制器

到目前為止，您有已傳回常數字串中的控制器。 在這個練習中，您將學習如何將參數傳遞至控制站使用的 URL 和查詢字串，，然後以文字回應到瀏覽器的方法動作。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>工作 1-StoreController 新增內容類型參數

在這個工作中，您將使用**querystring**傳送至的參數**瀏覽**中的動作方法**StoreController**。

1. 如果尚未開啟，啟動**VS Express for Web**。
2. 在 **檔案**功能表上，選擇**開啟專案**。 在 [開啟專案] 對話方塊中，瀏覽至**Source\Ex03 PassingParametersToAController\Begin**，選取**Begin.sln**然後按一下**開啟**。 或者，您可以繼續使用解決方案取得完成前一個練習之後。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
3. 開啟**StoreController**類別。 若要這樣做，請在**方案總管**，展開**控制站**資料夾，然後按兩下**StoreController.cs**。
4. 變更**瀏覽**方法，將字串參數來要求特定的內容類型。 ASP.NET MVC 會自動傳遞任何查詢字串或表單張貼參數，叫做**genre**叫用時，此動作方法。 若要這樣做，請取代**瀏覽**為下列程式碼的方法：

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> 您使用**HttpUtility.HtmlEncode**公用程式方法，以防止使用者插入檢視中的 Javascript，例如連結  **/Store/瀏覽？內容類型 =&lt;指令碼&gt;window.location = '[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**。
> 
> 如需進一步說明，請瀏覽[這篇 msdn 文章](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx)。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>工作 2-執行應用程式

在這個工作中，您將會嘗試在網頁瀏覽器應用程式，並使用**genre**參數。

1. 按下**F5**執行應用程式。
2. 在專案開始**首頁**頁面。 將 URL 變更為  */儲存] / [瀏覽？內容類型 = Disco*以確認該動作所接收的內容類型參數。

    ![瀏覽 StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "瀏覽 StoreBrowseGenre = Disco")

    *瀏覽 /Store/Browse 嗎？內容類型 = Disco*
3. 關閉瀏覽器。

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>工作 3-加入 URL 中內嵌的 Id 參數

在這個工作中，您將使用**URL**傳遞**識別碼**參數**詳細資料**動作方法的**StoreController**。 ASP.NET MVC 的預設路由的慣例是將 URL 區段在動作方法的名稱之後，名為的參數視為**識別碼**。如果動作方法具有名為 Id 參數，然後 ASP.NET MVC 會自動傳遞的 URL 區段您做為參數。 在 URL 中**存放區/詳細資料/5**，**識別碼**會解譯為**5**。

1. 變更**詳細資料**方法**StoreController**，來加入**int**稱為參數**識別碼**。若要這樣做，請取代**詳細資料**為下列程式碼的方法：

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>工作 4-執行應用程式

在這個工作中，您將會嘗試在網頁瀏覽器應用程式，並使用**識別碼**參數。

1. 按下**F5**執行應用程式。
2. 在專案開始**首頁**頁面。 將 URL 變更為 */Store/Details/5*以確認該動作所接收的 id 參數。

    ![瀏覽 StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "瀏覽 StoreDetails5")

    *瀏覽 /Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>練習 4： 建立檢視

到目前為止您有已傳回字串從控制器動作。 雖然這十分有用的方式了解控制器的運作方式，是不實際的 Web 應用程式如何建立。 檢視是提供更好的方法來產生 HTML，回到瀏覽器中使用的範本檔案使用的元件。

在這個練習中，您將了解如何新增版面配置主版頁面，設定一般的 HTML 內容，來增強網站以及最後的檢視範本，以啟用傳回 HTML HomeController 外觀的樣式表的範本。

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>工作 1-修改檔案\_layout.cshtml

檔案 **~/Views/Shared/\_layout.cshtml**可讓您設定整個網站上使用的通用 HTML 的範本。 在這項工作中，要將配置主版頁面的連結的通用標頭加入 首頁 存放區 區域中。

1. 如果尚未開啟，啟動**VS Express for Web**。
2. 在 **檔案**功能表上，選擇**開啟專案**。 在 [開啟專案] 對話方塊中，瀏覽至**Source\Ex04 CreatingAView\Begin**，選取**Begin.sln**然後按一下**開啟**。 或者，您可以繼續使用解決方案取得完成前一個練習之後。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
3. 檔案 <strong>\_layout.cshtml</strong>包含站台上的所有頁面的 HTML 容器配置。 它包含<strong>&lt;html&gt;</strong> HTML 回應中，項目，以及<strong>&lt;head&gt;</strong>並<strong>&lt;主體&gt;</strong>項目。 <strong>@RenderBody（)</strong>內 HTML 主體識別區域該範本可以填入 動態內容的檢視。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. 新增具有連結的通用標頭中的站台的所有頁面的 [首頁和存放區] 區域。 若要這樣做，請新增下列程式碼下方&lt;主體&gt;陳述式。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. 包含要呈現的每個頁面的主體區段的 div。 取代 <strong>@RenderBody（)</strong>下列 higlighted 程式碼: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > 您知道嗎？ Visual Studio 2012 有程式碼片段，讓您輕鬆將常用的程式碼加入 HTML、 程式碼檔案和更多功能 ！ 請試著輸入**&lt;div&gt;** 並按下**索引標籤**兩次來插入完整**div**標記。

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>工作 2-新增的 CSS 樣式表

空白專案範本包含非常簡化的 CSS 檔案只包含用來顯示基本的表單和驗證訊息的樣式。 您將使用其他的 CSS 和映像 （可能是由設計工具提供），以加強網站的外觀與風格。

在這個工作中，您將加入的 CSS 樣式表，來定義網站的樣式。

1. CSS 檔案和映像會包含在**Source\Assets\Content**本實驗室的資料夾。 若要將它們新增至應用程式中，拖曳其內容從**Windows 檔案總管**到視窗**方案總管 中**在 Visual Studio Express for Web，如下所示：

    ![拖曳樣式內容](aspnet-mvc-4-fundamentals/_static/image12.png "拖曳樣式內容")

    *拖曳樣式內容*
2. 警告會出現一個對話方塊，確認是否要取代詢問**Site.css**檔案和一些現有的映像。 請檢查**套用至所有項目**然後按一下**是**。

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>工作 3-新增檢視範本

在這個工作中，您將檢視範本來產生將使用的版面配置的主版頁面的 HTML 回應，並在這個練習中，新增 CSS。

1. 若要瀏覽網站的首頁時，請使用檢視範本，您首先需要而不是傳回字串，表示**HomeController 索引**方法會傳回**檢視**。 開啟**HomeController**類別，並變更其**Index**方法，以傳回**ActionResult**，並且會傳回**View()**。

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex4 HomeController 索引*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. 現在，您需要新增適當的檢視範本。 若要這樣做，請**上按一下滑鼠右鍵**內**Index**動作方法，然後選取**加入檢視**。 此時會出現**加入檢視**對話方塊。

    ![加入從索引方法內的檢視](aspnet-mvc-4-fundamentals/_static/image13.png "加入從索引方法內的檢視")

    *加入從索引方法內的檢視*
3. **加入檢視**對話方塊隨即出現，產生的檢視範本檔案。 根據預設，此對話方塊會預先填入檢視範本的名稱，使其符合使用它的動作方法。 因為您使用**加入檢視**內的操作功能表**索引**HomeController，動作方法**加入檢視**對話方塊具有做為預設檢視名稱的索引。 按一下 [加入] 。

    ![新增檢視 對話方塊](aspnet-mvc-4-fundamentals/_static/image14.png "新增檢視對話方塊")

    *新增檢視對話方塊*
4. Visual Studio 會產生**Index.cshtml**內的檢視範本**Views\Home**資料夾，然後開啟它。

    ![首頁建立的索引檢視](aspnet-mvc-4-fundamentals/_static/image15.png "建立首頁索引檢視")

    *建立主索引檢視*

    > [!NOTE]
    > 名稱和位置**Index.cshtml**檔案與相關，而且依照預設 ASP.NET MVC 的命名慣例。
    > 
    > 資料夾 \Views\**首頁** 符合控制器名稱 (**首頁**控制器)。 檢視範本名稱 (**Index**)，符合將會顯示檢視控制器動作方法。
    > 
    > 如此一來，ASP.NET MVC，就不需要使用此命名慣例，傳回的檢視時，明確指定的名稱或檢視範本的位置。
5. 產生的檢視範本為基礎 **\_layout.cshtml**稍早定義的範本。 要更新 ViewBag.Title 屬性**首頁**，並變更到主要內容**這是首頁**，如下列程式碼所示：

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. 選取  **MvcMusicStore**專案，在 方案總管，然後按**F5**執行應用程式。

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>工作 4： 驗證

若要確認，您已在前一個練習中，正確地執行所有步驟，請繼續進行，如下所示：

在瀏覽器中開啟應用程式時，您應該注意到：

1. HomeController 的索引動作方法找到並顯示**\Views\Home\Index.cshtml**檢視範本，即使呼叫的程式碼**傳回 View()**，因為檢視範本，然後再標準的命名慣例。
2. 首頁會顯示歡迎訊息內定義**\Views\Home\Index.cshtml**檢視範本。
3. 使用首頁 **\_layout.cshtml**範本，因此歡迎訊息內含 HTML 的標準網站的版面配置。

    ![首頁使用的定義 LayoutPage 和樣式的索引檢視](aspnet-mvc-4-fundamentals/_static/image16.png "首頁索引檢視，使用定義 LayoutPage 和樣式")

    *首頁使用的定義 LayoutPage 和樣式的索引檢視*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>練習 5： 建立檢視模型

到目前為止，進行您的檢視顯示硬式編碼的 HTML，但若要建立動態 web 應用程式，檢視範本應該從控制器接收資訊。 要用於此用途的一個常用的技巧是**ViewModel**模式，可讓控制器能夠封裝來產生適當的 HTML 回應所需的所有資訊。

在此練習中，您將了解如何建立 ViewModel 類別，並新增必要的屬性： 的存放區，這些內容類型清單中的內容類型的數目。 您也會更新以使用建立的 ViewModel，StoreController，最後，您將在其中建立新的檢視範本將會顯示在頁面中所述的屬性。

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>工作 1-建立 ViewModel 類別

在這個工作中，您將建立會實作存放區的內容類型清單案例的 ViewModel 類別。

1. 如果尚未開啟，啟動**VS Express for Web**。
2. 在 **檔案**功能表上，選擇**開啟專案**。 在 [開啟專案] 對話方塊中，瀏覽至**Source\Ex05 CreatingAViewModel\Begin**，選取**Begin.sln**然後按一下**開啟**。 或者，您可以繼續使用解決方案取得完成前一個練習之後。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
3. 建立**Viewmodel**資料夾以保存 ViewModel。 若要這樣做，以滑鼠右鍵按一下最上層**MvcMusicStore**專案，然後選取**新增**，然後**新資料夾**。

    ![加入新的資料夾](aspnet-mvc-4-fundamentals/_static/image17.png "新增新的資料夾")

    *加入新的資料夾*
4. 將資料夾命名*Viewmodel*。

    ![在 [方案總管] 中的 Viewmodel 資料夾](aspnet-mvc-4-fundamentals/_static/image18.png "方案總管 中的 Viewmodel 資料夾")

    *在 [方案總管] 中的 Viewmodel 資料夾*
5. 建立**ViewModel**類別。 若要這樣做，以滑鼠右鍵按一下**Viewmodel**最近建立資料夾，選取**新增**，然後**新項目**。 底下**程式碼**，選擇**類別**項目，然後將檔案命名為*StoreIndexViewModel.cs*，然後按一下**新增**。

    ![加入新的類別](aspnet-mvc-4-fundamentals/_static/image19.png "加入新的類別")

    *加入新的類別*

    ![建立 StoreIndexViewModel 類別](aspnet-mvc-4-fundamentals/_static/image20.png "建立 StoreIndexViewModel 類別")

    *建立 StoreIndexViewModel 類別*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>工作 2-將屬性新增到 ViewModel 類別

有兩個參數，傳遞從 StoreController 至檢視範本以產生預期的 HTML 回應： 的存放區，這些內容類型清單中的內容類型的數目。

在這個工作中，您將這些 2 將屬性新增至**StoreIndexViewModel**類別： **NumberOfGenres** （整數） 和**內容類型**（字串的清單）。

1. 新增**NumberOfGenres**並**內容類型**屬性，以**StoreIndexViewModel**類別。 若要這樣做，請在類別定義中加入下列 2 個幾行：

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex5 StoreIndexViewModel 屬性*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> **{Get; 設定;}** 標記法會利用 C# 的自動實作屬性 」 功能。 它提供屬性的優點，而不需要我們宣告支援欄位。

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>工作 3-使用 StoreIndexViewModel 更新 StoreController

**StoreIndexViewModel**類別會封裝將從所需的資訊**StoreController**的**索引**檢視範本，以便產生回應的方法.

在這個工作中，您將會更新**StoreController**使用**StoreIndexViewModel**。

1. 開啟**StoreController**類別。

    ![開啟 StoreController 類別](aspnet-mvc-4-fundamentals/_static/image21.png "開啟 StoreController 類別")

    *開啟 StoreController 類別*
2. 若要使用**StoreIndexViewModel**從類別**StoreController**，在頂端加入下列命名空間**StoreController**程式碼：

    (程式碼片段- *ASP.NET MVC 4 基本概念-使用 Viewmodel Ex5 StoreIndexViewModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. 變更**StoreController**的**Index**動作方法，讓它建立並填入**StoreIndexViewModel**物件，並再將它，傳遞至檢視範本產生 HTML 回應使用它。

    > [!NOTE]
    > 在實驗室&quot;ASP.NET MVC 模型和資料存取&quot;您將撰寫程式碼，從資料庫擷取存放區的內容類型清單。 在下列程式碼中，您將建立**清單**假資料內容類型，將會填入**StoreIndexViewModel**。
    > 
    > 建立及設定後**StoreIndexViewModel**物件，它會當做引數傳遞給**檢視**方法。 這表示在檢視範本會使用該物件來產生 HTML 回應使用它。
4. 取代**Index**為下列程式碼的方法：

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex5 StoreController Index 方法*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> 如果您不熟悉使用 C#，您可能會假設使用**var**表示**viewModel**變數是晚期繫結。 不正確的-C# 編譯器判斷使用根據您指派給變數的型別推斷**viewModel**別的**StoreIndexViewModel**。 此外，藉由編譯本機**viewModel**變數設定為**StoreIndexViewModel** get 編譯時期檢查和 Visual Studio 程式碼編輯器支援類型。

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>工作 4-建立使用 StoreIndexViewModel 的檢視範本

在這個工作中，您將建立會使用從控制器傳遞 StoreIndexViewModel 物件顯示的內容類型清單的檢視範本。

1. 建立新的檢視範本之前, 我們將建置專案，讓**新增檢視 對話方塊**知道**StoreIndexViewModel**類別。 建置專案，方法是選取**建置**功能表項目，然後**建置 MvcMusicStore**。

    ![建置專案](aspnet-mvc-4-fundamentals/_static/image22.png "建置專案")

    *建置專案*
2. 建立新的檢視範本。 若要這樣做，請以滑鼠右鍵按一下**Index**方法，然後選取**加入檢視**。

    ![新增檢視](aspnet-mvc-4-fundamentals/_static/image23.png "新增檢視")

    *新增檢視*
3. 因為**新增檢視 對話方塊**從叫用**StoreController**，它會將檢視範本新增預設會在**\Views\Store\Index.cshtml**檔案。 請檢查**建立強型別-檢視**核取方塊，然後選取**StoreIndexViewModel**作為**模型類別**。 此外，請確定選取的檢視引擎**Razor**。 按一下 [加入] 。

    ![新增檢視 對話方塊](aspnet-mvc-4-fundamentals/_static/image24.png "新增檢視對話方塊")

    *新增檢視對話方塊*

    **\Views\Store\Index.cshtml**檢視範本檔案會建立並開啟。 根據提供的資訊**加入檢視**對話方塊，在最後一個步驟中，檢視範本會預期**StoreIndexViewModel**做為要用來產生 HTML 回應資料的執行個體。 您會發現範本會繼承`ViewPage<musicstore.viewmodels.storeindexviewmodel>`C# 中。

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>工作 5-正在更新檢視範本

在這個工作中，您將會更新擷取的內容類型和其名稱中的頁面數目在最後一個工作中建立的檢視範本。

> [!NOTE]
> 您將會使用語法 @ (通常稱為&quot;程式碼區塊&quot;) 來執行程式碼中檢視範本。

1. 在  **Index.cshtml**檔案，內**存放區**資料夾中，其程式碼取代為下列：

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. 透過內容類型 清單中的迴圈**StoreIndexViewModel**並建立 HTML **&lt;ul&gt;** 列出使用**foreach**迴圈。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. 按下**F5**以執行應用程式，並瀏覽 **/儲存**。 您會看到傳入的內容類型清單**StoreIndexViewModel**物件**StoreController**檢視範本。

    ![顯示的內容類型清單的檢視](aspnet-mvc-4-fundamentals/_static/image26.png "檢視顯示的內容類型清單")

    *顯示的內容類型清單的檢視*
4. 關閉瀏覽器。

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>練習 6： 在檢視中使用參數

練習 3 中您已了解如何將參數傳遞到控制器。 在此練習中，您將學習如何在檢視範本中使用這些參數。 基於這個目的，您將會介紹第一次，以協助您管理您的資料和網域邏輯的模型類別。 此外，您將了解如何建立 ASP.NET MVC 應用程式內頁面的連結，而不必擔心的編碼方式的 URL 路徑等項目。

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>工作 1-新增模型類別

不同於 Viewmodel，只是為了將資訊從控制器傳遞至檢視建立模型類別是建置來包含和管理資料和網域邏輯。 在這項工作中，您會將兩個模型類別，來代表這些概念： **Genre**並**專輯**。

1. 如果尚未開啟，啟動**VS Express for Web**
2. 在 **檔案**功能表上，選擇**開啟專案**。 在 [開啟專案] 對話方塊中，瀏覽至**Source\Ex06 UsingParametersInView\Begin**，選取**Begin.sln**然後按一下**開啟**。 或者，您可以繼續使用解決方案取得完成前一個練習之後。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
3. 新增**Genre**模型類別。 若要這樣做，請以滑鼠右鍵按一下**模型**中的資料夾**方案總管**，選取**新增**，然後**新項目**選項。 底下**程式碼**，選擇**類別**項目，然後將檔案命名為*Genre.cs*，然後按一下**新增**。

    ![加入類別](aspnet-mvc-4-fundamentals/_static/image27.png "加入類別")

    *加入新項目*

    ![新增內容類型的模型類別](aspnet-mvc-4-fundamentals/_static/image28.png "新增內容類型的模型類別")

    *新增內容類型的模型類別*
4. 新增**名稱**內容類型類別的屬性。 若要這樣做，請新增下列程式碼：

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex6 Genre*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. 後方的相同程序之前，新增**專輯**類別。 若要這樣做，請以滑鼠右鍵按一下**模型**中的資料夾**方案總管**，選取**新增**，然後**新項目**選項。 底下**程式碼**，選擇**類別**項目，然後將檔案命名為*Album.cs*，然後按一下**新增**。
6. 專輯類別中加入兩個屬性： **Genre**並**標題**。 若要這樣做，請新增下列程式碼：

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex6 專輯*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>工作 2-新增 StoreBrowseViewModel

A **StoreBrowseViewModel**將這項工作中用來顯示符合所選的內容類型的 Album。 在這個工作中，您會建立這個類別，然後再新增 兩個屬性，以處理**Genre**及其**專輯**的清單。

1. 新增**StoreBrowseViewModel**類別。 若要這樣做，請以滑鼠右鍵按一下**Viewmodel**中的資料夾**方案總管**，選取**新增**，然後**新項目**選項。 底下**程式碼**，選擇**類別**項目，然後將檔案命名為*StoreBrowseViewModel.cs*，然後按一下**新增**。
2. 加入的參考中的模型**StoreBrowseViewModel**類別。 若要這樣做，請新增下列使用命名空間：

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. 新增兩個屬性，以**StoreBrowseViewModel**類別： **Genre**並**專輯**。 若要這樣做，請新增下列程式碼：

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> 何謂**清單&lt;專輯&gt;** ？: 使用此定義**清單&lt;T&gt;** 型別，其中**T**限制哪些項目，這個型別**清單**屬於，在此情況下**專輯**（或其任何子系）。
> 
> 這項功能設計類別和方法，延後的一或多個型別規格，直到類別或方法宣告和具現化用戶端程式碼是 C# 語言的一項功能稱為**泛型**。
> 
> **清單&lt;T&gt;** 相當於泛型**ArrayList**類型，適用於**System.Collections.Generic**命名空間。 其中一個使用的優點**泛型**是，由於指定型別，因此您不需要負責檢查作業，例如轉換到的項目型別的**專輯**和一樣具有**ArrayList**。

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>工作 3-使用新的 ViewModel StoreController 中

在這個工作中，您將修改**StoreController**的**瀏覽**並**詳細資料**動作方法，以使用新**StoreBrowseViewModel**.

1. 加入 [Models] 資料夾中的參考**StoreController**類別。 若要這樣做，請展開**控制器**資料夾中的**方案總管**，然後開啟**StoreController**類別。 然後新增下列程式碼：

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. 取代**瀏覽**動作方法，以使用**StoreViewBrowseController**類別。 您將建立內容類型和兩個新的專輯物件與虛擬資料 （在下一步 的實際操作實驗室中您會使用資料庫中的實際資料）。 若要這樣做，請取代**瀏覽**為下列程式碼的方法：

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. 取代**詳細資料**動作方法，以使用**StoreViewBrowseController**類別。 您將建立新**專輯**物件傳回給**檢視**。 若要這樣做，請取代**詳細資料**為下列程式碼的方法：

    (程式碼片段- *ASP.NET MVC 4 基本概念-Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>工作 4-新增瀏覽檢視範本

在這個工作中，您會將**瀏覽**檢視，以顯示特定內容類型為找到的 Album。

1. 之前建立新的檢視範本，您應該會建置專案，讓**加入檢視** 對話方塊之 snmp v1 **ViewModel**類別使用。 建置專案，方法是選取**建置**功能表項目，然後**建置 MvcMusicStore**。
2. 新增**瀏覽**檢視。 若要這樣做，以滑鼠右鍵按一下**瀏覽**動作方法的**StoreController**然後按一下**加入檢視**。
3. 在 **加入檢視**對話方塊方塊中，確認檢視名稱**瀏覽**。 請檢查**建立強型別檢視**核取方塊，然後選取**StoreBrowseViewModel**從**模型類別**下拉式清單。 其他欄位保留其預設值。 然後按一下 [加入] 。

    ![加入瀏覽檢視](aspnet-mvc-4-fundamentals/_static/image29.png "加入瀏覽檢視")

    *加入瀏覽檢視*
4. 修改**Browse.cshtml**要顯示的內容類型的詳細資訊，存取**StoreBrowseViewModel**傳遞至檢視範本的物件。 若要這樣做，請將內容取代為下列: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>工作 5-執行應用程式

在這個工作中，您將測試所**瀏覽**方法會擷取來自的專輯**瀏覽**方法動作。

1. 按下**F5**執行應用程式。
2. 專案一開始在首頁上。 將 URL 變更為  **/儲存] / [瀏覽？內容類型 = Disco**以確認此動作會傳回兩個相簿。

    ![瀏覽市集 Disco 專輯](aspnet-mvc-4-fundamentals/_static/image30.png "瀏覽市集 Disco 相簿")

    *瀏覽市集 Disco 相簿*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>工作 6-顯示資訊的相關特定專輯

在這個工作中，您將實作**存放區/詳細資料**檢視，以顯示特定專輯的相關資訊。 在這個實際操作實驗室中，您將會顯示關於專輯的所有項目已經包含在**檢視**範本。 是的而不是建立**StoreDetailsViewModel**類別，您將使用目前**StoreBrowseViewModel**將專輯傳遞給它的範本。

1. 關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。 加入新**詳細資料**檢視**StoreController**的**詳細資料**動作方法。 若要這樣做，請以滑鼠右鍵按一下**詳細資料**方法中的**StoreController**類別，然後按一下**加入檢視**。
2. 在 [**加入檢視**] 對話方塊中，確認**檢視名稱**是**詳細資料**。 請檢查**建立強型別檢視**核取方塊，然後選取**專輯**從**模型類別**下拉式清單。 其他欄位保留其預設值。 然後按一下 [加入] 。 這會建立並開啟**\Views\Store\Details.cshtml**檔案。

    ![加入詳細資料檢視](aspnet-mvc-4-fundamentals/_static/image31.png "加入詳細資料檢視")

    *加入詳細資料檢視*
3. 修改**Details.cshtml**若要顯示的專輯資訊的檔案存取**專輯**傳遞至檢視範本的物件。 若要這樣做，請將內容取代為下列: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>工作 7-執行應用程式

在這個工作中，您將測試所**詳細資料**View 擷取來自的專輯資訊**詳細資料動作**方法。

1. 按下**F5**執行應用程式。
2. 在專案開始**首頁**頁面。 將 URL 變更為 **/Store/Details/5**確認的專輯資訊。

    ![瀏覽專輯詳細](aspnet-mvc-4-fundamentals/_static/image32.png "瀏覽專輯詳細資料")

    *瀏覽專輯的詳細資料*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>工作 8-新增頁面之間的連結

在這個工作中，您將加入在存放區檢視中，將連結放在適當的每個內容類型名稱的連結 **/Store/瀏覽**URL。 如此一來，當您按一下 內容類型，例如**Disco**，它會瀏覽至**存放區/瀏覽？ 內容類型 = Disco** URL。

1. 關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。 更新**Index**新增至連結的頁面**瀏覽**頁面。 若要這樣做，請在**方案總管**展開**檢視**資料夾，則**存放區**資料夾，然後按兩下**Index.cshtml**頁面。
2. 表示內容類型選取 [瀏覽] 檢視中加入的連結。 若要這樣做，請取代下列反白顯示的程式碼，傳入**&lt;li&gt;** 標記: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > 另一種方法就直接連結到頁面上，使用程式碼，如下所示：
   > 
   > &lt;a =&quot;/Store/瀏覽？ 內容類型 =@genreName&quot;&gt;@genreName &lt; /a&gt;
   > 
   > 雖然這種方法的運作方式，它會相依於硬式編碼字串。 如果您稍後重新命名該控制站，您必須手動變更這項指示。 更好的替代做法是使用**HTML 協助程式**方法。 ASP.NET MVC 包含 HTML 協助程式方法，這是適用於這類工作的方法。 **Html.ActionLink()** 協助程式方法可讓您輕鬆地建置 HTML **&lt;&gt;** 連結，並確定具有正確的 URL 路徑 URL 編碼。
   > 
   > Htlm.ActionLink 有數個多載。 在本練習會使用另一個會採用三個參數：
   > 
   > 1. 連結文字，會顯示的內容類型名稱
   > 2. 控制器動作名稱 (**瀏覽**)
   > 3. 路由參數值，指定兩個名稱 (**Genre**) 和值 (**內容類型名稱**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>工作 9-執行應用程式

在這個工作中，您將測試每個內容類型會顯示適當的連結 **/Store/瀏覽**URL。

1. 按下**F5**執行應用程式。
2. 專案一開始在首頁上。 將 URL 變更為 **/儲存**若要確認每個內容類型連結至適當**存放區/瀏覽**URL。

    ![瀏覽連結來瀏覽 頁面的內容類型](aspnet-mvc-4-fundamentals/_static/image33.png "連結來瀏覽] 頁面的 [瀏覽內容類型")

    *瀏覽的內容類型瀏覽 頁面的連結*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>工作 10-使用動態 ViewModel 集合來傳遞值

在這個工作中，您將了解簡單且功能強大的方法，將控制器和檢視之間傳遞值，而不進行任何變更，在模型中。 ASP.NET MVC 4 提供集合&quot;ViewModel&quot;，這可以指派給任何動態的值和控制器和檢視以及內存取。

您現在會使用 ViewBag 動態集合，將一份&quot;**加星號內容類型**&quot;從控制器到檢視。 將存取存放區索引檢視，以**ViewModel**並顯示資訊。

1. 關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。 開啟**StoreController.cs** ，並修改**Index**方法用來建立一份起子頭插入 ViewModel 集合的內容類型：

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > 您也可以使用語法**ViewBag [&quot;Starred&quot;]** 來存取屬性。
2. 星狀圖示**&quot;starred.png&quot;** 納入**Source\Assets\Images**本實驗室的資料夾。 若要將它新增至應用程式中，拖曳其內容從**Windows 檔案總管**到視窗**方案總管 中**在 Visual Web Developer Express，如下所示：

    ![加入至方案的星型映像](aspnet-mvc-4-fundamentals/_static/image34.png "星星的影像加入至方案")

    *星型映像新增到方案*
3. 開啟檢視**Store/Index.cshtml**和修改內容。 您將讀取&quot;加星號&quot;中的屬性**ViewBag**集合，並詢問是否在清單中目前的內容類型名稱。 在此情況下，您會顯示由右至內容類型連結以星號圖示。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>工作 11-執行應用程式

在這個工作中，您將測試的星號的內容類型，顯示星星的圖示。

1. 按下**F5**執行應用程式。
2. 在專案開始**首頁**頁面。 將 URL 變更為 **/儲存**以確認每個精選的內容類型有 respecting 標籤：

    ![瀏覽內容類型，具有星號的項目](aspnet-mvc-4-fundamentals/_static/image35.png "瀏覽的內容類型與星號的項目")

    *瀏覽的內容類型與星號的項目*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>練習 7: 瀏覽 ASP.NET MVC 4 的新範本

在此練習中，您會瀏覽的增強功能，在 ASP.NET MVC 4 專案範本中，初步了解新範本的最相關的功能。

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>工作 1： 瀏覽 ASP.NET MVC 4 的網際網路應用程式範本

1. 如果尚未開啟，啟動**VS Express for Web**
2. 選取**檔案 |新 |專案**功能表命令。 在 **新的專案**對話方塊中，選取**Visual C# |Web**範本，在左窗格中樹狀結構，然後選擇  **ASP.NET MVC 4 Web 應用程式**。 **名稱**專案*MusicStore* ，並更新**方案名稱**來*開始*，然後選取位置 （或保留預設值），按一下  **確定**.

    ![建立新的 ASP.NET MVC 4 專案](aspnet-mvc-4-fundamentals/_static/image36.png "建立新的 ASP.NET MVC 4 專案")

    *建立新的 ASP.NET MVC 4 專案*
3. 在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**專案範本，然後按一下**確定**。 請注意，您可以選取 ASPX 或 Razor 檢視引擎。

    ![建立新 ASP.NET MVC 4 網際網路應用程式](aspnet-mvc-4-fundamentals/_static/image37.png "建立新 ASP.NET MVC 4 網際網路應用程式")

    *建立新 ASP.NET MVC 4 網際網路應用程式*

    > [!NOTE]
    > ASP.NET MVC 3 中已引進 razor 語法。 其目標是要減少字元和啟用快速和流暢的程式碼撰寫工作流程在檔案中，所需按鍵的數目。 Razor 會利用現有 C# /VB （或其他） 的語言能力，並提供可讓一個很棒的 HTML 建構工作流程的範本標記語法。
4. 按下**F5**執行方案，並查看更新的範本。 您可以查看下列功能：

    1. **現代化樣式範本**

        範本已經更新，提供更多的現代化外觀的樣式。

        ![重新設定樣式的 ASP.NET MVC 4 範本](aspnet-mvc-4-fundamentals/_static/image38.png "抵擋範本的 ASP.NET MVC 4")

        *重新設定樣式的 ASP.NET MVC 4 範本*
    2. **自適性轉譯**

        請參閱調整瀏覽器視窗的大小，並注意如何頁面配置動態調整為新的視窗大小。 這些範本會使用適應性呈現技巧，來正確轉譯，而不需要任何自訂的桌上型電腦和行動裝置平台。

        ![ASP.NET MVC 4 專案範本，以不同的瀏覽器尺寸](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 專案範本，在不同的瀏覽器大小")

        *ASP.NET MVC 4 專案範本，在不同的瀏覽器大小*
5. 關閉瀏覽器以停止偵錯工具，並返回 Visual Studio。
6. 現在您已經能夠探索解決方案，並查看一些推出由 ASP.NET MVC 4 專案範本的新功能。

    ![ASP.NET MVC4-網際網路-應用程式-專案-樣板](aspnet-mvc-4-fundamentals/_static/image40.png "ASP.NET MVC 4 的網際網路應用程式專案範本")

    *ASP.NET MVC 4 的網際網路應用程式專案範本*

   1. **HTML5 標記**

       瀏覽以找出新的佈景主題標記，例如開啟的範本檢視**About.cshtml**檢視中的**首頁**資料夾。

       ![新的範本，使用 Razor 和 HTML5 的標記](aspnet-mvc-4-fundamentals/_static/image41.png "新的範本，使用 Razor 和 HTML5 的標記")

       *新的範本，使用 Razor 和 HTML5 的標記*
   2. **包含的 JavaScript 程式庫**

      1. **jQuery**: jQuery 簡化 HTML 文件周遊、 事件處理、 加上動畫，以及 Ajax 互動。
      2. **jQuery UI**： 此程式庫提供低階互動和進階效果的動畫，以及主題化 widget，jQuery JavaScript 程式庫為基礎的抽象概念。

         > [!NOTE]
         > 您可以了解 jQuery 和 jQuery UI 中[ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/)。
      3. **KnockoutJS**： 現在包含 ASP.NET MVC 4 預設範本**KnockoutJS**，可讓您建立豐富且回應迅速的 web 應用程式使用 JavaScript 和 HTML 的 JavaScript MVVM 架構。 例如在 ASP.NET MVC 3 中，jQuery 和 jQuery UI 程式庫也包含 ASP.NET MVC 4 中。

          > [!NOTE]
          > 您可以取得 KnockOutJS 程式庫，在下列連結的詳細資訊： [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/)。
      4. **Modernizr**： 此程式庫會自動執行，讓您的網站與較舊的瀏覽器相容，使用 HTML5 和 css3 等技術時。

          > [!NOTE]
          > 您可以取得此連結中的 Modernizr 程式庫的詳細資訊： [ http://www.modernizr.com/ ](http://www.modernizr.com/)。
   3. **在方案中包含的 SimpleMembership**

       SimpleMembership 已設計為先前的 ASP.NET 角色和成員資格提供者系統取代。 它有許多新功能，輕鬆安全的網頁開發人員以更有彈性的方式。

       網際網路範本已經有設定整合 SimpleMembership 幾件事，例如 AccountController 已準備好要使用 OAuthWebSecurity （適用於 OAuth 的帳戶註冊、 登入、 管理等） 和 Web 的安全性。

       ![包含在解決方案中 SimpleMembership](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership 納入解決方案")

       *包含在解決方案中 SimpleMembership*

       > [!NOTE]
       > 進一步了解[OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) MSDN 中。

> [!NOTE]
> 此外，您可以在其中部署此應用程式以 Windows Azure 網站的下列[附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixB)。


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>總結

藉由完成這個實際操作實驗室中，您已了解 ASP.NET MVC 的基本概念：

- MVC 應用程式，以及它們如何互動的核心項目
- 如何建立 ASP.NET MVC 應用程式
- 如何新增和設定控制站來處理參數傳遞的 URL 和查詢字串
- 如何新增版面配置主版頁面，設定一般的 HTML 內容，以增強外觀與風格及檢視範本，以顯示 HTML 內容的樣式表的範本
- 如何將屬性傳遞至檢視範本中使用 ViewModel 模式，來顯示動態資訊
- 如何使用傳遞至控制器檢視範本參數
- 如何將連結新增至 ASP.NET MVC 應用程式內的頁面
- 如何加入和檢視中使用動態屬性
- ASP.NET MVC 4 專案範本中的增強功能

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附錄 a： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Windows Azure SDK</em>&quot;。
2. 按一下 **立即安裝**。 如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。

    ![接受授權條款](aspnet-mvc-4-fundamentals/_static/image44.png)

    *接受授權條款*
5. 等候完成的下載與安裝程序。

    ![安裝進度](aspnet-mvc-4-fundamentals/_static/image45.png)

    *安裝進度*
6. 安裝完成時，按一下**完成**。

    ![安裝已完成](aspnet-mvc-4-fundamentals/_static/image46.png)

    *安裝已完成*
7. 按一下 **結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。

    ![VS Express for Web 圖格](aspnet-mvc-4-fundamentals/_static/image47.png)

    *VS Express for Web 圖格*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy

本附錄將說明如何從 Windows Azure 管理入口網站中建立新的網站和發行您取得所遵循的實驗室中，利用 Web Deploy 發行功能的 Windows Azure 所提供的應用程式。

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>工作 1-建立新的網站，從 Windows Azure 入口網站

1. 移至[Windows Azure 管理入口網站](https://manage.windowsazure.com/)並使用您的訂用帳戶相關聯的 Microsoft 認證登入。

    > [!NOTE]
    > 使用 Windows Azure，您可以免費託管 10 個 ASP.NET 網站，並再隨著流量成長而調整。 您可以註冊申請[此處](http://aka.ms/aspnet-hol-azure)。

    ![登入 Windows Azure 入口網站](aspnet-mvc-4-fundamentals/_static/image48.png "登入 Windows Azure 入口網站")

    *登入 Windows Azure 管理入口網站*
2. 按一下 **新增**命令列上。

    ![建立新的 Web 站台](aspnet-mvc-4-fundamentals/_static/image49.png "建立新的網站")

    *建立新的網站*
3. 按一下 **計算** | **網站**。 然後選取**快速建立**選項。 新的網站提供可用的 URL，然後按**建立網站**。

    > [!NOTE]
    > Windows Azure 網站時，您可以控制和管理雲端中執行的 web 應用程式的主應用程式。 [快速建立] 選項可讓您部署已完成的 web 應用程式至 Windows Azure 網站從入口網站外部。 它不包含設定資料庫的步驟。

    ![建立新的網站上，使用 快速建立](aspnet-mvc-4-fundamentals/_static/image50.png "建立新的網站上，使用 快速建立")

    *建立新的網站上，使用 快速建立*
4. 等到新**網站**建立。
5. 建立網站後按一下底下的連結**URL**資料行。 檢查新的網站運作。

    ![瀏覽至新的 web 站台](aspnet-mvc-4-fundamentals/_static/image51.png "瀏覽至新的網站")

    *瀏覽至新的網站*

    ![執行的 web 站台](aspnet-mvc-4-fundamentals/_static/image52.png "執行的網站")

    *執行的網站*
6. 返回入口網站並按一下底下的網站名稱**名稱**資料行來顯示 [管理] 頁面。

    ![開啟 網站管理頁面](aspnet-mvc-4-fundamentals/_static/image53.png "開啟的網站管理頁面")

    *開啟 網站管理頁面*
7. 在 **儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。

    > [!NOTE]
    > *發行設定檔*包含所有發行至 Windows Azure 網站的每個已啟用的發行方法的 web 應用程式所需的資訊。 發行設定檔包含 Url、 使用者認證和連接到並對每個發行集的方法已啟用的端點進行驗證所需的資料庫字串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**並**Microsoft Visual Studio 2012**支援讀取發行設定檔，以自動化的這些程式的組態正在發行至 Windows Azure 網站的 web 應用程式。

    ![正在下載網站發行設定檔](aspnet-mvc-4-fundamentals/_static/image54.png "下載網站發行設定檔")

    *正在下載網站發行設定檔*
8. 下載發行設定檔至已知位置。 進一步在這個練習中，您會看到如何從 Visual Studio 的 web 應用程式至 Windows Azure 網站發行使用此檔案。

    ![儲存發行設定檔](aspnet-mvc-4-fundamentals/_static/image55.png "儲存發佈設定檔")

    *儲存發行設定檔*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>工作 2-設定資料庫伺服器

如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。 如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。

1. 您將需要 SQL Database 伺服器來儲存應用程式資料庫。 您可以從您的訂用帳戶，在 Windows Azure Management 入口網站中檢視 SQL Database 伺服器**Sql Database** | **伺服器** | **伺服器儀表板**。 如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。 請記下的**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，如同您會在下一個工作中使用它們。 並未建立資料庫，因為它會建立在稍後的階段。

    ![SQL Database 伺服器儀表板](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database 伺服器儀表板")

    *SQL Database 伺服器儀表板*
2. 下一個工作在您將測試資料庫連接，從 Visual Studio 中，因此您需要在伺服器的清單包含您的本機 IP 位址**允許的 IP 位址**。 若要這樣做，請按一下**設定**，選取的 IP 位址**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**並**結束 IP 位址**文字方塊中，然後按一下 [ ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) ] 按鈕。

    ![新增用戶端 IP 位址](aspnet-mvc-4-fundamentals/_static/image58.png)

    *新增用戶端 IP 位址*
3. 一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。

    ![確認變更](aspnet-mvc-4-fundamentals/_static/image59.png)

    *確認變更*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy

1. 返回至 ASP.NET MVC 4 的方案。 在 **方案總管**，以滑鼠右鍵按一下網站專案，然後選取**發佈**。

    ![發行應用程式](aspnet-mvc-4-fundamentals/_static/image60.png "發行應用程式")

    *發行網站*
2. 匯入發行設定檔儲存在第一項工作。

    ![匯入發行設定檔](aspnet-mvc-4-fundamentals/_static/image61.png "匯入發行設定檔")

    *匯入發行設定檔*
3. 按一下 **驗證連線**。 驗證完成後按一下**下一步**。

    > [!NOTE]
    > 一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。

    ![驗證連接](aspnet-mvc-4-fundamentals/_static/image62.png "驗證連線")

    *驗證連接*
4. 在 **設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。

    ![Web 部署組態](aspnet-mvc-4-fundamentals/_static/image63.png "Web 部署設定")

    *Web 部署設定*
5. 設定資料庫連線，如下所示：

   - 在 **伺服器名稱**輸入您使用 SQL Database 伺服器的 URL *tcp:* 前置詞。
   - 在 **使用者名**輸入您的伺服器系統管理員身分登入名稱。
   - 在 **密碼**輸入您的伺服器系統管理員身分登入密碼。
   - 輸入新的資料庫名稱，例如： *MVC4SampleDB*。

     ![設定目的地連接字串](aspnet-mvc-4-fundamentals/_static/image64.png "設定目的地連接字串")

     *設定目的地連接字串*
6. 然後按一下 [確定]。  當系統提示您建立資料庫時，按一下**是**。

    ![建立資料庫](aspnet-mvc-4-fundamentals/_static/image65.png "建立的資料庫字串")

    *建立資料庫*
7. 您將用來連接至 Windows Azure 中的 SQL 資料庫的連接字串會顯示在文字方塊中的預設連線。 然後按 [下一步] 。

    ![連接字串指向 SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "連接字串指向 SQL Database")

    *連接字串指向 SQL Database*
8. 在  **Preview**頁面上，按一下**發佈**。

    ![Web 應用程式發行](aspnet-mvc-4-fundamentals/_static/image67.png "發行 web 應用程式")

    *發行 web 應用程式*
9. 當發行程序完成時，預設瀏覽器會開啟已發行的網站。

    ![應用程式發行至 Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "應用程式發行至 Windows Azure")

    *應用程式發佈至 Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>附錄 c： 使用程式碼片段

使用程式碼片段，您會有您需要在隨手可得的所有程式碼。 實驗室課程文件會告訴您完全時您可以使用它們，如下圖所示。

![若要將程式碼插入您的專案使用 Visual Studio 程式碼片段](aspnet-mvc-4-fundamentals/_static/image69.png "程式碼插入您的專案使用 Visual Studio 程式碼片段")

*若要將程式碼插入您的專案使用 Visual Studio 程式碼片段*

***若要新增的程式碼片段，使用鍵盤 （僅限 C#)***

1. 將游標放在您想要插入的程式碼。
2. 開始輸入程式碼片段名稱 （不含空格或連字號）。
3. 觀看為 IntelliSense 會顯示相符的程式碼片段的名稱。
4. 選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。
5. 按下 Tab 鍵兩次的游標位置插入程式碼片段。

![開始鍵入程式碼片段名稱](aspnet-mvc-4-fundamentals/_static/image70.png "開始輸入程式碼片段名稱")

*開始鍵入程式碼片段名稱*

![按 Tab 鍵以選取反白顯示的程式碼片段](aspnet-mvc-4-fundamentals/_static/image71.png "按 Tab 鍵以選取反白顯示的程式碼片段")

*按 Tab 鍵以選取反白顯示的程式碼片段*

![再次按 Tab 鍵和程式碼片段會依序展開](aspnet-mvc-4-fundamentals/_static/image72.png "再次按 Tab 鍵和程式碼片段會展開")

*再次按 Tab 鍵和程式碼片段會展開*

***若要新增的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。 以滑鼠右鍵按一下您要插入程式碼片段。

1. 選取 **插入程式碼片段**後面**My Code Snippets**。
2. 選擇相關的程式片段，從清單中，對它按一下。

![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-fundamentals/_static/image73.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")

*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*

![對它按一下挑選清單中，相關程式碼片段](aspnet-mvc-4-fundamentals/_static/image74.png "對它按一下挑選清單中，相關程式碼片段")

*對它按一下挑選清單中，相關程式碼片段*
