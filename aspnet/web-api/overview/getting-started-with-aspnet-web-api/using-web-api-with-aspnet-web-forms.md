---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: 使用 Web API 和 ASP.NET Web Form |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: 91811031b937d3ca05888ce8d4f28bcc33537c76
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400875"
---
<a name="using-web-api-with-aspnet-web-forms"></a>使用 Web API 和 ASP.NET Web Form
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

雖然 ASP.NET Web API 隨附 ASP.NET MVC，很容易就能將 Web API 新增至傳統的 ASP.NET Web Forms 應用程式。 本教學課程將逐步引導您逐步完成。

## <a name="overview"></a>總覽

若要在 Web Form 應用程式中使用 Web API，有兩個主要步驟：

- 新增衍生自的 Web API 控制器**ApiController**類別。
- 加入路由表與**應用程式\_啟動**方法。

## <a name="create-a-web-forms-project"></a>建立 Web Form 專案

啟動 Visual Studio，然後選取**新的專案**從**開始**頁面。 或從**檔案**功能表上，選取**新增**，然後**專案**。

在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。 底下**Visual C#**，選取**Web**。 在專案範本清單中，選取**ASP.NET Web Forms 應用程式**。 輸入專案的名稱，然後按一下**確定**。

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>建立模型和控制器

本教學課程使用相同的模型和控制器類別作為[開始使用](tutorial-your-first-web-api.md)教學課程。

首先，新增模型類別。 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**加入類別**。 將類別命名為產品，並加入下列實作：

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

接下來，新增至專案。，A 的 Web API 控制器*控制器*是 Web api 處理 HTTP 要求的物件。

在 [**方案總管] 中**，以滑鼠右鍵按一下專案。 選取 **加入新項目**。

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

底下**已安裝的範本**，展開**Visual C#** ，然後選取**Web**。 然後，從範本清單中，選取**Web API 控制器類別**。 控制器命名為"ProductsController 」，然後按一下**新增**。

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

**加入新項目**精靈會建立名為 ProductsController.cs 的檔案。 刪除精靈包含的方法，並新增下列方法：

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

如需有關此控制器中的程式碼的詳細資訊，請參閱[開始使用](tutorial-your-first-web-api.md)教學課程。

## <a name="add-routing-information"></a>新增路由的資訊

接下來，我們將新增的 URI 路由因此該格式的 Uri &quot;/api/產品/&quot;會路由傳送至控制器。

在 [**方案總管] 中**，連按兩下以開啟程式碼後置檔案 Global.asax.cs Global.asax。 新增下列**使用**陳述式。

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

然後新增下列程式碼**應用程式\_啟動**方法：

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

如需有關路由表的詳細資訊，請參閱[ASP.NET Web API 中的路由](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)。

## <a name="add-client-side-ajax"></a>新增用戶端 AJAX

這是您只需要建立 web 用戶端可以存取的 API。 現在讓我們新增 會使用 jQuery 來呼叫 API 的 HTML 網頁。

請確定您的主版頁面 (例如*Site.Master*) 包含`ContentPlaceHolder`使用`ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

開啟 Default.aspx 檔案。 取代的未定案文字，是在主要內容區段中，如所示：

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

接下來，新增 jQuery 原始程式檔中的參考`HeaderContent`區段：

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

注意： 您可以輕鬆地加入指令碼參考拖放檔案從**方案總管 中**的程式碼編輯器視窗。

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

下列 jQuery 指令碼標記中，新增下列指令碼區塊：

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

此指令碼文件載入時，提出 AJAX 要求以&quot;api/產品&quot;。 要求會以 JSON 格式傳回產品的清單。 指令碼會將 HTML 表格中的產品資訊。

當您執行應用程式時，它看起來應該像這樣：

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
