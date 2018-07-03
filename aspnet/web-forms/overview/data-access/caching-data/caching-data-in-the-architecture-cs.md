---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: 快取資料的架構 (C#) |Microsoft Docs
author: rick-anderson
description: 在上一個教學課程中，我們學到如何適用於展示層的快取的內容。 在本教學課程中，我們了解如何善用我們的多層式 architectu 的內容...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 20c3c0cb5f3d13e66fbbceab77083c89f3015c53
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402905"
---
<a name="caching-data-in-the-architecture-c"></a>快取資料的架構 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe)或[下載 PDF](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> 在上一個教學課程中，我們學到如何適用於展示層的快取的內容。 在本教學課程中，我們了解如何利用我們的多層式架構商務邏輯層的快取資料的內容。 我們這樣做，藉由擴充的架構，若要加入的快取層。


## <a name="introduction"></a>簡介

如我們所見在先前的教學課程中，快取的 ObjectDataSource 的資料是簡單，只要設定幾個屬性。 不幸的是，ObjectDataSource 適用於展示層緊密結合與 ASP.NET 頁面的 快取原則快取。 建立多層式的架構的原因之一是允許這類中斷的聯繫。 商業邏輯層，比方說，減少商務邏輯，從 ASP.NET 頁面，而資料存取層，以減少資料存取的詳細資料。 這種商務邏輯和資料存取的詳細資料，最好，部分，因為它可讓系統更容易閱讀，更容易維護，且更有彈性變更。 它也可讓網域知識和人力資源展示層不 t 上工作的開發人員必須熟悉資料庫 s 的詳細資訊，才能執行其工作所需要的。 減少從展示層的快取原則，可提供類似的優點。

在本教學課程中，我們將增強我們架構進化為包含*快取層*（或簡稱 CL），使其利用我們的快取原則。 快取的圖層會包含`ProductsCL`類別，提供類似的方法中的產品資訊的存取權`GetProducts()`， `GetProductsByCategoryID(categoryID)`，依此類推，，叫用時，會從快取中擷取資料的第一次嘗試。 如果快取是空的這些方法會叫用適當`ProductsBLL`BLL，bll 從 DAL 接著會取得資料中的方法。 `ProductsCL`方法快取從 BLL 擷取之前將它傳回的資料。

如 [圖 1] 所示，CL 會位於展示層和商務邏輯層之間。


![快取層 (CL) 是我們的架構中的另一個圖層](caching-data-in-the-architecture-cs/_static/image1.png)

**圖 1**: 快取層 」 (CL) 是我們的架構中的另一個圖層


## <a name="step-1-creating-the-caching-layer-classes"></a>步驟 1： 建立快取層類別

在本教學課程我們會使用單一類別建立非常簡單的 CL`ProductsCL`有只有少數幾個方法。 針對整個應用程式需要建立建置完整的快取層`CategoriesCL`， `EmployeesCL`，和`SuppliersCL`類別，以及提供這些快取層類別中的方法，到 BLL 中每個資料存取或修改方法。 與 BLL 和 DAL 中，快取層應該在理想情況下會實作為個別的類別庫專案;不過，我們會做為類別中實作它`App_Code`資料夾。

從 DAL 和 BLL 類別的多個完全不同 CL 類別，可讓 s 會建立新的子資料夾中`App_Code`資料夾。 以滑鼠右鍵按一下`App_Code`資料夾，在 [方案總管] 中，選擇新的資料夾和新資料夾命名為`CL`。 建立此資料夾之後, 加入新的類別，名為`ProductsCL.cs`。


![加入新的資料夾，名為 CL 和名為 ProductsCL.cs 類別](caching-data-in-the-architecture-cs/_static/image2.png)

**圖 2**： 加入新的資料夾，名為`CL`和類別，名為 `ProductsCL.cs`


`ProductsCL`類別應該包含相同一組在其對應的商務邏輯層類別中找到的資料存取和修改方法之 (`ProductsBLL`)。 而非建立所有這些方法，讓的 s 建構 CL 幾個這裡的模式使用。 特別的是，我們會新增`GetProducts()`並`GetProductsByCategoryID(categoryID)`在步驟 3 中的方法和`UpdateProduct`在步驟 4 中的多載。 您可以將其餘`ProductsCL`方法和`CategoriesCL`， `EmployeesCL`，和`SuppliersCL`暇地的類別。

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>步驟 2： 讀取和寫入的資料快取

ObjectDataSource 快取內部前述教學課程中探索的功能會使用 ASP.NET 資料快取來儲存從 BLL 擷取的資料。 資料快取也可以存取以程式設計方式從 ASP.NET 頁面程式碼後置類別或 web 應用程式的架構中的類別。 若要讀取及寫入資料的快取，從 ASP.NET 頁面 s 程式碼後置類別，請使用下列模式：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

[ `Cache`類別](https://msdn.microsoft.com/library/system.web.caching.cache.aspx)s [ `Insert`方法](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx)有多種多載。 `Cache["key"] = value` 和`Cache.Insert(key, value)`同義，兩者都新增至使用指定的索引鍵，而不需要定義到期的快取的項目。 一般而言，我們要做為相依性、 以時間為基礎的到期，或同時快取中，加入一個項目時，請指定有效期限。 使用其中一個其他`Insert`的方法多載，來提供相依性或時間為基礎的到期資訊。

需要先檢查 如果要求的資料位於快取，若是如此，s 方法快取層會從該處將它傳回。 如果要求的資料不在快取中，適當的 BLL 方法需要叫用。 它的傳回值應該快取，並將傳回，如下列順序圖表所示。


![快取層的方法傳回的資料從快取它 s 可用](caching-data-in-the-architecture-cs/_static/image3.png)

**圖 3**: 快取層的方法傳回的資料從快取它 s 可用


圖 3 所述的順序是在 CL 類別中，使用下列模式來完成：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

在這裡，*型別*是儲存在快取中的資料類型`Northwind.ProductsDataTable`，例如時*金鑰*是可唯一識別快取項目索引鍵。 如果具有指定的項目*金鑰*不是快取中，則*執行個體*會`null`會從適當的 BLL 方法擷取資料，並且新增至快取。 依時間`return instance`達到*執行個體*包含資料，從快取的參考，或取自 BLL。

請務必從快取存取資料時，請使用上述的模式。 下列模式，其中第一眼，看起來相同，包含些微的差異，將會介紹競爭條件。 競爭情形很難進行偵錯，因為它們偶而會顯示本身並不容易重現。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

一秒的差異，不正確的程式碼片段是，而不是將快取項目的參考儲存在區域變數中，資料存取快取是直接在條件陳述式中*並*在`return`。 想像一下，當到達這個程式碼時，`Cache["key"]`是非`null`，但尚未`return`到達陳述式時，系統收回*金鑰*從快取。 在此罕見的情況下，程式碼會傳回`null`值而不是預期類型的物件。

> [!NOTE]
> 資料快取是安全執行緒，所以您不必 t 需要簡單的讀取或寫入的同步處理執行緒存取。 不過，如果您需要多個對資料執行作業的需要是不可部分完成的快取中，您必須負責實作鎖定或其他一些機制以確保執行緒安全性。 請參閱[ASP.NET 快取的同步處理存取](http://www.ddj.com/184406369)如需詳細資訊。


項目可以以程式設計方式從資料快取使用收回[`Remove`方法](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx)就像這樣：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>步驟 3： 傳回產品資訊`ProductsCL`類別

本教學課程可讓實作兩種方法來傳回產品資訊的 s`ProductsCL`類別：`GetProducts()`和`GetProductsByCategoryID(categoryID)`。 如同`ProductsBL`商業邏輯層中的類別`GetProducts()`CL 中的方法會傳回所有產品的相關資訊`Northwind.ProductsDataTable`物件，而`GetProductsByCategoryID(categoryID)`從指定的類別會傳回所有產品。

下列程式碼顯示中的方法的部分`ProductsCL`類別：


[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

首先，請記下`DataObject`和`DataObjectMethodAttribute`屬性套用至類別和方法。 這些屬性會提供資訊給 ObjectDataSource 的精靈中，指出哪些類別和方法應該會出現在精靈中的 s 步驟。 因為從展示層內的 ObjectDataSource 會存取的 CL 類別和方法，我加入了這些屬性，以增強的設計階段經驗。 回頭[建立商業邏輯層](../introduction/creating-a-business-logic-layer-cs.md)教學課程，如需更完整的描述，這些屬性和其效果。

在 `GetProducts()`並`GetProductsByCategoryID(categoryID)`方法、 從傳回的資料`GetCacheItem(key)`方法指派給區域變數。 `GetCacheItem(key)`方法，我們將檢驗，根據指定的快取從傳回的特定項目*金鑰*。 如果沒有這類資料會快取中找到，它會從對應`ProductsBLL`類別方法，然後再新增至快取使用`AddCacheItem(key, value)`方法。

`GetCacheItem(key)`和`AddCacheItem(key, value)`方法分別介面使用的資料快取、 讀取和寫入值。 `GetCacheItem(key)`方法較簡單的兩個。 它只會從快取類別使用傳入的傳回值*金鑰*:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)` 不會使用*金鑰*值，提供，而是呼叫`GetCacheKey(key)`方法，以傳回*金鑰*ProductsCache-前面加上。 `MasterCacheKeyArray`，其會保存字串 ProductsCache，也會使用`AddCacheItem(key, value)`方法，暫時如稍後所示。

從 ASP.NET 頁面 s 程式碼後置類別，資料快取可以使用來存取`Page`類別 s [ `Cache`屬性](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)，並允許以類似下面的語法`Cache["key"] = value`，步驟 2 中所述。 架構中的類別，從資料快取可使用`HttpRuntime.Cache`或`HttpContext.Current.Cache`。 [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)的部落格項目[HttpRuntime.Cache vs。HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache)資訊使用的效能優點`HttpRuntime`而不是`HttpContext.Current`; 因此，`ProductsCL`使用`HttpRuntime`。

> [!NOTE]
> 如果您的架構使用實作的類別庫專案，則您必須將參考加入`System.Web`若要使用的組件[HttpRuntime](https://msdn.microsoft.com/library/system.web.httpruntime.aspx)並[HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)類別。


如果快取中，找不到項目`ProductsCL`s 類別方法從 BLL 中取得資料，並將它新增至快取使用`AddCacheItem(key, value)`方法。 若要新增*值*至快取中，我們可以使用下列程式碼，使用 60 秒的時間到期：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)` 在未來的時間中指定以時間為基礎的到期 60 秒[ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx)表示 s，就沒有滑動期限。 雖然這`Insert`方法多載具有輸入參數的兩個絕對和滑動過期，您可以只提供兩個的其中一個。 如果您嘗試將指定的絕對時間和時間範圍內，`Insert`方法會擲回`ArgumentException`例外狀況。

> [!NOTE]
> 這個實作`AddCacheItem(key, value)`方法目前有一些缺點。 我們將位址，並解決這些問題，在步驟 4。


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>步驟 4： 失效快取資料時是架構中修改

資料擷取方法，以及快取的圖層必須提供相同的方法，BLL 的插入、 更新和刪除資料。 CL 的資料修改方法不會修改快取的資料，但而不是呼叫 BLL s 對應的資料修改的方法，然後確認 快取。 如我們在先前的教學課程中所見的這會是相同的 ObjectDataSource 適用於其快取的功能啟用時的行為及其`Insert`， `Update`，或`Delete`叫用方法。

下列`UpdateProduct`多載會說明如何實作 CL 中的資料修改方法：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

適當的資料修改商務邏輯層方法會叫用，但它的回應會傳回我們需要先確認快取。 不幸的是，快取無效並不直接了當因為`ProductsCL`類別 s`GetProducts()`並`GetProductsByCategoryID(categoryID)`方法每個項目加入快取具有不同的索引鍵，而`GetProductsByCategoryID(categoryID)`方法會將不同的快取項目加入每個唯一*categoryID*。

當快取無效，我們需要移除*所有*可能藉由加入的項目`ProductsCL`類別。 這可藉由建立關聯*快取相依性*每個項目新增至快取中`AddCacheItem(key, value)`方法。 一般情況下，快取相依性可以是另一個項目在快取中，檔案系統或將資料從 Microsoft SQL Server 資料庫上的檔案。 當相依性變更，或已從快取中移除，與其相關聯的快取項目會自動從快取收回。 本教學課程中，我們想要在快取，可透過新增快取相依性的所有項目建立額外的項目`ProductsCL`類別。 如此一來，所有這些項目可以是從快取移除只要移除快取相依性。

讓的 s update`AddCacheItem(key, value)`方法以便將每個項目新增至快取，透過這個方法是與單一快取相依性相關聯：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray` 是字串陣列，保留的 ProductsCache 的值。 首先，快取項目新增至快取，且指派的目前日期和時間。 如果快取項目已經存在，就會更新。 接下來，會建立快取相依性。 [ `CacheDependency`類別](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx)s 建構函式有幾個多載，但在這裡使用的一個必須要有兩個`string`陣列的輸入。 第一個指定檔案，可做為相依性的集合。 因為我們不想要使用任何檔案為基礎的相依性，值為`null`適用於第一個輸入參數。 第二個輸入的參數會指定將作為相依性的快取索引鍵的集合。 我們在這裡指定單一的相依性， `MasterCacheKeyArray`。 `CacheDependency`接著會傳遞至`Insert`方法。

若要進行此項修改`AddCacheItem(key, value)`invaliding、 快取很簡單，只要移除的相依性。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>步驟 5： 從展示層呼叫快取層

快取層的類別和方法可用來處理資料的技術我們 ve 檢查在這些教學課程。 為了說明使用快取的資料，您將變更儲存到`ProductsCL`類別，然後開啟`FromTheArchitecture.aspx`頁面中`Caching`資料夾，並新增 GridView。 從 GridView s 智慧標籤，建立新的 ObjectDataSource。 在精靈的 s 第一個步驟應該會看到`ProductsCL`類別做為其中一個選項，從下拉式清單。


[![在 [商務物件] 下拉式清單中包含 ProductsCL 類別](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**[圖 4**:`ProductsCL`類別會包含商務物件] 下拉式清單中 ([按一下以檢視完整大小的影像](caching-data-in-the-architecture-cs/_static/image6.png))


選取之後`ProductsCL`，按一下 [下一步]。 下拉式清單中的選取索引標籤中有兩個項目-`GetProducts()`並`GetProductsByCategoryID(categoryID)`和 [更新] 索引標籤具有唯一`UpdateProduct`多載。 選擇`GetProducts()`從 [選取] 索引標籤的方法和`UpdateProducts`方法，從 [更新] 索引標籤，然後按一下 [完成]。


[![ProductsCL 類別的方法詳列於下拉式清單](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**圖 5**:`ProductsCL`下拉式清單中列出類別的方法 ([按一下以檢視完整大小的影像](caching-data-in-the-architecture-cs/_static/image9.png))


完成精靈之後，Visual Studio 將會設定 ObjectDataSource s`OldValuesParameterFormatString`屬性設`original_{0}`並將適當的欄位新增至 GridView。 變更`OldValuesParameterFormatString`回其預設值的屬性`{0}`，及設定 GridView，以支援分頁、 排序和編輯。 因為`UploadProducts`CL 所使用的多載接受只編輯的產品的名稱和價格，限制 GridView，因此只有這些欄位是可編輯。

在先前的教學課程中，我們定義 GridView，以包含如欄位`ProductName`， `CategoryName`，和`UnitPrice`欄位。 歡迎複寫此格式和結構，在此情況下您 GridView 和 ObjectDataSource 的 s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

現在我們已使用快取層級的頁面。 若要查看動作中的快取，請在中設定中斷點`ProductsCL`類別 s`GetProducts()`和`UpdateProduct`方法。 排序和分頁才能看到資料提取從快取時，請造訪網頁瀏覽器，並逐步執行程式碼中。 然後更新記錄，並請注意，快取失效，因此，它從 BLL 時擷取資料會重新繫結至 GridView。

> [!NOTE]
> 本文所附的下載中提供的快取層會完成。 它包含一個類別， `ProductsCL`，這僅支援少數幾個方法。 此外，在單一 ASP.NET 頁面使用 CL (`~/Caching/FromTheArchitecture.aspx`) 所有其他人仍 BLL 直接參考。 如果您計劃使用 CL 應用程式中，從展示層的所有呼叫應該都前往 CL，這會要求 CL 的類別，方法會涵蓋這些類別和方法在目前由展示層 BLL。


## <a name="summary"></a>總結

雖然可以套用快取，在展示層與 ASP.NET 2.0 的 SqlDataSource 和 ObjectDataSource 控制項，在理想情況下快取責任會委派給獨立的圖層架構中。 在本教學課程中，我們建立一個快取的層級可位於展示層和商務邏輯層之間。 在快取層，就必須提供相同一組類別和存在於 BLL，以及從展示層呼叫的方法。

我們已探索這和先前的教學課程中的快取層範例展現*反應式載入*。 回應式載入，資料到快取時才會載入資料要求，而該資料遺漏從快取。 資料也可以*主動載入*到快取中，一種技術，將資料載入快取之前有實際需要的。 在下一個教學課程中我們會看到我們看看如何將靜態的值儲存至快取在應用程式啟動時主動載入範例。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Teresa Murph。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](caching-data-with-the-objectdatasource-cs.md)
> [下一頁](caching-data-at-application-startup-cs.md)
