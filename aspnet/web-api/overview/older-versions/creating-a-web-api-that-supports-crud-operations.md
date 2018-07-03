---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: 啟用 ASP.NET Web API 1 中的 CRUD 作業 |Microsoft Docs
author: MikeWasson
description: 本教學課程會示範如何使用 ASP.NET Web API HTTP 服務中支援的 CRUD 作業。 教學課程 Visual Studio 2012 Web AP 中所使用的軟體版本...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 1658e120225cd3c9425168238981133c96ff398a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402924"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>啟用 ASP.NET Web API 1 中的 CRUD 作業
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> 本教學課程會示範如何使用 ASP.NET Web API HTTP 服務中支援的 CRUD 作業。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Visual Studio 2012
> - Web API 1 （也適用於使用 Web API 2）


代表 CRUD&quot;建立、 讀取、 更新和刪除，&quot;是四個基本資料庫作業。 許多 HTTP 服務也模型透過 REST 或 REST 型 Api 的 CRUD 作業。

在本教學課程中，您將建置非常簡單的 web API 以管理的產品清單。 每項產品會包含名稱、 價格和類別目錄 (例如&quot;toys&quot;或是&quot;硬體&quot;)，再加上產品識別碼。

產品的 API 會公開下列方法。

| 動作 | HTTP 方法 | 相對 URI |
| --- | --- | --- |
| 取得所有產品的清單 | GET | / api/產品 |
| 取得產品識別碼 | GET | /api/產品/*識別碼* |
| 依類別取得的產品 | GET | /api/products?category=*category* |
| 建立新的產品 | POST | / api/產品 |
| 將產品更新 | PUT | /api/產品/*識別碼* |
| 刪除產品 | DELETE | /api/產品/*識別碼* |

請注意一些 Uri 路徑中包含產品識別碼。 例如，若要取得其識別碼為 28 的產品，用戶端傳送 GET 要求`http://hostname/api/products/28`。

### <a name="resources"></a>資源

產品的 API 會定義兩種資源類型的 Uri:

| 資源 | URI |
| --- | --- |
| 所有產品的清單。 | / api/產品 |
| 個別的產品。 | /api/產品/*識別碼* |

### <a name="methods"></a>方法

四個主要 HTTP 方法 （GET、 PUT、 POST 和 DELETE） 可以對應至 CRUD 作業，如下所示：

- 取得作業會擷取指定之 URI 的資源的表示法。 取得應該在伺服器上沒有任何副作用。
- PUT 更新資源，以在指定的 URI。 PUT 也可用來建立新的資源： 指定的 URI，如果伺服器允許用戶端指定新的 Uri。 本教學課程中，此 API 不支援透過 PUT 建立。
- POST 建立新的資源。 伺服器會指派新的物件的 URI，並傳回此 URI 做為回應訊息的一部分。
- [刪除] 刪除的資源時指定的 URI。

注意： PUT 方法會取代整個產品實體。 也就是用戶端應該傳送更新的產品的完整表示法。 如果您想要支援部分更新，PATCH 方法是慣用的。 本教學課程不會實作修補程式。

## <a name="create-a-new-web-api-project"></a>建立新的 Web API 專案

啟動執行 Visual Studio，並選取**新的專案**從**開始**頁面。 或從**檔案**功能表上，選取**新增**，然後**專案**。

在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。 底下**Visual C#**，選取**Web**。 在專案範本清單中，選取**ASP.NET MVC 4 Web 應用程式**。 將專案命名為&quot;ProductStore&quot;然後按一下**確定**。

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**Web API** ，按一下 **確定**。

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>新增模型

「模型」是代表應用程式中資料的物件。 在 ASP.NET Web API 中，您可以使用強型別的 CLR 物件模型，和他們將會自動序列化為 XML 或 JSON 的用戶端。

針對 ProductStore API，我們的資料包含產品，因此我們將建立新的類別，名為`Product`。

如果沒有顯示 [方案總管] 中，按一下**檢視**功能表，然後選取**方案總管 中**。 在 [方案總管] 中，以滑鼠右鍵按一下**模型**資料夾。 從內容功能表中，選取**新增**，然後選取**類別**。 將類別命名為&quot;產品&quot;。

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

新增下列屬性以`Product`類別。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>加入儲存機制

我們需要儲存 products 的集合。 它是個不錯的主意，來區隔我們的服務實作的集合。 如此一來，我們可以變更備份存放區，不需要重新撰寫的服務類別。 這種類型的設計會呼叫*存放庫*模式。 定義泛型介面，存放庫開始。

在 [方案總管] 中，以滑鼠右鍵按一下**模型**資料夾。 選取 **新增**，然後選取**新項目**。

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

在 [**範本**窗格中，選取**已安裝的範本**展開 C#] 節點。 在 C# 中，選取**程式碼**。 在程式碼範本清單中，選取**介面**。 介面命名&quot;IProductRepository&quot;。

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

新增下列實作：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

現在將另一個類別新增至 [模型] 資料夾，名為&quot;ProductRepository。&quot;此類別會實作 `IProductRespository` 介面。 新增下列實作：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

存放庫會保留在本機記憶體中的清單。 這是 [確定]，教學課程中，但實際的應用程式中，您會將資料儲存於外部的資料庫或在雲端儲存體中。 儲存機制模式將會進行變更之後的實作變得更容易。

## <a name="adding-a-web-api-controller"></a>新增 Web API 控制器

如果您已使用 ASP.NET MVC，然後您就已經熟悉控制站。 在 ASP.NET Web API 中，*控制器*是處理來自用戶端的 HTTP 要求的類別。 新增專案 精靈建立專案時，為您建立兩個控制站。 若要查看它們，請展開方案總管 中的 控制器 資料夾。

- HomeController 是傳統的 ASP.NET MVC 控制器。 它會負責提供 HTML 頁面的網站，並與我們的 web API 不直接相關。
- ValuesController 是範例 WebAPI 控制器。

請繼續並以滑鼠右鍵按一下方案總管 中的檔案，然後選取刪除 ValuesController，**刪除。** 現在加入的新控制器，如下所示：

在**方案總管 中**，以滑鼠右鍵按一下 [控制器] 資料夾。 選取 **新增**，然後選取**控制站**。

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

在 **新增控制器**精靈，將控制器命名&quot;ProductsController&quot;。 在 **樣板**下拉式清單中，選取**空白 API 控制器**。 然後按一下 [加入] 。

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> 您不需要將您的控制器 放入名為控制器的資料夾。 資料夾名稱並不重要;它只是方便方法來組織您的原始程式檔。


**新增控制器**精靈會建立名為 ProductsController.cs 在 Controllers 資料夾中的檔案。 如果此檔案尚未開啟，按兩下檔案以開啟它。 新增下列**使用**陳述式：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

新增欄位來保存**IProductRepository**執行個體。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> 呼叫`new ProductRepository()`控制器中不是最好的設計，因為它所繫結的特定實作控制器`IProductRepository`。 好的方法，請參閱 <<c0> [ 使用 Web API 的相依性解析程式](../advanced/dependency-injection.md)。


## <a name="getting-a-resource"></a>取得資源

ProductStore API 會公開數個&quot;讀取&quot;HTTP GET 方法的動作。 每個動作將會對應到方法，以在`ProductsController`類別。

| 動作 | HTTP 方法 | 相對 URI |
| --- | --- | --- |
| 取得所有產品的清單 | GET | / api/產品 |
| 取得產品識別碼 | GET | /api/產品/*識別碼* |
| 依類別取得的產品 | GET | /api/products?category=*category* |

若要取得所有產品的清單，將下列方法來新增`ProductsController`類別：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

方法名稱開頭&quot;取得&quot;，因此依照慣例則會對應至 GET 要求。 此外，因為方法沒有任何參數，則會對應至不包含在 URI *&quot;識別碼&quot;* 路徑中的區段。

若要取得的產品識別碼，將下列方法來新增`ProductsController`類別：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

此方法名稱也會以開頭&quot;取得&quot;，但方法具有一個名為參數*識別碼*。此參數對應到&quot;識別碼&quot;URI 路徑區段。 ASP.NET Web API 架構會自動將識別碼轉換為正確的資料類型 (**int**) 參數。

GetProduct 方法會擲回的例外狀況型別的**HttpResponseException**如果*識別碼*無效。 這個例外狀況會由架構轉譯為 404 （找不到） 錯誤。

最後，新增方法以依類別目錄來尋找產品：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

如果在要求 URI 查詢字串，Web API 就會嘗試比對上的控制器方法的參數的查詢參數。 因此，在表單的 URI"api/產品？ 類別 =*分類*"會對應至這個方法。

## <a name="creating-a-resource"></a>建立資源

接下來，我們將新增方法以`ProductsController`類別來建立新的產品。 以下是簡單的方法實作：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

請注意此方法的相關的兩件事：

- 方法名稱開頭為&quot;文章...&quot;. 若要建立新的產品，用戶端會傳送 HTTP POST 要求。
- 此方法會採用類型參數的產品。 在 Web API 中，使用複雜類型的參數會從要求主體還原序列化。 因此，我們會預期用戶端傳送以 XML 或 JSON 格式的產品物件的序列化表示法。

此實作會使用，但還不算完整。 在理想情況下，我們想要包括下列的 HTTP 回應：

- **回應碼：** 根據預設，Web API 架構會將回應狀態碼為 200 （確定）。 但是，根據 HTTP/1.1 通訊協定，POST 要求所產生的資源，建立伺服器應該會回覆狀態 201 （已建立）。
- **位置：** 當伺服器建立資源時，它應該回應的 Location 標頭中包含新資源的 URI。

ASP.NET Web API 可讓您更容易操作的 HTTP 回應訊息。 以下是改進的實作：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

請注意，方法的傳回型別現在**HttpResponseMessage**。 藉由傳回**HttpResponseMessage**而不是一項產品，我們可以控制的 HTTP 回應訊息，包括狀態碼和 Location 標頭詳細資料。

**CreateResponse**方法會建立**HttpResponseMessage**並自動將 Product 物件的序列化的表示寫入本文提出的回應訊息。

> [!NOTE]
> 此範例不會驗證`Product`。 如需模型驗證資訊，請參閱[ASP.NET Web API 中的模型驗證](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)。


## <a name="updating-a-resource"></a>更新資源

使用 PUT 更新產品很簡單：

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

方法名稱開頭為&quot;放...&quot;，使 Web API 的 PUT 要求來比對。 此方法會採用兩個參數、 產品識別碼和更新的產品。 *識別碼*參數取自 URI 路徑，而*產品*參數從要求主體還原序列化。 根據預設，ASP.NET Web API 架構會從路由的簡單參數類型和要求本文中的複雜類型。

## <a name="deleting-a-resource"></a>刪除資源

若要刪除 resourse，定義 「 刪除中...」 方法。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

如果 DELETE 要求成功，它會傳回狀態 200 （確定） 與實體本文描述狀態;狀態 202 （已接受） 如果仍在刪除暫止;或狀態 204 （沒有內容） 沒有實體主體。 在此情況下，`DeleteProduct`方法具有`void`傳回型別，因此 ASP.NET Web API 會自動轉譯為此狀態碼 204 （沒有內容）。
