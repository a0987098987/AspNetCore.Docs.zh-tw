---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: "使用 Entity Framework (VB) 建立模型類別 |Microsoft 文件"
author: microsoft
description: "在本教學課程中，您可以了解如何使用 ASP.NET MVC 與 Microsoft Entity Framework。 您了解如何使用實體精靈來建立 ADO.NET 實體資料..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: efc190d856fe9ebf1c09e0ae4758aabb1e3254dc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-model-classes-with-the-entity-framework-vb"></a>使用 Entity Framework (VB) 建立模型類別
====================
由[Microsoft](https://github.com/microsoft)

> 在本教學課程中，您可以了解如何使用 ASP.NET MVC 與 Microsoft Entity Framework。 您了解如何使用實體精靈來建立 ADO.NET 實體資料模型。 在本教學課程的過程中，我們會建置說明如何選取、 插入、 更新和刪除資料庫的資料使用 Entity Framework 的 web 應用程式。


本教學課程的目標是以說明如何建立建置 ASP.NET MVC 應用程式時使用 Microsoft Entity Framework 資料存取的類別。 本教學課程會假設 Microsoft Entity Framework 的任何先前的知識。 本教學課程結束時，您將了解如何使用 Entity Framework 來選取、 插入、 更新和刪除資料庫中的記錄。

Microsoft Entity Framework 是物件關聯式對應 (O/RM) 工具，可讓您從資料庫中自動產生的資料存取層。 Entity Framework 可讓您避免冗長工作以手動方式建立資料存取類別。

> [!NOTE] 
> 
> 沒有任何必要的連接之間 ASP.NET MVC 和 Microsoft Entity Framework。 有多個替代方案，您可以搭配 ASP.NET MVC 中使用 Entity framework。 例如，您可以建立使用其他的 O/RM 工具，例如 Microsoft LINQ to SQL、 NHibernate 或 SubSonic MVC 模型類別。


若要說明如何使用 Microsoft Entity Framework 搭配 ASP.NET MVC，我們會建置一個簡單的範例應用程式。 我們將建立電影資料庫應用程式可讓您顯示和編輯電影資料庫記錄。

本教學課程假設您有 Visual Studio 2008 或 Visual Web Developer 2008 Service Pack 1。 您需要 Service Pack 1，才能使用 Entity Framework。 您可以從下列網址下載 Visual Studio 2008 Service Pack 1 或 Service Pack 1 的 Visual Web Developer:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


## <a name="creating-the-movie-sample-database"></a>建立電影範例資料庫

影片資料庫應用程式會使用資料庫資料表名為電影，其中包含下列資料行：

| 資料行名稱 | 資料類型 | 允許 null 值嗎？ | 是主索引鍵嗎？ |
| --- | --- | --- | --- |
| ID | int | False | True |
| 標題 | Nvarchar （100) | False | False |
| 導向器 | Nvarchar （100) | False | False |

您可以加入至 ASP.NET MVC 專案的這個資料表，依照下列步驟：

1. 以滑鼠右鍵按一下應用程式\_Data 資料夾中的方案總管 視窗，然後選取功能表選項**新增、 新項目。**
2. 從**加入新項目**對話方塊中，選取**SQL Server 資料庫**，指定資料庫名稱 MoviesDB.mdf，再按一下**新增** 按鈕。
3. 按兩下要開啟 [伺服器總管/資料庫總管] 視窗的 MoviesDB.mdf 檔案。
4. 展開 MoviesDB.mdf 資料庫連接，以滑鼠右鍵按一下 [資料表] 資料夾中，並選取功能表選項**加入新的資料表**。
5. 在 資料表設計工具中，加入的識別碼、 標題和導向器的資料行。
6. 按一下**儲存**（具有磁碟的圖示） 的按鈕以儲存新的資料表名稱電影。

建立電影資料庫資料表之後，您應該將某些範例資料加入資料表。 以滑鼠右鍵按一下電影資料表，然後選取功能表選項**顯示資料表資料**。 您可以輸入假的電影資料到方格內出現。

## <a name="creating-the-adonet-entity-data-model"></a>建立 ADO.NET 實體資料模型

若要使用 Entity Framework，您要建立實體資料模型。 您可以利用 Visual Studio*實體資料模型精靈*從資料庫中自動產生實體資料模型。

請依照下列步驟：

1. 在 [方案總管] 視窗中的 [模型] 資料夾上按一下滑鼠右鍵，然後選取功能表選項**新增]、 [新項目**。
2. 在**加入新項目** 對話方塊中，選取的資料類別 （請參閱圖 1）。
3. 選取**ADO.NET 實體資料模型**範本，提供實體資料模型名稱 MoviesDBModel.edmx，然後按一下**新增** 按鈕。 按一下**新增**按鈕會啟動 [資料模型精靈]。
4. 在**選擇模型內容**步驟中，選擇 **從資料庫產生**選項，然後按一下**下一步**按鈕 （請參閱圖 2）。
5. 在**選擇資料連接**步驟選取 MoviesDB.mdf 資料庫連接，輸入的實體的連線設定命名 MoviesDBEntities，然後按一下**下一步**按鈕 （請參閱圖 3）。
6. 在**選擇您的資料庫物件**步驟選取影片資料庫資料表，按一下 **完成**按鈕 （請參閱圖 4）。

完成這些步驟後，便會開啟 ADO.NET Entity Data Model Designer （實體設計工具）。

**圖 1 – 建立新的實體資料模型**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**圖 2 – 選擇模型內容的步驟**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**圖 3 – 選擇資料連線**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**圖 4 – 選擇您的資料庫物件**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>修改 ADO.NET 實體資料模型

建立實體資料模型之後，您可以修改模型利用實體設計工具 （請參閱圖 5）。 您可以隨時開啟 Entity Designer 中，按兩下 [方案總管] 視窗內的 [模型] 資料夾中包含的 MoviesDBModel.edmx 檔案。

**圖 5 – ADO.NET 實體資料模型設計工具**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

例如，您可以使用 Entity Designer 中變更的實體模型資料精靈產生的類別名稱。 此精靈會建立新的資料存取類別，名為電影。 換句話說，精靈會讓類別非常同名的資料庫資料表。 因為我們將使用這個類別來代表特定的影片執行個體，我們應該重新命名電影影片的類別。

如果您想要重新命名為實體類別，您可以按兩下 Entity Designer 中的類別名稱，並輸入新名稱 （請參閱圖 6）。 或者，您可以在 Entity Designer 中選取實體之後變更 [屬性] 視窗中的實體的名稱。

**圖 6 – 變更實體名稱**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

請記得儲存您的實體資料模型之後所做的修改，依序按一下 [儲存] 按鈕 （磁片圖示）。 在幕後，實體設計工具會產生一組 Visual Basic.NET 類別。 您可以從 [方案總管] 視窗中開啟 MoviesDBModel.Designer.vb 檔案，以檢視這些類別。


不要修改.designer.vb 檔中的程式碼，因為您的變更將覆寫您使用 Entity Designer 中的下一次。 如果您想要擴充.designer.vb 檔中定義的實體類別的功能，則您可以建立*部分類別*在不同的檔案。


#### <a name="selecting-database-records-with-the-entity-framework"></a>選取與 Entity Framework 資料庫資料錄

讓我們開始建置影片資料庫應用程式藉由建立顯示一份影片資料錄的網頁。 Home 控制器中列出的 1 會公開名為 index （） 的動作。 Index 動作傳回的電影記錄所有電影資料庫資料表中利用 Entity Framework。

**列出 1 – Controllers\HomeController.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

請注意程式碼範例 1 中的控制站包含建構函式。 建構函式會初始化名為的類別層級欄位\_db。 \_Db 欄位代表由 Microsoft Entity Framework 產生資料庫實體。 \_Db 欄位是由實體設計工具產生 MoviesDBEntities 類別的執行個體。

\_Db 欄位 index 動作中用來從電影資料庫資料表中擷取的記錄。 運算式\_db。MovieSet 會代表所有記錄的電影資料庫資料表中。 Tolist （） 方法用來將電影的集合轉換成影片物件的泛型集合： 清單 （的影片）。

搭配 LINQ to Entities 擷取影片記錄。 Index （） 中的動作清單 1 使用 LINQ*方法語法*擷取的資料庫中的記錄組。 如果您想要的話，您可以使用 LINQ*查詢語法*改為。 下列兩個陳述式會執行完全相同的動作：

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

使用您認為最具直覺性無論 LINQ 語法 （方法語法或查詢語法）。 沒有任何差異，在兩種方法的效能而言唯一的差別樣式。

列出 2 中的檢視用來顯示影片資料錄。

**列出 2 – Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

列出 2 中的檢視包含**每個**迴圈，逐一查看每個影片記錄，並顯示電影錄製的標題和導向器屬性的值。 請注意，編輯和刪除的連結會顯示每一筆記錄旁。 此外，新增影片連結會出現在檢視的底端 （請參閱圖 7）。

**圖 7 – 索引檢視**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

索引檢視是*型別檢視*。 索引檢視表有&lt;%@ 頁面 %&gt;包含 Inherits 屬性的指示詞。 Inherits 屬性會轉換為強型別清單的泛型集合影片物件 – 清單 （的影片） ViewData.Model 屬性。

## <a name="inserting-database-records-with-the-entity-framework"></a>插入與 Entity Framework 資料庫記錄

您可以使用 Entity Framework，以便於新記錄插入資料庫資料表。 列出 3 包含兩個新的動作加入至主控制器類別可讓您將新記錄插入至電影資料庫資料表。

**列出 3 – Controllers\HomeController.vb （新增方法）**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

第一個 add （） 動作只會傳回檢視表。 此檢視包含來加入新的電影資料庫表單 （請參閱圖 8） 的記錄。 當您提交表單時，會叫用第二個 add （） 動作。

請注意第二個 add （） 動作使用 AcceptVerbs 屬性裝飾。 只有在執行 HTTP POST 作業時，可以叫用這個動作。 換句話說，此動作可以只叫用時張貼 HTML 表單。

第二個 add （） 動作會建立 ASP.NET MVC TryUpdateModel() 方法搭配 Entity Framework 影片類別的新執行個體。 TryUpdateModel() 方法會採用傳遞至 add （） 方法 FormCollection 中的欄位，並將這些 HTML 表單欄位的值指派給影片類別。


使用 Entity Framework 時，您必須提供屬性的 「 技術清單 」 TryUpdateModel 或 UpdateModel 方法來更新實體類別的屬性時。


接下來，add （） 動作會執行一些簡單的表單驗證。 動作可確認標題和導向器的屬性有值。 如果有驗證錯誤，驗證錯誤訊息將加入 ModelState。

如果沒有任何驗證錯誤新的電影錄製將加入電影資料庫資料表，Entity Framework 的協助。 新的記錄加入到具有下列兩行程式碼的資料庫：

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

第一行程式碼會將新的電影實體加入至電影由 Entity Framework 所追蹤的集合。 程式碼的第二行儲存的任何變更已對基礎資料庫正在追蹤的電影。

**圖 8 – 新增檢視**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>更新資料庫的記錄與 Entity Framework

您可以依照幾乎相同的方法來編輯與 Entity Framework 資料庫記錄為我們剛才後面插入新的資料庫記錄的方法。 列出 4 包含兩個名為 Edit() 的新控制器動作。 第一個 Edit() 動作將會傳回編輯電影錄製之 HTML 表單。 第二個 Edit() 動作會嘗試更新資料庫。

**列出 4 – Controllers\HomeController.vb （編輯方法）**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

第二個 Edit() 動作一開始會從符合正在編輯的電影識別碼的資料庫擷取電影錄製。 下列 LINQ to Entities 陳述式會抓取符合特定識別碼的第一個資料庫記錄：

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

接下來，TryUpdateModel() 方法用來將 HTML 表單欄位的值指派給影片實體的屬性。 請注意的白名單提供給指定要更新的確切屬性。

接下來，會執行一些簡單的驗證，來確認電影標題和導向器的屬性有值。 如果任一個屬性遺漏值，然後驗證錯誤訊息加入 ModelState 並 ModelState.IsValid 傳回其值為 false。

最後，如果有任何驗證錯誤，則基礎電影資料庫資料表已更新的任何變更呼叫 savechanges （） 方法。

當編輯資料庫記錄時，您需要傳遞至控制器的動作執行資料庫更新正在編輯的記錄的識別碼。 否則，控制器動作將不會知道哪一個要在基礎資料庫中更新記錄。 編輯檢視中，包含在列出 5 中，包括隱藏的表單欄位，表示正在編輯的資料庫記錄的識別碼。

**列出 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>刪除與 Entity Framework 資料庫資料錄

最終的資料庫作業，我們需要將解決本教學課程中，正在刪除資料庫中的記錄。 您可以使用程式碼範例 6 中的控制器動作刪除特定資料庫記錄。

**列出 6-\Controllers\HomeController.vb （Delete 動作）**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

Delete 動作會先擷取符合識別碼的實體傳遞給動作的電影。 接下來，電影會從資料庫刪除藉由呼叫 DeleteObject() 方法接著 savechanges （） 方法。 最後，使用者重新導向回索引檢視。

## <a name="summary"></a>總結

本教學課程的用途是示範如何藉由運用 ASP.NET MVC 和 Microsoft Entity Framework 建置資料庫驅動的 web 應用程式。 您已了解如何建置應用程式，可讓您選取、 插入、 更新和刪除資料庫中的記錄。

首先，我們將討論如何使用實體資料模型精靈來產生實體資料模型從 Visual Studio 中。 接下來，您會了解如何使用 LINQ to Entities 從資料庫資料表擷取一組資料庫記錄。 最後，我們會使用 Entity Framework 來插入、 更新和刪除資料庫的記錄。

>[!div class="step-by-step"]
[上一頁](validation-with-the-data-annotation-validators-cs.md)
[下一頁](creating-model-classes-with-linq-to-sql-vb.md)
