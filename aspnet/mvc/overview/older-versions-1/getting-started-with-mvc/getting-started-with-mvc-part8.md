---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: 將資料行加入至模型 |Microsoft 文件
author: shanselman
description: 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 建立簡單的 web 應用程式可讀取和寫入資料庫中。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="43fae-104">將資料行加入至模型</span><span class="sxs-lookup"><span data-stu-id="43fae-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="43fae-105">由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="43fae-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="43fae-106">這是初學者教學課程介紹基本概念的 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="43fae-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="43fae-107">您將建立簡單的 web 應用程式可讀取和寫入資料庫中。</span><span class="sxs-lookup"><span data-stu-id="43fae-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="43fae-108">請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="43fae-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="43fae-109">本節中，我們即將逐步解說我們如何對我們的資料庫，結構描述變更及處理應用程式中所做的變更。</span><span class="sxs-lookup"><span data-stu-id="43fae-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="43fae-110">讓我們影片資料表中加入"評等 」 的資料行。</span><span class="sxs-lookup"><span data-stu-id="43fae-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="43fae-111">返回 IDE，並按一下 [資料庫總管]。</span><span class="sxs-lookup"><span data-stu-id="43fae-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="43fae-112">以滑鼠右鍵按一下 影片 資料表，然後選取 開啟資料表定義。</span><span class="sxs-lookup"><span data-stu-id="43fae-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="43fae-113">新增 「 等級 」 資料行，如下所示。</span><span class="sxs-lookup"><span data-stu-id="43fae-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="43fae-114">我們現在不需要任何評等，因為資料行可允許 null。</span><span class="sxs-lookup"><span data-stu-id="43fae-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="43fae-115">按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="43fae-115">Click Save.</span></span>

<span data-ttu-id="43fae-116">[![編輯電影資料表](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="43fae-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="43fae-117">接下來，返回 [方案總管]，然後開啟 Movies.edmx 檔案 （這是 \Models 資料夾中）。</span><span class="sxs-lookup"><span data-stu-id="43fae-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="43fae-118">以滑鼠右鍵按一下設計介面 （白色區域） 上，選取 從資料庫更新模型。</span><span class="sxs-lookup"><span data-stu-id="43fae-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="43fae-119">[![影片-Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="43fae-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="43fae-120">這會啟動 「 更新精靈 」。</span><span class="sxs-lookup"><span data-stu-id="43fae-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="43fae-121">按一下 [重新整理] 索引標籤中的，然後按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="43fae-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="43fae-122">我們的影片模型類別就會更新與新的資料行。</span><span class="sxs-lookup"><span data-stu-id="43fae-122">Our Movie model class will then be updated with the new column.</span></span>

![更新精靈 (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="43fae-124">之後按一下 [完成]，您可以看到新的評等資料行已加入至電影中的實體我們的模型。</span><span class="sxs-lookup"><span data-stu-id="43fae-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="43fae-125">[![影片實體](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="43fae-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="43fae-126">我們已在資料庫模型中，加入資料行，但檢視不了解它。</span><span class="sxs-lookup"><span data-stu-id="43fae-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="43fae-127">更新檢視的模型變更</span><span class="sxs-lookup"><span data-stu-id="43fae-127">Update Views with Model Changes</span></span>

<span data-ttu-id="43fae-128">有幾個方法，我們無法更新以反映新的評等資料行我們檢視範本。</span><span class="sxs-lookup"><span data-stu-id="43fae-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="43fae-129">我們建立這些檢視透過 [新增檢視] 對話方塊產生它們，因為我們無法加以刪除，再重新建立它們。</span><span class="sxs-lookup"><span data-stu-id="43fae-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="43fae-130">不過，通常人員將已經完成初始 scaffold 產生從其檢視範本的修改，會想要加入或刪除欄位，以手動方式，就像我們之前使用 [識別碼] 欄位建立。</span><span class="sxs-lookup"><span data-stu-id="43fae-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="43fae-131">開啟 \Views\Movies\Index.aspx 範本，並新增&lt;th&gt;分級&lt;t&gt;標頭的電影資料表。</span><span class="sxs-lookup"><span data-stu-id="43fae-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="43fae-132">我將我加入內容類型之後。</span><span class="sxs-lookup"><span data-stu-id="43fae-132">I added mine after Genre.</span></span> <span data-ttu-id="43fae-133">然後，在相同的資料行位置，但下面的部分，將行加入輸出我們新的評等。</span><span class="sxs-lookup"><span data-stu-id="43fae-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="43fae-134">我們最終 Index.aspx 範本看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="43fae-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="43fae-135">讓我們再開啟 \Views\Movies\Create.aspx 範本，並將標籤和文字方塊中加入新的評等屬性：</span><span class="sxs-lookup"><span data-stu-id="43fae-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="43fae-136">我們最終 Create.aspx 範本才會看起來像這樣，並讓我們來變更 我們瀏覽器的標題和次要&lt;h2&gt;標題為類似"建立電影"雖然我們在這裡 ！</span><span class="sxs-lookup"><span data-stu-id="43fae-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="43fae-137">執行您的應用程式，現在您已經有新的欄位已加入到 [建立] 頁面的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="43fae-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="43fae-138">加入新的電影-分級-這個階段，並按一下 建立。</span><span class="sxs-lookup"><span data-stu-id="43fae-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="43fae-139">[![建立電影-Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="43fae-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="43fae-140">按一下 [建立] 之後，您要傳送至索引頁面在新的電影列出您使用新的評等資料行在資料庫中</span><span class="sxs-lookup"><span data-stu-id="43fae-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="43fae-141">[![電影清單-Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="43fae-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="43fae-142">此基本教學課程了您開始進行控制站，檢視與建立關聯，並傳遞硬式編碼的資料。</span><span class="sxs-lookup"><span data-stu-id="43fae-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="43fae-143">然後我們建立及設計資料庫，並將一些資料放在。</span><span class="sxs-lookup"><span data-stu-id="43fae-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="43fae-144">我們會從資料庫擷取資料，並顯示 HTML 表格中。</span><span class="sxs-lookup"><span data-stu-id="43fae-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="43fae-145">然後我們會加入可讓使用者將資料加入至資料庫本身從 Web 應用程式內建立新表單。</span><span class="sxs-lookup"><span data-stu-id="43fae-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="43fae-146">我們加入驗證，然後進行驗證用戶端使用 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="43fae-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="43fae-147">最後，我們變更了資料庫以包含新的資料行的資料，然後更新我們來建立並顯示這項新資料的兩個頁面。</span><span class="sxs-lookup"><span data-stu-id="43fae-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="43fae-148">現在鼓勵您移到我們的中繼層級教學課程 」[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)」 以及許多視訊和資源的[ https://asp.net/mvc ](https://asp.net/mvc)若要了解更多關於 ASP.NET MVC ！</span><span class="sxs-lookup"><span data-stu-id="43fae-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="43fae-149">敬祝您使用愉快！</span><span class="sxs-lookup"><span data-stu-id="43fae-149">Enjoy!</span></span>

- <span data-ttu-id="43fae-150">Scott Hanselman- [ http://hanselman.com ](http://hanselman.com)和[ @shanselman ](http://twitter.com/shanselman) Twitter 上。</span><span class="sxs-lookup"><span data-stu-id="43fae-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="43fae-151">上一步</span><span class="sxs-lookup"><span data-stu-id="43fae-151">Previous</span></span>](getting-started-with-mvc-part7.md)
