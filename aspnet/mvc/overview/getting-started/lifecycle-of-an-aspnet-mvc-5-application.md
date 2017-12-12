---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: "ASP.NET MVC 5 應用程式生命週期 |Microsoft 文件"
author: cephalin
description: "下載的 ASP.NET MVC 5 應用程式生命週期的圖表顯示的 PDF 文件。 生命週期本文提供 MVC 生命週期的高層級檢視..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 5692c43168eb261c91f40e2046897a1e5d31a028
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>ASP.NET MVC 5 應用程式生命週期
====================
由[Cephas 連結](https://github.com/cephalin)

[下載 PDF 文件](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

您可以在這裡下載的圖表的生命週期的每個 ASP.NET MVC 5 應用程式，使其無法接收 HTTP 要求傳送 HTTP 回應傳回至用戶端的 PDF 文件。 其設計是做為那些新 ASP.NET mvc 的教育性工具和也需要向下鑽研的應用程式的特定層面的使用者的參考。 PDF 文件具有下列功能：

- 相關[HttpApplication](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx)階段，協助您了解 MVC 整合至[ASP.NET 應用程式生命週期](https://msdn.microsoft.com/en-us/library/bb470252.aspx)。
- MVC 應用程式生命週期，您將可以了解每個 MVC 應用程式要求處理管線中通過的主要階段高層級檢視。  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- 詳細資料檢視會顯示向下的向下切入到要求處理管線的詳細資料。 您可以比較高層級檢視，以查看如何生命週期的詳細資料會收集到的不同階段的詳細資料檢視。 [下載 PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)若要查看較大的檢視。
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- 放置和上的所有可覆寫方法的目的[控制器](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller.aspx)要求處理管線中的物件。 您可能會或可能不需要覆寫任何一種方法，但請務必了解在應用程式生命週期中的角色，讓您可以撰寫程式碼，在適當的生命週期階段，您想要的效果。
- 顯示每個篩選器類型 （驗證、 授權、 動作和結果） 叫用方式爆向上圖表。
- 從詳細資料檢視中的每個點，以連結到有用的發行項或部落格。


## <a name="next-steps"></a>後續步驟

這份文件是否符合您的需求？ 我們非常感謝您的意見反應。 如果您有任何問題在 ASP.NET MVC 生命週期應用程式中[Stackoverflow](http://stackoverflow.com/help)和[ASP.NET MVC 論壇](https://forums.asp.net/1146.aspx)所要求的好地方。 請遵循[我](https://twitter.com/Cephas_MSFT)因此我最新的教學課程中的更新可能會發生在 twitter 上。
