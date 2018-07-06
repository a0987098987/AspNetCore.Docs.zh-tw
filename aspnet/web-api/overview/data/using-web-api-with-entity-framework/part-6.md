---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: 建立 JavaScript 用戶端 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: acadd8100b2698b4b17a750a02f875d4fd51c414
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826509"
---
<a name="create-the-javascript-client"></a>建立 JavaScript 用戶端
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](https://github.com/MikeWasson/BookService)

在本節中，您將建立使用 HTML、 JavaScript 的用戶端應用程式，而[Knockout.js](http://knockoutjs.com/)程式庫。 在階段中，我們將建置用戶端應用程式：

- 顯示 書籍清單。
- 顯示活頁簿詳細資料。
- 加入新的書籍。

油墨廓清程式庫會使用 Model View ViewModel (MVVM) 模式：

- **模型**商務網域 （在我們的案例、 書籍與作者） 中資料的伺服器端表示法。
- **檢視**是展示層 」 (HTML)。
- **檢視模型**是可儲存模型的 JavaScript 物件。 檢視模型是一種程式碼抽象概念的 ui。 它並不知道 HTML 的表示法。 相反地，它代表抽象功能的檢視，例如&quot;書籍清單&quot;。

檢視是資料繫結至檢視模型。 檢視模型的更新會自動反映在檢視中。 檢視模型也會取得事件從檢視中，例如按一下按鈕。

![](part-6/_static/image1.png)

這種方法輕鬆地變更版面配置和 UI 的應用程式，因為您可以變更繫結，不需要重新撰寫任何程式碼。 例如，您可能會顯示為項目清單`<ul>`，然後稍後再變更為資料表。

## <a name="add-the-knockout-library"></a>新增 Knockout 程式庫

在 Visual Studio 中，從**工具**功能表上，選取**程式庫套件管理員**。 然後選取**Package Manager Console**。 在 [套件管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](part-6/samples/sample1.cmd)]

此命令會將指令碼 資料夾中的 Knockout 檔案。

## <a name="create-the-view-model"></a>建立檢視模型

新增名為 app.js 的指令碼資料夾的 JavaScript 檔案。 (在 方案總管 中，以滑鼠右鍵按一下指令碼 資料夾中，選取**新增**，然後選取**JavaScript 檔案**。)貼上下列程式碼：

[!code-javascript[Main](part-6/samples/sample2.js)]

在 Knockout，`observable`類別可讓資料繫結。 在可預見值的內容變更時，可預見值會通知的所有資料繫結控制項，讓它們可以自行更新。 (`observableArray`類別是陣列的版本*observable*。)若要開始，我們的檢視模型有兩個可預見值：

- `books` 會保留在書籍清單。
- `error` AJAX 呼叫失敗時，請包含錯誤訊息。

`getAllBooks`方法進行 AJAX 呼叫以取得在書籍清單。 然後它會將推送到結果`books`陣列。

`ko.applyBindings`方法是 Knockout 程式庫的一部分。 它會做為參數的檢視模型，並設定資料繫結。

## <a name="add-a-script-bundle"></a>新增指令碼套件組合

統合是 ASP.NET 4.5，可讓您輕鬆地結合或配套成單一檔案的多個檔案中的功能。 統合至伺服器，以改善網頁載入時間減少要求的數目。

開啟檔案的應用程式\_Start/BundleConfig.cs。 將下列程式碼新增至 RegisterBundles 方法中。

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [上一頁](part-5.md)
> [下一頁](part-7.md)
