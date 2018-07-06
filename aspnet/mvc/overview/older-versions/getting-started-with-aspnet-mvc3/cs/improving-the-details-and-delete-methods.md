---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: 改善 Details 和 Delete 方法 (C#) |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 17789aa9e9388f3b05ece953cfc0f50ffa510f52
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803768"
---
<a name="improving-the-details-and-delete-methods-c"></a>改善 Details 和 Delete 方法 (C#)
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程中的更新的版本可[此處](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 更容易遵循，並示範更多的功能。
> 
> 
> 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 在開始之前，請確定您已安裝符合下列先決條件。 您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附了 C# 原始程式碼的 Visual Web Developer 專案。 [下載 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 Visual Basic，切換到[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。


在本教學課程的這個部分，您要進行一些改良，以自動產生`Details`和`Delete`方法。 這些變更不必要的但有少數的程式碼的小型個位元，您可以輕鬆地加強應用程式。

## <a name="improving-the-details-and-delete-methods"></a>改善 Details 和 Delete 方法

當您包含 scaffold`Movie`控制器，ASP.NET MVC 產生的程式碼，很適合，但是，可在短短的小型變更更穩固。

開啟`Movie`控制器及修改`Details`方法，藉由傳回`HttpNotFound`找電影不到。 您也應該修改`Details`方法，以設定識別碼傳遞給它的預設值。 (已進行類似變更`Edit`方法中的[第 6 部分](examining-the-edit-methods-and-edit-view.md)本教學課程。)不過，您必須變更的傳回型別`Details`方法，從`ViewResult`要`ActionResult`，因為`HttpNotFound`方法不會傳回`ViewResult`物件。 下列範例顯示修改後`Details`方法。

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

程式碼第一次可讓您輕鬆地搜尋資料使用`Find`方法。 一項重要的安全性功能，我們內建的方法是，程式碼會確認`Find`方法已找到電影之前的程式碼嘗試執行任何動作。 比方說，駭客可能會導致發生錯誤的站台的連結所建立的 URL 變更`http://localhost:xxxx/Movies/Details/1`為類似`http://localhost:xxxx/Movies/Details/12345`（或不代表實際電影的一些其他值）。 如果您不知道有 null 的電影的檢查，這可能導致資料庫錯誤。

同樣地，變更`Delete`並`DeleteConfirmed`方法，以指定的 ID 參數的預設值，並傳回`HttpNotFound`找電影不到。 已更新`Delete`中的方法`Movie`控制器如下所示。

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

請注意，`Delete`方法不會刪除資料。 如果您執行刪除作業以回應 GET 要求 (或是執行相關編輯作業、建立作業或任何會變更資料的其他作業)，則會造成安全性漏洞。 如需詳細資訊，請參閱 Stephen walther 所撰寫的部落格項目[ASP.NET MVC 秘訣 #46 — 不用刪除連結，因為它們會造成安全性漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)。

我們將可刪除資料的 `HttpPost` 方法命名為 `DeleteConfirmed`，以提供 HTTP POST 方法的唯一簽章或名稱。 這兩個方法簽章如下所示：

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

Common language runtime (CLR) 需要有唯一的簽章 （名稱相同，不同的參數清單） 的多載的方法。 不過，此處您需要兩個 Delete 方法，一個用於 GET-和另一個用於 POST 兩者都需要相同的簽章。 (它們都需要接受單一整數作為參數)。

若要排序時，您可以執行兩件事。 其中一個是為方法提供不同的名稱。 這是我們在上述範例他。 不過，這麼做會導致一個小問題：ASP.NET 會依名稱將 URL 區段與動作方法對應，一旦您重新命名方法，路由通常就會找不到這個方法。 解決辦法正如您看到的這個範例：將 `ActionName("Delete")` 屬性新增至 `DeleteConfirmed` 方法。 這會有效地執行對應的路由系統，讓 URL，包括<em>/Delete/</em>針對 POST 要求將會找到`DeleteConfirmed`方法。

若要避免具有相同名稱和簽章的方法有問題的另一個方法是人為方式變更 POST 方法，以包含未使用的參數的簽章。 比方說，有些開發人員將參數類型`FormCollection`傳遞至 POST 方法，然後只需不使用參數：

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>總結

您現在有完整的 ASP.NET MVC 應用程式在 SQL Server Compact 資料庫中儲存資料。 您可以建立、 讀取、 更新、 刪除和搜尋電影。

![](improving-the-details-and-delete-methods/_static/image1.png)

此基本教學課程中有您開始進行控制站，將它們關聯的檢視，並傳遞硬式編碼的資料。 然後您會建立並設計資料模型。 Entity Framework Code First 資料模型的過程中，從建立資料庫和 ASP.NET MVC scaffolding 系統自動產生動作方法和檢視表的基本 CRUD 作業。 接著，您會新增搜尋表單，可讓使用者搜尋資料庫。 您變更了資料庫以包含新的資料行的資料，，然後更新兩個頁面，來建立並顯示這項新資料。 標記中的屬性與資料模型新增驗證`DataAnnotations`命名空間。 在用戶端和伺服器上，就會執行產生的驗證。

如果您想要部署您的應用程式，很有幫助第一項測試您的本機 IIS 7 伺服器上的應用程式。 您可以使用此[Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;)連結，以啟用 ASP.NET 應用程式的 IIS 設定。 請參閱下列的部署連結：

- [ASP.NET 部署內容對應](https://msdn.microsoft.com/library/dd394698.aspx)
- [啟用 IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Web 應用程式專案部署](https://msdn.microsoft.com/library/dd394698.aspx)

現在建議您前往我們的中繼層級[建立 ASP.NET MVC 應用程式的 Entity Framework 資料模型](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)並[MVC Music 市集](../../mvc-music-store/mvc-music-store-part-1.md)教學課程中，瀏覽[ASP.NETMSDN 上的文章](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)，並簽出許多的影片和資源[ https://asp.net/mvc ](https://asp.net/mvc)更進一步了解 ASP.NET MVC ！ [ASP.NET MVC 論壇](https://forums.asp.net/1146.aspx)是一個很好詢問的問題。

敬祝您使用愉快！

--Scott Hanselman ([ http://hanselman.com ](http://hanselman.com)並[ @shanselman ](http://twitter.com/shanselman)在 Twitter 上) 和 Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [上一步](adding-validation-to-the-model.md)
