---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: 從控制器存取模型的資料 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 8d359de17bc35d0e14047ccd0589f080e0f949fc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396108"
---
<a name="accessing-your-models-data-from-a-controller"></a>從控制器存取模型資料
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

在本節中，您將建立新`MoviesController`類別，並撰寫程式碼會擷取電影資料，並顯示在瀏覽器中使用檢視範本。

**建置應用程式**再繼續進行下一個步驟。 如果您沒有建置應用程式，您會收到錯誤新增控制器。

在 [方案總管] 中，以滑鼠右鍵按一下*控制器*資料夾，然後按一下**新增**，然後**控制器**。

![](accessing-your-models-data-from-a-controller/_static/image1.png)

在 **新增 Scaffold**  對話方塊中，按一下**的 MVC 5 控制器與檢視，使用 Entity Framework**，然後按一下 **新增**。

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- 選取  **Movie (MvcMovie.Models)** 模型類別。
- 選取  **MovieDBContext (MvcMovie.Models)** 資料內容類別。
- 控制器名稱的輸入**MoviesController**。

  下圖顯示 [已完成] 對話方塊。  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

按一下 [加入] 。 （如果您收到錯誤，您可能未建置的應用程式，再啟動 新增控制器）。Visual Studio 會建立下列檔案和資料夾：

- *MoviesController.cs*檔案中*控制器*資料夾。
- A *Views\Movies*資料夾。
- *Create.cshtml，Delete.cshtml，Details.cshtml，Edit.cshtml*，並*Index.cshtml*在新*Views\Movies*資料夾。

自動建立的 visual Studio [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) （建立、 讀取、 更新和刪除） 動作方法和您的檢視 （自動建立 CRUD 動作方法和檢視稱為 scaffolding）。 您現在有功能完整的 web 應用程式，可讓您建立、 列出、 編輯和刪除電影的項目。

執行應用程式，然後按一下**MVC Movie**連結 (或瀏覽至`Movies`控制站，藉由附加 */Movies*至您的瀏覽器的網址列中的 URL)。 因為應用程式依賴預設路由 (定義於*應用程式\_Start\RouteConfig.cs*檔案)，瀏覽器要求`http://localhost:xxxxx/Movies`會路由傳送至預設`Index`動作方法`Movies`控制器。 換句話說，瀏覽器要求`http://localhost:xxxxx/Movies`實際上是相同的瀏覽器要求`http://localhost:xxxxx/Movies/Index`。 因為您尚未加入任何尚未，結果會是空白的電影清單。

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>建立電影

選取 **Create New** 連結。 輸入電影相關的一些詳細資料，然後按一下**建立** 按鈕。


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> 您可能無法在 [價格] 欄位中輸入小數點或逗號。 若要 jQuery 驗證支援非英文的地區設定，請使用逗號 (&quot;，&quot;) 的小數點和非英文日期格式，您必須加入*globalize.js*和您的特定*cultures/globalize.cultures.js*檔案 (從[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) 和使用的 JavaScript `Globalize.parseFloat`。 我將示範如何在下一個教學課程中這麼做。 現在，只要輸入如 10 之類的整數。


按一下 **建立**按鈕會使表單張貼至伺服器時，電影資訊儲存在資料庫中的位置。 若要將您再重新導向 */Movies* URL，您可以在其中看到新建立的電影清單中。

![](accessing-your-models-data-from-a-controller/_static/image6.png)

建立幾個電影項目。 嘗試 **Edit**、**Details** 和 **Delete** 連結，這些連結都可以正常運作。

## <a name="examining-the-generated-code"></a>檢查產生的程式碼

開啟*Controllers\MoviesController.cs*檔案，並檢查產生`Index`方法。 電影控制器的一部分`Index`方法如下所示。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

若要要求`Movies`控制器傳回中的所有項目`Movies`資料表，並接著會將結果傳遞`Index`檢視。 下面這行從`MoviesController`類別執行個體化的電影資料庫內容，如先前所述。 您可用於查詢、 編輯和刪除影片的電影資料庫內容。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>強型別模型和@model關鍵字

稍早在本教學課程中，您看到如何控制器傳遞資料或物件給檢視範本中使用`ViewBag`物件。 `ViewBag`是動態的物件，提供便利的晚期繫結方式，將資訊傳遞至檢視。

MVC 也讓您能夠傳遞*強*型別檢視範本的物件。 這個強類型的方法可讓更佳編譯時間檢查您的程式碼以及更豐富[IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) Visual Studio 編輯器中。 Scaffolding 機制，在 Visual Studio 中的使用這種方法 (也就傳遞*強烈*型別的模型) 與`MoviesController`類別並檢視範本，建立方法和檢視時。

在  *Controllers\MoviesController.cs*檢查所產生的檔案`Details`方法。 `Details`方法如下所示。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

`id`參數通常會傳遞為路由資料，例如`http://localhost:1234/movies/details/1`會將控制器設到電影控制器時，所要的動作`details`而`id`為 1。 您也可以傳入查詢字串的 id，如下所示：

`http://localhost:1234/movies/details?id=1`

如果`Movie`找到，則執行個體`Movie`的模型傳遞至`Details`檢視：

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

檢查的內容*Views\Movies\Details.cshtml*檔案：

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

藉由包括`@model`陳述式在檢視範本檔案的頂端，您可以指定檢視預期要有的物件類型。 當您建立電影控制器時，Visual Studio 會在 *Details.cshtml* 檔案的最上方自動包含下列 `@model` 陳述式：

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

這個 `@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影。 例如，在*Details.cshtml*範本，程式碼會傳遞至每個電影欄位`DisplayNameFor`並[DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML 協助程式的強型別`Model`物件。 `Create`和`Edit`方法和檢視範本也會傳遞影片模型物件。

檢查*Index.cshtml*檢視範本並`Index`中的方法*MoviesController.cs*檔案。 請注意程式碼的建立方式[ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)物件時它會呼叫`View`中的協助程式方法`Index`動作方法。 程式碼接著會傳遞這`Movies`從清單`Index`至檢視的動作方法：

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

當您建立電影控制器時，Visual Studio 會自動包含下列`@model`陳述式，在頂端*Index.cshtml*檔案：

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

這`@model`指示詞可讓您存取控制器傳遞至檢視使用的電影清單`Model`強類型的物件。 例如，在*Index.cshtml*範本，此程式碼迴圈電影透過操作`foreach`陳述式，透過強型別`Model`物件：

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

因為`Model`物件已強類型 (為`IEnumerable<Movie>`物件)，每個`item`迴圈中的物件型別為`Movie`。 撇開其他優點，這表示您取得的程式碼的編譯時間檢查，並完整的程式碼編輯器中的 IntelliSense 支援：

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>使用 SQL Server LocalDB

Entity Framework Code First 偵測到所提供的資料庫連接字串指向`Movies`不存在，所以 Code First 資料庫自動建立的資料庫。 您可以確認它在查看已建立*應用程式\_資料*資料夾。 如果您沒有看到*Movies.mdf*檔案中，按一下**顯示所有檔案**按鈕**方案總管] 中**工具列上，按一下 [**重新整理**按鈕，然後再展開*應用程式\_資料*資料夾。

![](accessing-your-models-data-from-a-controller/_static/image9.png)

按兩下*Movies.mdf*來開啟**伺服器總管**，然後展開**資料表**資料夾，以查看電影資料表。 請注意索引鍵的圖示旁邊識別碼。 根據預設，EF 將名為 ID 的主索引鍵的屬性。 如需有關 EF 和 MVC 的詳細資訊，請參閱 Tom Dykstra 絕佳的教學課程[MVC 和 EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

以滑鼠右鍵按一下`Movies`資料表，然後選取**顯示資料表資料**以查看您所建立的資料。

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

以滑鼠右鍵按一下`Movies`資料表，然後選取**開啟資料表定義**查看資料表結構，Entity Framework Code First 會為您建立。

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

請注意如何的結構描述`Movies`資料表會對應至`Movie`您稍早建立的類別。 Entity Framework Code First 自動建立此結構描述根據您`Movie`類別。

當您完成時，關閉連線，以滑鼠右鍵按一下*MovieDBContext* ，然後選取**關閉連線**。 （如果您未關閉連接，您可能會發生錯誤的下次您執行專案）。

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

您現在擁有一個資料庫和多個頁面，可用來顯示、編輯、更新和刪除資料。 在下一個教學課程中，我們將檢查的 scaffold 程式碼的其餘部分，並新增`SearchIndex`方法和`SearchIndex`可讓您搜尋此資料庫中的電影的檢視。 如需有關使用使用 MVC 的 Entity Framework 的詳細資訊，請參閱 <<c0> [ 建立 ASP.NET MVC 應用程式的 Entity Framework 資料模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。

> [!div class="step-by-step"]
> [上一頁](creating-a-connection-string.md)
> [下一頁](examining-the-edit-methods-and-edit-view.md)
