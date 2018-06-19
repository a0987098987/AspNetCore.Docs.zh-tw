---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: 快取資料中的架構 (C#) |Microsoft 文件
author: rick-anderson
description: 在上一個教學課程中，我們學到如何套用展示層的快取的內容。 本教學課程中我們會了解如何利用我們的分層 architectu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 9ca91ecdaed536fe69196e0f726138590d7a9b77
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879214"
---
<a name="caching-data-in-the-architecture-c"></a>快取資料中的架構 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe)或[下載 PDF](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> 在上一個教學課程中，我們學到如何套用展示層的快取的內容。 在本教學課程中，我們了解如何利用我們多層式架構商務邏輯層的快取資料。 執行此作業，延伸架構，用來加入的快取的圖層。


## <a name="introduction"></a>簡介

如我們所見前述教學課程中，快取 ObjectDataSource 的資料是只要設定幾個屬性。 不幸的是，適用於 ObjectDataSource，展示層緊密結合的 ASP.NET 網頁的快取原則快取。 其中一個原因而建立多層式的架構可讓這類聯繫損毀。 資料存取層，以減少資料的存取詳細資料時，商務邏輯層比方說，以減少從 ASP.NET 頁面的商務邏輯。 此種減少方式的商務邏輯和資料存取的詳細資料的慣用的部分，因為它會讓系統更容易閱讀，更易於維護，而且更有彈性來變更。 它也可讓定義域知識和人力資源展示層不 t 上工作的開發人員必須熟悉資料庫 s 詳細資料，才能執行其工作所需要的。 從展示層的快取原則的去耦合提供相似的效益。

本教學課程中我們將會增加我們架構，用來包含*快取層*（或簡稱 CL），使其利用我們的快取原則。 快取的圖層會包含`ProductsCL`提供類似的方法中的產品資訊的存取權的類別`GetProducts()`， `GetProductsByCategoryID(categoryID)`，依此類推，叫用時，會從快取中擷取資料的第一次嘗試。 如果快取是空的這些方法會叫用適當`ProductsBLL`BLL，接著會從 DAL 取得資料中的方法。 `ProductsCL`方法快取擷取從 BLL 再加以傳回的資料。

如圖 1 所示，CL 會位於簡報和商務邏輯層之間。


![快取層 (CL) 是另一個圖層，在我們的架構](caching-data-in-the-architecture-cs/_static/image1.png)

**圖 1**: 快取層 (CL) 是另一個圖層，在我們的架構


## <a name="step-1-creating-the-caching-layer-classes"></a>步驟 1： 建立快取層的類別

本教學課程中我們將使用單一類別建立非常簡單 CL`ProductsCL`具有少數幾個方法。 建置完成的快取層針對整個應用程式會需要建立`CategoriesCL`， `EmployeesCL`，和`SuppliersCL`類別，以及提供方法，這些快取層類別中的 BLL 中每個資料存取或修改方法。 如同 BLL 和 DAL，快取的圖層應該在理想情況下實作為個別的類別庫專案。不過，我們會做為類別中實作它`App_Code`資料夾。

自 DAL 和 BLL 類別的多個完全不同 CL 類別，讓 s 建立新的子資料夾中`App_Code`資料夾。 以滑鼠右鍵按一下`App_Code`資料夾在 [方案總管] 中，選擇新的資料夾，並將新的資料夾命名`CL`。 建立這個資料夾之後, 加入新的類別，名為`ProductsCL.cs`。


![加入新的資料夾，名為 CL 和名為 ProductsCL.cs 類別](caching-data-in-the-architecture-cs/_static/image2.png)

**圖 2**： 加入新的資料夾，名為`CL`和類別，名為 `ProductsCL.cs`


`ProductsCL`類別應包含相同的資料存取和修改方法，在其對應的商務邏輯層類別集 (`ProductsBLL`)。 而不是建立所有這些方法，可讓的 s 建構 CL 幾個這裡的操作有初步的模式使用。 特別是，我們會將新增`GetProducts()`和`GetProductsByCategoryID(categoryID)`步驟 3 中的方法和`UpdateProduct`步驟 4 中的多載。 您可以加入其餘`ProductsCL`方法和`CategoriesCL`， `EmployeesCL`，和`SuppliersCL`在閒暇類別。

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>步驟 2： 讀取和寫入資料快取

ObjectDataSource 快取內部瀏覽先前的教學課程中的功能會使用 ASP.NET 資料快取來儲存從 BLL 擷取的資料。 資料快取也可以存取以程式設計方式從 ASP.NET 網頁程式碼後置類別或 web 應用程式的架構中的類別。 若要讀取和寫入資料的快取，從 ASP.NET 頁面 s 程式碼後置類別，請使用下列模式：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

[ `Cache`類別](https://msdn.microsoft.com/library/system.web.caching.cache.aspx)s [ `Insert`方法](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx)的數目多載。 `Cache["key"] = value` 和`Cache.Insert(key, value)`同義，同時將項目至快取使用未定義的到期的指定的金鑰。 一般而言，我們想要指定到期時將項目加入快取中，當做相依性、 以時間為基礎的到期，或兩者。 使用其中一個其他`Insert`的方法多載，以提供相依性或時間為基礎的過期資訊。

快取層需要先檢查 如果要求的資料會快取中，若是如此，s 方法會傳回它從該處。 如果要求的資料不是快取中，適當的 BLL 方法需要叫用。 其傳回的值應該快取，並將傳回，如下列順序圖表所示。


![快取層的方法會傳回從快取資料如果它 s 可用](caching-data-in-the-architecture-cs/_static/image3.png)

**圖 3**: 快取層的方法會傳回從快取資料如果它 s 可用


圖 3 所示的順序是在 CL 類別中使用以下模式來完成：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

在這裡，*類型*是快取中所儲存的資料型別`Northwind.ProductsDataTable`，例如時*金鑰*是索引鍵可唯一識別快取項目。 如果具有指定的項目*金鑰*不在快取，然後*執行個體*將`null`會適當 BLL 方法所擷取的資料，並且新增至快取。 依時間`return instance`為止*執行個體*包含資料，從快取的參考，或從 BLL 提取。

請務必從快取中存取資料時，使用上述的模式。 下列的模式，其中第一眼看起來，看起來相同，包含種微妙的差異，將會介紹競爭情形。 競爭情形很難進行偵錯，因為它們偶發性顯示本身，而且難以重現。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

一秒的差異，不正確的程式碼片段，而不將快取項目的參考儲存在本機變數，資料存取快取是直接在條件陳述式中*和*中`return`。 當到達這個程式碼時，假設，`Cache["key"]`為非`null`前,`return`到達陳述式時，系統會收回*金鑰*從快取。 在此罕見的情況下，程式碼將傳回`null`值而不是預期類型的物件。

> [!NOTE]
> 資料快取是安全執行緒，所以您不必 t 需要簡單的讀取或寫入進行同步處理的執行緒存取。 不過，如果您需要執行多個作業資料必須是不可部分完成的快取中，您必須負責實作鎖定或其他一些機制來確保執行緒安全。 請參閱[ASP.NET 快取的同步處理存取](http://www.ddj.com/184406369)如需詳細資訊。


項目可以透過程式設計方式從資料快取使用收回[`Remove`方法](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx)就像這樣：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>步驟 3： 傳回產品資訊`ProductsCL`類別

針對本教學課程將實作兩個方法來傳回產品資訊的 s`ProductsCL`類別：`GetProducts()`和`GetProductsByCategoryID(categoryID)`。 像使用`ProductsBL`商務邏輯層中的類別`GetProducts()`CL 中的方法會傳回所有產品做為資訊`Northwind.ProductsDataTable`物件，而`GetProductsByCategoryID(categoryID)`從指定的類別目錄都傳回所有產品。

下列程式碼顯示的方法中的某一部分`ProductsCL`類別：


[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

首先，請記下`DataObject`和`DataObjectMethodAttribute`屬性套用至類別和方法。 這些屬性會提供資訊給 ObjectDataSource 的精靈中，表示哪些類別和方法應該會出現在精靈中的 s 步驟。 由於會從 ObjectDataSource 展示層中存取的 CL 類別和方法，我會加入這些屬性，以增強的設計階段經驗。 若要回頭參考[建立商務邏輯層](../introduction/creating-a-business-logic-layer-cs.md)如需更完整的說明，這些屬性和其效果教學課程。

在`GetProducts()`和`GetProductsByCategoryID(categoryID)`方法、 從傳回的資料`GetCacheItem(key)`方法指派給本機變數。 `GetCacheItem(key)`方法，我們將檢驗，傳回特定的項目從快取，根據指定*金鑰*。 如果沒有這類資料快取中找到，它會從對應`ProductsBLL`類別方法，並加入快取使用`AddCacheItem(key, value)`方法。

`GetCacheItem(key)`和`AddCacheItem(key, value)`方法分別介面與資料快取、 讀取和寫入值。 `GetCacheItem(key)`方法較簡單的兩個。 它只會從快取類別使用傳入的傳回值*金鑰*:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)` 不會使用*金鑰*值加以提供，而是呼叫`GetCacheKey(key)`方法，這個方法會傳回*金鑰*ProductsCache-前面加上。 `MasterCacheKeyArray`，其中保留字串 ProductsCache，也會使用`AddCacheItem(key, value)`方法中，我們將會立即看到。

從 ASP.NET 頁面 s 程式碼後置類別，讓資料快取可使用存取`Page`類別 s [ `Cache`屬性](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)，並允許類似下面的語法`Cache["key"] = value`，如步驟 2 所述。 架構中的類別，從資料快取可使用`HttpRuntime.Cache`或`HttpContext.Current.Cache`。 [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)的部落格項目[HttpRuntime.Cache vs。HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache)資訊使用的輕微的效能優點`HttpRuntime`而不是`HttpContext.Current`; 因此，`ProductsCL`使用`HttpRuntime`。

> [!NOTE]
> 如果您的架構使用實作的類別庫專案，則您必須將參考加入`System.Web`組件，才能使用[HttpRuntime](https://msdn.microsoft.com/library/system.web.httpruntime.aspx)和[HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)類別。


如果快取中，找不到項目`ProductsCL`類別的方法 BLL 從取得資料，並將它加入至快取使用`AddCacheItem(key, value)`方法。 若要加入*值*至快取中，我們可以使用下列程式碼會使用 60 秒鐘的時間到期：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)` 指定以時間為基礎的到期日期 60 秒的未來 while [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx)指出那里 s 沒有滑動期限。 雖然這`Insert`方法多載具有輸入參數的兩個絕對和滑動過期，您可以只提供兩個的其中一個。 如果您嘗試將指定的絕對時間和時間範圍內，`Insert`方法會擲回`ArgumentException`例外狀況。

> [!NOTE]
> 這項實作`AddCacheItem(key, value)`方法目前有一些缺點。 我們將地址，並克服這些問題，在步驟 4。


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>步驟 4： 失效快取時資料是透過架構，修改

資料擷取方法，以及快取的圖層必須提供相同的方法，BLL 插入、 更新和刪除資料。 CL 的資料修改方法不會修改快取的資料，但而不是呼叫 BLL s 對應資料修改方法，然後確認 快取。 因為我們在先前的教學課程中所見，這是相同 ObjectDataSource 適用於其快取的功能啟用時的行為及其`Insert`， `Update`，或`Delete`叫用方法。

下列`UpdateProduct`多載說明如何實作 CL 中的資料修改方法：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

適當的資料修改商務邏輯層方法會叫用，但會傳回其回應，我們需要先確認快取。 不幸的是，快取無效，所以無法直接`ProductsCL`類別 s`GetProducts()`和`GetProductsByCategoryID(categoryID)`方法每個項目加入快取具有不同索引鍵，而`GetProductsByCategoryID(categoryID)`方法會將不同的快取項目加入每個唯一*categoryID*。

當快取無效，我們需要移除*所有*可能藉由加入的項目`ProductsCL`類別。 這可藉由建立關聯*快取相依性*加入至快取中的每個項目與`AddCacheItem(key, value)`方法。 一般情況下，快取相依性可以是另一個項目在快取中，檔案系統或從 Microsoft SQL Server 資料庫的資料上的檔案。 當相依性變更或移除從快取，其相關聯的快取項目會自動從快取收回。 此教學課程中，我們想要建立額外的項目會作為透過新增快取相依性的所有項目在快取中`ProductsCL`類別。 這樣一來，所有這些項目可以會從快取只要移除快取相依性。

可讓的 s 更新`AddCacheItem(key, value)`方法，透過這個方法新增至快取的每個項目都與單一快取相依性：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray` 是保存的 ProductsCache 的值的字串陣列。 首先，快取項目加入至快取，並指派給目前的日期和時間。 如果快取項目已經存在，它會更新。 接下來，會建立快取相依性。 [ `CacheDependency`類別](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx)s 建構函式有多種多載，但在這裡使用的一個必須要有兩個`string`陣列的輸入。 第一個指定檔案，可做為相依性的集合。 因為我們不想使用以檔案為基礎的相依性，值為`null`用於第一個輸入參數。 第二個輸入的參數會指定要做為相依性的快取索引鍵集。 我們在這裡指定我們的單一相依性， `MasterCacheKeyArray`。 `CacheDependency`然後傳入`Insert`方法。

若要進行此項修改`AddCacheItem(key, value)`invaliding、 快取很簡單，只移除相依性。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>步驟 5： 從展示層中呼叫的快取的圖層

使用資料使用的技術，我們 ve 檢查這些教學課程整個可用的快取層的類別和方法。 為了說明使用快取資料，您將變更儲存到`ProductsCL`類別，然後開啟`FromTheArchitecture.aspx`頁面`Caching`資料夾並加入 GridView。 從 GridView s 智慧標籤，建立新的 ObjectDataSource。 在精靈的 s 第一個步驟應該會看到`ProductsCL`類別做為其中一個選項，從下拉式清單。


[![在 [商務物件] 下拉式清單中包含 ProductsCL 類別](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**圖 4**:`ProductsCL`商務物件 下拉式清單中包含類別 ([按一下以檢視完整大小的影像](caching-data-in-the-architecture-cs/_static/image6.png))


選取之後`ProductsCL`，按一下 [下一步]。 下拉式清單選取索引標籤中的有兩個項目-`GetProducts()`和`GetProductsByCategoryID(categoryID)`和 [更新] 索引標籤具有唯一`UpdateProduct`多載。 選擇`GetProducts()`方法，從 [選取] 索引標籤和`UpdateProducts`方法，從 [更新] 索引標籤，然後按一下 [完成]。


[![下拉式清單中列出這些 ProductsCL 類別的方法](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**圖 5**:`ProductsCL`下拉式清單中會列出類別的方法 ([按一下以檢視完整大小的影像](caching-data-in-the-architecture-cs/_static/image9.png))


完成精靈之後，Visual Studio 會將 ObjectDataSource s`OldValuesParameterFormatString`屬性`original_{0}`並將適當的欄位加入至 GridView。 變更`OldValuesParameterFormatString`回其預設值的屬性`{0}`，並設定以支援分頁、 排序和編輯 GridView。 因為`UploadProducts`CL 所使用的多載會接受只編輯的產品的名稱和價格，限制 GridView，因此只有這些欄位是可編輯。

在前述教學課程中，我們定義包含欄位的 GridView `ProductName`， `CategoryName`，和`UnitPrice`欄位。 歡迎複寫此格式和結構，在此情況下您 GridView 和 ObjectDataSource 的 s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

現在我們有會使用快取層的頁面。 若要查看作用中的快取，在中設定中斷點`ProductsCL`類別 s`GetProducts()`和`UpdateProduct`方法。 排序和分頁才能看到資料提取從快取，請造訪網頁瀏覽器，並逐步執行程式碼中。 然後更新的記錄，並請注意，快取失效，因此，它會從 BLL 當資料已不再重新繫結至 GridView。

> [!NOTE]
> 本文所附的下載中提供的快取層會完成。 它包含一個類別， `ProductsCL`，其中只有運動少數幾個方法。 此外，在單一 ASP.NET 頁面使用 CL (`~/Caching/FromTheArchitecture.aspx`) 其他所有項目仍 BLL 直接參考。 如果您計劃應用程式中使用 CL，展示層的所有呼叫應該都移至 CL，即需要，CL 的類別，並方法涵蓋這些類別和 BLL 展示層目前使用中的方法。


## <a name="summary"></a>總結

雖然快取可以套用在與 ASP.NET 2.0 的 SqlDataSource 展示層和 ObjectDataSource 控制項，在理想情況下快取責任會委派至個別圖層的架構中。 在本教學課程中，我們建立快取的圖層位於展示層與商務邏輯層之間。 快取的圖層必須提供相同一組類別和 BLL 存在於和展示層從呼叫的方法。

我們在此範例與上述的教學課程已探索的快取層範例展現*反應式載入*。 以反應靈敏的載入資料至快取時才會載入提出資料要求，並從快取該資料會遺失。 資料也可以是*主動載入*至快取中，一種技術，將資料載入快取之前有實際需要的。 在下一個教學課程中我們會看到主動式載入時，我們會審視如何儲存到應用程式啟動時快取的靜態值的範例。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已本文 Murph。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](caching-data-with-the-objectdatasource-cs.md)
> [下一頁](caching-data-at-application-startup-cs.md)
