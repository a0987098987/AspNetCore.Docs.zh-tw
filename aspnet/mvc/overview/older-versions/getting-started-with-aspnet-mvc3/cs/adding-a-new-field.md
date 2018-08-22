---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: 將新欄位新增至電影模型和資料表 (C#) |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 62995c1b53dad12f4d9202333520080af9e71e1a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827053"
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>將新欄位新增至電影模型和資料表 (C#)
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程中的更新的版本可[此處](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 更容易遵循，並示範更多的功能。
> 
> 
> 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 在開始之前，請確定您已安裝符合下列先決條件。 您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附了 C# 原始程式碼的 Visual Web Developer 專案。 [下載 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 Visual Basic，切換到[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。


在本節中您將模型類別來進行一些變更，並了解如何更新資料庫結構描述，以符合的模型變更。

## <a name="adding-a-rating-property-to-the-movie-model"></a>將 Rating 屬性新增至電影模型

藉由新增新開始`Rating`至現有的屬性`Movie`類別。 開啟 *Movie.cs* 檔案，並新增`Rating`與下列類似的屬性：

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

完整`Movie`類別現在看起來類似下列的程式碼：

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

重新編譯應用程式使用**偵錯** &gt;**建置電影**功能表命令。

既然您已更新`Model`類別，您也需要更新*\Views\Movies\Index.cshtml*並*\Views\Movies\Create.cshtml*檢視範本，以支援新`Rating`屬性。

開啟 *\Views\Movies\Index.cshtml* 檔案，並新增`<th>Rating</th>`資料行標題後方**價格**資料行。 然後新增`<td>`要呈現的範本結尾附近的資料行`@item.Rating`值。 以下是 哪些更新*Index.cshtml*檢視範本看起來像：

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

接下來，開啟*\Views\Movies\Create.cshtml*檔案，並新增下列標記表單的結尾附近。 這會呈現文字方塊中，如此當您建立新的電影時，您可以指定評等。

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>管理模型和資料庫結構描述相異

您現在已更新的應用程式程式碼，以支援新`Rating`屬性。

現在執行應用程式，並瀏覽至 */Movies* URL。 當您這樣做時，不過，您會看到下列錯誤：

![](adding-a-new-field/_static/image1.png)

您之所以看到此錯誤，因為已更新`Movie`應用程式中的模型類別現在是不同的結構描述`Movie`現有資料庫的資料表。 (資料庫資料表中沒有任何 `Rating` 資料行)。

根據預設，當您使用 Entity Framework Code First 自動建立資料庫，如同稍早在本教學課程中，第一個程式碼將資料表加入至資料庫，以協助追蹤資料庫的結構描述是否與它所產生的模型類別同步。 如果未同步，Entity Framework 會擲回錯誤。 這可讓您更輕鬆地在開發期間，您可能否則只會找到 （藉由難以理解的錯誤） 在執行階段追蹤問題。 「 同步處理檢查 」 功能是什麼情況導致的錯誤訊息，會顯示您剛剛看到。

有兩種方法可以解決這個錯誤：

1. 讓 Entity Framework 自動卸除資料庫，並重新依據新的模型類別結構描述來建立資料庫。 這個方法會很方便的測試資料庫上進行開發時因為它可讓您一併調整更加快速的模型和資料庫結構描述。 它的缺點是您在資料庫中現有的資料遺失，因此您 *不* 想要在生產資料庫上使用這種方法 ！
2. 您可明確修改現有資料庫的結構描述，使其符合模型類別。 這種方法的優點是可以保留您的資料。 您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。

本教學課程中，我們將使用第一種方法，就會看到 Entity Framework Code First 每當模型變更時自動重新建立資料庫。

## <a name="automatically-re-creating-the-database-on-model-changes"></a>自動重新建立資料庫模型變更

讓我們更新應用程式，使程式碼第一個自動卸除並重新建立資料庫，每當您變更應用程式的模型。

> [!NOTE] 
> 
> **警告**您應該啟用這種方法是自動卸除並重新建立資料庫，只有當您使用開發或測試資料庫時，才與 *永遠不會* 生產資料庫，其中包含實際資料。 使用實際執行伺服器上可能會導致資料遺失。


在 [**方案總管] 中**，以滑鼠右鍵按一下 *模型* 資料夾中，選取**新增**，然後選取**類別**。

![](adding-a-new-field/_static/image2.png)

將類別命名為"MovieInitializer 」。 更新`MovieInitializer`類別包含下列程式碼：

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

`MovieInitializer`類別會指定應該卸除模型所使用的資料庫，並自動重新建立如果變更的模型類別。 程式碼包含`Seed`方法，以指定一些預設資料，來自動新增資料庫中任何時間它已建立 （或重新建立）。 這會提供實用的方式，以填入某些範例資料，資料庫，而不需要您手動填入每次進行變更的模型。

既然您已定義`MovieInitializer`類別，您會想要連接它，以便每次應用程式執行時，它會檢查是否在資料庫中結構描述不同的模型類別。 如有需要，您可以執行初始設定式來重新建立要符合的模型並於其中填入範例資料與資料庫的資料庫。

開啟 *Global.asax* 檔案的根目錄`MvcMovies`專案：

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

*Global.asax* 檔案中包含的類別，定義整個應用程式專案，並包含`Application_Start`應用程式第一次啟動時執行的事件處理常式。

讓我們加入兩個 using 陳述式檔案的頂端。 第一個參考 Entity Framework 命名空間和第二個參考的命名空間，我們`MovieInitializer`類別存留於：

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

然後尋找`Application_Start`方法並將呼叫加入`Database.SetInitializer`開頭的方法，如下所示：

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

`Database.SetInitializer`剛加入的陳述式指出所使用的資料庫`MovieDBContext`執行個體應該會自動刪除並重新建立如果不符合結構描述和資料庫。 如您所見，它也會填入範例資料中指定資料庫和`MovieInitializer`類別。

關閉 *Global.asax* 檔案。

重新執行應用程式，並瀏覽至 */Movies* URL。 當應用程式啟動時，它會偵測模型結構不再符合資料庫結構描述。 它會自動重新建立資料庫，以符合新的模型結構，並於其中填入資料庫與範例影片：

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

按一下 新建**連結，可新增一部新電影。 請注意，您可以新增評分。

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)

按一下 [建立] 。 新的影片，包括評等，現在會出現電影清單：

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

在本節中您已看到如何，您便可以修改模型物件上，並讓資料庫保持同步的變更。 您也了解用來在填入新建立的資料庫，使用範例資料，以便您可以試試看案例。 接下來，讓我們看看如何將更豐富的驗證邏輯加入至模型類別，並啟用 強制執行某些商務規則。

> [!div class="step-by-step"]
> [上一頁](examining-the-edit-methods-and-edit-view.md)
> [下一頁](adding-validation-to-the-model.md)
