---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: 存取您的模型資料從控制器 |Microsoft 文件
author: shanselman
description: 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 建立簡單的 web 應用程式可讀取和寫入資料庫中。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="2027a-104">從控制器存取您的模型資料</span><span class="sxs-lookup"><span data-stu-id="2027a-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="2027a-105">由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="2027a-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="2027a-106">這是初學者教學課程介紹基本概念的 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="2027a-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="2027a-107">您將建立簡單的 web 應用程式可讀取和寫入資料庫中。</span><span class="sxs-lookup"><span data-stu-id="2027a-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="2027a-108">請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="2027a-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="2027a-109">本節中，我們會建立新的 MoviesController 類別，並撰寫一些程式碼會擷取我們電影資料並顯示它回到使用檢視範本的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="2027a-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="2027a-110">以滑鼠右鍵按一下 [控制器] 資料夾，並讓新 MoviesController。</span><span class="sxs-lookup"><span data-stu-id="2027a-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="2027a-111">[![加入控制器](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2027a-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="2027a-112">這會建立新的 「 MoviesController.cs"檔案內受測專案我們 \Controllers 資料夾下。</span><span class="sxs-lookup"><span data-stu-id="2027a-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="2027a-113">讓我們來更新 MovieController 從我們的新填入資料庫擷取的電影清單。</span><span class="sxs-lookup"><span data-stu-id="2027a-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="2027a-114">我們正在執行 LINQ 查詢，好讓我們只擷取 1984年的夏天之後發行的電影。</span><span class="sxs-lookup"><span data-stu-id="2027a-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="2027a-115">我們需要呈現的電影回此清單，因此在方法中以滑鼠右鍵按一下，然後選取 加入檢視，來建立它的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="2027a-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="2027a-116">[新增檢視] 對話方塊中我們將會指出我們會將清單傳遞&lt;Movies.Models.Movie&gt;我們檢視範本。</span><span class="sxs-lookup"><span data-stu-id="2027a-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="2027a-117">不同於前一次，我們使用 [新增檢視] 對話方塊，並選擇要建立的 「 空白 」 範本時，我們將會指出這次我們要 Visual Studio 自動"scaffold 」 檢視範本為我們具有某些預設內容。</span><span class="sxs-lookup"><span data-stu-id="2027a-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="2027a-118">將執行此作業，選取 「 清單 」 中的項目 」 檢視內容下拉式功能表。</span><span class="sxs-lookup"><span data-stu-id="2027a-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="2027a-119">請記住，當您有一個建立新的類別，您將需要編譯您的應用程式，讓它顯示在 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="2027a-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![加入檢視](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="2027a-121">按一下 [新增]，系統會自動產生程式碼檢視為我們會顯示我們的電影清單。</span><span class="sxs-lookup"><span data-stu-id="2027a-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="2027a-122">這是變更的好時機&lt;h2&gt;標題為類似 「 我的電影清單 」，如同我們稍早與 Hello World 檢視。</span><span class="sxs-lookup"><span data-stu-id="2027a-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="2027a-123">[![影片-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2027a-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="2027a-124">執行您的應用程式，並請 /Movies 瀏覽的網址列中。</span><span class="sxs-lookup"><span data-stu-id="2027a-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="2027a-125">現在我們已使用內部控制器的基本查詢從資料庫擷取資料，並傳回的資料到電影所知道的檢視。</span><span class="sxs-lookup"><span data-stu-id="2027a-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="2027a-126">該檢視微調透過的電影清單，然後我們建立資料的資料表。</span><span class="sxs-lookup"><span data-stu-id="2027a-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="2027a-127">[![電影清單-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="2027a-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="2027a-128">我們將不會實作與這個應用程式的編輯，詳細資料與刪除功能所以我們不需要為我們 scaffold 範本建立的預設連結。</span><span class="sxs-lookup"><span data-stu-id="2027a-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="2027a-129">開啟 /Movies/Index.aspx 檔案，並將它們移除。</span><span class="sxs-lookup"><span data-stu-id="2027a-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="2027a-130">以下是我們已更新的檢視表範本外觀應該為何一旦我們進行這些變更的程式碼：</span><span class="sxs-lookup"><span data-stu-id="2027a-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="2027a-131">我們會將其刪除此範例中，它建立我們不需要的連結。</span><span class="sxs-lookup"><span data-stu-id="2027a-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="2027a-132">不過，因為這是下一步，我們會保留我們建立新的連結 ！</span><span class="sxs-lookup"><span data-stu-id="2027a-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="2027a-133">以下是我們的應用程式與移除該資料行的外觀。</span><span class="sxs-lookup"><span data-stu-id="2027a-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="2027a-134">[![電影清單-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="2027a-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="2027a-135">我們現在有我們電影資料的簡單清單。</span><span class="sxs-lookup"><span data-stu-id="2027a-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="2027a-136">不過，如果我們按一下 「 建立新 」 的連結，我們會先出現的錯誤，因為它不會接到 ！</span><span class="sxs-lookup"><span data-stu-id="2027a-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="2027a-137">讓我們實作建立動作方法，並且讓使用者在資料庫中輸入新的電影。</span><span class="sxs-lookup"><span data-stu-id="2027a-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2027a-138">[上一頁](getting-started-with-mvc-part4.md)
> [下一頁](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="2027a-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
