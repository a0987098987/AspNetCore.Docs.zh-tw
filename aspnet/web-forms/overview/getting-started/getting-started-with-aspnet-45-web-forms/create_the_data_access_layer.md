---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: "建立資料存取層 |Microsoft 文件"
author: Erikre
description: "此教學課程將告訴您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Form 應用程式的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: ebace10dc8a861ab38bd5c834c2225e3373f13fe
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="create-the-data-access-layer"></a><span data-ttu-id="9816a-103">建立資料存取層</span><span class="sxs-lookup"><span data-stu-id="9816a-103">Create the Data Access Layer</span></span>
====================
<span data-ttu-id="9816a-104">由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="9816a-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="9816a-105">[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="9816a-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="9816a-106">此教學課程將告訴您建置一個使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Form 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="9816a-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="9816a-107">Visual Studio 2013[與 C# 原始程式碼的專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附此教學課程了。</span><span class="sxs-lookup"><span data-stu-id="9816a-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="9816a-108">本教學課程說明如何建立、 存取，以及檢閱使用 ASP.NET Web Form 和 Entity Framework Code First，從資料庫的資料。</span><span class="sxs-lookup"><span data-stu-id="9816a-108">This tutorial describes how to create, access, and review data from a database using ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="9816a-109">本教學課程上一個教學課程 < 建立專案 > 為基礎，而且是 Wingtip 玩具存放區的教學課程系列的一部分。</span><span class="sxs-lookup"><span data-stu-id="9816a-109">This tutorial builds on the previous tutorial "Create the Project" and is part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="9816a-110">當您已經完成本教學課程時，您會建置中的資料存取類別的群組*模型*專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="9816a-110">When you've completed this tutorial, you will have built a group of data-access classes that are in the *Models* folder of the project.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="9816a-111">您將學習：</span><span class="sxs-lookup"><span data-stu-id="9816a-111">What you'll learn:</span></span>

- <span data-ttu-id="9816a-112">如何建立資料模型。</span><span class="sxs-lookup"><span data-stu-id="9816a-112">How to create the data models.</span></span>
- <span data-ttu-id="9816a-113">如何初始化和植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="9816a-113">How to initialize and seed the database.</span></span>
- <span data-ttu-id="9816a-114">如何更新和應用程式設定為支援的資料庫。</span><span class="sxs-lookup"><span data-stu-id="9816a-114">How to update and configure the application to support the database.</span></span>

### <a name="these-are-the-features-introduced-in-the-tutorial"></a><span data-ttu-id="9816a-115">這些是教學課程中所引進的功能：</span><span class="sxs-lookup"><span data-stu-id="9816a-115">These are the features introduced in the tutorial:</span></span>

- <span data-ttu-id="9816a-116">Entity Framework Code First</span><span class="sxs-lookup"><span data-stu-id="9816a-116">Entity Framework Code First</span></span>
- <span data-ttu-id="9816a-117">LocalDB</span><span class="sxs-lookup"><span data-stu-id="9816a-117">LocalDB</span></span>
- <span data-ttu-id="9816a-118">資料註釋</span><span class="sxs-lookup"><span data-stu-id="9816a-118">Data Annotations</span></span>

## <a name="creating-the-data-models"></a><span data-ttu-id="9816a-119">建立資料模型</span><span class="sxs-lookup"><span data-stu-id="9816a-119">Creating the Data Models</span></span>

<span data-ttu-id="9816a-120">[Entity Framework](https://msdn.microsoft.com/en-us/data/aa937723)是物件關聯式對應 (ORM) 架構。</span><span class="sxs-lookup"><span data-stu-id="9816a-120">[Entity Framework](https://msdn.microsoft.com/en-us/data/aa937723) is an object-relational mapping (ORM) framework.</span></span> <span data-ttu-id="9816a-121">它可讓您使用關聯式資料，以排除大部分資料存取程式碼，您通常需要撰寫的物件。</span><span class="sxs-lookup"><span data-stu-id="9816a-121">It lets you work with relational data as objects, eliminating most of the data-access code that you'd usually need to write.</span></span> <span data-ttu-id="9816a-122">使用 Entity Framework，您可以發出查詢使用[LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx)，然後擷取和操作資料當做強型別物件。</span><span class="sxs-lookup"><span data-stu-id="9816a-122">Using Entity Framework, you can issue queries using [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx), then retrieve and manipulate data as strongly typed objects.</span></span> <span data-ttu-id="9816a-123">LINQ 提供查詢及更新的資料的模式。</span><span class="sxs-lookup"><span data-stu-id="9816a-123">LINQ provides patterns for querying and updating data.</span></span> <span data-ttu-id="9816a-124">使用 Entity Framework 可讓您專注於建立您的應用程式的其餘部分，而不是將焦點放在資料存取基礎。</span><span class="sxs-lookup"><span data-stu-id="9816a-124">Using Entity Framework allows you to focus on creating the rest of your application, rather than focusing on the data access fundamentals.</span></span> <span data-ttu-id="9816a-125">稍後在本教學課程的系列，我們將示範如何使用資料來填入瀏覽和產品的查詢。</span><span class="sxs-lookup"><span data-stu-id="9816a-125">Later in this tutorial series, we'll show you how to use the data to populate navigation and product queries.</span></span>

<span data-ttu-id="9816a-126">Entity Framework 支援呼叫開發架構*Code First*。</span><span class="sxs-lookup"><span data-stu-id="9816a-126">Entity Framework supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="9816a-127">程式碼第一次可讓您定義資料模型使用類別。</span><span class="sxs-lookup"><span data-stu-id="9816a-127">Code First lets you define your data models using classes.</span></span> <span data-ttu-id="9816a-128">類別是一種建構，可讓您建立您自己的自訂類型，分組在一起的其他型別、 方法和事件變數。</span><span class="sxs-lookup"><span data-stu-id="9816a-128">A class is a construct that enables you to create your own custom types by grouping together variables of other types, methods and events.</span></span> <span data-ttu-id="9816a-129">您可以將類別對應至現有的資料庫，或使用它們來產生資料庫。</span><span class="sxs-lookup"><span data-stu-id="9816a-129">You can map classes to an existing database or use them to generate a database.</span></span> <span data-ttu-id="9816a-130">在本教學課程中，您將撰寫資料模型類別來建立資料模型。</span><span class="sxs-lookup"><span data-stu-id="9816a-130">In this tutorial, you'll create the data models by writing data model classes.</span></span> <span data-ttu-id="9816a-131">然後，您會讓 Entity Framework 從這些新的類別上建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="9816a-131">Then, you'll let Entity Framework create the database on the fly from these new classes.</span></span>

<span data-ttu-id="9816a-132">您會藉由建立實體類別，定義資料模型來進行 Web Forms 應用程式開始。</span><span class="sxs-lookup"><span data-stu-id="9816a-132">You will begin by creating the entity classes that define the data models for the Web Forms application.</span></span> <span data-ttu-id="9816a-133">然後，您將建立管理的實體類別，並提供資料存取資料庫的內容類別。</span><span class="sxs-lookup"><span data-stu-id="9816a-133">Then you will create a context class that manages the entity classes and provides data access to the database.</span></span> <span data-ttu-id="9816a-134">您也將建立來擴展資料庫，您將使用的初始設定式類別。</span><span class="sxs-lookup"><span data-stu-id="9816a-134">You will also create an initializer class that you will use to populate the database.</span></span>

### <a name="entity-framework-and-references"></a><span data-ttu-id="9816a-135">實體架構和參考</span><span class="sxs-lookup"><span data-stu-id="9816a-135">Entity Framework and References</span></span>

<span data-ttu-id="9816a-136">根據預設，Entity Framework 時，會包含您建立新**ASP.NET Web 應用程式**使用**Web Form**範本。</span><span class="sxs-lookup"><span data-stu-id="9816a-136">By default, Entity Framework is included when you create a new **ASP.NET Web Application** using the **Web Forms** template.</span></span> <span data-ttu-id="9816a-137">Entity Framework 可以安裝、 解除安裝，而且以 NuGet 套件更新。</span><span class="sxs-lookup"><span data-stu-id="9816a-137">Entity Framework can be installed, uninstalled, and updated as a NuGet package.</span></span>

<span data-ttu-id="9816a-138">此 NuGet 封裝包含下列**執行階段**您的專案中的組件：</span><span class="sxs-lookup"><span data-stu-id="9816a-138">This NuGet package includes the following **runtime** assemblies within your project:</span></span>

- <span data-ttu-id="9816a-139">EntityFramework.dll – 所有通用執行階段程式碼使用 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="9816a-139">EntityFramework.dll – All the common runtime code used by Entity Framework</span></span>
- <span data-ttu-id="9816a-140">EntityFramework.SqlServer.dll – Entity Framework 的 Microsoft SQL Server 提供者</span><span class="sxs-lookup"><span data-stu-id="9816a-140">EntityFramework.SqlServer.dll – The Microsoft SQL Server provider for Entity Framework</span></span>

### <a name="entity-classes"></a><span data-ttu-id="9816a-141">實體類別</span><span class="sxs-lookup"><span data-stu-id="9816a-141">Entity Classes</span></span>

<span data-ttu-id="9816a-142">您所建立用以定義資料的結構描述的類別稱為 「 實體類別 」。</span><span class="sxs-lookup"><span data-stu-id="9816a-142">The classes you create to define the schema of the data are called entity classes.</span></span> <span data-ttu-id="9816a-143">如果您還不熟悉資料庫設計，將實體類別視為資料表定義的資料庫。</span><span class="sxs-lookup"><span data-stu-id="9816a-143">If you're new to database design, think of the entity classes as table definitions of a database.</span></span> <span data-ttu-id="9816a-144">類別中的每個屬性會指定資料庫的資料表中資料行。</span><span class="sxs-lookup"><span data-stu-id="9816a-144">Each property in the class specifies a column in the table of the database.</span></span> <span data-ttu-id="9816a-145">這些類別提供輕量型、 關聯式物件之間的介面物件導向的程式碼和資料庫的關聯式資料表結構。</span><span class="sxs-lookup"><span data-stu-id="9816a-145">These classes provide a lightweight, object-relational interface between object-oriented code and the relational table structure of the database.</span></span>

<span data-ttu-id="9816a-146">在本教學課程中，您將開始加入簡單的實體類別代表產品和分類的結構描述。</span><span class="sxs-lookup"><span data-stu-id="9816a-146">In this tutorial, you'll start out by adding simple entity classes representing the schemas for products and categories.</span></span> <span data-ttu-id="9816a-147">產品類別將包含每個產品定義。</span><span class="sxs-lookup"><span data-stu-id="9816a-147">The products class will contain definitions for each product.</span></span> <span data-ttu-id="9816a-148">每個產品類別的成員名稱會是`ProductID`， `ProductName`， `Description`， `ImagePath`， `UnitPrice`， `CategoryID`，和`Category`。</span><span class="sxs-lookup"><span data-stu-id="9816a-148">The name of each of the members of the product class will be `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, and `Category`.</span></span> <span data-ttu-id="9816a-149">目錄類別將包含產品可以屬於，例如汽車、 船或平面的每個分類的定義。</span><span class="sxs-lookup"><span data-stu-id="9816a-149">The category class will contain definitions for each category that a product can belong to, such as Car, Boat, or Plane.</span></span> <span data-ttu-id="9816a-150">每個類別目錄類別的成員名稱會是`CategoryID`， `CategoryName`， `Description`，和`Products`。</span><span class="sxs-lookup"><span data-stu-id="9816a-150">The name of each of the members of the category class will be `CategoryID`, `CategoryName`, `Description`, and `Products`.</span></span> <span data-ttu-id="9816a-151">每個產品將屬於其中一個類別。</span><span class="sxs-lookup"><span data-stu-id="9816a-151">Each product will belong to one of the categories.</span></span> <span data-ttu-id="9816a-152">這些實體類別就會加入至專案的現有*模型*資料夾。</span><span class="sxs-lookup"><span data-stu-id="9816a-152">These entity classes will be added to the project's existing *Models* folder.</span></span>

1. <span data-ttu-id="9816a-153">在**方案總管 中**，以滑鼠右鍵按一下*模型*資料夾，然後選取**新增** - &gt; **新項目**。</span><span class="sxs-lookup"><span data-stu-id="9816a-153">In **Solution Explorer**, right-click the *Models* folder and then select **Add** -&gt; **New Item**.</span></span> 

    ![建立資料存取層中的新項目功能表](create_the_data_access_layer/_static/image1.png)

 <span data-ttu-id="9816a-155">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="9816a-155">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="9816a-156">在下**Visual C#**從**已安裝**左邊的窗格，選取**程式碼**。</span><span class="sxs-lookup"><span data-stu-id="9816a-156">Under **Visual C#** from the **Installed** pane on the left, select **Code**.</span></span> 

    ![建立資料存取層中的新項目功能表](create_the_data_access_layer/_static/image2.png)
3. <span data-ttu-id="9816a-158">選取**類別**從中間窗格，並命名這個新類別*Product.cs*。</span><span class="sxs-lookup"><span data-stu-id="9816a-158">Select **Class** from the middle pane and name this new class *Product.cs*.</span></span>
4. <span data-ttu-id="9816a-159">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="9816a-159">Click **Add**.</span></span>  
 <span data-ttu-id="9816a-160">在編輯器中，會顯示新的類別檔案。</span><span class="sxs-lookup"><span data-stu-id="9816a-160">The new class file is displayed in the editor.</span></span>
5. <span data-ttu-id="9816a-161">下列程式碼取代預設程式碼：</span><span class="sxs-lookup"><span data-stu-id="9816a-161">Replace the default code with the following code:</span></span>   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. <span data-ttu-id="9816a-162">重複步驟 1 到 4，不過，命名為新的類別，以建立另一個類別*Category.cs* ，並以下列程式碼取代預設程式碼：</span><span class="sxs-lookup"><span data-stu-id="9816a-162">Create another class by repeating steps 1 through 4, however, name the new class *Category.cs* and replace the default code with the following code:</span></span>  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

<span data-ttu-id="9816a-163">如先前所述，`Category`類別類型的應用程式的產品設計銷售代表 (例如<a id="a"> </a>&quot;汽車&quot;，&quot;船&quot;， &quot;火箭&quot;等等)，而`Product`類別代表在資料庫中的個別產品 (toys)。</span><span class="sxs-lookup"><span data-stu-id="9816a-163">As previously mentioned, the `Category` class represents the type of product that the application is designed to sell (such as <a id="a"></a>&quot;Cars&quot;, &quot;Boats&quot;, &quot;Rockets&quot;, and so on), and the `Product` class represents the individual products (toys) in the database.</span></span> <span data-ttu-id="9816a-164">每個執行個體`Product`物件將會對應到關聯式資料庫資料表中的資料列和每個產品類別的屬性會對應到關聯式資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="9816a-164">Each instance of a `Product` object will correspond to a row within a relational database table, and each property of the Product class will map to a column in the relational database table.</span></span> <span data-ttu-id="9816a-165">稍後在本教學課程中，您將檢閱包含在資料庫中的產品資料。</span><span class="sxs-lookup"><span data-stu-id="9816a-165">Later in this tutorial, you'll review the product data contained in the database.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="9816a-166">資料註釋</span><span class="sxs-lookup"><span data-stu-id="9816a-166">Data Annotations</span></span>

<span data-ttu-id="9816a-167">您可能已注意到某些類別的成員具有屬性指定成員相關的詳細資料，例如`[ScaffoldColumn(false)]`。</span><span class="sxs-lookup"><span data-stu-id="9816a-167">You may have noticed that certain members of the classes have attributes specifying details about the member, such as `[ScaffoldColumn(false)]`.</span></span> <span data-ttu-id="9816a-168">這些是*資料註解*。</span><span class="sxs-lookup"><span data-stu-id="9816a-168">These are *data annotations*.</span></span> <span data-ttu-id="9816a-169">資料附註屬性可描述如何驗證使用者輸入，該成員，以指定格式，並指定如何它模型化資料庫建立時。</span><span class="sxs-lookup"><span data-stu-id="9816a-169">The data annotation attributes can describe how to validate user input for that member, to specify formatting for it, and to specify how it is modeled when the database is created.</span></span>

### <a name="context-class"></a><span data-ttu-id="9816a-170">Context 類別</span><span class="sxs-lookup"><span data-stu-id="9816a-170">Context Class</span></span>

<span data-ttu-id="9816a-171">若要開始使用資料存取的類別，您必須定義的內容類別。</span><span class="sxs-lookup"><span data-stu-id="9816a-171">To start using the classes for data access, you must define a context class.</span></span> <span data-ttu-id="9816a-172">如先前所述，內容類別管理的實體類別 (例如`Product`類別和`Category`類別)，並提供對資料庫的資料存取。</span><span class="sxs-lookup"><span data-stu-id="9816a-172">As mentioned previously, the context class manages the entity classes (such as the `Product` class and the `Category` class) and provides data access to the database.</span></span>

<span data-ttu-id="9816a-173">此程序會加入新 C# 內容類別來*模型*資料夾。</span><span class="sxs-lookup"><span data-stu-id="9816a-173">This procedure adds a new C# context class to the *Models* folder.</span></span>

1. <span data-ttu-id="9816a-174">以滑鼠右鍵按一下*模型*資料夾，然後選取**新增** - &gt; **新項目**。</span><span class="sxs-lookup"><span data-stu-id="9816a-174">Right-click the *Models* folder and then select **Add** -&gt; **New Item**.</span></span>   
 <span data-ttu-id="9816a-175">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="9816a-175">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="9816a-176">選取**類別**從中間窗格中，其命名*ProductContext.cs*按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="9816a-176">Select **Class** from the middle pane, name it *ProductContext.cs* and click **Add**.</span></span>
3. <span data-ttu-id="9816a-177">取代為下列程式碼類別中包含的預設程式碼：</span><span class="sxs-lookup"><span data-stu-id="9816a-177">Replace the default code contained in the class with the following code:</span></span>   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

<span data-ttu-id="9816a-178">這個程式碼加入`System.Data.Entity`命名空間，讓您可以存取所有核心功能的 Entity Framework 中，其中包含查詢的能力，插入、 更新和刪除資料使用強型別的物件。</span><span class="sxs-lookup"><span data-stu-id="9816a-178">This code adds the `System.Data.Entity` namespace so that you have access to all the core functionality of Entity Framework, which includes the capability to query, insert, update, and delete data by working with strongly typed objects.</span></span>

<span data-ttu-id="9816a-179">`ProductContext`類別代表 Entity Framework 產品的資料庫內容，用來處理擷取、 儲存及更新`Product`類別執行個體在資料庫中的。</span><span class="sxs-lookup"><span data-stu-id="9816a-179">The `ProductContext` class represents Entity Framework product database context, which handles fetching, storing, and updating `Product` class instances in the database.</span></span> <span data-ttu-id="9816a-180">`ProductContext`類別衍生自`DbContext`基底 Entity Framework 所提供的類別。</span><span class="sxs-lookup"><span data-stu-id="9816a-180">The `ProductContext` class derives from the `DbContext` base class provided by Entity Framework.</span></span>

### <a name="initializer-class"></a><span data-ttu-id="9816a-181">初始設定式類別</span><span class="sxs-lookup"><span data-stu-id="9816a-181">Initializer Class</span></span>

<span data-ttu-id="9816a-182">您必須執行一些自訂邏輯，來初始化資料庫第一個時間使用的內容。</span><span class="sxs-lookup"><span data-stu-id="9816a-182">You will need to run some custom logic to initialize the database the first time the context is used.</span></span> <span data-ttu-id="9816a-183">這可讓要加入至資料庫，以便您可以立即顯示產品和分類的種子資料。</span><span class="sxs-lookup"><span data-stu-id="9816a-183">This will allow seed data to be added to the database so that you can immediately display products and categories.</span></span>

<span data-ttu-id="9816a-184">此程序會加入新 C# 初始設定式的類別來*模型*資料夾。</span><span class="sxs-lookup"><span data-stu-id="9816a-184">This procedure adds a new C# initializer class to the *Models* folder.</span></span>

1. <span data-ttu-id="9816a-185">建立另一個`Class`中*模型*資料夾並將其命名*ProductDatabaseInitializer.cs*。</span><span class="sxs-lookup"><span data-stu-id="9816a-185">Create another `Class` in the *Models* folder and name it *ProductDatabaseInitializer.cs*.</span></span>
2. <span data-ttu-id="9816a-186">取代為下列程式碼類別中包含的預設程式碼：</span><span class="sxs-lookup"><span data-stu-id="9816a-186">Replace the default code contained in the class with the following code:</span></span>   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

<span data-ttu-id="9816a-187">建立和初始化，資料庫時，您可以看到上述程式碼，`Seed`屬性覆寫，而且設定。</span><span class="sxs-lookup"><span data-stu-id="9816a-187">As you can see from the above code, when the database is created and initialized, the `Seed` property is overridden and set.</span></span> <span data-ttu-id="9816a-188">當`Seed`屬性設定、 分類和產品中的值會用來填入資料庫。</span><span class="sxs-lookup"><span data-stu-id="9816a-188">When the `Seed` property is set, the values from the categories and products are used to populate the database.</span></span> <span data-ttu-id="9816a-189">如果您嘗試更新的種子資料，藉由修改上述的程式碼，在建立資料庫之後，將不會在執行 Web 應用程式時看到任何更新。</span><span class="sxs-lookup"><span data-stu-id="9816a-189">If you attempt to update the seed data by modifying the above code after the database has been created, you won't see any updates when you run the Web application.</span></span> <span data-ttu-id="9816a-190">原因是上述程式碼使用的實作`DropCreateDatabaseIfModelChanges`辨識重設種子資料之前是否已變更模型 （結構描述） 的類別。</span><span class="sxs-lookup"><span data-stu-id="9816a-190">The reason is the above code uses an implementation of the `DropCreateDatabaseIfModelChanges` class to recognize if the model (schema) has changed before resetting the seed data.</span></span> <span data-ttu-id="9816a-191">如果沒有變更`Category`和`Product`實體類別，資料庫將不會重新初始化的種子資料。</span><span class="sxs-lookup"><span data-stu-id="9816a-191">If no changes are made to the `Category` and `Product` entity classes, the database will not be reinitialized with the seed data.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9816a-192">如果您想要的資料庫重新建立每次執行該應用程式，您可以使用`DropCreateDatabaseAlways`類別而不是`DropCreateDatabaseIfModelChanges`類別。</span><span class="sxs-lookup"><span data-stu-id="9816a-192">If you wanted the database to be recreated every time you ran the application, you could use the `DropCreateDatabaseAlways` class instead of the `DropCreateDatabaseIfModelChanges` class.</span></span> <span data-ttu-id="9816a-193">不過對於此教學課程的數列，請使用`DropCreateDatabaseIfModelChanges`類別。</span><span class="sxs-lookup"><span data-stu-id="9816a-193">However for this tutorial series, use the `DropCreateDatabaseIfModelChanges` class.</span></span>


<span data-ttu-id="9816a-194">此時在本教學課程中，您將需要*模型*資料夾具有四個新的類別和一個預設類別：</span><span class="sxs-lookup"><span data-stu-id="9816a-194">At this point in this tutorial, you will have a *Models* folder with four new classes and one default class:</span></span>

![建立資料存取層-Models 資料夾](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a><span data-ttu-id="9816a-196">設定應用程式使用的資料模型</span><span class="sxs-lookup"><span data-stu-id="9816a-196">Configuring the Application to Use the Data Model</span></span>

<span data-ttu-id="9816a-197">既然您已建立的類別代表的資料，您必須設定應用程式使用的類別。</span><span class="sxs-lookup"><span data-stu-id="9816a-197">Now that you've created the classes that represent the data, you must configure the application to use the classes.</span></span> <span data-ttu-id="9816a-198">在*Global.asax*檔案中，您將初始化模型的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9816a-198">In the *Global.asax* file, you add code that initializes the model.</span></span> <span data-ttu-id="9816a-199">在*Web.config*檔案，在您加入的資訊，告訴您資料庫功能的應用程式用來儲存新的資料類別所代表的資料。</span><span class="sxs-lookup"><span data-stu-id="9816a-199">In the *Web.config* file you add information that tells the application what database you'll use to store the data that's represented by the new data classes.</span></span> <span data-ttu-id="9816a-200">*Global.asax*檔案可以用來處理應用程式事件或方法。</span><span class="sxs-lookup"><span data-stu-id="9816a-200">The *Global.asax* file can be used to handle application events or methods.</span></span> <span data-ttu-id="9816a-201">*Web.config*檔可讓您控制 ASP.NET web 應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="9816a-201">The *Web.config* file allows you to control the configuration of your ASP.NET web application.</span></span>

#### <a name="updating-the-globalasax-file"></a><span data-ttu-id="9816a-202">正在更新 Global.asax 檔案</span><span class="sxs-lookup"><span data-stu-id="9816a-202">Updating the Global.asax file</span></span>

<span data-ttu-id="9816a-203">若要初始化的資料模型，應用程式啟動時，您將更新`Application_Start`中的處理常式*Global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="9816a-203">To initialize the data models when the application starts, you will update the `Application_Start` handler in the *Global.asax.cs* file.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9816a-204">在 [方案總管] 中，您可以選取*Global.asax*檔案或*Global.asax.cs*檔案進行編輯*Global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="9816a-204">In Solution Explorer, you can select either the *Global.asax* file or the *Global.asax.cs* file to edit the *Global.asax.cs* file.</span></span>


1. <span data-ttu-id="9816a-205">加入下列程式碼中以黃色反白顯示`Application_Start`方法中的*Global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="9816a-205">Add the following code highlighted in yellow to the `Application_Start` method in the *Global.asax.cs* file.</span></span>   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> <span data-ttu-id="9816a-206">您的瀏覽器必須支援 HTML5 檢視瀏覽器中檢視此教學課程系列時，以黃色醒目提示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9816a-206">Your browser must support HTML5 to view the code highlighted in yellow when viewing this tutorial series in a browser.</span></span>


<span data-ttu-id="9816a-207">如上述程式碼所示，應用程式啟動時，應用程式會指定存取資料期間第一次將執行初始設定式。</span><span class="sxs-lookup"><span data-stu-id="9816a-207">As shown in the above code, when the application starts, the application specifies the initializer that will run during the first time the data is accessed.</span></span> <span data-ttu-id="9816a-208">兩個額外的命名空間所需存取`Database`物件和`ProductDatabaseInitializer`物件。</span><span class="sxs-lookup"><span data-stu-id="9816a-208">The two additional namespaces are required to access the `Database` object and the `ProductDatabaseInitializer` object.</span></span>

 <span data-ttu-id="9816a-209">修改 Web.Config 檔案</span><span class="sxs-lookup"><span data-stu-id="9816a-209">Modifying the Web.Config File</span></span> 

<span data-ttu-id="9816a-210">Entity Framework Code First 會產生資料庫針對您在預設位置時資料庫已填入種子資料，雖然您自己的連接資訊加入至應用程式會提供您的資料庫位置的控制項。</span><span class="sxs-lookup"><span data-stu-id="9816a-210">Although Entity Framework Code First will generate a database for you in a default location when the database is populated with seed data, adding your own connection information to your application gives you control of the database location.</span></span> <span data-ttu-id="9816a-211">您指定此應用程式中使用連接字串的資料庫連接*Web.config*專案根目錄的檔案。</span><span class="sxs-lookup"><span data-stu-id="9816a-211">You specify this database connection using a connection string in the application's *Web.config* file at the root of the project.</span></span> <span data-ttu-id="9816a-212">您可以藉由加入新的連接字串，指示資料庫的位置 (*wingtiptoys.mdf*) 建置應用程式的資料目錄 (*應用程式\_資料*)，而不是預設值位置。</span><span class="sxs-lookup"><span data-stu-id="9816a-212">By adding a new connection string, you can direct the location of the database (*wingtiptoys.mdf*) to be built in the application's data directory (*App\_Data*), rather than its default location.</span></span> <span data-ttu-id="9816a-213">進行這項變更可讓您找出並檢查資料庫檔案，稍後在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="9816a-213">Making this change will allow you to find and inspect the database file later in this tutorial.</span></span>

1. <span data-ttu-id="9816a-214">在**方案總管 中**、 尋找和開啟*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="9816a-214">In **Solution Explorer**, find and open the *Web.config* file.</span></span>
2. <span data-ttu-id="9816a-215">加入連接字串中以黃色反白顯示`<connectionStrings>`區段*Web.config*檔案，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9816a-215">Add the connection string highlighted in yellow to the `<connectionStrings>` section of the *Web.config* file as follows:</span></span>  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

<span data-ttu-id="9816a-216">第一次執行應用程式時，它會建立資料庫連接字串所指定的位置。</span><span class="sxs-lookup"><span data-stu-id="9816a-216">When the application is run for the first time, it will build the database at the location specified by the connection string.</span></span> <span data-ttu-id="9816a-217">但是之前執行的應用程式，讓我們來建立該第一次。</span><span class="sxs-lookup"><span data-stu-id="9816a-217">But before running the application, let's build it first.</span></span>

## <a name="building-the-application"></a><span data-ttu-id="9816a-218">建置應用程式</span><span class="sxs-lookup"><span data-stu-id="9816a-218">Building the Application</span></span>

<span data-ttu-id="9816a-219">若要確定所有類別和變更您的 Web 應用程式都運作，您應該建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="9816a-219">To make sure that all the classes and changes to your Web application work, you should build the application.</span></span>

1. <span data-ttu-id="9816a-220">從**偵錯**功能表上，選取**建置 WingtipToys**。</span><span class="sxs-lookup"><span data-stu-id="9816a-220">From the **Debug** menu, select **Build WingtipToys**.</span></span>  
 <span data-ttu-id="9816a-221">**輸出**視窗隨即顯示，且如果一切順利，您會看到*成功*訊息。</span><span class="sxs-lookup"><span data-stu-id="9816a-221">The **Output** window is displayed, and if all went well, you see a *succeeded* message.</span></span>  

    ![建立資料存取層-輸出視窗](create_the_data_access_layer/_static/image4.png)

<span data-ttu-id="9816a-223">如果您遇到錯誤時，請重新檢查前面的步驟。</span><span class="sxs-lookup"><span data-stu-id="9816a-223">If you run into an error, re-check the above steps.</span></span> <span data-ttu-id="9816a-224">中的資訊**輸出**視窗會指出哪些檔案有問題，而檔案中變更需要的地方。</span><span class="sxs-lookup"><span data-stu-id="9816a-224">The information in the **Output** window will indicate which file has a problem and where in the file a change is required.</span></span> <span data-ttu-id="9816a-225">這項資訊可讓您決定上述步驟的哪個部分要檢閱並修正您的專案中。</span><span class="sxs-lookup"><span data-stu-id="9816a-225">This information will enable you to determine what part of the above steps need to be reviewed and fixed in your project.</span></span>

## <a name="summary"></a><span data-ttu-id="9816a-226">總結</span><span class="sxs-lookup"><span data-stu-id="9816a-226">Summary</span></span>

<span data-ttu-id="9816a-227">在本教學課程系列的您有建立資料模型，以及，新增將用來初始化及植入資料庫的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9816a-227">In this tutorial of the series you have created the data model, as well as, added the code that will be used to initialize and seed the database.</span></span> <span data-ttu-id="9816a-228">您也已經設定應用程式執行應用程式時使用的資料模型。</span><span class="sxs-lookup"><span data-stu-id="9816a-228">You have also configured the application to use the data models when the application is run.</span></span>

<span data-ttu-id="9816a-229">在下一個教學課程中，您將會更新 UI、 加入導覽，和從資料庫擷取資料。</span><span class="sxs-lookup"><span data-stu-id="9816a-229">In the next tutorial, you'll update the UI, add navigation, and retrieve data from the database.</span></span> <span data-ttu-id="9816a-230">這會導致資料庫正在自動根據您在本教學課程中建立的實體類別。</span><span class="sxs-lookup"><span data-stu-id="9816a-230">This will result in the database being automatically created based on the entity classes that you created in this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9816a-231">其他資源</span><span class="sxs-lookup"><span data-stu-id="9816a-231">Additional Resources</span></span>

<span data-ttu-id="9816a-232">[Entity Framework 概觀](https://msdn.microsoft.com/en-us/library/bb399567.aspx) </span><span class="sxs-lookup"><span data-stu-id="9816a-232">[Entity Framework Overview](https://msdn.microsoft.com/en-us/library/bb399567.aspx) </span></span>  
<span data-ttu-id="9816a-233">[ADO.NET Entity Framework 初學者指南](https://msdn.microsoft.com/en-us/data/ee712907) </span><span class="sxs-lookup"><span data-stu-id="9816a-233">[Beginner's Guide to the ADO.NET Entity Framework](https://msdn.microsoft.com/en-us/data/ee712907) </span></span>  
<span data-ttu-id="9816a-234">[Code First 開發有 Entity Framework](http://www.msteched.com/2010/Europe/DEV212) （影片）</span><span class="sxs-lookup"><span data-stu-id="9816a-234">[Code First Development with Entity Framework](http://www.msteched.com/2010/Europe/DEV212) (video)</span></span>   
<span data-ttu-id="9816a-235">[程式碼第一個關聯性 Fluent 應用程式開發介面](https://msdn.microsoft.com/en-us/data/hh134698) </span><span class="sxs-lookup"><span data-stu-id="9816a-235">[Code First Relationships Fluent API](https://msdn.microsoft.com/en-us/data/hh134698) </span></span>  
[<span data-ttu-id="9816a-236">第一個資料註解的程式碼</span><span class="sxs-lookup"><span data-stu-id="9816a-236">Code First Data Annotations</span></span>](https://msdn.microsoft.com/en-us/data/gg193958)  
[<span data-ttu-id="9816a-237">Entity Framework 的產能改善功能</span><span class="sxs-lookup"><span data-stu-id="9816a-237">Productivity Improvements for the Entity Framework</span></span>](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

>[!div class="step-by-step"]
<span data-ttu-id="9816a-238">[上一頁](create-the-project.md)
[下一頁](ui_and_navigation.md)</span><span class="sxs-lookup"><span data-stu-id="9816a-238">[Previous](create-the-project.md)
[Next](ui_and_navigation.md)</span></span>
