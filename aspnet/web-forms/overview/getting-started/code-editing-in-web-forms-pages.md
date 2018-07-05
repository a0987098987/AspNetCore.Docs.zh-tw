---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: 程式碼編輯 Visual Studio 2013 中的 ASP.NET Web Form |Microsoft Docs
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 322a9aa6fb366eb9d5be33838fd6ac7ea51047f8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371790"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Visual Studio 2013 中的程式碼編輯 ASP.NET Web Form
====================
藉由[Erik Reitan](https://github.com/Erikre)

在許多的 ASP.NET Web Form 頁面中，您可以撰寫 Visual Basic、 C# 中，或另一種語言中的程式碼。 在 Visual Studio 中的程式碼編輯器可協助您快速撰寫程式碼，同時協助您避免錯誤。 此外，編輯器會提供方法讓您建立可重複使用的程式碼以減少您需要執行的工作量。

此逐步解說將說明 Visual Studio 程式碼編輯器的各種功能。

在這個逐步解說期間，您將了解如何：

- 修正內嵌程式碼錯誤。
- 重構和重新命名程式碼。
- 重新命名的變數和物件。
- 插入程式碼片段。

## <a name="prerequisites"></a>必要條件


若要完成這個逐步解說，您將需要：

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)或是[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。 .NET Framework 會自動安裝。 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 和 Microsoft Visual Studio Express 2013 for Web 將通常稱為 Visual Studio 在此教學課程系列。  
    >   
    > 如果您使用 Visual Studio，本逐步解說假設您已經選擇**Web 開發**設定集合，第一次您啟動 Visual Studio。 如需詳細資訊，請參閱[如何： 選取 Web 程式開發環境設定](https://msdn.microsoft.com/library/ff521558.aspx)。

  Visual Studio 及 ASP.NET 的簡介，請參閱 < [Visual Studio 2013 中建立基本的 ASP.NET 4.5 Web Form 頁面](creating-a-basic-web-forms-page.md)。   
 

## <a name="creating-a-web-application-project-and-a-page"></a>建立 Web 應用程式專案和頁面

<a id="sectionToggle0"></a>

在本逐步解說的這個部分，您會建立 Web 應用程式專案，並加入新的頁面。

### <a name="to-create-a-web-application-project"></a>若要建立的 Web 應用程式專案

1. 開啟 Microsoft Visual Studio。
2. 在 [檔案] 功能表上，選取 [新增專案]。  
    ![[檔案] 功能表](code-editing-in-web-forms-pages/_static/image1.png)

    [ **新增專案** ] 對話方塊隨即出現。
3. 選取 **範本** - &gt; **Visual C#**  - &gt; **Web**左側的 範本 群組。
4. 選擇**ASP.NET Web 應用程式**中間欄的範本。
5. 命名您的專案***BasicWebApp***然後按一下**確定** 按鈕。   
![[新增專案] 對話方塊](code-editing-in-web-forms-pages/_static/image2.png)
6. 接下來，選取**Web Form**範本，然後按一下**確定**按鈕，以建立專案。  
![[新增 ASP.NET 專案] 對話方塊](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio 會建立新的專案，其中包含預先建置的 Web Form 範本為基礎的功能。


## <a name="creating-a-new-aspnet-web-forms-page"></a>建立新的 ASP.NET Web Form 頁面


當您建立新的 Web Form 應用程式使用**ASP.NET Web 應用程式**專案範本，Visual Studio 會加入名為 ASP.NET 網頁 （Web Form 頁面） *Default.aspx*，以及數個其他檔案和資料夾。 您可以使用*Default.aspx*與 Web 應用程式的 [首頁] 頁面的頁面。 不過，對於此逐步解說中，您會建立並使用新的頁面。

### <a name="to-add-a-page-to-the-web-application"></a>將頁面新增至 Web 應用程式


1. 在**方案總管**，以滑鼠右鍵按一下 Web 應用程式名稱 (在應用程式名稱是本教學課程**BasicWebSite**)，然後按一下 **新增** - &gt;**新的項目**。   
隨即顯示 [ 新增項目] 對話方塊。
2. 選取  **Visual C#**  - &gt; **Web**左側的 範本 群組。 然後，選取**Web Form**從中間清單並將它命名*FirstWebPage.aspx*。   
    ![加入新項目對話方塊](code-editing-in-web-forms-pages/_static/image4.png)
3. 按一下 **新增**將 Web Form 頁面新增至您的專案。  
 Visual Studio 會建立新的頁面，並開啟它。
4. 接下來，將這個新頁面設定為預設起始頁中。 在 [**方案總管] 中**，以滑鼠右鍵按一下名為新的頁面*FirstWebPage.aspx* ，然後選取**設定為起始頁**。 下次您執行此應用程式來測試我們的進度，您會自動看到這個新的瀏覽器頁面。


## <a name="correcting-inline-coding-errors"></a>修正內嵌程式碼錯誤


在 Visual Studio 中的程式碼編輯器，可協助您避免發生錯誤，因為您撰寫程式碼，而且如果您已發生錯誤，程式碼編輯器可協助您更正錯誤。 在這部分的逐步解說中，您將撰寫一行程式碼說明在編輯器中的錯誤修正功能。

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>若要修正 Visual Studio 中的簡單程式碼撰寫錯誤


1. 在 **設計**檢視中，按兩下 空白的頁面，即可建立的處理常式**負載**事件頁面。   
   您只將事件處理常式當做位置，以撰寫一些程式碼。
2. 內部處理常式中，輸入下列行，其中包含錯誤，並按下**ENTER**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   當您按下**ENTER**，程式碼編輯器會在放置綠色和紅色底線 (通常會呼叫&quot;曲線&quot;行) 下的程式碼有問題的區域。 綠色的底線表示警告。 紅色底線表示您必須修正的錯誤。 

    將滑鼠指標停留`myStr`若要查看會告訴您有關警告的工具提示。 此外，您的滑鼠指標停留紅色底線，以查看錯誤訊息。

    下圖顯示以底線程式碼。

    ![在 [設計] 檢視中的歡迎文字](code-editing-in-web-forms-pages/_static/image5.png "設計 檢視中的歡迎文字")  
   必須修正錯誤，加上分號`;`一行的結尾。 警告只會通知您，您還沒有使用`myStr`尚未變數。  

    > [!NOTE] 
    > 
    > 檢視您目前的程式碼格式化選取的 Visual Studio 中的設定**工具** - &gt; **選項** - &gt; **字型和色彩**。


## <a name="refactoring-and-renaming"></a>重構和重新命名

重構是一種軟體的方法，包括重新建構您的程式碼更容易了解和維護，同時保留其功能。 簡單的範例可能是您從資料庫取得資料的事件處理常式撰寫程式碼。 當您開發您的頁面時，您會發現您需要從多個不同的處理常式存取資料。 因此，您會藉由建立網頁中的資料存取方法的處理常式中插入方法的呼叫，來重構網頁的程式碼。

程式碼編輯器包含可協助您執行重構的各種工作的工具。 在本逐步解說中，您將使用兩個重構的方法： 重新命名變數，並擷取方法。 其他重構的選項包括封裝欄位、 升級至方法參數的本機變數，以及管理方法的參數。 這些重構選項的可用性取決於程式碼中的位置。

### <a name="refactoring-code"></a>重構程式碼

常見的重構案例是建立 (extract) 從另一個成員，例如方法內的程式碼的方法。 這可減少原始成員的大小，並可擷取的程式碼可重複使用。

在本逐步解說的這個部分，您會撰寫一些簡單的程式碼，並再從中擷取方法。 重構支援 C# 中，因此您將建立使用 C# 做為程式設計語言的頁面。

### <a name="to-extract-a-method-in-a-c-page"></a>若要擷取的 C# 頁面方法

1. 若要切換**設計**檢視。
2. 在 **工具箱**，從**標準**索引標籤，拖曳[按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)拖曳至網頁。
3. 按兩下** 按鈕**控制項來建立的處理常式及其[按一下](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)事件，然後加入下列反白顯示的程式碼：

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   程式碼會建立**ArrayList**物件，載入的值，會使用迴圈，然後再使用另一個迴圈顯示的內容**ArrayList**物件。
4. 按下**CTRL + F5**來執行頁面，然後按一下** 按鈕**藉此確定您會看到下列輸出：   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. 返回 程式碼編輯器，，，然後選取 事件處理常式中的 下列幾行。   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. 以滑鼠右鍵按一下選取範圍中，按一下**重構**，然後選擇**擷取方法**。 

    **擷取方法** 對話方塊隨即出現。
7. 在 **新的方法名稱**方塊中，輸入**DisplayArray**，然後按一下**確定**。 

    程式碼編輯器中建立名為的新方法`DisplayArray`，並將放在新的方法呼叫**按一下**迴圈的最初的處理常式。

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. 按下**CTRL + F5**頁面上再次執行，然後按一下** 按鈕**。

    網頁的運作與以前一樣。 `DisplayArray`方法現在可以從任何地方的呼叫，頁面類別中。

## <a name="renaming-variables"></a>重新命名變數

當您使用變數，以及物件時，您可能想要將其重新命名之後在程式碼中參考。 不過，重新命名的變數和物件可能會導致中斷如果您未重新命名其中一個參考的程式碼。 因此，您可以使用重構執行重新命名。

### <a name="to-use-refactoring-to-rename-a-variable"></a>若要使用的變數重新命名的重構


1. 在 **按一下**事件處理常式中，找到下列這行：

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. 變數名稱上按一下滑鼠右鍵`alist`，選擇**重構**，然後選擇**重新命名**。

    **重新命名** 對話方塊隨即出現。
3. 在 **新的名稱**方塊中，輸入**ArrayList1** ，並確定**預覽參考變更**在選取核取方塊。 然後按一下 [確定]。 

    **預覽變更**對話方塊隨即出現，並顯示包含您要重新命名的變數的所有參考的樹狀結構。
4. 按一下 [**套用**以關閉**預覽變更**] 對話方塊。

    特別是指您所選取的執行個體的變數會重新命名。 不過請注意，變數`alist`下行中不會重新命名。

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    變數`alist`此行中不會重新命名，因為它不代表相同的值作為變數`alist`已重新命名。 變數`alist`在`DisplayArray`宣告是針對該方法的區域變數。 這說明使用重構重新命名變數不同於直接執行編輯器; 中的 尋找和取代動作了解其所使用的變數語意的重構重新命名變數。


## <a name="inserting-snippets"></a>插入程式碼片段

因為有許多 Web Form 開發人員經常需要執行的程式碼撰寫工作，所以程式碼編輯器中提供程式碼片段或預先撰寫的程式碼區塊的程式的庫。 您可以將這些程式碼片段插入您的頁面。

使用 Visual Studio 中的每種語言有插入程式碼片段的方式有些微的差異。 插入程式碼片段的相關資訊，請參閱[Visual Basic IntelliSense 程式碼片段](https://msdn.microsoft.com/library/18yz4be4.aspx)。 如需插入程式碼片段中 Visual C# 中的資訊，請參閱[Visual C# 程式碼片段](https://msdn.microsoft.com/library/z41h7fat.aspx)。

## <a name="next-steps"></a>後續步驟

本逐步解說已說明 Visual Studio 2010 程式碼編輯器的基本功能的修正程式碼中的錯誤、 重構程式碼、 重新命名變數，和您的程式碼中插入程式碼片段。 在編輯器中的其他功能可讓應用程式開發快速又簡單。 例如，您可能要：

- 深入了解 IntelliSense，例如修改 IntelliSense 選項、 管理程式碼片段，以及搜尋線上程式碼片段的功能。 如需詳細資訊，請參閱[使用 IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx)。
- 了解如何建立您自己的程式碼片段。 如需詳細資訊，請參閱[建立及使用 IntelliSense 程式碼片段](https://msdn.microsoft.com/library/ms165392.aspx)
- 深入了解 IntelliSense 程式碼片段，例如自訂程式碼片段和疑難排解的 Visual Basic 特定功能。 如需詳細資訊，請參閱[Visual Basic IntelliSense 程式碼片段](https://msdn.microsoft.com/library/18yz4be4.aspx)
- 深入了解 C#-的 IntelliSense、 重構和程式碼片段等的特定功能。 如需詳細資訊，請參閱 < [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx)。
