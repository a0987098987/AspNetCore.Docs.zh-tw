---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: ASP.NET Web API 中傳送 HTML 表單資料： form-urlencoded 資料 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/15/2012
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2d01212cc408f8bb66fa3103464c9a1f7a1e21c6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831171"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>ASP.NET Web API 中傳送 HTML 表單資料： form-urlencoded 資料
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>第 1 部分： Form-urlencoded 資料

本文說明如何發佈到 Web API 控制器的 form-urlencoded 資料。

- [HTML 表單的概觀](#overview_of_html_forms)
- [傳送的複雜型別](#sending_complex_types)
- [透過 AJAX 的表單資料傳送](#sending_form_data_via_ajax)
- [傳送的簡單類型](#sending_simple_types)

> [!NOTE]
> [下載已完成的專案](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007)。


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>HTML 表單的概觀

HTML 表單使用取得，或是張貼到將資料傳送到伺服器。 **方法**屬性**表單**項目提供的 HTTP 方法：

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

預設方法是 GET。 如果此表單會使用 GET，資料會被編碼為查詢字串之 URI 中的表單。 如果表單會使用 POST，表單資料被放在要求主體中。 已張貼的資料，如**enctype**屬性會指定要求主體格式：

| enctype | 描述 |
| --- | --- |
| application/x-www-form-urlencoded | 表單資料會編碼成名稱/值組，類似於 URI 查詢字串。 這是 POST 的預設格式。 |
| multipart/form-data | 表單資料會編碼為多部分 MIME 訊息。 如果您要將檔案上傳到伺服器，請使用此格式。 |

這篇文章的第 1 部分探討 x-www-表單-urlencoded 格式。 [第 2 部分](sending-html-form-data-part-2.md)描述多部分 MIME。

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>傳送的複雜型別

一般而言，您將會傳送複雜型別，從數個表單控制項的值所組成。 請考慮下列模型表示的狀態更新：

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

以下是可接受的 Web API 控制器`Update`透過 POST 的物件。

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> 此控制器會使用[動作為基礎的路由](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name)，因此，路由範本&quot;api / {controller} / {action} / {id}&quot;。 用戶端會將資料公佈至&quot;/api/updates/complex&quot;。


現在讓我們編寫 HTML 表單提交狀態更新的使用者。

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

請注意，**動作**表單上的屬性是我們的控制器動作的 URI。 以下是表單中輸入一些值：

![](sending-html-form-data-part-1/_static/image1.png)

當使用者按一下送出時，瀏覽器會傳送 HTTP 要求如下所示：

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

請注意要求主體包含表單資料，格式為名稱/值組。 Web API 會自動轉換為名稱/值組的執行個體`Update`類別。

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>透過 AJAX 的表單資料傳送

當使用者提交表單時，瀏覽器瀏覽離開目前頁面，並呈現回應訊息的本文。 回應是 HTML 網頁時 [確定]。 使用 web API 中，不過，回應主體是通常是空的或包含結構化的資料，例如 JSON。 在此情況下，較為合理，將傳送使用 AJAX 的表單資料要求，讓網頁可以處理回應。

下列程式碼顯示如何發佈使用 jQuery 表單資料。

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

JQuery**提交**函式會以新的函式取代為表單動作。 這會覆寫 [提交] 按鈕的預設行為。 **序列化**函式會將表單資料序列化成名稱/值組。 若要將表單資料傳送至伺服器中，呼叫`$.post()`。

要求完成時，`.success()`或`.error()`處理常式會向使用者顯示適當的訊息。

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>傳送的簡單類型

在先前章節中，我們會傳送複雜型別，Web API 的還原序列化的模型類別的執行個體。 您也可以傳送簡單的型別，例如字串。

> [!NOTE]
> 在傳送之前的簡單類型，請考慮改為包裝複雜型別中的值。 這可讓您在伺服器端上的模型驗證的優點，並可讓您更輕鬆地擴充您的模型，如有需要。


基本的步驟，以傳送簡單的型別相同，但有兩個微妙的差異。 首先，在控制器中，您必須裝飾的參數名稱前面加**FromBody**屬性。

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

根據預設，Web API 會嘗試取得要求 URI 中的簡單類型。 **FromBody**屬性會告知 Web API，可讀取的要求主體中的值。

> [!NOTE]
> Web API 回應主體讀取最多一次，因此只有一個動作的參數可能來自要求主體。 如果您需要從要求主體中取得多個值，定義複雜型別。


第二，用戶端必須傳送的值，其格式如下：

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

具體來說，名稱/值組的名稱部分必須是空的簡單型別。 並非所有瀏覽器都支援此 HTML 表單，但您建立此格式在指令碼，如下所示：

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

以下是範例表單：

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

而以下是要提交的表單值的指令碼。 先前的指令碼的唯一差別是傳入的引數**張貼**函式。

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

您可以使用相同的方法來傳送簡單類型的陣列：

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>其他資源

[第 2 部分： 檔案上傳和多個 MIME](sending-html-form-data-part-2.md)
