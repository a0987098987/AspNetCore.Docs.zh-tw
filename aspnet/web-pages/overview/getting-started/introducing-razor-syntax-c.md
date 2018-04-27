---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: 使用 Razor 語法 (C#) 的 ASP.NET Web 程式設計簡介 |Microsoft 文件
author: tfitzmac
description: 本章您可利用程式設計的概觀與 ASP.NET Web 網頁使用 Razor 語法。 ASP.NET 是 Microsoft 的技術，用於執行動態 web 的 pa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: 48f49f40a6fc0c6a0c664873879f9f61080132ea
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>使用 Razor 語法 (C#) 的 ASP.NET Web 程式設計簡介
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文提供您進行程式設計的概觀與 ASP.NET Web 網頁使用 Razor 語法。 ASP.NET 是 Microsoft 的技術，用於 web 伺服器上執行動態網頁。 此文件會著重於使用 C# 程式設計語言。
> 
> **您將學習**:
> 
> - 前 8 的程式設計入門程式設計使用 Razor 語法的 ASP.NET Web Pages 的提示。
> - 您需要的基本程式設計概念。
> - ASP.NET server 程式碼和 Razor 語法與所有有關。
>   
> 
> ## <a name="software-versions"></a>軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2。


## <a name="the-top-8-programming-tips"></a>最上層的 8 個程式設計提示

此區段會列出您一定要知道當您開始撰寫為使用 Razor 語法的 ASP.NET server 程式碼需要的一些秘訣。

> [!NOTE]
> 根據 Razor 語法的 C# 程式語言中，且最常使用以 ASP.NET Web Pages 語言。 不過，Razor 語法也支援 Visual Basic 語言，以及您會看到您也可以執行在 Visual Basic 中的所有項目。 如需詳細資訊，請參閱 < 附錄[Visual Basic 語言和語法](https://go.microsoft.com/fwlink/?LinkId=202908)。


您可以找到更多詳細的這些程式設計技術的大部分在本文稍後。

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1.您可以將程式碼加入頁面使用 @ 字元

`@`字元開始內嵌運算式、 單一陳述式區塊和多重陳述式區塊：

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

這是什麼這些陳述式看起來會像在網頁瀏覽器中執行：

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **HTML 編碼**
> 
> 當您在頁面上，使用顯示內容`@`的字元，如同上述範例中，ASP.NET 將 HTML 編碼的輸出。 這會取代保留的 HTML 字元 (例如`<`和`>`和`&`) 與啟用顯示網頁，而不會轉譯為 HTML 標記或實體中的字元為字元的代碼。 而不進行 HTML 編碼，可能不正確，會顯示您伺服器的程式碼的輸出，並可能會暴露於安全性風險的頁面。
> 
> 如果您的目標是要輸出轉譯為標記的標記的 HTML 標記 (例如`<p></p>`段落或`<em></em>`來強調文字)，請參閱節[結合文字、 標記和程式碼區塊中的程式碼](#BM_CombiningTextMarkupAndCode)本文稍後。
> 
> 閱讀更多有關中的 HTML 編碼[使用表單](https://go.microsoft.com/fwlink/?LinkId=202892)。


### <a name="2-you-enclose-code-blocks-in-braces"></a>2.您將程式碼區塊括在大括號

A*程式碼區塊*包含一或多個程式碼陳述式，且括在大括弧。

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

在瀏覽器中顯示結果：

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3.區塊中，您最後以分號的每個程式碼陳述式

程式碼區塊中，每個完整的程式碼陳述式必須以分號結束。 內嵌運算式不以分號結束。

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4.您可以使用變數來儲存值

您可以儲存在值*變數*，包括字串、 數字和日期等。您建立新變數使用`var`關鍵字。 您可以直接在頁面上，使用插入變數值`@`。

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

在瀏覽器中顯示結果：

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5.您將常值字串值括在雙引號中

A*字串*是會被視為文字的字元序列。 若要指定字串，您將它括在雙引號中：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

如果您想要顯示的字串包含反斜線字元 ( `\` ) 或雙引號 ( `"` )，使用*逐字字串常值*，有前置`@`運算子。 (在 C# 中，\ 字元有特殊意義，除非您使用逐字字串常值。)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

若要將內嵌雙引號，請使用逐字字串常值和重複引號：

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

在網頁中使用兩個範例的結果如下：

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> 請注意，`@`字元用來標示在 C# 中的逐字字串常值和標記中的 ASP.NET 網頁的程式碼。


### <a name="6-code-is-case-sensitive"></a>6.程式碼會區分大小寫

在 C# 中，關鍵字 (像是`var`， `true`，和`if`) 和變數名稱不區分大小寫。 下列程式碼會建立兩個不同的變數，`lastName`和 `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

如果您將變數宣告為`var lastName = "Smith";`，如果您嘗試參考該變數在頁面中為`@LastName`，錯誤結果，因為`LastName`無法辨識。

> [!NOTE]
> 在 Visual Basic 中的關鍵字而變數則屬於*不*區分大小寫。


### <a name="7-much-of-your-coding-involves-objects"></a>7.大部分的程式碼牽涉到物件

*物件*表示您可以使用程式設計的事情&#8212;頁面、 文字方塊、 檔案、 影像、 web 要求、 電子郵件訊息、 客戶記錄 （資料庫資料列）、 等等。物件具有描述其特性的屬性，您可以讀取或變更&#8212;文字方塊物件具有`Text`（和其他項目） 的屬性，要求物件具有`Url`屬性，電子郵件訊息有`From`屬性，客戶物件都有和`FirstName`屬性。 物件也有方法&quot;動詞&quot;他們可以執行。 範例包括檔案物件的`Save`方法中，映像物件的`Rotate`方法，以及電子郵件物件的`Send`方法。

您通常會使用`Request`物件，例如值的文字方塊 （表單欄位） 的資訊讓您在頁面上，瀏覽器類型提出要求，頁面、 使用者識別、 等等的 URL。下列範例示範如何存取的屬性`Request`物件以及如何呼叫`MapPath`方法`Request`物件，可讓您在伺服器的頁面上的絕對路徑：

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

在瀏覽器中顯示結果：

![Razor Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8.您可以撰寫程式碼所做的決策

動態網頁的主要功能是您可以判斷如何處理根據條件。 若要這樣做最常見的方法是使用`if`陳述式 (和選擇性`else`陳述式)。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

陳述式`if(IsPost)`會撰寫以快速`if(IsPost == true)`。 連同`if`陳述式，有多種方式來測試條件，重複程式碼區塊，以及等等，這是本文稍後所述。

在瀏覽器中顯示的結果 (按一下後**送出**):

![Razor Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET 和 POST 方法，以及 IsPost 屬性
> 
> 用於網頁 (HTTP) 的通訊協定支援很有限的數目的方法 （動詞命令），可用來對伺服器提出要求。 兩個最常見的是，用來讀取的頁面，以及 POST，用來提交頁面。 一般情況下，使用者要求網頁，第一次要求頁面時利用 GET 輕鬆得到。 如果使用者填滿表單中，然後按一下 [提交] 按鈕，瀏覽器進行 POST 要求到伺服器。
> 
> 中的 web 程式設計，通常會很有用知道是否正在要求頁面為 GET 或 POST，以了解如何處理頁面。 ASP.NET Web Pages 中，您可以使用`IsPost`屬性來查看是否有 GET 或 POST 要求。 如果要求是使用 post 要求`IsPost`屬性會傳回 true，而且您可以執行像是讀取表單上的文字方塊的值。 您會看到的許多範例會示範如何處理不同的值而定頁面`IsPost`。


## <a name="a-simple-code-example"></a>簡單的程式碼範例

此程序會示範如何建立可說明基本的程式設計技術的頁面。 在範例中，您可以建立可讓使用者輸入兩個數字，然後將它們加入，並顯示結果頁面。

1. 在編輯器中，建立新的檔案並將其命名*AddNumbers.cshtml*。
2. 複製下列程式碼和標記到頁面上，取代已經在頁面中的任何項目。  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    以下是您必須注意的一些事項：

    - `@`字元在頁面中，啟動程式碼的第一個區塊，而且之前`totalMessage`內嵌頁面底部附近的變數。
    - 以大括號括住位於頁面頂端的區塊。
    - 在頂端區塊中，所有線條會以分號都結束。
    - 變數`total`， `num1`， `num2`，和`totalMessage`儲存幾個數字和字串。
    - 常值字串值指派給`totalMessage`變數是置於雙引號之間。
    - 由於程式碼時區分大小寫，`totalMessage`頁面底部附近使用變數，其名稱必須完全符合在最上方的變數。
    - 運算式`num1.AsInt() + num2.AsInt()`示範如何使用物件和方法。 `AsInt`上每個變數的方法會將轉換的使用者輸入的數字 （整數），讓您可以在其上執行算術運算的字串。
    - `<form>`標記包含`method="post"`屬性。 這會指定當使用者按一下**新增**，頁面將會傳送至使用 HTTP POST 方法的伺服器。 當提交頁面時，`if(IsPost)`測試評估為 true，條件式程式碼執行時，顯示加上數字的結果。
3. 儲存頁面，並在瀏覽器中執行。 (請確定中選取頁面**檔案**才能執行這個工作區。)兩個整數的輸入，然後按一下**新增** 按鈕。 

    ![Razor Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>基本程式設計概念

本文章會提供您的 ASP.NET web 程式設計概觀。 它不是獨占檢查，只要快速瀏覽您最常使用的程式設計概念。 即便如此，它涵蓋了幾乎所有項目必須開始使用 ASP.NET Web Pages。

但首先，很少的技術背景。

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Razor 語法、 伺服端程式碼和 ASP.NET

Razor 語法是簡單的程式設計語法，在網頁中內嵌伺服器端程式碼。 在網頁使用 Razor 語法，有兩種類型的內容： 用戶端內容和伺服器程式碼。 用戶端內容是您要用來在網頁中的內容： HTML 標記 （項目） 樣式資訊，例如 CSS，或許某些用戶端指令碼，例如 JavaScript 和純文字。

Razor 語法可讓您將伺服器程式碼加入此用戶端內容。 如果頁面沒有伺服端程式碼，伺服器會執行該程式碼首先，它將頁面傳送至瀏覽器之前。 在伺服器上執行，程式碼可以執行可能要使用用戶端內容本身，例如存取伺服器端資料庫更加複雜的工作。 最重要的是，伺服端程式碼可以動態建立的用戶端內容&#8212;可以產生的 HTML 標記或其他內容即時，然後將它傳送到連同頁面可能會包含任何靜態 HTML 瀏覽器。 瀏覽器的觀點來看，由您的伺服器程式碼所產生的用戶端內容並無不同於其他任何用戶端內容。 如您所見，有需要的伺服器程式碼是相當簡單。

包含 Razor 語法的 ASP.NET web pages 有特殊的副檔名 (*.cshtml*或 *.vbhtml*)。 伺服器會辨識這些擴充功能，執行含有 Razor 語法標記，再將網頁傳送到瀏覽器的程式碼。

### <a name="where-does-aspnet-fit-in"></a>ASP.NET 納入其中？

Razor 語法根據 microsoft 呼叫 ASP.NET，根據 Microsoft.NET Framework 技術。 .Net Framework 是大型的完整程式設計架構，microsoft 開發幾乎任何類型的電腦應用程式。 ASP.NET 是專為建立 web 應用程式的.NET Framework 的一部分。 開發人員有 ASP.NET 中用來建立許多最大和最高流量網站世界。 (每當您看到的檔案名稱副檔名 *.aspx*中站台的 URL 的一部分，您就知道網站使用 ASP.NET 撰寫。)

Razor 語法可讓您的 ASP.NET 中，但使用簡化的語法更輕鬆地了解如果初學者，能讓您更具生產力是否您已熟悉的所有功能。 即使此語法很簡單，ASP.NET 和.NET Framework 系列關聯性表示由於您的網站，變得更複雜，您會有較大的架構可供您使用的電源。

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **類別和執行個體**
> 
> ASP.NET server 程式碼會使用物件，接著會根據類別的概念。 此類別是定義或範本物件。 例如，應用程式可能包含`Customer`定義的屬性和方法所需的任何客戶物件的類別。
> 
> 當應用程式必須使用實際的客戶資訊時，它會建立的執行個體 (或*具現化*) 客戶物件。 每個個別的客戶是獨立執行個體`Customer`類別。 每個執行個體支援相同的屬性和方法，但是每個執行個體的屬性值通常不同，因為每個客戶物件都是唯一。 在一個客戶物件、`LastName`屬性可能是"Smith"; 在另一個客戶物件、`LastName`屬性可能是"Jones。 」
> 
> 同樣地，在您的網站中的任何個別網頁是`Page`的執行個體的物件`Page`類別。 頁面上的按鈕是`Button`的執行個體的物件`Button`類別，依此類推。 每個執行個體都有它自己的特性，但所有它們所根據的物件類別定義中指定的內容。


## <a name="basic-syntax"></a>基本語法

您稍早已如何建立 ASP.NET Web Pages 頁面上，以及如何將伺服器程式碼加入至 HTML 標記的基本範例。 這裡您將了解撰寫為使用 Razor 語法的 ASP.NET server 程式碼的基本概念&#8212;也就是程式設計語言規則。

如果您熟悉程式設計 （特別是如果您使用 C、 c + +、 C#、 Visual Basic 或 JavaScript），大部分的讀取這裡都很熟悉。 您可能需要在您自己熟悉只伺服端程式碼加入至標記中的方式 *.cshtml*檔案。

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>結合文字、 標記和程式碼區塊中的程式碼

在伺服器程式碼區塊，您通常想要輸出的文字或標記 （或兩者） 頁面。 如果伺服器程式碼區塊包含的文字不是程式碼，而是應轉譯為是，ASP.NET 必須能從程式碼中區分該文字。 有幾個方式可做到這點。

- HTML 項目，例如括住文字`<p></p>`或`<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    HTML 項目可以包含文字、 其他 HTML 項目和伺服器程式碼運算式。 當 ASP.NET 會看到開頭 HTML 標記 (例如， `<p>`)，它會呈現所有項目包括項目且其內容，做為解決伺服器程式碼運算式，因為它會在瀏覽器。
- 使用`@:`運算子或`<text>`項目。 `@:`輸出內容包含純文字或無對應的 HTML 標記; 單行`<text>`元素會括住多個輸出行。 當您不想要呈現的 HTML 項目做為輸出的一部分，這些選項會是很有用。  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    如果您想要輸出的文字或無對應的 HTML 標記的多行，您可以在每一行與`@:`，或您可以用括號中的一行`<text>`項目。 像`@:`運算子，`<text>`標記由 ASP.NET 用來識別文字內容，並永遠不會在網頁輸出中轉譯。

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    第一個範例會重複前一個範例，但是會使用一對`<text>`標記來括住要呈現的文字。 在第二個範例中，`<text>`和`</text>`標記括住三行，其中有部分未包含的文字和無對應的 HTML 標記 (`<br />`) 以及伺服端程式碼，而相符的 HTML 標記。 同樣地，您也可以在每一行，以個別`@:`運算子; 任一種方式運作。

    > [!NOTE]
    > 當您在輸出文字這一節中所示&#8212;使用 HTML 項目，`@:`運算子，或`<text>`元素&#8212;ASP.NET 並不進行 HTML 編碼的輸出。 (如前文所述，ASP.NET 未編碼的伺服器程式碼運算式和伺服器程式碼區塊，前面加上輸出`@`，除非在這一節所述的特殊案例。)

### <a name="whitespace"></a>Whitespace

陳述式中 （且字串常值之外） 的額外空間不會影響陳述式：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

陳述式中的分行符號陳述式上的沒有作用，而且您可以包裝陳述式的可讀性。 下列陳述式都相同：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

不過，您無法包裝一條線中間的字串常值。 下列範例將無法運作：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

若要將長字串為多行，如上述程式碼包裝，有兩個選項。 您可以使用串連運算子 (`+`)，您會看到在本文稍後。 您也可以使用`@`建立逐字字串常值，如同稍早在本文章中的字元。 您可以中斷逐字字串常值到下一行：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>程式碼 （和標記） 的註解

註解可讓您保留供您自己或其他資訊。 它們也可讓您停用 (*標記為註解*) 一段程式碼或您不想要執行，但想要保留在目前頁面的標記。

沒有其他註解的 Razor 程式碼和 HTML 標記的語法。 如同所有的 Razor 程式碼 Razor 註解會處理 （然後移除） 之前在網頁傳送到瀏覽器在伺服器上。 因此，Razor 註解語法可讓您將註解放在程式碼 （或甚至標記），您可以看到時編輯檔案，但使用者沒有看到，即使是在網頁原始檔。

ASP.NET Razor 註解，對於您開始使用的註解`@*`，且結尾必須與`*@`。 註解可以在上一行或多行：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

以下是程式碼區塊中的註解：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

以下相同的程式碼區塊，透過程式碼行標記為註解，讓它不會執行：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

程式碼區塊中，使用 Razor 註解語法，或者您可以使用註解您正在使用，例如 C# 的程式設計語言的語法：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

在 C# 中，單行註解的前面都有`//`字元和多行註解的開頭`/*`，而結尾`*/`。 （如同 Razor 註解，C# 註解不會轉譯至瀏覽器。）

標記，您可能知道，您可以建立 HTML 註解：

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

HTML 註解的開頭`<!--`字元，且結尾`-->`。 您可以使用 HTML 註解文字，不過也任何 HTML 標記，您可能想要保留的頁面中，但不想要呈現不僅括住。 這個 HTML 註解將會隱藏整個內容，標記和其所包含的文字：

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

不同於 Razor 註解，HTML 註解*是*呈現在網頁和使用者可以看到它們藉由檢視網頁原始檔。

Razor 的 C# 的巢狀區塊有一些限制。 如需詳細資訊，請參閱[名為 C# 變數和巢狀區塊產生中斷程式碼](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>變數

變數是您用來儲存資料的具名的物件。 您可以為變數命名任何項目，但是名稱必須以字母字元開頭，而且不能包含空白字元或保留的字元。

### <a name="variables-and-data-types"></a>變數和資料類型

變數可以有特定資料類型，表示何種資料儲存在變數中。 您可以儲存字串值的字串變數 (例如&quot;Hello world&quot;)，儲存整數值 （例如 3 或 79） 的整數變數和日期值儲存在不同的格式 （例如 2012 年 4 月 12 日或 2009 年 3 月的日期變數). 而且還有許多其他您可以使用的資料類型。

不過，您通常不必指定類型的變數。 大部分的情況下，ASP.NET 可以找出如何使用變數中的資料為基礎的類型。 （有時候您必須指定一個類型，您會看到範例這是 true）。

您宣告變數使用`var`關鍵字 （如果您不想要指定型別） 或使用類型的名稱：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

下列範例會顯示在網頁中變數的部分典型用途：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

如果您結合中頁面的上一個範例，您會看到瀏覽器中顯示：

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>轉換和測試資料類型

雖然 ASP.NET 通常可以自動判斷的資料型別，有時無法。 因此，您可能需要幫助 ASP.NET 執行的明確轉換。 即使您沒有將類型轉換，有時候很有幫助測試以查看何種資料類型您可能使用。

最常見的情況是，您必須將字串轉換成其他類型，例如整數或日期。 下列範例顯示一般的情況下，您必須將字串轉換成數字。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

一般而言，使用者輸入都是在您為字串。 即使您已提示使用者輸入數字，而且即使他們已經提交使用者輸入時，而您閱讀程式碼中，輸入數字，資料字串格式。 因此，您必須將字串轉換為數字。 在範例中，如果您嘗試執行算術運算的值，而不需要轉換，會產生下列錯誤，因為 ASP.NET 無法新增兩個字串：

*無法隱含地轉換成 'int'，' string' 類型。*

若要將值轉換成整數，您呼叫`AsInt`方法。 如果轉換成功，然後您可以加入數字。

下表列出一些常見的轉換和測試方法的變數。

::: 資料列:::::: 資料行:::<strong>方法</strong>::: 資料行結尾:::::: 資料行:::<strong>描述</strong>::: 資料行結尾:::::: 資料行:::<strong>範例</strong>::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `AsInt(), IsInt()` ::: 資料行結尾:::::: 資料行::: 轉換字串，表示為整數 （例如"593 」) 之間的整數。
::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `AsBool(), IsBool()` ::: 資料行結尾:::::: 資料行::: 轉換字串 like &quot;true&quot;或&quot;false&quot;布林型別。
::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `AsFloat(), IsFloat()` ::: 資料行結尾:::::: 資料行::: 將具有類似的十進位值的字串轉換&quot;1.3&quot;或&quot;7.439&quot;浮點數。
::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `AsDecimal(), IsDecimal()` ::: 資料行結尾:::::: 資料行::: 將具有類似的十進位值的字串轉換&quot;1.3&quot;或&quot;7.439&quot;為十進位數字。 （在 ASP.NET、 十進位數字是更精確比浮點數）。::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `AsDateTime(), IsDateTime()` ::: 資料行結尾:::::: 資料行::: 將 asp.net 代表日期和時間值的字串轉換`DateTime`型別。
::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `ToString()` ::: 資料行結尾:::::: 資料行::: 將其他任何資料類型轉換為字串。
::: 資料行結尾:::::: 資料行::: [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    ::: 資料行結尾:::::: 資料列結束:::

## <a name="operators"></a>運算子

運算子是命令的關鍵字或字元，會告知 ASP.NET 何種在運算式中執行。 C# 語言 （和 Razor 語法為基礎） 支援許多運算子，但您只需要辨識一些開始。 下表摘要說明最常見的運算子。


::: 資料列:::::: 資料行:::<strong>運算子</strong>::: 資料行結尾:::::: 資料行:::<strong>描述</strong>::: 資料行結尾:::::: 資料行:::<strong>範例</strong>::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `+` `-` `*` `/` ::: 資料行結尾:::::: 資料行::: 數學運算子用在數值運算式。
::: 資料行結尾:::::: 資料行::: [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `=` ::: 資料行結尾:::::: 資料行::: 指派。 將陳述式右邊的值指派給左邊的物件。
::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `==` ::: 資料行結尾:::::: 資料行::: 等號比較。 傳回`true`值是否相等。 (請注意區分`=`運算子和`==`運算子。)::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `!=` ::: 資料行結尾:::::: 資料行::: 不等比較。 傳回`true`值是否不相等。
::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `< > <= >=` ::: 資料行結尾:::::: 資料行::: 較少-相比，大於-小於-比-或-等於、 與大於或等於比。
::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `+` ::: 資料行結尾:::::: 資料行::: 用來將字串的串連。 ASP.NET 知道這個運算子和運算式的資料型別，加法運算子之間的差異。
::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `+=` `-=` ::: 資料行結尾:::::: 資料行::: 遞增和遞減運算子，其中加入和減去 1 （分別） 變數。
::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `.` ::: 資料行結尾:::::: 資料行::: 點。 用來區別物件及其屬性和方法。
::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `()` ::: 資料行結尾:::::: 資料行::: 括號。 用來群組運算式，並將參數傳遞給方法。
::: 資料行結尾:::::: 資料行::: [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `[]` ::: 資料行結尾:::::: 資料行::: 方括號。 用來存取陣列或集合中的值。
::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `!` ::: 資料行結尾:::::: 資料行::: 沒有。 反轉`true`值設定為`false`，反之亦然。 一般而言，若要測試的簡略方法做`false`(也就是針對不`true`)。
::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    ::: 資料行結尾:::::: 資料列結束:::
* * *
::: 資料列:::::: 資料行::: `&&` <code>&#124;&#124;</code> ::: 資料行結尾:::::: 資料行::: 邏輯和及用來連結條件在一起。
::: 資料行結尾:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    ::: 資料行結尾:::::: 資料列結束:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>使用檔案和程式碼中的資料夾路徑

您通常將程式碼中使用檔案和資料夾路徑。 以下是網站的實體資料夾結構的範例，就可能會出現在開發電腦上：

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

以下是一些基本的詳細 Url 和路徑：

- 開始使用的網域名稱的 URL (`http://www.example.com`) 或伺服器名稱 (`http://localhost`， `http://mycomputer`)。
- URL 對應至主機電腦上的實體路徑。 例如，`http://myserver`可能對應至資料夾*C:\websites\mywebsite*在伺服器上。
- 虛擬路徑是代表程式碼中的路徑，而不必指定完整路徑的縮寫。 它包含的 url 的網域或伺服器名稱後面的部分。 當您使用的虛擬路徑時，您可以移動至不同的網域或伺服器的程式碼而不需要更新的路徑。

以下是範例，可協助您了解的差異：

| 完整的 URL | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| 伺服器名稱 | *mycompanyserver* |
| 虛擬路徑 | */humanresources/CompanyPolicy.htm* |
| 實體路徑 | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

虛擬根目錄為 /，如同您的 c： 根磁碟機是 \。 （虛擬資料夾路徑一律使用正斜線）。資料夾的虛擬路徑不需要有相同的名稱做為實體的資料夾。它可以是別名。 （在生產伺服器上的虛擬路徑很少比對確切的實體路徑。）

當您使用中程式碼檔案和資料夾時，有時候您需要參考實體的路徑和有時虛擬路徑，視您正在使用哪些物件而定。 ASP.NET 為您提供這些工具使用程式碼中的檔案和資料夾路徑：`Server.MapPath`方法，而`~`運算子和`Href`方法。

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>轉換虛擬與實體路徑： Server.MapPath 方法

`Server.MapPath`方法會將轉換的虛擬路徑 (例如 */default.cshtml*) 至絕對實體路徑 (例如*C:\WebSites\MyWebSiteFolder\default.cshtml*)。 使用此方法每次您需要完整的實體路徑。 典型的範例是當您正在讀取或寫入文字檔案或 web 伺服器上的映像檔。

您通常不知道您的網站裝載站台伺服器上的絕對實體路徑，這個方法可以將路徑轉換，因此您知道 — 虛擬路徑，您的伺服器上的對應路徑。 您將虛擬路徑傳遞至檔案或資料夾方法，並傳回的實體路徑：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>參考的虛擬根目錄： ~ 運算子和 Href 方法

在 *.cshtml*或 *.vbhtml*檔案中，您可以參考虛擬根路徑使用`~`運算子。 這是非常實用，因為您可以移動的頁面，在網站中，而且任何連結至其他頁面包含不會中斷。 如果您曾經將您的網站移至不同的位置很方便。 以下是一些範例：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

如果網站`http://myserver/myapp`，以下是如何 ASP.NET 會將這些路徑網頁執行時：

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

（實際上不會看到這些路徑做為變數的值，但 ASP.NET 會將路徑，是原先）。

您可以使用`~`運算子 （如上述） 的伺服器程式碼中，在標記中，像這樣：

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

在標記中，您使用`~`運算子來建立資源，例如映像檔案、 其他網頁和 CSS 檔案的路徑。 ASP.NET 網頁執行時，尋找整個頁面 （程式碼和標記），並解決所有`~`參考適當的路徑。

## <a name="conditional-logic-and-loops"></a>條件式邏輯和迴圈

ASP.NET server 程式碼可讓您根據條件執行工作並撰寫程式碼重複陳述式的特定數目的時間 （也就是程式碼會執行迴圈）。

### <a name="testing-conditions"></a>測試條件

若要測試您所使用的簡單條件`if`陳述式，傳回 true 或 false 會根據您指定的測試：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

`if`關鍵字開始區塊。 實際的測試 （條件） 位於括號內，並傳回 true 或 false。 測試為 true 時，執行的陳述式會放在大括號。 `if`陳述式可包含`else`指定當條件為 false 時要執行的陳述式區塊：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

您可以加入多個條件使用`else if`區塊：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

在此範例中，如果第一個條件，在如果區塊不成立，`else if`條件會檢查。 如果符合該條件中的陳述式`else if`區塊會執行。 如果不符合任何一個條件中的陳述式`else`區塊會執行。 您可以加入任意數目的 else 的 if 區塊，並關閉與`else`為封鎖&quot;一切&quot;條件。

若要測試條件數量龐大，請使用`switch`區塊：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

要測試的值位於括號內 (在範例中，`weekday`變數)。 每個個別的測試會使用`case`結尾是冒號 （:） 的陳述式。 如果值`case`陳述式的測試值相符，該案例區塊中的程式碼會執行。 關閉與每個 case 陳述式`break`陳述式。 (如果您忘記包含在每個中斷`case`封鎖，請從下一個程式碼`case`陳述式也會執行。)A`switch`區塊通常會有`default`陳述式的最後一個案例&quot;一切&quot;其他情況下都是的則為 true 時執行的選項。

在瀏覽器中顯示最後兩個條件式區塊的結果：

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>迴圈的程式碼

您通常需要重複執行相同的陳述式。 您的迴圈。 例如，您通常會執行相同的陳述式，每個項目集合中的資料。 如果您知道完全多少次您想要執行迴圈，您可以使用`for`迴圈。 這種迴圈是特別適用於計數或向下計數：

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

迴圈開頭`for`每個以分號結束關鍵字，後面接著括號中，三個陳述式。

- 在括弧內，第一個陳述式 (`var i=10;`) 建立計數器，並將它初始化為 10。 您不需要名稱計數器`i`&#8212;您可以使用任何變數。 當`for`迴圈會執行，此計數器會自動遞增。
- 第二個陳述式 (`i < 21;`) 設定您想要計算的幅度的條件。 您想在此情況下，請移至最多 20 個 （也就繼續執行時，此計數器會小於 21）。
- 第三個陳述式 (`i++` ) 會使用遞增運算子，只需指定計數器應有 1 加入至它在每次執行迴圈。

在括號內是程式碼會執行迴圈的每個反覆項目。 標記會建立新的段落 (`<p>`元素) 在每次且會將行加入輸出中顯示的值`i`（計數器）。 當您執行此頁面時，此範例會建立 11 行顯示輸出，以指出項目編號每一行文字。

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

如果您使用集合或陣列，您通常使用`foreach`迴圈。 集合是一組類似的物件，而`foreach`迴圈可讓您執行集合中每個項目的工作。 這種類型的迴圈很方便的集合，因為與不同的是`for`迴圈中，您不必遞增計數器或設定的限制。 相反地，`foreach`迴圈的程式碼會繼續執行此集合直到完成為止。

例如，下列程式碼會傳回中的項目`Request.ServerVariables`集合，其包含您的 web 伺服器的相關資訊的物件。 它會使用`foreac`h 迴圈以顯示每個項目的名稱來建立新`<li>`HTML 項目符號清單中的項目。

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

`foreach`關鍵字後面接著括號中您宣告的變數，表示集合中的單一項目 (在範例中， `var item`)，後面接著`in`關鍵字，後面接著您想要重複使用的集合。 本文的`foreach`迴圈中，您可以存取目前的項目，使用您稍早宣告的變數。

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

若要建立更通用的迴圈，請使用`while`陳述式：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

A`while`迴圈開頭`while`關鍵字，後面接著括號，您可以指定迴圈繼續的時間長度 (這裡，只要`countNum`小於 50)，然後重複區塊。 通常遞增迴圈 （加入） 或遞減 （被減數） 變數或物件用來計算。 在範例中，`+=`運算子會加入 1 到`countNum`每次執行迴圈。 (以遞減的變數向下計數在迴圈中，您會使用遞減運算子`-=`)。

## <a name="objects-and-collections"></a>物件和集合

幾乎所有的作業在 ASP.NET 網站是物件，包括網頁本身。 本節討論一些重要的物件，您將使用經常在您的程式碼中。

### <a name="page-objects"></a>頁面物件

在 ASP.NET 中的最基本物件是的頁面。 您可以存取直接沒有任何合格的物件的頁面物件的屬性。 下列程式碼取得網頁的檔案路徑，使用`Request`頁面的物件：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

要取消選取您要參考屬性和目前的頁面物件上的方法，您可以選擇性地使用關鍵字`this`來代表在程式碼中的頁面物件。 以下是上述程式碼範例中，與`this`新增至代表的頁面：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

您可以使用屬性的`Page`物件取得許多資訊，例如：

- `Request`. 如您所見，這是目前的要求，包括何種瀏覽器所做要求的頁面、 使用者識別、 等等的 URL 的相關資訊的集合。
- `Response`. 這是會在伺服端程式碼執行完成時傳送到瀏覽器的回應 （頁面） 的相關資訊的集合。 例如，您可以使用此屬性寫入至回應的資訊。 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>（陣列和字典） 的集合物件

A*集合*是屬於相同的類型，例如集合的物件群組`Customer`資料庫中的物件。 ASP.NET 包含許多的內建集合，例如`Request.Files`集合。

您通常會使用集合中的資料。 兩個常見的集合型別是*陣列*和*字典*。 當您想要儲存的類似項目集合，但不想要建立個別的變數來保存每個項目時，陣列會是很有用：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

使用陣列時，您宣告為特定的資料類型，例如`string`， `int`，或`DateTime`。 若要表示變數可以包含陣列，請在宣告中加入括號 (例如`string[]`或`int[]`)。 您可以存取項目中使用它們的位置 （索引） 的陣列，或使用`foreach`陳述式。 陣列索引以零為起始&#8212;也就是第一個項目是在位置 0，第二個項目位於位置 1，依此類推。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

您可以判斷陣列中的項目數目，藉由取得其`Length`屬性。 若要取得特定的項目 （如果您要搜尋的陣列） 的陣列中的位置，請使用`Array.IndexOf`方法。 您也可以執行像是反向陣列的內容 (`Array.Reverse`方法) 或排序內容 (`Array.Sort`方法)。

在瀏覽器中顯示的字串陣列程式碼的輸出：

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

字典是索引鍵/值組的集合，您提供的索引鍵 （或名稱） 來設定或擷取對應的值：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

若要建立字典，請使用`new`關鍵字，指出您要建立新的字典物件。 您可以將變數使用字典`var`關鍵字。 指出資料類型的字典使用角括號中的項目 ( `< >` )。 在宣告的結尾，您必須加入一組括弧，因為這是實際的方法，建立新的字典。

若要新增至字典的項目，您可以呼叫`Add`字典變數的方法 (`myScores`在此情況下)，然後指定索引鍵和值。 或者，您可以使用方括號，以表示索引鍵，並執行簡單指派，如下列範例所示：

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

若要從字典取得值，您可以指定索引鍵括號括住：

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>呼叫具有參數的方法

當您閱讀稍早在本文中，您使用程式設計物件的方法。 例如，`Database`物件可能會有`Database.Connect`方法。 許多方法也有一或多個參數。 A*參數*是傳遞至方法的值以啟用方法，以完成其工作。 例如，查看的宣告`Request.MapPath`方法，這個方法會採用三個參數：

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

（行被包裝成讓它更容易閱讀。 請記住您可以將的分行符號幾乎任何地方除了內部字串括在引號。)

這個方法會傳回至指定的虛擬路徑對應的伺服器上的實體路徑。 方法有三個參數`virtualPath`， `baseVirtualDir`，和`allowCrossAppMapping`。 （請注意在宣告中，列出參數的資料，它們會接受的資料類型）。當您呼叫這個方法時，您必須提供所有的三個參數的值。

Razor 語法可讓您將參數傳遞至方法的兩個選項：*位置參數*和*具名參數*。 若要呼叫方法，使用位置參數，您傳入參數嚴格的順序，方法宣告中所指定。 （您將通常知道此順序閱讀文件的方法）。您必須遵循的順序，並不能略過任何參數&#8212;如果有必要，您傳遞空字串 (`""`) 或`null`位置參數，您不需要的值。

下列範例假設您有一個名為資料夾*指令碼*在您的網站。 程式碼會呼叫`Request.MapPath`依正確順序的三個參數的方法，並傳遞值。 然後，它會顯示產生的對應的路徑。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

當方法有許多參數時，您可以使用具名參數，將程式碼更容易閱讀。 若要呼叫的方法使用具名的參數，您可以指定參數名稱後面接著冒號 （:），然後此值。 具名參數的優點是，您就可以將其傳遞任何您想要的順序。 （的缺點是在方法呼叫不是精簡）。

下列範例會呼叫上述方法，但使用具名參數，以提供值：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

如您所見，將參數傳遞不同的順序。 不過，如果您執行上述範例，此範例中，它們會傳回相同的值。

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>處理錯誤

### <a name="try-catch-statements"></a>Try Catch 陳述式

您通常會需要陳述式可能失敗的原因，您無法控制的程式碼。 例如: 

- 如果您的程式碼會嘗試建立或存取的檔案，可能會發生各種錯誤。 您想要的檔案可能不存在，可能會遭到鎖定，程式碼可能不具有權限，等等。
- 同樣地，如果您的程式碼會嘗試以更新資料庫中的記錄，可以有權限問題、 可能會卸除資料庫的連接、 要儲存的資料可能不正確，依此類推。

在程式設計的詞彙，這些情況下會呼叫*例外狀況*。 如果您的程式碼遇到例外狀況，則會產生 （會擲回） 的錯誤訊息，最多只能不快使用者：

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

在您的程式碼可能會遇到例外狀況的情況下，以及為了避免此類型的錯誤訊息，您可以使用`try/catch`陳述式。 在`try`陳述式中，執行您正在檢查的程式碼。 一或多個`catch`陳述式，您可以尋找特定可能發生的錯誤 （特定類型的例外狀況）。 您可以包含最大數量`catch`陳述式，當您需要尋找您所預期的錯誤。

> [!NOTE]
> 我們建議您避免使用`Response.Redirect`方法中的`try/catch`陳述式，因為它會在頁面中造成例外狀況。


下列範例顯示建立第一次要求的文字檔，然後顯示按鈕，可讓使用者開啟檔案的頁面。 此範例刻意使用不正確的檔案名稱，因此它將會導致例外狀況。 程式碼包含`catch`陳述式中的兩個可能的例外狀況： `FileNotFoundException`，發生錯誤，檔案名稱是否與`DirectoryNotFoundException`，發生於 ASP.NET 甚至無法尋找資料夾。 （您可以取消此範例中的陳述式才能看到一切正常運作時，它的執行方式註解）。

如果您的程式碼未處理例外狀況，您會看到錯誤頁面類似上一個螢幕擷取畫面。 不過，`try/catch`一節可協助防止使用者看到這些類型的錯誤。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>其他資源

**使用 Visual Basic 進行程式設計**


[附錄： Visual Basic 語言和語法](https://go.microsoft.com/fwlink/?LinkId=202908)


**參考文件**


[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[C# 語言](https://msdn.microsoft.com/library/kx37x362.aspx)
