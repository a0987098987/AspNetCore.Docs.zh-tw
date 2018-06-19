---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: ASP.NET MVC 應用程式建立更複雜的資料模型 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 5 應用程式使用 Entity Framework 6 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: fd8bf6502b0dd261505a86a2ed86d4c3f42e8755
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877095"
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application"></a>ASP.NET MVC 應用程式建立更複雜的資料模型
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在先前的教學課程中您使用過的簡單資料模型的三個實體所組成。 在本教學課程，您可以加入多個實體和關聯性，您將自訂資料模型，藉由指定的格式、 驗證和資料庫對應規則。 您會看到兩種方式可以自訂資料模型： 藉由加入屬性至實體類別，並將程式碼加入至資料庫內容類別。

當您完成時，實體類別會構成如下列圖例中所顯示的完整資料模型：

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>使用屬性自訂資料模型

在本節中，您會了解到如何使用指定格式、驗證和資料庫對應規則的屬性來自訂資料模型。 然後會在下列章節的幾個您建立完整`School`加入資料模型屬性類別已建立並建立新的類別，在模型中剩餘的實體類型。

### <a name="the-datatype-attribute"></a>資料類型屬性

針對學生的註冊日期，所有網頁目前都會同時顯示時間和日期，即使您針對此欄位只需要日期而已。 使用資料註解屬性，您可以透過僅對一個程式碼進行變更，來修正每個顯示資料的檢視上的顯示格式。 為了查看如何進行此操作的範例，您將會新增一個屬性至 `Student` 類別中的 `EnrollmentDate` 屬性。

在*Models\Student.cs*，新增`using`陳述式`System.ComponentModel.DataAnnotations`命名空間和新增`DataType`和`DisplayFormat`屬性至`EnrollmentDate`屬性，如下列範例所示：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性用來指定資料庫內建型別比更特定的資料類型。 在此案例中，我們只想要追蹤日期，而非日期和時間。 [DataType 列舉](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)提供許多資料類型，例如*日期、 時間、 電話號碼、 貨幣、 EmailAddress*等等。 `DataType` 屬性也可讓應用程式自動提供類型的特定功能。 比方說，`mailto:`可建立連結的[DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)，而且可以提供日期選擇器[DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)支援的瀏覽器[HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性發出 HTML 5[資料-](http://ejohn.org/blog/html-5-data-attributes/) (phishing 英文發音如*資料虛線*) 能夠辨識的 html5 瀏覽器的屬性。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性不提供任何驗證。

`DataType.Date` 未指定顯示日期的格式。 根據預設，資料欄位會顯示根據基礎在伺服器上的預設格式[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)。

`DisplayFormat` 屬性用來明確指定日期格式：


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode`設定指定指定的格式應該也套用時的值會顯示在文字方塊中進行編輯。 (您可能不想要的某些欄位，例如，貨幣值，您可能不想在文字方塊中的貨幣符號進行編輯。)

您可以使用[DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性本身，但它通常是最好使用[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)也屬性。 `DataType`屬性傳達*語意*的資料做為相對於如何呈現在畫面上，並提供下列優點，就無法取得與`DisplayFormat`:

- 瀏覽器可以啟用 HTML5 功能 (例如顯示日曆控制項、適合地區設定的貨幣符號、電子郵件連結、一些用戶端輸入驗證等等)。
- 根據預設，瀏覽器將會轉譯資料使用正確的格式，根據您[地區設定](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)。
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性可以讓 MVC 選擇正確的欄位範本轉譯資料 ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)使用字串的範本)。 如需詳細資訊，請參閱 Brad Wilson 的[ASP.NET MVC 2 範本](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)。 （針對 MVC 2 所撰寫，不過這篇文章仍適用於目前版本的 ASP.NET MVC。）

如果您使用`DataType`屬性使用 [日期] 欄位中，您必須指定`DisplayFormat`屬性也以確保欄位在 Chrome 瀏覽器中正確轉譯。 如需詳細資訊，請參閱[這個 StackOverflow 執行緒](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie)。

如需如何處理在 MVC 中的其他日期格式的詳細資訊，請移至[MVC 5 簡介： 檢查編輯方法與編輯檢視](../introduction/examining-the-edit-methods-and-edit-view.md)和搜尋的頁面中&quot;國際化&quot;。

再次執行學生索引頁，並請注意，時間不會再顯示註冊日期。 會使用任何檢視，則為 true 的相同`Student`模型。

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

您也可以使用屬性指定資料驗證規則和驗證錯誤訊息。 [StringLength 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)設定資料庫中的最大長度，並提供用戶端和伺服器端的 ASP.NET MVC 驗證。 您也可以在此屬性中指定最小字串長度，但最小值不會對資料庫結構描述造成任何影響。

假設您想要確保使用者不會在名稱中輸入超過 50 個字元。 若要加入這項限制，加入[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性至`LastName`和`FirstMidName`屬性，如下列範例所示：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性將不會防止使用者輸入名稱的泛空白字元。 您可以使用[RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)將限制套用至輸入的屬性。 例如下列程式碼需要第一個字元是大寫，而其餘字元是依字母順序排列：

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx)屬性會提供類似的功能[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性，但不會提供用戶端驗證。

執行應用程式，然後按一下**學生** 索引標籤。您會收到下列錯誤：

*備份 'SchoolContext' 內容的模型已變更，因為所建立的資料庫。請考慮使用 Code First 移轉來更新資料庫 ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269))。*

資料庫模型已變更，則需要在資料庫結構描述變更的方式和 Entity Framework 偵測到。 您將使用移轉，而不會遺失任何資料，使用 UI 新增到資料庫中更新結構描述。 如果您變更資料所建立的`Seed`方法，將會變更回其原始狀態，因為[AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)中使用的方法`Seed`方法。 ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)等於從資料庫詞彙中的 「 更新插入 」 作業。)

請在套件管理員主控台 (PMC) 中輸入下列命令：

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration`命令會建立名為*&lt;時間戳記&gt;\_MaxLengthOnNames.cs*。 此檔案包含了 `Up` 方法中的程式碼，可更新資料庫，使其符合目前的資料模型。 `update-database` 命令執行了該程式碼。

移轉檔案名稱的前面加上時間戳記 Entity framework 用於排序的移轉。 您可以建立多個移轉之前執行`update-database`命令，然後再移轉的所有會套用已建立的順序。

執行**建立**頁面，然後輸入任一名稱長度超過 50 個字元。 當您按一下 [建立] 時，用戶端驗證便會顯示錯誤訊息。

![用戶端 val 錯誤](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>資料行屬性

您也可以使用屬性控制您的類別和屬性對應到資料庫的方式。 假設您已針對名字欄位使用 `FirstMidName` 作為名稱，因為欄位中可能也會包含中間名。 但您想要將資料庫資料行命名為 `FirstName`，因為撰寫臨機操作查詢資料庫的使用者比較習慣該名稱。 若要進行此對應，您可以使用 `Column` 屬性。

`Column` 屬性指定當建立資料庫時，`Student` 資料表中對應到 `FirstMidName` 屬性的資料行會命名為 `FirstName`。 換句話說，當您的程式碼參照 `Student.FirstMidName` 時，資料便會來自 `Student` 資料表中的 `FirstName` 資料行或在其中更新。 如果您未指定資料行名稱，會收到相同的名稱與屬性名稱。

在*Student.cs* file、 add`using`陳述式[System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx)並加入至資料行名稱屬性`FirstMidName`屬性，如下所示下列反白顯示的程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

新增[資料行屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)變更備份 SchoolContext，因此它將不會符合資料庫的模型。 若要建立其他移轉 PMC 中輸入下列命令：

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

在**伺服器總管**，開啟*學生*資料表設計工具，按兩下*學生*資料表。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

下圖顯示原始的資料行名稱，因為它已套用的前兩個移轉作業之前。 除了從變更的資料行名稱`FirstMidName`至`FirstName`，兩個名稱的資料行已從`MAX`長度為 50 個字元。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

您也可以變更資料庫對應使用[Fluent API](https://msdn.microsoft.com/data/jj591617)，稍後在本教學課程，您會看到如下。

> [!NOTE]
> 若您嘗試在完成建立下列章節中所有的實體類別前進行編譯，您可能會收到編譯器錯誤。


## <a name="complete-changes-to-the-student-entity"></a>完成學生實體的變更

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

在*Models\Student.cs*，稍早為下列程式碼取代您所加入的程式碼。 所做的變更已醒目提示。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>必要的屬性

[必要的屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)讓名稱屬性的必要的欄位。 `Required attribute`不需要使用實值類型，例如 DateTime、 int、 double、 和 float。 因此原本就視為必要的欄位，實值類型無法指派 null 值。 您也可以移除[必要的屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)並取代的最小長度參數`StringLength`屬性：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

### <a name="the-display-attribute"></a>顯示屬性

`Display` 屬性指定了文字方塊的標題應為「名字」、「姓氏」、「全名」及「註冊日期」，而非每個執行個體中的屬性名稱 (沒有使用空格鍵分隔單字的名稱)。

### <a name="the-fullname-calculated-property"></a>FullName 計算內容

`FullName` 為一個計算屬性，會傳回藉由串連兩個其他屬性而建立的值。 因此只有`get`存取子，且沒有`FullName`會在資料庫中產生資料行。

## <a name="create-the-instructor-entity"></a>建立 Instructor 實體

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

建立*Models\Instructor.cs*，範本程式碼取代為下列程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

請注意，當中有幾個屬性跟 `Student` 和 `Instructor` 實體中的一樣。 在本系列稍後的[實作繼承](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)教學課程中，您會對此程式碼進行重構以消除冗餘。

您也可以如下所示撰寫講師類別，您可以在同一行，讓多個屬性：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>課程和 OfficeAssignment 導覽屬性

`Courses` 和 `OfficeAssignment` 屬性為導覽屬性。 已稍早所述，它們通常會定義為[虛擬](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx)，讓他們可以利用稱為 Entity Framework 功能[消極式載入](https://msdn.microsoft.com/magazine/hh205756.aspx)。 此外，如果導覽屬性都可以保存多個實體，其類型必須實作[ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx)介面。 例如[IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx)但不是會限定[IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx)因為`IEnumerable<T>`不會實作[新增](https://msdn.microsoft.com/library/63ywd54z.aspx).

講師可以教導任意數目的課程，因此`Courses`做為集合定義`Course`實體。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

我們的商務規則狀態講師只能使用最多一個辦事處的資料，因此`OfficeAssignment`定義為單一`OfficeAssignment`實體 (它可能是`null`如果被不指派任何 office)。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>建立 OfficeAssignment 實體

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

建立*Models\OfficeAssignment.cs*為下列程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

建置專案時，會將儲存您的變更，並確認您還未做任何複製和貼上，編譯器可以攔截的錯誤。

### <a name="the-key-attribute"></a>索引鍵屬性

以 0 或-1 一個之間沒有關聯性`Instructor`和`OfficeAssignment`實體。 Office 指派相對於講師指派，才會存在，因此其主索引鍵也是其外部索引鍵`Instructor`實體。 Entity Framework 無法自動辨識，但是`InstructorID`為主要索引鍵此實體的因為其名稱未遵循`ID`或*classname* `ID`命名慣例。 因此，必須使用 `Key` 屬性將其識別為 PK：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

您也可以使用`Key`屬性如果實體沒有自己的主索引鍵，但您想要指定不同的名稱屬性`classnameID`或`ID`。 根據預設 EF 視為金鑰非產生資料庫因為資料行是識別的關聯性。

### <a name="the-foreignkey-attribute"></a>ForeignKey 屬性

有一對零-或-一關聯性或兩個實體之間的一對一關聯性時 (這類之間`OfficeAssignment`和`Instructor`)，EF 不算出關聯性的一端是主體，而且相依的結尾。 一對一關聯性為其他類別的每個類別中有參考導覽屬性。 [ForeignKey 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)可以套用至相依的類別，以建立關聯性。 如果您省略[ForeignKey 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)，當您嘗試建立移轉作業時，收到下列錯誤：

*無法判定類型 'ContosoUniversity.Models.OfficeAssignment' 和 'ContosoUniversity.Models.Instructor' 之間的關聯的主要端點。此關聯的主要端點必須使用明確設定關聯性 fluent 應用程式開發介面或資料註解。*

稍後在教學課程中，您會看到如何使用來設定此關聯性 fluent 應用程式開發的應用程式開發介面。

### <a name="the-instructor-navigation-property"></a>講師導覽屬性

`Instructor`實體有允許 null`OfficeAssignment`導覽屬性 （因為講師可能沒有 office 指派），而`OfficeAssignment`實體具有不可為 null`Instructor`導覽屬性 （因為無法 office 指派沒有講師-存在`InstructorID`不可為 null)。 當`Instructor`實體具有相關`OfficeAssignment`實體，每個實體會有另一個在其導覽屬性的參考。

您無法將`[Required]`講師導覽屬性，指定必須有相關的講師，但您不需要這樣做，因為 InstructorID 外部索引鍵 （這也是這個資料表的索引鍵） 是不可為 null 的屬性。

## <a name="modify-the-course-entity"></a>修改 Course 實體

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

在*Models\Course.cs*，稍早為下列程式碼取代您所加入的程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

課程實體具有外部索引鍵屬性`DepartmentID`指向相關`Department`實體，且具有`Department`導覽屬性。 當您針對相關實體具有一個導覽屬性時，Entity Framework 便不需要您為資料模型新增一個外部索引鍵屬性。 在需要時會 EF 會自動在資料庫中建立外部索引鍵。 但在資料模型中擁有外部索引鍵，可讓更新變得更為簡單和有效率。 例如，當您擷取課程實體，若要編輯，`Department`實體為 null 如果您不載入它，因此當您更新課程實體中，您必須先擷取`Department`實體。 當外部索引鍵屬性`DepartmentID`是否包含在資料模型，您不需要擷取`Department`實體之後才更新。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 屬性

[DatabaseGenerated 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx)與[無](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx)參數`CourseID`屬性指定主索引鍵值是使用者所提供，而不是不是資料庫產生。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

根據預設，Entity Framework 會假設資料庫會產生主索引鍵值。 這是您在大多數案例下所希望的情況。 不過，對於`Course`實體，您會使用使用者指定課程數字，例如 1000年一連串一個部門，另一個部門，2000年數列等等。

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵和導覽屬性

外部索引鍵屬性和導覽屬性中的`Course`實體反映的下列關聯性：

- 課程會指派給一個部門，因此基於上述理由，會有一個 `DepartmentID` 外部索引鍵和一個 `Department` 導覽屬性。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- 由於課程可由任何數量的學生進行註冊，因此 `Enrollments` 導覽屬性為一個集合： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- 課程可由多個講師進行教授，因此 `Instructors` 導覽屬性為一個集合： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>建立 Department 實體

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

建立*Models\Department.cs*為下列程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>資料行屬性

您所用的稍早[資料行屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)若要變更資料行名稱的對應。 在程式碼`Department`實體，`Column`屬性用來變更 SQL 資料類型對應，以便將會定義資料行，使用 SQL Server [money](https://msdn.microsoft.com/library/ms179882.aspx)資料庫中的類型：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

資料行對應通常是不必要的因為 Entity Framework 通常會選擇根據您定義屬性的 CLR 類型的適當 SQL Server 資料類型。 CLR `decimal` 類型會對應到 SQL Server 的 `decimal` 類型。 但在此情況下您知道貨幣金額將會保留資料行和[money](https://msdn.microsoft.com/library/ms179882.aspx)資料類型是更適合的。 如需有關 CLR 資料型別和 SQL Server 資料型別如何比對的詳細資訊，請參閱[適用於 Entity Framework 的 SqlClient](https://msdn.microsoft.com/library/bb896344.aspx)。

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵和導覽屬性

外部索引鍵及導覽屬性反映了下列關聯性：

- 部門可以有或沒有一位系統管理員，而系統管理員一律為講師。 因此`InstructorID`屬性是包含外部索引鍵為`Instructor`後面會加上的實體以及問號`int`輸入指定將標示為可為 null 的屬性。導覽屬性名為`Administrator`但保留`Instructor`實體： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- 部門可能有許多課程，所以`Courses`導覽屬性： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > 根據慣例，Entity Framework 會為不可為 Null 的外部索引鍵和多對多關聯性啟用串聯刪除。 這可能會導致循環串聯刪除規則，並在您嘗試新增移轉時造成例外狀況。 例如，如果您未定義`Department.InstructorID`為可為 null 的屬性，您會收到下列例外狀況訊息: 「 參考關聯性將會導致不允許循環參考。 」 如果您的商務規則所需`InstructorID`屬性成為不可為 null，您必須使用下列的 fluent API 陳述式來停用串聯刪除關聯性上： 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modify-the-enrollment-entity"></a>修改註冊實體

![Enrollment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

 在*Models\Enrollment.cs*，稍早為下列程式碼取代您所加入的程式碼

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵和導覽屬性

外部索引鍵屬性及導覽屬性反映了下列關聯性：

- 註冊記錄乃針對單一課程，因此當中包含了一個 `CourseID` 外部索引鍵屬性及一個 `Course` 導覽屬性： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- 註冊記錄乃針對單一學生，因此當中包含了一個 `StudentID` 外部索引鍵屬性及一個 `Student` 導覽屬性： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>多對多關聯性

多對多關聯性之間`Student`和`Course`實體，而`Enrollment`實體做為多對多聯結資料表*裝載*資料庫中。 這表示`Enrollment`資料表包含額外的資料以外的聯結資料表的外部索引鍵 (在這個情況下，主索引鍵和`Grade`屬性)。

下列圖例展示了在實體圖表中這些關聯性的樣子。 (此圖表使用產生的[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); 建立圖表不屬於本教學課程，正只為一個實例。)

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

每個關聯性線條的一端和星號 1 (\*)，在指出的一對多關聯性。

如果`Enrollment`資料表未包含等級資訊，它只需要包含兩個外部索引鍵`CourseID`和`StudentID`。 在此情況下，它就會對應至多對多聯結資料表*不裝載*(或*純聯結資料表*) 在資料庫中，以及您不會有完全為它建立模型類別。 `Instructor`和`Course`實體具有多對多關聯性，該類型，如您所見，它們之間沒有任何實體類別：

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

聯結資料表所需要的是資料庫，不過，如下列的資料庫圖表中所示：

![Instructor-Course_many-to-many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Entity Framework 會自動建立`CourseInstructor`資料表，以及您讀取和更新的讀取和更新的間接`Instructor.Courses`和`Course.Instructors`導覽屬性。

## <a name="entity-diagram-showing-relationships"></a>顯示關聯性的實體圖表

下列圖例顯示了 Entity Framework Power Tools 為完成的 School 模型建立的圖表。

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

除了多對多關聯性線條 (\*至\*) 和一個對多關聯性線條 (1 到\*)，這裡可以看到的以 0 或-1 一個關聯線 (1 對 0..1) 之間`Instructor`和`OfficeAssignment`實體和 0-或--一對多關聯性線條 (0..1 對\*) 講師和 Department 實體之間。

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>將程式碼加入至資料庫內容中自訂資料模型

接下來您要加入新的實體來`SchoolContext`類別和自訂的對應使用某些[fluent API](https://msdn.microsoft.com/data/jj591617)呼叫。 應用程式開發介面是"fluent 應用程式開發，因為它通常由串連一系列的方法呼叫一起成單一陳述式，如下列範例所示：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

在本教學課程中，您將使用 fluent API 僅適用於資料庫對應時，您不能與屬性。 然而，您也可以使用 Fluent API 指定大部分透過屬性可完成的格式、驗證及對應規則。 某些屬性 (例如 `MinimumLength`) 無法使用 Fluent API 來套用。 如前所述，`MinimumLength`並不會變更結構描述中，它只適用於用戶端和伺服器端驗證規則

某些開發人員偏好單獨使用 Fluent API，使其實體類別保持「整潔」。 若想要的話，您也可以混合使用屬性和 Fluent API，並且有一些自訂項目只能使用 Fluent API 完成。但一般來說，建議的做法是在這兩種方法中選擇其中一項，然後盡可能的一致使用該方法。

若要加入新的實體資料模型，並執行您未使用屬性做的資料庫對應，取代中的程式碼*DAL\SchoolContext.cs*為下列程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

新的陳述式中[OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)方法，會設定 「 多對多聯結資料表：

- 之間的多對多關聯性`Instructor`和`Course`實體，程式碼指定聯結資料表的資料表和資料行名稱。 程式碼第一次可以設定多對多關聯性為您沒有這個程式碼，但如果在未呼叫它，就會預設名稱例如`InstructorInstructorID`如`InstructorID`資料行。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

下列程式碼提供如何您也可以使用 fluent 應用程式開發的應用程式開發介面而非屬性來指定之間的關聯性的範例`Instructor`和`OfficeAssignment`實體：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

如需 「 fluent API 」 陳述式在幕後的執行資訊，請參閱[Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx)部落格文章。

## <a name="seed-the-database-with-test-data"></a>使用測試資料植入資料庫

中的程式碼取代*Migrations\Configuration.cs*檔案取代下列程式碼，以針對您已建立新的實體提供種子資料。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

如同第一個教學課程中，大部分的這段程式碼只會更新或建立新的實體物件，載入所需的測試內容中的範例資料。 不過，請注意如何`Course`具有多對多關聯性的實體與`Instructor`實體，會處理：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

當您建立`Course`物件，您初始化`Instructors`導覽屬性為空集合，使用程式碼`Instructors = new List<Instructor>()`。 這樣做可新增`Instructor`至此相關實體`Course`使用`Instructors.Add`方法。 如果您未建立空的清單，您就可以新增這些關聯性，因為`Instructors`屬性會是 null，而且不會有`Add`方法。 您也可以加入清單初始化建構函式。

## <a name="add-a-migration-and-update-the-database"></a>新增的移轉，並更新資料庫

從 PMC 輸入`add-migration`命令 (不會做`update-database`尚未命令):

`add-Migration ComplexDataModel`

若您嘗試在這個時間點執行 `update-database` 命令，您會接收到下列錯誤：

*與外部索引鍵條件約束的 ALTER TABLE 陳述式衝突 」 FK\_dbo。課程\_dbo。部門\_DepartmentID"。衝突發生在資料庫"ContosoUniversity"，資料表"dbo。部門 」，資料行 'DepartmentID'。*

有時當您執行移轉以現有的資料，您需要 stub 資料插入資料庫以符合外部索引鍵條件約束，而且，您必須立即執行的作業。 產生的程式碼中 ComplexDataModel`Up`方法會將不可為 null`DepartmentID`外部索引鍵`Course`資料表。 因為已經有資料列中的`Course`資料表時執行的程式碼，`AddColumn`作業將會失敗，因為 SQL Server 並不知道何值，將置於不可為 null 的資料行。 因此必須變更為新的資料行提供預設值，並建立名為"Temp"做為預設部門 stub 部門的程式碼。 如此一來，現有`Course`相關資料列，將所有會"Temp"部門之後`Up`方法執行。 您可以將它們產生關聯到正確的部門中`Seed`方法。

編輯&lt;*時間戳記&gt;\_ComplexDataModel.cs*檔標記為註解加入 Course 資料表中，[DepartmentID] 欄的程式碼行並加入下列反白顯示程式碼 （加上註解線條會也反白顯示）：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

當`Seed`方法執行時，它會插入資料列中的`Department`資料表和它將與現有`Course`這些新的資料列`Department`資料列。 如果您尚未加入任何課程 UI 中，您然後不再需要"Temp"部門或預設值在`Course.DepartmentID`資料行。 若要允許的可能性，有人可能已經加入課程使用的應用程式，您也要更新`Seed`方法程式碼，請確認所有`Course`資料列 (不只是由先前執行的插入`Seed`方法) 有有效`DepartmentID`之前移除預設的值從資料行值，並刪除"Temp"部門。

當您完成編輯之後&lt;*時間戳記&gt;\_ComplexDataModel.cs*檔案中，輸入`update-database`執行移轉 PMC 命令。

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> 很可能在移轉資料和進行結構描述變更時，取得其他錯誤。 如果收到無法解決的移轉錯誤，您可以變更連接字串中的資料庫名稱，或刪除該資料庫。 最簡單的方法是在資料庫重新命名*Web.config*檔案。 下列範例顯示名稱變更為 CU\_測試：
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
> 
> 使用新資料庫時，沒有資料移轉，而`update-database`命令是更有可能能順利完成。 如需有關如何刪除資料庫的指示，請參閱[如何卸除資料庫，從 Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。
> 
> 如果失敗，另一個您可以嘗試的動作是在 PMC 中輸入下列命令以重新初始化資料庫：
> 
> `update-database -TargetMigration:0`


開啟中的資料庫**伺服器總管**像之前，並展開**資料表**節點以查看所有資料表具有已建立。 (如果仍有**伺服器總管**稍早的時間從開啟中，按一下**重新整理** 按鈕。)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

您未建立的模型類別`CourseInstructor`資料表。 如前所述，這是一個聯結資料表之間的多對多關聯性`Instructor`和`Course`實體。

以滑鼠右鍵按一下`CourseInstructor`資料表，然後選取**顯示資料表資料**驗證中它可能是有資料`Instructor`實體加入至`Course.Instructors`導覽屬性。

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="summary"></a>總結

您現在已有了更複雜的資料模型和對應的資料庫。 下列教學課程中您會深入了解不同的方式來存取相關的資料。

請在您所喜歡本教學課程的方式，我們可以改進留下意見反應。 您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一頁](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
