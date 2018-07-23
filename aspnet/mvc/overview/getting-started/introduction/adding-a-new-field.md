---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: 加入新的欄位。 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: a4eb759646fc77bbba687d8b4914465d58cd3aaa
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2018
ms.locfileid: "38151453"
---
<a name="adding-a-new-field"></a>新增欄位
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

在本節中，您將使用 Entity Framework Code First 移轉來移轉到模型類別的一些變更，因此變更套用至資料庫。

根據預設，當您使用 Entity Framework Code First 自動建立資料庫，如同稍早在本教學課程中，第一個程式碼將資料表加入至資料庫，以協助追蹤資料庫的結構描述是否與它所產生的模型類別同步。 如果未同步，Entity Framework 會擲回錯誤。 這可讓您更輕鬆地在開發期間，您可能否則只會找到 （藉由難以理解的錯誤） 在執行階段追蹤問題。

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Code First 移轉設定的模型變更

瀏覽至 [方案總管] 中。 以滑鼠右鍵按一下*Movies.mdf*檔案，然後選取**刪除**移除電影資料庫。 如果您沒有看到*Movies.mdf*檔案中，按一下**顯示所有檔案**圖示中的紅色外框，如下所示。

![](adding-a-new-field/_static/image1.png)

建置的應用程式，請確定沒有任何錯誤。

從**工具**功能表上，按一下**NuGet 套件管理員**，然後**Package Manager Console**。

![新增組件 Man](adding-a-new-field/_static/image2.png)

在 **Package Manager Console**視窗`PM>`提示字元中輸入

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

**Enable-migrations** （如上所示） 的命令會建立*Configuration.cs*檔案中的新*移轉*資料夾。

![](adding-a-new-field/_static/image4.png)

Visual Studio 會開啟*Configuration.cs*檔案。 取代`Seed`方法中的*Configuration.cs*為下列程式碼的檔案：

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

將滑鼠停留在紅色的波浪線`Movie`，按一下 `Show Potential Fixes` ，然後按一下**使用** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

此舉會將下列 using 陳述式：

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> 程式碼 First Migrations 會呼叫`Seed`方法之後每個移轉 (也就呼叫**更新資料庫**在套件管理員主控台)，這個方法會更新具有已插入，或將其插入，如果資料列和它們不存在。
> 
> [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)下列程式碼的方法會執行"upsert"作業：
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> 因為[種子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法會執行每個移轉，因為您嘗試新增的資料列會有第一個移轉建立資料庫之後插入資料。 「[Upsert](http://en.wikipedia.org/wiki/Upsert)」 作業會讓您嘗試插入資料列已經存在，會發生的錯誤，但是它會覆寫資料，您可能會在測試應用程式時所做的任何變更。 某些資料表中的測試資料與您可能不想發生這種情況： 在某些情況下當您將資料變更時測試您的要資料庫更新後要保持變更。 在此情況下您想要進行條件式的 insert 作業： 插入資料列，只有當不存在。   
> 
> 第一個參數傳遞給[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法會指定要用來檢查資料列是否已經存在的屬性。 為您提供測試電影資料`Title`屬性可以使用針對此目的，因為清單中的每個書名是唯一：
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> 此程式碼會假設這些項目是唯一。 如果您以手動方式新增重複的標題，您會收到下列例外狀況下一次您執行移轉。   
> 
>  *序列包含一個以上的項目*  
> 
> 如需詳細資訊[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法，請參閱[負責使用 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**按 CTRL-SHIFT-B 來建置專案。**（如果您不要在此時建置會失敗的下列步驟）。

下一個步驟是建立`DbMigration`類別初始移轉。 此移轉建立的新資料庫，就是為什麼您刪除*movie.mdf*上一個步驟中的檔案。

在 [ **Package Manager Console** ] 視窗中，輸入命令`add-migration Initial`建立初始移轉。 「 初始 」 的名稱是任意的用來命名移轉檔案建立。

![](adding-a-new-field/_static/image6.png)

Code First 移轉會建立另一個類別檔案中的*移轉*資料夾 (同名 *{時間戳記}\_Initial.cs* )，而且這個類別包含程式碼會建立資料庫結構描述。 預先移轉檔案名稱被固定時間戳記的來協助進行排序。 檢查 *{時間戳記}\_Initial.cs*檔案，它包含的指示來建立`Movies`電影資料庫的資料表。 當您更新的資料庫中的指示，在此之下 *{時間戳記}\_Initial.cs*檔案將會執行並建立資料庫結構描述。 然後**種子**方法會執行以填入測試資料的資料庫。

在 **Package Manager Console**，輸入命令`update-database`來建立資料庫和執行`Seed`方法。

![](adding-a-new-field/_static/image7.png)

如果您收到錯誤，指出資料表已經存在，而且無法建立時，可能是因為您已刪除的資料庫之後，您在執行之前，執行應用程式`update-database`。 在此情況下，刪除*Movies.mdf*檔案一次，然後再試一次`update-database`命令。 如果您仍然收到錯誤，刪除移轉資料夾內容，然後開始在此頁面頂端的指示 (這是刪除*Movies.mdf*檔案，然後繼續進行 Enable-migrations)。 如果您仍然收到錯誤，開啟 SQL Server 物件總管，並從清單中移除資料庫。

執行應用程式，並瀏覽至 */Movies* URL。 會顯示種子資料。

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>將 Rating 屬性新增至電影模型

藉由新增新開始`Rating`至現有的屬性`Movie`類別。 開啟*Models\Movie.cs*檔案，並新增`Rating`與下列類似的屬性：

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

完整`Movie`類別現在看起來類似下列的程式碼：

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

建置應用程式 (Ctrl + Shift + B)。

因為您已將新欄位加入到`Movie`類別，您也需要更新繫結*允許清單*以便將包含新的屬性。 更新`bind`屬性`Create`並`Edit`動作的方法，以納入`Rating`屬性：

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

您也需要更新檢視範本，以便在瀏覽器檢視中顯示、建立和編輯新的 `Rating` 屬性。

開啟 *\Views\Movies\Index.cshtml* 檔案，並新增`<th>Rating</th>`資料行標題後方**價格**資料行。 然後新增`<td>`要呈現的範本結尾附近的資料行`@item.Rating`值。 以下是 哪些更新 *Index.cshtml* 檢視範本看起來像：

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

接下來，開啟*\Views\Movies\Create.cshtml*檔案，並新增`Rating`下列 highlighed 標記欄位。 這會呈現文字方塊中，如此當您建立新的電影時，您可以指定評等。

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

您現在已更新的應用程式程式碼，以支援新`Rating`屬性。

執行應用程式，並瀏覽至 */Movies* URL。 當您這樣做時，不過，您會看到下列錯誤的其中一個：

![](adding-a-new-field/_static/image9.png)  
  
備份 'MovieDBContext' 內容的模型已變更，因為所建立的資料庫。 請考慮使用 Code First 移轉更新資料庫 (https://go.microsoft.com/fwlink/?LinkId=238269)。

![](adding-a-new-field/_static/image10.png)

您之所以看到此錯誤，因為已更新`Movie`應用程式中的模型類別現在是不同的結構描述`Movie`現有資料庫的資料表。 (資料庫資料表中沒有任何 `Rating` 資料行)。


有幾個方法可以解決這個錯誤：

1. 讓 Entity Framework 自動卸除資料庫，並重新依據新的模型類別結構描述來建立資料庫。 在開發週期早期，當您在測試資料庫上進行開發時，這個方法會很方便；其可讓您一併調整模型和資料庫結構描述，更加快速。 它的缺點是您在資料庫中現有的資料遺失，因此您*不*想要在生產資料庫上使用這種方法 ！ 使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。 如需有關 Entity Framework 資料庫初始設定式的詳細資訊，請參閱 < [ASP.NET MVC/Entity Framework 教學課程](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
2. 您可明確修改現有資料庫的結構描述，使其符合模型類別。 這種方法的優點是可以保留您的資料。 您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。
3. 使用 Code First 移轉來更新資料庫結構描述。


在本教學課程中，我們將使用 Code First 移轉。

更新種子方法，使它提供新的資料行的值。 開啟 Migrations\Configuration.cs 檔案，並將評等 欄位新增至電影中的每個物件。

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

建置方案，然後再開啟**Package Manager Console**視窗並輸入下列命令：

`add-migration Rating`

`add-migration`命令會告知移轉架構，檢查目前的電影模型，與目前的電影資料庫結構描述，並建立必要的程式碼將資料庫移轉至新的模型。 名稱*分級*是任意的用來命名移轉檔案。 您最好使用有意義的名稱，如移轉步驟。

此命令完成時，Visual Studio 會開啟可定義新的類別檔案`DbMigration`衍生類別，然後在`Up`方法，您可以看到建立新的資料行的程式碼。

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

建置方案，然後再輸入`update-database`命令所**Package Manager Console**視窗。

下圖顯示中的輸出 [ **Package Manager Console** ] 視窗 (日期戳記前面加上*分級*會不同。)

![](adding-a-new-field/_static/image11.png)

重新執行應用程式，並瀏覽至 /Movies URL。 您可以看到新的 [評等] 欄位。

![](adding-a-new-field/_static/image12.png)

按一下 新建**連結，可新增一部新電影。 請注意，您可以新增評分。

![7_CreateRioII](adding-a-new-field/_static/image13.png)

按一下 [建立] 。 新的影片，包括評等，現在會出現電影清單：

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

現在，專案使用移轉，您不需要卸除資料庫，當您加入新的欄位或否則更新結構描述。 在下一步 區段中，我們將進行更多的結構描述變更，並使用移轉來更新資料庫。

您也應該新增`Rating`欄位設為編輯、 詳細資料和刪除的檢視範本。

您可以輸入中的 [更新資料庫] 命令**Package Manager Console**視窗一次，並沒有移轉程式碼會執行，因為結構描述符合模型。 不過，執行 [更新資料庫] 就會執行`Seed`同樣地，方法，因為如果您變更的任何識別值種子資料，所做的變更將會遺失`Seed`方法會插入資料。 您可以深入了解`Seed`方法，在 Tom Dykstra 熱門[ASP.NET MVC/Entity Framework 教學課程](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。

在本節中您已看到如何，您便可以修改模型物件上，並讓資料庫保持同步的變更。 您也了解用來在填入新建立的資料庫，使用範例資料，以便您可以試試看案例。 這是只是快速介紹 Code First，請參閱 <<c0> [ 建立 ASP.NET MVC 應用程式的 Entity Framework 資料模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)更完整的教學課程中，在主題上。 接下來，讓我們看看如何將更豐富的驗證邏輯加入至模型類別，並啟用 強制執行某些商務規則。

> [!div class="step-by-step"]
> [上一頁](adding-search.md)
> [下一頁](adding-validation.md)
