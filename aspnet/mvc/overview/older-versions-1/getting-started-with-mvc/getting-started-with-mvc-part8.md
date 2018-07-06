---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: 加入至模型的資料行 |Microsoft Docs
author: shanselman
description: 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 建立簡單的 web 應用程式，從資料庫讀取與寫入。
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 5dbda1280c073d5d4f2d71918ca31bc44475f64d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817197"
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="f36ad-104">將資料行加入至模型</span><span class="sxs-lookup"><span data-stu-id="f36ad-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="f36ad-105">藉由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="f36ad-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="f36ad-106">這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="f36ad-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="f36ad-107">您將建立簡單 web 應用程式，從資料庫讀取與寫入。</span><span class="sxs-lookup"><span data-stu-id="f36ad-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="f36ad-108">請瀏覽[ASP.NET MVC 學習中心](../../../index.md)來尋找其他 ASP.NET MVC 教學課程和範例。</span><span class="sxs-lookup"><span data-stu-id="f36ad-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="f36ad-109">這一節我們要逐步解說如何對資料庫中的結構描述中的變更並在我們的應用程式內處理變更。</span><span class="sxs-lookup"><span data-stu-id="f36ad-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="f36ad-110">讓我們將 「 評等 」 的資料行新增至電影資料表中。</span><span class="sxs-lookup"><span data-stu-id="f36ad-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="f36ad-111">返回 IDE，然後按一下 [資料庫總管] 中。</span><span class="sxs-lookup"><span data-stu-id="f36ad-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="f36ad-112">以滑鼠右鍵按一下之電影資料表，然後選取 開啟資料表定義。</span><span class="sxs-lookup"><span data-stu-id="f36ad-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="f36ad-113">將 「 評等 」 資料行，如下所示。</span><span class="sxs-lookup"><span data-stu-id="f36ad-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="f36ad-114">我們現在還沒有任何評等，因為資料行可允許 null。</span><span class="sxs-lookup"><span data-stu-id="f36ad-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="f36ad-115">按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="f36ad-115">Click Save.</span></span>

<span data-ttu-id="f36ad-116">[![編輯電影資料表](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f36ad-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="f36ad-117">接下來，返回 [方案總管]，然後開啟 Movies.edmx 檔案 （這是 \Models 資料夾中）。</span><span class="sxs-lookup"><span data-stu-id="f36ad-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="f36ad-118">以滑鼠右鍵按一下設計介面 （白色區域），並選取 從資料庫更新模型。</span><span class="sxs-lookup"><span data-stu-id="f36ad-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="f36ad-119">[![影片-Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f36ad-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="f36ad-120">這會啟動 「 更新精靈 」。</span><span class="sxs-lookup"><span data-stu-id="f36ad-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="f36ad-121">按一下 [重新整理] 索引標籤中的，按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="f36ad-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="f36ad-122">我們的電影模型類別就會更新與新的資料行。</span><span class="sxs-lookup"><span data-stu-id="f36ad-122">Our Movie model class will then be updated with the new column.</span></span>

![更新精靈 (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="f36ad-124">之後按一下 [完成]，您可以看到新的評等資料行已新增至電影實體，在我們的模型。</span><span class="sxs-lookup"><span data-stu-id="f36ad-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="f36ad-125">[![電影實體](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="f36ad-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="f36ad-126">我們已新增資料行在資料庫模型中，但檢視不知道。</span><span class="sxs-lookup"><span data-stu-id="f36ad-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="f36ad-127">更新檢視的模型變更</span><span class="sxs-lookup"><span data-stu-id="f36ad-127">Update Views with Model Changes</span></span>

<span data-ttu-id="f36ad-128">有幾種方式，我們無法更新我們的檢視範本，以反映新的評等資料行。</span><span class="sxs-lookup"><span data-stu-id="f36ad-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="f36ad-129">我們產生它們透過 [新增檢視] 對話方塊來建立這些檢視，所以我們無法刪除它們，然後再重新建立它們。</span><span class="sxs-lookup"><span data-stu-id="f36ad-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="f36ad-130">不過，通常人將已經做了修改其檢視的範本，從初始的 scaffold 層代而且會想要加入或刪除欄位，以手動方式，就如同我們使用 [識別碼] 欄位建立。</span><span class="sxs-lookup"><span data-stu-id="f36ad-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="f36ad-131">開啟 \Views\Movies\Index.aspx 範本，並新增&lt;th&gt;評等&lt;/td>< /&gt;至之電影資料表標頭。</span><span class="sxs-lookup"><span data-stu-id="f36ad-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="f36ad-132">我加入了我的內容類型之後。</span><span class="sxs-lookup"><span data-stu-id="f36ad-132">I added mine after Genre.</span></span> <span data-ttu-id="f36ad-133">然後，在相同的資料行位置，但下方，新增一行輸出我們新評比。</span><span class="sxs-lookup"><span data-stu-id="f36ad-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="f36ad-134">我們最終的 Index.aspx 範本看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="f36ad-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="f36ad-135">讓我們再開啟 \Views\Movies\Create.aspx 範本，並新增標籤和文字方塊中，我們新的評等屬性：</span><span class="sxs-lookup"><span data-stu-id="f36ad-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="f36ad-136">我們最終 Create.aspx 範本會看起來像這樣，並讓我們變更我們瀏覽器的標題和次要&lt;h2&gt;標題為類似"建立電影 」 我們在這裡 ！</span><span class="sxs-lookup"><span data-stu-id="f36ad-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="f36ad-137">執行您的應用程式，現在您已經有新資料庫中的欄位已加入至 [建立] 頁面。</span><span class="sxs-lookup"><span data-stu-id="f36ad-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="f36ad-138">新增新的電影-評分-這次，然後按一下 建立。</span><span class="sxs-lookup"><span data-stu-id="f36ad-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="f36ad-139">[![建立電影-Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="f36ad-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="f36ad-140">按一下 [建立] 之後，您要傳送至 [索引] 頁面在您新增電影會列出新的評等資料行在資料庫中</span><span class="sxs-lookup"><span data-stu-id="f36ad-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="f36ad-141">[![電影清單-Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="f36ad-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="f36ad-142">此基本教學課程中有您開始進行控制站，將它們關聯的檢視，並傳遞硬式編碼的資料。</span><span class="sxs-lookup"><span data-stu-id="f36ad-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="f36ad-143">然後我們建立及設計資料庫，並放入一些資料中。</span><span class="sxs-lookup"><span data-stu-id="f36ad-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="f36ad-144">我們會從資料庫擷取資料，並顯示 HTML 表格中。</span><span class="sxs-lookup"><span data-stu-id="f36ad-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="f36ad-145">然後，我們會新增可讓使用者將資料加入至資料庫本身從 Web 應用程式內建立表單。</span><span class="sxs-lookup"><span data-stu-id="f36ad-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="f36ad-146">我們新增驗證，然後進行用戶端使用 JavaScript 的驗證。</span><span class="sxs-lookup"><span data-stu-id="f36ad-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="f36ad-147">最後，我們變更了資料庫以包含新的資料行的資料，然後更新我們的兩個頁面，來建立並顯示這項新資料。</span><span class="sxs-lookup"><span data-stu-id="f36ad-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="f36ad-148">現在建議您前往我們的中繼層級教學課程 」[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)」 以及許多的影片和辦公[ https://asp.net/mvc ](https://asp.net/mvc)更進一步了解 ASP.NET MVC ！</span><span class="sxs-lookup"><span data-stu-id="f36ad-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="f36ad-149">敬祝您使用愉快！</span><span class="sxs-lookup"><span data-stu-id="f36ad-149">Enjoy!</span></span>

- <span data-ttu-id="f36ad-150">Scott Hanselman- [ http://hanselman.com ](http://hanselman.com)並[ @shanselman ](http://twitter.com/shanselman)在 Twitter 上。</span><span class="sxs-lookup"><span data-stu-id="f36ad-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f36ad-151">上一步</span><span class="sxs-lookup"><span data-stu-id="f36ad-151">Previous</span></span>](getting-started-with-mvc-part7.md)
