---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework Scaffold 和移轉 |Microsoft Docs
author: rick-anderson
description: 如果您已熟悉 ASP.NET MVC 4 控制器方法，或已完成&quot;Helper、 表單和驗證&quot;實際操作實驗室中，您應該注意...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 31f593f294c4865f621a8556cb43d0d9c42f2660
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814114"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="c5c0b-103">ASP.NET MVC 4 Entity Framework Scaffold 和移轉</span><span class="sxs-lookup"><span data-stu-id="c5c0b-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="c5c0b-104">藉由[Web Camp 小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c5c0b-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="c5c0b-105">下載 Web 研討會訓練套件</span><span class="sxs-lookup"><span data-stu-id="c5c0b-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="c5c0b-106">如果您已熟悉 ASP.NET MVC 4 控制器方法，或已完成&quot;Helper、 表單和驗證&quot;實際操作實驗室中，您應該注意許多邏輯，以建立、 更新、 列出及移除它會重複任何資料實體在應用程式。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="c5c0b-107">更何況，如果您的模型有幾個類別，操作，您將有可能需要花費相當長的時間撰寫每個實體作業，以及每個檢視的 POST 與 GET 動作方法。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="c5c0b-108">在這個實驗室中，您將學習如何使用 ASP.NET MVC 4 scaffolding 自動產生應用程式的 CRUD （建立、 讀取、 更新和刪除） 的基準線。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="c5c0b-109">從開始從簡單的模型類別，並不需要撰寫一行程式碼，您將建立一個控制站，會包含所有 CRUD 作業，以及所有必要的檢視。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="c5c0b-110">在建置及執行簡單的解決方案之後，您必須產生的 MVC 邏輯和檢視資料操作的應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="c5c0b-111">此外，您將學習使用 Entity Framework 移轉至執行在整個應用程式中的模型更新是多麼容易。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="c5c0b-112">Entity Framework 移轉可讓您的模型已變更利用簡單的步驟之後，請修改您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="c5c0b-113">所有這些考量，您將能夠建置及維護 web 應用程式更有效率地利用 ASP.NET MVC 4 的最新功能。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="c5c0b-114">Web 研討會訓練套件中，在可從包含所有的範例程式碼和程式碼片段[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="c5c0b-115">這個實驗室中的特定專案將會位於[ASP.NET MVC 4 Entity Framework Scaffold 和移轉](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations)。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="c5c0b-116">目標</span><span class="sxs-lookup"><span data-stu-id="c5c0b-116">Objectives</span></span>

<span data-ttu-id="c5c0b-117">在這個實際操作實驗室中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="c5c0b-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="c5c0b-118">使用 ASP.NET scaffolding 的控制器中的 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="c5c0b-119">將使用 Entity Framework 移轉的資料庫模型的變更。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c5c0b-120">必要條件</span><span class="sxs-lookup"><span data-stu-id="c5c0b-120">Prerequisites</span></span>

<span data-ttu-id="c5c0b-121">您必須具備下列項目，即可完成此實驗室：</span><span class="sxs-lookup"><span data-stu-id="c5c0b-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="c5c0b-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更好 (讀取[附錄 A](#AppendixA)如需有關如何安裝它)。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="c5c0b-123">安裝程式</span><span class="sxs-lookup"><span data-stu-id="c5c0b-123">Setup</span></span>

<span data-ttu-id="c5c0b-124">**安裝程式碼片段**</span><span class="sxs-lookup"><span data-stu-id="c5c0b-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="c5c0b-125">為了方便起見，大部分的程式碼，您將沿著這個實驗室管理可從 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="c5c0b-126">若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="c5c0b-127">如果您不熟悉 Visual Studio 程式碼片段，而且想要了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 b： 使用程式碼片段](#AppendixB)&quot;。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c5c0b-128">練習</span><span class="sxs-lookup"><span data-stu-id="c5c0b-128">Exercises</span></span>

<span data-ttu-id="c5c0b-129">下列練習中，組成此實際操作實驗室：</span><span class="sxs-lookup"><span data-stu-id="c5c0b-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="c5c0b-130">使用 ASP.NET MVC 4 Scaffolding，Entity Framework 移轉與</span><span class="sxs-lookup"><span data-stu-id="c5c0b-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="c5c0b-131">此練習會伴隨**結束**包含完成練習之後，您應該取得所產生的方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="c5c0b-132">如果您需要其他說明逐步練習，您可以使用此解決方案作為指南。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="c5c0b-133">估計的時間才能完成這個實驗室： **30 分鐘**</span><span class="sxs-lookup"><span data-stu-id="c5c0b-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="c5c0b-134">練習 1： 使用 ASP.NET MVC 4 Scaffolding，Entity Framework 移轉與</span><span class="sxs-lookup"><span data-stu-id="c5c0b-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="c5c0b-135">ASP.NET MVC scaffolding 提供快速的方式，以產生標準化的方式，建立所需的邏輯，可讓您的應用程式與資料庫層級互動的 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="c5c0b-136">在此練習中，您將學習如何使用 ASP.NET MVC 4 的 scaffold 程式碼第一次建立 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="c5c0b-137">然後，您將了解如何更新您的模型來使用 Entity Framework 移轉套用在資料庫中的變更。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="c5c0b-138">工作 1-建立新的 ASP.NET MVC 4 專案使用 Scaffolding</span><span class="sxs-lookup"><span data-stu-id="c5c0b-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="c5c0b-139">如果尚未開啟，啟動**Visual Studio 2012**。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="c5c0b-140">選取**檔案 |新的專案**。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-140">Select **File | New Project**.</span></span> <span data-ttu-id="c5c0b-141">在 [新專案] 對話方塊中，在**Visual C# |Web**區段中，選取**ASP.NET MVC 4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="c5c0b-142">若要將專案命名**MVC4andEFMigrations**並將位置設定為**Source\Ex1 UsingMVC4ScaffoldingEFMigrations**本實驗室的資料夾。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="c5c0b-143">設定**方案名稱**要**開始**，並確保**為方案建立目錄**已核取。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="c5c0b-144">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-144">Click **OK**.</span></span>

    <span data-ttu-id="c5c0b-145">![新 ASP.NET MVC 4 專案 對話方塊](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "新 ASP.NET MVC 4 專案 對話方塊")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="c5c0b-146">*新 ASP.NET MVC 4 專案 對話方塊*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="c5c0b-147">在 [**新的 ASP.NET MVC 4 專案**] 對話方塊中選取**網際網路應用程式**範本，並確定**Razor**是選取**檢視引擎**.</span><span class="sxs-lookup"><span data-stu-id="c5c0b-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="c5c0b-148">按一下 [確定] 建立專案。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="c5c0b-149">![新的 ASP.NET MVC 4 網際網路應用程式](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "新的 ASP.NET MVC 4 網際網路應用程式")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="c5c0b-150">*新的 ASP.NET MVC 4 網際網路應用程式*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="c5c0b-151">在 [方案總管] 中，以滑鼠右鍵按一下**模型**，然後選取**新增 |類別**建立簡單的類別，個人 (POCO)。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="c5c0b-152">命名**Person** ，按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="c5c0b-153">開啟 Person 類別，並插入下列屬性。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="c5c0b-154">(程式碼片段- *ASP.NET MVC 4 和 Entity Framework 移轉 Ex1 人員屬性*)</span><span class="sxs-lookup"><span data-stu-id="c5c0b-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="c5c0b-155">按一下 **建置 |建置解決方案**儲存所做的變更，並建置專案。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="c5c0b-156">![建置應用程式](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "建置應用程式")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="c5c0b-157">*建置應用程式*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-157">*Building the Application*</span></span>
7. <span data-ttu-id="c5c0b-158">在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾，然後選取**新增 |控制器**。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="c5c0b-159">控制器*PersonController*並完成**Scaffolding 選項**具有下列值。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="c5c0b-160">在 **樣板**下拉式清單中，選取**讀取/寫入動作和檢視、 使用 Entity Framework 的 MVC 控制器**選項。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="c5c0b-161">在 **模型類別**下拉式清單中，選取**人員**類別。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="c5c0b-162">在 **的資料內容類別**清單中，選取**&lt;新資料內容...&gt;**.</span><span class="sxs-lookup"><span data-stu-id="c5c0b-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="c5c0b-163">選擇任何名稱，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="c5c0b-164">在 **檢視**下拉式清單中，請確定**Razor**已選取。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="c5c0b-165">![新增人員控制站，使用 scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "新增 scaffolding 人員控制器")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="c5c0b-166">*新增 scaffolding 人員控制器*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="c5c0b-167">按一下 **新增**使用 scaffolding 建立人員的新控制器。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="c5c0b-168">您現在已經產生控制器動作，以及檢視。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="c5c0b-169">![使用 scaffolding 建立人員控制站之後](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "之後使用 scaffolding 建立人員控制站")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="c5c0b-170">*使用 scaffolding 建立人員控制站之後*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="c5c0b-171">開啟**PersonController**類別。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-171">Open **PersonController** class.</span></span> <span data-ttu-id="c5c0b-172">請注意有自動產生完整的 CRUD 動作方法。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="c5c0b-173">![內部人員控制器](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "內部人員控制站")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="c5c0b-174">*內部人員控制站*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="c5c0b-175">工作 2-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="c5c0b-175">Task 2- Running the application</span></span>

<span data-ttu-id="c5c0b-176">此時，資料庫尚未建立。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="c5c0b-177">在這個工作中，您將會執行第一次應用程式，並測試的 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="c5c0b-178">將使用 Code First 立即建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="c5c0b-179">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="c5c0b-180">在瀏覽器中，加入 **/Person**至 URL，以開啟 [人員] 頁面。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="c5c0b-181">![第一次執行的應用程式](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "第一次執行的應用程式")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="c5c0b-182">*第一次執行應用程式：*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-182">*Application: first run*</span></span>
3. <span data-ttu-id="c5c0b-183">現在，您將探索人員頁面，以及測試的 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="c5c0b-184">按一下 **新建**來新增新的人員。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="c5c0b-185">輸入的名字和姓氏，然後按一下**建立**。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="c5c0b-186">![加入新的個人](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "新增新的人員")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="c5c0b-187">*加入新的人員*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="c5c0b-188">在連絡人的清單中，您可以刪除、 編輯或新增項目。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="c5c0b-189">![人員清單](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "人員清單")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="c5c0b-190">*人員清單*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-190">*Person list*</span></span>
    3. <span data-ttu-id="c5c0b-191">按一下 **詳細資料**開啟連絡人的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="c5c0b-192">![人員詳細資料](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "人員詳細資料")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="c5c0b-193">*人員詳細資料*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-193">*Person's details*</span></span>
4. <span data-ttu-id="c5c0b-194">關閉瀏覽器，並返回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="c5c0b-195">請注意您已建立完整的 CRUD 員 實體的整個應用程式-從檢視模型-而不需要撰寫一行程式碼 ！</span><span class="sxs-lookup"><span data-stu-id="c5c0b-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="c5c0b-196">工作 3-更新使用 Entity Framework 移轉資料庫</span><span class="sxs-lookup"><span data-stu-id="c5c0b-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="c5c0b-197">在這項工作中，您將會更新使用 Entity Framework 移轉的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="c5c0b-198">您會發現若要變更模型，並使用 Entity Framework 移轉功能，以反映您的資料庫中的變更是多麼容易。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="c5c0b-199">開啟 [Package Manager Console]。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-199">Open the Package Manager Console.</span></span> <span data-ttu-id="c5c0b-200">選取**工具 |程式庫套件管理員 |套件管理員主控台**。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="c5c0b-201">在套件管理員主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="c5c0b-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="c5c0b-202">PMC</span><span class="sxs-lookup"><span data-stu-id="c5c0b-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="c5c0b-203">![啟用移轉](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "啟用移轉")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="c5c0b-204">*啟用移轉*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-204">*Enabling migrations*</span></span>

    <span data-ttu-id="c5c0b-205">啟用移轉命令會建立**移轉**資料夾，其中包含指令碼來初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="c5c0b-206">![Migrations 資料夾](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations 資料夾")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="c5c0b-207">*Migrations 資料夾*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-207">*Migrations folder*</span></span>
3. <span data-ttu-id="c5c0b-208">開啟**Configuration.cs**移轉資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="c5c0b-209">尋找類別建構函式，並變更**AutomaticMigrationsEnabled**值 *，則為 true*。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="c5c0b-210">開啟 Person 類別並新增連絡人的中間名的屬性。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="c5c0b-211">利用此新的屬性，您要變更模型。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="c5c0b-212">選取**建置 |建置解決方案**在建置應用程式 功能表上。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="c5c0b-213">![建置應用程式](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "建置應用程式")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="c5c0b-214">*建置應用程式*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-214">*Building the application*</span></span>
6. <span data-ttu-id="c5c0b-215">在套件管理員主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="c5c0b-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="c5c0b-216">PMC</span><span class="sxs-lookup"><span data-stu-id="c5c0b-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="c5c0b-217">此命令會尋找資料物件中的變更，然後，它將據此修改資料庫所需的命令。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="c5c0b-218">![新增中間名](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "新增中間名")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="c5c0b-219">*新增中間名*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="c5c0b-220">（選擇性）您可以執行下列命令來產生 SQL 指令碼的差異更新。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="c5c0b-221">這可讓您以手動方式更新資料庫 （在此情況下不需要），或套用其他資料庫中的變更：</span><span class="sxs-lookup"><span data-stu-id="c5c0b-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="c5c0b-222">PMC</span><span class="sxs-lookup"><span data-stu-id="c5c0b-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="c5c0b-223">![產生 SQL 指令碼](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "產生 SQL 指令碼")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="c5c0b-224">*產生 SQL 指令碼*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="c5c0b-225">![SQL 指令碼更新](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL 指令碼更新")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="c5c0b-226">*SQL 指令碼更新*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-226">*SQL Script update*</span></span>
8. <span data-ttu-id="c5c0b-227">在 [套件管理員] 主控台中，輸入下列命令以更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="c5c0b-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="c5c0b-228">PMC</span><span class="sxs-lookup"><span data-stu-id="c5c0b-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="c5c0b-229">![更新資料庫](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "更新資料庫")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="c5c0b-230">*更新資料庫*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-230">*Updating the Database*</span></span>

    <span data-ttu-id="c5c0b-231">這會新增**MiddleName**中的資料行**人**表格以比對目前的定義**人員**類別。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="c5c0b-232">資料庫更新之後，以滑鼠右鍵按一下 [控制器] 資料夾，然後選取**新增 |控制器**加入人員控制器一次 （使用相同的值完成）。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="c5c0b-233">這會更新現有的方法和檢視表加入新的屬性。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="c5c0b-234">![新增控制器更新](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "新增控制器更新")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="c5c0b-235">*更新控制器*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-235">*Updating the controller*</span></span>
10. <span data-ttu-id="c5c0b-236">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-236">Click **Add**.</span></span> <span data-ttu-id="c5c0b-237">然後，選取 值**覆寫 PersonController.cs**而**覆寫相關聯的檢視**，按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![新增控制器覆寫](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="c5c0b-239">*更新控制器*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="c5c0b-240">Task4-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="c5c0b-240">Task4- Running the application</span></span>

1. <span data-ttu-id="c5c0b-241">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="c5c0b-242">開啟 **/Person**。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-242">Open **/Person**.</span></span> <span data-ttu-id="c5c0b-243">請注意，已保留資料，而中間名資料行加入。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="c5c0b-244">![新增的中間名](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "新增的中間名")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="c5c0b-245">*新增的中間名*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-245">*Middle Name added*</span></span>
3. <span data-ttu-id="c5c0b-246">如果您按一下**編輯**，您將能夠加入目前使用者的中間名。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="c5c0b-247">![中間名 edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "中間名版本")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c5c0b-248">總結</span><span class="sxs-lookup"><span data-stu-id="c5c0b-248">Summary</span></span>

<span data-ttu-id="c5c0b-249">在這個實際操作實驗室中，您已了解簡單的步驟來建立與使用任何模型類別的 ASP.NET MVC 4 Scaffolding 的 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="c5c0b-250">然後，您已了解如何利用 Entity Framework 移轉您的應用程式-從檢視的資料庫-執行端對端更新。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c5c0b-251">附錄 a： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="c5c0b-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c5c0b-252">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="c5c0b-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c5c0b-253">下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c5c0b-254">移至[ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c5c0b-255">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Windows Azure SDK</em>&quot;。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="c5c0b-256">按一下 **立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-256">Click on **Install Now**.</span></span> <span data-ttu-id="c5c0b-257">如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c5c0b-258">一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c5c0b-259">![安裝 Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="c5c0b-260">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="c5c0b-261">閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="c5c0b-263">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="c5c0b-264">等候完成的下載與安裝程序。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-264">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="c5c0b-266">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-266">*Installation progress*</span></span>
6. <span data-ttu-id="c5c0b-267">安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-267">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="c5c0b-269">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-269">*Installation completed*</span></span>
7. <span data-ttu-id="c5c0b-270">按一下 **結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c5c0b-271">若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 圖格](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="c5c0b-273">*VS Express for Web 圖格*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="c5c0b-274">附錄 b： 使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="c5c0b-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="c5c0b-275">使用程式碼片段，您會有您需要在隨手可得的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="c5c0b-276">實驗室課程文件會告訴您完全時您可以使用它們，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="c5c0b-277">![若要將程式碼插入您的專案使用 Visual Studio 程式碼片段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "程式碼插入您的專案使用 Visual Studio 程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="c5c0b-278">*若要將程式碼插入您的專案使用 Visual Studio 程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="c5c0b-279">***若要新增的程式碼片段，使用鍵盤 （僅限 C#)***</span><span class="sxs-lookup"><span data-stu-id="c5c0b-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="c5c0b-280">將游標放在您想要插入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="c5c0b-281">開始輸入程式碼片段名稱 （不含空格或連字號）。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="c5c0b-282">觀看為 IntelliSense 會顯示相符的程式碼片段的名稱。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="c5c0b-283">選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="c5c0b-284">按下 Tab 鍵兩次的游標位置插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="c5c0b-285">![開始鍵入程式碼片段名稱](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "開始輸入程式碼片段名稱")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="c5c0b-286">*開始鍵入程式碼片段名稱*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="c5c0b-287">![按 Tab 鍵以選取反白顯示的程式碼片段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "按 Tab 鍵以選取反白顯示的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="c5c0b-288">*按 Tab 鍵以選取反白顯示的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="c5c0b-289">![再次按 Tab 鍵和程式碼片段會依序展開](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "再次按 Tab 鍵和程式碼片段會展開")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="c5c0b-290">*再次按 Tab 鍵和程式碼片段會展開*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="c5c0b-291">***若要新增的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="c5c0b-292">以滑鼠右鍵按一下您要插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="c5c0b-293">選取 **插入程式碼片段**後面**My Code Snippets**。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="c5c0b-294">選擇相關的程式片段，從清單中，對它按一下。</span><span class="sxs-lookup"><span data-stu-id="c5c0b-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="c5c0b-295">![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="c5c0b-296">*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="c5c0b-297">![對它按一下挑選清單中，相關程式碼片段](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "對它按一下挑選清單中，相關程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="c5c0b-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="c5c0b-298">*對它按一下挑選清單中，相關程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="c5c0b-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
