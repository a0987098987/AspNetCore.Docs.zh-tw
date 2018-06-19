---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 傳送 HTML 表單資料的 ASP.NET Web API： 表單 urlencoded 資料 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506937"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>傳送 HTML 表單資料的 ASP.NET Web API： 表單 urlencoded 資料
====================
由[Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>第 1 部分： 表單 urlencoded 資料

本文示範如何將表單 urlencoded 資料發佈到 Web API 控制器。

- [HTML 表單的概觀](#overview_of_html_forms)
- [傳送複雜型別](#sending_complex_types)
- [透過 AJAX 傳送表單資料](#sending_form_data_via_ajax)
- [傳送的簡單類型](#sending_simple_types)

> [!NOTE]
> [下載完成的專案](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007)。


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>HTML 表單的概觀

HTML 表單使用取得，或是張貼到將資料傳送到伺服器。 **方法**屬性**表單**項目提供的 HTTP 方法：

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

預設的方法為 GET。 如果此表單會使用 GET，資料會被編碼為查詢字串 URI 中的表單。 如果此表單會使用 POST，表單資料會在要求主體中。 已張貼資料**enctype**屬性指定的要求主體格式：

| enctype | 說明 |
| --- | --- |
| application/x-www-form-urlencoded | 表單資料編碼為名稱/值組，類似於 URI 查詢字串。 這是文章的預設格式。 |
| multipart/表單資料 | 表單資料編碼為多部分 MIME 訊息。 如果您要將檔案上傳到伺服器，請使用此格式。 |

第 1 部分，這篇文章探討 x-www-表單-urlencoded 格式。 [第 2 部分](sending-html-form-data-part-2.md)描述 MIME 多組件。

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>傳送複雜型別

一般而言，您會將傳送複雜類型，從多個表單控制項的值所組成。 請考慮下列代表的狀態更新的模型：

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

以下是可接受的 Web API 控制器`Update`透過 POST 的物件。

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> 此控制器會使用[動作為基礎的路由](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name)，因此路由範本是&quot;api / {controller} / {action} / {id}&quot;。 用戶端會將資料公佈至&quot;/api/updates/complex&quot;。


現在讓我們寫 HTML 表單送出狀態更新的使用者。

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

請注意，**動作**表單上的屬性是我們的控制器動作的 URI。 以下是具有某些輸入值的表單：

![](sending-html-form-data-part-1/_static/image1.png)

當使用者按一下時送出時，瀏覽器會傳送 HTTP 要求與下列類似：

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

請注意要求主體包含表單資料，格式為名稱/值組。 Web 應用程式開發介面會自動將轉換的名稱/值組的執行個體`Update`類別。

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>透過 AJAX 傳送表單資料

當使用者提交表單時，瀏覽器巡覽離開目前的頁面，並呈現回應訊息的本文。 回應是 HTML 網頁，這會是 [確定]。 與 web 應用程式開發介面，不過，回應主體是通常是空的或包含結構化的資料，例如 JSON。 在此情況下，它可讓更多的意義上，將傳送的表單資料，使用 AJAX 要求，讓網頁可以處理回應。

下列程式碼會示範如何發佈使用 jQuery 表單資料。

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

JQuery**提交**函式取代新的函式中的表單動作。 這會覆寫 [提交] 按鈕的預設行為。 **序列化**函式會將表單資料序列化成名稱/值組。 若要將表單資料傳送至伺服器，呼叫`$.post()`。

當要求完成時，`.success()`或`.error()`處理常式會向使用者顯示適當的訊息。

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>傳送的簡單類型

上一節中，我們會傳送 Web API 模型類別的執行個體還原序列化的複雜類型。 您也可以傳送簡單類型，例如字串。

> [!NOTE]
> 在傳送之前的簡單類型，請考慮改為包裝複雜類型中的值。 這可讓您在伺服器端的模型驗證的優點，並可讓您更輕鬆地擴充您的模型，如有需要。


傳送簡單類型的基本步驟都相同，但有兩個只有些微的差異。 首先，控制站，您必須裝飾的參數名稱與**FromBody**屬性。

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

根據預設，Web API 會嘗試取得要求 URI 中的簡單類型。 **FromBody**屬性會告知 Web API 來讀取的要求主體中的值。

> [!NOTE]
> Web API 會讀取回應主體最多一次，因此只有一個動作的參數可能來自要求主體。 如果您需要從要求主體中取得多個值，定義複雜型別。


第二，用戶端必須傳送的值，以下列格式：

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

具體而言，名稱/值組的名稱部分必須是空的簡單類型。 並非所有瀏覽器支援 HTML 表單，但您建立此格式在指令碼，如下所示：

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

以下是範例格式：

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

此外，這裡有要送出表單值的指令碼。 從先前的指令碼的唯一差異是傳入的引數**張貼**函式。

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

您可以使用相同的方法來傳送簡單類型的陣列：

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>其他資源

[第 2 部分： 檔案上傳和多部分 MIME](sending-html-form-data-part-2.md)
