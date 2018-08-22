---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
title: 巢狀主版頁面 (C#) |Microsoft Docs
author: rick-anderson
description: 示範如何建立巢狀在另一個主版頁面。
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 32b7fb6e-d74b-4048-91f8-70631b2523ee
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1b60b0b7ce4be66bc24ccbc1d25ce4dc56766815
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825134"
---
<a name="nested-master-pages-c"></a>巢狀主版頁面 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.zip)或[下載 PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.pdf)

> 示範如何建立巢狀在另一個主版頁面。


## <a name="introduction"></a>簡介

過去九個教學課程的課程中，我們已了解如何實作與主版頁面的整個網站的版面配置。 簡單的說，主版頁面讓我們，網頁開發人員，來定義主版頁面，以及可自訂內容 頁面的內容頁面為基礎的特定區域中的常見的標記。 在主版頁面的 ContentPlaceHolder 控制項表示可自訂的地區。透過內容控制項的 [內容] 頁面中定義的 ContentPlaceHolder 控制項的自訂的標記。

我們探討了到目前為止的主版頁面技術是很好，如果您有單一配置，在整個網站使用。 不過，許多大型的網站會有一種自訂的網站配置跨各個不同區段。 例如，請考慮醫院人員用來管理病患資訊、 活動和計費的醫療保健應用程式。 可能有三種類型的這個應用程式中的網頁：

- 工作人員可以在這裡更新可用性的人員成員專屬頁面檢視排程，或要求休假時間。
- 病患特定頁面的工作人員檢視或編輯某個特定的病患資訊的所在。
- 其中會計師檢閱目前的計費特定頁面宣告狀態和財務報告。

每個頁面可能會共用通用的配置，例如跨頂端和底部的常用連結一系列的功能表。 但員工、 病患和計費特定頁面，可能需要自訂這個泛用的版面配置。 例如，可能是所有人員專屬頁面應都包含的行事曆和工作清單會顯示目前登入使用者的可用性和每日排程。 可能是所有病患特定頁面都必須顯示名稱、 地址和正在編輯其資訊的病患的保險資訊。

您可利用來建立這類自訂的版面配置*巢狀主版頁面*。 若要實作上述案例中，我們會先建立主版頁面使用 ContentPlaceHolders 定義可自訂的區域定義全網站的版面配置、 功能表和頁尾內容。 然後，我們會建立三個巢狀主版頁面，其中每種類型的網頁。 每個巢狀主版頁面會定義之間的內容使用主版頁面的頁面類型的內容。 換句話說，巢狀主版頁面病患專屬內容頁面會包含標記和程式設計邏輯，來顯示病患正在編輯的相關資訊。 建立新的 patient 特定頁面時我們會將它繫結至這個巢狀主版頁面。

本教學課程一開始會反白顯示巢狀主版頁面的優點。 然後，它會示範如何建立和使用巢狀主版頁面。

> [!NOTE]
> 自.NET Framework 2.0 版已經可以巢狀主版頁面。 不過，Visual Studio 2005 未包含巢狀主版頁面的設計階段支援。 好消息是，Visual Studio 2008 提供豐富的設計階段經驗，用於巢狀主版頁面。 如果您想要使用巢狀主版頁面，但仍在使用 Visual Studio 2005，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/)的部落格項目[VS 2005 設計階段中的巢狀主版頁面的提示](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)。


## <a name="the-benefits-of-nested-master-pages"></a>巢狀主版頁面的優點

許多網站有為名的網站設計，以及更多自訂的設計的特定頁面的特定類型。 比方說，在我們的示範 web 應用程式中我們建立了基本的 [管理] 區段 (在頁面`~/Admin`資料夾)。 目前在網頁`~/Admin`資料夾不在 [管理] 區段中的這些頁面與使用相同的主版頁面 (亦即`Site.master`或`Alternate.master`，取決於使用者的選取範圍)。

> [!NOTE]
> 現在，假設我們的網站有一個主版頁面， `Site.master`。 我們要討論本教學課程稍後使用巢狀主版頁面，開頭為 「 使用巢狀主版頁面的系統管理區段 」 的兩個 （或多個） 主要的頁面。


想像一下，我們必須自訂以包含其他資訊或連結，否則將不會出現在網站中的其他頁面的 [管理] 頁面的配置。 有四個方法，來實作這項需求：

1. 手動將管理特定的資訊和連結新增至中的每個內容頁面`~/Admin`資料夾。
2. 更新`Site.master`主版頁面来包含的系統管理 區段特有的資訊和連結，然後將程式碼新增至主版頁面，即可顯示或隱藏這些區段會根據是否其中一個系統管理頁面所造訪。
3. 建立新的主版頁面專為系統管理 區段中，複製 從標記`Site.master`新增的系統管理 區段特有的資訊和連結，，然後更新中的內容頁面`~/Admin`来使用這個新的主要資料夾頁面。
4. 建立繫結至巢狀主版頁面`Site.master`且具有內容的頁面中`~/Admin`這個新的資料夾使用巢狀主版頁面。 此巢狀主版頁面會包含其他資訊和特定的管理頁面的連結，並不需要重複的標記中已定義`Site.master`。

第一個選項是最不可行。 使用主版頁面的重點是將移開不必手動複製並貼上常見的標記，以新的 ASP.NET 網頁。 第二個選項可以接受，但是可讓應用程式比較不容易維護，它變偶爾才會顯示，且需要開發人員編輯主版頁面暫時解決此標記，並且必須記得時，標記與主版頁面完全，特定的標記會顯示與隱藏時。 這個方法是網頁的較不 tenable 越來越多的類型，此單一的主版頁面所容納所需的自訂。

第三個選項會移除雜亂，與複雜性問題顯示第二個選項。 不過，選項 3 的主要缺點是，我們必須複製並貼上從常見的版面配置`Site.master`至新的管理特定區段的主版頁面。 如果我們稍後決定變更整個網站的版面配置我們必須請記得要變更的兩個地方。

第四個選項中，巢狀主版頁面，並提供我們最佳的第二個和第三個選項。 全網站的版面配置資訊保留一個檔案-的最上層的主版頁面-雖然特有特定區域的內容會分成不同的檔案。

本教學課程開始了解建立和使用簡單的巢狀主版頁面。 我們會建立全新的最上層主版頁面，兩個巢狀主版頁面，以及兩個內容頁面。 從 「 使用巢狀主版頁面的系統管理區段 」 開始，我們更新我們現有的主版頁面架構，以包括使用巢狀主版頁面。 具體來說，我們建立巢狀主版頁面，並使用要包含的內容頁面中的其他自訂內容`~/Admin`資料夾。

## <a name="step-1-creating-a-simple-top-level-master-page"></a>步驟 1： 建立簡單的最上層主版頁面

建立巢狀主版根據其中一個現有的主版頁面，然後更新現有的內容頁面上，若要使用這個新的巢狀主版頁面，而不是最上層的主版頁面需要一些複雜性，因為現有的內容頁面已預期特定ContentPlaceHolder 定義最上層的主版頁面中的控制項。 因此，巢狀主版頁面也必須包含具有相同名稱的相同 ContentPlaceHolder 控制項。 此外，我們的特定的示範應用程式有兩個主版頁面 (`Site.master`和`Alternate.master`)，以動態方式指派給內容頁面根據使用者的喜好設定，以進一步將新增到這種複雜性。 我們將探討更新現有的應用程式，稍後在本教學課程中使用巢狀主版頁面，但讓我們先著重於簡單的巢狀主版頁面範例。

建立新的資料夾，名為`NestedMasterPages`然後將新的主版頁面檔案新增至名為該資料夾`Simple.master`。 （請參閱 [圖 1 的 [方案總管] 的螢幕擷取畫面之後加入這個資料夾及檔案。）拖曳`AlternateStyles.css`從方案總管] 中，拖曳至設計工具的樣式表檔案。 這會新增`<link>`項目中的樣式表檔案`<head>`其後的項目，主版頁面的`<head>`項目的標記應該看起來像：


[!code-aspx[Main](nested-master-pages-cs/samples/sample1.aspx)]

接下來，新增下列標記中的 Web Form `Simple.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample2.aspx)]

此標記會海軍藍的背景上的大型白色字型中的頁面頂端顯示標題為 「 巢狀主版頁面 （簡單） 」 的連結。 下方的`MainContent`ContentPlaceHolder。 [圖 1] 顯示`Simple.master`主版頁面，在 Visual Studio 設計工具中載入時。


[![巢狀主版頁面的頁面，在 [管理] 區段中定義內容的特定](nested-master-pages-cs/_static/image2.png)](nested-master-pages-cs/_static/image1.png)

**圖 01**: 巢狀主版頁面定義內容的特定 [管理] 區段中的頁面 ([按一下以檢視完整大小的影像](nested-master-pages-cs/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>步驟 2： 建立簡單的巢狀主版頁面

`Simple.master` 包含兩個 ContentPlaceHolder 控制項：`MainContent`我們新增了連同 Web 表單內的 ContentPlaceHolder `head` ContentPlaceHolder 中的`<head>`項目。 如果我們建立一個內容頁面，並繫結至`Simple.master`[內容] 頁面會有兩個參考兩個 ContentPlaceHolders 的內容控制項。 同樣地，如果我們建立巢狀主版頁面，並將它繫結`Simple.master`則巢狀主版頁面將會有兩個內容控制項。

讓我們新增至新的巢狀主版頁面`NestedMasterPages`名為資料夾`SimpleNested.master`。 以滑鼠右鍵按一下`NestedMasterPages`資料夾，然後選擇 加入新項目。 這會顯示 [加入新項目] 對話方塊，[圖 2] 所示。 選取主版頁面範本類型，然後輸入新的主版頁面的名稱。 若要指出新的主版頁面應該是巢狀主版頁面，檢查 「 選取的主版頁面 」 的核取方塊。

接下來，按一下 [新增] 按鈕。 這會顯示相同的 Select 主版頁面 對話方塊，您會看到繫結至主版頁面的內容頁面時 （請參閱 圖 3）。 選擇`Simple.master`中的主版頁面`NestedMasterPages`資料夾並按一下 [確定]。

> [!NOTE]
> 如果您使用 Web 應用程式專案模型，而不網站專案模型的 ASP.NET 網站建立您不會看到 [圖 2] 所示的 [加入新項目] 對話方塊中的 [選取主版頁面] 核取方塊。 若要使用 Web 應用程式專案模型時，建立巢狀主版頁面中，您必須選擇巢狀主版頁面範本 （而不是主版頁面範本中）。 在 選取巢狀主版頁面範本，然後按一下 新增 之後, 相同選取主版頁面 圖 3 所示的對話方塊隨即出現。


[![請檢查&quot;選取的主版頁面&quot;核取方塊以新增巢狀主版頁面](nested-master-pages-cs/_static/image5.png)](nested-master-pages-cs/_static/image4.png)

**圖 02**： 選取 選取主版頁面 」 的核取方塊新增巢狀主版頁面 ([按一下以檢視完整大小的影像](nested-master-pages-cs/_static/image6.png))


[![繫結至 Simple.master 主版頁面的巢狀主版頁面](nested-master-pages-cs/_static/image8.png)](nested-master-pages-cs/_static/image7.png)

**圖 03**： 將巢狀主版頁面，來繫結`Simple.master`主版頁面 ([按一下以檢視完整大小的影像](nested-master-pages-cs/_static/image9.png))


巢狀主版頁面的宣告式標記，如下所示，包含參考最上層的主版頁面的兩個 ContentPlaceHolder 控制項的兩個內容控制項。


[!code-aspx[Main](nested-master-pages-cs/samples/sample3.aspx)]

除了`<%@ Master %>`指示詞時，巢狀主版頁面的初始宣告式標記等同於一開始會在繫結至相同的最上層主版頁面的內容頁面時所產生的標記。 喜歡的內容頁面`<%@ Page %>`指示詞`<%@ Master %>`此處的指示詞包含`MasterPageFile`屬性，指定巢狀主版頁面的父主版頁面。 巢狀主版頁面和內容頁面繫結至相同的最上層主版頁面的主要差異是巢狀主版頁面可以包含 ContentPlaceHolder 控制項。 巢狀主版頁面 ContentPlaceHolder 控制項定義內容頁面可以在其中自訂標記的區域。

更新此巢狀主版頁面，使其顯示文字"Hello，from SimpleNested ！ 」 對應至內容控制項中`MainContent`ContentPlaceHolder 控制項。


[!code-aspx[Main](nested-master-pages-cs/samples/sample4.aspx)]

後此新增，請在儲存巢狀主版頁面，然後再新增 新的內容頁面，以`NestedMasterPages`名為資料夾`Default.aspx`，並將它繫結`SimpleNested.master`主版頁面。 在此頁面可能會大吃一驚以查看它包含任何內容的控制項 （請參閱 圖 4） ！ 內容頁面只能存取其*父*主要頁面的 ContentPlaceHolders。 `SimpleNested.master` 不包含任何 ContentPlaceHolder 控制項;因此，任何繫結到此主版頁面的內容頁面不能包含任何內容的控制項。


[![新的內容頁面不包含內容控制項](nested-master-pages-cs/_static/image11.png)](nested-master-pages-cs/_static/image10.png)

**圖 04**: 新內容頁包含沒有內容的控制項 ([按一下以檢視完整大小的影像](nested-master-pages-cs/_static/image12.png))


我們要做為更新的巢狀主版頁面 (`SimpleNested.master`) 包括 ContentPlaceHolder 控制項。 通常您會想要針對每個 ContentPlaceHolder 定義它的父主版網頁，藉此讓其下層主版頁面或內容頁面，即可使用任何最上層的主版頁面的 ContentPlaceHolder 包含 ContentPlaceHolder 您巢狀主版頁面控制項。

更新`SimpleNested.master`ContentPlaceHolder 納入其兩個內容控制項的主版頁面。 提供 ContentPlaceHolder 控制項及其內容的控制項是指 ContentPlaceHolder 控制項相同的名稱。 也就是新增名為的 ContentPlaceHolder 控制項`MainContent`的內容，在中控制`SimpleNested.master`參考`MainContent`ContentPlaceHolder 中的`Simple.master`。 執行相同的動作中參考的內容控制項`head`ContentPlaceHolder。

> [!NOTE]
> 雖然我建議在最上層的主版頁面 ContentPlaceHolders 相同命名 ContentPlaceHolder 控制項巢狀主版頁面中的，則不需要此命名對稱性。 您可以在巢狀主版頁面提供 ContentPlaceHolder 控制項，任何您喜歡的名稱。 不過，我認為它容易記住 ContentPlaceHolders 與對應的頁面中，如果我的最上層的主版頁面和巢狀主版頁面使用相同的名稱有哪些區域。


進行這些新增項目後您`SimpleNested.master`主版頁面的宣告式標記看起來應該如下所示：


[!code-aspx[Main](nested-master-pages-cs/samples/sample5.aspx)]

刪除`Default.aspx`內容我們剛剛建立的頁面並再重新新增它，繫結至`SimpleNested.master`主版頁面。 這次 Visual Studio 新增兩個內容控制項加入`Default.aspx`，現在參考 ContentPlaceHolders 中定義`SimpleNested.master`（請參閱 圖 6）。 加入文字"Hello，from Default.aspx ！ 」 在內容控制項參考`MainContent`。

[圖 5] 顯示了這裡-牽涉到三個實體`Simple.master`， `SimpleNested.master`，和`Default.aspx`-以及如何關聯到另一個。 如圖所示，巢狀主版頁面實作其父代 ContentPlaceHolder 內容控制項。 如果這些區域需要能夠存取 [內容] 頁面中，巢狀主版頁面必須將自己 ContentPlaceHolders 新增至內容控制項。


[![最上層和巢狀主版頁面指定 [內容] 頁面的版面配置](nested-master-pages-cs/_static/image14.png)](nested-master-pages-cs/_static/image13.png)

**圖 05**： 最上層和巢狀主版頁面要求內容的頁面配置 ([按一下以檢視完整大小的影像](nested-master-pages-cs/_static/image15.png))


此行為說明何只 cognizant 其父主版頁面的內容頁面或主版頁面。 這項行為也會顯示由 Visual Studio 設計工具中。 [圖 6] 顯示的設計工具`Default.aspx`。 有哪些區域可從 [內容] 頁面可編輯，而且部分的不會清楚地顯示設計工具，它不會區分非可編輯的區域是從巢狀主版頁面和區域是從最上層的主版頁面。


[![內容頁面現在包含巢狀主版頁面的 ContentPlaceHolders 內容控制項](nested-master-pages-cs/_static/image17.png)](nested-master-pages-cs/_static/image16.png)

**圖 06**: 內容頁面現在包含內容控制項的巢狀主版頁面的 ContentPlaceHolders ([按一下以檢視完整大小的影像](nested-master-pages-cs/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>步驟 3： 加入第二個簡單巢狀主版頁面

當有多個巢狀主版頁面時，巢狀主版頁面的優點是更加明顯。 為了說明這項權益，建立另一個巢狀主版頁面中的`NestedMasterPages`資料夾，將這個新的巢狀主版頁面`SimpleNestedAlternate.master`並將它繫結`Simple.master`主版頁面。 像我們在步驟 2 中，請在巢狀主版頁面的兩個內容控制項加入 ContentPlaceHolder 控制項。 也加入文字"Hello，from SimpleNestedAlternate ！ 」 對應至最上層的主版頁面的內容控制項中`MainContent`ContentPlaceHolder。 進行這些變更之後新巢狀主版頁面的宣告式標記看起來應該如下所示：


[!code-aspx[Main](nested-master-pages-cs/samples/sample6.aspx)]

建立名為的內容頁面`Alternate.aspx`中`NestedMasterPages`資料夾，並繫結至`SimpleNestedAlternate.master`巢狀主版頁面。 加入文字"Hello，from 替代 ！ 」 對應至內容控制項中`MainContent`。 [圖 7] 顯示`Alternate.aspx`時透過 Visual Studio 設計工具檢視。


[![Alternate.aspx 繫結至 SimpleNestedAlternate.master 主版頁面](nested-master-pages-cs/_static/image20.png)](nested-master-pages-cs/_static/image19.png)

**圖 07**:`Alternate.aspx`繫結至`SimpleNestedAlternate.master`主版頁面 ([按一下以檢視完整大小的影像](nested-master-pages-cs/_static/image21.png))


比較圖 7，以 圖 6 中的設計工具中的設計工具。 這兩個內容頁都共用相同的最上層的主版頁面中定義的配置 (`Simple.master`)，也就是 「 巢狀主版頁面教學課程 （簡單） 」 標題。 尚未兩者都有不同的內容定義於其父主版頁面-文字"Hello，from SimpleNested ！ 」 在 圖 6 和"Hello，from SimpleNestedAlternate ！ 」 圖 7。 授與，這些差異極為瑣碎，但您可以擴充此範例包含更有意義的差異。 比方說，`SimpleNested.master`的網頁可能包含具有其內容頁，特定選項的功能表，而`SimpleNestedAlternate.master`可能會有繫結至它的內容頁面的相關資訊。

現在，想像一下，我們需要變更為名的網站配置。 例如，假設我們想要將一份常用的連結新增至所有內容頁面。 若要這麼做我們更新的最上層的主版頁面， `Simple.master`。 那里任何變更會立即反映其巢狀主版頁面中，並由延伸模組，其內容的頁面。

為了示範方便，我們可以變更為名的網站配置，開啟`Simple.master`主版頁面，並新增下列標記之間`topContent`並`mainContent``<div>`項目：


[!code-aspx[Main](nested-master-pages-cs/samples/sample7.aspx)]

這會將兩個連結，加入繫結至每個頁面頂端`Simple.master`， `SimpleNested.master`，或`SimpleNestedAlternate.master`; 這些變更立即套用至所有巢狀主版頁面和其內容的頁面。 [圖 8] 顯示`Alternate.aspx`透過瀏覽器檢視時。 請注意頁面頂端的 （相較於 圖 7） 上的連結新增。


[![變更為頂層主版頁面會立即反映在其巢狀主版頁面和其內容頁面](nested-master-pages-cs/_static/image23.png)](nested-master-pages-cs/_static/image22.png)

**圖 08**： 變更為頂層主版頁面會立即反映在其巢狀主版頁面和其內容頁面 ([按一下以檢視完整大小的影像](nested-master-pages-cs/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>使用巢狀主版頁面的 [管理] 區段

現在我們已看過的優點的巢狀主版頁面，並已了解如何建立和 ASP.NET 應用程式中使用它們。 不過，在步驟 1、 2 和 3，範例牽涉到建立新的最上層主版頁面、 新的巢狀主版頁面，以及新的內容頁面。 什麼加入新巢狀主版頁面的網站與現有的最上層主版頁面和內容頁面嗎？

將巢狀主版頁面整合到現有的網站，並將它與現有的內容頁面關聯需要多一點工夫，比從頭開始。 步驟 4、 5、 6 和 7 瀏覽這些挑戰，因為我們加強我們的示範應用程式，以包含新的巢狀主版頁面名稱為`AdminNested.master`，包含系統管理員的指示，並由 ASP.NET 網頁中`~/Admin`資料夾。

將巢狀主版頁面整合到我們的示範應用程式導入了下列障礙：

- 現有的內容中的分頁`~/Admin`資料夾有特定的期望，從其主版頁面。 首先，他們預期要有特定 ContentPlaceHolder 控制項。 此外，`~/Admin/AddProduct.aspx`並`~/Admin/Products.aspx`頁呼叫主版頁面的公用`RefreshRecentProductsGrid`方法，將其`GridMessageText`屬性，或具有事件處理常式其`PricesDoubled`事件。 因此，我們的巢狀主版頁面必須提供相同的 ContentPlaceHolders 和公用成員。
- 在先前的教學課程中我們已強化`BasePage`類別來動態設定`Page`物件的`MasterPageFile`屬性，根據工作階段變數。 我們如何以支援動態主版頁面使用巢狀主版頁面時？

當我們建置的巢狀主版頁面，並使用從現有內容頁面，會呈現這些兩個挑戰。 我們將調查，並把握克服這些問題。

## <a name="step-4-creating-the-nested-master-page"></a>步驟 4： 建立巢狀主版頁面

第一個工作是建立巢狀主版頁面，以供在 [管理] 區段中的頁面。 當加入新的巢狀主版頁面時，我們看到在步驟 2 中，我們需要指定巢狀主版頁面的父主版頁面。 但我們有兩個最上層的主版頁面：`Site.master`和`Alternate.master`。 回想一下，我們建立了`Alternate.master`前述教學課程中還會撰寫程式碼`BasePage`設定頁面物件的類別`MasterPageFile`屬性設為執行階段`Site.master`或`Alternate.master`的值而定`MyMasterPage`工作階段的變數。

我們如何設定我們的巢狀主版頁面使其使用適當的最上層主版頁面？ 我們有兩個選項：

- 建立兩個巢狀主版頁面`AdminNestedSite.master`並`AdminNestedAlternate.master`，並將它們繫結至最上層的主版頁面`Site.master`和`Alternate.master`分別。 在  `BasePage`，然後，我們會將設定`Page`物件的`MasterPageFile`適當的巢狀主版頁面。
- 建立單一巢狀主版頁面，並已使用這個特定的主版頁面的內容頁面。 然後，在執行階段，我們需要設定巢狀主版頁面`MasterPageFile`屬性，以在執行階段適用的最上層主頁面。 (您可能已經猜到目前為止，主版頁面也有`MasterPageFile`屬性。)

讓我們使用第二個選項。 建立單一巢狀主版頁面中的檔案`~/Admin`名為資料夾`AdminNested.master`。 因為這兩`Site.master`並`Alternate.master`有相同一組 ContentPlaceHolder 控制項，無論何種主版頁面您將它繫結，雖然我建議您將它繫結`Site.master`為了一致性的起見。


[![加入 ~/Admin 資料夾中的巢狀主版頁面。](nested-master-pages-cs/_static/image26.png)](nested-master-pages-cs/_static/image25.png)

**圖 09**： 加入巢狀主版頁面，以便`~/Admin`資料夾。 ([按一下以檢視完整大小的影像](nested-master-pages-cs/_static/image27.png))


巢狀主版頁面繫結至四個 ContentPlaceHolder 控制項與主版頁面，因為 Visual Studio 會將四個內容控制項加入新的巢狀主版頁面檔案的初始標記。 像我們在步驟 2 和 3，在每個內容控制項中，新增 ContentPlaceHolder 控制項提供最上層的主版頁面的 ContentPlaceHolder 控制項相同的名稱。 也將下列標記新增至內容控制項對應至`MainContent`ContentPlaceHolder:


[!code-html[Main](nested-master-pages-cs/samples/sample8.html)]

接下來，定義`instructions`CSS 類別`Styles.css`和`AlternateStyles.css`CSS 檔案。 下列的 CSS 規則導致 HTML 項目樣式`instructions`使用淡黃色背景色彩和黑色的實線框線要顯示的類別：


[!code-css[Main](nested-master-pages-cs/samples/sample9.css)]

此標記已加入至巢狀主版頁面，因為它只會出現在這些頁面使用巢狀主版頁面 （也就是，在 [管理] 區段中的網頁）。

之後您巢狀主版頁面這些新增項目，其宣告式標記看起來應該如下所示：


[!code-aspx[Main](nested-master-pages-cs/samples/sample10.aspx)]

請注意，每個內容控制項的 ContentPlaceHolder 控制以及 ContentPlaceHolder 控制項的`ID`屬性會指派最上層的主版頁面中對應的 ContentPlaceHolder 控制項相同的值。 此外，管理特定區段的標記會出現在`MainContent`ContentPlaceHolder。

[圖 10] 顯示`AdminNested.master`時透過 Visual Studio 設計工具檢視的巢狀主版頁面。 在頂端的黃色方塊中的指示，請參閱 <<c0> `MainContent` 內容控制項。


[![巢狀主版頁面擴充最上層的主版頁面，為包含系統管理員的指示。](nested-master-pages-cs/_static/image29.png)](nested-master-pages-cs/_static/image28.png)

**圖 10**： 巢狀主版頁面擴充最上層的主版頁面，為包含系統管理員的指示。 ([按一下以檢視完整大小的影像](nested-master-pages-cs/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>步驟 5： 更新現有的內容頁面，以使用新的巢狀主版頁面

每當我們將新的內容頁面新增至 [管理] 區段，我們需要將它繫結`AdminNested.master`我們剛剛建立的主版頁面。 但是呢現有內容頁面？ 目前，網站中的所有內容頁面衍生自`BasePage`類別，以程式設計方式設定在執行階段的 內容頁面的主版頁面。 這不是我們想要在 [管理] 區段中的內容頁面的行為。 相反地，我們想要一律使用這些內容頁面`AdminNested.master`頁面。 它會選擇在執行階段權限最上層的內容頁面的巢狀主版頁面的責任。

最佳的方式，以達到這預期行為是建立新的自訂的基底頁面類別，名為`AdminBasePage`延伸`BasePage`類別。 `AdminBasePage` 接著即可覆寫`SetMasterPageFile`並設定`Page`物件的`MasterPageFile`硬式編碼值"~ / Admin/AdminNested.master"。 如此一來，任何頁面衍生自`AdminBasePage`會使用`AdminNested.master`，而任何頁面衍生自`BasePage`會有其`MasterPageFile`屬性設定為動態"~ / Site.master"或"~ / Alternate.master"的值為基礎`MyMasterPage`工作階段變數。

藉由新增新的類別檔案，以啟動`App_Code`名為資料夾`AdminBasePage.cs`。 已`AdminBasePage`擴充`BasePage`然後覆寫`SetMasterPageFile`方法。 該方法中指派`MasterPageFile`值"~ / Admin/AdminNested.master"。 您的類別進行這些變更後檔案看起來應該如下所示：


[!code-csharp[Main](nested-master-pages-cs/samples/sample11.cs)]

我們現在需要將現有的內容頁面在系統管理 區段是衍生自`AdminBasePage`而不是`BasePage`。 移至每個內容頁面中的程式碼後置類別檔案`~/Admin`資料夾並進行這項變更。 例如，在`~/Admin/Default.aspx`變更程式碼後置類別宣告：


[!code-csharp[Main](nested-master-pages-cs/samples/sample12.cs)]

收件者:


[!code-csharp[Main](nested-master-pages-cs/samples/sample13.cs)]

圖 11 說明如何最上層的主版頁面 (`Site.master`或`Alternate.master`)，巢狀主版頁面 (`AdminNested.master`)，並管理一節的內容頁面之間的關聯性。


[![巢狀主版頁面的頁面，在 [管理] 區段中定義內容的特定](nested-master-pages-cs/_static/image32.png)](nested-master-pages-cs/_static/image31.png)

**圖 11**: 巢狀主版頁面定義內容的特定 [管理] 區段中的頁面 ([按一下以檢視完整大小的影像](nested-master-pages-cs/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>步驟 6： 鏡像主版頁面的公用方法和屬性

請記得，`~/Admin/AddProduct.aspx`並`~/Admin/Products.aspx`頁面以程式設計方式使用主版頁面：`~/Admin/AddProduct.aspx`呼叫的主版頁面的公用`RefreshRecentProductsGrid`方法，並將其`GridMessageText`屬性;`~/Admin/Products.aspx`具有事件處理常式`PricesDoubled`事件。 在先前的教學課程中，我們建立抽象`BaseMasterPage`定義這些公用成員的類別。

`~/Admin/AddProduct.aspx`並`~/Admin/Products.aspx`頁面會假設其主版頁面衍生自`BaseMasterPage`類別。 `AdminNested.master`  頁面上，不過，目前延伸`System.Web.UI.MasterPage`類別。 如此一來，瀏覽時`~/Admin/Products.aspx``InvalidCastException`會擲回訊息: 「 無法將型別的物件轉換 'ASP.admin\_adminnested\_主要' 為類型 'BaseMasterPage'。 」

若要修正這我們必須先`AdminNested.master`程式碼後置類別延伸`BaseMasterPage`。 更新從巢狀主版頁面的程式碼後置類別宣告：


[!code-csharp[Main](nested-master-pages-cs/samples/sample14.cs)]

收件者:


[!code-csharp[Main](nested-master-pages-cs/samples/sample15.cs)]

我們還沒結束呢。 因為`BaseMasterPage`類別是抽象的我們必須覆寫`abstract`成員`RefreshRecentProductsGrid`和`GridMessageText`。 最上層的主版頁面會使用這些成員，來更新其使用者介面。 (僅限實際上`Site.master`主版頁面會使用這些方法，雖然這兩個最上層的主版頁面實作這些方法，因為兩者都擴充`BaseMasterPage`。)

雖然我們必須實作這些成員在`AdminNested.master`，這些實作需要是直接呼叫中使用巢狀主版頁面的最上層的主版頁面的 相同的成員。 比方說，當 [管理] 區段中的內容頁面呼叫巢狀主版頁面時，才`RefreshRecentProductsGrid`方法，所有的巢狀主版頁面要進行的動作，接著，呼叫`Site.master`或是`Alternate.master`的`RefreshRecentProductsGrid`方法。

若要達到此目的，先新增下列`@MasterType`指示詞加入頂端`AdminNested.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample16.aspx)]

請記得，`@MasterType`指示詞強型別將屬性加入至名為的程式碼後置類別`Master`。 然後覆寫`RefreshRecentProductsGrid`並`GridMessageText`成員和只委派呼叫`Master`的對應方法：


[!code-csharp[Main](nested-master-pages-cs/samples/sample17.cs)]

使用此程式碼就緒之後，您應該能夠瀏覽，並使用 [管理] 區段中的內容的頁面。 [圖 12] 顯示`~/Admin/Products.aspx`頁面上透過瀏覽器檢視時。 如您所見，此頁面會包含管理指示 方塊中，因為它定義於巢狀主版頁面。


[![在 [管理] 區段中的內容頁面包含每個頁面頂端的指示](nested-master-pages-cs/_static/image35.png)](nested-master-pages-cs/_static/image34.png)

**圖 12**： 管理 > 一節包含指示中最上方的每個網頁上的內容頁面 ([按一下以檢視完整大小的影像](nested-master-pages-cs/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>步驟 7： 在執行階段使用適當的最上層主版頁面

在 [管理] 區段中的所有內容頁面完全正常運作時，它們全都使用相同的最上層主版頁面，並忽略在使用者選取的主版頁面`ChooseMasterPage.aspx`。 這是因為，巢狀主版頁面及其`MasterPageFile`屬性以靜態方式設定為`Site.master`在其`<%@ Master %>`指示詞。

若要使用的最上層的主版頁面，選取我們要設定的使用者`AdminNested.master`的`MasterPageFile`屬性中的值為`MyMasterPage`工作階段變數。 因為我們設定的內容分頁`MasterPageFile`中的屬性`BasePage`，您可能認為我們會將設定巢狀主版頁面`MasterPageFile`中的屬性`BaseMasterPage`或在`AdminNested.master`的程式碼後置類別。 這將無法運作，不過，因為我們需要重新設定`MasterPageFile`PreInit 階段結束時的屬性。 我們可以以程式設計方式利用網頁生命週期，從主版頁面的最早時間是 Init 階段 （其中的 PreInit 階段之後，就會發生）。

因此，我們需要設定巢狀主版頁面`MasterPageFile`頁面之內容的屬性。 唯一的內容頁面，使用`AdminNested.master`主版頁面衍生自`AdminBasePage`。 因此，我們也可以那里放置此邏輯。 步驟 5 中我們已覆寫`SetMasterPageFile`方法的方法，將`Page`物件的`MasterPageFile`屬性，以"~ / Admin/AdminNested.master"。 更新`SetMasterPageFile`也設定主版頁面的`MasterPageFile`結果儲存在工作階段的屬性：


[!code-csharp[Main](nested-master-pages-cs/samples/sample18.cs)]

`GetMasterPageFileFromSession`方法，我們將新增至`BasePage`上述教學課程中，傳回適當的主版頁面檔案路徑會根據工作階段變數值中的類別。

使用此項變更之後，使用者的主版頁面選取項目也延續至 [管理] 區段。 [圖 13] 顯示於 [圖 12]，但之後，使用者已變更其主版頁面選取項目，若要在相同頁面`Alternate.master`。


[![巢狀的管理頁面會使用最上層使用者所選取的主版頁面](nested-master-pages-cs/_static/image38.png)](nested-master-pages-cs/_static/image37.png)

**圖 13**： 巢狀的 [管理] 頁面可讓您使用最高層級主版頁面中選取的使用者 ([按一下以檢視完整大小的影像](nested-master-pages-cs/_static/image39.png))


## <a name="summary"></a>總結

太多的 like 如何內容頁面可以繫結至主版頁面中，就可以建立巢狀主版頁面，讓子主版頁面的繫結至父主版頁面。 子主版頁面可能為每個父代的 ContentPlaceHolders; 定義內容控制項它可以將自己 ContentPlaceHolder 控制項 （以及其他標記） 加入這些內容的控制項。 巢狀主版頁面是在大型的 web 應用程式中的所有頁面都共用為名的外觀及操作，但是卻站台的特定區段需要唯一的自訂項目相當實用。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [巢狀的 ASP.NET 主版頁面](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [巢狀主版頁面與 VS 2005 設計階段的秘訣](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [VS 2008 巢狀主版頁面支援](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP 本書籍，他是 4GuysFromRolla.com 的創辦人，一直從事 Microsoft Web 技術自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 Scott 要聯絡[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](specifying-the-master-page-programmatically-cs.md)
> [下一頁](creating-a-site-wide-layout-using-master-pages-vb.md)
