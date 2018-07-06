---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 第 9 節： 註冊和簽出 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 9 涵蓋註冊和簽出。
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e521e30cb820d834d57c87538b869861a536657b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812869"
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="50dbb-104">第 9 節： 註冊和簽出</span><span class="sxs-lookup"><span data-stu-id="50dbb-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="50dbb-105">藉由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="50dbb-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="50dbb-106">MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="50dbb-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="50dbb-107">MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。</span><span class="sxs-lookup"><span data-stu-id="50dbb-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="50dbb-108">本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="50dbb-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="50dbb-109">第 9 涵蓋註冊和簽出。</span><span class="sxs-lookup"><span data-stu-id="50dbb-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="50dbb-110">在本節中，我們將建立 CheckoutController 來收集購物者的地址和付款資訊。</span><span class="sxs-lookup"><span data-stu-id="50dbb-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="50dbb-111">我們將需要向我們的網站前簽出，所以此控制器將會需要授權的使用者。</span><span class="sxs-lookup"><span data-stu-id="50dbb-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="50dbb-112">使用者會瀏覽至結帳程序從購物車 」 結帳 」 按鈕，即可。</span><span class="sxs-lookup"><span data-stu-id="50dbb-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="50dbb-113">如果使用者未登入中，將會提示他們。</span><span class="sxs-lookup"><span data-stu-id="50dbb-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="50dbb-114">成功登入，使用者則是顯示的地址和付款的檢視。</span><span class="sxs-lookup"><span data-stu-id="50dbb-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="50dbb-115">一旦他們已填滿表單，並提交訂單，它們會顯示 [訂單確認] 畫面。</span><span class="sxs-lookup"><span data-stu-id="50dbb-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="50dbb-116">嘗試檢視不存在順序或不屬於您的訂單會顯示 [錯誤] 檢視。</span><span class="sxs-lookup"><span data-stu-id="50dbb-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="50dbb-117">移轉購物車</span><span class="sxs-lookup"><span data-stu-id="50dbb-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="50dbb-118">當使用者按一下 [簽出] 按鈕時，是匿名的購物程序時，必須註冊和登入。</span><span class="sxs-lookup"><span data-stu-id="50dbb-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="50dbb-119">使用者會預期我們會維持之間瀏覽次數，因此我們需要將購物車資訊關聯的使用者，當他們完成註冊或登入其購物車 」 資訊。</span><span class="sxs-lookup"><span data-stu-id="50dbb-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="50dbb-120">這是執行動作，其實很簡單，因為我們 ShoppingCart 類別已經有方法，這個方法會將目前的購物車中的所有項目關聯的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="50dbb-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="50dbb-121">我們只需要呼叫這個方法，當使用者完成註冊或登入。</span><span class="sxs-lookup"><span data-stu-id="50dbb-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="50dbb-122">開啟**AccountController**我們所設定的成員資格和授權時，我們新增的類別。</span><span class="sxs-lookup"><span data-stu-id="50dbb-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="50dbb-123">新增 using 陳述式參考 MvcMusicStore.Models，然後新增下列 MigrateShoppingCart 方法：</span><span class="sxs-lookup"><span data-stu-id="50dbb-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="50dbb-124">接下來，修改登入後動作來呼叫 MigrateShoppingCart 之後使用者已驗證，, 如下所示：</span><span class="sxs-lookup"><span data-stu-id="50dbb-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="50dbb-125">進行相同變更，即可註冊張貼動作，只有在成功建立使用者帳戶之後，立即：</span><span class="sxs-lookup"><span data-stu-id="50dbb-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="50dbb-126">現在匿名的購物車會自動轉移至在成功註冊或登入的使用者帳戶就是這樣。</span><span class="sxs-lookup"><span data-stu-id="50dbb-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="50dbb-127">建立 CheckoutController</span><span class="sxs-lookup"><span data-stu-id="50dbb-127">Creating the CheckoutController</span></span>

<span data-ttu-id="50dbb-128">以滑鼠右鍵按一下 [控制器] 資料夾，並將新的控制站新增至名為 CheckoutController 使用空白控制器範本專案。</span><span class="sxs-lookup"><span data-stu-id="50dbb-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="50dbb-129">首先，新增 Authorize 屬性要求使用者註冊，才能簽出在控制器類別宣告之上：</span><span class="sxs-lookup"><span data-stu-id="50dbb-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="50dbb-130">*注意： 這是類似於我們先前所做 StoreManagerController，變更，但在此情況下 Authorize 屬性所需的使用者是系統管理員角色。在簽出控制器中，我們會要求使用者登入，但不需要他們系統管理員。*</span><span class="sxs-lookup"><span data-stu-id="50dbb-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="50dbb-131">為了簡單起見，我們將不會處理在本教學課程的付款資訊。</span><span class="sxs-lookup"><span data-stu-id="50dbb-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="50dbb-132">相反地，我們允許使用者查看使用促銷代碼。</span><span class="sxs-lookup"><span data-stu-id="50dbb-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="50dbb-133">我們將儲存此促銷代碼，使用名為促銷代碼的常數。</span><span class="sxs-lookup"><span data-stu-id="50dbb-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="50dbb-134">如同 StoreController，我們要宣告欄位以保留 MusicStoreEntities 類別，名為 storeDB 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="50dbb-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="50dbb-135">為了讓 MusicStoreEntities 類別的使用，我們將需要新增 using 陳述式 MvcMusicStore.Models 命名空間。</span><span class="sxs-lookup"><span data-stu-id="50dbb-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="50dbb-136">簽出控制器的頂端會出現下方。</span><span class="sxs-lookup"><span data-stu-id="50dbb-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="50dbb-137">CheckoutController 將會有下列控制器動作：</span><span class="sxs-lookup"><span data-stu-id="50dbb-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="50dbb-138">**AddressAndPayment （GET 方法）** 會顯示表單，以允許使用者輸入他們的資訊。</span><span class="sxs-lookup"><span data-stu-id="50dbb-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="50dbb-139">**AddressAndPayment （POST 方法）** 會驗證輸入，並且處理訂單。</span><span class="sxs-lookup"><span data-stu-id="50dbb-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="50dbb-140">**完成**之後使用者已順利完成結帳程序將會顯示。</span><span class="sxs-lookup"><span data-stu-id="50dbb-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="50dbb-141">此檢視會包含使用者的訂單號碼，以確認。</span><span class="sxs-lookup"><span data-stu-id="50dbb-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="50dbb-142">首先，讓我們將索引控制器動作 （這我們建立控制器時，所產生） 重新命名為 AddressAndPayment。</span><span class="sxs-lookup"><span data-stu-id="50dbb-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="50dbb-143">此控制器動作只會顯示簽出的表單中，因此它不需要任何模型的資訊。</span><span class="sxs-lookup"><span data-stu-id="50dbb-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="50dbb-144">我們的 AddressAndPayment POST 方法會遵循相同的模式，我們用於 StoreManagerController： 它會嘗試接受送出表單，並完成訂單，並會在失敗時重新顯示表單。</span><span class="sxs-lookup"><span data-stu-id="50dbb-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="50dbb-145">驗證表單輸入符合我們的驗證需求的訂單之後，我們將直接檢查促銷代碼表單值。</span><span class="sxs-lookup"><span data-stu-id="50dbb-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="50dbb-146">假設一切正確無誤，我們會與訂單儲存更新的資訊，請告知 ShoppingCart 物件完成訂單程序，並重新導向至完成的動作。</span><span class="sxs-lookup"><span data-stu-id="50dbb-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="50dbb-147">成功完成時簽出程序，將使用者重新導向至完整的控制器動作。</span><span class="sxs-lookup"><span data-stu-id="50dbb-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="50dbb-148">此動作會執行簡單的檢查，驗證順序會確實屬於登入的使用者之前先顯示確認的訂單號碼。</span><span class="sxs-lookup"><span data-stu-id="50dbb-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="50dbb-149">*注意： 錯誤檢視時自動建立為我們 /Views/Shared 資料夾中我們已經開始專案。*</span><span class="sxs-lookup"><span data-stu-id="50dbb-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="50dbb-150">完整的 CheckoutController 程式碼如下所示：</span><span class="sxs-lookup"><span data-stu-id="50dbb-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="50dbb-151">加入 AddressAndPayment 檢視</span><span class="sxs-lookup"><span data-stu-id="50dbb-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="50dbb-152">現在，讓我們建立 AddressAndPayment 檢視。</span><span class="sxs-lookup"><span data-stu-id="50dbb-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="50dbb-153">以滑鼠右鍵按一下其中一個 AddressAndPayment 控制器動作，並新增名為 AddressAndPayment 會強型別為訂單，並使用 編輯範本，如下所示的檢視。</span><span class="sxs-lookup"><span data-stu-id="50dbb-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="50dbb-154">此檢視會使用兩個我們討論過建置 StoreManagerEdit 檢視時的技術：</span><span class="sxs-lookup"><span data-stu-id="50dbb-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="50dbb-155">我們將使用 Html.EditorForModel() 來顯示表單欄位的順序模型</span><span class="sxs-lookup"><span data-stu-id="50dbb-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="50dbb-156">我們將會利用驗證規則使用驗證屬性中的 Order 類別</span><span class="sxs-lookup"><span data-stu-id="50dbb-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="50dbb-157">我們一開始先更新表單程式碼使用 Html.EditorForModel()，後面接著其他文字方塊的促銷代碼。</span><span class="sxs-lookup"><span data-stu-id="50dbb-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="50dbb-158">AddressAndPayment 檢視的完整程式碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="50dbb-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="50dbb-159">定義驗證規則的順序</span><span class="sxs-lookup"><span data-stu-id="50dbb-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="50dbb-160">現在，我們檢視已設定時，我們會設定驗證規則為我們的訂單模型如同我們先前專輯模型。</span><span class="sxs-lookup"><span data-stu-id="50dbb-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="50dbb-161">Models 資料夾上按一下滑鼠右鍵，並新增名為順序的類別。</span><span class="sxs-lookup"><span data-stu-id="50dbb-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="50dbb-162">除了我們先前使用相簿的驗證屬性，我們將也會使用規則運算式來驗證使用者的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="50dbb-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="50dbb-163">嘗試送出表單時遺漏或無效的資訊現在會顯示使用用戶端驗證的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="50dbb-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="50dbb-164">好了，已經完成大半的困難的工作，如結帳程序;我們只需要少數的機率 and 結束，才能完成。</span><span class="sxs-lookup"><span data-stu-id="50dbb-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="50dbb-165">我們需要加入兩個簡單的檢視，而且我們需要負責處理的登入程序的購物車資訊遞移式。</span><span class="sxs-lookup"><span data-stu-id="50dbb-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="50dbb-166">加入簽出完整的檢視</span><span class="sxs-lookup"><span data-stu-id="50dbb-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="50dbb-167">簽出完整的檢視也很簡單，因為它只需要顯示訂單識別碼。</span><span class="sxs-lookup"><span data-stu-id="50dbb-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="50dbb-168">以滑鼠右鍵按一下完成控制器動作，並新增名為完成，它強型別為整數的檢視</span><span class="sxs-lookup"><span data-stu-id="50dbb-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="50dbb-169">現在我們將會更新檢視程式碼，以顯示訂單 ID，如下所示。</span><span class="sxs-lookup"><span data-stu-id="50dbb-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="50dbb-170">正在更新檢視時發生錯誤</span><span class="sxs-lookup"><span data-stu-id="50dbb-170">Updating The Error view</span></span>

<span data-ttu-id="50dbb-171">預設範本包含錯誤檢視共用的 [views] 資料夾中，以便它可以重複使用其他位置中的站台。</span><span class="sxs-lookup"><span data-stu-id="50dbb-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="50dbb-172">此錯誤檢視包含非常簡單的錯誤，而且不會使用我們的網站配置，因此我們將會更新它。</span><span class="sxs-lookup"><span data-stu-id="50dbb-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="50dbb-173">由於這是一般錯誤頁面時，內容就非常簡單。</span><span class="sxs-lookup"><span data-stu-id="50dbb-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="50dbb-174">我們將會包含訊息和連結，以便瀏覽至歷程記錄中的上一頁，如果使用者想要重試其動作。</span><span class="sxs-lookup"><span data-stu-id="50dbb-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> <span data-ttu-id="50dbb-175">[上一頁](mvc-music-store-part-8.md)
> [下一頁](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="50dbb-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
