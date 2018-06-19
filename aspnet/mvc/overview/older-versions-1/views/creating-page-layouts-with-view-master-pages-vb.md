---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
title: 使用檢視主版頁面 (VB) 建立頁面配置 |Microsoft 文件
author: microsoft
description: 在本教學課程中，您會學習如何建立一般的頁面配置多個網頁應用程式中，藉由運用檢視主版頁面。 您可以使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: d34f90a1-6de3-482a-a326-f87fdcbaaaff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 5208cedd8d24a290a0227bdcbaa84ae6210cd969
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879617"
---
<a name="creating-page-layouts-with-view-master-pages-vb"></a><span data-ttu-id="320cc-104">使用檢視主版頁面 (VB) 建立頁面配置</span><span class="sxs-lookup"><span data-stu-id="320cc-104">Creating Page Layouts with View Master Pages (VB)</span></span>
====================
<span data-ttu-id="320cc-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="320cc-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="320cc-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="320cc-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> <span data-ttu-id="320cc-107">在本教學課程中，您會學習如何建立一般的頁面配置多個網頁應用程式中，藉由運用檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="320cc-107">In this tutorial, you learn how to create a common page layout for multiple pages in your application by taking advantage of view master pages.</span></span> <span data-ttu-id="320cc-108">您可以定義兩個資料行頁面配置和所有 web 應用程式中的頁面，使用兩欄版面配置，例如使用檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="320cc-108">You can use a view master page, for example, to define a two-column page layout and use the two-column layout for all of the pages in your web application.</span></span>


## <a name="creating-page-layouts-with-view-master-pages"></a><span data-ttu-id="320cc-109">使用檢視主版頁面建立頁面配置</span><span class="sxs-lookup"><span data-stu-id="320cc-109">Creating Page Layouts with View Master Pages</span></span>

<span data-ttu-id="320cc-110">在本教學課程中，您會學習如何建立一般的頁面配置多個網頁應用程式中，藉由運用檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="320cc-110">In this tutorial, you learn how to create a common page layout for multiple pages in your application by taking advantage of view master pages.</span></span> <span data-ttu-id="320cc-111">您可以定義兩個資料行頁面配置和所有 web 應用程式中的頁面，使用兩欄版面配置，例如使用檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="320cc-111">You can use a view master page, for example, to define a two-column page layout and use the two-column layout for all of the pages in your web application.</span></span>

<span data-ttu-id="320cc-112">您也可以利用檢視主版頁面在應用程式中的多個頁面之間共用通用的內容。</span><span class="sxs-lookup"><span data-stu-id="320cc-112">You also can take advantage of view master pages to share common content across multiple pages in your application.</span></span> <span data-ttu-id="320cc-113">例如，您可以在檢視主版頁面放置您的網站標誌、 瀏覽連結和橫幅廣告。</span><span class="sxs-lookup"><span data-stu-id="320cc-113">For example, you can place your website logo, navigation links, and banner advertisements in a view master page.</span></span> <span data-ttu-id="320cc-114">這樣一來，應用程式中的每一頁會顯示此內容會自動。</span><span class="sxs-lookup"><span data-stu-id="320cc-114">That way, every page in your application would display this content automatically.</span></span>

<span data-ttu-id="320cc-115">在本教學課程中，您會學習如何建立新的檢視主版頁面及建立新的檢視內容 頁面，並根據主版頁面。</span><span class="sxs-lookup"><span data-stu-id="320cc-115">In this tutorial, you learn how to create a new view master page and create a new view content page based on the master page.</span></span>

### <a name="creating-a-view-master-page"></a><span data-ttu-id="320cc-116">建立檢視主版頁面</span><span class="sxs-lookup"><span data-stu-id="320cc-116">Creating a View Master Page</span></span>

<span data-ttu-id="320cc-117">讓我們開始建立定義兩欄版面配置檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="320cc-117">Let's start by creating a view master page that defines a two-column layout.</span></span> <span data-ttu-id="320cc-118">您加入新的檢視主版頁面的 MVC 專案以滑鼠右鍵按一下 Views\Shared 資料夾中，選取功能表選項**新增、 新項目**，然後選取 MVC 檢視主版頁面範本 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="320cc-118">You add a new view master page to an MVC project by right-clicking the Views\Shared folder, selecting the menu option **Add, New Item**, and selecting the  MVC View Master Page template (see Figure 1).</span></span>


<span data-ttu-id="320cc-119">[![加入檢視主版頁面](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="320cc-119">[![Adding a view master page](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)</span></span>

<span data-ttu-id="320cc-120">**圖 01**： 加入檢視主版頁面 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="320cc-120">**Figure 01**: Adding a view master page ([Click to view full-size image](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))</span></span>


<span data-ttu-id="320cc-121">應用程式中，您可以建立多個檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="320cc-121">You can create more than one view master page in an application.</span></span> <span data-ttu-id="320cc-122">每個檢視主版頁面可以定義不同的頁面配置。</span><span class="sxs-lookup"><span data-stu-id="320cc-122">Each view master page can define a different page layout.</span></span> <span data-ttu-id="320cc-123">例如，您可以有兩個資料行配置特定頁面及其他三欄版面配置的頁面。</span><span class="sxs-lookup"><span data-stu-id="320cc-123">For example, you might want certain pages to have a two-column layout and other pages to have a three-column layout.</span></span>

<span data-ttu-id="320cc-124">檢視主版頁面看起來很像標準的 ASP.NET MVC 檢視。</span><span class="sxs-lookup"><span data-stu-id="320cc-124">A view master page looks very much like a standard ASP.NET MVC view.</span></span> <span data-ttu-id="320cc-125">不過，不同於標準模式中，檢視主版頁面包含一或多個`<asp:ContentPlaceHolder>`標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-125">However, unlike a normal view, a view master page contains one or more `<asp:ContentPlaceHolder>` tags.</span></span> <span data-ttu-id="320cc-126">`<contentplaceholder>`標記用於標示會覆寫個別的內容頁中的主版頁面的區域。</span><span class="sxs-lookup"><span data-stu-id="320cc-126">The `<contentplaceholder>` tags are used to mark the areas of the master page that can be overridden in an individual content page.</span></span>

<span data-ttu-id="320cc-127">例如，檢視主版頁面中列出的 1 會定義兩欄版面配置。</span><span class="sxs-lookup"><span data-stu-id="320cc-127">For example, the view master page in Listing 1 defines a two-column layout.</span></span> <span data-ttu-id="320cc-128">它包含兩個`<contentplaceholder>`標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-128">It contains two `<contentplaceholder>` tags.</span></span> <span data-ttu-id="320cc-129">一個`<ContentPlaceHolder>`每個資料行。</span><span class="sxs-lookup"><span data-stu-id="320cc-129">One `<ContentPlaceHolder>` for each column.</span></span>

<span data-ttu-id="320cc-130">**列出 1 – `Views\Shared\Site.master`**</span><span class="sxs-lookup"><span data-stu-id="320cc-130">**Listing 1 – `Views\Shared\Site.master`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

<span data-ttu-id="320cc-131">在程式碼範例 1 中的主版頁面包含兩個檢視的主體`<div>`對應到兩個資料行的標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-131">The body of the view master page in Listing 1 contains two `<div>` tags that correspond to the two columns.</span></span> <span data-ttu-id="320cc-132">階層式樣式表的資料行類別會套用至兩者`<div>`標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-132">The Cascading Style Sheet column class is applied to both `<div>` tags.</span></span> <span data-ttu-id="320cc-133">這個類別被定義在主版頁面的最上層宣告的樣式表中。</span><span class="sxs-lookup"><span data-stu-id="320cc-133">This class is defined in the style sheet declared at the top of the master page.</span></span> <span data-ttu-id="320cc-134">您可以預覽檢視主版頁面以切換至 [設計] 檢視中的轉譯方式。</span><span class="sxs-lookup"><span data-stu-id="320cc-134">You can preview how the view master page will be rendered by switching to Design view.</span></span> <span data-ttu-id="320cc-135">按一下左下方的原始碼程式碼編輯器的 [設計] 索引標籤 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="320cc-135">Click the Design tab at the bottom-left of the source code editor (see Figure 2).</span></span>


<span data-ttu-id="320cc-136">[![預覽設計工具中的主版頁面](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="320cc-136">[![Previewing a master page in the designer](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)</span></span>

<span data-ttu-id="320cc-137">**圖 02**： 預覽設計工具中的主版頁面 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="320cc-137">**Figure 02**: Previewing a master page in the designer ([Click to view full-size image](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))</span></span>


### <a name="creating-a-view-content-page"></a><span data-ttu-id="320cc-138">建立檢視的內容頁面</span><span class="sxs-lookup"><span data-stu-id="320cc-138">Creating a View Content Page</span></span>

<span data-ttu-id="320cc-139">建立檢視主版頁面之後，您可以建立一或多個檢視的檢視主版頁面為基礎的內容頁面。</span><span class="sxs-lookup"><span data-stu-id="320cc-139">After you create a view master page, you can create one or more view content pages based on the view master page.</span></span> <span data-ttu-id="320cc-140">例如，您可以建立主控制器的索引檢視內容頁面，以滑鼠右鍵按一下 Views\Home 資料夾中選取**新增]、 [新項目**、 選取**MVC 檢視內容頁面**範本中，輸入名稱 Index.aspx，然後按一下 [新增] 按鈕 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="320cc-140">For example, you can create an Index view content page for the Home controller by right-clicking the Views\Home folder, selecting **Add, New Item**, selecting the **MVC View Content Page** template, entering the name Index.aspx, and clicking the Add button (see Figure 3).</span></span>


<span data-ttu-id="320cc-141">[![加入檢視的內容頁](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="320cc-141">[![Adding a view content page](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)</span></span>

<span data-ttu-id="320cc-142">**圖 03**： 加入檢視的內容頁 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="320cc-142">**Figure 03**: Adding a view content page ([Click to view full-size image](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))</span></span>


<span data-ttu-id="320cc-143">按一下 [新增] 按鈕之後，新的對話方塊隨即出現，可讓您選取要與 [檢視內容] 頁面產生關聯的檢視主版頁面 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="320cc-143">After you click the Add button, a new dialog appears that enables you to select a view master page to associate with the view content page (see Figure 4).</span></span> <span data-ttu-id="320cc-144">您可以瀏覽至 Site.master 檢視主版頁面的上一節中建立。</span><span class="sxs-lookup"><span data-stu-id="320cc-144">You can navigate to the Site.master view master page that we created in the previous section.</span></span>


<span data-ttu-id="320cc-145">[![選取主版頁面](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="320cc-145">[![Selecting a master page](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)</span></span>

<span data-ttu-id="320cc-146">**圖 04**： 選取主版頁面 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="320cc-146">**Figure 04**: Selecting a master page ([Click to view full-size image](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))</span></span>


<span data-ttu-id="320cc-147">建立新的檢視內容頁面上，並根據 Site.master 主版頁面之後，您會取得列表 2 中的檔案。</span><span class="sxs-lookup"><span data-stu-id="320cc-147">After you create a new view content page based on the Site.master master page, you get the file in Listing 2.</span></span>

<span data-ttu-id="320cc-148">**列出 2 – `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="320cc-148">**Listing 2 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

<span data-ttu-id="320cc-149">請注意，這個檢視包含`<asp:Content>`對應至每個標記`<asp:ContentPlaceHolder>`檢視主版頁面中的標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-149">Notice that this view contains a `<asp:Content>` tag that corresponds to each of the `<asp:ContentPlaceHolder>` tags in the view master page.</span></span> <span data-ttu-id="320cc-150">每個`<asp:Content>`標記包含指向特定的 ContentPlaceHolderID 屬性`<asp:ContentPlaceHolder>`，它會覆寫。</span><span class="sxs-lookup"><span data-stu-id="320cc-150">Each `<asp:Content>` tag includes a ContentPlaceHolderID attribute that points to the particular `<asp:ContentPlaceHolder>` that it overrides.</span></span>

<span data-ttu-id="320cc-151">此外，請注意，內容檢視中的頁面清單 2 不包含任何正常的開頭和結尾 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-151">Notice, furthermore, that the content view page in Listing 2 does not contain any of the normal opening and closing HTML tags.</span></span> <span data-ttu-id="320cc-152">例如，它不包含在開頭和結尾`<html>`或`<head>`標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-152">For example, it does not contain the opening and closing `<html>` or `<head>` tags.</span></span> <span data-ttu-id="320cc-153">所有的一般的開頭和結尾標記都包含在檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="320cc-153">All of the normal opening and closing tags are contained in the view master page.</span></span>

<span data-ttu-id="320cc-154">任何您想要檢視內容頁面中顯示的內容必須置於`<asp:Content>`標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-154">Any content that you want to display in a view content page must be placed within a `<asp:Content>` tag.</span></span> <span data-ttu-id="320cc-155">如果您將任何 HTML 或這些標記之外的其他內容，您會收到錯誤當您嘗試檢視的頁面。</span><span class="sxs-lookup"><span data-stu-id="320cc-155">If you place any HTML or other content outside of these tags, then you will get an error when you attempt to view the page.</span></span>

<span data-ttu-id="320cc-156">您不需要覆寫每個`<asp:ContentPlaceHolder>`從內容檢視頁面中的主版頁面的標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-156">You don't need to override every `<asp:ContentPlaceHolder>` tag from a master page in a content view page.</span></span> <span data-ttu-id="320cc-157">您只需要覆寫`<asp:ContentPlaceHolder>`標記您想要以特定的內容取代標記時。</span><span class="sxs-lookup"><span data-stu-id="320cc-157">You only need to override a `<asp:ContentPlaceHolder>` tag when you want to replace the tag with particular content.</span></span>

<span data-ttu-id="320cc-158">例如，修改的索引檢視表中列出的 3 只包含兩個`<asp:Content>`標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-158">For example, the modified Index view in Listing 3 contains only two `<asp:Content>` tags.</span></span> <span data-ttu-id="320cc-159">每個`<asp:Content>`標記包含一些文字。</span><span class="sxs-lookup"><span data-stu-id="320cc-159">Each of the `<asp:Content>` tags includes some text.</span></span>

<span data-ttu-id="320cc-160">**列出 3 – `Views\Home\Index.aspx (modified)`**</span><span class="sxs-lookup"><span data-stu-id="320cc-160">**Listing 3 – `Views\Home\Index.aspx (modified)`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

<span data-ttu-id="320cc-161">要求中列出的 3 的檢視時，它會呈現在 「 圖 5 頁。</span><span class="sxs-lookup"><span data-stu-id="320cc-161">When the view in Listing 3 is requested, it renders the page in Figure 5.</span></span> <span data-ttu-id="320cc-162">請注意檢視會呈現兩個資料行與網頁。</span><span class="sxs-lookup"><span data-stu-id="320cc-162">Notice that the view renders a page with two columns.</span></span> <span data-ttu-id="320cc-163">此外，請注意，從 [檢視內容] 頁面的內容會合併與檢視主版頁面內容。</span><span class="sxs-lookup"><span data-stu-id="320cc-163">Notice, furthermore, that the content from the view content page is merged with the content from the view master page.</span></span>


<span data-ttu-id="320cc-164">[![索引檢視的 [內容] 頁面](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="320cc-164">[![The Index view content page](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)</span></span>

<span data-ttu-id="320cc-165">**圖 05**: 索引檢視的內容頁面 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="320cc-165">**Figure 05**: The Index view content page ([Click to view full-size image](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))</span></span>


### <a name="modifying-view-master-page-content"></a><span data-ttu-id="320cc-166">修改檢視主版頁面內容</span><span class="sxs-lookup"><span data-stu-id="320cc-166">Modifying View Master Page Content</span></span>

<span data-ttu-id="320cc-167">幾乎會遇到的問題之一立即使用檢視主版頁面時，要求不同的檢視內容的頁面時，請修改檢視主版頁面內容的問題。</span><span class="sxs-lookup"><span data-stu-id="320cc-167">One issue that you encounter almost immediately when working with view master pages is the problem of modifying view master page content when different view content pages are requested.</span></span> <span data-ttu-id="320cc-168">例如，您會想每一頁 web 應用程式具有唯一的標題中。</span><span class="sxs-lookup"><span data-stu-id="320cc-168">For example, you want each page in your web application to have a unique title.</span></span> <span data-ttu-id="320cc-169">不過，標題會宣告檢視主版頁面中，而不是在 [檢視內容] 頁面。</span><span class="sxs-lookup"><span data-stu-id="320cc-169">However, the title is declared in the view master page and not in the view content page.</span></span> <span data-ttu-id="320cc-170">因此，您該如何自訂每個檢視的內容頁面的頁面標題？</span><span class="sxs-lookup"><span data-stu-id="320cc-170">So, how do you customize the page title for each view content page?</span></span>

<span data-ttu-id="320cc-171">有兩種方式，您可以修改檢視 內容頁所顯示的標題。</span><span class="sxs-lookup"><span data-stu-id="320cc-171">There are two ways that you can modify the title displayed by a view content page.</span></span> <span data-ttu-id="320cc-172">首先，您可以將頁面標題的標題屬性指派`<%@ page %>`指示詞在頁面上方的檢視內容中宣告。</span><span class="sxs-lookup"><span data-stu-id="320cc-172">First, you can assign a page title to the title attribute of the `<%@ page %>` directive declared at the top of a view content page.</span></span> <span data-ttu-id="320cc-173">例如，如果您想要指派至索引檢視的頁面標題 「 超級絕佳的網站 」，您可以包含下列指示詞，在索引檢視的頂端：</span><span class="sxs-lookup"><span data-stu-id="320cc-173">For example, if you want to assign the page title "Super Great Website" to the Index view, then you can include the following directive at the top of the Index view:</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

<span data-ttu-id="320cc-174">當瀏覽器中呈現索引檢視時，所需的標題會出現在瀏覽器標題列中：</span><span class="sxs-lookup"><span data-stu-id="320cc-174">When the Index view is rendered to the browser, the desired title appears in the browser title bar:</span></span>


<span data-ttu-id="320cc-175">[![瀏覽器標題列](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="320cc-175">[![Browser title bar](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)</span></span>


<span data-ttu-id="320cc-176">沒有主版檢視頁面必須符合 title 屬性，工作順序中的一個重要需求。</span><span class="sxs-lookup"><span data-stu-id="320cc-176">There is one important requirement that a master view page must satisfy in order for the title attribute to work.</span></span> <span data-ttu-id="320cc-177">檢視主版頁面必須包含`<head runat="server">`標記，而不是一般`<head>`標頭標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-177">The view master page must contain a `<head runat="server">` tag instead of a normal `<head>` tag for its header.</span></span> <span data-ttu-id="320cc-178">如果`<head>`標記不包含 runat ="server"屬性，則不會顯示標題。</span><span class="sxs-lookup"><span data-stu-id="320cc-178">If the `<head>` tag does not include the runat="server" attribute then the title won't appear.</span></span> <span data-ttu-id="320cc-179">主版頁面包含所需的預設檢視`<head runat="server">`標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-179">The default view master page includes the required `<head runat="server">` tag.</span></span>

<span data-ttu-id="320cc-180">從個別的檢視內容頁面上修改主版頁面內容的替代方法是將您想要修改的區域`<asp:ContentPlaceHolder>`標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-180">An alternative approach to modifying master page content from an individual view content page is to wrap the region that you want to modify in a `<asp:ContentPlaceHolder>` tag.</span></span> <span data-ttu-id="320cc-181">例如，假設您想要變更標題，不僅 meta 標記，由主版檢視頁面呈現。</span><span class="sxs-lookup"><span data-stu-id="320cc-181">For example, imagine that you want to change not only the title, but also the meta tags, rendered by a master view page.</span></span> <span data-ttu-id="320cc-182">主版檢視頁面中列出的 4 包含`<asp:ContentPlaceHolder>`標記內其`<head>`標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-182">The master view page in Listing 4 contains a `<asp:ContentPlaceHolder>` tag within its `<head>` tag.</span></span>

<span data-ttu-id="320cc-183">**列出 4 – `Views\Shared\Site2.master`**</span><span class="sxs-lookup"><span data-stu-id="320cc-183">**Listing 4 – `Views\Shared\Site2.master`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

<span data-ttu-id="320cc-184">請注意，`<asp:ContentPlaceHolder>`中列出的 4 標記包含預設內容： 一個預設標題和預設 meta 標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-184">Notice that the `<asp:ContentPlaceHolder>` tag in Listing 4 includes default content: a default title and default meta tags.</span></span> <span data-ttu-id="320cc-185">如果您不要覆寫此`<asp:ContentPlaceHolder>`標記中個別的檢視內容 頁面上，則會顯示預設內容。</span><span class="sxs-lookup"><span data-stu-id="320cc-185">If you don't override this `<asp:ContentPlaceHolder>` tag in an individual view content page, then the default content will be displayed.</span></span>

<span data-ttu-id="320cc-186">內容檢視中的頁面列出 5 會覆寫`<asp:ContentPlaceHolder>`才能顯示自訂標題和自訂 meta 標記的標記。</span><span class="sxs-lookup"><span data-stu-id="320cc-186">The content view page in Listing 5 overrides the `<asp:ContentPlaceHolder>` tag in order to display a custom title and custom meta tags.</span></span>

<span data-ttu-id="320cc-187">**列出 5 – `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="320cc-187">**Listing 5 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a><span data-ttu-id="320cc-188">總結</span><span class="sxs-lookup"><span data-stu-id="320cc-188">Summary</span></span>

<span data-ttu-id="320cc-189">本教學課程為您提供的基本介紹，檢視主版頁面及內容頁面。</span><span class="sxs-lookup"><span data-stu-id="320cc-189">This tutorial provided you with a basic introduction to view master pages and view content pages.</span></span> <span data-ttu-id="320cc-190">您已學習如何建立新的檢視主版頁面，並建立其為基礎的檢視內容頁面。</span><span class="sxs-lookup"><span data-stu-id="320cc-190">You learned how to create new view master pages and create view content pages based on them.</span></span> <span data-ttu-id="320cc-191">我們也會檢查您如何修改檢視主版頁面的特定檢視的內容頁面的內容。</span><span class="sxs-lookup"><span data-stu-id="320cc-191">We also examined how you can modify the content of a view master page from a particular view content page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="320cc-192">[上一頁](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
> [下一頁](passing-data-to-view-master-pages-vb.md)</span><span class="sxs-lookup"><span data-stu-id="320cc-192">[Previous](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
[Next](passing-data-to-view-master-pages-vb.md)</span></span>
