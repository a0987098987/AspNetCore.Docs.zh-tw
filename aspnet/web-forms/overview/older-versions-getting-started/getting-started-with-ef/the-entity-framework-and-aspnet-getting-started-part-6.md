---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: 開始使用 Entity Framework 4.0 Database 中第一次與 ASP.NET 4 Web Form-第 6 部分 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 的 ASP.NET Web Forms 應用程式。 範例應用程式是...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 1f0a0f050f74b41d6e33a406dfcf7a760c81321b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817430"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>開始使用 Entity Framework 4.0 Database 中第一次和第 6 部分-ASP.NET 4 Web Form
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web Forms 應用程式。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>實作每個階層的資料表繼承

在上一個教學課程中您用過相關的資料加入和刪除關聯性，並可加入新的實體具有現有實體的關聯性。 本教學課程將示範如何在資料模型中實作繼承。

在物件導向程式設計中，您可以使用繼承，讓您輕鬆地使用相關的類別。 例如，您可以建立`Instructor`並`Student`衍生自類別`Person`基底類別。 Entity Framework 中，您可以建立實體之間的繼承結構相同的類型。

在本教學課程的這個部分，您將不會建立任何新的網頁。 相反地，您將新增 衍生的資料模型的實體和修改現有的頁面，以使用新的實體。

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>每個階層的資料表與表 (Table-per-type) 繼承

在一個資料表或多個資料表中，資料庫可以儲存相關物件的相關資訊。 例如，在`School`資料庫，`Person`資料表包含學生和講師單一資料表中的相關資訊。 某些資料行僅適用於講師 (`HireDate`)，有些只適用於學生 (`EnrollmentDate`)，而有些則兩者 (`LastName`， `FirstName`)。

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

您可以設定 Entity Framework 來建立`Instructor`並`Student`實體繼承自`Person`實體。 從單一資料庫資料表產生實體繼承結構的這個模式稱為*每個階層的資料表*(TPH) 繼承。

課程，`School`資料庫使用不同的模式。 線上課程和現場課程會儲存在不同的資料表，每個都有外部索引鍵指向`Course`資料表。 這兩個課程類型通用的資訊會儲存只在`Course`資料表。

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

您可以設定 Entity Framework 資料模型，讓`OnlineCourse`並`OnsiteCourse`實體繼承自`Course`實體。 從不同的資料表，請在每個型別，以及回頭參考資料表，將資料通用於所有類型，儲存每個個別的資料表產生實體繼承結構的這個模式稱為*每個類型的資料表*(TPT) 繼承。

TPH 繼承模式通常會提供更佳的效能比起 TPT 繼承模式，Entity Framework 中因為 TPT 模式可能會導致複雜的聯結查詢。 本逐步解說示範如何實作 TPH 繼承。 您將會先執行下列步驟：

- 建立`Instructor`並`Student`實體類型衍生自`Person`。
- 從衍生的實體有關的移動屬性`Person`至衍生的實體。
- 設定衍生型別中的屬性上的條件約束。
- 請`Person`實體抽象的實體。
- 對應每個衍生實體`Person`資料表的條件來指定如何判斷是否`Person`資料列代表該衍生型別。

## <a name="adding-instructor-and-student-entities"></a>新增的 Instructor 和 Student 實體

開啟<em>SchoolModel.edmx</em>檔案中，以滑鼠右鍵按一下 在設計師中，選取未佔用的區域<strong>新增</strong>，然後選取<strong>實體</strong><em>。</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

在 [**新增實體**] 對話方塊中，實體名稱`Instructor`並設定其**基底型別**選項設定為`Person`。

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

按一下 [確定 **Deploying Office Solutions**]。 設計工具會建立`Instructor`衍生自實體`Person`實體。 新的實體還沒有任何屬性。

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

重複此程序來建立`Student`實體，也是衍生自`Person`。

只有講師有雇用日期，因此您必須將該屬性從`Person`實體`Instructor`實體。 在 `Person`實體，以滑鼠右鍵按一下`HireDate`屬性，然後按一下**剪下**。 然後以滑鼠右鍵按一下**屬性**中`Instructor`實體，然後按一下**貼上**。

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

雇用日期`Instructor`實體不可為 null。 以滑鼠右鍵按一下`HireDate`屬性，按一下**屬性**，然後在**屬性**視窗中變更`Nullable`至`False`。

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

重複程序來移動`EnrollmentDate`屬性從`Person`實體`Student`實體。 請確定您將`Nullable`要`False`如`EnrollmentDate`屬性。

既然`Person`實體具有通用的屬性`Instructor`和`Student`實體 （除了導覽屬性，您未移動），實體只可用來當做基底實體繼承結構中。 因此，您需要確保永遠都不會被視為獨立的實體。 以滑鼠右鍵按一下`Person`實體中，選取**屬性**，然後在**屬性**視窗中的值變更**抽象**屬性設**True**。

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Instructor 和 Student 實體對應至 [人員] 資料表

現在，您需要告訴 Entity Framework 如何區分`Instructor`和`Student`資料庫中的實體。

以滑鼠右鍵按一下`Instructor`實體，然後選取**資料表對應**。 在**對應詳細資料** 視窗中，按一下**新增資料表或檢視表**，然後選取**人員**。

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

按一下 **新增條件**，然後選取**HireDate**。

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

變更**運算子**要**是**並**值 / 屬性**至**Not Null**。

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

重複的程序`Students`實體，指定此實體會對應至`Person`資料表`EnrollmentDate`資料行不是 null。 然後儲存並關閉資料模型。

建置專案，以建立新的實體做為類別，並將其提供在設計工具中。

## <a name="using-the-instructor-and-student-entities"></a>使用的 Instructor 和 Student 實體

當您建立網頁，才能使用 student 和 instructor 資料，您資料的繫結`Person`實體集，和您篩選`HireDate`或`EnrollmentDate`屬性來限制傳回的資料給學生或講師。 不過，現在當您繫結至每個資料來源控制項`Person`實體集，您可以指定只有`Student`或`Instructor`應該選取實體類型。 因為 Entity Framework 知道如何區分學生和講師`Person`實體集，您可以移除`Where`若要這樣做，手動輸入的屬性設定。

在 Visual Studio 設計工具中，您可以指定實體型別`EntityDataSource`控制項應該在中選取**EntityTypeFilter**的下拉式清單方塊`Configure Data Source`精靈 中，如下列範例所示。

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

然後在**屬性**視窗，您可以移除`Where`不再需要如下列範例所示的子句值。

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

不過，因為您已變更的標記`EntityDataSource`控制項，以使用`ContextTypeName`屬性，您無法執行**設定資料來源**精靈上`EntityDataSource`您已建立的控制項。 因此，您會改為變更標記來進行必要的變更。

開啟*Students.aspx*頁面。 在 `StudentsEntityDataSource`控制項，移除`Where`屬性，並新增`EntityTypeFilter="Student"`屬性。 標記現在會類似下列的範例：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

設定`EntityTypeFilter`屬性可確保`EntityDataSource`控制項就會選取指定的實體類型。 如果您想要擷取兩者`Student`和`Instructor`實體類型，您會設定這個屬性。 (您可以擷取與其中的多個實體類型的選擇`EntityDataSource`只有當您使用控制項的唯讀資料存取控制。 如果您使用`EntityDataSource`控制插入、 更新或刪除實體，和如果繫結至實體集可以包含多個類型，您只能使用一個實體類型和您必須設定這個屬性。)

重複的程序`SearchEntityDataSource`控制項，但移除只有部分`Where`屬性，可選取`Student`實體而非完全移除屬性。 控制項的開頭標記現在將會類似下列範例：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

執行頁面，確認它仍可如以前一樣。

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

更新您在先前的教學課程中，使其使用新建立的下列網頁`Student`並`Instructor`而不是實體`Person`實體，然後執行他們確認其如之前一樣運作：

- 在  *StudentsAdd.aspx*，新增`EntityTypeFilter="Student"`到`StudentsEntityDataSource`控制項。 標記現在會類似下列的範例： 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- 在  *about.aspx 的網頁*，新增`EntityTypeFilter="Student"`要`StudentStatisticsEntityDataSource`控制項，並移除`Where="it.EnrollmentDate is not null"`。 標記現在會類似下列的範例： 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- 在*Instructors.aspx*並*InstructorsCourses.aspx*，加入`EntityTypeFilter="Instructor"`來`InstructorsEntityDataSource`控制項，並移除`Where="it.HireDate is not null"`。 中的標記*Instructors.aspx*現在類似下列範例： 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    中的標記*InstructorsCourses.aspx*現在會類似下列的範例：

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

基於這些變更，您已改進在數種方式中的 Contoso 大學應用程式的可維護性。 移動選取項目和驗證邏輯，從 UI 層 (*.aspx*標記)，使得資料存取層中不可或缺的一部分。 這有助於找出您的應用程式程式碼，您可能會對資料庫結構描述或資料模型進行未來的變更。 例如，您可能會決定，學生可能師生輔助僱用，並因此會得到雇用日期。 然後，您可以加入新屬性來區別講師的學生，並更新資料模型。 Web 應用程式中的任何程式碼不必須變更，只有在您想要用來顯示適用於學生的雇用日期。 新增另一個優點`Instructor`並`Student`實體則是您的程式碼更容易理解，比當它稱為`Person`物件的實際學生或講師。

您現在已了解 Entity Framework 中實作的繼承模式一種方式。 在下列教學課程中，您將了解如何使用預存程序，以便更充分掌控 Entity Framework 如何存取資料庫。

> [!div class="step-by-step"]
> [上一頁](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [下一頁](the-entity-framework-and-aspnet-getting-started-part-7.md)
