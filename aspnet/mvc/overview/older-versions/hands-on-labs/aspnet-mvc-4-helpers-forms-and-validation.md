---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 協助程式、 表單和驗證 |Microsoft Docs
author: rick-anderson
description: 在 ASP.NET MVC 4 模型和資料存取實際操作實驗室中，您已載入並顯示資料庫中的資料。 在這個實際操作實驗室中，您會將加入...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: a84e35695fa08ac1bd4834d2803d2be76f863e5b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815917"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>ASP.NET MVC 4 協助程式、 表單和驗證

藉由[Web Camp 小組](https://twitter.com/webcamps)

[下載 Web 研討會訓練套件](https://aka.ms/webcamps-training-kit)

在  **ASP.NET MVC 4 模型和資料存取**實際操作實驗室中，您已載入和顯示資料庫中的資料。 在這個實際操作實驗室中，您將加入至**音樂市集**應用程式編輯該資料的能力。

記住這個目標，您會先建立將支援建立、 讀取、 更新和刪除 (CRUD) 動作的相簿的控制站。 您將會產生利用 ASP.NET MVC 樣板功能，以顯示 HTML 表格中的專輯屬性的索引檢視範本。 若要增強該檢視，您將加入將截斷的詳細描述的自訂 HTML helper。

之後，您將加入的編輯和建立的檢視表，可讓您改變資料庫，藉助表單元素，例如下拉式清單中的專輯。

最後，您會讓使用者刪除 album，也會讓使用者無法輸入錯誤的資料來驗證他們的意見。

這個實際操作實驗室會假設您有的基本知識**ASP.NET MVC**。 如果您未曾使用**ASP.NET MVC**之前，我們建議您將逐一**ASP.NET MVC 基礎**實際操作實驗室。

這個實驗室會引導您完成先前所述將微幅的變更套用到來源資料夾中提供的範例 Web 應用程式的新功能與增強功能。

> [!NOTE]
> 所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 這個實驗室中的特定專案將會位於[ASP.NET MVC 4 Helper、 表單和驗證](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation)。

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將了解如何：

- 建立控制器能夠支援 CRUD 作業
- 產生 HTML 表格中顯示實體屬性的索引 檢視
- 新增自訂的 HTML 協助程式
- 建立和自訂 [編輯] 檢視
- 區分，以回應 HTTP-GET 或 HTTP-POST 呼叫動作方法
- 新增並自訂 Create View
- 處理刪除的實體
- 驗證使用者輸入

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

您必須具備下列項目，即可完成此實驗室：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更好 (讀取[附錄 A](#AppendixA)如需有關如何安裝它)。

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

**安裝程式碼片段**

為了方便起見，大部分的程式碼，您將沿著這個實驗室管理可從 Visual Studio 程式碼片段。 若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。

如果您不熟悉 Visual Studio 程式碼片段，而且想要了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 b： 使用程式碼片段](#AppendixB)&quot;。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>練習

下列練習，組成此實際操作實驗室：

1. [建立存放區管理員控制站和其索引檢視](#Exercise1)
2. [新增 HTML 協助程式](#Exercise2)
3. [建立 [編輯] 檢視](#Exercise3)
4. [加入建立檢視](#Exercise4)
5. [處理刪除](#Exercise5)
6. [新增驗證](#Exercise6)
7. [使用不顯眼的 jQuery 用戶端](#Exercise7)

> [!NOTE]
> 每個練習會伴隨**結束**包含完成練習之後，您應該取得所產生的方案資料夾。 如果您需要的所有練習所使用的其他說明，您可以使用此解決方案作為指南。


估計的時間才能完成這個實驗室： **60 分鐘**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>練習 1： 建立存放區管理員控制站和其索引檢視

在此練習中，您將了解如何建立新的控制站，以支援 CRUD 作業，來自訂要從資料庫最後產生的索引檢視範本，利用 ASP.NET MVC scaffolding 傳回一份專輯其索引動作方法顯示 HTML 表格中的專輯屬性的功能。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>工作 1-建立 StoreManagerController

在這個工作中，您將建立新的控制器，稱為**StoreManagerController**支援 CRUD 作業。

1. 開啟**開始**解決方案位於**來源/Ex1-CreatingTheStoreManagerController/開始/** 資料夾。

   1. 您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 加入新的控制器。 若要這樣做，請以滑鼠右鍵按一下**控制器**在 [方案總管] 中選取的資料夾**新增**，然後**控制器**命令。 變更**控制器****名稱**來**StoreManagerController** ，並確定選擇**MVC 控制器具有空白讀取/寫入動作**已選取。 按一下 [加入] 。

    ![新增控制器 對話方塊](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "新增控制器 對話方塊")

    *新增控制器 對話方塊*

    產生新的控制器類別。 因為您指定要為讀取/寫入，虛設常式方法，新增動作一般的 CRUD 動作會建立 TODO 註解中，填滿提示以包含應用程式特定邏輯。

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>工作 2-自訂 StoreManager 索引

在這個工作中，您將自訂 StoreManager 索引動作方法，從資料庫傳回專輯的清單檢視。

1. 在 StoreManagerController 類別中，新增下列*使用*指示詞。

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex1 使用 MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. 將欄位加入**StoreManagerController**保存的執行個體**MusicStoreEntities。**

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. 實作 StoreManagerController 索引動作傳回專輯的清單檢視。

    控制器動作邏輯會非常類似於先前撰寫的 StoreController Index 動作。 使用 LINQ 來擷取所有的專輯，包括顯示的內容類型與演出者資訊。

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex1 StoreManagerController 索引*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>工作 3-建立索引檢視

在這個工作中，您將建立索引檢視範本，以顯示所傳回的相簿**StoreManager**控制站。

1. 然後再建立新的檢視範本，您應該建置專案，讓**新增檢視 對話方塊**知道**專輯**類別使用。 選取**建置 |建置 MvcMusicStore**來建置專案。
2. 以滑鼠右鍵按一下**Index**動作方法，然後選取**加入檢視**。 此時會出現**加入檢視**對話方塊。

    ![新增檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "新增檢視")

    *加入從索引方法內的檢視*
3. 在 [新增檢視] 對話方塊中，確認檢視表名稱是否**Index**。 選取 **建立強型別檢視**選項，然後選取**專輯 (MvcMusicStore.Models)** 從**模型類別**下拉式清單。 選取 **清單**從**Scaffold 樣板**下拉式清單。 離開**檢視引擎**要**Razor**和其他欄位具有其預設值，然後按一下**新增**。

    ![加入索引檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "加入索引檢視")

    *加入索引檢視*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>工作 4-自訂索引 檢視的 scaffold

在這個工作中，您將調整使用 ASP.NET MVC 樣板功能，讓它顯示您想要的欄位建立簡單的檢視範本。

> [!NOTE]
> **Scaffolding**支援 ASP.NET MVC 內產生簡單的檢視範本會列出專輯模型中的所有欄位。 **Scaffolding**提供快速的方式，若要開始使用強型別檢視:，而不必手動撰寫檢視範本快速 scaffolding 會產生一個預設範本，然後您可以修改產生的程式碼。


1. 檢閱建立的程式碼。 產生的欄位清單將會屬於下列 HTML 資料表，其中**Scaffolding**用來顯示表格式資料。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. 取代**&lt;表格&gt;** 只顯示為下列程式碼的程式碼**Genre**，**演出者**，**專輯標題**，並**價格**欄位。 這會刪除**AlbumId**並**專輯封面 URL**資料行。 此外，它會變更 GenreId 和 ArtistId 資料行以顯示其連結的類別屬性**Artist.Name**並**Genre.Name**，並移除**詳細資料**連結。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. 變更下列的描述。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>工作 5-執行應用程式

在這個工作中，您將測試所**StoreManager** **Index**檢視範本會顯示一份根據先前的步驟設計的相簿。

1. 按下**F5**執行應用程式。
2. 專案一開始在首頁上。 將 URL 變更為 **/StoreManager**若要確認已顯示一份專輯，顯示其**標題**，**演出者**並**Genre**。

    ![瀏覽相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "瀏覽相簿")

    *瀏覽相簿*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>練習 2： 新增 HTML 協助程式

StoreManager 索引分頁具有一個可能的問題： 標題和演出者名稱的屬性都可以是夠長，會擲回關閉資料表格式。 在這個練習中，您將學習如何新增自訂的 HTML helper 來截斷文字。

在下圖中，您可以看到如何格式因為文字的長度時修改您使用較小的瀏覽器大小。

![瀏覽與專輯的清單不會被截斷的文字](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "瀏覽與專輯的清單不會被截斷的文字")

*瀏覽與專輯的清單不會被截斷的文字*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>工作 1-擴充的 HTML Helper

在這個工作中，您會將新方法加入**Truncate**要**HTML** ASP.NET MVC 檢視中公開的物件。 若要這樣做，您會實作**擴充方法**內建**System.Web.Mvc.HtmlHelper** ASP.NET MVC 所提供的類別。

> [!NOTE]
> 若要深入了解**擴充方法**，請瀏覽這篇 msdn 文章。 [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. 開啟**開始**解決方案位於**來源/Ex2-AddingAnHTMLHelper/開始/** 資料夾。 否則，您可能會繼續使用**結束**方案取得完成前一個練習。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 開啟 StoreManager 的索引檢視。 若要這樣做，請在 [方案總管] 中展開**檢視**資料夾，則**StoreManager** ，然後開啟**Index.cshtml**檔案。
3. 新增下列程式碼下方<strong>@model</strong>指示詞來定義<strong>Truncate</strong> helper 方法。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>工作 2-截斷網頁中的文字

在這個工作中，您將使用**Truncate**截斷檢視範本中的文字的方法。

1. 開啟 StoreManager 的索引檢視。 若要這樣做，請在 [方案總管] 中展開**檢視**資料夾，則**StoreManager** ，然後開啟**Index.cshtml**檔案。
2. 取代顯示的行會**演出者名稱**和 專輯**標題**。 若要這樣做，請取代下列幾行。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>工作 3-執行應用程式

在這個工作中，您將測試所**StoreManager** **Index**檢視範本會截斷的專輯標題和演出者名稱。

1. 按下**F5**執行應用程式。
2. 專案一開始在首頁上。 將 URL 變更為 **/StoreManager**若要確認這麼久的時間中的文字**標題**並**演出者**會截斷資料行。

    ![截斷標題和演出者名稱](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "截斷標題和演出者名稱")

    *截斷的標題和演出者名稱*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>練習 3： 建立 [編輯] 檢視

在此練習中，您將學習如何建立表單，以允許編輯 Album 的存放區管理員。 將會在瀏覽 **/StoreManager/Edit/id** URL (**識別碼**正在編輯專輯的唯一識別碼)，因而讓伺服器的 HTTP GET 呼叫。

編輯控制器動作方法會從資料庫擷取適當的專輯，建立**StoreManagerViewModel**物件封裝 （以及演出者和內容類型的清單），並再將它，傳遞至檢視範本轉譯的 HTML 網頁傳回給使用者。 此頁面將包含**&lt;表單&gt;** 具有文字方塊和下拉式清單編輯專輯屬性的項目。

一旦使用者更新專輯表單值並按**儲存** 按鈕，變更會提交透過 HTTP POST 回呼 **/StoreManager/Edit/id**。雖然 URL 會保留最後一次呼叫相同，ASP.NET MVC 識別，這次它是 HTTP POST，因此會執行不同的編輯動作方法 (其中附有 **[HttpPost]**)。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>工作 1-實作 HTTP GET Edit 動作方法

在這個工作中，您將實作 Edit 動作方法，來擷取從資料庫中的適當相簿的 HTTP GET 版本以及所有的內容類型和演出者名稱清單。 它會封裝這項資料到**StoreManagerViewModel**的最後一個步驟，然後將傳遞至轉譯回應的檢視範本中定義的物件。

1. 開啟**開始**解決方案位於**來源/Ex3-CreatingTheEditView/開始/** 資料夾。 否則，您可能會繼續使用**結束**方案取得完成前一個練習。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 開啟**StoreManagerController**類別。 若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。
3. 取代**HTTP-GET 編輯**動作方法，以下列程式碼來擷取適當**專輯**，以及**內容類型**並**演出者**列出。

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex3 StoreManagerController HTTP-GET 編輯動作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > 您使用**System.Web.Mvc** **SelectList**演出者和內容類型，而非**System.Collections.Generic**清單。
    > 
    > **SelectList**是簡潔的方式，來填入 HTML 下拉式清單和管理目前選取範圍等項目。 具現化及更新版本藉由設定這些控制器動作中的 ViewModel 物件將編輯表單案例更簡潔。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>工作 2-建立 [編輯] 檢視

在這個工作中，您將建立稍後將會顯示專輯屬性編輯的檢視範本。

1. 建立 [編輯] 檢視。 若要這樣做，以滑鼠右鍵按一下**編輯**動作方法，然後選取**加入檢視**。
2. 在 [新增檢視] 對話方塊中，確認檢視表名稱是否**編輯**。 請檢查**建立強型別檢視**核取方塊，然後選取**專輯 (MvcMusicStore.Models)** 從**檢視資料類別**下拉式清單。 選取 **編輯**從**Scaffold 樣板**下拉式清單。 保留其他欄位的預設值，然後按**新增**。

    ![加入編輯檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "加入編輯檢視")

    *加入編輯檢視*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>工作 3-執行應用程式

在這個工作中，您將測試所**StoreManager** **編輯**檢視頁面會顯示屬性的值當做參數傳遞的相簿。

1. 按下**F5**執行應用程式。
2. 專案一開始在首頁上。 將 URL 變更為 **/StoreManager/Edit/1**確認傳遞的相簿的屬性的值會顯示。

    ![瀏覽專輯的編輯檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "瀏覽專輯的編輯檢視")

    *瀏覽專輯的編輯檢視*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>工作 4-實作專輯編輯器範本上的下拉式清單

在這個工作中，您將建立在最後一個工作中，檢視範本加入下拉式清單，讓使用者可以選取從演出者和內容類型的清單。

1. 程式碼取代所有**專輯**fieldset 程式碼取代為下列：

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > **Html.DropDownList**協助程式已新增至呈現下拉式清單選擇演出者和內容類型。 參數傳遞給**Html.DropDownList**是：
    > 
    > 1. 表單欄位的名稱 (**&quot;ArtistId&quot;**)。
    > 2. **SelectList**的下拉箭號的值。

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>工作 5-執行應用程式

在這個工作中，您將測試所**StoreManager** **編輯**檢視頁面會顯示下拉式清單，而不是演出者和內容類型識別碼的文字欄位。

1. 按下**F5**執行應用程式。
2. 專案一開始在首頁上。 將 URL 變更為 **/StoreManager/Edit/1**以確認它會顯示下拉式清單，而不是演出者和內容類型識別碼的文字欄位。

    ![瀏覽具有下拉式清單的 專輯的編輯檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "具有下拉式清單的 瀏覽專輯編輯檢視")

    *瀏覽專輯的編輯檢視，這次使用下拉式清單*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>工作 6-實作編輯 HTTP POST 動作方法

現在，[編輯] 檢視會顯示如預期般運作，您需要實作 HTTP POST 編輯動作方法，以儲存至相簿所做的變更。

1. 關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。 開啟**StoreManagerController**從**控制站**資料夾。
2. 取代**HTTP-POST 編輯**動作方法的程式碼取代下列項目 （請注意，您必須取代的方法會接收兩個參數的多載的版本）：

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex3 StoreManagerController HTTP-POST 編輯動作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > 這個方法會執行，當使用者按一下**儲存**檢視的按鈕，並執行 HTTP POST 至伺服器之表單值將它們保存在資料庫中。 裝飾項目 **[HttpPost]** 表示方法應該用於 HTTP POST 的那些案例。 此方法會採用**專輯**物件。 ASP.NET MVC 會自動建立的專輯物件，從張貼&lt;表單&gt;值。
    > 
    > 方法會執行下列步驟：
    > 
    > 1. 如果模型有效：
    > 
    >     1. 更新專輯中的項目將它標示為已修改物件的內容。
    >     2. 儲存變更並重新導向至 [索引] 檢視。
    > 2. 如果該模型不是有效的它將會填入與 ViewBag **GenreId**並**ArtistId**，則它會傳回與接收的專輯物件，以允許使用者檢視執行任何必要的更新。

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>工作 7-執行應用程式

在這個工作中，您將測試所**StoreManager 編輯**檢視頁面實際上會將更新的專輯資料儲存在資料庫中。

1. 按下**F5**執行應用程式。
2. 專案一開始在首頁上。 將 URL 變更為 **/StoreManager/Edit/1**。 將專輯標題來變更**負載**，然後按一下**儲存**。 請確認專輯標題實際變更清單中的專輯。

    ![更新 album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "更新相簿")

    *更新相簿*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>練習 4： 加入建立檢視

既然**StoreManagerController**支援**編輯**能力，在這個練習中您會了解如何加入建立檢視範本，讓儲存管理員應用程式中加入新的相簿。

像您一樣的編輯功能，您會實作使用兩個不同的方法內建立案例**StoreManagerController**類別：

1. 其中一個動作方法會顯示空白的表單，當存放區管理員第一次造訪**StoreManager/建立**URL。
2. 第二個動作方法會處理案例的存放區管理員，按一下**儲存**表單內的按鈕，然後送出的值將會回到**StoreManager/建立**URL 為 HTTP POST。

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>工作 1-實作 HTTP GET 建立動作方法

在這個工作中，您將實作建立動作方法，若要擷取所有的內容類型和演出者名稱清單，請封裝此資料至的 HTTP GET 新版**StoreManagerViewModel**物件，然後將傳遞至檢視範本。

1. 開啟**開始**解決方案位於**來源/Ex4-AddingACreateView/開始/** 資料夾。 否則，您可能會繼續使用**結束**方案取得完成前一個練習。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 開啟**StoreManagerController**類別。 若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。
3. 取代**建立**動作方法的程式碼取代為下列：

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex4 StoreManagerController HTTP-GET 建立動作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>工作 2-加入建立檢視

在這個工作中，您將加入建立檢視範本，將會顯示新的 （空的） 專輯表單。

1. 以滑鼠右鍵按一下**Create**動作方法，然後選取**加入檢視**。 這會顯示 [新增檢視] 對話方塊。
2. 在 [新增檢視] 對話方塊中，確認檢視表名稱是否**建立**。 選取 **建立強型別檢視**選項，然後選取**專輯 (MvcMusicStore.Models)** 從**模型類別**下拉式清單和**建立**從**Scaffold 樣板**下拉式清單。 保留其他欄位的預設值，然後按**新增**。

    ![加入建立檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "新增-a-建立-view.png")

    *加入建立檢視*
3. 更新**GenreId**並**ArtistId**欄位，使用下拉式清單，如下所示：

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>工作 3-執行應用程式

在這個工作中，您將測試所**StoreManager** **建立**檢視頁面會顯示一個空白的專輯表單。

1. 按下**F5**執行應用程式。
2. 專案一開始在首頁上。 將 URL 變更為**StoreManager/建立**。 確認的空白表單會顯示為填滿新專輯屬性。

    ![使用空白的表單建立檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "建立檢視的空白表單")

    *使用空白的表單建立檢視*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>工作 4-實作 HTTP POST 建立動作方法

在這個工作中，您將實作會叫用，當使用者按一下建立動作方法的 HTTP POST 新版**儲存** 按鈕。 此方法應該在資料庫中儲存新的相簿。

1. 關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。 開啟**StoreManagerController**類別。 若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。
2. 取代**HTTP POST 建立**動作方法的程式碼取代為下列：

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex4 StoreManagerController HTTP POST 建立動作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > 建立動作相當類似於上一個編輯動作方法，但而不是為已修改，請設定物件，它會新增至內容。

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>工作 5-執行應用程式

在這個工作中，您將測試所**StoreManager 建立**檢視頁面可讓您建立新的相簿，，然後重新導向至 StoreManager 索引 檢視。

1. 按下**F5**執行應用程式。
2. 專案一開始在首頁上。 將 URL 變更為**StoreManager/建立**。 新的專輯、 類似下圖中填入資料的所有表單欄位：

    ![建立相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "建立相簿")

    *建立相簿*
3. 請確認您會被重新導向，以包含剛才建立的新專輯 StoreManager 索引檢視。

    ![建立新的相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "新建立的相簿")

    *建立新的相簿*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>練習 5： 處理刪除

尚未實作刪除 album 的能力。 這是此練習中會有何相關。 像之前，您將在此實作使用兩個不同的方法中的刪除案例**StoreManagerController**類別：

1. 其中一個動作方法會顯示確認表單
2. 第二個動作方法將處理表單提交

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>工作 1-實作 HTTP GET Delete 動作方法

在這個工作中，您將實作要擷取的專輯資訊的 Delete 動作方法的 HTTP GET 版本。

1. 開啟**開始**解決方案位於**來源/Ex5-HandlingDeletion/開始/** 資料夾。 否則，您可能會繼續使用**結束**方案取得完成前一個練習。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 開啟**StoreManagerController**類別。 若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。
3. 刪除控制器動作正是與先前的存放區詳細資料控制器動作相同： 它會查詢**專輯**從資料庫使用的物件**識別碼**提供的 URL 和傳回適當**檢視**。 若要這樣做，請將 HTTP GET**刪除**動作方法的程式碼取代為下列：

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex5 處理刪除 HTTP-GET 刪除動作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. 以滑鼠右鍵按一下**刪除**動作方法，然後選取**加入檢視**。 這會顯示 [新增檢視] 對話方塊。
5. 在 [新增檢視] 對話方塊中，確認檢視表名稱是否**刪除**。 選取 **建立強型別檢視**選項，然後選取**專輯 (MvcMusicStore.Models)** 從**模型類別**下拉式清單。 選取 **刪除**從**Scaffold 樣板**下拉式清單。 保留其他欄位的預設值，然後按**新增**。

    ![加入刪除檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "加入刪除檢視")

    *加入刪除檢視*
6. 刪除範本會顯示模型中的所有欄位。 您將會顯示只有專輯標題。 若要這樣做，請取代下列程式碼中檢視的內容：

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>工作 2-執行應用程式

在這個工作中，您將測試所**StoreManager** **刪除**檢視頁面會顯示確認刪除表單。

1. 按下**F5**執行應用程式。
2. 專案一開始在首頁上。 將 URL 變更為 **/StoreManager**。 選取即可刪除一個專輯**刪除**並確認已上傳新的檢視。

    ![刪除 Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "刪除 Album")

    *刪除 Album*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>工作 3-實作 HTTP POST 的 Delete 動作方法

在這個工作中，您將實作會叫用，當使用者按一下的 Delete 動作方法的 HTTP POST 新版**刪除** 按鈕。 此方法應該刪除資料庫中的專輯。

1. 關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。 開啟**StoreManagerController**類別。 若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。
2. 取代**HTTP POST Delete**動作方法的程式碼取代為下列：

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex5 處理刪除 HTTP-POST 刪除動作*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>工作 4-執行應用程式

在這個工作中，您將測試所**StoreManager 刪除**檢視頁面可讓您刪除 Album，，然後重新導向至 StoreManager 索引 檢視。

1. 按下**F5**執行應用程式。
2. 專案一開始在首頁上。 將 URL 變更為 **/StoreManager**。 選取即可刪除一個專輯**刪除。** 按一下確認刪除**刪除**按鈕：

    ![刪除 Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "刪除 Album")

    *刪除 Album*
3. 確認已刪除 album，因為它不會顯示在**Index**頁面。

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>練習 6： 新增驗證

目前，您已備妥的 Create 和 Edit 表單不會執行任何一種驗證。 如果使用者離開的必要的欄位留空或輸入字母，在 [價格] 欄位中，您會收到的第一個錯誤將會從資料庫。

您可以新增至應用程式的驗證，藉由將您的模型類別中的資料註解。 資料註解允許描述套用至您的模型屬性，您需要的規則和 ASP.NET MVC 會負責強制執行，並向使用者顯示適當的訊息。

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>工作 1-新增資料註解

在這個工作中，您將加入資料註解，可讓建立與編輯頁面的專輯模型顯示在適當的驗證訊息。

對於簡單的模型類別，加入資料註解只處理加上**使用**陳述式**System.ComponentModel.DataAnnotation**，然後放置 **[必要]** 適當屬性的屬性。 下列範例會使**名稱**屬性檢視的必要欄位。

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

這是此應用程式的情況稍微複雜 Entity Data Model 產生的位置。 如果您的資料註解直接新增至模型類別，它們會覆寫您從資料庫更新模型。 相反地，您可以進行使用它來保存註解會存在，並與模型相關聯的中繼資料部分類別的類別使用 **[MetadataType]** 屬性。

1. 開啟**開始**解決方案位於**來源/Ex6-AddingValidation/開始/** 資料夾。 否則，您可能會繼續使用**結束**方案取得完成前一個練習。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 開啟**Album.cs**從**模型**資料夾。
3. 取代**Album.cs**內容以反白顯示的程式碼，使它看起來如下所示：

    > [!NOTE]
    > 線條 **[DisplayFormat(ConvertEmptyStringToNull=false)]** 代表模型中的空字串不會轉換為 null 的資料來源中更新資料欄位時。 當 Entity Framework 會指派 null 值的模型資料註解驗證欄位之前，此設定可避免例外狀況。

    (程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex6 專輯中繼資料的部分類別*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > 這**專輯**部分類別具有**MetadataType**屬性，指向**AlbumMetaData**資料註解的類別。 以下是一些您用來標註的專輯模型的資料註解屬性：
    > 
    > - 必要-指出屬性是否為必填的欄位
    > - DisplayName-定義用於表單欄位和驗證訊息的文字
    > - Displayformat 無法為指定資料欄位會顯示並格式化的方式。
    > - StringLength-定義的字串欄位的最大長度
    > - 範圍-提供最大和最小值為數值欄位
    > - ScaffoldColumn-可讓隱藏於編輯器表單的欄位

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>工作 2-執行應用程式

在這個工作中，您將測試 Create 和 Edit 頁面驗證欄位中，使用在前一項工作中選擇的顯示名稱。

1. 按下**F5**執行應用程式。
2. 專案一開始在首頁上。 將 URL 變更為**StoreManager/建立**。 確認 顯示名稱相符的部分類別中的項目 (例如**專輯封面 URL**而不是**AlbumArtUrl**)
3. 按一下 **建立**，而不需要填入表單。 請確認您取得對應的驗證訊息。

    ![驗證 [建立] 頁面中的欄位](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "驗證在 [建立] 頁面中的欄位")

    *在 [建立] 頁面中的驗證的欄位*
4. 您可以確認相同發生**編輯**頁面。 將 URL 變更為 **/StoreManager/Edit/1** ，並確認 顯示名稱相符的部分類別中的項目 (例如**專輯封面 URL**而非**AlbumArtUrl**)。 空**標題**並**價格**欄位，然後按一下 **儲存**。 請確認您取得對應的驗證訊息。

    ![在 [編輯] 頁面中的驗證的欄位](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *在 [編輯] 頁面中的驗證的欄位*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>練習 7： 使用不顯眼的 jQuery 用戶端

在此練習中，您將學習如何啟用 MVC 4 不顯眼的 jQuery 驗證，在用戶端。

> [!NOTE]
> 不顯眼的 jQuery 會使用資料 ajax 前置詞 JavaScript 來叫用伺服器，而非干擾的方式發出內嵌用戶端指令碼的動作方法。


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>工作 1-執行之前啟用不顯眼的 jQuery 應用程式

在這個工作中，您將執行的應用程式，再以比較這兩種驗證模型包括 jQuery。

1. 開啟**開始**解決方案位於**來源/Ex7-UnobtrusivejQueryValidation/開始/** 資料夾。 否則，您可能會繼續使用**結束**方案取得完成前一個練習。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 按 **F5** 執行應用程式。
3. 專案一開始在首頁上。 瀏覽**StoreManager/Create**然後按一下**建立**沒有填滿表單，以確認您取得驗證訊息：

    ![停用的用戶端驗證](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "停用的用戶端驗證")

    *停用的用戶端驗證*
4. 在瀏覽器中開啟 HTML 程式碼：

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>工作 2-啟用不顯眼的用戶端驗證

在這個工作中，您將啟用 jQuery**不顯眼的用戶端驗證**從**Web.config**預設會設定為 false，所有新的 ASP.NET MVC 4 專案中的檔案。 此外，您會加入必要的指令碼進行 jQuery 低調的用戶端驗證工作的參考。

1. 開啟**Web.Config**檔案位於專案根目錄，並確定**ClientValidationEnabled**並**UnobtrusiveJavaScriptEnabled**金鑰值都會設為 **，則為 true**。

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > 您也可以啟用用戶端驗證的程式碼在 Global.asax.cs 以取得相同的結果：
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > 此外，您可以將 ClientValidationEnabled 屬性指派至任何控制器，將自訂行為。
2. 開啟**Create.cshtml**在**Views\StoreManager**。
3. 下列指令碼檔案中，請確定**jquery.validate**並**jquery.validate.unobtrusive**中檢視透過, 參考&quot; **~/bundles/jqueryval**&quot;套件組合。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > 所有這些 jQuery 程式庫會包含在新的 MVC 4 專案。 您可以找到更多的程式庫中 **/指令碼**您專案的資料夾。
    > 
    > 若要讓這項驗證程式庫運作，您需要新增 jQuery framework 程式庫的參考。 因為在已加入這個參考 **\_Layout.cshtml**檔案中，您不需要將它加入這個特定的檢視中。

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>工作 3-執行應用程式使用不顯眼的 jQuery 驗證

在這個工作中，您將測試所**StoreManager**建立範本會執行在使用者建立新專輯時，使用 jQuery 程式庫的用戶端驗證的檢視。

1. 按 **F5** 執行應用程式。
2. 專案一開始在首頁上。 瀏覽**StoreManager/Create**然後按一下**建立**沒有填滿表單，以確認您取得驗證訊息：

    ![用戶端驗證與啟用的 jQuery](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "與啟用的 jQuery 用戶端驗證")

    *使用 jQuery 啟用的用戶端驗證*
3. 在瀏覽器中，開啟 建立檢視的原始程式碼：

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > 每個用戶端驗證規則不顯眼的 jQuery 加入資料屬性-val-*rulename*=&quot;*訊息*&quot;。 以下是一份標記該 Unobtrusive jQuery 插入 html 輸入欄位，來執行用戶端驗證：
   > 
   > - 資料值
   > - 資料值數目
   > - 資料值範圍
   > - 資料 val 範圍-分鐘/資料-val--最大範圍
   > - Val 需要資料
   > - 資料 val 長度
   > - 資料-val-長度-最大值/資料 val 長度-分鐘
   > 
   > 所有資料值會都填入模型**資料註解**。 然後，在伺服器端的所有邏輯可以在用戶端都執行。 比方說，價格屬性會在模型中，具有下列資料註解：
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

藉由完成這個實際操作實驗室中，您已了解如何讓使用者能夠變更藉由使用下列資料庫中儲存的資料：

- 控制器動作，例如索引，建立、 編輯、 刪除
- ASP.NET MVC 樣板功能，來以 HTML 表格顯示屬性
- 自訂 HTML 協助程式，以改善使用者體驗
- 動作方法，以回應 HTTP-GET 或 HTTP-POST 呼叫
- 類似的檢視範本，例如建立和編輯共用的編輯器範本
- Form 項目，例如下拉式清單
- 資料註解進行模型驗證
- 使用 jQuery 低調的程式庫的用戶端驗證

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附錄 a： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Windows Azure SDK</em>&quot;。
2. 按一下 **立即安裝**。 如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。

    ![接受授權條款](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *接受授權條款*
5. 等候完成的下載與安裝程序。

    ![安裝進度](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *安裝進度*
6. 安裝完成時，按一下**完成**。

    ![安裝已完成](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *安裝已完成*
7. 按一下 **結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。

    ![VS Express for Web 圖格](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express for Web 圖格*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>附錄 b： 使用程式碼片段

使用程式碼片段，您會有您需要在隨手可得的所有程式碼。 實驗室課程文件會告訴您完全時您可以使用它們，如下圖所示。

![若要將程式碼插入您的專案使用 Visual Studio 程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "程式碼插入您的專案使用 Visual Studio 程式碼片段")

*若要將程式碼插入您的專案使用 Visual Studio 程式碼片段*

***若要新增的程式碼片段，使用鍵盤 （僅限 C#)***

1. 將游標放在您想要插入的程式碼。
2. 開始輸入程式碼片段名稱 （不含空格或連字號）。
3. 觀看為 IntelliSense 會顯示相符的程式碼片段的名稱。
4. 選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。
5. 按下 Tab 鍵兩次的游標位置插入程式碼片段。

![開始鍵入程式碼片段名稱](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "開始輸入程式碼片段名稱")

*開始鍵入程式碼片段名稱*

![按 Tab 鍵以選取反白顯示的程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "按 Tab 鍵以選取反白顯示的程式碼片段")

*按 Tab 鍵以選取反白顯示的程式碼片段*

![再次按 Tab 鍵和程式碼片段會依序展開](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "再次按 Tab 鍵和程式碼片段會展開")

*再次按 Tab 鍵和程式碼片段會展開*

***若要新增的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。 以滑鼠右鍵按一下您要插入程式碼片段。

1. 選取 **插入程式碼片段**後面**My Code Snippets**。
2. 選擇相關的程式片段，從清單中，對它按一下。

![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")

*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*

![對它按一下挑選清單中，相關程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "對它按一下挑選清單中，相關程式碼片段")

*對它按一下挑選清單中，相關程式碼片段*
