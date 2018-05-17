---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 模型和資料存取 |Microsoft 文件
author: rick-anderson
description: 注意： 這個實際操作實驗室假設您擁有的 ASP.NET MVC 的基本知識。 如果您沒有使用 ASP.NET MVC 之前，我們建議您前往透過 ASP.NET MVC 4...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 57477cf15bf6755523f28356d5384517bea24982
ms.sourcegitcommit: 5ae0c125ee3bbd324edef3818d1d160f4dd84602
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/17/2018
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>ASP.NET MVC 4 模型和資料存取

由[Web 營小組](https://twitter.com/webcamps)

[下載 Web 營訓練套件](https://aka.ms/webcamps-training-kit)

這個實際操作實驗室假設您擁有的基本知識**ASP.NET MVC**。 如果您不使用**ASP.NET MVC**之前，我們建議您在一段**ASP.NET MVC 4 的基本概念**實際操作實驗室。

這個實驗室將引導您完成先前所述將微幅變更套用至來源資料夾中提供的範例 Web 應用程式的新功能與增強功能。

> [!NOTE]
> 所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 這個實驗室中的特定專案將會位於[ASP.NET MVC 4 模型和資料存取](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess)。

在**ASP.NET MVC 基礎**實際操作實驗室中，您有已硬式編碼的資料將從傳遞控制站至檢視範本。 但是，才能建立實際的 Web 應用程式，您可能想要使用實際的資料庫。

這個實際操作實驗室會示範如何使用 database engine 以儲存及擷取 Music Store 應用程式所需的資料。 若要完成，您將會啟動現有的資料庫，並從它建立實體資料模型。 在本實驗室中，您將會符合**Database First**方法以及**Code First**方法。

不過，您也可以使用**Model First**方法，建立相同的模型使用的工具，然後從它產生資料庫。

![第一個與資料庫。建立第一個模型](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs。第一次建立模型")

*第一個與資料庫。第一次建立模型*

之後產生模型，以提供從資料庫中，而不是使用硬式編碼資料的資料存放區檢視 StoreController 中進行適當的調整。 您必須不作任何修改檢視範本 StoreController 會返回相同 ViewModels 檢視範本，因為雖然這次的資料將來自資料庫。

**程式碼的第一種方法**

程式碼第一種方法可讓我們從程式碼模型定義而不會產生通常搭配架構的類別。

在程式碼第一次，模型物件會定義與 POCOs，&quot;純舊 CLR 物件&quot;。 POCOs 是簡單的一般類別，不具有任何繼承，而且不會實作介面。 我們可以自動產生的資料庫，或者我們可以使用現有的資料庫，並從程式碼產生的類別對應。

使用這種方法的優點是，模型會維持獨立於持續性架構 （在此情況下，Entity Framework），如 POCOs 類別不會搭配對應架構。

> [!NOTE]
> 這個實驗室以 ASP.NET MVC 4 為基礎，自訂 Music Store 範例應用程式的版本，且最小化成只顯示在這個實際操作實驗室中的功能。
> 
> 如果您想要瀏覽整個**Music Store**教學課程應用程式，您可以找到在[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)。


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

您必須具備下列項目才能完成本實驗室：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 A](#AppendixA)如需有關如何安裝指示)。

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

**安裝程式碼片段**

為了方便起見，大部分您將沿著這個實驗室管理的程式碼可做為 Visual Studio 程式碼片段。 若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。

如果您不熟悉 Visual Studio 程式碼片段，而且想来了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 c： 使用程式碼片段](#AppendixC)&quot;。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室是由下列練習所組成：

1. [練習 1： 加入資料庫](#Exercise1)
2. [練習 2： 建立資料庫，使用 Code First](#Exercise2)
3. [練習 3： 查詢參數的資料庫](#Exercise3)

> [!NOTE]
> 每個練習會伴隨著**結束**包含所產生的解決方案完成練習之後，您應該取得資料夾。 如果您需要其他協助使用的所有練習，您可以使用這個方案做為指南。


完成本實驗室估計時間： **35 分鐘內**。

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>練習 1： 加入資料庫

在此練習中，您將了解如何將含有 MusicStore 應用程式方案，才能使用其資料的資料表的資料庫。 一旦資料庫產生模型，並加入至方案，您將修改 StoreController 類別取自資料庫，而不要使用硬式編碼值的資料提供檢視範本。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>工作 1-加入資料庫

在這項工作，您會加入方案的 MusicStore 應用程式的主資料表與已建立的資料庫。

1. 開啟**開始**方案位於**來源/Ex1AddingADatabaseDBFirst/開始/** 資料夾。

   1. 您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 新增**MvcMusicStore**資料庫檔案。 在這個實際操作實驗室中，您將使用已建立的資料庫稱為**MvcMusicStore.mdf**。 若要這樣做，請以滑鼠右鍵按一下**應用程式\_資料**資料夾，指向**新增**，然後按一下 **現有項目**。 瀏覽至**\Source\Assets**選取**MvcMusicStore.mdf**檔案。

    ![加入現有項目](aspnet-mvc-4-models-and-data-access/_static/image2.png "加入現有項目")

    *加入現有項目*

    ![MvcMusicStore.mdf 資料庫檔案](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf 資料庫檔案")

    *MvcMusicStore.mdf 資料庫檔案*

    資料庫已加入專案。 即使資料庫位於 「 解決方案中，您可以查詢並更新它，因為它已裝載於不同的資料庫伺服器。

    ![在 [方案總管 MvcMusicStore 資料庫](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore 方案總管] 中的資料庫")

    *在 方案總管 MvcMusicStore 資料庫*
3. 請確認資料庫的連接。 若要這樣做，請按兩下**MvcMusicStore.mdf**建立連線。

    ![連接到 MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "連接到 MvcMusicStore.mdf")

    *連接到 MvcMusicStore.mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>工作 2-建立資料模型

在這項工作，您將建立資料模型互動的先前工作中加入的資料庫。

1. 建立表示資料庫的資料模型。 若要這樣做，請在 方案總管 中以滑鼠右鍵按一下**模型**資料夾，指向**新增**，然後按一下 **新項目**。 在**加入新項目**對話方塊中，選取**資料**範本，然後**ADO.NET 實體資料模型**項目。 資料模型將名稱變更為**StoreDB.edmx**按一下**新增**。

    ![加入 StoreDB ADO.NET 實體資料模型](aspnet-mvc-4-models-and-data-access/_static/image6.png "加入 StoreDB ADO.NET 實體資料模型")

    *加入 StoreDB ADO.NET 實體資料模型*
2. **實體資料模型精靈**會出現。 此精靈將引導您建立的模型層。 由於模型應該依據現有的資料庫 recentyl 加入建立，選取**從資料庫產生**按一下**下一步**。

    ![選擇模型內容](aspnet-mvc-4-models-and-data-access/_static/image7.png "選擇模型內容")

    *選擇模型內容*
3. 因為您正在從資料庫產生模型，您必須指定要使用的連接。 按一下**新連線**。
4. 選取**Microsoft SQL Server 資料庫檔案**按一下**繼續**。

    ![選擇資料來源](aspnet-mvc-4-models-and-data-access/_static/image8.png "選擇資料來源")

    *選擇資料來源 對話方塊*
5. 按一下**瀏覽**選取的資料庫**MvcMusicStore.mdf**您位於**應用程式\_資料**資料夾，然後按一下**確定**。

    ![連接屬性](aspnet-mvc-4-models-and-data-access/_static/image9.png "連接屬性")

    *連接屬性*
6. 產生的類別應該具有相同名稱做為實體連接字串，因此它將名稱變更為**MusicStoreEntities**按一下**下一步**。

    ![選擇資料連接](aspnet-mvc-4-models-and-data-access/_static/image10.png "選擇資料連接")

    *選擇資料連接*
7. 選擇要使用的資料庫物件。 當實體模型會使用只在資料庫資料表時，選取**資料表**選項，並確定**在模型中包含外部索引鍵資料行**和**將化或單數化產生物件名稱**也會選取選項。 若要將模型命名空間變更**MvcMusicStore.Model**按一下**完成**。

    ![選擇的資料庫物件](aspnet-mvc-4-models-and-data-access/_static/image11.png "選擇的資料庫物件")

    *選擇的資料庫物件*

    > [!NOTE]
    > 如果顯示安全性警告對話方塊，按一下**確定**執行範本，並產生該模型實體的類別。
8. 對應至資料庫的每個資料表的不同類別將會建立資料庫的實體圖表將會出現。 例如，**專輯**資料表將會由**專輯**類別，其中每個資料行的資料表中會對應至類別內容。 這可讓您查詢及處理物件，代表在資料庫中的資料列。

    ![實體圖](aspnet-mvc-4-models-and-data-access/_static/image12.png "實體圖表")

    *實體圖表*

    > [!NOTE]
    > T4 範本 (.tt) 執行程式碼來產生實體類別，並具有相同名稱將會覆寫現有的類別。 在此範例中，類別&quot;專輯&quot;，&quot;類型&quot;和&quot;演出者&quot;加以覆寫與產生的程式碼。


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>工作 3-建置應用程式

在這項工作，您將檢查，雖然移除了模型產生**專輯**，**類型**和**演出者**模型類別的專案建置成功使用新的資料模型類別。

1. 建置專案時選取**建置**功能表項目，然後**建置 MvcMusicStore**。

    ![建置專案](aspnet-mvc-4-models-and-data-access/_static/image13.png "建置專案")

    *建置專案*
2. 專案建置成功。 為什麼它仍然運作？ 這是因為資料庫資料表已將您所使用的屬性包含已移除的類別中的欄位**專輯**和**類型**。

    ![組建成功](aspnet-mvc-4-models-and-data-access/_static/image14.png "成功的組建")

    *成功的組建*
3. 設計工具會以圖表格式顯示實體，但它們實際上是 C# 類別。 展開**StoreDB.edmx** [方案總管] 中的節點，然後**StoreDB.tt**，您會看到新產生的實體。

    ![產生的檔案](aspnet-mvc-4-models-and-data-access/_static/image15.png "產生的檔案")

    *產生的檔案*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>工作 4-查詢資料庫

在這項工作，您將更新 StoreController 類別，以便不使用硬式編碼資料，它會查詢資料庫，以擷取資訊。

1. 開啟**Controllers\StoreController.cs**並將下列欄位加入至要保存的執行個體的類別**MusicStoreEntities**類別，名為**storeDB**:

    (程式碼片段-*模型和資料存取層 Ex1 storeDB*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
~~~
2. **MusicStoreEntities**類別會公開資料庫中每個資料表集合屬性。 更新**瀏覽**要擷取的內容類型的所有動作方法**專輯**。

    (程式碼片段-*模型和資料存取-Ex1 存放區瀏覽*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]
~~~

> [!NOTE]
> 您正在使用之.NET 呼叫的功能**LINQ** (language integrated query) 撰寫強型別的查詢運算式，針對這些集合的執行對資料庫的程式碼，並傳回物件，您可以程式設計針對。
> 
> 如需有關 LINQ 的詳細資訊，請瀏覽[msdn 網站](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx)。


3. 更新**索引**動作方法，以擷取所有內容類型。

    (程式碼片段-*模型和資料存取-Ex1 存放區索引*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
~~~
4. 更新**索引**動作方法，來擷取所有內容類型，並轉換為清單集合。

    (程式碼片段-*模型和資料存取-Ex1 存放區 GenreMenu*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]
~~~

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>工作 5-執行應用程式

在這項工作，您會檢查存放區索引頁面現在會顯示儲存在資料庫中而不是硬式編碼的內容類型。 若要變更檢視範本，因為不需要**StoreController**所傳回的相同實體如往常一般，但這次的資料將來自資料庫。

1. 重建方案，請按**F5**執行應用程式。
2. 在首頁上，啟動專案。 確認的功能表**內容類型**不再是硬式編碼清單，並直接從資料庫擷取資料。

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *從資料庫中瀏覽內容類型*
3. 現在瀏覽至任何類型，並確認將從資料庫填入專輯。

    ![從資料庫中瀏覽專輯](aspnet-mvc-4-models-and-data-access/_static/image17.png "瀏覽的相簿，從資料庫")

    *從資料庫中瀏覽相簿*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>練習 2： 建立資料庫，先使用程式碼

在此練習中，您將學習如何使用 Code First 的方式來建立資料庫與資料表 MusicStore 應用程式，以及如何存取其資料。

一旦模型產生後，您將修改 StoreController 取自資料庫，而不要使用硬式編碼值的資料提供檢視範本。

> [!NOTE]
> 如果您已完成練習 1，且已經使用資料庫第一種方法，您現在將會了解如何取得不同的程序相同的結果。 若要簡化您的讀取已標示和練習 1 相同的工作。 如果您尚未完成練習 1，但想要了解程式碼第一種方法，您可以從這個練習中啟動，並取得主題的完整涵蓋範圍。


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>工作 1-填入範例資料

在這項工作，您將資料庫中填入範例資料 intially 建立使用程式碼優先 （contract-first） 時。

1. 開啟**開始**方案位於**來源/Ex2CreatingADatabaseCodeFirst/開始/** 資料夾。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

   1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 新增**SampleData.cs**檔案**模型**資料夾。 若要這樣做，請以滑鼠右鍵按一下**模型**資料夾，指向**新增**，然後按一下 **現有項目**。 瀏覽至**\Source\Assets**選取**SampleData.cs**檔案。

    ![範例資料填入的程式碼](aspnet-mvc-4-models-and-data-access/_static/image18.png "範例資料填入的程式碼")

    *範例資料填入的程式碼*
3. 開啟**Global.asax.cs**檔案，然後加入下列*使用*陳述式。

    (程式碼片段-*模型和資料存取-Ex2 全域 Asax Using*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
~~~
4. 在**應用程式\_start**方法中加入下列這一行設定資料庫初始設定式。

    (程式碼片段-*模型和資料存取-Ex2 全域 Asax SetInitializer*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]
~~~

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>工作 2-設定資料庫的連接

既然您已新增資料庫專案，您會撰寫**Web.config**檔案的連接字串。

1. 加入連接字串在**Web.config**。若要這樣做，請開啟**Web.config**在專案根目錄和取代連接字串中的這一行以命名 DefaultConnection **&lt;connectionStrings&gt;** > 一節：

    ![Web.config 檔案位置](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config 檔案位置")

    *web.config 檔案位置*


~~~
[!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>工作 3-使用模型

既然您已經設定資料庫的連接，您將連結的模型與資料庫資料表。 在這項工作，您將建立的類別，將會連結到第一個程式碼的資料庫。 請記住是應該修改存在 POCO 模型類別。

   > [!NOTE]
> 如果您已完成練習 1，您會注意由精靈已執行此步驟。 透過這種程式碼第一次，您將手動建立將會連結到資料實體的類別。


1. 開啟 POCO 模型類別**類型**從**模型**專案資料夾，並包含 id。 使用 int 屬性具有名稱**GenreId**。

    (程式碼片段-*模型和資料存取-Ex2 程式碼的第一個內容類型*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

> [!NOTE]
> To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.
> 
> You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
~~~
2. 現在，開啟 POCO 模型類別**專輯**從**模型**專案資料夾和包含的外部索引鍵，建立屬性的名稱**GenreId**和**ArtistId**。 這個類別已經有**GenreId**主索引鍵。

    (程式碼片段-*模型和資料存取-Ex2 程式碼的第一個專輯*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
~~~
3. 開啟 POCO 模型類別**演出者**並包含**ArtistId**屬性。

    (程式碼片段-*模型和資料存取-Ex2 程式碼的第一個演出者*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
~~~
4. 以滑鼠右鍵按一下**模型**專案資料夾，然後選取**新增 |類別**。 將檔案命名**MusicStoreEntities.cs**。 然後，按一下 **新增。**

    ![將類別加入](aspnet-mvc-4-models-and-data-access/_static/image20.png "加入類別")

    *加入新項目*

    ![加入 class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "加入 class2")

    *加入類別*
5. 開啟您剛才建立的類別**MusicStoreEntities.cs**，包含命名空間和**System.Data.Entity**和**System.Data.Entity.Infrastructure**。


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
~~~
6. 取代類別宣告，以擴充**DbContext**類別： 宣告公用**DBSet**並覆寫**OnModelCreating**方法。 在此步驟之後，您會收到將會連結您的模型與 Entity Framework 的領域類別。 若要這樣做，請以下列內容取代類別程式碼：

    (程式碼片段-*模型和資料存取-Ex2 程式碼的第一個 MusicStoreEntities*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre. By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table. You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)
~~~

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>工作 4-查詢資料庫

在這項工作，您將更新 StoreController 類別，以便不使用硬式編碼資料，它將它從資料庫擷取。

> [!NOTE]
> 這項工作是與啟動器相同之練習 1。
> 
> 如果您已完成練習 1 表示您會發現這些步驟是這兩種方法中的相同 (第一次資料庫或 Code first)。 不在使用模型時，會連結的資料相同，但資料實體的存取尚未從控制器透明。


1. 開啟**Controllers\StoreController.cs**並將下列欄位加入至要保存的執行個體的類別**MusicStoreEntities**類別，名為**storeDB**:

    (程式碼片段-*模型和資料存取層 Ex1 storeDB*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
~~~
2. **MusicStoreEntities**類別會公開資料庫中每個資料表集合屬性。 更新**瀏覽**要擷取的內容類型的所有動作方法**專輯**。

    (程式碼片段-*模型和資料存取-Ex2 存放區瀏覽*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
~~~
3. 更新**索引**動作方法，以擷取所有內容類型。

    (程式碼片段-*模型和資料存取-Ex2 存放區索引*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
~~~
4. 更新**索引**動作方法，來擷取所有內容類型，並轉換為清單集合。

    (程式碼片段-*模型和資料存取-Ex2 存放區 GenreMenu*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]
~~~

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>工作 5-執行應用程式

在這項工作，您會檢查存放區索引頁面現在會顯示儲存在資料庫中而不是硬式編碼的內容類型。 若要變更檢視範本，因為不需要**StoreController**所傳回的相同**StoreIndexViewModel**如往常一般，但這次的資料將來自資料庫。

1. 重建方案，請按**F5**執行應用程式。
2. 在首頁上，啟動專案。 確認的功能表**內容類型**不再是硬式編碼清單，並直接從資料庫擷取資料。

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *從資料庫中瀏覽內容類型*
3. 現在瀏覽至任何類型，並確認將從資料庫填入專輯。

    ![從資料庫中瀏覽專輯](aspnet-mvc-4-models-and-data-access/_static/image23.png "瀏覽的相簿，從資料庫")

    *從資料庫中瀏覽相簿*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>練習 3： 查詢參數的資料庫

在此練習中，您將學習如何查詢資料庫中使用參數，以及如何使用查詢結果成形的功能會減少數字的資料庫會以更有效率的方式存取擷取資料。

> [!NOTE]
> 如需查詢結果來形成的進一步資訊，請造訪下列[msdn 文章](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx)。


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>工作 1-從資料庫擷取專輯修改 StoreController

在這個工作中，您將變更**StoreController**類別來存取資料庫，以擷取特定的內容類型中的專輯。

1. 開啟**開始**方案位於**Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin**資料夾，如果您想要使用程式碼優先方法或**Source\Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin**資料夾，如果您想要使用資料庫第一種方法。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

   1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 開啟**StoreController**類別，以變更**瀏覽**動作方法。 若要這樣做，請在**方案總管 中**，依序展開**控制器**資料夾，然後按兩下**StoreController.cs**。
3. 變更**瀏覽**動作方法，以擷取特定的內容類型的相簿。 若要這樣做，請取代下列程式碼：

    (程式碼片段-*模型和資料存取-Ex3 StoreController BrowseMethod*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too. You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album. The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.
> 
> You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved. This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information. In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.
> 
> The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well. This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>工作 2-執行應用程式

在這項工作，您將執行應用程式，並從資料庫擷取的特定內容類型的相簿。

1. 按**F5**執行應用程式。
2. 在首頁上，啟動專案。 將 URL 變更為 **/存放區/瀏覽？ 內容類型 = Pop**驗證，正在從資料庫擷取的結果。

    ![內容類型來瀏覽](aspnet-mvc-4-models-and-data-access/_static/image24.png "內容類型來瀏覽")

    *瀏覽/存放區/瀏覽？ 內容類型 = Pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>工作 3-Id 所存取的相簿

在這項工作，您將會重複先前的程序，以取得其識別碼的相簿

1. 關閉瀏覽器，如有需要若要返回 Visual Studio。 開啟**StoreController**類別，以變更**詳細資料**動作方法。 若要這樣做，請在**方案總管 中**，依序展開**控制器**資料夾，然後按兩下**StoreController.cs**。
2. 變更**詳細資料**擷取專輯詳細資料的動作方法會根據其**識別碼**。若要這樣做，請取代下列程式碼：

    (程式碼片段-*模型和資料存取-Ex3 StoreController DetailsMethod*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>工作 4-執行應用程式

在這項工作，您會在網頁瀏覽器中執行應用程式，並取得專輯詳細資料，其識別碼。

1. 按**F5**執行應用程式。
2. 在首頁上，啟動專案。 將 URL 變更為 **/Store/Details/51**或瀏覽內容類型，並選取要確認結果要從資料庫擷取的相簿。

    ![瀏覽詳細資料](aspnet-mvc-4-models-and-data-access/_static/image25.png "瀏覽詳細資料")

    *瀏覽 /Store/Details/51*

> [!NOTE]
> 此外，您可以部署此應用程式以 Windows Azure Web Sites 下列[附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixB)。


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>總結

藉由完成這個實際操作實驗室，您學到的 ASP.NET MVC 模型和資料存取的基本概念，並透過**Database First**方法以及**Code First**方法：

- 如何將資料庫加入方案，才能取用它的資料
- 如何更新控制器以取自資料庫而不是硬式編碼的資料提供檢視範本
- 如何查詢使用參數的資料庫
- 如何使用查詢結果成形，減少資料庫存取的功能更有效率的方式擷取資料
- 如何使用 Microsoft Entity Framework 中的資料庫第一和第一個程式碼方法來連結的模型資料庫

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附錄 a： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 與 Windows Azure SDK</em>&quot;。
2. 按一下**立即安裝**。 如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。

    ![接受授權條款](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *接受授權條款*
5. 等待直到完成下載和安裝程序。

    ![安裝進度](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *安裝進度*
6. 當安裝完成時，按一下**完成**。

    ![安裝已完成](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *安裝已完成*
7. 按一下**結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。

    ![VS Express for Web 方塊](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    *VS Express for Web 方塊*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy

此附錄將說明如何從 Windows Azure 管理入口網站建立新的網站和發行您取得依照實驗室中，利用 Web Deploy 發行功能的 Windows Azure 所提供的應用程式。

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>工作 1-建立新的網站，從 Windows Azure 入口網站

1. 移至[Windows Azure 管理入口網站](https://manage.windowsazure.com/)並使用與您訂用帳戶相關聯的 Microsoft 認證登入。

    > [!NOTE]
    > 與 Windows Azure 中，您可以免費裝載 10 個 ASP.NET 網站並再擴充隨流量成長。 您可以註冊[這裡](http://aka.ms/aspnet-hol-azure)。

    ![登入 Windows Azure 入口網站](aspnet-mvc-4-models-and-data-access/_static/image31.png "登入 Windows Azure 入口網站")

    *登入 Windows Azure 管理入口網站*
2. 按一下**新增**命令列上。

    ![建立新的網站](aspnet-mvc-4-models-and-data-access/_static/image32.png "建立新的網站")

    *建立新的網站*
3. 按一下**計算** | **網站**。 然後選取**快速建立**選項。 新的網站上提供可用的 URL，然後按一下**建立網站**。

    > [!NOTE]
    > Windows Azure 網站是您可以控制和管理在雲端中執行的 web 應用程式的主機。 快速建立 選項可讓您部署已完成的 web 應用程式至 Windows Azure 網站從入口網站外部。 它不包含設定資料庫的步驟。

    ![建立新的網站使用 快速建立](aspnet-mvc-4-models-and-data-access/_static/image33.png "建立新的網站使用 快速建立")

    *建立新的網站使用 快速建立*
4. 等候新**網站**建立。
5. 建立網站之後按一下底下的連結**URL**資料行。 請檢查新的網站運作。

    ![瀏覽至新的網站](aspnet-mvc-4-models-and-data-access/_static/image34.png "瀏覽至新的網站")

    *瀏覽至新的網站*

    ![執行網站](aspnet-mvc-4-models-and-data-access/_static/image35.png "執行的網站")

    *執行的網站*
6. 返回入口網站並按一下底下的網站名稱**名稱**資料行會顯示 [管理] 頁面。

    ![開啟網站管理頁面](aspnet-mvc-4-models-and-data-access/_static/image36.png "開啟網站管理頁面")

    *開啟網站管理頁面*
7. 在**儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。

    > [!NOTE]
    > *發行設定檔*包含所有 web 應用程式發行至 Windows Azure 網站的每個已啟用的發行方法所需的資訊。 發行設定檔包含 Url、 使用者認證和資料庫連接，並對每個端點已啟用的發行集的方法進行驗證所需的字串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支援讀取發行設定檔自動將這些程式的組態web 應用程式發行至 Windows Azure 網站。

    ![下載網站發行設定檔](aspnet-mvc-4-models-and-data-access/_static/image37.png "下載網站發行設定檔")

    *下載網站發行設定檔*
8. 下載發行設定檔至已知位置。 進一步在這個練習中，您會看到如何使用這個檔案從 Visual Studio 將 web 應用程式至 Windows Azure 網站發行。

    ![儲存發行設定檔](aspnet-mvc-4-models-and-data-access/_static/image38.png "儲存發行設定檔")

    *儲存發行設定檔*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>工作 2-設定資料庫伺服器

如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。 如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。

1. 您將需要 SQL Database 伺服器來儲存應用程式資料庫。 您可以從您的訂用帳戶，在 Windows Azure 管理入口網站中檢視 SQL Database 伺服器**Sql 資料庫** | **伺服器** | **伺服器儀表板**。 如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。 請注意**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，因為您將在下一個工作中使用它們。 並未建立資料庫，因為它會在後續階段中建立。

    ![SQL Database 伺服器儀表板](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database 伺服器儀表板")

    *SQL Database 伺服器儀表板*
2. 下一項工作在您將測試資料庫連接，從 Visual Studio，因此您需要在伺服器的清單中包含您的本機 IP 位址**允許的 IP 位址**。 若要這樣做，請按一下**設定**，選取 [IP 位址從**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**和**結束 IP 位址**文字方塊，然後按一下![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) ] 按鈕。

    ![加入用戶端 IP 位址](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *加入用戶端 IP 位址*
3. 一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。

    ![確認變更](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *確認變更*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy

1. 返回至 ASP.NET MVC 4 方案。 在**方案總管 中**，以滑鼠右鍵按一下網站專案，然後選取**發行**。

    ![發行應用程式](aspnet-mvc-4-models-and-data-access/_static/image43.png "發行應用程式")

    *發行網站*
2. 匯入發行設定檔儲存在第一項工作中。

    ![匯入發行設定檔](aspnet-mvc-4-models-and-data-access/_static/image44.png "匯入發行設定檔")

    *匯入發行設定檔*
3. 按一下**驗證連線**。 驗證完成後按一下**下一步**。

    > [!NOTE]
    > 一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。

    ![驗證連線](aspnet-mvc-4-models-and-data-access/_static/image45.png "驗證連線")

    *驗證連接*
4. 在**設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。

    ![Web 部署設定](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web 部署設定")

    *Web 部署設定*
5. 設定資料庫連接，如下所示：

   - 在**伺服器名稱**您 SQL Database 伺服器 URL 使用下列方法類型*tcp:* 前置詞。
   - 在**使用者名**輸入您的伺服器系統管理員身分登入名稱。
   - 在**密碼**輸入伺服器系統管理員身分登入密碼。
   - 輸入新的資料庫名稱。

     ![設定目的地連接字串](aspnet-mvc-4-models-and-data-access/_static/image47.png "設定目的地連接字串")

     *設定目的地連接字串*
6. 然後按一下 [確定]。  當系統提示您建立資料庫依序按一下**是**。

    ![建立資料庫](aspnet-mvc-4-models-and-data-access/_static/image48.png "建立的資料庫字串")

    *建立資料庫*
7. 您將用來連接到在 Windows Azure SQL Database 的連接字串會顯示在預設連接文字方塊中。 然後按 [下一步] 。

    ![連接字串指向 SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "連接字串指向 SQL 資料庫")

    *連接字串指向 SQL 資料庫*
8. 在**預覽**頁面上，按一下**發行**。

    ![Web 應用程式發行](aspnet-mvc-4-models-and-data-access/_static/image50.png "發行 web 應用程式")

    *發行 web 應用程式*
9. 一旦在發佈程序完成時，預設瀏覽器會開啟已發行的網站。

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>附錄 c： 使用程式碼片段

程式碼片段，您會有您需要在您可以的所有程式碼。 實驗室文件會告訴您完全時您可以使用它們，如下圖所示。

![若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段](aspnet-mvc-4-models-and-data-access/_static/image51.png "使用 Visual Studio 程式碼片段，將程式碼插入您的專案")

*若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段*

***若要加入的程式碼片段，使用鍵盤 （僅限 C#)***

1. 將游標放在您想要插入的程式碼。
2. 開始鍵入片段名稱 （不含空格或連字號）。
3. 觀察 IntelliSense 會顯示比對程式碼片段的名稱。
4. 選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。
5. 按下 Tab 鍵兩次的游標位置插入程式碼片段。

![開始輸入程式碼片段名稱](aspnet-mvc-4-models-and-data-access/_static/image52.png "開始鍵入片段名稱")

*開始輸入程式碼片段名稱*

![按 Tab 鍵選取反白顯示的程式碼片段](aspnet-mvc-4-models-and-data-access/_static/image53.png "按 Tab 鍵以選取反白顯示的程式碼片段")

*按 Tab 鍵選取反白顯示的程式碼片段*

![按 Tab 鍵一次程式碼片段就會擴展](aspnet-mvc-4-models-and-data-access/_static/image54.png "按 Tab 鍵一次程式碼片段就會擴展")

*按 Tab 鍵一次程式碼片段就會擴展*

***若要加入的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。 以滑鼠右鍵按一下您要插入程式碼片段。

1. 選取**插入程式碼片段**後面**我的程式碼片段**。
2. 透過在按一下挑選清單中，從相關的程式碼片段。

![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-models-and-data-access/_static/image55.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")

*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*

![透過在按一下挑選清單中，從相關的程式碼片段](aspnet-mvc-4-models-and-data-access/_static/image56.png "上它即可挑選清單中，從相關的程式碼片段")

*透過在按一下挑選清單中，從相關的程式碼片段*
