---
title: 後續步驟 - 使用ASP.NET核心和 Azure 進行 DevOps
author: CamSoper
description: 具有ASP.NET核心和 Azure 的 DevOps 的其他學習路徑。
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: a775dc42551a43bcce72b5f9ca364ed00b1dc4e6
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78659471"
---
# <a name="next-steps"></a><span data-ttu-id="e5af3-103">後續步驟</span><span class="sxs-lookup"><span data-stu-id="e5af3-103">Next steps</span></span>

<span data-ttu-id="e5af3-104">在本指南中,您為 ASP.NET 核心範例應用創建了 DevOps 管道。</span><span class="sxs-lookup"><span data-stu-id="e5af3-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="e5af3-105">恭喜！</span><span class="sxs-lookup"><span data-stu-id="e5af3-105">Congratulations!</span></span> <span data-ttu-id="e5af3-106">我們希望您喜歡學習將ASP.NET核心 Web 應用發佈到 Azure 應用服務,並自動進行更改的持續集成。</span><span class="sxs-lookup"><span data-stu-id="e5af3-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="e5af3-107">除了 Web 託管和 DevOps 之外,Azure 還具有一系列適用於 ASP.NET 核心開發人員的平臺即服務 (PaaS) 服務。</span><span class="sxs-lookup"><span data-stu-id="e5af3-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="e5af3-108">本節簡要概述了一些最常用的服務。</span><span class="sxs-lookup"><span data-stu-id="e5af3-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="e5af3-109">儲存和資料庫</span><span class="sxs-lookup"><span data-stu-id="e5af3-109">Storage and databases</span></span>

<span data-ttu-id="e5af3-110">[Redis Cache](/azure/redis-cache/)是高吞吐量、低延遲數據緩存,可作為服務提供。</span><span class="sxs-lookup"><span data-stu-id="e5af3-110">[Redis Cache](/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="e5af3-111">它可用於快取頁面輸出、減少資料庫請求以及跨應用的多個實例提供ASP.NET核心工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="e5af3-111">It can be used for caching page output, reducing database requests, and providing ASP.NET Core session state across multiple instances of an app.</span></span>

<span data-ttu-id="e5af3-112">[Azure 儲存](/azure/storage/)是 Azure 可大規模擴展的雲端儲存。</span><span class="sxs-lookup"><span data-stu-id="e5af3-112">[Azure Storage](/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="e5af3-113">開發人員可以利用[佇列儲存](/azure/storage/queues/storage-queues-introduction)進行可靠的消息排隊,而[表存儲](/azure/storage/tables/table-storage-overview)是一個 NoSQL 密鑰值存儲,專為使用大量半結構化數據集進行快速開發而設計。</span><span class="sxs-lookup"><span data-stu-id="e5af3-113">Developers can take advantage of [Queue Storage](/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="e5af3-114">[Azure SQL 資料庫](/azure/sql-database/)使用Microsoft SQL Server引擎作為服務提供熟悉的關係資料庫功能。</span><span class="sxs-lookup"><span data-stu-id="e5af3-114">[Azure SQL Database](/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="e5af3-115">[Cosmos DB](/azure/cosmos-db/)全域分佈的多模型 NoSQL 資料庫服務。</span><span class="sxs-lookup"><span data-stu-id="e5af3-115">[Cosmos DB](/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="e5af3-116">有多個 API 可用,包括 SQL API(以前稱為文檔 DB)、Cassandra 和 MongoDB。</span><span class="sxs-lookup"><span data-stu-id="e5af3-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## <a name="identity"></a><span data-ttu-id="e5af3-117">身分識別</span><span class="sxs-lookup"><span data-stu-id="e5af3-117">Identity</span></span>

<span data-ttu-id="e5af3-118">[Azure 活動目錄](/azure/active-directory/)和[Azure 活動目錄 B2C](/azure/active-directory-b2c/)都是識別服務。</span><span class="sxs-lookup"><span data-stu-id="e5af3-118">[Azure Active Directory](/azure/active-directory/) and [Azure Active Directory B2C](/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="e5af3-119">Azure 活動目錄專為企業方案而設計,支援 Azure AD B2B(企業對企業)協作,而 Azure 活動目錄 B2C 則旨在實現企業對企業方案,包括社交網路登錄。</span><span class="sxs-lookup"><span data-stu-id="e5af3-119">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="e5af3-120">行動訊息</span><span class="sxs-lookup"><span data-stu-id="e5af3-120">Mobile</span></span>

<span data-ttu-id="e5af3-121">[通知中心](/azure/notification-hubs/)是一個多平臺、可擴展的推送通知引擎,用於向在各種類型的設備上運行的應用快速發送數百萬條消息。</span><span class="sxs-lookup"><span data-stu-id="e5af3-121">[Notification Hubs](/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="e5af3-122">Web 結構</span><span class="sxs-lookup"><span data-stu-id="e5af3-122">Web infrastructure</span></span>

<span data-ttu-id="e5af3-123">[Azure 容器服務](/azure/aks/)管理託管 Kubernetes 環境,無需容器編排專業知識即可快速輕鬆地部署和管理容器化應用。</span><span class="sxs-lookup"><span data-stu-id="e5af3-123">[Azure Container Service](/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="e5af3-124">[Azure 搜索](/azure/search/)用於在私有的異質內容上創建企業搜索解決方案。</span><span class="sxs-lookup"><span data-stu-id="e5af3-124">[Azure Search](/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="e5af3-125">[Service Fabric](/azure/service-fabric/)是一個分散式系統平臺,可輕鬆打包、部署和管理可擴展且可靠的微服務和容器。</span><span class="sxs-lookup"><span data-stu-id="e5af3-125">[Service Fabric](/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>
