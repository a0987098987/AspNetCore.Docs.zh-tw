---
uid: overview
title: "ASP.NET 概觀 |Microsoft 文件"
author: rick-anderson
description: "ASP.NET，可用的架構，用於建立網站、 web 應用程式和 web Api 的簡介。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2010
ms.topic: article
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: 
msc.type: content
ms.openlocfilehash: 3d4c34a35e2e34ed78f481c759eda3718edb4da6
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-overview"></a><span data-ttu-id="d3971-103">ASP.NET 概觀</span><span class="sxs-lookup"><span data-stu-id="d3971-103">ASP.NET overview</span></span>

<span data-ttu-id="d3971-104">ASP.NET 是免費的 web 架構建置絕佳的網站和使用 HTML、 CSS 和 JavaScript 的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3971-104">ASP.NET is a free web framework for building great websites and web applications using HTML, CSS, and JavaScript.</span></span> <span data-ttu-id="d3971-105">您也可以建立 Web 應用程式開發介面，並使用即時技術，例如 Web 通訊端。</span><span class="sxs-lookup"><span data-stu-id="d3971-105">You can also create Web APIs and use real-time technologies like Web Sockets.</span></span>

<span data-ttu-id="d3971-106">[ASP.NET Core](https://docs.microsoft.com/aspnet/core/)是 ASP.NET 的替代方式。</span><span class="sxs-lookup"><span data-stu-id="d3971-106">[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) is an alternative to ASP.NET.</span></span>  <span data-ttu-id="d3971-107">請參閱[ASP.NET 與 ASP.NET Core 之間選擇的指引](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework)。</span><span class="sxs-lookup"><span data-stu-id="d3971-107">See the [guidance on how to choose between ASP.NET and ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).</span></span>

## <a name="get-started"></a><span data-ttu-id="d3971-108">開始使用</span><span class="sxs-lookup"><span data-stu-id="d3971-108">Get started</span></span>

<span data-ttu-id="d3971-109">[下載 Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=826064)、 適用於在 Windows 上的 ASP.NET 釋放 IDE。</span><span class="sxs-lookup"><span data-stu-id="d3971-109">[Download Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=826064), a free IDE for ASP.NET on Windows.</span></span>

## <a name="websites-and-web-applications"></a><span data-ttu-id="d3971-110">網站和 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="d3971-110">Websites and web applications</span></span>

 <span data-ttu-id="d3971-111">ASP.NET 提供三個架構建立 web 應用程式： Web Form、 ASP.NET MVC 和 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="d3971-111">ASP.NET offers three frameworks for creating web applications: Web Forms, ASP.NET MVC, and ASP.NET Web Pages.</span></span> <span data-ttu-id="d3971-112">所有的三個架構已穩定並且成熟，，您可以使用任何語言建立絕佳的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3971-112">All three frameworks are stable and mature, and you can create great web applications with any of them.</span></span> <span data-ttu-id="d3971-113">無論您選擇何種架構，您會收到所有的優點和 everywhere ASP.NET 的功能。</span><span class="sxs-lookup"><span data-stu-id="d3971-113">No matter what framework you choose, you will get all the benefits and features of ASP.NET everywhere.</span></span>

<span data-ttu-id="d3971-114">每個架構為目標的不同的開發樣式。</span><span class="sxs-lookup"><span data-stu-id="d3971-114">Each framework targets a different development style.</span></span> <span data-ttu-id="d3971-115">您選擇一個取決於程式設計資產 （知識、 技術和開發體驗） 的組合，您要建立，應用程式和您已經熟悉的開發方式的類型。</span><span class="sxs-lookup"><span data-stu-id="d3971-115">The one you choose depends on a combination of your programming assets (knowledge, skills, and development experience), the type of application you're creating, and the development approach you're comfortable with.</span></span>

<span data-ttu-id="d3971-116">以下是每一個架構和如何選擇兩者之間的一些概念的概觀。</span><span class="sxs-lookup"><span data-stu-id="d3971-116">Below is an overview of each of the frameworks and some ideas for how to choose between them.</span></span> <span data-ttu-id="d3971-117">如果您偏好的介紹影片，請參閱[讓網站與 ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET)和[Web 工具是什麼？](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)</span><span class="sxs-lookup"><span data-stu-id="d3971-117">If you prefer a video introduction, see [Making Websites with ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) and [What is Web Tools?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)</span></span>

|   | <span data-ttu-id="d3971-118">如果您曾經</span><span class="sxs-lookup"><span data-stu-id="d3971-118">If you have experience in</span></span> | <span data-ttu-id="d3971-119">開發樣式</span><span class="sxs-lookup"><span data-stu-id="d3971-119">Development style</span></span> | <span data-ttu-id="d3971-120">專業知識</span><span class="sxs-lookup"><span data-stu-id="d3971-120">Expertise</span></span> | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| <span data-ttu-id="d3971-121">Web Form</span><span class="sxs-lookup"><span data-stu-id="d3971-121">Web Forms</span></span> | <span data-ttu-id="d3971-122">Win Form、 WPF、.NET</span><span class="sxs-lookup"><span data-stu-id="d3971-122">Win Forms, WPF, .NET</span></span> | <span data-ttu-id="d3971-123">使用豐富的封裝的 HTML 標記的控制項程式庫的快速開發</span><span class="sxs-lookup"><span data-stu-id="d3971-123">Rapid development using a rich library of controls that encapsulate HTML markup</span></span> | <span data-ttu-id="d3971-124">中介層級中，進階 RAD</span><span class="sxs-lookup"><span data-stu-id="d3971-124">Mid-Level, Advanced RAD</span></span> |
| <span data-ttu-id="d3971-125">MVC</span><span class="sxs-lookup"><span data-stu-id="d3971-125">MVC</span></span>       | <span data-ttu-id="d3971-126">Ruby 上滑軌中，.NET</span><span class="sxs-lookup"><span data-stu-id="d3971-126">Ruby on Rails, .NET</span></span>  | <span data-ttu-id="d3971-127">對於 HTML 標記、 程式碼和標記分隔，且容易撰寫測試的完整控制權。</span><span class="sxs-lookup"><span data-stu-id="d3971-127">Full control over HTML markup, code and markup separated, and easy to write tests.</span></span> <span data-ttu-id="d3971-128">行動裝置版和單一頁面應用程式 (SPA) 的最佳選擇。</span><span class="sxs-lookup"><span data-stu-id="d3971-128">The best choice for mobile and single-page applications (SPA).</span></span> | <span data-ttu-id="d3971-129">中介層級中進階</span><span class="sxs-lookup"><span data-stu-id="d3971-129">Mid-Level, Advanced</span></span> |
| <span data-ttu-id="d3971-130">Web Pages</span><span class="sxs-lookup"><span data-stu-id="d3971-130">Web Pages</span></span>  | <span data-ttu-id="d3971-131">傳統 ASP 中 PHP</span><span class="sxs-lookup"><span data-stu-id="d3971-131">Classic ASP, PHP</span></span>     | <span data-ttu-id="d3971-132">HTML 標記和您的程式碼一起在相同的檔案</span><span class="sxs-lookup"><span data-stu-id="d3971-132">HTML markup and your code together in the same file</span></span> | <span data-ttu-id="d3971-133">新的中介層級</span><span class="sxs-lookup"><span data-stu-id="d3971-133">New, Mid-Level</span></span> |

### <a name="web-forms"></a><span data-ttu-id="d3971-134">Web Form</span><span class="sxs-lookup"><span data-stu-id="d3971-134">Web Forms</span></span>

<span data-ttu-id="d3971-135">使用 ASP.NET Web Form，您可以建置動態網站，使用熟悉的拖放、 事件導向模型。</span><span class="sxs-lookup"><span data-stu-id="d3971-135">With ASP.NET Web Forms, you can build dynamic websites using a familiar drag-and-drop, event-driven model.</span></span> <span data-ttu-id="d3971-136">設計介面及數以百計的控制項和元件可讓您迅速建置存取資料的複雜且功能強大 UI 驅動網站。</span><span class="sxs-lookup"><span data-stu-id="d3971-136">A design surface and hundreds of controls and components let you rapidly build sophisticated, powerful UI-driven sites with data access.</span></span> 

[<span data-ttu-id="d3971-137">深入了解 Web Form</span><span class="sxs-lookup"><span data-stu-id="d3971-137">Learn more about Web Forms</span></span>](web-forms/index.md)

### <a name="mvc"></a><span data-ttu-id="d3971-138">MVC</span><span class="sxs-lookup"><span data-stu-id="d3971-138">MVC</span></span>

<span data-ttu-id="d3971-139">ASP.NET MVC 可讓您建置動態網站，可讓清楚的重要性分離，並讓您完全掌控標記順暢、 靈活的開發功能強大、 以模式為基礎的方法。</span><span class="sxs-lookup"><span data-stu-id="d3971-139">ASP.NET MVC gives you a powerful, patterns-based way to build dynamic websites that enables a clean separation of concerns and that gives you full control over markup for enjoyable, agile development.</span></span> <span data-ttu-id="d3971-140">ASP.NET MVC 包括許多功能，可針對建立採用最新 web 標準的精密應用程式的快速、 tdd 的開發。</span><span class="sxs-lookup"><span data-stu-id="d3971-140">ASP.NET MVC includes many features that enable fast, TDD-friendly development for creating sophisticated applications that use the latest web standards.</span></span> 

[<span data-ttu-id="d3971-141">深入了解 MVC</span><span class="sxs-lookup"><span data-stu-id="d3971-141">Learn more about MVC</span></span>](mvc/index.md)

### <a name="aspnet-web-pages"></a><span data-ttu-id="d3971-142">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="d3971-142">ASP.NET Web Pages</span></span>

<span data-ttu-id="d3971-143">ASP.NET 網頁和 Razor 語法提供快速、 親近且輕量方式結合伺服器程式碼與 HTML，以建立動態網頁內容。</span><span class="sxs-lookup"><span data-stu-id="d3971-143">ASP.NET Web Pages and the Razor syntax provide a fast, approachable, and lightweight way to combine server code with HTML to create dynamic web content.</span></span> <span data-ttu-id="d3971-144">連接到資料庫、 新增影片、 連結至社交網路網站，並包含許多更多的功能可協助您建立符合最新 web 標準的美觀網站。</span><span class="sxs-lookup"><span data-stu-id="d3971-144">Connect to databases, add video, link to social networking sites, and include many more features that help you create beautiful sites that conform to the latest web standards.</span></span>

[<span data-ttu-id="d3971-145">深入了解 Web 網頁</span><span class="sxs-lookup"><span data-stu-id="d3971-145">Learn more about Web Pages</span></span>](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a><span data-ttu-id="d3971-146">Web Form、 MVC 和網頁的相關注意事項</span><span class="sxs-lookup"><span data-stu-id="d3971-146">Notes about Web Forms, MVC, and Web Pages</span></span>

<span data-ttu-id="d3971-147">所有的三個 ASP.NET 架構以.NET Framework 為基礎，並共用 ASP.NET 和.NET 的核心功能。</span><span class="sxs-lookup"><span data-stu-id="d3971-147">All three ASP.NET frameworks are based on the .NET Framework and share core functionality of .NET and of ASP.NET.</span></span> <span data-ttu-id="d3971-148">比方說，所有三個架構提供的登入安全性模型，根據成員資格，並全部三種共用同樣的功能來管理要求、 處理工作階段，以及其他核心 ASP.NET 功能的一部分。</span><span class="sxs-lookup"><span data-stu-id="d3971-148">For example, all three frameworks offer a login security model based around membership, and all three share the same facilities for managing requests, handling sessions, and so on that are part of the core ASP.NET functionality.</span></span>

<span data-ttu-id="d3971-149">此外，三個架構不是完全獨立，並選擇其中一個不會防止使用另一個。</span><span class="sxs-lookup"><span data-stu-id="d3971-149">In addition, the three frameworks are not entirely independent, and choosing one does not preclude using another.</span></span> <span data-ttu-id="d3971-150">架構可以共存於相同的 web 應用程式，因為並不常見的使用不同的架構所撰寫的應用程式的個別元件。</span><span class="sxs-lookup"><span data-stu-id="d3971-150">Since the frameworks can coexist in the same web application, it's not uncommon to see individual components of applications written using different frameworks.</span></span> <span data-ttu-id="d3971-151">比方說，在 MVC 中的資料存取和管理的部分被開發中利用資料控制項和簡單的資料存取的 Web Form 最佳化標記，可能會開發客戶對向應用程式部分。</span><span class="sxs-lookup"><span data-stu-id="d3971-151">For example, customer-facing portions of an app might be developed in MVC to optimize the markup, while the data access and administrative portions are developed in Web Forms to take advantage of data controls and simple data access.</span></span>

## <a name="web-apis"></a><span data-ttu-id="d3971-152">Web API</span><span class="sxs-lookup"><span data-stu-id="d3971-152">Web APIs</span></span>

<span data-ttu-id="d3971-153">ASP.NET Web API 是一種架構，可讓您更輕鬆地建立連線的用戶端，包括瀏覽器和行動裝置的較大範圍的 HTTP 服務。</span><span class="sxs-lookup"><span data-stu-id="d3971-153">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="d3971-154">ASP.NET Web 應用程式開發介面是在 .NET Framework 上建置 RESTful 應用程式的理想平台。</span><span class="sxs-lookup"><span data-stu-id="d3971-154">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

[<span data-ttu-id="d3971-155">深入了解 Web 應用程式開發介面</span><span class="sxs-lookup"><span data-stu-id="d3971-155">Learn more about Web API</span></span>](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a><span data-ttu-id="d3971-156">即時技術</span><span class="sxs-lookup"><span data-stu-id="d3971-156">Real-time technologies</span></span>

<span data-ttu-id="d3971-157">ASP.NET SignalR 是 ASP.NET 開發人員可以更輕鬆開發即時 web 功能的新程式庫。</span><span class="sxs-lookup"><span data-stu-id="d3971-157">ASP.NET SignalR is a new library for ASP.NET developers that makes developing real-time web functionality easier.</span></span> <span data-ttu-id="d3971-158">SignalR 可讓伺服器和用戶端之間的雙向通訊。</span><span class="sxs-lookup"><span data-stu-id="d3971-158">SignalR allows bi-directional communication between server and client.</span></span> <span data-ttu-id="d3971-159">伺服器可以將內容推至連線的用戶端立即成為可用。</span><span class="sxs-lookup"><span data-stu-id="d3971-159">Servers can push content to connected clients instantly as it becomes available.</span></span> <span data-ttu-id="d3971-160">SignalR 支援 Web 通訊端，並會回復為舊版瀏覽器其他相容的技術。</span><span class="sxs-lookup"><span data-stu-id="d3971-160">SignalR supports Web Sockets, and falls back to other compatible techniques for older browsers.</span></span> <span data-ttu-id="d3971-161">SignalR 連線的管理包含應用程式開發介面 （例如，連接和中斷連線事件），分組連線及授權。</span><span class="sxs-lookup"><span data-stu-id="d3971-161">SignalR includes APIs for connection management (for instance, connect and disconnect events), grouping connections, and authorization.</span></span>

[<span data-ttu-id="d3971-162">深入了解 SignalR</span><span class="sxs-lookup"><span data-stu-id="d3971-162">Learn more about SignalR</span></span>](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a><span data-ttu-id="d3971-163">行動裝置應用程式和站台</span><span class="sxs-lookup"><span data-stu-id="d3971-163">Mobile apps and sites</span></span> 

<span data-ttu-id="d3971-164">ASP.NET 可以電源與 Web API 後端，以及使用像 Twitter 啟動程序的回應式設計架構的行動 web 站台的原生行動應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3971-164">ASP.NET can power native mobile apps with a Web API back end, as well as mobile web sites using responsive design frameworks like Twitter Bootstrap.</span></span> <span data-ttu-id="d3971-165">如果您要建置原生行動應用程式，所以可以輕鬆地建立 JSON 型 Web API 來控制代碼的資料存取、 驗證和您的應用程式的推播通知。</span><span class="sxs-lookup"><span data-stu-id="d3971-165">If you are building a native mobile app, it's easy to create a JSON-based Web API to handle data access, authentication, and push notifications for your app.</span></span> <span data-ttu-id="d3971-166">如果您要建立回應式行動裝置的站台，您可以使用任何的 CSS framework 或方格中開啟系統，您想要的話，或選取功能強大的行動系統，例如 jQuery Mobile 或 Sencha 及 PhoneGap 好行動應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3971-166">If you are building a responsive mobile site, you can use any CSS framework or open grid system you prefer, or select a powerful mobile system like jQuery Mobile or Sencha and great mobile applications with PhoneGap.</span></span>

[<span data-ttu-id="d3971-167">深入了解行動應用程式和網站應用程式開發</span><span class="sxs-lookup"><span data-stu-id="d3971-167">Learn more about mobile app and site development</span></span>](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a><span data-ttu-id="d3971-168">單一頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="d3971-168">Single-page applications</span></span> 

<span data-ttu-id="d3971-169">ASP.NET 單一頁面應用程式 (SPA) 可協助您建置包含大量使用 HTML 5、 CSS 3 和 JavaScript 的用戶端互動的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3971-169">ASP.NET Single Page Application (SPA) helps you build applications that include significant client-side interactions using HTML 5, CSS 3 and JavaScript.</span></span> <span data-ttu-id="d3971-170">Visual Studio 包含用於建置使用解 knockout.js 和 ASP.NET Web API 的單一頁面應用程式的範本。</span><span class="sxs-lookup"><span data-stu-id="d3971-170">Visual Studio includes a template for building single page applications using knockout.js and ASP.NET Web API.</span></span> <span data-ttu-id="d3971-171">除了內建的 SPA 範本，社群建立 SPA 範本也會提供下載。</span><span class="sxs-lookup"><span data-stu-id="d3971-171">In addition to the built-in SPA template, community-created SPA templates are also available for download.</span></span>

[<span data-ttu-id="d3971-172">深入了解單一頁面應用程式開發</span><span class="sxs-lookup"><span data-stu-id="d3971-172">Learn more about single-page app development</span></span>](single-page-application/index.md)

## <a name="webhooks"></a><span data-ttu-id="d3971-173">WebHook</span><span class="sxs-lookup"><span data-stu-id="d3971-173">WebHooks</span></span>

<span data-ttu-id="d3971-174">Webhook 為輕量型的 HTTP 模式，提供簡單的 pub/sub 模型一起接線 Web Api 和 SaaS 服務。</span><span class="sxs-lookup"><span data-stu-id="d3971-174">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="d3971-175">當事件發生在服務中時，會傳送通知形式的 HTTP POST 要求到已註冊的訂閱者。</span><span class="sxs-lookup"><span data-stu-id="d3971-175">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="d3971-176">POST 要求包含可讓收件者要據此採取行動的事件的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="d3971-176">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="d3971-177">Webhook 大量包括 Dropbox、 GitHub、 Instagram、 MailChimp、 PayPal、 Slack、 Trello，以及更多的服務所公開。</span><span class="sxs-lookup"><span data-stu-id="d3971-177">WebHooks are exposed by a large number of services including Dropbox, GitHub, Instagram, MailChimp, PayPal, Slack, Trello, and many more.</span></span> <span data-ttu-id="d3971-178">比方說，WebHook 表示檔案已在 Dropbox，變更或在 GitHub 中的程式碼變更已認可或付款已起始中 PayPal，或是建立 Trello 卡片。</span><span class="sxs-lookup"><span data-stu-id="d3971-178">For example, a WebHook can indicate that a file has changed in Dropbox, or a code change has been committed in GitHub, or a payment has been initiated in PayPal, or a card has been created in Trello.</span></span>

[<span data-ttu-id="d3971-179">深入了解 Webhook</span><span class="sxs-lookup"><span data-stu-id="d3971-179">Learn more about WebHooks</span></span>](webhooks/index.md)





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
