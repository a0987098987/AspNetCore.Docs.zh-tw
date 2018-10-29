---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - 資料模型 - 5/8
author: rick-anderson
description: 在本教學課程中，請新增更多實體和關聯性，並透過指定格式、驗證和對應規則來自訂資料模型。
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: b81918cbd74200f0672f3002f916523fb4a9a914
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477653"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a>ASP.NET Core 中的 Razor 頁面與 EF Core - 資料模型 - 5/8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

先前的教學課程建立了基本的資料模型，該模型由三個實體組成。 在本教學課程中：

* 新增更多實體和關聯性。
* 藉由指定格式、驗證和資料庫對應規則來自訂資料模型。

完整資料模型的實體類別如下列圖例所示：

![實體圖表](complex-data-model/_static/diagram.png)

若您遇到無法解決的問題，請下載[完整應用程式](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)。

## <a name="customize-the-data-model-with-attributes"></a>使用屬性自訂資料模型

在本節中，您會使用屬性自訂資料模型。

### <a name="the-datatype-attribute"></a>DataType 屬性

學生頁面目前顯示了註冊日期的時間。 一般而言，日期欄位只會顯示日期，而非時間。

使用下列醒目提示的程式碼更新 *Models/Student.cs*：

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

[DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) 屬性會指定一個比資料庫內建類型更明確的資料類型。 在此情況下，該欄位應該只顯示日期，而不會同時顯示日期和時間。 [DataType 列舉](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1)提供了許多資料類型，例如　Date、Time、PhoneNumber、Currency、EmailAddress 等。`DataType` 屬性也可以讓應用程式自動提供限定於某些類型的功能。 例如: 

* `DataType.EmailAddress` 會自動建立 `mailto:` 連結。
* `DataType.Date` 在大多數的瀏覽器中都會提供日期選取器。

`DataType` 屬性會發出 HTML 5 `data-` (發音為 data dash) 屬性，可讓 HTML 5 瀏覽器取用。 `DataType` 屬性不會提供驗證。

`DataType.Date` 未指定顯示日期的格式。 根據預設，日期欄位會依照根據伺服器的 [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) 為基礎的預設格式顯示。

`DisplayFormat` 屬性用來明確指定日期格式：

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` 設定會指定格式也應套用在編輯 UI。 某些欄位不應該使用 `ApplyFormatInEditMode`。 例如，貨幣符號通常不應顯示在編輯文字方塊中。

`DisplayFormat` 屬性可由自身使用。 通常使用 `DataType` 屬性搭配 `DisplayFormat` 屬性是一個不錯的做法。 `DataType` 屬性會將資料的語意以其在螢幕上呈現方式的相反方式傳遞。 `DataType` 屬性提供了下列優點，並且這些優點在 `DisplayFormat` 中無法使用：

* 瀏覽器可以啟用 HTML5 功能。 例如，顯示日曆控制項、適合地區設定的貨幣符號、電子郵件連結、用戶端輸入驗證等。
* 根據預設，瀏覽器將根據地區設定，使用正確的格式呈現資料。

如需詳細資訊，請參閱 [\<input> 標籤協助程式文件](xref:mvc/views/working-with-forms#the-input-tag-helper)。

執行應用程式。 巡覽至 Students [索引] 頁面。 時間將不再顯示。 使用 `Student` 模型的每個檢視現在都只會顯示日期，而不會顯示時間。

![顯示日期而不顯示時間的 Students [索引] 頁面](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>StringLength 屬性

您可使用屬性指定資料驗證規則和驗證錯誤訊息。 [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) 屬性指定了在資料欄位中允許的最小及最大字元長度。 `StringLength` 屬性同時也提供了用戶端和伺服器端的驗證。 最小值對資料庫結構描述不會造成任何影響。

使用下列程式碼更新 `Student` 模型：

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

上述的程式碼會限制名稱不得超過 50 個字元。 `StringLength` 屬性不會防止使用者在名稱中輸入空白字元。 [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) 屬性可用於對輸入套用限制。 例如，下列程式碼會要求第一個字元必須是大寫，其餘字元則必須是英文字母：

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

執行應用程式：

* 巡覽至 Students 頁面。
* 選取 [新建]，然後輸入超過 50 個字元的名稱。
* 選取 [建立]，用戶端驗證便會顯示錯誤訊息。

![顯示字元長度錯誤的 Students [索引] 頁面](complex-data-model/_static/string-length-errors.png)

在 [SQL Server 物件總管] (SSOX) 中，按兩下 [Student] 資料表來開啟 Student 資料表設計工具。

![移轉之前於 SSOX 中的 Students 資料表](complex-data-model/_static/ssox-before-migration.png)

上述影像顯示了 `Student` 資料表的結構描述。 名稱欄位的類型為 `nvarchar(MAX)`，因為移轉尚未在資料庫中執行。 在本教學課程的稍後執行移轉後，名稱欄位便會成為 `nvarchar(50)`。

### <a name="the-column-attribute"></a>Column 屬性

可控制類別和屬性如何對應到資料庫的屬性。 在本節中，`Column` 屬性會用於將 `FirstMidName` 屬性的名稱對應到資料庫中的 "FirstName"。

當建立資料庫時，模型上的屬性名稱會用於資料行名稱 (除了使用 `Column` 屬性時之外)。

`Student` 模型針對名字欄位使用 `FirstMidName`，因為欄位中可能也會包含中間名。

使用下列醒目提示程式碼更新 *Student.cs* 檔案：

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

經過上述變更之後，應用程式中的 `Student.FirstMidName` 會對應到 `Student` 資料表的 `FirstName` 資料行。

新增 `Column` 屬性會變更支援 `SchoolContext` 的模型。 支援 `SchoolContext` 的模型不再符合資料庫。 若應用程式在套用移轉之前執行，便會產生下列例外狀況：

```SQL
SqlException: Invalid column name 'FirstName'.
```
若要更新資料庫：

* 建置專案。
* 在專案資料夾中開啟命令視窗。 輸入下列命令來建立新的移轉並更新資料庫：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

------

`migrations add ColumnFirstName` 命令會產生下列警告訊息：

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

產生警告的理由是名稱欄位現在限制長度為 50 個字元。 若在資料庫中有名稱超過 50 個字元，第 51 個字元到最後一個字元便會遺失。

* 測試應用程式。

在 SSOX 中開啟 Student 資料表：

![移轉之後 SSOX 中的 Students 資料表](complex-data-model/_static/ssox-after-migration.png)

在移轉套用之前，名稱資料行的類型為 [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)。 名稱資料行現在為 `nvarchar(50)`。 資料行的名稱已從 `FirstMidName` 變更為 `FirstName`。

> [!Note]
> 在下節中，在某些階段建置應用程式會產生編譯錯誤。 指令會指定何時應建置應用程式。

## <a name="student-entity-update"></a>Student 實體更新

![Student 實體](complex-data-model/_static/student-entity.png)

使用下列程式碼更新 *Models/Student.cs*：

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Required 屬性

`Required` 屬性會讓名稱屬性成為必要欄位。 針對不可為 Null 的類型，例如實值型別 (`DateTime`、`int`、`double` 等) 等，`Required` 屬性是不需要的。 不可為 Null 的類型會自動視為必要欄位。

`Required` 屬性可由 `StringLength` 屬性中的最小長度參數取代：

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Display 屬性

`Display` 屬性指定了文字方塊的標題應為「名字」、「姓氏」、「全名」及「註冊日期」。 預設標題中沒有使用空白分隔文字，例如「姓氏」。

### <a name="the-fullname-calculated-property"></a>FullName 計算屬性

`FullName` 為一個計算屬性，會傳回藉由串連兩個其他屬性而建立的值。 `FullName` 無法進行設定，其只有 get 存取子。 資料庫中不會建立 `FullName` 資料行。

## <a name="create-the-instructor-entity"></a>建立 Instructor 實體

![Instructor 實體](complex-data-model/_static/instructor-entity.png)

使用下列程式碼建立 *Models/Instructor.cs*：

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

在同一行上可以有多個屬性。 `HireDate` 屬性可以下列方式撰寫：

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>CourseAssignments 和 OfficeAssignment 導覽屬性

`CourseAssignments` 和 `OfficeAssignment` 屬性為導覽屬性。

由於講師可以教授任何數量的課程，因此 `CourseAssignments` 已定義為一個集合。

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

若導覽屬性中保留了多個實體：

* 它必須是一種清單類型，可對其中的實體進行新增、刪除和更新。

導覽屬性類型包括：

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

若已指定 `ICollection<T>`，EF Core 會根據預設建立一個 `HashSet<T>` 集合。

`CourseAssignment` 實體會在本節的多對多關聯性中解釋。

Contoso 大學商務規則訂定了講師最多只能有一間辦公室。 `OfficeAssignment` 屬性保留了一個單一 `OfficeAssignment` 實體。 若沒有指派任何辦公室，則 `OfficeAssignment` 為 Null。

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>建立 OfficeAssignment 實體

![OfficeAssignment 實體](complex-data-model/_static/officeassignment-entity.png)

使用下列程式碼建立 *Models/OfficeAssignment.cs*：

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Key 屬性

`[Key]` 屬性用於將某個屬性名稱不是 classnameID 或 ID 的屬性識別為主索引鍵 (PK)。

在 `Instructor` 和 `OfficeAssignment` 實體之間有一對零或一關聯性。 辦公室指派只存在於與其受指派的講師關聯中。 `OfficeAssignment` PK 同時也是其連結到 `Instructor` 實體的外部索引鍵 (FK)。 EF Core 無法自動將 `InstructorID` 識別為 `OfficeAssignment` 的主索引鍵，因為：

* `InstructorID` 並未遵循 ID 或 classnameID 的命名慣例。

因此，必須使用 `Key` 屬性將 `InstructorID` 識別為 PK：

```csharp
[Key]
public int InstructorID { get; set; }
```

根據預設，EF Core 會將索引鍵作為非資料庫產生的屬性處置，因為該資料行主要用於識別關聯性。

### <a name="the-instructor-navigation-property"></a>Instructor 導覽屬性

`Instructor` 實體的 `OfficeAssignment` 導覽屬性可為 Null，因為：

* 參考型別 (例如類別可為 Null)。
* 講師可能沒有辦公室指派。


`OfficeAssignment` 實體有不可為 Null 的`Instructor` 導覽屬性，因為：

* `InstructorID` 不可為 Null。
* 辦公室指派無法獨立於講師之外存在。

當 `Instructor` 實體有相關的 `OfficeAssignment` 實體時，每一個實體都會在其導覽屬性中包含一個其他實體的參考。

`[Required]` 屬性可套用到 `Instructor` 導覽屬性：

```csharp
[Required]
public Instructor Instructor { get; set; }
```

上述程式碼指定了必須要有相關的講師。 上述程式碼是不必要的，因為 `InstructorID` 外部索引鍵 (同時也是 PK) 不可為 Null。

## <a name="modify-the-course-entity"></a>修改 Course 實體

![Course 實體](complex-data-model/_static/course-entity.png)

使用下列程式碼更新 *Models/Course.cs*：

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

`Course` 實體具有外部索引鍵 (FK) 屬性`DepartmentID`。 `DepartmentID` 會指向相關的 `Department` 實體。 `Course` 實體具有一個 `Department` 導覽屬性。

當資料模型針對相關實體有一個導覽屬性時，EF Core 不需要針對該模型具備 FK 屬性。

EF Core 會自動在資料庫中需要的任何地方建立 FK。 EF Core 會為了自動建立的 FK 建立[陰影屬性](https://docs.microsoft.com/ef/core/modeling/shadow-properties)。 在資料模型中具備 FK 可讓更新變得更為簡單和有效率。 例如，假設有一個模型，當中「不」包含 `DepartmentID` FK 屬性。 當擷取課程實體以進行編輯時：

* 若沒有明確載入，`Department` 實體將為 Null。
* 若要更新課程實體，必須先擷取 `Department` 實體。

當 FK 屬性 `DepartmentID` 包含在資料模型中時，便不需要在更新前擷取 `Department` 實體。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 屬性

`[DatabaseGenerated(DatabaseGeneratedOption.None)]` 屬性會指定 PK 是由應用程式提供的，而非資料庫產生的。

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

根據預設，EF Core 會假設 PK 值都是由資料庫產生的。 由資料庫產生 PK 值通常都是最佳做法。 針對 `Course` 實體，使用者指定了 PK。 例如，課程號碼 1000 系列表示數學部門的課程，2000 系列則為英文部門的課程。

`DatabaseGenerated` 屬性也可用於產生預設值。 例如，資料庫會自動產生日期欄位來記錄資料列建立或更新的日期。 如需詳細資訊，請參閱[產生的屬性](https://docs.microsoft.com/ef/core/modeling/generated-properties)。

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵及導覽屬性

`Course` 實體中的外部索引鍵 (FK) 屬性和導覽屬性反映了下列關聯性：

由於一個課程已指派給了一個部門，因此當中具有 `DepartmentID` FK 和 `Department` 導覽屬性。

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

由於課程可由任何數量的學生進行註冊，因此 `Enrollments` 導覽屬性為一個集合：

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

課程可由多個講師進行教授，因此 `CourseAssignments` 導覽屬性為一個集合：

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment` 會在[稍後](#many-to-many-relationships)進行解釋。

## <a name="create-the-department-entity"></a>建立 Department 實體

![Department 實體](complex-data-model/_static/department-entity.png)

使用下列程式碼建立 *Models/Department.cs*：

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Column 屬性

先前，`Column` 屬性主要用於變更資料行的名稱對應。 在 `Department` 實體的程式碼中，`Column` 屬性則用於變更 SQL 資料類型對應。 `Budget` 資料行則是使用資料庫中的 SQL Server 金額類型定義：

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

通常您不需要資料行對應。 EF Core 通常會根據屬性的 CLR 類型選擇適當的 SQL Server 資料類型。 CLR `decimal` 類型會對應到 SQL Server `decimal` 類型。 由於 `Budget` 是貨幣，因此金額資料類型會比較適合貨幣。

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵及導覽屬性

FK 及導覽屬性反映了下列關聯性：

* 部門可能有或可能沒有系統管理員。
* 系統管理員一律為講師。 因此，`InstructorID` 已作為 FK 包含在 `Instructor` 實體中。

導覽屬性已命名為 `Administrator`，但其中保留了一個 `Instructor` 實體：

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

上述程式碼中的問號 (?) 表示屬性可為 Null。

部門中可能包含許多課程，因此當中包含了一個 Course 導覽屬性：

```csharp
public ICollection<Course> Courses { get; set; }
```

注意：根據慣例，EF Core 會為不可為 Null 的 FK 和多對多關聯性啟用串聯刪除。 串聯刪除可能會導致循環的串聯刪除規則。 循環串聯刪除規則會在新增移轉時造成例外狀況。

例如，若 `Department.InstructorID` 屬性並未定義為可為 Null：

* EF Core 會在部門遭到刪除時設定串聯刪除規則，以刪除講師。
* 在刪除部門時刪除講師並非預期的行為。

若商務規則要求 `InstructorID` 屬性為不可為 Null，則請使用下列 Fluent API 陳述式：

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

上述的程式碼會在部門-講師關聯性上停用串聯刪除。

## <a name="update-the-enrollment-entity"></a>更新 Enrollment 實體

註冊記錄是某位學生參加的一門課程。

![Enrollment 實體](complex-data-model/_static/enrollment-entity.png)

使用下列程式碼更新 *Models/Enrollment.cs*：

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵及導覽屬性

FK 屬性及導覽屬性反映了下列關聯性：

註冊記錄乃針對一個課程，因此當中包含了一個 `CourseID` FK 屬性及一個 `Course` 導覽屬性：

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

註冊記錄乃針對一位學生，因此當中包含了一個 `StudentID` FK 屬性及一個 `Student` 導覽屬性：

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>多對多關聯性

在 `Student` 和 `Course` 實體之間存在一個多對多關聯性。 `Enrollment` 實體的功能為資料庫中一個「具有承載」的多對多聯結資料表。 「具有承載」表示 `Enrollment` 資料表除了聯結資料表 (在此案例中為 PK 和 `Grade`) 的 FK 之外，還包含了額外的資料。

下列圖例展示了在實體圖表中這些關聯性的樣子。 (此圖表是使用 EF 6.x 的 [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) 產生的。 建立圖表並不是此教學課程的一部分)。

![學生-課程多對多關聯性](complex-data-model/_static/student-course.png)

每個關聯性線條都在其中一端有一個「1」，並在另外一端有一個「星號 (*)」，顯示其為一對多關聯性。

若 `Enrollment` 資料表並未包含成績資訊，則其便只需要包含兩個 FK (`CourseID` 和 `StudentID`)。 沒有承載的多對多聯結資料表有時候也稱為「純聯結資料表 (PJT)」。

`Instructor` 和 `Course` 實體具有使用了純聯結資料表的多對多關聯性。

注意：EF 6.x 支援多對多關聯性的隱含聯結資料表，但 EF Core 並不支援。 如需詳細資訊，請參閱 [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/) (EF Core 2.0 中的多對多關聯性)。

## <a name="the-courseassignment-entity"></a>CourseAssignment 實體

![CourseAssignment 實體](complex-data-model/_static/courseassignment-entity.png)

使用下列程式碼建立 *Models/CourseAssignment.cs*：

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>講師-課程

![講師-課程 m:M](complex-data-model/_static/courseassignment.png)

講師-課程多對多關聯性：

* 需要一個由實體集代表的聯結資料表。
* 為一個純聯結資料表 (沒有承載的資料表)。

通常會將聯結實體命名為 `EntityName1EntityName2`。 例如，使用此模式的講師-課程聯結資料表為 `CourseInstructor`。 不過，我們建議使用可描述關聯性的名稱。

資料模型一開始都是簡單的，之後便會持續成長。 無承載聯結 (PJT) 常常會演變為包含承載。 藉由在一開始便使用描述性的實體名稱，當聯結資料表變更時便不需要變更名稱。 理想情況下，聯結實體在公司網域中會有自己的自然 (可能為一個單字) 名稱。 例如，「書籍」和「客戶」可連結為一個名為「評分」的聯結實體。 針對講師-課程多對多關聯性，`CourseAssignment` 會比 `CourseInstructor` 來得好。

### <a name="composite-key"></a>複合索引鍵

FK 不可為 Null。 `CourseAssignment` (`InstructorID` 及 `CourseID`) 中的兩個 FK 一起搭配使用，便可唯一的識別 `CourseAssignment` 資料表中的每一個資料列。 `CourseAssignment` 並不需要其專屬的 PK。 `InstructorID` 及 `CourseID` 屬性的功能便是複合 PK。 為 EF Core 指定複合 PK 的唯一方法是使用 *Fluent API*。 下節會說明如何設定複合 PK。

複合 PK 可確保：

* 一個課程可以有多個資料列。
* 一個講師可以有多個資料列。
* 相同講師和課程不可有多個資料列。

由於 `Enrollment` 聯結實體定義了其自身的 PK，因此這種種類的重複項目是可能的。 若要防止這類重複項目：

* 在 FK 欄位中新增一個唯一的索引，或
* 為 `Enrollment` 設定一個主複合索引鍵，與 `CourseAssignment` 相似。 如需詳細資訊，請參閱[索引](https://docs.microsoft.com/ef/core/modeling/indexes)。

## <a name="update-the-db-context"></a>更新資料庫內容

將下列醒目提示的程式碼新增至 *Data/SchoolContext.cs*：

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

上述程式碼會新增一個新實體，並設定 `CourseAssignment` 實體的複合 PK。

## <a name="fluent-api-alternative-to-attributes"></a>屬性的 Fluent API 替代項目

上述程式碼中的 `OnModelCreating` 方法使用了 *Fluent API* 設定 EF Core 行為。 此 API 稱為 "fluent" ，因為其常常會用於將一系列的方法呼叫串在一起，使其成為一個單一陳述式。 [下列程式碼](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration)為 Fluent API 的其中一個範例：

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

在此教學課程中，Fluent API 僅會用於無法使用屬性完成的資料庫對應。 然而，Fluent API 可指定大部分透過屬性可完成的格式、驗證及對應規則。

某些屬性 (例如 `MinimumLength`) 無法使用 Fluent API 來套用。 `MinimumLength` 不會變更結構描述。它只會套用一項最小長度驗證規則。

某些開發人員偏好單獨使用 Fluent API，使其實體類別保持「整潔」。 屬性和 Fluent API 可混合使用。 有一些設定只能透過 Fluent API 完成 (指定複合 PK)。 有一些設定只能透過屬性完成 (`MinimumLength`)。 使用 Fluent API 或屬性的建議做法為：

* 從這兩種方法中選擇一項。
* 持續且盡量使用您選擇的方法。

此教學課程中使用到的某些屬性主要用於：

* 僅驗證 (例如，`MinimumLength`)。
* 僅 EF Core 組態 (例如，`HasKey`)。
* 驗證及 EF Core 組態 (例如，`[StringLength(50)]`)。

如需屬性與 Fluent API 的詳細資訊，請參閱[方法](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration)。

## <a name="entity-diagram-showing-relationships"></a>顯示關聯性的實體圖表

下圖顯示了 EF Power Tools 為完成的 School 模型建立的圖表。

![實體圖表](complex-data-model/_static/diagram.png)

上述圖表顯示：

* 數個一對多關聯性線條 (1 對 \*)。
* `Instructor` 和 `OfficeAssignment` 實體之間的一對零或一關聯性線條 (1 對 0..1)。
* `Instructor` 和 `Department` 實體之間的零或一對多關聯性線條 (0..1 對 *)。

## <a name="seed-the-db-with-test-data"></a>使用測試資料植入資料庫

更新 *Data/DbInitializer.cs* 中的程式碼：

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

上述程式碼為新的實體提供了種子資料。 此程式碼中的大部分主要用於建立新的實體物件並載入範例資料。 範例資料主要用於測試。 上述程式碼會建立下列多對多關聯性：

* `Enrollments`
* `CourseAssignment`

## <a name="add-a-migration"></a>新增移轉

建置專案。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet ef migrations add ComplexDataModel
```

------

上述命令會顯示關於可能發生資料遺失的警告。

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

若執行 `database update` 命令，便會產生下列錯誤：

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a>套用移轉

現在您有了現有的資料庫，您需要思考如何對其套用未來變更。 本教學課程示範兩種方法：
* [卸除並重新建立資料庫](#drop)
* [將移轉套用至現有資料庫](#applyexisting)。 雖然這個方法更複雜且耗時，卻是實際生產環境的慣用方法。 **請注意**：這是本教學課程的選擇性章節。 您可以執行卸除並重新建立步驟，然後略過本節。 如果您希望遵循本章節中的步驟，請不要執行卸除並重新建立的步驟。 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a>卸除並重新建立資料庫

更新的 `DbInitializer` 中的程式碼會為新的實體新增種子資料。 若要強制 EF Core 建立新的資料庫，請卸除並更新資料庫：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

在 [套件管理員主控台] (PMC) 中，執行下列命令：

```PMC
Drop-Database
Update-Database
```

從 PMC 執行 `Get-Help about_EntityFrameworkCore` 以取得說明資訊。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

開啟命令視窗並巡覽至專案資料夾。 專案資料夾中包含 *Startup.cs* 檔案。

在命令視窗中輸入下列命令：

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

------

執行應用程式。 執行應用程式會執行 `DbInitializer.Initialize` 方法。 `DbInitializer.Initialize` 會填入新的資料庫。

在 SSOX 中開啟資料庫：

* 若先前已開啟過 SSOX，按一下 [重新整理] 按鈕。
* 展開 [資料表] 節點。 建立的資料表便會顯示。

![SSOX 中的資料表](complex-data-model/_static/ssox-tables.png)

檢查 **CourseAssignment** 資料表：

* 以滑鼠右鍵按一下 **CourseAssignment** 資料表，然後選取 [檢視資料]。
* 驗證 **CourseAssignment** 資料表中是否包含資料。

![SSOX 中的 CourseAssignment 資料](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a>將移轉套用至現有資料庫

本節為選擇性。 只有當您略過先前[卸除並重新建立資料庫](#drop)一節，這些步驟才有效。

當使用現有的資料執行移轉作業時，某些 FK 條件約束可能會無法透過現有資料滿足。 當您使用的是生產資料時，您必須進行幾個步驟才能移轉現有資料。 本節提供了修正 FK 條件約束違規的範例。 請不要在沒有備份的情況下進行這些程式碼變更。 若您已完成了先前的章節並已更新資料庫，請不要進行這些程式碼變更。

*{timestamp}_ComplexDataModel.cs* 檔案包含了下列程式碼：

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

上述程式碼將一個不可為 Null 的 `DepartmentID` FK 新增至 `Course` 資料表。 先前教學課程中的資料庫在 `Course` 中包含了資料列，導致資料表無法藉由移轉進行更新。

若要讓 `ComplexDataModel` 與現有資料進行移轉：

* 變更程式碼，以給予新資料行 (`DepartmentID`) 一個新的預設值。
* 建立一個名為 "Temp" 的假部門以作為預設部門之用。

#### <a name="fix-the-foreign-key-constraints"></a>修正外部索引鍵條件約束

更新 `ComplexDataModel` 類別 `Up` 方法：

* 開啟 *{timestamp}_ComplexDataModel.cs* 檔案。
* 將新增 `DepartmentID` 資料行至 `Course` 資料表的程式碼全部標為註解。

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

新增下列醒目提示程式碼。 新的程式碼應位於 `.CreateTable( name: "Department"` 區塊之後：[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

藉由上述的變更，現有的 `Course` 資料列便會在執行 `ComplexDataModel``Up` 方法後與 "Temp" 部門產生關聯。

生產環境的應用程式會：

* 包含將 `Department` 資料列及相關 `Course` 資料列新增到新 `Department` 資料列的程式碼或指令碼。
* 不使用 "Temp" 部門或 `Course.DepartmentID` 的預設值。

下一個教學課程會涵蓋相關資料。

::: moniker-end

> [!div class="step-by-step"]
> [上一頁](xref:data/ef-rp/migrations)
> [下一頁](xref:data/ef-rp/read-related-data)
