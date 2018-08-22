---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: 使用 Entity Framework (VB) 建立模型類別 |Microsoft Docs
author: microsoft
description: 在本教學課程中，您將了解如何使用 Microsoft Entity Framework 的 ASP.NET MVC。 您了解如何使用實體精靈來建立 ADO.NET 實體資料...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: c1f64f57d4c23fe225a8268042104254e17dc456
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830107"
---
<a name="creating-model-classes-with-the-entity-framework-vb"></a>使用 Entity Framework (VB) 建立模型類別
====================
by [Microsoft](https://github.com/microsoft)

> 在本教學課程中，您將了解如何使用 Microsoft Entity Framework 的 ASP.NET MVC。 您了解如何使用實體精靈來建立 ADO.NET 實體資料模型。 在本教學課程的過程中，我們會建置說明如何選取、 插入、 更新和刪除資料庫的資料使用 Entity Framework 的 web 應用程式。


本教學課程的目標是要說明如何建立建置 ASP.NET MVC 應用程式時，使用 Microsoft Entity Framework 資料存取類別。 本教學課程會假設沒有事先了解 Microsoft Entity Framework。 本教學課程結束時，您將了解如何使用 Entity Framework 來選取、 插入、 更新和刪除資料庫中的記錄。

Microsoft Entity Framework 是物件關聯式對應 (O/RM) 的工具，可讓您從資料庫中自動產生資料存取層。 Entity Framework 可讓您不必以手動方式建立您的資料存取類別的繁瑣。

> [!NOTE] 
> 
> 沒有必要連線 Microsoft Entity Framework 與 ASP.NET MVC。 有多個替代方案，您可以使用 ASP.NET MVC 與 Entity framework。 例如，您可以建置您使用其他的 O/RM 工具，例如 Microsoft LINQ to SQL、 NHibernate 或 SubSonic 的 MVC 模型類別。


若要說明如何使用 Microsoft Entity Framework 搭配 ASP.NET MVC 中，我們將建置一個簡單的範例應用程式。 我們將建立影片資料庫應用程式，可讓您顯示和編輯電影資料庫記錄。

本教學課程假設您有 Visual Studio 2008 或 Visual Web Developer 2008 Service Pack 1。 需要 Service Pack 1 時，您才能使用 Entity Framework。 您可以從下列網址下載 Visual Studio 2008 Service Pack 1 或 Service Pack 1 的 Visual Web Developer:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


## <a name="creating-the-movie-sample-database"></a>建立電影範例資料庫

影片資料庫應用程式會使用包含下列資料行的資料庫資料表名為電影：

| 資料行名稱 | 資料類型 | 允許 null 值嗎？ | 是主索引鍵嗎？ |
| --- | --- | --- | --- |
| ID | int | False | True |
| 標題 | Nvarchar(100) | False | False |
| 總監 | Nvarchar(100) | False | False |

您可以加入至 ASP.NET MVC 專案的這個資料表，依照下列步驟：

1. 以滑鼠右鍵按一下 [應用程式\_Data 資料夾中的方案總管] 視窗，然後選取功能表選項**新增]、 [新項目。**
2. 從**加入新項目**對話方塊中，選取**SQL Server 資料庫**，提供該資料庫名稱 MoviesDB.mdf，然後按一下**新增** 按鈕。
3. 按兩下以開啟 [伺服器總管/資料庫總管] 視窗的 MoviesDB.mdf 檔案。
4. 展開 MoviesDB.mdf 資料庫連接，以滑鼠右鍵按一下 [資料表] 資料夾，然後選取功能表選項**加入新的資料表**。
5. 在 資料表設計工具中，新增識別碼、 標題和經理的資料行。
6. 按一下 **儲存**（具有的磁片圖示） 按鈕以儲存新的資料表名稱電影。

您建立的電影資料庫資料表之後，您應該一些範例資料加入資料表。 電影資料表上按一下滑鼠右鍵，然後選取功能表選項**顯示資料表資料**。 您可以將假的電影資料輸入出現的方格。

## <a name="creating-the-adonet-entity-data-model"></a>建立 ADO.NET 實體資料模型

若要使用 Entity Framework，您需要建立實體資料模型。 您可以利用 Visual Studio *Entity Data Model 精靈*自動從資料庫產生 Entity Data Model。

請依照下列步驟：

1. 在 [方案總管] 視窗中的 [模型] 資料夾上按一下滑鼠右鍵，然後選取功能表選項**新增]、 [新項目**。
2. 在 **加入新項目** 對話方塊中，選取 資料類別 （請參閱 圖 1）。
3. 選取 [ **ADO.NET 實體資料模型**範本，將實體資料模型命名 MoviesDBModel.edmx，然後按一下**新增**] 按鈕。 按一下 **新增**按鈕會啟動 資料模型精靈。
4. 在 **選擇模型內容**步驟中，選擇**從資料庫產生**選項，然後按一下**下一步**按鈕 （請參閱 圖 2）。
5. 在 **選擇資料連接**步驟中，選取 MoviesDB.mdf 資料庫連接，輸入的實體連接設定名稱 MoviesDBEntities，然後按一下 **下一步**按鈕 （請參閱 圖 3）。
6. 在 **選擇您的資料庫物件**步驟中，選取的電影資料庫資料表，然後按一下**完成**按鈕 （請參閱 圖 4）。

完成這些步驟之後，會開啟 ADO.NET 實體資料模型設計工具 (Entity Designer)。

**圖 1 – 建立新的實體資料模型**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**圖 2 – 選擇模型內容的步驟**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**圖 3 – 選擇您的資料連接**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**圖 4 – 選擇您的資料庫物件**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>修改 ADO.NET 實體資料模型

建立實體資料模型之後，您可以利用 Entity Designer 修改模型 （請參閱 [圖 5]）。 您可以隨時開啟實體設計工具，按兩下 [方案總管] 視窗中的 [Models] 資料夾中包含的 MoviesDBModel.edmx 檔案。

**圖 5 – ADO.NET 實體資料模型設計工具**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

比方說，您可以使用 Entity Designer 來變更 [實體模型資料精靈] 會產生類別的名稱。 精靈會建立新的資料存取類別，名為電影。 換句話說，精靈會指定類別完全相同名稱做為資料庫資料表。 因為我們將使用這個類別來代表特定的影片執行個體，我們應該重新命名電影到電影的類別。

如果您想要重新命名的實體類別，您可以在 Entity Designer 中的類別名稱上按兩下，並輸入新名稱 （請參閱 圖 6）。 或者，您可以在 Entity Designer 中選取實體之後，變更 [屬性] 視窗中的實體的名稱。

**圖 6 – 變更實體名稱**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

請記得儲存您的實體資料模型後所做的修改，依序按一下 [儲存] 按鈕 （磁片圖示）。 在幕後，Entity Designer 會產生一組 Visual Basic.NET 類別。 您可以從 [方案總管] 視窗中開啟 MoviesDBModel.Designer.vb 檔案，以檢視這些類別。


請勿修改.designer.vb 檔案中的程式碼，因為您的變更將覆寫您使用 Entity Designer 中的下一次。 如果您想要擴充的.designer.vb 檔案中定義的實體類別的功能，則您可以建立*部分類別*個別檔案中。


#### <a name="selecting-database-records-with-the-entity-framework"></a>選取 使用 Entity Framework 資料庫記錄

讓我們開始建立影片資料庫應用程式建立頁面，其中顯示電影資料錄的清單。 在 列表 1 中的主控制器會公開名為 index （） 的動作。 Index （） 動作會傳回所有電影資料錄的電影資料庫資料表中，利用 Entity Framework。

**列表 1 – Controllers\HomeController.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

請注意程式碼範例 1 中的控制器包含建構函式。 建構函式會初始化名為的類別層級欄位\_db。 \_Db 欄位代表 Microsoft Entity Framework 所產生的資料庫實體。 \_Db 欄位是實體設計工具所產生的 MoviesDBEntities 類別的執行個體。

\_Db 欄位來 index （） 動作中的電影資料庫資料表中擷取的記錄。 運算式\_db。MovieSet 會代表所有記錄的電影資料庫資料表中。 Tolist 方法用來將電影的集合轉換成電影物件的泛型集合： 清單 （的影片）。

影片資料錄會擷取與 LINQ to Entities 的協助。 在 列表 1 中的 index （） 動作會使用 LINQ*方法的語法*擷取的一組資料庫記錄。 如果您想，您可以使用 LINQ*的查詢語法*改。 下列兩個陳述式會執行相同的動作：

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

使用任何 LINQ 語法 – 方法語法或查詢語法 – 您找到最直覺的。 沒有任何差異，兩種方法之間的效能，唯一的差別在於樣式。

列表 2 中的檢視用來顯示電影資料錄。

**列表 2 – Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

列表 2 中的檢視包含**針對每個**迴圈，逐一查看影片的每筆記錄，並顯示電影資料錄的標題和主管屬性的值。 請注意，編輯和刪除的連結會顯示每一筆記錄旁。 此外，新增的影片連結會出現在底部的檢視 （請參閱 圖 7）。

**圖 7 – 索引 檢視**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

[索引] 檢視會*型別檢視*。 索引檢視表有&lt;%@ %頁&gt;包含 Inherits 屬性指示詞。 Inherits 屬性會轉換成強型別泛型清單集合電影物件 – 的清單 （的影片） ViewData.Model 屬性。

## <a name="inserting-database-records-with-the-entity-framework"></a>使用 Entity Framework 的插入資料庫記錄

您可以使用 Entity Framework，讓您輕鬆地將新記錄插入至資料庫資料表。 列表 3 包含兩個新的動作新增至首頁控制器類別可供您的電影資料庫資料表中插入新記錄。

**列表 3 – Controllers\HomeController.vb （Add 方法）**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

第一個 add （） 動作只會傳回檢視。 此檢視包含的表單中加入新的電影資料庫記錄 （請參閱 圖 8）。 當您提交表單時，會叫用第二個 add （） 動作。

請注意第二個 add （） 動作使用 AcceptVerbs 屬性裝飾。 只有在執行 HTTP POST 作業時，可以叫用此動作。 換句話說，此動作可以只叫用時張貼 HTML 表單。

第二個 add （） 動作會建立 Entity Framework 電影類別的新執行個體的 ASP.NET MVC TryUpdateModel() 方法的協助。 TryUpdateModel() 方法會傳遞至 neobsahuje metodu FormCollection 中的欄位，並將這些 HTML 表單欄位的值指派至電影類別。


使用 Entity Framework 時，您必須在 TryUpdateModel 或 UpdateModel 方法來更新實體類別的屬性時，提供 「 技術清單 」 的屬性。


接下來，add （） 動作會執行一些簡單的表單驗證。 動作可確認將 標題 與 導演屬性有值。 如果有驗證錯誤，驗證錯誤訊息會加入 ModelState。

如果不有任何驗證錯誤的 Entity Framework 協助電影資料庫的資料表已新增新的電影錄製。 使用下列兩行程式碼的資料庫加入新的記錄：

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

第一行程式碼會將新的電影實體加入至由 Entity Framework 所追蹤的電影的集合。 第二行程式碼會將儲存的任何變更，已對追蹤回基礎資料庫的電影。

**圖 8-新增檢視**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>使用 Entity Framework 更新資料庫中的記錄

您可以依照幾乎相同的方法只是要插入新的資料庫記錄所遵循的方法與編輯與 Entity Framework 的資料庫記錄。 列表 4 包含兩個名為 edit （） 的新控制器動作。 第一個 edit （） 動作會傳回編輯電影錄製之 HTML 表單。 第二個 edit （） 動作會嘗試更新資料庫。

**列表 4 – Controllers\HomeController.vb （編輯方法）**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

第二個 edit （） 動作首先會從符合正在編輯的電影識別碼的資料庫擷取電影錄製。 下列 LINQ to Entities 陳述式會抓取符合特定識別碼的第一個資料庫記錄：

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

接下來，TryUpdateModel() 方法用來將 HTML 表單欄位的值指派給電影實體的屬性。 請注意的允許清單提供給指定要更新的確切屬性。

接下來，會執行一些簡單的驗證，來確認電影標題和主管的屬性有值。 如果其中一個屬性遺漏值，然後 ModelState 中新增驗證錯誤訊息，並 ModelState.IsValid 會傳回 false 值。

最後，如果不有任何驗證錯誤，則基礎的電影資料庫資料表被更新的任何變更藉由呼叫 savechanges （） 方法。

當您編輯資料庫中的記錄，您需要傳遞至控制器動作，以執行資料庫更新正在編輯的記錄的識別碼。 否則，控制器動作不會知道哪一筆記錄更新基礎資料庫中。 編輯檢視中，包含在列表 5 中，包括隱藏的表單欄位，表示正在編輯的資料庫記錄的識別碼。

**列表 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>刪除資料庫記錄，使用 Entity Framework

最後的資料庫作業，我們需要將在本教學課程中處理，正在刪除資料庫中的記錄。 您可以使用列表 6 中的控制器動作，若要刪除特定資料庫記錄。

**列表 6-\Controllers\HomeController.vb （[刪除] 動作）**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

Delete （） 動作會先擷取符合識別碼的實體傳遞給動作的影片。 接下來，影片會從資料庫刪除藉由呼叫 DeleteObject() 方法，後面接著 savechanges （） 方法。 最後，使用者重新導向回到 [索引] 檢視。

## <a name="summary"></a>總結

本教學課程的目的是要示範如何利用 ASP.NET MVC 和 Microsoft Entity Framework 可以建置資料庫導向的 web 應用程式。 您已了解如何建置應用程式，可讓您選取、 插入、 更新和刪除資料庫記錄。

首先，我們會討論如何使用 Entity Data Model 精靈產生 Entity Data Model 從 Visual Studio 內。 接下來，您會了解如何使用 LINQ to Entities 來擷取資料庫資料表中的一組資料庫記錄。 最後，我們會使用 Entity Framework 來插入、 更新和刪除資料庫中的記錄。

> [!div class="step-by-step"]
> [上一頁](validation-with-the-data-annotation-validators-cs.md)
> [下一頁](creating-model-classes-with-linq-to-sql-vb.md)
