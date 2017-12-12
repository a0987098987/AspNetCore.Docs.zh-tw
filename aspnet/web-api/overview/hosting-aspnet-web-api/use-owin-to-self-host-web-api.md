---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: "使用 OWIN 自我裝載 ASP.NET Web API 2 |Microsoft 文件"
author: rick-anderson
description: "本教學課程會示範如何使用 OWIN 自我裝載的 Web API framework 主控台應用程式中裝載 ASP.NET Web API。 開啟 Web 介面的.NET (OWIN) d..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>使用 OWIN 自我裝載 ASP.NET Web API 2
====================
由[Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> 本教學課程會示範如何使用 OWIN 自我裝載的 Web API framework 主控台應用程式中裝載 ASP.NET Web API。
> 
> [開啟適用於.NET 的 Web 介面](http://owin.org)(OWIN) 定義.NET web 伺服器和 web 應用程式之間的抽象概念。 OWIN 以減少從伺服器，使得 OWIN 適合自我裝載的 web 應用程式，在您自己的處理序，在 IIS 外部的 web 應用程式。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) （也可用於 Visual Studio 2012）
> - Web API 2


> [!NOTE]
> 您可以在此教學課程中找到的完整原始程式碼[aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt)。


## <a name="create-a-console-application"></a>建立主控台應用程式

在**檔案**功能表上，按一下 **新增**，然後按一下 **專案**。 從**已安裝的範本**，在 Visual C# 中，按一下**Windows** ，然後按一下 **主控台應用程式**。 將專案命名為"OwinSelfhostSample 」，然後按一下**確定**。

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>加入 Web 應用程式開發介面和 OWIN 封裝

從**工具**功能表上，按一下 **程式庫套件管理員**，然後按一下  **Package Manager Console**。 在 [封裝管理員主控台] 視窗中，輸入下列命令：

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

這會安裝 WebAPI OWIN selfhost 封裝和所有必要的 OWIN 封裝。

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>設定 Web API 的自我裝載

在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增** / **類別**若要加入新的類別。 將類別命名為 `Startup` 。

![](use-owin-to-self-host-web-api/_static/image5.png)

以下列內容取代所有此檔案中的未定案程式碼：

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>加入 Web API 控制器

接下來，加入 Web API 控制器類別。 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增** / **類別**若要加入新的類別。 將類別命名為 `ValuesController` 。

以下列內容取代所有此檔案中的未定案程式碼：

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>啟動 OWIN 主機，並使用 HttpClient 要求

以下列內容取代所有在 Program.cs 檔案中的未定案程式碼：

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>執行應用程式

若要執行應用程式，請在 Visual Studio 中按 F5。 輸出看起來應該如下所示：

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>其他資源

[專案 Katana 的概觀](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[裝載 Azure 背景工作角色中的 ASP.NET Web API](host-aspnet-web-api-in-an-azure-worker-role.md)
