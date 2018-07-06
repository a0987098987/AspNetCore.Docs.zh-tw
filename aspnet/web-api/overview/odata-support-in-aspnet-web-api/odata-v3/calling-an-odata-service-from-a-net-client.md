---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: 呼叫 OData 服務的.NET 用戶端 (C#) |Microsoft Docs
author: MikeWasson
description: 本教學課程會示範如何從 C# 用戶端應用程式呼叫 OData 服務。 軟體版本，用於教學課程的 Visual Studio 2013 （適用於 Visual S...
ms.author: aspnetcontent
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 72dca7dc2ec27f15c9f0526621a713267f5835f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819551"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>從.NET 用戶端 (C#) 呼叫 OData 服務
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> 本教學課程會示範如何從 C# 用戶端應用程式呼叫 OData 服務。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) （適用於 Visual Studio 2012）
> - [WCF Data Services 用戶端程式庫](https://msdn.microsoft.com/library/cc668772.aspx)
> - Web API 2。 （使用 Web API 2 中，所建置 OData 服務的範例，但用戶端應用程式不相依於 Web API）。


在本教學課程中，我將逐步建立呼叫 OData 服務的用戶端應用程式。 OData 服務會公開下列實體：

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

下列文章說明如何在 Web API 中實作 OData 服務。 （您不需要讀取他們了解本教學課程中，不過）。

- [建立 Web API 2 中的 OData 端點](creating-an-odata-endpoint.md)
- [Web API 2 中的 OData 實體關聯性](working-with-entity-relations.md)
- [Web API 2 中的 OData 動作](odata-actions.md)

## <a name="generate-the-service-proxy"></a>產生服務 Proxy

第一個步驟是產生服務 proxy。 服務 proxy 是一個.NET 類別來定義存取 OData 服務的方法。 Proxy 會轉譯成 HTTP 要求的方法呼叫。

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

首先開啟 Visual Studio 中的 OData 服務專案。 按下 CTRL + F5，以在 IIS Express 在本機執行服務。 請注意本機位址，包括 Visual Studio 會將指派的通訊埠編號。 當您建立的 proxy 時，您將需要此位址。

接下來，開啟 Visual studio 的另一個執行個體，並建立主控台應用程式專案。 主控台應用程式將會是我們的 OData 用戶端應用程式。 （您也可以加入專案與服務相同的方案。）

> [!NOTE]
> 其餘的步驟，請參閱主控台專案。


在 [方案總管] 中，以滑鼠右鍵按一下**參考**，然後選取**加入服務參考**。

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

在 [**加入服務參考**] 對話方塊中，輸入的 OData 服務的位址：

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

何處*連接埠*的通訊埠編號。

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

針對**命名空間**，輸入 「 ProductService"。 此選項會定義 proxy 類別的命名空間。

按一下 [ **Go**]。 Visual Studio 會讀取 OData 中繼資料文件，來探索服務中的實體。

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

按一下 **確定**將 proxy 類別新增至您的專案。

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>建立服務 Proxy 類別的執行個體

在您`Main`方法，建立 proxy 類別的新執行個體，如下所示：

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

同樣地，使用實際的連接埠號碼正在執行您的服務。 當您部署您的服務時，您將使用即時服務的 URI。 您不需要更新的 proxy。

下列程式碼加入事件處理常式會列印至主控台視窗的要求 Uri。 此步驟並非必要，但很值得查看每個查詢的 Uri。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>查詢服務

下列程式碼會從 OData 服務取得產品的清單。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

請注意，您不需要撰寫任何程式碼來傳送 HTTP 要求，或剖析回應。 Proxy 類別會自動從事此當您列舉`Container.Products`中的集合**foreach**迴圈。

當您執行應用程式時，輸出看起來應該如下所示：

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

若要取得的識別碼實體，請使用`where`子句。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

本主題的其餘部分，我不會顯示整個`Main`函式，就呼叫服務所需的程式碼。

## <a name="apply-query-options"></a>套用查詢選項

OData 會定義[查詢選項](../supporting-odata-query-options.md)，可用來篩選、 排序、 分頁資料等等。 在服務 proxy，您可以使用各種 LINQ 運算式，以套用這些選項。

在本節中，我會說明簡單的範例。 如需詳細資訊，請參閱主題[LINQ 考量 (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) MSDN 上。

### <a name="filtering-filter"></a>篩選 ($filter)

若要篩選，請使用`where`子句。 下列範例會篩選依產品類別目錄。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

此程式碼會對應至下列 OData 查詢。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

請注意，proxy 會將轉換`where`OData 子句`$filter`運算式。

### <a name="sorting-orderby"></a>排序 ($orderby)

若要排序，請使用`orderby`子句。 下列範例會依價格，從最高到最低。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

以下是對應的 OData 要求。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>用戶端的分頁 （$skip 和 $top）

為大型實體集，用戶端可能會想要限制結果數目。 例如，用戶端可能會顯示 10 個項目一次。 這就叫做*用戶端分頁*。 (另外還有[伺服器端分頁](../supporting-odata-query-options.md#server-paging)，其中伺服器限制的結果數目。)若要執行用戶端分頁，請使用 LINQ**略過**並**採取**方法。 下列範例會略過前 40 的結果，並採用 10 的下一步。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

以下是對應的 OData 要求：

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>選取 ($select)，以及展開 ($expand)

若要加入相關的實體，使用`DataServiceQuery<t>.Expand`方法。 例如，若要納入`Supplier`針對每個`Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

以下是對應的 OData 要求：

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

若要變更回應的圖形，請使用 LINQ**選取**子句。 下列範例會取得每項產品，與任何其他屬性的名稱。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

以下是對應的 OData 要求：

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Select 子句可以包含相關的實體。 在此情況下，請勿呼叫**展開**; proxy 會自動包含擴充在此情況下。 下列範例會取得每個產品的供應商與名稱。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

以下是對應的 OData 要求。 請注意，它包含 **$expand**選項。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

如需有關 $select 和 $展開，請參閱 <<c0> [ 使用 $select，$expand、 和 Web API 2 中的 $value](../using-select-expand-and-value.md)。

## <a name="add-a-new-entity"></a>加入新的實體

若要新增新實體至實體集，請呼叫`AddToEntitySet`，其中*EntitySet*是實體集的名稱。 例如，`AddToProducts`將新`Product`到`Products`實體集。 當您產生 proxy 時，WCF Data Services 會自動建立這些強型別**AddTo**方法。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

若要新增兩個實體之間的連結，請使用**AddLink**並**SetLink**方法。 下列程式碼加入新的供應商和新的產品，並接著會建立它們之間的連結。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

使用**AddLink**導覽屬性時的集合。 在此範例中，我們會新增至產品`Products`上供應商的集合。

使用**SetLink**導覽屬性時的單一實體。 在此範例中，我們要設定`Supplier`產品上的屬性。

## <a name="update--patch"></a>更新/修補程式

若要更新實體，請呼叫**UpdateObject**方法。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

當您呼叫時，執行更新**SaveChanges**。 根據預設，WCF 會傳送 HTTP MERGE 要求。 **PatchOnUpdate**選項會告訴 WCF 要改為傳送的 HTTP PATCH。

> [!NOTE]
> 為什麼修補程式與合併？ 原始的 HTTP 1.1 規格 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) 未定義任何 HTTP 方法，以 「 部分更新 」 語意。 若要支援部分更新，OData 規格會定義合併式方法。 在 2010年[RFC 5789](http://tools.ietf.org/html/rfc5789)定義部分更新的 PATCH 方法。 您可以讀取此歷程記錄的一些[部落格文章](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx)WCF Data Services 部落格。 現在，修補程式，最好透過合併。 Web API scaffolding 所建立的 OData 控制器支援這兩種方法。


如果您想要取代整個實體 （PUT 語意），指定**ReplaceOnUpdate**選項。 這會導致傳送 HTTP PUT 要求的 WCF。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>刪除實體

若要刪除實體，請呼叫**DeleteObject**。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>叫用 OData 動作

在 OData 中，[動作](odata-actions.md)來新增伺服器端行為，不會輕易地定義為實體上的 CRUD 作業。

雖然 OData 中繼資料文件描述的動作，proxy 類別並不會為其建立任何強型別方法。 您可以使用泛型仍叫用 OData 動作**Execute**方法。 不過，您必須知道資料類型的參數和傳回值。

例如，`RateProduct`動作會將名為"Rating"類型的參數`Int32`，並傳回`double`。 下列程式碼示範如何叫用此動作。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

如需詳細資訊，請參閱 <<c0> [ 呼叫服務作業和動作](https://msdn.microsoft.com/library/hh230677.aspx)。

其中一個選項是擴充**容器**類別，以提供強型別的方法叫用的動作：

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
