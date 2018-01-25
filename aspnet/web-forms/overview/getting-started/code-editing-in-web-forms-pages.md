---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: "編輯程式碼的 Visual Studio 2013 中的 ASP.NET Web Form |Microsoft 文件"
author: Erikre
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 8714f673cb0434189ca23d2dda14035d8652a051
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Visual Studio 2013 中的程式碼編輯的 ASP.NET Web Form
====================
由[Erik Reitan](https://github.com/Erikre)

在許多的 ASP.NET Web Form 頁面中，您可以撰寫 Visual Basic、 C# 中，或另一種語言中的程式碼。 在 Visual Studio 中的程式碼編輯器可協助您快速地撰寫程式碼，同時又能夠協助您避免錯誤。 此外，編輯器會提供方法讓您建立可重複使用程式碼以減少您需要執行的工作量。

這個逐步解說將說明 Visual Studio 程式碼編輯器的各種功能。

在這個逐步解說期間，您將了解如何：

- 修正內嵌程式碼撰寫錯誤。
- 重構和重新命名程式碼。
- 重新命名的變數和物件。
- 插入程式碼片段。

## <a name="prerequisites"></a>必要條件


若要完成這個逐步解說，您將需要：

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)或[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。 會自動安裝.NET Framework。 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 和 Microsoft Visual Studio Express 2013 for Web 將通常被稱為 Visual Studio 在此教學課程系列。  
    >   
    > 如果您使用 Visual Studio，本逐步解說假設您已經選擇**Web 程式開發**設定的集合，您會啟動 Visual Studio 的第一次。 如需詳細資訊，請參閱[如何： 選取 Web 程式開發環境設定](https://msdn.microsoft.com/library/ff521558.aspx)。

 如需 Visual Studio 和 ASP.NET 的簡介，請參閱[在 Visual Studio 2013 中建立基本的 ASP.NET 4.5 Web Form 頁面](creating-a-basic-web-forms-page.md)。   
 

## <a name="creating-a-web-application-project-and-a-page"></a>建立 Web 應用程式專案和頁面

<a id="sectionToggle0"></a>

在這部分的逐步解說中，您將建立 Web 應用程式專案，並加入新的頁面。

### <a name="to-create-a-web-application-project"></a>若要建立 Web 應用程式專案

1. 開啟 Microsoft Visual Studio。
2. 在 [檔案] 功能表上，選取 [新增專案]。  
    ![檔案功能表](code-editing-in-web-forms-pages/_static/image1.png)

    [ **新增專案** ] 對話方塊隨即出現。
3. 選取**範本** - &gt; **Visual C#**  - &gt; **Web**左側的 [範本] 群組。
4. 選擇**ASP.NET Web 應用程式**中心資料行中的範本。
5. 命名您的專案***BasicWebApp***按一下**確定** 按鈕。   
![新增專案對話方塊](code-editing-in-web-forms-pages/_static/image2.png)
6. 接下來，選取**Web Form**範本，然後按一下**確定**按鈕以建立專案。  
![新增 ASP.NET 專案對話方塊](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio 建立新的專案，其中包含預先建置的 Web Form 範本為基礎的功能。


## <a name="creating-a-new-aspnet-web-forms-page"></a>建立新的 ASP.NET Web Form 網頁


當您建立新的 Web Form 應用程式使用**ASP.NET Web 應用程式**專案範本，Visual Studio 加入 ASP.NET 網頁 （Web Form 頁面） 名為*Default.aspx*，以及以多個其他檔案和資料夾。 您可以使用*Default.aspx*與 Web 應用程式的 [首頁] 頁面的頁面。 不過，對於此逐步解說中，您會建立和使用新的頁面。

### <a name="to-add-a-page-to-the-web-application"></a>若要將頁面加入至 Web 應用程式


1. 在**方案總管 中**，以滑鼠右鍵按一下 Web 應用程式名稱 (在此教學課程中的應用程式名稱是**BasicWebSite**)，然後按一下**新增** - &gt;**新項目**。   
隨即顯示 [ 新增項目] 對話方塊。
2. 選取**Visual C#**  - &gt; **Web**左側的 [範本] 群組。 然後，選取**Web Form**中間清單並將其命名*FirstWebPage.aspx*。   
    ![加入新項目對話方塊](code-editing-in-web-forms-pages/_static/image4.png)
3. 按一下**新增**Web Form 網頁加入您的專案。  
 Visual Studio 會建立新的頁面，並開啟它。
4. 接下來，將這個新的頁面設定為預設的起始頁。 在**方案總管 中**，以滑鼠右鍵按一下名為新頁面*FirstWebPage.aspx*選取**設定為起始頁**。 下次您執行此應用程式來測試進度，您會自動看到這個新的網頁瀏覽器中。


## <a name="correcting-inline-coding-errors"></a>修正內嵌程式碼錯誤


在 Visual Studio 中的程式碼編輯器，可協助您避免錯誤，因為您撰寫程式碼，以及如果您做了錯誤，程式碼編輯器可協助您更正這個錯誤。 在這個部分的逐步解說中，您將撰寫說明錯誤修正功能，在編輯器中的程式碼行。

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>若要修正 Visual Studio 中的簡單程式碼錯誤


1. 在**設計**檢視中，按兩下空白網頁，以建立的處理常式**負載**網頁事件。   
您使用只做為位置的事件處理常式撰寫一些程式碼。
2. 內部處理常式中，輸入下列行，其中包含錯誤和按**ENTER**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

 當您按**ENTER**，程式碼編輯器中放置綠色及紅色底線 (通常呼叫&quot;曲線&quot;行) 下的程式碼有問題的區域。 綠色的底線表示警告。 紅色底線指出您必須修正的錯誤。 

    將滑鼠指標`myStr`查看會告訴您有關警告的工具提示。 此外，請將滑鼠指標上的紅色底線，以查看錯誤訊息。

    下圖顯示的程式碼以底線表示。

    ![在 [設計] 檢視中的歡迎文字](code-editing-in-web-forms-pages/_static/image5.png "設計 檢視中的歡迎文字")  
 必須先修正錯誤，加上分號`;`到一行的結尾。 警告只是通知您您未使用`myStr`尚未變數。  

    > [!NOTE] 
    > 
    > 檢視您目前的程式碼格式化選取的 Visual Studio 中的設定**工具** - &gt; **選項** - &gt; **字型和色彩**。


## <a name="refactoring-and-renaming"></a>重構和重新命名

重構是一種軟體方法，包括重新建構您的程式碼，以便更容易了解和維護，同時保留其功能。 簡單的範例可能是您要從資料庫取得資料的事件處理常式中撰寫程式碼。 開發網頁時，會發現您需要從多個不同的處理常式存取資料。 因此，您會藉由在網頁中建立資料存取方法的處理常式中插入方法的呼叫，來重構網頁的程式碼。

程式碼編輯器中包含工具，可協助您執行各種重構的工作。 在本逐步解說中，您將使用兩個重構技術： 重新命名變數和擷取方法。 重構的其他選項包括封裝欄位、 將區域變數提升為方法參數，以及管理方法參數。 這些重構選項的可用性取決於程式碼中的位置。

### <a name="refactoring-code"></a>重構程式碼

重構的常見案例是建立 （擷取） 位於另一個成員，例如方法的程式碼中的方法。 這會減少原始成員的大小，並使擷取的程式碼可重複使用。

在這部分的逐步解說中，您將撰寫一些簡單的程式碼，然後從其中擷取方法。 重構被支援 C# 中，因此您將建立使用 C# 做為程式設計語言的網頁。

### <a name="to-extract-a-method-in-a-c-page"></a>若要擷取在 C# 頁面方法

1. 切換至**設計**檢視。
2. 在**工具箱**，從**標準**索引標籤，拖曳[按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)拖曳到頁面的控制項。
3. 按兩下**按鈕**控制項建立的處理常式其[按一下](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)事件，然後加入下列反白顯示的程式碼：

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

 程式碼會建立**ArrayList**物件，若要載入的值，會使用迴圈，然後再使用另一個迴圈顯示的內容**ArrayList**物件。
4. 按**CTRL + F5**來執行網頁，然後按一下**按鈕**先確定您會看到下列輸出：   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. 返回程式碼編輯器中，然後選取下列幾行的事件處理常式。   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. 以滑鼠右鍵按一下選取範圍中，按一下**重構**，然後選擇 **擷取方法**。 

    **擷取方法** 對話方塊隨即出現。
7. 在**新方法名稱**方塊中，輸入**DisplayArray**，然後按一下 **確定**。 

    程式碼編輯器中建立新的方法，名為`DisplayArray`，並放入新的方法中呼叫**按一下**其中迴圈原本的處理常式。

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. 按**CTRL + F5**執行頁面，並且按一下**按鈕**。

    網頁的運作與以前一樣。 `DisplayArray`方法現在可以從任何地方呼叫頁面類別中。

## <a name="renaming-variables"></a>重新命名的變數

當您使用變數，以及物件時，您可以之後在程式碼中參考重新命名。 不過，重新命名的變數和物件可能會導致程式碼中斷如果您未重新命名其中一個參考。 因此，您可以使用重構執行重新命名。

### <a name="to-use-refactoring-to-rename-a-variable"></a>若要使用重構為變數重新命名


1. 在**按一下**事件處理常式中，找出下列這一行：

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. 以滑鼠右鍵按一下變數名稱`alist`，選擇**重構**，然後選擇 **重新命名**。

    **重新命名** 對話方塊隨即出現。
3. 在**新名稱**方塊中，輸入**ArrayList1** ，並確定**預覽參考變更**已選取核取方塊。 然後按一下 [確定]。 

    **預覽變更** 對話方塊隨即出現，並顯示包含您要重新命名該變數的所有參考的樹狀結構。
4. 按一下**套用**關閉**預覽變更** 對話方塊。

    特別是指您所選取的執行個體的變數會重新命名。 不過請注意，變數`alist`下列行中不會重新命名。

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    變數`alist`此行中不重新命名，因為它不代表相同的變數值`alist`已重新命名。 變數`alist`中`DisplayArray`宣告是針對該方法的本機變數。 這說明使用重構重新命名變數不同於直接在編輯器中，執行尋找和取代動作了解正在使用該變數的語意的重構重新命名變數。


## <a name="inserting-snippets"></a>插入程式碼片段

有許多 Web Form 開發人員經常需要執行的程式碼撰寫工作，因為程式碼編輯器中提供程式碼片段或預先撰寫的程式碼區塊的程式的庫。 您可以將這些程式碼片段插入您的頁面。

您使用 Visual Studio 中的每種語言有插入程式碼片段的方式有些微差異。 插入程式碼片段的相關資訊，請參閱[Visual Basic IntelliSense 程式碼片段](https://msdn.microsoft.com/library/18yz4be4.aspx)。 如需插入程式碼片段中 Visual C# 中的資訊，請參閱[Visual C# 程式碼片段](https://msdn.microsoft.com/library/z41h7fat.aspx)。

## <a name="next-steps"></a>後續步驟

本逐步解說已說明 Visual Studio 2010 程式碼編輯器的基本功能的程式碼中的錯誤、 重構程式碼、 重新命名變數和程式碼片段插入程式碼。 在編輯器中的其他功能可以確保應用程式開發快速又簡單。 例如，您可能要：

- 進一步了解 IntelliSense，例如修改 IntelliSense 選項、 管理程式碼片段，以及搜尋線上程式碼片段的功能。 如需詳細資訊，請參閱[使用 IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx)。
- 了解如何建立您自己的程式碼片段。 如需詳細資訊，請參閱[建立和使用 IntelliSense 程式碼片段](https://msdn.microsoft.com/library/ms165392.aspx)
- 深入了解 Visual Basic 特定的 IntelliSense 程式碼片段，例如自訂程式碼片段，以及疑難排解功能。 如需詳細資訊，請參閱[Visual Basic IntelliSense 程式碼片段](https://msdn.microsoft.com/library/18yz4be4.aspx)
- 深入了解 C# 的特定功能的 IntelliSense，例如重構和程式碼片段。 如需詳細資訊，請參閱[Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx)。
