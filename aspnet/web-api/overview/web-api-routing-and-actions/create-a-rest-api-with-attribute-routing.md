---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: REST API 建立與 ASP.NET Web API 2 中的屬性路由 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 1f1e90544c9dd8439a522f2196d81d020ea2f4f2
ms.sourcegitcommit: 7f92990bad6a6cb901265d621dcbc136794f5f3f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>REST API 建立以 ASP.NET Web API 2 中的路由的屬性
====================
由[Mike Wasson](https://github.com/MikeWasson)

Web API 2 支援新類型的路由，稱為*屬性路由*。 路由屬性的一般概觀，請參閱[屬性路由中 Web API 2](attribute-routing-in-web-api-2.md)。 在此教學課程中，您將使用屬性路由建立 REST API 針對書籍的集合。 此 API 將會支援下列動作：

| 動作 | 範例 URI |
| --- | --- |
| 取得所有書籍的清單。 | / api/書籍 |
| 取得活頁簿的識別碼。 | /api/books/1 |
| 取得一本書詳細資料。 | /api/books/1/details |
| 取得內容類型的書籍清單。 | /api/books/fantasy |
| 取得發行日期的書籍清單。 | /api/books/date/2013-02-16 /api/books/date/2013/02/16 （替代形式） |
| 取得由特定作者的書籍清單。 | /api/authors/1/books |

所有方法都是唯讀狀態 （HTTP GET 要求）。

資料層，我們將使用 Entity Framework。 活頁簿的記錄有下列欄位：

- 識別碼
- 標題
- 內容類型
- 發行日期
- 價格
- 描述
- AuthorID （Authors 資料表外部索引鍵）

不過，大部分的要求，API 會傳回這項資料 （標題、 作者和內容類型） 的子集。 若要取得完整的記錄，用戶端要求`/api/books/{id}/details`。

## <a name="prerequisites"></a>必要條件

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、 Professional 或 Enterprise edition。

## <a name="create-the-visual-studio-project"></a>建立 Visual Studio 專案

執行 Visual Studio 啟動。 從**檔案**功能表上，選取**新增**，然後選取 **專案**。

在**範本**窗格中，選取**已安裝的範本**展開**Visual C#**節點。 在下**Visual C#**，選取**Web**。 在專案範本清單中選取**ASP.NET MVC 4 Web 應用程式**。 將專案命名&quot;BooksAPI&quot;。

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

在**新增 ASP.NET 專案**對話方塊中，選取**空**範本。 在 < 加入資料夾和核心參考 > 下選取**Web API**核取方塊。 按一下**建立專案**。

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

這會建立基本架構的專案設定為 Web 應用程式開發介面的功能。

### <a name="domain-models"></a>網域模型

接下來，新增網域模型類別。 在 [方案總管] 中，以滑鼠右鍵按一下 [模型] 資料夾。 選取**新增**，然後選取**類別**。 將類別命名為 `Author` 。

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

以下列內容取代 Author.cs 中的程式碼：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

現在加入另一個類別，名為`Book`。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>加入 Web API 控制器

在此步驟中，我們會新增為資料層使用 Entity Framework 的 Web API 控制器。

按 CTRL+SHIFT+B 以建置專案。 Entity Framework 會使用反映來探索模型的屬性，因此需要建立資料庫結構描述編譯的組件。

在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。 選取**新增**，然後選取**控制器**。

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

在**新增 Scaffold**對話方塊中，選取 「 Web API 2 控制器讀取/寫入動作會使用 Entity Framework。 」

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

在**加入控制器** 對話方塊中，針對**控制器名稱**，輸入&quot;BooksController&quot;。 選取&quot;使用非同步控制器動作&quot;核取方塊。 如**模型類別**，選取&quot;書籍&quot;。 (如果您沒有看到`Book`類別列在下拉式清單，請確定您建置專案。)然後按一下"+"按鈕。

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

按一下**新增**中**新的資料內容**對話方塊。

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

按一下**新增**中**加入控制器**對話方塊。 Scaffolding 加入類別，名為`BooksController`定義 API 控制器。 它也會將類別加入名為`BooksAPIContext`模型 資料夾中，在其定義 Entity framework 資料內容。

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>植入資料庫

從 [工具] 功能表中，選取**程式庫套件管理員**，然後選取**Package Manager Console**。

在 [封裝管理員主控台] 視窗中，輸入下列命令：

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

此命令會建立 Migrations 資料夾，並將名為 configuration.cs 中新的程式碼檔案。 開啟此檔案並加入下列程式碼加入`Configuration.Seed`方法。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

在 [封裝管理員主控台] 視窗中，輸入下列命令。

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

這些命令會建立本機資料庫，並叫用種子方法來擴展資料庫。

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>新增 DTO 類別

如果您執行應用程式現在，將 GET 要求傳送至 /api/books/1 回應看起來如下所示。 （我新增可讀性的縮排）。

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

相反地，我想要傳回之欄位的子集，此要求。 此外，我想要傳回作者姓名，而不是作者識別碼。 若要達成此目的，我們會修改控制站方法，以傳回*資料傳輸物件*(DTO) 而不是 EF 模型。 DTO 是設計只會包含資料的物件。

在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增** | **新資料夾**。 將資料夾命名為&quot;DTOs&quot;。 將類別命名為`BookDto`DTOs 資料夾，具有下列定義：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

加入另一個類別，名為`BookDetailDto`。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

接下來，更新`BooksController`類別，以傳回`BookDto`執行個體。 我們將使用[Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx)方法，以專案`Book`執行個體來`BookDto`執行個體。 以下是更新的程式碼，如控制器類別。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> 我已刪除`PutBook`， `PostBook`，和`DeleteBook`方法，因為本教學課程不需要它們。


現在，如果您執行應用程式，並要求 /api/books/1，回應主體看起來應該像這樣：

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>新增路由屬性

接下來，我們將會轉換使用屬性路由的控制器。 首先，新增**RoutePrefix**屬性控制站。 此屬性定義此控制站上的所有方法的初始 URI 區段。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

然後加入**[路由]**屬性，是將控制器的動作，如下所示：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

每個控制器方法的路由範本是前置詞加上字串中指定**路由**屬性。 如`GetBook`方法，路由範本包含參數化的字串&quot;{id: int}&quot;，會比對如果 URI 區段包含整數值。

| 方法 | 路由範本 | 範例 URI |
| --- | --- | --- |
| `GetBooks` | "應用程式開發介面/books" | `http://localhost/api/books` |
| `GetBook` | 「 書籍 api / / {id: int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>取得活頁簿的詳細資料

若要取得活頁簿的詳細資訊，用戶端會傳送要求的 GET 要求`/api/books/{id}/details`，其中*{id}*活頁簿的識別碼。

將下列方法加入 `BooksController` 類別中。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

如果您要求`/api/books/1/details`，回應看起來像這樣：

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>取得依內容類型的書籍

若要取得特定的內容類型的書籍清單，用戶端會傳送要求的 GET 要求`/api/books/genre`，其中*類型*內容類型的名稱。 (例如，`/api/books/fantasy`)。

將下列方法加入`BooksController`。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

這裡我們會定義路由，其中包含與 URI 範本中的 {類型} 參數。 請注意，Web API 可以區分這些兩個 Uri，並將它們路由傳送至不同的方法：

`/api/books/1`

`/api/books/fantasy`

這是因為`GetBook`方法包含 「 識別碼 」 區段必須是整數值的條件約束：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

如果您要求 /api/books/fantasy，回應看起來像這樣：

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>取得作者的書籍

若要取得某位特定作者的書籍清單，用戶端會傳送要求的 GET 要求`/api/authors/id/books`，其中*識別碼*作者的識別碼。

將下列方法加入`BooksController`。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

這個範例是有趣因為&quot;書籍&quot;已處理的子資源&quot;作者&quot;。 此模式相當 RESTful Api 中相當常見。

波狀符號 （~） 中的路由範本會覆寫中的路由前置詞**RoutePrefix**屬性。

## <a name="get-books-by-publication-date"></a>發行日期，以取得活頁簿

若要依發行日期取得書籍清單，用戶端會傳送要求的 GET 要求`/api/books/date/yyyy-mm-dd`，其中*yyyy-mm-dd*的日期。

以下是一種方法執行這項操作：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

`{pubdate:datetime}`參數限制為符合**DateTime**值。 這麼做，但我們想要比實際更寬鬆。 例如，這些 Uri 也會比對路由：

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

沒有任何問題讓這些 Uri。 不過，您也可以將規則運算式的條件約束加入至路由範本，以限制路由至特定的格式：

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

現在僅日期格式&quot;yyyy-mm-dd&quot;會比對。 請注意，我們不使用 regex 驗證，我們會取得實際的日期。 Web API 會嘗試將轉換成 URI 區段時，處理**DateTime**執行個體。 無效的日期等 ' 2012年-47-99' 將無法進行轉換，而且用戶端會收到 404 錯誤。

您也可以支援斜線分隔 (`/api/books/date/yyyy/mm/dd`) 藉由新增另一個**[路由]**與其他規則運算式的屬性。

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

沒有細微但重要這裡詳細。 第二個路由範本具有萬用字元 (\*) {pubdate} 參數的開頭：

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

這會告訴路由引擎該 {pubdate} 應該符合 URI 的其餘部分。 根據預設，範本參數必須符合在單一 URI 片段。 在此情況下，我們要 {pubdate} 橫跨數個 URI 區段：

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>控制器的程式碼

以下是 BooksController 類別的完整程式碼。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>總結

路由屬性可讓您更多控制權和更大的彈性設計時您的 API 的 Uri。
