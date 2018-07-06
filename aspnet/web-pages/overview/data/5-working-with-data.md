---
uid: web-pages/overview/data/5-working-with-data
title: 簡介使用的資料庫，在 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 本章說明如何從資料庫存取資料，並顯示其使用 ASP.NET Web Pages。
ms.author: aspnetcontent
ms.date: 02/18/2014
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 5185769530cf78c301f2ac43b25dba6e77ca75a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808502"
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>簡介使用的資料庫，在 ASP.NET Web Pages (Razor) 網站
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何使用 Microsoft WebMatrix 工具來建立資料庫，在 ASP.NET Web Pages (Razor) 網站中，以及如何建立可讓您顯示、 新增、 編輯和刪除資料的頁面。
> 
> **您將學到什麼：** 
> 
> - 如何建立資料庫。
> - 如何連接到資料庫。
> - 如何在網頁上顯示資料。
> - 如何插入、 更新和刪除資料庫中的記錄。
> 
> 以下是文件中所引進的功能：
> 
> - 使用 Microsoft SQL Server Compact Edition 資料庫。
> - 使用 SQL 查詢。
> - `Database` 類別。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> 本教學課程也適用於 WebMatrix 3。 您可以使用 ASP.NET Web Pages 3 和 Visual Studio 2013 （或 Visual Studio Express 2013 for Web）;不過，使用者介面將會不同。


## <a name="introduction-to-databases"></a>資料庫的簡介

想像一下一個典型的通訊錄。 通訊錄中的每個項目 (也就是為每個人) 有幾個部分的資訊，例如名字、 姓氏、 地址、 電子郵件地址和電話號碼。

就像這樣的圖片資料的典型方式是為包含資料列和資料行的資料表。 在資料庫詞彙中，每個資料列通常稱為一筆記錄。 （有時稱為 「 欄位 」） 的每個資料行包含每種類型的資料值： 名字、 最後一個名稱和等等。

| **ID** | **FirstName** | **lastName** | **地址** | **電子郵件** | **電話** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100 St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234 Main St.Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

針對大部分的資料庫資料表，則資料表必須有包含唯一的識別碼，例如客戶編號、 客戶編號等的資料行。這就所謂的資料表*主索引鍵*，並使用它來識別資料表中的每個資料列。 在範例中，識別碼資料行是通訊錄的主索引鍵。

這個基本的了解的資料庫，就可以了解如何建立簡單的資料庫，並執行作業，例如加入、 修改和刪除資料。

> [!TIP] 
> 
> **關聯式資料庫**
> 
> 您可以將資料儲存在許多種，包括文字檔案和試算表。 不過，為大部分的商業用途，資料會儲存在關聯式資料庫中。
> 
> 這篇文章不很多心力進入資料庫。 不過，您可能會發現有點瞭解它們很有用。 在關聯式資料庫中，資訊會邏輯區分為不同的資料表。 比方說，學校裡的資料庫可能包含不同的資料表，適用於學生和類別供應項目。 資料庫軟體 （例如 SQL Server) 支援強大命令，可讓您以動態方式建立資料表之間的關聯性。 例如，您可以使用關聯式資料庫，以建立學生與類別之間的邏輯關聯性以建立排程。 將資料儲存在個別的資料表的資料表結構的複雜度，並可減少多餘的資料保留在資料表中的需求。


## <a name="creating-a-database"></a>建立資料庫

此程序會示範如何建立名為 SmallBakery，使用 SQL Server Compact 資料庫設計工具包含在 WebMatrix 中的資料庫。 雖然您可以建立資料庫，使用程式碼，更通常會建立資料庫和資料庫資料表，使用像是 WebMatrix 的設計工具。

1. 啟動 WebMatrix，並在 快速入門 頁面中，按一下 **站台從範本**。
2. 選取 [**空白網站**，然後在**站台名稱**] 方塊中輸入 「 SmallBakery"，然後按一下**確定**。 建立並顯示在 WebMatrix 網站。
3. 在左窗格中，按一下**資料庫**工作區。
4. 在 功能區中，按一下**新的資料庫**。 空的資料庫會建立具有相同的名稱，作為您的網站。
5. 在左窗格中，依序展開**SmallBakery.sdf**節點，然後按一下**資料表**。
6. 在 功能區中，按一下**新的資料表**。 WebMatrix 會開啟資料表設計工具。

    ![[影像]](5-working-with-data/_static/image1.jpg)
7. 在中按一下**名稱**資料行，然後輸入&quot;識別碼&quot;。
8. 在 **資料型別**欄中，選取**int**。
9. 設定**是主索引鍵嗎？** 並**是識別嗎？** 選項，以**是**。

    恰如其名**是主索引鍵**會告知資料庫，這會是資料表的主索引鍵。 **身分識別**告知的資料庫自動建立每個新的記錄識別碼，並將它指派下一個循序編號 （從 1 開始）。
10. 按一下 [下一步] 的資料列中。 編輯器會啟動新的資料行定義。
11. 針對名稱值中，輸入&quot;名稱&quot;。
12. 針對**資料型別**，選擇&quot;nvarchar&quot;並將長度設定為 50。 *Var*屬於`nvarchar`會告知資料庫這個資料行的資料會是大小可能會不同記錄間的字串。 ( *n*前置詞代表*國家*，表示欄位可以保留代表任何字母的字元資料，或寫入系統&#8212;也就是，欄位就會保留 Unicode 資料。)
13. 設定**是否允許 null 值**選項設定為**No**。 這將會強制執行所*名稱*資料行不會保留空白。
14. 使用這個相同的程序，建立名為資料行*描述*。 設定**資料型別**"nvarchar"和長度，並將設定 50**允許 Null**設為 false。
15. 建立名為資料行*價格*。 設定 **"money"資料類型**並設定**允許 Null**設為 false。
16. 在頂端方塊中，資料表的命名&quot;產品&quot;。

    當您完成時，定義會如下所示：

    ![[影像]](5-working-with-data/_static/image2.jpg)
17. 按下 Ctrl + S 可儲存資料表。

## <a name="adding-data-to-the-database"></a>將資料加入至資料庫

現在您可以將一些範例資料加入您在本文稍後將使用的資料庫。

1. 在左窗格中，依序展開**SmallBakery.sdf**節點，然後按一下**資料表**。
2. 以滑鼠右鍵按一下 產品 資料表，然後按一下 **資料**。
3. 在 [編輯] 窗格中，輸入下列記錄：

    | **名稱** | **描述** | **價格** |
    | --- | --- | --- |
    | 階層 | 全新內建每一天。 | 2.99 |
    | Strawberry Shortcake | 對建立有機 strawberries 從我們的處理序區。 | 9.99 |
    | Apple 的圓形圖 | 只以您的母親派形的第二個。 | 12.99 |
    | Pecan 圓形圖 | 如果您要 pecans，這很適合您。 | 10.99 |
    | 檸檬的圓形圖 | 使用全球最佳 lemons 提出。 | 11.99 |
    | Cupcakes | 您的孩子和您的孩子一定會喜歡這些。 | 7.99 |

    請記住，您不必進行任何的輸入*識別碼*資料行。 您在建立時*識別碼*資料行，設定其**為識別**屬性設為 true，這會導致它自動填入。

    當您完成輸入資料時，資料表設計工具看起來像這樣：

    ![[影像]](5-working-with-data/_static/image3.jpg)
4. 關閉 包含的資料庫資料的索引標籤。

## <a name="displaying-data-from-a-database"></a>顯示資料庫中的資料

一旦您在其中有資料的資料庫，您就可以在 ASP.NET 網頁中顯示資料。 若要選取要顯示的資料表資料列，您可以使用 SQL 陳述式，也就是您傳遞至資料庫的命令。

1. 在左窗格中，按一下**檔案**工作區。
2. 在網站根目錄中，建立名為新的 CSHTML 頁面*ListProducts.cshtml*。
3. 以下列內容取代現有的標記：

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    在第一個程式碼區塊中，開啟*SmallBakery.sdf*您稍早建立的檔案 （資料庫）。 `Database.Open`方法會假設 *.sdf*檔案位於您的網站*應用程式\_資料*資料夾。 (請注意，您不需要指定 *.sdf*延伸模組&#8212;事實上，如果您這樣做，`Open`方法將無法運作。)

    > [!NOTE]
    > *應用程式\_資料*資料夾是用來儲存資料檔案的 ASP.NET 中的特殊資料夾。 如需詳細資訊，請參閱 <<c0> [ 連接到資料庫](#SB_ConnectingToADatabase)本文稍後。

    然後您要查詢資料庫以使用下列 SQL 要求`Select`陳述式：

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    在陳述式，`Product`識別要查詢的資料表。 `*`字元可讓您指定查詢應該從資料表傳回所有資料行。 （您可能也列出資料行個別以逗號分隔，如果您想要查看只是某些資料行。）`Order By`子句會指出應該如何排序資料&#8212;在此情況下，由*名稱*資料行。 這表示，資料是依字母順序根據排序的值*名稱*每個資料列的資料行。

    在頁面的主體中，標記會建立 HTML 資料表是用來顯示資料。 內部`<tbody>`項目，您使用`foreach`迴圈以個別取得每個查詢所傳回的資料列。 針對每個資料列中，您會建立 HTML 資料表資料列 (`<tr>`項目)。 然後建立 HTML 資料表儲存格 (`<td>`項目) 的每個資料行。 每次您會執行迴圈時下, 一個可用的資料列，從資料庫處於`row`變數 (您設定此功能`foreach`陳述式)。 若要取得個別資料行從資料列，您可以使用`row.Name`或`row.Description`或任何的名稱是資料行的您想要。
4. 執行網頁瀏覽器中。 (請確定中選取頁面**檔案**才能執行這個工作區。)頁面會顯示清單中的，如下所示：

    ![[影像]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **結構化的查詢語言 (SQL)**
> 
> SQL 是一種語言，在大部分的關聯式資料庫中用於管理資料庫中的資料。 它包含的指令，可讓您擷取資料，並加以更新，以及，可讓您建立、 修改和管理資料庫的資料表。 SQL 是不同的程式語言 （例如您在 WebMatrix 中使用的那一個） 因為 sql 的概念是，您需要告訴資料庫想什麼方法，它是資料庫的作業，以了解如何取得資料，或執行工作。 以下是一些 SQL 命令的範例，以及他們如何：
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> 這會擷取*識別碼*，*名稱*，並*價格*資料行中的記錄*產品*資料表的值*價格*超過 10，並將結果傳回依字母順序的值為基礎*名稱*資料行。 此命令會傳回結果集，其中包含符合準則或空的集合，如果沒有記錄符合的記錄。
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> 這會插入到新的資料錄*產品*資料表中，設定*名稱*資料行&quot;可頌麵包&quot;，則*描述*資料行&quot;不穩定的福音&quot;，和 1.99 價格。
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> 此命令會刪除記錄檔中的*產品*其到期的日期資料行是早於 2008 年 1 月 1 日的資料表。 (這是假設*產品*資料表中有這類資料行，當然。)在此處以 MM/DD/YYYY 格式輸入日期，但它應該使用於您的地區設定的格式輸入。
> 
> `Insert Into`和`Delete`命令未傳回結果集。 相反地，它們會傳回一個數字，會告訴您命令影響的多少筆記錄。
> 
> 其中一些作業 （例如插入和刪除記錄），要求作業的程序必須在資料庫中有適當的權限。 這是您通常需要提供使用者名稱和密碼，當您連接到資料庫的生產資料庫的原因。
> 
> 有數十個 SQL 命令，不過都會遵循類似的模式。 您可以使用 SQL 命令來建立資料庫資料表、 計算資料表中的記錄數目、 計算價格，以及執行許多更多的作業。


## <a name="inserting-data-in-a-database"></a>在資料庫中插入資料

本節說明如何建立可讓使用者新增至新的產品網頁*產品*資料庫資料表。 插入新的產品記錄後，此頁面會顯示更新過的資料表使用*ListProducts.cshtml*您在上一節中建立的頁面。

此頁面包含驗證，請確定使用者輸入的資料是有效的資料庫。 比方說，在頁面的程式碼可確保已針對所有必要的資料行輸入值。

1. 在網站中，建立新的 CSHTML 檔案，名為*InsertProducts.cshtml*。
2. 以下列內容取代現有的標記：

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    頁面的主體會包含一個 HTML 表單，以讓使用者輸入名稱、 描述和價格的三個文字方塊。 當使用者按一下**插入** 按鈕，在頁面頂端的程式碼會開啟的連接*SmallBakery.sdf*資料庫。 接著您可取得的值，使用者已送出使用`Request`物件，並將這些值指派給本機變數。

    若要驗證使用者輸入的值，每個必要的資料行，您註冊每個`<input>`您想要驗證的項目：

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    `Validation`協助程式會檢查每個已註冊的欄位中沒有值。 您可以測試是否所有欄位都通過驗證檢查`Validation.IsValid()`，您通常會執行之前處理您從使用者取得的資訊：

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (`&&`運算子表示 AND — 這項測試會*如果這是表單送出，且所有欄位都已都通過驗證*。)

    如果驗證所有的資料行 （none 為空），您可以繼續並建立將資料插入，，然後執行它，如下所示的 SQL 陳述式：

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    要插入值，您可以包含參數替代符號 (`@0`， `@1`， `@2`)。

    > [!NOTE]
    > 基於安全性考量，一律將值傳遞至 SQL 陳述式使用的參數，您在上述範例中看到。 這可讓您有機會驗證使用者的資料，再加上有助於防止惡意的命令傳送到您的資料庫 （有時稱為 SQL 插入式攻擊）。

    若要執行查詢時，您使用此陳述式，將包含要取代的預留位置值的變數傳遞給它：

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    之後`Insert Into`陳述式已執行，您將使用者傳送至頁面，其中列出使用這行的產品：

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    如果驗證不成功，則會略過 insert。 相反地，您有 helper 可以顯示的累積的錯誤訊息 （如果有的話） 頁面中：

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    請注意在標記中的樣式區塊包含名為 CSS 類別定義`.validation-summary-errors`。 這是預設值由 CSS 類別名稱`<div>`包含任何驗證錯誤的項目。 驗證摘要錯誤以紅色顯示，並以粗體，但您可以定義 CSS 類別會在此情況下，指定`.validation-summary-errors`類別，以顯示您所喜歡的任何格式。

### <a name="testing-the-insert-page"></a>測試插入頁面

1. 在瀏覽器中檢視網頁。 此頁面會顯示類似下圖顯示一個表單。

    ![[影像]](5-working-with-data/_static/image5.jpg)
2. 輸入值的所有資料行，但請確定您離開*價格*資料行保留空白。
3. 按一下 [插入] 。 頁面會顯示錯誤訊息，如下圖所示。 （建立新的記錄）。

    ![[影像]](5-working-with-data/_static/image6.jpg)
4. 完整填寫表單，然後按一下**插入**。 此時*ListProducts.cshtml*頁面隨即出現並顯示新的記錄。

## <a name="updating-data-in-a-database"></a>更新資料庫中的資料

沒有輸入資料到資料表之後，您可能需要更新它。 此程序會示範如何建立兩個頁面，類似於您為資料插入稍早建立的項目。 第一頁會顯示產品，並可讓使用者選取其中一個來變更。 第二頁可讓使用者實際上進行編輯，並將其儲存。

> [!NOTE] 
> 
> **重要**在生產網站中，您通常會限制誰可以對資料進行變更。 有關如何設定成員資格，以及有關如何授權使用者在站台上執行工作的相關資訊，請參閱[新增的安全性和 ASP.NET Web Pages 網站的成員資格](https://go.microsoft.com/fwlink/?LinkId=202904)。


1. 在網站中，建立新的 CSHTML 檔案，名為*EditProducts.cshtml*。
2. 將檔案中的現有標記取代為下列：

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    此頁面中唯一的差別並*ListProducts.cshtml*頁面從稍早是在此頁面的 HTML 資料表，包含顯示的額外資料行**編輯**連結。 當您按一下此連結時，它會帶您前往*UpdateProducts.cshtml*頁面上 （您將在接下來建立） 您可以在其中編輯選取的記錄。

    在建立的程式碼看起來**編輯**連結：

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    這會建立 HTML`<a>`項目其`href`屬性以動態方式設定。 `href`屬性會指定當使用者按一下連結時，所顯示頁面。 它也會傳遞`Id`要連結的目前資料列的值。 當執行網頁時，網頁原始檔可能已包含這類的連結：

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    請注意，`href`屬性設定為`UpdateProducts/n`，其中*n*是產品的數字。 當使用者按一下其中一個連結時，產生的 URL 看起來會像這樣：

    `http://localhost:18816/UpdateProducts/6`

    換句話說，若要編輯的產品號碼會在 URL 中傳遞。
3. 在瀏覽器中檢視網頁。 頁面會顯示資料的格式如下：

    ![[影像]](5-working-with-data/_static/image7.jpg)

    接下來，您將建立可讓使用者實際更新的資料頁面。 [更新] 頁面包含驗證來驗證使用者輸入的資料。 比方說，在頁面的程式碼可確保已針對所有必要的資料行輸入值。
4. 在網站中，建立新的 CSHTML 檔案，名為*UpdateProducts.cshtml*。
5. 以下列內容取代現有的標記檔案中。

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    頁面的主體會包含一個 HTML 表單，其中會顯示產品和使用者可以編輯。 若要取得要顯示的產品，您可以使用這個 SQL 陳述式：

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    這樣就會選取的產品的 ID 符合傳入的值`@0`參數。 (因為*識別碼*是主索引鍵，因此必須是唯一的只有一個產品記錄曾選取這種方式。)若要取得識別碼的值傳遞至此`Select`陳述式中，您可以讀取值傳遞至頁面的 url，使用下列語法：

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    若要實際擷取的產品記錄，您使用`QuerySingle`方法，它會傳回只是一筆記錄：

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    單一資料列會傳回成`row`變數。 您可以取得每個資料行中的資料，並將它指派給本機變數，就像這樣：

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    在表單的標記，這些值會自動顯示在個別的文字方塊中使用內嵌程式碼，如下所示：

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    該部分程式碼會顯示更新的產品記錄。 一旦已顯示的記錄，使用者可以編輯個別的資料行。

    當使用者送出表單，即可**更新**按鈕中的程式碼`if(IsPost)`封鎖執行。 這會取得使用者的值從`Request`物件，將值儲存在變數中，並驗證已填寫每個資料行。 如果通過驗證，程式碼會建立下列的 SQL Update 陳述式：

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    在 SQL`Update`陳述式，指定每個更新，並將它設定為值的資料行。 在此程式碼中，值使用指定的參數預留位置`@0`， `@1`， `@2`，依此類推。 （如前文所述，為了安全性，您應該一律傳遞值的 SQL 陳述式所使用的參數。）

    當您呼叫`db.Execute`方法，傳遞包含對應至 SQL 陳述式中參數的順序值的變數：

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    之後`Update`在執行陳述式中，您才能將使用者重新導向回到編輯頁面呼叫下列方法：

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    效果是，使用者會看到資料庫中資料的更新的清單，並可以編輯另一個產品。
6. 儲存網頁。
7. 執行*EditProducts.cshtml*網頁 （不是更新頁面），然後按一下**編輯**選取要編輯的產品。 *UpdateProducts.cshtml*顯示頁面，顯示您所選取的記錄。

    ![[影像]](5-working-with-data/_static/image8.jpg)
8. 進行變更，然後按一下 **更新**。 產品清單會顯示一次，以更新的資料。

## <a name="deleting-data-in-a-database"></a>刪除資料庫中的資料

本節說明如何讓使用者可以刪除中的產品*產品*資料庫資料表。 此範例包含兩個頁面。 在第一個頁面中，使用者會選取要刪除的記錄。 要刪除的記錄即會顯示在第二個頁面，讓他們確認想要刪除的記錄中。

> [!NOTE] 
> 
> **重要**在生產網站中，您通常會限制誰可以對資料進行變更。 有關如何設定成員資格，以及有關如何在站台上執行工作的使用者授與的相關資訊，請參閱[新增的安全性和 ASP.NET Web Pages 網站的成員資格](https://go.microsoft.com/fwlink/?LinkId=202904)。


1. 在網站中，建立新的 CSHTML 檔案，名為*ListProductsForDelete.cshtml*。
2. 以下列內容取代現有的標記：

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    此頁面大致*EditProducts.cshtml*稍早的頁面。 不過，不會顯示**編輯**連結每項產品，它會顯示**刪除**連結。 **刪除**在標記中使用下列的內嵌程式碼來建立連結：

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    這會建立當使用者按一下連結，看起來像這樣的 URL:

    `http://<server>/DeleteProduct/4`

    URL 會呼叫名為的頁面*DeleteProduct.cshtml* （這會建立下一步） 並將它傳遞至刪除的產品識別碼 （在此，4）。
3. 儲存檔案，但將它開啟。
4. 建立另一個名為的 CHTML 檔案*DeleteProduct.cshtml*。 以下列內容取代現有的內容：

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    此頁面會呼叫*ListProductsForDelete.cshtml*也可以讓使用者確認他們想要刪除的產品。 若要列出要刪除的產品，您取得的產品識別碼，刪除從 URL 使用下列程式碼：

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    頁面接著會要求使用者按一下按鈕，即可實際刪除記錄。 這是一項重要的安全性措施： 當您執行敏感性作業在您的網站，例如更新或刪除資料時，這些作業應該永遠是使用 POST 作業，而不是 GET 作業。 如果您的網站已設定，以便可以使用 「 取得 」 作業來執行刪除作業，任何人都可以傳遞之類的 URL`http://<server>/DeleteProduct/4`並從資料庫中刪除它們想要的任何項目。 藉由新增確認，並編碼的頁面，以便可以執行刪除，只能藉由使用某篇文章，以將的安全性的量值加入至您的站台的。

    使用下列程式碼，先確認這是 post 作業，並確認識別碼不是空白，被執行實際的刪除作業：

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    執行 SQL 陳述式會刪除指定的記錄，然後將使用者重新導向回到清單頁面的程式碼。
5. 執行*ListProductsForDelete.cshtml*瀏覽器中。

    ![[影像]](5-working-with-data/_static/image9.jpg)
6. 按一下 **刪除**連結其中一個產品。 *DeleteProduct.cshtml*頁面會顯示確認您想要刪除該記錄。
7. 按一下 [**刪除**] 按鈕。 刪除產品記錄，並使用更新的產品清單重新整理頁面。

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>連接到資料庫
> 
> 您可以連接到有兩種資料庫。 第一種是使用`Database.Open`方法並指定資料庫檔案的名稱 (較少 *.sdf*擴充功能):
> 
> `var db = Database.Open("SmallBakery");`
> 
> `Open`方法會假設。*sdf*檔案位於網站*應用程式\_資料*資料夾。 此資料夾被專為保留資料。 比方說，它有適當的權限，以允許讀取和寫入資料，網站，並基於安全性考量，WebMatrix 不允許存取的檔案從這個資料夾。
> 
> 第二種方式是使用連接字串。 連接字串會包含有關如何連接到資料庫的資訊。 這可包括檔案路徑，或它可以包含在本機或遠端伺服器，以及使用者名稱和密碼來連線到該伺服器上的 SQL Server 資料庫的名稱。 （如果您會將資料保留在集中管理的 SQL Server 版本，例如裝載提供者的網站上您一律使用連接字串來指定資料庫連接資訊。）
> 
> 在 WebMatrix 中，連接字串通常會儲存在名為 XML 檔案*Web.config*。如同名稱所暗示，您可以使用*Web.config*來儲存站台的設定資訊，包括任何連接字串，您的網站可能會需要您網站的根目錄中的檔案。 中的連接字串的範例*Web.config*檔案可能看起來如下所示：
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> 在範例中，連接字串會指向在某處的伺服器執行的 SQL Server 的執行個體中的資料庫 (而不是本機 *.sdf*檔案)。 您會需要更換的適當名稱`myServer`並`myDatabase`，並且指定 SQL Server 登入值`username`和`password`。 （使用者名稱和密碼的值不一定相同當做 Windows 認證，或做為主控提供者已提供您登入伺服器值。 請與您所需要的確切值的系統管理員）。
> 
> `Database.Open`方法是有彈性，因為它可讓您將資料庫的名稱傳遞 *.sdf*檔案或儲存在連接字串名稱*Web.config*檔案。 下列範例示範如何連接到資料庫使用的連接字串的前一個範例所示：
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> 如所述，`Database.Open`方法可讓您將資料庫名稱或連接字串，傳遞，它會找出要使用。 這會非常有用，當您部署 （發行） 網站。 您可以使用 *.sdf*中的檔案*應用程式\_資料*資料夾，當您開發並測試您的網站。 當您將您的站台移至實際伺服器時，您可以使用中的連接字串*Web.config*具有相同名稱的檔案您 *.sdf*檔案，但指向裝載提供者的資料庫&#8212;而完全不必變更程式碼。
> 
> 最後，如果您想要直接使用連接字串，您可以呼叫`Database.OpenConnectionString`方法並傳遞它實際的連接字串而不只在中的名稱*Web.config*檔案。 這可能會很有用，因為某些原因您沒有存取權的連接字串的情況下 (或值，例如 *.sdf*檔案名稱) 之前的頁面會執行。 不過，大部分的情況下，您可以使用`Database.Open`這篇文章中所述。


## <a name="additional-resources"></a>其他資源

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [連接到 SQL Server 或在 WebMatrix 中的 MySQL 資料庫](https://go.microsoft.com/fwlink/?LinkId=208661)
- [在 ASP.NET Web Pages 網站中驗證使用者輸入](https://go.microsoft.com/fwlink/?LinkId=253002)
