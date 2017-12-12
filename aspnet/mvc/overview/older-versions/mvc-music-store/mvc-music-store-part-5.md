---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: "第 5 部分： 編輯表單和範本 |Microsoft 文件"
author: jongalloway
description: "此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 5 部分涵蓋編輯表單和範本。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: cde6fe133291254531a797a434a4b2cdd226dd5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="part-5-edit-forms-and-templating"></a>第 5 部分： 編輯表單和範本
====================
由[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。  
>   
> MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。
> 
> 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 5 部分涵蓋編輯表單和範本。


在過去的章節中，我們已從資料庫載入資料，並顯示它。 在本章中，我們也會啟用編輯的資料。

## <a name="creating-the-storemanagercontroller"></a>建立 StoreManagerController

我們一開始會藉由建立新的控制器呼叫**StoreManagerController**。 此控制站，我們將會利用 Scaffolding 可用的功能在 ASP.NET MVC 3 Tools Update。 設定 [加入控制器] 對話方塊的選項，如下所示。

![](mvc-music-store-part-5/_static/image1.png)

當您按一下 [新增] 按鈕時，您會看到 ASP.NET MVC 3 scaffolding 機制會為您工作的理想數量：

- 它會建立新的 StoreManagerController 與 Entity Framework 的區域變數
- 它將 StoreManager 資料夾加入專案的 [檢視] 資料夾
- 它會新增 Create.cshtml、 Delete.cshtml、 Details.cshtml、 Edit.cshtml 和 Index.cshtml 檢視中，強類型專輯類別

![](mvc-music-store-part-5/_static/image2.png)

新的 StoreManager 控制器類別包含 CRUD （建立、 讀取、 更新、 刪除） 知道如何處理與專輯控制器動作模型類別，並使用我們的 Entity Framework 內容的資料庫存取權。

## <a name="modifying-a-scaffolded-view"></a>修改 Scaffold 的檢視

請務必記得，因為我們已產生這段程式碼，它是標準 ASP.NET MVC 的程式碼，如同我們已經撰寫本教學課程。 它已經用來儲存您要花費同樣的時間寫入未定案控制器程式碼，並以手動方式建立強型別的檢視，但這不是產生的程式碼可能會看到您做為您絕不能如何變更的相關註解中的這一類警告開頭的種類程式碼。 這是您的程式碼，而且您打算將它變更。

因此，讓我們快速編輯 以 StoreManager 索引檢視的開頭 (/ Views/StoreManager/Index.cshtml)。 此檢視會顯示資料表會列出在我們的存放區中編輯專輯 / 詳細資料 / 刪除的連結，並包含專輯的公用屬性。 我們會將移除 AlbumArtUrl 欄位中，因為它不在此顯示中非常有用。 在&lt;資料表&gt;區段的 檢視程式碼中，移除&lt;第&gt;和&lt;td&gt;周圍 AlbumArtUrl 參考下列反白顯示的程式行所示的項目：

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

修改過的檢視程式碼會出現，如下所示：

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>初探存放區管理員

現在執行應用程式，並瀏覽至/StoreManager /。 這會顯示我們未修改過，顯示一份專輯連結來編輯詳細資料，以及刪除存放區中儲存 Manager 索引。

![](mvc-music-store-part-5/_static/image3.png)

按一下 [編輯] 連結顯示的專輯、 音樂類型與演出者包括下拉式清單的編輯表單欄位。

![](mvc-music-store-part-5/_static/image4.png)

按一下底部的 「 回至清單 」 連結，然後按一下專輯的詳細資訊連結。 這會顯示個別的專輯的詳細資訊。

![](mvc-music-store-part-5/_static/image5.png)

同樣地，按一下 [清單] 連結回到，然後按一下 [刪除] 連結。 這會顯示確認對話方塊中，顯示專輯詳細資料，並詢問我們是否確定要刪除它。

![](mvc-music-store-part-5/_static/image6.png)

按一下底部的 [刪除] 按鈕會刪除專輯和您返回索引頁面，其中顯示刪除的專輯。

我們無法完成與存放區管理員，但我們有使用控制器和檢視 CRUD 作業，從啟動的程式碼。

## <a name="looking-at-the-store-manager-controller-code"></a>從存放區管理員控制器程式碼

存放區管理員控制器包含理想的程式碼數量。 我們再逐行這由上而下。 控制器包含一些標準的命名空間，MVC 控制器，以及我們的模型命名空間的參考。 控制器有 MusicStoreEntities，資料存取的每個控制器動作使用的私用執行個體。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>存放管理員索引和詳細資料的動作

索引檢視擷取專輯，包括每個相簿參考內容類型與演出者資訊，我們之前看到時使用的存放區瀏覽方法的清單。 索引檢視，讓它讓控制器正在有效率且查詢的原始要求中的這項資訊可以顯示每個相簿的內容類型名稱和演出者名稱遵循連結物件的參考。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

StoreManager 控制站的詳細資料控制器動作的運作方式完全相同的存放區控制站的詳細資料動作，我們先前-撰寫查詢專輯依識別碼使用 Find() 方法，再將它傳至檢視。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>建立動作方法

建立動作方法是從 1 到目前為止，我們看到稍有不同，因為它們處理表單的輸入。 當使用者第一次造訪時 /StoreManager/建立/它們將會顯示一個空白表單。 這個 HTML 網頁將會包含&lt;表單&gt;項目，其中包含下拉式清單和文字方塊中輸入項目可以在其中輸入專輯的詳細資料。

使用者填入專輯表單值之後，可按 [儲存] 按鈕回到我們的應用程式儲存在資料庫中的這些變更送出。 當使用者按下 [儲存] 按鈕&lt;表單&gt;會執行 HTTP POST 回到 /StoreManager/建立/URL，並提交&lt;表單&gt;值做為 HTTP post 要求的一部分。

ASP.NET MVC 可讓我們來輕鬆地分割的這兩個 URL 引動過程案例邏輯藉由啟用我們實作 StoreManagerController 類別 – 其中一個來處理初始 HTTP GET 瀏覽至 /StoreManager/Create 內兩個個別 「 建立 」 動作方法/ URL 和其他以處理 HTTP POST 的提交的變更。

### <a name="passing-information-to-a-view-using-viewbag"></a>將資訊傳遞至檢視，使用 ViewBag

我們稍早在本教學課程中使用 ViewBag，但您尚未討論許多資訊，請參閱。 ViewBag 可讓我們將資訊傳遞至檢視，而不需要使用強型別的模型物件。 在此情況下，需要這兩種內容類型和演出者名稱清單傳遞至表單，以填入下拉式清單中，我們編輯 HTTP-GET 控制器動作，並傳回它們作為 ViewBag 項目是最簡單的方式執行此作業。

ViewBag 是動態的物件，這表示，您可以輸入 ViewBag.Foo 或 ViewBag.YourNameHere 而不需要撰寫程式碼來定義這些屬性。 在此情況下，控制器會使用 ViewBag.GenreId 和 ViewBag.ArtistId，因此會 GenreId 與 ArtistId，它們將設定的專輯屬性的下拉式清單中值的表單提交。

使用 SelectList 物件、 建立只針對該目的表單會傳回這些下拉式清單中的值。 這是類似的程式碼：

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

您可以看到從動作方法的程式碼，建立此物件正在使用三個參數：

- 將會顯示下拉式清單中的項目清單。 請注意，這不只是字串的-我們所傳遞的內容類型清單。
- 下一個參數傳遞至 SelectList 為選取值。 此 SelectList 如何知道如何預先選取清單中的項目。 這會是更易於了解當我們查看的編輯表單，這可能會很類似。
- 最後一個參數是要顯示的屬性。 在此情況下，這代表 Genre.Name 屬性會將顯示給使用者。

這一點之後，HTTP GET 建立動作是很簡單-ViewBag，會加入兩個 SelectLists 然後沒有模型物件會傳遞至表單，（因為它尚未尚未建立）。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>顯示卸除的清單中建立檢視的 HTML Helper

因為我們談論的下拉式清單值傳遞至檢視的方式，讓我們快速查看檢視來查看這些值的顯示方式。 在 檢視程式碼 (/ Views/StoreManager/Create.cshtml)，您會看到顯示的內容類型的卸除進行下列呼叫向下。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

這就是所謂的 HTML Helper-公用程式方法，而這個方法會執行的一般檢視工作。 HTML Helper 會很有用的精簡、 可讀性，讓我們檢視程式碼。 Html.DropDownList helper 係由 ASP.NET MVC 中，但我們稍後就會看到很可能建立自己的 helper 檢視程式碼，我們將在我們的應用程式中重複使用。

Html.DropDownList 呼叫只要被告知兩件事-其中 get 清單以顯示，以及哪些值 （如果有的話） 應該是預先選取。 第一個參數 GenreId，告知 DropDownList 以尋找名 GenreId 為模型或 ViewBag 中的值。 第二個參數用來指出要顯示的值為初始下拉式清單中選取。 這種形式是建立表單，因為沒有要預先選取的值，並傳遞 String.Empty。

### <a name="handling-the-posted-form-values"></a>處理張貼的表單值

如同我們討論之前，有兩個與每個表單相關聯的動作方法。 第一個會處理 HTTP GET 要求，並顯示表單。 第二個處理的 HTTP POST 要求，其中包含送出的表單值。 請注意，控制器動作會有 [HttpPost] 屬性會告知 ASP.NET MVC 只應該回應 HTTP POST 要求。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

這個動作具有四個責任：

- 1. 讀取格式值
- 2. 檢查表單值是否傳遞的任何驗證規則
- 3. 如果表單提交有效時，將資料儲存和顯示更新的清單
- 4. 如果表單提交不是有效的重新顯示發生驗證錯誤表單

#### <a name="reading-form-values-with-model-binding"></a>使用模型繫結的讀取格式值

控制器動作正在處理的標題、 價格和 AlbumArtUrl 包含 GenreId 和值 ArtistId （從下拉式清單） 和文字方塊值的表單提交。 雖然可以直接存取表單值、 更好的方法是使用內建於 ASP.NET MVC 模型繫結功能。 當控制器動作，會採用類型做為參數的模型時，ASP.NET MVC 會嘗試將填入表單輸入 （以及路由和查詢字串值） 使用該類型的物件。 其做法是要尋找的名稱符合物件的內容模型，例如設定新的專輯物件 GenreId 值時，它會尋找名稱 GenreId 輸入。 當您建立檢視，在 ASP.NET MVC 中使用標準方法時，將一律使用屬性名稱做為輸入的欄位名稱，所以此欄位名稱會只符合會呈現表單。

#### <a name="validating-the-model"></a>驗證模型

簡單 ModelState.IsValid 呼叫驗證模型。 我們還沒有加入任何驗證規則，我們專輯類別，但-我們將會執行位元-所以現在這項檢查不是多執行。 重要的是此 ModelStat.IsValid 檢查將會調整我們打我們的模型，讓未來的變更，驗證規則不需要任何更新控制器動作的程式碼的驗證規則。

#### <a name="saving-the-submitted-values"></a>正在儲存送出的值

如果表單提交通過驗證，則將值儲存到資料庫的時間。 使用 Entity Framework 中，只需要將模型加入到專輯集合，然後呼叫 SaveChanges。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework 產生適當的 SQL 命令，以保存值。 將資料儲存之後, 我們將重新導向回相簿，我們會看到我們更新。 這是藉由傳回 RedirectToAction 控制器動作，我們想要顯示的名稱。 在此情況下，這是索引方法。

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>顯示無效的表單提交發生驗證錯誤

在無效的格式輸入的情況下的下拉式清單中的值會加入至 ViewBag （如同 HTTP GET 案例中） 且繫結的模型值已傳送回檢視顯示。 驗證錯誤會自動顯示使用@Html.ValidationMessageForHTML Helper。

#### <a name="testing-the-create-form"></a>測試建立表單

若要測試這種情況時執行的應用程式和瀏覽至 /StoreManager/建立 /-，這會顯示空白表單 StoreController 建立 HTTP GET 方法所傳回。

填滿一些值，然後按一下 送出表單的 建立 按鈕。

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>處理編輯

（HTTP GET 和 HTTP POST） 的編輯動作組是非常類似於剛剛建立動作方法。 因為編輯案例牽涉到使用現有的相簿，編輯 HTTP-GET 方法會根據 「 識別碼 」 參數的相簿，載入透過路由傳遞中。 因為我們已經先前討論過，詳細資料控制器動作中擷取的 AlbumId 相簿的這段程式碼都是相同的。 使用 建立 / HTTP GET 方法，透過 ViewBag 傳回下拉式值清單。 這可讓我們回到相簿為我們的模型物件的檢視 （這強型別專輯類別） 並透過 ViewBag 傳遞其他資料 （例如內容類型清單）。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

編輯 HTTP POST 動作是非常類似於建立 HTTP POST 動作。 唯一的差別，而非資料庫中新增新的相簿。專輯集合中，我們發現專輯的目前執行個體使用 db。Entry(album) 並將其狀態設定為已修改。 這告訴我們要修改現有的相簿，而非建立新的 Entity Framework。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

我們可以執行應用程式，並瀏覽至/StoreManger/，然後按一下專輯的編輯連結此進行測試。

![](mvc-music-store-part-5/_static/image9.png)

這會顯示編輯 HTTP GET 方法所顯示的編輯表單。 填滿一些值，然後按一下 [儲存] 按鈕。

![](mvc-music-store-part-5/_static/image10.png)

這將表單、 儲存值，並傳回我們相簿清單中，顯示已更新的值。

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>處理刪除

刪除遵循相同的模式，以編輯和建立、 使用一個控制器動作，以顯示 [確認] 表單中和另一個控制器動作，以處理表單送出。

HTTP GET 刪除控制器動作完全是我們先前存放區管理員詳細資料控制器動作相同。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

我們使用 刪除檢視的內容樣板專輯類型顯示表單的強型別。

![](mvc-music-store-part-5/_static/image12.png)

刪除範本會顯示模型的所有欄位，但我們可以稍微簡化該清單。 變更 /Views/StoreManager/Delete.cshtml 中的檢視程式碼所示。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

這會顯示確認簡化的刪除。

![](mvc-music-store-part-5/_static/image13.png)

按一下 [刪除] 按鈕會導致表單回傳至伺服器，它會執行 DeleteConfirmed 動作。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

我們 HTTP POST 刪除控制器動作會採取下列動作：

- 1. 載入的專輯依識別碼
- 2. 專輯將它刪除，並儲存變更
- 3. 重新導向至索引中，顯示已從清單中移除相簿

若要測試這種情況，執行應用程式並瀏覽至 /StoreManager。 從清單中選取相簿，然後按一下 [刪除] 連結。

![](mvc-music-store-part-5/_static/image14.png)

這會顯示我們刪除確認 畫面中。

![](mvc-music-store-part-5/_static/image15.png)

按一下 [刪除] 按鈕時，移除專輯，並傳回我們至存放區管理員索引頁中，顯示已刪除的專輯。

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>使用自訂的 HTML Helper 來截斷文字

我們有一個可能的問題與我們的存放區管理員索引頁面。 我們專輯標題和演出者名稱屬性可以同時為時間夠長，它們可能會擲回，關閉我們表格格式。 我們將建立自訂的 HTML Helper，可讓我們來輕鬆地截斷這些和其他屬性，在我們的檢視。

![](mvc-music-store-part-5/_static/image17.png)

Razor 的@helper語法，變成很容易就能在您的檢視中建立您自己的 helper 函式使用。 開啟 /Views/StoreManager/Index.cshtml 檢視，然後加入下列程式碼直接之後@model列。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

這個 helper 方法採用字串和允許的最大長度。 如果提供的文字長度小於指定的長度，協助專家將其輸出為-是。 如果是較長，它會截斷文字，並呈現"..."的其餘部分。

現在我們可以使用我們截斷的協助程式，以確保專輯標題和演出者名稱的屬性是少於 25 個字元。 使用新的截斷協助程式的完整檢視程式碼下方會出現。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

現在我們瀏覽 /StoreManager/ URL 時，專輯和標題會維持以下我們的最大長度。

![](mvc-music-store-part-5/_static/image18.png)

注意： 這會顯示簡單的情況下，建立及使用協助程式在一個檢視中。 若要深入了解建立協助程式，您可以在整個網站使用，請參閱我的部落格文章： [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


>[!div class="step-by-step"]
[上一頁](mvc-music-store-part-4.md)
[下一頁](mvc-music-store-part-6.md)
