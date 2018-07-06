---
uid: webhooks/receiving/dependencies
title: ASP.NET Webhook 接收器相依性 |Microsoft Docs
author: rick-anderson
description: 接收者的相依性和相依性插入在 ASP.NET Webhook。
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 05dcfac121e7974fd83c5b3736616479574944a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817833"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="51103-103">ASP.NET Webhook 接收器相依性</span><span class="sxs-lookup"><span data-stu-id="51103-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="51103-104">Microsoft ASP.NET Webhook 被設計在心的相依性插入。</span><span class="sxs-lookup"><span data-stu-id="51103-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="51103-105">在系統中的大部分相依性可以取代使用相依性插入引擎的替代實作。</span><span class="sxs-lookup"><span data-stu-id="51103-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="51103-106">請參閱[DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs)接收者相依性的清單。</span><span class="sxs-lookup"><span data-stu-id="51103-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="51103-107">如果尚未註冊任何相依性，則會使用預設實作。</span><span class="sxs-lookup"><span data-stu-id="51103-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="51103-108">請參閱[ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs)取得一份預設實作。</span><span class="sxs-lookup"><span data-stu-id="51103-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
