---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: EF Database First 與 ASP.NET MVC： 增強資料驗證 |Microsoft Docs
author: tfitzmac
description: 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本教學課程的里...
ms.author: riande
ms.date: 12/29/2014
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: dbff33a1c4f47474adda82e796d9c292392142f2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825157"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a><span data-ttu-id="725be-104">EF Database First 與 ASP.NET MVC： 增強資料驗證</span><span class="sxs-lookup"><span data-stu-id="725be-104">EF Database First with ASP.NET MVC: Enhancing Data Validation</span></span>
====================
<span data-ttu-id="725be-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="725be-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="725be-106">您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="725be-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="725be-107">本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="725be-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="725be-108">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="725be-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="725be-109">系列的這個部分會著重於將資料註解加入至資料模型，以指定驗證需求，並顯示格式設定。</span><span class="sxs-lookup"><span data-stu-id="725be-109">This part of the series focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="725be-110">它已改善根據意見反應的註解區段中的使用者。</span><span class="sxs-lookup"><span data-stu-id="725be-110">It was improved based on feedback from users in the comments section.</span></span>


## <a name="add-data-annotations"></a><span data-ttu-id="725be-111">新增資料註解</span><span class="sxs-lookup"><span data-stu-id="725be-111">Add data annotations</span></span>

<span data-ttu-id="725be-112">如您所見在先前主題中，某些資料驗證規則會自動套用到使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="725be-112">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="725be-113">比方說，您只可以提供一些級屬性。</span><span class="sxs-lookup"><span data-stu-id="725be-113">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="725be-114">若要指定更多的資料驗證規則，您可以新增至您的模型類別的資料註解。</span><span class="sxs-lookup"><span data-stu-id="725be-114">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="725be-115">這些註解會套用在您的 web 應用程式，針對指定的屬性。</span><span class="sxs-lookup"><span data-stu-id="725be-115">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="725be-116">您也可以套用變更之屬性的顯示方式; 的格式屬性例如，變更用於文字標籤的值。</span><span class="sxs-lookup"><span data-stu-id="725be-116">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="725be-117">在本教學課程中，您將加入資料註解提供的 FirstName、 LastName 和 MiddleName 屬性的值將長度限制。</span><span class="sxs-lookup"><span data-stu-id="725be-117">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="725be-118">在資料庫中，這些值僅限於 50 個字元;不過，在您的 web 應用程式中的字元限制目前不會強制執行。</span><span class="sxs-lookup"><span data-stu-id="725be-118">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="725be-119">如果使用者提供超過 50 個字元的其中一個值，當您嘗試將值儲存至資料庫時，會損毀頁面。</span><span class="sxs-lookup"><span data-stu-id="725be-119">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="725be-120">您也會限制級為 0 到 4 之間的值。</span><span class="sxs-lookup"><span data-stu-id="725be-120">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="725be-121">開啟**Student.cs**中的檔案**模型**資料夾。</span><span class="sxs-lookup"><span data-stu-id="725be-121">Open the **Student.cs** file in the **Models** folder.</span></span> <span data-ttu-id="725be-122">將下列反白顯示的程式碼新增至類別。</span><span class="sxs-lookup"><span data-stu-id="725be-122">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="725be-123">在 Enrollment.cs，加入下列反白顯示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="725be-123">In Enrollment.cs, add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="725be-124">建置方案。</span><span class="sxs-lookup"><span data-stu-id="725be-124">Build the solution.</span></span>

<span data-ttu-id="725be-125">瀏覽至 編輯或建立學生的頁面。</span><span class="sxs-lookup"><span data-stu-id="725be-125">Browse to a page for editing or creating a student.</span></span> <span data-ttu-id="725be-126">如果您嘗試輸入超過 50 個字元，則會顯示一則錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="725be-126">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![顯示錯誤訊息](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="725be-128">瀏覽至這個網頁進行編輯註冊，並嘗試提供一個等級，上述 4。</span><span class="sxs-lookup"><span data-stu-id="725be-128">Browse to the page for editing enrollments, and attempt to provide a grade above 4.</span></span>

![等級範圍錯誤](enhancing-data-validation/_static/image2.png)

<span data-ttu-id="725be-130">您可以套用至屬性和類別的資料驗證註解的完整清單，請參閱 < [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)。</span><span class="sxs-lookup"><span data-stu-id="725be-130">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="725be-131">加入中繼資料類別</span><span class="sxs-lookup"><span data-stu-id="725be-131">Add metadata classes</span></span>

<span data-ttu-id="725be-132">當您不想要變更; 的資料庫模型類別中直接加入驗證屬性運作不過，如果您的資料庫變更，而且您需要重新產生模型類別，您會遺失所有您已套用至模型類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="725be-132">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="725be-133">這種方法可以是非常沒有效率且容易會遺失重要的驗證規則。</span><span class="sxs-lookup"><span data-stu-id="725be-133">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="725be-134">若要避免這個問題，您可以新增包含屬性的中繼資料類別。</span><span class="sxs-lookup"><span data-stu-id="725be-134">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="725be-135">當您建立關聯的中繼資料類別的模型類別時，這些屬性會套用至模型中。</span><span class="sxs-lookup"><span data-stu-id="725be-135">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="725be-136">這種方法，就可以重新產生模型類別而不會遺失所有已套用至中繼資料類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="725be-136">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="725be-137">在 **模型**資料夾中，加入名為類別**Metadata.cs**。</span><span class="sxs-lookup"><span data-stu-id="725be-137">In the **Models** folder, add a class named **Metadata.cs**.</span></span>

![加入中繼資料類別](enhancing-data-validation/_static/image3.png)

<span data-ttu-id="725be-139">Metadata.cs 中的程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="725be-139">Replace the code in Metadata.cs with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="725be-140">這些中繼資料類別可包含的所有驗證屬性，您先前套用至模型類別。</span><span class="sxs-lookup"><span data-stu-id="725be-140">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="725be-141">**顯示**屬性用來變更所使用的文字標籤的值。</span><span class="sxs-lookup"><span data-stu-id="725be-141">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="725be-142">現在，您必須將模型類別關聯的中繼資料類別。</span><span class="sxs-lookup"><span data-stu-id="725be-142">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="725be-143">在 **模型**資料夾中，加入名為類別**PartialClasses.cs**。</span><span class="sxs-lookup"><span data-stu-id="725be-143">In the **Models** folder, add a class named **PartialClasses.cs**.</span></span>

<span data-ttu-id="725be-144">取代下列程式碼檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="725be-144">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="725be-145">請注意，每個類別標示為`partial`類別，和每個符合的名稱和命名空間會自動產生的類別。</span><span class="sxs-lookup"><span data-stu-id="725be-145">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="725be-146">藉由套用至部分類別的中繼資料屬性，確定您的資料驗證屬性將會套用至自動產生的類別。</span><span class="sxs-lookup"><span data-stu-id="725be-146">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="725be-147">當您重新產生模型類別，因為中繼資料屬性會套用在不會重新產生的部分類別中，這些屬性將不會遺失。</span><span class="sxs-lookup"><span data-stu-id="725be-147">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="725be-148">若要重新產生的自動產生的類別，請開啟 ContosoModel.edmx 檔案。</span><span class="sxs-lookup"><span data-stu-id="725be-148">To regenerate the automatically-generated classes, open the ContosoModel.edmx file.</span></span> <span data-ttu-id="725be-149">同樣地，以滑鼠右鍵按一下 設計介面，然後選取**從資料庫更新模型**。</span><span class="sxs-lookup"><span data-stu-id="725be-149">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="725be-150">即使您未變更的資料庫，此程序會重新產生類別。</span><span class="sxs-lookup"><span data-stu-id="725be-150">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="725be-151">在 **重新整理**索引標籤上，選取**資料表**並**完成**。</span><span class="sxs-lookup"><span data-stu-id="725be-151">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

![重新整理資料表](enhancing-data-validation/_static/image4.png)

<span data-ttu-id="725be-153">儲存 ContosoModel.edmx 檔案以套用變更。</span><span class="sxs-lookup"><span data-stu-id="725be-153">Save the ContosoModel.edmx file to apply the changes.</span></span>

<span data-ttu-id="725be-154">開啟 Student.cs 檔案或 Enrollment.cs 檔案，並請注意您在稍早所套用的資料驗證屬性已不在檔案中。</span><span class="sxs-lookup"><span data-stu-id="725be-154">Open the Student.cs file or the Enrollment.cs file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="725be-155">不過，執行應用程式，並請注意當您輸入的資料仍然會套用驗證規則。</span><span class="sxs-lookup"><span data-stu-id="725be-155">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="725be-156">[上一頁](customizing-a-view.md)
> [下一頁](publish-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="725be-156">[Previous](customizing-a-view.md)
[Next](publish-to-azure.md)</span></span>
