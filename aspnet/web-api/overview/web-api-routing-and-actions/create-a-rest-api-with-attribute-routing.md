---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: 使用 ASP.NET Web API 2 中的屬性路由建立 REST API |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 892d353ded17136825fda05b671eab4195b44b07
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819055"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>ASP.NET Web API 2 中的屬性路由建立 REST API
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

Web API 2 支援新類型的路由，稱為*屬性路由*。 屬性路由的一般概觀，請參閱 < [Web API 2 中的屬性路由](attribute-routing-in-web-api-2.md)。 在本教學課程中，您將使用屬性路由建立 REST API 的書籍集合。 API 將會支援下列動作：

| 動作 | 範例 URI |
| --- | --- |
| 取得所有書籍的清單。 | / api/書籍 |
| 取得活頁簿的識別碼。 | /api/books/1 |
| 取得活頁簿的詳細資訊。 | /api/books/1/details |
| 取得內容類型的書籍清單。 | /api/books/fantasy |
| 取得發行日期的書籍清單。 | /api/books/date/2013-02-16 /api/books/date/2013/02/16 （替代形式） |
| 取得特定作者的書籍清單。 | /api/authors/1/books |

所有方法都是唯讀狀態 （HTTP GET 要求）。

資料層，我們將使用 Entity Framework。 書籍記錄將會有下列欄位：

- 識別碼
- 標題
- 內容類型
- 發行日期
- 價格
- 描述
- AuthorID （Authors 資料表的外部索引鍵）

不過，對於大部分的要求，這個 API 會傳回此資料 （標題、 作者和內容類型） 的子集。 若要取得完整的記錄，用戶端要求`/api/books/{id}/details`。

## <a name="prerequisites"></a>必要條件

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、 Professional 或 Enterprise edition。

## <a name="create-the-visual-studio-project"></a>建立 Visual Studio 專案

執行 Visual Studio 啟動。 從**檔案**功能表上，選取**新增**，然後選取**專案**。

在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。 底下**Visual C#**，選取**Web**。 在專案範本清單中，選取**ASP.NET MVC 4 Web 應用程式**。 將專案命名為&quot;BooksAPI&quot;。

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

在 **新的 ASP.NET 專案**對話方塊中，選取**空白**範本。 在"新增資料夾和核心參考 」，選取**Web API**核取方塊。 按一下 **建立專案**。

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

這會建立基本架構的專案已針對 Web API 的功能。

### <a name="domain-models"></a>網域模型

接下來，新增的網域模型的類別。 在 [方案總管] 中，以滑鼠右鍵按一下 [模型] 資料夾。 選取 **新增**，然後選取**類別**。 將類別命名為 `Author` 。

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

您可以將 Author.cs 中的程式碼取代為下列：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

現在加入另一個類別，名為`Book`。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>新增 Web API 控制器

在此步驟中，我們將新增為資料層會使用 Entity Framework 的 Web API 控制器。

按 CTRL+SHIFT+B 以建置專案。 Entity Framework 會使用反射來探索模型中，屬性，因此需要編譯的組件，來建立資料庫結構描述。

在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。 選取 **新增**，然後選取**控制站**。

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

在**新增 Scaffold**對話方塊中，選取 「 Web API 2 控制器會執行讀取/寫入動作，使用 Entity Framework。 」

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

在 [**新增控制器**] 對話方塊中，如**控制器名稱**，輸入&quot;BooksController&quot;。 選取 &quot;使用非同步控制器動作&quot;核取方塊。 針對**模型類別**，選取&quot;活頁簿&quot;。 (如果您沒有看到`Book`類別列在下拉式清單，請確定您建置專案。)然後按一下"+"按鈕。

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

按一下 **新增**中**新的資料內容**對話方塊。

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

按一下 **新增**中**新增控制器**對話方塊。 Scaffolding 會加入名為類別`BooksController`定義 API 控制器。 它也會新增一個名為類別`BooksAPIContext`在 Models 資料夾中，其定義用於 Entity Framework 資料內容。

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>植入資料庫

從 [工具] 功能表中，選取**程式庫套件管理員**，然後選取**Package Manager Console**。

在 [套件管理員主控台] 視窗中，輸入下列命令：

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

此命令會建立 Migrations 資料夾，並將新的程式碼檔案，名為 Configuration.cs。 開啟此檔案並新增下列程式碼`Configuration.Seed`方法。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

在 [套件管理員主控台] 視窗中，輸入下列命令。

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

這些命令會建立本機資料庫，並叫用種子方法，來擴展資料庫。

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>新增的 DTO 類別

如果您執行應用程式現在，並將 GET 要求傳送至 /api/books/1，回應看起來如下所示。 （我新增縮排以提高可讀性）。

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

相反地，我想這項要求傳回的欄位子集。 此外，我希望它傳回作者的名稱，而不是作者的識別碼。 若要這麼做，我們要修改控制器方法，以傳回*資料傳輸物件*(DTO) 而不是 EF 模型。 DTO 是只設計來執行資料的物件。

在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增** | **新資料夾**。 將資料夾命名&quot;Dto&quot;。 新增類別，名為`BookDto`Dto 資料夾中，使用下列定義：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

新增另一個類別，名為`BookDetailDto`。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

接下來，更新`BooksController`類別，以傳回`BookDto`執行個體。 我們將使用[Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx)方法，以專案`Book`執行個體`BookDto`執行個體。 以下是控制器類別的更新程式碼。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> 我刪掉`PutBook`， `PostBook`，和`DeleteBook`方法，因為本教學課程不需要它們。


現在如果您執行應用程式，並要求 /api/books/1，回應主體看起來應該像這樣：

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>新增路由屬性

接下來，我們會將轉換控制器以使用屬性路由。 首先，新增**RoutePrefix**屬性控制站。 此屬性會定義此控制站上的所有方法的初始 URI 區段。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

然後新增 **[路由]** 屬性至控制器動作，如下所示：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

每個控制器方法的路由範本是前置詞加上字串中指定**路由**屬性。 針對`GetBook`方法，路由範本中包含的參數化的字串&quot;{id: int}&quot;，會比對如果 URI 區段包含整數值。

| 方法 | 路由範本 | 範例 URI |
| --- | --- | --- |
| `GetBooks` | "api/books" | `http://localhost/api/books` |
| `GetBook` | 「 書籍 api / / {id: int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>取得活頁簿詳細資料

若要取得活頁簿詳細資料，用戶端會傳送 GET 要求`/api/books/{id}/details`，其中 *{id}* 本書的識別碼。

將下列方法加入 `BooksController` 類別中。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

如果您要求`/api/books/1/details`，回應看起來像這樣：

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>依內容類型的書籍

若要取得特定的內容類型的書籍清單，用戶端會傳送 GET 要求`/api/books/genre`，其中*genre*是內容類型的名稱。 (例如，`/api/books/fantasy`)。

將下列方法加入`BooksController`。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

這裡我們所定義的路由，其中包含 URI 範本中的 {genre} 參數。 請注意，Web API 可以區分這些兩個 Uri，並將它們路由傳送至不同的方法：

`/api/books/1`

`/api/books/fantasy`

這是因為`GetBook`方法包含 「 識別碼 」 區段必須是整數值的條件約束：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

如果您要求 /api/books/fantasy，回應看起來像這樣：

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>作者的書籍

若要取得某位特定作者的書籍清單，用戶端會傳送 GET 要求`/api/authors/id/books`，其中*識別碼*作者的識別碼。

將下列方法加入`BooksController`。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

有趣的此範例中，是因為&quot;書籍&quot;是處理的子資源&quot;作者&quot;。 此模式是 RESTful Api 中相當常見。

波狀符號 （~） 路由範本中的會覆寫中的路由前置詞**RoutePrefix**屬性。

## <a name="get-books-by-publication-date"></a>取得活頁簿發行日期

若要取得依發行日期的書籍清單，用戶端會傳送 GET 要求`/api/books/date/yyyy-mm-dd`，其中*yyyy-mm-dd 的-* 的日期。

若要這樣做的其中一個方法如下：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

`{pubdate:datetime}`參數會限制為符合**DateTime**值。 可以運作，但超出我們想要的實際更寬鬆。 比方說，這些 Uri 也會比對路由：

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

沒有任何問題讓這些 Uri。 不過，您也可以將規則運算式的條件約束加入至路由範本，以限制路由至特定的格式：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

現在僅日期格式&quot;yyyy-mm-dd 的-&quot;會比對。 請注意，我們不使用 regex 驗證，我們會取得實際的日期。 Web API 會嘗試將轉換成 URI 區段時，處理**DateTime**執行個體。 無效的日期 2012 例如 '-47-99' 將無法進行轉換，而且用戶端會收到 404 錯誤。

您也可以支援斜線分隔符號 (`/api/books/date/yyyy/mm/dd`) 是新增另一個 **[Route]** 與其他規則運算式的屬性。

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

沒有細微但很重要明瞭。 第二個路由範本具有萬用字元 (\*) {pubdate} 參數的開頭：

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

這會告訴路由引擎該 {pubdate} 應該符合 URI 的其餘部分。 根據預設，範本參數會比對單一的 URI 區段。 在此情況下，我們要 {pubdate} 跨越數個 URI 區段：

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>控制器程式碼

以下是 BooksController 類別的完整程式碼。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>總結

屬性路由可讓您更多控制權和更大的彈性設計時為您的 API 的 Uri。
