---
uid: webhooks/receiving/dependencies
title: "ASP.NET Webhook 接收者相依性 |Microsoft 文件"
author: rick-anderson
description: "在 ASP.NET Webhook 相依性插入和接收者相依性。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="fa721-103">ASP.NET Webhook 接收者相依性</span><span class="sxs-lookup"><span data-stu-id="fa721-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="fa721-104">Microsoft ASP.NET Webhook 被設計在心的相依性插入。</span><span class="sxs-lookup"><span data-stu-id="fa721-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="fa721-105">在系統中的大部分相依性可以取代使用相依性的資料隱碼引擎的替代實作。</span><span class="sxs-lookup"><span data-stu-id="fa721-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="fa721-106">請參閱[DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs)接收者相依性的清單。</span><span class="sxs-lookup"><span data-stu-id="fa721-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="fa721-107">如果尚未註冊任何相依性，則會使用預設實作。</span><span class="sxs-lookup"><span data-stu-id="fa721-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="fa721-108">請參閱[ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs)實作預設的清單。</span><span class="sxs-lookup"><span data-stu-id="fa721-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
