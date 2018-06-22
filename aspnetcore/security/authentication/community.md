---
title: 適用於 ASP.NET Core community OSS 驗證選項
author: rick-anderson
description: 探索 ASP.NET Core 開放原始碼驗證的選項。
ms.author: riande
ms.date: 03/12/2018
uid: security/authentication/community
ms.openlocfilehash: 8b3800631cb71f6bd5120157c89765f6d72628ee
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276564"
---
# <a name="community-oss-authentication-options-for-aspnet-core"></a><span data-ttu-id="70b3d-103">適用於 ASP.NET Core community OSS 驗證選項</span><span class="sxs-lookup"><span data-stu-id="70b3d-103">Community OSS authentication options for ASP.NET Core</span></span>

<span data-ttu-id="70b3d-104">此頁面包含社群提供，開放原始碼 ASP.NET Core 的驗證選項。</span><span class="sxs-lookup"><span data-stu-id="70b3d-104">This page contains community-provided, open source authentication options for ASP.NET Core.</span></span> <span data-ttu-id="70b3d-105">此頁面會定期更新為新的提供者，就可以使用。</span><span class="sxs-lookup"><span data-stu-id="70b3d-105">This page is periodically updated as new providers become available.</span></span>

# <a name="oss-authentication-providers"></a><span data-ttu-id="70b3d-106">OS 驗證提供者</span><span class="sxs-lookup"><span data-stu-id="70b3d-106">OSS authentication providers</span></span>

<span data-ttu-id="70b3d-107">下列清單是依字母順序排序。</span><span class="sxs-lookup"><span data-stu-id="70b3d-107">The list below is sorted alphabetically.</span></span>

| <span data-ttu-id="70b3d-108">名稱</span><span class="sxs-lookup"><span data-stu-id="70b3d-108">Name</span></span> | <span data-ttu-id="70b3d-109">描述</span><span class="sxs-lookup"><span data-stu-id="70b3d-109">Description</span></span> |
| ---- | ----------- |
| [<span data-ttu-id="70b3d-110">AspNet.Security.OpenIdConnect.Server (ASOS)</span><span class="sxs-lookup"><span data-stu-id="70b3d-110">AspNet.Security.OpenIdConnect.Server (ASOS)</span></span>](https://github.com/aspnet-contrib/AspNet.Security.OpenIdConnect.Server) | <span data-ttu-id="70b3d-111">ASOS 是低階、 通訊協定第一個 OpenID Connect 伺服器架構，ASP.NET Core 與 OWIN/Katana。</span><span class="sxs-lookup"><span data-stu-id="70b3d-111">ASOS is a low-level, protocol-first OpenID Connect server framework for ASP.NET Core and OWIN/Katana.</span></span> |
| [<span data-ttu-id="70b3d-112">Cierge</span><span class="sxs-lookup"><span data-stu-id="70b3d-112">Cierge</span></span>](https://github.com/pwdless/Cierge) | <span data-ttu-id="70b3d-113">Cierge 是 OpenID Connect 的伺服器才能處理使用者註冊、 登入、 設定檔、 管理和社交登入。</span><span class="sxs-lookup"><span data-stu-id="70b3d-113">Cierge is an OpenID Connect server that handles user signup, login, profiles, management, and social logins.</span></span> |
| [<span data-ttu-id="70b3d-114">Gluu 伺服器</span><span class="sxs-lookup"><span data-stu-id="70b3d-114">Gluu Server</span></span>](https://gluu.org/) | <span data-ttu-id="70b3d-115">企業就緒時，請開啟原始碼軟體的身分識別存取管理 (IAM)，和單一登入 (SSO)。</span><span class="sxs-lookup"><span data-stu-id="70b3d-115">Enterprise ready, open source software for identity, access management (IAM), and single sign-on (SSO).</span></span> <span data-ttu-id="70b3d-116">如需詳細資訊，請參閱[Gluu 產品文件](https://gluu.org/docs/)。</span><span class="sxs-lookup"><span data-stu-id="70b3d-116">For more information, see the [Gluu Product Documentation](https://gluu.org/docs/).</span></span> |
| [<span data-ttu-id="70b3d-117">IdentityServer</span><span class="sxs-lookup"><span data-stu-id="70b3d-117">IdentityServer</span></span>](https://identityserver.io/) | <span data-ttu-id="70b3d-118">IdentityServer 是 ASP.NET Core，正式認證由 OpenID Foundation 和.NET Foundation 的控管下 OpenID Connect 和 OAuth 2.0 架構。</span><span class="sxs-lookup"><span data-stu-id="70b3d-118">IdentityServer is an OpenID Connect and OAuth 2.0 framework for ASP.NET Core, officially certified by the OpenID Foundation and under governance of the .NET Foundation.</span></span> <span data-ttu-id="70b3d-119">如需詳細資訊，請參閱[歡迎 IdentityServer4 （文件）](https://identityserver4.readthedocs.io/en/release/)。</span><span class="sxs-lookup"><span data-stu-id="70b3d-119">For more information, see [Welcome to IdentityServer4 (Documentation)](https://identityserver4.readthedocs.io/en/release/).</span></span> |
| [<span data-ttu-id="70b3d-120">OpenIddict</span><span class="sxs-lookup"><span data-stu-id="70b3d-120">OpenIddict</span></span>](https://github.com/openiddict/openiddict-core) | <span data-ttu-id="70b3d-121">OpenIddict 是 ASP.NET Core 方便使用 OpenID Connect 伺服器。</span><span class="sxs-lookup"><span data-stu-id="70b3d-121">OpenIddict is an easy-to-use OpenID Connect server for ASP.NET Core.</span></span> |

<span data-ttu-id="70b3d-122">若要新增提供者，[編輯這個頁面](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Faspnet%2FDocs%2Fedit%2Fmaster%2Faspnetcore%2Fsecurity%2Fauthentication%2Fcommunity.md)。</span><span class="sxs-lookup"><span data-stu-id="70b3d-122">To add a provider, [edit this page](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Faspnet%2FDocs%2Fedit%2Fmaster%2Faspnetcore%2Fsecurity%2Fauthentication%2Fcommunity.md).</span></span>
