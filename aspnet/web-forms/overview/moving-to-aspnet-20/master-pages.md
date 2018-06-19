---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: 主版頁面 |Microsoft 文件
author: microsoft
description: 其中一個成功的網站的關鍵元件是一致的外觀及操作。 在 ASP.NET 中 1.x，開發人員使用使用者控制項來複寫常見頁面 elem。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: f45dd9704f665244d2a48ec000326f6e98984e4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885096"
---
<a name="master-pages"></a><span data-ttu-id="5ffd7-104">主版頁面</span><span class="sxs-lookup"><span data-stu-id="5ffd7-104">Master Pages</span></span>
====================
<span data-ttu-id="5ffd7-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="5ffd7-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="5ffd7-106">其中一個成功的網站的關鍵元件是一致的外觀及操作。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-106">One of the key components to a successful Web site is a consistent look and feel.</span></span> <span data-ttu-id="5ffd7-107">在 ASP.NET 中 1.x，開發人員使用使用者控制項來跨 Web 應用程式複寫共同的頁面項目。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-107">In ASP.NET 1.x, developers used user controls to replicate common page elements across a Web application.</span></span> <span data-ttu-id="5ffd7-108">一定是可行的解決方案，使用使用者控制項也有一些缺點。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-108">While that is certainly a workable solution, using user controls does have some drawbacks.</span></span> <span data-ttu-id="5ffd7-109">比方說，使用者控制項的位置中的變更會在站台需要多個頁面的變更。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-109">For example, a change in position of a user control requires a change to multiple pages across a site.</span></span> <span data-ttu-id="5ffd7-110">使用者控制項也不會呈現之後所插入頁面的 [設計] 檢視中。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-110">User controls are also not rendered in Design view after being inserted on a page.</span></span>


<span data-ttu-id="5ffd7-111">其中一個成功的網站的關鍵元件是一致的外觀及操作。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-111">One of the key components to a successful Web site is a consistent look and feel.</span></span> <span data-ttu-id="5ffd7-112">在 ASP.NET 中 1.x，開發人員使用使用者控制項來跨 Web 應用程式複寫共同的頁面項目。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-112">In ASP.NET 1.x, developers used user controls to replicate common page elements across a Web application.</span></span> <span data-ttu-id="5ffd7-113">一定是可行的解決方案，使用使用者控制項也有一些缺點。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-113">While that is certainly a workable solution, using user controls does have some drawbacks.</span></span> <span data-ttu-id="5ffd7-114">比方說，使用者控制項的位置中的變更會在站台需要多個頁面的變更。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-114">For example, a change in position of a user control requires a change to multiple pages across a site.</span></span> <span data-ttu-id="5ffd7-115">使用者控制項也不會呈現之後所插入頁面的 [設計] 檢視中。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-115">User controls are also not rendered in Design view after being inserted on a page.</span></span>

<span data-ttu-id="5ffd7-116">ASP.NET 2.0 導入了主要頁面的方式來維護一致的外觀與風格，以及您很快就會知道主版頁面的使用者控制項方法代表顯著的改善。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-116">ASP.NET 2.0 introduces Master pages as a way of maintaining a consistent look and feel, and as you'll soon see, Master pages represent a significant improvement over the user control method.</span></span>

## <a name="why-master-pages"></a><span data-ttu-id="5ffd7-117">為什麼主版頁面嗎？</span><span class="sxs-lookup"><span data-stu-id="5ffd7-117">Why Master Pages?</span></span>

<span data-ttu-id="5ffd7-118">您可能會想知道為什麼主版頁面所需在 ASP.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-118">You may be wondering why master pages were needed in ASP.NET 2.0.</span></span> <span data-ttu-id="5ffd7-119">在所有情況下，網站開發人員已經使用使用者控制項在 ASP.NET 中 1.x 共用內容頁之間的區域。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-119">After all, Web site developers are already using user controls in ASP.NET 1.x to share content areas between pages.</span></span> <span data-ttu-id="5ffd7-120">沒有實際上有數種原因為何使用者控制項是無比最好的解決方案，來建立一般的版面配置。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-120">There are actually several reasons why user controls are a less-than-optimal solution for creating a common layout.</span></span>

<span data-ttu-id="5ffd7-121">使用者控制項不會實際定義的頁面配置。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-121">User controls don't actually define page layout.</span></span> <span data-ttu-id="5ffd7-122">相反地，他們定義的配置和網頁的部分功能。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-122">Instead, they define the layout and functionality for a portion of a page.</span></span> <span data-ttu-id="5ffd7-123">這兩個之間的差異很重要的因為它會讓使用者控制方案的管理性更為困難。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-123">The distinction between these two is important because it makes manageability of a user control solution much more difficult.</span></span> <span data-ttu-id="5ffd7-124">例如，當您想要變更您的頁面上的使用者控制項的位置，您必須編輯使用者控制項所顯示的實際頁。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-124">For example, when you want to change the position of a user control on your page, you must edit the actual page on which the user control appears.</span></span> <span data-ttu-id="5ffd7-125">Thats 精確，如果您有幾個頁面，但在大型網站，其很快會變得站台管理惡夢 ！</span><span class="sxs-lookup"><span data-stu-id="5ffd7-125">Thats fine if you have only a few pages, but in large sites, it quickly becomes a site management nightmare!</span></span>

<span data-ttu-id="5ffd7-126">定義通用的版面配置中使用使用者控制項的另一個缺點被根目錄結構中的 ASP.NET 本身。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-126">Another drawback of using user controls for defining a common layout is rooted in the architecture of ASP.NET itself.</span></span> <span data-ttu-id="5ffd7-127">如果變更使用者控制項的任何 public 成員，它會要求您重新編譯所有使用使用者控制項的頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-127">If any public member of a user control is changed, it requires you to recompile all of the pages that use the user control.</span></span> <span data-ttu-id="5ffd7-128">接著，ASP.NET 會重新編譯 JIT 網頁時，它們第一次存取。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-128">In turn, ASP.NET will then re-JIT your pages when they are first accessed.</span></span> <span data-ttu-id="5ffd7-129">這樣一來，同樣地，會產生不可擴充的架構和較大的站台的站台管理問題。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-129">This, once again, produces a non-scalable architecture and a site management problem for larger sites.</span></span>

<span data-ttu-id="5ffd7-130">在 ASP.NET 2.0 的主版頁面依妥善定址兩者的這些問題 （以及更多）。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-130">Both of these problems (and many more) are nicely addressed by master pages in ASP.NET 2.0.</span></span>

## <a name="how-master-pages-work"></a><span data-ttu-id="5ffd7-131">主版頁面的運作方式</span><span class="sxs-lookup"><span data-stu-id="5ffd7-131">How Master Pages Work</span></span>

<span data-ttu-id="5ffd7-132">主版頁面是類似於其他頁面的範本。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-132">A master page is analogous to a template for other pages.</span></span> <span data-ttu-id="5ffd7-133">應該由共用 （也就是功能表、 框線、 等等） 的其他頁面的頁面元素會加入至主版頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-133">Page elements that should be shared among other pages (i.e. menus, borders, etc.) are added to the master page.</span></span> <span data-ttu-id="5ffd7-134">當新頁面加入至網站時，您可以將它們與主版頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-134">When new pages are added to the site, you can associate them with a master page.</span></span> <span data-ttu-id="5ffd7-135">主版頁面與相關聯的頁面稱為**內容頁面**。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-135">A page that is associated with a master page is called a **content page**.</span></span> <span data-ttu-id="5ffd7-136">根據預設，內容頁面會從主版頁面外觀。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-136">By default, a content page takes on the appearance from the master page.</span></span> <span data-ttu-id="5ffd7-137">不過，當您建立主版頁面時，您可以定義內容的頁面可以使用自己的內容取代頁面上的部分。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-137">However, when you create a master page, you can define portions of the page that the content page can replace with its own content.</span></span> <span data-ttu-id="5ffd7-138">這些部分是透過定義在 ASP.NET 2.0; 引進了新的控制項**ContentPlaceHolder**控制項。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-138">These portions are defined using a new control introduced in ASP.NET 2.0; the **ContentPlaceHolder** control.</span></span>

<span data-ttu-id="5ffd7-139">主版頁面都可以包含任意數目的 ContentPlaceHolder 控制項 （或根本沒有）。在內容頁面上，從 ContentPlaceHolder 控制項的內容會出現內**內容**控制項、 ASP.NET 2.0 中的另一個新的控制項。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-139">A master page can contain any number of ContentPlaceHolder controls (or none at all.) On the content page, the content from the ContentPlaceHolder controls appears inside of **Content** controls, another new control in ASP.NET 2.0.</span></span> <span data-ttu-id="5ffd7-140">根據預設，內容控制項的內容頁面是空的讓您能夠提供您自己的內容。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-140">By default, the content pages Content controls are empty so that you can provide your own content.</span></span> <span data-ttu-id="5ffd7-141">如果您想要使用內容控制項內的主版頁面的內容，您可以因此當您會看到此模組中的更新版本。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-141">If you want to use the content from the master page inside of the Content controls, you can do so as you will see later in this module.</span></span> <span data-ttu-id="5ffd7-142">內容控制項會對應到 ContentPlaceHolder 控制項透過內容控制項的 ContentPlaceHolderID 屬性。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-142">The Content control is mapped to the ContentPlaceHolder control via the ContentPlaceHolderID attribute of the Content control.</span></span> <span data-ttu-id="5ffd7-143">對應將下列程式碼至主版頁面上呼叫 mainBody ContentPlaceHolder 控制項內容控制項。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-143">The code below maps a Content control to a ContentPlaceHolder control called mainBody on a master page.</span></span>

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> <span data-ttu-id="5ffd7-144">通常，您將聽到描述為其他頁面的基底類別的主版頁面的人員。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-144">You will often hear people describe master pages as being a base class for other pages.</span></span> <span data-ttu-id="5ffd7-145">Thats 實際上不正確。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-145">Thats actually not true.</span></span> <span data-ttu-id="5ffd7-146">主版頁面和內容頁面之間的關聯性不是其中一個繼承。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-146">The relationship between master pages and content pages is not one of inheritance.</span></span>


<span data-ttu-id="5ffd7-147">**圖 1**濆婞剢謅 Visual Studio 2005 會顯示在主版頁面和相關聯的內容頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-147">**Figure 1** shows a master page and an associated content page as they appear in Visual Studio 2005.</span></span> <span data-ttu-id="5ffd7-148">您可以看到的 ContentPlaceHolder 控制項在主版頁面和對應的內容控制項的內容頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-148">You can see the ContentPlaceHolder control in the master page and the corresponding Content control in the content page.</span></span> <span data-ttu-id="5ffd7-149">請注意，外部 ContentPlaceHolder 的主版頁面內容看得到，但是灰色內容頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-149">Notice that the master pages content that is outside of the ContentPlaceHolder is visible but grayed out in the content page.</span></span> <span data-ttu-id="5ffd7-150">ContentPlaceHolder 內的內容可以一般內容頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-150">Only the content inside of the ContentPlaceHolder can be supplanted by the content page.</span></span> <span data-ttu-id="5ffd7-151">來自主版頁面的所有其他內容不變。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-151">All other content that comes from the master page is immutable.</span></span>


![主版頁面和其相關聯的內容頁面](master-pages/_static/image1.jpg)

<span data-ttu-id="5ffd7-153">**圖 1**： 主版頁面和其相關聯的內容頁面</span><span class="sxs-lookup"><span data-stu-id="5ffd7-153">**Figure 1**: A master page and its associated content page</span></span>


## <a name="creating-a-master-page"></a><span data-ttu-id="5ffd7-154">建立主版頁面</span><span class="sxs-lookup"><span data-stu-id="5ffd7-154">Creating a Master Page</span></span>

<span data-ttu-id="5ffd7-155">若要建立新的主版頁面：</span><span class="sxs-lookup"><span data-stu-id="5ffd7-155">To create a new master page:</span></span>

1. <span data-ttu-id="5ffd7-156">開啟 Visual Studio 2005，並建立新的網站。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-156">Open Visual Studio 2005 and create a new Web site.</span></span>
2. <span data-ttu-id="5ffd7-157">按一下 新增檔案、 檔案。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-157">Click File, New, File.</span></span>
3. <span data-ttu-id="5ffd7-158">中所示，從 [加入新項目] 對話方塊選擇主要檔案**圖 2**。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-158">Choose Master File from the Add New Item dialog as shown in **figure 2**.</span></span>
4. <span data-ttu-id="5ffd7-159">按一下 新增。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-159">Click Add.</span></span>


![建立新的主版頁面](master-pages/_static/image2.jpg)

<span data-ttu-id="5ffd7-161">**圖 2**： 建立新的主版頁面</span><span class="sxs-lookup"><span data-stu-id="5ffd7-161">**Figure 2**: Creating a New Master Page</span></span>


<span data-ttu-id="5ffd7-162">請注意，在主版頁面檔案的副檔名是<em>.master</em>。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-162">Notice that the file extension for a master page is <em>.master</em>.</span></span> <span data-ttu-id="5ffd7-163">這是一種主版頁面不同於一般的頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-163">This is one of the ways that a master page differs from an ordinary page.</span></span> <span data-ttu-id="5ffd7-164">其主要的差別在於替代@Page指示詞，主版頁面包含@Master指示詞。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-164">The other primary difference is that in lieu of a @Page directive, the master page contains a @Master directive.</span></span> <span data-ttu-id="5ffd7-165">切換至來源檢視的主要頁面上您剛才建立和檢閱的程式碼。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-165">Switch to Source View for the master page youve just created and review the code.</span></span>

<span data-ttu-id="5ffd7-166">新的主版頁面預設將會擁有一個 ContentPlaceHolder 控制項。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-166">A new master page will have one ContentPlaceHolder control by default.</span></span> <span data-ttu-id="5ffd7-167">在大部分情況下，它更具意義先建立一般的頁面項目，然後插入 ContentPlaceHolder 控制項至想要自訂內容的地方。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-167">In most cases, it makes more sense to create the common page elements first and then insert ContentPlaceHolder controls where custom content is desired.</span></span> <span data-ttu-id="5ffd7-168">在這些情況下，開發人員會想要刪除預設 ContentPlaceHolder 控制項，並插入新的網頁開發。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-168">In those cases, developers will want to delete the default ContentPlaceHolder control and insert new ones as the page is developed.</span></span> <span data-ttu-id="5ffd7-169">ContentPlaceHolder 控制項不是可調整大小，儘管它們並顯示調整大小控點。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-169">ContentPlaceHolder controls are not resizable despite the fact that they do display sizing handles.</span></span> <span data-ttu-id="5ffd7-170">ContentPlaceHolder 控制項的大小會自動根據您有一個例外狀況; 它包含的內容如果您將區塊項目內的 ContentPlaceHolder 控制項例如表格儲存格時，它會根據項目的大小調整大小。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-170">The ContentPlaceHolder control sizes automatically based upon the content that it contains with one exception; if you place a ContentPlaceHolder control inside of a block element such as a table cell, it will size according to the size of the element.</span></span>

## <a name="lab-1-working-with-master-pages"></a><span data-ttu-id="5ffd7-171">實驗室 1 使用主版頁面</span><span class="sxs-lookup"><span data-stu-id="5ffd7-171">Lab 1 Working with Master Pages</span></span>

<span data-ttu-id="5ffd7-172">在此實驗室中，您會建立新的主版頁面和定義三個 ContentPlaceHolder 控制項。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-172">In this lab, you will create a new master page and define three ContentPlaceHolder controls.</span></span> <span data-ttu-id="5ffd7-173">您接著建立新的內容頁面，並取代其中至少一個 ContentPlaceHolder 控制項的內容。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-173">You will then create a new Content page and replace the content from at least one of the ContentPlaceHolder controls.</span></span>

1. <span data-ttu-id="5ffd7-174">建立主版頁面，並插入 ContentPlaceHolder 控制項。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-174">Create a master page and insert ContentPlaceHolder controls.</span></span> 

    1. <span data-ttu-id="5ffd7-175">如上面所述，建立新的主版頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-175">Create a new master page as described above.</span></span>
    2. <span data-ttu-id="5ffd7-176">刪除預設 ContentPlaceHolder 控制項。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-176">Delete the default ContentPlaceHolder control.</span></span>
    3. <span data-ttu-id="5ffd7-177">選取 ContentPlaceHolder 控制項按一下灰色上控制項的框線，然後按下 DEL 鍵鍵盤上的刪除。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-177">Select the ContentPlaceHolder control by clicking the shaded top border of the control and then delete it by hitting the DEL key on your keyboard.</span></span>
    4. <span data-ttu-id="5ffd7-178">插入新資料表使用*標頭和側邊*範本，如圖 3 所示。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-178">Insert a new table using the *Header and side* template as shown in figure 3.</span></span> <span data-ttu-id="5ffd7-179">設成 90%每個變更的寬度和高度，使整個資料表會顯示在設計工具中。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-179">Change the width and height to 90% each so that the entire table is visible in the designer.</span></span>


![](master-pages/_static/image3.jpg)

<span data-ttu-id="5ffd7-180">**圖 3**</span><span class="sxs-lookup"><span data-stu-id="5ffd7-180">**Figure 3**</span></span>


1. <span data-ttu-id="5ffd7-181">將游標放在資料表的每個資料格，並設定*valign*屬性*頂端*。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-181">Place the cursor into each cell of the table and set the *valign* property to *top*.</span></span>
2. <span data-ttu-id="5ffd7-182">從 [工具箱] 插入 ContentPlaceHolder 控制項在頂端資料格的資料表 （標頭資料格。）</span><span class="sxs-lookup"><span data-stu-id="5ffd7-182">From the Toolbox, insert a ContentPlaceHolder control in the top cell of the table (the header cell.)</span></span>
3. <span data-ttu-id="5ffd7-183">當您插入這個 ContentPlaceHolder 控制項時，您會注意到如圖 4 所示，資料列高度會佔用幾乎整個頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-183">When you insert this ContentPlaceHolder control, you will notice that the row height will take up almost the entire page as shown in figure 4.</span></span> <span data-ttu-id="5ffd7-184">請勿在此時在意了解。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-184">Dont be concerned about that at this point.</span></span>


![ContentPlaceHolder 為相同的資料格是空的空間](master-pages/_static/image1.gif)

<span data-ttu-id="5ffd7-186">**圖 4**: ContentPlaceHolder 為相同的資料格是空的空間</span><span class="sxs-lookup"><span data-stu-id="5ffd7-186">**Figure 4**: The empty space is in the same cell as the ContentPlaceHolder</span></span>


1. <span data-ttu-id="5ffd7-187">ContentPlaceHolder 將控制項放置在其他兩個資料格。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-187">Place a ContentPlaceHolder control in the other two cells.</span></span> <span data-ttu-id="5ffd7-188">插入其他 ContentPlaceHolder 控制項之後, 的表格儲存格大小應如您所預期。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-188">Once the other ContentPlaceHolder controls have been inserted, the size of the table cells should be as you would expect.</span></span> <span data-ttu-id="5ffd7-189">頁面現在看起來應該像所顯示的網頁**圖 5**。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-189">The page should now look like the page shown in **figure 5**.</span></span>


![所有的 ContentPlaceHolder 控制項與主機。](master-pages/_static/image2.gif)

<span data-ttu-id="5ffd7-192">**圖 5**: 母片 ContentPlaceHolder 的所有控制項。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-192">**Figure 5**: The Master with all ContentPlaceHolder controls.</span></span> <span data-ttu-id="5ffd7-193">請注意，標頭資料格的儲存格高度現在它應該是</span><span class="sxs-lookup"><span data-stu-id="5ffd7-193">Notice that the cell height for the header cell is now what it should be</span></span>


1. <span data-ttu-id="5ffd7-194">輸入您選擇的一些文字到每一個三個 ContentPlaceHolder 控制項。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-194">Enter some text of your choice into each of the three ContentPlaceHolder controls.</span></span>
2. <span data-ttu-id="5ffd7-195">將主版頁面儲存為 exercise1.master。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-195">Save the master page as exercise1.master.</span></span>
3. <span data-ttu-id="5ffd7-196">建立新的 Web 表單和其關聯 exercise1.master 主版頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-196">Create a new Web Form and associate it with the exercise1.master master page.</span></span>
4. <span data-ttu-id="5ffd7-197">選取新檔案，檔案在 Visual Studio 2005。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-197">Select File, New, File in Visual Studio 2005.</span></span>
5. <span data-ttu-id="5ffd7-198">選取**Web Form**在 [加入新項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-198">Select **Web Form** in the Add New Item dialog.</span></span>
6. <span data-ttu-id="5ffd7-199">請確定選取的主版頁面的核取方塊已核取，如圖 6 所示。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-199">Make sure that the Select master page checkbox is checked as shown in figure 6.</span></span>


![加入新的內容頁](master-pages/_static/image3.gif)

<span data-ttu-id="5ffd7-201">**圖 6**： 加入新的內容頁</span><span class="sxs-lookup"><span data-stu-id="5ffd7-201">**Figure 6**: Adding a new Content Page</span></span>


1. <span data-ttu-id="5ffd7-202">按一下 新增。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-202">Click Add.</span></span>
2. <span data-ttu-id="5ffd7-203">選取 exercise1.master 在 選取主版頁面對話方塊圖 7 所示。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-203">Select exercise1.master in the Select a master page dialog as shown in figure 7.</span></span>
3. <span data-ttu-id="5ffd7-204">按一下 [確定] 以加入新的內容頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-204">Click OK to add the new content page.</span></span>

<span data-ttu-id="5ffd7-205">新的內容頁面會出現在 Visual Studio 中，每個 ContentPlaceHolder 控制項在主版頁面的其中一個內容控制項。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-205">The new content page appears in Visual Studio with one Content control for each ContentPlaceHolder control on the master page.</span></span> <span data-ttu-id="5ffd7-206">根據預設，將內容控制項是空的好讓您可以加入您自己的內容。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-206">By default, the Content controls are empty so that you can add your own content.</span></span> <span data-ttu-id="5ffd7-207">如果 youd 喜歡的使用中的主版頁面的 ContentPlaceHolder 控制項的內容，只要按一下智慧標籤符號 （小型黑色箭頭控制項右上角），然後選擇*預設主機內容*從智慧標籤中所示**圖 8**。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-207">If youd like for them to use the content from the ContentPlaceHolder control on the master page, simply click on the smart tag symbol (the small black arrow in the upper-right corner of the control) and choose *Default to Masters Content* from the smart tag as shown in **figure 8**.</span></span> <span data-ttu-id="5ffd7-208">當您這樣做時，功能表項目變更為*建立自訂內容*。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-208">When you do so, the menu item changes to *Create Custom Content*.</span></span> <span data-ttu-id="5ffd7-209">按一下它在該點移除主版頁面可讓您定義該特定的內容控制項的自訂內容的內容。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-209">Clicking it at that point removes the content from the master page allowing you to define custom content for that particular Content control.</span></span>


![設定為預設為主版頁面的頁面內容的內容控制項](master-pages/_static/image4.gif)

<span data-ttu-id="5ffd7-211">**圖 7**： 將內容控制項設定為預設為主版頁面的頁面內容</span><span class="sxs-lookup"><span data-stu-id="5ffd7-211">**Figure 7**: Setting a Content Control to Default to the Master Pages Content</span></span>


## <a name="connecting-master-page-and-content-pages"></a><span data-ttu-id="5ffd7-212">連接主版頁面和內容頁面</span><span class="sxs-lookup"><span data-stu-id="5ffd7-212">Connecting Master Page and Content Pages</span></span>

<span data-ttu-id="5ffd7-213">主版頁面和內容頁面之間的關聯可以設定四種不同方式之一：</span><span class="sxs-lookup"><span data-stu-id="5ffd7-213">The association between a master page and a content page can be configured in one of four different ways:</span></span>

- <span data-ttu-id="5ffd7-214"><strong>MasterPageFile</strong>屬性@Page指示詞</span><span class="sxs-lookup"><span data-stu-id="5ffd7-214">The <strong>MasterPageFile</strong> attribute of the @Page directive</span></span>
- <span data-ttu-id="5ffd7-215">設定**Page.MasterPageFile**程式碼中的屬性。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-215">Setting the **Page.MasterPageFile** property in code.</span></span>
- <span data-ttu-id="5ffd7-216">**&lt;頁面&gt;** 應用程式組態檔 (web.config 應用程式的根資料夾中) 中的項目</span><span class="sxs-lookup"><span data-stu-id="5ffd7-216">The **&lt;pages&gt;** element in the applications configuration file (web.config in the root folder of the application)</span></span>
- <span data-ttu-id="5ffd7-217">**&lt;頁面&gt;** 子資料夾的組態檔 (位於子資料夾中的 web.config) 中的項目</span><span class="sxs-lookup"><span data-stu-id="5ffd7-217">The **&lt;pages&gt;** element in a subfolders configuration file (web.config in a subfolder)</span></span>

## <a name="masterpagefile-attribute"></a><span data-ttu-id="5ffd7-218">MasterPageFile 屬性</span><span class="sxs-lookup"><span data-stu-id="5ffd7-218">MasterPageFile Attribute</span></span>

<span data-ttu-id="5ffd7-219">MasterPageFile 屬性可讓您能輕鬆套用至特定的 ASP.NET 網頁的主版頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-219">The MasterPageFile attribute makes it easy to apply a master page to a particular ASP.NET page.</span></span> <span data-ttu-id="5ffd7-220">它也是用來檢查時套用的主版頁面的方法**選取主版頁面**核取方塊，當您未在練習 1 中。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-220">It is also the method used to apply the master page when you check the **Select Master Page** checkbox as you did in Exercise 1.</span></span>

## <a name="setting-pagemasterpagefile-in-code"></a><span data-ttu-id="5ffd7-221">程式碼中設定 Page.MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="5ffd7-221">Setting Page.MasterPageFile in Code</span></span>

<span data-ttu-id="5ffd7-222">程式碼中設定的 MasterPageFile 屬性，您可以套用特定的主版頁面，您在執行階段的內容。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-222">By setting the MasterPageFile property in code, you can apply a particular master page to your content at runtime.</span></span> <span data-ttu-id="5ffd7-223">這是適用於您可能需要套用特定的主版頁面，根據使用者角色或某些其他條件的情況。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-223">This is useful in cases where you may need to apply a specific master page based upon a users role or some other criteria.</span></span> <span data-ttu-id="5ffd7-224">MasterPageFile 屬性必須設定 PreInit 方法中。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-224">The MasterPageFile property must be set in the PreInit method.</span></span> <span data-ttu-id="5ffd7-225">如果它設定為 PreInit 方法之後，將會擲回 InvalidOperationException。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-225">If it is set after the PreInit method, an InvalidOperationException will be thrown.</span></span> <span data-ttu-id="5ffd7-226">頁面設定這個屬性也必須擁有內容控制項為頁面的最上層控制項。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-226">The page on which this property is being set must also have a Content control as the top-level control for the page.</span></span> <span data-ttu-id="5ffd7-227">否則 MasterPageFile 屬性設定時，HttpException 就會擲回。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-227">Otherwise an HttpException will be thrown when the MasterPageFile property is set.</span></span>

## <a name="using-the-ltpagesgt-element"></a><span data-ttu-id="5ffd7-228">使用&lt;頁面&gt;項目</span><span class="sxs-lookup"><span data-stu-id="5ffd7-228">Using the &lt;pages&gt; Element</span></span>

<span data-ttu-id="5ffd7-229">您可以為您的網頁設定主版頁面中設定的 masterPageFile 屬性&lt;頁面&gt;web.config 檔案的項目。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-229">You can configure a master page for your pages by setting the masterPageFile attribute in the &lt;pages&gt; element of the web.config file.</span></span> <span data-ttu-id="5ffd7-230">使用此方法時，請注意，應用程式結構中較低的 web.config 檔案，可以覆寫此設定。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-230">When using this method, keep in mind that web.config files lower in the application structure can override this setting.</span></span> <span data-ttu-id="5ffd7-231">在中設定任何 MasterPageFile 屬性@Page指示詞也會覆寫此設定。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-231">Any MasterPageFile attribute set in a @Page directive will also override this setting.</span></span> <span data-ttu-id="5ffd7-232">使用&lt;頁面&gt;項目可簡化建立<em>主要</em>會覆寫必要時在特定資料夾或檔案中的主版頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-232">Using the &lt;pages&gt; element makes it simple to create a <em>master</em> master page that can be overridden if necessary in particular folders or files.</span></span>

## <a name="properties-in-master-pages"></a><span data-ttu-id="5ffd7-233">主版頁面中的屬性</span><span class="sxs-lookup"><span data-stu-id="5ffd7-233">Properties in Master Pages</span></span>

<span data-ttu-id="5ffd7-234">主版頁面可以公開屬性只讓這些屬性的主版頁面內是公用。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-234">A master page can expose properties by simply making those properties public within the master page.</span></span> <span data-ttu-id="5ffd7-235">例如，下列的程式碼會定義名為 SomeProperty 的屬性：</span><span class="sxs-lookup"><span data-stu-id="5ffd7-235">For example, the following code defines a property called SomeProperty:</span></span>

[!code-csharp[Main](master-pages/samples/sample2.cs)]

<span data-ttu-id="5ffd7-236">若要存取 SomeProperty 屬性從 [內容] 頁面，您必須使用主要屬性如下所示：</span><span class="sxs-lookup"><span data-stu-id="5ffd7-236">To access the SomeProperty property from the Content page, you will need to use the Master property like so:</span></span>

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a><span data-ttu-id="5ffd7-237">巢狀主版頁面</span><span class="sxs-lookup"><span data-stu-id="5ffd7-237">Nesting Master Pages</span></span>

<span data-ttu-id="5ffd7-238">主版頁面是完美的解決方案，跨大型的 Web 應用程式確保一般的外觀及操作。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-238">Master pages are the perfect solution for ensuring a common look and feel across a large Web application.</span></span> <span data-ttu-id="5ffd7-239">不過，它不罕見有大型的站台共用的通用介面的某些部分，而其他部分共用不同的介面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-239">However, it's not uncommon to have certain parts of a large site share a common interface while other parts share a different interface.</span></span> <span data-ttu-id="5ffd7-240">若要解決這項需求，多重主版頁面是完美的解決方案。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-240">To address that need, multiple master pages are the perfect solution.</span></span> <span data-ttu-id="5ffd7-241">不過，該仍然不能處理大型應用程式可能有某些元件 （例如功能表，例如） 所共用的所有頁面和其他元件只有特定區段的站台所共用的事實。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-241">However, that still doesn't address the fact that a large application may have certain components (such as a menu, for example) that are shared among all pages and other components that are shared only among certain sections of the site.</span></span> <span data-ttu-id="5ffd7-242">該類型的情況中，巢狀主版頁面填入需要正確地切割。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-242">For that type of situation, nested master pages fill the need nicely.</span></span> <span data-ttu-id="5ffd7-243">如您所見，標準主版頁面包含在主版頁面和內容頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-243">As you've seen, a normal master page consists of a master page and a content page.</span></span> <span data-ttu-id="5ffd7-244">在巢狀主版頁面的情況下，有兩個主要的頁面。父主機和子系主機。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-244">In a nested master page situation, there are two master pages; a parent master and a child master.</span></span> <span data-ttu-id="5ffd7-245">子主版頁面也是內容頁面，且其主要父主版頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-245">The child master page is also a content page and its master is the parent master page.</span></span>

<span data-ttu-id="5ffd7-246">以下是典型的主版頁面的程式碼：</span><span class="sxs-lookup"><span data-stu-id="5ffd7-246">Here is the code for a typical master page:</span></span>

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

<span data-ttu-id="5ffd7-247">在巢狀的主要案例中，這會是父 master。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-247">In a nested master scenario, this would be the parent master.</span></span> <span data-ttu-id="5ffd7-248">另一個主版頁面會為其主版頁面中，使用此頁面，該程式碼會看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="5ffd7-248">Another master page would use this page as its master page, and that code would look like this:</span></span>

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

<span data-ttu-id="5ffd7-249">請注意，在此案例中，子主機也父主機的內容頁面。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-249">Note that in this scenario, the child master is also a content page for the parent master.</span></span> <span data-ttu-id="5ffd7-250">從父代的 ContentPlaceHolder 控制項取得其內容的內容控制項內的所有子主要內容隨即出現。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-250">All of the child master's content appears inside of a Content control that gets its content from the parent's ContentPlaceHolder control.</span></span>

> [!NOTE]
> <span data-ttu-id="5ffd7-251">找不到適用於巢狀主版頁面的設計工具支援。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-251">Designer support is not available for nested master pages.</span></span> <span data-ttu-id="5ffd7-252">當您開發使用巢狀主圖形時，您必須使用原始碼檢視。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-252">When you are developing using nested masters, you will need to use source view.</span></span>


<span data-ttu-id="5ffd7-253">這段影片會示範使用巢狀主版頁面的逐步解說。</span><span class="sxs-lookup"><span data-stu-id="5ffd7-253">This video shows a walkthrough of using nested master pages.</span></span>


![](master-pages/_static/image1.png)


[<span data-ttu-id="5ffd7-254">開啟全螢幕視訊</span><span class="sxs-lookup"><span data-stu-id="5ffd7-254">Open Full-Screen Video</span></span>](master-pages/_static/nested1.wmv)


![選取主版頁面](master-pages/_static/image4.jpg)

<span data-ttu-id="5ffd7-256">**圖 8**： 選取主版頁面</span><span class="sxs-lookup"><span data-stu-id="5ffd7-256">**Figure 8**: Selecting a Master Page</span></span>
