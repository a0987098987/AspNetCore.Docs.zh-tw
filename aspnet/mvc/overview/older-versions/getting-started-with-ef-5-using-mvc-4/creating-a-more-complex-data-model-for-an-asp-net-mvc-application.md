---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: "ASP.NET MVC 應用程式 (10-4) 中建立更複雜的資料模型 |Microsoft 文件"
author: tdykstra
description: "Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 350c2e4e92c8a53d22dd2500330281b4003a05e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>ASP.NET MVC 應用程式 (10-4) 中建立更複雜的資料模型
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio 2012。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載本章節的入門專案](building-the-ef5-mvc4-chapter-downloads.md)和從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，[下載已完成的章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現問題。 您通常可以藉由比較您的程式碼完成的程式碼會發現問題的解決方案。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在先前的教學課程中您使用過的簡單資料模型的三個實體所組成。 在本教學課程，您可以加入多個實體和關聯性，您將自訂資料模型，藉由指定的格式、 驗證和資料庫對應規則。 您會看到兩種方式可以自訂資料模型： 藉由加入屬性至實體類別，並將程式碼加入至資料庫內容類別。

當您完成時，實體類別會構成完成的資料模型，如下圖所示：

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>自訂資料模型使用屬性

本節中，您會看到如何自訂資料模型使用指定的格式、 驗證和資料庫對應規則的屬性。 然後會在下列章節的幾個您建立完整`School`加入資料模型屬性類別已建立並建立新的類別，在模型中剩餘的實體類型。

### <a name="the-datatype-attribute"></a>資料類型屬性

學生註冊日期，所有的網頁目前顯示的時間和日期，不過關心這個欄位是日期。 使用資料註解屬性，您可以進行一將修復的顯示格式來顯示資料的每個檢視中變更程式碼。 若要查看如何執行作業，您會加入至屬性的範例`EnrollmentDate`屬性`Student`類別。

在*Models\Student.cs*，新增`using`陳述式`System.ComponentModel.DataAnnotations`命名空間和新增`DataType`和`DisplayFormat`屬性至`EnrollmentDate`屬性，如下列範例所示：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

[DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性用來指定資料庫內建型別比更特定的資料類型。 在此情況下我們只想要追蹤的日期，不的日期和時間。 [DataType 列舉](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx)提供許多資料類型，例如*日期、 時間、 電話號碼、 貨幣、 EmailAddress*等等。 `DataType` 屬性也可讓應用程式自動提供類型的特定功能。 比方說，`mailto:`可建立連結的[DataType.EmailAddress](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx)，而且可以提供日期選擇器[DataType.Date](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx)支援的瀏覽器[HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性發出 HTML 5[資料-](http://ejohn.org/blog/html-5-data-attributes/) (phishing 英文發音如*資料虛線*) 能夠辨識的 html5 瀏覽器的屬性。 [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性不提供任何驗證。

`DataType.Date` 未指定顯示日期的格式。 根據預設，資料欄位會顯示根據基礎在伺服器上的預設格式[CultureInfo](https://msdn.microsoft.com/en-us/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)。

`DisplayFormat` 屬性用來明確指定日期格式：


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode`設定指定指定的格式應該也套用時的值會顯示在文字方塊中進行編輯。 (您可能不想要的某些欄位，例如，貨幣值，您可能不想在文字方塊中的貨幣符號進行編輯。)

您可以使用[DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性本身，但它通常是最好使用[DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx)也屬性。 `DataType`屬性傳達*語意*的資料做為相對於如何呈現在畫面上，並提供下列優點，就無法取得與`DisplayFormat`:

- 瀏覽器可以啟用 HTML5 功能 （例如顯示日曆控制項、 地區設定適當的貨幣符號、 電子郵件連結等等）。
- 根據預設，瀏覽器將會轉譯資料使用正確的格式，根據您[地區設定](https://msdn.microsoft.com/en-us/library/vstudio/wyzd2bce.aspx)。
- [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性可以讓 MVC 選擇正確的欄位範本轉譯資料 ( [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx)如果由本身使用字串的範本)。 如需詳細資訊，請參閱 Brad Wilson 的[ASP.NET MVC 2 範本](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)。 （針對 MVC 2 所撰寫，不過這篇文章仍適用於目前版本的 ASP.NET MVC。）

如果您使用`DataType`屬性使用 [日期] 欄位中，您必須指定`DisplayFormat`屬性也以確保欄位在 Chrome 瀏覽器中正確轉譯。 如需詳細資訊，請參閱[這個 StackOverflow 執行緒](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie)。

再次執行學生索引頁，並請注意，時間不會再顯示註冊日期。 會使用任何檢視，則為 true 的相同`Student`模型。

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

您也可以指定資料的驗證規則和使用屬性的訊息。 假設您想要確保使用者不要輸入超過 50 個字元的名稱。 若要加入這項限制，加入[StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性至`LastName`和`FirstMidName`屬性，如下列範例所示：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性將不會防止使用者輸入名稱的泛空白字元。 您可以使用[RegularExpression](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)將限制套用至輸入的屬性。 例如下列程式碼需要第一個字元是大寫，而其餘字元是依字母順序排列：

`[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]`

[MaxLength](https://msdn.microsoft.com/en-us/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx)屬性會提供類似的功能[StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性，但不會提供用戶端驗證。

執行應用程式，然後按一下**學生** 索引標籤。您會收到下列錯誤：

*備份 'SchoolContext' 內容的模型已變更，因為所建立的資料庫。請考慮使用 Code First 移轉來更新資料庫 ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269))。*

資料庫模型已變更，則需要在資料庫結構描述變更的方式和 Entity Framework 偵測到。 您將使用移轉，而不會遺失任何資料，使用 UI 新增到資料庫中更新結構描述。 如果您變更資料所建立的`Seed`方法，將會變更回其原始狀態，因為[AddOrUpdate](https://msdn.microsoft.com/en-us/library/hh846520(v=vs.103).aspx)中使用的方法`Seed`方法。 ([AddOrUpdate](https://msdn.microsoft.com/en-us/library/hh846520(v=vs.103).aspx)等於從資料庫詞彙中的 「 更新插入 」 作業。)

在 封裝管理員主控台 (PMC)，請輸入下列命令：

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration MaxLengthOnNames`命令會建立名為*&lt;時間戳記&gt;\_MaxLengthOnNames.cs*。 此檔案包含程式碼將會更新資料庫，以符合目前的資料模型。 移轉檔案名稱的前面加上時間戳記 Entity framework 用於排序的移轉。 如果您卸除資料庫，建立多個的移轉之後，或您使用移轉，部署專案，所有移轉會套用在所建立的順序。

執行**建立**頁面，然後輸入任一名稱長度超過 50 個字元。 一旦超過 50 個字元，用戶端驗證立即顯示錯誤訊息。

![用戶端 val 錯誤](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>資料行屬性

您也可以使用屬性來控制您的類別和屬性如何對應到資料庫。 假設您使用名稱`FirstMidName`，第一個名稱欄位，因為欄位也可能包含中間名。 但您想要命名的資料庫欄`FirstName`，因為使用者將會寫入資料庫的臨機操作查詢習慣於該名稱。 若要讓此對應，您可以使用`Column`屬性。

`Column`屬性指定當建立資料庫時，資料行`Student`對應到資料表`FirstMidName`屬性會被命名為`FirstName`。 換句話說，當您的程式碼是指`Student.FirstMidName`，資料將來自，或在更新`FirstName`資料行`Student`資料表。 如果您未指定資料行名稱，會收到相同的名稱與屬性名稱。

加入 using 陳述式[System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.aspx)和資料行名稱屬性來`FirstMidName`屬性，如下列反白顯示的程式碼所示：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

新增[資料行屬性](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)變更備份 SchoolContext，因此它將不會符合資料庫的模型。 若要建立其他移轉 PMC 中輸入下列命令：

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

在**伺服器總管**(**資料庫總管**如果您使用 Express for Web)，按兩下*學生*資料表。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

下圖顯示原始的資料行名稱，因為它已套用的前兩個移轉作業之前。 除了從變更的資料行名稱`FirstMidName`至`FirstName`，兩個名稱的資料行已從`MAX`長度為 50 個字元。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

您也可以變更資料庫對應使用[Fluent API](https://msdn.microsoft.com/en-us/data/jj591617)，稍後在本教學課程，您會看到如下。

> [!NOTE]
> 如果您嘗試編譯，才能完成建立所有的這些實體類別，您可能會收到編譯器錯誤。


## <a name="create-the-instructor-entity"></a>建立 [Instructor] 實體

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

建立*Models\Instructor.cs*，範本程式碼取代為下列程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

請注意一些屬性，在相同`Student`和`Instructor`實體。 在[實作繼承](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)稍後在本系列的教學課程，您將會重構使用繼承，若要消除這種冗餘。

### <a name="the-required-and-display-attributes"></a>所需，並顯示屬性

上的屬性`LastName`屬性指定它是必要的欄位，在文字方塊中的標題，應該是 「 姓氏 」 （而不是屬性名稱，不含空格的方式是 「 姓氏 」），和值不能超過 50 個字元。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

[StringLength 屬性](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)設定資料庫中的最大長度，並提供用戶端和伺服器端的 ASP.NET MVC 驗證。 您也可以指定最小字串長度，此屬性，但資料庫結構描述的最小值沒有任何影響。 [必要的屬性](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx)不需要使用實值類型，例如 DateTime、 int、 double、 和 float。 因此它們都是原本就是必要，實值類型無法指派 null 值。 您也可以移除[必要的屬性](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx)並取代的最小長度參數`StringLength`屬性：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

您也可以如下所示撰寫講師類別，您可以在同一行，讓多個屬性：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>FullName 計算內容

`FullName`是傳回值，由串連兩個其他屬性的導出的屬性。 因此只有`get`存取子，且沒有`FullName`會在資料庫中產生資料行。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>課程和 OfficeAssignment 導覽屬性

`Courses`和`OfficeAssignment`屬性為導覽屬性。 已稍早所述，它們通常會定義為[虛擬](https://msdn.microsoft.com/en-us/library/9fkccyh4(v=vs.110).aspx)，讓他們可以利用稱為 Entity Framework 功能[消極式載入](https://msdn.microsoft.com/en-us/magazine/hh205756.aspx)。 此外，如果導覽屬性都可以保存多個實體，其類型必須實作[ICollection&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/92t2ye13.aspx)介面。 (例如[IList&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/5y536ey6.aspx)但不是會限定[IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/9eekhta0.aspx)因為`IEnumerable<T>`不會實作[新增](https://msdn.microsoft.com/en-us/library/63ywd54z.aspx).

講師可以教導任意數目的課程，因此`Courses`做為集合定義`Course`實體。 我們的商務規則狀態講師只能使用最多一個辦事處的資料，因此`OfficeAssignment`定義為單一`OfficeAssignment`實體 (它可能是`null`如果被不指派任何 office)。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>建立 OfficeAssignment 實體

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

建立*Models\OfficeAssignment.cs*為下列程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

建置專案時，會將儲存您的變更，並確認您還未做任何複製和貼上，編譯器可以攔截的錯誤。

### <a name="the-key-attribute"></a>索引鍵屬性

以 0 或-1 一個之間沒有關聯性`Instructor`和`OfficeAssignment`實體。 Office 指派相對於講師指派，才會存在，因此其主索引鍵也是其外部索引鍵`Instructor`實體。 Entity Framework 無法自動辨識，但是`InstructorID`為主要索引鍵此實體的因為其名稱未遵循`ID`或*classname* `ID`命名慣例。 因此，`Key`屬性用來識別索引鍵：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

您也可以使用`Key`屬性如果實體沒有自己的主索引鍵，但您想要指定不同的名稱屬性`classnameID`或`ID`。 根據預設 EF 視為金鑰非產生資料庫因為資料行是識別的關聯性。

### <a name="the-foreignkey-attribute"></a>ForeignKey 屬性

有一對零-或-一關聯性或兩個實體之間的一對一關聯性時 (這類之間`OfficeAssignment`和`Instructor`)，EF 不算出關聯性的一端是主體，而且相依的結尾。 一對一關聯性為其他類別的每個類別中有參考導覽屬性。 [ForeignKey 屬性](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)可以套用至相依的類別，以建立關聯性。 如果您省略[ForeignKey 屬性](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)，當您嘗試建立移轉作業時，收到下列錯誤：

無法判定類型 'ContosoUniversity.Models.OfficeAssignment' 和 'ContosoUniversity.Models.Instructor' 之間的關聯的主要端點。 此關聯的主要端點必須使用明確設定關聯性 fluent 應用程式開發介面或資料註解。

稍後在本教學課程中我們將示範如何使用來設定此關聯性 fluent 應用程式開發的應用程式開發介面。

### <a name="the-instructor-navigation-property"></a>講師導覽屬性

`Instructor`實體有允許 null`OfficeAssignment`導覽屬性 （因為講師可能沒有 office 指派），而`OfficeAssignment`實體具有不可為 null`Instructor`導覽屬性 （因為無法 office 指派沒有講師-存在`InstructorID`不可為 null)。 當`Instructor`實體具有相關`OfficeAssignment`實體，每個實體會有另一個在其導覽屬性的參考。

您無法將`[Required]`講師導覽屬性，指定必須有相關的講師，但您不需要這樣做，因為 InstructorID 外部索引鍵 （這也是這個資料表的索引鍵） 是不可為 null 的屬性。

## <a name="modify-the-course-entity"></a>修改課程實體

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

在*Models\Course.cs*，稍早為下列程式碼取代您所加入的程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

課程實體具有外部索引鍵屬性`DepartmentID`指向相關`Department`實體，且具有`Department`導覽屬性。 Entity Framework 不需要具有相關實體的導覽屬性時，將外部索引鍵屬性加入至您的資料模型。 在需要時會 EF 會自動在資料庫中建立外部索引鍵。 但是，具有資料模型中的外部索引鍵可進行更新，更簡單且更有效率。 例如，當您擷取課程實體，若要編輯，`Department`實體為 null 如果您不載入它，因此當您更新課程實體中，您必須先擷取`Department`實體。 當外部索引鍵屬性`DepartmentID`是否包含在資料模型，您不需要擷取`Department`實體之後才更新。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 屬性

[DatabaseGenerated 屬性](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx)與[無](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx)參數`CourseID`屬性指定主索引鍵值是使用者所提供，而不是不是資料庫產生。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

根據預設，Entity Framework 會假設資料庫會產生主索引鍵值。 這是您想在大多數情況下。 不過，對於`Course`實體，您會使用使用者指定課程數字，例如 1000年一連串一個部門，另一個部門，2000年數列等等。

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵和導覽屬性

外部索引鍵屬性和導覽屬性中的`Course`實體反映的下列關聯性：

- 課程指派給一個部門，所以`DepartmentID`外部索引鍵和`Department`導覽屬性，如上面所提的原因。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- 課程可以有任意數目的學生註冊，所以`Enrollments`導覽屬性是集合： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- 課程會教由多個講師，所以`Instructors`導覽屬性是集合： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>建立 Department 實體

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

建立*Models\Department.cs*為下列程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>資料行屬性

您所用的稍早[資料行屬性](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)若要變更資料行名稱的對應。 在程式碼`Department`實體，`Column`屬性用來變更 SQL 資料類型對應，以便將會定義資料行，使用 SQL Server [money](https://msdn.microsoft.com/en-us/library/ms179882.aspx)資料庫中的類型：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

資料行對應通常是不必要的因為 Entity Framework 通常會選擇根據您定義屬性的 CLR 類型的適當 SQL Server 資料類型。 CLR`decimal`類型會對應至 SQL Server`decimal`型別。 但在此情況下您知道貨幣金額將會保留資料行和[money](https://msdn.microsoft.com/en-us/library/ms179882.aspx)資料類型是更適合的。

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵和導覽屬性

外部索引鍵和導覽屬性會反映的下列關聯性：

- 部門可能會或可能不會為系統管理員，而且系統管理員永遠是講師。 因此`InstructorID`屬性是包含外部索引鍵為`Instructor`後面會加上的實體以及問號`int`輸入指定將標示為可為 null 的屬性。導覽屬性名為`Administrator`但保留`Instructor`實體： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- 部門可能有許多課程，所以`Courses`導覽屬性： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

 > [!NOTE]
 > 依照慣例，Entity Framework 可讓 cascade delete 不可為 null 的外部索引鍵和多對多關聯性。 這會導致循環的串聯刪除規則，您的初始設定式程式碼執行時，會導致例外狀況。 例如，如果您未定義`Department.InstructorID`屬性做為可為 null，初始設定式執行時，會收到下列例外狀況訊息: 「 參考關聯性將會導致不允許循環參考。 」 如果您的商務規則所需`InstructorID`成不可為 null 的屬性，您必須使用下列 fluent API 停用串聯刪除關聯性上： 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modifying-the-student-entity"></a>修改學生實體

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

在*Models\Student.cs*，稍早為下列程式碼取代您所加入的程式碼。 所做的變更會反白顯示。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>註冊實體

 在*Models\Enrollment.cs*，稍早為下列程式碼取代您所加入的程式碼

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵和導覽屬性

外部索引鍵屬性和導覽屬性會反映下列關聯性：

- 註冊記錄是單一的課程中，所以沒有`CourseID`外部索引鍵屬性和`Course`導覽屬性： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- 註冊記錄是單一的學生，所以沒有`StudentID`外部索引鍵屬性和`Student`導覽屬性： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>多對多關聯性

多對多關聯性之間`Student`和`Course`實體，而`Enrollment`實體做為多對多聯結資料表*裝載*資料庫中。 這表示`Enrollment`資料表包含額外的資料以外的聯結資料表的外部索引鍵 (在這個情況下，主索引鍵和`Grade`屬性)。

下圖顯示這些關聯性中的實體圖表的外觀。 (此圖表使用產生的[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); 建立圖表不屬於本教學課程，正只為一個實例。)

![學生-Course_many-到-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

每個關聯性線條的一端和星號 1 (\*)，在指出的一對多關聯性。

如果`Enrollment`資料表未包含等級資訊，它只需要包含兩個外部索引鍵`CourseID`和`StudentID`。 在此情況下，它就會對應至多對多聯結資料表*不裝載*(或*純聯結資料表*) 在資料庫中，以及您不會有完全為它建立模型類別。 `Instructor`和`Course`實體具有多對多關聯性，該類型，如您所見，它們之間沒有任何實體類別：

![講師-Course_many-到-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

聯結資料表所需要的是資料庫，不過，如下列的資料庫圖表中所示：

![講師-Course_many-到-many_relationship_tables](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

Entity Framework 會自動建立`CourseInstructor`資料表，以及您讀取和更新的讀取和更新的間接`Instructor.Courses`和`Course.Instructors`導覽屬性。

## <a name="entity-diagram-showing-relationships"></a>實體圖表顯示關聯性

下圖顯示已完成的 School 模型的 Entity Framework Power Tools 建立的圖表。

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

除了多對多關聯性線條 (\*至\*) 和一個對多關聯性線條 (1 到\*)，這裡可以看到的以 0 或-1 一個關聯線 (1 對 0.1) 之間`Instructor`和`OfficeAssignment`實體和 0-或--一對多關聯性線條 (0.1 對\*) 講師和 Department 實體之間。

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>將程式碼加入至資料庫內容中自訂資料模型

接下來您要加入新的實體來`SchoolContext`類別和自訂的對應使用某些[fluent API](https://msdn.microsoft.com/en-us/data/jj591617)呼叫。 （應用程式開發介面是"fluent 應用程式開發 「 的因為它通常由串連成單一陳述式在一起的方法呼叫的一系列函式）。

在本教學課程中，您將使用 fluent API 僅適用於資料庫對應時，您不能與屬性。 不過，您也可以使用 fluent 應用程式開發的應用程式開發介面指定大部分的格式設定、 驗證和您可以使用屬性的對應規則。 某些屬性，例如`MinimumLength`fluent 應用程式開發的應用程式開發介面無法套用。 如前所述，`MinimumLength`並不會變更結構描述中，它只適用於用戶端和伺服器端驗證規則

有些開發人員偏好使用 fluent 應用程式開發的應用程式開發介面，以獨佔方式，讓它們可保留他們的實體類別 「 清除 」。 您如果想要的話，且有少數自訂項目才可使用 fluent API，可以混合屬性和 fluent 應用程式開發的應用程式開發介面，但一般建議的作法是選擇其中一個這兩種方法，並使用，以一致的方式盡量。

若要加入新的實體資料模型，並執行您未使用屬性做的資料庫對應，取代中的程式碼*DAL\SchoolContext.cs*為下列程式碼：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

新的陳述式中[OnModelCreating](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)方法，會設定 「 多對多聯結資料表：

- 之間的多對多關聯性`Instructor`和`Course`實體，程式碼指定聯結資料表的資料表和資料行名稱。 程式碼第一次可以設定多對多關聯性為您沒有這個程式碼，但如果在未呼叫它，就會預設名稱例如`InstructorInstructorID`如`InstructorID`資料行。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

下列程式碼提供如何您也可以使用 fluent 應用程式開發的應用程式開發介面而非屬性來指定之間的關聯性的範例`Instructor`和`OfficeAssignment`實體：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

如需 「 fluent API 」 陳述式在幕後的執行資訊，請參閱[Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx)部落格文章。

## <a name="seed-the-database-with-test-data"></a>種子使用的測試資料的資料庫

中的程式碼取代*Migrations\Configuration.cs*檔案取代下列程式碼，以針對您已建立新的實體提供種子資料。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

如同第一個教學課程中，大部分的這段程式碼只會更新或建立新的實體物件，載入所需的測試內容中的範例資料。 不過，請注意如何`Course`具有多對多關聯性的實體與`Instructor`實體，會處理：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

當您建立`Course`物件，您初始化`Instructors`導覽屬性為空集合，使用程式碼`Instructors = new List<Instructor>()`。 這樣做可新增`Instructor`至此相關實體`Course`使用`Instructors.Add`方法。 如果您未建立空的清單，您就可以新增這些關聯性，因為`Instructors`屬性會是 null，而且不會有`Add`方法。 您也可以加入清單初始化建構函式。

## <a name="add-a-migration-and-update-the-database"></a>新增的移轉，並更新資料庫

從 PMC 輸入`add-migration`命令：

`PM> add-Migration Chap4`

如果您嘗試更新資料庫，此時，您會得到下列錯誤：

*與外部索引鍵條件約束的 ALTER TABLE 陳述式衝突 」 FK\_dbo。課程\_dbo。部門\_DepartmentID"。衝突發生在資料庫"ContosoUniversity"，資料表"dbo。部門 」，資料行 'DepartmentID'。*

編輯&lt;*時間戳記&gt;\_Chap4.cs*檔案，並變更下列程式碼 (您將加入的 SQL 陳述式，並修改`AddColumn`陳述式):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(請確定您標記為註解或刪除現有`AddColumn`行時加入新的專案，或當您輸入時，會出現的錯誤`update-database`命令。)

有時當您執行移轉以現有的資料，您需要 stub 資料插入資料庫以符合外部索引鍵條件約束，而且這是您正在進行的作業現在。 產生的程式碼會加入不可為 null`DepartmentID`外部索引鍵`Course`資料表。 如果已經有資料列中的`Course`資料表時執行的程式碼，`AddColumn`作業會失敗，因為 SQL Server 並不知道何值，將置於不可為 null 的資料行。 因此，您已變更的程式碼，讓新的資料行的預設值為，而且您已建立名為"Temp"做為預設部門 stub 部門。 如此一來，如果那里現有`Course`資料列時執行此程式碼，它們將所有相關的"Temp"部門。

當`Seed`方法執行時，它會插入資料列中的`Department`資料表和它將與現有`Course`這些新的資料列`Department`資料列。 如果您尚未加入任何課程 UI 中，您然後不再需要"Temp"部門或預設值在`Course.DepartmentID`資料行。 若要允許的可能性，有人可能已經加入課程使用的應用程式，您也要更新`Seed`方法程式碼，請確認所有`Course`資料列 (不只是由先前執行的插入`Seed`方法) 有有效`DepartmentID`之前移除預設的值從資料行值，並刪除"Temp"部門。

當您完成編輯之後&lt;*時間戳記&gt;\_Chap4.cs*檔案中，輸入`update-database`執行移轉 PMC 命令。

> [!NOTE]
> 很可能在移轉資料和進行結構描述變更時，取得其他錯誤。 如果您移轉時發生錯誤，您無法解決，請變更 連接字串中的*Web.config*檔案或刪除資料庫。 最簡單的方法是在資料庫重新命名*Web.config*檔案。 例如，將資料庫名稱變更為 CU\_測試中，如下所示：
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
>  使用新資料庫時，沒有資料移轉，而`update-database`命令是更有可能能順利完成。 如需有關如何刪除資料庫的指示，請參閱[如何卸除資料庫，從 Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。


開啟中的資料庫**伺服器總管**像之前，並展開**資料表**節點以查看所有資料表具有已建立。 (如果仍有**伺服器總管**稍早的時間從開啟中，按一下**重新整理** 按鈕。)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

您未建立的模型類別`CourseInstructor`資料表。 如前所述，這是一個聯結資料表之間的多對多關聯性`Instructor`和`Course`實體。

以滑鼠右鍵按一下`CourseInstructor`資料表，然後選取**顯示資料表資料**驗證中它可能是有資料`Instructor`實體加入至`Course.Instructors`導覽屬性。

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>總結

您現在有更複雜的資料模型和對應的資料庫。 下列教學課程中您會深入了解不同的方式來存取相關的資料。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一頁](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一頁](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
