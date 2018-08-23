---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: ASP.NET MVC 應用程式 (4 之 10) 建立更複雜的資料模型 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: cfb01742c3921c24c71fd3fa4a14a9f71fac1ac1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831131"
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>ASP.NET MVC 應用程式 (4 之 10) 建立更複雜的資料模型
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載已完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載入門專案，如本章](building-the-ef5-mvc4-chapter-downloads.md)並從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，請[下載已完成的一章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現您的問題。 您通常可以找到問題的解決方案，藉由比較您的程式碼的完整程式碼。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在上一個教學課程中您用過三個實體所組成的簡單資料模型。 在本教學課程中，您將新增更多的實體和關聯性，並將指定格式、 驗證和資料庫對應規則來自訂資料模型。 您會看到來自訂資料模型的兩種方式： 藉由將屬性加入至實體類別和程式碼加入至資料庫內容類別。

當您完成時，實體類別會構成如下列圖例中所顯示的完整資料模型：

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>使用屬性自訂資料模型

在本節中，您會了解到如何使用指定格式、驗證和資料庫對應規則的屬性來自訂資料模型。 然後在下列各節的數個您要建立完整`School`加上的資料模型屬性類別已建立並建立新的類別，針對模型中剩餘的實體類型。

### <a name="the-datatype-attribute"></a>DataType 屬性

針對學生的註冊日期，所有網頁目前都會同時顯示時間和日期，即使您針對此欄位只需要日期而已。 使用資料註解屬性，您可以透過僅對一個程式碼進行變更，來修正每個顯示資料的檢視上的顯示格式。 為了查看如何進行此操作的範例，您將會新增一個屬性至 `Student` 類別中的 `EnrollmentDate` 屬性。

在*Models\Student.cs*，新增`using`陳述式`System.ComponentModel.DataAnnotations`命名空間，並新增`DataType`並`DisplayFormat`屬性加入`EnrollmentDate`屬性，如下列範例所示：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性用來指定比資料庫內建類型更特定的資料類型。 在此案例中，我們只想要追蹤日期，而非日期和時間。 [DataType 列舉](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)提供許多資料類型，例如*日期、 時間、 PhoneNumber、 Currency、 EmailAddress*和更多功能。 `DataType` 屬性也可讓應用程式自動提供類型的特定功能。 比方說，`mailto:`可以為建立連結[DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)，並可以針對提供的日期選擇器[DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)支援的瀏覽器[HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性會發出 HTML 5[資料集](http://ejohn.org/blog/html-5-data-attributes/)(phishing 英文發音如*data dash*) HTML 5 瀏覽器可以了解的屬性。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性不提供任何驗證。

`DataType.Date` 未指定顯示日期的格式。 根據預設，server 為基礎的預設格式顯示資料欄位[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)。

`DisplayFormat` 屬性用來明確指定日期格式：


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode`設定可讓您指定，而指定的格式應該也會套用時的值會顯示在文字方塊中進行編輯。 (您可能不想要的某些欄位 — 比方說，例如貨幣值，您可能不想在文字方塊中的貨幣符號進行編輯。)

您可以使用[displayformat 無法](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性，但它通常是個不錯的主意，若要使用[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)也屬性。 `DataType`屬性會傳達*語意*的資料做為相對於如何呈現在螢幕上，並提供下列優點，就無法取得與`DisplayFormat`:

- 瀏覽器可以啟用 HTML5 功能 （例如顯示日曆控制項、 適合地區設定的貨幣符號、 電子郵件連結、 等等。）。
- 根據預設，瀏覽器會呈現資料使用正確的格式，根據您[地區設定](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)。
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性可以讓 MVC 選擇正確的欄位範本呈現的資料 ( [displayformat 無法](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)如果由本身會使用字串範本)。 如需詳細資訊，請參閱 Brad Wilson [ASP.NET MVC 2 範本](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)。 （針對 MVC 2 所撰寫，但這份文件仍適用於目前版本的 ASP.NET MVC。）

如果您使用`DataType`屬性與 [日期] 欄位中，您必須指定`DisplayFormat`也為了確保欄位在 Chrome 瀏覽器中正確轉譯的屬性。 如需詳細資訊，請參閱 <<c0> [ 這個 StackOverflow 執行緒](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie)。

再次執行 學生的 索引 頁面，並請注意，時間都不會再顯示註冊日期欄位。 也會使用任何檢視，則為 true`Student`模型。

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

您也可以指定資料驗證規則和使用屬性的訊息。 假設您想要確保使用者不會在名稱中輸入超過 50 個字元。 若要新增這項限制，請新增[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性加入`LastName`和`FirstMidName`屬性，如下列範例所示：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性不會防止使用者名稱中輸入空白字元。 您可以使用[RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)將限制套用至輸入的屬性。 例如下列程式碼所需要的第一個字元必須是大寫，而其餘字元必須是英文字母：

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx)屬性會提供類似的功能[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性，但並不提供用戶端驗證。

執行應用程式，然後按一下**學生** 索引標籤。您會收到下列錯誤：

*備份 'SchoolContext' 內容的模型已變更，因為所建立的資料庫。請考慮使用 Code First 移轉更新資料庫 ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269))。*

資料庫模型已變更，需要變更資料庫結構描述中，Entity Framework 偵測到。 您將使用移轉，而不會遺失任何您使用 UI 新增到資料庫的資料更新結構描述。 如果您變更所建立的資料`Seed`方法，將會變更回其原始狀態，因為[AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)您在中使用的方法`Seed`方法。 ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)相當於"upsert"作業從資料庫詞彙中。)

請在套件管理員主控台 (PMC) 中輸入下列命令：

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration MaxLengthOnNames`命令會建立名為的檔案*&lt;時間戳記&gt;\_MaxLengthOnNames.cs*。 此檔案包含將會更新資料庫，以符合目前的資料模型的程式碼。 Entity Framework 會使用移轉檔案名稱的前面加上時間戳記來排序移轉。 如果您卸除資料庫，建立多個移轉之後，或您使用移轉，部署專案，所有移轉會套用所建立的順序。

執行**建立**頁面，然後輸入超過 50 個字元名稱。 只要您超過 50 個字元，用戶端驗證就會立即顯示一則錯誤訊息。

![用戶端端 val 錯誤](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>資料行屬性

您也可以使用屬性控制您的類別和屬性對應到資料庫的方式。 假設您已針對名字欄位使用 `FirstMidName` 作為名稱，因為欄位中可能也會包含中間名。 但您想要將資料庫資料行命名為 `FirstName`，因為撰寫臨機操作查詢資料庫的使用者比較習慣該名稱。 若要進行此對應，您可以使用 `Column` 屬性。

`Column` 屬性指定當建立資料庫時，`Student` 資料表中對應到 `FirstMidName` 屬性的資料行會命名為 `FirstName`。 換句話說，當您的程式碼參照 `Student.FirstMidName` 時，資料便會來自 `Student` 資料表中的 `FirstName` 資料行或在其中更新。 如果您未指定資料行名稱，就會提供相同的名稱與屬性名稱。

加入 using 陳述式[System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx)以及資料行的 name 屬性，以`FirstMidName`屬性，如下列醒目提示的程式碼所示：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

新增[資料行屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)變更備份 SchoolContext，因此它不會符合資料庫的模型。 在 PMC 中建立另一個移轉，請輸入下列命令：

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

在 **伺服器總管**(**資料庫總管**如果您使用 Express for Web)，按兩下*學生*資料表。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

您套用前兩個移轉之前下, 圖顯示原始的資料行名稱。 除了從變更資料行名稱`FirstMidName`要`FirstName`，兩個名稱的資料行已從`MAX`長度為 50 個字元。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

您也可以進行資料庫使用的對應變更[Fluent API](https://msdn.microsoft.com/data/jj591617)，如本教學課程稍後，您會看到。

> [!NOTE]
> 如果您嘗試將編譯您完成建立所有這些實體類別之前，您可能會收到編譯器錯誤。


## <a name="create-the-instructor-entity"></a>建立 Instructor 實體

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

建立*Models\Instructor.cs*，以下列程式碼取代範本程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

請注意，當中有幾個屬性跟 `Student` 和 `Instructor` 實體中的一樣。 在 [實作繼承](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)稍後在本系列教學課程中的，您將重構以消除多餘的結果使用繼承。

### <a name="the-required-and-display-attributes"></a>必要，並顯示屬性

上的屬性`LastName`屬性會指定它是必要的欄位，文字方塊的標題應該為 「 姓氏 」 （而不是屬性名稱，這會是"LastName"不含空格），以及值不能超過 50 個字元。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

[StringLength 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)設定資料庫中的最大長度，並提供用戶端和伺服器端 ASP.NET mvc 的驗證。 您也可以在此屬性中指定最小字串長度，但最小值不會對資料庫結構描述造成任何影響。 [必要的屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)不需要使用實值型別，例如 DateTime、 int、 double、 和 float。 實值型別不能指定 null 值，因此它們都是原本就是必要。 您也可以移除[必要的屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)，並使用的最小長度參數取代`StringLength`屬性：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

您可以將多個屬性放在同一行，因此您也可以如下撰寫 instructor 類別：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>FullName 計算屬性

`FullName` 為一個計算屬性，會傳回藉由串連兩個其他屬性而建立的值。 因此只有`get`存取子，且沒有`FullName`會產生資料庫中的資料行。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>課程和 OfficeAssignment 導覽屬性

`Courses` 和 `OfficeAssignment` 屬性為導覽屬性。 如已先前所述，它們通常會定義為[虛擬](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx)，讓他們可以利用稱為 Entity Framework 功能[消極式載入](https://msdn.microsoft.com/magazine/hh205756.aspx)。 此外，如果導覽屬性可以保存多個實體，其類型必須實作[ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx)介面。 (例如[IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx)而非限定[IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx)因為`IEnumerable<T>`不實作[新增](https://msdn.microsoft.com/library/63ywd54z.aspx).

講師可以教授任何數量的課程，因此`Courses`定義為集合`Course`實體。 我們的商務規則狀態講師只可以有最多一個辦公室，因此`OfficeAssignment`定義為單一`OfficeAssignment`實體 (這可能是`null`如果被不指派任何辦公室)。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>建立 OfficeAssignment 實體

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

建立*Models\OfficeAssignment.cs*為下列程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

建置專案時，會將儲存您的變更，並確認您未進行任何複製並貼上，編譯器可以攔截的錯誤。

### <a name="the-key-attribute"></a>索引鍵屬性

-0-或-一對一關聯性之間`Instructor`而`OfficeAssignment`實體。 辦公室指派只存在於，它指派的講師，因此其主索引鍵也是其外部索引鍵`Instructor`實體。 但 Entity Framework 無法自動辨識`InstructorID`做為主要索引鍵的實體因為其名稱並未遵循`ID`或*classname* `ID`命名慣例。 因此，必須使用 `Key` 屬性將其識別為 PK：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

您也可以使用`Key`屬性，若實體確實有它自己的主索引鍵，但您想要命名為不同名稱的屬性`classnameID`或`ID`。 根據預設 EF 會作為非資料庫產生將索引鍵，因為資料行是用於識別關聯性。

### <a name="the-foreignkey-attribute"></a>ForeignKey 屬性

-0-或-一對一關聯性或兩個實體之間的一對一關聯性時 (這類之間`OfficeAssignment`和`Instructor`)，EF 就出關聯性的一端是主體和相依的結束，即無法運作。 一對一關聯性會在每個類別與其他類別中的參考導覽屬性。 [ForeignKey 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)可以套用至相依的類別，以建立關聯性。 如果您省略[ForeignKey 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)，當您嘗試建立移轉作業時，收到下列錯誤：

無法判斷 'ContosoUniversity.Models.OfficeAssignment' 和 'ContosoUniversity.Models.Instructor' 類型間之關聯的主要末端。 此關聯的主體端點必須明確地設定使用的關聯性的 fluent API 」 或 「 資料註解。

稍後在本教學課程中，我們將示範如何使用 fluent API 設定此關聯性。

### <a name="the-instructor-navigation-property"></a>Instructor 導覽屬性

`Instructor`實體具有可為 null`OfficeAssignment`導覽屬性 （因為一名講師可能沒有辦公室指派），而`OfficeAssignment`實體有不可為 null`Instructor`導覽屬性 （因為辦公室指派無法講師-之外存在`InstructorID`不可為 null)。 當`Instructor`實體有相關`OfficeAssignment`實體，每個實體會有另一個在其導覽屬性中的參考。

您可以將放入`[Required]`上 Instructor 導覽屬性，指定必須有一個相關的講師，但您不需要這麼做，因為 InstructorID 外部索引鍵 （這也是這個資料表的索引鍵） 是不可為 null 的屬性。

## <a name="modify-the-course-entity"></a>修改 Course 實體

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

在  *Models\Course.cs*，稍早為下列程式碼取代您所新增的程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Course 實體具有外部索引鍵屬性`DepartmentID`指向相關`Department`實體，且具有`Department`導覽屬性。 當您針對相關實體具有一個導覽屬性時，Entity Framework 便不需要您為資料模型新增一個外部索引鍵屬性。 在需要時會，EF 會將外部索引鍵會自動建立資料庫中。 但在資料模型中擁有外部索引鍵，可讓更新變得更為簡單和有效率。 例如，當擷取課程實體，若要編輯，時才`Department`實體為 null 如果您沒有載入它，因此當您更新課程實體，您必須先擷取`Department`實體。 當外部索引鍵的屬性`DepartmentID`是否包含在資料模型中，您不需要擷取`Department`才能更新的實體。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 屬性

[DatabaseGenerated 屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx)具有[無](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx)參數`CourseID`屬性指定主索引鍵值是使用者所提供，而非資料庫產生的。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

根據預設，Entity Framework 會假設主索引鍵值由資料庫產生。 這是您在大多數案例下所希望的情況。 不過，對於`Course`實體，您會使用使用者指定的課程號碼 1000年系列的一個部門，另一個部門，使用 2000年系列等等。

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵及導覽屬性

外部索引鍵屬性和導覽屬性在`Course`實體反映了下列關聯性：

- 課程會指派給一個部門，因此基於上述理由，會有一個 `DepartmentID` 外部索引鍵和一個 `Department` 導覽屬性。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- 由於課程可由任何數量的學生進行註冊，因此 `Enrollments` 導覽屬性為一個集合： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- 課程可由多個講師進行教授，因此 `Instructors` 導覽屬性為一個集合： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>建立 Department 實體

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

建立*Models\Department.cs*為下列程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>資料行屬性

您使用稍早[資料行屬性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)來變更資料行名稱對應。 在程式碼`Department`實體，`Column`屬性用來變更 SQL 資料類型對應，以便使用 SQL Server 就會定義資料行[money](https://msdn.microsoft.com/library/ms179882.aspx)資料庫中的型別：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

資料行對應通常是不必要的因為 Entity Framework 通常會選擇根據您定義屬性的 CLR 類型的適當 SQL Server 資料類型。 CLR `decimal` 類型會對應到 SQL Server 的 `decimal` 類型。 但在此情況下您知道資料行，將會儲存貨幣數量，而[金錢](https://msdn.microsoft.com/library/ms179882.aspx)資料類型是最適合的。

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵及導覽屬性

外部索引鍵及導覽屬性反映了下列關聯性：

- 部門可以有或沒有一位系統管理員，而系統管理員一律為講師。 因此`InstructorID`屬性是包含做為外部索引鍵`Instructor`後面會加上實體，以及使用問號`int`類型標示為可為 null 的該屬性指定。導覽屬性名為`Administrator`但保留`Instructor`實體： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- 部門中可能包含許多課程，因此沒有`Courses`導覽屬性： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > 根據慣例，Entity Framework 會為不可為 Null 的外部索引鍵和多對多關聯性啟用串聯刪除。 這導致循環串聯刪除規則，您的初始設定式程式碼執行時，會導致例外狀況。 例如，如果您未定義`Department.InstructorID`屬性可為 null，初始設定式執行時，會收到下列例外狀況訊息: 「 參考關聯性將會導致不允許循環參考。 」 如果您的商務規則所需`InstructorID`成不可為 null 的屬性，您必須使用下列 fluent API 來停用串聯刪除關聯性： 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modifying-the-student-entity"></a>修改 Student 實體

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

在  *Models\Student.cs*，稍早為下列程式碼取代您所新增的程式碼。 所做的變更已醒目提示。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>Enrollment 實體

 在  *Models\Enrollment.cs*，稍早為下列程式碼取代您所新增的程式碼

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵及導覽屬性

外部索引鍵屬性及導覽屬性反映了下列關聯性：

- 註冊記錄乃針對單一課程，因此當中包含了一個 `CourseID` 外部索引鍵屬性及一個 `Course` 導覽屬性： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- 註冊記錄乃針對單一學生，因此當中包含了一個 `StudentID` 外部索引鍵屬性及一個 `Student` 導覽屬性： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>多對多關聯性

之間多對多關聯性`Student`和`Course`實體，而`Enrollment`實體的功能為多對多聯結資料表*承載*資料庫中。 這表示`Enrollment`資料表包含額外的資料，除了聯結資料表的外部索引鍵 (在此情況下，主索引鍵和`Grade`屬性)。

下列圖例展示了在實體圖表中這些關聯性的樣子。 (此圖表使用來產生[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); 建立圖表不屬於本教學課程，只是正在使用它作為。)

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

每個關聯性線條的一端，星號 1 (\*)，顯示其表示一個對多關聯性。

如果`Enrollment`資料表並未包含成績資訊，其便只需要包含兩個外部索引鍵`CourseID`和`StudentID`。 在此情況下，它就會對應至多對多聯結資料表*不具有承載*(或*純聯結資料表*) 資料庫中，您不需要完全為其建立的模型類別。 `Instructor`和`Course`實體具有此類型的多對多關聯性，並如您所見，它們之間沒有實體類別：

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

聯結資料表，才在資料庫中，不過，在下列的資料庫圖表所示：

![Instructor-Course_many-to-many_relationship_tables](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

Entity Framework 會自動建立`CourseInstructor`資料表，以及您讀取和更新的讀取和更新的間接`Instructor.Courses`和`Course.Instructors`導覽屬性。

## <a name="entity-diagram-showing-relationships"></a>顯示關聯性的實體圖表

下列圖例顯示了 Entity Framework Power Tools 為完成的 School 模型建立的圖表。

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

除了多對多關聯性線條 (\*來\*) 和一對多關聯性線條 (1 到\*)，您可以看到以下到零或-一一關聯性線條 (1 對 0..1) 之間`Instructor`和`OfficeAssignment`實體和零-或--一對多關聯性線條 (0..1 對\*) 之間的 Instructor 和 Department 實體。

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>程式碼加入至資料庫的內容來自訂資料模型

接下來您要在其中加入新的實體來`SchoolContext`類別，並自訂對應使用的一些[fluent API](https://msdn.microsoft.com/data/jj591617)呼叫。 （API 是"fluent"的因為它通常由串連在一起成為一個單一的陳述式的方法呼叫的一系列函式）。

在本教學課程中，您將使用 fluent API 僅針對您無法使用屬性操作的資料庫對應。 然而，您也可以使用 Fluent API 指定大部分透過屬性可完成的格式、驗證及對應規則。 某些屬性 (例如 `MinimumLength`) 無法使用 Fluent API 來套用。 如前所述，`MinimumLength`不會變更結構描述，它只適用於用戶端和伺服器端驗證規則

某些開發人員偏好單獨使用 Fluent API，使其實體類別保持「整潔」。 若想要的話，您也可以混合使用屬性和 Fluent API，並且有一些自訂項目只能使用 Fluent API 完成。但一般來說，建議的做法是在這兩種方法中選擇其中一項，然後盡可能的一致使用該方法。

若要加入新的實體資料模型，並執行您未使用屬性的資料庫對應，取代中的程式碼*DAL\SchoolContext.cs*為下列程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

在新的陳述式[OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)方法設定多對多聯結資料表：

- 多對多關聯性之間`Instructor`和`Course`實體，程式碼指定聯結資料表的資料表和資料行名稱。 程式碼第一次可以設定多對多關聯性，而不需要這個程式碼，但如果您未呼叫它，就會預設名稱這類`InstructorInstructorID`針對`InstructorID`資料行。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

下列程式碼提供如何您也可以使用 fluent API 而非屬性來指定之間的關聯性的範例`Instructor`和`OfficeAssignment`實體：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

如需 「 fluent API 」 陳述式在幕後的執行資訊，請參閱[Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx)部落格文章。

## <a name="seed-the-database-with-test-data"></a>使用測試資料植入資料庫

中的程式碼取代*Migrations\Configuration.cs*檔案取代下列程式碼，以針對您已建立的新實體提供種子資料。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

如您所見第一個教學課程中，大部分的這段程式碼只是更新或建立新的實體物件並載入範例資料所需的測試的屬性。 不過請注意如何`Course`具有多對多關聯性的實體與`Instructor`實體，會處理：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

當您建立`Course`物件，初始化`Instructors`導覽屬性，以空的集合，使用程式碼`Instructors = new List<Instructor>()`。 這讓您能夠新增`Instructor`與此相關的實體`Course`使用`Instructors.Add`方法。 如果您未建立空的清單，您便無法將這些關聯性，因為`Instructors`屬性會是 null，而且不會有`Add`方法。 您也可以加入清單初始化建構函式。

## <a name="add-a-migration-and-update-the-database"></a>新增移轉並更新資料庫

在 PMC 中，輸入`add-migration`命令：

`PM> add-Migration Chap4`

如果您嘗試在此時更新資料庫時，您會得到下列錯誤：

*ALTER TABLE 陳述式與 FOREIGN KEY 條件約束 」 FK\_dbo。課程\_dbo。部門\_DepartmentID"。衝突發生在資料庫"ContosoUniversity"，資料表"dbo。部門 」，資料行 'DepartmentID'。*

編輯&lt;*時間戳記&gt;\_Chap4.cs*檔案，然後進行下列程式碼變更 (您會將 SQL 陳述式加入和修改`AddColumn`陳述式):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(請確定您標記為註解或刪除現有`AddColumn`行時加入新的連線，或當您輸入時，您會收到錯誤`update-database`命令。)

有時當您執行使用現有的資料移轉時，您需要將 stub 資料插入資料庫中以滿足外部索引鍵條件約束，，而且這是您的所作所為現在。 產生的程式碼會新增一個不可為 null`DepartmentID`外部索引鍵`Course`資料表。 如果已有資料列`Course`資料表時執行的程式碼，`AddColumn`作業會失敗，因為 SQL Server 並不知道所要放入不可為 null 的資料行的值。 因此，您已變更預設值，提供新的資料行的程式碼，而且您已建立名為"Temp"作為預設部門的 stub 部門。 如此一來，如果已有`Course`資料列時執行此程式碼，它們將所有與"Temp"部門。

當`Seed`方法執行時，它會插入資料列`Department`資料表也會與現有`Course`這些新的資料列`Department`資料列。 如果您尚未新增任何課程在 UI 中，然後您不再需要"Temp"部門或預設值在`Course.DepartmentID`資料行。 若要允許，有人可能已經加入課程所使用的應用程式的可能性，也要更新`Seed`方法的程式碼，以確保所有`Course`資料列 (而不只是由先前執行插入`Seed`方法) 具有有效`DepartmentID`之前先移除預設的值從資料行值，並刪除"Temp"部門。

當您完成編輯之後&lt;*時間戳記&gt;\_Chap4.cs*檔案中，輸入`update-database`在 PMC 中執行移轉命令。

> [!NOTE]
> 可以移轉資料和制定的結構描述變更時，取得其他錯誤。 如果您收到無法解決的移轉錯誤，您可以變更中的連接字串*Web.config*檔案，或刪除的資料庫。 簡單的方法是在資料庫重新命名*Web.config*檔案。 例如，將資料庫名稱變更為 CU\_測試下列所示：
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
>  使用新資料庫時，沒有資料移轉，而`update-database`命令是很有可能能順利完成。 如需有關如何刪除資料庫的指示，請參閱 <<c0> [ 如何從 Visual Studio 2012 中卸除資料庫](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。


開啟中的資料庫**伺服器總管**當您稍早，並展開**資料表**節點以查看所有的資料表已建立的。 (如果您仍有**伺服器總管**開啟從稍早的時間，再按**重新整理** 按鈕。)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

您未建立的模型類別`CourseInstructor`資料表。 如先前所述，這是一個聯結資料表之間的多對多關聯性`Instructor`和`Course`實體。

以滑鼠右鍵按一下`CourseInstructor`資料表，然後選取**顯示資料表資料**以確認它的資料中的`Instructor`實體新增至`Course.Instructors`導覽屬性。

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>總結

您現在已有了更複雜的資料模型和對應的資料庫。 在下列教學課程，您將深入了解不同的方式可存取相關的資料。

其他 Entity Framework 資源連結可在[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一頁](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
