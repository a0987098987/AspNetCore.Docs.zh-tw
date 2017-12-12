---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: "UI 和瀏覽 |Microsoft 文件"
author: Erikre
description: "此教學課程將告訴您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Form 應用程式的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: 98c96749781f577d9544c80f58404353d40848b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="ui-and-navigation"></a>UI 和導覽
====================
由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 此教學課程將告訴您建置一個使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Form 應用程式的基本概念。 Visual Studio 2013[與 C# 原始程式碼的專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附此教學課程了。


在本教學課程中，您將修改的預設 Web 應用程式，以支援功能 Wingtip Toys store 前端應用程式的 UI。 此外，您會將簡單和資料繫結功能。 本教學課程上一個教學課程 」 建立資料存取層 」 為基礎，並是 Wingtip Toys 教學課程系列的一部分。

## <a name="what-youll-learn"></a>您將學習：

- 如何變更 UI，以支援 Wingtip Toys store 前端應用程式的功能。
- 如何設定要包含頁面導覽的 HTML5 項目。
- 如何建立資料驅動的控制項來瀏覽至特定產品的資料。
- 如何顯示來自資料庫使用 Entity Framework Code First 所建立的資料。

ASP.NET Web Forms 可讓您建立 Web 應用程式的動態內容。 每個 ASP.NET Web 網頁會建立靜態 HTML 網頁 （網頁不包含伺服器架構處理），類似的方式，但 ASP.NET 網頁包括 ASP.NET 辨識，並產生 HTML 網頁執行時處理的額外項目。

與靜態 HTML 頁面 (*.htm*或*.html*檔案)，伺服器具備`Web`讀取檔案，並將其傳送的要求-至瀏覽器。 相反地，當使用者要求的 ASP.NET 網頁 (*.aspx*檔案)，頁面當成程式執行 Web 伺服器上。 當網頁正在執行時，它可以執行任何需要的工作，您的網站，包括計算的值、 讀取或寫入資料庫的詳細資訊，或是呼叫其他程式。 輸出時，網頁，以動態方式產生標記 （例如 HTML 中的項目），並將這個動態的輸出傳送至瀏覽器。

## <a name="modifying-the-ui"></a>修改 UI

您將會繼續這個教學課程系列藉由修改*Default.aspx*頁面。 您將修改用來建立應用程式的預設範本已建立的 UI。 建立任何 Web Form 應用程式時，您將會執行的修改的類型是一般。 您要進行此變更標題、 一些內容，並移除不必要的預設內容。

1. 開啟或切換至*Default.aspx*頁面。
2. 如果頁面隨即出現在**設計**檢視中，切換至**來源**檢視。
3. 在頁面頂端`@Page`指示詞，變更`Title`中下方的黃色反白顯示 「 歡迎 」，此屬性。 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. 同時在*Default.aspx*頁面上，會取代預設的內容中所包含的所有`<asp:Content>`標記，標記會顯示為下面。 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. 儲存*Default.aspx*頁面中的選取**儲存 Default.aspx**從**檔案**功能表。

 產生*Default.aspx*頁面會出現，如下所示： 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

在範例中，您已設定`Title`屬性`@Page`指示詞。 在瀏覽器，伺服端程式碼中顯示 HTML 時`<%: Page.Title %>`中所包含的內容來解析`Title`屬性。

範例頁面包含構成 ASP.NET Web 網頁的基本項目。 此頁面包含靜態文字，因為您可能需要在 HTML 網頁，以及 ASP.NET 特定的項目。 中所包含的內容*Default.aspx*頁面將會與主版頁面內容，這將在稍後在本教學課程說明整合。

### <a name="page-directive"></a>@Page指示詞

ASP.NET Web Form 通常會包含指示詞可讓您指定網頁的網頁內容和組態資訊。 指示詞會使用 ASP.NET 作為指示如何處理序 頁面上，但是不會轉譯傳送到瀏覽器標記的一部分。

最常使用的指示詞`@Page`指示詞，可讓您指定的頁面上，包括許多組態選項：

1. 伺服器程式 頁面上，例如 C# 中的程式碼的語言。
2. 不論是透過伺服器程式碼直接在頁面中，會呼叫單一檔案網頁，或在個別的類別檔案中，也就所謂的程式碼後置頁面的程式碼的頁面。
3. 頁面是否有相關聯的主版頁面，並因此應視為內容頁面。
4. 偵錯和追蹤選項。

如果您未包含`@Page`指示詞在頁面中，或是如果指示詞不包含特定的設定，設定將會繼承自*Web.config*組態檔或從*Machine.config*組態檔。 *Machine.config*檔案提供至所有電腦上執行的應用程式的其他組態設定。

> [!NOTE] 
> 
> *Machine.config*也會提供所有可能的組態設定的相關詳細資料。


### <a name="web-server-controls"></a>Web 伺服器控制項

在大部分的 ASP.NET Web Form 應用程式，您將加入控制項，可讓使用者互動，請在頁面上，例如按鈕、 文字方塊、 清單和等等。 這些 Web 伺服器控制項是類似於 HTML 按鈕與輸入項目。 不過，它們會處理在伺服器上，讓您可以使用伺服端程式碼來設定其屬性。 這些控制項也會引發事件，您可以在伺服器程式碼中處理。

伺服器控制項使用 ASP.NET 辨識網頁執行時的特殊語法。 ASP.NET 伺服器控制項的標記名稱開頭`asp:`前置詞。 這可讓 ASP.NET 識別及處理這些伺服器控制項。 前置詞可能會不同，如果控制項不是.NET Framework 的一部分。 除了`asp:`前置詞，也包含 ASP.NET 伺服器控制項`runat="server"`屬性和`ID`可用來參考伺服器程式碼中的控制項。

頁面執行時，ASP.NET 識別的伺服器控制項，並執行這些控制項相關聯的程式碼。 許多控制項呈現 HTML 或其他標記至網頁瀏覽器中顯示時。

### <a name="server-code"></a>伺服端程式碼

大部分的 ASP.NET Web Form 應用程式會包含在處理網頁時，在伺服器執行的程式碼。 如上面所述，伺服端程式碼可以用於執行各種不同的項目，例如將資料加入至 ListView 控制項。 ASP.NET 支援包括 C#、 Visual Basic、 J# 及其他的伺服器上執行的許多語言。

ASP.NET 支援兩種模型撰寫的 Web 網頁的伺服器程式碼。 在單一檔案模式中，網頁的程式碼是在指令碼項目開頭標記，包含`runat="server"`屬性。 或者，您可以在個別的類別檔案中，稱為 「 程式碼後置模型，建立網頁的程式碼。 在此情況下，ASP.NET Web Form 網頁通常不包含伺服器程式碼。 相反地，`@Page`指示詞包含連結的資訊*.aspx*頁面與它相關聯的程式碼後置檔案。

`CodeBehind`屬性中包含`@Page`指示詞指定的不同類別檔案名稱和`Inherits`屬性會指定對應至頁面的程式碼後置檔案內的類別名稱。

### <a name="updating-the-master-page"></a>更新主版頁面

在 ASP.NET Web Form、 主版頁面可讓您在您的應用程式中建立一致的版面配置頁面。 單一的主版頁面定義應用程式中的外觀和您想要針對所有的頁面 （或頁面群組） 的標準行為。 然後，您可以建立個別的內容頁面包含您想要顯示，以上所述的內容。 當使用者要求內容的網頁時，ASP.NET 會將它們與主要的頁面，即可產生結合的主版頁面配置與內容的頁面內容輸出。

新的站台需要單一商標將顯示每個頁面。 若要加入這個標誌，您可以修改主版頁面的 HTML。

1. 在**方案總管 中**、 尋找和開啟**Site.Master**頁面。
2. 若頁面位於**設計**檢視中，切換至**來源**檢視。
3. 更新由主版頁面**修改或加入**以黃色反白顯示的標記： 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

此 HTML 會顯示名為映像*logo.jpg*從*映像*Web 應用程式，您稍後會加入資料夾。 使用主版頁面的網頁瀏覽器中顯示時，將會顯示標誌。 如果使用者按一下標誌，使用者會瀏覽回到*Default.aspx*頁面。 HTML 錨點標籤`<a>`包裝映像的伺服器控制項，並可讓要包含在連結的映像。 `href`屬性的錨定標記指定的根"`~/`"做為連結位置的網站。 根據預設， *Default.aspx*使用者導覽至網站的根目錄時，會顯示頁面。 **映像**`<asp:Image>`伺服器控制項包括新增屬性，例如`BorderStyle`，可呈現為 HTML 瀏覽器中顯示時。

### <a name="master-pages"></a>主版頁面

主版頁面是副檔名.master ASP.NET 檔案 (例如， *Site.Master*) 可以包含靜態文字、 HTML 項目和伺服器控制項的預先定義版面配置。 主版頁面由特殊`@Master`指示詞，以取代`@Page`用於一般的指示詞*.aspx*頁面。

除了`@Master`指示詞，主版頁面也包含所有最上層的 HTML 項目頁面，例如`html`， `head`，和`form`。 例如，在上述您新增主要頁面上，您使用 HTML`table`配置，請`img`公司標誌、 靜態文字，以及處理常見的成員資格，您站台伺服器控制項的項目。 您可以使用任何 HTML 及 ASP.NET 中的任何項目，做為您的主版頁面的一部分。

除了靜態文字和會出現在所有頁面的控制項、 主版頁面也包含一或多個**ContentPlaceHolder**控制項。 這些預留位置控制項定義可取代內容的顯示位置的區域。 接著，可取代內容定義在內容頁中，例如*Default.aspx*，並使用**內容**伺服器控制項。

#### <a name="adding-image-files"></a>將影像檔

標誌影像上方，參考的所有產品映像，以及必須新增 Web 應用程式，以便在瀏覽器中顯示專案時可以看到它們。

#### <a name="download-from-msdn-samples-site"></a>從 MSDN 範例網站下載：

[開始使用 ASP.NET 4.5 Web Form 和 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

此下載包含中的資源*WingtipToys 資產*用來建立範例應用程式的資料夾。

1. 如果您尚未這樣做，請下載從 MSDN 範例網站在使用上述連結的壓縮的範例檔案。
2. 在下載後，開啟.zip 檔案，並將內容複製到您的電腦上的本機資料夾。
3. 尋找和開啟*WingtipToys 資產*資料夾。
4. 藉由拖放、 複製*目錄*從您的本機資料夾的資料夾中的 Web 應用程式專案的根目錄**方案總管 中**的 Visual Studio。 

    ![UI 和瀏覽-檔案複製](ui_and_navigation/_static/image1.png)
5. 接下來，建立新的資料夾，名為*映像*以滑鼠右鍵按一下**WingtipToys**專案中**方案總管 中**，然後選取**新增** - &gt; **新資料夾**。
6. 複製*logo.jpg*檔案從*WingtipToys 資產*資料夾中的**檔案總管**至*映像*Web 應用程式的資料夾專案中**方案總管 中**的 Visual Studio。
7. 按一下**顯示所有檔案**選項上方的**方案總管 中**更新的檔案清單，如果您沒有看到新的檔案。  
  
    **方案總管 中**現在會顯示更新的專案檔案。 

    ![UI 和瀏覽-方案總管](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>新增頁

然後再加入瀏覽至 Web 應用程式，您要先加入兩個新的頁面，您會瀏覽至。 稍後在本教學課程系列，您會在這些新的頁面上顯示產品和產品詳細資料。

1. 在**方案總管 中**，以滑鼠右鍵按一下**WingtipToys**，按一下 **新增**，然後按一下 **新項目**。   
 隨即顯示 [ 新增項目] 對話方塊。
2. 選取**Visual C#**  - &gt; **Web**左側的 [範本] 群組。 然後，選取**使用主版頁面的 Web Form**中間清單並將其命名*ProductList.aspx*。 

    ![UI 和瀏覽-加入新項目 對話方塊](ui_and_navigation/_static/image3.png)
3. 選取**Site.Master**附加到新建立的主版頁面*.aspx*頁面。 

    ![UI 和瀏覽-選取主版頁面](ui_and_navigation/_static/image4.png)
4. 加入額外的頁面，名為*ProductDetails.aspx*遵循這些相同的步驟。

### <a name="updating-bootstrap"></a>更新啟動程序

Visual Studio 2013 專案範本使用[Bootstrap](http://getbootstrap.com/)，Twitter 所建立的配置和佈景主題的架構。 啟動程序會使用 CSS3 提供回應式設計，這表示配置可以動態的方式適應不同的瀏覽器視窗大小。 您也可以使用啟動程序的主題設定功能，以輕鬆地促使應用程式的外觀及操作中的變更。 根據預設，Visual Studio 2013 中的 ASP.NET Web 應用程式範本會以 NuGet 套件包含啟動程序。

在本教學課程中，您會藉由取代啟動載入器的 CSS 檔案變更 Wingtip Toys 應用程式的外觀與風格。

1. 在**方案總管 中**，開啟*內容*資料夾。
2. 以滑鼠右鍵按一下*bootstrap.css*檔案，並在它重新命名為*bootstrap original.css*。
3. 重新命名*bootstrap.min.css*至*bootstrap original.min.css*。
4. 在**方案總管 中**，以滑鼠右鍵按一下*內容*資料夾，然後選取**在檔案總管 中開啟資料夾**。  
 [檔案總管] 隨即出現。 您會將下載的啟動程序 CSS 檔案儲存至這個位置。
5. 在您的瀏覽器，移至[http://Bootswatch.com](http://bootswatch.com/)。
6. 捲動瀏覽器視窗中，直到您看到 Cerulean 佈景主題。 

    ![UI 和瀏覽-Cerulean 佈景主題](ui_and_navigation/_static/image5.png)
7. 下載兩者*bootstrap.css*檔案和*bootstrap.min.css*檔案*內容*資料夾。 使用路徑中所顯示的內容資料夾**檔案總管**先前開啟的視窗。
8. 在**Visual Studio**頂端**方案總管 中**，選取**顯示所有檔案**選項，以顯示新的檔案內容資料夾中。 

    ![UI 和瀏覽-方案總管](ui_and_navigation/_static/image6.png)

 您會看到兩個新的 CSS 檔案中**內容**資料夾，但請注意，每個檔案名稱旁邊的圖示會變成灰色。這表示，該檔案尚未尚未加入至專案。
9. 以滑鼠右鍵按一下*bootstrap.css*和*bootstrap.min.css*檔案，然後選取**包含在專案**。   
 當您稍後在本教學課程執行 Wingtip Toys 應用程式時，將會顯示新的使用者介面。

> [!NOTE] 
> 
> ASP.NET Web 應用程式範本使用*Bundle.config* Bootstrap CSS 檔案的路徑儲存至專案根目錄的檔案。


### <a name="modifying-the-default-navigation"></a>修改預設的巡覽

變更未排序的瀏覽清單項目。 也可以修改預設的巡覽應用程式中的每個分頁*Site.Master*頁面。

1. 在**方案總管 中**，找出並開啟*Site.Master*頁面。
2. 新增以未排序的清單，如下所示的黃色反白顯示的其他導覽連結：   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

您可以看到上述的 HTML 中，您必須修改每個明細項目`<li>`包含錨定標記`<a>`連結`href`屬性。 每個`href`指向 Web 應用程式中的頁面。 當使用者按一下這些連結的其中一個瀏覽器中 (例如**產品**)，它們會瀏覽至網頁中包含`href`(例如**ProductList.aspx**)。 在本教學課程結尾處，您將執行應用程式。

> [!NOTE] 
> 
> 波狀符號 (`~`) 字元用來指定`href`路徑根目錄中的專案開始。


### <a name="adding-a-data-control-to-display-navigation-data"></a>加入資料控制項以顯示 瀏覽資料

接下來，您要加入控制項以顯示所有資料庫中的類別。 每個類別目錄做為連結*ProductList.aspx*頁面。 當使用者按一下瀏覽器中的類別連結時，他們瀏覽至 [產品] 頁面，請參閱只與所選類別目錄相關聯的產品。

您將使用**ListView**控制項來顯示資料庫中包含的所有類別目錄。 若要加入**ListView**主版頁面的控制項：

1. 在*Site.Master*頁面上，加入下列反白顯示`<div>`元素**之後**`<div>`項目包含`id="TitleContent"`您先前加入的：  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

此程式碼會顯示資料庫中的所有類別目錄。 **ListView**控制項做為連結文字顯示每個分類名稱和包含的連結*ProductList.aspx*頁面的查詢字串值包含`ID`類別目錄。 藉由設定`ItemType`屬性**ListView**控制項，資料繫結運算式`Item`內`ItemTemplate`節點和控制項變成強型別。 您可以選取的詳細資料`Item`物件使用 IntelliSense，例如指定`CategoryName`。 此程式碼會包含容器內`<%#: %>`符號資料繫結運算式。 藉由新增 （:） 的結尾`<%#`前置詞，資料繫結運算式的結果是 HTML 編碼。 結果是 HTML 編碼，您的應用程式進一步保護可避免跨網站指令碼資料隱碼 (XSS) 和 HTML 資料隱碼攻擊。

> [!NOTE] 
> 
> **秘訣**
> 
> 當您在開發期間輸入加入程式碼時，您就可以確定物件的有效成員找到，因為強型別資料控制項顯示可用 IntelliSense 所根據的成員。 IntelliSense 提供了適當的內容程式碼選項，當您輸入程式碼，例如屬性、 方法和物件。


在下一個步驟中，您將實作`GetCategories`方法來擷取資料。

### <a name="linking-the-data-control-to-the-database"></a>將資料控制項連結至資料庫

您可以在資料控制項中顯示資料之前，您需要連線到資料庫的資料控制項。 若要進行連結，您可以修改的程式碼後置*Site.Master.cs*檔案。

1. 在**方案總管 中**，以滑鼠右鍵按一下*Site.Master*頁面，然後按一下**檢視程式碼**。 *Site.Master.cs*在編輯器中開啟檔案。
2. 接近開頭*Site.Master.cs*檔案中加入兩個額外的命名空間，使所有包含的命名空間出現，如下所示：  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. 將反白顯示`GetCategories`方法之後`Page_Load`事件處理常式，如下所示：  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

上述程式碼會執行任何會使用主版頁面的頁面載入瀏覽器中。 `ListView`您稍早在本教學課程中加入的控制項 (具名"categoryList 」) 會使用模型繫結選取的資料。 在標記中`ListView`將控制項的控制項`SelectMethod`屬性`GetCategories`上面顯示的方法。 `ListView`控制呼叫`GetCategories`方法頁面生命週期中的適當時間循環，並自動繫結傳回的資料。 您將進一步了解下一個教學課程中的資料繫結。

### <a name="running-the-application-and-creating-the-database"></a>執行應用程式和建立資料庫

稍早在本教學課程系列方法，您可以建立 （名為"ProductDatabaseInitializer"） 的初始設定式類別，並指定此類別中的*global.asax.cs*檔案。 當應用程式會執行第一次，因為 Entity Framework 將會產生資料庫`Application_Start`方法，包含在*global.asax.cs*檔將呼叫的初始設定式類別。 初始設定式類別會使用模型類別 (`Category`和`Product`) 您稍早在本教學課程的序列，以建立資料庫中加入。

1. 在**方案總管 中**，以滑鼠右鍵按一下*Default.aspx*頁面，然後選取**設定為起始頁**。
2. 在 Visual Studio **F5**。   
 需要一段時間才能在第一次執行此設定的所有項目。   
    ![UI 和瀏覽-瀏覽器視窗](ui_and_navigation/_static/image7.png)  
 當您執行應用程式時，將編譯的應用程式和資料庫名為*wingtiptoys.mdf*將建立在*應用程式\_資料*資料夾。 在瀏覽器中，您會看到類別目錄瀏覽功能表。 這個功能表是由從資料庫擷取的類別產生。 在下一個教學課程中，您將實作瀏覽。
3. 關閉瀏覽器以停止執行的應用程式。

### <a name="reviewing-the-database"></a>檢閱資料庫

開啟*Web.config*檔案，並查看連接字串部分。 您可以看到`AttachDbFilename`連接字串中的值指向`DataDirectory`Web 應用程式專案。 值`|DataDirectory|`是保留的值，代表*應用程式\_資料*專案資料夾中的。 此資料夾是由實體類別所建立的資料庫所在的位置。

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> 如果*應用程式\_資料*看不到資料夾或資料夾是空的如果選取**重新整理**圖示，然後**顯示所有檔案**上方圖示**方案總管 中**視窗。 展開的寬度**方案總管 中**windows 可能需要以顯示所有可用的圖示。


現在您可以檢查所包含的資料*wingtiptoys.mdf*資料庫檔案使用**伺服器總管**視窗。

1. 展開*應用程式\_資料*資料夾。 如果*應用程式\_資料*看不到資料夾，請參閱上述的注意事項。
2. 如果*wingtiptoys.mdf*資料庫檔案是不可見，並選取**重新整理**圖示，然後**顯示所有檔案**上方的圖示**方案總管**視窗。
3. 以滑鼠右鍵按一下*wingtiptoys.mdf*資料庫檔案，然後選取**開啟**。  
    **伺服器總管**隨即出現。 

    ![UI 和瀏覽-伺服器總管](ui_and_navigation/_static/image8.png)
4. 展開*資料表*資料夾。
5. 以滑鼠右鍵按一下**產品**資料表，然後選取**顯示資料表資料**。  
 **產品**資料表就會顯示。 

    ![UI 和瀏覽-Products 資料表](ui_and_navigation/_static/image9.png)
6. 此檢視可讓您檢視及修改中的資料**產品**資料表以手動方式。
7. 關閉**產品**資料表視窗中。
8. 在**伺服器總管**，以滑鼠右鍵按一下**產品**資料表一次，然後選取**開啟資料表定義**。  
 資料設計**產品**資料表就會顯示。 

    ![UI 和瀏覽-產品設計](ui_and_navigation/_static/image10.png)
9. 在**T-SQL**  索引標籤，您會看到用來建立資料表的 SQL DDL 陳述式。 您也可以使用中的 UI**設計**索引標籤來修改結構描述。
10. 在**伺服器總管**，以滑鼠右鍵按一下**WingtipToys**資料庫，並選取**關閉連接**。   
 卸離資料庫。 請從 Visual Studio，資料庫結構描述將能夠修改稍後在本教學課程系列。
11. 返回**方案總管 中**選取**方案總管 中** 索引標籤底部**伺服器總管**視窗。

## <a name="summary"></a>總結

您已經在本教學課程中的數列加入一些基本的 UI、 圖形、 頁面和導覽。 此外，您要執行 Web 應用程式，從您在上一個教學課程中加入的資料類別建立資料庫。 您也可以檢視的內容*產品*藉由直接檢視資料庫的資料庫資料表。 在下一個教學課程中，您要顯示資料的項目以及從資料庫的詳細資料。

## <a name="additional-resources"></a>其他資源

[介紹程式設計 ASP.NET Web Pages](https://msdn.microsoft.com/en-us/library/ms178125.aspx)   
[ASP.NET Web 伺服器控制項概觀](https://msdn.microsoft.com/en-us/library/zsyt68f1.aspx)   
[CSS 教學課程](http://www.w3schools.com/css/default.asp)

>[!div class="step-by-step"]
[上一頁](create_the_data_access_layer.md)
[下一頁](display_data_items_and_details.md)
