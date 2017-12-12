---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: "第 5 部分： 商務邏輯 |Microsoft 文件"
author: JoeStagner
description: "此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 5 部分加入某個商務邏輯。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e205788e05a2ad94d86d4847c11c40898b1c3113
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="part-5-business-logic"></a><span data-ttu-id="63576-104">第 5 部分： 商務邏輯</span><span class="sxs-lookup"><span data-stu-id="63576-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="63576-105">由[Joe stagner 以](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="63576-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="63576-106">Tailspin Spyworks 示範建立功能強大、 可調整的應用程式的.NET 平台是如何 hierarchy 簡單。</span><span class="sxs-lookup"><span data-stu-id="63576-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="63576-107">它會顯示如何使用 ASP.NET 4 中的強大的新功能，建置線上商店，包括購物、 簽出，以及管理關閉。</span><span class="sxs-lookup"><span data-stu-id="63576-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="63576-108">此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="63576-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="63576-109">第 5 部分加入某個商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="63576-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a><span data-ttu-id="63576-110">加入一些商務邏輯</span><span class="sxs-lookup"><span data-stu-id="63576-110">Adding Some Business Logic</span></span>

<span data-ttu-id="63576-111">我們想要我們購物的經驗，只要有人瀏覽我們的網站才能使用。</span><span class="sxs-lookup"><span data-stu-id="63576-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="63576-112">訪客可以瀏覽並加入至購物車的項目，即使未註冊或登入。</span><span class="sxs-lookup"><span data-stu-id="63576-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="63576-113">在準備簽出時即會獲得驗證選項，如果它們不是尚未成員將會讓它們能夠建立帳戶。</span><span class="sxs-lookup"><span data-stu-id="63576-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="63576-114">這表示我們必須實作邏輯，以購物車將匿名的狀態轉換為 「 已註冊的使用者 」 狀態。</span><span class="sxs-lookup"><span data-stu-id="63576-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="63576-115">讓我們來建立名為 「 類別 」 的目錄，然後在資料夾上按一下滑鼠右鍵並建立新的 「 類別 」 檔案，名為 MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="63576-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="63576-116">如先前所述實作 MyShoppingCart.aspx 頁面的類別會加以擴充，我們會執行此作業使用。網路的功能強大的 「 部分類別 」 建構。</span><span class="sxs-lookup"><span data-stu-id="63576-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="63576-117">產生的呼叫我們 MyShoppingCart.aspx.cf 檔案看起來像這樣。</span><span class="sxs-lookup"><span data-stu-id="63576-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="63576-118">請注意 「 部分 」 關鍵字的使用。</span><span class="sxs-lookup"><span data-stu-id="63576-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="63576-119">我們剛才產生的類別檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="63576-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="63576-120">我們會將部分的關鍵字加入這個檔案以及合併我們實作。</span><span class="sxs-lookup"><span data-stu-id="63576-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="63576-121">我們新的類別檔案現在看起來像這樣。</span><span class="sxs-lookup"><span data-stu-id="63576-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="63576-122">我們會將它加入至類別的第一個方法是 「 AddItem"方法。</span><span class="sxs-lookup"><span data-stu-id="63576-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="63576-123">這是將當使用者按一下 產品清單 和 產品詳細資料頁面的 「 新增圖案 」 連結最後呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="63576-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="63576-124">使用新增下列陳述式，在頁面頂端。</span><span class="sxs-lookup"><span data-stu-id="63576-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="63576-125">並將此方法加入至 MyShoppingCart 類別。</span><span class="sxs-lookup"><span data-stu-id="63576-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="63576-126">我們使用 LINQ to Entities 項目已在購物車中。</span><span class="sxs-lookup"><span data-stu-id="63576-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="63576-127">如果因此，我們可以更新訂單數量的項目，否則我們會建立新的項目選取的項目</span><span class="sxs-lookup"><span data-stu-id="63576-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="63576-128">若要呼叫這個方法中，我們會實作類別的這個方法不僅接著會顯示目前的項目加入後購物 = 購物車之 AddToCart.aspx 頁面。</span><span class="sxs-lookup"><span data-stu-id="63576-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="63576-129">以滑鼠右鍵按一下 [方案總管] 中的方案名稱，並加入與新命名 AddToCart.aspx，因為我們先前做過的頁面。</span><span class="sxs-lookup"><span data-stu-id="63576-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="63576-130">時要顯示暫時結果類似等低內建的問題，我們的實作中，我們可以使用此頁面，頁面將實際上不會呈現，而是呼叫 「 新增 」 邏輯和重新導向。</span><span class="sxs-lookup"><span data-stu-id="63576-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="63576-131">若要完成此我們會將下列程式碼新增至頁面\_載入事件。</span><span class="sxs-lookup"><span data-stu-id="63576-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="63576-132">請注意，我們會擷取從查詢字串參數，然後呼叫類別的 AddItem 方法新增至購物車的產品。</span><span class="sxs-lookup"><span data-stu-id="63576-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="63576-133">不假設任何錯誤發生，則控制權會傳遞給我們將會完整實作下一步 SHoppingCart.aspx 頁面。</span><span class="sxs-lookup"><span data-stu-id="63576-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="63576-134">如果應該要有錯誤，我們會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="63576-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="63576-135">目前我們還沒有實作全域錯誤處理常式，此例外狀況將會進入未處理的應用程式，但我們將會很快解決這個。</span><span class="sxs-lookup"><span data-stu-id="63576-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="63576-136">也請注意使用陳述式 （可透過 Debug.Fail()`using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="63576-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="63576-137">是在偵錯工具執行應用程式，這個方法會顯示詳細的對話方塊，以及我們在此指定的錯誤訊息的應用程式狀態的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="63576-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="63576-138">當生產環境中執行陳述式會忽略 Debug.Fail()。</span><span class="sxs-lookup"><span data-stu-id="63576-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="63576-139">您會注意到我們購物車類別名稱"GetShoppingCartId 」 中的方法呼叫上方的程式碼。</span><span class="sxs-lookup"><span data-stu-id="63576-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="63576-140">加入程式碼來實作的方法，如下所示。</span><span class="sxs-lookup"><span data-stu-id="63576-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="63576-141">請注意，我們也新增更新] 和 [簽出按鈕和標籤，我們可以在其中顯示購物車 」 總數 」。</span><span class="sxs-lookup"><span data-stu-id="63576-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="63576-142">我們現在可以將項目加入我們的購物車，但我們未實作已加入一項產品之後顯示購物車的邏輯。</span><span class="sxs-lookup"><span data-stu-id="63576-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="63576-143">因此，在 MyShoppingCart.aspx 頁面我們要加入 EntityDataSource 控制項和 GridVire 控制項，如下所示。</span><span class="sxs-lookup"><span data-stu-id="63576-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="63576-144">在設計工具中的表單，如此您可以按兩下更新購物車 5d; 按鈕，並產生在標記宣告中所指定的 click 事件處理常式呼叫。</span><span class="sxs-lookup"><span data-stu-id="63576-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="63576-145">我們稍後會實作詳細資料，但這樣讓我們建置並執行應用程式不會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="63576-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="63576-146">當您執行應用程式，並新增至購物車的項目將會看見這。</span><span class="sxs-lookup"><span data-stu-id="63576-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="63576-147">請注意我們有萬一從 「 預設 」 方格顯示藉由實作三個自訂的資料行。</span><span class="sxs-lookup"><span data-stu-id="63576-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="63576-148">第一個是編輯 」、 「 數量 」 繫結 」 欄位：</span><span class="sxs-lookup"><span data-stu-id="63576-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="63576-149">下一步是顯示的行項目總數 （項目成本時間來排序的數量） 的 「 計算 」 資料行：</span><span class="sxs-lookup"><span data-stu-id="63576-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="63576-150">最後，我們有自訂的資料行包含使用者要用來表示應該從購物圖表中移除之項目的核取方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="63576-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="63576-151">如您所見，總計列讓我們是空的順序就會加入某些邏輯，來計算訂單總計。</span><span class="sxs-lookup"><span data-stu-id="63576-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="63576-152">我們第一次將我們 MyShoppingCart 類別實作 」 GetTotal"方法。</span><span class="sxs-lookup"><span data-stu-id="63576-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="63576-153">MyShoppingCart.cs 檔案中加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="63576-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="63576-154">然後在頁面\_Load 事件處理常式，我們可以呼叫 GetTotal 方法。</span><span class="sxs-lookup"><span data-stu-id="63576-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="63576-155">在相同的時間，我們會新增是否為空的購物車，並據以調整顯示畫面，如果是測試。</span><span class="sxs-lookup"><span data-stu-id="63576-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="63576-156">現在如果是空的購物車我們取得這：</span><span class="sxs-lookup"><span data-stu-id="63576-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="63576-157">而且，如果不是，我們會了解我們的總計。</span><span class="sxs-lookup"><span data-stu-id="63576-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="63576-158">不過，此頁面尚未完成。</span><span class="sxs-lookup"><span data-stu-id="63576-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="63576-159">我們需要額外邏輯，以重新計算購物車，藉由移除標記為要移除的項目，以及藉由判斷新的量值，因為某些可能已經變更方格中的使用者。</span><span class="sxs-lookup"><span data-stu-id="63576-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="63576-160">可讓 「 RemoveItem"方法加入購物車類別 MyShoppingCart.cs 以處理的情況，當使用者將標示為移除的項目中。</span><span class="sxs-lookup"><span data-stu-id="63576-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="63576-161">現在讓我們 ad 方法以處理當時的情況，當使用者只要變更要在 GridView 中排序的品質。</span><span class="sxs-lookup"><span data-stu-id="63576-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="63576-162">我們可以就地基本移除和更新功能實作實際更新購物車資料庫中的邏輯。</span><span class="sxs-lookup"><span data-stu-id="63576-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="63576-163">（在 MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="63576-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="63576-164">您會注意到這個方法必須要有兩個參數。</span><span class="sxs-lookup"><span data-stu-id="63576-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="63576-165">其中一個是購物車識別碼，另一個是使用者定義類型之物件的陣列。</span><span class="sxs-lookup"><span data-stu-id="63576-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="63576-166">以我們需使用者介面的特定項目的邏輯的相依性降到最低，我們已定義資料結構，我們將傳遞至我們的程式碼的購物車項目，而我們需要直接存取 GridView 控制項的方法不可以使用。</span><span class="sxs-lookup"><span data-stu-id="63576-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="63576-167">在我們 MyShoppingCart.aspx.cs 檔案我們可以使用此結構在我們的更新按鈕 Click 事件處理常式，如下所示。</span><span class="sxs-lookup"><span data-stu-id="63576-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="63576-168">請注意，除了更新購物車我們將更新的購物車總計。</span><span class="sxs-lookup"><span data-stu-id="63576-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="63576-169">請注意與特別需要注意這行程式碼：</span><span class="sxs-lookup"><span data-stu-id="63576-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="63576-170">GetValues() 是特殊的協助程式函式，我們將在實作 MyShoppingCart.aspx.cs，如下所示。</span><span class="sxs-lookup"><span data-stu-id="63576-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="63576-171">這提供明確的方法，以存取我們的 GridView 控制項中的繫結項目值。</span><span class="sxs-lookup"><span data-stu-id="63576-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="63576-172">因為我們的 「 移除項目 」 核取方塊控制項未繫結，所以我們會透過 FindControl() 方法來進行存取。</span><span class="sxs-lookup"><span data-stu-id="63576-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="63576-173">在這個階段您的專案開發中，我們會準備實作在結帳程序。</span><span class="sxs-lookup"><span data-stu-id="63576-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="63576-174">這樣讓我們之前使用 Visual Studio 產生的成員資格資料庫，並將使用者加入成員資格儲存機制。</span><span class="sxs-lookup"><span data-stu-id="63576-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="63576-175">[上一頁](tailspin-spyworks-part-4.md)
[下一頁](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="63576-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
