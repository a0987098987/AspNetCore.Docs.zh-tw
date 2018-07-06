---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: 檢查 ASP.NET MVC 如何 scaffold DropDownList 協助程式 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 8e407d5426ed93388b3c6f7bdb125be5356c002c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837126"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="5747c-102">檢查 ASP.NET MVC 如何 scaffold DropDownList 協助程式</span><span class="sxs-lookup"><span data-stu-id="5747c-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="5747c-103">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="5747c-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="5747c-104">在 **方案總管**，以滑鼠右鍵按一下*控制站*資料夾，然後選取**新增控制器**。</span><span class="sxs-lookup"><span data-stu-id="5747c-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="5747c-105">控制器**StoreManagerController**。</span><span class="sxs-lookup"><span data-stu-id="5747c-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="5747c-106">設定的選項**新增控制器**如下圖所示的對話方塊。</span><span class="sxs-lookup"><span data-stu-id="5747c-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="5747c-107">編輯*StoreManager\Index.cshtml*檢視和移除`AlbumArtUrl`。</span><span class="sxs-lookup"><span data-stu-id="5747c-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="5747c-108">移除`AlbumArtUrl`會讓呈現更容易閱讀。</span><span class="sxs-lookup"><span data-stu-id="5747c-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="5747c-109">已完成的程式碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="5747c-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="5747c-110">開啟*Controllers\StoreManagerController.cs*檔案，並尋找`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="5747c-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="5747c-111">新增`OrderBy`子句，將價格依排序的相簿。</span><span class="sxs-lookup"><span data-stu-id="5747c-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="5747c-112">完整的程式碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="5747c-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="5747c-113">價格依排序會讓您更輕鬆地測試資料庫的變更。</span><span class="sxs-lookup"><span data-stu-id="5747c-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="5747c-114">當您要測試的編輯，並建立方法時，您可以使用較低的價格，因此已儲存的資料會出現在第一次。</span><span class="sxs-lookup"><span data-stu-id="5747c-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="5747c-115">開啟*StoreManager\Edit.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="5747c-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="5747c-116">下面這一行只之後加入圖例標籤。</span><span class="sxs-lookup"><span data-stu-id="5747c-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="5747c-117">下列程式碼顯示這項變更的內容：</span><span class="sxs-lookup"><span data-stu-id="5747c-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="5747c-118">`AlbumId` ，才可變更的專輯記錄。</span><span class="sxs-lookup"><span data-stu-id="5747c-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="5747c-119">按 CTRL+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="5747c-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="5747c-120">選取即可**系統管理員**連結，然後選取**建立新**連結，以建立新的相簿。</span><span class="sxs-lookup"><span data-stu-id="5747c-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="5747c-121">請確認已儲存的專輯資訊。</span><span class="sxs-lookup"><span data-stu-id="5747c-121">Verify the album information was saved.</span></span> <span data-ttu-id="5747c-122">編輯專輯，並確認您所做的變更會保存。</span><span class="sxs-lookup"><span data-stu-id="5747c-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="5747c-123">專輯結構描述</span><span class="sxs-lookup"><span data-stu-id="5747c-123">The Album Schema</span></span>

<span data-ttu-id="5747c-124">`StoreManager` MVC scaffolding 機制所建立的控制站將允許相簿的 CRUD （Create、 Read、 Update、 Delete） 存取 「 音樂市集 」 資料庫中。</span><span class="sxs-lookup"><span data-stu-id="5747c-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="5747c-125">專輯資訊的結構描述如下所示：</span><span class="sxs-lookup"><span data-stu-id="5747c-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="5747c-126">`Albums`相簿的內容類型和描述不會儲存資料表，其中存放了外部索引鍵`Genres`資料表。</span><span class="sxs-lookup"><span data-stu-id="5747c-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="5747c-127">`Genres`資料表包含的內容類型名稱和描述。</span><span class="sxs-lookup"><span data-stu-id="5747c-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="5747c-128">同樣地，`Albums`專輯演出者名稱，但的外部索引鍵，不包含資料表`Artists`資料表。</span><span class="sxs-lookup"><span data-stu-id="5747c-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="5747c-129">`Artists`資料表包含演出者的名稱。</span><span class="sxs-lookup"><span data-stu-id="5747c-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="5747c-130">如果您檢查中的資料`Albums`資料表中，您可以看到每個資料列包含外部索引鍵`Genres`資料表和外部索引鍵`Artists`資料表。</span><span class="sxs-lookup"><span data-stu-id="5747c-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="5747c-131">下圖顯示一些資料表資料從`Albums`資料表。</span><span class="sxs-lookup"><span data-stu-id="5747c-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="5747c-132">HTML 標記</span><span class="sxs-lookup"><span data-stu-id="5747c-132">The HTML Select Tag</span></span>

<span data-ttu-id="5747c-133">HTML`<select>`項目 (由 HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)協助程式) 用來顯示值 （例如內容類型清單） 的完整清單。</span><span class="sxs-lookup"><span data-stu-id="5747c-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="5747c-134">編輯表單時已知的目前值，則選取清單可以顯示目前的值。</span><span class="sxs-lookup"><span data-stu-id="5747c-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="5747c-135">我們先前所見此當我們選取的值設定為**喜劇**。</span><span class="sxs-lookup"><span data-stu-id="5747c-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="5747c-136">選取的清單很適合用於顯示類別目錄或外部索引鍵資料。</span><span class="sxs-lookup"><span data-stu-id="5747c-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="5747c-137">`<select>`內容類型的外部索引鍵的項目會顯示可能的內容類型名稱的清單，但當您儲存表單的內容類型屬性已包含內容類型外部索引鍵值，而不是顯示的內容類型名稱。</span><span class="sxs-lookup"><span data-stu-id="5747c-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="5747c-138">下圖中所選取的內容類型是**Disco**演出者，而且**Donna 夏天**。</span><span class="sxs-lookup"><span data-stu-id="5747c-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="5747c-139">檢查 ASP.NET MVC 包含 Scaffold 的程式碼</span><span class="sxs-lookup"><span data-stu-id="5747c-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="5747c-140">開啟*Controllers\StoreManagerController.cs*檔案，並尋找`HTTP GET Create`方法。</span><span class="sxs-lookup"><span data-stu-id="5747c-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="5747c-141">`Create`方法會將兩個[SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx)物件至`ViewBag`，其中包含內容類型的資訊，另一種包含演出者資訊。</span><span class="sxs-lookup"><span data-stu-id="5747c-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="5747c-142">[SelectList](https://msdn.microsoft.com/library/dd505286.aspx)上面使用的建構函式多載會採用三個引數：</span><span class="sxs-lookup"><span data-stu-id="5747c-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="5747c-143">*項目*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx)包含在清單中的項目。</span><span class="sxs-lookup"><span data-stu-id="5747c-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="5747c-144">在上述範例中，內容類型的清單傳回`db.Genres`。</span><span class="sxs-lookup"><span data-stu-id="5747c-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="5747c-145">*dataValueField*： 在屬性名稱**IEnumerable**清單，其中包含索引鍵的值。</span><span class="sxs-lookup"><span data-stu-id="5747c-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="5747c-146">在上述範例`GenreId`和`ArtistId`。</span><span class="sxs-lookup"><span data-stu-id="5747c-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="5747c-147">*dataTextField*： 在屬性名稱**IEnumerable**清單，其中包含要顯示的資訊。</span><span class="sxs-lookup"><span data-stu-id="5747c-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="5747c-148">演出者和內容類型表格中`name`使用欄位。</span><span class="sxs-lookup"><span data-stu-id="5747c-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="5747c-149">開啟*Views\StoreManager\Create.cshtml*檔案，並檢查`Html.DropDownList`協助程式類型欄位的標記。</span><span class="sxs-lookup"><span data-stu-id="5747c-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="5747c-150">第一行會顯示建立檢視採用`Album`模型。</span><span class="sxs-lookup"><span data-stu-id="5747c-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="5747c-151">在 `Create`方法如上所示，不傳遞任何模型，讓檢視取得**null** `Album`模型。</span><span class="sxs-lookup"><span data-stu-id="5747c-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="5747c-152">此時，我們會建立新專輯因此我們沒有任何`Album`為它的資料。</span><span class="sxs-lookup"><span data-stu-id="5747c-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="5747c-153">[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)如上所示的多載會採用要繫結至模型之欄位的名稱。</span><span class="sxs-lookup"><span data-stu-id="5747c-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="5747c-154">它也會使用此名稱來尋找**ViewBag**物件，包含[SelectList](https://msdn.microsoft.com/library/dd505286.aspx)物件。</span><span class="sxs-lookup"><span data-stu-id="5747c-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="5747c-155">使用這個多載，您都必須名稱**ViewBag SelectList**物件`GenreId`。</span><span class="sxs-lookup"><span data-stu-id="5747c-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="5747c-156">第二個參數 (`String.Empty`) 是用來在不選取任何項目時顯示文字。</span><span class="sxs-lookup"><span data-stu-id="5747c-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="5747c-157">這正是我們要建立新專輯時。</span><span class="sxs-lookup"><span data-stu-id="5747c-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="5747c-158">如果您移除第二個參數，並使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="5747c-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="5747c-159">選取清單會預設為第一個項目或 Rock，在我們的範例。</span><span class="sxs-lookup"><span data-stu-id="5747c-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="5747c-160">檢查`HTTP POST Create`方法。</span><span class="sxs-lookup"><span data-stu-id="5747c-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="5747c-161">這個多載`Create`方法會採用`album`張貼的表單值的 ASP.NET MVC 模型繫結系統所建立的物件。</span><span class="sxs-lookup"><span data-stu-id="5747c-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="5747c-162">當您送出新的相簿中，如果模型狀態無效，而且沒有任何資料庫錯誤時，新的專輯新增資料庫。</span><span class="sxs-lookup"><span data-stu-id="5747c-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="5747c-163">下圖顯示建立新的相簿。</span><span class="sxs-lookup"><span data-stu-id="5747c-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="5747c-164">您可以使用[fiddler 工具](http://www.fiddler2.com/fiddler2/)來檢查已張貼的表單值 ASP.NET MVC 模型繫結會使用建立專輯物件。</span><span class="sxs-lookup"><span data-stu-id="5747c-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="5747c-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="5747c-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="5747c-166">重構 ViewBag SelectList 建立</span><span class="sxs-lookup"><span data-stu-id="5747c-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="5747c-167">這兩個`Edit`方法和`HTTP POST Create`方法有相同的程式碼，以設定**SelectList**中**ViewBag**。</span><span class="sxs-lookup"><span data-stu-id="5747c-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="5747c-168">精神[DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself)，我們將會重構此程式碼。</span><span class="sxs-lookup"><span data-stu-id="5747c-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="5747c-169">我們要使用這個重構程式碼更新版本。</span><span class="sxs-lookup"><span data-stu-id="5747c-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="5747c-170">建立新的方法來新增內容類型與演出者**SelectList**要**ViewBag**。</span><span class="sxs-lookup"><span data-stu-id="5747c-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="5747c-171">取代設定的兩個線條`ViewBag`中每個`Create`並`Edit`方法呼叫`SetGenreArtistViewBag`方法。</span><span class="sxs-lookup"><span data-stu-id="5747c-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="5747c-172">已完成的程式碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="5747c-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="5747c-173">建立新的專輯和編輯以確認工作變更專輯。</span><span class="sxs-lookup"><span data-stu-id="5747c-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="5747c-174">明確地將 SelectList 傳遞至 DropDownList</span><span class="sxs-lookup"><span data-stu-id="5747c-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="5747c-175">建立 ASP.NET MVC scaffolding 使用下列的建立] 和 [編輯檢視**DropDownList**多載：</span><span class="sxs-lookup"><span data-stu-id="5747c-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="5747c-176">`DropDownList`標記建立檢視如下所示。</span><span class="sxs-lookup"><span data-stu-id="5747c-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="5747c-177">因為`ViewBag`屬性`SelectList`稱為`GenreId`，則**DropDownList**協助程式會使用`GenreId` **SelectList**中**ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="5747c-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="5747c-178">在下列**DropDownList**多載，`SelectList`明確傳遞。</span><span class="sxs-lookup"><span data-stu-id="5747c-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="5747c-179">開啟*Views\StoreManager\Edit.cshtml*檔案，然後變更**DropDownList**呼叫中明確地傳遞**SelectList**，使用上述的多載。</span><span class="sxs-lookup"><span data-stu-id="5747c-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="5747c-180">執行這項操作的內容類型分類。</span><span class="sxs-lookup"><span data-stu-id="5747c-180">Do this for the Genre category.</span></span> <span data-ttu-id="5747c-181">已完成的程式碼如下所示：</span><span class="sxs-lookup"><span data-stu-id="5747c-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="5747c-182">執行應用程式，然後按一下**系統管理員**連結，然後瀏覽至 Jazz 專輯，並選取**編輯**連結。</span><span class="sxs-lookup"><span data-stu-id="5747c-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="5747c-183">而是顯示 Jazz 目前選取的內容類型為 Rock 隨即出現。</span><span class="sxs-lookup"><span data-stu-id="5747c-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="5747c-184">當字串引數 （要繫結的屬性） 和**SelectList**物件具有相同的名稱，不會使用選取的值。</span><span class="sxs-lookup"><span data-stu-id="5747c-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="5747c-185">中的第一個項目未提供的選取的值時，預設瀏覽器**SelectList**(即**Rock**在上述範例中)。</span><span class="sxs-lookup"><span data-stu-id="5747c-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="5747c-186">這是已知的限制**DropDownList**協助程式。</span><span class="sxs-lookup"><span data-stu-id="5747c-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="5747c-187">開啟*Controllers\StoreManagerController.cs*檔案，並變更**SelectList**物件名稱來`Genres`和`Artists`。</span><span class="sxs-lookup"><span data-stu-id="5747c-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="5747c-188">已完成的程式碼如下所示：</span><span class="sxs-lookup"><span data-stu-id="5747c-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="5747c-189">內容類型和演出者名稱是類別，更好的名稱，因為它們包含不只是每個類別目錄的識別碼。</span><span class="sxs-lookup"><span data-stu-id="5747c-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="5747c-190">重構的稍早執行過了成果。</span><span class="sxs-lookup"><span data-stu-id="5747c-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="5747c-191">而不是變更**ViewBag**四種方法，在我們的變更已隔離至`SetGenreArtistViewBag`方法。</span><span class="sxs-lookup"><span data-stu-id="5747c-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="5747c-192">變更**DropDownList**呼叫中建立和編輯檢視，以使用新**SelectList**名稱。</span><span class="sxs-lookup"><span data-stu-id="5747c-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="5747c-193">[編輯] 檢視的新標記如下所示：</span><span class="sxs-lookup"><span data-stu-id="5747c-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="5747c-194">建立檢視需要空字串，以防止 SelectList 中的第一個項目顯示。</span><span class="sxs-lookup"><span data-stu-id="5747c-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="5747c-195">建立新的專輯和編輯以確認工作變更專輯。</span><span class="sxs-lookup"><span data-stu-id="5747c-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="5747c-196">測試編輯程式碼，藉由選取與內容類型 Rock 以外的專輯。</span><span class="sxs-lookup"><span data-stu-id="5747c-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="5747c-197">使用 DropDownList 協助程式中的檢視模型</span><span class="sxs-lookup"><span data-stu-id="5747c-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="5747c-198">建立新的類別中名為的 Viewmodel 資料夾`AlbumSelectListViewModel`。</span><span class="sxs-lookup"><span data-stu-id="5747c-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="5747c-199">中的程式碼取代`AlbumSelectListViewModel`取代為下列類別：</span><span class="sxs-lookup"><span data-stu-id="5747c-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="5747c-200">`AlbumSelectListViewModel`建構函式會專輯、 演出者和內容類型的清單，並建立物件，其中包含專輯和`SelectList`的內容類型和演出者名稱。</span><span class="sxs-lookup"><span data-stu-id="5747c-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="5747c-201">建置專案因此`AlbumSelectListViewModel`時才可以使用我們在下一個步驟中建立檢視。</span><span class="sxs-lookup"><span data-stu-id="5747c-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="5747c-202">新增`EditVM`方法，以`StoreManagerController`。</span><span class="sxs-lookup"><span data-stu-id="5747c-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="5747c-203">已完成的程式碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="5747c-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="5747c-204">以滑鼠右鍵按一下`AlbumSelectListViewModel`，選取**解決**，然後**使用 MvcMusicStore.ViewModels;**。</span><span class="sxs-lookup"><span data-stu-id="5747c-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="5747c-205">或者，您可以在其中新增下列 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="5747c-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="5747c-206">以滑鼠右鍵按一下`EditVM`，然後選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="5747c-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="5747c-207">使用如下所示的選項。</span><span class="sxs-lookup"><span data-stu-id="5747c-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="5747c-208">選取 **新增**，然後取代內容*Views\StoreManager\EditVM.cshtml*以下列檔案：</span><span class="sxs-lookup"><span data-stu-id="5747c-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="5747c-209">`EditVM`標記是非常類似於原始`Edit`標記，但有下列例外狀況。</span><span class="sxs-lookup"><span data-stu-id="5747c-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="5747c-210">模型中的屬性`Edit`檢視會在表單`model.property`(比方說， `model.Title` )。</span><span class="sxs-lookup"><span data-stu-id="5747c-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="5747c-211">模型中的屬性`EditVm`檢視會在表單`model.Album.property`(比方說， `model.Album.Title`)。</span><span class="sxs-lookup"><span data-stu-id="5747c-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="5747c-212">這是因為`EditVM`檢視會傳遞一個容器進行`Album`，而非`Album`示`Edit`檢視。</span><span class="sxs-lookup"><span data-stu-id="5747c-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="5747c-213">**DropDownList**第二個參數不是檢視模型中，來自**ViewBag**。</span><span class="sxs-lookup"><span data-stu-id="5747c-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="5747c-214">**BeginForm**中的 helper`EditVM`檢視明確回傳至`Edit`動作方法。</span><span class="sxs-lookup"><span data-stu-id="5747c-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="5747c-215">藉由公佈回到`Edit`動作，我們不需要撰寫`HTTP POST EditVM`動作，並可以重複使用`HTTP POST``Edit`動作。</span><span class="sxs-lookup"><span data-stu-id="5747c-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="5747c-216">執行應用程式，並編輯 album。</span><span class="sxs-lookup"><span data-stu-id="5747c-216">Run the application and edit an album.</span></span> <span data-ttu-id="5747c-217">將 URL 變更為使用`EditVM`。</span><span class="sxs-lookup"><span data-stu-id="5747c-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="5747c-218">變更的欄位，然後按**儲存** 按鈕，確認程式碼是否正常運作。</span><span class="sxs-lookup"><span data-stu-id="5747c-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="5747c-219">您應該使用哪種方法？</span><span class="sxs-lookup"><span data-stu-id="5747c-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="5747c-220">顯示的三個方法都是不得。</span><span class="sxs-lookup"><span data-stu-id="5747c-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="5747c-221">許多開發人員偏好 explictily pass`SelectList`要`DropDownList`使用`ViewBag`。</span><span class="sxs-lookup"><span data-stu-id="5747c-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="5747c-222">這個方法有新增的好處，是讓您彈性地使用更適當的名稱集合。</span><span class="sxs-lookup"><span data-stu-id="5747c-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="5747c-223">一個需要注意的是您不能命名`ViewBag SelectList`物件模型的屬性名稱相同。</span><span class="sxs-lookup"><span data-stu-id="5747c-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="5747c-224">有些開發人員偏好使用 ViewModel 方法。</span><span class="sxs-lookup"><span data-stu-id="5747c-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="5747c-225">其他人請考慮更詳細的標記，並產生 HTML ViewModel 方法的缺點。</span><span class="sxs-lookup"><span data-stu-id="5747c-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="5747c-226">在本節中我們已了解使用三種方法**DropDownList**與類別目錄資料。</span><span class="sxs-lookup"><span data-stu-id="5747c-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="5747c-227">在下一步 區段中，我們將示範如何新增新的類別。</span><span class="sxs-lookup"><span data-stu-id="5747c-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5747c-228">[上一頁](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [下一頁](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="5747c-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
