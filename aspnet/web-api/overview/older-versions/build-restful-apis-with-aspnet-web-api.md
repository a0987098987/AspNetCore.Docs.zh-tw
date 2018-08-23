---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: 建置使用 ASP.NET Web API 的 RESTful Api |Microsoft Docs
author: rick-anderson
description: 近年來，它具有一目了然，HTTP 不只是提供 HTML 頁面。 它也是強大的平台，建置 Web Api，使用少數幾個 o...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 00304f67138873318b63c5e2ad0cd69aa7449521
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833001"
---
<a name="build-restful-apis-with-aspnet-web-api"></a>建置使用 ASP.NET Web API 的 RESTful Api
====================
藉由[Web Camp 小組](https://twitter.com/webcamps)

> 近年來，它具有一目了然，HTTP 不只是提供 HTML 頁面。 它也是強大的平台，建置 Web Api，使用少數幾個動詞 （GET、 POST 等） 再加上幾個簡單的概念，例如*Uri*並*標頭*。 ASP.NET Web API 是一組簡化 HTTP 程式設計的元件。 由於它建置在 ASP.NET MVC 執行階段上，Web API 會自動處理 HTTP 傳輸低階詳細資料。 在此同時，Web API，自然地公開 HTTP 程式設計模型。 事實上，Web API 的其中一個目標是要*不*摘除 HTTP 的事實。 如此一來，Web API 是彈性且容易擴充。 在這個實際操作實驗室中，您將使用 Web API 來建置簡單的 REST API 連絡人管理員應用程式。 您也將建置的用戶端取用 API。 事實證明的 REST 架構樣式會運用 HTTP-的有效方法，但是它當然不是 HTTP 的唯一有效方法。 連絡人的管理員會公開清單、 加入和移除連絡人，以及其他符合 rest 限制。 這個實驗室需要基本 HTTP，其餘部分，了解，並假設您有基本的 HTML、 JavaScript 和 jQuery 的實用知識。
> 
> > [!NOTE]
> > ASP.NET 網站有一個專門用來在 ASP.NET Web API 架構的區域[ https://asp.net/web-api ](https://asp.net/web-api)。 此站台將持續提供最新的資訊、 範例和新聞與 Web API，因此請經常如果您想要更深入地鑽研建立給幾乎任何裝置或開發架構，可用的自訂 Web Api 的圖案。
> > 
> > ASP.NET Web API，類似於 ASP.NET MVC 4 中，有很大的彈性方面開來，讓您可以使用數個可用的相依性插入架構相當簡單的控制站的服務層。 示範如何使用 Ninject 相依性插入，您可以下載從 ASP.NET Web API 專案中的 MSDN 中沒有良好的樣本[此處](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)。
> 
> 
> 所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。


<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將了解如何：

- 實作 RESTful Web API
- 從 HTML 用戶端呼叫 API

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

需要下列項目才能完成這個實際操作實驗室：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更好 (讀取[附錄 B](#AppendixB)如需有關如何安裝它)。

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

**安裝程式碼片段**

為了方便起見，大部分的程式碼，您將沿著這個實驗室管理可從 Visual Studio 程式碼片段。 若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。

如果您不熟悉 Visual Studio 程式碼片段，而且想要了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 a： 使用程式碼片段](#AppendixA)&quot;。

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室還包含下列練習：

1. [練習 1： 建立唯讀的 Web API](#Exercise1)
2. [練習 2： 建立讀取/寫入 Web API](#Exercise2)
3. [練習 3： 使用從 HTML 用戶端的 Web API](#Exercise3)

> [!NOTE]
> 每個練習會伴隨**結束**包含完成練習之後，您應該取得所產生的方案資料夾。 如果您需要的所有練習所使用的其他說明，您可以使用此解決方案作為指南。


完成這個實驗室估計時間： **60 分鐘**。

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>練習 1： 建立唯讀的 Web API

在此練習中，您將連絡人的 manager 實作的唯寫的 GET 方法。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>工作 1-建立 API 專案

在這個工作中，您將使用新的 ASP.NET web 專案範本建立 Web API 的 web 應用程式。

1. 執行**Visual Studio 2012 Express for Web**，以前往**開始**然後輸入**VS Express for Web**然後按**Enter**。
2. 從**檔案**功能表上，選取**新的專案**。 選取**Visual C# |Web**專案類型從 專案類型樹狀檢視中，然後選取**ASP.NET MVC 4 Web 應用程式**專案類型。 將此專案的**名稱**要*ContactManager*和**方案名稱**至*開始*，然後按一下 **確定**.

    ![建立新的 ASP.NET MVC 4.0 Web 應用程式專案](build-restful-apis-with-aspnet-web-api/_static/image1.png "建立新的 ASP.NET MVC 4.0 Web 應用程式專案")

    *建立新的 ASP.NET MVC 4.0 Web 應用程式專案*
3. 在 ASP.NET MVC 4 專案的 [類型] 對話方塊中，選取**Web API**專案類型。 按一下 [確定 **Deploying Office Solutions**]。

    ![指定的 Web API 專案型別](build-restful-apis-with-aspnet-web-api/_static/image2.png "指定 Web API 專案類型")

    *指定 Web API 專案類型*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>工作 2-建立連絡人管理員 API 控制器

在這個工作中，您將建立的 API 方法所在的控制器類別。

1. 刪除名為的檔案**ValuesController.cs**內**控制站**從專案的資料夾。
2. 以滑鼠右鍵按一下**控制器**資料夾中的專案，然後選取**新增 |控制器**從內容功能表。

    ![將新的控制站新增至專案](build-restful-apis-with-aspnet-web-api/_static/image3.png "專案中加入新的控制站")

    *將新的控制站新增至專案*
3. 在 **新增控制器**出現對話方塊中，選取**空白 API 控制器**從 範本 功能表。 將類別命名為控制器**ContactController**。 然後，按一下 **新增。**

    ![使用 [新增控制器] 對話方塊來建立新的 Web API 控制器](build-restful-apis-with-aspnet-web-api/_static/image4.png "使用 [新增控制器] 對話方塊來建立新的 Web API 控制器")

    *使用 [新增控制器] 對話方塊來建立新的 Web API 控制器*
4. 將下列程式碼加入**ContactController**。

    (程式碼片段- *Web API 實驗室-Ex01-Get API 方法*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. 按下**F5**偵錯應用程式。 預設的首頁上，為 Web API 專案應該會出現。

    ![ASP.NET Web API 應用程式的預設首頁](build-restful-apis-with-aspnet-web-api/_static/image5.png "ASP.NET Web API 應用程式的預設首頁")

    *ASP.NET Web API 應用程式的預設首頁*
6. 在 [Internet Explorer] 視窗中，按下**F12**鍵以開啟**開發人員工具**視窗。 按一下 **網路**索引標籤，然後再按一下**開始擷取**按鈕，開始擷取網路流量到視窗。

    ![開啟 [網路] 索引標籤，然後啟動網路擷取](build-restful-apis-with-aspnet-web-api/_static/image6.png "開啟 [網路] 索引標籤，然後啟動網路擷取")

    *開啟 [網路] 索引標籤，然後啟動網路擷取*
7. 附加與瀏覽器的網址列中的 URL **/api/contact** ，然後按 enter。 傳輸的詳細資料會出現在網路的 [擷取] 視窗中。 請注意，回應的 MIME 類型，則**application/json**。 這示範了如何的預設輸出格式為 JSON。

    ![在 [網路] 檢視中檢視 Web API 要求的輸出](build-restful-apis-with-aspnet-web-api/_static/image7.png "檢視在 [網路] 檢視中的 Web API 要求的輸出")

    *在 [網路] 檢視中檢視 Web API 要求的輸出*

    > [!NOTE]
    > 此時 Internet Explorer 10 的預設行為將會詢問使用者想要儲存或開啟 Web API 呼叫所產生的資料流。 輸出會包含 Web API URL 呼叫的 JSON 結果的文字檔。 不要取消對話方塊，才能夠觀賞回應的內容，透過開發人員工具視窗。
8. 按一下 **前往 詳細檢視**以查看有關此 API 呼叫的回應詳細資料 按鈕。

    ![切換至 詳細檢視](build-restful-apis-with-aspnet-web-api/_static/image8.png "切換至 詳細資料檢視")

    *切換至 詳細檢視*
9. 按一下 [**回應主體**若要檢視實際的 JSON 回應文字] 索引標籤。

    ![檢視 JSON 輸出網路監視器中的文字](build-restful-apis-with-aspnet-web-api/_static/image9.png "檢視 JSON 輸出網路監視器中的文字")

    *在 網路監視器中檢視 JSON 輸出文字*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>工作 3-建立連絡人的模型，以及增強的連絡人的控制站

在這個工作中，您將建立的 API 方法所在的控制器類別。

1. 以滑鼠右鍵按一下**模型**資料夾，然後選取**新增 |類別...** 從內容功能表。

    ![將新模型加入至 web 應用程式](build-restful-apis-with-aspnet-web-api/_static/image10.png "將新模型加入至 web 應用程式")

    *將新模型加入至 web 應用程式*
2. 在 [**加入新項目**] 對話方塊中，將新檔案命名**Contact.cs**按一下**新增。**

    ![建立新的連絡人類別檔案](build-restful-apis-with-aspnet-web-api/_static/image11.png "建立新的連絡人類別檔案")

    *建立新的連絡人類別檔案*
3. 將下列反白顯示的程式碼，加入**連絡人**類別。

    (程式碼片段- *Web API 實驗室-Ex01-連絡類別*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. 在  **ContactController**類別中，選取 word**字串**方法定義中**取得**方法，並輸入文字*連絡人*。 一旦這個字型別中，指標就會出現在單字開頭**連絡人**。 請按住**Ctrl**鍵並按下句號 （.） 索引鍵或按一下圖示以開啟 設定在程式碼編輯器中，會自動填入的 協助 對話方塊中使用滑鼠**使用**模型指示詞命名空間。

    ![為命名空間宣告中使用 Intellisense 協助](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *為命名空間宣告中使用 Intellisense 協助*
5. 修改程式碼**取得**方法，讓它傳回連絡人模型執行個體陣列。

    (程式碼片段-*傳回的連絡人清單 Web API 實驗室-Ex01-*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. 按下**F5**偵錯 web 應用程式，在瀏覽器中的。 若要檢視對 API 的回應輸出的變更，請執行下列步驟。

   1. 瀏覽器開啟後，請按下**F12**如果開發人員工具還不開啟。
   2. 按一下 [**網路**] 索引標籤。
   3. 按下**開始擷取** 按鈕。
   4. 新增 URL 尾碼 **/api/contact** url 網址列，然後按下**Enter**索引鍵。
   5. 按下**前往 [詳細檢視**] 按鈕。
   6. 選取 [**回應主體**] 索引標籤。您應該會看到 JSON 字串，代表序列化的形式的連絡執行個體陣列。

      ![JSON 序列化的複雜的 Web API 方法呼叫的輸出](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON 序列化的複雜的 Web API 方法呼叫的輸出")

      *複雜的 Web API 方法呼叫的序列化的 JSON 輸出*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>工作 4-解壓縮到服務層的功能

這項工作中將示範如何擷取到的服務層，讓您輕鬆將其服務的功能分開控制站的圖層，藉此讓人重複使用之服務的實際執行工作的開發人員的功能。

1. 在方案根目錄中建立新的資料夾並將它命名**Services**。 若要這樣做，請以滑鼠右鍵按一下**ContactManager**專案，然後選取**新增** | **新資料夾**，其命名*服務*。

    ![建立服務 資料夾](build-restful-apis-with-aspnet-web-api/_static/image14.png "建立服務資料夾")

    *建立服務 資料夾*
2. 以滑鼠右鍵按一下**Services**資料夾，然後選取**新增 |類別...** 從內容功能表。

    ![將新類別新增至 [服務] 資料夾](build-restful-apis-with-aspnet-web-api/_static/image15.png "將新類別新增至 [服務] 資料夾")

    *將新類別新增至 [服務] 資料夾*
3. 當**加入新項目**對話方塊隨即出現，新類別命名**ContactRepository**然後按一下**新增**。

    ![建立類別檔案，以包含連絡人儲存機制的服務層的程式碼](build-restful-apis-with-aspnet-web-api/_static/image16.png "建立類別檔案，以包含連絡人儲存機制的服務層的程式碼")

    *建立類別檔案，以包含連絡人儲存機制的服務層的程式碼*
4. 新增 using 指示詞**ContactRepository.cs**檔案，以包含模型命名空間。

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. 將下列反白顯示的程式碼，加入**ContactRepository.cs**檔案以實作 GetAllContacts 方法。

    (程式碼片段- *Web API 實驗室-Ex01-連絡人儲存機制*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. 開啟**ContactController.cs**檔案如果它尚未開啟。
7. 新增下列 using 陳述式之檔案的命名空間宣告一節。

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. 將下列反白顯示的程式碼，加入**ContactController.cs**新增私用欄位來代表儲存機制的執行個體，以便成員可以建立的類別的其餘部分使用的服務實作的類別。

    (程式碼片段-*的 Web API 實驗室-Ex01-連絡人控制器*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. 變更**取得**方法，讓它可讓使用連絡人儲存機制的服務。

    (程式碼片段-*透過儲存機制中傳回的連絡人清單 Web API 實驗室-Ex01-*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. 將中斷點放**ContactController**的**取得**方法定義。

   ![將中斷點新增至連絡人控制器](build-restful-apis-with-aspnet-web-api/_static/image17.png "將中斷點新增至連絡人控制器")

   *將中斷點新增至連絡人控制器*
11. 按 **F5** 執行應用程式。
12. 當瀏覽器會開啟時，請按**F12**以開啟 開發人員工具。
13. 按一下 [**網路**] 索引標籤。
14. 按一下 [**開始擷取**] 按鈕。
15. 附加後置詞的 [網址] 列中的 URL **/api/contact**然後按**Enter**載入 API 控制器。
16. Visual Studio 2012 應該一次中斷**取得**方法開始執行。

   ![在 Get 方法中的重大](build-restful-apis-with-aspnet-web-api/_static/image18.png "中斷在 Get 方法")

   *在 Get 方法中的重大*
17. 按 **F5** 繼續。
18. 回到 Internet Explorer 如果它尚未在成為焦點。 請注意網路的 [擷取] 視窗。

    ![網路在 Internet Explorer 中顯示的 Web API 呼叫結果的檢視](build-restful-apis-with-aspnet-web-api/_static/image19.png "網路在 Internet Explorer 中顯示的 Web API 呼叫結果的檢視")

    *在 Internet Explorer 中顯示的 Web API 呼叫結果的 [網路] 檢視*
19. 按一下 **前往 詳細檢視** 按鈕。
20. 按一下 [**回應主體**] 索引標籤。請注意 JSON 輸出的 API，以及它如何表示兩個服務層所擷取的連絡人。

    ![開發人員工具 視窗中檢視 Web API 的 JSON 輸出](build-restful-apis-with-aspnet-web-api/_static/image20.png "開發人員工具 視窗中檢視 Web API 的 JSON 輸出")

    *開發人員工具 視窗中檢視 Web API 的 JSON 輸出*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>練習 2： 建立讀取/寫入 Web API

在此練習中，您會將實作 POST 和 PUT 方法的連絡人管理員 」 來啟用資料編輯功能。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>工作 1-開啟 Web API 專案

在這個工作中，您會準備增強，因此可以接受使用者輸入，在練習 1 中建立的 Web API 專案。

1. 執行**Visual Studio 2012 Express for Web**，以前往**開始**然後輸入**VS Express for Web**然後按**Enter**。
2. 開啟**開始**解決方案位於**來源/Ex02-ReadWriteWebAPI/開始/** 資料夾。 否則，您可能會繼續使用**結束**方案取得完成前一個練習。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
3. 開啟**Services/ContactRepository.cs**檔案。

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>工作 2-加入連絡人儲存機制實作的資料持續性功能

在這個工作中，您將會擴大 ContactRepository 的類別，使其可以保存，並接受使用者輸入和新的連絡人執行個體，在練習 1 中建立的 Web API 專案。

1. 新增下列常數**ContactRepository**類別來代表 web 伺服器快取項目索引鍵名稱的稍後在本練習中的名稱。

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. 新增的建構函式**ContactRepository**包含下列程式碼。

    (程式碼片段- *Web API 實驗室-Ex02-連絡人儲存機制的建構函式*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. 修改程式碼**GetAllContacts**方法如下所示。

    (程式碼片段- *Web API 實驗室-Ex02-取得所有連絡人*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > 這個範例是供示範之用，並將 web 伺服器的快取做為儲存媒體，以便值將同時適用於多個用戶端，而不是使用工作階段儲存機制或要求儲存體的存留期。 可以使用 Entity Framework、 XML 儲存體或任何其他各種取代 web 伺服器快取。
4. 實作一個名為的新方法**SaveContact**要**ContactRepository**類別來儲存連絡人的工作。 **SaveContact**方法應該採用單一**連絡人**參數和傳回布林值，指出成功或失敗。

    (程式碼片段- *Web API 的實驗室-Ex02-實作 SaveContact 方法*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>練習 3： 使用從 HTML 用戶端的 Web API

在此練習中，您將建立 HTML 用戶端呼叫 Web API。 此用戶端會使用 JavaScript 的 Web api 方便資料交換，並將結果顯示在網頁瀏覽器使用 HTML 標記。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>工作 1-修改 [索引] 檢視，以提供 GUI 顯示連絡人

在這個工作中，您將修改以支援的需求在 HTML 瀏覽器中顯示現有的連絡人清單 web 應用程式的預設索引檢視。

1. 開啟**Visual Studio 2012 Express for Web**如果它尚未開啟。
2. 開啟**開始**解決方案位於**來源/Ex03-ConsumingWebAPI/開始/** 資料夾。 否則，您可能會繼續使用**結束**方案取得完成前一個練習。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
3. 開啟**Index.cshtml**位於檔案**Views/Home**資料夾。
4. Div 項目內的 HTML 程式碼取代識別碼**主體**使它看起來像下列程式碼。

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. 要執行 Web API 的 HTTP 要求之檔案的底部新增下列 Javascript 程式碼。

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. 開啟**ContactController.cs**檔案如果它尚未開啟。
7. 將中斷點放在**取得**方法**ContactController**類別。

    ![在 API 控制器的 Get 方法中放置中斷點](build-restful-apis-with-aspnet-web-api/_static/image21.png "將中斷點放上 API 控制器的 Get 方法")

    *將中斷點放上 API 控制器的 Get 方法*
8. 按下**F5**執行專案。 HTML 文件時，會載入瀏覽器。

    > [!NOTE]
    > 請確定您正在瀏覽至您的應用程式的根目錄 URL。
9. 一旦頁面載入和執行的 JavaScript，會叫用中斷點，並執行程式碼，將會暫停在控制器中。

    ![使用 VS Express for Web 的 Web API 呼叫偵錯](build-restful-apis-with-aspnet-web-api/_static/image22.png "偵錯使用 VS Express for Web 的 Web API 呼叫")

    *偵錯使用 Visual Studio 2012 Express for Web 的 Web API 呼叫*
10. 移除中斷點，然後按**F5**或偵錯工具列**繼續**按鈕以繼續載入瀏覽器中的檢視。 Web API 呼叫完成之後應該會看到呼叫瀏覽器中的 顯示為清單項目從 Web API 傳回的連絡人。

    ![在瀏覽器中顯示為清單項目 API 呼叫的結果](build-restful-apis-with-aspnet-web-api/_static/image23.png "瀏覽器中顯示為清單項目 API 呼叫的結果")

    *在瀏覽器中顯示為清單項目 API 呼叫的結果*
11. 停止偵錯。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>工作 2-修改 [索引] 檢視，以提供 GUI 建立連絡人

在這個工作中，您將繼續修改索引檢視的 MVC 應用程式。 表單會新增至 HTML 頁面，將會擷取使用者輸入，並將它傳送給 Web API 來建立新的連絡人，並將建立新的 Web API 控制器方法，以收集從 GUI 的日期。

1. 開啟**ContactController.cs**檔案。
2. 新增至名為控制器類別的新方法**Post**如下列程式碼所示。

    (程式碼片段- *Web API 實驗室-Ex03-Post 方法*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. 開啟**Index.cshtml**檔案在 Visual Studio 中，如果它尚未開啟。
4. 將下列 HTML 程式碼加入檔案之後您加入在先前的工作的未排序清單。

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. 在底部的 文件的指令碼項目，新增下列醒目提示的程式碼，來處理按鈕 click 事件，將資料張貼至 Web API 使用 HTTP POST 呼叫。

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. 在  **ContactController.cs**，將中斷點放上**Post**方法。
7. 按下**F5**瀏覽器中執行應用程式。
8. 一旦頁面載入瀏覽器中，輸入新的連絡人名稱和識別碼，然後按一下**儲存** 按鈕。

    ![用戶端 HTML 文件載入瀏覽器](build-restful-apis-with-aspnet-web-api/_static/image24.png "瀏覽器中載入用戶端 HTML 文件")

    *用戶端 HTML 文件載入瀏覽器*
9. 當偵錯工具視窗會在中斷**Post**方法，可看一下屬性**連絡**參數。 值應該符合您在表單中輸入的資料。

    ![從用戶端傳送至 Web API 的連絡人物件](build-restful-apis-with-aspnet-web-api/_static/image25.png "從用戶端傳送至 Web API 的連絡人物件")

    *從用戶端傳送至 Web API 的連絡人物件*
10. 逐步執行直到偵錯工具中的方法**回應**在建立變數。 中的檢查時**區域變數**偵錯工具 視窗中的，您會看到已設定的所有屬性。

   ![在 偵錯工具中建立的回應](build-restful-apis-with-aspnet-web-api/_static/image26.png "偵錯工具中建立的回應")

   *在偵錯工具中建立的回應*
11. 如果您按下**F5**或按**繼續**在偵錯工具將完成的要求。 所儲存的連絡人清單一旦您切換回瀏覽器，已加入新的連絡人**ContactRepository**實作。

   ![瀏覽器會反映成功建立新的連絡人執行個體](build-restful-apis-with-aspnet-web-api/_static/image27.png "瀏覽器會反映成功建立新的連絡人執行個體")

   *瀏覽器會反映成功建立新的連絡人執行個體*

> [!NOTE]
> 此外，您可以在其中部署此應用程式至 Azure 的下列[附錄 c： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixC)。


* * *

<a id="Summary"></a>
## <a name="summary"></a>總結

這個實驗室已引進新的 ASP.NET Web API 架構，並使用架構的 RESTful Web Api 的實作。 從這裡開始，您可以建立新的存放庫，可協助資料持續性使用任意數目的機制，並連接該服務而不是提供做為範例，在這個實驗室中其中一個。 Web API 支援一些其他功能，例如啟用支援 HTTP 和 JSON 或 XML 的任何語言撰寫的非 HTML 用戶端傳來的通訊。 裝載 Web API，典型的 web 應用程式之外的能力也是可行的以及已建立您自己的序列化格式的能力。

ASP.NET 網站有一個專門用來在 ASP.NET Web API 架構的區域[ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api)。 此站台將持續提供最新的資訊、 範例和新聞與 Web API，因此請經常如果您想要更深入地鑽研建立給幾乎任何裝置或開發架構，可用的自訂 Web Api 的圖案。

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>附錄 a： 使用程式碼片段

使用程式碼片段，您會有您需要在隨手可得的所有程式碼。 實驗室課程文件會告訴您完全時您可以使用它們，如下圖所示。

![若要將程式碼插入您的專案使用 Visual Studio 程式碼片段](build-restful-apis-with-aspnet-web-api/_static/image28.png "程式碼插入您的專案使用 Visual Studio 程式碼片段")

*若要將程式碼插入您的專案使用 Visual Studio 程式碼片段*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>若要新增的程式碼片段，使用鍵盤 （僅限 C#)

1. 將游標放在您想要插入的程式碼。
2. 開始輸入程式碼片段名稱 （不含空格或連字號）。
3. 觀看為 IntelliSense 會顯示相符的程式碼片段的名稱。
4. 選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。
5. 按下 Tab 鍵兩次的游標位置插入程式碼片段。

    ![開始鍵入程式碼片段名稱](build-restful-apis-with-aspnet-web-api/_static/image29.png "開始輸入程式碼片段名稱")

    *開始鍵入程式碼片段名稱*

    ![按 Tab 鍵以選取反白顯示的程式碼片段](build-restful-apis-with-aspnet-web-api/_static/image30.png "按 Tab 鍵以選取反白顯示的程式碼片段")

    *按 Tab 鍵以選取反白顯示的程式碼片段*

    ![再次按 Tab 鍵和程式碼片段會依序展開](build-restful-apis-with-aspnet-web-api/_static/image31.png "再次按 Tab 鍵和程式碼片段會展開")

    *再次按 Tab 鍵和程式碼片段會展開*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>若要新增的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）

1. 以滑鼠右鍵按一下您要插入程式碼片段。
2. 選取 **插入程式碼片段**後面**My Code Snippets**。
3. 選擇相關的程式片段，從清單中，對它按一下。

    ![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](build-restful-apis-with-aspnet-web-api/_static/image32.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")

    *以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*

    ![對它按一下挑選清單中，相關程式碼片段](build-restful-apis-with-aspnet-web-api/_static/image33.png "對它按一下挑選清單中，相關程式碼片段")

    *對它按一下挑選清單中，相關程式碼片段*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>附錄 b： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Azure SDK</em>&quot;。
2. 按一下 **立即安裝**。 如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。

    ![接受授權條款](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *接受授權條款*
5. 等候完成的下載與安裝程序。

    ![安裝進度](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *安裝進度*
6. 安裝完成時，按一下**完成**。

    ![安裝已完成](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *安裝已完成*
7. 按一下 **結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。

    ![VS Express for Web 圖格](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *VS Express for Web 圖格*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>附錄 c： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy

本附錄將說明如何從 Azure 入口網站中建立新的網站和發行您取得所遵循的實驗室中，利用 Web Deploy 發行所提供的功能由 Azure 應用程式。

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>工作 1： 從 Azure 入口網站中建立新的網站

1. 移至[Azure 管理入口網站](https://manage.windowsazure.com/)並使用您的訂用帳戶相關聯的 Microsoft 認證登入。

    > [!NOTE]
    > 使用 Azure，您可以免費託管 10 個 ASP.NET 網站，並再隨著流量成長而調整。 您可以註冊申請[此處](http://aka.ms/aspnet-hol-azure)。

    ![登入 Windows Azure 入口網站](build-restful-apis-with-aspnet-web-api/_static/image39.png "登入 Windows Azure 入口網站")

    *登入入口網站*
2. 按一下 **新增**命令列上。

    ![建立新的 Web 站台](build-restful-apis-with-aspnet-web-api/_static/image40.png "建立新的網站")

    *建立新的網站*
3. 按一下 **計算** | **網站**。 然後選取**快速建立**選項。 新的網站提供可用的 URL，然後按**建立網站**。

    > [!NOTE]
    > Azure 是您可以控制和管理雲端中執行的 web 應用程式的主應用程式。 [快速建立] 選項可讓您從 azure 入口網站外部的已完成的 web 應用程式部署。 它不包含設定資料庫的步驟。

    ![建立新的網站上，使用 快速建立](build-restful-apis-with-aspnet-web-api/_static/image41.png "建立新的網站上，使用 快速建立")

    *建立新的網站上，使用 快速建立*
4. 等到新**網站**建立。
5. 建立網站後按一下底下的連結**URL**資料行。 檢查新的網站運作。

    ![瀏覽至新的 web 站台](build-restful-apis-with-aspnet-web-api/_static/image42.png "瀏覽至新的網站")

    *瀏覽至新的網站*

    ![執行的 web 站台](build-restful-apis-with-aspnet-web-api/_static/image43.png "執行的網站")

    *執行的網站*
6. 返回入口網站並按一下底下的網站名稱**名稱**資料行來顯示 [管理] 頁面。

    ![開啟 網站管理頁面](build-restful-apis-with-aspnet-web-api/_static/image44.png "開啟的網站管理頁面")

    *開啟 網站管理頁面*
7. 在 **儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。

    > [!NOTE]
    > *發行設定檔*包含所有發行至 Azure，以針對每個已啟用的發行方法的 web 應用程式所需的資訊。 發行設定檔包含 Url、 使用者認證和連接到並對每個發行集的方法已啟用的端點進行驗證所需的資料庫字串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**並**Microsoft Visual Studio 2012**支援讀取發行設定檔，以自動化的這些程式的組態正在發行至 Azure web 應用程式。

    ![正在下載網站發行設定檔](build-restful-apis-with-aspnet-web-api/_static/image45.png "下載網站發行設定檔")

    *正在下載網站發行設定檔*
8. 下載發行設定檔至已知位置。 進一步在這個練習中，您會看到如何使用此檔案來發佈 Azure web 應用程式從 Visual Studio。

    ![儲存發行設定檔](build-restful-apis-with-aspnet-web-api/_static/image46.png "儲存發佈設定檔")

    *儲存發行設定檔*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>工作 2-設定資料庫伺服器

如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。 如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。

1. 您將需要 SQL Database 伺服器來儲存應用程式資料庫。 您可以從您的訂用帳戶，在 Azure 管理入口網站中檢視 SQL Database 伺服器**Sql Database** | **伺服器** | **伺服器的儀表板**. 如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。 請記下的**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，如同您會在下一個工作中使用它們。 並未建立資料庫，因為它會建立在稍後的階段。

    ![SQL Database 伺服器儀表板](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database 伺服器儀表板")

    *SQL Database 伺服器儀表板*
2. 下一個工作在您將測試資料庫連接，從 Visual Studio 中，因此您需要在伺服器的清單包含您的本機 IP 位址**允許的 IP 位址**。 若要這樣做，請按一下**設定**，選取的 IP 位址**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**並**結束 IP 位址**文字方塊中，然後按一下 [ ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) ] 按鈕。

    ![新增用戶端 IP 位址](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *新增用戶端 IP 位址*
3. 一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。

    ![確認變更](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *確認變更*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy

1. 返回至 ASP.NET MVC 4 的方案。 在 **方案總管**，以滑鼠右鍵按一下網站專案，然後選取**發佈**。

    ![發行應用程式](build-restful-apis-with-aspnet-web-api/_static/image51.png "發行應用程式")

    *發行網站*
2. 匯入發行設定檔儲存在第一項工作。

    ![匯入發行設定檔](build-restful-apis-with-aspnet-web-api/_static/image52.png "匯入發行設定檔")

    *匯入發行設定檔*
3. 按一下 **驗證連線**。 驗證完成後按一下**下一步**。

    > [!NOTE]
    > 一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。

    ![驗證連接](build-restful-apis-with-aspnet-web-api/_static/image53.png "驗證連線")

    *驗證連接*
4. 在 **設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。

    ![Web 部署組態](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web 部署設定")

    *Web 部署設定*
5. 設定資料庫連線，如下所示：

   - 在 **伺服器名稱**輸入您使用 SQL Database 伺服器的 URL *tcp:* 前置詞。
   - 在 **使用者名**輸入您的伺服器系統管理員身分登入名稱。
   - 在 **密碼**輸入您的伺服器系統管理員身分登入密碼。
   - 輸入新的資料庫名稱，例如： *MVC4SampleDB*。

     ![設定目的地連接字串](build-restful-apis-with-aspnet-web-api/_static/image55.png "設定目的地連接字串")

     *設定目的地連接字串*
6. 然後按一下 [確定]。  當系統提示您建立資料庫時，按一下**是**。

    ![建立資料庫](build-restful-apis-with-aspnet-web-api/_static/image56.png "建立的資料庫字串")

    *建立資料庫*
7. 您將用來連接至 Windows Azure 中的 SQL 資料庫的連接字串會顯示在文字方塊中的預設連線。 然後按 [下一步] 。

    ![連接字串指向 SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "連接字串指向 SQL Database")

    *連接字串指向 SQL Database*
8. 在  **Preview**頁面上，按一下**發佈**。

    ![Web 應用程式發行](build-restful-apis-with-aspnet-web-api/_static/image58.png "發行 web 應用程式")

    *發行 web 應用程式*
9. 當發行程序完成時，預設瀏覽器會開啟已發行的網站。

    ![應用程式發行至 Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "應用程式發行至 Windows Azure")

    *應用程式發佈至 Azure*
