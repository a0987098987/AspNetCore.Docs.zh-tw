---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: ASP.NET MVC 5 應用程式的生命週期 |Microsoft Docs
author: cephalin
description: 下載圖表上的 ASP.NET MVC 5 應用程式的生命週期的 PDF 文件。 此生命週期文件提供 MVC 生命週期的高層級檢視...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 98735f2e04bdd0f2fec19524e59f6272dbc4ca57
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393911"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>ASP.NET MVC 5 應用程式的生命週期
====================
藉由[Cephas 連結](https://github.com/cephalin)

[下載 PDF 文件](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

您可以在這裡下載的圖表生命週期的每個 ASP.NET MVC 5 應用程式，從接收 HTTP 要求傳送 HTTP 回應傳回至用戶端的 PDF 文件。 它是作為教育性工具的人不熟悉 ASP.NET MVC 和也需要向下鑽研至應用程式的特定層面的參考。 PDF 文件具有下列功能：

- 相關[HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)協助您了解 MVC 整合到階段[ASP.NET 應用程式生命週期](https://msdn.microsoft.com/library/bb470252.aspx)。
- MVC 應用程式生命週期中，您將可以了解每個 MVC 應用程式通過要求處理管線中的主要階段高層級檢視。  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- 詳細資料檢視會顯示向下鑽研，要求處理管線的詳細資料。 您可以比較高層級檢視，以查看如何生命週期的詳細資料會收集到的各個階段的詳細資料檢視。 [下載 PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)若要查看較大的檢視。
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- 位置與用途上的所有可覆寫方法[控制器](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx)要求處理管線中的物件。 您可能會或可能沒有需要覆寫任何一種方法，但務必了解在應用程式生命週期中的角色，以便您可以撰寫程式碼，在適當的生命週期階段，您想要的效果。
- 震撼人心向上圖表，顯示每個篩選器類型 （驗證、 授權、 動作和結果） 叫用方式。
- 從 詳細資料檢視中的每個點，以連結至篇實用的文章或部落格。


## <a name="next-steps"></a>後續步驟

這份文件是否符合您的需求？ 我們非常感謝您的意見反應。 如果您有任何問題的 ASP.NET MVC 生命週期中您的應用程式[Stackoverflow](http://stackoverflow.com/help)並[ASP.NET MVC 論壇](https://forums.asp.net/1146.aspx)是很棒的地方，要求。 請遵循[我](https://twitter.com/Cephas_MSFT)twitter，因此您可以取得我最新的教學課程的更新。
