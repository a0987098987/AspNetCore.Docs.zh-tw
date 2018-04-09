---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: 導入 ASP.NET Web Pages-顯示資料 |Microsoft 文件
author: tfitzmac
description: 本教學課程會示範如何在 WebMatrix 中建立資料庫，以及如何顯示在網頁中的資料庫資料，當您使用 ASP.NET Web Pages (Razor)。 它會假設 y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 6c66e5fb0a1a49da411286e19c7954f83055c3fd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>導入的 ASP.NET Web Pages-顯示資料
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會示範如何在 WebMatrix 中建立資料庫，以及如何顯示在網頁中的資料庫資料，當您使用 ASP.NET Web Pages (Razor)。 它會假設您已完成透過數列[簡介 ASP.NET Web Pages 程式設計](../introducing-razor-syntax-c.md)。
> 
> 您將學習：
> 
> - 如何使用 WebMatrix 工具來建立資料庫和資料庫資料表。
> - 如何使用 WebMatrix 工具將資料加入至資料庫。
> - 如何顯示網頁上的資料庫中的資料。
> - 如何將 ASP.NET Web Pages 中執行 SQL 命令。
> - 如何自訂`WebGrid`helper 來變更資料顯示方式，以及加入分頁和排序。
>   
> 
> 功能/技術討論：
> 
> - WebMatrix 資料庫工具。
> - `WebGrid` 協助程式。


## <a name="what-youll-build"></a>您將建置

在上一個教學課程中，我們將會介紹至 ASP.NET Web Pages (*.cshtml*檔案)、 基本概念的 Razor 語法和 helper。 在本教學課程中，開始，您將建立實際的 web 應用程式，您將序列的其餘部分使用。 應用程式是簡單的電影應用程式，可讓您檢視、 加入、 變更與刪除電影的相關資訊。

當您完成本教學課程時，您可以檢視此頁面看起來的電影清單：

![WebGrid 顯示，其中包含參數設定到 CSS 類別名稱](displaying-data/_static/image1.png)

但是，若要開始，您必須建立資料庫。

## <a name="a-very-brief-introduction-to-databases"></a>資料庫非常簡短的簡介

本教學課程將提供僅資料庫 briefest 簡介。 如果您曾經資料庫，您可以略過這個簡短的區段。

資料庫包含一或多個包含資訊的資料表&mdash;，例如客戶、 訂單及廠商，或如學生、 老師、 類別和成績的資料表。 就結構而言，資料庫資料表是試算表類似。 假設典型的通訊錄。 通訊錄中的每個項目 (也就是每位人員) 有數個部分的資訊，例如名字、 姓氏、 地址、 電子郵件地址和電話號碼。

![範例資料庫的資料表做為一個簡單的方格](displaying-data/_static/image2.png)

(資料列有時稱為*記錄*，和資料行有時稱為*欄位*。)

對於大部分的資料庫資料表，資料表必須具有包含唯一的值，例如客戶編號、 帳戶號碼等的資料行。 這個值便稱為資料表的*主索引鍵*，並用它來識別資料表中的每個資料列。 在範例中，識別碼資料行是前一範例所示的通訊錄的主索引鍵。

大部分您 web 應用程式中的工作包含讀取出資料庫的資訊，以及顯示在頁面上。 您也經常會從使用者收集資訊並將它加入至資料庫時，或您將修改已經在資料庫中的記錄。 （我們將討論的所有這些作業，在此教學課程集過程中。）

資料庫工作可能會提供很複雜，而且可能需要專業的知識。 這個教學課程的集合，不過，您必須了解只有基本概念，會將所有在說明當您瀏。

## <a name="creating-a-database"></a>建立資料庫

WebMatrix 包含多種工具，讓您輕鬆建立資料庫，以及在資料庫中建立資料表。 (資料庫的結構指資料庫*結構描述*。)設定這個教學課程，您將建立的資料庫中有一份資料表&mdash;電影。

如果您沒有這樣做，開啟 WebMatrix 並開啟您在上一個教學課程中建立的 WebPagesMovies 站台。

在左窗格中，按一下 **資料庫**工作區。

![WebMatrix 資料庫工作區 索引標籤](displaying-data/_static/image3.png)

功能區的變更，以顯示與資料庫相關的工作。 在 功能區中，按一下 **新資料庫**。

![WebMatrix 功能區中的 '新的資料庫' 按鈕](displaying-data/_static/image4.png)

WebMatrix 會建立 SQL Server CE 的資料庫 ( *.sdf*檔案) 做為您網站具有相同名稱&mdash; *WebPagesMovies.sdf*。 (您將不會執行這項操作，但您可以將檔案重新命名任何想要的話，因為它具有*.sdf*擴充功能。)

## <a name="creating-a-table"></a>建立資料表

在 功能區中，按一下 **新資料表**。 WebMatrix 會在新索引標籤中開啟資料表設計工具。（如果無法使用新的資料表選項，請確定在左側樹狀檢視中已選取新的資料庫。）

![WebMatrix 功能區中的 '新 Table' 按鈕](displaying-data/_static/image5.png)

在頂端 （浮水印說 「 輸入資料表名稱 」） 文字方塊中輸入 「 電影 」。

![WebMatrix 資料庫設計工具中輸入資料表名稱](displaying-data/_static/image6.png)

資料表名稱下方的窗格是您在其中定義個別的資料行。 如*電影*資料表在此教學課程中，您會建立只有幾個資料行：*識別碼*，*標題*，*類型*，和*年*.

在**名稱**方塊中，輸入 「 識別碼 」。 輸入的值，就會啟動新的資料行的所有控制項。

索引標籤來**資料型別**清單，然後選擇**int**。這個值會指定 ID 資料行，將包含整數 （值） 的資料。

> [!NOTE]
> 我們將不會呼叫它任何這裡進一步說明 （遠），但是您可以使用標準 Windows 鍵盤手勢來瀏覽此方格中。 比方說，您可以索引標籤上，欄位之間，您可以只開始輸入，就可以在清單中，選取項目等等。


索引標籤上超過**預設值**方塊 （也就是保留空白）。 索引標籤來**是主索引鍵**核取方塊，並加以選取。 此選項會告知的資料庫，*識別碼*資料行會包含識別個別資料列的資料。 （也就是每個資料列將可用來尋找該資料列的識別碼資料行具有唯一值）。

選擇**為識別**選項。 此選項會告訴資料庫應該自動產生下一個循序號碼，每個新的資料列。 (**為識別**選項適用於只有當您也選取**是主索引鍵**選項。)

按一下 下一步的方格資料列，或按兩次以完成目前的資料列的索引標籤。 其中一個軌跡會儲存目前的資料列，並啟動下一個。 請注意，**預設值**資料行現在顯示**Null**。 （預設值是 null 的預設值，所以。）

當您完成定義新**識別碼**資料行，在設計工具看起來像下圖：

![WebMatrix 定義電影資料表的識別碼資料行之後的資料庫設計工具](displaying-data/_static/image7.png)

若要建立下一個資料行，按一下方塊中**名稱**資料行。 資料行中輸入"Title"，然後選取**nvarchar**如**資料型別**值。 "Var"一部分**nvarchar**告訴資料庫，此資料行的資料將會大小可能會記錄記錄不同的字串。 ("N"前置詞代表"national，"表示欄位可以包含任何字母或書寫系統的字元資料，亦即，欄位就會保留 Unicode 資料。)

當您選擇**nvarchar**，另一個方塊會顯示您可以在其中輸入欄位的最大長度。 輸入 50，假設您將在本教學課程使用沒有電影標題將會超過 50 個字元。

略過**預設值**清除**允許 Null**選項。 您不想要允許任何沒有標題的電影以輸入到資料庫的資料庫。

當您完成時，並移至下一個資料列時，在設計工具看起來像下圖中：

![WebMatrix 定義電影表格的標題資料行之後的資料庫設計工具](displaying-data/_static/image8.png)

重複這些步驟來建立資料行長度，除了名為 「 類型 」，將它設定為只 30。

建立另一個資料行名為 「 年 」。 資料型別中，選擇  **nchar** (不**nvarchar**) 並將長度設定為 4。 年度您要使用 4 位數的數字"1995"或"2010"，因此您不需要可變動大小的資料行。

以下是完成的設計看起來像：

![WebMatrix 資料庫設計工具之後電影資料表未定義所有的欄位](displaying-data/_static/image9.png)

按下 Ctrl + S 或按一下**儲存**中快速存取工具列按鈕。 關閉索引標籤，以關閉資料庫設計工具。

## <a name="adding-some-example-data"></a>加入一些範例資料

稍後在本教學課程系列，您將建立的頁面，讓您在表單中輸入新的電影。 不過，現在，您可以加入一些您可以在頁面顯示的範例資料。

在**資料庫**在 WebMatrix 中，請注意，有樹，其中顯示您的工作區*.sdf*您稍早建立的檔案。 開啟新的節點*.sdf*檔案，然後再開啟**資料表**節點。

![與樹狀目錄中開啟影片資料表 WebMatrix 資料庫 」 工作區](displaying-data/_static/image10.png)

以滑鼠右鍵按一下**電影**節點，然後選擇 **資料**。 WebMatrix 開啟的方格，您可以在此輸入資料*電影*資料表：

![在 WebMatrix （空白） 資料庫項目方格](displaying-data/_static/image11.png)

按一下**標題**資料行，並輸入"時 Harry 符合 Sally"。 移至**類型**（您可以使用 Tab 鍵） 的資料行，並輸入"浪漫喜劇"。 移至**年**資料行，並輸入"1989":

![在 WebMatrix 有一筆記錄的資料庫項目方格](displaying-data/_static/image12.png)

按下 Enter，WebMatrix 會將儲存新的電影。 請注意，**識別碼**已填入資料行。

![在 WebMatrix 中的一筆記錄和自動產生識別碼的資料庫項目方格](displaying-data/_static/image13.png)

請輸入其他影片 (比方說，「 不存在與 Wind"，"戲劇"，"1939")。 識別碼資料行是一次填入：

![在 WebMatrix 中兩筆記錄與自動產生 Id 的資料庫項目方格](displaying-data/_static/image14.png)

輸入第三個影片 （例如，"Ghostbusters"、"喜劇"）。 做一個試驗，保留**年**資料行的空白，然後按 Enter。 因為您取消選取 **允許 Null**選項，資料庫便會顯示錯誤：

![如果必要的資料行值保留空白，顯示 「 無效的資料 」 錯誤](displaying-data/_static/image15.png)

按一下**確定**返回並修正的項目 （「 Ghostbusters"的年份 1984年），然後按 Enter 鍵。

填滿之前有 8，或讓數個電影中。 （輸入 8 可讓您更輕鬆地與更新版本的分頁。 但如果太多，輸入幾現在）。實際資料並不重要。

![在 WebMatrix 中任一個記錄的資料庫項目方格](displaying-data/_static/image16.png)

如果您沒有任何錯誤的所有電影中都輸入是連續的識別碼值。 如果您嘗試儲存未完成的電影錄製時，可能不是循序的識別碼。 如果是的話，也沒關係。 數字沒有任何原有的意義，而且唯一很重要的是這些是唯一的*電影*資料表。

關閉包含資料庫設計工具的索引標籤。

現在您可以開啟網頁上顯示這個資料。

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>在頁面中顯示資料，利用 WebGrid 協助程式

若要在網頁中顯示資料，您要使用`WebGrid`協助程式。 此協助程式會顯示在方格或資料表 （資料列和資料行）。 如您所見，您可能會無法縮小方格格式及其他功能。

若要執行的方格，您必須撰寫幾行程式碼。 下列幾行將做為一種幾乎所有您在本教學課程中執行資料存取模式。

> [!NOTE]
> 您實際上有許多選擇可用來顯示資料在頁面上。`WebGrid` helper 只是其中一個。 我們之所以選擇它本教學課程中，因為它最簡單的方式顯示資料，所以相當有彈性。 在下一個教學課程組中，您會看到如何使用更多的"manual"方式來處理在頁面中，可讓您更直接控制如何顯示資料的資料。


在 WebMatrix 中左窗格中，按一下 **檔案**工作區。

您建立新的資料庫處於*應用程式\_資料*資料夾。 如果資料夾不存在，WebMatrix 會建立它為您新的資料庫。 （資料夾可能已經存在於如果您先前已安裝的協助程式。）

在樹狀檢視中，選取網站的根目錄。 您必須選取網站; 的根目錄否則，新的檔案新增到應用程式\_Data 資料夾。

在 功能區中，按一下 **新增**。 在**選擇檔案型別**方塊中，選擇**CSHTML**。

在**名稱**方塊，將新的頁面 」 Movies.cshtml":

!['選擇的檔案類型 對話方塊，顯示 '電影' 頁面](displaying-data/_static/image17.png)

按一下**確定** 按鈕。 WebMatrix 會開啟新檔案，某些基本架構中的項目它。 第一次您將撰寫一些程式碼，請從資料庫取得資料。 然後您會將標記加入頁面以實際顯示資料。

### <a name="writing-the-data-query-code"></a>寫入資料的查詢程式碼

在頁面頂端之間`@{`和`}`字元，輸入下列程式碼。 （請確認您輸入的開頭和右大括號之間的這段程式碼）。

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

第一行會開啟您稍早建立的資料庫是一律的第一個步驟之前執行的資料庫。 您可告知`Database.Open`方法開啟的資料庫名稱。 請注意，您不想加入*.sdf*名稱中。 `Open`方法會假設它正在尋求*.sdf*檔案 (也就是*WebPagesMovies.sdf*) 而且*.sdf*檔案位於*應用程式\_資料*資料夾。 (我們稍早所述，*應用程式\_資料*資料夾已保留，則為此案例是在其中一個位置，ASP.NET 會使該名稱的相關假設。)

開啟資料庫時，它的參考會放入名為的變數`db`。 （這可以命名為任何項目。）`db`變數是最後會與資料庫互動方式。

第二行實際上會擷取資料庫資料使用`Query`方法。 請注意這段程式碼的運作方式：`db`變數有開啟的資料庫的參考，而且您叫用`Query`方法藉由使用`db`變數 (`db.Query`)。

查詢本身是 SQL`Select`陳述式。 （如有點 SQL 的詳細背景，請參閱稍後說明）。在陳述式，`Movies`識別要查詢的資料表。 `*`字元會指定查詢應該從資料表傳回所有資料行。 （您可能也列出資料行個別，並以逗號分隔。）

結果的查詢，如果有的話，會傳回，而且可在 `selectedData`變數。 同樣地，變數可以命名為任何項目。

最後，第三行會告知 ASP.NET 您想要使用的執行個體`WebGrid`協助程式。 您所建立 (*具現化*) 協助程式物件，使用`new`關鍵字，並將它傳遞查詢結果，透過`selectedData`變數。 新`WebGrid`物件，以及在資料庫查詢的結果會提供在`grid`變數。 您將需要該結果立即實際頁面中顯示資料。

在這個階段中，開啟資料庫，您已在 取得資料，而且您已備妥`WebGrid`包含該資料的協助程式。 下一步是建立網頁中的標記。

> [!TIP] 
> 
> **結構化的查詢語言 (SQL)**
> 
> SQL 是一種語言，用來在大部分的關聯式資料庫管理資料庫中的資料。 它包含的命令，可讓您擷取資料並加以更新和，可讓您建立、 修改及管理資料庫資料表中的資料。 SQL 是不同於程式設計語言 （例如 C# 中)。 使用 SQL，您需要告訴資料庫決定，而且它是資料庫的工作，以了解如何取得資料，或執行工作。 以下是一些 SQL 命令的範例，以及它們執行的動作：
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> 第一個`Select`陳述式取得的所有資料行 (所指定`*`) 從*電影*資料表。
> 
> 第二個`Select`陳述式會從記錄中擷取的識別碼、 名稱和價格的資料行*產品*價格資料行值是 10 個以上的資料表。 此命令會傳回結果，依字母順序，根據名稱資料行的值。 如果沒有記錄符合價格準則，則此命令會傳回空集合。
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> 此命令會將新的記錄，到*產品*設 1.99 名稱 欄，"Croissant"和"A 穩定的天堂"，描述資料行價格的資料表。
> 
> 請注意當您正在指定非數值，該值括在單引號 （沒有引號，和在 C#）。 您使用這些周圍文字或日期值，但不是數字周圍的引號。
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> 此命令會刪除記錄檔中的*產品*其到期的日期資料行是早於 2008 年 1 月 1 日的資料表。 (此命令假設*產品*資料表中有這類資料行，當然。)此處以 MM/DD/YYYY 格式輸入日期，但它應該用於您的地區設定的格式輸入。
> 
> `Insert`和`Delete`命令沒有傳回結果集。 相反地，它們會傳回數字，會告訴您命令影響的多少筆記錄。
> 
> 某些這些作業 （例如插入及刪除記錄），要求作業的程序中必須有適當的權限的資料庫。 這就是為什麼您通常需要提供使用者名稱和密碼，當您連接到資料庫的生產資料庫。
> 
> 有許多 SQL 命令，但它們都遵循類似您在這裡看到的命令模式。 您可以使用 SQL 命令來建立資料庫資料表、 計算資料表中的記錄數目，計算價格和執行許多更多的作業。


### <a name="adding-markup-to-display-the-data"></a>加入標記，以顯示資料

內部`<head>`項目，設定內容`<title>`"影片"項目：

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

內部`<body>`項目頁面上，加入下列：

[!code-html[Main](displaying-data/samples/sample3.html)]

就這麼容易！ `grid`變數是在您建立時的值`WebGrid`先前的程式碼中的物件。

在 WebMatrix 樹狀結構檢視中，以滑鼠右鍵按一下 [] 頁面上，選取**瀏覽器中啟動**。 您會看到此頁面類似：

![電影資料表中的預設 WebGrid 協助程式輸出](displaying-data/_static/image18.png)

按一下 排序依據該資料行的資料行標題連結。 能夠按一下標題來排序是內建功能**WebGrid**協助程式。

`GetHtml`方法，如其名所示，會產生標記，顯示的資料。 根據預設，`GetHtml`方法會產生 HTML`<table>`項目。 （如果您想，您可以確認呈現藉由查看網頁瀏覽器中的來源。）

## <a name="modifying-the-look-of-the-grid"></a>修改在格線的外觀

使用`WebGrid`helper 像就很容易，但顯示的結果為純文字。 `WebGrid`協助專家擁有各式各樣的選項可讓您控制資料的顯示方式。 還有瀏覽在本教學課程中有太多，但是這個區段可讓您了解這些選項的部分。 將在這一系列之後的教學課程中涵蓋的一些其他選項。

### <a name="specifying-individual-columns-to-display"></a>指定要顯示的個別資料行

若要開始，您可以指定您想要顯示特定資料行。 根據預設，如您所見，此方格會顯示所有的四個中的資料行*電影*資料表。

在*Movies.cshtml*檔案中，取代`@grid.GetHtml()`您剛才加入下列標記：

[!code-css[Main](displaying-data/samples/sample4.css)]

若要告知協助專家来顯示的資料行，包括`columns`參數`GetHtml`方法並傳入資料行的集合。 在集合中，您可以指定要包含的每個資料行。 您指定的個別資料行加入顯示`grid.Column`物件，並傳入您想要的資料行。 (這些資料行必須包含在 SQL 查詢結果 —`WebGrid`協助程式無法顯示不查詢所傳回的資料行。)

啟動*Movies.cshtml*和同樣地，此時您收到類似下列的其中一個顯示在瀏覽器中的頁面 （請注意，會顯示沒有識別碼資料行）：

![WebGrid 顯示選取的資料行](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>變更格線的外觀

有不少顯示資料行，其中有些會探索在之後的教學課程，在此集合中的更多選項。 現在，本節將介紹您可以在其中設定樣式整個方格的方式。

內部`<head>` 頁面上，只在關閉前一節`</head>`標記中，加入下列`<style>`項目：

[!code-css[Main](displaying-data/samples/sample5.css)]

這個 CSS 標記定義名為類別`grid`， `head`，依此類推。 您也可以將這些樣式定義放在個別*.css*檔案並將其連結的頁面。 （事實上，您將會執行，稍後在此教學課程的集合中。）但為了方便項目在此教學課程，請在相同的頁面，顯示的資料。

現在您可以取得`WebGrid`協助專家使用這些樣式類別。 協助專家擁有許多屬性 (例如， `tableStyle`) 針對這個用途 — 您將 CSS 樣式類別名稱指派給它們，而且該類別名稱呈現標記協助程式 」 所呈現的一部分。

變更`grid.GetHtml`標記，讓它看起來像此程式碼：

[!code-css[Main](displaying-data/samples/sample6.css)]

這裡的新是您已新增的`tableStyle`， `headerStyle`，和`alternatingRowStyle`參數`GetHtml`方法。 尚未設定這些參數加入一點的 CSS 樣式的名稱。

執行網頁，以及這次您看見一個方格，看起來較單純比之前：

![WebGrid 顯示，其中包含參數設定到 CSS 類別名稱](displaying-data/_static/image20.png)

若要查看哪些`GetHtml`產生的方法，您可以查看瀏覽器中網頁的來源。 我們將不會移到這裡，詳細資料，但是很重要的一點是藉由指定的參數如`tableStyle`，造成方格產生 HTML 標記，如下所示：

`<table class="grid">`

`<table>`標記已`class`屬性加入至它所參考的 CSS 規則，您先前加入的其中一個。 此程式碼會示範基本模式&mdash;不同參數`GetHtml`方法，然後產生標記的方法可讓您參考 CSS 類別。 您用來做什麼的 CSS 類別是由您決定。

## <a name="adding-paging"></a>加入分頁

在此教學課程的最後一個工作，您會加入至方格中的分頁。 以滑鼠右鍵現在沒問題，一次顯示您的電影。 但如果您新增的數百個影片，頁面顯示會很長。

網頁程式碼中變更的行，建立`WebGrid`下列程式碼的物件：

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

從之前的唯一差異是您已新增的`rowsPerPage`設為 3 的參數。

執行網頁。 此方格會在建立時間，再加上瀏覽連結，可讓您瀏覽您的資料庫中的電影顯示 3 個資料列：

![使用分頁 WebGrid 顯示](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>接下來

在下一個教學課程中，您將學習如何使用 Razor 和 C# 程式碼取得使用者輸入的表單。 因此您可以找到影片依標題或音樂類型，您會將在搜尋方塊加入至 [影片] 頁面中。

## <a name="complete-listing-for-movies-page"></a>影片頁面的完整清單

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>其他資源

- [使用 Razor 語法的 ASP.NET Web 程式設計簡介](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [上一頁](intro-to-web-pages-programming.md)
> [下一頁](form-basics.md)
