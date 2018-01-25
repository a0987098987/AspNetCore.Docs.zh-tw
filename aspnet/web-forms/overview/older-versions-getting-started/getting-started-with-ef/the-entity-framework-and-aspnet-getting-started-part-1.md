---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: "開始使用 Entity Framework 4.0 資料庫中第一次與 ASP.NET 4 Web Form |Microsoft 文件"
author: tdykstra
description: "Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web Form 應用程式..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: ae2fddc81f6f4da866ec0719a0e74516bdd2a4d1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>開始使用 Entity Framework 4.0 資料庫中第一次與 ASP.NET 4 Web Form
====================
由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Web Form 應用程式使用 Entity Framework 4.0 和 Visual Studio 2010。 範例應用程式是針對虛構的 Contoso 大學的網站。 其中包括功能，例如許可學生、 課程建立和講師指派。
> 
> 教學課程會示範 C# 中的範例。 [可下載範例](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)包含 C# 和 Visual Basic 中的程式碼。
> 
> ## <a name="database-first"></a>第一次資料庫
> 
> 有三種方式，您可以使用 Entity Framework 中的資料： *Database First*， *Model First*，和*Code First*。 本教學課程適用於第一個資料庫。 如需如何選擇適合您案例的這些工作流程和指引之間差異的詳細資訊，請參閱[Entity Framework 的開發工作流程](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。
> 
> ## <a name="web-forms"></a>Web Form
> 
> 此教學課程系列使用 ASP.NET Web Form 模型，並假設您知道如何使用 Visual Studio 中的 ASP.NET Web Form。 如果您沒有這麼做，請參閱[開始使用 ASP.NET 4.5 Web Form](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)。 如果您想要使用 ASP.NET MVC framework，請參閱[Entity Framework 使用 ASP.NET MVC 使用者入門](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
> 
> ## <a name="software-versions"></a>軟體版本
> 
> | **教學課程中所示** | **也可以搭配** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express for Web. 本教學課程尚未經過測試的 Visual Studio 版本。 有許多差異功能表選取項目、 對話方塊和範本。 |
> | .NET 4 | .NET 4.5 回溯相容於.NET 4 中，但本教學課程不經過.NET 4.5。 |
> | Entity Framework 4 | 本教學課程尚未經過測試新的 Entity Framework 的版本。 從 Entity Framework 5 開始，依預設使用 EF`DbContext API`導入的已 EF 4.1。 EntityDataSource 控制項的設計使用`ObjectContext`應用程式開發介面。 如需如何使用 EntityDataSource 控制項`DbContext`API，請參閱[此部落格文章](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx)。 |
> 
> ## <a name="questions"></a>問題
> 
> 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)、 [Entity Framework 和 LINQ to Entities 論壇](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)，或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>總覽

您會在這些教學課程建置的應用程式是簡單的大學網站。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

使用者可以檢視和更新學生、 課程、 和講師資訊。 以下顯示幾個您要建立的畫面。

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>建立 Web 應用程式

若要開始本教學課程，請開啟 Visual Studio 並再建立新的 ASP.NET Web 應用程式專案使用**ASP.NET Web 應用程式**範本：

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

此範本所建立的 web 應用程式專案已經包含樣式表和主版頁面：

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

開啟*Site.Master*檔案，並且變更"My ASP.NET 應用程式 」 以 「 Contoso 大學"。

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

尋找*功能表*控制項，名為`NavigationMenu`並取代為下列標記加入您將建立之頁面的功能表項目。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

開啟*Default.aspx*頁面並變更`Content`控制項，名為`BodyContent`這樣：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

您現在有簡單的首頁上，您將建立的不同頁面的連結：

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>建立資料庫

這些教學課程中，您將使用 Entity Framework 資料模型設計工具以自動建立資料模型，根據現有的資料庫 (通常稱為*資料庫優先*方法)。 未涵蓋在本教學課程系列的替代方式是以手動方式建立資料模型，然後讓 建立資料庫的設計工具產生指令碼 (*模型優先*方法)。

在本教學課程中使用資料庫第一個方法下, 一個步驟是將資料庫加入至網站。 最簡單的方式是先下載隨附此教學課程專案。 以滑鼠右鍵按一下*應用程式\_資料*資料夾中，選取**加入現有項目**，然後選取*School.mdf*資料庫檔案從下載的專案。

另一個方法是請遵循指示[建立 School 範例資料庫](https://msdn.microsoft.com/library/bb399731.aspx)。 您下載資料庫或建立它時，是否將複製*School.mdf*下列資料夾中的檔案到您的應用程式*應用程式\_資料*資料夾：

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(此位置的*.mdf*檔案會假設您正在使用 SQL Server 2008 Express。)

如果您從指令碼建立資料庫，執行下列步驟來建立資料庫圖表：

1. 在**伺服器總管**，依序展開**資料連接**，展開*School.mdf*，以滑鼠右鍵按一下**資料庫圖表**，並選取**加入新的圖表**。

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. 選取的所有資料表，然後按一下**新增**。

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server 建立資料庫圖表，顯示資料表、 資料表和資料表之間的關聯性中的資料行。 您可以移動大約要將它們組織，不過您喜歡的資料表。
3. 儲存為"SchoolDiagram 」 圖表，並將它關閉。

如果您要下載*School.mdf*檔案隨附此教學課程中，您可以按兩下來檢視資料庫圖表**SchoolDiagram**下**資料庫圖表**中**伺服器總管**。

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

圖表外觀如下所示 （資料表可能會在不同的位置，從這裡所顯示）：

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>建立 Entity Framework 資料模型

現在您可以從這個資料庫建立 Entity Framework 資料模型。 您可以建立資料模型中的應用程式的根資料夾，但本教學課程中您會將它放在名為的資料夾*DAL* （適用於資料存取層）。

在**方案總管 中**，加入名為的專案資料夾*DAL* （請確定在專案中，不是在方案之下）。

以滑鼠右鍵按一下*DAL*資料夾，然後選取**新增**和**新項目**。 在下**已安裝的範本**，選取**資料**，選取**ADO.NET 實體資料模型**範本，其命名*SchoolModel.edmx*，和然後按一下 **新增**。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

啟動實體資料模型精靈。 在第一個精靈步驟中，**從資料庫產生**預設會選取選項。 按 [ **下一步**]。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

在**選擇資料連接**步驟，請保留預設值，然後按一下**下一步**。 預設會選取 School 資料庫，且連接設定會儲存於*Web.config*檔案做為**SchoolEntities**。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

在**選擇您的資料庫物件**精靈步驟中，選取所有的資料表除外`sysdiagrams`（這您先前產生的圖表建立），然後按一下 **完成**。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

它已完成建立模型之後，Visual Studio 會顯示您對應至資料庫資料表的 Entity Framework 物件 （實體） 的圖形表示法。 （如同資料庫圖表中，個別項目的位置可能是您在此圖中所看到的不同。 您可以拖曳大約要比對圖，如果您想要的項目。）

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>瀏覽 Entity Framework 資料模型

您可以看到實體圖表看起來非常類似於資料庫圖表中，具有兩個差異。 其中一種差異是每個關聯的結尾表示關聯類型的符號新增 （資料表關聯性稱為實體關聯的資料模型中）：

- 以 0 或-1 一個關聯是由"1"和"0..1"表示。

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    在此情況下，`Person`實體可能會或可能未與相關聯`OfficeAssignment`實體。 `OfficeAssignment`實體必須與相關聯`Person`實體。 換句話說，講師可能或可能不會指派給 office，而任何 office 可以指派給一個講師的功能。
- 一對多關聯由"1"和"\*"。

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    在此情況下，`Person`實體可能會或可能沒有相關聯的`StudentGrade`實體。 A`StudentGrade`實體必須與一個`Person`實體。 `StudentGrade`實體實際上代表已註冊此資料庫; 中的課程如果您在課程中註冊一位學生，而且沒有任何等級的棒的是，`Grade`屬性為 null。 換句話說，一位學生可能不會註冊任何課程中，可能會在一個課程中，註冊，或可能會在多個課程中註冊。 在已註冊的過程中的每一年級適用於只有一個學生。
- 多對多關聯由 「\*"和"\*"。

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    在此情況下，`Person`實體可能會或可能沒有相關聯的`Course`實體，也可以反向也是如此：`Course`實體可能會或可能沒有相關聯的`Person`實體。 換句話說，講師可能教導多個課程中，並可能由多個講師教課程。 （在此資料庫中，此關聯性只適用於講師; 它未連結學生課程。 學生連結至課程 StudentGrades 資料表。）

資料庫圖表] 和 [資料模型的另一個差異在於額外**導覽屬性**區段的每個實體。 實體的導覽屬性參考的相關的實體。 例如，`Courses`屬性`Person`實體包含所有的集合`Course`的實體有關的`Person`實體。

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

尚未進行，另一個資料庫和資料模型之間的差異是缺少`CourseInstructor`關聯資料表的資料庫中用來連結`Person`和`Course`多對多關聯性中的資料表。 導覽屬性可讓您取得相關`Course`實體`Person`實體與相關`Person`實體`Course`實體，所以不需要代表資料模型中的關聯資料表。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

基於本教學課程的目的，假設`FirstName`資料行`Person`資料表實際上包含個人的名字和中間名。 您想要變更以反映此欄位的名稱，但資料庫管理員 (DBA) 可能不想要變更資料庫。 您可以變更名稱`FirstName`資料模型，但保留對等其資料庫中的屬性不變。

在設計師中，以滑鼠右鍵按一下**FirstName**中`Person`實體，，然後選取**重新命名**。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

輸入 「 FirstMidName"的新名稱。 這樣會變更您的程式碼中的資料行參考而不需要變更資料庫的方式。

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

模型瀏覽器提供另一種方式來檢視資料庫結構、 資料模型的結構，以及它們之間的對應。 看到它，請在 entity designer 中的空白區域上按一下滑鼠右鍵，然後按一下 **模型瀏覽器**。

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

**模型瀏覽器** 窗格會顯示的樹狀檢視。 (**模型瀏覽器**可能具有停駐窗格**方案總管 中**窗格。)**SchoolModel**節點代表資料模型的結構，而**SchoolModel.Store**節點代表資料庫結構。

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

展開**SchoolModel.Store**資料表，請展開**資料表 / 檢視**至資料表，然後展開 **課程**查看資料表中的資料行。

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

展開**SchoolModel**，依序展開**實體類型**，然後展開**課程**節點以查看實體和實體內的屬性。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

在設計工具或**模型瀏覽器**窗格，您可以查看如何 Entity Framework 與相關聯的兩個模型物件。 以滑鼠右鍵按一下`Person`實體，然後選取**資料表對應**。

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

這會開啟**對應詳細資料**視窗。 請注意，此視窗可讓您查看的資料庫資料行`FirstName`對應至`FirstMidName`，這是什麼您它重新命名為資料模型中。

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Entity Framework 會使用 XML 來儲存有關資料庫、 資料模型，以及它們之間的對應資訊。 *SchoolModel.edmx*檔案是實際的 XML 檔案，其中包含這項資訊。 設計工具中呈現的資訊以圖形格式，但您也可以檢視為 XML 檔案，以滑鼠右鍵按一下*.edmx*檔案**方案總管 中**，然後按一下**開啟與**，然後選取**XML （文字） 編輯器**。 （資料模型設計工具和 XML 編輯器是開啟和使用同一個檔案中，因此您不能有開啟，並在 XML 編輯器中開啟檔案，同時在設計工具的兩個不同的方式）。

現在您已建立網站、 資料庫和資料模型。 在下一個逐步解說開始，您將使用資料模型和 ASP.NET 的資料搭配使用`EntityDataSource`控制項。

>[!div class="step-by-step"]
[下一步](the-entity-framework-and-aspnet-getting-started-part-2.md)
