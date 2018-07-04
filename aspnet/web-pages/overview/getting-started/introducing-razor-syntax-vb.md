---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: 使用 Razor 語法 (Visual Basic) 的 ASP.NET Web 程式設計簡介 |Microsoft Docs
author: tfitzmac
description: 本附錄提供您的概觀與 ASP.NET Web pages 程式設計在 Visual Basic 中使用 Razor 語法。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: d4be7e2ed1b847d8b4167872728815330dbfe432
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378737"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>使用 Razor 語法 (Visual Basic) 的 ASP.NET Web 程式設計簡介
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章概述您程式設計的 ASP.NET 網頁使用 Razor 語法和 Visual Basic。 ASP.NET 是 Microsoft 的技術，用於在 web 伺服器上執行動態網頁。
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


使用 Razor 語法的 ASP.NET Web Pages 的大多數範例使用 C#。 但 Razor 語法也支援 Visual Basic。 若要程式 ASP.NET web 網頁在 Visual Basic 中，您會建立與網頁 *.vbhtml*副檔名，然後新增 Visual Basic 程式碼。 這篇文章會提供使用 Visual Basic 語言和語法，來建立 ASP.NET 網頁的概觀。

> [!NOTE]
> Microsoft WebMatrix 的網站預設範本 (**這家麵包店**，**相片圖庫**，並**入門網站**等) 可用於 C# 和 Visual Basic 版本。 您可以安裝的 Visual Basic 範本，藉由以 NuGet 套件。 網站範本會安裝在您的網站，在名為的資料夾中的根資料夾*Microsoft 範本*。


## <a name="the-top-8-programming-tips"></a>最佳的 8 程式設計秘訣

此區段會列出您一定要知道當您開始撰寫使用 Razor 語法的 ASP.NET 伺服器程式碼的一些秘訣。

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1.您將程式碼新增至頁面上，使用 @ 字元

`@`字元開始內嵌運算式、 單一陳述式區塊和多重陳述式區塊：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

在瀏覽器中顯示結果：

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **HTML 編碼**
> 
> 當您在頁面上，使用顯示的內容時`@`字元 ASP.NET HTML 編碼的輸出，如上述範例中，所示。 這會取代 HTML 的保留的字元 (例如`<`並`>`和`&`) 與啟用顯示網頁，而不會轉譯為 HTML 標記或實體中的字元為字元的代碼。 而不進行 HTML 編碼，從伺服器程式碼的輸出可能不正確，會顯示，並可能會暴露於安全性風險的頁面。
> 
> 如果您的目標是要輸出轉譯為標記的標記的 HTML 標記 (例如`<p></p>`段落或`<em></em>`來強調文字)，請參閱節[結合文字、 標記和程式碼區塊中的程式碼](#BM_CombiningTextMarkupAndCode)本文稍後的。
> 
> 您可以深入了解中的 HTML 編碼[使用 ASP.NET Web Pages 網站中的 HTML 表單](https://go.microsoft.com/fwlink/?LinkId=202892)。


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2.您將程式碼區塊，使用程式碼...結束程式碼

程式碼區塊包含一個或多個程式碼陳述式，並加上關鍵字`Code`和`End Code`。 將放在開頭`Code`關鍵字後面`@`字元&#8212;兩者之間不能有空白字元。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

在瀏覽器中顯示結果：

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3.區塊中，您最終使用分行符號的每個程式碼陳述式

在 Visual Basic 程式碼區塊中，每個陳述式結尾分行符號。 （在本文稍後您會看到用來包裝成多行程式碼陳述式，如有需要）。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4.您可以使用變數來儲存值

您可以儲存中的值*變數*，包括字串、 數字和日期等。您建立新變數使用`Dim`關鍵字。 您可以直接在頁面上，使用中插入變數的值`@`。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

在瀏覽器中顯示結果：

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5.您將常值字串值括在雙引號中

A*字串*是會被視為文字的字元序列。 若要指定為字串，您將它括在雙引號中：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

若要內嵌引號內的字串值，會插入兩個雙引號字元。 如果您想出現一次在網頁輸出中的雙引號字元，輸入為`""`在括住字串，並在您想要出現兩次，如果輸入為`""""`內加上引號的字串。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

在瀏覽器中顯示結果：

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6.Visual Basic 程式碼不區分大小寫

Visual Basic 語言不區分大小寫。 程式設計關鍵字 (例如`Dim`， `If`，和`True`) 和變數名稱 (例如`myString`，或`subTotal`) 可以在任何情況下寫入。

下列程式碼會將值指派給變數`lastname`使用小寫名稱，然後再輸出至使用大寫名稱頁面變數的值。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

在瀏覽器中顯示結果：

![vb 語法 5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7.大部分您撰寫程式碼牽涉到使用物件

物件表示您可以使用程式設計的項目&#8212;頁面、 文字方塊、 檔案、 影像、 web 要求、 電子郵件訊息、 客戶記錄 （資料庫資料列） 等等。物件具有描述其特性的屬性&#8212;的文字 方塊中的物件具有`Text`屬性，要求物件具有`Url`屬性，電子郵件訊息已`From`屬性，與客戶物件都有`FirstName`屬性。 物件也有方法&quot;動詞&quot;他們可以執行。 範例包括將檔案物件的`Save`方法中，映像物件的`Rotate`方法，以及電子郵件物件的`Send`方法。

您會經常使用`Request`物件，提供您資訊，例如值的表單欄位 （文字方塊等），頁面上進行要求的頁面、 使用者身分識別等 URL 的瀏覽器的何種類型。此範例示範如何存取的屬性`Request`物件，以及如何呼叫`MapPath`方法`Request`物件，可讓您在伺服器的頁面上的絕對路徑：

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

在瀏覽器中顯示結果：

![Razor Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8.您可以撰寫程式碼，讓決策

動態網頁的重要功能是，您可以決定如何根據條件。 若要這樣做最常見的方法是使用`If`陳述式 (和選擇性`Else`陳述式)。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

陳述式`If IsPost`是本文撰寫的簡略方法`If IsPost = True`。 連同`If`陳述式，有各種不同的方式來測試條件，重複程式碼區塊，並依此類推，這是本文稍後所述。

在瀏覽器中顯示結果 (按一下後**送出**):

![Razor Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET 和 POST 方法與 IsPost 屬性**
> 
> 用於 web 網頁 (HTTP) 的通訊協定支援非常有限的數目的方法 (&quot;動詞&quot;)，用來對伺服器提出要求。 最常見的兩個為 GET，會用來讀取的頁面和文章，用來提交頁面。 一般情況下，使用者要求的頁面上，第一次要求頁面時使用 GET。 如果使用者填寫表單，並按一下**送出**，瀏覽器對伺服器提出 POST 要求。
> 
> 在 web 程式設計中，通常很有用知道是否要求網頁時正在為 GET 或 POST，讓您知道如何處理頁面。 ASP.NET Web Pages 中，您可以使用`IsPost`屬性，以查看要求是 GET 或 POST。 如果要求是某篇文章，`IsPost`屬性會傳回 true，而且您可以執行像是讀取表單上的文字方塊的值。 您會看到的許多範例會示範如何處理值的方式而定頁面`IsPost`。


## <a name="a-simple-code-example"></a>簡單的程式碼範例

此程序會示範如何建立一個網頁，說明基本的程式設計技術。 在範例中，您可以建立讓使用者輸入兩個數字，然後再將它們加入，並顯示結果頁面。

1. 在編輯器中，建立新的檔案並將它命名*AddNumbers.vbhtml*。
2. 將下列程式碼和標記複製到頁面上，而且要取代已經在頁面中的任何項目中。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    以下是您必須注意的事項：

    - `@`字元會在頁面中，啟動程式碼的第一個區塊，而且它前面出現`totalMessage`變數內嵌底部附近。
    - 位於頁面頂端的區塊括住`Code...End Code`。
    - 變數`total`， `num1`， `num2`，和`totalMessage`儲存數個數字和字串。
    - 常值字串值，指派給`totalMessage`變數是雙引號括住。
    - 因為 Visual Basic 程式碼是不區分大小寫，當`totalMessage`頁面底部附近，會使用變數，其名稱只必須符合變數宣告，在頁面頂端的拼字。 大小寫並不重要。
    - 運算式`num1.AsInt()`  +  `num2.AsInt()`示範如何使用物件和方法。 `AsInt`上每個變數的方法會將轉換為整數 （整數） 可加入的使用者所輸入的字串。
    - `<form>`標記會包括`method="post"`屬性。 這表示當使用者按一下**新增**，頁面將會傳送到伺服器使用 HTTP POST 方法。 當送出頁面、 程式碼`If IsPost`評估為 true，條件式程式碼執行時，顯示個數字相加的結果。
3. 儲存頁面，並在瀏覽器中執行它。 (請確定中選取頁面**檔案**才能執行這個工作區。)輸入兩個整數數字，然後按一下**新增** 按鈕。

    ![Razor Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Visual Basic 語言和語法

先前您已看到如何建立 ASP.NET 網頁，以及如何將伺服器程式碼新增至 HTML 標記的基本範例。 這裡您將了解使用 Visual Basic 來撰寫 ASP.NET 伺服器程式碼使用 Razor 語法的基本概念&#8212;也就是程式設計語言規則。

如果您是熟悉程式設計 （尤其是如果您已使用 C、 c + +、 C#、 Visual Basic 或 JavaScript），大多什麼您閱讀這裡應該不陌生。 您可能需要在自己熟悉只 WebMatrix 程式碼新增至標記中的如何 *.vbhtml*檔案。

### <a id="BM_CombiningTextMarkupAndCode"></a>  結合文字、 標記和程式碼區塊中的程式碼

在伺服器程式碼區塊，您通常要輸出的文字和頁面的標記。 如果伺服器程式碼區塊包含的文字，不是程式碼，而是應轉譯為是，ASP.NET 必須能夠區別該文字與程式碼。 有幾個方式可做到這點。

- 中的 HTML 區塊項目，例如括住文字`<p></p>`或`<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    HTML 項目可以包含文字、 其他 HTML 項目和伺服器程式碼運算式。 當 ASP.NET 看到 HTML 開頭標記 (例如`<p>`) 它將元素呈現的所有項目，做為其內容是以瀏覽器 （並解析為伺服器程式碼運算式）。

- 使用`@:`運算子或`<text>`項目。 `@:`輸出內容包含純文字] 或 [無對應的 HTML 標記。 單行`<text>`元素會括住要輸出的多行。 當您不想要呈現的 HTML 項目作為輸出的一部分，則這些選項會很有用。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    下列範例會重複上一個範例，但會使用一對`<text>`標記括住要轉譯的文字。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    在下列範例中，`<text>`並`</text>`標記括住三行，全部都有部分未包含的文字和不相符的 HTML 標記 (`<br />`)，以及伺服器程式碼和相符的 HTML 標記。 同樣地，您也無法在個別使用每一行`@:`運算子; 其中一個的方式運作。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > 當您輸出文字，這一節所示&#8212;使用 HTML 元素，`@:`運算子，或`<text>`項目&#8212;ASP.NET 不進行 HTML 編碼的輸出。 (如先前所述，ASP.NET 未編碼的伺服器程式碼運算式和伺服器程式碼區塊，前面都會加上輸出`@`，除非在這一節所述的特殊案例。)

### <a name="whitespace"></a>Whitespace

額外的空格，陳述式 （內部和外部字串常值），就不會影響陳述式：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>長的陳述式分成多行

您也可以使用底線字元為多行中斷很長的程式碼陳述式`_`(在 Visual Basic 中稱為*接續字元*) 在每一行程式碼之後。 若要中斷陳述式到下一行中，線的一端會新增一個空格，然後接續字元。 在下一行繼續陳述式。 您可以包裝到您需要以提高可讀性的行數的陳述式。 下列陳述式都相同：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

不過，您無法包裝字串常值的中間一條線。 下列範例無法運作︰

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

若要結合換行至類似上述的程式碼的多行的長字串，您必須使用*串連運算子*(`&`)，您將在本文稍後看到。

### <a name="code-comments"></a>程式碼註解

註解可讓您為您自己或其他保留資訊。 Razor 語法的註解前面會加上`@*`結尾`*@`。

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

在程式碼區塊中，您可以使用 Razor 語法的註解，或者您可以使用一般的 Visual Basic 註解字元，也就是單引號 (`'`) 前面加上一行。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>變數

變數是您用來儲存資料的具名的物件。 您可以任何項目，名稱變數名稱必須以字母字元開頭但不能包含空格或保留的字元。 在 Visual Basic 中，如您所見更早版本，並不重要的變數名稱中的字母大小寫。

### <a name="variables-and-data-types"></a>變數和資料類型

變數可以有特定的資料類型，表示何種資料儲存在變數中。 您可以儲存字串值的字串變數 (例如&quot;Hello world&quot;)，儲存整數值 （例如 3 或 79） 的整數變數和日期值儲存在各種不同的格式 （例如 2012 年 4 月 12 日或 2009 年 3 月的日期變數). 而且有許多您可以使用其他資料型別。

不過，您不必指定變數的類型。 在大部分情況下 ASP.NET 可以找出如何使用變數中的資料為基礎的類型。 （有時候您必須指定一個類型，您會看到範例即為 true）。

若要宣告變數但未指定類型時，使用`Dim`再加上變數的名稱 (例如， `Dim myVar`)。 若要宣告變數的類型時，使用`Dim`變數的名稱，後面接著加號`As`，然後是類型名稱 (比方說， `Dim myVar As String`)。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

下列範例會顯示在網頁中使用變數某些內嵌運算式。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

在瀏覽器中顯示結果：

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>轉換和測試資料類型

雖然 ASP.NET 通常可以自動判斷資料型別，有時它不可轉換。 因此，您可能需要協助 ASP.NET 執行的明確轉換。 即使您沒有轉換的型別，有時候很有幫助測試以查看何種資料類型您可能會使用。

最常見的情況是，您必須將字串轉換成其他類型，例如整數或日期。 下列範例示範一般的情況下，您必須將字串轉換為數字。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

因此，使用者輸入來到您為字串。 即使您已提示使用者輸入數字，即使它們已送出使用者輸入和您在程式碼中讀取時所輸入數字、 資料的格式字串。 因此，您必須將字串轉換為數字。 在範例中，如果您嘗試對值執行算術運算，而不需要轉換，會產生下列錯誤，因為 ASP.NET 無法新增兩個字串：

`Cannot implicitly convert type 'string' to 'int'.`

若要轉換成整數的值，請呼叫`AsInt`方法。 如果轉換成功，然後您可以加入數字。

下表列出一些常見的轉換和測試方法的變數。


::: 資料列:::::: 資料行:::<strong>方法</strong>::: 資料行後端:::::: 資料行:::<strong>描述</strong>::: 資料行後端:::::: 資料行:::<strong>範例</strong>::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `AsInt(), IsInt()` ::: 資料行後端:::::: 資料行::: 轉換字串，表示整數值 (例如&quot;593&quot;) 成整數。
::: 資料行後端:::::: 資料行::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    ::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `AsBool(), IsBool()` ::: 資料行後端:::::: 資料行::: 轉換字串 like &quot;，則為 true&quot;或&quot;false&quot;布林型別。
::: 資料行後端:::::: 資料行::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    ::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `AsFloat(), IsFloat()` ::: 資料行後端:::::: 資料行::: 將具有類似的十進位值的字串轉換&quot;1.3&quot;或是&quot;7.439&quot;浮點數。
::: 資料行後端:::::: 資料行::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    ::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `AsDecimal(), IsDecimal()` ::: 資料行後端:::::: 資料行::: 將具有類似的十進位值的字串轉換&quot;1.3&quot;或是&quot;7.439&quot;十進位數字。 （在 ASP.NET 中，十進位數字是更精確比浮點數）。::: 資料行後端:::::: 資料行::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    ::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `AsDateTime(), IsDateTime()` ::: 資料行後端:::::: 資料行::: 將 asp.net 代表的日期和時間值的字串轉換`DateTime`型別。
::: 資料行後端:::::: 資料行::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    ::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `ToString()` ::: 資料行後端:::::: 資料行::: 將其他任何資料類型轉換為字串。
::: 資料行後端:::::: 資料行::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    ::: 資料行後端:::::: 資料列結尾:::


## <a name="operators"></a>運算子

運算子是關鍵字或字元，會告訴 ASP.NET 在運算式中執行命令的類型。 Visual Basic 支援許多運算子，但您只需要識別一些開始使用開發 ASP.NET 網頁。 下表摘要說明最常見的運算子。


::: 資料列:::::: 資料行:::<strong>運算子</strong>::: 資料行後端:::::: 資料行:::<strong>描述</strong>::: 資料行後端:::::: 資料行:::<strong>範例</strong>::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `+ - * /` ::: 資料行後端:::::: 資料行::: 用在數值運算式的數學運算子。
::: 資料行後端:::::: 資料行::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    ::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `=` ::: 資料行後端:::::: 資料行::: 指派和相等。 根據內容，將陳述式的右邊的值指派給物件，在左側，或檢查值相等。
::: 資料行後端:::::: 資料行::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    ::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `<>` ::: 資料行後端:::::: 資料行::: 不等比較。 傳回`True`值是否不相等。
::: 資料行後端:::::: 資料行::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    ::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `< > <= >=` ::: 資料行後端:::::: 資料行::: 小於、 大於、 小於或等於、 與大於或等於。
::: 資料行後端:::::: 資料行::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    ::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `&` ::: 資料行後端:::::: 資料行::: 用來將字串的串連。
::: 資料行後端:::::: 資料行::: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    ::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `+= -=` ::: 資料行後端:::::: 資料行::: 遞增和遞減運算子，以新增和從變數 （分別） 減 1。
::: 資料行後端:::::: 資料行::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    ::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `.` ::: 資料行後端:::::: 資料行::: 點。 用來區別物件及其屬性和方法。
::: 資料行後端:::::: 資料行::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    ::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `()` ::: 資料行後端:::::: 資料行::: 括號。 群組運算式，用來將參數傳遞至方法，以及存取陣列和集合的成員。
::: 資料行後端:::::: 資料行::: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    ::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `Not` ::: 資料行後端:::::: 資料行::: 沒有。 反轉，則為 true 的值為 false，反之亦然。 常用縮寫來測試`False`(也就是針對不`True`)。
::: 資料行後端:::::: 資料行::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    ::: 資料行後端:::::: 資料列結尾:::
* * *
::: 資料列:::::: 資料行::: `AndAlso OrElse` ::: 資料行後端:::::: 資料行::: 邏輯 AND 和 OR，這用來連結條件一起。
::: 資料行後端:::::: 資料行::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    ::: 資料行後端:::::: 資料列結尾:::

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

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>參考的虛擬根目錄： ~ 運算子和 Href 方法

在  *.cshtml*或 *.vbhtml*檔案中，您可以參考虛擬根目錄的路徑使用`~`運算子。 這是非常好用，因為您可以移動的頁面，在網站中，而且它們包含其他頁面的任何連結不會中斷。 如果您曾經將您的網站移至不同的位置還有好用。 以下是一些範例：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

如果網站是`http://myserver/myapp`，以下是 ASP.NET 如何處理此頁面會執行的這些路徑：

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

（您實際上不會看到這些路徑做為變數的值，但，是它們的是，ASP.NET 會將路徑）。

您可以使用`~`運算子伺服端程式碼 （如上所述） 並在標記中，像這樣：

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

在標記中，您可以使用`~`運算子來建立資源，例如映像檔案、 其他網頁和 CSS 檔案的路徑。 ASP.NET 頁面執行時，尋找整個頁面 （程式碼和標記），並解決所有`~`參考適當的路徑。

## <a name="conditional-logic-and-loops"></a>條件式邏輯和迴圈

ASP.NET server 程式碼可讓您根據條件和撰寫程式碼重複特定次數也就是程式碼的陳述式執行迴圈執行工作）。

### <a name="testing-conditions"></a>測試條件

若要測試您所使用的簡單條件`If...Then`陳述式，它會傳回`True`或`False`根據您指定的測試：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

`If`關鍵字開始區塊。 實際的測試 （條件） 遵循`If`關鍵字並傳回 true 或 false。 `If`陳述式結尾`Then`。 如果測試為 true，將執行的陳述式會加上`If`和`End If`。 `If`陳述式可包含`Else`，指定當條件為 false 時要執行的陳述式區塊：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

如果`If`陳述式啟動的程式碼區塊時，您不需要使用 「 正常 」`Code...End Code`包含區塊的陳述式。 您可以新增`@`區塊，它會運作。 此方法適用於`If`以及其他的 Visual Basic 程式設計的關鍵字，後面接著程式碼區塊，包括`For`， `For Each`，`Do While`等等。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

您可以加入多個條件使用一或多個`ElseIf`區塊：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

在此範例中，如果第一個條件中`If`區塊不是 true，`ElseIf`在檢查條件。 如果符合該條件中的陳述式`ElseIf`區塊會執行。 如果條件符合中的陳述式`Else`區塊會執行。 您可以新增任意數目的`ElseIf`區塊中使用，並關閉具有`Else`封鎖&quot;一切&quot;條件。

若要測試大量的條件，請使用`Select Case`區塊：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

（在範例中，weekday 變數） 的括號括住，是要測試的值。 每個個別的測試會使用`Case`列出值的陳述式。 如果值`Case`陳述式的測試值符合中的程式碼`Case`區塊執行。

在瀏覽器中顯示的最後兩個條件式區塊的結果：

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>迴圈的程式碼

您通常需要重複執行相同的陳述式。 您可以進行迴圈。 例如，您經常執行相同的陳述式，每個項目集合中的資料。 如果您知道確實多少次您想要執行迴圈，您可以使用`For`迴圈。 這種迴圈是特別適用於計算或向下計數：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

迴圈開頭`For`關鍵字，後面接著三個項目：

- 之後立即`For`陳述式，宣告的計數器變數 (您不必使用`Dim`)，接下來指明中的 [範圍] `i = 10 to 20`。 這表示變數`i`將會在 10 開始計算，並繼續，直到它到達 20 （含）。
- 之間`For`和`Next`陳述式是在區塊的內容。 這可以包含一或多個程式碼執行陳述式以及每個迴圈。
- `Next i`陳述式會結束迴圈。 它會遞增計數器，並啟動迴圈的下一個反覆項目。

程式碼之間的一行`For`和`Next`行包含迴圈的每個反覆項目執行的程式碼。 標記會建立新的段落 (`<p>`項目) 每次，且輸出中顯示的值中加入一行 i （計數器）。 當您執行此頁面時，此範例會建立 11 行顯示輸出，表示項目數目每一行中的文字。

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

如果您正在使用集合或陣列，您經常使用`For Each`迴圈。 集合是一組類似的物件，而`For Each`迴圈可讓您執行的工作集合中的每個項目上。 這種類型的迴圈是方便的集合，因為不像`For`迴圈中，您不需要遞增計數器，或設定限制。 相反地，`For Each`迴圈的程式碼會繼續透過集合直到完成為止。

這個範例會傳回中的項目`Request.ServerVariables`集合 （其中包含您的 web 伺服器的相關資訊）。 它會使用`For Each`迴圈，以顯示每個項目的名稱來建立新`<li>`HTML 項目符號清單中的項目。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

`For Each`關鍵字後面的變數，表示集合中的單一項目 (在範例中， `myItem`)，後面接著`In`關鍵字，後面接著您要循環的集合。 本文的`For Each`迴圈中，您可以存取目前的項目使用您稍早宣告的變數。

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

若要建立更通用的迴圈，請使用`Do While`陳述式：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

這個迴圈開頭`Do While`關鍵字，後面接著一項條件，後面接著要重複的區塊。 迴圈通常遞增 （加入） 或遞減 （減去） 的變數或用於計算的物件。 在範例中，`+=`運算子將 1 加到變數的值在每次執行迴圈。 (以遞減的向下計數的迴圈中的變數，您會使用遞減運算子`-=`。)

## <a name="objects-and-collections"></a>物件和集合

幾乎在 ASP.NET 網站中所有項目是物件，包括網頁本身。 本章節將討論一些重要的物件，您將使用經常在您的程式碼。

### <a name="page-objects"></a>頁面物件

在 ASP.NET 中最基本的物件是頁面。 您可以存取任何合格的物件不直接的 page 物件的屬性。 下列程式碼會取得頁面的檔案路徑，使用`Request`頁面物件：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

您可以使用屬性`Page`物件，以取得大量資訊，例如：

- `Request`. 如您所見，這是目前的要求，包括進行要求的頁面、 使用者身分識別等 URL 的瀏覽器的何種類型的相關資訊的集合。
- `Response`. 這是會在伺服器程式碼執行完成時傳送到瀏覽器的回應 （頁面） 的相關資訊的集合。 例如，您可以使用此屬性寫入至回應的資訊。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>（陣列和字典） 的集合物件

集合是一組相同的類型，例如的集合物件`Customer`資料庫的物件。 ASP.NET 包含許多的內建集合，例如`Request.Files`集合。

您通常會使用在集合中的資料。 兩個常見的集合類型*陣列*並*字典*。 當您想要儲存一組類似的項目，但不想要建立個別的變數來保存每個項目時，陣列是很有用：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

使用陣列時，您宣告特定的資料類型，例如`String`， `Integer`，或`DateTime`。 若要指出變數可以包含陣列，您在宣告中的變數名稱加上括號 (例如`Dim myVar() As String`)。 您可以存取項目中使用它們的位置 （索引） 的陣列，或使用`For Each`陳述式。 陣列索引以零為起始&#8212;也就是第一個項目是在位置 0，第二個項目位於位置 1，依此類推。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

您可以判斷陣列中的項目數，藉由取得其`Length`屬性。 若要取得特定項目的陣列中的位置 (也就是在陣列搜尋)，使用`Array.IndexOf`方法。 您也可以執行像是反向陣列的內容 (`Array.Reverse`方法) 或排序內容 (`Array.Sort`方法)。

在瀏覽器中顯示的字串陣列程式碼的輸出：

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

字典是索引鍵/值組的集合，您可在此提供金鑰 （或名稱），以設定或擷取對應的值：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

若要建立字典，您使用`New`關鍵字來表示，您要建立新`Dictionary`物件。 您可以將字典指派給變數使用`Dim`關鍵字。 指出資料類型的字典使用括號中的項目 ( `( )` )。 宣告的結尾，在中，您必須新增另一組括號，因為這是實際的方法，建立新的字典。

若要新增至字典的項目，您可以呼叫`Add`字典變數的方法 (`myScores`在此情況下)，然後指定 索引鍵和值。 或者，您可以使用括號表示索引鍵，並執行簡單的指派，如下列範例所示：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

若要從字典取得值，您可以指定索引鍵括號括住：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>呼叫具有參數的方法

如您所見稍早在本文中，您使用程式設計的物件具有方法。 例如，`Database`物件可能會有`Database.Connect`方法。 許多的方法也會有一或多個參數。 A*參數*是一個值傳遞至方法，若要啟用以完成其工作的方法。 例如，看看的宣告`Request.MapPath`方法，後者會採用三個參數：

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

這個方法會傳回對應的伺服器上的實體路徑，以指定的虛擬路徑。 方法的三個參數都`virtualPath`， `baseVirtualDir`，和`allowCrossAppMapping`。 （請注意，在宣告中，列出參數的資料類型，它們會接受的資料）。當您呼叫這個方法時，您必須提供所有的三個參數的值。

當您使用 Visual Basic 含有 Razor 語法時，您會有兩個選項可將參數傳遞至方法：*位置參數*或是*具名參數*。 若要呼叫使用位置參數的方法，您會以嚴格的順序，指定在方法宣告中傳遞參數。 （您將通常知道此順序，請閱讀文件的方法）。您必須遵循的順序，以及您不能略過的任何參數&#8212;如果有必要，您傳遞空字串 (`""`) 或為 null 的位置參數，您不需要的值。

下列範例假設您有一個名為的資料夾*指令碼*在您的網站。 程式碼會呼叫`Request.MapPath`方法，並傳遞正確的順序的三個參數值。 然後，它會顯示產生的對應的路徑。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

當有許多方法的參數時，您可以保留您的程式碼更簡潔且更容易閱讀使用具名參數。 若要呼叫方法，使用具名的參數，指定參數名稱，後面接著`:=`，然後提供值。 具名參數的優點是您可以在任何您想要的順序新增它們。 （一項缺點是，在方法呼叫並不會精簡）。

下列範例會呼叫與上述相同的方法，但使用具名參數，以提供值：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

如您所見，參數會傳遞不同的順序。 不過，如果您執行先前的範例和此範例中，它們會傳回相同的值。

## <a name="handling-errors"></a>處理錯誤

### <a name="try-catch-statements"></a>Try Catch 陳述式

陳述式通常必須在程式碼中可能會失敗的原因，您無法控制。 例如: 

- 如果您的程式碼會嘗試開啟、 建立、 讀取或寫入檔案，可能會發生各種錯誤。 您想要的檔案可能不存在，它可能會遭到鎖定，程式碼可能不具有權限，等等。
- 同樣地，如果您的程式碼會嘗試更新資料庫中的記錄，可以有權限問題，可能會卸除資料庫的連接，要儲存的資料可能無效，依此類推。

在程式設計的詞彙中，這些情況下則稱為*例外狀況*。 如果您的程式碼發生例外狀況，則會產生 （擲回） 的錯誤訊息也就是，最惱人的使用者。

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

在您的程式碼可能會遇到例外狀況的情況下，並以避免此類型的錯誤訊息，您可以使用`Try/Catch`陳述式。 在 `Try`陳述式中，執行您的程式碼。 在一或多個`Catch`陳述式，您可以尋找特定可能發生的錯誤 （特定類型的例外狀況）。 您可以包含更多`Catch`陳述式，為您要尋找的預期的錯誤。

> [!NOTE]
> 我們建議您避免使用`Response.Redirect`方法中的`Try/Catch`陳述式，因為它會在網頁中造成例外狀況。


下列範例顯示建立第一個要求上的文字檔案，然後顯示按鈕，讓使用者可以開啟檔案的頁面。 此範例刻意使用不正確的檔案名稱，如此會導致例外狀況。 程式碼包含`Catch`陳述式中的兩個可能的例外狀況： `FileNotFoundException`，就會出現錯誤，檔案名稱是否與`DirectoryNotFoundException`，發生於 ASP.NET 即使找不到資料夾。 （您可以取消此範例中的陳述式以查看一切正常運作時，它的執行方式註解）。

如果您的程式碼未處理例外狀況，您會看到先前的螢幕擷取畫面類似的錯誤頁面。 不過，`Try/Catch`一節可協助避免使用者看到這些錯誤類型。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>其他資源

### <a name="reference-documentation"></a>參考文件

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Visual Basic 語言](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
