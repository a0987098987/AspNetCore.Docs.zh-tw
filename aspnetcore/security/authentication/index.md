---
title: "ASP.NET Core 中的驗證"
author: rick-anderson
description: "探索有關 ASP.NET 驗證技術的主題。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/index
ms.openlocfilehash: 20a6d5ae598a0d1e8d7735cb1311fac1c10513eb
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
# <a name="authentication-in-aspnet-core"></a><span data-ttu-id="82c4b-103">ASP.NET Core 中的驗證</span><span class="sxs-lookup"><span data-stu-id="82c4b-103">Authentication in ASP.NET Core</span></span>

* [<span data-ttu-id="82c4b-104">社群 OSS 驗證選項</span><span class="sxs-lookup"><span data-stu-id="82c4b-104">Community OSS authentication options</span></span>](xref:security/authentication/community)

* [<span data-ttu-id="82c4b-105">身分識別簡介</span><span class="sxs-lookup"><span data-stu-id="82c4b-105">Introduction to Identity</span></span>](xref:security/authentication/identity)

* [<span data-ttu-id="82c4b-106">使用 Facebook、Google 和其他外部提供者啟用驗證</span><span class="sxs-lookup"><span data-stu-id="82c4b-106">Enable authentication using Facebook, Google, and other external providers</span></span>](xref:security/authentication/social/index)

* [<span data-ttu-id="82c4b-107">以 WS 同盟啟用驗證</span><span class="sxs-lookup"><span data-stu-id="82c4b-107">Enable authentication with WS-Federation</span></span>](xref:security/authentication/ws-federation)

* [<span data-ttu-id="82c4b-108">在身分識別中啟用 QR 程式碼產生</span><span class="sxs-lookup"><span data-stu-id="82c4b-108">Enable QR code generation in Identity</span></span>](xref:security/authentication/identity-enable-qrcodes)

* [<span data-ttu-id="82c4b-109">使用 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="82c4b-109">Configure Windows Authentication</span></span>](xref:security/authentication/windowsauth)

* [<span data-ttu-id="82c4b-110">帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="82c4b-110">Account confirmation and password recovery</span></span>](xref:security/authentication/accconfirm)

* [<span data-ttu-id="82c4b-111">使用 SMS 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="82c4b-111">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)

* [<span data-ttu-id="82c4b-112">使用沒有身分識別的 Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="82c4b-112">Use cookie authentication without Identity</span></span>](xref:security/authentication/cookie)

* [<span data-ttu-id="82c4b-113">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="82c4b-113">Azure Active Directory</span></span>](xref:security/authentication/azure-active-directory/index)

  * <span data-ttu-id="82c4b-114">[Integrate Azure AD Into an ASP.NET Core web app](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/) (將 Azure AD 整合到 ASP.NET Core Web 應用程式)</span><span class="sxs-lookup"><span data-stu-id="82c4b-114">[Integrate Azure AD into an ASP.NET Core web app](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)</span></span>

  * [<span data-ttu-id="82c4b-115">將 Azure AD B2C 整合到與使用者互動的 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="82c4b-115">Integrate Azure AD B2C into a customer-facing ASP.NET Core web app</span></span>](xref:security/authentication/azure-ad-b2c)

  * [<span data-ttu-id="82c4b-116">將 Azure AD B2C 整合到 ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="82c4b-116">Integrate Azure AD B2C into an ASP.NET Core web API</span></span>](xref:security/authentication/azure-ad-b2c-webapi)

  * <span data-ttu-id="82c4b-117">[Call a ASP.NET Core Web API from a WPF app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/) (使用 Azure AD 從 WPF 應用程式呼叫 ASP.NET Core Web API)</span><span class="sxs-lookup"><span data-stu-id="82c4b-117">[Call an ASP.NET Core Web API from a WPF app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)</span></span>

  * <span data-ttu-id="82c4b-118">[Call a Web API in an ASP.NET Core web app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/) (使用 Azure AD 在 ASP.NET Core Web 應用程式中呼叫 Web API)</span><span class="sxs-lookup"><span data-stu-id="82c4b-118">[Call a Web API in an ASP.NET Core web app using Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)</span></span>

* [<span data-ttu-id="82c4b-119">使用 IdentityServer4 保護 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="82c4b-119">Secure ASP.NET Core apps with IdentityServer4</span></span>](http://docs.identityserver.io/en/release/)

* [<span data-ttu-id="82c4b-120">使用 Azure App Service 驗證 (簡單驗證) 保護 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="82c4b-120">Secure ASP.NET Core apps with Azure App Service Authentication (Easy Auth)</span></span>](/azure/app-service/app-service-authentication-overview)

* [<span data-ttu-id="82c4b-121">關於以個別使用者帳戶建立之專案的文章</span><span class="sxs-lookup"><span data-stu-id="82c4b-121">Articles based on projects created with individual user accounts</span></span>](xref:security/authentication/individual)
