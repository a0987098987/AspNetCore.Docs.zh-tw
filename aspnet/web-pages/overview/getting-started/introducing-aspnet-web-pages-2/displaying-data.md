---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: 簡介 ASP.NET Web Pages-顯示資料 |Microsoft Docs
author: tfitzmac
description: 本教學課程會示範如何在 WebMatrix 中建立資料庫，以及如何在網頁中顯示資料庫的資料，當您使用 ASP.NET Web Pages (Razor)。 它會假設 y...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: eeceb08e3aa281c45a2cfe35af4f23b76a5b1d25
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834293"
---
<a name="introducing-aspnet-web-pages---displaying-data"></a><span data-ttu-id="4ad82-104">ASP.NET Web Pages 簡介-顯示資料</span><span class="sxs-lookup"><span data-stu-id="4ad82-104">Introducing ASP.NET Web Pages - Displaying Data</span></span>
====================
<span data-ttu-id="4ad82-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4ad82-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4ad82-106">本教學課程會示範如何在 WebMatrix 中建立資料庫，以及如何在網頁中顯示資料庫的資料，當您使用 ASP.NET Web Pages (Razor)。</span><span class="sxs-lookup"><span data-stu-id="4ad82-106">This tutorial shows you how to create a database in WebMatrix and how to display database data in a page when you use ASP.NET Web Pages (Razor).</span></span> <span data-ttu-id="4ad82-107">它假設您已完成透過數列[ASP.NET Web Pages 程式設計簡介](../introducing-razor-syntax-c.md)。</span><span class="sxs-lookup"><span data-stu-id="4ad82-107">It assumes you have completed the series through [Introduction to ASP.NET Web Pages Programming](../introducing-razor-syntax-c.md).</span></span>
> 
> <span data-ttu-id="4ad82-108">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="4ad82-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="4ad82-109">如何使用 WebMatrix 工具來建立資料庫和資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="4ad82-109">How to use WebMatrix tools to create a database and database tables.</span></span>
> - <span data-ttu-id="4ad82-110">如何使用 WebMatrix 工具將資料加入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="4ad82-110">How to use WebMatrix tools to add data to a database.</span></span>
> - <span data-ttu-id="4ad82-111">如何顯示網頁上的資料庫中的資料。</span><span class="sxs-lookup"><span data-stu-id="4ad82-111">How to display data from the database on a page.</span></span>
> - <span data-ttu-id="4ad82-112">如何將 ASP.NET Web Pages 中執行的 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="4ad82-112">How to run SQL commands in ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="4ad82-113">如何自訂`WebGrid`helper 來變更資料顯示，並新增分頁和排序。</span><span class="sxs-lookup"><span data-stu-id="4ad82-113">How to customize the `WebGrid` helper to change the data display and to add paging and sorting.</span></span>
>   
> 
> <span data-ttu-id="4ad82-114">功能/技術討論：</span><span class="sxs-lookup"><span data-stu-id="4ad82-114">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="4ad82-115">WebMatrix 資料庫工具。</span><span class="sxs-lookup"><span data-stu-id="4ad82-115">WebMatrix database tools.</span></span>
> - <span data-ttu-id="4ad82-116">`WebGrid` 協助程式。</span><span class="sxs-lookup"><span data-stu-id="4ad82-116">`WebGrid` helper.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="4ad82-117">您將建置</span><span class="sxs-lookup"><span data-stu-id="4ad82-117">What You'll Build</span></span>

<span data-ttu-id="4ad82-118">在上一個教學課程中，已向您介紹以 「 ASP.NET Web Pages (*.cshtml*檔案)、 Razor 語法的基本概念和協助程式。</span><span class="sxs-lookup"><span data-stu-id="4ad82-118">In the previous tutorial, you were introduced to ASP.NET Web Pages (*.cshtml* files), to the basics of Razor syntax, and to helpers.</span></span> <span data-ttu-id="4ad82-119">在本教學課程中，您會開始建立您將用於其餘的一系列的實際 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4ad82-119">In this tutorial, you'll begin creating the actual web application that you'll use for the rest of the series.</span></span> <span data-ttu-id="4ad82-120">應用程式是簡單的電影應用程式，可讓您檢視、 新增、 變更和刪除電影的資訊。</span><span class="sxs-lookup"><span data-stu-id="4ad82-120">The app is a simple movie application that lets you view, add, change, and delete information about movies.</span></span>

<span data-ttu-id="4ad82-121">當您完成本教學課程中時，您將能夠檢視此頁面看起來的電影清單：</span><span class="sxs-lookup"><span data-stu-id="4ad82-121">When you're done with this tutorial, you'll be able to view a movie listing that looks like this page:</span></span>

![包含參數的 WebGrid 顯示設 CSS 類別名稱](displaying-data/_static/image1.png)

<span data-ttu-id="4ad82-123">但是，若要開始，您必須建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="4ad82-123">But to begin, you have to create a database.</span></span>

## <a name="a-very-brief-introduction-to-databases"></a><span data-ttu-id="4ad82-124">資料庫非常簡短的簡介</span><span class="sxs-lookup"><span data-stu-id="4ad82-124">A Very Brief Introduction to Databases</span></span>

<span data-ttu-id="4ad82-125">本教學課程會提供資料庫只送入的介紹。</span><span class="sxs-lookup"><span data-stu-id="4ad82-125">This tutorial will provide only the briefest introduction to databases.</span></span> <span data-ttu-id="4ad82-126">如果您有資料庫的經驗，您可以略過本節將簡短。</span><span class="sxs-lookup"><span data-stu-id="4ad82-126">If you have database experience, you can skip this short section.</span></span>

<span data-ttu-id="4ad82-127">資料庫包含一或多個資料表包含資訊&mdash;，例如客戶、 訂單及廠商，或針對學生、 教師、 類別和成績的資料表。</span><span class="sxs-lookup"><span data-stu-id="4ad82-127">A database contains one or more tables that contain information &mdash; for example, tables for customers, orders, and vendors, or for students, teachers, classes, and grades.</span></span> <span data-ttu-id="4ad82-128">結構，資料庫資料表，就是試算表等。</span><span class="sxs-lookup"><span data-stu-id="4ad82-128">Structurally, a database table is like a spreadsheet.</span></span> <span data-ttu-id="4ad82-129">想像一下一個典型的通訊錄。</span><span class="sxs-lookup"><span data-stu-id="4ad82-129">Imagine a typical address book.</span></span> <span data-ttu-id="4ad82-130">通訊錄中的每個項目 (也就是為每個人) 有幾個部分的資訊，例如名字、 姓氏、 地址、 電子郵件地址和電話號碼。</span><span class="sxs-lookup"><span data-stu-id="4ad82-130">For each entry in the address book (that is, for each person) you have several pieces of information such as first name, last name, address, email address, and phone number.</span></span>

![以簡單的方格顯示的範例資料庫資料表](displaying-data/_static/image2.png)

<span data-ttu-id="4ad82-132">(資料列有時稱為*記錄*，和資料行有時稱為*欄位*。)</span><span class="sxs-lookup"><span data-stu-id="4ad82-132">(Rows are sometimes referred to as *records*, and columns are sometimes referred to as *fields*.)</span></span>

<span data-ttu-id="4ad82-133">針對大部分的資料庫資料表，資料表必須有一個資料行包含唯一的值，例如客戶編號、 客戶編號，等等。</span><span class="sxs-lookup"><span data-stu-id="4ad82-133">For most database tables, the table has to have a column that contains a unique value, like a customer number, account number, and so on.</span></span> <span data-ttu-id="4ad82-134">這個值就所謂的資料表*主索引鍵*，並使用它來識別資料表中的每個資料列。</span><span class="sxs-lookup"><span data-stu-id="4ad82-134">This value is known as the table's *primary key*, and you use it to identify each row in the table.</span></span> <span data-ttu-id="4ad82-135">在範例中，識別碼資料行是主索引鍵，如先前範例所示的通訊錄。</span><span class="sxs-lookup"><span data-stu-id="4ad82-135">In the example, the ID column is the primary key for the address book shown in the previous example.</span></span>

<span data-ttu-id="4ad82-136">大部分您在 web 應用程式中執行的工作包含讀取移出資料庫的資訊，以及顯示在頁面上。</span><span class="sxs-lookup"><span data-stu-id="4ad82-136">Much of the work you do in web applications consists of reading information out of the database and displaying it on a page.</span></span> <span data-ttu-id="4ad82-137">您通常也會向使用者收集資訊，並將它新增至資料庫，或您要修改已在資料庫中的記錄。</span><span class="sxs-lookup"><span data-stu-id="4ad82-137">You'll also often gather information from users and add it to a database, or you'll modify records that are already in the database.</span></span> <span data-ttu-id="4ad82-138">（我們將涵蓋所有這些作業，在本教學課程組的過程中。）</span><span class="sxs-lookup"><span data-stu-id="4ad82-138">(We'll cover all of these operations in the course of this tutorial set.)</span></span>

<span data-ttu-id="4ad82-139">資料庫工作可能會大幅增長很複雜，而且可能需要專業的知識。</span><span class="sxs-lookup"><span data-stu-id="4ad82-139">Database work can be enormously complex and can require specialized knowledge.</span></span> <span data-ttu-id="4ad82-140">本教學課程組，不過，您必須了解僅基本概念，可將所有會說明當您瀏。</span><span class="sxs-lookup"><span data-stu-id="4ad82-140">For this tutorial set, though, you have to understand only basic concepts, which will all be explained as you go.</span></span>

## <a name="creating-a-database"></a><span data-ttu-id="4ad82-141">建立資料庫</span><span class="sxs-lookup"><span data-stu-id="4ad82-141">Creating a Database</span></span>

<span data-ttu-id="4ad82-142">WebMatrix 包含工具，可讓您輕鬆建立資料庫，且在資料庫中建立資料表。</span><span class="sxs-lookup"><span data-stu-id="4ad82-142">WebMatrix includes tools that make it easy to create a database and to create tables in the database.</span></span> <span data-ttu-id="4ad82-143">(資料庫的結構指資料庫*結構描述*。)本教學課程組，您將建立有只有一個資料表中的資料庫，&mdash;電影。</span><span class="sxs-lookup"><span data-stu-id="4ad82-143">(The structure of a database is referred to as the database's *schema*.) For this tutorial set, you'll create a database that has only one table in it &mdash; Movies.</span></span>

<span data-ttu-id="4ad82-144">如果您還沒有這麼做，請開啟 WebMatrix，然後開啟您在上一個教學課程中建立 WebPagesMovies 站台。</span><span class="sxs-lookup"><span data-stu-id="4ad82-144">Open WebMatrix if you haven't already done so, and open the WebPagesMovies site that you created in the previous tutorial.</span></span>

<span data-ttu-id="4ad82-145">在左窗格中，按一下**資料庫**工作區。</span><span class="sxs-lookup"><span data-stu-id="4ad82-145">In the left pane, click the **Database** workspace.</span></span>

![WebMatrix 資料庫工作區 索引標籤](displaying-data/_static/image3.png)

<span data-ttu-id="4ad82-147">功能區的變更，以顯示與資料庫相關的工作。</span><span class="sxs-lookup"><span data-stu-id="4ad82-147">The ribbon changes to show database-related tasks.</span></span> <span data-ttu-id="4ad82-148">在 功能區中，按一下**新的資料庫**。</span><span class="sxs-lookup"><span data-stu-id="4ad82-148">In the ribbon, click **New Database**.</span></span>

![WebMatrix 功能區中的 [新增資料庫] 按鈕](displaying-data/_static/image4.png)

<span data-ttu-id="4ad82-150">WebMatrix 會建立 SQL Server CE 的資料庫 ( *.sdf*檔案)，具有相同的名稱作為站台&mdash; *WebPagesMovies.sdf*。</span><span class="sxs-lookup"><span data-stu-id="4ad82-150">WebMatrix creates a SQL Server CE database (an *.sdf* file) that has the same name as your site &mdash; *WebPagesMovies.sdf*.</span></span> <span data-ttu-id="4ad82-151">(您不會執行這項操作，但您可以任何想要的話，重新命名檔案，只要其 *.sdf*延伸模組。)</span><span class="sxs-lookup"><span data-stu-id="4ad82-151">(You won't do this here, but you can rename the file to anything you like, as long as it has an *.sdf* extension.)</span></span>

## <a name="creating-a-table"></a><span data-ttu-id="4ad82-152">建立資料表</span><span class="sxs-lookup"><span data-stu-id="4ad82-152">Creating a Table</span></span>

<span data-ttu-id="4ad82-153">在 功能區中，按一下**新的資料表**。</span><span class="sxs-lookup"><span data-stu-id="4ad82-153">In the ribbon, click **New Table**.</span></span> <span data-ttu-id="4ad82-154">WebMatrix 會在新的索引標籤中開啟資料表設計工具。（如果無法使用新的資料表選項，請確定新的資料庫在左側樹狀檢視中選取。）</span><span class="sxs-lookup"><span data-stu-id="4ad82-154">WebMatrix opens the table designer in a new tab. (If the New Table option isn't available, make sure that the new database is selected in the tree view on the left.)</span></span>

![WebMatrix 功能區中的 '新的資料表' 按鈕](displaying-data/_static/image5.png)

<span data-ttu-id="4ad82-156">在頂端 （浮水印是 「 輸入資料表名稱"） 的 [文字] 方塊中，輸入 「 Movies 」。</span><span class="sxs-lookup"><span data-stu-id="4ad82-156">In the text box at the top (where the watermark says "Enter table name"), enter "Movies".</span></span>

![WebMatrix 資料庫設計工具中輸入資料表名稱](displaying-data/_static/image6.png)

<span data-ttu-id="4ad82-158">[] 窗格下方的資料表名稱是您定義個別的資料行的位置。</span><span class="sxs-lookup"><span data-stu-id="4ad82-158">The pane underneath the table name is where you define individual columns.</span></span> <span data-ttu-id="4ad82-159">針對*電影*資料表，您將在本教學課程中，建立只有少數的資料行：*識別碼*， *Title*，*內容類型*，和*年*.</span><span class="sxs-lookup"><span data-stu-id="4ad82-159">For the *Movies* table in this tutorial, you'll create only a few columns: *ID*, *Title*, *Genre*, and *Year*.</span></span>

<span data-ttu-id="4ad82-160">在 **名稱**方塊中，輸入 「 識別碼 」。</span><span class="sxs-lookup"><span data-stu-id="4ad82-160">In the **Name** box, enter "ID".</span></span> <span data-ttu-id="4ad82-161">輸入的值，就會啟動新的資料行的所有控制項。</span><span class="sxs-lookup"><span data-stu-id="4ad82-161">Entering a value here activates all the controls for the new column.</span></span>

<span data-ttu-id="4ad82-162">Tab 鍵移至**資料型別**清單，然後選擇**int**。這個值會指定識別碼資料行，將包含整數 （值） 的資料。</span><span class="sxs-lookup"><span data-stu-id="4ad82-162">Tab to the **Data Type** list and choose **int**. This value specifies that the ID column will contain integer (number) data.</span></span>

> [!NOTE]
> <span data-ttu-id="4ad82-163">我們不會呼叫它任何這裡 （多），但您可以使用標準 Windows 鍵盤手勢來瀏覽此方格中。</span><span class="sxs-lookup"><span data-stu-id="4ad82-163">We won't call it out any more here (much), but you can use standard Windows keyboard gestures to navigate in this grid.</span></span> <span data-ttu-id="4ad82-164">比方說，您可以使用 tab 鍵的欄位之間，您可能只要開始輸入，就可以在清單中，選取項目等等。</span><span class="sxs-lookup"><span data-stu-id="4ad82-164">For example, you can tab between fields, you can just start typing in order to select an item in a list, and so on.</span></span>


<span data-ttu-id="4ad82-165">Tab 跳過**預設值**方塊 （也就是保留為空白）。</span><span class="sxs-lookup"><span data-stu-id="4ad82-165">Tab past the **Default Value** box (that is, leave it blank).</span></span> <span data-ttu-id="4ad82-166">Tab 鍵移至**是主索引鍵**核取方塊，然後選取它。</span><span class="sxs-lookup"><span data-stu-id="4ad82-166">Tab to the **Is Primary Key** check box and select it.</span></span> <span data-ttu-id="4ad82-167">此選項會告知資料庫之*識別碼*資料行會包含識別個別資料列的資料。</span><span class="sxs-lookup"><span data-stu-id="4ad82-167">This option tells the database that the *ID* column will contain the data that identifies individual rows.</span></span> <span data-ttu-id="4ad82-168">（也就是每個資料列會可供您尋找該資料列識別碼資料行中有唯一的值）。</span><span class="sxs-lookup"><span data-stu-id="4ad82-168">(That is, each row will have a unique value in the ID column that you can use to find that row.)</span></span>

<span data-ttu-id="4ad82-169">選擇**是識別**選項。</span><span class="sxs-lookup"><span data-stu-id="4ad82-169">Choose the **Is Identity** option.</span></span> <span data-ttu-id="4ad82-170">此選項會告訴資料庫應該會自動產生下一個循序號碼，每個新的資料列。</span><span class="sxs-lookup"><span data-stu-id="4ad82-170">This option tells the database that it should automatically generate the next sequential number for each new row.</span></span> <span data-ttu-id="4ad82-171">(**是身分識別**只有當您也選取了選項適用於**是主索引鍵**選項。)</span><span class="sxs-lookup"><span data-stu-id="4ad82-171">(The **Is Identity** option works only if you've also selected the **Is Primary Key** option.)</span></span>

<span data-ttu-id="4ad82-172">在下一步 的 格線 列中，按一下或按兩次以完成目前的資料列的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="4ad82-172">Click in the next grid row, or press Tab twice to finish the current row.</span></span> <span data-ttu-id="4ad82-173">其中一個軌跡儲存目前的資料列，並啟動下一個。</span><span class="sxs-lookup"><span data-stu-id="4ad82-173">Either gesture saves the current row and starts the next one.</span></span> <span data-ttu-id="4ad82-174">請注意，**預設值**資料行現在說**Null**。</span><span class="sxs-lookup"><span data-stu-id="4ad82-174">Notice that the **Default Value** column now says **Null**.</span></span> <span data-ttu-id="4ad82-175">（預設值是 null 的預設值，就是所謂。）</span><span class="sxs-lookup"><span data-stu-id="4ad82-175">(Null is the default value for the default value, so to speak.)</span></span>

<span data-ttu-id="4ad82-176">當您完成定義新**識別碼** 欄中，設計工具看起來像下圖：</span><span class="sxs-lookup"><span data-stu-id="4ad82-176">When you've finished defining the new **ID** column, the designer will look like this illustration:</span></span>

![在定義電影資料表的識別碼資料行之後的 WebMatrix 資料庫設計工具](displaying-data/_static/image7.png)

<span data-ttu-id="4ad82-178">若要建立下一個資料行，按一下 在方塊中，在**名稱**資料行。</span><span class="sxs-lookup"><span data-stu-id="4ad82-178">To create the next column, click in the box in the **Name** column.</span></span> <span data-ttu-id="4ad82-179">資料行中輸入"Title"，然後選取**nvarchar** for**資料型別**值。</span><span class="sxs-lookup"><span data-stu-id="4ad82-179">Enter "Title" for the column and then select **nvarchar** for the **Data Type** value.</span></span> <span data-ttu-id="4ad82-180">"Var"一部分**nvarchar**會告知資料庫這個資料行的資料會是大小可能會不同記錄間的字串。</span><span class="sxs-lookup"><span data-stu-id="4ad82-180">The "var" part of **nvarchar** tells the database that the data for this column will be a string whose size might vary from record to record.</span></span> <span data-ttu-id="4ad82-181">("N 名"的前置詞表示 「 國家 」，表示欄位可以保存字元資料的任何字母，或寫入系統 — 亦即，欄位就會保留 Unicode 資料。)</span><span class="sxs-lookup"><span data-stu-id="4ad82-181">(The "n" prefix represents "national," which indicates that the field can hold character data for any alphabet or writing system — that is, the field holds Unicode data.)</span></span>

<span data-ttu-id="4ad82-182">當您選擇**nvarchar**，另一個方塊會顯示您可以在其中輸入欄位的最大長度。</span><span class="sxs-lookup"><span data-stu-id="4ad82-182">When you choose **nvarchar**, another box appears where you can enter the maximum length for the field.</span></span> <span data-ttu-id="4ad82-183">輸入 50，假設您將在本教學課程中使用任何電影標題將會超過 50 個字元。</span><span class="sxs-lookup"><span data-stu-id="4ad82-183">Enter 50, on the assumption that no movie title that you'll work with in this tutorial will be longer than 50 characters.</span></span>

<span data-ttu-id="4ad82-184">略過**預設值**並清除**允許 Null**選項。</span><span class="sxs-lookup"><span data-stu-id="4ad82-184">Skip **Default Value** and clear the **Allow Nulls** option.</span></span> <span data-ttu-id="4ad82-185">您不想要允許任何沒有標題的電影要輸入至資料庫的資料庫。</span><span class="sxs-lookup"><span data-stu-id="4ad82-185">You don't want the database to allow any movies to be entered into the database that don't have a title.</span></span>

<span data-ttu-id="4ad82-186">當您完成時，並移至下一個資料列時，設計工具看起來像下圖中：</span><span class="sxs-lookup"><span data-stu-id="4ad82-186">When you're done and move to the next row, the designer looks like this illustration:</span></span>

![在定義電影資料表的標題資料行之後的 WebMatrix 資料庫設計工具](displaying-data/_static/image8.png)

<span data-ttu-id="4ad82-188">重複下列步驟來建立資料行，名為"Genre"，除了長度，將它設定為只是 30。</span><span class="sxs-lookup"><span data-stu-id="4ad82-188">Repeat these steps to create a column named "Genre", except for the length, set it to just 30.</span></span>

<span data-ttu-id="4ad82-189">建立另一個資料行，名為 「 年 」。</span><span class="sxs-lookup"><span data-stu-id="4ad82-189">Create another column named "Year."</span></span> <span data-ttu-id="4ad82-190">針對資料類型中，選擇**nchar** (不**nvarchar**) 並將長度設定為 4。</span><span class="sxs-lookup"><span data-stu-id="4ad82-190">For the data type, choose **nchar** (not **nvarchar**) and set the length to 4.</span></span> <span data-ttu-id="4ad82-191">年中，您要使用 4 位數數字，例如 「 1995"或"2010"，因此您不需要可變大小的資料行。</span><span class="sxs-lookup"><span data-stu-id="4ad82-191">For the year, you're going to use a 4-digit number like "1995" or "2010", so you don't require a variable-sized column.</span></span>

<span data-ttu-id="4ad82-192">以下是完成的設計看起來像：</span><span class="sxs-lookup"><span data-stu-id="4ad82-192">Here's what the finished design looks like:</span></span>

![WebMatrix 資料庫設計工具的所有欄位都定義之電影資料表之後](displaying-data/_static/image9.png)

<span data-ttu-id="4ad82-194">按下 Ctrl + S 或按一下**儲存**中快速存取工具列按鈕。</span><span class="sxs-lookup"><span data-stu-id="4ad82-194">Press Ctrl+S or click the **Save** button in the Quick Access toolbar.</span></span> <span data-ttu-id="4ad82-195">關閉索引標籤，以關閉資料庫設計工具。</span><span class="sxs-lookup"><span data-stu-id="4ad82-195">Close the database designer by closing the tab.</span></span>

## <a name="adding-some-example-data"></a><span data-ttu-id="4ad82-196">新增一些範例資料</span><span class="sxs-lookup"><span data-stu-id="4ad82-196">Adding Some Example Data</span></span>

<span data-ttu-id="4ad82-197">稍後在本教學課程系列中，您將建立的頁面，您可以在其中輸入表單中新的電影。</span><span class="sxs-lookup"><span data-stu-id="4ad82-197">Later in this tutorial series you'll create a page where you can enter new movies in a form.</span></span> <span data-ttu-id="4ad82-198">不過，現在，您可以新增一些您可以在網頁顯示的範例資料。</span><span class="sxs-lookup"><span data-stu-id="4ad82-198">For now, however, you can add some example data that you can then display on a page.</span></span>

<span data-ttu-id="4ad82-199">在 **資料庫**在 WebMatrix 中，請注意，有樹，其中顯示您的工作區 *.sdf*您稍早建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="4ad82-199">In the **Database** workspace in WebMatrix, notice that there's a tree that shows you the *.sdf* file you created earlier.</span></span> <span data-ttu-id="4ad82-200">開啟新的節點 *.sdf*檔案，然後再開啟**資料表**節點。</span><span class="sxs-lookup"><span data-stu-id="4ad82-200">Open the node for your new *.sdf* file, and then open the **Tables** node.</span></span>

![WebMatrix 資料庫工作區，以樹狀目錄中開啟電影資料表](displaying-data/_static/image10.png)

<span data-ttu-id="4ad82-202">以滑鼠右鍵按一下**電影**節點，然後選擇**資料**。</span><span class="sxs-lookup"><span data-stu-id="4ad82-202">Right-click the **Movies** node and then choose **Data**.</span></span> <span data-ttu-id="4ad82-203">WebMatrix 會開啟一個方格，您可以在此輸入資料*電影*資料表：</span><span class="sxs-lookup"><span data-stu-id="4ad82-203">WebMatrix opens a grid where you can enter data for the *Movies* table:</span></span>

![在 WebMatrix 中 （空白） 的資料庫項目格線](displaying-data/_static/image11.png)

<span data-ttu-id="4ad82-205">按一下  **Title**資料行，然後輸入 「 當 Harry 符合 Sally"。</span><span class="sxs-lookup"><span data-stu-id="4ad82-205">Click the **Title** column and enter "When Harry Met Sally".</span></span> <span data-ttu-id="4ad82-206">移至**內容類型**（您可以使用 Tab 鍵） 的資料行，然後輸入 「 浪漫喜劇"。</span><span class="sxs-lookup"><span data-stu-id="4ad82-206">Move to the **Genre** column (you can use the Tab key) and enter "Romantic Comedy".</span></span> <span data-ttu-id="4ad82-207">移至**年份**資料行，然後輸入 「 1989 」:</span><span class="sxs-lookup"><span data-stu-id="4ad82-207">Move to the **Year** column and enter "1989":</span></span>

![在 WebMatrix 中有一筆記錄的資料庫項目格線](displaying-data/_static/image12.png)

<span data-ttu-id="4ad82-209">按 Enter 鍵和 WebMatrix 將儲存新的電影。</span><span class="sxs-lookup"><span data-stu-id="4ad82-209">Press Enter, and WebMatrix saves the new movie.</span></span> <span data-ttu-id="4ad82-210">請注意，**識別碼**填入資料行。</span><span class="sxs-lookup"><span data-stu-id="4ad82-210">Notice that the **ID** column has been filled in.</span></span>

![在 WebMatrix 中自動產生的識別碼與一筆記錄的資料庫項目方格](displaying-data/_static/image13.png)

<span data-ttu-id="4ad82-212">輸入另一部影片 （比方說，「 進入與風力 」，「 劇本 」，「 1939"）。</span><span class="sxs-lookup"><span data-stu-id="4ad82-212">Enter another movie (for example, "Gone with the Wind", "Drama", "1939").</span></span> <span data-ttu-id="4ad82-213">識別碼資料行已重新填入：</span><span class="sxs-lookup"><span data-stu-id="4ad82-213">The ID column is filled in again:</span></span>

![在 WebMatrix 中兩筆記錄與自動產生的識別碼的資料庫項目格線](displaying-data/_static/image14.png)

<span data-ttu-id="4ad82-215">輸入 （比方說，「 Ghostbusters"、"喜劇"） 第三個影片。</span><span class="sxs-lookup"><span data-stu-id="4ad82-215">Enter a third movie (for example, "Ghostbusters", "Comedy").</span></span> <span data-ttu-id="4ad82-216">做一個試驗，離開**年份**資料行保留空白，然後按 Enter。</span><span class="sxs-lookup"><span data-stu-id="4ad82-216">As an experiment, leave the **Year** column blank and then press Enter.</span></span> <span data-ttu-id="4ad82-217">因為您取消選取 **是否允許 null 值**選項，資料庫便會顯示錯誤：</span><span class="sxs-lookup"><span data-stu-id="4ad82-217">Because you unselected the **Allow Nulls** option, the database shows an error:</span></span>

![如果必要的資料行值保留空白，會顯示 「 無效的資料 」 錯誤](displaying-data/_static/image15.png)

<span data-ttu-id="4ad82-219">按一下 **確定**返回並修正的項目 （「 Ghostbusters 「 1984 年度），然後按 Enter 鍵。</span><span class="sxs-lookup"><span data-stu-id="4ad82-219">Click **OK** to go back and fix the entry (the year for "Ghostbusters" is 1984), and then press Enter.</span></span>

<span data-ttu-id="4ad82-220">填寫幾個電影，除非您有 8 左右。</span><span class="sxs-lookup"><span data-stu-id="4ad82-220">Fill in several movies until you have 8 or so.</span></span> <span data-ttu-id="4ad82-221">（輸入 8 可讓您更輕鬆地與更新版本的分頁。</span><span class="sxs-lookup"><span data-stu-id="4ad82-221">(Entering 8 makes it easier to work with paging later.</span></span> <span data-ttu-id="4ad82-222">但如果這太多，請輸入只是現在的一些）。實際的資料並不重要。</span><span class="sxs-lookup"><span data-stu-id="4ad82-222">But if that's too many, enter just a few for now.) The actual data doesn't matter.</span></span>

![在 WebMatrix 中任一個記錄的資料庫項目格線](displaying-data/_static/image16.png)

<span data-ttu-id="4ad82-224">如果您輸入未發生任何錯誤的所有影片，ID 值是連續的。</span><span class="sxs-lookup"><span data-stu-id="4ad82-224">If you entered all the movies without any errors, the ID values are sequential.</span></span> <span data-ttu-id="4ad82-225">如果您嘗試儲存未完成的電影錄製時，可能無法循序的識別碼。</span><span class="sxs-lookup"><span data-stu-id="4ad82-225">If you tried to save an incomplete movie record, the ID numbers might not be sequential.</span></span> <span data-ttu-id="4ad82-226">如果是的話，也沒關係。</span><span class="sxs-lookup"><span data-stu-id="4ad82-226">If so, that's okay.</span></span> <span data-ttu-id="4ad82-227">數字沒有任何固有的意義和唯一重要的是，唯一想*電影*資料表。</span><span class="sxs-lookup"><span data-stu-id="4ad82-227">The numbers don't have any inherent meaning, and the only thing that's important is that they're unique in the *Movies* table.</span></span>

<span data-ttu-id="4ad82-228">關閉包含 「 資料庫設計工具的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="4ad82-228">Close the tab that contains the database designer.</span></span>

<span data-ttu-id="4ad82-229">現在您可以開啟 web 網頁上顯示這項資料。</span><span class="sxs-lookup"><span data-stu-id="4ad82-229">Now you can turn to displaying this data on a web page.</span></span>

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a><span data-ttu-id="4ad82-230">在頁面中顯示資料，使用 WebGrid 協助程式</span><span class="sxs-lookup"><span data-stu-id="4ad82-230">Displaying Data in a Page by Using the WebGrid Helper</span></span>

<span data-ttu-id="4ad82-231">若要顯示資料在頁面中，您要使用`WebGrid`協助程式。</span><span class="sxs-lookup"><span data-stu-id="4ad82-231">To display data in a page, you're going to use the `WebGrid` helper.</span></span> <span data-ttu-id="4ad82-232">此協助程式會產生以方格或資料表 （資料列和資料行） 的顯示。</span><span class="sxs-lookup"><span data-stu-id="4ad82-232">This helper produces a display in a grid or table (rows and columns).</span></span> <span data-ttu-id="4ad82-233">如您所見，我們馬上就能調整的方格格式及其他功能。</span><span class="sxs-lookup"><span data-stu-id="4ad82-233">As you'll see, you'll be able refine the grid with formatting and other features.</span></span>

<span data-ttu-id="4ad82-234">若要執行的方格，您必須撰寫幾行程式碼。</span><span class="sxs-lookup"><span data-stu-id="4ad82-234">To run the grid, you'll have to write a few lines of code.</span></span> <span data-ttu-id="4ad82-235">這幾行將做為一種幾乎所有您在本教學課程中執行資料存取模式。</span><span class="sxs-lookup"><span data-stu-id="4ad82-235">These few lines will serve as a kind of pattern for almost all of the data access that you do in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="4ad82-236">您實際上有許多選項來顯示資料在頁面的`WebGrid`協助程式只是其中一個。</span><span class="sxs-lookup"><span data-stu-id="4ad82-236">You actually have many options for displaying data on a page; the `WebGrid` helper is just one.</span></span> <span data-ttu-id="4ad82-237">我們選擇了它在本教學課程，因為它最簡單的方式顯示資料因為它是相當有彈性。</span><span class="sxs-lookup"><span data-stu-id="4ad82-237">We chose it for this tutorial because it's the easiest way to display data and because it's reasonably flexible.</span></span> <span data-ttu-id="4ad82-238">在下一個教學課程組中，您會看到如何使用更多的 「 手動 」 方式，在頁面中，可讓您更直接控制如何顯示資料的資料搭配使用。</span><span class="sxs-lookup"><span data-stu-id="4ad82-238">In the next tutorial set, you'll see how to use a more "manual" way to work with data in the page, which gives you more direct control over how to display the data.</span></span>


<span data-ttu-id="4ad82-239">在 WebMatrix 中左窗格中，按一下**檔案**工作區。</span><span class="sxs-lookup"><span data-stu-id="4ad82-239">In the left pane in WebMatrix, click the **Files** workspace.</span></span>

<span data-ttu-id="4ad82-240">您所建立的新資料庫處於*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="4ad82-240">The new database you created is in the *App\_Data* folder.</span></span> <span data-ttu-id="4ad82-241">如果資料夾不存在，WebMatrix 會建立它的新資料庫。</span><span class="sxs-lookup"><span data-stu-id="4ad82-241">If the folder didn't already exist, WebMatrix created it for your new database.</span></span> <span data-ttu-id="4ad82-242">（資料夾可能已經存在於如果您先前已安裝的協助程式。）</span><span class="sxs-lookup"><span data-stu-id="4ad82-242">(The folder might have existed if you'd previously installed helpers.)</span></span>

<span data-ttu-id="4ad82-243">在 [樹狀] 檢視中，選取網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="4ad82-243">In the tree view, select the root of the website.</span></span> <span data-ttu-id="4ad82-244">您必須選取網站; 的根目錄否則，新的檔案可能會加入應用程式\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="4ad82-244">You must select the root of the website; otherwise, the new file might be added to the App\_Data folder.</span></span>

<span data-ttu-id="4ad82-245">在 功能區中，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="4ad82-245">In the ribbon, click **New**.</span></span> <span data-ttu-id="4ad82-246">在 **選擇 檔案類型**方塊中，選擇**CSHTML**。</span><span class="sxs-lookup"><span data-stu-id="4ad82-246">In the **Choose a File Type** box, choose **CSHTML**.</span></span>

<span data-ttu-id="4ad82-247">在 **名稱**方塊，將新的頁面 」 Movies.cshtml 」:</span><span class="sxs-lookup"><span data-stu-id="4ad82-247">In the **Name** box, name the new page "Movies.cshtml":</span></span>

![選擇檔案類型 對話方塊中顯示 '電影' 頁面](displaying-data/_static/image17.png)

<span data-ttu-id="4ad82-249">按一下 [**確定**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="4ad82-249">Click the **OK** button.</span></span> <span data-ttu-id="4ad82-250">WebMatrix 會開啟新的檔案，與在其中一些基本架構項目。</span><span class="sxs-lookup"><span data-stu-id="4ad82-250">WebMatrix opens a new file with some skeleton elements in it.</span></span> <span data-ttu-id="4ad82-251">首先，您將撰寫一些程式碼以從資料庫取得資料。</span><span class="sxs-lookup"><span data-stu-id="4ad82-251">First you'll write some code to go get the data from the database.</span></span> <span data-ttu-id="4ad82-252">然後您會將標記加入頁面，來顯示資料。</span><span class="sxs-lookup"><span data-stu-id="4ad82-252">Then you'll add markup to the page to actually display the data.</span></span>

### <a name="writing-the-data-query-code"></a><span data-ttu-id="4ad82-253">寫入資料的查詢程式碼</span><span class="sxs-lookup"><span data-stu-id="4ad82-253">Writing the Data Query Code</span></span>

<span data-ttu-id="4ad82-254">在頁面上，頂端之間`@{`和`}`字元，輸入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="4ad82-254">At the top of the page, between the `@{` and `}` characters, enter the following code.</span></span> <span data-ttu-id="4ad82-255">（請確定您的開頭和結尾大括號之間輸入下列程式碼）。</span><span class="sxs-lookup"><span data-stu-id="4ad82-255">(Make sure that you enter this code between the opening and closing braces.)</span></span>

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

<span data-ttu-id="4ad82-256">第一行會開啟您稍早建立的資料庫是一律的第一個步驟之前執行的資料庫。</span><span class="sxs-lookup"><span data-stu-id="4ad82-256">The first line opens the database that you created earlier, which is always the first step before doing something with the database.</span></span> <span data-ttu-id="4ad82-257">您告訴`Database.Open`方法開啟的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="4ad82-257">You tell the `Database.Open` method name of the database to open.</span></span> <span data-ttu-id="4ad82-258">請注意，不包括 「 *.sdf*名稱中。</span><span class="sxs-lookup"><span data-stu-id="4ad82-258">Notice that you don't include *.sdf* in the name.</span></span> <span data-ttu-id="4ad82-259">`Open`方法會假設它正在尋求 *.sdf*檔案 (亦即*WebPagesMovies.sdf*) 且 *.sdf*檔案位於*的應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="4ad82-259">The `Open` method assumes that it's looking for an *.sdf* file (that is, *WebPagesMovies.sdf*) and that the *.sdf* file is in the *App\_Data* folder.</span></span> <span data-ttu-id="4ad82-260">(我們稍早所述，*應用程式\_資料*資料夾已保留; 這種情況下是其中一個位置，ASP.NET 會假設該名稱。)</span><span class="sxs-lookup"><span data-stu-id="4ad82-260">(Earlier we noted that the *App\_Data* folder is reserved; this scenario is one of the places where ASP.NET makes assumptions about that name.)</span></span>

<span data-ttu-id="4ad82-261">開啟資料庫時，它的參考會放入名為的變數`db`。</span><span class="sxs-lookup"><span data-stu-id="4ad82-261">When the database is opened, a reference to it is put into the variable named `db`.</span></span> <span data-ttu-id="4ad82-262">（這可以命名為任何項目。）`db`變數是您所得到與資料庫互動的方式。</span><span class="sxs-lookup"><span data-stu-id="4ad82-262">(Which could be named anything.) The `db` variable is how you'll end up interacting with the database.</span></span>

<span data-ttu-id="4ad82-263">實際使用擷取的資料庫資料的第二行`Query`方法。</span><span class="sxs-lookup"><span data-stu-id="4ad82-263">The second line actually fetches the database data by using the `Query` method.</span></span> <span data-ttu-id="4ad82-264">請注意此程式碼的運作方式：`db`變數參考開啟的資料庫，而且您叫用`Query`方法使用`db`變數 (`db.Query`)。</span><span class="sxs-lookup"><span data-stu-id="4ad82-264">Notice how this code works: the `db` variable has a reference to the opened database, and you invoke the `Query` method by using the `db` variable (`db.Query`).</span></span>

<span data-ttu-id="4ad82-265">查詢本身是 SQL`Select`陳述式。</span><span class="sxs-lookup"><span data-stu-id="4ad82-265">The query itself is a SQL `Select` statement.</span></span> <span data-ttu-id="4ad82-266">（如有點 SQL 的詳細背景，請參閱稍後說明）。在陳述式，`Movies`識別要查詢的資料表。</span><span class="sxs-lookup"><span data-stu-id="4ad82-266">(For a little background about SQL, see the explanation later.) In the statement, `Movies` identifies the table to query.</span></span> <span data-ttu-id="4ad82-267">`*`字元可讓您指定查詢應該從資料表傳回所有資料行。</span><span class="sxs-lookup"><span data-stu-id="4ad82-267">The `*` character specifies that the query should return all the columns from the table.</span></span> <span data-ttu-id="4ad82-268">（您可能也列出資料行個別，並以逗號分隔。）</span><span class="sxs-lookup"><span data-stu-id="4ad82-268">(You could also list columns individually, separated by commas.)</span></span>

<span data-ttu-id="4ad82-269">查詢的結果，如果有的話，會傳回，並可在`selectedData`變數。</span><span class="sxs-lookup"><span data-stu-id="4ad82-269">The results of the query, if any, are returned and made available in the `selectedData` variable.</span></span> <span data-ttu-id="4ad82-270">同樣地，變數可以命名為任何項目。</span><span class="sxs-lookup"><span data-stu-id="4ad82-270">Again, the variable could be named anything.</span></span>

<span data-ttu-id="4ad82-271">最後，第三個一行會告知 ASP.NET，您想要使用的執行個體`WebGrid`協助程式。</span><span class="sxs-lookup"><span data-stu-id="4ad82-271">Finally, the third line tells ASP.NET that you want to use an instance of the `WebGrid` helper.</span></span> <span data-ttu-id="4ad82-272">您所建立 (*具現化*) 使用，協助程式物件`new`關鍵字並將它傳遞查詢結果，透過`selectedData`變數。</span><span class="sxs-lookup"><span data-stu-id="4ad82-272">You create (*instantiate*) the helper object by using the `new` keyword and pass it the query results via the `selectedData` variable.</span></span> <span data-ttu-id="4ad82-273">新`WebGrid`物件，以及在資料庫查詢的結果可透過`grid`變數。</span><span class="sxs-lookup"><span data-stu-id="4ad82-273">The new `WebGrid` object, along with the results of the database query, are made available in the `grid` variable.</span></span> <span data-ttu-id="4ad82-274">您將實際顯示在頁面的 資料稍後需要該結果。</span><span class="sxs-lookup"><span data-stu-id="4ad82-274">You'll need that result in a moment to actually display the data in the page.</span></span>

<span data-ttu-id="4ad82-275">在這個階段中，開啟資料庫，您已取得資料，且您已備妥`WebGrid`協助程式與該資料。</span><span class="sxs-lookup"><span data-stu-id="4ad82-275">At this stage, the database has been opened, you've gotten the data you want, and you've prepared the `WebGrid` helper with that data.</span></span> <span data-ttu-id="4ad82-276">接下來是建立在頁面的標記。</span><span class="sxs-lookup"><span data-stu-id="4ad82-276">Next is to create the markup in the page.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="4ad82-277">**結構化的查詢語言 (SQL)**</span><span class="sxs-lookup"><span data-stu-id="4ad82-277">**Structured Query Language (SQL)**</span></span>
> 
> <span data-ttu-id="4ad82-278">SQL 是一種語言，在大部分的關聯式資料庫中用於管理資料庫中的資料。</span><span class="sxs-lookup"><span data-stu-id="4ad82-278">SQL is a language that's used in most relational databases for managing data in a database.</span></span> <span data-ttu-id="4ad82-279">它包含的指令，可讓您擷取資料，並加以更新，以及它可讓您建立、 修改及管理資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="4ad82-279">It includes commands that let you retrieve data and update it, and that let you create, modify, and manage data in database tables.</span></span> <span data-ttu-id="4ad82-280">SQL 是不同的程式語言 （例如 C# 中)。</span><span class="sxs-lookup"><span data-stu-id="4ad82-280">SQL is different than a programming language (like C#).</span></span> <span data-ttu-id="4ad82-281">使用 SQL，您需要告訴資料庫想什麼方法，而且資料庫的作業，以了解如何取得資料，或執行工作。</span><span class="sxs-lookup"><span data-stu-id="4ad82-281">With SQL, you tell the database what you want, and it's the database's job to figure out how to get the data or perform the task.</span></span> <span data-ttu-id="4ad82-282">以下是一些 SQL 命令的範例，以及他們如何：</span><span class="sxs-lookup"><span data-stu-id="4ad82-282">Here are examples of some SQL commands and what they do:</span></span>
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> <span data-ttu-id="4ad82-283">第一個`Select`陳述式會取得所有資料行 (依`*`) 從*電影*資料表。</span><span class="sxs-lookup"><span data-stu-id="4ad82-283">The first `Select` statement gets all the columns (specified by `*`) from the *Movies* table.</span></span>
> 
> <span data-ttu-id="4ad82-284">第二個`Select`陳述式會從記錄中擷取的識別碼、 名稱和價格資料行*產品*價格資料行值是 10 個以上的資料表。</span><span class="sxs-lookup"><span data-stu-id="4ad82-284">The second `Select` statement fetches the ID, Name, and Price columns from records in the *Product* table whose Price column value is more than 10.</span></span> <span data-ttu-id="4ad82-285">此命令會傳回結果，依字母順序，根據 名稱資料行的值。</span><span class="sxs-lookup"><span data-stu-id="4ad82-285">The command returns the results in alphabetical order based on the values of the Name column.</span></span> <span data-ttu-id="4ad82-286">如果沒有記錄符合價格準則，此命令會傳回空集合。</span><span class="sxs-lookup"><span data-stu-id="4ad82-286">If no records match the price criteria, the command returns an empty set.</span></span>
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> <span data-ttu-id="4ad82-287">此命令會將新記錄到*產品*設 1.99 」 可頌麵包 」 和 「 的不穩定愉悅的感覺 」，描述資料行的價格的名稱資料行的資料表。</span><span class="sxs-lookup"><span data-stu-id="4ad82-287">This command inserts a new record into the *Product* table, setting the Name column to "Croissant", the Description column to "A flaky delight", and the price to 1.99.</span></span>
> 
> <span data-ttu-id="4ad82-288">請注意當您指定的非數字值，該值括在單引號 （不雙引號括住，和在 C#）。</span><span class="sxs-lookup"><span data-stu-id="4ad82-288">Notice that when you're specifying a non-numeric value, the value is enclosed in single quotation marks (not double quotation marks, as in C#).</span></span> <span data-ttu-id="4ad82-289">您使用這些文字或日期值，但不是數字周圍的引號。</span><span class="sxs-lookup"><span data-stu-id="4ad82-289">You use these quotation marks around text or date values, but not around numbers.</span></span>
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> <span data-ttu-id="4ad82-290">此命令會刪除記錄檔中的*產品*其到期的日期資料行是早於 2008 年 1 月 1 日的資料表。</span><span class="sxs-lookup"><span data-stu-id="4ad82-290">This command deletes records in the *Product* table whose expiration date column is earlier than January 1, 2008.</span></span> <span data-ttu-id="4ad82-291">(此命令假設*產品*資料表中有這類資料行，當然。)在此處以 MM/DD/YYYY 格式輸入日期，但它應該使用於您的地區設定的格式輸入。</span><span class="sxs-lookup"><span data-stu-id="4ad82-291">(The command assumes that the *Product* table has such a column, of course.) The date is entered here in MM/DD/YYYY format, but it should be entered in the format that's used for your locale.</span></span>
> 
> <span data-ttu-id="4ad82-292">`Insert`和`Delete`命令未傳回結果集。</span><span class="sxs-lookup"><span data-stu-id="4ad82-292">The `Insert` and `Delete` commands don't return result sets.</span></span> <span data-ttu-id="4ad82-293">相反地，它們會傳回一個數字，會告訴您命令影響的多少筆記錄。</span><span class="sxs-lookup"><span data-stu-id="4ad82-293">Instead, they return a number that tells you how many records were affected by the command.</span></span>
> 
> <span data-ttu-id="4ad82-294">其中一些作業 （例如插入和刪除記錄），要求作業的程序必須在資料庫中有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="4ad82-294">For some of these operations (like inserting and deleting records), the process that's requesting the operation has to have appropriate permissions in the database.</span></span> <span data-ttu-id="4ad82-295">這是您通常需要提供使用者名稱和密碼，當您連接到資料庫的生產資料庫的原因。</span><span class="sxs-lookup"><span data-stu-id="4ad82-295">That's why for production databases you often have to supply a user name and password when you connect to the database.</span></span>
> 
> <span data-ttu-id="4ad82-296">有數十個 SQL 命令，但它們都遵循類似您在這裡看到的命令模式。</span><span class="sxs-lookup"><span data-stu-id="4ad82-296">There are dozens of SQL commands, but they all follow a pattern like the commands you see here.</span></span> <span data-ttu-id="4ad82-297">您可以使用 SQL 命令來建立資料庫資料表、 計算資料表中的記錄數目、 計算價格，以及執行許多更多的作業。</span><span class="sxs-lookup"><span data-stu-id="4ad82-297">You can use SQL commands to create database tables, count the number of records in a table, calculate prices, and perform many more operations.</span></span>


### <a name="adding-markup-to-display-the-data"></a><span data-ttu-id="4ad82-298">新增標記，以顯示資料</span><span class="sxs-lookup"><span data-stu-id="4ad82-298">Adding Markup to Display the Data</span></span>

<span data-ttu-id="4ad82-299">內部`<head>`項目，設定內容`<title>`「 Movies 」 的項目：</span><span class="sxs-lookup"><span data-stu-id="4ad82-299">Inside the `<head>` element, set contents of the `<title>` element to "Movies":</span></span>

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

<span data-ttu-id="4ad82-300">內`<body>`項目 頁面上，新增下列：</span><span class="sxs-lookup"><span data-stu-id="4ad82-300">Inside the `<body>` element of the page, add the following:</span></span>

[!code-html[Main](displaying-data/samples/sample3.html)]

<span data-ttu-id="4ad82-301">就這麼容易！</span><span class="sxs-lookup"><span data-stu-id="4ad82-301">That's it.</span></span> <span data-ttu-id="4ad82-302">`grid`變數是您建立您在建立時的值`WebGrid`稍早的程式碼中的物件。</span><span class="sxs-lookup"><span data-stu-id="4ad82-302">The `grid` variable is the value you created when you created the `WebGrid` object in code earlier.</span></span>

<span data-ttu-id="4ad82-303">在 WebMatrix 樹狀結構檢視中，以滑鼠右鍵按一下  頁面上，然後選取**在瀏覽器中啟動**。</span><span class="sxs-lookup"><span data-stu-id="4ad82-303">In the WebMatrix tree view, right-click the page and select **Launch in browser**.</span></span> <span data-ttu-id="4ad82-304">您會看到此頁面類似：</span><span class="sxs-lookup"><span data-stu-id="4ad82-304">You'll see something like this page:</span></span>

![從電影資料表的預設 WebGrid 協助程式輸出](displaying-data/_static/image18.png)

<span data-ttu-id="4ad82-306">按一下 排序依據該資料行的資料行標題連結。</span><span class="sxs-lookup"><span data-stu-id="4ad82-306">Click a column heading link to sort by that column.</span></span> <span data-ttu-id="4ad82-307">能夠按一下標題排序是內建的功能**WebGrid**協助程式。</span><span class="sxs-lookup"><span data-stu-id="4ad82-307">Being able to sort by clicking a heading is a feature that's built into the **WebGrid** helper.</span></span>

<span data-ttu-id="4ad82-308">`GetHtml`方法，正如其名，會產生標記，顯示的資料。</span><span class="sxs-lookup"><span data-stu-id="4ad82-308">The `GetHtml` method, as its name suggests, generates markup that displays the data.</span></span> <span data-ttu-id="4ad82-309">根據預設，`GetHtml`方法會產生 HTML`<table>`項目。</span><span class="sxs-lookup"><span data-stu-id="4ad82-309">By default, the `GetHtml` method generates an HTML `<table>` element.</span></span> <span data-ttu-id="4ad82-310">（如果您想，您可以確認呈現藉由查看網頁瀏覽器中的來源。）</span><span class="sxs-lookup"><span data-stu-id="4ad82-310">(If you want, you can verify the rendering by looking at the source of the page in the browser.)</span></span>

## <a name="modifying-the-look-of-the-grid"></a><span data-ttu-id="4ad82-311">修改方格的外觀</span><span class="sxs-lookup"><span data-stu-id="4ad82-311">Modifying the Look of the Grid</span></span>

<span data-ttu-id="4ad82-312">使用`WebGrid`像您剛剛輸入的協助程式很容易，但是在顯示的結果是純文字。</span><span class="sxs-lookup"><span data-stu-id="4ad82-312">Using the `WebGrid` helper like you just did is easy, but the resulting display is plain.</span></span> <span data-ttu-id="4ad82-313">`WebGrid`協助專家擁有各種選項，可讓您控制資料的顯示方式。</span><span class="sxs-lookup"><span data-stu-id="4ad82-313">The `WebGrid` helper has all sorts of options that let you control how the data is displayed.</span></span> <span data-ttu-id="4ad82-314">有太多，若要在本教學課程中，瀏覽，但本節可讓您了解這些選項的一部分。</span><span class="sxs-lookup"><span data-stu-id="4ad82-314">There are far too many to explore in this tutorial, but this section will give you an idea of some of those options.</span></span> <span data-ttu-id="4ad82-315">在本系列稍後的教學課程中，將會說明幾個其他選項。</span><span class="sxs-lookup"><span data-stu-id="4ad82-315">A few additional options will be covered in later tutorials in this series.</span></span>

### <a name="specifying-individual-columns-to-display"></a><span data-ttu-id="4ad82-316">指定要顯示的個別資料行</span><span class="sxs-lookup"><span data-stu-id="4ad82-316">Specifying Individual Columns to Display</span></span>

<span data-ttu-id="4ad82-317">若要開始，您可以指定您想要顯示特定資料行。</span><span class="sxs-lookup"><span data-stu-id="4ad82-317">To start, you can specify that you want to display only certain columns.</span></span> <span data-ttu-id="4ad82-318">根據預設，如您所見，此方格會顯示全部的四個欄位的資料行*電影*資料表。</span><span class="sxs-lookup"><span data-stu-id="4ad82-318">By default, as you've seen, the grid shows all four of the columns from the *Movies* table.</span></span>

<span data-ttu-id="4ad82-319">在  *Movies.cshtml*檔案中，取代`@grid.GetHtml()`您剛才加入以下列標記：</span><span class="sxs-lookup"><span data-stu-id="4ad82-319">In the *Movies.cshtml* file, replace the `@grid.GetHtml()` markup that you just added with the following:</span></span>

[!code-css[Main](displaying-data/samples/sample4.css)]

<span data-ttu-id="4ad82-320">若要告知協助專家要顯示的資料行，包括`columns`參數`GetHtml`方法並傳入的資料行集合。</span><span class="sxs-lookup"><span data-stu-id="4ad82-320">To tell the helper which columns to display, you include a `columns` parameter for the `GetHtml` method and pass in a collection of columns.</span></span> <span data-ttu-id="4ad82-321">在集合中，您可以指定要包含的每個資料行。</span><span class="sxs-lookup"><span data-stu-id="4ad82-321">In the collection, you specify each column to include.</span></span> <span data-ttu-id="4ad82-322">指定要顯示包含的個別資料行`grid.Column`物件，並傳入您想要的資料行名稱。</span><span class="sxs-lookup"><span data-stu-id="4ad82-322">You specify an individual column to display by including a `grid.Column` object, and pass in the name of the data column you want.</span></span> <span data-ttu-id="4ad82-323">(這些資料行必須包含在 SQL 查詢結果-`WebGrid`協助程式無法顯示不查詢所傳回的資料行。)</span><span class="sxs-lookup"><span data-stu-id="4ad82-323">(These columns must be included in the SQL query results — the `WebGrid` helper cannot display columns that were not returned by the query.)</span></span>

<span data-ttu-id="4ad82-324">啟動*Movies.cshtml*同樣地，而且這次得到如下所示顯示在瀏覽器中的頁面 （請注意，會顯示沒有識別碼資料行）：</span><span class="sxs-lookup"><span data-stu-id="4ad82-324">Launch the *Movies.cshtml* page in the browser again, and this time you get a display like the following one (notice that no ID column is displayed):</span></span>

![WebGrid 顯示所選的資料行](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a><span data-ttu-id="4ad82-326">變更格線的外觀</span><span class="sxs-lookup"><span data-stu-id="4ad82-326">Changing the Look of the Grid</span></span>

<span data-ttu-id="4ad82-327">有不少的更多選項，來顯示資料行，其中有些還將探索在稍後的教學課程，在此集合中。</span><span class="sxs-lookup"><span data-stu-id="4ad82-327">There are quite a few more options for displaying columns, some of which will be explored in later tutorials in this set.</span></span> <span data-ttu-id="4ad82-328">現在，本節將為您介紹您可以在其中設定樣式整個方格的方式。</span><span class="sxs-lookup"><span data-stu-id="4ad82-328">For now, this section will introduce you to ways in which you can style the grid as a whole.</span></span>

<span data-ttu-id="4ad82-329">內部`<head>`頁面上，只在關閉前一節`</head>`標記中加入下列`<style>`項目：</span><span class="sxs-lookup"><span data-stu-id="4ad82-329">Inside the `<head>` section of the page, just before the closing `</head>` tag, add the following `<style>` element:</span></span>

[!code-css[Main](displaying-data/samples/sample5.css)]

<span data-ttu-id="4ad82-330">這個 CSS 標記會定義名為的類別`grid`， `head`，依此類推。</span><span class="sxs-lookup"><span data-stu-id="4ad82-330">This CSS markup defines classes named `grid`, `head`, and so on.</span></span> <span data-ttu-id="4ad82-331">您也可以將這些樣式定義放在個別 *.css*檔案，並連結至頁面。</span><span class="sxs-lookup"><span data-stu-id="4ad82-331">You could also put these style definitions in a separate *.css* file and link that to the page.</span></span> <span data-ttu-id="4ad82-332">（事實上，您會執行，稍後在本教學課程組中。）但為了讓本教學課程簡單項目，它們的同一頁所顯示的資料內。</span><span class="sxs-lookup"><span data-stu-id="4ad82-332">(In fact, you'll do that later in this tutorial set.) But to make things easy for this tutorial, they're inside the same page that displays the data.</span></span>

<span data-ttu-id="4ad82-333">現在您可以取得`WebGrid`協助程式，才能使用這些樣式類別。</span><span class="sxs-lookup"><span data-stu-id="4ad82-333">Now you can get the `WebGrid` helper to use these style classes.</span></span> <span data-ttu-id="4ad82-334">協助專家擁有許多屬性 (例如`tableStyle`) 針對這個用途，您指派給它們的 CSS 樣式類別名稱和類別名稱會轉譯為呈現的標記協助程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="4ad82-334">The helper has a number of properties (for example, `tableStyle`) for just this purpose — you assign a CSS style class name to them, and that class name is rendered as part of the markup that's rendered by the helper.</span></span>

<span data-ttu-id="4ad82-335">變更`grid.GetHtml`標記，所以它現在看起來像此程式碼：</span><span class="sxs-lookup"><span data-stu-id="4ad82-335">Change the `grid.GetHtml` markup so that it now looks like this code:</span></span>

[!code-css[Main](displaying-data/samples/sample6.css)]

<span data-ttu-id="4ad82-336">這裡的新是您已新增`tableStyle`， `headerStyle`，並`alternatingRowStyle`參數來`GetHtml`方法。</span><span class="sxs-lookup"><span data-stu-id="4ad82-336">What's new here is that you've added `tableStyle`, `headerStyle`, and `alternatingRowStyle` parameters to the `GetHtml` method.</span></span> <span data-ttu-id="4ad82-337">這些參數，已設定為您加入的 CSS 樣式的名稱。</span><span class="sxs-lookup"><span data-stu-id="4ad82-337">These parameters have been set to the names of the CSS styles that you added a moment ago.</span></span>

<span data-ttu-id="4ad82-338">執行網頁，，此時您會看到一個方格，其中看起來較一般比之前：</span><span class="sxs-lookup"><span data-stu-id="4ad82-338">Run the page, and this time you see a grid that looks much less plain than before:</span></span>

![包含參數的 WebGrid 顯示設 CSS 類別名稱](displaying-data/_static/image20.png)

<span data-ttu-id="4ad82-340">若要查看`GetHtml`產生的方法，您可以查看網頁瀏覽器中的來源。</span><span class="sxs-lookup"><span data-stu-id="4ad82-340">To see what the `GetHtml` method generated, you can look at the source of the page in the browser.</span></span> <span data-ttu-id="4ad82-341">我們不會詳述明瞭，但很重要的一點是，藉由指定參數，例如`tableStyle`，造成資料格以產生 HTML 標記，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4ad82-341">We won't go into detail here, but the important point is that by specifying parameters like `tableStyle`, you caused the grid to generate HTML tags like the following:</span></span>

`<table class="grid">`

<span data-ttu-id="4ad82-342">`<table>`標記已`class`加入它的屬性會參考其中一個您先前加入的 CSS 規則。</span><span class="sxs-lookup"><span data-stu-id="4ad82-342">The `<table>` tag has had a `class` attribute added to it that references one of the CSS rules that you added earlier.</span></span> <span data-ttu-id="4ad82-343">此程式碼會示範基本模式&mdash;不同的參數，如`GetHtml`方法可讓您參考 CSS 類別的方法，然後產生標記。</span><span class="sxs-lookup"><span data-stu-id="4ad82-343">This code shows you the basic pattern &mdash; different parameters for the `GetHtml` method let you reference CSS classes that the method then generates along with the markup.</span></span> <span data-ttu-id="4ad82-344">您用來做什麼的 CSS 類別是由您決定。</span><span class="sxs-lookup"><span data-stu-id="4ad82-344">What you do with the CSS classes is up to you.</span></span>

## <a name="adding-paging"></a><span data-ttu-id="4ad82-345">加入分頁</span><span class="sxs-lookup"><span data-stu-id="4ad82-345">Adding Paging</span></span>

<span data-ttu-id="4ad82-346">在本教學課程的最後一個工作，您會將分頁新增至方格。</span><span class="sxs-lookup"><span data-stu-id="4ad82-346">As the last task for this tutorial, you'll add paging to the grid.</span></span> <span data-ttu-id="4ad82-347">現在就完全沒問題，一次顯示所有您影片的權限。</span><span class="sxs-lookup"><span data-stu-id="4ad82-347">Right now it's no problem to display all your movies at once.</span></span> <span data-ttu-id="4ad82-348">但如果您新增數百個電影時，頁面顯示就會很長。</span><span class="sxs-lookup"><span data-stu-id="4ad82-348">But if you added hundreds of movies, the page display would get long.</span></span>

<span data-ttu-id="4ad82-349">網頁程式碼中建立的行的變更`WebGrid`物件，以下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="4ad82-349">In the page code, change the line that creates the `WebGrid` object to the following code:</span></span>

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

<span data-ttu-id="4ad82-350">之前的唯一差別是您已新增的`rowsPerPage`設為 3 的參數。</span><span class="sxs-lookup"><span data-stu-id="4ad82-350">The only difference from before is that you've added a `rowsPerPage` parameter that's set to 3.</span></span>

<span data-ttu-id="4ad82-351">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="4ad82-351">Run the page.</span></span> <span data-ttu-id="4ad82-352">方格會顯示 3 個資料列，在建立時間，再加上瀏覽連結，可讓您的頁面上透過您的資料庫中的電影：</span><span class="sxs-lookup"><span data-stu-id="4ad82-352">The grid displays 3 rows at a time, plus navigation links that let you page through the movies in your database:</span></span>

![使用分頁 WebGrid 顯示](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a><span data-ttu-id="4ad82-354">接下來的下一步</span><span class="sxs-lookup"><span data-stu-id="4ad82-354">Coming Up Next</span></span>

<span data-ttu-id="4ad82-355">在下一個教學課程中，您將了解如何使用 Razor 和 C# 的程式碼的形式取得使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="4ad82-355">In the next tutorial, you'll learn how to use Razor and C# code to get user input in a form.</span></span> <span data-ttu-id="4ad82-356">因此您可以找到電影的標題或內容類型，您將在搜尋方塊新增至 Movies 頁面。</span><span class="sxs-lookup"><span data-stu-id="4ad82-356">You'll add a search box to the Movies page so that you can find movies by title or genre.</span></span>

## <a name="complete-listing-for-movies-page"></a><span data-ttu-id="4ad82-357">Movies 頁面的完整清單</span><span class="sxs-lookup"><span data-stu-id="4ad82-357">Complete Listing for Movies Page</span></span>

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="4ad82-358">其他資源</span><span class="sxs-lookup"><span data-stu-id="4ad82-358">Additional Resources</span></span>

- [<span data-ttu-id="4ad82-359">使用 Razor 語法的 ASP.NET Web 程式設計簡介</span><span class="sxs-lookup"><span data-stu-id="4ad82-359">Introduction to ASP.NET Web Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> <span data-ttu-id="4ad82-360">[上一頁](intro-to-web-pages-programming.md)
> [下一頁](form-basics.md)</span><span class="sxs-lookup"><span data-stu-id="4ad82-360">[Previous](intro-to-web-pages-programming.md)
[Next](form-basics.md)</span></span>
