---
title: ASP.NET Core 與 Azure 的 DevOps
author: CamSoper
description: 本指南為如何為 Azure 上裝載的 ASP.NET Core 應用程式，建置 DevOps 管線的完整指導。
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722529"
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="51127-103">ASP.NET Core 與 Azure 的 DevOps</span><span class="sxs-lookup"><span data-stu-id="51127-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="51127-104">歡迎使用適用於.NET 的 Azure 開發生命週期指南！</span><span class="sxs-lookup"><span data-stu-id="51127-104">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="51127-105">本指南介紹如何使用.NET 工具及程序，為 Azure 建置開發生命週期的基本概念。</span><span class="sxs-lookup"><span data-stu-id="51127-105">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="51127-106">完成本指南之後，您就能充分運用完善 DevOps 工具鏈的各項優點。</span><span class="sxs-lookup"><span data-stu-id="51127-106">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="51127-107">本指南的適用對象</span><span class="sxs-lookup"><span data-stu-id="51127-107">Who this guide is for</span></span>

<span data-ttu-id="51127-108">您應是有經驗的 ASP.NET 開發人員 (200-300 級)。</span><span class="sxs-lookup"><span data-stu-id="51127-108">You should be an experienced ASP.NET developer (200-300 level).</span></span> <span data-ttu-id="51127-109">因為本指南涵蓋 Azure 介紹，所以您無須具備這方面的知識。</span><span class="sxs-lookup"><span data-stu-id="51127-109">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="51127-110">本指南也適用於工作內容偏重於操作，而不在開發的 DevOps 工程師。</span><span class="sxs-lookup"><span data-stu-id="51127-110">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="51127-111">本指南的對象是 Windows 開發人員，</span><span class="sxs-lookup"><span data-stu-id="51127-111">This guide targets Windows developers.</span></span> <span data-ttu-id="51127-112">但 .NET Core 也 100% 支援 Linux 與 macOS。</span><span class="sxs-lookup"><span data-stu-id="51127-112">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="51127-113">若要將本指南應用到 Linux/macOS，請留意圖說文字所示的 Linux/macOS 差異。</span><span class="sxs-lookup"><span data-stu-id="51127-113">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="51127-114">本指南未說明的內容</span><span class="sxs-lookup"><span data-stu-id="51127-114">What this guide doesn't cover</span></span>

<span data-ttu-id="51127-115">本指南適用於.NET 開發人員，著重於端對端的持續部署體驗，</span><span class="sxs-lookup"><span data-stu-id="51127-115">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="51127-116">而不會詳盡解說 Azure 的一切，也不會在 Azure 服務適用的 .NET API 上著墨太多。</span><span class="sxs-lookup"><span data-stu-id="51127-116">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="51127-117">其會將重點圍繞在持續整合、部署、監視及偵錯上。</span><span class="sxs-lookup"><span data-stu-id="51127-117">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="51127-118">在快速入門的最後，則會提供後續步驟的建議。</span><span class="sxs-lookup"><span data-stu-id="51127-118">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="51127-119">建議中會包含 ASP.NET 開發人員使用的 Azure 平台服務。</span><span class="sxs-lookup"><span data-stu-id="51127-119">Included in the suggestions are Azure platform services that are useful to ASP.NET developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="51127-120">本指南內容</span><span class="sxs-lookup"><span data-stu-id="51127-120">What's in this guide</span></span>

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[<span data-ttu-id="51127-121">工具及下載</span><span class="sxs-lookup"><span data-stu-id="51127-121">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="51127-122">了解如何取得本指南中使用的工具。</span><span class="sxs-lookup"><span data-stu-id="51127-122">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[<span data-ttu-id="51127-123">部署到 App Service</span><span class="sxs-lookup"><span data-stu-id="51127-123">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="51127-124">了解各種如何將 ASP.NET Core 應用程式部署到 Azure App Service 的方法。</span><span class="sxs-lookup"><span data-stu-id="51127-124">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[<span data-ttu-id="51127-125">持續整合與部署</span><span class="sxs-lookup"><span data-stu-id="51127-125">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="51127-126">使用 GitHub、 VSTS 及 Azure，為您的 ASP.NET Core 應用程式建置端對端的持續整合與部署解決方案。</span><span class="sxs-lookup"><span data-stu-id="51127-126">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, VSTS, and Azure.</span></span>

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[<span data-ttu-id="51127-127">監視及偵錯</span><span class="sxs-lookup"><span data-stu-id="51127-127">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="51127-128">使用 Azure 工來監視、疑難排解問題，以及微調您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="51127-128">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[<span data-ttu-id="51127-129">後續步驟</span><span class="sxs-lookup"><span data-stu-id="51127-129">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="51127-130">ASP.NET Core 開發人員學習 Azure 的其他學習途徑。</span><span class="sxs-lookup"><span data-stu-id="51127-130">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="51127-131">感謝</span><span class="sxs-lookup"><span data-stu-id="51127-131">Acknowledgments</span></span>

<span data-ttu-id="51127-132">感謝 .NET 社群中，對本指南提出實用建議的所有人！</span><span class="sxs-lookup"><span data-stu-id="51127-132">Thank you to everyone in the .NET community who contributed to this guide with helpful suggestions!</span></span> <span data-ttu-id="51127-133">我們特別要感謝下列社群成員，擔任這份文件最終審核的工作：</span><span class="sxs-lookup"><span data-stu-id="51127-133">We'd like to specifically thank the following community members who contributed to the final review of this material:</span></span>

* [<span data-ttu-id="51127-134">Sam Wronski</span><span class="sxs-lookup"><span data-stu-id="51127-134">Sam Wronski</span></span>](https://www.youtube.com/c/worldofzerodevelopment)
* [<span data-ttu-id="51127-135">Jeffrey Palermo</span><span class="sxs-lookup"><span data-stu-id="51127-135">Jeffrey Palermo</span></span>](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a><span data-ttu-id="51127-136">結論</span><span class="sxs-lookup"><span data-stu-id="51127-136">Conclusion</span></span>

<span data-ttu-id="51127-137">本指南為您在 ASP.NET Core 及 Azure App Service 上建置持續整合開發生命週期茵定基礎。</span><span class="sxs-lookup"><span data-stu-id="51127-137">This guide prepares you to build a continuous integration development lifecycle built around ASP.NET Core and Azure App Service.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="51127-138">其他閱讀資料</span><span class="sxs-lookup"><span data-stu-id="51127-138">Additional reading</span></span>

* [<span data-ttu-id="51127-139">什麼是雲端運算？</span><span class="sxs-lookup"><span data-stu-id="51127-139">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="51127-140">雲端運算的範例</span><span class="sxs-lookup"><span data-stu-id="51127-140">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="51127-141">什麼是 IaaS？</span><span class="sxs-lookup"><span data-stu-id="51127-141">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="51127-142">什麼是 PaaS？</span><span class="sxs-lookup"><span data-stu-id="51127-142">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)
