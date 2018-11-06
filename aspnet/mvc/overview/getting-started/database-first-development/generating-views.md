---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: EF Database First 與 ASP.NET MVC： 產生檢視 |Microsoft Docs
author: Rick-Anderson
description: 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本教學課程的里...
ms.author: riande
ms.date: 12/29/2014
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 7d925573dd4cdf5c1a36e51f312e18093bd35043
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021080"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="b1a1c-104">EF Database First 與 ASP.NET MVC： 產生檢視</span><span class="sxs-lookup"><span data-stu-id="b1a1c-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="b1a1c-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b1a1c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b1a1c-106">您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="b1a1c-107">本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="b1a1c-108">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="b1a1c-109">系列的這個部分會著重於使用 ASP.NET Scaffolding 產生控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="b1a1c-110">新增 scaffold</span><span class="sxs-lookup"><span data-stu-id="b1a1c-110">Add scaffold</span></span>

<span data-ttu-id="b1a1c-111">您已準備好產生程式碼，將會提供標準資料作業，模型類別。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="b1a1c-112">您可以加入 scaffold 項目，以將程式碼。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="b1a1c-113">有許多類型的樣板，您可以新增;在本教學課程中，控制器和檢視對應至您在上一節中建立的 Student 和註冊模型，會包含 scaffold。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="b1a1c-114">若要維護您的專案中的一致性，您會將新的控制站加入現有**控制器**資料夾。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="b1a1c-115">以滑鼠右鍵按一下**控制器**資料夾，然後選取**新增**–**新增 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![新增 scaffold](generating-views/_static/image1.png)

<span data-ttu-id="b1a1c-117">選取  **MVC 5 控制器與檢視，使用 Entity Framework**選項。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="b1a1c-118">此選項會產生控制器和檢視更新、 刪除、 建立和顯示您的模型中的資料。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![新增 mvc 控制器](generating-views/_static/image2.png)

<span data-ttu-id="b1a1c-120">選取 [**學生**模型類別]，然後選取**ContosoUniversityEntities**內容類別。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="b1a1c-121">保留做為控制器名稱**StudentsController**，</span><span class="sxs-lookup"><span data-stu-id="b1a1c-121">Keep the controller name as **StudentsController**,</span></span>

![指定控制器](generating-views/_static/image3.png)

<span data-ttu-id="b1a1c-123">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-123">Click **Add**.</span></span>

<span data-ttu-id="b1a1c-124">如果您收到錯誤，可能是因為您並未建置在上一節中的專案。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="b1a1c-125">如果是的話，請嘗試建置專案，，然後再次新增 scaffold 項目。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="b1a1c-126">程式碼產生程序完成之後，您會看到新的控制器和檢視專案中。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![顯示檢視](generating-views/_static/image4.png)

<span data-ttu-id="b1a1c-128">同樣地，執行相同的步驟，但是加入 scaffold，以註冊類別。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="b1a1c-129">完成後，您應該有**EnrollmentsController.cs**檔案和資料夾之下**檢視**名為**註冊**以 Create、 Delete、 詳細資料、 編輯和索引檢視。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![顯示檢視](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="b1a1c-131">將連結新增至新的檢視</span><span class="sxs-lookup"><span data-stu-id="b1a1c-131">Add links to new views</span></span>

<span data-ttu-id="b1a1c-132">為了讓您更輕鬆地瀏覽至您新的檢視，您可以為學生和註冊幾個超連結新增至索引檢視。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="b1a1c-133">開啟的檔案**Views/Home/Index.cshtml**，這是您網站的 [首頁] 頁面。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="b1a1c-134">下列程式碼下方新增 jumbotron。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="b1a1c-135">ActionLink 方法中，第一個參數是要顯示在連結的文字。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="b1a1c-136">第二個參數是動作，第三個參數是控制器的名稱。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="b1a1c-137">比方說，第一個連結會指向 StudentsController 中的索引動作。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="b1a1c-138">實際的超連結會從這些值來建構。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="b1a1c-139">最終的第一個連結會帶領使用者前往**Index.cshtml**檔案內**檢視/學生**資料夾。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="b1a1c-140">顯示學生檢視</span><span class="sxs-lookup"><span data-stu-id="b1a1c-140">Display student views</span></span>

<span data-ttu-id="b1a1c-141">您將驗證新增至您的專案正確的程式碼顯示的學生，清單，並可讓使用者編輯、 建立或刪除資料庫中的學生記錄。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="b1a1c-142">以滑鼠右鍵按一下**Views/Home/Index.cshtml**檔案，然後選取**瀏覽器中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="b1a1c-143">在此頁面上，按一下連結的學生清單。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="b1a1c-144">在此頁面上，注意到的學生與連結，以修改此資料的清單。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-144">On this page, notice the list of the students and links to modify this data.</span></span>

![學生的清單](generating-views/_static/image7.png)

<span data-ttu-id="b1a1c-146">按一下 **新建**連結，並提供一些值給新的學生。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-146">Click the **Create New** link and provide some values for a new student.</span></span>

![建立新的學生](generating-views/_static/image8.png)

<span data-ttu-id="b1a1c-148">按一下 **建立**，並注意新的學生新增至您的清單。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-148">Click **Create**, and notice the new student is added to your list.</span></span>

![使用新的學生清單](generating-views/_static/image9.png)

<span data-ttu-id="b1a1c-150">選取 **編輯**連結，然後變更的某些值的學生。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![編輯學生](generating-views/_static/image10.png)

<span data-ttu-id="b1a1c-152">按一下 **儲存**，並注意學生資料錄已變更。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="b1a1c-153">最後，選取**刪除**連結，並確認您想要刪除此記錄，依序按一下**刪除** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![刪除學生](generating-views/_static/image11.png)

<span data-ttu-id="b1a1c-155">不需要撰寫任何程式碼，您已新增一般對資料執行作業的 Student 資料表中的檢視。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="b1a1c-156">您可能已經注意到，欄位的文字標籤為基礎之資料庫屬性 (例如**LastName**) 不一定是您想要顯示在網頁上。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="b1a1c-157">比方說，您可能會想要的標籤**姓氏**。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="b1a1c-158">稍後在本教學課程中，您將會修正這個顯示問題。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="b1a1c-159">顯示註冊檢視</span><span class="sxs-lookup"><span data-stu-id="b1a1c-159">Display enrollment views</span></span>

<span data-ttu-id="b1a1c-160">您的資料庫都包含學生和註冊的資料表，與 Course 與 Enrollment 資料表之間的一對多關聯性之間的一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="b1a1c-161">註冊的檢視正確處理這些關聯性。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="b1a1c-162">瀏覽至您的網站，並選取首頁**註冊清單**連結，然後**建立新**連結。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="b1a1c-163">檢視會顯示用來建立新的註冊記錄的表單。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="b1a1c-164">特別是，請注意，表單包含兩個相關資料表中的值會填入的下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![建立註冊](generating-views/_static/image12.png)

<span data-ttu-id="b1a1c-166">此外，驗證提供的值會自動套用根據欄位的資料類型。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="b1a1c-167">級需要的數字，因此如果您嘗試使用不相容的價值，就會顯示一則錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![驗證訊息](generating-views/_static/image13.png)

<span data-ttu-id="b1a1c-169">您已驗證的自動產生的檢視，可讓使用者可以使用資料庫中的資料。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="b1a1c-170">在本系列中下一個教學課程中，您會更新資料庫，並在 web 應用程式中進行對應的變更。</span><span class="sxs-lookup"><span data-stu-id="b1a1c-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b1a1c-171">[上一頁](creating-the-web-application.md)
> [下一頁](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="b1a1c-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
