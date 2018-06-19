---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: 將新欄位加入至電影模型和資料表 |Microsoft 文件
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本這裡會提供使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 容易遵循，以及示範...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d8a42e9acdce687ab6e9742071dd2949f244622f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872786"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>將新欄位加入至電影模型和資料表
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程的更新的版本時使用[這裡](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 容易遵循，及示範更多的功能。


這一節中，您將使用 Entity Framework Code First 移轉來移轉至模型類別的某些變更，因此變更套用至資料庫。

根據預設，當您使用 Entity Framework Code First 來自動建立的資料庫，您可以如同稍早在本教學課程，第一個程式碼將資料表加入至要協助追蹤資料庫的結構描述是否從產生的模型類別同步的資料庫。 如果不是同步，Entity Framework 會擲回錯誤。 這可讓您更輕鬆地在開發期間，您可能否則只 （藉由晦澀難懂的錯誤） 在執行階段追蹤問題。

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Code First 移轉設定的模型變更

如果您使用 Visual Studio 2012，按兩下 [ *Movies.mdf*從方案總管] 以開啟資料庫工具檔案。 Visual Studio Express for Web 會顯示資料庫總管 中，Visual Studio 2012 會顯示伺服器總管。 如果您使用 Visual Studio 2010，請使用 SQL Server 物件總管。

在 [資料庫] 工具 （資料庫總管、 伺服器總管或 SQL Server 物件總管），以滑鼠右鍵按一下`MovieDBContext`選取**刪除**卸除電影資料庫。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

瀏覽至 方案總管。 以滑鼠右鍵按一下*Movies.mdf*檔案，然後選取**刪除**移除電影資料庫。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

建置應用程式，確定沒有任何錯誤。

從**工具**功能表上，按一下 **程式庫套件管理員**然後**Package Manager Console**。

![加入組件攔截](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

在**Package Manager Console**視窗`PM>`提示字元輸入"Enable-migrations-ContextTypeName MvcMovie.Models.MovieDBContext"。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

**Enable-migrations** （如上所示） 的命令會建立*configuration.cs 中*檔案中的新*移轉*資料夾。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio 隨即開啟*configuration.cs 中*檔案。 取代`Seed`方法中的*configuration.cs 中*以下列程式碼檔案：

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

以滑鼠右鍵按一下底下的紅色曲線`Movie`選取**解決**然後**使用** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

此舉會將下列 using 陳述式：

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First 移轉呼叫`Seed`方法之後每個移轉 (也就呼叫**更新資料庫**Package Manager Console 中)，這個方法會更新已經插入，或如果將它們插入的資料列和它們不存在。


**按 CTRL-SHIFT-B 以建置專案。**(下列步驟將會失敗，如果您在此時不用建置。)

下一個步驟是建立`DbMigration`初始移轉的類別。 移轉到建立的新資料庫，就是為什麼您刪除*movie.mdf*上一個步驟中的檔案。

在**Package Manager Console**視窗中，輸入 「 新增移轉初始"命令建立初始的移轉。 「 初始 」 的名稱是任意的用來建立的移轉檔案名稱。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Code First 移轉建立的另一個類別檔案*移轉*資料夾 (具有名稱 *{DateStamp}\_Initial.cs* )，而且這個類別包含程式碼會建立資料庫結構描述。 移轉 filename 預先固定時間戳記為協助進行排序。 檢查 *{DateStamp}\_Initial.cs*檔案，它包含的指示來建立電影 db 電影資料表。 當您更新的資料庫中的指示，在此之下 *{DateStamp}\_Initial.cs*檔案將會執行並建立資料庫結構描述。 然後在**種子**方法會執行填入資料庫的測試資料。

在**Package Manager Console**，輸入命令 [更新的資料庫] 來建立資料庫和執行**種子**方法。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

如果您收到錯誤，指出資料表已經存在，而且無法建立，它可能是因為刪除資料庫之後，您在執行之前，執行該應用程式`update-database`。 在此情況下，刪除*Movies.mdf*檔案一次，然後重試`update-database`命令。 如果您仍然收到錯誤，刪除 migrations 資料夾內容，則開頭的指示，在此頁面最上方 (這是刪除*Movies.mdf*檔案，然後繼續進行 Enable-migrations)。

執行應用程式，並瀏覽至 */Movies* URL。 種子資料會顯示。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>將 Rating 屬性新增至電影模型

開始透過新增`Rating`屬性至現有`Movie`類別。 開啟*Models\Movie.cs*檔案，然後加入`Rating`屬性如下所示：

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

完整`Movie`類別現在看起來類似下列程式碼：

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

建置應用程式使用**建置** &gt;**建置影片**功能表命令或按 CTRL-SHIFT b。

既然您已更新`Model`類別，您也需要更新*\Views\Movies\Index.cshtml*和*\Views\Movies\Create.cshtml*檢視範本以顯示新`Rating`瀏覽器檢視中的屬性。

開啟<em>\Views\Movies\Index.cshtml</em>檔案，然後加入`<th>Rating</th>`欄名後方<strong>價格</strong>資料行。 然後加入`<td>`即將要呈現之範本的結束欄`@item.Rating`值。 以下是哪些更新<em>Index.cshtml</em>檢視範本外觀就像：

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

接下來，開啟*\Views\Movies\Create.cshtml*檔案，然後加入下列標記表單的結尾附近。 這會呈現文字方塊中，以便建立新的電影時，您可以指定評等。

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

您現在已更新以支援新的應用程式程式碼`Rating`屬性。

現在執行應用程式中，瀏覽至 */Movies* URL。 當您這樣做時，不過，您會看到下列錯誤：

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

您在看到這個錯誤，因為已更新`Movie`應用程式中的模型類別現在是不同的結構描述`Movie`現有資料庫的資料表。 (資料庫資料表中沒有任何 `Rating` 資料行)。


有幾個方法可以解決這個錯誤：

1. 讓 Entity Framework 自動卸除資料庫，並重新依據新的模型類別結構描述來建立資料庫。 這種方法在執行測試資料庫上的作用中開發時非常方便它可讓您快速發展的模型和資料庫結構描述在一起。 這個選項的缺點是，您在資料庫中現有的資料遺失，因此您*不*想要在實際執行資料庫上使用此方法 ！ 使用初始設定式來自動植入的測試資料的資料庫，通常是有效的方式開發應用程式。 如需有關 Entity Framework 資料庫初始設定式的詳細資訊，請參閱 Tom Dykstra [ASP.NET MVC/Entity Framework 教學課程](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
2. 您可明確修改現有資料庫的結構描述，使其符合模型類別。 這種方法的優點是可以保留您的資料。 您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。
3. 使用 Code First 移轉來更新資料庫結構描述。


在本教學課程中，我們將使用 Code First 移轉。

更新 Seed 方法，使其提供新的資料行的值。 開啟 Migrations\Configuration.cs 檔案，並將評等 欄位加入至每個影片物件。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

建置方案，然後再開啟**Package Manager Console**視窗並輸入下列命令：

`add-migration AddRatingMig`

`add-migration`命令告訴移轉架構，以檢查目前的電影模型，與目前的電影 DB 結構描述，並建立必要的程式碼將資料庫移轉至新的模型。 AddRatingMig 是任意的用來移轉檔案名稱。 最好先使用有意義的名稱，如移轉步驟。

此命令完成時，Visual Studio 會開啟定義新的類別檔案`DbMIgration`衍生的類別，然後在`Up`方法，您可以看到建立新的資料行的程式碼。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

建置方案，並輸入中的 [更新資料庫] 命令**Package Manager Console**視窗。

下圖顯示在輸出**Package Manager Console**視窗 （AddRatingMig 前面加上日期戳記將會不同。）

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

重新執行應用程式，並瀏覽至 /Movies URL。 您可以看到新的 [分級] 欄位。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

按一下**新建**連結加入新的電影。 請注意，您可以加入評等。

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

按一下 [建立] 。 新的電影，包括評等，現在會顯示列出的電影中：

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

您也應該加入`Rating`欄位編輯，詳細資料和 SearchIndex 檢視範本。

您可以輸入中的 [更新資料庫] 命令**Package Manager Console**視窗一次並沒有變更做，因為結構描述符合模型。

本節中您已看到如何修改模型物件和資料庫中與變更保持同步。 您也學到如何填入新建立的資料庫中的範例資料，因此您可以嘗試的案例。 接下來，讓我們看看如何將更豐富的驗證邏輯加入至模型類別，並啟用某些商務規則，以強制執行。

> [!div class="step-by-step"]
> [上一頁](examining-the-edit-methods-and-edit-view.md)
> [下一頁](adding-validation-to-the-model.md)
