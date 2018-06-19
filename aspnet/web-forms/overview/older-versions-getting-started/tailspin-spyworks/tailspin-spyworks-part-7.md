---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 第 7 部分： 加入功能 |Microsoft 文件
author: JoeStagner
description: 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 部分 7 加入其他功能，例如帳戶 revie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 17f068155f6726047901e2f7d580d375a4e07c87
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888389"
---
<a name="part-7-adding-features"></a><span data-ttu-id="b9057-104">第 7 部分： 新增功能</span><span class="sxs-lookup"><span data-stu-id="b9057-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="b9057-105">由[Joe stagner 以](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="b9057-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="b9057-106">Tailspin Spyworks 示範建立功能強大、 可調整的應用程式的.NET 平台是如何 hierarchy 簡單。</span><span class="sxs-lookup"><span data-stu-id="b9057-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="b9057-107">它會顯示如何使用 ASP.NET 4 中的強大的新功能，建置線上商店，包括購物、 簽出，以及管理關閉。</span><span class="sxs-lookup"><span data-stu-id="b9057-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="b9057-108">此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="b9057-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="b9057-109">部分 7 加入其他功能，例如帳戶檢閱、 產品評論和 「 熱門項目 」 和 「 也已購買 「 使用者控制項。</span><span class="sxs-lookup"><span data-stu-id="b9057-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="b9057-110">加入功能</span><span class="sxs-lookup"><span data-stu-id="b9057-110">Adding Features</span></span>

<span data-ttu-id="b9057-111">使用者可以瀏覽我們的目錄，將項目放在購物車，並完成簽出程序，還是有一些支援的功能，我們將包含改善我們的網站。</span><span class="sxs-lookup"><span data-stu-id="b9057-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="b9057-112">帳戶檢閱 （清單放訂單，並檢視詳細資料。）</span><span class="sxs-lookup"><span data-stu-id="b9057-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="b9057-113">前端網頁加入內容的特定內容。</span><span class="sxs-lookup"><span data-stu-id="b9057-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="b9057-114">新增功能，讓使用者檢閱產品類別目錄中。</span><span class="sxs-lookup"><span data-stu-id="b9057-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="b9057-115">建立使用者控制項加入 [前端] 頁面上顯示常用的項目和位置，以控制。</span><span class="sxs-lookup"><span data-stu-id="b9057-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="b9057-116">建立 「 也購買 」 的使用者控制項，並將它加入至產品詳細資料頁面上。</span><span class="sxs-lookup"><span data-stu-id="b9057-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="b9057-117">新增連絡人 頁面。</span><span class="sxs-lookup"><span data-stu-id="b9057-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="b9057-118">新增有關頁面。</span><span class="sxs-lookup"><span data-stu-id="b9057-118">Add an About Page.</span></span>
8. <span data-ttu-id="b9057-119">全域錯誤</span><span class="sxs-lookup"><span data-stu-id="b9057-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="b9057-120">帳戶檢閱</span><span class="sxs-lookup"><span data-stu-id="b9057-120">Account Review</span></span>

<span data-ttu-id="b9057-121">在 [帳戶] 資料夾中建立一個具名的 OrderList.aspx 和其他具名的 OrderDetails.aspx 的兩個.aspx 頁面</span><span class="sxs-lookup"><span data-stu-id="b9057-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="b9057-122">就像我們之前有 OrderList.aspx 會利用 GridView 和 EntityDataSoure 控制項。</span><span class="sxs-lookup"><span data-stu-id="b9057-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="b9057-123">EntityDataSoure Orders 資料表中的使用者名稱上篩選選取記錄 （請參閱 WhereParameter） 我們中工作階段變數的使用者記錄時設定。</span><span class="sxs-lookup"><span data-stu-id="b9057-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="b9057-124">此外請注意，這些參數的 GridView HyperlinkField 中：</span><span class="sxs-lookup"><span data-stu-id="b9057-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="b9057-125">這樣會指定 OrderID 欄位指定為查詢字串參數，以 OrderDetails.aspx 頁面每項產品的訂單詳細資料檢視的連結。</span><span class="sxs-lookup"><span data-stu-id="b9057-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="b9057-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="b9057-126">OrderDetails.aspx</span></span>

<span data-ttu-id="b9057-127">我們將使用 EntityDataSource 控制項存取訂單和 FormView 顯示訂單資料，另一個與 GridView EntityDataSource，以顯示所有訂單行項目。</span><span class="sxs-lookup"><span data-stu-id="b9057-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="b9057-128">在程式碼後置檔案 (OrderDetails.aspx.cs) 中有兩個小的位元環境維護。</span><span class="sxs-lookup"><span data-stu-id="b9057-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="b9057-129">首先，我們需要確定 OrderDetails 永遠都會取得 OrderId。</span><span class="sxs-lookup"><span data-stu-id="b9057-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="b9057-130">我們也要計算並顯示訂單總計的明細項目。</span><span class="sxs-lookup"><span data-stu-id="b9057-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="b9057-131">[首頁] 頁面</span><span class="sxs-lookup"><span data-stu-id="b9057-131">The Home Page</span></span>

<span data-ttu-id="b9057-132">讓我們來加入靜態內容至 Default.aspx 頁面。</span><span class="sxs-lookup"><span data-stu-id="b9057-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="b9057-133">首先，我要建立的 「 內容 」 的資料夾並在其中一個 Images 資料夾 （和我會包括在 [首頁] 頁面上使用的影像）。</span><span class="sxs-lookup"><span data-stu-id="b9057-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="b9057-134">Default.aspx 頁面底部版面配置，加入下列標記。</span><span class="sxs-lookup"><span data-stu-id="b9057-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="b9057-135">產品評論</span><span class="sxs-lookup"><span data-stu-id="b9057-135">Product Reviews</span></span>

<span data-ttu-id="b9057-136">第一次我們會將含有連結按鈕新增至表單，讓我們可以輸入的產品評論。</span><span class="sxs-lookup"><span data-stu-id="b9057-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="b9057-137">請注意我們的查詢字串中傳遞 ProductID</span><span class="sxs-lookup"><span data-stu-id="b9057-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="b9057-138">下一步 讓我們加入名為 ReviewAdd.aspx 頁面</span><span class="sxs-lookup"><span data-stu-id="b9057-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="b9057-139">此頁面會使用 ASP.NET AJAX Control Toolkit。</span><span class="sxs-lookup"><span data-stu-id="b9057-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="b9057-140">如果您沒有已完成，您可以下載從[DevExpress](http://devexpress.com/act)而上設定 Visual Studio 搭配使用此工具組的指引[ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md)。</span><span class="sxs-lookup"><span data-stu-id="b9057-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="b9057-141">在設計模式中，從 [工具箱] 拖曳控制項和驗證程式，建立如下的表單。</span><span class="sxs-lookup"><span data-stu-id="b9057-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="b9057-142">標記看起來會像這樣。</span><span class="sxs-lookup"><span data-stu-id="b9057-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="b9057-143">現在，我們可以輸入評論，可讓這些檢閱顯示在 [產品] 頁面上。</span><span class="sxs-lookup"><span data-stu-id="b9057-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="b9057-144">將這個標記加入至 ProductDetails.aspx 頁面。</span><span class="sxs-lookup"><span data-stu-id="b9057-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="b9057-145">執行現在應用程式，以及巡覽至產品顯示產品資訊，包括客戶評論。</span><span class="sxs-lookup"><span data-stu-id="b9057-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="b9057-146">熱門項目控制項 （建立使用者控制項）</span><span class="sxs-lookup"><span data-stu-id="b9057-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="b9057-147">以增加您的網站上的銷售我們將 「 建議賣出 「 熱門或相關產品加入一組功能。</span><span class="sxs-lookup"><span data-stu-id="b9057-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="b9057-148">這些功能的第一個會在我們的產品目錄中常見的產品的清單。</span><span class="sxs-lookup"><span data-stu-id="b9057-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="b9057-149">我們將建立 「 使用者控制項 」 以顯示最暢銷的首頁上的應用程式的項目。</span><span class="sxs-lookup"><span data-stu-id="b9057-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="b9057-150">由於這會為控制項，我們可以使用它的任何頁面直接拖曳到控制項放入任何頁面中，我們希望 Visual Studio 設計工具中。</span><span class="sxs-lookup"><span data-stu-id="b9057-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="b9057-151">在 Visual Studio 方案總管 中，以滑鼠右鍵按一下方案名稱，建立名為 「 控制項 」 的新目錄。</span><span class="sxs-lookup"><span data-stu-id="b9057-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="b9057-152">雖然您不需要這樣做，我們會協助讓我們依 「 控制項 」 目錄中建立我們所有的使用者控制項的專案。</span><span class="sxs-lookup"><span data-stu-id="b9057-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="b9057-153">以滑鼠右鍵按一下 [控制項] 資料夾，然後選擇 「 新的項目 」:</span><span class="sxs-lookup"><span data-stu-id="b9057-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="b9057-154">指定我們的掌控"PopularItems 」 的名稱。</span><span class="sxs-lookup"><span data-stu-id="b9057-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="b9057-155">請注意使用者控制項檔案的副檔名為.ascx 不.aspx。</span><span class="sxs-lookup"><span data-stu-id="b9057-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="b9057-156">我們熱門項目使用者控制項將如下所示定義。</span><span class="sxs-lookup"><span data-stu-id="b9057-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="b9057-157">這裡我們使用我們有尚未使用此應用程式中的方法。</span><span class="sxs-lookup"><span data-stu-id="b9057-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="b9057-158">我們會使用在中繼器控制項，而不使用資料來源控制項我們要繫結中繼器控制項至 LINQ to Entities 查詢的結果。</span><span class="sxs-lookup"><span data-stu-id="b9057-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="b9057-159">在我們的掌控背後的程式碼我們這樣做，如下所示。</span><span class="sxs-lookup"><span data-stu-id="b9057-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="b9057-160">也請注意我們的掌控標記頂端重要此行。</span><span class="sxs-lookup"><span data-stu-id="b9057-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="b9057-161">由於最受歡迎的項目將不會變更以分鐘以分鐘為基礎，我們可以加入發疼的指示詞，以改善我們的應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="b9057-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="b9057-162">這個指示詞會使控制項的程式碼只執行快取的輸出控制項的過期時。</span><span class="sxs-lookup"><span data-stu-id="b9057-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="b9057-163">否則，會使用這個控制項的輸出快取的版本。</span><span class="sxs-lookup"><span data-stu-id="b9057-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="b9057-164">現在我們只需要是在我們 Default.aspc 頁面中包含我們新的控制項。</span><span class="sxs-lookup"><span data-stu-id="b9057-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="b9057-165">使用拖放在開啟的資料行的預設表單中放置控制項的執行個體。</span><span class="sxs-lookup"><span data-stu-id="b9057-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="b9057-166">現在當我們執行我們的應用程式首頁會顯示最受歡迎的項目。</span><span class="sxs-lookup"><span data-stu-id="b9057-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="b9057-167">「 也購買 」 控制 （具有參數的使用者控制項）</span><span class="sxs-lookup"><span data-stu-id="b9057-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="b9057-168">我們將建立第二個使用者控制項將會建議加入內容精確性銷售到下一個層級。</span><span class="sxs-lookup"><span data-stu-id="b9057-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="b9057-169">為非一般的邏輯，以計算前 」 也購買 」 項目。</span><span class="sxs-lookup"><span data-stu-id="b9057-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="b9057-170">我們"也購買 」 控制項將會選取目前選取的 ProductID OrderDetails 記錄 （先前購買） 和抓取找到每個唯一訂單的訂單。</span><span class="sxs-lookup"><span data-stu-id="b9057-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="b9057-171">然後我們將會選取所有產品從所有這些訂單及購買數量的總和。</span><span class="sxs-lookup"><span data-stu-id="b9057-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="b9057-172">我們將依據數量加總產品進行排序，並顯示前五個項目。</span><span class="sxs-lookup"><span data-stu-id="b9057-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="b9057-173">指定此邏輯的複雜度，我們會實作這個演算法做為預存程序。</span><span class="sxs-lookup"><span data-stu-id="b9057-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="b9057-174">T-SQL 預存程序如下所示。</span><span class="sxs-lookup"><span data-stu-id="b9057-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="b9057-175">請注意，當我們包含它在我們的應用程式中，我們產生實體資料模型，我們除了的資料表和檢視，我們需要實體資料模型已指定，此預存程序 (SelectPurchasedWithProducts) 存在於資料庫應該包含此預存程序。</span><span class="sxs-lookup"><span data-stu-id="b9057-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="b9057-176">若要存取實體資料模型中的預存程序，我們需要匯入函式。</span><span class="sxs-lookup"><span data-stu-id="b9057-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="b9057-177">實體資料模型設計工具中開啟它，然後將模型瀏覽器中，開啟 [方案總管] 中按兩下然後以滑鼠右鍵按一下設計工具中，並選取 「 新增函式匯入 」。</span><span class="sxs-lookup"><span data-stu-id="b9057-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="b9057-178">這樣會開啟此對話方塊。</span><span class="sxs-lookup"><span data-stu-id="b9057-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="b9057-179">填寫您，請參閱上述選取"SelectPurchasedWithProducts 」 欄位，並使用我們匯入的函式名稱的程序名稱。</span><span class="sxs-lookup"><span data-stu-id="b9057-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="b9057-180">按一下 「 確定 」。</span><span class="sxs-lookup"><span data-stu-id="b9057-180">Click "Ok".</span></span>

<span data-ttu-id="b9057-181">需要這樣做我們只是可以撰寫預存程序針對如同我們可能會在模型中的任何其他項目。</span><span class="sxs-lookup"><span data-stu-id="b9057-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="b9057-182">因此，我們 「 控制項 」 資料夾中建立新的使用者控制項，名為 AlsoPurchased.ascx。</span><span class="sxs-lookup"><span data-stu-id="b9057-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="b9057-183">此控制項的標記看起來非常熟悉 PopularItems 控制項。</span><span class="sxs-lookup"><span data-stu-id="b9057-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="b9057-184">顯著的差異在於，會無法快取輸出因為項目的轉譯會因產品而異。</span><span class="sxs-lookup"><span data-stu-id="b9057-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="b9057-185">ProductId 將控制項的 「 屬性 」。</span><span class="sxs-lookup"><span data-stu-id="b9057-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="b9057-186">控制項的 PreRender 事件處理常式中我們 eed 做三件事。</span><span class="sxs-lookup"><span data-stu-id="b9057-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="b9057-187">請確定設定的 ProductID。</span><span class="sxs-lookup"><span data-stu-id="b9057-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="b9057-188">查看是否有與目前已購買任何產品。</span><span class="sxs-lookup"><span data-stu-id="b9057-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="b9057-189">輸出 #2 中決定的某些項目。</span><span class="sxs-lookup"><span data-stu-id="b9057-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="b9057-190">請注意，呼叫預存程序，透過模型是多麼的輕鬆。</span><span class="sxs-lookup"><span data-stu-id="b9057-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="b9057-191">決定，那里"也購買 」 之後無法直接繫結中繼器至查詢所傳回的結果。</span><span class="sxs-lookup"><span data-stu-id="b9057-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="b9057-192">如果沒有任何 「 也購買 」 項目我們將只是從我們的目錄中顯示其他常用的項目。</span><span class="sxs-lookup"><span data-stu-id="b9057-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="b9057-193">若要檢視 「 也購買 」 項目，開啟 ProductDetails.aspx 頁面，並從 [方案總管] 中拖曳 AlsoPurchased 控制項，使其出現在這個標記中的位置。</span><span class="sxs-lookup"><span data-stu-id="b9057-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="b9057-194">這樣會在頁面頂端的 ProductDetails 建立控制項的參考。</span><span class="sxs-lookup"><span data-stu-id="b9057-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="b9057-195">因為 AlsoPurchased 使用者控制項需要 ProductId 數字，我們會使用目前的資料模型項目頁面的 Eval 陳述式設定我們的掌控中的 [ProductID] 屬性。</span><span class="sxs-lookup"><span data-stu-id="b9057-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="b9057-196">當我們建置並立即執行，以及瀏覽至產品我們會看到 「 也購買 」 項目。</span><span class="sxs-lookup"><span data-stu-id="b9057-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="b9057-197">[上一頁](tailspin-spyworks-part-6.md)
> [下一頁](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="b9057-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
