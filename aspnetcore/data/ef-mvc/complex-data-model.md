---
title: "使用 EF Core-資料模型-10-5 的 ASP.NET Core MVC"
author: tdykstra
description: "本教學課程中您要新增更多的實體和關聯性，並指定格式、 驗證和資料庫對應規則，以自訂資料模型。"
keywords: "ASP.NET Core，Entity Framework Core，資料註解"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 0dd63913-a041-48b6-96a4-3aeaedbdf5d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: dde50f766dc9842089cbb0561b8bd6e2d8e7c34f
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a><span data-ttu-id="37644-104">建立複雜的資料模型的 EF Core 與 ASP.NET Core MVC 教學課程 (10-5)</span><span class="sxs-lookup"><span data-stu-id="37644-104">Creating a complex data model - EF Core with ASP.NET Core MVC tutorial (5 of 10)</span></span>

<span data-ttu-id="37644-105">由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="37644-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="37644-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="37644-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="37644-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="37644-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="37644-108">在上一個教學課程中，您使用過的簡單資料模型的三個實體所組成。</span><span class="sxs-lookup"><span data-stu-id="37644-108">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="37644-109">在此教學課程中，您將新增多個實體和關聯性，您將自訂資料模型，藉由指定的格式、 驗證和資料庫對應規則。</span><span class="sxs-lookup"><span data-stu-id="37644-109">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="37644-110">當您完成時，實體類別會構成完成的資料模型，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="37644-110">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![實體圖表](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="37644-112">自訂資料模型使用屬性</span><span class="sxs-lookup"><span data-stu-id="37644-112">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="37644-113">本節中，您會看到如何自訂資料模型使用指定的格式、 驗證和資料庫對應規則的屬性。</span><span class="sxs-lookup"><span data-stu-id="37644-113">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="37644-114">然後在您將建立下列各節的幾個完整學校資料模型，藉由新增屬性類別您已經建立，並在模型中建立新類別的其餘的實體類型。</span><span class="sxs-lookup"><span data-stu-id="37644-114">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="37644-115">資料類型屬性</span><span class="sxs-lookup"><span data-stu-id="37644-115">The DataType attribute</span></span>

<span data-ttu-id="37644-116">學生註冊日期，所有的網頁目前顯示的時間和日期，不過關心這個欄位是日期。</span><span class="sxs-lookup"><span data-stu-id="37644-116">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="37644-117">使用資料註解屬性，您可以進行一將修復的顯示格式來顯示資料的每個檢視中變更程式碼。</span><span class="sxs-lookup"><span data-stu-id="37644-117">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="37644-118">若要查看如何執行作業，您會加入至屬性的範例`EnrollmentDate`屬性`Student`類別。</span><span class="sxs-lookup"><span data-stu-id="37644-118">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="37644-119">在*Models/Student.cs*，新增`using`陳述式`System.ComponentModel.DataAnnotations`命名空間和新增`DataType`和`DisplayFormat`屬性至`EnrollmentDate`屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="37644-119">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="37644-120">`DataType` 屬性用來指定比資料庫內建類型更特殊的資料類型。</span><span class="sxs-lookup"><span data-stu-id="37644-120">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="37644-121">在此情況下我們只想要追蹤的日期，不的日期和時間。</span><span class="sxs-lookup"><span data-stu-id="37644-121">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="37644-122">`DataType`列舉型別提供許多的資料類型，例如日期、 時間、 PhoneNumber、 貨幣、 EmailAddress，等等。</span><span class="sxs-lookup"><span data-stu-id="37644-122">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="37644-123">`DataType` 屬性也可讓應用程式自動提供類型的特定功能。</span><span class="sxs-lookup"><span data-stu-id="37644-123">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="37644-124">例如，可建立 `DataType.EmailAddress` 的 `mailto:` 連結，而且可以在支援 HTML5 的瀏覽器中提供 `DataType.Date` 的日期選擇器。</span><span class="sxs-lookup"><span data-stu-id="37644-124">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="37644-125">`DataType`屬性發出 HTML 5 `data-` HTML 5 瀏覽器可以了解的 (唸成的資料 dash) 屬性。</span><span class="sxs-lookup"><span data-stu-id="37644-125">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="37644-126">`DataType`屬性不提供任何驗證。</span><span class="sxs-lookup"><span data-stu-id="37644-126">The `DataType` attributes do not provide any validation.</span></span>

<span data-ttu-id="37644-127">`DataType.Date` 未指定顯示日期的格式。</span><span class="sxs-lookup"><span data-stu-id="37644-127">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="37644-128">根據預設，資料欄位會顯示根據伺服器的 CultureInfo 為基礎的預設格式。</span><span class="sxs-lookup"><span data-stu-id="37644-128">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="37644-129">`DisplayFormat` 屬性用來明確指定日期格式：</span><span class="sxs-lookup"><span data-stu-id="37644-129">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="37644-130">`ApplyFormatInEditMode` 設定可指定在文字方塊中顯示值以供編輯時，也應該套用的格式。</span><span class="sxs-lookup"><span data-stu-id="37644-130">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="37644-131">（您可能不想，某些欄位-例如，貨幣值，您可能不想在文字方塊中的貨幣符號進行編輯）。</span><span class="sxs-lookup"><span data-stu-id="37644-131">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="37644-132">您可以使用`DisplayFormat`屬性本身，但它通常是最好使用`DataType`也屬性。</span><span class="sxs-lookup"><span data-stu-id="37644-132">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="37644-133">`DataType`屬性的語意，而不是如何呈現在螢幕上的資料並提供下列優點，就無法取得與`DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="37644-133">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="37644-134">瀏覽器可以啟用 HTML5 功能 （例如若要顯示日曆控制項、 地區設定適當的貨幣符號、 電子郵件連結，某些用戶端輸入驗證等。）。</span><span class="sxs-lookup"><span data-stu-id="37644-134">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="37644-135">根據預設，瀏覽器將根據您的地區設定，使用正確的格式呈現資料。</span><span class="sxs-lookup"><span data-stu-id="37644-135">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="37644-136">如需詳細資訊，請參閱[\<輸入 > 標記協助程式文件](../../mvc/views/working-with-forms.md#the-input-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="37644-136">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="37644-137">執行應用程式，請學生索引頁，請注意，時間不會再顯示註冊日期。</span><span class="sxs-lookup"><span data-stu-id="37644-137">Run the app, go to the Students Index page and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="37644-138">相同將學生模型會使用任何檢視，則為 true。</span><span class="sxs-lookup"><span data-stu-id="37644-138">The same will be true for any view that uses the Student model.</span></span>

![顯示不含時間日期的學生索引頁面](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="37644-140">StringLength 屬性</span><span class="sxs-lookup"><span data-stu-id="37644-140">The StringLength attribute</span></span>

<span data-ttu-id="37644-141">您也可以指定資料的驗證規則和使用屬性的驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="37644-141">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="37644-142">`StringLength`屬性設定資料庫中的最大長度，並提供用戶端和伺服器端的 ASP.NET MVC 驗證。</span><span class="sxs-lookup"><span data-stu-id="37644-142">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="37644-143">您也可以指定最小字串長度，此屬性，但資料庫結構描述的最小值沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="37644-143">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="37644-144">假設您想要確保使用者不要輸入超過 50 個字元的名稱。</span><span class="sxs-lookup"><span data-stu-id="37644-144">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="37644-145">若要加入這項限制，加入`StringLength`屬性至`LastName`和`FirstMidName`屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="37644-145">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="37644-146">`StringLength`屬性將不會防止使用者輸入名稱的泛空白字元。</span><span class="sxs-lookup"><span data-stu-id="37644-146">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="37644-147">您可以使用`RegularExpression`將限制套用至輸入的屬性。</span><span class="sxs-lookup"><span data-stu-id="37644-147">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="37644-148">例如，下列程式碼需要第一個字元是大寫，而其餘字元是依字母順序排列：</span><span class="sxs-lookup"><span data-stu-id="37644-148">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

<span data-ttu-id="37644-149">`MaxLength`屬性會提供類似的功能`StringLength`屬性，但不會提供用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="37644-149">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="37644-150">資料庫模型現在已變更，則需要在資料庫結構描述變更的方式。</span><span class="sxs-lookup"><span data-stu-id="37644-150">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="37644-151">您將使用移轉，而不會遺失任何資料，您可能使用應用程式 UI 加入至資料庫中更新結構描述。</span><span class="sxs-lookup"><span data-stu-id="37644-151">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="37644-152">儲存您的變更，並建置專案。</span><span class="sxs-lookup"><span data-stu-id="37644-152">Save your changes and build the project.</span></span> <span data-ttu-id="37644-153">然後在專案資料夾中開啟 [命令] 視窗並輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="37644-153">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="37644-154">`migrations add`命令會發出警告，可能會發生資料遺失，因為變更會讓較短的兩個資料行的最大長度。</span><span class="sxs-lookup"><span data-stu-id="37644-154">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="37644-155">移轉會建立名為*\<時間戳記 > _MaxLengthOnNames.cs*。</span><span class="sxs-lookup"><span data-stu-id="37644-155">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="37644-156">這個檔案包含程式碼中的`Up`方法將會更新資料庫，以符合目前的資料模型。</span><span class="sxs-lookup"><span data-stu-id="37644-156">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="37644-157">`database update`命令執行該程式碼。</span><span class="sxs-lookup"><span data-stu-id="37644-157">The `database update` command ran that code.</span></span>

<span data-ttu-id="37644-158">移轉的檔案名稱的前置詞的時間戳記 Entity framework 用於排序的移轉。</span><span class="sxs-lookup"><span data-stu-id="37644-158">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="37644-159">您可以建立多個移轉之前執行更新資料庫的命令，然後所有移轉都套用中所建立的順序。</span><span class="sxs-lookup"><span data-stu-id="37644-159">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="37644-160">執行應用程式中，選取**學生**索引標籤上，按一下 **新建**，並輸入任一名稱長度超過 50 個字元。</span><span class="sxs-lookup"><span data-stu-id="37644-160">Run the app, select the **Students** tab, click **Create New**, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="37644-161">當您按一下**建立**，用戶端驗證會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="37644-161">When you click **Create**, client side validation shows an error message.</span></span>

![學生索引頁面顯示字串長度錯誤](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="37644-163">資料行屬性</span><span class="sxs-lookup"><span data-stu-id="37644-163">The Column attribute</span></span>

<span data-ttu-id="37644-164">您也可以使用屬性來控制您的類別和屬性如何對應到資料庫。</span><span class="sxs-lookup"><span data-stu-id="37644-164">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="37644-165">假設您使用名稱`FirstMidName`，第一個名稱欄位，因為欄位也可能包含中間名。</span><span class="sxs-lookup"><span data-stu-id="37644-165">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="37644-166">但您想要命名的資料庫欄`FirstName`，因為使用者將會寫入資料庫的臨機操作查詢習慣於該名稱。</span><span class="sxs-lookup"><span data-stu-id="37644-166">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="37644-167">若要讓此對應，您可以使用`Column`屬性。</span><span class="sxs-lookup"><span data-stu-id="37644-167">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="37644-168">`Column`屬性指定當建立資料庫時，資料行`Student`對應到資料表`FirstMidName`屬性會被命名為`FirstName`。</span><span class="sxs-lookup"><span data-stu-id="37644-168">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="37644-169">換句話說，當您的程式碼是指`Student.FirstMidName`，資料將來自，或在更新`FirstName`資料行`Student`資料表。</span><span class="sxs-lookup"><span data-stu-id="37644-169">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="37644-170">如果您未指定資料行名稱，會收到相同的名稱與屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="37644-170">If you don't specify column names, they are given the same name as the property name.</span></span>

<span data-ttu-id="37644-171">在*Student.cs* file、 add`using`陳述式`System.ComponentModel.DataAnnotations.Schema`並加入至資料行名稱屬性`FirstMidName`屬性，如下列反白顯示的程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="37644-171">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="37644-172">新增`Column`屬性變更模型支援`SchoolContext`，因此它將不會符合資料庫。</span><span class="sxs-lookup"><span data-stu-id="37644-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="37644-173">儲存您的變更，並建置專案。</span><span class="sxs-lookup"><span data-stu-id="37644-173">Save your changes and build the project.</span></span> <span data-ttu-id="37644-174">然後在專案資料夾中開啟 [命令] 視窗並輸入下列命令來建立其他移轉：</span><span class="sxs-lookup"><span data-stu-id="37644-174">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="37644-175">在**SQL Server 物件總管**，按兩下開啟學生資料表設計工具**學生**資料表。</span><span class="sxs-lookup"><span data-stu-id="37644-175">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![在移轉之後，在 SSOX students 資料表](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="37644-177">套用的前兩個移轉作業之前，名稱資料行的類型 nvarchar （max）。</span><span class="sxs-lookup"><span data-stu-id="37644-177">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="37644-178">它們現在是 nvarchar （50） 和資料行名稱已經從 FirstMidName FirstName。</span><span class="sxs-lookup"><span data-stu-id="37644-178">They are now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="37644-179">如果您嘗試編譯，才能完成下列各節中建立的所有實體類別，您可能會收到編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="37644-179">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="37644-180">最後的學生實體的變更</span><span class="sxs-lookup"><span data-stu-id="37644-180">Final changes to the Student entity</span></span>

![學生實體](complex-data-model/_static/student-entity.png)

<span data-ttu-id="37644-182">在*Models/Student.cs*，稍早為下列程式碼取代您所加入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="37644-182">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="37644-183">所做的變更會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="37644-183">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="37644-184">必要的屬性</span><span class="sxs-lookup"><span data-stu-id="37644-184">The Required attribute</span></span>

<span data-ttu-id="37644-185">`Required`屬性會讓名稱屬性的必要的欄位。</span><span class="sxs-lookup"><span data-stu-id="37644-185">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="37644-186">`Required`屬性不需要不可為 null 的類型，例如實值類型 (DateTime、 int、 double、 float、 等等。)。</span><span class="sxs-lookup"><span data-stu-id="37644-186">The `Required` attribute is not needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="37644-187">不可為 null 的型別會自動視為必要欄位。</span><span class="sxs-lookup"><span data-stu-id="37644-187">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="37644-188">您也可以移除`Required`屬性並將它取代的最小長度參數`StringLength`屬性：</span><span class="sxs-lookup"><span data-stu-id="37644-188">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="37644-189">顯示屬性</span><span class="sxs-lookup"><span data-stu-id="37644-189">The Display attribute</span></span>

<span data-ttu-id="37644-190">`Display`屬性指定的文字方塊中的標題應該 「 名字 」、 「 姓氏 」、 「 完整名稱 」 和 「 註冊日期 」，而不是屬性名稱在每個執行個體 （其除以字沒有空格）。</span><span class="sxs-lookup"><span data-stu-id="37644-190">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="37644-191">計算的 FullName 屬性</span><span class="sxs-lookup"><span data-stu-id="37644-191">The FullName calculated property</span></span>

<span data-ttu-id="37644-192">`FullName`是傳回值，由串連兩個其他屬性的導出的屬性。</span><span class="sxs-lookup"><span data-stu-id="37644-192">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="37644-193">因此它具有只有 get 存取子，但沒有`FullName`會在資料庫中產生資料行。</span><span class="sxs-lookup"><span data-stu-id="37644-193">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="37644-194">建立 [Instructor] 實體</span><span class="sxs-lookup"><span data-stu-id="37644-194">Create the Instructor Entity</span></span>

![Instructor 實體](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="37644-196">建立*Models/Instructor.cs*，範本程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="37644-196">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="37644-197">請注意一些屬性，在 「 學生 」 和 「 講師實體相同。</span><span class="sxs-lookup"><span data-stu-id="37644-197">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="37644-198">在[實作繼承](inheritance.md)稍後在本系列的教學課程，您將會重構此程式碼以消除重複。</span><span class="sxs-lookup"><span data-stu-id="37644-198">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="37644-199">您也可以將多個屬性放在同一行，您也可以撰寫`HireDate`屬性，如下所示：</span><span class="sxs-lookup"><span data-stu-id="37644-199">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="37644-200">CourseAssignments 和 OfficeAssignment 的導覽屬性</span><span class="sxs-lookup"><span data-stu-id="37644-200">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="37644-201">`CourseAssignments`和`OfficeAssignment`屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="37644-201">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="37644-202">講師可以教導任意數目的課程，因此`CourseAssignments`定義為集合。</span><span class="sxs-lookup"><span data-stu-id="37644-202">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="37644-203">如果導覽屬性都可以保存多個實體，其型別必須是的清單中的項目可加入、 刪除及更新。</span><span class="sxs-lookup"><span data-stu-id="37644-203">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="37644-204">您可以指定`ICollection<T>`或型別，例如`List<T>`或`HashSet<T>`。</span><span class="sxs-lookup"><span data-stu-id="37644-204">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="37644-205">如果您指定`ICollection<T>`，EF 建立`HashSet<T>`預設集合。</span><span class="sxs-lookup"><span data-stu-id="37644-205">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="37644-206">這些是為什麼原因`CourseAssignment`實體是下面將說明有關多對多關聯性 > 一節。</span><span class="sxs-lookup"><span data-stu-id="37644-206">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="37644-207">Contoso 大學商務規則狀態講師只能有最多一個辦事處的資料，所以`OfficeAssignment`屬性會保留單一 OfficeAssignment 實體 （這可能是 null，如果沒有 office 指派）。</span><span class="sxs-lookup"><span data-stu-id="37644-207">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="37644-208">建立 OfficeAssignment 實體</span><span class="sxs-lookup"><span data-stu-id="37644-208">Create the OfficeAssignment entity</span></span>

![OfficeAssignment 實體](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="37644-210">建立*Models/OfficeAssignment.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="37644-210">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="37644-211">索引鍵屬性</span><span class="sxs-lookup"><span data-stu-id="37644-211">The Key attribute</span></span>

<span data-ttu-id="37644-212">沒有講師 OfficeAssignment 實體之間以 0 或-1 一個關聯性。</span><span class="sxs-lookup"><span data-stu-id="37644-212">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="37644-213">Office 指派相對於講師指派，才會存在，因此其主索引鍵也是其外部索引鍵 [Instructor] 實體。</span><span class="sxs-lookup"><span data-stu-id="37644-213">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="37644-214">但是，Entity Framework 無法自動辨識 InstructorID 為此實體的主索引鍵因為其名稱未遵循識別碼或 classnameID 命名慣例。</span><span class="sxs-lookup"><span data-stu-id="37644-214">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="37644-215">因此，`Key`屬性用來識別索引鍵：</span><span class="sxs-lookup"><span data-stu-id="37644-215">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="37644-216">您也可以使用`Key`屬性如果實體沒有自己的主索引鍵，但您想要的項目名稱屬性，非 classnameID 或識別碼。</span><span class="sxs-lookup"><span data-stu-id="37644-216">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="37644-217">根據預設，EF 視為金鑰非產生資料庫因為資料行是識別的關聯性。</span><span class="sxs-lookup"><span data-stu-id="37644-217">By default, EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="37644-218">講師導覽屬性</span><span class="sxs-lookup"><span data-stu-id="37644-218">The Instructor navigation property</span></span>

<span data-ttu-id="37644-219">[Instructor] 實體有允許 null`OfficeAssignment`導覽屬性 （因為講師可能沒有 office 指派），而且 OfficeAssignment 實體具有不可為 null`Instructor`導覽屬性 （因為無法 office 指派沒有講師-存在`InstructorID`不可為 null)。</span><span class="sxs-lookup"><span data-stu-id="37644-219">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="37644-220">當 [Instructor] 實體具有相關的 OfficeAssignment 實體時，每個實體會有另一個在其導覽屬性的參考。</span><span class="sxs-lookup"><span data-stu-id="37644-220">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="37644-221">您無法將`[Required]`講師導覽屬性，指定必須有相關的講師，但您不需要這樣做，因為屬性`InstructorID`（這也是這個資料表的索引鍵） 的外部索引鍵是不可為 null。</span><span class="sxs-lookup"><span data-stu-id="37644-221">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="37644-222">修改課程實體</span><span class="sxs-lookup"><span data-stu-id="37644-222">Modify the Course Entity</span></span>

![課程實體](complex-data-model/_static/course-entity.png)

<span data-ttu-id="37644-224">在*Models/Course.cs*，稍早為下列程式碼取代您所加入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="37644-224">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="37644-225">所做的變更會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="37644-225">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="37644-226">課程實體具有外部索引鍵屬性`DepartmentID`哪些點及其相關的 Department 實體具有`Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="37644-226">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="37644-227">Entity Framework 不需要具有相關實體的導覽屬性時，將外部索引鍵屬性加入至您的資料模型。</span><span class="sxs-lookup"><span data-stu-id="37644-227">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="37644-228">EF 自動在資料庫中建立外部索引鍵，在需要時會和建立[遮蔽屬性](https://docs.microsoft.com/ef/core/modeling/shadow-properties)它們。</span><span class="sxs-lookup"><span data-stu-id="37644-228">EF automatically creates foreign keys in the database wherever they are needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="37644-229">但是，具有資料模型中的外部索引鍵可進行更新，更簡單且更有效率。</span><span class="sxs-lookup"><span data-stu-id="37644-229">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="37644-230">例如，當您擷取一個課程實體，來編輯、 Department 實體為 null 如果您不載入它，因此當您更新課程實體中，您必須先擷取 Department 實體。</span><span class="sxs-lookup"><span data-stu-id="37644-230">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="37644-231">當外部索引鍵屬性`DepartmentID`是否包含在資料模型，您不需要在更新之前擷取 Department 實體。</span><span class="sxs-lookup"><span data-stu-id="37644-231">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="37644-232">DatabaseGenerated 屬性</span><span class="sxs-lookup"><span data-stu-id="37644-232">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="37644-233">`DatabaseGenerated`屬性附帶`None`參數`CourseID`屬性指定主索引鍵值是使用者所提供，而不是不是資料庫產生。</span><span class="sxs-lookup"><span data-stu-id="37644-233">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="37644-234">根據預設，Entity Framework 會假設資料庫會產生主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="37644-234">By default, Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="37644-235">這是您想在大多數情況下。</span><span class="sxs-lookup"><span data-stu-id="37644-235">That's what you want in most scenarios.</span></span> <span data-ttu-id="37644-236">不過，課程實體，您會使用使用者指定課程數字，例如 1000年一連串一個部門，另一個部門，2000年數列等等。</span><span class="sxs-lookup"><span data-stu-id="37644-236">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="37644-237">`DatabaseGenerated`屬性也可用來產生預設值，如果是用來記錄日期的資料庫資料行建立或更新資料列。</span><span class="sxs-lookup"><span data-stu-id="37644-237">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="37644-238">如需詳細資訊，請參閱[產生屬性](https://docs.microsoft.com/ef/core/modeling/generated-properties)。</span><span class="sxs-lookup"><span data-stu-id="37644-238">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="37644-239">外部索引鍵和導覽屬性</span><span class="sxs-lookup"><span data-stu-id="37644-239">Foreign key and navigation properties</span></span>

<span data-ttu-id="37644-240">外部索引鍵屬性和課程實體中的導覽屬性會反映下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="37644-240">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="37644-241">課程指派給一個部門，所以`DepartmentID`外部索引鍵和`Department`導覽屬性，如上面所提的原因。</span><span class="sxs-lookup"><span data-stu-id="37644-241">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="37644-242">課程可以有任意數目的學生註冊，所以`Enrollments`導覽屬性是集合：</span><span class="sxs-lookup"><span data-stu-id="37644-242">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="37644-243">課程會教由多個講師，所以`CourseAssignments`導覽屬性是集合 (型別`CourseAssignment`說明[稍後](#many-to-many-relationships)):</span><span class="sxs-lookup"><span data-stu-id="37644-243">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="37644-244">建立 Department 實體</span><span class="sxs-lookup"><span data-stu-id="37644-244">Create the Department entity</span></span>

![Department 實體](complex-data-model/_static/department-entity.png)


<span data-ttu-id="37644-246">建立*Models/Department.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="37644-246">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="37644-247">資料行屬性</span><span class="sxs-lookup"><span data-stu-id="37644-247">The Column attribute</span></span>

<span data-ttu-id="37644-248">您所用的稍早`Column`屬性，以變更資料行名稱的對應。</span><span class="sxs-lookup"><span data-stu-id="37644-248">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="37644-249">在 Department 實體，程式碼`Column`屬性用來變更 SQL 資料類型對應，以便將使用資料庫中的 SQL Server money 類型會定義資料行：</span><span class="sxs-lookup"><span data-stu-id="37644-249">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="37644-250">資料行對應通常是不必要的因為 Entity Framework 選擇適當 SQL Server 資料類型，根據您定義屬性的 CLR 型別。</span><span class="sxs-lookup"><span data-stu-id="37644-250">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="37644-251">CLR`decimal`類型會對應至 SQL Server`decimal`型別。</span><span class="sxs-lookup"><span data-stu-id="37644-251">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="37644-252">但在此情況下您知道資料行保留貨幣金額，而 money 資料類型更適合的。</span><span class="sxs-lookup"><span data-stu-id="37644-252">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="37644-253">外部索引鍵和導覽屬性</span><span class="sxs-lookup"><span data-stu-id="37644-253">Foreign key and navigation properties</span></span>

<span data-ttu-id="37644-254">外部索引鍵和導覽屬性會反映的下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="37644-254">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="37644-255">部門可能會或可能不會為系統管理員，而且系統管理員永遠是講師。</span><span class="sxs-lookup"><span data-stu-id="37644-255">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="37644-256">因此`InstructorID`屬性是包含為 [Instructor] 實體的外部索引鍵，且後面會加上問號`int`輸入指定將標示為可為 null 的屬性。</span><span class="sxs-lookup"><span data-stu-id="37644-256">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="37644-257">導覽屬性名為`Administrator`但保留 [Instructor] 實體：</span><span class="sxs-lookup"><span data-stu-id="37644-257">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="37644-258">部門可能具有許多課程，課程導覽屬性是：</span><span class="sxs-lookup"><span data-stu-id="37644-258">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="37644-259">依照慣例，Entity Framework 可讓 cascade delete 不可為 null 的外部索引鍵和多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="37644-259">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="37644-260">這會導致循環的串聯刪除規則，這會造成例外狀況，當您嘗試新增的移轉。</span><span class="sxs-lookup"><span data-stu-id="37644-260">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="37644-261">例如，如果您未定義 Department.InstructorID 屬性為可為 null，EF 會設定 cascade delete 規則刪除講師，當您刪除部門，這不是您想要發生的事情。</span><span class="sxs-lookup"><span data-stu-id="37644-261">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which is not what you want to have happen.</span></span> <span data-ttu-id="37644-262">如果您的商務規則所需`InstructorID`屬性成為不可為 null，您必須使用下列的 fluent API 陳述式來停用串聯刪除關聯性上：</span><span class="sxs-lookup"><span data-stu-id="37644-262">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="37644-263">修改註冊實體</span><span class="sxs-lookup"><span data-stu-id="37644-263">Modify the Enrollment entity</span></span>

![註冊實體](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="37644-265">在*Models/Enrollment.cs*，稍早為下列程式碼取代您所加入的程式碼：</span><span class="sxs-lookup"><span data-stu-id="37644-265">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="37644-266">外部索引鍵和導覽屬性</span><span class="sxs-lookup"><span data-stu-id="37644-266">Foreign key and navigation properties</span></span>

<span data-ttu-id="37644-267">外部索引鍵屬性和導覽屬性會反映下列關聯性：</span><span class="sxs-lookup"><span data-stu-id="37644-267">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="37644-268">註冊記錄是單一的課程中，所以沒有`CourseID`外部索引鍵屬性和`Course`導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="37644-268">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="37644-269">註冊記錄是單一的學生，所以沒有`StudentID`外部索引鍵屬性和`Student`導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="37644-269">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="37644-270">多對多關聯性</span><span class="sxs-lookup"><span data-stu-id="37644-270">Many-to-Many Relationships</span></span>

<span data-ttu-id="37644-271">學生與課程實體之間，沒有多對多關聯性，而且註冊實體做為多對多聯結資料表*裝載*資料庫中。</span><span class="sxs-lookup"><span data-stu-id="37644-271">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="37644-272">「 內容 」 代表 Enrollment 資料表包含外部索引鍵的聯結的資料表 （在此情況下，主索引鍵和等級屬性） 以外的其他資料。</span><span class="sxs-lookup"><span data-stu-id="37644-272">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="37644-273">下圖顯示這些關聯性中的實體圖表的外觀。</span><span class="sxs-lookup"><span data-stu-id="37644-273">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="37644-274">(此圖表使用產生的 Entity Framework Power Tools 的 EF 6.x; 建立圖表不屬於本教學課程，正只為一個實例。)</span><span class="sxs-lookup"><span data-stu-id="37644-274">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![學生課程多對多關聯性](complex-data-model/_static/student-course.png)

<span data-ttu-id="37644-276">每個關聯性線條在另一個對多關聯性，表示有一個結束作業，並以星號 （*） 1。</span><span class="sxs-lookup"><span data-stu-id="37644-276">Each relationship line has a 1 at one end and an asterisk (*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="37644-277">如果在 Enrollment 資料表未包含等級資訊，它只需要包含兩個外部索引鍵 CourseID 和 StudentID。</span><span class="sxs-lookup"><span data-stu-id="37644-277">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="37644-278">在此情況下，它可以裝載多對多聯結資料表 （或純聯結資料表） 在資料庫。</span><span class="sxs-lookup"><span data-stu-id="37644-278">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="37644-279">「 講師 」 和 「 課程實體具有多對多關聯性，該類型和下一步是建立實體類別，以做為裝載聯結資料表。</span><span class="sxs-lookup"><span data-stu-id="37644-279">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="37644-280">（並不會隱含聯結資料表的多對多關聯性，但 EF 核心 EF 6.x 支援。</span><span class="sxs-lookup"><span data-stu-id="37644-280">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core does not.</span></span> <span data-ttu-id="37644-281">如需詳細資訊，請參閱[EF 核心 GitHub 儲存機制中的討論](https://github.com/aspnet/EntityFramework/issues/1368)。)</span><span class="sxs-lookup"><span data-stu-id="37644-281">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span> 

## <a name="the-courseassignment-entity"></a><span data-ttu-id="37644-282">CourseAssignment 實體</span><span class="sxs-lookup"><span data-stu-id="37644-282">The CourseAssignment entity</span></span>

![CourseAssignment 實體](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="37644-284">建立*Models/CourseAssignment.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="37644-284">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a><span data-ttu-id="37644-285">加入實體名稱</span><span class="sxs-lookup"><span data-stu-id="37644-285">Join entity names</span></span>

<span data-ttu-id="37644-286">聯結資料表需要講師-課程多對多關聯性的資料庫中，而且它必須由實體集。</span><span class="sxs-lookup"><span data-stu-id="37644-286">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="37644-287">通常會命名為聯結實體`EntityName1EntityName2`，它在此情況下會是`CourseInstructor`。</span><span class="sxs-lookup"><span data-stu-id="37644-287">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="37644-288">不過，我們建議您選擇描述關聯性的名稱。</span><span class="sxs-lookup"><span data-stu-id="37644-288">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="37644-289">資料模型開始簡單及隨經常收到稍後裝載否裝載聯結。</span><span class="sxs-lookup"><span data-stu-id="37644-289">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="37644-290">如果您開始使用描述性的實體名稱，您不必之後變更名稱。</span><span class="sxs-lookup"><span data-stu-id="37644-290">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="37644-291">理想情況下，聯結實體商務網域中必須有自己的自然 (可能是單一 word) 名稱。</span><span class="sxs-lookup"><span data-stu-id="37644-291">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="37644-292">比方說，書籍客戶無法連結到評等。</span><span class="sxs-lookup"><span data-stu-id="37644-292">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="37644-293">此關聯性，`CourseAssignment`是較佳的選擇，比`CourseInstructor`。</span><span class="sxs-lookup"><span data-stu-id="37644-293">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="37644-294">複合索引鍵</span><span class="sxs-lookup"><span data-stu-id="37644-294">Composite key</span></span>

<span data-ttu-id="37644-295">因為外部索引鍵都不是可為 null 且一起唯一識別資料表的每個資料列，就不必再為個別的主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="37644-295">Since the foreign keys are not nullable and together uniquely identify each row of the table, there is no need for a separate primary key.</span></span> <span data-ttu-id="37644-296">*InstructorID*和*CourseID*屬性應該當做複合主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="37644-296">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="37644-297">若要識別 EF 複合主索引鍵的唯一方法是使用*fluent API* （它無法完成使用屬性）。</span><span class="sxs-lookup"><span data-stu-id="37644-297">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="37644-298">您會看到如何設定在下一節中的複合主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="37644-298">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="37644-299">複合索引鍵可確保您可以有一個課程中，和一個講師的多個資料列的多個資料列，而您不能有多個資料列的相同講師和課程。</span><span class="sxs-lookup"><span data-stu-id="37644-299">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="37644-300">`Enrollment`聯結實體會定義它自己的主索引鍵，因此可能會有這種重複的項目。</span><span class="sxs-lookup"><span data-stu-id="37644-300">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="37644-301">若要避免這種重複的項目，您無法加入唯一的索引上的外部索引鍵欄位，或設定`Enrollment`主複合索引鍵類似`CourseAssignment`。</span><span class="sxs-lookup"><span data-stu-id="37644-301">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="37644-302">如需詳細資訊，請參閱[索引](https://docs.microsoft.com/ef/core/modeling/indexes)。</span><span class="sxs-lookup"><span data-stu-id="37644-302">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="37644-303">更新的資料庫內容</span><span class="sxs-lookup"><span data-stu-id="37644-303">Update the database context</span></span>

<span data-ttu-id="37644-304">將下列反白顯示的程式碼加入*Data/SchoolContext.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="37644-304">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="37644-305">此程式碼會新增新的實體，並設定 CourseAssignment 實體的複合主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="37644-305">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="37644-306">Fluent API 替代項目屬性</span><span class="sxs-lookup"><span data-stu-id="37644-306">Fluent API alternative to attributes</span></span>

<span data-ttu-id="37644-307">中的程式碼`OnModelCreating`方法`DbContext`類別會使用*fluent API*設定 EF 行為。</span><span class="sxs-lookup"><span data-stu-id="37644-307">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="37644-308">API 稱為 「 fluent"，因為它通常由一系列的方法呼叫一起串連成單一陳述式，從如此範例所示[EF 核心文件集](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="37644-308">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="37644-309">在本教學課程中，您使用 fluent API 僅適用於資料庫對應時，您不能與屬性。</span><span class="sxs-lookup"><span data-stu-id="37644-309">In this tutorial, you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="37644-310">不過，您也可以使用 fluent 應用程式開發的應用程式開發介面指定大部分的格式設定、 驗證和您可以使用屬性的對應規則。</span><span class="sxs-lookup"><span data-stu-id="37644-310">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="37644-311">某些屬性，例如`MinimumLength`fluent 應用程式開發的應用程式開發介面無法套用。</span><span class="sxs-lookup"><span data-stu-id="37644-311">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="37644-312">如前所述，`MinimumLength`並不會變更結構描述中，它只適用於用戶端和伺服器端驗證規則。</span><span class="sxs-lookup"><span data-stu-id="37644-312">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="37644-313">有些開發人員偏好使用 fluent 應用程式開發的應用程式開發介面，以獨佔方式，讓它們可保留他們的實體類別 「 清除 」。</span><span class="sxs-lookup"><span data-stu-id="37644-313">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="37644-314">您如果想要的話，且有少數自訂項目才可使用 fluent API，可以混合屬性和 fluent 應用程式開發的應用程式開發介面，但一般建議的作法是選擇其中一個這兩種方法，並使用，以一致的方式盡量。</span><span class="sxs-lookup"><span data-stu-id="37644-314">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="37644-315">如果您使用兩者，請注意，就會發生衝突，只要 Fluent API 會覆寫屬性。</span><span class="sxs-lookup"><span data-stu-id="37644-315">If you do use both, note that wherever there is a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="37644-316">如需 fluent API 與屬性的詳細資訊，請參閱[的組態方法](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration)。</span><span class="sxs-lookup"><span data-stu-id="37644-316">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="37644-317">實體圖表顯示關聯性</span><span class="sxs-lookup"><span data-stu-id="37644-317">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="37644-318">下圖顯示已完成的 School 模型的 Entity Framework Power Tools 建立的圖表。</span><span class="sxs-lookup"><span data-stu-id="37644-318">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![實體圖表](complex-data-model/_static/diagram.png)

<span data-ttu-id="37644-320">除了一對多關聯性線條 (1 到\*)，您可以在這裡看到到 0 或-1 一個之間的關聯線 (1 對 0..1) 「 講師 」 和 「 OfficeAssignment 實體和 0-或--一對多關聯性線條 (0..1 對 *) 之間講師和 Department 實體。</span><span class="sxs-lookup"><span data-stu-id="37644-320">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to *) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="37644-321">種子使用的測試資料的資料庫</span><span class="sxs-lookup"><span data-stu-id="37644-321">Seed the Database with Test Data</span></span>

<span data-ttu-id="37644-322">中的程式碼取代*Data/DbInitializer.cs*檔案取代下列程式碼，以針對您已建立新的實體提供種子資料。</span><span class="sxs-lookup"><span data-stu-id="37644-322">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="37644-323">如同第一個教學課程中，大部分的這段程式碼只會建立新的實體物件，並將範例資料載入至內容所需的測試。</span><span class="sxs-lookup"><span data-stu-id="37644-323">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="37644-324">請注意多對多關聯性的處理方式： 程式碼中的實體建立關聯性`Enrollments`和`CourseAssignment`加入實體集。</span><span class="sxs-lookup"><span data-stu-id="37644-324">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="37644-325">新增移轉</span><span class="sxs-lookup"><span data-stu-id="37644-325">Add a migration</span></span>

<span data-ttu-id="37644-326">儲存您的變更，並建置專案。</span><span class="sxs-lookup"><span data-stu-id="37644-326">Save your changes and build the project.</span></span> <span data-ttu-id="37644-327">然後在專案資料夾中開啟 [命令] 視窗並輸入`migrations add`命令 （不執行 update-database 命令尚未）：</span><span class="sxs-lookup"><span data-stu-id="37644-327">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="37644-328">您會收到有關可能資料遺失警告。</span><span class="sxs-lookup"><span data-stu-id="37644-328">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="37644-329">如果您嘗試執行`database update`此時命令 （不要這麼做還），就會收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="37644-329">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="37644-330">ALTER TABLE 陳述式發生衝突的外部索引鍵條件約束 」 FK_dbo。Course_dbo。Department_DepartmentID"。</span><span class="sxs-lookup"><span data-stu-id="37644-330">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="37644-331">衝突發生在資料庫"ContosoUniversity"，資料表"dbo。部門 」，資料行 'DepartmentID'。</span><span class="sxs-lookup"><span data-stu-id="37644-331">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="37644-332">有時當您執行移轉以現有的資料，您需要將虛設常式資料插入資料庫以符合外部索引鍵條件約束。</span><span class="sxs-lookup"><span data-stu-id="37644-332">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="37644-333">產生的程式碼`Up`方法會加入 Course 資料表不可為 null 的 DepartmentID 外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="37644-333">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="37644-334">如果已經有資料列在 Course 資料表中的程式碼執行時，`AddColumn`作業失敗，因為 SQL Server 並不知道何值，將置於不可為 null 的資料行。</span><span class="sxs-lookup"><span data-stu-id="37644-334">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="37644-335">本教學課程中您將在新的資料庫上執行移轉，但實際執行應用程式在您必須進行移轉處理現有的資料，所以下列指示會顯示如何執行這些作業的範例。</span><span class="sxs-lookup"><span data-stu-id="37644-335">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="37644-336">若要進行這項移轉使用現有的資料，您必須變更程式碼，讓新的資料行預設值，並建立 stub 部門名為"Temp"做為預設部門。</span><span class="sxs-lookup"><span data-stu-id="37644-336">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="37644-337">如此一來，現有的資料列課程將所有相關"Temp"部門之後`Up`方法執行。</span><span class="sxs-lookup"><span data-stu-id="37644-337">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="37644-338">開啟*{timestamp}_ComplexDataModel.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="37644-338">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span> 

* <span data-ttu-id="37644-339">註解加入 Course 資料表中的 [DepartmentID] 欄的程式碼行。</span><span class="sxs-lookup"><span data-stu-id="37644-339">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* <span data-ttu-id="37644-340">建立將 Department 資料表的程式碼之後加入下列反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="37644-340">Add the following highlighted code after the code that creates the Department table:</span></span>

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="37644-341">在實際執行應用程式中，您會撰寫程式碼或指令碼以加入部門資料列，並與新的部門資料列相關課程資料列。</span><span class="sxs-lookup"><span data-stu-id="37644-341">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="37644-342">您接著不再需要"Temp"部門或 Course.DepartmentID 資料行的預設值。</span><span class="sxs-lookup"><span data-stu-id="37644-342">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="37644-343">儲存您的變更，並建置專案。</span><span class="sxs-lookup"><span data-stu-id="37644-343">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="37644-344">變更連接字串，並更新資料庫</span><span class="sxs-lookup"><span data-stu-id="37644-344">Change the connection string and update the database</span></span>

<span data-ttu-id="37644-345">您現在有新的程式碼`DbInitializer`將新的實體的種子資料新增至空的資料庫類別。</span><span class="sxs-lookup"><span data-stu-id="37644-345">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="37644-346">若要建立新的空白資料庫的 EF，變更連接字串中的資料庫名稱*appsettings.json* ContosoUniversity3 或您並未使用您所使用的電腦上的其他部份名稱。</span><span class="sxs-lookup"><span data-stu-id="37644-346">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="37644-347">儲存變更至*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="37644-347">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="37644-348">變更資料庫名稱的替代方案，您可以刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="37644-348">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="37644-349">使用**SQL Server 物件總管**(SSOX) 或`database drop`CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="37644-349">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="37644-350">您已變更資料庫名稱，或刪除資料庫之後，請執行`database update`命令在命令視窗中執行移轉。</span><span class="sxs-lookup"><span data-stu-id="37644-350">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="37644-351">執行應用程式，讓`DbInitializer.Initialize`方法來執行，並填入新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="37644-351">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="37644-352">SSOX 中開啟的資料庫，您可以如同更早版本，然後展開**資料表**節點以查看所有資料表具有已建立。</span><span class="sxs-lookup"><span data-stu-id="37644-352">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="37644-353">(如果仍有 SSOX 開啟從較早的時間，按一下**重新整理** 按鈕。)</span><span class="sxs-lookup"><span data-stu-id="37644-353">(If you still have SSOX open from the earlier time, click the **Refresh** button.)</span></span>

![SSOX 中的資料表](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="37644-355">執行觸發植入資料庫的初始設定式程式碼應用程式。</span><span class="sxs-lookup"><span data-stu-id="37644-355">Run the app to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="37644-356">以滑鼠右鍵按一下**CourseAssignment**資料表，然後選取**檢視資料**以確認它中沒有資料。</span><span class="sxs-lookup"><span data-stu-id="37644-356">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![SSOX CourseAssignment 資料](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="37644-358">總結</span><span class="sxs-lookup"><span data-stu-id="37644-358">Summary</span></span>

<span data-ttu-id="37644-359">您現在有更複雜的資料模型和對應的資料庫。</span><span class="sxs-lookup"><span data-stu-id="37644-359">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="37644-360">下列教學課程中，您將進一步了解如何存取相關的資料。</span><span class="sxs-lookup"><span data-stu-id="37644-360">In the following tutorial, you'll learn more about how to access related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="37644-361">[上一頁](migrations.md)
[下一頁](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="37644-361">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>  
