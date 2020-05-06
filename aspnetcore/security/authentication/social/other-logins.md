---
title: 外部 OAuth 驗證提供者
author: rick-anderson
description: 探索可與 ASP.NET Core 應用程式搭配使用的外部 OAuth 驗證提供者。
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/otherlogins
ms.openlocfilehash: c61bbef26017f14df09f8ca6f494d29f79903b6b
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776457"
---
# <a name="external-oauth-authentication-providers"></a>外部 OAuth 驗證提供者

由[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Pranav 請參閱 rastogi](https://github.com/rustd)和[Valeriy Novytskyy](https://github.com/01binary)

下列清單包含與 ASP.NET Core 應用程式搭配使用的一般外部 OAuth 驗證提供者。 協力廠商 NuGet 套件（例如由[aspnet contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth)維護）可以用來補充 ASP.NET Core 小組所實行的驗證提供者。

* [LinkedIn](https://www.linkedin.com/developer/apps) （[指示](https://developer.linkedin.com/docs/oauth2)）

* [Instagram](https://www.instagram.com/developer/register/) （[指示](https://www.instagram.com/developer/authentication/)）

* [Reddit](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps) （[指示](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example)）

* [Github](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew) （[指示](https://developer.github.com/v3/oauth/)）

* [Yahoo](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F) （[指示](https://developer.yahoo.com/bbauth/user.html)）

* [Tumblr](https://www.tumblr.com/oauth/apps) （[指示](https://www.tumblr.com/docs/api/v2#auth)）

* [Pinterest](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F) （[指示](https://developers.pinterest.com/docs/api/overview/?)）

* [Pocket](https://getpocket.com/developer/apps/new) （[指示](https://getpocket.com/developer/docs/authentication)）

* [Flickr](https://www.flickr.com/services/apps/create) （[指示](https://www.flickr.com/services/api/auth.oauth.html)）

* [Dribble](https://dribbble.com/signup) （[指示](https://developer.dribbble.com/v1/oauth/)）

* [Vimeo](https://vimeo.com/join) （[指示](https://developer.vimeo.com/api/authentication)）

* [SoundCloud](https://soundcloud.com/you/apps/new) （[指示](https://developers.soundcloud.com/blog/we-love-oauth-2)）

* [VK](https://vk.com/apps?act=manage) （[指示](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites)）

[!INCLUDE[Multiple authentication providers](includes/chain-auth-providers.md)]

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]
