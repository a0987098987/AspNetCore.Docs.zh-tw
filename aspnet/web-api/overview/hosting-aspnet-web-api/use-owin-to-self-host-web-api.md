---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: 使用 OWIN 自我裝載 ASP.NET Web API 2 |Microsoft Docs
author: rick-anderson
description: 本教學課程會示範如何使用 OWIN 自我裝載 Web API 架構的主控台應用程式中裝載 ASP.NET Web API。 Open Web Interface for.NET (OWIN) d...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 0d16498e94ac0a66c117ed057db398c14080beaa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826612"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>使用 OWIN 自我裝載 ASP.NET Web API 2
====================
藉由[Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> 本教學課程會示範如何使用 OWIN 自我裝載 Web API 架構的主控台應用程式中裝載 ASP.NET Web API。
> 
> [Open Web Interface for.NET](http://owin.org) (OWIN) 定義.NET web 伺服器和 web 應用程式之間的抽象概念。 OWIN 可分隔的伺服器，讓 OWIN 很適合用於自我裝載的 web 應用程式，在您自己的程序，IIS 外部的 web 應用程式。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) （也可用於 Visual Studio 2012）
> - Web API 2


> [!NOTE]
> 您可以在本教學課程中找到完整的原始程式碼[aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt)。


## <a name="create-a-console-application"></a>建立主控台應用程式

在上**檔案**功能表上，按一下**新增**，然後按一下**專案**。 從**已安裝的範本**，在 Visual C# 中，按一下**Windows** ，然後按一下 **主控台應用程式**。 將專案命名為"OwinSelfhostSample 」，然後按一下**確定**。

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>新增 Web API 和 OWIN 套件

從**工具**功能表上，按一下**程式庫套件管理員**，然後按一下**Package Manager Console**。 在 [套件管理員主控台] 視窗中，輸入下列命令：

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

這會安裝 WebAPI OWIN selfhost 封裝和所有必要的 OWIN 套件。

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>設定 Web API 的自我裝載

在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增** / **類別**若要加入新的類別。 將類別命名為 `Startup` 。

![](use-owin-to-self-host-web-api/_static/image5.png)

您可以將所有未定案程式碼，此檔案中取代為下列：

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>新增 Web API 控制器

接下來，新增 Web API 控制器類別。 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增** / **類別**若要加入新的類別。 將類別命名為 `ValuesController` 。

您可以將所有未定案程式碼，此檔案中取代為下列：

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>啟動 OWIN 主機，並使用 HttpClient 提出要求

您可以將所有的 Program.cs 檔案中的未定案程式碼取代為下列：

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>執行應用程式

若要執行應用程式，請在 Visual Studio 中按 F5。 輸出看起來應該如下所示：

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>其他資源

[Katana 專案概觀](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[裝載的 Azure 背景工作角色中的 ASP.NET Web API](host-aspnet-web-api-in-an-azure-worker-role.md)
