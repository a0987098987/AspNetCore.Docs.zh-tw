---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 第 5 部分： 編輯表單和範本 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 5 部分涵蓋編輯表單和範本。
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: c1065dcb45b6d28672edba32b95c7fc476c8b944
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832650"
---
<a name="part-5-edit-forms-and-templating"></a>第 5 部分： 編輯表單和範本
====================
藉由[Jon Galloway](https://github.com/jongalloway)

> MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。  
>   
> MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。
> 
> 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 5 部分涵蓋編輯表單和範本。


在過去的章節中，我們已從資料庫載入資料，並顯示它。 在本章中，我們也會啟用編輯的資料。

## <a name="creating-the-storemanagercontroller"></a>建立 StoreManagerController

我們一開始先建立新的控制器，稱為**StoreManagerController**。 此控制器中，我們將可利用 ASP.NET MVC 3 Tools Update 中提供的 Scaffolding 功能。 設定 [新增控制器] 對話方塊的選項，如下所示。

![](mvc-music-store-part-5/_static/image1.png)

當您按一下 [新增] 按鈕時，您會看到的 ASP.NET MVC 3 scaffolding 機制，會為您的工作：

- 它會建立新的 StoreManagerController，其區域變數的 Entity Framework
- 它將 StoreManager 資料夾加入至專案的 [Views] 資料夾
- 它會新增 Create.cshtml 」、 「 Delete.cshtml 」、 「 Details.cshtml 」、 「 Edit.cshtml 和 「 Index.cshtml 檢視中，強型別為專輯類別

![](mvc-music-store-part-5/_static/image2.png)

新的 StoreManager 控制器類別包含 CRUD （建立、 讀取、 更新、 刪除） 模型類別知道如何與專輯的控制器動作，並使用我們的 Entity Framework 內容進行資料庫存取。

## <a name="modifying-a-scaffolded-view"></a>修改包含 Scaffold 的檢視

請務必記住，雖然為我們產生這個程式碼，它是標準 ASP.NET MVC 的程式碼，就像我們最近撰寫本教學課程。 其目的是要儲存於撰寫未定案控制器程式碼和手動的方式，建立強型別的檢視的省下花的時間，但這不是產生的程式碼可能看過開頭不得變更的相關註解中的可怕的類型程式碼。 這是您的程式碼，您應該要加以變更。

因此，讓我們開始快速編輯 以 StoreManager 索引 檢視 (/ Views/StoreManager/Index.cshtml)。 此檢視會顯示資料表會列出在我們的存放區中的 Album，以編輯 / 詳細資料 / 刪除的連結，並包含專輯的公用屬性。 我們將先移除 AlbumArtUrl 欄位中，因為它不在此顯示中非常有用。 中&lt;資料表&gt;區段的 檢視程式碼中，移除&lt;th&gt;並&lt;td&gt;周圍 AlbumArtUrl 參考，如下列醒目提示的程式行所示的項目：

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

修改過的檢視程式碼會如下所示：

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>率先一睹存放區管理員

現在執行應用程式，並瀏覽至 StoreManager /。 這會顯示我們剛才修改，顯示一份專輯連結編輯的詳細資訊，以及刪除存放區中儲存管理員索引。

![](mvc-music-store-part-5/_static/image3.png)

按一下 [編輯] 連結顯示欄位是編輯的表單的專輯、 音樂類型與演出者包括下拉式清單。

![](mvc-music-store-part-5/_static/image4.png)

按一下底部的 「 回至清單 」 連結，然後按一下 Album 的詳細資料 連結。 這會顯示個別的專輯的詳細資訊。

![](mvc-music-store-part-5/_static/image5.png)

同樣地，按一下 [回到清單] 連結，，然後按一下 [刪除] 連結。 這會顯示確認對話方塊中，顯示專輯詳細資料，並詢問我們是否確定要刪除它。

![](mvc-music-store-part-5/_static/image6.png)

按一下底部的 [刪除] 按鈕將會刪除 album，並返回 [索引] 頁面，其中顯示刪除的專輯。

我們無法完成存放區管理員 中，但我們具有運作中的控制器和檢視的 CRUD 作業，從啟動的程式碼。

## <a name="looking-at-the-store-manager-controller-code"></a>查看市集管理員控制器程式碼

存放區管理員控制器包含的程式碼。 讓我們進行此程序從上到下。 控制器包含一些標準的命名空間，MVC 控制器，以及我們的模型命名空間的參考。 控制器有 MusicStoreEntities，供用於資料存取的每個控制器動作的私用執行個體。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>存放管理員 Index 和 Details 動作

[索引] 檢視會擷取一份專輯，包括每一個 album 的參考內容類型與演出者資訊，如我們先前所見的存放區瀏覽的方法上工作時。 [索引] 檢視會遵循連結物件的參考，讓它讓控制器正在有效率且查詢的原始要求中的這項資訊可以顯示每個相簿的內容類型名稱和演出者名稱。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

StoreManager 控制器的詳細資料控制器動作的運作方式完全相同的存放區控制站的詳細資料動作，我們撰寫了先前-它會查詢該專輯使用 Find() 方法的識別碼，然後將它傳回至檢視。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>建立動作方法

建立動作方法會稍有不同之處我們到目前為止，已了解，因為它們會處理表單的輸入。 當使用者第一次瀏覽時 /StoreManager/建立/它們將會顯示空白的表單。 這個 HTML 網頁會包含&lt;表單&gt;元素，其中包含下拉式清單中，文字方塊中輸入項目可以在其中輸入專輯的詳細資料。

使用者會專輯表單值中的填滿之後，可按下 [儲存] 按鈕送出這些變更回到我們的應用程式，以儲存在資料庫中。 當使用者按下 [儲存] 按鈕&lt;表單&gt;就會執行 HTTP POST 回到 /StoreManager/建立/URL，並提交&lt;表單&gt;值做為 HTTP post 要求的一部分。

ASP.NET MVC 可讓我們輕鬆地將分割邏輯的這兩種 URL 引動過程案例讓我們實作我們 StoreManagerController 類別 – 處理初始的 HTTP GET 瀏覽至 /StoreManager/Create 內兩個個別 [建立] 動作方法/ URL，以及其他處理提交的變更 HTTP POST。

### <a name="passing-information-to-a-view-using-viewbag"></a>將資訊傳遞至檢視，使用 ViewBag

我們已在稍早在本教學課程中使用 ViewBag，但還沒有特別談一下。 ViewBag 可讓我們將資訊傳遞至檢視，而不需使用強型別的模型物件。 在此情況下，不需要將這兩個內容類型和演出者名稱的清單傳遞至表單，以填入下拉式清單中，我們的編輯 HTTP-GET 控制器動作，而若要這樣做，最簡單的方法是傳回它們，當做 ViewBag 項目。

ViewBag 是動態物件，這表示，您可以輸入 ViewBag.Foo 或 ViewBag.YourNameHere 而不需要撰寫程式碼以定義這些屬性。 在此情況下，控制器程式碼會使用 ViewBag.GenreId 和 ViewBag.ArtistId 以便 GenreId 和 ArtistId，會專輯屬性會設定它們，將會提交表單的下拉式清單值。

這些下拉式清單中的值會傳回使用 SelectList 物件，只針對該目的內建表單。 這是使用如下的程式碼：

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

您可以看到從動作方法的程式碼，以便建立這個物件正在使用三個參數：

- 將會顯示下拉式清單中的項目清單。 請注意，這不只是字串-我們要將傳遞的內容類型清單。
- 下一個參數傳遞至 SelectList 是選取的值。 此 SelectList 如何知道如何預先選取清單中的項目。 這會是更容易了解當我們查看的編輯表單，這是十分類似。
- 最後一個參數是要顯示的屬性。 在此情況下，這表示 Genre.Name 屬性會顯示給使用者。

這一點之後，接著，建立 HTTP GET 的動作是很簡單-ViewBag，加入兩個 SelectLists，並沒有模型物件傳遞至表單 （因為它尚未尚未建立）。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>卸除的清單檢視中顯示建立的 HTML 協助程式

因為我們一直在討論如何將下拉式清單值傳遞至檢視，讓我們快速瀏覽檢視來查看這些值的顯示方式。 在 檢視程式碼 (/ Views/StoreManager/Create.cshtml)，您會看到下列呼叫來顯示內容類型下拉式清單下。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

這稱為 HTML 協助程式-執行常見的檢視工作的公用程式方法。 HTML 協助程式會很有用的精簡、 可讀性，讓我們檢視程式碼。 Html.DropDownList 協助程式由 ASP.NET MVC 提供，但如我們稍後所見，就可以建立自己的協助程式，我們將在我們的應用程式中重複使用的檢視程式碼。

Html.DropDownList 呼叫只需要告知兩件事-其中取得清單中，若要顯示，以及哪些值 （如果有的話） 應會預先選取。 第一個參數，也就是 GenreId 會告訴 DropDownList 以尋找名稱為 GenreId 模型或 ViewBag 中的值。 第二個參數用來指出要顯示的值為一開始選取的下拉式清單中。 這種形式是建立表單，因為沒有要預先選取的值，並傳遞 String.Empty。

### <a name="handling-the-posted-form-values"></a>處理張貼的表單值

如同我們討論之前，有兩個與每個表單相關聯的動作方法。 第一個會處理 HTTP GET 要求，並顯示表單。 第二個會處理 HTTP POST 要求，其中包含送出的表單值。 請注意，控制器動作有 [HttpPost] 屬性，它會告訴 ASP.NET MVC 只應該回應 HTTP POST 要求。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

此動作會有四個責任：

- 1. 讀取表單值
- 2. 檢查表單值是否傳遞的任何驗證規則
- 3. 如果表單提交有效，將資料儲存，並顯示更新的清單
- 4. 如果表單提交不是有效的重新顯示有驗證錯誤表單

#### <a name="reading-form-values-with-model-binding"></a>讀取與模型繫結的表單值

控制器動作正在處理的標題、 價格和 AlbumArtUrl 包含 GenreId 和 ArtistId （從下拉式清單） 的值和文字方塊值的表單提交。 雖然您可以直接存取表單值、 更好的方法是使用內建於 ASP.NET MVC 模型繫結功能。 當控制器動作將做為參數的模型型別時，ASP.NET MVC 會嘗試填入表單的輸入 （以及路由和查詢字串值） 在該類型的物件。 其做法是要尋找的名稱符合屬性的模型物件，例如設定新的專輯物件 GenreId 值時，它會尋找名稱 GenreId 的輸入。 當您建立檢視，在 ASP.NET MVC 中使用標準的方法時，將一律使用屬性名稱做為輸入的欄位名稱，因此這的欄位名稱會只比對會呈現表單。

#### <a name="validating-the-model"></a>驗證模型

使用簡單的呼叫來 ModelState.IsValid 驗證模型。 我們還沒有任何驗證規則加入我們的專輯類別-但我們會這麼做有點-現在這項檢查不會有許多執行。 重要的是這 ModelStat.IsValid 項檢查會適應我們放在我們的模型，以便未來的變更，驗證規則不需要任何更新至控制器動作程式碼的驗證規則。

#### <a name="saving-the-submitted-values"></a>正在儲存提交的值

如果表單提交會通過驗證，就可以將值儲存到資料庫。 使用 Entity Framework，只需要將模型新增至相簿集合，並呼叫 SaveChanges。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework 會產生適當的 SQL 命令，以保存的值。 儲存資料之後, 我們重新導向回專輯的清單，我們會看到我們的更新。 這是藉由傳回 RedirectToAction 控制器動作，我們想要顯示的名稱。 在此情況下，這是 Index 方法。

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>顯示無效的表單提交作業，發生驗證錯誤

在無效的格式輸入的情況下的下拉式清單中的值加入 ViewBag （如 HTTP GET 案例中），並繫結的模型值如顯示時，會傳回給檢視。 驗證錯誤會自動顯示使用@Html.ValidationMessageForHTML 協助程式。

#### <a name="testing-the-create-form"></a>測試建立表單

若要測試此時執行的應用程式和瀏覽至 /StoreManager/建立 /-這會顯示您 StoreController 建立 HTTP GET 方法所傳回的空白表單。

填入一些值，然後按一下 [建立] 按鈕來送出表單。

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>處理編輯

編輯動作配對 （HTTP-GET 和 HTTP-POST） 是非常類似於我們剛才看到的建立動作方法。 因為編輯案例牽涉到使用現有的相簿，編輯 HTTP GET 方法會載入專輯根據 「 識別碼 」 的參數，傳遞透過路由。 因為我們已經先前討論過，在 詳細資料控制器動作來擷取由 AlbumId album 的這段程式碼都是相同的。 使用 建立 / HTTP GET 方法，透過 ViewBag 傳回下拉式值清單。 這可讓我們回到 Album 為我們的模型物件的檢視 （也就強型別專輯類別） 並透過 ViewBag 傳遞其他資料 （例如內容類型清單）。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

編輯 HTTP POST 動作是非常類似於建立的 HTTP POST 動作。 唯一的差別，而非資料庫中新增新的相簿。專輯集合中，我們覺得專輯的目前執行個體使用 db。Entry(album) 並將其狀態設定為已修改。 這會告訴 Entity Framework，我們會修改現有的相簿，而不是建立一個新。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

我們可以執行應用程式，並瀏覽至/StoreManger/，然後按一下 album 的編輯連結此進行測試。

![](mvc-music-store-part-5/_static/image9.png)

這會顯示編輯的 HTTP GET 方法所顯示的編輯表單。 填入一些值，然後按一下 [儲存] 按鈕。

![](mvc-music-store-part-5/_static/image10.png)

這會張貼表單時，儲存值，並傳回我們 [專輯] 清單中，顯示已更新的值。

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>處理刪除

刪除會遵循與編輯和建立，使用一個控制器動作，以顯示 [確認] 表單中和另一個控制器動作，以處理表單提交相同的模式。

HTTP GET 刪除控制器動作正是我們先前的存放區管理員詳細資料控制器動作相同。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

專輯類型，使用 刪除檢視的內容樣板，我們就會顯示一個表單，強型別。

![](mvc-music-store-part-5/_static/image12.png)

刪除範本會顯示在模型中的所有欄位，但我們可以稍微簡化該清單。 變更檢視中的程式碼 /Views/StoreManager/Delete.cshtml 所示。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

這會顯示簡化的刪除確認。

![](mvc-music-store-part-5/_static/image13.png)

按一下 [刪除] 按鈕會使表單回傳至伺服器，它會執行 DeleteConfirmed 動作。

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

我們 HTTP-POST 刪除控制器動作會採取下列動作：

- 1. 依識別碼專輯的負載
- 2. 會專輯刪除它並儲存變更
- 3. 重新導向至索引中，顯示已從清單中移除相簿

若要測試這種情況，執行應用程式，並瀏覽至 /StoreManager。 從清單中選取 專輯，然後按一下 刪除 連結。

![](mvc-music-store-part-5/_static/image14.png)

這會顯示我們刪除確認 畫面。

![](mvc-music-store-part-5/_static/image15.png)

按一下 刪除 按鈕會專輯中移除，並傳回我們至存放區管理員索引 頁面，其中顯示已刪除 album。

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>使用自訂的 HTML Helper 來截斷文字

我們有一項潛在的問題與我們的存放區管理員索引頁面。 我們專輯標題和演出者名稱的屬性都可以是時間夠長，則可能會擲回，關閉我們的表格格式。 我們將建立自訂的 HTML Helper，讓我們輕鬆地截斷這些及其他屬性，在我們的檢視。

![](mvc-music-store-part-5/_static/image17.png)

Razor 的@helper語法使得它很容易就能建立您自己的 helper 函式，使用您的檢視中。 開啟 /Views/StoreManager/Index.cshtml 檢視，然後加入下列程式碼直接之後@model列。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

這個 helper 方法會採用字串和允許的最大長度。 如果提供的文字會小於指定的長度，協助專家將其輸出做為是。 如果很長，它會截斷文字，並呈現"..."的其餘部分。

現在我們可以使用我們截斷的協助程式，以確保的專輯標題和演出者名稱的屬性都少於 25 個字元。 使用新的截斷協助程式的完整檢視程式碼下方會出現。

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

現在當我們瀏覽 /StoreManager/ URL，則專輯和標題會保留下面我們最大長度。

![](mvc-music-store-part-5/_static/image18.png)

注意： 這會顯示簡單的情況下，建立和使用協助程式在一個檢視中。 若要深入了解建立可供您在整個網站的協助程式，請參閱我部落格文章： [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


> [!div class="step-by-step"]
> [上一頁](mvc-music-store-part-4.md)
> [下一頁](mvc-music-store-part-6.md)
