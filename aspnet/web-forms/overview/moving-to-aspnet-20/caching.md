---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: 快取 |Microsoft Docs
author: microsoft
description: 了解快取是很重要的一個良好的 ASP.NET 應用程式。 ASP.NET 1.x 提供三種不同的選項，進行快取輸出快取...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 0c38092c47060e6d02791f9672df6703852f4b5a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829482"
---
<a name="caching"></a>快取
====================
by [Microsoft](https://github.com/microsoft)

> 了解快取是很重要的一個良好的 ASP.NET 應用程式。 ASP.NET 1.x 提供三種不同的選項，進行快取輸出快取、 片段快取和快取 API。


了解快取是很重要的一個良好的 ASP.NET 應用程式。 ASP.NET 1.x 提供三種不同的選項，進行快取輸出快取、 片段快取和快取 API。 ASP.NET 2.0 提供所有這三種方法，但它會加入一些重要的其他功能。 有數個新的快取相依性和開發人員現在可以選擇建立自訂快取相依性。 在 ASP.NET 2.0 中，也大幅改善的快取設定。

## <a name="new-features"></a>新功能

## <a name="cache-profiles"></a>快取設定檔

快取設定檔可讓開發人員定義特定的快取設定，然後可以套用到個別的頁面。 比方說，如果您有應該在 12 小時後過期從快取某些頁面，您可以輕鬆地建立可以套用到該些頁面的快取設定檔。 若要新增新的快取設定檔，請使用&lt;outputCacheSettings&gt;組態檔中的區段。 例如，以下是為快取設定檔的組態*twoday* ，會設定為 12 小時的快取期間。

[!code-xml[Main](caching/samples/sample1.xml)]

若要將此快取設定檔套用到特定頁面，請使用 @ OutputCache 指示詞的 CacheProfile 屬性，如下所示：

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>自訂快取相依性

ASP.NET 1.x 開發人員哭自訂快取相依性。 在 ASP.NET 1.x 中，CacheDependency 類別已密封予以防止開發人員從它衍生自己的類別。 在 ASP.NET 2.0 中，會移除這項限制，並且開發人員可用來開發自己的自訂快取相依性。 CacheDependency 類別可以讓您建立檔案、 目錄或快取索引鍵為基礎的自訂快取相依性。

例如，下列程式碼會建立新的自訂快取相依性，根據稱為 stuff.xml Web 應用程式的根目錄中找到的檔案：

[!code-csharp[Main](caching/samples/sample3.cs)]

在此案例中，當 stuff.xml 檔案變更時，就會失效的快取的項目。

它也可建立自訂快取相依性，使用快取索引鍵。 使用此方法，移除快取索引鍵會快取的資料失效。 下面這個範例可說明這點：

[!code-csharp[Main](caching/samples/sample4.cs)]

若要使其失效的上方插入的項目，只要移除插入至快取，以做為快取索引鍵的項目。

[!code-csharp[Main](caching/samples/sample5.cs)]

請注意，做為快取索引鍵之項目的索引鍵必須是加入至快取金鑰的陣列中的值相同。

## <a name="polling-based-sql-cache-dependenciesemalso-called-table-based-dependenciesem"></a>輪詢為基礎的 SQL 快取相依性<em>（也稱為資料表為基礎的相依性）</em>

SQL Server 7 和 2000年使用 SQL 快取相依性輪詢為基礎的模型。 輪詢為基礎的模型會使用觸發程序會在資料表中的資料變更時，會觸發資料庫資料表。 觸發更新**changeId** ASP.NET 會定期檢查通知資料表中的欄位。 如果**changeId**已更新欄位，ASP.NET 會知道的資料已變更，而且它失效的快取的資料。

> [!NOTE]
> SQL Server 2005 也可以使用輪詢為基礎的模型，但因為輪詢為基礎的模型不是最有效率的模型，建議您最好使用 SQL Server 2005 中的查詢為基礎的模型 （稍後討論）。


為了讓 SQL 快取的相依性，才能正確使用輪詢為基礎的模型，這些資料表必須有已啟用的通知。 這可以透過程式設計方式使用 SqlCacheDependencyAdmin 類別來完成或使用 aspnet\_regsql.exe 公用程式。

下列的命令列上的 SQL Server 執行個體，名為 Northwind 資料庫中註冊的 Products 資料表*dbase* sql 快取相依性。

[!code-console[Main](caching/samples/sample6.cmd)]

上述命令中使用命令列參數的說明如下：

| **命令列參數** | **目的** |
| --- | --- |
| -S *server* | 指定伺服器名稱。 |
| -ed | 指定資料庫，應該啟用 SQL 快取相依性。 |
| -d*資料庫\_名稱* | 指定應該啟用 SQL 快取相依性的資料庫名稱。 |
| -E | 指定該 aspnet\_regsql 在連線至資料庫時，應該使用 Windows 驗證。 |
| -et | 指定我們要啟用 SQL 快取相依性的資料庫資料表。 |
| -t*資料表\_名稱* | 指定要啟用 SQL 快取相依性的資料庫資料表名稱。 |

> [!NOTE]
> 有其他參數可用來 aspnet\_regsql.exe。 如需完整的清單中，執行 aspnet\_regsql.exe-嗎？ 從命令列。


當此命令會執行下列變更系統會對 SQL Server 資料庫中：

- **AspNet\_SqlCacheTablesForChangeNotification**資料表會加入。 此資料表包含每個資料表已啟用 SQL 快取相依性的資料庫中的一個資料列。
- 下列的預存程序會建立內部資料庫：


| AspNet\_SqlCachePollingStoredProcedure | 查詢 AspNet\_SqlCacheTablesForChangeNotification 資料表，並傳回所有的資料表已啟用 SQL 快取相依性和 changeId 每個資料表的值。 此預存程序用於輪詢以判斷資料是否已變更。 |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | 傳回的所有資料表已啟用 SQL 快取相依性，藉由查詢 AspNet\_SqlCacheTablesForChangeNotification 資料表並傳回所有的資料表已啟用 sql 快取相依性。 |
| AspNet\_SqlCacheRegisterTableStoredProcedure | 註冊 SQL 快取相依性的資料表通知資料表中加入必要的項目，並新增觸發程序。 |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | 藉由移除通知資料表中的項目會取消註冊 SQL 快取相依性的資料表，並移除觸發程序。 |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | 藉由遞增已變更的資料表 changeId 更新通知資料表。 ASP.NET 會使用此值來判斷資料是否已變更。 如下所示，此預存程序會執行觸發程序啟用資料表時建立。 |


- SQL Server 觸發程序呼叫 ***表格\_名稱 *\_AspNet\_SqlCacheNotification\_觸發程序**建立資料表。 此觸發程序執行 AspNet\_SqlCacheUpdateChangeIdStoredProcedure 資料表上執行的 INSERT、 UPDATE 或 DELETE 時。
- SQL Server 角色稱為**aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess**加入至資料庫。

**Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** SQL Server 角色具有 EXEC 權限，以 AspNet\_SqlCachePollingStoredProcedure。 為了讓輪詢模型才能正常運作，必須將您的處理序帳戶新增至 aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess 角色。 Aspnet\_regsql.exe 工具不會為您完成。

### <a name="configuring-polling-based-sql-cache-dependencies"></a>設定輪詢為基礎的 SQL 快取相依性

有幾個步驟所需的設定以輪詢為基礎的 SQL 快取相依性。 第一個步驟是啟用的資料庫和資料表，如上面所討論。 一旦完成該步驟，其餘的設定如下所示：

- 設定 ASP.NET 組態檔。
- 設定 SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>設定 ASP.NET 組態檔

除了新增連接字串，在前一個模組中所述，您也必須設定&lt;快取&gt;項目&lt;sqlCacheDependency&gt;項目，如下所示：

[!code-xml[Main](caching/samples/sample7.xml)]

此組態上啟用 SQL 快取相依性*pubs*資料庫。 請注意，屬性上 pollTime &lt;sqlCacheDependency&gt;項目的預設值為 60000 毫秒或 1 分鐘。 （此值不能小於 500 毫秒）。在此範例中，&lt;新增&gt;項目加入新的資料庫，並覆寫 pollTime，並將它設定為 9000000 毫秒為單位。

#### <a name="configuring-the-sqlcachedependency"></a>設定 SqlCacheDependency

下一個步驟是設定 SqlCacheDependency。 達到此目的的最簡單方式是，如下所示在 @ Outcache 指示詞中指定的值的 SqlDependency 屬性：

[!code-aspx[Main](caching/samples/sample8.aspx)]

在上述的 @ OutputCache 指示詞，用 SQL 快取相依性，已針對*作者*資料表中*pubs*資料庫。 以分號分隔，可以設定多個相依性就像這樣：

[!code-aspx[Main](caching/samples/sample9.aspx)]

若要這樣做以程式設計的方式，是設定 SqlCacheDependency 的另一個方法。 下列程式碼上建立新的 SQL 快取相依性*作者*資料表中*pubs*資料庫。

[!code-csharp[Main](caching/samples/sample10.cs)]

以程式設計方式定義 SQL 快取相依性的優點之一是您可以處理任何可能發生的例外狀況。 例如，如果您嘗試定義尚未啟用通知，資料庫的 SQL 快取相依性**DatabaseNotEnabledForNotificationException**會擲回例外狀況。 在此情況下，您可以嘗試啟用通知的資料庫，藉由呼叫**SqlCacheDependencyAdmin.EnableNotifications**方法並傳遞它的資料庫名稱。

同樣地，如果您嘗試定義尚未啟用通知，請為資料表的 SQL 快取相依性**TableNotEnabledForNotificationException**就會擲回。 您可以接著呼叫**SqlCacheDependencyAdmin.EnableTableForNotifications**方法並傳遞它的資料庫名稱和資料表名稱。

下列程式碼範例說明如何設定 SQL 快取相依性時，適當地設定例外狀況處理。

[!code-csharp[Main](caching/samples/sample11.cs)]

詳細資訊： [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>以查詢為基礎的 SQL 快取相依性 (限 SQL Server 2005)

當使用 SQL Server 2005 的 SQL 快取相依性，不需要輪詢為基礎的模型。 SQL Server 2005 搭配使用時，SQL 快取相依性進行通訊 （沒有進一步的設定是必要的） SQL Server 執行個體的 SQL 連線透過直接使用 SQL Server 2005 查詢通知。

啟用以查詢為基礎的通知的最簡單方式是藉由設定因此以宣告方式執行**SqlCacheDependency**屬性的資料來源物件，來**CommandNotification**和設定**EnableCaching**屬性設定為 **，則為 true**。 使用此方法，則不不需要中任何程式碼。 如果針對資料來源變更時執行命令的結果，它會導致無效的快取資料。

下列範例會設定 SQL 快取相依性的資料來源控制項：

[!code-aspx[Main](caching/samples/sample12.aspx)]

在此情況下，如果查詢中指定**SelectCommand**傳回不同的結果，比起原先的做法，會快取的結果會失效。

您也可以指定所有的資料來源會藉由設定啟用 SQL 快取相依性**SqlDependency**屬性 **@ OutputCache**指示詞加入**CommandNotification**. 下列範例說明這點。

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> 如需有關 SQL Server 2005 中的查詢通知的詳細資訊，請參閱 SQL Server 線上叢書 》 文件。


設定查詢式 SQL 快取相依性的另一個方法是以程式設計方式使用 SqlCacheDependency 類別若要這樣做。 下列程式碼範例說明如何完成此作業。

[!code-csharp[Main](caching/samples/sample14.cs)]

詳細資訊： [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>快取後替代

快取的頁面，可以大幅增加 Web 應用程式的效能。 不過，在某些情況下您需要動態網頁中的大部分的快取網頁和某些片段。 例如，如果您建立的新聞報導是完全靜態設定一段時間的網頁時，您可以設定快取整個頁面。 如果您想要加入旋轉的廣告橫幅上的每個頁面要求變更，頁面包含廣告的部分必須是動態的。 為了讓您快取的頁面，但動態取代一些內容，您可以使用 ASP.NET 快取後替代。 使用快取後替代整個頁面是使用特定的組件標示為需要快取來快取的輸出。 在範例中的廣告橫幅，AdRotator 控制項可讓您利用快取後替代以便廣告，動態建立的每位使用者及每次頁面重新整理。

有三種方式來實作快取後替代：

- 以宣告方式，使用替代的控制項。
- 以程式設計的方式，使用替代控制項 API。
- 隱含地使用 AdRotator 控制項。

### <a name="substitution-control"></a>替代控制項

ASP.NET 替代控制項指定的快取的頁面，是以動態方式建立，而非快取的區段。 您想要顯示的動態內容的位置在頁面上放置 Substitution control。 在執行階段，Substitution control 會呼叫您使用的 MethodName 屬性指定的方法。 此方法必須傳回字串，它就會取代替代控制項的內容。 方法必須是包含頁面或使用者控制項的控制項上的靜態方法。 使用替代控制項會導致變更伺服器快取性，以用戶端快取性，以便將不在用戶端上快取頁面。 這可確保未來的頁面要求呼叫一次以產生動態內容的方法。

### <a name="substitution-api"></a>替代的 API

若要以程式設計方式建立的快取的頁面的動態內容，您可以呼叫[WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx)網頁程式碼，將它做為參數傳遞的方法名稱中的方法。 處理動態內容的建立方法會採用單一[HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)參數並傳回的字串。 傳回的字串會是內容，其會取代在指定的位置。 呼叫 WriteSubstitution 方法，而不是以宣告方式使用替代控制項的優點是您可以呼叫任何任意的物件，而不是靜態方法呼叫的頁面或使用者控制項物件的方法。

呼叫 WriteSubstitution 方法會導致變更伺服器快取性，以用戶端快取性，以便將不在用戶端上快取頁面。 這可確保未來的頁面要求呼叫一次以產生動態內容的方法。

### <a name="adrotator-control"></a>AdRotator 控制項

伺服器控制項實作 AdRotator 支援進行快取後替代的內部。 如果您放置在頁面上的 AdRotator 控制項時，它會轉譯每個要求，不論是否快取父系分頁上的唯一廣告。 如此一來，包含 AdRotator 控制項的頁面是只快取的伺服器端。

## <a name="controlcachepolicy-class"></a>ControlCachePolicy 類別

ControlCachePolicy 類別用來以程式設計方式控制的快取使用使用者控制項的片段。 ASP.NET 會內嵌使用者控制項內[BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx)執行個體。 BasePartialCachingControl 類別表示有輸出快取啟用的使用者控制項。

當您存取[BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx)屬性[PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx)控制項，您一律會收到有效的 ControlCachePolicy 物件。 不過，如果您存取[UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx)屬性[UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx)控制項，如果您收到有效的 ControlCachePolicy 物件只有已包裝使用者控制項BasePartialCachingControl 控制項。 如果它不會包裝，屬性所傳回的 ControlCachePolicy 物件將會擲回例外狀況當您嘗試將管理它，因為它並沒有相關聯的 BasePartialCachingControl。 若要判斷 UserControl 的執行個體是否支援快取而不會產生例外狀況，檢查[SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx)屬性。

使用 ControlCachePolicy 類別是您可以啟用輸出快取的數種方式之一。 下列清單描述您可用來啟用輸出快取的方法：

- 使用[@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx)指示詞，以啟用輸出快取在宣告式的情況下。
- 使用[PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx)來啟用快取使用者控制項程式碼後置檔案中的屬性。
- 使用中用來代表 BasePartialCachingControl 執行個體已啟用快取使用其中一個先前的方法並以動態方式載入使用處理您的案例中以程式設計方式指定快取設定的ControlCachePolicy類別[System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx)方法。

Init 和 PreRender 控制項生命週期階段之間，才可成功操作 ControlCachePolicy 執行個體。 如果您修改 ControlCachePolicy 物件 PreRender 階段之後，ASP.NET 擲回例外狀況，因為之後要呈現控制項所做的任何變更實際上不會影響快取設定 （轉譯階段期間，會快取控制項） 也一樣。 最後，使用者控制項執行個體 （以及其 ControlCachePolicy 物件） 只適用於以程式設計方式管理實際轉譯時。

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>快取組態-會變為&lt;快取&gt;項目

有數個 ASP.NET 2.0 中的快取組態的變更。 &lt;快取&gt;元素是 ASP.NET 2.0 的新功能，可讓您在組態檔中進行快取的組態變更。 使用下列屬性。

| **目** | **描述** |
| --- | --- |
| **cache** | 選擇性項目。 定義通用的應用程式快取設定。 |
| **outputCache** | 選擇性項目。 指定整個應用程式的輸出快取設定。 |
| **outputCacheSettings** | 選擇性項目。 指定可以套用到應用程式中的頁面輸出快取設定。 |
| **sqlCacheDependency** | 選擇性項目。 設定 ASP.NET 應用程式的 SQL 快取相依性。 |

### <a name="the-ltcachegt-element"></a>&lt;快取&gt;項目

下列屬性位於&lt;快取&gt;項目：

| **屬性** | **描述** |
| --- | --- |
| **disableMemoryCollection** | 選擇性**布林**屬性。 取得或設定值，指出是否要停用機器的記憶體不足的壓力時，就會發生快取記憶體回收。 |
| **disableExpiration** | 選擇性**布林**屬性。 取得或設定值，指出是否要停用快取到期時間。 停用時，快取項目不會過期，背景清除過期的快取項目不會發生。 |
| **privateBytesLimit** | 選擇性**Int64**屬性。 取得或設定值，指出快取開始排清之前的應用程式的私用位元組的大小上限已過期的項目，並嘗試以回收記憶體。 這項限制包含快取所使用的記憶體以及標準的記憶體負荷是從執行的應用程式。 若設定為零表示，ASP.NET 會使用它自己的啟發學習法來判斷何時開始回收記憶體。 |
| **percentagePhysicalMemoryUsedLimit** | 選擇性**Int32**屬性。 取得或設定值，指出快取開始排清之前可使用的應用程式的機器的實體記憶體的最大百分比過期項目，並嘗試以回收此記憶體使用量會包含這兩個快取也使用的記憶體的記憶體為執行中應用程式的一般記憶體使用量。 若設定為零表示，ASP.NET 會使用它自己的啟發學習法來判斷何時開始回收記憶體。 |
| **privateBytesPollTime** | 選擇性**TimeSpan**屬性。 取得或設定值，指出應用程式的私用位元組記憶體使用輪詢間的時間間隔。 |

### <a name="the-ltoutputcachegt-element"></a>&lt;OutputCache&gt;項目

下列屬性可供&lt;outputCache&gt;項目。


|       <strong>屬性</strong>        |                                                                                                                                                                                                                                                       <strong>描述</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          選擇性<strong>布林</strong>屬性。 啟用/停用網頁輸出快取。 如果停用，沒有任何頁面會快取，無論以程式設計方式或宣告式設定。 預設值是<strong>，則為 true</strong>。                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                選擇性<strong>布林</strong>屬性。 啟用/停用應用程式片段快取。 如果停用，不會快取的無關[@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx)指示詞或快取使用的設定檔。 包含快取控制標頭，指出，上游 proxy 伺服器，以及瀏覽器用戶端不應該嘗試快取頁面輸出。 預設值是<strong>false</strong>。                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      選擇性<strong>布林</strong>屬性。 取得或設定值，指出是否<strong>快取-: private</strong>預設輸出快取模組傳送標頭。 預設值是<strong>false</strong>。                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | 選擇性<strong>布林</strong>屬性。 啟用/停用 Http 傳送 「<strong>Vary: \</ s t ><em>"標頭回應中的。使用預設設定為 false，「</em>* Vary: \* <strong>"標頭傳送的輸出快取頁面。Vary 標頭傳送時，它可讓不同版本，以快取根據 Vary 標頭中指定的內容。例如， <em>Vary： 使用者-代理程式</em>會儲存不同版本的頁面，根據發出要求的使用者代理程式。預設值是 * * false</strong>。 |

### <a name="the-ltoutputcachesettingsgt-element"></a>&lt;OutputCacheSettings&gt;項目

&lt;OutputCacheSettings&gt;項目可讓您建立的快取設定檔，如先前所述。 唯一子系項目&lt;outputCacheSettings&gt;項目是&lt;outputCacheProfiles&gt;來設定快取設定檔的項目。

### <a name="the-ltsqlcachedependencygt-element"></a>&lt;SqlCacheDependency&gt;項目

下列屬性可供&lt;sqlCacheDependency&gt;項目。

| **屬性** | **描述** |
| --- | --- |
| **enabled** | 所需**布林**屬性。 表示輪詢變更。 |
| **pollTime** | 選擇性**Int32**屬性。 設定與 SqlCacheDependency 輪詢資料庫資料表的變更的頻率。 此值對應至連續輪詢之間的毫秒數。 它不能設定為小於 500 毫秒。 預設值為 1 分鐘。 |

### <a name="more-information"></a>更多資訊

還有一些您應該要注意有關快取組態的額外資訊。

- 如果未設定的背景工作處理序私用位元組限制，快取會使用其中一個下列限制： 

    - x86 2 GB: 800 MB 或 60%的實體 RAM 少於
    - x86 3 GB: 1800 MB 或 60%的實體 RAM 少於
    - 64: 1 tb 或 60%的實體 RAM 少於
- 如果這兩個背景工作處理序私用位元組限制並&lt;快取 privateBytesLimit /&gt;設定快取會使用兩個最小值。
- 就像在 1.x 中，我們卸除快取項目並呼叫 GC。收集的原因有二： 

    - 我們也非常接近私用位元組限制
    - 可用的記憶體是接近 100 或小於 10%
- 您可以有效地停用修剪，並藉由設定快取的可用記憶體不足情況&lt;快取 percentagePhysicalMemoryUseLimit /&gt;到 100 之間。
- 與 1.x 不同 2.0 就會擱置的修剪和收集呼叫最後一個 GC。收集不能降低私用位元組或多個 1%（快取） 記憶體限制的 managed 堆積的大小。

## <a name="lab1-custom-cache-dependencies"></a>Lab1： 自訂快取相依性

1. 建立新的網站。
2. 加入新的 XML 檔案，稱為 cache.xml，並將它儲存至 Web 應用程式的根目錄。
3. 將下列程式碼新增至頁面\_載入 default.aspx 程式碼後置中的方法： 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. 原始碼檢視中的 default.aspx 的頂端加入下列內容： 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. 瀏覽 Default.aspx。 時間說什麼？
6. 重新整理瀏覽器。 時間說什麼？
7. 開啟 cache.xml 並新增下列程式碼： 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. 儲存 cache.xml。
9. 重新整理瀏覽器。 時間說什麼？
10. 說明為什麼不會顯示先前快取的值更新的時間：

## <a name="lab-2-using-polling-based-cache-dependencies"></a>實驗室 2： 使用輪詢為基礎的快取相依性

這個實驗室會使用您在前一個模組可供編輯的 GridView 和 DetailsView 控制項透過 Northwind 資料庫中的資料中建立的專案。

1. 在 Visual Studio 2005 中開啟專案。
2. 執行 aspnet\_regsql 公用程式，對 Northwind 資料庫來啟用資料庫和 [產品] 資料表。 使用下列命令從 Visual Studio 命令提示字元： 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. 將下列內容新增至 web.config 檔案： 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. 加入新的 webform 呼叫 showdata.aspx。
5. Showdata.aspx 頁面中加入下列 @ outputcache 指示詞： 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. 將下列程式碼新增至頁面\_showdata.aspx 負載： 

    [!code-html[Main](caching/samples/sample21.html)]
7. 將新的 SqlDataSource 控制項新增至 showdata.aspx，並將它設定為使用 Northwind 資料庫連接。 按一下 [下一步]。
8. 選取的產品名稱 」 和 「 ProductID 核取方塊，然後按一下 [下一步]。
9. 按一下 [完成]。
10. 加入 showdata.aspx 頁面中的新 GridView。
11. 從下拉式清單中選擇 [SqlDataSource1]。
12. 儲存並瀏覽 showdata.aspx。 記下顯示的時間。
