---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: "組件 10： 瀏覽及站台的設計，結論最後更新 |Microsoft 文件"
author: jongalloway
description: "此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 組件 10 涵蓋最後更新瀏覽和 S..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: af08039de2d810948b9ab64974111b0346c7fa0f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="daf99-104">瀏覽及站台的設計，結論的組件 10： 最後更新</span><span class="sxs-lookup"><span data-stu-id="daf99-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="daf99-105">由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="daf99-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="daf99-106">MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="daf99-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="daf99-107">MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。</span><span class="sxs-lookup"><span data-stu-id="daf99-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="daf99-108">此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="daf99-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="daf99-109">組件 10 涵蓋瀏覽及站台的設計，結論最後的更新。</span><span class="sxs-lookup"><span data-stu-id="daf99-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="daf99-110">我們已經完成的所有主要功能為我們的網站，但我們仍有一些功能，可新增至站台瀏覽、 首頁和存放區瀏覽 頁面。</span><span class="sxs-lookup"><span data-stu-id="daf99-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="daf99-111">建立購物車摘要部分檢視</span><span class="sxs-lookup"><span data-stu-id="daf99-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="daf99-112">我們想要在整個網站中公開使用者的購物車中的項目數。</span><span class="sxs-lookup"><span data-stu-id="daf99-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="daf99-113">我們可以輕鬆實作這藉由建立部分加入至我們 Site.master 檢視。</span><span class="sxs-lookup"><span data-stu-id="daf99-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="daf99-114">如先前所示，ShoppingCart 控制器包含 CartSummary 動作方法會傳回部分檢視：</span><span class="sxs-lookup"><span data-stu-id="daf99-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="daf99-115">若要建立 CartSummary 部分檢視，檢視/ShoppingCart 資料夾上按一下滑鼠右鍵並選取 [新增檢視]。</span><span class="sxs-lookup"><span data-stu-id="daf99-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="daf99-116">CartSummary 檢視的名稱，並檢查 「 建立部分檢視 」 的核取方塊，如下所示。</span><span class="sxs-lookup"><span data-stu-id="daf99-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="daf99-117">真正簡易 CartSummary 部分檢視-它是只 ShoppingCart 索引檢視中顯示的項目數購物車中的連結。</span><span class="sxs-lookup"><span data-stu-id="daf99-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="daf99-118">CartSummary.cshtml 的完整程式碼如下所示：</span><span class="sxs-lookup"><span data-stu-id="daf99-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="daf99-119">我們可以併入任何在網站中，使用 Html.RenderAction 方法包括網站主版頁面的部分檢視。</span><span class="sxs-lookup"><span data-stu-id="daf99-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="daf99-120">RenderAction 需要我們指定動作名稱 ("CartSummary") 和控制器名稱 ("ShoppingCart") 做為下面。</span><span class="sxs-lookup"><span data-stu-id="daf99-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="daf99-121">之前加入這個站台版面配置，我們也會建立內容類型功能表讓我們進行的所有我們 Site.master 更新一次。</span><span class="sxs-lookup"><span data-stu-id="daf99-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="daf99-122">建立部分檢視的內容類型 功能表</span><span class="sxs-lookup"><span data-stu-id="daf99-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="daf99-123">我們可以讓我們來瀏覽市集，藉由新增內容類型 功能表會列出所有內容類型可以使用我們的存放區中的使用者來得簡單許多。</span><span class="sxs-lookup"><span data-stu-id="daf99-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="daf99-124">我們會遵循相同步驟也會建立 GenreMenu 部分檢視，並將我們可以加入兩者主要站台。</span><span class="sxs-lookup"><span data-stu-id="daf99-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="daf99-125">首先，加入下列 GenreMenu 控制器動作 StoreController:</span><span class="sxs-lookup"><span data-stu-id="daf99-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="daf99-126">這個動作會傳回一份這會顯示由部分檢視，接下來，我們將建立的內容類型。</span><span class="sxs-lookup"><span data-stu-id="daf99-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="daf99-127">*注意： 我們加入了 [ChildActionOnly] 屬性此控制器動作，這表示我們只想要從部分檢視使用此動作。這個屬性會防止從正在執行瀏覽至 /Store/GenreMenu 控制器動作。這不是必要的部分檢視，但最好的作法是，因為我們想要確定我們想在使用我們的控制器動作。我們也會傳回 PartialView，而不是檢視，可讓檢視引擎知道它不應該使用這個檢視中，配置為包含在其他檢視中。*</span><span class="sxs-lookup"><span data-stu-id="daf99-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="daf99-128">以滑鼠右鍵按一下 GenreMenu 控制器動作，並建立名為 GenreMenu 其強型別使用的內容類型 檢視資料類別，如下所示的部分檢視。</span><span class="sxs-lookup"><span data-stu-id="daf99-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="daf99-129">更新要使用的未排序的清單，如下所示的項目顯示的 GenreMenu 部分檢視的檢視程式碼。</span><span class="sxs-lookup"><span data-stu-id="daf99-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="daf99-130">若要顯示我們的部分檢視更新的站台版面配置</span><span class="sxs-lookup"><span data-stu-id="daf99-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="daf99-131">我們可以加入我們的部分檢視的網站配置 (/檢視/共用/\_Layout.cshtml) 藉由呼叫 Html.RenderAction()。</span><span class="sxs-lookup"><span data-stu-id="daf99-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="daf99-132">我們會將兩者中，以及一些額外的標記，以顯示它們，如下所示：</span><span class="sxs-lookup"><span data-stu-id="daf99-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="daf99-133">現在當我們執行應用程式時，我們將會看到左邊的導覽區域中的內容類型和購物車摘要頂端。</span><span class="sxs-lookup"><span data-stu-id="daf99-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="daf99-134">更新資料存放區瀏覽頁</span><span class="sxs-lookup"><span data-stu-id="daf99-134">Update to the Store Browse page</span></span>

<span data-ttu-id="daf99-135">存放區瀏覽 頁面可運作，但是看起來很理想。</span><span class="sxs-lookup"><span data-stu-id="daf99-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="daf99-136">我們可以更新頁面以顯示更好的版面配置中的專輯，藉由更新檢視程式碼 （/Views/Store/Browse.cshtml 中找到），如下所示：</span><span class="sxs-lookup"><span data-stu-id="daf99-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="daf99-137">這裡我們正在使用 Url.Action，而不是 Html.ActionLink，讓我們可以套用特殊格式以包含專輯圖檔連結。</span><span class="sxs-lookup"><span data-stu-id="daf99-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="daf99-138">*注意： 我們會顯示這些相簿的泛型專輯封面。此資訊儲存在資料庫中，並透過存放區管理員可加以編輯。您可以隨意地加入您自己的作品。*</span><span class="sxs-lookup"><span data-stu-id="daf99-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="daf99-139">現在當我們瀏覽至內容類型，我們將會看到具有專輯作品的方格中顯示的相簿。</span><span class="sxs-lookup"><span data-stu-id="daf99-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="daf99-140">更新以顯示頂端銷售專輯首頁</span><span class="sxs-lookup"><span data-stu-id="daf99-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="daf99-141">我們想要的功能我們最暢銷的專輯在首頁上，來增加銷售量。</span><span class="sxs-lookup"><span data-stu-id="daf99-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="daf99-142">我們要處理，並新增一些其他的圖形以及我們 HomeController 進行部分更新。</span><span class="sxs-lookup"><span data-stu-id="daf99-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="daf99-143">首先，我們會加入導覽屬性專輯類別以便 EntityFramework 知道它們的相關聯。</span><span class="sxs-lookup"><span data-stu-id="daf99-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="daf99-144">最後幾行程式我們**專輯**類別現在看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="daf99-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="daf99-145">*注意： 這將需要加入 using System.Collections.Generic 命名空間中的陳述式。*</span><span class="sxs-lookup"><span data-stu-id="daf99-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="daf99-146">首先，我們會將 storeDB 欄位以及 MvcMusicStore.Models using 陳述式，如同我們控制站。</span><span class="sxs-lookup"><span data-stu-id="daf99-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="daf99-147">接下來，我們會將下列方法加入 HomeController 會查詢資料庫以尋找 OrderDetails 根據最上層銷售的相簿。</span><span class="sxs-lookup"><span data-stu-id="daf99-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="daf99-148">因為我們不想要使它成為控制器動作，這是私用方法。</span><span class="sxs-lookup"><span data-stu-id="daf99-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="daf99-149">我們為了簡單起見，HomeController 中加入它，但建議您使用成個別的服務類別，適當地移動您的商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="daf99-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="daf99-150">具有該位置中，我們可以更新查詢前 5 項暢銷相簿，並將其傳回檢視的索引控制器動作。</span><span class="sxs-lookup"><span data-stu-id="daf99-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="daf99-151">更新 HomeController 的完整程式碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="daf99-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="daf99-152">最後，我們需要更新我們的首頁索引檢視，使它可以顯示一份專輯更新的模型型別，並新增專輯清單底部。</span><span class="sxs-lookup"><span data-stu-id="daf99-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="daf99-153">我們將利用這個機會也新增至頁面的標題和升級一節。</span><span class="sxs-lookup"><span data-stu-id="daf99-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="daf99-154">現在當我們執行應用程式時，我們會看到我們更新的首頁上方銷售專輯和我們促銷的訊息。</span><span class="sxs-lookup"><span data-stu-id="daf99-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="daf99-155">結論</span><span class="sxs-lookup"><span data-stu-id="daf99-155">Conclusion</span></span>

<span data-ttu-id="daf99-156">我們已看到，該 ASP.NET MVC 可讓您輕鬆建立複雜的網站與資料庫存取權，成員資格、 AJAX 等。</span><span class="sxs-lookup"><span data-stu-id="daf99-156">We've seen that that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="daf99-157">很快。</span><span class="sxs-lookup"><span data-stu-id="daf99-157">pretty quickly.</span></span> <span data-ttu-id="daf99-158">希望本教學課程已提供您要開始建置您自己的 ASP.NET MVC 應用程式所需的工具 ！</span><span class="sxs-lookup"><span data-stu-id="daf99-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


>[!div class="step-by-step"]
[<span data-ttu-id="daf99-159">上一步</span><span class="sxs-lookup"><span data-stu-id="daf99-159">Previous</span></span>](mvc-music-store-part-9.md)
