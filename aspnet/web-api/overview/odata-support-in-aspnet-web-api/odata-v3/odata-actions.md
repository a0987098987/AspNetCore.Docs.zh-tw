---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: "支援在 ASP.NET Web API 2 OData 動作 |Microsoft 文件"
author: MikeWasson
description: "在 OData 中，動作是加入伺服器端行為，不會輕易地定義為實體上的 CRUD 作業的方式。 動作的一些用法包括： 實作..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>支援在 ASP.NET Web API 2 OData 動作
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> 在 OData 中，*動作*會加入伺服器端行為，不會輕易地定義為實體上的 CRUD 作業的方式。 動作的一些用法包括：
> 
> - 實作複雜的交易。
> - 同時管理數個項目。
> - 允許特定實體屬性的更新。
> - 將資訊傳送到未定義實體中的伺服器。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Web API 2
> - OData 第 3 版
> - Entity Framework 6


## <a name="example-rating-a-product"></a>範例： 評等產品

在此範例中，我們想要讓使用者評等產品，然後再公開之每項產品的平均評等。 在資料庫上，我們將儲存評等做為索引鍵與產品的清單。

以下是我們可能會使用它來表示 Entity Framework 中的評等的模型：

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

但我們不想用戶端張貼`ProductRating`物件加入至 「 評等 」 集合。 直覺的方式，評等產品集合中，與相關聯且用戶端應該只需要文章的分級值。

因此，而不是使用一般的 CRUD 作業，我們會定義用戶端可以叫用動作產品上。 在 OData 術語中，動作是*繫結*產品實體。

>動作的伺服器上有副作用。 基於這個理由，它們會叫用使用 HTTP POST 要求。 動作可以有參數和傳回型別，服務中繼資料中所述。 用戶端傳送參數在要求主體中，且伺服器會在回應主體中傳送的傳回值。 要叫用 「 速率產品 」 動作，用戶端傳送 POST 至 URI，如下所示：

[!code-console[Main](odata-actions/samples/sample2.cmd)]

POST 要求中的資料是直接的產品評比：

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>宣告的實體資料模型中的動作

在 Web API 設定中，將動作加入至實體資料模型 (EDM):

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

此程式碼會定義為可以在產品實體執行的動作"RateProduct"。 它也會宣告採取的動作**int**參數名為"評等 」，並傳回**int**值。

## <a name="add-the-action-to-the-controller"></a>將動作加入至控制器

產品實體所繫結 」 RateProduct 」 動作。 若要實作動作，新增名為`RateProduct`產品控制站：

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

請注意方法名稱符合 EDM 中的動作名稱。 方法有兩個參數：

- *索引鍵*： 速率的產品金鑰。
- *參數*： 動作參數值的字典。

如果您使用的預設路由慣例，機碼的參數必須為 「 金鑰 」。 它也是一定要包含**[FromOdataUri]**屬性，如下所示。 這個屬性會告知使用 OData 語法規則，它會剖析要求 URI 中的索引鍵時的 Web API。

使用*參數*取得動作參數的字典：

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

如果用戶端傳送的 action 參數中正確格式化，值**ModelState.IsValid**為 true。 在此情況下，您可以使用**ODataActionParameters**字典，以便取得參數值。 在此範例中，`RateProduct`動作接受名為"評等 」 的單一參數。

## <a name="action-metadata"></a>動作的中繼資料

若要檢視服務中繼資料，GET 要求傳送至 /odata/$ 中繼資料。 以下是宣告的中繼資料的部分`RateProduct`動作：

[!code-xml[Main](odata-actions/samples/sample7.xml)]

**FunctionImport**項目宣告的動作。 大部分欄位都一目了然，但兩個值得注意的是：

- **Isbindable 時**表示可以叫用動作上，在目標實體至少部分時間。
- **IsAlwaysBindable**表示可以永遠目標實體上叫用動作。

不同的是，有些動作永遠都可以使用用戶端使用，但其他動作可能會取決於實體的狀態。 例如，假設您定義 「 採購 」 動作。 您可以只購買是庫存項目。 如果項目存貨不足時，用戶端無法叫用該動作。

當您定義 EDM**動作**方法會建立可繫結的動作：

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

我將討論不一定為可繫結的動作 (也稱為*暫時性*動作) 本主題稍後。

## <a name="invoking-the-action"></a>叫用動作

現在讓我們來看看如何在用戶端會叫用這個動作。 假設用戶端想要讓識別碼的產品為等級 2 = 4。 以下是範例要求訊息，要求主體中使用 JSON 格式：

[!code-console[Main](odata-actions/samples/sample9.cmd)]

以下是回應訊息：

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>繫結至實體集的動作

在上述範例中，動作會繫結至單一實體： 用戶端的程度來分級單一產品。 您也可以結合動作至實體的集合。 只要進行下列變更：

在 EDM 中，將動作加入至實體的**集合**屬性。

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

在控制器方法中，省略*金鑰*參數。

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

現在用戶端會叫用 Products 實體集的動作：

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>集合參數的動作

動作可以有參數接受值的集合。 在 EDM 中，使用**CollectionParameter&lt;T&gt;** 來宣告參數。

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

這會宣告名為"評等 」，可接受的集合參數**int**值。 在控制器方法中，您仍可以獲得參數值從**ODataActionParameters**物件，但現在值是**ICollection&lt;int&gt;** 值：

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>暫時性的動作

在 「 RateProduct 」 範例中，使用者永遠會率產品，因此永遠都是可用的動作。 但是某些動作取決於實體的狀態。 例如，在影片出租服務中，「 結帳 」 動作不一定可用。 （它相依該視訊的複本是否可以使用）。這種類型的動作稱為*暫時性*動作。

服務中繼資料中的暫時性的動作有**IsAlwaysBindable**等於 false。 是實際的預設值，讓中繼資料看起來像這樣：

[!code-xml[Main](odata-actions/samples/sample16.xml)]

以下這之所以重要的原因： 如果動作是暫時性的則伺服器需要告知用戶端時的動作是否可用。 它會在實體中包含的動作連結。 以下是電影實體的範例：

[!code-console[Main](odata-actions/samples/sample17.cmd)]

「 #CheckOut"屬性包含簽出動作的連結。 找不到動作，伺服器會忽略連結。

若要宣告 EDM 中的暫時性動作，請呼叫**TransientAction**方法：

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

此外，您必須提供傳回特定實體的動作連結的函式。 設定此函式，藉由呼叫**HasActionLink**。 您可以撰寫函式做為 lambda 運算式：

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

如果動作是否可用，lambda 運算式會傳回連結至動作。 OData 序列化程式包含此連結時，它會序列化實體。 無法使用此動作時，此函數會傳回`null`。

## <a name="additional-resources"></a>其他資源

[OData 動作範例](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
