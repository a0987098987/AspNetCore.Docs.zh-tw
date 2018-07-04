---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: 建立基本的 ASP.NET 4.5 Web Form 頁面在 Visual Studio 2013 |Microsoft Docs
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: c2051b166b8800654a107c5100ad5ed94af93407
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394449"
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>建立基本的 ASP.NET 4.5 Web Form 頁面在 Visual Studio 2013
====================
藉由[Erik Reitan](https://github.com/Erikre)

本逐步解說會將您提供的簡介中的 Web 開發環境[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)然後在[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。 此逐步解說會引導您完成建立簡單的 ASP.NET Web Form 網頁，並說明如何建立新的頁面、 加入控制項，以及撰寫程式碼的基本技巧。

這個逐步解說中所述的工作包括：

- 建立檔案系統 Web Form 應用程式專案。
- 您熟悉 Visual Studio。
- 建立 ASP.NET 網頁。
- 加入控制項。
- 加入事件處理常式。
- 執行和測試 Visual Studio 的頁面。

## <a name="prerequisites"></a>必要條件


若要完成這個逐步解說，您將需要：

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)或是[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。 .NET Framework 會自動安裝。 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 和 Microsoft Visual Studio Express 2013 for Web 將通常稱為 Visual Studio 在此教學課程系列。  
    >   
    > 如果您使用 Visual Studio，本逐步解說假設您已經選擇**Web 開發**設定集合，第一次您啟動 Visual Studio。 如需詳細資訊，請參閱[如何： 選取 Web 程式開發環境設定](https://msdn.microsoft.com/library/ff521558.aspx)。


## <a name="creating-a-web-application-project-and-a-page"></a>建立 Web 應用程式專案和頁面

<a id="sectionToggle0"></a>

在本逐步解說的這個部分，您會建立 Web 應用程式專案，並加入新的頁面。 您也會新增 HTML 文字，並在您的瀏覽器中執行的頁面。

### <a name="to-create-a-web-application-project"></a>若要建立的 Web 應用程式專案

1. 開啟 Microsoft Visual Studio。
2. 在 [檔案] 功能表上，選取 [新增專案]。  
    ![[檔案] 功能表](creating-a-basic-web-forms-page/_static/image1.png)

    [ **新增專案** ] 對話方塊隨即出現。
3. 選取 **範本** - &gt; **Visual C#**  - &gt; **Web**左側的 範本 群組。
4. 選擇**ASP.NET Web 應用程式**中間欄的範本。
5. 命名您的專案***BasicWebApp***然後按一下**確定** 按鈕。   
![[新增專案] 對話方塊](creating-a-basic-web-forms-page/_static/image2.png)
6. 接下來，選取**Web Form**範本，然後按一下**確定**按鈕，以建立專案。  
![[新增 ASP.NET 專案] 對話方塊](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio 會建立新的專案，其中包含預先建置的 Web Form 範本為基礎的功能。 它不僅提供您*Home.aspx*頁面上， *about.aspx 的網頁*頁面上， *Contact.aspx*頁面上，但也包含 註冊使用者，並將儲存的成員資格功能他們的認證，讓他們可以登入您的網站。 建立新的頁面時，預設 Visual Studio 會顯示在頁面**來源**檢視中，您可以在其中看到網頁的 HTML 項目。 下圖顯示您在想看到的內容**來源**如果您建立名為新的 Web 網頁檢視*BasicWebApp.aspx*。  
    ![原始碼檢視](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>教學課程的 Visual Studio Web 開發環境


藉由修改網頁在繼續之前，最好熟悉 Visual Studio 開發環境。 下圖顯示您在 windows 和 Visual Studio 和 Visual Studio Express for Web 中可用的工具。

> [!NOTE] 
> 
> 下圖顯示預設視窗及視窗位置。 **檢視**功能表可讓您顯示其他視窗，以及重新排列和調整大小以符合您的喜好設定的 windows。 如果變更已經進行了視窗排列，您看到的內容將不會符合圖。


 Visual Studio 環境

![Visual Studio 環境](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>熟悉 Web 設計工具

檢查上面的圖例，且符合下列清單中，所要的文字視窗和工具，其中描述最常使用。 （並非所有的視窗和工具，您會看到此處列出，只標示在上圖中。）

- 工具列。 提供命令的格式化文字，尋找文字，以及等等。 某些工具列是可用的只有當您使用時，才**設計**檢視。
- **方案總管 中**視窗。 Web 應用程式中顯示的檔案和資料夾。
- 文件視窗。 顯示您正在使用中索引標籤式視窗的文件。 您可以透過按一下索引標籤的文件之間切換。
- **屬性**視窗。 可讓您變更設定 頁面、 HTML 項目、 控制項和其他物件。
- 檢視索引標籤。 為您提供相同的文件的不同檢視。 **設計**檢視是 WYSIWYG 編輯的介面。 **來源**檢視是頁面的 HTML 編輯器。 **分割**檢視會同時顯示兩者**設計**檢視和**來源**文件的檢視。 您將使用**設計**並**來源**稍後在本逐步解說中的檢視。 如果您想要開啟中的網頁**設計**檢視，請在**工具**功能表上，按一下 **選項**，選取**HTML 設計工具**節點，然後變更**頁面起始位置**選項。
- **工具箱**。 提供控制項和您可以將它拖曳到頁面的 HTML 項目。 **工具箱**項目會依常見的函式。
- S **erver 總管**。 顯示資料庫連接。 如果看不到 [伺服器總管] 中，請在 [檢視] 功能表中，按一下 [伺服器總管]。


### <a name="creating-a-new-aspnet-web-forms-page"></a>建立新的 ASP.NET Web Form 頁面


當您建立新的 Web Form 應用程式使用**ASP.NET Web 應用程式**專案範本，Visual Studio 會加入名為 ASP.NET 網頁 （Web Form 頁面） *Default.aspx*，以及數個其他檔案和資料夾。 您可以使用*Default.aspx*與 Web 應用程式的 [首頁] 頁面的頁面。 不過，對於此逐步解說中，您會建立並使用新的頁面。

### <a name="to-add-a-page-to-the-web-application"></a>將頁面新增至 Web 應用程式


1. 關閉*Default.aspx*頁面。 若要這樣做，請按一下顯示的檔案名稱的索引標籤，然後按一下 [關閉] 選項。
2. 在**方案總管**，以滑鼠右鍵按一下 Web 應用程式名稱 (在應用程式名稱是本教學課程**BasicWebSite**)，然後按一下 **新增** - &gt;**新的項目**。   
隨即顯示 [ 新增項目] 對話方塊。
3. 選取  **Visual C#**  - &gt; **Web**左側的 範本 群組。 然後，選取**Web Form**從中間清單並將它命名*FirstWebPage.aspx*。   
    ![加入新項目對話方塊](creating-a-basic-web-forms-page/_static/image6.png)
4. 按一下 **新增**將網頁新增至您的專案。  
Visual Studio 會建立新的頁面，並開啟它。


### <a name="adding-html-to-the-page"></a>將 HTML 加入至頁面


在這個部分的逐步解說中，您將一些靜態文字頁面。

### <a name="to-add-text-to-the-page"></a>若要將文字加入至頁面


1. 在文件視窗的底部，按一下**設計**索引標籤切換至**設計**檢視。

    設計檢視會顯示目前的頁面，以類似於 WYSIWYG 的方式。 此時，您沒有任何文字或控制項的頁面上，因此網頁為空白，但是概述矩形的虛線。 代表這個矩形**div**頁面上的項目。
2. 按一下以虛線框起來的矩形內部。
3. 型別**歡迎使用 Visual Web Developer**然後按**ENTER**兩次。

    下圖顯示在您輸入的文字**設計**檢視。

    ![在 [設計] 檢視中的歡迎文字](creating-a-basic-web-forms-page/_static/image7.png "設計 檢視中的歡迎文字")
4. 若要切換**來源**檢視。

    您可以看到在 HTML**來源**您在輸入時所建立的檢視**設計**檢視。  
    ![包含靜態文字的網頁](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>執行網頁


您將控制項加入至頁面繼續進行之前，您可以先執行。

### <a name="to-run-the-page"></a>若要執行的頁面


1. 在 [**方案總管] 中**，以滑鼠右鍵按一下*FirstWebPage.aspx* ，然後選取**設定為起始頁**。
2. 按下**CTRL + F5**執行頁面。

    頁面會顯示在瀏覽器。 雖然您建立的頁面具有的檔案名稱副檔名 *.aspx*，目前執行像是任何 HTML 頁面。

    在瀏覽器中顯示的頁面您可以也以滑鼠右鍵按一下在頁面**方案總管**，然後選取**瀏覽器中的檢視**。
3. 關閉瀏覽器停止 Web 應用程式。


## <a name="adding-and-programming-controls"></a>新增和程式設計控制項


<a id="sectionToggle1"></a>

您現在會加入網頁伺服器控制項。 伺服器控制項，例如按鈕、 標籤、 文字方塊和其他熟悉的控制項，提供您的 Web Form 網頁的一般形式處理功能。 不過，您可以程式設計的控制項，以在伺服器上，而不是用戶端執行的程式碼。

您將會加入[ 按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項[文字方塊中](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控制項，和[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項加入頁面，並撰寫程式碼來處理[按一下](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)事件[ 按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項。

### <a name="to-add-controls-to-the-page"></a>若要將控制項加入頁面


1. 按一下 **設計**索引標籤切換至**設計**檢視。
2. 將插入點放在結尾**歡迎使用 Visual Web Developer**文字，然後按**ENTER**五個以上的時間，以清出一些空間，在**div**項目方塊。
3. 在 **工具箱**，展開**標準**如果尚未展開群組。  
請注意，您可能需要展開**工具箱**視窗左邊來檢視它。
4. 拖曳[TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控制項拖曳至頁面並放置在中間的**div**內含的項目方塊**歡迎使用 Visual Web Developer**第一行中。
5. 拖曳[ 按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項拖曳至頁面，並置放到右邊[TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控制項。
6. 拖曳[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項拖曳至網頁並將它放在以下的一行上[ 按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項。
7. 將上述的插入點放[TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控制項，然後輸入**輸入您的名稱：** 。

    這個靜態的 HTML 文字是標題[TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控制項。 您可以混合靜態 HTML 和相同的頁面上的伺服器控制項。 下圖顯示三個控制項如何出現在**設計**檢視。

    ![在 [設計] 檢視中的三個控制項](creating-a-basic-web-forms-page/_static/image9.png "設計 檢視中的三個控制項")


### <a name="setting-control-properties"></a>設定控制項屬性


Visual Studio 提供各種設定頁面上控制項的屬性。 在這個部分的逐步解說中，您將設定屬性，在這兩**設計**檢視和**來源**檢視。

### <a name="to-set-control-properties"></a>若要設定控制項屬性


1. 第一次，顯示**屬性**視窗中的，選取從**檢視**功能表-&gt; **其他 Windows**  - &gt; **屬性視窗**。 您也可以選取**F4**顯示**屬性**視窗。
2. 選取 [ [] 按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項，然後在**屬性**視窗中，設定的值**文字**至**顯示名稱**。 下圖所示，您所輸入的文字會出現在設計工具中的按鈕。

    ![設定按鈕文字](creating-a-basic-web-forms-page/_static/image10.png "設定按鈕文字")
3. 若要切換**來源**檢視。

    **來源**檢視會顯示的 HTML 頁面上，包括 Visual Studio 建立伺服器控制項的項目。 控制項使用宣告 HTML 的類似語法，不同之處在於標籤使用的前置詞**asp:** ，並包含屬性**runat =&quot;server&quot;**。

    控制項的屬性會宣告為屬性。 例如，當您設定[文字](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx)屬性[ 按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項，在步驟 1 中，您已實際設定**文字**中控制項的標記屬性。

    > [!NOTE] 
    > 
    > 所有控制項都位於**表單**項目，也具有屬性**runat =&quot;server&quot;**。 **Runat =&quot;伺服器&quot;** 屬性和**asp:** 的前置詞控制標籤標記的控制項，以便處理 ASP.NET 伺服器上網頁執行時。 外部程式碼**&lt;m runat =&quot;伺服器&quot;&gt;** 並**&lt;指令碼 runat =&quot;server&quot; &gt;** 項目會傳送至瀏覽器，這就是為什麼 ASP.NET 程式碼必須在其開頭標記中包含的項目內不變**runat =&quot;伺服器&quot;** 屬性。
4. 接下來，您將在其中加入一個額外的屬性，來[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項。 將直接插入點後的放**asp: Label**中**&lt;asp: Label&gt;** 標記，並再按**空格鍵**。

    下拉式清單隨即出現，顯示您可以設定的可用屬性的清單[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項。 這項功能，稱為**IntelliSense**，可協助您在**來源**語法的伺服器控制項、 HTML 項目和其他項目 頁面上的檢視。 如下圖所示**IntelliSense**下拉式清單，如[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項。

    ![IntelliSense 屬性](creating-a-basic-web-forms-page/_static/image11.png "IntelliSense 屬性")
5. 選取 [ **ForeColor** ]，然後輸入等號。

    IntelliSense 會顯示色彩的清單。

    > [!NOTE] 
    > 
    > 您可以顯示**IntelliSense**下拉式清單，隨時按下**CTRL + J**檢視程式碼時。
6. 選取的色彩**[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** 控制項的文字。 請確定您選取濃，讀取在白色背景的色彩。

    **ForeColor**屬性已完成，但您已選取，包括右括號的色彩。


### <a name="programming-the-button-control"></a>程式設計 [按鈕] 控制項


此逐步解說中，您將撰寫程式碼，可讀取的名稱，在使用者輸入到文字方塊中，並接著會顯示在 名稱[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項。

### <a name="add-a-default-button-event-handler"></a>新增的預設按鈕事件處理常式


1. 若要切換**設計**檢視。
2. 按兩下[ 按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項。

    根據預設，Visual Studio 會切換成程式碼後置檔案並建立的基本架構的事件處理常式[ 按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項的預設事件[按一下](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)事件。 程式碼後置檔案會從您的伺服器程式碼 （例如 C#) 分隔您 UI 的標記 （例如 HTML)。   
   資料指標位於此事件處理常式的程式碼。

    > [!NOTE] 
    > 
    > 按兩下中的控制項**設計**檢視只是數種方式，您可以建立事件處理常式。
3. 內部**Button1\_按一下 **事件處理常式中，輸入**Label1**後面接著句號 (**。**)。

    當您輸入超過此時限後的**識別碼**的標籤 (**Label1**)，Visual Studio 會顯示一份可用的成員，如[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項，如下列所示圖。 成員通常是屬性、 方法或事件。

    ![程式碼檢視中的 IntelliSense](creating-a-basic-web-forms-page/_static/image12.png "程式碼檢視中的 IntelliSense")
4. 完成**按一下**事件處理常式，因此它會讀取，如下列程式碼範例所示的按鈕。

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. 切換回 [檢視**來源**檢視您的 HTML 標記，以滑鼠右鍵按一下*FirstWebPage.aspx*中**方案總管] 中**，然後選取**檢視標記**。
6. 若要捲動**&lt;p: Button&gt;** 項目。 請注意， **&lt;p: Button&gt;** 項目現在具有屬性**onclick =&quot;Button1\_按一下&quot;**。

    這個屬性繫結的按鈕[按一下](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)您在上一個步驟中自動程式化的處理常式方法的事件。

    事件處理常式方法可以有任何名稱;您看到的名稱是由 Visual Studio 建立的預設名稱。 重要的一點是，使用名稱**OnClick** HTML 中的屬性必須符合的程式碼後置中定義的方法名稱。


### <a name="running-the-page"></a>執行網頁


您現在可以測試頁面上的伺服器控制項。

### <a name="to-run-the-page"></a>若要執行的頁面


1. 按下**CTRL + F5**執行網頁瀏覽器中。 如果發生錯誤，重新檢查上述的步驟。
2. 在文字方塊中輸入名稱，然後按一下**顯示名稱** 按鈕。

    您輸入的名稱會顯示在[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項。 請注意，當您按一下按鈕時，頁面張貼至 Web 伺服器。 ASP.NET 然後重新建立頁面時，便會執行您的程式碼 (在此情況下， [] 按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項的[按一下](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)事件處理常式的執行)，然後傳送至瀏覽器的 [新的頁面。 如果您觀察瀏覽器中的 [狀態] 列，您可以看到的頁面對往返 Web 伺服器每次您按一下按鈕。
3. 在瀏覽器中檢視您正在執行的頁面上按一下滑鼠右鍵，然後選取的頁面的來源**檢視原始檔**。

    頁面在原始程式碼中，您會看到 HTML 而不需要任何伺服器程式碼。 具體來說，您看不見**&lt;asp:&gt;** 中所使用的項目**來源**檢視。 頁面執行時，ASP.NET 會處理伺服器控制項，並轉譯頁面執行的函式，表示控制項的 HTML 元素。 例如， **&lt;p: Button&gt;** 控制項呈現為 HTML **&lt;輸入類型 =&quot;提交&quot;&gt;** 項目。
4. 關閉瀏覽器。


## <a name="working-with-additional-controls"></a>使用其他控制項

<a id="sectionToggle2"></a>

在這個部分的逐步解說中，您會使用[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項，一次顯示一個月的日期。 [行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項是更複雜的控制項，比您已使用按鈕、 文字方塊和標籤，並說明一些其他功能的伺服器控制項。

在本節中，您將新增[System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項加入頁面，並設定其格式。

### <a name="to-add-a-calendar-control"></a>若要加入行事曆控制項


1. 在 Visual Studio 中，切換至**設計**檢視。
2. 從**標準**一節**工具箱**，拖曳[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項拖曳至頁面上，把它放在下面的**div**項目，包含其他控制項。

    行事曆的智慧標籤面板隨即顯示。 面板會顯示命令，以便讓您輕鬆地執行所選控制項的最常見的工作。 如下圖所示[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項中呈現**設計**檢視。

    ![月曆控制項在設計檢視](creating-a-basic-web-forms-page/_static/image13.png "月曆控制項在設計檢視")
3. 在智慧標籤面板中，選擇**自動格式化**。

    **自動格式化**對話方塊隨即出現，可讓您選取的行事曆的格式化配置。 如下圖所示**自動格式化** 對話方塊[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項。

    ![自動格式化對話方塊 （Calendar 控制項）](creating-a-basic-web-forms-page/_static/image14.png "自動格式化對話方塊 （Calendar 控制項）")
4. 從**選取配置**清單中，選取**簡單**，然後按一下 **確定**。
5. 若要切換**來源**檢視。

    您所見 **&lt;asp： 行事曆&gt;** 項目。 此元素會遠超過簡單的控制項，您稍早建立的項目。 它也包含子元素，例如 **&lt;WeekEndDayStyle&gt;**，其代表各種不同的格式設定。 如下圖所示[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制中**來源**檢視。 (您在中看到的確切標記**來源**檢視可能會稍有不同的圖。)

    ![月曆控制項在原始碼檢視](creating-a-basic-web-forms-page/_static/image15.png "月曆控制項在原始碼檢視")


### <a name="programming-the-calendar-control"></a>程式設計 行事曆控制項


在本節中，您將編寫[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項來顯示目前選取的日期。

### <a name="to-program-the-calendar-control"></a>以程式設計的行事曆控制項


1. 在 **設計**檢視中，按兩下[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項。

    新的事件處理常式會建立並顯示在名為的程式碼後置檔案*FirstWebPage.aspx.cs*。
2. 完成[SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx)為下列程式碼的事件處理常式。

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    上述程式碼中的日曆控制項選取的日期設定 label 控制項的文字。


### <a name="running-the-page"></a>執行網頁


您現在可以測試行事曆。

### <a name="to-run-the-page"></a>若要執行的頁面


1. 按下**CTRL + F5**執行網頁瀏覽器中。
2. 按一下行事曆中的日期。

    您按下的日期會顯示在[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項。
3. 在瀏覽器中檢視網頁的原始程式碼。

    請注意，[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項的呈現至與頁面**表格**，以做為每一天**td**項目。
4. 關閉瀏覽器。


## <a name="next-steps"></a>後續步驟


<a id="nextStepsToggle"></a>

本逐步解說，說明了 Visual Studio 網頁設計工具的基本功能。 既然您了解如何建立和編輯 Visual Studio 中的 Web Form 網頁，您可能想要探索其他功能。 例如，您可以執行下列作業：

- 深入了解 ASP.NET Web Form 遵循逐步教學課程系列[Getting Started with ASP.NET 4.5 Web Form 和 Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)。
- 深入了解重疊顯示樣式表 (CSS)。 如需詳細資訊，請參閱 <<c0> [ 使用 CSS 概觀](https://msdn.microsoft.com/library/bb398931.aspx)。
