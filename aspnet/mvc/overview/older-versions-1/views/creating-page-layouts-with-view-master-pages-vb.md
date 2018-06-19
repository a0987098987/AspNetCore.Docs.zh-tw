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
<a name="creating-page-layouts-with-view-master-pages-vb"></a>使用檢視主版頁面 (VB) 建立頁面配置
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> 在本教學課程中，您會學習如何建立一般的頁面配置多個網頁應用程式中，藉由運用檢視主版頁面。 您可以定義兩個資料行頁面配置和所有 web 應用程式中的頁面，使用兩欄版面配置，例如使用檢視主版頁面。


## <a name="creating-page-layouts-with-view-master-pages"></a>使用檢視主版頁面建立頁面配置

在本教學課程中，您會學習如何建立一般的頁面配置多個網頁應用程式中，藉由運用檢視主版頁面。 您可以定義兩個資料行頁面配置和所有 web 應用程式中的頁面，使用兩欄版面配置，例如使用檢視主版頁面。

您也可以利用檢視主版頁面在應用程式中的多個頁面之間共用通用的內容。 例如，您可以在檢視主版頁面放置您的網站標誌、 瀏覽連結和橫幅廣告。 這樣一來，應用程式中的每一頁會顯示此內容會自動。

在本教學課程中，您會學習如何建立新的檢視主版頁面及建立新的檢視內容 頁面，並根據主版頁面。

### <a name="creating-a-view-master-page"></a>建立檢視主版頁面

讓我們開始建立定義兩欄版面配置檢視主版頁面。 您加入新的檢視主版頁面的 MVC 專案以滑鼠右鍵按一下 Views\Shared 資料夾中，選取功能表選項**新增、 新項目**，然後選取 MVC 檢視主版頁面範本 （請參閱圖 1）。


[![加入檢視主版頁面](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)

**圖 01**： 加入檢視主版頁面 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))


應用程式中，您可以建立多個檢視主版頁面。 每個檢視主版頁面可以定義不同的頁面配置。 例如，您可以有兩個資料行配置特定頁面及其他三欄版面配置的頁面。

檢視主版頁面看起來很像標準的 ASP.NET MVC 檢視。 不過，不同於標準模式中，檢視主版頁面包含一或多個`<asp:ContentPlaceHolder>`標記。 `<contentplaceholder>`標記用於標示會覆寫個別的內容頁中的主版頁面的區域。

例如，檢視主版頁面中列出的 1 會定義兩欄版面配置。 它包含兩個`<contentplaceholder>`標記。 一個`<ContentPlaceHolder>`每個資料行。

**列出 1 – `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

在程式碼範例 1 中的主版頁面包含兩個檢視的主體`<div>`對應到兩個資料行的標記。 階層式樣式表的資料行類別會套用至兩者`<div>`標記。 這個類別被定義在主版頁面的最上層宣告的樣式表中。 您可以預覽檢視主版頁面以切換至 [設計] 檢視中的轉譯方式。 按一下左下方的原始碼程式碼編輯器的 [設計] 索引標籤 （請參閱圖 2）。


[![預覽設計工具中的主版頁面](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)

**圖 02**： 預覽設計工具中的主版頁面 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))


### <a name="creating-a-view-content-page"></a>建立檢視的內容頁面

建立檢視主版頁面之後，您可以建立一或多個檢視的檢視主版頁面為基礎的內容頁面。 例如，您可以建立主控制器的索引檢視內容頁面，以滑鼠右鍵按一下 Views\Home 資料夾中選取**新增]、 [新項目**、 選取**MVC 檢視內容頁面**範本中，輸入名稱 Index.aspx，然後按一下 [新增] 按鈕 （請參閱圖 3）。


[![加入檢視的內容頁](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)

**圖 03**： 加入檢視的內容頁 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))


按一下 [新增] 按鈕之後，新的對話方塊隨即出現，可讓您選取要與 [檢視內容] 頁面產生關聯的檢視主版頁面 （請參閱圖 4）。 您可以瀏覽至 Site.master 檢視主版頁面的上一節中建立。


[![選取主版頁面](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)

**圖 04**： 選取主版頁面 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))


建立新的檢視內容頁面上，並根據 Site.master 主版頁面之後，您會取得列表 2 中的檔案。

**列出 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

請注意，這個檢視包含`<asp:Content>`對應至每個標記`<asp:ContentPlaceHolder>`檢視主版頁面中的標記。 每個`<asp:Content>`標記包含指向特定的 ContentPlaceHolderID 屬性`<asp:ContentPlaceHolder>`，它會覆寫。

此外，請注意，內容檢視中的頁面清單 2 不包含任何正常的開頭和結尾 HTML 標記。 例如，它不包含在開頭和結尾`<html>`或`<head>`標記。 所有的一般的開頭和結尾標記都包含在檢視主版頁面。

任何您想要檢視內容頁面中顯示的內容必須置於`<asp:Content>`標記。 如果您將任何 HTML 或這些標記之外的其他內容，您會收到錯誤當您嘗試檢視的頁面。

您不需要覆寫每個`<asp:ContentPlaceHolder>`從內容檢視頁面中的主版頁面的標記。 您只需要覆寫`<asp:ContentPlaceHolder>`標記您想要以特定的內容取代標記時。

例如，修改的索引檢視表中列出的 3 只包含兩個`<asp:Content>`標記。 每個`<asp:Content>`標記包含一些文字。

**列出 3 – `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

要求中列出的 3 的檢視時，它會呈現在 「 圖 5 頁。 請注意檢視會呈現兩個資料行與網頁。 此外，請注意，從 [檢視內容] 頁面的內容會合併與檢視主版頁面內容。


[![索引檢視的 [內容] 頁面](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)

**圖 05**: 索引檢視的內容頁面 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>修改檢視主版頁面內容

幾乎會遇到的問題之一立即使用檢視主版頁面時，要求不同的檢視內容的頁面時，請修改檢視主版頁面內容的問題。 例如，您會想每一頁 web 應用程式具有唯一的標題中。 不過，標題會宣告檢視主版頁面中，而不是在 [檢視內容] 頁面。 因此，您該如何自訂每個檢視的內容頁面的頁面標題？

有兩種方式，您可以修改檢視 內容頁所顯示的標題。 首先，您可以將頁面標題的標題屬性指派`<%@ page %>`指示詞在頁面上方的檢視內容中宣告。 例如，如果您想要指派至索引檢視的頁面標題 「 超級絕佳的網站 」，您可以包含下列指示詞，在索引檢視的頂端：

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

當瀏覽器中呈現索引檢視時，所需的標題會出現在瀏覽器標題列中：


[![瀏覽器標題列](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)


沒有主版檢視頁面必須符合 title 屬性，工作順序中的一個重要需求。 檢視主版頁面必須包含`<head runat="server">`標記，而不是一般`<head>`標頭標記。 如果`<head>`標記不包含 runat ="server"屬性，則不會顯示標題。 主版頁面包含所需的預設檢視`<head runat="server">`標記。

從個別的檢視內容頁面上修改主版頁面內容的替代方法是將您想要修改的區域`<asp:ContentPlaceHolder>`標記。 例如，假設您想要變更標題，不僅 meta 標記，由主版檢視頁面呈現。 主版檢視頁面中列出的 4 包含`<asp:ContentPlaceHolder>`標記內其`<head>`標記。

**列出 4 – `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

請注意，`<asp:ContentPlaceHolder>`中列出的 4 標記包含預設內容： 一個預設標題和預設 meta 標記。 如果您不要覆寫此`<asp:ContentPlaceHolder>`標記中個別的檢視內容 頁面上，則會顯示預設內容。

內容檢視中的頁面列出 5 會覆寫`<asp:ContentPlaceHolder>`才能顯示自訂標題和自訂 meta 標記的標記。

**列出 5 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a>總結

本教學課程為您提供的基本介紹，檢視主版頁面及內容頁面。 您已學習如何建立新的檢視主版頁面，並建立其為基礎的檢視內容頁面。 我們也會檢查您如何修改檢視主版頁面的特定檢視的內容頁面的內容。

> [!div class="step-by-step"]
> [上一頁](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
> [下一頁](passing-data-to-view-master-pages-vb.md)
