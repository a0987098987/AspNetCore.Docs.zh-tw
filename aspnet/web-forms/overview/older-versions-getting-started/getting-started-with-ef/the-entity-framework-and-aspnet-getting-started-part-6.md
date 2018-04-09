---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: 開始使用 Entity Framework 4.0 資料庫中第一次與 ASP.NET 4 Web Form-第 6 單元 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 的 ASP.NET Web Form 應用程式。 範例應用程式是...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: b76be25501275ba676c9a9acca8e73333439ee70
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>開始使用 Entity Framework 4.0 資料庫中第一次和 ASP.NET 4 Web Form 第 6 單元
====================
由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Web Form 應用程式使用 Entity Framework 4.0 和 Visual Studio 2010。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>實作每個階層的資料表繼承

在上一個教學課程中您在使用相關的資料加入和刪除關聯性並新增至現有的實體的關聯性的實體。 本教學課程將示範如何在資料模型中實作繼承。

在物件導向程式設計中，您可以使用繼承，讓您更輕鬆地使用相關的類別。 例如，您可以建立`Instructor`和`Student`類別衍生自`Person`基底類別。 Entity Framework 中，您可以建立相同的實體之間的繼承結構類型。

在本教學課程的這個部分，您將不會建立任何新的 web 網頁。 相反地，您將加入衍生實體資料模型，並修改現有的頁面，以使用新的實體。

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>資料表每個階層與資料表每個類型的繼承

在一個資料表或多個資料表中，資料庫可以儲存相關物件的相關資訊。 例如，在`School`資料庫`Person`資料表包含學生和講師單一資料表中的相關資訊。 某些資料行僅適用於講師 (`HireDate`)，有些則只能學生 (`EnrollmentDate`)，而有些則兩者 (`LastName`， `FirstName`)。

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

您可以設定 Entity Framework 建立`Instructor`和`Student`實體繼承自`Person`實體。 這種模式的單一資料庫資料表產生實體繼承結構會呼叫*資料表每個階層*(TPH) 繼承。

課程，`School`資料庫使用不同的模式。 線上課程和現場課程會儲存在不同的資料表，每一個都有外部索引鍵指向`Course`資料表。 只有在儲存資訊通用於這兩個課程類型`Course`資料表。

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

您可以設定 Entity Framework 資料模型，讓`OnlineCourse`和`OnsiteCourse`實體都繼承自`Course`實體。 此模式，從不同的資料表，請在每個型別，以及回頭參考資料表來儲存資料通用於所有類型，每個個別的資料表中產生實體繼承結構會呼叫*資料表每個型別*(TPT) 繼承。

因為 TPT 模式可能會導致複雜的聯結查詢 TPH 繼承模式通常 TPT 繼承的模式，比在 Entity framework 提供更佳的效能。 本逐步解說示範如何實作 TPH 繼承。 您將會執行，藉由執行下列步驟：

- 建立`Instructor`和`Student`實體類型衍生自`Person`。
- 從衍生的實體有關的移動屬性`Person`至衍生的實體。
- 在衍生類型中設定屬性的條件約束。
- 請`Person`實體抽象的實體。
- 對應每個衍生實體以`Person`的條件來指定如何判斷資料表是否`Person`資料列代表該衍生型別。

## <a name="adding-instructor-and-student-entities"></a>加入講師和學生實體

開啟<em>SchoolModel.edmx</em>檔案中，以滑鼠右鍵按一下未佔用的區域，在設計師中，選取<strong>新增</strong>，然後選取<strong>實體</strong><em>。</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

在**加入實體**對話方塊中，實體名稱`Instructor`並設定其**基底型別**選項設定為`Person`。

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

按一下 [確定 **Deploying Office Solutions**]。 在設計工具建立`Instructor`衍生自實體`Person`實體。 新的實體還沒有任何屬性。

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

重複此程序來建立`Student`實體也衍生自`Person`。

只有講師有雇用日期，因此您必須將該屬性從`Person`實體`Instructor`實體。 在`Person`實體，以滑鼠右鍵按一下`HireDate`屬性，然後按一下**剪下**。 以滑鼠右鍵按一下**屬性**中`Instructor`實體，然後按一下**貼上**。

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

雇用日期`Instructor`實體不能是 null。 以滑鼠右鍵按一下`HireDate`屬性中，按一下**屬性**，然後在**屬性**視窗變更`Nullable`至`False`。

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

重複程序來移動`EnrollmentDate`屬性從`Person`實體`Student`實體。 請確定也設`Nullable`至`False`如`EnrollmentDate`屬性。

既然`Person`實體具有通用的屬性`Instructor`和`Student`實體 （除了導覽屬性，您未移動），實體只可用來當作基底實體的繼承結構中。 因此，您需要確保它永遠不會視為獨立的實體。 以滑鼠右鍵按一下`Person`實體中，選取**屬性**，然後在**屬性**視窗變更的值**抽象**屬性**True**。

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>講師和學生的實體對應至 Person 資料表

現在您要如何區分告知 Entity Framework`Instructor`和`Student`資料庫中的實體。

以滑鼠右鍵按一下`Instructor`實體，然後選取**資料表對應**。 在**對應詳細資料**視窗中，按一下 **新增資料表或檢視表**選取**人員**。

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

按一下**加入條件**，然後選取**HireDate**。

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

變更**運算子**至**是**和**值 / 屬性**至**Not Null**。

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

重複的程序`Students`實體，指定此實體，對應至`Person`資料表`EnrollmentDate`資料行不是 null。 然後儲存並關閉 資料模型。

建置專案，才能建立新的實體做為類別，並讓它們可在設計工具中。

## <a name="using-the-instructor-and-student-entities"></a>使用講師和學生實體

當您建立網頁，即可使用 student 與講師資料，您在資料繫結`Person`實體集，且您篩選上`HireDate`或`EnrollmentDate`學生或講師限制傳回的資料屬性。 不過，現在當您繫結到每個資料來源控制項`Person`實體集，您可以只指定`Student`或`Instructor`應該選取實體類型。 因為 Entity Framework 知道如何區分學生版和講師中的`Person`實體集，您可以移除`Where`手動輸入執行此作業的屬性設定。

在 Visual Studio 設計工具中，您可以指定的實體類型的`EntityDataSource`控制項應該在中選取**EntityTypeFilter**的下拉式清單方塊`Configure Data Source`精靈中，如下列範例所示。

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

然後在**屬性**視窗，您可以移除`Where`不再需要如下列範例所示的子句值。

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

不過，因為您已變更的標記`EntityDataSource`控制項，以使用`ContextTypeName`屬性，您無法執行**設定資料來源**精靈`EntityDataSource`您已經建立的控制項。 因此，您要變更標記改為進行必要的變更。

開啟*Students.aspx*頁面。 在`StudentsEntityDataSource`控制項，移除`Where`屬性，並新增`EntityTypeFilter="Student"`屬性。 標記現在會類似下列的範例：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

設定`EntityTypeFilter`屬性可確保`EntityDataSource`控制項就會選取指定的實體類型。 如果您想要擷取`Student`和`Instructor`實體類型，您就不應設定此屬性。 (您可以選擇擷取多個實體類型，其中包含一個`EntityDataSource`控制只有當您使用控制項的唯讀資料存取。 如果您使用`EntityDataSource`控制插入、 更新或刪除實體，以及如果繫結至實體集可以包含多個型別，您只能使用一個實體類型，您必須將此屬性。)

重複的程序`SearchEntityDataSource`控制項，除了移除中的只有部分`Where`選取的屬性`Student`實體而不是完全移除屬性。 控制項的開頭標記現在會類似下列的範例：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

執行頁面以確認它仍可運作與以前一樣。

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

更新您在先前的教學課程中，使其使用新建立的下列頁面`Student`和`Instructor`實體而不是`Person`實體，然後執行驗證之前一樣運作：

- 在*StudentsAdd.aspx*，新增`EntityTypeFilter="Student"`至`StudentsEntityDataSource`控制項。 標記現在會類似下列的範例： 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- 在*about.aspx 的網頁*，新增`EntityTypeFilter="Student"`至`StudentStatisticsEntityDataSource`控制，並除去`Where="it.EnrollmentDate is not null"`。 標記現在會類似下列的範例： 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- 在*Instructors.aspx*和*InstructorsCourses.aspx*，新增`EntityTypeFilter="Instructor"`至`InstructorsEntityDataSource`控制，並除去`Where="it.HireDate is not null"`。 中的標記*Instructors.aspx*就會類似下列的範例如下： 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    中的標記*InstructorsCourses.aspx*現在會類似下列的範例：

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

這些變更，因為您已改進 Contoso 大學應用程式的可維護性數種方式。 您已移動的選取和驗證邏輯排除在 UI 層外 (*.aspx*標記)，以及變更的資料存取層中不可或缺的一部分。 這有助於找出您的應用程式程式碼，您可能會對未來的資料庫結構描述或資料模型的變更。 例如，您可能會決定，學生可能老師輔助僱用，並因此得到雇用日期。 然後，您無法加入新的屬性來區別講師學生版和更新資料模型。 Web 應用程式中的程式碼不需要變更，只有在您要顯示學生版雇用日期。 加入另一個優點`Instructor`和`Student`實體是您的程式碼更容易理解比當它稱為`Person`物件的實際學生或講師。

您現在已經瞭解如何實作 Entity Framework 中的繼承模式。 在下列的教學課程中，您將學習如何使用預存程序，以便更充分掌控 Entity Framework 存取之資料庫的方式。

> [!div class="step-by-step"]
> [上一頁](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [下一頁](the-entity-framework-and-aspnet-getting-started-part-7.md)
