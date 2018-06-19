---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: 什麼是 Entity Framework 4.0 新功能 |Microsoft 文件
author: tdykstra
description: 此教學課程系列為基礎所建立的開始使用 Entity Framework 4.0 教學課程系列的 Contoso 大學 web 應用程式。 I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 04444ce98fa60045cf617a6c518dd55677258148
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889182"
---
<a name="whats-new-in-the-entity-framework-40"></a>什麼是 Entity Framework 4.0 新功能
====================
由[Tom Dykstra](https://github.com/tdykstra)

> 此教學課程系列為基礎所建立的 Contoso 大學 web 應用程式[Entity Framework 使用者入門](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)教學課程系列。 如果您未完成先前的教學課程，您可以身分本教學課程的起始點來[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。 您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完整的教學課程所建立。 如果您有疑問的教學課程，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)。


在上一個教學課程中，您會看到一些方法來使用 Entity Framework 的 web 應用程式的效能最大化。 本教學課程會檢閱一些最重要的新功能，Entity Framework 中，第 4 版中，它會連結到資源提供更完整的所有新功能的簡介。 本教學課程中反白顯示的功能包括：

- 外部索引鍵關聯。
- 執行使用者定義的 SQL 命令。
- 模型優先開發。
- POCO 支援。

此外，本教學課程將簡單介紹*程式碼優先開發*，未來 Entity Framework 的下一個版本的功能。

若要開始本教學課程，請啟動 Visual Studio 並開啟您在上一個教學課程使用 Contoso 大學 web 應用程式。

## <a name="foreign-key-associations"></a>外部索引鍵關聯

Entity Framework 3.5 版包含導覽屬性，但其中不包含資料模型中的外部索引鍵屬性。 例如，`CourseID`和`StudentID`的資料行`StudentGrade`將忽略資料表`StudentGrade`實體。

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

這種方法的原因是，嚴格來說，外部索引鍵是實體的實作詳細資料，並且不屬於概念性資料模型中。 不過，事實上，通常會很容易使用程式碼中的實體時，您可以直接存取的外部索引鍵。

針對如何外部索引鍵的資料模型的範例，可簡化您的程式碼，請考慮如何您原本的程式碼*DepartmentsAdd.aspx*未加上的頁面。 在`Department`實體，`Administrator`屬性是對應至外部索引鍵`PersonID`中`Person`實體。 若要建立新的部門 」 和 「 以系統管理員之間的關聯，您必須執行所有已設定的值`Administrator`屬性`ItemInserting`資料繫結控制項的事件處理常式：

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

資料模型中的外部索引鍵，不會處理`Inserting`事件的資料來源控制項，而不是`ItemInserting`事件的資料繫結控制項，以實體加入至實體集之前，取得實體本身的參考。 當您擁有該參考時，您會建立在下列範例類似的使用程式碼的關聯：

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

您可以看到在 Entity Framework 小組[上外部索引鍵關聯的部落格文章](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx)，有更大的差異，在程式碼複雜度其他情況。 為了滿足那些喜歡存留較簡單的程式碼為了概念資料模型中的實作詳細資料的需求，Entity Framework 現在可以讓您在資料模型中包含外部索引鍵的選項。

在 Entity Framework 詞彙中，如果您在資料模型中包含外部索引鍵使用*外部索引鍵關聯*，如果您排除您所使用的外部索引鍵和*獨立關聯*。

## <a name="executing-user-defined-sql-commands"></a>執行使用者定義的 SQL 命令

在舊版的 Entity Framework 中，有無簡單的方式來即時建立您自己的 SQL 命令，並且執行它們。 Entity Framework 動態產生的 SQL 命令，或是您必須建立預存程序，並為函式將它匯入。 第 4 版新增`ExecuteStoreQuery`和`ExecuteStoreCommand`方法`ObjectContext`類別可讓您更輕鬆地傳遞直接對資料庫的任何查詢。

假設 Contoso 大學系統管理員想要能夠在資料庫中執行大量變更，而不需要經歷建立預存程序，並在匯入資料模型的程序。 其第一個要求是對可讓他們變更數目的資料庫中的所有課程信用額度的頁面。 他們想要可以輸入要使用的值相乘的數字在網頁上，每個`Course`資料列的`Credits`資料行。

建立新的頁面使用*Site.Master*主版頁面並將其命名*UpdateCredits.aspx*。 然後加入下列標記以`Content`控制項，名為`Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

這個標記會建立`TextBox`控制使用者可以在其中輸入數值，`Button`控制項才能執行此命令中，按一下與`Label`控制項，以便指出受影響的資料列數目。

開啟*UpdateCredits.aspx.cs*，並加入下列`using`陳述式和按鈕的處理常式`Click`事件：

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

此程式碼會執行 SQL`Update`命令使用文字方塊內的值，並使用標籤顯示的受影響的資料列數目。 執行網頁之前，先執行*Courses.aspx*頁面，即可取得 「 之前 」 圖片的一些資料。

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

執行*UpdateCredits.aspx*乘數，輸入 「 10 」，然後按**Execute**。

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

執行*Courses.aspx*頁面，即可查看變更的資料。

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(如果您想要設定的信用額度數目設回其原始值，在*UpdateCredits.aspx.cs*變更`Credits * {0}`至`Credits / {0}`並重新執行 頁面上，輸入 10 做為除數。)

如需有關如何執行您在程式碼中定義的查詢的詳細資訊，請參閱[如何： 直接執行命令對資料來源](https://msdn.microsoft.com/library/ee358769.aspx)。

## <a name="model-first-development"></a>模型優先開發

這些逐步解說中您會先建立資料庫和資料庫結構為基礎的資料模型，則產生。 Entity Framework 4 中，您可以改為開頭的資料模型，並產生資料模型結構為基礎的資料庫。 如果您要建立的資料庫不存在的應用程式，模型第一種方法可讓您建立實體和關聯性在概念上對應用程式時不擔心實體實作詳細資料意義. （這會保持只能透過開發的初始階段，則為 true 不過。 最後將會建立資料庫，並會在實際執行資料，並重新建立從模型將無法再實際;此時，您會回到資料庫第一種方法。）

在本章節的教學課程中，您將建立簡單的資料模型，並從其中產生資料庫。

在**方案總管 中**，以滑鼠右鍵按一下*DAL*資料夾，然後選取**加入新項目**。 在**加入新項目**對話方塊的 **已安裝的範本**選取**資料**，然後選取  **ADO.NET 實體資料模型**範本. 新的檔案名稱*AlumniAssociationModel.edmx*按一下**新增**。

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

這會啟動實體資料模型精靈。 在**選擇模型內容**步驟中，選取**空的模型**，然後按一下 **完成**。

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

**Entity Data Model Designer**與空白設計介面隨即開啟。 拖曳**實體**項目從**工具箱**拖曳至設計介面。

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

變更實體名稱，從`Entity1`至`Alumnus`，變更`Id`屬性名稱，以`AlumnusId`，並加入新的純量屬性，名為`Name`。 若要將新屬性加入您可以按 Enter 鍵的名稱變更之後`Id`資料行，或以滑鼠右鍵按一下實體，並選取**加入純量屬性**。 新屬性的預設類型是`String`，這是此簡單的示範，不過沒有關係，但當然您也可以變更中的資料類型等**屬性**視窗。

相同的方式來建立另一個實體並將其命名`Donation`。 變更`Id`屬性`DonationId`並新增名為的純量屬性`DateAndAmount`。

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

若要新增這兩個實體之間的關聯，以滑鼠右鍵按一下`Alumnus`實體中，選取**新增**，然後選取**關聯**。

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

中的預設值**新增關聯**對話方塊都是您想要的結果 （一對多、 包含導覽屬性，包含外部索引鍵），所以只需按一下**確定**。

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

設計工具加入關聯線和外部索引鍵屬性。

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

現在您已準備好要建立資料庫。 以滑鼠右鍵按一下設計介面，並選取**由模型產生資料庫**。

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

這會啟動產生資料庫精靈。 （如果您看到警告，指出沒有對應的實體，您可以忽略這些緩。）

在**選擇資料連接**步驟中，按一下 **新連線**。

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

在**連接屬性**對話方塊，選取 SQL Server Express 的本機執行個體，為資料庫命名`AlumniAsssociation`。

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

按一下**是**當詢問您要建立資料庫。 當**選擇資料連接**步驟一次，請按一下 **下一步**。

在**摘要和設定**步驟中，按一下 **完成**。

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

A *.sql*使用資料定義語言 (DDL) 命令來建立檔案，但您尚未尚未執行的命令。

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

使用一種工具，例如**SQL Server Management Studio**執行指令碼，並建立資料表，您可能執行您在建立時`School`資料庫[快速入門教學課程系列的第一個教學課程](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). （除非您下載此資料庫。）

您現在可以使用`AlumniAssociation`資料模型，在您的 web pages 您所使用的相同方式`School`模型。 若要再試一次時，一些資料加入至資料表並建立顯示資料的網頁。

使用**伺服器總管**，加入下列的資料列來`Alumnus`和`Donation`資料表。

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

建立名為的新網頁*Alumni.aspx*使用*Site.Master*主版頁面。 加入下列標記以`Content`控制項，名為`Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

這個標記會建立巢狀`GridView`控制外部顯示申請書名稱以及與內部的一個顯示捐贈日期和金額。

開啟*Alumni.aspx.cs*。 新增`using`陳述式的資料存取層和外部的處理常式`GridView`控制項的`RowDataBound`事件：

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

此程式碼將內部`GridView`控制使用`Donations`導覽屬性的目前資料列`Alumnus`實體。

執行網頁。

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(注意： 此頁面包含在可下載專案中，但讓它正常運作，您必須在本機 SQL Server Express 執行個體中建立資料庫，則資料庫不包含 *.mdf*檔案*應用程式\_資料*資料夾。)

如需有關如何使用 Entity Framework 模型優先 （contract-first） 功能的詳細資訊，請參閱[Entity Framework 4 中的模型優先](https://msdn.microsoft.com/data/ff830362.aspx)。

## <a name="poco-support"></a>POCO 支援

當您使用網域導向設計方法時，您會設計資料類別，代表資料與商務網域相關的行為。 應該獨立於任何特定的技術，用來儲存這些類別 （保存） 資料。也就是說，它們應該*永續性無知*。 永續性無知之也可以進行類別容易單元測試因為單元測試專案可以使用任何持續性技術是最方便的測試。 舊版的 Entity Framework 提供有限的支援永續性無知之實體類別必須繼承自`EntityObject`類別，並因此包含許多實體架構專屬功能。

Entity Framework 4 導入了使用不會繼承的實體類別的能力`EntityObject`類別，並因此不知道會持續性。 在 Entity Framework 內容中，像這樣的類別通常稱為*純舊 CLR 物件*（POCO 或 POCOs）。 您可以手動撰寫 POCO 類別或您可以自動產生根據現有的資料模型使用 Entity Framework 所提供的文字範本轉換工具組 (T4) 範本。

如需使用 POCOs Entity Framework 中的詳細資訊，請參閱下列資源：

- [處理 POCO 實體](https://msdn.microsoft.com/library/dd456853.aspx)。 這是 POCOs，概觀與其他有更詳細資訊的文件連結的 MSDN 文件。
- [逐步解說： POCO Entity Framework 的範本](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx)這是 Entity Framework 的開發小組，關於 POCOs 其他部落格文章的連結的部落格文章。

## <a name="code-first-development"></a>程式碼優先開發

POCO Entity Framework 4 中的支援仍需要您建立資料模型，並將實體類別連結至資料模型。 Entity Framework 的下一個版本會包含名為的功能*程式碼優先開發*。 這項功能可讓您使用 Entity Framework POCO 類別而不需要使用資料模型設計工具或資料模型的 XML 檔案。 (因此，此選項也已經呼叫*純程式碼*;*程式碼優先*和*純程式碼*都會指向相同的 Entity Framework 功能。)

如需有關如何使用程式開發的程式碼第一個方法的詳細資訊，請參閱下列資源：

- [程式碼優先使用 Entity Framework 4 開發](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx)。 這是所介紹的程式碼優先開發 Scott Guthrie 的部落格文章。
- [Entity Framework 開發小組部落格-張貼標記的 CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Entity Framework 開發小組部落格-張貼標記 Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [MVC Music Store 教學課程-第 4 部分： 模型和資料存取](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [開始使用 MVC 3-第 4 部分： Entity Framework 程式碼優先開發](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

此外，新的 MVC 程式碼的第一個教學課程可建立類似於 Contoso 大學應用程式的應用程式預計在 2011 年的 spring 中發行 [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>更多資訊

完成此動作以新增功能在 Entity Framework 與此繼續 Entity Framework 的教學課程系列的概觀。 如需有關 Entity Framework 4 中未涵蓋的新功能的詳細資訊，請參閱下列資源：

- [What's New in ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx)的 Entity Framework 版本 4 中的新功能的 MSDN 主題。
- [宣佈適用於 Entity Framework 4 版本](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx)中第 4 版新功能相關的 Entity Framework 開發小組的部落格文章。

> [!div class="step-by-step"]
> [上一步](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
