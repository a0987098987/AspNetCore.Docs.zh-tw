---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: 快取 |Microsoft 文件
author: microsoft
description: 了解快取，請務必在執行良好的 ASP.NET 應用程式。 ASP.NET 1.x 提供三種不同的選項，為快取。輸出快取...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 90faaae75cc85585efa05e6e50eabe8c990d076e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="caching"></a>快取
====================
by [Microsoft](https://github.com/microsoft)

> 了解快取，請務必在執行良好的 ASP.NET 應用程式。 ASP.NET 1.x 提供三種不同的選項，為快取。輸出快取、 片段快取和快取 API。


了解快取，請務必在執行良好的 ASP.NET 應用程式。 ASP.NET 1.x 提供三種不同的選項，為快取。輸出快取、 片段快取和快取 API。 ASP.NET 2.0 提供所有這三種方法，但會增加一些重要的其他功能。 有數個新的快取相依性和開發人員現在可以選擇建立自訂快取相依性。 ASP.NET 2.0 中，也會大幅改善的快取設定。

## <a name="new-features"></a>新功能

## <a name="cache-profiles"></a>快取設定檔

快取設定檔可讓開發人員可以定義特定快取設定，然後可以套用至個別頁面。 比方說，如果您有數個頁面應該從快取過期，在 12 小時後，您可以輕鬆地建立可套用至這些網頁的快取設定檔。 若要加入新的快取設定檔，請使用&lt;outputCacheSettings&gt;組態檔中的區段。 例如，以下是稱為快取設定檔的組態*twoday* ，會設定快取持續時間為 12 小時。

[!code-xml[Main](caching/samples/sample1.xml)]

若要將此快取設定檔套用至特定的頁面上，使用 @ OutputCache 指示詞的 CacheProfile 屬性，如下所示：

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>自訂快取相依性

ASP.NET 1.x 開發人員哭的自訂快取相依性。 在 ASP.NET 中 1.x CacheDependency 類別已密封，予以防止的開發衍生自它自己的類別。 在 ASP.NET 2.0 中，會移除這項限制，並且開發人員可用來開發自己的自訂快取相依性。 CacheDependency 類別可允許建立檔案、 目錄或快取索引鍵為基礎的自訂快取相依性。

例如，下列程式碼會建立新的自訂快取相依性呼叫的 Web 應用程式根目錄中找到的 stuff.xml 檔案為基礎：

[!code-csharp[Main](caching/samples/sample3.cs)]

在此案例中，當 stuff.xml 檔案變更時，就會失效快取的項目。

它也可建立自訂快取相依性，使用快取索引鍵。 使用此方法時，快取索引鍵移除會導致無效的快取的資料。 下面這個範例可說明這點：

[!code-csharp[Main](caching/samples/sample4.cs)]

若要使上方插入的項目，只移除已插入快取，以做為快取索引鍵的項目。

[!code-csharp[Main](caching/samples/sample5.cs)]

請注意，做為快取索引鍵的項目索引鍵必須新增到陣列中的快取索引鍵的值相同。

## <a name="polling-based-sql-cache-dependenciesemalso-called-table-based-dependenciesem"></a>以輪詢為基礎的 SQL 快取相依性<em>（也稱為資料表為基礎的相依性）</em>

SQL Server 7 及 2000年用於 SQL 快取相依性輪詢為基礎的模型。 輪詢為基礎的模型資料表中的資料變更時觸發的資料庫資料表上使用觸發程序。 觸發更新**changeId** ASP.NET 會定期檢查通知資料表中的欄位。 如果**changeId**欄位已更新，ASP.NET 知道資料已變更，並不會使失效快取的資料。

> [!NOTE]
> SQL Server 2005 也可以使用以輪詢基礎的模型，但由於輪詢為基礎的模型不是最有效率的模型，建議使用 SQL Server 2005 中的查詢為基礎的模型 （稍後再討論）。


為了使用以輪詢基礎的模型，才能正確運作的 SQL 快取相依性，這些資料表必須有啟用通知。 達成這點可以使用 SqlCacheDependencyAdmin 類別以程式設計方式或使用 aspnet\_regsql.exe 公用程式。

下列命令列位於名為 SQL Server 執行個體上的 Northwind 資料庫中註冊的 Products 資料表*dbase* SQL 快取相依性。

[!code-console[Main](caching/samples/sample6.cmd)]

上述命令中使用的命令列選項的說明如下：

| **命令列參數** | **目的** |
| --- | --- |
| -S *server* | 指定伺服器名稱。 |
| -ed | 指定資料庫應該啟用 SQL 快取相依性。 |
| -d*資料庫\_名稱* | 指定應該啟用 SQL 快取相依性的資料庫名稱。 |
| -E | 指定該 aspnet\_regsql 連接至資料庫時，應該使用 Windows 驗證。 |
| -et | 指定我們正在啟用 SQL 快取相依性的資料庫資料表。 |
| -t *table\_name* | 指定要啟用 SQL 快取相依性的資料庫資料表名稱。 |

> [!NOTE]
> 另外還有其他參數 aspnet\_regsql.exe。 如需完整清單，請執行 aspnet\_regsql.exe-？ 從命令列。


當此命令會執行 SQL Server 資料庫會進行下列變更：

- **AspNet\_SqlCacheTablesForChangeNotification**資料表加入。 此資料表包含每個資料表已啟用 SQL 快取相依性的資料庫中的一個資料列。
- 下列預存程序會建立內部資料庫：


| AspNet\_SqlCachePollingStoredProcedure | 查詢 AspNet\_SqlCacheTablesForChangeNotification 資料表，並傳回所有資料表，啟用 SQL 快取相依性和 changeId 每個資料表的值。 這個預存程序用於輪詢來判斷資料是否已經變更。 |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | 傳回的所有資料表已啟用 SQL 快取相依性，藉由查詢 AspNet\_SqlCacheTablesForChangeNotification 資料表並傳回所有資料表已都啟用 sql 快取相依性。 |
| AspNet\_SqlCacheRegisterTableStoredProcedure | 註冊 SQL 快取相依性的資料表通知資料表中加入必要的項目，並新增觸發程序。 |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | 取消註冊 SQL 快取相依性的資料表，藉由移除通知資料表中的項目，並移除觸發程序。 |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | 更新通知資料表已變更的資料表 changeId 是遞增。 ASP.NET 會使用此值來判斷資料是否已變更。 下面所指出，此預存程序會執行觸發程序啟用的資料表時建立。 |


- SQL Server 觸發程序稱為 ***資料表\_名稱 *\_AspNet\_SqlCacheNotification\_觸發程序**建立資料表。 這個觸發程序執行 AspNet\_SqlCacheUpdateChangeIdStoredProcedure 資料表上執行插入、 更新或刪除時。
- SQL Server 角色呼叫**aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess**加入至資料庫。

**Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** SQL Server 角色具有 EXEC 權限以 AspNet\_SqlCachePollingStoredProcedure。 為了讓輪詢模型，才能正確運作，您必須加入您的處理序帳戶 aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess 角色。 Aspnet\_regsql.exe 工具將不會進行這讓您。

### <a name="configuring-polling-based-sql-cache-dependencies"></a>設定輪詢基礎 SQL 快取相依性

有幾個設定輪詢基礎 SQL 快取相依性時所需的步驟。 第一個步驟是啟用的資料庫和資料表，如同上面所討論。 一旦完成該步驟，其餘的組態如下所示：

- 設定 ASP.NET 組態檔。
- 設定 SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>設定 ASP.NET 組態檔

除了新增連接字串前, 一個模組中所述，您也必須設定&lt;快取&gt;具有項目&lt;sqlCacheDependency&gt;項目，如下所示：

[!code-xml[Main](caching/samples/sample7.xml)]

此設定可啟用 SQL 快取相依性上*pubs*資料庫。 請注意，中的 pollTime 屬性&lt;sqlCacheDependency&gt;項目的預設值為 60000 毫秒或 1 分鐘。 （此值不能小於 500 毫秒）。在此範例中，&lt;新增&gt;項目會將新的資料庫，並覆寫 pollTime，並將它設定為 9000000 毫秒為單位。

#### <a name="configuring-the-sqlcachedependency"></a>設定 SqlCacheDependency

下一個步驟是設定 SqlCacheDependency。 若要完成這個動作的最簡單的方式是，如下所示在 @ Outcache 指示詞中指定 SqlDependency 屬性的值：

[!code-aspx[Main](caching/samples/sample8.aspx)]

在上述的 @ OutputCache 指示詞，用 SQL 快取相依性，已針對*作者*資料表中*pubs*資料庫。 可以設定多個相依性，並以分號分隔就像這樣：

[!code-aspx[Main](caching/samples/sample9.aspx)]

設定 SqlCacheDependency 的另一個方法是執行這項操作以程式設計的方式。 下列程式碼上建立新的 SQL 快取相依性*作者*資料表中*pubs*資料庫。

[!code-csharp[Main](caching/samples/sample10.cs)]

其中一個以程式設計方式定義 SQL 快取相依性的優點是您可以處理可能會發生任何例外狀況。 例如，如果您嘗試定義尚未啟用通知，請為資料庫的 SQL 快取相依性**DatabaseNotEnabledForNotificationException**擲回例外狀況。 在此情況下，您可以嘗試啟用通知的資料庫，藉由呼叫**SqlCacheDependencyAdmin.EnableNotifications**方法，並將其傳遞的資料庫名稱。

同樣地，如果您嘗試定義尚未啟用通知，請為資料表的 SQL 快取相依性**TableNotEnabledForNotificationException**就會擲回。 您可以接著呼叫**SqlCacheDependencyAdmin.EnableTableForNotifications**方法將傳遞給它的資料庫名稱和資料表名稱。

下列程式碼範例說明如何設定 SQL 快取相依性時，適當地設定例外狀況處理。

[!code-csharp[Main](caching/samples/sample11.cs)]

詳細資訊： [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>以查詢為基礎的 SQL 快取相依性 (SQL Server 2005)

當使用 SQL Server 2005 SQL 快取相依性時，不需要輪詢為基礎的模型。 SQL Server 2005 搭配使用時，SQL 快取相依性進行通訊 （沒有進一步的設定是必要的） SQL Server 執行個體的 SQL 連線透過直接使用 SQL Server 2005 查詢通知。

啟用以查詢為基礎的通知的最簡單方式是藉由設定因此以宣告方式執行**SqlCacheDependency**屬性的資料來源物件以**CommandNotification**並設定**EnableCaching**屬性**true**。 使用此方法時，程式碼不需要。 如果針對資料來源變更執行命令的結果，它會將使快取資料。

下列範例會設定資料來源控制項 SQL 快取相依性：

[!code-aspx[Main](caching/samples/sample12.aspx)]

在此情況下，如果查詢中指定**SelectCommand**傳回原始未不同於它的結果時，會快取的結果會失效。

您也可以指定所有的資料來源會藉由設定啟用 SQL 快取相依性**SqlDependency**屬性**@ OutputCache**指示詞加入**CommandNotification**. 下列範例將說明這點。

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> 如需有關 SQL Server 2005 中的查詢通知的詳細資訊，請參閱 SQL Server 線上叢書 》。


設定以查詢為基礎的 SQL 快取相依性的另一個方法是以程式設計方式使用 SqlCacheDependency 類別若要這樣做。 下列程式碼範例說明如何完成此作業。

[!code-csharp[Main](caching/samples/sample14.cs)]

詳細資訊： [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>快取後替換

快取的頁面，可以大幅增加 Web 應用程式的效能。 不過，在某些情況下您需要動態網頁中的大部分的快取頁面和某些片段。 例如，如果您建立的新聞報導完全靜態網頁的設定期間內，您可以設定快取整個頁面。 如果您想要包含在每個頁面要求變更輪換廣告橫幅，包含廣告的網頁組件必須是動態。 若要讓您將快取網頁，但是動態替代的一些內容，您可以使用 ASP.NET 快取後替換。 使用快取後替換整個頁面是使用標示為不需要快取的特定組件快取的輸出。 在範例中的廣告橫幅，AdRotator 控制項可讓您利用快取後替換廣告以動態方式建立每個使用者，以及每個頁面重新整理。

有三種方式來實作快取後替換：

- 以宣告方式，使用替代控制項。
- 以程式設計的方式，使用替代控制項 API。
- 隱含地使用 AdRotator 控制項。

### <a name="substitution-control"></a>替代控制項

ASP.NET Substitution control 指定的快取的頁面，是以動態方式建立，而非快取的區段。 您在替代控制項的位置放置在頁面上您想要顯示的動態內容。 在執行階段，替代控制項會呼叫您指定的 MethodName 屬性的方法。 此方法必須傳回字串，它就會取代替代控制項的內容。 方法必須是包含的頁面或 UserControl 控制項上的靜態方法。 使用替代控制項，會造成用戶端快取性必須變更為伺服器快取性，以便將不在用戶端上快取頁面。 這可確保未來的要求頁面呼叫的方法，以產生動態內容。

### <a name="substitution-api"></a>替代的 API

若要以程式設計方式建立動態內容快取的頁面，您可以呼叫[WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx)網頁程式碼，將它當做參數傳遞的方法名稱中的方法。 處理動態內容的建立方法接受單一[HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)參數並傳回字串。 傳回的字串會是內容，其會取代在指定的位置。 呼叫 WriteSubstitution 方法，而不是以宣告方式使用替代控制項的優點是您可以呼叫任何任意物件，而不是呼叫頁面或 UserControl 物件的靜態方法的方法。

呼叫 WriteSubstitution 方法會導致用戶端快取性必須變更為伺服器快取性，以便將不在用戶端上快取頁面。 這可確保未來的要求頁面呼叫的方法，以產生動態內容。

### <a name="adrotator-control"></a>AdRotator 控制項

快取後替換內部伺服器控制項實作的 AdRotator 支援。 如果您 AdRotator 將控制項放置在頁面上，它會呈現每個要求，不論是否快取的父系頁面上的唯一廣告。 如此一來，一個網頁，內含 AdRotator 控制項是只有快取的伺服器端。

## <a name="controlcachepolicy-class"></a>ControlCachePolicy 類別

ControlCachePolicy 類別可讓您以程式設計方式控制的片段快取使用使用者控制項。 ASP.NET 會內嵌在使用者控制項[BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx)執行個體。 BasePartialCachingControl 類別代表已啟用快取輸出的使用者控制項。

當您存取[BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx)屬性[PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx)控制項，您一定會收到有效的 ControlCachePolicy 物件。 不過，如果您存取[UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx)屬性[UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx)控制項，您會收到有效 ControlCachePolicy 物件只由已包裝的使用者控制項BasePartialCachingControl 控制項。 如果並未包裝，屬性所傳回的 ControlCachePolicy 物件將會擲回例外狀況時操作它，因為它並沒有相關聯的 BasePartialCachingControl。 若要判斷 UserControl 的執行個體是否支援快取而不會產生例外狀況，檢查[SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx)屬性。

使用 ControlCachePolicy 類別是下列其中一種您可以啟用輸出快取。 下列清單描述您可用來啟用輸出快取的方法：

- 使用[@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx)指示詞，以啟用輸出快取中宣告的案例。
- 使用[PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx)啟用快取做為程式碼後置檔案中的使用者控制項的屬性。
- 使用您已啟用快取使用先前的方法之一並以動態方式載入使用BasePartialCachingControl執行個體使用的程式設計案例中指定快取設定ControlCachePolicy類別[System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx)方法。

ControlCachePolicy 執行個體可以成功操作只會控制生命週期的 Init 和 PreRender 階段之間。 如果您修改 ControlCachePolicy 物件 PreRender 階段之後，ASP.NET 擲回例外狀況，因為之後要呈現控制項所做的任何變更實際上不會影響快取設定 （在呈現階段期間，會快取控制項）。 最後，使用者控制項的執行個體 （以及其 ControlCachePolicy 物件） 適用於以程式設計方式操作時才會真的呈現。

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>變更為快取組態-&lt;快取&gt;項目

有數個 ASP.NET 2.0 中的快取組態變更。 &lt;快取&gt;ASP.NET 2.0 的新項目，並可讓您在組態檔中進行快取的組態變更。 使用下列屬性。

| **目** | **描述** |
| --- | --- |
| **cache** | 選擇性項目。 定義通用的應用程式快取設定。 |
| **outputCache** | 選擇性項目。 指定整個應用程式的輸出快取設定。 |
| **outputCacheSettings** | 選擇性項目。 指定可以套用至應用程式中的頁面輸出快取設定。 |
| **sqlCacheDependency** | 選擇性項目。 設定 ASP.NET 應用程式的 SQL 快取相依性。 |

### <a name="the-ltcachegt-element"></a>&lt;快取&gt;項目

中的下列屬性可用&lt;快取&gt;項目：

| **屬性** | **描述** |
| --- | --- |
| **disableMemoryCollection** | 選擇性**布林**屬性。 取得或設定值，指出是否要停用電腦記憶體不足的壓力時，就會發生快取記憶體回收。 |
| **disableExpiration** | 選擇性**布林**屬性。 取得或設定值，指出是否要停用快取逾期。 停用時，快取項目不會過期，背景清除已過期的快取項目不會發生。 |
| **privateBytesLimit** | 選擇性**Int64**屬性。 取得或設定值，表示快取開始排清之前的應用程式私用位元組的大小上限過期的項目，並嘗試回收記憶體。 這項限制包含在快取所使用的記憶體以及從執行中應用程式的額外負荷一般記憶體。 設為零時，表示 ASP.NET 會使用它自己的啟發學習法，以判斷何時開始回收記憶體。 |
| **percentagePhysicalMemoryUsedLimit** | 選擇性**Int32**屬性。 取得或設定值，表示快取開始排清之前可以由應用程式的機器的實體記憶體的最大百分比過期的項目，並嘗試回收此記憶體使用量會包含在快取所使用的兩個記憶體的記憶體為執行中應用程式的一般記憶體使用量。 設為零時，表示 ASP.NET 會使用它自己的啟發學習法，以判斷何時開始回收記憶體。 |
| **privateBytesPollTime** | 選擇性**TimeSpan**屬性。 取得或設定值，指出應用程式的私用位元組記憶體使用量的輪詢之間的時間間隔。 |

### <a name="the-ltoutputcachegt-element"></a>&lt;OutputCache&gt;項目

下列屬性可供&lt;outputCache&gt;項目。


|       <strong>屬性</strong>        |                                                                                                                                                                                                                                                       <strong>描述</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          選擇性<strong>布林</strong>屬性。 啟用/停用頁面輸出快取。 如果停用，不會快取不論以程式設計方式或宣告式設定。 預設值是<strong>true</strong>。                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                選擇性<strong>布林</strong>屬性。 啟用/停用應用程式片段快取。 如果停用，頁面會快取，不論[@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx)指示詞或快取使用的設定檔。 包含快取控制標頭，指出，上游 proxy 伺服器，以及瀏覽器用戶端不應嘗試快取頁面輸出。 預設值是<strong>false</strong>。                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      選擇性<strong>布林</strong>屬性。 取得或設定值，指出是否<strong>快取的控制項： 私用</strong>標頭傳送預設輸出快取模組。 預設值是<strong>false</strong>。                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | 選擇性<strong>布林</strong>屬性。 啟用/停用 Http 傳送 「<strong>Vary: \<n g ><em>「 回應中的標頭。預設值為 false，使用"</em>* Vary: \* <strong>"標頭傳送的輸出快取頁面。Vary 標頭傳送時，它可讓您不同快取的版本為基礎的 Vary 標頭中指定的內容。例如， <em>Vary： 使用者-代理程式</em>會儲存不同版本的頁面，根據發出要求的使用者代理程式。預設值是 * * false</strong>。 |

### <a name="the-ltoutputcachesettingsgt-element"></a>&lt;OutputCacheSettings&gt;項目

&lt;OutputCacheSettings&gt;項目可讓如先前所述的快取設定檔的建立。 唯一子系項目&lt;outputCacheSettings&gt;項目是&lt;outputCacheProfiles&gt;項目來設定快取設定檔。

### <a name="the-ltsqlcachedependencygt-element"></a>&lt;SqlCacheDependency&gt;項目

下列屬性可供&lt;sqlCacheDependency&gt;項目。

| **屬性** | **描述** |
| --- | --- |
| **enabled** | 需要**布林**屬性。 表示輪詢變更。 |
| **pollTime** | 選擇性**Int32**屬性。 設定與 SqlCacheDependency 輪詢有變更的資料庫資料表的頻率。 此值對應至連續輪詢之間的毫秒數。 它不能設定為小於 500 毫秒。 預設值為 1 分鐘。 |

### <a name="more-information"></a>更多資訊

還有一些您應該了解有關快取組態的額外資訊。

- 如果未設定背景工作處理序的私用位元組限制，快取會使用其中一個下列限制： 

    - x86 2 GB: 800 MB 或 60%的實體 RAM 少於
    - x86 3 GB: 1800 MB 或 60%的實體 RAM 少於
    - x 64: 1 tb 或 60%的實體 RAM 少於
- 如果這兩個工作者處理序私用位元組限制和&lt;快取 privateBytesLimit /&gt;設定快取將會使用兩個最小值。
- 就像在 1.x 我們卸除快取項目並呼叫 GC。收集兩個原因： 

    - 我們也非常接近私用位元組的上限
    - 可用的記憶體是接近 100 或小於 10%
- 您可以有效地停用修剪，並且藉由設定快取可用記憶體不足狀況&lt;快取 percentagePhysicalMemoryUseLimit /&gt;到 100 之間。
- 不同於 1.x 2.0 會擱置修剪並收集呼叫如果上次的 GC。收集不能降低私用位元組或多個 1%（快取） 記憶體限制的 managed 堆積的大小。

## <a name="lab1-custom-cache-dependencies"></a>Lab1： 自訂快取相依性

1. 建立新的網站。
2. 加入新的 XML 檔案，稱為 cache.xml，並將它儲存到 Web 應用程式的根目錄。
3. 下列程式碼加入至頁面\_載入程式碼後置的 default.aspx 中的方法： 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Default.aspx 來源檢視中的最上方加入下列： 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. 瀏覽 Default.aspx。 時間說什麼？
6. 重新整理瀏覽器。 時間說什麼？
7. 開啟 cache.xml 並加入下列程式碼： 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. 儲存 cache.xml。
9. 重新整理瀏覽器。 時間說什麼？
10. 說明原因而不是顯示先前的快取的值更新的時間：

## <a name="lab-2-using-polling-based-cache-dependencies"></a>實驗室 2： 使用輪詢快取相依性

這個實驗室會使用您在前一個模組，允許編輯 GridView 和 DetailsView 控制項透過 Northwind 資料庫中的資料中建立的專案。

1. 在 Visual Studio 2005 中開啟專案。
2. 執行 aspnet\_regsql 對 Northwind 資料庫，若要啟用資料庫和 [產品] 資料表的公用程式。 使用下列命令從 Visual Studio 命令提示字元： 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. 將下列加入至您的 web.config 檔案： 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. 加入新的 webform 呼叫 showdata.aspx。
5. 將下列 @ outputcache 指示詞加入至 showdata.aspx 頁面： 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. 下列程式碼加入至頁面\_showdata.aspx 負載： 

    [!code-html[Main](caching/samples/sample21.html)]
7. 將新的 SqlDataSource 控制項加入至 showdata.aspx，並將它設定為使用 Northwind 資料庫連接。 按一下 [下一步]。
8. 選取的產品名稱 」 和 「 ProductID 核取方塊，然後按一下 [下一步]。
9. 按一下 [完成]。
10. 將新的 GridView 加入 showdata.aspx 頁面。
11. 從下拉式清單中選擇 SqlDataSource1。
12. 儲存並瀏覽 showdata.aspx。 記下顯示的時間。
