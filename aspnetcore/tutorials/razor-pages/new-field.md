---
title: "將新欄位新增至 Razor 頁面"
author: rick-anderson
description: "示範如何使用 Entity Framework Core 將新欄位新增至 Razor 頁面"
keywords: "ASP.NET Core,Entity Framework Core,移轉"
ms.author: riande
manager: wpickett
ms.date: 8/7/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: bda00f290043251ad308192c5b1a873ae7cd0d85
ms.sourcegitcommit: e832a9b9f41a8b26a8c88edfd8fc35b8bfd97d5d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2017
---
# <a name="adding-a-new-field-to-a-razor-page"></a>將新欄位新增至 Razor 頁面

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

在本節中，您會使用 [Entity Framework](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) Code First 移轉，將新欄位新增至模型，然後將該變更移轉至資料庫。

當您使用 EF Code First 自動建立資料庫時，Code First 會將資料表新增至資料庫，以協助追蹤資料庫的結構描述是否與其產生的來源模型類別同步。 如果未同步，EF 會擲回例外狀況。 這可讓您更輕鬆地找出不一致的資料庫/程式碼問題。

## <a name="adding-a-rating-property-to-the-movie-model"></a>將 Rating 屬性新增至電影模型

開啟 *Models/Movie.cs* 檔案，然後新增 `Rating` 屬性：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

建置應用程式 (Ctrl+Shift+B)。

編輯 *Pages/Movies/Index.cshtml*，然後新增 `Rating` 欄位：

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

將 `Rating` 欄位新增至 Delete 和 Details 頁面。

使用 `Rating` 欄位更新 *Create.cshtml*。 您可以複製/貼上前一個 `<div>` 項目，讓 IntelliSense 協助您更新這些欄位。 IntelliSense 會使用[標記協助程式](xref:mvc/views/tag-helpers/intro)。

![在檢視的第二個 Label 項目中，開發人員已針對 asp-for 屬性值鍵入字母 R。 IntelliSense 的操作功能表會顯示可用的欄位，包括 Rating (清單中已自動反白顯示)。 當開發人員按一下欄位，或按下鍵盤上的 Enter 鍵時，即會將值設為 Rating。](new-field/_static/cr.png)

下列程式碼會示範 *Create.cshtml* 與 `Rating` 欄位：

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=31-35)]

將 `Rating` 欄位新增至 Edit 頁面。

在更新資料庫以包含新欄位之前，應用程式無法運作。 如果立即執行，應用程式會擲回 `SqlException`：

`SqlException: Invalid column name 'Rating'.`

此錯誤是因為更新的電影模型類別，不同於資料庫之電影資料表的結構描述 (資料庫資料表中沒有任何 `Rating` 資料行)。

有幾個方法可以解決這個錯誤：

1. 讓 Entity Framework 自動卸除資料庫，並使用新的模型類別結構描述來重新建立資料庫。 在開發週期早期，這個方法會很方便；其可讓您一併調整模型和資料庫結構描述，更加快速。 缺點是會遺失在資料庫中的現有資料。 您不想要在生產環境資料庫上使用此方法 ！ 在結構描述變更時卸除資料庫以及使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。

2. 您可明確修改現有資料庫的結構描述，使其符合模型類別。 這種方法的優點是可以保留您的資料。 您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。

3. 使用 Code First 移轉來更新資料庫結構描述。

在本教學課程中，請使用 Code First 移轉。

更新 `SeedData` 類別，使其提供新資料行的值。 範例變更如下所示，但您會想要為每個 `new Movie` 區塊進行這項變更。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

請參閱[完整的 SeedData.cs 檔案](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs) (英文)。

建置方案。

<a name="pmc"></a>

從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。
在 PMC 中，輸入下列命令：

```PMC
Add-Migration Rating
Update-Database
```

`Add-Migration` 命令會告知架構，以便：

* 比較 `Movie` 模型與 `Movie` 資料庫結構描述。
* 建立程式碼，將資料庫結構描述移轉至新模型。

"Rating" 是用來命名移轉檔案的任意名稱。 建議您針對移轉檔案使用有意義的名稱，這更加實用。

<a name="ssox"></a> 如果您刪除資料庫中的所有記錄，初始設定式會將內容植入資料庫，並包含 `Rating` 欄位。 您可以使用瀏覽器或 [Sql Server 物件總管](xref:tutorials/razor-pages/sql#ssox) (SSOX) 的刪除連結來執行這項操作。 若要從 SSOX 中刪除資料庫：

* 在 SSOX 中選取資料庫。
* 以滑鼠右鍵按一下資料庫，然後選取 [刪除]。
* 核取 *[關閉現有的連接]
* 選取 [確定]
* 在 [PMC](xref:tutorials/razor-pages/new-field#pmc) 中，更新資料庫 

    ```PMC
    Update-Database
    ```

執行應用程式，並驗證您可以使用 `Rating` 欄位建立/編輯/顯示電影。 如果未植入資料庫，請停止 IIS Express，然後執行應用程式。

>[!div class="step-by-step"]
[上一步：新增搜尋](xref:tutorials/razor-pages/search)
[下一步：新增欄位](xref:tutorials/razor-pages/new-field)
