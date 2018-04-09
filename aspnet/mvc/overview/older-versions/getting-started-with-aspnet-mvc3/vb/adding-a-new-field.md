---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: 將新欄位加入至電影模型和資料庫資料表 (VB) |Microsoft 文件
author: Rick-Anderson
description: 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 5927b7d977e375881fe618b4b844cbd708023ba1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a>將新欄位加入至電影模型和資料庫資料表 (VB)
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 開始之前，請確定您已安裝下面所列的必要條件。 您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附在 Visual Web Developer 專案中的使用 VB.NET 原始程式碼。 [下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 C#，切換至[C# 版本](../cs/adding-a-new-field.md)本教學課程。


本節中您將模型類別來進行一些變更，並了解如何更新資料庫結構描述，以符合模型變更。

## <a name="adding-a-rating-property-to-the-movie-model"></a>將 Rating 屬性新增至電影模型

開始透過新增`Rating`屬性至現有`Movie`類別。 開啟*Movie.cs*檔案，然後加入`Rating`屬性如下所示：

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

完整`Movie`類別現在看起來類似下列程式碼：

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

重新編譯應用程式使用**偵錯** &gt;**建置影片**功能表命令。

既然您已更新`Model`類別，您也需要更新*\Views\Movies\Index.vbhtml*和*\Views\Movies\Create.vbhtml*檢視範本，以支援新`Rating`屬性。

開啟<em>\Views\Movies\Index.vbhtml</em>檔案，然後加入`<th>Rating</th>`欄名後方<strong>價格</strong>資料行。 然後加入`<td>`即將要呈現之範本的結束欄`@item.Rating`值。 以下是哪些更新<em>Index.vbhtml</em>檢視範本外觀就像：

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

接下來，開啟*\Views\Movies\Create.vbhtml*檔案，然後加入下列標記表單的結尾附近。 這會呈現文字方塊中，以便建立新的電影時，您可以指定評等。

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>管理模型和資料庫結構描述的差異

您現在已更新以支援新的應用程式程式碼`Rating`屬性。

現在執行應用程式中，瀏覽至*/Movies* URL。 當您這樣做時，不過，您會看到下列錯誤：

![](adding-a-new-field/_static/image1.png)

您在看到這個錯誤，因為已更新`Movie`應用程式中的模型類別現在是不同的結構描述`Movie`現有資料庫的資料表。 (資料庫資料表中沒有任何 `Rating` 資料行)。

根據預設，當您使用 Entity Framework Code First 來自動建立的資料庫，您可以如同稍早在本教學課程，第一個程式碼將資料表加入至要協助追蹤資料庫的結構描述是否從產生的模型類別同步的資料庫。 如果不是同步，Entity Framework 會擲回錯誤。 這可讓您更輕鬆地在開發期間，您可能否則只 （藉由晦澀難懂的錯誤） 在執行階段追蹤問題。 檢查同步處理的功能是什麼造成的錯誤訊息，顯示您看到的。

有兩種方法來解決錯誤：

1. 讓 Entity Framework 自動卸除資料庫，並重新依據新的模型類別結構描述來建立資料庫。 這種方法時，非常方便進行開發的測試資料庫上，因為它可讓您快速發展的模型和資料庫結構描述在一起。 這個選項的缺點是，您在資料庫中現有的資料遺失，因此您*不*想要在實際執行資料庫上使用此方法 ！
2. 您可明確修改現有資料庫的結構描述，使其符合模型類別。 這種方法的優點是可以保留您的資料。 您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。

本教學課程中，我們將使用第一種方法，您必須在 Entity Framework Code First 每當模型變更會自動重新建立資料庫。

## <a name="automatically-re-creating-the-database-on-model-changes"></a>自動重新建立的資料庫上的模型變更

讓我們來更新應用程式，使程式碼第一個自動卸除並重新建立資料庫，每當您變更應用程式的模型。

> [!NOTE] 
> 
> **警告**您應該啟用自動卸除和重新建立資料庫，僅當您正在使用開發或測試的資料庫，這種方法和*從未*生產資料庫，其中包含實際資料。 使用實際執行伺服器上可能會導致資料遺失。


在**方案總管 中**，以滑鼠右鍵按一下*模型*資料夾中，選取**新增**，然後選取**類別**。

![](adding-a-new-field/_static/image2.png)

將類別&quot;MovieInitializer&quot;。 更新`MovieInitializer`類別包含下列程式碼：

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

`MovieInitializer`類別會指定應該卸除模型所使用的資料庫，並自動重新建立如果曾變更模型類別。 程式碼包含`Seed`方法，以指定預設資料會自動加入到資料庫任何時間已建立 （或重新建立）。 這會提供實用的方式，來填入資料庫與一些範例資料，而不需要您手動擴展每次進行變更的模型。

既然您已定義`MovieInitializer`類別，您會想要連接它，以便每次應用程式執行時，它會檢查是否不同於資料庫中的結構描述模型類別。 如有需要，您可以執行初始設定式來重新建立要符合的模型，並於其中填入範例資料與資料庫的資料庫。

開啟*Global.asax*檔案的根目錄，`MvcMovies`專案：

*Global.asax*檔案包含的類別會定義整個應用程式的專案，並包含`Application_Start`應用程式第一次啟動時執行的事件處理常式。

尋找`Application_Start`方法與將呼叫加入`Database.SetInitializer`開頭的方法，如下所示：

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

`Database.SetInitializer`您剛才加入的陳述式指出所使用的資料庫`MovieDBContext`執行個體應該會自動刪除並重新建立如果不相符的結構描述和資料庫。 如您所見，它也會填入範例資料中所指定的資料庫和`MovieInitializer`類別。

關閉*Global.asax*檔案。

重新執行應用程式，並瀏覽至*/Movies* URL。 當應用程式啟動時，它會偵測模型結構不再符合資料庫結構描述。 它會自動重新建立資料庫，以符合新的模型結構，並於其中填入資料庫的範例影片：

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

按一下**新建**連結加入新的電影。 請注意，您可以加入評等。

[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)

按一下 [建立] 。 新的電影，包括評等，現在會顯示列出的電影中：

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

本節中您已看到如何修改模型物件和資料庫中與變更保持同步。 您也學到如何填入新建立的資料庫中的範例資料，因此您可以嘗試的案例。 接下來，讓我們看看如何將更豐富的驗證邏輯加入至模型類別，並啟用某些商務規則，以強制執行。

> [!div class="step-by-step"]
> [上一頁](examining-the-edit-methods-and-edit-view.md)
> [下一頁](adding-validation-to-the-model.md)
