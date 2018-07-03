---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 協助程式、 表單和驗證 |Microsoft Docs
author: rick-anderson
description: 在 ASP.NET MVC 4 模型和資料存取實際操作實驗室中，您已載入並顯示資料庫中的資料。 在這個實際操作實驗室中，您會將加入...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: f2eb624e72d6f52d1694b5753ee2b1f8117c2851
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376733"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="3e6f4-104">ASP.NET MVC 4 協助程式、 表單和驗證</span><span class="sxs-lookup"><span data-stu-id="3e6f4-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="3e6f4-105">藉由[Web Camp 小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="3e6f4-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="3e6f4-106">下載 Web 研討會訓練套件</span><span class="sxs-lookup"><span data-stu-id="3e6f4-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="3e6f4-107">在  **ASP.NET MVC 4 模型和資料存取**實際操作實驗室中，您已載入和顯示資料庫中的資料。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="3e6f4-108">在這個實際操作實驗室中，您將加入至**音樂市集**應用程式編輯該資料的能力。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="3e6f4-109">記住這個目標，您會先建立將支援建立、 讀取、 更新和刪除 (CRUD) 動作的相簿的控制站。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="3e6f4-110">您將會產生利用 ASP.NET MVC 樣板功能，以顯示 HTML 表格中的專輯屬性的索引檢視範本。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="3e6f4-111">若要增強該檢視，您將加入將截斷的詳細描述的自訂 HTML helper。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="3e6f4-112">之後，您將加入的編輯和建立的檢視表，可讓您改變資料庫，藉助表單元素，例如下拉式清單中的專輯。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="3e6f4-113">最後，您會讓使用者刪除 album，也會讓使用者無法輸入錯誤的資料來驗證他們的意見。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="3e6f4-114">這個實際操作實驗室會假設您有的基本知識**ASP.NET MVC**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="3e6f4-115">如果您未曾使用**ASP.NET MVC**之前，我們建議您將逐一**ASP.NET MVC 基礎**實際操作實驗室。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="3e6f4-116">這個實驗室會引導您完成先前所述將微幅的變更套用到來源資料夾中提供的範例 Web 應用程式的新功能與增強功能。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="3e6f4-117">所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="3e6f4-118">這個實驗室中的特定專案將會位於[ASP.NET MVC 4 Helper、 表單和驗證](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation)。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="3e6f4-119">目標</span><span class="sxs-lookup"><span data-stu-id="3e6f4-119">Objectives</span></span>

<span data-ttu-id="3e6f4-120">在這個實際操作實驗室中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="3e6f4-121">建立控制器能夠支援 CRUD 作業</span><span class="sxs-lookup"><span data-stu-id="3e6f4-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="3e6f4-122">產生 HTML 表格中顯示實體屬性的索引 檢視</span><span class="sxs-lookup"><span data-stu-id="3e6f4-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="3e6f4-123">新增自訂的 HTML 協助程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="3e6f4-124">建立和自訂 [編輯] 檢視</span><span class="sxs-lookup"><span data-stu-id="3e6f4-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="3e6f4-125">區分，以回應 HTTP-GET 或 HTTP-POST 呼叫動作方法</span><span class="sxs-lookup"><span data-stu-id="3e6f4-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="3e6f4-126">新增並自訂 Create View</span><span class="sxs-lookup"><span data-stu-id="3e6f4-126">Add and customize a Create View</span></span>
- <span data-ttu-id="3e6f4-127">處理刪除的實體</span><span class="sxs-lookup"><span data-stu-id="3e6f4-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="3e6f4-128">驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="3e6f4-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="3e6f4-129">必要條件</span><span class="sxs-lookup"><span data-stu-id="3e6f4-129">Prerequisites</span></span>

<span data-ttu-id="3e6f4-130">您必須具備下列項目，即可完成此實驗室：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="3e6f4-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更好 (讀取[附錄 A](#AppendixA)如需有關如何安裝它)。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="3e6f4-132">安裝程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-132">Setup</span></span>

<span data-ttu-id="3e6f4-133">**安裝程式碼片段**</span><span class="sxs-lookup"><span data-stu-id="3e6f4-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="3e6f4-134">為了方便起見，大部分的程式碼，您將沿著這個實驗室管理可從 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="3e6f4-135">若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="3e6f4-136">如果您不熟悉 Visual Studio 程式碼片段，而且想要了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 b： 使用程式碼片段](#AppendixB)&quot;。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="3e6f4-137">練習</span><span class="sxs-lookup"><span data-stu-id="3e6f4-137">Exercises</span></span>

<span data-ttu-id="3e6f4-138">下列練習，組成此實際操作實驗室：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="3e6f4-139">建立存放區管理員控制站和其索引檢視</span><span class="sxs-lookup"><span data-stu-id="3e6f4-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="3e6f4-140">新增 HTML 協助程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-140">Adding an HTML Helper</span></span>](#Exercise2)
3. <span data-ttu-id="3e6f4-141">[建立 [編輯] 檢視](#Exercise3)</span><span class="sxs-lookup"><span data-stu-id="3e6f4-141">[Creating the Edit View](#Exercise3)</span></span>
4. [<span data-ttu-id="3e6f4-142">加入建立檢視</span><span class="sxs-lookup"><span data-stu-id="3e6f4-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="3e6f4-143">處理刪除</span><span class="sxs-lookup"><span data-stu-id="3e6f4-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="3e6f4-144">新增驗證</span><span class="sxs-lookup"><span data-stu-id="3e6f4-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="3e6f4-145">使用不顯眼的 jQuery 用戶端</span><span class="sxs-lookup"><span data-stu-id="3e6f4-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="3e6f4-146">每個練習會伴隨**結束**包含完成練習之後，您應該取得所產生的方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="3e6f4-147">如果您需要的所有練習所使用的其他說明，您可以使用此解決方案作為指南。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="3e6f4-148">估計的時間才能完成這個實驗室： **60 分鐘**</span><span class="sxs-lookup"><span data-stu-id="3e6f4-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="3e6f4-149">練習 1： 建立存放區管理員控制站和其索引檢視</span><span class="sxs-lookup"><span data-stu-id="3e6f4-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="3e6f4-150">在此練習中，您將了解如何建立新的控制站，以支援 CRUD 作業，來自訂要從資料庫最後產生的索引檢視範本，利用 ASP.NET MVC scaffolding 傳回一份專輯其索引動作方法顯示 HTML 表格中的專輯屬性的功能。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="3e6f4-151">工作 1-建立 StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="3e6f4-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="3e6f4-152">在這個工作中，您將建立新的控制器，稱為**StoreManagerController**支援 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="3e6f4-153">開啟**開始**解決方案位於**來源/Ex1-CreatingTheStoreManagerController/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="3e6f4-154">您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3e6f4-155">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="3e6f4-156">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="3e6f4-157">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="3e6f4-158">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3e6f4-159">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3e6f4-160">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="3e6f4-161">加入新的控制器。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-161">Add a new controller.</span></span> <span data-ttu-id="3e6f4-162">若要這樣做，請以滑鼠右鍵按一下**控制器**在 [方案總管] 中選取的資料夾**新增**，然後**控制器**命令。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="3e6f4-163">變更**控制器****名稱**來**StoreManagerController** ，並確定選擇**MVC 控制器具有空白讀取/寫入動作**已選取。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="3e6f4-164">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-164">Click **Add**.</span></span>

    <span data-ttu-id="3e6f4-165">![新增控制器 對話方塊](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "新增控制器 對話方塊")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="3e6f4-166">*新增控制器 對話方塊*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="3e6f4-167">產生新的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-167">A new Controller class is generated.</span></span> <span data-ttu-id="3e6f4-168">因為您指定要為讀取/寫入，虛設常式方法，新增動作一般的 CRUD 動作會建立 TODO 註解中，填滿提示以包含應用程式特定邏輯。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="3e6f4-169">工作 2-自訂 StoreManager 索引</span><span class="sxs-lookup"><span data-stu-id="3e6f4-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="3e6f4-170">在這個工作中，您將自訂 StoreManager 索引動作方法，從資料庫傳回專輯的清單檢視。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="3e6f4-171">在 StoreManagerController 類別中，新增下列*使用*指示詞。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="3e6f4-172">(程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex1 使用 MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="3e6f4-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="3e6f4-173">將欄位加入**StoreManagerController**保存的執行個體**MusicStoreEntities。**</span><span class="sxs-lookup"><span data-stu-id="3e6f4-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="3e6f4-174">(程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="3e6f4-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="3e6f4-175">實作 StoreManagerController 索引動作傳回專輯的清單檢視。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="3e6f4-176">控制器動作邏輯會非常類似於先前撰寫的 StoreController Index 動作。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="3e6f4-177">使用 LINQ 來擷取所有的專輯，包括顯示的內容類型與演出者資訊。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="3e6f4-178">(程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex1 StoreManagerController 索引*)</span><span class="sxs-lookup"><span data-stu-id="3e6f4-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="3e6f4-179">工作 3-建立索引檢視</span><span class="sxs-lookup"><span data-stu-id="3e6f4-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="3e6f4-180">在這個工作中，您將建立索引檢視範本，以顯示所傳回的相簿**StoreManager**控制站。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="3e6f4-181">然後再建立新的檢視範本，您應該建置專案，讓**新增檢視 對話方塊**知道**專輯**類別使用。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="3e6f4-182">選取**建置 |建置 MvcMusicStore**來建置專案。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="3e6f4-183">以滑鼠右鍵按一下**Index**動作方法，然後選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="3e6f4-184">此時會出現**加入檢視**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="3e6f4-185">![新增檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "新增檢視")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="3e6f4-186">*加入從索引方法內的檢視*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="3e6f4-187">在 [新增檢視] 對話方塊中，確認檢視表名稱是否**Index**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="3e6f4-188">選取 **建立強型別檢視**選項，然後選取**專輯 (MvcMusicStore.Models)** 從**模型類別**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="3e6f4-189">選取 **清單**從**Scaffold 樣板**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="3e6f4-190">離開**檢視引擎**要**Razor**和其他欄位具有其預設值，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="3e6f4-191">![加入索引檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "加入索引檢視")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="3e6f4-192">*加入索引檢視*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="3e6f4-193">工作 4-自訂索引 檢視的 scaffold</span><span class="sxs-lookup"><span data-stu-id="3e6f4-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="3e6f4-194">在這個工作中，您將調整使用 ASP.NET MVC 樣板功能，讓它顯示您想要的欄位建立簡單的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="3e6f4-195">**Scaffolding**支援 ASP.NET MVC 內產生簡單的檢視範本會列出專輯模型中的所有欄位。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="3e6f4-196">**Scaffolding**提供快速的方式，若要開始使用強型別檢視:，而不必手動撰寫檢視範本快速 scaffolding 會產生一個預設範本，然後您可以修改產生的程式碼。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="3e6f4-197">檢閱建立的程式碼。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-197">Review the code created.</span></span> <span data-ttu-id="3e6f4-198">產生的欄位清單將會屬於下列 HTML 資料表，其中**Scaffolding**用來顯示表格式資料。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="3e6f4-199">取代**&lt;表格&gt;** 只顯示為下列程式碼的程式碼**Genre**，**演出者**，**專輯標題**，並**價格**欄位。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="3e6f4-200">這會刪除**AlbumId**並**專輯封面 URL**資料行。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="3e6f4-201">此外，它會變更 GenreId 和 ArtistId 資料行以顯示其連結的類別屬性**Artist.Name**並**Genre.Name**，並移除**詳細資料**連結。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="3e6f4-202">變更下列的描述。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="3e6f4-203">工作 5-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="3e6f4-204">在這個工作中，您將測試所**StoreManager** **Index**檢視範本會顯示一份根據先前的步驟設計的相簿。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="3e6f4-205">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3e6f4-206">專案一開始在首頁上。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-206">The project starts in the Home page.</span></span> <span data-ttu-id="3e6f4-207">將 URL 變更為 **/StoreManager**若要確認已顯示一份專輯，顯示其**標題**，**演出者**並**Genre**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="3e6f4-208">![瀏覽相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "瀏覽相簿")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="3e6f4-209">*瀏覽相簿*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="3e6f4-210">練習 2： 新增 HTML 協助程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="3e6f4-211">StoreManager 索引分頁具有一個可能的問題： 標題和演出者名稱的屬性都可以是夠長，會擲回關閉資料表格式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="3e6f4-212">在這個練習中，您將學習如何新增自訂的 HTML helper 來截斷文字。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="3e6f4-213">在下圖中，您可以看到如何格式因為文字的長度時修改您使用較小的瀏覽器大小。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="3e6f4-214">![瀏覽與專輯的清單不會被截斷的文字](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "瀏覽與專輯的清單不會被截斷的文字")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="3e6f4-215">*瀏覽與專輯的清單不會被截斷的文字*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="3e6f4-216">工作 1-擴充的 HTML Helper</span><span class="sxs-lookup"><span data-stu-id="3e6f4-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="3e6f4-217">在這個工作中，您會將新方法加入**Truncate**要**HTML** ASP.NET MVC 檢視中公開的物件。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="3e6f4-218">若要這樣做，您會實作**擴充方法**內建**System.Web.Mvc.HtmlHelper** ASP.NET MVC 所提供的類別。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="3e6f4-219">若要深入了解**擴充方法**，請瀏覽這篇 msdn 文章。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="3e6f4-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="3e6f4-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="3e6f4-221">開啟**開始**解決方案位於**來源/Ex2-AddingAnHTMLHelper/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="3e6f4-222">否則，您可能會繼續使用**結束**方案取得完成前一個練習。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="3e6f4-223">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3e6f4-224">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="3e6f4-225">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="3e6f4-226">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="3e6f4-227">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3e6f4-228">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3e6f4-229">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="3e6f4-230">開啟 StoreManager 的索引檢視。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="3e6f4-231">若要這樣做，請在 [方案總管] 中展開**檢視**資料夾，則**StoreManager** ，然後開啟**Index.cshtml**檔案。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="3e6f4-232">新增下列程式碼下方<strong>@model</strong>指示詞來定義<strong>Truncate</strong> helper 方法。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="3e6f4-233">工作 2-截斷網頁中的文字</span><span class="sxs-lookup"><span data-stu-id="3e6f4-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="3e6f4-234">在這個工作中，您將使用**Truncate**截斷檢視範本中的文字的方法。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="3e6f4-235">開啟 StoreManager 的索引檢視。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="3e6f4-236">若要這樣做，請在 [方案總管] 中展開**檢視**資料夾，則**StoreManager** ，然後開啟**Index.cshtml**檔案。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="3e6f4-237">取代顯示的行會**演出者名稱**和 專輯**標題**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="3e6f4-238">若要這樣做，請取代下列幾行。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="3e6f4-239">工作 3-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="3e6f4-240">在這個工作中，您將測試所**StoreManager** **Index**檢視範本會截斷的專輯標題和演出者名稱。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="3e6f4-241">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3e6f4-242">專案一開始在首頁上。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-242">The project starts in the Home page.</span></span> <span data-ttu-id="3e6f4-243">將 URL 變更為 **/StoreManager**若要確認這麼久的時間中的文字**標題**並**演出者**會截斷資料行。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="3e6f4-244">![截斷標題和演出者名稱](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "截斷標題和演出者名稱")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="3e6f4-245">*截斷的標題和演出者名稱*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="3e6f4-246">練習 3： 建立 [編輯] 檢視</span><span class="sxs-lookup"><span data-stu-id="3e6f4-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="3e6f4-247">在此練習中，您將學習如何建立表單，以允許編輯 Album 的存放區管理員。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="3e6f4-248">將會在瀏覽 **/StoreManager/Edit/id** URL (**識別碼**正在編輯專輯的唯一識別碼)，因而讓伺服器的 HTTP GET 呼叫。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="3e6f4-249">編輯控制器動作方法會從資料庫擷取適當的專輯，建立**StoreManagerViewModel**物件封裝 （以及演出者和內容類型的清單），並再將它，傳遞至檢視範本轉譯的 HTML 網頁傳回給使用者。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="3e6f4-250">此頁面將包含**&lt;表單&gt;** 具有文字方塊和下拉式清單編輯專輯屬性的項目。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="3e6f4-251">一旦使用者更新專輯表單值並按**儲存** 按鈕，變更會提交透過 HTTP POST 回呼 **/StoreManager/Edit/id**。雖然 URL 會保留最後一次呼叫相同，ASP.NET MVC 識別，這次它是 HTTP POST，因此會執行不同的編輯動作方法 (其中附有 **[HttpPost]**)。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="3e6f4-252">工作 1-實作 HTTP GET Edit 動作方法</span><span class="sxs-lookup"><span data-stu-id="3e6f4-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="3e6f4-253">在這個工作中，您將實作 Edit 動作方法，來擷取從資料庫中的適當相簿的 HTTP GET 版本以及所有的內容類型和演出者名稱清單。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="3e6f4-254">它會封裝這項資料到**StoreManagerViewModel**的最後一個步驟，然後將傳遞至轉譯回應的檢視範本中定義的物件。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="3e6f4-255">開啟**開始**解決方案位於**來源/Ex3-CreatingTheEditView/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="3e6f4-256">否則，您可能會繼續使用**結束**方案取得完成前一個練習。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="3e6f4-257">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3e6f4-258">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="3e6f4-259">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="3e6f4-260">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="3e6f4-261">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3e6f4-262">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3e6f4-263">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="3e6f4-264">開啟**StoreManagerController**類別。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="3e6f4-265">若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="3e6f4-266">取代**HTTP-GET 編輯**動作方法，以下列程式碼來擷取適當**專輯**，以及**內容類型**並**演出者**列出。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="3e6f4-267">(程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex3 StoreManagerController HTTP-GET 編輯動作*)</span><span class="sxs-lookup"><span data-stu-id="3e6f4-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="3e6f4-268">您使用**System.Web.Mvc** **SelectList**演出者和內容類型，而非**System.Collections.Generic**清單。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="3e6f4-269">**SelectList**是簡潔的方式，來填入 HTML 下拉式清單和管理目前選取範圍等項目。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="3e6f4-270">具現化及更新版本藉由設定這些控制器動作中的 ViewModel 物件將編輯表單案例更簡潔。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="3e6f4-271">工作 2-建立 [編輯] 檢視</span><span class="sxs-lookup"><span data-stu-id="3e6f4-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="3e6f4-272">在這個工作中，您將建立稍後將會顯示專輯屬性編輯的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="3e6f4-273">建立 [編輯] 檢視。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-273">Create the Edit View.</span></span> <span data-ttu-id="3e6f4-274">若要這樣做，以滑鼠右鍵按一下**編輯**動作方法，然後選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="3e6f4-275">在 [新增檢視] 對話方塊中，確認檢視表名稱是否**編輯**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="3e6f4-276">請檢查**建立強型別檢視**核取方塊，然後選取**專輯 (MvcMusicStore.Models)** 從**檢視資料類別**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="3e6f4-277">選取 **編輯**從**Scaffold 樣板**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="3e6f4-278">保留其他欄位的預設值，然後按**新增**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="3e6f4-279">![加入編輯檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "加入編輯檢視")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="3e6f4-280">*加入編輯檢視*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="3e6f4-281">工作 3-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="3e6f4-282">在這個工作中，您將測試所**StoreManager** **編輯**檢視頁面會顯示屬性的值當做參數傳遞的相簿。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="3e6f4-283">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3e6f4-284">專案一開始在首頁上。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-284">The project starts in the Home page.</span></span> <span data-ttu-id="3e6f4-285">將 URL 變更為 **/StoreManager/Edit/1**確認傳遞的相簿的屬性的值會顯示。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="3e6f4-286">![瀏覽專輯的編輯檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "瀏覽專輯的編輯檢視")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="3e6f4-287">*瀏覽專輯的編輯檢視*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="3e6f4-288">工作 4-實作專輯編輯器範本上的下拉式清單</span><span class="sxs-lookup"><span data-stu-id="3e6f4-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="3e6f4-289">在這個工作中，您將建立在最後一個工作中，檢視範本加入下拉式清單，讓使用者可以選取從演出者和內容類型的清單。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="3e6f4-290">程式碼取代所有**專輯**fieldset 程式碼取代為下列：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="3e6f4-291">**Html.DropDownList**協助程式已新增至呈現下拉式清單選擇演出者和內容類型。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="3e6f4-292">參數傳遞給**Html.DropDownList**是：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="3e6f4-293">表單欄位的名稱 (**&quot;ArtistId&quot;**)。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="3e6f4-294">**SelectList**的下拉箭號的值。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="3e6f4-295">工作 5-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="3e6f4-296">在這個工作中，您將測試所**StoreManager** **編輯**檢視頁面會顯示下拉式清單，而不是演出者和內容類型識別碼的文字欄位。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="3e6f4-297">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3e6f4-298">專案一開始在首頁上。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-298">The project starts in the Home page.</span></span> <span data-ttu-id="3e6f4-299">將 URL 變更為 **/StoreManager/Edit/1**以確認它會顯示下拉式清單，而不是演出者和內容類型識別碼的文字欄位。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="3e6f4-300">![瀏覽具有下拉式清單的 專輯的編輯檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "具有下拉式清單的 瀏覽專輯編輯檢視")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="3e6f4-301">*瀏覽專輯的編輯檢視，這次使用下拉式清單*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="3e6f4-302">工作 6-實作編輯 HTTP POST 動作方法</span><span class="sxs-lookup"><span data-stu-id="3e6f4-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="3e6f4-303">現在，[編輯] 檢視會顯示如預期般運作，您需要實作 HTTP POST 編輯動作方法，以儲存至相簿所做的變更。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="3e6f4-304">關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="3e6f4-305">開啟**StoreManagerController**從**控制站**資料夾。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="3e6f4-306">取代**HTTP-POST 編輯**動作方法的程式碼取代下列項目 （請注意，您必須取代的方法會接收兩個參數的多載的版本）：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="3e6f4-307">(程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex3 StoreManagerController HTTP-POST 編輯動作*)</span><span class="sxs-lookup"><span data-stu-id="3e6f4-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="3e6f4-308">這個方法會執行，當使用者按一下**儲存**檢視的按鈕，並執行 HTTP POST 至伺服器之表單值將它們保存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="3e6f4-309">裝飾項目 **[HttpPost]** 表示方法應該用於 HTTP POST 的那些案例。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="3e6f4-310">此方法會採用**專輯**物件。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-310">The method takes an **Album** object.</span></span> <span data-ttu-id="3e6f4-311">ASP.NET MVC 會自動建立的專輯物件，從張貼&lt;表單&gt;值。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="3e6f4-312">方法會執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="3e6f4-313">如果模型有效：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="3e6f4-314">更新專輯中的項目將它標示為已修改物件的內容。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="3e6f4-315">儲存變更並重新導向至 [索引] 檢視。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="3e6f4-316">如果該模型不是有效的它將會填入與 ViewBag **GenreId**並**ArtistId**，則它會傳回與接收的專輯物件，以允許使用者檢視執行任何必要的更新。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="3e6f4-317">工作 7-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="3e6f4-318">在這個工作中，您將測試所**StoreManager 編輯**檢視頁面實際上會將更新的專輯資料儲存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="3e6f4-319">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3e6f4-320">專案一開始在首頁上。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-320">The project starts in the Home page.</span></span> <span data-ttu-id="3e6f4-321">將 URL 變更為 **/StoreManager/Edit/1**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="3e6f4-322">將專輯標題來變更**負載**，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="3e6f4-323">請確認專輯標題實際變更清單中的專輯。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="3e6f4-324">![更新 album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "更新相簿")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="3e6f4-325">*更新相簿*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="3e6f4-326">練習 4： 加入建立檢視</span><span class="sxs-lookup"><span data-stu-id="3e6f4-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="3e6f4-327">既然**StoreManagerController**支援**編輯**能力，在這個練習中您會了解如何加入建立檢視範本，讓儲存管理員應用程式中加入新的相簿。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="3e6f4-328">像您一樣的編輯功能，您會實作使用兩個不同的方法內建立案例**StoreManagerController**類別：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="3e6f4-329">其中一個動作方法會顯示空白的表單，當存放區管理員第一次造訪**StoreManager/建立**URL。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="3e6f4-330">第二個動作方法會處理案例的存放區管理員，按一下**儲存**表單內的按鈕，然後送出的值將會回到**StoreManager/建立**URL 為 HTTP POST。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="3e6f4-331">工作 1-實作 HTTP GET 建立動作方法</span><span class="sxs-lookup"><span data-stu-id="3e6f4-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="3e6f4-332">在這個工作中，您將實作建立動作方法，若要擷取所有的內容類型和演出者名稱清單，請封裝此資料至的 HTTP GET 新版**StoreManagerViewModel**物件，然後將傳遞至檢視範本。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="3e6f4-333">開啟**開始**解決方案位於**來源/Ex4-AddingACreateView/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="3e6f4-334">否則，您可能會繼續使用**結束**方案取得完成前一個練習。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="3e6f4-335">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3e6f4-336">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="3e6f4-337">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="3e6f4-338">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="3e6f4-339">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3e6f4-340">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3e6f4-341">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="3e6f4-342">開啟**StoreManagerController**類別。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="3e6f4-343">若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="3e6f4-344">取代**建立**動作方法的程式碼取代為下列：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="3e6f4-345">(程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex4 StoreManagerController HTTP-GET 建立動作*)</span><span class="sxs-lookup"><span data-stu-id="3e6f4-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="3e6f4-346">工作 2-加入建立檢視</span><span class="sxs-lookup"><span data-stu-id="3e6f4-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="3e6f4-347">在這個工作中，您將加入建立檢視範本，將會顯示新的 （空的） 專輯表單。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="3e6f4-348">以滑鼠右鍵按一下**Create**動作方法，然後選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="3e6f4-349">這會顯示 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="3e6f4-350">在 [新增檢視] 對話方塊中，確認檢視表名稱是否**建立**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="3e6f4-351">選取 **建立強型別檢視**選項，然後選取**專輯 (MvcMusicStore.Models)** 從**模型類別**下拉式清單和**建立**從**Scaffold 樣板**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="3e6f4-352">保留其他欄位的預設值，然後按**新增**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="3e6f4-353">![加入建立檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "新增-a-建立-view.png")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="3e6f4-354">*加入建立檢視*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="3e6f4-355">更新**GenreId**並**ArtistId**欄位，使用下拉式清單，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="3e6f4-356">工作 3-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="3e6f4-357">在這個工作中，您將測試所**StoreManager** **建立**檢視頁面會顯示一個空白的專輯表單。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="3e6f4-358">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3e6f4-359">專案一開始在首頁上。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-359">The project starts in the Home page.</span></span> <span data-ttu-id="3e6f4-360">將 URL 變更為**StoreManager/建立**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="3e6f4-361">確認的空白表單會顯示為填滿新專輯屬性。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="3e6f4-362">![使用空白的表單建立檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "建立檢視的空白表單")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="3e6f4-363">*使用空白的表單建立檢視*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="3e6f4-364">工作 4-實作 HTTP POST 建立動作方法</span><span class="sxs-lookup"><span data-stu-id="3e6f4-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="3e6f4-365">在這個工作中，您將實作會叫用，當使用者按一下建立動作方法的 HTTP POST 新版**儲存** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="3e6f4-366">此方法應該在資料庫中儲存新的相簿。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="3e6f4-367">關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="3e6f4-368">開啟**StoreManagerController**類別。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="3e6f4-369">若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="3e6f4-370">取代**HTTP POST 建立**動作方法的程式碼取代為下列：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="3e6f4-371">(程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex4 StoreManagerController HTTP POST 建立動作*)</span><span class="sxs-lookup"><span data-stu-id="3e6f4-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="3e6f4-372">建立動作相當類似於上一個編輯動作方法，但而不是為已修改，請設定物件，它會新增至內容。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="3e6f4-373">工作 5-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="3e6f4-374">在這個工作中，您將測試所**StoreManager 建立**檢視頁面可讓您建立新的相簿，，然後重新導向至 StoreManager 索引 檢視。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="3e6f4-375">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3e6f4-376">專案一開始在首頁上。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-376">The project starts in the Home page.</span></span> <span data-ttu-id="3e6f4-377">將 URL 變更為**StoreManager/建立**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="3e6f4-378">新的專輯、 類似下圖中填入資料的所有表單欄位：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="3e6f4-379">![建立相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "建立相簿")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="3e6f4-380">*建立相簿*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-380">*Creating an Album*</span></span>
3. <span data-ttu-id="3e6f4-381">請確認您會被重新導向，以包含剛才建立的新專輯 StoreManager 索引檢視。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="3e6f4-382">![建立新的相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "新建立的相簿")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="3e6f4-383">*建立新的相簿*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="3e6f4-384">練習 5： 處理刪除</span><span class="sxs-lookup"><span data-stu-id="3e6f4-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="3e6f4-385">尚未實作刪除 album 的能力。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="3e6f4-386">這是此練習中會有何相關。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-386">This is what this exercise will be about.</span></span> <span data-ttu-id="3e6f4-387">像之前，您將在此實作使用兩個不同的方法中的刪除案例**StoreManagerController**類別：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="3e6f4-388">其中一個動作方法會顯示確認表單</span><span class="sxs-lookup"><span data-stu-id="3e6f4-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="3e6f4-389">第二個動作方法將處理表單提交</span><span class="sxs-lookup"><span data-stu-id="3e6f4-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="3e6f4-390">工作 1-實作 HTTP GET Delete 動作方法</span><span class="sxs-lookup"><span data-stu-id="3e6f4-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="3e6f4-391">在這個工作中，您將實作要擷取的專輯資訊的 Delete 動作方法的 HTTP GET 版本。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="3e6f4-392">開啟**開始**解決方案位於**來源/Ex5-HandlingDeletion/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="3e6f4-393">否則，您可能會繼續使用**結束**方案取得完成前一個練習。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="3e6f4-394">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3e6f4-395">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="3e6f4-396">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="3e6f4-397">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="3e6f4-398">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3e6f4-399">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3e6f4-400">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="3e6f4-401">開啟**StoreManagerController**類別。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="3e6f4-402">若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="3e6f4-403">刪除控制器動作正是與先前的存放區詳細資料控制器動作相同： 它會查詢**專輯**從資料庫使用的物件**識別碼**提供的 URL 和傳回適當**檢視**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="3e6f4-404">若要這樣做，請將 HTTP GET**刪除**動作方法的程式碼取代為下列：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="3e6f4-405">(程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex5 處理刪除 HTTP-GET 刪除動作*)</span><span class="sxs-lookup"><span data-stu-id="3e6f4-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="3e6f4-406">以滑鼠右鍵按一下**刪除**動作方法，然後選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="3e6f4-407">這會顯示 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="3e6f4-408">在 [新增檢視] 對話方塊中，確認檢視表名稱是否**刪除**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="3e6f4-409">選取 **建立強型別檢視**選項，然後選取**專輯 (MvcMusicStore.Models)** 從**模型類別**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="3e6f4-410">選取 **刪除**從**Scaffold 樣板**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="3e6f4-411">保留其他欄位的預設值，然後按**新增**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="3e6f4-412">![加入刪除檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "加入刪除檢視")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="3e6f4-413">*加入刪除檢視*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="3e6f4-414">刪除範本會顯示模型中的所有欄位。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="3e6f4-415">您將會顯示只有專輯標題。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-415">You will show only the album's title.</span></span> <span data-ttu-id="3e6f4-416">若要這樣做，請取代下列程式碼中檢視的內容：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="3e6f4-417">工作 2-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="3e6f4-418">在這個工作中，您將測試所**StoreManager** **刪除**檢視頁面會顯示確認刪除表單。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="3e6f4-419">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3e6f4-420">專案一開始在首頁上。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-420">The project starts in the Home page.</span></span> <span data-ttu-id="3e6f4-421">將 URL 變更為 **/StoreManager**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="3e6f4-422">選取即可刪除一個專輯**刪除**並確認已上傳新的檢視。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="3e6f4-423">![刪除 Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "刪除 Album")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="3e6f4-424">*刪除 Album*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="3e6f4-425">工作 3-實作 HTTP POST 的 Delete 動作方法</span><span class="sxs-lookup"><span data-stu-id="3e6f4-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="3e6f4-426">在這個工作中，您將實作會叫用，當使用者按一下的 Delete 動作方法的 HTTP POST 新版**刪除** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="3e6f4-427">此方法應該刪除資料庫中的專輯。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="3e6f4-428">關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="3e6f4-429">開啟**StoreManagerController**類別。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="3e6f4-430">若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="3e6f4-431">取代**HTTP POST Delete**動作方法的程式碼取代為下列：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="3e6f4-432">(程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex5 處理刪除 HTTP-POST 刪除動作*)</span><span class="sxs-lookup"><span data-stu-id="3e6f4-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="3e6f4-433">工作 4-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="3e6f4-434">在這個工作中，您將測試所**StoreManager 刪除**檢視頁面可讓您刪除 Album，，然後重新導向至 StoreManager 索引 檢視。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="3e6f4-435">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3e6f4-436">專案一開始在首頁上。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-436">The project starts in the Home page.</span></span> <span data-ttu-id="3e6f4-437">將 URL 變更為 **/StoreManager**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="3e6f4-438">選取即可刪除一個專輯**刪除。**</span><span class="sxs-lookup"><span data-stu-id="3e6f4-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="3e6f4-439">按一下確認刪除**刪除**按鈕：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="3e6f4-440">![刪除 Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "刪除 Album")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="3e6f4-441">*刪除 Album*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="3e6f4-442">確認已刪除 album，因為它不會顯示在**Index**頁面。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="3e6f4-443">練習 6： 新增驗證</span><span class="sxs-lookup"><span data-stu-id="3e6f4-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="3e6f4-444">目前，您已備妥的 Create 和 Edit 表單不會執行任何一種驗證。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="3e6f4-445">如果使用者離開的必要的欄位留空或輸入字母，在 [價格] 欄位中，您會收到的第一個錯誤將會從資料庫。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="3e6f4-446">您可以新增至應用程式的驗證，藉由將您的模型類別中的資料註解。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="3e6f4-447">資料註解允許描述套用至您的模型屬性，您需要的規則和 ASP.NET MVC 會負責強制執行，並向使用者顯示適當的訊息。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="3e6f4-448">工作 1-新增資料註解</span><span class="sxs-lookup"><span data-stu-id="3e6f4-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="3e6f4-449">在這個工作中，您將加入資料註解，可讓建立與編輯頁面的專輯模型顯示在適當的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="3e6f4-450">對於簡單的模型類別，加入資料註解只處理加上**使用**陳述式**System.ComponentModel.DataAnnotation**，然後放置 **[必要]** 適當屬性的屬性。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="3e6f4-451">下列範例會使**名稱**屬性檢視的必要欄位。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="3e6f4-452">這是此應用程式的情況稍微複雜 Entity Data Model 產生的位置。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="3e6f4-453">如果您的資料註解直接新增至模型類別，它們會覆寫您從資料庫更新模型。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="3e6f4-454">相反地，您可以進行使用它來保存註解會存在，並與模型相關聯的中繼資料部分類別的類別使用 **[MetadataType]** 屬性。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="3e6f4-455">開啟**開始**解決方案位於**來源/Ex6-AddingValidation/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="3e6f4-456">否則，您可能會繼續使用**結束**方案取得完成前一個練習。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="3e6f4-457">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3e6f4-458">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="3e6f4-459">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="3e6f4-460">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="3e6f4-461">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3e6f4-462">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3e6f4-463">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="3e6f4-464">開啟**Album.cs**從**模型**資料夾。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="3e6f4-465">取代**Album.cs**內容以反白顯示的程式碼，使它看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="3e6f4-466">線條 **[DisplayFormat(ConvertEmptyStringToNull=false)]** 代表模型中的空字串不會轉換為 null 的資料來源中更新資料欄位時。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="3e6f4-467">當 Entity Framework 會指派 null 值的模型資料註解驗證欄位之前，此設定可避免例外狀況。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="3e6f4-468">(程式碼片段- *ASP.NET MVC 4 Helper 和表單和驗證-Ex6 專輯中繼資料的部分類別*)</span><span class="sxs-lookup"><span data-stu-id="3e6f4-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="3e6f4-469">這**專輯**部分類別具有**MetadataType**屬性，指向**AlbumMetaData**資料註解的類別。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="3e6f4-470">以下是一些您用來標註的專輯模型的資料註解屬性：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="3e6f4-471">必要-指出屬性是否為必填的欄位</span><span class="sxs-lookup"><span data-stu-id="3e6f4-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="3e6f4-472">DisplayName-定義用於表單欄位和驗證訊息的文字</span><span class="sxs-lookup"><span data-stu-id="3e6f4-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="3e6f4-473">Displayformat 無法為指定資料欄位會顯示並格式化的方式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="3e6f4-474">StringLength-定義的字串欄位的最大長度</span><span class="sxs-lookup"><span data-stu-id="3e6f4-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="3e6f4-475">範圍-提供最大和最小值為數值欄位</span><span class="sxs-lookup"><span data-stu-id="3e6f4-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="3e6f4-476">ScaffoldColumn-可讓隱藏於編輯器表單的欄位</span><span class="sxs-lookup"><span data-stu-id="3e6f4-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="3e6f4-477">工作 2-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="3e6f4-478">在這個工作中，您將測試 Create 和 Edit 頁面驗證欄位中，使用在前一項工作中選擇的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="3e6f4-479">按下**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3e6f4-480">專案一開始在首頁上。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-480">The project starts in the Home page.</span></span> <span data-ttu-id="3e6f4-481">將 URL 變更為**StoreManager/建立**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="3e6f4-482">確認 顯示名稱相符的部分類別中的項目 (例如**專輯封面 URL**而不是**AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="3e6f4-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="3e6f4-483">按一下 **建立**，而不需要填入表單。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="3e6f4-484">請確認您取得對應的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="3e6f4-485">![驗證 [建立] 頁面中的欄位](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "驗證在 [建立] 頁面中的欄位")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="3e6f4-486">*在 [建立] 頁面中的驗證的欄位*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="3e6f4-487">您可以確認相同發生**編輯**頁面。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="3e6f4-488">將 URL 變更為 **/StoreManager/Edit/1** ，並確認 顯示名稱相符的部分類別中的項目 (例如**專輯封面 URL**而非**AlbumArtUrl**)。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="3e6f4-489">空**標題**並**價格**欄位，然後按一下 **儲存**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="3e6f4-490">請確認您取得對應的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-490">Verify that you get the corresponding validation messages.</span></span>

    ![在 [編輯] 頁面中的驗證的欄位](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="3e6f4-492">*在 [編輯] 頁面中的驗證的欄位*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="3e6f4-493">練習 7： 使用不顯眼的 jQuery 用戶端</span><span class="sxs-lookup"><span data-stu-id="3e6f4-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="3e6f4-494">在此練習中，您將學習如何啟用 MVC 4 不顯眼的 jQuery 驗證，在用戶端。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="3e6f4-495">不顯眼的 jQuery 會使用資料 ajax 前置詞 JavaScript 來叫用伺服器，而非干擾的方式發出內嵌用戶端指令碼的動作方法。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="3e6f4-496">工作 1-執行之前啟用不顯眼的 jQuery 應用程式</span><span class="sxs-lookup"><span data-stu-id="3e6f4-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="3e6f4-497">在這個工作中，您將執行的應用程式，再以比較這兩種驗證模型包括 jQuery。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="3e6f4-498">開啟**開始**解決方案位於**來源/Ex7-UnobtrusivejQueryValidation/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="3e6f4-499">否則，您可能會繼續使用**結束**方案取得完成前一個練習。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="3e6f4-500">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3e6f4-501">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="3e6f4-502">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="3e6f4-503">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="3e6f4-504">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3e6f4-505">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3e6f4-506">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="3e6f4-507">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="3e6f4-508">專案一開始在首頁上。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-508">The project starts in the Home page.</span></span> <span data-ttu-id="3e6f4-509">瀏覽**StoreManager/Create**然後按一下**建立**沒有填滿表單，以確認您取得驗證訊息：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="3e6f4-510">![停用的用戶端驗證](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "停用的用戶端驗證")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="3e6f4-511">*停用的用戶端驗證*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="3e6f4-512">在瀏覽器中開啟 HTML 程式碼：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="3e6f4-513">工作 2-啟用不顯眼的用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="3e6f4-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="3e6f4-514">在這個工作中，您將啟用 jQuery**不顯眼的用戶端驗證**從**Web.config**預設會設定為 false，所有新的 ASP.NET MVC 4 專案中的檔案。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="3e6f4-515">此外，您會加入必要的指令碼進行 jQuery 低調的用戶端驗證工作的參考。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="3e6f4-516">開啟**Web.Config**檔案位於專案根目錄，並確定**ClientValidationEnabled**並**UnobtrusiveJavaScriptEnabled**金鑰值都會設為 **，則為 true**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="3e6f4-517">您也可以啟用用戶端驗證的程式碼在 Global.asax.cs 以取得相同的結果：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="3e6f4-518">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="3e6f4-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="3e6f4-519">此外，您可以將 ClientValidationEnabled 屬性指派至任何控制器，將自訂行為。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="3e6f4-520">開啟**Create.cshtml**在**Views\StoreManager**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="3e6f4-521">下列指令碼檔案中，請確定**jquery.validate**並**jquery.validate.unobtrusive**中檢視透過, 參考&quot; **~/bundles/jqueryval**&quot;套件組合。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="3e6f4-522">所有這些 jQuery 程式庫會包含在新的 MVC 4 專案。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="3e6f4-523">您可以找到更多的程式庫中 **/指令碼**您專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="3e6f4-524">若要讓這項驗證程式庫運作，您需要新增 jQuery framework 程式庫的參考。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="3e6f4-525">因為在已加入這個參考 **\_Layout.cshtml**檔案中，您不需要將它加入這個特定的檢視中。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="3e6f4-526">工作 3-執行應用程式使用不顯眼的 jQuery 驗證</span><span class="sxs-lookup"><span data-stu-id="3e6f4-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="3e6f4-527">在這個工作中，您將測試所**StoreManager**建立範本會執行在使用者建立新專輯時，使用 jQuery 程式庫的用戶端驗證的檢視。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="3e6f4-528">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="3e6f4-529">專案一開始在首頁上。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-529">The project starts in the Home page.</span></span> <span data-ttu-id="3e6f4-530">瀏覽**StoreManager/Create**然後按一下**建立**沒有填滿表單，以確認您取得驗證訊息：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="3e6f4-531">![用戶端驗證與啟用的 jQuery](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "與啟用的 jQuery 用戶端驗證")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="3e6f4-532">*使用 jQuery 啟用的用戶端驗證*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="3e6f4-533">在瀏覽器中，開啟 建立檢視的原始程式碼：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="3e6f4-534">每個用戶端驗證規則不顯眼的 jQuery 加入資料屬性-val-*rulename*=&quot;*訊息*&quot;。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="3e6f4-535">以下是一份標記該 Unobtrusive jQuery 插入 html 輸入欄位，來執行用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="3e6f4-536">資料值</span><span class="sxs-lookup"><span data-stu-id="3e6f4-536">Data-val</span></span>
   > - <span data-ttu-id="3e6f4-537">資料值數目</span><span class="sxs-lookup"><span data-stu-id="3e6f4-537">Data-val-number</span></span>
   > - <span data-ttu-id="3e6f4-538">資料值範圍</span><span class="sxs-lookup"><span data-stu-id="3e6f4-538">Data-val-range</span></span>
   > - <span data-ttu-id="3e6f4-539">資料 val 範圍-分鐘/資料-val--最大範圍</span><span class="sxs-lookup"><span data-stu-id="3e6f4-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="3e6f4-540">Val 需要資料</span><span class="sxs-lookup"><span data-stu-id="3e6f4-540">Data-val-required</span></span>
   > - <span data-ttu-id="3e6f4-541">資料 val 長度</span><span class="sxs-lookup"><span data-stu-id="3e6f4-541">Data-val-length</span></span>
   > - <span data-ttu-id="3e6f4-542">資料-val-長度-最大值/資料 val 長度-分鐘</span><span class="sxs-lookup"><span data-stu-id="3e6f4-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="3e6f4-543">所有資料值會都填入模型**資料註解**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="3e6f4-544">然後，在伺服器端的所有邏輯可以在用戶端都執行。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="3e6f4-545">比方說，價格屬性會在模型中，具有下列資料註解：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="3e6f4-546">使用不顯眼的 jQuery 之後, 產生的程式碼是：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="3e6f4-547">總結</span><span class="sxs-lookup"><span data-stu-id="3e6f4-547">Summary</span></span>

<span data-ttu-id="3e6f4-548">藉由完成這個實際操作實驗室中，您已了解如何讓使用者能夠變更藉由使用下列資料庫中儲存的資料：</span><span class="sxs-lookup"><span data-stu-id="3e6f4-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="3e6f4-549">控制器動作，例如索引，建立、 編輯、 刪除</span><span class="sxs-lookup"><span data-stu-id="3e6f4-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="3e6f4-550">ASP.NET MVC 樣板功能，來以 HTML 表格顯示屬性</span><span class="sxs-lookup"><span data-stu-id="3e6f4-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="3e6f4-551">自訂 HTML 協助程式，以改善使用者體驗</span><span class="sxs-lookup"><span data-stu-id="3e6f4-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="3e6f4-552">動作方法，以回應 HTTP-GET 或 HTTP-POST 呼叫</span><span class="sxs-lookup"><span data-stu-id="3e6f4-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="3e6f4-553">類似的檢視範本，例如建立和編輯共用的編輯器範本</span><span class="sxs-lookup"><span data-stu-id="3e6f4-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="3e6f4-554">Form 項目，例如下拉式清單</span><span class="sxs-lookup"><span data-stu-id="3e6f4-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="3e6f4-555">資料註解進行模型驗證</span><span class="sxs-lookup"><span data-stu-id="3e6f4-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="3e6f4-556">使用 jQuery 低調的程式庫的用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="3e6f4-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="3e6f4-557">附錄 a： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="3e6f4-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="3e6f4-558">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="3e6f4-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="3e6f4-559">下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="3e6f4-560">移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="3e6f4-561">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Windows Azure SDK</em>&quot;。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="3e6f4-562">按一下 **立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-562">Click on **Install Now**.</span></span> <span data-ttu-id="3e6f4-563">如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="3e6f4-564">一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="3e6f4-565">![安裝 Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="3e6f4-566">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="3e6f4-567">閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="3e6f4-569">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="3e6f4-570">等候完成的下載與安裝程序。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-570">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="3e6f4-572">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-572">*Installation progress*</span></span>
6. <span data-ttu-id="3e6f4-573">安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-573">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="3e6f4-575">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-575">*Installation completed*</span></span>
7. <span data-ttu-id="3e6f4-576">按一下 **結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="3e6f4-577">若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 圖格](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="3e6f4-579">*VS Express for Web 圖格*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="3e6f4-580">附錄 b： 使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="3e6f4-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="3e6f4-581">使用程式碼片段，您會有您需要在隨手可得的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="3e6f4-582">實驗室課程文件會告訴您完全時您可以使用它們，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="3e6f4-583">![若要將程式碼插入您的專案使用 Visual Studio 程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "程式碼插入您的專案使用 Visual Studio 程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="3e6f4-584">*若要將程式碼插入您的專案使用 Visual Studio 程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="3e6f4-585">***若要新增的程式碼片段，使用鍵盤 （僅限 C#)***</span><span class="sxs-lookup"><span data-stu-id="3e6f4-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="3e6f4-586">將游標放在您想要插入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="3e6f4-587">開始輸入程式碼片段名稱 （不含空格或連字號）。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="3e6f4-588">觀看為 IntelliSense 會顯示相符的程式碼片段的名稱。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="3e6f4-589">選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="3e6f4-590">按下 Tab 鍵兩次的游標位置插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="3e6f4-591">![開始鍵入程式碼片段名稱](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "開始輸入程式碼片段名稱")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="3e6f4-592">*開始鍵入程式碼片段名稱*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="3e6f4-593">![按 Tab 鍵以選取反白顯示的程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "按 Tab 鍵以選取反白顯示的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="3e6f4-594">*按 Tab 鍵以選取反白顯示的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="3e6f4-595">![再次按 Tab 鍵和程式碼片段會依序展開](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "再次按 Tab 鍵和程式碼片段會展開")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="3e6f4-596">*再次按 Tab 鍵和程式碼片段會展開*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="3e6f4-597">***若要新增的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="3e6f4-598">以滑鼠右鍵按一下您要插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="3e6f4-599">選取 **插入程式碼片段**後面**My Code Snippets**。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="3e6f4-600">選擇相關的程式片段，從清單中，對它按一下。</span><span class="sxs-lookup"><span data-stu-id="3e6f4-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="3e6f4-601">![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="3e6f4-602">*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="3e6f4-603">![對它按一下挑選清單中，相關程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "對它按一下挑選清單中，相關程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="3e6f4-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="3e6f4-604">*對它按一下挑選清單中，相關程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="3e6f4-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
