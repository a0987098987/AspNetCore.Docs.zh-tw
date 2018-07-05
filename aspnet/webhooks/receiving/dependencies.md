---
uid: webhooks/receiving/dependencies
title: ASP.NET Webhook 接收器相依性 |Microsoft Docs
author: rick-anderson
description: 接收者的相依性和相依性插入在 ASP.NET Webhook。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.openlocfilehash: cf45e3d2e45e4b7882b42d9aa0931e18c08f76b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401178"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="5a27e-103">ASP.NET Webhook 接收器相依性</span><span class="sxs-lookup"><span data-stu-id="5a27e-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="5a27e-104">Microsoft ASP.NET Webhook 被設計在心的相依性插入。</span><span class="sxs-lookup"><span data-stu-id="5a27e-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="5a27e-105">在系統中的大部分相依性可以取代使用相依性插入引擎的替代實作。</span><span class="sxs-lookup"><span data-stu-id="5a27e-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="5a27e-106">請參閱[DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs)接收者相依性的清單。</span><span class="sxs-lookup"><span data-stu-id="5a27e-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="5a27e-107">如果尚未註冊任何相依性，則會使用預設實作。</span><span class="sxs-lookup"><span data-stu-id="5a27e-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="5a27e-108">請參閱[ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs)取得一份預設實作。</span><span class="sxs-lookup"><span data-stu-id="5a27e-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
