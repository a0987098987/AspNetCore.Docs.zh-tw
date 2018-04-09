---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 第 3 部分： 建立系統管理控制站 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-3-creating-an-admin-controller"></a>第 3 部分： 建立系統管理控制站
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>新增系統管理控制站

在本節中，我們會將新增的 Web API 控制器支援 CRUD （建立、 讀取、 更新和刪除） 的產品上的作業。 控制器將會使用 Entity Framework 資料庫層級與通訊。 只有系統管理員可以使用此控制站。 客戶會透過另一個控制站存取的產品。

在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。 選取**新增**然後**控制器**。

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

在**加入控制器**對話方塊中，控制器`AdminController`。 在下**範本**，選取&quot;使用 Entity Framework 的讀取/寫入動作的 API 控制器&quot;。 在下**模型類別**，選取 「 產品 (ProductStore.Models) 」。 在下**資料內容**，選取 「&lt;新的資料內容&gt;"。

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> 如果**模型類別**下拉式清單不會顯示任何模型類別，請確定您編譯專案。 Entity Framework 會使用反映，因此它需要編譯的組件。


選取 「&lt;新的資料內容&gt;」 將會開啟**新的資料內容**對話方塊。 命名的資料內容`ProductStore.Models.OrdersContext`。

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

按一下**確定**解除**新的資料內容**對話方塊。 在**加入控制器**] 對話方塊中，按一下 [**新增**。

以下是項目已加入至專案：

- 類別，名為`OrdersContext`衍生自**DbContext**。 這個類別會提供黏附 POCO 模型與資料庫之間。
- 名為此 Web API 控制器`AdminController`。 此控制站上支援 CRUD 作業`Product`執行個體。 它會使用`OrdersContext`與 Entity Framework 進行的類別。
- 新資料庫的連接字串中的 Web.config 檔案。

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

開啟 OrdersContext.cs 檔案。 請注意，建構函式會指定資料庫連接字串的名稱。 這個名稱會參考已加入至 Web.config 連接字串。

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

將下列屬性新增至 `OrdersContext` 類別：

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

A **DbSet**代表一組可查詢的實體。 以下是完整清單`OrdersContext`類別：

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

`AdminController`類別會定義五種實作基本 CRUD 功能的方法。 每個方法會對應至用戶端可以叫用的 URI:

| 控制器方法 | 描述 | URI | HTTP 方法 |
| --- | --- | --- | --- |
| GetProducts | 取得所有產品。 | 應用程式開發介面/產品 | GET |
| GetProduct | 找到的產品識別碼。 | api/products/*id* | GET |
| PutProduct | 更新的產品。 | api/products/*id* | PUT |
| PostProduct | 建立新的產品。 | 應用程式開發介面/產品 | POST |
| DeleteProduct | 刪除產品。 | api/products/*id* | DELETE |

每個方法呼叫`OrdersContext`查詢資料庫。 修改集合 （PUT、 POST 和 DELETE） 的方法呼叫`db.SaveChanges`保存到資料庫變更。 控制站建立每個 HTTP 要求，然後處置，因此需要保存的變更，方法傳回之前。

## <a name="add-a-database-initializer"></a>加入的資料庫初始設定式

Entity Framework 必須方便的功能可讓您在啟動時，在資料庫中填入，每當變更模型時，會自動重新建立資料庫。 您始終有一些測試資料，即使您變更模型，這項功能是在開發期間相當實用。

在 [方案總管] 中，以滑鼠右鍵按一下 [模型] 資料夾和建立新的類別，名為`OrdersContextInitializer`。 貼上下列實作：

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

透過繼承自**DropCreateDatabaseIfModelChanges**類別中，我們會告知 Entity Framework，每當我們修改模型類別時，請卸除資料庫。 當 Entity Framework 建立 （或重新建立） 資料庫時，它會呼叫**種子**來擴展資料表的方法。 我們使用**種子**方法，將某些範例產品加上的範例訂單。

這項功能最適合用於測試，但並不使用**DropCreateDatabaseIfModelChanges**在實際執行環境、 類別，因為如果有人變更模型類別，您可能會失去您的資料。

接下來，開啟 Global.asax 並加入下列程式碼加入**應用程式\_啟動**方法：

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>將要求傳送至控制器

此時，我們尚未撰寫任何用戶端程式碼，但您可以叫用的 web 應用程式開發介面使用網頁瀏覽器或 HTTP 偵錯工具，例如[Fiddler](http://www.fiddler2.com/fiddler2/)。 在 Visual Studio 中，按 f5 鍵開始偵錯。 您的網頁瀏覽器會開啟`http://localhost:*portnum*/`，其中*portnum*是某些連接埠號碼。

傳送 HTTP 要求，以便 「`http://localhost:*portnum*/api/admin`。 第一次要求可能慢而無法完成，因為 Entify Framework 需要建立並植入資料庫。 回應應與下列類似：

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [上一頁](using-web-api-with-entity-framework-part-2.md)
> [下一頁](using-web-api-with-entity-framework-part-4.md)
