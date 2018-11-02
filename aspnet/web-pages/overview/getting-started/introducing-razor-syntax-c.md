---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: 使用 Razor 語法 (C#) 的 ASP.NET Web 程式設計簡介 |Microsoft Docs
author: tfitzmac
description: 本章概述您程式設計的 ASP.NET 網頁使用 Razor 語法。 ASP.NET 是 Microsoft 的技術，用於執行動態 web 的 pa...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: b242bf52bbd63d726e6ce6ab7be01a1b81c5bf1b
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758254"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>使用 Razor 語法 (C#) 的 ASP.NET Web 程式設計簡介
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章概述您程式設計的 ASP.NET 網頁使用 Razor 語法。 ASP.NET 是 Microsoft 的技術，用於在 web 伺服器上執行動態網頁。 本文章著重於使用 C# 程式設計語言中。
> 
> **您將學到什麼**:
> 
> - 前 8 的程式設計入門程式設計使用 Razor 語法的 ASP.NET Web Pages 的提示。
> - 您需要的基本程式設計概念。
> - ASP.NET server 程式碼和 Razor 語法是有關。
>   
> 
> ## <a name="software-versions"></a>軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2。


## <a name="the-top-8-programming-tips"></a>最佳的 8 程式設計秘訣

此區段會列出您一定要知道當您開始撰寫使用 Razor 語法的 ASP.NET 伺服器程式碼的一些秘訣。

> [!NOTE]
> 根據 Razor 語法的 C# 程式設計語言，以及這最常使用以 ASP.NET Web Pages 的語言。 不過，Razor 語法也支援 Visual Basic 語言，以及您會看到您也可以執行在 Visual Basic 中的所有項目。 如需詳細資訊，請參閱附錄[Visual Basic 語言與語法](https://go.microsoft.com/fwlink/?LinkId=202908)。


在本文稍後，您可以找到大部分的這些程式設計技術的更多詳細。

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1.您將程式碼新增至頁面上，使用 @ 字元

`@`字元開始內嵌運算式、 單一陳述式區塊和多重陳述式區塊：

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

這是這些陳述式如下所示在網頁瀏覽器中執行時：

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **HTML 編碼**
> 
> 當您在頁面上，使用顯示的內容時`@`字元 ASP.NET HTML 編碼的輸出，如上述範例中，所示。 這會取代 HTML 的保留的字元 (例如`<`並`>`和`&`) 與啟用顯示網頁，而不會轉譯為 HTML 標記或實體中的字元為字元的代碼。 而不進行 HTML 編碼，從伺服器程式碼的輸出可能不正確，會顯示，並可能會暴露於安全性風險的頁面。
> 
> 如果您的目標是要輸出轉譯為標記的標記的 HTML 標記 (例如`<p></p>`段落或`<em></em>`來強調文字)，請參閱節[結合文字、 標記和程式碼區塊中的程式碼](#BM_CombiningTextMarkupAndCode)本文稍後的。
> 
> 您可以深入了解中的 HTML 編碼[使用表單](https://go.microsoft.com/fwlink/?LinkId=202892)。


### <a name="2-you-enclose-code-blocks-in-braces"></a>2.大括號括住程式碼區塊

A*程式碼區塊*包含一或多個程式碼陳述式和大括號括住。

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

在瀏覽器中顯示結果：

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3.區塊中，您最後使用分號的每個程式碼陳述式

程式碼區塊中，每個完整的程式碼陳述式必須以分號結尾。 內嵌運算式不以分號結尾。

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4.您可以使用變數來儲存值

您可以儲存中的值*變數*，包括字串、 數字和日期等。您建立新變數使用`var`關鍵字。 您可以直接在頁面上，使用中插入變數的值`@`。

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

在瀏覽器中顯示結果：

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5.您將常值字串值括在雙引號中

A*字串*是會被視為文字的字元序列。 若要指定為字串，您將它括在雙引號中：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

如果您想要顯示的字串包含反斜線字元 ( `\` ) 或雙引號 ( `"` )，使用*逐字字串常值*，前面會加上`@`運算子。 (在 C# 中，\ 字元具有特殊意義，除非您使用逐字字串常值。)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

若要內嵌雙引號，請使用逐字字串常值並重複引號：

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

以下是在網頁中使用這兩個範例的結果：

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> 請注意，`@`字元用來將標記在 C# 中逐字字串常值，以及將 ASP.NET 頁面的程式碼。


### <a name="6-code-is-case-sensitive"></a>6.程式碼會區分大小寫

以 C# 關鍵字 (例如`var`， `true`，和`if`) 和變數名稱會區分大小寫。 下列程式碼會建立兩個不同的變數，`lastName`和 `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

如果您宣告一個變數`var lastName = "Smith";`如果您嘗試參考該變數做為頁面中的`@LastName`，會發生錯誤，因為`LastName`無法辨識。

> [!NOTE]
> 在 Visual Basic 中的關鍵字而變數則屬於*不*區分大小寫。


### <a name="7-much-of-your-coding-involves-objects"></a>7.大部分您撰寫程式碼牽涉到物件

*物件*代表您可以使用程式設計的項目&#8212;頁面、 文字方塊、 檔案、 影像、 web 要求、 電子郵件訊息、 客戶記錄 （資料庫資料列） 等等。物件具有描述其特性的屬性，以及您可以用來讀取，或變更&#8212;的文字 方塊中的物件具有`Text`（還有其他） 的屬性，要求物件具有`Url`屬性，電子郵件訊息已`From`屬性，與客戶物件都有`FirstName`屬性。 物件也有方法&quot;動詞&quot;他們可以執行。 範例包括將檔案物件的`Save`方法中，映像物件的`Rotate`方法，以及電子郵件物件的`Send`方法。

您會經常使用`Request`物件，例如值的文字方塊 （表單欄位） 的資訊讓您在頁面上，進行要求的頁面、 使用者身分識別等 URL 的瀏覽器的何種類型。下列範例示範如何存取的屬性`Request`物件，以及如何呼叫`MapPath`方法`Request`物件，可讓您在伺服器的頁面上的絕對路徑：

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

在瀏覽器中顯示結果：

![Razor Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8.您可以撰寫程式碼，讓決策

動態網頁的重要功能是，您可以決定如何根據條件。 若要這樣做最常見的方法是使用`if`陳述式 (和選擇性`else`陳述式)。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

陳述式`if(IsPost)`是本文撰寫的簡略方法`if(IsPost == true)`。 連同`if`陳述式，有各種不同的方式來測試條件，重複程式碼區塊，並依此類推，這是本文稍後所述。

在瀏覽器中顯示結果 (按一下後**送出**):

![Razor Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET 和 POST 方法與 IsPost 屬性
> 
> 用於 web 網頁 (HTTP) 的通訊協定支援非常有限的用來對伺服器提出要求的方法 （動詞命令）。 最常見的兩個為 GET，會用來讀取的頁面和文章，用來提交頁面。 一般情況下，使用者要求的頁面上，第一次要求頁面時使用 GET。 如果使用者在表單中填入，然後再按一下 [提交] 按鈕，瀏覽器會對伺服器提出 POST 要求。
> 
> 在 web 程式設計中，通常很有用知道是否要求網頁時正在為 GET 或 POST，讓您知道如何處理頁面。 ASP.NET Web Pages 中，您可以使用`IsPost`屬性，以查看要求是 GET 或 POST。 如果要求是某篇文章，`IsPost`屬性會傳回 true，而且您可以執行像是讀取表單上的文字方塊的值。 您會看到的許多範例會示範如何處理值的方式而定頁面`IsPost`。


## <a name="a-simple-code-example"></a>簡單的程式碼範例

此程序會示範如何建立一個網頁，說明基本的程式設計技術。 在範例中，您可以建立讓使用者輸入兩個數字，然後再將它們加入，並顯示結果頁面。

1. 在編輯器中，建立新的檔案並將它命名*AddNumbers.cshtml*。
2. 將下列程式碼和標記複製到頁面上，而且要取代已經在頁面中的任何項目中。  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    以下是您必須注意的事項：

    - `@`字元會在頁面中，啟動程式碼的第一個區塊，而且它前面出現`totalMessage`內嵌頁面底部附近的變數。
    - 位於頁面頂端的區塊會括在大括弧。
    - 在頂端的區塊，每一行都會以分號結尾。
    - 變數`total`， `num1`， `num2`，和`totalMessage`儲存數個數字和字串。
    - 常值字串值，指派給`totalMessage`變數是雙引號括住。
    - 由於程式碼時區分大小寫，`totalMessage`頁面底部附近，會使用變數，其名稱必須完全符合在上方的變數。
    - 運算式`num1.AsInt() + num2.AsInt()`示範如何使用物件和方法。 `AsInt`上每個變數的方法會將轉換使用者輸入的數字 （整數），以便您可以在其上執行算術運算的字串。
    - `<form>`標記會包括`method="post"`屬性。 這表示當使用者按一下**新增**，頁面將會傳送到伺服器使用 HTTP POST 方法。 當提交頁面時，`if(IsPost)`測試評估為 true，條件式程式碼執行時，顯示個數字相加的結果。
3. 儲存頁面，並在瀏覽器中執行它。 (請確定中選取頁面**檔案**才能執行這個工作區。)輸入兩個整數數字，然後按一下**新增** 按鈕。 

    ![Razor Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>基本程式設計概念

這篇文章為您提供 ASP.NET web 程式設計的概觀。 它不是詳盡的檢查，只是透過將最常使用的程式設計概念的快速教學。 即便如此，它涵蓋了幾乎所有您要開始使用 ASP.NET Web Pages。

但首先，小小的技術背景。

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Razor 語法、 伺服端程式碼和 ASP.NET

Razor 語法是一個簡單的程式設計語法，將伺服器為基礎的程式碼內嵌在網頁中。 使用 Razor 語法的網頁，在中，有兩種類型的內容： 用戶端內容和伺服器程式碼。 用戶端的內容是您要用來在網頁中的項目： HTML 標記 （項目），樣式資訊，例如 CSS、 或許有些用戶端指令碼，例如 JavaScript 和純文字。

Razor 語法可讓您將伺服器程式碼新增至這個用戶端內容。 如果網頁沒有伺服端程式碼，在伺服器執行該程式碼第一次後, 才會傳送至瀏覽器的頁面。 藉由在伺服器上執行，程式碼可以執行可能更複雜使用單獨的情況，就像是存取伺服器端資料庫的用戶端內容來執行動作的工作。 伺服端程式碼以動態方式可以建立用戶端內容的最重要的是，&#8212;它可以產生 HTML 標記或其他內容，即時並再將它傳送至以及頁面可能會包含任何靜態 HTML 瀏覽器。 瀏覽器的觀點而言，伺服器程式碼所產生的用戶端內容並沒什麼兩樣任何其他的用戶端內容。 如您所見，所需的伺服器程式碼都非常簡單。

包含 Razor 語法的 ASP.NET web pages 有特殊的檔案副檔名 (*.cshtml*或是 *.vbhtml*)。 伺服器會辨識這些擴充功能，執行含有 Razor 語法標記，然後再將網頁傳送至瀏覽器的程式碼。

### <a name="where-does-aspnet-fit-in"></a>沒有 ASP.NET 適用於什麼情況？

Razor 語法是以呼叫為基礎的 Microsoft.NET Framework 的 ASP.NET 的 microsoft 技術為基礎。 .Net Framework 是大型的完整程式設計架構，microsoft 開發幾乎任何類型的電腦應用程式。 ASP.NET 是專為建立 web 應用程式的.NET framework 組件。 開發人員使用 ASP.NET 來建立許多最大和最高流量網站的世界中。 (每當您看到的檔案名稱副檔名 *.aspx*中站台的 URL 的一部分，就會知道使用 ASP.NET 撰寫的網站。)

Razor 語法可讓您的 ASP.NET 中，但使用簡化的語法，可以更輕鬆地瞭解您是否專家如果您是新手，就能讓您更具生產力的所有功能。 即使這種語法用法很簡單，系列 ASP.NET 和.NET Framework 關係表示隨著您的網站變得更複雜的您會有較大的架構可供您的能力。

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **類別和執行個體**
> 
> ASP.NET server 程式碼會使用物件，會依次組建於類別的概念。 此類別是定義或範本物件。 例如，應用程式可能會包含`Customer`類別來定義的屬性和任何客戶物件所需的方法。
> 
> 當應用程式必須使用實際的客戶資訊時，它會建立的執行個體 (或*具現化*) customer 物件。 每個個別的客戶是獨立執行個體`Customer`類別。 每個執行個體支援相同的屬性和方法，但每個執行個體的屬性值是通常不同，因為每個客戶物件都是唯一。 一位客戶物件中`LastName`屬性可能是"Smith"，另一個客戶物件，在`LastName`屬性可能是"Jones"。
> 
> 同樣地，是在您的網站中任何個別網頁`Page`的執行個體的物件`Page`類別。 在頁面上的按鈕`Button`的執行個體的物件`Button`類別，並依此類推。 每個執行個體具有其本身的特性，但它們全都以基礎物件的類別定義中指定的內容。


## <a name="basic-syntax"></a>基本語法

先前您已看到如何建立 ASP.NET Web Pages 頁面，以及如何將伺服器程式碼新增至 HTML 標記的基本範例。 這裡您將了解撰寫使用 Razor 語法的 ASP.NET 伺服器程式碼的基本概念&#8212;也就是程式設計語言規則。

如果您是熟悉程式設計 （尤其是如果您已使用 C、 c + +、 C#、 Visual Basic 或 JavaScript），大多什麼您閱讀這裡應該不陌生。 您可能需要在自己熟悉只伺服端程式碼新增至標記中的如何 *.cshtml*檔案。

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>結合文字、 標記和程式碼區塊中的程式碼

在伺服器程式碼區塊，您通常會想要輸出的文字或標記 （或兩者） 頁面。 如果伺服器程式碼區塊包含的文字，不是程式碼，而是應轉譯為是，ASP.NET 必須能夠區別該文字與程式碼。 有幾個方式可做到這點。

- 在類似 HTML 元素括住文字`<p></p>`或`<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    HTML 項目可以包含文字、 其他 HTML 項目和伺服器程式碼運算式。 當 ASP.NET 看到 HTML 開頭標記 (例如`<p>`)，它會呈現所有項目包括項目且其內容做為解決伺服器程式碼運算式，因為它會在瀏覽器。
- 使用`@:`運算子或`<text>`項目。 `@:`輸出內容包含純文字] 或 [無對應的 HTML 標記。 單行`<text>`元素會括住要輸出的多行。 當您不想要呈現的 HTML 項目作為輸出的一部分，則這些選項會很有用。  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    如果您想要輸出多個文字行或不相符的 HTML 標記，您可以在前面上使用的每一行`@:`，或您可以用括號中的一行`<text>`項目。 像是`@:`運算子，`<text>`標記由 ASP.NET 識別的文字內容，而且永遠不會在網頁輸出中轉譯。

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    第一個範例會重複上一個範例，但是會使用一對`<text>`標記括住要轉譯的文字。 在第二個範例中，`<text>`並`</text>`標記括住三行，全部都有部分未包含的文字和不相符的 HTML 標記 (`<br />`)，以及伺服器程式碼和相符的 HTML 標記。 同樣地，您也無法在個別使用每一行`@:`運算子; 其中一個的方式運作。

    > [!NOTE]
    > 當您輸出文字，這一節所示&#8212;使用 HTML 元素，`@:`運算子，或`<text>`項目&#8212;ASP.NET 不進行 HTML 編碼的輸出。 (如先前所述，ASP.NET 未編碼的伺服器程式碼運算式和伺服器程式碼區塊，前面都會加上輸出`@`，除非在這一節所述的特殊案例。)

### <a name="whitespace"></a>Whitespace

額外的空格，陳述式 （內部和外部字串常值），就不會影響陳述式：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

陳述式中的分行符號陳述式上的沒有作用，而且您可以包裝陳述式，以提高可讀性。 下列陳述式都相同：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

不過，您無法包裝字串常值的中間一條線。 下列範例無法運作︰

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

若要結合換行至類似上述的程式碼的多行的長字串，有兩個選項。 您可以使用串連運算子 (`+`)，您將在本文稍後看到。 您也可以使用`@`建立逐字字串常值，如您稍早在本文中所見的字元。 您可以跨行中斷逐字字串常值：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>程式碼 （和標記） 的註解

註解可讓您為您自己或其他保留資訊。 它們也可讓您停用 (*標記為註解*) 程式碼或標記，您不想要執行，但想要保留您目前的頁面中的區段。

沒有其他註解的 Razor 程式碼和 HTML 標記的語法。 如同所有 Razor 程式碼中，Razor 註解會處理 （然後移除） 之前的網頁傳送到瀏覽器在伺服器上。 因此，Razor 註解語法可讓您將註解放在程式碼 （或甚至到標記），您可以看到當您編輯檔案，但使用者沒有看到，即使是在網頁原始檔。

ASP.NET Razor 註解，您開始的註解`@*`，且結尾必須與`*@`。 註解可以在一行或多行︰

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

以下是程式碼區塊中的註解：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

以下相同的程式碼區塊，使用程式碼行標記為註解，讓它不會執行：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

程式碼區塊中，若要使用 Razor 註解語法，或者您可以使用註解您使用，例如 C# 的程式設計語言的語法：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

在 C# 中，單行註解前面會加上`//`字元和多行註解開頭`/*`結尾`*/`。 （如同 Razor 註解，C# 註解不會轉譯至瀏覽器。）

標記，您可能也知道，您可以建立於 HTML 註解：

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

HTML 註解開頭`<!--`字元，並以結尾`-->`。 您可以使用 HTML 註解來括住的文字，不僅也任何 HTML 標記，您可能想要保留的頁面中，但不想要呈現。 此 HTML 註解會隱藏的標記和其所包含的文字的整個內容：

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

不同於 Razor 註解，HTML 註解*是*在網頁上呈現和使用者可以檢視網頁原始檔來查看它們。

Razor 會對 C# 的巢狀區塊中的限制。 如需詳細資訊，請參閱[名為 C# 變數和巢狀區塊產生中斷程式碼](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>變數

變數是您用來儲存資料的具名的物件。 您可以任何項目，名稱變數名稱必須以字母字元開頭但不能包含空格或保留的字元。

### <a name="variables-and-data-types"></a>變數和資料類型

變數可以有特定的資料類型，表示何種資料儲存在變數中。 您可以儲存字串值的字串變數 (例如&quot;Hello world&quot;)，儲存整數值 （例如 3 或 79） 的整數變數和日期值儲存在各種不同的格式 （例如 2012 年 4 月 12 日或 2009 年 3 月的日期變數). 而且有許多您可以使用其他資料型別。

不過，您通常不需要指定變數的類型。 大部分的情況下，ASP.NET 可以找出如何使用變數中的資料為基礎的類型。 （有時候您必須指定一個類型，您會看到範例即為 true）。

您宣告變數使用`var`關鍵字 （如果您不想要指定型別） 或使用型別名稱：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

下列範例會顯示變數的部分典型用途，在網頁中：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

如果您結合先前的範例，在頁面中，您會看到瀏覽器中顯示：

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>轉換和測試資料類型

雖然 ASP.NET 通常可以自動判斷資料型別，有時它不可轉換。 因此，您可能需要協助 ASP.NET 執行的明確轉換。 即使您沒有轉換的型別，有時候很有幫助測試以查看何種資料類型您可能會使用。

最常見的情況是，您必須將字串轉換成其他類型，例如整數或日期。 下列範例示範一般的情況下，您必須將字串轉換為數字。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

因此，使用者輸入來到您為字串。 即使您已提示使用者輸入數字，即使它們已送出使用者輸入和您在程式碼中讀取時所輸入數字、 資料的格式字串。 因此，您必須將字串轉換為數字。 在範例中，如果您嘗試對值執行算術運算，而不需要轉換，會產生下列錯誤，因為 ASP.NET 無法新增兩個字串：

*無法隱含地轉換成 'int'，' string' 類型。*

若要轉換成整數的值，請呼叫`AsInt`方法。 如果轉換成功，然後您可以加入數字。

下表列出一些常見的轉換和測試方法的變數。

:::row:::
    :::column:::
    <strong>方法</strong>
    :::column-end:::
    :::column:::
    <strong>描述</strong>
    :::column-end:::
    :::column:::
    <strong>範例</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
    將轉換成整數表示 （例如"593 」) 之間的整數的字串。
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
    將轉換的字串，例如&quot;，則為 true&quot;或是&quot;false&quot;布林型別。
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
    將具有類似的十進位值的字串轉換&quot;1.3&quot;或是&quot;7.439&quot;浮點數。
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
    將具有類似的十進位值的字串轉換&quot;1.3&quot;或是&quot;7.439&quot;十進位數字。 （在 ASP.NET 中，十進位數字是更精確比浮點數）。
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
    將 asp.net 代表的日期和時間值的字串轉換`DateTime`型別。
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
    將任何其他資料類型轉換為字串。
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>運算子

運算子是關鍵字或字元，會告訴 ASP.NET 在運算式中執行命令的類型。 C# 語言 （和 Razor 語法為基礎） 支援許多運算子，但您只需要識別一些開始使用。 下表摘要說明最常見的運算子。


:::row:::
    :::column:::
    <strong>Operator</strong>
    :::column-end:::
    :::column:::
    <strong>描述</strong>
    :::column-end:::
    :::column:::
    <strong>範例</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
    用在數值運算式的數學運算子。
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    指派。 將陳述式的右邊的值指派給左邊的物件中。
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    相等。 傳回`true`值是否相等。 (請注意區分`=`運算子和`==`運算子。)
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    不等。 傳回`true`值是否不相等。
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    較少-相比，大於-小於-或-等於、 與大於或等於比。
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    串連，用來聯結字串。 ASP.NET 會知道此運算子和運算式的資料類型的加法運算子之間的差異。
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
    遞增和遞減運算子，以新增和從變數 （分別） 減 1。
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
    點。 用來區別物件及其屬性和方法。
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    括號。 用來群組運算式，並將參數傳遞給方法。
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    方括號。 用來存取陣列或集合中的值。
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    不。 反轉`true`值`false`，反之亦然。 常用縮寫來測試`false`(也就是針對不`true`)。
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&&` <code>&#124;&#124;</code>
    :::column-end:::
    :::column:::
    邏輯 AND 和 OR，這用來連結條件一起。
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>使用檔案和程式碼中的資料夾路徑

您通常會在您的程式碼中使用檔案和資料夾的路徑。 以下是網站實體資料夾結構的範例，可能出現在您的開發電腦上：

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

以下是一些關於 Url 和路徑的基本詳細資料：

- 開始使用的網域名稱的 URL (`http://www.example.com`) 或伺服器名稱 (`http://localhost`， `http://mycomputer`)。
- URL 對應至主機電腦上的實體路徑。 例如，`http://myserver`可能會對應至資料夾*C:\websites\mywebsite*伺服器上。
- 虛擬路徑是以代表在程式碼中的路徑，而不需要指定完整路徑。 它包含的 url 的網域或伺服器名稱後面的部分。 當您使用虛擬路徑時，您可以移動至不同的網域或伺服器程式碼而不需要更新的路徑。

以下是範例，以協助您了解的差異：

| 完整的 URL | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| 伺服器名稱 | *mycompanyserver* |
| 虛擬路徑 | */humanresources/CompanyPolicy.htm* |
| 實體路徑 | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

虛擬根目錄是 /，就像根目錄的 c： 磁碟機 \。 （虛擬資料夾路徑永遠使用正斜線）。資料夾的虛擬路徑不需要有相同的名稱做為實體的資料夾;它可以是一個別名。 （在實際執行伺服器上的虛擬路徑很少比對確切的實體路徑。）

當您使用中的程式碼檔案和資料夾時，有時候您需要參考的實體路徑或按一下 虛擬路徑，視您正在使用哪些物件而定。 ASP.NET 讓您知道這些工具使用程式碼中的檔案和資料夾路徑：`Server.MapPath`方法，而`~`運算子和`Href`方法。

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>轉換虛擬與實體路徑： Server.MapPath 方法

`Server.MapPath`方法會將轉換的虛擬路徑 (例如 */default.cshtml*) 成絕對實體路徑 (例如*C:\WebSites\MyWebSiteFolder\default.cshtml*)。 每當您需要完整的實體路徑使用這個方法。 當您讀取或寫入的文字檔或 web 伺服器上的映像檔，就會是一個典型的例子。

您通常不知道您的網站裝載站台伺服器上的絕對實體路徑，因此這個方法可以將路徑轉換您知道 — 虛擬路徑，來為您在伺服器上對應的路徑。 您將虛擬路徑傳遞至檔案或資料夾的方法，並傳回的實體路徑：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>參考的虛擬根目錄： ~ 運算子和 Href 方法

在  *.cshtml*或 *.vbhtml*檔案中，您可以參考虛擬根目錄的路徑使用`~`運算子。 這是非常好用，因為您可以移動的頁面，在網站中，而且它們包含其他頁面的任何連結不會中斷。 如果您曾經將您的網站移至不同的位置還有好用。 以下是一些範例：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

如果網站是`http://myserver/myapp`，以下是 ASP.NET 如何處理此頁面會執行的這些路徑：

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

（您實際上不會看到這些路徑做為變數的值，但，是它們的是，ASP.NET 會將路徑）。

您可以使用`~`運算子伺服端程式碼 （如上所述） 並在標記中，像這樣：

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

在標記中，您可以使用`~`運算子來建立資源，例如映像檔案、 其他網頁和 CSS 檔案的路徑。 ASP.NET 頁面執行時，尋找整個頁面 （程式碼和標記），並解決所有`~`參考適當的路徑。

## <a name="conditional-logic-and-loops"></a>條件式邏輯和迴圈

ASP.NET server 程式碼可讓您執行以條件為基礎的工作，並撰寫重複陳述式的程式碼次數 （也就是程式碼會執行迴圈）。

### <a name="testing-conditions"></a>測試條件

若要測試您所使用的簡單條件`if`陳述式，此傳回 true 或 false 取決於您所指定的測試：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

`if`關鍵字開始區塊。 實際的測試 （條件） 是括號括住，並傳回 true 或 false。 執行測試結果為 true 的陳述式會括在大括號。 `if`陳述式可包含`else`，指定當條件為 false 時要執行的陳述式區塊：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

您可以加入多個條件使用`else if`區塊：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

在此範例中，如果第一個條件，在 if 區塊不是 true，`else if`在檢查條件。 如果符合該條件中的陳述式`else if`區塊會執行。 如果條件符合中的陳述式`else`區塊會執行。 您可以新增任意數目的 else 的 if 區塊中使用，並關閉與`else`封鎖&quot;一切&quot;條件。

若要測試大量的條件，請使用`switch`區塊：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

要測試的值位於括號內 (在範例中，`weekday`變數)。 每個個別的測試會使用`case`陳述式的結尾是冒號 （:）。 如果值`case`陳述式符合測試值，該案例區塊中的程式碼會執行。 關閉與每個 case 陳述式`break`陳述式。 (如果您忘記在每個包含中斷`case`封鎖，請從下一個程式碼`case`陳述式也會執行。)A`switch`區塊通常會有`default`陳述式的最後一個案例作為&quot;一切&quot;執行如果沒有任何其他情況下，則為 true 的選項。

在瀏覽器中顯示的最後兩個條件式區塊的結果：

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>迴圈的程式碼

您通常需要重複執行相同的陳述式。 您可以進行迴圈。 例如，您經常執行相同的陳述式，每個項目集合中的資料。 如果您知道確實多少次您想要執行迴圈，您可以使用`for`迴圈。 這種迴圈是特別適用於計算或向下計數：

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

迴圈開頭`for`每個以分號結束關鍵字，後面接著括號中的三個陳述式。

- 在括弧內，第一個陳述式 (`var i=10;`) 建立計數器，並且將它初始化為 10。 您不需要名稱計數器`i`&#8212;您可以使用任何變數。 當`for`迴圈執行時，會自動遞增的計數器。
- 第二個陳述式 (`i < 21;`) 設定條件，以您想要計算的幅度。 您想在此情況下，請移至最多 20 個 （也就繼續執行時的計數器是小於 21）。
- 第三個陳述式 (`i++` ) 會使用遞增運算子，只需指定 計數器應有 1 加入至它在每次執行迴圈。

括號內是迴圈的每個反覆項目，就會執行的程式碼。 標記會建立新的段落 (`<p>`項目) 每次，且輸出中顯示的值中加入一行`i`（計數器）。 當您執行此頁面時，此範例會建立 11 行顯示輸出，表示項目數目每一行中的文字。

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

如果您正在使用集合或陣列，您經常使用`foreach`迴圈。 集合是一組類似的物件，而`foreach`迴圈可讓您執行的工作集合中的每個項目上。 這種類型的迴圈是方便的集合，因為不像`for`迴圈中，您不需要遞增計數器，或設定限制。 相反地，`foreach`迴圈的程式碼會繼續透過集合直到完成為止。

例如，下列程式碼會傳回中的項目`Request.ServerVariables`集合，也就是物件，包含 web 伺服器的相關資訊。 它會使用`foreac`h 的迴圈，以顯示每個項目的名稱來建立新`<li>`HTML 項目符號清單中的項目。

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

`foreach`關鍵字後面接著括號您用來宣告變數，表示集合中的單一項目 (在範例中， `var item`)，後面接著`in`關鍵字，後面接著您要循環的集合。 本文的`foreach`迴圈中，您可以存取目前的項目使用您稍早宣告的變數。

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

若要建立更通用的迴圈，請使用`while`陳述式：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

A`while`迴圈開頭`while`關鍵字，後面接著括號，您可以指定迴圈繼續的時間長度 (這裡，只要`countNum`小於 50)，然後重複的區塊。 迴圈通常遞增 （加入） 或遞減 （減去） 的變數或用於計算的物件。 在範例中，`+=`運算子會加入至 1`countNum`每次執行迴圈。 (以遞減的向下計數的迴圈中的變數，您會使用遞減運算子`-=`)。

## <a name="objects-and-collections"></a>物件和集合

幾乎在 ASP.NET 網站中所有項目是物件，包括網頁本身。 本章節將討論一些重要的物件，您將使用經常在您的程式碼。

### <a name="page-objects"></a>頁面物件

在 ASP.NET 中最基本的物件是頁面。 您可以存取任何合格的物件不直接的 page 物件的屬性。 下列程式碼會取得頁面的檔案路徑，使用`Request`頁面物件：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

要取消選取您要參考的屬性和目前的頁面物件上的方法，您可以選擇性地使用關鍵字`this`來代表您的程式碼中的頁面物件。 以下是上述的程式碼範例中，使用`this`新增至代表的頁面：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

您可以使用屬性`Page`物件，以取得大量資訊，例如：

- `Request`. 如您所見，這是目前的要求，包括進行要求的頁面、 使用者身分識別等 URL 的瀏覽器的何種類型的相關資訊的集合。
- `Response`. 這是會在伺服器程式碼執行完成時傳送到瀏覽器的回應 （頁面） 的相關資訊的集合。 例如，您可以使用此屬性寫入至回應的資訊。 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>（陣列和字典） 的集合物件

A*集合*是一組相同的類型，例如的集合物件`Customer`資料庫的物件。 ASP.NET 包含許多的內建集合，例如`Request.Files`集合。

您通常會使用在集合中的資料。 兩個常見的集合類型*陣列*並*字典*。 當您想要儲存一組類似的項目，但不想要建立個別的變數來保存每個項目時，陣列是很有用：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

使用陣列時，您宣告特定的資料類型，例如`string`， `int`，或`DateTime`。 若要指出變數可以包含陣列，您將加入宣告的方括號 (例如`string[]`或`int[]`)。 您可以存取項目中使用它們的位置 （索引） 的陣列，或使用`foreach`陳述式。 陣列索引以零為起始&#8212;也就是第一個項目是在位置 0，第二個項目位於位置 1，依此類推。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

您可以判斷陣列中的項目數，藉由取得其`Length`屬性。 若要取得特定項目的陣列中 （如果您要搜尋的陣列） 的位置，請使用`Array.IndexOf`方法。 您也可以執行像是反向陣列的內容 (`Array.Reverse`方法) 或排序內容 (`Array.Sort`方法)。

在瀏覽器中顯示的字串陣列程式碼的輸出：

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

字典是索引鍵/值組的集合，您可在此提供金鑰 （或名稱），以設定或擷取對應的值：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

若要建立字典，您使用`new`關鍵字指出您要建立新的字典物件。 您可以將字典指派給變數使用`var`關鍵字。 指出資料類型的字典使用角括弧中的項目 ( `< >` )。 宣告的結尾，在中，您必須新增一組括號，因為這是實際的方法，建立新的字典。

若要新增至字典的項目，您可以呼叫`Add`字典變數的方法 (`myScores`在此情況下)，然後指定 索引鍵和值。 或者，您可以使用方括號表示索引鍵，並執行簡單的指派，如下列範例所示：

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

若要從字典取得值，指定金鑰括號括住：

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>呼叫具有參數的方法

如同您稍早在本文中，您使用程式設計物件的方法。 例如，`Database`物件可能會有`Database.Connect`方法。 許多的方法也會有一或多個參數。 A*參數*是一個值傳遞至方法，若要啟用以完成其工作的方法。 例如，看看的宣告`Request.MapPath`方法，後者會採用三個參數：

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

（行已換行，讓它更容易閱讀。 請記住，您可以將幾乎任何放置除了內部字串換行符號包含在引號中。)

這個方法會傳回對應的伺服器上的實體路徑，以指定的虛擬路徑。 方法的三個參數都`virtualPath`， `baseVirtualDir`，和`allowCrossAppMapping`。 （請注意，在宣告中，列出參數的資料類型，它們會接受的資料）。當您呼叫這個方法時，您必須提供所有的三個參數的值。

Razor 語法可讓您將參數傳遞至方法的兩個選項：*位置參數*並*具名參數*。 若要呼叫使用位置參數的方法，您會以嚴格的順序，指定在方法宣告中傳遞參數。 （您將通常知道此順序，請閱讀文件的方法）。您必須遵循的順序，以及您不能略過的任何參數&#8212;如果有必要，您傳遞空字串 (`""`) 或`null`您不需要的值為位置參數。

下列範例假設您有一個名為的資料夾*指令碼*在您的網站。 程式碼會呼叫`Request.MapPath`方法，並傳遞正確的順序的三個參數值。 然後，它會顯示產生的對應的路徑。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

當方法有許多參數時，您可以使用具名參數，讓您的程式碼更容易閱讀。 若要呼叫方法，使用具名的參數，您可以指定參數名稱後面接著冒號 （:），然後按一下 值。 具名參數的優點是，您就可以將其傳遞任何您想要的順序。 （一項缺點是，在方法呼叫並不會精簡）。

下列範例會呼叫與上述相同的方法，但使用具名參數，以提供值：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

如您所見，參數會傳遞不同的順序。 不過，如果您執行先前的範例和此範例中，它們會傳回相同的值。

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>處理錯誤

### <a name="try-catch-statements"></a>Try Catch 陳述式

陳述式通常必須在程式碼中可能會失敗的原因，您無法控制。 例如: 

- 如果您的程式碼會嘗試建立或存取的檔案，可能會發生各種錯誤。 您想要的檔案可能不存在，它可能會遭到鎖定，程式碼可能不具有權限，等等。
- 同樣地，如果您的程式碼會嘗試更新資料庫中的記錄，可以有權限問題，可能會卸除資料庫的連接，要儲存的資料可能無效，依此類推。

在程式設計的詞彙中，這些情況下則稱為*例外狀況*。 如果您的程式碼發生例外狀況，則會產生 （擲回） 的錯誤訊息，最惱人的使用者：

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

在您的程式碼可能會遇到例外狀況的情況下，並以避免此類型的錯誤訊息，您可以使用`try/catch`陳述式。 在 `try`陳述式中，執行您的程式碼。 在一或多個`catch`陳述式，您可以尋找特定可能發生的錯誤 （特定類型的例外狀況）。 您可以包含更多`catch`陳述式，為您要尋找您預期的錯誤。

> [!NOTE]
> 我們建議您避免使用`Response.Redirect`方法中的`try/catch`陳述式，因為它會在網頁中造成例外狀況。


下列範例顯示建立第一個要求上的文字檔案，然後顯示按鈕，讓使用者可以開啟檔案的頁面。 此範例刻意使用不正確的檔案名稱，如此會導致例外狀況。 程式碼包含`catch`陳述式中的兩個可能的例外狀況： `FileNotFoundException`，就會出現錯誤，檔案名稱是否與`DirectoryNotFoundException`，發生於 ASP.NET 即使找不到資料夾。 （您可以取消此範例中的陳述式以查看一切正常運作時，它的執行方式註解）。

如果您的程式碼未處理例外狀況，您會看到先前的螢幕擷取畫面類似的錯誤頁面。 不過，`try/catch`一節可協助避免使用者看到這些錯誤類型。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>其他資源

**使用 Visual Basic 進行程式設計**


[附錄： Visual Basic 語言和語法](https://go.microsoft.com/fwlink/?LinkId=202908)


**參考文件**


[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[C# 語言](https://msdn.microsoft.com/library/kx37x362.aspx)
