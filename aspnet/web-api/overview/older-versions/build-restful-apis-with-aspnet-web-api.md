---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: 建置 RESTful 應用程式開發介面使用 ASP.NET Web API |Microsoft 文件
author: rick-anderson
description: 近年來，它已經變成清除不是 HTTP： 只能用來提供 HTML 網頁。 這也是功能強大的平台，來建置 Web Api，使用少數 o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: cb02288e93be801a1e55852741ed1443d8d3617d
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/18/2018
---
<a name="build-restful-apis-with-aspnet-web-api"></a>建置 RESTful 應用程式開發介面使用 ASP.NET Web API
====================
由[Web 營小組](https://twitter.com/webcamps)

> 近年來，它已經變成清除不是 HTTP： 只能用來提供 HTML 網頁。 它也是功能強大的平台，來建置 Web Api，使用少數幾個動詞命令 （GET、 POST 等） 再加上一些簡單的概念，例如*Uri*和*標頭*。 ASP.NET Web API 是一組簡化 HTTP 程式設計的元件。 因為它內建 ASP.NET MVC 執行階段，Web 應用程式開發介面會自動處理 HTTP 的傳輸低階詳細資料。 同時，Web API 自然會公開 HTTP 程式設計模型。 事實上，Web API 的其中一個目標是要*不*提取出的 HTTP 狀況。 如此一來，Web API 是彈性且易於擴充。 在這個實際操作實驗室中，您將建置簡單的 REST API 連絡人管理員應用程式使用 Web API。 您也將建置用戶端使用的 API。 Rest 架構已證明是利用 HTTP-有效率的方式，雖然不一定唯一有效的方法，為 HTTP。 連絡人的管理員會公開 RESTful 清單、 加入和移除連絡人和其他項目。 這個實驗室需要基本的了解 HTTP，其餘部分，而且假設您有基本的 HTML、 JavaScript 和 jQuery 的實用知識。
> 
> > [!NOTE]
> > ASP.NET 網站有一個專門用來在 ASP.NET Web API framework 的區域[ https://asp.net/web-api ](https://asp.net/web-api)。 此站台會繼續以提供最新的資訊、 範例和新聞與 Web 應用程式開發介面，因此請經常如果您想要更深入地建立自訂 Web 應用程式開發介面使用幾乎任何裝置或開發架構的封面鑽研。
> > 
> > ASP.NET Web API，類似於 ASP.NET MVC 4，已分離可讓您使用數個可用的相依性插入架構相當簡單的控制站的服務層的絕佳彈性。 沒有良好的範例，示範如何使用 ASP.NET Web API 專案中，您可以從下載的相依性插入 Ninject 的 MSDN 中[這裡](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)。
> 
> 
> 所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。


<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將學會如何：

- 實作 rest 式 Web 應用程式開發介面
- 從 HTML 用戶端呼叫 API

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

需要下列項目才能完成這個實際操作實驗室：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 B](#AppendixB)如需有關如何安裝指示)。

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

**安裝程式碼片段**

為了方便起見，大部分您將沿著這個實驗室管理的程式碼可做為 Visual Studio 程式碼片段。 若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。

如果您不熟悉 Visual Studio 程式碼片段，而且想来了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 a： 使用程式碼片段](#AppendixA)&quot;。

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室包含下列練習中：

1. [練習 1： 建立唯讀 Web API](#Exercise1)
2. [練習 2： 建立讀取/寫入 Web API](#Exercise2)
3. [練習 3： 使用 Web API，從 HTML 用戶端](#Exercise3)

> [!NOTE]
> 每個練習會伴隨著**結束**包含所產生的解決方案完成練習之後，您應該取得資料夾。 如果您需要其他協助使用的所有練習，您可以使用這個方案做為指南。


完成本實驗室估計時間： **60 分鐘**。

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>練習 1： 建立唯讀 Web API

在此練習中，您將連絡人的 manager 實作唯讀 GET 方法。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>工作 1-建立 API 專案

在這項工作，您將使用新的 ASP.NET web 專案範本建立 Web API 的 web 應用程式。

1. 執行**Visual Studio 2012 Express for Web**，請移至要**啟動**和型別**VS Express for Web**然後按下**Enter**。
2. 從**檔案**功能表上，選取**新專案**。 選取**Visual C# |Web**專案類型，從 專案類型樹狀檢視中，然後選取  **ASP.NET MVC 4 Web 應用程式**專案類型。 將專案的**名稱**至*ContactManager*和**方案名稱**至*開始*，然後按一下 **確定**.

    ![建立新的 ASP.NET MVC 4.0 Web 應用程式專案](build-restful-apis-with-aspnet-web-api/_static/image1.png "建立新的 ASP.NET MVC 4.0 Web 應用程式專案")

    *建立新的 ASP.NET MVC 4.0 Web 應用程式專案*
3. 在 ASP.NET MVC 4 專案的 [類型] 對話方塊中，選取**Web API**專案類型。 按一下 [確定 **Deploying Office Solutions**]。

    ![指定 Web API 專案類型](build-restful-apis-with-aspnet-web-api/_static/image2.png "指定 Web API 專案類型")

    *指定 Web API 專案類型*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>工作 2-建立連絡人管理員應用程式開發介面控制站

在這項工作，您將建立應用程式開發介面方法所在的控制器類別。

1. 刪除名為檔案**ValuesController.cs**內**控制器**從專案的資料夾。
2. 以滑鼠右鍵按一下**控制器**中專案，然後選取資料夾**新增 |控制器**從內容功能表。

    ![專案中加入新的控制站](build-restful-apis-with-aspnet-web-api/_static/image3.png "專案中加入新的控制站")

    *將新的控制站新增至專案*
3. 在**加入控制器**出現對話方塊，選取**空白的 API 控制器**從 [範本] 功能表。 將控制器類別**ContactController**。 然後，按一下 **新增。**

    ![使用 [加入控制器] 對話方塊建立新的 Web API 控制器](build-restful-apis-with-aspnet-web-api/_static/image4.png "使用加入控制器 對話方塊來建立新的 Web API 控制器")

    *使用 [加入控制器] 對話方塊建立新的 Web API 控制器*
4. 將下列程式碼加入**ContactController**。

    (程式碼片段- *Web API 實驗室-Ex01-取得應用程式開發介面方法*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. 按**F5**偵錯應用程式。 預設的首頁 Web API 專案應該會出現。

    ![ASP.NET Web API 應用程式的預設首頁](build-restful-apis-with-aspnet-web-api/_static/image5.png "ASP.NET Web API 應用程式的預設首頁")

    *ASP.NET Web API 應用程式的預設首頁*
6. 在 Internet Explorer 視窗中，按**F12**鍵開啟**開發人員工具**視窗。 按一下**網路**索引標籤，然後再按一下**開始擷取** 按鈕，開始到視窗擷取網路流量。

    ![開啟 [網路] 索引標籤，然後啟動網路擷取](build-restful-apis-with-aspnet-web-api/_static/image6.png "開啟 [網路] 索引標籤，然後啟動網路擷取")

    *開啟 [網路] 索引標籤，然後啟動網路擷取*
7. 附加與瀏覽器的網址列中的 URL **/api/連絡人**然後按 enter 鍵。 傳輸的詳細資料會出現在網路的 [擷取] 視窗中。 請注意，回應的 MIME 類型為**應用程式 /json**。 這將示範如何將預設輸出格式為 JSON。

    ![在 [網路] 檢視中檢視 Web API 要求的輸出](build-restful-apis-with-aspnet-web-api/_static/image7.png "[網路] 檢視中檢視的 Web API 要求的輸出")

    *在 [網路] 檢視中檢視 Web API 要求的輸出*

    > [!NOTE]
    > 此時 Internet Explorer 10 的預設行為將會詢問使用者是否想要儲存或開啟 Web API 呼叫所產生的資料流。 輸出會包含 Web API URL 呼叫的 JSON 結果的文字檔。 不要取消對話方塊才能觀看回應的內容，透過開發人員工具視窗。
8. 按一下**移至 詳細檢視**按鈕可查看有關此應用程式開發介面呼叫的回應詳細資料。

    ![切換至 詳細檢視](build-restful-apis-with-aspnet-web-api/_static/image8.png "切換至 詳細資料檢視")

    *切換至 詳細檢視*
9. 按一下**回應主體**若要檢視實際的 JSON 回應文字 索引標籤。

    ![網路監視器中的文字檢視 JSON 輸出](build-restful-apis-with-aspnet-web-api/_static/image9.png "網路監視器中的文字檢視 JSON 輸出")

    *網路監視器中檢視 JSON 輸出文字*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>工作 3-建立連絡人的模型，將它擴大連絡控制器

在這項工作，您將建立應用程式開發介面方法所在的控制器類別。

1. 以滑鼠右鍵按一下**模型**資料夾，然後選取**新增 |類別...** 從內容功能表。

    ![將新模型加入至 web 應用程式](build-restful-apis-with-aspnet-web-api/_static/image10.png "將新模型加入至 web 應用程式")

    *將新模型加入至 web 應用程式*
2. 在**加入新項目**對話方塊中，將新的檔案命名**Contact.cs**按一下**新增。**

    ![建立新的連絡人類別檔案](build-restful-apis-with-aspnet-web-api/_static/image11.png "建立新的連絡人類別檔案")

    *建立新的連絡人類別檔案*
3. 將下列反白顯示的程式碼加入**連絡人**類別。

    (程式碼片段- *Web API 實驗室-Ex01-連絡人類別*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. 在**ContactController**類別中，選取 word**字串**方法定義中**取得**方法，然後輸入這個字*連絡人*。 一旦這個字型別中，指標就會出現這個字開頭**連絡人**。 請按住**Ctrl**索引鍵並按下句號 （.） 索引鍵或按一下圖示以開啟 [協助] 對話方塊，在程式碼編輯器中，以自動填入使用滑鼠**使用**模型指示詞命名空間。

    ![命名空間宣告中使用 Intellisense 協助](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *命名空間宣告中使用 Intellisense 協助*
5. 修改的程式碼**取得**方法，使它傳回連絡人模型執行個體的陣列。

    (程式碼片段-*傳回的連絡人清單 Web API 實驗室-Ex01-*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. 按**F5**偵錯 web 應用程式，在瀏覽器中的。 若要檢視應用程式開發介面的回應輸出所做的變更，請執行下列步驟。

   1. 瀏覽器會開啟之後, 按**F12**如果開發人員工具還未開啟。
   2. 按一下**網路** 索引標籤。
   3. 按**開始擷取** 按鈕。
   4. 新增 URL 尾碼 **/api/連絡人**在位址列中按的 url **Enter**索引鍵。
   5. 按**移至 [詳細檢視**] 按鈕。
   6. 選取**回應主體** 索引標籤。您應該會看到代表序列化的形式的連絡人執行個體陣列的 JSON 字串。

      ![JSON 序列化的複雜的 Web API 方法呼叫輸出](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON 序列化複雜的 Web API 方法呼叫的輸出")

      *複雜的 Web API 方法呼叫的序列化的 JSON 輸出*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>工作 4-解壓縮至服務層的功能

此工作將會示範如何擷取到服務層，以方便開發人員在其服務功能分開控制器圖層，藉此讓人重複使用之服務的實際執行工作的功能。

1. 在方案根目錄中建立新的資料夾並將其命名**服務**。 若要這樣做，請以滑鼠右鍵按一下**ContactManager**專案，然後選取**新增** | **新資料夾**，其命名*服務*。

    ![建立服務 資料夾](build-restful-apis-with-aspnet-web-api/_static/image14.png "建立服務資料夾")

    *建立服務 資料夾*
2. 以滑鼠右鍵按一下**服務**資料夾，然後選取**新增 |類別...** 從內容功能表。

    ![將新類別加入至 [服務] 資料夾](build-restful-apis-with-aspnet-web-api/_static/image15.png "將新類別加入至 [服務] 資料夾")

    *將新類別加入至 [服務] 資料夾*
3. 當**加入新項目**對話方塊隨即出現，將新類別**ContactRepository**按一下**新增**。

    ![建立包含連絡人儲存機制的服務層的程式碼的類別檔案](build-restful-apis-with-aspnet-web-api/_static/image16.png "建立要包含連絡人儲存機制的服務層的程式碼的類別檔案")

    *建立包含連絡人儲存機制的服務層的程式碼的類別檔案*
4. 加入 using 指示詞加入**ContactRepository.cs**檔案以包含模型命名空間。

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. 將下列反白顯示的程式碼加入**ContactRepository.cs**檔案，以實作 GetAllContacts 方法。

    (程式碼片段- *Web API 實驗室-Ex01-連絡人儲存機制*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. 開啟**ContactController.cs**如果還沒有開啟檔案。
7. 加入下列 using 陳述式之檔案的命名空間宣告區段。

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. 將下列反白顯示的程式碼加入**ContactController.cs**新增私用欄位來代表執行個體的儲存機制，讓成員可以建立的類別的其餘部分使用的服務實作的類別。

    (程式碼片段- *Web API 實驗室-Ex01-連絡人控制器*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. 變更**取得**方法，使它可讓使用連絡人儲存機制的服務。

    (程式碼片段-*透過儲存機制傳回的連絡人清單 Web API 實驗室-Ex01-*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. 將中斷點放**ContactController**的**取得**方法定義。

   ![將中斷點加入至連絡人控制器](build-restful-apis-with-aspnet-web-api/_static/image17.png "連絡人的控制站加入中斷點")

   *連絡人的控制站加入中斷點*
11. 按 **F5** 執行應用程式。
12. 當瀏覽器會開啟時，請按**F12**以開啟 開發人員工具。
13. 按一下**網路** 索引標籤。
14. 按一下**開始擷取** 按鈕。
15. 附加後置詞，在網址列中的 URL **/api/連絡人**按**Enter**載入 API 控制器。
16. Visual Studio 2012 應該中斷一次**取得**方法就會開始執行。

   ![取得方法內中斷](build-restful-apis-with-aspnet-web-api/_static/image18.png "Get 方法內中斷")

   *取得方法內中斷*
17. 按 **F5** 繼續。
18. 回到 Internet Explorer 如果還沒有焦點。 請注意網路擷取視窗。

    ![網路在 Internet Explorer 中顯示 Web 應用程式開發介面呼叫的結果檢視](build-restful-apis-with-aspnet-web-api/_static/image19.png "網路在 Internet Explorer 中顯示 Web 應用程式開發介面呼叫的結果檢視")

    *在 Internet Explorer 顯示 Web 應用程式開發介面呼叫的結果中的網路檢視*
19. 按一下**移至 [詳細檢視**] 按鈕。
20. 按一下**回應主體** 索引標籤。請注意 JSON 輸出的 API，以及它如何表示兩個服務層所擷取的連絡人。

    ![在 [開發人員工具] 視窗中檢視 Web API 的 JSON 輸出](build-restful-apis-with-aspnet-web-api/_static/image20.png "開發人員工具 視窗中檢視 Web API 的 JSON 輸出")

    *在 [開發人員工具] 視窗中檢視 Web API 的 JSON 輸出*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>練習 2： 建立讀取/寫入 Web API

在此練習中，您將實作 POST 和 PUT 方法連絡管理員以啟用資料編輯功能。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>工作 1-開啟 Web API 專案

在這項工作，您將準備增強，因此可接受使用者輸入，練習 1 中建立的 Web API 專案。

1. 執行**Visual Studio 2012 Express for Web**，請移至要**啟動**和型別**VS Express for Web**然後按下**Enter**。
2. 開啟**開始**方案位於**來源/Ex02ReadWriteWebAPI/開始/** 資料夾。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

   1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
3. 開啟**Services/ContactRepository.cs**檔案。

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>工作 2-加入連絡人儲存機制實作的資料持續性功能

在這項工作，您將會增加 ContactRepository 類別，因此可以保存並接受使用者輸入和新的連絡人執行個體，練習 1 中建立的 Web API 專案。

1. 新增下列常數**ContactRepository**類別來代表 web 伺服器快取項目索引鍵名稱稍後在這個練習中的名稱。

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. 新增的建構函式**ContactRepository**包含下列程式碼。

    (程式碼片段- *Web API 實驗室-Ex02-連絡人儲存機制的建構函式*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. 修改的程式碼**GetAllContacts**依照以下所示的方法。

    (程式碼片段- *Web API 實驗室-Ex02-取得所有連絡人*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > 這個範例是為了示範之用，並會為儲存體中使用 web 伺服器的快取，以便值將同時適用於多個用戶端，而不使用工作階段儲存機制或要求儲存體存留期。 其中一個可以使用 Entity Framework、 XML 儲存體或任何其他各種取代 web 伺服器快取。
4. 實作新方法，名為**SaveContact**至**ContactRepository**類別來儲存連絡人的工作。 **SaveContact**方法應該採用單一**連絡人**參數和傳回布林值表示成功或失敗。

    (程式碼片段- *Web API 實驗室-Ex02-實作 SaveContact 方法*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>練習 3： 使用 Web API，從 HTML 用戶端

在此練習中，您將建立 HTML 用戶端呼叫 Web API。 此用戶端會協助使用 JavaScript 的 Web api 的資料交換，並會使用 HTML 標記的 web 瀏覽器中顯示的結果。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>工作 1-修改索引檢視，以提供 GUI 顯示連絡人

在這個工作中，您將修改成可支援在 HTML 瀏覽器中顯示現有的連絡人清單需求的 web 應用程式的預設索引檢視。

1. 開啟**Visual Studio 2012 Express for Web**如果它尚未開啟。
2. 開啟**開始**方案位於**來源/Ex03ConsumingWebAPI/開始/** 資料夾。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

   1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
3. 開啟**Index.cshtml**檔案位於**Views/Home**資料夾。
4. Div 項目內的 HTML 程式碼取代為 id**主體**，讓它看起來像下列程式碼。

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. 加入下列 Javascript 程式碼檔案來執行 Web API 的 HTTP 要求的底部。

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. 開啟**ContactController.cs**如果還沒有開啟檔案。
7. 將中斷點放在**取得**方法**ContactController**類別。

    ![Get 方法的 API 控制器上放置中斷點](build-restful-apis-with-aspnet-web-api/_static/image21.png "將中斷點放置在 Get 方法的 API 控制器")

    *將中斷點放置在 Get 方法的 API 控制器*
8. 按**F5**執行專案。 瀏覽器將會載入 HTML 文件。

    > [!NOTE]
    > 請確定您在瀏覽至您的應用程式的根目錄 URL。
9. 一旦載入頁面，而且 JavaScript 執行，會叫用中斷點，並執行程式碼將會暫停控制器中。

    ![使用 VS Express for Web 的 Web API 呼叫偵錯](build-restful-apis-with-aspnet-web-api/_static/image22.png "偵錯使用 VS Express for Web 的 Web API 呼叫")

    *使用 Visual Studio 2012 Express for Web 的 Web 應用程式開發介面呼叫偵錯*
10. 移除中斷點，並按**F5**或偵錯 工具列的**繼續**按鈕以繼續載入瀏覽器中的檢視。 Web 應用程式開發介面呼叫完成之後應該會看到呼叫為清單項目顯示在瀏覽器從 Web API 傳回的連絡人。

    ![在瀏覽器中顯示為清單項目 API 呼叫的結果](build-restful-apis-with-aspnet-web-api/_static/image23.png "瀏覽器中顯示為清單項目 API 呼叫的結果")

    *在瀏覽器中顯示為清單項目 API 呼叫的結果*
11. 停止偵錯。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>工作 2-修改索引檢視，以提供用於建立連絡人的 GUI

在這項工作，您將會繼續修改索引檢視的 MVC 應用程式。 表單將會加入至 HTML 頁面，會擷取使用者輸入，並將其傳送至 Web API 來建立新的連絡人，並將建立新的 Web API 控制器方法，以收集從 GUI 的日期。

1. 開啟**ContactController.cs**檔案。
2. 將新方法加入至名為的控制器類別**Post**如下列程式碼所示。

    (程式碼片段- *Web API 實驗室-Ex03-Post 方法*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. 開啟**Index.cshtml**檔案在 Visual Studio 中，如果它尚未開啟。
4. 將下列 HTML 程式碼加入檔案之後您加入先前工作中的未排序清單。

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. 在底部的 文件的指令碼項目，加入下列反白顯示的程式碼，以處理按鈕 click 事件，將會張貼資料到 Web API 使用 HTTP POST 呼叫。

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. 在**ContactController.cs**，將中斷點放在**Post**方法。
7. 按**F5**瀏覽器中執行應用程式。
8. 一旦載入網頁瀏覽器中，輸入新的連絡人名稱和識別碼和按一下**儲存** 按鈕。

    ![用戶端 HTML 文件載入瀏覽器](build-restful-apis-with-aspnet-web-api/_static/image24.png "用戶端 HTML 文件載入瀏覽器")

    *用戶端 HTML 文件載入瀏覽器*
9. 偵錯工具視窗中的分行符號當**Post**方法的內容查看**連絡**參數。 值應該符合您在表單中輸入的資料。

    ![從用戶端傳送至 Web API 的連絡人物件](build-restful-apis-with-aspnet-web-api/_static/image25.png "連絡人物件從用戶端傳送至 Web API")

    *從用戶端傳送至 Web API 的連絡人物件*
10. 逐步執行直到偵錯工具中的方法**回應**已經建立變數。 中的檢查時**區域變數**偵錯工具視窗中的，您會看到已設定的所有屬性。

   ![下列偵錯工具中的建立的回應](build-restful-apis-with-aspnet-web-api/_static/image26.png "遵循偵錯工具中的建立的回應")

   *下列偵錯工具中的建立的回應*
11. 如果您按下**F5**或按一下**繼續**偵錯工具要求將會完成。 一旦您切換回瀏覽器，新的連絡人具有所儲存的連絡人清單中加入**ContactRepository**實作。

   ![瀏覽器會反映在成功建立新連絡人的執行個體](build-restful-apis-with-aspnet-web-api/_static/image27.png "瀏覽器會反映在成功建立新連絡人的執行個體")

   *瀏覽器會反映在成功建立新連絡人的執行個體*

> [!NOTE]
> 此外，您可以部署此應用程式以 Azure 下列[附錄 c： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixC)。


* * *

<a id="Summary"></a>
## <a name="summary"></a>總結

這個實驗室已引進新的 ASP.NET Web API framework，並使用 framework RESTful Web Api 的實作。 從這裡開始，可以建立新的儲存機制，可促進使用任意數目的機制的資料持續性，而不是提供做為範例在這個實驗室中的簡單一個連接該服務。 Web 應用程式開發介面支援許多其他功能，例如啟用非 HTML 用戶端以支援 HTTP 和 JSON 或 XML 的任何語言撰寫的通訊。 裝載 Web API 一般 web 應用程式之外的能力也是可行的以及已建立您自己的序列化格式的能力。

ASP.NET 網站有一個專門用來在 ASP.NET Web API framework 的區域[ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api)。 此站台會繼續以提供最新的資訊、 範例和新聞與 Web 應用程式開發介面，因此請經常如果您想要更深入地建立自訂 Web 應用程式開發介面使用幾乎任何裝置或開發架構的封面鑽研。

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>附錄 a： 使用程式碼片段

程式碼片段，您會有您需要在您可以的所有程式碼。 實驗室文件會告訴您完全時您可以使用它們，如下圖所示。

![若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段](build-restful-apis-with-aspnet-web-api/_static/image28.png "使用 Visual Studio 程式碼片段，將程式碼插入您的專案")

*若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>若要加入的程式碼片段，使用鍵盤 （僅限 C#)

1. 將游標放在您想要插入的程式碼。
2. 開始鍵入片段名稱 （不含空格或連字號）。
3. 觀察 IntelliSense 會顯示比對程式碼片段的名稱。
4. 選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。
5. 按下 Tab 鍵兩次的游標位置插入程式碼片段。

    ![開始輸入程式碼片段名稱](build-restful-apis-with-aspnet-web-api/_static/image29.png "開始鍵入片段名稱")

    *開始輸入程式碼片段名稱*

    ![按 Tab 鍵選取反白顯示的程式碼片段](build-restful-apis-with-aspnet-web-api/_static/image30.png "按 Tab 鍵以選取反白顯示的程式碼片段")

    *按 Tab 鍵選取反白顯示的程式碼片段*

    ![按 Tab 鍵一次程式碼片段就會擴展](build-restful-apis-with-aspnet-web-api/_static/image31.png "按 Tab 鍵一次程式碼片段就會擴展")

    *按 Tab 鍵一次程式碼片段就會擴展*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>若要加入的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）

1. 以滑鼠右鍵按一下您要插入程式碼片段。
2. 選取**插入程式碼片段**後面**我的程式碼片段**。
3. 透過在按一下挑選清單中，從相關的程式碼片段。

    ![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](build-restful-apis-with-aspnet-web-api/_static/image32.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")

    *以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*

    ![透過在按一下挑選清單中，從相關的程式碼片段](build-restful-apis-with-aspnet-web-api/_static/image33.png "上它即可挑選清單中，從相關的程式碼片段")

    *透過在按一下挑選清單中，從相關的程式碼片段*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>附錄 b： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 與 Azure SDK</em>&quot;。
2. 按一下**立即安裝**。 如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。

    ![接受授權條款](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *接受授權條款*
5. 等待直到完成下載和安裝程序。

    ![安裝進度](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *安裝進度*
6. 當安裝完成時，按一下**完成**。

    ![安裝已完成](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *安裝已完成*
7. 按一下**結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。

    ![VS Express for Web 方塊](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *VS Express for Web 方塊*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>附錄 c： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy

此附錄將說明如何從 Azure 入口網站建立新的網站和發行您取得依照實驗室中，利用 Web Deploy 發行功能的 Azure 所提供的應用程式。

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>工作 1-從 Azure 入口網站建立新的網站

1. 移至[Azure 管理入口網站](https://manage.windowsazure.com/)並使用與您訂用帳戶相關聯的 Microsoft 認證登入。

    > [!NOTE]
    > 使用 Azure 中，您可以免費裝載 10 個 ASP.NET 網站並再擴充隨流量成長。 您可以註冊[這裡](http://aka.ms/aspnet-hol-azure)。

    ![登入 Windows Azure 入口網站](build-restful-apis-with-aspnet-web-api/_static/image39.png "登入 Windows Azure 入口網站")

    *登入入口網站*
2. 按一下**新增**命令列上。

    ![建立新的網站](build-restful-apis-with-aspnet-web-api/_static/image40.png "建立新的網站")

    *建立新的網站*
3. 按一下**計算** | **網站**。 然後選取**快速建立**選項。 新的網站上提供可用的 URL，然後按一下**建立網站**。

    > [!NOTE]
    > Azure 是您可以控制和管理在雲端中執行 web 應用程式的主機。 快速建立 選項可讓您部署已完成的 web 應用程式，從 azure 入口網站外部。 它不包含設定資料庫的步驟。

    ![建立新的網站使用 快速建立](build-restful-apis-with-aspnet-web-api/_static/image41.png "建立新的網站使用 快速建立")

    *建立新的網站使用 快速建立*
4. 等候新**網站**建立。
5. 建立網站之後按一下底下的連結**URL**資料行。 請檢查新的網站運作。

    ![瀏覽至新的網站](build-restful-apis-with-aspnet-web-api/_static/image42.png "瀏覽至新的網站")

    *瀏覽至新的網站*

    ![執行網站](build-restful-apis-with-aspnet-web-api/_static/image43.png "執行的網站")

    *執行的網站*
6. 返回入口網站並按一下底下的網站名稱**名稱**資料行會顯示 [管理] 頁面。

    ![開啟網站管理頁面](build-restful-apis-with-aspnet-web-api/_static/image44.png "開啟網站管理頁面")

    *開啟網站管理頁面*
7. 在**儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。

    > [!NOTE]
    > *發行設定檔*包含所有 web 應用程式發行至 Azure 的每個已啟用的發行方法所需的資訊。 發行設定檔包含 Url、 使用者認證和資料庫連接，並對每個端點已啟用的發行集的方法進行驗證所需的字串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支援讀取發行設定檔自動將這些程式的組態web 應用程式發行至 Azure。

    ![下載網站發行設定檔](build-restful-apis-with-aspnet-web-api/_static/image45.png "下載網站發行設定檔")

    *下載網站發行設定檔*
8. 下載發行設定檔至已知位置。 進一步在這個練習中，您會看到如何使用此檔案發行至 Azure 從 Visual Studio web 應用程式。

    ![儲存發行設定檔](build-restful-apis-with-aspnet-web-api/_static/image46.png "儲存發行設定檔")

    *儲存發行設定檔*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>工作 2-設定資料庫伺服器

如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。 如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。

1. 您將需要 SQL Database 伺服器來儲存應用程式資料庫。 您可以從您的訂用帳戶，在 Azure 管理入口網站中檢視 SQL Database 伺服器**Sql 資料庫** | **伺服器** | **伺服器的儀表板**. 如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。 請注意**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，因為您將在下一個工作中使用它們。 並未建立資料庫，因為它會在後續階段中建立。

    ![SQL Database 伺服器儀表板](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database 伺服器儀表板")

    *SQL Database 伺服器儀表板*
2. 下一項工作在您將測試資料庫連接，從 Visual Studio，因此您需要在伺服器的清單中包含您的本機 IP 位址**允許的 IP 位址**。 若要這樣做，請按一下**設定**，選取 [IP 位址從**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**和**結束 IP 位址**文字方塊，然後按一下![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) ] 按鈕。

    ![加入用戶端 IP 位址](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *加入用戶端 IP 位址*
3. 一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。

    ![確認變更](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *確認變更*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy

1. 返回至 ASP.NET MVC 4 方案。 在**方案總管 中**，以滑鼠右鍵按一下網站專案，然後選取**發行**。

    ![發行應用程式](build-restful-apis-with-aspnet-web-api/_static/image51.png "發行應用程式")

    *發行網站*
2. 匯入發行設定檔儲存在第一項工作中。

    ![匯入發行設定檔](build-restful-apis-with-aspnet-web-api/_static/image52.png "匯入發行設定檔")

    *匯入發行設定檔*
3. 按一下**驗證連線**。 驗證完成後按一下**下一步**。

    > [!NOTE]
    > 一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。

    ![驗證連線](build-restful-apis-with-aspnet-web-api/_static/image53.png "驗證連線")

    *驗證連接*
4. 在**設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。

    ![Web 部署設定](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web 部署設定")

    *Web 部署設定*
5. 設定資料庫連接，如下所示：

   - 在**伺服器名稱**您 SQL Database 伺服器 URL 使用下列方法類型*tcp:* 前置詞。
   - 在**使用者名**輸入您的伺服器系統管理員身分登入名稱。
   - 在**密碼**輸入伺服器系統管理員身分登入密碼。
   - 輸入新的資料庫名稱，例如： *MVC4SampleDB*。

     ![設定目的地連接字串](build-restful-apis-with-aspnet-web-api/_static/image55.png "設定目的地連接字串")

     *設定目的地連接字串*
6. 然後按一下 [確定]。  當系統提示您建立資料庫依序按一下**是**。

    ![建立資料庫](build-restful-apis-with-aspnet-web-api/_static/image56.png "建立的資料庫字串")

    *建立資料庫*
7. 您將用來連接到在 Windows Azure SQL Database 的連接字串會顯示在預設連接文字方塊中。 然後按 [下一步] 。

    ![連接字串指向 SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "連接字串指向 SQL 資料庫")

    *連接字串指向 SQL 資料庫*
8. 在**預覽**頁面上，按一下**發行**。

    ![Web 應用程式發行](build-restful-apis-with-aspnet-web-api/_static/image58.png "發行 web 應用程式")

    *發行 web 應用程式*
9. 一旦在發佈程序完成時，預設瀏覽器會開啟已發行的網站。

    ![應用程式發行至 Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "應用程式發行至 Windows Azure")

    *應用程式發行至 Azure*
