---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - 資料模型 - 5/8
author: rick-anderson
description: 在本教學課程中，請新增更多實體和關聯性，並透過指定格式、驗證和對應規則來自訂資料模型。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 930a6f2b860c71b6f499cff53e0d909a130f7948
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637894"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="7680a-103">ASP.NET Core 中的 Razor 頁面與 EF Core - 資料模型 - 5/8</span><span class="sxs-lookup"><span data-stu-id="7680a-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7680a-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7680a-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="7680a-105">先前的教學課程建立了基本的資料模型，該模型由三個實體組成。</span><span class="sxs-lookup"><span data-stu-id="7680a-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="7680a-106">在本教學課程中：</span><span class="sxs-lookup"><span data-stu-id="7680a-106">In this tutorial:</span></span>

* <span data-ttu-id="7680a-107">新增更多實體和關聯性。</span><span class="sxs-lookup"><span data-stu-id="7680a-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="7680a-108">藉由指定格式、驗證和資料庫對應規則來自訂資料模型。</span><span class="sxs-lookup"><span data-stu-id="7680a-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="7680a-109">完整資料模型的實體類別如下列圖例所示：</span><span class="sxs-lookup"><span data-stu-id="7680a-109">The entity classes for the completed data model is shown in the following illustration:</span></span>

![實體圖表](complex-data-model/_static/diagram.png)

<span data-ttu-id="7680a-111">若您遇到無法解決的問題，請下載[完整應用程式](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)。</span><span class="sxs-lookup"><span data-stu-id="7680a-111">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="7680a-112">使用屬性自訂資料模型</span><span class="sxs-lookup"><span data-stu-id="7680a-112">Customize the data model with attributes</span></span>

<span data-ttu-id="7680a-113">在本節中，您會使用屬性自訂資料模型。</span><span class="sxs-lookup"><span data-stu-id="7680a-113">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="7680a-114">DataType 屬性</span><span class="sxs-lookup"><span data-stu-id="7680a-114">The DataType attribute</span></span>

<span data-ttu-id="7680a-115">學生頁面目前顯示了註冊日期的時間。</span><span class="sxs-lookup"><span data-stu-id="7680a-115">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="7680a-116">一般而言，日期欄位只會顯示日期，而非時間。</span><span class="sxs-lookup"><span data-stu-id="7680a-116">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="7680a-117">使用下列醒目提示的程式碼更新 *Models/Student.cs*：</span><span class="sxs-lookup"><span data-stu-id="7680a-117">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="7680a-118">[DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) 屬性會指定一個比資料庫內建類型更明確的資料類型。</span><span class="sxs-lookup"><span data-stu-id="7680a-118">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="7680a-119">在此情況下，該欄位應該只顯示日期，而不會同時顯示日期和時間。</span><span class="sxs-lookup"><span data-stu-id="7680a-119">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="7680a-120">[DataType 列舉](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1)提供了許多資料類型，例如　Date、Time、PhoneNumber、Currency、EmailAddress 等。`DataType` 屬性也可以讓應用程式自動提供限定於某些類型的功能。</span><span class="sxs-lookup"><span data-stu-id="7680a-120">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="7680a-121">例如：</span><span class="sxs-lookup"><span data-stu-id="7680a-121">For example:</span></span>

* <span data-ttu-id="7680a-122">`DataType.EmailAddress` 會自動建立 `mailto:` 連結。</span><span class="sxs-lookup"><span data-stu-id="7680a-122">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="7680a-123">`DataType.Date` 在大多數的瀏覽器中都會提供日期選取器。</span><span class="sxs-lookup"><span data-stu-id="7680a-123">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="7680a-124">`DataType` 屬性會發出 HTML 5 `data-` (發音為 data dash) 屬性，可讓 HTML 5 瀏覽器取用。</span><span class="sxs-lookup"><span data-stu-id="7680a-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="7680a-125">`DataType` 屬性不會提供驗證。</span><span class="sxs-lookup"><span data-stu-id="7680a-125">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="7680a-126">`DataType.Date` 未指定顯示日期的格式。</span><span class="sxs-lookup"><span data-stu-id="7680a-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="7680a-127">根據預設，日期欄位會依照根據伺服器的 [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) 為基礎的預設格式顯示。</span><span class="sxs-lookup"><span data-stu-id="7680a-127">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="7680a-128">`DisplayFormat` 屬性用來明確指定日期格式：</span><span class="sxs-lookup"><span data-stu-id="7680a-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="7680a-129">`ApplyFormatInEditMode` 設定會指定格式也應套用在編輯 UI。</span><span class="sxs-lookup"><span data-stu-id="7680a-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="7680a-130">某些欄位不應該使用 `ApplyFormatInEditMode`。</span><span class="sxs-lookup"><span data-stu-id="7680a-130">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="7680a-131">例如，貨幣符號通常不應顯示在編輯文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="7680a-131">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="7680a-132">`DisplayFormat` 屬性可由自身使用。</span><span class="sxs-lookup"><span data-stu-id="7680a-132">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="7680a-133">通常使用 `DataType` 屬性搭配 `DisplayFormat` 屬性是一個不錯的做法。</span><span class="sxs-lookup"><span data-stu-id="7680a-133">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="7680a-134">`DataType` 屬性會將資料的語意以其在螢幕上呈現方式的相反方式傳遞。</span><span class="sxs-lookup"><span data-stu-id="7680a-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="7680a-135">`DataType` 屬性提供了下列優點，並且這些優點在 `DisplayFormat` 中無法使用：</span><span class="sxs-lookup"><span data-stu-id="7680a-135">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="7680a-136">瀏覽器可以啟用 HTML5 功能。</span><span class="sxs-lookup"><span data-stu-id="7680a-136">The browser can enable HTML5 features.</span></span> <span data-ttu-id="7680a-137">例如，顯示日曆控制項、適合地區設定的貨幣符號、電子郵件連結、用戶端輸入驗證等。</span><span class="sxs-lookup"><span data-stu-id="7680a-137">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="7680a-138">根據預設，瀏覽器將根據地區設定，使用正確的格式呈現資料。</span><span class="sxs-lookup"><span data-stu-id="7680a-138">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="7680a-139">如需詳細資訊，請參閱 [\<input> 標籤協助程式文件](xref:mvc/views/working-with-forms#the-input-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="7680a-139">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="7680a-140">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7680a-140">Run the app.</span></span> <span data-ttu-id="7680a-141">巡覽至 Students [索引] 頁面。</span><span class="sxs-lookup"><span data-stu-id="7680a-141">Navigate to the Students Index page.</span></span> <span data-ttu-id="7680a-142">時間將不再顯示。</span><span class="sxs-lookup"><span data-stu-id="7680a-142">Times are no longer displayed.</span></span> <span data-ttu-id="7680a-143">使用 `Student` 模型的每個檢視現在都只會顯示日期，而不會顯示時間。</span><span class="sxs-lookup"><span data-stu-id="7680a-143">Every view that uses the `Student` model displays the date without time.</span></span>

![顯示日期而不顯示時間的 Students [索引] 頁面](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="7680a-145">StringLength 屬性</span><span class="sxs-lookup"><span data-stu-id="7680a-145">The StringLength attribute</span></span>

<span data-ttu-id="7680a-146">您可使用屬性指定資料驗證規則和驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="7680a-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="7680a-147">[StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) 屬性指定了在資料欄位中允許的最小及最大字元長度。</span><span class="sxs-lookup"><span data-stu-id="7680a-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="7680a-148">`StringLength` 屬性同時也提供了用戶端和伺服器端的驗證。</span><span class="sxs-lookup"><span data-stu-id="7680a-148">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="7680a-149">最小值對資料庫結構描述不會造成任何影響。</span><span class="sxs-lookup"><span data-stu-id="7680a-149">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="7680a-150">使用下列程式碼更新 `Student` 模型：</span><span class="sxs-lookup"><span data-stu-id="7680a-150">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="7680a-151">上述的程式碼會限制名稱不得超過 50 個字元。</span><span class="sxs-lookup"><span data-stu-id="7680a-151">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="7680a-152">`StringLength` 屬性不會防止使用者在名稱中輸入空白字元。</span><span class="sxs-lookup"><span data-stu-id="7680a-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="7680a-153">[RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) 屬性可用於對輸入套用限制。</span><span class="sxs-lookup"><span data-stu-id="7680a-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="7680a-154">例如，下列程式碼會要求第一個字元必須是大寫，其餘字元則必須是英文字母：</span><span class="sxs-lookup"><span data-stu-id="7680a-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="7680a-155">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="7680a-155">Run the app:</span></span>

* <span data-ttu-id="7680a-156">巡覽至 Students 頁面。</span><span class="sxs-lookup"><span data-stu-id="7680a-156">Navigate to the Students page.</span></span>
* <span data-ttu-id="7680a-157">選取 [新建]，然後輸入超過 50 個字元的名稱。</span><span class="sxs-lookup"><span data-stu-id="7680a-157">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="7680a-158">選取 [建立]，用戶端驗證便會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="7680a-158">Select **Create**, client-side validation shows an error message.</span></span>

![顯示字元長度錯誤的 Students [索引] 頁面](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="7680a-160">在 [SQL Server 物件總管] (SSOX) 中，按兩下 [Student] 資料表來開啟 Student 資料表設計工具。</span><span class="sxs-lookup"><span data-stu-id="7680a-160">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![移轉之前於 SSOX 中的 Students 資料表](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="7680a-162">上述影像顯示了 `Student` 資料表的結構描述。</span><span class="sxs-lookup"><span data-stu-id="7680a-162">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="7680a-163">名稱欄位的類型為 `nvarchar(MAX)`，因為移轉尚未在資料庫中執行。</span><span class="sxs-lookup"><span data-stu-id="7680a-163">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="7680a-164">在本教學課程的稍後執行移轉後，名稱欄位便會成為 `nvarchar(50)`。</span><span class="sxs-lookup"><span data-stu-id="7680a-164">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="7680a-165">Column 屬性</span><span class="sxs-lookup"><span data-stu-id="7680a-165">The Column attribute</span></span>

<span data-ttu-id="7680a-166">可控制類別和屬性如何對應到資料庫的屬性。</span><span class="sxs-lookup"><span data-stu-id="7680a-166">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="7680a-167">在本節中，`Column` 屬性會用於將 `FirstMidName` 屬性的名稱對應到資料庫中的 "FirstName"。</span><span class="sxs-lookup"><span data-stu-id="7680a-167">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="7680a-168">當建立資料庫時，模型上的屬性名稱會用於資料行名稱 (除了使用 `Column` 屬性時之外)。</span><span class="sxs-lookup"><span data-stu-id="7680a-168">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="7680a-169">`Student` 模型針對名字欄位使用 `FirstMidName`，因為欄位中可能也會包含中間名。</span><span class="sxs-lookup"><span data-stu-id="7680a-169">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="7680a-170">使用下列醒目提示程式碼更新 *Student.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="7680a-170">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="7680a-171">經過上述變更之後，應用程式中的 `Student.FirstMidName` 會對應到 `Student` 資料表的 `FirstName` 資料行。</span><span class="sxs-lookup"><span data-stu-id="7680a-171">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="7680a-172">新增 `Column` 屬性會變更支援 `SchoolContext` 的模型。</span><span class="sxs-lookup"><span data-stu-id="7680a-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="7680a-173">支援 `SchoolContext` 的模型不再符合資料庫。</span><span class="sxs-lookup"><span data-stu-id="7680a-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="7680a-174">若應用程式在套用移轉之前執行，便會產生下列例外狀況：</span><span class="sxs-lookup"><span data-stu-id="7680a-174">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="7680a-175">若要更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="7680a-175">To update the DB:</span></span>

* <span data-ttu-id="7680a-176">建置專案。</span><span class="sxs-lookup"><span data-stu-id="7680a-176">Build the project.</span></span>
* <span data-ttu-id="7680a-177">在專案資料夾中開啟命令視窗。</span><span class="sxs-lookup"><span data-stu-id="7680a-177">Open a command window in the project folder.</span></span> <span data-ttu-id="7680a-178">輸入下列命令來建立新的移轉並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="7680a-178">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7680a-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7680a-179">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7680a-180">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7680a-180">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

------

<span data-ttu-id="7680a-181">`migrations add ColumnFirstName` 命令會產生下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="7680a-181">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="7680a-182">產生警告的理由是名稱欄位現在限制長度為 50 個字元。</span><span class="sxs-lookup"><span data-stu-id="7680a-182">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="7680a-183">若在資料庫中有名稱超過 50 個字元，第 51 個字元到最後一個字元便會遺失。</span><span class="sxs-lookup"><span data-stu-id="7680a-183">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="7680a-184">測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="7680a-184">Test the app.</span></span>

<span data-ttu-id="7680a-185">在 SSOX 中開啟 Student 資料表：</span><span class="sxs-lookup"><span data-stu-id="7680a-185">Open the Student table in SSOX:</span></span>

![移轉之後 SSOX 中的 Students 資料表](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="7680a-187">在移轉套用之前，名稱資料行的類型為 [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)。</span><span class="sxs-lookup"><span data-stu-id="7680a-187">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="7680a-188">名稱資料行現在為 `nvarchar(50)`。</span><span class="sxs-lookup"><span data-stu-id="7680a-188">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="7680a-189">資料行的名稱已從 `FirstMidName` 變更為 `FirstName`。</span><span class="sxs-lookup"><span data-stu-id="7680a-189">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="7680a-190">在下節中，在某些階段建置應用程式會產生編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="7680a-190">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="7680a-191">指令會指定何時應建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="7680a-191">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="7680a-192">Student 實體更新</span><span class="sxs-lookup"><span data-stu-id="7680a-192">Student entity update</span></span>

![Student 實體](complex-data-model/_static/student-entity.png)

<span data-ttu-id="7680a-194">使用下列程式碼更新 *Models/Student.cs*：</span><span class="sxs-lookup"><span data-stu-id="7680a-194">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="7680a-195">Required 屬性</span><span class="sxs-lookup"><span data-stu-id="7680a-195">The Required attribute</span></span>

<span data-ttu-id="7680a-196">`Required` 屬性會讓名稱屬性成為必要欄位。</span><span class="sxs-lookup"><span data-stu-id="7680a-196">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="7680a-197">針對不可為 Null 的類型，例如實值型別 (`DateTime`、`int`、`double` 等) 等，`Required` 屬性是不需要的。</span><span class="sxs-lookup"><span data-stu-id="7680a-197">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="7680a-198">不可為 Null 的類型會自動視為必要欄位。</span><span class="sxs-lookup"><span data-stu-id="7680a-198">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="7680a-199">`Required` 屬性可由 `StringLength` 屬性中的最小長度參數取代：</span><span class="sxs-lookup"><span data-stu-id="7680a-199">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="7680a-200">Display 屬性</span><span class="sxs-lookup"><span data-stu-id="7680a-200">The Display attribute</span></span>

<span data-ttu-id="7680a-201">`Display` 屬性指定了文字方塊的標題應為「名字」、「姓氏」、「全名」及「註冊日期」。</span><span class="sxs-lookup"><span data-stu-id="7680a-201">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="7680a-202">預設標題中沒有使用空白分隔文字，例如「姓氏」。</span><span class="sxs-lookup"><span data-stu-id="7680a-202">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="7680a-203">FullName 計算屬性</span><span class="sxs-lookup"><span data-stu-id="7680a-203">The FullName calculated property</span></span>

<span data-ttu-id="7680a-204">`FullName` 為一個計算屬性，會傳回藉由串連兩個其他屬性而建立的值。</span><span class="sxs-lookup"><span data-stu-id="7680a-204">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="7680a-205">`FullName` 無法進行設定，其只有 get 存取子。</span><span class="sxs-lookup"><span data-stu-id="7680a-205">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="7680a-206">資料庫中不會建立 `FullName` 資料行。</span><span class="sxs-lookup"><span data-stu-id="7680a-206">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="7680a-207">建立 Instructor 實體</span><span class="sxs-lookup"><span data-stu-id="7680a-207">Create the Instructor Entity</span></span>

![Instructor 實體](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="7680a-209">使用下列程式碼建立 *Models/Instructor.cs*：</span><span class="sxs-lookup"><span data-stu-id="7680a-209">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="7680a-210">在同一行上可以有多個屬性。</span><span class="sxs-lookup"><span data-stu-id="7680a-210">Multiple attributes can be on one line.</span></span> <span data-ttu-id="7680a-211">`HireDate` 屬性可以下列方式撰寫：</span><span class="sxs-lookup"><span data-stu-id="7680a-211">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="7680a-212">CourseAssignments 和 OfficeAssignment 導覽屬性</span><span class="sxs-lookup"><span data-stu-id="7680a-212">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="7680a-213">`CourseAssignments` 和 `OfficeAssignment` 屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="7680a-213">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="7680a-214">由於講師可以教授任何數量的課程，因此 `CourseAssignments` 已定義為一個集合。</span><span class="sxs-lookup"><span data-stu-id="7680a-214">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="7680a-215">若導覽屬性中保留了多個實體：</span><span class="sxs-lookup"><span data-stu-id="7680a-215">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="7680a-216">它必須是一種清單類型，可對其中的實體進行新增、刪除和更新。</span><span class="sxs-lookup"><span data-stu-id="7680a-216">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="7680a-217">導覽屬性類型包括：</span><span class="sxs-lookup"><span data-stu-id="7680a-217">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="7680a-218">若已指定 `ICollection<T>`，EF Core 會根據預設建立一個 `HashSet<T>` 集合。</span><span class="sxs-lookup"><span data-stu-id="7680a-218">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="7680a-219">`CourseAssignment` 實體會在本節的多對多關聯性中解釋。</span><span class="sxs-lookup"><span data-stu-id="7680a-219">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="7680a-220">Contoso 大學商務規則訂定了講師最多只能有一間辦公室。</span><span class="sxs-lookup"><span data-stu-id="7680a-220">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="7680a-221">`OfficeAssignment` 屬性保留了一個單一 `OfficeAssignment` 實體。</span><span class="sxs-lookup"><span data-stu-id="7680a-221">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="7680a-222">若沒有指派任何辦公室，則 `OfficeAssignment` 為 Null。</span><span class="sxs-lookup"><span data-stu-id="7680a-222">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="7680a-223">建立 OfficeAssignment 實體</span><span class="sxs-lookup"><span data-stu-id="7680a-223">Create the OfficeAssignment entity</span></span>

![OfficeAssignment 實體](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="7680a-225">使用下列程式碼建立 *Models/OfficeAssignment.cs*：</span><span class="sxs-lookup"><span data-stu-id="7680a-225">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="7680a-226">Key 屬性</span><span class="sxs-lookup"><span data-stu-id="7680a-226">The Key attribute</span></span>

<span data-ttu-id="7680a-227">`[Key]` 屬性用於將某個屬性名稱不是 classnameID 或 ID 的屬性識別為主索引鍵 (PK)。</span><span class="sxs-lookup"><span data-stu-id="7680a-227">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="7680a-228">在 `Instructor` 和 `OfficeAssignment` 實體之間有一對零或一關聯性。</span><span class="sxs-lookup"><span data-stu-id="7680a-228">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="7680a-229">辦公室指派只存在於與其受指派的講師關聯中。</span><span class="sxs-lookup"><span data-stu-id="7680a-229">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="7680a-230">`OfficeAssignment` PK 同時也是其連結到 `Instructor` 實體的外部索引鍵 (FK)。</span><span class="sxs-lookup"><span data-stu-id="7680a-230">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="7680a-231">EF Core 無法自動將 `InstructorID` 識別為 `OfficeAssignment` 的主索引鍵，因為：</span><span class="sxs-lookup"><span data-stu-id="7680a-231">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="7680a-232">`InstructorID` 並未遵循 ID 或 classnameID 的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="7680a-232">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="7680a-233">因此，必須使用 `Key` 屬性將 `InstructorID` 識別為 PK：</span><span class="sxs-lookup"><span data-stu-id="7680a-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="7680a-234">根據預設，EF Core 會將索引鍵作為非資料庫產生的屬性處置，因為該資料行主要用於識別關聯性。</span><span class="sxs-lookup"><span data-stu-id="7680a-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="7680a-235">Instructor 導覽屬性</span><span class="sxs-lookup"><span data-stu-id="7680a-235">The Instructor navigation property</span></span>

<span data-ttu-id="7680a-236">`Instructor` 實體的 `OfficeAssignment` 導覽屬性可為 Null，因為：</span><span class="sxs-lookup"><span data-stu-id="7680a-236">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="7680a-237">參考型別 (例如類別可為 Null)。</span><span class="sxs-lookup"><span data-stu-id="7680a-237">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="7680a-238">講師可能沒有辦公室指派。</span><span class="sxs-lookup"><span data-stu-id="7680a-238">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="7680a-239">`OfficeAssignment` 實體有不可為 Null 的`Instructor` 導覽屬性，因為：</span><span class="sxs-lookup"><span data-stu-id="7680a-239">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="7680a-240">`InstructorID` 不可為 Null。</span><span class="sxs-lookup"><span data-stu-id="7680a-240">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="7680a-241">辦公室指派無法獨立於講師之外存在。</span><span class="sxs-lookup"><span data-stu-id="7680a-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="7680a-242">當 `Instructor` 實體有相關的 `OfficeAssignment` 實體時，每一個實體都會在其導覽屬性中包含一個其他實體的參考。</span><span class="sxs-lookup"><span data-stu-id="7680a-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="7680a-243">`[Required]` 屬性可套用到 `Instructor` 導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="7680a-243">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="7680a-244">上述程式碼指定了必須要有相關的講師。</span><span class="sxs-lookup"><span data-stu-id="7680a-244">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="7680a-245">上述程式碼是不必要的，因為 `InstructorID` 外部索引鍵 (同時也是 PK) 不可為 Null。</span><span class="sxs-lookup"><span data-stu-id="7680a-245">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="7680a-246">修改 Course 實體</span><span class="sxs-lookup"><span data-stu-id="7680a-246">Modify the Course Entity</span></span>

![Course 實體](complex-data-model/_static/course-entity.png)

<span data-ttu-id="7680a-248">使用下列程式碼更新 *Models/Course.cs*：</span><span class="sxs-lookup"><span data-stu-id="7680a-248">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="7680a-249">`Course` 實體具有外部索引鍵 (FK) 屬性`DepartmentID`。</span><span class="sxs-lookup"><span data-stu-id="7680a-249">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="7680a-250">`DepartmentID` 會指向相關的 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="7680a-250">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="7680a-251">`Course` 實體具有一個 `Department` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="7680a-251">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="7680a-252">當資料模型針對相關實體有一個導覽屬性時，EF Core 不需要針對該模型具備 FK 屬性。</span><span class="sxs-lookup"><span data-stu-id="7680a-252">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="7680a-253">EF Core 會自動在資料庫中需要的任何地方建立 FK。</span><span class="sxs-lookup"><span data-stu-id="7680a-253">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="7680a-254">EF Core 會為了自動建立的 FK 建立[陰影屬性](/ef/core/modeling/shadow-properties)。</span><span class="sxs-lookup"><span data-stu-id="7680a-254">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="7680a-255">在資料模型中具備 FK 可讓更新變得更為簡單和有效率。</span><span class="sxs-lookup"><span data-stu-id="7680a-255">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="7680a-256">例如，假設有一個模型，當中「不」包含 `DepartmentID` FK 屬性。</span><span class="sxs-lookup"><span data-stu-id="7680a-256">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="7680a-257">當擷取課程實體以進行編輯時：</span><span class="sxs-lookup"><span data-stu-id="7680a-257">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="7680a-258">若沒有明確載入，`Department` 實體將為 Null。</span><span class="sxs-lookup"><span data-stu-id="7680a-258">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="7680a-259">若要更新課程實體，必須先擷取 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="7680a-259">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="7680a-260">當 FK 屬性 `DepartmentID` 包含在資料模型中時，便不需要在更新前擷取 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="7680a-260">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="7680a-261">DatabaseGenerated 屬性</span><span class="sxs-lookup"><span data-stu-id="7680a-261">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="7680a-262">`[DatabaseGenerated(DatabaseGeneratedOption.None)]` 屬性會指定 PK 是由應用程式提供的，而非資料庫產生的。</span><span class="sxs-lookup"><span data-stu-id="7680a-262">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="7680a-263">根據預設，EF Core 會假設 PK 值都是由資料庫產生的。</span><span class="sxs-lookup"><span data-stu-id="7680a-263">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="7680a-264">由資料庫產生 PK 值通常都是最佳做法。</span><span class="sxs-lookup"><span data-stu-id="7680a-264">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="7680a-265">針對 `Course` 實體，使用者指定了 PK。</span><span class="sxs-lookup"><span data-stu-id="7680a-265">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="7680a-266">例如，課程號碼 1000 系列表示數學部門的課程，2000 系列則為英文部門的課程。</span><span class="sxs-lookup"><span data-stu-id="7680a-266">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="7680a-267">`DatabaseGenerated` 屬性也可用於產生預設值。</span><span class="sxs-lookup"><span data-stu-id="7680a-267">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="7680a-268">例如，資料庫會自動產生日期欄位來記錄資料列建立或更新的日期。</span><span class="sxs-lookup"><span data-stu-id="7680a-268">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="7680a-269">如需詳細資訊，請參閱[產生的屬性](/ef/core/modeling/generated-properties)。</span><span class="sxs-lookup"><span data-stu-id="7680a-269">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="7680a-270">外部索引鍵及導覽屬性</span><span class="sxs-lookup"><span data-stu-id="7680a-270">Foreign key and navigation properties</span></span>

<span data-ttu-id="7680a-271">`Course` 實體中的外部索引鍵 (FK) 屬性和導覽屬性反映了下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="7680a-271">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="7680a-272">由於一個課程已指派給了一個部門，因此當中具有 `DepartmentID` FK 和 `Department` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="7680a-272">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="7680a-273">由於課程可由任何數量的學生進行註冊，因此 `Enrollments` 導覽屬性為一個集合：</span><span class="sxs-lookup"><span data-stu-id="7680a-273">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="7680a-274">課程可由多個講師進行教授，因此 `CourseAssignments` 導覽屬性為一個集合：</span><span class="sxs-lookup"><span data-stu-id="7680a-274">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="7680a-275">`CourseAssignment` 會在[稍後](#many-to-many-relationships)進行解釋。</span><span class="sxs-lookup"><span data-stu-id="7680a-275">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="7680a-276">建立 Department 實體</span><span class="sxs-lookup"><span data-stu-id="7680a-276">Create the Department entity</span></span>

![Department 實體](complex-data-model/_static/department-entity.png)

<span data-ttu-id="7680a-278">使用下列程式碼建立 *Models/Department.cs*：</span><span class="sxs-lookup"><span data-stu-id="7680a-278">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="7680a-279">Column 屬性</span><span class="sxs-lookup"><span data-stu-id="7680a-279">The Column attribute</span></span>

<span data-ttu-id="7680a-280">先前，`Column` 屬性主要用於變更資料行的名稱對應。</span><span class="sxs-lookup"><span data-stu-id="7680a-280">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="7680a-281">在 `Department` 實體的程式碼中，`Column` 屬性則用於變更 SQL 資料類型對應。</span><span class="sxs-lookup"><span data-stu-id="7680a-281">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="7680a-282">`Budget` 資料行則是使用資料庫中的 SQL Server 金額類型定義：</span><span class="sxs-lookup"><span data-stu-id="7680a-282">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="7680a-283">通常您不需要資料行對應。</span><span class="sxs-lookup"><span data-stu-id="7680a-283">Column mapping is generally not required.</span></span> <span data-ttu-id="7680a-284">EF Core 通常會根據屬性的 CLR 類型選擇適當的 SQL Server 資料類型。</span><span class="sxs-lookup"><span data-stu-id="7680a-284">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="7680a-285">CLR `decimal` 類型會對應到 SQL Server `decimal` 類型。</span><span class="sxs-lookup"><span data-stu-id="7680a-285">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="7680a-286">由於 `Budget` 是貨幣，因此金額資料類型會比較適合貨幣。</span><span class="sxs-lookup"><span data-stu-id="7680a-286">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="7680a-287">外部索引鍵及導覽屬性</span><span class="sxs-lookup"><span data-stu-id="7680a-287">Foreign key and navigation properties</span></span>

<span data-ttu-id="7680a-288">FK 及導覽屬性反映了下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="7680a-288">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="7680a-289">部門可能有或可能沒有系統管理員。</span><span class="sxs-lookup"><span data-stu-id="7680a-289">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="7680a-290">系統管理員一律為講師。</span><span class="sxs-lookup"><span data-stu-id="7680a-290">An administrator is always an instructor.</span></span> <span data-ttu-id="7680a-291">因此，`InstructorID` 已作為 FK 包含在 `Instructor` 實體中。</span><span class="sxs-lookup"><span data-stu-id="7680a-291">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="7680a-292">導覽屬性已命名為 `Administrator`，但其中保留了一個 `Instructor` 實體：</span><span class="sxs-lookup"><span data-stu-id="7680a-292">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="7680a-293">上述程式碼中的問號 (?) 表示屬性可為 Null。</span><span class="sxs-lookup"><span data-stu-id="7680a-293">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="7680a-294">部門中可能包含許多課程，因此當中包含了一個 Course 導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="7680a-294">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="7680a-295">注意:根據慣例，EF Core 會為不可為 Null 的 FK 和多對多關聯性啟用串聯刪除。</span><span class="sxs-lookup"><span data-stu-id="7680a-295">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="7680a-296">串聯刪除可能會導致循環的串聯刪除規則。</span><span class="sxs-lookup"><span data-stu-id="7680a-296">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="7680a-297">循環串聯刪除規則會在新增移轉時造成例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7680a-297">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="7680a-298">例如，若 `Department.InstructorID` 屬性並未定義為可為 Null：</span><span class="sxs-lookup"><span data-stu-id="7680a-298">For example, if the `Department.InstructorID` property wasn't defined as nullable:</span></span>

* <span data-ttu-id="7680a-299">EF Core 會在部門遭到刪除時設定串聯刪除規則，以刪除講師。</span><span class="sxs-lookup"><span data-stu-id="7680a-299">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="7680a-300">在刪除部門時刪除講師並非預期的行為。</span><span class="sxs-lookup"><span data-stu-id="7680a-300">Deleting the instructor when the department is deleted isn't the intended behavior.</span></span>

<span data-ttu-id="7680a-301">若商務規則要求 `InstructorID` 屬性為不可為 Null，則請使用下列 Fluent API 陳述式：</span><span class="sxs-lookup"><span data-stu-id="7680a-301">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="7680a-302">上述的程式碼會在部門-講師關聯性上停用串聯刪除。</span><span class="sxs-lookup"><span data-stu-id="7680a-302">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="7680a-303">更新 Enrollment 實體</span><span class="sxs-lookup"><span data-stu-id="7680a-303">Update the Enrollment entity</span></span>

<span data-ttu-id="7680a-304">註冊記錄是某位學生參加的一門課程。</span><span class="sxs-lookup"><span data-stu-id="7680a-304">An enrollment record is for one course taken by one student.</span></span>

![Enrollment 實體](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="7680a-306">使用下列程式碼更新 *Models/Enrollment.cs*：</span><span class="sxs-lookup"><span data-stu-id="7680a-306">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="7680a-307">外部索引鍵及導覽屬性</span><span class="sxs-lookup"><span data-stu-id="7680a-307">Foreign key and navigation properties</span></span>

<span data-ttu-id="7680a-308">FK 屬性及導覽屬性反映了下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="7680a-308">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="7680a-309">註冊記錄乃針對一個課程，因此當中包含了一個 `CourseID` FK 屬性及一個 `Course` 導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="7680a-309">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="7680a-310">註冊記錄乃針對一位學生，因此當中包含了一個 `StudentID` FK 屬性及一個 `Student` 導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="7680a-310">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="7680a-311">多對多關聯性</span><span class="sxs-lookup"><span data-stu-id="7680a-311">Many-to-Many Relationships</span></span>

<span data-ttu-id="7680a-312">在 `Student` 和 `Course` 實體之間存在一個多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="7680a-312">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="7680a-313">`Enrollment` 實體的功能為資料庫中一個「具有承載」的多對多聯結資料表。</span><span class="sxs-lookup"><span data-stu-id="7680a-313">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="7680a-314">「具有承載」表示 `Enrollment` 資料表除了聯結資料表 (在此案例中為 PK 和 `Grade`) 的 FK 之外，還包含了額外的資料。</span><span class="sxs-lookup"><span data-stu-id="7680a-314">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="7680a-315">下列圖例展示了在實體圖表中這些關聯性的樣子。</span><span class="sxs-lookup"><span data-stu-id="7680a-315">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="7680a-316">(此圖表是使用 EF 6.x 的 [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) 產生的。</span><span class="sxs-lookup"><span data-stu-id="7680a-316">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="7680a-317">建立圖表並不是此教學課程的一部分)。</span><span class="sxs-lookup"><span data-stu-id="7680a-317">Creating the diagram isn't part of the tutorial.)</span></span>

![學生-課程多對多關聯性](complex-data-model/_static/student-course.png)

<span data-ttu-id="7680a-319">每個關聯性線條都在其中一端有一個「1」，並在另外一端有一個「星號 (\*)」，顯示其為一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="7680a-319">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="7680a-320">若 `Enrollment` 資料表並未包含成績資訊，則其便只需要包含兩個 FK (`CourseID` 和 `StudentID`)。</span><span class="sxs-lookup"><span data-stu-id="7680a-320">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="7680a-321">沒有承載的多對多聯結資料表有時候也稱為「純聯結資料表 (PJT)」。</span><span class="sxs-lookup"><span data-stu-id="7680a-321">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="7680a-322">`Instructor` 和 `Course` 實體具有使用了純聯結資料表的多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="7680a-322">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="7680a-323">注意:EF 6.x 支援多對多關聯性的隱含聯結資料表，但 EF Core 並不支援。</span><span class="sxs-lookup"><span data-stu-id="7680a-323">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="7680a-324">如需詳細資訊，請參閱 [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/) (EF Core 2.0 中的多對多關聯性)。</span><span class="sxs-lookup"><span data-stu-id="7680a-324">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="7680a-325">CourseAssignment 實體</span><span class="sxs-lookup"><span data-stu-id="7680a-325">The CourseAssignment entity</span></span>

![CourseAssignment 實體](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="7680a-327">使用下列程式碼建立 *Models/CourseAssignment.cs*：</span><span class="sxs-lookup"><span data-stu-id="7680a-327">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="7680a-328">講師-課程</span><span class="sxs-lookup"><span data-stu-id="7680a-328">Instructor-to-Courses</span></span>

![講師-課程 m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="7680a-330">講師-課程多對多關聯性：</span><span class="sxs-lookup"><span data-stu-id="7680a-330">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="7680a-331">需要一個由實體集代表的聯結資料表。</span><span class="sxs-lookup"><span data-stu-id="7680a-331">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="7680a-332">為一個純聯結資料表 (沒有承載的資料表)。</span><span class="sxs-lookup"><span data-stu-id="7680a-332">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="7680a-333">通常會將聯結實體命名為 `EntityName1EntityName2`。</span><span class="sxs-lookup"><span data-stu-id="7680a-333">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="7680a-334">例如，使用此模式的講師-課程聯結資料表為 `CourseInstructor`。</span><span class="sxs-lookup"><span data-stu-id="7680a-334">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="7680a-335">不過，我們建議使用可描述關聯性的名稱。</span><span class="sxs-lookup"><span data-stu-id="7680a-335">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="7680a-336">資料模型一開始都是簡單的，之後便會持續成長。</span><span class="sxs-lookup"><span data-stu-id="7680a-336">Data models start out simple and grow.</span></span> <span data-ttu-id="7680a-337">無承載聯結 (PJT) 常常會演變為包含承載。</span><span class="sxs-lookup"><span data-stu-id="7680a-337">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="7680a-338">藉由在一開始便使用描述性的實體名稱，當聯結資料表變更時便不需要變更名稱。</span><span class="sxs-lookup"><span data-stu-id="7680a-338">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="7680a-339">理想情況下，聯結實體在公司網域中會有自己的自然 (可能為一個單字) 名稱。</span><span class="sxs-lookup"><span data-stu-id="7680a-339">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="7680a-340">例如，「書籍」和「客戶」可連結為一個名為「評分」 的聯結實體。</span><span class="sxs-lookup"><span data-stu-id="7680a-340">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="7680a-341">針對講師-課程多對多關聯性，`CourseAssignment` 會比 `CourseInstructor` 來得好。</span><span class="sxs-lookup"><span data-stu-id="7680a-341">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="7680a-342">複合索引鍵</span><span class="sxs-lookup"><span data-stu-id="7680a-342">Composite key</span></span>

<span data-ttu-id="7680a-343">FK 不可為 Null。</span><span class="sxs-lookup"><span data-stu-id="7680a-343">FKs are not nullable.</span></span> <span data-ttu-id="7680a-344">`CourseAssignment` (`InstructorID` 及 `CourseID`) 中的兩個 FK 一起搭配使用，便可唯一的識別 `CourseAssignment` 資料表中的每一個資料列。</span><span class="sxs-lookup"><span data-stu-id="7680a-344">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="7680a-345">`CourseAssignment` 並不需要其專屬的 PK。</span><span class="sxs-lookup"><span data-stu-id="7680a-345">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="7680a-346">`InstructorID` 及 `CourseID` 屬性的功能便是複合 PK。</span><span class="sxs-lookup"><span data-stu-id="7680a-346">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="7680a-347">為 EF Core 指定複合 PK 的唯一方法是使用 *Fluent API*。</span><span class="sxs-lookup"><span data-stu-id="7680a-347">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="7680a-348">下節會說明如何設定複合 PK。</span><span class="sxs-lookup"><span data-stu-id="7680a-348">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="7680a-349">複合 PK 可確保：</span><span class="sxs-lookup"><span data-stu-id="7680a-349">The composite key ensures:</span></span>

* <span data-ttu-id="7680a-350">一個課程可以有多個資料列。</span><span class="sxs-lookup"><span data-stu-id="7680a-350">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="7680a-351">一個講師可以有多個資料列。</span><span class="sxs-lookup"><span data-stu-id="7680a-351">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="7680a-352">相同講師和課程不可有多個資料列。</span><span class="sxs-lookup"><span data-stu-id="7680a-352">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="7680a-353">由於 `Enrollment` 聯結實體定義了其自身的 PK，因此這種種類的重複項目是可能的。</span><span class="sxs-lookup"><span data-stu-id="7680a-353">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="7680a-354">若要防止這類重複項目：</span><span class="sxs-lookup"><span data-stu-id="7680a-354">To prevent such duplicates:</span></span>

* <span data-ttu-id="7680a-355">在 FK 欄位中新增一個唯一的索引，或</span><span class="sxs-lookup"><span data-stu-id="7680a-355">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="7680a-356">為 `Enrollment` 設定一個主複合索引鍵，與 `CourseAssignment` 相似。</span><span class="sxs-lookup"><span data-stu-id="7680a-356">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="7680a-357">如需詳細資訊，請參閱[索引](/ef/core/modeling/indexes)。</span><span class="sxs-lookup"><span data-stu-id="7680a-357">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="7680a-358">更新資料庫內容</span><span class="sxs-lookup"><span data-stu-id="7680a-358">Update the DB context</span></span>

<span data-ttu-id="7680a-359">將下列醒目提示的程式碼新增至 *Data/SchoolContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="7680a-359">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="7680a-360">上述程式碼會新增一個新實體，並設定 `CourseAssignment` 實體的複合 PK。</span><span class="sxs-lookup"><span data-stu-id="7680a-360">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="7680a-361">屬性的 Fluent API 替代項目</span><span class="sxs-lookup"><span data-stu-id="7680a-361">Fluent API alternative to attributes</span></span>

<span data-ttu-id="7680a-362">上述程式碼中的 `OnModelCreating` 方法使用了 *Fluent API* 設定 EF Core 行為。</span><span class="sxs-lookup"><span data-stu-id="7680a-362">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="7680a-363">此 API 稱為 "fluent" ，因為其常常會用於將一系列的方法呼叫串在一起，使其成為一個單一陳述式。</span><span class="sxs-lookup"><span data-stu-id="7680a-363">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="7680a-364">[下列程式碼](/ef/core/modeling/#methods-of-configuration)為 Fluent API 的其中一個範例：</span><span class="sxs-lookup"><span data-stu-id="7680a-364">The [following code](/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="7680a-365">在此教學課程中，Fluent API 僅會用於無法使用屬性完成的資料庫對應。</span><span class="sxs-lookup"><span data-stu-id="7680a-365">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="7680a-366">然而，Fluent API 可指定大部分透過屬性可完成的格式、驗證及對應規則。</span><span class="sxs-lookup"><span data-stu-id="7680a-366">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="7680a-367">某些屬性 (例如 `MinimumLength`) 無法使用 Fluent API 來套用。</span><span class="sxs-lookup"><span data-stu-id="7680a-367">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="7680a-368">`MinimumLength` 不會變更結構描述。它只會套用一項最小長度驗證規則。</span><span class="sxs-lookup"><span data-stu-id="7680a-368">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="7680a-369">某些開發人員偏好單獨使用 Fluent API，使其實體類別保持「整潔」。</span><span class="sxs-lookup"><span data-stu-id="7680a-369">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="7680a-370">屬性和 Fluent API 可混合使用。</span><span class="sxs-lookup"><span data-stu-id="7680a-370">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="7680a-371">有一些設定只能透過 Fluent API 完成 (指定複合 PK)。</span><span class="sxs-lookup"><span data-stu-id="7680a-371">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="7680a-372">有一些設定只能透過屬性完成 (`MinimumLength`)。</span><span class="sxs-lookup"><span data-stu-id="7680a-372">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="7680a-373">使用 Fluent API 或屬性的建議做法為：</span><span class="sxs-lookup"><span data-stu-id="7680a-373">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="7680a-374">從這兩種方法中選擇一項。</span><span class="sxs-lookup"><span data-stu-id="7680a-374">Choose one of these two approaches.</span></span>
* <span data-ttu-id="7680a-375">持續且盡量使用您選擇的方法。</span><span class="sxs-lookup"><span data-stu-id="7680a-375">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="7680a-376">此教學課程中使用到的某些屬性主要用於：</span><span class="sxs-lookup"><span data-stu-id="7680a-376">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="7680a-377">僅驗證 (例如，`MinimumLength`)。</span><span class="sxs-lookup"><span data-stu-id="7680a-377">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="7680a-378">僅 EF Core 組態 (例如，`HasKey`)。</span><span class="sxs-lookup"><span data-stu-id="7680a-378">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="7680a-379">驗證及 EF Core 組態 (例如，`[StringLength(50)]`)。</span><span class="sxs-lookup"><span data-stu-id="7680a-379">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="7680a-380">如需屬性與 Fluent API 的詳細資訊，請參閱[方法](/ef/core/modeling/#methods-of-configuration)。</span><span class="sxs-lookup"><span data-stu-id="7680a-380">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="7680a-381">顯示關聯性的實體圖表</span><span class="sxs-lookup"><span data-stu-id="7680a-381">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="7680a-382">下圖顯示了 EF Power Tools 為完成的 School 模型建立的圖表。</span><span class="sxs-lookup"><span data-stu-id="7680a-382">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![實體圖表](complex-data-model/_static/diagram.png)

<span data-ttu-id="7680a-384">上述圖表顯示：</span><span class="sxs-lookup"><span data-stu-id="7680a-384">The preceding diagram shows:</span></span>

* <span data-ttu-id="7680a-385">數個一對多關聯性線條 (1 對 \*)。</span><span class="sxs-lookup"><span data-stu-id="7680a-385">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="7680a-386">`Instructor` 和 `OfficeAssignment` 實體之間的一對零或一關聯性線條 (1 對 0..1)。</span><span class="sxs-lookup"><span data-stu-id="7680a-386">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="7680a-387">`Instructor` 和 `Department` 實體之間的零或一對多關聯性線條 (0..1 對 \*)。</span><span class="sxs-lookup"><span data-stu-id="7680a-387">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="7680a-388">使用測試資料植入資料庫</span><span class="sxs-lookup"><span data-stu-id="7680a-388">Seed the DB with Test Data</span></span>

<span data-ttu-id="7680a-389">更新 *Data/DbInitializer.cs* 中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="7680a-389">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="7680a-390">上述程式碼為新的實體提供了種子資料。</span><span class="sxs-lookup"><span data-stu-id="7680a-390">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="7680a-391">此程式碼中的大部分主要用於建立新的實體物件並載入範例資料。</span><span class="sxs-lookup"><span data-stu-id="7680a-391">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="7680a-392">範例資料主要用於測試。</span><span class="sxs-lookup"><span data-stu-id="7680a-392">The sample data is used for testing.</span></span> <span data-ttu-id="7680a-393">如需如何植入多對多聯結資料表的範例，請參閱 `Enrollments` 和 `CourseAssignments`。</span><span class="sxs-lookup"><span data-stu-id="7680a-393">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="7680a-394">新增移轉</span><span class="sxs-lookup"><span data-stu-id="7680a-394">Add a migration</span></span>

<span data-ttu-id="7680a-395">建置專案。</span><span class="sxs-lookup"><span data-stu-id="7680a-395">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7680a-396">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7680a-396">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7680a-397">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7680a-397">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add ComplexDataModel
```

------

<span data-ttu-id="7680a-398">上述命令會顯示關於可能發生資料遺失的警告。</span><span class="sxs-lookup"><span data-stu-id="7680a-398">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="7680a-399">若執行 `database update` 命令，便會產生下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="7680a-399">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="7680a-400">套用移轉</span><span class="sxs-lookup"><span data-stu-id="7680a-400">Apply the migration</span></span>

<span data-ttu-id="7680a-401">現在您有了現有的資料庫，您需要思考如何對其套用未來變更。</span><span class="sxs-lookup"><span data-stu-id="7680a-401">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="7680a-402">本教學課程示範兩種方法：</span><span class="sxs-lookup"><span data-stu-id="7680a-402">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="7680a-403">卸除並重新建立資料庫</span><span class="sxs-lookup"><span data-stu-id="7680a-403">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="7680a-404">[將移轉套用至現有資料庫](#applyexisting)。</span><span class="sxs-lookup"><span data-stu-id="7680a-404">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="7680a-405">雖然這個方法更複雜且耗時，卻是實際生產環境的慣用方法。</span><span class="sxs-lookup"><span data-stu-id="7680a-405">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="7680a-406">**注意**：這是本教學課程的選擇性章節。</span><span class="sxs-lookup"><span data-stu-id="7680a-406">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="7680a-407">您可以執行卸除並重新建立步驟，然後略過本節。</span><span class="sxs-lookup"><span data-stu-id="7680a-407">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="7680a-408">如果您希望遵循本章節中的步驟，請不要執行卸除並重新建立的步驟。</span><span class="sxs-lookup"><span data-stu-id="7680a-408">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="7680a-409">卸除並重新建立資料庫</span><span class="sxs-lookup"><span data-stu-id="7680a-409">Drop and re-create the database</span></span>

<span data-ttu-id="7680a-410">更新的 `DbInitializer` 中的程式碼會為新的實體新增種子資料。</span><span class="sxs-lookup"><span data-stu-id="7680a-410">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="7680a-411">若要強制 EF Core 建立新的資料庫，請卸除並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="7680a-411">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7680a-412">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7680a-412">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7680a-413">在 [套件管理員主控台] (PMC) 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="7680a-413">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="7680a-414">從 PMC 執行 `Get-Help about_EntityFrameworkCore` 以取得說明資訊。</span><span class="sxs-lookup"><span data-stu-id="7680a-414">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7680a-415">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7680a-415">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7680a-416">開啟命令視窗並巡覽至專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="7680a-416">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="7680a-417">專案資料夾中包含 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7680a-417">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="7680a-418">在命令視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="7680a-418">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

------

<span data-ttu-id="7680a-419">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7680a-419">Run the app.</span></span> <span data-ttu-id="7680a-420">執行應用程式會執行 `DbInitializer.Initialize` 方法。</span><span class="sxs-lookup"><span data-stu-id="7680a-420">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="7680a-421">`DbInitializer.Initialize` 會填入新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="7680a-421">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="7680a-422">在 SSOX 中開啟資料庫：</span><span class="sxs-lookup"><span data-stu-id="7680a-422">Open the DB in SSOX:</span></span>

* <span data-ttu-id="7680a-423">若先前已開啟過 SSOX，按一下 [重新整理] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7680a-423">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="7680a-424">展開 **Tables** 節點。</span><span class="sxs-lookup"><span data-stu-id="7680a-424">Expand the **Tables** node.</span></span> <span data-ttu-id="7680a-425">建立的資料表便會顯示。</span><span class="sxs-lookup"><span data-stu-id="7680a-425">The created tables are displayed.</span></span>

![SSOX 中的資料表](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="7680a-427">檢查 **CourseAssignment** 資料表：</span><span class="sxs-lookup"><span data-stu-id="7680a-427">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="7680a-428">以滑鼠右鍵按一下 **CourseAssignment** 資料表，然後選取 [檢視資料]。</span><span class="sxs-lookup"><span data-stu-id="7680a-428">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="7680a-429">驗證 **CourseAssignment** 資料表中是否包含資料。</span><span class="sxs-lookup"><span data-stu-id="7680a-429">Verify the **CourseAssignment** table contains data.</span></span>

![SSOX 中的 CourseAssignment 資料](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="7680a-431">將移轉套用至現有資料庫</span><span class="sxs-lookup"><span data-stu-id="7680a-431">Apply the migration to the existing database</span></span>

<span data-ttu-id="7680a-432">本節為選擇性。</span><span class="sxs-lookup"><span data-stu-id="7680a-432">This section is optional.</span></span> <span data-ttu-id="7680a-433">只有當您略過先前[卸除並重新建立資料庫](#drop)一節，這些步驟才有效。</span><span class="sxs-lookup"><span data-stu-id="7680a-433">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="7680a-434">當使用現有的資料執行移轉作業時，某些 FK 條件約束可能會無法透過現有資料滿足。</span><span class="sxs-lookup"><span data-stu-id="7680a-434">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="7680a-435">當您使用的是生產資料時，您必須進行幾個步驟才能移轉現有資料。</span><span class="sxs-lookup"><span data-stu-id="7680a-435">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="7680a-436">本節提供了修正 FK 條件約束違規的範例。</span><span class="sxs-lookup"><span data-stu-id="7680a-436">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="7680a-437">請不要在沒有備份的情況下進行這些程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="7680a-437">Don't make these code changes without a backup.</span></span> <span data-ttu-id="7680a-438">若您已完成了先前的章節並已更新資料庫，請不要進行這些程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="7680a-438">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="7680a-439">*{timestamp}_ComplexDataModel.cs* 檔案包含了下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="7680a-439">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="7680a-440">上述程式碼將一個不可為 Null 的 `DepartmentID` FK 新增至 `Course` 資料表。</span><span class="sxs-lookup"><span data-stu-id="7680a-440">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="7680a-441">先前教學課程中的資料庫在 `Course` 中包含了資料列，導致資料表無法藉由移轉進行更新。</span><span class="sxs-lookup"><span data-stu-id="7680a-441">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="7680a-442">若要讓 `ComplexDataModel` 與現有資料進行移轉：</span><span class="sxs-lookup"><span data-stu-id="7680a-442">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="7680a-443">變更程式碼，以給予新資料行 (`DepartmentID`) 一個新的預設值。</span><span class="sxs-lookup"><span data-stu-id="7680a-443">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="7680a-444">建立一個名為 "Temp" 的假部門以作為預設部門之用。</span><span class="sxs-lookup"><span data-stu-id="7680a-444">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="7680a-445">修正外部索引鍵條件約束</span><span class="sxs-lookup"><span data-stu-id="7680a-445">Fix the foreign key constraints</span></span>

<span data-ttu-id="7680a-446">更新 `ComplexDataModel` 類別 `Up` 方法：</span><span class="sxs-lookup"><span data-stu-id="7680a-446">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="7680a-447">開啟 *{timestamp}_ComplexDataModel.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7680a-447">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="7680a-448">將新增 `DepartmentID` 資料行至 `Course` 資料表的程式碼全部標為註解。</span><span class="sxs-lookup"><span data-stu-id="7680a-448">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="7680a-449">新增下列醒目提示程式碼。</span><span class="sxs-lookup"><span data-stu-id="7680a-449">Add the following highlighted code.</span></span> <span data-ttu-id="7680a-450">新的程式碼應位於 `.CreateTable( name: "Department"` 區塊之後：[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="7680a-450">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="7680a-451">藉由上述的變更，現有的 `Course` 資料列便會在執行 `ComplexDataModel``Up` 方法後與 "Temp" 部門產生關聯。</span><span class="sxs-lookup"><span data-stu-id="7680a-451">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="7680a-452">生產環境的應用程式會：</span><span class="sxs-lookup"><span data-stu-id="7680a-452">A production app would:</span></span>

* <span data-ttu-id="7680a-453">包含將 `Department` 資料列及相關 `Course` 資料列新增到新 `Department` 資料列的程式碼或指令碼。</span><span class="sxs-lookup"><span data-stu-id="7680a-453">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="7680a-454">不使用 "Temp" 部門或 `Course.DepartmentID` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="7680a-454">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="7680a-455">下一個教學課程會涵蓋相關資料。</span><span class="sxs-lookup"><span data-stu-id="7680a-455">The next tutorial covers related data.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="7680a-456">[上一頁](xref:data/ef-rp/migrations)
> [下一頁](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="7680a-456">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>
