---
title: 教學課程：建立複雜的資料模型-使用 EF Core ASP.NET MVC
description: 在本教學課程中，請新增更多實體和關聯性，並透過指定格式、驗證和對應規則來自訂資料模型。
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: 91fd09874ecab8bfdb6a38a404faba04aeb73edc
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657427"
---
# <a name="tutorial-create-a-complex-data-model---aspnet-mvc-with-ef-core"></a>教學課程：建立複雜的資料模型-使用 EF Core ASP.NET MVC

在先前的教學課程中，您建立了由三個實體組成的簡單資料模型。 在本教學課程中，您會新增更多實體和關聯性，並透過指定格式、驗證和資料庫對應規則來自訂資料模型。

當您完成時，實體類別會構成如下列圖例中所顯示的完整資料模型：

![實體圖表](complex-data-model/_static/diagram.png)

在本教學課程中，您：

> [!div class="checklist"]
> * 自訂資料模型
> * 對 Student 實體進行變更
> * 建立 Instructor 實體
> * 建立 OfficeAssignment 實體
> * 修改 Course 實體
> * 建立 Department 實體
> * 修改 Enrollment 實體
> * 更新資料庫內容
> * 將測試資料植入資料庫
> * 新增移轉
> * 變更連接字串
> * 更新資料庫

## <a name="prerequisites"></a>Prerequisites

* [使用 EF Core 移轉](migrations.md)

## <a name="customize-the-data-model"></a>自訂資料模型

在本節中，您會了解到如何使用指定格式、驗證和資料庫對應規則的屬性來自訂資料模型。 然後在下列幾個章節中，您會透過將屬性新增到您已建立的類別，以及為模型中剩餘的實體類型建立新的類別，來建立完整的 School 資料模型。

### <a name="the-datatype-attribute"></a>DataType 屬性

針對學生的註冊日期，所有網頁目前都會同時顯示時間和日期，即使您針對此欄位只需要日期而已。 使用資料註解屬性，您可以透過僅對一個程式碼進行變更，來修正每個顯示資料的檢視上的顯示格式。 為了查看如何進行此操作的範例，您將會新增一個屬性至 `EnrollmentDate` 類別中的 `Student` 屬性。

在 *Models/Student.cs* 中，為 `using` 命名空間新增一個 `System.ComponentModel.DataAnnotations` 陳述式，然後將 `DataType` 和 `DisplayFormat` 屬性新增到 `EnrollmentDate` 屬性，如下列範例所示：

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

`DataType` 屬性可用於指定比資料庫內建類型更特定的資料類型。 在此案例中，我們只想要追蹤日期，而非日期和時間。 `DataType` 列舉提供了許多資料類型，例如　Date、Time、PhoneNumber、Currency、EmailAddress 等。 `DataType` 屬性也可讓應用程式自動提供類型的特定功能。 例如，可建立 `mailto:` 的 `DataType.EmailAddress` 連結，而且可以在支援 HTML5 的瀏覽器中提供 `DataType.Date` 的日期選擇器。 `DataType` 屬性會發出 HTML 5 瀏覽器了解的 HTML 5 `data-` (讀音為 data dash) 屬性。 `DataType` 屬性不會提供任何驗證。

`DataType.Date` 未指定顯示日期的格式。 根據預設，資料欄位會依照根據伺服器的 CultureInfo 為基礎的預設格式顯示。

`DisplayFormat` 屬性用來明確指定日期格式：

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` 設定可指定在文字方塊中顯示值以供編輯時，也應該套用的格式。 (您也許會不想讓它出現在某些欄位中 - 例如貨幣值，您可能會不希望在進行編輯的文字方塊中出現貨幣符號。)

您可單獨使用 `DisplayFormat` 屬性，但通常最好是也使用 `DataType` 屬性。 `DataType` 屬性會傳逹資料的語意，而不是在螢幕上呈現資料的方式，並提供下列使用 `DisplayFormat` 無法取得的優點：

* 瀏覽器可以啟用 HTML5 功能 (例如顯示日曆控制項、適合地區設定的貨幣符號、電子郵件連結、一些用戶端輸入驗證等等)。

* 根據預設，瀏覽器將根據您的地區設定，使用正確的格式呈現資料。

如需詳細資訊，請參閱 [\<input> 標籤協助程式文件](../../mvc/views/working-with-forms.md#the-input-tag-helper)。

執行應用程式，移至 Students [索引] 頁面，您會注意到註冊日期欄位中不再顯示時間。 任何使用 Student 模型的檢視都會有相同的結果。

![顯示日期而不顯示時間的 Students [索引] 頁面](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>StringLength 屬性

您也可以使用屬性指定資料驗證規則和驗證錯誤訊息。 `StringLength` 屬性會設定資料庫中的最大長度，並為 ASP.NET Core MVC 提供用戶端和伺服器端驗證。 您也可以在此屬性中指定最小字串長度，但最小值不會對資料庫結構描述造成任何影響。

假設您想要確保使用者不會在名稱中輸入超過 50 個字元。 若要新增這項限制，請將 `StringLength` 屬性新增到 `LastName` 及 `FirstMidName` 屬性，如下列範例所示：

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

`StringLength` 屬性不會防止使用者在名稱中輸入空白字元。 您可以使用 `RegularExpression` 屬性來將限制套用至輸入。 例如，下列程式碼會要求第一個字元必須是大寫，其餘字元則必須是英文字母：

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

`MaxLength` 屬性提供了與 `StringLength` 屬性類似的功能，但不會提供用戶端驗證。

資料庫模型現已變更，需要變更資料庫結構描述。 您會使用移轉來更新結構描述，而不會遺失任何您可能已透過應用程式 UI 新增到資料庫中的資料。

儲存您的變更，並建置專案。 然後，在專案資料夾中開啟命令視窗，並輸入下列命令：

```dotnetcli
dotnet ef migrations add MaxLengthOnNames
```

```dotnetcli
dotnet ef database update
```

`migrations add` 命令會警告可能發生資料遺失，因為該項變更縮短了兩個資料行的最大長度。  移轉會建立名為 *時間戳記 >_MaxLengthOnNames.cs\<* 的檔案。 此檔案包含了 `Up` 方法中的程式碼，可更新資料庫，使其符合目前的資料模型。 `database update` 命令執行了該程式碼。

Entity Framework 會使用移轉檔案名稱前置的時間戳記來排序移轉。 您可以在執行 update-database 命令前建立多個移轉，然後所有的移轉便會依照其建立的先後順序套用。

請執行應用程式、選取 [Students] 索引標籤、按一下 [建立新項目]，然後嘗試輸入長度超過 50 個字元的名稱。 應用程式應該會防止您這麼做。 

### <a name="the-column-attribute"></a>Column 屬性

您也可以使用屬性控制您的類別和屬性對應到資料庫的方式。 假設您已針對名字欄位使用 `FirstMidName` 作為名稱，因為欄位中可能也會包含中間名。 但您想要將資料庫資料行命名為 `FirstName`，因為撰寫臨機操作查詢資料庫的使用者比較習慣該名稱。 若要進行此對應，您可以使用 `Column` 屬性。

`Column` 屬性指定當建立資料庫時，`Student` 資料表中對應到 `FirstMidName` 屬性的資料行會命名為 `FirstName`。 換句話說，當您的程式碼參照 `Student.FirstMidName` 時，資料便會來自 `FirstName` 資料表中的 `Student` 資料行或在其中更新。 若您沒有指定資料行名稱，則它們便會命名為與屬性名稱相同的名稱。

在 *Student.cs* 檔案中，為 `using` 新增一個 `System.ComponentModel.DataAnnotations.Schema` 陳述式，然後將資料行名稱屬性新增到 `FirstMidName` 屬性，如下列醒目提示程式碼中所示：

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

新增 `Column` 屬性會變更支援 `SchoolContext` 的模型，因此它將不會符合資料庫。

儲存您的變更，並建置專案。 然後，在專案資料夾中開啟命令視窗，並輸入下列命令以建立另一個移轉：

```dotnetcli
dotnet ef migrations add ColumnFirstName
```

```dotnetcli
dotnet ef database update
```

在 [SQL Server 物件總管] 中，按兩下 [Student] 資料表來開啟 Student 資料表設計工具。

![移轉之後 SSOX 中的 Students 資料表](complex-data-model/_static/ssox-after-migration.png)

在您套用前兩個移轉前，名稱資料行的類型為 nvarchar(MAX)。 它們現在已是 nvarchar (50)，並且資料行名稱也已從 FirstMidName 變更為 FirstName。

> [!Note]
> 若您嘗試在完成建立下列章節中所有的實體類別前進行編譯，您可能會收到編譯器錯誤。

## <a name="changes-to-student-entity"></a>對 Student 實體進行變更

![Student 實體](complex-data-model/_static/student-entity.png)

在 *Models/Student.cs* 中，以下列程式碼取代您在先前新增的程式碼。 所做的變更已醒目標示。

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Required 屬性

`Required` 屬性會讓名稱屬性成為必要欄位。 針對不可為 Null 的類型，例如實值型別 (DateTime、int、double、float 等) 等，`Required` 屬性是不需要的。 不可為 Null 的類型會自動視為必要欄位。

`Required` 屬性必須搭配 `MinimumLength` 使用，才能強制執行 `MinimumLength`。

```csharp
[Display(Name = "Last Name")]
[Required]
[StringLength(50, MinimumLength=2)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Display 屬性

`Display` 屬性指定了文字方塊的標題應為「名字」、「姓氏」、「全名」及「註冊日期」，而非每個執行個體中的屬性名稱 (沒有使用空格鍵分隔單字的名稱)。

### <a name="the-fullname-calculated-property"></a>FullName 計算屬性

`FullName` 為一個計算屬性，會傳回藉由串連兩個其他屬性而建立的值。 因此其僅具有 get 存取子，且資料庫中不會產生 `FullName` 資料行。

## <a name="create-instructor-entity"></a>建立 Instructor 實體

![Instructor 實體](complex-data-model/_static/instructor-entity.png)

建立 *Models/Instructor.cs* 並以下列程式碼取代範本程式碼：

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

請注意，Student 和 Instructor 實體中有幾個屬性是一樣的。 在本系列稍後的[實作繼承](inheritance.md)教學課程中，您會對此程式碼進行重構以消除冗餘。

您也可以將多個屬性放在同一行上，並以下列方式撰寫 `HireDate` 屬性：

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>CourseAssignments 和 OfficeAssignment 導覽屬性

`CourseAssignments` 和 `OfficeAssignment` 屬性為導覽屬性。

由於講師可以教授任何數量的課程，因此 `CourseAssignments` 已定義為一個集合。

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

若導覽屬性可保有多個實體，其類型必須為一個清單，使得實體可以在該清單中新增、刪除或更新。  您可以指定 `ICollection<T>` 或如 `List<T>` 或 `HashSet<T>` 等類型。 若您指定了 `ICollection<T>`，EF 會根據預設建立一個 `HashSet<T>` 集合。

這些之所以為 `CourseAssignment` 實體的原因會在此節下方關於多對多關聯性中解釋。

Contoso 大學的商務規則要求講師最多只能擁有一間辦公室，因此 `OfficeAssignment` 屬性會保有單一 OfficeAssignment 實體 (若沒有指派任何辦公室，則其可為 Null)。

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-officeassignment-entity"></a>建立 OfficeAssignment 實體

![OfficeAssignment 實體](complex-data-model/_static/officeassignment-entity.png)

使用下列程式碼建立 *Models/OfficeAssignment.cs*：

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Key 屬性

在 Instructor 和 OfficeAssignment 實體之間存在一對零或一關聯性。 辦公室指派只有在有指派的講師時才會存在，因此其主索引鍵也是其繫結到 Instructor 實體的外部索引鍵。 但 Entity Framework 無法自動將 InstructorID 識別為此實體的主索引鍵，因為其名稱並未遵循 ID 或 classnameID 命名慣例。 因此，必須使用 `Key` 屬性將其識別為 PK：

```csharp
[Key]
public int InstructorID { get; set; }
```

若實體確實有其自身的主索引鍵，但您想要將該屬性命名為其他名稱，而非 classnameID 或 ID，則您也可以使用 `Key` 屬性。

根據預設，EF 會將索引鍵作為非資料庫產生的屬性處置，因為該資料行主要用於識別關聯性。

### <a name="the-instructor-navigation-property"></a>Instructor 導覽屬性

Instructor 實體具有一個可為 Null 的 `OfficeAssignment` 導覽屬性 (因為一名講師可能會沒有任何辦公室指派)，而 OfficeAssignment 實體則有一個不可為 Null 的 `Instructor` 導覽屬性 (因為辦公室指派不可獨立於講師之外存在 - `InstructorID` 不可為 Null)。 當 Instructor 實體有相關的 OfficeAssignment 實體時，每一個實體都會在其導覽屬性中包含一個其他實體的參考。

您可以將 `[Required]` 屬性置於 Instructor 導覽屬性上，以指定其必須要有一個相關的講師，但您不需要這麼做，因為 `InstructorID` 外部索引鍵 (同時也是此資料表的索引鍵) 不可為 Null。

## <a name="modify-course-entity"></a>修改 Course 實體

![Course 實體](complex-data-model/_static/course-entity.png)

在 *Models/Course.cs* 中，以下列程式碼取代您在先前新增的程式碼。 所做的變更已醒目標示。

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

課程實體有一個外部索引鍵屬性 (`DepartmentID`)，該索引鍵指向了相關的 Department 實體，並且其擁有一個 `Department` 導覽屬性。

當您針對相關實體具有一個導覽屬性時，Entity Framework 便不需要您為資料模型新增一個外部索引鍵屬性。  每當需要的時候，EF 便會自動在資料庫中建立外部索引鍵，並為他們建立[陰影屬性](/ef/core/modeling/shadow-properties)。 但在資料模型中擁有外部索引鍵，可讓更新變得更為簡單和有效率。 例如，當您擷取了一個要編輯的課程實體，若您沒有載入它，Department 實體便會為 Null，因此當您要更新課程實體時，您必須先擷取 Department 實體。 當外部索引鍵屬性 `DepartmentID` 包含在資料模型中時，您便不需要在更新前擷取 Department 實體。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 屬性

`DatabaseGenerated` 屬性和 `None` 屬性上的 `CourseID` 參數指定主索引鍵由使用者提供，而非由資料庫產生。

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

根據預設，Entity Framework 會假設主索引鍵的值是由資料庫產生。 這是您在大多數案例下所希望的情況。 然而，針對 Course 實體，您會使用使用者定義的課程號碼，例如讓一個部門使用 1000 系列，另一個部門則使用 2000 系列等等。

如果是用於記錄資料列建立或更新的資料庫資料行，則 `DatabaseGenerated` 屬性也能用於產生預設值。  如需詳細資訊，請參閱[產生的屬性](/ef/core/modeling/generated-properties)。

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵及導覽屬性

Course 實體中的外部索引鍵屬性和導覽屬性反映了下列關聯性：

課程會指派給一個部門，因此基於上述理由，會有一個 `DepartmentID` 外部索引鍵和一個 `Department` 導覽屬性。

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

由於課程可由任何數量的學生進行註冊，因此 `Enrollments` 導覽屬性為一個集合：

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

課程可由多個講師進行教授，因此 `CourseAssignments` 導覽屬性為一個集合 (`CourseAssignment` 類型會在[稍後](#many-to-many-relationships)說明)：

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-department-entity"></a>建立 Department 實體

![部門實體](complex-data-model/_static/department-entity.png)

使用下列程式碼建立 *Models/Department.cs*：

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Column 屬性

先前您使用了 `Column` 屬性來變更資料行的名稱對應。 在 Department 實體的程式碼中，`Column` 屬性則會用於變更 SQL 資料類型對應，讓資料行在資料庫中會使用 SQL Server 的貨幣類型進行定義：

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

資料行對應通常都是不必要的，因為 Entity Framework 會根據您為該屬性定義的 CLR 類型選擇適合的 SQL Server 資料類型。 CLR `decimal` 類型會對應到 SQL Server `decimal` 類型。 但在此案例下，您知道資料行會儲存貨幣數量，而貨幣資料類型是最適合的。

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵及導覽屬性

外部索引鍵及導覽屬性反映了下列關聯性：

部門可以有或沒有一位系統管理員，而系統管理員一律為講師。 因此 `InstructorID` 屬性會作為繫結到 Instructor 實體的外部索引鍵包含在其中，並且在 `int` 類型指定後會新增一個問號，標示該屬性可為 Null。 導覽屬性會命名為 `Administrator`，但其包含的內容為一個 Instructor 實體：

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

部門中可能包含許多課程，因此當中包含了一個 Course 導覽屬性：

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> 根據慣例，Entity Framework 會為不可為 Null 的外部索引鍵和多對多關聯性啟用串聯刪除。 這可能會導致循環串聯刪除規則，並在您嘗試新增移轉時造成例外狀況。 例如，若您未將 Department.InstructorID 屬性定義成可為 Null，EF 就會設定串聯刪除規則，在您刪除講師時刪除部門，而這可能是您不願見到的情況。 若您的商務規則要求 `InstructorID` 屬性不可為 Null，則您將必須使用 Fluent API 陳述式來在關聯性上停用串聯刪除：
>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-enrollment-entity"></a>修改 Enrollment 實體

![Enrollment 實體](complex-data-model/_static/enrollment-entity.png)

在 *Models/Enrollment.cs* 中，以下列程式碼取代您在先前新增的程式碼：

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>外部索引鍵及導覽屬性

外部索引鍵屬性及導覽屬性反映了下列關聯性：

註冊記錄乃針對單一課程，因此當中包含了一個 `CourseID` 外部索引鍵屬性及一個 `Course` 導覽屬性：

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

註冊記錄乃針對單一學生，因此當中包含了一個 `StudentID` 外部索引鍵屬性及一個 `Student` 導覽屬性：

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>多對多關聯性

Student 和 Course 實體之間存在一個多對多關聯性，且 Enrollment 實體的功能便是多對多聯結資料表，其在資料庫中帶有*承載*。 「帶有承載」的意思是 Enrollment 資料表除了聯結資料表的外部索引鍵之外，還包含了額外的資料 (在此案例中為主索引鍵和 Grade 屬性)。

下列圖例展示了在實體圖表中這些關聯性的樣子。 (此圖表使用了 EF 6.x 的 Entity Framework Power Tools 產生。建立圖表不是此教學課程的一部分，其僅作為展示之用。)

![學生-課程多對多關聯性](complex-data-model/_static/student-course.png)

每個關聯性線條都在其中一端有一個「1」，並在另外一端有一個「星號 (*)」，顯示其為一對多關聯性。

若 Enrollment 資料表並未包含年級資訊，則其便只需要包含兩個外部索引鍵 (CourseID 和 StudentID)。 在此案例下，其在資料庫中將會是不具有承載的多對多聯結資料表 (或純聯結資料表)。 Instructor 和 Course 實體具有此類型的多對多關聯性，並且您的下一步驟便是建立一個實體類別作為不具有承載的聯結資料表。

(EF 6.x 支援多對多關聯性的隱含聯結資料表，但 EF Core 並不支援。 如需詳細資訊，請參閱 [EF Core GitHub 存放庫中的討論](https://github.com/aspnet/EntityFramework/issues/1368)。)

## <a name="the-courseassignment-entity"></a>CourseAssignment 實體

![CourseAssignment 實體](complex-data-model/_static/courseassignment-entity.png)

使用下列程式碼建立 *Models/CourseAssignment.cs*：

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a>聯結實體名稱

針對「講師-課程」多對多關聯性，資料庫中必須要有一個聯結資料表，且其必須由一個實體集代表。 將聯結實體命名為 `EntityName1EntityName2` 的做法非常常見。在此案例中，即為 `CourseInstructor`。 不過，我們建議您選擇可描述關聯性的名稱。 資料模型是從簡單開始慢慢成長的，並且常常一開始沒有承載的聯結在之後便會變成具有承載。 若您在一開始便使用了描述性的實體名稱，您在之後便不需要變更該名稱。 理想情況下，聯結實體在公司網域中會有自己的自然 (可能為一個單字) 名稱。 例如，「書籍」和「客戶」可透過「評分」進行連結。 針對此關聯性，相較於 `CourseAssignment`，`CourseInstructor` 是較佳的選擇。

### <a name="composite-key"></a>複合索引鍵

由於外部索引鍵不可為 Null，並且當一起使用時便可唯一識別資料表的每一個資料列，因此您不需要擁有一個個別的主索引鍵。 *InstructorID* 和 *CourseID* 屬性可作為複合主索引鍵發揮功能。 針對 EF 識別複合主索引鍵的唯一方法便是使用 *Fluent API* (您無法使用屬性完成這項操作)。 您會在下節中了解如何設定複合主索引鍵。

複合索引鍵可確保當您針對一個課程擁有多個資料列，且針對一位講師擁有多個資料列時，您無法針對相同的講師和課程擁有多個資料列。 由於 `Enrollment` 聯結實體定義了其自身的主索引鍵，因此這種種類的重複項目是可能的。 若要避免這種重複的項目，您可以在外部索引鍵欄位上新增一個唯一的索引，或使用與 `Enrollment` 相似的主複合索引鍵來設定 `CourseAssignment`。 如需詳細資訊，請參閱[索引](/ef/core/modeling/indexes)。

## <a name="update-the-database-context"></a>更新資料庫內容

將下列醒目提示的程式碼新增至 *Data/SchoolContext.cs* 檔案：

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

此程式碼會新增一個新實體，並設定 CourseAssignment 實體的複合主索引鍵。

## <a name="about-a-fluent-api-alternative"></a>關於 Fluent API 替代項目

`OnModelCreating` 類別的 `DbContext` 方法中的程式碼使用了 *Fluent API* 來設定 EF 行為。 此 API 稱為 "fluent" ，因為其常常會用於將一系列的方法呼叫串在一起，使其成為一個單一陳述式，例如這個從 [EF Core 文件](/ef/core/modeling/#use-fluent-api-to-configure-a-model)取得的範例：

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

在本教學課程中，您僅會使用 Fluent API 作為無法使用屬性操作時的資料庫對應之用。 然而，您也可以使用 Fluent API 指定大部分透過屬性可完成的格式、驗證及對應規則。 某些屬性 (例如 `MinimumLength`) 無法使用 Fluent API 來套用。 如前文所述，`MinimumLength` 不會變更結構描述，其僅會套用用戶端及伺服器端驗證規則。

某些開發人員偏好單獨使用 Fluent API，使其實體類別保持「整潔」。 若想要的話，您也可以混合使用屬性和 Fluent API，並且有一些自訂項目只能使用 Fluent API 完成。但一般來說，建議的做法是在這兩種方法中選擇其中一項，然後盡可能的一致使用該方法。 若您確實同時使用了兩者，則請注意當發生衝突時，Fluent API 會覆寫屬性。

如需屬性與 Fluent API 的詳細資訊，請參閱[方法](/ef/core/modeling/)。

## <a name="entity-diagram-showing-relationships"></a>顯示關聯性的實體圖表

下列圖例顯示了 Entity Framework Power Tools 為完成的 School 模型建立的圖表。

![實體圖表](complex-data-model/_static/diagram.png)

除了一對多關聯性線條外 (1 到 \*)，您也可以在 Instructor 和 OfficeAssignment 實體間看到一對零或一關聯性線條 (1 到 0..1)，以及介於 Instructor 和 Department 實體間的零或一對多關聯性線條 (0..1 到 *)。

## <a name="seed-database-with-test-data"></a>將測試資料植入資料庫

使用下列程式碼取代 *Data/DbInitializer.cs* 中的程式碼，以為您建立的新實體提供種子資料。

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

如同您在第一個課程中所看到的，此程式碼中大部分僅只是用於建立新實體物件，並針對測試需求載入範例資料。 請注意程式碼處理多對多關聯性的方式：程式碼會藉由在 `Enrollments` 和 `CourseAssignment` 聯結實體集中建立實體來建立關聯性。

## <a name="add-a-migration"></a>新增移轉

儲存您的變更，並建置專案。 然後在專案資料夾中開啟命令視窗，並輸入 `migrations add` 命令 (請還不要執行 update-database 命令)：

```dotnetcli
dotnet ef migrations add ComplexDataModel
```

您會收到有關可能發生資料遺失的警告。

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

若您嘗試在這個時間點執行 `database update` 命令，您會接收到下列錯誤：

> ALTER TABLE 陳述式與 FOREIGN KEY 條件約束 "FK_dbo.Course_dbo.Department_DepartmentID" 發生衝突。 衝突發生在 "ContoseUniversity" 資料庫、"dbo.Department" 資料表、"DepartmentID" 資料行中。

有時候當您使用現有資料執行移轉時，您必須將 Stub 資料插入資料庫中以滿足外部索引鍵條件約束。 `Up` 方法中產生的程式碼會新增一個不可為 Null 的 DepartmentID 外部索引鍵至 Course 資料表。 若在程式碼執行時 Course 資料表中已有資料列，則 `AddColumn` 作業會失敗，因為 SQL Server 無法得知要在不可為 Null 的資料行中填入何值。 針對此教學課程，您會在一個新的資料庫中執行移轉，但在生產環境下的應用程式中，您必須讓移轉能夠處理現有的資料，因此下列指引顯示了如何進行處理的範例。

若要使用現有資料完成移轉，您必須變更程式碼，給予新的資料行預設值，然後建立名為 "Temp" 的 Stub 部門，以作為預設部門。 其結果為現有的 Course 資料列便會在執行 `Up` 方法後與 "Temp" 部門產生關聯。

* 開啟 *{timestamp}_ComplexDataModel.cs* 檔案。

* 將新增 DepartmentID 資料行至 Course 資料表的程式碼全部標為註解。

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* 在建立 Department 資料表的程式碼之後新增下列醒目提示程式碼：

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

在生產環境的應用程式中，您會撰寫程式碼或指令碼以新增 Department 資料列，並使 Course 資料列與新的 Department 資料列產生關聯。 屆時您便不再需要為 Course.DepartmentID 資料行設定 "Temp" 部門或預設值。

儲存您的變更，並建置專案。

## <a name="change-the-connection-string"></a>變更連接字串

您現在在 `DbInitializer` 類別中已有了新的程式碼，可將新實體的種子資料新增至空白資料庫中。 若要使 EF 建立新的空白資料庫，將位於 *appsettings.json* 連接字串中的資料庫名稱變更為 ContosoUniversity3 或您所使用之電腦上未用過的其他名稱。

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

將您的變更儲存到 *appsettings.json*。

> [!NOTE]
> 如果您不想變更資料庫名稱，替代方法是刪除資料庫。 使用 [SQL Server 物件總管] (SSOX) 或 `database drop` CLI 命令：
>
> ```dotnetcli
> dotnet ef database drop
> ```

## <a name="update-the-database"></a>更新資料庫

在您變更資料庫名稱或刪除資料庫之後，在命令視窗中執行 `database update` 命令以執行移轉。

```dotnetcli
dotnet ef database update
```

執行應用程式以執行 `DbInitializer.Initialize` 方法並填入新資料庫。

如同先前操作，在 SSOX 中開啟資料庫，展開 [資料表] 節點以查看所有已建立的資料表。 (若您先前開啟的 SSOX 還在，請按一下 [重新整理] 按鈕。)

![SSOX 中的資料表](complex-data-model/_static/ssox-tables.png)

執行應用程式以觸發植入資料庫的初始設定式程式碼。

以滑鼠右鍵按一下 **CourseAssignment** 資料表，然後選取 [檢視資料] 以驗證其中已有資料。

![SSOX 中的 CourseAssignment 資料](complex-data-model/_static/ssox-ci-data.png)

## <a name="get-the-code"></a>取得程式碼

[下載或檢視已完成的應用程式。](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您：

> [!div class="checklist"]
> * 自訂資料模型
> * 對 Student 實體進行變更
> * 建立 Instructor 實體
> * 建立 OfficeAssignment 實體
> * 修改 Course 實體
> * 建立 Department 實體
> * 修改 Enrollment 實體
> * 更新資料庫內容
> * 將測試資料植入資料庫
> * 新增移轉
> * 變更連接字串
> * 更新資料庫

若要深入了解如何存取相關資料，請前往下個教學課程。

> [!div class="nextstepaction"]
> [下一步：存取相關資料](read-related-data.md)
