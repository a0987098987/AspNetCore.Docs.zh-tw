---
title: "使用 EF Core-資料模型-5 8 的 razor 頁面"
author: rick-anderson
description: "本教學課程中您要新增更多的實體和關聯性，並指定格式、 驗證和資料庫對應規則，以自訂資料模型。"
keywords: "ASP.NET Core，Entity Framework Core，資料註解"
ms.author: riande
manager: wpickett
ms.date: 10/25/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: c2761f79fa4836d29541526782969bb0fd47778b
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a>建立複雜的資料模型的 EF 核心 Razor 頁面教學課程 (5 8 個)

由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

先前的教學課程使用過的基本資料模型的三個實體所組成。 在本教學課程：

* 會加入更多的實體和關聯性。
* 以指定格式、 驗證和資料庫對應規則自訂資料模型。

在下圖顯示完成的資料模型的實體類別：

![實體圖表](complex-data-model/_static/diagram.png)

如果您遇到無法解決的問題時，請下載[已完成的應用程式，為此階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex)。

## <a name="customize-the-data-model-with-attributes"></a>自訂屬性的資料模型

在本節中的資料模型使用屬性自訂。

### <a name="the-datatype-attribute"></a>資料類型屬性

目前的學生頁面就會顯示註冊日期的時間。 一般而言，日期欄位會顯示只以日期而不是時間。

更新*Models/Student.cs*以下列反白顯示程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

[DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1)屬性會指定資料庫內建型別比更特定的資料類型。 在此情況下應該顯示的日期，不的日期和時間。 [DataType 列舉](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1)提供許多的資料類型，例如日期、 時間、 電話號碼、 貨幣、 EmailAddress 等等。`DataType`屬性也可以啟用應用程式會自動提供特定類型的功能。 例如: 

* `mailto:`連結會自動建立`DataType.EmailAddress`。
* 日期選擇器提供`DataType.Date`大部分的瀏覽器中。

`DataType`屬性發出 HTML 5 `data-` HTML 5 瀏覽器使用的 (唸成的資料 dash) 屬性。 `DataType`屬性是否提供驗證。

`DataType.Date` 未指定顯示日期的格式。 根據預設，[日期] 欄位會顯示根據基礎在伺服器上的預設格式[CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support)。

`DisplayFormat` 屬性用來明確指定日期格式：

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode`設定可讓您指定的格式應該也會套用至編輯 UI。 某些欄位不應該使用`ApplyFormatInEditMode`。 例如，貨幣符號應該通常不會顯示在 [編輯] 文字方塊中。

`DisplayFormat`屬性可由本身。 它通常是最好使用`DataType`屬性附帶`DisplayFormat`屬性。 `DataType`屬性提供的語意，而不是如何呈現在螢幕上的資料。 `DataType`屬性會提供下列優點，不適用於`DisplayFormat`:

* 瀏覽器可以啟用 HTML5 功能。 例如，顯示日曆控制項、 地區設定適當的貨幣符號、 電子郵件連結、 用戶端輸入的驗證等。
* 根據預設，瀏覽器會呈現資料使用正確的格式會根據地區設定。

如需詳細資訊，請參閱[\<輸入 > 標記協助程式文件](xref:mvc/views/working-with-forms#the-input-tag-helper)。

執行應用程式。 瀏覽至學生索引頁面。 不會再顯示的時間。 使用的每個檢視`Student`模型就會顯示不含時間日期。

![顯示不含時間日期的學生索引頁面](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>StringLength 屬性

資料驗證規則和驗證錯誤訊息可以使用屬性來指定。 [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1)屬性會指定字元資料欄位中允許的最小和最大長度。 `StringLength`屬性也會提供用戶端和伺服器端驗證。 最小值的資料庫結構描述沒有任何影響。

更新`Student`包含下列程式碼的模型：

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

名稱不能超過 50 個字元限制為上述的程式碼。 `StringLength`屬性不會防止使用者輸入名稱的泛空白字元。 [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1)屬性用來將限制套用至輸入。 例如，下列程式碼需要第一個字元是大寫，而其餘字元是依字母順序排列：

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

執行應用程式：

* 瀏覽至學生頁面。
* 選取**新建**，並輸入的名稱長度超過 50 個字元。
* 選取**建立**，用戶端驗證顯示錯誤訊息。

![學生索引頁面顯示字串長度錯誤](complex-data-model/_static/string-length-errors.png)

在**SQL Server 物件總管**(SSOX)，按兩下開啟學生資料表設計工具**學生**資料表。

![移轉之前，在 SSOX students 資料表](complex-data-model/_static/ssox-before-migration.png)

上述影像中顯示的結構描述`Student`資料表。 名稱欄位有類型`nvarchar(MAX)`因為尚未在資料庫上執行移轉。 移轉作業執行時稍後在本教學課程中，名稱欄位會成為`nvarchar(50)`。

### <a name="the-column-attribute"></a>資料行屬性

屬性可以控制如何對應到資料庫的類別和屬性。 在本節中，`Column`屬性用來對應名稱`FirstMidName`DB 中的"FirstName"的屬性。

建立資料庫時，在模型上的屬性名稱會用於資料行名稱 (除非`Column`屬性使用)。

`Student`模型使用`FirstMidName`，第一個名稱欄位，因為欄位也可能包含中間名。

更新*Student.cs*以下列反白顯示的程式碼檔案：

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

與上述變更，`Student.FirstMidName`應用程式中對應至`FirstName`資料行`Student`資料表。

新增`Column`屬性變更模型支援`SchoolContext`。 模型支援`SchoolContext`不再符合資料庫。 如果套用移轉之前執行的應用程式，則會產生下列例外狀況：

```SQL
SqlException: Invalid column name 'FirstName'.
```
若要更新資料庫：

* 建置專案。
* 專案資料夾中，開啟命令視窗。 輸入下列命令來建立新的移轉，並更新資料庫：

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

`dotnet ef migrations add ColumnFirstName`命令會產生下列警告訊息：

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

會產生警告，因為現在的名稱欄位限制為 50 個字元。 如果資料庫中的名稱具有超過 50 個字元，將會遺失最後一個字元 51。

* 測試應用程式。

開啟 SSOX 學生資料表：

![在移轉之後，在 SSOX students 資料表](complex-data-model/_static/ssox-after-migration.png)

名稱資料行已套用移轉之前，都型別的[nvarchar （max)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)。 名稱資料行現在會`nvarchar(50)`。 資料行名稱已經從`FirstMidName`至`FirstName`。

> [!Note]
> 下一節，在某些階段建置的應用程式會產生編譯器錯誤。 指示可指定何時要建置應用程式。

## <a name="student-entity-update"></a>學生實體更新

![學生實體](complex-data-model/_static/student-entity.png)

更新*Models/Student.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>必要的屬性

`Required`屬性會讓名稱屬性的必要的欄位。 `Required`屬性不需要不可為 null 的類型，例如實值類型 (`DateTime`， `int`，`double`等。)。 不可為 null 的型別會自動視為必要欄位。

`Required`屬性無法取代中的最小長度參數`StringLength`屬性：

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>顯示屬性

`Display`屬性指定的標題文字方塊中應該是 「 名字 」、 「 姓氏 」、 「 完整名稱 」 和 「 註冊日期 」。 預設標題已經沒有空間分割單字，例如"Lastname"。

### <a name="the-fullname-calculated-property"></a>計算的 FullName 屬性

`FullName`是傳回值，由串連兩個其他屬性的導出的屬性。 `FullName`無法設定，它有只有 get 存取子。 否`FullName`資料庫中建立資料行。

## <a name="create-the-instructor-entity"></a>建立 [Instructor] 實體

![Instructor 實體](complex-data-model/_static/instructor-entity.png)

建立*Models/Instructor.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

請注意一些屬性，在相同`Student`和`Instructor`實體。 實作繼承教學課程稍後在本系列中，此程式碼已重構消除重複性。

多個屬性可以在同一行。 `HireDate`可以寫入屬性，如下所示：

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>CourseAssignments 和 OfficeAssignment 的導覽屬性

`CourseAssignments`和`OfficeAssignment`屬性為導覽屬性。

講師可以教導任意數目的課程，因此`CourseAssignments`定義為集合。

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

如果導覽屬性會保留多個實體：

* 它必須是清單型別其中項目可以新增、 刪除和更新。

瀏覽內容類型包括：

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

如果`ICollection<T>`指定，則會建立 EF 核心`HashSet<T>`預設集合。

`CourseAssignment`實體是一節所述的多對多關聯性。

Contoso 大學商務規則講師可以有最多一個辦事處的狀態。 `OfficeAssignment`屬性會保留單一`OfficeAssignment`實體。 `OfficeAssignment`如果沒有 office 會指派，為 null。

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>建立 OfficeAssignment 實體

![OfficeAssignment 實體](complex-data-model/_static/officeassignment-entity.png)

建立*Models/OfficeAssignment.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>索引鍵屬性

`[Key]`屬性用來識別屬性的主索引鍵 (PK) 的屬性名稱任務在身時以外 classnameID 或識別碼。

以 0 或-1 一個之間沒有關聯性`Instructor`和`OfficeAssignment`實體。 相對於指派給講師只存在 office 指派。 `OfficeAssignment` PK 也是其外部索引鍵 (FK) 至`Instructor`實體。 無法自動辨識 EF 核心`InstructorID`為的 PK`OfficeAssignment`因為：

* `InstructorID`未遵循識別碼或 classnameID 命名慣例。

因此，`Key`屬性用來識別`InstructorID`PK 為：

```csharp
[Key]
public int InstructorID { get; set; }
```

根據預設，EF 核心視為金鑰非產生資料庫因為資料行是識別的關聯性。

### <a name="the-instructor-navigation-property"></a>講師導覽屬性

`OfficeAssignment`瀏覽屬性`Instructor`可為 null 的實體，是因為：

* 參考類型 （例如類別是可為 null）。
* 講師可能沒有 office 指派。


`OfficeAssignment`實體具有不可為 null`Instructor`導覽屬性因為：

* `InstructorID`是不可為 null。
* 沒有講師就無法存在 office 指派。

當`Instructor`實體具有相關`OfficeAssignment`實體，每個實體有另一個在其導覽屬性的參考。

`[Required]`屬性無法套用至`Instructor`導覽屬性：

```csharp
[Required]
public Instructor Instructor { get; set; }
```

上述程式碼會指定必須相關的講師。 上述程式碼不需要因為`InstructorID`（這也是在 PK） 的外部索引鍵是不可為 null。

## <a name="modify-the-course-entity"></a>修改課程實體

![課程實體](complex-data-model/_static/course-entity.png)

更新*Models/Course.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

`Course`實體具有外部索引鍵 (FK) 屬性`DepartmentID`。 `DepartmentID`指向 相關`Department`實體。 `Course`實體具有`Department`導覽屬性。

EF 核心在模型具有相關實體的導覽屬性時，不需要 FK 屬性的資料模型。

在需要時會 EF 核心會自動在資料庫中建立 FKs。 建立 EF 核心[遮蔽屬性](https://docs.microsoft.com/ef/core/modeling/shadow-properties)FKs 自動建立的。 在資料模型中具有 FK 可以進行更新，更簡單且更有效率。 例如，假設模型其中 FK 屬性`DepartmentID`是*不*包含。 當課程實體會擷取編輯：

* `Department`實體為 null，如果未明確載入。
* 若要更新，在課程實體`Department`必須先擷取實體。

當 FK 屬性`DepartmentID`是否包含在資料模型中，就不必再擷取`Department`之前更新的實體。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 屬性

`[DatabaseGenerated(DatabaseGeneratedOption.None)]`屬性指定 PK 是應用程式提供，而不是產生的資料庫。

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

根據預設，EF 核心假設 PK 值由資料庫產生。 DB 產生 PK 值通常是最好的方法。 如`Course`實體，使用者指定 PK 例如，課程號碼例如 1000年一連串數學部門、 2000年一連串的英文版的部門。

`DatabaseGenerated`屬性也可以用來產生預設值。 例如，資料庫可以自動產生日期欄位來記錄資料列已建立或更新的日期。 如需詳細資訊，請參閱[產生屬性](https://docs.microsoft.com/ef/core/modeling/generated-properties)。

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵和導覽屬性

外部索引鍵 (FK) 屬性和導覽屬性中的`Course`實體反映的下列關聯性：

課程指派給一個部門，所以`DepartmentID`FK 和`Department`導覽屬性。

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

課程可以有任意數目的學生註冊，所以`Enrollments`導覽屬性是集合：

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

課程會教由多個講師，所以`CourseAssignments`導覽屬性是集合：

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment`說明[稍後](#many-to-many-relationships)。

## <a name="create-the-department-entity"></a>建立 Department 實體

![Department 實體](complex-data-model/_static/department-entity.png)

建立*Models/Department.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>資料行屬性

先前`Column`屬性用來變更資料行名稱的對應。 在程式碼`Department`實體，`Column`屬性用來變更 SQL 資料類型對應。 `Budget` DB 中使用 SQL Server money 類型來定義資料行：

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

資料行對應則通常不需要。 EF 核心通常會選擇適當屬性的 CLR 型別為基礎的 SQL Server 資料型別。 CLR`decimal`類型會對應至 SQL Server`decimal`型別。 `Budget`適用於貨幣和 money 資料類型為更適當的貨幣。

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵和導覽屬性

FK 和導覽屬性會反映的下列關聯性：

* 部門可能會或可能沒有系統管理員。
* 系統管理員永遠是講師。 因此`InstructorID`屬性做為包含要 FK`Instructor`實體。

導覽屬性名為`Administrator`但保留`Instructor`實體：

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

問號 （？），在上述程式碼中指定的屬性是可為 null。

部門可能具有許多課程，課程導覽屬性是：

```csharp
public ICollection<Course> Courses { get; set; }
```

注意： 依照慣例，EF 核心 可讓 cascade delete 不可為 null 的 FKs 和多對多關聯性。 串聯刪除可能會導致循環的串聯刪除規則。 循環的 cascade 加入移轉時，刪除規則原因例外狀況。

例如，如果`Department.InstructorID`屬性未定義為可為 null:

* EF 核心設定部門刪除時刪除講師串聯刪除規則。
* 刪除講師部門刪除時，不是預期的行為。

如果需要使用商務規則`InstructorID`屬性都不可為 null，使用下列 fluent API 陳述式：

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

上述程式碼會停用 cascade delete 部門講師關聯性。

## <a name="update-the-enrollment-entity"></a>更新註冊實體

註冊記錄是由一位學生採取一個課程。

![註冊實體](complex-data-model/_static/enrollment-entity.png)

更新*Models/Enrollment.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵和導覽屬性

FK 屬性和導覽屬性會反映下列關聯性：

註冊記錄是一個課程中，所以沒有`CourseID`FK 屬性和`Course`導覽屬性：

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

註冊記錄是一個學生，所以沒有`StudentID`FK 屬性和`Student`導覽屬性：

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>多對多關聯性

多對多關聯性之間`Student`和`Course`實體。 `Enrollment`實體做為多對多聯結資料表*裝載*資料庫中。 「 裝載 」 表示`Enrollment`資料表包含額外資料除了 FKs 聯結的資料表 (在此情況下，在 PK 和`Grade`)。

下圖顯示這些關聯性中的實體圖表的外觀。 (此圖表使用產生的 EF Power Tools 的 EF 6.x。 建立圖表不是本教學課程的一部分。）

![學生課程多對多關聯性](complex-data-model/_static/student-course.png)

每個關聯性線條在另一個對多關聯性，表示有一個結束作業，並以星號 （*） 1。

如果`Enrollment`資料表未包含等級資訊，它只需要包含兩個 FKs (`CourseID`和`StudentID`)。 多對多聯結資料表沒有裝載有時稱為純聯結資料表 (PJT)。

`Instructor`和`Course`實體具有使用純聯結資料表的多對多關聯性。

注意： EF 6.x 支援多對多關聯性，但 EF 核心隱含聯結資料表並不會。 如需詳細資訊，請參閱[多對多關聯性在 EF 核心 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/)。

## <a name="the-courseassignment-entity"></a>CourseAssignment 實體

![CourseAssignment 實體](complex-data-model/_static/courseassignment-entity.png)

建立*Models/CourseAssignment.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>講師-課程

![講師-課程 m:M](complex-data-model/_static/courseassignment.png)

講師-課程多對多關聯性：

* 需要聯結資料表，必須由實體集。
* 是一個純聯結資料表 （沒有內容的資料表）。

通常會命名為聯結實體`EntityName1EntityName2`。 例如，使用這個模式對講師-課程聯結資料表是`CourseInstructor`。 不過，我們建議使用可描述關聯性的名稱。

資料模型開始簡單及成長。 要包含裝載經常發展否裝載聯結 (PJTs)。 藉由啟動具有描述性的實體名稱，名稱不需要變更聯結資料表時變更。 理想情況下，聯結實體商務網域中必須有自己的自然 (可能是單一 word) 名稱。 例如，書籍客戶無法連結與聯結實體稱為評等。 講師-課程多對多關聯性，`CourseAssignment`最好透過`CourseInstructor`。

### <a name="composite-key"></a>複合索引鍵

FKs 不是可為 null。 在兩個 FKs `CourseAssignment` (`InstructorID`和`CourseID`) 一起唯一識別每個資料列的`CourseAssignment`資料表。 `CourseAssignment`不需要專用的 PK `InstructorID`和`CourseID`屬性當做複合 PK 若要指定複合 PKs 到 EF Core 的唯一方法是使用*fluent API*。 下節會說明如何設定複合 PK

複合索引鍵，以確保：

* 一個課程允許多個資料列。
* 一個講師允許多個資料列。
* 不允許多個資料列的相同講師和課程。

`Enrollment`聯結實體定義自己的 PK，因此可能會有這種重複的項目。 若要防止這類重複項目：

* FK 欄位上加入唯一索引或
* 設定`Enrollment`主複合索引鍵類似`CourseAssignment`。 如需詳細資訊，請參閱[索引](https://docs.microsoft.com/ef/core/modeling/indexes)。

## <a name="update-the-db-context"></a>更新 DB 內容

將下列反白顯示的程式碼加入*Data/SchoolContext.cs*:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

上述程式碼會新增新的實體，並設定`CourseAssignment`實體的複合 PK

## <a name="fluent-api-alternative-to-attributes"></a>Fluent API 替代項目屬性

`OnModelCreating`方法，在上述程式碼會使用*fluent API*設定 EF 核心行為。 API 稱為 「 fluent 應用程式開發，因為它通常由串連一系列的方法呼叫一起成單一陳述式。 [下列程式碼](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration)是 fluent 應用程式開發的應用程式開發介面的範例：

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

在本教學課程，fluent 應用程式開發的應用程式開發介面只適用於無法透過屬性的 DB 對應。 不過，fluent 應用程式開發的應用程式開發介面，可以指定大部分的格式設定、 驗證和對應規則，可以使用屬性。

某些屬性，例如`MinimumLength`fluent 應用程式開發的應用程式開發介面無法套用。 `MinimumLength`不會變更結構描述中，它只適用於最小長度驗證規則。

有些開發人員偏好使用 fluent 應用程式開發的應用程式開發介面，以獨佔方式，讓它們可保留他們的實體類別 「 清除 」。 可以混合屬性和 fluent 應用程式開發的應用程式開發介面。 有一些設定，才可使用 fluent API （指定複合 PK）。 有某些只能透過屬性的組態 (`MinimumLength`)。 使用 fluent 應用程式開發的應用程式開發介面或屬性的建議的做法：

* 選擇其中一個這兩種方法。
* 使用一致的方式盡量所選擇的方法。

有些用於這個屬性會用於教學課程：

* 驗證 (例如， `MinimumLength`)。
* 只有 EF 核心設定 (例如， `HasKey`)。
* 驗證和 EF 核心設定 (例如， `[StringLength(50)]`)。

如需 fluent API 與屬性的詳細資訊，請參閱[的組態方法](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration)。

## <a name="entity-diagram-showing-relationships"></a>實體圖表顯示關聯性

下圖顯示為已完成的 School 模型的 EF Power Tools 建立的圖表。

![實體圖表](complex-data-model/_static/diagram.png)

在上圖顯示：

* 數個一對多關聯性線條 (1 到\*)。
* 以 0 或-1 一個之間的關聯線 (1 對 0..1)`Instructor`和`OfficeAssignment`實體。
* 零-或--一對多關聯性線條 (0..1 對 *) 之間`Instructor`和`Department`實體。

## <a name="seed-the-db-with-test-data"></a>種子使用的測試資料的資料庫

更新中的程式碼*Data/DbInitializer.cs*:

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

上述程式碼會提供新的實體中的種子資料。 大部分的這段程式碼會建立新的實體物件，並載入資料範例。 範例資料用於測試。 上述程式碼會建立下列的多對多關聯性：

* `Enrollments`
* `CourseAssignment`

注意： [EF 核心 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap)將支援[資料植入](https://github.com/aspnet/EntityFrameworkCore/issues/629)。

## <a name="add-a-migration"></a>新增移轉

建置專案。 專案資料夾中開啟命令視窗並輸入下列命令：

```console
dotnet ef migrations add ComplexDataModel
```

上述命令會顯示有關可能遺失資料的警告。

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

如果`database update`執行命令時，會產生下列錯誤：

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

移轉作業執行時使用現有的資料，可能有不滿意的現有資料的 FK 條件約束。 此教學課程中，會建立新的資料庫，因此沒有 FK 條件約束違規。 請參閱[修正 foreign key 條件約束與傳統資料](#fk)如需如何在目前的 DB 修正 FK 違規的指示。

## <a name="change-the-connection-string-and-update-the-db"></a>變更連接字串，並更新資料庫

中已更新的程式碼`DbInitializer`加入新的實體的種子資料。 若要強制建立新的空白資料庫的 EF 核心：

* 資料庫連接字串中變更名稱*appsettings.json* ContosoUniversity3 至。 新的名稱必須是電腦未使用的名稱。

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* 或者，刪除資料庫的使用：

    * **SQL Server 物件總管**(SSOX)。
    * `database drop` CLI 命令：

   ```console
   dotnet ef database drop
   ```

執行`database update`命令視窗中：

```console
dotnet ef database update
```

上述命令會執行所有移轉。

執行應用程式。 執行應用程式執行`DbInitializer.Initialize`方法。 `DbInitializer.Initialize`填入新的 DB。

開啟 SSOX DB:

* 展開**資料表**節點。 會顯示建立的資料表。
* 如果先前已開啟 SSOX，按一下**重新整理** 按鈕。

![SSOX 中的資料表](complex-data-model/_static/ssox-tables.png)

檢查**CourseAssignment**資料表：

* 以滑鼠右鍵按一下**CourseAssignment**資料表，然後選取**檢視資料**。
* 確認**CourseAssignment**資料表包含資料。

![SSOX CourseAssignment 資料](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a>與傳統資料修正 foreign key 條件約束

此區段是選擇性的。

移轉作業執行時使用現有的資料，可能有不滿意的現有資料的 FK 條件約束。 使用生產資料，必須採取步驟來移轉現有的資料。 本節提供修正 FK 條件約束違規的範例。 不要變更這些程式碼不使用備份。 不要讓這些程式碼變更，如果您已完成上一節，並更新資料庫。

*{Timestamp}_ComplexDataModel.cs*檔案包含下列程式碼：

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

上述程式碼中加入不可為 null`DepartmentID`至 FK`Course`資料表。 包含資料列中的，從上一個教學課程 DB `Course`，因此無法更新移轉該資料表。

若要讓`ComplexDataModel`的現有資料的移轉工作：

* 變更程式碼，讓新的資料行 (`DepartmentID`) 預設值。
* 建立名為"Temp"做為預設部門假部門。

### <a name="fix-the-foreign-key-constraints"></a>修正 foreign key 條件約束

更新`ComplexDataModel`類別`Up`方法：

* 開啟*{timestamp}_ComplexDataModel.cs*檔案。
* 註解新增的程式碼行`DepartmentID`欄`Course`資料表。

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

加入下列反白顯示的程式碼。 新的程式碼會進入之後`.CreateTable( name: "Department"`區塊：[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

上述變更，現存`Course`將相關資料列之後的"Temp"部門`ComplexDataModel``Up`方法執行。

在生產應用程式會：

* 包含程式碼或指令碼，以新增`Department`的資料列和相關`Course`至新的資料列`Department`資料列。
* 不會使用"Temp"部門的預設值為`Course.DepartmentID `。

下一個教學課程涵蓋相關的資料。

>[!div class="step-by-step"]
[上一頁](xref:data/ef-rp/migrations)
[下一頁](xref:data/ef-rp/read-related-data)