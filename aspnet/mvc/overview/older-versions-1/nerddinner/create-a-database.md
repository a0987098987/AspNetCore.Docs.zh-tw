---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: 建立資料庫 |Microsoft Docs
author: microsoft
description: 步驟 2 顯示建立資料庫的保留所有 dinner 和回覆我們 NerdDinner 應用程式資料的步驟。
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b6ab0740f251889f0fa0561809cac2bbe79bcb3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826804"
---
<a name="create-a-database"></a><span data-ttu-id="aab2e-103">建立資料庫</span><span class="sxs-lookup"><span data-stu-id="aab2e-103">Create a Database</span></span>
====================
<span data-ttu-id="aab2e-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="aab2e-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="aab2e-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="aab2e-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="aab2e-106">這是一套免費的步驟 2 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="aab2e-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="aab2e-107">步驟 2 顯示建立資料庫的保留所有 dinner 和回覆我們 NerdDinner 應用程式資料的步驟。</span><span class="sxs-lookup"><span data-stu-id="aab2e-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="aab2e-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="aab2e-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="aab2e-109">NerdDinner 步驟 2： 建立資料庫</span><span class="sxs-lookup"><span data-stu-id="aab2e-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="aab2e-110">我們將使用資料庫來儲存所有 Dinner 和 RSVP 資料 NerdDinner 應用程式。</span><span class="sxs-lookup"><span data-stu-id="aab2e-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="aab2e-111">下列步驟會示範如何建立使用免費的 SQL Server Express edition 資料庫 (您可以輕鬆地安裝使用的 V2 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx))。</span><span class="sxs-lookup"><span data-stu-id="aab2e-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="aab2e-112">我們將撰寫的程式碼的所有適用於 SQL Server Express 和 SQL Server 完整版。</span><span class="sxs-lookup"><span data-stu-id="aab2e-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="aab2e-113">建立新的 SQL Server Express 資料庫</span><span class="sxs-lookup"><span data-stu-id="aab2e-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="aab2e-114">我們會開始透過我們的 web 專案上按一下滑鼠右鍵，然後選取**Add-&gt;新的項目**功能表命令：</span><span class="sxs-lookup"><span data-stu-id="aab2e-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="aab2e-115">這會顯示 Visual Studio 的 「 新增新的項目 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="aab2e-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="aab2e-116">我們會依據 「 資料 」 類別目錄篩選，並選取 「 SQL Server 資料庫 」 項目範本：</span><span class="sxs-lookup"><span data-stu-id="aab2e-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="aab2e-117">我們將會命名為我們想要建立 「 NerdDinner.mdf"，然後按 [確定] 在 SQL Server Express 的資料庫。</span><span class="sxs-lookup"><span data-stu-id="aab2e-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="aab2e-118">Visual Studio 會接著詢問我們是否我們想要將這個檔案加入我們 \App\_資料目錄 （即已設定與讀取和寫入安全性 Acl）：</span><span class="sxs-lookup"><span data-stu-id="aab2e-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="aab2e-119">我們按一下 是，和我們新的資料庫會建立並加入我們的方案總管 中：</span><span class="sxs-lookup"><span data-stu-id="aab2e-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="aab2e-120">建立資料庫中的資料表</span><span class="sxs-lookup"><span data-stu-id="aab2e-120">Creating Tables within our Database</span></span>

<span data-ttu-id="aab2e-121">我們現在有新的空白資料庫。</span><span class="sxs-lookup"><span data-stu-id="aab2e-121">We now have a new empty database.</span></span> <span data-ttu-id="aab2e-122">讓我們新增一些資料表。</span><span class="sxs-lookup"><span data-stu-id="aab2e-122">Let's add some tables to it.</span></span>

<span data-ttu-id="aab2e-123">若要這樣做，我們會瀏覽可讓我們來管理資料庫和伺服器中的 Visual Studio 中 [伺服器總管] 索引標籤視窗。</span><span class="sxs-lookup"><span data-stu-id="aab2e-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="aab2e-124">SQL Server Express 資料庫儲存在 \App\_我們的應用程式資料資料夾會自動顯示在 [伺服器總管] 中。</span><span class="sxs-lookup"><span data-stu-id="aab2e-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="aab2e-125">選擇性地，我們可以使用 [伺服器總管] 視窗頂端的 [連接到資料庫] 圖示將其他的 SQL Server 資料庫 （本機和遠端） 新增至清單以及：</span><span class="sxs-lookup"><span data-stu-id="aab2e-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="aab2e-126">NerdDinner 資料庫 – 一個用來儲存我們 Dinners，和其他追蹤 RSVP 接受它們，我們會新增兩個資料表。</span><span class="sxs-lookup"><span data-stu-id="aab2e-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="aab2e-127">我們可以建立新的資料表，在資料庫裡面的 「 資料表 」 資料夾上按一下滑鼠右鍵，然後選擇 「 加入新的資料表 」 功能表命令：</span><span class="sxs-lookup"><span data-stu-id="aab2e-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="aab2e-128">這會開啟可讓我們設定的資料表結構描述的資料表設計工具。</span><span class="sxs-lookup"><span data-stu-id="aab2e-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="aab2e-129">我們 「 Dinners 」 資料表中，我們會將加入 10 個資料行的資料：</span><span class="sxs-lookup"><span data-stu-id="aab2e-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="aab2e-130">我們想要 「 DinnerID 」 資料行資料表唯一主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="aab2e-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="aab2e-131">我們可以將它設定"DinnerID 」 資料行上按一下滑鼠右鍵，然後選擇 [設定主要金鑰] 功能表項目：</span><span class="sxs-lookup"><span data-stu-id="aab2e-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="aab2e-132">除了更 DinnerID 主索引鍵，我們也想要將它設定為新的資料列加入至資料表，其值會自動遞增的 「 身分識別 」 資料行 （亦即第一個插入的 Dinner 資料列會有 DinnerID 為 1，第二個插入資料列將具有 DinnerID 2 等等)。</span><span class="sxs-lookup"><span data-stu-id="aab2e-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="aab2e-133">我們可以這麼做的選取"DinnerID 」 資料行，並再使用 [資料行屬性] 編輯器來設定 「 （」 身分識別） 的屬性為"Yes"的資料行上。</span><span class="sxs-lookup"><span data-stu-id="aab2e-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="aab2e-134">我們將使用標準的識別預設值 （從 1 開始，並遞增每個新的 Dinner 資料列上的 1）：</span><span class="sxs-lookup"><span data-stu-id="aab2e-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="aab2e-135">按 CTRL-S 或使用，我們會接著儲存資料表**檔案&gt;儲存**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="aab2e-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="aab2e-136">這會提示我們命名的資料表。</span><span class="sxs-lookup"><span data-stu-id="aab2e-136">This will prompt us to name the table.</span></span> <span data-ttu-id="aab2e-137">我們將它 「 Dinners"命名：</span><span class="sxs-lookup"><span data-stu-id="aab2e-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="aab2e-138">我們新的 Dinners 資料表會再顯示在伺服器總管 中，在資料庫裡面。</span><span class="sxs-lookup"><span data-stu-id="aab2e-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="aab2e-139">然後，我們會重複上述步驟，並建立 「 RSVP 」 資料表。</span><span class="sxs-lookup"><span data-stu-id="aab2e-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="aab2e-140">此表格中的有 3 個資料行。</span><span class="sxs-lookup"><span data-stu-id="aab2e-140">This table with have 3 columns.</span></span> <span data-ttu-id="aab2e-141">我們將設定 RsvpID 做的資料行主要金鑰，並也讓身分識別資料行：</span><span class="sxs-lookup"><span data-stu-id="aab2e-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="aab2e-142">我們會將它儲存，並提供名稱 「 RSVP"。</span><span class="sxs-lookup"><span data-stu-id="aab2e-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="aab2e-143">設定資料表之間的外部索引鍵關聯性</span><span class="sxs-lookup"><span data-stu-id="aab2e-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="aab2e-144">我們現在會在資料庫裡面有兩個資料表。</span><span class="sxs-lookup"><span data-stu-id="aab2e-144">We now have two tables within our database.</span></span> <span data-ttu-id="aab2e-145">我們最後一個結構描述設計步驟會設定 「 一對多 」 之間的關聯性這兩個資料表 –，以便我們可以對其套用的零或多個 RSVP 資料列與相關聯的每一列 Dinner。</span><span class="sxs-lookup"><span data-stu-id="aab2e-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="aab2e-146">我們會設定"Dinners 」 資料表中有"DinnerID 」 資料行的外部索引鍵關聯性的 RSVP 資料表的"DinnerID 」 資料行來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="aab2e-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="aab2e-147">若要這樣做我們會藉由在 [伺服器總管] 中按兩下開啟資料表設計工具內的 RSVP 資料表。</span><span class="sxs-lookup"><span data-stu-id="aab2e-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="aab2e-148">我們會選取 「 DinnerID 」 內的資料行，以滑鼠右鍵按一下，然後選擇 [Relationshps] 內容功能表命令：</span><span class="sxs-lookup"><span data-stu-id="aab2e-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="aab2e-149">這會顯示一個對話方塊，可用來在資料表之間的關聯性設定：</span><span class="sxs-lookup"><span data-stu-id="aab2e-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="aab2e-150">我們會按一下 [新增] 按鈕，將新的關聯性新增至對話方塊。</span><span class="sxs-lookup"><span data-stu-id="aab2e-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="aab2e-151">一旦新增關聯性之後，我們將右邊的對話方塊中，展開 「 資料表和資料行規格 」 樹狀檢視節點，在屬性方格中的，然後按一下 ... 按鈕右邊：</span><span class="sxs-lookup"><span data-stu-id="aab2e-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="aab2e-152">按一下 ... 按鈕會顯示另一個對話方塊，讓我們指定的資料表和資料行參與關聯性，以及讓我們命名關聯性。</span><span class="sxs-lookup"><span data-stu-id="aab2e-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="aab2e-153">我們會變更主索引鍵資料表要 「 Dinners"，並選取 Dinners 資料表中的"DinnerID 」 資料行的主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="aab2e-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="aab2e-154">我們的 RSVP 資料表是外部索引鍵資料表和 RSVP。DinnerID 資料行將會是做為外部索引鍵相關聯的：</span><span class="sxs-lookup"><span data-stu-id="aab2e-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="aab2e-155">現在 RSVP 資料表中的每個資料列會與 Dinner 資料表中的資料列相關聯。</span><span class="sxs-lookup"><span data-stu-id="aab2e-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="aab2e-156">SQL Server 會為我們 – 維護參考完整性，並讓我們無法新增新的 RSVP 資料列，如果它不是指向有效的 Dinner 資料列。</span><span class="sxs-lookup"><span data-stu-id="aab2e-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="aab2e-157">它也會防止我們刪除 Dinner 資料列，如果有仍然 RSVP 參考它的資料列。</span><span class="sxs-lookup"><span data-stu-id="aab2e-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="aab2e-158">將資料加入至資料表</span><span class="sxs-lookup"><span data-stu-id="aab2e-158">Adding Data to our Tables</span></span>

<span data-ttu-id="aab2e-159">讓我們先完成我們 Dinners 資料表中加入一些範例資料。</span><span class="sxs-lookup"><span data-stu-id="aab2e-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="aab2e-160">我們可以用滑鼠右鍵按一下它在 [伺服器總管] 中，然後選擇 [顯示資料表資料] 命令，將資料新增至資料表：</span><span class="sxs-lookup"><span data-stu-id="aab2e-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="aab2e-161">我們將新增幾個資料列的 Dinner 稍後當我們開始實作應用程式，我們可以使用：</span><span class="sxs-lookup"><span data-stu-id="aab2e-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="aab2e-162">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="aab2e-162">Next Step</span></span>

<span data-ttu-id="aab2e-163">我們已經完成建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="aab2e-163">We've finished creating our database.</span></span> <span data-ttu-id="aab2e-164">我們現在來建立模型類別，我們可以用來查詢和更新它。</span><span class="sxs-lookup"><span data-stu-id="aab2e-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aab2e-165">[上一頁](create-a-new-aspnet-mvc-project.md)
> [下一頁](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="aab2e-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
