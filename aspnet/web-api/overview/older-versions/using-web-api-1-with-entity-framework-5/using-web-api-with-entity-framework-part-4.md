---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 第 4 部分： 新增管理員檢視 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 599f684ba200821d7fcd33819937c5a5b5a29ce8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371047"
---
<a name="part-4-adding-an-admin-view"></a>第 4 部分： 新增管理員檢視
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>新增系統管理員檢視

現在我們會向用戶端，並加入可從系統管理控制站使用資料的頁面。 頁面可讓使用者建立、 編輯或刪除產品，將 AJAX 要求傳送到控制器。

在方案總管 中，展開 Controllers 資料夾，然後開啟名為 HomeController.cs 檔案。 此檔案包含此 MVC 控制器。 新增名為`Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

**HttpRouteUrl**方法會建立 web API 的 URI，以及我們將檢視包供稍後使用。

接下來，將文字游標放在`Admin`動作方法，然後以滑鼠右鍵按一下並選取**加入檢視**。 此時會出現**加入檢視**對話方塊。

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

在 **加入檢視**對話方塊中，命名為"Admin"的檢視。 選取核取方塊**建立強型別檢視**。 底下**模型類別**，選取 [產品 (ProductStore.Models)]。 所有其他選項保留為其預設值。

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

按一下 **新增**新增名為 Admin.cshtml Views/Home 下的檔案。 開啟這個檔案，並新增下列 HTML。 此 HTML 會定義 頁面上的結構，但尚未有線設定任何功能。

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>建立系統管理員 頁面的連結

在 [方案總管] 中，展開 [Views] 資料夾，然後展開 Shared 資料夾。 開啟檔案，名為\_Layout.cshtml。 找出**ul**項目識別碼 = [功能表] 和 [系統管理] 檢視的動作連結：

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> 在範例專案中，我可以做幾個其他外觀變更，例如取代 「 您標誌的位置 」 的字串。 這些不會影響應用程式的功能。 您可以下載專案，並比較檔案。


執行應用程式，然後按一下出現在 [首頁] 頁面頂端的 「 管理員 」 連結。 系統管理員 頁面看起來應該如下所示：

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

權限現在頁面不會執行任何項目。 在下一步 區段中，我們將使用 Knockout.js 建立動態 UI。

## <a name="add-authorization"></a>新增授權

目前瀏覽網站的任何人都能存取系統管理員 頁面。 讓我們變更此選項可限制系統管理員的權限。

啟動新增"Administrator"角色和系統管理員使用者。 在方案總管 中，展開 篩選器 資料夾，然後開啟名為 InitializeSimpleMembershipAttribute.cs 檔案。 找出`SimpleMembershipInitializer`建構函式。 若要在呼叫之後**WebSecurity.InitializeDatabaseConnection**，新增下列程式碼：

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

這是應急的方法來新增 「 系統管理員 」 角色，並建立使用者角色。

在方案總管 中，展開 Controllers 資料夾，然後開啟 HomeController.cs 檔案。 新增**Authorize**屬性設定為`Admin`方法。

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

開啟 AdminController.cs 檔案並新增**Authorize**屬性設定為整個`AdminController`類別。

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC 和 Web API，同時定義**授權**不同命名空間中的屬性。 MVC 會使用**System.Web.Mvc.AuthorizeAttribute**，而 Web API 會使用**System.Web.Http.AuthorizeAttribute**。


現在只有系統管理員可以檢視 [系統管理] 頁面。 此外，如果您將 HTTP 要求傳送至系統管理控制站時，要求必須包含驗證 cookie。 如果沒有，則伺服器會傳送 HTTP 401 （未經授權） 回應。 您可以先將這在 Fiddler 中看到，傳送 GET 要求來`http://localhost:*port*/api/admin`。

> [!div class="step-by-step"]
> [上一頁](using-web-api-with-entity-framework-part-3.md)
> [下一頁](using-web-api-with-entity-framework-part-5.md)
