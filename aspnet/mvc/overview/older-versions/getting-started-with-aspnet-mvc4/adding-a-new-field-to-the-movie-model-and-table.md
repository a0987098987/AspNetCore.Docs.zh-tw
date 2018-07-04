---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: 將新欄位新增至電影模型和資料表 |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本就可以使用這裡使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 更容易遵循，並示範...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 25a29e783f02e66e1713d8120eb526e1a02961a3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366472"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>將新欄位新增至電影模型和資料表
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程中的更新的版本可[此處](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 更容易遵循，並示範更多的功能。


在本節中，您將使用 Entity Framework Code First 移轉來移轉到模型類別的一些變更，因此變更套用至資料庫。

根據預設，當您使用 Entity Framework Code First 自動建立資料庫，如同稍早在本教學課程中，第一個程式碼將資料表加入至資料庫，以協助追蹤資料庫的結構描述是否與它所產生的模型類別同步。 如果未同步，Entity Framework 會擲回錯誤。 這可讓您更輕鬆地在開發期間，您可能否則只會找到 （藉由難以理解的錯誤） 在執行階段追蹤問題。

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Code First 移轉設定的模型變更

如果您使用 Visual Studio 2012，按兩下*Movies.mdf*從方案總管 以開啟資料庫工具的檔案。 Visual Studio Express for Web 會顯示資料庫總管 中，Visual Studio 2012 會顯示伺服器總管。 如果您使用 Visual Studio 2010，請使用 SQL Server 物件總管。

在資料庫工具 （資料庫總管、 伺服器總管或 SQL Server 物件總管），以滑鼠右鍵按一下`MovieDBContext`，然後選取**刪除**卸除電影資料庫。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

瀏覽至 方案總管。 以滑鼠右鍵按一下*Movies.mdf*檔案，然後選取**刪除**移除電影資料庫。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

建置的應用程式，請確定沒有任何錯誤。

從**工具**功能表上，按一下**程式庫套件管理員**，然後**Package Manager Console**。

![新增組件 Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

在  **Package Manager Console**視窗`PM>`提示字元中輸入"Enable-migrations-ContextTypeName MvcMovie.Models.MovieDBContext"。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

**Enable-migrations** （如上所示） 的命令會建立*Configuration.cs*檔案中的新*移轉*資料夾。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio 會開啟*Configuration.cs*檔案。 取代`Seed`方法中的*Configuration.cs*為下列程式碼的檔案：

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

以滑鼠右鍵按一下紅色曲線底下`Movie`，然後選取**解決**然後**使用** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

此舉會將下列 using 陳述式：

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> 程式碼 First Migrations 會呼叫`Seed`方法之後每個移轉 (也就呼叫**更新資料庫**在套件管理員主控台)，這個方法會更新具有已插入，或將其插入，如果資料列和它們不存在。


**按 CTRL-SHIFT-B 來建置專案。**(下列步驟將會失敗，如果您不要在此時建置。)

下一個步驟是建立`DbMigration`類別初始移轉。 此移轉建立的新資料庫，就是為什麼您刪除*movie.mdf*上一個步驟中的檔案。

在 [ **Package Manager Console** ] 視窗中，輸入命令"add-migration Initial 」 來建立初始移轉。 「 初始 」 的名稱是任意的用來命名移轉檔案建立。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Code First 移轉會建立另一個類別檔案中的*移轉*資料夾 (同名 *{時間戳記}\_Initial.cs* )，而且這個類別包含程式碼會建立資料庫結構描述。 預先移轉檔案名稱被固定時間戳記的來協助進行排序。 檢查 *{時間戳記}\_Initial.cs*檔案，它包含的指示來建立電影資料表的電影資料庫。 當您更新的資料庫中的指示，在此之下 *{時間戳記}\_Initial.cs*檔案將會執行並建立資料庫結構描述。 然後**種子**方法會執行以填入測試資料的資料庫。

在  **Package Manager Console**，輸入命令"更新資料庫內 」 來建立資料庫和執行**種子**方法。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

如果您收到錯誤，指出資料表已經存在，而且無法建立時，可能是因為您已刪除的資料庫之後，您在執行之前，執行應用程式`update-database`。 在此情況下，刪除*Movies.mdf*檔案一次，然後再試一次`update-database`命令。 如果您仍然收到錯誤，刪除移轉資料夾內容，然後開始在此頁面頂端的指示 (這是刪除*Movies.mdf*檔案，然後繼續進行 Enable-migrations)。

執行應用程式，並瀏覽至 */Movies* URL。 會顯示種子資料。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>將 Rating 屬性新增至電影模型

藉由新增新開始`Rating`至現有的屬性`Movie`類別。 開啟*Models\Movie.cs*檔案，並新增`Rating`與下列類似的屬性：

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

完整`Movie`類別現在看起來類似下列的程式碼：

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

建置應用程式使用**建置** &gt;**建置電影**功能表命令，或是按 CTRL shift 鍵。

既然您已更新`Model`類別，您也需要更新*\Views\Movies\Index.cshtml*並*\Views\Movies\Create.cshtml*檢視範本，以顯示新`Rating`瀏覽器檢視中的屬性。

開啟<em>\Views\Movies\Index.cshtml</em>檔案，並新增`<th>Rating</th>`資料行標題後方<strong>價格</strong>資料行。 然後新增`<td>`要呈現的範本結尾附近的資料行`@item.Rating`值。 以下是 哪些更新<em>Index.cshtml</em>檢視範本看起來像：

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

接下來，開啟*\Views\Movies\Create.cshtml*檔案，並新增下列標記表單的結尾附近。 這會呈現文字方塊中，如此當您建立新的電影時，您可以指定評等。

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

您現在已更新的應用程式程式碼，以支援新`Rating`屬性。

現在執行應用程式，並瀏覽至 */Movies* URL。 當您這樣做時，不過，您會看到下列錯誤的其中一個：

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

您之所以看到此錯誤，因為已更新`Movie`應用程式中的模型類別現在是不同的結構描述`Movie`現有資料庫的資料表。 (資料庫資料表中沒有任何 `Rating` 資料行)。


有幾個方法可以解決這個錯誤：

1. 讓 Entity Framework 自動卸除資料庫，並重新依據新的模型類別結構描述來建立資料庫。 這個方法會很方便，當測試資料庫上進行開發它可讓您一併調整更加快速的模型和資料庫結構描述。 它的缺點是您在資料庫中現有的資料遺失，因此您*不*想要在生產資料庫上使用這種方法 ！ 使用初始設定式，將自動植入測試資料的資料庫，通常是開發應用程式的有效方式。 如需有關 Entity Framework 資料庫初始設定式的詳細資訊，請參閱 Tom Dykstra [ASP.NET MVC/Entity Framework 教學課程](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
2. 您可明確修改現有資料庫的結構描述，使其符合模型類別。 這種方法的優點是可以保留您的資料。 您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。
3. 使用 Code First 移轉來更新資料庫結構描述。


在本教學課程中，我們將使用 Code First 移轉。

更新種子方法，使它提供新的資料行的值。 開啟 Migrations\Configuration.cs 檔案，並將評等 欄位新增至電影中的每個物件。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

建置方案，然後再開啟**Package Manager Console**視窗並輸入下列命令：

`add-migration AddRatingMig`

`add-migration`命令會告知移轉架構，檢查目前的電影模型，與目前的電影資料庫結構描述，並建立必要的程式碼將資料庫移轉至新的模型。 AddRatingMig 是任意的用來命名移轉檔案。 您最好使用有意義的名稱，如移轉步驟。

此命令完成時，Visual Studio 會開啟可定義新的類別檔案`DbMIgration`衍生類別，然後在`Up`方法，您可以看到建立新的資料行的程式碼。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

建置方案，並輸入中的 [更新資料庫] 命令**Package Manager Console**視窗。

下圖顯示中的輸出**Package Manager Console**視窗 （前面加上 AddRatingMig 日期戳記將會不同。）

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

重新執行應用程式，並瀏覽至 /Movies URL。 您可以看到新的 [評等] 欄位。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

按一下 **新建**連結，可新增一部新電影。 請注意，您可以新增評分。

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

按一下 [建立] 。 新的影片，包括評等，現在會出現電影清單：

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

您也應該新增`Rating`欄位來編輯、 詳細資料和 SearchIndex 檢視範本。

您可以輸入中的 [更新資料庫] 命令**Package Manager Console**視窗一次，而且不需要變更做，因為結構描述符合模型。

在本節中您已看到如何，您便可以修改模型物件上，並讓資料庫保持同步的變更。 您也了解用來在填入新建立的資料庫，使用範例資料，以便您可以試試看案例。 接下來，讓我們看看如何將更豐富的驗證邏輯加入至模型類別，並啟用 強制執行某些商務規則。

> [!div class="step-by-step"]
> [上一頁](examining-the-edit-methods-and-edit-view.md)
> [下一頁](adding-validation-to-the-model.md)
