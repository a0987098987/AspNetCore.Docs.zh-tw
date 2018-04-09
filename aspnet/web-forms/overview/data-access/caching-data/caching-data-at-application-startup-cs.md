---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
title: 快取資料在應用程式啟動 (C#) |Microsoft 文件
author: rick-anderson
description: 在任何 Web 應用程式資料會經常使用，將不常使用某些資料。 我們可以改進我們的 ASP.NET 應用程式 b 效能...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 22ca8efa-7cd1-45a7-b9ce-ce6eb3b3ff95
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
msc.type: authoredcontent
ms.openlocfilehash: 8d962a182b5136d3e44ce678a355c9679b4c8be1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="caching-data-at-application-startup-c"></a>快取資料在應用程式啟動 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載 PDF](caching-data-at-application-startup-cs/_static/datatutorial60cs1.pdf)

> 在任何 Web 應用程式資料會經常使用，將不常使用某些資料。 預先載入常用的資料，ant colony optimization，我們可以改進我們的 ASP.NET 應用程式的效能。 本教學課程示範主動式載入，也就是將資料載入至快取應用程式啟動時的其中一個方法。


## <a name="introduction"></a>簡介

在快取中的簡報和快取層級中的資料，查看兩個先前的教學課程。 在[快取資料與 ObjectDataSource](caching-data-with-the-objectdatasource-cs.md)，我們探討了使用 ObjectDataSource s 中的快取來展示層中的快取資料的功能。 [快取資料的架構中](caching-data-in-the-architecture-cs.md)檢查新的、 不同的快取層中的快取。 這兩個使用這些教學課程*反應式載入*中使用的資料快取。 使用反應靈敏的載入，每次要求資料時，系統會先檢查如果它快取。 如果沒有，它 grabs 示範的資料從原始來源，例如資料庫，並將其儲存在快取中。 反應式載入的主要優點是容易實作。 其缺點是不平均的效能要求。 假設使用快取層前述教學課程來顯示產品資訊的頁面。 當此頁面瀏覽的第一次，或瀏覽的第一次之後已收回快取的資料，因為記憶體條件約束或具有已達到指定的到期日期時，則必須從資料庫擷取的資料。 因此，這些使用者要求將會比長可提供的使用者要求快取。

*主動式載入*提供替代的快取管理策略效能該會平滑化跨要求藉由載入之前快取的資料所需。 一般而言，主動載入會使用某些處理程序來定期檢查，或已對基礎資料更新時，會收到通知。 此程序，然後更新要保持最新的快取。 主動載入為基礎的資料來自緩慢的資料庫連接、 Web 服務或其他特別緩慢的資料來源的特別有用。 但這種方法來主動載入比較難實作，因為它需要建立、 管理和部署處理程序，檢查有變更，並更新快取。

主動式載入，以及我們將在本教學課程中，瀏覽的類型的另一個類別會將資料載入應用程式啟動時快取。 這個方法是特別適用於快取靜態資料，例如資料庫查閱資料表中的記錄。

> [!NOTE]
> 主動式與反應式載入以及專業人員及缺點，實作建議的清單之間的差異更深入探討，請參閱[管理的快取的內容](https://msdn.microsoft.com/library/ms978503.aspx)區段[快取 .NET Framework 應用程式的架構指南](https://msdn.microsoft.com/library/ms978498.aspx)。


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>步驟 1： 決定要在應用程式啟動時的快取的資料

我們檢查用於產生，可能會定期變更，且未採用 exorbitantly 長資料的先前兩個教學課程工作中使用反應靈敏載入快取的範例。 但如果快取的資料永遠不會變更，使用反應靈敏的載入到期多餘。 同樣地，如果所快取的資料需要非常長的時間，來產生，然後這些使用者的要求可讓您尋找快取空白必須 endure 基礎資料執行時間較長的等候擷取。 請考慮快取靜態資料和資料，要花更長的時間來產生應用程式啟動時。

雖然資料庫都有許多動態，經常變更的值，大部分也會有大量的靜態資料。 比方說，幾乎所有的資料模型有一個或多個資料行包含特定值從一組固定的選項。 A`Patients`資料庫資料表可能會有`PrimaryLanguage`資料行集的值可能是英文、 西班牙文、 法文、 俄文、 日文、 等等。 有時候，實作這些類型的資料行是使用*查閱資料表*。 而不是英文或法文中的將字串儲存`Patients`，第二個資料表建立資料表的情況下，有兩個資料行的唯一識別碼和字串描述為每個可能值的記錄。 `PrimaryLanguage`中的資料行`Patients`資料表查閱資料表中儲存對應的唯一識別碼。 圖 1 病患 John Doe s 主要語言是英文，俄羅斯 Ed Johnson s 時。


![語言資料表是病患資料表查閱資料表使用](caching-data-at-application-startup-cs/_static/image1.png)

**圖 1**:`Languages`資料表是查閱資料表由`Patients`資料表


編輯或建立新的 patient 的使用者介面的可允許的語言中的記錄填入下拉式清單包括`Languages`資料表。 但不快取，這個介面是每次瀏覽系統必須查詢`Languages`資料表。 這是浪費不必要查閱資料表值的變更很少，因為如果有的話。

我們無法快取`Languages`使用相同的反應靈敏的載入技術檢查先前的教學課程中的資料。 反應式載入，不過，會使用以時間為基礎的到期，就不需要的靜態查閱資料表的資料。 而使用反應靈敏的載入快取必須是優於無快取完全，最好的方法是主動載入應用程式啟動時快取中的查閱資料表資料。

在此教學課程中我們將探討如何快取查閱資料表資料及其他靜態資訊。

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>步驟 2： 檢查快取資料的不同方式

可在 ASP.NET 應用程式使用多種方式，以程式設計方式快取資訊。 我們我們已經看到如何使用先前的教學課程中的資料快取。 或者，可以以程式設計方式快取物件使用*靜態成員*或*應用程式狀態*。

當使用類別，通常類別必須先具現化之前可以存取其成員。 例如，若要叫用中的其中一個在我們的商務邏輯層中類別的方法，我們必須先建立類別的執行個體：


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample1.cs)]

我們可以叫用之前*SomeMethod*或使用*SomeProperty*，我們必須先建立類別使用的執行個體`new`關鍵字。 *SomeMethod*和*SomeProperty*相關聯的特定執行個體。 這些成員的存留期會繫結及其相關聯的物件的存留期。 *靜態成員*，相反地，是變數、 屬性和方法所共用*所有*類別的執行個體，因此，只要類別存留期。 靜態成員以關鍵字代表`static`。

除了靜態成員，可以快取的資料使用應用程式狀態。 每個 ASP.NET 應用程式會維護名稱/值集合 s 的所有使用者和應用程式的頁面之間共用。 此集合可以使用存取[`HttpContext`類別](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)s [ `Application`屬性](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)，並使用從 ASP.NET 頁面 s 程式碼後置類別就像這樣：


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample2.cs)]

資料快取的快取資料，提供時間和相依性基礎 expiries、 快取項目優先順序、 等等的機制提供更豐富的 API。 靜態成員和應用程式狀態，這類功能必須手動新增網頁開發人員。 當應用程式的存留期間，快取應用程式啟動時的資料，不過，資料快取的優點是毫無意義。 在此教學課程中我們將探討使用所有的三種技術來快取靜態資料的程式碼。

## <a name="step-3-caching-thesupplierstable-data"></a>步驟 3： 快取`Suppliers`表格資料

Northwind 資料庫資料表我們 ve 至今實作不包含任何傳統的查閱資料表。 在我們的 DAL 中實作的四個 Datatable 所有模型的資料表，其值為非靜態。 而不是花時間來 DAL 然後新類別和方法，以 BLL，加入新 DataTable 的本教學課程只讓 s 假裝，`Suppliers`資料表的資料是靜態的。 因此，我們可以快取這項資料在應用程式啟動。

若要開始，建立新的類別，名為`StaticCache.cs`中`CL`資料夾。


![CL 資料夾中建立 StaticCache.cs 類別](caching-data-at-application-startup-cs/_static/image2.png)

**圖 2**： 建立`StaticCache.cs`類別`CL`資料夾


我們要加入的方法，在啟動時將資料載入適當的快取存放區中，以及從這個快取中傳回資料的方法。


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample3.cs)]

上述程式碼會使用靜態成員變數， `suppliers`，來保存從結果`SuppliersBLL`類別 s`GetSuppliers()`方法，從呼叫`LoadStaticCache()`方法。 `LoadStaticCache()`方法用於應用程式的啟動期間呼叫。 這項資料在應用程式啟動時載入之後, 就可以呼叫需要搭配供應商資料的任何頁面`StaticCache`類別的`GetSuppliers()`方法。 因此，為了取得供應商到資料庫的呼叫只發生一次，在應用程式啟動。

而不是做為快取存放區使用的靜態成員變數，我們原也可以另外使用應用程式狀態或讓資料快取。 下列程式碼顯示使用應用程式狀態改裝類別：


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample4.cs)]

在`LoadStaticCache()`，供應商資訊儲存在應用程式變數*金鑰*。 它傳回適當的型別 (`Northwind.SuppliersDataTable`) 從`GetSuppliers()`。 雖然可以使用 ASP.NET 網頁的程式碼後置類別中存取應用程式狀態`Application["key"]`，我們必須使用的架構中`HttpContext.Current.Application["key"]`以取得目前`HttpContext`。

同樣地，資料快取可用做為快取存放區，如下列程式碼所示：


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample5.cs)]

若要加入任何以時間為基礎的到期資料的快取項目，請使用`System.Web.Caching.Cache.NoAbsoluteExpiration`和`System.Web.Caching.Cache.NoSlidingExpiration`做為輸入參數的值。 這個特定的多載的資料快取 s`Insert`方法已選取，因此我們無法指定*優先順序*的快取項目。 優先順序用來判斷哪些項目可用的記憶體不足時，從快取清除。 這裡我們使用優先順序 `NotRemovable`，後者可確保此快取項目贏了 t 清除。

> [!NOTE]
> 此教學課程的下載實作`StaticCache`類別使用靜態成員變數的方法。 使用中的類別檔案中的註解的程式碼應用程式狀態和資料快取技術。


## <a name="step-4-executing-code-at-application-startup"></a>步驟 4： 執行應用程式啟動時的程式碼

Web 應用程式初次啟動時執行程式碼，我們必須建立名為的特殊檔案`Global.asax`。 這個檔案可以包含為應用程式，工作階段的事件處理常式，並要求層級事件，而且這是這裡我們可以在其中加入會在每次應用程式啟動時執行的程式碼。

新增`Global.asax`檔案，以 web 應用程式 s 根目錄 Visual Studio 的方案總管] 中的網站專案名稱上按一下滑鼠右鍵，然後選擇 [加入新項目。 [加入新項目] 對話方塊中，從選取的全域應用程式類別的項目類型，然後按一下 [新增] 按鈕。

> [!NOTE]
> 如果您已經有`Global.asax`檔案在專案中，全域項目類型不會列在 [加入新項目] 對話方塊中的應用程式類別。


[![Global.asax 檔加入 Web 應用程式 s 根目錄](caching-data-at-application-startup-cs/_static/image4.png)](caching-data-at-application-startup-cs/_static/image3.png)

**圖 3**： 新增`Global.asax`檔案到您的 Web 應用程式 s 根目錄 ([按一下以檢視完整大小的影像](caching-data-at-application-startup-cs/_static/image5.png))


預設值`Global.asax`檔案範本包含五種方法在伺服器端內`<script>`標記：

- **`Application_Start`** web 應用程式第一次啟動時執行
- **`Application_End`** 應用程式正在關機而關閉時執行
- **`Application_Error`** 執行時處理的例外狀況到達應用程式
- **`Session_Start`** 建立新的工作階段時執行
- **`Session_End`** 執行工作階段已過期或已放棄時

`Application_Start` s 應用程式生命週期內一次呼叫事件處理常式。 在應用程式啟動第一次 ASP.NET 資源會從應用程式要求，會繼續執行，直到重新啟動應用程式時，這可能會藉由修改的內容`/Bin`資料夾中，修改`Global.asax`，修改在內容`App_Code`資料夾，或修改`Web.config`檔案，在其他原因。 請參閱[ASP.NET 應用程式生命週期概觀](https://msdn.microsoft.com/library/ms178473.aspx)的應用程式生命週期的更詳細討論。

如需這些教學課程我們只需要將程式碼加入`Application_Start`方法中，因此可自由移除其他。 在`Application_Start`，只需呼叫`StaticCache`類別的`LoadStaticCache()`方法，將會載入並快取供應商資訊：


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample6.aspx)]

S 都是這麼簡單 ！ 在應用程式啟動時，`LoadStaticCache()`方法將會抓取 BLL，從供應商的資訊，並將它儲存在靜態成員變數 (或任何快取儲存您結束使用中`StaticCache`類別)。 若要確認這種行為，請在設定的中斷點`Application_Start`方法並執行您的應用程式。 請注意，應用程式啟動時叫用中斷點。 後續的要求，不過，不會導致`Application_Start`方法才能執行。


[![使用確認 Application_Start 事件處理常式正在執行的中斷點](caching-data-at-application-startup-cs/_static/image7.png)](caching-data-at-application-startup-cs/_static/image6.png)

**圖 4**： 使用驗證的中斷點，`Application_Start`事件處理常式是正在執行 ([按一下以檢視完整大小的影像](caching-data-at-application-startup-cs/_static/image8.png))


> [!NOTE]
> 如果您不會遇到`Application_Start`您第一次開始偵錯中斷點，這是因為您的應用程式已經開始。 強制應用程式藉由修改重新啟動您`Global.asax`或`Web.config`檔，並再試一次。 您可以直接新增 （或移除） 空白行其中一個快速地重新啟動應用程式檔案的結尾。


## <a name="step-5-displaying-the-cached-data"></a>步驟 5： 顯示快取的資料

此時`StaticCache`類別有一個版本的應用程式啟動時，可以透過存取快取的供應商資料及其`GetSuppliers()`方法。 若要使用這項資料從展示層，我們可以使用 ObjectDataSource，或以程式設計方式叫用`StaticCache`類別的`GetSuppliers()`從 ASP.NET 頁面 s 程式碼後置類別的方法。 可讓 s 查看使用 ObjectDataSource 和 GridView 控制項以顯示快取供應商的資訊。

先開啟`AtApplicationStartup.aspx`頁面`Caching`資料夾。 從工具箱拖曳至設計工具，設定將 GridView 其`ID`屬性`Suppliers`。 接下來，從 GridView s 智慧標籤選擇要建立名為新 ObjectDataSource `SuppliersCachedDataSource`。 設定用於 ObjectDataSource`StaticCache`類別的`GetSuppliers()`方法。


[![設定使用 StaticCache 類別 ObjectDataSource](caching-data-at-application-startup-cs/_static/image10.png)](caching-data-at-application-startup-cs/_static/image9.png)

**圖 5**： 設定用於 ObjectDataSource`StaticCache`類別 ([按一下以檢視完整大小的影像](caching-data-at-application-startup-cs/_static/image11.png))


[![使用 GetSuppliers() 方法來擷取快取供應商資料](caching-data-at-application-startup-cs/_static/image13.png)](caching-data-at-application-startup-cs/_static/image12.png)

**圖 6**： 使用`GetSuppliers()`方法來擷取快取供應商資料 ([按一下以檢視完整大小的影像](caching-data-at-application-startup-cs/_static/image14.png))


完成精靈之後，Visual Studio 會自動加入 BoundFields 中的資料欄位的每個`SuppliersDataTable`。 GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample7.aspx)]

圖 7 顯示透過瀏覽器檢視時的頁面。 輸出會與相同有我們提取的資料從 BLL s`SuppliersBLL`類別，但使用`StaticCache`類別會傳回做為應用程式啟動時快取的供應商資料。 您可以設定中斷點，`StaticCache`類別的`GetSuppliers()`方法以驗證這種行為。


[![快取供應商資料會顯示在 GridView](caching-data-at-application-startup-cs/_static/image16.png)](caching-data-at-application-startup-cs/_static/image15.png)

**圖 7**: 快取供應商資料會顯示在 GridView ([按一下以檢視完整大小的影像](caching-data-at-application-startup-cs/_static/image17.png))


## <a name="summary"></a>總結

大部分每一個資料模型包含大量的靜態資料，通常在查閱資料表的形式實作。 這項資訊是靜態的因為那里 s 沒有理由持續存取資料庫每次需要顯示這項資訊。 此外，由於其靜態的本質，快取的資料時那里 s 到期不需要。 在此教學課程中我們可了解如何取得這類資料並加以快取中的資料快取，應用程式狀態，以及透過靜態成員變數。 這項資訊會快取應用程式啟動時，會保留在快取整個應用程式 s 存留期。

在本教學課程和過去的兩個，我們在應用程式 s 存留期的持續期間快取資料，以及使用以時間為基礎的 expiries 查看發生。 當快取資料庫的資料，不過，以時間為基礎的到期可能不盡理想。 而不是定期快取清除，亦即是最佳的作法是基礎資料庫資料修改時，只能收回快取的項目。 可以透過 SQL 快取相依性，這在我們的下一個教學課程中，我們將檢驗使用這個最理想的。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已本文菲和 Zack Jones。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](caching-data-in-the-architecture-cs.md)
> [下一頁](using-sql-cache-dependencies-cs.md)
