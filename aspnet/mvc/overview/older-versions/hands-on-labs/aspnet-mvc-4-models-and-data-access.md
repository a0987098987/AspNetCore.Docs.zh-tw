---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 模型和資料存取 |Microsoft Docs
author: rick-anderson
description: 注意： 這個實際操作實驗室中假設您有 ASP.NET MVC 的基本知識。 如果您未曾使用之前的 ASP.NET MVC，我們建議您將會透過 ASP.NET MVC 4...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: afc03d87431632bbb3ab59241de0edb4bb7af12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371125"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>ASP.NET MVC 4 模型和資料存取

藉由[Web Camp 小組](https://twitter.com/webcamps)

[下載 Web 研討會訓練套件](https://aka.ms/webcamps-training-kit)

這個實際操作實驗室會假設您有的基本知識**ASP.NET MVC**。 如果您未曾使用**ASP.NET MVC**之前，我們建議您將逐一**ASP.NET MVC 4 基本概念**實際操作實驗室。

這個實驗室會引導您完成先前所述將微幅的變更套用到來源資料夾中提供的範例 Web 應用程式的新功能與增強功能。

> [!NOTE]
> 所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 這個實驗室中的特定專案將會位於[ASP.NET MVC 4 模型和資料存取](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess)。

在  **ASP.NET MVC 基礎**實際操作實驗室中，您有已硬式編碼的資料將從傳遞控制站至檢視範本。 但是，若要建置實際的 Web 應用程式，您可能想要使用實際的資料庫。

這個實際操作實驗室會示範如何使用資料庫引擎來儲存和擷取音樂市集應用程式所需的資料。 若要達到此目的，您會啟動與現有的資料庫，並從它建立實體資料模型。 在這個實驗室中，您將會符合**Database First**方法，以及**Code First**方法。

不過，您也可以使用**Model First**方法，建立相同的模型使用的工具，然後從它產生資料庫。

![第一個與資料庫。Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs。第一次建立模型")

*第一個與資料庫。第一次建立模型*

之後產生模型，以提供從資料庫中，而不是使用硬式編碼資料的資料存放區檢視 StoreController 中進行適當調整。 此外，您將不需要進行任何變更的檢視範本 StoreController 將相同的 Viewmodel 傳回檢視範本，因為雖然此時資料會來自資料庫。

**Code First 方法**

Code First 方法可讓我們不會產生類別，通常會結合 framework 定義的程式碼中的模型。

在程式碼第一，模型物件會定義 Poco，&quot;純舊 CLR 物件&quot;。 Poco 是簡單的一般類別，具有沒有繼承，而且不會實作介面。 我們可以自動產生的資料庫，或者我們可以使用現有的資料庫，並從程式碼產生的類別對應。

使用這種方法的優點是，模型會維持不變 （在此情況下，Entity Framework），持續性架構無關的 Poco 類別不會搭配對應的架構。

> [!NOTE]
> 這個實驗室根據 ASP.NET MVC 4 和自訂的音樂市集 」 範例應用程式的版本，和最小化成只顯示這個實際操作實驗室的功能。
> 
> 如果您想要瀏覽整個**音樂市集**您可以找到它的應用程式教學課程[MVC Music 市集](https://github.com/evilDave/MVC-Music-Store)。


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

如果您不熟悉 Visual Studio 程式碼片段，而且想要了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 c： 使用程式碼片段](#AppendixC)&quot;。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室是由下列練習所組成的：

1. [練習 1： 加入資料庫](#Exercise1)
2. [練習 2： 建立資料庫，使用 Code First](#Exercise2)
3. [練習 3： 查詢具有參數的資料庫](#Exercise3)

> [!NOTE]
> 每個練習會伴隨**結束**包含完成練習之後，您應該取得所產生的方案資料夾。 如果您需要的所有練習所使用的其他說明，您可以使用此解決方案作為指南。


完成這個實驗室估計時間： **35 分鐘內**。

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>練習 1： 加入資料庫

在此練習中，您將學習如何新增含有 MusicStore 應用程式方案，以便取用其資料的資料表的資料庫。 一旦資料庫產生模型，然後加入至方案，您將修改 StoreController 類別，以提供從資料庫中，而不是使用硬式編碼值的資料檢視範本。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>工作 1-新增資料庫

在這個工作中，您將加入已建立的資料庫解決方案 MusicStore 應用程式的主資料表。

1. 開啟**開始**解決方案位於**來源/Ex1-AddingADatabaseDBFirst/開始/** 資料夾。

   1. 您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 新增**MvcMusicStore**資料庫檔案。 在這個實際操作實驗室中，您將使用已建立的資料庫呼叫**MvcMusicStore.mdf**。 若要這樣做，請以滑鼠右鍵按一下**應用程式\_資料**資料夾，指向**新增**，然後按一下**現有項目**。 瀏覽至**\Source\Assets** ，然後選取**MvcMusicStore.mdf**檔案。

    ![加入現有項目](aspnet-mvc-4-models-and-data-access/_static/image2.png "加入現有項目")

    *加入現有項目*

    ![MvcMusicStore.mdf 資料庫檔案](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf 資料庫檔案")

    *MvcMusicStore.mdf 資料庫檔案*

    資料庫已新增至專案。 即使資料庫位於置於方案中，您可以查詢並更新它，因為它裝載在不同的資料庫伺服器。

    ![在 方案總管 MvcMusicStore 資料庫](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore 方案總管 中的資料庫")

    *在 [方案總管] 中的 MvcMusicStore 資料庫*
3. 請確認資料庫的連接。 若要這樣做，請按兩下**MvcMusicStore.mdf**建立連線。

    ![連接到 MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "連接到 MvcMusicStore.mdf")

    *連接到 MvcMusicStore.mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>工作 2-建立資料模型

在這個工作中，您將建立資料模型與先前的工作中新增的資料庫互動。

1. 建立資料模型，以代表資料庫。 若要這樣做，請在 方案總管 中以滑鼠右鍵按一下**模型**資料夾，指向**新增**，然後按一下 **新項目**。 在 **加入新項目**對話方塊中，選取**Data**範本，然後**ADO.NET 實體資料模型**項目。 資料模型將名稱變更為**StoreDB.edmx**然後按一下**新增**。

    ![新增 StoreDB ADO.NET 實體資料模型](aspnet-mvc-4-models-and-data-access/_static/image6.png "新增 StoreDB ADO.NET 實體資料模型")

    *新增 StoreDB ADO.NET 實體資料模型*
2. **Entity Data Model 精靈**會出現。 此精靈會引導您完成建立模型層。 由於模型應該根據現有的資料庫 recentyl 新增建立，選取**從資料庫產生**然後按一下**下一步**。

    ![選擇模型內容](aspnet-mvc-4-models-and-data-access/_static/image7.png "選擇模型內容")

    *選擇模型內容*
3. 因為您從資料庫產生模型，您必須指定要使用的連接。 按一下 **新的連接**。
4. 選取  **Microsoft SQL Server 資料庫檔案**然後按一下**繼續**。

    ![選擇資料來源](aspnet-mvc-4-models-and-data-access/_static/image8.png "選擇資料來源")

    *[選擇資料來源] 對話方塊*
5. 按一下 **瀏覽**，然後選取資料庫**MvcMusicStore.mdf**您位於**應用程式\_資料**資料夾，然後按一下**確定**。

    ![連接屬性](aspnet-mvc-4-models-and-data-access/_static/image9.png "連接屬性")

    *連接屬性*
6. 產生的類別應該具有相同名稱的實體連接字串，因此它將名稱變更為**MusicStoreEntities**然後按一下**下一步**。

    ![選擇資料連接](aspnet-mvc-4-models-and-data-access/_static/image10.png "選擇資料連接")

    *選擇資料連接*
7. 選擇要使用的資料庫物件。 因為實體模型將使用剛才資料庫的資料表，選取**資料表**選項，並確定**模型中包含外部索引鍵資料行**和**化或單數化產生物件名稱**選項也會一併選取。 若要將模型命名空間變更**MvcMusicStore.Model**然後按一下**完成**。

    ![選擇的資料庫物件](aspnet-mvc-4-models-and-data-access/_static/image11.png "選擇的資料庫物件")

    *選擇的資料庫物件*

    > [!NOTE]
    > 如果會顯示安全性警告對話方塊，按一下**確定**執行範本，並產生該模型實體的類別。
8. 將會建立個別的類別對應至資料庫的每個資料表實體的圖表資料庫將會出現。 例如，**專輯**資料表會由**專輯**類別，其中每個資料行的資料表中會對應至類別內容。 這可讓您查詢及處理物件，代表在資料庫中的資料列。

    ![實體圖表](aspnet-mvc-4-models-and-data-access/_static/image12.png "實體圖表")

    *實體圖表*

    > [!NOTE]
    > T4 範本 (.tt) 執行程式碼來產生實體類別，並具有相同名稱將會覆寫現有的類別。 在此範例中，類別&quot;專輯&quot;， &quot;Genre&quot;並&quot;演出者&quot;已覆寫產生的程式碼。


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>工作 3-建置應用程式

在這個工作中，您將會檢查，雖然模型產生已移除**專輯**， **Genre**並**演出者**模型類別專案成功建置使用新的資料模型類別。

1. 建置專案，方法是選取**建置**功能表項目，然後**建置 MvcMusicStore**。

    ![建置專案](aspnet-mvc-4-models-and-data-access/_static/image13.png "建置專案")

    *建置專案*
2. 專案建置成功。 為什麼它仍在使用？ 這是因為資料庫資料表有包含您所使用的屬性已移除的類別中的欄位**專輯**並**內容類型**。

    ![成功的組建](aspnet-mvc-4-models-and-data-access/_static/image14.png "成功的組建")

    *組建成功*
3. 雖然設計工具會以圖表格式顯示實體，但它們其實是 C# 類別。 依序展開**StoreDB.edmx**方案總管 中的節點，然後**StoreDB.tt**，您會看到新產生的實體。

    ![產生的檔案](aspnet-mvc-4-models-and-data-access/_static/image15.png "產生的檔案")

    *產生的檔案*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>工作 4-查詢資料庫

在這個工作中，您將會更新 StoreController 類別，以便不使用硬式編碼資料，它會查詢資料庫以擷取資訊。

1. 開啟**Controllers\StoreController.cs**並將下列欄位新增至類別，以保存的執行個體**MusicStoreEntities**類別，名為**storeDB**:

    (程式碼片段-*模型和資料存取-Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. **MusicStoreEntities**類別會公開資料庫中的每個資料表的集合屬性。 更新**瀏覽**動作方法，來擷取所有的內容類型**專輯**。

    (程式碼片段-*模型和資料存取-Ex1 存放區瀏覽*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > 您使用的功能，稱為.NET **LINQ** (language integrated query) 撰寫強型別的查詢運算式，針對這些集合-將會執行對資料庫的程式碼，並傳回物件，您可以設計程式針對。
    > 
    > 如需有關 LINQ 的詳細資訊，請瀏覽[msdn 網站](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx)。
3. 更新**Index**動作方法，來擷取所有的內容類型。

    (程式碼片段-*模型和資料存取-Ex1 存放區索引*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. 更新**Index**動作方法，來擷取所有的內容類型，並轉換為清單集合。

    (程式碼片段-*模型和資料存取-Ex1 市集 GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>工作 5-執行應用程式

在這個工作中，您將會檢查存放區索引頁面現在會顯示儲存在資料庫中，而不是硬式編碼的內容類型。 若要變更檢視範本，因為不需要**StoreController**傳回相同的實體和以前一樣，雖然這次的資料會來自資料庫。

1. 重建方案，然後按**F5**執行應用程式。
2. 專案一開始在首頁上。 確認的功能表**內容類型**不再是硬式編碼清單，並直接從資料庫擷取資料。

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *瀏覽的內容類型，從資料庫*
3. 現在瀏覽至任何內容類型，並確認從資料庫填入相簿。

    ![從資料庫中瀏覽專輯](aspnet-mvc-4-models-and-data-access/_static/image17.png "瀏覽的專輯，從資料庫")

    *從資料庫中瀏覽相簿*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>練習 2： 建立資料庫，先使用程式碼

在此練習中，您將了解如何使用 Code First 方法來建立資料庫資料表 MusicStore 應用程式，以及如何存取其資料。

一旦產生模型時，您將修改 StoreController 取自資料庫，而不是使用硬式編碼值的資料提供檢視範本。

> [!NOTE]
> 如果您已完成練習 1，而且已經使用資料庫第一種方法，您現在將會了解如何取得相同的結果，以不同方式處理。 為了方便您讀取已標示和練習 1 相同的工作。 如果您有未完成練習 1，但想要了解 Code First 方法，您可以從這個練習中啟動，並取得主題的完整的涵蓋範圍。


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>工作 1-填入範例資料

在這個工作中，您將資料庫中填入範例資料本節一開始建立使用 Code First 時。

1. 開啟**開始**解決方案位於**來源/Ex2-CreatingADatabaseCodeFirst/開始/** 資料夾。 否則，您可能會繼續使用**結束**方案取得完成前一個練習。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 新增**SampleData.cs**的檔案**模型**資料夾。 若要這樣做，請以滑鼠右鍵按一下**模型**資料夾，指向**新增**，然後按一下 **現有項目**。 瀏覽至**\Source\Assets** ，然後選取**SampleData.cs**檔案。

    ![範例資料填入程式碼](aspnet-mvc-4-models-and-data-access/_static/image18.png "範例資料填入程式碼")

    *範例資料填入程式碼*
3. 開啟**Global.asax.cs**檔案，並新增下列*使用*陳述式。

    (程式碼片段-*模型和資料存取-Ex2 全域 Asax Using*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. 在 **應用程式\_start （)** 方法中加入下列這一行加入設定的資料庫初始設定式。

    (程式碼片段-*模型和資料存取-Ex2 全域 Asax SetInitializer*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>工作 2-設定資料庫的連接

既然您已新增資料庫至我們的專案，您將撰寫**Web.config**檔案的連接字串。

1. 新增在連接字串**Web.config**。若要這樣做，請開啟**Web.config**專案根目錄，取代連接字串中的這一行以命名 DefaultConnection **&lt;connectionStrings&gt;** 區段：

    ![Web.config 檔案位置](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config 檔案位置")

    *Web.config 檔案位置*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>工作 3-使用模型

既然您已經設定資料庫的連接，您將連結的模型與資料庫資料表。 在這個工作中，您將建立的類別，將會連結到使用 Code First 資料庫。 請記住，有現存的 POCO 模型類別，應該加以修改。

> [!NOTE]
> 如果您已完成練習 1 時，您會發現此步驟已執行精靈。 透過這種程式碼第一次，您將手動建立將會連結至資料實體的類別。

1. 開啟 POCO 模型類別**Genre**從**模型**專案資料夾，並包含執行識別碼。 使用 int 屬性同名**GenreId**。

    (程式碼片段-*模型和資料存取-Ex2 程式碼的第一個內容類型*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > 若要使用 Code First 慣例，內容類型的類別必須將會自動偵測到主索引鍵屬性。
    > 
    > 您可以深入了解程式碼中的第一個慣例[msdn 文章](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx)。
2. 現在，開啟 POCO 模型類別**專輯**從**模型**專案資料夾和包含外部索引鍵中，建立具有名稱的屬性**GenreId**並**ArtistId**。 這個類別已經**GenreId**主索引鍵。

    (程式碼片段-*模型和資料存取-Ex2 程式碼的第一張專輯*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. 開啟 POCO 模型類別**演出者**，並包含**ArtistId**屬性。

    (程式碼片段-*模型和資料存取-Ex2 程式碼的第一位演出者*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. 以滑鼠右鍵按一下**模型**專案資料夾，然後選取**新增 |類別**。 將檔案命名**MusicStoreEntities.cs**。 然後，按一下 **新增。**

    ![加入類別](aspnet-mvc-4-models-and-data-access/_static/image20.png "加入類別")

    *加入新項目*

    ![新增 class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "新增 class2")

    *加入類別*
5. 開啟您剛才建立的類別**MusicStoreEntities.cs**，並包含命名空間**System.Data.Entity**並**System.Data.Entity.Infrastructure**。

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. 取代類別宣告，來擴充**DbContext**類別： 宣告公用**DBSet**並覆寫**OnModelCreating**方法。 在此步驟之後，您會將連結您的模型使用 Entity Framework 的網域類別。 若要這樣做，請將類別的程式碼取代為下列：

    (程式碼片段-*模型和資料存取-Ex2 程式碼的第一個 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> 使用 Entity Framework **DbContext**並**DBSet**您將能夠查詢 POCO 類別內容類型。 藉由擴充**OnModelCreating**方法，就中指定**程式碼**內容類型對應至資料庫資料表的方式。 您可以在這篇 msdn 文章中找到 DBContext 和 DBSet 的詳細資訊：[連結](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>工作 4-查詢資料庫

在這個工作中，您將會更新 StoreController 類別，以便不使用硬式編碼資料，它將它從資料庫擷取。

> [!NOTE]
> 這項工作是與啟動器相同之練習 1。
> 
> 如果您已完成練習 1 您會發現這些步驟也在這兩種方法 (第一次資料庫或 Code first)。 它們是不同的模型中，會連結的資料，但存取資料的實體是透明的控制站。


1. 開啟**Controllers\StoreController.cs**並將下列欄位新增至類別，以保存的執行個體**MusicStoreEntities**類別，名為**storeDB**:

    (程式碼片段-*模型和資料存取-Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. **MusicStoreEntities**類別會公開資料庫中的每個資料表的集合屬性。 更新**瀏覽**動作方法，來擷取所有的內容類型**專輯**。

    (程式碼片段-*模型和資料存取-Ex2 存放區瀏覽*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > 您使用的功能，稱為.NET **LINQ** (language integrated query) 撰寫強型別的查詢運算式，針對這些集合-將會執行對資料庫的程式碼，並傳回物件，您可以設計程式針對。
    > 
    > 如需有關 LINQ 的詳細資訊，請瀏覽[msdn 網站](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx)。
3. 更新**Index**動作方法，來擷取所有的內容類型。

    (程式碼片段-*模型和資料存取-Ex2 存放區索引*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. 更新**Index**動作方法，來擷取所有的內容類型，並轉換為清單集合。

    (程式碼片段-*模型和資料存取-Ex2 市集 GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>工作 5-執行應用程式

在這個工作中，您將會檢查存放區索引頁面現在會顯示儲存在資料庫中，而不是硬式編碼的內容類型。 若要變更檢視範本，因為不需要**StoreController**會傳回相同**StoreIndexViewModel**如往常一般，但這次的資料會來自資料庫。

1. 重建方案，然後按**F5**執行應用程式。
2. 專案一開始在首頁上。 確認的功能表**內容類型**不再是硬式編碼清單，並直接從資料庫擷取資料。

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *瀏覽的內容類型，從資料庫*
3. 現在瀏覽至任何內容類型，並確認從資料庫填入相簿。

    ![從資料庫中瀏覽專輯](aspnet-mvc-4-models-and-data-access/_static/image23.png "瀏覽的專輯，從資料庫")

    *從資料庫中瀏覽相簿*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>練習 3： 查詢具有參數的資料庫

在這個練習中，您將學習如何查詢資料庫中使用參數，以及如何使用查詢結果成形，功能會減少數字的資料庫存取擷取資料以更有效率的方式。

> [!NOTE]
> 如需查詢結果成形的詳細資訊，請造訪下列[msdn 文章](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx)。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>工作 1-修改 StoreController 從資料庫擷取相簿

在這個工作中，您會變更**StoreController**類別來存取資料庫，以擷取特定的內容類型中的專輯。

1. 開啟**開始**解決方案位於**Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin**資料夾，如果您想要使用程式碼優先方法或**Source\Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin**資料夾，如果您想要使用資料庫優先方法。 否則，您可能會繼續使用**結束**方案取得完成前一個練習。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 開啟**StoreController**類別，以變更**瀏覽**動作方法。 若要這樣做，請在**方案總管**，展開**控制站**資料夾，然後按兩下**StoreController.cs**。
3. 變更**瀏覽**動作方法，來擷取特定的內容類型的相簿。 若要這樣做，請取代下列程式碼：

    (程式碼片段-*模型和資料存取-Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> 若要填入實體的集合，您必須使用**Include**方法，以指定您想要太擷取的專輯。 您可以使用。**Single （)** LINQ 中的延伸模組因為在此情況下為 album 預期只有一個內容類型。 **Single （)** 方法會採用 Lambda 運算式當做參數，而在此情況下，其名稱符合定義的值，指定單一的內容類型物件。
> 
> 您會利用可讓您指出您想要在擷取的內容類型的物件時所一併載入其他相關的實體的功能。 這項功能稱為**查詢結果成形**，並可讓您減少存取資料庫，以擷取資訊所需的次數。 在此案例中，您會想要預先擷取的專輯，如您所擷取的內容類型。
> 
> 此查詢包含**Genres.Include (&quot;專輯&quot;)** 以表示您想要相關的相簿。 這會導致更有效率的應用程式中，因為它會擷取在單一資料庫的要求中的內容類型和專輯資料。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>工作 2-執行應用程式

在這個工作中，您將執行應用程式，並從資料庫擷取的特定內容類型的相簿。

1. 按下**F5**執行應用程式。
2. 專案一開始在首頁上。 將 URL 變更為 **/Store/瀏覽？ 內容類型 = Pop**驗證，正在從資料庫擷取的結果。

    ![依內容類型瀏覽](aspnet-mvc-4-models-and-data-access/_static/image24.png "依內容類型瀏覽")

    *瀏覽/Store/瀏覽？ 內容類型 = Pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>工作 3-Id 所存取的相簿

在這個工作中，您將會重複先前的程序，以取得由其 id 的相簿

1. 關閉瀏覽器，如有需要若要返回 Visual Studio。 開啟**StoreController**類別，以變更**詳細資料**動作方法。 若要這樣做，請在**方案總管**，展開**控制站**資料夾，然後按兩下**StoreController.cs**。
2. 變更**詳細資料**擷取專輯詳細資料的動作方法，根據其**識別碼**。若要這樣做，請取代下列程式碼：

    (程式碼片段-*模型和資料存取-Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>工作 4-執行應用程式

在這個工作中，您會在網頁瀏覽器中執行應用程式，並取得專輯詳細資料，其識別碼。

1. 按下**F5**執行應用程式。
2. 專案一開始在首頁上。 將 URL 變更為 **/Store/Details/51**或瀏覽的內容類型，並選取驗證，正在從資料庫擷取的結果 album。

    ![瀏覽詳細資料](aspnet-mvc-4-models-and-data-access/_static/image25.png "瀏覽詳細資料")

    *瀏覽 /Store/Details/51*

> [!NOTE]
> 此外，您可以在其中部署此應用程式以 Windows Azure 網站的下列[附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixB)。

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>總結

藉由完成這個實際操作實驗室，您已了解 ASP.NET MVC 模型和資料存取的基本概念、 使用**Database First**方法，以及**Code First**方法：

- 如何將資料庫加入至方案中，若要取用其資料
- 如何更新控制站，以提供取自資料庫而不是硬式編碼的資料檢視範本
- 如何查詢使用參數的資料庫
- 如何使用查詢結果形成的功能會減少資料庫存取數目，以更有效率的方式擷取資料
- 如何使用 Database First 和程式碼優先方法中，Microsoft Entity Framework 的模型資料庫連結

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附錄 a： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Windows Azure SDK</em>&quot;。
2. 按一下 **立即安裝**。 如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。

    ![接受授權條款](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *接受授權條款*
5. 等候完成的下載與安裝程序。

    ![安裝進度](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *安裝進度*
6. 安裝完成時，按一下**完成**。

    ![安裝已完成](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *安裝已完成*
7. 按一下 **結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。

    ![VS Express for Web 圖格](aspnet-mvc-4-models-and-data-access/_static/image30.png)

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

    ![登入 Windows Azure 入口網站](aspnet-mvc-4-models-and-data-access/_static/image31.png "登入 Windows Azure 入口網站")

    *登入 Windows Azure 管理入口網站*
2. 按一下 **新增**命令列上。

    ![建立新的 Web 站台](aspnet-mvc-4-models-and-data-access/_static/image32.png "建立新的網站")

    *建立新的網站*
3. 按一下 **計算** | **網站**。 然後選取**快速建立**選項。 新的網站提供可用的 URL，然後按**建立網站**。

    > [!NOTE]
    > Windows Azure 網站時，您可以控制和管理雲端中執行的 web 應用程式的主應用程式。 [快速建立] 選項可讓您部署已完成的 web 應用程式至 Windows Azure 網站從入口網站外部。 它不包含設定資料庫的步驟。

    ![建立新的網站上，使用 快速建立](aspnet-mvc-4-models-and-data-access/_static/image33.png "建立新的網站上，使用 快速建立")

    *建立新的網站上，使用 快速建立*
4. 等到新**網站**建立。
5. 建立網站後按一下底下的連結**URL**資料行。 檢查新的網站運作。

    ![瀏覽至新的 web 站台](aspnet-mvc-4-models-and-data-access/_static/image34.png "瀏覽至新的網站")

    *瀏覽至新的網站*

    ![執行的 web 站台](aspnet-mvc-4-models-and-data-access/_static/image35.png "執行的網站")

    *執行的網站*
6. 返回入口網站並按一下底下的網站名稱**名稱**資料行來顯示 [管理] 頁面。

    ![開啟 網站管理頁面](aspnet-mvc-4-models-and-data-access/_static/image36.png "開啟的網站管理頁面")

    *開啟 網站管理頁面*
7. 在 **儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。

    > [!NOTE]
    > *發行設定檔*包含所有發行至 Windows Azure 網站的每個已啟用的發行方法的 web 應用程式所需的資訊。 發行設定檔包含 Url、 使用者認證和連接到並對每個發行集的方法已啟用的端點進行驗證所需的資料庫字串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**並**Microsoft Visual Studio 2012**支援讀取發行設定檔，以自動化的這些程式的組態正在發行至 Windows Azure 網站的 web 應用程式。

    ![正在下載網站發行設定檔](aspnet-mvc-4-models-and-data-access/_static/image37.png "下載網站發行設定檔")

    *正在下載網站發行設定檔*
8. 下載發行設定檔至已知位置。 進一步在這個練習中，您會看到如何從 Visual Studio 的 web 應用程式至 Windows Azure 網站發行使用此檔案。

    ![儲存發行設定檔](aspnet-mvc-4-models-and-data-access/_static/image38.png "儲存發佈設定檔")

    *儲存發行設定檔*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>工作 2-設定資料庫伺服器

如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。 如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。

1. 您將需要 SQL Database 伺服器來儲存應用程式資料庫。 您可以從您的訂用帳戶，在 Windows Azure Management 入口網站中檢視 SQL Database 伺服器**Sql Database** | **伺服器** | **伺服器儀表板**。 如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。 請記下的**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，如同您會在下一個工作中使用它們。 並未建立資料庫，因為它會建立在稍後的階段。

    ![SQL Database 伺服器儀表板](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database 伺服器儀表板")

    *SQL Database 伺服器儀表板*
2. 下一個工作在您將測試資料庫連接，從 Visual Studio 中，因此您需要在伺服器的清單包含您的本機 IP 位址**允許的 IP 位址**。 若要這樣做，請按一下**設定**，選取的 IP 位址**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**並**結束 IP 位址**文字方塊中，然後按一下 [ ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) ] 按鈕。

    ![新增用戶端 IP 位址](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *新增用戶端 IP 位址*
3. 一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。

    ![確認變更](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *確認變更*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy

1. 返回至 ASP.NET MVC 4 的方案。 在 **方案總管**，以滑鼠右鍵按一下網站專案，然後選取**發佈**。

    ![發行應用程式](aspnet-mvc-4-models-and-data-access/_static/image43.png "發行應用程式")

    *發行網站*
2. 匯入發行設定檔儲存在第一項工作。

    ![匯入發行設定檔](aspnet-mvc-4-models-and-data-access/_static/image44.png "匯入發行設定檔")

    *匯入發行設定檔*
3. 按一下 **驗證連線**。 驗證完成後按一下**下一步**。

    > [!NOTE]
    > 一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。

    ![驗證連接](aspnet-mvc-4-models-and-data-access/_static/image45.png "驗證連線")

    *驗證連接*
4. 在 **設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。

    ![Web 部署組態](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web 部署設定")

    *Web 部署設定*
5. 設定資料庫連線，如下所示：

   - 在 **伺服器名稱**輸入您使用 SQL Database 伺服器的 URL *tcp:* 前置詞。
   - 在 **使用者名**輸入您的伺服器系統管理員身分登入名稱。
   - 在 **密碼**輸入您的伺服器系統管理員身分登入密碼。
   - 輸入新的資料庫名稱。

     ![設定目的地連接字串](aspnet-mvc-4-models-and-data-access/_static/image47.png "設定目的地連接字串")

     *設定目的地連接字串*
6. 然後按一下 [確定]。  當系統提示您建立資料庫時，按一下**是**。

    ![建立資料庫](aspnet-mvc-4-models-and-data-access/_static/image48.png "建立的資料庫字串")

    *建立資料庫*
7. 您將用來連接至 Windows Azure 中的 SQL 資料庫的連接字串會顯示在文字方塊中的預設連線。 然後按 [下一步] 。

    ![連接字串指向 SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "連接字串指向 SQL Database")

    *連接字串指向 SQL Database*
8. 在  **Preview**頁面上，按一下**發佈**。

    ![Web 應用程式發行](aspnet-mvc-4-models-and-data-access/_static/image50.png "發行 web 應用程式")

    *發行 web 應用程式*
9. 當發行程序完成時，預設瀏覽器會開啟已發行的網站。

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>附錄 c： 使用程式碼片段

使用程式碼片段，您會有您需要在隨手可得的所有程式碼。 實驗室課程文件會告訴您完全時您可以使用它們，如下圖所示。

![若要將程式碼插入您的專案使用 Visual Studio 程式碼片段](aspnet-mvc-4-models-and-data-access/_static/image51.png "程式碼插入您的專案使用 Visual Studio 程式碼片段")

*若要將程式碼插入您的專案使用 Visual Studio 程式碼片段*

***若要新增的程式碼片段，使用鍵盤 （僅限 C#)***

1. 將游標放在您想要插入的程式碼。
2. 開始輸入程式碼片段名稱 （不含空格或連字號）。
3. 觀看為 IntelliSense 會顯示相符的程式碼片段的名稱。
4. 選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。
5. 按下 Tab 鍵兩次的游標位置插入程式碼片段。

![開始鍵入程式碼片段名稱](aspnet-mvc-4-models-and-data-access/_static/image52.png "開始輸入程式碼片段名稱")

*開始鍵入程式碼片段名稱*

![按 Tab 鍵以選取反白顯示的程式碼片段](aspnet-mvc-4-models-and-data-access/_static/image53.png "按 Tab 鍵以選取反白顯示的程式碼片段")

*按 Tab 鍵以選取反白顯示的程式碼片段*

![再次按 Tab 鍵和程式碼片段會依序展開](aspnet-mvc-4-models-and-data-access/_static/image54.png "再次按 Tab 鍵和程式碼片段會展開")

*再次按 Tab 鍵和程式碼片段會展開*

***若要新增的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。 以滑鼠右鍵按一下您要插入程式碼片段。

1. 選取 **插入程式碼片段**後面**My Code Snippets**。
2. 選擇相關的程式片段，從清單中，對它按一下。

![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-models-and-data-access/_static/image55.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")

*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*

![對它按一下挑選清單中，相關程式碼片段](aspnet-mvc-4-models-and-data-access/_static/image56.png "對它按一下挑選清單中，相關程式碼片段")

*對它按一下挑選清單中，相關程式碼片段*
