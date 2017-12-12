---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: "建立使用商務規則驗證模型 |Microsoft 文件"
author: microsoft
description: "步驟 3 說明如何建立模型，我們可以使用這兩個查詢，並更新資料庫中我們 NerdDinner 應用程式。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: dbe6370979f218988c168df3e80314ef9b338fbd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="build-a-model-with-business-rule-validations"></a><span data-ttu-id="0f94f-103">建立使用商務規則驗證模型</span><span class="sxs-lookup"><span data-stu-id="0f94f-103">Build a Model with Business Rule Validations</span></span>
====================
<span data-ttu-id="0f94f-104">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0f94f-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="0f94f-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="0f94f-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="0f94f-106">這是一套免費的步驟 3 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。</span><span class="sxs-lookup"><span data-stu-id="0f94f-106">This is step 3 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="0f94f-107">步驟 3 說明如何建立模型，我們可以使用這兩個查詢，並更新資料庫中我們 NerdDinner 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0f94f-107">Step 3 shows how to create a model that we can use to both query and update the database for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="0f94f-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="0f94f-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-3-building-the-model"></a><span data-ttu-id="0f94f-109">NerdDinner 步驟 3： 建立模型</span><span class="sxs-lookup"><span data-stu-id="0f94f-109">NerdDinner Step 3: Building the Model</span></span>

<span data-ttu-id="0f94f-110">模型檢視控制器架構中 「 模型 」 一詞是指代表應用程式，以及與它整合驗證和商務規則的對應網域邏輯資料的物件。</span><span class="sxs-lookup"><span data-stu-id="0f94f-110">In a model-view-controller framework the term "model" refers to the objects that represent the data of the application, as well as the corresponding domain logic that integrates validation and business rules with it.</span></span> <span data-ttu-id="0f94f-111">模型在許多方面 「 活動 」 的 MVC 為基礎的應用程式，並稍後基本上，我們會看到磁碟機的行為。</span><span class="sxs-lookup"><span data-stu-id="0f94f-111">The model is in many ways the "heart" of an MVC-based application, and as we'll see later fundamentally drives the behavior of it.</span></span>

<span data-ttu-id="0f94f-112">ASP.NET MVC 架構支援使用任何資料存取技術，以及開發人員可以選擇各式各樣的豐富的.NET 資料選項，來實作其模型包括： LINQ to Entities 中，LINQ to SQL、 NHibernate、 LLBLGen Pro、 SubSonic、 WilsonORM 或只是未經處理的 ADO。NET Datareader 或資料集。</span><span class="sxs-lookup"><span data-stu-id="0f94f-112">The ASP.NET MVC framework supports using any data access technology, and developers can choose from a variety of rich .NET data options to implement their models including: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM, or just raw ADO.NET DataReaders or DataSets.</span></span>

<span data-ttu-id="0f94f-113">我們要建立簡單的模型相當緊密對應至我們的資料庫設計，並將某些自訂驗證邏輯和商務規則中使用 LINQ to SQL NerdDinner 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0f94f-113">For our NerdDinner application we are going to use LINQ to SQL to create a simple model that corresponds fairly closely to our database design, and adds some custom validation logic and business rules.</span></span> <span data-ttu-id="0f94f-114">我們再會實作儲存機制類別，可協助抽象離開於應用程式，以及可讓我們能夠輕鬆地將單元測試的其餘部分的資料持續性實作。</span><span class="sxs-lookup"><span data-stu-id="0f94f-114">We will then implement a repository class that helps abstract away the data persistence implementation from the rest of the application, and enables us to easily unit test it.</span></span>

### <a name="linq-to-sql"></a><span data-ttu-id="0f94f-115">LINQ to SQL</span><span class="sxs-lookup"><span data-stu-id="0f94f-115">LINQ to SQL</span></span>

<span data-ttu-id="0f94f-116">LINQ to SQL 是隨附於.NET 3.5 ORM （物件關聯式對應程式）。</span><span class="sxs-lookup"><span data-stu-id="0f94f-116">LINQ to SQL is an ORM (object relational mapper) that ships as part of .NET 3.5.</span></span>

<span data-ttu-id="0f94f-117">LINQ to SQL 提供輕鬆地將資料庫資料表對應到我們可以撰寫程式碼針對.NET 類別。</span><span class="sxs-lookup"><span data-stu-id="0f94f-117">LINQ to SQL provides an easy way to map database tables to .NET classes we can code against.</span></span> <span data-ttu-id="0f94f-118">我們將 Dinner 和 RSVP 類別我們資料庫中對應的 Dinners 和 RSVP 資料表使用 NerdDinner 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0f94f-118">For our NerdDinner application we'll use it to map the Dinners and RSVP tables within our database to Dinner and RSVP classes.</span></span> <span data-ttu-id="0f94f-119">Dinners 和 RSVP 資料表的資料行，會對應至為 Dinner 和 RSVP 類別上的屬性。</span><span class="sxs-lookup"><span data-stu-id="0f94f-119">The columns of the Dinners and RSVP tables will correspond to properties on the Dinner and RSVP classes.</span></span> <span data-ttu-id="0f94f-120">每個 Dinner 和 RSVP 物件將代表 Dinners 或 RSVP 資料表在資料庫內的個別資料列。</span><span class="sxs-lookup"><span data-stu-id="0f94f-120">Each Dinner and RSVP object will represent a separate row within the Dinners or RSVP tables in the database.</span></span>

<span data-ttu-id="0f94f-121">LINQ to SQL 可讓我們避免手動建構 SQL 陳述式來擷取和更新 Dinner 與 RSVP 與資料庫資料的物件。</span><span class="sxs-lookup"><span data-stu-id="0f94f-121">LINQ to SQL allows us to avoid having to manually construct SQL statements to retrieve and update Dinner and RSVP objects with database data.</span></span> <span data-ttu-id="0f94f-122">相反地，我們會定義 Dinner 和 RSVP 類別如何將它們對應到 azure 或從資料庫和它們之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="0f94f-122">Instead, we'll define the Dinner and RSVP classes, how they map to/from the database, and the relationships between them.</span></span> <span data-ttu-id="0f94f-123">LINQ to SQL 會接著會負責產生我們互動和使用它們時，在執行階段使用適當的 SQL 執行邏輯。</span><span class="sxs-lookup"><span data-stu-id="0f94f-123">LINQ to SQL will then takes care of generating the appropriate SQL execution logic to use at runtime when we interact and use them.</span></span>

<span data-ttu-id="0f94f-124">我們可以使用 VB 和 C# 中的 LINQ 語言支援，來撰寫易懂的查詢，會擷取 Dinner 和 RSVP 從資料庫物件。</span><span class="sxs-lookup"><span data-stu-id="0f94f-124">We can use the LINQ language support within VB and C# to write expressive queries that retrieve Dinner and RSVP objects from the database.</span></span> <span data-ttu-id="0f94f-125">這會盡量縮短資料程式碼，我們必須撰寫，並可讓我們建立真的全新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0f94f-125">This minimizes the amount of data code we need to write, and allows us to build really clean applications.</span></span>

### <a name="adding-linq-to-sql-classes-to-our-project"></a><span data-ttu-id="0f94f-126">將 LINQ to SQL 類別加入至受測專案</span><span class="sxs-lookup"><span data-stu-id="0f94f-126">Adding LINQ to SQL Classes to our project</span></span>

<span data-ttu-id="0f94f-127">我們將開始在專案中，「 模型 」 資料夾上按一下滑鼠右鍵，然後選取**新增-&gt;新項目**功能表命令：</span><span class="sxs-lookup"><span data-stu-id="0f94f-127">We'll begin by right-clicking on the "Models" folder within our project, and select the **Add-&gt;New Item** menu command:</span></span>

![](build-a-model-with-business-rule-validations/_static/image1.png)

<span data-ttu-id="0f94f-128">這會顯示 「 加入新項目 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="0f94f-128">This will bring up the "Add New Item" dialog.</span></span> <span data-ttu-id="0f94f-129">我們會依 「 資料 」 類別目錄篩選，然後選取 「 LINQ 到 SQL 的類別 」 範本內：</span><span class="sxs-lookup"><span data-stu-id="0f94f-129">We'll filter by the "Data" category and select the "LINQ to SQL Classes" template within it:</span></span>

![](build-a-model-with-business-rule-validations/_static/image2.png)

<span data-ttu-id="0f94f-130">我們會命名為"NerdDinner"的項目，然後按一下 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0f94f-130">We'll name the item "NerdDinner" and click the "Add" button.</span></span> <span data-ttu-id="0f94f-131">Visual Studio 會將我們 \Models 目錄下的 NerdDinner.dbml 檔案，並接著開啟 LINQ to SQL 物件關聯式設計工具：</span><span class="sxs-lookup"><span data-stu-id="0f94f-131">Visual Studio will add a NerdDinner.dbml file under our \Models directory, and then open the LINQ to SQL object relational designer:</span></span>

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a><span data-ttu-id="0f94f-132">建立資料模型類別搭配 LINQ to SQL</span><span class="sxs-lookup"><span data-stu-id="0f94f-132">Creating Data Model Classes with LINQ to SQL</span></span>

<span data-ttu-id="0f94f-133">LINQ to SQL 可讓我們快速地從現有的資料庫結構描述中建立資料模型類別。</span><span class="sxs-lookup"><span data-stu-id="0f94f-133">LINQ to SQL enables us to quickly create data model classes from existing database schema.</span></span> <span data-ttu-id="0f94f-134">待辦事項我們想要在其中建立模型的此我們會在 [伺服器總管] 中開啟 NerdDinner 資料庫，並選取的資料表：</span><span class="sxs-lookup"><span data-stu-id="0f94f-134">To-do this we'll open the NerdDinner database in the Server Explorer, and select the Tables we want to model in it:</span></span>

![](build-a-model-with-business-rule-validations/_static/image4.png)

<span data-ttu-id="0f94f-135">我們可以再將資料表拖曳至 LINQ 到 SQL 設計工具介面。</span><span class="sxs-lookup"><span data-stu-id="0f94f-135">We can then drag the tables onto the LINQ to SQL designer surface.</span></span> <span data-ttu-id="0f94f-136">當我們執行這個 LINQ to SQL 會自動建立 Dinner 和 RSVP 類別使用的結構描述的資料表 （對應至資料庫資料表資料行的類別內容）：</span><span class="sxs-lookup"><span data-stu-id="0f94f-136">When we do this LINQ to SQL will automatically create Dinner and RSVP classes using the schema of the tables (with class properties that map to the database table columns):</span></span>

![](build-a-model-with-business-rule-validations/_static/image5.png)

<span data-ttu-id="0f94f-137">預設 LINQ to SQL 設計工具自動"複數化 」 資料表和資料行名稱時，它會建立資料庫結構描述為基礎的類別。</span><span class="sxs-lookup"><span data-stu-id="0f94f-137">By default the LINQ to SQL designer automatically "pluralizes" table and column names when it creates classes based on a database schema.</span></span> <span data-ttu-id="0f94f-138">例如:"Dinners 」 資料表，在上述範例中會導致 「 Dinner 」 類別。</span><span class="sxs-lookup"><span data-stu-id="0f94f-138">For example: the "Dinners" table in our example above resulted in a "Dinner" class.</span></span> <span data-ttu-id="0f94f-139">此類別命名有助於讓我們的模型與.NET 的命名慣例，一致，而且我通常發現該設計工具修復這向上方便 （特別是當新增大量資料表）。</span><span class="sxs-lookup"><span data-stu-id="0f94f-139">This class naming helps make our models consistent with .NET naming conventions, and I usually find that having the designer fix this up convenient (especially when adding lots of tables).</span></span> <span data-ttu-id="0f94f-140">如果您不喜歡的類別或設計工具產生，但您可以一律覆寫它，並將它變更為任何您想要的名稱的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="0f94f-140">If you don't like the name of a class or property that the designer generates, though, you can always override it and change it to any name you want.</span></span> <span data-ttu-id="0f94f-141">您可以藉由編輯實體/屬性名稱內嵌在設計工具中，或藉由修改透過屬性方格。</span><span class="sxs-lookup"><span data-stu-id="0f94f-141">You can do this either by editing the entity/property name in-line within the designer or by modifying it via the property grid.</span></span>

<span data-ttu-id="0f94f-142">根據預設，LINQ to SQL 設計工具也會檢查主索引鍵/外部索引鍵關聯性的資料表，並根據它們自動會建立預設 」 關聯性的關聯"建立不同的模型類別之間。</span><span class="sxs-lookup"><span data-stu-id="0f94f-142">By default the LINQ to SQL designer also inspects the primary key/foreign key relationships of the tables, and based on them automatically creates default "relationship associations" between the different model classes it creates.</span></span> <span data-ttu-id="0f94f-143">例如，當我們拖曳 Dinners 和 RSVP 兩者之間的一對多關聯性關聯資料表拖曳至 LINQ to SQL 設計工具推斷基礎的 RSVP 資料表有外部索引鍵 Dinners 資料表 (中的箭號表示這一點設計工具）：</span><span class="sxs-lookup"><span data-stu-id="0f94f-143">For example, when we dragged the Dinners and RSVP tables onto the LINQ to SQL designer a one-to-many relationship association between the two was inferred based on the fact that the RSVP table had a foreign-key to the Dinners table (this is indicated by the arrow in the designer):</span></span>

![](build-a-model-with-business-rule-validations/_static/image6.png)

<span data-ttu-id="0f94f-144">上述的關聯將會使 LINQ to SQL 會將強類型的 「 Dinner"屬性加入至開發人員可用來存取指定 RSVP 相關聯的 Dinner RSVP 類別。</span><span class="sxs-lookup"><span data-stu-id="0f94f-144">The above association will cause LINQ to SQL to add a strongly typed "Dinner" property to the RSVP class that developers can use to access the Dinner associated with a given RSVP.</span></span> <span data-ttu-id="0f94f-145">它也會造成 Dinner 類別擁有 「 RSVPs 」 集合屬性，可讓開發人員，以擷取和更新 RSVP 特定 Dinner 相關聯的物件。</span><span class="sxs-lookup"><span data-stu-id="0f94f-145">It will also cause the Dinner class to have a "RSVPs" collection property that enables developers to retrieve and update RSVP objects associated with a particular Dinner.</span></span>

<span data-ttu-id="0f94f-146">以下您可以看見範例的 Visual Studio 中的 intellisense，當我們建立新的 RSVP 物件，並將它加入至 Dinner 與會者集合。</span><span class="sxs-lookup"><span data-stu-id="0f94f-146">Below you can see an example of intellisense within Visual Studio when we create a new RSVP object and add it to a Dinner's RSVPs collection.</span></span> <span data-ttu-id="0f94f-147">請注意如何 LINQ to SQL 會自動加入 「 與會者 」 集合 Dinner 物件上：</span><span class="sxs-lookup"><span data-stu-id="0f94f-147">Notice how LINQ to SQL automatically added a "RSVPs" collection on the Dinner object:</span></span>

![](build-a-model-with-business-rule-validations/_static/image7.png)

<span data-ttu-id="0f94f-148">將 RSVP 物件加入至 Dinner 與會者集合我們就是在告訴 LINQ to SQL 關聯外部索引鍵之間的關聯性 Dinner RSVP 資料列，在資料庫中：</span><span class="sxs-lookup"><span data-stu-id="0f94f-148">By adding the RSVP object to the Dinner's RSVPs collection we are telling LINQ to SQL to associate a foreign-key relationship between the Dinner and the RSVP row in our database:</span></span>

![](build-a-model-with-business-rule-validations/_static/image8.png)

<span data-ttu-id="0f94f-149">如果您不喜歡設計工具已建立模型或具名資料表關聯的方式，您可以覆寫它。</span><span class="sxs-lookup"><span data-stu-id="0f94f-149">If you don't like how the designer has modeled or named a table association, you can override it.</span></span> <span data-ttu-id="0f94f-150">只要按一下設計工具內的關聯箭號，並存取其透過重新命名、 刪除或修改它的屬性方格的屬性。</span><span class="sxs-lookup"><span data-stu-id="0f94f-150">Just click on the association arrow within the designer and access its properties via the property grid to rename, delete or modify it.</span></span> <span data-ttu-id="0f94f-151">NerdDinner 應用程式，不過，預設關聯規則適用於我們所建置的資料模型類別，我們就可以使用的預設行為。</span><span class="sxs-lookup"><span data-stu-id="0f94f-151">For our NerdDinner application, though, the default association rules work well for the data model classes we are building and we can just use the default behavior.</span></span>

### <a name="nerddinnerdatacontext-class"></a><span data-ttu-id="0f94f-152">NerdDinnerDataContext 類別</span><span class="sxs-lookup"><span data-stu-id="0f94f-152">NerdDinnerDataContext Class</span></span>

<span data-ttu-id="0f94f-153">Visual Studio 會自動建立.NET 類別，代表模型和資料庫使用 LINQ to SQL 設計工具定義的關聯性。</span><span class="sxs-lookup"><span data-stu-id="0f94f-153">Visual Studio will automatically create .NET classes that represent the models and database relationships defined using the LINQ to SQL designer.</span></span> <span data-ttu-id="0f94f-154">LINQ to SQL DataContext 類別也會產生每個 linq to SQL 設計工具檔案加入至方案。</span><span class="sxs-lookup"><span data-stu-id="0f94f-154">A LINQ to SQL DataContext class is also generated for each LINQ to SQL designer file added to the solution.</span></span> <span data-ttu-id="0f94f-155">我們名為我們的 LINQ to SQL 類別項目 」 NerdDinner"，因為建立的 DataContext 類別會呼叫 「 NerdDinnerDataContext"。</span><span class="sxs-lookup"><span data-stu-id="0f94f-155">Because we named our LINQ to SQL class item "NerdDinner", the DataContext class created will be called "NerdDinnerDataContext".</span></span> <span data-ttu-id="0f94f-156">這個 NerdDinnerDataContext 類別是我們將會與資料庫互動的主要方式。</span><span class="sxs-lookup"><span data-stu-id="0f94f-156">This NerdDinnerDataContext class is the primary way we will interact with the database.</span></span>

<span data-ttu-id="0f94f-157">我們 NerdDinnerDataContext 類別會公開兩個屬性的"Dinners"和"RSVPs"-表示我們包含在模型資料庫中的兩個資料表。</span><span class="sxs-lookup"><span data-stu-id="0f94f-157">Our NerdDinnerDataContext class exposes two properties - "Dinners" and "RSVPs" - that represent the two tables we modeled within the database.</span></span> <span data-ttu-id="0f94f-158">我們可以使用 C# 來從資料庫查詢和擷取 Dinner 和 RSVP 的物件寫入 LINQ 查詢，對這些屬性。</span><span class="sxs-lookup"><span data-stu-id="0f94f-158">We can use C# to write LINQ queries against those properties to query and retrieve Dinner and RSVP objects from the database.</span></span>

<span data-ttu-id="0f94f-159">下列程式碼會示範如何具現化 NerdDinnerDataContext 物件，並執行，以取得序列是未來的 Dinners 針對 LINQ 查詢。</span><span class="sxs-lookup"><span data-stu-id="0f94f-159">The following code demonstrates how to instantiate a NerdDinnerDataContext object and perform a LINQ query against it to obtain a sequence of Dinners that occur in the future.</span></span> <span data-ttu-id="0f94f-160">撰寫 LINQ 查詢時，visual Studio 提供完整的 intellisense，並從中傳回強型別和物件也支援 intellisense:</span><span class="sxs-lookup"><span data-stu-id="0f94f-160">Visual Studio provides full intellisense when writing the LINQ query, and the objects returned from it are strongly-typed and also support intellisense:</span></span>

![](build-a-model-with-business-rule-validations/_static/image9.png)

<span data-ttu-id="0f94f-161">讓我們為 Dinner 和 RSVP 物件查詢，除了 NerdDinnerDataContext 也會自動追蹤我們接下來我們透過它來擷取 Dinner 和 RSVP 物件所做的任何變更。</span><span class="sxs-lookup"><span data-stu-id="0f94f-161">In addition to allowing us to query for Dinner and RSVP objects, a NerdDinnerDataContext also automatically tracks any changes we subsequently make to the Dinner and RSVP objects we retrieve through it.</span></span> <span data-ttu-id="0f94f-162">若要輕鬆地將變更儲存回資料庫-而不需要撰寫任何明確的 SQL 更新程式碼，我們可以使用這項功能。</span><span class="sxs-lookup"><span data-stu-id="0f94f-162">We can use this functionality to easily save the changes back to the database - without having to write any explicit SQL update code.</span></span>

<span data-ttu-id="0f94f-163">例如，下列程式碼會示範如何使用 LINQ 查詢從資料庫中擷取單一的 Dinner 物件、 更新兩個 Dinner 屬性，然後再將變更儲存回資料庫：</span><span class="sxs-lookup"><span data-stu-id="0f94f-163">For example, the code below demonstrates how to use a LINQ query to retrieve a single Dinner object from the database, update two of the Dinner properties, and then save the changes back to the database:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

<span data-ttu-id="0f94f-164">NerdDinnerDataContext 物件上方的程式碼會自動追蹤對我們從它擷取 Dinner 物件屬性變更。</span><span class="sxs-lookup"><span data-stu-id="0f94f-164">The NerdDinnerDataContext object in the code above automatically tracked the property changes made to the Dinner object we retrieved from it.</span></span> <span data-ttu-id="0f94f-165">當我們稱為 「 SubmitChanges()"方法時，它會執行的適當 SQL 「 更新 」 陳述式到資料庫來保存回更新的值。</span><span class="sxs-lookup"><span data-stu-id="0f94f-165">When we called the "SubmitChanges()" method, it will execute an appropriate SQL "UPDATE" statement to the database to persist the updated values back.</span></span>

### <a name="creating-a-dinnerrepository-class"></a><span data-ttu-id="0f94f-166">建立 DinnerRepository 類別</span><span class="sxs-lookup"><span data-stu-id="0f94f-166">Creating a DinnerRepository Class</span></span>

<span data-ttu-id="0f94f-167">對於小型應用程式可能會很有控制站工作直接針對 LINQ to SQL DataContext 類別和內嵌控制器中的 LINQ 查詢會有問題。</span><span class="sxs-lookup"><span data-stu-id="0f94f-167">For small applications it is sometimes fine to have Controllers work directly against a LINQ to SQL DataContext class, and embed LINQ queries within the Controllers.</span></span> <span data-ttu-id="0f94f-168">當應用程式變大，不過，這種方法就容易維護和測試。</span><span class="sxs-lookup"><span data-stu-id="0f94f-168">As applications get larger, though, this approach becomes cumbersome to maintain and test.</span></span> <span data-ttu-id="0f94f-169">它也會導致我們複製相同 LINQ 查詢中的多個位置。</span><span class="sxs-lookup"><span data-stu-id="0f94f-169">It can also lead to us duplicating the same LINQ queries in multiple places.</span></span>

<span data-ttu-id="0f94f-170">可以讓您更輕鬆地維護並測試的應用程式的其中一個方法是使用 「 儲存機制 」 模式。</span><span class="sxs-lookup"><span data-stu-id="0f94f-170">One approach that can make applications easier to maintain and test is to use a "repository" pattern.</span></span> <span data-ttu-id="0f94f-171">儲存機制類別可協助封裝資料查詢，並持續性邏輯和摘要了從應用程式的資料持續性的實作詳細資料。</span><span class="sxs-lookup"><span data-stu-id="0f94f-171">A repository class helps encapsulate data querying and persistence logic, and abstracts away the implementation details of the data persistence from the application.</span></span> <span data-ttu-id="0f94f-172">除了讓應用程式程式碼更清楚，使用儲存機制模式可以讓您更輕鬆地在未來，變更資料儲存區實作，它可以幫助便利單元測試應用程式，而不需要實際的資料庫。</span><span class="sxs-lookup"><span data-stu-id="0f94f-172">In addition to making application code cleaner, using a repository pattern can make it easier to change data storage implementations in the future, and it can help facilitate unit testing an application without requiring a real database.</span></span>

<span data-ttu-id="0f94f-173">NerdDinner 應用程式中，我們會定義 DinnerRepository 類別具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="0f94f-173">For our NerdDinner application we'll define a DinnerRepository class with the below signature:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

<span data-ttu-id="0f94f-174">*注意： 本章稍後我們將 IDinnerRepository 介面擷取自這個類別，讓我們控制站上使用它的相依性插入。一開始，我們即將開始簡單和直接與 DinnerRepository 類別能夠運作。*</span><span class="sxs-lookup"><span data-stu-id="0f94f-174">*Note: Later in this chapter we'll extract an IDinnerRepository interface from this class and enable dependency injection with it on our Controllers. To begin with, though, we are going to start simple and just work directly with the DinnerRepository class.*</span></span>

<span data-ttu-id="0f94f-175">若要實作此類別，我們將在我們的 「 模型 」 資料夾上按一下滑鼠右鍵，然後選擇**新增-&gt;新項目**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="0f94f-175">To implement this class we'll right-click on our "Models" folder and choose the **Add-&gt;New Item** menu command.</span></span> <span data-ttu-id="0f94f-176">「 加入新項目 對話方塊中我們會選取 「 類別 」 範本，並將檔案命名為"DinnerRepository.cs 」:</span><span class="sxs-lookup"><span data-stu-id="0f94f-176">Within the "Add New Item" dialog we'll select the "Class" template and name the file "DinnerRepository.cs":</span></span>

![](build-a-model-with-business-rule-validations/_static/image10.png)

<span data-ttu-id="0f94f-177">然後，我們可以實作 DinnerRespository 類別使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="0f94f-177">We can then implement our DinnerRespository class using the code below:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a><span data-ttu-id="0f94f-178">擷取、 更新、 插入和刪除使用 DinnerRepository 類別</span><span class="sxs-lookup"><span data-stu-id="0f94f-178">Retrieving, Updating, Inserting and Deleting using the DinnerRepository class</span></span>

<span data-ttu-id="0f94f-179">現在，我們已建立 DinnerRepository 類別，讓我們看看一些程式碼範例示範如何使用它的一般工作：</span><span class="sxs-lookup"><span data-stu-id="0f94f-179">Now that we've created our DinnerRepository class, let's look at a few code examples that demonstrate common tasks we can do with it:</span></span>

#### <a name="querying-examples"></a><span data-ttu-id="0f94f-180">查詢範例</span><span class="sxs-lookup"><span data-stu-id="0f94f-180">Querying Examples</span></span>

<span data-ttu-id="0f94f-181">下列程式碼會擷取單一的 Dinner 使用 DinnerID 值：</span><span class="sxs-lookup"><span data-stu-id="0f94f-181">The code below retrieves a single Dinner using the DinnerID value:</span></span>


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

<span data-ttu-id="0f94f-182">下列程式碼會擷取所有即將 dinners 和迴圈它們：</span><span class="sxs-lookup"><span data-stu-id="0f94f-182">The code below retrieves all upcoming dinners and loops over them:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a><span data-ttu-id="0f94f-183">插入和更新範例</span><span class="sxs-lookup"><span data-stu-id="0f94f-183">Insert and Update Examples</span></span>

<span data-ttu-id="0f94f-184">下列程式碼示範如何將兩個新 dinners。</span><span class="sxs-lookup"><span data-stu-id="0f94f-184">The code below demonstrates adding two new dinners.</span></span> <span data-ttu-id="0f94f-185">加入/修改儲存機制不是向資料庫認可，直到其上呼叫"save （）"方法。</span><span class="sxs-lookup"><span data-stu-id="0f94f-185">Additions/modifications to the repository aren't committed to the database until the "Save()" method is called on it.</span></span> <span data-ttu-id="0f94f-186">就可能發生的所有變更或是沒有任何不要儲存我們的儲存機制時，LINQ to SQL 會自動在資料庫交易 – 包裝所有變更：</span><span class="sxs-lookup"><span data-stu-id="0f94f-186">LINQ to SQL automatically wraps all changes in a database transaction – so either all changes happen or none of them do when our repository saves:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

<span data-ttu-id="0f94f-187">下列程式碼擷取現有 Dinner 物件，並修改在其上的兩個屬性。</span><span class="sxs-lookup"><span data-stu-id="0f94f-187">The code below retrieves an existing Dinner object, and modifies two properties on it.</span></span> <span data-ttu-id="0f94f-188">我們的儲存機制上呼叫"save （）"方法時，在傳回至資料庫認可變更：</span><span class="sxs-lookup"><span data-stu-id="0f94f-188">The changes are committed back to the database when the "Save()" method is called on our repository:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

<span data-ttu-id="0f94f-189">下列程式碼擷取 dinner，並將其加入指明它。</span><span class="sxs-lookup"><span data-stu-id="0f94f-189">The code below retrieves a dinner and then adds an RSVP to it.</span></span> <span data-ttu-id="0f94f-190">它會使用 （因為這兩個資料庫之間沒有主索引鍵/外部索引鍵關聯性） 為我們建立 LINQ to SQL Dinner 物件上的與會者集合。</span><span class="sxs-lookup"><span data-stu-id="0f94f-190">It does this using the RSVPs collection on the Dinner object that LINQ to SQL created for us (because there is a primary-key/foreign-key relationship between the two in the database).</span></span> <span data-ttu-id="0f94f-191">這項變更保存回資料庫為新的 RSVP 資料表資料列儲存機制上呼叫"save （）"方法：</span><span class="sxs-lookup"><span data-stu-id="0f94f-191">This change is persisted back to the database as a new RSVP table row when the "Save()" method is called on the repository:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a><span data-ttu-id="0f94f-192">刪除範例</span><span class="sxs-lookup"><span data-stu-id="0f94f-192">Delete Example</span></span>

<span data-ttu-id="0f94f-193">下列程式碼擷取現有的 Dinner 物件，並隨後會將標示要刪除它。</span><span class="sxs-lookup"><span data-stu-id="0f94f-193">The code below retrieves an existing Dinner object, and then marks it to be deleted.</span></span> <span data-ttu-id="0f94f-194">儲存機制上呼叫"save （）"方法時它將會認可刪除資料庫：</span><span class="sxs-lookup"><span data-stu-id="0f94f-194">When the "Save()" method is called on the repository it will commit the delete back to the database:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a><span data-ttu-id="0f94f-195">整合驗證及商務規則邏輯與模型類別</span><span class="sxs-lookup"><span data-stu-id="0f94f-195">Integrating Validation and Business Rule Logic with Model Classes</span></span>

<span data-ttu-id="0f94f-196">驗證和商務規則邏輯是重要的部分資料適用於任何應用程式整合。</span><span class="sxs-lookup"><span data-stu-id="0f94f-196">Integrating validation and business rule logic is a key part of any application that works with data.</span></span>

#### <a name="schema-validation"></a><span data-ttu-id="0f94f-197">結構描述驗證</span><span class="sxs-lookup"><span data-stu-id="0f94f-197">Schema Validation</span></span>

<span data-ttu-id="0f94f-198">當使用 LINQ to SQL 設計工具來定義模型類別時，資料模型類別中屬性的資料類型會對應到資料庫資料表的資料類型。</span><span class="sxs-lookup"><span data-stu-id="0f94f-198">When model classes are defined using the LINQ to SQL designer, the datatypes of the properties in the data model classes correspond to the datatypes of the database table.</span></span> <span data-ttu-id="0f94f-199">例如:"datetime"Dinners 資料表中的"EventDate 」 資料行時，LINQ to SQL 所建立的資料模型類別，都屬於類型"DateTime"（這是內建的.NET 資料類型）。</span><span class="sxs-lookup"><span data-stu-id="0f94f-199">For example: if the "EventDate" column in the Dinners table is a "datetime", the data model class created by LINQ to SQL will be of type "DateTime" (which is a built-in .NET datatype).</span></span> <span data-ttu-id="0f94f-200">這表示如果您嘗試從程式碼中，將整數或布林值給它，而且它會引發錯誤自動如果您嘗試在執行階段隱含轉換成它的無效字串型別，會發生編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="0f94f-200">This means you will get compile errors if you attempt to assign an integer or boolean to it from code, and it will raise an error automatically if you attempt to implicitly convert a non-valid string type to it at runtime.</span></span>

<span data-ttu-id="0f94f-201">使用的字串，可協助防止 SQL 資料隱碼攻擊時使用它時 LINQ to SQL 也會自動將為您處理逸出 SQL 值。</span><span class="sxs-lookup"><span data-stu-id="0f94f-201">LINQ to SQL will also automatically handles escaping SQL values for you when using strings - which helps protect you against SQL injection attacks when using it.</span></span>

#### <a name="validation-and-business-rule-logic"></a><span data-ttu-id="0f94f-202">驗證及商務規則邏輯</span><span class="sxs-lookup"><span data-stu-id="0f94f-202">Validation and Business Rule Logic</span></span>

<span data-ttu-id="0f94f-203">結構描述驗證第一個步驟中，但是便很少。</span><span class="sxs-lookup"><span data-stu-id="0f94f-203">Schema validation is useful as a first step, but is rarely sufficient.</span></span> <span data-ttu-id="0f94f-204">大部分的真實世界案例需要指定更豐富的驗證邏輯，跨越多個屬性、 執行程式碼，以及經常會有不同的模型狀態感知的功能 (例如： 它正在建立/更新/刪除，或網域特有狀態中例如 「 封存 」）。</span><span class="sxs-lookup"><span data-stu-id="0f94f-204">Most real-world scenarios require the ability to specify richer validation logic that can span multiple properties, execute code, and often have awareness of a model's state (for example: is it being created /updated/deleted, or within a domain-specific state like "archived").</span></span> <span data-ttu-id="0f94f-205">有各種不同的模式和架構，可用來定義，並將驗證規則套用至模型類別，並有數個基礎.NET 架構有可以用來提供協助。</span><span class="sxs-lookup"><span data-stu-id="0f94f-205">There are a variety of different patterns and frameworks that can be used to define and apply validation rules to model classes, and there are several .NET based frameworks out there that can be used to help with this.</span></span> <span data-ttu-id="0f94f-206">您可以使用幾乎任何的 ASP.NET MVC 應用程式中將它們。</span><span class="sxs-lookup"><span data-stu-id="0f94f-206">You can use pretty much any of them within ASP.NET MVC applications.</span></span>

<span data-ttu-id="0f94f-207">基於我們 NerdDinner 應用程式的目的，我們會使用相對較簡單且直接的模式，我們展現 IsValid 屬性和 GetRuleViolations() 方法我們 Dinner 模型物件上。</span><span class="sxs-lookup"><span data-stu-id="0f94f-207">For the purposes of our NerdDinner application, we'll use a relatively simple and straight-forward pattern where we expose an IsValid property and a GetRuleViolations() method on our Dinner model object.</span></span> <span data-ttu-id="0f94f-208">IsValid 屬性會傳回 true 或 false，根據的驗證和商務規則是否有效。</span><span class="sxs-lookup"><span data-stu-id="0f94f-208">The IsValid property will return true or false depending on whether the validation and business rules are all valid.</span></span> <span data-ttu-id="0f94f-209">GetRuleViolations() 方法會傳回任何規則錯誤的清單。</span><span class="sxs-lookup"><span data-stu-id="0f94f-209">The GetRuleViolations() method will return a list of any rule errors.</span></span>

<span data-ttu-id="0f94f-210">我們將為我們 Dinner 模型實作 IsValid 和 GetRuleViolations()，方法是加入受測專案的 「 部分類別 」。</span><span class="sxs-lookup"><span data-stu-id="0f94f-210">We'll implement IsValid and GetRuleViolations() for our Dinner model by adding a "partial class" to our project.</span></span> <span data-ttu-id="0f94f-211">部分類別可用來將方法/屬性/事件加入至類別維護的 VS 設計工具 （例如 Dinner 所產生的類別，LINQ to SQL 設計工具），並幫助避免從觸及我們的程式碼的工具。</span><span class="sxs-lookup"><span data-stu-id="0f94f-211">Partial classes can be used to add methods/properties/events to classes maintained by a VS designer (like the Dinner class generated by the LINQ to SQL designer) and help avoid the tool from messing with our code.</span></span> <span data-ttu-id="0f94f-212">我們可以 \Models 資料夾上按一下滑鼠右鍵，將新的部分類別加入至受測專案，然後選取 加入新項目 」 的功能表命令。</span><span class="sxs-lookup"><span data-stu-id="0f94f-212">We can add a new partial class to our project by right-clicking on the \Models folder, and then select the "Add New Item" menu command.</span></span> <span data-ttu-id="0f94f-213">我們可以再選擇 加入新項目 」 對話方塊中的 「 類別 」 範本，並將它命名為 Dinner.cs 中。</span><span class="sxs-lookup"><span data-stu-id="0f94f-213">We can then choose the "Class" template within the "Add New Item" dialog and name it Dinner.cs.</span></span>

![](build-a-model-with-business-rule-validations/_static/image11.png)

<span data-ttu-id="0f94f-214">按一下 [新增] 按鈕，將 Dinner.cs 檔案加入專案，並在 IDE 中開啟。</span><span class="sxs-lookup"><span data-stu-id="0f94f-214">Clicking the "Add" button will add a Dinner.cs file to our project and open it within the IDE.</span></span> <span data-ttu-id="0f94f-215">接著可以實作基本的規則驗證強制 framework 使用程式碼下方：</span><span class="sxs-lookup"><span data-stu-id="0f94f-215">We can then implement a basic rule/validation enforcement framework using the below code:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

<span data-ttu-id="0f94f-216">上述程式碼的一些注意事項：</span><span class="sxs-lookup"><span data-stu-id="0f94f-216">A few notes about the above code:</span></span>

- <span data-ttu-id="0f94f-217">Dinner 類別開頭處以 「 部分 」 關鍵字 – 這表示，其中包含的程式碼會結合的 LINQ to SQL 設計工具的產生和保留的類別，並編譯成單一類別。</span><span class="sxs-lookup"><span data-stu-id="0f94f-217">The Dinner class is prefaced with a "partial" keyword – which means the code contained within it will be combined with the class generated/maintained by the LINQ to SQL designer and compiled into a single class.</span></span>
- <span data-ttu-id="0f94f-218">RuleViolation 類別是我們會加入專案，讓我們提供更多詳細的規則違規的 helper 類別。</span><span class="sxs-lookup"><span data-stu-id="0f94f-218">The RuleViolation class is a helper class we'll add to the project that allows us to provide more details about a rule violation.</span></span>
- <span data-ttu-id="0f94f-219">Dinner.GetRuleViolations() 方法會導致我們驗證和商務規則進行評估 （我們會實作它們很快）。</span><span class="sxs-lookup"><span data-stu-id="0f94f-219">The Dinner.GetRuleViolations() method causes our validation and business rules to be evaluated (we'll implement them shortly).</span></span> <span data-ttu-id="0f94f-220">它接著會傳回回 RuleViolation 物件提供任何規則錯誤的更多詳細的序列。</span><span class="sxs-lookup"><span data-stu-id="0f94f-220">It then returns back a sequence of RuleViolation objects that provide more details about any rule errors.</span></span>
- <span data-ttu-id="0f94f-221">Dinner.IsValid 屬性提供一個便利的協助程式屬性，指出 Dinner 物件是否有任何作用中 RuleViolations。</span><span class="sxs-lookup"><span data-stu-id="0f94f-221">The Dinner.IsValid property provides a convenient helper property that indicates whether the Dinner object has any active RuleViolations.</span></span> <span data-ttu-id="0f94f-222">它能夠主動檢查由開發人員隨時使用 Dinner 物件 （並不會引發例外狀況）。</span><span class="sxs-lookup"><span data-stu-id="0f94f-222">It can be proactively checked by a developer using the Dinner object at anytime (and does not raise an exception).</span></span>
- <span data-ttu-id="0f94f-223">Dinner.OnValidate() 部分方法是讓我們隨時 Dinner 物件即將保存在資料庫中會收到通知 LINQ to SQL 提供的攔截。</span><span class="sxs-lookup"><span data-stu-id="0f94f-223">The Dinner.OnValidate() partial method is a hook that LINQ to SQL provides that allows us to be notified anytime the Dinner object is about to be persisted within the database.</span></span> <span data-ttu-id="0f94f-224">上述我們 OnValidate() 實作可確保當 Dinner 會在儲存之前會有任何 RuleViolations。</span><span class="sxs-lookup"><span data-stu-id="0f94f-224">Our OnValidate() implementation above ensures that the Dinner has no RuleViolations before it is saved.</span></span> <span data-ttu-id="0f94f-225">如果它處於無效的狀態，則會引發例外狀況，這會導致 LINQ to SQL 中止交易。</span><span class="sxs-lookup"><span data-stu-id="0f94f-225">If it is in an invalid state it raises an exception, which will cause LINQ to SQL to abort the transaction.</span></span>

<span data-ttu-id="0f94f-226">此方法提供一種簡單的架構，我們可以將驗證及整合到商務規則。</span><span class="sxs-lookup"><span data-stu-id="0f94f-226">This approach provides a simple framework that we can integrate validation and business rules into.</span></span> <span data-ttu-id="0f94f-227">現在讓我們加入下列規則來 GetRuleViolations() 方法：</span><span class="sxs-lookup"><span data-stu-id="0f94f-227">For now let's add the below rules to our GetRuleViolations() method:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

<span data-ttu-id="0f94f-228">我們使用 「 產生傳回"功能的 C# 傳回的任何 RuleViolations 序列。</span><span class="sxs-lookup"><span data-stu-id="0f94f-228">We are using the "yield return" feature of C# to return a sequence of any RuleViolations.</span></span> <span data-ttu-id="0f94f-229">上述的第六個規則檢查只會強制執行我們 Dinner 字串屬性不能為 null 或空白。</span><span class="sxs-lookup"><span data-stu-id="0f94f-229">The first six rule checks above simply enforce that string properties on our Dinner cannot be null or empty.</span></span> <span data-ttu-id="0f94f-230">上一個規則更有趣的是，並呼叫 PhoneValidator.IsValidNumber() helper 方法，我們加入至受測專案可讓您確認 ContactPhone 數字格式相符項目 Dinner 的國家/地區。</span><span class="sxs-lookup"><span data-stu-id="0f94f-230">The last rule is a little more interesting, and calls a PhoneValidator.IsValidNumber() helper method that we can add to our project to verify that the ContactPhone number format matches the Dinner's country.</span></span>

<span data-ttu-id="0f94f-231">我們可以使用。若要實作這項電話驗證支援網路的規則運算式支援。</span><span class="sxs-lookup"><span data-stu-id="0f94f-231">We can use .NET's regular expression support to implement this phone validation support.</span></span> <span data-ttu-id="0f94f-232">以下是我們可以將加入的簡單 PhoneValidator 實作可讓我們加入國家/地區特定 Regex 模式檢查，我們專案：</span><span class="sxs-lookup"><span data-stu-id="0f94f-232">Below is a simple PhoneValidator implementation that we can add to our project that enables us to add country-specific Regex pattern checks:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a><span data-ttu-id="0f94f-233">處理驗證和商務邏輯違規</span><span class="sxs-lookup"><span data-stu-id="0f94f-233">Handling Validation and Business Logic Violations</span></span>

<span data-ttu-id="0f94f-234">既然我們已經加入上述驗證和商務規則程式碼，我們嘗試建立或更新 Dinner 任何時間，我們的驗證邏輯規則會進行評估並強制執行。</span><span class="sxs-lookup"><span data-stu-id="0f94f-234">Now that we've added the above validation and business rule code, any time we try to create or update a Dinner, our validation logic rules will be evaluated and enforced.</span></span>

<span data-ttu-id="0f94f-235">開發人員可以撰寫程式碼類似下面主動判斷 Dinner 物件是否有效，並擷取所有違規，而不會引發任何例外狀況的清單：</span><span class="sxs-lookup"><span data-stu-id="0f94f-235">Developers can write code like below to proactively determine if a Dinner object is valid, and retrieve a list of all violations in it without raising any exceptions:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

<span data-ttu-id="0f94f-236">如果我們嘗試儲存 Dinner 處於無效狀態，當我們 DinnerRepository 上呼叫 save （） 方法，就會引發例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0f94f-236">If we attempt to save a Dinner in an invalid state, an exception will be raised when we call the Save() method on the DinnerRepository.</span></span> <span data-ttu-id="0f94f-237">這是因為 LINQ to SQL 自動我們 Dinner.OnValidate() 部分方法之前呼叫它 Dinner 將變更儲存，而我們將程式碼加入 Dinner.OnValidate() Dinner 存在於任何規則違規時引發例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0f94f-237">This occurs because LINQ to SQL automatically calls our Dinner.OnValidate() partial method before it saves the Dinner's changes, and we added code to Dinner.OnValidate() to raise an exception if any rule violations exist in the Dinner.</span></span> <span data-ttu-id="0f94f-238">我們可以捕捉此例外狀況及被動擷取違規，若要修正清單：</span><span class="sxs-lookup"><span data-stu-id="0f94f-238">We can catch this exception and reactively retrieve a list of the violations to fix:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

<span data-ttu-id="0f94f-239">因為我們驗證和商務規則中實作，在我們的模型層和不在 UI 層，會套用，會在所有情況下，我們的應用程式內使用。</span><span class="sxs-lookup"><span data-stu-id="0f94f-239">Because our validation and business rules are implemented within our model layer, and not within the UI layer, they will be applied and used across all scenarios within our application.</span></span> <span data-ttu-id="0f94f-240">我們稍後可以變更或新增商務規則，可搭配我們 Dinner 物件接受這些所有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0f94f-240">We can later change or add business rules and have all code that works with our Dinner objects honor them.</span></span>

<span data-ttu-id="0f94f-241">有彈性地變更商務規則，在一個地方，而不需 ripple 整個應用程式和 UI 邏輯，這些變更是正負號的編寫完善的應用程式與 MVC framework 有助於鼓勵的好處。</span><span class="sxs-lookup"><span data-stu-id="0f94f-241">Having the flexibility to change business rules in one place, without having these changes ripple throughout the application and UI logic, is a sign of a well-written application, and a benefit that an MVC framework helps encourage.</span></span>

### <a name="next-step"></a><span data-ttu-id="0f94f-242">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="0f94f-242">Next Step</span></span>

<span data-ttu-id="0f94f-243">我們現在有我們可以使用查詢和更新資料庫的模型。</span><span class="sxs-lookup"><span data-stu-id="0f94f-243">We've now got a model that we can use to both query and update our database.</span></span>

<span data-ttu-id="0f94f-244">我們現在加入某些控制器和檢視的專案，我們可以使用它來建立其周圍的 HTML UI 體驗。</span><span class="sxs-lookup"><span data-stu-id="0f94f-244">Let's now add some controllers and views to the project that we can use to build an HTML UI experience around it.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0f94f-245">[上一頁](create-a-database.md)
[下一頁](use-controllers-and-views-to-implement-a-listingdetails-ui.md)</span><span class="sxs-lookup"><span data-stu-id="0f94f-245">[Previous](create-a-database.md)
[Next](use-controllers-and-views-to-implement-a-listingdetails-ui.md)</span></span>
