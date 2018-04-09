---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 模型和資料存取 |Microsoft 文件
author: rick-anderson
description: 注意： 這個實際操作實驗室假設您擁有的 ASP.NET MVC 的基本知識。 如果您沒有使用 ASP.NET MVC 之前，我們建議您前往透過 ASP.NET MVC 4...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 081a71ef67a6eee6c84058c30f9e15301afbed23
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="99307-104">ASP.NET MVC 4 模型和資料存取</span><span class="sxs-lookup"><span data-stu-id="99307-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="99307-105">由[Web 營小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="99307-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="99307-106">下載 Web 營訓練套件</span><span class="sxs-lookup"><span data-stu-id="99307-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="99307-107">這個實際操作實驗室假設您擁有的基本知識**ASP.NET MVC**。</span><span class="sxs-lookup"><span data-stu-id="99307-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="99307-108">如果您不使用**ASP.NET MVC**之前，我們建議您在一段**ASP.NET MVC 4 的基本概念**實際操作實驗室。</span><span class="sxs-lookup"><span data-stu-id="99307-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="99307-109">這個實驗室將引導您完成先前所述將微幅變更套用至來源資料夾中提供的範例 Web 應用程式的新功能與增強功能。</span><span class="sxs-lookup"><span data-stu-id="99307-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="99307-110">所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="99307-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="99307-111">這個實驗室中的特定專案將會位於[ASP.NET MVC 4 模型和資料存取](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess)。</span><span class="sxs-lookup"><span data-stu-id="99307-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="99307-112">在**ASP.NET MVC 基礎**實際操作實驗室中，您有已硬式編碼的資料將從傳遞控制站至檢視範本。</span><span class="sxs-lookup"><span data-stu-id="99307-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="99307-113">但是，才能建立實際的 Web 應用程式，您可能想要使用實際的資料庫。</span><span class="sxs-lookup"><span data-stu-id="99307-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="99307-114">這個實際操作實驗室會示範如何使用 database engine 以儲存及擷取 Music Store 應用程式所需的資料。</span><span class="sxs-lookup"><span data-stu-id="99307-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="99307-115">若要完成，您將會啟動現有的資料庫，並從它建立實體資料模型。</span><span class="sxs-lookup"><span data-stu-id="99307-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="99307-116">在本實驗室中，您將會符合**Database First**方法以及**Code First**方法。</span><span class="sxs-lookup"><span data-stu-id="99307-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="99307-117">不過，您也可以使用**Model First**方法，建立相同的模型使用的工具，然後從它產生資料庫。</span><span class="sxs-lookup"><span data-stu-id="99307-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="99307-118">![第一個與資料庫。建立第一個模型](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs。第一次建立模型")</span><span class="sxs-lookup"><span data-stu-id="99307-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="99307-119">*第一個與資料庫。第一次建立模型*</span><span class="sxs-lookup"><span data-stu-id="99307-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="99307-120">之後產生模型，以提供從資料庫中，而不是使用硬式編碼資料的資料存放區檢視 StoreController 中進行適當的調整。</span><span class="sxs-lookup"><span data-stu-id="99307-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="99307-121">您必須不作任何修改檢視範本 StoreController 會返回相同 ViewModels 檢視範本，因為雖然這次的資料將來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="99307-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="99307-122">**程式碼的第一種方法**</span><span class="sxs-lookup"><span data-stu-id="99307-122">**The Code First Approach**</span></span>

<span data-ttu-id="99307-123">程式碼第一種方法可讓我們從程式碼模型定義而不會產生通常搭配架構的類別。</span><span class="sxs-lookup"><span data-stu-id="99307-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="99307-124">在程式碼第一次，模型物件會定義與 POCOs，&quot;純舊 CLR 物件&quot;。</span><span class="sxs-lookup"><span data-stu-id="99307-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="99307-125">POCOs 是簡單的一般類別，不具有任何繼承，而且不會實作介面。</span><span class="sxs-lookup"><span data-stu-id="99307-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="99307-126">我們可以自動產生的資料庫，或者我們可以使用現有的資料庫，並從程式碼產生的類別對應。</span><span class="sxs-lookup"><span data-stu-id="99307-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="99307-127">使用這種方法的優點是，模型會維持獨立於持續性架構 （在此情況下，Entity Framework），如 POCOs 類別不會搭配對應架構。</span><span class="sxs-lookup"><span data-stu-id="99307-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="99307-128">這個實驗室以 ASP.NET MVC 4 為基礎，自訂 Music Store 範例應用程式的版本，且最小化成只顯示在這個實際操作實驗室中的功能。</span><span class="sxs-lookup"><span data-stu-id="99307-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="99307-129">如果您想要瀏覽整個**Music Store**教學課程應用程式，您可以找到在[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)。</span><span class="sxs-lookup"><span data-stu-id="99307-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="99307-130">必要條件</span><span class="sxs-lookup"><span data-stu-id="99307-130">Prerequisites</span></span>

<span data-ttu-id="99307-131">您必須具備下列項目才能完成本實驗室：</span><span class="sxs-lookup"><span data-stu-id="99307-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="99307-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 A](#AppendixA)如需有關如何安裝指示)。</span><span class="sxs-lookup"><span data-stu-id="99307-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="99307-133">安裝程式</span><span class="sxs-lookup"><span data-stu-id="99307-133">Setup</span></span>

<span data-ttu-id="99307-134">**安裝程式碼片段**</span><span class="sxs-lookup"><span data-stu-id="99307-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="99307-135">為了方便起見，大部分您將沿著這個實驗室管理的程式碼可做為 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="99307-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="99307-136">若要安裝執行的程式碼片段**.\Source\Setup\CodeSnippets.vsi**檔案。</span><span class="sxs-lookup"><span data-stu-id="99307-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="99307-137">如果您不熟悉 Visual Studio 程式碼片段，而且想来了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 c： 使用程式碼片段](#AppendixC)&quot;。</span><span class="sxs-lookup"><span data-stu-id="99307-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="99307-138">練習</span><span class="sxs-lookup"><span data-stu-id="99307-138">Exercises</span></span>

<span data-ttu-id="99307-139">這個實際操作實驗室是由下列練習所組成：</span><span class="sxs-lookup"><span data-stu-id="99307-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="99307-140">練習 1： 加入資料庫</span><span class="sxs-lookup"><span data-stu-id="99307-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="99307-141">練習 2： 建立資料庫，使用 Code First</span><span class="sxs-lookup"><span data-stu-id="99307-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="99307-142">練習 3： 查詢參數的資料庫</span><span class="sxs-lookup"><span data-stu-id="99307-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="99307-143">每個練習會伴隨著**結束**包含所產生的解決方案完成練習之後，您應該取得資料夾。</span><span class="sxs-lookup"><span data-stu-id="99307-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="99307-144">如果您需要其他協助使用的所有練習，您可以使用這個方案做為指南。</span><span class="sxs-lookup"><span data-stu-id="99307-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="99307-145">完成本實驗室估計時間： **35 分鐘內**。</span><span class="sxs-lookup"><span data-stu-id="99307-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="99307-146">練習 1： 加入資料庫</span><span class="sxs-lookup"><span data-stu-id="99307-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="99307-147">在此練習中，您將了解如何將含有 MusicStore 應用程式方案，才能使用其資料的資料表的資料庫。</span><span class="sxs-lookup"><span data-stu-id="99307-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="99307-148">一旦資料庫產生模型，並加入至方案，您將修改 StoreController 類別取自資料庫，而不要使用硬式編碼值的資料提供檢視範本。</span><span class="sxs-lookup"><span data-stu-id="99307-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="99307-149">工作 1-加入資料庫</span><span class="sxs-lookup"><span data-stu-id="99307-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="99307-150">在這項工作，您會加入方案的 MusicStore 應用程式的主資料表與已建立的資料庫。</span><span class="sxs-lookup"><span data-stu-id="99307-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="99307-151">開啟**開始**方案位於**來源/Ex1AddingADatabaseDBFirst/開始/**資料夾。</span><span class="sxs-lookup"><span data-stu-id="99307-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="99307-152">您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="99307-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="99307-153">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="99307-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="99307-154">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="99307-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="99307-155">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="99307-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="99307-156">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="99307-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="99307-157">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="99307-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="99307-158">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="99307-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="99307-159">新增**MvcMusicStore**資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="99307-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="99307-160">在這個實際操作實驗室中，您將使用已建立的資料庫稱為**MvcMusicStore.mdf**。</span><span class="sxs-lookup"><span data-stu-id="99307-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="99307-161">若要這樣做，請以滑鼠右鍵按一下**應用程式\_資料**資料夾，指向**新增**，然後按一下 **現有項目**。</span><span class="sxs-lookup"><span data-stu-id="99307-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="99307-162">瀏覽至**\Source\Assets**選取**MvcMusicStore.mdf**檔案。</span><span class="sxs-lookup"><span data-stu-id="99307-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="99307-163">![加入現有項目](aspnet-mvc-4-models-and-data-access/_static/image2.png "加入現有項目")</span><span class="sxs-lookup"><span data-stu-id="99307-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="99307-164">*加入現有項目*</span><span class="sxs-lookup"><span data-stu-id="99307-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="99307-165">![MvcMusicStore.mdf 資料庫檔案](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf 資料庫檔案")</span><span class="sxs-lookup"><span data-stu-id="99307-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="99307-166">*MvcMusicStore.mdf 資料庫檔案*</span><span class="sxs-lookup"><span data-stu-id="99307-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="99307-167">資料庫已加入專案。</span><span class="sxs-lookup"><span data-stu-id="99307-167">The database has been added to the project.</span></span> <span data-ttu-id="99307-168">即使資料庫位於 「 解決方案中，您可以查詢並更新它，因為它已裝載於不同的資料庫伺服器。</span><span class="sxs-lookup"><span data-stu-id="99307-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="99307-169">![在 [方案總管 MvcMusicStore 資料庫](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore 方案總管] 中的資料庫")</span><span class="sxs-lookup"><span data-stu-id="99307-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="99307-170">*在 方案總管 MvcMusicStore 資料庫*</span><span class="sxs-lookup"><span data-stu-id="99307-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="99307-171">請確認資料庫的連接。</span><span class="sxs-lookup"><span data-stu-id="99307-171">Verify the connection to the database.</span></span> <span data-ttu-id="99307-172">若要這樣做，請按兩下**MvcMusicStore.mdf**建立連線。</span><span class="sxs-lookup"><span data-stu-id="99307-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="99307-173">![連接到 MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "連接到 MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="99307-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="99307-174">*連接到 MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="99307-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="99307-175">工作 2-建立資料模型</span><span class="sxs-lookup"><span data-stu-id="99307-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="99307-176">在這項工作，您將建立資料模型互動的先前工作中加入的資料庫。</span><span class="sxs-lookup"><span data-stu-id="99307-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="99307-177">建立表示資料庫的資料模型。</span><span class="sxs-lookup"><span data-stu-id="99307-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="99307-178">若要這樣做，請在 方案總管 中以滑鼠右鍵按一下**模型**資料夾，指向**新增**，然後按一下 **新項目**。</span><span class="sxs-lookup"><span data-stu-id="99307-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="99307-179">在**加入新項目**對話方塊中，選取**資料**範本，然後**ADO.NET 實體資料模型**項目。</span><span class="sxs-lookup"><span data-stu-id="99307-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="99307-180">資料模型將名稱變更為**StoreDB.edmx**按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="99307-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="99307-181">![加入 StoreDB ADO.NET 實體資料模型](aspnet-mvc-4-models-and-data-access/_static/image6.png "加入 StoreDB ADO.NET 實體資料模型")</span><span class="sxs-lookup"><span data-stu-id="99307-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="99307-182">*加入 StoreDB ADO.NET 實體資料模型*</span><span class="sxs-lookup"><span data-stu-id="99307-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="99307-183">**實體資料模型精靈**會出現。</span><span class="sxs-lookup"><span data-stu-id="99307-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="99307-184">此精靈將引導您建立的模型層。</span><span class="sxs-lookup"><span data-stu-id="99307-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="99307-185">由於模型應該依據現有的資料庫 recentyl 加入建立，選取**從資料庫產生**按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="99307-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="99307-186">![選擇模型內容](aspnet-mvc-4-models-and-data-access/_static/image7.png "選擇模型內容")</span><span class="sxs-lookup"><span data-stu-id="99307-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="99307-187">*選擇模型內容*</span><span class="sxs-lookup"><span data-stu-id="99307-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="99307-188">因為您正在從資料庫產生模型，您必須指定要使用的連接。</span><span class="sxs-lookup"><span data-stu-id="99307-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="99307-189">按一下**新連線**。</span><span class="sxs-lookup"><span data-stu-id="99307-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="99307-190">選取**Microsoft SQL Server 資料庫檔案**按一下**繼續**。</span><span class="sxs-lookup"><span data-stu-id="99307-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="99307-191">![選擇資料來源](aspnet-mvc-4-models-and-data-access/_static/image8.png "選擇資料來源")</span><span class="sxs-lookup"><span data-stu-id="99307-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="99307-192">*選擇資料來源 對話方塊*</span><span class="sxs-lookup"><span data-stu-id="99307-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="99307-193">按一下**瀏覽**選取的資料庫**MvcMusicStore.mdf**您位於**應用程式\_資料**資料夾，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="99307-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="99307-194">![連接屬性](aspnet-mvc-4-models-and-data-access/_static/image9.png "連接屬性")</span><span class="sxs-lookup"><span data-stu-id="99307-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="99307-195">*連接屬性*</span><span class="sxs-lookup"><span data-stu-id="99307-195">*Connection properties*</span></span>
6. <span data-ttu-id="99307-196">產生的類別應該具有相同名稱做為實體連接字串，因此它將名稱變更為**MusicStoreEntities**按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="99307-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="99307-197">![選擇資料連接](aspnet-mvc-4-models-and-data-access/_static/image10.png "選擇資料連接")</span><span class="sxs-lookup"><span data-stu-id="99307-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="99307-198">*選擇資料連接*</span><span class="sxs-lookup"><span data-stu-id="99307-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="99307-199">選擇要使用的資料庫物件。</span><span class="sxs-lookup"><span data-stu-id="99307-199">Choose the database objects to use.</span></span> <span data-ttu-id="99307-200">當實體模型會使用只在資料庫資料表時，選取**資料表**選項，並確定**在模型中包含外部索引鍵資料行**和**將化或單數化產生物件名稱**也會選取選項。</span><span class="sxs-lookup"><span data-stu-id="99307-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="99307-201">若要將模型命名空間變更**MvcMusicStore.Model**按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="99307-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="99307-202">![選擇的資料庫物件](aspnet-mvc-4-models-and-data-access/_static/image11.png "選擇的資料庫物件")</span><span class="sxs-lookup"><span data-stu-id="99307-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="99307-203">*選擇的資料庫物件*</span><span class="sxs-lookup"><span data-stu-id="99307-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="99307-204">如果顯示安全性警告對話方塊，按一下**確定**執行範本，並產生該模型實體的類別。</span><span class="sxs-lookup"><span data-stu-id="99307-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="99307-205">對應至資料庫的每個資料表的不同類別將會建立資料庫的實體圖表將會出現。</span><span class="sxs-lookup"><span data-stu-id="99307-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="99307-206">例如，**專輯**資料表將會由**專輯**類別，其中每個資料行的資料表中會對應至類別內容。</span><span class="sxs-lookup"><span data-stu-id="99307-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="99307-207">這可讓您查詢及處理物件，代表在資料庫中的資料列。</span><span class="sxs-lookup"><span data-stu-id="99307-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="99307-208">![實體圖](aspnet-mvc-4-models-and-data-access/_static/image12.png "實體圖表")</span><span class="sxs-lookup"><span data-stu-id="99307-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="99307-209">*實體圖表*</span><span class="sxs-lookup"><span data-stu-id="99307-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="99307-210">T4 範本 (.tt) 執行程式碼來產生實體類別，並具有相同名稱將會覆寫現有的類別。</span><span class="sxs-lookup"><span data-stu-id="99307-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="99307-211">在此範例中，類別&quot;專輯&quot;，&quot;類型&quot;和&quot;演出者&quot;加以覆寫與產生的程式碼。</span><span class="sxs-lookup"><span data-stu-id="99307-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="99307-212">工作 3-建置應用程式</span><span class="sxs-lookup"><span data-stu-id="99307-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="99307-213">在這項工作，您將檢查，雖然移除了模型產生**專輯**，**類型**和**演出者**模型類別的專案建置成功使用新的資料模型類別。</span><span class="sxs-lookup"><span data-stu-id="99307-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="99307-214">建置專案時選取**建置**功能表項目，然後**建置 MvcMusicStore**。</span><span class="sxs-lookup"><span data-stu-id="99307-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="99307-215">![建置專案](aspnet-mvc-4-models-and-data-access/_static/image13.png "建置專案")</span><span class="sxs-lookup"><span data-stu-id="99307-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="99307-216">*建置專案*</span><span class="sxs-lookup"><span data-stu-id="99307-216">*Building the project*</span></span>
2. <span data-ttu-id="99307-217">專案建置成功。</span><span class="sxs-lookup"><span data-stu-id="99307-217">The project builds successfully.</span></span> <span data-ttu-id="99307-218">為什麼它仍然運作？</span><span class="sxs-lookup"><span data-stu-id="99307-218">Why does it still work?</span></span> <span data-ttu-id="99307-219">這是因為資料庫資料表已將您所使用的屬性包含已移除的類別中的欄位**專輯**和**類型**。</span><span class="sxs-lookup"><span data-stu-id="99307-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="99307-220">![組建成功](aspnet-mvc-4-models-and-data-access/_static/image14.png "成功的組建")</span><span class="sxs-lookup"><span data-stu-id="99307-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="99307-221">*成功的組建*</span><span class="sxs-lookup"><span data-stu-id="99307-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="99307-222">設計工具會以圖表格式顯示實體，但它們實際上是 C# 類別。</span><span class="sxs-lookup"><span data-stu-id="99307-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="99307-223">展開**StoreDB.edmx** [方案總管] 中的節點，然後**StoreDB.tt**，您會看到新產生的實體。</span><span class="sxs-lookup"><span data-stu-id="99307-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="99307-224">![產生的檔案](aspnet-mvc-4-models-and-data-access/_static/image15.png "產生的檔案")</span><span class="sxs-lookup"><span data-stu-id="99307-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="99307-225">*產生的檔案*</span><span class="sxs-lookup"><span data-stu-id="99307-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="99307-226">工作 4-查詢資料庫</span><span class="sxs-lookup"><span data-stu-id="99307-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="99307-227">在這項工作，您將更新 StoreController 類別，以便不使用硬式編碼資料，它會查詢資料庫，以擷取資訊。</span><span class="sxs-lookup"><span data-stu-id="99307-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="99307-228">開啟**Controllers\StoreController.cs**並將下列欄位加入至要保存的執行個體的類別**MusicStoreEntities**類別，名為**storeDB**:</span><span class="sxs-lookup"><span data-stu-id="99307-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="99307-229">(程式碼片段-*模型和資料存取層 Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="99307-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
~~~
2. <span data-ttu-id="99307-230">**MusicStoreEntities**類別會公開資料庫中每個資料表集合屬性。</span><span class="sxs-lookup"><span data-stu-id="99307-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="99307-231">更新**瀏覽**要擷取的內容類型的所有動作方法**專輯**。</span><span class="sxs-lookup"><span data-stu-id="99307-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="99307-232">(程式碼片段-*模型和資料存取-Ex1 存放區瀏覽*)</span><span class="sxs-lookup"><span data-stu-id="99307-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
~~~
3. <span data-ttu-id="99307-233">更新**索引**動作方法，以擷取所有內容類型。</span><span class="sxs-lookup"><span data-stu-id="99307-233">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="99307-234">(程式碼片段-*模型和資料存取-Ex1 存放區索引*)</span><span class="sxs-lookup"><span data-stu-id="99307-234">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
~~~
4. <span data-ttu-id="99307-235">更新**索引**動作方法，來擷取所有內容類型，並轉換為清單集合。</span><span class="sxs-lookup"><span data-stu-id="99307-235">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="99307-236">(程式碼片段-*模型和資料存取-Ex1 存放區 GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="99307-236">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]
~~~

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="99307-237">工作 5-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="99307-237">Task 5 - Running the Application</span></span>

<span data-ttu-id="99307-238">在這項工作，您會檢查存放區索引頁面現在會顯示儲存在資料庫中而不是硬式編碼的內容類型。</span><span class="sxs-lookup"><span data-stu-id="99307-238">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="99307-239">若要變更檢視範本，因為不需要**StoreController**所傳回的相同實體如往常一般，但這次的資料將來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="99307-239">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="99307-240">重建方案，請按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="99307-240">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="99307-241">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="99307-241">The project starts in the Home page.</span></span> <span data-ttu-id="99307-242">確認的功能表**內容類型**不再是硬式編碼清單，並直接從資料庫擷取資料。</span><span class="sxs-lookup"><span data-stu-id="99307-242">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="99307-244">*從資料庫中瀏覽內容類型*</span><span class="sxs-lookup"><span data-stu-id="99307-244">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="99307-245">現在瀏覽至任何類型，並確認將從資料庫填入專輯。</span><span class="sxs-lookup"><span data-stu-id="99307-245">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="99307-246">![從資料庫中瀏覽專輯](aspnet-mvc-4-models-and-data-access/_static/image17.png "瀏覽的相簿，從資料庫")</span><span class="sxs-lookup"><span data-stu-id="99307-246">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="99307-247">*從資料庫中瀏覽相簿*</span><span class="sxs-lookup"><span data-stu-id="99307-247">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="99307-248">練習 2： 建立資料庫，先使用程式碼</span><span class="sxs-lookup"><span data-stu-id="99307-248">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="99307-249">在此練習中，您將學習如何使用 Code First 的方式來建立資料庫與資料表 MusicStore 應用程式，以及如何存取其資料。</span><span class="sxs-lookup"><span data-stu-id="99307-249">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="99307-250">一旦模型產生後，您將修改 StoreController 取自資料庫，而不要使用硬式編碼值的資料提供檢視範本。</span><span class="sxs-lookup"><span data-stu-id="99307-250">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="99307-251">如果您已完成練習 1，且已經使用資料庫第一種方法，您現在將會了解如何取得不同的程序相同的結果。</span><span class="sxs-lookup"><span data-stu-id="99307-251">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="99307-252">若要簡化您的讀取已標示和練習 1 相同的工作。</span><span class="sxs-lookup"><span data-stu-id="99307-252">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="99307-253">如果您尚未完成練習 1，但想要了解程式碼第一種方法，您可以從這個練習中啟動，並取得主題的完整涵蓋範圍。</span><span class="sxs-lookup"><span data-stu-id="99307-253">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="99307-254">工作 1-填入範例資料</span><span class="sxs-lookup"><span data-stu-id="99307-254">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="99307-255">在這項工作，您將資料庫中填入範例資料 intially 建立使用程式碼優先 （contract-first） 時。</span><span class="sxs-lookup"><span data-stu-id="99307-255">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="99307-256">開啟**開始**方案位於**來源/Ex2CreatingADatabaseCodeFirst/開始/**資料夾。</span><span class="sxs-lookup"><span data-stu-id="99307-256">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="99307-257">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="99307-257">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="99307-258">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="99307-258">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="99307-259">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="99307-259">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="99307-260">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="99307-260">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="99307-261">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="99307-261">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="99307-262">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="99307-262">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="99307-263">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="99307-263">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="99307-264">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="99307-264">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="99307-265">新增**SampleData.cs**檔案**模型**資料夾。</span><span class="sxs-lookup"><span data-stu-id="99307-265">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="99307-266">若要這樣做，請以滑鼠右鍵按一下**模型**資料夾，指向**新增**，然後按一下 **現有項目**。</span><span class="sxs-lookup"><span data-stu-id="99307-266">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="99307-267">瀏覽至**\Source\Assets**選取**SampleData.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="99307-267">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="99307-268">![範例資料填入的程式碼](aspnet-mvc-4-models-and-data-access/_static/image18.png "範例資料填入的程式碼")</span><span class="sxs-lookup"><span data-stu-id="99307-268">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="99307-269">*範例資料填入的程式碼*</span><span class="sxs-lookup"><span data-stu-id="99307-269">*Sample data populate code*</span></span>
3. <span data-ttu-id="99307-270">開啟**Global.asax.cs**檔案，然後加入下列*使用*陳述式。</span><span class="sxs-lookup"><span data-stu-id="99307-270">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="99307-271">(程式碼片段-*模型和資料存取-Ex2 全域 Asax Using*)</span><span class="sxs-lookup"><span data-stu-id="99307-271">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
~~~
4. <span data-ttu-id="99307-272">在**應用程式\_start**方法中加入下列這一行設定資料庫初始設定式。</span><span class="sxs-lookup"><span data-stu-id="99307-272">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="99307-273">(程式碼片段-*模型和資料存取-Ex2 全域 Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="99307-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]
~~~

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="99307-274">工作 2-設定資料庫的連接</span><span class="sxs-lookup"><span data-stu-id="99307-274">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="99307-275">既然您已新增資料庫專案，您會撰寫**Web.config**檔案的連接字串。</span><span class="sxs-lookup"><span data-stu-id="99307-275">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="99307-276">加入連接字串在**Web.config**。若要這樣做，請開啟**Web.config**在專案根目錄和取代連接字串中的這一行以命名 DefaultConnection **&lt;connectionStrings&gt;** > 一節：</span><span class="sxs-lookup"><span data-stu-id="99307-276">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="99307-277">![Web.config 檔案位置](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config 檔案位置")</span><span class="sxs-lookup"><span data-stu-id="99307-277">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="99307-278">*web.config 檔案位置*</span><span class="sxs-lookup"><span data-stu-id="99307-278">*Web.config file location*</span></span>


~~~
[!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="99307-279">工作 3-使用模型</span><span class="sxs-lookup"><span data-stu-id="99307-279">Task 3 - Working with the Model</span></span>

<span data-ttu-id="99307-280">既然您已經設定資料庫的連接，您將連結的模型與資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="99307-280">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="99307-281">在這項工作，您將建立的類別，將會連結到第一個程式碼的資料庫。</span><span class="sxs-lookup"><span data-stu-id="99307-281">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="99307-282">請記住是應該修改存在 POCO 模型類別。</span><span class="sxs-lookup"><span data-stu-id="99307-282">Remember that there is an existent POCO model class that should be modified.</span></span>

   > [!NOTE]
> <span data-ttu-id="99307-283">如果您已完成練習 1，您會注意由精靈已執行此步驟。</span><span class="sxs-lookup"><span data-stu-id="99307-283">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="99307-284">透過這種程式碼第一次，您將手動建立將會連結到資料實體的類別。</span><span class="sxs-lookup"><span data-stu-id="99307-284">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>


1. <span data-ttu-id="99307-285">開啟 POCO 模型類別**類型**從**模型**專案資料夾，並包含 id。</span><span class="sxs-lookup"><span data-stu-id="99307-285">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="99307-286">使用 int 屬性具有名稱**GenreId**。</span><span class="sxs-lookup"><span data-stu-id="99307-286">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="99307-287">(程式碼片段-*模型和資料存取-Ex2 程式碼的第一個內容類型*)</span><span class="sxs-lookup"><span data-stu-id="99307-287">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

> [!NOTE]
> To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.
> 
> You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
~~~
2. <span data-ttu-id="99307-288">現在，開啟 POCO 模型類別**專輯**從**模型**專案資料夾和包含的外部索引鍵，建立屬性的名稱**GenreId**和**ArtistId**。</span><span class="sxs-lookup"><span data-stu-id="99307-288">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="99307-289">這個類別已經有**GenreId**主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="99307-289">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="99307-290">(程式碼片段-*模型和資料存取-Ex2 程式碼的第一個專輯*)</span><span class="sxs-lookup"><span data-stu-id="99307-290">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
~~~
3. <span data-ttu-id="99307-291">開啟 POCO 模型類別**演出者**並包含**ArtistId**屬性。</span><span class="sxs-lookup"><span data-stu-id="99307-291">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="99307-292">(程式碼片段-*模型和資料存取-Ex2 程式碼的第一個演出者*)</span><span class="sxs-lookup"><span data-stu-id="99307-292">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
~~~
4. <span data-ttu-id="99307-293">以滑鼠右鍵按一下**模型**專案資料夾，然後選取**新增 |類別**。</span><span class="sxs-lookup"><span data-stu-id="99307-293">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="99307-294">將檔案命名**MusicStoreEntities.cs**。</span><span class="sxs-lookup"><span data-stu-id="99307-294">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="99307-295">然後，按一下 **新增。**</span><span class="sxs-lookup"><span data-stu-id="99307-295">Then, click **Add.**</span></span>

    <span data-ttu-id="99307-296">![將類別加入](aspnet-mvc-4-models-and-data-access/_static/image20.png "加入類別")</span><span class="sxs-lookup"><span data-stu-id="99307-296">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="99307-297">*加入新項目*</span><span class="sxs-lookup"><span data-stu-id="99307-297">*Adding a new item*</span></span>

    <span data-ttu-id="99307-298">![加入 class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "加入 class2")</span><span class="sxs-lookup"><span data-stu-id="99307-298">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="99307-299">*加入類別*</span><span class="sxs-lookup"><span data-stu-id="99307-299">*Adding a class*</span></span>
5. <span data-ttu-id="99307-300">開啟您剛才建立的類別**MusicStoreEntities.cs**，包含命名空間和**System.Data.Entity**和**System.Data.Entity.Infrastructure**。</span><span class="sxs-lookup"><span data-stu-id="99307-300">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
~~~
6. <span data-ttu-id="99307-301">取代類別宣告，以擴充**DbContext**類別： 宣告公用**DBSet**並覆寫**OnModelCreating**方法。</span><span class="sxs-lookup"><span data-stu-id="99307-301">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="99307-302">在此步驟之後，您會收到將會連結您的模型與 Entity Framework 的領域類別。</span><span class="sxs-lookup"><span data-stu-id="99307-302">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="99307-303">若要這樣做，請以下列內容取代類別程式碼：</span><span class="sxs-lookup"><span data-stu-id="99307-303">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="99307-304">(程式碼片段-*模型和資料存取-Ex2 程式碼的第一個 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="99307-304">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre. By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table. You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)
~~~

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="99307-305">工作 4-查詢資料庫</span><span class="sxs-lookup"><span data-stu-id="99307-305">Task 4 - Querying the Database</span></span>

<span data-ttu-id="99307-306">在這項工作，您將更新 StoreController 類別，以便不使用硬式編碼資料，它將它從資料庫擷取。</span><span class="sxs-lookup"><span data-stu-id="99307-306">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="99307-307">這項工作是與啟動器相同之練習 1。</span><span class="sxs-lookup"><span data-stu-id="99307-307">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="99307-308">如果您已完成練習 1 表示您會發現這些步驟是這兩種方法中的相同 (第一次資料庫或 Code first)。</span><span class="sxs-lookup"><span data-stu-id="99307-308">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="99307-309">不在使用模型時，會連結的資料相同，但資料實體的存取尚未從控制器透明。</span><span class="sxs-lookup"><span data-stu-id="99307-309">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="99307-310">開啟**Controllers\StoreController.cs**並將下列欄位加入至要保存的執行個體的類別**MusicStoreEntities**類別，名為**storeDB**:</span><span class="sxs-lookup"><span data-stu-id="99307-310">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="99307-311">(程式碼片段-*模型和資料存取層 Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="99307-311">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
~~~
2. <span data-ttu-id="99307-312">**MusicStoreEntities**類別會公開資料庫中每個資料表集合屬性。</span><span class="sxs-lookup"><span data-stu-id="99307-312">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="99307-313">更新**瀏覽**要擷取的內容類型的所有動作方法**專輯**。</span><span class="sxs-lookup"><span data-stu-id="99307-313">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="99307-314">(程式碼片段-*模型和資料存取-Ex2 存放區瀏覽*)</span><span class="sxs-lookup"><span data-stu-id="99307-314">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
~~~
3. <span data-ttu-id="99307-315">更新**索引**動作方法，以擷取所有內容類型。</span><span class="sxs-lookup"><span data-stu-id="99307-315">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="99307-316">(程式碼片段-*模型和資料存取-Ex2 存放區索引*)</span><span class="sxs-lookup"><span data-stu-id="99307-316">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
~~~
4. <span data-ttu-id="99307-317">更新**索引**動作方法，來擷取所有內容類型，並轉換為清單集合。</span><span class="sxs-lookup"><span data-stu-id="99307-317">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="99307-318">(程式碼片段-*模型和資料存取-Ex2 存放區 GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="99307-318">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]
~~~

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="99307-319">工作 5-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="99307-319">Task 5 - Running the Application</span></span>

<span data-ttu-id="99307-320">在這項工作，您會檢查存放區索引頁面現在會顯示儲存在資料庫中而不是硬式編碼的內容類型。</span><span class="sxs-lookup"><span data-stu-id="99307-320">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="99307-321">若要變更檢視範本，因為不需要**StoreController**所傳回的相同**StoreIndexViewModel**如往常一般，但這次的資料將來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="99307-321">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="99307-322">重建方案，請按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="99307-322">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="99307-323">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="99307-323">The project starts in the Home page.</span></span> <span data-ttu-id="99307-324">確認的功能表**內容類型**不再是硬式編碼清單，並直接從資料庫擷取資料。</span><span class="sxs-lookup"><span data-stu-id="99307-324">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="99307-326">*從資料庫中瀏覽內容類型*</span><span class="sxs-lookup"><span data-stu-id="99307-326">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="99307-327">現在瀏覽至任何類型，並確認將從資料庫填入專輯。</span><span class="sxs-lookup"><span data-stu-id="99307-327">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="99307-328">![從資料庫中瀏覽專輯](aspnet-mvc-4-models-and-data-access/_static/image23.png "瀏覽的相簿，從資料庫")</span><span class="sxs-lookup"><span data-stu-id="99307-328">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="99307-329">*從資料庫中瀏覽相簿*</span><span class="sxs-lookup"><span data-stu-id="99307-329">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="99307-330">練習 3： 查詢參數的資料庫</span><span class="sxs-lookup"><span data-stu-id="99307-330">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="99307-331">在此練習中，您將學習如何查詢資料庫中使用參數，以及如何使用查詢結果成形的功能會減少數字的資料庫會以更有效率的方式存取擷取資料。</span><span class="sxs-lookup"><span data-stu-id="99307-331">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="99307-332">如需查詢結果來形成的進一步資訊，請造訪下列[msdn 文章](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx)。</span><span class="sxs-lookup"><span data-stu-id="99307-332">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="99307-333">工作 1-從資料庫擷取專輯修改 StoreController</span><span class="sxs-lookup"><span data-stu-id="99307-333">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="99307-334">在這個工作中，您將變更**StoreController**類別來存取資料庫，以擷取特定的內容類型中的專輯。</span><span class="sxs-lookup"><span data-stu-id="99307-334">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="99307-335">開啟**開始**方案位於**Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin**資料夾，如果您想要使用程式碼優先方法或**Source\Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin**資料夾，如果您想要使用資料庫第一種方法。</span><span class="sxs-lookup"><span data-stu-id="99307-335">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="99307-336">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="99307-336">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="99307-337">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="99307-337">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="99307-338">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="99307-338">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="99307-339">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="99307-339">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="99307-340">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="99307-340">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="99307-341">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="99307-341">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="99307-342">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="99307-342">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="99307-343">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="99307-343">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="99307-344">開啟**StoreController**類別，以變更**瀏覽**動作方法。</span><span class="sxs-lookup"><span data-stu-id="99307-344">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="99307-345">若要這樣做，請在**方案總管 中**，依序展開**控制器**資料夾，然後按兩下**StoreController.cs**。</span><span class="sxs-lookup"><span data-stu-id="99307-345">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="99307-346">變更**瀏覽**動作方法，以擷取特定的內容類型的相簿。</span><span class="sxs-lookup"><span data-stu-id="99307-346">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="99307-347">若要這樣做，請取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="99307-347">To do this, replace the following code:</span></span>

    <span data-ttu-id="99307-348">(程式碼片段-*模型和資料存取-Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="99307-348">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too. You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album. The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.
> 
> You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved. This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information. In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.
> 
> The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well. This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="99307-349">工作 2-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="99307-349">Task 2 - Running the Application</span></span>

<span data-ttu-id="99307-350">在這項工作，您將執行應用程式，並從資料庫擷取的特定內容類型的相簿。</span><span class="sxs-lookup"><span data-stu-id="99307-350">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="99307-351">按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="99307-351">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="99307-352">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="99307-352">The project starts in the Home page.</span></span> <span data-ttu-id="99307-353">將 URL 變更為**/存放區/瀏覽？ 內容類型 = Pop**驗證，正在從資料庫擷取的結果。</span><span class="sxs-lookup"><span data-stu-id="99307-353">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="99307-354">![內容類型來瀏覽](aspnet-mvc-4-models-and-data-access/_static/image24.png "內容類型來瀏覽")</span><span class="sxs-lookup"><span data-stu-id="99307-354">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="99307-355">*瀏覽/存放區/瀏覽？ 內容類型 = Pop*</span><span class="sxs-lookup"><span data-stu-id="99307-355">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="99307-356">工作 3-Id 所存取的相簿</span><span class="sxs-lookup"><span data-stu-id="99307-356">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="99307-357">在這項工作，您將會重複先前的程序，以取得其識別碼的相簿</span><span class="sxs-lookup"><span data-stu-id="99307-357">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="99307-358">關閉瀏覽器，如有需要若要返回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="99307-358">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="99307-359">開啟**StoreController**類別，以變更**詳細資料**動作方法。</span><span class="sxs-lookup"><span data-stu-id="99307-359">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="99307-360">若要這樣做，請在**方案總管 中**，依序展開**控制器**資料夾，然後按兩下**StoreController.cs**。</span><span class="sxs-lookup"><span data-stu-id="99307-360">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="99307-361">變更**詳細資料**擷取專輯詳細資料的動作方法會根據其**識別碼**。若要這樣做，請取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="99307-361">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="99307-362">(程式碼片段-*模型和資料存取-Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="99307-362">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="99307-363">工作 4-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="99307-363">Task 4 - Running the Application</span></span>

<span data-ttu-id="99307-364">在這項工作，您會在網頁瀏覽器中執行應用程式，並取得專輯詳細資料，其識別碼。</span><span class="sxs-lookup"><span data-stu-id="99307-364">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="99307-365">按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="99307-365">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="99307-366">在首頁上，啟動專案。</span><span class="sxs-lookup"><span data-stu-id="99307-366">The project starts in the Home page.</span></span> <span data-ttu-id="99307-367">將 URL 變更為**/Store/Details/51**或瀏覽內容類型，並選取要確認結果要從資料庫擷取的相簿。</span><span class="sxs-lookup"><span data-stu-id="99307-367">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="99307-368">![瀏覽詳細資料](aspnet-mvc-4-models-and-data-access/_static/image25.png "瀏覽詳細資料")</span><span class="sxs-lookup"><span data-stu-id="99307-368">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="99307-369">*瀏覽 /Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="99307-369">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="99307-370">此外，您可以部署此應用程式以 Windows Azure Web Sites 下列[附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixB)。</span><span class="sxs-lookup"><span data-stu-id="99307-370">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="99307-371">總結</span><span class="sxs-lookup"><span data-stu-id="99307-371">Summary</span></span>

<span data-ttu-id="99307-372">藉由完成這個實際操作實驗室，您學到的 ASP.NET MVC 模型和資料存取的基本概念，並透過**Database First**方法以及**Code First**方法：</span><span class="sxs-lookup"><span data-stu-id="99307-372">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="99307-373">如何將資料庫加入方案，才能取用它的資料</span><span class="sxs-lookup"><span data-stu-id="99307-373">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="99307-374">如何更新控制器以取自資料庫而不是硬式編碼的資料提供檢視範本</span><span class="sxs-lookup"><span data-stu-id="99307-374">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="99307-375">如何查詢使用參數的資料庫</span><span class="sxs-lookup"><span data-stu-id="99307-375">How to query the database using parameters</span></span>
- <span data-ttu-id="99307-376">如何使用查詢結果成形，減少資料庫存取的功能更有效率的方式擷取資料</span><span class="sxs-lookup"><span data-stu-id="99307-376">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="99307-377">如何使用 Microsoft Entity Framework 中的資料庫第一和第一個程式碼方法來連結的模型資料庫</span><span class="sxs-lookup"><span data-stu-id="99307-377">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="99307-378">附錄 a： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="99307-378">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="99307-379">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="99307-379">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="99307-380">下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="99307-380">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="99307-381">移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="99307-381">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="99307-382">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 與 Windows Azure SDK</em>&quot;。</span><span class="sxs-lookup"><span data-stu-id="99307-382">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="99307-383">按一下**立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="99307-383">Click on **Install Now**.</span></span> <span data-ttu-id="99307-384">如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="99307-384">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="99307-385">一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="99307-385">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="99307-386">![安裝 Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="99307-386">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="99307-387">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="99307-387">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="99307-388">閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。</span><span class="sxs-lookup"><span data-stu-id="99307-388">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="99307-390">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="99307-390">*Accepting the license terms*</span></span>
5. <span data-ttu-id="99307-391">等待直到完成下載和安裝程序。</span><span class="sxs-lookup"><span data-stu-id="99307-391">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="99307-393">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="99307-393">*Installation progress*</span></span>
6. <span data-ttu-id="99307-394">當安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="99307-394">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="99307-396">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="99307-396">*Installation completed*</span></span>
7. <span data-ttu-id="99307-397">按一下**結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="99307-397">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="99307-398">若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。</span><span class="sxs-lookup"><span data-stu-id="99307-398">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 方塊](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="99307-400">*VS Express for Web 方塊*</span><span class="sxs-lookup"><span data-stu-id="99307-400">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="99307-401">附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="99307-401">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="99307-402">此附錄將說明如何從 Windows Azure 管理入口網站建立新的網站和發行您取得依照實驗室中，利用 Web Deploy 發行功能的 Windows Azure 所提供的應用程式。</span><span class="sxs-lookup"><span data-stu-id="99307-402">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="99307-403">工作 1-建立新的網站，從 Windows Azure 入口網站</span><span class="sxs-lookup"><span data-stu-id="99307-403">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="99307-404">移至[Windows Azure 管理入口網站](https://manage.windowsazure.com/)並使用與您訂用帳戶相關聯的 Microsoft 認證登入。</span><span class="sxs-lookup"><span data-stu-id="99307-404">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="99307-405">與 Windows Azure 中，您可以免費裝載 10 個 ASP.NET 網站並再擴充隨流量成長。</span><span class="sxs-lookup"><span data-stu-id="99307-405">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="99307-406">您可以註冊[這裡](http://aka.ms/aspnet-hol-azure)。</span><span class="sxs-lookup"><span data-stu-id="99307-406">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="99307-407">![登入 Windows Azure 入口網站](aspnet-mvc-4-models-and-data-access/_static/image31.png "登入 Windows Azure 入口網站")</span><span class="sxs-lookup"><span data-stu-id="99307-407">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="99307-408">*登入 Windows Azure 管理入口網站*</span><span class="sxs-lookup"><span data-stu-id="99307-408">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="99307-409">按一下**新增**命令列上。</span><span class="sxs-lookup"><span data-stu-id="99307-409">Click **New** on the command bar.</span></span>

    <span data-ttu-id="99307-410">![建立新的網站](aspnet-mvc-4-models-and-data-access/_static/image32.png "建立新的網站")</span><span class="sxs-lookup"><span data-stu-id="99307-410">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="99307-411">*建立新的網站*</span><span class="sxs-lookup"><span data-stu-id="99307-411">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="99307-412">按一下**計算** | **網站**。</span><span class="sxs-lookup"><span data-stu-id="99307-412">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="99307-413">然後選取**快速建立**選項。</span><span class="sxs-lookup"><span data-stu-id="99307-413">Then select **Quick Create** option.</span></span> <span data-ttu-id="99307-414">新的網站上提供可用的 URL，然後按一下**建立網站**。</span><span class="sxs-lookup"><span data-stu-id="99307-414">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="99307-415">Windows Azure 網站是您可以控制和管理在雲端中執行的 web 應用程式的主機。</span><span class="sxs-lookup"><span data-stu-id="99307-415">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="99307-416">快速建立 選項可讓您部署已完成的 web 應用程式至 Windows Azure 網站從入口網站外部。</span><span class="sxs-lookup"><span data-stu-id="99307-416">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="99307-417">它不包含設定資料庫的步驟。</span><span class="sxs-lookup"><span data-stu-id="99307-417">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="99307-418">![建立新的網站使用 快速建立](aspnet-mvc-4-models-and-data-access/_static/image33.png "建立新的網站使用 快速建立")</span><span class="sxs-lookup"><span data-stu-id="99307-418">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="99307-419">*建立新的網站使用 快速建立*</span><span class="sxs-lookup"><span data-stu-id="99307-419">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="99307-420">等候新**網站**建立。</span><span class="sxs-lookup"><span data-stu-id="99307-420">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="99307-421">建立網站之後按一下底下的連結**URL**資料行。</span><span class="sxs-lookup"><span data-stu-id="99307-421">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="99307-422">請檢查新的網站運作。</span><span class="sxs-lookup"><span data-stu-id="99307-422">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="99307-423">![瀏覽至新的網站](aspnet-mvc-4-models-and-data-access/_static/image34.png "瀏覽至新的網站")</span><span class="sxs-lookup"><span data-stu-id="99307-423">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="99307-424">*瀏覽至新的網站*</span><span class="sxs-lookup"><span data-stu-id="99307-424">*Browsing to the new web site*</span></span>

    <span data-ttu-id="99307-425">![執行網站](aspnet-mvc-4-models-and-data-access/_static/image35.png "執行的網站")</span><span class="sxs-lookup"><span data-stu-id="99307-425">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="99307-426">*執行的網站*</span><span class="sxs-lookup"><span data-stu-id="99307-426">*Web site running*</span></span>
6. <span data-ttu-id="99307-427">返回入口網站並按一下底下的網站名稱**名稱**資料行會顯示 [管理] 頁面。</span><span class="sxs-lookup"><span data-stu-id="99307-427">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="99307-428">![開啟網站管理頁面](aspnet-mvc-4-models-and-data-access/_static/image36.png "開啟網站管理頁面")</span><span class="sxs-lookup"><span data-stu-id="99307-428">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="99307-429">*開啟網站管理頁面*</span><span class="sxs-lookup"><span data-stu-id="99307-429">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="99307-430">在**儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。</span><span class="sxs-lookup"><span data-stu-id="99307-430">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="99307-431">*發行設定檔*包含所有 web 應用程式發行至 Windows Azure 網站的每個已啟用的發行方法所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="99307-431">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="99307-432">發行設定檔包含 Url、 使用者認證和資料庫連接，並對每個端點已啟用的發行集的方法進行驗證所需的字串。</span><span class="sxs-lookup"><span data-stu-id="99307-432">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="99307-433">**Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支援讀取發行設定檔自動將這些程式的組態web 應用程式發行至 Windows Azure 網站。</span><span class="sxs-lookup"><span data-stu-id="99307-433">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="99307-434">![下載網站發行設定檔](aspnet-mvc-4-models-and-data-access/_static/image37.png "下載網站發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="99307-434">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="99307-435">*下載網站發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="99307-435">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="99307-436">下載發行設定檔至已知位置。</span><span class="sxs-lookup"><span data-stu-id="99307-436">Download the publish profile file to a known location.</span></span> <span data-ttu-id="99307-437">進一步在這個練習中，您會看到如何使用這個檔案從 Visual Studio 將 web 應用程式至 Windows Azure 網站發行。</span><span class="sxs-lookup"><span data-stu-id="99307-437">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="99307-438">![儲存發行設定檔](aspnet-mvc-4-models-and-data-access/_static/image38.png "儲存發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="99307-438">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="99307-439">*儲存發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="99307-439">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="99307-440">工作 2-設定資料庫伺服器</span><span class="sxs-lookup"><span data-stu-id="99307-440">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="99307-441">如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。</span><span class="sxs-lookup"><span data-stu-id="99307-441">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="99307-442">如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。</span><span class="sxs-lookup"><span data-stu-id="99307-442">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="99307-443">您將需要 SQL Database 伺服器來儲存應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="99307-443">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="99307-444">您可以從您的訂用帳戶，在 Windows Azure 管理入口網站中檢視 SQL Database 伺服器**Sql 資料庫** | **伺服器** | **伺服器儀表板**。</span><span class="sxs-lookup"><span data-stu-id="99307-444">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="99307-445">如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="99307-445">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="99307-446">請注意**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，因為您將在下一個工作中使用它們。</span><span class="sxs-lookup"><span data-stu-id="99307-446">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="99307-447">並未建立資料庫，因為它會在後續階段中建立。</span><span class="sxs-lookup"><span data-stu-id="99307-447">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="99307-448">![SQL Database 伺服器儀表板](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database 伺服器儀表板")</span><span class="sxs-lookup"><span data-stu-id="99307-448">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="99307-449">*SQL Database 伺服器儀表板*</span><span class="sxs-lookup"><span data-stu-id="99307-449">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="99307-450">下一項工作在您將測試資料庫連接，從 Visual Studio，因此您需要在伺服器的清單中包含您的本機 IP 位址**允許的 IP 位址**。</span><span class="sxs-lookup"><span data-stu-id="99307-450">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="99307-451">若要這樣做，請按一下**設定**，選取 [IP 位址從**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**和**結束 IP 位址**文字方塊，然後按一下![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="99307-451">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![加入用戶端 IP 位址](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="99307-453">*加入用戶端 IP 位址*</span><span class="sxs-lookup"><span data-stu-id="99307-453">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="99307-454">一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。</span><span class="sxs-lookup"><span data-stu-id="99307-454">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![確認變更](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="99307-456">*確認變更*</span><span class="sxs-lookup"><span data-stu-id="99307-456">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="99307-457">工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="99307-457">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="99307-458">返回至 ASP.NET MVC 4 方案。</span><span class="sxs-lookup"><span data-stu-id="99307-458">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="99307-459">在**方案總管 中**，以滑鼠右鍵按一下網站專案，然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="99307-459">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="99307-460">![發行應用程式](aspnet-mvc-4-models-and-data-access/_static/image43.png "發行應用程式")</span><span class="sxs-lookup"><span data-stu-id="99307-460">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="99307-461">*發行網站*</span><span class="sxs-lookup"><span data-stu-id="99307-461">*Publishing the web site*</span></span>
2. <span data-ttu-id="99307-462">匯入發行設定檔儲存在第一項工作中。</span><span class="sxs-lookup"><span data-stu-id="99307-462">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="99307-463">![匯入發行設定檔](aspnet-mvc-4-models-and-data-access/_static/image44.png "匯入發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="99307-463">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="99307-464">*匯入發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="99307-464">*Importing publish profile*</span></span>
3. <span data-ttu-id="99307-465">按一下**驗證連線**。</span><span class="sxs-lookup"><span data-stu-id="99307-465">Click **Validate Connection**.</span></span> <span data-ttu-id="99307-466">驗證完成後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="99307-466">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="99307-467">一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。</span><span class="sxs-lookup"><span data-stu-id="99307-467">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="99307-468">![驗證連線](aspnet-mvc-4-models-and-data-access/_static/image45.png "驗證連線")</span><span class="sxs-lookup"><span data-stu-id="99307-468">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="99307-469">*驗證連接*</span><span class="sxs-lookup"><span data-stu-id="99307-469">*Validating connection*</span></span>
4. <span data-ttu-id="99307-470">在**設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="99307-470">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="99307-471">![Web 部署設定](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web 部署設定")</span><span class="sxs-lookup"><span data-stu-id="99307-471">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="99307-472">*Web 部署設定*</span><span class="sxs-lookup"><span data-stu-id="99307-472">*Web deploy configuration*</span></span>
5. <span data-ttu-id="99307-473">設定資料庫連接，如下所示：</span><span class="sxs-lookup"><span data-stu-id="99307-473">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="99307-474">在**伺服器名稱**您 SQL Database 伺服器 URL 使用下列方法類型*tcp:*前置詞。</span><span class="sxs-lookup"><span data-stu-id="99307-474">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="99307-475">在**使用者名**輸入您的伺服器系統管理員身分登入名稱。</span><span class="sxs-lookup"><span data-stu-id="99307-475">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="99307-476">在**密碼**輸入伺服器系統管理員身分登入密碼。</span><span class="sxs-lookup"><span data-stu-id="99307-476">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="99307-477">輸入新的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="99307-477">Type a new database name.</span></span>

     <span data-ttu-id="99307-478">![設定目的地連接字串](aspnet-mvc-4-models-and-data-access/_static/image47.png "設定目的地連接字串")</span><span class="sxs-lookup"><span data-stu-id="99307-478">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="99307-479">*設定目的地連接字串*</span><span class="sxs-lookup"><span data-stu-id="99307-479">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="99307-480">然後按一下 [確定]。 </span><span class="sxs-lookup"><span data-stu-id="99307-480">Then click **OK**.</span></span> <span data-ttu-id="99307-481">當系統提示您建立資料庫依序按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="99307-481">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="99307-482">![建立資料庫](aspnet-mvc-4-models-and-data-access/_static/image48.png "建立的資料庫字串")</span><span class="sxs-lookup"><span data-stu-id="99307-482">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="99307-483">*建立資料庫*</span><span class="sxs-lookup"><span data-stu-id="99307-483">*Creating the database*</span></span>
7. <span data-ttu-id="99307-484">您將用來連接到在 Windows Azure SQL Database 的連接字串會顯示在預設連接文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="99307-484">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="99307-485">然後按 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="99307-485">Then click **Next**.</span></span>

    <span data-ttu-id="99307-486">![連接字串指向 SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "連接字串指向 SQL 資料庫")</span><span class="sxs-lookup"><span data-stu-id="99307-486">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="99307-487">*連接字串指向 SQL 資料庫*</span><span class="sxs-lookup"><span data-stu-id="99307-487">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="99307-488">在**預覽**頁面上，按一下**發行**。</span><span class="sxs-lookup"><span data-stu-id="99307-488">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="99307-489">![Web 應用程式發行](aspnet-mvc-4-models-and-data-access/_static/image50.png "發行 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="99307-489">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="99307-490">*發行 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="99307-490">*Publishing the web application*</span></span>
9. <span data-ttu-id="99307-491">一旦在發佈程序完成時，預設瀏覽器會開啟已發行的網站。</span><span class="sxs-lookup"><span data-stu-id="99307-491">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="99307-492">附錄 c： 使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="99307-492">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="99307-493">程式碼片段，您會有您需要在您可以的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="99307-493">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="99307-494">實驗室文件會告訴您完全時您可以使用它們，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="99307-494">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="99307-495">![若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段](aspnet-mvc-4-models-and-data-access/_static/image51.png "使用 Visual Studio 程式碼片段，將程式碼插入您的專案")</span><span class="sxs-lookup"><span data-stu-id="99307-495">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="99307-496">*若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="99307-496">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="99307-497">***若要加入的程式碼片段，使用鍵盤 （僅限 C#)***</span><span class="sxs-lookup"><span data-stu-id="99307-497">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="99307-498">將游標放在您想要插入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="99307-498">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="99307-499">開始鍵入片段名稱 （不含空格或連字號）。</span><span class="sxs-lookup"><span data-stu-id="99307-499">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="99307-500">觀察 IntelliSense 會顯示比對程式碼片段的名稱。</span><span class="sxs-lookup"><span data-stu-id="99307-500">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="99307-501">選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。</span><span class="sxs-lookup"><span data-stu-id="99307-501">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="99307-502">按下 Tab 鍵兩次的游標位置插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="99307-502">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="99307-503">![開始輸入程式碼片段名稱](aspnet-mvc-4-models-and-data-access/_static/image52.png "開始鍵入片段名稱")</span><span class="sxs-lookup"><span data-stu-id="99307-503">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="99307-504">*開始輸入程式碼片段名稱*</span><span class="sxs-lookup"><span data-stu-id="99307-504">*Start typing the snippet name*</span></span>

<span data-ttu-id="99307-505">![按 Tab 鍵選取反白顯示的程式碼片段](aspnet-mvc-4-models-and-data-access/_static/image53.png "按 Tab 鍵以選取反白顯示的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="99307-505">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="99307-506">*按 Tab 鍵選取反白顯示的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="99307-506">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="99307-507">![按 Tab 鍵一次程式碼片段就會擴展](aspnet-mvc-4-models-and-data-access/_static/image54.png "按 Tab 鍵一次程式碼片段就會擴展")</span><span class="sxs-lookup"><span data-stu-id="99307-507">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="99307-508">*按 Tab 鍵一次程式碼片段就會擴展*</span><span class="sxs-lookup"><span data-stu-id="99307-508">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="99307-509">***若要加入的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。</span><span class="sxs-lookup"><span data-stu-id="99307-509">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="99307-510">以滑鼠右鍵按一下您要插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="99307-510">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="99307-511">選取**插入程式碼片段**後面**我的程式碼片段**。</span><span class="sxs-lookup"><span data-stu-id="99307-511">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="99307-512">透過在按一下挑選清單中，從相關的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="99307-512">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="99307-513">![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-models-and-data-access/_static/image55.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="99307-513">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="99307-514">*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="99307-514">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="99307-515">![透過在按一下挑選清單中，從相關的程式碼片段](aspnet-mvc-4-models-and-data-access/_static/image56.png "上它即可挑選清單中，從相關的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="99307-515">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="99307-516">*透過在按一下挑選清單中，從相關的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="99307-516">*Pick the relevant snippet from the list, by clicking on it*</span></span>
