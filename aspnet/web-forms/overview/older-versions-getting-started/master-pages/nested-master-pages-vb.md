---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: "巢狀主版頁面 (VB) |Microsoft 文件"
author: rick-anderson
description: "示範如何巢狀另一個主版頁面。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 41a9fd6b752252d563a0f15a420262cbb31c19ff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="nested-master-pages-vb"></a>巢狀主版頁面 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip)或[下載 PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> 示範如何巢狀另一個主版頁面。


## <a name="introduction"></a>簡介

過去 9 個教學課程的過程中，我們已經看到如何實作與主版頁面的全站台的配置。 簡而言之，主版頁面讓我們、 網頁開發人員，以及您可以自訂內容頁面的內容頁面為基礎的特定區域的主版頁面中定義通用的標記。 在主版頁面的 ContentPlaceHolder 控制項表示可自訂的地區。透過內容控制項的內容頁面中定義自訂的 ContentPlaceHolder 控制項的標記。

到目前為止，我們已探索的主版頁面技術是很好，如果您有在整個網站使用的單一配置。 不過，許多大型網站有自訂的網站配置在各個不同區段。 例如，假設醫院人員用來管理病患資訊、 活動和計費醫療保健應用程式。 可能有此應用程式中網頁的三種類型：

- 其中的工作人員可以更新可用性的人員成員特定頁面檢視排程，或要求休假時間。
- Patient 特定頁面的工作人員檢視或編輯某個特定的病患資訊的所在。
- 其中會計檢閱目前的計費特定頁面宣告狀態，而且的財務報告。

每個頁面可能會共用一般的版面配置，例如跨頂端和底部經常使用連結的一系列的功能表。 但人員、 病患和計費特定頁面，可能需要自訂此配置命名為泛型。 例如，可能是所有員工特定頁面應該都包含行事曆和工作清單，顯示目前登入使用者的可用性和每日排程。 可能是所有病患特定頁面都需要顯示名稱、 地址和其資訊被編輯的病患的保險資訊。

您可使用建立這類自訂版面配置*巢狀主版頁面*。 若要實作上述的案例，我們會先建立主版頁面以定義可自訂的區域 ContentPlaceHolders 定義全站台的版面配置、 功能表和頁尾內容。 然後，我們可以建立三個巢狀主版頁面，一個用於每種類型的 web 網頁。 每個巢狀主版頁面會定義在使用主版頁面的內容頁的型別之間的內容。 換句話說，巢狀主版頁面的病患特定內容頁面會包含標記和程式設計邏輯，來顯示正在編輯的病患的資訊。 當建立新的 patient 特定頁面，我們會將它繫結至這個巢狀主版頁面。

本教學課程一開始會反白顯示巢狀主版頁面的優點。 然後，它會示範如何建立和使用巢狀主版頁面。

> [!NOTE]
> .NET Framework 2.0 版自以來可能巢狀主版頁面。 不過，Visual Studio 2005 未包含巢狀主版頁面的設計階段支援。 好消息是 Visual Studio 2008 提供豐富的設計階段經驗，針對巢狀主版頁面。 如果您有興趣使用巢狀主版頁面，但仍在使用 Visual Studio 2005，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/)的部落格項目[VS 2005 設計階段中的巢狀主版頁面的提示](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)。


## <a name="the-benefits-of-nested-master-pages"></a>巢狀主版頁面的優點

許多網站具有由為名的站台設計，以及頁面的特定類型的特定自訂的設計多。 比方說，我們示範 web 應用程式中我們建立了初步的 [管理] 區段 (在頁面`~/Admin`資料夾)。 目前在網頁`~/Admin`資料夾做為這些頁面不在 [管理] 區段中使用相同的主版頁面 (也就是`Site.master`或`Alternate.master`，取決於使用者的選取範圍)。

> [!NOTE]
> 現在，假設我們的網站有一個主版頁面`Site.master`。 我們將其使用與 「 使用巢狀主版頁面的系統管理區段 」 為開頭的兩個 （或以上） 主版的頁面的巢狀主版頁面稍後在本教學課程。


假設我們已自訂為包含其他資訊或連結，否則將不會出現在網站中的其他頁面的 [管理] 頁面的配置要求。 有四個方法來實作這項需求：

1. 手動將管理特定的資訊和連結新增至每個內容頁面中`~/Admin`資料夾。
2. 更新`Site.master`主版頁面包括系統管理區段特有的資訊和連結，然後再新增主版頁面，即可顯示或隱藏這些區段的 程式碼會根據是否造訪其中一個系統管理頁面。
3. 建立新的主版頁面專為系統管理] 區段中，複製 [從標記`Site.master`加入系統管理區段特有的資訊和連結，，然後更新中的內容頁面`~/Admin`来使用這個新的主要資料夾頁面。
4. 建立繫結至巢狀主版頁面`Site.master`，而且在沒有內容頁面`~/Admin`資料夾使用這個新巢狀主版頁面。 此巢狀主版頁面會包含其他資訊和特定管理網頁的連結，就不需要重複的標記已經定義於`Site.master`。

第一個選項是至少可行的。 使用主版頁面的重點是將移開不必以手動方式複製和貼上加入新的 ASP.NET 網頁的一般標記。 第二個選項可以接受，但是可讓應用程式比較不容易維護為它 bulks 註冊標記，偶爾才會顯示，並需要開發人員編輯主版頁面若要解決此標記，並需要記住時，使用主版頁面牌，某些標記會顯示與隱藏時。 這個方法是較不 tenable 為越來越多的類型，可容納這個單一主版頁面所需的 web 網頁的自訂項目。

第三個選項會移除和複雜性問題浮現的第二個選項。 不過，三個的選項主要缺點是，我們必須複製並貼上從一般的版面配置`Site.master`至新的管理特定區段的主要頁面。 如果我們稍後決定變更全站台的版面配置，我們必須請記得要變更在兩個地方。

第四個選項中，巢狀主版頁面，讓我們最棒的是第二個和第三個選項。 全站台的配置資訊是在特定區域的特定內容分割成不同的檔案時，在一個檔案的最上層的主版頁面-進行維護。

本教學課程開頭看建立和使用簡單的巢狀主版頁面。 我們建立全新的最上層主版頁面、 兩個巢狀主版頁面和兩個內容頁面。 從 「 使用巢狀主版頁面的系統管理區段 」 開始，我們會審視更新我們現有的主版頁面架構，以包括使用巢狀主版頁面。 具體來說，我們建立巢狀主版頁面，並使用它來包含其他自訂內容，內容頁面的`~/Admin`資料夾。

## <a name="step-1-creating-a-simple-top-level-master-page"></a>步驟 1： 建立簡單的最上層主版頁面

建立巢狀主版根據其中一個現有的主要頁面，然後更新現有的內容頁面，即可使用這個新的巢狀主版頁面而不是最上層的主版頁面需要少許複雜性，因為現有的內容頁面已經預期某些ContentPlaceHolder 控制項最上層的主版頁面中定義。 因此，巢狀主版頁面也必須包含具有相同名稱的相同 ContentPlaceHolder 控制項。 此外，我們示範特定的應用程式有兩個主版頁面 (`Site.master`和`Alternate.master`)，以動態方式指派給內容頁面根據使用者的喜好設定，以進一步將新增到這種複雜性。 我們將探討更新現有的應用程式使用巢狀主版頁面稍後在本教學課程中，但我們第一次專注簡單巢狀主版頁面的範例。

建立新的資料夾，名為`NestedMasterPages`然後將新的主版頁面檔案新增到該資料夾中名為`Simple.master`。 （請參閱圖 1 的 [方案總管] 的螢幕擷取畫面之後加入這個資料夾和檔案。）拖曳`AlternateStyles.css`拖曳至設計工具的 [方案總管] 從樣式表檔案。 這樣會加入`<link>`項目中的樣式表檔案`<head>`其後的項目、 主版頁面的`<head>`項目的標記應該看起來：


[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

接下來，加入下列標記內的 Web Form `Simple.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

這個標記中海軍藍的背景上的大型白色字型頁面頂端顯示標題為 「 巢狀主版頁面 （簡單） 」 的連結。 下方的`MainContent`ContentPlaceHolder。 圖 1 顯示`Simple.master`主版頁面，在 Visual Studio 設計工具中載入時。


[![巢狀主版頁面內容的特定定義系統管理 」 區段中的頁面](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**圖 01**: 巢狀主版頁面定義內容的特定系統管理 」 區段中的頁面 ([按一下以檢視完整大小的影像](nested-master-pages-vb/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>步驟 2： 建立簡單的巢狀主版頁面

`Simple.master`包含兩個 ContentPlaceHolder 控制項： `MainContent` ContentPlaceHolder 我們加入表單中之 Web 連同`head`中的 ContentPlaceHolder`<head>`項目。 如果我們建立的內容頁面，並將它繫結`Simple.master`內容頁面將會有兩個參考兩個 ContentPlaceHolders 的內容控制項。 同樣地，如果我們建立巢狀主版頁面，並將它繫結`Simple.master`巢狀主版頁面將會有兩個內容控制項。

讓我們加入至新的巢狀主版頁面`NestedMasterPages`資料夾名為`SimpleNested.master`。 以滑鼠右鍵按一下`NestedMasterPages`資料夾，然後選擇 加入新項目。 這會開啟 [加入新項目] 對話方塊，在圖 2 所示。 選取的主版頁面範本類型和新的主版頁面名稱的類型。 若要表示新的主版頁面應該是巢狀主版頁面，選取 「 選取的主版頁面 」 核取方塊。

接下來，按一下 [新增] 按鈕。 這會顯示相同的 Select 繫結至主版頁面的內容頁面時，您會看到主版頁面對話方塊 （請參閱圖 3）。 選擇`Simple.master`主版頁面中的`NestedMasterPages`資料夾，按一下 [確定]。

> [!NOTE]
> 如果您已建立您的 ASP.NET 網站使用的 Web 應用程式專案的模型，而不網站專案模型不會看到在圖 2 所示的 [加入新項目] 對話方塊中，「 選取主版頁面 」 核取方塊。 若要使用 Web 應用程式專案的模型時，建立巢狀主版頁面中，您必須選擇巢狀主版頁面範本 （而不是主版頁面的範本）。 在 選取巢狀主版頁面的範本，然後按一下 新增 之後, 相同選取主版頁面圖 3 所示的對話方塊隨即出現。


[![請檢查&quot;選取的主版頁面&quot;核取方塊以加入巢狀主版頁面](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**圖 02**： 選取 「 選取主版頁面 」 的核取方塊加入巢狀主版頁面 ([按一下以檢視完整大小的影像](nested-master-pages-vb/_static/image6.png))


[![繫結至 Simple.master 主版頁面的巢狀主版頁面](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**圖 03**： 巢狀主版頁面，即可將繫結`Simple.master`主版頁面 ([按一下以檢視完整大小的影像](nested-master-pages-vb/_static/image9.png))


巢狀主版頁面的宣告式標記，底下所示，包含兩個參考最上層的主版頁面的兩個 ContentPlaceHolder 控制項的內容控制項。


[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

除了`<%@ Master %>`指示詞，巢狀主版頁面的初始宣告式標記等同於最初在繫結至相同的最上層主版頁面的內容頁面時所產生的標記。 類似的內容頁面`<%@ Page %>`指示詞，`<%@ Master %>`此處的指示詞包含`MasterPageFile`屬性，指定巢狀主版頁面上層主版頁面。 巢狀主版頁面和內容頁面繫結至相同的最上層主版頁面的主要差異是巢狀主版頁面可以包含 ContentPlaceHolder 控制項。 巢狀主版頁面的 ContentPlaceHolder 控制項定義區域 內容頁面可以讓自訂標記。

更新此巢狀主版頁面，使其顯示文字"Hello，從 SimpleNested ！" 對應至內容控制項中`MainContent`ContentPlaceHolder 控制項。


[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

此新增之後，請儲存巢狀主版頁面，然後加入新的內容頁面以`NestedMasterPages`資料夾名為`Default.aspx`，並將它繫結`SimpleNested.master`主版頁面。 加入此頁面時您可能會對以查看它包含沒有內容的控制項 （請參閱圖 4） 程式碼 ！ 內容頁面只能存取其*父*主頁面的 ContentPlaceHolders。 `SimpleNested.master`不包含任何 ContentPlaceHolder 控制項。因此，任何繫結至這個主版頁面的內容頁面不能包含任何內容的控制項。


[![新的內容頁面不包含內容控制項](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**圖 04**: 新內容頁包含沒有內容的控制項 ([按一下以檢視完整大小的影像](nested-master-pages-vb/_static/image12.png))


我們需要如何做為更新的巢狀主版頁面 (`SimpleNested.master`) 包含 ContentPlaceHolder 的控制項。 通常您會想要包括由其父代的主版頁面上，定義每個 ContentPlaceHolder，進而其下層主版頁面或內容頁面，即可使用任何最上層的主版頁面的 ContentPlaceHolder 到 ContentPlaceHolder 巢狀主版頁面控制項。

更新`SimpleNested.master`ContentPlaceHolder 包含在兩個內容控制項中的主版頁面。 讓 ContentPlaceHolder 控制項為其內容的控制項是指 ContentPlaceHolder 控制項具有相同名稱。 亦即，加入名為的 ContentPlaceHolder 控制項`MainContent`內容控制`SimpleNested.master`參考`MainContent`中的 ContentPlaceHolder `Simple.master`。 執行相同的動作中參考的內容控制項`head`ContentPlaceHolder。

> [!NOTE]
> 雖然建議您命名巢狀主版頁面中的 ContentPlaceHolder 控制項最上層的主版頁面中 ContentPlaceHolders 相同，則不需要這個命名對稱。 您可以在巢狀主版頁面中提供 ContentPlaceHolder 控制項，任何您喜歡的名稱。 不過，我發現容易記住 ContentPlaceHolders 對應與網頁如果我的最上層的主版頁面和巢狀主版頁面都使用相同名稱的哪些區域。


進行這些新增項目後您`SimpleNested.master`主版頁面的宣告式標記看起來應該類似下列：


[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

刪除`Default.aspx`內容剛才所建立的頁面，然後再重新加入，繫結至`SimpleNested.master`主版頁面。 這次 Visual Studio 會加入兩個內容控制項加入`Default.aspx`，現在參考 ContentPlaceHolders 中定義`SimpleNested.master`（請參閱圖 6）。 新增的文字"Hello，從 Default.aspx ！" 在內容控制參考`MainContent`。

圖 5 顯示這裡涉及三個實體`Simple.master`， `SimpleNested.master`，和`Default.aspx`-以及如何關聯到另一個。 如圖所示的巢狀主版頁面內容控制項中實作其父代 ContentPlaceHolder。 如果這些區域需要能夠存取內容頁面中，巢狀主版頁面就必須將自己 ContentPlaceHolders 加入內容控制項。


[![最上層和巢狀主版頁面聽寫內容頁面的版面配置](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**圖 05**: 最上層和巢狀主版頁面要求內容的頁面配置 ([按一下以檢視完整大小的影像](nested-master-pages-vb/_static/image15.png))


此行為說明內容頁或主版頁面的方式只有一點其上層的主版頁面。 此行為也會指示 Visual Studio 設計工具。 圖 6 顯示的設計工具`Default.aspx`。 哪些區域是從 [內容] 頁面可編輯和部分並不會清楚地顯示在設計工具，而不會釐清不可編輯的區域是從巢狀主版頁面和區域是從最上層的主版頁面。


[![內容頁面現在包含巢狀主版頁面 ContentPlaceHolders 內容控制項](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**圖 06**: 內容頁面現在包含內容控制項的巢狀主版頁面的 ContentPlaceHolders ([按一下以檢視完整大小的影像](nested-master-pages-vb/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>步驟 3： 加入第二個簡單巢狀主版頁面

有多個巢狀主版頁面時，巢狀主版頁面的優點是更明顯。 為了說明這項優點，建立另一個巢狀主版頁面中的`NestedMasterPages`資料夾，則將這個新的巢狀主版頁面`SimpleNestedAlternate.master`並將它繫結`Simple.master`主版頁面。 像我們在步驟 2 中，請在巢狀主版頁面的兩個內容控制項加入 ContentPlaceHolder 控制項。 也新增的文字"Hello，從 SimpleNestedAlternate ！" 對應至最上層的主版頁面的內容控制項中`MainContent`ContentPlaceHolder。 進行這些變更後您新巢狀主版頁面的宣告式標記看起來應該如下所示：


[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

建立名為的內容頁面`Alternate.aspx`中`NestedMasterPages`資料夾並將它繫結`SimpleNestedAlternate.master`巢狀主版頁面。 新增的文字"Hello，替代中 ！" 對應至內容控制項中`MainContent`。 圖 7 顯示`Alternate.aspx`時透過 Visual Studio 設計工具檢視。


[![Alternate.aspx 繫結至 SimpleNestedAlternate.master 主版頁面](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**圖 07**:`Alternate.aspx`繫結至`SimpleNestedAlternate.master`主版頁面 ([按一下以檢視完整大小的影像](nested-master-pages-vb/_static/image21.png))


比較圖 7，以圖 6 中的設計工具中的設計工具。 這兩個內容頁面共用相同的最上層的主版頁面中定義的配置 (`Simple.master`)，也就是 「 巢狀主版頁面教學課程 （簡單） 」 標題。 同時還沒有相異內容定義於其父主版頁面的文字"Hello，從 SimpleNested ！" 在圖 6 和"Hello，從 SimpleNestedAlternate ！" 圖 7。 授與，這些差異這裡是簡單式 」、，但是您無法擴充此範例包含更有意義的差異。 比方說，`SimpleNested.master`頁面可能包含具有其內容的頁面的特定選項的功能表，而`SimpleNestedAlternate.master`可能必須將繫結至它的內容頁面的相關資訊。

現在，假設我們需要進行變更，為名的站台版面配置。 例如，假設我們想要將一般的連結清單加入至所有的內容頁面。 若要完成此我們更新最上層的主版頁面`Simple.master`。 那里任何變更會立即反映其巢狀主版頁面中，並由延伸模組，其內容的頁面。

若要示範的輕鬆，我們可以變更為名的站台版面配置，請開啟`Simple.master`主版頁面，並加入下列標記之間`topContent`和`mainContent``<div>`項目：


[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

這會將兩個連結，加入繫結至每一頁頂端`Simple.master`， `SimpleNested.master`，或`SimpleNestedAlternate.master`; 這些變更立即套用至所有巢狀主版頁面和其內容的頁面。 圖 8 顯示`Alternate.aspx`透過瀏覽器檢視時。 請注意增加 （相較於圖 7） 的頁面頂端的連結。


[![變更為頂層主版頁面會立即反映在其巢狀主版頁面和其內容頁面](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**圖 08**： 變更為頂層主版頁面會立即反映在其巢狀主版頁面和其內容頁面 ([按一下以檢視完整大小的影像](nested-master-pages-vb/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>使用巢狀主版頁面的 [管理] 區段

此時我們看的優點巢狀主版頁面，並已經看到如何建立和 ASP.NET 應用程式中使用它們。 不過，在步驟 1、 2 和 3，範例牽涉到建立新的最上層主版頁面、 新巢狀主版頁面和新的內容頁面。 如何新增新巢狀主版頁面的網站與現有的最上層主版頁面和內容頁面嗎？

將巢狀主版頁面整合至現有的網站並將它與現有的內容頁面需要更多心力比從頭開始。 步驟 4、 5、 6 和 7 瀏覽這些挑戰，我們擴大要包含新的巢狀主版頁面名稱為我們示範應用程式為`AdminNested.master`，包含系統管理員的指示，而且會由 ASP.NET 網頁中`~/Admin`資料夾。

將巢狀主版頁面整合到我們的示範應用程式中導入了下列障礙：

- 現有的內容中的分頁`~/Admin`資料夾有預期從其主版頁面。 首先，他們期望應用程式必須有特定 ContentPlaceHolder 的控制項。 此外，`~/Admin/AddProduct.aspx`和`~/Admin/Products.aspx`頁面呼叫主版頁面的公用`RefreshRecentProductsGrid`方法，設定其`GridMessageText`屬性，或具有事件處理常式其`PricesDoubled`事件。 因此，我們的巢狀主版頁面必須提供相同 ContentPlaceHolders 和 public 成員。
- 在前述教學課程中我們增強`BasePage`類別來動態設定`Page`物件的`MasterPageFile`根據工作階段變數的屬性。 我們如何以支援動態主版頁面時使用巢狀主版頁面？

我們建立的巢狀主版頁面，並使用它從我們現有的內容頁面，就會出現這些兩個挑戰。 我們將調查，並出現時加以克服這些問題。

## <a name="step-4-creating-the-nested-master-page"></a>步驟 4： 建立巢狀主版頁面

我們第一項工作是建立巢狀主版頁面，以供系統管理 」 區段中的頁面。 加入新的巢狀主版頁面時，我們看到在步驟 2 中，我們需要將指定的巢狀主版頁面的上層主版頁面。 但我們有兩個最上層的主版頁面：`Site.master`和`Alternate.master`。 提醒您，我們建立`Alternate.master`前述教學課程中，在撰寫程式碼`BasePage`類別該組`Page`物件的`MasterPageFile`屬性在執行階段為`Site.master`或`Alternate.master`值而定`MyMasterPage`工作階段的變數。

我們如何設定我們巢狀主版頁面使其使用適當的最上層主版頁面？ 我們有兩個選項：

- 建立兩個巢狀主版頁面`AdminNestedSite.master`和`AdminNestedAlternate.master`，並將其繫結至最上層的主版頁面`Site.master`和`Alternate.master`分別。 在`BasePage`，接著，我們會設定`Page`物件的`MasterPageFile`適當的巢狀主版頁面。
- 建立單一巢狀主版頁面再使用這個特定的主版頁面的內容頁面。 然後，在執行階段，我們將需要設定巢狀主版頁面`MasterPageFile`適當最上層主版頁面在執行階段屬性。 (您可能已經了解到目前為止，主版頁面也會有`MasterPageFile`屬性。)

我們將使用第二個選項。 建立單一巢狀主版頁面檔案中的`~/Admin`資料夾名為`AdminNested.master`。 因為同時`Site.master`和`Alternate.master`ContentPlaceHolder 控制項的設定相同，無論何種主版頁面您將它繫結，雖然建議您將繫結至`Site.master`為了一致性的起見。


[![加入 ~/Admin 資料夾中的巢狀主版頁面。](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**圖 09**： 加入巢狀主版頁面，即可`~/Admin`資料夾。 ([按一下以檢視完整大小的影像](nested-master-pages-vb/_static/image27.png))


因為巢狀主版頁面繫結至具有四個 ContentPlaceHolder 控制項在主版頁面中，Visual Studio 會將四個內容控制項加入新的巢狀主版頁面檔案初始標記。 像我們在步驟 2 和 3，每個內容控制項，以加入 ContentPlaceHolder 控制項提供相同的名稱做為最上層的主版頁面的 ContentPlaceHolder 控制項。 也對應至內容控制項中加入下列標記`MainContent`ContentPlaceHolder:


[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

接下來，定義`instructions`CSS 類別`Styles.css`和`AlternateStyles.css`CSS 檔案。 下列 CSS 規則導致 HTML 項目與樣式`instructions`淺黃色背景色彩與實心黑色框線要顯示的類別：


[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

因為此標記已加入巢狀主版頁面，只會使用巢狀主版頁面 （也就是在 [管理] 區段中的頁面），這些頁面中出現。

這些新增項目巢狀主版頁面之後，其宣告式標記看起來應該如下所示：


[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

請注意，每個內容控制項的 ContentPlaceHolder 控制項，ContentPlaceHolder 控制項的`ID`對應 ContentPlaceHolder 控制項最上層的主版頁面中相同的值指派給屬性。 此外，系統管理區段特定標記會出現在`MainContent`ContentPlaceHolder。

圖 10 顯示`AdminNested.master`時透過 Visual Studio 設計工具檢視的巢狀主版頁面。 在頂端的黃色方塊中的指示，請參閱 <<c0> `MainContent` 內容控制項。


[![巢狀主版頁面延伸到包含的系統管理員指示最上層的主版頁面。](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**圖 10**： 巢狀主版頁面延伸到包含的系統管理員指示最上層的主版頁面。 ([按一下以檢視完整大小的影像](nested-master-pages-vb/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>步驟 5： 更新現有的內容頁面，以使用新的巢狀主版頁面

每當我們將新的內容頁面加入至 [管理] 區段，我們需要將它繫結`AdminNested.master`剛才所建立的主版頁面。 但怎麼的現有內容頁？ 目前，在站台中的所有內容頁面是衍生自`BasePage`類別，以程式設計方式在執行階段設定內容頁面的主版頁面。 這不是我們想要在 [管理] 區段中的內容頁面的行為。 相反地，我們想要一律使用這些內容頁面`AdminNested.master`頁面。 它將會選擇在執行階段權限最上層的內容頁面的巢狀主版頁面的責任。

最佳的方式來達成這所需的行為是建立新的自訂基礎頁面類別，名為`AdminBasePage`延伸`BasePage`類別。 `AdminBasePage`接著可以覆寫`SetMasterPageFile`並設定`Page`物件的`MasterPageFile`硬式編碼值"~ / Admin/AdminNested.master"。 如此一來，任何分頁，衍生自`AdminBasePage`將使用`AdminNested.master`，而任何分頁，衍生自`BasePage`會有其`MasterPageFile`屬性設定為動態"~ / Site.master"或"~ / Alternate.master"的值為基礎`MyMasterPage`工作階段變數。

將新的類別檔案來開始`App_Code`資料夾名為`AdminBasePage.vb`。 具有`AdminBasePage`擴充`BasePage`，然後覆寫`SetMasterPageFile`方法。 在該方法會將指派`MasterPageFile`值"~ / Admin/AdminNested.master"。 這些變更後您的類別檔案看起來應該如下所示：


[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

我們現在需要有現有內容頁，的管理 > 一節衍生自`AdminBasePage`而不是`BasePage`。 移至每個內容頁面中的程式碼後置類別檔`~/Admin`資料夾並進行這項變更。 例如，在`~/Admin/Default.aspx`想要變更的程式碼後置類別宣告：


[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

收件者:


[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

圖 11 顯示如何最上層的主版頁面 (`Site.master`或`Alternate.master`)，巢狀主版頁面 (`AdminNested.master`)，並相互關聯管理 > 一節的內容頁面。


[![巢狀主版頁面內容的特定定義系統管理 」 區段中的頁面](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**圖 11**: 巢狀主版頁面定義內容的特定系統管理 」 區段中的頁面 ([按一下以檢視完整大小的影像](nested-master-pages-vb/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>步驟 6： 鏡像主版頁面的公用方法和屬性

請記得，`~/Admin/AddProduct.aspx`和`~/Admin/Products.aspx`頁面以程式設計方式使用主版頁面的互動：`~/Admin/AddProduct.aspx`呼叫主版頁面的公用`RefreshRecentProductsGrid`方法，並設定其`GridMessageText`屬性。`~/Admin/Products.aspx`具有事件處理常式`PricesDoubled`事件。 在前述教學課程中我們建立`MustInherit``BaseMasterPage`定義這些公用成員的類別。

`~/Admin/AddProduct.aspx`和`~/Admin/Products.aspx`頁面假設其主版頁面衍生自`BaseMasterPage`類別。 `AdminNested.master`  頁面上，不過，目前延伸`System.Web.UI.MasterPage`類別。 如此一來，瀏覽時`~/Admin/Products.aspx``InvalidCastException`會擲回訊息: 「 無法將型別的物件轉換 'ASP.admin\_adminnested\_主要' 輸入 'BaseMasterPage'。 」

若要修正此我們必須將`AdminNested.master`程式碼後置類別延伸`BaseMasterPage`。 更新從巢狀主版頁面的程式碼後置類別宣告：


[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

收件者:


[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

我們您尚未完成。 我們要覆寫成員標示為`MustOverride`，也就是`RefreshRecentProductsGrid`和`GridMessageText`。 最上層的主版頁面會使用這些成員，以更新其使用者介面。 (僅限其實`Site.master`主版頁面會使用這些方法中，雖然這兩個最上層的主版頁面會實作這些方法，因為兩者擴充`BaseMasterPage`。)

雖然我們必須實作這些成員在`AdminNested.master`，這些實作需要是只要使用巢狀主版頁面的最上層的主版頁面中呼叫相同的成員。 比方說，當 [管理] 區段中的內容頁面呼叫巢狀主版頁面`RefreshRecentProductsGrid`方法時，巢狀主版頁面需要，接著，請呼叫`Site.master`或`Alternate.master`的`RefreshRecentProductsGrid`方法。

若要達成此目的，一開始加入下列`@MasterType`指示詞加入頂端`AdminNested.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

請記得，`@MasterType`指示詞將強型別屬性，加入名為的程式碼後置類別`Master`。 然後覆寫`RefreshRecentProductsGrid`和`GridMessageText`成員，以及只委派呼叫`Master`的對應方法：


[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

此位置的程式碼，您應該能夠瀏覽和使用 [管理] 區段中的內容的頁面。 圖 12 顯示`~/Admin/Products.aspx`頁面上透過瀏覽器檢視時。 如您所見，此頁面包含系統管理指示方塊中，定義巢狀主版頁面中。


[![在 [管理] 區段中的內容頁面包含每一頁頂端的指示](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**圖 12**: 管理 > 一節包含指示在頂端的每個頁面中的內容頁面 ([按一下以檢視完整大小的影像](nested-master-pages-vb/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>步驟 7： 在執行階段使用適當的最上層主版頁面

[管理] 區段中的所有內容頁面完全正常運作時，它們全都使用相同的最上層主版頁面，並忽略在使用者選取的主版頁面`ChooseMasterPage.aspx`。 這是因為，巢狀主版頁面有其`MasterPageFile`屬性以靜態方式設定為`Site.master`中其`<%@ Master %>`指示詞。

若要使用最上層的主版頁面，所以我們需要將使用者選取`AdminNested.master`的`MasterPageFile`屬性中的值`MyMasterPage`工作階段變數。 因為我們設定內容的頁面`MasterPageFile`中的屬性`BasePage`，您可能會認為我們可以將巢狀主版頁面`MasterPageFile`屬性`BaseMasterPage`或`AdminNested.master`的程式碼後置類別。 這將無法運作，不過，因為我們需要有設定`MasterPageFile`PreInit 階段結束的屬性。 我們可以透過程式設計方式挖掘網頁生命週期，從主版頁面的最早時間是在初始化階段 （其中 PreInit 階段之後，就會發生）。

因此，我們需要設定巢狀主版頁面`MasterPageFile`頁面之內容的屬性。 僅限內容頁使用`AdminNested.master`主版頁面是衍生自`AdminBasePage`。 因此，我們那里可以將此邏輯。 在步驟 5 中我們已覆寫`SetMasterPageFile`方法，將頁面物件的`MasterPageFile`屬性為"~ / Admin/AdminNested.master"。 更新`SetMasterPageFile`也設定主版頁面的`MasterPageFile`屬性儲存在工作階段的結果：


[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

`GetMasterPageFileFromSession`方法，我們加入`BasePage`前述教學課程中，傳回適當的主版頁面檔案路徑會根據工作階段的變數值中的類別。

完成備妥這項變更，使用者的主版頁面的選取範圍通訊系統管理 」 區段。 圖 13 顯示相同頁面圖 12，但使用者變更其主版頁面選取範圍後`Alternate.master`。


[![巢狀的管理頁面會使用使用者選取的最上層的主版頁面](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**圖 13**： 巢狀的 [管理] 頁面會使用最高層級主版頁面中選取的使用者 ([按一下以檢視完整大小的影像](nested-master-pages-vb/_static/image39.png))


## <a name="summary"></a>總結

太多 like 如何在內容頁面可以繫結至主版頁面中，就可以建立巢狀主版頁面，讓子主版頁面繫結至父主版頁面。 子主版頁面可能會針對每個父代的 ContentPlaceHolders; 定義內容控制項它可以將自己 ContentPlaceHolder 控制項 （以及其他標記） 加入這些內容控制項。 巢狀主版頁面是在大型的 web 應用程式，其中所有頁面都共用為名的外觀與風格，但有特定區段的網站需要唯一的自訂項目相當實用。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [巢狀的 ASP.NET 主版頁面](https://msdn.microsoft.com/en-us/library/x2b3ktt7.aspx)
- [巢狀主版頁面和 VS 2005 設計階段的秘訣](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [VS 2008 巢狀主版頁面支援](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP/ASP.NET 書籍和 4GuysFromRolla.com 的創辦，目前正在使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 在可到達 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過在他的部落格[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一步](specifying-the-master-page-programmatically-vb.md)
