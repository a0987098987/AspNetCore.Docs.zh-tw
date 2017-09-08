---
title: "其他驗證提供者的簡短問卷調查。"
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 11/3/2016
ms.topic: article
ms.assetid: BC36CA84-3DE8-496E-9AA2-2F1B74AE8309
ms.prod: asp.net-core
uid: security/authentication/otherlogins
ms.openlocfilehash: 421db8c89e01cebba0c1f98cc9286288a75777f2
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="short-survey-of-other-authentication-providers"></a><span data-ttu-id="9e0cb-102">其他驗證提供者的簡短問卷調查</span><span class="sxs-lookup"><span data-stu-id="9e0cb-102">Short survey of other authentication providers</span></span>

<a name=security-authentication-other-logins></a>

<span data-ttu-id="9e0cb-103">由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Pranav Rastogi](https://github.com/rustd)，和[Valeriy Novytskyy](https://github.com/01binary)</span><span class="sxs-lookup"><span data-stu-id="9e0cb-103">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), and [Valeriy Novytskyy](https://github.com/01binary)</span></span>

<span data-ttu-id="9e0cb-104">以下是設定一些其他常見的 OAuth 提供者的指示。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-104">Here are set up instructions for some other common OAuth providers.</span></span> <span data-ttu-id="9e0cb-105">例如維護所適用的協力廠商 NuGet 封裝[aspnet contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth)可補充由 ASP.NET Core 小組實作的驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-105">Third-party NuGet packages such as the ones maintained by [aspnet-contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth) can be used to complement authentication providers implemented by the ASP.NET Core team.</span></span>

* <span data-ttu-id="9e0cb-106">設定**LinkedIn**登入： [https://developer.linkedin.com/my-apps](https://developer.linkedin.com/my-apps)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-106">Set up **LinkedIn** sign in: [https://developer.linkedin.com/my-apps](https://developer.linkedin.com/my-apps).</span></span> <span data-ttu-id="9e0cb-107">請參閱[官方步驟](https://developer.linkedin.com/docs/oauth2)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-107">See [official steps](https://developer.linkedin.com/docs/oauth2).</span></span>

* <span data-ttu-id="9e0cb-108">設定**Instagram**登入： [https://www.instagram.com/developer/clients/manage](https://www.instagram.com/developer/clients/manage)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-108">Set up **Instagram** sign in: [https://www.instagram.com/developer/clients/manage](https://www.instagram.com/developer/clients/manage).</span></span> <span data-ttu-id="9e0cb-109">請參閱[官方步驟](https://www.instagram.com/developer/authentication/)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-109">See [official steps](https://www.instagram.com/developer/authentication/).</span></span>

* <span data-ttu-id="9e0cb-110">設定**Reddit**登入： [https://www.reddit.com/prefs/apps](https://www.reddit.com/prefs/apps)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-110">Set up **Reddit** sign in: [https://www.reddit.com/prefs/apps](https://www.reddit.com/prefs/apps).</span></span> <span data-ttu-id="9e0cb-111">請參閱[官方步驟](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-111">See [official steps](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example).</span></span>

* <span data-ttu-id="9e0cb-112">設定**Github**登入： [https://github.com/settings/applications/new](https://github.com/settings/applications/new)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-112">Set up **Github** sign in: [https://github.com/settings/applications/new](https://github.com/settings/applications/new).</span></span> <span data-ttu-id="9e0cb-113">請參閱[官方步驟](https://developer.github.com/v3/oauth/)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-113">See [official steps](https://developer.github.com/v3/oauth/).</span></span>

* <span data-ttu-id="9e0cb-114">設定**Yahoo**登入： [https://developer.yahoo.com/apps/create/](https://developer.yahoo.com/apps/create/)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-114">Set up **Yahoo** sign in: [https://developer.yahoo.com/apps/create/](https://developer.yahoo.com/apps/create/).</span></span> <span data-ttu-id="9e0cb-115">請參閱[官方步驟](https://developer.yahoo.com/bbauth/user.html)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-115">See [official steps](https://developer.yahoo.com/bbauth/user.html).</span></span>

* <span data-ttu-id="9e0cb-116">設定**Tumblr**登入： [https://www.tumblr.com/oauth/apps](https://www.tumblr.com/oauth/apps)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-116">Set up **Tumblr** sign in: [https://www.tumblr.com/oauth/apps](https://www.tumblr.com/oauth/apps).</span></span> <span data-ttu-id="9e0cb-117">請參閱[官方步驟](https://www.tumblr.com/docs/en/api/v2#auth)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-117">See [official steps](https://www.tumblr.com/docs/en/api/v2#auth).</span></span>

* <span data-ttu-id="9e0cb-118">設定**Pinterest**登入： [https://developers.pinterest.com/apps](https://developers.pinterest.com/apps)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-118">Set up **Pinterest** sign in: [https://developers.pinterest.com/apps](https://developers.pinterest.com/apps).</span></span> <span data-ttu-id="9e0cb-119">請參閱[官方步驟](https://developers.pinterest.com/docs/api/overview/?)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-119">See [official steps](https://developers.pinterest.com/docs/api/overview/?).</span></span>

* <span data-ttu-id="9e0cb-120">設定**Pocket**登入： [http://getpocket.com/developer/apps/new](http://getpocket.com/developer/apps/new)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-120">Set up **Pocket** sign in: [http://getpocket.com/developer/apps/new](http://getpocket.com/developer/apps/new).</span></span> <span data-ttu-id="9e0cb-121">請參閱[官方步驟](https://getpocket.com/developer/docs/authentication)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-121">See [official steps](https://getpocket.com/developer/docs/authentication).</span></span>

* <span data-ttu-id="9e0cb-122">設定**Flickr**登入： [https://www.flickr.com/services/apps/create](https://www.flickr.com/services/apps/create)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-122">Set up **Flickr** sign in: [https://www.flickr.com/services/apps/create](https://www.flickr.com/services/apps/create).</span></span> <span data-ttu-id="9e0cb-123">請參閱[官方步驟](https://www.flickr.com/services/api/auth.oauth.html)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-123">See [official steps](https://www.flickr.com/services/api/auth.oauth.html).</span></span>

* <span data-ttu-id="9e0cb-124">設定**Dribble**登入： [https://dribbble.com/signup](https://dribbble.com/signup)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-124">Set up **Dribble** sign in: [https://dribbble.com/signup](https://dribbble.com/signup).</span></span> <span data-ttu-id="9e0cb-125">請參閱[官方步驟](http://developer.dribbble.com/v1/oauth/)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-125">See [official steps](http://developer.dribbble.com/v1/oauth/).</span></span>

* <span data-ttu-id="9e0cb-126">設定**Vimeo**登入： [https://developer.vimeo.com/apps](https://developer.vimeo.com/apps)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-126">Set up **Vimeo** sign in: [https://developer.vimeo.com/apps](https://developer.vimeo.com/apps).</span></span> <span data-ttu-id="9e0cb-127">請參閱[官方步驟](https://developer.vimeo.com/api/authentication)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-127">See [official steps](https://developer.vimeo.com/api/authentication).</span></span>

* <span data-ttu-id="9e0cb-128">設定**SoundCloud**登入： [http://soundcloud.com/you/apps/new](http://soundcloud.com/you/apps/new)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-128">Set up **SoundCloud** sign in: [http://soundcloud.com/you/apps/new](http://soundcloud.com/you/apps/new).</span></span> <span data-ttu-id="9e0cb-129">請參閱[官方步驟](https://developers.soundcloud.com/blog/we-love-oauth-2)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-129">See [official steps](https://developers.soundcloud.com/blog/we-love-oauth-2).</span></span>

* <span data-ttu-id="9e0cb-130">設定**VK**登入： [https://vk.com/apps?act=manage](https://vk.com/apps?act=manage)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-130">Set up **VK** sign in: [https://vk.com/apps?act=manage](https://vk.com/apps?act=manage).</span></span> <span data-ttu-id="9e0cb-131">請參閱[官方步驟](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites)。</span><span class="sxs-lookup"><span data-stu-id="9e0cb-131">See [official steps](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites).</span></span>
