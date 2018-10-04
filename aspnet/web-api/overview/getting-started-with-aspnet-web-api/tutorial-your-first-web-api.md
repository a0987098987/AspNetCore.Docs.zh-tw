---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: 開始使用 ASP.NET Web API 2 (C#)
author: MikeWasson
description: HTTP 不只是提供 web 頁面。 它也是強大的平台，建置公開 （expose) 服務和資料的 Api。 HTTP 是簡單、 彈性和 ubiq...
ms.author: riande
ms.date: 11/28/2017
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 62e99a41ba935470c39476c9aea8ee4193543425
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795289"
---
<a name="get-started-with-aspnet-web-api-2-c"></a>開始使用 ASP.NET Web API 2 (C#)
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

HTTP 不只是提供 web 頁面。 HTTP 也是強大的平台，建置公開 （expose) 服務和資料的 Api。 HTTP 是簡單、 彈性且無所不在。 您可以將幾乎任何平台會有 HTTP 程式庫，因此 HTTP 服務可以連線到各種用戶端，包括瀏覽器、 行動裝置，以及傳統桌面應用程式。

ASP.NET Web API 是用於建置 web Api 在.NET framework 的架構。 在本教學課程中，您將使用 ASP.NET Web API 來建立 web API 會傳回一份產品。

## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- Web API 2

請參閱[使用 ASP.NET Core 和 Visual Studio for Windows 建立 web API](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api)本教學課程的較新版本。

## <a name="create-a-web-api-project"></a>建立 Web API 專案

在本教學課程中，您將使用 ASP.NET Web API 來建立 web API 會傳回一份產品。 前端的 web 網頁會使用 jQuery 來顯示結果。

![](tutorial-your-first-web-api/_static/image1.png)

啟動 Visual Studio，然後選取**新的專案**從**開始**頁面。 或從**檔案**功能表上，選取**新增**，然後**專案**。

在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。 底下**Visual C#**，選取**Web**。 在專案範本清單中，選取**ASP.NET Web 應用程式**。 將專案命名為"ProductsApp 」，然後按一下**確定**。

![](tutorial-your-first-web-api/_static/image2.png)

在 **新的 ASP.NET 專案**對話方塊中，選取**空白**範本。 底下&quot;新增的資料夾和核心參考&quot;，檢查**Web API**。 按一下 [確定 **Deploying Office Solutions**]。

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> 您也可以建立使用 Web API 專案&quot;Web API&quot;範本。 Web API 範本會使用 ASP.NET MVC 提供 API 說明頁面。 我正在使用空白範本本教學課程中，因為我想要顯示沒有 MVC 的 Web API。 一般情況下，您不需要知道要使用 Web API 的 ASP.NET MVC。


## <a name="adding-a-model"></a>新增模型

「模型」是代表應用程式中資料的物件。 ASP.NET Web API 可以自動序列化至 JSON、 XML 或某些其他格式，您的模型，然後將序列化的資料寫入 HTTP 回應訊息的本文。 只要用戶端可以讀取序列化格式，它可以還原序列化物件。 大部分的用戶端可以剖析 XML 或 JSON。 此外，用戶端可以指定它想要藉由設定 Accept 標頭在 HTTP 要求訊息中的哪一種格式。

讓我們開始建立簡單的模型，代表產品。

如果沒有顯示 [方案總管] 中，按一下**檢視**功能表，然後選取**方案總管 中**。 在 [方案總管] 中，以滑鼠右鍵按一下 [模型] 資料夾。 從操作功能表中，選取**新增**然後選取**類別**。

![](tutorial-your-first-web-api/_static/image4.png)

將類別命名為&quot;產品&quot;。 新增下列屬性以`Product`類別。

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>新增控制器

在 Web API 中，*控制器*是處理 HTTP 要求的物件。 我們將新增可以傳回一份產品或單一產品識別碼所指定的控制站

> [!NOTE]
> 如果您已使用 ASP.NET MVC，您已熟悉控制站。 Web API 控制器 MVC 控制器類似，但繼承**ApiController**而不是類別**控制站**類別。

在**方案總管 中**，以滑鼠右鍵按一下 [控制器] 資料夾。 選取 **新增**，然後選取**控制站**。

![](tutorial-your-first-web-api/_static/image5.png)

在 **新增 Scaffold**對話方塊中，選取**Web API 控制器-空白**。 按一下 [加入] 。

![](tutorial-your-first-web-api/_static/image6.png)

在 [**新增控制器**] 對話方塊中，將控制器命名&quot;ProductsController&quot;。 按一下 [加入] 。

![](tutorial-your-first-web-api/_static/image7.png)

Scaffolding 會建立名為 ProductsController.cs 在 Controllers 資料夾中的檔案。

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> 您不需要將您的控制站放入名為控制器的資料夾。 資料夾名稱只是便利方式來組織您的原始程式檔。


如果此檔案尚未開啟，按兩下檔案以開啟它。 您可以將此檔案中的程式碼取代為下列：

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

為了簡化範例中，產品會儲存在控制器類別內的固定陣列。 當然，實際的應用程式中，您會查詢資料庫，或使用其他外部資料來源。

控制器會定義傳回產品的兩個方法：

- `GetAllProducts`方法會傳回整個清單的產品**IEnumerable&lt;產品&gt;** 型別。
- `GetProduct`方法查閱單一產品依其識別碼。

就這麼容易！ 您必須使用 web API。 每個控制站上的方法對應至一或多個 Uri:

| 控制器方法 | URI |
| --- | --- |
| GetAllProducts | / api/產品 |
| GetProduct | /api/產品/*識別碼* |

針對`GetProduct`方法中，*識別碼*URI 中是預留位置。 例如，若要取得識別碼 5 的產品，URI 是`api/products/5`。

如需有關如何 Web API 會將 HTTP 要求路由至控制器方法的詳細資訊，請參閱[ASP.NET Web API 中的路由](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)。

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>呼叫 Web API 以 Javascript 和 jQuery

在本節中，我們將新增使用 AJAX 呼叫 web API 的 HTML 網頁。 我們將 jQuery 進行 AJAX 呼叫並以結果更新頁面。

在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增**，然後選取**新項目**。

![](tutorial-your-first-web-api/_static/image9.png)

在 **加入新項目**對話方塊中，選取**Web**節點下的**Visual C#**，然後選取**HTML 網頁**項目。 將頁面命名&quot;index.html&quot;。

![](tutorial-your-first-web-api/_static/image10.png)

您可以將此檔案中的所有項目取代為下列：

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

您可以使用幾種方式來取得 jQuery。 在此範例中，我使用了[Microsoft Ajax CDN](../../../ajax/cdn/overview.md)。 您也可以下載從[http://jquery.com/](http://jquery.com/)，和 ASP.NET"Web API 專案範本包含 jQuery 以及。

### <a name="getting-a-list-of-products"></a>取得產品清單

若要取得的產品清單，將傳送至 HTTP GET 要求給&quot;/api/產品&quot;。

JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/)函式會將 AJAX 要求。 回應包含 JSON 物件的陣列。 `done`函式會指定如果要求成功呼叫的回呼。 在回呼中，我們可以更新 DOM 的產品資訊。

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>取得產品識別碼

若要取得的產品識別碼，將傳送至 HTTP GET 要求給&quot;/api/產品/*識別碼*&quot;，其中*識別碼*是產品識別碼。

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

我們仍然呼叫`getJSON`傳送 AJAX 要求，但這次我們 ID 放在要求 URI 中。 此要求的回應是單一產品的 JSON 表示法。

## <a name="running-the-application"></a>執行應用程式

按 f5 鍵開始偵錯應用程式。 網頁看起來應該如下所示：

![](tutorial-your-first-web-api/_static/image11.png)

若要取得的產品識別碼，請輸入識別碼，再按一下 [搜尋]:

![](tutorial-your-first-web-api/_static/image12.png)

如果您輸入無效的識別碼時，伺服器就會傳回 HTTP 錯誤：

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>若要檢視的 HTTP 要求和回應中使用 F12

當您使用的 HTTP 服務時，它可以是很適合用來查看 HTTP 要求，並要求訊息。 您可以使用 Internet Explorer 9 中的 F12 開發人員工具來執行這項操作。 從 Internet Explorer 9 中，按下**F12**來開啟 [工具]。 按一下**網路** 索引標籤，然後按**開始擷取**。 現在請返回 web 頁面，然後按**F5**重新載入網頁。 Internet Explorer 會擷取瀏覽器與 web 伺服器之間的 HTTP 流量。 [摘要] 檢視會顯示頁面的所有網路流量：

![](tutorial-your-first-web-api/_static/image14.png)

找出項目相對的 uri"api/產品 /"。 選取此項目，然後按一下**前往 詳細檢視**。 在詳細資料 檢視中，有一些索引標籤來檢視要求和回應標頭和本文。 例如，如果您按一下**要求標頭**索引標籤上，您可以看到用戶端已要求&quot;application/json&quot; Accept 標頭中。

![](tutorial-your-first-web-api/_static/image15.png)

如果您按一下 [回應內文] 索引標籤，您可以看到如何將產品清單已序列化為 JSON。 其他瀏覽器具有類似的功能。 另一個有用的工具是[Fiddler](http://www.fiddler2.com/fiddler2/)、 web 偵錯 proxy。 您可以使用 Fiddler 來檢視您的 HTTP 流量，以及撰寫 HTTP 要求，這可讓您在要求中的 HTTP 標頭的完整控制。

## <a name="see-this-app-running-on-azure"></a>請參閱在 Azure 上執行此應用程式

若要查看為即時 web 應用程式正在執行的完成的網站嗎？ 您可以部署您的 Azure 帳戶的應用程式的完整版本，只要按一下下面的按鈕。

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

您需要有 Azure 帳戶才能將此解決方案部署至 Azure。 如果您還沒有帳戶，您會有下列選項：

- [免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度來試用 Azure 付費服務，您可以使用和甚至用後最多，您可保留帳戶，並使用免費的 Azure 服務。
- [啟用 MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶信用額度每月都會提供您 Azure 付費服務，您可以使用。

## <a name="next-steps"></a>後續步驟

- 支援 POST、 PUT 和 DELETE 動作，並將寫入到資料庫的 HTTP 服務的更完整範例，請參閱 <<c0> [ 與 Entity Framework 6 中使用 Web API 2](../data/using-web-api-with-entity-framework/part-1.md)。
- 如需有關建立 HTTP 服務之上的流暢並反應快速的 web 應用程式的詳細資訊，請參閱[ASP.NET 單一頁面應用程式](../../../single-page-application/index.md)。
- 如需如何將 Visual Studio web 專案部署至 Azure App Service 的資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。
