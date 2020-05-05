---
title: 後續步驟-使用 ASP.NET Core 和 Azure DevOps
author: CamSoper
description: 使用 ASP.NET Core 和 Azure 進行 DevOps 的其他學習路徑。
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: azure/devops/next-steps
ms.openlocfilehash: 92401d45d36dd3b93d175e08a8fa8697217ca7c7
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82766520"
---
# <a name="next-steps"></a><span data-ttu-id="26b5b-103">後續步驟</span><span class="sxs-lookup"><span data-stu-id="26b5b-103">Next steps</span></span>

<span data-ttu-id="26b5b-104">在本指南中，您已建立 ASP.NET Core 範例應用程式的 DevOps 管線。</span><span class="sxs-lookup"><span data-stu-id="26b5b-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="26b5b-105">恭喜！</span><span class="sxs-lookup"><span data-stu-id="26b5b-105">Congratulations!</span></span> <span data-ttu-id="26b5b-106">我們希望您喜歡學習如何發佈 ASP.NET Core web 應用程式來 Azure App Service，並將變更的持續整合自動化。</span><span class="sxs-lookup"><span data-stu-id="26b5b-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="26b5b-107">除了 web 裝載和 DevOps 之外，Azure 還提供各式各樣的平臺即服務（PaaS）服務，適用于 ASP.NET Core 開發人員。</span><span class="sxs-lookup"><span data-stu-id="26b5b-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="26b5b-108">本節提供一些最常用服務的簡要概述。</span><span class="sxs-lookup"><span data-stu-id="26b5b-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="26b5b-109">儲存體和資料庫</span><span class="sxs-lookup"><span data-stu-id="26b5b-109">Storage and databases</span></span>

<span data-ttu-id="26b5b-110">[Redis Cache](/azure/redis-cache/)是以服務形式提供的高輸送量、低延遲的資料快取。</span><span class="sxs-lookup"><span data-stu-id="26b5b-110">[Redis Cache](/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="26b5b-111">它可以用來快取頁面輸出、減少資料庫要求，以及在多個應用程式實例之間提供 ASP.NET Core 會話狀態。</span><span class="sxs-lookup"><span data-stu-id="26b5b-111">It can be used for caching page output, reducing database requests, and providing ASP.NET Core session state across multiple instances of an app.</span></span>

<span data-ttu-id="26b5b-112">[Azure 儲存體](/azure/storage/)是 Azure 可大規模調整的雲端儲存體。</span><span class="sxs-lookup"><span data-stu-id="26b5b-112">[Azure Storage](/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="26b5b-113">開發人員可以利用[佇列儲存體](/azure/storage/queues/storage-queues-introduction)來提供可靠的訊息佇列，而[表格儲存體](/azure/storage/tables/table-storage-overview)則是 NoSQL 索引鍵值存放區，其設計目的是要使用大量的半結構化資料集進行快速開發。</span><span class="sxs-lookup"><span data-stu-id="26b5b-113">Developers can take advantage of [Queue Storage](/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="26b5b-114">[Azure SQL Database](/azure/sql-database/)使用 Microsoft SQL Server 引擎，提供熟悉的關係資料庫功能做為服務。</span><span class="sxs-lookup"><span data-stu-id="26b5b-114">[Azure SQL Database](/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="26b5b-115">[Cosmos DB](/azure/cosmos-db/)全域散發的多模型 NoSQL 資料庫服務。</span><span class="sxs-lookup"><span data-stu-id="26b5b-115">[Cosmos DB](/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="26b5b-116">有多個 Api 可供使用，包括 SQL API （先前稱為 DocumentDB）、Cassandra 和 MongoDB。</span><span class="sxs-lookup"><span data-stu-id="26b5b-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## Identity

<span data-ttu-id="26b5b-117">[Azure Active Directory](/azure/active-directory/)和[Azure Active Directory B2C](/azure/active-directory-b2c/)都是身分識別服務。</span><span class="sxs-lookup"><span data-stu-id="26b5b-117">[Azure Active Directory](/azure/active-directory/) and [Azure Active Directory B2C](/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="26b5b-118">Azure Active Directory 是針對企業案例而設計，可讓 Azure AD B2B （企業對企業）共同作業，而 Azure Active Directory B2C 則是預期的企業對客戶案例，包括社交網路登入。</span><span class="sxs-lookup"><span data-stu-id="26b5b-118">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="26b5b-119">行動訊息</span><span class="sxs-lookup"><span data-stu-id="26b5b-119">Mobile</span></span>

<span data-ttu-id="26b5b-120">[通知中樞](/azure/notification-hubs/)是多平臺、可擴充的推播通知引擎，可快速地將數百萬個訊息傳送到在各種類型的裝置上執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="26b5b-120">[Notification Hubs](/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="26b5b-121">Web 基礎結構</span><span class="sxs-lookup"><span data-stu-id="26b5b-121">Web infrastructure</span></span>

<span data-ttu-id="26b5b-122">[Azure Container Service](/azure/aks/)會管理您託管的 Kubernetes 環境，讓您快速且輕鬆地部署和管理容器化應用程式，而不需要容器協調流程專業知識。</span><span class="sxs-lookup"><span data-stu-id="26b5b-122">[Azure Container Service](/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="26b5b-123">[Azure 搜尋服務](/azure/search/)是用來透過私人的非公開內容來建立企業搜尋解決方案。</span><span class="sxs-lookup"><span data-stu-id="26b5b-123">[Azure Search](/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="26b5b-124">[Service Fabric](/azure/service-fabric/)是分散式系統平臺，可讓您輕鬆封裝、部署及管理可調整和可靠的微服務和容器。</span><span class="sxs-lookup"><span data-stu-id="26b5b-124">[Service Fabric](/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>
