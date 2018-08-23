---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 第 4 部分： 列出產品 |Microsoft Docs
author: JoeStagner
description: 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 4 部分涵蓋與 GridView contr.列出產品...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: ca7eccd684473d9a1ec4a8adfd8690b291fe702f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834670"
---
<a name="part-4-listing-products"></a><span data-ttu-id="3b165-104">第 4 部分： 列出產品</span><span class="sxs-lookup"><span data-stu-id="3b165-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="3b165-105">藉由[Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="3b165-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="3b165-106">Tailspin Spyworks 示範建立功能強大、 可擴充的應用程式，適用於.NET 平台是如何富含簡單。</span><span class="sxs-lookup"><span data-stu-id="3b165-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="3b165-107">它會展示如何在 ASP.NET 4 中使用最棒的新功能，建置線上商店，包括購物、 簽出，以及系統管理。</span><span class="sxs-lookup"><span data-stu-id="3b165-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="3b165-108">本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="3b165-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="3b165-109">第 4 部分涵蓋列出產品使用 GridView 控制項。</span><span class="sxs-lookup"><span data-stu-id="3b165-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="3b165-110">列出產品使用 GridView 控制項</span><span class="sxs-lookup"><span data-stu-id="3b165-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="3b165-111">讓我們開始實作我們 ProductsList.aspx 頁面 」 以滑鼠右鍵按一下 「 我們的解決方案，並選取 新增 和 新增項目 」。</span><span class="sxs-lookup"><span data-stu-id="3b165-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="3b165-112">選擇 Web 表單使用主版頁面並輸入 ProductsList.aspx 頁面名稱。 」</span><span class="sxs-lookup"><span data-stu-id="3b165-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="3b165-113">按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="3b165-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="3b165-114">接下來選擇我們放置 Site.Master 頁面的 「 樣式 」 資料夾，並從 [資料夾的內容] 視窗中選取。</span><span class="sxs-lookup"><span data-stu-id="3b165-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="3b165-115">按一下 [確定] 以建立頁面。</span><span class="sxs-lookup"><span data-stu-id="3b165-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="3b165-116">我們的資料庫會填入產品資料，如下所示。</span><span class="sxs-lookup"><span data-stu-id="3b165-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="3b165-117">建立我們的頁面之後我們再將使用實體資料來源來存取該產品的資料，但我們要選取的產品實體在本例中，我們需要限制為只有那些針對選取的類別會傳回的項目。</span><span class="sxs-lookup"><span data-stu-id="3b165-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="3b165-118">若要完成這項作業我們會告訴來自動產生 EntityDataSource WHERE 子句，我們會指定 WhereParameter。</span><span class="sxs-lookup"><span data-stu-id="3b165-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="3b165-119">您應該還記得，我們建立我們 [產品類別目錄功能表] 中的功能表項目時我們以動態方式建立連結 CatagoryID 加入每個連結的 QueryString。</span><span class="sxs-lookup"><span data-stu-id="3b165-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="3b165-120">我們會告訴實體資料來源，從該查詢字串參數中衍生的位置參數。</span><span class="sxs-lookup"><span data-stu-id="3b165-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="3b165-121">接下來，我們將設定 ListView 控制項來顯示產品清單。</span><span class="sxs-lookup"><span data-stu-id="3b165-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="3b165-122">若要建立最佳的購物體驗，我們會到每個個別的產品，顯示在我們 ListVew 壓縮數種簡潔的功能。</span><span class="sxs-lookup"><span data-stu-id="3b165-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="3b165-123">產品名稱會是產品的詳細資料檢視的連結。</span><span class="sxs-lookup"><span data-stu-id="3b165-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="3b165-124">產品的價格將會顯示。</span><span class="sxs-lookup"><span data-stu-id="3b165-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="3b165-125">將顯示產品的映像，我們會以動態方式從選取映像目錄映像的目錄在我們的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b165-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="3b165-126">我們將立即將特定的產品加入購物車的連結。</span><span class="sxs-lookup"><span data-stu-id="3b165-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="3b165-127">以下是我們的 ListView 控制項執行個體的標記。</span><span class="sxs-lookup"><span data-stu-id="3b165-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="3b165-128">我們會以動態方式建置每個顯示產品的幾個連結。</span><span class="sxs-lookup"><span data-stu-id="3b165-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="3b165-129">此外，我們測試自己的新頁面之前我們需要建立目錄結構的產品類別目錄的映像，如下所示。</span><span class="sxs-lookup"><span data-stu-id="3b165-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="3b165-130">存取我們的產品映像之後，我們可以測試我們的產品清單頁面。</span><span class="sxs-lookup"><span data-stu-id="3b165-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="3b165-131">從網站的首頁上，按一下其中一個類別目錄清單連結。</span><span class="sxs-lookup"><span data-stu-id="3b165-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="3b165-132">現在我們要實作 ProductDetials.apsx 頁面和 AddToCart 功能。</span><span class="sxs-lookup"><span data-stu-id="3b165-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="3b165-133">使用檔案-&gt;新建立的頁面名稱 ProductDetails.aspx 先前一樣，使用網站主版頁面。</span><span class="sxs-lookup"><span data-stu-id="3b165-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="3b165-134">我們再次 EntityDataSource 控制項用來存取資料庫中的特定產品記錄，我們將使用 ASP.NET FormView 控制項來顯示產品的資料，如下所示。</span><span class="sxs-lookup"><span data-stu-id="3b165-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="3b165-135">別擔心，如果格式看起來是有點有趣給您。</span><span class="sxs-lookup"><span data-stu-id="3b165-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="3b165-136">上述標記挪出空間顯示版面配置中的幾個更新版本，我們將實作的功能。</span><span class="sxs-lookup"><span data-stu-id="3b165-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="3b165-137">「 購物車 」 將會代表我們的應用程式在更複雜的邏輯。</span><span class="sxs-lookup"><span data-stu-id="3b165-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="3b165-138">若要開始使用，請使用檔案-&gt;新增 以建立一個稱為 MyShoppingCart.aspx 的頁面。</span><span class="sxs-lookup"><span data-stu-id="3b165-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="3b165-139">請注意，我們不會選擇 ShoppingCart.aspx 的名稱。</span><span class="sxs-lookup"><span data-stu-id="3b165-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="3b165-140">我們的資料庫包含名為"ShoppingCart"的資料表。</span><span class="sxs-lookup"><span data-stu-id="3b165-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="3b165-141">當我們產生 Entity Data Model 類別被建立在資料庫中的每個資料表。</span><span class="sxs-lookup"><span data-stu-id="3b165-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="3b165-142">因此，實體資料模型會產生名為"ShoppingCart"實體類別。</span><span class="sxs-lookup"><span data-stu-id="3b165-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="3b165-143">我們無法編輯模型，讓我們可以使用我們的購物車實作該名稱，或將其延伸為我們的需求，但我們會選擇改為直接請選取可避免衝突的名稱。</span><span class="sxs-lookup"><span data-stu-id="3b165-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="3b165-144">另外值得注意的是，我們將建立簡單的購物車和購物車 」 顯示的內嵌功能的購物車 」 邏輯。</span><span class="sxs-lookup"><span data-stu-id="3b165-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="3b165-145">我們還可以選擇在完全不同的商務層中實作我們的購物車。</span><span class="sxs-lookup"><span data-stu-id="3b165-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3b165-146">[上一頁](tailspin-spyworks-part-3.md)
> [下一頁](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="3b165-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
