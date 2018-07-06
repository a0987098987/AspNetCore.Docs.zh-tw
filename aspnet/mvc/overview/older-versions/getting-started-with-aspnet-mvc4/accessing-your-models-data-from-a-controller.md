---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: 從控制器存取模型的資料 |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本就可以使用這裡使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 更容易遵循，並示範...
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: cf1d27088c1e65d55a6820825eebe63f7fdcb515
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804933"
---
<a name="accessing-your-models-data-from-a-controller"></a>從控制器存取模型資料
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程中的更新的版本可[此處](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 更容易遵循，並示範更多的功能。


在本節中，您將建立新`MoviesController`類別，並撰寫程式碼會擷取電影資料，並顯示在瀏覽器中使用檢視範本。

**建置應用程式**再繼續進行下一個步驟。

以滑鼠右鍵按一下*控制器*資料夾，並建立新`MoviesController`控制站。 必須先建置您的應用程式，不會出現下列選項。 選取下列選項：

- 控制器名稱： **MoviesController**。 （這是預設值。 )
- 範本：**使用 Entity Framework 的 MVC 控制器具有讀取/寫入動作和檢視，**。
- 模型類別： **Movie (MvcMovie.Models)**。
- 資料內容類別： **MovieDBContext (MvcMovie.Models)**。
- 檢視： **Razor (CSHTML)**。 （預設值。）

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

按一下 [加入] 。 Visual Studio Express，會建立下列檔案和資料夾：

- *MoviesController.cs*專案中的檔案*控制器*資料夾。
- A*電影*在專案的資料夾*檢視*資料夾。
- *Create.cshtml，Delete.cshtml，Details.cshtml，Edit.cshtml*，並*Index.cshtml*在新*Views\Movies*資料夾。

ASP.NET MVC 4 自動建立 CRUD （建立、 讀取、 更新和刪除） 動作方法和您的檢視 （自動建立 CRUD 動作方法和檢視稱為 scaffolding）。 您現在有功能完整的 web 應用程式，可讓您建立、 列出、 編輯和刪除電影的項目。

執行應用程式，並瀏覽至`Movies`藉由附加的控制站 */Movies*至您的瀏覽器的網址列中的 URL。 因為應用程式依賴預設路由 (定義於*Global.asax*檔案)，瀏覽器要求`http://localhost:xxxxx/Movies`會路由傳送至預設`Index`動作方法的`Movies`控制站。 換句話說，瀏覽器要求`http://localhost:xxxxx/Movies`實際上是相同的瀏覽器要求`http://localhost:xxxxx/Movies/Index`。 因為您尚未加入任何尚未，結果會是空白的電影清單。

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>建立電影

選取 **Create New** 連結。 輸入電影相關的一些詳細資料，然後按一下**建立** 按鈕。

![](accessing-your-models-data-from-a-controller/_static/image3.png)

按一下 **建立**按鈕會使表單張貼至伺服器時，電影資訊儲存在資料庫中的位置。 若要將您再重新導向 */Movies* URL，您可以在其中看到新建立的電影清單中。

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

建立幾個電影項目。 嘗試 **Edit**、**Details** 和 **Delete** 連結，這些連結都可以正常運作。

## <a name="examining-the-generated-code"></a>檢查產生的程式碼

開啟*Controllers\MoviesController.cs*檔案，並檢查產生`Index`方法。 電影控制器的一部分`Index`方法如下所示。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

下面這行從`MoviesController`類別執行個體化的電影資料庫內容，如先前所述。 您可用於查詢、 編輯和刪除影片的電影資料庫內容。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

要求`Movies`控制器中的所有項目會傳回`Movies`電影資料庫資料表然後將結果傳送至`Index`檢視。

## <a name="strongly-typed-models-and-the-model-keyword"></a>強型別模型和@model關鍵字

稍早在本教學課程中，您看到如何控制器傳遞資料或物件給檢視範本中使用`ViewBag`物件。 `ViewBag`是動態的物件，提供便利的晚期繫結方式，將資訊傳遞至檢視。

ASP.NET MVC 也會提供能夠傳遞強型別資料或檢視範本的物件。 強型別方法可讓更佳編譯時間檢查您的程式碼和更豐富的 IntelliSense，Visual Studio 編輯器中。 在 Visual Studio 中的 scaffolding 機制使用這個方法和`MoviesController`類別並檢視範本，建立方法和檢視時。

在  *Controllers\MoviesController.cs*檢查所產生的檔案`Details`方法。 電影控制器的一部分`Details`方法如下所示。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

如果`Movie`找到，則執行個體`Movie`模型傳遞至詳細資料檢視。 檢查的內容*Views\Movies\Details.cshtml*檔案。

藉由包括`@model`陳述式在檢視範本檔案的頂端，您可以指定檢視預期要有的物件類型。 當您建立電影控制器時，Visual Studio 會在 *Details.cshtml* 檔案的最上方自動包含下列 `@model` 陳述式：

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

這個 `@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影。 例如，在*Details.cshtml*範本，程式碼會傳遞至每個電影欄位`DisplayNameFor`並[DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML 協助程式的強型別`Model`物件。 Create 及 Edit 方法和檢視範本也會傳遞影片模型物件。

檢查*Index.cshtml*檢視範本並`Index`中的方法*MoviesController.cs*檔案。 請注意程式碼的建立方式[ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)物件時它會呼叫`View`中的協助程式方法`Index`動作方法。 程式碼接著會傳遞這`Movies`從控制器到檢視的清單：

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

當您建立電影控制器時，Visual Studio Express 會自動包含下列`@model`陳述式，在頂端*Index.cshtml*檔案：

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

這`@model`指示詞可讓您存取控制器傳遞至檢視使用的電影清單`Model`強類型的物件。 例如，在*Index.cshtml*範本，此程式碼迴圈電影透過操作`foreach`陳述式，透過強型別`Model`物件：

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

因為`Model`物件已強類型 (為`IEnumerable<Movie>`物件)，每個`item`迴圈中的物件型別為`Movie`。 撇開其他優點，這表示您取得的程式碼的編譯時間檢查，並完整的程式碼編輯器中的 IntelliSense 支援：

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>使用 SQL Server LocalDB

Entity Framework Code First 偵測到所提供的資料庫連接字串指向`Movies`不存在，所以 Code First 資料庫自動建立的資料庫。 您可以確認它在查看已建立*應用程式\_資料*資料夾。 如果您沒有看到*Movies.mdf*檔案中，按一下**顯示所有檔案**按鈕**方案總管] 中**工具列上，按一下 [**重新整理**按鈕，然後再展開*應用程式\_資料*資料夾。

![](accessing-your-models-data-from-a-controller/_static/image6.png)

按兩下*Movies.mdf*來開啟**資料庫總管**，然後展開**資料表**資料夾，以查看電影資料表。

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> 如果未顯示 [資料庫總管] 中，從**工具**功能表上，選取**連接到資料庫**，然後取消**選擇資料來源**對話方塊。 這會強制開啟 [資料庫總管] 中。


> [!NOTE]
> 如果您使用的 VWD 或 Visual Studio 2010，並出現類似下列的下列任何一個錯誤：
> 
> - 資料庫 ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES。MDF' 無法開啟，因為它是版本 706。 這個伺服器支援有 655 和更早版本。 不支援降級路徑。
> - &quot;InvalidOperation 例外狀況未處理使用者程式碼&quot;提供的 SqlConnection 未指定初始目錄。
> 
> 您必須安裝[SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)並[LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)。 確認`MovieDBContext`前一頁上指定的連接字串。


以滑鼠右鍵按一下`Movies`資料表，然後選取**顯示資料表資料**以查看您所建立的資料。

![](accessing-your-models-data-from-a-controller/_static/image8.png)

以滑鼠右鍵按一下`Movies`資料表，然後選取**開啟資料表定義**查看資料表結構，Entity Framework Code First 會為您建立。

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

請注意如何的結構描述`Movies`資料表會對應至`Movie`您稍早建立的類別。 Entity Framework Code First 自動建立此結構描述根據您`Movie`類別。

當您完成時，關閉連線，以滑鼠右鍵按一下*MovieDBContext* ，然後選取**關閉連線**。 （如果您未關閉連接，您可能會發生錯誤的下次您執行專案）。

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

您現在有資料庫和簡單的清單頁面，以顯示來自它的內容。 在下一個教學課程中，我們將檢查的 scaffold 程式碼的其餘部分，並新增`SearchIndex`方法和`SearchIndex`可讓您搜尋此資料庫中的電影的檢視。

> [!div class="step-by-step"]
> [上一頁](adding-a-model.md)
> [下一頁](examining-the-edit-methods-and-edit-view.md)
