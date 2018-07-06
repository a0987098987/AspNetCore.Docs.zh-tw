---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: 更新、 刪除和建立資料模型繫結和 web form |Microsoft Docs
author: tfitzmac
description: 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行資料互動更多簡單-...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 1cf9873db177b67927b579def1eedd08e3e9a762
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821628"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="c1e7d-104">更新、 刪除和建立資料模型繫結和 web form</span><span class="sxs-lookup"><span data-stu-id="c1e7d-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="c1e7d-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c1e7d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c1e7d-106">本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="c1e7d-107">模型繫結進行更簡單的比處理資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="c1e7d-108">此系列簡介資料的開頭，並在稍後的教學課程中將移至更進階的概念。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="c1e7d-109">本教學課程會示範如何建立、 更新和刪除與模型繫結的資料。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="c1e7d-110">您會設定下列屬性：</span><span class="sxs-lookup"><span data-stu-id="c1e7d-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="c1e7d-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="c1e7d-111">DeleteMethod</span></span>
> - <span data-ttu-id="c1e7d-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="c1e7d-112">InsertMethod</span></span>
> - <span data-ttu-id="c1e7d-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="c1e7d-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="c1e7d-114">這些屬性會接收對應的作業會處理方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="c1e7d-115">在該方法中，您提供的邏輯與資料互動。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="c1e7d-116">本教學課程是根據在第一個建立的專案[一部分](retrieving-data.md)的數列。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="c1e7d-117">您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)完整的專案，以 C# 或 vb。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="c1e7d-118">可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="c1e7d-119">它會使用 Visual Studio 2012 範本，也就是本教學課程中的 Visual Studio 2013 範本稍有不同。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="c1e7d-120">您將建置</span><span class="sxs-lookup"><span data-stu-id="c1e7d-120">What you'll build</span></span>

<span data-ttu-id="c1e7d-121">在本教學課程中，您將會：</span><span class="sxs-lookup"><span data-stu-id="c1e7d-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="c1e7d-122">新增動態資料範本</span><span class="sxs-lookup"><span data-stu-id="c1e7d-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="c1e7d-123">透過模型繫結方法來啟用更新和刪除資料</span><span class="sxs-lookup"><span data-stu-id="c1e7d-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="c1e7d-124">將資料驗證規則套用-啟用資料庫中建立新的記錄</span><span class="sxs-lookup"><span data-stu-id="c1e7d-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="c1e7d-125">新增動態資料範本</span><span class="sxs-lookup"><span data-stu-id="c1e7d-125">Add dynamic data templates</span></span>

<span data-ttu-id="c1e7d-126">若要提供最佳使用者體驗，並減少重複的程式碼，您會使用動態資料範本。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="c1e7d-127">您可以安裝 NuGet 套件，到現有的站台上輕鬆整合預先建置的動態資料範本。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="c1e7d-128">從**管理 NuGet 套件**，安裝**DynamicDataTemplatesCS**。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![動態資料範本](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="c1e7d-130">請注意，您的專案現在會包含名為的資料夾**動態**。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="c1e7d-131">在該資料夾中，您會發現要自動套用到在您的 web form 中的動態控制項的範本。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![動態資料資料夾](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="c1e7d-133">啟用更新和刪除</span><span class="sxs-lookup"><span data-stu-id="c1e7d-133">Enable updating and deleting</span></span>

<span data-ttu-id="c1e7d-134">讓使用者更新和刪除資料庫中的記錄會擷取資料的程序非常類似。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="c1e7d-135">在  **UpdateMethod**並**DeleteMethod**屬性，您可以指定執行這些作業的方法名稱。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="c1e7d-136">與 GridView 控制項，您也可以指定自動產生的編輯和刪除按鈕。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="c1e7d-137">下列醒目提示的程式碼顯示的 GridView 程式碼的新增項目。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="c1e7d-138">在程式碼後置檔案中，新增 using 陳述式**System.Data.Entity.Infrastructure**。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="c1e7d-139">然後，新增下列更新和刪除方法。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="c1e7d-140">**TryUpdateModel**方法從 web form 相符的資料繫結值套用至資料項目。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="c1e7d-141">根據 id 參數的值，擷取資料的項目。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="c1e7d-142">強制執行驗證需求</span><span class="sxs-lookup"><span data-stu-id="c1e7d-142">Enforce validation requirements</span></span>

<span data-ttu-id="c1e7d-143">更新資料時，會自動強制執行驗證屬性套用至 Student 類別中的 FirstName、 LastName 和年的屬性。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="c1e7d-144">DynamicField 控制項加入以驗證屬性為基礎的用戶端和伺服器驗證。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="c1e7d-145">FirstName 和 LastName 屬性都需要。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="c1e7d-146">FirstName 不能超過 20 個字元、 長度和 LastName 不能超過 40 個字元。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="c1e7d-147">年份必須 AcademicYear 列舉的有效值。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="c1e7d-148">如果使用者違反了其中一項驗證需求，就不會繼續更新。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="c1e7d-149">若要查看錯誤訊息，新增上述 GridView ValidationSummary 控制項。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="c1e7d-150">若要顯示從模型繫結的驗證錯誤，請設定**ShowModelStateErrors**屬性設定為 **，則為 true**。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="c1e7d-151">執行 web 應用程式中，更新和刪除任何記錄。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-151">Run the web application, and update and delete any of the records.</span></span>

![更新資料](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="c1e7d-153">請注意，當處於編輯模式 [年] 屬性的值自動轉譯為下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="c1e7d-154">[年] 屬性是列舉值，並列舉值的動態資料範本指定的下拉式清單中進行編輯。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="c1e7d-155">您可以找到該範本，開啟**列舉型別\_Edit.ascx**中的檔案**動態**/**FieldTemplates**資料夾。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="c1e7d-156">如果您提供有效的值，更新就會順利完成。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="c1e7d-157">如果您違反其中一項驗證需求，不會繼續更新和方格上方顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![錯誤訊息](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="c1e7d-159">新增新的記錄</span><span class="sxs-lookup"><span data-stu-id="c1e7d-159">Add new records</span></span>

<span data-ttu-id="c1e7d-160">GridView 控制項不包含**InsertMethod**屬性，因此無法使用模型繫結中加入新的記錄。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="c1e7d-161">您可以找到 InsertMethod 屬性**FormView**， **DetailsView**，或**ListView**控制項。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="c1e7d-162">在本教學課程中，您將使用 FormView 控制項來加入新的記錄。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="c1e7d-163">首先，加入新的頁面，您將建立新記錄的連結。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="c1e7d-164">ValidationSummary，上方新增：</span><span class="sxs-lookup"><span data-stu-id="c1e7d-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="c1e7d-165">新的連結會出現在頂端的 [Students] 頁面的內容。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-165">The new link will appear at the top of the content for the Students page.</span></span>

![新的連結](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="c1e7d-167">然後，新增新的 web form 使用主版頁面，並將它命名**AddStudent**。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="c1e7d-168">您可以選取 Site.Master 作為主版頁面。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="c1e7d-169">您將轉譯的欄位使用，以加入新的學生**DynamicEntity**控制項。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="c1e7d-170">DynamicEntity 控制項呈現 ItemType 屬性中指定的類別中的可編輯的屬性。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="c1e7d-171">StudentID 屬性標記為 **[ScaffoldColumn(false)]** 屬性，讓它不會呈現。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="c1e7d-172">在 AddStudent 頁面 MainContent 預留位置，加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="c1e7d-173">在程式碼後置檔案 (AddStudent.aspx.cs) 中，新增**使用**陳述式**ContosoUniversityModelBinding.Models**命名空間。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="c1e7d-174">然後，新增下列方法來指定如何插入新的記錄 和 取消 按鈕的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="c1e7d-175">將儲存的所有變更。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-175">Save all of the changes.</span></span>

<span data-ttu-id="c1e7d-176">執行 web 應用程式，並建立新的學生。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-176">Run the web application and create a new student.</span></span>

![加入新的學生](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="c1e7d-178">按一下 **插入**並注意已建立新的學生。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-178">Click **Insert** and notice the new student has been created.</span></span>

![顯示新的學生](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="c1e7d-180">結論</span><span class="sxs-lookup"><span data-stu-id="c1e7d-180">Conclusion</span></span>

<span data-ttu-id="c1e7d-181">在本教學課程中，您可以啟用更新、 刪除和建立資料。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="c1e7d-182">您可確保與資料互動時，會套用驗證規則。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="c1e7d-183">在接下來[教學課程](sorting-paging-and-filtering-data.md)在本系列中，您將啟用排序、 分頁和篩選資料。</span><span class="sxs-lookup"><span data-stu-id="c1e7d-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c1e7d-184">[上一頁](retrieving-data.md)
> [下一頁](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="c1e7d-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
