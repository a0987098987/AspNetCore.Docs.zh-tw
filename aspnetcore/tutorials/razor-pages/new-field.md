---
title: 將欄位新增至 ASP.NET Core 中的 Razor 頁面
author: rick-anderson
description: 示範如何使用 Entity Framework Core 將新欄位新增至 Razor Pages
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 9b3ad5f6c4b1c9b5f016f5591127c8d1b213948d
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329129"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a>將欄位新增至 ASP.NET Core 中的 Razor 頁面

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/rp/download.md)]

在本節中，您會使用 [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First 移轉：

* 在模型中新增一個欄位。
* 將新的欄位結構描述變更移轉至資料庫。

使用 EF Code First 自動建立資料庫時，Code First 會：

* 將資料表新增至資料庫，以追蹤資料庫的結構描述是否與其產生的來源模型類別同步。
* 如果模型類別與資料庫不同步，EF 會擲回例外狀況。

自動驗證結構描述/模型是否同步，讓您更容易發現不一致的資料庫/程式碼問題。

## <a name="adding-a-rating-property-to-the-movie-model"></a>將 Rating 屬性新增至電影模型

開啟 *Models/Movie.cs* 檔案，然後新增 `Rating` 屬性：

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

建置應用程式。

編輯 *Pages/Movies/Index.cshtml*，然後新增 `Rating` 欄位：

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

更新下列頁面：

* 將 `Rating` 欄位新增至 Delete 和 Details 頁面。
* 使用 `Rating` 欄位更新 [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml)。
* 將 `Rating` 欄位新增至 Edit 頁面。

在更新資料庫以包含新欄位之前，應用程式無法運作。 如果立即執行，應用程式會擲回 `SqlException`：

`SqlException: Invalid column name 'Rating'.`

此錯誤是因為更新的電影模型類別，不同於資料庫之電影資料表的結構描述 (資料庫資料表中沒有任何 `Rating` 資料行)。

有幾個方法可以解決這個錯誤：

1. 讓 Entity Framework 自動卸除資料庫，並使用新的模型類別結構描述來重新建立資料庫。 在開發週期早期，這個方法會很方便；其可讓您一併調整模型和資料庫結構描述，更加快速。 缺點是會遺失在資料庫中的現有資料。 請勿在生產環境資料庫上使用此方法！ 在結構描述變更時卸除資料庫以及使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。

2. 您可明確修改現有資料庫的結構描述，使其符合模型類別。 這種方法的優點是可以保留您的資料。 您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。

3. 使用 Code First 移轉來更新資料庫結構描述。

在本教學課程中，請使用 Code First 移轉。

更新 `SeedData` 類別，使其提供新資料行的值。 範例變更如下所示，但您會想要為每個 `new Movie` 區塊進行這項變更。

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

請參閱[完整的 SeedData.cs 檔案](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs) (英文)。

建置方案。

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a>新增評等欄位的移轉

從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。
在 PMC 中，輸入下列命令：

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` 命令會告知架構，以便：

* 比較 `Movie` 模型與 `Movie` 資料庫結構描述。
* 建立程式碼，將資料庫結構描述移轉至新模型。

"Rating" 是用來命名移轉檔案的任意名稱。 建議您針對移轉檔案使用有意義的名稱，這更加實用。

`Update-Database` 命令會指示架構將結構描述變更套用至資料庫。

<a name="ssox"></a>

如果您刪除資料庫中的所有記錄，初始設定式會將內容植入資料庫，並包含 `Rating` 欄位。 您可以使用瀏覽器或 [Sql Server 物件總管](xref:tutorials/razor-pages/sql#ssox) (SSOX) 的刪除連結來執行這項操作。

另一個選擇是刪除資料庫並使用移轉重新建立資料庫。 若要在 SSOX 中刪除資料庫：

* 在 SSOX 中選取資料庫。
* 以滑鼠右鍵按一下資料庫，然後選取 [刪除]。
* 核取 [關閉現有的連接]。
* 選取 [確定]。
* 在 [PMC](xref:tutorials/razor-pages/new-field#pmc) 中，更新資料庫：

  ```powershell
  Update-Database
  ```

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

<!-- copy/paste this tab to the next. Not worth an include  -->

執行下列 .NET Core CLI 命令：

```console
dotnet ef migrations add Rating
dotnet ef database update
```

`ef migrations add` 命令會告知架構，以便：

* 比較 `Movie` 模型與 `Movie` 資料庫結構描述。
* 建立程式碼，將資料庫結構描述移轉至新模型。

"Rating" 是用來命名移轉檔案的任意名稱。 建議您針對移轉檔案使用有意義的名稱，這更加實用。

`ef database update` 命令會指示架構將結構描述變更套用至資料庫。

如果您刪除資料庫中的所有記錄，初始設定式會將內容植入資料庫，並包含 `Rating` 欄位。 您可以使用瀏覽器中的刪除連結或使用 SQLite 工具來執行這項操作。

另一個選擇是刪除資料庫並使用移轉重新建立資料庫。 若要刪除資料庫，請刪除資料庫檔案 (*MvcMovie.db*)。 然後執行 `ef database update` 命令： 

```console
dotnet ef database update
```

> [!NOTE]
> EF Core SQLite 提供者不支援許多結構描述變更作業。 例如，其支援新增資料行，但不支援移除資料行。 如果您新增移轉來移除資料行，`ef migrations add` 命令會成功，但 `ef database update` 命令會失敗。 您可以手動撰寫移轉程式碼來重建資料表，藉此解決一些限制。 重建資料表需要重新命名現有的資料表、建立新的資料表、將資料複製到新的資料表，以及卸除舊的資料表。 如需詳細資訊，請參閱下列資源：
> * [SQLite EF Core 資料庫提供者限制](/ef/core/providers/sqlite/limitations)
> * [自訂移轉程式碼](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [資料植入](/ef/core/modeling/data-seeding)

---  
<!-- End of VS tabs -->

執行應用程式，並驗證您可以使用 `Rating` 欄位建立/編輯/顯示電影。 若未植入資料庫，請在 `SeedData.Initialize` 方法中設定中斷點。

> [!div class="step-by-step"]
> [上一步：新增搜尋](xref:tutorials/razor-pages/search)
> [下一步：新增驗證](xref:tutorials/razor-pages/validation)
