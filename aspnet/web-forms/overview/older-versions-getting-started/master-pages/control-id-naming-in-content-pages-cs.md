---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: 控制在內容頁 (C#) 命名識別碼 |Microsoft 文件
author: rick-anderson
description: 說明如何 ContentPlaceHolder 控制項做為命名的容器，因此做以程式設計方式使用 （透過 FindConrol) 困難控制項...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1e834c38457c8477e0c81598d32f1e98473949d7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="control-id-naming-in-content-pages-c"></a>在內容頁 (C#) 命名的控制項 ID
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip)或[下載 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> 說明如何 ContentPlaceHolder 控制項做為命名的容器，因此做以程式設計方式使用難 （透過 FindConrol) 控制項。 查看這個問題及因應措施。 也會討論如何以程式設計方式存取產生的 ClientID 值。


## <a name="introduction"></a>簡介

所有 ASP.NET 伺服器控制項都包括`ID`屬性，可唯一識別控制項，並會依據該控制項以程式設計方式存取程式碼後置類別中的方法。 同樣地，HTML 文件中的項目可能包括`id`屬性可唯一識別項; 這些`id`值通常在用戶端指令碼中用來以程式設計方式參考特定的 HTML 項目。 根據這點，您可能假設，當 ASP.NET 伺服器控制項轉譯為 HTML，其`ID`值作為`id`呈現之 HTML 項目的值。 這不一定愈因為在某些情況下使用單一來控制單一`ID`值可能會多次出現在呈現的標記。 請考慮 GridView 控制項包含與標籤 Web 控制項具有為 TemplateField`ID`產品名稱的值。 GridView 繫結至其資料來源，在執行階段，此標籤會針對每個 GridView 資料列重複一次。 每個呈現標籤需求唯一`id`值。

若要處理這種情況下，ASP.NET 會允許特定的控制項，以表示為命名容器。 命名的容器做為新`ID`命名空間。 任何伺服器控制項的命名容器內的顯示有其呈現`id`值前面加上`ID`命名的容器控制項。 例如，`GridView`和`GridViewRow`類別都是這兩個命名的容器。 因此，與 GridView TemplateField 中定義的標籤控制項`ID`ProductName 指定呈現`id`值`GridViewID_GridViewRowID_ProductName`。 因為*GridViewRowID*都是唯一的每個 GridView 資料列，產生`id`值是唯一的。

> [!NOTE]
> [ `INamingContainer`介面](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx)用來表示特定的 ASP.NET 伺服器控制項應該作為命名容器。 `INamingContainer`介面不會不拼出任何伺服器控制項必須實作的方法; 相反地，它用做為標記。 在產生呈現的標記時，如果控制項實作此介面然後 ASP.NET 引擎自動前置詞其`ID`及其下階的值轉譯`id`屬性值。 在步驟 2 中詳細討論此程序。


命名的容器不只變更轉譯`id`屬性值，但也會影響如何控制項可能會從 ASP.NET 網頁的程式碼後置類別以程式設計方式參考。 `FindControl("controlID")`方法通常用來以程式設計方式參考 Web 控制項。 不過，`FindControl`不滲入透過命名容器。 因此，您無法直接使用`Page.FindControl`方法來參考 GridView 或其他命名的容器內的控制項。

您可能會有 surmised，因為主版頁面和 ContentPlaceHolders 會同時實作為命名容器。 在本教學課程中，我們檢驗如何主版頁面影響 HTML 項目`id`值和方法來以程式設計方式參考內容的頁面使用的 Web 控制項`FindControl`。

## <a name="step-1-adding-a-new-aspnet-page"></a>步驟 1： 加入新的 ASP.NET 網頁

若要示範此教學課程中所討論的概念，讓我們我們的網站加入新的 ASP.NET 網頁。 建立名為的新內容頁面`IDIssues.aspx`在根資料夾中，將它繫結至`Site.master`主版頁面。


![新增至根資料夾的內容頁面 IDIssues.aspx](control-id-naming-in-content-pages-cs/_static/image1.png)

**圖 01**： 新增內容 頁面`IDIssues.aspx`到根資料夾


Visual Studio 會自動建立每個主版頁面的四個 ContentPlaceHolders 的內容控制項。 如中所述[*多個 ContentPlaceHolders 和預設內容*](multiple-contentplaceholders-and-default-content-cs.md)教學課程，如果將內容控制項不存在改為發出主版頁面的預設 ContentPlaceHolder 內容。 因為`QuickLoginUI`和`LeftColumnContent`ContentPlaceHolders 包含適合的預設這個網頁的標記、 繼續和移除其對應內容控制項從`IDIssues.aspx`。 此時，內容頁面的宣告式標記看起來應該如下所示：


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

在[*主版頁面中指定的標題、 Meta 標記和其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)教學課程中我們建立的自訂基礎頁面類別 (`BasePage`) 如果是自動設定頁面的標題未明確設定。 如`IDIssues.aspx`運用這項功能頁面上，在頁面的程式碼後置類別必須衍生自`BasePage`類別 (而不是`System.Web.UI.Page`)。 修改程式碼後置類別的定義，讓它看起來如下：


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

最後，更新`Web.sitemap`檔案以包含這一課中新項目。 新增`<siteMapNode>`項目並設定其`title`和`url`屬性，將 「 控制項 ID 命名問題 」 和`~/IDIssues.aspx`分別。 進行此新增之後您`Web.sitemap`檔案的標記應該看起來與下列類似：


[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

如圖 2 所示，新的站台對應項目中`Web.sitemap`會立即反映在左邊資料行中的課程 > 一節。


![課程區段現在包含的連結&quot;控制項 ID 命名的問題&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**圖 02**： 課程區段現在包含 「 控制項 ID 命名的問題 」 的連結


## <a name="step-2-examining-the-renderedidchanges"></a>步驟 2： 檢查呈現`ID`變更

為了進一步了解修改 ASP.NET 引擎會呈現`id`伺服器的值會控制，讓我們加入一些 Web 控制項`IDIssues.aspx`頁面上，然後再檢視呈現至瀏覽器所傳送的標記。 具體而言，在文字中的型別"請輸入您的年齡: 」 後面的文字方塊中的 Web 控制項。 進一步向下頁面上加入按鈕 Web 控制項和標籤 Web 控制項。 設定文字方塊的`ID`和`Columns`屬性`Age`和 3，分別。 設定按鈕的`Text`和`ID`"Submit"的屬性和`SubmitButton`。 清除標籤`Text`屬性並設定其`ID`至`Results`。

此時內容控制項的宣告式標記看起來應該如下所示：


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

圖 3 顯示時透過 Visual Studio 設計工具檢視的頁面。


[![此頁面包含三個 Web 控制項： 文字方塊、 按鈕和標籤](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**圖 03**: 頁面包含三個 Web 控制項： 文字方塊、 按鈕和標籤 ([按一下以檢視完整大小的影像](control-id-naming-in-content-pages-cs/_static/image5.png))


瀏覽的頁面，透過瀏覽器，然後再檢視的 HTML 原始檔。 為所示，標記`id`文字方塊、 按鈕和標籤 Web 控制項的 HTML 項目的值為組合`ID`Web 控制項的值和`ID`頁面命名容器的值。


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

如稍早在本教學課程中所述，則主版頁面和其 ContentPlaceHolders 將做為命名容器。 因此，同時提供呈現`ID`其巢狀控制項的值。 取得文字方塊的`id`屬性，例如： `ctl00_MainContent_Age`。 請記得，文字方塊控制項`ID`值`Age`。 這加上其 ContentPlaceHolder 控制項的`ID`值`MainContent`。 此外，這個值前面會加上的主版頁面`ID`值`ctl00`。 結果是`id`屬性值包含`ID`主版頁面、 ContentPlaceHolder 控制項和文字方塊本身的值。

圖 4 說明此行為。 若要判斷呈現`id`的`Age`文字方塊中，開頭`ID`值的文字方塊控制項， `Age`。 接下來，執行您在控制項階層的方式。 每個命名容器 （peach 色彩這些節點），前置詞呈現目前`id`命名容器`id`。


![Rendered id 屬性為基礎上識別碼值的命名容器](control-id-naming-in-content-pages-cs/_static/image6.png)

**圖 04**: 轉譯`id`屬性都是根據`ID`命名容器的值


> [!NOTE]
> 如我們所討論，`ctl00`呈現的部分`id`屬性構成`ID`值的主版頁面中，但您可能想知道此`ID`值產生了。 我們並未指定其任何地方我們 master 或內容頁面中。 透過網頁的宣告式標記明確新增 ASP.NET 網頁中的大部分伺服器控制項。 `MainContent` ContentPlaceHolder 控制項的標記中明確指定`Site.master`;`Age`文字方塊中已定義`IDIssues.aspx`的標記。 我們可以指定`ID`這些類型的控制項，透過 [屬性] 視窗，或從宣告式語法的值。 其他控制項，例如主版頁面本身，宣告式標記中未定義。 因此，其`ID`值必須為我們自動產生。 ASP.NET 引擎集`ID`一道尚未明確設定這些控制項的執行階段值。 它會使用命名模式`ctlXX`，其中*XX*是循序遞增的整數值。


主版頁面本身可做為命名的容器，因為定義中的主版頁面的 Web 控制項也已更改轉譯`id`屬性值。 例如，`DisplayDate`我們加入主版頁面中的標籤[*使用主版頁面建立全站台的配置*](creating-a-site-wide-layout-using-master-pages-cs.md)教學課程包含下列呈現標記：


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

請注意，`id`屬性包含這兩個主版頁面的`ID`值 (`ctl00`) 和`ID`標籤 Web 控制項的值 (`DateDisplay`)。

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>步驟 3： 以程式設計方式參考透過 Web 控制項`FindControl`

每個 ASP.NET 伺服器控制項包括`FindControl("controlID")`方法，會搜尋控制項，名為的控制項的下階*controlID*。 如果找到這類控制項，它會傳回。如果找不到任何相符的控制項，`FindControl`傳回`null`。

`FindControl` 當您需要存取控制項，但您不需要直接參考它的案例中很有用。 GridView 的欄位內的控制項時使用的 Web 控制項，例如 GridView，比方說，資料會一次定義在宣告式語法中，但在執行階段建立控制項的執行個體的每個 GridView 資料列。 因此，在執行階段產生的控制項，但我們並沒有可從程式碼後置類別的直接參考。 因此我們需要使用`FindControl`以程式設計的方式與 GridView 的欄位內特定的控制項。 (如需有關使用`FindControl`若要存取資料 Web 控制項範本中的控制項，請參閱[自訂格式化時資料](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md)。)以動態方式將 Web 控制項加入至 Web 表單時，就會發生這種狀況下，主題中討論[建立動態資料輸入使用者介面](https://msdn.microsoft.com/library/aa479330.aspx)。

為了說明使用`FindControl`方法來搜尋控制項在內容頁面，建立事件處理常式`SubmitButton`的`Click`事件。 事件處理常式中加入下列程式碼，以程式設計方式參考`Age`文字方塊和`Results`標籤使用`FindControl`方法，然後顯示中的訊息`Results`根據使用者輸入。

> [!NOTE]
> 當然，我們不需要使用`FindControl`以參考此範例中的標籤和文字方塊控制項。 我們無法參考它們直接透過其`ID`屬性值。 我使用`FindControl`這裡將說明使用時，會發生什麼事`FindControl`從內容頁面。


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

雖然用來呼叫的語法`FindControl`方法有些許不同的前兩個程式行`SubmitButton_Click`，這些語意相等。 提醒您，所有 ASP.NET 伺服器控制項都包括`FindControl`方法。 這包括`Page`從所有 ASP.NET 程式碼後置類別必須衍生自的類別。 因此，呼叫`FindControl("controlID")`就相當於呼叫`Page.FindControl("controlID")`，假設您尚未覆寫`FindControl`方法程式碼後置類別中，或自訂的基底類別中。

輸入此程式碼之後, 請瀏覽`IDIssues.aspx`透過瀏覽器頁面上，輸入您的年齡，然後按一下"Submit"按鈕。 按一下 [送出] 按鈕後`NullReferenceException`，就會引發 （請參閱圖 5）。


[![NullReferenceException，就會引發](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**圖 05**: A `NullReferenceException` ，就會引發 ([按一下以檢視完整大小的影像](control-id-naming-in-content-pages-cs/_static/image9.png))


如果您在中設定中斷點`SubmitButton_Click`事件處理常式，您會看到 同時呼叫至`FindControl`傳回`null`值。 `NullReferenceException`我們嘗試存取時，會引發`Age`文字方塊的`Text`屬性。

問題在於`Control.FindControl`只會搜尋*控制項*的下階的*在相同的命名容器*。 因為主版頁面會構成新的命名容器，呼叫`Page.FindControl("controlID")`從未 permeates 主版頁面物件`ctl00`。 (若要檢視在控制項階層架構，會顯示圖 4 會回頭`Page`物件做為主版頁面之物件的父系`ctl00`。)因此，`Results`標籤和`Age`找不到文字方塊中，`ResultsLabel`和`AgeTextBox`指派的值`null`。

有兩種解決方法，這個挑戰： 我們可以向下鑽研，一個命名容器一次，以適當的控制項。或者我們可以建立自己`FindControl`permeates 命名容器的方法。 讓我們來檢查每個選項。

### <a name="drilling-into-the-appropriate-naming-container"></a>鑽研的適當命名的容器

若要使用`FindControl`參考`Results`標籤或`Age`文字方塊中，我們需要呼叫`FindControl`從相同的命名容器中的上階控制項。 如圖 4 顯示， `MainContent` ContentPlaceHolder 控制項是唯一的上階的`Results`或`Age`，是在相同的命名容器中。 換句話說，呼叫`FindControl`方法從`MainContent`控制項，如下列程式碼片段中所示正確將參考傳回給`Results`或`Age`控制項。


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

不過，我們無法與處理`MainContent`ContentPlaceHolder 從我們的內容頁面的程式碼後置類別使用上述語法，因為 ContentPlaceHolder 主版頁面中定義。 相反地，我們必須使用`FindControl`取得參考`MainContent`。 中的程式碼取代`SubmitButton_Click`事件處理常式和下列修改：


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

如果您瀏覽透過瀏覽器頁面，輸入您的年齡，然後按一下"Submit"按鈕， `NullReferenceException` ，就會引發。 如果您在中設定中斷點`SubmitButton_Click`即會嘗試呼叫時，會發生此例外狀況的事件處理常式`MainContent`物件的`FindControl`方法。 `MainContent`物件是`null`因為`FindControl`方法找不到名為"MainContent"的物件。 根本原因是與使用相同`Results`標籤和`Age`TextBox 控制項：`FindControl`從控制項階層架構的頂端開始其搜尋，並不滲入命名的容器，但`MainContent`ContentPlaceHolder 是主版頁面內，也就是命名容器。

我們可以使用之前`FindControl`取得參考`MainContent`，我們必須先主版頁面控制項的參考。 一旦主版頁面的參考，我們可以得到的參考`MainContent`透過 ContentPlaceHolder`FindControl`並從該處，參考`Results`標籤和`Age`文字方塊中 (同樣地，透過使用`FindControl`)。 但我們該如何取得主版頁面的參考嗎？ 藉由檢查`id`很明顯地呈現標記中的屬性，主版頁面的`ID`值是`ctl00`。 因此，我們可以使用`Page.FindControl("ctl00")`取得主版頁面的參考，然後使用該物件取得的參考`MainContent`，依此類推。 下列程式碼片段將說明這類邏輯：


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

雖然這段程式碼確實運作，它會假設主版頁面的自動產生`ID`一定會`ctl00`。 絕不會是不錯的主意，以便自動產生值的相關假設。

幸運的是，則可透過存取主版頁面的參考`Page`類別的`Master`屬性。 因此，而不需使用`FindControl("ctl00")`取得主版頁面的參考，才能存取`MainContent`ContentPlaceHolder，我們可以改用`Page.Master.FindControl("MainContent")`。 更新`SubmitButton_Click`事件處理常式，以下列程式碼：


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

此時，請瀏覽網頁瀏覽器，透過輸入您的年齡，並按一下"Submit"按鈕會顯示訊息中的`Results`加上標籤，如預期般。


[![在標籤中顯示使用者的年齡](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**圖 06**： 在標籤中顯示使用者的年齡 ([按一下以檢視完整大小的影像](control-id-naming-in-content-pages-cs/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>以遞迴方式搜尋整個命名容器

參考上述程式碼範例的原因`MainContent`ContentPlaceHolder 控制項從主版頁面上，然後`Results`標籤和`Age`TextBox 控制項從`MainContent`，是因為`Control.FindControl`方法只會搜尋內*控制項*的命名容器。 具有`FindControl`命名的容器內保持合理，在大部分情況下因為兩個不同的命名容器中的兩個控制項可能會有相同`ID`值。 請考慮的 GridView 會定義名為標籤 Web 控制項的大小寫`ProductName`其 TemplateFields 的其中一個內。 當資料繫結至 GridView，在執行階段，`ProductName`建立每個 GridView 資料列的標籤。 如果`FindControl`搜尋透過設定命名的所有容器和我們稱為`Page.FindControl("ProductName")`，哪些標籤執行個體應該`FindControl`傳回？ `ProductName`中第一個 GridView 資料列標籤嗎？ 最後一個資料列中的一項嗎？

如此`Control.FindControl`只搜尋*控制項*的命名容器的意義在大部分情況下。 但有其他情況下，例如對向我們，我們有一個唯一`ID`所有命名容器，而且想要避免盡責參考每個存取控制項的控制項階層架構中的命名容器。 具有`FindControl`命名的所有容器都會以遞迴方式搜尋有意義，太的 variant。 不幸的是，.NET Framework 不包含這種方法。

好消息是我們可以建立自己`FindControl`方法，以遞迴方式搜尋所有的命名容器。 事實上，使用*擴充方法*我們可以再加上`FindControlRecursive`方法`Control`伴隨著其現有的類別`FindControl`方法。

> [!NOTE]
> 擴充方法是以 C# 3.0 和 Visual Basic 9，這些語言隨附的.NET Framework 3.5 版和 Visual Studio 2008 的新的功能。 簡單地說，擴充方法可讓開發人員建立新的特殊語法透過現有的類別類型的方法。 如需有關此有幫助功能的詳細資訊，請參閱我的文件：[擴充基底型別功能的擴充方法與](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)。


若要建立擴充方法，加入新的檔案`App_Code`資料夾名為`PageExtensionMethods.cs`。 Add 擴充方法，名為`FindControlRecursive`會做為輸入`string`參數名為`controlID`。 擴充方法，才能正常運作，是重要本身的類別和其擴充方法來標記`static`。 此外，所有擴充方法必須都接受第一個物件的擴充方法套用的類型參數，此輸入的參數必須在前面加上關鍵字`this`。

將下列程式碼加入`PageExtensionMethods.cs`定義此類別的類別檔案和`FindControlRecursive`擴充方法：


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

與這個程式碼的位置中，返回`IDIssues.aspx`頁面的程式碼後置類別和註解目前`FindControl`方法呼叫。 它們取代成呼叫`Page.FindControlRecursive("controlID")`。 何謂棒的擴充方法是會直接在 IntelliSense 下拉式清單中。 如圖 7 所示，當您輸入頁面並再叫用期間，`FindControlRecursive`方法包含 IntelliSense 以及其他的下拉式清單中`Control`類別方法。


[![擴充方法會包含在 IntelliSense 下拉式清單](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**圖 07**： 擴充方法會包含在 IntelliSense 下拉式清單 ([按一下以檢視完整大小的影像](control-id-naming-in-content-pages-cs/_static/image15.png))


輸入下列程式碼`SubmitButton_Click`事件處理常式，然後瀏覽頁面、 輸入您的年齡，並按一下 送出 」 按鈕。 圖 6 中所示，所產生的輸出會是訊息、 「"您已經年齡歲 ！


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> 因為擴充方法不熟悉 C# 3.0 和 Visual Basic 9，如果您使用 Visual Studio 2005，您無法使用擴充方法。 相反地，您必須實作`FindControlRecursive`協助程式類別中的方法。 [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx)他的部落格文章，具有這類範例[微波的 ASP.NET 網頁和`FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx)。


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>步驟 4： 使用正確`id`屬性在用戶端指令碼中的值

本教學課程簡介所述，Web 控制項的呈現`id`屬性有時候用戶端指令碼中用來以程式設計方式參考特定的 HTML 項目。 例如，下列 JavaScript 參考 HTML 項目由其`id`然後強制回應的訊息方塊中顯示其值：


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

提醒您，在 ASP.NET 網頁，並包含命名容器，將呈現之 HTML 項`id`屬性等同於 Web 控制項`ID`屬性值。 因此，很容易在硬`id`至 JavaScript 程式碼的屬性值。 也就是說，如果您知道您想要存取`Age`TextBox Web 控制項，透過用戶端指令碼，這樣做，透過對呼叫`document.getElementById("Age")`。

這個方法的問題是，當使用主版頁面 （或其他命名的容器控制項），轉譯的 HTML`id`而不是與 Web 控制項`ID`屬性。 瀏覽的頁面，透過瀏覽器並檢視來判斷實際的來源可能是您的首要`id`屬性。 一旦您知道轉譯`id`值，您可以貼上的呼叫`getElementById`來存取您需要使用透過用戶端指令碼的 HTML 項目。 這種方法是不盡理想，因為控制項階層架構中頁面的特定變更或變更加入到`ID`命名控制項的內容會改變產生`id`屬性，因而中斷您的 JavaScript 程式碼。

好消息是`id`呈現的屬性值是伺服器端程式碼，透過 Web 控制項的可存取[`ClientID`屬性](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx)。 您應該使用這個屬性來判斷`id`屬性值，用戶端指令碼中使用。 例如，若要加入頁面中的 JavaScript 函式，呼叫時，會顯示值`Age`文字方塊中，在強制回應的訊息方塊中，加入下列程式碼加入`Page_Load`事件處理常式：


[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

上述程式碼會插入的值`Age`的 JavaScript 呼叫的文字方塊中的 ClientID 屬性`getElementById`。 如果您造訪此頁透過瀏覽器，並檢視的 HTML 原始檔，您會發現下列 JavaScript 程式碼：


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

請注意如何正確`id`屬性值、 `ctl00_MainContent_Age`，會出現在呼叫`getElementById`。 這個值會計算在執行階段，因為它的運作方式不論頁面控制項階層架構較晚的變更。

> [!NOTE]
> 這個 JavaScript 範例只示範如何加入 JavaScript 函式的正確參考伺服器控制項所呈現的 HTML 項目。 若要使用此函式，您必須撰寫額外的 JavaScript，或某些特定的使用者動作瓿文件載入時呼叫的函數。 如需詳細資訊，這些及相關的主題，讀取[使用用戶端指令碼](https://msdn.microsoft.com/library/aa479302.aspx)。


## <a name="summary"></a>總結

特定 ASP.NET 伺服器控制項當做命名容器，這會影響呈現`id`屬性及其子系的控制項的值以及由 canvassed 控制項的範圍`FindControl`方法。 與主版頁面本身的主版頁面和其 ContentPlaceHolder 控制項命名容器。 因此，我們必須等一些工作來以程式設計方式參考中的內容頁面使用控制項`FindControl`。 在本教學課程中，我們檢查兩種技術： 鑽研到 ContentPlaceHolder 控制項並呼叫其`FindControl`方法; 和復原自己`FindControl`實作該以遞迴方式搜尋所有的命名容器。

除了與參考 Web 控制項導入的命名容器的伺服器端問題，還有用戶端問題。 如果沒有命名容器，Web 控制項的`ID`屬性值和呈現`id`屬性值是在相同的其中一種。 但加上的命名容器，呈現`id`屬性同時包含`ID`Web 控制項和其控制項階層架構的上階中的命名容器的值。 這些命名的問題並非問題，只要您使用 Web 控制項`ClientID`屬性來判斷呈現`id`屬性值，用戶端指令碼中的。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 主版頁面和 `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [建立動態資料的項目使用者介面](https://msdn.microsoft.com/library/aa479330.aspx)
- [擴充基底型別功能與擴充方法](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [如何： 參考 ASP.NET 主版頁面內容](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [來自網頁： 秘訣、 竅門與設陷](http://www.odetocode.com/articles/450.aspx)
- [使用用戶端指令碼](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP/ASP.NET 書籍和 4GuysFromRolla.com 的創辦，目前正在使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 在可到達 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過在他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Zack Jones 和 Suchi Barnerjee。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

> [!div class="step-by-step"]
> [上一頁](urls-in-master-pages-cs.md)
> [下一頁](interacting-with-the-master-page-from-the-content-page-cs.md)
