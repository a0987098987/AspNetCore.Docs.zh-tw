---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: "Web API 和 ASP.NET Web Form 搭配使用 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/01/2017
---
<a name="using-web-api-with-aspnet-web-forms"></a>Web API 和 ASP.NET Web Form 搭配使用
====================
由[Mike Wasson](https://github.com/MikeWasson)

雖然 ASP.NET Web API 搭配 ASP.NET MVC 封裝時，所以可以輕鬆地加入傳統 ASP.NET Web Form 應用程式中 Web API。 本教學課程將引導您逐步完成。

## <a name="overview"></a>概觀

若要使用 Web API Web Form 應用程式中，有兩個主要步驟：

- 加入衍生自的 Web API 控制器**ApiController**類別。
- 加入路由表新增至**應用程式\_啟動**方法。

## <a name="create-a-web-forms-project"></a>建立 Web Form 專案

啟動 Visual Studio，然後選取**新專案**從**啟動**頁面。 或從**檔案**功能表上，選取**新增**然後**專案**。

在**範本**窗格中，選取**已安裝的範本**展開**Visual C#**節點。 在下**Visual C#**，選取**Web**。 在專案範本清單中選取**ASP.NET Web Form 應用程式**。 輸入專案的名稱，然後按一下**確定**。

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>建立模型和控制器

本教學課程使用相同的模型和控制器類別做為[入門](tutorial-your-first-web-api.md)教學課程。

首先，將模型類別。 在**方案總管 中**，以滑鼠右鍵按一下專案，然後選取**加入類別**。 將類別的產品，並加入下列實作：

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

接下來，將 Web API 控制器加入至專案。，*控制器*是負責處理 HTTP 要求 Web 應用程式開發介面的物件。

在**方案總管 中**，以滑鼠右鍵按一下專案。 選取**加入新項目**。

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

在下**已安裝的範本**，依序展開**Visual C#**選取**Web**。 然後，從範本清單中，選取**Web API 控制器類別**。 將控制器"ProductsController 」，然後按一下**新增**。

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

**加入新項目**精靈會建立名為 ProductsController.cs 的檔案。 刪除精靈包含的方法，並加入下列方法：

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

如需此控制器中的程式碼的詳細資訊，請參閱[入門](tutorial-your-first-web-api.md)教學課程。

## <a name="add-routing-information"></a>新增路由資訊

接下來，我們要加入的 URI 路由因此的該 Uri &quot;/api/產品/&quot;會路由傳送至控制器。

在**方案總管 中**，連按兩下以開啟程式碼後置檔案 Global.asax.cs Global.asax。 加入下列**使用**陳述式。

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

然後加入下列程式碼加入**應用程式\_啟動**方法：

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

如需有關路由表的詳細資訊，請參閱[中 ASP.NET Web API 的路由](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)。

## <a name="add-client-side-ajax"></a>新增用戶端 AJAX

這就是您要建立的 web 用戶端可以存取的 API。 現在讓我們加入用來呼叫 API 的 jQuery HTML 網頁。

請確定您的主版頁面 (例如， *Site.Master*) 包含`ContentPlaceHolder`與`ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

開啟 Default.aspx 檔案。 取代的未定案文字，主要內容 > 一節中所示：

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

接下來，將參考加入 jQuery 原始程式檔中`HeaderContent`> 一節：

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

注意： 您可以輕鬆地加入指令碼參考透過拖放方式將檔案從**方案總管 中**到程式碼編輯器視窗。

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

下方的 jQuery 指令碼標記中，加入下列指令碼區塊：

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

此指令碼文件載入時，會以 AJAX 要求&quot;api/產品&quot;。 要求會傳回 JSON 格式的產品清單。 指令碼會將產品資訊加入至 HTML 表格。

當您執行應用程式時，它看起來應該像這樣：

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
