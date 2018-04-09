---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: 使用 Razor 語法 (Visual Basic) 的 ASP.NET Web 程式設計簡介 |Microsoft 文件
author: tfitzmac
description: 本附錄可讓您使用 ASP.NET Web 網頁程式設計的概觀在 Visual Basic 中為使用 Razor 語法。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 715e52715fb22b92f94d3d602ec58c29a913426c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>使用 Razor 語法 (Visual Basic) 的 ASP.NET Web 程式設計簡介
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文提供您進行程式設計的概觀與 ASP.NET Web 網頁使用 Razor 語法與 Visual Basic。 ASP.NET 是 Microsoft 的技術，用於 web 伺服器上執行動態網頁。
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


大部分的 ASP.NET 網頁使用 Razor 語法的範例使用 C#。 但 Razor 語法也支援 Visual Basic。 若要程式在 Visual Basic 中 ASP.NET 網頁上，您可以建立與網頁*.vbhtml*檔案的副檔名，然後加入 Visual Basic 程式碼。 這篇文章會提供使用 Visual Basic 語言及語法來建立 ASP.NET 網頁的概觀。

> [!NOTE]
> Microsoft WebMatrix 的預設網站範本 (**由於這家蛋糕店**，**相片圖庫**，和**入門網站**等等) 都有 C# 和 Visual Basic 版本。 您可以安裝 Visual Basic 的範本會以 NuGet 套件。 在您的網站資料夾中的根資料夾中安裝網站範本*Microsoft 範本*。


## <a name="the-top-8-programming-tips"></a>最上層的 8 個程式設計提示

此區段會列出您一定要知道當您開始撰寫為使用 Razor 語法的 ASP.NET server 程式碼需要的一些秘訣。

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1.您可以將程式碼加入頁面使用 @ 字元

`@`字元開始內嵌運算式、 單一陳述式區塊和多重陳述式區塊：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

在瀏覽器中顯示結果：

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **HTML 編碼**
> 
> 當您在頁面上，使用顯示內容`@`的字元，如同上述範例中，ASP.NET 將 HTML 編碼的輸出。 這會取代保留的 HTML 字元 (例如`<`和`>`和`&`) 與啟用顯示網頁，而不會轉譯為 HTML 標記或實體中的字元為字元的代碼。 而不進行 HTML 編碼，可能不正確，會顯示您伺服器的程式碼的輸出，並可能會暴露於安全性風險的頁面。
> 
> 如果您的目標是要輸出轉譯為標記的標記的 HTML 標記 (例如`<p></p>`段落或`<em></em>`來強調文字)，請參閱節[結合文字、 標記和程式碼區塊中的程式碼](#BM_CombiningTextMarkupAndCode)本文稍後。
> 
> 閱讀更多有關中的 HTML 編碼[使用 ASP.NET Web Pages 站台中的 HTML 表單](https://go.microsoft.com/fwlink/?LinkId=202892)。


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2.您將程式碼區塊程式碼...結束程式碼

程式碼區塊包含一個或多個程式碼陳述式，且括以關鍵字`Code`和`End Code`。 將開頭`Code`關鍵字之後立即`@`字元&#8212;它們之間不能有空格。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

在瀏覽器中顯示結果：

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3.區塊中，您最後分行符號與每個程式碼陳述式

在 Visual Basic 程式碼區塊中，每個陳述式結尾分行符號。 （在本文稍後您會看到用來在包裝成多行冗長的程式碼陳述式，如有需要）。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4.您可以使用變數來儲存值

您可以儲存在值*變數*，包括字串、 數字和日期等。您建立新變數使用`Dim`關鍵字。 您可以直接在頁面上，使用插入變數值`@`。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

在瀏覽器中顯示結果：

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5.您將常值字串值括在雙引號中

A*字串*是會被視為文字的字元序列。 若要指定字串，您將它括在雙引號中：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

若要內嵌引號內的字串值，會插入兩個雙引號字元。 如果您想要對網頁輸出中出現一次的雙引號字元時，請輸入做為`""`在括住字串，並在您想要出現兩次，如果輸入為`""""`加上引號的字串內。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

在瀏覽器中顯示結果：

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6.Visual Basic 程式碼不區分大小寫

Visual Basic 語言不區分大小寫。 程式設計關鍵字 (像是`Dim`， `If`，和`True`) 和變數名稱 (例如`myString`，或`subTotal`) 可以在任何情況下撰寫。

下列程式碼會將值指派給變數`lastname`使用小寫名稱，然後再輸出使用大寫名稱的頁面變數的值。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

在瀏覽器中顯示結果：

![vb 語法 5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7.大部分的程式碼牽涉到使用物件

物件代表您可以使用程式設計的事情&#8212;頁面、 文字方塊、 檔案、 影像、 web 要求、 電子郵件訊息、 客戶記錄 （資料庫資料列）、 等等。物件具有描述其特性的屬性&#8212;文字方塊物件具有`Text`屬性，要求物件具有`Url`屬性，電子郵件訊息有`From`屬性，而客戶物件具有`FirstName`屬性。 物件也有方法&quot;動詞&quot;他們可以執行。 範例包括檔案物件的`Save`方法中，映像物件的`Rotate`方法，以及電子郵件物件的`Send`方法。

您通常會使用`Request`物件，提供您資訊例如值的表單欄位 （文字方塊等），頁面上何種瀏覽器所做要求的頁面、 使用者識別、 等等的 URL。這個範例示範如何存取屬性`Request`物件以及如何呼叫`MapPath`方法`Request`物件，可讓您在伺服器的頁面上的絕對路徑：

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

在瀏覽器中顯示結果：

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8.您可以撰寫程式碼所做的決策

動態網頁的主要功能是您可以判斷如何處理根據條件。 若要這樣做最常見的方法是使用`If`陳述式 (和選擇性`Else`陳述式)。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

陳述式`If IsPost`會撰寫以快速`If IsPost = True`。 連同`If`陳述式，有多種方式來測試條件，重複程式碼區塊，以及等等，這是本文稍後所述。

在瀏覽器中顯示的結果 (按一下後**送出**):

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET 和 POST 方法，以及 IsPost 屬性**
> 
> 用於網頁 (HTTP) 的通訊協定支援非常有限的方法 (&quot;動詞&quot;)，用來對伺服器提出要求。 兩個最常見的是，用來讀取的頁面，以及 POST，用來提交頁面。 一般情況下，使用者要求網頁，第一次要求頁面時利用 GET 輕鬆得到。 如果使用者填入表單，並按一下**送出**，瀏覽器對伺服器提出 POST 要求。
> 
> 中的 web 程式設計，通常會很有用知道是否正在要求頁面為 GET 或 POST，以了解如何處理頁面。 ASP.NET Web Pages 中，您可以使用`IsPost`屬性來查看是否有 GET 或 POST 要求。 如果要求是使用 post 要求`IsPost`屬性會傳回 true，而且您可以執行像是讀取表單上的文字方塊的值。 您會看到的許多範例會示範如何處理不同的值而定頁面`IsPost`。


## <a name="a-simple-code-example"></a>簡單的程式碼範例

此程序會示範如何建立可說明基本的程式設計技術的頁面。 在範例中，您可以建立可讓使用者輸入兩個數字，然後將它們加入，並顯示結果頁面。

1. 在編輯器中，建立新的檔案並將其命名*AddNumbers.vbhtml*。
2. 複製下列程式碼和標記到頁面上，取代已經在頁面中的任何項目。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    以下是您必須注意的一些事項：

    - `@`字元在頁面中，啟動程式碼的第一個區塊，而且之前`totalMessage`內嵌底部附近的變數。
    - 位於頁面頂端的區塊括在`Code...End Code`。
    - 變數`total`， `num1`， `num2`，和`totalMessage`儲存幾個數字和字串。
    - 常值字串值指派給`totalMessage`變數是置於雙引號之間。
    - 因為 Visual Basic 程式碼是不區分大小寫，當`totalMessage`頁面底部附近使用變數，其名稱就只需要比對變數的宣告在頁面頂端的拼字。 大小寫並不重要。
    - 運算式`num1.AsInt()`  +  `num2.AsInt()`示範如何使用物件和方法。 `AsInt`上每個變數的方法會將轉換成整數 （整數），可加入使用者輸入的字串。
    - `<form>`標記包含`method="post"`屬性。 這會指定當使用者按一下**新增**，頁面將會傳送至使用 HTTP POST 方法的伺服器。 送出頁面時，程式碼`If IsPost`評估為 true，條件式程式碼執行時，顯示加上數字的結果。
3. 儲存頁面，並在瀏覽器中執行。 (請確定中選取頁面**檔案**才能執行這個工作區。)兩個整數的輸入，然後按一下**新增** 按鈕。

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Visual Basic 語言和語法

您稍早已基本範例如何建立 ASP.NET 網頁，以及如何將伺服器程式碼加入至 HTML 標記。 這裡您將學習使用 Visual Basic 撰寫為使用 Razor 語法的 ASP.NET server 程式碼的基本概念&#8212;也就是程式設計語言規則。

如果您熟悉程式設計 （特別是如果您使用 C、 c + +、 C#、 Visual Basic 或 JavaScript），大部分的讀取這裡都很熟悉。 您可能需要在您自己熟悉只 WebMatrix 程式碼加入至標記中的方式*.vbhtml*檔案。

### <a id="BM_CombiningTextMarkupAndCode"></a>  結合文字、 標記和程式碼區塊中的程式碼

在伺服器程式碼區塊，您通常要輸出的文字和網頁的標記。 如果伺服器程式碼區塊包含的文字不是程式碼，而是應轉譯為是，ASP.NET 必須能從程式碼中區分該文字。 有幾個方式可做到這點。

- HTML 區塊項目，例如括住文字`<p></p>`或`<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    HTML 項目可以包含文字、 其他 HTML 項目和伺服器程式碼運算式。 當 ASP.NET 會看到開頭 HTML 標記 (例如， `<p>`)，它將元素呈現的所有項目且做為其內容至瀏覽器 （和解析的伺服器程式碼運算式）。

- 使用`@:`運算子或`<text>`項目。 `@:`輸出內容包含純文字或無對應的 HTML 標記; 單行`<text>`元素會括住多個輸出行。 當您不想要呈現的 HTML 項目做為輸出的一部分，這些選項會是很有用。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    下列範例會重複上一個範例，但會使用一對`<text>`標記來括住要呈現的文字。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    在下列範例中，`<text>`和`</text>`標記括住三行，其中有部分未包含的文字和無對應的 HTML 標記 (`<br />`) 以及伺服端程式碼，而相符的 HTML 標記。 同樣地，您也可以在每一行，以個別`@:`運算子; 任一種方式運作。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > 當您在輸出文字這一節中所示&#8212;使用 HTML 項目，`@:`運算子，或`<text>`元素&#8212;ASP.NET 並不進行 HTML 編碼的輸出。 (如前文所述，ASP.NET 未編碼的伺服器程式碼運算式和伺服器程式碼區塊，前面加上輸出`@`，除非在這一節所述的特殊案例。)

### <a name="whitespace"></a>Whitespace

陳述式中 （且字串常值之外） 的額外空間不會影響陳述式：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>長的陳述式分成多行

您也可以使用底線字元成多行中斷冗長的程式碼陳述式`_`(在 Visual Basic 中稱為*接續字元*) 在每一行程式碼之後。 若要中斷到下一行的陳述式，在一行的結尾加入一個空格，然後接續字元。 在下一行繼續陳述式。 您可以包裝陳述式分成多行，視需要以提高可讀性。 下列陳述式都相同：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

不過，您無法包裝一條線中間的字串常值。 下列範例將無法運作：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

要結合多個線條，類似上述的程式碼包裝的長字串，您必須使用*串連運算子*(`&`)，您會看到在本文稍後。

### <a name="code-comments"></a>程式碼註解

註解可讓您保留供您自己或其他資訊。 Razor 語法的註解前面會加上`@*`，而結尾`*@`。

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

程式碼區塊內，您可以使用 Razor 語法的註解，或者您可以使用一般的 Visual Basic 註解字元，也就是單引號 (`'`) 至每一行的前置詞。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>變數

變數是您用來儲存資料的具名的物件。 您可以為變數命名任何項目，但是名稱必須以字母字元開頭，而且不能包含空白字元或保留的字元。 在 Visual Basic 中，如您所見更早版本，並不重要的變數名稱中的字母大小寫。

### <a name="variables-and-data-types"></a>變數和資料類型

變數可以有特定資料類型，表示何種資料儲存在變數中。 您可以儲存字串值的字串變數 (例如&quot;Hello world&quot;)，儲存整數值 （例如 3 或 79） 的整數變數和日期值儲存在不同的格式 （例如 2012 年 4 月 12 日或 2009 年 3 月的日期變數). 而且還有許多其他您可以使用的資料類型。

不過，您不必指定類型的變數。 在大部分情況下 ASP.NET 可以找出如何使用變數中的資料為基礎的類型。 （有時候您必須指定一個類型，您會看到範例這是 true）。

若要宣告但未指定類型的變數，使用`Dim`再加上變數的名稱 (比方說， `Dim myVar`)。 若要宣告變數的類型時，使用`Dim`變數的名稱，後面接著加號`As`和型別名稱 (比方說， `Dim myVar As String`)。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

下列範例會顯示在網頁中使用變數某些內嵌運算式。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

在瀏覽器中顯示結果：

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>轉換和測試資料類型

雖然 ASP.NET 通常可以自動判斷的資料型別，有時無法。 因此，您可能需要幫助 ASP.NET 執行的明確轉換。 即使您沒有將類型轉換，有時候很有幫助測試以查看何種資料類型您可能使用。

最常見的情況是，您必須將字串轉換成其他類型，例如整數或日期。 下列範例顯示一般的情況下，您必須將字串轉換成數字。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

一般而言，使用者輸入都是在您為字串。 即使您已提示使用者輸入數字，而且即使他們已經提交使用者輸入時，而您閱讀程式碼中，輸入數字，資料字串格式。 因此，您必須將字串轉換為數字。 在範例中，如果您嘗試執行算術運算的值，而不需要轉換，會產生下列錯誤，因為 ASP.NET 無法新增兩個字串：

`Cannot implicitly convert type 'string' to 'int'.`

若要將值轉換成整數，您呼叫`AsInt`方法。 如果轉換成功，然後您可以加入數字。

下表列出一些常見的轉換和測試方法的變數。


|   <strong>方法</strong>    |                                                                              <strong>描述</strong>                                                                              |                     <strong>範例</strong>                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
|      `AsInt(), IsInt()`      |                                                 將字串表示之間的整數轉換 (例如&quot;593&quot;) 成整數。                                                 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)] |
|     `AsBool(), IsBool()`     |                                                    轉換字串 like &quot;true&quot;或&quot;false&quot;布林型別。                                                     | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)] |
|    `AsFloat(), IsFloat()`    |                                    將具有類似的十進位值的字串轉換&quot;1.3&quot;或&quot;7.439&quot;浮點數。                                    | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)] |
|  `AsDecimal(), IsDecimal()`  | 將具有類似的十進位值的字串轉換&quot;1.3&quot;或&quot;7.439&quot;為十進位數字。 （在 ASP.NET、 十進位數字是更精確比浮點數）。 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)] |
| `AsDateTime(), IsDateTime()` |                                                將 asp.net 代表日期和時間值的字串轉換`DateTime`型別。                                                 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)] |
|         `ToString()`         |                                                                       將其他任何資料類型轉換為字串。                                                                        | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)] |

## <a name="operators"></a>運算子

運算子是命令的關鍵字或字元，會告知 ASP.NET 何種在運算式中執行。 Visual Basic 支援許多運算子，但您只需要辨識一些開始進行開發的 ASP.NET web pages。 下表摘要說明最常見的運算子。


| <strong>Operator</strong> |                                                                        <strong>描述</strong>                                                                         |                         <strong>範例</strong>                         |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
|         `+ - * /`         |                                                                數學運算子用在數值運算式。                                                                |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]     |
|            `=`            | 指派和等號比較。 根據內容，將陳述式右邊的值指派給物件，在左邊，或檢查值相等。 |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]     |
|           `<>`            |                                                           不等。 傳回`True`值是否不相等。                                                           |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]     |
|        `< > <= >=`        |                                                   小於、 大於、 小於或等於、 與大於或等於。                                                   |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]     |
|            `&`            |                                                                串連，用來加入字串。                                                                | [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)] |
|          `+= -=`          |                                       遞增和遞減運算子，其中加入和減去 1 （分別） 變數。                                       |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]     |
|            `.`            |                                                     點。 用來區別物件及其屬性和方法。                                                      |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]     |
|           `()`            |                           括號。 群組運算式，用來將參數傳遞至方法，以及存取成員的陣列與集合。                           | [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)] |
|           `Not`           |                    不是。 反轉，則為 true 的值為 false，反之亦然。 一般而言，若要測試的簡略方法做`False`(也就是針對不`True`)。                     |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]     |
|     `AndAlso OrElse`      |                                                       邏輯 AND 和 OR，用來連結條件在一起。                                                       |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]     |

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

`Server.MapPath`方法會將轉換的虛擬路徑 (例如*/default.cshtml*) 至絕對實體路徑 (例如*C:\WebSites\MyWebSiteFolder\default.cshtml*)。 使用此方法每次您需要完整的實體路徑。 典型的範例是當您正在讀取或寫入文字檔案或 web 伺服器上的映像檔。

您通常不知道您的網站裝載站台伺服器上的絕對實體路徑，這個方法可以將路徑轉換，因此您知道 — 虛擬路徑，您的伺服器上的對應路徑。 您將虛擬路徑傳遞至檔案或資料夾方法，並傳回的實體路徑：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>參考的虛擬根目錄： ~ 運算子和 Href 方法

在*.cshtml*或*.vbhtml*檔案中，您可以參考虛擬根路徑使用`~`運算子。 這是非常實用，因為您可以移動的頁面，在網站中，而且任何連結至其他頁面包含不會中斷。 如果您曾經將您的網站移至不同的位置很方便。 以下是一些範例：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

如果網站`http://myserver/myapp`，以下是如何 ASP.NET 會將這些路徑網頁執行時：

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

（實際上不會看到這些路徑做為變數的值，但 ASP.NET 會將路徑，是原先）。

您可以使用`~`運算子 （如上述） 的伺服器程式碼中，在標記中，像這樣：

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

在標記中，您使用`~`運算子來建立資源，例如映像檔案、 其他網頁和 CSS 檔案的路徑。 ASP.NET 網頁執行時，尋找整個頁面 （程式碼和標記），並解決所有`~`參考適當的路徑。

## <a name="conditional-logic-and-loops"></a>條件式邏輯和迴圈

ASP.NET server 程式碼可讓您根據條件和撰寫程式碼的重複次數也就是程式碼陳述式執行迴圈執行工作）。

### <a name="testing-conditions"></a>測試條件

若要測試您所使用的簡單條件`If...Then`陳述式會傳回`True`或`False`根據您指定的測試：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

`If`關鍵字開始區塊。 實際的測試 （條件） 遵循`If`關鍵字，並傳回 true 或 false。 `If`陳述式結尾`Then`。 如果測試為 true，將執行的陳述式會括住`If`和`End If`。 `If`陳述式可包含`Else`指定當條件為 false 時要執行的陳述式區塊：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

如果`If`陳述式會啟動程式碼區塊，您不必使用法線`Code...End Code`包含區塊的陳述式。 您可以加入`@`區塊，它會運作。 這個方法適用於`If`以及其他的 Visual Basic 程式設計關鍵字後面的程式碼區塊，包括`For`， `For Each`，`Do While`等等。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

您可以加入多個條件使用一或多個`ElseIf`區塊：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

在此範例中，如果第一個條件中`If`區塊不成立，`ElseIf`條件會檢查。 如果符合該條件中的陳述式`ElseIf`區塊會執行。 如果不符合任何一個條件中的陳述式`Else`區塊會執行。 您可以加入任意數目的`ElseIf`封鎖，並關閉與`Else`為封鎖&quot;一切&quot;條件。

若要測試條件數量龐大，請使用`Select Case`區塊：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

要測試的值是在括號內 （在範例中，weekday 變數）。 每個個別的測試會使用`Case`列出值的陳述式。 如果值`Case`陳述式的測試值相符中的程式碼`Case`執行區塊。

在瀏覽器中顯示最後兩個條件式區塊的結果：

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>迴圈的程式碼

您通常需要重複執行相同的陳述式。 您的迴圈。 例如，您通常會執行相同的陳述式，每個項目集合中的資料。 如果您知道完全多少次您想要執行迴圈，您可以使用`For`迴圈。 這種迴圈是特別適用於計數或向下計數：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

迴圈開頭`For`關鍵字，後面接著三個項目：

- 之後立即`For`陳述式，宣告計數器變數 (不需要使用`Dim`) 並在指定範圍， `i = 10 to 20`。 這表示變數`i`會在 10 開始計算，並繼續進行直到達到 20 （含）。
- 之間`For`和`Next`陳述式是在區塊的內容。 這可以包含一或多個程式碼執行陳述式與每個迴圈。
- `Next i`陳述式會結束迴圈。 它會遞增計數器，並開始迴圈的下一個反覆項目。

程式碼之間的行`For`和`Next`行包含迴圈的每個反覆項目執行的程式碼。 標記會建立新的段落 (`<p>`元素) 在每次且會將行加入輸出中顯示的值 i （計數器）。 當您執行此頁面時，此範例會建立 11 行顯示輸出，以指出項目編號每一行文字。

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

如果您使用集合或陣列，您通常使用`For Each`迴圈。 集合是一組類似的物件，而`For Each`迴圈可讓您執行集合中每個項目的工作。 這種類型的迴圈很方便的集合，因為與不同的是`For`迴圈中，您不必遞增計數器或設定的限制。 相反地，`For Each`迴圈的程式碼會繼續執行此集合直到完成為止。

這個範例會傳回中的項目`Request.ServerVariables`集合 （其中包含您的 web 伺服器的相關資訊）。 它會使用`For Each`迴圈以顯示每個項目的名稱來建立新`<li>`HTML 項目符號清單中的項目。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

`For Each`關鍵字後面都代表單一項目集合中的變數 (在範例中， `myItem`)，後面接著`In`關鍵字，後面接著您想要重複使用的集合。 本文的`For Each`迴圈中，您可以存取目前的項目，使用您稍早宣告的變數。

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

若要建立更通用的迴圈，請使用`Do While`陳述式：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

此迴圈開頭`Do While`關鍵字，後面接著條件，後面接著要重複的區塊。 通常遞增迴圈 （加入） 或遞減 （被減數） 變數或物件用來計算。 在範例中，`+=`運算子增加 1 變數的值在每次執行迴圈。 (以遞減的變數向下計數在迴圈中，您會使用遞減運算子`-=`。)

## <a name="objects-and-collections"></a>物件和集合

幾乎所有的作業在 ASP.NET 網站是物件，包括網頁本身。 本節討論一些重要的物件，您將使用經常在您的程式碼中。

### <a name="page-objects"></a>頁面物件

在 ASP.NET 中的最基本物件是的頁面。 您可以存取直接沒有任何合格的物件的頁面物件的屬性。 下列程式碼取得網頁的檔案路徑，使用`Request`頁面的物件：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

您可以使用屬性的`Page`物件取得許多資訊，例如：

- `Request`. 如您所見，這是目前的要求，包括何種瀏覽器所做要求的頁面、 使用者識別、 等等的 URL 的相關資訊的集合。
- `Response`. 這是會在伺服端程式碼執行完成時傳送到瀏覽器的回應 （頁面） 的相關資訊的集合。 例如，您可以使用此屬性寫入至回應的資訊。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>（陣列和字典） 的集合物件

集合是一組相同的類型，例如的集合物件的`Customer`資料庫中的物件。 ASP.NET 包含許多的內建集合，例如`Request.Files`集合。

您通常會使用集合中的資料。 兩個常見的集合型別是*陣列*和*字典*。 當您想要儲存的類似項目集合，但不想要建立個別的變數來保存每個項目時，陣列會是很有用：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

使用陣列時，您宣告為特定的資料類型，例如`String`， `Integer`，或`DateTime`。 若要表示變數可以包含陣列，請在您的宣告中變數的名稱加上括號 (例如`Dim myVar() As String`)。 您可以存取項目中使用它們的位置 （索引） 的陣列，或使用`For Each`陳述式。 陣列索引以零為起始&#8212;也就是第一個項目是在位置 0，第二個項目位於位置 1，依此類推。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

您可以判斷陣列中的項目數目，藉由取得其`Length`屬性。 若要取得特定項目的陣列中的位置 (也就是搜尋陣列)，使用`Array.IndexOf`方法。 您也可以執行像是反向陣列的內容 (`Array.Reverse`方法) 或排序內容 (`Array.Sort`方法)。

在瀏覽器中顯示的字串陣列程式碼的輸出：

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

字典是索引鍵/值組的集合，您提供的索引鍵 （或名稱） 來設定或擷取對應的值：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

若要建立字典，請使用`New`關鍵字表示，您要建立新`Dictionary`物件。 您可以將變數使用字典`Dim`關鍵字。 指出資料類型的字典使用括號中的項目 ( `( )` )。 在宣告的結尾，您必須加入另一組括號，因為這是實際的方法，建立新的字典。

若要新增至字典的項目，您可以呼叫`Add`字典變數的方法 (`myScores`在此情況下)，然後指定索引鍵和值。 或者，您可以使用括號表示索引鍵，並執行簡單指派，如下列範例所示：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

若要從字典取得值，您可以指定索引鍵在括號中：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>呼叫具有參數的方法

如同稍早在本文中，您以程式設計的物件有方法。 例如，`Database`物件可能會有`Database.Connect`方法。 許多方法也有一或多個參數。 A*參數*是傳遞至方法的值以啟用方法，以完成其工作。 例如，查看的宣告`Request.MapPath`方法，這個方法會採用三個參數：

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

這個方法會傳回至指定的虛擬路徑對應的伺服器上的實體路徑。 方法有三個參數`virtualPath`， `baseVirtualDir`，和`allowCrossAppMapping`。 （請注意在宣告中，列出參數的資料，它們會接受的資料類型）。當您呼叫這個方法時，您必須提供所有的三個參數的值。

當您使用 Visual Basic 含有 Razor 語法時，您有兩個選項傳遞至方法的參數：*位置參數*或*具名參數*。 若要呼叫方法，使用位置參數，您傳入參數嚴格的順序，方法宣告中所指定。 （您將通常知道此順序閱讀文件的方法）。您必須遵循的順序，並不能略過任何參數&#8212;如果有必要，您傳遞空字串 (`""`) 或您不需要的值是位置參數為 null。

下列範例假設您有一個名為資料夾*指令碼*在您的網站。 程式碼會呼叫`Request.MapPath`依正確順序的三個參數的方法，並傳遞值。 然後，它會顯示產生的對應的路徑。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

當有許多方法的參數時，您可以將程式碼較為簡潔且更容易閱讀使用具名參數。 若要呼叫的方法使用具名的參數，指定參數名稱，後面接著`:=`，然後提供值。 具名參數的優點是您可以在任何您想要的順序新增它們。 （的缺點是在方法呼叫不是精簡）。

下列範例會呼叫上述方法，但使用具名參數，以提供值：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

如您所見，將參數傳遞不同的順序。 不過，如果您執行上述範例，此範例中，它們會傳回相同的值。

## <a name="handling-errors"></a>處理錯誤

### <a name="try-catch-statements"></a>Try Catch 陳述式

您通常會需要陳述式可能失敗的原因，您無法控制的程式碼。 例如: 

- 如果您的程式碼嘗試開啟、 建立、 讀取或寫入檔案時，可能會發生各種錯誤。 您想要的檔案可能不存在，可能會遭到鎖定，程式碼可能不具有權限，等等。
- 同樣地，如果您的程式碼會嘗試以更新資料庫中的記錄，可以有權限問題、 可能會卸除資料庫的連接、 要儲存的資料可能不正確，依此類推。

在程式設計的詞彙，這些情況下會呼叫*例外狀況*。 如果您的程式碼遇到例外狀況，則會產生 （會擲回） 錯誤訊息也就是，最多只能惱人給使用者。

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

在您的程式碼可能會遇到例外狀況的情況下，以及為了避免此類型的錯誤訊息，您可以使用`Try/Catch`陳述式。 在`Try`陳述式中，執行您正在檢查的程式碼。 一或多個`Catch`陳述式，您可以尋找特定可能發生的錯誤 （特定類型的例外狀況）。 您可以包含最大數量`Catch`陳述式，當您需要尋找您所預期的錯誤。

> [!NOTE]
> 我們建議您避免使用`Response.Redirect`方法中的`Try/Catch`陳述式，因為它會在頁面中造成例外狀況。


下列範例顯示建立第一次要求的文字檔，然後顯示按鈕，可讓使用者開啟檔案的頁面。 此範例刻意使用不正確的檔案名稱，因此它將會導致例外狀況。 程式碼包含`Catch`陳述式中的兩個可能的例外狀況： `FileNotFoundException`，發生錯誤，檔案名稱是否與`DirectoryNotFoundException`，發生於 ASP.NET 甚至無法尋找資料夾。 （您可以取消此範例中的陳述式才能看到一切正常運作時，它的執行方式註解）。

如果您的程式碼未處理例外狀況，您會看到錯誤頁面類似上一個螢幕擷取畫面。 不過，`Try/Catch`一節可協助防止使用者看到這些類型的錯誤。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>其他資源

### <a name="reference-documentation"></a>參考文件

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Visual Basic 語言](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
