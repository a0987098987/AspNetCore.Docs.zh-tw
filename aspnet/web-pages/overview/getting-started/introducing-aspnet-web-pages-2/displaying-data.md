---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: 簡介 ASP.NET Web Pages-顯示資料 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程會示範如何在 WebMatrix 中建立資料庫，以及如何在網頁中顯示資料庫的資料，當您使用 ASP.NET Web Pages (Razor)。 它會假設 y...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 9158a1f53268daec6e6fbdf003dd73e1d62cc667
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021582"
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>ASP.NET Web Pages 簡介-顯示資料
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會示範如何在 WebMatrix 中建立資料庫，以及如何在網頁中顯示資料庫的資料，當您使用 ASP.NET Web Pages (Razor)。 它假設您已完成透過數列[ASP.NET Web Pages 程式設計簡介](../introducing-razor-syntax-c.md)。
> 
> 您將學到什麼：
> 
> - 如何使用 WebMatrix 工具來建立資料庫和資料庫資料表。
> - 如何使用 WebMatrix 工具將資料加入至資料庫。
> - 如何顯示網頁上的資料庫中的資料。
> - 如何將 ASP.NET Web Pages 中執行的 SQL 命令。
> - 如何自訂`WebGrid`helper 來變更資料顯示，並新增分頁和排序。
>   
> 
> 功能/技術討論：
> 
> - WebMatrix 資料庫工具。
> - `WebGrid` 協助程式。


## <a name="what-youll-build"></a>您將建置

在上一個教學課程中，已向您介紹以 「 ASP.NET Web Pages (*.cshtml*檔案)、 Razor 語法的基本概念和協助程式。 在本教學課程中，您會開始建立您將用於其餘的一系列的實際 web 應用程式。 應用程式是簡單的電影應用程式，可讓您檢視、 新增、 變更和刪除電影的資訊。

當您完成本教學課程中時，您將能夠檢視此頁面看起來的電影清單：

![包含參數的 WebGrid 顯示設 CSS 類別名稱](displaying-data/_static/image1.png)

但是，若要開始，您必須建立資料庫。

## <a name="a-very-brief-introduction-to-databases"></a>資料庫非常簡短的簡介

本教學課程會提供資料庫只送入的介紹。 如果您有資料庫的經驗，您可以略過本節將簡短。

資料庫包含一或多個資料表包含資訊&mdash;，例如客戶、 訂單及廠商，或針對學生、 教師、 類別和成績的資料表。 結構，資料庫資料表，就是試算表等。 想像一下一個典型的通訊錄。 通訊錄中的每個項目 (也就是為每個人) 有幾個部分的資訊，例如名字、 姓氏、 地址、 電子郵件地址和電話號碼。

![以簡單的方格顯示的範例資料庫資料表](displaying-data/_static/image2.png)

(資料列有時稱為*記錄*，和資料行有時稱為*欄位*。)

針對大部分的資料庫資料表，資料表必須有一個資料行包含唯一的值，例如客戶編號、 客戶編號，等等。 這個值就所謂的資料表*主索引鍵*，並使用它來識別資料表中的每個資料列。 在範例中，識別碼資料行是主索引鍵，如先前範例所示的通訊錄。

大部分您在 web 應用程式中執行的工作包含讀取移出資料庫的資訊，以及顯示在頁面上。 您通常也會向使用者收集資訊，並將它新增至資料庫，或您要修改已在資料庫中的記錄。 （我們將涵蓋所有這些作業，在本教學課程組的過程中。）

資料庫工作可能會大幅增長很複雜，而且可能需要專業的知識。 本教學課程組，不過，您必須了解僅基本概念，可將所有會說明當您瀏。

## <a name="creating-a-database"></a>建立資料庫

WebMatrix 包含工具，可讓您輕鬆建立資料庫，且在資料庫中建立資料表。 (資料庫的結構指資料庫*結構描述*。)本教學課程組，您將建立有只有一個資料表中的資料庫，&mdash;電影。

如果您還沒有這麼做，請開啟 WebMatrix，然後開啟您在上一個教學課程中建立 WebPagesMovies 站台。

在左窗格中，按一下**資料庫**工作區。

![WebMatrix 資料庫工作區 索引標籤](displaying-data/_static/image3.png)

功能區的變更，以顯示與資料庫相關的工作。 在 功能區中，按一下**新的資料庫**。

![WebMatrix 功能區中的 [新增資料庫] 按鈕](displaying-data/_static/image4.png)

WebMatrix 會建立 SQL Server CE 的資料庫 ( *.sdf*檔案)，具有相同的名稱作為站台&mdash; *WebPagesMovies.sdf*。 (您不會執行這項操作，但您可以任何想要的話，重新命名檔案，只要其 *.sdf*延伸模組。)

## <a name="creating-a-table"></a>建立資料表

在 功能區中，按一下**新的資料表**。 WebMatrix 會在新的索引標籤中開啟資料表設計工具。（如果無法使用新的資料表選項，請確定新的資料庫在左側樹狀檢視中選取。）

![WebMatrix 功能區中的 '新的資料表' 按鈕](displaying-data/_static/image5.png)

在頂端 （浮水印是 「 輸入資料表名稱"） 的 [文字] 方塊中，輸入 「 Movies 」。

![WebMatrix 資料庫設計工具中輸入資料表名稱](displaying-data/_static/image6.png)

[] 窗格下方的資料表名稱是您定義個別的資料行的位置。 針對*電影*資料表，您將在本教學課程中，建立只有少數的資料行：*識別碼*， *Title*，*內容類型*，和*年*.

在 **名稱**方塊中，輸入 「 識別碼 」。 輸入的值，就會啟動新的資料行的所有控制項。

Tab 鍵移至**資料型別**清單，然後選擇**int**。這個值會指定識別碼資料行，將包含整數 （值） 的資料。

> [!NOTE]
> 我們不會呼叫它任何這裡 （多），但您可以使用標準 Windows 鍵盤手勢來瀏覽此方格中。 比方說，您可以使用 tab 鍵的欄位之間，您可能只要開始輸入，就可以在清單中，選取項目等等。


Tab 跳過**預設值**方塊 （也就是保留為空白）。 Tab 鍵移至**是主索引鍵**核取方塊，然後選取它。 此選項會告知資料庫之*識別碼*資料行會包含識別個別資料列的資料。 （也就是每個資料列會可供您尋找該資料列識別碼資料行中有唯一的值）。

選擇**是識別**選項。 此選項會告訴資料庫應該會自動產生下一個循序號碼，每個新的資料列。 (**是身分識別**只有當您也選取了選項適用於**是主索引鍵**選項。)

在下一步 的 格線 列中，按一下或按兩次以完成目前的資料列的索引標籤。 其中一個軌跡儲存目前的資料列，並啟動下一個。 請注意，**預設值**資料行現在說**Null**。 （預設值是 null 的預設值，就是所謂。）

當您完成定義新**識別碼** 欄中，設計工具看起來像下圖：

![在定義電影資料表的識別碼資料行之後的 WebMatrix 資料庫設計工具](displaying-data/_static/image7.png)

若要建立下一個資料行，按一下 在方塊中，在**名稱**資料行。 資料行中輸入"Title"，然後選取**nvarchar** for**資料型別**值。 "Var"一部分**nvarchar**會告知資料庫這個資料行的資料會是大小可能會不同記錄間的字串。 ("N 名"的前置詞表示 「 國家/地區 」，表示欄位可以保存字元資料的任何字母，或寫入系統 — 亦即，欄位就會保留 Unicode 資料。)

當您選擇**nvarchar**，另一個方塊會顯示您可以在其中輸入欄位的最大長度。 輸入 50，假設您將在本教學課程中使用任何電影標題將會超過 50 個字元。

略過**預設值**並清除**允許 Null**選項。 您不想要允許任何沒有標題的電影要輸入至資料庫的資料庫。

當您完成時，並移至下一個資料列時，設計工具看起來像下圖中：

![在定義電影資料表的標題資料行之後的 WebMatrix 資料庫設計工具](displaying-data/_static/image8.png)

重複下列步驟來建立資料行，名為"Genre"，除了長度，將它設定為只是 30。

建立另一個資料行，名為 「 年 」。 針對資料類型中，選擇**nchar** (不**nvarchar**) 並將長度設定為 4。 年中，您要使用 4 位數數字，例如 「 1995"或"2010"，因此您不需要可變大小的資料行。

以下是完成的設計看起來像：

![WebMatrix 資料庫設計工具的所有欄位都定義之電影資料表之後](displaying-data/_static/image9.png)

按下 Ctrl + S 或按一下**儲存**中快速存取工具列按鈕。 關閉索引標籤，以關閉資料庫設計工具。

## <a name="adding-some-example-data"></a>新增一些範例資料

稍後在本教學課程系列中，您將建立的頁面，您可以在其中輸入表單中新的電影。 不過，現在，您可以新增一些您可以在網頁顯示的範例資料。

在 **資料庫**在 WebMatrix 中，請注意，有樹，其中顯示您的工作區 *.sdf*您稍早建立的檔案。 開啟新的節點 *.sdf*檔案，然後再開啟**資料表**節點。

![WebMatrix 資料庫工作區，以樹狀目錄中開啟電影資料表](displaying-data/_static/image10.png)

以滑鼠右鍵按一下**電影**節點，然後選擇**資料**。 WebMatrix 會開啟一個方格，您可以在此輸入資料*電影*資料表：

![在 WebMatrix 中 （空白） 的資料庫項目格線](displaying-data/_static/image11.png)

按一下  **Title**資料行，然後輸入 「 當 Harry 符合 Sally"。 移至**內容類型**（您可以使用 Tab 鍵） 的資料行，然後輸入 「 浪漫喜劇"。 移至**年份**資料行，然後輸入 「 1989 」:

![在 WebMatrix 中有一筆記錄的資料庫項目格線](displaying-data/_static/image12.png)

按 Enter 鍵和 WebMatrix 將儲存新的電影。 請注意，**識別碼**填入資料行。

![在 WebMatrix 中自動產生的識別碼與一筆記錄的資料庫項目方格](displaying-data/_static/image13.png)

輸入另一部影片 （比方說，「 進入與風力 」，「 劇本 」，「 1939"）。 識別碼資料行已重新填入：

![在 WebMatrix 中兩筆記錄與自動產生的識別碼的資料庫項目格線](displaying-data/_static/image14.png)

輸入 （比方說，「 Ghostbusters"、"喜劇"） 第三個影片。 做一個試驗，離開**年份**資料行保留空白，然後按 Enter。 因為您取消選取 **是否允許 null 值**選項，資料庫便會顯示錯誤：

![如果必要的資料行值保留空白，會顯示 「 無效的資料 」 錯誤](displaying-data/_static/image15.png)

按一下 **確定**返回並修正的項目 （「 Ghostbusters 「 1984 年度），然後按 Enter 鍵。

填寫幾個電影，除非您有 8 左右。 （輸入 8 可讓您更輕鬆地與更新版本的分頁。 但如果這太多，請輸入只是現在的一些）。實際的資料並不重要。

![在 WebMatrix 中任一個記錄的資料庫項目格線](displaying-data/_static/image16.png)

如果您輸入未發生任何錯誤的所有影片，ID 值是連續的。 如果您嘗試儲存未完成的電影錄製時，可能無法循序的識別碼。 如果是的話，也沒關係。 數字沒有任何固有的意義和唯一重要的是，唯一想*電影*資料表。

關閉包含 「 資料庫設計工具的索引標籤。

現在您可以開啟 web 網頁上顯示這項資料。

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>在頁面中顯示資料，使用 WebGrid 協助程式

若要顯示資料在頁面中，您要使用`WebGrid`協助程式。 此協助程式會產生以方格或資料表 （資料列和資料行） 的顯示。 如您所見，我們馬上就能調整的方格格式及其他功能。

若要執行的方格，您必須撰寫幾行程式碼。 這幾行將做為一種幾乎所有您在本教學課程中執行資料存取模式。

> [!NOTE]
> 您實際上有許多選項來顯示資料在頁面的`WebGrid`協助程式只是其中一個。 我們選擇了它在本教學課程，因為它最簡單的方式顯示資料因為它是相當有彈性。 在下一個教學課程組中，您會看到如何使用更多的 「 手動 」 方式，在頁面中，可讓您更直接控制如何顯示資料的資料搭配使用。


在 WebMatrix 中左窗格中，按一下**檔案**工作區。

您所建立的新資料庫處於*應用程式\_資料*資料夾。 如果資料夾不存在，WebMatrix 會建立它的新資料庫。 （資料夾可能已經存在於如果您先前已安裝的協助程式。）

在 [樹狀] 檢視中，選取網站的根目錄。 您必須選取網站; 的根目錄否則，新的檔案可能會加入應用程式\_Data 資料夾。

在 功能區中，按一下**新增**。 在 **選擇 檔案類型**方塊中，選擇**CSHTML**。

在 **名稱**方塊，將新的頁面 」 Movies.cshtml 」:

![選擇檔案類型 對話方塊中顯示 '電影' 頁面](displaying-data/_static/image17.png)

按一下 [**確定**] 按鈕。 WebMatrix 會開啟新的檔案，與在其中一些基本架構項目。 首先，您將撰寫一些程式碼以從資料庫取得資料。 然後您會將標記加入頁面，來顯示資料。

### <a name="writing-the-data-query-code"></a>寫入資料的查詢程式碼

在頁面上，頂端之間`@{`和`}`字元，輸入下列程式碼。 （請確定您的開頭和結尾大括號之間輸入下列程式碼）。

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

第一行會開啟您稍早建立的資料庫是一律的第一個步驟之前執行的資料庫。 您告訴`Database.Open`方法開啟的資料庫名稱。 請注意，不包括 「 *.sdf*名稱中。 `Open`方法會假設它正在尋求 *.sdf*檔案 (亦即*WebPagesMovies.sdf*) 且 *.sdf*檔案位於*的應用程式\_資料*資料夾。 (我們稍早所述，*應用程式\_資料*資料夾已保留; 這種情況下是其中一個位置，ASP.NET 會假設該名稱。)

開啟資料庫時，它的參考會放入名為的變數`db`。 （這可以命名為任何項目。）`db`變數是您所得到與資料庫互動的方式。

實際使用擷取的資料庫資料的第二行`Query`方法。 請注意此程式碼的運作方式：`db`變數參考開啟的資料庫，而且您叫用`Query`方法使用`db`變數 (`db.Query`)。

查詢本身是 SQL`Select`陳述式。 （如有點 SQL 的詳細背景，請參閱稍後說明）。在陳述式，`Movies`識別要查詢的資料表。 `*`字元可讓您指定查詢應該從資料表傳回所有資料行。 （您可能也列出資料行個別，並以逗號分隔。）

查詢的結果，如果有的話，會傳回，並可在`selectedData`變數。 同樣地，變數可以命名為任何項目。

最後，第三個一行會告知 ASP.NET，您想要使用的執行個體`WebGrid`協助程式。 您所建立 (*具現化*) 使用，協助程式物件`new`關鍵字並將它傳遞查詢結果，透過`selectedData`變數。 新`WebGrid`物件，以及在資料庫查詢的結果可透過`grid`變數。 您將實際顯示在頁面的 資料稍後需要該結果。

在這個階段中，開啟資料庫，您已取得資料，且您已備妥`WebGrid`協助程式與該資料。 接下來是建立在頁面的標記。

> [!TIP] 
> 
> **結構化的查詢語言 (SQL)**
> 
> SQL 是一種語言，在大部分的關聯式資料庫中用於管理資料庫中的資料。 它包含的指令，可讓您擷取資料，並加以更新，以及它可讓您建立、 修改及管理資料庫資料表中的資料。 SQL 是不同的程式語言 （例如 C# 中)。 使用 SQL，您需要告訴資料庫想什麼方法，而且資料庫的作業，以了解如何取得資料，或執行工作。 以下是一些 SQL 命令的範例，以及他們如何：
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> 第一個`Select`陳述式會取得所有資料行 (依`*`) 從*電影*資料表。
> 
> 第二個`Select`陳述式會從記錄中擷取的識別碼、 名稱和價格資料行*產品*價格資料行值是 10 個以上的資料表。 此命令會傳回結果，依字母順序，根據 名稱資料行的值。 如果沒有記錄符合價格準則，此命令會傳回空集合。
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> 此命令會將新記錄到*產品*設 1.99 」 可頌麵包 」 和 「 的不穩定愉悅的感覺 」，描述資料行的價格的名稱資料行的資料表。
> 
> 請注意當您指定的非數字值，該值括在單引號 （不雙引號括住，和在 C#）。 您使用這些文字或日期值，但不是數字周圍的引號。
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> 此命令會刪除記錄檔中的*產品*其到期的日期資料行是早於 2008 年 1 月 1 日的資料表。 (此命令假設*產品*資料表中有這類資料行，當然。)在此處以 MM/DD/YYYY 格式輸入日期，但它應該使用於您的地區設定的格式輸入。
> 
> `Insert`和`Delete`命令未傳回結果集。 相反地，它們會傳回一個數字，會告訴您命令影響的多少筆記錄。
> 
> 其中一些作業 （例如插入和刪除記錄），要求作業的程序必須在資料庫中有適當的權限。 這是您通常需要提供使用者名稱和密碼，當您連接到資料庫的生產資料庫的原因。
> 
> 有數十個 SQL 命令，但它們都遵循類似您在這裡看到的命令模式。 您可以使用 SQL 命令來建立資料庫資料表、 計算資料表中的記錄數目、 計算價格，以及執行許多更多的作業。


### <a name="adding-markup-to-display-the-data"></a>新增標記，以顯示資料

內部`<head>`項目，設定內容`<title>`「 Movies 」 的項目：

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

內`<body>`項目 頁面上，新增下列：

[!code-html[Main](displaying-data/samples/sample3.html)]

就這麼容易！ `grid`變數是您建立您在建立時的值`WebGrid`稍早的程式碼中的物件。

在 WebMatrix 樹狀結構檢視中，以滑鼠右鍵按一下  頁面上，然後選取**在瀏覽器中啟動**。 您會看到此頁面類似：

![從電影資料表的預設 WebGrid 協助程式輸出](displaying-data/_static/image18.png)

按一下 排序依據該資料行的資料行標題連結。 能夠按一下標題排序是內建的功能**WebGrid**協助程式。

`GetHtml`方法，正如其名，會產生標記，顯示的資料。 根據預設，`GetHtml`方法會產生 HTML`<table>`項目。 （如果您想，您可以確認呈現藉由查看網頁瀏覽器中的來源。）

## <a name="modifying-the-look-of-the-grid"></a>修改方格的外觀

使用`WebGrid`像您剛剛輸入的協助程式很容易，但是在顯示的結果是純文字。 `WebGrid`協助專家擁有各種選項，可讓您控制資料的顯示方式。 有太多，若要在本教學課程中，瀏覽，但本節可讓您了解這些選項的一部分。 在本系列稍後的教學課程中，將會說明幾個其他選項。

### <a name="specifying-individual-columns-to-display"></a>指定要顯示的個別資料行

若要開始，您可以指定您想要顯示特定資料行。 根據預設，如您所見，此方格會顯示全部的四個欄位的資料行*電影*資料表。

在  *Movies.cshtml*檔案中，取代`@grid.GetHtml()`您剛才加入以下列標記：

[!code-css[Main](displaying-data/samples/sample4.css)]

若要告知協助專家要顯示的資料行，包括`columns`參數`GetHtml`方法並傳入的資料行集合。 在集合中，您可以指定要包含的每個資料行。 指定要顯示包含的個別資料行`grid.Column`物件，並傳入您想要的資料行名稱。 (這些資料行必須包含在 SQL 查詢結果-`WebGrid`協助程式無法顯示不查詢所傳回的資料行。)

啟動*Movies.cshtml*同樣地，而且這次得到如下所示顯示在瀏覽器中的頁面 （請注意，會顯示沒有識別碼資料行）：

![WebGrid 顯示所選的資料行](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>變更格線的外觀

有不少的更多選項，來顯示資料行，其中有些還將探索在稍後的教學課程，在此集合中。 現在，本節將為您介紹您可以在其中設定樣式整個方格的方式。

內部`<head>`頁面上，只在關閉前一節`</head>`標記中加入下列`<style>`項目：

[!code-css[Main](displaying-data/samples/sample5.css)]

這個 CSS 標記會定義名為的類別`grid`， `head`，依此類推。 您也可以將這些樣式定義放在個別 *.css*檔案，並連結至頁面。 （事實上，您會執行，稍後在本教學課程組中。）但為了讓本教學課程簡單項目，它們的同一頁所顯示的資料內。

現在您可以取得`WebGrid`協助程式，才能使用這些樣式類別。 協助專家擁有許多屬性 (例如`tableStyle`) 針對這個用途，您指派給它們的 CSS 樣式類別名稱和類別名稱會轉譯為呈現的標記協助程式的一部分。

變更`grid.GetHtml`標記，所以它現在看起來像此程式碼：

[!code-css[Main](displaying-data/samples/sample6.css)]

這裡的新是您已新增`tableStyle`， `headerStyle`，並`alternatingRowStyle`參數來`GetHtml`方法。 這些參數，已設定為您加入的 CSS 樣式的名稱。

執行網頁，，此時您會看到一個方格，其中看起來較一般比之前：

![包含參數的 WebGrid 顯示設 CSS 類別名稱](displaying-data/_static/image20.png)

若要查看`GetHtml`產生的方法，您可以查看網頁瀏覽器中的來源。 我們不會詳述明瞭，但很重要的一點是，藉由指定參數，例如`tableStyle`，造成資料格以產生 HTML 標記，如下所示：

`<table class="grid">`

`<table>`標記已`class`加入它的屬性會參考其中一個您先前加入的 CSS 規則。 此程式碼會示範基本模式&mdash;不同的參數，如`GetHtml`方法可讓您參考 CSS 類別的方法，然後產生標記。 您用來做什麼的 CSS 類別是由您決定。

## <a name="adding-paging"></a>加入分頁

在本教學課程的最後一個工作，您會將分頁新增至方格。 現在就完全沒問題，一次顯示所有您影片的權限。 但如果您新增數百個電影時，頁面顯示就會很長。

網頁程式碼中建立的行的變更`WebGrid`物件，以下列程式碼：

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

之前的唯一差別是您已新增的`rowsPerPage`設為 3 的參數。

執行網頁。 方格會顯示 3 個資料列，在建立時間，再加上瀏覽連結，可讓您的頁面上透過您的資料庫中的電影：

![使用分頁 WebGrid 顯示](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>接下來的下一步

在下一個教學課程中，您將了解如何使用 Razor 和 C# 的程式碼的形式取得使用者輸入。 因此您可以找到電影的標題或內容類型，您將在搜尋方塊新增至 Movies 頁面。

## <a name="complete-listing-for-movies-page"></a>Movies 頁面的完整清單

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>其他資源

- [使用 Razor 語法的 ASP.NET Web 程式設計簡介](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [上一頁](intro-to-web-pages-programming.md)
> [下一頁](form-basics.md)
