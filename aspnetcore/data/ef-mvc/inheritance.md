---
title: ASP.NET Core MVC 與 EF Core - 繼承 - 9/10
author: rick-anderson
description: 本教學課程將說明如何在 ASP.NET Core 應用程式中使用 Entity Framework Core，以實作資料模型中的繼承。
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 60417040dd296311e1aecff8f224aadf8da82779
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090754"
---
# <a name="aspnet-core-mvc-with-ef-core---inheritance---9-of-10"></a>ASP.NET Core MVC 與 EF Core - 繼承 - 9/10

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 Web 應用程式將示範如何以 Entity Framework Core 和 Visual Studio 來建立 ASP.NET Core MVC Web 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](intro.md)。

在上一個教學課程中，您已處理並行存取例外狀況。 本教學課程將示範如何在資料模型中實作繼承。

在物件導向程式設計中，您可以使用繼承，以便重複使用程式碼。 在本教學課程中，您將變更 `Instructor` 和 `Student` 類別，讓它們衍生自 `Person` 基底類別，而此基底類別包含講師和學生通用的屬性，例如 `LastName`。 您不會新增或變更任何網頁，但是您將變更一些程式碼，這些變更將會自動反映在資料庫中。

## <a name="options-for-mapping-inheritance-to-database-tables"></a>將繼承對應至資料庫資料表的選項

School 資料模型中的 `Instructor` 和 `Student` 類別有數個完全相同的屬性：

![Student 和 Instructor 類別。](inheritance/_static/no-inheritance.png)

假設您想要針對 `Instructor` 和 `Student` 實體所共用的屬性消除多餘的程式碼。 或者，您想要撰寫服務，以便用來格式化名稱，而無需在意名稱是來自講師還是學生。 您可以建立只包含這些共用屬性的 `Person` 基底類別，然後使 `Instructor` 和 `Student` 類別繼承自該基底類別，如下圖所示：

![衍生自 Person 類別的 Student 和 Instructor 類別](inheritance/_static/inheritance.png)

有幾種方式可以在資料庫中表示此繼承結構。 您擁有的 Person 資料表可能在單一資料表中同時包含學生和講師的資訊。 有些資料行僅適用於講師 (HireDate)，有些只適用於學生 (EnrollmentDate)，有些則兩者通用 (LastName、FirstName)。 一般而言，您必須有指出每個資料列代表哪種類型的鑑別子資料行。 例如，鑑別子資料行的 "Instructor" 代表講師，而 "Student" 代表學生。

![單表 (Table-per-hierarchy) 範例](inheritance/_static/tph.png)

從單一資料庫資料表產生實體繼承結構的這種模式稱為單表 (TPH) 繼承。

替代方法是讓資料庫看起來更像繼承結構。 比方說，您可以只在 Person 資料表中包含名稱欄位，而在個別的 Instructor 和 Student 資料表中包含日期欄位。

![一類一表 (Table-Per-Type) 繼承](inheritance/_static/tpt.png)

針對每個實體類別建立資料庫資料表的這種模式稱為一類一表 (TPT) 繼承。

還有另一個選項是將所有的非抽象類型對應至個別資料表。 類別的所有屬性 (包括繼承的屬性) 都會對應至對應資料表的資料行。 這個模式稱為一實體類一表 (TPC) 繼承。 如果您已針對 Person、Student 和 Instructor 類別實作 TPC 繼承 (如上所示)，Student 和 Instructor 資料表在實作繼承之後，看起來與實作之前一樣。

比起 TPT 繼承模式，TPC 和 TPH 繼承模式通常會提供更好的效能，因為 TPT 模式可能會導致複雜的聯結查詢。

本教學課程將示範如何實作 TPH 繼承。 TPH 是 Entity Framework Core 支援的唯一繼承模式。  您要做的是建立 `Person` 類別、變更 `Instructor` 和 `Student` 類別以衍生自 `Person`、將新類別新增至 `DbContext`，以及建立移轉。

> [!TIP]
> 在進行下列變更之前，請考慮儲存專案的複本。  那麼，當您遇到問題而需要從新開始時，就可以輕鬆地從儲存的專案開始，而不是還原本教學課程完成的步驟或回到整個系列的開頭。

## <a name="create-the-person-class"></a>建立 Person 類別

在 Models 資料夾中，建立 Person.cs，並以下列程式碼取代範本程式碼：

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>使 Student 和 Instructor 類別繼承自 Person 類別

在 *Instructor.cs* 中，從 Person 類別衍生 Instructor 類別，並移除索引鍵和名稱欄位。 程式碼看起來應該如下列範例所示：

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

在 *Student.cs* 中進行相同的變更。

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>將 Person 實體類型新增至資料模型

將 Person 實體類型新增至 *SchoolContext.cs*。 新增的行已醒目提示。

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

這就是 Entity Framework 為了設定單表繼承而必須執行的所有工作。 如您所見，當資料庫更新時，會有一個 Person 資料表來替代 Student 和 Instructor 資料表。

## <a name="create-and-customize-migration-code"></a>建立和自訂移轉程式碼

儲存您的變更，並建置專案。 然後，在專案資料夾中開啟命令視窗，並輸入下列命令：

```console
dotnet ef migrations add Inheritance
```

還不要執行 `database update` 命令。 該命令將導致資料遺失，因為它會卸除 Instructor 資料表，然後將 Student 資料表重新命名為 Person。 您必須提供自訂程式碼來保留現有的資料。

開啟 Migrations/\<時間戳記>_Inheritance.cs，並以下列程式碼取代 `Up` 方法：

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

此程式碼負責下列資料庫更新工作：

* 移除指向 Student 資料表的外部索引鍵條件約束和索引。

* 將 Instructor 資料表重新命名為 Person，並對其進行所需的變更來儲存 Student 資料：

* 針對學生新增可為 null 的 EnrollmentDate。

* 新增 Discriminator 資料行，以指出資料列適用於學生或講師。

* 使 HireDate 成為可為 Null，因為學生資料列不會有雇用日期。

* 新增暫存欄位，它將用來更新指向學生的外部索引鍵。 當您將學生複製到 Person 資料表時，它們會取得新的主索引鍵值。

* 將 Student 資料表中的資料複製到 Person 資料表。 這會導致學生獲指派新的主索引鍵值。

* 修正指向學生的外部索引鍵值。

* 重新建立外部索引鍵條件約束和索引，現在將它們指向 Person 資料表。

(如果您使用 GUID 而不是整數作為主索引鍵類型，學生的主索引鍵值將無需變更，而且可能已省略其中幾個步驟。)

執行 `database update` 命令：

```console
dotnet ef database update
```

(在生產環境系統中，您會對 `Down` 方法進行對應的變更，以防您必須使用該方法來回到先前的資料庫版本。 在此教學課程中，您將不會使用 `Down` 方法。)

> [!NOTE]
> 在具有現有資料的資料庫中進行結構描述變更時，可能會收到其他錯誤。 如果收到無法解決的移轉錯誤，您可以變更連接字串中的資料庫名稱，或刪除該資料庫。 使用新資料庫時，沒有可移轉的資料，因此 update-database 命令更可能會在沒有錯誤的情況下完成。 若要刪除資料庫，請使用 SSOX 或執行 `database drop` CLI 命令。

## <a name="test-with-inheritance-implemented"></a>使用實作的繼承進行測試

執行應用程式，然後嘗試各種頁面。 一切項目的運作與之前一樣。

在 [SQL Server 物件總管] 中，展開 [資料連線/SchoolContext]，然後展開 [資料表]，您會看到 Person 資料表已取代 Student 和 Instructor 資料表。 開啟 Person 資料表設計工具，您會看到它包含了過去位在 Student 和 Instructor 資料表中的所有資料行。

![SSOX 中的 Person 資料表](inheritance/_static/ssox-person-table.png)

以滑鼠右鍵按一下 Person 資料表，然後按一下 [顯示資料表資料] 以查看鑑別子資料行。

![SSOX 中的 Person 資料 - 資料表資料](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>總結

您已針對 `Person`、`Student` 和 `Instructor` 類別實作單表繼承。 如需 Entity Framework Core 中有關繼承的詳細資訊，請參閱[繼承](/ef/core/modeling/inheritance)。 在下一個教學課程中，您將了解如何處理各種相對進階的 Entity Framework 案例。

::: moniker-end

> [!div class="step-by-step"]
> [上一頁](concurrency.md)
> [下一頁](advanced.md)
