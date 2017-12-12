---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: "第 4 部分： 列出產品 |Microsoft 文件"
author: JoeStagner
description: "此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 4 部分涵蓋 GridView contr.清單的產品..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 420cdbcc002bcbfff619d399a7a374009999d754
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="part-4-listing-products"></a><span data-ttu-id="84c1d-104">第 4 部分： 列出產品</span><span class="sxs-lookup"><span data-stu-id="84c1d-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="84c1d-105">由[Joe stagner 以](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="84c1d-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="84c1d-106">Tailspin Spyworks 示範建立功能強大、 可調整的應用程式的.NET 平台是如何 hierarchy 簡單。</span><span class="sxs-lookup"><span data-stu-id="84c1d-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="84c1d-107">它會顯示如何使用 ASP.NET 4 中的強大的新功能，建置線上商店，包括購物、 簽出，以及管理關閉。</span><span class="sxs-lookup"><span data-stu-id="84c1d-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="84c1d-108">此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="84c1d-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="84c1d-109">第 4 部分涵蓋的 GridView 控制項的產品清單。</span><span class="sxs-lookup"><span data-stu-id="84c1d-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a><span data-ttu-id="84c1d-110">列出產品與 GridView 控制項</span><span class="sxs-lookup"><span data-stu-id="84c1d-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="84c1d-111">讓我們開始實作我們 ProductsList.aspx 頁面 」 以滑鼠右鍵按一下 「 我們的解決方案，然後選取 「 新增 」 和 「 新的項目 」。</span><span class="sxs-lookup"><span data-stu-id="84c1d-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="84c1d-112">選擇 Web 表單使用主版頁面 」，並輸入 ProductsList.aspx 頁面名稱 」。</span><span class="sxs-lookup"><span data-stu-id="84c1d-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="84c1d-113">按一下 [加入]。</span><span class="sxs-lookup"><span data-stu-id="84c1d-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="84c1d-114">接下來選擇放置於 Site.Master 頁面的 「 樣式 」 資料夾，然後從資料夾的 「 內容 」 視窗中選取。</span><span class="sxs-lookup"><span data-stu-id="84c1d-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="84c1d-115">按一下 「 確定 」 建立頁面。</span><span class="sxs-lookup"><span data-stu-id="84c1d-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="84c1d-116">我們的資料庫會填入產品資料，如下所示。</span><span class="sxs-lookup"><span data-stu-id="84c1d-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="84c1d-117">建立我們網頁之後我們試要用為實體資料來源來存取該產品的資料，但是我們需要這個執行個體中選取的產品實體，而且我們要限制傳回給只有選取的類別目錄的項目。</span><span class="sxs-lookup"><span data-stu-id="84c1d-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="84c1d-118">若要完成此我們會告訴 EntityDataSource 來自動產生 WHERE 子句，我們將會指定 WhereParameter。</span><span class="sxs-lookup"><span data-stu-id="84c1d-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="84c1d-119">請記得，當我們 [產品類別目錄功能表] 中建立功能表項目我們以動態方式建立連結 CatagoryID 加入每個連結的 QueryString。</span><span class="sxs-lookup"><span data-stu-id="84c1d-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="84c1d-120">我們會告訴實體資料來源是位置參數衍生自該查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="84c1d-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="84c1d-121">接下來，我們稍後會設定 ListView 控制項顯示的產品清單。</span><span class="sxs-lookup"><span data-stu-id="84c1d-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="84c1d-122">若要建立購物懽呏枔我們會壓縮數種精簡功能到我們 ListVew 中顯示每個個別產品。</span><span class="sxs-lookup"><span data-stu-id="84c1d-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="84c1d-123">產品名稱是產品的詳細資料檢視的連結。</span><span class="sxs-lookup"><span data-stu-id="84c1d-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="84c1d-124">將顯示產品的價格。</span><span class="sxs-lookup"><span data-stu-id="84c1d-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="84c1d-125">將顯示產品的映像，我們以動態方式將選取映像目錄從映像，在我們的應用程式。</span><span class="sxs-lookup"><span data-stu-id="84c1d-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="84c1d-126">我們將包括立即將特定的產品加入購物車的連結。</span><span class="sxs-lookup"><span data-stu-id="84c1d-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="84c1d-127">以下是我們的 ListView 控制項執行個體的標記。</span><span class="sxs-lookup"><span data-stu-id="84c1d-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="84c1d-128">我們以動態方式所建立每個顯示的產品數個的連結。</span><span class="sxs-lookup"><span data-stu-id="84c1d-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="84c1d-129">此外，我們測試自己的新頁面之前，我們需要建立目錄結構的產品類別目錄影像，如下所示。</span><span class="sxs-lookup"><span data-stu-id="84c1d-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="84c1d-130">可存取我們的產品映像之後，我們可以測試我們的產品清單頁面。</span><span class="sxs-lookup"><span data-stu-id="84c1d-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="84c1d-131">從網站的首頁上，按一下其中一個類別目錄清單連結。</span><span class="sxs-lookup"><span data-stu-id="84c1d-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="84c1d-132">現在我們需要實作 ProductDetials.apsx 頁面和 AddToCart 功能。</span><span class="sxs-lookup"><span data-stu-id="84c1d-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="84c1d-133">使用檔案-&gt;新增 建立 ProductDetails.aspx 如同我們先前使用的站台主版頁面的頁面名稱。</span><span class="sxs-lookup"><span data-stu-id="84c1d-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="84c1d-134">我們一次用來存取資料庫中的特定產品記錄 EntityDataSource 控制項，我們會使用 ASP.NET FormView 控制項來顯示產品資料，如下所示。</span><span class="sxs-lookup"><span data-stu-id="84c1d-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="84c1d-135">如果格式設定的位元居然看起來您，擔心。</span><span class="sxs-lookup"><span data-stu-id="84c1d-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="84c1d-136">上述的標記挪出空間顯示版面配置中的一組功能，我們將稍後在實作。</span><span class="sxs-lookup"><span data-stu-id="84c1d-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="84c1d-137">購物車將代表我們的應用程式在更複雜的邏輯。</span><span class="sxs-lookup"><span data-stu-id="84c1d-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="84c1d-138">若要開始使用，請使用檔案-&gt;新增 建立一個稱為 MyShoppingCart.aspx 的頁面。</span><span class="sxs-lookup"><span data-stu-id="84c1d-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="84c1d-139">請注意，我們不會選擇 ShoppingCart.aspx 的名稱。</span><span class="sxs-lookup"><span data-stu-id="84c1d-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="84c1d-140">我們的資料庫包含名為"ShoppingCart"的資料表。</span><span class="sxs-lookup"><span data-stu-id="84c1d-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="84c1d-141">我們產生實體資料模型時已針對資料庫中的每個資料表建立的類別。</span><span class="sxs-lookup"><span data-stu-id="84c1d-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="84c1d-142">因此，實體資料模型會產生名為"ShoppingCart"實體類別。</span><span class="sxs-lookup"><span data-stu-id="84c1d-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="84c1d-143">我們無法編輯此模型，讓我們無法使用該名稱，我們的購物車實作或擴充為我們的需求，但我們會選擇改為直接請選取將會避免衝突的名稱。</span><span class="sxs-lookup"><span data-stu-id="84c1d-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="84c1d-144">另外值得注意的是，我們將建立簡單購物車和內嵌的購物車顯示購物車邏輯。</span><span class="sxs-lookup"><span data-stu-id="84c1d-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="84c1d-145">我們也可能會選擇實作我們的購物車中完全不同的商務層。</span><span class="sxs-lookup"><span data-stu-id="84c1d-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="84c1d-146">[上一頁](tailspin-spyworks-part-3.md)
[下一頁](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="84c1d-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
