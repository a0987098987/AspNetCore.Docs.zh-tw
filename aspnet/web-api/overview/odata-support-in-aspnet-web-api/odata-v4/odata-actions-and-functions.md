---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: 動作和函式在 OData v4 中的使用 ASP.NET Web API 2.2 |Microsoft Docs
author: MikeWasson
description: 在 OData 中，動作和函式會將不會輕易地定義為實體上的 CRUD 作業的伺服器端行為的方式。 本教學課程示範如何...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: c78a9acabcb346b33a4fe53aa0d18ef67ddcaf33
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371959"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>動作和函式在 OData v4 中的使用 ASP.NET Web API 2.2
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

> 在 OData 中，動作和函式會將不會輕易地定義為實體上的 CRUD 作業的伺服器端行為的方式。 本教學課程會示範如何將 OData v4 端點，使用 Web API 2.2 中的動作和函式。 本教學課程是根據本教學課程[建立 OData v4 端點使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Web API 2.2
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>教學課程的版本
> 
> 適用於 OData 版本 3，請參閱[ASP.NET Web API 2 中的 OData 動作](../odata-v3/odata-actions.md)。


之間的差異*動作*並*函式*是動作可能有副作用，而且函式不這麼做。 動作和函式可以傳回資料。 動作的部分用法包括：

- 複雜的交易。
- 一次處理數個實體。
- 允許特定實體屬性的更新。
- 傳送不是實體的資料。

函式可用於直接至實體或集合傳回未對應的資訊。

動作 （或函式） 可以為單一實體或集合目標。 這是在 OData 術語*繫結*。 您也可以&quot;未繫結&quot;動作/函式，名為靜態服務上的作業。

## <a name="example-adding-an-action"></a>範例： 加入動作

讓我們定義產品評分的動作。

> [!NOTE]
> 本教學課程是根據本教學課程[建立 OData v4 端點使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)


首先，新增`ProductRating`來表示評等模型。

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

也加入**DbSet**到`ProductsContext`類別，因此，EF 會建立在資料庫中的評等資料表。

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>將動作加入 EDM

在 WebApiConfig.cs 中，新增下列程式碼：

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

**EntityTypeConfiguration.Action**方法會將 entity data model (EDM) 中的動作。 **參數**方法指定動作的具型別的參數。

此程式碼也會設定為 EDM 命名空間。 命名空間很重要，因為動作 URI 包含完整的動作名稱：

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> 在典型的 IIS 設定中，此 URL 中的句點會導致 IIS 傳回 404 錯誤。 您可以藉由將您的 Web.Config 檔案中的下一節來解決此問題：

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>新增動作的控制器方法

若要啟用&quot;速率&quot;動作，將下列方法加入`ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

請注意方法名稱比對的動作名稱。 **[HttpPost]** 屬性可讓您指定的方法是 HTTP POST 方法。

若要叫用動作，用戶端會傳送 HTTP POST 要求，如下所示：

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

&quot;速率&quot;動作繫結到 Product 執行個體，因此動作 URI 是附加至實體 URI 的完整動作名稱。 (您應該記得我們將 EDM 命名空間設定為&quot;ProductService&quot;，因此，完整的動作名稱&quot;ProductService.Rate&quot;。)

要求的主體會包含動作參數作為 JSON 承載。 Web API 會自動將轉換的 JSON 承載，以**ODataActionParameters**物件，也就是只是參數值的字典。 您可以使用此字典，存取控制器方法中的參數。

如果用戶端傳送錯誤的動作參數，值格式化**ModelState.IsValid**為 false。 檢查此旗標，在您的控制器方法中，並傳回錯誤，如果**IsValid**為 false。

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>範例： 加入函式

現在讓我們新增 OData 函式會傳回最便宜的產品。 如之前，第一個步驟加入至 EDM 的函式。 在 WebApiConfig.cs 中，新增下列程式碼。

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

在此情況下，函式繫結至產品集合，而不是個別產品執行個體。 用戶端叫用函式，藉由傳送 GET 要求：

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

以下是此函式的控制器方法：

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

請注意方法名稱的函數名稱相符。 **[HttpGet]** 屬性可讓您指定的方法是 HTTP GET 方法。

以下是 HTTP 回應：

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>範例： 加入未繫結的函式

前一個範例是繫結至集合的函式。 在下一個範例中，我們將建立*未繫結*函式。 作為服務上的靜態作業呼叫未繫結的函式。 在此範例中的函式會傳回指定的郵遞區號的營業稅。

在 WebApiConfig 檔案中，加入至 EDM 的函式：

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

請注意，我們會撥打**函式**直接依據**ODataModelBuilder**，而不是實體類型或集合。 這會告知模型產生器函式是未繫結。

以下是實作函式的控制器方法：

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

不論您放置在這個方法的 Web API 控制器。 您可以將它放`ProductsController`，或定義個別的控制器。 **[ODataRoute]** 屬性定義的函式的 URI 範本。

以下是範例用戶端要求：

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

HTTP 回應：

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
