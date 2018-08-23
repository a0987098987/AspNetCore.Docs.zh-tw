---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
title: 使用檢視主版頁面 (VB) 建立頁面配置 |Microsoft Docs
author: microsoft
description: 在本教學課程中，您已了解如何在您的應用程式中建立多個頁面的一般版面，利用檢視主版頁面。 您可以使用...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: d34f90a1-6de3-482a-a326-f87fdcbaaaff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 4ff74b1dc1d83b7ec1c8ecf6eca341a5cd14403f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834625"
---
<a name="creating-page-layouts-with-view-master-pages-vb"></a>使用檢視主版頁面 (VB) 建立頁面配置
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> 在本教學課程中，您已了解如何在您的應用程式中建立多個頁面的一般版面，利用檢視主版頁面。 您可以定義兩個資料行頁面配置，並使用兩欄版面配置的所有 web 應用程式頁面，比方說，使用檢視主版頁面。


## <a name="creating-page-layouts-with-view-master-pages"></a>使用檢視主版頁面建立頁面配置

在本教學課程中，您已了解如何在您的應用程式中建立多個頁面的一般版面，利用檢視主版頁面。 您可以定義兩個資料行頁面配置，並使用兩欄版面配置的所有 web 應用程式頁面，比方說，使用檢視主版頁面。

您也可以利用檢視主版頁面可以跨應用程式中的多個網頁共用通用的內容。 比方說，您可以檢視主版頁面中放置您的網站標誌、 瀏覽連結和橫幅廣告。 如此一來，在您的應用程式中的每一頁會顯示此內容會自動。

在本教學課程中，您將了解如何建立新的檢視主版頁面，並建立新的檢視內容 頁面，並根據主版頁面。

### <a name="creating-a-view-master-page"></a>建立檢視主版頁面

現在就開始建立會定義兩欄版面配置檢視主版頁面。 加入新的檢視主版頁面 MVC 專案中以滑鼠右鍵按一下 Views\Shared 資料夾中，選取功能表選項**新增、 新項目**，然後選取 MVC 檢視主版頁面範本 （見 圖 1）。


[![新增檢視主版頁面](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)

**圖 01**： 新增檢視主版頁面 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))


您可以建立多個檢視主版頁面的應用程式中。 每個檢視主版頁面可以定義不同的網頁版面配置。 比方說，您可能想要特定的網頁有兩欄版面配置和其他頁面有三欄版面配置。

檢視主版頁面看起來很像標準的 ASP.NET MVC 檢視。 不過，不同於標準模式中，檢視主版頁面包含一或多個`<asp:ContentPlaceHolder>`標記。 `<contentplaceholder>`標記用於標示可覆寫個別的內容頁面中的主版頁面的區域。

例如，檢視主版頁面，在 列表 1 中的定義兩欄版面配置。 它包含兩個`<contentplaceholder>`標記。 一個`<ContentPlaceHolder>`每個資料行。

**列表 1 – `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

在 列表 1 中的主版頁面包含兩個檢視的主體`<div>`對應到兩個資料行的標記。 階層式樣式表的資料行類別會套用至兩者`<div>`標記。 這個類別被定義在主版頁面的最上層宣告的樣式表中。 您可以在預覽檢視主版頁面藉由切換至 [設計] 檢視中的呈現方式。 按一下左下角的原始碼程式碼編輯器的 設計 索引標籤 （請參閱 圖 2）。


[![預覽設計工具中的主版頁面](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)

**圖 02**： 預覽設計工具中的主版頁面 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))


### <a name="creating-a-view-content-page"></a>建立檢視的內容頁面

建立檢視主版頁面之後，您可以建立一或多個檢視以檢視主版頁面的內容頁面。 比方說，您可以建立主控制器的索引檢視內容頁面，以滑鼠右鍵按一下 Views\Home 資料夾中，選取**新增]、 [新項目**，並選取**MVC 檢視內容頁面**範本中，輸入名稱 Index.aspx，，然後按一下 [新增] 按鈕 （請參閱 [圖 3]）。


[![新增檢視內容頁面](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)

**圖 03**： 新增檢視內容頁面 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))


按一下 新增 按鈕之後，新的對話方塊隨即出現，可讓您選取 檢視 內容頁面相關聯的檢視主版頁面 （請參閱 圖 4）。 您可以瀏覽至 Site.master 檢視主版頁面，我們在上一節中建立。


[![選取主版頁面](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)

**圖 04**： 選取主版頁面 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))


建立新的檢視內容頁面上，並根據 Site.master 主版頁面之後，您會取得列表 2 中的檔案。

**列表 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

請注意，這份檢視含有`<asp:Content>`對應至每個標記`<asp:ContentPlaceHolder>`檢視主版頁面中的標記。 每個`<asp:Content>`標記會包括指向特定 atribut ContentPlaceHolderID `<asp:ContentPlaceHolder>` ，它會覆寫。

此外，請注意，在列表 2 中的 [內容檢視] 頁面不包含任何一般的開頭和結尾 HTML 標記。 比方說，它不包含的開頭和結尾`<html>`或`<head>`標記。 所有一般的開頭和結尾標記會包含在檢視主版頁面。

任何您想要檢視內容頁面中顯示的內容必須置於`<asp:Content>`標記。 如果您將任何 HTML 或其他內容，這些標記之外，您會收到錯誤當您嘗試檢視該頁面。

您不需要覆寫每個`<asp:ContentPlaceHolder>`內容檢視頁面中的主版頁面的標記。 您只需要覆寫`<asp:ContentPlaceHolder>`標記時您想要以特定的內容取代標記。

例如，在 列表 3 中修改過的 索引 檢視只包含兩個`<asp:Content>`標記。 每個`<asp:Content>`標記包含一些文字。

**列表 3 – `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

要求列表 3 中的檢視時，它會呈現 [圖 5] 頁面。 請注意，檢視會呈現兩個資料行的頁面。 此外，請注意，從 [檢視內容] 頁面的內容會合併檢視主版頁面的內容。


[![索引檢視的 [內容] 頁面](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)

**圖 05**: 索引檢視的內容頁面 ([按一下以檢視完整大小的影像](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>修改檢視主版頁面內容

一個問題，您會遇到幾乎立即使用檢視主版頁面時，要求不同的檢視內容頁面時，請修改檢視主版頁面內容的問題。 比方說，您會想每個頁面中您的 web 應用程式，將唯一的標題。 不過，標題會宣告檢視主版頁面中，而不是在 [檢視內容] 頁面。 因此，您該如何自訂每個檢視的內容頁面的頁面標題？

有兩種方式，您可以修改的檢視內容頁面所顯示的標題。 首先，您可以將頁面標題的標題屬性指派`<%@ page %>`指示詞宣告頂端的 檢視內容頁面。 例如，如果您想要將 [索引] 檢視中的頁面標題 「 超級絕佳的網站 」，您可以包含下列指示詞，在 [索引] 檢視的頂端：

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

當 [索引] 檢視呈現至瀏覽器時，所需的標題會出現在瀏覽器標題列中：


[![瀏覽器標題列](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)


沒有主版檢視頁面必須滿足才能 title 屬性的順序的一項重要需求。 檢視主版頁面必須包含`<head runat="server">`標記，而不是一般`<head>`標頭標記。 如果`<head>`標記不包含 runat ="server"屬性，則不會出現標題。 主版頁面包含所需的預設檢視`<head runat="server">`標記。

從個別的檢視內容 頁面中的公式修改主版頁面內容的替代方法是將包裝您想要修改的區域`<asp:ContentPlaceHolder>`標記。 例如，假設您想要變更標題，不僅 meta 標記，主版檢視頁面所呈現。 列表 4 中的主版檢視頁面包含`<asp:ContentPlaceHolder>`標記內其`<head>`標記。

**列表 4 – `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

請注意，`<asp:ContentPlaceHolder>`列表 4 中的標籤包含預設內容： 預設標題和預設的中繼標記。 如果您不覆寫此`<asp:ContentPlaceHolder>`標記在個別的檢視內容 頁面中，則會顯示預設內容。

表 5 中的 內容檢視 頁面會覆寫`<asp:ContentPlaceHolder>`才能顯示一個自訂的標題和自訂 meta 標記的標記。

**列表 5 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a>總結

本教學課程會提供您使用檢視主版頁面和檢視內容頁面的基本簡介。 您已了解如何建立新的檢視主版頁面，並建立其為基礎的檢視內容頁面。 我們也會檢查您如何修改檢視主版頁面，從特定的檢視內容頁面的內容。

> [!div class="step-by-step"]
> [上一頁](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
> [下一頁](passing-data-to-view-master-pages-vb.md)
