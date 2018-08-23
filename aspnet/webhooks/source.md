---
uid: webhooks/source
title: ASP.NET Webhook 原始程式碼和 NuGet 套件 |Microsoft Docs
author: rick-anderson
description: ASP.NET Webhook 原始程式碼和 NuGet 套件的連結
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: ff716b476f7dc69b6071d3febd5b5871e4f02689
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825560"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="28bae-103">ASP.NET Webhook 原始程式碼和 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="28bae-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="28bae-104">Microsoft ASP.NET Webhook 是模組的 Microsoft ASP.NET 系列的一部分，且裝載作為[GitHub 上的開放原始碼專案](https://github.com/aspnet/WebHooks)。</span><span class="sxs-lookup"><span data-stu-id="28bae-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="28bae-105">這表示我們接受參與，但請看看[貢獻指導方針](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md)之前提交提取要求。</span><span class="sxs-lookup"><span data-stu-id="28bae-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="28bae-106">您正在閱讀此線上文件現在也做[GitHub 上的開放原始碼](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide)並接受貢獻。</span><span class="sxs-lookup"><span data-stu-id="28bae-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="28bae-107">NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="28bae-107">NuGet packages</span></span>

<span data-ttu-id="28bae-108">[NuGet 套件](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks)分為三個部分：</span><span class="sxs-lookup"><span data-stu-id="28bae-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="28bae-109">[常見](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common)： 寄件者與接收者之間共用的常見套件。</span><span class="sxs-lookup"><span data-stu-id="28bae-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="28bae-110">[寄件者](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom)： 一組套件支援給其他人傳送您自己的 Webhook。</span><span class="sxs-lookup"><span data-stu-id="28bae-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="28bae-111">傳送 Webhook 的功能更詳細地說明[傳送 Webhook](sending/index.md)。</span><span class="sxs-lookup"><span data-stu-id="28bae-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="28bae-112">[接收者](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)： 一組支援 Webhook 接收其他人的套件。</span><span class="sxs-lookup"><span data-stu-id="28bae-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="28bae-113">接收 Webhook 的功能更詳細地說明[接收 Webhook](receiving/index.md)。</span><span class="sxs-lookup"><span data-stu-id="28bae-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
