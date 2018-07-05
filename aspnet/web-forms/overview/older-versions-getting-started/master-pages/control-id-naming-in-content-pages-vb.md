---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
title: 控制項識別碼命名內容頁面 (VB) |Microsoft Docs
author: rick-anderson
description: 說明如何 ContentPlaceHolder 控制項做為命名容器，因此做以程式設計方式使用控制項很困難 （透過 FindConrol)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: dbb024a6-f043-4fc5-ad66-56556711875b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 1b922fb230169824659222da0b9504ec38c36d8a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387708"
---
<a name="control-id-naming-in-content-pages-vb"></a>控制項中的識別碼命名內容頁面 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_VB.zip)或[下載 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_VB.pdf)

> 說明如何 ContentPlaceHolder 控制項做為命名容器，因此做以程式設計方式使用很困難 （透過 FindConrol) 控制項。 查看這個問題及因應措施。 也會討論如何以程式設計方式存取產生的 ClientID 值。


## <a name="introduction"></a>簡介

所有的 ASP.NET 伺服器控制項包括`ID`屬性，可唯一識別控制項，並會用該控制項以程式設計方式存取程式碼後置類別中的方法。 同樣地，在 HTML 文件中的項目可能包括`id`屬性可唯一識別項目; 這些`id`值通常在用戶端指令碼中用來以程式設計方式參考特定的 HTML 項目。 如此一來，您可能會假設，當 ASP.NET 伺服器控制項轉譯為 HTML，其`ID`值作為`id`所呈現的 HTML 項目的值。 這不一定如此因為在某些情況下使用單一來控制單一`ID`值可能會多次出現在呈現的標記。 請考慮 GridView 控制項，其中包含與使用標籤 Web 控制項為 TemplateField`ID`的值`ProductName`。 GridView 繫結至其資料來源，在執行階段，此標籤會針對每個 GridView 資料列重複一次。 每個呈現標籤需求唯一`id`值。

若要處理這種情況下，ASP.NET 會允許特定的控制項，以表示為命名容器。 做為新的命名容器`ID`命名空間。 會出現在命名容器中任何伺服器控制項有其呈現`id`值前面加上`ID`命名的容器控制項。 例如，`GridView`和`GridViewRow`類別是這兩個命名的容器。 因此，一個 Label 控制項，定義於與 GridView TemplateField `ID` `ProductName`指定呈現`id`的值`GridViewID_GridViewRowID_ProductName`。 因為*GridViewRowID*都是唯一的每個 GridView 資料列，產生`id`值是唯一的。

> [!NOTE]
> [ `INamingContainer`介面](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx)用來表示特定的 ASP.NET 伺服器控制項應該做的命名容器。 `INamingContainer`介面不會不會拼出任何伺服器控制項必須實作的方法; 相反地，它用做為標記。 在產生時呈現的標記，如果控制項實作此介面，然後 ASP.NET 引擎自動前置詞，其`ID`子代的值呈現`id`屬性值。 此程序會在步驟 2 中的更詳細討論。


命名的容器不只變更轉譯`id`屬性值，但也會影響如何控制項可能會從 ASP.NET 頁面的程式碼後置類別以程式設計方式參考。 `FindControl("controlID")`方法通常用來以程式設計方式參考的 Web 控制項。 不過，`FindControl`無法穿透透過命名容器。 因此，您無法直接使用`Page.FindControl`參考 GridView 或其他命名的容器內控制項的方法。

因為您可能會有猜想，主版頁面和 ContentPlaceHolders 會同時實作為命名容器。 在本教學課程中我們將探討如何主版頁面影響 HTML 項目`id`值，以及如何以程式設計方式參考 [內容] 頁面，使用的 Web 控制項`FindControl`。

## <a name="step-1-adding-a-new-aspnet-page"></a>步驟 1： 加入新的 ASP.NET 網頁

為了示範在本教學課程所討論的概念，讓我們將新的 ASP.NET 網頁新增至我們的網站。 建立名為的新內容頁面`IDIssues.aspx`在根資料夾中，將它繫結至`Site.master`主版頁面。


![新增至根資料夾的內容頁面 IDIssues.aspx](control-id-naming-in-content-pages-vb/_static/image1.png)

**圖 01**： 新增內容頁`IDIssues.aspx`的根資料夾


Visual Studio 會針對每個主版頁面的四個 ContentPlaceHolders，自動建立的內容控制項。 如中所述[*多個 ContentPlaceHolders 和預設內容*](multiple-contentplaceholders-and-default-content-vb.md)教學課程中，如果內容控制項不存在要改為發出主版頁面的預設 ContentPlaceHolder 內容。 因為`QuickLoginUI`並`LeftColumnContent`ContentPlaceHolders 包含適合的預設標記，此頁面，就移除其對應的內容控制項從`IDIssues.aspx`。 此時，[內容] 頁面的宣告式標記看起來應該如下所示：


[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample1.aspx)]

在  [*指定主版頁面的標題、 中繼標籤及其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)教學課程中我們建立自訂的基底頁面類別 (`BasePage`)，會自動設定頁面的標題，如果它是未明確設定。 針對`IDIssues.aspx`頁面，即可採用這項功能，在頁面的程式碼後置類別必須衍生自`BasePage`類別 (而不是`System.Web.UI.Page`)。 修改程式碼後置類別的定義，讓它看起來如下所示：


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample2.vb)]

最後，更新`Web.sitemap`檔案，以包含這一課中新的項目。 新增`<siteMapNode>`項目並將其`title`並`url`屬性加入 「 控制項識別碼命名問題 」 和`~/IDIssues.aspx`分別。 進行此新增後您`Web.sitemap`檔案的標記看起來應該如下所示：


[!code-xml[Main](control-id-naming-in-content-pages-vb/samples/sample3.xml)]

圖 2 所示，在新的站台對應項`Web.sitemap`會立即反映在左側的資料行中的 課程 區段。


![[課程] 區段現在包含連結&quot;控制項識別碼命名的問題&quot;](control-id-naming-in-content-pages-vb/_static/image2.png)

**圖 02**: 課程 區段現在包含 「 控制項識別碼命名的問題 」 的連結


## <a name="step-2-examining-the-renderedidchanges"></a>步驟 2： 檢查呈現`ID`變更

為了進一步了解修改 ASP.NET 引擎會呈現`id`值的伺服器控制項，讓我們加入一些 Web 控制項`IDIssues.aspx`頁面上，然後再檢視呈現至瀏覽器傳送的標記。 具體來說，在文字中的型別 「 請輸入您的年齡: 」 後面的文字方塊中的 Web 控制項。 進一步向下頁面上加入按鈕 Web 控制項和標籤 Web 控制項。 設定 TextBox`ID`並`Columns`屬性，以`Age`和 3，分別。 將按鈕的`Text`並`ID`屬性，以 「 提交 」 和`SubmitButton`。 清除標籤`Text`屬性並設定其`ID`至`Results`。

此時您內容的控制項宣告式標記看起來應該如下所示：


[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample4.aspx)]

圖 3 顯示頁面上，當透過 Visual Studio 設計工具檢視。


[![此頁面包含三個 Web 控制項： 文字方塊、 按鈕和標籤](control-id-naming-in-content-pages-vb/_static/image4.png)](control-id-naming-in-content-pages-vb/_static/image3.png)

**圖 03**: 頁面包含三個 Web 控制項： 文字方塊、 按鈕和標籤 ([按一下以檢視完整大小的影像](control-id-naming-in-content-pages-vb/_static/image5.png))


請瀏覽透過瀏覽器頁面，然後檢視 HTML 原始檔。 為如下所示，標記`id`值的文字方塊、 按鈕和標籤 Web 控制項的 HTML 項目是組合`ID`Web 控制項的值和`ID`頁面中的命名容器的值。


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample5.html)]

如稍早在本教學課程中所述，則主版頁面和其 ContentPlaceHolders 將做為命名容器。 因此，兩者都提供轉譯`ID`其巢狀控制項的值。 取得文字方塊的`id`屬性，例如： `ctl00_MainContent_Age`。 請注意，文字方塊控制項的`ID`的值為`Age`。 這加上其 ContentPlaceHolder 控制項的`ID`值， `MainContent`。 此外，這個值前面會加上的主版頁面`ID`值， `ctl00`。 最後的結果是`id`屬性值組成`ID`主版頁面、 ContentPlaceHolder 控制項和文字方塊本身的值。

圖 4 說明此行為。 若要判斷呈現`id`的`Age`文字方塊中，開頭`ID`值的文字方塊控制項， `Age`。 接下來，進行控制項階層架構。 在每個命名容器 （這些節點會以桃色色彩），前置詞呈現目前`id`命名的容器使用`id`。


![轉譯 id 屬性是基礎上識別碼的值命名的容器](control-id-naming-in-content-pages-vb/_static/image6.png)

**圖 04**: 轉譯`id`屬性是根據`ID`命名容器的值


> [!NOTE]
> 如我們所討論，`ctl00`部分轉譯`id`屬性會構成`ID`值的主版頁面中，但您可能想知道如何將這個`ID`值產生了。 我們並未指定其任何地方在我們的主要或內容頁面。 大部分的伺服器控制項在 ASP.NET 網頁會明確地新增透過頁面的宣告式標記。 `MainContent`標記中明確指定 ContentPlaceHolder 控制項`Site.master`;`Age`文字方塊中所定義`IDIssues.aspx`的標記。 我們可以指定`ID`針對這些類型的控制項，透過 [屬性] 視窗或從宣告式語法的值。 宣告式標記中未定義其他控制項，例如主版頁面本身。 因此，其`ID`值必須為我們自動產生。 ASP.NET 引擎集`ID`在執行階段識別碼尚未明確設定這些控制項的值。 它會使用命名模式`ctlXX`，其中*XX*是循序遞增的整數值。


主版頁面本身可做為命名容器，因為定義中的主版頁面的 Web 控制項也有改變轉譯`id`屬性值。 例如，`DisplayDate`我們新增至主版頁面中的標籤[*使用主版頁面建立全網站的版面配置*](creating-a-site-wide-layout-using-master-pages-vb.md)教學課程包含下列呈現標記：


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample6.html)]

請注意，`id`屬性包含這兩個主版頁面的`ID`值 (`ctl00`) 和`ID`Label Web 控制項的值 (`DateDisplay`)。

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>步驟 3： 以程式設計方式參考透過 Web 控制項`FindControl`

每個 ASP.NET 伺服器控制項包括`FindControl("controlID")`方法，會搜尋控制項，名為控制項的下階*controlID*。 如果找到這類控制項，則會傳回此錯誤;如果不找到任何相符的控制項，則`FindControl`傳回`Nothing`。

`FindControl` 您需要存取控制項，但您不需要直接參考它的案例中很有用。 當使用資料 Web 控制項，如 GridView，比方說，在宣告式語法中，一次定義 GridView 的欄位內的控制項，但在執行階段建立控制項的執行個體的每個 GridView 資料列。 因此，在執行階段產生的控制項存在，但我們並沒有可從程式碼後置類別的直接參考。 如此一來，我們需要使用`FindControl`以程式設計方式使用 GridView 的欄位內的特定控制項。 (如需有關使用`FindControl`若要存取資料 Web 控制項的範本內的控制項，請參閱[自訂格式設定在資料](../../data-access/custom-formatting/custom-formatting-based-upon-data-vb.md)。)以動態方式將 Web 控制項新增至 Web 表單時，就會發生這種狀況下，主題所述[建立動態資料輸入使用者介面](https://msdn.microsoft.com/library/aa479330.aspx)。

若要說明如何使用`FindControl`方法來搜尋控制項中內容的頁面上，建立事件處理常式`SubmitButton`的`Click`事件。 事件處理常式中加入下列程式碼，以程式設計方式參考`Age`文字方塊並`Results`加上標籤使用`FindControl`方法，然後顯示中的訊息`Results`根據使用者的輸入。

> [!NOTE]
> 當然，我們不需要使用`FindControl`參考此範例中的標籤和文字方塊控制項。 我們無法參考它們直接透過其`ID`屬性值。 我使用`FindControl`這裡以說明使用時，會發生什麼事`FindControl`從內容頁面。


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample7.vb)]

雖然用來呼叫的語法`FindControl`方法稍有不同的前兩行`SubmitButton_Click`，這些語意相等。 回想一下，所有的 ASP.NET 伺服器控制項包括`FindControl`方法。 這包括`Page`從哪一個所有 ASP.NET 程式碼後置類別必須衍生自的類別。 因此，呼叫`FindControl("controlID")`相當於呼叫`Page.FindControl("controlID")`，假設您還沒有覆寫`FindControl`方法在您的程式碼後置類別或自訂的基底類別中。

輸入此程式碼之後, 請瀏覽`IDIssues.aspx`透過瀏覽器頁面上，輸入您的年齡，然後按一下 [提交] 按鈕。 按一下 [提交] 按鈕後`NullReferenceException`，就會引發 （請參閱 [圖 5]）。


[![則引發 NullReferenceException](control-id-naming-in-content-pages-vb/_static/image8.png)](control-id-naming-in-content-pages-vb/_static/image7.png)

**圖 05**: A`NullReferenceException`就會引發 ([按一下以檢視完整大小的影像](control-id-naming-in-content-pages-vb/_static/image9.png))


如果您在中設定中斷點`SubmitButton_Click`您會看到，同時呼叫的事件處理常式`FindControl`傳回`Nothing`。 `NullReferenceException`我們嘗試存取時，會引發`Age`TextBox 的`Text`屬性。

問題在於`Control.FindControl`只會搜尋*控制*的相同的命名容器中的下階。 因為主版頁面構成新的命名容器，呼叫`Page.FindControl("controlID")`從未 permeates 主版頁面物件`ctl00`。 (若要檢視的控制項階層架構，它會顯示 圖 4 回頭參考`Page`物件做為物件的父系主版頁面`ctl00`。)因此，`Results`標籤和`Age`找不到文字方塊並`ResultsLabel`並`AgeTextBox`指派的值`Nothing`。

有兩種因應措施，這項挑戰： 我們可以向下切入，一個的命名容器一次，以適當的控制項;或者，我們可以建立自己`FindControl`permeates 命名容器的方法。 讓我們檢查每個選項。

### <a name="drilling-into-the-appropriate-naming-container"></a>深入探索的適當命名的容器

若要使用`FindControl`參考`Results`標籤或`Age`文字方塊中，我們必須呼叫`FindControl`從祖系控制項相同的命名容器中。 圖 4 顯示，如`MainContent`ContentPlaceHolder 控制項是唯一的上階的`Results`或`Age`，是在相同的命名容器中。 換句話說，呼叫`FindControl`方法，從`MainContent`控制項，在下面的程式碼片段所示正確傳回參考`Results`或`Age`控制項。


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample8.vb)]

不過，我們無法使用`MainContent`ContentPlaceHolder 從我們的內容頁面的程式碼後置類別因為 ContentPlaceHolder 定義主版頁面中，使用上述語法。 相反地，我們必須使用`FindControl`以取得參考`MainContent`。 中的程式碼取代`SubmitButton_Click`事件處理常式和下列修改：


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample9.vb)]

如果您瀏覽透過瀏覽器頁面，輸入您的年齡，然後按一下 [提交] 按鈕， `NullReferenceException` ，就會引發。 如果您在中設定中斷點`SubmitButton_Click`您會看到這個例外狀況發生於嘗試呼叫的事件處理常式`MainContent`物件的`FindControl`方法。 `MainContent`物件是否等於`Nothing`因為`FindControl`方法找不到名為"MainContent"的物件。 根本原因是與相同`Results`標籤及`Age`TextBox 控制項：`FindControl`頂端的控制項階層架構中啟動其搜尋，並無法穿透命名容器，但`MainContent`ContentPlaceHolder 是主版頁面內，也就是命名容器。

我們可以使用之前`FindControl`以取得參考`MainContent`，我們首先需要主版頁面控制項的參考。 一旦我們擁有主版頁面的參考，我們就可以取得參考`MainContent`透過 ContentPlaceHolder`FindControl`並從該處，參考`Results`標籤並`Age`文字方塊中 (同樣地，透過使用`FindControl`)。 但我們該如何取得主版頁面的參考嗎？ 藉由檢查`id`中呈現的標記屬性很明顯地，主版頁面的`ID`值是`ctl00`。 因此，我們可以使用`Page.FindControl("ctl00")`若要取得主版頁面的參考，然後使用該物件取得的參考`MainContent`，依此類推。 下列程式碼片段說明這個邏輯：


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample10.vb)]

雖然此程式碼當然可以，它會假設主版頁面的自動產生`ID`一律為`ctl00`。 它永遠不會是個不錯的主意，讓自動產生值的相關假設。

幸運的是，主版頁面的參考是可透過存取`Page`類別的`Master`屬性。 因此，而不需使用`FindControl("ctl00")`若要取得主版頁面的參考，才能存取`MainContent`ContentPlaceHolder，我們可以改為使用`Page.Master.FindControl("MainContent")`。 更新`SubmitButton_Click`為下列程式碼的事件處理常式：


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample11.vb)]

此時，瀏覽的頁面，透過瀏覽器中，輸入您的年齡，然後按一下 [提交] 按鈕顯示訊息`Results`加上標籤，如預期般運作。


[![在標籤中顯示使用者的年齡](control-id-naming-in-content-pages-vb/_static/image11.png)](control-id-naming-in-content-pages-vb/_static/image10.png)

**圖 06**: 使用者的存留期會顯示在標籤中 ([按一下以檢視完整大小的影像](control-id-naming-in-content-pages-vb/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>以遞迴方式搜尋整個命名容器

參考前一個程式碼範例的原因`MainContent`ContentPlaceHolder 控制項，從主版頁面中，然後`Results`標籤和`Age`TextBox 控制項從`MainContent`，是因為`Control.FindControl`方法只會搜尋內*控制*的命名容器。 擁有`FindControl`命名的容器內保持合理，在大部分情況下因為兩個不同的命名容器中的兩個控制項可能會有相同`ID`值。 請考慮的 GridView 會定義名為的 Label Web 控制項的大小寫`ProductName`其 TemplateFields 的其中一個內。 當資料繫結至在執行階段，GridView`ProductName`建立每個 GridView 資料列的標籤。 如果`FindControl`搜尋到所有的命名容器，我們稱為`Page.FindControl("ProductName")`，哪一個標籤執行個體應該`FindControl`傳回？ `ProductName`中第一個的 GridView 資料列標籤嗎？ 最後一個資料列中的一個嗎？

因此`Control.FindControl`只搜尋*控制*命名容器的意義在大部分情況下。 但有其他的情況下，例如面向我們，我們有一個唯一`ID`所有命名容器，而且想要避免精心參考每個存取控制項的控制項階層架構中的命名容器。 具有`FindControl`太以遞迴方式搜尋所有的命名容器可感知的 variant。 不幸的是，.NET Framework 不包含這種方法。

好消息是，我們可以建立自己`FindControl`方法，以遞迴方式搜尋所有的命名容器。 事實上，使用*擴充方法*我們可以加上`FindControlRecursive`方法來`Control`伴隨著其現有的類別`FindControl`方法。

> [!NOTE]
> 擴充方法是以 C# 3.0 和 Visual Basic 9 的語言與.NET Framework 3.5 版和 Visual Studio 2008 隨附的新的功能。 簡單地說，延伸方法可以讓開發人員建立新的方法，為現有類別類型透過特殊的語法。 如需有關這個實用的功能的詳細資訊，請參閱我文章[擴充方法與擴充基底型別功能](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)。


若要建立擴充方法，加入新檔案，以便`App_Code`名為資料夾`PageExtensionMethods.vb`。 新增名為擴充方法`FindControlRecursive`做為輸入採用`String`名為的參數`controlID`。 擴充方法，才能正常運作，是很重要的標示為類別`Module`且前面加上的擴充方法`<Extension()>`屬性。 此外，所有擴充方法必須都接受其第一個參數的型別物件的擴充方法套用至。

將下列程式碼加入`PageExtensionMethods.vb`檔案，以定義這`Module`而`FindControlRecursive`擴充方法：


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample12.vb)]

使用此程式碼就緒之後，返回`IDIssues.aspx`頁面的程式碼後置類別和標記為註解目前`FindControl`方法呼叫。 它們取代成呼叫`Page.FindControlRecursive("controlID")`。 最棒的擴充方法是，它們會出現 IntelliSense 下拉式清單中，直接。 如 [圖 7] 所示，當您鍵入`Page`然後按下句號`FindControlRecursive`方法包含在 IntelliSense 下拉式清單，以及其他`Control`類別方法。


[![擴充方法都包含在 IntelliSense 下拉式清單](control-id-naming-in-content-pages-vb/_static/image14.png)](control-id-naming-in-content-pages-vb/_static/image13.png)

**圖 07**： 在 IntelliSense 下拉式清單包含擴充方法 ([按一下以檢視完整大小的影像](control-id-naming-in-content-pages-vb/_static/image15.png))


輸入下列程式碼插入`SubmitButton_Click`事件處理常式，然後瀏覽頁面、 輸入您的年齡，然後按一下 [提交] 按鈕測試它。 在 圖 6 所示，所產生的輸出會顯示，「"您就時代歲 ！


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample13.vb)]

> [!NOTE]
> 剛接觸 C# 3.0 和 Visual Basic 9 中，如果您使用 Visual Studio 2005 擴充方法，是因為您無法使用擴充方法。 相反地，您必須實作`FindControlRecursive`協助程式類別中的方法。 [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx)在他的部落格文章中，有這類範例[微波的 ASP.NET 網頁並`FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx)。


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>步驟 4： 使用正確`id`屬性在用戶端指令碼中的值

本教學課程簡介所述，Web 控制項的呈現`id`屬性通常在用戶端指令碼中用來以程式設計方式參考特定的 HTML 項目。 例如，下列 JavaScript 參考 HTML 項目由其`id`，然後在強制回應的訊息方塊中顯示它的值：


[!code-csharp[Main](control-id-naming-in-content-pages-vb/samples/sample14.cs)]

回想一下，在 ASP.NET 頁面，並包含命名容器，轉譯的 HTML 項目的`id`屬性是與 Web 控制項的相同`ID`屬性值。 基於這個原因，您會嘗試硬碟中的程式碼`id`JavaScript 程式碼的屬性值。 也就是說，如果您知道您想要存取`Age`TextBox Web 控制項，透過用戶端指令碼中，執行這項操作，透過對呼叫`document.getElementById("Age")`。

此方法的問題是，當使用主版頁面 （或其他命名的容器控制項），轉譯的 HTML`id`而不是 Web 控制項的`ID`屬性。 您可能會瀏覽透過瀏覽器頁面，並檢視來判斷實際來源`id`屬性。 一旦您知道轉譯`id`值，您可以將它貼到呼叫`getElementById`來存取您要使用透過用戶端指令碼的 HTML 項目。 這種方法是不盡理想，因為網頁的特定變更控制階層架構，或變更`ID`命名的控制項的屬性並更改產生`id`屬性，藉此破壞您的 JavaScript 程式碼。

好消息是，`id`呈現的屬性值是在透過 Web 控制項的伺服器端程式碼中存取[`ClientID`屬性](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx)。 您應該使用這個屬性來判斷`id`屬性用於用戶端指令碼中的值。 例如，若要新增至頁面的 JavaScript 函式，呼叫時，會顯示的值`Age`在強制回應訊息方塊中，文字方塊中，新增下列程式碼`Page_Load`事件處理常式：


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample15.vb)]

上述程式碼會插入的值`Age`文字方塊中的`ClientID`屬性的 JavaScript 呼叫插入`getElementById`。 如果您瀏覽此頁面，透過瀏覽器，並檢視的 HTML 原始檔，您會發現下列 JavaScript 程式碼：


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample16.html)]

請注意如何正確`id`屬性值， `ctl00_MainContent_Age`，會出現在呼叫`getElementById`。 這個值會計算在執行階段，因為它適用於無論稍後對網頁控制階層架構。

> [!NOTE]
> 此 JavaScript 範例只示範如何新增 JavaScript 函式會正確地參考伺服器控制項所呈現的 HTML 項目。 若要使用此函式，您必須撰寫額外的 JavaScript，或某些特定的使用者動作瓿文件載入時呼叫的函數。 如需這些的詳細資訊及相關的主題，讀取[使用用戶端指令碼](https://msdn.microsoft.com/library/aa479302.aspx)。


## <a name="summary"></a>總結

某些 ASP.NET 伺服器控制項做為命名容器，而這會影響轉譯`id`屬性值及其子系的控制項以及 canvassed 由控制項的範圍`FindControl`方法。 關於主版頁面、 主版頁面本身和它的 ContentPlaceHolder 控制項命名容器。 因此，我們需要把闡述更多的工作，以程式設計方式參考內容的頁面使用的控制項`FindControl`。 在本教學課程中，我們檢查兩種技術： 鑽研到 ContentPlaceHolder 控制項並呼叫其`FindControl`方法，和正在復原自己`FindControl`實作，以遞迴方式搜尋所有的命名容器。

除了與參考 Web 控制項的命名容器導入的伺服器端問題，還有用戶端問題。 沒有命名容器的 Web 控制項的`ID`屬性值和呈現`id`屬性值是在相同的其中一個。 但加上命名容器，呈現`id`屬性同時包含`ID`Web 控制項和其控制項階層架構的上階中的命名容器的值。 這些命名問題是問題，只要您使用 Web 控制項的`ClientID`屬性來判斷呈現`id`屬性在用戶端指令碼的值。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 主版頁面和 `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [建立動態資料輸入的使用者介面](https://msdn.microsoft.com/library/aa479330.aspx)
- [擴充基底型別功能的擴充方法](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [如何： 參考 ASP.NET 主版頁面內容](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [主要頁面： 秘訣、 技巧和陷阱](http://www.odetocode.com/articles/450.aspx)
- [使用用戶端指令碼](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP 本書籍，他是 4GuysFromRolla.com 的創辦人，一直從事 Microsoft Web 技術自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 Scott 要聯絡[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Zack Jones 和 Suchi Barnerjee。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

> [!div class="step-by-step"]
> [上一頁](urls-in-master-pages-vb.md)
> [下一頁](interacting-with-the-master-page-from-the-content-page-vb.md)
