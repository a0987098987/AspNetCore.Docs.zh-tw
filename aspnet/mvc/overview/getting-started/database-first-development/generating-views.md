---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 第一個使用 ASP.NET MVC 的 EF 資料庫： 產生檢視 |Microsoft 文件
author: tfitzmac
description: 使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程里...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: b60e89a187a879255eb051dc87241714cef6fa63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="f6fe7-104">第一個使用 ASP.NET MVC 的 EF 資料庫： 產生檢視表</span><span class="sxs-lookup"><span data-stu-id="f6fe7-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="f6fe7-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f6fe7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f6fe7-106">使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="f6fe7-107">此教學課程的數列會顯示如何自動產生程式碼，可讓使用者顯示、 編輯、 建立和刪除存在於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="f6fe7-108">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="f6fe7-109">數列的這個部分著重於使用 ASP.NET Scaffolding 產生控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="f6fe7-110">加入 scaffold</span><span class="sxs-lookup"><span data-stu-id="f6fe7-110">Add scaffold</span></span>

<span data-ttu-id="f6fe7-111">您已準備好產生程式碼將會提供標準的資料作業模型類別。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="f6fe7-112">您可以加入 scaffold 項目將程式碼。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="f6fe7-113">有許多選項，您可以加入; scaffolding 類型在本教學課程中，控制器和檢視對應至您在上一節中建立的學生和註冊模型，將包含 scaffold。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="f6fe7-114">為了維持一致性，您的專案中，您會將新的控制站加入現有**控制器**資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="f6fe7-115">以滑鼠右鍵按一下**控制器**資料夾，然後選取**新增**–**新的 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![加入 scaffold](generating-views/_static/image1.png)

<span data-ttu-id="f6fe7-117">選取**的 MVC 5 控制器與檢視，使用 Entity Framework**選項。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="f6fe7-118">此選項將會產生控制器和檢視更新、 刪除、 建立和顯示您的模型中的資料。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![新增 mvc 控制器](generating-views/_static/image2.png)

<span data-ttu-id="f6fe7-120">選取**學生**模型類別，並選取**ContosoUniversityEntities**內容類別。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="f6fe7-121">保留相同的控制器名稱**StudentsController**，</span><span class="sxs-lookup"><span data-stu-id="f6fe7-121">Keep the controller name as **StudentsController**,</span></span>

![指定控制器](generating-views/_static/image3.png)

<span data-ttu-id="f6fe7-123">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-123">Click **Add**.</span></span>

<span data-ttu-id="f6fe7-124">如果您收到錯誤，它可能是因為您未建立在上一節中的專案。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="f6fe7-125">如果是這樣，建置專案，再試一次，然後再次新增 scaffold 項目。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="f6fe7-126">程式碼產生程序完成之後，您會看到新的控制器和檢視您的專案中。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![顯示檢視](generating-views/_static/image4.png)

<span data-ttu-id="f6fe7-128">同樣地，執行相同的步驟，但是加入 scaffold 註冊類別。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="f6fe7-129">完成時，您應該有**EnrollmentsController.cs**檔和資料夾之下的**檢視**名為**註冊**以 Create、 Delete、 詳細資料、 編輯和索引檢視。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![顯示檢視](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="f6fe7-131">將連結加入至新的檢視</span><span class="sxs-lookup"><span data-stu-id="f6fe7-131">Add links to new views</span></span>

<span data-ttu-id="f6fe7-132">若要讓您更輕鬆地瀏覽至新的檢視，您可以將超連結數加入索引檢視表學生版和註冊項目。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="f6fe7-133">開啟位於檔案**Views/Home/Index.cshtml**，這是您的網站的首頁。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="f6fe7-134">新增 jumbotron 以下程式碼。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="f6fe7-135">ActionLink 方法中，第一個參數是要顯示在連結的文字。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="f6fe7-136">第二個參數是動作，而第三個參數是控制器的名稱。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="f6fe7-137">例如，第一個連結會指向 StudentsController 中的索引動作。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="f6fe7-138">實際的超連結是從這些值所建構。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="f6fe7-139">第一個連結最終會採用使用者**Index.cshtml**檔案內**檢視/學生**資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="f6fe7-140">顯示學生檢視</span><span class="sxs-lookup"><span data-stu-id="f6fe7-140">Display student views</span></span>

<span data-ttu-id="f6fe7-141">您將驗證加入至您的專案正確的程式碼顯示學生的清單，並可讓使用者編輯、 建立或刪除資料庫中的學生記錄。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="f6fe7-142">以滑鼠右鍵按一下**Views/Home/Index.cshtml**檔案，然後選取**瀏覽器中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="f6fe7-143">在此頁面上，按一下 學員清單的連結。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="f6fe7-144">這個頁面上，請注意學生與連結，以修改此資料的清單。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-144">On this page, notice the list of the students and links to modify this data.</span></span>

![學員清單](generating-views/_static/image7.png)

<span data-ttu-id="f6fe7-146">按一下**新建**連結，並提供新的學生的某些值。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-146">Click the **Create New** link and provide some values for a new student.</span></span>

![建立新的學生](generating-views/_static/image8.png)

<span data-ttu-id="f6fe7-148">按一下**建立**，並注意新的學生新增到清單。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-148">Click **Create**, and notice the new student is added to your list.</span></span>

![與新的學生的清單](generating-views/_static/image9.png)

<span data-ttu-id="f6fe7-150">選取**編輯**連結，並變更部分的值為一位學生。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![編輯學生](generating-views/_static/image10.png)

<span data-ttu-id="f6fe7-152">按一下**儲存**，並注意學生記錄已變更。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="f6fe7-153">最後，選取**刪除**連結，並確認您想要刪除記錄，方法是按一下**刪除** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![刪除學生](generating-views/_static/image11.png)

<span data-ttu-id="f6fe7-155">不需要撰寫任何程式碼，您已加入執行常見的作業資料學生表中的檢視。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="f6fe7-156">您可能已注意到欄位文字標籤為基礎之資料庫屬性 (例如**LastName**) 但不一定要在網頁上顯示。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="f6fe7-157">例如，您可能會想要標籤**姓氏**。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="f6fe7-158">稍後在本教學課程中，您會修正此顯示問題。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="f6fe7-159">顯示註冊檢視</span><span class="sxs-lookup"><span data-stu-id="f6fe7-159">Display enrollment views</span></span>

<span data-ttu-id="f6fe7-160">您的資料庫包含 Student 與註冊的資料表，與 「 課程 」 和 「 註冊資料表之間的一對多關聯性之間的一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="f6fe7-161">註冊的檢視會正確地處理這些關聯性。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="f6fe7-162">瀏覽的網站，並選取 [首頁]**的註冊項目清單**連結，然後**新建**連結。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="f6fe7-163">檢視會顯示表單，以建立新的註冊記錄。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="f6fe7-164">特別是，請注意，表單會包含兩個下拉式清單，其中會填入相關資料表中的值。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![建立註冊](generating-views/_static/image12.png)

<span data-ttu-id="f6fe7-166">此外，驗證提供的值會自動套用根據欄位的資料類型。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="f6fe7-167">等級需要數字，因此如果您嘗試以提供不相容的值，會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![驗證訊息](generating-views/_static/image13.png)

<span data-ttu-id="f6fe7-169">您已驗證自動產生的檢視，讓使用者使用的資料庫中的資料。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="f6fe7-170">在此系列的下一個教學課程，您將會更新資料庫和對應變更 web 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="f6fe7-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f6fe7-171">[上一頁](creating-the-web-application.md)
> [下一頁](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="f6fe7-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
