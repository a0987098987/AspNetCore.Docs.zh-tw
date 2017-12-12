---
uid: web-pages/overview/data/5-working-with-data
title: "簡介使用的資料庫中 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件"
author: tfitzmac
description: "本章節描述如何從資料庫中存取資料，並顯示使用 ASP.NET Web Pages。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 460af471a1b0650f8d782d582ce6cd9a06664d5c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>簡介使用的資料庫中 ASP.NET Web Pages (Razor) 站台
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何使用 Microsoft WebMatrix 工具來建立資料庫的 ASP.NET Web Pages (Razor) 網站，以及如何建立可讓您顯示、 加入、 編輯和刪除資料的頁面。
> 
> **您將學習：** 
> 
> - 如何建立資料庫。
> - 如何連接到資料庫。
> - 如何在網頁中顯示的資料。
> - 如何插入、 更新和刪除資料庫的記錄。
> 
> 這些是發行項中所引進的功能：
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


## <a name="introduction-to-databases"></a>資料庫簡介

假設典型的通訊錄。 通訊錄中的每個項目 (也就是每位人員) 有數個部分的資訊，例如名字、 姓氏、 地址、 電子郵件地址和電話號碼。

像這樣的圖片資料的典型方式是為包含資料列和資料行的資料表。 在資料庫詞彙中，每個資料列通常稱為一筆記錄。 每個資料行 （有時候稱為欄位） 包含每種類型的資料值： 名字、 最後一個名稱和等等。

| **ID** | **名字** | **LastName** | **地址** | **電子郵件** | **電話** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100 St SE Orcas 98031 WA | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234 Main St.Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

對於大部分的資料庫資料表，資料表必須具有包含唯一識別碼，例如客戶編號、 帳戶號碼等的資料行。這稱為資料表的*主索引鍵*，並用它來識別資料表中的每個資料列。 在範例中，識別碼資料行是主索引鍵的通訊錄。

這個基本的了解資料庫，您就可以了解如何建立簡單的資料庫和執行作業，例如加入、 修改和刪除資料。

> [!TIP] 
> 
> **關聯式資料庫**
> 
> 您可以在許多種，包括文字檔和試算表中儲存資料。 大多數的業務使用，不過，資料會儲存在關聯式資料庫。
> 
> 這篇文章不非常深進入資料庫。 不過，您可能會發現有助於您了解其相關的一點。 在關聯式資料庫中，資訊會邏輯區分為不同的資料表。 比方說，school 資料庫可能包含不同的資料表，學生版和為類別供應項目。 資料庫軟體 （例如 SQL Server) 支援強大命令，可讓您以動態方式建立資料表之間的關聯性。 例如，您可以使用關聯式資料庫來建立學生與類別之間的邏輯關聯性以建立排程。 將資料儲存在個別的資料表的資料表結構的複雜度，並減少重複的資料保留在資料表中。


## <a name="creating-a-database"></a>建立資料庫

此程序會示範如何建立使用 SQL Server Compact 資料庫設計工具包含在 WebMatrix 中名為 SmallBakery 的資料庫。 雖然您可以建立資料庫，使用程式碼，更通常會建立資料庫和資料庫資料表使用 WebMatrix 設計工具。

1. 啟動 WebMatrix，然後在快速入門 頁面上，按一下**網站從範本**。
2. 選取**空白網站**，然後在**站台名稱** 方塊中輸入"SmallBakery 」，然後按一下**確定**。 建立並顯示在 WebMatrix 網站。
3. 在左窗格中，按一下 **資料庫**工作區。
4. 在 功能區中，按一下 **新資料庫**。 空的資料庫會建立具有相同名稱做為您的網站。
5. 在左窗格中，依序展開**SmallBakery.sdf**節點，然後按一下**資料表**。
6. 在 功能區中，按一下 **新資料表**。 WebMatrix 會開啟資料表設計工具。

    ![[影像]](5-working-with-data/_static/image1.jpg)
7. 在按一下**名稱**資料行，並輸入&quot;識別碼&quot;。
8. 在**資料型別**欄中，選取**int**。
9. 設定**是主索引鍵？**和**是識別？**選項，以**是**。

    正如其名，**是主索引鍵**告知的資料庫，這會是資料表的主索引鍵。 **識別**告知的資料庫自動建立每一筆新記錄的識別碼號碼，以及將它指派下一個循序編號 （從 1 開始）。
10. 按一下 下一列。 編輯器會啟動新的資料行定義。
11. 名稱值中，輸入&quot;名稱&quot;。
12. 如**資料型別**，選擇&quot;nvarchar&quot;並將長度設定為 50。 *Var*屬於`nvarchar`告訴資料庫，此資料行的資料將會大小可能會記錄記錄不同的字串。 (  *n* 前置詞代表*national*，指出欄位可以保存字元資料，代表任何字母，或寫入系統 &#8212; 也就是說，該欄位就會保留 Unicode資料）。
13. 設定**允許 Null**選項設定為**否**。 這將會強制執行的*名稱*資料行不保留空白。
14. 使用這個相同的程序，建立名為資料行*描述*。 設定**資料型別**"nvarchar"和長度，以及設定為 50**允許 Null**設為 false。
15. 建立名為資料行*價格*。 設定**"money"資料類型**並設定**允許 Null**設為 false。
16. 在頂端方塊中，命名資料表&quot;產品&quot;。

    當您完成時，定義看起來像這樣：

    ![[影像]](5-working-with-data/_static/image2.jpg)
17. 按 Ctrl + S 以儲存資料表。

## <a name="adding-data-to-the-database"></a>將資料加入至資料庫

現在您可以將某些範例資料加入至您在本文稍後將使用的資料庫。

1. 在左窗格中，依序展開**SmallBakery.sdf**節點，然後按一下**資料表**。
2. 以滑鼠右鍵按一下 產品 資料表，然後按一下 **資料**。
3. 在 [編輯] 窗格中，輸入下列記錄：

    | **Name** | **說明** | **價格** |
    | --- | --- | --- |
    | 麵包 | 確定最新狀態每一天。 | 2.99 |
    | 草霉 Shortcake | 與有機 strawberries 從此我們處理序區。 | 9.99 |
    | Apple 圓形圖 | 第二個只用於您的母親圓形圖。 | 12.99 |
    | Pecan 圓形圖 | 如果您喜歡 pecans，這是您。 | 10.99 |
    | 檸檬的圓形圖 | 搭配世界最佳 lemons 進行。 | 11.99 |
    | 杯子蛋糕 | 您的孩子和您的孩子會愛上這些。 | 7.99 |

    請記住，您不必輸入任何內容的*識別碼*資料行。 當您建立*識別碼*設定資料行，其**為識別**屬性設定為 true，可讓它自動填入的。

    當您完成輸入資料時，資料表設計工具看起來像這樣：

    ![[影像]](5-working-with-data/_static/image3.jpg)
4. 關閉包含資料庫資料的索引標籤。

## <a name="displaying-data-from-a-database"></a>顯示資料庫的資料

一旦你具有資料的資料庫中，您可以在 ASP.NET web 網頁中顯示資料。 若要選取資料表資料列來顯示，您可以使用 SQL 陳述式，它是您傳遞給資料庫的命令。

1. 在左窗格中，按一下 **檔案**工作區。
2. 在網站的根目錄，會建立名為的新 CSHTML 頁面*ListProducts.cshtml*。
3. 以下列內容取代現有的標記：

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    在第一個程式碼區塊中，開啟*SmallBakery.sdf*您稍早建立的檔案 （資料庫）。 `Database.Open`方法會假設*.sdf*檔案位於您的網站*應用程式\_資料*資料夾。 (請注意，您不需要指定*.sdf*延伸 &#8212; 事實上，如果您這樣做，`Open`方法將無法運作。)

    > [!NOTE]
    > *應用程式\_資料*資料夾是用來儲存資料檔案的 ASP.NET 中的特殊資料夾。 如需詳細資訊，請參閱[連接至資料庫](#SB_ConnectingToADatabase)本文稍後。

    然後您要使用下列的 SQL 資料庫中查詢的要求`Select`陳述式：

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    在陳述式，`Product`識別要查詢的資料表。 `*`字元會指定查詢應該從資料表傳回所有資料行。 （您可能也列出資料行個別，以逗號分隔，如果您想要查看只是某些資料行。）`Order By`子句會指出應該如何排序資料 &#8212; 在此情況下，由*名稱*資料行。 這表示資料已排序的值依字母順序根據*名稱*每個資料列的資料行。

    在網頁主體中，標記會建立可用於顯示資料的 HTML 表格。 內部`<tbody>`項目，使用`foreach`迴圈，以分別取得每個查詢所傳回的資料列。 每個資料列，您會建立一個 HTML 資料表資料列 (`<tr>`項目)。 然後您會建立 HTML 資料表儲存格 (`<td>`項目) 的每個資料行。 每當您瀏覽迴圈下, 一個可用的資料列，從資料庫處於`row`變數 (您設定此項目`foreach`陳述式)。 若要取得個別資料行從資料列，您可以使用`row.Name`或`row.Description`或名稱是資料行的您想要。
4. 執行網頁瀏覽器中。 (請確定中選取頁面**檔案**才能執行這個工作區。)頁面會顯示清單，如下所示：

    ![[影像]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **結構化的查詢語言 (SQL)**
> 
> SQL 是一種語言，用來在大部分的關聯式資料庫管理資料庫中的資料。 它包含的命令，可讓您擷取資料並加以更新和，可讓您建立、 修改及管理資料庫的資料表。 SQL 是不同的程式設計語言 （類似您在 WebMatrix 使用），所以使用 SQL，這個概念，您需要告訴資料庫決定，而且這是資料庫的工作，以了解如何取得資料，或執行工作。 以下是一些 SQL 命令的範例，以及它們執行的動作：
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> 這會擷取*識別碼*，*名稱*，和*價格*記錄中的資料行*產品*資料表的值*價格*超過 10，並將結果傳回的值為基礎的字母順序*名稱*資料行。 此命令會傳回結果集，其中包含符合準則或空的集合，如果沒有記錄符合的記錄。
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> 這會插入新的記錄，到*產品*資料表中，設定*名稱*欄&quot;可頌麵包&quot;、*描述*資料行&quot;穩定的天堂&quot;，和 1.99 價格。
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> 此命令會刪除記錄檔中的*產品*其到期的日期資料行是早於 2008 年 1 月 1 日的資料表。 (這是假設*產品*資料表中有這類資料行，當然。)此處以 MM/DD/YYYY 格式輸入日期，但它應該用於您的地區設定的格式輸入。
> 
> `Insert Into`和`Delete`命令沒有傳回結果集。 相反地，它們會傳回數字，會告訴您命令影響的多少筆記錄。
> 
> 某些這些作業 （例如插入及刪除記錄），要求作業的程序中必須有適當的權限的資料庫。 這是您通常需要提供使用者名稱和密碼，當您連接到資料庫的生產資料庫的原因。
> 
> 有許多 SQL 命令，但它們都遵循類似的模式。 您可以使用 SQL 命令來建立資料庫資料表、 計算資料表中的記錄數目，計算價格和執行許多更多的作業。


## <a name="inserting-data-in-a-database"></a>在資料庫中插入資料

本節示範如何建立可讓使用者新增至新的產品頁面*產品*資料庫資料表。 插入新的產品記錄之後，此頁面會顯示更新的資料表使用*ListProducts.cshtml*您在上一節中建立的頁面。

此頁面包含驗證，以確定使用者輸入的資料是有效的資料庫。 例如，頁面中的程式碼可確保已針對所有必要的資料行輸入值。

1. 在網站上建立名為新 CSHTML 檔案*InsertProducts.cshtml*。
2. 以下列內容取代現有的標記：

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    頁面主體會包含具有三個文字方塊，讓使用者可以輸入名稱、 描述和價格的 HTML 表單。 當使用者按一下**插入** 按鈕，在頁面頂端的程式碼會開啟的連接*SmallBakery.sdf*資料庫。 接著您可取得的值，使用者已送出使用`Request`物件，並將這些值指派給本機變數。

    若要驗證使用者輸入的值，每個必要資料行，您註冊每個`<input>`您想要驗證的項目：

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    `Validation` Helper 會檢查每個已註冊的欄位中沒有值。 您可以測試是否所有欄位都通過驗證檢查`Validation.IsValid()`，而您通常會執行之前處理您從使用者取得的資訊：

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (`&&`運算子表示 AND — 這項測試很*表單送出，且所有欄位都已都通過驗證*。)

    如果驗證所有的資料行 （沒有空白），請繼續進行，並建立 SQL 陳述式，將資料插入，，然後執行它，如下所示：

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    要插入值，您可以包含參數替代符號 (`@0`， `@1`， `@2`)。

    > [!NOTE]
    > 基於安全性考量，一律將值傳遞至 SQL 陳述式使用的參數，上述範例中所示。 這可讓您驗證使用者的資料，再加上有助於防止惡意的命令傳送到您的資料庫 （有時稱為 SQL 資料隱碼攻擊） 嘗試。

    若要執行查詢時，您使用此陳述式，包含要取代的預留位置值的變數傳遞給它來：

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    之後`Insert Into`陳述式已執行，您將使用者傳送至頁面，其中列出使用此列的產品：

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    如果驗證不成功，則會略過插入。 相反地，您會有協助程式可以顯示的累積的錯誤訊息 （如果有的話） 的頁面中：

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    請注意，在標記中的樣式區塊包含名為的 CSS 類別定義`.validation-summary-errors`。 這是使用預設的 CSS 類別名稱`<div>`包含任何驗證錯誤的項目。 驗證摘要錯誤會以紅色顯示，並以粗體，但您可以定義 CSS 類別會在此情況下，指定`.validation-summary-errors`類別，以顯示任何您喜歡的格式。

### <a name="testing-the-insert-page"></a>測試插入頁面

1. 在瀏覽器中檢視網頁。 頁面會顯示類似下圖中所顯示的表單。

    ![[影像]](5-working-with-data/_static/image5.jpg)
2. 輸入值的所有資料行，但請確定您保留*價格*空白的資料行。
3. 按一下 [插入] 。 頁面會顯示錯誤訊息，如圖所示。 （會不建立任何新的記錄）。

    ![[影像]](5-working-with-data/_static/image6.jpg)
4. 完整地填寫表單，然後按一下 **插入**。 這次*ListProducts.cshtml*頁面隨即出現並顯示新的記錄。

## <a name="updating-data-in-a-database"></a>更新資料庫中的資料

資料輸入到資料表之後，您可能需要加以更新。 此程序會示範如何建立會類似於您為資料插入稍早建立的兩個頁面。 第一頁會顯示產品，並可讓使用者選取其中一個來變更。 第二頁可讓使用者實際進行編輯，並將其儲存。

> [!NOTE] 
> 
> **重要**在生產網站，您通常會限制已獲允許的人員進行變更的資料。 如需如何設定成員資格以及方式來授權使用者在站台上執行工作的相關資訊，請參閱[加入安全性和 ASP.NET Web Pages 站台中的成員資格](https://go.microsoft.com/fwlink/?LinkId=202904)。


1. 在網站上建立名為新 CSHTML 檔案*EditProducts.cshtml*。
2. 以下列內容取代檔案中現有的標記：

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    此頁面之間的唯一差異和*ListProducts.cshtml*頁面從先前是 HTML 表格，在這個頁面中的包含額外的資料行，顯示**編輯**連結。 當您按一下此連結時，它會帶您前往*UpdateProducts.cshtml*頁面上 （您將接著建立），您可以在其中編輯選取的記錄。

    查看程式碼會建立**編輯**連結：

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    這會建立 HTML`<a>`項目其`href`動態設定屬性。 `href`屬性會指定當使用者按一下連結時所顯示頁面。 它也會傳遞`Id`要連結的目前資料列的值。 當執行網頁時，網頁原始檔可能包含這些連結：

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    請注意，`href`屬性設為`UpdateProducts/n`，其中 *n* 是產品的數字。 當使用者按一下其中一個連結時，產生的 URL 看起來會像這樣：

    `http://localhost:18816/UpdateProducts/6`

    換句話說，可供編輯的產品號碼將會在 URL 中傳遞。
3. 在瀏覽器中檢視網頁。 頁面會顯示資料的格式如下：

    ![[影像]](5-working-with-data/_static/image7.jpg)

    接下來，您將建立可讓使用者實際更新的資料頁面。 [更新] 頁面包含驗證來驗證使用者輸入的資料。 例如，頁面中的程式碼可確保已針對所有必要的資料行輸入值。
4. 在網站上建立名為新 CSHTML 檔案*UpdateProducts.cshtml*。
5. 以下列內容取代檔案中現有的標記。

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    頁面主體會包含 HTML 表單，其中會顯示產品和使用者可以編輯。 若要取得要顯示的產品，您可以使用此 SQL 陳述式：

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    這樣會選取其識別碼會傳入的值相符的產品`@0`參數。 (因為*識別碼*是主索引鍵，因此必須是唯一的只能有一個產品記錄曾選取這種方式。)若要取得的識別碼值，傳遞至這個`Select`陳述式中，您可以讀取值傳遞至頁面為組件的 URL，請使用下列語法：

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    若要實際擷取的產品記錄，您使用`QuerySingle`方法，將會傳回一筆記錄：

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    單一資料列會傳回成`row`變數。 您可以取得每個資料行的資料，並將它指派給本機變數，就像這樣：

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    在表單的標記中，這些值會自動顯示在個別的文字方塊中使用內嵌程式碼，如下所示：

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    一部分的程式碼會顯示更新的產品記錄。 一旦已顯示的記錄，使用者可以編輯個別資料行。

    當使用者提交表單，即可**更新**按鈕中的程式碼`if(IsPost)`封鎖執行。 這會取得使用者的值從`Request`物件，將值儲存在變數中，並驗證已填入每個資料行。 如果通過驗證，程式碼會建立下列的 SQL Update 陳述式：

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    在 SQL 中`Update`陳述式，指定每個更新，並將它設定為值的資料行。 在此程式碼，這些值使用指定的參數預留位置`@0`， `@1`， `@2`，依此類推。 （如前文所述，為了安全性，您應該一律傳遞值的 SQL 陳述式使用的參數。）

    當您呼叫`db.Execute`方法，傳遞包含對應至 SQL 陳述式的參數順序值的變數：

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    之後`Update`在執行陳述式中，您才能將使用者重新導向回到編輯頁面呼叫下列方法：

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    結果是使用者會看到資料庫中資料的更新的清單，並可以編輯另一個產品。
6. 儲存網頁。
7. 執行*EditProducts.cshtml*頁面 （而不更新頁面），然後按一下**編輯**選取要編輯的產品。 *UpdateProducts.cshtml*頁面隨即顯示，顯示您所選取的記錄。

    ![[影像]](5-working-with-data/_static/image8.jpg)
8. 進行變更，然後按一下**更新**。 更新的資料會再次顯示產品清單。

## <a name="deleting-data-in-a-database"></a>刪除資料庫中的資料

本節說明如何讓使用者可以刪除從產品*產品*資料庫資料表。 此範例包含兩個頁面。 在第一個頁面中，使用者會選取要刪除的記錄。 要刪除的記錄即會顯示在第二個頁面，讓他們確認想要刪除記錄中。

> [!NOTE] 
> 
> **重要**在生產網站，您通常會限制已獲允許的人員進行變更的資料。 如需如何設定成員資格以及站台上執行工作的使用者授與方式的相關資訊，請參閱[加入安全性和 ASP.NET Web Pages 站台中的成員資格](https://go.microsoft.com/fwlink/?LinkId=202904)。


1. 在網站上建立名為新 CSHTML 檔案*ListProductsForDelete.cshtml*。
2. 以下列內容取代現有的標記：

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    此頁面是類似於*EditProducts.cshtml*從先前的頁面。 不過，而不是顯示**編輯**連結每項產品，它會顯示**刪除**連結。 **刪除**標記中使用下列的內嵌程式碼來建立連結：

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    這會建立使用者按下連結時，看起來像這樣的 URL:

    `http://<server>/DeleteProduct/4`

    URL 會呼叫名為的頁面*DeleteProduct.cshtml* （其中您要建立 下一步） 並將其傳遞要刪除的產品識別碼 （此處，4）。
3. 儲存檔案，但讓它保持開啟。
4. 建立名為另一個 CHTML 檔案*DeleteProduct.cshtml*。 以下列內容取代現有的內容：

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    此頁面由呼叫*ListProductsForDelete.cshtml*並可讓使用者確認他們想要刪除的產品。 若要列出要刪除之產品，您取得的產品識別碼，刪除從 URL 使用下列程式碼：

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    頁面會接著詢問使用者按一下按鈕以實際刪除記錄。 這是一項重要的安全性措施： 當您執行機密的作業，在您的網站，例如更新或刪除資料，這些作業應該永遠是使用 POST 作業，不是 GET 作業。 如果您的網站已設定，以便可以使用 「 取得 」 作業來執行刪除作業，任何人都可以傳遞的 URL，例如`http://<server>/DeleteProduct/4`並從資料庫刪除他們想要的任何項目。 藉由新增確認頁面撰寫程式碼，以便可以使用 POST 執行刪除，您加入您的站台的安全性考量。

    使用下列程式碼，首先會確認這是 post 作業識別碼不是空被執行實際的刪除作業：

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    執行 SQL 陳述式會刪除指定的記錄，然後將使用者重新導向回到 [清單] 頁面的程式碼。
5. 執行*ListProductsForDelete.cshtml*瀏覽器中。

    ![[影像]](5-working-with-data/_static/image9.jpg)
6. 按一下**刪除**連結的其中一個產品。 *DeleteProduct.cshtml*頁面會顯示確認您想要刪除該記錄。
7. 按一下**刪除** 按鈕。 刪除產品記錄，並使用更新的產品清單重新整理頁面。

> [!TIP] 
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>連接至資料庫
> 
> 您可以連接到兩種方式中的資料庫。 第一個方法是使用`Database.Open`方法並指定資料庫檔案的名稱 (較少*.sdf*副檔名):
> 
> `var db = Database.Open("SmallBakery");`
> 
> `Open`方法會假設。*sdf*檔案位於網站*應用程式\_資料*資料夾。 此資料夾所專用的保存資料。 例如，它有適當的權限允許讀取和寫入資料，網站，並基於安全性考量，WebMatrix 不允許存取的檔案從這個資料夾。
> 
> 第二種方式是使用連接字串。 連接字串包含有關如何連接到資料庫的資訊。 這可以包含檔案路徑，或它可以包含在本機或遠端伺服器，以及使用者名稱和密碼以連接到該伺服器上的 SQL Server 資料庫的名稱。 （如果您保留資料集中管理的 SQL Server 版本中，例如裝載提供者的站台，您一律使用連接字串來指定資料庫連接資訊）。
> 
> 在 WebMatrix 中，連接字串通常會儲存在名為 XML 檔案*Web.config*。正如其名，您可以使用*Web.config*根目錄中的網站，以儲存站台的設定資訊，包括您的網站可能需要的任何連接字串的檔案。 連接字串中的範例*Web.config*檔案可能看起來如下所示：
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> 在範例中，連接字串會指向某個位置的伺服器執行的 SQL Server 執行個體中的資料庫 (而不是本機*.sdf*檔案)。 您必須替換為適當的名稱`myServer`和`myDatabase`，並且指定 SQL Server 登入值`username`和`password`。 （使用者名稱和密碼值不一定是相同為您的 Windows 認證或您的主機服務提供者已提供您的登入其伺服器的值。 請洽詢系統管理員，您需要的確切值。）
> 
> `Database.Open`方法是，有彈性，因為它可讓您將資料庫的名稱傳遞*.sdf*檔案或儲存中的連接字串名稱*Web.config*檔案。 下列範例會示範如何連接到資料庫使用的連接字串上一個範例所示：
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> 如前所述，`Database.Open`方法可讓您將資料庫名稱或連接字串，它會找出要使用。 這會非常有用的當您部署 （發行） 您的網站。 您可以使用*.sdf*檔案*應用程式\_資料*資料夾，當您開發並測試您的網站。 當您將您的站台移至實際伺服器時，您可以使用連接字串中的*Web.config*檔案具有相同名稱做為您*.sdf*檔案，但指向裝載提供者的資料庫 &#8212;所有不需要變更您的程式碼。
> 
> 最後，如果您想要直接使用連接字串，您可以呼叫`Database.OpenConnectionString`方法，然後傳遞其實際的連接字串而不只在其中一個的名稱*Web.config*檔案。 這可能會很有用，因為某些原因您沒有存取權連接字串的情況下 (或值，例如*.sdf*檔案名稱) 直到網頁執行時。 不過，大部分情況下，您可以使用`Database.Open`這篇文章中所述。


## <a name="additional-resources"></a>其他資源

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [連接到 SQL Server 或 WebMatrix 的 MySQL 資料庫](https://go.microsoft.com/fwlink/?LinkId=208661)
- [ASP.NET Web Pages 站台中中驗證使用者輸入](https://go.microsoft.com/fwlink/?LinkId=253002)
