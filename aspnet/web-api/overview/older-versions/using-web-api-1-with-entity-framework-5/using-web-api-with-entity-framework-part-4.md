---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 第 4 部分： 加入系統管理員檢視 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: cbf42f1dbd744d7b85dde7d2dcd99a13c6208a13
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-4-adding-an-admin-view"></a>第 4 部分： 加入系統管理員檢視
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>新增系統管理員檢視

現在我們將開啟用戶端，並加入頁面可以取用從系統管理控制站的資料。 頁面可讓使用者建立、 編輯或刪除產品 AJAX 要求傳送到控制器。

在方案總管 中，展開 Controllers 資料夾，然後開啟名為 HomeController.cs 檔案。 此檔案包含此 MVC 控制器。 新增名為`Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

**HttpRouteUrl**方法會建立對 web API，URI 和我們存放此檢視的屬性包中的更新版本。

接下來，將文字資料指標內`Admin`動作方法，然後以滑鼠右鍵按一下，然後選取**加入檢視**。 這會顯示**加入檢視**對話方塊。

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

在**加入檢視**對話方塊中，檢視 「 系統管理員 」。 選取標示的核取方塊**建立強型別檢視**。 在下**模型類別**，選取 「 產品 (ProductStore.Models) 」。 將其他所有選項都保留為其預設值。

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

按一下**新增**加入名為 Admin.cshtml Views/Home 下的檔案。 開啟此檔案並加入以下的 HTML。 此 HTML 會定義的頁面結構，但尚未有線設定任何功能。

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>建立管理頁面的連結

在方案總管 中，展開 Views 資料夾，然後展開 Shared 資料夾。 開啟的檔案命名\_Layout.cshtml。 找出**ul**項目 id =""，並在 系統管理 檢視的動作連結：

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> 在範例專案中，我變更幾個其他外觀，例如字串 「 您的標誌在此"來取代。 這些不會影響應用程式的功能。 您可以下載專案，並比較檔案。


執行應用程式，然後按一下 [首頁] 的上方都會出現的"Admin"連結。 [管理] 頁面看起來應該如下所示：

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

權限，頁面不會執行任何動作。 在下一步 區段中，我們會使用解 Knockout.js 建立動態的 UI。

## <a name="add-authorization"></a>新增授權

目前存取給任何人瀏覽的網站管理頁面。 讓我們來變更這項限制系統管理員的權限。

開始加入 「 系統管理員 」 角色和系統管理員使用者。 在方案總管 中，展開篩選器 資料夾，然後開啟名為 InitializeSimpleMembershipAttribute.cs 的檔案。 找出`SimpleMembershipInitializer`建構函式。 若要在呼叫之後**WebSecurity.InitializeDatabaseConnection**，加入下列程式碼：

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

這是應急的方法，加入 「 系統管理員 」 角色並建立使用者角色。

在方案總管 中，展開 Controllers 資料夾，然後開啟 HomeController.cs 檔案。 新增**授權**屬性`Admin`方法。

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

開啟 AdminController.cs 檔案並加入**授權**屬性為整個`AdminController`類別。

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC 和 Web API 兩者都定義**授權**不同命名空間中的屬性。 MVC 會使用**System.Web.Mvc.AuthorizeAttribute**，而 Web API 所使用**System.Web.Http.AuthorizeAttribute**。


現在只有系統管理員可以檢視管理頁面。 此外，如果您的系統管理控制站來傳送 HTTP 要求，要求必須包含驗證 cookie。 如果沒有，則伺服器會傳送 HTTP 401 （未經授權） 回應。 您可以在看到 Fiddler 的 GET 要求傳送到`http://localhost:*port*/api/admin`。

> [!div class="step-by-step"]
> [上一頁](using-web-api-with-entity-framework-part-3.md)
> [下一頁](using-web-api-with-entity-framework-part-5.md)
