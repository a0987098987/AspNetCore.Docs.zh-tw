---
title: "使用 EF 核心繼承-9 10 個 ASP.NET Core MVC"
author: tdykstra
description: "本教學課程將說明如何在資料模型中，使用 Entity Framework Core ASP.NET Core 應用程式中實作繼承。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/inheritance
ms.openlocfilehash: a4ae696bdd114ab9c36d1218f753fa3d515f2300
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a>繼承的 EF Core 與 ASP.NET Core MVC 教學課程 (9 / 10)

由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。

在上一個教學課程中，您可以處理並行存取例外狀況。 本教學課程會示範如何實作資料模型中的繼承。

在物件導向程式設計中，您可以使用繼承，以便重複使用程式碼。 在此教學課程中，您要變更`Instructor`和`Student`類別，讓它們衍生自`Person`基底類別，其中包含屬性，例如`LastName`通用講師和學生。 您將不會加入或變更任何網頁，但是您要變更的一些程式碼，而且這些變更將會自動反映在資料庫中。

## <a name="options-for-mapping-inheritance-to-database-tables"></a>將繼承對應至資料庫資料表的選項

`Instructor`和`Student`學校資料模型中的類別有數個完全相同的屬性：

![學生和講師類別](inheritance/_static/no-inheritance.png)

假設您想要消除多餘的程式碼所共用的屬性`Instructor`和`Student`實體。 或者您想要撰寫可以不含關懷名稱是否會來自講師或學生格式名稱的服務。 您可以建立`Person`基底類別，其中包含這些共用屬性，則請`Instructor`和`Student`類別繼承自該基底類別，如下圖所示：

![Student 與講師類別衍生自人員類別](inheritance/_static/inheritance.png)

有幾種的方式可能會表示此繼承結構，在資料庫中。 您可能會有一個 Person 資料表，包含學生和講師單一資料表中的資訊。 某些資料行無法僅適用於講師 (HireDate)，有些則只能學生 (EnrollmentDate)，有些則兩個 (LastName FirstName)。 一般而言，您必須指出每個資料列都代表哪種類型的鑑別子資料行。 例如，鑑別子資料行可能會有 「 講師 」 講師和 「 學生 」 學生版。

![每個階層的資料表範例](inheritance/_static/tph.png)

這種模式的單一資料庫資料表產生實體繼承結構會呼叫每個階層的資料表 (TPH) 繼承。

替代方法是讓資料庫起來更繼承結構。 比方說，您無法只 Person 資料表中的名稱欄位並且個別位講師和學生的資料表與日期欄位。

![一類一表 (Table-Per-Type) 繼承](inheritance/_static/tpt.png)

這種模式的資料庫資料表針對每個實體類別會呼叫每個型別 (TPT) 繼承資料表。

還有另一個選項是將所有的非抽象類型對應至個別資料表。 所有類別的屬性，包括繼承的屬性，對應至相對應資料表的資料行。 這個模式稱為資料表每個具象類別 (TPC) 繼承。 如果您實作 TPC 的人員、 學生和講師類別繼承，如先前所示，Student 與講師資料表看起來會一樣比之前未實作繼承之後。

TPC 和 TPH 繼承模式通常會傳遞更佳的效能比 TPT 繼承的模式，因為 TPT 模式可能會導致複雜聯結的查詢。

本教學課程會示範如何實作 TPH 繼承。 TPH 是 Entity Framework Core 支援的只有繼承模式。  您將會執行方式是建立`Person`類別中，變更`Instructor`和`Student`類別衍生自`Person`，將新的類別加入`DbContext`，和建立的移轉。

> [!TIP] 
> 請考慮下列變更之前儲存專案的複本。  然後如果您遇到的問題而需要從頭開始，會更輕鬆地回到整個序列的開頭啟動從儲存的專案，而不是反轉完成本教學課程的步驟或持續。

## <a name="create-the-person-class"></a>建立個人類別

在 [模型] 資料夾中，建立 Person.cs 並取代為下列程式碼中的範本程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>將 Student 與講師類別繼承自人員

在*Instructor.cs*、 講師類別衍生自人員類別和移除索引鍵和名稱的欄位。 程式碼看起來像下列的範例：

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

中的相同變更*Student.cs*。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>加入資料模型的人員實體類型

新增人員實體類型*SchoolContext.cs*。 新的線條會反白顯示。

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

這是所有 Entity Framework 必須以設定資料表每個階層繼承。 您會發現，更新資料庫時，它會有一個 Person 資料表，來取代 「 學生 」 和 「 講師資料表。

## <a name="create-and-customize-migration-code"></a>建立及自訂移轉程式碼

儲存您的變更，並建置專案。 然後在專案資料夾中開啟 [命令] 視窗並輸入下列命令：

```console
dotnet ef migrations add Inheritance
```

不會執行`database update`尚未命令。 該命令將會導致資料遺失，因為它將會卸除講師資料表，然後將學生資料表重新命名為 Person。 您要提供給自訂程式碼來保留現有的資料。

開啟*移轉 /\<時間戳記 > _Inheritance.cs*和取代`Up`方法取代下列程式碼：

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

此程式碼會負責下列資料庫更新工作：

* 移除外部索引鍵條件約束和學生資料表所指向的索引。

* 重新命名為 Person 講師資料表，然後儲存學生資料所需的變更：

* 加入可為 null 的 EnrollmentDate 學生版。

* 加入以指出資料列是一位學生或講師鑑別子資料行。

* 由於學生資料列將不會有雇用日期，使 HireDate 可為 null。

* 將暫存的欄位會用來更新指向學生的外部索引鍵。 當您將學生複製到 [人員] 資料表，它們會取得新的主索引鍵值。

* 從 Person 資料表學生資料表複製資料。 這會導致學生在指派新的主索引鍵值。

* 修正指向學生的外部索引鍵值。

* 重新建立 foreign key 條件約束和索引，立即將它們指向 Person 資料表。

（如果您使用 GUID 而不是整數做為主要索引鍵類型，學生的主索引鍵值不會有變更，並有幾個步驟可能已省略）。

執行`database update`命令：

```console
dotnet ef database update
```

(在生產系統中，您會使對應的變更`Down`萬一您曾經使用該項資訊來返回先前的資料庫版本的方法。 您將不會使用此教學課程`Down`方法。)

> [!NOTE] 
> 很可能在具有現有資料的資料庫進行結構描述變更時，取得其他錯誤。 如果您移轉時發生錯誤，您無法解決，您可以變更連接字串中的資料庫名稱，或刪除資料庫。 使用新資料庫時，若要移轉，沒有資料並更新資料庫指令更容易完成無誤。 若要刪除的資料庫，請使用 SSOX 或執行`database drop`CLI 命令。

## <a name="test-with-inheritance-implemented"></a>使用繼承實作測試

執行應用程式，然後再次嘗試各種不同的頁面。 運作一切正常相同與以前一樣。

在**SQL Server 物件總管**，展開**資料連線/SchoolContext**然後**資料表**，而且您會看到 「 學生 」 和 「 講師資料表有被取代Person 資料表。 開啟 Person 資料表設計工具，您會看到它具有所有 Student 與講師資料表中使用的資料行。

![Person 資料表中 SSOX](inheritance/_static/ssox-person-table.png)

以滑鼠右鍵按一下 [人員] 資料表，然後按一下**顯示資料表資料**看見鑑別子資料行。

![Person 資料表中 SSOX-資料表的資料](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>總結

您已實作的每個階層的資料表繼承`Person`， `Student`，和`Instructor`類別。 如需 Entity Framework 的核心中的繼承的詳細資訊，請參閱[繼承](https://docs.microsoft.com/ef/core/modeling/inheritance)。 在下一個教學課程中，您會看到如何處理各種不同的相對較進階的 Entity Framework 案例。

>[!div class="step-by-step"]
[上一頁](concurrency.md)
[下一頁](advanced.md)  
