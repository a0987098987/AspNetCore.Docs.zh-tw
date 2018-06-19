---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[如何:]選擇方法的 AJAX 頁面上的更新嗎？ | Microsoft Docs'
author: JoeStagner
description: 在這段影片 Joe stagner 以比較執行 ASP.NET 應用程式中的 AJAX 型頁面更新兩種主要方法。 第一個方法是使用 Upd...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2007
ms.topic: article
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: b86471b93b7e3a1ed371288195f09fa28353ab36
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885523"
---
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a><span data-ttu-id="1c7b2-105">[如何:]選擇方法的 AJAX 頁面上的更新嗎？</span><span class="sxs-lookup"><span data-stu-id="1c7b2-105">[How Do I:] Choose Between Methods of AJAX Page Updates?</span></span>
====================
<span data-ttu-id="1c7b2-106">由[Joe stagner 以](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="1c7b2-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="1c7b2-107">在這段影片 Joe stagner 以比較執行 ASP.NET 應用程式中的 AJAX 型頁面更新兩種主要方法。</span><span class="sxs-lookup"><span data-stu-id="1c7b2-107">In this video Joe Stagner compares the two primary methods of performing AJAX-style page updates in an ASP.NET application.</span></span> <span data-ttu-id="1c7b2-108">第一個方法是使用的 UpdatePanel，其中沒有額外的程式碼以供需要寫用戶端或伺服器端上。</span><span class="sxs-lookup"><span data-stu-id="1c7b2-108">The first method is to use an UpdatePanel, where no additional code needs to be written on either the client side or the server side.</span></span> <span data-ttu-id="1c7b2-109">使用 UpdatePanel 的好處是所有項目會自動運作。</span><span class="sxs-lookup"><span data-stu-id="1c7b2-109">The benefit of using the UpdatePanel is that everything works automatically.</span></span> <span data-ttu-id="1c7b2-110">損失是，在需要大量資料要包含在 AJAX 要求和回應，用戶端和伺服器需要執行完整的頁面生命週期。</span><span class="sxs-lookup"><span data-stu-id="1c7b2-110">The penalty is that at the client it requires a lot of data to be included in the AJAX request and response, and at the server it requires a full page lifecycle to be executed.</span></span> <span data-ttu-id="1c7b2-111">第二種方法是使用網路回呼 (callback)，其中額外的程式碼以供需要寫用戶端和伺服器端上。</span><span class="sxs-lookup"><span data-stu-id="1c7b2-111">The second method is to use network callbacks, where additional code needs to be written on both the client side and the server side.</span></span> <span data-ttu-id="1c7b2-112">在用戶端需要併入 AJAX 要求和回應，很少的資料，而需要在伺服器只能被呼叫的服務執行的方法是使用網路回呼的好處。</span><span class="sxs-lookup"><span data-stu-id="1c7b2-112">The benefit of using network callbacks is that at the client it requires very little data to be included in the AJAX request and response, and at the server it requires only the called service method to be executed.</span></span> <span data-ttu-id="1c7b2-113">Penality 是時間和精力寫入必要的程式碼。</span><span class="sxs-lookup"><span data-stu-id="1c7b2-113">The penality is the time and effort it takes to write the necessary code.</span></span> <span data-ttu-id="1c7b2-114">Joe 結束時，視訊會討論 AJAX 型網頁更新兩種主要方法之間選擇時應該考慮的事項。</span><span class="sxs-lookup"><span data-stu-id="1c7b2-114">Joe concludes the video by discussing what you should consider when choosing between the two primary methods of AJAX-style page updates.</span></span> <span data-ttu-id="1c7b2-115">(這段影片中使用的程式碼[如何開始使用 ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md)視訊和[如何進行用戶端網路回呼與 ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md)視訊。)</span><span class="sxs-lookup"><span data-stu-id="1c7b2-115">(This video uses the code from the [How Do I Get Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video and the [How Do I Make Client-Side Network Callbacks with ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span></span>

[<span data-ttu-id="1c7b2-116">&#9654;觀看影片 （11 分鐘）</span><span class="sxs-lookup"><span data-stu-id="1c7b2-116">&#9654; Watch video (11 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> <span data-ttu-id="1c7b2-117">[上一頁](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [下一頁](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span><span class="sxs-lookup"><span data-stu-id="1c7b2-117">[Previous](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Next](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span></span>
