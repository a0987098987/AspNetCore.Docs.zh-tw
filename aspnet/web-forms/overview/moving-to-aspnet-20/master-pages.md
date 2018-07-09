---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: 主版頁面 |Microsoft Docs
author: microsoft
description: 其中一個成功的 Web 站台的主要元件是一致的外觀及操作。 在 ASP.NET 1.x 中，開發人員使用使用者控制項來複寫常見頁面 elem。...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: 631aa39c51601f1ae843079d8cd930f77b1093ab
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819211"
---
<a name="master-pages"></a><span data-ttu-id="c76ed-104">主版頁面</span><span class="sxs-lookup"><span data-stu-id="c76ed-104">Master Pages</span></span>
====================
<span data-ttu-id="c76ed-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c76ed-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c76ed-106">其中一個成功的 Web 站台的主要元件是一致的外觀及操作。</span><span class="sxs-lookup"><span data-stu-id="c76ed-106">One of the key components to a successful Web site is a consistent look and feel.</span></span> <span data-ttu-id="c76ed-107">在 ASP.NET 1.x 中，開發人員用使用者控制項來複寫通用頁面項目之間的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c76ed-107">In ASP.NET 1.x, developers used user controls to replicate common page elements across a Web application.</span></span> <span data-ttu-id="c76ed-108">雖然這當然是可行的解決方案，使用使用者控制項也有一些缺點。</span><span class="sxs-lookup"><span data-stu-id="c76ed-108">While that is certainly a workable solution, using user controls does have some drawbacks.</span></span> <span data-ttu-id="c76ed-109">比方說，在使用者控制項的位置的變更會在站台需要多個頁面的變更。</span><span class="sxs-lookup"><span data-stu-id="c76ed-109">For example, a change in position of a user control requires a change to multiple pages across a site.</span></span> <span data-ttu-id="c76ed-110">使用者控制項也不會呈現之後所插入到網頁上的 [設計] 檢視中。</span><span class="sxs-lookup"><span data-stu-id="c76ed-110">User controls are also not rendered in Design view after being inserted on a page.</span></span>


<span data-ttu-id="c76ed-111">其中一個成功的 Web 站台的主要元件是一致的外觀及操作。</span><span class="sxs-lookup"><span data-stu-id="c76ed-111">One of the key components to a successful Web site is a consistent look and feel.</span></span> <span data-ttu-id="c76ed-112">在 ASP.NET 1.x 中，開發人員用使用者控制項來複寫通用頁面項目之間的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c76ed-112">In ASP.NET 1.x, developers used user controls to replicate common page elements across a Web application.</span></span> <span data-ttu-id="c76ed-113">雖然這當然是可行的解決方案，使用使用者控制項也有一些缺點。</span><span class="sxs-lookup"><span data-stu-id="c76ed-113">While that is certainly a workable solution, using user controls does have some drawbacks.</span></span> <span data-ttu-id="c76ed-114">比方說，在使用者控制項的位置的變更會在站台需要多個頁面的變更。</span><span class="sxs-lookup"><span data-stu-id="c76ed-114">For example, a change in position of a user control requires a change to multiple pages across a site.</span></span> <span data-ttu-id="c76ed-115">使用者控制項也不會呈現之後所插入到網頁上的 [設計] 檢視中。</span><span class="sxs-lookup"><span data-stu-id="c76ed-115">User controls are also not rendered in Design view after being inserted on a page.</span></span>

<span data-ttu-id="c76ed-116">ASP.NET 2.0 引進主版頁面的方式來維護一致的外觀及操作，以及您馬上就可以看到，主要頁面代表顯著的改進，透過將使用者控制項的方法。</span><span class="sxs-lookup"><span data-stu-id="c76ed-116">ASP.NET 2.0 introduces Master pages as a way of maintaining a consistent look and feel, and as you'll soon see, Master pages represent a significant improvement over the user control method.</span></span>

## <a name="why-master-pages"></a><span data-ttu-id="c76ed-117">為什麼主版頁面嗎？</span><span class="sxs-lookup"><span data-stu-id="c76ed-117">Why Master Pages?</span></span>

<span data-ttu-id="c76ed-118">您可能會好奇為什麼主版頁面所需在 ASP.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="c76ed-118">You may be wondering why master pages were needed in ASP.NET 2.0.</span></span> <span data-ttu-id="c76ed-119">畢竟，網站開發人員已經使用使用者控制項在 ASP.NET 1.x 共用內容頁之間的區域。</span><span class="sxs-lookup"><span data-stu-id="c76ed-119">After all, Web site developers are already using user controls in ASP.NET 1.x to share content areas between pages.</span></span> <span data-ttu-id="c76ed-120">有實際原因使用者控制項都是較不比最好的解決方案，來建立常用的版面配置的幾個原因。</span><span class="sxs-lookup"><span data-stu-id="c76ed-120">There are actually several reasons why user controls are a less-than-optimal solution for creating a common layout.</span></span>

<span data-ttu-id="c76ed-121">使用者控制項未實際定義頁面配置。</span><span class="sxs-lookup"><span data-stu-id="c76ed-121">User controls don't actually define page layout.</span></span> <span data-ttu-id="c76ed-122">相反地，它們定義的配置和頁面的部分功能。</span><span class="sxs-lookup"><span data-stu-id="c76ed-122">Instead, they define the layout and functionality for a portion of a page.</span></span> <span data-ttu-id="c76ed-123">這兩個之間的差異很重要的因為它可讓使用者控制解決方案的管理性更為困難。</span><span class="sxs-lookup"><span data-stu-id="c76ed-123">The distinction between these two is important because it makes manageability of a user control solution much more difficult.</span></span> <span data-ttu-id="c76ed-124">比方說，當您想要變更使用者控制項在頁面上的位置，您必須編輯使用者控制項所在的實際頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-124">For example, when you want to change the position of a user control on your page, you must edit the actual page on which the user control appears.</span></span> <span data-ttu-id="c76ed-125">Thats 精確，如果您只有幾頁，但在大型的網站，它很快就會是站台管理惡夢一場 ！</span><span class="sxs-lookup"><span data-stu-id="c76ed-125">Thats fine if you have only a few pages, but in large sites, it quickly becomes a site management nightmare!</span></span>

<span data-ttu-id="c76ed-126">定義常用的版面配置中使用使用者控制項的另一個缺點是 ASP.NET 本身的架構為基礎。</span><span class="sxs-lookup"><span data-stu-id="c76ed-126">Another drawback of using user controls for defining a common layout is rooted in the architecture of ASP.NET itself.</span></span> <span data-ttu-id="c76ed-127">如果變更使用者控制項的任何 public 成員，它會要求您重新編譯所有使用的使用者控制項的頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-127">If any public member of a user control is changed, it requires you to recompile all of the pages that use the user control.</span></span> <span data-ttu-id="c76ed-128">接著，ASP.NET 會則 opakovanou kompilaci JIT 時第一個頁面存取。</span><span class="sxs-lookup"><span data-stu-id="c76ed-128">In turn, ASP.NET will then re-JIT your pages when they are first accessed.</span></span> <span data-ttu-id="c76ed-129">如此一來，同樣地，會產生不可擴充的架構和更大的站台的站台管理的問題。</span><span class="sxs-lookup"><span data-stu-id="c76ed-129">This, once again, produces a non-scalable architecture and a site management problem for larger sites.</span></span>

<span data-ttu-id="c76ed-130">這兩個這些問題 （及更多） 會妥善解決在 ASP.NET 2.0 主版頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-130">Both of these problems (and many more) are nicely addressed by master pages in ASP.NET 2.0.</span></span>

## <a name="how-master-pages-work"></a><span data-ttu-id="c76ed-131">主版頁面的運作方式</span><span class="sxs-lookup"><span data-stu-id="c76ed-131">How Master Pages Work</span></span>

<span data-ttu-id="c76ed-132">主版頁面相當於其他頁面的範本。</span><span class="sxs-lookup"><span data-stu-id="c76ed-132">A master page is analogous to a template for other pages.</span></span> <span data-ttu-id="c76ed-133">應該在其他頁面 （也就是功能表中，框線等） 之間共用的頁面元素會新增至主版頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-133">Page elements that should be shared among other pages (i.e. menus, borders, etc.) are added to the master page.</span></span> <span data-ttu-id="c76ed-134">當新頁面加入至站台時，您可以將它們關聯的主版頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-134">When new pages are added to the site, you can associate them with a master page.</span></span> <span data-ttu-id="c76ed-135">主版頁面與相關聯的頁面會呼叫**內容頁面**。</span><span class="sxs-lookup"><span data-stu-id="c76ed-135">A page that is associated with a master page is called a **content page**.</span></span> <span data-ttu-id="c76ed-136">根據預設，內容頁面會從主版頁面的外觀。</span><span class="sxs-lookup"><span data-stu-id="c76ed-136">By default, a content page takes on the appearance from the master page.</span></span> <span data-ttu-id="c76ed-137">不過，當您建立主版頁面，您可以定義內容頁面可以取代自身的內容頁面上的部分。</span><span class="sxs-lookup"><span data-stu-id="c76ed-137">However, when you create a master page, you can define portions of the page that the content page can replace with its own content.</span></span> <span data-ttu-id="c76ed-138">這些部分定義時，使用 ASP.NET 2.0; 中導入新的控制項**ContentPlaceHolder**控制項。</span><span class="sxs-lookup"><span data-stu-id="c76ed-138">These portions are defined using a new control introduced in ASP.NET 2.0; the **ContentPlaceHolder** control.</span></span>

<span data-ttu-id="c76ed-139">主版頁面可以包含任意數目的 ContentPlaceHolder 控制項 （或根本沒有）。在 [內容] 頁面 ContentPlaceHolder 控制項的內容會出現內**內容**控制項、 ASP.NET 2.0 中的另一個新控制項。</span><span class="sxs-lookup"><span data-stu-id="c76ed-139">A master page can contain any number of ContentPlaceHolder controls (or none at all.) On the content page, the content from the ContentPlaceHolder controls appears inside of **Content** controls, another new control in ASP.NET 2.0.</span></span> <span data-ttu-id="c76ed-140">根據預設，內容控制項的內容頁面是空的以便您可以提供您自己的內容。</span><span class="sxs-lookup"><span data-stu-id="c76ed-140">By default, the content pages Content controls are empty so that you can provide your own content.</span></span> <span data-ttu-id="c76ed-141">如果您想要使用內容控制項內的主版頁面的內容，則可以因此當您將看到此模組中的更新版本。</span><span class="sxs-lookup"><span data-stu-id="c76ed-141">If you want to use the content from the master page inside of the Content controls, you can do so as you will see later in this module.</span></span> <span data-ttu-id="c76ed-142">內容控制項對應至 ContentPlaceHolder 控制項透過 ContentPlaceHolderID 屬性的內容控制項。</span><span class="sxs-lookup"><span data-stu-id="c76ed-142">The Content control is mapped to the ContentPlaceHolder control via the ContentPlaceHolderID attribute of the Content control.</span></span> <span data-ttu-id="c76ed-143">將內容控制項對應以下的程式碼至主版頁面上呼叫 mainBody ContentPlaceHolder 控制項中。</span><span class="sxs-lookup"><span data-stu-id="c76ed-143">The code below maps a Content control to a ContentPlaceHolder control called mainBody on a master page.</span></span>

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> <span data-ttu-id="c76ed-144">您通常會聽到描述為其他頁面的基底類別的主版頁面的人員。</span><span class="sxs-lookup"><span data-stu-id="c76ed-144">You will often hear people describe master pages as being a base class for other pages.</span></span> <span data-ttu-id="c76ed-145">Thats 實際上不為 true。</span><span class="sxs-lookup"><span data-stu-id="c76ed-145">Thats actually not true.</span></span> <span data-ttu-id="c76ed-146">主版頁面和內容頁面之間的關聯性不是其中一個繼承。</span><span class="sxs-lookup"><span data-stu-id="c76ed-146">The relationship between master pages and content pages is not one of inheritance.</span></span>


<span data-ttu-id="c76ed-147">**圖 1**濆婞剢謅 Visual Studio 2005 會顯示主版頁面與相關聯的內容頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-147">**Figure 1** shows a master page and an associated content page as they appear in Visual Studio 2005.</span></span> <span data-ttu-id="c76ed-148">您可以看到 ContentPlaceHolder 控制項中的主版頁面和對應內容控制項，在 [內容] 頁面中的。</span><span class="sxs-lookup"><span data-stu-id="c76ed-148">You can see the ContentPlaceHolder control in the master page and the corresponding Content control in the content page.</span></span> <span data-ttu-id="c76ed-149">請注意，主版頁面內容超出 ContentPlaceHolder 可見但灰色 [內容] 頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-149">Notice that the master pages content that is outside of the ContentPlaceHolder is visible but grayed out in the content page.</span></span> <span data-ttu-id="c76ed-150">ContentPlaceHolder 內的內容可以取代由內容頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-150">Only the content inside of the ContentPlaceHolder can be supplanted by the content page.</span></span> <span data-ttu-id="c76ed-151">來自主版頁面的所有其他內容永遠不變。</span><span class="sxs-lookup"><span data-stu-id="c76ed-151">All other content that comes from the master page is immutable.</span></span>


![主版頁面和其相關聯的內容頁面](master-pages/_static/image1.jpg)

<span data-ttu-id="c76ed-153">**圖 1**： 主版頁面和其相關聯的內容頁面</span><span class="sxs-lookup"><span data-stu-id="c76ed-153">**Figure 1**: A master page and its associated content page</span></span>


## <a name="creating-a-master-page"></a><span data-ttu-id="c76ed-154">建立主版頁面</span><span class="sxs-lookup"><span data-stu-id="c76ed-154">Creating a Master Page</span></span>

<span data-ttu-id="c76ed-155">若要建立新的主版頁面：</span><span class="sxs-lookup"><span data-stu-id="c76ed-155">To create a new master page:</span></span>

1. <span data-ttu-id="c76ed-156">開啟 Visual Studio 2005 並建立新的網站。</span><span class="sxs-lookup"><span data-stu-id="c76ed-156">Open Visual Studio 2005 and create a new Web site.</span></span>
2. <span data-ttu-id="c76ed-157">按一下 新增檔案、 檔案。</span><span class="sxs-lookup"><span data-stu-id="c76ed-157">Click File, New, File.</span></span>
3. <span data-ttu-id="c76ed-158">從 [加入新項目] 對話方塊選擇主要檔案，如中所示**圖 2**。</span><span class="sxs-lookup"><span data-stu-id="c76ed-158">Choose Master File from the Add New Item dialog as shown in **figure 2**.</span></span>
4. <span data-ttu-id="c76ed-159">按一下 新增。</span><span class="sxs-lookup"><span data-stu-id="c76ed-159">Click Add.</span></span>


![建立新的主版頁面](master-pages/_static/image2.jpg)

<span data-ttu-id="c76ed-161">**圖 2**： 建立新的主版頁面</span><span class="sxs-lookup"><span data-stu-id="c76ed-161">**Figure 2**: Creating a New Master Page</span></span>


<span data-ttu-id="c76ed-162">請注意，主版頁面檔案的副檔名<em>.master</em>。</span><span class="sxs-lookup"><span data-stu-id="c76ed-162">Notice that the file extension for a master page is <em>.master</em>.</span></span> <span data-ttu-id="c76ed-163">這是其中一種主版頁面不同於一般的頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-163">This is one of the ways that a master page differs from an ordinary page.</span></span> <span data-ttu-id="c76ed-164">其主要的差異在於替代@Page指示詞時，主版頁面包含@Master指示詞。</span><span class="sxs-lookup"><span data-stu-id="c76ed-164">The other primary difference is that in lieu of a @Page directive, the master page contains a @Master directive.</span></span> <span data-ttu-id="c76ed-165">切換至來源檢視的主要頁面，您剛剛建立，並檢閱程式碼。</span><span class="sxs-lookup"><span data-stu-id="c76ed-165">Switch to Source View for the master page youve just created and review the code.</span></span>

<span data-ttu-id="c76ed-166">新的主版頁面預設會有一個 ContentPlaceHolder 控制項。</span><span class="sxs-lookup"><span data-stu-id="c76ed-166">A new master page will have one ContentPlaceHolder control by default.</span></span> <span data-ttu-id="c76ed-167">在大部分情況下，較為合理要先建立一般的頁面項目，然後插入 ContentPlaceHolder 控制項自訂內容想要的地方。</span><span class="sxs-lookup"><span data-stu-id="c76ed-167">In most cases, it makes more sense to create the common page elements first and then insert ContentPlaceHolder controls where custom content is desired.</span></span> <span data-ttu-id="c76ed-168">在這些情況下，開發人員會想要刪除預設 ContentPlaceHolder 控制項，並插入新的開發頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-168">In those cases, developers will want to delete the default ContentPlaceHolder control and insert new ones as the page is developed.</span></span> <span data-ttu-id="c76ed-169">ContentPlaceHolder 控制項不是可調整大小，儘管它們並顯示調整大小控點事實。</span><span class="sxs-lookup"><span data-stu-id="c76ed-169">ContentPlaceHolder controls are not resizable despite the fact that they do display sizing handles.</span></span> <span data-ttu-id="c76ed-170">ContentPlaceHolder 控制項的大小會自動根據它有一個例外狀況; 包含的內容如果您將 ContentPlaceHolder 控制項內的區塊項目例如表格儲存格時，它會根據項目的大小的大小。</span><span class="sxs-lookup"><span data-stu-id="c76ed-170">The ContentPlaceHolder control sizes automatically based upon the content that it contains with one exception; if you place a ContentPlaceHolder control inside of a block element such as a table cell, it will size according to the size of the element.</span></span>

## <a name="lab-1-working-with-master-pages"></a><span data-ttu-id="c76ed-171">實驗室 1 使用主版頁面</span><span class="sxs-lookup"><span data-stu-id="c76ed-171">Lab 1 Working with Master Pages</span></span>

<span data-ttu-id="c76ed-172">在此實驗室中，您會建立新的主版頁面，並定義三個 ContentPlaceHolder 控制項。</span><span class="sxs-lookup"><span data-stu-id="c76ed-172">In this lab, you will create a new master page and define three ContentPlaceHolder controls.</span></span> <span data-ttu-id="c76ed-173">然後，您會建立新的內容頁面，並將至少其中一個 ContentPlaceHolder 控制項的內容。</span><span class="sxs-lookup"><span data-stu-id="c76ed-173">You will then create a new Content page and replace the content from at least one of the ContentPlaceHolder controls.</span></span>

1. <span data-ttu-id="c76ed-174">建立主版頁面，並插入 ContentPlaceHolder 控制項。</span><span class="sxs-lookup"><span data-stu-id="c76ed-174">Create a master page and insert ContentPlaceHolder controls.</span></span> 

    1. <span data-ttu-id="c76ed-175">如上面所述，請建立新的主版頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-175">Create a new master page as described above.</span></span>
    2. <span data-ttu-id="c76ed-176">刪除預設 ContentPlaceHolder 控制項。</span><span class="sxs-lookup"><span data-stu-id="c76ed-176">Delete the default ContentPlaceHolder control.</span></span>
    3. <span data-ttu-id="c76ed-177">選取 ContentPlaceHolder 控制項按一下控制項的陰影的框線，然後加以刪除，請按下鍵盤上的 DEL 鍵。</span><span class="sxs-lookup"><span data-stu-id="c76ed-177">Select the ContentPlaceHolder control by clicking the shaded top border of the control and then delete it by hitting the DEL key on your keyboard.</span></span>
    4. <span data-ttu-id="c76ed-178">插入新的資料表使用*標頭和側邊*範本，如 圖 3 所示。</span><span class="sxs-lookup"><span data-stu-id="c76ed-178">Insert a new table using the *Header and side* template as shown in figure 3.</span></span> <span data-ttu-id="c76ed-179">90%每個變更的寬度和高度，使整個資料表會顯示在設計工具。</span><span class="sxs-lookup"><span data-stu-id="c76ed-179">Change the width and height to 90% each so that the entire table is visible in the designer.</span></span>


![](master-pages/_static/image3.jpg)

<span data-ttu-id="c76ed-180">**圖 3**</span><span class="sxs-lookup"><span data-stu-id="c76ed-180">**Figure 3**</span></span>


1. <span data-ttu-id="c76ed-181">將游標放在資料表的每個資料格，並設定*valign*屬性設*頂端*。</span><span class="sxs-lookup"><span data-stu-id="c76ed-181">Place the cursor into each cell of the table and set the *valign* property to *top*.</span></span>
2. <span data-ttu-id="c76ed-182">從 [工具箱] 中，插入 ContentPlaceHolder 控制項在上方表格的儲存格 （標頭資料格。）</span><span class="sxs-lookup"><span data-stu-id="c76ed-182">From the Toolbox, insert a ContentPlaceHolder control in the top cell of the table (the header cell.)</span></span>
3. <span data-ttu-id="c76ed-183">當您將這個 ContentPlaceHolder 控制項時，您會發現 圖 4 所示，將資料列高度會花費幾乎整個頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-183">When you insert this ContentPlaceHolder control, you will notice that the row height will take up almost the entire page as shown in figure 4.</span></span> <span data-ttu-id="c76ed-184">不需要考量，此時。</span><span class="sxs-lookup"><span data-stu-id="c76ed-184">Dont be concerned about that at this point.</span></span>


![空白空間位於相同的儲存格為 ContentPlaceHolder](master-pages/_static/image1.gif)

<span data-ttu-id="c76ed-186">**圖 4**： 空白空間位於相同的儲存格為 ContentPlaceHolder</span><span class="sxs-lookup"><span data-stu-id="c76ed-186">**Figure 4**: The empty space is in the same cell as the ContentPlaceHolder</span></span>


1. <span data-ttu-id="c76ed-187">ContentPlaceHolder 將控制項放置在其他兩個資料格。</span><span class="sxs-lookup"><span data-stu-id="c76ed-187">Place a ContentPlaceHolder control in the other two cells.</span></span> <span data-ttu-id="c76ed-188">一旦已插入其他 ContentPlaceHolder 控制項，如您所預期，應該是表格儲存格的大小。</span><span class="sxs-lookup"><span data-stu-id="c76ed-188">Once the other ContentPlaceHolder controls have been inserted, the size of the table cells should be as you would expect.</span></span> <span data-ttu-id="c76ed-189">頁面現在看起來應該像中所顯示的網頁**圖 5**。</span><span class="sxs-lookup"><span data-stu-id="c76ed-189">The page should now look like the page shown in **figure 5**.</span></span>


![與所有 ContentPlaceHolder 控制項 Master。](master-pages/_static/image2.gif)

<span data-ttu-id="c76ed-192">**圖 5**: 主要與所有 ContentPlaceHolder 控制項。</span><span class="sxs-lookup"><span data-stu-id="c76ed-192">**Figure 5**: The Master with all ContentPlaceHolder controls.</span></span> <span data-ttu-id="c76ed-193">請注意，標頭資料格的儲存格高度現在它應該是</span><span class="sxs-lookup"><span data-stu-id="c76ed-193">Notice that the cell height for the header cell is now what it should be</span></span>


1. <span data-ttu-id="c76ed-194">輸入您選擇的一些文字到三個 ContentPlaceHolder 控制項的每個。</span><span class="sxs-lookup"><span data-stu-id="c76ed-194">Enter some text of your choice into each of the three ContentPlaceHolder controls.</span></span>
2. <span data-ttu-id="c76ed-195">將主版頁面儲存為 exercise1.master 中。</span><span class="sxs-lookup"><span data-stu-id="c76ed-195">Save the master page as exercise1.master.</span></span>
3. <span data-ttu-id="c76ed-196">建立新的 Web 表單，並將它與 exercise1.master 主版頁面產生關聯。</span><span class="sxs-lookup"><span data-stu-id="c76ed-196">Create a new Web Form and associate it with the exercise1.master master page.</span></span>
4. <span data-ttu-id="c76ed-197">選取 新增檔案，Visual Studio 2005 中的檔案。</span><span class="sxs-lookup"><span data-stu-id="c76ed-197">Select File, New, File in Visual Studio 2005.</span></span>
5. <span data-ttu-id="c76ed-198">選取  **Web Form**在 加入新項目 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="c76ed-198">Select **Web Form** in the Add New Item dialog.</span></span>
6. <span data-ttu-id="c76ed-199">請確定選取的主版頁面的核取方塊已核取，如 圖 6 所示。</span><span class="sxs-lookup"><span data-stu-id="c76ed-199">Make sure that the Select master page checkbox is checked as shown in figure 6.</span></span>


![加入新的 [內容] 頁面](master-pages/_static/image3.gif)

<span data-ttu-id="c76ed-201">**圖 6**： 加入新的 內容 頁面</span><span class="sxs-lookup"><span data-stu-id="c76ed-201">**Figure 6**: Adding a new Content Page</span></span>


1. <span data-ttu-id="c76ed-202">按一下 新增。</span><span class="sxs-lookup"><span data-stu-id="c76ed-202">Click Add.</span></span>
2. <span data-ttu-id="c76ed-203">選取 exercise1.master 在 選取主版頁面對話方塊如 圖 7 所示。</span><span class="sxs-lookup"><span data-stu-id="c76ed-203">Select exercise1.master in the Select a master page dialog as shown in figure 7.</span></span>
3. <span data-ttu-id="c76ed-204">按一下 [確定] 以加入新的內容頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-204">Click OK to add the new content page.</span></span>

<span data-ttu-id="c76ed-205">新的內容頁面會出現在 Visual Studio 中，每個 ContentPlaceHolder 控制項在主版頁面的其中一個內容控制項。</span><span class="sxs-lookup"><span data-stu-id="c76ed-205">The new content page appears in Visual Studio with one Content control for each ContentPlaceHolder control on the master page.</span></span> <span data-ttu-id="c76ed-206">根據預設，內容控制項是空的好讓您可以加入自己的內容。</span><span class="sxs-lookup"><span data-stu-id="c76ed-206">By default, the Content controls are empty so that you can add your own content.</span></span> <span data-ttu-id="c76ed-207">如果過您要使用來自 ContentPlaceHolder 控制項在主版頁面的內容，只要按一下智慧標籤符號 （小型黑色箭號控制項右上角），然後選擇*預設為 主機內容*從智慧標籤所示**圖 8**。</span><span class="sxs-lookup"><span data-stu-id="c76ed-207">If youd like for them to use the content from the ContentPlaceHolder control on the master page, simply click on the smart tag symbol (the small black arrow in the upper-right corner of the control) and choose *Default to Masters Content* from the smart tag as shown in **figure 8**.</span></span> <span data-ttu-id="c76ed-208">當您這樣做時，功能表項目變更為*建立自訂內容*。</span><span class="sxs-lookup"><span data-stu-id="c76ed-208">When you do so, the menu item changes to *Create Custom Content*.</span></span> <span data-ttu-id="c76ed-209">此時，按一下它，是在主版頁面可讓您定義該特定的內容控制項的自訂內容中移除內容。</span><span class="sxs-lookup"><span data-stu-id="c76ed-209">Clicking it at that point removes the content from the master page allowing you to define custom content for that particular Content control.</span></span>


![設定預設主版頁面內容的內容控制項](master-pages/_static/image4.gif)

<span data-ttu-id="c76ed-211">**圖 7**： 將內容控制項設定為預設主版頁面內容</span><span class="sxs-lookup"><span data-stu-id="c76ed-211">**Figure 7**: Setting a Content Control to Default to the Master Pages Content</span></span>


## <a name="connecting-master-page-and-content-pages"></a><span data-ttu-id="c76ed-212">連線的主版頁面和內容頁面</span><span class="sxs-lookup"><span data-stu-id="c76ed-212">Connecting Master Page and Content Pages</span></span>

<span data-ttu-id="c76ed-213">可以使用四種不同方式的其中一個設定主版頁面和內容頁面之間的關聯：</span><span class="sxs-lookup"><span data-stu-id="c76ed-213">The association between a master page and a content page can be configured in one of four different ways:</span></span>

- <span data-ttu-id="c76ed-214"><strong>MasterPageFile</strong>屬性@Page指示詞</span><span class="sxs-lookup"><span data-stu-id="c76ed-214">The <strong>MasterPageFile</strong> attribute of the @Page directive</span></span>
- <span data-ttu-id="c76ed-215">設定**Page.MasterPageFile**在程式碼中的屬性。</span><span class="sxs-lookup"><span data-stu-id="c76ed-215">Setting the **Page.MasterPageFile** property in code.</span></span>
- <span data-ttu-id="c76ed-216">**&lt;頁&gt;** 應用程式組態檔 (web.config 應用程式的根資料夾中) 中的項目</span><span class="sxs-lookup"><span data-stu-id="c76ed-216">The **&lt;pages&gt;** element in the applications configuration file (web.config in the root folder of the application)</span></span>
- <span data-ttu-id="c76ed-217">**&lt;頁&gt;** 子組態檔 (web.config 的子資料夾中) 中的項目</span><span class="sxs-lookup"><span data-stu-id="c76ed-217">The **&lt;pages&gt;** element in a subfolders configuration file (web.config in a subfolder)</span></span>

## <a name="masterpagefile-attribute"></a><span data-ttu-id="c76ed-218">MasterPageFile 屬性</span><span class="sxs-lookup"><span data-stu-id="c76ed-218">MasterPageFile Attribute</span></span>

<span data-ttu-id="c76ed-219">MasterPageFile 屬性可讓您能輕鬆套用到特定的 ASP.NET 頁面的主版頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-219">The MasterPageFile attribute makes it easy to apply a master page to a particular ASP.NET page.</span></span> <span data-ttu-id="c76ed-220">它也是用來套用主版頁面，當您選取的方法**選取主版頁面**核取方塊，當您未在練習 1 中。</span><span class="sxs-lookup"><span data-stu-id="c76ed-220">It is also the method used to apply the master page when you check the **Select Master Page** checkbox as you did in Exercise 1.</span></span>

## <a name="setting-pagemasterpagefile-in-code"></a><span data-ttu-id="c76ed-221">程式碼中設定 Page.MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="c76ed-221">Setting Page.MasterPageFile in Code</span></span>

<span data-ttu-id="c76ed-222">藉由在程式碼中設定的 MasterPageFile 屬性，您可以套用特定的主版頁面，您在執行階段的內容。</span><span class="sxs-lookup"><span data-stu-id="c76ed-222">By setting the MasterPageFile property in code, you can apply a particular master page to your content at runtime.</span></span> <span data-ttu-id="c76ed-223">這種情況下，您可能需要套用特定的主版頁面，根據使用者角色或其他準則。</span><span class="sxs-lookup"><span data-stu-id="c76ed-223">This is useful in cases where you may need to apply a specific master page based upon a users role or some other criteria.</span></span> <span data-ttu-id="c76ed-224">MasterPageFile 屬性必須設定 PreInit 方法中。</span><span class="sxs-lookup"><span data-stu-id="c76ed-224">The MasterPageFile property must be set in the PreInit method.</span></span> <span data-ttu-id="c76ed-225">如果它設定為 PreInit 方法之後，將會擲回 InvalidOperationException。</span><span class="sxs-lookup"><span data-stu-id="c76ed-225">If it is set after the PreInit method, an InvalidOperationException will be thrown.</span></span> <span data-ttu-id="c76ed-226">頁面設定這個屬性也必須有內容為頁面的最上層控制項的控制項。</span><span class="sxs-lookup"><span data-stu-id="c76ed-226">The page on which this property is being set must also have a Content control as the top-level control for the page.</span></span> <span data-ttu-id="c76ed-227">否則 MasterPageFile 屬性設定時，HttpException 就會擲回。</span><span class="sxs-lookup"><span data-stu-id="c76ed-227">Otherwise an HttpException will be thrown when the MasterPageFile property is set.</span></span>

## <a name="using-the-ltpagesgt-element"></a><span data-ttu-id="c76ed-228">使用&lt;頁&gt;項目</span><span class="sxs-lookup"><span data-stu-id="c76ed-228">Using the &lt;pages&gt; Element</span></span>

<span data-ttu-id="c76ed-229">您可以為您的網頁設定主版頁面中設定的 masterPageFile 屬性&lt;頁&gt;web.config 檔案的項目。</span><span class="sxs-lookup"><span data-stu-id="c76ed-229">You can configure a master page for your pages by setting the masterPageFile attribute in the &lt;pages&gt; element of the web.config file.</span></span> <span data-ttu-id="c76ed-230">使用此方法時，記住應用程式結構中較低的 web.config 檔案，可以覆寫此設定。</span><span class="sxs-lookup"><span data-stu-id="c76ed-230">When using this method, keep in mind that web.config files lower in the application structure can override this setting.</span></span> <span data-ttu-id="c76ed-231">在中設定任何 MasterPageFile 屬性@Page指示詞也會覆寫此設定。</span><span class="sxs-lookup"><span data-stu-id="c76ed-231">Any MasterPageFile attribute set in a @Page directive will also override this setting.</span></span> <span data-ttu-id="c76ed-232">使用&lt;頁&gt;項目會建立簡單<em>主要</em>可以覆寫視特定的資料夾或檔案中的主版頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-232">Using the &lt;pages&gt; element makes it simple to create a <em>master</em> master page that can be overridden if necessary in particular folders or files.</span></span>

## <a name="properties-in-master-pages"></a><span data-ttu-id="c76ed-233">主版頁面中的屬性</span><span class="sxs-lookup"><span data-stu-id="c76ed-233">Properties in Master Pages</span></span>

<span data-ttu-id="c76ed-234">主版頁面可以公開屬性，只要讓這些屬性主版頁面內公用。</span><span class="sxs-lookup"><span data-stu-id="c76ed-234">A master page can expose properties by simply making those properties public within the master page.</span></span> <span data-ttu-id="c76ed-235">比方說，下列程式碼會定義名為 SomeProperty 的屬性：</span><span class="sxs-lookup"><span data-stu-id="c76ed-235">For example, the following code defines a property called SomeProperty:</span></span>

[!code-csharp[Main](master-pages/samples/sample2.cs)]

<span data-ttu-id="c76ed-236">若要存取 SomeProperty 屬性從 [內容] 頁面，您必須使用主要屬性如下：</span><span class="sxs-lookup"><span data-stu-id="c76ed-236">To access the SomeProperty property from the Content page, you will need to use the Master property like so:</span></span>

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a><span data-ttu-id="c76ed-237">巢狀主版頁面</span><span class="sxs-lookup"><span data-stu-id="c76ed-237">Nesting Master Pages</span></span>

<span data-ttu-id="c76ed-238">主版頁面是完美的解決方案，以確保跨大型的 Web 應用程式的常見的外觀與風格。</span><span class="sxs-lookup"><span data-stu-id="c76ed-238">Master pages are the perfect solution for ensuring a common look and feel across a large Web application.</span></span> <span data-ttu-id="c76ed-239">不過，它不是常會有大型的站台共用的通用介面的某些部分，而其他部分共用不同的介面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-239">However, it's not uncommon to have certain parts of a large site share a common interface while other parts share a different interface.</span></span> <span data-ttu-id="c76ed-240">若要解決這項需求，多個主版頁面是完美的解決方案。</span><span class="sxs-lookup"><span data-stu-id="c76ed-240">To address that need, multiple master pages are the perfect solution.</span></span> <span data-ttu-id="c76ed-241">不過，仍然無法解決，大型的應用程式可能會有某些元件 （例如功能表，例如） 可共用的所有頁面和其他元件只在特定區段的站台之間共用的事實。</span><span class="sxs-lookup"><span data-stu-id="c76ed-241">However, that still doesn't address the fact that a large application may have certain components (such as a menu, for example) that are shared among all pages and other components that are shared only among certain sections of the site.</span></span> <span data-ttu-id="c76ed-242">這樣的情況中，巢狀主版頁面填入需要妥善。</span><span class="sxs-lookup"><span data-stu-id="c76ed-242">For that type of situation, nested master pages fill the need nicely.</span></span> <span data-ttu-id="c76ed-243">如您所見，一般的主版頁面包含主版頁面和內容頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-243">As you've seen, a normal master page consists of a master page and a content page.</span></span> <span data-ttu-id="c76ed-244">在巢狀主版頁面的情況下，有兩個主版頁面;父主要和下層主版。</span><span class="sxs-lookup"><span data-stu-id="c76ed-244">In a nested master page situation, there are two master pages; a parent master and a child master.</span></span> <span data-ttu-id="c76ed-245">子主版頁面也是內容頁面，其主要是父主版頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-245">The child master page is also a content page and its master is the parent master page.</span></span>

<span data-ttu-id="c76ed-246">以下是典型的主版頁面的程式碼：</span><span class="sxs-lookup"><span data-stu-id="c76ed-246">Here is the code for a typical master page:</span></span>

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

<span data-ttu-id="c76ed-247">在巢狀的主要案例中，這會是父 master。</span><span class="sxs-lookup"><span data-stu-id="c76ed-247">In a nested master scenario, this would be the parent master.</span></span> <span data-ttu-id="c76ed-248">另一個主版頁面會為其主版頁面中，使用此頁面，該程式碼會看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="c76ed-248">Another master page would use this page as its master page, and that code would look like this:</span></span>

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

<span data-ttu-id="c76ed-249">請注意，在此案例中，子主要也是內容的父主版頁面。</span><span class="sxs-lookup"><span data-stu-id="c76ed-249">Note that in this scenario, the child master is also a content page for the parent master.</span></span> <span data-ttu-id="c76ed-250">從父代的 ContentPlaceHolder 控制項取得其內容的內容控制項內的所有子主機的內容會出現。</span><span class="sxs-lookup"><span data-stu-id="c76ed-250">All of the child master's content appears inside of a Content control that gets its content from the parent's ContentPlaceHolder control.</span></span>

> [!NOTE]
> <span data-ttu-id="c76ed-251">找不到適用於巢狀主版頁面的設計工具的支援。</span><span class="sxs-lookup"><span data-stu-id="c76ed-251">Designer support is not available for nested master pages.</span></span> <span data-ttu-id="c76ed-252">當您開發使用巢狀的主機時，您必須使用來源檢視。</span><span class="sxs-lookup"><span data-stu-id="c76ed-252">When you are developing using nested masters, you will need to use source view.</span></span>


<span data-ttu-id="c76ed-253">這段影片示範使用巢狀主版頁面的逐步解說。</span><span class="sxs-lookup"><span data-stu-id="c76ed-253">This video shows a walkthrough of using nested master pages.</span></span>


![](master-pages/_static/image1.png)


[<span data-ttu-id="c76ed-254">開啟全螢幕影片</span><span class="sxs-lookup"><span data-stu-id="c76ed-254">Open Full-Screen Video</span></span>](master-pages/_static/nested1.wmv)


![選取主版頁面](master-pages/_static/image4.jpg)

<span data-ttu-id="c76ed-256">**圖 8**： 選取主版頁面</span><span class="sxs-lookup"><span data-stu-id="c76ed-256">**Figure 8**: Selecting a Master Page</span></span>
