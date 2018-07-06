---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: 建立單一 OData v4 中的使用 Web API 2.2 |Microsoft Docs
author: rick-anderson
description: 本主題說明如何在 Web API 2.2 中的 OData 端點定義單一值。
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: baccfed79949ae4efd45395bed3ad0058549cb4c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830512"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>建立單一 OData v4 中的使用 Web API 2.2
====================
由 Zoe Luo

> 傳統上，如果它封裝在實體集，可能只能存取實體。 但 OData v4 提供兩個額外的選項，單一和內含項目，這兩種 WebAPI 2.2 的支援。


本文說明如何在 Web API 2.2 中的 OData 端點定義單一值。 如需哪些 singleton 則和您如何受惠於使用該資訊，請參閱[來定義特殊的實體使用單一](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx)。 若要建立 Web API OData V4 端點，請參閱[建立 OData v4 端點使用 ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)。 

使用下列的資料模型在 Web API 專案中，我們會建立單一值：

![資料模型](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

名為單一`Umbrella`會根據型別定義`Company`，且實體集具名`Employees`會根據型別定義`Employee`。

您可以從下載本教學課程中使用的解決方案[CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)。

## <a name="define-the-data-model"></a>定義資料模型

1. 定義的 CLR 型別。

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. 產生的 CLR 型別為基礎的 EDM 模型。

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    在這裡，`builder.Singleton<Company>("Umbrella")`會告知模型產生器來建立名為單一`Umbrella`中的 EDM 模型。

    產生的中繼資料將會如下所示：

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    從中繼資料我們可以看到的導覽屬性`Company`中`Employees`實體集繫結至單一`Umbrella`。 繫結會自動進行`ODataConventionModelBuilder`，因為只有`Umbrella`具有`Company`型別。 如果在模型中沒有任何模稜兩可，您可以使用`HasSingletonBinding`來明確地將導覽屬性繫結至單一值;`HasSingletonBinding`具有相同的效果與使用`Singleton`CLR 型別定義中的屬性：

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>定義單一控制器

EntitySet 控制器，例如單一控制站會繼承自`ODataController`，而且單一控制站名稱應該`[singletonName]Controller`。

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

若要處理不同類型的要求，動作必須是在控制器中預先定義。 **屬性路由**WebApi 2.2 中的預設會啟用。 例如，若要定義的動作，以處理查詢`Revenue`從`Company`使用屬性路由中，使用下列命令：

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

如果您不願意定義每個動作的屬性，只會定義您遵循的動作[OData 路由慣例](../odata-routing-conventions.md)。 找不到需要查詢為單一子句的金鑰，因為單一控制器中定義的動作是 entityset 控制器中定義的動作稍有不同。

如需參考，如下所列的單一控制器中的每個動作定義的方法簽章。

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

基本上，這是您只需要在服務端上執行。 [範例專案](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)包含所有的解決方案以及示範如何使用單一的 OData 用戶端程式碼。 建置用戶端中的步驟[建立 OData v4 用戶端應用程式](create-an-odata-v4-client-app.md)。

。 

*感謝您來代表 Leo Hu 這篇文章的原始內容。*
