---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: "動作和函數中 OData v4 使用 ASP.NET Web API 2.2 |Microsoft 文件"
author: MikeWasson
description: "在 OData 中，動作和函數是加入伺服器端行為，不會輕易地定義為實體上的 CRUD 作業的方式。 本教學課程示範如何..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>動作和函數中 OData v4 使用 ASP.NET Web API 2.2
====================
由[Mike Wasson](https://github.com/MikeWasson)

> 在 OData 中，動作和函數是加入伺服器端行為，不會輕易地定義為實體上的 CRUD 作業的方式。 本教學課程會示範如何將動作和函數加入至 OData v4 端點，使用 Web 應用程式開發介面 2.2。 本教學課程是根據本教學課程[建立 OData v4 端點使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
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
> ## <a name="tutorial-versions"></a>教學課程版本
> 
> 適用於 OData 版本 3，查看[中 ASP.NET Web API 2 OData 動作](../odata-v3/odata-actions.md)。


之間的差異*動作*和*函式*是動作可能有副作用，而且函式不相符。 動作和函數可以傳回資料。 動作的一些用法包括：

- 複雜的交易。
- 同時管理數個項目。
- 允許特定實體屬性的更新。
- 傳送不是實體的資料。

函式可用於直接至實體或集合傳回未對應的資訊。

動作 （或函式） 可以針對單一實體或集合。 在 OData 術語中，這是*繫結*。 您也可以讓&quot;未繫結&quot;動作/函式，它們被稱為靜態服務上的作業。

## <a name="example-adding-an-action"></a>範例： 新增動作

讓我們來定義要評等產品的動作。

> [!NOTE]
> 本教學課程是本教學課程[建立 OData v4 端點使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)


首先，新增`ProductRating`模型來表示評等。

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

也新增**DbSet**至`ProductsContext`類別，以便 EF 會在資料庫中建立的評等資料表。

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>新增 EDM 的動作

在 WebApiConfig.cs，加入下列程式碼：

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

**EntityTypeConfiguration.Action**方法會將動作加入至實體資料模型 (EDM)。 **參數**方法指定型別的參數的動作。

這個程式碼也會設定命名空間為 EDM。 命名空間很重要，因為動作 URI 包含完整的動作名稱：

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> 在一般的 IIS 設定中，此 URL 中的句點會導致 IIS 傳回錯誤 404。 您可以將下列區段加入至 Web.Config 檔案來解決此問題：

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>加入控制器方法的動作

若要啟用&quot;速率&quot;動作，將下列方法加入`ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

請注意方法名稱比對的動作名稱。 **[HttpPost]**屬性會指定方法為 HTTP POST 方法。

若要叫用動作時，用戶端會傳送 HTTP POST 要求，如下所示：

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

&quot;速率&quot;動作繫結至產品執行個體，因此動作 URI 是附加至實體 URI 的完整動作名稱。 (請記得我們設定的 EDM 命名空間為&quot;ProductService&quot;讓完整的動作名稱、 &quot;ProductService.Rate&quot;。)

要求的主體會包含動作參數做為 JSON 裝載。 Web 應用程式開發介面會自動將轉換的 JSON 裝載**ODataActionParameters**物件，它是只參數值的字典。 您可以使用此字典來存取控制站方法中的參數。

如果用戶端傳送的 action 參數中的錯誤，將值格式化的**ModelState.IsValid**為 false。 請檢查控制器方法中的這個旗標，並傳回錯誤**IsValid**為 false。

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>範例： 加入函式

現在讓我們加入的 OData 函數會傳回成本最高的產品。 如之前，第一個步驟新增 EDM 函式。 在 WebApiConfig.cs，加入下列程式碼。

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

在此情況下，函式繫結至產品集合中，而不是個別產品的執行個體。 用戶端傳送 GET 要求來叫用函式：

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

以下是這個函式的控制站方法：

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

請注意方法名稱的函數名稱相符。 **[HttpGet]**屬性會指定方法是 HTTP GET 的方法。

以下是 HTTP 回應：

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>範例： 加入未繫結的函數

前一個範例是繫結至集合的函式。 在下一個範例中，我們將建立*未繫結*函式。 未繫結函式稱為做為靜態服務上的作業。 在此範例中的函式會傳回給定的郵遞區號營業稅。

在到 WebApiConfig 檔案中，加入 EDM 中的函式：

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

請注意，我們正在撥打**函式**直接依據**ODataModelBuilder**，而不是集合的實體類型。 這會告知模型產生器函式是未繫結。

以下是實作的函式的控制站方法：

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

並不重要的 Web API 控制器會放置在這個方法。 您無法將其置於`ProductsController`，或定義不同的控制站。 **[ODataRoute]**屬性定義的函式的 URI 範本。

以下是用戶端的要求範例：

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

HTTP 回應：

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
