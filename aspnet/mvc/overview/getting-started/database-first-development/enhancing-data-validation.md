---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 第一個使用 ASP.NET MVC 的 EF 資料庫： 強化資料驗證 |Microsoft 文件
author: tfitzmac
description: 使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程里...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 8ea2e94db7956b76c5ccf0a139ac024e38910b49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a><span data-ttu-id="36794-104">第一個使用 ASP.NET MVC 的 EF 資料庫： 強化資料驗證</span><span class="sxs-lookup"><span data-stu-id="36794-104">EF Database First with ASP.NET MVC: Enhancing Data Validation</span></span>
====================
<span data-ttu-id="36794-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="36794-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="36794-106">使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="36794-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="36794-107">此教學課程的數列會顯示如何自動產生程式碼，可讓使用者顯示、 編輯、 建立和刪除存在於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="36794-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="36794-108">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="36794-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="36794-109">數列的這個部分著重在將資料註解加入至資料模型，以指定驗證需求，並顯示格式設定。</span><span class="sxs-lookup"><span data-stu-id="36794-109">This part of the series focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="36794-110">它已根據上改善的註解區段中的使用者意見。</span><span class="sxs-lookup"><span data-stu-id="36794-110">It was improved based on feedback from users in the comments section.</span></span>


## <a name="add-data-annotations"></a><span data-ttu-id="36794-111">新增資料註解</span><span class="sxs-lookup"><span data-stu-id="36794-111">Add data annotations</span></span>

<span data-ttu-id="36794-112">如同先前的主題，一些資料的驗證規則會自動套用到使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="36794-112">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="36794-113">例如，您可以只提供數字等級屬性。</span><span class="sxs-lookup"><span data-stu-id="36794-113">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="36794-114">若要指定詳細資料的驗證規則，您可以將資料註解加入至模型類別。</span><span class="sxs-lookup"><span data-stu-id="36794-114">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="36794-115">這些註解會套用在 web 應用程式指定的屬性。</span><span class="sxs-lookup"><span data-stu-id="36794-115">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="36794-116">您也可以套用變更之屬性的顯示方式; 的格式屬性例如，變更文字標籤所使用的值。</span><span class="sxs-lookup"><span data-stu-id="36794-116">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="36794-117">在本教學課程中，您將加入資料註解，FirstName、 LastName 和 MiddleName 屬性提供的值長度限制。</span><span class="sxs-lookup"><span data-stu-id="36794-117">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="36794-118">在資料庫中，這些值會限制為 50 個字元。不過，web 應用程式中，字元是目前未強制限制。</span><span class="sxs-lookup"><span data-stu-id="36794-118">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="36794-119">使用者提供超過 50 個字元的其中一個值，如果嘗試將值儲存至資料庫時，會損毀頁面。</span><span class="sxs-lookup"><span data-stu-id="36794-119">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="36794-120">您也會限制等級為 0 到 4 之間的值。</span><span class="sxs-lookup"><span data-stu-id="36794-120">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="36794-121">開啟**Student.cs**檔案**模型**資料夾。</span><span class="sxs-lookup"><span data-stu-id="36794-121">Open the **Student.cs** file in the **Models** folder.</span></span> <span data-ttu-id="36794-122">將下列反白顯示的程式碼加入至類別。</span><span class="sxs-lookup"><span data-stu-id="36794-122">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="36794-123">在 Enrollment.cs，加入下列反白顯示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="36794-123">In Enrollment.cs, add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="36794-124">建置方案。</span><span class="sxs-lookup"><span data-stu-id="36794-124">Build the solution.</span></span>

<span data-ttu-id="36794-125">瀏覽頁面，以便進行編輯或建立一位學生。</span><span class="sxs-lookup"><span data-stu-id="36794-125">Browse to a page for editing or creating a student.</span></span> <span data-ttu-id="36794-126">如果您嘗試輸入超過 50 個字元，則會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="36794-126">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![顯示錯誤訊息](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="36794-128">瀏覽至網頁進行編輯的註冊項目，並會嘗試提供 4 上方的等級。</span><span class="sxs-lookup"><span data-stu-id="36794-128">Browse to the page for editing enrollments, and attempt to provide a grade above 4.</span></span>

![等級範圍錯誤](enhancing-data-validation/_static/image2.png)

<span data-ttu-id="36794-130">資料驗證註解，您可以將套用至屬性和類別的完整清單，請參閱[System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)。</span><span class="sxs-lookup"><span data-stu-id="36794-130">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="36794-131">新增中繼資料類別</span><span class="sxs-lookup"><span data-stu-id="36794-131">Add metadata classes</span></span>

<span data-ttu-id="36794-132">驗證屬性直接加入模型類別運作時不想要變更; 因此，資料庫不過，如果您的資料庫變更，且您需要重新產生模型類別，您會遺失所有您已套用至模型類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="36794-132">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="36794-133">這個方法可能非常沒有效率且容易會遺失重要的驗證規則。</span><span class="sxs-lookup"><span data-stu-id="36794-133">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="36794-134">若要避免這個問題，您可以加入包含屬性的中繼資料類別。</span><span class="sxs-lookup"><span data-stu-id="36794-134">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="36794-135">當您建立關聯的中繼資料類別的模型類別時，這些屬性會套用至模型。</span><span class="sxs-lookup"><span data-stu-id="36794-135">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="36794-136">這種方法，而不會遺失的所有屬性已套用的中繼資料類別可以會重新產生模型類別。</span><span class="sxs-lookup"><span data-stu-id="36794-136">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="36794-137">在**模型**資料夾中，加入名為類別**Metadata.cs**。</span><span class="sxs-lookup"><span data-stu-id="36794-137">In the **Models** folder, add a class named **Metadata.cs**.</span></span>

![新增中繼資料類別](enhancing-data-validation/_static/image3.png)

<span data-ttu-id="36794-139">Metadata.cs 中的程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="36794-139">Replace the code in Metadata.cs with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="36794-140">這些中繼資料類別可包含的所有驗證屬性，您先前套用至模型類別。</span><span class="sxs-lookup"><span data-stu-id="36794-140">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="36794-141">**顯示**屬性用來變更所使用的文字標籤的值。</span><span class="sxs-lookup"><span data-stu-id="36794-141">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="36794-142">現在，您必須將模型類別關聯的中繼資料類別。</span><span class="sxs-lookup"><span data-stu-id="36794-142">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="36794-143">在**模型**資料夾中，加入名為類別**PartialClasses.cs**。</span><span class="sxs-lookup"><span data-stu-id="36794-143">In the **Models** folder, add a class named **PartialClasses.cs**.</span></span>

<span data-ttu-id="36794-144">取代下列程式碼檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="36794-144">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="36794-145">請注意，每個類別標示為`partial`類別，而每個符合名稱和命名空間為自動產生的類別。</span><span class="sxs-lookup"><span data-stu-id="36794-145">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="36794-146">將中繼資料屬性套用至部分類別，您可以確保資料的驗證屬性，會套用至自動產生的類別。</span><span class="sxs-lookup"><span data-stu-id="36794-146">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="36794-147">當您重新產生模型類別，因為中繼資料已套用該屬性不會重新產生的部分類別中，這些屬性將不會遺失。</span><span class="sxs-lookup"><span data-stu-id="36794-147">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="36794-148">若要重新產生的自動產生的類別，請開啟 ContosoModel.edmx 檔案。</span><span class="sxs-lookup"><span data-stu-id="36794-148">To regenerate the automatically-generated classes, open the ContosoModel.edmx file.</span></span> <span data-ttu-id="36794-149">同樣地，以滑鼠右鍵按一下 設計介面，並選取**從資料庫更新模型**。</span><span class="sxs-lookup"><span data-stu-id="36794-149">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="36794-150">即使沒有變更資料庫，此程序會重新產生的類別。</span><span class="sxs-lookup"><span data-stu-id="36794-150">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="36794-151">在**重新整理**索引標籤上，選取**資料表**和**完成**。</span><span class="sxs-lookup"><span data-stu-id="36794-151">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

![重新整理的資料表](enhancing-data-validation/_static/image4.png)

<span data-ttu-id="36794-153">儲存 ContosoModel.edmx 檔案才能套用變更。</span><span class="sxs-lookup"><span data-stu-id="36794-153">Save the ContosoModel.edmx file to apply the changes.</span></span>

<span data-ttu-id="36794-154">開啟 Student.cs 檔案或 Enrollment.cs 檔案，並請注意，先前所套用的資料驗證屬性不再是檔案中。</span><span class="sxs-lookup"><span data-stu-id="36794-154">Open the Student.cs file or the Enrollment.cs file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="36794-155">不過，執行應用程式，並注意當您輸入的資料仍會套用驗證規則。</span><span class="sxs-lookup"><span data-stu-id="36794-155">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="36794-156">[上一頁](customizing-a-view.md)
> [下一頁](publish-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="36794-156">[Previous](customizing-a-view.md)
[Next](publish-to-azure.md)</span></span>
