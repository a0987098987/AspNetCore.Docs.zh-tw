---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: 什麼是 Entity Framework 4.0 的新功能 |Microsoft Docs
author: tdykstra
description: 本教學課程系列由開始使用 Entity Framework 4.0 的教學課程系列的 Contoso 大學 web 應用程式為基礎。 我...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 402e7ace1abad899d32ed179d6b68de4e5a129f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830786"
---
<a name="whats-new-in-the-entity-framework-40"></a>Entity Framework 4.0 中最新消息
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> 本教學課程系列是根據所建立的 Contoso 大學 web 應用程式[Getting Started with Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)教學課程系列。 如果您未完成先前的教學課程，為本教學課程的起始點即可[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。 您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)，它由完整的教學課程系列。 如果您有疑問的教學課程，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)。


在上一個教學課程，您會看到一些方法來將使用 Entity Framework 的 web 應用程式的效能最大化。 本教學課程中檢閱的一些最重要的新功能的 Entity Framework 版本 4 中，它會連結到資源提供所有新功能的更完整介紹。 在本教學課程中反白顯示的功能包括下列各項：

- 外部索引鍵的關聯。
- 執行使用者定義的 SQL 命令。
- 模型優先開發。
- POCO 支援。

此外，本教學課程將簡短介紹*code first 開發*，即將在下一個版本的 Entity Framework 的功能。

若要開始本教學課程，請啟動 Visual Studio，並開啟您已在上一個教學課程中使用的 Contoso 大學 web 應用程式。

## <a name="foreign-key-associations"></a>外部索引鍵的關聯

Entity Framework 3.5 版包含導覽屬性，但其未包含資料模型中的外部索引鍵屬性。 例如，`CourseID`並`StudentID`的資料行`StudentGrade`資料表也會一併省略從`StudentGrade`實體。

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

這種方法的原因是，嚴格來說，外部索引鍵是實體的實作詳細資料，並不屬於概念資料模型中。 不過，實際上，通常很容易使用的程式碼中的實體，當您直接存取的外部索引鍵。

如需資料模型中如何外部索引鍵的範例可以簡化程式碼，請考慮如何您原本的程式碼*DepartmentsAdd.aspx*未加上的頁面。 在 `Department`實體`Administrator`屬性會對應至外部索引鍵`PersonID`中`Person`實體。 若要建立新的部門和其系統管理員之間的關聯，您只需要設定的值`Administrator`屬性中的`ItemInserting`的資料繫結控制項的事件處理常式：

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

沒有資料模型中的外部索引鍵，您會處理`Inserting`事件的資料來源控制項，而不是`ItemInserting`事件的資料繫結控制項，以取得實體本身的參考，才能將該實體加入至實體集。 當您擁有該參考時，您會建立關聯的使用中的下列範例的程式碼：

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

如您在 Entity Framework 小組中所見[外部索引鍵關聯上的部落格文章](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx)，其他地方的程式碼複雜度差別更大的情況下。 為想要即時為了更簡單的程式碼的概念資料模型中的實作詳細資料的任何人的需求，Entity Framework 現在可以讓您選擇的資料模型中包括外部索引鍵。

在 Entity Framework 詞彙中，您的資料模型中包含外部索引鍵，如果您使用*外部索引鍵關聯*，而且如果您排除您使用的外部索引鍵*獨立關聯*。

## <a name="executing-user-defined-sql-commands"></a>執行使用者定義的 SQL 命令

在舊版的 Entity Framework 中，有無簡單的方式，立即建立您自己的 SQL 命令，並執行它們。 Entity Framework 動態產生的 SQL 命令，或是您必須建立預存程序，並為函式將它匯入。 第 4 版新增`ExecuteStoreQuery`並`ExecuteStoreCommand`方法`ObjectContext`類別可讓您更輕鬆地傳遞直接與資料庫的任何查詢。

假設 Contoso 大學的系統管理員想要能夠在資料庫中執行大量變更，而不需要經歷建立預存程序，並在匯入資料模型的程序。 第一個要求是，讓他們變更的資料庫中的所有課程的學分數頁面。 在網頁上，他們想要可以輸入要用來將值相乘的數字每隔`Course`資料列的`Credits`資料行。

建立新的頁面會*Site.Master*主版頁面，然後將它命名*UpdateCredits.aspx*。 然後新增下列標記，即可`Content`控制項，名為`Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

此標記會建立`TextBox`使用者可以在其中輸入乘數的值，控制`Button`才能執行此命令中，按一下 控制和`Label`控制項，指出受影響的資料列數目。

開啟*UpdateCredits.aspx.cs*，並新增下列`using`陳述式和按鈕的處理常式`Click`事件：

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

此程式碼執行的 SQL`Update`命令使用的值，在文字方塊中，並使用標籤來顯示受影響的資料列數目。 執行此頁面之前，先執行*Courses.aspx*頁面，即可取得 「 過去 」 圖片的一些資料。

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

執行*UpdateCredits.aspx*乘數，請輸入"10"，然後按一下  **Execute**。

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

執行*Courses.aspx*頁面以查看變更的資料。

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(如果您想要設定的學分數設回其原始值，在*UpdateCredits.aspx.cs*變更`Credits * {0}`到`Credits / {0}`並重新執行 頁面上，輸入 10 做為除數。)

如需有關如何執行您在程式碼中定義的查詢的詳細資訊，請參閱 <<c0> [ 如何： 直接執行命令對資料來源](https://msdn.microsoft.com/library/ee358769.aspx)。

## <a name="model-first-development"></a>模型優先開發

這些逐步解說中您會先建立資料庫，並產生資料庫結構為基礎的資料模型。 在 Entity Framework 4 中，您可以改為使用資料模型開始，並產生資料模型結構為基礎的資料庫。 如果您要建立的資料庫不存在的應用程式，模型優先方法可讓您建立實體和關聯性在概念上對應用程式，同時不擔心實際的實作詳細資料意義. （如此只能透過初始的開發階段，不過。 最終將會建立資料庫，而且都會在實際執行資料，並徹底重建模型將不再實用;此時，您會回到資料庫優先方法。）

在本節的教學課程中，您將建立簡單的資料模型，並從它產生資料庫。

在 **方案總管**，以滑鼠右鍵按一下*DAL*資料夾，然後選取**加入新項目**。 在 **加入新項目**對話方塊的 **已安裝的範本**選取**資料**，然後選取**ADO.NET 實體資料模型**範本. 將新檔案命名*AlumniAssociationModel.edmx*然後按一下**新增**。

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

這會啟動 Entity Data Model 精靈。 在 **選擇模型內容**步驟中，選取**空的模型**，然後按一下 **完成**。

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

**Entity Data Model Designer**的空白設計介面隨即開啟。 拖曳**實體**項目從**工具箱**拖曳至設計介面。

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

變更實體名稱，從`Entity1`要`Alumnus`，變更`Id`屬性名稱，以`AlumnusId`，並加入新的純量屬性，名為`Name`。 若要新增新的屬性可以按 Enter 鍵之後變更的名稱`Id`資料行，或以滑鼠右鍵按一下實體，然後選取**加入純量屬性**。 新屬性的預設類型是`String`，即適合使用這個簡單的示範，但當然您可以在此變更中的資料類型等**屬性**視窗。

相同的方式來建立另一個實體並將它命名`Donation`。 變更`Id`屬性，以`DonationId`，並新增一個名為的純量屬性`DateAndAmount`。

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

若要新增這兩個實體之間的關聯，以滑鼠右鍵按一下`Alumnus`實體中，選取**新增**，然後選取**關聯**。

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

中的預設值**新增關聯** 對話方塊中，是您想要一個對多、 包含導覽屬性 (包括外部索引鍵），因此只要按一下**確定**。

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

設計工具加入的關聯線和外部索引鍵屬性。

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

現在您已準備好要建立資料庫。 以滑鼠右鍵按一下設計介面，然後選取**從模型產生資料庫**。

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

這會啟動 [產生資料庫精靈]。 （如果您看到警告，指出未對應的實體，您可以忽略這些目前。）

在 **選擇資料連接**步驟中，按一下**新的連接**。

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

在 **連接屬性**對話方塊中，選取 本機的 SQL Server Express 執行個體，為資料庫命名`AlumniAsssociation`。

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

按一下 **是**當系統詢問您要建立資料庫。 當**選擇資料連接**步驟會再次顯示，請按一下**下一步**。

在 **摘要和設定**步驟中，按一下**完成**。

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

A *.sql*使用資料定義語言 (DDL) 命令來建立檔案，但尚未執行的命令。

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

使用一種工具，例如**SQL Server Management Studio**來執行指令碼，並且在建立資料表，您可能會在建立時完成`School`資料庫的[快速入門教學課程系列的第一個教學課程](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). （除非您已下載的資料庫。）

您現在可以使用`AlumniAssociation`資料模型，在您的 web pages 您所使用的相同方式`School`模型。 若要這麼做時，將一些資料加入資料表並建立網頁，顯示的資料。

使用**伺服器總管**，將下列資料列加入`Alumnus`和`Donation`資料表。

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

建立名為的新網頁*Alumni.aspx*使用*Site.Master*主版頁面。 新增下列標記，即可`Content`控制項，名為`Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

此標記會建立巢狀`GridView`控制外部顯示校友名稱以及內部顯示捐贈日期與數量。

開啟*Alumni.aspx.cs*。 新增`using`陳述式的資料存取層和外部的處理常式`GridView`控制項的`RowDataBound`事件：

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

此程式碼將內部`GridView`使用控制`Donations`導覽屬性的目前資料列`Alumnus`實體。

執行網頁。

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(注意： 此頁面會包含在可下載的專案中，但讓它正常運作，您必須在本機 SQL Server Express 執行個體中建立資料庫，資料庫不包含 *.mdf*中的檔案*應用程式\_資料*資料夾。)

如需使用 Entity Framework 的 「 模型優先 」 功能的詳細資訊，請參閱 < [Entity Framework 4 中的模型優先](https://msdn.microsoft.com/data/ff830362.aspx)。

## <a name="poco-support"></a>POCO 支援

當您使用網域導向設計方法時，您會設計資料類別，代表資料和商務領域相關的行為。 這些類別都應該獨立於用來儲存任何特定技術 （保留） 的資料;也就是說，它們應該*持續*。 持續性無知也方便類別進行單元測試因為單元測試專案可以使用任何持續性技術是最方便進行測試。 較早版本的 Entity Framework 會提供有限的非持續性支援，因為實體類別必須繼承自`EntityObject`類別，並因此包含大量的實體架構特有功能。

Entity Framework 4 引進了使用實體類別不繼承自`EntityObject`類別，並因此會保存非持續性。 在 Entity Framework 內容中，通常稱為類別如下*純舊 CLR 物件*（POCO 或 Poco）。 您可以手動撰寫的 POCO 類別，或您可以自動產生根據現有的資料模型使用 Entity Framework 所提供的文字範本轉換工具組 (T4) 範本。

如需使用 Poco Entity Framework 中的詳細資訊，請參閱下列資源：

- [處理 POCO 實體](https://msdn.microsoft.com/library/dd456853.aspx)。 這是 Poco，有更多詳細資訊的其他文件的連結與概觀的 MSDN 文件。
- [逐步解說： POCO Entity framework 的範本](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx)這是 Entity Framework 的開發小組，其他有關 Poco 的部落格文章的連結的部落格文章。

## <a name="code-first-development"></a>Code First 開發

POCO 支援 Entity Framework 4 中的仍需要您建立資料模型，並連結至資料模型的實體類別。 Entity Framework 的下一個版本將包含稱為*code first 開發*。 這項功能可讓您使用 Entity Framework 搭配您自己的 POCO 類別而不需要來使用資料模型設計工具或資料模型的 XML 檔案。 (因此，此選項也已呼叫*僅適用程式碼*;*code first*並*僅限程式碼的*都會指向相同的 Entity Framework 功能。)

如需使用程式開發的程式碼優先方法的詳細資訊，請參閱下列資源：

- [Code First 和 Entity Framework 4 的開發](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx)。 這是由 Scott Guthrie 介紹 code first 開發部落格文章。
- [Entity Framework 開發小組部落格-張貼標記的 CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Entity Framework 開發小組部落格-張貼標記 Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [MVC Music Store 教學課程-第 4 部分： 模型和資料存取](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [開始使用 MVC 3-第 4 部分： Entity Framework Code First 開發](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

此外，新的 MVC 程式碼-第一個教學課程可建立類似於的 Contoso 大學應用程式的應用程式預計在 2011 年春季發行 [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>更多資訊

這樣就完成的新功能的 Entity Framework 和此繼續透過 Entity Framework 的教學課程系列的概觀。 如需 Entity Framework 4 中這裡未涵蓋的新功能的詳細資訊，請參閱下列資源：

- [What's New in ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) MSDN 主題中的 Entity framework 第 4 版的新功能。
- [發表最新的 Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx)第 4 版的新功能的 Entity Framework 開發小組的部落格文章。

> [!div class="step-by-step"]
> [上一步](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
