---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: 建立資料庫 |Microsoft Docs
author: shanselman
description: 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 建立簡單的 web 應用程式，從資料庫讀取與寫入。
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: fff974edfaffeb164879c10b8e70bd3482c47554
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813756"
---
<a name="creating-a-database"></a><span data-ttu-id="2f79d-104">建立資料庫</span><span class="sxs-lookup"><span data-stu-id="2f79d-104">Creating a Database</span></span>
====================
<span data-ttu-id="2f79d-105">藉由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="2f79d-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="2f79d-106">這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="2f79d-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="2f79d-107">您將建立簡單 web 應用程式，從資料庫讀取與寫入。</span><span class="sxs-lookup"><span data-stu-id="2f79d-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="2f79d-108">請瀏覽[ASP.NET MVC 學習中心](../../../index.md)來尋找其他 ASP.NET MVC 教學課程和範例。</span><span class="sxs-lookup"><span data-stu-id="2f79d-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="2f79d-109">我們要建立新的 SQL Express 資料庫，我們將使用它來儲存和擷取電影資料這一節。</span><span class="sxs-lookup"><span data-stu-id="2f79d-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="2f79d-110">從的 在 Visual Web Developer IDE 中，選取 檢視 |伺服器總管。</span><span class="sxs-lookup"><span data-stu-id="2f79d-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="2f79d-111">在資料連線上按一下滑鼠右鍵，然後按一下 [新增連接...]</span><span class="sxs-lookup"><span data-stu-id="2f79d-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="2f79d-113">在 選擇資料來源 對話方塊中，選取 Microsoft SQL Server，並選取 繼續。</span><span class="sxs-lookup"><span data-stu-id="2f79d-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="2f79d-114">在 [新增連接] 對話方塊中，輸入 「。 \SQLEXPRESS 」 為您的伺服器名稱，並輸入 「 Movies 」 做為您的新資料庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="2f79d-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="2f79d-115">[![[新增連接] 對話方塊](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="2f79d-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="2f79d-116">按一下 [確定]，並將系統要求您想要建立該資料庫。</span><span class="sxs-lookup"><span data-stu-id="2f79d-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="2f79d-117">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="2f79d-117">Select yes.</span></span>

<span data-ttu-id="2f79d-118">[![建立電影嗎？](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="2f79d-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="2f79d-119">現在您已經在伺服器總管 中有空白的資料庫。</span><span class="sxs-lookup"><span data-stu-id="2f79d-119">Now you've got an empty database in Server Explorer.</span></span>

![加入新的資料表](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="2f79d-121">以滑鼠右鍵按一下資料表，然後按一下 加入資料表。</span><span class="sxs-lookup"><span data-stu-id="2f79d-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="2f79d-122">資料表設計工具將會出現。</span><span class="sxs-lookup"><span data-stu-id="2f79d-122">The Table Designer will appear.</span></span> <span data-ttu-id="2f79d-123">加入資料行的識別碼、 標題、 ReleaseDate、 內容類型和價格。</span><span class="sxs-lookup"><span data-stu-id="2f79d-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="2f79d-124">以滑鼠右鍵按一下 識別碼 欄，並按一下 設定主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2f79d-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="2f79d-125">以下是看起來像什麼我設計區域。</span><span class="sxs-lookup"><span data-stu-id="2f79d-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="2f79d-126">[![資料庫資料表編輯器](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="2f79d-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="2f79d-127">此外，選取 [識別碼] 資料行和下列資料行屬性底下將 「 身分識別規格 」 變更為 「 是 」。</span><span class="sxs-lookup"><span data-stu-id="2f79d-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="2f79d-128">[![IsIdentity-資料行屬性](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="2f79d-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="2f79d-129">當您有完成工作時，按一下工具列中的 [儲存] 圖示，或選取檔案 |從功能表中，儲存並命名您的資料表 」**電影**」 （個別）。</span><span class="sxs-lookup"><span data-stu-id="2f79d-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="2f79d-130">我們有資料庫和資料表 ！</span><span class="sxs-lookup"><span data-stu-id="2f79d-130">We've got a database and a table!</span></span>

<span data-ttu-id="2f79d-131">[![選擇名稱](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="2f79d-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="2f79d-132">返回 伺服器總管 中以滑鼠右鍵按一下 影片 資料表中，然後選取 顯示資料表資料 」。</span><span class="sxs-lookup"><span data-stu-id="2f79d-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="2f79d-133">因此我們的資料庫有一些資料，請輸入幾個電影。</span><span class="sxs-lookup"><span data-stu-id="2f79d-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="2f79d-134">[![資料庫表格編輯](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="2f79d-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="2f79d-135">建立模型</span><span class="sxs-lookup"><span data-stu-id="2f79d-135">Creating a Model</span></span>

<span data-ttu-id="2f79d-136">現在，切換回在 IDE 的右手邊的 方案總管 和 Models 資料夾上按一下滑鼠右鍵並選取 新增 |新的項目。</span><span class="sxs-lookup"><span data-stu-id="2f79d-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="2f79d-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="2f79d-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="2f79d-138">我們建立我們新的資料庫中的實體模型。</span><span class="sxs-lookup"><span data-stu-id="2f79d-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="2f79d-139">這會將一組類別，加入我們的專案，可讓您更容易讓我們來查詢及管理資料庫中的資料。</span><span class="sxs-lookup"><span data-stu-id="2f79d-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="2f79d-140">選取對話方塊中，左側的 資料 節點，然後選取 ADO.NET 實體資料模型項目範本。</span><span class="sxs-lookup"><span data-stu-id="2f79d-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="2f79d-141">命名 Movies.edmx。</span><span class="sxs-lookup"><span data-stu-id="2f79d-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="2f79d-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="2f79d-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="2f79d-143">按一下 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2f79d-143">Click the "Add" button.</span></span> <span data-ttu-id="2f79d-144">然後，這會啟動 「 實體資料模型精靈 」。</span><span class="sxs-lookup"><span data-stu-id="2f79d-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="2f79d-145">在新快顯對話方塊中，選取 從資料庫產生。</span><span class="sxs-lookup"><span data-stu-id="2f79d-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="2f79d-146">我們剛了資料庫，因為我們只需要告訴我們新的資料庫和其資料表中的 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="2f79d-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="2f79d-147">按一下 下一步儲存我們在我們的 web 應用程式組態中的資料庫連接。</span><span class="sxs-lookup"><span data-stu-id="2f79d-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="2f79d-148">現在，請檢查資料表與電影核取方塊，然後按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="2f79d-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="2f79d-149">[![實體資料模型精靈](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="2f79d-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="2f79d-150">現在我們可以看到我們在 Entity Framework Designer 中的新電影資料表，並從程式碼中存取。</span><span class="sxs-lookup"><span data-stu-id="2f79d-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="2f79d-151">[![影片-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="2f79d-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="2f79d-152">在設計介面上，您可以看到 「 電影 」 類別。</span><span class="sxs-lookup"><span data-stu-id="2f79d-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="2f79d-153">此類別會對應到在資料庫中之 「 電影 」 資料表，並在其中每個屬性會對應至資料表的資料行。</span><span class="sxs-lookup"><span data-stu-id="2f79d-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="2f79d-154">「 電影 」 類別的每個執行個體將會對應到在 「 電影 」 資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="2f79d-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="2f79d-155">如果您不喜歡預設命名和對應 Entity Framework 所使用的慣例，您可以使用 Entity Framework 設計工具變更或加以自訂。</span><span class="sxs-lookup"><span data-stu-id="2f79d-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="2f79d-156">我們將此應用程式使用預設值，並只將檔案儲存為集是。</span><span class="sxs-lookup"><span data-stu-id="2f79d-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="2f79d-157">現在，我們將使用一些實際資料 ！</span><span class="sxs-lookup"><span data-stu-id="2f79d-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2f79d-158">[上一頁](getting-started-with-mvc-part3.md)
> [下一頁](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="2f79d-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
