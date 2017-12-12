---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
title: "改善的詳細資料和 Delete 方法 (VB) |Microsoft 文件"
author: Rick-Anderson
description: "本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: c5c14ef0-c128-4dc1-8c01-7f0fdb09e411
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 24c986f7ec8376bc997f1ebc575338772507cbc9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="improving-the-details-and-delete-methods-vb"></a>改善的詳細資料和 Delete 方法 (VB)
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 開始之前，請確定您已安裝下面所列的必要條件。 您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附在 Visual Web Developer 專案中的使用 VB.NET 原始程式碼。 [下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 C#，切換至[C# 版本](../cs/improving-the-details-and-delete-methods.md)本教學課程。


在這個部分的教學課程中，您要進行一些改良的自動產生`Details`和`Delete`方法。 這些變更不是必要項目，但有幾個程式碼小個位元，您可以輕鬆地加強應用程式。

## <a name="improving-the-details-and-delete-methods"></a>改善的詳細資料和 Delete 方法

當您建立結構`Movie`控制站，ASP.NET MVC 產生的程式碼正常運作的很好，但是，可以更健全與少數進行小變更進行。

開啟`Movie`控制器及修改`Details`方法藉由傳回`HttpNotFound`找電影不到。 您也應該修改`Details`方法來設定預設值傳遞給它的識別碼。 (您做了類似的變更`Edit`方法中的[第 6 部分](examining-the-edit-methods-and-edit-view.md)本教學課程。)不過，您必須變更傳回型別`Details`方法從`ViewResult`至`ActionResult`，因為`HttpNotFound`方法不會傳回`ViewResult`物件。 下列範例顯示已修改`Details`方法。

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample1.vb)]

程式碼第一次輕鬆地搜尋資料使用`Find`方法。 一項重要的安全性功能，我們內建的方法是，程式碼驗證`Find`方法發現電影，這個程式碼嘗試執行與它的任何項目之前。 比方說，駭客可能會導致發生錯誤的站台變更建立者 」 從連結的 URL`http://localhost:xxxx/Movies/Details/1`為類似`http://localhost:xxxx/Movies/Details/12345`（或其他值並不代表實際的影片）。 如果沒有 null 電影的檢查，這可能導致資料庫錯誤。

同樣地，變更`Delete`和`DeleteConfirmed`方法指定為 ID 參數的預設值，並傳回`HttpNotFound`找電影不到。 已更新`Delete`方法`Movie`控制器如下所示。

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample2.vb)]

請注意，`Delete`方法不會刪除資料。 如果您執行刪除作業以回應 GET 要求 (或是執行相關編輯作業、建立作業或任何會變更資料的其他作業)，則會造成安全性漏洞。 如需詳細資訊，請參閱 Stephen Walther 部落格文章： [ASP.NET MVC 秘訣 #46-請勿使用刪除連結，因為它們會產生安全性漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)。

我們將可刪除資料的 `HttpPost` 方法命名為 `DeleteConfirmed`，以提供 HTTP POST 方法的唯一簽章或名稱。 這兩個方法簽章如下所示：

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample3.vb)]

Common language runtime (CLR) 需要有唯一的簽章 （名稱相同，不同的參數清單） 的多載的方法。 不過，您需要在這裡兩種刪除方法-一個用於 GET-，另一個用於 POST 兩者都需要相同的簽章。 (它們都需要接受單一整數作為參數)。

若要排序時，您可以執行兩件事。 其中一個是讓方法不同的名稱。 這是我們在他前面範例。 不過，這麼做會導致一個小問題：ASP.NET 會依名稱將 URL 區段與動作方法對應，一旦您重新命名方法，路由通常就會找不到這個方法。 解決辦法正如您看到的這個範例：將 `ActionName("Delete")` 屬性新增至 `DeleteConfirmed` 方法。 這會有效地執行對應的路由系統，讓 URL 包含*/Delete/*POST 要求會發現`DeleteConfirmed`方法。

若要避免具有相同名稱和簽章的方法有問題的另一個方法是以人為方式變更為包含未使用的參數 POST 方法的簽章。 比方說，有些開發人員將參數類型`FormCollection`傳遞至 POST 方法，然後只要不使用參數：

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample4.vb)]

## <a name="wrapping-up"></a>總結

現在，您會有完整的 ASP.NET MVC 應用程式將資料儲存在 SQL Server Compact 資料庫中。 您可以建立、 讀取、 更新、 刪除及搜尋電影。

![](improving-the-details-and-delete-methods/_static/image1.png)

此基本教學課程了您開始進行控制站，關聯的檢視，並傳遞硬式編碼的資料。 然後您會建立並設計資料模型。 Entity Framework Code First 即時資料模型中建立資料庫，並在 ASP.NET MVC scaffolding 系統自動產生的動作方法和執行基本 CRUD 作業的檢視。 然後，您會加入搜尋表單，讓使用者可以搜尋資料庫。 您變更了資料庫以包含新的資料行的資料，，然後更新兩個頁面，以建立及顯示新的資料。 將標記與屬性的資料模型新增驗證`DataAnnotations`命名空間。 在用戶端和伺服器上，產生的驗證將會執行。

如果您想要部署應用程式，則第一項測試您的本機 IIS 7 伺服器上的應用程式很有幫助。 您可以使用這個[Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;)連結，以啟用 ASP.NET 應用程式的 IIS 設定。 請參閱下列的部署連結：

- [ASP.NET 部署內容地圖](https://msdn.microsoft.com/en-us/library/dd394698.aspx)
- [啟用 IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Web 應用程式專案部署](https://msdn.microsoft.com/en-us/library/dd394698.aspx)

現在鼓勵您移到我們的中繼層級[建立 ASP.NET MVC 應用程式的 Entity Framework 資料模型](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)和[MVC Music Store](../../mvc-music-store/mvc-music-store-part-1.md)教學課程中，瀏覽[ASP.NETMSDN 上的發行項](https://msdn.microsoft.com/en-us/library/gg416514(VS.98).aspx)，和簽出許多視訊和資源的[https://asp.net/mvc](https://asp.net/mvc)若要了解更多關於 ASP.NET MVC ！ [ASP.NET MVC 論壇](https://forums.asp.net/1146.aspx)是一個很好詢問的問題。

享受 ！

— Scott Hanselman ([http://hanselman.com](http://hanselman.com)和[ @shanselman ](http://twitter.com/shanselman) Twitter 上) 以及 Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

>[!div class="step-by-step"]
[上一步](adding-validation-to-the-model.md)
