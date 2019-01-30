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
ms.openlocfilehash: 85299d70c6cba52c1d40a42edfd429c96318134a
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236480"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="8073d-103">教學課程：增強 EF Database First 與 ASP.NET MVC 應用程式的資料的驗證</span><span class="sxs-lookup"><span data-stu-id="8073d-103">Tutorial: Enhance data validation for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="8073d-104">您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="8073d-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="8073d-105">本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="8073d-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="8073d-106">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="8073d-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="8073d-107">本教學課程著重於將資料註解加入至資料模型，以指定驗證需求，並顯示格式設定。</span><span class="sxs-lookup"><span data-stu-id="8073d-107">This tutorial focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="8073d-108">它已改善根據意見反應的註解區段中的使用者。</span><span class="sxs-lookup"><span data-stu-id="8073d-108">It was improved based on feedback from users in the comments section.</span></span>

<span data-ttu-id="8073d-109">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="8073d-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8073d-110">新增資料註解</span><span class="sxs-lookup"><span data-stu-id="8073d-110">Add data annotations</span></span>
> * <span data-ttu-id="8073d-111">加入中繼資料類別</span><span class="sxs-lookup"><span data-stu-id="8073d-111">Add metadata classes</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8073d-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="8073d-112">Prerequisites</span></span>

* [<span data-ttu-id="8073d-113">自訂檢視</span><span class="sxs-lookup"><span data-stu-id="8073d-113">Customize a view</span></span>](customizing-a-view.md)

## <a name="add-data-annotations"></a><span data-ttu-id="8073d-114">新增資料註解</span><span class="sxs-lookup"><span data-stu-id="8073d-114">Add data annotations</span></span>

<span data-ttu-id="8073d-115">如您所見在先前主題中，某些資料驗證規則會自動套用到使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="8073d-115">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="8073d-116">比方說，您只可以提供一些級屬性。</span><span class="sxs-lookup"><span data-stu-id="8073d-116">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="8073d-117">若要指定更多的資料驗證規則，您可以新增至您的模型類別的資料註解。</span><span class="sxs-lookup"><span data-stu-id="8073d-117">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="8073d-118">這些註解會套用在您的 web 應用程式，針對指定的屬性。</span><span class="sxs-lookup"><span data-stu-id="8073d-118">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="8073d-119">您也可以套用變更之屬性的顯示方式; 的格式屬性例如，變更用於文字標籤的值。</span><span class="sxs-lookup"><span data-stu-id="8073d-119">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="8073d-120">在本教學課程中，您將加入資料註解提供的 FirstName、 LastName 和 MiddleName 屬性的值將長度限制。</span><span class="sxs-lookup"><span data-stu-id="8073d-120">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="8073d-121">在資料庫中，這些值僅限於 50 個字元;不過，在您的 web 應用程式中的字元限制目前不會強制執行。</span><span class="sxs-lookup"><span data-stu-id="8073d-121">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="8073d-122">如果使用者提供超過 50 個字元的其中一個值，當您嘗試將值儲存至資料庫時，會損毀頁面。</span><span class="sxs-lookup"><span data-stu-id="8073d-122">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="8073d-123">您也會限制級為 0 到 4 之間的值。</span><span class="sxs-lookup"><span data-stu-id="8073d-123">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="8073d-124">選取 **模型** > **ContosoModel.edmx** > **ContosoModel.tt** ，然後開啟*Student.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="8073d-124">Select **Models** > **ContosoModel.edmx** > **ContosoModel.tt** and open the *Student.cs* file.</span></span> <span data-ttu-id="8073d-125">將下列反白顯示的程式碼新增至類別。</span><span class="sxs-lookup"><span data-stu-id="8073d-125">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="8073d-126">開啟*Enrollment.cs*並新增下列醒目提示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="8073d-126">Open *Enrollment.cs* and add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="8073d-127">建置方案。</span><span class="sxs-lookup"><span data-stu-id="8073d-127">Build the solution.</span></span>

<span data-ttu-id="8073d-128">按一下 **學生的清單**，然後選取**編輯**。</span><span class="sxs-lookup"><span data-stu-id="8073d-128">Click **List of students** and select **Edit**.</span></span> <span data-ttu-id="8073d-129">如果您嘗試輸入超過 50 個字元，則會顯示一則錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8073d-129">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![顯示錯誤訊息](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="8073d-131">回到首頁。</span><span class="sxs-lookup"><span data-stu-id="8073d-131">Go back to the home page.</span></span> <span data-ttu-id="8073d-132">按一下 **註冊清單**，然後選取**編輯**。</span><span class="sxs-lookup"><span data-stu-id="8073d-132">Click **List of enrollments** and select **Edit**.</span></span> <span data-ttu-id="8073d-133">嘗試提供一個等級，上述 4。</span><span class="sxs-lookup"><span data-stu-id="8073d-133">Attempt to provide a grade above 4.</span></span> <span data-ttu-id="8073d-134">您會收到此錯誤：*等級必須是介於 0 到 4 之間的欄位。*</span><span class="sxs-lookup"><span data-stu-id="8073d-134">You will receive this error: *The field Grade must be between 0 and 4.*</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="8073d-135">加入中繼資料類別</span><span class="sxs-lookup"><span data-stu-id="8073d-135">Add metadata classes</span></span>

<span data-ttu-id="8073d-136">當您不想要變更; 的資料庫模型類別中直接加入驗證屬性運作不過，如果您的資料庫變更，而且您需要重新產生模型類別，您會遺失所有您已套用至模型類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="8073d-136">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="8073d-137">這種方法可以是非常沒有效率且容易會遺失重要的驗證規則。</span><span class="sxs-lookup"><span data-stu-id="8073d-137">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="8073d-138">若要避免這個問題，您可以新增包含屬性的中繼資料類別。</span><span class="sxs-lookup"><span data-stu-id="8073d-138">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="8073d-139">當您建立關聯的中繼資料類別的模型類別時，這些屬性會套用至模型中。</span><span class="sxs-lookup"><span data-stu-id="8073d-139">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="8073d-140">這種方法，就可以重新產生模型類別而不會遺失所有已套用至中繼資料類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="8073d-140">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="8073d-141">在 **模型**資料夾中，加入名為類別*Metadata.cs*。</span><span class="sxs-lookup"><span data-stu-id="8073d-141">In the **Models** folder, add a class named *Metadata.cs*.</span></span>

<span data-ttu-id="8073d-142">中的程式碼取代*Metadata.cs*為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="8073d-142">Replace the code in *Metadata.cs* with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="8073d-143">這些中繼資料類別可包含的所有驗證屬性，您先前套用至模型類別。</span><span class="sxs-lookup"><span data-stu-id="8073d-143">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="8073d-144">**顯示**屬性用來變更所使用的文字標籤的值。</span><span class="sxs-lookup"><span data-stu-id="8073d-144">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="8073d-145">現在，您必須將模型類別關聯的中繼資料類別。</span><span class="sxs-lookup"><span data-stu-id="8073d-145">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="8073d-146">在 **模型**資料夾中，加入名為類別*PartialClasses.cs*。</span><span class="sxs-lookup"><span data-stu-id="8073d-146">In the **Models** folder, add a class named *PartialClasses.cs*.</span></span>

<span data-ttu-id="8073d-147">取代下列程式碼檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="8073d-147">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="8073d-148">請注意，每個類別標示為`partial`類別，和每個符合的名稱和命名空間會自動產生的類別。</span><span class="sxs-lookup"><span data-stu-id="8073d-148">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="8073d-149">藉由套用至部分類別的中繼資料屬性，確定您的資料驗證屬性將會套用至自動產生的類別。</span><span class="sxs-lookup"><span data-stu-id="8073d-149">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="8073d-150">當您重新產生模型類別，因為中繼資料屬性會套用在不會重新產生的部分類別中，這些屬性將不會遺失。</span><span class="sxs-lookup"><span data-stu-id="8073d-150">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="8073d-151">若要重新產生的自動產生的類別，請開啟*ContosoModel.edmx*檔案。</span><span class="sxs-lookup"><span data-stu-id="8073d-151">To regenerate the automatically-generated classes, open the *ContosoModel.edmx* file.</span></span> <span data-ttu-id="8073d-152">同樣地，以滑鼠右鍵按一下 設計介面，然後選取**從資料庫更新模型**。</span><span class="sxs-lookup"><span data-stu-id="8073d-152">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="8073d-153">即使您未變更的資料庫，此程序會重新產生類別。</span><span class="sxs-lookup"><span data-stu-id="8073d-153">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="8073d-154">在 **重新整理**索引標籤上，選取**資料表**並**完成**。</span><span class="sxs-lookup"><span data-stu-id="8073d-154">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

<span data-ttu-id="8073d-155">儲存*ContosoModel.edmx*檔案，以套用所做的變更。</span><span class="sxs-lookup"><span data-stu-id="8073d-155">Save the *ContosoModel.edmx* file to apply the changes.</span></span>

<span data-ttu-id="8073d-156">開啟*Student.cs*檔案或*Enrollment.cs*檔案，並請注意，您在稍早所套用的資料驗證屬性已不在檔案中。</span><span class="sxs-lookup"><span data-stu-id="8073d-156">Open the *Student.cs* file or the *Enrollment.cs* file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="8073d-157">不過，執行應用程式，並請注意當您輸入的資料仍然會套用驗證規則。</span><span class="sxs-lookup"><span data-stu-id="8073d-157">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8073d-158">其他資源</span><span class="sxs-lookup"><span data-stu-id="8073d-158">Additional resources</span></span>

<span data-ttu-id="8073d-159">您可以套用至屬性和類別的資料驗證註解的完整清單，請參閱 < [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8073d-159">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8073d-160">後續步驟</span><span class="sxs-lookup"><span data-stu-id="8073d-160">Next steps</span></span>

<span data-ttu-id="8073d-161">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="8073d-161">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8073d-162">已新增的資料註解</span><span class="sxs-lookup"><span data-stu-id="8073d-162">Added data annotations</span></span>
> * <span data-ttu-id="8073d-163">已新增的中繼資料類別</span><span class="sxs-lookup"><span data-stu-id="8073d-163">Added metadata classes</span></span>

<span data-ttu-id="8073d-164">請前進到下一個教學課程，以了解如何將 web 應用程式和資料庫發佈至 Azure。</span><span class="sxs-lookup"><span data-stu-id="8073d-164">Advance to the next tutorial to learn how to publish the web app and database to Azure.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="8073d-165">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="8073d-165">Publish to Azure</span></span>](publish-to-azure.md)