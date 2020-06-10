---
title: 第5部分， Razor ASP.NET Core 資料模型中具有 EF Core 的頁面
author: rick-anderson
description: 第5部分 Razor 頁面和 Entity Framework 教學課程系列。
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: f44ca9857ea127cf7e662e2712cc6d4b460450e9
ms.sourcegitcommit: fa67462abdf0cc4051977d40605183c629db7c64
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/10/2020
ms.locfileid: "84652497"
---
# <a name="part-5-razor-pages-with-ef-core-in-aspnet-core---data-model"></a><span data-ttu-id="f9209-103">第5部分， Razor ASP.NET Core 資料模型中具有 EF Core 的頁面</span><span class="sxs-lookup"><span data-stu-id="f9209-103">Part 5, Razor Pages with EF Core in ASP.NET Core - Data Model</span></span>

<span data-ttu-id="f9209-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f9209-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f9209-105">先前的教學課程建立了基本的資料模型，該模型由三個實體組成。</span><span class="sxs-lookup"><span data-stu-id="f9209-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="f9209-106">在本教學課程中：</span><span class="sxs-lookup"><span data-stu-id="f9209-106">In this tutorial:</span></span>

* <span data-ttu-id="f9209-107">新增更多實體和關聯性。</span><span class="sxs-lookup"><span data-stu-id="f9209-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="f9209-108">藉由指定格式、驗證和資料庫對應規則來自訂資料模型。</span><span class="sxs-lookup"><span data-stu-id="f9209-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="f9209-109">已完成的資料模型如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="f9209-109">The completed data model is shown in the following illustration:</span></span>

![實體圖表](complex-data-model/_static/diagram.png)

## <a name="the-student-entity"></a><span data-ttu-id="f9209-111">Student 實體</span><span class="sxs-lookup"><span data-stu-id="f9209-111">The Student entity</span></span>

![Student 實體](complex-data-model/_static/student-entity.png)

<span data-ttu-id="f9209-113">將 *Models/Student.cs* 中的程式碼替換成下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f9209-113">Replace the code in *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Student.cs)]

<span data-ttu-id="f9209-114">上述程式碼會新增 `FullName` 屬性，並將下列屬性新增到現有屬性：</span><span class="sxs-lookup"><span data-stu-id="f9209-114">The preceding code adds a `FullName` property and adds the following attributes to existing properties:</span></span>

* `[DataType]`
* `[DisplayFormat]`
* `[StringLength]`
* `[Column]`
* `[Required]`
* `[Display]`

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="f9209-115">FullName 計算屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-115">The FullName calculated property</span></span>

<span data-ttu-id="f9209-116">`FullName` 為一個計算屬性，會傳回藉由串連兩個其他屬性而建立的值。</span><span class="sxs-lookup"><span data-stu-id="f9209-116">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="f9209-117">您無法設定 `FullName`，因此它只會有 get 存取子。</span><span class="sxs-lookup"><span data-stu-id="f9209-117">`FullName` can't be set, so it has only a get accessor.</span></span> <span data-ttu-id="f9209-118">資料庫中不會建立 `FullName` 資料行。</span><span class="sxs-lookup"><span data-stu-id="f9209-118">No `FullName` column is created in the database.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="f9209-119">DataType 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-119">The DataType attribute</span></span>

```csharp
[DataType(DataType.Date)]
```

<span data-ttu-id="f9209-120">針對學生註冊日期，所有頁面目前都會同時顯示日期和一天當中的時間，雖然只要日期才是重要項目。</span><span class="sxs-lookup"><span data-stu-id="f9209-120">For student enrollment dates, all of the pages currently display the time of day along with the date, although only the date is relevant.</span></span> <span data-ttu-id="f9209-121">透過使用資料註解屬性，您便可以只透過單一程式碼變更來修正每個顯示資料頁面中的顯示格式。</span><span class="sxs-lookup"><span data-stu-id="f9209-121">By using data annotation attributes, you can make one code change that will fix the display format in every page that shows the data.</span></span> 

<span data-ttu-id="f9209-122">[DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) 屬性會指定一個比資料庫內建類型更明確的資料類型。</span><span class="sxs-lookup"><span data-stu-id="f9209-122">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="f9209-123">在此情況下，該欄位應該只顯示日期，而不會同時顯示日期和時間。</span><span class="sxs-lookup"><span data-stu-id="f9209-123">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="f9209-124">[DataType 列舉](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1)提供許多資料類型，例如 Date、Time、PhoneNumber、Currency、EmailAddress 等等。`DataType`屬性也可以讓應用程式自動提供類型特有的功能。</span><span class="sxs-lookup"><span data-stu-id="f9209-124">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="f9209-125">例如：</span><span class="sxs-lookup"><span data-stu-id="f9209-125">For example:</span></span>

* <span data-ttu-id="f9209-126">`DataType.EmailAddress` 會自動建立 `mailto:` 連結。</span><span class="sxs-lookup"><span data-stu-id="f9209-126">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="f9209-127">`DataType.Date` 在大多數的瀏覽器中都會提供日期選取器。</span><span class="sxs-lookup"><span data-stu-id="f9209-127">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="f9209-128">`DataType` 屬性會發出 HTML5 `data-` (發音為 data dash) 屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-128">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes.</span></span> <span data-ttu-id="f9209-129">`DataType` 屬性不會提供驗證。</span><span class="sxs-lookup"><span data-stu-id="f9209-129">The `DataType` attributes don't provide validation.</span></span>

### <a name="the-displayformat-attribute"></a><span data-ttu-id="f9209-130">DisplayFormat 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-130">The DisplayFormat attribute</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="f9209-131">`DataType.Date` 未指定顯示日期的格式。</span><span class="sxs-lookup"><span data-stu-id="f9209-131">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="f9209-132">根據預設，日期欄位會依照根據伺服器的 [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) 為基礎的預設格式顯示。</span><span class="sxs-lookup"><span data-stu-id="f9209-132">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="f9209-133">`DisplayFormat` 屬性會用來明確指定日期格式。</span><span class="sxs-lookup"><span data-stu-id="f9209-133">The `DisplayFormat` attribute is used to explicitly specify the date format.</span></span> <span data-ttu-id="f9209-134">`ApplyFormatInEditMode` 設定會指定格式也應套用在編輯 UI。</span><span class="sxs-lookup"><span data-stu-id="f9209-134">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="f9209-135">某些欄位不應該使用 `ApplyFormatInEditMode`。</span><span class="sxs-lookup"><span data-stu-id="f9209-135">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="f9209-136">例如，貨幣符號通常不應顯示在編輯文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="f9209-136">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="f9209-137">`DisplayFormat` 屬性可由自身使用。</span><span class="sxs-lookup"><span data-stu-id="f9209-137">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="f9209-138">通常使用 `DataType` 屬性搭配 `DisplayFormat` 屬性是一個不錯的做法。</span><span class="sxs-lookup"><span data-stu-id="f9209-138">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="f9209-139">`DataType` 屬性會將資料的語意以其在螢幕上呈現方式的相反方式傳遞。</span><span class="sxs-lookup"><span data-stu-id="f9209-139">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="f9209-140">`DataType` 屬性提供了下列優點，並且這些優點在 `DisplayFormat` 中無法使用：</span><span class="sxs-lookup"><span data-stu-id="f9209-140">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="f9209-141">瀏覽器可以啟用 HTML5 功能。</span><span class="sxs-lookup"><span data-stu-id="f9209-141">The browser can enable HTML5 features.</span></span> <span data-ttu-id="f9209-142">例如，顯示日曆控制項、適合地區設定的貨幣符號、電子郵件連結和用戶端輸入驗證。</span><span class="sxs-lookup"><span data-stu-id="f9209-142">For example, show a calendar control, the locale-appropriate currency symbol, email links, and client-side input validation.</span></span>
* <span data-ttu-id="f9209-143">根據預設，瀏覽器將根據地區設定，使用正確的格式呈現資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-143">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="f9209-144">如需詳細資訊，請參閱標記協助程式[ \<input> 檔](xref:mvc/views/working-with-forms#the-input-tag-helper)集。</span><span class="sxs-lookup"><span data-stu-id="f9209-144">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

### <a name="the-stringlength-attribute"></a><span data-ttu-id="f9209-145">StringLength 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-145">The StringLength attribute</span></span>

```csharp
[StringLength(50, ErrorMessage = "First name cannot be longer than 50 characters.")]
```

<span data-ttu-id="f9209-146">您可使用屬性指定資料驗證規則和驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="f9209-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="f9209-147">[StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) 屬性指定了在資料欄位中允許的最小及最大字元長度。</span><span class="sxs-lookup"><span data-stu-id="f9209-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="f9209-148">此處所顯示的程式碼會限制名稱不得超過 50 個字元。</span><span class="sxs-lookup"><span data-stu-id="f9209-148">The code shown limits names to no more than 50 characters.</span></span> <span data-ttu-id="f9209-149">設定字串長度下限的範例會在[稍後](#the-required-attribute)顯示。</span><span class="sxs-lookup"><span data-stu-id="f9209-149">An example that sets the minimum string length is shown [later](#the-required-attribute).</span></span>

<span data-ttu-id="f9209-150">`StringLength` 屬性同時也提供了用戶端和伺服器端的驗證。</span><span class="sxs-lookup"><span data-stu-id="f9209-150">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="f9209-151">最小值對資料庫結構描述不會造成任何影響。</span><span class="sxs-lookup"><span data-stu-id="f9209-151">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="f9209-152">`StringLength` 屬性不會防止使用者在名稱中輸入空白字元。</span><span class="sxs-lookup"><span data-stu-id="f9209-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="f9209-153">[RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) 屬性可用來將限制套用到輸入。</span><span class="sxs-lookup"><span data-stu-id="f9209-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute can be used to apply restrictions to the input.</span></span> <span data-ttu-id="f9209-154">例如，下列程式碼會要求第一個字元必須是大寫，其餘字元則必須是英文字母：</span><span class="sxs-lookup"><span data-stu-id="f9209-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z]*$")]
```

# <a name="visual-studio"></a>[<span data-ttu-id="f9209-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9209-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f9209-156">在 [SQL Server 物件總管]\*\*\*\* (SSOX) 中，按兩下 [Student]\*\*\*\* 資料表來開啟 Student 資料表設計工具。</span><span class="sxs-lookup"><span data-stu-id="f9209-156">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![移轉之前於 SSOX 中的 Students 資料表](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="f9209-158">上述影像顯示了 `Student` 資料表的結構描述。</span><span class="sxs-lookup"><span data-stu-id="f9209-158">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="f9209-159">名稱欄位的型別為 `nvarchar(MAX)`。</span><span class="sxs-lookup"><span data-stu-id="f9209-159">The name fields have type `nvarchar(MAX)`.</span></span> <span data-ttu-id="f9209-160">在本教學課程稍後建立移轉並套用時，名稱欄位會因為字串長度屬性而變更為 `nvarchar(50)`。</span><span class="sxs-lookup"><span data-stu-id="f9209-160">When a migration is created and applied later in this tutorial, the name fields become `nvarchar(50)` as a result of the string length attributes.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="f9209-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f9209-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f9209-162">在您的 SQLite 工具中，檢查 `Student` 資料表的資料行定義。</span><span class="sxs-lookup"><span data-stu-id="f9209-162">In your SQLite tool, examine the column definitions for the `Student` table.</span></span> <span data-ttu-id="f9209-163">名稱欄位的型別為 `Text`。</span><span class="sxs-lookup"><span data-stu-id="f9209-163">The name fields have type `Text`.</span></span> <span data-ttu-id="f9209-164">請注意，第一個名稱欄位稱為 `FirstMidName`。</span><span class="sxs-lookup"><span data-stu-id="f9209-164">Notice that the first name field is called `FirstMidName`.</span></span> <span data-ttu-id="f9209-165">在下一節中，您會將該資料行的名稱變更為 `FirstName`。</span><span class="sxs-lookup"><span data-stu-id="f9209-165">In the next section, you change the name of that column to `FirstName`.</span></span>

---

### <a name="the-column-attribute"></a><span data-ttu-id="f9209-166">Column 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-166">The Column attribute</span></span>

```csharp
[Column("FirstName")]
public string FirstMidName { get; set; }
```

<span data-ttu-id="f9209-167">可控制類別和屬性如何對應到資料庫的屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-167">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="f9209-168">在 `Student` 模型中，`Column` 屬性會用來將 `FirstMidName` 屬性名稱對應到資料庫中的 "FirstName"。</span><span class="sxs-lookup"><span data-stu-id="f9209-168">In the `Student` model, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the database.</span></span>

<span data-ttu-id="f9209-169">建立資料庫時，模型上的屬性名稱會用來作為資料行名稱 (使用 `Column` 屬性時除外)。</span><span class="sxs-lookup"><span data-stu-id="f9209-169">When the database is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span> <span data-ttu-id="f9209-170">`Student` 模型針對名字欄位使用 `FirstMidName`，因為欄位中可能也會包含中間名。</span><span class="sxs-lookup"><span data-stu-id="f9209-170">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="f9209-171">透過 `[Column]` 屬性，資料模型中 `Student.FirstMidName` 會對應到 `Student` 資料表的 `FirstName` 資料行。</span><span class="sxs-lookup"><span data-stu-id="f9209-171">With the `[Column]` attribute, `Student.FirstMidName` in the data model maps to the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="f9209-172">新增 `Column` 屬性會變更支援 `SchoolContext` 的模型。</span><span class="sxs-lookup"><span data-stu-id="f9209-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="f9209-173">支援 `SchoolContext` 的模型不再符合資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9209-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="f9209-174">這種不一致會透過在本教學課程中稍後部分新增移轉來解決。</span><span class="sxs-lookup"><span data-stu-id="f9209-174">That discrepancy will be resolved by adding a migration later in this tutorial.</span></span>

### <a name="the-required-attribute"></a><span data-ttu-id="f9209-175">Required 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-175">The Required attribute</span></span>

```csharp
[Required]
```

<span data-ttu-id="f9209-176">`Required` 屬性會讓名稱屬性成為必要欄位。</span><span class="sxs-lookup"><span data-stu-id="f9209-176">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="f9209-177">針對不可為 Null 型別的實值型別 (例如 `DateTime`、`int` 和 `double`)，`Required` 屬性並非必要項目。</span><span class="sxs-lookup"><span data-stu-id="f9209-177">The `Required` attribute isn't needed for non-nullable types such as value types (for example, `DateTime`, `int`, and `double`).</span></span> <span data-ttu-id="f9209-178">不可為 Null 的類型會自動視為必要欄位。</span><span class="sxs-lookup"><span data-stu-id="f9209-178">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="f9209-179">`Required` 屬性必須搭配 `MinimumLength` 使用，才能強制執行 `MinimumLength`。</span><span class="sxs-lookup"><span data-stu-id="f9209-179">The `Required` attribute must be used with `MinimumLength` for the `MinimumLength` to be enforced.</span></span>

```csharp
[Display(Name = "Last Name")]
[Required]
[StringLength(50, MinimumLength=2)]
public string LastName { get; set; }
```

<span data-ttu-id="f9209-180">`MinimumLength` 與 `Required` 允許空白字元以滿足驗證。</span><span class="sxs-lookup"><span data-stu-id="f9209-180">`MinimumLength` and `Required` allow whitespace to satisfy the validation.</span></span> <span data-ttu-id="f9209-181">使用 `RegularExpression` 屬性來完全控制字串。</span><span class="sxs-lookup"><span data-stu-id="f9209-181">Use the `RegularExpression` attribute for full control over the string.</span></span>

### <a name="the-display-attribute"></a><span data-ttu-id="f9209-182">Display 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-182">The Display attribute</span></span>

```csharp
[Display(Name = "Last Name")]
```

<span data-ttu-id="f9209-183">`Display` 屬性指定了文字方塊的標題應為「名字」、「姓氏」、「全名」及「註冊日期」。</span><span class="sxs-lookup"><span data-stu-id="f9209-183">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="f9209-184">預設標題中沒有使用空白分隔文字，例如「姓氏」。</span><span class="sxs-lookup"><span data-stu-id="f9209-184">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="create-a-migration"></a><span data-ttu-id="f9209-185">建立移轉</span><span class="sxs-lookup"><span data-stu-id="f9209-185">Create a migration</span></span>

<span data-ttu-id="f9209-186">執行應用程式並移至 Students 頁面。</span><span class="sxs-lookup"><span data-stu-id="f9209-186">Run the app and go to the Students page.</span></span> <span data-ttu-id="f9209-187">隨即擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f9209-187">An exception is thrown.</span></span> <span data-ttu-id="f9209-188">`[Column]` 屬性會造成 EF 預期尋找名為 `FirstName` 的資料行，但資料庫中的資料行名稱仍是 `FirstMidName`。</span><span class="sxs-lookup"><span data-stu-id="f9209-188">The `[Column]` attribute causes EF to expect to find a column named `FirstName`, but the column name in the database is still `FirstMidName`.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f9209-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9209-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f9209-190">錯誤訊息會與下列範例相似：</span><span class="sxs-lookup"><span data-stu-id="f9209-190">The error message is similar to the following example:</span></span>

```
SqlException: Invalid column name 'FirstName'.
```

* <span data-ttu-id="f9209-191">在 PMC 中，輸入下列命令來建立新的移轉並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="f9209-191">In the PMC, enter the following commands to create a new migration and update the database:</span></span>

  ```powershell
  Add-Migration ColumnFirstName
  Update-Database
  ```

  <span data-ttu-id="f9209-192">這些命令中的第一個命令會產生下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="f9209-192">The first of these commands generates the following warning message:</span></span>

  ```text
  An operation was scaffolded that may result in the loss of data.
  Please review the migration for accuracy.
  ```

  <span data-ttu-id="f9209-193">產生警告的理由是名稱欄位現在限制長度為 50 個字元。</span><span class="sxs-lookup"><span data-stu-id="f9209-193">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="f9209-194">若資料庫中的名稱超過 50 個字元，則第 51 到最後一個字元便會遺失。</span><span class="sxs-lookup"><span data-stu-id="f9209-194">If a name in the database had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="f9209-195">在 SSOX 中開啟 Student 資料表：</span><span class="sxs-lookup"><span data-stu-id="f9209-195">Open the Student table in SSOX:</span></span>

  ![移轉之後 SSOX 中的 Students 資料表](complex-data-model/_static/ssox-after-migration.png)

  <span data-ttu-id="f9209-197">在套用移轉前，名稱資料行的型別為 [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)。</span><span class="sxs-lookup"><span data-stu-id="f9209-197">Before the migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="f9209-198">名稱資料行現在為 `nvarchar(50)`。</span><span class="sxs-lookup"><span data-stu-id="f9209-198">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="f9209-199">資料行的名稱已從 `FirstMidName` 變更為 `FirstName`。</span><span class="sxs-lookup"><span data-stu-id="f9209-199">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="f9209-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f9209-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f9209-201">錯誤訊息會與下列範例相似：</span><span class="sxs-lookup"><span data-stu-id="f9209-201">The error message is similar to the following example:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: s.FirstName'.
```

* <span data-ttu-id="f9209-202">在專案資料夾中開啟命令視窗。</span><span class="sxs-lookup"><span data-stu-id="f9209-202">Open a command window in the project folder.</span></span> <span data-ttu-id="f9209-203">輸入下列命令來建立新的移轉並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="f9209-203">Enter the following commands to create a new migration and update the database:</span></span>

  ```dotnetcli
  dotnet ef migrations add ColumnFirstName
  dotnet ef database update
  ```

  <span data-ttu-id="f9209-204">資料庫更新命令會顯示與下列範例相似的錯誤：</span><span class="sxs-lookup"><span data-stu-id="f9209-204">The database update command displays an error like the following example:</span></span>

  ```text
  SQLite does not support this migration operation ('AlterColumnOperation'). For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
  ```

<span data-ttu-id="f9209-205">針對本教學課程，通過此錯誤的方法是刪除和重新建立初始移轉。</span><span class="sxs-lookup"><span data-stu-id="f9209-205">For this tutorial, the way to get past this error is to delete and re-create the initial migration.</span></span> <span data-ttu-id="f9209-206">如需詳細資訊，請參閱[移轉教學課程](xref:data/ef-rp/migrations)頂端的 SQLite 警告附註。</span><span class="sxs-lookup"><span data-stu-id="f9209-206">For more information, see the SQLite warning note at the top of the [migrations tutorial](xref:data/ef-rp/migrations).</span></span>

* <span data-ttu-id="f9209-207">刪除 *Migrations* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f9209-207">Delete the *Migrations* folder.</span></span>
* <span data-ttu-id="f9209-208">執行下列命令來卸除資料庫、建立新的初始移轉，然後套用移轉：</span><span class="sxs-lookup"><span data-stu-id="f9209-208">Run the following commands to drop the database, create a new initial migration, and apply the migration:</span></span>

  ```dotnetcli
  dotnet ef database drop --force
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

* <span data-ttu-id="f9209-209">在您的 SQLite 工具中檢查 Student 資料表。</span><span class="sxs-lookup"><span data-stu-id="f9209-209">Examine the Student table in your SQLite tool.</span></span> <span data-ttu-id="f9209-210">原先為 FirstMidName 的資料行現在已變更為 FirstName。</span><span class="sxs-lookup"><span data-stu-id="f9209-210">The column that was FirstMidName is now FirstName.</span></span>

---

* <span data-ttu-id="f9209-211">執行應用程式並移至 Students 頁面。</span><span class="sxs-lookup"><span data-stu-id="f9209-211">Run the app and go to the Students page.</span></span>
* <span data-ttu-id="f9209-212">請注意時間並未輸入或和日期一同顯示。</span><span class="sxs-lookup"><span data-stu-id="f9209-212">Notice that times are not input or displayed along with dates.</span></span>
* <span data-ttu-id="f9209-213">選取 [新建]\*\*\*\* 然後嘗試輸入超過 50 個字元的名稱。</span><span class="sxs-lookup"><span data-stu-id="f9209-213">Select **Create New**, and try to enter a name longer than 50 characters.</span></span>

> [!Note]
> <span data-ttu-id="f9209-214">在下列各節中，在某些階段建置應用程式會產生編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="f9209-214">In the following sections, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="f9209-215">指令會指定何時應建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="f9209-215">The instructions specify when to build the app.</span></span>

## <a name="the-instructor-entity"></a><span data-ttu-id="f9209-216">Instructor 實體</span><span class="sxs-lookup"><span data-stu-id="f9209-216">The Instructor Entity</span></span>

![Instructor 實體](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="f9209-218">使用下列程式碼建立 *Models/Instructor.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-218">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Instructor.cs)]

<span data-ttu-id="f9209-219">在同一行上可以有多個屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-219">Multiple attributes can be on one line.</span></span> <span data-ttu-id="f9209-220">`HireDate` 屬性可以下列方式撰寫：</span><span class="sxs-lookup"><span data-stu-id="f9209-220">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="navigation-properties"></a><span data-ttu-id="f9209-221">導覽屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-221">Navigation properties</span></span>

<span data-ttu-id="f9209-222">`CourseAssignments` 和 `OfficeAssignment` 屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-222">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="f9209-223">由於講師可以教授任何數量的課程，因此 `CourseAssignments` 已定義為一個集合。</span><span class="sxs-lookup"><span data-stu-id="f9209-223">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="f9209-224">講師最多只能擁有一間辦公室，因此 `OfficeAssignment` 屬性會保存單一 `OfficeAssignment` 實體。</span><span class="sxs-lookup"><span data-stu-id="f9209-224">An instructor can have at most one office, so the `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="f9209-225">若沒有指派任何辦公室，則 `OfficeAssignment` 為 Null。</span><span class="sxs-lookup"><span data-stu-id="f9209-225">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="the-officeassignment-entity"></a><span data-ttu-id="f9209-226">OfficeAssignment 實體</span><span class="sxs-lookup"><span data-stu-id="f9209-226">The OfficeAssignment entity</span></span>

![OfficeAssignment 實體](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="f9209-228">使用下列程式碼建立 *Models/OfficeAssignment.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-228">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="f9209-229">Key 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-229">The Key attribute</span></span>

<span data-ttu-id="f9209-230">`[Key]` 屬性用於將某個屬性名稱不是 classnameID 或 ID 的屬性識別為主索引鍵 (PK)。</span><span class="sxs-lookup"><span data-stu-id="f9209-230">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="f9209-231">在 `Instructor` 和 `OfficeAssignment` 實體之間有一對零或一關聯性。</span><span class="sxs-lookup"><span data-stu-id="f9209-231">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="f9209-232">辦公室指派只存在於與其受指派的講師關聯中。</span><span class="sxs-lookup"><span data-stu-id="f9209-232">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="f9209-233">`OfficeAssignment` PK 同時也是其連結到 `Instructor` 實體的外部索引鍵 (FK)。</span><span class="sxs-lookup"><span data-stu-id="f9209-233">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span>

<span data-ttu-id="f9209-234">EF Core 無法將 `InstructorID` 自動識別為 `OfficeAssignment` 的主索引鍵，因為 `InstructorID` 並未遵循識別碼或 classnameID 命名慣例。</span><span class="sxs-lookup"><span data-stu-id="f9209-234">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because `InstructorID` doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="f9209-235">因此，必須使用 `Key` 屬性將 `InstructorID` 識別為 PK：</span><span class="sxs-lookup"><span data-stu-id="f9209-235">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="f9209-236">根據預設，EF Core 會將索引鍵作為非資料庫產生的屬性處置，因為該資料行主要用於識別關聯性。</span><span class="sxs-lookup"><span data-stu-id="f9209-236">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="f9209-237">Instructor 導覽屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-237">The Instructor navigation property</span></span>

<span data-ttu-id="f9209-238">`Instructor.OfficeAssignment` 導覽屬性可以是 Null，因為指定講師可能不會有 `OfficeAssignment` 資料列。</span><span class="sxs-lookup"><span data-stu-id="f9209-238">The `Instructor.OfficeAssignment` navigation property can be null because there might not be an `OfficeAssignment` row for a given instructor.</span></span> <span data-ttu-id="f9209-239">講師可能沒有辦公室指派。</span><span class="sxs-lookup"><span data-stu-id="f9209-239">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="f9209-240">`OfficeAssignment.Instructor` 導覽屬性一律會擁有一個講師實體，因為外部索引鍵 `InstructorID` 的型別是 `int`，其為不可為 Null 的實值型別。</span><span class="sxs-lookup"><span data-stu-id="f9209-240">The `OfficeAssignment.Instructor` navigation property will always have an instructor entity because the foreign key `InstructorID` type is `int`, a non-nullable value type.</span></span> <span data-ttu-id="f9209-241">辦公室指派無法獨立於講師之外存在。</span><span class="sxs-lookup"><span data-stu-id="f9209-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="f9209-242">當 `Instructor` 實體有相關的 `OfficeAssignment` 實體時，每一個實體都會在其導覽屬性中包含一個其他實體的參考。</span><span class="sxs-lookup"><span data-stu-id="f9209-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="f9209-243">Course 實體</span><span class="sxs-lookup"><span data-stu-id="f9209-243">The Course Entity</span></span>

![Course 實體](complex-data-model/_static/course-entity.png)

<span data-ttu-id="f9209-245">使用下列程式碼更新 *Models/Course.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-245">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Course.cs?highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="f9209-246">`Course` 實體具有外部索引鍵 (FK) 屬性`DepartmentID`。</span><span class="sxs-lookup"><span data-stu-id="f9209-246">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="f9209-247">`DepartmentID` 會指向相關的 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="f9209-247">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="f9209-248">`Course` 實體具有一個 `Department` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-248">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="f9209-249">若模型針對相關實體已有導覽屬性，則 EF Core 針對資料模型便不需要外部索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-249">EF Core doesn't require a foreign key property for a data model when the model has a navigation property for a related entity.</span></span> <span data-ttu-id="f9209-250">EF Core 會自動在資料庫中需要的任何地方建立 FK。</span><span class="sxs-lookup"><span data-stu-id="f9209-250">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="f9209-251">EF Core 會為了自動建立的 FK 建立[陰影屬性](/ef/core/modeling/shadow-properties)。</span><span class="sxs-lookup"><span data-stu-id="f9209-251">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="f9209-252">但是，在資料模型中明確包含 FK (外部索引鍵) 可讓更新更簡單且更有效率。</span><span class="sxs-lookup"><span data-stu-id="f9209-252">However, explicitly including the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="f9209-253">例如，假設有一個模型，當中「不」\*\* 包含 `DepartmentID` FK 屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-253">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="f9209-254">當擷取課程實體以進行編輯時：</span><span class="sxs-lookup"><span data-stu-id="f9209-254">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="f9209-255">若 `Department` 屬性未明確載入，則其為 Null。</span><span class="sxs-lookup"><span data-stu-id="f9209-255">The `Department` property is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="f9209-256">若要更新課程實體，必須先擷取 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="f9209-256">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="f9209-257">當 FK 屬性 `DepartmentID` 包含在資料模型中時，便不需要在更新前擷取 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="f9209-257">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="f9209-258">DatabaseGenerated 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-258">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="f9209-259">`[DatabaseGenerated(DatabaseGeneratedOption.None)]` 屬性會指定 PK 是由應用程式提供的，而非資料庫產生的。</span><span class="sxs-lookup"><span data-stu-id="f9209-259">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="f9209-260">根據預設，EF Core 會假設 PK (主索引鍵) 的值是由資料庫所產生。</span><span class="sxs-lookup"><span data-stu-id="f9209-260">By default, EF Core assumes that PK values are generated by the database.</span></span> <span data-ttu-id="f9209-261">由資料庫產生主索引鍵通常是最佳做法。</span><span class="sxs-lookup"><span data-stu-id="f9209-261">Database-generated is generally the best approach.</span></span> <span data-ttu-id="f9209-262">針對 `Course` 實體，使用者指定了 PK。</span><span class="sxs-lookup"><span data-stu-id="f9209-262">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="f9209-263">例如，課程號碼 1000 系列表示數學部門的課程，2000 系列則為英文部門的課程。</span><span class="sxs-lookup"><span data-stu-id="f9209-263">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="f9209-264">`DatabaseGenerated` 屬性也可用於產生預設值。</span><span class="sxs-lookup"><span data-stu-id="f9209-264">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="f9209-265">例如，資料庫可以自動產生日期欄位來記錄建立或更新資料列的日期。</span><span class="sxs-lookup"><span data-stu-id="f9209-265">For example, the database can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="f9209-266">如需詳細資訊，請參閱[產生的屬性](/ef/core/modeling/generated-properties)。</span><span class="sxs-lookup"><span data-stu-id="f9209-266">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="f9209-267">外部索引鍵及導覽屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-267">Foreign key and navigation properties</span></span>

<span data-ttu-id="f9209-268">`Course` 實體中的外部索引鍵 (FK) 屬性和導覽屬性反映了下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="f9209-268">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="f9209-269">由於一個課程已指派給了一個部門，因此當中具有 `DepartmentID` FK 和 `Department` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-269">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="f9209-270">由於課程可由任何數量的學生進行註冊，因此 `Enrollments` 導覽屬性為一個集合：</span><span class="sxs-lookup"><span data-stu-id="f9209-270">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="f9209-271">課程可由多個講師進行教授，因此 `CourseAssignments` 導覽屬性為一個集合：</span><span class="sxs-lookup"><span data-stu-id="f9209-271">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="f9209-272">`CourseAssignment` 會在[稍後](#many-to-many-relationships)進行解釋。</span><span class="sxs-lookup"><span data-stu-id="f9209-272">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="the-department-entity"></a><span data-ttu-id="f9209-273">Department 實體</span><span class="sxs-lookup"><span data-stu-id="f9209-273">The Department entity</span></span>

![部門實體](complex-data-model/_static/department-entity.png)

<span data-ttu-id="f9209-275">使用下列程式碼建立 *Models/Department.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-275">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Models/Department1.cs)]

### <a name="the-column-attribute"></a><span data-ttu-id="f9209-276">Column 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-276">The Column attribute</span></span>

<span data-ttu-id="f9209-277">先前，`Column` 屬性主要用於變更資料行的名稱對應。</span><span class="sxs-lookup"><span data-stu-id="f9209-277">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="f9209-278">在 `Department` 實體的程式碼中，`Column` 屬性則用於變更 SQL 資料類型對應。</span><span class="sxs-lookup"><span data-stu-id="f9209-278">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="f9209-279">`Budget` 資料行在資料庫中是使用 SQL Server 的 money 型別來定義：</span><span class="sxs-lookup"><span data-stu-id="f9209-279">The `Budget` column is defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="f9209-280">通常您不需要資料行對應。</span><span class="sxs-lookup"><span data-stu-id="f9209-280">Column mapping is generally not required.</span></span> <span data-ttu-id="f9209-281">EF Core 會根據屬性的 CLR 型別來選擇適當 SQL Server 資料型別。</span><span class="sxs-lookup"><span data-stu-id="f9209-281">EF Core chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="f9209-282">CLR `decimal` 類型會對應到 SQL Server 的 `decimal` 類型。</span><span class="sxs-lookup"><span data-stu-id="f9209-282">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="f9209-283">由於 `Budget` 是貨幣，因此金額資料類型會比較適合貨幣。</span><span class="sxs-lookup"><span data-stu-id="f9209-283">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="f9209-284">外部索引鍵及導覽屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-284">Foreign key and navigation properties</span></span>

<span data-ttu-id="f9209-285">FK 及導覽屬性反映了下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="f9209-285">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="f9209-286">部門可能有或可能沒有系統管理員。</span><span class="sxs-lookup"><span data-stu-id="f9209-286">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="f9209-287">系統管理員一律為講師。</span><span class="sxs-lookup"><span data-stu-id="f9209-287">An administrator is always an instructor.</span></span> <span data-ttu-id="f9209-288">因此，`InstructorID` 已作為 FK 包含在 `Instructor` 實體中。</span><span class="sxs-lookup"><span data-stu-id="f9209-288">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="f9209-289">導覽屬性已命名為 `Administrator`，但其中保留了一個 `Instructor` 實體：</span><span class="sxs-lookup"><span data-stu-id="f9209-289">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="f9209-290">上述程式碼中的問號 (?) 表示屬性可為 Null。</span><span class="sxs-lookup"><span data-stu-id="f9209-290">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="f9209-291">部門中可能包含許多課程，因此當中包含了一個 Course 導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="f9209-291">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="f9209-292">根據慣例，EF Core 會為不可為 Null 的 FK 和多對多關聯性啟用串聯刪除。</span><span class="sxs-lookup"><span data-stu-id="f9209-292">By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="f9209-293">此預設行為可能會導致循環串聯刪除規則。</span><span class="sxs-lookup"><span data-stu-id="f9209-293">This default behavior can result in circular cascade delete rules.</span></span> <span data-ttu-id="f9209-294">循環串聯刪除規則會在新增移轉時造成例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f9209-294">Circular cascade delete rules cause an exception when a migration is added.</span></span>

<span data-ttu-id="f9209-295">例如，若 `Department.InstructorID` 屬性已定義成不可為 Null，EF Core 便會設定串聯刪除規則。</span><span class="sxs-lookup"><span data-stu-id="f9209-295">For example, if the `Department.InstructorID` property was defined as non-nullable, EF Core would configure a cascade delete rule.</span></span> <span data-ttu-id="f9209-296">在這種情況下，若指派為部門管理員的講師遭到刪除，則會同時刪除部門。</span><span class="sxs-lookup"><span data-stu-id="f9209-296">In that case, the department would be deleted when the instructor assigned as its administrator is deleted.</span></span> <span data-ttu-id="f9209-297">在這種情況下，限制規則會更有意義。</span><span class="sxs-lookup"><span data-stu-id="f9209-297">In this scenario, a restrict rule would make more sense.</span></span> <span data-ttu-id="f9209-298">下列[Fluent API](#fluent-api-alternative-to-attributes)會設定 [限制規則] 和 [停用串聯刪除]。</span><span class="sxs-lookup"><span data-stu-id="f9209-298">The following [fluent API](#fluent-api-alternative-to-attributes) would set a restrict rule and disable cascade delete.</span></span>

  ```csharp
  modelBuilder.Entity<Department>()
     .HasOne(d => d.Administrator)
     .WithMany()
     .OnDelete(DeleteBehavior.Restrict)
  ```

## <a name="the-enrollment-entity"></a><span data-ttu-id="f9209-299">Enrollment 實體</span><span class="sxs-lookup"><span data-stu-id="f9209-299">The Enrollment entity</span></span>

<span data-ttu-id="f9209-300">註冊記錄是某位學生參加的一門課程。</span><span class="sxs-lookup"><span data-stu-id="f9209-300">An enrollment record is for one course taken by one student.</span></span>

![Enrollment 實體](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="f9209-302">使用下列程式碼更新 *Models/Enrollment.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-302">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Enrollment.cs?highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="f9209-303">外部索引鍵及導覽屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-303">Foreign key and navigation properties</span></span>

<span data-ttu-id="f9209-304">FK 屬性及導覽屬性反映了下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="f9209-304">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="f9209-305">註冊記錄乃針對一個課程，因此當中包含了一個 `CourseID` FK 屬性及一個 `Course` 導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="f9209-305">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="f9209-306">註冊記錄乃針對一位學生，因此當中包含了一個 `StudentID` FK 屬性及一個 `Student` 導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="f9209-306">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="f9209-307">Many-to-Many Relationships</span><span class="sxs-lookup"><span data-stu-id="f9209-307">Many-to-Many Relationships</span></span>

<span data-ttu-id="f9209-308">在 `Student` 和 `Course` 實體之間存在一個多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="f9209-308">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="f9209-309">`Enrollment` 實體的功能為資料庫中一個「具有承載」\*\* 的多對多聯結資料表。</span><span class="sxs-lookup"><span data-stu-id="f9209-309">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="f9209-310">「具有承載」表示 `Enrollment` 資料表除了聯結資料表 (在此案例中為 PK 和 `Grade`) 的 FK 之外，還包含了額外的資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-310">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="f9209-311">下列圖例展示了在實體圖表中這些關聯性的樣子。</span><span class="sxs-lookup"><span data-stu-id="f9209-311">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="f9209-312">(此圖表是使用 EF 6.x 的 [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) 產生的。</span><span class="sxs-lookup"><span data-stu-id="f9209-312">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="f9209-313">建立圖表並不是此教學課程的一部分)。</span><span class="sxs-lookup"><span data-stu-id="f9209-313">Creating the diagram isn't part of the tutorial.)</span></span>

![「學生-課程」多對多關聯性](complex-data-model/_static/student-course.png)

<span data-ttu-id="f9209-315">每個關聯性線條都在其中一端有一個「1」，並在另外一端有一個「星號 (\*)」，顯示其為一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="f9209-315">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="f9209-316">若 `Enrollment` 資料表並未包含成績資訊，則其便只需要包含兩個 FK (`CourseID` 和 `StudentID`)。</span><span class="sxs-lookup"><span data-stu-id="f9209-316">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="f9209-317">沒有承載的多對多聯結資料表有時候也稱為「純聯結資料表 (PJT)」。</span><span class="sxs-lookup"><span data-stu-id="f9209-317">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="f9209-318">`Instructor` 和 `Course` 實體具有使用了純聯結資料表的多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="f9209-318">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="f9209-319">注意：EF 6.x 支援多對多關聯性的隱含聯結資料表，但 EF Core 並不支援。</span><span class="sxs-lookup"><span data-stu-id="f9209-319">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="f9209-320">如需詳細資訊，請參閱 [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/) (EF Core 2.0 中的多對多關聯性)。</span><span class="sxs-lookup"><span data-stu-id="f9209-320">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="f9209-321">CourseAssignment 實體</span><span class="sxs-lookup"><span data-stu-id="f9209-321">The CourseAssignment entity</span></span>

![CourseAssignment 實體](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="f9209-323">使用下列程式碼建立 *Models/CourseAssignment.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-323">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/CourseAssignment.cs)]

<span data-ttu-id="f9209-324">Instructor 對 Courses 的多對多關聯性需要聯結資料表，而該聯結資料表的實體為 CourseAssignment。</span><span class="sxs-lookup"><span data-stu-id="f9209-324">The Instructor-to-Courses many-to-many relationship requires a join table, and the entity for that join table is CourseAssignment.</span></span>

![講師-課程 m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="f9209-326">通常會將聯結實體命名為 `EntityName1EntityName2`。</span><span class="sxs-lookup"><span data-stu-id="f9209-326">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="f9209-327">例如，使用此模式的 Instructor 對 Courses 聯結資料表將會是 `CourseInstructor`。</span><span class="sxs-lookup"><span data-stu-id="f9209-327">For example, the Instructor-to-Courses join table using this pattern would be `CourseInstructor`.</span></span> <span data-ttu-id="f9209-328">不過，我們建議使用可描述關聯性的名稱。</span><span class="sxs-lookup"><span data-stu-id="f9209-328">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="f9209-329">資料模型一開始都是簡單的，之後便會持續成長。</span><span class="sxs-lookup"><span data-stu-id="f9209-329">Data models start out simple and grow.</span></span> <span data-ttu-id="f9209-330">不含承載的聯結資料表 (PJT) 常常會演變成包含承載。</span><span class="sxs-lookup"><span data-stu-id="f9209-330">Join tables without payload (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="f9209-331">藉由在一開始便使用描述性的實體名稱，當聯結資料表變更時便不需要變更名稱。</span><span class="sxs-lookup"><span data-stu-id="f9209-331">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="f9209-332">理想情況下，聯結實體在公司網域中會有自己的自然 (可能為一個單字) 名稱。</span><span class="sxs-lookup"><span data-stu-id="f9209-332">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="f9209-333">例如，「書籍」和「客戶」可連結為一個名為「評分」 的聯結實體。</span><span class="sxs-lookup"><span data-stu-id="f9209-333">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="f9209-334">針對講師-課程多對多關聯性，`CourseAssignment` 會比 `CourseInstructor` 來得好。</span><span class="sxs-lookup"><span data-stu-id="f9209-334">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="f9209-335">複合索引鍵</span><span class="sxs-lookup"><span data-stu-id="f9209-335">Composite key</span></span>

<span data-ttu-id="f9209-336">`CourseAssignment` (`InstructorID` 及 `CourseID`) 中的兩個 FK 一起搭配使用，便可唯一的識別 `CourseAssignment` 資料表中的每一個資料列。</span><span class="sxs-lookup"><span data-stu-id="f9209-336">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="f9209-337">`CourseAssignment` 並不需要其專屬的 PK。</span><span class="sxs-lookup"><span data-stu-id="f9209-337">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="f9209-338">`InstructorID` 及 `CourseID` 屬性的功能便是複合 PK。</span><span class="sxs-lookup"><span data-stu-id="f9209-338">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="f9209-339">為 EF Core 指定複合 PK 的唯一方法是使用 *Fluent API*。</span><span class="sxs-lookup"><span data-stu-id="f9209-339">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="f9209-340">下節會說明如何設定複合 PK。</span><span class="sxs-lookup"><span data-stu-id="f9209-340">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="f9209-341">複合索引鍵可確保：</span><span class="sxs-lookup"><span data-stu-id="f9209-341">The composite key ensures that:</span></span>

* <span data-ttu-id="f9209-342">一個課程可以有多個資料列。</span><span class="sxs-lookup"><span data-stu-id="f9209-342">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="f9209-343">一個講師可以有多個資料列。</span><span class="sxs-lookup"><span data-stu-id="f9209-343">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="f9209-344">不允許相同講師和課程的多個資料列。</span><span class="sxs-lookup"><span data-stu-id="f9209-344">Multiple rows aren't allowed for the same instructor and course.</span></span>

<span data-ttu-id="f9209-345">由於 `Enrollment` 聯結實體定義了其自身的 PK，因此這種種類的重複項目是可能的。</span><span class="sxs-lookup"><span data-stu-id="f9209-345">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="f9209-346">若要防止這類重複項目：</span><span class="sxs-lookup"><span data-stu-id="f9209-346">To prevent such duplicates:</span></span>

* <span data-ttu-id="f9209-347">在 FK 欄位中新增一個唯一的索引，或</span><span class="sxs-lookup"><span data-stu-id="f9209-347">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="f9209-348">為 `Enrollment` 設定一個主複合索引鍵，與 `CourseAssignment` 相似。</span><span class="sxs-lookup"><span data-stu-id="f9209-348">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="f9209-349">如需詳細資訊，請參閱[索引](/ef/core/modeling/indexes)。</span><span class="sxs-lookup"><span data-stu-id="f9209-349">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="f9209-350">更新資料庫內容</span><span class="sxs-lookup"><span data-stu-id="f9209-350">Update the database context</span></span>

<span data-ttu-id="f9209-351">使用下列程式碼來更新 *Data/SchoolContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-351">Update *Data/SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Data/SchoolContext.cs?highlight=15-18,25-31)]

<span data-ttu-id="f9209-352">上述程式碼會新增一個新實體，並設定 `CourseAssignment` 實體的複合 PK。</span><span class="sxs-lookup"><span data-stu-id="f9209-352">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="f9209-353">屬性的 Fluent API 替代項目</span><span class="sxs-lookup"><span data-stu-id="f9209-353">Fluent API alternative to attributes</span></span>

<span data-ttu-id="f9209-354">上述程式碼中的 `OnModelCreating` 方法使用了 *Fluent API* 設定 EF Core 行為。</span><span class="sxs-lookup"><span data-stu-id="f9209-354">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="f9209-355">此 API 稱為 "fluent" ，因為其常常會用於將一系列的方法呼叫串在一起，使其成為一個單一陳述式。</span><span class="sxs-lookup"><span data-stu-id="f9209-355">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="f9209-356">[下列程式碼](/ef/core/modeling/#use-fluent-api-to-configure-a-model)為 Fluent API 的其中一個範例：</span><span class="sxs-lookup"><span data-stu-id="f9209-356">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="f9209-357">在本教學課程中，Fluent API 僅會用於無法使用屬性完成的資料庫對應。</span><span class="sxs-lookup"><span data-stu-id="f9209-357">In this tutorial, the fluent API is used only for database mapping that can't be done with attributes.</span></span> <span data-ttu-id="f9209-358">然而，Fluent API 可指定大部分透過屬性可完成的格式、驗證及對應規則。</span><span class="sxs-lookup"><span data-stu-id="f9209-358">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="f9209-359">某些屬性 (例如 `MinimumLength`) 無法使用 Fluent API 來套用。</span><span class="sxs-lookup"><span data-stu-id="f9209-359">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="f9209-360">`MinimumLength` 不會變更結構描述。它只會套用一項最小長度驗證規則。</span><span class="sxs-lookup"><span data-stu-id="f9209-360">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="f9209-361">某些開發人員偏好單獨使用 Fluent API，使其實體類別保持「整潔」。</span><span class="sxs-lookup"><span data-stu-id="f9209-361">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="f9209-362">屬性和 Fluent API 可混合使用。</span><span class="sxs-lookup"><span data-stu-id="f9209-362">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="f9209-363">有一些設定只能透過 Fluent API 完成 (指定複合 PK)。</span><span class="sxs-lookup"><span data-stu-id="f9209-363">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="f9209-364">有一些設定只能透過屬性完成 (`MinimumLength`)。</span><span class="sxs-lookup"><span data-stu-id="f9209-364">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="f9209-365">使用 Fluent API 或屬性的建議做法為：</span><span class="sxs-lookup"><span data-stu-id="f9209-365">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="f9209-366">從這兩種方法中選擇一項。</span><span class="sxs-lookup"><span data-stu-id="f9209-366">Choose one of these two approaches.</span></span>
* <span data-ttu-id="f9209-367">持續且盡量使用您選擇的方法。</span><span class="sxs-lookup"><span data-stu-id="f9209-367">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="f9209-368">本教學課程中使用到的某些屬性主要用於：</span><span class="sxs-lookup"><span data-stu-id="f9209-368">Some of the attributes used in this tutorial are used for:</span></span>

* <span data-ttu-id="f9209-369">僅驗證 (例如，`MinimumLength`)。</span><span class="sxs-lookup"><span data-stu-id="f9209-369">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="f9209-370">僅 EF Core 組態 (例如，`HasKey`)。</span><span class="sxs-lookup"><span data-stu-id="f9209-370">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="f9209-371">驗證及 EF Core 組態 (例如，`[StringLength(50)]`)。</span><span class="sxs-lookup"><span data-stu-id="f9209-371">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="f9209-372">如需屬性與 Fluent API 的詳細資訊，請參閱[組態方法](/ef/core/modeling/)。</span><span class="sxs-lookup"><span data-stu-id="f9209-372">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram"></a><span data-ttu-id="f9209-373">實體圖表</span><span class="sxs-lookup"><span data-stu-id="f9209-373">Entity diagram</span></span>

<span data-ttu-id="f9209-374">下圖顯示了 EF Power Tools 為完成的 School 模型建立的圖表。</span><span class="sxs-lookup"><span data-stu-id="f9209-374">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![實體圖表](complex-data-model/_static/diagram.png)

<span data-ttu-id="f9209-376">上圖顯示︰</span><span class="sxs-lookup"><span data-stu-id="f9209-376">The preceding diagram shows:</span></span>

* <span data-ttu-id="f9209-377">數個一對多關聯性線條 (1 對 \*)。</span><span class="sxs-lookup"><span data-stu-id="f9209-377">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="f9209-378">`Instructor` 和 `OfficeAssignment` 實體之間的一對零或一關聯性線條 (1 對 0..1)。</span><span class="sxs-lookup"><span data-stu-id="f9209-378">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="f9209-379">`Instructor` 和 `Department` 實體之間的零或一對多關聯性線條 (0..1 對 \*)。</span><span class="sxs-lookup"><span data-stu-id="f9209-379">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="f9209-380">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="f9209-380">Seed the database</span></span>

<span data-ttu-id="f9209-381">更新 *Data/DbInitializer.cs* 中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="f9209-381">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu30/Data/DbInitializer.cs)]

<span data-ttu-id="f9209-382">上述程式碼為新的實體提供了種子資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-382">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="f9209-383">此程式碼中的大部分主要用於建立新的實體物件並載入範例資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-383">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="f9209-384">範例資料主要用於測試。</span><span class="sxs-lookup"><span data-stu-id="f9209-384">The sample data is used for testing.</span></span> <span data-ttu-id="f9209-385">如需如何植入多對多聯結資料表的範例，請參閱 `Enrollments` 和 `CourseAssignments`。</span><span class="sxs-lookup"><span data-stu-id="f9209-385">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="f9209-386">新增移轉</span><span class="sxs-lookup"><span data-stu-id="f9209-386">Add a migration</span></span>

<span data-ttu-id="f9209-387">建置專案。</span><span class="sxs-lookup"><span data-stu-id="f9209-387">Build the project.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f9209-388">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9209-388">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f9209-389">在 PMC 中，執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="f9209-389">In PMC, run the following command.</span></span>

```powershell
Add-Migration ComplexDataModel
```

<span data-ttu-id="f9209-390">上述命令會顯示關於可能發生資料遺失的警告。</span><span class="sxs-lookup"><span data-stu-id="f9209-390">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="f9209-391">若執行 `database update` 命令，便會產生下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="f9209-391">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="f9209-392">在下一節中，您會看到如何針對此錯誤採取動作。</span><span class="sxs-lookup"><span data-stu-id="f9209-392">In the next section, you see what to do about this error.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="f9209-393">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f9209-393">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f9209-394">若您新增移轉和執行 `database update` 命令，即會產生下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="f9209-394">If you add a migration and run the `database update` command, the following error would be produced:</span></span>

```text
SQLite does not support this migration operation ('DropForeignKeyOperation').
For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
```

<span data-ttu-id="f9209-395">在下一節中，您會了解如何避免此錯誤。</span><span class="sxs-lookup"><span data-stu-id="f9209-395">In the next section, you see how to avoid this error.</span></span>

---

## <a name="apply-the-migration-or-drop-and-re-create"></a><span data-ttu-id="f9209-396">套用移轉或卸除並重新建立</span><span class="sxs-lookup"><span data-stu-id="f9209-396">Apply the migration or drop and re-create</span></span>

<span data-ttu-id="f9209-397">現在您已經具有現有的資料庫，您需要思考如何將變更套用到該資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9209-397">Now that you have an existing database, you need to think about how to apply changes to it.</span></span> <span data-ttu-id="f9209-398">本教學課程示範兩種替代方法：</span><span class="sxs-lookup"><span data-stu-id="f9209-398">This tutorial shows two alternatives:</span></span>

* <span data-ttu-id="f9209-399">[卸除並重新建立資料庫](#drop)。</span><span class="sxs-lookup"><span data-stu-id="f9209-399">[Drop and re-create the database](#drop).</span></span> <span data-ttu-id="f9209-400">若您正在使用 SQLite，請選擇此節。</span><span class="sxs-lookup"><span data-stu-id="f9209-400">Choose this section if you're using SQLite.</span></span>
* <span data-ttu-id="f9209-401">[將遷移套用到現有的資料庫](#applyexisting)。</span><span class="sxs-lookup"><span data-stu-id="f9209-401">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="f9209-402">本節中的說明僅適用於 SQL Server，不適用於 **SQLite**。</span><span class="sxs-lookup"><span data-stu-id="f9209-402">The instructions in this section work for SQL Server only, **not for SQLite**.</span></span> 

<span data-ttu-id="f9209-403">這兩種選擇都適用於 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="f9209-403">Either choice works for SQL Server.</span></span> <span data-ttu-id="f9209-404">雖然套用移轉方法更複雜且耗時，但它是現實世界生產環境的慣用方法。</span><span class="sxs-lookup"><span data-stu-id="f9209-404">While the apply-migration method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> 

<a name="drop"></a>

## <a name="drop-and-re-create-the-database"></a><span data-ttu-id="f9209-405">卸除並重新建立資料庫</span><span class="sxs-lookup"><span data-stu-id="f9209-405">Drop and re-create the database</span></span>

<span data-ttu-id="f9209-406">若您正在使用 SQL Server 且想要在下一節中執行套用移轉方法，請[跳過此節](#apply-the-migration)。</span><span class="sxs-lookup"><span data-stu-id="f9209-406">[Skip this section](#apply-the-migration) if you're using SQL Server and want to do the apply-migration approach in the following section.</span></span>

<span data-ttu-id="f9209-407">強制 EF Core 建立新的資料庫、卸除並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="f9209-407">To force EF Core to create a new database, drop and update the database:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f9209-408">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9209-408">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f9209-409">在 [套件管理員主控台]\*\*\*\* (PMC) 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f9209-409">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Drop-Database
  ```

* <span data-ttu-id="f9209-410">刪除 *Migrations* 資料夾，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f9209-410">Delete the *Migrations* folder, then run the following command:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

# <a name="visual-studio-code"></a>[<span data-ttu-id="f9209-411">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f9209-411">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f9209-412">開啟命令視窗並巡覽至專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="f9209-412">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="f9209-413">專案資料夾包含 *ContosoUniversity.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f9209-413">The project folder contains the *ContosoUniversity.csproj* file.</span></span>

* <span data-ttu-id="f9209-414">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="f9209-414">Run the following command:</span></span>

  ```dotnetcli
  dotnet ef database drop --force
  ```

* <span data-ttu-id="f9209-415">刪除 *Migrations* 資料夾，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f9209-415">Delete the *Migrations* folder, then run the following command:</span></span>

  ```dotnetcli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

---

<span data-ttu-id="f9209-416">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f9209-416">Run the app.</span></span> <span data-ttu-id="f9209-417">執行應用程式會執行 `DbInitializer.Initialize` 方法。</span><span class="sxs-lookup"><span data-stu-id="f9209-417">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="f9209-418">`DbInitializer.Initialize` 會填入新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9209-418">The `DbInitializer.Initialize` populates the new database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f9209-419">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9209-419">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f9209-420">在 SSOX 中開啟資料庫：</span><span class="sxs-lookup"><span data-stu-id="f9209-420">Open the database in SSOX:</span></span>

* <span data-ttu-id="f9209-421">若先前已開啟過 SSOX，按一下 [重新整理]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f9209-421">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="f9209-422">展開 **Tables** 節點。</span><span class="sxs-lookup"><span data-stu-id="f9209-422">Expand the **Tables** node.</span></span> <span data-ttu-id="f9209-423">建立的資料表便會顯示。</span><span class="sxs-lookup"><span data-stu-id="f9209-423">The created tables are displayed.</span></span>

  ![SSOX 中的資料表](complex-data-model/_static/ssox-tables.png)

* <span data-ttu-id="f9209-425">檢查 **CourseAssignment** 資料表：</span><span class="sxs-lookup"><span data-stu-id="f9209-425">Examine the **CourseAssignment** table:</span></span>

  * <span data-ttu-id="f9209-426">以滑鼠右鍵按一下 **CourseAssignment** 資料表，然後選取 [檢視資料]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f9209-426">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
  * <span data-ttu-id="f9209-427">驗證 **CourseAssignment** 資料表中是否包含資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-427">Verify the **CourseAssignment** table contains data.</span></span>

  ![SSOX 中的 CourseAssignment 資料](complex-data-model/_static/ssox-ci-data.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="f9209-429">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f9209-429">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f9209-430">使用您的 SQLite 工具來檢查資料庫：</span><span class="sxs-lookup"><span data-stu-id="f9209-430">Use your SQLite tool to examine the database:</span></span>

* <span data-ttu-id="f9209-431">新的資料表和資料行。</span><span class="sxs-lookup"><span data-stu-id="f9209-431">New tables and columns.</span></span>
* <span data-ttu-id="f9209-432">在資料表中植入資料，例如 **CourseAssignment** 資料表。</span><span class="sxs-lookup"><span data-stu-id="f9209-432">Seeded data in tables, for example the **CourseAssignment** table.</span></span>

---

<a name="applyexisting"></a>

## <a name="apply-the-migration"></a><span data-ttu-id="f9209-433">套用移轉</span><span class="sxs-lookup"><span data-stu-id="f9209-433">Apply the migration</span></span>

<span data-ttu-id="f9209-434">此為選擇性區段。</span><span class="sxs-lookup"><span data-stu-id="f9209-434">This section is optional.</span></span> <span data-ttu-id="f9209-435">這些步驟僅適用於 SQL Server LocalDB，且只有在您跳過先前的[卸除並重新建立資料庫](#drop)一節時才適用。</span><span class="sxs-lookup"><span data-stu-id="f9209-435">These steps work only for SQL Server LocalDB and only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="f9209-436">當使用現有的資料執行移轉作業時，某些 FK 條件約束可能會無法透過現有資料滿足。</span><span class="sxs-lookup"><span data-stu-id="f9209-436">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="f9209-437">當您使用的是生產資料時，您必須進行幾個步驟才能移轉現有資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-437">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="f9209-438">本節提供了修正 FK 條件約束違規的範例。</span><span class="sxs-lookup"><span data-stu-id="f9209-438">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="f9209-439">請不要在沒有備份的情況下進行這些程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="f9209-439">Don't make these code changes without a backup.</span></span> <span data-ttu-id="f9209-440">若您已完成先前的[卸除並重新建立資料庫](#drop)一節，請不要變更這些程式碼。</span><span class="sxs-lookup"><span data-stu-id="f9209-440">Don't make these code changes if you completed the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="f9209-441">*{timestamp}_ComplexDataModel.cs* 檔案包含了下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f9209-441">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="f9209-442">上述程式碼將一個不可為 Null 的 `DepartmentID` FK 新增至 `Course` 資料表。</span><span class="sxs-lookup"><span data-stu-id="f9209-442">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="f9209-443">先前教學課程中的資料庫在 `Course` 中包含了資料列，因此無法使用移轉來更新資料表。</span><span class="sxs-lookup"><span data-stu-id="f9209-443">The database from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="f9209-444">若要讓 `ComplexDataModel` 與現有資料進行移轉：</span><span class="sxs-lookup"><span data-stu-id="f9209-444">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="f9209-445">變更程式碼，以給予新資料行 (`DepartmentID`) 一個新的預設值。</span><span class="sxs-lookup"><span data-stu-id="f9209-445">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="f9209-446">建立一個名為 "Temp" 的假部門以作為預設部門之用。</span><span class="sxs-lookup"><span data-stu-id="f9209-446">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="f9209-447">修正外部索引鍵條件約束</span><span class="sxs-lookup"><span data-stu-id="f9209-447">Fix the foreign key constraints</span></span>

<span data-ttu-id="f9209-448">在 `ComplexDataModel` 移轉類別中，更新 `Up` 方法：</span><span class="sxs-lookup"><span data-stu-id="f9209-448">In the `ComplexDataModel` migration class, update the `Up` method:</span></span>

* <span data-ttu-id="f9209-449">開啟 *{timestamp}_ComplexDataModel.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f9209-449">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="f9209-450">將新增 `DepartmentID` 資料行至 `Course` 資料表的程式碼全部標為註解。</span><span class="sxs-lookup"><span data-stu-id="f9209-450">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="f9209-451">新增下列醒目提示程式碼。</span><span class="sxs-lookup"><span data-stu-id="f9209-451">Add the following highlighted code.</span></span> <span data-ttu-id="f9209-452">新的程式碼位於 `.CreateTable( name: "Department"` 區塊後方：</span><span class="sxs-lookup"><span data-stu-id="f9209-452">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]

<span data-ttu-id="f9209-453">透過上述變更，現有的 `Course` 資料列將會在執行 `ComplexDataModel.Up` 方法後與 "Temp" 部門相關。</span><span class="sxs-lookup"><span data-stu-id="f9209-453">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel.Up` method runs.</span></span>

<span data-ttu-id="f9209-454">此處顯示處理這種情況的方式已針對本教學課程進行簡化。</span><span class="sxs-lookup"><span data-stu-id="f9209-454">The way of handling the situation shown here is simplified for this tutorial.</span></span> <span data-ttu-id="f9209-455">生產環境的應用程式會：</span><span class="sxs-lookup"><span data-stu-id="f9209-455">A production app would:</span></span>

* <span data-ttu-id="f9209-456">包含將 `Department` 資料列及相關 `Course` 資料列新增到新 `Department` 資料列的程式碼或指令碼。</span><span class="sxs-lookup"><span data-stu-id="f9209-456">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="f9209-457">不使用 "Temp" 部門或 `Course.DepartmentID` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="f9209-457">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f9209-458">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9209-458">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f9209-459">在 [套件管理員主控台]\*\*\*\* (PMC) 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f9209-459">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="f9209-460">因為 `DbInitializer.Initialize` 方法的設計僅適用於空白資料庫，所以請使用 SSOX 刪除 Student 和 Course 資料表中的所有資料列。</span><span class="sxs-lookup"><span data-stu-id="f9209-460">Because the `DbInitializer.Initialize` method is designed to work only with an empty database, use SSOX to delete all the rows in the Student and Course tables.</span></span> <span data-ttu-id="f9209-461">(串聯刪除會負責處理 Enrollment 資料表。)</span><span class="sxs-lookup"><span data-stu-id="f9209-461">(Cascade delete will take care of the Enrollment table.)</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="f9209-462">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f9209-462">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f9209-463">若您正在搭配 Visual Studio Code 使用 SQL Server LocalDB，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f9209-463">If you're using SQL Server LocalDB with Visual Studio Code, run the following command:</span></span>

  ```dotnetcli
  dotnet ef database update
  ```

---

<span data-ttu-id="f9209-464">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f9209-464">Run the app.</span></span> <span data-ttu-id="f9209-465">執行應用程式會執行 `DbInitializer.Initialize` 方法。</span><span class="sxs-lookup"><span data-stu-id="f9209-465">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="f9209-466">`DbInitializer.Initialize` 會填入新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9209-466">The `DbInitializer.Initialize` populates the new database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f9209-467">後續步驟</span><span class="sxs-lookup"><span data-stu-id="f9209-467">Next steps</span></span>

<span data-ttu-id="f9209-468">接下來的兩個教學課程會示範如何讀取和更新相關資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-468">The next two tutorials show how to read and update related data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f9209-469">[上一個教學](xref:data/ef-rp/migrations) 
>  課程[下一個教學](xref:data/ef-rp/read-related-data)課程</span><span class="sxs-lookup"><span data-stu-id="f9209-469">[Previous tutorial](xref:data/ef-rp/migrations)
[Next tutorial](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f9209-470">先前的教學課程建立了基本的資料模型，該模型由三個實體組成。</span><span class="sxs-lookup"><span data-stu-id="f9209-470">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="f9209-471">在本教學課程中：</span><span class="sxs-lookup"><span data-stu-id="f9209-471">In this tutorial:</span></span>

* <span data-ttu-id="f9209-472">新增更多實體和關聯性。</span><span class="sxs-lookup"><span data-stu-id="f9209-472">More entities and relationships are added.</span></span>
* <span data-ttu-id="f9209-473">藉由指定格式、驗證和資料庫對應規則來自訂資料模型。</span><span class="sxs-lookup"><span data-stu-id="f9209-473">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="f9209-474">下圖顯示已完成資料模型的實體類別：</span><span class="sxs-lookup"><span data-stu-id="f9209-474">The entity classes for the completed data model are shown in the following illustration:</span></span>

![實體圖表](complex-data-model/_static/diagram.png)

<span data-ttu-id="f9209-476">若您遇到無法解決的問題，請下載[完整應用程式](
https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)。</span><span class="sxs-lookup"><span data-stu-id="f9209-476">If you run into problems you can't solve, download the [completed app](
https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="f9209-477">使用屬性自訂資料模型</span><span class="sxs-lookup"><span data-stu-id="f9209-477">Customize the data model with attributes</span></span>

<span data-ttu-id="f9209-478">在本節中，您會使用屬性自訂資料模型。</span><span class="sxs-lookup"><span data-stu-id="f9209-478">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="f9209-479">DataType 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-479">The DataType attribute</span></span>

<span data-ttu-id="f9209-480">學生頁面目前顯示了註冊日期的時間。</span><span class="sxs-lookup"><span data-stu-id="f9209-480">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="f9209-481">一般而言，日期欄位只會顯示日期，而非時間。</span><span class="sxs-lookup"><span data-stu-id="f9209-481">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="f9209-482">使用下列醒目提示的程式碼更新 *Models/Student.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-482">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="f9209-483">[DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) 屬性會指定一個比資料庫內建類型更明確的資料類型。</span><span class="sxs-lookup"><span data-stu-id="f9209-483">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="f9209-484">在此情況下，該欄位應該只顯示日期，而不會同時顯示日期和時間。</span><span class="sxs-lookup"><span data-stu-id="f9209-484">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="f9209-485">[DataType 列舉](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1)提供許多資料類型，例如 Date、Time、PhoneNumber、Currency、EmailAddress 等等。`DataType`屬性也可以讓應用程式自動提供類型特有的功能。</span><span class="sxs-lookup"><span data-stu-id="f9209-485">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="f9209-486">例如：</span><span class="sxs-lookup"><span data-stu-id="f9209-486">For example:</span></span>

* <span data-ttu-id="f9209-487">`DataType.EmailAddress` 會自動建立 `mailto:` 連結。</span><span class="sxs-lookup"><span data-stu-id="f9209-487">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="f9209-488">`DataType.Date` 在大多數的瀏覽器中都會提供日期選取器。</span><span class="sxs-lookup"><span data-stu-id="f9209-488">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="f9209-489">`DataType` 屬性會發出 HTML 5 `data-` (發音為 data dash) 屬性，可讓 HTML 5 瀏覽器取用。</span><span class="sxs-lookup"><span data-stu-id="f9209-489">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="f9209-490">`DataType` 屬性不會提供驗證。</span><span class="sxs-lookup"><span data-stu-id="f9209-490">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="f9209-491">`DataType.Date` 未指定顯示日期的格式。</span><span class="sxs-lookup"><span data-stu-id="f9209-491">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="f9209-492">根據預設，日期欄位會依照根據伺服器的 [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) 為基礎的預設格式顯示。</span><span class="sxs-lookup"><span data-stu-id="f9209-492">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="f9209-493">`DisplayFormat` 屬性用來明確指定日期格式：</span><span class="sxs-lookup"><span data-stu-id="f9209-493">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="f9209-494">`ApplyFormatInEditMode` 設定會指定格式也應套用在編輯 UI。</span><span class="sxs-lookup"><span data-stu-id="f9209-494">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="f9209-495">某些欄位不應該使用 `ApplyFormatInEditMode`。</span><span class="sxs-lookup"><span data-stu-id="f9209-495">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="f9209-496">例如，貨幣符號通常不應顯示在編輯文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="f9209-496">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="f9209-497">`DisplayFormat` 屬性可由自身使用。</span><span class="sxs-lookup"><span data-stu-id="f9209-497">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="f9209-498">通常使用 `DataType` 屬性搭配 `DisplayFormat` 屬性是一個不錯的做法。</span><span class="sxs-lookup"><span data-stu-id="f9209-498">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="f9209-499">`DataType` 屬性會將資料的語意以其在螢幕上呈現方式的相反方式傳遞。</span><span class="sxs-lookup"><span data-stu-id="f9209-499">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="f9209-500">`DataType` 屬性提供了下列優點，並且這些優點在 `DisplayFormat` 中無法使用：</span><span class="sxs-lookup"><span data-stu-id="f9209-500">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="f9209-501">瀏覽器可以啟用 HTML5 功能。</span><span class="sxs-lookup"><span data-stu-id="f9209-501">The browser can enable HTML5 features.</span></span> <span data-ttu-id="f9209-502">例如，顯示日曆控制項、適合地區設定的貨幣符號、電子郵件連結、用戶端輸入驗證等。</span><span class="sxs-lookup"><span data-stu-id="f9209-502">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="f9209-503">根據預設，瀏覽器將根據地區設定，使用正確的格式呈現資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-503">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="f9209-504">如需詳細資訊，請參閱標記協助程式[ \<input> 檔](xref:mvc/views/working-with-forms#the-input-tag-helper)集。</span><span class="sxs-lookup"><span data-stu-id="f9209-504">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="f9209-505">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f9209-505">Run the app.</span></span> <span data-ttu-id="f9209-506">巡覽至 Students [索引] 頁面。</span><span class="sxs-lookup"><span data-stu-id="f9209-506">Navigate to the Students Index page.</span></span> <span data-ttu-id="f9209-507">時間將不再顯示。</span><span class="sxs-lookup"><span data-stu-id="f9209-507">Times are no longer displayed.</span></span> <span data-ttu-id="f9209-508">使用 `Student` 模型的每個檢視現在都只會顯示日期，而不會顯示時間。</span><span class="sxs-lookup"><span data-stu-id="f9209-508">Every view that uses the `Student` model displays the date without time.</span></span>

![顯示日期而不顯示時間的 Students [索引] 頁面](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="f9209-510">StringLength 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-510">The StringLength attribute</span></span>

<span data-ttu-id="f9209-511">您可使用屬性指定資料驗證規則和驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="f9209-511">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="f9209-512">[StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) 屬性指定了在資料欄位中允許的最小及最大字元長度。</span><span class="sxs-lookup"><span data-stu-id="f9209-512">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="f9209-513">`StringLength` 屬性同時也提供了用戶端和伺服器端的驗證。</span><span class="sxs-lookup"><span data-stu-id="f9209-513">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="f9209-514">最小值對資料庫結構描述不會造成任何影響。</span><span class="sxs-lookup"><span data-stu-id="f9209-514">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="f9209-515">使用下列程式碼更新 `Student` 模型：</span><span class="sxs-lookup"><span data-stu-id="f9209-515">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="f9209-516">上述的程式碼會限制名稱不得超過 50 個字元。</span><span class="sxs-lookup"><span data-stu-id="f9209-516">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="f9209-517">`StringLength` 屬性不會防止使用者在名稱中輸入空白字元。</span><span class="sxs-lookup"><span data-stu-id="f9209-517">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="f9209-518">[RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) 屬性可用於對輸入套用限制。</span><span class="sxs-lookup"><span data-stu-id="f9209-518">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="f9209-519">例如，下列程式碼會要求第一個字元必須是大寫，其餘字元則必須是英文字母：</span><span class="sxs-lookup"><span data-stu-id="f9209-519">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z]*$")]
```

<span data-ttu-id="f9209-520">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="f9209-520">Run the app:</span></span>

* <span data-ttu-id="f9209-521">巡覽至 Students 頁面。</span><span class="sxs-lookup"><span data-stu-id="f9209-521">Navigate to the Students page.</span></span>
* <span data-ttu-id="f9209-522">選取 [新建]\*\*\*\*，然後輸入超過 50 個字元的名稱。</span><span class="sxs-lookup"><span data-stu-id="f9209-522">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="f9209-523">選取 [建立]\*\*\*\*，用戶端驗證便會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="f9209-523">Select **Create**, client-side validation shows an error message.</span></span>

![顯示字元長度錯誤的 Students [索引] 頁面](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="f9209-525">在 [SQL Server 物件總管]\*\*\*\* (SSOX) 中，按兩下 [Student]\*\*\*\* 資料表來開啟 Student 資料表設計工具。</span><span class="sxs-lookup"><span data-stu-id="f9209-525">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![移轉之前於 SSOX 中的 Students 資料表](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="f9209-527">上述影像顯示了 `Student` 資料表的結構描述。</span><span class="sxs-lookup"><span data-stu-id="f9209-527">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="f9209-528">名稱欄位的類型為 `nvarchar(MAX)`，因為移轉尚未在資料庫中執行。</span><span class="sxs-lookup"><span data-stu-id="f9209-528">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="f9209-529">在本教學課程的稍後執行移轉後，名稱欄位便會成為 `nvarchar(50)`。</span><span class="sxs-lookup"><span data-stu-id="f9209-529">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="f9209-530">Column 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-530">The Column attribute</span></span>

<span data-ttu-id="f9209-531">可控制類別和屬性如何對應到資料庫的屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-531">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="f9209-532">在本節中，`Column` 屬性會用於將 `FirstMidName` 屬性的名稱對應到資料庫中的 "FirstName"。</span><span class="sxs-lookup"><span data-stu-id="f9209-532">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="f9209-533">當建立資料庫時，模型上的屬性名稱會用於資料行名稱 (除了使用 `Column` 屬性時之外)。</span><span class="sxs-lookup"><span data-stu-id="f9209-533">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="f9209-534">`Student` 模型針對名字欄位使用 `FirstMidName`，因為欄位中可能也會包含中間名。</span><span class="sxs-lookup"><span data-stu-id="f9209-534">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="f9209-535">使用下列醒目提示程式碼更新 *Student.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="f9209-535">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="f9209-536">經過上述變更之後，應用程式中的 `Student.FirstMidName` 會對應到 `Student` 資料表的 `FirstName` 資料行。</span><span class="sxs-lookup"><span data-stu-id="f9209-536">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="f9209-537">新增 `Column` 屬性會變更支援 `SchoolContext` 的模型。</span><span class="sxs-lookup"><span data-stu-id="f9209-537">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="f9209-538">支援 `SchoolContext` 的模型不再符合資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9209-538">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="f9209-539">若應用程式在套用移轉之前執行，便會產生下列例外狀況：</span><span class="sxs-lookup"><span data-stu-id="f9209-539">If the app is run before applying migrations, the following exception is generated:</span></span>

```
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="f9209-540">若要更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="f9209-540">To update the DB:</span></span>

* <span data-ttu-id="f9209-541">建置專案。</span><span class="sxs-lookup"><span data-stu-id="f9209-541">Build the project.</span></span>
* <span data-ttu-id="f9209-542">在專案資料夾中開啟命令視窗。</span><span class="sxs-lookup"><span data-stu-id="f9209-542">Open a command window in the project folder.</span></span> <span data-ttu-id="f9209-543">輸入下列命令來建立新的移轉並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="f9209-543">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f9209-544">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9209-544">Visual Studio</span></span>](#tab/visual-studio)

```powershell
Add-Migration ColumnFirstName
Update-Database
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="f9209-545">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f9209-545">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

---

<span data-ttu-id="f9209-546">`migrations add ColumnFirstName` 命令會產生下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="f9209-546">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="f9209-547">產生警告的理由是名稱欄位現在限制長度為 50 個字元。</span><span class="sxs-lookup"><span data-stu-id="f9209-547">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="f9209-548">若在資料庫中有名稱超過 50 個字元，第 51 個字元到最後一個字元便會遺失。</span><span class="sxs-lookup"><span data-stu-id="f9209-548">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="f9209-549">測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="f9209-549">Test the app.</span></span>

<span data-ttu-id="f9209-550">在 SSOX 中開啟 Student 資料表：</span><span class="sxs-lookup"><span data-stu-id="f9209-550">Open the Student table in SSOX:</span></span>

![移轉之後 SSOX 中的 Students 資料表](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="f9209-552">在移轉套用之前，名稱資料行的類型為 [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)。</span><span class="sxs-lookup"><span data-stu-id="f9209-552">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="f9209-553">名稱資料行現在為 `nvarchar(50)`。</span><span class="sxs-lookup"><span data-stu-id="f9209-553">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="f9209-554">資料行的名稱已從 `FirstMidName` 變更為 `FirstName`。</span><span class="sxs-lookup"><span data-stu-id="f9209-554">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="f9209-555">在下節中，在某些階段建置應用程式會產生編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="f9209-555">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="f9209-556">指令會指定何時應建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="f9209-556">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="f9209-557">Student 實體更新</span><span class="sxs-lookup"><span data-stu-id="f9209-557">Student entity update</span></span>

![Student 實體](complex-data-model/_static/student-entity.png)

<span data-ttu-id="f9209-559">使用下列程式碼更新 *Models/Student.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-559">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="f9209-560">Required 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-560">The Required attribute</span></span>

<span data-ttu-id="f9209-561">`Required` 屬性會讓名稱屬性成為必要欄位。</span><span class="sxs-lookup"><span data-stu-id="f9209-561">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="f9209-562">針對不可為 Null 的類型，例如實值型別 (`DateTime`、`int`、`double` 等) 等，`Required` 屬性是不需要的。</span><span class="sxs-lookup"><span data-stu-id="f9209-562">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="f9209-563">不可為 Null 的類型會自動視為必要欄位。</span><span class="sxs-lookup"><span data-stu-id="f9209-563">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="f9209-564">`Required` 屬性可由 `StringLength` 屬性中的最小長度參數取代：</span><span class="sxs-lookup"><span data-stu-id="f9209-564">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="f9209-565">Display 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-565">The Display attribute</span></span>

<span data-ttu-id="f9209-566">`Display` 屬性指定了文字方塊的標題應為「名字」、「姓氏」、「全名」及「註冊日期」。</span><span class="sxs-lookup"><span data-stu-id="f9209-566">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="f9209-567">預設標題中沒有使用空白分隔文字，例如「姓氏」。</span><span class="sxs-lookup"><span data-stu-id="f9209-567">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="f9209-568">FullName 計算屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-568">The FullName calculated property</span></span>

<span data-ttu-id="f9209-569">`FullName` 為一個計算屬性，會傳回藉由串連兩個其他屬性而建立的值。</span><span class="sxs-lookup"><span data-stu-id="f9209-569">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="f9209-570">`FullName` 無法進行設定，其只有 get 存取子。</span><span class="sxs-lookup"><span data-stu-id="f9209-570">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="f9209-571">資料庫中不會建立 `FullName` 資料行。</span><span class="sxs-lookup"><span data-stu-id="f9209-571">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="f9209-572">建立 Instructor 實體</span><span class="sxs-lookup"><span data-stu-id="f9209-572">Create the Instructor Entity</span></span>

![Instructor 實體](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="f9209-574">使用下列程式碼建立 *Models/Instructor.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-574">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="f9209-575">在同一行上可以有多個屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-575">Multiple attributes can be on one line.</span></span> <span data-ttu-id="f9209-576">`HireDate` 屬性可以下列方式撰寫：</span><span class="sxs-lookup"><span data-stu-id="f9209-576">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="f9209-577">CourseAssignments 和 OfficeAssignment 導覽屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-577">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="f9209-578">`CourseAssignments` 和 `OfficeAssignment` 屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-578">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="f9209-579">由於講師可以教授任何數量的課程，因此 `CourseAssignments` 已定義為一個集合。</span><span class="sxs-lookup"><span data-stu-id="f9209-579">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="f9209-580">若導覽屬性中保留了多個實體：</span><span class="sxs-lookup"><span data-stu-id="f9209-580">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="f9209-581">它必須是一種清單類型，可對其中的實體進行新增、刪除和更新。</span><span class="sxs-lookup"><span data-stu-id="f9209-581">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="f9209-582">導覽屬性類型包括：</span><span class="sxs-lookup"><span data-stu-id="f9209-582">Navigation property types include:</span></span>

* `ICollection<T>`
* `List<T>`
* `HashSet<T>`

<span data-ttu-id="f9209-583">若已指定 `ICollection<T>`，EF Core 會根據預設建立一個 `HashSet<T>` 集合。</span><span class="sxs-lookup"><span data-stu-id="f9209-583">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="f9209-584">`CourseAssignment` 實體會在本節的多對多關聯性中解釋。</span><span class="sxs-lookup"><span data-stu-id="f9209-584">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="f9209-585">Contoso 大學商務規則訂定了講師最多只能有一間辦公室。</span><span class="sxs-lookup"><span data-stu-id="f9209-585">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="f9209-586">`OfficeAssignment` 屬性保留了一個單一 `OfficeAssignment` 實體。</span><span class="sxs-lookup"><span data-stu-id="f9209-586">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="f9209-587">若沒有指派任何辦公室，則 `OfficeAssignment` 為 Null。</span><span class="sxs-lookup"><span data-stu-id="f9209-587">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="f9209-588">建立 OfficeAssignment 實體</span><span class="sxs-lookup"><span data-stu-id="f9209-588">Create the OfficeAssignment entity</span></span>

![OfficeAssignment 實體](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="f9209-590">使用下列程式碼建立 *Models/OfficeAssignment.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-590">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="f9209-591">Key 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-591">The Key attribute</span></span>

<span data-ttu-id="f9209-592">`[Key]` 屬性用於將某個屬性名稱不是 classnameID 或 ID 的屬性識別為主索引鍵 (PK)。</span><span class="sxs-lookup"><span data-stu-id="f9209-592">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="f9209-593">在 `Instructor` 和 `OfficeAssignment` 實體之間有一對零或一關聯性。</span><span class="sxs-lookup"><span data-stu-id="f9209-593">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="f9209-594">辦公室指派只存在於與其受指派的講師關聯中。</span><span class="sxs-lookup"><span data-stu-id="f9209-594">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="f9209-595">`OfficeAssignment` PK 同時也是其連結到 `Instructor` 實體的外部索引鍵 (FK)。</span><span class="sxs-lookup"><span data-stu-id="f9209-595">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="f9209-596">EF Core 無法自動將 `InstructorID` 識別為 `OfficeAssignment` 的主索引鍵，因為：</span><span class="sxs-lookup"><span data-stu-id="f9209-596">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="f9209-597">`InstructorID` 並未遵循 ID 或 classnameID 的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="f9209-597">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="f9209-598">因此，必須使用 `Key` 屬性將 `InstructorID` 識別為 PK：</span><span class="sxs-lookup"><span data-stu-id="f9209-598">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="f9209-599">根據預設，EF Core 會將索引鍵作為非資料庫產生的屬性處置，因為該資料行主要用於識別關聯性。</span><span class="sxs-lookup"><span data-stu-id="f9209-599">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="f9209-600">Instructor 導覽屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-600">The Instructor navigation property</span></span>

<span data-ttu-id="f9209-601">`Instructor` 實體的 `OfficeAssignment` 導覽屬性可為 Null，因為：</span><span class="sxs-lookup"><span data-stu-id="f9209-601">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="f9209-602">參考型別 (例如類別可為 Null)。</span><span class="sxs-lookup"><span data-stu-id="f9209-602">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="f9209-603">講師可能沒有辦公室指派。</span><span class="sxs-lookup"><span data-stu-id="f9209-603">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="f9209-604">`OfficeAssignment` 實體有不可為 Null 的`Instructor` 導覽屬性，因為：</span><span class="sxs-lookup"><span data-stu-id="f9209-604">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="f9209-605">`InstructorID` 不可為 Null。</span><span class="sxs-lookup"><span data-stu-id="f9209-605">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="f9209-606">辦公室指派無法獨立於講師之外存在。</span><span class="sxs-lookup"><span data-stu-id="f9209-606">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="f9209-607">當 `Instructor` 實體有相關的 `OfficeAssignment` 實體時，每一個實體都會在其導覽屬性中包含一個其他實體的參考。</span><span class="sxs-lookup"><span data-stu-id="f9209-607">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="f9209-608">`[Required]` 屬性可套用到 `Instructor` 導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="f9209-608">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="f9209-609">上述程式碼指定了必須要有相關的講師。</span><span class="sxs-lookup"><span data-stu-id="f9209-609">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="f9209-610">上述程式碼是不必要的，因為 `InstructorID` 外部索引鍵 (同時也是 PK) 不可為 Null。</span><span class="sxs-lookup"><span data-stu-id="f9209-610">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="f9209-611">修改 Course 實體</span><span class="sxs-lookup"><span data-stu-id="f9209-611">Modify the Course Entity</span></span>

![Course 實體](complex-data-model/_static/course-entity.png)

<span data-ttu-id="f9209-613">使用下列程式碼更新 *Models/Course.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-613">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="f9209-614">`Course` 實體具有外部索引鍵 (FK) 屬性`DepartmentID`。</span><span class="sxs-lookup"><span data-stu-id="f9209-614">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="f9209-615">`DepartmentID` 會指向相關的 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="f9209-615">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="f9209-616">`Course` 實體具有一個 `Department` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-616">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="f9209-617">當資料模型針對相關實體有一個導覽屬性時，EF Core 不需要針對該模型具備 FK 屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-617">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="f9209-618">EF Core 會自動在資料庫中需要的任何地方建立 FK。</span><span class="sxs-lookup"><span data-stu-id="f9209-618">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="f9209-619">EF Core 會為了自動建立的 FK 建立[陰影屬性](/ef/core/modeling/shadow-properties)。</span><span class="sxs-lookup"><span data-stu-id="f9209-619">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="f9209-620">在資料模型中具備 FK 可讓更新變得更為簡單和有效率。</span><span class="sxs-lookup"><span data-stu-id="f9209-620">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="f9209-621">例如，假設有一個模型，當中「不」\*\* 包含 `DepartmentID` FK 屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-621">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="f9209-622">當擷取課程實體以進行編輯時：</span><span class="sxs-lookup"><span data-stu-id="f9209-622">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="f9209-623">若沒有明確載入，`Department` 實體將為 Null。</span><span class="sxs-lookup"><span data-stu-id="f9209-623">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="f9209-624">若要更新課程實體，必須先擷取 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="f9209-624">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="f9209-625">當 FK 屬性 `DepartmentID` 包含在資料模型中時，便不需要在更新前擷取 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="f9209-625">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="f9209-626">DatabaseGenerated 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-626">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="f9209-627">`[DatabaseGenerated(DatabaseGeneratedOption.None)]` 屬性會指定 PK 是由應用程式提供的，而非資料庫產生的。</span><span class="sxs-lookup"><span data-stu-id="f9209-627">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="f9209-628">根據預設，EF Core 會假設 PK 值都是由資料庫產生的。</span><span class="sxs-lookup"><span data-stu-id="f9209-628">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="f9209-629">由資料庫產生 PK 值通常都是最佳做法。</span><span class="sxs-lookup"><span data-stu-id="f9209-629">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="f9209-630">針對 `Course` 實體，使用者指定了 PK。</span><span class="sxs-lookup"><span data-stu-id="f9209-630">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="f9209-631">例如，課程號碼 1000 系列表示數學部門的課程，2000 系列則為英文部門的課程。</span><span class="sxs-lookup"><span data-stu-id="f9209-631">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="f9209-632">`DatabaseGenerated` 屬性也可用於產生預設值。</span><span class="sxs-lookup"><span data-stu-id="f9209-632">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="f9209-633">例如，資料庫會自動產生日期欄位來記錄資料列建立或更新的日期。</span><span class="sxs-lookup"><span data-stu-id="f9209-633">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="f9209-634">如需詳細資訊，請參閱[產生的屬性](/ef/core/modeling/generated-properties)。</span><span class="sxs-lookup"><span data-stu-id="f9209-634">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="f9209-635">外部索引鍵及導覽屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-635">Foreign key and navigation properties</span></span>

<span data-ttu-id="f9209-636">`Course` 實體中的外部索引鍵 (FK) 屬性和導覽屬性反映了下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="f9209-636">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="f9209-637">由於一個課程已指派給了一個部門，因此當中具有 `DepartmentID` FK 和 `Department` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f9209-637">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="f9209-638">由於課程可由任何數量的學生進行註冊，因此 `Enrollments` 導覽屬性為一個集合：</span><span class="sxs-lookup"><span data-stu-id="f9209-638">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="f9209-639">課程可由多個講師進行教授，因此 `CourseAssignments` 導覽屬性為一個集合：</span><span class="sxs-lookup"><span data-stu-id="f9209-639">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="f9209-640">`CourseAssignment` 會在[稍後](#many-to-many-relationships)進行解釋。</span><span class="sxs-lookup"><span data-stu-id="f9209-640">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="f9209-641">建立 Department 實體</span><span class="sxs-lookup"><span data-stu-id="f9209-641">Create the Department entity</span></span>

![部門實體](complex-data-model/_static/department-entity.png)

<span data-ttu-id="f9209-643">使用下列程式碼建立 *Models/Department.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-643">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="f9209-644">Column 屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-644">The Column attribute</span></span>

<span data-ttu-id="f9209-645">先前，`Column` 屬性主要用於變更資料行的名稱對應。</span><span class="sxs-lookup"><span data-stu-id="f9209-645">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="f9209-646">在 `Department` 實體的程式碼中，`Column` 屬性則用於變更 SQL 資料類型對應。</span><span class="sxs-lookup"><span data-stu-id="f9209-646">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="f9209-647">`Budget` 資料行則是使用資料庫中的 SQL Server 金額類型定義：</span><span class="sxs-lookup"><span data-stu-id="f9209-647">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="f9209-648">通常您不需要資料行對應。</span><span class="sxs-lookup"><span data-stu-id="f9209-648">Column mapping is generally not required.</span></span> <span data-ttu-id="f9209-649">EF Core 通常會根據屬性的 CLR 類型選擇適當的 SQL Server 資料類型。</span><span class="sxs-lookup"><span data-stu-id="f9209-649">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="f9209-650">CLR `decimal` 類型會對應到 SQL Server 的 `decimal` 類型。</span><span class="sxs-lookup"><span data-stu-id="f9209-650">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="f9209-651">由於 `Budget` 是貨幣，因此金額資料類型會比較適合貨幣。</span><span class="sxs-lookup"><span data-stu-id="f9209-651">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="f9209-652">外部索引鍵及導覽屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-652">Foreign key and navigation properties</span></span>

<span data-ttu-id="f9209-653">FK 及導覽屬性反映了下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="f9209-653">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="f9209-654">部門可能有或可能沒有系統管理員。</span><span class="sxs-lookup"><span data-stu-id="f9209-654">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="f9209-655">系統管理員一律為講師。</span><span class="sxs-lookup"><span data-stu-id="f9209-655">An administrator is always an instructor.</span></span> <span data-ttu-id="f9209-656">因此，`InstructorID` 已作為 FK 包含在 `Instructor` 實體中。</span><span class="sxs-lookup"><span data-stu-id="f9209-656">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="f9209-657">導覽屬性已命名為 `Administrator`，但其中保留了一個 `Instructor` 實體：</span><span class="sxs-lookup"><span data-stu-id="f9209-657">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="f9209-658">上述程式碼中的問號 (?) 表示屬性可為 Null。</span><span class="sxs-lookup"><span data-stu-id="f9209-658">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="f9209-659">部門中可能包含許多課程，因此當中包含了一個 Course 導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="f9209-659">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="f9209-660">注意：根據慣例，EF Core 會為不可為 Null 的 FK 和多對多關聯性啟用串聯刪除。</span><span class="sxs-lookup"><span data-stu-id="f9209-660">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="f9209-661">串聯刪除可能會導致循環的串聯刪除規則。</span><span class="sxs-lookup"><span data-stu-id="f9209-661">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="f9209-662">循環串聯刪除規則會在新增移轉時造成例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f9209-662">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="f9209-663">例如，若已將 `Department.InstructorID` 屬性定義為不可為 Null：</span><span class="sxs-lookup"><span data-stu-id="f9209-663">For example, if the `Department.InstructorID` property was defined as non-nullable:</span></span>

* <span data-ttu-id="f9209-664">EF Core 會設定串聯刪除規則，以便在刪除講師時刪除部門。</span><span class="sxs-lookup"><span data-stu-id="f9209-664">EF Core configures a cascade delete rule to delete the department when the instructor is deleted.</span></span>
* <span data-ttu-id="f9209-665">在刪除講師時刪除部門並非預期的行為。</span><span class="sxs-lookup"><span data-stu-id="f9209-665">Deleting the department when the instructor is deleted isn't the intended behavior.</span></span>
* <span data-ttu-id="f9209-666">下列[Fluent API](#fluent-api-alternative-to-attributes)會設定限制規則，而不是 cascade。</span><span class="sxs-lookup"><span data-stu-id="f9209-666">The following [fluent API](#fluent-api-alternative-to-attributes) would set a restrict rule instead of cascade.</span></span>

   ```csharp
   modelBuilder.Entity<Department>()
      .HasOne(d => d.Administrator)
      .WithMany()
      .OnDelete(DeleteBehavior.Restrict)
  ```

<span data-ttu-id="f9209-667">上述的程式碼會在部門-講師關聯性上停用串聯刪除。</span><span class="sxs-lookup"><span data-stu-id="f9209-667">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="f9209-668">更新 Enrollment 實體</span><span class="sxs-lookup"><span data-stu-id="f9209-668">Update the Enrollment entity</span></span>

<span data-ttu-id="f9209-669">註冊記錄是某位學生參加的一門課程。</span><span class="sxs-lookup"><span data-stu-id="f9209-669">An enrollment record is for one course taken by one student.</span></span>

![Enrollment 實體](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="f9209-671">使用下列程式碼更新 *Models/Enrollment.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-671">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="f9209-672">外部索引鍵及導覽屬性</span><span class="sxs-lookup"><span data-stu-id="f9209-672">Foreign key and navigation properties</span></span>

<span data-ttu-id="f9209-673">FK 屬性及導覽屬性反映了下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="f9209-673">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="f9209-674">註冊記錄乃針對一個課程，因此當中包含了一個 `CourseID` FK 屬性及一個 `Course` 導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="f9209-674">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="f9209-675">註冊記錄乃針對一位學生，因此當中包含了一個 `StudentID` FK 屬性及一個 `Student` 導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="f9209-675">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="f9209-676">Many-to-Many Relationships</span><span class="sxs-lookup"><span data-stu-id="f9209-676">Many-to-Many Relationships</span></span>

<span data-ttu-id="f9209-677">在 `Student` 和 `Course` 實體之間存在一個多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="f9209-677">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="f9209-678">`Enrollment` 實體的功能為資料庫中一個「具有承載」\*\* 的多對多聯結資料表。</span><span class="sxs-lookup"><span data-stu-id="f9209-678">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="f9209-679">「具有承載」表示 `Enrollment` 資料表除了聯結資料表 (在此案例中為 PK 和 `Grade`) 的 FK 之外，還包含了額外的資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-679">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="f9209-680">下列圖例展示了在實體圖表中這些關聯性的樣子。</span><span class="sxs-lookup"><span data-stu-id="f9209-680">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="f9209-681">(此圖表是使用 EF 6.x 的 [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) 產生的。</span><span class="sxs-lookup"><span data-stu-id="f9209-681">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="f9209-682">建立圖表並不是此教學課程的一部分)。</span><span class="sxs-lookup"><span data-stu-id="f9209-682">Creating the diagram isn't part of the tutorial.)</span></span>

![「學生-課程」多對多關聯性](complex-data-model/_static/student-course.png)

<span data-ttu-id="f9209-684">每個關聯性線條都在其中一端有一個「1」，並在另外一端有一個「星號 (\*)」，顯示其為一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="f9209-684">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="f9209-685">若 `Enrollment` 資料表並未包含成績資訊，則其便只需要包含兩個 FK (`CourseID` 和 `StudentID`)。</span><span class="sxs-lookup"><span data-stu-id="f9209-685">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="f9209-686">沒有承載的多對多聯結資料表有時候也稱為「純聯結資料表 (PJT)」。</span><span class="sxs-lookup"><span data-stu-id="f9209-686">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="f9209-687">`Instructor` 和 `Course` 實體具有使用了純聯結資料表的多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="f9209-687">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="f9209-688">注意：EF 6.x 支援多對多關聯性的隱含聯結資料表，但 EF Core 並不支援。</span><span class="sxs-lookup"><span data-stu-id="f9209-688">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="f9209-689">如需詳細資訊，請參閱 [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/) (EF Core 2.0 中的多對多關聯性)。</span><span class="sxs-lookup"><span data-stu-id="f9209-689">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="f9209-690">CourseAssignment 實體</span><span class="sxs-lookup"><span data-stu-id="f9209-690">The CourseAssignment entity</span></span>

![CourseAssignment 實體](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="f9209-692">使用下列程式碼建立 *Models/CourseAssignment.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-692">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="f9209-693">講師-課程</span><span class="sxs-lookup"><span data-stu-id="f9209-693">Instructor-to-Courses</span></span>

![講師-課程 m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="f9209-695">講師-課程多對多關聯性：</span><span class="sxs-lookup"><span data-stu-id="f9209-695">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="f9209-696">需要一個由實體集代表的聯結資料表。</span><span class="sxs-lookup"><span data-stu-id="f9209-696">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="f9209-697">為一個純聯結資料表 (沒有承載的資料表)。</span><span class="sxs-lookup"><span data-stu-id="f9209-697">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="f9209-698">通常會將聯結實體命名為 `EntityName1EntityName2`。</span><span class="sxs-lookup"><span data-stu-id="f9209-698">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="f9209-699">例如，使用此模式的講師-課程聯結資料表為 `CourseInstructor`。</span><span class="sxs-lookup"><span data-stu-id="f9209-699">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="f9209-700">不過，我們建議使用可描述關聯性的名稱。</span><span class="sxs-lookup"><span data-stu-id="f9209-700">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="f9209-701">資料模型一開始都是簡單的，之後便會持續成長。</span><span class="sxs-lookup"><span data-stu-id="f9209-701">Data models start out simple and grow.</span></span> <span data-ttu-id="f9209-702">無承載聯結 (PJT) 常常會演變為包含承載。</span><span class="sxs-lookup"><span data-stu-id="f9209-702">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="f9209-703">藉由在一開始便使用描述性的實體名稱，當聯結資料表變更時便不需要變更名稱。</span><span class="sxs-lookup"><span data-stu-id="f9209-703">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="f9209-704">理想情況下，聯結實體在公司網域中會有自己的自然 (可能為一個單字) 名稱。</span><span class="sxs-lookup"><span data-stu-id="f9209-704">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="f9209-705">例如，「書籍」和「客戶」可連結為一個名為「評分」 的聯結實體。</span><span class="sxs-lookup"><span data-stu-id="f9209-705">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="f9209-706">針對講師-課程多對多關聯性，`CourseAssignment` 會比 `CourseInstructor` 來得好。</span><span class="sxs-lookup"><span data-stu-id="f9209-706">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="f9209-707">複合索引鍵</span><span class="sxs-lookup"><span data-stu-id="f9209-707">Composite key</span></span>

<span data-ttu-id="f9209-708">FK 不可為 Null。</span><span class="sxs-lookup"><span data-stu-id="f9209-708">FKs are not nullable.</span></span> <span data-ttu-id="f9209-709">`CourseAssignment` (`InstructorID` 及 `CourseID`) 中的兩個 FK 一起搭配使用，便可唯一的識別 `CourseAssignment` 資料表中的每一個資料列。</span><span class="sxs-lookup"><span data-stu-id="f9209-709">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="f9209-710">`CourseAssignment` 並不需要其專屬的 PK。</span><span class="sxs-lookup"><span data-stu-id="f9209-710">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="f9209-711">`InstructorID` 及 `CourseID` 屬性的功能便是複合 PK。</span><span class="sxs-lookup"><span data-stu-id="f9209-711">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="f9209-712">為 EF Core 指定複合 PK 的唯一方法是使用 *Fluent API*。</span><span class="sxs-lookup"><span data-stu-id="f9209-712">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="f9209-713">下節會說明如何設定複合 PK。</span><span class="sxs-lookup"><span data-stu-id="f9209-713">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="f9209-714">複合 PK 可確保：</span><span class="sxs-lookup"><span data-stu-id="f9209-714">The composite key ensures:</span></span>

* <span data-ttu-id="f9209-715">一個課程可以有多個資料列。</span><span class="sxs-lookup"><span data-stu-id="f9209-715">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="f9209-716">一個講師可以有多個資料列。</span><span class="sxs-lookup"><span data-stu-id="f9209-716">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="f9209-717">相同講師和課程不可有多個資料列。</span><span class="sxs-lookup"><span data-stu-id="f9209-717">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="f9209-718">由於 `Enrollment` 聯結實體定義了其自身的 PK，因此這種種類的重複項目是可能的。</span><span class="sxs-lookup"><span data-stu-id="f9209-718">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="f9209-719">若要防止這類重複項目：</span><span class="sxs-lookup"><span data-stu-id="f9209-719">To prevent such duplicates:</span></span>

* <span data-ttu-id="f9209-720">在 FK 欄位中新增一個唯一的索引，或</span><span class="sxs-lookup"><span data-stu-id="f9209-720">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="f9209-721">為 `Enrollment` 設定一個主複合索引鍵，與 `CourseAssignment` 相似。</span><span class="sxs-lookup"><span data-stu-id="f9209-721">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="f9209-722">如需詳細資訊，請參閱[索引](/ef/core/modeling/indexes)。</span><span class="sxs-lookup"><span data-stu-id="f9209-722">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="f9209-723">更新資料庫內容</span><span class="sxs-lookup"><span data-stu-id="f9209-723">Update the DB context</span></span>

<span data-ttu-id="f9209-724">將下列醒目提示的程式碼新增至 *Data/SchoolContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="f9209-724">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="f9209-725">上述程式碼會新增一個新實體，並設定 `CourseAssignment` 實體的複合 PK。</span><span class="sxs-lookup"><span data-stu-id="f9209-725">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="f9209-726">屬性的 Fluent API 替代項目</span><span class="sxs-lookup"><span data-stu-id="f9209-726">Fluent API alternative to attributes</span></span>

<span data-ttu-id="f9209-727">上述程式碼中的 `OnModelCreating` 方法使用了 *Fluent API* 設定 EF Core 行為。</span><span class="sxs-lookup"><span data-stu-id="f9209-727">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="f9209-728">此 API 稱為 "fluent" ，因為其常常會用於將一系列的方法呼叫串在一起，使其成為一個單一陳述式。</span><span class="sxs-lookup"><span data-stu-id="f9209-728">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="f9209-729">[下列程式碼](/ef/core/modeling/#use-fluent-api-to-configure-a-model)為 Fluent API 的其中一個範例：</span><span class="sxs-lookup"><span data-stu-id="f9209-729">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="f9209-730">在此教學課程中，Fluent API 僅會用於無法使用屬性完成的資料庫對應。</span><span class="sxs-lookup"><span data-stu-id="f9209-730">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="f9209-731">然而，Fluent API 可指定大部分透過屬性可完成的格式、驗證及對應規則。</span><span class="sxs-lookup"><span data-stu-id="f9209-731">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="f9209-732">某些屬性 (例如 `MinimumLength`) 無法使用 Fluent API 來套用。</span><span class="sxs-lookup"><span data-stu-id="f9209-732">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="f9209-733">`MinimumLength` 不會變更結構描述。它只會套用一項最小長度驗證規則。</span><span class="sxs-lookup"><span data-stu-id="f9209-733">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="f9209-734">某些開發人員偏好單獨使用 Fluent API，使其實體類別保持「整潔」。</span><span class="sxs-lookup"><span data-stu-id="f9209-734">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="f9209-735">屬性和 Fluent API 可混合使用。</span><span class="sxs-lookup"><span data-stu-id="f9209-735">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="f9209-736">有一些設定只能透過 Fluent API 完成 (指定複合 PK)。</span><span class="sxs-lookup"><span data-stu-id="f9209-736">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="f9209-737">有一些設定只能透過屬性完成 (`MinimumLength`)。</span><span class="sxs-lookup"><span data-stu-id="f9209-737">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="f9209-738">使用 Fluent API 或屬性的建議做法為：</span><span class="sxs-lookup"><span data-stu-id="f9209-738">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="f9209-739">從這兩種方法中選擇一項。</span><span class="sxs-lookup"><span data-stu-id="f9209-739">Choose one of these two approaches.</span></span>
* <span data-ttu-id="f9209-740">持續且盡量使用您選擇的方法。</span><span class="sxs-lookup"><span data-stu-id="f9209-740">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="f9209-741">此教學課程中使用到的某些屬性主要用於：</span><span class="sxs-lookup"><span data-stu-id="f9209-741">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="f9209-742">僅驗證 (例如，`MinimumLength`)。</span><span class="sxs-lookup"><span data-stu-id="f9209-742">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="f9209-743">僅 EF Core 組態 (例如，`HasKey`)。</span><span class="sxs-lookup"><span data-stu-id="f9209-743">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="f9209-744">驗證及 EF Core 組態 (例如，`[StringLength(50)]`)。</span><span class="sxs-lookup"><span data-stu-id="f9209-744">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="f9209-745">如需屬性與 Fluent API 的詳細資訊，請參閱[組態方法](/ef/core/modeling/)。</span><span class="sxs-lookup"><span data-stu-id="f9209-745">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="f9209-746">顯示關聯性的實體圖表</span><span class="sxs-lookup"><span data-stu-id="f9209-746">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="f9209-747">下圖顯示了 EF Power Tools 為完成的 School 模型建立的圖表。</span><span class="sxs-lookup"><span data-stu-id="f9209-747">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![實體圖表](complex-data-model/_static/diagram.png)

<span data-ttu-id="f9209-749">上圖顯示︰</span><span class="sxs-lookup"><span data-stu-id="f9209-749">The preceding diagram shows:</span></span>

* <span data-ttu-id="f9209-750">數個一對多關聯性線條 (1 對 \*)。</span><span class="sxs-lookup"><span data-stu-id="f9209-750">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="f9209-751">`Instructor` 和 `OfficeAssignment` 實體之間的一對零或一關聯性線條 (1 對 0..1)。</span><span class="sxs-lookup"><span data-stu-id="f9209-751">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="f9209-752">`Instructor` 和 `Department` 實體之間的零或一對多關聯性線條 (0..1 對 \*)。</span><span class="sxs-lookup"><span data-stu-id="f9209-752">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="f9209-753">使用測試資料植入資料庫</span><span class="sxs-lookup"><span data-stu-id="f9209-753">Seed the DB with Test Data</span></span>

<span data-ttu-id="f9209-754">更新 *Data/DbInitializer.cs* 中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="f9209-754">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="f9209-755">上述程式碼為新的實體提供了種子資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-755">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="f9209-756">此程式碼中的大部分主要用於建立新的實體物件並載入範例資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-756">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="f9209-757">範例資料主要用於測試。</span><span class="sxs-lookup"><span data-stu-id="f9209-757">The sample data is used for testing.</span></span> <span data-ttu-id="f9209-758">如需如何植入多對多聯結資料表的範例，請參閱 `Enrollments` 和 `CourseAssignments`。</span><span class="sxs-lookup"><span data-stu-id="f9209-758">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="f9209-759">新增移轉</span><span class="sxs-lookup"><span data-stu-id="f9209-759">Add a migration</span></span>

<span data-ttu-id="f9209-760">建置專案。</span><span class="sxs-lookup"><span data-stu-id="f9209-760">Build the project.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f9209-761">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9209-761">Visual Studio</span></span>](#tab/visual-studio)

```powershell
Add-Migration ComplexDataModel
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="f9209-762">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f9209-762">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet ef migrations add ComplexDataModel
```

---

<span data-ttu-id="f9209-763">上述命令會顯示關於可能發生資料遺失的警告。</span><span class="sxs-lookup"><span data-stu-id="f9209-763">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="f9209-764">若執行 `database update` 命令，便會產生下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="f9209-764">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="f9209-765">套用移轉</span><span class="sxs-lookup"><span data-stu-id="f9209-765">Apply the migration</span></span>

<span data-ttu-id="f9209-766">現在您有了現有的資料庫，您需要思考如何對其套用未來變更。</span><span class="sxs-lookup"><span data-stu-id="f9209-766">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="f9209-767">本教學課程示範兩種方法：</span><span class="sxs-lookup"><span data-stu-id="f9209-767">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="f9209-768">卸載並重新建立資料庫</span><span class="sxs-lookup"><span data-stu-id="f9209-768">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="f9209-769">[將遷移套用到現有的資料庫](#applyexisting)。</span><span class="sxs-lookup"><span data-stu-id="f9209-769">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="f9209-770">雖然這個方法更複雜且耗時，卻是實際生產環境的慣用方法。</span><span class="sxs-lookup"><span data-stu-id="f9209-770">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="f9209-771">**請注意**：這是本教學課程的選擇性章節。</span><span class="sxs-lookup"><span data-stu-id="f9209-771">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="f9209-772">您可以執行卸除並重新建立步驟，然後略過本節。</span><span class="sxs-lookup"><span data-stu-id="f9209-772">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="f9209-773">如果您希望遵循本章節中的步驟，請不要執行卸除並重新建立的步驟。</span><span class="sxs-lookup"><span data-stu-id="f9209-773">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="f9209-774">卸除並重新建立資料庫</span><span class="sxs-lookup"><span data-stu-id="f9209-774">Drop and re-create the database</span></span>

<span data-ttu-id="f9209-775">更新的 `DbInitializer` 中的程式碼會為新的實體新增種子資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-775">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="f9209-776">若要強制 EF Core 建立新的資料庫，請卸除並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="f9209-776">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f9209-777">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9209-777">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f9209-778">在 [套件管理員主控台]\*\*\*\* (PMC) 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f9209-778">In the **Package Manager Console** (PMC), run the following command:</span></span>

```powershell
Drop-Database
Update-Database
```

<span data-ttu-id="f9209-779">從 PMC 執行 `Get-Help about_EntityFrameworkCore` 以取得說明資訊。</span><span class="sxs-lookup"><span data-stu-id="f9209-779">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="f9209-780">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f9209-780">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f9209-781">開啟命令視窗並巡覽至專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="f9209-781">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="f9209-782">專案資料夾中包含 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f9209-782">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="f9209-783">在命令視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="f9209-783">Enter the following in the command window:</span></span>

```dotnetcli
dotnet ef database drop
dotnet ef database update
```

---

<span data-ttu-id="f9209-784">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f9209-784">Run the app.</span></span> <span data-ttu-id="f9209-785">執行應用程式會執行 `DbInitializer.Initialize` 方法。</span><span class="sxs-lookup"><span data-stu-id="f9209-785">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="f9209-786">`DbInitializer.Initialize` 會填入新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9209-786">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="f9209-787">在 SSOX 中開啟資料庫：</span><span class="sxs-lookup"><span data-stu-id="f9209-787">Open the DB in SSOX:</span></span>

* <span data-ttu-id="f9209-788">若先前已開啟過 SSOX，按一下 [重新整理]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f9209-788">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="f9209-789">展開 **Tables** 節點。</span><span class="sxs-lookup"><span data-stu-id="f9209-789">Expand the **Tables** node.</span></span> <span data-ttu-id="f9209-790">建立的資料表便會顯示。</span><span class="sxs-lookup"><span data-stu-id="f9209-790">The created tables are displayed.</span></span>

![SSOX 中的資料表](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="f9209-792">檢查 **CourseAssignment** 資料表：</span><span class="sxs-lookup"><span data-stu-id="f9209-792">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="f9209-793">以滑鼠右鍵按一下 **CourseAssignment** 資料表，然後選取 [檢視資料]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f9209-793">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="f9209-794">驗證 **CourseAssignment** 資料表中是否包含資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-794">Verify the **CourseAssignment** table contains data.</span></span>

![SSOX 中的 CourseAssignment 資料](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="f9209-796">將移轉套用至現有資料庫</span><span class="sxs-lookup"><span data-stu-id="f9209-796">Apply the migration to the existing database</span></span>

<span data-ttu-id="f9209-797">此為選擇性區段。</span><span class="sxs-lookup"><span data-stu-id="f9209-797">This section is optional.</span></span> <span data-ttu-id="f9209-798">只有當您略過先前[卸除並重新建立資料庫](#drop)一節，這些步驟才有效。</span><span class="sxs-lookup"><span data-stu-id="f9209-798">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="f9209-799">當使用現有的資料執行移轉作業時，某些 FK 條件約束可能會無法透過現有資料滿足。</span><span class="sxs-lookup"><span data-stu-id="f9209-799">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="f9209-800">當您使用的是生產資料時，您必須進行幾個步驟才能移轉現有資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-800">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="f9209-801">本節提供了修正 FK 條件約束違規的範例。</span><span class="sxs-lookup"><span data-stu-id="f9209-801">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="f9209-802">請不要在沒有備份的情況下進行這些程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="f9209-802">Don't make these code changes without a backup.</span></span> <span data-ttu-id="f9209-803">若您已完成了先前的章節並已更新資料庫，請不要進行這些程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="f9209-803">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="f9209-804">*{timestamp}_ComplexDataModel.cs* 檔案包含了下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f9209-804">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="f9209-805">上述程式碼將一個不可為 Null 的 `DepartmentID` FK 新增至 `Course` 資料表。</span><span class="sxs-lookup"><span data-stu-id="f9209-805">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="f9209-806">先前教學課程中的資料庫在 `Course` 中包含了資料列，導致資料表無法藉由移轉進行更新。</span><span class="sxs-lookup"><span data-stu-id="f9209-806">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="f9209-807">若要讓 `ComplexDataModel` 與現有資料進行移轉：</span><span class="sxs-lookup"><span data-stu-id="f9209-807">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="f9209-808">變更程式碼，以給予新資料行 (`DepartmentID`) 一個新的預設值。</span><span class="sxs-lookup"><span data-stu-id="f9209-808">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="f9209-809">建立一個名為 "Temp" 的假部門以作為預設部門之用。</span><span class="sxs-lookup"><span data-stu-id="f9209-809">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="f9209-810">修正外部索引鍵條件約束</span><span class="sxs-lookup"><span data-stu-id="f9209-810">Fix the foreign key constraints</span></span>

<span data-ttu-id="f9209-811">更新 `ComplexDataModel` 類別 `Up` 方法：</span><span class="sxs-lookup"><span data-stu-id="f9209-811">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="f9209-812">開啟 *{timestamp}_ComplexDataModel.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f9209-812">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="f9209-813">將新增 `DepartmentID` 資料行至 `Course` 資料表的程式碼全部標為註解。</span><span class="sxs-lookup"><span data-stu-id="f9209-813">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="f9209-814">新增下列醒目提示程式碼。</span><span class="sxs-lookup"><span data-stu-id="f9209-814">Add the following highlighted code.</span></span> <span data-ttu-id="f9209-815">新的程式碼位於 `.CreateTable( name: "Department"` 區塊後方：</span><span class="sxs-lookup"><span data-stu-id="f9209-815">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="f9209-816">透過上述變更，現有的 `Course` 資料列將會在執行 `ComplexDataModel` `Up` 方法後與 "Temp" 部門相關。</span><span class="sxs-lookup"><span data-stu-id="f9209-816">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="f9209-817">生產環境的應用程式會：</span><span class="sxs-lookup"><span data-stu-id="f9209-817">A production app would:</span></span>

* <span data-ttu-id="f9209-818">包含將 `Department` 資料列及相關 `Course` 資料列新增到新 `Department` 資料列的程式碼或指令碼。</span><span class="sxs-lookup"><span data-stu-id="f9209-818">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="f9209-819">不使用 "Temp" 部門或 `Course.DepartmentID` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="f9209-819">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="f9209-820">下一個教學課程會涵蓋相關資料。</span><span class="sxs-lookup"><span data-stu-id="f9209-820">The next tutorial covers related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f9209-821">其他資源</span><span class="sxs-lookup"><span data-stu-id="f9209-821">Additional resources</span></span>

* [<span data-ttu-id="f9209-822">這個教學課程的 YouTube 版本 (第 1 部分)</span><span class="sxs-lookup"><span data-stu-id="f9209-822">YouTube version of this tutorial(Part 1)</span></span>](https://www.youtube.com/watch?v=0n2f0ObgCoA)
* [<span data-ttu-id="f9209-823">這個教學課程的 YouTube 版本 (第 2 部分)</span><span class="sxs-lookup"><span data-stu-id="f9209-823">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=Je0Z5K1TNmY)

> [!div class="step-by-step"]
> <span data-ttu-id="f9209-824">[上一個](xref:data/ef-rp/migrations) 
> [下一步](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="f9209-824">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end
