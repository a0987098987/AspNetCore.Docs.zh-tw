---
uid: web-pages/overview/data/5-working-with-data
title: 簡介使用的資料庫，在 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 本章說明如何從資料庫存取資料，並顯示其使用 ASP.NET Web Pages。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 1ee2f680d6088ad786022b3c590c33e4e25e04db
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363354"
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="9fc43-103">簡介使用的資料庫，在 ASP.NET Web Pages (Razor) 網站</span><span class="sxs-lookup"><span data-stu-id="9fc43-103">Introduction to Working with a Database in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="9fc43-104">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9fc43-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9fc43-105">本文說明如何使用 Microsoft WebMatrix 工具來建立資料庫，在 ASP.NET Web Pages (Razor) 網站中，以及如何建立可讓您顯示、 新增、 編輯和刪除資料的頁面。</span><span class="sxs-lookup"><span data-stu-id="9fc43-105">This article describes how to use Microsoft WebMatrix tools to create a database in an ASP.NET Web Pages (Razor) website, and how to create pages that let you display, add, edit, and delete data.</span></span>
> 
> <span data-ttu-id="9fc43-106">**您將學到什麼：**</span><span class="sxs-lookup"><span data-stu-id="9fc43-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="9fc43-107">如何建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="9fc43-107">How to create a database.</span></span>
> - <span data-ttu-id="9fc43-108">如何連接到資料庫。</span><span class="sxs-lookup"><span data-stu-id="9fc43-108">How to connect to a database.</span></span>
> - <span data-ttu-id="9fc43-109">如何在網頁上顯示資料。</span><span class="sxs-lookup"><span data-stu-id="9fc43-109">How to display data in a web page.</span></span>
> - <span data-ttu-id="9fc43-110">如何插入、 更新和刪除資料庫中的記錄。</span><span class="sxs-lookup"><span data-stu-id="9fc43-110">How to insert, update, and delete database records.</span></span>
> 
> <span data-ttu-id="9fc43-111">以下是文件中所引進的功能：</span><span class="sxs-lookup"><span data-stu-id="9fc43-111">These are the features introduced in the article:</span></span>
> 
> - <span data-ttu-id="9fc43-112">使用 Microsoft SQL Server Compact Edition 資料庫。</span><span class="sxs-lookup"><span data-stu-id="9fc43-112">Working with a Microsoft SQL Server Compact Edition database.</span></span>
> - <span data-ttu-id="9fc43-113">使用 SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="9fc43-113">Working with SQL queries.</span></span>
> - <span data-ttu-id="9fc43-114">`Database` 類別。</span><span class="sxs-lookup"><span data-stu-id="9fc43-114">The `Database` class.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9fc43-115">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="9fc43-115">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9fc43-116">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="9fc43-116">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="9fc43-117">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="9fc43-117">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="9fc43-118">本教學課程也適用於 WebMatrix 3。</span><span class="sxs-lookup"><span data-stu-id="9fc43-118">This tutorial also works with WebMatrix 3.</span></span> <span data-ttu-id="9fc43-119">您可以使用 ASP.NET Web Pages 3 和 Visual Studio 2013 （或 Visual Studio Express 2013 for Web）;不過，使用者介面將會不同。</span><span class="sxs-lookup"><span data-stu-id="9fc43-119">You can use ASP.NET Web Pages 3 and Visual Studio 2013 (or Visual Studio Express 2013 for Web); however, the user interface will be different.</span></span>


## <a name="introduction-to-databases"></a><span data-ttu-id="9fc43-120">資料庫的簡介</span><span class="sxs-lookup"><span data-stu-id="9fc43-120">Introduction to Databases</span></span>

<span data-ttu-id="9fc43-121">想像一下一個典型的通訊錄。</span><span class="sxs-lookup"><span data-stu-id="9fc43-121">Imagine a typical address book.</span></span> <span data-ttu-id="9fc43-122">通訊錄中的每個項目 (也就是為每個人) 有幾個部分的資訊，例如名字、 姓氏、 地址、 電子郵件地址和電話號碼。</span><span class="sxs-lookup"><span data-stu-id="9fc43-122">For each entry in the address book (that is, for each person) you have several pieces of information such as first name, last name, address, email address, and phone number.</span></span>

<span data-ttu-id="9fc43-123">就像這樣的圖片資料的典型方式是為包含資料列和資料行的資料表。</span><span class="sxs-lookup"><span data-stu-id="9fc43-123">A typical way to picture data like this is as a table with rows and columns.</span></span> <span data-ttu-id="9fc43-124">在資料庫詞彙中，每個資料列通常稱為一筆記錄。</span><span class="sxs-lookup"><span data-stu-id="9fc43-124">In database terms, each row is often referred to as a record.</span></span> <span data-ttu-id="9fc43-125">（有時稱為 「 欄位 」） 的每個資料行包含每種類型的資料值： 名字、 最後一個名稱和等等。</span><span class="sxs-lookup"><span data-stu-id="9fc43-125">Each column (sometimes referred to as fields) contains a value for each type of data: first name, last name, and so on.</span></span>

| <span data-ttu-id="9fc43-126">**ID**</span><span class="sxs-lookup"><span data-stu-id="9fc43-126">**ID**</span></span> | <span data-ttu-id="9fc43-127">**FirstName**</span><span class="sxs-lookup"><span data-stu-id="9fc43-127">**FirstName**</span></span> | <span data-ttu-id="9fc43-128">**lastName**</span><span class="sxs-lookup"><span data-stu-id="9fc43-128">**LastName**</span></span> | <span data-ttu-id="9fc43-129">**地址**</span><span class="sxs-lookup"><span data-stu-id="9fc43-129">**Address**</span></span> | <span data-ttu-id="9fc43-130">**電子郵件**</span><span class="sxs-lookup"><span data-stu-id="9fc43-130">**Email**</span></span> | <span data-ttu-id="9fc43-131">**電話**</span><span class="sxs-lookup"><span data-stu-id="9fc43-131">**Phone**</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="9fc43-132">1</span><span class="sxs-lookup"><span data-stu-id="9fc43-132">1</span></span> | <span data-ttu-id="9fc43-133">Jim</span><span class="sxs-lookup"><span data-stu-id="9fc43-133">Jim</span></span> | <span data-ttu-id="9fc43-134">Abrus</span><span class="sxs-lookup"><span data-stu-id="9fc43-134">Abrus</span></span> | <span data-ttu-id="9fc43-135">210 100 St SE Orcas WA 98031</span><span class="sxs-lookup"><span data-stu-id="9fc43-135">210 100th St SE Orcas WA 98031</span></span> | jim@contoso.com | <span data-ttu-id="9fc43-136">555 0100</span><span class="sxs-lookup"><span data-stu-id="9fc43-136">555 0100</span></span> |
| <span data-ttu-id="9fc43-137">2</span><span class="sxs-lookup"><span data-stu-id="9fc43-137">2</span></span> | <span data-ttu-id="9fc43-138">Terry</span><span class="sxs-lookup"><span data-stu-id="9fc43-138">Terry</span></span> | <span data-ttu-id="9fc43-139">Adams</span><span class="sxs-lookup"><span data-stu-id="9fc43-139">Adams</span></span> | <span data-ttu-id="9fc43-140">1234 Main St.Seattle WA 99011</span><span class="sxs-lookup"><span data-stu-id="9fc43-140">1234 Main St. Seattle WA 99011</span></span> | terry@cohowinery.com | <span data-ttu-id="9fc43-141">555 0101</span><span class="sxs-lookup"><span data-stu-id="9fc43-141">555 0101</span></span> |

<span data-ttu-id="9fc43-142">針對大部分的資料庫資料表，則資料表必須有包含唯一的識別碼，例如客戶編號、 客戶編號等的資料行。這就所謂的資料表*主索引鍵*，並使用它來識別資料表中的每個資料列。</span><span class="sxs-lookup"><span data-stu-id="9fc43-142">For most database tables, the table has to have a column that contains a unique identifier, like a customer number, account number, etc. This is known as the table's *primary key*, and you use it to identify each row in the table.</span></span> <span data-ttu-id="9fc43-143">在範例中，識別碼資料行是通訊錄的主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9fc43-143">In the example, the ID column is the primary key for the address book.</span></span>

<span data-ttu-id="9fc43-144">這個基本的了解的資料庫，就可以了解如何建立簡單的資料庫，並執行作業，例如加入、 修改和刪除資料。</span><span class="sxs-lookup"><span data-stu-id="9fc43-144">With this basic understanding of databases, you're ready to learn how to create a simple database and perform operations such as adding, modifying, and deleting data.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="9fc43-145">**關聯式資料庫**</span><span class="sxs-lookup"><span data-stu-id="9fc43-145">**Relational Databases**</span></span>
> 
> <span data-ttu-id="9fc43-146">您可以將資料儲存在許多種，包括文字檔案和試算表。</span><span class="sxs-lookup"><span data-stu-id="9fc43-146">You can store data in lots of ways, including text files and spreadsheets.</span></span> <span data-ttu-id="9fc43-147">不過，為大部分的商業用途，資料會儲存在關聯式資料庫中。</span><span class="sxs-lookup"><span data-stu-id="9fc43-147">For most business uses, though, data is stored in a relational database.</span></span>
> 
> <span data-ttu-id="9fc43-148">這篇文章不很多心力進入資料庫。</span><span class="sxs-lookup"><span data-stu-id="9fc43-148">This article doesn't go very deeply into databases.</span></span> <span data-ttu-id="9fc43-149">不過，您可能會發現有點瞭解它們很有用。</span><span class="sxs-lookup"><span data-stu-id="9fc43-149">However, you might find it useful to understand a little about them.</span></span> <span data-ttu-id="9fc43-150">在關聯式資料庫中，資訊會邏輯區分為不同的資料表。</span><span class="sxs-lookup"><span data-stu-id="9fc43-150">In a relational database, information is logically divided into separate tables.</span></span> <span data-ttu-id="9fc43-151">比方說，學校裡的資料庫可能包含不同的資料表，適用於學生和類別供應項目。</span><span class="sxs-lookup"><span data-stu-id="9fc43-151">For example, a database for a school might contain separate tables for students and for class offerings.</span></span> <span data-ttu-id="9fc43-152">資料庫軟體 （例如 SQL Server) 支援強大命令，可讓您以動態方式建立資料表之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="9fc43-152">The database software (such as SQL Server) supports powerful commands that let you dynamically establish relationships between the tables.</span></span> <span data-ttu-id="9fc43-153">例如，您可以使用關聯式資料庫，以建立學生與類別之間的邏輯關聯性以建立排程。</span><span class="sxs-lookup"><span data-stu-id="9fc43-153">For example, you can use the relational database to establish a logical relationship between students and classes in order to create a schedule.</span></span> <span data-ttu-id="9fc43-154">將資料儲存在個別的資料表的資料表結構的複雜度，並可減少多餘的資料保留在資料表中的需求。</span><span class="sxs-lookup"><span data-stu-id="9fc43-154">Storing data in separate tables reduces the complexity of the table structure and reduces the need to keep redundant data in tables.</span></span>


## <a name="creating-a-database"></a><span data-ttu-id="9fc43-155">建立資料庫</span><span class="sxs-lookup"><span data-stu-id="9fc43-155">Creating a Database</span></span>

<span data-ttu-id="9fc43-156">此程序會示範如何建立名為 SmallBakery，使用 SQL Server Compact 資料庫設計工具包含在 WebMatrix 中的資料庫。</span><span class="sxs-lookup"><span data-stu-id="9fc43-156">This procedure shows you how to create a database named SmallBakery by using the SQL Server Compact Database design tool that's included in WebMatrix.</span></span> <span data-ttu-id="9fc43-157">雖然您可以建立資料庫，使用程式碼，更通常會建立資料庫和資料庫資料表，使用像是 WebMatrix 的設計工具。</span><span class="sxs-lookup"><span data-stu-id="9fc43-157">Although you can create a database using code, it's more typical to create the database and database tables using a design tool like WebMatrix.</span></span>

1. <span data-ttu-id="9fc43-158">啟動 WebMatrix，並在 快速入門 頁面中，按一下 **站台從範本**。</span><span class="sxs-lookup"><span data-stu-id="9fc43-158">Start WebMatrix, and on the Quick Start page, click **Site From Template**.</span></span>
2. <span data-ttu-id="9fc43-159">選取 [**空白網站**，然後在**站台名稱**] 方塊中輸入 「 SmallBakery"，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="9fc43-159">Select **Empty Site**, and in the **Site Name** box enter "SmallBakery" and then click **OK**.</span></span> <span data-ttu-id="9fc43-160">建立並顯示在 WebMatrix 網站。</span><span class="sxs-lookup"><span data-stu-id="9fc43-160">The site is created and displayed in WebMatrix.</span></span>
3. <span data-ttu-id="9fc43-161">在左窗格中，按一下**資料庫**工作區。</span><span class="sxs-lookup"><span data-stu-id="9fc43-161">In the left pane, click the **Databases** workspace.</span></span>
4. <span data-ttu-id="9fc43-162">在 功能區中，按一下**新的資料庫**。</span><span class="sxs-lookup"><span data-stu-id="9fc43-162">In the ribbon, click **New Database**.</span></span> <span data-ttu-id="9fc43-163">空的資料庫會建立具有相同的名稱，作為您的網站。</span><span class="sxs-lookup"><span data-stu-id="9fc43-163">An empty database is created with the same name as your site.</span></span>
5. <span data-ttu-id="9fc43-164">在左窗格中，依序展開**SmallBakery.sdf**節點，然後按一下**資料表**。</span><span class="sxs-lookup"><span data-stu-id="9fc43-164">In the left pane, expand the **SmallBakery.sdf** node and then click **Tables**.</span></span>
6. <span data-ttu-id="9fc43-165">在 功能區中，按一下**新的資料表**。</span><span class="sxs-lookup"><span data-stu-id="9fc43-165">In the ribbon, click **New Table**.</span></span> <span data-ttu-id="9fc43-166">WebMatrix 會開啟資料表設計工具。</span><span class="sxs-lookup"><span data-stu-id="9fc43-166">WebMatrix opens the table designer.</span></span>

    ![[影像]](5-working-with-data/_static/image1.jpg)
7. <span data-ttu-id="9fc43-168">在中按一下**名稱**資料行，然後輸入&quot;識別碼&quot;。</span><span class="sxs-lookup"><span data-stu-id="9fc43-168">Click in the **Name** column and enter &quot;Id&quot;.</span></span>
8. <span data-ttu-id="9fc43-169">在 **資料型別**欄中，選取**int**。</span><span class="sxs-lookup"><span data-stu-id="9fc43-169">In the **Data Type** column, select **int**.</span></span>
9. <span data-ttu-id="9fc43-170">設定**是主索引鍵嗎？** 並**是識別嗎？** 選項，以**是**。</span><span class="sxs-lookup"><span data-stu-id="9fc43-170">Set the **Is Primary Key?** and **Is Identify?** options to **Yes**.</span></span>

    <span data-ttu-id="9fc43-171">恰如其名**是主索引鍵**會告知資料庫，這會是資料表的主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9fc43-171">As the name suggests, **Is Primary Key** tells the database that this will be the table's primary key.</span></span> <span data-ttu-id="9fc43-172">**身分識別**告知的資料庫自動建立每個新的記錄識別碼，並將它指派下一個循序編號 （從 1 開始）。</span><span class="sxs-lookup"><span data-stu-id="9fc43-172">**Is Identity** tells the database to automatically create an ID number for every new record and to assign it the next sequential number (starting at 1).</span></span>
10. <span data-ttu-id="9fc43-173">按一下 [下一步] 的資料列中。</span><span class="sxs-lookup"><span data-stu-id="9fc43-173">Click in the next row.</span></span> <span data-ttu-id="9fc43-174">編輯器會啟動新的資料行定義。</span><span class="sxs-lookup"><span data-stu-id="9fc43-174">The editor starts a new column definition.</span></span>
11. <span data-ttu-id="9fc43-175">針對名稱值中，輸入&quot;名稱&quot;。</span><span class="sxs-lookup"><span data-stu-id="9fc43-175">For the Name value, enter &quot;Name&quot;.</span></span>
12. <span data-ttu-id="9fc43-176">針對**資料型別**，選擇&quot;nvarchar&quot;並將長度設定為 50。</span><span class="sxs-lookup"><span data-stu-id="9fc43-176">For **Data Type**, choose &quot;nvarchar&quot; and set the length to 50.</span></span> <span data-ttu-id="9fc43-177">*Var*屬於`nvarchar`會告知資料庫這個資料行的資料會是大小可能會不同記錄間的字串。</span><span class="sxs-lookup"><span data-stu-id="9fc43-177">The *var* part of `nvarchar` tells the database that the data for this column will be a string whose size might vary from record to record.</span></span> <span data-ttu-id="9fc43-178">( *n*前置詞代表*國家*，表示欄位可以保留代表任何字母的字元資料，或寫入系統&#8212;也就是，欄位就會保留 Unicode 資料。)</span><span class="sxs-lookup"><span data-stu-id="9fc43-178">(The *n* prefix represents *national*, indicating that the field can hold character data that represents any alphabet or writing system &#8212; that is, that the field holds Unicode data.)</span></span>
13. <span data-ttu-id="9fc43-179">設定**是否允許 null 值**選項設定為**No**。</span><span class="sxs-lookup"><span data-stu-id="9fc43-179">Set the **Allow Nulls** option to **No**.</span></span> <span data-ttu-id="9fc43-180">這將會強制執行所*名稱*資料行不會保留空白。</span><span class="sxs-lookup"><span data-stu-id="9fc43-180">This will enforce that the *Name* column is not left blank.</span></span>
14. <span data-ttu-id="9fc43-181">使用這個相同的程序，建立名為資料行*描述*。</span><span class="sxs-lookup"><span data-stu-id="9fc43-181">Using this same process, create a column named *Description*.</span></span> <span data-ttu-id="9fc43-182">設定**資料型別**"nvarchar"和長度，並將設定 50**允許 Null**設為 false。</span><span class="sxs-lookup"><span data-stu-id="9fc43-182">Set **Data Type** to "nvarchar" and 50 for the length, and set **Allow Nulls** to false.</span></span>
15. <span data-ttu-id="9fc43-183">建立名為資料行*價格*。</span><span class="sxs-lookup"><span data-stu-id="9fc43-183">Create a column named *Price*.</span></span> <span data-ttu-id="9fc43-184">設定 **"money"資料類型**並設定**允許 Null**設為 false。</span><span class="sxs-lookup"><span data-stu-id="9fc43-184">Set **Data Type to "money"** and set **Allow Nulls** to false.</span></span>
16. <span data-ttu-id="9fc43-185">在頂端方塊中，資料表的命名&quot;產品&quot;。</span><span class="sxs-lookup"><span data-stu-id="9fc43-185">In the box at the top, name the table &quot;Product&quot;.</span></span>

    <span data-ttu-id="9fc43-186">當您完成時，定義會如下所示：</span><span class="sxs-lookup"><span data-stu-id="9fc43-186">When you're done, the definition will look like this:</span></span>

    ![[影像]](5-working-with-data/_static/image2.jpg)
17. <span data-ttu-id="9fc43-188">按下 Ctrl + S 可儲存資料表。</span><span class="sxs-lookup"><span data-stu-id="9fc43-188">Press Ctrl+S to save the table.</span></span>

## <a name="adding-data-to-the-database"></a><span data-ttu-id="9fc43-189">將資料加入至資料庫</span><span class="sxs-lookup"><span data-stu-id="9fc43-189">Adding Data to the Database</span></span>

<span data-ttu-id="9fc43-190">現在您可以將一些範例資料加入您在本文稍後將使用的資料庫。</span><span class="sxs-lookup"><span data-stu-id="9fc43-190">Now you can add some sample data to your database that you'll work with later in the article.</span></span>

1. <span data-ttu-id="9fc43-191">在左窗格中，依序展開**SmallBakery.sdf**節點，然後按一下**資料表**。</span><span class="sxs-lookup"><span data-stu-id="9fc43-191">In the left pane, expand the **SmallBakery.sdf** node and then click **Tables**.</span></span>
2. <span data-ttu-id="9fc43-192">以滑鼠右鍵按一下 產品 資料表，然後按一下 **資料**。</span><span class="sxs-lookup"><span data-stu-id="9fc43-192">Right-click the Product table and then click **Data**.</span></span>
3. <span data-ttu-id="9fc43-193">在 [編輯] 窗格中，輸入下列記錄：</span><span class="sxs-lookup"><span data-stu-id="9fc43-193">In the edit pane, enter the following records:</span></span>

    | <span data-ttu-id="9fc43-194">**名稱**</span><span class="sxs-lookup"><span data-stu-id="9fc43-194">**Name**</span></span> | <span data-ttu-id="9fc43-195">**描述**</span><span class="sxs-lookup"><span data-stu-id="9fc43-195">**Description**</span></span> | <span data-ttu-id="9fc43-196">**價格**</span><span class="sxs-lookup"><span data-stu-id="9fc43-196">**Price**</span></span> |
    | --- | --- | --- |
    | <span data-ttu-id="9fc43-197">階層</span><span class="sxs-lookup"><span data-stu-id="9fc43-197">Bread</span></span> | <span data-ttu-id="9fc43-198">全新內建每一天。</span><span class="sxs-lookup"><span data-stu-id="9fc43-198">Baked fresh every day.</span></span> | <span data-ttu-id="9fc43-199">2.99</span><span class="sxs-lookup"><span data-stu-id="9fc43-199">2.99</span></span> |
    | <span data-ttu-id="9fc43-200">Strawberry Shortcake</span><span class="sxs-lookup"><span data-stu-id="9fc43-200">Strawberry Shortcake</span></span> | <span data-ttu-id="9fc43-201">對建立有機 strawberries 從我們的處理序區。</span><span class="sxs-lookup"><span data-stu-id="9fc43-201">Made with organic strawberries from our garden.</span></span> | <span data-ttu-id="9fc43-202">9.99</span><span class="sxs-lookup"><span data-stu-id="9fc43-202">9.99</span></span> |
    | <span data-ttu-id="9fc43-203">Apple 的圓形圖</span><span class="sxs-lookup"><span data-stu-id="9fc43-203">Apple Pie</span></span> | <span data-ttu-id="9fc43-204">只以您的母親派形的第二個。</span><span class="sxs-lookup"><span data-stu-id="9fc43-204">Second only to your mom's pie.</span></span> | <span data-ttu-id="9fc43-205">12.99</span><span class="sxs-lookup"><span data-stu-id="9fc43-205">12.99</span></span> |
    | <span data-ttu-id="9fc43-206">Pecan 圓形圖</span><span class="sxs-lookup"><span data-stu-id="9fc43-206">Pecan Pie</span></span> | <span data-ttu-id="9fc43-207">如果您要 pecans，這很適合您。</span><span class="sxs-lookup"><span data-stu-id="9fc43-207">If you like pecans, this is for you.</span></span> | <span data-ttu-id="9fc43-208">10.99</span><span class="sxs-lookup"><span data-stu-id="9fc43-208">10.99</span></span> |
    | <span data-ttu-id="9fc43-209">檸檬的圓形圖</span><span class="sxs-lookup"><span data-stu-id="9fc43-209">Lemon Pie</span></span> | <span data-ttu-id="9fc43-210">使用全球最佳 lemons 提出。</span><span class="sxs-lookup"><span data-stu-id="9fc43-210">Made with the best lemons in the world.</span></span> | <span data-ttu-id="9fc43-211">11.99</span><span class="sxs-lookup"><span data-stu-id="9fc43-211">11.99</span></span> |
    | <span data-ttu-id="9fc43-212">Cupcakes</span><span class="sxs-lookup"><span data-stu-id="9fc43-212">Cupcakes</span></span> | <span data-ttu-id="9fc43-213">您的孩子和您的孩子一定會喜歡這些。</span><span class="sxs-lookup"><span data-stu-id="9fc43-213">Your kids and the kid in you will love these.</span></span> | <span data-ttu-id="9fc43-214">7.99</span><span class="sxs-lookup"><span data-stu-id="9fc43-214">7.99</span></span> |

    <span data-ttu-id="9fc43-215">請記住，您不必進行任何的輸入*識別碼*資料行。</span><span class="sxs-lookup"><span data-stu-id="9fc43-215">Remember that you don't have to enter anything for the *Id* column.</span></span> <span data-ttu-id="9fc43-216">您在建立時*識別碼*資料行，設定其**為識別**屬性設為 true，這會導致它自動填入。</span><span class="sxs-lookup"><span data-stu-id="9fc43-216">When you created the *Id* column, you set its **Is Identity** property to true, which causes it to automatically be filled in.</span></span>

    <span data-ttu-id="9fc43-217">當您完成輸入資料時，資料表設計工具看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="9fc43-217">When you're finished entering the data, the table designer will look like this:</span></span>

    ![[影像]](5-working-with-data/_static/image3.jpg)
4. <span data-ttu-id="9fc43-219">關閉 包含的資料庫資料的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="9fc43-219">Close the tab that contains the database data.</span></span>

## <a name="displaying-data-from-a-database"></a><span data-ttu-id="9fc43-220">顯示資料庫中的資料</span><span class="sxs-lookup"><span data-stu-id="9fc43-220">Displaying Data from a Database</span></span>

<span data-ttu-id="9fc43-221">一旦您在其中有資料的資料庫，您就可以在 ASP.NET 網頁中顯示資料。</span><span class="sxs-lookup"><span data-stu-id="9fc43-221">Once you've got a database with data in it, you can display the data in an ASP.NET web page.</span></span> <span data-ttu-id="9fc43-222">若要選取要顯示的資料表資料列，您可以使用 SQL 陳述式，也就是您傳遞至資料庫的命令。</span><span class="sxs-lookup"><span data-stu-id="9fc43-222">To select the table rows to display, you use a SQL statement, which is a command that you pass to the database.</span></span>

1. <span data-ttu-id="9fc43-223">在左窗格中，按一下**檔案**工作區。</span><span class="sxs-lookup"><span data-stu-id="9fc43-223">In the left pane, click the **Files** workspace.</span></span>
2. <span data-ttu-id="9fc43-224">在網站根目錄中，建立名為新的 CSHTML 頁面*ListProducts.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9fc43-224">In the root of the website, create a new CSHTML page named *ListProducts.cshtml*.</span></span>
3. <span data-ttu-id="9fc43-225">以下列內容取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="9fc43-225">Replace the existing markup with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    <span data-ttu-id="9fc43-226">在第一個程式碼區塊中，開啟*SmallBakery.sdf*您稍早建立的檔案 （資料庫）。</span><span class="sxs-lookup"><span data-stu-id="9fc43-226">In the first code block, you open the *SmallBakery.sdf* file (database) that you created earlier.</span></span> <span data-ttu-id="9fc43-227">`Database.Open`方法會假設 *.sdf*檔案位於您的網站*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fc43-227">The `Database.Open` method assumes that the *.sdf* file is in your website's *App\_Data* folder.</span></span> <span data-ttu-id="9fc43-228">(請注意，您不需要指定 *.sdf*延伸模組&#8212;事實上，如果您這樣做，`Open`方法將無法運作。)</span><span class="sxs-lookup"><span data-stu-id="9fc43-228">(Notice that you don't need to specify the *.sdf* extension &#8212; in fact, if you do, the `Open` method won't work.)</span></span>

    > [!NOTE]
    > <span data-ttu-id="9fc43-229">*應用程式\_資料*資料夾是用來儲存資料檔案的 ASP.NET 中的特殊資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fc43-229">The *App\_Data* folder is a special folder in ASP.NET that's used to store data files.</span></span> <span data-ttu-id="9fc43-230">如需詳細資訊，請參閱 <<c0> [ 連接到資料庫](#SB_ConnectingToADatabase)本文稍後。</span><span class="sxs-lookup"><span data-stu-id="9fc43-230">For more information, see [Connecting to a Database](#SB_ConnectingToADatabase) later in this article.</span></span>

    <span data-ttu-id="9fc43-231">然後您要查詢資料庫以使用下列 SQL 要求`Select`陳述式：</span><span class="sxs-lookup"><span data-stu-id="9fc43-231">You then make a request to query the database using the following SQL `Select` statement:</span></span>

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    <span data-ttu-id="9fc43-232">在陳述式，`Product`識別要查詢的資料表。</span><span class="sxs-lookup"><span data-stu-id="9fc43-232">In the statement, `Product` identifies the table to query.</span></span> <span data-ttu-id="9fc43-233">`*`字元可讓您指定查詢應該從資料表傳回所有資料行。</span><span class="sxs-lookup"><span data-stu-id="9fc43-233">The `*` character specifies that the query should return all the columns from the table.</span></span> <span data-ttu-id="9fc43-234">（您可能也列出資料行個別以逗號分隔，如果您想要查看只是某些資料行。）`Order By`子句會指出應該如何排序資料&#8212;在此情況下，由*名稱*資料行。</span><span class="sxs-lookup"><span data-stu-id="9fc43-234">(You could also list columns individually, separated by commas, if you wanted to see only some of the columns.) The `Order By` clause indicates how the data should be sorted &#8212; in this case, by the *Name* column.</span></span> <span data-ttu-id="9fc43-235">這表示，資料是依字母順序根據排序的值*名稱*每個資料列的資料行。</span><span class="sxs-lookup"><span data-stu-id="9fc43-235">This means that the data is sorted alphabetically based on the value of the *Name* column for each row.</span></span>

    <span data-ttu-id="9fc43-236">在頁面的主體中，標記會建立 HTML 資料表是用來顯示資料。</span><span class="sxs-lookup"><span data-stu-id="9fc43-236">In the body of the page, the markup creates an HTML table that will be used to display the data.</span></span> <span data-ttu-id="9fc43-237">內部`<tbody>`項目，您使用`foreach`迴圈以個別取得每個查詢所傳回的資料列。</span><span class="sxs-lookup"><span data-stu-id="9fc43-237">Inside the `<tbody>` element, you use a `foreach` loop to individually get each data row that's returned by the query.</span></span> <span data-ttu-id="9fc43-238">針對每個資料列中，您會建立 HTML 資料表資料列 (`<tr>`項目)。</span><span class="sxs-lookup"><span data-stu-id="9fc43-238">For each data row, you create an HTML table row (`<tr>` element).</span></span> <span data-ttu-id="9fc43-239">然後建立 HTML 資料表儲存格 (`<td>`項目) 的每個資料行。</span><span class="sxs-lookup"><span data-stu-id="9fc43-239">Then you create HTML table cells (`<td>` elements) for each column.</span></span> <span data-ttu-id="9fc43-240">每次您會執行迴圈時下, 一個可用的資料列，從資料庫處於`row`變數 (您設定此功能`foreach`陳述式)。</span><span class="sxs-lookup"><span data-stu-id="9fc43-240">Each time you go through the loop, the next available row from the database is in the `row` variable (you set this up in the `foreach` statement).</span></span> <span data-ttu-id="9fc43-241">若要取得個別資料行從資料列，您可以使用`row.Name`或`row.Description`或任何的名稱是資料行的您想要。</span><span class="sxs-lookup"><span data-stu-id="9fc43-241">To get an individual column from the row, you can use `row.Name` or `row.Description` or whatever the name is of the column you want.</span></span>
4. <span data-ttu-id="9fc43-242">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="9fc43-242">Run the page in a browser.</span></span> <span data-ttu-id="9fc43-243">(請確定中選取頁面**檔案**才能執行這個工作區。)頁面會顯示清單中的，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9fc43-243">(Make sure the page is selected in the **Files** workspace before you run it.) The page displays a list like the following:</span></span>

    ![[影像]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> <span data-ttu-id="9fc43-245">**結構化的查詢語言 (SQL)**</span><span class="sxs-lookup"><span data-stu-id="9fc43-245">**Structured Query Language (SQL)**</span></span>
> 
> <span data-ttu-id="9fc43-246">SQL 是一種語言，在大部分的關聯式資料庫中用於管理資料庫中的資料。</span><span class="sxs-lookup"><span data-stu-id="9fc43-246">SQL is a language that's used in most relational databases for managing data in a database.</span></span> <span data-ttu-id="9fc43-247">它包含的指令，可讓您擷取資料，並加以更新，以及，可讓您建立、 修改和管理資料庫的資料表。</span><span class="sxs-lookup"><span data-stu-id="9fc43-247">It includes commands that let you retrieve data and update it, and that let you create, modify, and manage database tables.</span></span> <span data-ttu-id="9fc43-248">SQL 是不同的程式語言 （例如您在 WebMatrix 中使用的那一個） 因為 sql 的概念是，您需要告訴資料庫想什麼方法，它是資料庫的作業，以了解如何取得資料，或執行工作。</span><span class="sxs-lookup"><span data-stu-id="9fc43-248">SQL is different than a programming language (like the one you're using in WebMatrix) because with SQL, the idea is that you tell the database what you want, and it's the database's job to figure out how to get the data or perform the task.</span></span> <span data-ttu-id="9fc43-249">以下是一些 SQL 命令的範例，以及他們如何：</span><span class="sxs-lookup"><span data-stu-id="9fc43-249">Here are examples of some SQL commands and what they do:</span></span>
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> <span data-ttu-id="9fc43-250">這會擷取*識別碼*，*名稱*，並*價格*資料行中的記錄*產品*資料表的值*價格*超過 10，並將結果傳回依字母順序的值為基礎*名稱*資料行。</span><span class="sxs-lookup"><span data-stu-id="9fc43-250">This fetches the *Id*, *Name*, and *Price* columns from records in the *Product* table if the value of *Price* is more than 10, and returns the results in alphabetical order based on the values of the *Name* column.</span></span> <span data-ttu-id="9fc43-251">此命令會傳回結果集，其中包含符合準則或空的集合，如果沒有記錄符合的記錄。</span><span class="sxs-lookup"><span data-stu-id="9fc43-251">This command will return a result set that contains the records that meet the criteria, or an empty set if no records match.</span></span>
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> <span data-ttu-id="9fc43-252">這會插入到新的資料錄*產品*資料表中，設定*名稱*資料行&quot;可頌麵包&quot;，則*描述*資料行&quot;不穩定的福音&quot;，和 1.99 價格。</span><span class="sxs-lookup"><span data-stu-id="9fc43-252">This inserts a new record into the *Product* table, setting the *Name* column to &quot;Croissant&quot;, the *Description* column to &quot;A flaky delight&quot;, and the price to 1.99.</span></span>
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> <span data-ttu-id="9fc43-253">此命令會刪除記錄檔中的*產品*其到期的日期資料行是早於 2008 年 1 月 1 日的資料表。</span><span class="sxs-lookup"><span data-stu-id="9fc43-253">This command deletes records in the *Product* table whose expiration date column is earlier than January 1, 2008.</span></span> <span data-ttu-id="9fc43-254">(這是假設*產品*資料表中有這類資料行，當然。)在此處以 MM/DD/YYYY 格式輸入日期，但它應該使用於您的地區設定的格式輸入。</span><span class="sxs-lookup"><span data-stu-id="9fc43-254">(This assumes that the *Product* table has such a column, of course.) The date is entered here in MM/DD/YYYY format, but it should be entered in the format that's used for your locale.</span></span>
> 
> <span data-ttu-id="9fc43-255">`Insert Into`和`Delete`命令未傳回結果集。</span><span class="sxs-lookup"><span data-stu-id="9fc43-255">The `Insert Into` and `Delete` commands don't return result sets.</span></span> <span data-ttu-id="9fc43-256">相反地，它們會傳回一個數字，會告訴您命令影響的多少筆記錄。</span><span class="sxs-lookup"><span data-stu-id="9fc43-256">Instead, they return a number that tells you how many records were affected by the command.</span></span>
> 
> <span data-ttu-id="9fc43-257">其中一些作業 （例如插入和刪除記錄），要求作業的程序必須在資料庫中有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="9fc43-257">For some of these operations (like inserting and deleting records), the process that's requesting the operation has to have appropriate permissions in the database.</span></span> <span data-ttu-id="9fc43-258">這是您通常需要提供使用者名稱和密碼，當您連接到資料庫的生產資料庫的原因。</span><span class="sxs-lookup"><span data-stu-id="9fc43-258">This is why for production databases you often have to supply a username and password when you connect to the database.</span></span>
> 
> <span data-ttu-id="9fc43-259">有數十個 SQL 命令，不過都會遵循類似的模式。</span><span class="sxs-lookup"><span data-stu-id="9fc43-259">There are dozens of SQL commands, but they all follow a pattern like this.</span></span> <span data-ttu-id="9fc43-260">您可以使用 SQL 命令來建立資料庫資料表、 計算資料表中的記錄數目、 計算價格，以及執行許多更多的作業。</span><span class="sxs-lookup"><span data-stu-id="9fc43-260">You can use SQL commands to create database tables, count the number of records in a table, calculate prices, and perform many more operations.</span></span>


## <a name="inserting-data-in-a-database"></a><span data-ttu-id="9fc43-261">在資料庫中插入資料</span><span class="sxs-lookup"><span data-stu-id="9fc43-261">Inserting Data in a Database</span></span>

<span data-ttu-id="9fc43-262">本節說明如何建立可讓使用者新增至新的產品網頁*產品*資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="9fc43-262">This section shows how to create a page that lets users add a new product to the *Product* database table.</span></span> <span data-ttu-id="9fc43-263">插入新的產品記錄後，此頁面會顯示更新過的資料表使用*ListProducts.cshtml*您在上一節中建立的頁面。</span><span class="sxs-lookup"><span data-stu-id="9fc43-263">After a new product record is inserted, the page displays the updated table using the *ListProducts.cshtml* page that you created in the previous section.</span></span>

<span data-ttu-id="9fc43-264">此頁面包含驗證，請確定使用者輸入的資料是有效的資料庫。</span><span class="sxs-lookup"><span data-stu-id="9fc43-264">The page includes validation to make sure that the data that the user enters is valid for the database.</span></span> <span data-ttu-id="9fc43-265">比方說，在頁面的程式碼可確保已針對所有必要的資料行輸入值。</span><span class="sxs-lookup"><span data-stu-id="9fc43-265">For example, code in the page makes sure that a value has been entered for all required columns.</span></span>

1. <span data-ttu-id="9fc43-266">在網站中，建立新的 CSHTML 檔案，名為*InsertProducts.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9fc43-266">In the website, create a new CSHTML file named *InsertProducts.cshtml*.</span></span>
2. <span data-ttu-id="9fc43-267">以下列內容取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="9fc43-267">Replace the existing markup with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    <span data-ttu-id="9fc43-268">頁面的主體會包含一個 HTML 表單，以讓使用者輸入名稱、 描述和價格的三個文字方塊。</span><span class="sxs-lookup"><span data-stu-id="9fc43-268">The body of the page contains an HTML form with three text boxes that let users enter a name, description, and price.</span></span> <span data-ttu-id="9fc43-269">當使用者按一下**插入** 按鈕，在頁面頂端的程式碼會開啟的連接*SmallBakery.sdf*資料庫。</span><span class="sxs-lookup"><span data-stu-id="9fc43-269">When users click the **Insert** button, the code at the top of the page opens a connection to the *SmallBakery.sdf* database.</span></span> <span data-ttu-id="9fc43-270">接著您可取得的值，使用者已送出使用`Request`物件，並將這些值指派給本機變數。</span><span class="sxs-lookup"><span data-stu-id="9fc43-270">You then get the values that the user has submitted by using the `Request` object and assign those values to local variables.</span></span>

    <span data-ttu-id="9fc43-271">若要驗證使用者輸入的值，每個必要的資料行，您註冊每個`<input>`您想要驗證的項目：</span><span class="sxs-lookup"><span data-stu-id="9fc43-271">To validate that the user entered a value for each required column, you register each `<input>` element that you want to validate:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    <span data-ttu-id="9fc43-272">`Validation`協助程式會檢查每個已註冊的欄位中沒有值。</span><span class="sxs-lookup"><span data-stu-id="9fc43-272">The `Validation` helper checks that there is a value in each of the fields that you've registered.</span></span> <span data-ttu-id="9fc43-273">您可以測試是否所有欄位都通過驗證檢查`Validation.IsValid()`，您通常會執行之前處理您從使用者取得的資訊：</span><span class="sxs-lookup"><span data-stu-id="9fc43-273">You can test whether all the fields passed validation by checking `Validation.IsValid()`, which you typically do before you process the information you get from the user:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    <span data-ttu-id="9fc43-274">(`&&`運算子表示 AND — 這項測試會*如果這是表單送出，且所有欄位都已都通過驗證*。)</span><span class="sxs-lookup"><span data-stu-id="9fc43-274">(The `&&` operator means AND — this test is *If this is a form submission AND all the fields have passed validation*.)</span></span>

    <span data-ttu-id="9fc43-275">如果驗證所有的資料行 （none 為空），您可以繼續並建立將資料插入，，然後執行它，如下所示的 SQL 陳述式：</span><span class="sxs-lookup"><span data-stu-id="9fc43-275">If all the columns validated (none were empty), you go ahead and create a SQL statement to insert the data and then execute it as shown next:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    <span data-ttu-id="9fc43-276">要插入值，您可以包含參數替代符號 (`@0`， `@1`， `@2`)。</span><span class="sxs-lookup"><span data-stu-id="9fc43-276">For the values to insert, you include parameter placeholders (`@0`, `@1`, `@2`).</span></span>

    > [!NOTE]
    > <span data-ttu-id="9fc43-277">基於安全性考量，一律將值傳遞至 SQL 陳述式使用的參數，您在上述範例中看到。</span><span class="sxs-lookup"><span data-stu-id="9fc43-277">As a security precaution, always pass values to a SQL statement using parameters, as you see in the preceding example.</span></span> <span data-ttu-id="9fc43-278">這可讓您有機會驗證使用者的資料，再加上有助於防止惡意的命令傳送到您的資料庫 （有時稱為 SQL 插入式攻擊）。</span><span class="sxs-lookup"><span data-stu-id="9fc43-278">This gives you a chance to validate the user's data, plus it helps protect against attempts to send malicious commands to your database (sometimes referred to as SQL injection attacks).</span></span>

    <span data-ttu-id="9fc43-279">若要執行查詢時，您使用此陳述式，將包含要取代的預留位置值的變數傳遞給它：</span><span class="sxs-lookup"><span data-stu-id="9fc43-279">To execute the query, you use this statement, passing to it the variables that contain the values to substitute for the placeholders:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    <span data-ttu-id="9fc43-280">之後`Insert Into`陳述式已執行，您將使用者傳送至頁面，其中列出使用這行的產品：</span><span class="sxs-lookup"><span data-stu-id="9fc43-280">After the `Insert Into` statement has executed, you send the user to the page that lists the products using this line:</span></span>

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    <span data-ttu-id="9fc43-281">如果驗證不成功，則會略過 insert。</span><span class="sxs-lookup"><span data-stu-id="9fc43-281">If validation didn't succeed, you skip the insert.</span></span> <span data-ttu-id="9fc43-282">相反地，您有 helper 可以顯示的累積的錯誤訊息 （如果有的話） 頁面中：</span><span class="sxs-lookup"><span data-stu-id="9fc43-282">Instead, you have a helper in the page that can display the accumulated error messages (if any):</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    <span data-ttu-id="9fc43-283">請注意在標記中的樣式區塊包含名為 CSS 類別定義`.validation-summary-errors`。</span><span class="sxs-lookup"><span data-stu-id="9fc43-283">Notice that the style block in the markup includes a CSS class definition named `.validation-summary-errors`.</span></span> <span data-ttu-id="9fc43-284">這是預設值由 CSS 類別名稱`<div>`包含任何驗證錯誤的項目。</span><span class="sxs-lookup"><span data-stu-id="9fc43-284">This is the name of the CSS class that's used by default for the `<div>` element that contains any validation errors.</span></span> <span data-ttu-id="9fc43-285">驗證摘要錯誤以紅色顯示，並以粗體，但您可以定義 CSS 類別會在此情況下，指定`.validation-summary-errors`類別，以顯示您所喜歡的任何格式。</span><span class="sxs-lookup"><span data-stu-id="9fc43-285">In this case, the CSS class specifies that validation summary errors are displayed in red and in bold, but you can define the `.validation-summary-errors` class to display any formatting you like.</span></span>

### <a name="testing-the-insert-page"></a><span data-ttu-id="9fc43-286">測試插入頁面</span><span class="sxs-lookup"><span data-stu-id="9fc43-286">Testing the Insert Page</span></span>

1. <span data-ttu-id="9fc43-287">在瀏覽器中檢視網頁。</span><span class="sxs-lookup"><span data-stu-id="9fc43-287">View the page in a browser.</span></span> <span data-ttu-id="9fc43-288">此頁面會顯示類似下圖顯示一個表單。</span><span class="sxs-lookup"><span data-stu-id="9fc43-288">The page displays a form that's similar to the one that's shown in the following illustration.</span></span>

    ![[影像]](5-working-with-data/_static/image5.jpg)
2. <span data-ttu-id="9fc43-290">輸入值的所有資料行，但請確定您離開*價格*資料行保留空白。</span><span class="sxs-lookup"><span data-stu-id="9fc43-290">Enter values for all the columns, but make sure that you leave the *Price* column blank.</span></span>
3. <span data-ttu-id="9fc43-291">按一下 [插入] 。</span><span class="sxs-lookup"><span data-stu-id="9fc43-291">Click **Insert**.</span></span> <span data-ttu-id="9fc43-292">頁面會顯示錯誤訊息，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="9fc43-292">The page displays an error message, as shown in the following illustration.</span></span> <span data-ttu-id="9fc43-293">（建立新的記錄）。</span><span class="sxs-lookup"><span data-stu-id="9fc43-293">(No new record is created.)</span></span>

    ![[影像]](5-working-with-data/_static/image6.jpg)
4. <span data-ttu-id="9fc43-295">完整填寫表單，然後按一下**插入**。</span><span class="sxs-lookup"><span data-stu-id="9fc43-295">Fill the form out completely, and then click **Insert**.</span></span> <span data-ttu-id="9fc43-296">此時*ListProducts.cshtml*頁面隨即出現並顯示新的記錄。</span><span class="sxs-lookup"><span data-stu-id="9fc43-296">This time, the *ListProducts.cshtml* page is displayed and shows the new record.</span></span>

## <a name="updating-data-in-a-database"></a><span data-ttu-id="9fc43-297">更新資料庫中的資料</span><span class="sxs-lookup"><span data-stu-id="9fc43-297">Updating Data in a Database</span></span>

<span data-ttu-id="9fc43-298">沒有輸入資料到資料表之後，您可能需要更新它。</span><span class="sxs-lookup"><span data-stu-id="9fc43-298">After data has been entered into a table, you might need to update it.</span></span> <span data-ttu-id="9fc43-299">此程序會示範如何建立兩個頁面，類似於您為資料插入稍早建立的項目。</span><span class="sxs-lookup"><span data-stu-id="9fc43-299">This procedure shows you how to create two pages that are similar to the ones you created for data insertion earlier.</span></span> <span data-ttu-id="9fc43-300">第一頁會顯示產品，並可讓使用者選取其中一個來變更。</span><span class="sxs-lookup"><span data-stu-id="9fc43-300">The first page displays products and lets users select one to change.</span></span> <span data-ttu-id="9fc43-301">第二頁可讓使用者實際上進行編輯，並將其儲存。</span><span class="sxs-lookup"><span data-stu-id="9fc43-301">The second page lets the users actually make the edits and save them.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9fc43-302">**重要**在生產網站中，您通常會限制誰可以對資料進行變更。</span><span class="sxs-lookup"><span data-stu-id="9fc43-302">**Important** In a production website, you typically restrict who's allowed to make changes to the data.</span></span> <span data-ttu-id="9fc43-303">有關如何設定成員資格，以及有關如何授權使用者在站台上執行工作的相關資訊，請參閱[新增的安全性和 ASP.NET Web Pages 網站的成員資格](https://go.microsoft.com/fwlink/?LinkId=202904)。</span><span class="sxs-lookup"><span data-stu-id="9fc43-303">For information about how to set up membership and about ways to authorize users to perform tasks on the site, see [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).</span></span>


1. <span data-ttu-id="9fc43-304">在網站中，建立新的 CSHTML 檔案，名為*EditProducts.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9fc43-304">In the website, create a new CSHTML file named *EditProducts.cshtml*.</span></span>
2. <span data-ttu-id="9fc43-305">將檔案中的現有標記取代為下列：</span><span class="sxs-lookup"><span data-stu-id="9fc43-305">Replace the existing markup in the file with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    <span data-ttu-id="9fc43-306">此頁面中唯一的差別並*ListProducts.cshtml*頁面從稍早是在此頁面的 HTML 資料表，包含顯示的額外資料行**編輯**連結。</span><span class="sxs-lookup"><span data-stu-id="9fc43-306">The only difference between this page and the *ListProducts.cshtml* page from earlier is that the HTML table in this page includes an extra column that displays an **Edit** link.</span></span> <span data-ttu-id="9fc43-307">當您按一下此連結時，它會帶您前往*UpdateProducts.cshtml*頁面上 （您將在接下來建立） 您可以在其中編輯選取的記錄。</span><span class="sxs-lookup"><span data-stu-id="9fc43-307">When you click this link, it takes you to the *UpdateProducts.cshtml* page (which you'll create next) where you can edit the selected record.</span></span>

    <span data-ttu-id="9fc43-308">在建立的程式碼看起來**編輯**連結：</span><span class="sxs-lookup"><span data-stu-id="9fc43-308">Look at the code that creates the **Edit** link:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    <span data-ttu-id="9fc43-309">這會建立 HTML`<a>`項目其`href`屬性以動態方式設定。</span><span class="sxs-lookup"><span data-stu-id="9fc43-309">This creates an HTML `<a>` element whose `href` attribute is set dynamically.</span></span> <span data-ttu-id="9fc43-310">`href`屬性會指定當使用者按一下連結時，所顯示頁面。</span><span class="sxs-lookup"><span data-stu-id="9fc43-310">The `href` attribute specifies the page to display when the user clicks the link.</span></span> <span data-ttu-id="9fc43-311">它也會傳遞`Id`要連結的目前資料列的值。</span><span class="sxs-lookup"><span data-stu-id="9fc43-311">It also passes the `Id` value of the current row to the link.</span></span> <span data-ttu-id="9fc43-312">當執行網頁時，網頁原始檔可能已包含這類的連結：</span><span class="sxs-lookup"><span data-stu-id="9fc43-312">When the page runs, the page source might contain links like these:</span></span>

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    <span data-ttu-id="9fc43-313">請注意，`href`屬性設定為`UpdateProducts/n`，其中*n*是產品的數字。</span><span class="sxs-lookup"><span data-stu-id="9fc43-313">Notice that the `href` attribute is set to `UpdateProducts/n`, where *n* is a product number.</span></span> <span data-ttu-id="9fc43-314">當使用者按一下其中一個連結時，產生的 URL 看起來會像這樣：</span><span class="sxs-lookup"><span data-stu-id="9fc43-314">When a user clicks one of these links, the resulting URL will look something like this:</span></span>

    `http://localhost:18816/UpdateProducts/6`

    <span data-ttu-id="9fc43-315">換句話說，若要編輯的產品號碼會在 URL 中傳遞。</span><span class="sxs-lookup"><span data-stu-id="9fc43-315">In other words, the product number to be edited will be passed in the URL.</span></span>
3. <span data-ttu-id="9fc43-316">在瀏覽器中檢視網頁。</span><span class="sxs-lookup"><span data-stu-id="9fc43-316">View the page in a browser.</span></span> <span data-ttu-id="9fc43-317">頁面會顯示資料的格式如下：</span><span class="sxs-lookup"><span data-stu-id="9fc43-317">The page displays the data in a format like this:</span></span>

    ![[影像]](5-working-with-data/_static/image7.jpg)

    <span data-ttu-id="9fc43-319">接下來，您將建立可讓使用者實際更新的資料頁面。</span><span class="sxs-lookup"><span data-stu-id="9fc43-319">Next, you'll create the page that lets users actually update the data.</span></span> <span data-ttu-id="9fc43-320">[更新] 頁面包含驗證來驗證使用者輸入的資料。</span><span class="sxs-lookup"><span data-stu-id="9fc43-320">The update page includes validation to validate the data that the user enters.</span></span> <span data-ttu-id="9fc43-321">比方說，在頁面的程式碼可確保已針對所有必要的資料行輸入值。</span><span class="sxs-lookup"><span data-stu-id="9fc43-321">For example, code in the page makes sure that a value has been entered for all required columns.</span></span>
4. <span data-ttu-id="9fc43-322">在網站中，建立新的 CSHTML 檔案，名為*UpdateProducts.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9fc43-322">In the website, create a new CSHTML file named *UpdateProducts.cshtml*.</span></span>
5. <span data-ttu-id="9fc43-323">以下列內容取代現有的標記檔案中。</span><span class="sxs-lookup"><span data-stu-id="9fc43-323">Replace the existing markup in the file with the following.</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    <span data-ttu-id="9fc43-324">頁面的主體會包含一個 HTML 表單，其中會顯示產品和使用者可以編輯。</span><span class="sxs-lookup"><span data-stu-id="9fc43-324">The body of the page contains an HTML form where a product is displayed and where users can edit it.</span></span> <span data-ttu-id="9fc43-325">若要取得要顯示的產品，您可以使用這個 SQL 陳述式：</span><span class="sxs-lookup"><span data-stu-id="9fc43-325">To get the product to display, you use this SQL statement:</span></span>

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    <span data-ttu-id="9fc43-326">這樣就會選取的產品的 ID 符合傳入的值`@0`參數。</span><span class="sxs-lookup"><span data-stu-id="9fc43-326">This will select the product whose ID matches the value that's passed in the `@0` parameter.</span></span> <span data-ttu-id="9fc43-327">(因為*識別碼*是主索引鍵，因此必須是唯一的只有一個產品記錄曾選取這種方式。)若要取得識別碼的值傳遞至此`Select`陳述式中，您可以讀取值傳遞至頁面的 url，使用下列語法：</span><span class="sxs-lookup"><span data-stu-id="9fc43-327">(Because *Id* is the primary key and therefore must be unique, only one product record can ever be selected this way.) To get the ID value to pass to this `Select` statement, you can read the value that's passed to the page as part of the URL, using the following syntax:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    <span data-ttu-id="9fc43-328">若要實際擷取的產品記錄，您使用`QuerySingle`方法，它會傳回只是一筆記錄：</span><span class="sxs-lookup"><span data-stu-id="9fc43-328">To actually fetch the product record, you use the `QuerySingle` method, which will return just one record:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    <span data-ttu-id="9fc43-329">單一資料列會傳回成`row`變數。</span><span class="sxs-lookup"><span data-stu-id="9fc43-329">The single row is returned into the `row` variable.</span></span> <span data-ttu-id="9fc43-330">您可以取得每個資料行中的資料，並將它指派給本機變數，就像這樣：</span><span class="sxs-lookup"><span data-stu-id="9fc43-330">You can get data out of each column and assign it to local variables like this:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    <span data-ttu-id="9fc43-331">在表單的標記，這些值會自動顯示在個別的文字方塊中使用內嵌程式碼，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9fc43-331">In the markup for the form, these values are displayed automatically in individual text boxes by using embedded code like the following:</span></span>

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    <span data-ttu-id="9fc43-332">該部分程式碼會顯示更新的產品記錄。</span><span class="sxs-lookup"><span data-stu-id="9fc43-332">That part of the code displays the product record to be updated.</span></span> <span data-ttu-id="9fc43-333">一旦已顯示的記錄，使用者可以編輯個別的資料行。</span><span class="sxs-lookup"><span data-stu-id="9fc43-333">Once the record has been displayed, the user can edit individual columns.</span></span>

    <span data-ttu-id="9fc43-334">當使用者送出表單，即可**更新**按鈕中的程式碼`if(IsPost)`封鎖執行。</span><span class="sxs-lookup"><span data-stu-id="9fc43-334">When the user submits the form by clicking the **Update** button, the code in the `if(IsPost)` block runs.</span></span> <span data-ttu-id="9fc43-335">這會取得使用者的值從`Request`物件，將值儲存在變數中，並驗證已填寫每個資料行。</span><span class="sxs-lookup"><span data-stu-id="9fc43-335">This gets the user's values from the `Request` object, stores the values in variables, and validates that each column has been filled in.</span></span> <span data-ttu-id="9fc43-336">如果通過驗證，程式碼會建立下列的 SQL Update 陳述式：</span><span class="sxs-lookup"><span data-stu-id="9fc43-336">If validation passes, the code creates the following SQL Update statement:</span></span>

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    <span data-ttu-id="9fc43-337">在 SQL`Update`陳述式，指定每個更新，並將它設定為值的資料行。</span><span class="sxs-lookup"><span data-stu-id="9fc43-337">In a SQL `Update` statement, you specify each column to update and the value to set it to.</span></span> <span data-ttu-id="9fc43-338">在此程式碼中，值使用指定的參數預留位置`@0`， `@1`， `@2`，依此類推。</span><span class="sxs-lookup"><span data-stu-id="9fc43-338">In this code, the values are specified using the parameter placeholders `@0`, `@1`, `@2`, and so on.</span></span> <span data-ttu-id="9fc43-339">（如前文所述，為了安全性，您應該一律傳遞值的 SQL 陳述式所使用的參數。）</span><span class="sxs-lookup"><span data-stu-id="9fc43-339">(As noted earlier, for security, you should always pass values to a SQL statement by using parameters.)</span></span>

    <span data-ttu-id="9fc43-340">當您呼叫`db.Execute`方法，傳遞包含對應至 SQL 陳述式中參數的順序值的變數：</span><span class="sxs-lookup"><span data-stu-id="9fc43-340">When you call the `db.Execute` method, you pass the variables that contain the values in the order that corresponds to the parameters in the SQL statement:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    <span data-ttu-id="9fc43-341">之後`Update`在執行陳述式中，您才能將使用者重新導向回到編輯頁面呼叫下列方法：</span><span class="sxs-lookup"><span data-stu-id="9fc43-341">After the `Update` statement has been executed, you call the following method in order to redirect the user back to the edit page:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    <span data-ttu-id="9fc43-342">效果是，使用者會看到資料庫中資料的更新的清單，並可以編輯另一個產品。</span><span class="sxs-lookup"><span data-stu-id="9fc43-342">The effect is that the user sees an updated listing of the data in the database and can edit another product.</span></span>
6. <span data-ttu-id="9fc43-343">儲存網頁。</span><span class="sxs-lookup"><span data-stu-id="9fc43-343">Save the page.</span></span>
7. <span data-ttu-id="9fc43-344">執行*EditProducts.cshtml*網頁 （不是更新頁面），然後按一下**編輯**選取要編輯的產品。</span><span class="sxs-lookup"><span data-stu-id="9fc43-344">Run the *EditProducts.cshtml* page (not the update page) and then click **Edit** to select a product to edit.</span></span> <span data-ttu-id="9fc43-345">*UpdateProducts.cshtml*顯示頁面，顯示您所選取的記錄。</span><span class="sxs-lookup"><span data-stu-id="9fc43-345">The *UpdateProducts.cshtml* page is displayed, showing the record you selected.</span></span>

    ![[影像]](5-working-with-data/_static/image8.jpg)
8. <span data-ttu-id="9fc43-347">進行變更，然後按一下 **更新**。</span><span class="sxs-lookup"><span data-stu-id="9fc43-347">Make a change and click **Update**.</span></span> <span data-ttu-id="9fc43-348">產品清單會顯示一次，以更新的資料。</span><span class="sxs-lookup"><span data-stu-id="9fc43-348">The products list is shown again with your updated data.</span></span>

## <a name="deleting-data-in-a-database"></a><span data-ttu-id="9fc43-349">刪除資料庫中的資料</span><span class="sxs-lookup"><span data-stu-id="9fc43-349">Deleting Data in a Database</span></span>

<span data-ttu-id="9fc43-350">本節說明如何讓使用者可以刪除中的產品*產品*資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="9fc43-350">This section shows how to let users delete a product from the *Product* database table.</span></span> <span data-ttu-id="9fc43-351">此範例包含兩個頁面。</span><span class="sxs-lookup"><span data-stu-id="9fc43-351">The example consists of two pages.</span></span> <span data-ttu-id="9fc43-352">在第一個頁面中，使用者會選取要刪除的記錄。</span><span class="sxs-lookup"><span data-stu-id="9fc43-352">In the first page, users select a record to delete.</span></span> <span data-ttu-id="9fc43-353">要刪除的記錄即會顯示在第二個頁面，讓他們確認想要刪除的記錄中。</span><span class="sxs-lookup"><span data-stu-id="9fc43-353">The record to be deleted is then displayed in a second page that lets them confirm that they want to delete the record.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9fc43-354">**重要**在生產網站中，您通常會限制誰可以對資料進行變更。</span><span class="sxs-lookup"><span data-stu-id="9fc43-354">**Important** In a production website, you typically restrict who's allowed to make changes to the data.</span></span> <span data-ttu-id="9fc43-355">有關如何設定成員資格，以及有關如何在站台上執行工作的使用者授與的相關資訊，請參閱[新增的安全性和 ASP.NET Web Pages 網站的成員資格](https://go.microsoft.com/fwlink/?LinkId=202904)。</span><span class="sxs-lookup"><span data-stu-id="9fc43-355">For information about how to set up membership and about ways to authorize user to perform tasks on the site, see [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).</span></span>


1. <span data-ttu-id="9fc43-356">在網站中，建立新的 CSHTML 檔案，名為*ListProductsForDelete.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9fc43-356">In the website, create a new CSHTML file named *ListProductsForDelete.cshtml*.</span></span>
2. <span data-ttu-id="9fc43-357">以下列內容取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="9fc43-357">Replace the existing markup with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    <span data-ttu-id="9fc43-358">此頁面大致*EditProducts.cshtml*稍早的頁面。</span><span class="sxs-lookup"><span data-stu-id="9fc43-358">This page is similar to the *EditProducts.cshtml* page from earlier.</span></span> <span data-ttu-id="9fc43-359">不過，不會顯示**編輯**連結每項產品，它會顯示**刪除**連結。</span><span class="sxs-lookup"><span data-stu-id="9fc43-359">However, instead of displaying an **Edit** link for each product, it displays a **Delete** link.</span></span> <span data-ttu-id="9fc43-360">**刪除**在標記中使用下列的內嵌程式碼來建立連結：</span><span class="sxs-lookup"><span data-stu-id="9fc43-360">The **Delete** link is created using the following embedded code in the markup:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    <span data-ttu-id="9fc43-361">這會建立當使用者按一下連結，看起來像這樣的 URL:</span><span class="sxs-lookup"><span data-stu-id="9fc43-361">This creates a URL that looks like this when users click the link:</span></span>

    `http://<server>/DeleteProduct/4`

    <span data-ttu-id="9fc43-362">URL 會呼叫名為的頁面*DeleteProduct.cshtml* （這會建立下一步） 並將它傳遞至刪除的產品識別碼 （在此，4）。</span><span class="sxs-lookup"><span data-stu-id="9fc43-362">The URL calls a page named *DeleteProduct.cshtml* (which you'll create next) and passes it the ID of the product to delete (here, 4).</span></span>
3. <span data-ttu-id="9fc43-363">儲存檔案，但將它開啟。</span><span class="sxs-lookup"><span data-stu-id="9fc43-363">Save the file, but leave it open.</span></span>
4. <span data-ttu-id="9fc43-364">建立另一個名為的 CHTML 檔案*DeleteProduct.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9fc43-364">Create another CHTML file named *DeleteProduct.cshtml*.</span></span> <span data-ttu-id="9fc43-365">以下列內容取代現有的內容：</span><span class="sxs-lookup"><span data-stu-id="9fc43-365">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    <span data-ttu-id="9fc43-366">此頁面會呼叫*ListProductsForDelete.cshtml*也可以讓使用者確認他們想要刪除的產品。</span><span class="sxs-lookup"><span data-stu-id="9fc43-366">This page is called by *ListProductsForDelete.cshtml* and lets users confirm that they want to delete a product.</span></span> <span data-ttu-id="9fc43-367">若要列出要刪除的產品，您取得的產品識別碼，刪除從 URL 使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9fc43-367">To list the product to be deleted, you get the ID of the product to delete from the URL using the following code:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    <span data-ttu-id="9fc43-368">頁面接著會要求使用者按一下按鈕，即可實際刪除記錄。</span><span class="sxs-lookup"><span data-stu-id="9fc43-368">The page then asks the user to click a button to actually delete the record.</span></span> <span data-ttu-id="9fc43-369">這是一項重要的安全性措施： 當您執行敏感性作業在您的網站，例如更新或刪除資料時，這些作業應該永遠是使用 POST 作業，而不是 GET 作業。</span><span class="sxs-lookup"><span data-stu-id="9fc43-369">This is an important security measure: when you perform sensitive operations in your website like updating or deleting data, these operations should always be done using a POST operation, not a GET operation.</span></span> <span data-ttu-id="9fc43-370">如果您的網站已設定，以便可以使用 「 取得 」 作業來執行刪除作業，任何人都可以傳遞之類的 URL`http://<server>/DeleteProduct/4`並從資料庫中刪除它們想要的任何項目。</span><span class="sxs-lookup"><span data-stu-id="9fc43-370">If your site is set up so that a delete operation can be performed using a GET operation, anyone can pass a URL like `http://<server>/DeleteProduct/4` and delete anything they want from your database.</span></span> <span data-ttu-id="9fc43-371">藉由新增確認，並編碼的頁面，以便可以執行刪除，只能藉由使用某篇文章，以將的安全性的量值加入至您的站台的。</span><span class="sxs-lookup"><span data-stu-id="9fc43-371">By adding the confirmation and coding the page so that the deletion can be performed only by using a POST, you add a measure of security to your site.</span></span>

    <span data-ttu-id="9fc43-372">使用下列程式碼，先確認這是 post 作業，並確認識別碼不是空白，被執行實際的刪除作業：</span><span class="sxs-lookup"><span data-stu-id="9fc43-372">The actual delete operation is performed using the following code, which first confirms that this is a post operation and that the ID isn't empty:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    <span data-ttu-id="9fc43-373">執行 SQL 陳述式會刪除指定的記錄，然後將使用者重新導向回到清單頁面的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9fc43-373">The code runs a SQL statement that deletes the specified record and then redirects the user back to the listing page.</span></span>
5. <span data-ttu-id="9fc43-374">執行*ListProductsForDelete.cshtml*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="9fc43-374">Run *ListProductsForDelete.cshtml* in a browser.</span></span>

    ![[影像]](5-working-with-data/_static/image9.jpg)
6. <span data-ttu-id="9fc43-376">按一下 **刪除**連結其中一個產品。</span><span class="sxs-lookup"><span data-stu-id="9fc43-376">Click the **Delete** link for one of the products.</span></span> <span data-ttu-id="9fc43-377">*DeleteProduct.cshtml*頁面會顯示確認您想要刪除該記錄。</span><span class="sxs-lookup"><span data-stu-id="9fc43-377">The *DeleteProduct.cshtml* page is displayed to confirm that you want to delete that record.</span></span>
7. <span data-ttu-id="9fc43-378">按一下 [**刪除**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="9fc43-378">Click the **Delete** button.</span></span> <span data-ttu-id="9fc43-379">刪除產品記錄，並使用更新的產品清單重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="9fc43-379">The product record is deleted and the page is refreshed with an updated product listing.</span></span>

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a><span data-ttu-id="9fc43-380">連接到資料庫</span><span class="sxs-lookup"><span data-stu-id="9fc43-380">Connecting to a Database</span></span>
> 
> <span data-ttu-id="9fc43-381">您可以連接到有兩種資料庫。</span><span class="sxs-lookup"><span data-stu-id="9fc43-381">You can connect to a database in two ways.</span></span> <span data-ttu-id="9fc43-382">第一種是使用`Database.Open`方法並指定資料庫檔案的名稱 (較少 *.sdf*擴充功能):</span><span class="sxs-lookup"><span data-stu-id="9fc43-382">The first is to use the `Database.Open` method and to specify the name of the database file (less the *.sdf* extension):</span></span>
> 
> `var db = Database.Open("SmallBakery");`
> 
> <span data-ttu-id="9fc43-383">`Open`方法會假設。*sdf*檔案位於網站*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fc43-383">The `Open` method assumes that the .*sdf* file is in the website's *App\_Data* folder.</span></span> <span data-ttu-id="9fc43-384">此資料夾被專為保留資料。</span><span class="sxs-lookup"><span data-stu-id="9fc43-384">This folder is designed specifically for holding data.</span></span> <span data-ttu-id="9fc43-385">比方說，它有適當的權限，以允許讀取和寫入資料，網站，並基於安全性考量，WebMatrix 不允許存取的檔案從這個資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fc43-385">For example, it has appropriate permissions to allow the website to read and write data, and as a security measure, WebMatrix does not allow access to files from this folder.</span></span>
> 
> <span data-ttu-id="9fc43-386">第二種方式是使用連接字串。</span><span class="sxs-lookup"><span data-stu-id="9fc43-386">The second way is to use a connection string.</span></span> <span data-ttu-id="9fc43-387">連接字串會包含有關如何連接到資料庫的資訊。</span><span class="sxs-lookup"><span data-stu-id="9fc43-387">A connection string contains information about how to connect to a database.</span></span> <span data-ttu-id="9fc43-388">這可包括檔案路徑，或它可以包含在本機或遠端伺服器，以及使用者名稱和密碼來連線到該伺服器上的 SQL Server 資料庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="9fc43-388">This can include a file path, or it can include the name of a SQL Server database on a local or remote server, along with a user name and password to connect to that server.</span></span> <span data-ttu-id="9fc43-389">（如果您會將資料保留在集中管理的 SQL Server 版本，例如裝載提供者的網站上您一律使用連接字串來指定資料庫連接資訊。）</span><span class="sxs-lookup"><span data-stu-id="9fc43-389">(If you keep data in a centrally managed version of SQL Server, such as on a hosting provider's site, you always use a connection string to specify the database connection information.)</span></span>
> 
> <span data-ttu-id="9fc43-390">在 WebMatrix 中，連接字串通常會儲存在名為 XML 檔案*Web.config*。如同名稱所暗示，您可以使用*Web.config*來儲存站台的設定資訊，包括任何連接字串，您的網站可能會需要您網站的根目錄中的檔案。</span><span class="sxs-lookup"><span data-stu-id="9fc43-390">In WebMatrix, connection strings are usually stored in an XML file named *Web.config*. As the name implies, you can use a *Web.config* file in the root of your website to store the site's configuration information, including any connection strings that your site might require.</span></span> <span data-ttu-id="9fc43-391">中的連接字串的範例*Web.config*檔案可能看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="9fc43-391">An example of a connection string in a *Web.config* file might look like the following:</span></span>
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> <span data-ttu-id="9fc43-392">在範例中，連接字串會指向在某處的伺服器執行的 SQL Server 的執行個體中的資料庫 (而不是本機 *.sdf*檔案)。</span><span class="sxs-lookup"><span data-stu-id="9fc43-392">In the example, the connection string points to a database in an instance of SQL Server that's running on a server somewhere (as opposed to a local *.sdf* file).</span></span> <span data-ttu-id="9fc43-393">您會需要更換的適當名稱`myServer`並`myDatabase`，並且指定 SQL Server 登入值`username`和`password`。</span><span class="sxs-lookup"><span data-stu-id="9fc43-393">You would need to substitute the appropriate names for `myServer` and `myDatabase`, and specify SQL Server login values for `username` and `password`.</span></span> <span data-ttu-id="9fc43-394">（使用者名稱和密碼的值不一定相同當做 Windows 認證，或做為主控提供者已提供您登入伺服器值。</span><span class="sxs-lookup"><span data-stu-id="9fc43-394">(The username and password values are not necessarily the same as your Windows credentials or as the values that your hosting provider has given you for logging in to their servers.</span></span> <span data-ttu-id="9fc43-395">請與您所需要的確切值的系統管理員）。</span><span class="sxs-lookup"><span data-stu-id="9fc43-395">Check with the administrator for the exact values you need.)</span></span>
> 
> <span data-ttu-id="9fc43-396">`Database.Open`方法是有彈性，因為它可讓您將資料庫的名稱傳遞 *.sdf*檔案或儲存在連接字串名稱*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="9fc43-396">The `Database.Open` method is flexible, because it lets you pass either the name of a database *.sdf* file or the name of a connection string that's stored in the *Web.config* file.</span></span> <span data-ttu-id="9fc43-397">下列範例示範如何連接到資料庫使用的連接字串的前一個範例所示：</span><span class="sxs-lookup"><span data-stu-id="9fc43-397">The following example shows how to connect to the database using the connection string illustrated in the previous example:</span></span>
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> <span data-ttu-id="9fc43-398">如所述，`Database.Open`方法可讓您將資料庫名稱或連接字串，傳遞，它會找出要使用。</span><span class="sxs-lookup"><span data-stu-id="9fc43-398">As noted, the `Database.Open` method lets you pass either a database name or a connection string, and it'll figure out which to use.</span></span> <span data-ttu-id="9fc43-399">這會非常有用，當您部署 （發行） 網站。</span><span class="sxs-lookup"><span data-stu-id="9fc43-399">This is very useful when you deploy (publish) your website.</span></span> <span data-ttu-id="9fc43-400">您可以使用 *.sdf*中的檔案*應用程式\_資料*資料夾，當您開發並測試您的網站。</span><span class="sxs-lookup"><span data-stu-id="9fc43-400">You can use an *.sdf* file in the *App\_Data* folder when you're developing and testing your site.</span></span> <span data-ttu-id="9fc43-401">當您將您的站台移至實際伺服器時，您可以使用中的連接字串*Web.config*具有相同名稱的檔案您 *.sdf*檔案，但指向裝載提供者的資料庫&#8212;而完全不必變更程式碼。</span><span class="sxs-lookup"><span data-stu-id="9fc43-401">Then when you move your site to a production server, you can use a connection string in the *Web.config* file that has the same name as your *.sdf* file but that points to the hosting provider's database &#8212; all without having to change your code.</span></span>
> 
> <span data-ttu-id="9fc43-402">最後，如果您想要直接使用連接字串，您可以呼叫`Database.OpenConnectionString`方法並傳遞它實際的連接字串而不只在中的名稱*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="9fc43-402">Finally, if you want to work directly with a connection string, you can call the `Database.OpenConnectionString` method and pass it the actual connection string instead of just the name of one in the *Web.config* file.</span></span> <span data-ttu-id="9fc43-403">這可能會很有用，因為某些原因您沒有存取權的連接字串的情況下 (或值，例如 *.sdf*檔案名稱) 之前的頁面會執行。</span><span class="sxs-lookup"><span data-stu-id="9fc43-403">This might be useful in situations where for some reason you don't have access to the connection string (or values in it, such as the *.sdf* file name) until the page is running.</span></span> <span data-ttu-id="9fc43-404">不過，大部分的情況下，您可以使用`Database.Open`這篇文章中所述。</span><span class="sxs-lookup"><span data-stu-id="9fc43-404">However, for most scenarios, you can use `Database.Open` as described in this article.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="9fc43-405">其他資源</span><span class="sxs-lookup"><span data-stu-id="9fc43-405">Additional Resources</span></span>

- [<span data-ttu-id="9fc43-406">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="9fc43-406">SQL Server Compact</span></span>](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [<span data-ttu-id="9fc43-407">連接到 SQL Server 或在 WebMatrix 中的 MySQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="9fc43-407">Connecting to a SQL Server or MySQL Database in WebMatrix</span></span>](https://go.microsoft.com/fwlink/?LinkId=208661)
- [<span data-ttu-id="9fc43-408">在 ASP.NET Web Pages 網站中驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="9fc43-408">Validating User Input in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=253002)
