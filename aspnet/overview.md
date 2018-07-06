---
uid: overview
title: ASP.NET 概觀 |Microsoft Docs
author: rick-anderson
description: ASP.NET 中，免費的架構，用於建立網站、 web 應用程式和 web Api 的簡介。
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.author: aspnetcontent
ms.date: 03/12/2010
ms.technology: aspnet
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: d477db5f38fff6549f0a7f62963a48ee89b74101
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809481"
---
# <a name="aspnet-overview"></a>ASP.NET 概觀

ASP.NET 是免費的 web 架構來建置絕佳的網站和使用 HTML、 CSS 和 JavaScript 的 web 應用程式。 您也可以建立 Web Api，並使用 Web 通訊端等即時技術。

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/)是 ASP.NET 的替代方式。  請參閱[如何在 ASP.NET 和 ASP.NET Core 之間進行選擇指引](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework)。

## <a name="get-started"></a>開始使用

[Visual Studio Community 2017](https://www.visualstudio.com/downloads/)的免費 IDE，用以在 Windows 上的 ASP.NET。

## <a name="websites-and-web-applications"></a>網站和 web 應用程式

 ASP.NET 提供三種建立 web 應用程式的架構： Web Form、 ASP.NET MVC 和 ASP.NET Web Pages。 所有的三個架構穩定且成熟的而且您可以使用任何一個來建立絕佳的 web 應用程式。 無論您選擇何種架構，您會收到所有的優點和功能 ASP.NET 無所不在。

每個架構為目標的不同的開發樣式。 其中您選擇取決於您程式設計的資產 （知識、 技能和開發體驗） 的組合，您要建立的應用程式和您已經熟悉的開發方法的類型。

以下是每個架構和如何在它們之間做出選擇的一些概念的概觀。 如果您偏好影片介紹，請參閱[進行 ASP.NET 網站](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET)和[Web 工具是什麼？](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | 如果您在體驗 | 開發樣式 | 專業知識 | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Form | Win Form、 WPF、.NET | 快速開發使用豐富的程式庫的封裝的 HTML 標記的控制項 | 中級、 進階的 RAD |
| MVC       | Ruby on Rails，.NET  | 完整控制 HTML 標記、 程式碼和標記分隔，且容易撰寫測試的詳細資訊。 行動和單一頁面應用程式 (SPA) 是最佳選擇。 | 中級、 進階 |
| Web Pages  | 傳統 ASP、 PHP     | HTML 標記和您的程式碼一起在相同的檔案 | 新的、 中間層級 |

### <a name="web-forms"></a>Web Form

您可以使用 ASP.NET Web Form，建置動態網站，使用熟悉的拖放、 事件驅動模型。 在設計介面及數以百計的控制項和元件，可讓您快速建置複雜且功能強大 UI 導向網站的資料存取。 

[深入了解 Web Form](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC 可讓您建置動態網站，可讓清楚分離關注點，並讓您完全掌控標記，進而順暢、 敏捷式軟體開發開發功能強大、 以模式為基礎的方法。 ASP.NET MVC 包含許多功能可讓您建立使用最新的 web 標準的精密應用程式的快速、 tdd 的開發。 

[深入了解 MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

ASP.NET Web Pages 和 Razor 語法提供快速、 容易使用且輕量方式結合伺服器程式碼與 HTML，以建立動態 web 內容。 連接到資料庫、 新增影片、 連結至社交網路網站，和包含許多更多的功能，可協助您建立美觀的網站符合最新的 web 標準。

[深入了解網頁](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Web Form、 MVC 和 Web 網頁的相關注意事項

所有的三種 ASP.NET 架構以.NET Framework 為基礎，並共用.NET 和 ASP.NET 的核心功能。 比方說，所有的三個架構提供成員資格以基礎的登入安全性模型，並全部三種共用屬於 ASP.NET Core功能相同的設施管理要求、 處理工作階段，等等。

此外，這三種架構不是完全獨立，並選擇其中一個不會防止使用另一個。 由於架構可以共存於相同的 web 應用程式，並不常見的使用不同的架構所撰寫的應用程式的個別元件。 比方說，MVC 最佳化標記，而若要利用資料控制項和簡單資料存取的 Web Form 開發的資料存取和管理的部分可能會進行開發的應用程式的客戶使用的部分。

## <a name="web-apis"></a>Web API

ASP.NET Web API 是一種架構，可讓您更輕鬆建置 HTTP 服務並擴及各種用戶端，包括瀏覽器和行動裝置。 ASP.NET Web 應用程式開發介面是在 .NET Framework 上建置 RESTful 應用程式的理想平台。

[深入了解 Web API](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>即時技術

ASP.NET SignalR 是 ASP.NET 開發人員更輕鬆開發即時 web 功能的新程式庫。 SignalR 可讓伺服器和用戶端之間的雙向通訊。 伺服器可以將內容推至連線的用戶端立即可供使用。 SignalR 支援 Web 通訊端，並會回復為舊版瀏覽器其他相容的技術。 SignalR 連線管理包含 Api （例如，連接和中斷連接事件），分組連線及授權。

[深入了解 SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>行動裝置應用程式和網站 

ASP.NET 能夠與 Web API 後端，以及使用回應式設計架構，像是 Twitter Bootstrap 的行動 web 站台的原生行動應用程式。 如果您要建置原生的行動裝置應用程式，很容易建立 JSON 型 Web API 來控制代碼的資料存取、 驗證及您的應用程式的推播通知。 如果您要建置回應靈敏的行動裝置網站，您可以使用任何的 CSS 架構或您想要的話，或選取功能強大的行動系統，例如 jQuery Mobile 或 Sencha 和 PhoneGap 絕佳行動應用程式的開啟方格系統。

[深入了解行動裝置應用程式和站台的開發](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>單一頁面應用程式 

ASP.NET 單一頁面應用程式 (SPA) 可協助您建置包含大量使用 HTML 5、 CSS 3 和 JavaScript 的用戶端互動的應用程式。 Visual Studio 包含建置使用 knockout.js 和 ASP.NET Web API 的單一頁面應用程式的範本。 除了內建的 SPA 範本中，社群建立 SPA 範本是可供下載。

[深入了解單一頁面應用程式開發](single-page-application/index.md)

## <a name="webhooks"></a>WebHook

Webhook 是一起接線 Web Api 和 SaaS 服務提供簡單 pub/sub 模型的輕量型 HTTP 模式。 當事件發生在服務中時，會傳送通知的 HTTP POST 要求表單中已註冊的訂閱者。 POST 要求包含可讓收件者要據此採取行動之事件的相關資訊。

Webhook 會公開大量包括 Dropbox、 GitHub、 Instagram、 MailChimp、 PayPal、 Slack、 Trello 和更多的服務。 比方說，WebHook 可以表示檔案已在 Dropbox 中，變更或已在 GitHub 中，認可程式碼變更或付款已起始 PayPal、 中或卡片在 Trello 中建立。

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
