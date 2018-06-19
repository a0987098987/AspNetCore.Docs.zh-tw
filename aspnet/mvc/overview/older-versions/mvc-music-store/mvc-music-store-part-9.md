---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 部分 9： 註冊並簽出 |Microsoft 文件
author: jongalloway
description: 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 9 部份涵蓋註冊和簽出。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e7e83b70f2508b6dfc0c078b992747a76e4d0ff2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870111"
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="7fd9c-104">部分 9： 註冊並簽出</span><span class="sxs-lookup"><span data-stu-id="7fd9c-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="7fd9c-105">由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="7fd9c-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="7fd9c-106">MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="7fd9c-107">MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="7fd9c-108">此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="7fd9c-109">第 9 部份涵蓋註冊和簽出。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="7fd9c-110">在本節中，我們將建立 CheckoutController 來收集購物者的地址和付款資訊。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="7fd9c-111">我們會要求使用者註冊，簽入之前網站，讓此控制器將會需要授權。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="7fd9c-112">使用者會巡覽至在結帳程序從購物車 「 結帳 」 按鈕，即可。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="7fd9c-113">如果使用者尚未登入中，將會提示他們。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="7fd9c-114">成功登入，使用者會再顯示位址 」 和 「 付款檢視。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="7fd9c-115">一旦他們有填滿表單，並送出訂單，它們將會顯示 [訂單確認] 畫面。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="7fd9c-116">嘗試檢視不存在順序或不屬於您的訂單會顯示檢視時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="7fd9c-117">移轉購物車</span><span class="sxs-lookup"><span data-stu-id="7fd9c-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="7fd9c-118">雖然購物程序是匿名的當使用者簽出 5d; 按鈕時，他們必須註冊和登入。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="7fd9c-119">使用者將預期我們會維持之間瀏覽，因此我們必須要與使用者產生關聯的購物車資訊，當他們完成註冊或登入及其購物車資訊。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="7fd9c-120">這是執行動作，其實很簡單，因為我們 ShoppingCart 類別已經有方法，這個方法會將目前的購物車中的所有項目關聯的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="7fd9c-121">我們只需要呼叫這個方法，當使用者完成註冊或登入。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="7fd9c-122">開啟**AccountController**我們所設定的成員資格和授權時，我們加入的類別。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="7fd9c-123">加入 using 陳述式參考 MvcMusicStore.Models，然後加入下列 MigrateShoppingCart 方法：</span><span class="sxs-lookup"><span data-stu-id="7fd9c-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="7fd9c-124">接下來，修改登入後動作之後, 要呼叫 MigrateShoppingCart 已驗證使用者，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7fd9c-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="7fd9c-125">進行相同變更註冊後動作，，已成功建立使用者帳戶之後，立即：</span><span class="sxs-lookup"><span data-stu-id="7fd9c-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="7fd9c-126">這是它-現在匿名的購物車會自動轉移至在成功註冊或登入的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="7fd9c-127">建立 CheckoutController</span><span class="sxs-lookup"><span data-stu-id="7fd9c-127">Creating the CheckoutController</span></span>

<span data-ttu-id="7fd9c-128">以滑鼠右鍵按一下 [控制器] 資料夾，並將新的控制站新增至名為 CheckoutController 使用空白的控制器範本的專案。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="7fd9c-129">首先，新增控制器類別宣告，以要求使用者註冊，才能簽出上方 Authorize 屬性：</span><span class="sxs-lookup"><span data-stu-id="7fd9c-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="7fd9c-130">*注意： 這是類似於我們先前對 StoreManagerController，變更，但在此情況下 Authorize 屬性所需的使用者是系統管理員角色中。簽出控制器，我們正在要求使用者登入，但不需要它們由系統管理員。*</span><span class="sxs-lookup"><span data-stu-id="7fd9c-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="7fd9c-131">為了簡單起見，我們將不會處理與本教學課程中的付款資訊。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="7fd9c-132">相反地，我們會讓使用者查看促銷代碼。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="7fd9c-133">我們會儲存使用名為 PromoCode 常數此促銷代碼。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="7fd9c-134">如同 StoreController，我們會宣告來保存 MusicStoreEntities 類別，名為 storeDB 的執行個體欄位。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="7fd9c-135">為了讓 MusicStoreEntities 類別使用，我們需要加入 using MvcMusicStore.Models 命名空間陳述式。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="7fd9c-136">簽出 controller 頂端會出現下方。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="7fd9c-137">CheckoutController 會有下列的控制器動作：</span><span class="sxs-lookup"><span data-stu-id="7fd9c-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="7fd9c-138">**AddressAndPayment （GET 方法）** 會顯示表單，以允許使用者輸入他們的資訊。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="7fd9c-139">**AddressAndPayment （POST 方法）** 會驗證輸入和處理順序。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="7fd9c-140">**完成**使用者順利完成在結帳程序之後將會顯示。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="7fd9c-141">此檢視會包含使用者的訂單號碼，以確認。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="7fd9c-142">首先，請將 AddressAndPayment 來重新命名索引控制器動作 （其中我們建立控制器時，所產生）。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="7fd9c-143">此控制器的動作只會顯示簽出的表單中，因此它並不需要任何模型的資訊。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="7fd9c-144">我們 AddressAndPayment POST 方法會依循相同的模式，我們使用 StoreManagerController： 它會嘗試接受送出表單，並完成順序，並會在失敗時重新顯示表單。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="7fd9c-145">驗證表單輸入符合我們的驗證需求的訂單後，我們將直接簽 PromoCode 表單值。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="7fd9c-146">假設所有設定都正確，我們會與訂單儲存更新的資訊，請告知 ShoppingCart 物件完成訂單程序，並會重新導向至完成的動作。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="7fd9c-147">成功完成時簽出程序，將使用者重新導向至完整的控制器動作。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="7fd9c-148">此動作將會執行驗證，順序並確實屬於已登入使用者顯示訂單號碼當做確認訊息之前的簡單檢查。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="7fd9c-149">*注意： 錯誤檢視時自動建立為我們 /Views/Shared 資料夾中我們已經開始專案。*</span><span class="sxs-lookup"><span data-stu-id="7fd9c-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="7fd9c-150">完整 CheckoutController 程式碼如下所示：</span><span class="sxs-lookup"><span data-stu-id="7fd9c-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="7fd9c-151">加入 AddressAndPayment 檢視</span><span class="sxs-lookup"><span data-stu-id="7fd9c-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="7fd9c-152">現在，讓我們來建立 AddressAndPayment 檢視。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="7fd9c-153">以滑鼠右鍵按一下其中一個 AddressAndPayment 控制器動作，並加入名為 AddressAndPayment 強型別為訂單，並且使用 編輯範本，如下所示的檢視。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="7fd9c-154">這個檢視可讓我們看建置 StoreManagerEdit 檢視時的技術的兩個使用：</span><span class="sxs-lookup"><span data-stu-id="7fd9c-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="7fd9c-155">我們將使用 Html.EditorForModel() 顯示表單欄位的順序模型</span><span class="sxs-lookup"><span data-stu-id="7fd9c-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="7fd9c-156">我們將利用 Order 類別中使用驗證屬性的驗證規則</span><span class="sxs-lookup"><span data-stu-id="7fd9c-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="7fd9c-157">我們會先更新表單程式碼使用 Html.EditorForModel()，後面接著其他的文字方塊中的促銷代碼。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="7fd9c-158">AddressAndPayment 檢視的完整程式碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="7fd9c-159">定義順序的驗證規則</span><span class="sxs-lookup"><span data-stu-id="7fd9c-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="7fd9c-160">既然我們檢視已設定，我們將設定的驗證規則為我們順序的模型可以如同我們先前專輯模型。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="7fd9c-161">以滑鼠右鍵按一下 [模型] 資料夾，然後加入類別的順序。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="7fd9c-162">除了我們先前使用相簿的驗證屬性，我們將也使用規則運算式來驗證使用者的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="7fd9c-163">嘗試送出表單時遺失或無效的資訊現在將會顯示錯誤訊息使用用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="7fd9c-164">好了，我們所做過大部分的困難工作在結帳程序中;我們只需要完成幾個勝算的特徵 and 結束。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="7fd9c-165">我們需要加入兩個簡單的檢視，因此需要處理的登入程序期間的購物車資訊遞交。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="7fd9c-166">新增簽出完整的檢視</span><span class="sxs-lookup"><span data-stu-id="7fd9c-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="7fd9c-167">簽出完整的檢視是相當簡單，只需要顯示順序識別碼。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="7fd9c-168">以滑鼠右鍵按一下 完成控制器動作，並加入名為完成其強型別當做整數的檢視</span><span class="sxs-lookup"><span data-stu-id="7fd9c-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="7fd9c-169">現在我們將會更新以顯示訂單 ID，檢視程式碼，如下所示。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="7fd9c-170">更新檢視時發生錯誤</span><span class="sxs-lookup"><span data-stu-id="7fd9c-170">Updating The Error view</span></span>

<span data-ttu-id="7fd9c-171">預設範本包括錯誤檢視共用的 [檢視] 資料夾中，使它可以重複使用其他位置中的站台。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="7fd9c-172">此錯誤檢視包含非常簡單的錯誤，而且不會使用配置中，我們的網站，因此我們會將它更新。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="7fd9c-173">由於這是一般錯誤網頁，內容是非常簡單。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="7fd9c-174">我們將會包含訊息和連結以瀏覽到記錄中上一頁如果使用者想要再試一次其動作。</span><span class="sxs-lookup"><span data-stu-id="7fd9c-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> <span data-ttu-id="7fd9c-175">[上一頁](mvc-music-store-part-8.md)
> [下一頁](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="7fd9c-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
