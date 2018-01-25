---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: "模型中 ASP.NET Web API 驗證 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 45b519af4073b62c8be1ca8951e44d6cf3cbe075
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="model-validation-in-aspnet-web-api"></a>ASP.NET Web API 中的模型驗證
====================
由[Mike Wasson](https://github.com/MikeWasson)

當用戶端會將資料傳送至您的 web 應用程式開發介面時，通常您要驗證資料，再進行任何處理。 本文將說明如何加上註解您的模型、 使用進行資料驗證的註解和處理您的 web 應用程式開發介面中的驗證錯誤。

## <a name="data-annotations"></a>資料註釋

在 ASP.NET Web API 中，您可以使用屬性[System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)在模型上設定屬性的驗證規則的命名空間。 請考慮下列模型：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

如果您已在 ASP.NET MVC 中使用模型驗證，這應該看起來很熟悉。 **需要**屬性指出，`Name`屬性不可為 null。 **範圍**屬性指出，`Weight`必須是介於 0 到 999 之間。

假設用戶端傳送 POST 要求的下列 JSON 表示法：

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

您可以看到用戶端未包含`Name`屬性標示為不需要。 當 Web 應用程式開發介面將轉換成 JSON`Product`執行個體，它會驗證`Product`針對驗證屬性。 在控制器動作中，您可以檢查模型是否為有效：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

模型驗證並不保證用戶端資料安全無虞。 應用程式的其他圖層中，可能會需要額外的驗證。 （例如，資料層可能會強制執行 foreign key 條件約束。）本教學課程[使用 Web API 和 Entity Framework](../data/using-web-api-with-entity-framework/part-1.md)探討這些問題。

**「 資料張貼 」**： 用戶端將被排除一些屬性時會張貼資料。 例如，假設用戶端會傳送下列：

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

在這裡，用戶端未指定值`Price`或`Weight`。 JSON 格式子會指派預設值是零到遺漏的屬性。

![](model-validation-in-aspnet-web-api/_static/image1.png)

模型狀態無效，因為 0 是有效的值，這些屬性。 這是問題，是否取決於您的案例。 比方說，在更新作業中，您可能想要區別 「 零 」 和 「 未設定。 」 若要強制用戶端設定值，將屬性設為可為 null，並設定**需要**屬性：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**「 過度張貼 」**： 用戶端也可以傳送*詳細*比您預期的資料。 例如: 

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

JSON，包括屬性 （[色彩]），不存在於`Product`模型。 在此情況下，只是 JSON 格式器會忽略此值。 （XML 格式器會執行相同的。）如果您的模型具有您預定處於唯讀模式的屬性，過度張貼會造成問題。 例如: 

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

您不想要更新的使用者`IsAdmin`屬性本身提高為系統管理員和 ！ 最安全的策略是使用完全符合用戶端允許傳送的模型類別：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Brad Wilson 的部落格文章"[vs 輸入驗證。模型驗證 ASP.NET MVC 中的](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)「 有探討資料張貼以及過度。 雖然文章是關於 ASP.NET MVC 2，問題是仍與 Web 應用程式開發介面。


## <a name="handling-validation-errors"></a>處理驗證錯誤

Web 應用程式開發介面不會自動傳回錯誤給用戶端驗證失敗時。 這是由控制器動作，以檢查模型狀態，並適當地回應。

您也可以建立動作篩選條件之前叫用控制器動作，檢查模型狀態。 下列程式碼顯示範例：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

如果模型驗證失敗，此篩選條件會傳回 HTTP 回應，其中包含驗證錯誤。 在此情況下，控制器動作不會叫用。

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

若要將此篩選器套用至所有的 Web API 控制器，將加入的篩選條件的執行個體**HttpConfiguration.Filters**集合在設定期間：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

另一個選項是設定為屬性的篩選器，在個別的控制站或控制器的動作：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
