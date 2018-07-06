---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 第 3 部分： 版面配置與分類 功能表 |Microsoft Docs
author: JoeStagner
description: 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 3 部分涵蓋如何加入版面配置及類別目錄功能表項目。
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3047c53e21a418aef8617bd772a247bc46edb98f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817521"
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="8d94a-104">第 3 部分： 版面配置與分類 功能表</span><span class="sxs-lookup"><span data-stu-id="8d94a-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="8d94a-105">藉由[Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="8d94a-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="8d94a-106">Tailspin Spyworks 示範建立功能強大、 可擴充的應用程式，適用於.NET 平台是如何富含簡單。</span><span class="sxs-lookup"><span data-stu-id="8d94a-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="8d94a-107">它會展示如何在 ASP.NET 4 中使用最棒的新功能，建置線上商店，包括購物、 簽出，以及系統管理。</span><span class="sxs-lookup"><span data-stu-id="8d94a-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="8d94a-108">本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="8d94a-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="8d94a-109">第 3 部分涵蓋如何加入版面配置及類別目錄功能表項目。</span><span class="sxs-lookup"><span data-stu-id="8d94a-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a>  <span data-ttu-id="8d94a-110">新增一些版面配置和類別目錄功能表</span><span class="sxs-lookup"><span data-stu-id="8d94a-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="8d94a-111">在我們網站的主版頁面中，我們將新增包含我們的產品類別目錄 功能表左邊資料行的 div。</span><span class="sxs-lookup"><span data-stu-id="8d94a-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="8d94a-112">請注意，所需的對齊和其他格式會由我們加入我們的 Style.css 檔案的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="8d94a-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="8d94a-113">[產品類別目錄] 功能表將會以動態方式建立在執行階段藉由查詢商務資料庫的現有產品類別目錄以及建立功能表項目和對應的連結之用。</span><span class="sxs-lookup"><span data-stu-id="8d94a-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="8d94a-114">若要這麼做，我們將使用兩個 ASP。NET 的功能強大的資料控制項。</span><span class="sxs-lookup"><span data-stu-id="8d94a-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="8d94a-115">「 實體資料來源 」 控制項和"ListView"控制項。</span><span class="sxs-lookup"><span data-stu-id="8d94a-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="8d94a-116">讓我們切換到 [設計檢視]，並設定控制項中使用協助程式。</span><span class="sxs-lookup"><span data-stu-id="8d94a-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="8d94a-117">讓我們將 EntityDataSource 識別碼屬性設定為 EDS\_分類\_功能表，然後按一下 「 設定資料來源 」。</span><span class="sxs-lookup"><span data-stu-id="8d94a-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="8d94a-118">選取我們建立我們商務資料庫的實體資料來源模型時，為我們所建立的 CommerceEntities 連接，然後按一下 下一步 」。</span><span class="sxs-lookup"><span data-stu-id="8d94a-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="8d94a-119">選取 「 類別 」 實體集名稱，然後將其餘的選項為預設值。</span><span class="sxs-lookup"><span data-stu-id="8d94a-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="8d94a-120">按一下 [完成]。</span><span class="sxs-lookup"><span data-stu-id="8d94a-120">Click "Finish".</span></span>

<span data-ttu-id="8d94a-121">現在讓我們設定我們放在我們的頁面，即可 ListView 的 ListView 控制項執行個體的 ID 屬性\_ProductsMenu 並啟用其協助程式。</span><span class="sxs-lookup"><span data-stu-id="8d94a-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="8d94a-122">不過我們可以使用 控制選項來格式化資料的項目顯示和格式化，我們功能表建立只需要簡單的標記讓我們將輸入來源檢視中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="8d94a-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="8d94a-123">請注意"Eval"陳述式： &lt;# Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="8d94a-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="8d94a-124">使用 ASP.NET 語法&lt;%# %&gt;是指示執行階段執行任何內含和輸出的結果"Line"的縮寫慣例。</span><span class="sxs-lookup"><span data-stu-id="8d94a-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="8d94a-125">陳述式 Eval("CategoryName")，指示目前項目的繫結集合中的資料項目時，擷取實體模型項目名稱的值"CatagoryName 」。</span><span class="sxs-lookup"><span data-stu-id="8d94a-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="8d94a-126">這是簡潔的語法非常強大的功能。</span><span class="sxs-lookup"><span data-stu-id="8d94a-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="8d94a-127">讓我們執行應用程式現在。</span><span class="sxs-lookup"><span data-stu-id="8d94a-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="8d94a-128">請注意，現在會顯示我們的產品類別目錄 功能表，並當我們將滑鼠停留在其中一個類別目錄功能表項目，我們可以看到此功能表項目連結指向頁面，我們必須尚未實作名為 ProductsList.aspx，且我們已建置動態查詢字串引數，包含 類別目錄識別碼。</span><span class="sxs-lookup"><span data-stu-id="8d94a-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8d94a-129">[上一頁](tailspin-spyworks-part-2.md)
> [下一頁](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="8d94a-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
