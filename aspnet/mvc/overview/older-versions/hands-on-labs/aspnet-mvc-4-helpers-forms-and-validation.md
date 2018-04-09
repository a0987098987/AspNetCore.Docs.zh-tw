---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 Helper、 表單和驗證 |Microsoft 文件
author: rick-anderson
description: 在 ASP.NET MVC 4 模型和資料存取實際操作實驗室中，您已載入和顯示資料的資料庫。 在這個實際操作實驗室中，您將會加入...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 4cfa98144919c3f1bdb3608970af1a7952fe6ea7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>ASP.NET MVC 4 Helper、 表單和驗證

由[Web 營小組](https://twitter.com/webcamps)

[下載 Web 營訓練套件](https://aka.ms/webcamps-training-kit)

在**ASP.NET MVC 4 模型和資料存取**實際操作實驗室中，您已載入和顯示資料的資料庫。 在這個實際操作實驗室中，您將會加入**Music Store**應用程式編輯該資料的功能。

請注意該目標，您將先建立將支援建立、 讀取、 更新和刪除 (CRUD) 動作的相簿的控制站。 您將會產生運用 ASP.NET MVC 的 scaffolding 功能，以顯示 HTML 表格中的專輯屬性的索引檢視範本。 若要增強該檢視，您將加入將會截斷長描述的自訂 HTML helper。

之後，您將加入的編輯和建立的檢視表，以供您變更在資料庫中，使用下拉式清單類似的表單項目說明相簿。

最後，您將會讓使用者可以刪除專輯和您將會防止使用者從輸入錯誤的資料來驗證其輸入。

這個實際操作實驗室假設您擁有的基本知識**ASP.NET MVC**。 如果您不使用**ASP.NET MVC**之前，我們建議您在一段**ASP.NET MVC 基礎**實際操作實驗室。

這個實驗室將引導您完成先前所述將微幅變更套用至來源資料夾中提供的範例 Web 應用程式的新功能與增強功能。

> [!NOTE]
> 所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 這個實驗室中的特定專案將會位於[ASP.NET MVC 4 Helper、 表單和驗證](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation)。

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將學會如何：

- 建立支援 CRUD 作業的控制站
- 產生索引檢視來顯示 HTML 表格中的實體屬性
- 加入自訂的 HTML helper
- 建立和自訂的編輯檢視
- 區分回應 HTTP GET 或 HTTP POST 呼叫的動作方法
- 加入和自訂建立的檢視
- 處理刪除的實體
- 驗證使用者輸入

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

您必須具備下列項目才能完成本實驗室：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 A](#AppendixA)如需有關如何安裝指示)。

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

**安裝程式碼片段**

為了方便起見，大部分您將沿著這個實驗室管理的程式碼可做為 Visual Studio 程式碼片段。 若要安裝執行的程式碼片段**.\Source\Setup\CodeSnippets.vsi**檔案。

如果您不熟悉 Visual Studio 程式碼片段，而且想来了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 b： 使用程式碼片段](#AppendixB)&quot;。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>練習

下列練習構成這個實際操作實驗室：

1. [建立存放區管理員控制站和其索引的檢視](#Exercise1)
2. [加入 HTML Helper](#Exercise2)
3. [建立編輯檢視](#Exercise3)
4. [加入建立檢視](#Exercise4)
5. [處理刪除](#Exercise5)
6. [新增驗證](#Exercise6)
7. [在用戶端使用不顯眼的 jQuery](#Exercise7)

> [!NOTE]
> 每個練習會伴隨著**結束**包含所產生的解決方案完成練習之後，您應該取得資料夾。 如果您需要其他協助使用的所有練習，您可以使用這個方案做為指南。


完成本實驗室估計時間： **60 分鐘的時間**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>練習 1： 建立存放區管理員控制站和其索引的檢視

在此練習中，您將學習如何建立新的控制站，以支援 CRUD 作業，來自訂其從資料庫，最後產生運用 ASP.NET MVC 的 scaffolding 索引檢視表範本傳回一份專輯的索引動作方法顯示 HTML 表格中的專輯屬性的功能。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>工作 1-建立 StoreManagerController

在這個工作中，您將建立新的控制器呼叫**StoreManagerController**支援 CRUD 作業。

1. 開啟**開始**方案位於**來源/Ex1CreatingTheStoreManagerController/開始/**資料夾。

   1. 您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 加入新的控制器。 若要這樣做，請以滑鼠右鍵按一下**控制器**資料夾在方案總管中，選取**新增**然後**控制器**命令。 變更**控制器****名稱**至**StoreManagerController** ，並確定選項**MVC 控制器具有空白讀取/寫入動作**已選取。 按一下 [加入] 。

    ![加入控制器 對話方塊](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "加入控制器 對話方塊")

    *加入控制器 對話方塊*

    會產生新的控制器類別。 因為您指定要加入的讀取/寫入，虛設常式方法的動作一般 CRUD 動作會建立 TODO 註解中，填滿提示要包含應用程式特定邏輯。

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>工作 2-自訂 StoreManager 索引

在這項工作，您將自訂 StoreManager 索引動作方法，從資料庫傳回相簿的清單檢視。

1. 在 StoreManagerController 類別中，加入下列*使用*指示詞。

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex1 使用 MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. 加入欄位以**StoreManagerController**來保存的執行個體**MusicStoreEntities。**

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. 實作 StoreManagerController 索引動作傳回的專輯清單檢視。

    控制器動作邏輯將會非常類似於先前寫入 StoreController 索引動作。 使用 LINQ 來擷取所有的相簿，包括顯示的內容類型與演出者資訊。

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex1 StoreManagerController 索引*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>工作 3-建立索引檢視

在這個工作中，您將建立索引檢視範本，以顯示所傳回的相簿**StoreManager**控制站。

1. 之前建立新的檢視表範本，您應該可以建置專案，讓**新增檢視對話方塊**知道**專輯**類別，才能使用。 選取**建置 |建置 MvcMusicStore**建置專案。
2. 以滑鼠右鍵按一下**索引**動作方法，然後選取**加入檢視**。 這會顯示**加入檢視**對話方塊。

    ![加入檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "加入檢視")

    *加入從索引方法內的檢視*
3. 在 [新增檢視] 對話方塊中，確認檢視表名稱為**索引**。 選取**建立強型別檢視**選項，然後選取**專輯 (MvcMusicStore.Models)**從**模型類別**下拉式清單。 選取**清單**從**Scaffold 範本**下拉式清單。 保留**檢視引擎**至**Razor** ，其預設值的其他欄位值，然後按**新增**。

    ![加入索引檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "加入索引檢視")

    *加入索引檢視*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>工作 4-自訂的索引檢視 scaffold

在這項工作，您將調整建立 ASP.NET MVC scaffolding 功能，讓它顯示您想要的欄位的簡單檢視範本。

> [!NOTE]
> **Scaffolding**支援 ASP.NET MVC 中的會產生列出專輯模型中的所有欄位的簡單檢視範本。 **Scaffolding**提供快速的方法，若要開始強型別檢視:，而不必手動撰寫檢視表範本的 scaffolding 快速產生的預設範本，然後您可以修改產生的程式碼。


1. 檢閱建立的程式碼。 產生的欄位清單將屬於下列 HTML 資料表，其中**Scaffolding**用來顯示表格式資料。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. 取代**&lt;資料表&gt;**只顯示為下列程式碼的程式碼**類型**，**演出者**，**專輯標題**，和**價格**欄位。 這會刪除**AlbumId**和**專輯封面 URL**資料行。 此外，它會變更 GenreId 和 ArtistId 的資料行，以顯示其連結的類別屬性**Artist.Name**和**Genre.Name**，並移除**詳細資料**連結。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. 變更下列說明。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>工作 5-執行應用程式

在這個工作中，您將測試的**StoreManager** **索引**檢視範本會顯示一份專輯根據上一個步驟的設計。

1. 按**F5**執行應用程式。
2. 在首頁上，啟動專案。 將 URL 變更為**/StoreManager**確認會顯示一份相簿，顯示其**標題**，**演出者**和**類型**。

    ![瀏覽相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "瀏覽相簿")

    *瀏覽相簿*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>練習 2： 加入 HTML Helper

StoreManager 索引頁有一個可能的問題： 標題和演出者名稱屬性可以同時為夠長，會擲回關閉資料表格式。 在這個練習中，您將學習如何加入自訂的 HTML helper 來截斷該文字。

在下圖中，您可以看到如何格式修改，因為文字的長度，而當您使用較小的瀏覽器的大小。

![瀏覽與專輯的清單不會被截斷文字](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "瀏覽與專輯的清單不會被截斷的文字")

*瀏覽與專輯的清單不會被截斷的文字*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>工作 1-擴充的 HTML Helper

在這項工作，您會將新方法加入**Truncate**至**HTML** ASP.NET MVC 檢視中公開的物件。 若要這樣做，您會實作**擴充方法**內建**System.Web.Mvc.HtmlHelper** ASP.NET MVC 所提供的類別。

> [!NOTE]
> 若要深入了解**擴充方法**，請瀏覽 msdn 上的本文。 [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. 開啟**開始**方案位於**來源/Ex2AddingAnHTMLHelper/開始/**資料夾。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

   1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 開啟 StoreManager 的索引檢視。 若要這樣做，請在 [方案總管] 中展開**檢視**資料夾，然後在**StoreManager**並開啟**Index.cshtml**檔案。
3. 加入下列程式碼下方<strong>@model</strong>指示詞來定義<strong>Truncate</strong> helper 方法。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>工作 2-截斷網頁中的文字

在這個工作中，您將使用**Truncate**截斷檢視範本中的文字的方法。

1. 開啟 StoreManager 的索引檢視。 若要這樣做，請在 [方案總管] 中展開**檢視**資料夾，然後在**StoreManager**並開啟**Index.cshtml**檔案。
2. 取代顯示的行**演出者名稱**和專輯**標題**。 若要這樣做，請取代下列幾行。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>工作 3-執行應用程式

在這個工作中，您將測試的**StoreManager** **索引**檢視範本會截斷的專輯標題和演出者名稱。

1. 按**F5**執行應用程式。
2. 在首頁上，啟動專案。 將 URL 變更為**/StoreManager**以確認該長時間中的文字**標題**和**演出者**資料行就會被截斷。

    ![截斷標題和演出者名稱](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "截斷標題和演出者名稱")

    *截斷的標題和演出者名稱*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>練習 3： 建立編輯檢視

在此練習中，您將學習如何建立表單，以允許存放區管理員，以編輯專輯。 將會在瀏覽**/StoreManager/Edit/id** URL (**識別碼**正在編輯專輯的唯一識別碼)，因而讓伺服器的 HTTP GET 呼叫。

編輯控制器動作方法會從資料庫擷取適當的專輯、 建立**StoreManagerViewModel**物件封裝 （以及演出者與內容類型的清單），然後將它，傳遞至檢視範本呈現的 HTML 網頁傳回給使用者。 此頁面將包含**&lt;表單&gt;**具有文字方塊和下拉式清單編輯專輯屬性的項目。

一旦使用者更新專輯表單值，並按一下**儲存** 按鈕，將變更送出透過 HTTP POST 呼叫回**/StoreManager/Edit/id**。雖然 URL 會保留最後一次呼叫相同，ASP.NET MVC 識別，這次它是 HTTP POST，因此會執行不同的編輯動作方法 (一個使用裝飾**[HttpPost]**)。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>工作 1-實作 HTTP GET 編輯動作方法

在這個工作中，您將實作要擷取從資料庫中的適當相簿的編輯動作方法的 HTTP GET 版本以及所有的內容類型和演出者名稱清單。 它會封裝此資料到**StoreManagerViewModel**的最後一個步驟，然後將傳遞至轉譯與回應的檢視範本中定義的物件。

1. 開啟**開始**方案位於**來源/Ex3CreatingTheEditView/開始/**資料夾。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

   1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 開啟**StoreManagerController**類別。 若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。
3. 取代**HTTP-GET 編輯**動作方法，以下列程式碼來擷取適當**專輯**以及**內容類型**和**演出者**列出。

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex3 StoreManagerController HTTP-GET 編輯動作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > 您使用**System.Web.Mvc** **SelectList**演出者和內容類型，而不是**System.Collections.Generic**清單。
    > 
    > **SelectList**是清楚的方式，來填入 HTML 下拉式清單和管理目前選取範圍等。 具現化及更新版本中控制器動作的這些 ViewModel 物件設定將編輯表單案例清理。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>工作 2-建立編輯檢視

在這項工作，您將建立稍後將會顯示專輯屬性編輯檢視範本。

1. 建立編輯檢視。 若要這樣做，以滑鼠右鍵按一下**編輯**動作方法，然後選取**加入檢視**。
2. 在 [新增檢視] 對話方塊中，確認檢視表名稱為**編輯**。 請檢查**建立強型別檢視**核取方塊並選取**專輯 (MvcMusicStore.Models)**從**檢視資料類別**下拉式清單。 選取**編輯**從**Scaffold 範本**下拉式清單。 保留其他欄位的預設值，然後按**新增**。

    ![加入編輯檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "加入編輯檢視")

    *加入編輯檢視*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>工作 3-執行應用程式

在這個工作中，您將測試的**StoreManager** **編輯**檢視頁面會顯示屬性的值當做參數傳遞的相簿。

1. 按**F5**執行應用程式。
2. 在首頁上，啟動專案。 將 URL 變更為**/StoreManager/Edit/1**以確認已顯示屬性的值傳遞的相簿。

    ![瀏覽專輯的編輯檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "瀏覽專輯的編輯檢視")

    *瀏覽專輯的編輯檢視*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>工作 4-實作專輯編輯器範本上的下拉式清單

在這個工作中，您將會加入下拉式清單中最後一個工作中，建立的檢視範本，讓使用者可以選取從演出者與內容類型的清單。

1. 全部取代**專輯**fieldset 以下列的程式碼：

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > **Html.DropDownList** helper 已新增至呈現下拉式清單選擇演出者與內容類型。 參數傳遞至**Html.DropDownList**是：
    > 
    > 1. 表單欄位的名稱 (**&quot;ArtistId&quot;**)。
    > 2. **SelectList**之下拉式清單的值。

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>工作 5-執行應用程式

在這個工作中，您將測試的**StoreManager** **編輯**檢視頁面會顯示下拉式清單，而不是演出者 和 類型識別碼 文字欄位。

1. 按**F5**執行應用程式。
2. 在首頁上，啟動專案。 將 URL 變更為**/StoreManager/Edit/1**以確認它會顯示下拉式清單，而不是演出者 和 類型識別碼 文字欄位。

    ![瀏覽專輯的下拉式清單與編輯檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "具有下拉式清單的 瀏覽專輯編輯檢視")

    *瀏覽專輯編輯檢視，這次具有下拉式清單*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>工作 6： 實作 HTTP POST 編輯動作方法

現在，編輯檢視會顯示如預期般，您需要實作 HTTP POST 編輯動作方法，以將變更儲存至相簿。

1. 關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。 開啟**StoreManagerController**從**控制器**資料夾。
2. 取代**HTTP POST 編輯**動作方法的程式碼以下列 （請注意，您必須將取代的方法接收兩個參數的多載的版本）：

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex3 StoreManagerController HTTP POST 編輯動作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > 這個方法將會執行，當使用者按一下**儲存**檢視的按鈕，並執行 HTTP-張貼至伺服器的表單值將它們保存在資料庫中。 Decorator **[HttpPost]**表示方法應用於 HTTP POST 的那些案例。 這個方法會接受**專輯**物件。 ASP.NET MVC 會自動建立專輯物件，從張貼&lt;表單&gt;值。
    > 
    > 方法會執行下列步驟：
    > 
    > 1. 如果模型有效：
    > 
    >     1. 更新專輯中的項目將其標示為已修改物件的內容。
    >     2. 儲存變更並重新導向至索引檢視。
    > 2. 如果模型不是有效的它會填入與 ViewBag **GenreId**和**ArtistId**，則它會傳回與所接收的專輯物件，讓使用者檢視執行任何必要的更新。

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>工作 7： 執行應用程式

在這個工作中，您將測試的**StoreManager 編輯**檢視頁面實際上會將更新的專輯資料儲存在資料庫中。

1. 按**F5**執行應用程式。
2. 在首頁上，啟動專案。 將 URL 變更為**/StoreManager/Edit/1**。 將專輯標題來變更**負載**，然後按一下 **儲存**。 請確認專輯標題實際變更清單中的專輯。

    ![更新專輯](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "更新相簿")

    *更新相簿*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>練習 4： 加入建立檢視

既然**StoreManagerController**支援**編輯**能力，在此練習，您將學習如何加入建立檢視，讓儲存範本管理員將新專輯新增至應用程式。

如同具有編輯功能，您將要實作建立案例中，使用兩個不同的方法內**StoreManagerController**類別：

1. 當存放區管理員第一次瀏覽一個動作方法會顯示一個空白表單**/StoreManager/建立**URL。
2. 第二個動作方法將處理案例的存放區管理員以便按一下**儲存**按鈕中的表單，並將送回給的值**/StoreManager/建立**URL 為 HTTP POST。

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>工作 1-執行 HTTP GET 建立動作方法

在這項工作，您將要實作建立動作方法，來擷取所有的內容類型和演出者名稱清單，請到封裝這項資料的 HTTP GET 新版**StoreManagerViewModel**物件，然後將傳遞至檢視範本。

1. 開啟**開始**方案位於**來源/Ex4AddingACreateView/開始/**資料夾。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

   1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 開啟**StoreManagerController**類別。 若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。
3. 取代**建立**動作方法的程式碼以下列：

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex4 StoreManagerController HTTP-GET 建立動作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>工作 2-加入建立檢視

在這項工作，您將加入將會顯示新的 （空的） 專輯表單建立檢視範本。

1. 以滑鼠右鍵按一下**建立**動作方法，然後選取**加入檢視**。 這會顯示 [新增檢視] 對話方塊。
2. 在 [新增檢視] 對話方塊中，確認檢視表名稱為**建立**。 選取**建立強型別檢視**選項，然後選取**專輯 (MvcMusicStore.Models)**從**模型類別**下拉式清單和**建立**從**Scaffold 範本**下拉式清單。 保留其他欄位的預設值，然後按**新增**。

    ![加入建立檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "加入-a-建立-view.png")

    *加入建立檢視*
3. 更新**GenreId**和**ArtistId**欄位，使用下拉式清單，如下所示：

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>工作 3-執行應用程式

在這個工作中，您將測試的**StoreManager** **建立**檢視頁面會顯示一個空白的專輯表單。

1. 按**F5**執行應用程式。
2. 在首頁上，啟動專案。 將 URL 變更為**/StoreManager/建立**。 請確認的空白表單隨即填入新的專輯屬性。

    ![建立一個空白表單檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "建立一個空白表單檢視")

    *建立一個空白表單檢視*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>工作 4-實作 HTTP POST 建立動作方法

在這項工作，您將要實作建立動作方法會叫用，當使用者按一下的 HTTP POST 新版**儲存** 按鈕。 方法應該在資料庫中儲存新的相簿。

1. 關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。 開啟**StoreManagerController**類別。 若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。
2. 取代**HTTP POST 建立**動作方法的程式碼以下列：

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex4 StoreManagerController HTTP POST 建立動作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > 建立動作方法和上一個編輯動作很類似，但而不是為已修改，請設定物件，它會加入至內容。

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>工作 5-執行應用程式

在這個工作中，您將測試的**StoreManager 建立**檢視頁面可讓您建立新的相簿，然後重新導向至 StoreManager 索引檢視。

1. 按**F5**執行應用程式。
2. 在首頁上，啟動專案。 將 URL 變更為**/StoreManager/建立**。 填入資料的所有表單欄位新專輯、 類似下圖中的一項：

    ![建立相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "建立相簿")

    *建立相簿*
3. 請確認您取得重新導向至包含剛才建立的新專輯 StoreManager 索引檢視。

    ![建立新的相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "新建立的相簿")

    *建立新的相簿*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>練習 5： 處理刪除

刪除相簿的功能尚未實作。 這是本練習將會相關。 像之前，您會實作使用兩個不同的方法內的 Delete 案例**StoreManagerController**類別：

1. 其中一個動作方法將會顯示確認表單
2. 第二個動作方法會處理送出表單

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>工作 1-實作 HTTP GET Delete 動作方法

在這個工作中，您將實作要擷取的專輯資訊的刪除動作方法的 HTTP GET 版本。

1. 開啟**開始**方案位於**來源/Ex5HandlingDeletion/開始/**資料夾。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

   1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 開啟**StoreManagerController**類別。 若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。
3. 刪除控制器動作正是先前存放區詳細資料控制器動作相同： 它會查詢**專輯**物件從資料庫使用**識別碼**中提供 URL 與傳回適當**檢視**。 若要這樣做，取代 HTTP GET**刪除**動作方法的程式碼以下列：

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex5 處理刪除 HTTP-GET 刪除動作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. 以滑鼠右鍵按一下**刪除**動作方法，然後選取**加入檢視**。 這會顯示 [新增檢視] 對話方塊。
5. 在 [新增檢視] 對話方塊中，確認檢視表名稱為**刪除**。 選取**建立強型別檢視**選項，然後選取**專輯 (MvcMusicStore.Models)**從**模型類別**下拉式清單。 選取**刪除**從**Scaffold 範本**下拉式清單。 保留其他欄位的預設值，然後按**新增**。

    ![加入刪除檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "加入刪除檢視")

    *加入刪除檢視*
6. 刪除範本會顯示模型中的所有欄位。 您將會顯示只有專輯標題。 若要這樣做，請取代下列程式碼中檢視的內容：

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>工作 2-執行應用程式

在這個工作中，您將測試的**StoreManager** **刪除**檢視頁面會顯示確認刪除表單。

1. 按**F5**執行應用程式。
2. 在首頁上，啟動專案。 將 URL 變更為**/StoreManager**。 選取即可刪除一個專輯**刪除**並驗證已上傳新的檢視。

    ![刪除相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "刪除相簿")

    *刪除相簿*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>工作 3-實作 HTTP POST 的 Delete 動作方法

在這個工作中，您將實作當使用者按一下就會叫用的 Delete 動作方法的 HTTP POST 新版**刪除** 按鈕。 此方法應該刪除資料庫中的專輯。

1. 關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。 開啟**StoreManagerController**類別。 若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。
2. 取代**HTTP POST 刪除**動作方法的程式碼以下列：

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex5 處理刪除 HTTP POST 刪除動作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>工作 4-執行應用程式

在這個工作中，您將測試的**StoreManager 刪除**檢視頁面可讓您刪除相簿，然後重新導向至 StoreManager 索引檢視。

1. 按**F5**執行應用程式。
2. 在首頁上，啟動專案。 將 URL 變更為**/StoreManager**。 選取即可刪除一個專輯**刪除。** 按一下以確認刪除動作**刪除**按鈕：

    ![刪除相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "刪除相簿")

    *刪除相簿*
3. 確認已刪除的專輯，因為它不會顯示在**索引**頁面。

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>練習 6： 加入驗證

目前，您已備妥的建立與編輯表單不會執行任何一種驗證。 如果使用者必要的欄位空白或型別字母離開 [價格] 欄位中，就會在第一個錯誤將會從資料庫中。

您可以加入至應用程式的驗證，將資料註解加入至您的模型類別。 資料註解允許描述您想要套用至您的模型內容的規則和 ASP.NET MVC 會負責強制執行和顯示適當的訊息給使用者。

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>工作 1-新增資料註解

在這項工作，您將加入資料註解專輯模型，可讓建立與編輯頁面顯示時適當的驗證訊息。

針對簡單的模型類別，加入資料註解只處理所加入**使用**陳述式**System.ComponentModel.DataAnnotation**，然後放置**[必要]**上適當的內容屬性。 下列範例會使**名稱**屬性在檢視中的必要欄位。

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

這是稍微複雜，就像此應用程式的情況下實體資料模型，產生的位置。 如果您資料註解直接加入模型類別，它們就會覆寫更新資料庫中的模型。 相反地，您可使用它來保存 註解會存在，並與模型相關聯的中繼資料部分類別的類別使用**[MetadataType]**屬性。

1. 開啟**開始**方案位於**來源/Ex6AddingValidation/開始/**資料夾。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

   1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 開啟**Album.cs**從**模型**資料夾。
3. 取代**Album.cs**內容以反白顯示的程式碼，使它看起來如下所示：

    > [!NOTE]
    > 線條**[DisplayFormat(ConvertEmptyStringToNull=false)]**代表模型中的空字串不會轉換為 null 的資料來源中更新資料欄位時。 Entity Framework 會指派至模型 null 值之前資料註解驗證欄位時，此設定可避免例外狀況。

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex6 專輯中繼資料的部分類別*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > 這**專輯**部分類別具有**MetadataType**屬性指向**AlbumMetaData**資料註解的類別。 以下是一些您用來標註專輯模型的資料註解屬性：
    > 
    > - 必要-表示此屬性是必要的欄位
    > - DisplayName-定義要用於表單欄位和驗證訊息的文字
    > - DisplayFormat-指定資料欄位會顯示並格式化的方式。
    > - StringLength-定義的字串欄位的最大長度
    > - 範圍-提供數值欄位的最大和最小值
    > - ScaffoldColumn-可讓隱藏編輯器表單中的欄位

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>工作 2-執行應用程式

在這項工作，您將測試建立與編輯頁面驗證欄位，使用最後一個工作中選擇的顯示名稱。

1. 按**F5**執行應用程式。
2. 在首頁上，啟動專案。 將 URL 變更為**/StoreManager/建立**。 確認顯示名稱符合的部分類別中的項目 (例如**專輯封面 URL**而不是**AlbumArtUrl**)
3. 按一下**建立**，而不填滿表單。 請確認您取得對應的驗證訊息。

    ![驗證 [建立] 頁面中的欄位](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "驗證 [建立] 頁面中的欄位")

    *在 [建立] 頁面中驗證的欄位*
4. 您可以確認相同發生**編輯**頁面。 將 URL 變更為**/StoreManager/Edit/1**並確認 顯示名稱符合的部分類別中的項目 (例如**專輯封面 URL**而不是**AlbumArtUrl**)。 空白**標題**和**價格**欄位和按一下**儲存**。 請確認您取得對應的驗證訊息。

    ![驗證的欄位中編輯頁面](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *驗證的欄位中編輯頁面*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>練習 7： 在用戶端使用不顯眼的 jQuery

在此練習中，您將學習如何啟用 MVC 4 不顯眼的 jQuery 驗證用戶端。

> [!NOTE]
> 不顯眼的 jQuery 會使用資料 ajax 首碼 JavaScript 來叫用伺服器，而非 intrusively 發出內嵌用戶端指令碼的動作方法。


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>工作 1-執行之前啟用不顯眼的 jQuery 應用程式

在這個工作中，您將執行的應用程式，才能包括 jQuery 才能比較這兩種驗證模式。

1. 開啟**開始**方案位於**來源/Ex7UnobtrusivejQueryValidation/開始/**資料夾。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

   1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 按 **F5** 執行應用程式。
3. 在首頁上，啟動專案。 瀏覽**/StoreManager/建立**按一下**建立**沒有填滿表單，以確認您看到驗證訊息：

    ![停用用戶端驗證](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "停用用戶端驗證")

    *停用用戶端驗證*
4. 在瀏覽器中開啟 HTML 程式碼：

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>工作 2-啟用不顯眼的用戶端驗證

在這個工作中，您將啟用 jQuery**不顯眼的用戶端驗證**從**Web.config**檔案，也就是預設值為 false，所有新的 ASP.NET MVC 4 專案中。 此外，您會加入必要的指令碼進行 jQuery 不顯眼的用戶端驗證工作的參考。

1. 開啟**Web.Config**檔案在專案的根目錄，並確定**ClientValidationEnabled**和**UnobtrusiveJavaScriptEnabled**金鑰值都會設為**true**。

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > 您也可以啟用用戶端驗證的程式碼中 Global.asax.cs 以取得相同的結果：
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > 此外，您可以指派 ClientValidationEnabled 屬性到任何控制器有自訂的行為。
2. 開啟**Create.cshtml**在**Views\StoreManager**。
3. 下列指令碼檔案，請確定**jquery.validate**和**jquery.validate.unobtrusive**，都會在檢視透過&quot; **~/bundles/jqueryval**&quot;組合。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > 所有這些 jQuery 程式庫會包含在新的 MVC 4 專案。 您可以找到更多程式庫中的**/指令碼**您專案的資料夾。
    > 
    > 若要讓這項驗證程式庫工作，您需要加入 jQuery framework 程式庫的參考。 因為中已加入此參考 **\_Layout.cshtml**檔案中，您不需要將它加入這個特定的檢視中。

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>工作 3-執行應用程式使用不顯眼的 jQuery 驗證

在這個工作中，您將測試的**StoreManager**建立範本執行的用戶端驗證使用 jQuery 程式庫，當使用者建立新的相簿的檢視。

1. 按 **F5** 執行應用程式。
2. 在首頁上，啟動專案。 瀏覽**/StoreManager/建立**按一下**建立**沒有填滿表單，以確認您看到驗證訊息：

    ![使用 jQuery 啟用的用戶端驗證](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "jQuery 啟用與用戶端驗證")

    *使用 jQuery 啟用的用戶端驗證*
3. 在瀏覽器中開啟 建立檢視的程式碼：

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > 每個用戶端驗證規則不顯眼的 jQuery 加入資料屬性-val-*rulename*=&quot;*訊息*&quot;。 以下是一份標記該 Unobtrusive jQuery 插入 html 輸入欄位，以執行用戶端驗證：
   > 
   > - 資料值
   > - Data-val-number
   > - 資料 val 範圍
   > - 資料 val 範圍-min/資料 val-range max
   > - Data-val-required
   > - 資料 val 長度
   > - 資料值的長度上限/資料 val 長度-分鐘
   > 
   > 所有的資料值會填入模型**資料註解**。 然後，在伺服器端的所有邏輯可以都執行用戶端。 例如，價格屬性都具有下列資料註解模型中：
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > 使用不顯眼的 jQuery 之後, 產生的程式碼是：
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>總結

藉由完成這個實際操作實驗室中，您已經學會如何讓使用者變更儲存在資料庫中使用下列資料：

- 控制器動作，例如索引，建立、 編輯、 刪除
- ASP.NET MVC 的 scaffolding 功能 HTML 表格中顯示屬性
- 自訂 HTML helper 來改善使用者體驗
- 回應 HTTP GET 或 HTTP POST 呼叫的動作方法
- 類似的檢視範本，例如建立與編輯共用的編輯器範本
- 下拉式清單類似的表單項目
- 模型驗證的資料註解
- 使用 jQuery 不顯眼的程式庫的用戶端驗證

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附錄 a： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 與 Windows Azure SDK</em>&quot;。
2. 按一下**立即安裝**。 如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。

    ![接受授權條款](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *接受授權條款*
5. 等待直到完成下載和安裝程序。

    ![安裝進度](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *安裝進度*
6. 當安裝完成時，按一下**完成**。

    ![安裝已完成](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *安裝已完成*
7. 按一下**結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。

    ![VS Express for Web 方塊](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express for Web 方塊*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>附錄 b： 使用程式碼片段

程式碼片段，您會有您需要在您可以的所有程式碼。 實驗室文件會告訴您完全時您可以使用它們，如下圖所示。

![若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "使用 Visual Studio 程式碼片段，將程式碼插入您的專案")

*若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段*

***若要加入的程式碼片段，使用鍵盤 （僅限 C#)***

1. 將游標放在您想要插入的程式碼。
2. 開始鍵入片段名稱 （不含空格或連字號）。
3. 觀察 IntelliSense 會顯示比對程式碼片段的名稱。
4. 選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。
5. 按下 Tab 鍵兩次的游標位置插入程式碼片段。

![開始輸入程式碼片段名稱](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "開始鍵入片段名稱")

*開始輸入程式碼片段名稱*

![按 Tab 鍵選取反白顯示的程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "按 Tab 鍵以選取反白顯示的程式碼片段")

*按 Tab 鍵選取反白顯示的程式碼片段*

![按 Tab 鍵一次程式碼片段就會擴展](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "按 Tab 鍵一次程式碼片段就會擴展")

*按 Tab 鍵一次程式碼片段就會擴展*

***若要加入的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。 以滑鼠右鍵按一下您要插入程式碼片段。

1. 選取**插入程式碼片段**後面**我的程式碼片段**。
2. 透過在按一下挑選清單中，從相關的程式碼片段。

![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")

*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*

![透過在按一下挑選清單中，從相關的程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "上它即可挑選清單中，從相關的程式碼片段")

*透過在按一下挑選清單中，從相關的程式碼片段*
