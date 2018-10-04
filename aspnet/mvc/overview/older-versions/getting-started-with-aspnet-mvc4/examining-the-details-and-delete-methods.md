---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
title: 檢查 Details 和 Delete 方法 |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本就可以使用這裡使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 更容易遵循，並示範...
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 11425ff3-09fc-4efa-be9a-b53bce503460
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 1e23189fe927a5145647baa1f8886c4aed057b78
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578337"
---
<a name="examining-the-details-and-delete-methods"></a>檢查 Details 和 Delete 方法
====================
藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > 本教學課程中的更新的版本可[此處](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 更容易遵循，並示範更多的功能。


在本教學課程的這個部分，您將會檢查自動產生`Details`和`Delete`方法。

## <a name="examining-the-details-and-delete-methods"></a>檢查 Details 和 Delete 方法

開啟`Movie`控制器，並檢查`Details`方法。

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

建立這個動作方法的 MVC scaffolding 引擎會將顯示 HTTP 要求叫用方法的註解。 在此情況下很`GET`三個 URL 區段，要求`Movies`控制器`Details`方法和`ID`值。

程式碼第一次可讓您輕鬆地搜尋資料使用`Find`方法。 方法內建的重要安全性功能是，程式碼會確認`Find`方法已找到電影之前的程式碼嘗試執行任何動作。 比方說，駭客可能會導致發生錯誤的站台的連結所建立的 URL 變更`http://localhost:xxxx/Movies/Details/1`為類似`http://localhost:xxxx/Movies/Details/12345`（或不代表實際電影的一些其他值）。 如果您未檢查是否有 null 的電影，null 的電影會導致資料庫錯誤。

檢查 `Delete` 和 `DeleteConfirmed` 方法。

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

請注意，`HTTP Get``Delete`方法並不會刪除指定的電影，它會傳回電影的檢視設定，您可以提交 (`HttpPost`) 刪除... 如果您執行刪除作業以回應 GET 要求 (或是執行相關編輯作業、建立作業或任何會變更資料的其他作業)，則會造成安全性漏洞。 如需詳細資訊，請參閱 Stephen walther 所撰寫的部落格項目[ASP.NET MVC 秘訣 #46 — 不用刪除連結，因為它們會造成安全性漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)。

我們將可刪除資料的 `HttpPost` 方法命名為 `DeleteConfirmed`，以提供 HTTP POST 方法的唯一簽章或名稱。 這兩個方法簽章如下所示：

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

通用語言執行平台 (CLR) 需要多載方法，以提供唯一的參數簽章 (方法名稱相同但參數清單不同)。 不過，此處您需要兩個 Delete 方法，一個用於 GET-和另一個用於 POST 兩者都有相同的參數簽章。 (它們都需要接受單一整數作為參數)。

若要排序時，您可以執行兩件事。 其中一個是為方法提供不同的名稱。 這是 scaffolding 機制在上述範例採取的方法。 不過，這麼做會導致一個小問題：ASP.NET 會依名稱將 URL 區段與動作方法對應，一旦您重新命名方法，路由通常就會找不到這個方法。 解決辦法正如您看到的這個範例：將 `ActionName("Delete")` 屬性新增至 `DeleteConfirmed` 方法。 這會有效地執行對應的路由系統，讓 URL，包括<em>/Delete/</em>針對 POST 要求將會找到`DeleteConfirmed`方法。

若要避免具有相同名稱和簽章的方法有問題的另一個常見方法是人為方式變更 POST 方法，以包含未使用的參數的簽章。 比方說，有些開發人員將參數類型`FormCollection`傳遞至 POST 方法，然後只需不使用參數：

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>總結

您現在有完整的 ASP.NET MVC 應用程式將資料儲存在本機的 DB 資料庫。 您可以建立、 讀取、 更新、 刪除和搜尋電影。

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>後續步驟

在您建置並測試 web 應用程式之後下, 一個步驟是將它提供給其他人透過網際網路使用。 若要這麼做，您必須將它部署至 web 主控提供者。 Microsoft 提供免費的 web 裝載中的最多 10 個 web sites[免費的 Windows Azure 試用帳戶](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)。 我建議您接下來會依照我教學課程[將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 應用程式部署至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 絕佳的教學課程是 Tom Dykstra 中繼層級[建立 ASP.NET MVC 應用程式的 Entity Framework 資料模型](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 [Stackoverflow](http://stackoverflow.com/help)而[ASP.NET MVC 論壇](https://forums.asp.net/1146.aspx)是很大的位置，以詢問問題。 請遵循[我](https://twitter.com/RickAndMSFT)twitter，因此您可以取得我最新的教學課程的更新。

意見反應非常歡迎畫面。

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [上一步](adding-validation-to-the-model.md)
