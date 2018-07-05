---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 第 3 部分： 建立管理員控制器 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: caf7649f2e420b4063966fd1a371c3d9a5eb4d04
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815453"
---
<a name="part-3-creating-an-admin-controller"></a>第 3 部分： 建立管理員控制器
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>新增系統管理員控制站

在本節中，我們會將新增的 Web API 控制器支援 CRUD （建立、 讀取、 更新和刪除） 產品上的作業。 控制器會使用 Entity Framework 與資料庫層級進行通訊。 只有系統管理員可以使用此控制站。 客戶會透過另一個控制站存取產品。

在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。 選取 **新增**，然後**控制站**。

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

在 [**新增控制器**] 對話方塊中，將控制器命名`AdminController`。 底下**樣板**，選取&quot;讀取/寫入動作，使用 Entity Framework 的 API 控制器&quot;。 底下**模型類別**，選取 [產品 (ProductStore.Models)]。 底下**資料內容**，選取 「&lt;新的資料內容&gt;"。

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> 如果**模型類別**下拉式清單不會顯示任何模型類別，請確定您編譯專案。 Entity Framework 會使用反映，因此它需要編譯的組件。


選取 「&lt;新的資料內容&gt;」 就會開啟**新的資料內容**對話方塊。 命名的資料內容`ProductStore.Models.OrdersContext`。

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

按一下  **確定**以關閉**新的資料內容**對話方塊。 在 [**新增控制器**] 對話方塊中，按一下**新增**。

以下是項目加入至專案：

- 類別，名為`OrdersContext`衍生自**DbContext**。 這個類別提供 POCO 模型與資料庫之間的連結。
- 名為的 Web API 控制器`AdminController`。 此控制站支援上的 CRUD 作業`Product`執行個體。 它會使用`OrdersContext`類別與 Entity Framework 進行通訊。
- 新資料庫的連接字串中的 Web.config 檔案。

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

開啟 OrdersContext.cs 檔案。 請注意，建構函式會指定資料庫連接字串的名稱。 這個名稱指的是已加入至 Web.config 的連接字串。

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

將下列屬性新增至 `OrdersContext` 類別：

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

A **DbSet**代表一組可查詢的實體。 以下是完整清單，如`OrdersContext`類別：

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

`AdminController`類別會定義五個實作基本的 CRUD 功能的方法。 每個方法會對應至用戶端可以叫用的 URI:

| 控制器方法 | 描述 | URI | HTTP 方法 |
| --- | --- | --- | --- |
| GetProducts | 取得所有產品。 | api/產品 | GET |
| GetProduct | 尋找產品識別碼。 | api/產品/*識別碼* | GET |
| PutProduct | 更新的產品。 | api/產品/*識別碼* | PUT |
| PostProduct | 建立新的產品。 | api/產品 | POST |
| DeleteProduct | 刪除產品。 | api/產品/*識別碼* | DELETE |

每個方法會呼叫`OrdersContext`查詢資料庫。 修改集合 （PUT、 POST 和 DELETE） 的方法呼叫`db.SaveChanges`保存到資料庫變更。 控制站建立每個 HTTP 要求，然後處置，因此必須保存的變更，方法傳回之前。

## <a name="add-a-database-initializer"></a>新增資料庫初始設定式

Entity Framework 有不錯的功能，可讓您在啟動時，資料庫中填入資料和模型變更時，會自動重新建立資料庫。 這項功能是在開發期間非常有用，因為您永遠有一些測試資料，即使您變更模型。

在 [方案總管] 中，以滑鼠右鍵按一下 [模型] 資料夾，並建立新的類別，名為`OrdersContextInitializer`。 貼上下列實作：

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

藉由繼承自**DropCreateDatabaseIfModelChanges**類別中，我們會告訴 Entity Framework，使卸除資料庫，每當我們修改模型類別。 當 Entity Framework 建立 （或重新建立） 的資料庫時，它會呼叫**種子**來填入資料表的方法。 我們會使用**種子**將某些範例產品再加上範例訂單的方法。

這項功能很適合用於測試，但不用**DropCreateDatabaseIfModelChanges**類別在實際執行環境、，因為如果有人變更模型類別，您可能會遺失您的資料。

接下來，開啟 Global.asax，然後將下列程式碼加入**應用程式\_啟動**方法：

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>將要求傳送至控制器

到目前為止，我們還沒有撰寫任何用戶端程式碼，但您可以叫用的 web API 使用網頁瀏覽器或 HTTP 偵錯工具，例如[Fiddler](http://www.fiddler2.com/fiddler2/)。 在 Visual Studio 中，按下 f5 鍵啟動偵錯。 您的網頁瀏覽器會開啟`http://localhost:*portnum*/`，其中*portnum*是某些連接埠號碼。

傳送 HTTP 要求，以 「`http://localhost:*portnum*/api/admin`。 第一個要求可能慢而無法完成，因為 Entify Framework，就必須建立並植入資料庫。 回應應該類似如下：

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [上一頁](using-web-api-with-entity-framework-part-2.md)
> [下一頁](using-web-api-with-entity-framework-part-4.md)
