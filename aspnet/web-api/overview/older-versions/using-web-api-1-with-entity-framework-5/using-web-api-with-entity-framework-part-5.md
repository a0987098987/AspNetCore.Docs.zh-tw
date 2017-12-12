---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: "第 5 部分： 使用解 Knockout.js 建立動態 UI |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 20ebdb1b8ba710e0fbc6040f7cd4064b44658c53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>第 5 部分： 使用解 Knockout.js 建立動態 UI
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>建立動態 UI 與解 Knockout.js

在本節中，我們將使用解 Knockout.js，將功能加入至 [系統管理] 檢視。

[解 Knockout.js](http://knockoutjs.com/)是 Javascript 程式庫，以簡化 HTML 控制項繫結至資料。 解 Knockout.js 使用模型-檢視-ViewModel (MVVM) 模式。

- *模型*商務網域中 （在我們的案例、 products 和 orders） 中的資料的伺服器端表示法。
- *檢視*是展示層 」 (HTML)。
- *檢視模型*是 Javascript 物件，包含模型資料。 檢視模型是抽象的 ui 的程式碼。 它不了解 HTML 表示法。 相反地，它代表抽象檢視，例如 「 清單的項目 」 的功能。

檢視是資料繫結至檢視模型。 檢視模型的更新會自動反映在檢視中。 檢視模型也會從檢視中，例如按一下按鈕，取得事件，並執行模型，例如建立訂單上的作業。

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

首先，我們會定義檢視模型。 在這之後，我們會檢視模型繫結的 HTML 標記。

將下列 Razor 區段加入至 Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

您可以在檔案中任何位置加入這一節。 呈現檢視時，區段會出現在頁面底部的 HTML，以滑鼠右鍵在關閉前&lt;/b&gt;標記。

所有的指令碼的這個頁面將會移內以註解的指令碼標記：

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

首先，定義的檢視模型類別：

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray**是一種特殊的 Knockout，呼叫中的物件*observable*。 從[解 Knockout.js 文件](http://knockoutjs.com/documentation/observables.html): observable 為 「 JavaScript 物件通知有關變更的訂閱者 」。 當可觀察的內容變更時，檢視會自動更新來比對。

若要填入`products`陣列，請對 web API 的 AJAX 要求。 還記得我們針對檢視包中的 API，儲存的基底 URI (請參閱[第 4 部分](using-web-api-with-entity-framework-part-4.md)的教學課程)。

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

接下來，將功能加入檢視模型來建立、 更新及刪除產品。 這些函式會提交至 web API 的 AJAX 呼叫，並使用結果更新檢視模型。

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

現在最重要的部分： 當 DOM 是 fulled 載入、 呼叫**ko.applyBindings**函式，並傳入的新執行個體`ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

**Ko.applyBindings**方法啟動 Knockout 和繫結在一起檢視模型檢視。

現在，我們已經檢視模型，我們可以建立繫結。 在解 Knockout.js，您可以加入`data-bind`至 HTML 元素屬性。 例如，若要將 HTML 清單繫結至陣列，使用`foreach`繫結：

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

`foreach`繫結逐一查看陣列和陣列中建立的每個物件的項目子系。 陣列物件的屬性可以參考繫結的子項目上。

將下列繫結加入至 「 產品更新 」 清單：

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

`<li>`項目範圍內發生**foreach**繫結。 表示 Knockout 會轉譯一次中每項產品的項目`products`陣列。 所有的繫結內`<li>`元素參考該產品的執行個體。 例如，`$data.Name`指`Name`產品上的屬性。

若要設定的文字輸入值，請使用`value`繫結。 按鈕會繫結至函式上模型檢視中，使用`click`繫結。 產品執行個體做為參數傳遞至每個函式。 如需詳細資訊，[解 Knockout.js 文件](http://knockoutjs.com/documentation/observables.html)有良好的各種繫結的描述。

接下來，加入的繫結**提交**新增產品表單上的事件：

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

這個繫結呼叫`create`函式上檢視模型建立新的產品。

以下是管理檢視的完整程式碼：

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

執行應用程式的系統管理員帳戶，登入，按"Admin"連結。 您應該查看的產品清單，並能夠建立、 更新或刪除產品。

>[!div class="step-by-step"]
[上一頁](using-web-api-with-entity-framework-part-4.md)
[下一頁](using-web-api-with-entity-framework-part-6.md)
