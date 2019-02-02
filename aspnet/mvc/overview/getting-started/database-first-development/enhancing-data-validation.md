---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 教學課程：增強 EF Database First 與 ASP.NET MVC 應用程式的資料的驗證
description: 本教學課程著重於將資料註解加入至資料模型，以指定驗證需求，並顯示格式設定。
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667618"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="e99cf-103">教學課程：增強 EF Database First 與 ASP.NET MVC 應用程式的資料的驗證</span><span class="sxs-lookup"><span data-stu-id="e99cf-103">Tutorial: Enhance data validation for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="e99cf-104">您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="e99cf-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="e99cf-105">本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="e99cf-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="e99cf-106">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="e99cf-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="e99cf-107">本教學課程著重於將資料註解加入至資料模型，以指定驗證需求，並顯示格式設定。</span><span class="sxs-lookup"><span data-stu-id="e99cf-107">This tutorial focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="e99cf-108">它已改善根據意見反應的註解區段中的使用者。</span><span class="sxs-lookup"><span data-stu-id="e99cf-108">It was improved based on feedback from users in the comments section.</span></span>

<span data-ttu-id="e99cf-109">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="e99cf-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e99cf-110">新增資料註解</span><span class="sxs-lookup"><span data-stu-id="e99cf-110">Add data annotations</span></span>
> * <span data-ttu-id="e99cf-111">加入中繼資料類別</span><span class="sxs-lookup"><span data-stu-id="e99cf-111">Add metadata classes</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e99cf-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="e99cf-112">Prerequisites</span></span>

* [<span data-ttu-id="e99cf-113">自訂檢視</span><span class="sxs-lookup"><span data-stu-id="e99cf-113">Customize a view</span></span>](customizing-a-view.md)

## <a name="add-data-annotations"></a><span data-ttu-id="e99cf-114">新增資料註解</span><span class="sxs-lookup"><span data-stu-id="e99cf-114">Add data annotations</span></span>

<span data-ttu-id="e99cf-115">如您所見在先前主題中，某些資料驗證規則會自動套用到使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="e99cf-115">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="e99cf-116">比方說，您只可以提供一些級屬性。</span><span class="sxs-lookup"><span data-stu-id="e99cf-116">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="e99cf-117">若要指定更多的資料驗證規則，您可以新增至您的模型類別的資料註解。</span><span class="sxs-lookup"><span data-stu-id="e99cf-117">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="e99cf-118">這些註解會套用在您的 web 應用程式，針對指定的屬性。</span><span class="sxs-lookup"><span data-stu-id="e99cf-118">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="e99cf-119">您也可以套用變更之屬性的顯示方式; 的格式屬性例如，變更用於文字標籤的值。</span><span class="sxs-lookup"><span data-stu-id="e99cf-119">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="e99cf-120">在本教學課程中，您將加入資料註解提供的 FirstName、 LastName 和 MiddleName 屬性的值將長度限制。</span><span class="sxs-lookup"><span data-stu-id="e99cf-120">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="e99cf-121">在資料庫中，這些值僅限於 50 個字元;不過，在您的 web 應用程式中的字元限制目前不會強制執行。</span><span class="sxs-lookup"><span data-stu-id="e99cf-121">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="e99cf-122">如果使用者提供超過 50 個字元的其中一個值，當您嘗試將值儲存至資料庫時，會損毀頁面。</span><span class="sxs-lookup"><span data-stu-id="e99cf-122">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="e99cf-123">您也會限制級為 0 到 4 之間的值。</span><span class="sxs-lookup"><span data-stu-id="e99cf-123">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="e99cf-124">選取 **模型** > **ContosoModel.edmx** > **ContosoModel.tt** ，然後開啟*Student.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="e99cf-124">Select **Models** > **ContosoModel.edmx** > **ContosoModel.tt** and open the *Student.cs* file.</span></span> <span data-ttu-id="e99cf-125">將下列反白顯示的程式碼新增至類別。</span><span class="sxs-lookup"><span data-stu-id="e99cf-125">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="e99cf-126">開啟*Enrollment.cs*並新增下列醒目提示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e99cf-126">Open *Enrollment.cs* and add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="e99cf-127">建置方案。</span><span class="sxs-lookup"><span data-stu-id="e99cf-127">Build the solution.</span></span>

<span data-ttu-id="e99cf-128">按一下 **學生的清單**，然後選取**編輯**。</span><span class="sxs-lookup"><span data-stu-id="e99cf-128">Click **List of students** and select **Edit**.</span></span> <span data-ttu-id="e99cf-129">如果您嘗試輸入超過 50 個字元，則會顯示一則錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e99cf-129">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![顯示錯誤訊息](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="e99cf-131">回到首頁。</span><span class="sxs-lookup"><span data-stu-id="e99cf-131">Go back to the home page.</span></span> <span data-ttu-id="e99cf-132">按一下 **註冊清單**，然後選取**編輯**。</span><span class="sxs-lookup"><span data-stu-id="e99cf-132">Click **List of enrollments** and select **Edit**.</span></span> <span data-ttu-id="e99cf-133">嘗試提供一個等級，上述 4。</span><span class="sxs-lookup"><span data-stu-id="e99cf-133">Attempt to provide a grade above 4.</span></span> <span data-ttu-id="e99cf-134">您會收到此錯誤：*等級必須是介於 0 到 4 之間的欄位。*</span><span class="sxs-lookup"><span data-stu-id="e99cf-134">You will receive this error: *The field Grade must be between 0 and 4.*</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="e99cf-135">加入中繼資料類別</span><span class="sxs-lookup"><span data-stu-id="e99cf-135">Add metadata classes</span></span>

<span data-ttu-id="e99cf-136">當您不想要變更; 的資料庫模型類別中直接加入驗證屬性運作不過，如果您的資料庫變更，而且您需要重新產生模型類別，您會遺失所有您已套用至模型類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="e99cf-136">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="e99cf-137">這種方法可以是非常沒有效率且容易會遺失重要的驗證規則。</span><span class="sxs-lookup"><span data-stu-id="e99cf-137">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="e99cf-138">若要避免這個問題，您可以新增包含屬性的中繼資料類別。</span><span class="sxs-lookup"><span data-stu-id="e99cf-138">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="e99cf-139">當您建立關聯的中繼資料類別的模型類別時，這些屬性會套用至模型中。</span><span class="sxs-lookup"><span data-stu-id="e99cf-139">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="e99cf-140">這種方法，就可以重新產生模型類別而不會遺失所有已套用至中繼資料類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="e99cf-140">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="e99cf-141">在 **模型**資料夾中，加入名為類別*Metadata.cs*。</span><span class="sxs-lookup"><span data-stu-id="e99cf-141">In the **Models** folder, add a class named *Metadata.cs*.</span></span>

<span data-ttu-id="e99cf-142">中的程式碼取代*Metadata.cs*為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="e99cf-142">Replace the code in *Metadata.cs* with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="e99cf-143">這些中繼資料類別可包含的所有驗證屬性，您先前套用至模型類別。</span><span class="sxs-lookup"><span data-stu-id="e99cf-143">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="e99cf-144">**顯示**屬性用來變更所使用的文字標籤的值。</span><span class="sxs-lookup"><span data-stu-id="e99cf-144">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="e99cf-145">現在，您必須將模型類別關聯的中繼資料類別。</span><span class="sxs-lookup"><span data-stu-id="e99cf-145">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="e99cf-146">在 **模型**資料夾中，加入名為類別*PartialClasses.cs*。</span><span class="sxs-lookup"><span data-stu-id="e99cf-146">In the **Models** folder, add a class named *PartialClasses.cs*.</span></span>

<span data-ttu-id="e99cf-147">取代下列程式碼檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="e99cf-147">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="e99cf-148">請注意，每個類別標示為`partial`類別，和每個符合的名稱和命名空間會自動產生的類別。</span><span class="sxs-lookup"><span data-stu-id="e99cf-148">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="e99cf-149">藉由套用至部分類別的中繼資料屬性，確定您的資料驗證屬性將會套用至自動產生的類別。</span><span class="sxs-lookup"><span data-stu-id="e99cf-149">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="e99cf-150">當您重新產生模型類別，因為中繼資料屬性會套用在不會重新產生的部分類別中，這些屬性將不會遺失。</span><span class="sxs-lookup"><span data-stu-id="e99cf-150">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="e99cf-151">若要重新產生的自動產生的類別，請開啟*ContosoModel.edmx*檔案。</span><span class="sxs-lookup"><span data-stu-id="e99cf-151">To regenerate the automatically-generated classes, open the *ContosoModel.edmx* file.</span></span> <span data-ttu-id="e99cf-152">同樣地，以滑鼠右鍵按一下 設計介面，然後選取**從資料庫更新模型**。</span><span class="sxs-lookup"><span data-stu-id="e99cf-152">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="e99cf-153">即使您未變更的資料庫，此程序會重新產生類別。</span><span class="sxs-lookup"><span data-stu-id="e99cf-153">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="e99cf-154">在 **重新整理**索引標籤上，選取**資料表**並**完成**。</span><span class="sxs-lookup"><span data-stu-id="e99cf-154">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

<span data-ttu-id="e99cf-155">儲存*ContosoModel.edmx*檔案，以套用所做的變更。</span><span class="sxs-lookup"><span data-stu-id="e99cf-155">Save the *ContosoModel.edmx* file to apply the changes.</span></span>

<span data-ttu-id="e99cf-156">開啟*Student.cs*檔案或*Enrollment.cs*檔案，並請注意，您在稍早所套用的資料驗證屬性已不在檔案中。</span><span class="sxs-lookup"><span data-stu-id="e99cf-156">Open the *Student.cs* file or the *Enrollment.cs* file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="e99cf-157">不過，執行應用程式，並請注意當您輸入的資料仍然會套用驗證規則。</span><span class="sxs-lookup"><span data-stu-id="e99cf-157">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

## <a name="conclusion"></a><span data-ttu-id="e99cf-158">結論</span><span class="sxs-lookup"><span data-stu-id="e99cf-158">Conclusion</span></span>

<span data-ttu-id="e99cf-159">這一系列將提供如何從現有的資料庫，可讓使用者編輯、 更新、 建立和刪除的資料產生程式碼的簡單範例。</span><span class="sxs-lookup"><span data-stu-id="e99cf-159">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="e99cf-160">它使用 ASP.NET MVC 5、 Entity Framework 與 ASP.NET Scaffolding 來建立專案。</span><span class="sxs-lookup"><span data-stu-id="e99cf-160">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span> 

<span data-ttu-id="e99cf-161">Code First 開發簡介範例，請參閱[Getting Started with ASP.NET MVC 5](../introduction/getting-started.md)。</span><span class="sxs-lookup"><span data-stu-id="e99cf-161">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> 

<span data-ttu-id="e99cf-162">如需更進階的範例，請參閱 <<c0> [ 建立 ASP.NET MVC 4 應用程式的 Entity Framework 資料模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="e99cf-162">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="e99cf-163">請注意，您使用第一個資料庫中的資料使用 DbContext API 與您用來處理在 Code First 資料的 API 相同。</span><span class="sxs-lookup"><span data-stu-id="e99cf-163">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="e99cf-164">即使您想要使用第一個資料庫，您可以了解如何處理更複雜的案例，例如讀取及更新的相關的資料，處理並行衝突，Code First 的教學課程中，依此類推。</span><span class="sxs-lookup"><span data-stu-id="e99cf-164">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="e99cf-165">唯一的差別是在建立資料庫、 內容類別和實體類別的方式。</span><span class="sxs-lookup"><span data-stu-id="e99cf-165">The only difference is in how the database, context class, and entity classes are created.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e99cf-166">其他資源</span><span class="sxs-lookup"><span data-stu-id="e99cf-166">Additional resources</span></span>

<span data-ttu-id="e99cf-167">您可以套用至屬性和類別的資料驗證註解的完整清單，請參閱 < [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e99cf-167">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e99cf-168">後續步驟</span><span class="sxs-lookup"><span data-stu-id="e99cf-168">Next steps</span></span>

<span data-ttu-id="e99cf-169">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="e99cf-169">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e99cf-170">已新增的資料註解</span><span class="sxs-lookup"><span data-stu-id="e99cf-170">Added data annotations</span></span>
> * <span data-ttu-id="e99cf-171">已新增的中繼資料類別</span><span class="sxs-lookup"><span data-stu-id="e99cf-171">Added metadata classes</span></span>

<span data-ttu-id="e99cf-172">若要了解如何部署至 Azure App Service 的 web 應用程式和 SQL database，請參閱本教學課程：</span><span class="sxs-lookup"><span data-stu-id="e99cf-172">To learn how to deploy a web app and SQL database to Azure App Service, see this tutorial:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="e99cf-173">將.NET 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e99cf-173">Deploy a .NET app to Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)
