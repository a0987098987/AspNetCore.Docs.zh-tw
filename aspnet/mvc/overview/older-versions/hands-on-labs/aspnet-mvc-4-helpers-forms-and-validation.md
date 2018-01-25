---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: "ASP.NET MVC 4 Helper、 表單和驗證 |Microsoft 文件"
author: rick-anderson
description: "在 ASP.NET MVC 4 模型和資料存取實際操作實驗室中，您已載入和顯示資料的資料庫。 在這個實際操作實驗室中，您將會加入..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 925d659f42496045089ba056e194ac977c37a8de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="b3433-104">ASP.NET MVC 4 Helper、 表單和驗證</span><span class="sxs-lookup"><span data-stu-id="b3433-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>
====================
<span data-ttu-id="b3433-105">由[Web 營小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b3433-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="b3433-106">在**ASP.NET MVC 4 模型和資料存取**實際操作實驗室中，您已載入和顯示資料的資料庫。</span><span class="sxs-lookup"><span data-stu-id="b3433-106">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="b3433-107">在這個實際操作實驗室中，您將會加入**Music Store**應用程式編輯該資料的功能。</span><span class="sxs-lookup"><span data-stu-id="b3433-107">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>
> 
> <span data-ttu-id="b3433-108">請注意該目標，您將先建立將支援建立、 讀取、 更新和刪除 (CRUD) 動作的相簿的控制站。</span><span class="sxs-lookup"><span data-stu-id="b3433-108">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="b3433-109">您將會產生運用 ASP.NET MVC 的 scaffolding 功能，以顯示 HTML 表格中的專輯屬性的索引檢視範本。</span><span class="sxs-lookup"><span data-stu-id="b3433-109">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="b3433-110">若要增強該檢視，您將加入將會截斷長描述的自訂 HTML helper。</span><span class="sxs-lookup"><span data-stu-id="b3433-110">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>
> 
> <span data-ttu-id="b3433-111">之後，您將加入的編輯和建立的檢視表，以供您變更在資料庫中，使用下拉式清單類似的表單項目說明相簿。</span><span class="sxs-lookup"><span data-stu-id="b3433-111">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>
> 
> <span data-ttu-id="b3433-112">最後，您將會讓使用者可以刪除專輯和您將會防止使用者從輸入錯誤的資料來驗證其輸入。</span><span class="sxs-lookup"><span data-stu-id="b3433-112">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="b3433-113">這個實際操作實驗室假設您擁有的基本知識**ASP.NET MVC**。</span><span class="sxs-lookup"><span data-stu-id="b3433-113">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="b3433-114">如果您不使用**ASP.NET MVC**之前，我們建議您在一段**ASP.NET MVC 基礎**實際操作實驗室。</span><span class="sxs-lookup"><span data-stu-id="b3433-114">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>
> 
> 
> <span data-ttu-id="b3433-115">這個實驗室將引導您完成先前所述將微幅變更套用至來源資料夾中提供的範例 Web 應用程式的新功能與增強功能。</span><span class="sxs-lookup"><span data-stu-id="b3433-115">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="b3433-116">所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843)。</span><span class="sxs-lookup"><span data-stu-id="b3433-116">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="b3433-117">目標</span><span class="sxs-lookup"><span data-stu-id="b3433-117">Objectives</span></span>

<span data-ttu-id="b3433-118">在這個實際操作實驗室中，您將學會如何：</span><span class="sxs-lookup"><span data-stu-id="b3433-118">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="b3433-119">建立支援 CRUD 作業的控制站</span><span class="sxs-lookup"><span data-stu-id="b3433-119">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="b3433-120">產生索引檢視來顯示 HTML 表格中的實體屬性</span><span class="sxs-lookup"><span data-stu-id="b3433-120">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="b3433-121">加入自訂的 HTML helper</span><span class="sxs-lookup"><span data-stu-id="b3433-121">Add a custom HTML helper</span></span>
- <span data-ttu-id="b3433-122">建立和自訂的編輯檢視</span><span class="sxs-lookup"><span data-stu-id="b3433-122">Create and customize an Edit View</span></span>
- <span data-ttu-id="b3433-123">區分回應 HTTP GET 或 HTTP POST 呼叫的動作方法</span><span class="sxs-lookup"><span data-stu-id="b3433-123">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="b3433-124">加入和自訂建立的檢視</span><span class="sxs-lookup"><span data-stu-id="b3433-124">Add and customize a Create View</span></span>
- <span data-ttu-id="b3433-125">處理刪除的實體</span><span class="sxs-lookup"><span data-stu-id="b3433-125">Handle the deletion of an entity</span></span>
- <span data-ttu-id="b3433-126">驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="b3433-126">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b3433-127">必要條件</span><span class="sxs-lookup"><span data-stu-id="b3433-127">Prerequisites</span></span>

<span data-ttu-id="b3433-128">您必須具備下列項目才能完成本實驗室：</span><span class="sxs-lookup"><span data-stu-id="b3433-128">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="b3433-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 A](#AppendixA)如需有關如何安裝指示)。</span><span class="sxs-lookup"><span data-stu-id="b3433-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b3433-130">安裝程式</span><span class="sxs-lookup"><span data-stu-id="b3433-130">Setup</span></span>

<span data-ttu-id="b3433-131">**安裝程式碼片段**</span><span class="sxs-lookup"><span data-stu-id="b3433-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="b3433-132">為了方便起見，大部分您將沿著這個實驗室管理的程式碼可做為 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="b3433-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b3433-133">若要安裝執行的程式碼片段**.\Source\Setup\CodeSnippets.vsi**檔案。</span><span class="sxs-lookup"><span data-stu-id="b3433-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b3433-134">如果您不熟悉 Visual Studio 程式碼片段，而且想来了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 b： 使用程式碼片段](#AppendixB)&quot;。</span><span class="sxs-lookup"><span data-stu-id="b3433-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b3433-135">練習</span><span class="sxs-lookup"><span data-stu-id="b3433-135">Exercises</span></span>

<span data-ttu-id="b3433-136">下列練習構成這個實際操作實驗室：</span><span class="sxs-lookup"><span data-stu-id="b3433-136">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="b3433-137">建立存放區管理員控制站和其索引的檢視</span><span class="sxs-lookup"><span data-stu-id="b3433-137">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="b3433-138">加入 HTML Helper</span><span class="sxs-lookup"><span data-stu-id="b3433-138">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="b3433-139">建立編輯檢視</span><span class="sxs-lookup"><span data-stu-id="b3433-139">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="b3433-140">加入建立檢視</span><span class="sxs-lookup"><span data-stu-id="b3433-140">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="b3433-141">處理刪除</span><span class="sxs-lookup"><span data-stu-id="b3433-141">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="b3433-142">新增驗證</span><span class="sxs-lookup"><span data-stu-id="b3433-142">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="b3433-143">在用戶端使用不顯眼的 jQuery</span><span class="sxs-lookup"><span data-stu-id="b3433-143">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="b3433-144">每個練習會伴隨著**結束**包含所產生的解決方案完成練習之後，您應該取得資料夾。</span><span class="sxs-lookup"><span data-stu-id="b3433-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="b3433-145">如果您需要其他協助使用的所有練習，您可以使用這個方案做為指南。</span><span class="sxs-lookup"><span data-stu-id="b3433-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="b3433-146">完成本實驗室估計時間： **60 分鐘的時間**</span><span class="sxs-lookup"><span data-stu-id="b3433-146">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="b3433-147">練習 1： 建立存放區管理員控制站和其索引的檢視</span><span class="sxs-lookup"><span data-stu-id="b3433-147">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="b3433-148">在此練習中，您將學習如何建立新的控制站，以支援 CRUD 作業，來自訂其從資料庫，最後產生運用 ASP.NET MVC 的 scaffolding 索引檢視表範本傳回一份專輯的索引動作方法顯示 HTML 表格中的專輯屬性的功能。</span><span class="sxs-lookup"><span data-stu-id="b3433-148">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="b3433-149">工作 1-建立 StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="b3433-149">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="b3433-150">在這個工作中，您將建立新的控制器呼叫**StoreManagerController**支援 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="b3433-150">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="b3433-151">開啟**開始**方案位於**來源/Ex1CreatingTheStoreManagerController/開始/**資料夾。</span><span class="sxs-lookup"><span data-stu-id="b3433-151">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

    1. <span data-ttu-id="b3433-152">您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="b3433-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b3433-153">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="b3433-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="b3433-154">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="b3433-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="b3433-155">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="b3433-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3433-156">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="b3433-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b3433-157">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b3433-158">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="b3433-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b3433-159">加入新的控制器。</span><span class="sxs-lookup"><span data-stu-id="b3433-159">Add a new controller.</span></span> <span data-ttu-id="b3433-160">若要這樣做，請以滑鼠右鍵按一下**控制器**資料夾在方案總管中，選取**新增**然後**控制器**命令。</span><span class="sxs-lookup"><span data-stu-id="b3433-160">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="b3433-161">變更**控制器****名稱**至**StoreManagerController** ，並確定選項**MVC 控制器具有空白讀取/寫入動作**已選取。</span><span class="sxs-lookup"><span data-stu-id="b3433-161">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="b3433-162">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="b3433-162">Click **Add**.</span></span>

    <span data-ttu-id="b3433-163">![加入控制器 對話方塊](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "加入控制器 對話方塊")</span><span class="sxs-lookup"><span data-stu-id="b3433-163">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="b3433-164">*加入控制器 對話方塊*</span><span class="sxs-lookup"><span data-stu-id="b3433-164">*Add Controller Dialog*</span></span>

    <span data-ttu-id="b3433-165">會產生新的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="b3433-165">A new Controller class is generated.</span></span> <span data-ttu-id="b3433-166">因為您指定要加入的讀取/寫入，虛設常式方法的動作一般 CRUD 動作會建立 TODO 註解中，填滿提示要包含應用程式特定邏輯。</span><span class="sxs-lookup"><span data-stu-id="b3433-166">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="b3433-167">工作 2-自訂 StoreManager 索引</span><span class="sxs-lookup"><span data-stu-id="b3433-167">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="b3433-168">在這項工作，您將自訂 StoreManager 索引動作方法，從資料庫傳回相簿的清單檢視。</span><span class="sxs-lookup"><span data-stu-id="b3433-168">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="b3433-169">在 StoreManagerController 類別中，加入下列*使用*指示詞。</span><span class="sxs-lookup"><span data-stu-id="b3433-169">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="b3433-170">(程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex1 使用 MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="b3433-170">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="b3433-171">加入欄位以**StoreManagerController**來保存的執行個體**MusicStoreEntities。**</span><span class="sxs-lookup"><span data-stu-id="b3433-171">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="b3433-172">(程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="b3433-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="b3433-173">實作 StoreManagerController 索引動作傳回的專輯清單檢視。</span><span class="sxs-lookup"><span data-stu-id="b3433-173">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="b3433-174">控制器動作邏輯將會非常類似於先前寫入 StoreController 索引動作。</span><span class="sxs-lookup"><span data-stu-id="b3433-174">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="b3433-175">使用 LINQ 來擷取所有的相簿，包括顯示的內容類型與演出者資訊。</span><span class="sxs-lookup"><span data-stu-id="b3433-175">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="b3433-176">(程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex1 StoreManagerController 索引*)</span><span class="sxs-lookup"><span data-stu-id="b3433-176">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="b3433-177">工作 3-建立索引檢視</span><span class="sxs-lookup"><span data-stu-id="b3433-177">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="b3433-178">在這個工作中，您將建立索引檢視範本，以顯示所傳回的相簿**StoreManager**控制站。</span><span class="sxs-lookup"><span data-stu-id="b3433-178">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="b3433-179">之前建立新的檢視表範本，您應該可以建置專案，讓**新增檢視對話方塊**知道**專輯**類別，才能使用。</span><span class="sxs-lookup"><span data-stu-id="b3433-179">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="b3433-180">選取**建置 |建置 MvcMusicStore**建置專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-180">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="b3433-181">以滑鼠右鍵按一下**索引**動作方法，然後選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="b3433-181">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="b3433-182">這會顯示**加入檢視**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="b3433-182">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="b3433-183">![加入檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "加入檢視")</span><span class="sxs-lookup"><span data-stu-id="b3433-183">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="b3433-184">*加入從索引方法內的檢視*</span><span class="sxs-lookup"><span data-stu-id="b3433-184">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="b3433-185">在 [新增檢視] 對話方塊中，確認檢視表名稱為**索引**。</span><span class="sxs-lookup"><span data-stu-id="b3433-185">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="b3433-186">選取**建立強型別檢視**選項，然後選取**專輯 (MvcMusicStore.Models)**從**模型類別**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="b3433-186">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="b3433-187">選取**清單**從**Scaffold 範本**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="b3433-187">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="b3433-188">保留**檢視引擎**至**Razor** ，其預設值的其他欄位值，然後按**新增**。</span><span class="sxs-lookup"><span data-stu-id="b3433-188">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="b3433-189">![加入索引檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "加入索引檢視")</span><span class="sxs-lookup"><span data-stu-id="b3433-189">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="b3433-190">*加入索引檢視*</span><span class="sxs-lookup"><span data-stu-id="b3433-190">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="b3433-191">工作 4-自訂的索引檢視 scaffold</span><span class="sxs-lookup"><span data-stu-id="b3433-191">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="b3433-192">在這項工作，您將調整建立 ASP.NET MVC scaffolding 功能，讓它顯示您想要的欄位的簡單檢視範本。</span><span class="sxs-lookup"><span data-stu-id="b3433-192">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="b3433-193">**Scaffolding**支援 ASP.NET MVC 中的會產生列出專輯模型中的所有欄位的簡單檢視範本。</span><span class="sxs-lookup"><span data-stu-id="b3433-193">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="b3433-194">**Scaffolding**提供快速的方法，若要開始強型別檢視:，而不必手動撰寫檢視表範本的 scaffolding 快速產生的預設範本，然後您可以修改產生的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b3433-194">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="b3433-195">檢閱建立的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b3433-195">Review the code created.</span></span> <span data-ttu-id="b3433-196">產生的欄位清單將屬於下列 HTML 資料表，其中**Scaffolding**用來顯示表格式資料。</span><span class="sxs-lookup"><span data-stu-id="b3433-196">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="b3433-197">取代**&lt;資料表&gt;**只顯示為下列程式碼的程式碼**類型**，**演出者**，**專輯標題**，和**價格**欄位。</span><span class="sxs-lookup"><span data-stu-id="b3433-197">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="b3433-198">這會刪除**AlbumId**和**專輯封面 URL**資料行。</span><span class="sxs-lookup"><span data-stu-id="b3433-198">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="b3433-199">此外，它會變更 GenreId 和 ArtistId 的資料行，以顯示其連結的類別屬性**Artist.Name**和**Genre.Name**，並移除**詳細資料**連結。</span><span class="sxs-lookup"><span data-stu-id="b3433-199">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="b3433-200">變更下列說明。</span><span class="sxs-lookup"><span data-stu-id="b3433-200">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="b3433-201">工作 5-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b3433-201">Task 5 - Running the Application</span></span>

<span data-ttu-id="b3433-202">在這個工作中，您將測試的**StoreManager** **索引**檢視範本會顯示一份專輯根據上一個步驟的設計。</span><span class="sxs-lookup"><span data-stu-id="b3433-202">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="b3433-203">按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3433-203">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3433-204">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-204">The project starts in the Home page.</span></span> <span data-ttu-id="b3433-205">將 URL 變更為**/StoreManager**確認會顯示一份相簿，顯示其**標題**，**演出者**和**類型**。</span><span class="sxs-lookup"><span data-stu-id="b3433-205">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="b3433-206">![瀏覽相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "瀏覽相簿")</span><span class="sxs-lookup"><span data-stu-id="b3433-206">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="b3433-207">*瀏覽相簿*</span><span class="sxs-lookup"><span data-stu-id="b3433-207">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="b3433-208">練習 2： 加入 HTML Helper</span><span class="sxs-lookup"><span data-stu-id="b3433-208">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="b3433-209">StoreManager 索引頁有一個可能的問題： 標題和演出者名稱屬性可以同時為夠長，會擲回關閉資料表格式。</span><span class="sxs-lookup"><span data-stu-id="b3433-209">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="b3433-210">在這個練習中，您將學習如何加入自訂的 HTML helper 來截斷該文字。</span><span class="sxs-lookup"><span data-stu-id="b3433-210">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="b3433-211">在下圖中，您可以看到如何格式修改，因為文字的長度，而當您使用較小的瀏覽器的大小。</span><span class="sxs-lookup"><span data-stu-id="b3433-211">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="b3433-212">![瀏覽與專輯的清單不會被截斷文字](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "瀏覽與專輯的清單不會被截斷的文字")</span><span class="sxs-lookup"><span data-stu-id="b3433-212">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="b3433-213">*瀏覽與專輯的清單不會被截斷的文字*</span><span class="sxs-lookup"><span data-stu-id="b3433-213">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="b3433-214">工作 1-擴充的 HTML Helper</span><span class="sxs-lookup"><span data-stu-id="b3433-214">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="b3433-215">在這項工作，您會將新方法加入**Truncate**至**HTML** ASP.NET MVC 檢視中公開的物件。</span><span class="sxs-lookup"><span data-stu-id="b3433-215">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="b3433-216">若要這樣做，您會實作**擴充方法**內建**System.Web.Mvc.HtmlHelper** ASP.NET MVC 所提供的類別。</span><span class="sxs-lookup"><span data-stu-id="b3433-216">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="b3433-217">若要深入了解**擴充方法**，請瀏覽 msdn 上的本文。</span><span class="sxs-lookup"><span data-stu-id="b3433-217">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="b3433-218">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="b3433-218">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="b3433-219">開啟**開始**方案位於**來源/Ex2AddingAnHTMLHelper/開始/**資料夾。</span><span class="sxs-lookup"><span data-stu-id="b3433-219">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="b3433-220">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="b3433-220">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="b3433-221">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="b3433-221">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b3433-222">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="b3433-222">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="b3433-223">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="b3433-223">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="b3433-224">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="b3433-224">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3433-225">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="b3433-225">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b3433-226">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-226">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b3433-227">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="b3433-227">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b3433-228">開啟 StoreManager 的索引檢視。</span><span class="sxs-lookup"><span data-stu-id="b3433-228">Open StoreManager's Index View.</span></span> <span data-ttu-id="b3433-229">若要這樣做，請在 [方案總管] 中展開**檢視**資料夾，然後在**StoreManager**並開啟**Index.cshtml**檔案。</span><span class="sxs-lookup"><span data-stu-id="b3433-229">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="b3433-230">加入下列程式碼下方 **@model** 指示詞來定義**Truncate** helper 方法。</span><span class="sxs-lookup"><span data-stu-id="b3433-230">Add the following code below the **@model** directive to define the **Truncate** helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="b3433-231">工作 2-截斷網頁中的文字</span><span class="sxs-lookup"><span data-stu-id="b3433-231">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="b3433-232">在這個工作中，您將使用**Truncate**截斷檢視範本中的文字的方法。</span><span class="sxs-lookup"><span data-stu-id="b3433-232">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="b3433-233">開啟 StoreManager 的索引檢視。</span><span class="sxs-lookup"><span data-stu-id="b3433-233">Open StoreManager's Index View.</span></span> <span data-ttu-id="b3433-234">若要這樣做，請在 [方案總管] 中展開**檢視**資料夾，然後在**StoreManager**並開啟**Index.cshtml**檔案。</span><span class="sxs-lookup"><span data-stu-id="b3433-234">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="b3433-235">取代顯示的行**演出者名稱**和專輯**標題**。</span><span class="sxs-lookup"><span data-stu-id="b3433-235">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="b3433-236">若要這樣做，請取代下列幾行。</span><span class="sxs-lookup"><span data-stu-id="b3433-236">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="b3433-237">工作 3-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b3433-237">Task 3 - Running the Application</span></span>

<span data-ttu-id="b3433-238">在這個工作中，您將測試的**StoreManager** **索引**檢視範本會截斷的專輯標題和演出者名稱。</span><span class="sxs-lookup"><span data-stu-id="b3433-238">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="b3433-239">按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3433-239">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3433-240">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-240">The project starts in the Home page.</span></span> <span data-ttu-id="b3433-241">將 URL 變更為**/StoreManager**以確認該長時間中的文字**標題**和**演出者**資料行就會被截斷。</span><span class="sxs-lookup"><span data-stu-id="b3433-241">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="b3433-242">![截斷標題和演出者名稱](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "截斷標題和演出者名稱")</span><span class="sxs-lookup"><span data-stu-id="b3433-242">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="b3433-243">*截斷的標題和演出者名稱*</span><span class="sxs-lookup"><span data-stu-id="b3433-243">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="b3433-244">練習 3： 建立編輯檢視</span><span class="sxs-lookup"><span data-stu-id="b3433-244">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="b3433-245">在此練習中，您將學習如何建立表單，以允許存放區管理員，以編輯專輯。</span><span class="sxs-lookup"><span data-stu-id="b3433-245">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="b3433-246">將會在瀏覽**/StoreManager/Edit/id** URL (**識別碼**正在編輯專輯的唯一識別碼)，因而讓伺服器的 HTTP GET 呼叫。</span><span class="sxs-lookup"><span data-stu-id="b3433-246">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="b3433-247">編輯控制器動作方法會從資料庫擷取適當的專輯、 建立**StoreManagerViewModel**物件封裝 （以及演出者與內容類型的清單），然後將它，傳遞至檢視範本呈現的 HTML 網頁傳回給使用者。</span><span class="sxs-lookup"><span data-stu-id="b3433-247">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="b3433-248">此頁面將包含**&lt;表單&gt;**具有文字方塊和下拉式清單編輯專輯屬性的項目。</span><span class="sxs-lookup"><span data-stu-id="b3433-248">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="b3433-249">一旦使用者更新專輯表單值，並按一下**儲存** 按鈕，將變更送出透過 HTTP POST 呼叫回**/StoreManager/Edit/id**。雖然 URL 會保留最後一次呼叫相同，ASP.NET MVC 識別，這次它是 HTTP POST，因此會執行不同的編輯動作方法 (一個使用裝飾**[HttpPost]**)。</span><span class="sxs-lookup"><span data-stu-id="b3433-249">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="b3433-250">工作 1-實作 HTTP GET 編輯動作方法</span><span class="sxs-lookup"><span data-stu-id="b3433-250">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="b3433-251">在這個工作中，您將實作要擷取從資料庫中的適當相簿的編輯動作方法的 HTTP GET 版本以及所有的內容類型和演出者名稱清單。</span><span class="sxs-lookup"><span data-stu-id="b3433-251">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="b3433-252">它會封裝此資料到**StoreManagerViewModel**的最後一個步驟，然後將傳遞至轉譯與回應的檢視範本中定義的物件。</span><span class="sxs-lookup"><span data-stu-id="b3433-252">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="b3433-253">開啟**開始**方案位於**來源/Ex3CreatingTheEditView/開始/**資料夾。</span><span class="sxs-lookup"><span data-stu-id="b3433-253">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="b3433-254">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="b3433-254">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="b3433-255">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="b3433-255">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b3433-256">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="b3433-256">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="b3433-257">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="b3433-257">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="b3433-258">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="b3433-258">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3433-259">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="b3433-259">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b3433-260">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-260">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b3433-261">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="b3433-261">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b3433-262">開啟**StoreManagerController**類別。</span><span class="sxs-lookup"><span data-stu-id="b3433-262">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="b3433-263">若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。</span><span class="sxs-lookup"><span data-stu-id="b3433-263">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="b3433-264">取代**HTTP-GET 編輯**動作方法，以下列程式碼來擷取適當**專輯**以及**內容類型**和**演出者**列出。</span><span class="sxs-lookup"><span data-stu-id="b3433-264">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="b3433-265">(程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex3 StoreManagerController HTTP-GET 編輯動作*)</span><span class="sxs-lookup"><span data-stu-id="b3433-265">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="b3433-266">您使用**System.Web.Mvc** **SelectList**演出者和內容類型，而不是**System.Collections.Generic**清單。</span><span class="sxs-lookup"><span data-stu-id="b3433-266">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="b3433-267">**SelectList**是清楚的方式，來填入 HTML 下拉式清單和管理目前選取範圍等。</span><span class="sxs-lookup"><span data-stu-id="b3433-267">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="b3433-268">具現化及更新版本中控制器動作的這些 ViewModel 物件設定將編輯表單案例清理。</span><span class="sxs-lookup"><span data-stu-id="b3433-268">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="b3433-269">工作 2-建立編輯檢視</span><span class="sxs-lookup"><span data-stu-id="b3433-269">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="b3433-270">在這項工作，您將建立稍後將會顯示專輯屬性編輯檢視範本。</span><span class="sxs-lookup"><span data-stu-id="b3433-270">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="b3433-271">建立編輯檢視。</span><span class="sxs-lookup"><span data-stu-id="b3433-271">Create the Edit View.</span></span> <span data-ttu-id="b3433-272">若要這樣做，以滑鼠右鍵按一下**編輯**動作方法，然後選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="b3433-272">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="b3433-273">在 [新增檢視] 對話方塊中，確認檢視表名稱為**編輯**。</span><span class="sxs-lookup"><span data-stu-id="b3433-273">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="b3433-274">請檢查**建立強型別檢視**核取方塊並選取**專輯 (MvcMusicStore.Models)**從**檢視資料類別**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="b3433-274">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="b3433-275">選取**編輯**從**Scaffold 範本**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="b3433-275">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="b3433-276">保留其他欄位的預設值，然後按**新增**。</span><span class="sxs-lookup"><span data-stu-id="b3433-276">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="b3433-277">![加入編輯檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "加入編輯檢視")</span><span class="sxs-lookup"><span data-stu-id="b3433-277">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="b3433-278">*加入編輯檢視*</span><span class="sxs-lookup"><span data-stu-id="b3433-278">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="b3433-279">工作 3-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b3433-279">Task 3 - Running the Application</span></span>

<span data-ttu-id="b3433-280">在這個工作中，您將測試的**StoreManager** **編輯**檢視頁面會顯示屬性的值當做參數傳遞的相簿。</span><span class="sxs-lookup"><span data-stu-id="b3433-280">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="b3433-281">按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3433-281">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3433-282">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-282">The project starts in the Home page.</span></span> <span data-ttu-id="b3433-283">將 URL 變更為**/StoreManager/Edit/1**以確認已顯示屬性的值傳遞的相簿。</span><span class="sxs-lookup"><span data-stu-id="b3433-283">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="b3433-284">![瀏覽專輯的編輯檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "瀏覽專輯的編輯檢視")</span><span class="sxs-lookup"><span data-stu-id="b3433-284">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="b3433-285">*瀏覽專輯的編輯檢視*</span><span class="sxs-lookup"><span data-stu-id="b3433-285">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="b3433-286">工作 4-實作專輯編輯器範本上的下拉式清單</span><span class="sxs-lookup"><span data-stu-id="b3433-286">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="b3433-287">在這個工作中，您將會加入下拉式清單中最後一個工作中，建立的檢視範本，讓使用者可以選取從演出者與內容類型的清單。</span><span class="sxs-lookup"><span data-stu-id="b3433-287">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="b3433-288">全部取代**專輯**fieldset 以下列的程式碼：</span><span class="sxs-lookup"><span data-stu-id="b3433-288">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="b3433-289">**Html.DropDownList** helper 已新增至呈現下拉式清單選擇演出者與內容類型。</span><span class="sxs-lookup"><span data-stu-id="b3433-289">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="b3433-290">參數傳遞至**Html.DropDownList**是：</span><span class="sxs-lookup"><span data-stu-id="b3433-290">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="b3433-291">表單欄位的名稱 (**&quot;ArtistId&quot;**)。</span><span class="sxs-lookup"><span data-stu-id="b3433-291">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="b3433-292">**SelectList**之下拉式清單的值。</span><span class="sxs-lookup"><span data-stu-id="b3433-292">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="b3433-293">工作 5-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b3433-293">Task 5 - Running the Application</span></span>

<span data-ttu-id="b3433-294">在這個工作中，您將測試的**StoreManager** **編輯**檢視頁面會顯示下拉式清單，而不是演出者 和 類型識別碼 文字欄位。</span><span class="sxs-lookup"><span data-stu-id="b3433-294">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="b3433-295">按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3433-295">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3433-296">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-296">The project starts in the Home page.</span></span> <span data-ttu-id="b3433-297">將 URL 變更為**/StoreManager/Edit/1**以確認它會顯示下拉式清單，而不是演出者 和 類型識別碼 文字欄位。</span><span class="sxs-lookup"><span data-stu-id="b3433-297">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="b3433-298">![瀏覽專輯的下拉式清單與編輯檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "具有下拉式清單的 瀏覽專輯編輯檢視")</span><span class="sxs-lookup"><span data-stu-id="b3433-298">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="b3433-299">*瀏覽專輯編輯檢視，這次具有下拉式清單*</span><span class="sxs-lookup"><span data-stu-id="b3433-299">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="b3433-300">工作 6： 實作 HTTP POST 編輯動作方法</span><span class="sxs-lookup"><span data-stu-id="b3433-300">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="b3433-301">現在，編輯檢視會顯示如預期般，您需要實作 HTTP POST 編輯動作方法，以將變更儲存至相簿。</span><span class="sxs-lookup"><span data-stu-id="b3433-301">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="b3433-302">關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。</span><span class="sxs-lookup"><span data-stu-id="b3433-302">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="b3433-303">開啟**StoreManagerController**從**控制器**資料夾。</span><span class="sxs-lookup"><span data-stu-id="b3433-303">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="b3433-304">取代**HTTP POST 編輯**動作方法的程式碼以下列 （請注意，您必須將取代的方法接收兩個參數的多載的版本）：</span><span class="sxs-lookup"><span data-stu-id="b3433-304">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="b3433-305">(程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex3 StoreManagerController HTTP POST 編輯動作*)</span><span class="sxs-lookup"><span data-stu-id="b3433-305">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="b3433-306">這個方法將會執行，當使用者按一下**儲存**檢視的按鈕，並執行 HTTP-張貼至伺服器的表單值將它們保存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="b3433-306">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="b3433-307">Decorator **[HttpPost]**表示方法應用於 HTTP POST 的那些案例。</span><span class="sxs-lookup"><span data-stu-id="b3433-307">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="b3433-308">這個方法會接受**專輯**物件。</span><span class="sxs-lookup"><span data-stu-id="b3433-308">The method takes an **Album** object.</span></span> <span data-ttu-id="b3433-309">ASP.NET MVC 會自動建立專輯物件，從張貼&lt;表單&gt;值。</span><span class="sxs-lookup"><span data-stu-id="b3433-309">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="b3433-310">方法會執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="b3433-310">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="b3433-311">如果模型有效：</span><span class="sxs-lookup"><span data-stu-id="b3433-311">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="b3433-312">更新專輯中的項目將其標示為已修改物件的內容。</span><span class="sxs-lookup"><span data-stu-id="b3433-312">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="b3433-313">儲存變更並重新導向至索引檢視。</span><span class="sxs-lookup"><span data-stu-id="b3433-313">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="b3433-314">如果模型不是有效的它會填入與 ViewBag **GenreId**和**ArtistId**，則它會傳回與所接收的專輯物件，讓使用者檢視執行任何必要的更新。</span><span class="sxs-lookup"><span data-stu-id="b3433-314">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="b3433-315">工作 7： 執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b3433-315">Task 7 - Running the Application</span></span>

<span data-ttu-id="b3433-316">在這個工作中，您將測試的**StoreManager 編輯**檢視頁面實際上會將更新的專輯資料儲存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="b3433-316">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="b3433-317">按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3433-317">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3433-318">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-318">The project starts in the Home page.</span></span> <span data-ttu-id="b3433-319">將 URL 變更為**/StoreManager/Edit/1**。</span><span class="sxs-lookup"><span data-stu-id="b3433-319">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="b3433-320">將專輯標題來變更**負載**，然後按一下 **儲存**。</span><span class="sxs-lookup"><span data-stu-id="b3433-320">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="b3433-321">請確認專輯標題實際變更清單中的專輯。</span><span class="sxs-lookup"><span data-stu-id="b3433-321">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="b3433-322">![更新專輯](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "更新相簿")</span><span class="sxs-lookup"><span data-stu-id="b3433-322">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="b3433-323">*更新相簿*</span><span class="sxs-lookup"><span data-stu-id="b3433-323">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="b3433-324">練習 4： 加入建立檢視</span><span class="sxs-lookup"><span data-stu-id="b3433-324">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="b3433-325">既然**StoreManagerController**支援**編輯**能力，在此練習，您將學習如何加入建立檢視，讓儲存範本管理員將新專輯新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3433-325">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="b3433-326">如同具有編輯功能，您將要實作建立案例中，使用兩個不同的方法內**StoreManagerController**類別：</span><span class="sxs-lookup"><span data-stu-id="b3433-326">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="b3433-327">當存放區管理員第一次瀏覽一個動作方法會顯示一個空白表單**/StoreManager/建立**URL。</span><span class="sxs-lookup"><span data-stu-id="b3433-327">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="b3433-328">第二個動作方法將處理案例的存放區管理員以便按一下**儲存**按鈕中的表單，並將送回給的值**/StoreManager/建立**URL 為 HTTP POST。</span><span class="sxs-lookup"><span data-stu-id="b3433-328">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="b3433-329">工作 1-執行 HTTP GET 建立動作方法</span><span class="sxs-lookup"><span data-stu-id="b3433-329">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="b3433-330">在這項工作，您將要實作建立動作方法，來擷取所有的內容類型和演出者名稱清單，請到封裝這項資料的 HTTP GET 新版**StoreManagerViewModel**物件，然後將傳遞至檢視範本。</span><span class="sxs-lookup"><span data-stu-id="b3433-330">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="b3433-331">開啟**開始**方案位於**來源/Ex4AddingACreateView/開始/**資料夾。</span><span class="sxs-lookup"><span data-stu-id="b3433-331">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="b3433-332">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="b3433-332">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="b3433-333">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="b3433-333">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b3433-334">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="b3433-334">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="b3433-335">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="b3433-335">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="b3433-336">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="b3433-336">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3433-337">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="b3433-337">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b3433-338">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-338">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b3433-339">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="b3433-339">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b3433-340">開啟**StoreManagerController**類別。</span><span class="sxs-lookup"><span data-stu-id="b3433-340">Open **StoreManagerController** class.</span></span> <span data-ttu-id="b3433-341">若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。</span><span class="sxs-lookup"><span data-stu-id="b3433-341">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="b3433-342">取代**建立**動作方法的程式碼以下列：</span><span class="sxs-lookup"><span data-stu-id="b3433-342">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="b3433-343">(程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex4 StoreManagerController HTTP-GET 建立動作*)</span><span class="sxs-lookup"><span data-stu-id="b3433-343">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="b3433-344">工作 2-加入建立檢視</span><span class="sxs-lookup"><span data-stu-id="b3433-344">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="b3433-345">在這項工作，您將加入將會顯示新的 （空的） 專輯表單建立檢視範本。</span><span class="sxs-lookup"><span data-stu-id="b3433-345">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="b3433-346">以滑鼠右鍵按一下**建立**動作方法，然後選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="b3433-346">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="b3433-347">這會顯示 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="b3433-347">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="b3433-348">在 [新增檢視] 對話方塊中，確認檢視表名稱為**建立**。</span><span class="sxs-lookup"><span data-stu-id="b3433-348">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="b3433-349">選取**建立強型別檢視**選項，然後選取**專輯 (MvcMusicStore.Models)**從**模型類別**下拉式清單和**建立**從**Scaffold 範本**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="b3433-349">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="b3433-350">保留其他欄位的預設值，然後按**新增**。</span><span class="sxs-lookup"><span data-stu-id="b3433-350">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="b3433-351">![加入建立檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "加入-a-建立-view.png")</span><span class="sxs-lookup"><span data-stu-id="b3433-351">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="b3433-352">*加入建立檢視*</span><span class="sxs-lookup"><span data-stu-id="b3433-352">*Adding the Create View*</span></span>
3. <span data-ttu-id="b3433-353">更新**GenreId**和**ArtistId**欄位，使用下拉式清單，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b3433-353">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="b3433-354">工作 3-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b3433-354">Task 3 - Running the Application</span></span>

<span data-ttu-id="b3433-355">在這個工作中，您將測試的**StoreManager** **建立**檢視頁面會顯示一個空白的專輯表單。</span><span class="sxs-lookup"><span data-stu-id="b3433-355">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="b3433-356">按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3433-356">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3433-357">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-357">The project starts in the Home page.</span></span> <span data-ttu-id="b3433-358">將 URL 變更為**/StoreManager/建立**。</span><span class="sxs-lookup"><span data-stu-id="b3433-358">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="b3433-359">請確認的空白表單隨即填入新的專輯屬性。</span><span class="sxs-lookup"><span data-stu-id="b3433-359">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="b3433-360">![建立一個空白表單檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "建立一個空白表單檢視")</span><span class="sxs-lookup"><span data-stu-id="b3433-360">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="b3433-361">*建立一個空白表單檢視*</span><span class="sxs-lookup"><span data-stu-id="b3433-361">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="b3433-362">工作 4-實作 HTTP POST 建立動作方法</span><span class="sxs-lookup"><span data-stu-id="b3433-362">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="b3433-363">在這項工作，您將要實作建立動作方法會叫用，當使用者按一下的 HTTP POST 新版**儲存** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b3433-363">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="b3433-364">方法應該在資料庫中儲存新的相簿。</span><span class="sxs-lookup"><span data-stu-id="b3433-364">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="b3433-365">關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。</span><span class="sxs-lookup"><span data-stu-id="b3433-365">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="b3433-366">開啟**StoreManagerController**類別。</span><span class="sxs-lookup"><span data-stu-id="b3433-366">Open **StoreManagerController** class.</span></span> <span data-ttu-id="b3433-367">若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。</span><span class="sxs-lookup"><span data-stu-id="b3433-367">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="b3433-368">取代**HTTP POST 建立**動作方法的程式碼以下列：</span><span class="sxs-lookup"><span data-stu-id="b3433-368">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="b3433-369">(程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex4 StoreManagerController HTTP POST 建立動作*)</span><span class="sxs-lookup"><span data-stu-id="b3433-369">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="b3433-370">建立動作方法和上一個編輯動作很類似，但而不是為已修改，請設定物件，它會加入至內容。</span><span class="sxs-lookup"><span data-stu-id="b3433-370">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="b3433-371">工作 5-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b3433-371">Task 5 - Running the Application</span></span>

<span data-ttu-id="b3433-372">在這個工作中，您將測試的**StoreManager 建立**檢視頁面可讓您建立新的相簿，然後重新導向至 StoreManager 索引檢視。</span><span class="sxs-lookup"><span data-stu-id="b3433-372">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="b3433-373">按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3433-373">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3433-374">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-374">The project starts in the Home page.</span></span> <span data-ttu-id="b3433-375">將 URL 變更為**/StoreManager/建立**。</span><span class="sxs-lookup"><span data-stu-id="b3433-375">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="b3433-376">填入資料的所有表單欄位新專輯、 類似下圖中的一項：</span><span class="sxs-lookup"><span data-stu-id="b3433-376">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="b3433-377">![建立相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "建立相簿")</span><span class="sxs-lookup"><span data-stu-id="b3433-377">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="b3433-378">*建立相簿*</span><span class="sxs-lookup"><span data-stu-id="b3433-378">*Creating an Album*</span></span>
3. <span data-ttu-id="b3433-379">請確認您取得重新導向至包含剛才建立的新專輯 StoreManager 索引檢視。</span><span class="sxs-lookup"><span data-stu-id="b3433-379">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="b3433-380">![建立新的相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "新建立的相簿")</span><span class="sxs-lookup"><span data-stu-id="b3433-380">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="b3433-381">*建立新的相簿*</span><span class="sxs-lookup"><span data-stu-id="b3433-381">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="b3433-382">練習 5： 處理刪除</span><span class="sxs-lookup"><span data-stu-id="b3433-382">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="b3433-383">刪除相簿的功能尚未實作。</span><span class="sxs-lookup"><span data-stu-id="b3433-383">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="b3433-384">這是本練習將會相關。</span><span class="sxs-lookup"><span data-stu-id="b3433-384">This is what this exercise will be about.</span></span> <span data-ttu-id="b3433-385">像之前，您會實作使用兩個不同的方法內的 Delete 案例**StoreManagerController**類別：</span><span class="sxs-lookup"><span data-stu-id="b3433-385">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="b3433-386">其中一個動作方法將會顯示確認表單</span><span class="sxs-lookup"><span data-stu-id="b3433-386">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="b3433-387">第二個動作方法會處理送出表單</span><span class="sxs-lookup"><span data-stu-id="b3433-387">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="b3433-388">工作 1-實作 HTTP GET Delete 動作方法</span><span class="sxs-lookup"><span data-stu-id="b3433-388">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="b3433-389">在這個工作中，您將實作要擷取的專輯資訊的刪除動作方法的 HTTP GET 版本。</span><span class="sxs-lookup"><span data-stu-id="b3433-389">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="b3433-390">開啟**開始**方案位於**來源/Ex5HandlingDeletion/開始/**資料夾。</span><span class="sxs-lookup"><span data-stu-id="b3433-390">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="b3433-391">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="b3433-391">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="b3433-392">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="b3433-392">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b3433-393">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="b3433-393">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="b3433-394">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="b3433-394">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="b3433-395">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="b3433-395">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3433-396">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="b3433-396">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b3433-397">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-397">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b3433-398">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="b3433-398">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b3433-399">開啟**StoreManagerController**類別。</span><span class="sxs-lookup"><span data-stu-id="b3433-399">Open **StoreManagerController** class.</span></span> <span data-ttu-id="b3433-400">若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。</span><span class="sxs-lookup"><span data-stu-id="b3433-400">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="b3433-401">刪除控制器動作正是先前存放區詳細資料控制器動作相同： 它會查詢**專輯**物件從資料庫使用**識別碼**中提供 URL 與傳回適當**檢視**。</span><span class="sxs-lookup"><span data-stu-id="b3433-401">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="b3433-402">若要這樣做，取代 HTTP GET**刪除**動作方法的程式碼以下列：</span><span class="sxs-lookup"><span data-stu-id="b3433-402">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="b3433-403">(程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex5 處理刪除 HTTP-GET 刪除動作*)</span><span class="sxs-lookup"><span data-stu-id="b3433-403">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="b3433-404">以滑鼠右鍵按一下**刪除**動作方法，然後選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="b3433-404">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="b3433-405">這會顯示 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="b3433-405">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="b3433-406">在 [新增檢視] 對話方塊中，確認檢視表名稱為**刪除**。</span><span class="sxs-lookup"><span data-stu-id="b3433-406">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="b3433-407">選取**建立強型別檢視**選項，然後選取**專輯 (MvcMusicStore.Models)**從**模型類別**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="b3433-407">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="b3433-408">選取**刪除**從**Scaffold 範本**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="b3433-408">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="b3433-409">保留其他欄位的預設值，然後按**新增**。</span><span class="sxs-lookup"><span data-stu-id="b3433-409">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="b3433-410">![加入刪除檢視](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "加入刪除檢視")</span><span class="sxs-lookup"><span data-stu-id="b3433-410">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="b3433-411">*加入刪除檢視*</span><span class="sxs-lookup"><span data-stu-id="b3433-411">*Adding a Delete View*</span></span>
6. <span data-ttu-id="b3433-412">刪除範本會顯示模型中的所有欄位。</span><span class="sxs-lookup"><span data-stu-id="b3433-412">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="b3433-413">您將會顯示只有專輯標題。</span><span class="sxs-lookup"><span data-stu-id="b3433-413">You will show only the album's title.</span></span> <span data-ttu-id="b3433-414">若要這樣做，請取代下列程式碼中檢視的內容：</span><span class="sxs-lookup"><span data-stu-id="b3433-414">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="b3433-415">工作 2-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b3433-415">Task 2 - Running the Application</span></span>

<span data-ttu-id="b3433-416">在這個工作中，您將測試的**StoreManager** **刪除**檢視頁面會顯示確認刪除表單。</span><span class="sxs-lookup"><span data-stu-id="b3433-416">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="b3433-417">按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3433-417">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3433-418">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-418">The project starts in the Home page.</span></span> <span data-ttu-id="b3433-419">將 URL 變更為**/StoreManager**。</span><span class="sxs-lookup"><span data-stu-id="b3433-419">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="b3433-420">選取即可刪除一個專輯**刪除**並驗證已上傳新的檢視。</span><span class="sxs-lookup"><span data-stu-id="b3433-420">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="b3433-421">![刪除相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "刪除相簿")</span><span class="sxs-lookup"><span data-stu-id="b3433-421">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="b3433-422">*刪除相簿*</span><span class="sxs-lookup"><span data-stu-id="b3433-422">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="b3433-423">工作 3-實作 HTTP POST 的 Delete 動作方法</span><span class="sxs-lookup"><span data-stu-id="b3433-423">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="b3433-424">在這個工作中，您將實作當使用者按一下就會叫用的 Delete 動作方法的 HTTP POST 新版**刪除** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b3433-424">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="b3433-425">此方法應該刪除資料庫中的專輯。</span><span class="sxs-lookup"><span data-stu-id="b3433-425">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="b3433-426">關閉瀏覽器，如有需要若要返回 Visual Studio 視窗。</span><span class="sxs-lookup"><span data-stu-id="b3433-426">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="b3433-427">開啟**StoreManagerController**類別。</span><span class="sxs-lookup"><span data-stu-id="b3433-427">Open **StoreManagerController** class.</span></span> <span data-ttu-id="b3433-428">若要這樣做，請展開**控制器**資料夾，然後按兩下**StoreManagerController.cs**。</span><span class="sxs-lookup"><span data-stu-id="b3433-428">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="b3433-429">取代**HTTP POST 刪除**動作方法的程式碼以下列：</span><span class="sxs-lookup"><span data-stu-id="b3433-429">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="b3433-430">(程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex5 處理刪除 HTTP POST 刪除動作*)</span><span class="sxs-lookup"><span data-stu-id="b3433-430">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="b3433-431">工作 4-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b3433-431">Task 4 - Running the Application</span></span>

<span data-ttu-id="b3433-432">在這個工作中，您將測試的**StoreManager 刪除**檢視頁面可讓您刪除相簿，然後重新導向至 StoreManager 索引檢視。</span><span class="sxs-lookup"><span data-stu-id="b3433-432">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="b3433-433">按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3433-433">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3433-434">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-434">The project starts in the Home page.</span></span> <span data-ttu-id="b3433-435">將 URL 變更為**/StoreManager**。</span><span class="sxs-lookup"><span data-stu-id="b3433-435">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="b3433-436">選取即可刪除一個專輯**刪除。**</span><span class="sxs-lookup"><span data-stu-id="b3433-436">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="b3433-437">按一下以確認刪除動作**刪除**按鈕：</span><span class="sxs-lookup"><span data-stu-id="b3433-437">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="b3433-438">![刪除相簿](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "刪除相簿")</span><span class="sxs-lookup"><span data-stu-id="b3433-438">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="b3433-439">*刪除相簿*</span><span class="sxs-lookup"><span data-stu-id="b3433-439">*Deleting an Album*</span></span>
3. <span data-ttu-id="b3433-440">確認已刪除的專輯，因為它不會顯示在**索引**頁面。</span><span class="sxs-lookup"><span data-stu-id="b3433-440">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="b3433-441">練習 6： 加入驗證</span><span class="sxs-lookup"><span data-stu-id="b3433-441">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="b3433-442">目前，您已備妥的建立與編輯表單不會執行任何一種驗證。</span><span class="sxs-lookup"><span data-stu-id="b3433-442">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="b3433-443">如果使用者必要的欄位空白或型別字母離開 [價格] 欄位中，就會在第一個錯誤將會從資料庫中。</span><span class="sxs-lookup"><span data-stu-id="b3433-443">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="b3433-444">您可以加入至應用程式的驗證，將資料註解加入至您的模型類別。</span><span class="sxs-lookup"><span data-stu-id="b3433-444">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="b3433-445">資料註解允許描述您想要套用至您的模型內容的規則和 ASP.NET MVC 會負責強制執行和顯示適當的訊息給使用者。</span><span class="sxs-lookup"><span data-stu-id="b3433-445">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="b3433-446">工作 1-新增資料註解</span><span class="sxs-lookup"><span data-stu-id="b3433-446">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="b3433-447">在這項工作，您將加入資料註解專輯模型，可讓建立與編輯頁面顯示時適當的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="b3433-447">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="b3433-448">針對簡單的模型類別，加入資料註解只處理所加入**使用**陳述式**System.ComponentModel.DataAnnotation**，然後放置**[必要]**上適當的內容屬性。</span><span class="sxs-lookup"><span data-stu-id="b3433-448">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="b3433-449">下列範例會使**名稱**屬性在檢視中的必要欄位。</span><span class="sxs-lookup"><span data-stu-id="b3433-449">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="b3433-450">這是稍微複雜，就像此應用程式的情況下實體資料模型，產生的位置。</span><span class="sxs-lookup"><span data-stu-id="b3433-450">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="b3433-451">如果您資料註解直接加入模型類別，它們就會覆寫更新資料庫中的模型。</span><span class="sxs-lookup"><span data-stu-id="b3433-451">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="b3433-452">相反地，您可使用它來保存 註解會存在，並與模型相關聯的中繼資料部分類別的類別使用**[MetadataType]**屬性。</span><span class="sxs-lookup"><span data-stu-id="b3433-452">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="b3433-453">開啟**開始**方案位於**來源/Ex6AddingValidation/開始/**資料夾。</span><span class="sxs-lookup"><span data-stu-id="b3433-453">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="b3433-454">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="b3433-454">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="b3433-455">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="b3433-455">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b3433-456">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="b3433-456">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="b3433-457">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="b3433-457">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="b3433-458">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="b3433-458">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3433-459">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="b3433-459">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b3433-460">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-460">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b3433-461">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="b3433-461">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b3433-462">開啟**Album.cs**從**模型**資料夾。</span><span class="sxs-lookup"><span data-stu-id="b3433-462">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="b3433-463">取代**Album.cs**內容以反白顯示的程式碼，使它看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="b3433-463">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3433-464">線條**[DisplayFormat(ConvertEmptyStringToNull=false)]**代表模型中的空字串不會轉換為 null 的資料來源中更新資料欄位時。</span><span class="sxs-lookup"><span data-stu-id="b3433-464">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="b3433-465">Entity Framework 會指派至模型 null 值之前資料註解驗證欄位時，此設定可避免例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b3433-465">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="b3433-466">(程式碼片段- *ASP.NET MVC 4 Helper 和表單驗證-Ex6 專輯中繼資料的部分類別*)</span><span class="sxs-lookup"><span data-stu-id="b3433-466">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="b3433-467">這**專輯**部分類別具有**MetadataType**屬性指向**AlbumMetaData**資料註解的類別。</span><span class="sxs-lookup"><span data-stu-id="b3433-467">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="b3433-468">以下是一些您用來標註專輯模型的資料註解屬性：</span><span class="sxs-lookup"><span data-stu-id="b3433-468">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="b3433-469">必要-表示此屬性是必要的欄位</span><span class="sxs-lookup"><span data-stu-id="b3433-469">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="b3433-470">DisplayName-定義要用於表單欄位和驗證訊息的文字</span><span class="sxs-lookup"><span data-stu-id="b3433-470">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="b3433-471">DisplayFormat-指定資料欄位會顯示並格式化的方式。</span><span class="sxs-lookup"><span data-stu-id="b3433-471">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="b3433-472">StringLength-定義的字串欄位的最大長度</span><span class="sxs-lookup"><span data-stu-id="b3433-472">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="b3433-473">範圍-提供數值欄位的最大和最小值</span><span class="sxs-lookup"><span data-stu-id="b3433-473">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="b3433-474">ScaffoldColumn-可讓隱藏編輯器表單中的欄位</span><span class="sxs-lookup"><span data-stu-id="b3433-474">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="b3433-475">工作 2-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b3433-475">Task 2 - Running the Application</span></span>

<span data-ttu-id="b3433-476">在這項工作，您將測試建立與編輯頁面驗證欄位，使用最後一個工作中選擇的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="b3433-476">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="b3433-477">按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3433-477">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3433-478">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-478">The project starts in the Home page.</span></span> <span data-ttu-id="b3433-479">將 URL 變更為**/StoreManager/建立**。</span><span class="sxs-lookup"><span data-stu-id="b3433-479">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="b3433-480">確認顯示名稱符合的部分類別中的項目 (例如**專輯封面 URL**而不是**AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="b3433-480">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="b3433-481">按一下**建立**，而不填滿表單。</span><span class="sxs-lookup"><span data-stu-id="b3433-481">Click **Create**, without filling the form.</span></span> <span data-ttu-id="b3433-482">請確認您取得對應的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="b3433-482">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="b3433-483">![驗證 [建立] 頁面中的欄位](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "驗證 [建立] 頁面中的欄位")</span><span class="sxs-lookup"><span data-stu-id="b3433-483">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="b3433-484">*在 [建立] 頁面中驗證的欄位*</span><span class="sxs-lookup"><span data-stu-id="b3433-484">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="b3433-485">您可以確認相同發生**編輯**頁面。</span><span class="sxs-lookup"><span data-stu-id="b3433-485">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="b3433-486">將 URL 變更為**/StoreManager/Edit/1**並確認 顯示名稱符合的部分類別中的項目 (例如**專輯封面 URL**而不是**AlbumArtUrl**)。</span><span class="sxs-lookup"><span data-stu-id="b3433-486">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="b3433-487">空白**標題**和**價格**欄位和按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="b3433-487">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="b3433-488">請確認您取得對應的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="b3433-488">Verify that you get the corresponding validation messages.</span></span>

    ![驗證的欄位中編輯頁面](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="b3433-490">*驗證的欄位中編輯頁面*</span><span class="sxs-lookup"><span data-stu-id="b3433-490">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="b3433-491">練習 7： 在用戶端使用不顯眼的 jQuery</span><span class="sxs-lookup"><span data-stu-id="b3433-491">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="b3433-492">在此練習中，您將學習如何啟用 MVC 4 不顯眼的 jQuery 驗證用戶端。</span><span class="sxs-lookup"><span data-stu-id="b3433-492">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="b3433-493">不顯眼的 jQuery 會使用資料 ajax 首碼 JavaScript 來叫用伺服器，而非 intrusively 發出內嵌用戶端指令碼的動作方法。</span><span class="sxs-lookup"><span data-stu-id="b3433-493">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="b3433-494">工作 1-執行之前啟用不顯眼的 jQuery 應用程式</span><span class="sxs-lookup"><span data-stu-id="b3433-494">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="b3433-495">在這個工作中，您將執行的應用程式，才能包括 jQuery 才能比較這兩種驗證模式。</span><span class="sxs-lookup"><span data-stu-id="b3433-495">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="b3433-496">開啟**開始**方案位於**來源/Ex7UnobtrusivejQueryValidation/開始/**資料夾。</span><span class="sxs-lookup"><span data-stu-id="b3433-496">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="b3433-497">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="b3433-497">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="b3433-498">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="b3433-498">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b3433-499">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="b3433-499">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="b3433-500">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="b3433-500">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="b3433-501">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="b3433-501">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3433-502">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="b3433-502">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b3433-503">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-503">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b3433-504">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="b3433-504">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b3433-505">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3433-505">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="b3433-506">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-506">The project starts in the Home page.</span></span> <span data-ttu-id="b3433-507">瀏覽**/StoreManager/建立**按一下**建立**沒有填滿表單，以確認您看到驗證訊息：</span><span class="sxs-lookup"><span data-stu-id="b3433-507">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="b3433-508">![停用用戶端驗證](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "停用用戶端驗證")</span><span class="sxs-lookup"><span data-stu-id="b3433-508">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="b3433-509">*停用用戶端驗證*</span><span class="sxs-lookup"><span data-stu-id="b3433-509">*Client validation disabled*</span></span>
4. <span data-ttu-id="b3433-510">在瀏覽器中開啟 HTML 程式碼：</span><span class="sxs-lookup"><span data-stu-id="b3433-510">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="b3433-511">工作 2-啟用不顯眼的用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="b3433-511">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="b3433-512">在這個工作中，您將啟用 jQuery**不顯眼的用戶端驗證**從**Web.config**檔案，也就是預設值為 false，所有新的 ASP.NET MVC 4 專案中。</span><span class="sxs-lookup"><span data-stu-id="b3433-512">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="b3433-513">此外，您會加入必要的指令碼進行 jQuery 不顯眼的用戶端驗證工作的參考。</span><span class="sxs-lookup"><span data-stu-id="b3433-513">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="b3433-514">開啟**Web.Config**檔案在專案的根目錄，並確定**ClientValidationEnabled**和**UnobtrusiveJavaScriptEnabled**金鑰值都會設為**true**。</span><span class="sxs-lookup"><span data-stu-id="b3433-514">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="b3433-515">您也可以啟用用戶端驗證的程式碼中 Global.asax.cs 以取得相同的結果：</span><span class="sxs-lookup"><span data-stu-id="b3433-515">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="b3433-516">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="b3433-516">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="b3433-517">此外，您可以指派 ClientValidationEnabled 屬性到任何控制器有自訂的行為。</span><span class="sxs-lookup"><span data-stu-id="b3433-517">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="b3433-518">開啟**Create.cshtml**在**Views\StoreManager**。</span><span class="sxs-lookup"><span data-stu-id="b3433-518">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="b3433-519">下列指令碼檔案，請確定**jquery.validate**和**jquery.validate.unobtrusive**，都會在檢視透過&quot; **~/bundles/jqueryval**&quot;組合。</span><span class="sxs-lookup"><span data-stu-id="b3433-519">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="b3433-520">所有這些 jQuery 程式庫會包含在新的 MVC 4 專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-520">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="b3433-521">您可以找到更多程式庫中的**/指令碼**您專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b3433-521">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="b3433-522">若要讓這項驗證程式庫工作，您需要加入 jQuery framework 程式庫的參考。</span><span class="sxs-lookup"><span data-stu-id="b3433-522">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="b3433-523">因為中已加入此參考 **\_Layout.cshtml**檔案中，您不需要將它加入這個特定的檢視中。</span><span class="sxs-lookup"><span data-stu-id="b3433-523">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="b3433-524">工作 3-執行應用程式使用不顯眼的 jQuery 驗證</span><span class="sxs-lookup"><span data-stu-id="b3433-524">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="b3433-525">在這個工作中，您將測試的**StoreManager**建立範本執行的用戶端驗證使用 jQuery 程式庫，當使用者建立新的相簿的檢視。</span><span class="sxs-lookup"><span data-stu-id="b3433-525">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="b3433-526">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3433-526">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b3433-527">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="b3433-527">The project starts in the Home page.</span></span> <span data-ttu-id="b3433-528">瀏覽**/StoreManager/建立**按一下**建立**沒有填滿表單，以確認您看到驗證訊息：</span><span class="sxs-lookup"><span data-stu-id="b3433-528">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="b3433-529">![使用 jQuery 啟用的用戶端驗證](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "jQuery 啟用與用戶端驗證")</span><span class="sxs-lookup"><span data-stu-id="b3433-529">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="b3433-530">*使用 jQuery 啟用的用戶端驗證*</span><span class="sxs-lookup"><span data-stu-id="b3433-530">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="b3433-531">在瀏覽器中開啟 建立檢視的程式碼：</span><span class="sxs-lookup"><span data-stu-id="b3433-531">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > <span data-ttu-id="b3433-532">每個用戶端驗證規則不顯眼的 jQuery 加入資料屬性-val-*rulename*=&quot;*訊息*&quot;。</span><span class="sxs-lookup"><span data-stu-id="b3433-532">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="b3433-533">以下是一份標記該 Unobtrusive jQuery 插入 html 輸入欄位，以執行用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="b3433-533">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
    > 
    > - <span data-ttu-id="b3433-534">資料值</span><span class="sxs-lookup"><span data-stu-id="b3433-534">Data-val</span></span>
    > - <span data-ttu-id="b3433-535">Data-val-number</span><span class="sxs-lookup"><span data-stu-id="b3433-535">Data-val-number</span></span>
    > - <span data-ttu-id="b3433-536">資料 val 範圍</span><span class="sxs-lookup"><span data-stu-id="b3433-536">Data-val-range</span></span>
    > - <span data-ttu-id="b3433-537">資料 val 範圍-min/資料 val-range max</span><span class="sxs-lookup"><span data-stu-id="b3433-537">Data-val-range-min / Data-val-range-max</span></span>
    > - <span data-ttu-id="b3433-538">Data-val-required</span><span class="sxs-lookup"><span data-stu-id="b3433-538">Data-val-required</span></span>
    > - <span data-ttu-id="b3433-539">資料 val 長度</span><span class="sxs-lookup"><span data-stu-id="b3433-539">Data-val-length</span></span>
    > - <span data-ttu-id="b3433-540">資料值的長度上限/資料 val 長度-分鐘</span><span class="sxs-lookup"><span data-stu-id="b3433-540">Data-val-length-max / Data-val-length-min</span></span>
    > 
    > <span data-ttu-id="b3433-541">所有的資料值會填入模型**資料註解**。</span><span class="sxs-lookup"><span data-stu-id="b3433-541">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="b3433-542">然後，在伺服器端的所有邏輯可以都執行用戶端。</span><span class="sxs-lookup"><span data-stu-id="b3433-542">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="b3433-543">例如，價格屬性都具有下列資料註解模型中：</span><span class="sxs-lookup"><span data-stu-id="b3433-543">For example, Price attribute has the following data annotation in the model:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
    > 
    > <span data-ttu-id="b3433-544">使用不顯眼的 jQuery 之後, 產生的程式碼是：</span><span class="sxs-lookup"><span data-stu-id="b3433-544">After using Unobtrusive jQuery, the generated code is:</span></span>
    >  
    > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b3433-545">總結</span><span class="sxs-lookup"><span data-stu-id="b3433-545">Summary</span></span>

<span data-ttu-id="b3433-546">藉由完成這個實際操作實驗室中，您已經學會如何讓使用者變更儲存在資料庫中使用下列資料：</span><span class="sxs-lookup"><span data-stu-id="b3433-546">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="b3433-547">控制器動作，例如索引，建立、 編輯、 刪除</span><span class="sxs-lookup"><span data-stu-id="b3433-547">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="b3433-548">ASP.NET MVC 的 scaffolding 功能 HTML 表格中顯示屬性</span><span class="sxs-lookup"><span data-stu-id="b3433-548">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="b3433-549">自訂 HTML helper 來改善使用者體驗</span><span class="sxs-lookup"><span data-stu-id="b3433-549">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="b3433-550">回應 HTTP GET 或 HTTP POST 呼叫的動作方法</span><span class="sxs-lookup"><span data-stu-id="b3433-550">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="b3433-551">類似的檢視範本，例如建立與編輯共用的編輯器範本</span><span class="sxs-lookup"><span data-stu-id="b3433-551">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="b3433-552">下拉式清單類似的表單項目</span><span class="sxs-lookup"><span data-stu-id="b3433-552">Form elements like drop-downs</span></span>
- <span data-ttu-id="b3433-553">模型驗證的資料註解</span><span class="sxs-lookup"><span data-stu-id="b3433-553">Data annotations for Model validation</span></span>
- <span data-ttu-id="b3433-554">使用 jQuery 不顯眼的程式庫的用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="b3433-554">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b3433-555">附錄 a： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="b3433-555">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b3433-556">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="b3433-556">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b3433-557">下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="b3433-557">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b3433-558">移至[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="b3433-558">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b3433-559">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; *Visual Studio Express 2012 for Web 與 Windows Azure SDK*&quot;。</span><span class="sxs-lookup"><span data-stu-id="b3433-559">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="b3433-560">按一下**立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="b3433-560">Click on **Install Now**.</span></span> <span data-ttu-id="b3433-561">如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="b3433-561">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b3433-562">一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="b3433-562">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b3433-563">![安裝 Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="b3433-563">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b3433-564">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="b3433-564">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b3433-565">閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。</span><span class="sxs-lookup"><span data-stu-id="b3433-565">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="b3433-567">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="b3433-567">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b3433-568">等待直到完成下載和安裝程序。</span><span class="sxs-lookup"><span data-stu-id="b3433-568">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="b3433-570">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="b3433-570">*Installation progress*</span></span>
6. <span data-ttu-id="b3433-571">當安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="b3433-571">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="b3433-573">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="b3433-573">*Installation completed*</span></span>
7. <span data-ttu-id="b3433-574">按一下**結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="b3433-574">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b3433-575">若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。</span><span class="sxs-lookup"><span data-stu-id="b3433-575">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 方塊](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="b3433-577">*VS Express for Web 方塊*</span><span class="sxs-lookup"><span data-stu-id="b3433-577">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="b3433-578">附錄 b： 使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="b3433-578">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="b3433-579">程式碼片段，您會有您需要在您可以的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="b3433-579">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b3433-580">實驗室文件會告訴您完全時您可以使用它們，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="b3433-580">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b3433-581">![若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "使用 Visual Studio 程式碼片段，將程式碼插入您的專案")</span><span class="sxs-lookup"><span data-stu-id="b3433-581">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b3433-582">*若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="b3433-582">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="b3433-583">***若要加入的程式碼片段，使用鍵盤 （僅限 C#)***</span><span class="sxs-lookup"><span data-stu-id="b3433-583">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="b3433-584">將游標放在您想要插入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b3433-584">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b3433-585">開始鍵入片段名稱 （不含空格或連字號）。</span><span class="sxs-lookup"><span data-stu-id="b3433-585">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b3433-586">觀察 IntelliSense 會顯示比對程式碼片段的名稱。</span><span class="sxs-lookup"><span data-stu-id="b3433-586">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b3433-587">選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。</span><span class="sxs-lookup"><span data-stu-id="b3433-587">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b3433-588">按下 Tab 鍵兩次的游標位置插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="b3433-588">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="b3433-589">![開始輸入程式碼片段名稱](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "開始鍵入片段名稱")</span><span class="sxs-lookup"><span data-stu-id="b3433-589">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="b3433-590">*開始輸入程式碼片段名稱*</span><span class="sxs-lookup"><span data-stu-id="b3433-590">*Start typing the snippet name*</span></span>

<span data-ttu-id="b3433-591">![按 Tab 鍵選取反白顯示的程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "按 Tab 鍵以選取反白顯示的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="b3433-591">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="b3433-592">*按 Tab 鍵選取反白顯示的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="b3433-592">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="b3433-593">![按 Tab 鍵一次程式碼片段就會擴展](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "按 Tab 鍵一次程式碼片段就會擴展")</span><span class="sxs-lookup"><span data-stu-id="b3433-593">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="b3433-594">*按 Tab 鍵一次程式碼片段就會擴展*</span><span class="sxs-lookup"><span data-stu-id="b3433-594">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="b3433-595">***若要加入的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。</span><span class="sxs-lookup"><span data-stu-id="b3433-595">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="b3433-596">以滑鼠右鍵按一下您要插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="b3433-596">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="b3433-597">選取**插入程式碼片段**後面**我的程式碼片段**。</span><span class="sxs-lookup"><span data-stu-id="b3433-597">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="b3433-598">透過在按一下挑選清單中，從相關的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="b3433-598">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="b3433-599">![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="b3433-599">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="b3433-600">*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="b3433-600">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="b3433-601">![透過在按一下挑選清單中，從相關的程式碼片段](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "上它即可挑選清單中，從相關的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="b3433-601">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="b3433-602">*透過在按一下挑選清單中，從相關的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="b3433-602">*Pick the relevant snippet from the list, by clicking on it*</span></span>
