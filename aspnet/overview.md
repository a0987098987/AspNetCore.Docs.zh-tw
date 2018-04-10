---
uid: overview
title: ASP.NET 概觀 |Microsoft 文件
author: rick-anderson
description: ASP.NET，可用的架構，用於建立網站、 web 應用程式和 web Api 的簡介。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2010
ms.topic: article
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: 0ba7814d4004b17e678eab9a2a41a6d6f34773e1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
# <a name="aspnet-overview"></a>ASP.NET 概觀

ASP.NET 是免費的 web 架構建置絕佳的網站和使用 HTML、 CSS 和 JavaScript 的 web 應用程式。 您也可以建立 Web 應用程式開發介面，並使用即時技術，例如 Web 通訊端。

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/)是 ASP.NET 的替代方式。  請參閱[ASP.NET 與 ASP.NET Core 之間選擇的指引](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework)。

## <a name="get-started"></a>開始使用

[Visual Studio 社群 2017年](https://www.visualstudio.com/downloads/)、 適用於在 Windows 上的 ASP.NET 釋放 IDE。

## <a name="websites-and-web-applications"></a>網站和 web 應用程式

 ASP.NET 提供三個架構建立 web 應用程式： Web Form、 ASP.NET MVC 和 ASP.NET Web Pages。 所有的三個架構已穩定並且成熟，，您可以使用任何語言建立絕佳的 web 應用程式。 無論您選擇何種架構，您會收到所有的優點和 everywhere ASP.NET 的功能。

每個架構為目標的不同的開發樣式。 您選擇一個取決於程式設計資產 （知識、 技術和開發體驗） 的組合，您要建立，應用程式和您已經熟悉的開發方式的類型。

以下是每一個架構和如何選擇兩者之間的一些概念的概觀。 如果您偏好的介紹影片，請參閱[讓網站與 ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET)和[Web 工具是什麼？](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | 如果您曾經 | 開發樣式 | 專業知識 | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Form | Win Form、 WPF、.NET | 使用豐富的封裝的 HTML 標記的控制項程式庫的快速開發 | 中介層級中，進階 RAD |
| MVC       | Ruby 上滑軌中，.NET  | 對於 HTML 標記、 程式碼和標記分隔，且容易撰寫測試的完整控制權。 行動裝置版和單一頁面應用程式 (SPA) 的最佳選擇。 | 中介層級中進階 |
| Web Pages  | 傳統 ASP 中 PHP     | HTML 標記和您的程式碼一起在相同的檔案 | 新的中介層級 |

### <a name="web-forms"></a>Web Form

使用 ASP.NET Web Form，您可以建置動態網站，使用熟悉的拖放、 事件導向模型。 設計介面及數以百計的控制項和元件可讓您迅速建置存取資料的複雜且功能強大 UI 驅動網站。 

[深入了解 Web Form](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC 可讓您建置動態網站，可讓清楚的重要性分離，並讓您完全掌控標記順暢、 靈活的開發功能強大、 以模式為基礎的方法。 ASP.NET MVC 包括許多功能，可針對建立採用最新 web 標準的精密應用程式的快速、 tdd 的開發。 

[深入了解 MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

ASP.NET 網頁和 Razor 語法提供快速、 親近且輕量方式結合伺服器程式碼與 HTML，以建立動態網頁內容。 連接到資料庫、 新增影片、 連結至社交網路網站，並包含許多更多的功能可協助您建立符合最新 web 標準的美觀網站。

[深入了解 Web 網頁](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Web Form、 MVC 和網頁的相關注意事項

所有的三個 ASP.NET 架構以.NET Framework 為基礎，並共用 ASP.NET 和.NET 的核心功能。 比方說，所有三個架構提供的登入安全性模型，根據成員資格，並全部三種共用同樣的功能來管理要求、 處理工作階段，以及其他核心 ASP.NET 功能的一部分。

此外，三個架構不是完全獨立，並選擇其中一個不會防止使用另一個。 架構可以共存於相同的 web 應用程式，因為並不常見的使用不同的架構所撰寫的應用程式的個別元件。 比方說，在 MVC 中的資料存取和管理的部分被開發中利用資料控制項和簡單的資料存取的 Web Form 最佳化標記，可能會開發客戶對向應用程式部分。

## <a name="web-apis"></a>Web API

ASP.NET Web API 是一種架構，可讓您更輕鬆地建立連線的用戶端，包括瀏覽器和行動裝置的較大範圍的 HTTP 服務。 ASP.NET Web 應用程式開發介面是在 .NET Framework 上建置 RESTful 應用程式的理想平台。

[深入了解 Web 應用程式開發介面](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>即時技術

ASP.NET SignalR 是 ASP.NET 開發人員可以更輕鬆開發即時 web 功能的新程式庫。 SignalR 可讓伺服器和用戶端之間的雙向通訊。 伺服器可以將內容推至連線的用戶端立即成為可用。 SignalR 支援 Web 通訊端，並會回復為舊版瀏覽器其他相容的技術。 SignalR 連線的管理包含應用程式開發介面 （例如，連接和中斷連線事件），分組連線及授權。

[深入了解 SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>行動裝置應用程式和站台 

ASP.NET 可以電源與 Web API 後端，以及使用像 Twitter 啟動程序的回應式設計架構的行動 web 站台的原生行動應用程式。 如果您要建置原生行動應用程式，所以可以輕鬆地建立 JSON 型 Web API 來控制代碼的資料存取、 驗證和您的應用程式的推播通知。 如果您要建立回應式行動裝置的站台，您可以使用任何的 CSS framework 或方格中開啟系統，您想要的話，或選取功能強大的行動系統，例如 jQuery Mobile 或 Sencha 及 PhoneGap 好行動應用程式。

[深入了解行動應用程式和網站應用程式開發](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>單一頁面應用程式 

ASP.NET 單一頁面應用程式 (SPA) 可協助您建置包含大量使用 HTML 5、 CSS 3 和 JavaScript 的用戶端互動的應用程式。 Visual Studio 包含用於建置使用解 knockout.js 和 ASP.NET Web API 的單一頁面應用程式的範本。 除了內建的 SPA 範本，社群建立 SPA 範本也會提供下載。

[深入了解單一頁面應用程式開發](single-page-application/index.md)

## <a name="webhooks"></a>WebHook

Webhook 為輕量型的 HTTP 模式，提供簡單的 pub/sub 模型一起接線 Web Api 和 SaaS 服務。 當事件發生在服務中時，會傳送通知形式的 HTTP POST 要求到已註冊的訂閱者。 POST 要求包含可讓收件者要據此採取行動的事件的相關資訊。

Webhook 大量包括 Dropbox、 GitHub、 Instagram、 MailChimp、 PayPal、 Slack、 Trello，以及更多的服務所公開。 比方說，WebHook 表示檔案已在 Dropbox，變更或在 GitHub 中的程式碼變更已認可或付款已起始中 PayPal，或是建立 Trello 卡片。

[深入了解 Webhook](webhooks/index.md)





<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
