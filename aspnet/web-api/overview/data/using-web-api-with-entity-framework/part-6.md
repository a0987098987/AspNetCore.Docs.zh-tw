---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: 建立 JavaScript 用戶端 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 29d50e448e6d282c7db06b9d1946ac221347e1ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879305"
---
<a name="create-the-javascript-client"></a>建立 JavaScript 用戶端
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](https://github.com/MikeWasson/BookService)

在本節中，您將建立使用 JavaScript 的 HTML 用戶端應用程式，而[解 Knockout.js](http://knockoutjs.com/)程式庫。 在階段中，我們會建置用戶端應用程式：

- 顯示書籍清單。
- 顯示活頁簿的詳細資料。
- 加入新的書籍。

Knockout 程式庫會使用模型-檢視-ViewModel (MVVM) 模式：

- **模型**商務網域中 （在我們的案例、 書籍和作者） 中的資料的伺服器端表示法。
- **檢視**是展示層 」 (HTML)。
- **檢視模型**是 JavaScript 物件所在的模型。 檢視模型是抽象的 ui 的程式碼。 它不了解 HTML 表示法。 相反地，它代表抽象功能的檢視，例如&quot;書籍清單&quot;。

檢視是資料繫結至檢視模型。 檢視模型的更新會自動反映在檢視中。 檢視模型也會取得事件從檢視中，例如按一下按鈕。

![](part-6/_static/image1.png)

這種方法容易變更版面配置和 UI 的應用程式，因為您可以變更繫結，而不用重寫的任何程式碼。 例如，您可能會顯示為項目清單`<ul>`，然後稍後變更為資料表。

## <a name="add-the-knockout-library"></a>加入 Knockout 程式庫

在 Visual Studio 中，從**工具**功能表上，選取**程式庫套件管理員**。 然後選取**Package Manager Console**。 在 [封裝管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](part-6/samples/sample1.cmd)]

此命令會將 Knockout 檔案加入至指令碼 資料夾。

## <a name="create-the-view-model"></a>建立檢視模型

加入名為 app.js 的指令碼資料夾的 JavaScript 檔案。 (在 方案總管 中，以滑鼠右鍵按一下指令碼 資料夾中，選取**新增**，然後選取**JavaScript 檔案**。)貼上下列程式碼：

[!code-javascript[Main](part-6/samples/sample2.js)]

在 Knockout，`observable`類別可讓資料繫結。 當可觀察的內容變更時，observable，以自行更新通知的所有資料繫結控制項。 (`observableArray`類別是陣列版本*observable*。)開始使用，我們的檢視模型中有兩個可預見值：

- `books` 保存書籍的清單。
- `error` 如果 AJAX 呼叫失敗，則包含錯誤訊息。

`getAllBooks`方法會 AJAX 呼叫以取得書籍的清單。 然後它會將結果推入`books`陣列。

`ko.applyBindings`方法是 Knockout 程式庫的一部分。 它會做為參數的檢視模型，並設定資料繫結。

## <a name="add-a-script-bundle"></a>新增指令碼組合

結合在一起是 ASP.NET 4.5，可讓您輕鬆地結合或多個檔案配套成單一檔案中的功能。 結合在一起會要求數目減少到伺服器，可以改善頁面載入時間。

開啟檔案的應用程式\_Start/BundleConfig.cs。 將下列程式碼加入至 RegisterBundles 方法。

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [上一頁](part-5.md)
> [下一頁](part-7.md)
