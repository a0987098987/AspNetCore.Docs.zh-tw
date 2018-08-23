---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: 將新類別新增至 DropDownList 使用 jQuery UI |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 9a893bf55070adde575d223cd527b02d6f0d470c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831779"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="be5f2-102">將新類別新增至 DropDownList 使用 jQuery UI</span><span class="sxs-lookup"><span data-stu-id="be5f2-102">Adding a New Category to the DropDownList using jQuery UI</span></span>
====================
<span data-ttu-id="be5f2-103">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="be5f2-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="be5f2-104">HTML`Select`標記是適合用來呈現一份固定的類別目錄資料，但通常您需要新增新的類別。</span><span class="sxs-lookup"><span data-stu-id="be5f2-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="be5f2-105">假設我們想要在資料庫中的類別中加入內容類型 」 Opera 」？</span><span class="sxs-lookup"><span data-stu-id="be5f2-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="be5f2-106">在本節中，我們將使用的 jQuery UI 加入對話方塊中，我們可以使用來新增新的類別。</span><span class="sxs-lookup"><span data-stu-id="be5f2-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="be5f2-107">下圖顯示如何在 UI 將會顯示在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="be5f2-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="be5f2-108">當使用者選取**加入新的內容類型**連結，快顯對話方塊提示使用者輸入新的內容類型名稱 （並選擇性輸入說明）。</span><span class="sxs-lookup"><span data-stu-id="be5f2-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="be5f2-109">下的圖顯示**新增的內容類型**快顯對話方塊。</span><span class="sxs-lookup"><span data-stu-id="be5f2-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="be5f2-110">輸入新的內容類型名稱時，**儲存**按鈕推送時，會發生下列情況：</span><span class="sxs-lookup"><span data-stu-id="be5f2-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="be5f2-111">AJAX 呼叫張貼的內容類型的控制站，這會將新的內容類型儲存至資料庫，並傳回新內容類型的資訊 （「 內容類型名稱 」 和 「 識別碼 」） 為 JSON 的 Create 方法的資料。</span><span class="sxs-lookup"><span data-stu-id="be5f2-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="be5f2-112">JavaScript 會將選取清單中的新內容類型的資料。</span><span class="sxs-lookup"><span data-stu-id="be5f2-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="be5f2-113">JavaScript 可讓新的內容類型選取的項目。</span><span class="sxs-lookup"><span data-stu-id="be5f2-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="be5f2-114">在下圖中， **Opera**已加入至資料庫，而在中選取**內容類型**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="be5f2-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="be5f2-115">開啟*Views\StoreManager\Create.cshtml*檔案，並取代為下列內容類型標記的下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="be5f2-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="be5f2-116">`_ChooseGenre`部分檢視會包含連結的 JavaScript 和 jQuery 用來實作新增新的內容類型功能的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="be5f2-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="be5f2-117">當我們完成後就會輕鬆地執行相同的演出者 UI 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="be5f2-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="be5f2-118">在 [方案總管] 中，以滑鼠右鍵按一下*Views\StoreManager*資料夾，然後選取**新增**，然後**檢視**。</span><span class="sxs-lookup"><span data-stu-id="be5f2-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="be5f2-119">在 **檢視表名稱**輸入、 輸入`_ChooseGenre`然後選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="be5f2-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="be5f2-120">取代中的標記*Views\StoreManager\\_ChooseGenre.cshtml*以下列檔案：</span><span class="sxs-lookup"><span data-stu-id="be5f2-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="be5f2-121">第一行會宣告我們傳入`Album`為我們的模型，完全相同模型中的 Create view 陳述式。</span><span class="sxs-lookup"><span data-stu-id="be5f2-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="be5f2-122">接下來的幾行會都**標籤**標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="be5f2-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="be5f2-123">下一行是**DropDownList**協助程式呼叫時，原始的 Create view 完全相同。</span><span class="sxs-lookup"><span data-stu-id="be5f2-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="be5f2-124">下一行加入名為連結`Add New Genre`，並設定它像按鈕的樣式。</span><span class="sxs-lookup"><span data-stu-id="be5f2-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="be5f2-125">包含的列`ValidationMessageFor`複製直接來自建立檢視。</span><span class="sxs-lookup"><span data-stu-id="be5f2-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="be5f2-126">下列幾行：</span><span class="sxs-lookup"><span data-stu-id="be5f2-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="be5f2-127">會隱藏的 div 中，建立識別碼為`genreDialog`。</span><span class="sxs-lookup"><span data-stu-id="be5f2-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="be5f2-128">我們會使用 jQuery 來連結我們**新增的內容類型**對話方塊中，識別碼`genreDialog`中這個 div。</span><span class="sxs-lookup"><span data-stu-id="be5f2-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="be5f2-129">最後兩個指令碼標記包含我們將用來實作新增新的內容類型功能的 JavaScript 檔案連結。</span><span class="sxs-lookup"><span data-stu-id="be5f2-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="be5f2-130">*/Scripts/chooseGenre.js*檔案是提供為您專案中，我們將檢視在教學課程稍後它。</span><span class="sxs-lookup"><span data-stu-id="be5f2-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="be5f2-131">執行應用程式，然後按一下**加入新的內容類型** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="be5f2-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="be5f2-132">在 **新增的內容類型**對話方塊方塊中，輸入**Opera**中**名稱**輸入的方塊。</span><span class="sxs-lookup"><span data-stu-id="be5f2-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="be5f2-133">按一下 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="be5f2-133">Click the **Save** button.</span></span> <span data-ttu-id="be5f2-134">AJAX 呼叫建立 Opera 類別然後填入下拉式清單和 Opera，並將 Opera 設定為所選取的內容類型。</span><span class="sxs-lookup"><span data-stu-id="be5f2-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="be5f2-135">輸入演出者、 標題和價格，然後選取**建立** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="be5f2-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="be5f2-136">如果您輸入少於 8.99 美金的價格，新的相簿會出現在頂端的 [索引] 檢視中。</span><span class="sxs-lookup"><span data-stu-id="be5f2-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="be5f2-137">確認新的專輯項目已儲存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="be5f2-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="be5f2-138">嘗試建立新的內容類型只能有一個字母。</span><span class="sxs-lookup"><span data-stu-id="be5f2-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="be5f2-139">下列程式碼中*Models\Genre.cs*檔案設定的內容類型名稱的最小和最大長度。</span><span class="sxs-lookup"><span data-stu-id="be5f2-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="be5f2-140">用戶端驗證會報告您必須輸入介於 2 到 20 個字元的字串。</span><span class="sxs-lookup"><span data-stu-id="be5f2-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="be5f2-141">檢查方式的新內容類型加入至資料庫，以及選取清單中。</span><span class="sxs-lookup"><span data-stu-id="be5f2-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="be5f2-142">開啟*Scripts\chooseGenre.js*檔案，並檢查程式碼。</span><span class="sxs-lookup"><span data-stu-id="be5f2-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="be5f2-143">第二行則會使用識別碼`genreDialog`若要建立對話方塊上的 div 標記*Views\StoreManager\\_ChooseGenre.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="be5f2-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="be5f2-144">具名參數的大部分都是自我說明。</span><span class="sxs-lookup"><span data-stu-id="be5f2-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="be5f2-145">`autoOpen`參數設為 false，選取**建立的內容類型** 按鈕會明確地開啟的對話方塊 （這說明後者上）。</span><span class="sxs-lookup"><span data-stu-id="be5f2-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="be5f2-146">對話方塊有兩個按鈕**儲存**並**取消**。</span><span class="sxs-lookup"><span data-stu-id="be5f2-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="be5f2-147">**取消**按鈕關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="be5f2-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="be5f2-148">下列程式碼示範**儲存**按鈕函式。</span><span class="sxs-lookup"><span data-stu-id="be5f2-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="be5f2-149">`var createGenreForm`從選取`createGenreForm`識別碼。</span><span class="sxs-lookup"><span data-stu-id="be5f2-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="be5f2-150">`createGenreForm`中找到下列程式碼中設定的識別碼*Views\Genre\\_CreateGenre.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="be5f2-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="be5f2-151">[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx)中所使用的 helper 多載*Views\Genre\\_CreateGenre.cshtml*檔案會產生與包含 URL 來送出表單的 action 屬性的 HTML。</span><span class="sxs-lookup"><span data-stu-id="be5f2-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="be5f2-152">您可以看到這個瀏覽器中顯示建立的 專輯 頁面，然後選取 瀏覽器中的 顯示來源。</span><span class="sxs-lookup"><span data-stu-id="be5f2-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="be5f2-153">下列標記會顯示包含表單標記產生的 HTML。</span><span class="sxs-lookup"><span data-stu-id="be5f2-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="be5f2-154">JQuery`$.post`一行可讓動作屬性的 AJAX 呼叫 (`/StoreManager/Create`)，並傳入的資料**建立的內容類型** 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="be5f2-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="be5f2-155">資料是由新的內容類型和選擇性的描述名稱所組成。</span><span class="sxs-lookup"><span data-stu-id="be5f2-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="be5f2-156">AJAX 呼叫成功時，新的內容類型名稱和值加入選取的標記，與新的內容類型設定為選取的值。</span><span class="sxs-lookup"><span data-stu-id="be5f2-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="be5f2-157">因為這是動態產生的標記，您無法看到新選取的選項，藉由瀏覽器中檢視的來源。</span><span class="sxs-lookup"><span data-stu-id="be5f2-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="be5f2-158">您可以看到新的 HTML 與 IE 9 F12 開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="be5f2-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="be5f2-159">若要檢視新的選取選項，在 Internet Explorer 9 中，按 F12 鍵以啟動 F12 開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="be5f2-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="be5f2-160">瀏覽至 [建立] 頁面，並加入新的內容類型，因此在內容類型選取清單中選取新的內容類型。</span><span class="sxs-lookup"><span data-stu-id="be5f2-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="be5f2-161">在 F12 開發人員工具：</span><span class="sxs-lookup"><span data-stu-id="be5f2-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="be5f2-162">選取 [HTML] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="be5f2-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="be5f2-163">叫用重新整理圖示。</span><span class="sxs-lookup"><span data-stu-id="be5f2-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="be5f2-164">在 [搜尋] 方塊中，輸入 GenreID。</span><span class="sxs-lookup"><span data-stu-id="be5f2-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="be5f2-165">使用 [下一步] 圖示，</span><span class="sxs-lookup"><span data-stu-id="be5f2-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="be5f2-166">瀏覽至下列選取的標記：</span><span class="sxs-lookup"><span data-stu-id="be5f2-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="be5f2-167">展開的最後一個選項值。</span><span class="sxs-lookup"><span data-stu-id="be5f2-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="be5f2-168">下列程式碼中*Scripts\chooseGenre.js*檔案會顯示如何**新增新的內容類型** 按鈕取得連線至 click 事件，以及如何**加入新的內容類型**對話方塊建立。</span><span class="sxs-lookup"><span data-stu-id="be5f2-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="be5f2-169">第一行會建立附加至按一下函式**加入新的內容類型** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="be5f2-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="be5f2-170">下列標記從 Views\StoreManager\\_ChooseGenre.cshtml 檔案顯示如何**加入新的內容類型**按鈕建立：</span><span class="sxs-lookup"><span data-stu-id="be5f2-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="be5f2-171">Load 方法會建立並開啟 [新增內容類型] 對話方塊並呼叫 jQuery`parse`方法，讓用戶端驗證，就會發生在對話方塊中輸入的資料。</span><span class="sxs-lookup"><span data-stu-id="be5f2-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="be5f2-172">在本節中，您已經學會如何建立可用來在選取清單中加入新的類別目錄資料 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="be5f2-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="be5f2-173">您可以依照相同的程序建立的演出者選取清單中加入新的演出者的 UI。</span><span class="sxs-lookup"><span data-stu-id="be5f2-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="be5f2-174">本教學課程提供使用 ASP.NET MVC 的 HTML helper 的總覽**DropDownList**。</span><span class="sxs-lookup"><span data-stu-id="be5f2-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="be5f2-175">如需有關使用所需的詳細資訊**DropDownList**，請參閱 [新增參考] 區段下方。</span><span class="sxs-lookup"><span data-stu-id="be5f2-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="be5f2-176">請讓我們知道是否已有幫助本教學課程。</span><span class="sxs-lookup"><span data-stu-id="be5f2-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="be5f2-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="be5f2-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="be5f2-178">其他參考資料</span><span class="sxs-lookup"><span data-stu-id="be5f2-178">Additional References</span></span>

- <span data-ttu-id="be5f2-179">[ASP.NET MVC – 階層式下拉式清單中列出的教學課程](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx)由[Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="be5f2-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="be5f2-180">[所選](http://harvesthq.github.com/chosen/)JavaScript 外掛程式，可支援多重選取和篩選。</span><span class="sxs-lookup"><span data-stu-id="be5f2-180">[Chosen](http://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="be5f2-181">Contributors</span><span class="sxs-lookup"><span data-stu-id="be5f2-181">Contributors</span></span>

- [<span data-ttu-id="be5f2-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="be5f2-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="be5f2-183">Jean Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="be5f2-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="be5f2-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="be5f2-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="be5f2-185">檢閱者</span><span class="sxs-lookup"><span data-stu-id="be5f2-185">Reviewers</span></span>

- <span data-ttu-id="be5f2-186">Jean Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="be5f2-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="be5f2-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="be5f2-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="be5f2-188">Mike 主教</span><span class="sxs-lookup"><span data-stu-id="be5f2-188">Mike Pope</span></span>
- <span data-ttu-id="be5f2-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="be5f2-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="be5f2-190">上一步</span><span class="sxs-lookup"><span data-stu-id="be5f2-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
