---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: 建立資料庫 |Microsoft 文件
author: shanselman
description: 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 建立簡單的 web 應用程式可讀取和寫入資料庫中。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: ff2a41803cd31ce50bbf79e630d827b6de441ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-database"></a><span data-ttu-id="76ea9-104">建立資料庫</span><span class="sxs-lookup"><span data-stu-id="76ea9-104">Creating a Database</span></span>
====================
<span data-ttu-id="76ea9-105">由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="76ea9-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="76ea9-106">這是初學者教學課程介紹基本概念的 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="76ea9-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="76ea9-107">您將建立簡單的 web 應用程式可讀取和寫入資料庫中。</span><span class="sxs-lookup"><span data-stu-id="76ea9-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="76ea9-108">請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="76ea9-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="76ea9-109">本節中，我們即將建立新的 SQL Express 資料庫，我們將使用來儲存和擷取我們電影資料。</span><span class="sxs-lookup"><span data-stu-id="76ea9-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="76ea9-110">從選取 在 Visual Web Developer ide 的 檢視 |伺服器總管。</span><span class="sxs-lookup"><span data-stu-id="76ea9-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="76ea9-111">以滑鼠右鍵按一下資料連接，並按一下 新增連接...</span><span class="sxs-lookup"><span data-stu-id="76ea9-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="76ea9-113">在 選擇資料來源 對話方塊中，選取 Microsoft SQL Server，並選取 繼續。</span><span class="sxs-lookup"><span data-stu-id="76ea9-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="76ea9-114">在 [加入連接] 對話方塊中，輸入 「。 \SQLEXPRESS 」 為您的伺服器名稱，並輸入"影片"做為您的新資料庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="76ea9-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="76ea9-115">[![新增連接對話方塊](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="76ea9-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="76ea9-116">按一下 [確定]，您將需要在想要建立該資料庫。</span><span class="sxs-lookup"><span data-stu-id="76ea9-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="76ea9-117">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="76ea9-117">Select yes.</span></span>

<span data-ttu-id="76ea9-118">[![建立電影嗎？](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="76ea9-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="76ea9-119">現在您已經在伺服器總管 中有空白資料庫。</span><span class="sxs-lookup"><span data-stu-id="76ea9-119">Now you've got an empty database in Server Explorer.</span></span>

![加入新的資料表](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="76ea9-121">以滑鼠右鍵按一下資料表，然後按一下 加入資料表。</span><span class="sxs-lookup"><span data-stu-id="76ea9-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="76ea9-122">資料表設計工具將會出現。</span><span class="sxs-lookup"><span data-stu-id="76ea9-122">The Table Designer will appear.</span></span> <span data-ttu-id="76ea9-123">加入資料行的識別碼、 標題、 ReleaseDate、 類型和價格。</span><span class="sxs-lookup"><span data-stu-id="76ea9-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="76ea9-124">識別碼資料行上按一下滑鼠右鍵，按一下 設定主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="76ea9-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="76ea9-125">以下是我外觀的設計區域。</span><span class="sxs-lookup"><span data-stu-id="76ea9-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="76ea9-126">[![資料庫資料表編輯器](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="76ea9-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="76ea9-127">此外，選取識別碼資料行，並在下方的資料行屬性將 「 身分識別規格 」 變更為"Yes"。</span><span class="sxs-lookup"><span data-stu-id="76ea9-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="76ea9-128">[![IsIdentity-資料行屬性](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="76ea9-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="76ea9-129">當您選擇了它完成時，請按一下工具列中的 [儲存] 圖示，或選取檔案 |從功能表中，儲存並命名您的資料表 」**影片**」 （單數）。</span><span class="sxs-lookup"><span data-stu-id="76ea9-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="76ea9-130">我們有資料庫和資料表 ！</span><span class="sxs-lookup"><span data-stu-id="76ea9-130">We've got a database and a table!</span></span>

<span data-ttu-id="76ea9-131">[![選擇名稱](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="76ea9-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="76ea9-132">返回 [伺服器總管] 並以滑鼠右鍵按一下 [影片] 資料表，然後選取 [顯示資料表資料]。</span><span class="sxs-lookup"><span data-stu-id="76ea9-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="76ea9-133">因此我們的資料庫具有一些資料，請輸入一些影片。</span><span class="sxs-lookup"><span data-stu-id="76ea9-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="76ea9-134">[![資料庫表格編輯](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="76ea9-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="76ea9-135">建立模型</span><span class="sxs-lookup"><span data-stu-id="76ea9-135">Creating a Model</span></span>

<span data-ttu-id="76ea9-136">現在，切換回上 IDE 的右手邊的 方案總管 和 模型 資料夾上按一下滑鼠右鍵，然後選取 加入 |新項目。</span><span class="sxs-lookup"><span data-stu-id="76ea9-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="76ea9-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="76ea9-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="76ea9-138">我們將我們新的資料庫中建立實體模型。</span><span class="sxs-lookup"><span data-stu-id="76ea9-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="76ea9-139">這會將一組類別，加入受測專案，以簡化我們來查詢與管理資料庫內的資料。</span><span class="sxs-lookup"><span data-stu-id="76ea9-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="76ea9-140">選取對話方塊左側的資料節點，然後選取 ADO.NET 實體資料模型項目範本。</span><span class="sxs-lookup"><span data-stu-id="76ea9-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="76ea9-141">命名 Movies.edmx。</span><span class="sxs-lookup"><span data-stu-id="76ea9-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="76ea9-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="76ea9-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="76ea9-143">按一下 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="76ea9-143">Click the "Add" button.</span></span> <span data-ttu-id="76ea9-144">然後，這會啟動 「 實體資料模型精靈 」。</span><span class="sxs-lookup"><span data-stu-id="76ea9-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="76ea9-145">在新的快顯對話方塊中，選取 從資料庫產生。</span><span class="sxs-lookup"><span data-stu-id="76ea9-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="76ea9-146">因為我們剛才資料庫，我們只需要告訴我們新的資料庫和其資料表的 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="76ea9-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="76ea9-147">按一下 旁邊，儲存在我們的 web 應用程式設定資料庫連線。</span><span class="sxs-lookup"><span data-stu-id="76ea9-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="76ea9-148">現在，請檢查資料表及電影核取方塊，然後按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="76ea9-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="76ea9-149">[![實體資料模型精靈](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="76ea9-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="76ea9-150">現在我們可以看到我們新的電影資料表在 Entity Framework Designer 中，並從程式碼存取它。</span><span class="sxs-lookup"><span data-stu-id="76ea9-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="76ea9-151">[![影片-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="76ea9-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="76ea9-152">在設計介面上，您可以看到 「 電影 」 類別。</span><span class="sxs-lookup"><span data-stu-id="76ea9-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="76ea9-153">這個類別會對應到 「 電影 」 資料表在資料庫中，並在其中每個屬性會對應至資料表的資料行。</span><span class="sxs-lookup"><span data-stu-id="76ea9-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="76ea9-154">「 電影"類別的每個執行個體將會對應到在 「 電影 」 資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="76ea9-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="76ea9-155">如果您不喜歡的預設命名和對應 Entity Framework 所使用的慣例，您可以使用 Entity Framework 設計工具來變更或自訂這兩者。</span><span class="sxs-lookup"><span data-stu-id="76ea9-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="76ea9-156">此應用程式，我們將使用預設值，只將檔案儲存為-是。</span><span class="sxs-lookup"><span data-stu-id="76ea9-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="76ea9-157">現在，讓我們來處理一些實際資料 ！</span><span class="sxs-lookup"><span data-stu-id="76ea9-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="76ea9-158">[上一頁](getting-started-with-mvc-part3.md)
> [下一頁](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="76ea9-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
