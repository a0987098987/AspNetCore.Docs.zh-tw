---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: 啟用 ASP.NET Web API 1 中的 CRUD 作業 |Microsoft 文件
author: MikeWasson
description: 本教學課程會示範如何使用 ASP.NET Web API HTTP 服務中支援 CRUD 作業。 教學課程 Visual Studio 2012 Web 應用程式中使用的軟體版本...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 69b7d5453b6ff36d6e28a69428b016cb8cfd06e9
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>啟用 ASP.NET Web API 1 中的 CRUD 作業
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> 本教學課程會示範如何使用 ASP.NET Web API HTTP 服務中支援 CRUD 作業。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Visual Studio 2012
> - Web API 1 （也適用於 Web API 2）


代表 CRUD&quot;建立、 讀取、 更新和刪除，&quot;哪些是四個基本資料庫作業。 許多 HTTP 服務也模型透過 REST 或類似的 REST Api 的 CRUD 作業。

在本教學課程中，您將建立非常簡單的 web 應用程式開發介面來管理的產品清單。 名稱、 價格和類別，將會包含每個產品 (例如&quot;toys&quot;或&quot;硬體&quot;)，再加上產品識別碼。

產品的 API 會公開下列方法。

| 動作 | HTTP 方法 | 相對 URI |
| --- | --- | --- |
| 取得所有產品的清單 | GET | / api/產品 |
| 取得產品識別碼 | GET | /api/products/*id* |
| 取得產品類別目錄 | GET | /api/products?category=*category* |
| 建立新的產品 | POST | / api/產品 |
| 產品更新 | PUT | /api/products/*id* |
| 刪除產品 | DELETE | /api/products/*id* |

請注意，部分 Uri 的路徑中包含產品識別碼。 例如，若要取得其識別碼為 28 的產品，用戶端會傳送 GET 要求`http://hostname/api/products/28`。

### <a name="resources"></a>資源

產品應用程式開發介面會定義兩個資源類型的 Uri:

| 資源 | URI |
| --- | --- |
| 所有產品的清單。 | / api/產品 |
| 個別產品。 | /api/products/*id* |

### <a name="methods"></a>方法

四個主要 HTTP 方法 （GET、 PUT、 POST 和 DELETE） 可以對應至 CRUD 作業，如下所示：

- 取得作業會擷取在指定的 URI 資源的表示法。 取得應該在伺服器上沒有任何副作用。
- PUT 更新的資源時指定的 URI。 PUT 也可用來建立新的資源在指定的 URI，如果伺服器允許用戶端指定新的 Uri。 此教學課程中，應用程式開發介面不支援透過 PUT 建立。
- POST 建立新的資源。 伺服器會指派新物件的 URI，並傳回此 URI 做為回應訊息的一部分。
- DELETE 會刪除的資源時指定的 URI。

注意： PUT 方法會取代整個產品實體。 也就是說，用戶端應該傳送更新的產品的完整表示法。 如果您想要支援部分更新，更新程式最好方法。 本教學課程未實作修補程式。

## <a name="create-a-new-web-api-project"></a>建立新的 Web API 專案

開始執行 Visual Studio，並選取**新專案**從**啟動**頁面。 或從**檔案**功能表上，選取**新增**然後**專案**。

在**範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。 在下**Visual C#**，選取**Web**。 在專案範本清單中選取**ASP.NET MVC 4 Web 應用程式**。 將專案命名&quot;ProductStore&quot;按一下**確定**。

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**Web API**按一下**確定**。

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>加入模型

「模型」是代表應用程式中資料的物件。 在 ASP.NET Web API 中，您可以使用強型別的 CLR 物件模型中，為，它們會自動序列化為 XML 或 JSON 用戶端。

ProductStore api 中，我們的資料組成產品，因此我們將建立新的類別，名為`Product`。

如果沒有顯示 方案總管 中，按一下**檢視**功能表，然後選取**方案總管 中**。 在 [方案總管] 中，以滑鼠右鍵按一下**模型**資料夾。 從內容 meny 中，選取**新增**，然後選取**類別**。 將類別&quot;產品&quot;。

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

加入下列屬性以`Product`類別。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>加入儲存機制

我們需要儲存產品的集合。 最好區隔我們的服務實作中的集合。 這樣一來，我們可以變更備份存放區，而不用重寫服務類別。 這種設計稱為*儲存機制*模式。 會定義儲存機制的泛型介面開始。

在 [方案總管] 中，以滑鼠右鍵按一下**模型**資料夾。 選取**新增**，然後選取**新項目**。

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

在**範本**窗格中，選取**已安裝的範本**展開 C# 節點。 在 C# 中，選取**程式碼**。 在程式碼的範本清單中，選取**介面**。 命名介面&quot;IProductRepository&quot;。

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

加入下列實作：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

現在將另一個類別加入至 [模型] 資料夾，名為&quot;ProductRepository。&quot;此類別會實作 `IProductRespository` 介面。 加入下列實作：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

儲存機制會保留在本機記憶體中的清單。 這是 [確定]，教學課程中，但在實際應用中，您會將資料儲存於外部的資料庫或雲端儲存體中。 儲存機制模式會讓您更輕鬆地變更更新版本的實作。

## <a name="adding-a-web-api-controller"></a>加入 Web API 控制器

如果您已使用 ASP.NET MVC，則您必須已熟悉控制站。 在 ASP.NET Web API 中，*控制器*是處理來自用戶端的 HTTP 要求的類別。 新增專案 精靈建立專案時，為您建立兩個控制站。 若要查看它們，請展開 [方案總管] 中的 [控制器] 資料夾。

- HomeController 是傳統的 ASP.NET MVC 控制器。 它會負責處理站台的 HTML 網頁，並與我們的 web 應用程式開發介面不直接相關。
- ValuesController 是範例 WebAPI 控制器。

繼續進行和刪除 ValuesController，以滑鼠右鍵按一下方案總管 中的檔案，然後選取**刪除。** 現在加入新的控制站，如下所示：

在**方案總管 中**，以滑鼠右鍵按一下 控制器 資料夾。 選取**新增**，然後選取 **控制器**。

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

在**加入控制器**精靈 中，名稱控制器&quot;ProductsController&quot;。 在**範本**下拉式清單中，選取**空白的 API 控制器**。 然後按一下 [加入] 。

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> 您不需要您 contollers 放入名為控制器的資料夾。 資料夾名稱並不重要;它只是方便方法來組織您的來源檔案。


**加入控制器**精靈會建立名為 ProductsController.cs [控制器] 資料夾中的檔案。 如果此檔案尚未開啟，按兩下檔案將它開啟。 加入下列**使用**陳述式：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

加入欄位保存**IProductRepository**執行個體。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> 呼叫`new ProductRepository()`控制器中不是最好的設計，因為它繫結至特定的實作控制器`IProductRepository`。 更好的方法，請參閱[使用 Web 應用程式開發介面的相依性解析程式](../advanced/dependency-injection.md)。


## <a name="getting-a-resource"></a>取得資源

ProductStore API 會公開數個&quot;讀取&quot;HTTP GET 方法的動作。 每個動作將會對應到中的方法`ProductsController`類別。

| 動作 | HTTP 方法 | 相對 URI |
| --- | --- | --- |
| 取得所有產品的清單 | GET | / api/產品 |
| 取得產品識別碼 | GET | /api/products/*id* |
| 取得產品類別目錄 | GET | /api/products?category=*category* |

若要取得所有產品的清單，請將此方法加入`ProductsController`類別：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

方法名稱開頭&quot;取得&quot;，好讓它對應至 GET 要求的慣例。 此外，因為方法不有任何參數，則會對應至 URI，其中不包含*&quot;識別碼&quot;* 中路徑區段。

若要取得的產品識別碼，請將此方法加入`ProductsController`類別：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

這個方法的名稱也會啟動具有&quot;取得&quot;，但這個方法有名稱為的參數*識別碼*。這個參數會對應至&quot;識別碼&quot;URI 路徑的區段。 ASP.NET Web API framework 自動將 ID 轉換成正確的資料類型 (**int**) 參數。

GetProduct 方法擲回例外狀況型別的**HttpResponseException**如果*識別碼*不正確。 這個例外狀況會由架構轉譯為 404 （找不到） 錯誤。

最後，將方法加入依分類搜尋產品：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

要求 URI 具有查詢字串，如果 Web API 會嘗試比對控制器方法上的參數的查詢參數。 因此，在表單的 URI"api/產品？ 類別 =*類別*」 將會對應至這個方法。

## <a name="creating-a-resource"></a>建立資源

接下來，我們會將方法加入`ProductsController`類別來建立新的產品。 以下是簡單的實作的方法：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

請注意此方法的相關兩件事：

- 方法名稱開頭&quot;Post...&quot;. 若要建立新的產品，用戶端會傳送 HTTP POST 要求。
- 方法會接受 Product 類型的參數。 在 Web API 中，具有複雜類型的參數會從要求主體還原序列化。 因此，我們會以 XML 或 JSON 格式傳送序列化的物件的表示產品，用戶端。

這項實作也能運作，但還不算完整。 在理想情況下，我們都希望 HTTP 回應對包括下列各項：

- **回應碼：**根據預設，Web API framework 設定回應狀態碼 200 （確定）。 但是，根據 HTTP/1.1 通訊協定，POST 要求所產生的資源，建立伺服器應該回覆狀態 201 （已建立）。
- **位置：**當伺服器建立資源時，它應該包含新資源的 URI 回應的位置標頭中。

ASP.NET Web API 輕鬆地操作 HTTP 回應訊息。 以下是改進的實作：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

請注意，方法的傳回型別現在**HttpResponseMessage**。 藉由傳回**HttpResponseMessage**而不是產品，我們可以控制的 HTTP 回應訊息，包括狀態碼和 Location 標頭的詳細資料。

**CreateResponse**方法會建立**HttpResponseMessage**並自動將 Product 物件的序列化表示法寫入本文 fo 回應訊息。

> [!NOTE]
> 此範例不會驗證`Product`。 如需模型驗證資訊，請參閱[ASP.NET Web API 中的模型驗證](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)。


## <a name="updating-a-resource"></a>更新資源

使用 PUT 更新產品相當直接：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

方法名稱開頭&quot;放...&quot;，因此 Web API 進行比對 PUT 要求。 此方法會採用兩個參數、 產品識別碼和更新的產品。 *識別碼*參數取自 URI 路徑，而*產品*參數會從要求主體還原序列化。 根據預設，ASP.NET Web API 架構會取用來自路由的簡單參數類型和複雜類型的要求主體。

## <a name="deleting-a-resource"></a>刪除資源

若要刪除 resourse，定義 「 刪除...」 方法。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

如果成功刪除要求，它可以傳回狀態 200 (OK) 描述狀態; 實體主體狀態 202 （已接受） 如果仍已刪除暫止的;或狀態沒有實體主體 204 （沒有內容）。 在此情況下，`DeleteProduct`方法有`void`傳回型別，因此 ASP.NET Web API 自動轉譯為此狀態碼 204 （沒有內容）。
