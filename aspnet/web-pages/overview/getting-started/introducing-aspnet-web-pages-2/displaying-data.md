---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: 導入 ASP.NET Web Pages-顯示資料 |Microsoft 文件
author: tfitzmac
description: 本教學課程會示範如何在 WebMatrix 中建立資料庫，以及如何顯示在網頁中的資料庫資料，當您使用 ASP.NET Web Pages (Razor)。 它會假設 y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 6c66e5fb0a1a49da411286e19c7954f83055c3fd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898453"
---
<a name="introducing-aspnet-web-pages---displaying-data"></a><span data-ttu-id="ea289-104">導入的 ASP.NET Web Pages-顯示資料</span><span class="sxs-lookup"><span data-stu-id="ea289-104">Introducing ASP.NET Web Pages - Displaying Data</span></span>
====================
<span data-ttu-id="ea289-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ea289-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ea289-106">本教學課程會示範如何在 WebMatrix 中建立資料庫，以及如何顯示在網頁中的資料庫資料，當您使用 ASP.NET Web Pages (Razor)。</span><span class="sxs-lookup"><span data-stu-id="ea289-106">This tutorial shows you how to create a database in WebMatrix and how to display database data in a page when you use ASP.NET Web Pages (Razor).</span></span> <span data-ttu-id="ea289-107">它會假設您已完成透過數列[簡介 ASP.NET Web Pages 程式設計](../introducing-razor-syntax-c.md)。</span><span class="sxs-lookup"><span data-stu-id="ea289-107">It assumes you have completed the series through [Introduction to ASP.NET Web Pages Programming](../introducing-razor-syntax-c.md).</span></span>
> 
> <span data-ttu-id="ea289-108">您將學習：</span><span class="sxs-lookup"><span data-stu-id="ea289-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="ea289-109">如何使用 WebMatrix 工具來建立資料庫和資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="ea289-109">How to use WebMatrix tools to create a database and database tables.</span></span>
> - <span data-ttu-id="ea289-110">如何使用 WebMatrix 工具將資料加入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="ea289-110">How to use WebMatrix tools to add data to a database.</span></span>
> - <span data-ttu-id="ea289-111">如何顯示網頁上的資料庫中的資料。</span><span class="sxs-lookup"><span data-stu-id="ea289-111">How to display data from the database on a page.</span></span>
> - <span data-ttu-id="ea289-112">如何將 ASP.NET Web Pages 中執行 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="ea289-112">How to run SQL commands in ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="ea289-113">如何自訂`WebGrid`helper 來變更資料顯示方式，以及加入分頁和排序。</span><span class="sxs-lookup"><span data-stu-id="ea289-113">How to customize the `WebGrid` helper to change the data display and to add paging and sorting.</span></span>
>   
> 
> <span data-ttu-id="ea289-114">功能/技術討論：</span><span class="sxs-lookup"><span data-stu-id="ea289-114">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="ea289-115">WebMatrix 資料庫工具。</span><span class="sxs-lookup"><span data-stu-id="ea289-115">WebMatrix database tools.</span></span>
> - <span data-ttu-id="ea289-116">`WebGrid` 協助程式。</span><span class="sxs-lookup"><span data-stu-id="ea289-116">`WebGrid` helper.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="ea289-117">您將建置</span><span class="sxs-lookup"><span data-stu-id="ea289-117">What You'll Build</span></span>

<span data-ttu-id="ea289-118">在上一個教學課程中，我們將會介紹至 ASP.NET Web Pages (*.cshtml*檔案)、 基本概念的 Razor 語法和 helper。</span><span class="sxs-lookup"><span data-stu-id="ea289-118">In the previous tutorial, you were introduced to ASP.NET Web Pages (*.cshtml* files), to the basics of Razor syntax, and to helpers.</span></span> <span data-ttu-id="ea289-119">在本教學課程中，開始，您將建立實際的 web 應用程式，您將序列的其餘部分使用。</span><span class="sxs-lookup"><span data-stu-id="ea289-119">In this tutorial, you'll begin creating the actual web application that you'll use for the rest of the series.</span></span> <span data-ttu-id="ea289-120">應用程式是簡單的電影應用程式，可讓您檢視、 加入、 變更與刪除電影的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="ea289-120">The app is a simple movie application that lets you view, add, change, and delete information about movies.</span></span>

<span data-ttu-id="ea289-121">當您完成本教學課程時，您可以檢視此頁面看起來的電影清單：</span><span class="sxs-lookup"><span data-stu-id="ea289-121">When you're done with this tutorial, you'll be able to view a movie listing that looks like this page:</span></span>

![WebGrid 顯示，其中包含參數設定到 CSS 類別名稱](displaying-data/_static/image1.png)

<span data-ttu-id="ea289-123">但是，若要開始，您必須建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="ea289-123">But to begin, you have to create a database.</span></span>

## <a name="a-very-brief-introduction-to-databases"></a><span data-ttu-id="ea289-124">資料庫非常簡短的簡介</span><span class="sxs-lookup"><span data-stu-id="ea289-124">A Very Brief Introduction to Databases</span></span>

<span data-ttu-id="ea289-125">本教學課程將提供僅資料庫 briefest 簡介。</span><span class="sxs-lookup"><span data-stu-id="ea289-125">This tutorial will provide only the briefest introduction to databases.</span></span> <span data-ttu-id="ea289-126">如果您曾經資料庫，您可以略過這個簡短的區段。</span><span class="sxs-lookup"><span data-stu-id="ea289-126">If you have database experience, you can skip this short section.</span></span>

<span data-ttu-id="ea289-127">資料庫包含一或多個包含資訊的資料表&mdash;，例如客戶、 訂單及廠商，或如學生、 老師、 類別和成績的資料表。</span><span class="sxs-lookup"><span data-stu-id="ea289-127">A database contains one or more tables that contain information &mdash; for example, tables for customers, orders, and vendors, or for students, teachers, classes, and grades.</span></span> <span data-ttu-id="ea289-128">就結構而言，資料庫資料表是試算表類似。</span><span class="sxs-lookup"><span data-stu-id="ea289-128">Structurally, a database table is like a spreadsheet.</span></span> <span data-ttu-id="ea289-129">假設典型的通訊錄。</span><span class="sxs-lookup"><span data-stu-id="ea289-129">Imagine a typical address book.</span></span> <span data-ttu-id="ea289-130">通訊錄中的每個項目 (也就是每位人員) 有數個部分的資訊，例如名字、 姓氏、 地址、 電子郵件地址和電話號碼。</span><span class="sxs-lookup"><span data-stu-id="ea289-130">For each entry in the address book (that is, for each person) you have several pieces of information such as first name, last name, address, email address, and phone number.</span></span>

![範例資料庫的資料表做為一個簡單的方格](displaying-data/_static/image2.png)

<span data-ttu-id="ea289-132">(資料列有時稱為*記錄*，和資料行有時稱為*欄位*。)</span><span class="sxs-lookup"><span data-stu-id="ea289-132">(Rows are sometimes referred to as *records*, and columns are sometimes referred to as *fields*.)</span></span>

<span data-ttu-id="ea289-133">對於大部分的資料庫資料表，資料表必須具有包含唯一的值，例如客戶編號、 帳戶號碼等的資料行。</span><span class="sxs-lookup"><span data-stu-id="ea289-133">For most database tables, the table has to have a column that contains a unique value, like a customer number, account number, and so on.</span></span> <span data-ttu-id="ea289-134">這個值便稱為資料表的*主索引鍵*，並用它來識別資料表中的每個資料列。</span><span class="sxs-lookup"><span data-stu-id="ea289-134">This value is known as the table's *primary key*, and you use it to identify each row in the table.</span></span> <span data-ttu-id="ea289-135">在範例中，識別碼資料行是前一範例所示的通訊錄的主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="ea289-135">In the example, the ID column is the primary key for the address book shown in the previous example.</span></span>

<span data-ttu-id="ea289-136">大部分您 web 應用程式中的工作包含讀取出資料庫的資訊，以及顯示在頁面上。</span><span class="sxs-lookup"><span data-stu-id="ea289-136">Much of the work you do in web applications consists of reading information out of the database and displaying it on a page.</span></span> <span data-ttu-id="ea289-137">您也經常會從使用者收集資訊並將它加入至資料庫時，或您將修改已經在資料庫中的記錄。</span><span class="sxs-lookup"><span data-stu-id="ea289-137">You'll also often gather information from users and add it to a database, or you'll modify records that are already in the database.</span></span> <span data-ttu-id="ea289-138">（我們將討論的所有這些作業，在此教學課程集過程中。）</span><span class="sxs-lookup"><span data-stu-id="ea289-138">(We'll cover all of these operations in the course of this tutorial set.)</span></span>

<span data-ttu-id="ea289-139">資料庫工作可能會提供很複雜，而且可能需要專業的知識。</span><span class="sxs-lookup"><span data-stu-id="ea289-139">Database work can be enormously complex and can require specialized knowledge.</span></span> <span data-ttu-id="ea289-140">這個教學課程的集合，不過，您必須了解只有基本概念，會將所有在說明當您瀏。</span><span class="sxs-lookup"><span data-stu-id="ea289-140">For this tutorial set, though, you have to understand only basic concepts, which will all be explained as you go.</span></span>

## <a name="creating-a-database"></a><span data-ttu-id="ea289-141">建立資料庫</span><span class="sxs-lookup"><span data-stu-id="ea289-141">Creating a Database</span></span>

<span data-ttu-id="ea289-142">WebMatrix 包含多種工具，讓您輕鬆建立資料庫，以及在資料庫中建立資料表。</span><span class="sxs-lookup"><span data-stu-id="ea289-142">WebMatrix includes tools that make it easy to create a database and to create tables in the database.</span></span> <span data-ttu-id="ea289-143">(資料庫的結構指資料庫*結構描述*。)設定這個教學課程，您將建立的資料庫中有一份資料表&mdash;電影。</span><span class="sxs-lookup"><span data-stu-id="ea289-143">(The structure of a database is referred to as the database's *schema*.) For this tutorial set, you'll create a database that has only one table in it &mdash; Movies.</span></span>

<span data-ttu-id="ea289-144">如果您沒有這樣做，開啟 WebMatrix 並開啟您在上一個教學課程中建立的 WebPagesMovies 站台。</span><span class="sxs-lookup"><span data-stu-id="ea289-144">Open WebMatrix if you haven't already done so, and open the WebPagesMovies site that you created in the previous tutorial.</span></span>

<span data-ttu-id="ea289-145">在左窗格中，按一下 **資料庫**工作區。</span><span class="sxs-lookup"><span data-stu-id="ea289-145">In the left pane, click the **Database** workspace.</span></span>

![WebMatrix 資料庫工作區 索引標籤](displaying-data/_static/image3.png)

<span data-ttu-id="ea289-147">功能區的變更，以顯示與資料庫相關的工作。</span><span class="sxs-lookup"><span data-stu-id="ea289-147">The ribbon changes to show database-related tasks.</span></span> <span data-ttu-id="ea289-148">在 功能區中，按一下 **新資料庫**。</span><span class="sxs-lookup"><span data-stu-id="ea289-148">In the ribbon, click **New Database**.</span></span>

![WebMatrix 功能區中的 '新的資料庫' 按鈕](displaying-data/_static/image4.png)

<span data-ttu-id="ea289-150">WebMatrix 會建立 SQL Server CE 的資料庫 ( *.sdf*檔案) 做為您網站具有相同名稱&mdash; *WebPagesMovies.sdf*。</span><span class="sxs-lookup"><span data-stu-id="ea289-150">WebMatrix creates a SQL Server CE database (an *.sdf* file) that has the same name as your site &mdash; *WebPagesMovies.sdf*.</span></span> <span data-ttu-id="ea289-151">(您將不會執行這項操作，但您可以將檔案重新命名任何想要的話，因為它具有 *.sdf*擴充功能。)</span><span class="sxs-lookup"><span data-stu-id="ea289-151">(You won't do this here, but you can rename the file to anything you like, as long as it has an *.sdf* extension.)</span></span>

## <a name="creating-a-table"></a><span data-ttu-id="ea289-152">建立資料表</span><span class="sxs-lookup"><span data-stu-id="ea289-152">Creating a Table</span></span>

<span data-ttu-id="ea289-153">在 功能區中，按一下 **新資料表**。</span><span class="sxs-lookup"><span data-stu-id="ea289-153">In the ribbon, click **New Table**.</span></span> <span data-ttu-id="ea289-154">WebMatrix 會在新索引標籤中開啟資料表設計工具。（如果無法使用新的資料表選項，請確定在左側樹狀檢視中已選取新的資料庫。）</span><span class="sxs-lookup"><span data-stu-id="ea289-154">WebMatrix opens the table designer in a new tab. (If the New Table option isn't available, make sure that the new database is selected in the tree view on the left.)</span></span>

![WebMatrix 功能區中的 '新 Table' 按鈕](displaying-data/_static/image5.png)

<span data-ttu-id="ea289-156">在頂端 （浮水印說 「 輸入資料表名稱 」） 文字方塊中輸入 「 電影 」。</span><span class="sxs-lookup"><span data-stu-id="ea289-156">In the text box at the top (where the watermark says "Enter table name"), enter "Movies".</span></span>

![WebMatrix 資料庫設計工具中輸入資料表名稱](displaying-data/_static/image6.png)

<span data-ttu-id="ea289-158">資料表名稱下方的窗格是您在其中定義個別的資料行。</span><span class="sxs-lookup"><span data-stu-id="ea289-158">The pane underneath the table name is where you define individual columns.</span></span> <span data-ttu-id="ea289-159">如*電影*資料表在此教學課程中，您會建立只有幾個資料行：*識別碼*，*標題*，*類型*，和*年*.</span><span class="sxs-lookup"><span data-stu-id="ea289-159">For the *Movies* table in this tutorial, you'll create only a few columns: *ID*, *Title*, *Genre*, and *Year*.</span></span>

<span data-ttu-id="ea289-160">在**名稱**方塊中，輸入 「 識別碼 」。</span><span class="sxs-lookup"><span data-stu-id="ea289-160">In the **Name** box, enter "ID".</span></span> <span data-ttu-id="ea289-161">輸入的值，就會啟動新的資料行的所有控制項。</span><span class="sxs-lookup"><span data-stu-id="ea289-161">Entering a value here activates all the controls for the new column.</span></span>

<span data-ttu-id="ea289-162">索引標籤來**資料型別**清單，然後選擇**int**。這個值會指定 ID 資料行，將包含整數 （值） 的資料。</span><span class="sxs-lookup"><span data-stu-id="ea289-162">Tab to the **Data Type** list and choose **int**. This value specifies that the ID column will contain integer (number) data.</span></span>

> [!NOTE]
> <span data-ttu-id="ea289-163">我們將不會呼叫它任何這裡進一步說明 （遠），但是您可以使用標準 Windows 鍵盤手勢來瀏覽此方格中。</span><span class="sxs-lookup"><span data-stu-id="ea289-163">We won't call it out any more here (much), but you can use standard Windows keyboard gestures to navigate in this grid.</span></span> <span data-ttu-id="ea289-164">比方說，您可以索引標籤上，欄位之間，您可以只開始輸入，就可以在清單中，選取項目等等。</span><span class="sxs-lookup"><span data-stu-id="ea289-164">For example, you can tab between fields, you can just start typing in order to select an item in a list, and so on.</span></span>


<span data-ttu-id="ea289-165">索引標籤上超過**預設值**方塊 （也就是保留空白）。</span><span class="sxs-lookup"><span data-stu-id="ea289-165">Tab past the **Default Value** box (that is, leave it blank).</span></span> <span data-ttu-id="ea289-166">索引標籤來**是主索引鍵**核取方塊，並加以選取。</span><span class="sxs-lookup"><span data-stu-id="ea289-166">Tab to the **Is Primary Key** check box and select it.</span></span> <span data-ttu-id="ea289-167">此選項會告知的資料庫，*識別碼*資料行會包含識別個別資料列的資料。</span><span class="sxs-lookup"><span data-stu-id="ea289-167">This option tells the database that the *ID* column will contain the data that identifies individual rows.</span></span> <span data-ttu-id="ea289-168">（也就是每個資料列將可用來尋找該資料列的識別碼資料行具有唯一值）。</span><span class="sxs-lookup"><span data-stu-id="ea289-168">(That is, each row will have a unique value in the ID column that you can use to find that row.)</span></span>

<span data-ttu-id="ea289-169">選擇**為識別**選項。</span><span class="sxs-lookup"><span data-stu-id="ea289-169">Choose the **Is Identity** option.</span></span> <span data-ttu-id="ea289-170">此選項會告訴資料庫應該自動產生下一個循序號碼，每個新的資料列。</span><span class="sxs-lookup"><span data-stu-id="ea289-170">This option tells the database that it should automatically generate the next sequential number for each new row.</span></span> <span data-ttu-id="ea289-171">(**為識別**選項適用於只有當您也選取**是主索引鍵**選項。)</span><span class="sxs-lookup"><span data-stu-id="ea289-171">(The **Is Identity** option works only if you've also selected the **Is Primary Key** option.)</span></span>

<span data-ttu-id="ea289-172">按一下 下一步的方格資料列，或按兩次以完成目前的資料列的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="ea289-172">Click in the next grid row, or press Tab twice to finish the current row.</span></span> <span data-ttu-id="ea289-173">其中一個軌跡會儲存目前的資料列，並啟動下一個。</span><span class="sxs-lookup"><span data-stu-id="ea289-173">Either gesture saves the current row and starts the next one.</span></span> <span data-ttu-id="ea289-174">請注意，**預設值**資料行現在顯示**Null**。</span><span class="sxs-lookup"><span data-stu-id="ea289-174">Notice that the **Default Value** column now says **Null**.</span></span> <span data-ttu-id="ea289-175">（預設值是 null 的預設值，所以。）</span><span class="sxs-lookup"><span data-stu-id="ea289-175">(Null is the default value for the default value, so to speak.)</span></span>

<span data-ttu-id="ea289-176">當您完成定義新**識別碼**資料行，在設計工具看起來像下圖：</span><span class="sxs-lookup"><span data-stu-id="ea289-176">When you've finished defining the new **ID** column, the designer will look like this illustration:</span></span>

![WebMatrix 定義電影資料表的識別碼資料行之後的資料庫設計工具](displaying-data/_static/image7.png)

<span data-ttu-id="ea289-178">若要建立下一個資料行，按一下方塊中**名稱**資料行。</span><span class="sxs-lookup"><span data-stu-id="ea289-178">To create the next column, click in the box in the **Name** column.</span></span> <span data-ttu-id="ea289-179">資料行中輸入"Title"，然後選取**nvarchar**如**資料型別**值。</span><span class="sxs-lookup"><span data-stu-id="ea289-179">Enter "Title" for the column and then select **nvarchar** for the **Data Type** value.</span></span> <span data-ttu-id="ea289-180">"Var"一部分**nvarchar**告訴資料庫，此資料行的資料將會大小可能會記錄記錄不同的字串。</span><span class="sxs-lookup"><span data-stu-id="ea289-180">The "var" part of **nvarchar** tells the database that the data for this column will be a string whose size might vary from record to record.</span></span> <span data-ttu-id="ea289-181">("N"前置詞代表"national，"表示欄位可以包含任何字母或書寫系統的字元資料，亦即，欄位就會保留 Unicode 資料。)</span><span class="sxs-lookup"><span data-stu-id="ea289-181">(The "n" prefix represents "national," which indicates that the field can hold character data for any alphabet or writing system — that is, the field holds Unicode data.)</span></span>

<span data-ttu-id="ea289-182">當您選擇**nvarchar**，另一個方塊會顯示您可以在其中輸入欄位的最大長度。</span><span class="sxs-lookup"><span data-stu-id="ea289-182">When you choose **nvarchar**, another box appears where you can enter the maximum length for the field.</span></span> <span data-ttu-id="ea289-183">輸入 50，假設您將在本教學課程使用沒有電影標題將會超過 50 個字元。</span><span class="sxs-lookup"><span data-stu-id="ea289-183">Enter 50, on the assumption that no movie title that you'll work with in this tutorial will be longer than 50 characters.</span></span>

<span data-ttu-id="ea289-184">略過**預設值**清除**允許 Null**選項。</span><span class="sxs-lookup"><span data-stu-id="ea289-184">Skip **Default Value** and clear the **Allow Nulls** option.</span></span> <span data-ttu-id="ea289-185">您不想要允許任何沒有標題的電影以輸入到資料庫的資料庫。</span><span class="sxs-lookup"><span data-stu-id="ea289-185">You don't want the database to allow any movies to be entered into the database that don't have a title.</span></span>

<span data-ttu-id="ea289-186">當您完成時，並移至下一個資料列時，在設計工具看起來像下圖中：</span><span class="sxs-lookup"><span data-stu-id="ea289-186">When you're done and move to the next row, the designer looks like this illustration:</span></span>

![WebMatrix 定義電影表格的標題資料行之後的資料庫設計工具](displaying-data/_static/image8.png)

<span data-ttu-id="ea289-188">重複這些步驟來建立資料行長度，除了名為 「 類型 」，將它設定為只 30。</span><span class="sxs-lookup"><span data-stu-id="ea289-188">Repeat these steps to create a column named "Genre", except for the length, set it to just 30.</span></span>

<span data-ttu-id="ea289-189">建立另一個資料行名為 「 年 」。</span><span class="sxs-lookup"><span data-stu-id="ea289-189">Create another column named "Year."</span></span> <span data-ttu-id="ea289-190">資料型別中，選擇  **nchar** (不**nvarchar**) 並將長度設定為 4。</span><span class="sxs-lookup"><span data-stu-id="ea289-190">For the data type, choose **nchar** (not **nvarchar**) and set the length to 4.</span></span> <span data-ttu-id="ea289-191">年度您要使用 4 位數的數字"1995"或"2010"，因此您不需要可變動大小的資料行。</span><span class="sxs-lookup"><span data-stu-id="ea289-191">For the year, you're going to use a 4-digit number like "1995" or "2010", so you don't require a variable-sized column.</span></span>

<span data-ttu-id="ea289-192">以下是完成的設計看起來像：</span><span class="sxs-lookup"><span data-stu-id="ea289-192">Here's what the finished design looks like:</span></span>

![WebMatrix 資料庫設計工具之後電影資料表未定義所有的欄位](displaying-data/_static/image9.png)

<span data-ttu-id="ea289-194">按下 Ctrl + S 或按一下**儲存**中快速存取工具列按鈕。</span><span class="sxs-lookup"><span data-stu-id="ea289-194">Press Ctrl+S or click the **Save** button in the Quick Access toolbar.</span></span> <span data-ttu-id="ea289-195">關閉索引標籤，以關閉資料庫設計工具。</span><span class="sxs-lookup"><span data-stu-id="ea289-195">Close the database designer by closing the tab.</span></span>

## <a name="adding-some-example-data"></a><span data-ttu-id="ea289-196">加入一些範例資料</span><span class="sxs-lookup"><span data-stu-id="ea289-196">Adding Some Example Data</span></span>

<span data-ttu-id="ea289-197">稍後在本教學課程系列，您將建立的頁面，讓您在表單中輸入新的電影。</span><span class="sxs-lookup"><span data-stu-id="ea289-197">Later in this tutorial series you'll create a page where you can enter new movies in a form.</span></span> <span data-ttu-id="ea289-198">不過，現在，您可以加入一些您可以在頁面顯示的範例資料。</span><span class="sxs-lookup"><span data-stu-id="ea289-198">For now, however, you can add some example data that you can then display on a page.</span></span>

<span data-ttu-id="ea289-199">在**資料庫**在 WebMatrix 中，請注意，有樹，其中顯示您的工作區 *.sdf*您稍早建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="ea289-199">In the **Database** workspace in WebMatrix, notice that there's a tree that shows you the *.sdf* file you created earlier.</span></span> <span data-ttu-id="ea289-200">開啟新的節點 *.sdf*檔案，然後再開啟**資料表**節點。</span><span class="sxs-lookup"><span data-stu-id="ea289-200">Open the node for your new *.sdf* file, and then open the **Tables** node.</span></span>

![與樹狀目錄中開啟影片資料表 WebMatrix 資料庫 」 工作區](displaying-data/_static/image10.png)

<span data-ttu-id="ea289-202">以滑鼠右鍵按一下**電影**節點，然後選擇 **資料**。</span><span class="sxs-lookup"><span data-stu-id="ea289-202">Right-click the **Movies** node and then choose **Data**.</span></span> <span data-ttu-id="ea289-203">WebMatrix 開啟的方格，您可以在此輸入資料*電影*資料表：</span><span class="sxs-lookup"><span data-stu-id="ea289-203">WebMatrix opens a grid where you can enter data for the *Movies* table:</span></span>

![在 WebMatrix （空白） 資料庫項目方格](displaying-data/_static/image11.png)

<span data-ttu-id="ea289-205">按一下**標題**資料行，並輸入"時 Harry 符合 Sally"。</span><span class="sxs-lookup"><span data-stu-id="ea289-205">Click the **Title** column and enter "When Harry Met Sally".</span></span> <span data-ttu-id="ea289-206">移至**類型**（您可以使用 Tab 鍵） 的資料行，並輸入"浪漫喜劇"。</span><span class="sxs-lookup"><span data-stu-id="ea289-206">Move to the **Genre** column (you can use the Tab key) and enter "Romantic Comedy".</span></span> <span data-ttu-id="ea289-207">移至**年**資料行，並輸入"1989":</span><span class="sxs-lookup"><span data-stu-id="ea289-207">Move to the **Year** column and enter "1989":</span></span>

![在 WebMatrix 有一筆記錄的資料庫項目方格](displaying-data/_static/image12.png)

<span data-ttu-id="ea289-209">按下 Enter，WebMatrix 會將儲存新的電影。</span><span class="sxs-lookup"><span data-stu-id="ea289-209">Press Enter, and WebMatrix saves the new movie.</span></span> <span data-ttu-id="ea289-210">請注意，**識別碼**已填入資料行。</span><span class="sxs-lookup"><span data-stu-id="ea289-210">Notice that the **ID** column has been filled in.</span></span>

![在 WebMatrix 中的一筆記錄和自動產生識別碼的資料庫項目方格](displaying-data/_static/image13.png)

<span data-ttu-id="ea289-212">請輸入其他影片 (比方說，「 不存在與 Wind"，"戲劇"，"1939")。</span><span class="sxs-lookup"><span data-stu-id="ea289-212">Enter another movie (for example, "Gone with the Wind", "Drama", "1939").</span></span> <span data-ttu-id="ea289-213">識別碼資料行是一次填入：</span><span class="sxs-lookup"><span data-stu-id="ea289-213">The ID column is filled in again:</span></span>

![在 WebMatrix 中兩筆記錄與自動產生 Id 的資料庫項目方格](displaying-data/_static/image14.png)

<span data-ttu-id="ea289-215">輸入第三個影片 （例如，"Ghostbusters"、"喜劇"）。</span><span class="sxs-lookup"><span data-stu-id="ea289-215">Enter a third movie (for example, "Ghostbusters", "Comedy").</span></span> <span data-ttu-id="ea289-216">做一個試驗，保留**年**資料行的空白，然後按 Enter。</span><span class="sxs-lookup"><span data-stu-id="ea289-216">As an experiment, leave the **Year** column blank and then press Enter.</span></span> <span data-ttu-id="ea289-217">因為您取消選取 **允許 Null**選項，資料庫便會顯示錯誤：</span><span class="sxs-lookup"><span data-stu-id="ea289-217">Because you unselected the **Allow Nulls** option, the database shows an error:</span></span>

![如果必要的資料行值保留空白，顯示 「 無效的資料 」 錯誤](displaying-data/_static/image15.png)

<span data-ttu-id="ea289-219">按一下**確定**返回並修正的項目 （「 Ghostbusters"的年份 1984年），然後按 Enter 鍵。</span><span class="sxs-lookup"><span data-stu-id="ea289-219">Click **OK** to go back and fix the entry (the year for "Ghostbusters" is 1984), and then press Enter.</span></span>

<span data-ttu-id="ea289-220">填滿之前有 8，或讓數個電影中。</span><span class="sxs-lookup"><span data-stu-id="ea289-220">Fill in several movies until you have 8 or so.</span></span> <span data-ttu-id="ea289-221">（輸入 8 可讓您更輕鬆地與更新版本的分頁。</span><span class="sxs-lookup"><span data-stu-id="ea289-221">(Entering 8 makes it easier to work with paging later.</span></span> <span data-ttu-id="ea289-222">但如果太多，輸入幾現在）。實際資料並不重要。</span><span class="sxs-lookup"><span data-stu-id="ea289-222">But if that's too many, enter just a few for now.) The actual data doesn't matter.</span></span>

![在 WebMatrix 中任一個記錄的資料庫項目方格](displaying-data/_static/image16.png)

<span data-ttu-id="ea289-224">如果您沒有任何錯誤的所有電影中都輸入是連續的識別碼值。</span><span class="sxs-lookup"><span data-stu-id="ea289-224">If you entered all the movies without any errors, the ID values are sequential.</span></span> <span data-ttu-id="ea289-225">如果您嘗試儲存未完成的電影錄製時，可能不是循序的識別碼。</span><span class="sxs-lookup"><span data-stu-id="ea289-225">If you tried to save an incomplete movie record, the ID numbers might not be sequential.</span></span> <span data-ttu-id="ea289-226">如果是的話，也沒關係。</span><span class="sxs-lookup"><span data-stu-id="ea289-226">If so, that's okay.</span></span> <span data-ttu-id="ea289-227">數字沒有任何原有的意義，而且唯一很重要的是這些是唯一的*電影*資料表。</span><span class="sxs-lookup"><span data-stu-id="ea289-227">The numbers don't have any inherent meaning, and the only thing that's important is that they're unique in the *Movies* table.</span></span>

<span data-ttu-id="ea289-228">關閉包含資料庫設計工具的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="ea289-228">Close the tab that contains the database designer.</span></span>

<span data-ttu-id="ea289-229">現在您可以開啟網頁上顯示這個資料。</span><span class="sxs-lookup"><span data-stu-id="ea289-229">Now you can turn to displaying this data on a web page.</span></span>

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a><span data-ttu-id="ea289-230">在頁面中顯示資料，利用 WebGrid 協助程式</span><span class="sxs-lookup"><span data-stu-id="ea289-230">Displaying Data in a Page by Using the WebGrid Helper</span></span>

<span data-ttu-id="ea289-231">若要在網頁中顯示資料，您要使用`WebGrid`協助程式。</span><span class="sxs-lookup"><span data-stu-id="ea289-231">To display data in a page, you're going to use the `WebGrid` helper.</span></span> <span data-ttu-id="ea289-232">此協助程式會顯示在方格或資料表 （資料列和資料行）。</span><span class="sxs-lookup"><span data-stu-id="ea289-232">This helper produces a display in a grid or table (rows and columns).</span></span> <span data-ttu-id="ea289-233">如您所見，您可能會無法縮小方格格式及其他功能。</span><span class="sxs-lookup"><span data-stu-id="ea289-233">As you'll see, you'll be able refine the grid with formatting and other features.</span></span>

<span data-ttu-id="ea289-234">若要執行的方格，您必須撰寫幾行程式碼。</span><span class="sxs-lookup"><span data-stu-id="ea289-234">To run the grid, you'll have to write a few lines of code.</span></span> <span data-ttu-id="ea289-235">下列幾行將做為一種幾乎所有您在本教學課程中執行資料存取模式。</span><span class="sxs-lookup"><span data-stu-id="ea289-235">These few lines will serve as a kind of pattern for almost all of the data access that you do in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="ea289-236">您實際上有許多選擇可用來顯示資料在頁面上。`WebGrid` helper 只是其中一個。</span><span class="sxs-lookup"><span data-stu-id="ea289-236">You actually have many options for displaying data on a page; the `WebGrid` helper is just one.</span></span> <span data-ttu-id="ea289-237">我們之所以選擇它本教學課程中，因為它最簡單的方式顯示資料，所以相當有彈性。</span><span class="sxs-lookup"><span data-stu-id="ea289-237">We chose it for this tutorial because it's the easiest way to display data and because it's reasonably flexible.</span></span> <span data-ttu-id="ea289-238">在下一個教學課程組中，您會看到如何使用更多的"manual"方式來處理在頁面中，可讓您更直接控制如何顯示資料的資料。</span><span class="sxs-lookup"><span data-stu-id="ea289-238">In the next tutorial set, you'll see how to use a more "manual" way to work with data in the page, which gives you more direct control over how to display the data.</span></span>


<span data-ttu-id="ea289-239">在 WebMatrix 中左窗格中，按一下 **檔案**工作區。</span><span class="sxs-lookup"><span data-stu-id="ea289-239">In the left pane in WebMatrix, click the **Files** workspace.</span></span>

<span data-ttu-id="ea289-240">您建立新的資料庫處於*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="ea289-240">The new database you created is in the *App\_Data* folder.</span></span> <span data-ttu-id="ea289-241">如果資料夾不存在，WebMatrix 會建立它為您新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="ea289-241">If the folder didn't already exist, WebMatrix created it for your new database.</span></span> <span data-ttu-id="ea289-242">（資料夾可能已經存在於如果您先前已安裝的協助程式。）</span><span class="sxs-lookup"><span data-stu-id="ea289-242">(The folder might have existed if you'd previously installed helpers.)</span></span>

<span data-ttu-id="ea289-243">在樹狀檢視中，選取網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="ea289-243">In the tree view, select the root of the website.</span></span> <span data-ttu-id="ea289-244">您必須選取網站; 的根目錄否則，新的檔案新增到應用程式\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ea289-244">You must select the root of the website; otherwise, the new file might be added to the App\_Data folder.</span></span>

<span data-ttu-id="ea289-245">在 功能區中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="ea289-245">In the ribbon, click **New**.</span></span> <span data-ttu-id="ea289-246">在**選擇檔案型別**方塊中，選擇**CSHTML**。</span><span class="sxs-lookup"><span data-stu-id="ea289-246">In the **Choose a File Type** box, choose **CSHTML**.</span></span>

<span data-ttu-id="ea289-247">在**名稱**方塊，將新的頁面 」 Movies.cshtml":</span><span class="sxs-lookup"><span data-stu-id="ea289-247">In the **Name** box, name the new page "Movies.cshtml":</span></span>

!['選擇的檔案類型 對話方塊，顯示 '電影' 頁面](displaying-data/_static/image17.png)

<span data-ttu-id="ea289-249">按一下**確定** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="ea289-249">Click the **OK** button.</span></span> <span data-ttu-id="ea289-250">WebMatrix 會開啟新檔案，某些基本架構中的項目它。</span><span class="sxs-lookup"><span data-stu-id="ea289-250">WebMatrix opens a new file with some skeleton elements in it.</span></span> <span data-ttu-id="ea289-251">第一次您將撰寫一些程式碼，請從資料庫取得資料。</span><span class="sxs-lookup"><span data-stu-id="ea289-251">First you'll write some code to go get the data from the database.</span></span> <span data-ttu-id="ea289-252">然後您會將標記加入頁面以實際顯示資料。</span><span class="sxs-lookup"><span data-stu-id="ea289-252">Then you'll add markup to the page to actually display the data.</span></span>

### <a name="writing-the-data-query-code"></a><span data-ttu-id="ea289-253">寫入資料的查詢程式碼</span><span class="sxs-lookup"><span data-stu-id="ea289-253">Writing the Data Query Code</span></span>

<span data-ttu-id="ea289-254">在頁面頂端之間`@{`和`}`字元，輸入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="ea289-254">At the top of the page, between the `@{` and `}` characters, enter the following code.</span></span> <span data-ttu-id="ea289-255">（請確認您輸入的開頭和右大括號之間的這段程式碼）。</span><span class="sxs-lookup"><span data-stu-id="ea289-255">(Make sure that you enter this code between the opening and closing braces.)</span></span>

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

<span data-ttu-id="ea289-256">第一行會開啟您稍早建立的資料庫是一律的第一個步驟之前執行的資料庫。</span><span class="sxs-lookup"><span data-stu-id="ea289-256">The first line opens the database that you created earlier, which is always the first step before doing something with the database.</span></span> <span data-ttu-id="ea289-257">您可告知`Database.Open`方法開啟的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="ea289-257">You tell the `Database.Open` method name of the database to open.</span></span> <span data-ttu-id="ea289-258">請注意，您不想加入 *.sdf*名稱中。</span><span class="sxs-lookup"><span data-stu-id="ea289-258">Notice that you don't include *.sdf* in the name.</span></span> <span data-ttu-id="ea289-259">`Open`方法會假設它正在尋求 *.sdf*檔案 (也就是*WebPagesMovies.sdf*) 而且 *.sdf*檔案位於*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="ea289-259">The `Open` method assumes that it's looking for an *.sdf* file (that is, *WebPagesMovies.sdf*) and that the *.sdf* file is in the *App\_Data* folder.</span></span> <span data-ttu-id="ea289-260">(我們稍早所述，*應用程式\_資料*資料夾已保留，則為此案例是在其中一個位置，ASP.NET 會使該名稱的相關假設。)</span><span class="sxs-lookup"><span data-stu-id="ea289-260">(Earlier we noted that the *App\_Data* folder is reserved; this scenario is one of the places where ASP.NET makes assumptions about that name.)</span></span>

<span data-ttu-id="ea289-261">開啟資料庫時，它的參考會放入名為的變數`db`。</span><span class="sxs-lookup"><span data-stu-id="ea289-261">When the database is opened, a reference to it is put into the variable named `db`.</span></span> <span data-ttu-id="ea289-262">（這可以命名為任何項目。）`db`變數是最後會與資料庫互動方式。</span><span class="sxs-lookup"><span data-stu-id="ea289-262">(Which could be named anything.) The `db` variable is how you'll end up interacting with the database.</span></span>

<span data-ttu-id="ea289-263">第二行實際上會擷取資料庫資料使用`Query`方法。</span><span class="sxs-lookup"><span data-stu-id="ea289-263">The second line actually fetches the database data by using the `Query` method.</span></span> <span data-ttu-id="ea289-264">請注意這段程式碼的運作方式：`db`變數有開啟的資料庫的參考，而且您叫用`Query`方法藉由使用`db`變數 (`db.Query`)。</span><span class="sxs-lookup"><span data-stu-id="ea289-264">Notice how this code works: the `db` variable has a reference to the opened database, and you invoke the `Query` method by using the `db` variable (`db.Query`).</span></span>

<span data-ttu-id="ea289-265">查詢本身是 SQL`Select`陳述式。</span><span class="sxs-lookup"><span data-stu-id="ea289-265">The query itself is a SQL `Select` statement.</span></span> <span data-ttu-id="ea289-266">（如有點 SQL 的詳細背景，請參閱稍後說明）。在陳述式，`Movies`識別要查詢的資料表。</span><span class="sxs-lookup"><span data-stu-id="ea289-266">(For a little background about SQL, see the explanation later.) In the statement, `Movies` identifies the table to query.</span></span> <span data-ttu-id="ea289-267">`*`字元會指定查詢應該從資料表傳回所有資料行。</span><span class="sxs-lookup"><span data-stu-id="ea289-267">The `*` character specifies that the query should return all the columns from the table.</span></span> <span data-ttu-id="ea289-268">（您可能也列出資料行個別，並以逗號分隔。）</span><span class="sxs-lookup"><span data-stu-id="ea289-268">(You could also list columns individually, separated by commas.)</span></span>

<span data-ttu-id="ea289-269">結果的查詢，如果有的話，會傳回，而且可在 `selectedData`變數。</span><span class="sxs-lookup"><span data-stu-id="ea289-269">The results of the query, if any, are returned and made available in the `selectedData` variable.</span></span> <span data-ttu-id="ea289-270">同樣地，變數可以命名為任何項目。</span><span class="sxs-lookup"><span data-stu-id="ea289-270">Again, the variable could be named anything.</span></span>

<span data-ttu-id="ea289-271">最後，第三行會告知 ASP.NET 您想要使用的執行個體`WebGrid`協助程式。</span><span class="sxs-lookup"><span data-stu-id="ea289-271">Finally, the third line tells ASP.NET that you want to use an instance of the `WebGrid` helper.</span></span> <span data-ttu-id="ea289-272">您所建立 (*具現化*) 協助程式物件，使用`new`關鍵字，並將它傳遞查詢結果，透過`selectedData`變數。</span><span class="sxs-lookup"><span data-stu-id="ea289-272">You create (*instantiate*) the helper object by using the `new` keyword and pass it the query results via the `selectedData` variable.</span></span> <span data-ttu-id="ea289-273">新`WebGrid`物件，以及在資料庫查詢的結果會提供在`grid`變數。</span><span class="sxs-lookup"><span data-stu-id="ea289-273">The new `WebGrid` object, along with the results of the database query, are made available in the `grid` variable.</span></span> <span data-ttu-id="ea289-274">您將需要該結果立即實際頁面中顯示資料。</span><span class="sxs-lookup"><span data-stu-id="ea289-274">You'll need that result in a moment to actually display the data in the page.</span></span>

<span data-ttu-id="ea289-275">在這個階段中，開啟資料庫，您已在 取得資料，而且您已備妥`WebGrid`包含該資料的協助程式。</span><span class="sxs-lookup"><span data-stu-id="ea289-275">At this stage, the database has been opened, you've gotten the data you want, and you've prepared the `WebGrid` helper with that data.</span></span> <span data-ttu-id="ea289-276">下一步是建立網頁中的標記。</span><span class="sxs-lookup"><span data-stu-id="ea289-276">Next is to create the markup in the page.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="ea289-277">**結構化的查詢語言 (SQL)**</span><span class="sxs-lookup"><span data-stu-id="ea289-277">**Structured Query Language (SQL)**</span></span>
> 
> <span data-ttu-id="ea289-278">SQL 是一種語言，用來在大部分的關聯式資料庫管理資料庫中的資料。</span><span class="sxs-lookup"><span data-stu-id="ea289-278">SQL is a language that's used in most relational databases for managing data in a database.</span></span> <span data-ttu-id="ea289-279">它包含的命令，可讓您擷取資料並加以更新和，可讓您建立、 修改及管理資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="ea289-279">It includes commands that let you retrieve data and update it, and that let you create, modify, and manage data in database tables.</span></span> <span data-ttu-id="ea289-280">SQL 是不同於程式設計語言 （例如 C# 中)。</span><span class="sxs-lookup"><span data-stu-id="ea289-280">SQL is different than a programming language (like C#).</span></span> <span data-ttu-id="ea289-281">使用 SQL，您需要告訴資料庫決定，而且它是資料庫的工作，以了解如何取得資料，或執行工作。</span><span class="sxs-lookup"><span data-stu-id="ea289-281">With SQL, you tell the database what you want, and it's the database's job to figure out how to get the data or perform the task.</span></span> <span data-ttu-id="ea289-282">以下是一些 SQL 命令的範例，以及它們執行的動作：</span><span class="sxs-lookup"><span data-stu-id="ea289-282">Here are examples of some SQL commands and what they do:</span></span>
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> <span data-ttu-id="ea289-283">第一個`Select`陳述式取得的所有資料行 (所指定`*`) 從*電影*資料表。</span><span class="sxs-lookup"><span data-stu-id="ea289-283">The first `Select` statement gets all the columns (specified by `*`) from the *Movies* table.</span></span>
> 
> <span data-ttu-id="ea289-284">第二個`Select`陳述式會從記錄中擷取的識別碼、 名稱和價格的資料行*產品*價格資料行值是 10 個以上的資料表。</span><span class="sxs-lookup"><span data-stu-id="ea289-284">The second `Select` statement fetches the ID, Name, and Price columns from records in the *Product* table whose Price column value is more than 10.</span></span> <span data-ttu-id="ea289-285">此命令會傳回結果，依字母順序，根據名稱資料行的值。</span><span class="sxs-lookup"><span data-stu-id="ea289-285">The command returns the results in alphabetical order based on the values of the Name column.</span></span> <span data-ttu-id="ea289-286">如果沒有記錄符合價格準則，則此命令會傳回空集合。</span><span class="sxs-lookup"><span data-stu-id="ea289-286">If no records match the price criteria, the command returns an empty set.</span></span>
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> <span data-ttu-id="ea289-287">此命令會將新的記錄，到*產品*設 1.99 名稱 欄，"Croissant"和"A 穩定的天堂"，描述資料行價格的資料表。</span><span class="sxs-lookup"><span data-stu-id="ea289-287">This command inserts a new record into the *Product* table, setting the Name column to "Croissant", the Description column to "A flaky delight", and the price to 1.99.</span></span>
> 
> <span data-ttu-id="ea289-288">請注意當您正在指定非數值，該值括在單引號 （沒有引號，和在 C#）。</span><span class="sxs-lookup"><span data-stu-id="ea289-288">Notice that when you're specifying a non-numeric value, the value is enclosed in single quotation marks (not double quotation marks, as in C#).</span></span> <span data-ttu-id="ea289-289">您使用這些周圍文字或日期值，但不是數字周圍的引號。</span><span class="sxs-lookup"><span data-stu-id="ea289-289">You use these quotation marks around text or date values, but not around numbers.</span></span>
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> <span data-ttu-id="ea289-290">此命令會刪除記錄檔中的*產品*其到期的日期資料行是早於 2008 年 1 月 1 日的資料表。</span><span class="sxs-lookup"><span data-stu-id="ea289-290">This command deletes records in the *Product* table whose expiration date column is earlier than January 1, 2008.</span></span> <span data-ttu-id="ea289-291">(此命令假設*產品*資料表中有這類資料行，當然。)此處以 MM/DD/YYYY 格式輸入日期，但它應該用於您的地區設定的格式輸入。</span><span class="sxs-lookup"><span data-stu-id="ea289-291">(The command assumes that the *Product* table has such a column, of course.) The date is entered here in MM/DD/YYYY format, but it should be entered in the format that's used for your locale.</span></span>
> 
> <span data-ttu-id="ea289-292">`Insert`和`Delete`命令沒有傳回結果集。</span><span class="sxs-lookup"><span data-stu-id="ea289-292">The `Insert` and `Delete` commands don't return result sets.</span></span> <span data-ttu-id="ea289-293">相反地，它們會傳回數字，會告訴您命令影響的多少筆記錄。</span><span class="sxs-lookup"><span data-stu-id="ea289-293">Instead, they return a number that tells you how many records were affected by the command.</span></span>
> 
> <span data-ttu-id="ea289-294">某些這些作業 （例如插入及刪除記錄），要求作業的程序中必須有適當的權限的資料庫。</span><span class="sxs-lookup"><span data-stu-id="ea289-294">For some of these operations (like inserting and deleting records), the process that's requesting the operation has to have appropriate permissions in the database.</span></span> <span data-ttu-id="ea289-295">這就是為什麼您通常需要提供使用者名稱和密碼，當您連接到資料庫的生產資料庫。</span><span class="sxs-lookup"><span data-stu-id="ea289-295">That's why for production databases you often have to supply a user name and password when you connect to the database.</span></span>
> 
> <span data-ttu-id="ea289-296">有許多 SQL 命令，但它們都遵循類似您在這裡看到的命令模式。</span><span class="sxs-lookup"><span data-stu-id="ea289-296">There are dozens of SQL commands, but they all follow a pattern like the commands you see here.</span></span> <span data-ttu-id="ea289-297">您可以使用 SQL 命令來建立資料庫資料表、 計算資料表中的記錄數目，計算價格和執行許多更多的作業。</span><span class="sxs-lookup"><span data-stu-id="ea289-297">You can use SQL commands to create database tables, count the number of records in a table, calculate prices, and perform many more operations.</span></span>


### <a name="adding-markup-to-display-the-data"></a><span data-ttu-id="ea289-298">加入標記，以顯示資料</span><span class="sxs-lookup"><span data-stu-id="ea289-298">Adding Markup to Display the Data</span></span>

<span data-ttu-id="ea289-299">內部`<head>`項目，設定內容`<title>`"影片"項目：</span><span class="sxs-lookup"><span data-stu-id="ea289-299">Inside the `<head>` element, set contents of the `<title>` element to "Movies":</span></span>

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

<span data-ttu-id="ea289-300">內部`<body>`項目頁面上，加入下列：</span><span class="sxs-lookup"><span data-stu-id="ea289-300">Inside the `<body>` element of the page, add the following:</span></span>

[!code-html[Main](displaying-data/samples/sample3.html)]

<span data-ttu-id="ea289-301">就這麼容易！</span><span class="sxs-lookup"><span data-stu-id="ea289-301">That's it.</span></span> <span data-ttu-id="ea289-302">`grid`變數是在您建立時的值`WebGrid`先前的程式碼中的物件。</span><span class="sxs-lookup"><span data-stu-id="ea289-302">The `grid` variable is the value you created when you created the `WebGrid` object in code earlier.</span></span>

<span data-ttu-id="ea289-303">在 WebMatrix 樹狀結構檢視中，以滑鼠右鍵按一下 [] 頁面上，選取**瀏覽器中啟動**。</span><span class="sxs-lookup"><span data-stu-id="ea289-303">In the WebMatrix tree view, right-click the page and select **Launch in browser**.</span></span> <span data-ttu-id="ea289-304">您會看到此頁面類似：</span><span class="sxs-lookup"><span data-stu-id="ea289-304">You'll see something like this page:</span></span>

![電影資料表中的預設 WebGrid 協助程式輸出](displaying-data/_static/image18.png)

<span data-ttu-id="ea289-306">按一下 排序依據該資料行的資料行標題連結。</span><span class="sxs-lookup"><span data-stu-id="ea289-306">Click a column heading link to sort by that column.</span></span> <span data-ttu-id="ea289-307">能夠按一下標題來排序是內建功能**WebGrid**協助程式。</span><span class="sxs-lookup"><span data-stu-id="ea289-307">Being able to sort by clicking a heading is a feature that's built into the **WebGrid** helper.</span></span>

<span data-ttu-id="ea289-308">`GetHtml`方法，如其名所示，會產生標記，顯示的資料。</span><span class="sxs-lookup"><span data-stu-id="ea289-308">The `GetHtml` method, as its name suggests, generates markup that displays the data.</span></span> <span data-ttu-id="ea289-309">根據預設，`GetHtml`方法會產生 HTML`<table>`項目。</span><span class="sxs-lookup"><span data-stu-id="ea289-309">By default, the `GetHtml` method generates an HTML `<table>` element.</span></span> <span data-ttu-id="ea289-310">（如果您想，您可以確認呈現藉由查看網頁瀏覽器中的來源。）</span><span class="sxs-lookup"><span data-stu-id="ea289-310">(If you want, you can verify the rendering by looking at the source of the page in the browser.)</span></span>

## <a name="modifying-the-look-of-the-grid"></a><span data-ttu-id="ea289-311">修改在格線的外觀</span><span class="sxs-lookup"><span data-stu-id="ea289-311">Modifying the Look of the Grid</span></span>

<span data-ttu-id="ea289-312">使用`WebGrid`helper 像就很容易，但顯示的結果為純文字。</span><span class="sxs-lookup"><span data-stu-id="ea289-312">Using the `WebGrid` helper like you just did is easy, but the resulting display is plain.</span></span> <span data-ttu-id="ea289-313">`WebGrid`協助專家擁有各式各樣的選項可讓您控制資料的顯示方式。</span><span class="sxs-lookup"><span data-stu-id="ea289-313">The `WebGrid` helper has all sorts of options that let you control how the data is displayed.</span></span> <span data-ttu-id="ea289-314">還有瀏覽在本教學課程中有太多，但是這個區段可讓您了解這些選項的部分。</span><span class="sxs-lookup"><span data-stu-id="ea289-314">There are far too many to explore in this tutorial, but this section will give you an idea of some of those options.</span></span> <span data-ttu-id="ea289-315">將在這一系列之後的教學課程中涵蓋的一些其他選項。</span><span class="sxs-lookup"><span data-stu-id="ea289-315">A few additional options will be covered in later tutorials in this series.</span></span>

### <a name="specifying-individual-columns-to-display"></a><span data-ttu-id="ea289-316">指定要顯示的個別資料行</span><span class="sxs-lookup"><span data-stu-id="ea289-316">Specifying Individual Columns to Display</span></span>

<span data-ttu-id="ea289-317">若要開始，您可以指定您想要顯示特定資料行。</span><span class="sxs-lookup"><span data-stu-id="ea289-317">To start, you can specify that you want to display only certain columns.</span></span> <span data-ttu-id="ea289-318">根據預設，如您所見，此方格會顯示所有的四個中的資料行*電影*資料表。</span><span class="sxs-lookup"><span data-stu-id="ea289-318">By default, as you've seen, the grid shows all four of the columns from the *Movies* table.</span></span>

<span data-ttu-id="ea289-319">在*Movies.cshtml*檔案中，取代`@grid.GetHtml()`您剛才加入下列標記：</span><span class="sxs-lookup"><span data-stu-id="ea289-319">In the *Movies.cshtml* file, replace the `@grid.GetHtml()` markup that you just added with the following:</span></span>

[!code-css[Main](displaying-data/samples/sample4.css)]

<span data-ttu-id="ea289-320">若要告知協助專家来顯示的資料行，包括`columns`參數`GetHtml`方法並傳入資料行的集合。</span><span class="sxs-lookup"><span data-stu-id="ea289-320">To tell the helper which columns to display, you include a `columns` parameter for the `GetHtml` method and pass in a collection of columns.</span></span> <span data-ttu-id="ea289-321">在集合中，您可以指定要包含的每個資料行。</span><span class="sxs-lookup"><span data-stu-id="ea289-321">In the collection, you specify each column to include.</span></span> <span data-ttu-id="ea289-322">您指定的個別資料行加入顯示`grid.Column`物件，並傳入您想要的資料行。</span><span class="sxs-lookup"><span data-stu-id="ea289-322">You specify an individual column to display by including a `grid.Column` object, and pass in the name of the data column you want.</span></span> <span data-ttu-id="ea289-323">(這些資料行必須包含在 SQL 查詢結果 —`WebGrid`協助程式無法顯示不查詢所傳回的資料行。)</span><span class="sxs-lookup"><span data-stu-id="ea289-323">(These columns must be included in the SQL query results — the `WebGrid` helper cannot display columns that were not returned by the query.)</span></span>

<span data-ttu-id="ea289-324">啟動*Movies.cshtml*和同樣地，此時您收到類似下列的其中一個顯示在瀏覽器中的頁面 （請注意，會顯示沒有識別碼資料行）：</span><span class="sxs-lookup"><span data-stu-id="ea289-324">Launch the *Movies.cshtml* page in the browser again, and this time you get a display like the following one (notice that no ID column is displayed):</span></span>

![WebGrid 顯示選取的資料行](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a><span data-ttu-id="ea289-326">變更格線的外觀</span><span class="sxs-lookup"><span data-stu-id="ea289-326">Changing the Look of the Grid</span></span>

<span data-ttu-id="ea289-327">有不少顯示資料行，其中有些會探索在之後的教學課程，在此集合中的更多選項。</span><span class="sxs-lookup"><span data-stu-id="ea289-327">There are quite a few more options for displaying columns, some of which will be explored in later tutorials in this set.</span></span> <span data-ttu-id="ea289-328">現在，本節將介紹您可以在其中設定樣式整個方格的方式。</span><span class="sxs-lookup"><span data-stu-id="ea289-328">For now, this section will introduce you to ways in which you can style the grid as a whole.</span></span>

<span data-ttu-id="ea289-329">內部`<head>` 頁面上，只在關閉前一節`</head>`標記中，加入下列`<style>`項目：</span><span class="sxs-lookup"><span data-stu-id="ea289-329">Inside the `<head>` section of the page, just before the closing `</head>` tag, add the following `<style>` element:</span></span>

[!code-css[Main](displaying-data/samples/sample5.css)]

<span data-ttu-id="ea289-330">這個 CSS 標記定義名為類別`grid`， `head`，依此類推。</span><span class="sxs-lookup"><span data-stu-id="ea289-330">This CSS markup defines classes named `grid`, `head`, and so on.</span></span> <span data-ttu-id="ea289-331">您也可以將這些樣式定義放在個別 *.css*檔案並將其連結的頁面。</span><span class="sxs-lookup"><span data-stu-id="ea289-331">You could also put these style definitions in a separate *.css* file and link that to the page.</span></span> <span data-ttu-id="ea289-332">（事實上，您將會執行，稍後在此教學課程的集合中。）但為了方便項目在此教學課程，請在相同的頁面，顯示的資料。</span><span class="sxs-lookup"><span data-stu-id="ea289-332">(In fact, you'll do that later in this tutorial set.) But to make things easy for this tutorial, they're inside the same page that displays the data.</span></span>

<span data-ttu-id="ea289-333">現在您可以取得`WebGrid`協助專家使用這些樣式類別。</span><span class="sxs-lookup"><span data-stu-id="ea289-333">Now you can get the `WebGrid` helper to use these style classes.</span></span> <span data-ttu-id="ea289-334">協助專家擁有許多屬性 (例如， `tableStyle`) 針對這個用途 — 您將 CSS 樣式類別名稱指派給它們，而且該類別名稱呈現標記協助程式 」 所呈現的一部分。</span><span class="sxs-lookup"><span data-stu-id="ea289-334">The helper has a number of properties (for example, `tableStyle`) for just this purpose — you assign a CSS style class name to them, and that class name is rendered as part of the markup that's rendered by the helper.</span></span>

<span data-ttu-id="ea289-335">變更`grid.GetHtml`標記，讓它看起來像此程式碼：</span><span class="sxs-lookup"><span data-stu-id="ea289-335">Change the `grid.GetHtml` markup so that it now looks like this code:</span></span>

[!code-css[Main](displaying-data/samples/sample6.css)]

<span data-ttu-id="ea289-336">這裡的新是您已新增的`tableStyle`， `headerStyle`，和`alternatingRowStyle`參數`GetHtml`方法。</span><span class="sxs-lookup"><span data-stu-id="ea289-336">What's new here is that you've added `tableStyle`, `headerStyle`, and `alternatingRowStyle` parameters to the `GetHtml` method.</span></span> <span data-ttu-id="ea289-337">尚未設定這些參數加入一點的 CSS 樣式的名稱。</span><span class="sxs-lookup"><span data-stu-id="ea289-337">These parameters have been set to the names of the CSS styles that you added a moment ago.</span></span>

<span data-ttu-id="ea289-338">執行網頁，以及這次您看見一個方格，看起來較單純比之前：</span><span class="sxs-lookup"><span data-stu-id="ea289-338">Run the page, and this time you see a grid that looks much less plain than before:</span></span>

![WebGrid 顯示，其中包含參數設定到 CSS 類別名稱](displaying-data/_static/image20.png)

<span data-ttu-id="ea289-340">若要查看哪些`GetHtml`產生的方法，您可以查看瀏覽器中網頁的來源。</span><span class="sxs-lookup"><span data-stu-id="ea289-340">To see what the `GetHtml` method generated, you can look at the source of the page in the browser.</span></span> <span data-ttu-id="ea289-341">我們將不會移到這裡，詳細資料，但是很重要的一點是藉由指定的參數如`tableStyle`，造成方格產生 HTML 標記，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ea289-341">We won't go into detail here, but the important point is that by specifying parameters like `tableStyle`, you caused the grid to generate HTML tags like the following:</span></span>

`<table class="grid">`

<span data-ttu-id="ea289-342">`<table>`標記已`class`屬性加入至它所參考的 CSS 規則，您先前加入的其中一個。</span><span class="sxs-lookup"><span data-stu-id="ea289-342">The `<table>` tag has had a `class` attribute added to it that references one of the CSS rules that you added earlier.</span></span> <span data-ttu-id="ea289-343">此程式碼會示範基本模式&mdash;不同參數`GetHtml`方法，然後產生標記的方法可讓您參考 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="ea289-343">This code shows you the basic pattern &mdash; different parameters for the `GetHtml` method let you reference CSS classes that the method then generates along with the markup.</span></span> <span data-ttu-id="ea289-344">您用來做什麼的 CSS 類別是由您決定。</span><span class="sxs-lookup"><span data-stu-id="ea289-344">What you do with the CSS classes is up to you.</span></span>

## <a name="adding-paging"></a><span data-ttu-id="ea289-345">加入分頁</span><span class="sxs-lookup"><span data-stu-id="ea289-345">Adding Paging</span></span>

<span data-ttu-id="ea289-346">在此教學課程的最後一個工作，您會加入至方格中的分頁。</span><span class="sxs-lookup"><span data-stu-id="ea289-346">As the last task for this tutorial, you'll add paging to the grid.</span></span> <span data-ttu-id="ea289-347">以滑鼠右鍵現在沒問題，一次顯示您的電影。</span><span class="sxs-lookup"><span data-stu-id="ea289-347">Right now it's no problem to display all your movies at once.</span></span> <span data-ttu-id="ea289-348">但如果您新增的數百個影片，頁面顯示會很長。</span><span class="sxs-lookup"><span data-stu-id="ea289-348">But if you added hundreds of movies, the page display would get long.</span></span>

<span data-ttu-id="ea289-349">網頁程式碼中變更的行，建立`WebGrid`下列程式碼的物件：</span><span class="sxs-lookup"><span data-stu-id="ea289-349">In the page code, change the line that creates the `WebGrid` object to the following code:</span></span>

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

<span data-ttu-id="ea289-350">從之前的唯一差異是您已新增的`rowsPerPage`設為 3 的參數。</span><span class="sxs-lookup"><span data-stu-id="ea289-350">The only difference from before is that you've added a `rowsPerPage` parameter that's set to 3.</span></span>

<span data-ttu-id="ea289-351">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="ea289-351">Run the page.</span></span> <span data-ttu-id="ea289-352">此方格會在建立時間，再加上瀏覽連結，可讓您瀏覽您的資料庫中的電影顯示 3 個資料列：</span><span class="sxs-lookup"><span data-stu-id="ea289-352">The grid displays 3 rows at a time, plus navigation links that let you page through the movies in your database:</span></span>

![使用分頁 WebGrid 顯示](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a><span data-ttu-id="ea289-354">接下來</span><span class="sxs-lookup"><span data-stu-id="ea289-354">Coming Up Next</span></span>

<span data-ttu-id="ea289-355">在下一個教學課程中，您將學習如何使用 Razor 和 C# 程式碼取得使用者輸入的表單。</span><span class="sxs-lookup"><span data-stu-id="ea289-355">In the next tutorial, you'll learn how to use Razor and C# code to get user input in a form.</span></span> <span data-ttu-id="ea289-356">因此您可以找到影片依標題或音樂類型，您會將在搜尋方塊加入至 [影片] 頁面中。</span><span class="sxs-lookup"><span data-stu-id="ea289-356">You'll add a search box to the Movies page so that you can find movies by title or genre.</span></span>

## <a name="complete-listing-for-movies-page"></a><span data-ttu-id="ea289-357">影片頁面的完整清單</span><span class="sxs-lookup"><span data-stu-id="ea289-357">Complete Listing for Movies Page</span></span>

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="ea289-358">其他資源</span><span class="sxs-lookup"><span data-stu-id="ea289-358">Additional Resources</span></span>

- [<span data-ttu-id="ea289-359">使用 Razor 語法的 ASP.NET Web 程式設計簡介</span><span class="sxs-lookup"><span data-stu-id="ea289-359">Introduction to ASP.NET Web Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> <span data-ttu-id="ea289-360">[上一頁](intro-to-web-pages-programming.md)
> [下一頁](form-basics.md)</span><span class="sxs-lookup"><span data-stu-id="ea289-360">[Previous](intro-to-web-pages-programming.md)
[Next](form-basics.md)</span></span>
