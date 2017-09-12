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
ms.openlocfilehash: a7a7443129c23269dd10fd224d7f8c46011d5d11
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="short-survey-of-other-authentication-providers"></a><span data-ttu-id="94af5-102">其他驗證提供者的簡短問卷調查</span><span class="sxs-lookup"><span data-stu-id="94af5-102">Short survey of other authentication providers</span></span>

<a name=security-authentication-other-logins></a>

<span data-ttu-id="94af5-103">由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Pranav Rastogi](https://github.com/rustd)，和[Valeriy Novytskyy](https://github.com/01binary)</span><span class="sxs-lookup"><span data-stu-id="94af5-103">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), and [Valeriy Novytskyy](https://github.com/01binary)</span></span>

<span data-ttu-id="94af5-104">以下是設定一些其他常見的 OAuth 提供者的指示。</span><span class="sxs-lookup"><span data-stu-id="94af5-104">Here are set up instructions for some other common OAuth providers.</span></span> <span data-ttu-id="94af5-105">例如維護所適用的協力廠商 NuGet 封裝[aspnet contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth)可補充由 ASP.NET Core 小組實作的驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="94af5-105">Third-party NuGet packages such as the ones maintained by [aspnet-contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth) can be used to complement authentication providers implemented by the ASP.NET Core team.</span></span>

* <span data-ttu-id="94af5-106">設定**LinkedIn**登入： [https://www.linkedin.com/developer/apps](https://www.linkedin.com/developer/apps)。</span><span class="sxs-lookup"><span data-stu-id="94af5-106">Set up **LinkedIn** sign in: [https://www.linkedin.com/developer/apps](https://www.linkedin.com/developer/apps).</span></span> <span data-ttu-id="94af5-107">請參閱[官方步驟](https://developer.linkedin.com/docs/oauth2)。</span><span class="sxs-lookup"><span data-stu-id="94af5-107">See [official steps](https://developer.linkedin.com/docs/oauth2).</span></span>

* <span data-ttu-id="94af5-108">設定**Instagram**登入： [https://www.instagram.com/developer/register/](https://www.instagram.com/developer/register/)。</span><span class="sxs-lookup"><span data-stu-id="94af5-108">Set up **Instagram** sign in: [https://www.instagram.com/developer/register/](https://www.instagram.com/developer/register/).</span></span> <span data-ttu-id="94af5-109">請參閱[官方步驟](https://www.instagram.com/developer/authentication/)。</span><span class="sxs-lookup"><span data-stu-id="94af5-109">See [official steps](https://www.instagram.com/developer/authentication/).</span></span>

* <span data-ttu-id="94af5-110">設定**Reddit**登入： [https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps)。</span><span class="sxs-lookup"><span data-stu-id="94af5-110">Set up **Reddit** sign in: [https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps).</span></span> <span data-ttu-id="94af5-111">請參閱[官方步驟](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example)。</span><span class="sxs-lookup"><span data-stu-id="94af5-111">See [official steps](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example).</span></span>

* <span data-ttu-id="94af5-112">設定**Github**登入： [https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew)。</span><span class="sxs-lookup"><span data-stu-id="94af5-112">Set up **Github** sign in: [https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew).</span></span> <span data-ttu-id="94af5-113">請參閱[官方步驟](https://developer.github.com/v3/oauth/)。</span><span class="sxs-lookup"><span data-stu-id="94af5-113">See [official steps](https://developer.github.com/v3/oauth/).</span></span>

* <span data-ttu-id="94af5-114">設定**Yahoo**登入： [https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F)。</span><span class="sxs-lookup"><span data-stu-id="94af5-114">Set up **Yahoo** sign in: [https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F).</span></span> <span data-ttu-id="94af5-115">請參閱[官方步驟](https://developer.yahoo.com/bbauth/user.html)。</span><span class="sxs-lookup"><span data-stu-id="94af5-115">See [official steps](https://developer.yahoo.com/bbauth/user.html).</span></span>

* <span data-ttu-id="94af5-116">設定**Tumblr**登入： [https://www.tumblr.com/oauth/apps](https://www.tumblr.com/oauth/apps)。</span><span class="sxs-lookup"><span data-stu-id="94af5-116">Set up **Tumblr** sign in: [https://www.tumblr.com/oauth/apps](https://www.tumblr.com/oauth/apps).</span></span> <span data-ttu-id="94af5-117">請參閱[官方步驟](https://www.tumblr.com/docs/api/v2#auth)。</span><span class="sxs-lookup"><span data-stu-id="94af5-117">See [official steps](https://www.tumblr.com/docs/api/v2#auth).</span></span>

* <span data-ttu-id="94af5-118">設定**Pinterest**登入： [https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F)。</span><span class="sxs-lookup"><span data-stu-id="94af5-118">Set up **Pinterest** sign in: [https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F).</span></span> <span data-ttu-id="94af5-119">請參閱[官方步驟](https://developers.pinterest.com/docs/api/overview/?)。</span><span class="sxs-lookup"><span data-stu-id="94af5-119">See [official steps](https://developers.pinterest.com/docs/api/overview/?).</span></span>

* <span data-ttu-id="94af5-120">設定**Pocket**登入： [https://getpocket.com/developer/apps/new](https://getpocket.com/developer/apps/new)。</span><span class="sxs-lookup"><span data-stu-id="94af5-120">Set up **Pocket** sign in: [https://getpocket.com/developer/apps/new](https://getpocket.com/developer/apps/new).</span></span> <span data-ttu-id="94af5-121">請參閱[官方步驟](https://getpocket.com/developer/docs/authentication)。</span><span class="sxs-lookup"><span data-stu-id="94af5-121">See [official steps](https://getpocket.com/developer/docs/authentication).</span></span>

* <span data-ttu-id="94af5-122">設定**Flickr**登入： [https://www.flickr.com/services/apps/create](https://www.flickr.com/services/apps/create)。</span><span class="sxs-lookup"><span data-stu-id="94af5-122">Set up **Flickr** sign in: [https://www.flickr.com/services/apps/create](https://www.flickr.com/services/apps/create).</span></span> <span data-ttu-id="94af5-123">請參閱[官方步驟](https://www.flickr.com/services/api/auth.oauth.html)。</span><span class="sxs-lookup"><span data-stu-id="94af5-123">See [official steps](https://www.flickr.com/services/api/auth.oauth.html).</span></span>

* <span data-ttu-id="94af5-124">設定**Dribble**登入： [https://dribbble.com/signup](https://dribbble.com/signup)。</span><span class="sxs-lookup"><span data-stu-id="94af5-124">Set up **Dribble** sign in: [https://dribbble.com/signup](https://dribbble.com/signup).</span></span> <span data-ttu-id="94af5-125">請參閱[官方步驟](http://developer.dribbble.com/v1/oauth/)。</span><span class="sxs-lookup"><span data-stu-id="94af5-125">See [official steps](http://developer.dribbble.com/v1/oauth/).</span></span>

* <span data-ttu-id="94af5-126">設定**Vimeo**登入： [https://vimeo.com/join](https://vimeo.com/join)。</span><span class="sxs-lookup"><span data-stu-id="94af5-126">Set up **Vimeo** sign in: [https://vimeo.com/join](https://vimeo.com/join).</span></span> <span data-ttu-id="94af5-127">請參閱[官方步驟](https://developer.vimeo.com/api/authentication)。</span><span class="sxs-lookup"><span data-stu-id="94af5-127">See [official steps](https://developer.vimeo.com/api/authentication).</span></span>

* <span data-ttu-id="94af5-128">設定**SoundCloud**登入： [https://soundcloud.com/you/apps/new](https://soundcloud.com/you/apps/new)。</span><span class="sxs-lookup"><span data-stu-id="94af5-128">Set up **SoundCloud** sign in: [https://soundcloud.com/you/apps/new](https://soundcloud.com/you/apps/new).</span></span> <span data-ttu-id="94af5-129">請參閱[官方步驟](https://developers.soundcloud.com/blog/we-love-oauth-2)。</span><span class="sxs-lookup"><span data-stu-id="94af5-129">See [official steps](https://developers.soundcloud.com/blog/we-love-oauth-2).</span></span>

* <span data-ttu-id="94af5-130">設定**VK**登入： [https://vk.com/apps?act=manage](https://vk.com/apps?act=manage)。</span><span class="sxs-lookup"><span data-stu-id="94af5-130">Set up **VK** sign in: [https://vk.com/apps?act=manage](https://vk.com/apps?act=manage).</span></span> <span data-ttu-id="94af5-131">請參閱[官方步驟](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites)。</span><span class="sxs-lookup"><span data-stu-id="94af5-131">See [official steps](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites).</span></span>
