---
title: 第8部分，將新欄位新增至 ASP.NET Core MVC 應用程式
author: rick-anderson
description: ASP.NET Core MVC 的教學課程系列第8部分。
ms.author: riande
ms.custom: mvc
ms.date: 12/13/2018
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: d56be93c4dd46b4712fe793ccd453807517a2022
ms.sourcegitcommit: d243fadeda20ad4f142ea60301ae5f5e0d41ed60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2020
ms.locfileid: "84724330"
---
# <a name="part-8-add-a-new-field-to-an-aspnet-core-mvc-app"></a>第8部分，將新欄位新增至 ASP.NET Core MVC 應用程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

在本節中，您會使用 [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First 移轉：

* 在模型中新增一個欄位。
* 將新的欄位移轉至資料庫。

使用 EF Code First 自動建立資料庫時，Code First 會：

* 將資料表新增至資料庫，以追蹤資料庫的結構描述。
* 驗證資料庫與其產生來源的模型類別同步。 如果未同步，EF 會擲回例外狀況。 這可讓您更輕鬆地找出不一致的資料庫/程式碼問題。

## <a name="add-a-rating-property-to-the-movie-model"></a>將 Rating 屬性新增至電影模型

將 `Rating` 屬性新增至 *Models/Movie.cs*：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

建置應用程式

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

 CTRL+SHIFT+B

### <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```dotnetcli
dotnet build
```

### <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

Command ⌘ + B

------

因為您已將欄位新增至 `Movie` 類別，所以需要更新繫結允許清單，以便包含這個新屬性。 在 *MoviesController.cs* 中，更新 `Create` 和 `Edit` 這兩個動作方法的 `[Bind]` 屬性 (attribute)，以包括 `Rating` 屬性 (property)：

```csharp
[Bind("Id,Title,ReleaseDate,Genre,Price,Rating")]
   ```

更新檢視範本，以便在瀏覽器檢視中顯示、建立和編輯新的 `Rating` 屬性。

編輯 */Views/Movies/Index.cshtml* 檔案，然後新增 `Rating` 欄位：

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGenreRating.cshtml?highlight=16,38&range=24-64)]

使用 `Rating` 欄位更新 */Views/Movies/Create.cshtml*。

# <a name="visual-studio--visual-studio-for-mac"></a>[Visual Studio / Visual Studio for Mac](#tab/visual-studio+visual-studio-mac)

您可以複製/貼上前一個「表單群組」，讓 IntelliSense 協助您更新這些欄位。 IntelliSense 會使用[標記協助程式](xref:mvc/views/tag-helpers/intro)。

![在檢視的第二個 Label 項目中，開發人員已針對 asp-for 屬性值鍵入字母 R。 IntelliSense 的操作功能表會顯示可用的欄位，包括 Rating (清單中已自動反白顯示)。 當開發人員按一下欄位，或按下鍵盤上的 Enter 鍵時，即會將值設為 Rating。](new-field/_static/cr.png)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- This tab intentionally left blank. -->

---

更新其餘的範本。

更新 `SeedData` 類別，使其提供新資料行的值。 範例變更如下所示，但您會想要為每個 `new Movie` 進行這項變更。

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

在更新資料庫以包含新欄位之前，應用程式無法運作。 如果立即執行，則會擲回下列 `SqlException`：

`SqlException: Invalid column name 'Rating'.`

此錯誤是因為更新的電影模型類別，不同於現有資料庫之電影資料表的結構描述。 (資料庫資料表中沒有任何 `Rating` 資料行)。

有幾個方法可以解決這個錯誤：

1. 讓 Entity Framework 自動卸除資料庫，並重新依據新的模型類別結構描述來建立資料庫。 在開發週期早期，當您在測試資料庫上進行開發時，這個方法會很方便；其可讓您一併調整模型和資料庫結構描述，更加快速。 不過，它的缺點是您會遺失資料庫中現有的資料 — 因此您不會想在實際執行的資料庫上使用這種方法！ 使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。 這是早期開發和使用 SQLite 時的好方法。

2. 您可明確修改現有資料庫的結構描述，使其符合模型類別。 這種方法的優點是可以保留您的資料。 您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。

3. 使用 Code First 移轉來更新資料庫結構描述。

在本教學課程中，請使用 Code First 移轉。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

 從 [工具]**** 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]****。

  ![PMC 功能表](adding-model/_static/pmc.png)

在 PMC 中，輸入下列命令：

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` 命令會告知移轉架構，檢查目前的 `Movie` 模型與目前的 `Movie` 資料庫結構描述，並建立必要的程式碼以將資料庫移轉至新的模型。

"Rating" 是用來命名移轉檔案的任意名稱。 建議您針對移轉檔案使用有意義的名稱，這更加實用。

如果刪除資料庫中的所有記錄，初始化方法會將內容植入資料庫，並包含 `Rating` 欄位。

# <a name="visual-studio-code--visual-studio-for-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

刪除資料庫並使用移轉重新建立資料庫。 若要刪除資料庫，請刪除資料庫檔案 (*MvcMovie.db*)。 然後執行 `ef database update` 命令：

```dotnetcli
dotnet ef database update
```

---
<!-- End of VS tabs -->

執行應用程式，並確認您可以使用欄位來建立、編輯和顯示電影 `Rating` 。

> [!div class="step-by-step"]
> [上一個](search.md) 
> [下一步](validation.md)
