---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: 開始使用 Entity Framework 4.0 Database 中第一次與 ASP.NET 4 Web Form |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web Forms 應用程式...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9f100ccaf5e9cfdaf0633f9bfebbad273212a0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827221"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>開始使用 Entity Framework 4.0 Database 中第一次與 ASP.NET 4 Web Form
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web Forms 應用程式。 範例應用程式是虛構的 Contoso 大學網站。 其中包括的功能有學生入學許可、課程建立、教師指派。
> 
> 本教學課程會顯示在 C# 範例。 [可下載範例](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)包含 C# 和 Visual Basic 中的程式碼。
> 
> ## <a name="database-first"></a>第一次資料庫
> 
> 有三種方式，您可以使用 Entity Framework 中的資料： *Database First*， *Model First*，並*Code First*。 本教學課程是針對第一個資料庫。 如需如何選擇最適合您案例的差異，這些工作流程和指引的相關資訊，請參閱[Entity Framework 的開發工作流程](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。
> 
> ## <a name="web-forms"></a>Web Form
> 
> 本教學課程系列會使用 ASP.NET Web Form 模型，並假設您知道如何使用 Visual Studio 中的 ASP.NET Web Form。 如果您沒有這麼做，請參閱 < [Getting Started with ASP.NET 4.5 Web Form](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)。 如果您想要使用 ASP.NET MVC 架構，請參閱[開始使用 ASP.NET MVC 的 Entity Framework 使用](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
> 
> ## <a name="software-versions"></a>軟體版本
> 
> | **在本教學課程中所示** | **也可以搭配** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express for Web。 本教學課程尚未經過測試的 Visual Studio 更新版本。 有許多差異功能表選取項目、 對話方塊和範本。 |
> | .NET 4 | .NET 4.5 回溯相容於.NET 4 中，但本教學課程不經過.NET 4.5。 |
> | Entity Framework 4 | 本教學課程尚未經過測試新的 Entity Framework 的版本。 從 Entity Framework 5 開始，使用預設的 EF `DbContext API` ，引進了 EF 4.1。 EntityDataSource 控制項的設計旨在使用`ObjectContext`API。 如需如何使用 EntityDataSource 控制項`DbContext`API，請參閱 <<c2> [ 此部落格文章](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx)。 |
> 
> ## <a name="questions"></a>問題
> 
> 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)，則[Entity Framework 和 LINQ to Entities 論壇](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)，或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>總覽

您將在這些教學課程中建置應用程式是簡單的大學網站。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

使用者可以檢視和更新學生、課程和教師資訊。 幾個您要建立畫面如下所示。

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>建立 Web 應用程式

若要開始本教學課程，請開啟 Visual Studio，然後使用下列方法建立新的 ASP.NET Web 應用程式專案**ASP.NET Web 應用程式**範本：

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

此範本會建立 web 應用程式專案已經包含樣式表和主版頁面：

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

開啟*Site.Master*檔案，並變更"My ASP.NET Application"為"Contoso University"。

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

尋找* 功能表*控制項，名為`NavigationMenu`並取代為下列的標記中，加入您要建立之頁面的功能表項目。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

開啟*Default.aspx*頁面，並變更`Content`控制項，名為`BodyContent`這樣：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

您現在有簡單的首頁上，您將會建立在各種頁面的連結：

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>建立資料庫

這些教學課程中，您將使用 Entity Framework 資料模型設計工具會自動建立的現有資料庫為基礎的資料模型 (通常稱為*資料庫優先*方法)。 未涵蓋在本教學課程系列中的替代方式是手動建立資料模型，然後就會建立資料庫的設計工具產生指令碼 (*模型優先*方法)。

針對本教學課程使用的資料庫優先方法下, 一個步驟是將資料庫加入至網站。 最簡單的方式是先下載會在本教學課程專案。 然後以滑鼠右鍵按一下*應用程式\_資料*資料夾中，選取**加入現有項目**，然後選取*School.mdf*資料庫檔案從下載的專案。

替代方法是遵循的指示[建立 School 範例資料庫](https://msdn.microsoft.com/library/bb399731.aspx)。 無論您下載的資料庫，或建立它，複製*School.mdf*下列資料夾從您的應用程式的檔案*應用程式\_資料*資料夾：

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(此位置 *.mdf*檔案會假設您使用 SQL Server 2008 Express。)

如果您從指令碼建立資料庫，請執行下列步驟來建立資料庫圖表：

1. 中**伺服器總管**，展開**資料連接**，展開*School.mdf*，以滑鼠右鍵按一下**資料庫圖表**，然後選取**加入新的圖表**。

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. 選取所有資料表，然後按一下**新增**。

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server 建立資料庫圖表，顯示資料表、 資料表和資料表之間的關聯性中的資料行。 您可以移動大約以組織這些，不過您要的資料表。
3. 儲存為"SchoolDiagram 」 圖表，並將它關閉。

如果您要下載*School.mdf*檔案，會在本教學課程中，您可以按兩下來檢視資料庫圖表**SchoolDiagram**之下**資料庫圖表**中**伺服器總管**。

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

圖表看起來像這樣 （資料表可能會在不同的位置，從項目如下所示）：

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>建立 Entity Framework 資料模型

現在，您可以從這個資料庫建立 Entity Framework 資料模型。 您可以在應用程式的根資料夾中建立資料模型，但本教學課程中您會將它放在名為的資料夾*DAL* （適用於資料存取層）。

在 **方案總管**，新增名為專案資料夾*DAL* （請確定它不存在於方案的專案之下）。

以滑鼠右鍵按一下*DAL*資料夾，然後選取**新增**並**新項目**。 底下**已安裝的範本**，選取**資料**，選取**ADO.NET 實體資料模型**範本，其命名為*SchoolModel.edmx*，及然後按一下**新增**。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

這會啟動 Entity Data Model 精靈。 在第一個精靈步驟中，**從資料庫產生**預設會選取選項。 按 [ **下一步**]。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

在 [**選擇資料連接**步驟，保留預設值，然後按一下**下一步]**。 預設會選取 School 資料庫，並將連線設定會儲存在*Web.config*檔案作為**SchoolEntities**。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

在 **選擇您的資料庫物件**精靈步驟中，選取所有的資料表除外`sysdiagrams`（這您稍早產生的圖表建立），然後按一下 **完成**。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

完成建立模型之後，Visual Studio 會顯示您對應至資料庫資料表的 Entity Framework 物件 （實體） 的圖形表示。 （如同資料庫圖表中，個別元素的位置可能會不同於您在此圖中所看到的內容。 您可以拖曳大約以符合圖，如果您想要的項目。）

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>探索 Entity Framework 資料模型

您可以看到實體圖表看起來非常類似資料庫圖表中，有幾個差異。 其中一項差異，是加入的符號結尾的每一個關聯，表示關聯型別 （資料表關聯性稱為實體關聯的資料模型中）：

- -0-或-一對一關聯被以"1"和"0..1"。

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    在此情況下，`Person`實體可能會或可能未與相關聯`OfficeAssignment`實體。 `OfficeAssignment`必須與相關聯實體`Person`實體。 換句話說，一名講師可能會或可能不會指派給 office，而任何 office 可以指派給只有一位講師的功能。
- 一對多關聯以"1"和"\*」。

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    在此情況下，`Person`實體可能會或可能沒有相關聯的`StudentGrade`實體。 A`StudentGrade`實體必須與其中一個關聯`Person`實體。 `StudentGrade` 實體實際上代表已註冊的課程，在此資料庫;如果在課程中註冊為學生，而且沒有任何等級的`Grade`屬性為 null。 也就是說，學生可能未註冊任何課程中，可能會註冊在一個課程中，或可以註冊多個課程。 在已註冊的課程中的每一年級適用於只有一個的學生。
- 多對多關聯由 「\*"和"\*」。

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    在此情況下，`Person`實體可能會或可能沒有相關聯`Course`實體，也可以反向也是如此︰`Course`實體可能會或可能沒有相關聯的`Person`實體。 換句話說，講師可教授多個課程，課程可由多個講師進行教授。 （在此資料庫中，此關聯性只適用於講師; 它未連結至課程的學生。 學生會連結到課程 StudentGrades 資料表。）

資料庫圖表] 和 [資料模型的另一個差異是額外**導覽屬性**一節以取得每個實體。 實體的導覽屬性參考相關的實體。 例如，`Courses`中的屬性`Person`實體包含的所有集合`Course`相關的實體`Person`實體。

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

資料庫和資料模型的另一個差異是缺少`CourseInstructor`關聯資料表的資料庫中用來連結`Person`和`Course`多對多關聯性中的資料表。 導覽屬性可讓您取得相關`Course`從實體`Person`實體和相關`Person`實體`Course`實體，這樣就不需要代表資料模型中的關聯資料表。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

基於本教學課程的目的，假設`FirstName`資料行`Person`資料表實際上包含一個人的名字和中間名。 您想要變更以反映此欄位的名稱，但資料庫管理員 (DBA) 可能不想要變更資料庫。 您可以變更名稱`FirstName`在資料模型中，同時讓其資料庫的對等的屬性不變。

在設計師中，以滑鼠右鍵按一下**FirstName**中`Person`實體，，然後選取**重新命名**。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

輸入新名稱"FirstMidName"。 這會變更您的程式碼中的資料行參考而不需要變更資料庫的方式。

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

模型瀏覽器提供另一種方式來檢視資料庫結構、 資料模型結構，以及它們之間的對應。 若要查看它，以滑鼠右鍵按一下 entity designer 中的空白區域，然後按一下 **模型瀏覽器**。

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

**模型瀏覽器**窗格顯示的樹狀檢視。 (**模型瀏覽器**可能會與一起停駐窗格**方案總管 中**窗格中。)**SchoolModel**節點代表資料的模型結構中，而**SchoolModel.Store**節點所表示的資料庫結構。

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

依序展開**SchoolModel.Store**若要查看資料表中，展開**資料表 / 檢視**來查看資料表，然後再展開**課程**查看資料表中的資料行。

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

依序展開**SchoolModel**，展開**實體類型**，然後展開**課程**節點以查看的實體和實體內的屬性。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

在設計工具或**模型瀏覽器**窗格，您可以看到 Entity Framework 如何與相關聯的兩個模型的物件。 以滑鼠右鍵按一下`Person`實體，然後選取**資料表對應**。

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

這會開啟**對應詳細資料**視窗。 請注意，此視窗可讓您看到的資料庫資料行將`FirstName`對應至`FirstMidName`，這是什麼您它重新命名為資料模型中。

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Entity Framework 會使用 XML 來儲存資料庫、 資料模型，以及它們之間的對應相關資訊。 *SchoolModel.edmx*檔案實際上是 XML 檔案，其中包含這項資訊。 設計工具呈現的資訊，以圖形格式，但您也可以檢視為 XML 檔案，以滑鼠右鍵按一下 *.edmx*中的檔案**方案總管**，再按一下**開啟與**，然後選取**XML （文字） 編輯器**。 （資料模型設計工具和 XML 編輯器是只有兩個不同的方式開啟和使用的相同檔案中，因此您不能有設計工具開啟，並在 XML 編輯器中開啟檔案，在相同的時間）。

您現在已建立網站、 資料庫和資料模型。 在下一個逐步解說中，您會開始使用資料模型和 ASP.NET 資料使用`EntityDataSource`控制項。

> [!div class="step-by-step"]
> [下一步](the-entity-framework-and-aspnet-getting-started-part-2.md)
