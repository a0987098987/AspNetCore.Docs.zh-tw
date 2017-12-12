---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: "建立單一 OData v4 中的使用 Web 應用程式開發介面 2.2 |Microsoft 文件"
author: rick-anderson
description: "本主題說明如何在 Web 應用程式開發介面 2.2 中的 OData 端點中定義為單一子句。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>建立單一 OData v4 中的使用 Web 應用程式開發介面 2.2
====================
由毛毯 Luo

> 傳統上，如果它封裝內的實體集，可能只存取實體。 但是 OData v4 提供另外兩個選項，單一和內含項目，這兩種 WebAPI 2.2 的支援。


本文示範如何在 Web 應用程式開發介面 2.2 中的 OData 端點中定義為單一子句。 為單一子句以及如何享有使用它的資訊，請參閱[使用單一定義特殊實體](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx)。 若要建立 Web API OData V4 端點，請參閱[建立 OData v4 端點使用 ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)。 

我們將建立單一 Web API 專案使用下列資料模型中：

![資料模型](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

名為單一`Umbrella`會根據型別定義`Company`，和名為實體`Employees`會根據型別定義`Employee`。

您可以從下載本教學課程中所用之解決方案[CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)。

## <a name="define-the-data-model"></a>定義資料模型

1. 定義 CLR 類型。

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. 產生的 CLR 型別為基礎的 EDM 模型。

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    在這裡，`builder.Singleton<Company>("Umbrella")`會告知模型產生器建立名為單一`Umbrella`中的 EDM 模型。

    產生的中繼資料將會如下所示：

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    從中繼資料我們可以看到的導覽屬性`Company`中`Employees`實體集繫結至 singleton `Umbrella`。 繫結是自動由`ODataConventionModelBuilder`，因為只有`Umbrella`具有`Company`型別。 如果模型中有任何模稜兩可，您可以使用`HasSingletonBinding`明確地為單一子句; 繫結的瀏覽屬性`HasSingletonBinding`有使用相同的效果`Singleton`CLR 型別定義中的屬性：

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>定義單一控制器

單一控制器繼承自 EntitySet 控制器，例如`ODataController`，而且單一控制器名稱應該是`[singletonName]Controller`。

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

若要處理不同類型的要求，動作必須是在控制器中預先定義。 **屬性路由**WebApi 2.2 中預設會啟用。 例如，若要定義的動作來處理查詢`Revenue`從`Company`使用路由屬性，使用下列命令：

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

如果您不願意定義每個動作的屬性，只會定義您的動作之後[OData 路由慣例](../odata-routing-conventions.md)。 則不需要查詢單一金鑰，因為單一控制器中定義的動作就會有稍微不同於 entityset 控制器中定義的動作。

如需參考，每個動作中定義的單一控制站的方法簽章如下所示。

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

基本上，這是您只需要在服務端上執行。 [範例專案](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)包含所有方案和示範如何使用單一的 OData 用戶端的程式碼。 建置用戶端中的步驟[建立 OData v4 用戶端應用程式](create-an-odata-v4-client-app.md)。

。 

*由於原始內容的這篇文章 Leo Hu。*
