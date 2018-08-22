---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 第 5 節： 商務邏輯 |Microsoft Docs
author: JoeStagner
description: 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 5 部分加入一些商務邏輯。
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e18acb66dbdb3bd3e0dfa21193f617dad82afc74
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826429"
---
<a name="part-5-business-logic"></a><span data-ttu-id="234fa-104">第 5 節： 商務邏輯</span><span class="sxs-lookup"><span data-stu-id="234fa-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="234fa-105">藉由[Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="234fa-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="234fa-106">Tailspin Spyworks 示範建立功能強大、 可擴充的應用程式，適用於.NET 平台是如何富含簡單。</span><span class="sxs-lookup"><span data-stu-id="234fa-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="234fa-107">它會展示如何在 ASP.NET 4 中使用最棒的新功能，建置線上商店，包括購物、 簽出，以及系統管理。</span><span class="sxs-lookup"><span data-stu-id="234fa-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="234fa-108">本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="234fa-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="234fa-109">第 5 部分加入一些商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="234fa-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="234fa-110">加入一些商務邏輯</span><span class="sxs-lookup"><span data-stu-id="234fa-110">Adding Some Business Logic</span></span>

<span data-ttu-id="234fa-111">我們想要我們的購物體驗，只要有人瀏覽我們的網站才能使用。</span><span class="sxs-lookup"><span data-stu-id="234fa-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="234fa-112">訪客可以瀏覽並放入購物車新增項目，即使未註冊或登入。</span><span class="sxs-lookup"><span data-stu-id="234fa-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="234fa-113">當他們準備好要簽出即會獲得驗證選項，如果它們不是尚未成員都能夠建立帳戶。</span><span class="sxs-lookup"><span data-stu-id="234fa-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="234fa-114">這表示，我們必須實作邏輯以將購物車從匿名狀態轉換為 「 已註冊使用者 」 狀態。</span><span class="sxs-lookup"><span data-stu-id="234fa-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="234fa-115">讓我們建立名為 「 類別 」 的目錄，然後在資料夾上按一下滑鼠右鍵並建立新的 「 類別 」 檔案，名為 MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="234fa-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="234fa-116">我們將擴充實作 MyShoppingCart.aspx 頁面的類別，我們將會執行這項操作使用如先前所述。NET 的功能強大的 「 部分類別 」 建構。</span><span class="sxs-lookup"><span data-stu-id="234fa-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="234fa-117">我們 MyShoppingCart.aspx.cf 檔案產生的呼叫看起來像這樣。</span><span class="sxs-lookup"><span data-stu-id="234fa-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="234fa-118">請注意 「 部分 」 關鍵字的使用。</span><span class="sxs-lookup"><span data-stu-id="234fa-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="234fa-119">我們剛剛產生的類別檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="234fa-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="234fa-120">我們會將合併我們實作 partial 關鍵字加入這個檔案。</span><span class="sxs-lookup"><span data-stu-id="234fa-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="234fa-121">我們的新類別檔案現在看起來像這樣。</span><span class="sxs-lookup"><span data-stu-id="234fa-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="234fa-122">我們會將我們的類別加入的第一個方法是 「 AddItem 」 方法。</span><span class="sxs-lookup"><span data-stu-id="234fa-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="234fa-123">這是會在使用者按一下 產品清單 和 產品詳細資料頁面的 「 新增術 」 連結時最後呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="234fa-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="234fa-124">將下列附加至使用頁面頂端的陳述式。</span><span class="sxs-lookup"><span data-stu-id="234fa-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="234fa-125">並將下列方法新增至 MyShoppingCart 類別。</span><span class="sxs-lookup"><span data-stu-id="234fa-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="234fa-126">若要查看項目是否已在購物車中，我們會使用 LINQ to Entities。</span><span class="sxs-lookup"><span data-stu-id="234fa-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="234fa-127">如果因此，我們將更新項目的數量，否則我們會建立新的項目選取的項目</span><span class="sxs-lookup"><span data-stu-id="234fa-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="234fa-128">若要呼叫這個方法中，我們將實作 AddToCart.aspx 頁面，不僅類別此方法接著會顯示目前的購物 = 購物車新增項目之後。</span><span class="sxs-lookup"><span data-stu-id="234fa-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="234fa-129">以滑鼠右鍵按一下 [方案總管] 中的方案名稱，並新增和名為 AddToCart.aspx，因為我們先前做過的新頁面。</span><span class="sxs-lookup"><span data-stu-id="234fa-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="234fa-130">雖然我們無法使用此頁面顯示暫時結果等低內建的問題，我們的實作中，頁面將不會實際呈現，但而不是呼叫 「 Add 」 邏輯和重新導向。</span><span class="sxs-lookup"><span data-stu-id="234fa-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="234fa-131">若要完成此我們會加入下列程式碼頁面\_Load 事件。</span><span class="sxs-lookup"><span data-stu-id="234fa-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="234fa-132">請注意，我們會擷取要從查詢字串參數，然後呼叫類別的 AddItem 方法新增至購物車的產品。</span><span class="sxs-lookup"><span data-stu-id="234fa-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="234fa-133">假設沒有錯誤發生，則控制權會傳遞至我們將會完全實作下一步 SHoppingCart.aspx 頁面。</span><span class="sxs-lookup"><span data-stu-id="234fa-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="234fa-134">如果應該擲出例外狀況錯誤。</span><span class="sxs-lookup"><span data-stu-id="234fa-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="234fa-135">目前我們還沒有實作全域錯誤處理常式使這個例外狀況會形成未處理的應用程式，但我們將會解決這個問題很快。</span><span class="sxs-lookup"><span data-stu-id="234fa-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="234fa-136">也請注意使用陳述式 （可透過 Debug.Fail() `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="234fa-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="234fa-137">是偵錯工具內執行應用程式，這個方法會顯示詳細的對話方塊，以及我們所指定的錯誤訊息的應用程式狀態的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="234fa-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="234fa-138">當生產環境中執行陳述式會忽略 Debug.Fail()。</span><span class="sxs-lookup"><span data-stu-id="234fa-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="234fa-139">您會發現我們購物車類別名稱"GetShoppingCartId 」 中的方法呼叫上方的程式碼。</span><span class="sxs-lookup"><span data-stu-id="234fa-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="234fa-140">加入程式碼以實作方法，如下所示。</span><span class="sxs-lookup"><span data-stu-id="234fa-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="234fa-141">請注意，我們也新增更新] 和 [簽出按鈕和標籤，我們可以在其中顯示購物車 」 總數 」。</span><span class="sxs-lookup"><span data-stu-id="234fa-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="234fa-142">我們現在可以將項目加入我們的購物車，但我們尚未實作邏輯，以新增一項產品之後，顯示購物車。</span><span class="sxs-lookup"><span data-stu-id="234fa-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="234fa-143">因此，MyShoppingCart.aspx 頁面中我們將新增 EntityDataSource 控制項和 GridVire 控制項，如下所示。</span><span class="sxs-lookup"><span data-stu-id="234fa-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="234fa-144">表單設計工具中呼叫，以便您可以按兩下更新購物車 按鈕，並產生在標記中宣告中指定的 click 事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="234fa-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="234fa-145">我們稍後將實作詳細資料，但如此一來我們建置並執行我們的應用程式，而且未發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="234fa-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="234fa-146">當您執行應用程式，並放入購物車新增項目時，您會看到這。</span><span class="sxs-lookup"><span data-stu-id="234fa-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="234fa-147">請注意，我們必須偏離 「 預設 」 方格顯示藉由實作三個自訂資料行。</span><span class="sxs-lookup"><span data-stu-id="234fa-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="234fa-148">第一個是可編輯 」、 「 數量 」 繫結 」 欄位：</span><span class="sxs-lookup"><span data-stu-id="234fa-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="234fa-149">下一步 會顯示一行的項目總數 （項目成本時間來排序的數量） 的 「 計算 」 資料行：</span><span class="sxs-lookup"><span data-stu-id="234fa-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="234fa-150">最後，我們會有包含使用者要用來表示應該從購物圖表中移除之項目的核取方塊控制項的自訂資料行。</span><span class="sxs-lookup"><span data-stu-id="234fa-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="234fa-151">如您所見，總計列讓我們是空的順序就會加入一些邏輯來計算訂單總計。</span><span class="sxs-lookup"><span data-stu-id="234fa-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="234fa-152">我們先將 MyShoppingCart 類別實作 」 GetTotal 」 方法。</span><span class="sxs-lookup"><span data-stu-id="234fa-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="234fa-153">MyShoppingCart.cs 檔案中新增下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="234fa-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="234fa-154">然後在頁面\_Load 事件處理常式，我們可以呼叫我們 GetTotal 方法。</span><span class="sxs-lookup"><span data-stu-id="234fa-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="234fa-155">同時，我們將新增至購物車是否為空，並據以調整顯示，如果是測試。</span><span class="sxs-lookup"><span data-stu-id="234fa-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="234fa-156">現在如果是空的購物車出現這樣：</span><span class="sxs-lookup"><span data-stu-id="234fa-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="234fa-157">如果不是，我們會看到我們的總計。</span><span class="sxs-lookup"><span data-stu-id="234fa-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="234fa-158">不過，此頁面尚未完成。</span><span class="sxs-lookup"><span data-stu-id="234fa-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="234fa-159">我們需要其他邏輯來重新計算購物車，藉由移除標記為要移除的項目，以及藉由判斷新的量值，因為部分可能已變更方格中的使用者。</span><span class="sxs-lookup"><span data-stu-id="234fa-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="234fa-160">可讓 「 RemoveItem 」 方法加入購物車類別 MyShoppingCart.cs 來處理此案例，當使用者將標示為移除項目中。</span><span class="sxs-lookup"><span data-stu-id="234fa-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="234fa-161">現在讓我們 ad 方法來處理的情況下，當使用者只需變更排序 GridView 裡的品質。</span><span class="sxs-lookup"><span data-stu-id="234fa-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="234fa-162">使用中位置的基本移除和更新功能，我們可以實作實際更新購物車在資料庫中的邏輯。</span><span class="sxs-lookup"><span data-stu-id="234fa-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="234fa-163">（在 MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="234fa-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="234fa-164">您會注意到這個方法需要兩個參數。</span><span class="sxs-lookup"><span data-stu-id="234fa-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="234fa-165">一個是購物車識別碼，另一個是使用者定義型別之物件的陣列。</span><span class="sxs-lookup"><span data-stu-id="234fa-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="234fa-166">以我們的邏輯，在使用者介面特性上的相依性降到最低，我們已經定義了我們可以將購物車項目的程式碼，而我們無須直接存取 GridView 控制項的方法不使用的資料結構。</span><span class="sxs-lookup"><span data-stu-id="234fa-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="234fa-167">在我們 MyShoppingCart.aspx.cs 檔案我們可以使用此結構在我們更新按鈕 Click 事件處理常式中，如下所示。</span><span class="sxs-lookup"><span data-stu-id="234fa-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="234fa-168">請注意，除了更新購物車我們將更新的購物車總計。</span><span class="sxs-lookup"><span data-stu-id="234fa-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="234fa-169">請注意特別需要注意這行程式碼：</span><span class="sxs-lookup"><span data-stu-id="234fa-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="234fa-170">GetValues() 是特殊的協助程式函式，我們會在實作 MyShoppingCart.aspx.cs，如下所示。</span><span class="sxs-lookup"><span data-stu-id="234fa-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="234fa-171">這提供明確的方法，以存取我們的 GridView 控制項繫結項目的值。</span><span class="sxs-lookup"><span data-stu-id="234fa-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="234fa-172">因為我們的 [移除項目] 核取方塊控制項未繫結，所以我們會透過 FindControl() 方法來進行存取。</span><span class="sxs-lookup"><span data-stu-id="234fa-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="234fa-173">在這個階段，在您的專案開發過程中，我們會準備實作簽出程序。</span><span class="sxs-lookup"><span data-stu-id="234fa-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="234fa-174">這麼做讓我們先使用 Visual Studio 產生的成員資格資料庫，並將使用者新增至成員資格儲存機制。</span><span class="sxs-lookup"><span data-stu-id="234fa-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="234fa-175">[上一頁](tailspin-spyworks-part-4.md)
> [下一頁](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="234fa-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
