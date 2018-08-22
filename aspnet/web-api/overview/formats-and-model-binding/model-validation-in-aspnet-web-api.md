---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: ASP.NET Web API 中的模型驗證 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/20/2012
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 611a6466e160387592df678b3b8556625ff8e234
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824501"
---
<a name="model-validation-in-aspnet-web-api"></a>ASP.NET Web API 中的模型驗證
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

當用戶端會將資料傳送至您的 web API 時，通常您想要驗證資料再進行任何處理。 本文說明如何將模型加上註解、 使用註解進行資料驗證和處理您的 web API 中的驗證錯誤。

## <a name="data-annotations"></a>資料註釋

在 ASP.NET Web API 中，您可以使用中的屬性[System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations)在模型上設定屬性的驗證規則的命名空間。 請考慮下列模型：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

如果您已在 ASP.NET MVC 中使用模型驗證，這應該看起來很熟悉。 **所需**屬性指出`Name`屬性不得為 null。 **範圍**屬性指出`Weight`必須是介於 0 到 999 之間。

假設用戶端傳送 POST 要求，使用下列的 JSON 表示法：

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

您可以看到用戶端未包含`Name`屬性，它會標示為必要。 當 Web API 會將轉換成 JSON`Product`執行個體，它會驗證`Product`針對驗證屬性。 在控制器動作中，您可以檢查模型是否有效：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

模型驗證並不保證用戶端的資料安全無虞。 在應用程式的其他層級，可能會需要額外的驗證。 （例如，資料層可能會強制執行 foreign key 條件約束。）本教學課程[使用 Web API 和 Entity Framework](../data/using-web-api-with-entity-framework/part-1.md)探討其中一些問題。

**「 不足公佈 」**： 用戶端將被排除某些屬性時會不足張貼。 例如，假設用戶端會傳送下列：

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

在這裡，用戶端未指定值`Price`或`Weight`。 JSON 格式器指派的預設值為 0 時可遺漏的屬性。

![](model-validation-in-aspnet-web-api/_static/image1.png)

模型狀態無效，因為 0 是有效的值，這些屬性。 是否為問題取決於您的案例。 比方說，在更新作業中，您可能想要區別 「 零 」 和 「 未設定 」。 若要強制用戶端設定值，將屬性設為可為 null 和 set**需要**屬性：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**「 過度發佈 」**： 用戶端也可以傳送*詳細*比您預期的資料。 例如: 

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

JSON，包含屬性 （[色彩]），不存在於`Product`模型。 在此情況下，JSON 格式器只會忽略此值。 （XML 格式器執行相同作業。）如果您的模型有您想要處於唯讀模式的屬性，過度發佈就會造成問題。 例如: 

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

您不希望使用者更新`IsAdmin`屬性並將自己提升至系統管理員 ！ 最安全的策略是使用完全符合 用戶端允許傳送的模型類別：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Brad Wilson 的部落格文章"[驗證輸入 vs。ASP.NET MVC 中的模型驗證](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)「 有不足張貼和 over-posting 棒的探討。 雖然文章是關於 ASP.NET MVC 2，問題是與 Web API 仍然相關。


## <a name="handling-validation-errors"></a>處理驗證錯誤

Web API 不會自動傳回錯誤給用戶端驗證失敗時。 負責檢查模型狀態，並適當地回應的控制器動作。

您也可以建立動作篩選條件在叫用控制器動作之前，檢查模型狀態。 下列程式碼顯示範例：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

如果模型驗證失敗，此篩選器就會傳回 HTTP 回應，其中包含驗證錯誤。 在此情況下，不會叫用控制器動作。

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

若要將此篩選套用至所有的 Web API 控制器，將新增的篩選，以執行個體**HttpConfiguration.Filters**設定期間的集合：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

另一個選項是設定為屬性的篩選器，在個別的控制器或控制器動作：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
