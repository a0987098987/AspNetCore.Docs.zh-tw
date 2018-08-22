---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 第 5 部分： 使用 Knockout.js 建立動態 UI |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: d019941700992e173a5812b11b558b6726dfd809
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824919"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>第 5 部分： 使用 Knockout.js 建立動態 UI
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>使用 Knockout.js 建立動態 UI

在本節中，我們將使用 Knockout.js 將功能新增至 [系統管理] 檢視。

[Knockout.js](http://knockoutjs.com/)是 Javascript 程式庫，可讓您輕鬆地將 HTML 控制項繫結至資料。 Knockout.js 使用 Model View ViewModel (MVVM) 模式。

- *模型*商務網域 （在我們的案例、 products 和 orders） 中資料的伺服器端表示法。
- *檢視*是展示層 」 (HTML)。
- *檢視模型*是可儲存模型資料的 Javascript 物件。 檢視模型是程式碼的抽象概念的 UI。 它並不知道 HTML 的表示法。 相反地，它代表檢視，例如"的項目清單 」 的抽象功能。

檢視是資料繫結至檢視模型。 檢視模型的更新會自動反映在檢視中。 檢視模型也會從檢視中，例如按鈕點選，取得事件，並執行模型，例如建立訂單上的作業。

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

首先，我們會定義檢視模型。 在那之後，我們會將 HTML 標記繫結至檢視模型。

Admin.cshtml 中新增下列 Razor 區段：

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

您可以在檔案中的任何位置新增這一節。 呈現檢視時，區段會出現在底部的 [HTML] 頁面中，以滑鼠右鍵在關閉前&lt;/b&gt;標記。

所有的指令碼的這個頁面將會移內以註解的指令碼標記：

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

首先，定義的檢視模型類別：

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray**是一種特殊的 Knockout，呼叫中的物件*observable*。 從[Knockout.js 文件](http://knockoutjs.com/documentation/observables.html)： 可預見值會是 「 JavaScript 物件，可通知有關變更的訂閱者 」。 當可預見值的內容變更時，檢視會自動更新來比對。

若要填入`products`陣列中，對 web API 中的 AJAX 要求。 您應該記得我們儲存的檢視封包中的 API 的基底 URI (請參閱[第 4 部分](using-web-api-with-entity-framework-part-4.md)的教學課程)。

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

接下來，將功能加入檢視模型來建立、 更新及刪除產品。 這些函式會提交至 web API 的 AJAX 呼叫，並使用結果更新的檢視模型。

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

現在最重要的部分： 當 DOM 是 fulled 的載入、 呼叫**ko.applyBindings**函式並傳入的新執行個體`ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

**Ko.applyBindings**方法會啟用 Knockout，並將檢視模型至檢視。

既然我們已經檢視模型，我們可以建立繫結。 在 Knockout.js，您可以新增`data-bind`至 HTML 元素屬性。 例如，若要將 HTML 清單繫結至陣列，使用`foreach`繫結：

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

`foreach`繫結會逐一查看陣列，並建立子陣列中的每個物件的項目。 陣列物件的屬性可以參考的子項目上的繫結。

將 [產品更新] 清單中的下列繫結：

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

`<li>`項目範圍內，就會發生**foreach**繫結。 表示 Knockout 會轉譯一次到每個產品中的項目`products`陣列。 所有內的繫結`<li>`項目參考該產品的執行個體。 例如，`$data.Name`是指`Name`產品上的屬性。

若要設定的文字輸入的值時，使用`value`繫結。 按鈕會繫結至函式上模型檢視中，使用`click`繫結。 Product 執行個體是做為參數傳遞至每個函式。 如需詳細資訊， [Knockout.js 文件](http://knockoutjs.com/documentation/observables.html)有良好的各種繫結的描述。

接下來，新增的繫結**提交**新增產品表單上的事件：

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

這個繫結呼叫`create`函式在檢視模型來建立新的產品。

以下是 [系統管理] 檢視的完整程式碼：

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

執行應用程式、 系統管理員帳戶，登入，並按一下 「 系統管理員 」 連結。 您應該查看的產品清單，而且能夠建立、 更新或刪除產品。

> [!div class="step-by-step"]
> [上一頁](using-web-api-with-entity-framework-part-4.md)
> [下一頁](using-web-api-with-entity-framework-part-6.md)
