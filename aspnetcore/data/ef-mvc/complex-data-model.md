---
title: "使用 EF Core-資料模型-10-5 的 ASP.NET Core MVC"
author: tdykstra
description: "本教學課程中您要新增更多的實體和關聯性，並指定格式、 驗證和資料庫對應規則，以自訂資料模型。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: ac30d9ae5531934ba5163a8d9114b11ac54af8d2
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a>建立複雜的資料模型的 EF Core 與 ASP.NET Core MVC 教學課程 (10-5)

由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。

在上一個教學課程中，您使用過的簡單資料模型的三個實體所組成。 在此教學課程中，您將新增多個實體和關聯性，您將自訂資料模型，藉由指定的格式、 驗證和資料庫對應規則。

當您完成時，實體類別會構成完成的資料模型，如下圖所示：

![實體圖表](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a>自訂資料模型使用屬性

本節中，您會看到如何自訂資料模型使用指定的格式、 驗證和資料庫對應規則的屬性。 然後在您將建立下列各節的幾個完整學校資料模型，藉由新增屬性類別您已經建立，並在模型中建立新類別的其餘的實體類型。

### <a name="the-datatype-attribute"></a>資料類型屬性

學生註冊日期，所有的網頁目前顯示的時間和日期，不過關心這個欄位是日期。 使用資料註解屬性，您可以進行一將修復的顯示格式來顯示資料的每個檢視中變更程式碼。 若要查看如何執行作業，您會加入至屬性的範例`EnrollmentDate`屬性`Student`類別。

在*Models/Student.cs*，新增`using`陳述式`System.ComponentModel.DataAnnotations`命名空間和新增`DataType`和`DisplayFormat`屬性至`EnrollmentDate`屬性，如下列範例所示：

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

`DataType`屬性用來指定資料庫內建型別比更特定的資料類型。 在此情況下我們只想要追蹤的日期，不的日期和時間。 `DataType`列舉型別提供許多的資料類型，例如日期、 時間、 PhoneNumber、 貨幣、 EmailAddress，等等。 `DataType` 屬性也可讓應用程式自動提供類型的特定功能。 例如，可建立 `DataType.EmailAddress` 的 `mailto:` 連結，而且可以在支援 HTML5 的瀏覽器中提供 `DataType.Date` 的日期選擇器。 `DataType`屬性發出 HTML 5 `data-` HTML 5 瀏覽器可以了解的 (唸成的資料 dash) 屬性。 `DataType`屬性沒有提供任何驗證。

`DataType.Date`未指定的格式顯示日期。 根據預設，資料欄位會顯示根據伺服器的 CultureInfo 為基礎的預設格式。

`DisplayFormat` 屬性用來明確指定日期格式：

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` 設定可指定在文字方塊中顯示值以供編輯時，也應該套用的格式。 （您可能不想，某些欄位-例如，貨幣值，您可能不想在文字方塊中的貨幣符號進行編輯）。

您可以使用`DisplayFormat`屬性本身，但它通常是最好使用`DataType`也屬性。 `DataType`屬性的語意，而不是如何呈現在螢幕上的資料並提供下列優點，就無法取得與`DisplayFormat`:

* 瀏覽器可以啟用 HTML5 功能 （例如若要顯示日曆控制項、 地區設定適當的貨幣符號、 電子郵件連結，某些用戶端輸入驗證等。）。

* 根據預設，瀏覽器將根據您的地區設定，使用正確的格式呈現資料。

如需詳細資訊，請參閱[\<輸入 > 標記協助程式文件](../../mvc/views/working-with-forms.md#the-input-tag-helper)。

執行應用程式，請學生索引頁，請注意，時間不會再顯示註冊日期。 相同將學生模型會使用任何檢視，則為 true。

![顯示不含時間日期的學生索引頁面](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>StringLength 屬性

您也可以指定資料的驗證規則和使用屬性的驗證錯誤訊息。 `StringLength`屬性設定資料庫中的最大長度，並提供用戶端和伺服器端的 ASP.NET MVC 驗證。 您也可以指定最小字串長度，此屬性，但資料庫結構描述的最小值沒有任何影響。

假設您想要確保使用者不要輸入超過 50 個字元的名稱。 若要加入這項限制，加入`StringLength`屬性至`LastName`和`FirstMidName`屬性，如下列範例所示：

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

`StringLength`屬性將不會防止使用者輸入名稱的泛空白字元。 您可以使用`RegularExpression`將限制套用至輸入的屬性。 例如，下列程式碼需要第一個字元是大寫，而其餘字元是依字母順序排列：

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

`MaxLength`屬性會提供類似的功能`StringLength`屬性，但不會提供用戶端驗證。

資料庫模型現在已變更，則需要在資料庫結構描述變更的方式。 您將使用移轉，而不會遺失任何資料，您可能使用應用程式 UI 加入至資料庫中更新結構描述。

儲存您的變更，並建置專案。 然後在專案資料夾中開啟 [命令] 視窗並輸入下列命令：

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

`migrations add`命令會發出警告，可能會發生資料遺失，因為變更會讓較短的兩個資料行的最大長度。  移轉會建立名為*\<時間戳記 > _MaxLengthOnNames.cs*。 這個檔案包含程式碼中的`Up`方法將會更新資料庫，以符合目前的資料模型。 `database update`命令執行該程式碼。

移轉的檔案名稱的前置詞的時間戳記 Entity framework 用於排序的移轉。 您可以建立多個移轉之前執行更新資料庫的命令，然後所有移轉都套用中所建立的順序。

執行應用程式中，選取**學生**索引標籤上，按一下 **新建**，並輸入任一名稱長度超過 50 個字元。 當您按一下**建立**，用戶端驗證會顯示錯誤訊息。

![學生索引頁面顯示字串長度錯誤](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a>資料行屬性

您也可以使用屬性來控制您的類別和屬性如何對應到資料庫。 假設您使用名稱`FirstMidName`，第一個名稱欄位，因為欄位也可能包含中間名。 但您想要命名的資料庫欄`FirstName`，因為使用者將會寫入資料庫的臨機操作查詢習慣於該名稱。 若要讓此對應，您可以使用`Column`屬性。

`Column`屬性指定當建立資料庫時，資料行`Student`對應到資料表`FirstMidName`屬性會被命名為`FirstName`。 換句話說，當您的程式碼是指`Student.FirstMidName`，資料將來自，或在更新`FirstName`資料行`Student`資料表。 如果您未指定資料行名稱，它們會獲得相同的名稱與屬性名稱。

在*Student.cs* file、 add`using`陳述式`System.ComponentModel.DataAnnotations.Schema`並加入至資料行名稱屬性`FirstMidName`屬性，如下列反白顯示的程式碼所示：

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

新增`Column`屬性變更模型支援`SchoolContext`，因此它將不會符合資料庫。

儲存您的變更，並建置專案。 然後在專案資料夾中開啟 [命令] 視窗並輸入下列命令來建立其他移轉：

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

在**SQL Server 物件總管**，按兩下開啟學生資料表設計工具**學生**資料表。

![在移轉之後，在 SSOX students 資料表](complex-data-model/_static/ssox-after-migration.png)

套用的前兩個移轉作業之前，名稱資料行的類型 nvarchar （max）。 它們現在 nvarchar （50） 和資料行名稱已從 FirstMidName 改成 FirstName。

> [!Note]
> 如果您嘗試編譯，才能完成下列各節中建立的所有實體類別，您可能會收到編譯器錯誤。

## <a name="final-changes-to-the-student-entity"></a>最後的學生實體的變更

![學生實體](complex-data-model/_static/student-entity.png)

在*Models/Student.cs*，稍早為下列程式碼取代您所加入的程式碼。 所做的變更會反白顯示。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>必要的屬性

`Required`屬性會讓名稱屬性的必要的欄位。 `Required`屬性不需要不可為 null 的類型，例如實值類型 (DateTime、 int、 double、 float、 等等。)。 不可為 null 的型別會自動視為必要欄位。

您也可以移除`Required`屬性並將它取代的最小長度參數`StringLength`屬性：

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>顯示屬性

`Display`屬性指定的文字方塊中的標題應該 「 名字 」、 「 姓氏 」、 「 完整名稱 」 和 「 註冊日期 」，而不是屬性名稱在每個執行個體 （其除以字沒有空格）。

### <a name="the-fullname-calculated-property"></a>計算的 FullName 屬性

`FullName`是傳回值，由串連兩個其他屬性的導出的屬性。 因此它具有只有 get 存取子，但沒有`FullName`會在資料庫中產生資料行。

## <a name="create-the-instructor-entity"></a>建立 [Instructor] 實體

![Instructor 實體](complex-data-model/_static/instructor-entity.png)

建立*Models/Instructor.cs*，範本程式碼取代為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

請注意一些屬性，在 「 學生 」 和 「 講師實體相同。 在[實作繼承](inheritance.md)稍後在本系列的教學課程，您將會重構此程式碼以消除重複。

您也可以將多個屬性放在同一行，您也可以撰寫`HireDate`屬性，如下所示：

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>CourseAssignments 和 OfficeAssignment 的導覽屬性

`CourseAssignments`和`OfficeAssignment`屬性為導覽屬性。

講師可以教導任意數目的課程，因此`CourseAssignments`定義為集合。

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

如果導覽屬性都可以保存多個實體，其型別必須是的清單中的項目可加入、 刪除及更新。  您可以指定`ICollection<T>`或型別，例如`List<T>`或`HashSet<T>`。 如果您指定`ICollection<T>`，EF 建立`HashSet<T>`預設集合。

這些是為什麼原因`CourseAssignment`實體是下面將說明有關多對多關聯性 > 一節。

Contoso 大學商務規則狀態講師只能有最多一個辦事處的資料，所以`OfficeAssignment`屬性會保留單一 OfficeAssignment 實體 （這可能是 null，如果沒有 office 指派）。

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>建立 OfficeAssignment 實體

![OfficeAssignment 實體](complex-data-model/_static/officeassignment-entity.png)

建立*Models/OfficeAssignment.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>索引鍵屬性

沒有講師 OfficeAssignment 實體之間以 0 或-1 一個關聯性。 Office 指派相對於講師指派，才會存在，因此其主索引鍵也是其外部索引鍵 [Instructor] 實體。 但是，Entity Framework 無法自動辨識 InstructorID 為此實體的主索引鍵因為其名稱未遵循識別碼或 classnameID 命名慣例。 因此，`Key`屬性用來識別索引鍵：

```csharp
[Key]
public int InstructorID { get; set; }
```

您也可以使用`Key`屬性如果實體沒有自己的主索引鍵，但您想要的項目名稱屬性，非 classnameID 或識別碼。

根據預設，EF 視為金鑰非產生資料庫因為資料行是識別的關聯性。

### <a name="the-instructor-navigation-property"></a>講師導覽屬性

[Instructor] 實體有允許 null`OfficeAssignment`導覽屬性 （因為講師可能沒有 office 指派），而且 OfficeAssignment 實體具有不可為 null`Instructor`導覽屬性 （因為無法 office 指派沒有講師-存在`InstructorID`不可為 null)。 當 [Instructor] 實體具有相關的 OfficeAssignment 實體時，每個實體會有另一個在其導覽屬性的參考。

您無法將`[Required]`講師導覽屬性，指定必須有相關的講師，但您不需要這樣做，因為屬性`InstructorID`（這也是這個資料表的索引鍵） 的外部索引鍵是不可為 null。

## <a name="modify-the-course-entity"></a>修改課程實體

![課程實體](complex-data-model/_static/course-entity.png)

在*Models/Course.cs*，稍早為下列程式碼取代您所加入的程式碼。 所做的變更會反白顯示。

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

課程實體具有外部索引鍵屬性`DepartmentID`哪些點及其相關的 Department 實體具有`Department`導覽屬性。

Entity Framework 不需要具有相關實體的導覽屬性時，將外部索引鍵屬性加入至您的資料模型。  EF 自動在資料庫中建立外部索引鍵，只要它們仍必要的並建立[遮蔽屬性](https://docs.microsoft.com/ef/core/modeling/shadow-properties)它們。 但是，具有資料模型中的外部索引鍵可進行更新，更簡單且更有效率。 例如，當您擷取一個課程實體，來編輯、 Department 實體為 null 如果您不載入它，因此當您更新課程實體中，您必須先擷取 Department 實體。 當外部索引鍵屬性`DepartmentID`是否包含在資料模型，您不需要在更新之前擷取 Department 實體。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 屬性

`DatabaseGenerated`屬性附帶`None`參數`CourseID`屬性指定主索引鍵值是使用者所提供，而不是不是資料庫產生。

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

根據預設，Entity Framework 會假設資料庫會產生主索引鍵值。 這是您想在大多數情況下。 不過，課程實體，您會使用使用者指定課程數字，例如 1000年一連串一個部門，另一個部門，2000年數列等等。

`DatabaseGenerated`屬性也可用來產生預設值，如果是用來記錄日期的資料庫資料行建立或更新資料列。  如需詳細資訊，請參閱[產生屬性](https://docs.microsoft.com/ef/core/modeling/generated-properties)。

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵和導覽屬性

外部索引鍵屬性和課程實體中的導覽屬性會反映下列關聯性：

課程指派給一個部門，所以`DepartmentID`外部索引鍵和`Department`導覽屬性，如上面所提的原因。

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

課程可以有任意數目的學生註冊，所以`Enrollments`導覽屬性是集合：

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

課程會教由多個講師，所以`CourseAssignments`導覽屬性是集合 (型別`CourseAssignment`說明[稍後](#many-to-many-relationships)):

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a>建立 Department 實體

![Department 實體](complex-data-model/_static/department-entity.png)


建立*Models/Department.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>資料行屬性

您所用的稍早`Column`屬性，以變更資料行名稱的對應。 在 Department 實體，程式碼`Column`屬性用來變更 SQL 資料類型對應，以便將使用資料庫中的 SQL Server money 類型會定義資料行：

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

資料行對應通常是不必要的因為 Entity Framework 選擇適當 SQL Server 資料類型，根據您定義屬性的 CLR 型別。 CLR`decimal`類型會對應至 SQL Server`decimal`型別。 但在此情況下您知道資料行保留貨幣金額，而 money 資料類型更適合的。

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵和導覽屬性

外部索引鍵和導覽屬性會反映的下列關聯性：

部門可能會或可能不會為系統管理員，而且系統管理員永遠是講師。 因此`InstructorID`屬性是包含為 [Instructor] 實體的外部索引鍵，且後面會加上問號`int`輸入指定將標示為可為 null 的屬性。 導覽屬性名為`Administrator`但保留 [Instructor] 實體：

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

部門可能具有許多課程，課程導覽屬性是：

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> 依照慣例，Entity Framework 可讓 cascade delete 不可為 null 的外部索引鍵和多對多關聯性。 這會導致循環的串聯刪除規則，這會造成例外狀況，當您嘗試新增的移轉。 比方說，如果您未定義 Department.InstructorID 屬性為可為 null，EF 會設定要刪除的部門，這並不是您想要發生的事情時刪除講師串聯刪除規則。 如果您的商務規則所需`InstructorID`屬性成為不可為 null，您必須使用下列的 fluent API 陳述式來停用串聯刪除關聯性上：
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a>修改註冊實體

![註冊實體](complex-data-model/_static/enrollment-entity.png)

在*Models/Enrollment.cs*，稍早為下列程式碼取代您所加入的程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵和導覽屬性

外部索引鍵屬性和導覽屬性會反映下列關聯性：

註冊記錄是單一的課程中，所以沒有`CourseID`外部索引鍵屬性和`Course`導覽屬性：

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

註冊記錄是單一的學生，所以沒有`StudentID`外部索引鍵屬性和`Student`導覽屬性：

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>多對多關聯性

學生與課程實體之間，沒有多對多關聯性，而且註冊實體做為多對多聯結資料表*裝載*資料庫中。 「 內容 」 代表 Enrollment 資料表包含外部索引鍵的聯結的資料表 （在此情況下，主索引鍵和等級屬性） 以外的其他資料。

下圖顯示這些關聯性中的實體圖表的外觀。 (此圖表使用產生的 Entity Framework Power Tools 的 EF 6.x; 建立圖表不屬於本教學課程，正只為一個實例。)

![學生課程多對多關聯性](complex-data-model/_static/student-course.png)

每個關聯性線條在另一個對多關聯性，表示有一個結束作業，並以星號 （*） 1。

如果在 Enrollment 資料表未包含等級資訊，它只需要包含兩個外部索引鍵 CourseID 和 StudentID。 在此情況下，它可以裝載多對多聯結資料表 （或純聯結資料表） 在資料庫。 「 講師 」 和 「 課程實體具有多對多關聯性，該類型和下一步是建立實體類別，以做為裝載聯結資料表。

（不會隱含聯結資料表的多對多關聯性，但 EF 核心 EF 6.x 支援。 如需詳細資訊，請參閱[EF 核心 GitHub 儲存機制中的討論](https://github.com/aspnet/EntityFramework/issues/1368)。) 

## <a name="the-courseassignment-entity"></a>CourseAssignment 實體

![CourseAssignment 實體](complex-data-model/_static/courseassignment-entity.png)

建立*Models/CourseAssignment.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a>加入實體名稱

聯結資料表需要講師-課程多對多關聯性的資料庫中，而且它必須由實體集。 通常會命名為聯結實體`EntityName1EntityName2`，它在此情況下會是`CourseInstructor`。 不過，我們建議您選擇描述關聯性的名稱。 資料模型開始簡單及隨經常收到稍後裝載否裝載聯結。 如果您開始使用描述性的實體名稱，您不必之後變更名稱。 理想情況下，聯結實體商務網域中必須有自己的自然 (可能是單一 word) 名稱。 比方說，書籍客戶無法連結到評等。 此關聯性，`CourseAssignment`是較佳的選擇，比`CourseInstructor`。

### <a name="composite-key"></a>複合索引鍵

因為外部索引鍵都不是可為 null 且一起唯一識別資料表的每個資料列，就不必再為個別的主索引鍵。 *InstructorID*和*CourseID*屬性應該當做複合主索引鍵。 若要識別 EF 複合主索引鍵的唯一方法是使用*fluent API* （它無法完成使用屬性）。 您會看到如何設定在下一節中的複合主索引鍵。

複合索引鍵可確保您可以有一個課程中，和一個講師的多個資料列的多個資料列，而您不能有多個資料列的相同講師和課程。 `Enrollment`聯結實體會定義它自己的主索引鍵，因此可能會有這種重複的項目。 若要避免這種重複的項目，您無法加入唯一的索引上的外部索引鍵欄位，或設定`Enrollment`主複合索引鍵類似`CourseAssignment`。 如需詳細資訊，請參閱[索引](https://docs.microsoft.com/ef/core/modeling/indexes)。

## <a name="update-the-database-context"></a>更新的資料庫內容

將下列反白顯示的程式碼加入*Data/SchoolContext.cs*檔案：

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

此程式碼會新增新的實體，並設定 CourseAssignment 實體的複合主索引鍵。

## <a name="fluent-api-alternative-to-attributes"></a>Fluent API 替代項目屬性

中的程式碼`OnModelCreating`方法`DbContext`類別會使用*fluent API*設定 EF 行為。 API 稱為 「 fluent"，因為它通常由一系列的方法呼叫一起串連成單一陳述式，從如此範例所示[EF 核心文件集](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

在本教學課程中，您使用 fluent API 僅適用於資料庫對應時，您不能與屬性。 不過，您也可以使用 fluent 應用程式開發的應用程式開發介面指定大部分的格式設定、 驗證和您可以使用屬性的對應規則。 某些屬性，例如`MinimumLength`fluent 應用程式開發的應用程式開發介面無法套用。 如前所述，`MinimumLength`並不會變更結構描述中，它只適用於用戶端和伺服器端驗證規則。

有些開發人員偏好使用 fluent 應用程式開發的應用程式開發介面，以獨佔方式，讓它們可保留他們的實體類別 「 清除 」。 您如果想要的話，且有少數自訂項目才可使用 fluent API，可以混合屬性和 fluent 應用程式開發的應用程式開發介面，但一般建議的作法是選擇其中一個這兩種方法，並使用，以一致的方式盡量。 如果您使用兩者，請注意，就會發生衝突，只要 Fluent API 會覆寫屬性。

如需 fluent API 與屬性的詳細資訊，請參閱[的組態方法](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration)。

## <a name="entity-diagram-showing-relationships"></a>實體圖表顯示關聯性

下圖顯示已完成的 School 模型的 Entity Framework Power Tools 建立的圖表。

![實體圖表](complex-data-model/_static/diagram.png)

除了一對多關聯性線條 (1 到\*)，您可以在這裡看到到 0 或-1 一個之間的關聯線 (1 對 0..1) 「 講師 」 和 「 OfficeAssignment 實體和 0-或--一對多關聯性線條 (0..1 對 *) 之間講師和 Department 實體。

## <a name="seed-the-database-with-test-data"></a>種子使用的測試資料的資料庫

中的程式碼取代*Data/DbInitializer.cs*檔案取代下列程式碼，以針對您已建立新的實體提供種子資料。

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

如同第一個教學課程中，大部分的這段程式碼只會建立新的實體物件，並將範例資料載入至內容所需的測試。 請注意多對多關聯性的處理方式： 程式碼中的實體建立關聯性`Enrollments`和`CourseAssignment`加入實體集。

## <a name="add-a-migration"></a>新增移轉

儲存您的變更，並建置專案。 然後在專案資料夾中開啟 [命令] 視窗並輸入`migrations add`命令 （不執行 update-database 命令尚未）：

```console
dotnet ef migrations add ComplexDataModel
```

您會收到有關可能資料遺失警告。

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

如果您嘗試執行`database update`此時命令 （不要這麼做還），就會收到下列錯誤：

> ALTER TABLE 陳述式發生衝突的外部索引鍵條件約束 」 FK_dbo。Course_dbo。Department_DepartmentID"。 衝突發生在資料庫"ContosoUniversity"，資料表"dbo。部門 」，資料行 'DepartmentID'。

有時當您執行移轉以現有的資料，您需要將虛設常式資料插入資料庫以符合外部索引鍵條件約束。 產生的程式碼`Up`方法會加入 Course 資料表不可為 null 的 DepartmentID 外部索引鍵。 如果已經有資料列在 Course 資料表中的程式碼執行時，`AddColumn`作業失敗，因為 SQL Server 並不知道何值，將置於不可為 null 的資料行。 本教學課程中您將在新的資料庫上執行移轉，但實際執行應用程式在您必須進行移轉處理現有的資料，所以下列指示會顯示如何執行這些作業的範例。

若要進行這項移轉使用現有的資料，您必須變更程式碼，讓新的資料行預設值，並建立 stub 部門名為"Temp"做為預設部門。 如此一來，現有的資料列課程將所有相關"Temp"部門之後`Up`方法執行。

* 開啟*{timestamp}_ComplexDataModel.cs*檔案。 

* 註解加入 Course 資料表中的 [DepartmentID] 欄的程式碼行。

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* 建立將 Department 資料表的程式碼之後加入下列反白顯示的程式碼：

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

在實際執行應用程式中，您會撰寫程式碼或指令碼以加入部門資料列，並與新的部門資料列相關課程資料列。 您接著不再需要"Temp"部門或 Course.DepartmentID 資料行的預設值。

儲存您的變更，並建置專案。

## <a name="change-the-connection-string-and-update-the-database"></a>變更連接字串，並更新資料庫

您現在有新的程式碼`DbInitializer`將新的實體的種子資料新增至空的資料庫類別。 若要建立新的空白資料庫的 EF，變更連接字串中的資料庫名稱*appsettings.json* ContosoUniversity3 或您並未使用您所使用的電腦上的其他部份名稱。

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

儲存變更至*appsettings.json*。

> [!NOTE]
> 變更資料庫名稱的替代方案，您可以刪除資料庫。 使用**SQL Server 物件總管**(SSOX) 或`database drop`CLI 命令：
> ```console
> dotnet ef database drop
> ```

您已變更資料庫名稱，或刪除資料庫之後，請執行`database update`命令在命令視窗中執行移轉。

```console
dotnet ef database update
```

執行應用程式，讓`DbInitializer.Initialize`方法來執行，並填入新的資料庫。

SSOX 中開啟的資料庫，您可以如同更早版本，然後展開**資料表**節點以查看所有資料表具有已建立。 (如果仍有 SSOX 開啟從較早的時間，按一下**重新整理** 按鈕。)

![SSOX 中的資料表](complex-data-model/_static/ssox-tables.png)

執行觸發植入資料庫的初始設定式程式碼應用程式。

以滑鼠右鍵按一下**CourseAssignment**資料表，然後選取**檢視資料**以確認它中沒有資料。

![SSOX CourseAssignment 資料](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a>總結

您現在有更複雜的資料模型和對應的資料庫。 下列教學課程中，您將進一步了解如何存取相關的資料。

>[!div class="step-by-step"]
[上一頁](migrations.md)
[下一頁](read-related-data.md)  
