---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
title: 快取資料在應用程式啟動 (C#) |Microsoft Docs
author: rick-anderson
description: 任何 Web 應用程式中某些資料會經常使用，並將不常使用一些資料。 我們可以改進我們的 ASP.NET 應用程式 b 的效能...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 22ca8efa-7cd1-45a7-b9ce-ce6eb3b3ff95
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
msc.type: authoredcontent
ms.openlocfilehash: 01f80353e3f871606d022b01c98b0d36d15d2379
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370474"
---
<a name="caching-data-at-application-startup-c"></a>快取資料在應用程式啟動 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載 PDF](caching-data-at-application-startup-cs/_static/datatutorial60cs1.pdf)

> 任何 Web 應用程式中某些資料會經常使用，並將不常使用一些資料。 我們可以預先載入常用的資料，又稱為技術來改善我們的 ASP.NET 應用程式的效能。 本教學課程會示範其中一種方法積極載入，也就是將資料載入至快取在應用程式啟動。


## <a name="introduction"></a>簡介

兩個先前的教學課程會討論過快取中的簡報和快取層級的資料。 在 [使用 ObjectDataSource 快取的資料](caching-data-with-the-objectdatasource-cs.md)，我們探討使用 ObjectDataSource s 中的快取來快取資料展示層中的功能。 [快取架構中的資料](caching-data-in-the-architecture-cs.md)檢查新的、 不同的快取層中的快取。 這兩個使用這些教學課程*反應式載入*中使用的資料快取。 透過回應式的載入，每次要求資料時，系統會先檢查如果它快取。 如果沒有，它會擷取原始的來源，例如資料庫中的資料，並再將其儲存在快取。 回應式載入的主要優點是容易實作。 其缺點是它不平均的效能要求之間。 想像一下使用快取的圖層，從先前的教學課程，以顯示產品資訊的頁面。 當此頁面是第一次，瀏覽，或瀏覽的第一次快取的資料具有已收回，因為記憶體條件約束或已達到指定的到期之後時，必須從資料庫擷取資料。 因此，這些使用者要求將花費超過可提供的使用者要求的快取。

*主動式載入*提供替代的快取管理策略的效能，會平滑化跨要求藉由載入之前快取的資料所需。 通常，主動載入會使用某些處理程序，定期檢查，或已更新基礎資料時，會收到通知。 此程序，然後更新以保持最新的快取。 主動式的載入是特別有用，如果基礎資料來源緩慢的資料庫連接、 Web 服務或某些其他特別緩慢的資料來源。 但這種方法積極載入比較難實施，因為它需要建立、 管理和部署處理程序來檢查變更，並更新快取。

另一項好處主動式的載入，以及我們將在本教學課程中，瀏覽的型別都將資料載入快取在應用程式啟動。 這種方法是特別適用於快取靜態資料，例如資料庫查閱資料表中的記錄。

> [!NOTE]
> 如需更深入了解主動式與反應式載入，以及專業人員、 缺點和實作建議的清單之間的差異，請參閱[管理的快取的內容](https://msdn.microsoft.com/library/ms978503.aspx)一節[快取.NET Framework 應用程式架構指南](https://msdn.microsoft.com/library/ms978498.aspx)。


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>步驟 1： 決定要在應用程式啟動的快取的資料

使用回應式載入的快取範例中，我們檢查在先前兩個教學課程適用於產生的資料，可能會定期變更，且不接受 exorbitantly 長。 但如果快取的資料永遠不會變更，是多餘的回應式載入所使用的過期狀態。 同樣地，如果快取的資料需要非常長的時間，才能產生，則這些使用者，其要求可讓您尋找快取空必須忍受長時間等候基礎資料被擷取。 請考慮快取靜態資料和資料，要花更長的時間，以在應用程式啟動時產生。

當資料庫有許多動態時，經常變更值，但大部分也會有大量的靜態資料。 例如，幾乎所有的資料模型會有一或多個資料行，包含特定的值從一組固定的選項。 A`Patients`資料庫資料表可能有`PrimaryLanguage`資料行，其組的值可能是英文、 西班牙文、 法文、 俄文、 日文和等等。 通常，實作這些類型的資料行是使用*查閱資料表*。 而不是英文或法文中的將字串儲存`Patients`，第二個資料表建立資料表的情況下，有兩個資料行集的唯一識別碼和字串描述-每個可能值的記錄。 `PrimaryLanguage`中的資料行`Patients`資料表查閱資料表中儲存的相對應的唯一識別碼。 在 圖 1 中，病患 John Doe s 主要語言是英文，俄羅斯 Ed Johnson s 時。


![語言資料表是病患資料表查閱資料表使用](caching-data-at-application-startup-cs/_static/image1.png)

**圖 1**:`Languages`資料表是查閱資料表由`Patients`資料表


編輯或建立新的 patient 的使用者介面會包含允許的語言中的記錄填入的下拉式清單`Languages`資料表。 沒有快取，這個介面是每次瀏覽系統必須查詢`Languages`資料表。 這是浪費和不必要，因為查閱資料表的值會變更很少發生，如果有的話。

我們無法快取`Languages`使用相同的回應式載入技術檢查先前的教學課程中的資料。 回應式載入，不過，會使用以時間為基礎的到期，就不需要靜態的查閱資料表資料。 雖然快取使用回應式載入會優於無快取完全，最好的方法是主動載入應用程式啟動時快取中的查閱資料表的資料。

在本教學課程中我們將探討如何快取查閱資料表資料和其他靜態資訊。

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>步驟 2： 檢查快取資料的不同方式

可在 ASP.NET 應用程式使用各種不同的方法中以程式設計方式快取資訊。 我們 ve 已經看過如何使用在先前的教學課程中的資料快取。 或者，物件可以以程式設計方式使用快取*靜態成員*或是*應用程式狀態*。

當使用類別，通常必須先會具現化類別才能存取其成員。 例如，若要叫用方法，以從我們的商務邏輯層中的類別的其中一個，我們必須先建立類別的執行個體：


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample1.cs)]

我們可以叫用之前*SomeMethod* ，或搭配*SomeProperty*，我們必須先建立類別使用的執行個體`new`關鍵字。 *SomeMethod*並*SomeProperty*與特定的執行個體相關聯。 這些成員的存留期會繫結至其相關聯的物件的存留期。 *靜態成員*，相反地，是變數、 屬性和方法，由共用*所有*類別的執行個體，因此，需要長達類別的存留期。 靜態成員會以關鍵字表示`static`。

除了靜態成員，可以快取的資料使用應用程式狀態。 每個 ASP.NET 應用程式會維護名稱/值集合的所有使用者和應用程式的頁面之間共用該 s。 此集合可以使用來存取[`HttpContext`類別](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)s [ `Application`屬性](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)，以及使用過 ASP.NET 頁面 s 程式碼後置類別就像這樣：


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample2.cs)]

資料快取進行快取資料、 時間和相依性為基礎的 expiries、 快取項目優先順序、 和等提供機制提供更豐富的 API。 靜態成員和應用程式狀態，這類功能必須手動加入由頁面開發人員。 當應用程式的存留期間，快取在應用程式啟動的資料，不過，資料快取的優點是毫無意義。 在本教學課程中我們將探討使用所有的三個技術來快取靜態資料的程式碼。

## <a name="step-3-caching-thesupplierstable-data"></a>步驟 3： 快取`Suppliers`資料表資料

Northwind 資料庫資料表我們已實作的日期不包含任何傳統的查閱資料表。 在我們的 DAL 中實作的四個 Datatable 所有模型的資料表，其值為非靜態。 需要花時間來新增新的 DataTable 來 DAL 然後新的類別和方法，以 BLL，本教學課程可讓 s 就假裝，`Suppliers`資料表的資料是靜態的。 因此，我們無法快取這項資料在應用程式啟動。

若要開始，建立新的類別，名為`StaticCache.cs`在`CL`資料夾。


![CL 資料夾中建立 StaticCache.cs 類別](caching-data-at-application-startup-cs/_static/image2.png)

**圖 2**： 建立`StaticCache.cs`類別中`CL`資料夾


我們需要加入會在啟動時將資料載入適當的快取存放區的方法，以及從這個快取中傳回資料的方法。


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample3.cs)]

上述程式碼會使用靜態成員變數`suppliers`，以保留的結果`SuppliersBLL`類別 s`GetSuppliers()`方法，它會從呼叫`LoadStaticCache()`方法。 `LoadStaticCache()`方法是在應用程式的啟動期間呼叫。 任何需要使用供應商資料的頁面載入之後的這項資料在應用程式啟動時，可以呼叫`StaticCache`類別的`GetSuppliers()`方法。 因此，資料庫呼叫，以取得 [供應商] 才會出現一次，在應用程式啟動。

而不被使用靜態成員變數做為快取存放區時，我們也可以另外使用應用程式狀態或資料快取。 下列程式碼顯示使用應用程式狀態改裝類別：


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample4.cs)]

在  `LoadStaticCache()`，供應商資訊儲存在應用程式變數*金鑰*。 它傳回為適當的類型 (`Northwind.SuppliersDataTable`) 從`GetSuppliers()`。 雖然可以使用 ASP.NET 頁面的程式碼後置類別中存取應用程式狀態`Application["key"]`，在架構中，我們必須使用`HttpContext.Current.Application["key"]`若要取得目前`HttpContext`。

同樣地，資料快取可用做快取存放區，如下列程式碼所示：


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample5.cs)]

若要加入無時間為基礎的到期日的資料快取中的項目，請使用`System.Web.Caching.Cache.NoAbsoluteExpiration`和`System.Web.Caching.Cache.NoSlidingExpiration`做為輸入參數的值。 這個多載的資料快取 s`Insert`已選取方法，以便我們可以指定*優先順序*的快取項目。 優先順序用來判斷可用的記憶體不足時，從快取清除項目。 在這裡我們使用優先順序`NotRemovable`，以確保此快取項目贏得 t 清除。

> [!NOTE]
> 此教學課程的下載實作`StaticCache`類別使用靜態成員變數的方法。 提供的類別檔案中的註解的程式碼的應用程式的狀態和資料快取技術。


## <a name="step-4-executing-code-at-application-startup"></a>步驟 4： 執行在應用程式啟動的程式碼

Web 應用程式初次啟動時，請執行程式碼，我們需要建立一個名為的特殊檔案`Global.asax`。 這個檔案可以包含應用程式-，工作階段-的事件處理常式和要求層級事件，而且都在這裡我們可以在其中加入將在每次應用程式啟動時執行的程式碼。

新增`Global.asax`檔案至您 web 應用程式的根目錄 Visual Studio 的方案總管] 中的網站專案名稱上按一下滑鼠右鍵，然後選擇 [加入新項目。 從 [加入新項目] 對話方塊中，選取全域應用程式類別項目類型，然後按一下 [新增] 按鈕。

> [!NOTE]
> 如果您已經有`Global.asax`檔案在專案中，全域項目類型將不會列在 [加入新項目] 對話方塊中的應用程式類別。


[![加入 Web 應用程式的根目錄中的 Global.asax 檔案](caching-data-at-application-startup-cs/_static/image4.png)](caching-data-at-application-startup-cs/_static/image3.png)

**圖 3**： 新增`Global.asax`到您的 Web 應用程式 s 根目錄的檔案 ([按一下以檢視完整大小的影像](caching-data-at-application-startup-cs/_static/image5.png))


預設值`Global.asax`檔案範本包含五個方法，在伺服器端內`<script>`標記：

- **`Application_Start`** 當 web 應用程式第一次啟動時執行
- **`Application_End`** 當應用程式正在關閉時執行
- **`Application_Error`** 執行時處理的例外狀況到達應用程式
- **`Session_Start`** 建立新的工作階段時執行
- **`Session_End`** 執行時的工作階段會過期或已放棄

`Application_Start` s 的應用程式生命週期內一次呼叫事件處理常式。 在應用程式啟動第一次 ASP.NET 資源會從應用程式要求，並繼續執行，直到重新啟動應用程式時，會發生這個情況所修改的內容`/Bin`資料夾中，修改`Global.asax`，修改中的內容`App_Code`資料夾，或修改`Web.config`檔，以及其他原因。 請參閱[ASP.NET 應用程式生命週期概觀](https://msdn.microsoft.com/library/ms178473.aspx)如應用程式生命週期的更詳細討論。

這些教學課程中我們只需要將程式碼加入`Application_Start`方法中，因此隨時移除其他人。 在  `Application_Start`，只要呼叫`StaticCache`類別的`LoadStaticCache()`方法，它會載入並快取供應商資訊：


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample6.aspx)]

S 就是這麼簡單 ！ 在應用程式啟動時，`LoadStaticCache()`方法會擷取從 BLL，供應商資訊，並將它儲存在靜態成員變數 (或任何快取儲存您結束使用中`StaticCache`類別)。 若要確認這種行為，請在設定的中斷點`Application_Start`方法並執行您的應用程式。 請注意應用程式啟動時叫用中斷點。 後續的要求，不過，不會造成`Application_Start`方法來執行。


[![使用中斷點，以確認 Application_Start 事件處理常式正在執行](caching-data-at-application-startup-cs/_static/image7.png)](caching-data-at-application-startup-cs/_static/image6.png)

**圖 4**： 使用中斷點，以確認所`Application_Start`事件處理常式是正在執行 ([按一下以檢視完整大小的影像](caching-data-at-application-startup-cs/_static/image8.png))


> [!NOTE]
> 如果您不會叫用`Application_Start`您第一次開始偵錯的中斷點，這是因為您的應用程式已經開始。 強制重新啟動，藉由修改應用程式`Global.asax`或`Web.config`檔，並再試一次。 您可以只是新增 （或移除） 的其中一個快速重新啟動應用程式的檔案結尾空白的行。


## <a name="step-5-displaying-the-cached-data"></a>步驟 5： 顯示快取的資料

此時`StaticCache`類別有一個版本的供應商資料快取在應用程式啟動時，您可以透過其`GetSuppliers()`方法。 若要使用這項資料從展示層，我們可以使用 ObjectDataSource，或以程式設計方式叫用`StaticCache`類別的`GetSuppliers()`從 ASP.NET 頁面 s 程式碼後置類別的方法。 可讓 s 看看使用 ObjectDataSource 和 GridView 控制項來顯示快取供應商資訊。

首先開啟`AtApplicationStartup.aspx`頁面中`Caching`資料夾。 拖曳的 GridView，從 [工具箱] 拖曳至設計工具，設定其`ID`屬性設`Suppliers`。 接下來，從 GridView s 智慧標籤選擇要建立名為新 ObjectDataSource `SuppliersCachedDataSource`。 設定要使用 ObjectDataSource`StaticCache`類別的`GetSuppliers()`方法。


[![設定使用 StaticCache 類別 ObjectDataSource](caching-data-at-application-startup-cs/_static/image10.png)](caching-data-at-application-startup-cs/_static/image9.png)

**圖 5**： 設定要使用 ObjectDataSource`StaticCache`類別 ([按一下以檢視完整大小的影像](caching-data-at-application-startup-cs/_static/image11.png))


[![使用 GetSuppliers() 方法來擷取快取供應商資料](caching-data-at-application-startup-cs/_static/image13.png)](caching-data-at-application-startup-cs/_static/image12.png)

**圖 6**： 使用`GetSuppliers()`方法來擷取快取供應商資料 ([按一下以檢視完整大小的影像](caching-data-at-application-startup-cs/_static/image14.png))


完成精靈之後，Visual Studio 會自動將新增 BoundFields 中的資料欄位的每個`SuppliersDataTable`。 GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample7.aspx)]

[圖 7] 顯示頁面透過瀏覽器檢視時。 輸出會與相同有我們提取的資料從 BLL s`SuppliersBLL`類別，但使用`StaticCache`類別會傳回做為快取在應用程式啟動的供應商資料。 您可以在 設定中斷點`StaticCache`類別的`GetSuppliers()`方法以驗證這種行為。


[![快取供應商資料會顯示在 GridView](caching-data-at-application-startup-cs/_static/image16.png)](caching-data-at-application-startup-cs/_static/image15.png)

**圖 7**: GridView 中顯示的快取供應商資料 ([按一下以檢視完整大小的影像](caching-data-at-application-startup-cs/_static/image17.png))


## <a name="summary"></a>總結

大部分的每個資料模型會包含相當多的查閱資料表的形式通常實作的靜態資料。 這項資訊是靜態的因為那里 s 這項資訊必須顯示沒有理由持續每次存取資料庫。 此外，由於其靜態的本質，快取資料時那里 s 到期不需要。 在本教學課程中，我們了解如何取得這類資料，並加以快取的資料快取中，應用程式狀態，並透過靜態成員變數的內容。 這項資訊快取在應用程式啟動，且會保留在快取整個應用程式 s 存留期。

在本教學課程和過去的兩個，我們已討論過在應用程式的存留期間快取資料，以及使用以時間為基礎的 expiries。 當快取資料庫的資料，不過，時間為基礎的到期可能不盡理想。 而不是定期清除快取，則可最佳基礎資料庫資料修改時，只能收回快取的項目。 可以透過 SQL 快取相依性，我們將檢驗在我們的下一個教學課程中使用這個理想。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Teresa Murphy 和 Zack Jones。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](caching-data-in-the-architecture-cs.md)
> [下一頁](using-sql-cache-dependencies-cs.md)
