---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: "第 2 部分： 建立網域模型 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: a573b47d27767dc78d557cd2b6c73714eb9e94f4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="part-2-creating-the-domain-models"></a>第 2 部分： 建立網域模型
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>加入模型

有三種方法 Entity Framework:

- 資料庫優先 （contract-first）： 使用資料庫時，開始和 Entity Framework 產生的程式碼。
- 模型優先： 開始您以視覺化的模型，與 Entity Framework 產生資料庫與程式碼。
- 程式碼優先： 開始您以程式碼中，與 Entity Framework 產生資料庫。

我們使用的程式碼第一個方法，讓我們一開始定義為 POCOs （純舊 CLR 物件） 的網域物件。 使用程式碼優先方法時，網域物件不需要任何額外的程式碼，以支援資料庫層級，例如交易或持續性。 (具體而言，它們不需要繼承自[EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx)類別。)您仍然可以使用資料註解來控制 Entity Framework 建立的資料庫結構描述的方式。

因為 POCOs 不會包含任何額外的屬性描述[的資料庫狀態](https://msdn.microsoft.com/library/system.data.entitystate.aspx)，他們可以輕鬆地序列化為 JSON 或 XML。 不過，這不表示您應該一律公開給用戶端，直接 Entity Framework 模型稍後在本教學課程中，我們會看到。

我們將建立下列 POCOs:

- 產品
- 訂單
- OrderDetail

若要建立每個類別，以滑鼠右鍵按一下方案總管 中的 模型 資料夾。 從內容功能表中，選取**新增**，然後選取 **類別。**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

新增`Product`類別具有下列實作：

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

依照慣例，Entity Framework 會使用`Id`屬性做為主要金鑰並將它對應至資料庫資料表中識別資料行。 當您建立新`Product`執行個體，您將不會將值設`Id`，因為資料庫產生值。

**ScaffoldColumn**屬性會告知 ASP.NET MVC 略過`Id`產生編輯器表單時的屬性。 **需要**屬性用來驗證模型。 它會指定`Name`屬性必須是非空白字串。

新增`Order`類別：

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

新增`OrderDetail`類別：

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>外部索引鍵關聯

訂單包含許多訂單詳細資料，以及每個訂單詳細資料是指單一產品。 若要代表這些關聯性，`OrderDetail`類別會定義名為屬性`OrderId`和`ProductId`。 Entity Framework 會推斷這些屬性代表外部索引鍵，而且會將外部索引鍵條件約束加入至資料庫。

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

`Order`和`OrderDetail`類別也會包含 「 瀏覽 」 屬性，其中包含相關物件的參考。 指定的順序，您可以瀏覽至訂單中產品所遵循的導覽屬性。

現在編譯專案。 Entity Framework 會使用反映來探索模型的屬性，因此需要建立資料庫結構描述編譯的組件。

## <a name="configure-the-media-type-formatters"></a>設定媒體類型格式器

A[媒體類型格式器](../../formats-and-model-binding/media-formatters.md)是將資料序列化時 Web API 寫入 HTTP 回應主體的物件。 內建的格式器支援 JSON 和 XML 輸出。 根據預設，這兩個這些格式器序列化值的所有物件。

值序列化會有問題，如果物件圖形包含循環參考。 完全與如此`Order`和`OrderDetail`類別，因為每個持有另的參考。 格式器會遵循撰寫的值，每個物件參考，並移圓形中。 因此，我們需要變更預設行為。

在 方案總管 中，展開 應用程式\_啟動資料夾，然後開啟名為 WebApiConfig.cs 的檔案。 將下列程式碼加入 `WebApiConfig` 類別：

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

此程式碼設定 JSON 格式器，來保留物件參考，並完全移除管線的 XML 格式器。 （您可以設定 XML 格式器，來保留物件參考，但更多的工作，我們只會為此應用程式需要 JSON。 如需詳細資訊，請參閱[處理循環的物件參考](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)。)

>[!div class="step-by-step"]
[上一頁](using-web-api-with-entity-framework-part-1.md)
[下一頁](using-web-api-with-entity-framework-part-3.md)
